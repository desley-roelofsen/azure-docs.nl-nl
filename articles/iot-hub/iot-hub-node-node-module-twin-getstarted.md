---
title: Beginnen met de identiteit van de Azure IoT Hub-module & de module dubbele (node. js)
description: Meer informatie over het maken van module-identiteits-en update-modules, met behulp van IoT Sdk's voor node. js.
author: wesmc7777
manager: philmea
ms.author: wesmc
ms.service: iot-hub
services: iot-hub
ms.devlang: nodejs
ms.topic: conceptual
ms.date: 04/26/2018
ms.openlocfilehash: bf80925a2dc5c6d06ba14fe1c0d72a2fc1f88d51
ms.sourcegitcommit: 44c2a964fb8521f9961928f6f7457ae3ed362694
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/12/2019
ms.locfileid: "73953859"
---
# <a name="get-started-with-iot-hub-module-identity-and-module-twin-nodejs"></a>Aan de slag met IoT Hub module identiteit en module dubbele (node. js)

[!INCLUDE [iot-hub-selector-module-twin-getstarted](../../includes/iot-hub-selector-module-twin-getstarted.md)]

> [!NOTE]
> [Module-id's en moduledubbels](iot-hub-devguide-module-twins.md) zijn vergelijkbaar met Azure IoT Hub-apparaat-id's en apparaatdubbels, maar bieden een hogere granulariteit. Met een Azure IoT Hub-apparaat-id en apparaatdubbel kan een apparaat worden geconfigureerd via de back-endtoepassing en kunt u inzicht krijgen in de toestand van een apparaat. Een module-id en moduledubbel bieden deze mogelijkheden voor afzonderlijke onderdelen van een apparaat. Op apparaten met meerdere onderdelen, zoals apparaten met een besturingssysteem of firmware, kunt u afzonderlijke configuraties en voorwaarden voor elk onderdeel instellen.

Aan het einde van deze zelf studie hebt u twee node. js-apps:

* **CreateIdentities**: deze toepassing maakt een apparaat-id, een module-id en de bijbehorende beveiligingssleutel waarmee uw apparaat- en moduleclients verbonden kunnen worden.

* **UpdateModuleTwinReportedProperties**: deze toepassing verzendt bijgewerkte gerapporteerde eigenschappen van de moduledubbel naar uw IoT Hub.

> [!NOTE]
> Voor informatie over de Azure IoT-Sdk's die u kunt gebruiken om beide toepassingen te bouwen die op apparaten kunnen worden uitgevoerd en de back-end van uw oplossing, raadpleegt u [Azure IOT sdk's](iot-hub-devguide-sdks.md).

## <a name="prerequisites"></a>Vereisten

