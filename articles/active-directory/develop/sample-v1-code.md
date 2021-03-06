---
title: Code voorbeelden voor Azure Active Directory v 1.0 | Microsoft Docs
description: Biedt een index van code voorbeelden van Azure Active Directory (v 1.0-eind punt), geordend op scenario.
services: active-directory
documentationcenter: dev-center-name
author: jmprieur
manager: CelesteDG
editor: ''
ms.assetid: a242a5ff-7300-40c2-ba83-fb6035707433
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/15/2019
ms.author: ryanwi
ms.reviewer: jmprieur
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: a71eaebcc6345d6550d6ddd38350fe709706d740
ms.sourcegitcommit: ec2b75b1fc667c4e893686dbd8e119e7c757333a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/23/2019
ms.locfileid: "72809297"
---
# <a name="azure-active-directory-code-samples-v10-endpoint"></a>Azure Active Directory code voorbeelden (v 1.0-eind punt)

[!INCLUDE [active-directory-develop-applies-v1-adal](../../../includes/active-directory-develop-applies-v1-adal.md)]

U kunt Microsoft Azure Active Directory (Azure AD) gebruiken om verificatie en autorisatie toe te voegen aan uw webtoepassingen en Web-Api's.

In deze sectie vindt u koppelingen naar voor beelden die u kunt gebruiken voor meer informatie over het Azure AD v 1.0-eind punt. In deze voor beelden ziet u hoe deze worden uitgevoerd, samen met code fragmenten die u in uw toepassingen kunt gebruiken. Op de pagina code voorbeeld vindt u gedetailleerde onderwerpen over lees mijzelf die u helpen bij vereisten, installatie en configuratie. En de code is een opmerking waarmee u de essentiële secties kunt begrijpen.

> [!NOTE]
> Als u geïnteresseerd bent in azure AD v2-code voorbeelden, raadpleegt u [v 2.0-code voorbeelden per scenario](sample-v2-code.md).

