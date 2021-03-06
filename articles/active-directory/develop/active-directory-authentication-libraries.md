---
title: Verificatie bibliotheken Azure Active Directory | Microsoft Docs
description: Met de Azure AD Authentication Library (ADAL) kunnen ontwikkel aars van client toepassingen eenvoudig gebruikers verifiëren voor Cloud-of on-premises Active Directory (AD) en vervolgens toegangs tokens verkrijgen voor het beveiligen van API-aanroepen.
services: active-directory
author: rwike77
manager: CelesteDG
ms.assetid: 2e4fc79a-0285-40be-8c77-65edee408a22
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 12/01/2018
ms.author: ryanwi
ms.reviewer: saeeda, jmprieur
ms.custom: aaddev, identityplatformtop40
ms.collection: M365-identity-device-management
ms.openlocfilehash: c391120ccf9fafd4ca51489d7849e838d77a1dbf
ms.sourcegitcommit: c38a1f55bed721aea4355a6d9289897a4ac769d2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/05/2019
ms.locfileid: "74845498"
---
# <a name="azure-active-directory-authentication-libraries"></a>Verificatie bibliotheken Azure Active Directory

Met de Azure Active Directory Authentication Library (ADAL) v 1.0 kunnen ontwikkel aars van toepassingen gebruikers verifiëren in de Cloud of on-premises Active Directory (AD), en tokens verkrijgen voor het beveiligen van API-aanroepen. ADAL maakt verificatie gemakkelijker voor ontwikkel aars via functies zoals:

- Configureer bare token cache waarin toegangs tokens worden opgeslagen en tokens worden vernieuwd
- Automatische token vernieuwing wanneer een toegangs token verloopt en er een vernieuwings token beschikbaar is
- Ondersteuning voor asynchrone methode aanroepen

> [!NOTE]
> Zoekt u de Azure AD v 2.0-bibliotheken (MSAL)? De [MSAL-bibliotheek handleiding](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-libraries)uitchecken.
>
>

## <a name="microsoft-supported-client-libraries"></a>Door micro soft ondersteunde client bibliotheken

