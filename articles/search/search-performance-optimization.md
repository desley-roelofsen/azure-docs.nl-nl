---
title: Optimalisatie van implementatie en prestaties
titleSuffix: Azure Cognitive Search
description: Leer technieken en aanbevolen procedures voor het afstemmen van de prestaties van Azure Cognitive Search en het configureren van de optimale schaal.
manager: nitinme
author: LiamCavanagh
ms.author: liamca
ms.devlang: rest-api
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 11/04/2019
ms.openlocfilehash: 4ff6779843546bd41423c140ef40de6a7e24b026
ms.sourcegitcommit: 598c5a280a002036b1a76aa6712f79d30110b98d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/15/2019
ms.locfileid: "74113259"
---
# <a name="deployment-strategies-and-best-practices-for-optimizing-performance-on-azure-cognitive-search"></a>Implementatie strategieën en aanbevolen procedures voor het optimaliseren van de prestaties van Azure Cognitive Search

In dit artikel worden aanbevolen procedures beschreven voor geavanceerde scenario's met geavanceerde vereisten voor schaal baarheid en beschik baarheid. 

## <a name="develop-baseline-numbers"></a>Basislijn nummers ontwikkelen
Bij optimalisatie voor zoek prestaties moet u zich richten op het verminderen van de uitvoerings tijd van de query. Hiervoor moet u weten wat een typische query belasting lijkt te zijn. De volgende richt lijnen kunnen u helpen bij het aankomen van basislijn query nummers.

1. Kies een doel latentie (of maximale tijds duur) waarmee een typische zoek opdracht moet worden voltooid.
2. Maak en test een echte werk belasting op uw zoek service met een realistische gegevensset om deze latentie tarieven te meten.
3. Beginnen met een laag aantal query's per seconde (QPS) en het aantal dat in de test wordt uitgevoerd geleidelijk verhogen, totdat de query latentie onder de gedefinieerde doel latentie daalt. Dit is een belang rijk referentie punt om u te helpen bij het plannen van de schaal als uw toepassing groeit in gebruik.
4. Gebruik waar mogelijk HTTP-verbindingen. Als u de Azure Cognitive Search .NET SDK gebruikt, betekent dit dat u een instantie of [SearchIndexClient](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.searchindexclient) -exemplaar opnieuw moet gebruiken. Als u de rest API gebruikt, moet u één httpclient maakt opnieuw gebruiken.
5. U kan de query op aanvragen van query's variëren zodat de zoek actie plaatsvindt in verschillende onderdelen van de index. Variatie is belang rijk omdat als u dezelfde Zoek opdrachten doorlopend uitvoert, het opslaan van gegevens in de cache wordt gestart, waardoor de prestaties kunnen worden verbeterd.
6. U kunt de structuur van query-aanvragen variëren zodat u verschillende soorten query's krijgt. Niet alle zoek query's moeten op hetzelfde niveau worden uitgevoerd. Bijvoorbeeld: een zoek-of zoek voorstel voor documenten is doorgaans sneller dan een query met een groot aantal facetten en filters. De test samenstelling moet verschillende query's bevatten, in ongeveer dezelfde verhouding als bij de productie.  

Tijdens het maken van deze test werkbelastingen zijn er enkele kenmerken van Azure Cognitive Search die u moet onthouden:

+ Het is mogelijk om uw service te overbelasten door te veel zoek query's tegelijk te pushen. Als dit gebeurt, worden HTTP 503-antwoord codes weer geven. Als u een 503 tijdens het testen wilt voor komen, kunt u beginnen met verschillende bereiken van zoek opdrachten om de verschillen in latentie te zien wanneer u meer zoek aanvragen toevoegt.

+ Met Azure Cognitive Search worden index taken niet op de achtergrond uitgevoerd. Als uw service tegelijkertijd query's uitvoert en werk belastingen gelijktijdig indexeert, moet u rekening houden met het introduceren van index taken in uw query tests of door opties te verkennen voor het uitvoeren van index taken tijdens de piek uren.

