---
title: Gegevens model leren en partitioneren op Azure Cosmos DB met behulp van een echt voor beeld
description: Meer informatie over het model leren en partitioneren van een echt voor beeld met behulp van de Azure Cosmos DB core-API
author: ThomasWeiss
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 05/23/2019
ms.author: thweiss
ms.openlocfilehash: 55290b88fedabe59417ea49f1cd3c3bc9961678d
ms.sourcegitcommit: 44e85b95baf7dfb9e92fb38f03c2a1bc31765415
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/28/2019
ms.locfileid: "70093417"
---
# <a name="how-to-model-and-partition-data-on-azure-cosmos-db-using-a-real-world-example"></a>Gegevens model leren en partitioneren op Azure Cosmos DB met behulp van een echt voor beeld

In dit artikel worden verschillende Azure Cosmos DB concepten, zoals [gegevens modellering](modeling-data.md), [partitioneren](partitioning-overview.md)en [ingerichte door Voer](request-units.md) , gebouwd om te laten zien hoe u een praktijk oefening voor het ontwerpen van gegevens kunt beleven.

Als u meestal met relationele data bases werkt, hebt u waarschijnlijk gewoonten en intuitions gemaakt over het ontwerpen van een gegevens model. Vanwege de specifieke beperkingen, maar ook de unieke kracht van Azure Cosmos DB, worden de meeste van deze best practices niet goed vertaald en kunnen ze naar eigen inzicht worden gesleept. Het doel van dit artikel is om u door het proces van het model leren van een Real-World use-case op Azure Cosmos DB, van artikel modellering tot entiteits-en container partities.

## <a name="the-scenario"></a>Het scenario

Voor deze oefening gaan we het domein van een blog platform overwegen waar *gebruikers* *berichten*kunnen maken. Gebruikers kunnen ook *notities* toevoegen aan deze berichten.

> [!TIP]
> Er zijn enkele woorden *cursief*gemarkeerd. deze woorden identificeren het soort ' dingen ' die ons model moet manipuleren.

Meer vereisten aan onze specificatie toevoegen:

- Op een front page wordt een feed van onlangs gemaakte Posts weer gegeven
- We kunnen alle berichten voor een gebruiker ophalen, alle opmerkingen voor een post en alles voor een post,
- Berichten worden geretourneerd met de gebruikers naam van hun auteurs en een telling van het aantal opmerkingen en leuk vinden deze.
- Opmerkingen en dergelijke worden ook geretourneerd met de gebruikers naam van de gebruikers die ze hebben gemaakt,
- Als lijsten worden weer gegeven, hoeft u alleen maar een afgekapte samen vatting van hun inhoud te presen teren.

## <a name="identify-the-main-access-patterns"></a>De belangrijkste toegangs patronen identificeren

We geven een deel van onze oorspronkelijke specificatie door de toegangs patronen van onze oplossing te identificeren. Bij het ontwerpen van een gegevens model voor Azure Cosmos DB, is het belang rijk om te begrijpen welke aanvragen ons model moet gebruiken om ervoor te zorgen dat het model deze aanvragen efficiënt zal verwerken.

Om het algehele proces gemakkelijker te kunnen volgen, categoriseren we die verschillende aanvragen als opdrachten of query's, waarbij een woorden lijst wordt uitgeleend van [CQRS](https://en.wikipedia.org/wiki/Command%E2%80%93query_separation#Command_query_responsibility_segregation) waarbij opdrachten schrijf aanvragen zijn (dat wil zeggen, intenties om het systeem bij te werken) en de query's alleen-lezen zijn aanvragen.

Hier volgt een lijst met aanvragen die ons platform moet openbaren:

- **[C1]** Een gebruiker maken/bewerken
- **[KW]** Een gebruiker ophalen
- **[C2]** Een bericht maken/bewerken
- **[Q2]** Een bericht ophalen
- **[Q3]** De berichten van een gebruiker in een korte vorm weer geven
- **[C3]** Een opmerking maken
- **[Q4]** Opmerkingen bij een bericht weer geven
- **[C4]** Als een post
- **[Q5]** Een leuk bericht weer geven
- **[Q6]** De *x* meest recente Posts weer geven die zijn gemaakt in korte vorm (feed)

