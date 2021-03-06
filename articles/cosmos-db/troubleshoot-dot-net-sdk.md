---
title: Problemen vaststellen en oplossen bij het gebruik van Azure Cosmos DB .NET SDK
description: Gebruik functies als logboek registratie aan client zijde en andere hulpprogram ma's van derden voor het identificeren, diagnosticeren en Azure Cosmos DB oplossen van problemen met het gebruik van .NET SDK.
author: j82w
ms.service: cosmos-db
ms.date: 05/28/2019
ms.author: jawilley
ms.subservice: cosmosdb-sql
ms.topic: troubleshooting
ms.reviewer: sngun
ms.openlocfilehash: 51b37c43b94ad59090f32af0d57bbefaa57f30fa
ms.sourcegitcommit: f3f4ec75b74124c2b4e827c29b49ae6b94adbbb7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/12/2019
ms.locfileid: "70932559"
---
# <a name="diagnose-and-troubleshoot-issues-when-using-azure-cosmos-db-net-sdk"></a>Problemen vaststellen en oplossen bij het gebruik van Azure Cosmos DB .NET SDK
Dit artikel heeft betrekking op veelvoorkomende problemen, tijdelijke oplossingen, diagnostische stappen en hulpprogram ma's wanneer u de [.NET SDK](sql-api-sdk-dotnet.md) gebruikt met Azure Cosmos DB SQL API-accounts.
De .NET-SDK biedt logische weer gave aan de client zijde om toegang te krijgen tot de Azure Cosmos DB SQL-API. In dit artikel worden hulp middelen en benaderingen beschreven om u te helpen bij het uitvoeren van problemen.

## <a name="checklist-for-troubleshooting-issues"></a>Controle lijst voor het oplossen van problemen:
Bekijk de volgende controle lijst voordat u uw toepassing naar productie gaat verplaatsen. Met de controle lijst voor komt u dat er verschillende veelvoorkomende problemen worden weer geven. U kunt ook snel vaststellen wanneer er een probleem optreedt:

