---
title: Wat is de Speech-service?
titleSuffix: Azure Cognitive Services
description: De speech-service is de combineert van spraak naar tekst, tekst naar spraak en spraak omzetting in één Azure-abonnement. Voeg spraak toe aan uw toepassingen, hulpprogram ma's en apparaten met de Speech SDK, de speech-apparaten SDK of REST Api's.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: overview
ms.date: 11/05/2019
ms.author: erhopf
ms.openlocfilehash: c366beb80eda7087f1f74fffbcfbf8b143676f32
ms.sourcegitcommit: d614a9fc1cc044ff8ba898297aad638858504efa
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/10/2019
ms.locfileid: "74995895"
---
# <a name="what-is-the-speech-service"></a>Wat is de Speech-service?

De speech-service is het combineert van spraak naar tekst, tekst naar spraak en spraak omzetting in één Azure-abonnement. Het is eenvoudig om uw toepassingen, hulpprogram ma's en apparaten met de spraak- [SDK](speech-sdk-reference.md), [Speech-apparaten SDK](https://aka.ms/sdsdk-quickstart)of [rest api's](rest-apis.md)in te scha kelen.

> [!IMPORTANT]
> De speech-service is Bing Speech-API, Translator Speech en Custom Speech vervangen. Raadpleeg de _hand leidingen > migratie_ voor migratie-instructies.

Deze functies vormen de spraak service. Gebruik de koppelingen in deze tabel voor meer informatie over veelvoorkomende use cases voor elke functie of blader door de API-verwijzing.