In deze fase hebben we geen aandacht meer bedacht op de details van wat elke entiteit (gebruiker, post enz.) zal bevatten. Deze stap is doorgaans de eerste die u moet uitvoeren bij het ontwerpen op basis van een relationele Store, omdat u moet nagaan hoe deze entiteiten worden vertaald in termen van tabellen, kolommen, refererende sleutels, enzovoort. Het is veel minder van een probleem met een document database die geen enkel schema afdwingt bij het schrijven.

De belangrijkste reden waarom het belang rijk is om de toegangs patronen te identificeren vanaf het begin, is omdat deze lijst met aanvragen wordt door ons test pakket. Elke keer dat we ons gegevens model herhalen, gaan we elk van de aanvragen door en controleren ze de prestaties en schaal baarheid.

## <a name="v1-a-first-version"></a>RIP Een eerste versie

We beginnen met twee containers: `users` en `posts`.

### <a name="users-container"></a>Container gebruikers

Met deze container worden alleen gebruikers items opgeslagen:

    {
      "id": "<user-id>",
      "username": "<username>"
    }

Deze container wordt gepartitioneerd door `id`. Dit betekent dat elke logische partitie in die container slechts één item bevat.

### <a name="posts-container"></a>Berichten container

Deze container fungeert als host voor berichten, opmerkingen en leuk:

    {
      "id": "<post-id>",
      "type": "post",
      "postId": "<post-id>",
      "userId": "<post-author-id>",
      "title": "<post-title>",
      "content": "<post-content>",
      "creationDate": "<post-creation-date>"
    }

    {
      "id": "<comment-id>",
      "type": "comment",
      "postId": "<post-id>",
      "userId": "<comment-author-id>",
      "content": "<comment-content>",
      "creationDate": "<comment-creation-date>"
    }

    {
      "id": "<like-id>",
      "type": "like",
      "postId": "<post-id>",
      "userId": "<liker-id>",
      "creationDate": "<like-creation-date>"
    }

Deze container wordt gepartitioneerd door `postId`. Dit betekent dat elke logische partitie in die container één post bevat, alle opmerkingen voor dat bericht en alle voor die post.

Houd er rekening mee dat we `type` een eigenschap hebben geïntroduceerd in de items die in deze container zijn opgeslagen om onderscheid te maken tussen de drie typen entiteiten die door deze container worden gehost.

Daarnaast hebben we gekozen om te verwijzen naar gerelateerde gegevens in plaats van deze in te sluiten (Raadpleeg [deze sectie](modeling-data.md) voor meer informatie over deze concepten). de reden hiervoor is:

- Er is geen bovengrens voor het aantal berichten dat een gebruiker kan maken,
- berichten kunnen wille keurig lang zijn,
- Er is geen bovengrens voor het aantal opmerkingen dat een bericht kan hebben,
- We willen een opmerking kunnen toevoegen of een soort bericht, zonder dat u het bericht hoeft bij te werken.

## <a name="how-well-does-our-model-perform"></a>Hoe goed presteert het model?

Het is nu tijd om de prestaties en schaal baarheid van onze eerste versie te beoordelen. Voor elk van de eerder geïdentificeerde aanvragen meten we de latentie en het aantal aanvraag eenheden dat wordt verbruikt. Deze meting geschiedt op basis van een dummy gegevensverzameling met 100.000 gebruikers met 5 tot 50 berichten per gebruiker, en Maxi maal 25 reacties en 100 leuk per post.

### <a name="c1-createedit-a-user"></a>C1 Een gebruiker maken/bewerken

Deze aanvraag is eenvoudig te implementeren omdat er alleen een item in de `users` container wordt gemaakt of bijgewerkt. De aanvragen worden goed verspreid over alle partities dankzij de `id` partitie sleutel.

![Eén item naar de container gebruikers schrijven](./media/how-to-model-partition-example/V1-C1.png)

| **Latentie** | **RU-kosten** | **Prestaties** |
| --- | --- | --- |
| 7 MS | 5,71 RU | ✅ |

### <a name="q1-retrieve-a-user"></a>Eerste Een gebruiker ophalen

Het ophalen van een gebruiker wordt uitgevoerd door het bijbehorende item uit de `users` container te lezen.

![Eén item ophalen uit de container gebruikers](./media/how-to-model-partition-example/V1-Q1.png)

