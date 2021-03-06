---
title: Tekst-naar-spraak-spraak service
titleSuffix: Azure Cognitive Services
description: Met de functie voor tekst naar spraak in de speech-service kunt u uw toepassingen, hulpprogram ma's of apparaten gebruiken om tekst te converteren naar natuurlijke menselijke-achtige gesynthesizerde spraak. Kies vooraf ingestelde stemmen of maak uw eigen aangepaste stem.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 06/24/2019
ms.author: erhopf
ms.openlocfilehash: f5929be58c5e2159e832005b0e0f8d01e014427e
ms.sourcegitcommit: a5ebf5026d9967c4c4f92432698cb1f8651c03bb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/08/2019
ms.locfileid: "74923746"
---
# <a name="what-is-text-to-speech"></a>Wat is tekst-naar-spraak?

Tekst-naar-spraak van de speech-service zorgt ervoor dat uw toepassingen, hulpprogram ma's of apparaten tekst kunnen omzetten in natuurlijke menselijke-achtige gesynthesizerde spraak. Kies uit standaard-en Neural stemmen of maak uw eigen aangepaste spraak die uniek is voor uw product of merk. 75 + standaard stemmen zijn verkrijgbaar in meer dan 45 talen en land instellingen en vijf Neural stemmen zijn beschikbaar in vier talen en land instellingen. Zie [ondersteunde talen](language-support.md#text-to-speech)voor een volledige lijst.

Met de tekst-naar-spraak-technologie kunnen makers van inhoud op verschillende manieren communiceren met hun gebruikers. Tekst-naar-spraak kan de toegankelijkheid verbeteren door gebruikers de mogelijkheid te bieden om te communiceren met inhoud audibly. Of de gebruiker een visuele waardevermindering, een leer bare handicap of navigatie-informatie vereist tijdens het besturings element, tekst-naar-spraak, een bestaande ervaring kan verbeteren. Tekst-naar-spraak is ook een waardevolle invoeg toepassing voor spraak bots en spraak assistenten.

