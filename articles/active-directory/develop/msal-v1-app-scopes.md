---
title: MSAL-bereiken voor een v 1.0-toepassing | Azure
description: Meer informatie over de bereiken voor een v 1.0-toepassing met behulp van de micro soft Authentication Library (MSAL).
services: active-directory
author: TylerMSFT
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 11/25/2019
ms.author: twhitney
ms.reviewer: saeeda
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 0606376c455733ca90f1bff3d0295cbd1a075637
ms.sourcegitcommit: 5ab4f7a81d04a58f235071240718dfae3f1b370b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/10/2019
ms.locfileid: "74963253"
---
# <a name="scopes-for-a-web-api-accepting-v10-tokens"></a>Bereiken voor een web-API die v 1.0-tokens accepteert

OAuth2 machtigingen zijn machtigings bereiken die een Azure Active Directory-toepassing (Azure AD) voor ontwikkel aars (v 1.0) Web-API (resource) beschikbaar maakt voor client toepassingen. Deze machtigings bereiken kunnen tijdens de toestemming aan client toepassingen worden verleend. Zie de sectie over `oauth2Permissions` in de [Naslag informatie over het Azure Active Directory-toepassings manifest](reference-app-manifest.md#manifest-reference).

## <a name="scopes-to-request-access-to-specific-oauth2-permissions-of-a-v10-application"></a>Bereiken voor het aanvragen van toegang tot specifieke OAuth2-machtigingen van een v 1.0-toepassing

Voor het verkrijgen van tokens voor specifieke bereiken van een v 1.0-toepassing (bijvoorbeeld de Azure AD-grafiek, die is https://graph.windows.net), maakt u bereiken door een gewenste resource-id te koppelen met een gewenste OAuth2-machtiging voor die bron.

Bijvoorbeeld voor toegang namens de gebruiker een v 1.0 Web-API waarbij de URI van de App-ID is `ResourceId`:

```csharp
var scopes = new [] {  ResourceId+"/user_impersonation"};
```

```javascript
var scopes = [ ResourceId + "/user_impersonation"];
```

Als u wilt lezen en schrijven met MSAL.NET Azure AD Graph API (https:\//graph.windows.net/), moet u een lijst met bereiken maken, zoals wordt weer gegeven in de volgende voor beelden:

```csharp
string ResourceId = "https://graph.windows.net/";
var scopes = new [] { ResourceId + "Directory.Read", ResourceID + "Directory.Write"}
```

```javascript
var ResourceId = "https://graph.windows.net/";
var scopes = [ ResourceId + "Directory.Read", ResourceID + "Directory.Write"];
```

Als u het bereik wilt schrijven dat overeenkomt met de Azure Resource Manager-API (https:\//management.core.windows.net/), moet u het volgende bereik aanvragen (Let op de twee slashes):

```csharp
var scopes = new[] {"https://management.core.windows.net//user_impersonation"};
var result = await app.AcquireTokenInteractive(scopes).ExecuteAsync();

// then call the API: https://management.azure.com/subscriptions?api-version=2016-09-01
```

> [!NOTE]
> U moet twee slashes gebruiken omdat de Azure Resource Manager-API een slash in de claim van de doel groep (AUD) verwacht en vervolgens een slash is om de API-naam van het bereik te scheiden.

De logica die door Azure AD wordt gebruikt, is als volgt:

- Voor ADAL (Azure AD v 1.0)-eind punt met een v 1.0-toegangs token (alleen mogelijk), AUD = resource
- Voor MSAL (micro soft Identity platform (v 2.0))-eind punt dat een toegangs token vraagt voor een resource die v 2.0-tokens accepteert, `aud=resource.AppId`
- Voor MSAL (v 2.0-eind punt) waarbij een toegangs token wordt gevraagd voor een resource die een v 1.0-toegangs token accepteert (dit is het geval hierboven), parseert Azure AD de gewenste doel groep uit het aangevraagde bereik door alles vóór de laatste slash te nemen en deze als de resource-id te gebruiken. Als https:\/-database.windows.net de doel groep ' https:\//database.windows.net/' verwacht, moet u daarom een scope van ' https:\//database.windows.net//.default ' aanvragen. Zie ook GitHub issue [#747: de afsluitende slash van de resource-URL wordt wegge laten, wat een SQL-verificatie fout heeft veroorzaakt](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/issues/747).

## <a name="scopes-to-request-access-to-all-the-permissions-of-a-v10-application"></a>Bereiken om toegang aan te vragen tot alle machtigingen van een v 1.0-toepassing

Als u een token wilt verkrijgen voor alle statische bereiken van een v 1.0-toepassing, voegt u '. default ' toe aan de URI van de App-ID van de API:

```csharp
ResourceId = "someAppIDURI";
var scopes = new [] {  ResourceId+"/.default"};
```

```javascript
var ResourceId = "someAppIDURI";
var scopes = [ ResourceId + "/.default"];
```

## <a name="scopes-to-request-for-a-client-credential-flowdaemon-app"></a>Bereiken voor het aanvragen van een client referentie stroom/daemon-app

In het geval van een client referentie stroom wordt het bereik dat moet worden door gegeven ook `/.default`. Dit geeft aan Azure AD: ' alle machtigingen op app-niveau die de beheerder heeft ingestemd in de registratie van de toepassing.