| **Latentie** | **RU-kosten** | **Prestaties** |
| --- | --- | --- |
| 2 MS | 1 RU | ✅ |

### <a name="c2-createedit-a-post"></a>Vind Een bericht maken/bewerken

Net als bij **[C1]** hoeven we alleen maar naar de `posts` container te schrijven.

![Een enkel item schrijven naar de container Posts](./media/how-to-model-partition-example/V1-C2.png)

| **Latentie** | **RU-kosten** | **Prestaties** |
| --- | --- | --- |
| 9 MS | 8,76 RU | ✅ |

### <a name="q2-retrieve-a-post"></a>Inclusief Een bericht ophalen

We beginnen met het ophalen van het bijbehorende document uit `posts` de container. Maar dat is niet voldoende, conform onze specificatie, moeten we de gebruikers naam van de auteur van het bericht en het aantal opmerkingen en het aantal reacties en de aantallen van de informatie in dit bericht, opgeven, waarvoor drie extra SQL-query's moeten worden uitgegeven.

![Een bericht ophalen en aanvullende gegevens samen voegen](./media/how-to-model-partition-example/V1-Q2.png)

Elk van de extra query's filtert op de partitie sleutel van de betreffende container. Dit is precies wat we nodig hebben om de prestaties en schaal baarheid te maximaliseren. Maar we moeten uiteindelijk vier bewerkingen uitvoeren om één post te retour neren, dus we verbeteren die in een volgende iteratie.

| **Latentie** | **RU-kosten** | **Prestaties** |
| --- | --- | --- |
| 9 MS | 19,54 RU | ⚠ |

### <a name="q3-list-a-users-posts-in-short-form"></a>K3 De berichten van een gebruiker in een korte vorm weer geven

Eerst moeten we de gewenste berichten ophalen met een SQL-query waarmee de berichten worden opgehaald die overeenkomen met die specifieke gebruiker. Maar we moeten ook extra query's geven om de gebruikers naam van de auteur samen te voegen en het aantal opmerkingen en de bevindt.

![Ophalen van alle Posts voor een gebruiker en het samen voegen van de aanvullende gegevens](./media/how-to-model-partition-example/V1-Q3.png)

Deze implementatie geeft veel nadelen:

- de query's voor het samen voegen van het aantal opmerkingen en van zichzelf moeten worden uitgegeven voor elk bericht dat door de eerste query wordt geretourneerd.
- de hoofd query filtert niet op de partitie sleutel van de `posts` container, waardoor er een uitwaaiering wordt uitgevoerd en een partitie scan in de container.

| **Latentie** | **RU-kosten** | **Prestaties** |
| --- | --- | --- |
| 130 MS | 619,41 RU | ⚠ |

### <a name="c3-create-a-comment"></a>Stand Een opmerking maken

Er wordt een opmerking gemaakt door het bijbehorende item in de `posts` container te schrijven.

![Een enkel item schrijven naar de container Posts](./media/how-to-model-partition-example/V1-C2.png)

| **Latentie** | **RU-kosten** | **Prestaties** |
| --- | --- | --- |
| 7 MS | 8,57 RU | ✅ |

### <a name="q4-list-a-posts-comments"></a>Taal Opmerkingen bij een bericht weer geven

We beginnen met een query waarmee alle opmerkingen voor die post worden opgehaald en opnieuw worden verzameld, moeten ook de gebruikers namen voor elke opmerking afzonderlijk worden geaggregeerd.

![Alle opmerkingen voor een bericht ophalen en de aanvullende gegevens samen voegen](./media/how-to-model-partition-example/V1-Q4.png)

Hoewel de hoofd query filtert op de partitie sleutel van de container, is het samen voegen van de gebruikers namen afzonderlijk van belang voor de algehele prestaties. Dit wordt later verbeterd.

| **Latentie** | **RU-kosten** | **Prestaties** |
| --- | --- | --- |
| 23 MS | 27,72 RU | ⚠ |

### <a name="c4-like-a-post"></a>C4 Als een post

Net als bij **[C3]** maken we het bijbehorende item in de `posts` container.

![Een enkel item schrijven naar de container Posts](./media/how-to-model-partition-example/V1-C2.png)

| **Latentie** | **RU-kosten** | **Prestaties** |
| --- | --- | --- |
| 6 MS | 7,05 RU | ✅ |

