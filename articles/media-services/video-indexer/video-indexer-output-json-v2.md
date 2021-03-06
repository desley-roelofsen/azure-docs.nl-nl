---
title: Controleer de Azure Media Services Video Indexer uitvoer die is geproduceerd door v2 API
titleSuffix: Azure Media Services
description: In dit onderwerp wordt de Azure Media Services Video Indexer uitvoer onderzocht die is geproduceerd door v2 API.
services: media-services
author: Juliako
manager: femila
ms.service: media-services
ms.subservice: video-indexer
ms.topic: article
ms.date: 12/09/2019
ms.author: juliako
ms.openlocfilehash: c978fed1675ea80ae9b2f6fb7fbe9a4c84472638
ms.sourcegitcommit: 5ab4f7a81d04a58f235071240718dfae3f1b370b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/10/2019
ms.locfileid: "74978301"
---
# <a name="examine-the-video-indexer-output-produced-by-api"></a>Bekijk de Video Indexer uitvoer die door de API is geproduceerd

Wanneer u de API **Get video-index** aanroept en de reactie status OK is, krijgt u een gedetailleerde JSON-uitvoer als de antwoord inhoud. De JSON-inhoud bevat details van de opgegeven video inzichten. De inzichten bevatten dimensies als: transcripten, OCRs, gezichten, onderwerpen, blokken, enzovoort. De dimensies hebben exemplaren van peri Oden die laten zien wanneer elke dimensie in de video wordt weer gegeven.  

