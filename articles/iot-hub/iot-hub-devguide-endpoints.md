---
title: Meer informatie over Azure IoT Hub-eind punten | Microsoft Docs
description: 'Hand leiding voor ontwikkel aars: Naslag informatie over IoT Hub naar een apparaat gerichte en service gerichte eind punten.'
author: robinsh
manager: philmea
ms.author: robinsh
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 06/10/2019
ms.openlocfilehash: 689a702863dda53870f775bd8520d5dd406d242f
ms.sourcegitcommit: 3877b77e7daae26a5b367a5097b19934eb136350
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/30/2019
ms.locfileid: "68640598"
---
# <a name="reference---iot-hub-endpoints"></a>Referentie-IoT Hub-eind punten

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-partial.md)]

## <a name="iot-hub-names"></a>IoT Hub namen

U kunt de hostnaam van de IoT-hub die als host fungeert voor uw eind punten vinden in de portal op de overzichts pagina van uw hub. De DNS-naam van een IoT-hub ziet er standaard uit `{your iot hub name}.azure-devices.net`als:.

## <a name="list-of-built-in-iot-hub-endpoints"></a>Lijst met ingebouwde IoT Hub-eind punten

Azure IoT Hub is een service met meerdere tenants die de functionaliteit van verschillende actors weergeeft. In het volgende diagram ziet u de verschillende eind punten die IoT Hub beschikbaar maakt.

![IoT Hub-eindpunten](./media/iot-hub-devguide-endpoints/endpoints.png)

In de volgende lijst worden de eind punten beschreven:

* **Resource provider**. De provider van de IoT Hub-resource maakt een [Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) -interface beschikbaar. Met deze interface kunnen Azure-abonnements eigenaren IoT-hubs maken en verwijderen en IoT hub-eigenschappen bijwerken. IoT Hub eigenschappen regelen op het [niveau van beveiligings beleid](iot-hub-devguide-security.md#access-control-and-permissions)op hubniveau, in tegens telling tot toegangs beheer op apparaatniveau, en functionele opties voor Cloud-naar-apparaat-en apparaat-naar-Cloud-berichten. Met de resource provider van IoT Hub kunt u ook [apparaat-id's exporteren](iot-hub-devguide-identity-registry.md#import-and-export-device-identities).

* **Apparaat-id beheren**. Elke IoT hub bevat een set HTTPS REST-eind punten voor het beheren van apparaat-id's (maken, ophalen, bijwerken en verwijderen). [Apparaat-id's](iot-hub-devguide-identity-registry.md) worden gebruikt voor verificatie van apparaten en toegangs beheer.

* **Dubbele beheer van apparaten**. Elke IoT hub biedt een set service gerichte HTTPS REST-eind punten om [apparaatdubbels](iot-hub-devguide-device-twins.md) te zoeken en bij te werken (tags en eigenschappen bijwerken).

* **Taak beheer**. Elke IoT hub biedt een reeks service gerichte HTTPS REST-eind punten voor het opvragen en beheren van [taken](iot-hub-devguide-jobs.md).