*   De nieuwste [SDK](https://github.com/Azure/azure-cosmos-dotnet-v2/blob/master/changelog.md)gebruiken. Preview-Sdk's mogen niet worden gebruikt voor productie. Dit voor komt dat bekende problemen die al zijn opgelost, worden gerepareerd.
*   Bekijk de [Tips voor prestaties](performance-tips.md)en volg de aanbevolen procedures. Dit helpt voor komen dat schalen, latentie en andere prestatie problemen worden opgelost.
*   Schakel de SDK-logboek registratie in om u te helpen bij het oplossen van een probleem. Het inschakelen van de logboek registratie kan van invloed zijn op de prestaties, zodat het het beste kan worden ingeschakeld wanneer u problemen oplost. U kunt de volgende logboeken inschakelen:
    *   [Metrische gegevens vastleggen](monitor-accounts.md) met behulp van de Azure Portal. Met metrische gegevens van de portal wordt de Azure Cosmos DB telemetrie weer gegeven. Dit is handig om te bepalen of het probleem overeenkomt met Azure Cosmos DB of van de client zijde.
    *   Registreer de [Diagnostische teken reeks](https://docs.microsoft.com/dotnet/api/microsoft.azure.documents.client.resourceresponsebase.requestdiagnosticsstring?view=azure-dotnet) vanuit de antwoorden op punt bewerkingen.
    *   De [metrische gegevens](sql-api-query-metrics.md) van de SQL-query registreren bij alle antwoorden op query's 
    *   Volg de installatie van de [SDK-logboek registratie]( https://github.com/Azure/azure-cosmos-dotnet-v2/blob/master/docs/documentdb-sdk_capture_etl.md)

Bekijk de sectie [veelvoorkomende problemen en tijdelijke oplossingen](#common-issues-workarounds) in dit artikel.

Raadpleeg de [sectie met github-problemen](https://github.com/Azure/azure-cosmos-dotnet-v2/issues) die actief worden bewaakt. Controleer of er een soortgelijk probleem met een tijdelijke oplossing al is ingediend. Als u geen oplossing hebt gevonden, kunt u een GitHub-probleem oplossen. U kunt een ondersteunings streepje openen voor urgente problemen.


## <a name="common-issues-workarounds"></a>Veelvoorkomende problemen en tijdelijke oplossingen

### <a name="general-suggestions"></a>Algemene suggesties
* Voer, indien mogelijk, uw app uit in dezelfde Azure-regio als uw Azure Cosmos DB-account. 
* U kunt problemen met de connectiviteit/Beschik baarheid ondervinden als gevolg van een gebrek aan resources op de client computer. U wordt aangeraden uw CPU-gebruik te bewaken op knoop punten waarop de Azure Cosmos DB-client wordt uitgevoerd, en omhoog/omlaag te schalen als deze bij hoge belasting worden uitgevoerd.

### <a name="check-the-portal-metrics"></a>Controleer de metrische gegevens van de portal
Door de [metrische gegevens](monitor-accounts.md) van de portal te controleren, kunt u bepalen of het een probleem aan de client zijde is of dat er een probleem is met de service. Als de metrische gegevens bijvoorbeeld een hoog aantal aanvragen met een rente beperking bevatten (HTTP-status code 429), wat betekent dat de aanvraag wordt beperkt, controleert u de sectie [Aanvraag frequentie te groot] . 

### <a name="request-timeouts"></a>Time-outs van aanvragen
RequestTimeout treedt doorgaans op wanneer u direct/TCP gebruikt, maar kan zich in de gateway modus voordoen. Dit zijn de meest voorkomende bekende oorzaken en suggesties voor het oplossen van het probleem.

* CPU-gebruik is hoog, waardoor latentie en/of time-outs voor aanvragen worden veroorzaakt. De klant kan de hostcomputer zo schalen dat er meer resources worden toegewezen, of de belasting kan worden gedistribueerd op meer computers.
* Beschik baarheid van socket/poort is mogelijk laag. Bij het uitvoeren in azure kunnen clients die gebruikmaken van de .NET SDK, de poort uitputting voor Azure SNAT (PAT) aanraken. Gebruik de meest recente versie 2. x of 3. x van de .NET-SDK om te voor komen dat dit probleem optreedt. Dit is een voor beeld van waarom het wordt aanbevolen om altijd de nieuwste SDK-versie uit te voeren.
* Het maken van meerdere DocumentClient-instanties kan leiden tot verbindings conflicten en time-outproblemen. Volg de [Tips voor prestaties](performance-tips.md)en gebruik één DocumentClient-exemplaar in een heel proces.
* Gebruikers zien soms verhoogde latentie of time-outs van aanvragen omdat hun verzamelingen ontoereikend zijn ingericht, de back-end vertragings aanvragen en de client worden intern zonder halen aan de aanroeper. Controleer de [metrische gegevens](monitor-accounts.md)van de portal.
* Azure Cosmos DB distribueert de totale ingerichte door Voer gelijkmatig over fysieke partities. Controleer de metrische gegevens van de portal om te zien of de werk belasting een Hot- [partitie sleutel](partition-data.md)tegen komt. Dit zorgt ervoor dat de geaggregeerde door Voer (RU/s) onder het ingerichte RUs lijkt, maar een door Voer (RU/s) van één partitie overschrijdt de ingerichte door voer. 
* Daarnaast voegt de 2,0 SDK kanaal semantiek toe aan directe/TCP-verbindingen. Er wordt één TCP-verbinding gebruikt voor meerdere aanvragen tegelijk. Dit kan leiden tot twee problemen onder specifieke gevallen:
    * Een hoge mate van gelijktijdigheid kan leiden tot conflicten op het kanaal.
    * Grote aanvragen of antwoorden kunnen leiden tot hoofd-of-line blok kering van het kanaal en de exacerbate-inhoud, zelfs met een relatief lage mate van gelijktijdigheid.
    * Als de aanvraag in een van deze twee categorieën valt (of als er sprake is van een hoog CPU-gebruik), zijn dit mogelijke oplossingen:
        * Probeer de toepassing omhoog te schalen.
        * Daarnaast kunnen SDK-logboeken worden vastgelegd via de [traceer-listener](https://github.com/Azure/azure-cosmosdb-dotnet/blob/master/docs/documentdb-sdk_capture_etl.md) om meer details te krijgen.

### <a name="connection-throttling"></a>Verbindings beperking
De verbindings beperking kan worden veroorzaakt door een verbindings limiet op een hostcomputer. Vóór 2,0 kunnen clients die in Azure worden uitgevoerd, de [Uitputting van de poort van Azure SNAT (PAT)]aanraken.

### <a name="snat"></a>Uitputting van de poort van Azure SNAT (PAT)

Als uw app is geïmplementeerd op Azure Virtual Machines zonder een openbaar IP-adres, worden de standaard [Azure SNAT-poorten](https://docs.microsoft.com/azure/load-balancer/load-balancer-outbound-connections#preallocatedports) gebruikt om verbinding te maken met een wille keurig eind punt buiten uw VM. Het aantal verbindingen dat van de virtuele machine naar het Azure Cosmos DB-eind punt is toegestaan, wordt beperkt door de [Azure SNAT-configuratie](https://docs.microsoft.com/azure/load-balancer/load-balancer-outbound-connections#preallocatedports).

 Azure SNAT-poorten worden alleen gebruikt als uw virtuele machine een privé-IP-adres heeft en een proces van de virtuele machine probeert verbinding te maken met een openbaar IP-adres. Er zijn twee oplossingen om Azure SNAT-beperking te voor komen:

* Voeg uw Azure Cosmos DB Service-eind punt toe aan het subnet van uw virtuele Azure Virtual Machines-netwerk. Zie [Azure Virtual Network Service-eind punten](https://docs.microsoft.com/azure/virtual-network/virtual-network-service-endpoints-overview)voor meer informatie. 

    Wanneer het service-eind punt is ingeschakeld, worden de aanvragen niet langer verzonden vanuit een openbaar IP-adres naar Azure Cosmos DB. In plaats daarvan worden de identiteit van het virtuele netwerk en het subnet verzonden. Deze wijziging kan ertoe leiden dat de firewall wordt neergezet als alleen open bare IP-adressen zijn toegestaan. Als u een firewall gebruikt en u het service-eind punt inschakelt, voegt u een subnet toe aan de firewall door gebruik te maken van [Virtual Network acl's](https://docs.microsoft.com/azure/virtual-network/virtual-networks-acl).
* Wijs een openbaar IP-adres toe aan uw Azure-VM.

### <a name="http-proxy"></a>HTTP-proxy
Als u een HTTP-proxy gebruikt, moet u ervoor zorgen dat het het aantal verbindingen dat in de `ConnectionPolicy`SDK is geconfigureerd kan ondersteunen.
Anders worden er verbindings problemen beschreven.

### Aanvraag frequentie te groot<a name="request-rate-too-large"></a>
' De aanvraag snelheid is te groot ' of de fout code 429 geeft aan dat uw aanvragen worden beperkt, omdat de verbruikte door Voer (RU/s) de ingerichte door Voer heeft overschreden. De SDK voert automatisch aanvragen opnieuw uit op basis van het opgegeven [beleid voor opnieuw proberen](https://docs.microsoft.com/dotnet/api/microsoft.azure.documents.client.connectionpolicy.retryoptions?view=azure-dotnet). Als deze fout vaak optreedt, kunt u overwegen de door Voer van de verzameling te verhogen. Controleer de [metrische gegevens van de portal](use-metrics.md) om te zien of er 429 fouten optreden. Controleer uw [partitie sleutel](https://docs.microsoft.com/azure/cosmos-db/partitioning-overview#choose-partitionkey) om ervoor te zorgen dat deze een gelijkmatige verdeling van de opslag en het aanvraag volume levert. 

### <a name="slow-query-performance"></a>Trage query prestaties
Met de [metrische gegevens](sql-api-query-metrics.md) van de query kunt u bepalen waar de query de meeste tijd in bebesteding neemt. Vanuit de metrische gegevens van de query kunt u zien hoeveel er wordt uitgegeven aan de back-end van de client.
* Als de back-end-query snel wordt geretourneerd en een grote tijd op de client wordt gespendeerd, controleert u de belasting van de computer. Het is waarschijnlijk dat er onvoldoende bronnen zijn en de SDK wacht totdat de resources beschikbaar zijn voor het verwerken van het antwoord.
* Als de back-end-query langzaam is, [optimaliseert u de query](optimize-cost-queries.md) en bekijkt u het huidige [indexerings beleid](index-overview.md) 

 <!--Anchors-->
[Common issues and workarounds]: #common-issues-workarounds
[Enable client SDK logging]: #logging
[Aanvraag frequentie te groot]: #request-rate-too-large
[Request Timeouts]: #request-timeouts
[Uitputting van de poort van Azure SNAT (PAT)]: #snat
[Production check list]: #production-check-list