1. Als u het JSON-bestand wilt ophalen, roept u [video-index ophalen](https://api-portal.videoindexer.ai/docs/services/Operations/operations/Get-Video-Index?) op
1. Als u ook specifieke artefacten wilt ontvangen, roept u [video artefact downloaden URL API](https://api-portal.videoindexer.ai/docs/services/Operations/operations/Get-Video-Artifact-Download-Url?) op

    Geef in de API-aanroep het aangevraagde artefact type op (OCR, gezichten, keyframes, enzovoort)

U kunt de samen vatting van de video ook visueel bekijken door op de knop **afspelen** op de video op de [video indexer](https://www.videoindexer.ai/) website te drukken. Zie [video Insights weer geven en bewerken](video-indexer-view-edit.md)voor meer informatie.

![Inzichten](./media/video-indexer-output-json/video-indexer-summarized-insights.png)

In dit artikel wordt de JSON-inhoud onderzocht die wordt geretourneerd door de API **video-index ophalen** . 

> [!NOTE]
> Het verlopen van alle toegangs tokens in Video Indexer is een uur.


## <a name="root-elements"></a>Hoofd elementen

|Naam|Beschrijving|
|---|---|
|accountId|De VI-account-ID van de afspeel lijst.|
|id|De ID van de afspeel lijst.|
|name|De naam van de afspeel lijst.|
|description|De beschrijving van de afspeel lijst.|
|userName|De naam van de gebruiker die de afspeel lijst heeft gemaakt.|
|toegevoegd|De aanmaak tijd van de afspeel lijst.|
|privacyMode|De privacy-modus van de afspeel lijst (privé/openbaar).|
|state|De afspeel lijst (geüpload, verwerking, verwerkt, mislukt, in quarantaine geplaatst).|
|isOwned|Hiermee wordt aangegeven of de afspeel lijst is gemaakt door de huidige gebruiker.|
|isEditable|Hiermee wordt aangegeven of de huidige gebruiker gemachtigd is om de afspeel lijst te bewerken.|
|isBase|Hiermee wordt aangegeven of de afspeel lijst een basis-afspeel lijst (een video) of een afspeel lijst is gemaakt van andere Video's (afgeleid).|
|durationInSeconds|De totale duur van de afspeel lijst.|
|summarizedInsights|Bevat één [summarizedInsights](#summarizedinsights).
|video's|Een lijst met [Video's](#videos) die de afspeel lijst bouwen.<br/>Als deze afspeel lijst met geconstrueerde Peri Oden van andere Video's (afgeleid) is, bevatten de Video's in deze lijst alleen gegevens uit de opgenomen Peri Oden.|

```json
{
  "accountId": "bca61527-1221-bca6-1527-a90000002000",
  "id": "abc3454321",
  "name": "My first video",
  "description": "I am trying VI",
  "userName": "Some name",
  "created": "2018/2/2 18:00:00.000",
  "privacyMode": "Private",
  "state": "Processed",
  "isOwned": true,
  "isEditable": false,
  "isBase": false,
  "durationInSeconds": 120, 
  "summarizedInsights" : { . . . }
  "videos": [{ . . . }]
}
```

## <a name="summarizedinsights"></a>summarizedInsights

In deze sectie vindt u een overzicht van de inzichten.

|Kenmerk | Beschrijving|
|---|---|
|name|De naam van de video. Bijvoorbeeld Azure Monitor.|
|id|De ID van de video. Bijvoorbeeld 63c6d532ff.|
|privacyMode|Uw uitsplitsing kan een van de volgende modi hebben: **persoonlijk**, **openbaar**. **Openbaar** : de video is zichtbaar voor iedereen in uw account en iedereen met een koppeling naar de video. **Persoonlijk** : de video is zichtbaar voor iedereen in uw account.|
|duration|Bevat één duur die aangeeft hoe lang een inzicht heeft plaatsgevonden. De duur is in seconden.|
|thumbnailVideoId|De ID van de video waarvan de miniatuur is gemaakt.
|thumbnailId|De miniatuur-ID van de video. Als u de daad werkelijke miniatuur wilt ophalen, roept u [Get-thumbnail](https://api-portal.videoindexer.ai/docs/services/operations/operations/Get-Video-Thumbnail) aan en geeft u deze ThumbnailVideoId en thumbnailId door.|
|aanhoudende|Kan nul of meer gezichten bevatten. Zie [gezichten](#faces)voor meer gedetailleerde informatie.|
|trefwoorden|Kan nul of meer tref woorden bevatten. Zie [tref woorden](#keywords)voor meer gedetailleerde informatie.|
|gevoel|Kan nul of meer gevoel bevatten. Zie [gevoel](#sentiments)voor meer gedetailleerde informatie.|
|audioEffects| Kan nul of meer audioEffects bevatten. Zie [audioEffects](#audioEffects)voor meer gedetailleerde informatie.|
|labels| Mag niet meer dan nul labels bevatten. Zie [labels](#labels)voor meer informatie.|
|merken| Kan nul of meer merken bevatten. Zie [Brands](#brands)voor meer gedetailleerde informatie.|
|statistieken | Zie [Statistieken](#statistics)voor meer informatie.|
|emoties| Kan nul of meer emoties bevatten. Zie [emoties](#emotions)voor meer gedetailleerde informatie.|
|-onderwerpen|Kan nul of meer onderwerpen bevatten. De dimensie [onderwerpen](#topics) .|

## <a name="videos"></a>video's

|Naam|Beschrijving|
|---|---|
|accountId|De VI-account-ID van de video.|
|id|De ID van de video.|
|name|De naam van de video.
|state|De status van de video (geüpload, verwerkt, verwerkt, mislukt, in quarantaine geplaatst).|
|processingProgress|De verwerkings voortgang tijdens de verwerking (bijvoorbeeld 20%).|
|failureCode|De fout code als de verwerking is mislukt (bijvoorbeeld ' UnsupportedFileType ').|
|failureMessage|Het fout bericht als de verwerking is mislukt.|
|externalId|De externe ID van de video (indien opgegeven door de gebruiker).|
|externalUrl|De externe URL van de video (indien opgegeven door de gebruiker).|
|metagegevens|De externe meta gegevens van de video (indien opgegeven door de gebruiker).|
|isAdult|Hiermee wordt aangegeven of de video hand matig is gecontroleerd en geïdentificeerd als een volwassene video.|
|Insights|Het Insights-object. Zie [inzichten](#insights)voor meer informatie.|
|thumbnailId|De miniatuur-ID van de video. Als u de daad werkelijke miniatuur wilt ophalen, kunt u een [miniatuur](https://api-portal.videoindexer.ai/docs/services/operations/operations/Get-Video-Thumbnail) maken en de video-id en thumbnailId door geven.|
|publishedUrl|Een URL voor het streamen van de video.|
|publishedUrlProxy|Een URL voor het streamen van de video van (voor Apple-apparaten).|
|viewToken|Een kort weer gave-token voor het streamen van de video.|
|sourceLanguage|De bron taal van de video.|
|language|De werkelijke taal van de video (vertaling).|
|indexingPreset|De definitie die wordt gebruikt om de video te indexeren.|
|streamingPreset|De voor instelling die wordt gebruikt voor het publiceren van de video.|
|linguisticModelId|Het CRI'S-model dat wordt gebruikt om de video te transcriberen.|
|statistieken | Zie [Statistieken](#statistics)voor meer informatie.|

```json
{
    "videos": [{
        "accountId": "2cbbed36-1972-4506-9bc7-55367912df2d",
        "id": "142a356aa6",
        "state": "Processed",
        "privacyMode": "Private",
        "processingProgress": "100%",
        "failureCode": "General",
        "failureMessage": "",
        "externalId": null,
        "externalUrl": null,
        "metadata": null,
        "insights": {. . . },
        "thumbnailId": "89d7192c-1dab-4377-9872-473eac723845",
        "publishedUrl": "https://videvmediaservices.streaming.mediaservices.windows.net:443/d88a652d-334b-4a66-a294-3826402100cd/Xamarine.ism/manifest",
        "publishedProxyUrl": null,
        "viewToken": "Bearer=<token>",
        "sourceLanguage": "En-US",
        "language": "En-US",
        "indexingPreset": "Default",
        "linguisticModelId": "00000000-0000-0000-0000-000000000000"
    }],
}
```
### <a name="insights"></a>Insights

De inzichten zijn een set dimensies (bijvoorbeeld transcript regels, gezichten, Brands enz.), waarbij elke dimensie een lijst met unieke elementen is (bijvoorbeeld face1, face2, face3), en elk element heeft zijn eigen meta gegevens en een lijst met de bijbehorende instanties (die peri Oden zijn met aanvullende optionele meta gegevens).

Een gezicht kan een ID, een naam, een miniatuur, andere meta gegevens en een lijst met de tijdelijke instanties hebben (bijvoorbeeld: 00:00:05 – 00:00:10, 00:01:00-00:02:30 en 00:41:21 – 00:41:49.) Elke tijdelijke instantie kan aanvullende meta gegevens bevatten. Bijvoorbeeld: de coördinaten van het gezicht (20230, 60, 60).

|Version|De code versie|
|---|---|
|sourceLanguage|De bron taal van de video (uitgaande van één hoofd taal). In de vorm van een [bcp-47-](https://tools.ietf.org/html/bcp47) teken reeks.|
|language|De Insights-taal (vertaald van de bron taal). In de vorm van een [bcp-47-](https://tools.ietf.org/html/bcp47) teken reeks.|
|verslag|De dimensie [transcript](#transcript) .|
|optische|De [OCR](#ocr) -dimensie.|
|trefwoorden|De dimensie [tref woorden](#keywords) .|
|blokken|Kan een of meer [blokken](#blocks) bevatten|
|aanhoudende|De dimensie [gezichten](#faces) .|
|labels|De dimensie [labels](#labels) .|
|afzonderlijke|De dimensie voor de [opnamen](#shots) .|
|merken|De dimensie [Brands](#brands) .|
|audioEffects|De dimensie [audioEffects](#audioEffects) .|
|gevoel|De dimensie [gevoel](#sentiments) .|
|visualContentModeration|De dimensie [visualContentModeration](#visualcontentmoderation) .|
|textualContentModeration|De dimensie [textualContentModeration](#textualcontentmoderation) .|
|emoties| De dimensie [emoties](#emotions) .|
|-onderwerpen|De dimensie [onderwerpen](#topics) .|

Voorbeeld:

```json
{
  "version": "0.9.0.0",
  "sourceLanguage": "en-US",
  "language": "es-ES",
  "transcript": ...,
  "ocr": ...,
  "keywords": ...,
  "faces": ...,
  "labels": ...,
  "shots": ...,
  "brands": ...,
  "audioEffects": ...,
  "sentiments": ...,
  "visualContentModeration": ...,
  "textualContentModeration": ...
}
```

#### <a name="blocks"></a>blokken

Kenmerk | Beschrijving
---|---
id|De ID van het blok.|
instanties|Een lijst met tijds bereiken van dit blok.|

#### <a name="transcript"></a>verslag

|Naam|Beschrijving|
|---|---|
|id|De regel-ID.|
|tekst|De transcriptie zelf.|
|language|De transcript taal. Bedoeld ter ondersteuning van transcripten waarbij elke regel een andere taal kan hebben.|
|instanties|Een lijst met peri Oden waarin deze regel wordt weer gegeven. Als de instantie een transcript heeft, heeft deze slechts één exemplaar.|

Voorbeeld:

```json
"transcript": [
{
    "id": 0,
    "text": "Hi I'm Doug from office.",
    "language": "en-US",
    "instances": [
    {
        "start": "00:00:00.5100000",
        "end": "00:00:02.7200000"
    }
    ]
},
{
    "id": 1,
    "text": "I have a guest. It's Michelle.",
    "language": "en-US",
    "instances": [
    {
        "start": "00:00:02.7200000",
        "end": "00:00:03.9600000"
    }
    ]
}
] 
```

#### <a name="ocr"></a>optische

|Naam|Beschrijving|
|---|---|
|id|De OCR-regel-ID.|
|tekst|De OCR-tekst.|
|betrouwbaarheid|Het vertrouwens niveau van de herkenning.|
|language|De OCR-taal.|
|instanties|Een lijst met peri Oden waarin deze OCR is verschenen (dezelfde OCR kan meerdere keren voor komen).|
|hoogte|De hoogte van de OCR-rechthoek|
|top|De bovenste locatie in px|
|links| De vestiging links in px|
|breedte|De breedte van de OCR-rechthoek|

```json
"ocr": [
    {
      "id": 0,
      "text": "LIVE FROM NEW YORK",
      "confidence": 675.971,
      "height": 35,
      "language": "en-US",
      "left": 31,
      "top": 97,
      "width": 400,      
      "instances": [
        {
          "start": "00:00:26",
          "end": "00:00:52"
        }
      ]
    }
  ],
```

#### <a name="keywords"></a>trefwoorden

|Naam|Beschrijving|
|---|---|
|id|De ID van het sleutel woord.|
|tekst|De tekst van het sleutel woord.|
|betrouwbaarheid|Het vertrouwens niveau van het tref woord.|
|language|De taal van het tref woord (tijdens het vertalen).|
|instanties|Een lijst met tijds bereiken waar dit tref woord is opgetreden (een tref woord kan meerdere keren voor komen).|

```json
{
    id: 0,
    text: "technology",
    confidence: 1,
    language: "en-US",
    instances: [{
            adjustedStart: "0:05:15.782",
            adjustedEnd: "0:05:16.249",
            start: "0:05:15.782",
            end: "0:05:16.249"
    },
    {
            adjustedStart: "0:04:54.761",
            adjustedEnd: "0:04:55.228",
            start: "0:04:54.761",
            end: "0:04:55.228"
    }]
}
```

#### <a name="faces"></a>aanhoudende

|Naam|Beschrijving|
|---|---|
|id|De face-ID.|
|name|De naam van het gezicht. Dit kan ' onbekend #0, een geïdentificeerde beroemdheden of een door de klant getrainde persoon zijn.|
|betrouwbaarheid|De gezichts-id-betrouw baarheid.|
|description|Een beschrijving van de beroemdheden. |
|thumbnailId|De ID van de miniatuur van het gezicht.|
|knownPersonId|De interne ID van een bekende persoon.|
|referenceId|Als het een Bing-beroemdheden is, is dit de Bing-ID.|
|Type|Op dit moment hebben we alleen Bing.|
|titel|Als het een beroemdheden is, is dit de titel (bijvoorbeeld ' micro soft CEO ').|
|imageUrl|Als het een beroemdheden is, wordt de afbeeldings-URL.|
|instanties|Dit zijn exemplaren van waar het gezicht zich in het opgegeven tijds bereik bevindt. Elk exemplaar heeft ook een thumbnailsId. |

```json
"faces": [{
    "id": 2002,
    "name": "Xam 007",
    "confidence": 0.93844,
    "description": null,
    "thumbnailId": "00000000-aee4-4be2-a4d5-d01817c07955",
    "knownPersonId": "8340004b-5cf5-4611-9cc4-3b13cca10634",
    "referenceId": null,
    "title": null,
    "imageUrl": null,
    "instances": [{
        "thumbnailsIds": ["00000000-9f68-4bb2-ab27-3b4d9f2d998e",
        "cef03f24-b0c7-4145-94d4-a84f81bb588c"],
        "adjustedStart": "00:00:07.2400000",
        "adjustedEnd": "00:00:45.6780000",
        "start": "00:00:07.2400000",
        "end": "00:00:45.6780000"
    },
    {
        "thumbnailsIds": ["00000000-51e5-4260-91a5-890fa05c68b0"],
        "adjustedStart": "00:10:23.9570000",
        "adjustedEnd": "00:10:39.2390000",
        "start": "00:10:23.9570000",
        "end": "00:10:39.2390000"
    }]
}]
```

#### <a name="labels"></a>labels

|Naam|Beschrijving|
|---|---|
|id|De label-ID.|
|name|De naam van het label (bijvoorbeeld ' computer ', ' TV ').|
|language|De naam taal van het label (bij omzetting). BCP-47|
|instanties|Een lijst met tijds bereiken waar dit label wordt weer gegeven (een label kan meerdere keren voor komen). Elk exemplaar heeft een veld betrouw baarheid. |


```json
"labels": [
    {
      "id": 0,
      "name": "person",
      "language": "en-US",
      "instances": [
        {
          "confidence": 1.0,
          "start": "00: 00: 00.0000000",
          "end": "00: 00: 25.6000000"
        },
        {
          "confidence": 1.0,
          "start": "00: 01: 33.8670000",
          "end": "00: 01: 39.2000000"
        }
      ]
    },
    {
      "name": "indoor",
      "language": "en-US",
      "id": 1,
      "instances": [
        {
          "confidence": 1.0,
          "start": "00: 00: 06.4000000",
          "end": "00: 00: 07.4670000"
        },
        {
          "confidence": 1.0,
          "start": "00: 00: 09.6000000",
          "end": "00: 00: 10.6670000"
        },
        {
          "confidence": 1.0,
          "start": "00: 00: 11.7330000",
          "end": "00: 00: 20.2670000"
        },
        {
          "confidence": 1.0,
          "start": "00: 00: 21.3330000",
          "end": "00: 00: 25.6000000"
        }
      ]
    }
  ] 
```

#### <a name="scenes"></a>scene

|Naam|Beschrijving|
|---|---|
|id|De scène-ID.|
|instanties|Een lijst met tijds bereiken van deze scène (een scène kan slechts één exemplaar hebben).|

```json
"scenes":[  
    {  
      "id":0,
      "instances":[  
          {  
            "start":"0:00:00",
            "end":"0:00:06.34",
            "duration":"0:00:06.34"
          }
      ]
    },
    {  
      "id":1,
      "instances":[  
          {  
            "start":"0:00:06.34",
            "end":"0:00:47.047",
            "duration":"0:00:40.707"
          }
      ]
    },

]
```

#### <a name="shots"></a>afzonderlijke

|Naam|Beschrijving|
|---|---|
|id|De opname-ID.|
|keyFrames|Een lijst met keyframes in de foto (elke bevat een ID en een lijst met tijds bereik exemplaren). Elk keyframe-exemplaar heeft een thumbnailId-veld met de miniatuur-ID van het keyframe.|
|instanties|Een lijst met tijds bereiken van deze opname (een opname kan slechts 1 exemplaar hebben).|

```json
"shots":[  
    {  
      "id":0,
      "keyFrames":[  
          {  
            "id":0,
            "instances":[  
                {  
                  "thumbnailId":"00000000-0000-0000-0000-000000000000",
                  "start":"0:00:00.209",
                  "end":"0:00:00.251",
                  "duration":"0:00:00.042"
                }
            ]
          },
          {  
            "id":1,
            "instances":[  
                {  
                  "thumbnailId":"00000000-0000-0000-0000-000000000000",
                  "start":"0:00:04.755",
                  "end":"0:00:04.797",
                  "duration":"0:00:00.042"
                }
            ]
          }
      ],
      "instances":[  
          {  
            "start":"0:00:00",
            "end":"0:00:06.34",
            "duration":"0:00:06.34"
          }
      ]
    },

]
```

#### <a name="brands"></a>merken

Merk namen van bedrijven en producten die worden herkend in de spraak naar tekst transcriptie en/of video OCR. Dit omvat geen visuele herkenning van merken of logo detectie.

|Naam|Beschrijving|
|---|---|
|id|De merk-ID.|
|name|De naam van het merk.|
|referenceId | Het achtervoegsel van de brand Wikipedia-URL. "Target_Corporation" is bijvoorbeeld het achtervoegsel van [https://en.wikipedia.org/wiki/Target_Corporation](https://en.wikipedia.org/wiki/Target_Corporation).
|referenceUrl | De Wikipedia-URL van het merk, indien aanwezig. Bijvoorbeeld [https://en.wikipedia.org/wiki/Target_Corporation](https://en.wikipedia.org/wiki/Target_Corporation).
|description|De beschrijving van de Brands.|
|tags|Een lijst met vooraf gedefinieerde labels die aan dit merk zijn gekoppeld.|
|betrouwbaarheid|De betrouw bare waarde van de Video Indexer merk detector (0-1).|
|instanties|Een lijst met tijds bereiken van dit merk. Elk exemplaar heeft een brandType, waarmee wordt aangegeven of dit merk zich in het transcript of in de OCR bevindt.|

```json
"brands": [
{
    "id": 0,
    "name": "MicrosoftExcel",
    "referenceId": "Microsoft_Excel",
    "referenceUrl": "http: //en.wikipedia.org/wiki/Microsoft_Excel",
    "referenceType": "Wiki",
    "description": "Microsoft Excel is a sprea..",
    "tags": [],
    "confidence": 0.975,
    "instances": [
    {
        "brandType": "Transcript",
        "start": "00: 00: 31.3000000",
        "end": "00: 00: 39.0600000"
    }
    ]
},
{
    "id": 1,
    "name": "Microsoft",
    "referenceId": "Microsoft",
    "referenceUrl": "http: //en.wikipedia.org/wiki/Microsoft",
    "description": "Microsoft Corporation is...",
    "tags": [
    "competitors",
    "technology"
    ],
    "confidence": 1.0,
    "instances": [
    {
        "brandType": "Transcript",
        "start": "00: 01: 44",
        "end": "00: 01: 45.3670000"
    },
    {
        "brandType": "Ocr",
        "start": "00: 01: 54",
        "end": "00: 02: 45.3670000"
    }
    ]
}
]
```

#### <a name="statistics"></a>statistieken

|Naam|Beschrijving|
|---|---|
|CorrespondenceCount|Aantal correspondentie in de video.|
|SpeakerWordCount|Het aantal woorden per spreker.|
|SpeakerNumberOfFragments|De hoeveelheid fragmenten die de spreker in een video heeft.|
|SpeakerLongestMonolog|De langste monolog van de spreker. Als de spreker stiltes bevat in de monolog, is deze opgenomen. Stilte aan het begin en het einde van de monolog wordt verwijderd.| 
|SpeakerTalkToListenRatio|De berekening is gebaseerd op de tijd die is besteed aan de monolog van de spreker (zonder de stilte in tussen) gedeeld door de totale tijd van de video. De tijd wordt afgerond op het derde decimale punt.|

#### <a name="a-idaudioeffectsaudioeffects"></a><a id="audioEffects"/>audioEffects

|Naam|Beschrijving|
|---|---|
|id|De audio-effect-ID.|
|type|Het type audio-effect (bijvoorbeeld Clapping, Speech, stilte).|
|instanties|Een lijst met peri Oden waarin dit audio-effect is opgetreden.|

```json
"audioEffects": [
{
    "id": 0,
    "type": "Clapping",
    "instances": [
    {
        "start": "00:00:00",
        "end": "00:00:03"
    },
    {
        "start": "00:01:13",
        "end": "00:01:21"
    }
    ]
}
]
```

#### <a name="sentiments"></a>gevoel

Gevoel worden geaggregeerd met het veld sentimentType (positief/neutraal/negatief). Bijvoorbeeld: 0-0,1, 0,1-0,2.

|Naam|Beschrijving|
|---|---|
|id|De sentiment-ID.|
|averageScore |Het gemiddelde van alle scores van alle exemplaren van dat sentiment type-positief/neutraal/negatief|
|instanties|Een lijst met tijds bereikn waar deze sentiment verscheen.|
|sentimentType |Het type kan ' positief ', ' neutraal ' of ' negatief ' zijn.|

```json
"sentiments": [
{
    "id": 0,
    "averageScore": 0.87,
    "sentimentType": "Positive",
    "instances": [
    {
        "start": "00:00:23",
        "end": "00:00:41"
    }
    ]
}, {
    "id": 1,
    "averageScore": 0.11,
    "sentimentType": "Positive",
    "instances": [
    {
        "start": "00:00:13",
        "end": "00:00:21"
    }
    ]
}
]
```

#### <a name="visualcontentmoderation"></a>visualContentModeration

Het visualContentModeration-blok bevat Peri Oden die Video Indexer mogelijk inhoud voor volwassenen hebben gevonden. Als visualContentModeration leeg is, is er geen inhoud voor volwassenen gevonden.

Video's die een inhoud van volwassenen of ongepaste bevatten, zijn mogelijk alleen beschikbaar voor de persoonlijke weer gave. Gebruikers hebben de mogelijkheid om een aanvraag in te dienen voor een menselijke beoordeling van de inhoud. in dat geval zal het kenmerk IsAdult het resultaat van de beoordeling van de mens bevatten.

|Naam|Beschrijving|
|---|---|
|id|De controle-ID van de visuele inhoud.|
|adultScore|De volwassen Score (van content moderator).|
|racyScore|De ongepaste-Score (van inhouds toezicht).|
|instanties|Een lijst met peri Oden waarin deze visuele elementen worden gevisualiseerd.|

```json
"VisualContentModeration": [
{
    "id": 0,
    "adultScore": 0.00069,
    "racyScore": 0.91129,
    "instances": [
    {
        "start": "00:00:25.4840000",
        "end": "00:00:25.5260000"
    }
    ]
},
{
    "id": 1,
    "adultScore": 0.99231,
    "racyScore": 0.99912,
    "instances": [
    {
        "start": "00:00:35.5360000",
        "end": "00:00:35.5780000"
    }
    ]
}
] 
```

#### <a name="textualcontentmoderation"></a>textualContentModeration 

|Naam|Beschrijving|
|---|---|
|id|De moderator-ID van de tekst inhoud.|
|bannedWordsCount |Het aantal verboden woorden.|
|bannedWordsRatio |De verhouding van het totale aantal woorden.|

#### <a name="emotions"></a>emoties

Video Indexer identificeert emoties op basis van spraak-en audio-hints. De geïdentificeerde Emotion kan zijn: Joy, verdriet, boosheid of vrezen.

|Naam|Beschrijving|
|---|---|
|id|De Emotion-ID.|
|type|Het EMOTION-tijdstip dat is geïdentificeerd, is gebaseerd op spraak-en audio hints. De EMOTION kan worden: Joy, verdriet, boosheid of vrezen.|
|instanties|Een lijst met tijds bereikn waar deze Emotion verscheen.|

```json
"emotions": [{
    "id": 0,
    "type": "Fear",
    "instances": [{
      "adjustedStart": "0:00:39.47",
      "adjustedEnd": "0:00:45.56",
      "start": "0:00:39.47",
      "end": "0:00:45.56"
    },
    {
      "adjustedStart": "0:07:19.57",
      "adjustedEnd": "0:07:23.25",
      "start": "0:07:19.57",
      "end": "0:07:23.25"
    }]
  },
  {
    "id": 1,
    "type": "Anger",
    "instances": [{
      "adjustedStart": "0:03:55.99",
      "adjustedEnd": "0:04:05.06",
      "start": "0:03:55.99",
      "end": "0:04:05.06"
    },
    {
      "adjustedStart": "0:04:56.5",
      "adjustedEnd": "0:05:04.35",
      "start": "0:04:56.5",
      "end": "0:05:04.35"
    }]
  },
  {
    "id": 2,
    "type": "Joy",
    "instances": [{
      "adjustedStart": "0:12:23.68",
      "adjustedEnd": "0:12:34.76",
      "start": "0:12:23.68",
      "end": "0:12:34.76"
    },
    {
      "adjustedStart": "0:12:46.73",
      "adjustedEnd": "0:12:52.8",
      "start": "0:12:46.73",
      "end": "0:12:52.8"
    },
    {
      "adjustedStart": "0:30:11.29",
      "adjustedEnd": "0:30:16.43",
      "start": "0:30:11.29",
      "end": "0:30:16.43"
    },
    {
      "adjustedStart": "0:41:37.23",
      "adjustedEnd": "0:41:39.85",
      "start": "0:41:37.23",
      "end": "0:41:39.85"
    }]
  },
  {
    "id": 3,
    "type": "Sad",
    "instances": [{
      "adjustedStart": "0:13:38.67",
      "adjustedEnd": "0:13:41.3",
      "start": "0:13:38.67",
      "end": "0:13:41.3"
    },
    {
      "adjustedStart": "0:28:08.88",
      "adjustedEnd": "0:28:18.16",
      "start": "0:28:08.88",
      "end": "0:28:18.16"
    }]
  }
],
```

#### <a name="topics"></a>-onderwerpen

Video Indexer van de belangrijkste onderwerpen van transcripten wordt verduidelijkt. Als dat mogelijk is, wordt de [IPTC](https://iptc.org/standards/media-topics/) -taxonomie van het 2de niveau opgenomen. 

|Naam|Beschrijving|
|---|---|
|id|De onderwerp-ID.|
|name|De naam van het onderwerp, bijvoorbeeld ' Farmaceutischen '.|
|referenceId|Brood kruimels die de hiërarchie van onderwerpen weer spie gelen. Bijvoorbeeld: "Health en Wellbeing/medicijn en gezondheids zorg/farmaceutische bedrijven".|
|betrouwbaarheid|De betrouwbaarheids Score in het bereik [0, 1]. Hoger is meer vertrouwen.|
|language|De taal die wordt gebruikt in het onderwerp.|
|iptcName|De naam van de IPTC-media code, indien gedetecteerd.|
|instanties |Momenteel indexeert Video Indexer geen onderwerp naar tijds intervallen, zodat de hele video wordt gebruikt als het interval.|

```json
"topics": [{
    "id": 0,
    "name": "INTERNATIONAL RELATIONS",
    "referenceId": "POLITICS AND GOVERNMENT/FOREIGN POLICY/INTERNATIONAL RELATIONS",
    "referenceType": "VideoIndexer",
    "confidence": 1,
    "language": "en-US",
    "instances": [{
        "adjustedStart": "0:00:00",
        "adjustedEnd": "0:03:36.25",
        "start": "0:00:00",
        "end": "0:03:36.25"
    }]
}, {
    "id": 1,
    "name": "Politics and Government",
    "referenceType": "VideoIndexer",
    "iptcName": "Politics",
    "confidence": 0.9041,
    "language": "en-US",
    "instances": [{
        "adjustedStart": "0:00:00",
        "adjustedEnd": "0:03:36.25",
        "start": "0:00:00",
        "end": "0:03:36.25"
    }]
}]
. . .
```

## <a name="next-steps"></a>Volgende stappen

[Video Indexer ontwikkelaars Portal](https://api-portal.videoindexer.ai)

Zie [video indexer widgets insluiten in uw toepassingen](video-indexer-embed-widgets.md)voor meer informatie over het insluiten van widgets in uw toepassing. 