| Service | Functie | Beschrijving | SDK | REST |
| ------- | ------- | ----------- | --- | ---- |
| [Spraak naar tekst](speech-to-text.md) | Spraak naar tekst | Met spraak naar tekst worden audio stromen naar tekst getranscribeerd in realtime die uw toepassingen, hulpprogram ma's of apparaten kunnen gebruiken of weer geven. Gebruik spraak-naar-tekst met [Language Understanding (Luis)](https://docs.microsoft.com/azure/cognitive-services/luis/) om gebruikers intentie af te leiden van transcribed speech en Act on Voice Commands. | [Ja](https://docs.microsoft.com/azure/cognitive-services/speech-service/speech-sdk-reference) | [Ja](https://docs.microsoft.com/azure/cognitive-services/speech-service/rest-apis) |
|         | [Batch-transcriptie](batch-transcription.md) | Batch transcriptie maakt asynchrone spraak-naar-tekst-transcriptie met grote hoeveel heden gegevens mogelijk. Dit is een REST-gebaseerde service, die hetzelfde eind punt gebruikt als aanpassing en model beheer. | Nee | [Ja](https://westus.cris.ai/swagger/ui/index) |
|         | [Gesprek transcriptie](conversation-transcription-service.md) | Hiermee schakelt u realtime spraak herkenning, identificatie van de spreker en diarization in. Het is ideaal voor het overzetten van persoonlijke vergaderingen met de mogelijkheid om de luid sprekers te onderscheiden. | Ja | Nee |
|         | [Custom Speech modellen maken](#customize-your-speech-experience) | Als u spraak-naar-tekst gebruikt voor herkenning en transcriptie in een unieke omgeving, kunt u aangepaste akoestische, taal en uitspraak modellen maken en trainen om omgevings lawaai of branchespecifieke woorden lijsten te verhelpen. | Nee | [Ja](https://westus.cris.ai/swagger/ui/index) |
| [Tekst naar spraak](text-to-speech.md) | Tekst naar spraak | Tekst-naar-spraak zet invoer tekst om in Human-achtige, geprogrammeerde spraak met behulp van [SSML (Speech synthese Markup Language)](text-to-speech.md#speech-synthesis-markup-language-ssml). Kies uit standaard stemmen en Neural stemmen (Zie [taal ondersteuning](language-support.md)). | [Ja](https://docs.microsoft.com/azure/cognitive-services/speech-service/speech-sdk-reference) | [Ja](https://docs.microsoft.com/azure/cognitive-services/speech-service/rest-apis) |
|         | [Aangepaste stemmen maken](#customize-your-speech-experience) | Aangepaste spraak lettertypen maken die uniek zijn voor uw merk of product. | Nee | [Ja](https://docs.microsoft.com/azure/cognitive-services/speech-service/rest-apis) |
| [Spraakomzetting](speech-translation.md) | Spraakomzetting | Met spraak omzetting kan spraak op meerdere talen worden omgezet in uw toepassingen, hulpprogram ma's en apparaten. Gebruik deze service voor conversie van spraak naar spraak en spraak naar tekst. | [Ja](https://docs.microsoft.com/azure/cognitive-services/speech-service/speech-sdk-reference) | Nee |
| [Spraak assistenten](voice-assistants.md) | Spraakassistenten | Met de spraak service kunnen ontwikkel aars natuurlijke, menselijke-achtige gespreks interfaces maken voor hun toepassingen en ervaringen. De Voice Assistant-service biedt een snelle, betrouw bare interactie tussen een apparaat en een assistent-implementatie die gebruikmaakt van het directe lijn spraak kanaal van het bot-Framework of de geïntegreerde service voor het uitvoeren van aangepaste opdrachten (preview) voor taak voltooiing. | [Ja](voice-assistants.md) | Nee |

## <a name="news-and-updates"></a>Nieuws en updates

Meer informatie over wat er nieuw is in de speech service.

- September 2019
  - 1\.7.0 van vrijgegeven spraak-SDK. Zie [release opmerkingen](releasenotes.md)voor een volledige lijst met updates, verbeteringen en bekende problemen.
- Augustus 2019
  - **Nieuwe zelf studie**: [stem uw bot in met de Speech SDK C# ,](tutorial-voice-enable-your-bot-speech-sdk.md)
  - Er is voor de `en-US-JessaNeural` spraak een nieuwe spraak stijl, [`chat`](speech-synthesis-markup.md#adjust-speaking-styles)toegevoegd.
- Juni 2019
  - 1\.6.0 van vrijgegeven spraak-SDK. Zie [release opmerkingen](releasenotes.md)voor een volledige lijst met updates, verbeteringen en bekende problemen.
- Mei 2019-documentatie is nu beschikbaar voor [transcriptie](conversation-transcription-service.md), [Call Center transcriptie](call-center-transcription.md)en [Voice Assistants](voice-assistants.md).
- Mei 2019
  - 1\.5.1 van vrijgegeven spraak-SDK. Zie [release opmerkingen](releasenotes.md)voor een volledige lijst met updates, verbeteringen en bekende problemen.
  - 1\.5.0 van vrijgegeven spraak-SDK. Zie [release opmerkingen](releasenotes.md)voor een volledige lijst met updates, verbeteringen en bekende problemen.

## <a name="try-the-speech-service"></a>Probeer de speech-service

We bieden Quick starts in de populairste programmeer talen, die allemaal ontworpen zijn voor het uitvoeren van code in minder dan 10 minuten. Deze tabel bevat de populairste Quick starts voor elke functie. Gebruik de linkernavigatiebalk om extra talen en platformen te verkennen.

| Spraak naar tekst (SDK) | Tekst-naar-spraak (SDK) | Vertaling (SDK) |
| -------------------- | -------------------- | ----------------- |
| [Spraak herkennen vanuit een audio bestand](quickstarts/speech-to-text-from-file.md) | [Spraak samen te brengen in een audio bestand](quickstarts/text-to-speech-audio-file.md) | [Spraak omzetten naar tekst](quickstarts/translate-speech-to-text.md) |
| [Spraak herkennen met een microfoon](quickstarts/speech-to-text-from-microphone.md) | [Spraak op een spreker bekunsten](quickstarts/text-to-speech.md) | [Spraak omzetten naar meerdere doel talen](quickstarts/translate-speech-to-text-multiple-languages.md) |
| [Spraak herkennen die zijn opgeslagen in Blob Storage](quickstarts/from-blob.md) | [Asynchrone synthese voor lange-vorm audio](quickstarts/text-to-speech/async-synthesis-long-form-audio.md) | [Spraak-naar-spraak omzetten](quickstarts/translate-speech-to-speech.md) |

> [!NOTE]
> Spraak-naar-tekst en tekst-naar-spraak hebben ook REST-eind punten en bijbehorende Quick starts.

Nadat u de spraak service hebt bekeken, kunt u de zelf studie uitproberen die u leert hoe u de intenties kunt herkennen vanuit spraak met behulp van de Speech SDK en LUIS.

- [Zelf studie: intenties herkennen vanuit spraak met de Speech SDK en LUIS,C#](how-to-recognize-intents-from-speech-csharp.md)
- [Zelf studie: stem uw bot in met de Speech SDK,C#](tutorial-voice-enable-your-bot-speech-sdk.md)
- [Zelf studie: een kolf-app bouwen om tekst te vertalen, sentiment te analyseren en vertaalde tekst naar spraak te, rest](https://docs.microsoft.com/azure/cognitive-services/translator/tutorial-build-flask-app-translation-synthesis?toc=%2fazure%2fcognitive-services%2fspeech-service%2ftoc.json&bc=%2fazure%2fcognitive-services%2fspeech-service%2fbreadcrumb%2ftoc.json&toc=%2Fen-us%2Fazure%2Fcognitive-services%2Fspeech-service%2Ftoc.json&bc=%2Fen-us%2Fazure%2Fbread%2Ftoc.json)

## <a name="get-sample-code"></a>Voorbeeldcode ophalen

Voorbeeld code is beschikbaar op GitHub voor de speech-service. Deze voor beelden hebben betrekking op veelvoorkomende scenario's, zoals het lezen van audio van een bestand of stream, continue en eenmalige herkenning en het werken met aangepaste modellen. Gebruik deze koppelingen om SDK-en REST-voor beelden weer te geven:

- [Voor beelden van spraak naar tekst, tekst naar spraak en spraak omzetting (SDK)](https://github.com/Azure-Samples/cognitive-services-speech-sdk)
- [Voor beelden van batch transcriptie (REST)](https://github.com/Azure-Samples/cognitive-services-speech-sdk/tree/master/samples/batch)
- [Voor beelden van tekst naar spraak (REST)](https://github.com/Azure-Samples/Cognitive-Speech-TTS)
- [Voor beelden van Voice Assistant (SDK)](https://aka.ms/csspeech/samples)

## <a name="customize-your-speech-experience"></a>Uw spraak ervaring aanpassen

De spraak service is goed geschikt voor ingebouwde modellen, maar u kunt de ervaring voor uw product of omgeving verder aanpassen en afstemmen. Aanpassings opties variëren van het verfijnen van akoestische modellen tot unieke spraak lettertypen voor uw merk.

| Speech Service | Platform | Beschrijving |
| -------------- | -------- | ----------- |
| Spraak naar tekst | [Aangepaste spraak](https://aka.ms/customspeech) | Pas de modellen voor spraak herkenning aan uw behoeften en beschik bare gegevens aan. Elimineer hindernissen bij spraakherkenning, zoals spreekstijl, vocabulaire en achtergrondgeluiden. |
| Tekst naar spraak | [Aangepaste spraak](https://aka.ms/customvoice) | Bouw een herkenbare, eenduidige stem voor uw tekst-naar-spraak-apps met uw beschikbare gesproken gegevens. U kunt de spraak uitvoer verder verfijnen door een set spraak parameters aan te passen. |

## <a name="reference-docs"></a>Referentiedocumenten

- [Speech-SDK](speech-sdk-reference.md)
- [SDK voor spraak apparaten](speech-devices-sdk.md)
- [REST API: spraak naar tekst](rest-speech-to-text.md)
- [REST API: tekst-naar-spraak](rest-text-to-speech.md)
- [REST API: batch transcriptie en-aanpassing](https://westus.cris.ai/swagger/ui/index)

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Gratis een abonnements sleutel voor een spraak service ophalen](get-started.md)
