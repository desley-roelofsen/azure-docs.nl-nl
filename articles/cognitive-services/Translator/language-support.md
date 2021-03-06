---
title: Taal ondersteuning-Translator Text-API
titleSuffix: Azure Cognitive Services
description: De Translator Text-API ondersteunt de volgende talen voor tekst omzetting met Neural machine translation (NMT).
services: cognitive-services
author: swmachan
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: reference
ms.date: 12/02/2019
ms.author: swmachan
ms.openlocfilehash: 62c101751e07d8ee31789191ad45fbdd33a1bc4b
ms.sourcegitcommit: c69c8c5c783db26c19e885f10b94d77ad625d8b4
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/03/2019
ms.locfileid: "74707966"
---
# <a name="language-and-region-support-for-the-translator-text-api"></a>Ondersteuning van talen en regio's voor de Translator Text-API

De Translator Text-API ondersteunt de volgende talen voor tekst vertaling. Neural machine translation (NMT) is de nieuwe standaard voor automatische vertalingen van een hoge kwaliteit en is beschikbaar als de standaard waarde met behulp van v3 van de Translator Text-API wanneer er een Neural-systeem beschikbaar is.

[Meer informatie over hoe automatische vertaling werkt](https://www.microsoft.com/translator/mt.aspx)

## <a name="translation"></a>Omzetting

**V2 Translator-API**

> [!NOTE]
> V2 is op 30 april 2018 afgeschaft. Migreer uw toepassingen naar v3 om te kunnen profiteren van de nieuwe functionaliteit die alleen beschikbaar is in v3.

* Alleen statistisch: er is geen Neural systeem beschikbaar voor deze taal.
* Neural beschikbaar: er is een Neural-systeem beschikbaar. Gebruik de para meter `category=generalnn` om toegang te krijgen tot het Neural-systeem.
* Neural standaard: Neural is het standaard Vertaal systeem. Gebruik de para meter `category=smt` om toegang te krijgen tot het statistische systeem voor gebruik met de micro soft Translator-hub.
* Alleen Neural: alleen Neural-omzetting is beschikbaar.

**V3 Translator-API** De V3 Translator-API is Neural standaard en statistische systemen zijn alleen beschikbaar als er geen Neural-systeem bestaat.

> [!NOTE]
> Momenteel is een subset van de Neural-talen beschikbaar in het aangepaste conversie programma en worden er geleidelijk extra toegevoegd. [Talen weer geven die momenteel beschikbaar zijn in het aangepaste conversie programma](#customization).

|Taal|  Taalcode|  V2-API| V3-API|
|:-----|:-----:|:-----|:-----|
|Afrikaans| `af`    |Alleen statistisch|  Neuraal|
|Arabisch|    `ar`    |Neural beschikbaar|  Neuraal|
|Bengalese|    `bn`    |Neural beschikbaar|  Neuraal|
|Bosnisch (Latijns)|   `bs`    |Neural beschikbaar|  Neuraal|
|Bulgaars| `bg`    |Neural beschikbaar|  Neuraal|
|Kantonees (traditioneel)|   `yue`   |Alleen statistisch|  Statische|
|Catalaans|   `ca`    |Alleen statistisch|  Statische|
|Vereenvoudigd Chinees|    `zh-Hans`   |Neural standaard |Neuraal|
|Traditioneel Chinees|   `zh-Hant`   |Neural standaard |Neuraal|
|Kroatisch|  `hr`    |Neural beschikbaar|  Neuraal|
|Tsjechisch| `cs`    |Neural beschikbaar|  Neuraal|
|Deens|    `da`    |Neural beschikbaar   |Neuraal|
|Nederlands| `nl`    |Neural beschikbaar|  Neuraal|
|Nederlands|   `en`    |Neural beschikbaar|  Neuraal|
|Estisch|  `et`    |Neural beschikbaar|  Neuraal|
|Fijian|    `fj`    |Alleen statistisch|  Statische|
|Filipijns|  `fil`   |Alleen statistisch|  Statische|
|Fins|   `fi`    |Neural beschikbaar|  Neuraal|
|Frans|    `fr`    |Neural beschikbaar|  Neuraal|
|Duits|    `de`    |Neural beschikbaar|  Neuraal|
|Grieks| `el`    |Neural beschikbaar|  Neuraal|
|Haïtiaans|    `ht`    |Alleen statistisch   |Statische|
|Hebreeuws |`he`   |Neural beschikbaar   |Neuraal|
|Hindi| `hi`    |Neural standaard|    Neuraal|
|Hmong Daw| `mww`   |Alleen statistisch|  Statische|
|Hongaars| `hu`    |Neural beschikbaar|  Neuraal|
|IJslands| `is`    |Alleen Neural|   Neuraal|
|Indonesisch|    `id`    |Alleen statistisch|  Statische|
|Italiaans|   `it`    |Neural beschikbaar|  Neuraal|
|Japans|  `ja`    |Neural beschikbaar|  Neuraal|
|Swahili| `sw`    |Alleen statistisch|  Statische|
|Klingon|   `tlh`   |Alleen statistisch|  Statische|
|Klingon (plqaD)|   `tlh-Qaak`  |Alleen statistisch|  Statische|
|Koreaans |`ko`   |Neural beschikbaar|  Neuraal|
|Lets|   `lv`    |Neural beschikbaar|  Neuraal|
|Litouws|    `lt`    |Neural beschikbaar|  Neuraal|
|Malagassische|  `mg`    |Alleen statistisch|  Statische|
|Maleis| `ms`    |Alleen statistisch   |Statische|
|Maltees|   `mt`    |Alleen statistisch|  Statische|
|Maori| `mi`  |Alleen Neural| Neuraal|
|Noors| `nb`    |Neural beschikbaar|  Neuraal|
|Perzisch|   `fa`    |Neural beschikbaar|  Neuraal|
|Pools|    `pl`    |Neural beschikbaar|  Neuraal|
|Portugees|    `pt`    |Neural beschikbaar|  Neuraal|
|Queretaro Otomi|   `otq`   |Alleen statistisch|  Statische|
|Roemeens|  `ro`    |Neural beschikbaar|  Neuraal|
|Russisch|   `ru`    |Neural beschikbaar|  Neuraal|
|Samoan|    `sm`    |Alleen statistisch|  Statische|
|Servisch (Cyrillisch)|    `sr-Cyrl`   |Alleen statistisch|  Statische|
|Servisch (Latijns)|   `sr-Latn`   |Alleen statistisch   |Statische|
|Slowaaks|    `sk`    |Neural beschikbaar|  Neuraal|
|Sloveens| `sl`    |Neural beschikbaar|  Neuraal|
|Spaans|   `es`    |Neural beschikbaar|  Neuraal|
|Zweeds|   `sv`    |Neural beschikbaar   |Neuraal|
|Tahitian|  `ty`    |Alleen statistisch|  Statische|
|Tamil| `ta`    |Neural beschikbaar | Neuraal|
|Telugu|    `te`    |Alleen Neural|   Neuraal|
|Thais|  `th`    |Neural beschikbaar|  Neuraal|
|Tongaanse|    `to`    |Alleen statistisch|  Statische|
|Turks|   `tr`    |Neural beschikbaar   |Neuraal|
|Oekraïens| `uk`    |Neural beschikbaar|  Neuraal|
|Urdu|  `ur`    |Alleen statistisch|  Statische|
|Vietnamees|    `vi`    |Neural beschikbaar|  Neuraal|
|Welsh| `cy`    |Neural beschikbaar|  Neuraal|
|Yucatec Maya|  `yua`   |Alleen statistisch|  Statische|

## <a name="transliteration"></a>Transliteratie

De methode transtranscrib ondersteunt de volgende talen. In de ' aan/van ', ' <-> ' geeft aan dat de taal kan worden getranscribeerd van of naar een van de vermelde scripts. De '--> ' geeft aan dat de taal alleen kan worden getranscribeerd van het ene script naar het andere.

| Taal    | Taalcode | Script | Naar/van | Script|
|:----------- |:-------------:|:-------------:|:-------------:|:-------------:|
| Arabisch | `ar` | Arabische `Arab` | <--> | Latijns `Latn` |
|Bengalese  | `bn` | Bengaals `Beng` | <--> | Latijns `Latn` |
| Chinees (vereenvoudigd) | `zh-Hans` | Vereenvoudigd Chinees `Hans`| <--> | Latijns `Latn` |
| Chinees (vereenvoudigd) | `zh-Hans` | Vereenvoudigd Chinees `Hans`| <--> | Traditioneel Chinees `Hant`|
| Chinees (traditioneel) | `zh-Hant` | Traditioneel Chinees `Hant`| <--> | Latijns `Latn` |
| Chinees (traditioneel) | `zh-Hant` | Traditioneel Chinees `Hant`| <--> | Vereenvoudigd Chinees `Hans` |
| Gujarati | `gu`  | Gujarati `Gujr` | --> | Latijns `Latn` |
| Hebreeuws | `he` | Hebreeuws `Hebr` | <--> | Latijns `Latn` |
| Hindi | `hi` | Devanagari `Deva` | <--> | Latijns `Latn` |
| Japans | `ja` | Japanse `Jpan` | <--> | Latijns `Latn` |
| Kannada | `kn` | Kannada `Knda` | --> | Latijns `Latn` |
| Malajalam | `ml` | Malayalam `Mlym` | --> | Latijns `Latn` |
| Marathi | `mr` | Devanagari `Deva` | --> | Latijns `Latn` |
| Odia | `or` | Oriya `Orya` | <--> | Latijns `Latn` |
| Punjabi | `pa` | Gurmukhi `Guru`  | <--> | Latijns `Latn`  |
| Servisch (Cyrillisch) | `sr-Cyrl` | Cyrillische `Cyrl`  | --> | Latijns `Latn` |
| Servisch (Latijns) | `sr-Latn` | Latijns `Latn` | --> | Cyrillische `Cyrl`|
| Tamil | `ta` | Tamil `Taml` | --> | Latijns `Latn` |
| Telugu | `te` | Telugu `Telu` | --> | Latijns `Latn` |
| Thais | `th` | Thais `Thai` | <--> | Latijns `Latn` |

## <a name="dictionary"></a>Woordenlijst

De woorden lijst ondersteunt de volgende talen in of vanuit het Engels met behulp van de methoden voor zoeken en voor beelden.

| Taal    | Taalcode |
|:----------- |:-------------:|
| Afrikaans      | `af`          |
| Arabisch       | `ar`          |
| Bengalese      | `bn`          |
| Bosnisch (Latijns)      | `bs`          |
| Bulgaars      | `bg`          |
| Catalaans      | `ca`          |
| Vereenvoudigd Chinees      | `zh-Hans`          |
| Kroatisch      | `hr`          |
| Tsjechisch      | `cs`          |
| Deens      | `da`          |
| Nederlands      | `nl`          |
| Estisch      | `et`          |
| Fins      | `fi`          |
| Frans      | `fr`          |
| Duits      | `de`          |
| Grieks      | `el`          |
| Haïtiaans      | `ht`          |
| Hebreeuws      | `he`          |
| Hindi      | `hi`          |
| Hmong Daw      | `mww`          |
| Hongaars      | `hu`          |
| IJslands    | `is`  |
| Indonesisch      | `id`          |
| Italiaans      | `it`          |
| Japans      | `ja`          |
| Swahili      | `sw`          |
| Klingon      | `tlh`          |
| Koreaans      | `ko`          |
| Lets      | `lv`          |
| Litouws      | `lt`          |
| Maleis      | `ms`          |
| Maltees      | `mt`          |
| Noors      | `nb`          |
| Perzisch      | `fa`          |
| Pools      | `pl`          |
| Portugees      | `pt`          |
| Roemeens      | `ro`          |
| Russisch      | `ru`          |
| Servisch (Latijns)      | `sr-Latn`          |
| Slowaaks     | `sk`          |
| Sloveens      | `sl`          |
| Spaans      | `es`          |
| Zweeds      | `sv`          |
| Tamil      | `ta`          |
| Thais      | `th`          |
| Turks      | `tr`          |
| Oekraïens      | `uk`          |
| Urdu      | `ur`          |
| Vietnamees      | `vi`          |
| Welsh      | `cy`          |

## <a name="detect"></a>Detecteren

Translator Text-API detecteert alle talen die beschikbaar zijn voor vertaal-en vele.


## <a name="access-the-translator-text-api-language-list-programmatically"></a>Programmatisch toegang tot de lijst met Translator Text-API talen

U kunt een lijst met ondersteunde talen voor de Translator Text-API v 3.0 ophalen met behulp van de talen methode. U kunt de lijst weer geven op functie, taal code en de taal naam in het Engels of een andere ondersteunde taal. Deze lijst wordt automatisch bijgewerkt door de micro soft Translator-service wanneer er nieuwe talen beschikbaar worden gesteld.

[Documentatie voor het bewerkings overzicht van talen weer geven](reference/v3-0-languages.md)

## <a name="customization"></a>Aanpassing

De volgende talen zijn beschikbaar voor aanpassing in of vanuit het Engels met [aangepaste vertaler](https://aka.ms/CustomTranslator).

| Taal    | Taalcode |
|:----------- |:-------------:|
| Arabisch       | `ar`          |
| Bengalese      | `bn`          |
| Bosnisch (Latijns)      | `bs`          |
| Bulgaars      | `bg`          |
| Vereenvoudigd Chinees      | `zh-Hans`          |
|Traditioneel Chinees|   `zh-Hant`   |
| Kroatisch      | `hr`          |
| Tsjechisch      | `cs`          |
| Deens      | `da`          |
| Nederlands      | `nl`          |
| Nederlands    | `en`     |
| Estisch      | `et`          |
| Fins      | `fi`          |
| Frans      | `fr`          |
| Duits      | `de`          |
| Grieks      | `el`          |
| Hebreeuws      | `he`          |
| Hindi      | `hi`          |
| Hongaars      | `hu`          |
| IJslands | `is` |
| Indonesisch|   `id`    |
| Iers | `ga`  |
| Italiaans      | `it`          |
| Japans      | `ja`          |
|Swahili| `sw`    |
| Koreaans      | `ko`          |
| Lets      | `lv`          |
| Litouws      | `lt`          |
|Malagassische|  `mg`    |
|Maori| `mi`  |
| Noors      | `nb`          |
| Perzisch      | `fa`          |
| Pools      | `pl`          |
| Portugees      | `pt`          |
| Roemeens      | `ro`          |
| Russisch      | `ru`          |
|Samoan|    `sm`    |
| Servisch (Latijns)      | `sr-Latn`          |
| Slowaaks     | `sk`          |
| Sloveens      | `sl`          |
| Spaans      | `es`          |
| Zweeds      | `sv`          |
| Thais      | `th`          |
| Turks      | `tr`          |
| Oekraïens      | `uk`          |
| Vietnamees      | `vi`          |
| Welsh | `cy` |

## <a name="access-the-list-on-the-microsoft-translator-website"></a>Toegang tot de lijst op de website van micro soft Translator

Voor een beknopt overzicht van de talen bevat de micro soft Translator-website alle talen die worden ondersteund door de Translator Text-en spraak-Api's. Deze lijst bevat geen informatie die specifiek is voor de ontwikkelaar, zoals taal codes.

[Bekijk de lijst met talen](https://www.microsoft.com/translator/languages.aspx)
