---
title: 'Quickstart: Antwoord uit knowledge base ophalen - REST, Go - QnA Maker'
titleSuffix: Azure Cognitive Services
description: Deze Go REST-quickstart begeleidt u bij het programmatisch ophalen van een antwoord uit een knowledge base.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: quickstart
ms.date: 10/01/2019
ms.author: diberry
ms.openlocfilehash: 7fa0a747965f946198c59aa521c6269fa159c702
ms.sourcegitcommit: 4f3f502447ca8ea9b932b8b7402ce557f21ebe5a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/02/2019
ms.locfileid: "71802957"
---
# <a name="quickstart-get-answers-to-a-question-from-a-knowledge-base-with-go"></a>Quickstart: Antwoorden vinden op vragen met behulp van een knowledge base met Go

In deze quickstart wordt beschreven hoe u programmatisch een antwoord uit een gepubliceerde QnA Maker-knowledge base kunt ophalen. De Knowledge Base bevat vragen en antwoorden van [gegevens bronnen](../Concepts/data-sources-supported.md) , zoals Veelgestelde vragen. De [vraag](../how-to/metadata-generateanswer-usage.md#generateanswer-request-configuration) wordt verzonden naar de QnA Maker-service. Het [antwoord](../how-to/metadata-generateanswer-usage.md#generateanswer-response-properties) bevat het meest voorspelde antwoord. 

## <a name="prerequisites"></a>Vereisten

* [Go 1.10.1](https://golang.org/dl/)
* [Visual Studio Code](https://code.visualstudio.com/)
* U moet een [QnA Maker-service ](../How-To/set-up-qnamaker-service-azure.md) hebben. Selecteer **Sleutels** onder **Resourcebeheer** in het Azure-dashboard voor de QnA Maker-resource om uw sleutel op te halen. 
* Pagina Instellingen voor **Publiceren**. Als u nog geen knowledge base hebt gepubliceerd, moet u een lege knowledge base maken, een knowledge base importeren op de pagina **Instellingen** en de knowledge base publiceren. U kunt [deze eenvoudige knowledge base](https://github.com/Azure-Samples/cognitive-services-sample-data-files/blob/master/qna-maker/knowledge-bases/basic-kb.tsv) downloaden en gebruiken. 

    De pagina Instellingen voor Publiceren bevat de waarde voor POST-route, Host en EndpointKey. 

    ![Instellingen voor Publiceren](../media/qnamaker-quickstart-get-answer/publish-settings.png)

De code voor deze quickstart bevindt zich in de opslagplaats [https://github.com/Azure-Samples/cognitive-services-qnamaker-Go](https://github.com/Azure-Samples/cognitive-services-qnamaker-Go/tree/master/documentation-samples/quickstarts/get-answer). 

## <a name="create-a-go-file"></a>Een Go-bestand maken

Open VSCode, maak een nieuw bestand met de naam `get-answer.go` en voeg de volgende klasse toe:

```Go
package main

func main() {

}
```

## <a name="add-the-required-dependencies"></a>De vereiste afhankelijkheden toevoegen

Voeg boven de functie `main`, bovenin het bestand `get-answer.go`, de vereiste afhankelijkheden toe aan het project:

[!code-go[Add the required dependencies](~/samples-qnamaker-go/documentation-samples/quickstarts/get-answer/get-answer.go?range=3-9 "Add the required dependencies")]

## <a name="add-the-required-constants"></a>De vereiste constanten toevoegen

Voeg bovenaan de functie `main` de vereiste constanten toe voor toegang tot QnA Maker. Deze waarden vindt u op de pagina **Publiceren** nadat u de knowledge base hebt gepubliceerd. 

[!code-go[Add the required constants](~/samples-qnamaker-go/documentation-samples/quickstarts/get-answer/get-answer.go?range=17-33 "Add the required constants")]

## <a name="add-a-post-request-to-send-question-and-get-answer"></a>Een POST-aanvraag toevoegen om vragen te verzenden en antwoorden op te halen

Met de volgende code wordt een HTTPS-aanvraag naar de QnA Maker-API verzonden om de vraag naar de knowledge base te verzenden en het antwoord te ontvangen:

[!code-go[Add a POST request to send question to knowledge base](~/samples-qnamaker-go/documentation-samples/quickstarts/get-answer/get-answer.go?range=35-48 "Add a POST request to send question to knowledge base")]

De waarde van de header van `Authorization` bevat de tekenreeks `EndpointKey`. 

Meer informatie over de [aanvraag](../how-to/metadata-generateanswer-usage.md#generateanswer-request) en het [antwoord](../how-to/metadata-generateanswer-usage.md#generateanswer-response).

## <a name="build-and-run-the-program"></a>Het programma bouwen en uitvoeren

Compileer het programma en voer het uit vanaf de opdrachtregel. De aanvraag wordt automatisch verzonden naar de QnA Maker-API, waarna het antwoord wordt weergegeven in het consolevenster.

1. Het bestand compileren:

    ```bash
    go build get-answer.go
    ```

1. Het bestand uitvoeren:

    ```bash
    ./get-answer
    ```

[!INCLUDE [JSON request and response](../../../../includes/cognitive-services-qnamaker-quickstart-get-answer-json.md)] 


[!INCLUDE [Clean up files and knowledge base](../../../../includes/cognitive-services-qnamaker-quickstart-cleanup-resources.md)] 

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Naslaginformatie over REST API voor QnA Maker (V4)](https://go.microsoft.com/fwlink/?linkid=2092179)
