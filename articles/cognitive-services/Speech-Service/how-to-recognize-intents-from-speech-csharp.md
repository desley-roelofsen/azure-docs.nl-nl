---
title: Intenties herkennen vanuit spraak met behulp van de Speech SDKC#
titleSuffix: Azure Cognitive Services
description: In deze hand leiding leert u hoe u de intenties kunt herkennen vanuit spraak met behulp C#van de Speech SDK voor.
services: cognitive-services
author: wolfma61
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 08/28/2019
ms.author: wolfma
ms.openlocfilehash: 554a7cbd79dbb6e1306686600474f727c99defed
ms.sourcegitcommit: 5aefc96fd34c141275af31874700edbb829436bb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/04/2019
ms.locfileid: "74805889"
---
# <a name="how-to-recognize-intents-from-speech-using-the-speech-sdk-for-c"></a>Intenties herkennen vanuit spraak met de Speech SDK voorC#

De Cognitive Services [Speech SDK](speech-sdk.md) integreert met de [Language Understanding-service (Luis)](https://www.luis.ai/home) om de **intentie**te kunnen herkennen. Een intentie is iets dat de gebruiker wil doen: een vlucht reserveren, de weersverwachting controleren of iemand bellen. De gebruiker kan elke term gebruiken die natuurlijk aanvoelt. Met behulp van machine learning wijst LUIS gebruikers aanvragen toe aan de intenties die u hebt gedefinieerd.

> [!NOTE]
> Een LUIS-toepassing definieert de intenties en entiteiten die u wilt herkennen. Deze toepassing staat los van de C#-toepassing die gebruikmaakt van de Speech-service. In dit artikel verwijst 'app' naar de LUIS-app en 'toepassing' naar de C#-code.

In deze hand leiding gebruikt u de Speech SDK voor het ontwikkelen C# van een console toepassing die intenties van gebruikers uitingen afleiden via de microfoon van uw apparaat. U leert het volgende:

> [!div class="checklist"]
>
> - Een Visual Studio-project maken dat verwijst naar het NuGet-pakket van de Speech SDK
> - Een spraak configuratie maken en een intentie herkenning verkrijgen
> - Het model voor uw LUIS-app ophalen en de benodigde intenties toevoegen
> - De taal voor spraakherkenning opgeven
> - Spraak herkennen uit een bestand
> - Asynchrone, gebeurtenisgestuurde continue herkenning gebruiken

## <a name="prerequisites"></a>Vereisten

Zorg ervoor dat u de volgende items hebt voordat u aan de slag gaat met deze hand leiding:

- Een LUIS-account. U kunt een gratis account aanvragen via het [LUIS portal](https://www.luis.ai/home).
- [Visual Studio 2019](https://visualstudio.microsoft.com/downloads/) (alle edities).

## <a name="luis-and-speech"></a>LUIS en spraakherkenning

LUIS kan worden geïntegreerd met de Speech-service voor het herkennen van intenties van spraak. U hebt geen abonnement op de Speech-service nodig, alleen op LUIS.

LUIS maakt gebruik van drie soorten sleutels:

| Type sleutel  | Doel                                               |
| --------- | ----------------------------------------------------- |
| Ontwerpen | Hiermee kunt u LUIS-apps programmatisch maken en wijzigen |
| Starter   | Hiermee kunt u uw LUIS-toepassing testen met alleen tekst   |
| Eindpunt  | Hiermee wordt toegang tot een bepaalde LUIS-app geautoriseerd            |

Voor deze hand leiding hebt u het sleutel type van het eind punt nodig. In deze hand leiding wordt gebruikgemaakt van de voor beeld-app Home Automation LUIS, die u kunt maken door de Snelstartgids voor het [gebruik van vooraf ontwikkelde Home Automation-apps](https://docs.microsoft.com/azure/cognitive-services/luis/luis-get-started-create-app) te volgen. Als u zelf een LUIS-app hebt gemaakt, kunt u deze gebruiken in plaats daarvan.

Wanneer u een LUIS-app maakt, wordt door LUIS automatisch een start sleutel gegenereerd zodat u de app kunt testen met behulp van tekst query's. Met deze sleutel wordt de integratie van de spraak service niet ingeschakeld en werkt deze niet met deze hand leiding. Maak een LUIS-resource in het Azure-dash board en wijs deze toe aan de LUIS-app. U kunt de gratis Subscription-laag voor deze hand leiding gebruiken.

Nadat u de LUIS-resource hebt gemaakt in het Azure-dash board, meldt u zich aan bij de [Luis-Portal](https://www.luis.ai/home), kiest u uw toepassing op de pagina **mijn apps** en gaat u naar de pagina **beheren** van de app. Ten slotte selecteert u de **toetsen en eind punten** in de zijbalk.

![Instellingen voor sleutels en eindpunten in de LUIS-portal](media/sdk/luis-keys-endpoints-page.png)

Op de pagina **sleutels en eindpunt instellingen** :

1. Schuif omlaag naar de sectie **resources en sleutels** en selecteer **resource toewijzen**.
1. Breng in het dialoog venster **een sleutel toewijzen aan uw app** de volgende wijzigingen aan:

   - Onder **Tenant**kiest u **micro soft**.
   - Kies onder **abonnements naam**het Azure-abonnement met de Luis-resource die u wilt gebruiken.
   - Kies onder **sleutel**de Luis-resource die u wilt gebruiken met de app.

   Na korte tijd wordt het nieuwe abonnement weergegeven in de tabel onderaan de pagina.

1. Selecteer het pictogram naast een sleutel om het naar het klem bord te kopiëren. (U kunt beide sleutels gebruiken.)

![Abonnementssleutels voor LUIS-app](media/sdk/luis-keys-assigned.png)

## <a name="create-a-speech-project-in-visual-studio"></a>Een spraakproject maken in Visual Studio

[!INCLUDE [Create project](../../../includes/cognitive-services-speech-service-create-speech-project-vs-csharp.md)]

## <a name="add-the-code"></a>Code toevoegen

Vervolgens voegt u code toe aan het project.

1. Open het bestand **Program.cs**van **Solution Explorer**.

1. Vervang het blok met `using`-instructies aan het begin van het bestand door de volgende declaraties:

   [!code-csharp[Top-level declarations](~/samples-cognitive-services-speech-sdk/samples/csharp/sharedcontent/console/intent_recognition_samples.cs#toplevel)]

1. Voeg in de gegeven `Main()` methode de volgende code toe:

   ```csharp
   RecognizeIntentAsync().Wait();
   Console.WriteLine("Please press Enter to continue.");
   Console.ReadLine();
   ```

1. Maak een lege, asynchrone methode `RecognizeIntentAsync()`, zoals hier wordt weer gegeven:

   ```csharp
   static async Task RecognizeIntentAsync()
   {
   }
   ```

1. Voeg in de hoofd tekst van deze nieuwe methode deze code toe:

   [!code-csharp[Intent recognition by using a microphone](~/samples-cognitive-services-speech-sdk/samples/csharp/sharedcontent/console/intent_recognition_samples.cs#intentRecognitionWithMicrophone)]

1. Vervang de tijdelijke aanduidingen in deze methode als volgt door de sleutel, regio en app-id van uw LUIS-abonnement.

   | Tijdelijke aanduiding | Vervangen door |
   | ----------- | ------------ |
   | `YourLanguageUnderstandingSubscriptionKey` | Uw LUIS-endpoint-sleutel Ook hier moet u dit item ophalen van uw Azure-dash board, niet van een ' Start sleutel '. U kunt deze vinden op de pagina **sleutels en eind punten** van uw app (onder **beheren**) in de [Luis-Portal](https://www.luis.ai/home). |
   | `YourLanguageUnderstandingServiceRegion` | De korte id voor de regio van uw LUIS-abonnement, zoals `westus` voor US - west. Zie [Regio's](regions.md). |
   | `YourLanguageUnderstandingAppId` | De id van de LUIS-app. U vindt deze in de [Luis-Portal](https://www.luis.ai/home)op de pagina **instellingen** van uw app. |

Als deze wijzigingen zijn aangebracht, kunt u maken (**Control + Shift + B**) en de toepassing (**F5**) uitvoeren. Wanneer u hierom wordt gevraagd, kunt u ' de lampjes uitschakelen ' in de microfoon van uw PC zeggen. In de toepassing wordt het resultaat weer gegeven in het console venster.

In de volgende secties wordt dieper ingegaan op de code.

## <a name="create-an-intent-recognizer"></a>Een mechanisme voor intentieherkenning maken

Eerst moet u een spraak configuratie maken op basis van uw LUIS-eindpunt sleutel en-regio. U kunt spraak configuraties gebruiken om herkennings functies te maken voor de verschillende mogelijkheden van de spraak-SDK. De spraak configuratie heeft meerdere manieren om het abonnement op te geven dat u wilt gebruiken. hier gebruiken we `FromSubscription`, die de sleutel en de regio van het abonnement neemt.

> [!NOTE]
> Gebruik de sleutel en regio van uw LUIS-abonnement, niet van een spraak service-abonnement.

Maak vervolgens een mechanisme voor intentieherkenning met behulp van `new IntentRecognizer(config)`. Aangezien de configuratie al kent welk abonnement moet worden gebruikt, hoeft u de abonnements sleutel en het eind punt niet opnieuw op te geven bij het maken van de herkenner.

## <a name="import-a-luis-model-and-add-intents"></a>Een LUIS-model importeren en intenties toevoegen

Importeer het model nu uit de LUIS-app met `LanguageUnderstandingModel.FromAppId()` en voeg de intenties van LUIS toe die u wilt herkennen via de methode `AddIntent()` van het mechanisme. Deze twee stappen verbeteren de nauwkeurigheid van spraakherkenning door woorden aan te geven die de gebruiker waarschijnlijk zal gebruiken in aanvragen. U hoeft niet alle intenties van de app toe te voegen als u ze niet allemaal in uw toepassing hoeft te herkennen.

Als u intenties wilt toevoegen, moet u drie argumenten opgeven: het LUIS-model (dat is gemaakt met de naam `model`), de naam van de doel groep en een intentie-ID. Het verschil tussen de id en de naam is als volgt.

| argument `AddIntent()`&nbsp; | Doel |
| --------------------------- | ------- |
| `intentName` | De naam van de intentie zoals gedefinieerd in de LUIS-app. Deze waarde moet exact overeenkomen met de naam van het LUIS-doel. |
| `intentID` | Een id die door de Speech SDK wordt toegewezen aan een herkende intentie. Deze waarde kan wille keurig zijn. het hoeft niet overeen te komen met de naam van de doel groep zoals gedefinieerd in de LUIS-app. Als in dezelfde code bijvoorbeeld meerdere intenties worden afgehandeld, kun u dezelfde id voor de intenties gebruiken. |

De Home Automation LUIS-app heeft twee intenties: één om een apparaat in te scha kelen en een ander voor het uitschakelen van een apparaat. Met de onderstaande regels worden deze intenties toegevoegd aan het mechanisme voor intentieherkenning. Vervang de drie regels `AddIntent` in de methode `RecognizeIntentAsync()` door deze code.

```csharp
recognizer.AddIntent(model, "HomeAutomation.TurnOff", "off");
recognizer.AddIntent(model, "HomeAutomation.TurnOn", "on");
```

In plaats van afzonderlijke intenties toe te voegen, kunt u ook de methode `AddAllIntents` gebruiken om alle intenties in een model toe te voegen aan de herkenner.

## <a name="start-recognition"></a>Herkenning starten

Het mechanisme voor intentieherkenning is gemaakt en de intenties zijn toegevoegd. We kunnen dus het proces van herkenning starten. De Speech SDK biedt ondersteuning voor herkenning van zowel afzonderlijke opnamen als continue herkenning.

| Herkennings-modus | Methoden om aan te roepen | Resultaat |
| ---------------- | --------------- | ------ |
| Eén opname | `RecognizeOnceAsync()` | Retourneert de herkende intentie, indien aanwezig, na één utterance. |
| Continu | `StartContinuousRecognitionAsync()`<br>`StopContinuousRecognitionAsync()` | Herkent meerdere uitingen; verzendt gebeurtenissen (bijvoorbeeld `IntermediateResultReceived`) wanneer er resultaten beschikbaar zijn. |

De toepassing maakt gebruik van de modus voor eenmalige opname en roept daarom `RecognizeOnceAsync()` aan om te beginnen met herkenning. Het resultaat is een `IntentRecognitionResult`-object met informatie over de herkende intentie. U haalt het LUIS JSON-antwoord op met behulp van de volgende expressie:

```csharp
result.Properties.GetProperty(PropertyId.LanguageUnderstandingServiceResponse_JsonResult)
```

De toepassing parseert het JSON-resultaat niet. De JSON-tekst wordt alleen weer gegeven in het console venster.

![Enkele LUIS herkennings resultaten](media/sdk/luis-results.png)

## <a name="specify-recognition-language"></a>Herkenningstaal opgeven

Standaard herkent LUIS intenties in het Amerikaans-Engels (`en-us`). U kunt een landinstellingscode toewijzen aan de eigenschap `SpeechRecognitionLanguage` van de spraakconfiguratie om intenties in andere talen te herkennen. U kunt bijvoorbeeld `config.SpeechRecognitionLanguage = "de-de";` toevoegen in de toepassing voordat u de herkenner maakt om de intenties in het Duits te herkennen. Zie [ondersteunde talen](language-support.md#speech-to-text)voor meer informatie.

## <a name="continuous-recognition-from-a-file"></a>Continue herkenning uit een bestand

De volgende code illustreert twee aanvullende mogelijkheden van intentieherkenning met behulp van de Speech SDK. De eerste mogelijkheid, die eerder is genoemd, is continue spraakherkenning. Hierbij verzendt het mechanisme voor intentieherkenning gebeurtenissen wanneer er resultaten beschikbaar zijn. Deze gebeurtenissen kunnen vervolgens worden verwerkt door gebeurtenis-handlers die u opgeeft. Met doorlopende herkenning roept u de `StartContinuousRecognitionAsync()` methode van de Recognizer aan om herkenning te starten in plaats van `RecognizeOnceAsync()`.

De andere mogelijkheid is het lezen van de audio met de spraak die moet worden verwerkt uit een WAV-bestand. Implementatie omvat het maken van een audio configuratie die kan worden gebruikt bij het maken van de intentie herkenning. Het bestand moet éénkanaals (mono) zijn met een samplefrequentie van 16 kHz.

Als u deze functies wilt uitproberen, verwijdert u de hoofd tekst van de `RecognizeIntentAsync()` methode en voegt u de volgende code toe.

[!code-csharp[Intent recognition by using events from a file](~/samples-cognitive-services-speech-sdk/samples/csharp/sharedcontent/console/intent_recognition_samples.cs#intentContinuousRecognitionWithFile)]

Wijzig de code met uw LUIS-eindpuntsleutel, regio en app-id en voeg de intenties van Home Automation toe, zoals u eerder hebt gedaan. Wijzig `whatstheweatherlike.wav` in de naam van het opgenomen audio bestand. Vervolgens bouwt u het audio bestand naar de map build en voert u de toepassing uit.

Als u bijvoorbeeld ' verlichting uitschakelen ' hebt uitgeschakeld, de lamp ' pauzeren ' inschakelt ' in het opgenomen audio bestand, kan de console-uitvoer er ongeveer als volgt uitzien:

![LUIS herkennings resultaten van audio bestand](media/sdk/luis-results-2.png)

[!INCLUDE [Download the sample](../../../includes/cognitive-services-speech-service-speech-sdk-sample-download-h2.md)]
Zoek naar de code in dit artikel in de map **samples/csharp/sharedcontent/console** .

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Snelstartgids: spraak van een microfoon herkennen](~/articles/cognitive-services/Speech-Service/quickstarts/speech-to-text-from-microphone.md?pivots=programming-language-csharp&tabs=dotnetcore)