### <a name="q5-list-a-posts-likes"></a>[Q5] Een leuk bericht weer geven

Net als **[Q4]** zoeken we de zoek opdracht naar het bericht en voegen ze vervolgens hun gebruikers namen samen.

![Alles ophalen voor een post en het samen voegen van de aanvullende gegevens](./media/how-to-model-partition-example/V1-Q5.png)

| **Latentie** | **RU-kosten** | **Prestaties** |
| --- | --- | --- |
| 59 MS | 58,92 RU | ⚠ |

### <a name="q6-list-the-x-most-recent-posts-created-in-short-form-feed"></a>[Q6] De x meest recente Posts weer geven die zijn gemaakt in korte vorm (feed)

De meest recente berichten worden opgehaald door de `posts` container te doorzoeken op aflopende aanmaak datum, vervolgens de gebruikers namen en het aantal opmerkingen en de aantallen voor elk van de berichten te verzamelen.

![Ophalen van de meest recente posts en het samen voegen van de aanvullende gegevens](./media/how-to-model-partition-example/V1-Q6.png)

Daarna wordt de eerste query niet gefilterd op de partitie sleutel van `posts` de container, waardoor er een dure ventilator wordt geactiveerd. Dit is nog erger omdat we een veel grotere resultatenset richten en de resultaten sorteren met een `ORDER BY` component, waardoor het duurder is voor de aanvraag eenheden.

| **Latentie** | **RU-kosten** | **Prestaties** |
| --- | --- | --- |
| 306 MS | 2063,54 RU | ⚠ |

## <a name="reflecting-on-the-performance-of-v1"></a>Weer spie gelen met betrekking tot de prestaties van v1

Op basis van de prestatie problemen die in de vorige sectie zijn besproken, kunnen we twee belang rijke klassen van problemen identificeren:

- voor sommige aanvragen moeten meerdere query's worden uitgegeven om alle gegevens te verzamelen die we moeten retour neren.
- Sommige query's filteren niet op de partitie sleutel van de containers waarop ze zijn gericht, waardoor de schaal baarheid wordt belemmerd door een ventilator.

Laten we elk van deze problemen oplossen, beginnend met de eerste.

## <a name="v2-introducing-denormalization-to-optimize-read-queries"></a>V2: Inleiding tot het optimaliseren van Lees query's

De reden waarom we extra aanvragen in sommige gevallen moeten uitgeven, is omdat de resultaten van de eerste aanvraag niet alle gegevens bevatten die we moeten retour neren. Wanneer u werkt met een niet-relationeel gegevens archief, zoals Azure Cosmos DB, wordt dit soort probleem doorgaans opgelost door het denormaliseren van gegevens in de gegevensset.

In ons voor beeld wijzigen we post-items om de gebruikers naam van de auteur van het bericht toe te voegen, het aantal opmerkingen en het aantal keren:

    {
      "id": "<post-id>",
      "type": "post",
      "postId": "<post-id>",
      "userId": "<post-author-id>",
      "userUsername": "<post-author-username>",
      "title": "<post-title>",
      "content": "<post-content>",
      "commentCount": <count-of-comments>,
      "likeCount": <count-of-likes>,
      "creationDate": "<post-creation-date>"
    }

We wijzigen ook opmerkingen en soort gelijke items om de gebruikers naam toe te voegen van de gebruiker die ze heeft gemaakt:

    {
      "id": "<comment-id>",
      "type": "comment",
      "postId": "<post-id>",
      "userId": "<comment-author-id>",
      "userUsername": "<comment-author-username>",
      "content": "<comment-content>",
      "creationDate": "<comment-creation-date>"
    }

    {
      "id": "<like-id>",
      "type": "like",
      "postId": "<post-id>",
      "userId": "<liker-id>",
      "userUsername": "<liker-username>",
      "creationDate": "<like-creation-date>"
    }

### <a name="denormalizing-comment-and-like-counts"></a>Het denormaliseren van opmerkingen en soort gelijke tellingen

