---
title: Aantal vooraf samengestelde entiteit-LUIS
titleSuffix: Azure Cognitive Services
description: Dit artikel bevat informatie over het aantal vooraf gedefinieerde entiteits gegevens in Language Understanding (LUIS).
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 09/27/2019
ms.author: diberry
ms.openlocfilehash: 6a59cf83b3912e31b8aae67319902ce516519af8
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/04/2019
ms.locfileid: "73491300"
---
# <a name="number-prebuilt-entity-for-a-luis-app"></a>Aantal vooraf samengestelde entiteiten voor een LUIS-app
Er zijn veel manieren waarop numerieke waarden worden gebruikt om stukjes informatie te kwantificeren, in te delen en te beschrijven. In dit artikel komen alleen enkele van de mogelijke voor beelden aan bod. LUIS interpreteert de variaties in gebruikers uitingen en retourneert consistente numerieke waarden. Omdat deze entiteit al is getraind, hoeft u geen voor beeld-uitingen met een nummer toe te voegen aan de toepassings intentie. 

## <a name="types-of-number"></a>Typen nummer
Nummer wordt beheerd vanuit de map [recognizers-text](https://github.com/Microsoft/Recognizers-Text/blob/master/Patterns/English/English-Numbers.yaml) github

## <a name="examples-of-number-resolution"></a>Voor beelden van nummer omzetting

| Utterance        | Entiteit   | Resolutie |
| ------------- |:----------------:| --------------:|
| ```one thousand times```  | ```"one thousand"``` |   ```"1000"```      | 
| ```1,000 people```        | ```"1,000"```    |   ```"1000"```      |
| ```1/2 cup```         | ```"1 / 2"```    |    ```"0.5"```      |
|  ```one half the amount```     | ```"one half"```     |    ```"0.5"```      |
| ```one hundred fifty orders``` | ```"one hundred fifty"``` | ```"150"``` |
| ```one hundred and fifty books``` | ```"one hundred and fifty"``` | ```"150"```|
| ```a grade of one point five```| ```"one point five"``` |  ```"1.5"``` |
| ```buy two dozen eggs```    | ```"two dozen"``` | ```"24"``` |


LUIS bevat de herkende waarde van een **`builtin.number`** entiteit in het veld `resolution` van het JSON-antwoord dat wordt geretourneerd.

## <a name="resolution-for-prebuilt-number"></a>Oplossing voor het vooraf gedefinieerde nummer

De volgende entiteits objecten worden geretourneerd voor de query:

`order two dozen eggs`

#### <a name="v3-responsetabv3"></a>[V3-antwoord](#tab/V3)

De volgende JSON is waarvan de `verbose` para meter is ingesteld op `false`:

```json
"entities": {
    "number": [
        24
    ]
}
```
#### <a name="v3-verbose-responsetabv3-verbose"></a>[Uitgebreide respons van v3](#tab/V3-verbose)

De volgende JSON is waarvan de `verbose` para meter is ingesteld op `true`:

```json
"entities": {
    "number": [
        24
    ],
    "$instance": {
        "number": [
            {
                "type": "builtin.number",
                "text": "two dozen",
                "startIndex": 6,
                "length": 9,
                "modelTypeId": 2,
                "modelType": "Prebuilt Entity Extractor",
                "recognitionSources": [
                    "model"
                ]
            }
        ]
    }
}
```
#### <a name="v2-responsetabv2"></a>[V2-antwoord](#tab/V2)

In het volgende voor beeld ziet u een JSON-reactie van LUIS, die de resolutie van de waarde 24 voor de utterance "twee dozijn" omvat.

```json
"entities": [
  {
    "entity": "two dozen",
    "type": "builtin.number",
    "startIndex": 6,
    "endIndex": 14,
    "resolution": {
      "subtype": "integer",
      "value": "24"
    }
  }
]
```
* * * 

## <a name="next-steps"></a>Volgende stappen

Meer informatie over het [v3-Voorspellings eindpunt](luis-migration-api-v3.md).

Meer informatie over de [valuta](luis-reference-prebuilt-currency.md), het [rang telwoord](luis-reference-prebuilt-ordinal.md)en het [percentage](luis-reference-prebuilt-percentage.md). 
