---
title: Een aangepaste weer gave zoeken-Bing Aangepaste zoekopdrachten
titleSuffix: Azure Cognitive Services
description: Nadat u uw aangepaste zoek ervaring hebt geconfigureerd, kunt u deze testen vanuit de Bing Aangepaste zoekopdrachten Portal.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-custom-search
ms.topic: conceptual
ms.date: 05/09/2019
ms.author: maheshb
ms.openlocfilehash: f04bfc306a89c7bb6caf3f928f5fdfec66358649
ms.sourcegitcommit: a107430549622028fcd7730db84f61b0064bf52f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/14/2019
ms.locfileid: "74072747"
---
# <a name="call-your-bing-custom-search-instance-from-the-portal"></a>Uw Bing Aangepaste zoekopdrachten-exemplaar aanroepen vanuit de portal

Nadat u uw aangepaste zoek ervaring hebt geconfigureerd, kunt u deze testen vanuit de Bing Aangepaste zoekopdrachten [Portal](https://customsearch.ai). 

![een scherm afbeelding van de Bing Custom Search-Portal](media/portal-search-screen.png)
## <a name="create-a-search-query"></a>Een zoek query maken 

Nadat u zich hebt aangemeld bij de Bing Aangepaste zoekopdrachten [Portal](https://customsearch.ai), selecteert u uw Zoek instantie en klikt u op het tabblad **productie** . Onder **eind punten**selecteert u een API-eind punt (bijvoorbeeld Web-API). Uw abonnement bepaalt welke eind punten worden weer gegeven.

Als u een zoek query wilt maken, voert u de parameter waarden voor uw eind punt in. Houd er rekening mee dat de para meters die in de portal worden weer gegeven, kunnen veranderen, afhankelijk van het eind punt dat u kiest. Zie de [aangepaste zoekopdrachten-API referentie](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-custom-search-api-v7-reference#query-parameters) voor meer informatie. Als u het abonnement wilt wijzigen dat door uw Zoek instantie wordt gebruikt, voegt u de juiste abonnements sleutel toe en werkt u de juiste markt-en/of taal parameters bij.

Hieronder vindt u enkele belang rijke para meters:


|Parameter  |Beschrijving  |
|---------|---------|
|Query's uitvoeren     | De zoek term waarnaar moet worden gezocht. Alleen beschikbaar voor eind punten voor web-, afbeeldings-, video-en automatische suggesties |
|Aangepaste configuratie-ID | De configuratie-ID van het geselecteerde exemplaar van de aangepaste zoek actie. Dit veld is alleen-lezen. |
|Market     | De markt waarvan de resultaten afkomstig zijn. Alleen beschikbaar voor web-, afbeeldings-, video-en gehoste UI-eind punten.        |
|Abonnements sleutel | De abonnements sleutel die moet worden getest. U kunt een sleutel selecteren in de vervolg keuzelijst of hand matig invoeren.          |

Als u op **extra para meters** klikt, worden de volgende para meters zichtbaar  

|Parameter  |Beschrijving  |
|---------|---------|
|Safe Search     | Een filter dat wordt gebruikt om webpagina's te filteren op inhoud voor volwassenen. Alleen beschikbaar voor web-, afbeeldings-, video-en gehoste UI-eind punten.        |
|Taal van de gebruikers interface    | De taal die wordt gebruikt voor teken reeksen van de gebruikers interface. Als u bijvoorbeeld installatie kopieën en Video's in de gehoste gebruikers interface inschakelt, wordt in de **afbeeldings** -en **video** tabbladen de opgegeven taal gebruikt.        |
|Count     | Het aantal Zoek resultaten dat in het antwoord moet worden geretourneerd. Alleen beschikbaar voor web-, afbeeldings-en video-eind punten.         |
|Offset    | Het aantal Zoek resultaten dat moet worden overgeslagen voordat resultaten worden geretourneerd. Alleen beschikbaar voor web-, afbeeldings-en video-eind punten.        |
    
Nadat u alle vereiste opties hebt opgegeven, klikt u op **aanroep** om het JSON-antwoord in het rechterdeel venster weer te geven. Als u het gehoste gebruikers interface-eind punt selecteert, kunt u de zoek ervaring testen in het onderste deel venster.

## <a name="change-your-bing-custom-search-subscription"></a>Uw Bing Aangepaste zoekopdrachten-abonnement wijzigen

U kunt het abonnement dat is gekoppeld aan uw Bing Aangepaste zoekopdrachten-exemplaar wijzigen zonder een nieuw exemplaar te maken. Als u wilt dat API-aanroepen worden verzonden en gefactureerd naar een nieuw abonnement, maakt u een nieuwe Bing Aangepaste zoekopdrachten resource in de Azure Portal. Gebruik de nieuwe abonnements sleutel in uw API-aanvragen, samen met de aangepaste configuratie-ID van uw exemplaar.

## <a name="next-steps"></a>Volgende stappen

- [Uw aangepaste weer gave aanroepen metC#](./call-endpoint-csharp.md)
- [Uw aangepaste weer gave met Java aanroepen](./call-endpoint-java.md)
- [Uw aangepaste weer gave aanroepen met NodeJs](./call-endpoint-nodejs.md)
- [Uw aangepaste weer gave met python aanroepen](./call-endpoint-python.md)

- [Uw aangepaste weer gave aanroepen C# met de SDK](./sdk-csharp-quick-start.md)