Wat we willen doen, is dat elke keer dat we een opmerking of als soort toevoegen, we ook de `commentCount` of de `likeCount` in de bijbehorende post verhogen. Als de `postId`container is gepartitioneerd door, wordt het nieuwe item (opmerking of leuk) en het bijbehorende bericht in dezelfde logische partitie geplaatst. `posts` Als gevolg hiervan kunnen we een [opgeslagen procedure](stored-procedures-triggers-udfs.md) gebruiken om die bewerking uit te voeren.

Wanneer u nu een opmerking maakt ( **[C3]** ) in plaats van een nieuw item toe te voegen aan de `posts` container, noemen we de volgende opgeslagen procedure in die container:

```javascript
function createComment(postId, comment) {
  var collection = getContext().getCollection();

  collection.readDocument(
    `${collection.getAltLink()}/docs/${postId}`,
    function (err, post) {
      if (err) throw err;

      post.commentCount++;
      collection.replaceDocument(
        post._self,
        post,
        function (err) {
          if (err) throw err;

          comment.postId = postId;
          collection.createDocument(
            collection.getSelfLink(),
            comment
          );
        }
      );
    })
}
```

In deze opgeslagen procedure worden de ID van het bericht en de hoofd tekst van de nieuwe opmerking als para meters gebruikt, waarna:

- Hiermee wordt het bericht opgehaald
- verhoogt de`commentCount`
- vervangt het bericht
- voegt de nieuwe opmerking toe

Als opgeslagen procedures worden uitgevoerd als atomische trans acties, wordt gegarandeerd dat de waarde `commentCount` van en het werkelijke aantal opmerkingen altijd synchroon blijft.

We gaan natuurlijk een soort gelijke opgeslagen procedure aanroepen bij het `likeCount`toevoegen van nieuwe leuks.

### <a name="denormalizing-usernames"></a>Gebruikers namen denormaliseren

Gebruikers namen moeten een andere benadering hebben wanneer gebruikers niet alleen in andere partities zitten, maar in een andere container. Wanneer we gegevens over partities en containers moeten denormaliseren, kunnen we de [feed voor wijzigingen](change-feed.md)van de bron container gebruiken.

In ons voor beeld gebruiken we de wijzigings feed van `users` de container om te reageren wanneer gebruikers hun gebruikers namen bijwerken. Als dat gebeurt, wordt de wijziging door gegeven door een andere opgeslagen procedure aan te `posts` roepen op de container:

![De gebruikers namen worden genormaliseerd in de container Posts](./media/how-to-model-partition-example/denormalization-1.png)

```javascript
function updateUsernames(userId, username) {
  var collection = getContext().getCollection();
  
  collection.queryDocuments(
    collection.getSelfLink(),
    `SELECT * FROM p WHERE p.userId = '${userId}'`,
    function (err, results) {
      if (err) throw err;

      for (var i in results) {
        var doc = results[i];
        doc.userUsername = username;

        collection.upsertDocument(
          collection.getSelfLink(),
          doc);
      }
    });
}
```

In deze opgeslagen procedure worden de ID van de gebruiker en de nieuwe gebruikers naam van de gebruiker gebruikt als para meters, vervolgens:

- Hiermee worden alle items opgehaald die `userId` overeenkomen met de (die berichten, opmerkingen of leuk kunnen zijn)
- voor elk van deze items
  - vervangt de`userUsername`
  - Hiermee wordt het item vervangen

> [!IMPORTANT]
> Deze bewerking is kostbaar omdat deze opgeslagen procedure moet worden uitgevoerd op elke partitie van de `posts` container. We gaan ervan uit dat de meeste gebruikers tijdens het aanmelden een geschikte gebruikers naam kiezen en niet ooit wijzigen, zodat deze update zeer zelden wordt uitgevoerd.

## <a name="what-are-the-performance-gains-of-v2"></a>Wat zijn de prestatie verbeteringen van v2?

### <a name="q2-retrieve-a-post"></a>Inclusief Een bericht ophalen

Nu onze normalisatie is ingesteld, hoeft u slechts één item op te halen om die aanvraag af te handelen.

![Eén item uit de container Posts ophalen](./media/how-to-model-partition-example/V2-Q2.png)

| **Latentie** | **RU-kosten** | **Prestaties** |
| --- | --- | --- |
| 2 MS | 1 RU | ✅ |

### <a name="q4-list-a-posts-comments"></a>Taal Opmerkingen bij een bericht weer geven

