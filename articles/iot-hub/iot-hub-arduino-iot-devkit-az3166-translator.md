---
title: Translator met Azure Functions, Cognitive Services, IoT-DevKit maken
description: De microfoon gebruiken op een IoT-DevKit om een voicemail bericht te ontvangen en vervolgens Azure Cognitive Services te gebruiken voor het verwerken van de tekst in de Engelse taal
author: liydu
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.tgt_pltfrm: arduino
ms.date: 12/19/2018
ms.author: liydu
ms.openlocfilehash: 8e840a1ae7161ea3e7b370889a1f0fb648ca120e
ms.sourcegitcommit: 44c2a964fb8521f9961928f6f7457ae3ed362694
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/12/2019
ms.locfileid: "73953360"
---
# <a name="use-iot-devkit-az3166-with-azure-functions-and-cognitive-services-to-make-a-language-translator"></a>IoT DevKit AZ3166 met Azure Functions en Cognitive Services gebruiken om een taal vertaler te maken

In dit artikel leert u hoe u IoT DevKit als een taal conversie maakt met behulp van [Azure Cognitive Services](https://azure.microsoft.com/services/cognitive-services/). Het registreert uw stem en zet deze om in Engelse tekst die op het scherm DevKit wordt weer gegeven.

[MXChip IOT DevKit](https://aka.ms/iot-devkit) is een alles-in-een Arduino-compatibel bord met rijke rand apparatuur en Sens oren. U kunt dit ontwikkelen met behulp van [Azure IOT Device Workbench](https://aka.ms/iot-workbench) of [Azure IOT tools](https://aka.ms/azure-iot-tools) Extension Pack in Visual Studio code. De [catalogus projecten](https://microsoft.github.io/azure-iot-developer-kit/docs/projects/) bevat voorbeeld toepassingen die u helpen bij het prototypen van IOT-oplossingen.

## <a name="before-you-begin"></a>Voordat u begint

Voer eerst de volgende taken uit om de stappen in deze zelf studie uit te voeren:

* Bereid uw DevKit voor door de stappen in [Connect IOT DEVKIT AZ3166 te volgen op Azure IOT hub in de Cloud](/azure/iot-hub/iot-hub-arduino-iot-devkit-az3166-get-started).

## <a name="create-azure-cognitive-service"></a>Een Azure cognitieve service maken

1. Klik in de Azure Portal op **een resource maken** en zoek naar **spraak**. Vul het formulier in om de spraak service te maken.
  ![Speech-Service](media/iot-hub-arduino-iot-devkit-az3166-translator/speech-service.png)

1. Ga naar de spraak service die u zojuist hebt gemaakt, klik op de sectie **sleutels** om de **key1** voor DevKit toegang tot deze te kopiëren en te noteren.
  sleutels ![kopiëren](media/iot-hub-arduino-iot-devkit-az3166-translator/copy-keys.png)

## <a name="open-sample-project"></a>Voorbeeld project openen

1. Zorg ervoor dat uw IoT-DevKit **niet is verbonden** met uw computer. Start VS code eerst en sluit de DevKit aan op uw computer.

1. Klik op `F1` om het opdracht palet te openen, typ en selecteer **Azure IOT Device Workbench: voor beelden openen...** . Selecteer vervolgens **IOT DevKit** as Board.

1. Ga in de pagina met IoT Workbench-voor beelden naar **DevKit Translator** en klik op voor **Beeld openen**. Selecteert vervolgens het standaardpad voor het downloaden van de voorbeeld code.
  ![voorbeeld](media/iot-hub-arduino-iot-devkit-az3166-translator/open-sample.png) openen

## <a name="use-speech-service-with-azure-functions"></a>Speech Service gebruiken met Azure Functions

1. In VS code, klikt u op `F1`, typt en selecteert u **Azure IOT Device Workbench: Azure-Services inrichten...** . ![het inrichten van Azure-Services](media/iot-hub-arduino-iot-devkit-az3166-translator/provision.png)

1. Volg de stappen voor het volt ooien van de inrichting van Azure IoT Hub en Azure Functions.
   stappen voor ![inrichten](media/iot-hub-arduino-iot-devkit-az3166-translator/provision-steps.png)

   Noteer de naam van het Azure IoT Hub-apparaat dat u hebt gemaakt.

1. Open `Functions\DevKitTranslatorFunction.cs` en werk de volgende regels code bij met de apparaatnaam en de Speech Service-sleutel die u hebt genoteerd.
   ```csharp
   // Subscription Key of Speech Service
   const string speechSubscriptionKey = "";

   // Region of the speech service, see https://docs.microsoft.com/azure/cognitive-services/speech-service/regions for more details.
   const string speechServiceRegion = "";

   // Device ID
   const string deviceName = "";
   ```

1. Klik op `F1`, typ en selecteer **Azure IOT Device Workbench: implementeren naar Azure...** . Als VS code vraagt om bevestiging voor het opnieuw implementeren, klikt u op **Ja**.
   ](media/iot-hub-arduino-iot-devkit-az3166-translator/deploy-warning.png) waarschuwing ![implementeren

1. Controleer of de implementatie is geslaagd.
   ![implementatie is voltooid](media/iot-hub-arduino-iot-devkit-az3166-translator/deploy-success.png)

1. Ga in Azure Portal naar **functies apps** sectie en zoek de Azure function-app die u zojuist hebt gemaakt. Klik op `devkit_translator`en klik vervolgens op **</> functie-URL ophalen** om de URL te kopiëren.
   functie-URL van ![kopiëren](media/iot-hub-arduino-iot-devkit-az3166-translator/get-function-url.png)

1. Plak de URL in `azure_config.h` bestand.
   ![Azure-configuratie](media/iot-hub-arduino-iot-devkit-az3166-translator/azure-config.png)

   > [!NOTE]
   > Als de functie-app niet goed werkt, raadpleegt u deze sectie met [Veelgestelde vragen](https://microsoft.github.io/azure-iot-developer-kit/docs/faq#compilation-error-for-azure-function) om het probleem op te lossen.

## <a name="build-and-upload-device-code"></a>Apparaatcode bouwen en uploaden

1. Schakel de DevKit over naar de **configuratie modus** :
   * Houd **de**knop ingedrukt.
   * Knop **opnieuw instellen** indrukken en loslaten.

   Het scherm wordt weer gegeven met de DevKit-ID en- **configuratie**.

   ![Configuratie modus DevKit](media/iot-hub-arduino-iot-devkit-az3166-translator/devkit-configuration-mode.png)

1. Klik op `F1`, typ en selecteer **Azure IOT Device Workbench: Apparaatinstellingen configureren... Verbindings reeks voor configuratie apparaat >** . Selecteer **IOT hub apparaat verbindings reeks selecteren** om deze te configureren voor de DevKit.
   connection string](media/iot-hub-arduino-iot-devkit-az3166-translator/configure-connection-string.png) ![configureren

1. U ziet de melding zodra deze is voltooid.
   ![connection string geslaagde](media/iot-hub-arduino-iot-devkit-az3166-translator/configure-connection-string-success.png) configureren

1. Klik nogmaals op `F1`, typ en selecteer **Azure IOT Device Workbench: code van het apparaat uploaden**. De code wordt nu gecompileerd en geüpload naar DevKit.
   ![apparaat uploaden](media/iot-hub-arduino-iot-devkit-az3166-translator/device-upload.png)

## <a name="test-the-project"></a>Het project testen

Volg de instructies in het scherm DevKit na de initialisatie van de app. De standaard taal voor de bron is Chinees.

Een andere taal voor de vertaling selecteren:

1. Druk op de knop om de installatie modus in te voeren.

2. Druk op de knop B om alle ondersteunde bron talen te schuiven.

3. Druk op de knop om te bevestigen dat u de bron taal hebt gekozen.

4. Houd de knop B ingedrukt terwijl u spreekt en klik vervolgens op de knop release B om de vertaling te initiëren.

5. De vertaalde tekst in het Engels wordt weer gegeven op het scherm.

![Schuiven om taal te selecteren](media/iot-hub-arduino-iot-devkit-az3166-translator/select-language.jpg)

![Vertaal resultaat](media/iot-hub-arduino-iot-devkit-az3166-translator/translation-result.jpg)

In het scherm Vertaal resultaat kunt u het volgende doen:

- Druk op de knoppen A en B om de bron taal te schuiven en te selecteren.

- Druk op de knop B om te praten. Als u de stem wilt verzenden en de tekst van de vertaling wilt ontvangen, laat u de B-knop los.

## <a name="how-it-works"></a>How it works (Engelstalig artikel)

![mini-solution-voice-to-tweet-diagram](media/iot-hub-arduino-iot-devkit-az3166-translator/diagram.png)

De IoT DevKit registreert uw stem en plaatst vervolgens een HTTP-aanvraag om Azure Functions te activeren. Azure Functions roept de cognitieve service speech Translator API aan om de vertaling uit te voeren. Nadat Azure Functions de tekst van de vertaling hebt opgehaald, wordt een C2D-bericht naar het apparaat verzonden. Vervolgens wordt de vertaling op het scherm weer gegeven.

## <a name="problems-and-feedback"></a>Problemen en feedback

Als u problemen ondervindt, raadpleeg dan de [Veelgestelde vragen over IOT DevKit](https://microsoft.github.io/azure-iot-developer-kit/docs/faq/) of neem contact op met de volgende kanalen:

* [Gitter.im](https://gitter.im/Microsoft/azure-iot-developer-kit)
* [Stack Overflow](https://stackoverflow.com/questions/tagged/iot-devkit)

## <a name="next-steps"></a>Volgende stappen

U hebt geleerd hoe u de IoT DevKit als een vertaler kunt gebruiken met behulp van Azure Functions en Cognitive Services. In deze procedure hebt u het volgende geleerd:

> [!div class="checklist"]
> * Visual Studio code Task gebruiken om Cloud voorzieningen te automatiseren
> * Azure IoT-apparaat connection string configureren
> * De Azure-functie implementeren
> * De vertaling van spraak berichten testen

Ga naar de andere zelf studies voor meer informatie over:

> [!div class="nextstepaction"]
> [Een verbinding maken tussen IoT DevKit AZ3166 en de oplossings versneller voor externe controle van Azure IoT](https://docs.microsoft.com/azure/iot-hub/iot-hub-arduino-iot-devkit-az3166-devkit-remote-monitoring)
