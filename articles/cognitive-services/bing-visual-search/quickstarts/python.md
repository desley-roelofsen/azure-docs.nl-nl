---
title: 'Snelstartgids: Image Insights ophalen met behulp van de REST API en python-Bing Visual Search'
titleSuffix: Azure Cognitive Services
description: Leer hoe u een afbeelding uploadt naar de Bing Visual Search-API en inzichten in de afbeelding verkrijgt.
services: cognitive-services
author: swhite-msft
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-visual-search
ms.topic: quickstart
ms.date: 4/02/2019
ms.author: scottwhi
ms.openlocfilehash: 6fafc35d9d74927789fee3f3fea3014ff3be5717
ms.sourcegitcommit: f523c8a8557ade6c4db6be12d7a01e535ff32f32
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/22/2019
ms.locfileid: "74383182"
---
# <a name="quickstart-get-image-insights-using-the-bing-visual-search-rest-api-and-python"></a>Snelstartgids: Image Insights ophalen met behulp van de Bing Visual Search REST API en python

Gebruik deze Quick Start om de Bing Visual Search-API te maken en de resultaten weer te geven. Deze python-toepassing uploadt een installatie kopie naar de API en geeft de informatie weer die wordt geretourneerd. Hoewel deze toepassing is geschreven in Python, is de API een REST-webservice die compatibel is met de meeste programmeer talen.

Wanneer u een lokale installatie kopie uploadt, moeten de formulier gegevens de `Content-Disposition`-header bevatten. U moet de para meter `name` instellen op "afbeelding" en u kunt de para meter `filename` instellen op elke wille keurige teken reeks. De inhoud van het formulier bevat de binaire gegevens van de installatie kopie. De maximale afbeeldings grootte die u kunt uploaden, is 1 MB.

```
--boundary_1234-abcd
Content-Disposition: form-data; name="image"; filename="myimagefile.jpg"

ÿØÿà JFIF ÖÆ68g-¤CWŸþ29ÌÄøÖ‘º«™æ±èuZiÀ)"óÓß°Î= ØJ9á+*G¦...

--boundary_1234-abcd--
```

## <a name="prerequisites"></a>Vereisten

* [Python 3.x](https://www.python.org/)

[!INCLUDE [cognitive-services-bing-visual-search-signup-requirements](../../../../includes/cognitive-services-bing-image-search-signup-requirements.md)]

## <a name="initialize-the-application"></a>De toepassing initialiseren

1. Maak een nieuw python-bestand in uw favoriete IDE of editor en voeg de volgende `import`-instructie toe:

    ```python
    import requests, json
    ```

2. Maak variabelen voor uw abonnements sleutel, eind punt en het pad naar de installatie kopie die u wilt uploaden:

    ```python

    BASE_URI = 'https://api.cognitive.microsoft.com/bing/v7.0/images/visualsearch'
    SUBSCRIPTION_KEY = 'your-subscription-key'
    imagePath = 'your-image-path'
    ```

3. Maak een woordenlijst object voor de koptekst gegevens van uw aanvraag. Bind uw abonnements sleutel met de teken reeks `Ocp-Apim-Subscription-Key`, zoals hieronder wordt weer gegeven:

    ```python
    HEADERS = {'Ocp-Apim-Subscription-Key': SUBSCRIPTION_KEY}
    ```

4. Maak een andere woorden lijst die uw afbeelding bevat, die wordt geopend en geüpload wanneer u de aanvraag verzendt:

    ```python
    file = {'image' : ('myfile', open(imagePath, 'rb'))}
    ```

## <a name="parse-the-json-response"></a>Het JSON-antwoord parseren

1. Maak een methode met de naam `print_json()` om in de API-reactie te nemen en druk de JSON af:

    ```python
    def print_json(obj):
        """Print the object as json"""
        print(json.dumps(obj, sort_keys=True, indent=2, separators=(',', ': ')))
    ```

## <a name="send-the-request"></a>De aanvraag verzenden

1. Gebruik `requests.post()` om een aanvraag naar de Bing Visual Search-API te verzenden. Neem hierin de tekenreeks voor uw eindpunt, de header en gegevens over het bestand op. `response.json()` met `print_json()`afdrukken:

    ```python
    try:
        response = requests.post(BASE_URI, headers=HEADERS, files=file)
        response.raise_for_status()
        print_json(response.json())
    
    except Exception as ex:
        raise ex
    ```

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Een Visual Search Web-app met één pagina maken](../tutorial-bing-visual-search-single-page-app.md)