Nu opnieuw, kunnen we de extra aanvragen voor het ophalen van de gebruikers namen en eindigen met één query die op de partitie sleutel filtert.

![Alle opmerkingen voor een bericht ophalen](./media/how-to-model-partition-example/V2-Q4.png)

| **Latentie** | **RU-kosten** | **Prestaties** |
| --- | --- | --- |
| 4 MS | 7,72 RU | ✅ |

### <a name="q5-list-a-posts-likes"></a>[Q5] Een leuk bericht weer geven

Precies dezelfde situatie wanneer u het leuk vindt.

![Alle keren dat een bericht wordt opgehaald](./media/how-to-model-partition-example/V2-Q5.png)

| **Latentie** | **RU-kosten** | **Prestaties** |
| --- | --- | --- |
| 4 MS | 8,92 RU | ✅ |

## <a name="v3-making-sure-all-requests-are-scalable"></a>V3: Controleren of alle aanvragen schaalbaar zijn

Bekijk onze algemene prestatie verbeteringen, maar er zijn nog twee aanvragen die niet volledig zijn geoptimaliseerd: **[Q3]** en **[Q6]** . Dit zijn de aanvragen met betrekking tot query's die niet worden gefilterd op de partitie sleutel van de containers waarop ze zijn gericht.

### <a name="q3-list-a-users-posts-in-short-form"></a>K3 De berichten van een gebruiker in een korte vorm weer geven

Deze aanvraag is al voor delen van de verbeteringen die zijn geïntroduceerd in v2, waardoor extra query's worden gemaakt.

![Alle berichten voor een gebruiker ophalen](./media/how-to-model-partition-example/V2-Q3.png)

Maar de resterende query wordt nog steeds niet gefilterd op de partitie `posts` sleutel van de container.

De manier waarop u rekening moet houden met deze situatie is eigenlijk eenvoudig:

1. Deze aanvraag moet worden gefilterd `userId` op de omdat we alle Posts voor een bepaalde gebruiker willen ophalen
1. De app wordt niet goed uitgevoerd omdat deze wordt uitgevoerd `posts` op de container, die niet is gepartitioneerd door`userId`
1. Wat het duidelijkst is, wij zullen ons prestatie probleem oplossen door deze aanvraag uit te voeren op een container die *is* gepartitioneerd door`userId`
1. Het maakt niet uit of een dergelijke container al is: de `users` container!

We introduceren dus een tweede niveau van denormalisatie door volledige berichten naar de `users` container te dupliceren. Op die manier krijgen we een kopie van onze berichten, alleen gepartitioneerd langs een andere dimensie, waardoor ze efficiënter zijn `userId`om ze op te halen.

De `users` container bevat nu twee soorten items:

    {
      "id": "<user-id>",
      "type": "user",
      "userId": "<user-id>",
      "username": "<username>"
    }

    {
      "id": "<post-id>",
      "type": "post",
      "postId": "<post-id>",
      "userId": "<post-author-id>",
      "userUsername": "<post-author-username>",
      "title": "<post-title>",
      "content": "<post-content>",
      "commentCount": <count-of-comments>,
      "likeCount": <count-of-likes>,
      "creationDate": "<post-creation-date>"
    }

Houd rekening met het volgende:

- We hebben een `type` veld in het gebruikers item geïntroduceerd om gebruikers te onderscheiden van berichten,
- Er is `userId` ook een veld toegevoegd aan het gebruikers item, dat overbodig is met het `id` veld, maar is vereist omdat de `users` container nu is gepartitioneerd door `userId` ( `id` en niet als voorheen)

Om dat te doen, gebruiken we de wijzigings feed opnieuw. Deze keer reageren we op de wijzigings feed van de `posts` container om een nieuwe of bijgewerkte post naar de `users` container te verzenden. En omdat het weer geven van berichten geen volledige inhoud hoeft te retour neren, kunnen ze in het proces worden afgekapt.

![De berichten in de container gebruikers worden genormaliseerd](./media/how-to-model-partition-example/denormalization-2.png)

We kunnen onze query nu naar de `users` container routeren, filteren op de partitie sleutel van de container.

![Alle berichten voor een gebruiker ophalen](./media/how-to-model-partition-example/V3-Q3.png)

| **Latentie** | **RU-kosten** | **Prestaties** |
| --- | --- | --- |
| 4 MS | 6,46 RU | ✅ |

