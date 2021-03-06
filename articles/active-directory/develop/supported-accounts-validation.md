---
title: Validatie verschillen per account type-micro soft Identity-platform | Azure
description: Meer informatie over de validatie verschillen van verschillende eigenschappen voor verschillende ondersteunde account typen bij het registreren van uw app met het micro soft Identity platform.
author: SureshJa
ms.author: sureshja
manager: CelesteDG
ms.date: 10/12/2019
ms.topic: conceptual
ms.subservice: develop
ms.custom: aaddev
ms.service: active-directory
ms.reviewer: lenalepa, manrath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 576adc99ef7d794f50efeb61375f3e59f8815033
ms.sourcegitcommit: a5ebf5026d9967c4c4f92432698cb1f8651c03bb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/08/2019
ms.locfileid: "74919355"
---
# <a name="validation-differences-by-supported-account-types-signinaudience"></a>Validatie verschillen per ondersteund account type (signInAudience)

Wanneer u een toepassing registreert met het micro soft-identiteits platform voor ontwikkel aars, wordt u gevraagd welke account typen door uw toepassing worden ondersteund. In het object en manifest van de toepassing is deze eigenschap `signInAudience`.

De volgende opties zijn beschikbaar:

- *AzureADMyOrg*: alleen accounts in de organisatie directory waar de app is geregistreerd (single-tenant)
- *AzureADMultipleOrgs*: accounts in elke organisatorische Directory (multi tenant)
- *AzureADandPersonalMicrosoftAccount*: accounts in een organisatorische Directory (multi tenant) en persoonlijke micro soft-accounts (bijvoorbeeld Skype, Xbox en Outlook.com)

Voor geregistreerde toepassingen kunt u de waarde voor ondersteunde account typen vinden in het gedeelte **verificatie** van een toepassing. U kunt deze ook vinden onder de eigenschap `signInAudience` in het **manifest**.

De waarde die u voor deze eigenschap selecteert, heeft gevolgen voor andere eigenschappen van app-objecten. Als u deze eigenschap wijzigt, moet u daarom wellicht eerst andere eigenschappen wijzigen.

Zie de volgende tabel voor de verschillen in de validatie van verschillende eigenschappen voor verschillende typen ondersteunde accounts.

| Eigenschap | `AzureADMyOrg` | `AzureADMultipleOrgs`  | `AzureADandPersonalMicrosoftAccount` |
|--------------|---------------|----------------|----------------|
| URI van de toepassings-ID (`identifierURIs`)  | Moet uniek zijn in de Tenant <br><br> urn://-schema's worden ondersteund <br><br> Joker tekens worden niet ondersteund <br><br> Query reeksen en-fragmenten worden ondersteund <br><br> Maximale lengte van 255 tekens <br><br> Geen limiet * voor aantal identifierURIs  | Moet globaal uniek zijn <br><br> urn://-schema's worden ondersteund <br><br> Joker tekens worden niet ondersteund <br><br> Query reeksen en-fragmenten worden ondersteund <br><br> Maximale lengte van 255 tekens <br><br> Geen limiet * voor aantal identifierURIs | Moet globaal uniek zijn <br><br> urn://-schema's worden niet ondersteund <br><br> Joker tekens, fragmenten en query reeksen worden niet ondersteund <br><br> Maximale lengte van 120 tekens <br><br> Maxi maal 50 identifierURIs |
| Certificaten (`keyCredentials`) | Symmetrische handtekening sleutel | Symmetrische handtekening sleutel | Versleuteling en asymmetrische handtekening sleutel | 
| Client geheimen (`passwordCredentials`) | Geen limiet * | Geen limiet * | Als liveSDK is ingeschakeld: Maxi maal 2 client geheimen | 
| Omleidings-Uri's (`replyURLs`) | Zie de beperkingen van de [omleidings-URI/antwoord-URL en beperkingen](reply-url.md) voor meer informatie. | | | 
| API-machtigingen (`requiredResourceAccess`) | Geen limiet * | Geen limiet * | Maxi maal 30 machtigingen per resource toegestaan (bijvoorbeeld Microsoft Graph) | 
| Bereiken die door deze API worden gedefinieerd (`oauth2Permissions`) | Maximale lengte van de scope naam van 120 tekens <br><br> Geen limiet * voor het aantal gedefinieerde bereiken | Maximale lengte van de scope naam van 120 tekens <br><br> Geen limiet * voor het aantal gedefinieerde bereiken |  Maximale lengte van de scope naam van 40 tekens <br><br> Maxi maal 100 scopes gedefinieerd | 
| Geautoriseerde client toepassingen (`preautorizedApplications`) | Geen limiet * | Geen limiet * | Totaal aantal van 500 <br><br> Maximum aantal gedefinieerde 100-client-apps <br><br> Maxi maal 30 scopes gedefinieerd per client | 
| appRoles | Ondersteund <br> Geen limiet * | Ondersteund <br> Geen limiet * | Niet ondersteund | 
| Afmeldings-URL | http://localhost is toegestaan <br><br> Maximale lengte van 255 tekens | http://localhost is toegestaan <br><br> Maximale lengte van 255 tekens | <br><br> https://localhost is toegestaan http://localhost mislukt voor MSA <br><br> Maximale lengte van 255 tekens <br><br> HTTP-schema is niet toegestaan <br><br> Joker tekens worden niet ondersteund | 

\* Er is een algemene limiet van ongeveer 1000 items in alle verzamelings eigenschappen van het app-object

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over [toepassings registratie](app-objects-and-service-principals.md)
- Meer informatie over het [toepassings manifest](reference-app-manifest.md)