* Node. js versie 10.0. x of hoger. [Uw ontwikkel omgeving voorbereiden](https://github.com/Azure/azure-iot-sdk-node/tree/master/doc/node-devbox-setup.md) hierin wordt beschreven hoe u node. js installeert voor deze zelf studie op Windows of Linux.

* Een actief Azure-account. (Als u geen account hebt, kunt u in slechts een paar minuten een [gratis account](https://azure.microsoft.com/pricing/free-trial/) maken.)

## <a name="create-an-iot-hub"></a>Een IoT Hub maken

[!INCLUDE [iot-hub-include-create-hub](../../includes/iot-hub-include-create-hub.md)]

## <a name="get-the-iot-hub-connection-string"></a>De IoT hub-connection string ophalen

[!INCLUDE [iot-hub-howto-module-twin-shared-access-policy-text](../../includes/iot-hub-howto-module-twin-shared-access-policy-text.md)]

[!INCLUDE [iot-hub-include-find-registryrw-connection-string](../../includes/iot-hub-include-find-registryrw-connection-string.md)]

## <a name="create-a-device-identity-and-a-module-identity-in-iot-hub"></a>Een apparaat-id en een module-ID maken in IoT Hub

In deze sectie maakt u een node. js-app die een apparaat-id en een module-ID maakt in het identiteits register van uw IoT-hub. Een apparaat of module kan alleen verbinding maken met de IoT-hub als het apparaat of de module is vermeld in het identiteitsregister. Zie de sectie identiteits register van de [IOT hub-ontwikkelaars handleiding](iot-hub-devguide-identity-registry.md)voor meer informatie. Als u deze consoletoepassing uitvoert, worden er een unieke id en een unieke sleutel gemaakt voor zowel het apparaat als de module. Deze waarden worden door het apparaat en de module gebruikt om zichzelf te identificeren bij het verzenden van apparaat-naar-cloud-berichten naar IoT Hub. De id's zijn hoofdlettergevoelig.

1. Maak een directory waarin u uw code kunt opslaan.

2. Voer in de map eerst **NPM init-y** uit om een leeg pakket. json met de standaard waarden te maken. Dit is het project bestand voor uw code.

3. Voer **NPM install-S Azure-iothub\@modules-preview** uit om de Service-SDK in de submap **node_modules** te installeren.

    > [!NOTE]
    > De naam van de submap node_modules gebruikt de Word-module om "een knooppunt bibliotheek" te zeggen. De term hier heeft niets te maken met IoT Hub-modules.

4. Maak het volgende. js-bestand in de map. Noem het **toevoegen. js**. Kopieer en plak de hub connection string en de naam van de hub.

    ```javascript
    var Registry = require('azure-iothub').Registry;
    var uuid = require('uuid');
    // Copy/paste your connection string and hub name here
    var serviceConnectionString = '<hub connection string from portal>';
    var hubName = '<hub name>.azure-devices.net';
    // Create an instance of the IoTHub registry
    var registry = Registry.fromConnectionString(serviceConnectionString);
    // Insert your device ID and moduleId here.
    var deviceId = 'myFirstDevice';
    var moduleId = 'myFirstModule';
    // Create your device as a SAS authentication device
    var primaryKey = new Buffer(uuid.v4()).toString('base64');
    var secondaryKey = new Buffer(uuid.v4()).toString('base64');
    var deviceDescription = {
      deviceId: deviceId,
      status: 'enabled',
      authentication: {
        type: 'sas',
        symmetricKey: {
          primaryKey: primaryKey,
          secondaryKey: secondaryKey
        }
      }
    };

    // First, create a device identity
    registry.create(deviceDescription, function(err) {
      if (err) {
        console.log('Error creating device identity: ' + err);
        process.exit(1);
      }
      console.log('device connection string = "HostName=' + hubName + ';DeviceId=' + deviceId + ';SharedAccessKey=' + primaryKey + '"');

      // Then add a module to that device
      registry.addModule({ deviceId: deviceId, moduleId: moduleId }, function(err) {
        if (err) {
          console.log('Error creating module identity: ' + err);
          process.exit(1);
        }

        // Finally, retrieve the module details from the hub so we can construct the connection string
        registry.getModule(deviceId, moduleId, function(err, foundModule) {
          if (err) {
            console.log('Error getting module back from hub: ' + err);
            process.exit(1);
          }
          console.log('module connection string = "HostName=' + hubName + ';DeviceId=' + foundModule.deviceId + ';ModuleId='+foundModule.moduleId+';SharedAccessKey=' + foundModule.authentication.symmetricKey.primaryKey + '"');
          process.exit(0);
        });
      });
    });

    ```

Met deze app maakt u een apparaat-id met de naam **myFirstDevice** en een module-ID met id **MyFirstModule** onder apparaat **myFirstDevice**. (Als deze module-ID al bestaat in het identiteits register, haalt de code gewoon de bestaande module gegevens op.) In de app wordt vervolgens de primaire sleutel voor die identiteit weer gegeven. U gebruikt deze sleutel in de gesimuleerde module-app om verbinding te maken met uw IoT-hub.

Voer dit uit met behulp van node add. js. Hiermee krijgt u een connection string voor uw apparaat-id en een andere voor uw module-identiteit.

> [!NOTE]
> In het identiteitsregister van IoT Hub worden alleen apparaat- en module-id's opgeslagen waarmee veilig toegang tot de IoT-hub kan worden verkregen. In het identiteitsregister worden apparaat-id's en -sleutels opgeslagen die als beveiligingsreferenties worden gebruikt. In het identiteitsregister wordt ook een vlag ingeschakeld/uitgeschakeld voor elk apparaat opgeslagen die u kunt gebruiken om de toegang tot dat apparaat uit te schakelen. Als uw toepassing andere apparaatspecifieke metagegevens moet opslaan, moet deze een toepassingsspecifieke opslagmethode gebruiken. Er is geen vlag voor ingeschakeld/uitgeschakeld voor module-id's. Zie [IOT hub ontwikkelaars handleiding](iot-hub-devguide-identity-registry.md)voor meer informatie.

## <a name="update-the-module-twin-using-nodejs-device-sdk"></a>De module bijwerken met behulp van node. js apparaat SDK

In deze sectie maakt u een node. js-app op uw gesimuleerde apparaat die de module dubbele gerapporteerde eigenschappen bijwerkt.

1. **Uw module Connection String ophalen** : Meld u aan bij de [Azure Portal](https://portal.azure.com/). Navigeer naar uw IoT Hub en klik op IoT-apparaten. Zoek myFirstDevice, open het en controleer of myFirstModule is gemaakt. Kopieer de moduleverbindingsreeks. Deze is vereist voor de volgende stap.

   ![Details van de Azure Portal-module](./media/iot-hub-node-node-module-twin-getstarted/module-detail.png)

2. Net als bij de bovenstaande stap maakt u een map voor uw apparaatcode en gebruikt u NPM om deze te initialiseren en de SDK van het apparaat te installeren (**NPM install-S Azure-IOT-Device-amqp\@modules-preview**).

   > [!NOTE]
   > De NPM-installatie opdracht kan traag zijn. Het is een ogen blik dat u veel code uit de pakket opslagplaats haalt.

   > [!NOTE]
   > Als u een fout ziet met de tekst NPM ERR! register fout bij het parseren van JSON. Dit is veilig om te negeren. Als u een fout ziet met de tekst NPM ERR! register fout bij het parseren van JSON. Dit is veilig om te negeren.

3. Maak een bestand met de naam dubbele. js. Kopieer en plak uw module identiteits teken reeks.

    ```javascript
    var Client = require('azure-iot-device').Client;
    var Protocol = require('azure-iot-device-amqp').Amqp;
    // Copy/paste your module connection string here.
    var connectionString = '<insert module connection string here>';
    // Create a client using the Amqp protocol.
    var client = Client.fromConnectionString(connectionString, Protocol);
    client.on('error', function (err) {
      console.error(err.message);
    });
    // connect to the hub
    client.open(function(err) {
      if (err) {
        console.error('error connecting to hub: ' + err);
        process.exit(1);
      }
      console.log('client opened');
    // Create device Twin
      client.getTwin(function(err, twin) {
        if (err) {
          console.error('error getting twin: ' + err);
          process.exit(1);
        }
        // Output the current properties
        console.log('twin contents:');
        console.log(twin.properties);
        // Add a handler for desired property changes
        twin.on('properties.desired', function(delta) {
            console.log('new desired properties received:');
            console.log(JSON.stringify(delta));
        });
        // create a patch to send to the hub
        var patch = {
          updateTime: new Date().toString(),
          firmwareVersion:'1.2.1',
          weather:{
            temperature: 72,
            humidity: 17
          }
        };
        // send the patch
        twin.properties.reported.update(patch, function(err) {
          if (err) throw err;
          console.log('twin state reported');
        });
      });
    });
    ```

4. Voer dit nu uit met behulp van het opdracht **knooppunt dubbele. js**.

   ```cmd/sh
   F:\temp\module_twin>node twin.js
   ```

   U ziet dan het volgende:

   ```console
   client opened
   twin contents:
   { reported: { update: [Function: update], '$version': 1 },
     desired: { '$version': 1 } }
   new desired properties received:
   {"$version":1}
   twin state reported
   ```

## <a name="next-steps"></a>Volgende stappen

Als u aan de slag wilt gaan met IoT Hub en andere IoT-scenario's wilt verkennen, leest u deze artikelen:

* [Aan de slag met Apparaatbeheer](iot-hub-node-node-device-management-get-started.md)

* [Aan de slag met IoT Edge](../iot-edge/tutorial-simulate-device-linux.md)