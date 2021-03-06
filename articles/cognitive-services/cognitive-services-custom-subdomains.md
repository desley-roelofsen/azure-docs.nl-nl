---
title: Aangepaste subdomeinen
titleSuffix: Azure Cognitive Services
description: Aangepaste subdomein namen voor elke cognitieve service resource worden gemaakt via de Azure Portal, Azure Cloud Shell of Azure CLI.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.topic: conceptual
ms.date: 07/24/2019
ms.author: erhopf
ms.openlocfilehash: 108e5ebfa34db711af4ff33cbf4fbba897b8947d
ms.sourcegitcommit: 359930a9387dd3d15d39abd97ad2b8cb69b8c18b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/06/2019
ms.locfileid: "73647682"
---
# <a name="custom-subdomain-names-for-cognitive-services"></a>Aangepaste subdomein namen voor Cognitive Services

In azure Cognitive Services worden aangepaste subdomein namen gebruikt voor elke resource die via de [Azure Portal](https://portal.azure.com), [Azure Cloud shell](https://azure.microsoft.com/features/cloud-shell/)of de [Azure cli](https://docs.microsoft.com/cli/azure/install-azure-cli)is gemaakt. In tegens telling tot regionale eind punten, die gemeen schappelijk zijn voor alle klanten in een specifieke Azure-regio, zijn aangepaste subdomein namen uniek voor de resource. Aangepaste subdomein namen zijn vereist om functies als Azure Active Directory (Azure AD) voor verificatie in te scha kelen.

## <a name="how-does-this-impact-existing-resources"></a>Hoe beïnvloedt dit de bestaande resources?

Cognitive Services resources die zijn gemaakt voor 1 juli 2019, zullen de regionale eind punten voor de bijbehorende service gebruiken. Deze eind punten werken met bestaande en nieuwe resources.

Als u een bestaande resource wilt migreren om aangepaste subdomeinen te gebruiken, zodat u functies als Azure AD kunt inschakelen, volgt u deze instructies:

1. Meld u aan bij de Azure Portal en zoek de Cognitive Services resource waaraan u een aangepast subdomein naam wilt toevoegen.
2. Zoek op de Blade **overzicht** de optie **aangepaste domein naam genereren**en selecteer deze.
3. Hiermee opent u een deel venster met instructies voor het maken van een uniek aangepast subdomein voor uw resource.
   > [!WARNING]
   > Nadat u een aangepaste subdomeinnaam hebt gemaakt, **kan deze niet meer** worden gewijzigd.

## <a name="do-i-need-to-update-my-existing-resources"></a>Moet ik mijn bestaande resources bijwerken?

Nee. Het regionale eind punt blijft werken voor nieuwe en bestaande Cognitive Services en de aangepaste subdomeinnaam is optioneel. Zelfs als de naam van een aangepast subdomein wordt toegevoegd, blijft het regionale eind punt werken met de resource.

## <a name="what-if-an-sdk-asks-me-for-the-region-for-a-resource"></a>Wat gebeurt er als een SDK mij vraagt voor de regio van een resource?

> [!WARNING]
> De spraak Services ondersteunen op dit moment **geen** aangepaste subdomeinen. Gebruik de regionale eind punten wanneer u de spraak Services en de bijbehorende Sdk's gebruikt.

Regionale eind punten en aangepaste subdomeinen worden beide ondersteund en kunnen door elkaar worden gebruikt. Het volledige eind punt is echter vereist.

Informatie over de regio is beschikbaar op de Blade **overzicht** voor uw resource in de [Azure Portal](https://portal.azure.com). Zie [is er een lijst met regionale eind](#is-there-a-list-of-regional-endpoints) punten voor de volledige lijst met regionale eind punten?

## <a name="are-custom-subdomain-names-regional"></a>Zijn aangepaste subdomein namen regionaal?

Ja. Het gebruik van een aangepaste subdomeinnaam heeft geen invloed op de regionale aspecten van uw Cognitive Services bron.

## <a name="what-are-the-requirements-for-a-custom-subdomain-name"></a>Wat zijn de vereisten voor een aangepaste subdomeinnaam?

De naam van een aangepast subdomein is uniek voor uw resource. De naam mag alleen alfanumerieke tekens en het `-` teken bevatten. de waarde moet tussen 2 en 64 tekens lang zijn en mag niet eindigen met een `-`.

## <a name="can-i-change-a-custom-domain-name"></a>Kan ik een aangepaste domein naam wijzigen?

Nee. Nadat een aangepaste subdomeinnaam is gemaakt en aan een resource is gekoppeld, kan deze niet meer worden gewijzigd.

## <a name="can-i-reuse-a-custom-domain-name"></a>Kan ik een aangepaste domein naam opnieuw gebruiken?

Elke aangepaste subdomeinnaam is uniek, dus als u een aangepaste subdomeinnaam die u hebt toegewezen aan een Cognitive Services resource opnieuw wilt gebruiken, moet u de bestaande resource verwijderen. Nadat de resource is verwijderd, kunt u de aangepaste subdomeinnaam opnieuw gebruiken.

## <a name="is-there-a-list-of-regional-endpoints"></a>Is er een lijst met regionale eind punten?

Ja. Dit is een lijst met regionale eind punten die u kunt gebruiken met Azure Cognitive Services-resources.

> [!NOTE]
> De Translator Text-API en Bing Zoeken-API's globale eind punten gebruiken.

| Type eind punt | Regio | Eindpunt |
|---------------|--------|----------|
| Openbaar | Global (Translator Text & Bing) | `https://api.cognitive.microsoft.com` |
| | Australië - oost | `https://australiaeast.api.cognitive.microsoft.com` |
| | Brazilië - zuid | `https://brazilsouth.api.cognitive.microsoft.com` |
| | Canada - midden | `https://canadacentral.api.cognitive.microsoft.com` |
| | US - centraal | `https://centralus.api.cognitive.microsoft.com` |
| | Azië - oost | `https://eastasia.api.cognitive.microsoft.com` |
| | US - oost | `https://eastus.api.cognitive.microsoft.com` |
| | US - oost 2 | `https://eastus2.api.cognitive.microsoft.com` |
| | Frankrijk - centraal | `https://francecentral.api.cognitive.microsoft.com` |
| | India - centraal | `https://centralindia.api.cognitive.microsoft.com` |
| | Japan - oost | `https://japaneast.api.cognitive.microsoft.com` |
| | Korea - centraal | `https://koreacentral.api.cognitive.microsoft.com` |
| | US - noord-centraal | `https://northcentralus.api.cognitive.microsoft.com` |
| | Europa - noord | `https://northeurope.api.cognitive.microsoft.com` |
| | Zuid-Afrika - noord | `https://southafricanorth.api.cognitive.microsoft.com` |
| | US - zuid-centraal | `https://southcentralus.api.cognitive.microsoft.com` |
| | Azië - zuidoost | `https://southeastasia.api.cognitive.microsoft.com` |
| | VK - zuid | `https://uksouth.api.cognitive.microsoft.com` |
| | US - west-centraal | `https://westcentralus.api.cognitive.microsoft.com` |
| | Europa -west | `https://westeurope.api.cognitive.microsoft.com` |
| | US - west | `https://westus.api.cognitive.microsoft.com` |
| | US - west 2 | `https://westus2.api.cognitive.microsoft.com` |
| Amerikaanse overheid | VS (overheid) - Virginia | `https://virginia.api.cognitive.microsoft.us` |
| China | China - oost 2 | `https://chinaeast2.api.cognitive.azure.cn` |
| | China - noord | `https://chinanorth.api.cognitive.azure.cn` |

## <a name="see-also"></a>Zie ook

* [Wat zijn de Cognitive Services?](Welcome.md)
* [Verificatie](authentication.md)
