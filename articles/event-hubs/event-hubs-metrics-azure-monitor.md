---
title: Metrische gegevens in Azure Monitor-Azure Event Hubs | Microsoft Docs
description: Dit artikel bevat informatie over het gebruik van de Azure Monitoring voor het bewaken van Azure Event Hubs
services: event-hubs
documentationcenter: .NET
author: ShubhaVijayasarathy
manager: timlt
editor: ''
ms.assetid: ''
ms.service: event-hubs
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.custom: seodec18
ms.date: 09/18/2019
ms.author: shvija
ms.openlocfilehash: 788f0647bec11184c2a85d87d0dfde2cb6c5744c
ms.sourcegitcommit: 3f22ae300425fb30be47992c7e46f0abc2e68478
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/25/2019
ms.locfileid: "71266307"
---
# <a name="azure-event-hubs-metrics-in-azure-monitor"></a>Event Hubs metrische gegevens voor Azure in Azure Monitor

Event Hubs metrische gegevens geven u de status van Event Hubs resources in uw Azure-abonnement. Met een uitgebreide set metrische gegevens, kunt u de algemene status van uw eventhubs niet alleen op het niveau van de naamruimte, maar ook op het entiteitsniveau van de beoordelen. Deze statistische gegevens is belangrijk, omdat ze u houden op de status van uw eventhubs. Metrische gegevens kunnen ook helpen problemen hoofdoorzaak zonder contact opnemen met ondersteuning van Azure.

