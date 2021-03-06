---
title: Wat is Azure IoT Central? | Microsoft Docs
description: Azure IoT Central is een IoT-toepassingsplatform waarmee het eenvoudiger is om IoT-oplossingen te maken. In dit artikel vindt u een overzicht van de functies van Azure IoT Central.
author: dominicbetts
ms.author: dobett
ms.date: 08/26/2019
ms.topic: overview
ms.service: iot-central
services: iot-central
ms.custom: mvc
manager: timlt
ms.openlocfilehash: a1aa2f12f62a95ac14750c821079df2bac46e8ac
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/25/2019
ms.locfileid: "75434973"
---
<!---
Purpose of an Overview article: 
1. To give a TECHNICAL overview of a service/product: What is it? Why should I use it? It's a "learn" topic that describes key benefits and our competitive advantage. It's not a "do" topic.
2. To help audiences who are new to service but who may be familiar with related concepts. 
3. To compare the service to another service/product that has some similar functionality, ex. SQL Database / SQL Data Warehouse, if appropriate. This info can be in a short list or table. 
-->

# <a name="what-is-azure-iot-central"></a>Wat is Azure IoT Central?

[!INCLUDE [iot-central-original-pnp](../../../includes/iot-central-original-pnp-note.md)]

Azure IoT Central is een IoT app-platform waarmee de belasting en de kosten voor het ontwikkelen, beheren en onderhouden van IoT-oplossingen op bedrijfs niveau worden verminderd. Als u ervoor kiest om met Azure IoT Central te bouwen, kunt u zich richten op uw tijd, geld en energie over het trans formatie van uw bedrijf met IoT-gegevens, in plaats van alleen maar een complexe en voortdurend veranderende IoT-infra structuur te onderhouden en bij te werken.

Met de gebruiks vriendelijke interface kunt u eenvoudig de voor waarden van apparaten bewaken, regels maken en miljoenen apparaten en hun gegevens in hun levens cyclus beheren. Daarnaast kunt u hiermee op Device Insights reageren door IoT Intelligence uit te breiden in line-of-business-toepassingen.

Dit artikel bevat een overzicht van Azure IoT Central:

- De typische persona's die aan een project zijn gekoppeld.
- Het maken van uw toepassing.
- Het maken van verbinding tussen uw apparaten en uw toepassing
- Het beheren van uw toepassing.

## <a name="personas"></a>Persona's

De documentatie van Azure IoT Central verwijst naar vier personen die communiceren met een Azure IoT Central-toepassing:

- Een _maker_ is verantwoordelijk voor het definiëren van de typen apparaten die verbinding maken met de toepassing en voor het aanpassen van de toepassing voor de operator.
- Een _operator_ beheert de apparaten die met de toepassing zijn verbonden.
- Een _beheerder_ is verantwoordelijk voor taken zoals het beheren van [gebruikers rollen en machtigingen](howto-administer.md) binnen de toepassing.
- Een _apparaatontwikkelaar_ maakt de code die wordt uitgevoerd op een apparaat dat met uw toepassing is verbonden.

## <a name="create-your-azure-iot-central-application"></a>Uw Azure IoT Central-toepassing maken

Als maker gebruikt u Azure IoT Central om een aangepaste, in de cloud gehoste IoT-oplossing voor uw organisatie te maken. Een aangepaste IoT-oplossing bestaat meestal uit:

- Een cloudtoepassing die telemetriegegevens van uw apparaten ontvangt en die u in staat stelt om deze apparaten te beheren.
- Meerdere apparaten waarop aangepaste code wordt uitgevoerd en die met uw cloudtoepassing zijn verbonden.

U kunt snel een nieuwe Azure IoT Central-toepassing implementeren en deze vervolgens aanpassen aan uw specifieke vereisten in uw browser. Als ontwerper gebruikt u de webgebaseerde hulpprogram ma's voor het maken van een _apparaatprofiel_ voor de apparaten die verbinding maken met uw toepassing. Een sjabloon voor een apparaat is de blauw druk waarmee de kenmerken en het gedrag van een type apparaat worden gedefinieerd, zoals:

- Telemetrie verzendt.
- Bedrijfseigenschappen die kunnen worden gewijzigd door een operator.
- Apparaateigenschappen die zijn ingesteld door een apparaat en die het kenmerk alleen-lezen hebben in de toepassing.
- De drempel waarden die de toepassing beantwoordt.
- Instellingen die het gedrag van het apparaat bepalen.

