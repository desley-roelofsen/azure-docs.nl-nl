---
title: Gegevens extractie-LUIS
titleSuffix: Azure Cognitive Services
description: Gegevens uit utterance-tekst ophalen met intents en entiteiten. Meer informatie over wat voor soort gegevens kan worden geëxtraheerd uit Language Understanding (LUIS).
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 09/27/2019
ms.author: diberry
ms.openlocfilehash: ff0a9838d1fcc9db3b6cc25b47c840e01056e6cd
ms.sourcegitcommit: 6fe40d080bd1561286093b488609590ba355c261
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/01/2019
ms.locfileid: "71703137"
---
# <a name="extract-data-from-utterance-text-with-intents-and-entities"></a>Gegevens uit utterance-tekst ophalen met intents en entiteiten
LUIS biedt u de mogelijkheid informatie ophalen van natuurlijke taal-uitingen van een gebruiker. De informatie wordt opgehaald op een manier dat deze kan worden gebruikt door een programma, toepassing of bot chatten om actie te ondernemen. In de volgende secties meer informatie over welke gegevens worden geretourneerd door intenties en entiteiten met voorbeelden van JSON.

De hardste gegevens die u wilt extra heren, zijn de door de machine geleerde gegevens omdat het geen exacte tekst overeenkomst is. Gegevens extractie van de door de machine geleerde [entiteiten](luis-concept-entity-types.md) moet deel uitmaken van de [ontwerp cyclus](luis-concept-app-iteration.md) totdat u zeker weet dat u de verwachte gegevens ontvangt.