Door gebruik te maken van SSML (Speech synthese Markup Language), een op XML gebaseerde Markup-taal, kunnen ontwikkel aars die de tekst naar spraak-service gebruiken, opgeven hoe invoer tekst wordt omgezet in gesynthesizerde spraak. Met SSML kunt u de hoogte, de uitspraak, de spreek snelheid, het volume en meer aanpassen. Zie [SSML](#speech-synthesis-markup-language-ssml)voor meer informatie.

### <a name="standard-voices"></a>Standaard stemmen

Standaard stemmen worden gemaakt met behulp van statistische parametrische synthese en/of samenvoeg synthese technieken. Deze stemmen zijn zeer begrijpelijk en klinkt natuurlijk. U kunt uw toepassingen eenvoudig laten spreken in meer dan 45 talen, met een breed scala aan spraak opties. Deze stemmen bieden een hoge nauw keurigheid van de uitspraak, inclusief ondersteuning voor afkortingen, acroniem uitbrei dingen, datum-en tijd interpretaties, telefoons en meer. Gebruik standaard stem om de toegankelijkheid van uw toepassingen en services te verbeteren door gebruikers toe te staan te communiceren met uw inhouds audibly.

### <a name="neural-voices"></a>Neural stemmen

Neural stemmen gebruiken diepe Neural-netwerken om de limieten van traditionele tekst-naar-spraak-systemen te verruimen in het vergelijken van de patronen van stress en intonation in gesp roken taal en bij het samen werken aan de spraak eenheden in een computer spraak. Standaard tekst-naar-spraak onderbreekt prosody in afzonderlijke linguïstische analyse en akoestische Voorspellings stappen die worden geregeld door onafhankelijke modellen, wat kan leiden tot muffled-spraak synthese. Met onze neurale mogelijkheid wordt de voorspelling van de prosodie en de stemsynthese tegelijkertijd uitgevoerd. Dit leidt tot een vloeiender, natuurlijker klinkende stem.

Neural stemmen kunnen worden gebruikt om interacties te maken met chat bots uitbreiden en spraak assistenten die natuurlijk en aantrekkelijker zijn, en om digitale teksten, zoals e-books, te converteren naar Audiobooks en in-car navigatie systemen te verbeteren. Met het menselijke net zoals natuurlijke prosody en heldere afbakening van woorden, verlaagt Neural stemmen veel luister intensief wanneer u met AI-systemen communiceert.

Neural stemmen ondersteunen verschillende stijlen, zoals Neutral en cheerful. Zo kan de Jessa (en-US) Voice cheerfully spreken, die is geoptimaliseerd voor warme, blije gesprek. U kunt de spraak uitvoer aanpassen, zoals Toon, Toon hoogte en snelheid, met behulp van de taal voor het [opmaken van spraak synthese](speech-synthesis-markup.md). Zie [ondersteunde talen](language-support.md#text-to-speech)voor een volledige lijst met beschik bare stemmen.

Ga voor meer informatie over de voor delen van Neural stemmen naar [de nieuwe Neural-tekst naar-speech-service van micro soft, waarmee computers kunnen spreken zoals mensen](https://azure.microsoft.com/blog/microsoft-s-new-neural-text-to-speech-service-helps-machines-speak-like-people/).

### <a name="custom-voices"></a>Aangepaste stemmen

Met stem aanpassing kunt u een herken bare, eenzijdige stem voor uw merk maken. Als u uw aangepaste spraak letter type wilt maken, maakt u een studio-opname en uploadt u de bijbehorende scripts als de trainings gegevens. De service maakt vervolgens een uniek spraak model dat is afgestemd op uw opname. U kunt dit aangepaste spraak lettertype gebruiken om spraak te vervormen. Zie [aangepaste stemmen](how-to-customize-voice-font.md)voor meer informatie.

## <a name="speech-synthesis-markup-language-ssml"></a>Speech Synthesis Markup Language (SSML)

SSML (Speech synthese Markup Language) is een op XML gebaseerde Markup-taal waarmee ontwikkel aars kunnen opgeven hoe invoer tekst wordt omgezet in gesynthesizerde spraak met behulp van de tekst-naar-spraak-service. Vergeleken met onbewerkte tekst kunnen ontwikkel aars met SSML de hoogte, de uitspraak, de spreek snelheid, het volume en meer van de tekst-naar-spraak-uitvoer nauw keuriger instellen. Normale Lees tekens, zoals het onderbreken na een periode, of het gebruik van de juiste intonation wanneer een zin eindigt met een vraag teken, worden automatisch afgehandeld.

Alle tekst invoer die wordt verzonden naar de service tekst naar spraak moet zijn gestructureerd als SSML. Zie voor meer informatie [taal voor opmaak van spraak synthese](speech-synthesis-markup.md).

### <a name="pricing-note"></a>Prijs notitie

Wanneer u de service tekst naar spraak gebruikt, wordt u gefactureerd voor elk teken dat naar spraak wordt geconverteerd, inclusief Lees tekens. Hoewel het SSML-document zelf niet factureerbaar is, worden optionele elementen die worden gebruikt voor het aanpassen van de manier waarop de tekst naar spraak wordt geconverteerd, zoals fonemen en pitch, geteld als factureer bare tekens. Hier volgt een lijst met wat factureerbaar is:

- Tekst die wordt door gegeven aan de service tekst naar spraak in de SSML-hoofd tekst van de aanvraag
- Alle aantekeningen in het tekst veld van de aanvraag tekst in de SSML-indeling, met uitzonde ring van `<speak>`-en `<voice>`-Tags
- Letters, lees tekens, spaties, tabs, opmaak en alle spatie tekens
- Elk code punt dat in Unicode is gedefinieerd

Zie [prijzen](https://azure.microsoft.com/pricing/details/cognitive-services/speech-services/)voor gedetailleerde informatie.

> [!IMPORTANT]
> Elk Chinees, Japans en Koreaans teken worden als twee tekens beschouwd voor facturering.

## <a name="core-features"></a>Kern functies

In deze tabel worden de belangrijkste functies voor tekst naar spraak weer gegeven:

| Toepassing                                  | SDK | REST  |
| ----------------------------------------- | --- | ----- |
| Zet tekst om in spraak.                   | Ja | Ja   |
| Upload gegevens sets voor stem aanpassing.     | Nee  | Ja\* |
| Spraak lettertype modellen maken en beheren.      | Nee  | Ja\* |
| Implementaties van spraak lettertype maken en beheren. | Nee  | Ja\* |
| Stem lettertype testen maken en beheren.       | Nee  | Ja\* |
| Abonnementen beheren.                     | Nee  | Ja\* |

\*_deze services zijn beschikbaar via het CRIS.ai-eind punt. Zie [Swagger-verwijzing](https://westus.cris.ai/swagger/ui/index). Deze aangepaste spraak training en beheer-Api's implementeren beperking waarbij aanvragen worden beperkt tot 25 per vijf seconden, terwijl de spraakherkennings-API zelf de beperking implementeert, waardoor er Maxi maal 200 aanvragen per seconde worden toegestaan. Wanneer er een beperking optreedt, wordt u gewaarschuwd via bericht headers._

## <a name="get-started-with-text-to-speech"></a>Aan de slag met tekst-naar-spraak

We bieden Quick starts die zijn ontworpen om code in minder dan 10 minuten uit te voeren. Deze tabel bevat een lijst met Quick starts voor tekst naar spraak, geordend op taal.

### <a name="sdk-quickstarts"></a>SDK-Quick starts

| Quick Start (SDK) | Platform | API-referentie |
| ---------------- | -------- | ------------- |
| [C#, .NET Core](~/articles/cognitive-services/Speech-Service/quickstarts/text-to-speech.md?pivots=programming-language-csharp&tabs=dotnetcore)  | Windows | [Door](https://aka.ms/csspeech/csharpref) |
| [C#.NET Framework](~/articles/cognitive-services/Speech-Service/quickstarts/text-to-speech.md?pivots=programming-language-csharp&tabs=dotnet) | Windows | [Door](https://aka.ms/csspeech/csharpref) |
| [C#, UWP](~/articles/cognitive-services/Speech-Service/quickstarts/text-to-speech.md?pivots=programming-language-csharp&tabs=uwp) | Windows | [Door](https://aka.ms/csspeech/csharpref) |
| [C#, Eenheid](~/articles/cognitive-services/Speech-Service/quickstarts/text-to-speech.md?pivots=programming-language-csharp&tabs=unity) | Windows, Android | [Door](https://aka.ms/csspeech/csharpref) |
| [C++](~/articles/cognitive-services/Speech-Service/quickstarts/text-to-speech.md?pivots=programming-language-cpp&tabs=windows) | Windows | [Door](https://aka.ms/csspeech/cppref) |
| [C++](~/articles/cognitive-services/Speech-Service/quickstarts/text-to-speech.md?pivots=programming-language-cpp&tabs=linux) | Linux | [Door](https://aka.ms/csspeech/cppref) |
| [Java](~/articles/cognitive-services/Speech-Service/quickstarts/text-to-speech.md?pivots=programming-language-java&tabs=jre) | Windows, Linux, macOS | [Door](https://aka.ms/csspeech/javaref) |
| [Java](~/articles/cognitive-services/Speech-Service/quickstarts/text-to-speech.md?pivots=programming-language-java&tabs=android) | Android | [Door](https://aka.ms/csspeech/javaref) |
| [Objective-C](~/articles/cognitive-services/speech-service/quickstarts/text-to-speech-langs/objectivec-macos.md) | macOS | [Door](https://aka.ms/csspeech/objectivecref) |
| [Objective-C](~/articles/cognitive-services/speech-service/quickstarts/text-to-speech-langs/objectivec-ios.md) | iOS | [Door](https://aka.ms/csspeech/objectivecref) |
| [Swift](~/articles/cognitive-services/speech-service/quickstarts/text-to-speech-langs/swift-macos.md) | macOS | [Door](https://aka.ms/csspeech/objectivecref) |
| [Swift](~/articles/cognitive-services/speech-service/quickstarts/text-to-speech-langs/swift-ios.md) | iOS | [Door](https://aka.ms/csspeech/objectivecref) |
| [Python](~/articles/cognitive-services/Speech-Service/quickstarts/text-to-speech.md?pivots=programming-language-python) | Window, Linux, macOS | [Door](https://aka.ms/csspeech/pythonref) |

### <a name="rest-quickstarts"></a>REST-Quick starts

| Snelstartgids (REST) | Platform | API-referentie |
| ----------------- | -------- | ------------- |
| [C#, .NET Core](~/articles/cognitive-services/Speech-Service/quickstarts/text-to-speech.md?pivots=programming-language-csharp) | Windows, macOS, Linux | [Door](https://docs.microsoft.com/azure/cognitive-services/speech-service/rest-apis) |
| [Node.js](quickstart-nodejs-text-to-speech.md) | Venster, macOS, Linux | [Door](https://docs.microsoft.com/azure/cognitive-services/speech-service/rest-apis) |
| [Python](quickstart-python-text-to-speech.md) | Venster, macOS, Linux | [Door](https://docs.microsoft.com/azure/cognitive-services/speech-service/rest-apis) |

## <a name="sample-code"></a>Voorbeeldcode

Voorbeeld code voor tekst-naar-spraak is beschikbaar op GitHub. Deze voor beelden hebben betrekking op conversie van tekst naar spraak in de populairste programmeer talen.

- [Voor beelden van tekst naar spraak (SDK)](https://github.com/Azure-Samples/cognitive-services-speech-sdk)
- [Voor beelden van tekst naar spraak (REST)](https://github.com/Azure-Samples/Cognitive-Speech-TTS)

## <a name="reference-docs"></a>Referentiedocumenten

- [Speech-SDK](speech-sdk-reference.md)
- [SDK voor spraak apparaten](speech-devices-sdk.md)
- [REST API: spraak naar tekst](rest-speech-to-text.md)
- [REST API: tekst-naar-spraak](rest-text-to-speech.md)
- [REST API: batch transcriptie en-aanpassing](https://westus.cris.ai/swagger/ui/index)

## <a name="next-steps"></a>Volgende stappen

- [Een gratis spraak service-abonnement ontvangen](get-started.md)
- [Aangepaste spraakstijlen maken](how-to-customize-voice-font.md)
