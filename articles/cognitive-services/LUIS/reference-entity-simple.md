---
title: Type eenvoudige entiteit-LUIS
titleSuffix: Azure Cognitive Services
description: Een eenvoudige entiteit beschrijft één concept van de door de computer geleerde context. Voeg een woordgroepen lijst toe wanneer u een eenvoudige entiteit gebruikt om de resultaten te verbeteren.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: reference
ms.date: 09/29/2019
ms.author: diberry
ms.openlocfilehash: 8b92aa6057c81ec9442372c5b85918cb92196d61
ms.sourcegitcommit: 8bd85510aee664d40614655d0ff714f61e6cd328
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/06/2019
ms.locfileid: "74894765"
---
# <a name="simple-entity"></a>Eenvoudige entiteit

Een eenvoudige entiteit is een generieke entiteit die één concept beschrijft en die wordt geleerd van de door de computer geleerde context. Omdat eenvoudige entiteiten doorgaans namen zijn zoals bedrijfs namen, product namen of andere categorieën namen, voegt u een [woordgroepen lijst](luis-concept-feature.md) toe wanneer u een eenvoudige entiteit gebruikt om het signaal van de gebruikte namen te verhogen.

**De entiteit is goed geschikt wanneer:**

* De gegevens zijn niet consistent opgemaakt, maar aangeven hetzelfde.

![eenvoudige entiteit](./media/luis-concept-entities/simple-entity.png)

## <a name="example-json"></a>Voorbeeld van JSON

`Bob Jones wants 3 meatball pho`

In de vorige utterance `Bob Jones` wordt aangeduid als een eenvoudige `Customer` entiteit.

De gegevens die worden geretourneerd van het eindpunt bevat de naam van de entiteit, de gedetecteerde tekst van de utterance, de locatie van de gedetecteerde tekst en de score is:

#### <a name="v2-prediction-endpoint-responsetabv2"></a>[V2-antwoord op Voorspellings eindpunt](#tab/V2)

```JSON
"entities": [
  {
  "entity": "bob jones",
  "type": "Customer",
  "startIndex": 0,
  "endIndex": 8,
  "score": 0.473899543
  }
]
```

#### <a name="v3-prediction-endpoint-responsetabv3"></a>[V3-Voorspellings eindpunt antwoord](#tab/V3)

Dit is de JSON als `verbose=false` is ingesteld in de query reeks:

```json
"entities": {
    "Customer": [
        "Bob Jones"
    ]
}```

This is the JSON if `verbose=true` is set in the query string:

```json
"entities": {
    "Customer": [
        "Bob Jones"
    ],
    "$instance": {
        "Customer": [
            {
                "type": "Customer",
                "text": "Bob Jones",
                "startIndex": 0,
                "length": 9,
                "score": 0.9339134,
                "modelTypeId": 1,
                "modelType": "Entity Extractor",
                "recognitionSources": [
                    "model"
                ]
            }
        ]
    }
}
```

* * *

|Data-object|Naam van de entiteit|Waarde|
|--|--|--|
|Eenvoudige entiteit|`Customer`|`bob jones`|

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Syntaxis van het patroon leren](reference-pattern-syntax.md)