U kunt uw apparaatsjablonen en uw toepassing direct testen met gesimuleerde gegevens die Azure IoT Central voor u genereert.

Als maker kunt u de gebruikersinterface van de Azure IoT Central-toepassing ook aanpassen voor de operators die verantwoordelijk zijn voor het dagelijks gebruik van de toepassing. Voorbeelden van aanpassingen die een maker kan maken, zijn:

- Het definiëren van de indeling van eigenschappen en instellingen in een apparaatsjabloon.
- Het configureren van aangepaste dashboards, zodat operators nieuwe inzichten kunnen krijgen en problemen sneller kunnen oplossen.
- Het configureren van aangepaste analyses om tijdseriegegevens van uw verbonden apparaten te verkennen.

## <a name="connect-your-devices"></a>Uw apparaten verbinden

Nadat de maker heeft gedefinieerd welke typen apparaten verbinding kunnen maken met de toepassing, maakt een apparaatontwikkelaar de code die op de apparaten moet worden uitgevoerd. Als apparaatontwikkelaar gebruikt u de [Azure IoT SDK's](https://github.com/Azure/azure-iot-sdks) van Microsoft, die open-source zijn, om de code voor uw apparaat te maken. Deze SDK's hebben uitgebreide taal-, platform- en protocolondersteuning, zodat ze kunnen voldoen aan uw behoeften bij het verbinden van uw apparaten met uw Azure IoT Central-toepassing. De Sdk's helpen u bij het implementeren van de volgende mogelijkheden voor apparaten:

- Een beveiligde verbinding maken.
- Telemetrie verzenden.
- Status melden.
- Configuratie-updates ontvangen.

Lees de blogpost [Benefits of using the Azure IoT SDKs, and pitfalls to avoid if you don't](https://azure.microsoft.com/blog/benefits-of-using-the-azure-iot-sdks-in-your-azure-iot-solution/) (Voordelen van het gebruik van de Azure IoT SDK's en valkuilen die u moet vermijden als u deze niet gebruikt) voor meer informatie.

## <a name="manage-your-application"></a>Uw toepassing beheren

Azure IoT Central-toepassingen worden volledig gehost door Microsoft, waardoor de overheadkosten voor het beheer van uw toepassingen worden verlaagd.

Als operator gebruikt u de Azure IoT Central-toepassing om de apparaten in uw Azure IoT Central-oplossing te beheren. Opera tors doen taken zoals:

- Het controleren van de apparaten die met de toepassing zijn verbonden.
- Het oplossen en verhelpen van problemen met apparaten.
- Het inrichten van nieuwe apparaten.

Als opbouw functie kunt u aangepaste regels en acties definiëren die worden uitgevoerd via het streamen van gegevens vanaf verbonden apparaten. Een operator kan deze regels op apparaatniveau in- of uitschakelen om taken binnen de toepassing te beheren en automatiseren.

Beheerders beheren de toegang tot uw toepassing met [gebruikers rollen en machtigingen](howto-administer.md).

## <a name="next-steps"></a>Volgende stappen

Nu u een overzicht van Azure IoT Central hebt, zijn dit mogelijke volgende stappen:

- Meer informatie over de beschik bare [Azure-technologieën en-services voor het maken van IOT-oplossingen](../../iot-fundamentals/iot-services-and-technologies.md).
- Raak vertrouwd met de [gebruikersinterface van Azure IoT Central](overview-iot-central-tour.md).
- Ga aan de slag door [een Azure IoT Central-toepassing te maken](quick-deploy-iot-central.md).
- Volg een reeks zelfstudies die u leren hoe u:
  - [Als maker een apparaatsjabloon maakt](tutorial-define-device-type.md)
  - [Als maker regels toevoegt om uw oplossing te automatiseren](tutorial-configure-rules.md)
  - [Als maker de toepassing aanpast voor uw operators](tutorial-customize-operator.md)
  - [Als operator uw apparaten bewaakt](tutorial-monitor-devices.md)
  - [Als operator een echt apparaat toevoegt aan uw oplossing](tutorial-add-device.md)
  - [Als apparaatontwikkelaar code voor uw apparaten maakt](tutorial-add-device.md#prepare-the-client-code)
