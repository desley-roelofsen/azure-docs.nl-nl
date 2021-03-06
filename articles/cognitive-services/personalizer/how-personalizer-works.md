---
title: 'Hoe Personaler werkt: Personaler'
titleSuffix: Azure Cognitive Services
description: Personaler gebruikt machine learning om te ontdekken welke actie in een context moet worden gebruikt. Elke Learning-lus heeft een model dat uitsluitend is getraind op gegevens die u hebt verzonden via rang en beloning. Elke Learning-lus is volledig onafhankelijk van elkaar.
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: personalizer
ms.topic: conceptual
ms.date: 10/23/2019
ms.author: diberry
ms.openlocfilehash: 902bf84ebf090cf9f0f886ad1e774ff7bdfeca93
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/04/2019
ms.locfileid: "73490753"
---
# <a name="how-personalizer-works"></a>Hoe Personalizer werkt

Personaler gebruikt machine learning om te ontdekken welke actie in een context moet worden gebruikt. Elke Learning-lus heeft een model dat uitsluitend is getraind op gegevens die u hebt verzonden via **rang** en **beloning** . Elke Learning-lus is volledig onafhankelijk van elkaar. Maak een leer proces voor elk onderdeel of gedrag van de toepassing die u wilt personaliseren.

Voor elke lus **roept u de Rank API aan** op basis van de huidige context, waarbij:

* Lijst met mogelijke acties: inhouds items waarvan u de bovenste actie wilt selecteren.
* Lijst met [context functies](concepts-features.md): contextuele relevante gegevens zoals gebruiker, inhoud en context.

De **positie** -API besluit om de volgende te gebruiken:

* _Exploit_: het huidige model om de beste actie te bepalen op basis van gegevens in het verleden.
* _Verkennen_: Selecteer een andere actie in plaats van de bovenste actie.

De **belonings** -API:

* Verzamelt gegevens om het model te trainen door de functies en belonings scores van elke positie oproep vast te leggen.
* Gebruikt deze gegevens om het model bij te werken op basis van de configuratie die is opgegeven in het _Learning-beleid_.

## <a name="architecture"></a>Architectuur

In de volgende afbeelding ziet u de architectuur stroom van het aanroepen van de rang-en belonings aanroepen:

![alternatieve tekst](./media/how-personalizer-works/personalization-how-it-works.png "Hoe persoonlijke instellingen werken")

1. Personaler gebruikt een intern AI-model om de positie van de actie te bepalen.
1. De service beslist of het huidige model moet worden misbruikt of dat er nieuwe opties voor het model moeten worden bekeken.  
1. Het resultaat van de rang schikking wordt verzonden naar EventHub.
1. Wanneer Personaler de beloning ontvangt, wordt de vergoeding verzonden naar EventHub. 
1. De positie en beloning worden gecorreleerd.
1. Het AI-model wordt bijgewerkt op basis van de correlatie resultaten.
1. De engine voor het ingrijpen van de functie wordt bijgewerkt met het nieuwe model. 

## <a name="research-behind-personalizer"></a>Onderzoek achter persoonlijker

Personaler is gebaseerd op wetenschap en onderzoek op het gebied van versterking van het [onderwijs](concepts-reinforcement-learning.md) , waaronder documenten, onderzoeksactiviteiten en lopende onderzoeksgebieden in micro soft Research.

## <a name="terminology"></a>Terminologie

* **Learning-lus**: u kunt een leer proces maken voor elk deel van uw toepassing dat kan profiteren van personalisatie. Als u meer dan één ervaring hebt om persoonlijker te maken, maakt u een lus voor elke. 

* **Acties**: acties zijn de inhouds items, zoals producten of promoties, waaruit u kunt kiezen. Personaler kiest voor de beste actie die aan uw gebruikers wordt weer gegeven. dit wordt aangeduid met de _actie_-API. Elke actie kan functies hebben verzonden met de rang aanvraag.

* **Context**: Geef informatie op over uw context om een nauw keurigere positie te geven, bijvoorbeeld:
    * Uw gebruiker.
    * Het apparaat waarop deze zich bevinden. 
    * De huidige tijd.
    * Andere gegevens over de huidige situatie.
    * Historische gegevens over de gebruiker of context.

    Uw specifieke toepassing heeft mogelijk andere context informatie. 

* **[Functies](concepts-features.md)** : een eenheid van informatie over een inhouds item of een gebruikers context.

* **Beloning**: een meting van de manier waarop de gebruiker heeft gereageerd op de geretourneerde actie-API, als een score tussen 0 en 1. De waarde van 0 tot 1 wordt ingesteld door uw bedrijfs logica, op basis van de manier waarop de keuze heeft geholpen bij het bereiken van uw zakelijke doel stellingen voor persoonlijke instellingen. 

* **Exploratie**: de personaler-service wordt gebruikt wanneer, in plaats van de beste actie te retour neren, een andere actie voor de gebruiker wordt gekozen. De Personaler-service vermijdt drift, stagnation en kan worden aangepast aan het doorlopende gedrag van gebruikers door te verkennen. 