> [!NOTE]
> [Visual Studio load tests](https://www.visualstudio.com/docs/test/performance-testing/run-performance-tests-app-before-release) zijn een goede manier om uw benchmark tests uit te voeren, omdat u hiermee HTTP-aanvragen kunt uitvoeren, zoals u moet doen om query's uit te voeren op Azure Cognitive Search en parallel Lise ring van aanvragen mogelijk te maken.
> 
> 

## <a name="scaling-for-high-query-volume-and-throttled-requests"></a>Schalen voor hoog query volume en vertraagde aanvragen
Wanneer u te veel vertraagde aanvragen ontvangt of als u de snelheid van de doel latentie overschrijdt van een verbeterde query belasting, kunt u de latentie snelheid op twee manieren verlagen:

1. **Replica's verg Roten:**  Een replica is een kopie van uw gegevens waarmee Azure-Cognitive Search de belasting van aanvragen op meerdere exemplaren kan verdelen.  Alle taak verdeling en replicatie van gegevens tussen replica's wordt beheerd door Azure Cognitive Search en u kunt het aantal replica's dat voor uw service wordt toegewezen op elk gewenst moment wijzigen.  U kunt Maxi maal 12 replica's toewijzen in een Standard-zoek service en 3 replica's in een Basic Search-service. Replica's kunnen worden aangepast op basis van de [Azure Portal](search-create-service-portal.md) of [Power shell](search-manage-powershell.md).
2. **Zoek niveau verhogen:**  Azure Cognitive Search komt in een [aantal lagen](https://azure.microsoft.com/pricing/details/search/) voor en elk van deze lagen biedt verschillende prestatie niveaus.  In sommige gevallen hebt u mogelijk zoveel query's die de laag die u aanmeldt niet voldoende latentie kan bieden, zelfs wanneer er replica's benut uitvallen. In dit geval kunt u overwegen om gebruik te maken van een van de hogere Zoek lagen, zoals de laag Azure Cognitive Search S3, die geschikt is voor scenario's met een groot aantal documenten en extreem hoge query werkbelastingen.

## <a name="scaling-for-slow-individual-queries"></a>Verg Roten/verkleinen voor langzame afzonderlijke query's
Een andere reden voor hoge latentie is dat een enkele query te lang duurt om te worden voltooid. In dit geval is het toevoegen van replica's geen uitkomst. Twee mogelijke opties die kunnen helpen bij het volgende:

1. **Partities verhogen** Een partitie is een mechanisme voor het splitsen van gegevens over extra resources. Wanneer u een tweede partitie toevoegt, worden gegevens gesplitst in twee, wordt deze door een derde partitie gesplitst in drie, enzovoort. Eén positieve neven effect is dat tragere query's soms sneller worden uitgevoerd als gevolg van parallelle computing. We hebben op parallel Lise ring genoteerd dat er weinig selectiviteit-query's zijn, zoals query's die overeenkomen met veel documenten, of facetten die aantallen bieden over een groot aantal documenten. Aangezien er een belang rijke berekening is vereist om de relevancy van de documenten te beoordelen, of om het aantal documenten te tellen, kunnen er met extra partities sneller query's worden uitgevoerd.  
   
   Er kunnen Maxi maal 12 partities in de Standard Search-service en 1 partitie in de Basic Search-service zijn.  Partities kunnen worden aangepast op basis van de [Azure Portal](search-create-service-portal.md) of [Power shell](search-manage-powershell.md).

2. **Velden met hoge kardinaliteit beperken:** Een hoog veld voor kardinaliteit bestaat uit een facetbaar of filterbaar veld met een groot aantal unieke waarden. als gevolg hiervan worden aanzienlijke bronnen verbruikt bij het berekenen van resultaten. Zo kunt u bijvoorbeeld een product-ID of beschrijvings veld instellen als facetable/filterbaar als hoge kardinaliteit, omdat de meeste waarden van document naar document uniek zijn. Beperk waar mogelijk het aantal velden met hoge kardinaliteit.

3. **Zoek niveau verhogen:**  Het verplaatsen naar een hogere Azure Cognitive Search-laag kan een andere manier zijn om de prestaties van trage query's te verbeteren. Elke hogere laag biedt snellere Cpu's en meer geheugen, beide met een positieve invloed op de prestaties van query's.

## <a name="scaling-for-availability"></a>Schalen voor Beschik baarheid
Replica's helpen niet alleen de latentie van query's te beperken, maar kunnen ook hoge Beschik baarheid toestaan. Met één replica moet u een periodieke downtime verwachten omdat de server opnieuw wordt opgestart na software-updates of voor andere onderhouds gebeurtenissen die optreden.  Daarom is het belang rijk om te overwegen of uw toepassing hoge Beschik baarheid van zoek opdrachten (query's) vereist en schrijf bewerkingen (indexerings gebeurtenissen). Azure Cognitive Search biedt SLA-opties voor alle betaalde zoek aanbiedingen met de volgende kenmerken:

* 2 replica's voor hoge Beschik baarheid van alleen-lezen workloads (query's)
* 3 of meer replica's voor hoge Beschik baarheid van werk belastingen met lees bewerkingen (query's en indexering)

Ga naar de [Azure Cognitive Search Service Level Agreement](https://azure.microsoft.com/support/legal/sla/search/v1_0/)voor meer informatie hierover.

Omdat replica's kopieën zijn van uw gegevens, kunnen met meerdere replica's Azure Cognitive Search de computer opnieuw opstarten en onderhoud uitvoeren voor een replica, terwijl het uitvoeren van query's kan worden voortgezet op andere replica's. Als u replica's hebt verwijderd, kunt u de prestaties van de query afnemen, ervan uitgaande dat deze replica's een minder gebruikte bron zijn.

## <a name="scaling-for-geo-distributed-workloads-and-geo-redundancy"></a>Schalen voor geografisch gedistribueerde workloads en geo-redundantie
Voor geografisch gedistribueerde werk belastingen hebben gebruikers die zich ver van het Data Center hosten van Azure Cognitive Search een hogere latentie snelheid. Een beperking is het inrichten van meerdere zoek services in regio's met dichter nabijheid voor deze gebruikers. Azure Cognitive Search biedt momenteel geen automatische methode voor het geo-repliceren van Azure Cognitive Search-indexen in regio's, maar er zijn enkele technieken die kunnen worden gebruikt om dit proces eenvoudig te implementeren en te beheren. Deze worden in de volgende paar secties beschreven.

Het doel van een geografisch gedistribueerde set Zoek Services is om twee of meer indexen beschikbaar te hebben in twee of meer regio's waar een gebruiker wordt doorgestuurd naar de Azure Cognitive Search-service die de laagste latentie biedt zoals in dit voor beeld wordt weer gegeven:

   ![Meerdere tabbladen van services per regio][1]

### <a name="keeping-data-in-sync-across-multiple-azure-cognitive-search-services"></a>Gegevens synchroon houden op meerdere Azure Cognitive Search Services
Er zijn twee opties voor het bewaren van uw gedistribueerde zoek services die bestaan uit het gebruik van de [azure Cognitive Search indexer](search-indexer-overview.md) of de Push-API (ook wel de [Azure Cognitive Search rest API](https://docs.microsoft.com/rest/api/searchservice/)genoemd).  

### <a name="use-indexers-for-updating-content-on-multiple-services"></a>Indexeer functies gebruiken voor het bijwerken van inhoud op meerdere services

Als u Indexeer functie al gebruikt voor één service, kunt u een tweede Indexeer functie configureren voor een tweede service om hetzelfde gegevens bron object te gebruiken en gegevens uit dezelfde locatie op te halen. Elke service in elke regio heeft een eigen Indexeer functie en een doel index (uw zoek index wordt niet gedeeld, wat betekent dat de gegevens worden gedupliceerd), maar elke Indexeer functie verwijst naar dezelfde gegevens bron.

Hier volgt een globaal visueel element van wat die architectuur eruit zou zien.

   ![Eén gegevens bron met combi Naties van gedistribueerde Indexeer functies en services][2]

### <a name="use-rest-apis-for-pushing-content-updates-on-multiple-services"></a>REST-Api's gebruiken voor het pushen van inhouds updates op meerdere services
Als u de Azure Cognitive Search-REST API gebruikt om [inhoud in uw Azure Cognitive search-index te pushen](https://docs.microsoft.com/rest/api/searchservice/update-index), kunt u uw verschillende Zoek Services synchroon laten door wijzigingen in alle zoek services te pushen wanneer een update is vereist. Zorg ervoor dat u in uw code cases afhandelt waarbij een update naar één zoek service mislukt, maar slaagt voor andere zoek services.

## <a name="leverage-azure-traffic-manager"></a>Gebruik Azure Traffic Manager
Met [Azure Traffic Manager](../traffic-manager/traffic-manager-overview.md) kunt u aanvragen naar meerdere geo-locaties routeren die vervolgens door meerdere zoek services worden ondersteund. Een voor deel van het Traffic Manager is dat het Azure Cognitive Search kan testen om er zeker van te zijn dat het beschikbaar is en gebruikers kan omleiden naar alternatieve Zoek Services in het geval van downtime. Daarnaast kunt u, als u zoek aanvragen begeleidt via Azure-websites, in azure Traffic Manager taken verdelen waarbij de website wel of niet Azure Cognitive Search is. Hier volgt een voor beeld van de architectuur die gebruikmaakt van Traffic Manager.

   ![Meerdere tabbladen van services per regio, met centrale Traffic Manager][3]

## <a name="next-steps"></a>Volgende stappen
Zie [service limieten in Azure Cognitive Search](search-limits-quotas-capacity.md)voor meer informatie over de prijs categorieën en de limieten voor de services voor elk van deze opties.

Bezoek [capaciteits planning](search-capacity-planning.md) voor meer informatie over combi Naties van partities en replica's.

Bekijk de volgende video voor meer informatie over de prestaties en voor een aantal demonstraties van het implementeren van de optimalisaties die in dit artikel worden besproken:

> [!VIDEO https://channel9.msdn.com/Events/Microsoft-Azure/AzureCon-2015/ACON319/player]
> 
> 

<!--Image references-->
[1]: ./media/search-performance-optimization/geo-redundancy.png
[2]: ./media/search-performance-optimization/scale-indexers.png
[3]: ./media/search-performance-optimization/geo-search-traffic-mgr.png
