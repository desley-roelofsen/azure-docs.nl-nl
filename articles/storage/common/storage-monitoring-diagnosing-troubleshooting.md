---
title: Azure Storage controleren, diagnosticeren en problemen oplossen | Microsoft Docs
description: Gebruik functies zoals opslag analyse, logboek registratie aan client zijde en andere hulpprogram ma's van derden om problemen met Azure Storage te identificeren, vast te stellen en op te lossen.
author: normesta
ms.service: storage
ms.topic: conceptual
ms.date: 09/23/2019
ms.author: normesta
ms.reviewer: fryu
ms.subservice: common
ms.openlocfilehash: 34aa4ff6c54b34acf865af0b57c3dfa7945a637c
ms.sourcegitcommit: 7df70220062f1f09738f113f860fad7ab5736e88
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2019
ms.locfileid: "71212831"
---
# <a name="monitor-diagnose-and-troubleshoot-microsoft-azure-storage"></a>Microsoft Azure Storage bewaken, problemen opsporen en oplossen
[!INCLUDE [storage-selector-portal-monitoring-diagnosing-troubleshooting](../../../includes/storage-selector-portal-monitoring-diagnosing-troubleshooting.md)]

## <a name="overview"></a>Overzicht
Diagnose-en probleemoplossings problemen in een gedistribueerde toepassing die wordt gehost in een cloud omgeving, kunnen complexer zijn dan in traditionele omgevingen. Toepassingen kunnen worden geïmplementeerd in een PaaS-of IaaS-infra structuur, on-premises, op een mobiel apparaat of in een bepaalde combi natie van deze omgevingen. Normaal gesp roken wordt het netwerk verkeer van uw toepassing in open bare en particuliere netwerken geplaatst en kan uw toepassing gebruikmaken van meerdere opslag technologieën zoals Microsoft Azure Storage tabellen, blobs, wacht rijen of bestanden, naast andere gegevensopslag plaatsen, zoals relationele gegevens en document databases.

Als u dergelijke toepassingen wilt beheren, moet u ze proactief controleren en inzicht krijgen in het vaststellen en oplossen van alle aspecten van deze en hun afhankelijke technologieën. Als gebruiker van Azure Storage services moet u de opslag services die uw toepassing gebruikt, continu bewaken voor eventuele onverwachte wijzigingen in het gedrag (zoals trager dan gebruikelijke reactie tijden), en logboek registratie gebruiken om gedetailleerde gegevens te verzamelen en een probleem in te analyseren diepga. De diagnostische gegevens die u van zowel controle als logboek registratie ontvangt, helpen u bij het bepalen van de hoofd oorzaak van het probleem dat de toepassing heeft ondervonden. Vervolgens kunt u het probleem oplossen en bepalen welke stappen u kunt ondernemen om de problemen op te lossen. Azure Storage is een kern service van Azure en vormt een belang rijk onderdeel van de meeste oplossingen die klanten implementeren naar de Azure-infra structuur. Azure Storage bevat mogelijkheden voor het vereenvoudigen van bewakings-, diagnose-en probleem oplossing voor opslag problemen in uw Cloud toepassingen.

> [!NOTE]
> Registratie wordt op dit moment niet door Azure Files ondersteund.
>

Zie [end-to-end-probleem oplossing met behulp van Azure Storage metrische gegevens en logboek registratie, AzCopy en Message Analyzer](../storage-e2e-troubleshooting.md)voor een praktische hand leiding voor end-to-end probleem oplossing in azure Storage toepassingen.

* [Inleiding]
  * [Hoe deze hand leiding is ingedeeld]
* [Uw opslag service controleren]
  * [Service status controleren]
  * [Bewakings capaciteit]
  * [Beschik baarheid bewaken]
  * [Prestaties bewaken]