## <a name="data-location-and-key-usage"></a>Locatie en de sleutel gegevensgebruik
LUIS, biedt de gegevens van de gepubliceerde [eindpunt](luis-glossary.md#endpoint). De **HTTPS-aanvraag** (POST of GET) bevat de utterance, evenals van sommige optionele configuraties, zoals fasering of productie-omgevingen.

#### <a name="v2-prediction-endpoint-requesttabv2"></a>[V2-aanvraag voor Voorspellings eindpunt](#tab/V2)

`https://westus.api.cognitive.microsoft.com/luis/v2.0/apps/<appID>?subscription-key=<subscription-key>&verbose=true&timezoneOffset=0&q=book 2 tickets to paris`

#### <a name="v3-prediction-endpoint-requesttabv3"></a>[V3-Voorspellings eindpunt aanvraag](#tab/V3)

`https://westus.api.cognitive.microsoft.com/luis/v3.0-preview/apps/<appID>/slots/<slot-type>/predict?subscription-key=<subscription-key>&verbose=true&timezoneOffset=0&query=book 2 tickets to paris`

Meer informatie over het [v3-Voorspellings eindpunt](luis-migration-api-v3.md).

* * * 

De `appID` is beschikbaar op de pagina **instellingen** van uw Luis-app, evenals een deel van de URL (na `/apps/`) wanneer u deze Luis-app bewerkt. De `subscription-key` is de eindpuntsleutel die wordt gebruikt voor query's in uw app. Hoewel u uw gratis versie van authoring/starter kunt gebruiken terwijl u LUIS Learning, is het belang rijk dat u de eindpunt sleutel wijzigt in een sleutel die het [verwachte Luis gebruik](luis-boundaries.md#key-limits)ondersteunt. De `timezoneOffset` eenheid minuten is.

De **HTTPS-antwoord** bevat alle de intentie en entiteit informatie LUIS kunt bepalen op basis van de huidige gepubliceerde model van een eindpunt van de fasering of productie. Het eindpunt van de URL die is gevonden op de [LUIS](luis-reference-regions.md) website in de **beheren** sectie, op de **sleutels en eindpunten** pagina.

## <a name="data-from-intents"></a>Gegevens van intenties
De primaire gegevens is de hoogste score **intentie naam**. De eindpunt-antwoord is:

#### <a name="v2-prediction-endpoint-responsetabv2"></a>[V2-antwoord op Voorspellings eindpunt](#tab/V2)

```JSON
{
  "query": "when do you open next?",
  "topScoringIntent": {
    "intent": "GetStoreInfo",
    "score": 0.984749258
  },
  "entities": []
}
```

#### <a name="v3-prediction-endpoint-responsetabv3"></a>[V3-Voorspellings eindpunt antwoord](#tab/V3)

```JSON
{
  "query": "when do you open next?",
  "prediction": {
    "normalizedQuery": "when do you open next?",
    "topIntent": "GetStoreInfo",
    "intents": {
        "GetStoreInfo": {
            "score": 0.984749258
        }
    }
  },
  "entities": []
}
```

Meer informatie over het [v3-Voorspellings eindpunt](luis-migration-api-v3.md).

* * * 

|Data-Object|Gegevenstype|Gegevenslocatie|Waarde|
|--|--|--|--|
|Intentie|Reeks|topScoringIntent.intent|"GetStoreInfo"|

Als uw chatbot-of LUIS-app een beslissing neemt op basis van meer dan één intentie Score, kunt u alle uitstaande scores voor intenties retour neren.


#### <a name="v2-prediction-endpoint-responsetabv2"></a>[V2-antwoord op Voorspellings eindpunt](#tab/V2)

Stel de query string-para meter `verbose=true` in. De eindpunt-antwoord is:

```JSON
{
  "query": "when do you open next?",
  "topScoringIntent": {
    "intent": "GetStoreInfo",
    "score": 0.984749258
  },
  "intents": [
    {
      "intent": "GetStoreInfo",
      "score": 0.984749258
    },
    {
      "intent": "None",
      "score": 0.2040639
    }
  ],
  "entities": []
}
```

#### <a name="v3-prediction-endpoint-responsetabv3"></a>[V3-Voorspellings eindpunt antwoord](#tab/V3)

Stel de query string-para meter `show-all-intents=true` in. De eindpunt-antwoord is:

```JSON
{
    "query": "when do you open next?",
    "prediction": {
        "normalizedQuery": "when do you open next?",
        "topIntent": "GetStoreInfo",
        "intents": {
            "GetStoreInfo": {
                "score": 0.984749258
            },
            "None": {
                 "score": 0.2040639
            }
        },
        "entities": {
        }
    }
}
```

Meer informatie over het [v3-Voorspellings eindpunt](luis-migration-api-v3.md).

* * * 

De intenties zijn gerangschikt op de hoogste naar laagste score.

|Data-Object|Gegevenstype|Gegevenslocatie|Waarde|Score|
|--|--|--|--|:--|
|Intentie|Reeks|intents [0] .intent|"GetStoreInfo"|0.984749258|
|Intentie|Reeks|.intent intents [1]|'Geen'|0.0168218873|

Als u vooraf gemaakte domeinen toevoegt, geeft de naam van de intentie het domein, zoals `Utilties` of `Communication` en het doel:

#### <a name="v2-prediction-endpoint-responsetabv2"></a>[V2-antwoord op Voorspellings eindpunt](#tab/V2)

```JSON
{
  "query": "Turn on the lights next monday at 9am",
  "topScoringIntent": {
    "intent": "Utilities.ShowNext",
    "score": 0.07842206
  },
  "intents": [
    {
      "intent": "Utilities.ShowNext",
      "score": 0.07842206
    },
    {
      "intent": "Communication.StartOver",
      "score": 0.0239675418
    },
    {
      "intent": "None",
      "score": 0.0168218873
    }],
  "entities": []
}
```

#### <a name="v3-prediction-endpoint-responsetabv3"></a>[V3-Voorspellings eindpunt antwoord](#tab/V3)

```JSON
{
    "query": "Turn on the lights next monday at 9am",
    "prediction": {
        "normalizedQuery": "Turn on the lights next monday at 9am",
        "topIntent": "Utilities.ShowNext",
        "intents": {
            "Utilities.ShowNext": {
                "score": 0.07842206
            },
            "Communication.StartOver": {
                "score": 0.0239675418
            },
            "None": {
                "score": 0.00085447653
            }
        },
        "entities": []
    }
}
```

Meer informatie over het [v3-Voorspellings eindpunt](luis-migration-api-v3.md).

* * * 

|Domein|Data-Object|Gegevenstype|Gegevenslocatie|Waarde|
|--|--|--|--|--|
|Nutsbedrijven|Intentie|Reeks|intents [0] .intent|"<b>Hulpprogramma's voor</b>. ShowNext"|
|Communicatie|Intentie|Reeks|.intent intents [1]|<b>Communicatie</b>. Opnieuw"|
||Intentie|Reeks|.intent intents [2]|'Geen'|


## <a name="data-from-entities"></a>Gegevens van entiteiten
De meeste chatbots en toepassingen moeten meer dan de naam van de intentie. Deze aanvullende, optionele gegevens afkomstig van entiteiten in de utterance gedetecteerd. Elk type entiteit retourneert verschillende gegevens over de overeenkomst.

Een bepaald woord of zinsdeel in een utterance kan overeenkomen met meer dan één entiteit. In dat geval wordt elke overeenkomende entiteit geretourneerd met de score.

Alle entiteiten worden geretourneerd in de **entiteiten** matrix van het antwoord van het eindpunt:

#### <a name="v2-prediction-endpoint-responsetabv2"></a>[V2-antwoord op Voorspellings eindpunt](#tab/V2)

```JSON
"entities": [
  {
    "entity": "bob jones",
    "type": "Name",
    "startIndex": 0,
    "endIndex": 8,
    "score": 0.473899543
  },
  {
    "entity": "3",
    "type": "builtin.number",
    "startIndex": 16,
    "endIndex": 16,
    "resolution": {
      "value": "3"
    }
  }
]
```

#### <a name="v3-prediction-endpoint-responsetabv3"></a>[V3-Voorspellings eindpunt antwoord](#tab/V3)

```JSON
"entities": {
    "name":["bob jones"],
    "number": [3]
}
```
Meer informatie over het [v3-Voorspellings eindpunt](luis-migration-api-v3.md).

* * * 

## <a name="tokenized-entity-returned"></a>tokens entiteit geretourneerd
Verschillende [culturen](luis-language-support.md#tokenization) retourneren van de entiteit met de `entity` waarde [tokenized](luis-glossary.md#token). De startIndex en endIndex die zijn geretourneerd door LUIS in de entiteitsobject toewijzen niet aan de nieuwe, tokens waarde, maar in plaats daarvan naar de oorspronkelijke query zodat u de onbewerkte entiteit via een programma ophalen. 

Bijvoorbeeld, in het Duits, het woord `das Bauernbrot` is tokenized in `das bauern brot`. De waarde van tokens, `das bauern brot`, wordt geretourneerd en de oorspronkelijke waarde kan via een programma kan worden bepaald uit het startIndex en endIndex van de oorspronkelijke query, zodat u `das Bauernbrot`.

## <a name="simple-entity-data"></a>Eenvoudige entiteitsgegevens

Een [eenvoudige entiteit](reference-entity-simple.md) is een waarde hebt geleerd van een machine. Een woord of woordgroep kan het zijn.

## <a name="composite-entity-data"></a>Samengestelde entiteitsgegevens

Een [samengestelde entiteit](reference-entity-composite.md) bestaat uit andere entiteiten, zoals vooraf gebouwde entiteiten, eenvoudige, reguliere expressies en lijst entiteiten. De afzonderlijke entiteiten vormen een hele entiteit. 

## <a name="list-entity-data"></a>Lijst met entiteitsgegevens

[Lijst entiteiten](reference-entity-list.md) vertegenwoordigen een vaste, gesloten set verwante woorden samen met hun synoniemen. LUIS detecteert geen aanvullende waarden voor de lijst met entiteiten. Gebruik de **raden** functie om te bekijken van suggesties voor nieuwe woorden op basis van de huidige lijst. Als er meer dan één lijst entiteit met dezelfde waarde, wordt elke entiteit in de query eindpunt geretourneerd. 

## <a name="prebuilt-entity-data"></a>Vooraf gemaakte entiteiten
[Vooraf gedefinieerde](luis-concept-entity-types.md) entiteiten worden gedetecteerd op basis van een komt overeen met de reguliere expressie met behulp van de open-source [kenmerken tekst](https://github.com/Microsoft/Recognizers-Text) project. Vooraf gemaakte entiteiten in de matrix entiteiten worden geretourneerd en gebruikt u de naam van het type voorafgegaan door `builtin::`. De volgende tekst is een voorbeeld-utterance met de geretourneerde vooraf gemaakte entiteiten:

`Dec 5th send to +1 360-555-1212`

#### <a name="v2-prediction-endpoint-responsetabv2"></a>[V2-antwoord op Voorspellings eindpunt](#tab/V2)

```JSON
"entities": [
    {
      "entity": "dec 5th",
      "type": "builtin.datetimeV2.date",
      "startIndex": 0,
      "endIndex": 6,
      "resolution": {
        "values": [
          {
            "timex": "XXXX-12-05",
            "type": "date",
            "value": "2017-12-05"
          },
          {
            "timex": "XXXX-12-05",
            "type": "date",
            "value": "2018-12-05"
          }
        ]
      }
    },
    {
      "entity": "1",
      "type": "builtin.number",
      "startIndex": 18,
      "endIndex": 18,
      "resolution": {
        "value": "1"
      }
    },
    {
      "entity": "360",
      "type": "builtin.number",
      "startIndex": 20,
      "endIndex": 22,
      "resolution": {
        "value": "360"
      }
    },
    {
      "entity": "555",
      "type": "builtin.number",
      "startIndex": 26,
      "endIndex": 28,
      "resolution": {
        "value": "555"
      }
    },
    {
      "entity": "1212",
      "type": "builtin.number",
      "startIndex": 32,
      "endIndex": 35,
      "resolution": {
        "value": "1212"
      }
    },
    {
      "entity": "5th",
      "type": "builtin.ordinal",
      "startIndex": 4,
      "endIndex": 6,
      "resolution": {
        "value": "5"
      }
    },
    {
      "entity": "1 360 - 555 - 1212",
      "type": "builtin.phonenumber",
      "startIndex": 18,
      "endIndex": 35,
      "resolution": {
        "value": "1 360 - 555 - 1212"
      }
    }
  ]
```

#### <a name="v3-prediction-endpoint-responsetabv3"></a>[V3-Voorspellings eindpunt antwoord](#tab/V3)

Zonder de query string-para meter, `verbose=true`:

```json
"entities": {
    "datetimeV2": [
        {
            "type": "date",
            "values": [
                {
                    "timex": "XXXX-12-05",
                    "value": "2018-12-05"
                },
                {
                    "timex": "XXXX-12-05",
                    "value": "2019-12-05"
                }
            ]
        }
    ],
    "ordinal": [
        {
            "offset": 5,
            "relativeTo": "start"
        }
    ],
    "ordinalV2": [
        {
            "offset": 5,
            "relativeTo": "start"
        }
    ],
    "number": [
        1360,
        555,
        1212
    ],
    "phonenumber": [
        "1 360-555-1212"
    ]
}
```

Met de query string-para meter, `verbose=true`:

```json

"entities": {
    "datetimeV2": [
        {
            "type": "date",
            "values": [
                {
                    "timex": "XXXX-12-05",
                    "value": "2018-12-05"
                },
                {
                    "timex": "XXXX-12-05",
                    "value": "2019-12-05"
                }
            ]
        }
    ],
    "ordinal": [
        {
            "offset": 5,
            "relativeTo": "start"
        }
    ],
    "ordinalV2": [
        {
            "offset": 5,
            "relativeTo": "start"
        }
    ],
    "number": [
        1360,
        555,
        1212
    ],
    "phonenumber": [
        "1 360-555-1212"
    ],
    "$instance": {
        "datetimeV2": [
            {
                "type": "builtin.datetimeV2.date",
                "text": "Dec 5th",
                "startIndex": 0,
                "length": 7,
                "modelTypeId": 2,
                "modelType": "Prebuilt Entity Extractor",
                "recognitionSources": [
                    "model"
                ]
            }
        ],
        "ordinal": [
            {
                "type": "builtin.ordinal",
                "text": "5th",
                "startIndex": 4,
                "length": 3,
                "modelTypeId": 2,
                "modelType": "Prebuilt Entity Extractor",
                "recognitionSources": [
                    "model"
                ]
            }
        ],
        "ordinalV2": [
            {
                "type": "builtin.ordinalV2",
                "text": "5th",
                "startIndex": 4,
                "length": 3,
                "modelTypeId": 2,
                "modelType": "Prebuilt Entity Extractor",
                "recognitionSources": [
                    "model"
                ]
            }
        ],
        "number": [
            {
                "type": "builtin.number",
                "text": "1 360",
                "startIndex": 17,
                "length": 5,
                "modelTypeId": 2,
                "modelType": "Prebuilt Entity Extractor",
                "recognitionSources": [
                    "model"
                ]
            },
            {
                "type": "builtin.number",
                "text": "555",
                "startIndex": 23,
                "length": 3,
                "modelTypeId": 2,
                "modelType": "Prebuilt Entity Extractor",
                "recognitionSources": [
                    "model"
                ]
            },
            {
                "type": "builtin.number",
                "text": "1212",
                "startIndex": 27,
                "length": 4,
                "modelTypeId": 2,
                "modelType": "Prebuilt Entity Extractor",
                "recognitionSources": [
                    "model"
                ]
            }
        ],
        "phonenumber": [
            {
                "type": "builtin.phonenumber",
                "text": "1 360-555-1212",
                "startIndex": 17,
                "length": 14,
                "score": 1.0,
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

Meer informatie over het [v3-Voorspellings eindpunt](luis-migration-api-v3.md).

* * * 
## <a name="regular-expression-entity-data"></a>Entiteitsgegevens reguliere expressie

Een [reguliere expressie-entiteit](reference-entity-regular-expression.md) extraheert een entiteit op basis van een reguliere expressie patroon dat u opgeeft.

## <a name="extracting-names"></a>Uitpakken van namen
Namen ophalen uit een utterance is moeilijk, omdat een naam mag bestaan uit vrijwel elke combinatie van letters en woorden. Afhankelijk van het type naam dat u uitpakt, hebt u verschillende opties. De volgende suggesties zijn geen regels, maar ook meer richt lijnen.

### <a name="add-prebuilt-personname-and-geographyv2-entities"></a>Preconstrueerde persoons-en GeographyV2-entiteiten toevoegen

De entiteiten [persoons](luis-reference-prebuilt-person.md) -en [GeographyV2](luis-reference-prebuilt-geographyV2.md) zijn beschikbaar in sommige [taal culturen](luis-reference-prebuilt-entities.md). 

### <a name="names-of-people"></a>Namen van personen

De naam van mensen kan een lichte indeling, afhankelijk van de taal en cultuur hebben. Gebruik een vooraf gedefinieerde entiteit **[persoon](luis-reference-prebuilt-person.md)** of een **[eenvoudige entiteit](luis-concept-entity-types.md#simple-entity)** met [rollen](luis-concept-roles.md) van de voor-en achternaam. 

Als u de eenvoudige entiteit gebruikt, moet u voor beelden opgeven die gebruikmaken van de voor-en achternaam in verschillende delen van de utterance, in uitingen met verschillende lengten en uitingen voor alle intenten, inclusief de geen intentie. [Beoordeling](luis-how-to-review-endoint-utt.md) eindpunt uitingen regelmatig naar elke labelnamen zijn niet correct voorspeld.

### <a name="names-of-places"></a>Namen van plaatsen

Locatie namen worden ingesteld en bekend, zoals steden, regio's, provincies, provincies en landen/regio's. Gebruik de vooraf samengestelde entiteit **[geographyV2](luis-reference-prebuilt-geographyv2.md)** om locatie gegevens te extra heren.

### <a name="new-and-emerging-names"></a>Nieuwe en opkomende namen

Sommige apps moeten kunnen om nieuwe en opkomende namen, zoals producten of bedrijven te vinden. Deze typen namen zijn het lastigste type gegevens extractie. Begin met een **[eenvoudige entiteit](luis-concept-entity-types.md#simple-entity)** en voeg een [woordgroepen lijst](luis-concept-feature.md)toe. [Beoordeling](luis-how-to-review-endoint-utt.md) eindpunt uitingen regelmatig naar elke labelnamen zijn niet correct voorspeld.

## <a name="pattern-roles-data"></a>Patroon rollen gegevens
Rollen zijn contextuele verschillen van entiteiten.


#### <a name="v2-prediction-endpoint-responsetabv2"></a>[V2-antwoord op Voorspellings eindpunt](#tab/V2)

De naam van de entiteit is `Location`, met twee rollen, `Origin` en `Destination`.

```JSON
"entities": [
  {
    "entity": "bob jones",
    "type": "Employee",
    "startIndex": 5,
    "endIndex": 13,
    "score": 0.922820568,
    "role": ""
  },
  {
    "entity": "seattle",
    "type": "Location",
    "startIndex": 20,
    "endIndex": 26,
    "score": 0.948008537,
    "role": "Origin"
  },
  {
    "entity": "redmond",
    "type": "Location",
    "startIndex": 31,
    "endIndex": 37,
    "score": 0.7047979,
    "role": "Destination"
  }
]
```

#### <a name="v3-prediction-endpoint-responsetabv3"></a>[V3-Voorspellings eindpunt antwoord](#tab/V3)

In v3 is de **rolnaam** de primaire naam van het object. 

De naam van de entiteit is `Location`, met twee rollen, `Origin` en `Destination`.

Zonder de query string-para meter, `verbose=true`:

```json
"entities": {
    "Employee": [
        "bob jones"
    ],
    "Origin": [
        "seattle"
    ],
    "Destination": [
        "redmond"
    ]
}
```

Met de query string-para meter, `verbose=true`:

```json
"entities": {
    "Employee": [
        "bob jones"
    ],
    "LocationOrigin": [
        "seattle"
    ],
    "LocationDestination": [
        "redmond"
    ],
    "$instance": {
        "Employee": [
            {
                "type": "Employee",
                "text": "bob jones",
                "startIndex": 5,
                "length": 9,
                "score": 0.982873261,
                "modelTypeId": 1,
                "modelType": "Entity Extractor",
                "recognitionSources": [
                    "model"
                ]
            }
        ],
        "Origin": [
            {
                "role": "Origin",
                "type": "Location",
                "text": "seattle",
                "startIndex": 20,
                "length": 7,
                "score": 0.9913306,
                "modelTypeId": 1,
                "modelType": "Entity Extractor",
                "recognitionSources": [
                    "model"
                ]
            }
        ],
        "Destination": [
            {
                "role": "Destination",
                "type": "Location",
                "text": "redmond",
                "startIndex": 31,
                "length": 7,
                "score": 0.898179531,
                "modelTypeId": 1,
                "modelType": "Entity Extractor",
                "recognitionSources": [
                    "model"
                ]
            }
        ]

}
```

Meer informatie over het [v3-Voorspellings eindpunt](luis-migration-api-v3.md).

* * *

## <a name="patternany-entity-data"></a>Entiteitsgegevens Pattern.any

[Patroon. any](reference-entity-pattern-any.md) is een tijdelijke aanduiding met variabele lengte die alleen wordt gebruikt in de sjabloon van een patroon utterance om te markeren waar de entiteit begint en eindigt.  

## <a name="sentiment-analysis"></a>Sentimentanalyse
Als u sentimentanalyse is geconfigureerd, bevat de json-antwoord van LUIS sentimentanalyse. Meer informatie over sentimentanalyse in de [Tekstanalyse](https://docs.microsoft.com/azure/cognitive-services/text-analytics/) documentatie.

### <a name="sentiment-data"></a>Sentimentsgegevens
Sentimentsgegevens is een score tussen 1 en 0 waarmee wordt aangegeven welke positieve (dichter bij 1) of een negatieve (dichter bij 0) gevoel van de gegevens.

Wanneer de cultuur is `en-us`, het antwoord is:

```JSON
"sentimentAnalysis": {
  "label": "positive",
  "score": 0.9163064
}
```

Voor alle andere culturen en is het antwoord:

```JSON
"sentimentAnalysis": {
  "score": 0.9163064
}
```


### <a name="key-phrase-extraction-entity-data"></a>Sleuteluitdrukkingen extraheren entiteitsgegevens
De entiteit van sleuteluitdrukkingen extraheren retourneert belangrijke zinnen in de utterance, geleverd door [Tekstanalyse](https://docs.microsoft.com/azure/cognitive-services/text-analytics/).


#### <a name="v2-prediction-endpoint-responsetabv2"></a>[V2-antwoord op Voorspellings eindpunt](#tab/V2)

```JSON
{
  "query": "Is there a map of places with beautiful views on a favorite trail?",
  "topScoringIntent": {
    "intent": "GetJobInformation",
    "score": 0.764368951
  },
  "intents": [
    ...
  ],
  "entities": [
    {
      "entity": "beautiful views",
      "type": "builtin.keyPhrase",
      "startIndex": 30,
      "endIndex": 44
    },
    {
      "entity": "map of places",
      "type": "builtin.keyPhrase",
      "startIndex": 11,
      "endIndex": 23
    },
    {
      "entity": "favorite trail",
      "type": "builtin.keyPhrase",
      "startIndex": 51,
      "endIndex": 64
    }
  ]
}
```

#### <a name="v3-prediction-endpoint-responsetabv3"></a>[V3-Voorspellings eindpunt antwoord](#tab/V3)

Meer informatie over het [v3-Voorspellings eindpunt](luis-migration-api-v3.md).

Zonder de query string-para meter, `verbose=true`:

```json
"entities": {
    "keyPhrase": [
        "map of places",
        "beautiful views",
        "favorite trail"
    ]
}
```

Met de query string-para meter, `verbose=true`:

```json
"entities": {
    "keyPhrase": [
        "map of places",
        "beautiful views",
        "favorite trail"
    ],
    "$instance": {
        "keyPhrase": [
            {
                "type": "builtin.keyPhrase",
                "text": "map of places",
                "startIndex": 11,
                "length": 13,
                "modelTypeId": 2,
                "modelType": "Prebuilt Entity Extractor",
                "recognitionSources": [
                    "model"
                ]
            },
            {
                "type": "builtin.keyPhrase",
                "text": "beautiful views",
                "startIndex": 30,
                "length": 15,
                "modelTypeId": 2,
                "modelType": "Prebuilt Entity Extractor",
                "recognitionSources": [
                    "model"
                ]
            },
            {
                "type": "builtin.keyPhrase",
                "text": "favorite trail",
                "startIndex": 51,
                "length": 14,
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

Meer informatie over het [v3-Voorspellings eindpunt](luis-migration-api-v3.md).

* * *


## <a name="data-matching-multiple-entities"></a>Gegevens die overeenkomen met meerdere entiteiten

LUIS retourneert alle entiteiten in de utterance gedetecteerd. Als gevolg hiervan moet de chatbot mogelijk beslissing nemen op basis van de resultaten. Een utterance kan veel entiteiten in een utterance hebben:

`book me 2 adult business tickets to paris tomorrow on air france`

Het LUIS-eind punt kan dezelfde gegevens in verschillende entiteiten detecteren.

#### <a name="v2-prediction-endpoint-responsetabv2"></a>[V2-antwoord op Voorspellings eindpunt](#tab/V2)

```JSON
{
  "query": "book me 2 adult business tickets to paris tomorrow on air france",
  "topScoringIntent": {
    "intent": "BookFlight",
    "score": 1.0
  },
  "intents": [
    {
      "intent": "BookFlight",
      "score": 1.0
    },
    {
      "intent": "Concierge",
      "score": 0.04216196
    },
    {
      "intent": "None",
      "score": 0.03610297
    }
  ],
  "entities": [
    {
      "entity": "air france",
      "type": "Airline",
      "startIndex": 54,
      "endIndex": 63,
      "score": 0.8291798
    },
    {
      "entity": "adult",
      "type": "Category",
      "startIndex": 10,
      "endIndex": 14,
      "resolution": {
        "values": [
          "adult"
        ]
      }
    },
    {
      "entity": "paris",
      "type": "Cities",
      "startIndex": 36,
      "endIndex": 40,
      "resolution": {
        "values": [
          "Paris"
        ]
      }
    },
    {
      "entity": "tomorrow",
      "type": "builtin.datetimeV2.date",
      "startIndex": 42,
      "endIndex": 49,
      "resolution": {
        "values": [
          {
            "timex": "2018-02-21",
            "type": "date",
            "value": "2018-02-21"
          }
        ]
      }
    },
    {
      "entity": "paris",
      "type": "Location::ToLocation",
      "startIndex": 36,
      "endIndex": 40,
      "score": 0.9730773
    },
    {
      "entity": "2",
      "type": "builtin.number",
      "startIndex": 8,
      "endIndex": 8,
      "resolution": {
        "value": "2"
      }
    },
    {
      "entity": "business",
      "type": "Seat",
      "startIndex": 16,
      "endIndex": 23,
      "resolution": {
        "values": [
          "business"
        ]
      }
    },
    {
      "entity": "2 adult business",
      "type": "TicketSeatOrder",
      "startIndex": 8,
      "endIndex": 23,
      "score": 0.8784727
    }
  ],
  "compositeEntities": [
    {
      "parentType": "TicketSeatOrder",
      "value": "2 adult business",
      "children": [
        {
          "type": "Category",
          "value": "adult"
        },
        {
          "type": "builtin.number",
          "value": "2"
        },
        {
          "type": "Seat",
          "value": "business"
        }
      ]
    }
  ]
}
```

#### <a name="v3-prediction-endpoint-responsetabv3"></a>[V3-Voorspellings eindpunt antwoord](#tab/V3)

Zonder `verbose=true` als een query string-para meter.

```json
"entities": {
    "TicketsOrder": [
        {
            "number": [
                2
            ],
            "PassengerCategory": [
                "adult"
            ],
            "TravelClass": [
                "business"
            ]
        }
    ],
    "Location::LocationTo": [
        "paris"
    ],
    "datetimeV2": [
        {
            "type": "date",
            "values": [
                {
                    "timex": "2019-09-28",
                    "value": "2019-09-28"
                }
            ]
        }
    ],
    "Airline": [
        "air france"
    ]
}
```

Met `verbose=true` als een query string-para meter.


```json
"entities": {
    "TicketsOrder": [
        {
            "number": [
                2
            ],
            "PassengerCategory": [
                "adult"
            ],
            "TravelClass": [
                "business"
            ],
            "$instance": {
                "number": [
                    {
                        "type": "builtin.number",
                        "text": "2",
                        "startIndex": 8,
                        "length": 1,
                        "modelTypeId": 2,
                        "modelType": "Prebuilt Entity Extractor",
                        "recognitionSources": [
                            "model"
                        ]
                    }
                ],
                "PassengerCategory": [
                    {
                        "type": "PassengerCategory",
                        "text": "adult",
                        "startIndex": 10,
                        "length": 5,
                        "score": 0.9503733,
                        "modelTypeId": 3,
                        "modelType": "Hierarchical Entity Extractor",
                        "recognitionSources": [
                            "model"
                        ]
                    }
                ],
                "TravelClass": [
                    {
                        "type": "TravelClass",
                        "text": "business",
                        "startIndex": 16,
                        "length": 8,
                        "score": 0.950095,
                        "modelTypeId": 3,
                        "modelType": "Hierarchical Entity Extractor",
                        "recognitionSources": [
                            "model"
                        ]
                    }
                ]
            }
        }
    ],
    "Location::LocationTo": [
        "paris"
    ],
    "datetimeV2": [
        {
            "type": "date",
            "values": [
                {
                    "timex": "2019-09-28",
                    "value": "2019-09-28"
                }
            ]
        }
    ],
    "Airline": [
        "air france"
    ],
    "$instance": {
        "TicketsOrder": [
            {
                "type": "TicketsOrder",
                "text": "2 adult business",
                "startIndex": 8,
                "length": 16,
                "score": 0.942183256,
                "modelTypeId": 4,
                "modelType": "Composite Entity Extractor",
                "recognitionSources": [
                    "model"
                ]
            }
        ],
        "Location::LocationTo": [
            {
                "type": "Location::LocationTo",
                "text": "paris",
                "startIndex": 36,
                "length": 5,
                "score": 0.9905354,
                "modelTypeId": 3,
                "modelType": "Hierarchical Entity Extractor",
                "recognitionSources": [
                    "model"
                ]
            }
        ],
        "datetimeV2": [
            {
                "type": "builtin.datetimeV2.date",
                "text": "tomorrow",
                "startIndex": 42,
                "length": 8,
                "modelTypeId": 2,
                "modelType": "Prebuilt Entity Extractor",
                "recognitionSources": [
                    "model"
                ]
            }
        ],
        "Airline": [
            {
                "type": "Airline",
                "text": "air france",
                "startIndex": 54,
                "length": 10,
                "score": 0.9455415,
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

Meer informatie over het [v3-Voorspellings eindpunt](luis-migration-api-v3.md).

* * *

## <a name="data-matching-multiple-list-entities"></a>Gegevens die overeenkomen met meerdere lijst entiteiten

Als een woord of woordgroep komt overeen met meer dan één entiteit van de lijst, retourneert de query eindpunt elke entiteit die lijst.

Voor de query `when is the best time to go to red rock?`, en de app is het woord `red` in meer dan één lijst LUIS herkent alle entiteiten en retourneert een matrix van entiteiten als onderdeel van het JSON-eindpunt-antwoord: 

#### <a name="v2-prediction-endpoint-responsetabv2"></a>[V2-antwoord op Voorspellings eindpunt](#tab/V2)

```JSON
{
  "query": "when is the best time to go to red rock?",
  "topScoringIntent": {
    "intent": "Calendar.Find",
    "score": 0.06701678
  },
  "entities": [
    {
      "entity": "red",
      "type": "Colors",
      "startIndex": 31,
      "endIndex": 33,
      "resolution": {
        "values": [
          "Red"
        ]
      }
    },
    {
      "entity": "red rock",
      "type": "Cities",
      "startIndex": 31,
      "endIndex": 38,
      "resolution": {
        "values": [
          "Destinations"
        ]
      }
    }
  ]
}
```



#### <a name="v3-prediction-endpoint-responsetabv3"></a>[V3-Voorspellings eindpunt antwoord](#tab/V3)

Zonder `verbose=true` in de query reeks:

```JSON
{
    "query": "when is the best time to go to red rock",
    "prediction": {
        "normalizedQuery": "when is the best time to go to red rock",
        "topIntent": "None",
        "intents": {
            "None": {
                "score": 0.823669851
            }
        },
        "entities": {
            "Colors": [
                [
                    "red"
                ]
            ],
            "Cities": [
                [
                    "Destinations"
                ]
            ]
        }
    }
}
```


Met `verbose=true` in de query reeks:

```JSON
{
    "query": "when is the best time to go to red rock",
    "prediction": {
        "normalizedQuery": "when is the best time to go to red rock",
        "topIntent": "None",
        "intents": {
            "None": {
                "score": 0.823669851
            }
        },
        "entities": {
            "Colors": [
                [
                    "red"
                ]
            ],
            "Cities": [
                [
                    "Destinations"
                ]
            ],
            "$instance": {
                "Colors": [
                    {
                        "type": "Colors",
                        "text": "red",
                        "startIndex": 31,
                        "length": 3,
                        "modelTypeId": 5,
                        "modelType": "List Entity Extractor",
                        "recognitionSources": [
                            "model"
                        ]
                    }
                ],
                "Cities": [
                    {
                        "type": "Cities",
                        "text": "red rock",
                        "startIndex": 31,
                        "length": 8,
                        "modelTypeId": 5,
                        "modelType": "List Entity Extractor",
                        "recognitionSources": [
                            "model"
                        ]
                    }
                ]
            }
        }
    }
}
```

Meer informatie over het [v3-Voorspellings eindpunt](luis-migration-api-v3.md).

* * *

## <a name="next-steps"></a>Volgende stappen

Zie [entiteiten toevoegen](luis-how-to-add-entities.md) voor meer informatie over entiteiten toevoegen aan uw LUIS-app.