* **Apparaat-eind punten**. Voor elk apparaat in het identiteits register IoT Hub een set eind punten beschikbaar:

  * *Apparaat-naar-Cloud-berichten verzenden*. Een apparaat gebruikt dit eind punt om [apparaat-naar-Cloud-berichten te verzenden](iot-hub-devguide-messages-d2c.md).

  * *Cloud-naar-apparaat-berichten ontvangen*. Een apparaat gebruikt dit eind punt om de doel [berichten van Cloud naar apparaat](iot-hub-devguide-messages-c2d.md)te ontvangen.

  * *Uploads van bestanden starten*. Een apparaat gebruikt dit eind punt om een Azure Storage SAS-URI van IoT Hub te ontvangen om [een bestand te uploaden](iot-hub-devguide-file-upload.md).

  * *Dubbele eigenschappen van het apparaat ophalen en bijwerken*. Een apparaat gebruikt dit eind punt om toegang te krijgen tot de eigenschappen van het [apparaat twee](iot-hub-devguide-device-twins.md).

  * *Ontvang direct-methode aanvragen*. Een apparaat gebruikt dit eind punt om te Luis teren naar aanvragen van [directe methoden](iot-hub-devguide-direct-methods.md).

    Deze eind punten worden beschikbaar gemaakt met behulp van [MQTT v 3.1.1](https://mqtt.org/), https 1,1 en [AMQP 1,0](https://www.amqp.org/) -protocollen. AMQP is ook beschikbaar via [](https://tools.ietf.org/html/rfc6455) websockets op poort 443.

* **Service-eind punten**. Elke IoT hub biedt een set eind punten voor de back-end van uw oplossing om met uw apparaten te communiceren. Met één uitzonde ring worden deze eind punten alleen weer gegeven met behulp van het [AMQP](https://www.amqp.org/) -protocol. Het eind punt van de methode aanroep wordt weer gegeven via het HTTPS-protocol.
  
  * *Apparaat-naar-Cloud-berichten ontvangen*. Dit eind punt is compatibel met [Azure Event hubs](https://azure.microsoft.com/documentation/services/event-hubs/). Een back-end-service kan deze gebruiken om de [apparaat-naar-Cloud-berichten](iot-hub-devguide-messages-d2c.md) te lezen die door uw apparaten worden verzonden. U kunt naast dit ingebouwde eind punt aangepaste eind punten maken op uw IoT-hub.
  
  * *Cloud-naar-apparaat-berichten verzenden en*ontvangst bevestigingen ontvangen. Met deze eind punten kan de back-end van uw oplossing een betrouw bare [Cloud-naar-apparaat-berichten](iot-hub-devguide-messages-c2d.md)verzenden en de bijbehorende bezorgings-en vergelijkings bevestigingen ontvangen.
  
  * *Bestands meldingen ontvangen*. Met dit berichten eindpunt kunt u meldingen ontvangen wanneer een bestand is geüpload door uw apparaten. 
  
  * *Directe methode aanroep*. Met dit eind punt kan een back-end-service een [rechtstreekse methode](iot-hub-devguide-direct-methods.md) aanroepen op een apparaat.
  
  * Gebeurtenissen voor het bewaken van *bewerkingen ontvangen*. Met dit eind punt kunt u bewakings gebeurtenissen van bewerkingen ontvangen als uw IoT-hub is geconfigureerd om deze te verzenden. Zie [IOT hub Operations monitoring](iot-hub-operations-monitoring.md)voor meer informatie.

In het artikel [Azure IOT sdk's](iot-hub-devguide-sdks.md) worden de verschillende manieren beschreven om toegang te krijgen tot deze eind punten.

Alle IoT Hub-eind punten gebruiken het [TLS](https://tools.ietf.org/html/rfc5246) -protocol en er wordt nooit een eind punt weer gegeven op niet-versleutelde/niet-beveiligde kanalen.

## <a name="custom-endpoints"></a>Aangepaste eindpunten

U kunt bestaande Azure-Services in uw abonnement koppelen aan uw IoT-hub om te fungeren als eind punten voor bericht routering. Deze eind punten fungeren als service-eind punten en worden gebruikt als sinks voor bericht routes. Apparaten kunnen niet rechtstreeks naar de extra eind punten schrijven. Meer informatie over [bericht routering](../iot-hub/iot-hub-devguide-messages-d2c.md).

IoT Hub ondersteunt momenteel de volgende Azure-Services als extra eind punten:

* Azure-opslagcontainers
* Event Hubs
* Service Bus-wachtrijen
* Service Bus-onderwerpen

Zie [quota's en beperken](iot-hub-devguide-quotas-throttling.md)voor de limieten voor het aantal eind punten dat u kunt toevoegen.

U kunt de REST API status van [eind punt ophalen](https://docs.microsoft.com/rest/api/iothub/iothubresource/getendpointhealth#iothubresource_getendpointhealth) gebruiken om de status van de eind punten op te halen. U kunt het beste de [IOT hub metrische gegevens](iot-hub-metrics.md) met betrekking tot de bericht latentie van de route ring gebruiken om fouten op te sporen en op te sporen wanneer de status van het eind punt inactief of beschadigd is, omdat er een latentie wordt verwacht wanneer het eind punt zich in een van deze statussen bevindt.

|Status|Description|
|---|---|
|healthy|Het eind punt accepteert berichten zoals verwacht.|
|slechte|Het eind punt accepteert geen berichten zoals verwacht en IoT Hub probeert gegevens naar dit eind punt te verzenden. De status van een onjuist eind punt wordt bijgewerkt naar in orde wanneer IoT Hub een uiteindelijk consistente status van de status heeft bereikt.|
|onbekend|IoT Hub heeft geen verbinding gemaakt met het eind punt. Er zijn geen berichten van dit eind punt bezorgd aan of afgewezen.|
|geval|Het eind punt accepteert geen berichten nadat IoT Hub opnieuw hebt geprobeerd berichten te verzenden voor de evaluatie periode.|

## <a name="field-gateways"></a>Veld gateways

In een IoT-oplossing zit een *veld Gateway* tussen uw apparaten en uw IOT hub-eind punten. Het bevindt zich doorgaans dicht bij uw apparaten. Uw apparaten communiceren rechtstreeks met de veld Gateway met behulp van een protocol dat door de apparaten wordt ondersteund. De veld Gateway maakt verbinding met een IoT Hub-eind punt met behulp van een protocol dat wordt ondersteund door IoT Hub. Een veld Gateway kan een specifiek hardwareapparaat zijn of een computer met een laag energie verbruik aangepaste gateway software.

U kunt [Azure IOT Edge](/azure/iot-edge/) gebruiken om een veld Gateway te implementeren. IoT Edge biedt functionaliteit zoals multiplex communicatie van meerdere apparaten op dezelfde IoT Hub verbinding.

## <a name="next-steps"></a>Volgende stappen

Andere naslag onderwerpen in deze IoT Hub ontwikkelaars handleiding zijn:

* [IoT Hub query taal voor apparaatdubbels, Jobs en bericht routering](iot-hub-devguide-query-language.md)
* [Quota en beperkingen](iot-hub-devguide-quotas-throttling.md)
* [Ondersteuning voor IoT Hub MQTT](iot-hub-mqtt-support.md)
* [Meer informatie over het IP-adres van uw IoT-hub](iot-hub-understand-ip-address.md)