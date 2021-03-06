---
title: Verg Roten/verkleinen van Azure IoT Hub | Microsoft Docs
description: Uw IoT-hub schalen om uw verwachte bericht doorvoer en gewenste functies te ondersteunen. Bevat een samen vatting van de ondersteunde door Voer voor elke laag en opties voor sharding.
author: wesmc7777
manager: timlt
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 06/28/2019
ms.author: wesmc
ms.openlocfilehash: d1de29124825a7f398b9722bb2455d1105e9c9f7
ms.sourcegitcommit: 11265f4ff9f8e727a0cbf2af20a8057f5923ccda
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/08/2019
ms.locfileid: "72023637"
---
# <a name="choose-the-right-iot-hub-tier-for-your-solution"></a>Kies de juiste IoT Hub laag voor uw oplossing

Elke IoT-oplossing verschilt, zodat Azure IoT Hub verschillende opties biedt op basis van prijzen en schaal. Dit artikel is bedoeld om u te helpen bij het evalueren van uw IoT Hub behoeften. Zie voor prijs informatie over IoT Hub lagen [IOT hub prijzen](https://azure.microsoft.com/pricing/details/iot-hub).

Als u wilt beslissen welke IoT Hub-laag geschikt is voor uw oplossing, stelt u de volgende twee vragen:

**Welke functies moet ik gebruiken?**

Azure IoT Hub biedt twee lagen, basis en standaard, die verschillen in het aantal functies dat wordt ondersteund. Als uw IoT-oplossing is gebaseerd op het verzamelen van gegevens van apparaten en deze centraal analyseert, is de basis-laag waarschijnlijk geschikt voor u. Als u meer geavanceerde configuraties wilt gebruiken om IoT-apparaten op afstand te beheren of een deel van uw werk belastingen op de apparaten zelf te distribueren, moet u rekening houden met de Standard-laag. Voor een gedetailleerd overzicht van de functies die zijn opgenomen in elke laag, blijven de [basis-en standaard-laag](#basic-and-standard-tiers).

**Hoeveel gegevens moet ik dagelijks verplaatsen?**

Elke IoT Hub-laag is beschikbaar in drie grootten, op basis van de hoeveelheid gegevens doorvoer die ze op een bepaalde dag kunnen verwerken. Deze grootten worden numeriek aangeduid als 1, 2 en 3. Elk eenheid van een niveau 1 IoT hub kan bijvoorbeeld 400.000 berichten per dag afhandelen, terwijl een eenheid van niveau 3 300.000.000 kan verwerken. Ga verder met de [door Voer van berichten](#message-throughput)voor meer informatie over de richt lijnen voor gegevens.

## <a name="basic-and-standard-tiers"></a>Basis-en standaard lagen

De laag standaard van IoT Hub maakt alle functies mogelijk en is vereist voor IoT-oplossingen die gebruik moeten maken van de bidirectionele communicatie mogelijkheden. De laag basis maakt een subset van de functies mogelijk en is bedoeld voor IoT-oplossingen die alleen uni-directionele communicatie van apparaten naar de Cloud nodig hebben. Beide lagen bieden dezelfde beveiligings-en verificatie functies.

Er kan slechts één type [versie](https://azure.microsoft.com/pricing/details/iot-hub/) in een laag worden gekozen per IOT hub. U kunt bijvoorbeeld een IoT Hub maken met meerdere eenheden van S1, maar niet met een combi natie van eenheden uit verschillende versies, zoals S1 en S2.

| Mogelijkheid | Basislaag | Laag gratis/Standard |
| ---------- | ---------- | ------------- |
| [Telemetrie van apparaat naar Cloud](iot-hub-devguide-messaging.md) | Ja | Ja |
| [Identiteit per apparaat](iot-hub-devguide-identity-registry.md) | Ja | Ja |
| [Bericht routering](iot-hub-devguide-messages-read-custom.md), [verrijkingen van berichten](iot-hub-message-enrichments-overview.md)en [integratie van Event grid](iot-hub-event-grid.md) | Ja | Ja |
| [HTTP-, AMQP-en MQTT-protocollen](iot-hub-devguide-protocols.md) | Ja | Ja |
| [Device Provisioning Service](../iot-dps/about-iot-dps.md) | Ja | Ja |
| [Bewaking en diagnose](iot-hub-monitor-resource-health.md) | Ja | Ja |
| [Cloud-naar-apparaat-berichten](iot-hub-devguide-c2d-guidance.md) |   | Ja |
| [Apparaat apparaatdubbels](iot-hub-devguide-device-twins.md), [module apparaatdubbels](iot-hub-devguide-module-twins.md)en [Apparaatbeheer](iot-hub-device-management-overview.md) |   | Ja |
| [Apparaatversleuteling (preview-versie)](iot-hub-device-streams-overview.md) |   | Ja |
| [Azure IoT Edge](../iot-edge/about-iot-edge.md) |   | Ja |
| [Preview van IoT Plug en Play](../iot-pnp/overview-iot-plug-and-play.md) |   | Ja |

IoT Hub biedt ook een gratis laag die bedoeld is voor test-en evaluatie doeleinden. Het bevat alle mogelijkheden van de laag standaard, maar beperkte bericht limieten. U kunt geen upgrade uitvoeren van de gratis laag naar Basic of Standard.

## <a name="partitions"></a>Partities

Azure IoT hubs bevatten veel kern onderdelen van [azure Event hubs](../event-hubs/event-hubs-features.md), met inbegrip van [partities](../event-hubs/event-hubs-features.md#partitions). Gebeurtenis stromen voor IoT-hubs worden doorgaans gevuld met inkomende telemetriegegevens die worden gerapporteerd door verschillende IoT-apparaten. De partitionering van de gebeurtenis stroom wordt gebruikt om de conflicten te verminderen die optreden bij het gelijktijdig lezen van en schrijven naar gebeurtenis stromen.

De partitie limiet wordt gekozen als IoT Hub wordt gemaakt en kan niet worden gewijzigd. De maximale partitie limiet voor de Basic-laag IoT Hub en de standaardlaag IoT Hub is 32. De meeste IoT-hubs hebben alleen vier partities nodig. Zie voor meer informatie over het bepalen van de partities het Event Hubs Veelgestelde vragen [hoeveel partities heb ik nodig?](../event-hubs/event-hubs-faq.md#how-many-partitions-do-i-need)

## <a name="tier-upgrade"></a>Upgrade van Tier

Als u uw IoT-hub hebt gemaakt, kunt u een upgrade uitvoeren van de Basic-laag naar de Standard-laag zonder uw bestaande bewerkingen te onderbreken. Zie [een upgrade uitvoeren van uw IOT-hub](iot-hub-upgrade.md)voor meer informatie.

De partitie configuratie blijft ongewijzigd wanneer u migreert van de Basic-laag naar de Standard-laag.

> [!NOTE]
> De gratis laag biedt geen ondersteuning voor upgrades naar Basic of Standard.

## <a name="iot-hub-rest-apis"></a>REST API's voor IoT Hub

Het verschil in ondersteunde mogelijkheden tussen de lagen basis en standaard van IoT Hub betekent dat sommige API-aanroepen niet werken met de Basic-laag hubs. In de volgende tabel ziet u welke Api's beschikbaar zijn:

| API | Basislaag | Laag gratis/Standard |
| --- | ---------- | ------------- |
| [Apparaat verwijderen](https://docs.microsoft.com/rest/api/iothub/service/deletedevice) | Ja | Ja |
| [Apparaat ophalen](https://docs.microsoft.com/rest/api/iothub/service/getdevice) | Ja | Ja |
| [Module verwijderen](https://docs.microsoft.com/rest/api/iothub/service/deletemodule) | Ja | Ja |
| [Module ophalen](https://docs.microsoft.com/rest/api/iothub/service/getmodule) | Ja | Ja |
| [Register statistieken ophalen](https://docs.microsoft.com/rest/api/iothub/service/getdeviceregistrystatistics) | Ja | Ja |
| [Statistieken van services ophalen](https://docs.microsoft.com/rest/api/iothub/service/getservicestatistics) | Ja | Ja |
| [Apparaat maken of bijwerken](https://docs.microsoft.com/rest/api/iothub/service/createorupdatedevice) | Ja | Ja |
| [Module maken of bijwerken](https://docs.microsoft.com/rest/api/iothub/service/createorupdatemodule) | Ja | Ja |
| [Query IoT Hub](https://docs.microsoft.com/rest/api/iothub/service/queryiothub) | Ja | Ja |
| [De SAS-URI voor het uploaden van bestanden maken](https://docs.microsoft.com/rest/api/iothub/device/createfileuploadsasuri) | Ja | Ja |
| [Bevestiging van afhankelijk apparaat ontvangen](https://docs.microsoft.com/rest/api/iothub/device/receivedeviceboundnotification) | Ja | Ja |
| [Gebeurtenis van apparaat verzenden](https://docs.microsoft.com/rest/api/iothub/device/senddeviceevent) | Ja | Ja |
| Module gebeurtenis verzenden | Alleen AMQP en MQTT | Alleen AMQP en MQTT |
| [Upload status van update bestand](https://docs.microsoft.com/rest/api/iothub/device/updatefileuploadstatus) | Ja | Ja |
| [Bewerking bulksgewijs apparaat](https://docs.microsoft.com/rest/api/iothub/service/bulkcreateorupdatedevices) | Ja, met uitzonde ring van IoT Edge mogelijkheden | Ja |
| [Export taak importeren annuleren](https://docs.microsoft.com/rest/api/iothub/service/cancelimportexportjob) | Ja | Ja |
| [Import export taak maken](https://docs.microsoft.com/rest/api/iothub/service/createimportexportjob) | Ja | Ja |
| [Import export taak ophalen](https://docs.microsoft.com/rest/api/iothub/service/getimportexportjob) | Ja | Ja |
| [Import-export taken ophalen](https://docs.microsoft.com/rest/api/iothub/service/getimportexportjobs) | Ja | Ja |
| [Opdracht wachtrij leegmaken](https://docs.microsoft.com/rest/api/iothub/service/purgecommandqueue) |   | Ja |
| [Apparaat dubbele ophalen](https://docs.microsoft.com/rest/api/iothub/service/gettwin) |   | Ja |
| [Module dubbele ophalen](https://docs.microsoft.com/rest/api/iothub/service/getmoduletwin) |   | Ja |
| [Methode aanroepen van apparaat](https://docs.microsoft.com/rest/api/iothub/service/invokedevicemethod) |   | Ja |
| [Dubbele update van apparaat](https://docs.microsoft.com/rest/api/iothub/service/updatetwin) |   | Ja |
| [Module bijwerken dubbele](https://docs.microsoft.com/rest/api/iothub/service/updatemoduletwin) |   | Ja |
| [Melding van afhankelijkheid van apparaat afbreken](https://docs.microsoft.com/rest/api/iothub/device/abandondeviceboundnotification) |   | Ja |
| [Bevestiging van afhankelijk apparaat volt ooien](https://docs.microsoft.com/rest/api/iothub/device/completedeviceboundnotification) |   | Ja |
| [Taak annuleren](https://docs.microsoft.com/rest/api/iothub/service/canceljob) |   | Ja |
| [Taak maken](https://docs.microsoft.com/rest/api/iothub/service/createjob) |   | Ja |
| [Taak ophalen](https://docs.microsoft.com/rest/api/iothub/service/getjob) |   | Ja |
| [Query taken](https://docs.microsoft.com/rest/api/iothub/service/queryjobs) |   | Ja |

## <a name="message-throughput"></a>Bericht doorvoer

De beste manier om een IoT Hub oplossing te verg Roten is om het verkeer per eenheid te evalueren. Houd in het bijzonder rekening met de vereiste piek doorvoer voor de volgende categorieën bewerkingen:

* Apparaat-naar-cloud-berichten
* Cloud-naar-apparaat-berichten
* Registerbewerkingen voor identiteit

Verkeer wordt per eenheid gemeten voor uw IoT-hub. Wanneer u een IoT-hub maakt, kiest u de betreffende laag en editie en stelt u het aantal beschik bare eenheden in. U kunt tot 200 eenheden kopen voor de B1, B2, S1 of S2-editie, of tot 10 eenheden voor de B3-of S3-editie. Nadat uw IoT-hub is gemaakt, kunt u het aantal beschik bare eenheden voor de editie wijzigen, een upgrade of downgrade uitvoeren tussen edities binnen de laag (B1 naar B2) of een upgrade uitvoeren van het basis niveau naar de Standard-laag (B1 naar S1) zonder uw bestaande bewerkingen te onderbreken. Zie [een upgrade uitvoeren van uw IOT-hub](iot-hub-upgrade.md)voor meer informatie.  

Als voor beeld van de verkeers mogelijkheden van elke laag gelden voor apparaat-naar-Cloud-berichten de volgende continue doorvoer richtlijnen:

| Tier Edition | Continue door Voer | Continue verzend frequentie |
| --- | --- | --- |
| B1, S1 |Maxi maal 1111 KB/minuut per eenheid<br/>(1,5 GB/dag/eenheid) |Gemiddeld aantal 278 berichten/minuut per eenheid<br/>(400.000 berichten/dag per eenheid) |
| B2, S2 |Maxi maal 16 MB/minuut per eenheid<br/>(22,8 GB/dag/eenheid) |Gemiddeld aantal 4.167 berichten/minuut per eenheid<br/>(6.000.000 berichten/dag per eenheid) |
| B3, S3 |Maxi maal 814 MB/minuut per eenheid<br/>(1144,4 GB/dag/eenheid) |Gemiddeld aantal 208.333 berichten/minuut per eenheid<br/>(300.000.000 berichten/dag per eenheid) |

Een apparaat-naar-Cloud-door Voer is slechts een van de metrische gegevens die u moet overwegen bij het ontwerpen van een IoT-oplossing. Zie [IOT hub quota's en gashendel](iot-hub-devguide-quotas-throttling.md)voor meer informatie.

### <a name="identity-registry-operation-throughput"></a>Door Voer van identiteits register bewerking

IoT Hub-identiteits register bewerkingen moeten geen runtime bewerkingen zijn, aangezien deze voornamelijk betrekking hebben op het inrichten van apparaten.

Zie [IOT hub quota's en gashendel](iot-hub-devguide-quotas-throttling.md)voor specifieke burst-prestatie nummers.

## <a name="auto-scale"></a>Automatisch schalen

Als u de toegestane bericht limiet op uw IoT-hub nadert, kunt u deze [stappen gebruiken om automatisch te schalen](https://azure.microsoft.com/resources/samples/iot-hub-dotnet-autoscale/) om een IOT hub eenheid op hetzelfde IOT hub niveau te verhogen.

## <a name="next-steps"></a>Volgende stappen

* Zie [IOT hub prijzen](https://azure.microsoft.com/pricing/details/iot-hub) of [IOT hub quota's en](iot-hub-devguide-quotas-throttling.md)beperkingen voor meer informatie over IOT hub mogelijkheden en Details van de prestaties.

* Volg de stappen in [een upgrade van uw IOT-hub](iot-hub-upgrade.md)om uw IOT hub-laag te wijzigen.