### <a name="q6-list-the-x-most-recent-posts-created-in-short-form-feed"></a>[Q6] De x meest recente Posts weer geven die zijn gemaakt in korte vorm (feed)

We moeten hier een vergelijk bare situatie behandelen: zelfs na het afwijzen van de extra query's die onnodig zijn gemaakt in v2, wordt de resterende query niet gefilterd op de partitie sleutel van de container:

![De meest recente posts worden opgehaald](./media/how-to-model-partition-example/V2-Q6.png)

Volgens dezelfde benadering moet de prestaties en schaal baarheid van deze aanvraag worden gemaximaliseerd, maar is er slechts één partitie. Dit kan worden bedacht omdat er slechts een beperkt aantal items moet worden geretourneerd. Als u de start pagina van ons blog platform wilt vullen, moeten we de 100 meest recente Posts ophalen, zonder dat u de hele gegevensset hoeft te pagineren.

Om deze laatste aanvraag te optimaliseren, introduceren we een derde container aan ons ontwerp, die volledig is toegewezen aan deze aanvraag. We kunnen onze berichten naar deze nieuwe `feed` container denormaliseren:

    {
      "id": "<post-id>",
      "type": "post",
      "postId": "<post-id>",
      "userId": "<post-author-id>",
      "userUsername": "<post-author-username>",
      "title": "<post-title>",
      "content": "<post-content>",
      "commentCount": <count-of-comments>,
      "likeCount": <count-of-likes>,
      "creationDate": "<post-creation-date>"
    }

Deze container is gepartitioneerd `type`door, die zich `post` altijd in onze items bevindt. Dit zorgt ervoor dat alle items in deze container zich in dezelfde partitie bevinden.

