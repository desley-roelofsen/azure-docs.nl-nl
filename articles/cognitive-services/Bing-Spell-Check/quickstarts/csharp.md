---
title: 'Snelstartgids: Controleer de spelling met de REST API C# en-Bing spellingcontrole'
titleSuffix: Azure Cognitive Services
description: Aan de slag met de Bing Spellingcontrole-REST API om de spelling en grammatica te controleren.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-spell-check
ms.topic: quickstart
ms.date: 04/11/2019
ms.author: aahi
ms.openlocfilehash: e51c1220e120d157ea4a413b95a7beb20c950518
ms.sourcegitcommit: f523c8a8557ade6c4db6be12d7a01e535ff32f32
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/22/2019
ms.locfileid: "74378910"
---
# <a name="quickstart-check-spelling-with-the-bing-spell-check-rest-api-and-c"></a>Snelstartgids: Controleer de spelling met de Bing Spellingcontrole REST API enC#

Gebruik deze quickstart om uw eerste aanroep naar de Bing Spellingcontrole REST API te maken. Deze eenvoudige C#-toepassing verzendt een aanvraag naar de API en retourneert een lijst met voorgestelde correcties. Hoewel deze toepassing in C# is geschreven, is de API een RESTful-webservice die compatibel is met vrijwel elke programmeertaal. De broncode voor deze toepassing is beschikbaar [op GitHub](https://github.com/Azure-Samples/cognitive-services-REST-api-samples/blob/master/dotnet/Search/BingAutosuggestv7.cs).

## <a name="prerequisites"></a>Vereisten

* Een versie van [Visual Studio 2017 of hoger](https://www.visualstudio.com/downloads/).
* `Newtonsoft.Json` installeren als een NuGet-pakket in Visual Studio:
    1. Klik in **Solution Explorer**met de rechter muisknop op het oplossings bestand.
    1. Selecteer **NuGet-pakketten beheren voor oplossing**.
    1. Zoek naar `Newtonsoft.Json` en installeer het pakket.
* Als u Linux/MacOS gebruikt, kan deze toepassing worden uitgevoerd met behulp van [Mono](https://www.mono-project.com/).

[!INCLUDE [cognitive-services-bing-spell-check-signup-requirements](../../../../includes/cognitive-services-bing-spell-check-signup-requirements.md)]

## <a name="create-and-initialize-a-project"></a>Een project maken en initialiseren

1. Maak in Visual Studio een nieuwe console oplossing met de naam `SpellCheckSample`. Voeg de volgende naamruimten in het hoofdcodebestand in.
    
    ```csharp
    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Net.Http;
    using System.Net.Http.Headers;
    using System.Text;
    using Newtonsoft.Json;
    ```

2. Maak variabelen voor het API-eindpunt, uw abonnementssleutel en de tekst die moet worden gecontroleerd.

    ```csharp
    namespace SpellCheckSample
    {
        class Program
        {
            static string host = "https://api.cognitive.microsoft.com";
            static string path = "/bing/v7.0/spellcheck?";
            static string key = "<ENTER-KEY-HERE>";
            //text to be spell-checked
            static string text = "Hollo, wrld!";
        }
    }
    ```

3. Doe hetzelfde voor uw zoekparameters. Voeg uw markt code toe na `mkt=`. De markt code is het land van waaruit u de aanvraag maakt. Voeg ook de modus voor spelling controle toe na `&mode=`. De modus is `proof` (de meeste spelling-en grammatica fouten worden onderschept) of `spell` (de meeste spelling wordt niet zo veel grammaticale fouten onderschept).
    
    ```csharp
    static string params_ = "mkt=en-US&mode=proof";
    ```

## <a name="create-and-send-a-spell-check-request"></a>Een spellingcontroleaanvraag maken en verzenden

1. Maken van een asynchrone functie met de naam `SpellCheck()` om een aanvraag te verzenden naar de API. Maak een `HttpClient`, en voeg uw abonnementssleutel toe aan de header `Ocp-Apim-Subscription-Key`. Voer binnen deze functie de volgende stappen uit.

    ```csharp
    async static void SpellCheck()
    {
        var client = new HttpClient();
        client.DefaultRequestHeaders.Add("Ocp-Apim-Subscription-Key", key);

        HttpResponseMessage response = null;
        // add the rest of the code snippets here (except for main())...
    }
    ```

2. Maak de URI voor uw aanvraag door de host, het pad en de para meters toe te voegen.
    
    ```csharp
    string uri = host + path + params_;
    ```

3. Maak een lijst met een object `KeyValuePair` met uw tekst en gebruik het voor het maken van een object `FormUrlEncodedContent`. Stel de headerinformatie in en gebruik `PostAsync()` om de aanvraag te verzenden.

    ```csharp
    var values = new Dictionary<string, string>();
    values.Add("text", text);
    var content = new FormUrlEncodedContent(values);
    content.Headers.ContentType = new MediaTypeHeaderValue("application/x-www-form-urlencoded");
    response = await client.PostAsync(uri, new FormUrlEncodedContent(values));
    ```

## <a name="get-and-print-the-api-response"></a>De API-reactie ophalen en afdrukken

### <a name="get-the-client-id-header"></a>De client-ID-header ophalen

Als het antwoord een koptekst `X-MSEdge-ClientID` bevat, verkrijg dan de waarde en druk deze af.

``` csharp
string client_id;
if (response.Headers.TryGetValues("X-MSEdge-ClientID", out IEnumerable<string> header_values))
{
    client_id = header_values.First();
    Console.WriteLine("Client ID: " + client_id);
}
```

### <a name="get-the-response"></a>Haal het antwoord op

Haal het antwoord op uit de API. Deserialiseer het JSON-object en druk het af naar de console.

```csharp
string contentString = await response.Content.ReadAsStringAsync();

dynamic jsonObj = JsonConvert.DeserializeObject(contentString);
Console.WriteLine(jsonObj);
```

## <a name="call-the-spell-check-function"></a>Roep de spellingcontrolefunctie aan

In de Main-functie van uw project, roept u `SpellCheck()` aan.

```csharp
static void Main(string[] args)
{
    SpellCheck();
    Console.ReadLine();
}
```

## <a name="example-json-response"></a>Voorbeeld van JSON-antwoord

Een geslaagd antwoord wordt geretourneerd in de JSON-indeling, zoals u kunt zien in het volgende voorbeeld: 

```json
{
   "_type": "SpellCheck",
   "flaggedTokens": [
      {
         "offset": 0,
         "token": "Hollo",
         "type": "UnknownToken",
         "suggestions": [
            {
               "suggestion": "Hello",
               "score": 0.9115257530801
            },
            {
               "suggestion": "Hollow",
               "score": 0.858039839213461
            },
            {
               "suggestion": "Hallo",
               "score": 0.597385084464481
            }
         ]
      },
      {
         "offset": 7,
         "token": "wrld",
         "type": "UnknownToken",
         "suggestions": [
            {
               "suggestion": "world",
               "score": 0.9115257530801
            }
         ]
      }
   ]
}
```

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Een web-app met één pagina maken](../tutorials/spellcheck.md)

- [Wat is de Bing Spellingcontrole-API?](../overview.md)
- [Referentie voor de Bing Spellingcontrole-API v7](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-spell-check-api-v7-reference)
