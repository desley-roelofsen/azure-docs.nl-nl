---
title: Azure AD-verificatie & nationale Clouds | Azure
titleSuffix: Microsoft identity platform
description: Meer informatie over app-registratie en verificatie-eind punten voor nationale Clouds.
services: active-directory
author: negoe
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 08/28/2019
ms.author: negoe
ms.reviewer: negoe,celested
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 14b97677f5aa9624ba70696114ac34fcd9f46182
ms.sourcegitcommit: a5ebf5026d9967c4c4f92432698cb1f8651c03bb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/08/2019
ms.locfileid: "74918029"
---
# <a name="national-clouds"></a>Nationale Clouds

Nationale Clouds zijn fysiek geïsoleerde exemplaren van Azure. Deze regio's van Azure zijn zodanig ontworpen dat de vereisten voor gegevens locatie, soevereiniteit en naleving binnen geografische grenzen worden gerespecteerd.

Met inbegrip van de wereld wijde Cloud, Azure Active Directory (Azure AD), wordt geïmplementeerd in de volgende nationale Clouds:  

- Azure Government
- Azure Duitsland
- Azure China 21Vianet

Nationale Clouds zijn uniek en een afzonderlijke omgeving van Azure Global. Het is belang rijk om rekening te houden met belang rijke verschillen tijdens het ontwikkelen van uw toepassing voor deze omgevingen. Verschillen zijn onder andere het registreren van toepassingen, het verkrijgen van tokens en het configureren van eind punten.

## <a name="app-registration-endpoints"></a>App-registratie-eind punten

Er is een afzonderlijke Azure Portal voor elk van de nationale Clouds. Als u toepassingen wilt integreren met het micro soft Identity-platform in een nationale Cloud, moet u uw toepassing afzonderlijk registreren in elk Azure Portal dat specifiek is voor de omgeving.

De volgende tabel bevat de basis-Url's voor de Azure AD-eind punten die worden gebruikt om een toepassing voor elke nationale Cloud te registreren.

| Nationale Cloud | Azure AD-Portal-eind punt |
|----------------|--------------------------|
| Azure AD voor de Amerikaanse overheid | `https://portal.azure.us` |
| Azure AD Duitsland | `https://portal.microsoftazure.de` |
| Azure AD China beheerd door 21Vianet | `https://portal.azure.cn` |
| Azure AD (globale service) |`https://portal.azure.com` |

## <a name="azure-ad-authentication-endpoints"></a>Azure AD-verificatie-eind punten

Alle nationale Clouds verifiëren gebruikers afzonderlijk in elke omgeving en hebben afzonderlijke verificatie-eind punten.

De volgende tabel bevat de basis-Url's voor de Azure AD-eind punten die worden gebruikt om tokens voor elke nationale Cloud te verkrijgen.

| Nationale Cloud | Azure AD-verificatie-eind punt |
|----------------|-------------------------|
| Azure AD voor de Amerikaanse overheid | `https://login.microsoftonline.us` |
| Azure AD Duitsland| `https://login.microsoftonline.de` |
| Azure AD China beheerd door 21Vianet | `https://login.chinacloudapi.cn` |
| Azure AD (globale service)| `https://login.microsoftonline.com` |

U kunt aanvragen indienen bij de autorisatie-of Token-eind punten van Azure AD met behulp van de juiste regiospecifieke basis-URL. Bijvoorbeeld voor Azure Duitsland:

  - Het gemeen schappelijke eind punt voor autorisatie is `https://login.microsoftonline.de/common/oauth2/authorize`.
  - Het token common endpoint is `https://login.microsoftonline.de/common/oauth2/token`.

Vervang voor toepassingen met één Tenant "common" in de vorige Url's door uw Tenant-ID of-naam. Een voorbeeld is `https://login.microsoftonline.de/contoso.com`.

## <a name="microsoft-graph-api"></a>Microsoft Graph-API

Ga voor meer informatie over het aanroepen van de Microsoft Graph-Api's in een nationale cloud omgeving naar [Microsoft Graph in nationale Cloud implementaties](https://developer.microsoft.com/graph/docs/concepts/deployments).

> [!IMPORTANT]
> Bepaalde services en functies in bepaalde regio's van de wereld wijde service zijn mogelijk niet beschikbaar in alle nationale Clouds. Ga naar [beschik bare producten per regio](https://azure.microsoft.com/global-infrastructure/services/?products=all&regions=usgov-non-regional,us-dod-central,us-dod-east,usgov-arizona,usgov-iowa,usgov-texas,usgov-virginia,china-non-regional,china-east,china-east-2,china-north,china-north-2,germany-non-regional,germany-central,germany-northeast)voor meer informatie over welke services beschikbaar zijn.

Als u wilt weten hoe u een toepassing bouwt met behulp van het micro soft-identiteits platform, volgt u de [zelf studie micro soft Authentication Library (MSAL)](msal-national-cloud.md). Met name deze app meldt zich aan bij een gebruiker en krijgt een toegangs token om de Microsoft Graph-API aan te roepen.

## <a name="next-steps"></a>Volgende stappen

Meer informatie over:

- [Azure Government](https://docs.microsoft.com/azure/azure-government/)
- [Azure China 21Vianet](https://docs.microsoft.com/azure/china/)
- [Azure Duitsland](https://docs.microsoft.com/azure/germany/)
- [Basis beginselen van Azure AD-verificatie](authentication-scenarios.md)