Zie [verificatie scenario's voor Azure AD](v1-authentication-scenarios.md)voor meer informatie over het basis scenario voor elk type sample.

U kunt ook bijdragen aan onze voor beelden op GitHub. Zie Microsoft Azure Active Directory-voor [beelden en-documentatie](https://github.com/Azure-Samples?page=3&query=active-directory)voor meer informatie.

## <a name="single-page-applications"></a>Toepassingen met één pagina

In dit voor beeld ziet u hoe u een toepassing met één pagina schrijft die is beveiligd met Azure AD.

 Platform | Roept een eigen API aan | Hiermee wordt een andere web-API aangeroepen
 -------- |  --------------------- | ------------------ 
![Deze afbeelding toont het Java script-logo](media/sample-v2-code/logo_js.png) | [Java script-singlepageapp](https://github.com/Azure-Samples/active-directory-javascript-singlepageapp-dotnet-webapi) |
![Deze afbeelding toont het hoek JS-logo](media/sample-v2-code/logo_angular.png) | [angularjs-singlepageapp](https://github.com/Azure-Samples/active-directory-angularjs-singlepageapp) | [angularjs-singlepageapp-cors](https://github.com/Azure-Samples/active-directory-angularjs-singlepageapp-dotnet-webapi)

## <a name="web-applications"></a>Webtoepassingen

### <a name="web-applications-signing-in-users-calling-microsoft-graph-or-a-web-api-with-the-users-identity"></a>Webtoepassingen voor het aanmelden van gebruikers, het aanroepen van Microsoft Graph of een web-API met de identiteit van de gebruiker

De volgende voor beelden illustreren webtoepassingen die gebruikers ondertekenen. Sommige van deze toepassingen roepen ook de Microsoft Graph of uw eigen web-API aan, in de naam van de aangemelde gebruiker.

 Platform | Alleen tekenen in gebruikers | Aanroepen Microsoft Graph of AAD-grafiek| Hiermee wordt een andere ASP.NET-of ASP.NET Core 2,0-Web-API aangeroepen
 -------- | ------------------- | --------------------- | -------------------------
![In deze afbeelding wordt het ASP.NET-logo weer gegeven](media/sample-v2-code/logo_NETcore.png)</p>ASP.NET Core 2,0 | [DotNet-webapp-openidconnect-aspnetcore](https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect-aspnetcore) | [webapp-webapi-multi tenant-openidconnect-aspnetcore](https://github.com/Azure-Samples/active-directory-webapp-webapi-multitenant-openidconnect-aspnetcore/) </p>(AAD-grafiek) | [DotNet-webapp-webapi-openidconnect-aspnetcore](https://github.com/Azure-Samples/active-directory-dotnet-webapp-webapi-openidconnect-aspnetcore)
![In deze afbeelding wordt het ASP.NET-logo weer gegeven](media/sample-v2-code/logo_NETframework.png)</p> ASP.NET 4,5 | [webApp-openidconnect-DotNet](quickstart-v1-aspnet-webapp.md) </p> [webapp-WSFederation-dotNet](https://github.com/Azure-Samples/active-directory-dotnet-webapp-wsfederation) </p> [DotNet-webapp-webapi-oauth2-useridentity](https://github.com/Azure-Samples/active-directory-dotnet-webapp-webapi-oauth2-useridentity) | [DotNet-webapp-multi tenant-openidconnect](https://github.com/Azure-Samples/active-directory-dotnet-webapp-multitenant-openidconnect)</p> (AAD-grafiek) |
![Deze afbeelding toont het python-logo](media/sample-v2-code/logo_python.png) | | [python-webapp-graphapi](https://github.com/Azure-Samples/active-directory-python-webapp-graphapi)  |
![In deze afbeelding wordt het Java-logboek weer gegeven](media/sample-v2-code/logo_java.png)  | | [Java-webapp-openidconnect](https://github.com/azure-samples/active-directory-java-webapp-openidconnect)  |
![Deze afbeelding toont het PHP-logo](media/sample-v2-code/logo_php.png) | | [PHP-graphapi-Web](https://github.com/Azure-Samples/active-directory-php-graphapi-web)  |

### <a name="web-applications-demonstrating-role-based-access-control-authorization"></a>Webtoepassingen die op rollen gebaseerd toegangs beheer (autorisatie) demonstreren

De volgende voor beelden laten zien hoe u op rollen gebaseerd toegangs beheer (RBAC) implementeert. RBAC wordt gebruikt om de machtigingen van bepaalde functies in een webtoepassing te beperken tot bepaalde gebruikers. De gebruikers zijn gemachtigd, afhankelijk van of ze deel uitmaken van een **Azure AD-groep** of een **bepaalde toepassingsrol hebben.**

Platform | Voorbeeld |
 -------- | ------------------- |
![In deze afbeelding wordt het ASP.NET-logo weer gegeven](media/sample-v2-code/logo_NETframework.png)</p> ASP.NET 4,5 | [DotNet-webapp-groupclaims](https://github.com/Azure-Samples/active-directory-dotnet-webapp-groupclaims) </p>  [DotNet-webapp-roleclaims](https://github.com/Azure-Samples/active-directory-dotnet-webapp-roleclaims) | Een .NET 4,5 MVC-Web-app die gebruikmaakt van Azure AD- **rollen** voor autorisatie

## <a name="desktop-and-mobile-public-client-applications-calling-microsoft-graph-or-a-web-api"></a>Desktop-en Mobile Public client-toepassingen die Microsoft Graph of een web-API aanroepen

De volgende voor beelden illustreren open bare client toepassingen (deskto/pmobile-toepassingen) die toegang hebben tot de Microsoft Graph of een web-API in de naam van een gebruiker. Afhankelijk van de apparaten en platformen kunnen toepassingen zich op verschillende manieren aanmelden (stromen/subsidies):

- Interactief
- Op de achtergrond (met geïntegreerde Windows-verificatie in Windows of gebruikers naam/wacht woord)
- Door de interactieve aanmelding te delegeren naar een ander apparaat (apparaatcode stroom op apparaten die geen Webbe sturings elementen bieden)

Client toepassing | Platform | Stroom/verlenen | Aanroepen Microsoft Graph | Hiermee wordt een ASP.NET-of ASP.NET Core 2. x-Web-API aangeroepen
------------------ | -------- | ---------- | -------------------- | -------------------------
Bureau blad (WPF)           | ![In deze afbeelding wordt het .NETC# /logo weer gegeven](media/sample-v2-code/logo_NET.png)  | Interactief | Onderdeel van [DotNet-systeem eigen doel](https://github.com/azure-samples/active-directory-dotnet-native-multitarget) | [DotNet-systeem eigen-desktop](https://github.com/Azure-Samples/active-directory-dotnet-native-desktop) </p> [DotNet-native-aspnetcore](https://github.com/Azure-Samples/active-directory-dotnet-native-aspnetcore/)</p> [DotNet-webapi-hand matige-JWT-validatie](https://github.com/azure-samples/active-directory-dotnet-webapi-manual-jwt-validation)
Mobiel (UWP)            | .![Deze afbeelding toont de .NET-C#/UWP](media/sample-v2-code/logo_Windows.png)   | Interactief | [DotNet-systeem eigen-UWP-WAM](https://github.com/azure-samples/active-directory-dotnet-native-uwp-wam) </p> Dit voor beeld maakt gebruik van [WAM](/windows/uwp/security/web-account-manager), niet [ADAL.net](https://aka.ms/adalnet)|  [DotNet-Windows-Store](https://github.com/Azure-Samples/active-directory-dotnet-windows-store) (UWP-toepassing met behulp van ADAL.net om één Tenant Web-API aan te roepen) </p> [DotNet-webapi-multi tenant-Windows-Store](https://github.com/Azure-Samples/active-directory-dotnet-webapi-multitenant-windows-store) (UWP-toepassing met ADAL.net voor het aanroepen van een web-API met meerdere tenants)|
Mobiel (Android, iOS, UWP)   | ![Deze afbeelding toont de .NET/C# (Xamarin)](media/sample-v2-code/logo_xamarin.png) | Interactief | [DotNet-systeem eigen doel](https://github.com/azure-samples/active-directory-dotnet-native-multitarget) |
Mobiel (Android)           | ![Deze afbeelding toont het Android-logo](media/sample-v2-code/logo_Android.png) | Interactief |   [Android](https://github.com/Azure-Samples/active-directory-android) |
Mobiel (iOS)           | ![Deze afbeelding toont iOS/objectief C of SWIFT](media/sample-v2-code/logo_iOS.png) | Interactief |   [nativeClient-iOS](https://github.com/azureadquickstarts/nativeclient-ios) |
Bureau blad (console)          | ![In deze afbeelding wordt het .NETC# /logo weer gegeven](media/sample-v2-code/logo_NET.png) | Gebruikers naam/wacht woord </p>  Geïntegreerde Windows-verificatie | | [DotNet-native-headless](https://github.com/azure-samples/active-directory-dotnet-native-headless)
Bureau blad (console)          | ![In deze afbeelding wordt het Java-logo weer gegeven](media/sample-v2-code/logo_Java.png) | Gebruikers naam/wacht woord | | [Java-Native-headless](https://github.com/Azure-Samples/active-directory-java-native-headless)
Bureau blad (console)           | ![In deze afbeelding wordt de .NET-C# kern/het logo weer gegeven](media/sample-v2-code/logo_NETcore.png) | Toestel code stroom | | [DotNet-deviceprofile](https://github.com/Azure-Samples/active-directory-dotnet-deviceprofile)

## <a name="daemon-applications-accessing-web-apis-with-the-applications-identity"></a>Daemon-toepassingen (toegang tot Web-Api's met de identiteit van de toepassing)

De volgende voor beelden tonen bureau blad-of webtoepassingen die toegang hebben tot de Microsoft Graph of een web-API zonder gebruiker (met de toepassings-id).

Client toepassing | Platform | Stroom/verlenen | Hiermee wordt een ASP.NET-of ASP.NET Core 2,0-Web-API aangeroepen
------------------ | -------- | ---------- | -------------------- 
Daemon-app (console)          | ![In deze afbeelding wordt het .NET-logo weer gegeven](media/sample-v2-code/logo_NETframework.png) | Client referenties met geheim of certificaat van app | [DotNet-daemon](https://github.com/azure-samples/active-directory-dotnet-daemon)</p> [DotNet-daemon-certificaat-referentie](https://github.com/azure-samples/active-directory-dotnet-daemon-certificate-credential)
Daemon-app (console)         | ![In deze afbeelding wordt het .NET-logo weer gegeven](media/sample-v2-code/logo_NETcore.png) | Client referenties met certificaat| [dotnetcore-daemon-certificaat-referentie](https://github.com/Azure-Samples/active-directory-dotnetcore-daemon-certificate-credential)
ASP.NET-Web-app  | ![In deze afbeelding wordt het .NET-logo weer gegeven](media/sample-v2-code/logo_NETframework.png) | Clientreferenties | [DotNet-webapp-webapi-oauth2-APPIDENTITY](https://github.com/Azure-Samples/active-directory-dotnet-webapp-webapi-oauth2-appidentity)

## <a name="web-apis"></a>Web-API's

### <a name="web-api-protected-by-azure-active-directory"></a>Web-API beveiligd door Azure Active Directory

In het volgende voor beeld ziet u hoe u een node. js-Web-API beveiligt met Azure AD.

In de vorige secties van dit artikel vindt u ook andere voor beelden van een client toepassing die een ASP.NET of ASP.NET Core web- **API** **aanroept** . Deze voor beelden worden niet opnieuw vermeld in deze sectie, maar u vindt ze in de laatste kolom van de bovenstaande tabellen of onder

| Platform | Voorbeeld |
|--------|-------------------|
| ![Deze afbeelding toont het node. js-logo](media/sample-v2-code/logo_nodejs.png)  | [knoop punt-webapi](https://github.com/Azure-Samples/active-directory-node-webapi) |

### <a name="web-api-calling-microsoft-graph-or-another-web-api"></a>Web-API die Microsoft Graph of een andere web-API aanroept

In de volgende voor beelden ziet u een web-API die een andere web-API aanroept. Het tweede voor beeld laat zien hoe u voorwaardelijke toegang kunt verwerken.

| Platform |  Aanroepen Microsoft Graph | Hiermee wordt een andere ASP.NET-of ASP.NET Core 2,0-Web-API aangeroepen |
| -------- |  --------------------- | ------------------------- |
| ![In deze afbeelding wordt het ASP.NET-logo weer gegeven](media/sample-v2-code/logo_NETframework.png)</p> ASP.NET 4,5 | [DotNet-webapi-onbehalfof](https://github.com/azure-samples/active-directory-dotnet-webapi-onbehalfof) </p> [DotNet-webapi-onbehalfof-ca](https://github.com/azure-samples/active-directory-dotnet-webapi-onbehalfof-ca) | [DotNet-webapi-onbehalfof](https://github.com/azure-samples/active-directory-dotnet-webapi-onbehalfof) </p> [DotNet-webapi-onbehalfof-ca](https://github.com/azure-samples/active-directory-dotnet-webapi-onbehalfof-ca) |

## <a name="other-microsoft-graph-samples"></a>Andere Microsoft Graph-voor beelden

Zie [Microsoft Graph Community-voor beelden & zelf studies](https://github.com/microsoftgraph/msgraph-community-samples)voor voor beelden en zelf studies die verschillende gebruiks patronen demonstreren voor de Microsoft Graph-API, inclusief verificatie met Azure AD.

## <a name="see-also"></a>Zie ook

[Ontwikkelaars handleiding Azure Active Directory](v1-overview.md)

[Verificatie bibliotheken Azure Active Directory](active-directory-authentication-libraries.md)

[Concepten en naslag informatie over Azure AD Graph API](https://msdn.microsoft.com/library/azure/hh974476.aspx)

[Azure AD Graph API helper-bibliotheek](https://www.nuget.org/packages/Microsoft.Azure.ActiveDirectory.GraphClient)
