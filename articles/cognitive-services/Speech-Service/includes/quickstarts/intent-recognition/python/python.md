---
title: 'Snelstartgids: spraak, intenties en entiteiten herkennen, python-Speech-Service'
titleSuffix: Azure Cognitive Services
description: Nader te bepalen
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 10/28/2019
ms.author: erhopf
zone_pivot_groups: programming-languages-set-two
ms.openlocfilehash: 9a4c7f24a2e28743679e312e3dce0bc605db6749
ms.sourcegitcommit: 5aefc96fd34c141275af31874700edbb829436bb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/04/2019
ms.locfileid: "74816124"
---
## <a name="prerequisites"></a>Vereisten

Voordat u aan de slag gaat, moet u het volgende doen:

> [!div class="checklist"]
>
> * [Een Azure-spraak resource maken](../../../../get-started.md)
> * [Een Language Understanding-toepassing (LUIS) maken en een eindpunt sleutel ophalen](../../../../quickstarts/create-luis.md)
> * [Uw ontwikkel omgeving instellen](../../../../quickstarts/setup-platform.md)
> * [Een leeg voorbeeld project maken](../../../../quickstarts/create-project.md)

## <a name="open-your-project"></a>Uw project openen

Open Quickstart.py in uw python-editor.

## <a name="start-with-some-boilerplate-code"></a>Begin met een van de standaard code

Laten we een code toevoegen die als een skelet voor het project werkt.
[!code-python[](~/samples-cognitive-services-speech-sdk/quickstart/python/intent-recognition/quickstart.py?range=5-7)]

## <a name="create-a-speech-configuration"></a>Een spraak configuratie maken

Voordat u een `IntentRecognizer`-object kunt initialiseren, moet u een configuratie maken die gebruikmaakt van uw LUIS-eindpunt sleutel en-regio. Voeg deze code toe volgende.

Met dit voor beeld wordt het `SpeechConfig`-object gebouwd met behulp van LUIS-sleutel en-regio. Zie [SpeechConfig-klasse](https://docs.microsoft.com/python/api/azure-cognitiveservices-speech/azure.cognitiveservices.speech.speechconfig)voor een volledige lijst met beschik bare methoden.
De spraak-SDK wordt standaard herkend door en-US voor de taal. Zie de [bron taal voor spraak opgeven](../../../../how-to-specify-source-language.md) voor de tekst voor informatie over het kiezen van de bron taal.

> [!NOTE]
> Het is belang rijk dat u de LUIS-eindpunt sleutel gebruikt en niet de Starter-of ontwerp sleutels, omdat alleen de eindpunt sleutel geldig is voor spraak op de intentie herkenning. Zie [een Luis-toepassing maken en een eindpunt sleutel ophalen](~/articles/cognitive-services/Speech-Service/quickstarts/create-luis.md) voor instructies over het ophalen van de juiste sleutel.

[!code-python[](~/samples-cognitive-services-speech-sdk/quickstart/python/intent-recognition/quickstart.py?range=12)]

## <a name="initialize-an-intentrecognizer"></a>Een IntentRecognizer initialiseren

Nu gaan we een `IntentRecognizer`maken. Voeg deze code toe aan de rechter kant onder uw spraak configuratie.
[!code-python[](~/samples-cognitive-services-speech-sdk/quickstart/python/intent-recognition/quickstart.py?range=15)]

## <a name="add-a-languageunderstandingmodel-and-intents"></a>Een LanguageUnderstandingModel en intenties toevoegen

U moet nu een `LanguageUnderstandingModel` koppelen aan de intentie herkenning en de intenties toevoegen die u wilt herkennen.
[!code-python[](~/samples-cognitive-services-speech-sdk/quickstart/python/intent-recognition/quickstart.py?range=19-27)]

## <a name="recognize-an-intent"></a>Een intentie herkennen

Vanuit het `IntentRecognizer`-object roept u de `recognize_once()`-methode aan. Met deze methode kan de speech-service weten dat u één woord groep verstuurt voor herkenning en dat zodra de woord groep is geïdentificeerd om te stoppen met het herkennen van spraak.
[!code-python[](~/samples-cognitive-services-speech-sdk/quickstart/python/intent-recognition/quickstart.py?range=35)]

## <a name="display-the-recognition-results-or-errors"></a>De herkennings resultaten (of fouten) weer geven

Wanneer het herkennings resultaat wordt geretourneerd door de spraak service, wilt u er iets mee doen. We gaan het eenvoudig opslaan en het resultaat afdrukken naar de console.

Voeg in de instructie using onder uw aanroep naar `recognize_once()`de volgende code toe: [!code-python[](~/samples-cognitive-services-speech-sdk/quickstart/python/intent-recognition/quickstart.py?range=38-47)]

## <a name="check-your-code"></a>Controleer uw code

Op dit moment moet uw code er als volgt uitzien:  
(Er zijn enkele opmerkingen aan deze versie toegevoegd) [!code-python[](~/samples-cognitive-services-speech-sdk/quickstart/python/intent-recognition/quickstart.py?range=5-47)]

## <a name="build-and-run-your-app"></a>Uw app bouwen en uitvoeren

Voer het voor beeld uit vanaf de-console of in uw IDE:

    ````
    python quickstart.py
    ````

De volgende 15 seconden aan spraakinvoer vanuit uw microfoon worden herkend en geregistreerd in het consolevenster.

## <a name="next-steps"></a>Volgende stappen

[!INCLUDE [footer](./footer.md)]