| Platform | Bibliotheek | Downloaden | Broncode | Voorbeeld | Referentie
| --- | --- | --- | --- | --- | --- |
| .NET-client, Windows Store, UWP, Xamarin iOS en Android |ADAL .NET v3 |[NuGet](https://www.nuget.org/packages/Microsoft.IdentityModel.Clients.ActiveDirectory) |[GitHub](https://github.com/AzureAD/azure-activedirectory-library-for-dotnet) | [Bureau blad-app](https://docs.microsoft.com/azure/active-directory/active-directory-devquickstarts-dotnet) |[Naslaginformatie](https://docs.microsoft.com/dotnet/api/microsoft.identitymodel.clients.activedirectory?view=azure-dotnet) |
| .NET-client, Windows Store, Windows Phone 8,1 |ADAL .NET v2 |[NuGet](https://www.nuget.org/packages/Microsoft.IdentityModel.Clients.ActiveDirectory/2.28.4) |[GitHub](https://github.com/AzureAD/azure-activedirectory-library-for-dotnet/releases/tag/v2.28.4) | [Bureau blad-app](https://github.com/AzureADQuickStarts/NativeClient-DotNet/releases/tag/v2.X) | |
| Javascript |ADAL.js |[GitHub](https://github.com/AzureAD/azure-activedirectory-library-for-js) |[GitHub](https://github.com/AzureAD/azure-activedirectory-library-for-js) |[App van één pagina](https://github.com/Azure-Samples/active-directory-javascript-singlepageapp-dotnet-webapi) | |
| iOS, macOS |ADAL |[GitHub](https://github.com/AzureAD/azure-activedirectory-library-for-objc/releases) |[GitHub](https://github.com/AzureAD/azure-activedirectory-library-for-objc) |[iOS-app](https://docs.microsoft.com/azure/active-directory/active-directory-devquickstarts-ios) | [Naslaginformatie](http://cocoadocs.org/docsets/ADAL/2.5.1/)|
| Android |ADAL |[Maven](https://search.maven.org/search?q=g:com.microsoft.aad+AND+a:adal&core=gav) |[GitHub](https://github.com/AzureAD/azure-activedirectory-library-for-android) |[Android-app](https://docs.microsoft.com/azure/active-directory/active-directory-devquickstarts-android) | [JavaDocs](https://javadoc.io/doc/com.microsoft.aad/adal/)|
| Node.js |ADAL |[npm](https://www.npmjs.com/package/adal-node) |[GitHub](https://github.com/AzureAD/azure-activedirectory-library-for-nodejs) | [Node.js-web-app](https://github.com/Azure-Samples/active-directory-node-webapp-openidconnect)|[Naslaginformatie](https://docs.microsoft.com/javascript/api/overview/azure/activedirectory) |
| Java |ADAL4J |[Maven](https://search.maven.org/#search%7Cga%7C1%7Ca%3Aadal4j%20g%3Acom.microsoft.azure) |[GitHub](https://github.com/AzureAD/azure-activedirectory-library-for-java) |[Java-web-app](https://github.com/Azure-Samples/active-directory-java-webapp-openidconnect) |[Naslaginformatie](https://javadoc.io/doc/com.microsoft.azure/adal4j) |
| Python |ADAL |[GitHub](https://github.com/AzureAD/azure-activedirectory-library-for-python) |[GitHub](https://github.com/AzureAD/azure-activedirectory-library-for-python) |[Python-web-app](https://github.com/Azure-Samples/active-directory-python-webapp-graphapi) |[Naslaginformatie](https://adal-python.readthedocs.io/) |

## <a name="microsoft-supported-server-libraries"></a>Door micro soft ondersteunde server bibliotheken

| Platform | Bibliotheek | Downloaden | Broncode | Voorbeeld | Referentie
| --- | --- | --- | --- | --- | --- |
| .NET |OWIN voor AzureAD|[NuGet](https://www.nuget.org/packages/Microsoft.Owin.Security.ActiveDirectory/) |[GitHub](https://github.com/aspnet/AspNetKatana/tree/dev/src/Microsoft.Owin.Security.ActiveDirectory) |[MVC App](https://docs.microsoft.com/azure/active-directory/active-directory-devquickstarts-webapp-dotnet) | |
| .NET |OWIN voor OpenIDConnect |[NuGet](https://www.nuget.org/packages/Microsoft.Owin.Security.OpenIdConnect) |[GitHub](https://github.com/aspnet/AspNetKatana/tree/dev/src/Microsoft.Owin.Security.OpenIdConnect) |[Web-app](https://github.com/AzureADSamples/WebApp-OpenIDConnect-DotNet) | |
| .NET |OWIN voor WS-Federation |[NuGet](https://www.nuget.org/packages/Microsoft.Owin.Security.WsFederation) |[GitHub](https://github.com/aspnet/AspNetKatana/tree/dev/src/Microsoft.Owin.Security.WsFederation) |[MVC Web App](https://github.com/AzureADSamples/WebApp-WSFederation-DotNet) | |
| .NET |Identity Protocol-extensies voor .NET 4,5 |[NuGet](https://www.nuget.org/packages/Microsoft.IdentityModel.Protocol.Extensions) |[GitHub](https://github.com/AzureAD/azure-activedirectory-identitymodel-extensions-for-dotnet) | | |
| .NET |JWT-handler voor .NET 4,5 |[NuGet](https://www.nuget.org/packages/System.IdentityModel.Tokens.Jwt) |[GitHub](https://github.com/AzureAD/azure-activedirectory-identitymodel-extensions-for-dotnet) | | |
| Node.js |Azure AD Pass Port |[npm](https://www.npmjs.com/package/passport-azure-ad) |[GitHub](https://github.com/AzureAD/passport-azure-ad) | [Web-API](https://docs.microsoft.com/azure/active-directory/active-directory-devquickstarts-webapi-nodejs)| |

## <a name="scenarios"></a>Scenario's

Hier volgen drie veelvoorkomende scenario's voor het gebruik van ADAL in een client die toegang heeft tot een externe bron:

### <a name="authenticating-users-of-a-native-client-application-running-on-a-device"></a>Gebruikers verifiëren van een systeem eigen client toepassing die op een apparaat wordt uitgevoerd

In dit scenario heeft een ontwikkelaar een mobiele client-of bureaublad toepassing die toegang nodig heeft tot een externe bron, zoals een web-API. De Web-API staat geen anonieme aanroepen toe en moet worden aangeroepen in de context van een geverifieerde gebruiker. De Web-API is vooraf geconfigureerd voor het vertrouwen van de toegangs tokens die zijn uitgegeven door een specifieke Azure AD-Tenant. Azure AD is vooraf geconfigureerd voor het uitgeven van toegangs tokens voor die bron. Voor het aanroepen van de Web-API van de client gebruikt de ontwikkelaar ADAL om verificatie met Azure AD te vergemakkelijken. De veiligste manier om ADAL te gebruiken, is het weer geven van de gebruikers interface voor het verzamelen van gebruikers referenties (weer gegeven als browser venster).

ADAL maakt het eenvoudig om de gebruiker te verifiëren, een toegangs token op te halen en token te vernieuwen vanuit Azure AD en vervolgens de Web-API aan te roepen met behulp van het toegangs token.

Zie [native client WPF application to Web API](https://github.com/azureadsamples/nativeclient-dotnet)(Engelstalig) voor een code voorbeeld waarin dit scenario wordt gedemonstreerd met behulp van verificatie voor Azure AD.

### <a name="authenticating-a-confidential-client-application-running-on-a-web-server"></a>Verificatie van een vertrouwelijke client toepassing die wordt uitgevoerd op een webserver

In dit scenario heeft een ontwikkelaar een toepassing die wordt uitgevoerd op een server die toegang nodig heeft tot een externe bron, zoals een web-API. De Web-API staat geen anonieme aanroepen toe en moet daarom worden aangeroepen vanuit een geautoriseerde service. De Web-API is vooraf geconfigureerd voor het vertrouwen van de toegangs tokens die zijn uitgegeven door een specifieke Azure AD-Tenant. Azure AD is vooraf geconfigureerd voor het uitgeven van toegangs tokens voor die bron aan een service met client referenties (client-ID en geheim). ADAL vereenvoudigt de verificatie van de service met Azure AD die een toegangs token retourneert dat kan worden gebruikt om de Web-API aan te roepen. ADAL zorgt ook voor het beheren van de levens duur van het toegangs token door het in de cache te plaatsen en zo nodig te vernieuwen. Zie voor een code voorbeeld dat dit scenario demonstreert de [toepassing daemon console to Web API](https://github.com/AzureADSamples/Daemon-DotNet).

### <a name="authenticating-a-confidential-client-application-running-on-a-server-on-behalf-of-a-user"></a>Verificatie van een vertrouwelijke client toepassing die op een server wordt uitgevoerd namens een gebruiker

In dit scenario heeft een ontwikkelaar een webtoepassing die wordt uitgevoerd op een server die toegang nodig heeft tot een externe bron, zoals een web-API. De Web-API staat geen anonieme aanroepen toe, zodat deze moet worden aangeroepen vanuit een geautoriseerde service namens een geverifieerde gebruiker. De Web-API is vooraf geconfigureerd voor het vertrouwen van toegangs tokens die zijn uitgegeven door een specifieke Azure AD-Tenant, en Azure AD is vooraf geconfigureerd om toegangs tokens voor die bron te verlenen aan een service met client referenties. Zodra de gebruiker is geverifieerd in de webtoepassing, kan de toepassing een autorisatie code voor de gebruiker verkrijgen uit Azure AD. De webtoepassing kan vervolgens ADAL gebruiken voor het verkrijgen van een toegangs token en het vernieuwen van token namens een gebruiker met behulp van de autorisatie code en de client referenties die zijn gekoppeld aan de toepassing vanuit Azure AD. Zodra de webtoepassing in bezit is van het toegangs token, kan de Web-API worden aangeroepen totdat het token verloopt. Wanneer het token verloopt, kan de webtoepassing ADAL gebruiken om een nieuw toegangs token op te halen met behulp van het vernieuwings token dat eerder is ontvangen. Zie [native client to Web API to Web API](https://github.com/Azure-Samples/active-directory-dotnet-webapi-onbehalfof)(Engelstalig) voor een code voorbeeld waarin dit scenario wordt gedemonstreerd.

## <a name="see-also"></a>Zie ook

- [De hand leiding voor Azure Active Directory ontwikkel aars](v1-overview.md)
- [Verificatie scenario's voor Azure Active Directory](v1-authentication-scenarios.md)
- [Voor beelden van Azure Active Directory code](sample-v1-code.md)