* **Duur**van het experiment: de hoeveelheid tijd die de personaler-service wacht op een beloning, vanaf het moment dat de positie oproep voor die gebeurtenis is opgetreden.

* **Inactieve gebeurtenissen**: een inactieve gebeurtenis is de locatie waar u positie noemt, maar u weet niet zeker dat de gebruiker het resultaat ooit zal zien vanwege beslissingen van de client toepassing. Inactieve gebeurtenissen bieden u de mogelijkheid om persoonlijke resultaten te maken en op te slaan. vervolgens besluit u deze later te negeren zonder dat dit van invloed is op het machine learning model.

* **Model**: een personalr model legt alle gegevens vast die zijn geleerd over het gedrag van gebruikers, en trainings gegevens ophalen uit de combi natie van de argumenten die u verzendt voor het classificeren en beloningen en met een opleidings gedrag dat door het leer beleid wordt bepaald. 

* **Trainings beleid**: hoe personaler een model voor elke gebeurtenis moet treinen, wordt bepaald door Meta-para meters die van invloed zijn op de werking van de machine learning-algoritmen. Nieuwe Personaler-lussen beginnen met een standaard learningbeleid, waarmee de prestaties kunnen worden vertraagd. Bij het uitvoeren van [evaluaties](concepts-offline-evaluation.md)kan personaler een nieuw leer beleid maken dat specifiek is geoptimaliseerd voor de gebruiks voorbeelden van uw lus. Personaler presteert aanzienlijk beter met beleids regels die zijn geoptimaliseerd voor elke specifieke lus, die tijdens de evaluatie wordt gegenereerd.

## <a name="example-use-cases-for-personalizer"></a>Voor beelden van use cases for Personaler

* Verklaring van intentie & ondubbelzinnigheid: Help uw gebruikers een betere ervaring te hebben wanneer hun intentie niet duidelijk is door een optie te bieden die aan elke gebruiker is aangepast.
* Standaard suggesties voor menu's & opties: laat de bot het meest waarschijnlijke item op een gepersonaliseerde manier Voorst Ellen als een eerste stap, in plaats van een niet-persoonlijk menu of lijst met alternatieven te presen teren.
* Bot-eigenschappen & Toon: voor bots die de Toon, uitgebreidheid en schrijf stijl kunnen variëren, kunt u op een aangepaste manier overwegen deze eigenschappen te variëren.
* Inhoud van meldings & waarschuwing: Beslis welke tekst u voor waarschuwingen wilt gebruiken om gebruikers meer te benaderen.
* Timing van meldings & waarschuwing: laat u weten wanneer u meldingen naar gebruikers wilt verzenden om ze te benaderen.

## <a name="how-to-use-personalizer-in-a-web-application"></a>Personaliseren gebruiken in een webtoepassing

Het toevoegen van een lus aan een webtoepassing omvat:

* Bepaal welke ervaring u wilt aanpassen, welke acties en functies u hebt, welke context functies u moet gebruiken en welke beloning u moet instellen.
* Voeg een verwijzing naar de personalisatie-SDK in uw toepassing toe.
* Roep de classificatie-API aan wanneer u klaar bent om te personaliseren.
* Sla de gebeurtenis-eigenbestand op. U stuurt later een beloning met de belonings-API.
1. Roep Activate aan voor de gebeurtenis zodra u zeker weet dat de gebruiker uw persoonlijke pagina heeft bekeken.
1. Wacht tot de gebruiker geclassificeerde inhoud heeft geselecteerd. 
1. Roep belonings-API om op te geven hoe goed de uitvoer van de Rank API is.

## <a name="how-to-use-personalizer-with-a-chat-bot"></a>Personaler gebruiken met een chat sessie

In dit voor beeld ziet u hoe u personalisatie kunt gebruiken om een standaard suggestie te maken in plaats van de gebruiker elke keer een reeks menu's of keuzes te verzenden.

