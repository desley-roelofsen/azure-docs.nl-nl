---
title: Sentiment cognitieve vaardigheid
titleSuffix: Azure Cognitive Search
description: Extraheer een positieve, negatieve sentiment-Score van tekst in een AI-pijp lijn in azure Cognitive Search.
manager: nitinme
author: luiscabrer
ms.author: luisca
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 11/04/2019
ms.openlocfilehash: cc3aab703b9c5ffcb5f3280060417ce32fcec2fc
ms.sourcegitcommit: b050c7e5133badd131e46cab144dd5860ae8a98e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/23/2019
ms.locfileid: "72791900"
---
# <a name="sentiment-cognitive-skill"></a>Sentiment cognitieve vaardigheid

De **sentiment** -vaardigheid evalueert ongestructureerde tekst langs een positieve negatieve continuüm, en voor elke record wordt een numerieke Score geretourneerd tussen 0 en 1. Scores dicht bij 1 geven een positieve stemming aan en scores dicht bij 0 geven een negatieve stemming aan. Deze vaardigheid maakt gebruik van de machine learning modellen van [Text Analytics](https://docs.microsoft.com/azure/cognitive-services/text-analytics/overview) in cognitive Services.

> [!NOTE]
> Als u het bereik uitbreidt door de verwerkings frequentie te verhogen, meer documenten toe te voegen of meer AI-algoritmen toe te voegen, moet u [een factureer bare Cognitive Services resource koppelen](cognitive-search-attach-cognitive-services.md). Er worden kosten in rekening gebracht bij het aanroepen van Api's in Cognitive Services en voor het ophalen van afbeeldingen als onderdeel van de fase voor het kraken van documenten in azure Cognitive Search. Er worden geen kosten in rekening gebracht voor het ophalen van tekst uit documenten.
>
> De uitvoering van ingebouwde vaardig heden wordt in rekening gebracht op basis van de bestaande [Cognitive Services betalen naar](https://azure.microsoft.com/pricing/details/cognitive-services/)gebruik-prijs. Prijzen voor Image extractie worden beschreven op de [pagina met prijzen voor Azure Cognitive Search](https://go.microsoft.com/fwlink/?linkid=2042400).


## <a name="odatatype"></a>@odata.type  
Micro soft. skills. Text. SentimentSkill

## <a name="data-limits"></a>Gegevenslimieten
De maximale grootte van een record moet 5000 tekens zijn, zoals gemeten door [`String.Length`](https://docs.microsoft.com/dotnet/api/system.string.length). Als u uw gegevens wilt opsplitsen voordat u deze naar de sentiment Analyzer verzendt, gebruikt u de functie [tekst splitsen](cognitive-search-skill-textsplit.md).


## <a name="skill-parameters"></a>Vaardigheids parameters

Para meters zijn hoofdletter gevoelig.

| Parameternaam |                      |
|----------------|----------------------|
| defaultLanguageCode | Beschrijving De taal code die moet worden toegepast op documenten die geen taal expliciet opgeven. <br/> [Volledige lijst met ondersteunde talen](../cognitive-services/text-analytics/text-analytics-supported-languages.md) weer geven |

## <a name="skill-inputs"></a>Vaardigheids invoer 

| Invoer naam | Beschrijving |
|--------------------|-------------|
| tekst | De tekst die moet worden geanalyseerd.|
| languageCode  |  Beschrijving Een teken reeks die de taal van de records aangeeft. Als deze para meter niet wordt opgegeven, is de standaard waarde "en". <br/>Bekijk de [volledige lijst met ondersteunde talen](../cognitive-services/text-analytics/text-analytics-supported-languages.md).|

## <a name="skill-outputs"></a>Vaardigheids uitvoer

| Uitvoer naam | Beschrijving |
|--------------------|-------------|
| Score | Een waarde tussen 0 en 1 die de sentiment van de geanalyseerde tekst aangeeft. Waarden die dicht bij 0 liggen, hebben een negatieve sentiment, dicht bij 0,5 hebben neutrale sentiment en waarden die dicht bij 1 staan, hebben een positieve sentiment.|


##  <a name="sample-definition"></a>Voorbeeld definitie

```json
{
    "@odata.type": "#Microsoft.Skills.Text.SentimentSkill",
    "inputs": [
        {
            "name": "text",
            "source": "/document/content"
        },
        {
            "name": "languageCode",
            "source": "/document/languagecode"
        }
    ],
    "outputs": [
        {
            "name": "score",
            "targetName": "mySentiment"
        }
    ]
}
```

##  <a name="sample-input"></a>Voorbeeld invoer

```json
{
    "values": [
        {
            "recordId": "1",
            "data": {
                "text": "I had a terrible time at the hotel. The staff was rude and the food was awful.",
                "languageCode": "en"
            }
        }
    ]
}
```


##  <a name="sample-output"></a>Voorbeelduitvoer

```json
{
    "values": [
        {
            "recordId": "1",
            "data": {
                "score": 0.01
            }
        }
    ]
}
```

## <a name="notes"></a>Opmerkingen
Als deze leeg is, wordt er geen sentiment-Score geretourneerd voor die records.

## <a name="error-cases"></a>Fout cases
Als een taal niet wordt ondersteund, wordt er een fout gegenereerd en wordt er geen sentiment Score geretourneerd.

## <a name="see-also"></a>Zie ook

+ [Ingebouwde vaardig heden](cognitive-search-predefined-skills.md)
+ [Een vaardig heden definiëren](cognitive-search-defining-skillset.md)