Azure Monitor biedt een uniforme gebruikersinterfaces voor bewaking over de verschillende Azure-services. Zie voor meer informatie, [bewaken in Microsoft Azure](../monitoring-and-diagnostics/monitoring-overview.md) en de [ophalen van Azure Monitor metrics met .NET](https://github.com/Azure-Samples/monitor-dotnet-metrics-api) op GitHub.

## <a name="access-metrics"></a>Toegang tot metrische gegevens

Azure Monitor biedt meerdere manieren voor toegang tot metrische gegevens. U hebt toegang tot metrische gegevens via de [Azure Portal](https://portal.azure.com), of u kunt gebruikmaken van de Azure monitor API'S (rest en .net) en analyse oplossingen, zoals Log Analytics en Event hubs. Zie voor meer informatie, [door gegevens te controleren die worden verzameld door Azure Monitor](../azure-monitor/platform/data-platform.md).

Metrische gegevens zijn standaard ingeschakeld en u hebt toegang tot gegevens van de meest recente 30 dagen. Als u behouden van gegevens voor een langere periode wilt, kunt u metrische gegevens om een Azure Storage-account te archiveren. Dit is geconfigureerd in [diagnostische instellingen](../azure-monitor/platform/diagnostic-settings.md) in Azure Monitor.


## <a name="access-metrics-in-the-portal"></a>Toegang tot metrische gegevens in de portal

U kunt metrische gegevens controleren na verloop van tijd in de [Azure-portal](https://portal.azure.com). Het volgende voorbeeld laat zien hoe om binnenkomende aanvragen op accountniveau en geslaagde aanvragen weer te geven:

![Geslaagde metrische gegevens weergeven][1]

U kunt ook toegang tot metrische gegevens rechtstreeks via de naamruimte. Hiertoe selecteert u uw naam ruimte en klikt u op **metrische gegevens**. Als u de metrische gegevens wilt weer geven die zijn gefilterd op het bereik van de Event Hub, selecteert u de Event Hub en klikt u vervolgens op **metrische gegevens**.

Voor metrische gegevens voor ondersteuning van dimensies, moet u filteren met de waarde van de gewenste dimensie zoals wordt weergegeven in het volgende voorbeeld:

![Filteren met de dimensiewaarde][2]

## <a name="billing"></a>Billing

Het gebruik van metrische gegevens in Azure Monitor is momenteel gratis. Echter, als u aanvullende oplossingen die metrische gegevens opnemen, u mogelijk worden kosten in rekening gebracht door deze oplossingen. U wordt bijvoorbeeld gefactureerd door Azure Storage als u metrische gegevens om een Azure Storage-account te archiveren. U wordt ook gefactureerd door Azure als u metrische gegevens streamt naar Azure Monitor-logboeken voor geavanceerde analyse.

De volgende metrische gegevens geven u een overzicht van de status van uw service. 

> [!NOTE]
> Er zijn verschillende metrische gegevens niet meer ondersteund als ze worden verplaatst onder een andere naam. Dit moet u mogelijk uw referenties bijwerken. Metrische gegevens die zijn gemarkeerd met het sleutelwoord 'afgeschaft' wordt niet ondersteund voortaan.

Alle metrische waarden worden verzonden naar Azure Monitor elke minuut. De tijdgranulatie definieert het tijdsinterval waarvoor metrische waarden worden weergegeven. Het ondersteunde tijdsinterval voor alle Event Hubs-metrische gegevens is 1 minuut.

## <a name="request-metrics"></a>Aanvraag voor metrische gegevens

Telt het aantal aanvragen voor beheer van gegevens en bewerkingen.

| Naam van meetwaarde | Description |
| ------------------- | ----------------- |
| Binnenkomende aanvragen  | Het aantal aanvragen voor de Azure Event Hubs-service gedurende een bepaalde periode. <br/><br/> Teleenheid Count <br/> Aggregatie type: Totaal <br/> Dimensielideigenschap EntityName |
| Geslaagde aanvragen    | Het aantal geslaagde aanvragen voor de Azure Event Hubs-service gedurende een bepaalde periode. <br/><br/> Teleenheid Count <br/> Aggregatie type: Totaal <br/> Dimensielideigenschap EntityName |
| Serverfouten  | Het aantal aanvragen die niet worden verwerkt vanwege een fout in de Azure Event Hubs-service gedurende een bepaalde periode. <br/><br/>Teleenheid Count <br/> Aggregatie type: Totaal <br/> Dimensielideigenschap EntityName |
|Gebruikers fouten |Het aantal aanvragen die niet worden verwerkt wegens gebruikersfouten gedurende een bepaalde periode.<br/><br/> Teleenheid Count <br/> Aggregatie type: Totaal <br/> Dimensielideigenschap EntityName|
|Quota overschreden fouten |Het aantal aanvragen van overschreden het beschikbare quotum. Zie [in dit artikel](event-hubs-quotas.md) voor meer informatie over Event Hubs-quota.<br/><br/> Teleenheid Count <br/> Aggregatie type: Totaal <br/> Dimensielideigenschap EntityName|

## <a name="throughput-metrics"></a>Metrische gegevens over doorvoer

| Naam van meetwaarde | Description |
| ------------------- | ----------------- |
|Vertraagde aanvragen |Het aantal aanvragen die zijn beperkt omdat het gebruik van de eenheid doorvoer is overschreden.<br/><br/> Teleenheid Count <br/> Aggregatie type: Totaal <br/> Dimensielideigenschap EntityName|

## <a name="message-metrics"></a>Bericht metrische gegevens

| Naam van meetwaarde | Description |
| ------------------- | ----------------- |
|Binnenkomende berichten |Het aantal gebeurtenissen of berichten die worden verzonden naar Event Hubs in een opgegeven periode.<br/><br/> Teleenheid Count <br/> Aggregatie type: Totaal <br/> Dimensielideigenschap EntityName|
|Uitgaande berichten |Het aantal gebeurtenissen of berichten ophalen uit Event Hubs in een opgegeven periode.<br/><br/> Teleenheid Count <br/> Aggregatie type: Totaal <br/> Dimensielideigenschap EntityName|
|Binnenkomende bytes |Het aantal bytes dat is verzonden naar de Azure Event Hubs-service gedurende een bepaalde periode.<br/><br/> Teleenheid Bytes <br/> Aggregatie type: Totaal <br/> Dimensielideigenschap EntityName|
|Uitgaande bytes |Het aantal bytes opgehaald vanuit de Azure Event Hubs-service gedurende een bepaalde periode.<br/><br/> Teleenheid Bytes <br/> Aggregatie type: Totaal <br/> Dimensielideigenschap EntityName|

## <a name="connection-metrics"></a>Metrische verbindingsgegevens

| Naam van meetwaarde | Description |
| ------------------- | ----------------- |
|ActiveConnections |Het aantal actieve verbindingen voor een naamruimte, maar ook op een entiteit.<br/><br/> Teleenheid Count <br/> Aggregatie type: Totaal <br/> Dimensielideigenschap EntityName|
|Geopende verbindingen |Het aantal open verbindingen.<br/><br/> Teleenheid Count <br/> Aggregatie type: Totaal <br/> Dimensielideigenschap EntityName|
|Gesloten verbindingen |Het aantal gesloten verbindingen.<br/><br/> Teleenheid Count <br/> Aggregatie type: Totaal <br/> Dimensielideigenschap EntityName|

## <a name="event-hubs-capture-metrics"></a>Event Hubs Capture metrische gegevens

U kunt Event Hubs Capture metrische gegevens controleren wanneer u de Capture-functie voor uw eventhubs inschakelt. De volgende metrische gegevens wordt beschreven wat u kunt controleren met vastleggen is ingeschakeld.

| Naam van meetwaarde | Description |
| ------------------- | ----------------- |
|Achterstand vastleggen |Het aantal bytes die nog moeten worden vastgelegd in het geselecteerde doel.<br/><br/> Teleenheid Bytes <br/> Aggregatie type: Totaal <br/> Dimensielideigenschap EntityName|
|Vastgelegde berichten |Het aantal berichten of gebeurtenissen die zijn vastgelegd op de gekozen bestemming gedurende een bepaalde periode.<br/><br/> Teleenheid Count <br/> Aggregatie type: Totaal <br/> Dimensielideigenschap EntityName|
|Vastgelegde bytes |Het aantal bytes dat voor de gekozen bestemming gedurende een bepaalde periode worden vastgelegd.<br/><br/> Teleenheid Bytes <br/> Aggregatie type: Totaal <br/> Dimensielideigenschap EntityName|

## <a name="metrics-dimensions"></a>Metrische gegevens over dimensies

Azure Event Hubs ondersteunt de volgende dimensies voor metrische gegevens in Azure Monitor. Dimensies toevoegen aan uw metrische gegevens is optioneel. Als u dimensies niet toevoegt, worden de metrische gegevens opgegeven op het niveau van de naamruimte. 

| Naam van meetwaarde | Description |
| ------------------- | ----------------- |
|EntityName| Eventhubs biedt ondersteuning voor de event hub-entiteiten in de naamruimte.|

## <a name="azure-monitor-integration-with-siem-tools"></a>Integratie met SIEM-hulpprogram ma's Azure Monitor
Door uw bewakings gegevens (activiteiten logboeken, Diagnostische logboeken, enzovoort) te routeren naar een Event Hub met Azure Monitor kunt u gemakkelijk integreren met de hulpprogram ma's van Security Information and Event Management (SIEM). Raadpleeg de volgende artikelen/blog berichten voor meer informatie:

- [Azure-bewakings gegevens streamen naar een Event Hub voor gebruik door een extern hulp programma](../azure-monitor/platform/stream-monitoring-data-event-hubs.md)
- [Inleiding tot Azure Log Integration](../security/fundamentals/azure-log-integration-overview.md)
- [Azure Monitor gebruiken om te integreren met SIEM-hulpprogramma's](https://azure.microsoft.com/blog/use-azure-monitor-to-integrate-with-siem-tools/)

In het scenario waarin een SIEM-hulp programma logboek gegevens van een Event Hub verbruikt, als u geen inkomende berichten ziet of als u inkomende berichten ziet, maar geen uitgaande berichten in de grafiek metrische gegevens, volgt u deze stappen:

- Als er **geen inkomende berichten**zijn, betekent dit dat de Azure Monitor-service geen controle-en Diagnostische logboeken naar de Event hub verplaatst. In dit scenario opent u een ondersteunings ticket met het Azure Monitor team. 
- Als er inkomende berichten zijn, maar **geen uitgaande berichten**, betekent dit dat de Siem-toepassing de berichten niet leest. Neem contact op met de SIEM-provider om te bepalen of de configuratie van de Event Hub deze toepassingen juist is.


## <a name="next-steps"></a>Volgende stappen

* Zie de [overzicht Azure Monitoring](../monitoring-and-diagnostics/monitoring-overview.md).
* [Ophalen van Azure Monitor metrics met .NET](https://github.com/Azure-Samples/monitor-dotnet-metrics-api) op GitHub. 

Voor meer informatie over Event Hubs gaat u naar de volgende koppelingen:

* Aan de slag met een [Event Hubs-zelfstudie](event-hubs-dotnet-standard-getstarted-send.md)
* [Veelgestelde vragen over Event Hubs](event-hubs-faq.md)
* [Voorbeeldtoepassingen die gebruikmaken van Event Hubs](https://github.com/Azure/azure-event-hubs/tree/master/samples)

[1]: ./media/event-hubs-metrics-azure-monitor/event-hubs-monitor1.png
[2]: ./media/event-hubs-metrics-azure-monitor/event-hubs-monitor2.png