* [Problemen met opslag vaststellen]
  * [Service status problemen]
  * [Prestatie problemen]
  * [Fouten vaststellen]
  * [Problemen met de opslag emulator]
  * [Hulpprogram ma's voor opslag logboek registratie]
  * [Hulpprogram ma's voor netwerk logboek registratie gebruiken]
* [End-to-end tracering]
  * [Gerelateerde logboek gegevens]
  * [Client aanvraag-ID]
  * [Server aanvraag-ID]
  * [Timestamps]
* [Hulp bij het oplossen van problemen]
  * [Prestatiegegevens geven hoge AverageE2ELatency en lage AverageServerLatency aan]
  * [Prestatiegegevens geven lage AverageE2ELatency en lage AverageServerLatency aan, maar de client ondervindt hoge latentie]
  * [Prestatiegegevens geven hoge AverageServerLatency aan]
  * [U ervaart onverwachte vertragingen bij de levering van berichten in een wachtrij]
  * [Prestatiegegevens geven een toename in PercentThrottlingError aan]
  * [Prestatiegegevens geven een toename in PercentTimeoutError aan]
  * [Prestatiegegevens geven een toename in PercentNetworkError aan]
  * [De client ontvangt HTTP 403-meldingen (verboden)]
  * [De client ontvangt HTTP 404-meldingen (niet gevonden)]
  * [De client ontvangt berichten van de HTTP-409 (Conflict)]
  * [Metrische gegevens tonen lage PercentSuccess of analytics logboekvermeldingen bewerkingen hebben met de status van ClientOtherErrors]
  * [Capaciteit metrische gegevens tonen een onverwachte toename in opslaggebruik capaciteit]
  * [Het probleem zich voordoet de opslagemulator voor ontwikkeling of tests gebruiken]
  * [U ondervindt problemen met het installeren van de Azure SDK voor .NET]
  * [U hebt een ander probleem met storage-service]
  * [Problemen met Vhd's op virtuele Windows-machines oplossen](../../virtual-machines/windows/troubleshoot-vhds.md)   
  * [Problemen met Vhd's op virtuele Linux-machines oplossen](../../virtual-machines/linux/troubleshoot-vhds.md)
  * [Problemen met Azure Files oplossen met Windows](../files/storage-troubleshoot-windows-file-connection-problems.md)   
  * [Problemen met Azure Files oplossen met Linux](../files/storage-troubleshoot-linux-file-connection-problems.md)
* [Bijlagen]
  * [Bijlage 1: HTTP-en HTTPS-verkeer vastleggen met behulp van Fiddler]
  * [Bijlage 2: Wireshark gebruiken om netwerk verkeer vast te leggen]
  * [Bijlage 3: Micro soft Message Analyzer gebruiken om netwerk verkeer vast te leggen]
  * [Aanhangsel 4: Excel gebruiken voor het weer geven van metrische gegevens en logbestanden]
  * [Bijlage 5: Bewaking met Application Insights voor Azure DevOps]

## <a name="introduction"></a>Inleiding
In deze hand leiding wordt beschreven hoe u functies zoals Azure Opslaganalyse, logboek registratie aan client zijde in de Azure Storage-client bibliotheek en andere hulpprogram ma's van derden kunt gebruiken voor het identificeren, vaststellen en oplossen van Azure Storage gerelateerde problemen.

![][1]

Deze hand leiding is voornamelijk bedoeld voor ontwikkel aars van onlineservices die gebruikmaken van Azure Storage services en IT-professionals die verantwoordelijk zijn voor het beheer van dergelijke onlineservices. De doel stellingen van deze hand leiding zijn:

* Om u te helpen bij het onderhouden van de status en prestaties van uw Azure Storage-accounts.
* Om u te helpen de benodigde processen en hulpprogram ma's te gebruiken om te bepalen of een probleem of probleem in een toepassing is gekoppeld aan Azure Storage.
* Om u te voorzien van praktische richt lijnen voor het oplossen van problemen met betrekking tot Azure Storage.

### <a name="how-this-guide-is-organized"></a>Hoe deze hand leiding is ingedeeld
In de sectie[uw opslag service controleren]wordt beschreven hoe u de status en prestaties van uw Azure Storage-services kunt controleren met behulp van Azure Opslaganalyse metrieken (metrische gegevens voor opslag).

In de sectie '[problemen met opslag vaststellen]' wordt beschreven hoe u problemen kunt vaststellen met behulp van Azure Opslaganalyse logboek registratie (logboek registratie van opslag). Ook wordt beschreven hoe u logboek registratie aan client zijde inschakelt met behulp van de-faciliteiten in een van de client Bibliotheken, zoals de Storage-client bibliotheek voor .NET of de Azure SDK voor Java.

In de sectie[End-to-end tracering]wordt beschreven hoe u de informatie in verschillende logboek bestanden en metrische gegevens kunt correleren.

De sectie '[Hulp bij het oplossen van problemen]' bevat richt lijnen voor probleem oplossing voor enkele veelvoorkomende problemen met betrekking tot de opslag.

De '[bijlagen]' bevatten informatie over het gebruik van andere hulpprogram ma's, zoals wireshark en netmon voor het analyseren van netwerk pakket gegevens, het Fiddler voor het analyseren van HTTP/HTTPS-berichten en micro soft Message Analyzer voor het correleren van logboek gegevens.

## <a name="monitoring-your-storage-service"></a>Uw opslag service controleren
Als u bekend bent met de bewaking van Windows-prestaties, kunt u de metrische gegevens van de opslag beschouwen als Azure Storage equivalent van prestatie meter items van Windows. In metrische gegevens over opslag vindt u een uitgebreide set metrische gegevens (prestatie meter items in de terminologie van Windows prestatie meter), zoals service beschikbaarheid, het totale aantal aanvragen voor de service of het percentage voltooide aanvragen aan de service. Zie voor een volledige lijst met beschik bare metrische gegevens het [tabel schema voor Opslaganalyse metrische gegevens](https://msdn.microsoft.com/library/azure/hh343264.aspx). U kunt opgeven of u wilt dat de opslag service elk uur of elke minuut metrische gegevens verzamelt en samenvoegt. Voor meer informatie over het inschakelen van metrische gegevens en het bewaken van uw opslag accounts raadpleegt u metrische waarden voor [opslag inschakelen en statistieken weer geven](https://go.microsoft.com/fwlink/?LinkId=510865).

U kunt kiezen welke metrische gegevens per uur u wilt weer geven in de [Azure Portal](https://portal.azure.com) en regels configureren waarmee beheerders per e-mail op de hoogte worden gesteld wanneer een metrische waarde van elk uur een bepaalde drempel waarde overschrijdt. Zie [waarschuwings meldingen ontvangen](/azure/monitoring-and-diagnostics/monitoring-overview-alerts)voor meer informatie.

U wordt aangeraden [Azure monitor voor opslag](../../azure-monitor/insights/storage-insights-overview.md) te bekijken (preview). Het is een functie van Azure Monitor die uitgebreide controle biedt over uw Azure Storage-accounts door een uniforme weer gave te bieden van de prestaties, capaciteit en beschik baarheid van uw Azure Storage services. U hoeft niets in te scha kelen of te configureren, en u kunt deze metrische gegevens direct weer geven vanuit de vooraf gedefinieerde interactieve diagrammen en andere visualisaties die zijn opgenomen.

De opslag service verzamelt metrische gegevens aan de hand van een beste poging, maar kan niet elke opslag bewerking opnemen.

In de Azure Portal kunt u metrische gegevens weer geven, zoals de beschik baarheid, het totaal aantal aanvragen en de gemiddelde latentie nummers van een opslag account. Er is ook een meldings regel ingesteld om een beheerder te waarschuwen als de beschik baarheid onder een bepaald niveau daalt. Voor het weer geven van deze gegevens is een mogelijk gebied voor onderzoek het percentage van het tabel service-succes dat onder 100% ligt (Zie de sectie[Metrische gegevens tonen lage PercentSuccess of analytics logboekvermeldingen bewerkingen hebben met de status van ClientOtherErrors]").

U moet uw Azure-toepassingen continu controleren om ervoor te zorgen dat ze in orde zijn en worden uitgevoerd zoals verwacht door:

* Het opstellen van bepaalde basislijn metrieken voor de toepassing waarmee u de huidige gegevens kunt vergelijken en eventuele belang rijke wijzigingen in het gedrag van Azure Storage en uw toepassing moet identificeren. De waarden van de metrische gegevens voor de basis lijn zijn in veel gevallen toepassings specifiek en u moet deze instellen wanneer u de prestaties van uw toepassing testen.
* De metrische gegevens over de minuut vastleggen en deze gebruiken om te controleren of deze actief zijn voor onverwachte fouten en afwijkingen, zoals pieken in het aantal fouten of aanvraag tarieven.
* Metrische gegevens per uur vastleggen en gebruiken om gemiddelde waarden te bewaken, zoals het gemiddelde aantal fouten en aanvraag tarieven.
* Het onderzoeken van mogelijke problemen met diagnostische hulpprogram ma's zoals verderop in de sectie ' problemen met de[Problemen met opslag vaststellen]' worden beschreven.

In de grafieken in de volgende afbeelding ziet u hoe het gemiddelde voor de metrische gegevens van een uur pieken in de activiteit kan verbergen. De metrische gegevens per uur worden weer gegeven om een constante frequentie van aanvragen weer te geven, terwijl de gegevens over de minuut de schommelingen onthullen die werkelijk plaatsvinden.

![][3]

In de rest van deze sectie wordt beschreven welke metrische gegevens u moet controleren en waarom.

### <a name="monitoring-service-health"></a>Service status controleren
U kunt de [Azure Portal](https://portal.azure.com) gebruiken om de status van de opslag service (en andere Azure-Services) in alle Azure-regio's over de hele wereld weer te geven. Met bewaking kunt u onmiddellijk zien of een probleem buiten uw besturings element de opslag service beïnvloedt in de regio die u voor uw toepassing gebruikt.

De [Azure Portal](https://portal.azure.com) kan ook meldingen over incidenten bieden die van invloed zijn op de verschillende Azure-Services.
Opmerking: Deze informatie was eerder beschikbaar, samen met historische gegevens, in het [Azure service-dash board](https://status.azure.com).

Terwijl de [Azure Portal](https://portal.azure.com) status gegevens verzamelt vanuit de Azure-Data Centers (interne bewaking), kunt u ook overwegen om een externe benadering te gebruiken voor het genereren van synthetische trans acties die regel matig toegang hebben tot uw door Azure gehoste website toepassing vanaf meerdere locaties. De services die worden aangeboden door [Dynatrace](https://www.dynatrace.com/en/synthetic-monitoring) en Application Insights voor Azure DevOps zijn voor beelden van deze benadering. Voor meer informatie over Application Insights voor Azure DevOps raadpleegt u de bijlage[bijlage 5: Bewaking met Application Insights voor Azure DevOps](#appendix-5). "

### <a name="monitoring-capacity"></a>Bewakings capaciteit
Met metrische gegevens voor opslag worden alleen metrische gegevens over capaciteit opgeslagen voor de BLOB-service, omdat blobs doorgaans het grootste deel van de opgeslagen data hebben (op het moment van schrijven, is het niet mogelijk metrische opslag waarden te gebruiken om de capaciteit van uw tabellen en wacht rijen te controleren). U kunt deze gegevens vinden in de tabel **$MetricsCapacityBlob** als u bewaking hebt ingeschakeld voor de BLOB service. Met metrische gegevens van de opslag records worden deze eenmalig per dag geregistreerd en kunt u de waarde van de **RowKey** gebruiken om te bepalen of de rij een entiteit bevat die is gekoppeld aan gebruikers gegevens (waardegegevens) of Analytics-gegevens **(Value** **Analytics**). Elke opgeslagen entiteit bevat informatie over de hoeveelheid gebruikte opslag ruimte (**capaciteit** gemeten in bytes) en het huidige aantal containers (**ContainerCount**) en blobs (**ObjectCount**) die worden gebruikt in het opslag account. Zie [Opslaganalyse-tabel schema metrische](https://msdn.microsoft.com/library/azure/hh343264.aspx)gegevens voor meer informatie over de capaciteits gegevens die zijn opgeslagen in de tabel **$MetricsCapacityBlob** .

> [!NOTE]
> U moet deze waarden controleren voor een vroegtijdige waarschuwing dat u de capaciteits limieten van uw opslag account nadert. U kunt in de Azure Portal waarschuwings regels toevoegen om u te waarschuwen als het gebruik van geaggregeerde opslag overschrijdt of lager is dan de drempel waarden die u opgeeft.
>
>

Zie het blog bericht [over Azure Storage facturering – band breedte, trans acties en capaciteit](https://blogs.msdn.com/b/windowsazurestorage/archive/2010/07/09/understanding-windows-azure-storage-billing-bandwidth-transactions-and-capacity.aspx)voor hulp bij het schatten van de grootte van verschillende opslag objecten, zoals blobs.

### <a name="monitoring-availability"></a>Beschik baarheid bewaken
U moet de beschik baarheid van de opslag Services in uw opslag account controleren door de waarde in de kolom **Beschik baarheid** in de metrische tabellen per uur of minuut te bewaken, **$MetricsHourPrimaryTransactionsBlob**, **$ MetricsHourPrimaryTransactionsTable**, **$MetricsHourPrimaryTransactionsQueue**, **$MetricsMinutePrimaryTransactionsBlob**, **$MetricsMinutePrimaryTransactionsTable**, **$MetricsMinutePrimaryTransactionsQueue** , **$MetricsCapacityBlob**. De kolom **Beschik baarheid** bevat een percentage waarde waarmee de beschik baarheid van de service of de API-bewerking wordt aangegeven die wordt vertegenwoordigd door de rij (de **RowKey** geeft aan of de rij metrische gegevens bevat voor de service als geheel of voor een specifieke API-bewerking) .

Een waarde van minder dan 100% geeft aan dat sommige opslag aanvragen mislukken. U kunt zien waarom ze mislukken door de andere kolommen in de metrische gegevens te onderzoeken die het aantal aanvragen weer geven met verschillende fout typen, zoals **ServerTimeoutError**. U verwacht dat de **Beschik baarheid** tijdelijk onder 100% wordt weer gegeven om redenen als tijdelijke time-outs bij de server terwijl de service partities verplaatst naar een betere Load-Balancing-aanvraag; de logica voor opnieuw proberen in uw client toepassing moet dergelijke periodieke omstandigheden afhandelen. Het artikel [Opslaganalyse vastgelegde bewerkingen en status berichten](https://msdn.microsoft.com/library/azure/hh343260.aspx) bevat een lijst met de transactie typen die door metrische gegevens voor opslag zijn opgenomen in de **beschikbaarheids** berekening.

In de [Azure Portal](https://portal.azure.com)kunt u waarschuwings regels toevoegen om u te waarschuwen als **Beschik baarheid** voor een service onder een drempel waarde valt die u opgeeft.

In het gedeelte "[Hulp bij het oplossen van problemen]" van deze hand leiding worden enkele veelvoorkomende problemen met de opslag service met betrekking tot Beschik baarheid beschreven.

### <a name="monitoring-performance"></a>Prestaties bewaken
Voor het bewaken van de prestaties van de opslag Services, kunt u de volgende metrische gegevens uit de metrische tabellen per uur en minuut gebruiken.

* De waarden in de kolommen **AverageE2ELatency** en **averageserverlatency aan** tonen de gemiddelde tijd dat het bewerkings type van de opslag service of de API bezig is met het verwerken van aanvragen. **AverageE2ELatency** is een maat eenheid voor de end-to-end-latentie, inclusief de tijd die nodig is om de aanvraag te lezen en het antwoord te verzenden, naast de tijd die nodig is om de aanvraag te verwerken (dit omvat ook netwerk latentie zodra de aanvraag de opslag bereikt Service); **Averageserverlatency aan** is een meting van alleen de verwerkings tijd en sluit daarom elke netwerk latentie uit die betrekking heeft op de communicatie met de client. Zie de sectie "[Prestatiegegevens geven hoge AverageE2ELatency en lage AverageServerLatency aan]" verderop in deze hand leiding voor een bespreking van de redenen waarom er sprake is van een belang rijk verschil tussen deze twee waarden.
* De waarden in de kolommen **TotalIngress** en **TotalEgress** tonen de totale hoeveelheid gegevens, in bytes, die afkomstig zijn van uw opslag service of via een specifiek API-bewerkings type.
* De waarden in de kolom **TotalRequests** tonen het totaal aantal aanvragen dat de opslag service van de API-bewerking ontvangt. **TotalRequests** is het totale aantal aanvragen dat door de opslag service wordt ontvangen.

Normaal gesp roken controleert u op onverwachte wijzigingen in een van deze waarden als een indicator waarvoor u een probleem hebt waarvoor onderzoek vereist is.

In de [Azure Portal](https://portal.azure.com)kunt u waarschuwings regels toevoegen om u te waarschuwen als een van de prestatie gegevens voor deze service lager of hoger is dan de drempel die u opgeeft.

In het gedeelte "[Hulp bij het oplossen van problemen]" in deze hand leiding worden enkele veelvoorkomende problemen met de opslag service met betrekking tot prestaties beschreven.

## <a name="diagnosing-storage-issues"></a>Problemen met opslag vaststellen
Er zijn een aantal manieren waarop u op de hoogte kan worden gesteld van een probleem of probleem in uw toepassing, waaronder:

* Een grote fout die ervoor zorgt dat de toepassing vastloopt of niet meer werkt.
* Belang rijke wijzigingen ten opzichte van basislijn waarden in de metrische gegevens die u bewaken, zoals beschreven in de vorige sectie '[uw opslag service controleren]'.
* Rapporten van gebruikers van uw toepassing dat een bepaalde bewerking niet is voltooid zoals verwacht, of dat sommige functies niet werken.
* Fouten die in de toepassing worden gegenereerd en die worden weer gegeven in de logboek bestanden of via een andere meldings methode.

Problemen met betrekking tot Azure Storage-services vallen doorgaans onder een van de volgende vier categorieën:

* Uw toepassing heeft een prestatie probleem, dat door uw gebruikers wordt gerapporteerd of door wijzigingen in de metrische gegevens over prestaties heeft gedetecteerd.
* Er is een probleem met de Azure Storage-infra structuur in een of meer regio's.
* Er is een fout opgetreden in uw toepassing, die door uw gebruikers is gerapporteerd of die wordt weer gegeven door een toename in een van de metrische fouten die u bewaken.
* Tijdens het ontwikkelen en testen gebruikt u mogelijk de lokale opslag emulator. Er kunnen problemen optreden die specifiek betrekking hebben op het gebruik van de opslag emulator.

In de volgende secties vindt u een overzicht van de stappen die u moet volgen om problemen in elk van deze vier categorieën op te sporen en op te lossen. In de sectie '[Hulp bij het oplossen van problemen]' verderop in deze hand leiding vindt u meer informatie over enkele veelvoorkomende problemen die zich kunnen voordoen.

### <a name="service-health-issues"></a>Service status problemen
Problemen met de service status vallen doorgaans buiten uw besturings systeem. De [Azure Portal](https://portal.azure.com) biedt informatie over lopende problemen met Azure-Services, waaronder opslag Services. Als u hebt gekozen voor geografisch redundante opslag met lees toegang tijdens het maken van uw opslag account, dan kan uw toepassing tijdelijk overschakelen naar de kopie alleen-lezen op de secundaire locatie als uw gegevens niet meer beschikbaar zijn op de primaire locatie. Om van de secundaire te lezen, moet uw toepassing kunnen overschakelen tussen de primaire en secundaire opslag locatie en kunnen werken in een modus met beperkte functionaliteit met alleen-lezen gegevens. Met de Azure Storage-client bibliotheken kunt u een beleid voor opnieuw proberen definiëren dat kan worden gelezen uit de secundaire opslag als het lezen van de primaire opslag mislukt. Uw toepassing moet er ook rekening mee houden dat de gegevens op de secundaire locatie uiteindelijk consistent zijn. Zie voor meer informatie het blog bericht [Azure Storage redundantie opties en lees toegang geografisch redundante opslag](https://blogs.msdn.microsoft.com/windowsazurestorage/2013/12/11/windows-azure-storage-redundancy-options-and-read-access-geo-redundant-storage/).

### <a name="performance-issues"></a>Prestatie problemen
De prestaties van een toepassing kunnen subjectief zijn, met name vanuit het perspectief van een gebruiker. Het is daarom belangrijk dat u over prestatiegegevens voor een basislijn beschikt aan de hand waarvan u kunt bepalen waar er prestatieproblemen zijn. Veel factoren kunnen van invloed zijn op de prestaties van een Azure Storage-service vanuit het perspectief van de client toepassing. Deze factoren kunnen worden gebruikt in de Storage-service, in de-client of in de netwerk infrastructuur; Daarom is het belang rijk dat u een strategie hebt voor het identificeren van de oorsprong van het prestatie probleem.

Nadat u de waarschijnlijke locatie van de oorzaak van het prestatie probleem van de metrieken hebt vastgesteld, kunt u de logboek bestanden gebruiken om gedetailleerde informatie te vinden en het probleem op te lossen.

In de sectie '[Hulp bij het oplossen van problemen]' verderop in deze hand leiding vindt u meer informatie over enkele veelvoorkomende prestatie problemen die u kunt tegen komen.

### <a name="diagnosing-errors"></a>Fouten vaststellen
Gebruikers van uw toepassing kunnen u een melding geven over fouten die zijn gerapporteerd door de client toepassing. Metrische opslag gegevens registreren ook het aantal verschillende fout typen van uw opslag Services, zoals **NetworkError**, **ClientTimeoutError**of **AuthorizationError**. Hoewel de metrische gegevens van de opslag slechts een aantal verschillende fout typen hebben, kunt u meer details over afzonderlijke aanvragen verkrijgen door de server-, client-en netwerk logboeken te controleren. Normaal gesp roken geeft de HTTP-status code die wordt geretourneerd door de opslag service een indicatie waarom de aanvraag is mislukt.

> [!NOTE]
> Houd er rekening mee dat u verwacht een aantal periodieke fouten te zien: bijvoorbeeld fouten als gevolg van tijdelijke netwerk problemen of toepassings fouten.
>
>

De volgende resources kunnen worden gebruikt voor een overzicht van status- en foutcodes in verband met de opslag:

* [Veelvoorkomende fout codes voor REST API](https://msdn.microsoft.com/library/azure/dd179357.aspx)
* [Foutcodes voor blob-services](https://msdn.microsoft.com/library/azure/dd179439.aspx)
* [Fout codes voor Queue-service](https://msdn.microsoft.com/library/azure/dd179446.aspx)
* [Fout codes voor tabel Services](https://msdn.microsoft.com/library/azure/dd179438.aspx)
* [Fout codes voor bestands Services](https://msdn.microsoft.com/library/azure/dn690119.aspx)

### <a name="storage-emulator-issues"></a>Problemen met de opslag emulator
De Azure SDK bevat een opslag emulator die u kunt uitvoeren op een ontwikkel werkstation. Deze emulator simuleert het grootste deel van het gedrag van de Azure Storage-services en is nuttig tijdens het ontwikkelen en testen, zodat u toepassingen kunt uitvoeren die gebruikmaken van Azure Storage-services zonder dat u een Azure-abonnement en een Azure-opslag account nodig hebt.

In het gedeelte "[Hulp bij het oplossen van problemen]" van deze hand leiding worden enkele veelvoorkomende problemen beschreven die zich voordoen met de opslag emulator.

### <a name="storage-logging-tools"></a>Hulpprogram ma's voor opslag logboek registratie
Opslag logboek registratie biedt logboek registratie aan de server zijde van opslag aanvragen in uw Azure Storage-account. Zie [opslag logboeken inschakelen en logboek gegevens openen](https://go.microsoft.com/fwlink/?LinkId=510867)voor meer informatie over het inschakelen van logboek registratie aan de server zijde en het openen van logboek gegevens.

Met de Storage-client bibliotheek voor .NET kunt u logboek gegevens aan de client zijde verzamelen die betrekking hebben op opslag bewerkingen die worden uitgevoerd door uw toepassing. Zie [Logboekregistratie op de client inschakelen met de .NET-opslagclientbibliotheek](https://go.microsoft.com/fwlink/?LinkId=510868) voor meer informatie.

> [!NOTE]
> In sommige gevallen (zoals SAS-autorisatie fouten), kan een gebruiker een fout melden waarvoor u geen aanvraag gegevens in de opslag logboeken aan de server zijde kunt vinden. U kunt de logboek functies van de Storage-client bibliotheek gebruiken om te onderzoeken of de oorzaak van het probleem zich op de client bevindt of netwerk controle hulpprogramma's gebruiken om het netwerk te onderzoeken.
>
>

### <a name="using-network-logging-tools"></a>Hulpprogram ma's voor netwerk logboek registratie gebruiken
U kunt het verkeer tussen de client en de server vastleggen om gedetailleerde informatie te geven over de gegevens die door de client en de server worden uitgewisseld en de onderliggende netwerk omstandigheden. Nuttige hulpprogram ma's voor logboek registratie van netwerken zijn onder andere:

* [Fiddler](https://www.telerik.com/fiddler) is een gratis proxy voor webfoutopsporing waarmee u de kopteksten en payload-gegevens van http-en HTTPS-aanvragen en-antwoord berichten kunt onderzoeken. Zie [voor meer informatie bijlage 1: Fiddler gebruiken voor het vastleggen van HTTP-](#appendix-1)en HTTPS-verkeer.
* [Microsoft Network Monitor (netmon)](https://www.microsoft.com/download/details.aspx?id=4865) en [wireshark](https://www.wireshark.org/) zijn gratis netwerkprotocol analyse functies waarmee u gedetailleerde pakket gegevens kunt weer geven voor een breed scala aan netwerk protocollen. Zie voor meer informatie over wireshark "[bijlage 2: Wireshark gebruiken om netwerk verkeer](#appendix-2)vast te leggen.
* Micro soft Message Analyzer is een hulp programma van micro soft dat netmon vervangt en dat naast het vastleggen van netwerk pakket gegevens, u helpt de logboek gegevens weer te geven en te analyseren die zijn opgenomen in andere hulpprogram ma's. Zie voor meer informatie "[bijlage 3: Micro soft Message Analyzer gebruiken om netwerk verkeer](#appendix-3)vast te leggen.
* Als u een basis connectiviteits test wilt uitvoeren om te controleren of uw client computer via het netwerk verbinding kan maken met de Azure Storage-service, kunt u dit niet doen met het hulp programma standaard **ping** op de client. U kunt echter het [ **tcping** -hulp programma](https://www.elifulkerson.com/projects/tcping.php) gebruiken om de connectiviteit te controleren.

In veel gevallen zijn de logboek gegevens van opslag logboeken en de opslag-client bibliotheek voldoende om een probleem op te sporen, maar in sommige gevallen hebt u mogelijk de gedetailleerde informatie nodig die deze hulpprogram ma's voor netwerk logboek registratie kunnen bieden. Als u bijvoorbeeld Fiddler gebruikt om HTTP-en HTTPS-berichten weer te geven, kunt u de header-en payload-gegevens weer geven die zijn verzonden naar en van de opslag Services, zodat u kunt onderzoeken hoe een client toepassing de opslag bewerkingen opnieuw probeert uit te voeren. Protocol analyse functies zoals wireshark worden uitgevoerd op pakket niveau, zodat u TCP-gegevens kunt weer geven, waarmee u problemen met verloren pakketten en verbindings problemen kunt oplossen. Message Analyzer kan worden gebruikt op HTTP-en TCP-lagen.

## <a name="end-to-end-tracing"></a>End-to-end tracering
End-to-end tracering met behulp van een groot aantal logboek bestanden is een handige techniek voor het onderzoeken van potentiële problemen. U kunt de datum-en tijd gegevens uit uw metrische gegevens gebruiken als aanwijzing voor het zoeken naar de logboek bestanden voor gedetailleerde informatie die u helpt bij het oplossen van het probleem.

### <a name="correlating-log-data"></a>Gerelateerde logboek gegevens
Bij het weer geven van logboeken van client toepassingen, netwerk traceringen en logboek registratie aan de server zijde is het essentieel om aanvragen in de verschillende logboek bestanden te correleren. De logboek bestanden bevatten een aantal verschillende velden die handig zijn als correlatie-id's. De aanvraag-ID van de client is het meest nuttige veld dat wordt gebruikt voor het correleren van vermeldingen in de verschillende logboeken. Soms kan het nuttig zijn om ofwel de server aanvraag-ID of tijds tempels te gebruiken. In de volgende secties vindt u meer informatie over deze opties.

### <a name="client-request-id"></a>Client aanvraag-ID
De Storage-client bibliotheek genereert automatisch een unieke client aanvraag-ID voor elke aanvraag.

* In het client-side-logboek dat door de Storage-client bibliotheek wordt gemaakt, wordt de client aanvraag-ID weer gegeven in het veld **client aanvraag-id** van elk logboek vermelding met betrekking tot de aanvraag.
* In een netwerk tracering, zoals een vastgelegd door Fiddler, is de aanvraag-ID van de client zichtbaar in aanvraag berichten als de **x-MS-Client-Request-id** http-header waarde.
* De aanvraag-ID van de client wordt weer gegeven in de kolom client aanvraag-ID in het logboek voor logboek registratie aan de server zijde.

> [!NOTE]
> Het is mogelijk dat meerdere aanvragen dezelfde client aanvraag-ID delen, omdat de client deze waarde kan toewijzen (hoewel de opslag-client bibliotheek automatisch een nieuwe waarde toewijst). Wanneer de client opnieuw probeert, delen alle pogingen dezelfde client aanvraag-ID. In het geval van een batch die van de client is verzonden, heeft de batch één aanvraag-ID van de client.
>
>

### <a name="server-request-id"></a>Server aanvraag-ID
De opslag service genereert automatisch server aanvraag-Id's.

* In het logboek logboek registratie voor opslag op de server wordt de server aanvraag-ID weer gegeven in de kolom **aanvraag-ID-header** .
* In een netwerk tracering, zoals een vastgelegd door Fiddler, wordt de server aanvraag-ID weer gegeven in antwoord berichten als de **x-MS-Request-id** http-header waarde.
* In het logboek aan de client zijde die door de opslag-client bibliotheek wordt gemaakt, wordt de server aanvraag-ID weer gegeven in de kolom **bewerking tekst** voor de logboek vermelding met details van de reactie van de server.

> [!NOTE]
> De opslag service wijst altijd een unieke server aanvraag-ID toe aan elke aanvraag die wordt ontvangen, zodat elke nieuwe poging van de client en elke bewerking die in een batch is opgenomen, een unieke server aanvraag-ID heeft.
>
>

Als de opslag-client bibliotheek een **StorageException** in de client genereert, bevat de eigenschap **RequestInformation** een **RequestResult** -object met daarin een **ServiceRequestID** -eigenschap. U kunt ook toegang krijgen tot een **RequestResult** -object vanuit een **OperationContext** -exemplaar.

In het volgende code voorbeeld ziet u hoe u een aangepaste **ClientRequestId** -waarde instelt door een **OperationContext** -object te koppelen aan de opslag service. Ook wordt uitgelegd hoe u de **ServerRequestId** -waarde ophaalt uit het antwoord bericht.

```csharp
//Parse the connection string for the storage account.
const string ConnectionString = "DefaultEndpointsProtocol=https;AccountName=account-name;AccountKey=account-key";
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(ConnectionString);
CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

// Create an Operation Context that includes custom ClientRequestId string based on constants defined within the application along with a Guid.
OperationContext oc = new OperationContext();
oc.ClientRequestID = String.Format("{0} {1} {2} {3}", HOSTNAME, APPNAME, USERID, Guid.NewGuid().ToString());

try
{
    CloudBlobContainer container = blobClient.GetContainerReference("democontainer");
    ICloudBlob blob = container.GetBlobReferenceFromServer("testImage.jpg", null, null, oc);  
    var downloadToPath = string.Format("./{0}", blob.Name);
    using (var fs = File.OpenWrite(downloadToPath))
    {
        blob.DownloadToStream(fs, null, null, oc);
        Console.WriteLine("\t Blob downloaded to file: {0}", downloadToPath);
    }
}
catch (StorageException storageException)
{
    Console.WriteLine("Storage exception {0} occurred", storageException.Message);
    // Multiple results may exist due to client side retry logic - each retried operation will have a unique ServiceRequestId
    foreach (var result in oc.RequestResults)
    {
            Console.WriteLine("HttpStatus: {0}, ServiceRequestId {1}", result.HttpStatusCode, result.ServiceRequestID);
    }
}
```

### <a name="timestamps"></a>Tijds tempels
U kunt ook tijds tempels gebruiken om gerelateerde logboek vermeldingen te vinden, maar wees voorzichtig met de klok scheefheid tussen de client en de server die mogelijk bestaat. Zoek plus of min 15 minuten voor overeenkomende vermeldingen aan de server zijde op basis van de tijds tempel van de client. Houd er rekening mee dat de BLOB-meta gegevens voor de blobs die meet waarden bevatten, het tijds bereik aangeven voor metrische gegevens die zijn opgeslagen in de blob. Dit tijds bereik is handig als u veel metrische blobs voor dezelfde minuut of uur hebt.

## <a name="troubleshooting-guidance"></a>Richt lijnen voor probleem oplossing
Deze sectie helpt u bij het diagnosticeren en oplossen van problemen met enkele veelvoorkomende problemen die uw toepassing kan tegen komen wanneer u de Azure Storage-services gebruikt. Gebruik de onderstaande lijst om de informatie te vinden die relevant is voor uw specifieke probleem.

**Beslissings structuur oplossen**

---
Heeft uw probleem betrekking op de prestaties van een van de opslag Services?

* [Prestatiegegevens geven hoge AverageE2ELatency en lage AverageServerLatency aan]
* [Prestatiegegevens geven lage AverageE2ELatency en lage AverageServerLatency aan, maar de client ondervindt hoge latentie]
* [Prestatiegegevens geven hoge AverageServerLatency aan]
* [U ervaart onverwachte vertragingen bij de levering van berichten in een wachtrij]

---
Is uw probleem gerelateerd aan de beschik baarheid van een van de opslag Services?

* [Prestatiegegevens geven een toename in PercentThrottlingError aan]
* [Prestatiegegevens geven een toename in PercentTimeoutError aan]
* [Prestatiegegevens geven een toename in PercentNetworkError aan]

---
 Ontvangt uw client toepassing een reactie van het HTTP-4XX (zoals 404) van een opslag service?

* [De client ontvangt HTTP 403-meldingen (verboden)]
* [De client ontvangt HTTP 404-meldingen (niet gevonden)]
* [De client ontvangt berichten van de HTTP-409 (Conflict)]

---
[Metrische gegevens tonen lage PercentSuccess of analytics logboekvermeldingen bewerkingen hebben met de status van ClientOtherErrors]

---
[Capaciteit metrische gegevens tonen een onverwachte toename in opslaggebruik capaciteit]

---
[U ondervindt onverwachte opnieuw opstarten van Virtual Machines met een groot aantal gekoppelde Vhd's]

---
[Het probleem zich voordoet de opslagemulator voor ontwikkeling of tests gebruiken]

---
[U ondervindt problemen met het installeren van de Azure SDK voor .NET]

---
[U hebt een ander probleem met storage-service]

---
### <a name="metrics-show-high-AverageE2ELatency-and-low-AverageServerLatency"></a>Met metrische gegevens worden hoge AverageE2ELatency en lage Averageserverlatency aan weer gegeven
In de onderstaande afbeelding van het hulp programma [Azure Portal](https://portal.azure.com) bewaking ziet u een voor beeld waarin de **AverageE2ELatency** aanzienlijk hoger is dan de **averageserverlatency aan**.

![][4]

De opslag service berekent alleen de metrische **AverageE2ELatency** voor voltooide aanvragen en, in tegens telling tot **averageserverlatency aan**, omvat de tijd die de client nodig heeft om de gegevens te verzenden en bevestiging van de opslag service te ontvangen. Daarom kan een verschil tussen **AverageE2ELatency** en **averageserverlatency aan** worden veroorzaakt doordat de client toepassing langzaam reageert of als gevolg van de omstandigheden in het netwerk.

> [!NOTE]
> U kunt ook **E2ELatency** en **ServerLatency** weer geven voor afzonderlijke opslag bewerkingen in de logboek gegevens van de opslag logboek registratie.
>
>

#### <a name="investigating-client-performance-issues"></a>Problemen met client prestaties onderzoeken
Mogelijke oorzaken voor de client lopen langzaam, zoals een beperkt aantal beschik bare verbindingen of threads, of er is onvoldoende bronnen beschikbaar, zoals CPU, geheugen of netwerk bandbreedte. U kunt het probleem mogelijk oplossen door de client code efficiënter te wijzigen (bijvoorbeeld door asynchrone aanroepen naar de opslag service te gebruiken) of door een grotere virtuele machine te gebruiken (met meer kernen en meer geheugen).

Voor de tabel-en wachtrij Services kan het Nagle-algoritme ook hoge **AverageE2ELatency** veroorzaken ten opzichte van **averageserverlatency aan**: Zie het [algoritme van post Nagle is niet geschikt voor kleine aanvragen](https://blogs.msdn.com/b/windowsazurestorage/archive/2010/06/25/nagle-s-algorithm-is-not-friendly-towards-small-requests.aspx)voor meer informatie. U kunt de Nagle-algoritme in code uitschakelen met behulp van de klasse **ServicePointManager** in de naam ruimte **System.net** . U moet dit doen voordat u aanroepen naar de tabel-of wachtrij Services in uw toepassing, omdat dit geen invloed heeft op verbindingen die al zijn geopend. Het volgende voor beeld wordt opgehaald uit de **Application_Start** -methode in een worker-rol.

```csharp
var storageAccount = CloudStorageAccount.Parse(connStr);
ServicePoint tableServicePoint = ServicePointManager.FindServicePoint(storageAccount.TableEndpoint);
tableServicePoint.UseNagleAlgorithm = false;
ServicePoint queueServicePoint = ServicePointManager.FindServicePoint(storageAccount.QueueEndpoint);
queueServicePoint.UseNagleAlgorithm = false;
```

Controleer de logboeken aan de client om te zien hoeveel aanvragen uw client toepassing verzendt en controleer op algemene .NET-gerelateerde prestatie knelpunten in uw client zoals CPU, .NET-garbagecollection, netwerk gebruik of geheugen. Als uitgangs punt voor het oplossen van problemen met .NET-client toepassingen raadpleegt u [fout opsporing, tracering en profile ring](https://msdn.microsoft.com/library/7fe0dd2y).

#### <a name="investigating-network-latency-issues"></a>Problemen met netwerk latentie onderzoeken
Een hoge end-to-end latentie als gevolg van het netwerk wordt meestal veroorzaakt door tijdelijke omstandigheden. U kunt zowel tijdelijke als permanente netwerk problemen, zoals verwijderde pakketten, onderzoeken door gebruik te maken van hulpprogram ma's zoals wireshark of micro soft Message Analyzer.

Zie voor meer informatie over het gebruik van wireshark om netwerk problemen op[Bijlage 2: Wireshark gebruiken om netwerk verkeer vast te leggen]vast te leggen. "

Zie voor meer informatie over het gebruik van micro soft Message Analyzer om netwerk problemen[Bijlage 3: Micro soft Message Analyzer gebruiken om netwerk verkeer vast te leggen]vast te leggen.

### <a name="metrics-show-low-AverageE2ELatency-and-low-AverageServerLatency"></a>Metrische gegevens tonen een laag AverageE2ELatency en lage Averageserverlatency aan, maar de client ondervindt een hoge latentie
In dit scenario is de waarschijnlijke oorzaak een vertraging in de opslag aanvragen die de opslag service bereiken. U moet onderzoeken waarom aanvragen van de client niet worden door middel van de BLOB-service.

Een mogelijke reden voor de client vertraging bij het verzenden van aanvragen is dat er een beperkt aantal beschik bare verbindingen of threads is.

Controleer ook of de client meerdere nieuwe pogingen uitvoert en onderzoek de reden als dat het geval is. Als u wilt bepalen of de client meerdere nieuwe pogingen uitvoert, kunt u het volgende doen:

* Controleer de Opslaganalyse-Logboeken. Als er meerdere pogingen plaatsvinden, worden er meerdere bewerkingen met dezelfde client aanvraag-ID weer geven, maar met verschillende server aanvraag-Id's.
* Controleer de client Logboeken. Uitgebreide logboek registratie geeft aan dat er een nieuwe poging is gedaan.
* Spoor fouten op in uw code en controleer de eigenschappen van het **OperationContext** -object dat is gekoppeld aan de aanvraag. Als de bewerking opnieuw wordt geprobeerd, bevat de eigenschap **RequestResults** meerdere unieke server aanvraag-id's. U kunt ook de begin-en eind tijden voor elke aanvraag controleren. Zie het code voorbeeld in de sectie [server aanvraag-id]voor meer informatie.

Als de client geen problemen bevat, moet u mogelijke netwerk problemen, zoals pakket verlies, onderzoeken. U kunt hulpprogram ma's zoals wireshark of micro soft Message Analyzer gebruiken om netwerk problemen te onderzoeken.

Zie voor meer informatie over het gebruik van wireshark om netwerk problemen op[Bijlage 2: Wireshark gebruiken om netwerk verkeer vast te leggen]vast te leggen. "

Zie voor meer informatie over het gebruik van micro soft Message Analyzer om netwerk problemen[Bijlage 3: Micro soft Message Analyzer gebruiken om netwerk verkeer vast te leggen]vast te leggen.

### <a name="metrics-show-high-AverageServerLatency"></a>Metrische gegevens tonen high-Averageserverlatency aan
In het geval van hoge **averageserverlatency aan** voor aanvragen voor het downloaden van blobs, moet u de logboeken voor logboek registratie gebruiken om te zien of er herhaalde aanvragen voor dezelfde BLOB (of set blobs) zijn. Voor BLOB-upload aanvragen moet u onderzoeken welke blok grootte door de client wordt gebruikt (bijvoorbeeld blokken die kleiner zijn dan 64 K in grootte kan leiden tot overhead, tenzij de Lees bewerkingen ook in minder dan 64 K-segmenten staan) en als meerdere clients upload blokken naar dezelfde Blob in de alinea worden geüpload. llel. U moet ook de metrische gegevens per minuut voor pieken controleren in het aantal aanvragen dat resulteert in het overschrijden van de schaal baarheids doelen van de per seconde. Zie ook '[Prestatiegegevens geven een toename in PercentTimeoutError aan]'.

Als er hoge **averageserverlatency aan** voor het downloaden van blobs worden weer gegeven wanneer er herhaalde aanvragen dezelfde BLOB of set blobs zijn, kunt u overwegen om deze blobs in de cache op te slaan met behulp van Azure cache of Azure Content Delivery Network (CDN). Voor upload aanvragen kunt u de door Voer verbeteren door een grotere blok grootte te gebruiken. Voor query's naar tabellen is het ook mogelijk caching aan de client zijde te implementeren op clients die dezelfde query bewerkingen uitvoeren en waar de gegevens niet regel matig worden gewijzigd.

High- **averageserverlatency aan** -waarden kunnen ook een symptoom zijn van slecht ontworpen tabellen of query's die resulteren in scan bewerkingen of die het Anti-patroon Append/laten voorafgaan door volgen. Zie "[Prestatiegegevens geven een toename in PercentThrottlingError aan]" voor meer informatie.

> [!NOTE]
> U vindt hier een uitgebreid overzicht van de controle lijst prestaties: [Microsoft Azure Storage controle lijst voor prestaties en schaal baarheid](storage-performance-checklist.md).
>
>

### <a name="you-are-experiencing-unexpected-delays-in-message-delivery"></a>U ondervindt onverwachte vertragingen bij de levering van berichten in een wachtrij
Als u een vertraging ondervindt tussen het moment dat een toepassing een bericht aan een wachtrij toevoegt en de tijd die beschikbaar is om te worden gelezen uit de wachtrij, moet u de volgende stappen uitvoeren om het probleem op te sporen:

* Controleer of de toepassing de berichten aan de wachtrij heeft toegevoegd. Controleer of de toepassing niet enkele keren opnieuw wordt geprobeerd de **AddMessage** -methode voordat de bewerking is voltooid. In de logboeken van de opslag-client bibliotheek worden herhaalde pogingen van opslag bewerkingen weer gegeven.
* Controleer of er geen klok verschil is tussen de werknemersrol waarmee het bericht wordt toegevoegd aan de wachtrij en de worker-rol die het bericht uit de wachtrij leest, waardoor het lijkt alsof er een vertraging optreedt in de verwerking.
* Controleer of de werk rollen die de berichten van de wachtrij leest, niet werkt. Als een wachtrij-client de methode **GetMessage** aanroept maar niet met een bevestiging reageert, blijft het bericht onzichtbaar in de wachtrij totdat de **invisibilityTimeout** -periode is verlopen. Op dit moment wordt het bericht opnieuw beschikbaar voor verwerking.
* Controleer of de lengte van de wachtrij in de loop van de tijd toeneemt. Dit kan gebeuren als er onvoldoende werk rollen beschikbaar zijn om alle berichten te verwerken die andere werk nemers in de wachtrij plaatsen. Controleer ook de metrische gegevens om te zien of er een fout is opgetreden bij het verwijderen van aanvragen en de verwijdering van de wachtrij op berichten. Dit kan duiden op herhaalde mislukte pogingen om het bericht te verwijderen.
* Bekijk de logboeken voor opslag logboeken voor alle wachtrij bewerkingen die gedurende een langere periode meer dan de verwachte **E2ELatency** -en **ServerLatency** -waarden hebben dan normaal.

### <a name="metrics-show-an-increase-in-PercentThrottlingError"></a>Met metrische gegevens wordt een toename in Percentthrottlingerror aan weer gegeven
Beperkings fouten treden op wanneer u de schaalbaarheids doelen van een opslag service overschrijdt. De opslag service beperkt om ervoor te zorgen dat geen enkele client of Tenant de service bij de kosten van anderen kan gebruiken. Zie [Azure Storage schaalbaarheids-en prestatie doelen](storage-scalability-targets.md) voor meer informatie over de schaalbaarheids doelen voor opslag accounts en prestatie doelen voor partities binnen opslag accounts.

Als de **percentthrottlingerror aan** -metriek een toename weergeeft van het percentage aanvragen dat mislukt met een beperkings fout, moet u een van de volgende twee scenario's onderzoeken:

* [Tijdelijke toename in Percentthrottlingerror aan]
* [Permanente toename van de Percentthrottlingerror aan-fout]

Een toename in **percentthrottlingerror aan** treedt vaak op op hetzelfde moment als een toename van het aantal opslag aanvragen of wanneer u in eerste instantie de test uitvoert voor uw toepassing. Dit kan ook in de client als ' 503 Server bezet ' of ' time-out van 500-bewerking ' HTTP-status berichten van opslag bewerkingen zijn.

#### <a name="transient-increase-in-PercentThrottlingError"></a>Tijdelijke toename in Percentthrottlingerror aan
Als er pieken in de waarde van **percentthrottlingerror aan** worden weer gegeven die samen vallen met peri Oden van hoge activiteiten voor de toepassing, implementeert u een exponentiële (niet-lineaire) back-upstrategie voor nieuwe pogingen in uw client. Bij nieuwe back-ups wordt de directe belasting van de partitie verminderd en kan uw toepassing de pieken in het verkeer vloeiend maken. Zie de [micro soft. Azure. storage. RetryPolicies-naam ruimte](/dotnet/api/microsoft.azure.storage.retrypolicies)voor meer informatie over het implementeren van beleid voor opnieuw proberen met behulp van de Storage-client bibliotheek.

> [!NOTE]
> U ziet mogelijk ook pieken in de waarde van **percentthrottlingerror aan** die niet samen vallen met peri Oden met hoge activiteit voor de toepassing: de meest waarschijnlijke oorzaak is dat de opslag service partities verplaatst om de taak verdeling te verbeteren.
>
>

#### <a name="permanent-increase-in-PercentThrottlingError"></a>Permanente toename van de Percentthrottlingerror aan-fout
Als er een consistente hoge waarde wordt weer gegeven voor **percentthrottlingerror aan** na een permanente toename van uw transactie volumes, of wanneer u de initiële laad tests uitvoert voor uw toepassing, moet u evalueren hoe uw toepassing is opslag partities gebruiken en of het de schaalbaarheids doelen voor een opslag account nadert. Als er bijvoorbeeld vertragings fouten in een wachtrij worden weer gegeven (die als één partitie tellen), kunt u overwegen extra wacht rijen te gebruiken om de trans acties over meerdere partities te verdelen. Als er vertragings fouten in een tabel worden weer gegeven, moet u overwegen een ander partitie schema te gebruiken om uw trans acties over meerdere partities te verdelen met behulp van een groter aantal partitie sleutel waarden. Een veelvoorkomende oorzaak van dit probleem is het anti-laten voorafgaan door/toevoegen van een locatie waar u de datum als de partitie sleutel selecteert en vervolgens alle gegevens op een bepaalde dag naar één partitie worden geschreven: onder belasting, kan dit leiden tot een schrijf knelpunt. Overweeg een ander gepartitioneerd ontwerp of Bepaal of het gebruik van Blob Storage een betere oplossing kan zijn. Controleer ook of er bandbreedte beperking optreedt als gevolg van pieken in uw verkeer en onderzoek manieren om uw patroon van aanvragen te versoepelen.

Als u uw trans acties over meerdere partities distribueert, moet u nog steeds op de hoogte zijn van de schaalbaarheids limieten die zijn ingesteld voor het opslag account. Als u bijvoorbeeld tien wacht rijen hebt gebruikt elke verwerking van het maximum van 2.000 1 KB berichten per seconde, hebt u de algemene limiet van 20.000 berichten per seconde voor het opslag account. Als u meer dan 20.000 entiteiten per seconde wilt verwerken, kunt u overwegen meerdere opslag accounts te gebruiken. Het is ook belang rijk dat de grootte van uw aanvragen en entiteiten gevolgen heeft wanneer de opslag service uw clients beperkt: als u grotere aanvragen en entiteiten hebt, is het wellicht eerder beperkt.

Het inefficiënte query ontwerp kan er ook voor zorgen dat u de schaalbaarheids limieten voor tabel partities bereikt. Een query met een filter dat bijvoorbeeld slechts één procent van de entiteiten in een partitie selecteert, maar die alle entiteiten in een partitie scant, heeft toegang tot elke entiteit nodig. Voor elke entiteit die wordt gelezen, wordt het totale aantal trans acties in die partitie geteld. Daarom kunt u de schaalbaarheids doelen gemakkelijk bereiken.

> [!NOTE]
> Bij het testen van de prestaties moeten inefficiënte query ontwerpen worden weer geven in uw toepassing.
>
>

### <a name="metrics-show-an-increase-in-PercentTimeoutError"></a>Met metrische gegevens wordt een toename in Percenttimeouterror aan weer gegeven
Uw metrische gegevens tonen een toename in **percenttimeouterror aan** voor een van uw opslag Services. Tegelijkertijd ontvangt de client een groot aantal ' 500 bewerkings time-out ' HTTP-status berichten van opslag bewerkingen.

> [!NOTE]
> Mogelijk worden er tijdelijk time-outfouten weer geven, omdat aanvragen door de opslag service worden gebalanceerd door een partitie naar een nieuwe server te verplaatsen.
>
>

De metrische waarde voor **percenttimeouterror aan** is een aggregatie van de volgende metrische gegevens: **ClientTimeoutError**, **AnonymousClientTimeoutError**, **SASClientTimeoutError**, **ServerTimeoutError**, **AnonymousServerTimeoutError**en **SASServerTimeoutError**.

De time-outs van de server worden veroorzaakt door een fout op de server. De time-out van de client treedt op omdat een bewerking op de server de time-out overschrijdt die is opgegeven door de client. een client die de Storage-client bibliotheek gebruikt, kan bijvoorbeeld een time-out voor een bewerking instellen met behulp van de eigenschap **ServerTimeout** van de klasse **QueueRequestOptions** .

Server-time-outs duiden op een probleem met de opslag service die verder moet worden onderzocht. U kunt metrische gegevens gebruiken om te zien of u de schaalbaarheids limieten voor de service krijgt en eventuele pieken in het verkeer die dit probleem veroorzaken, kunnen identificeren. Als het probleem zich voordoet, kan dit worden veroorzaakt door taak verdeling in de service. Als het probleem zich blijft voordoen en niet wordt veroorzaakt door de toepassing die de schaalbaarheids limieten van de service bereikt, moet u een ondersteunings probleem veroorzaken. Voor client-time-outs moet u bepalen of de time-out is ingesteld op een geschikte waarde in de client en wijzigt u de time-outwaarde in de client of onderzoekt u hoe u de prestaties van de bewerkingen in de opslag service kunt verbeteren, bijvoorbeeld door te optimaliseren uw tabel voert query's uit of vermindert de grootte van uw berichten.

### <a name="metrics-show-an-increase-in-PercentNetworkError"></a>Met metrische gegevens wordt een toename in PercentNetworkError weer gegeven
Uw metrische gegevens tonen een toename in **PercentNetworkError** voor een van uw opslag Services. De metrische waarde voor **PercentNetworkError** is een aggregatie van de volgende metrische gegevens: **NetworkError**, **AnonymousNetworkError**en **SASNetworkError**. Deze treden op wanneer de opslag service een netwerk fout detecteert wanneer de client een opslag aanvraag doet.

De meest voorkomende oorzaak van deze fout is dat een client de verbinding verbreekt voordat een time-out in de opslag service verloopt. Onderzoek de code in uw client om te begrijpen waarom en wanneer de client de verbinding met de opslag service verbreekt. U kunt ook wireshark, micro soft Message Analyzer of Tcping gebruiken voor het onderzoeken van problemen met de netwerk verbinding van de client. Deze hulpprogram ma's worden beschreven in de [bijlagen].

### <a name="the-client-is-receiving-403-messages"></a>De client ontvangt HTTP 403-berichten (verboden)
Als de clienttoepassing een HTTP 403-fout (verboden) weergeeft, is een vermoedelijke oorzaak dat de client gebruikmaakt van een verlopen Shared Access Signature (SAS) bij het verzenden van een opslagaanvraag (hoewel andere mogelijke oorzaken een tijdverschil, ongeldige sleutels of lege headers kunnen zijn). Als een verlopen SAS-sleutel de oorzaak is, ziet u geen vermeldingen in de logboekgegevens voor logboekregistratie voor opslag aan de serverzijde. In de volgende tabel ziet u een voor beeld van het logboek aan de client zijde dat is gegenereerd door de Storage-client bibliotheek die het volgende laat zien:

| Source | Uitbreidings | Uitbreidings | Clientaanvraag-id | Tekst van bewerking |
| --- | --- | --- | --- | --- |
| Microsoft.Azure.Storage |Information |3 |85d077ab-… |De bewerking wordt gestart met locatie Primary per locatie modus PrimaryOnly. |
| Microsoft.Azure.Storage |Information |3 |85d077ab -… |Synchrone aanvraag wordt gestart om<https://domemaildist.blob.core.windows.netazureimblobcontainer/blobCreatedViaSAS.txt?sv=2014-02-14&sr=c&si=mypolicy&sig=OFnd4Rd7z01fIvh%2BmcR6zbudIH2F5Ikm%2FyhNYZEmJNQ%3D&api-version=2014-02-14> |
| Microsoft.Azure.Storage |Information |3 |85d077ab -… |Er wordt gewacht op reactie. |
| Microsoft.Azure.Storage |Waarschuwing |2 |85d077ab -… |Uitzonde ring opgetreden tijdens wachten op antwoord: De externe server heeft een fout geretourneerd: (403) Verboden. |
| Microsoft.Azure.Storage |Information |3 |85d077ab -… |Antwoord ontvangen. Status code = 403, aanvraag-ID = 9d67c64a-64ed-4b0d-9515-3b14bbcdc63d, content-MD5 =, ETag =. |
| Microsoft.Azure.Storage |Waarschuwing |2 |85d077ab -… |Uitzonde ring opgetreden tijdens de bewerking: De externe server heeft een fout geretourneerd: (403) verboden.. |
| Microsoft.Azure.Storage |Information |3 |85d077ab -… |Controleren of de bewerking opnieuw moet worden uitgevoerd. Aantal nieuwe pogingen = 0, HTTP-status code = 403, uitzonde ring = de externe server heeft een fout geretourneerd: (403) verboden.. |
| Microsoft.Azure.Storage |Information |3 |85d077ab -… |De volgende locatie is ingesteld op primair, op basis van de locatie modus. |
| Microsoft.Azure.Storage |Fout |1 |85d077ab -… |Het beleid voor opnieuw proberen is niet toegestaan voor een nieuwe poging. Fout bij het retour neren van de externe server: (403) Verboden. |

In dit scenario moet u onderzoeken waarom de SAS-token verloopt voordat de client het token naar de server verzendt:

* Normaliter mag u geen begintijd instellen als u een SAS voor een client maakt voor onmiddellijk gebruik. Als er kleine tijdverschillen zijn tussen de host die de SAS genereert en die gebruikmaakt van de huidige tijd en de opslagservice, dan kan de opslagservice een SAS ontvangen die nog niet geldig is.
* Stel geen heel korte verlooptijd in voor een SAS. Kleine tijdverschillen tussen de host die de SAS genereert en de opslagservice kunnen er ook toe leiden dat een SAS kennelijk eerder verloopt dan verwacht.
* Komt de versie parameter in de SAS-sleutel (bijvoorbeeld **SV = 2015-04-05**) overeen met de versie van de Storage-client bibliotheek die u gebruikt? U wordt aangeraden altijd de nieuwste versie van de [Storage-client bibliotheek](https://www.nuget.org/packages/WindowsAzure.Storage/)te gebruiken.
* Als u uw opslagtoegangssleutels opnieuw genereert, kunnen eventuele SAS-tokens ongeldig worden. Dit probleem doet zich voor als u SAS-tokens genereert met een lange verlooptijd voor clienttoepassingen die in de cache worden opgeslagen.

Als u de Storage-clientbibliotheek gebruikt om SAS-tokens te genereren, dan kan er gemakkelijk een geldig token worden gemaakt. Als u echter de opslag REST API gebruikt en de SAS-tokens hand matig bouwt, raadpleegt u [toegang met een Shared Access Signature overdragen](https://msdn.microsoft.com/library/azure/ee395415.aspx).

### <a name="the-client-is-receiving-404-messages"></a>De client ontvangt HTTP 404 (niet gevonden)-berichten
Als de clienttoepassing een HTTP 404-melding (niet gevonden) van de server ontvangt, houdt dit in dat het object dat de client wilde gebruiken (zoals een entiteit, tabel, blob, container of wachtrij) niet in de opslagservice aanwezig is. Hiervoor zijn een aantal mogelijke redenen, bijvoorbeeld:

* [Het object is eerder door de client of een ander proces verwijderd]
* [Een probleem met de verificatie van een SAS (Shared Access Signature)]
* [JavaScript-code aan de clientzijde heeft geen toestemming voor toegang tot het object]
* [Netwerkfout]

#### <a name="client-previously-deleted-the-object"></a>De client of een ander proces heeft het object eerder verwijderd
In scenario's waarin de client probeert gegevens te lezen, bij te werken of te verwijderen in een opslag service, is het meestal gemakkelijk te identificeren in de logboeken aan de server zijde een eerdere bewerking die het betreffende object uit de opslag service heeft verwijderd. Vaak toont de logboek gegevens aan dat een andere gebruiker of het proces het object heeft verwijderd. In het logboek voor logboek registratie aan de server zijde worden de kolommen bewerking-type en aangevraagde object sleutel weer gegeven wanneer een client een object heeft verwijderd.

In het scenario waarin een-client probeert een object in te voegen, is het mogelijk niet onmiddellijk duidelijk waarom dit resulteert in een HTTP 404 (niet gevonden), gezien het antwoord dat de client een nieuw object heeft gemaakt. Als de client echter een BLOB maakt, moet deze de BLOB-container kunnen vinden als de client een bericht maakt, moet er een wachtrij kunnen worden gevonden en als de client een rij toevoegt, moet deze de tabel kunnen vinden.

U kunt het logboek aan de client zijde van de Storage-client bibliotheek gebruiken om een gedetailleerder inzicht te krijgen wanneer de client specifieke aanvragen naar de opslag service verzendt.

Het volgende logboek aan de client zijde dat door de Storage-client bibliotheek wordt gegenereerd, illustreert het probleem wanneer de client de container voor de blob die het maakt, niet kan vinden. Dit logboek bevat details over de volgende opslag bewerkingen:

| Aanvraag-ID | Bewerking |
| --- | --- |
| 07b26a5d-... |Methode **DeleteIfExists** om de BLOB-container te verwijderen. Houd er rekening mee dat deze bewerking een **Head** -aanvraag bevat om te controleren of de container bestaat. |
| e2d06d78… |Methode **CreateIfNotExists** om de BLOB-container te maken. Houd er rekening mee dat deze bewerking een **Head** -aanvraag bevat waarmee wordt gecontroleerd of de container bestaat. De **kop** retourneert een 404-bericht, maar gaat door. |
| de8b1c3c-... |Methode **UploadFromStream** om de BLOB te maken. De **put** -aanvraag mislukt met een 404-bericht |

Logboek vermeldingen:

| Aanvraag-ID | Tekst van bewerking |
| --- | --- |
| 07b26a5d-... |Synchrone aanvraag wordt gestart https://domemaildist.blob.core.windows.net/azuremmblobcontainer naar. |
| 07b26a5d-... |StringToSign = HEAD............x-ms-client-request-id:07b26a5d-....x-ms-date:Tue, 03 Jun 2014 10:33:11 GMT.x-ms-version:2014-02-14./domemaildist/azuremmblobcontainer.restype:container. |
| 07b26a5d-... |Er wordt gewacht op reactie. |
| 07b26a5d-... |Antwoord ontvangen. Status code = 200, aanvraag-ID = eeead849-... Content-MD5 =, ETAG = &quot;0x8D14D2DC63D059B&quot;. |
| 07b26a5d-... |De antwoord headers zijn verwerkt, waarbij de rest van de bewerking wordt voortgezet. |
| 07b26a5d-... |De antwoord tekst wordt gedownload. |
| 07b26a5d-... |De bewerking is voltooid. |
| 07b26a5d-... |Synchrone aanvraag wordt gestart https://domemaildist.blob.core.windows.net/azuremmblobcontainer naar. |
| 07b26a5d-... |StringToSign = verwijderen........... x-MS-Client-Request-id: 07b26a5d-.... x-MS-date: DIN, 03 jun 2014 10:33:12 GMT. x-MS-version: 2014-02-14./domemaildist/azuremmblobcontainer. restype: container. |
| 07b26a5d-... |Er wordt gewacht op reactie. |
| 07b26a5d-... |Antwoord ontvangen. Status code = 202, aanvraag-ID = 6ab2a4cf-..., content-MD5 =, ETag =. |
| 07b26a5d-... |De antwoord headers zijn verwerkt, waarbij de rest van de bewerking wordt voortgezet. |
| 07b26a5d-... |De antwoord tekst wordt gedownload. |
| 07b26a5d-... |De bewerking is voltooid. |
| e2d06d78-... |Asynchrone aanvraag wordt gestart https://domemaildist.blob.core.windows.net/azuremmblobcontainer naar.</td> |
| e2d06d78-... |StringToSign = HEAD.............. x-MS-Client-Request-id: e2d06d78-.... x-MS-date: DIN, 03 jun 2014 10:33:12 GMT. x-MS-version: 2014-02-14./domemaildist/azuremmblobcontainer. restype: container. |
| e2d06d78-... |Er wordt gewacht op reactie. |
| de8b1c3c-... |Synchrone aanvraag wordt gestart https://domemaildist.blob.core.windows.net/azuremmblobcontainer/blobCreated.txt naar. |
| de8b1c3c-... |StringToSign = PUT... 64. qCmF + TQLPhq/YYK50mP9ZQ = =........ x-MS-BLOB-type: BlockBlob. x-MS-Client-Request-id: de8b1c3c-.... x-MS-date: DIN, 03 jun 2014 10:33:12 GMT. x-MS-version: 2014-02-14./domemaildist/azuremmblobcontainer/blobCreated. txt. |
| de8b1c3c-... |Het schrijven van aanvraag gegevens wordt voor bereid. |
| e2d06d78-... |Uitzonde ring opgetreden tijdens wachten op antwoord: De externe server heeft een fout geretourneerd: (404) niet gevonden.. |
| e2d06d78-... |Antwoord ontvangen. Status code = 404, aanvraag-ID = 353ae3bc-..., content-MD5 =, ETag =. |
| e2d06d78-... |De antwoord headers zijn verwerkt, waarbij de rest van de bewerking wordt voortgezet. |
| e2d06d78-... |De antwoord tekst wordt gedownload. |
| e2d06d78-... |De bewerking is voltooid. |
| e2d06d78-... |Asynchrone aanvraag wordt gestart https://domemaildist.blob.core.windows.net/azuremmblobcontainer naar. |
| e2d06d78-... |StringToSign = PUT... 0.......: x-MS-Client-Request-id: e2d06d78-.... x-MS-date: DIN, 03 jun 2014 10:33:12 GMT. x-MS-version: 2014-02-14./domemaildist/azuremmblobcontainer. restype: container. |
| e2d06d78-... |Er wordt gewacht op reactie. |
| de8b1c3c-... |Aanvraag gegevens worden geschreven. |
| de8b1c3c-... |Er wordt gewacht op reactie. |
| e2d06d78-... |Uitzonde ring opgetreden tijdens wachten op antwoord: De externe server heeft een fout geretourneerd: (409) conflict.. |
| e2d06d78-... |Antwoord ontvangen. Status code = 409, aanvraag-ID = c27da20e-..., content-MD5 =, ETag =. |
| e2d06d78-... |De tekst van het fout bericht wordt gedownload. |
| de8b1c3c-... |Uitzonde ring opgetreden tijdens wachten op antwoord: De externe server heeft een fout geretourneerd: (404) niet gevonden.. |
| de8b1c3c-... |Antwoord ontvangen. Status code = 404, aanvraag-ID = 0eaeab3e-..., content-MD5 =, ETag =. |
| de8b1c3c-... |Uitzonde ring opgetreden tijdens de bewerking: De externe server heeft een fout geretourneerd: (404) niet gevonden.. |
| de8b1c3c-... |Het beleid voor opnieuw proberen is niet toegestaan voor een nieuwe poging. Fout bij het retour neren van de externe server: (404) niet gevonden.. |
| e2d06d78-... |Het beleid voor opnieuw proberen is niet toegestaan voor een nieuwe poging. Fout bij het retour neren van de externe server: (409) conflict.. |

In dit voor beeld ziet u in het logboek dat de client via de **CreateIfNotExists** -methode (aanvraag-id e2d06d78...) aanvragen interleaveert met de aanvragen van de methode **UploadFromStream** (de8b1c3c-...). Deze interleaving gebeurt omdat de client toepassing deze methoden asynchroon aanroept. Wijzig de asynchrone code in de client om ervoor te zorgen dat deze de container maakt voordat u gegevens naar een BLOB in die container uploadt. In het ideale geval maakt u al uw containers vooraf.

#### <a name="SAS-authorization-issue"></a>Een Shared Access Signature-autorisatie probleem (SAS)
Als de client toepassing een SAS-sleutel probeert te gebruiken die niet de benodigde machtigingen voor de bewerking bevat, retourneert de opslag service een HTTP 404 (niet gevonden)-bericht naar de client. Op hetzelfde moment ziet u ook een waarde die niet gelijk is aan nul voor **SASAuthorizationError** in de metrische gegevens.

In de volgende tabel ziet u een voor beeld van een logboek bericht aan de server zijde uit het logboek bestand van de opslag logboek registratie:

| Name | Value |
| --- | --- |
| Begin tijd van aanvraag | 2014-05-30T06:17:48.4473697 Z |
| Bewerkings type     | GetBlobProperties            |
| Aanvraagstatus     | SASAuthorizationError        |
| HTTP-statuscode   | 404                          |
| Verificatietype| Gebaseerd                          |
| Servicetype       | Blob                         |
| Aanvraag-URL        | https://domemaildist.blob.core.windows.net/azureimblobcontainer/blobCreatedViaSAS.txt |
| &nbsp;                 |   ?sv=2014-02-14&sr=c&si=mypolicy&sig=XXXXX&;api-version=2014-02-14 |
| Header aanvraag-ID  | a1f348d5-8032-4912-93ef-b393e5252a3b |
| Clientaanvraag-id  | 2d064953-8436-4ee0-aa0c-65cb874f7929 |


Onderzoek waarom uw client toepassing een bewerking probeert uit te voeren waarvoor deze geen machtigingen heeft gekregen.

#### <a name="JavaScript-code-does-not-have-permission"></a>Java script-code aan de client zijde heeft geen machtiging voor toegang tot het object
Als u een Java script-client gebruikt en de opslag service HTTP 404-berichten retourneert, controleert u de volgende Java script-fouten in de browser:

```
SEC7120: Origin http://localhost:56309 not found in Access-Control-Allow-Origin header.
SCRIPT7002: XMLHttpRequest: Network Error 0x80070005, Access is denied.
```

> [!NOTE]
> U kunt de F12-Ontwikkelhulpprogramma's in Internet Explorer gebruiken om de berichten te traceren die worden uitgewisseld tussen de browser en de opslag service wanneer u problemen met Java script aan de client zijde oplost.
>
>

Deze fouten treden op omdat de webbrowser dezelfde beveiligings beperking voor het [herkomst beleid](https://www.w3.org/Security/wiki/Same_Origin_Policy) implementeert, waarmee wordt voor komen dat een webpagina een API aanroept in een ander domein dan het domein waarvan de pagina afkomstig is.

U kunt het Java script-probleem omzeilen door cross Origin Resource Sharing (CORS) te configureren voor de opslag service waartoe de client toegang heeft. Zie [ondersteuning voor het gebruik van meerdere bronnen (CORS) voor Azure Storage services](https://msdn.microsoft.com/library/azure/dn535601.aspx)voor meer informatie.

In het volgende code voorbeeld ziet u hoe u de BLOB-service zo configureert dat Java script wordt uitgevoerd in het contoso-domein om toegang te krijgen tot een BLOB in de Blob Storage-service:

```csharp
CloudBlobClient client = new CloudBlobClient(blobEndpoint, new StorageCredentials(accountName, accountKey));
// Set the service properties.
ServiceProperties sp = client.GetServiceProperties();
sp.DefaultServiceVersion = "2013-08-15";
CorsRule cr = new CorsRule();
cr.AllowedHeaders.Add("*");
cr.AllowedMethods = CorsHttpMethods.Get | CorsHttpMethods.Put;
cr.AllowedOrigins.Add("http://www.contoso.com");
cr.ExposedHeaders.Add("x-ms-*");
cr.MaxAgeInSeconds = 5;
sp.Cors.CorsRules.Clear();
sp.Cors.CorsRules.Add(cr);
client.SetServiceProperties(sp);
```

#### <a name="network-failure"></a>Netwerk fout
In sommige gevallen kunnen verloren netwerk pakketten leiden tot de opslag service die HTTP 404-berichten retourneert aan de client. Als uw client toepassing bijvoorbeeld een entiteit uit de tabel service verwijdert, ziet u dat de client een uitzonderings rapport voor een opslag uitzondering genereert met het status bericht ' HTTP 404 (niet gevonden) ' uit de tabel service. Wanneer u de tabel in de Table Storage-service onderzoekt, ziet u dat de service de entiteit heeft verwijderd zoals aangevraagd.

De details van de uitzonde ring in de client bevatten de aanvraag-ID (7e84f12d...) die is toegewezen door de tabel service voor de aanvraag: u kunt deze informatie gebruiken om de aanvraag gegevens in de opslag logboeken aan de server zijde te vinden door te zoeken in de kolom **aanvraag-id-header** in het logboek bestand. U kunt de metrische gegevens ook gebruiken om te bepalen wanneer er storingen optreden en vervolgens de logboek bestanden doorzoeken op basis van het tijdstip waarop de metrische gegevens deze fout hebben vastgelegd. In deze logboek vermelding ziet u dat het verwijderen is mislukt met het status bericht ' HTTP (404) andere fout '. Dezelfde logboek vermelding bevat ook de aanvraag-ID die door de client is gegenereerd in de kolom **client-request-id** (813ea74f...).

Het logboek aan de server zijde bevat ook een andere vermelding met dezelfde **client-aanvraag-id** -waarde (813ea74f...) voor een geslaagde Verwijder bewerking voor dezelfde entiteit en van dezelfde client. Deze geslaagde verwijderings bewerking duurde zeer kort vóór de mislukte verwijderings aanvraag.

De meest waarschijnlijke oorzaak van dit scenario is dat de client een aanvraag voor het verwijderen van de entiteit naar de tabel service heeft verzonden die is geslaagd, maar geen bevestiging heeft ontvangen van de server (mogelijk vanwege een tijdelijk netwerk probleem). De client heeft vervolgens automatisch opnieuw geprobeerd de bewerking uit te voeren (met dezelfde **client-aanvraag-id**) en de nieuwe poging is mislukt omdat de entiteit al is verwijderd.

Als dit probleem regel matig optreedt, moet u onderzoeken waarom de client geen bevestigingen van de tabel service kan ontvangen. Als het probleem zich weer voordoet, moet u de fout ' HTTP (404) niet gevonden ' onderscheppen en registreren in de client, maar de client toestaan om door te gaan.

### <a name="the-client-is-receiving-409-messages"></a>De client ontvangt HTTP 409-berichten (conflict)
In de volgende tabel ziet u een uittreksel van het logboek aan de server zijde voor twee client bewerkingen: **DeleteIfExists** wordt direct gevolgd door **CreateIfNotExists** met dezelfde BLOB-container naam. Elke client bewerking resulteert in twee aanvragen die naar de server worden verzonden, eerst een **GetContainerProperties** -aanvraag om te controleren of de container bestaat, gevolgd door de **Delete container** -of **CreateContainer** -aanvraag.

| Timestamp | Bewerking | Resultaat | Containernaam | Clientaanvraag-id |
| --- | --- | --- | --- | --- |
| 05:10:13.7167225 |GetContainerProperties |200 |mmcont |c9f52c89-… |
| 05:10:13.8167325 |DeleteContainer |202 |mmcont |c9f52c89-… |
| 05:10:13.8987407 |GetContainerProperties |404 |mmcont |bc881924-… |
| 05:10:14.2147723 |CreateContainer |409 |mmcont |bc881924-… |

Met de code in de client toepassing wordt een BLOB-container met dezelfde naam verwijderd en vervolgens opnieuw gemaakt: de methode **CreateIfNotExists** (client aanvraag-ID bc881924-...) uiteindelijk mislukt met de http 409 (conflict) fout. Als een client blobcontainers, tabellen of wachtrijen verwijderd, is er een korte periode voordat de naam opnieuw beschikbaar is.

De clienttoepassing moet een unieke containernaam gebruiken wanneer er een nieuwe container wordt gemaakt als het patroon voor verwijderen/opnieuw maken algemeen is.

### <a name="metrics-show-low-percent-success"></a>Metrische gegevens tonen een laag PercentSuccess of logboek vermeldingen van een analyse bewerking met de transactie status ClientOtherErrors
Met de metrische gegevens van de **PercentSuccess** wordt het percentage bewerkingen vastgelegd dat is geslaagd op basis van de HTTP-status code. Bewerkingen met status codes van 2XX tellen als geslaagd, terwijl bewerkingen met status codes in 3XX-, 4XX-en 5XX-bereiken als niet-geslaagde waarden worden beschouwd en de **PercentSuccess** metrische waarde verlagen. In de logboek bestanden voor opslag aan de server zijde worden deze bewerkingen vastgelegd met de transactie status **ClientOtherErrors**.

Het is belang rijk te weten dat deze bewerkingen zijn voltooid en daarom niet van invloed zijn op andere metrische gegevens, zoals Beschik baarheid. Enkele voor beelden van bewerkingen die met succes worden uitgevoerd, maar die kunnen leiden tot mislukte HTTP-status codes zijn:

* **ResourceNotFound** (Niet gevonden 404), bijvoorbeeld van een GET-aanvraag naar een blob die niet bestaat.
* **ResourceAlreadyExists** (Conflict 409), bijvoorbeeld van een **CreateIfNotExist** -bewerking waarbij de resource al bestaat.
* **ConditionNotMet** (Niet gewijzigd 304), bijvoorbeeld van een voorwaardelijke bewerking, zoals wanneer een client een **ETAG** -waarde en een http **If-None-Match-** header verzendt om een installatie kopie alleen aan te vragen als deze sinds de laatste bewerking is bijgewerkt.

U vindt een lijst met algemene REST API fout codes die door de opslag Services op de pagina worden geretourneerd [rest API fout codes](https://msdn.microsoft.com/library/azure/dd179357.aspx).

### <a name="capacity-metrics-show-an-unexpected-increase"></a>Met metrische gegevens over capaciteit wordt een onverwachte toename van het gebruik van de opslag capaciteit weer gegeven
Als er onverwachte, onverwacht wijzigingen in capaciteits gebruik in uw opslag account worden weer gegeven, kunt u de redenen onderzoeken door eerst te kijken naar uw metrische gegevens over beschik baarheid. een toename van het aantal mislukte verwijderings aanvragen kan bijvoorbeeld leiden tot een toename van de hoeveelheid Blob-opslag die u gebruikt als toepassingsspecifieke opschoon bewerkingen die u mogelijk had verwacht dat het vrijmaken van ruimte mogelijk niet werkt zoals verwacht (bijvoorbeeld , omdat de SAS-tokens die worden gebruikt voor het vrijmaken van ruimte, verlopen zijn).

### <a name="your-issue-arises-from-using-the-storage-emulator"></a>Uw probleem doet zich voor bij het gebruik van de opslag emulator voor ontwikkelen of testen
Normaal gesp roken gebruikt u de opslag emulator tijdens het ontwikkelen en testen om de vereiste voor een Azure-opslag account te voor komen. De meest voorkomende problemen die kunnen optreden wanneer u de opslag emulator gebruikt, zijn:

* [Functie ' X ' werkt niet in de opslag emulator]
* [Fout: de waarde voor een van de HTTP-headers heeft niet de juiste indeling als u de opslag emulator gebruikt]
* [Voor het uitvoeren van de opslag emulator zijn beheerders bevoegdheden vereist]

#### <a name="feature-X-is-not-working"></a>Functie ' X ' werkt niet in de opslag emulator
De opslag emulator biedt geen ondersteuning voor alle functies van de Azure Storage-services, zoals de bestands service. Zie [Use the Azure Storage Emulator for Development and Testing](storage-use-emulator.md) (De Azure-opslagemulator gebruiken voor het ontwikkelen en testen) voor meer informatie.

Gebruik de Azure Storage-service in de Cloud voor de functies die door de opslag emulator worden ondersteund.

#### <a name="error-HTTP-header-not-correct-format"></a>Fout: de waarde voor een van de HTTP-headers heeft niet de juiste indeling als u de opslag emulator gebruikt
U test uw toepassing die gebruikmaakt van de Storage-client bibliotheek op basis van de lokale-opslag emulator en methode aanroepen zoals **CreateIfNotExists** mislukt met het fout bericht ' de waarde voor een van de HTTP-headers heeft niet de juiste indeling. ' Dit geeft aan dat de versie van de door u gebruikte opslag-emulator geen ondersteuning biedt voor de versie van de opslag-client bibliotheek die u gebruikt. De Storage-client bibliotheek voegt de header **x-MS-version** toe aan alle aanvragen die het maakt. Als de opslag emulator de waarde in de **x-MS-version-** header niet herkent, wordt de aanvraag geweigerd.

U kunt de logboeken van de opslag bibliotheek-client gebruiken om de waarde van de **x-MS-version-header** te bekijken. deze wordt verzonden. U kunt ook de waarde van de **x-MS-version-header** zien als u Fiddler gebruikt om de aanvragen van uw client toepassing te traceren.

Dit scenario treedt doorgaans op als u de meest recente versie van de Storage-client bibliotheek installeert en gebruikt zonder de opslag emulator bij te werken. U moet de nieuwste versie van de opslag emulator installeren of Cloud opslag gebruiken in plaats van de emulator voor ontwikkeling en testen.

#### <a name="storage-emulator-requires-administrative-privileges"></a>Voor het uitvoeren van de opslag emulator zijn beheerders bevoegdheden vereist
U wordt gevraagd om beheerders referenties wanneer u de-opslag emulator uitvoert. Dit gebeurt alleen wanneer u de opslag emulator voor de eerste keer initialiseert. Nadat u de opslag emulator hebt geïnitialiseerd, hebt u geen beheerders bevoegdheden nodig om deze opnieuw uit te voeren.

Zie [Use the Azure Storage Emulator for Development and Testing](storage-use-emulator.md) (De Azure-opslagemulator gebruiken voor het ontwikkelen en testen) voor meer informatie. U kunt ook de opslag emulator initialiseren in Visual Studio, waarvoor ook beheerders bevoegdheden zijn vereist.

### <a name="you-are-encountering-problems-installing-the-Windows-Azure-SDK"></a>U ondervindt problemen bij het installeren van de Azure SDK voor .NET
Wanneer u de SDK probeert te installeren, wordt er geen poging gedaan om de opslag emulator op uw lokale computer te installeren. Het installatie logboek bevat een van de volgende berichten:

* CAQuietExec:  Fout: Geen toegang tot SQL-exemplaar
* CAQuietExec:  Fout: Kan geen data base maken

De oorzaak is een probleem met de bestaande LocalDB-installatie. De opslag emulator maakt standaard gebruik van LocalDB om gegevens op te slaan wanneer de Azure Storage-services worden gesimuleerd. U kunt uw LocalDB-exemplaar opnieuw instellen door de volgende opdrachten uit te voeren in een opdracht prompt venster voordat u de SDK probeert te installeren.

```
sqllocaldb stop v11.0
sqllocaldb delete v11.0
delete %USERPROFILE%\WAStorageEmulatorDb3*.*
sqllocaldb create v11.0
```

De opdracht **verwijderen** verwijdert oude database bestanden van eerdere installaties van de opslag emulator.

### <a name="you-have-a-different-issue-with-a-storage-service"></a>U hebt een ander probleem met een opslag service
Als de vorige probleemoplossings secties niet het probleem omvatten dat u met een opslag service hebt, moet u de volgende aanpak voor het vaststellen en oplossen van uw probleem vaststellen.

* Controleer uw metrische gegevens om te zien of er wijzigingen zijn in het verwachte basis gedrag van de regel. Vanuit de metrische gegevens kunt u mogelijk bepalen of het probleem tijdelijk of permanent is en welke opslag bewerkingen het probleem beïnvloedt.
* U kunt de metrische gegevens gebruiken om u te helpen bij het doorzoeken van de logboek gegevens aan de server zijde voor meer gedetailleerde informatie over de fouten die optreden. Deze informatie kan u helpen bij het oplossen van problemen en het oplossen van het probleem.
* Als de gegevens in de logboeken aan de server zijde niet voldoende zijn om het probleem op te lossen, kunt u de client bibliotheek van de opslag client-app gebruiken om het gedrag van uw client toepassing te onderzoeken en hulpprogram ma's zoals Fiddler, wireshark en micro soft Message Analyzer om uw netwerk te onderzoeken.

Zie voor meer informatie over het gebruik van Fiddler[Bijlage 1: HTTP-en HTTPS-verkeer vastleggen met behulp van Fiddler]en HTTPS-verkeer.

Zie voor meer informatie over het gebruik van wireshark[Bijlage 2: Wireshark gebruiken om netwerk verkeer vast te leggen]vast te leggen. "

Zie voor meer informatie over het gebruik van micro soft Message[Bijlage 3: Micro soft Message Analyzer gebruiken om netwerk verkeer vast te leggen]vast te leggen.

## <a name="appendices"></a>Bijlagen
In de bijlagen worden verschillende hulp middelen beschreven die nuttig kunnen zijn wanneer u problemen met Azure Storage (en andere services) opspoort en oplost. Deze hulpprogram ma's maken geen deel uit van Azure Storage en sommige producten van derden. De hulpprogram ma's die in deze aanhangsels worden behandeld, vallen niet onder een ondersteunings overeenkomst met Microsoft Azure of Azure Storage en daarom moet u als onderdeel van uw evaluatie proces de licentie-en ondersteunings opties bekijken die beschikbaar zijn via de providers van deze hulpprogram ma's.

### <a name="appendix-1"></a>Bijlage 1: HTTP-en HTTPS-verkeer vastleggen met behulp van Fiddler
[Fiddler](https://www.telerik.com/fiddler) is een handig hulp middel voor het analyseren van het http-en HTTPS-verkeer tussen uw client toepassing en de Azure Storage-service die u gebruikt.

> [!NOTE]
> Fiddler kunnen HTTPS-verkeer decoderen; Lees de documentatie van Fiddler aandachtig door om te begrijpen hoe dit gebeurt, en om inzicht te krijgen in de beveiligings implicaties.
>
>

Deze bijlage bevat een kort overzicht van het configureren van Fiddler voor het vastleggen van verkeer tussen de lokale computer waarop u Fiddler en de Azure Storage-services hebt geïnstalleerd.

Nadat u Fiddler hebt gestart, wordt het HTTP-en HTTPS-verkeer op uw lokale machine vastgelegd. Hier volgen enkele handige opdrachten voor het beheren van Fiddler:

* Het vastleggen van verkeer stoppen en starten. Ga in het hoofd menu naar **bestand** en klik vervolgens op vast **verkeer vastleggen** om vastleggen in of uit te scha kelen.
* Gegevens van vastgelegd verkeer opslaan. Ga in het hoofd menu naar **bestand**, klik op **Opslaan**en klik vervolgens op **alle sessies**. Hiermee kunt u het verkeer in een sessie archief bestand opslaan. U kunt later een sessie archief opnieuw laden voor analyse, of het verzenden als het wordt aangevraagd bij micro soft ondersteuning.

U kunt de hoeveelheid verkeer dat Fiddler vastlegt, beperken door filters te gebruiken die u configureert op het tabblad **filters** . De volgende scherm afbeelding toont een filter waarmee alleen verkeer wordt vastgelegd dat naar het **contosoemaildist.table.core.Windows.net** Storage-eind punt wordt verzonden:

![][5]

### <a name="appendix-2"></a>Bijlage 2: Wireshark gebruiken om netwerk verkeer vast te leggen
[Wireshark](https://www.wireshark.org/) is een Network Protocol Analyzer waarmee u gedetailleerde pakket gegevens kunt bekijken voor een breed scala aan netwerk protocollen.

De volgende procedure laat zien hoe u gedetailleerde pakket gegevens voor verkeer van de lokale computer waarop u wireshark hebt geïnstalleerd, kunt vastleggen in de tabel service in uw Azure-opslag account.

1. Start wireshark op uw lokale machine.
2. Selecteer in de sectie **starten** de lokale netwerk interface of interfaces die zijn verbonden met internet.
3. Klik op **vastleg opties**.
4. Voeg een filter toe aan het tekstvak voor het **opname filter** . **Host contosoemaildist.table.core.Windows.net** configureert bijvoorbeeld wireshark voor het vastleggen van alleen pakketten die zijn verzonden naar of van het eind punt van de tabel service in het **contosoemaildist** -opslag account. Bekijk de [volledige lijst met opname filters](https://wiki.wireshark.org/CaptureFilters).

   ![][6]
5. Klik op **Starten**. Wireshark legt nu alle pakketten vast die worden verzonden naar of van het eind punt van de tabel service wanneer u uw client toepassing op uw lokale computer gebruikt.
6. Wanneer u klaar bent, klikt u in het hoofd menu op **vastleggen** en vervolgens op **stoppen**.
7. Klik in het hoofd menu op **bestand** en vervolgens op **Opslaan**om de opgenomen gegevens op te slaan in een Wireshark-capture-bestand.

Met WireShark worden eventuele fouten in het venster **packetlist** gemarkeerd. U kunt ook het venster met informatie over de **expert** gebruiken (Klik op **analyseren**en vervolgens op **expert info**) om een samen vatting van fouten en waarschuwingen weer te geven.

![][7]

U kunt er ook voor kiezen om de TCP-gegevens weer te geven wanneer de toepassingslaag de laag ziet door met de rechter muisknop op de TCP-gegevens te klikken en **TCP-stroom volgen**te selecteren. Dit is handig als u uw dump hebt vastgelegd zonder een opname filter. Zie de [volgende TCP-streams](https://www.wireshark.org/docs/wsug_html_chunked/ChAdvFollowTCPSection.html)voor meer informatie.

![][8]

> [!NOTE]
> Zie de [Gebruikers handleiding voor wireshark](https://www.wireshark.org/docs/wsug_html_chunked)voor meer informatie over het gebruik van wireshark.
>
>

### <a name="appendix-3"></a>Bijlage 3: Micro soft Message Analyzer gebruiken om netwerk verkeer vast te leggen
U kunt micro soft Message Analyzer gebruiken voor het vastleggen van HTTP-en HTTPS-verkeer op een vergelijk bare manier als Fiddler en het vastleggen van netwerk verkeer op een vergelijk bare manier als wireshark.

#### <a name="configure-a-web-tracing-session-using-microsoft-message-analyzer"></a>Een web-tracerings sessie configureren met micro soft Message Analyzer
Als u een webtraceering-sessie wilt configureren voor HTTP-en HTTPS-verkeer met behulp van micro soft Message Analyzer, voert u de toepassing micro soft Message Analyzer uit en klikt u vervolgens in het menu **File** op **Capture/Trace**. Selecteer **Web Proxy**in de lijst met beschik bare traceer scenario's. Voeg vervolgens in het venster **configuratie van traceer scenario** in het tekstvak **HostnameFilter** de namen van uw opslag eindpunten toe (u kunt deze namen opzoeken in de [Azure Portal](https://portal.azure.com)). Als de naam van uw Azure Storage-account bijvoorbeeld **contosodata**is, voegt u het volgende toe aan het tekstvak **HostnameFilter** :

```
contosodata.blob.core.windows.net contosodata.table.core.windows.net contosodata.queue.core.windows.net
```

> [!NOTE]
> Een spatie teken scheidt de hostnamen.
>
>

Wanneer u klaar bent om te beginnen met het verzamelen van tracerings gegevens, klikt u op de knop **beginnen met** .

Zie [micro soft-PEF-webproxy-provider](https://technet.microsoft.com/library/jj674814.aspx)voor meer informatie over de **webproxy** -tracering van micro soft Message Analyzer.

De ingebouwde **Web Proxy** -tracering in micro soft Message Analyzer is gebaseerd op Fiddler. Er kan HTTPS-verkeer aan de client zijde worden vastgelegd en niet-versleutelde HTTPS-berichten worden weer gegeven. De **webproxy** tracering werkt door een lokale proxy te configureren voor alle http-en HTTPS-verkeer die het toegang geeft tot niet-versleutelde berichten.

#### <a name="diagnosing-network-issues-using-microsoft-message-analyzer"></a>Netwerk problemen vaststellen met behulp van micro soft Message Analyzer
Naast het gebruik van de **webproxy** tracering van micro soft Message Analyzer voor het vastleggen van gegevens van het HTTP/HTTPS-verkeer tussen de client toepassing en de Storage-service, kunt u ook de ingebouwde **lokale link-laag** traceren gebruiken om netwerk vast te leggen pakket gegevens. Zo kunt u gegevens vastleggen die vergelijkbaar zijn met die u kunt vastleggen met wireshark en problemen met het netwerk, zoals verwijderde pakketten, vaststellen.

De volgende scherm afbeelding toont een voor beeld van een **lokale koppelingslaag** Trace met enkele **informatieve** berichten in de kolom **DiagnosisTypes** . Als u op een pictogram in de kolom **DiagnosisTypes** klikt, worden de details van het bericht weer gegeven. In dit voor beeld wordt de server opnieuw verzonden met het bericht #305 omdat er geen bevestiging is ontvangen van de client:

![][9]

Wanneer u de traceer sessie in micro soft Message Analyzer maakt, kunt u filters opgeven om de hoeveelheid ruis in de tracering te verminderen. Klik op de pagina **vastleggen/traceren** waar u de tracering definieert op de koppeling **configureren** naast **micro soft-Windows-NDIS-PacketCapture**. De volgende scherm afbeelding toont een configuratie waarmee TCP-verkeer wordt gefilterd voor de IP-adressen van drie opslag Services:

![][10]

Zie [micro soft-PEF-NDIS-PacketCapture-provider](https://technet.microsoft.com/library/jj659264.aspx)voor meer informatie over de local link layer trace van micro soft Message Analyzer.

### <a name="appendix-4"></a>Aanhangsel 4: Excel gebruiken voor het weer geven van metrische gegevens en logbestanden
Met veel hulpprogram ma's kunt u de metrische gegevens van de opslag van Azure Table-opslag downloaden in een indeling met scheidings tekens, waardoor het eenvoudig wordt om de gegevens in Excel te laden voor weer gave en analyse. Gegevens van de opslag registratie van Azure Blob-opslag bevindt zich al in een indeling met scheidings tekens die u in Excel kunt laden. U moet echter wel de juiste kolom koppen toevoegen op basis van de gegevens in [Opslaganalyse logboek indeling](https://msdn.microsoft.com/library/azure/hh343259.aspx) en [Opslaganalyse metrische tabel schema](https://msdn.microsoft.com/library/azure/hh343264.aspx).

Als u de gegevens van uw opslag logboek wilt importeren in Excel nadat u deze hebt gedownload van Blob Storage:

* Klik in het menu **Data** op **van tekst**.
* Blader naar het logboek bestand dat u wilt weer geven en klik op **importeren**.
* In stap 1 van de **wizard Tekst importeren**selecteert u **gescheiden**.

Selecteer in stap 1 van de **wizard Tekst importeren** **punt komma** als het enige scheidings teken en kies dubbele aanhalings tekens als **tekst indicator**. Klik vervolgens op **volt ooien** en kies waar u de gegevens in uw werkmap wilt plaatsen.

### <a name="appendix-5"></a>Bijlage 5: Bewaking met Application Insights voor Azure DevOps
U kunt ook de Application Insights functie voor Azure DevOps gebruiken als onderdeel van de prestatie-en beschikbaarheids bewaking. Dit hulp programma kan:

* Zorg ervoor dat uw webservice beschikbaar is en reageert. Of uw app een website of een apparaat-app is die gebruikmaakt van een webservice, u kunt uw URL om de paar minuten testen vanaf locaties over de hele wereld, en u laten weten of er een probleem is.
* Snel diagnose stellen van prestatie problemen of uitzonde ringen in uw webservice. Bepaal of er CPU-of andere resources worden uitgerekt, haal stack traceringen op uit uitzonde ringen en zoek eenvoudig logboek traceringen. Als de prestaties van de app onder aanvaard bare limieten vallen, kan micro soft u een e-mail sturen. U kunt zowel .NET als Java-webservices bewaken.

U kunt meer informatie vinden op [wat Application Insights is](../../azure-monitor/app/app-insights-overview.md).

## <a name="next-steps"></a>Volgende stappen

Raadpleeg de volgende bronnen voor meer informatie over Analytics in Azure Storage:

* [Een Storage-account bewaken in de Azure-portal](storage-monitor-storage-account.md)
* [Opslag analyse](storage-analytics.md)
* [Metrische gegevens van opslag analyse](storage-analytics-metrics.md)
* [Tabel schema voor metrische gegevens van opslag analyse](/rest/api/storageservices/storage-analytics-metrics-table-schema)
* [Logboeken voor opslag analyse](storage-analytics-logging.md)
* [Logboek bestands indeling voor opslag analyse](/rest/api/storageservices/storage-analytics-log-format)

<!--Anchors-->
[Inleiding]: #introduction
[Hoe deze hand leiding is ingedeeld]: #how-this-guide-is-organized

[Uw opslag service controleren]: #monitoring-your-storage-service
[Service status controleren]: #monitoring-service-health
[Bewakings capaciteit]: #monitoring-capacity
[Beschik baarheid bewaken]: #monitoring-availability
[Prestaties bewaken]: #monitoring-performance

[Problemen met opslag vaststellen]: #diagnosing-storage-issues
[Service status problemen]: #service-health-issues
[Prestatie problemen]: #performance-issues
[Fouten vaststellen]: #diagnosing-errors
[Problemen met de opslag emulator]: #storage-emulator-issues
[Hulpprogram ma's voor opslag logboek registratie]: #storage-logging-tools
[Hulpprogram ma's voor netwerk logboek registratie gebruiken]: #using-network-logging-tools

[End-to-end tracering]: #end-to-end-tracing
[Gerelateerde logboek gegevens]: #correlating-log-data
[Client aanvraag-ID]: #client-request-id
[Server aanvraag-ID]: #server-request-id
[Timestamps]: #timestamps

[Hulp bij het oplossen van problemen]: #troubleshooting-guidance
[Prestatiegegevens geven hoge AverageE2ELatency en lage AverageServerLatency aan]: #metrics-show-high-AverageE2ELatency-and-low-AverageServerLatency
[Prestatiegegevens geven lage AverageE2ELatency en lage AverageServerLatency aan, maar de client ondervindt hoge latentie]: #metrics-show-low-AverageE2ELatency-and-low-AverageServerLatency
[Prestatiegegevens geven hoge AverageServerLatency aan]: #metrics-show-high-AverageServerLatency
[U ervaart onverwachte vertragingen bij de levering van berichten in een wachtrij]: #you-are-experiencing-unexpected-delays-in-message-delivery

[Prestatiegegevens geven een toename in PercentThrottlingError aan]: #metrics-show-an-increase-in-PercentThrottlingError
[Tijdelijke toename in Percentthrottlingerror aan]: #transient-increase-in-PercentThrottlingError
[Permanente toename van de Percentthrottlingerror aan-fout]: #permanent-increase-in-PercentThrottlingError
[Prestatiegegevens geven een toename in PercentTimeoutError aan]: #metrics-show-an-increase-in-PercentTimeoutError
[Prestatiegegevens geven een toename in PercentNetworkError aan]: #metrics-show-an-increase-in-PercentNetworkError

[De client ontvangt HTTP 403-meldingen (verboden)]: #the-client-is-receiving-403-messages
[De client ontvangt HTTP 404-meldingen (niet gevonden)]: #the-client-is-receiving-404-messages
[Het object is eerder door de client of een ander proces verwijderd]: #client-previously-deleted-the-object
[Een probleem met de verificatie van een SAS (Shared Access Signature)]: #SAS-authorization-issue
[JavaScript-code aan de clientzijde heeft geen toestemming voor toegang tot het object]: #JavaScript-code-does-not-have-permission
[Netwerkfout]: #network-failure
[De client ontvangt berichten van de HTTP-409 (Conflict)]: #the-client-is-receiving-409-messages

[Metrische gegevens tonen lage PercentSuccess of analytics logboekvermeldingen bewerkingen hebben met de status van ClientOtherErrors]: #metrics-show-low-percent-success
[Capaciteit metrische gegevens tonen een onverwachte toename in opslaggebruik capaciteit]: #capacity-metrics-show-an-unexpected-increase
[Het probleem zich voordoet de opslagemulator voor ontwikkeling of tests gebruiken]: #your-issue-arises-from-using-the-storage-emulator
[Functie ' X ' werkt niet in de opslag emulator]: #feature-X-is-not-working
[Fout: de waarde voor een van de HTTP-headers heeft niet de juiste indeling als u de opslag emulator gebruikt]: #error-HTTP-header-not-correct-format
[Voor het uitvoeren van de opslag emulator zijn beheerders bevoegdheden vereist]: #storage-emulator-requires-administrative-privileges
[U ondervindt problemen met het installeren van de Azure SDK voor .NET]: #you-are-encountering-problems-installing-the-Windows-Azure-SDK
[U hebt een ander probleem met storage-service]: #you-have-a-different-issue-with-a-storage-service

[Bijlagen]: #appendices
[Bijlage 1: HTTP-en HTTPS-verkeer vastleggen met behulp van Fiddler]: #appendix-1
[Bijlage 2: Wireshark gebruiken om netwerk verkeer vast te leggen]: #appendix-2
[Bijlage 3: Micro soft Message Analyzer gebruiken om netwerk verkeer vast te leggen]: #appendix-3
[Aanhangsel 4: Excel gebruiken voor het weer geven van metrische gegevens en logbestanden]: #appendix-4
[Bijlage 5: Bewaking met Application Insights voor Azure DevOps]: #appendix-5

<!--Image references-->
[1]: ./media/storage-monitoring-diagnosing-troubleshooting/overview.png
[3]: ./media/storage-monitoring-diagnosing-troubleshooting/hour-minute-metrics.png
[4]: ./media/storage-monitoring-diagnosing-troubleshooting/high-e2e-latency.png
[5]: ./media/storage-monitoring-diagnosing-troubleshooting/fiddler-screenshot.png
[6]: ./media/storage-monitoring-diagnosing-troubleshooting/wireshark-screenshot-1.png
[7]: ./media/storage-monitoring-diagnosing-troubleshooting/wireshark-screenshot-2.png
[8]: ./media/storage-monitoring-diagnosing-troubleshooting/wireshark-screenshot-3.png
[9]: ./media/storage-monitoring-diagnosing-troubleshooting/mma-screenshot-1.png
[10]: ./media/storage-monitoring-diagnosing-troubleshooting/mma-screenshot-2.png