* Haal de [code](https://github.com/Azure-Samples/cognitive-services-personalizer-samples/tree/master/samples/ChatbotExample) op voor dit voor beeld.
* Stel uw bot-oplossing in. Zorg ervoor dat u uw LUIS-toepassing publiceert. 
* Rang en beloning-API-aanroepen voor bot beheren.
    * Voeg code toe om LUIS intentie verwerking te beheren. Als de **geen** wordt geretourneerd als de hoogste intentie of als de Score van het hoogste niveau lager is dan de drempel waarde voor uw bedrijfs logica, stuurt u de lijst met intenties naar persoonlijker om de intentie te rangschikken.
    * De lijst met intents weer geven aan de gebruiker als selecteerbaar koppelingen met de eerste intentie van de rang orde die het beste bij de positie API-reactie ligt.
    * Leg de selectie van de gebruiker vast en verzend deze in de API-aanroep van beloning. 

### <a name="recommended-bot-patterns"></a>Aanbevolen bot-patronen

* Stel de aanroepen van de gebruikers interface van de persoonlijker-API telkens wanneer er een ondubbelzinnigheid nodig is, in tegens telling tot cache resultaten voor elke gebruiker. Het resultaat van disambiguating intentie kan in de loop van de tijd worden gewijzigd voor één persoon en de classificatie-API om afwijkingen te verkennen, versnelt het algehele leer proces.
* Kies een interactie die gemeen schappelijk is voor veel gebruikers, zodat u voldoende gegevens hebt om te personaliseren. Inleidende vragen kunnen bijvoorbeeld beter worden afgestemd op de diepte van kleinere toelichtingen in de conversatie grafiek die slechts een paar gebruikers kan krijgen.
* Gebruik rank API-aanroepen om de conversaties ' eerste suggestie is geschikt ' in te scha kelen, waarbij de gebruiker wordt gevraagd om X? of ' hebt u X? ' en de gebruiker kan gewoon bevestigen; in plaats van opties te geven aan de gebruiker, waar ze moeten kiezen in een menu. Bijvoorbeeld gebruiker: ' Ik wil graag een koffie-bot best Ellen: ' wilt u graag een dubbele espresso hebben? '. Op deze manier is het belonings signaal ook sterk, omdat het direct op de ene suggestie van toepassing is.

## <a name="how-to-use-personalizer-with-a-recommendation-solution"></a>Personaler gebruiken met een aanbevelings oplossing

Gebruik uw aanbevelings Engine om een grote catalogus te filteren op enkele items die vervolgens kunnen worden weer gegeven als 30 mogelijke acties die worden verzonden naar de classificatie-API.

U kunt aanbevelings motoren gebruiken met Personaler:

* Stel de [aanbevelings oplossing](https://github.com/Microsoft/Recommenders/)in. 
* Bij het weer geven van een pagina roept u het Aanbevelings model op om een korte lijst met aanbevelingen te krijgen.
* Roep personalisatie aan om de uitvoer van Aanbevelings oplossing te rangschikken.
* Stuur feedback over uw actie van uw gebruiker met behulp van de API-aanroep van beloning.


## <a name="pitfalls-to-avoid"></a>Valk uilen om te voor komen

* Gebruik persoonlijke voor keuren niet wanneer het persoonlijke gedrag niet iets is dat kan worden gedetecteerd voor alle gebruikers, maar iets dat moet worden onthouden voor specifieke gebruikers of afkomstig is van een gebruikersspecifieke lijst met alternatieven. Als u bijvoorbeeld met Personaler een pizza-volg orde van een lijst met 20 mogelijke menu-items wilt gebruiken, is het nuttig, maar welke contact persoon moet worden aangeroepen vanuit de lijst met contact personen van de gebruiker wanneer u hulp nodig hebt bij Childcare (zoals ' Grandma ') is niet iets dat persoonlijk is in uw gebruikers basis.


## <a name="adding-content-safeguards-to-your-application"></a>Inhouds beveiligingen toevoegen aan uw toepassing

Als uw toepassing grote afwijkingen toestaat in inhoud die wordt weer gegeven voor gebruikers, en een deel van die inhoud onveilig of ongepast is voor sommige gebruikers, moet u ervoor zorgen dat de juiste veiligheids maatregelen worden getroffen om te voor komen dat uw gebruikers onaanvaardbaar kunnen zien Inhoudbeheer. Het beste patroon om beveiligingen te implementeren is:
    * De lijst met te rangschikken acties ophalen.
    * Filter de items die niet geschikt zijn voor de doel groep.
    * Rang Schik deze levensvat bare acties alleen.
    * De bovenste geclassificeerde actie weer geven voor de gebruiker.

In sommige architecturen is de bovenstaande reeks mogelijk moeilijk te implementeren. In dat geval is er een alternatieve benadering voor het implementeren van veiligheids maatregelen na de rang schikking, maar een inrichting moet worden gemaakt, zodat acties die buiten de beveiliging vallen, niet worden gebruikt voor het trainen van het Personaler model.

* Haal de lijst met acties op die moeten worden gerangschikt, waarbij learning is gedeactiveerd.
* Rang schikkings acties.
* Controleer of de meest voorkomende actie levensvatbaar is.
    * Als de meest voorkomende actie haalbaar is, kunt u learning voor deze classificatie activeren en deze vervolgens weer geven aan de gebruiker.
    * Als de meest voorkomende actie niet haalbaar is, moet u learning voor deze classificatie niet activeren en zelf bepalen hoe u uw eigen logica of alternatieve benaderingen kunt zien wat u aan de gebruiker kunt weer geven. Ook als u de optie voor de tweede beste gerangschikt gebruikt, moet u learning voor deze classificatie niet activeren.

## <a name="verifying-adequate-effectiveness-of-personalizer"></a>De juiste effectiviteit van Personaler controleren

U kunt de effectiviteit van personalisatie periodiek controleren door [offline-evaluaties](how-to-offline-evaluation.md) uit te voeren

## <a name="next-steps"></a>Volgende stappen

Meer informatie over [hoe u personaler kunt gebruiken](where-can-you-use-personalizer.md).
[Offline-evaluaties](how-to-offline-evaluation.md) uitvoeren