Ter verkrijging van de denormalie moeten we de wijzigings pijplijn die we eerder hebben geïntroduceerd voor het verzenden van de berichten naar die nieuwe container, aansluiten op de pijp lijn voor veranderingen in de feed. Een belang rijk voor deel is dat u er zeker van moet zijn dat we alleen de 100 meest recente berichten opslaan. anders kan de inhoud van de container groter worden dan de maximale grootte van een partitie. Dit wordt gedaan door het aanroepen van een [post-trigger](stored-procedures-triggers-udfs.md#triggers) wanneer een document wordt toegevoegd aan de container:

![Berichten in de feed-container worden genormaliseerd](./media/how-to-model-partition-example/denormalization-3.png)

Dit is de hoofd tekst van de post-trigger waarmee de verzameling wordt afgekapt:

```javascript
function truncateFeed() {
  const maxDocs = 100;
  var context = getContext();
  var collection = context.getCollection();

  collection.queryDocuments(
    collection.getSelfLink(),
    "SELECT VALUE COUNT(1) FROM f",
    function (err, results) {
      if (err) throw err;

      processCountResults(results);
    });

  function processCountResults(results) {
    // + 1 because the query didn't count the newly inserted doc
    if ((results[0] + 1) > maxDocs) {
      var docsToRemove = results[0] + 1 - maxDocs;
      collection.queryDocuments(
        collection.getSelfLink(),
        `SELECT TOP ${docsToRemove} * FROM f ORDER BY f.creationDate`,
        function (err, results) {
          if (err) throw err;

          processDocsToRemove(results, 0);
        });
    }
  }

  function processDocsToRemove(results, index) {
    var doc = results[index];
    if (doc) {
      collection.deleteDocument(
        doc._self,
        function (err) {
          if (err) throw err;

          processDocsToRemove(results, index + 1);
        });
    }
  }
}
```

De laatste stap bestaat uit het omleiden van onze query naar `feed` onze nieuwe container:

![De meest recente posts worden opgehaald](./media/how-to-model-partition-example/V3-Q6.png)

| **Latentie** | **RU-kosten** | **Prestaties** |
| --- | --- | --- |
| 9 MS | 16,97 RU | ✅ |

## <a name="conclusion"></a>Conclusie

Laten we eens kijken naar de algemene verbeteringen op het gebied van prestaties en schaal baarheid die we hebben geïntroduceerd in de verschillende versies van het ontwerp.

| | V1 | V2 | V3 |
| --- | --- | --- | --- |
| **[C1]** | 7 MS/5,71 RU | 7 MS/5,71 RU | 7 MS/5,71 RU |
| **[Q1]** | 2 MS/1 RU | 2 MS/1 RU | 2 MS/1 RU |
| **[C2]** | 9 MS/8,76 RU | 9 MS/8,76 RU | 9 MS/8,76 RU |
| **[Q2]** | 9 MS/19,54 RU | 2 MS/1 RU | 2 MS/1 RU |
| **[Q3]** | 130 MS/619,41 RU | 28 MS/201,54 RU | 4 MS/6,46 RU |
| **[C3]** | 7 MS/8,57 RU | 7 MS/15,27 RU | 7 MS/15,27 RU |
| **[Q4]** | 23 MS/27,72 RU | 4 MS/7,72 RU | 4 MS/7,72 RU |
| **[C4]** | 6 MS/7,05 RU | 7 MS/14,67 RU | 7 MS/14,67 RU |
| **[Q5]** | 59 MS/58,92 RU | 4 MS/8,92 RU | 4 MS/8,92 RU |
| **[Q6]** | 306 MS/2063,54 RU | 83 MS/532,33 RU | 9 MS/16,97 RU |

### <a name="we-have-optimized-a-read-heavy-scenario"></a>We hebben een lees-zwaar scenario geoptimaliseerd

U hebt wellicht gezien dat onze inspanningen zijn geconcentreerd op het verbeteren van de prestaties van Lees aanvragen (query's) op kosten voor schrijf aanvragen (opdrachten). In veel gevallen activeren schrijf bewerkingen nu de volgende denormalisatie via Change-feeds, waardoor ze meer reken tijd en langer kunnen worden realiseren.

Dit wordt gerechtvaardigd door het feit dat een blog platform (zoals de meeste sociale apps) lezen-zwaar is. Dit betekent dat de hoeveelheid Lees aanvragen die moet worden verzonden meestal vaak bestellingen van omvang hoger is dan de hoeveelheid schrijf aanvragen. Daarom is het zinvol schrijf aanvragen duurder te maken om te worden uitgevoerd, zodat Lees aanvragen goed koper en beter kunnen worden uitgevoerd.

Als we eens kijken naar de meest extreme optimalisatie die we hebben gedaan, is **[Q6]** van 2000 + RUs naar gewoon 17 RUs; We hebben geduurd dat door het denormaliseren van berichten tegen een prijs van circa 10 RUs per item. Omdat we veel meer feed aanvragen doen dan het maken of bijwerken van berichten, zijn de kosten voor deze denormalisatie te verwaarlozen gelet op de totale besparingen.

### <a name="denormalization-can-be-applied-incrementally"></a>Het denormaliseren kan stapsgewijs worden toegepast

De verbeteringen voor de schaal baarheid die in dit artikel worden besproken, omvatten het denormaliseren en dupliceren van gegevens in de gegevensset. Opgemerkt moet worden dat deze optimalisaties op dag 1 niet op de plaats hoeven te worden geplaatst. Query's die worden gefilterd op partitie sleutels, worden op schaal verbeterd, maar Kruis partitie query's kunnen volledig worden geaccepteerd als ze zelden of op een beperkte gegevensset worden genoemd. Als u alleen een prototype bouwt of een product start met een klein en beheerd gebruikers bestand, kunt u deze verbeteringen waarschijnlijk later voor bereid hebben. het is belang rijk dat u de prestaties van uw model [bewaakt](use-metrics.md) , zodat u kunt bepalen of en wanneer het tijd is om deze in te zetten.

De wijzigings feed die we gebruiken om updates naar andere containers te distribueren, slaan al deze updates permanent op. Dit maakt het mogelijk om alle updates te aanvragen sinds het maken van de container en het opschonen van de Boots trap als een eenmalige, opvallende weer gave, zelfs als uw systeem al veel gegevens bevat.

## <a name="next-steps"></a>Volgende stappen

Na deze inleiding tot praktische gegevens modellering en-partitionering kunt u de volgende artikelen controleren om de concepten te bekijken die we hebben behandeld:

- [Werken met data bases, containers en items](databases-containers-items.md)
- [Partitionering in Azure Cosmos DB](partitioning-overview.md)
- [Feed wijzigen in Azure Cosmos DB](change-feed.md)
