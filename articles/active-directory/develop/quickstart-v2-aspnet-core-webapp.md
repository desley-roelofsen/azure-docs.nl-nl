---
title: Aanmelden met micro soft toevoegen aan ASP.NET Core web apps-micro soft Identity platform | Azure
description: Informatie over het implementeren van Microsoft-aanmelding in een ASP.NET Core-web-app met behulp van OpenID Connect
services: active-directory
author: jmprieur
manager: CelesteDG
ms.assetid: 820acdb7-d316-4c3b-8de9-79df48ba3b06
ms.service: active-directory
ms.subservice: develop
ms.topic: quickstart
ms.workload: identity
ms.date: 04/11/2019
ms.author: jmprieur
ms.custom: aaddev, identityplatformtop40
ms.collection: M365-identity-device-management
ms.openlocfilehash: b86c8f79902c0234e35e4d195e4680b6e9b3f3c2
ms.sourcegitcommit: 5ab4f7a81d04a58f235071240718dfae3f1b370b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/10/2019
ms.locfileid: "74963542"
---
# <a name="quickstart-add-sign-in-with-microsoft-to-an-aspnet-core-web-app"></a>Snelstartgids: aanmelden toevoegen met micro soft aan een ASP.NET Core web-app

[!INCLUDE [active-directory-develop-applies-v2](../../../includes/active-directory-develop-applies-v2.md)]

In deze snelstart leert u hoe een ASP.NET Core-web-app persoonlijke accounts (hotmail.com, outlook.com, anderen) en werk- en schoolaccounts kan aanmelden vanuit een willekeurig exemplaar van Azure Active Directory (Azure AD).

![Toont hoe de voor beeld-app die door deze Quick start is gegenereerd, werkt](media/quickstart-v2-aspnet-core-webapp/aspnetcorewebapp-intro.svg)

> [!div renderon="docs"]
> ## <a name="register-and-download-your-quickstart-app"></a>De quickstart-app registreren en downloaden
> U hebt twee opties voor het starten van de snelstarttoepassing:
> * [Express] [Optie 1: registreer de toepassing en laat deze automatisch configureren. Download vervolgens de voorbeeldcode](#option-1-register-and-auto-configure-your-app-and-then-download-your-code-sample)
> * [Handmatig] [Optie 2: registreer de toepassing en configureer handmatig de toepassing en het codevoorbeeld](#option-2-register-and-manually-configure-your-application-and-code-sample)
>
> ### <a name="option-1-register-and-auto-configure-your-app-and-then-download-your-code-sample"></a>Optie 1: de app registreren en automatisch configureren, en vervolgens de voorbeeldcode downloaden
>
> 1. Ga naar de [Azure Portal-app-registraties](https://aka.ms/aspnetcore2-1-aad-quickstart-v2).
> 1. Voer een naam in voor de toepassing en selecteer **Registreren**.
> 1. Volg de instructies om de nieuwe toepassing met slechts één klik te downloaden en automatisch te configureren.
>
> ### <a name="option-2-register-and-manually-configure-your-application-and-code-sample"></a>Optie 2: de toepassing en voorbeeldcode registreren en handmatig configureren
>
> #### <a name="step-1-register-your-application"></a>Stap 1: Uw toepassing registreren
> Volg deze stappen om de toepassing te registreren en de registratiegegevens van de app handmatig toe te voegen aan uw oplossing:
>
> 1. Meld u bij de [Azure-portal](https://portal.azure.com) aan met een werk- of schoolaccount of een persoonlijk Microsoft-account.
> 1. Als u via uw account toegang tot meer dan één tenant hebt, selecteert u uw account in de rechterbovenhoek en stelt u uw portalsessie in op de gewenste Azure Active Directory-tenant.
> 1. Navigeer naar de pagina micro soft-identiteits platform voor ontwikkel aars [app-registraties](https://go.microsoft.com/fwlink/?linkid=2083908) .
> 1. Selecteer **nieuwe registratie**.
> 1. Wanneer de pagina **Een toepassing registreren** verschijnt, voert u de registratiegegevens van de toepassing in:
>    - Voer in de sectie **Naam** een beschrijvende toepassingsnaam. Deze wordt zichtbaar voor gebruikers van de app. Bijvoorbeeld: `AspNetCore-Quickstart`.
>    - Voeg `https://localhost:44321/`in de **omleidings-URI**toe en selecteer **registreren**.
> 1. Selecteer het menu **Verificatie** en voeg dan de volgende gegevens toe:
>    - In **omleidings-uri's**voegt u `https://localhost:44321/signin-oidc`toe en selecteert u **Opslaan**.
>    - Bij **Geavanceerde instellingen** stelt u de **afmeldings-URL** in op `https://localhost:44321/signout-oidc`.
>    - Bij **Impliciete toekenning** controleert u de **id-tokens**.
>    - Selecteer **Opslaan**.

> [!div class="sxs-lookup" renderon="portal"]
> #### <a name="step-1-configure-your-application-in-the-azure-portal"></a>Stap 1: uw toepassing configureren in de Azure-portal
> Als u het codevoorbeeld uit deze snelstart wilt gebruiken, moet u antwoord-URL's toevoegen als `https://localhost:44321/` en `https://localhost:44321/signin-oidc`, voegt u de afmeldings-URL toe als `https://localhost:44321/signout-oidc` en vraagt u de id-tokens aan die moeten worden uitgegeven door het autorisatie-eindpunt.
> > [!div renderon="portal" id="makechanges" class="nextstepaction"]
> > [Deze wijziging voor mij maken]()
>
> > [!div id="appconfigured" class="alert alert-info"]
> > ![Al geconfigureerd](media/quickstart-v2-aspnet-webapp/green-check.png) Uw toepassing is al geconfigureerd met deze kenmerken.

#### <a name="step-2-download-your-aspnet-core-project"></a>Stap 2: uw ASP.NET Core-project downloaden

- [Down load de Visual Studio 2019-oplossing](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/archive/aspnetcore2-2.zip)

#### <a name="step-3-configure-your-visual-studio-project"></a>Stap 3: Uw Visual Studio-project configureren

1. Pak het zip-bestand uit in een lokale map in de hoofdmap (bijvoorbeeld **C:\Azure-Samples**)
1. Als u Visual Studio 2019 gebruikt, opent u de oplossing in Visual Studio (optioneel).
1. Bewerk het bestand **appsettings.json**. Zoek `ClientId` en werk de waarde van `ClientId` bij met de ID-waarde van de **toepassing (client)** van de toepassing die u hebt geregistreerd. 

    ```json
    "ClientId": "Enter_the_Application_Id_here"
    "TenantId": "Enter_the_Tenant_Info_Here"
    ```

> [!div class="sxs-lookup" renderon="portal"]
> > [!NOTE]
> > Deze Quick Start ondersteunt Enter_the_Supported_Account_Info_Here.

> [!div renderon="docs"]
> Waar:
> - `Enter_the_Application_Id_here`: is de **toepassings-id** (client-id) voor de toepassing die is geregistreerd in de Azure-portal. U vindt de **toepassings-id (client-id)** op de pagina **Overzicht** van de app.
> - `Enter_the_Tenant_Info_Here` - is een van de volgende opties:
>   - Als uw toepassing **Alleen accounts in deze organisatiemap** ondersteunt, vervangt u deze waarde door de **tenant-id** of **tenantnaam** (bijvoorbeeld contoso.microsoft.com)
>   - Als uw toepassing **Accounts in elke organisatiemap** ondersteunt, vervang deze waarde dan door `organizations`
>   - Als uw toepassing **Alle Microsoft-accountgebruikers** ondersteunt, vervang deze waarde dan door `common`
>
> > [!TIP]
> > Om de waarden van **Toepassings-id (client-id)** , **Map-id (tenant-id)** en **Ondersteunde accounttypen** te achterhalen, gaat u naar de **Overzichtspagina** van de app in de Azure-portal.

## <a name="more-information"></a>Meer informatie

In deze sectie vindt u een overzicht van de code die is vereist om gebruikers aan te melden. Dit overzicht kan handig zijn om te begrijpen hoe de code werkt, hoofd argumenten en ook als u aanmelden wilt toevoegen aan een bestaande ASP.NET Core-toepassing.

### <a name="startup-class"></a>Opstartklasse

De *Microsoft.AspNetCore.Authentication*-middleware maakt gebruik van een opstartklasse die wordt uitgevoerd wanneer in het hostingproces het volgende wordt geïnitialiseerd:

```csharp
public void ConfigureServices(IServiceCollection services)
{
  services.Configure<CookiePolicyOptions>(options =>
  {
    // This lambda determines whether user consent for non-essential cookies is needed for a given request.
    options.CheckConsentNeeded = context => true;
    options.MinimumSameSitePolicy = SameSiteMode.None;
  });

  services.AddAuthentication(AzureADDefaults.AuthenticationScheme)
          .AddAzureAD(options => Configuration.Bind("AzureAd", options));

  services.Configure<OpenIdConnectOptions>(AzureADDefaults.OpenIdScheme, options =>
  {
    options.Authority = options.Authority + "/v2.0/";         // Microsoft identity platform

    options.TokenValidationParameters.ValidateIssuer = false; // accept several tenants (here simplified)
  });

  services.AddMvc(options =>
  {
     var policy = new AuthorizationPolicyBuilder()
                     .RequireAuthenticatedUser()
                     .Build();
     options.Filters.Add(new AuthorizeFilter(policy));
  })
  .SetCompatibilityVersion(CompatibilityVersion.Version_2_1);
}
```

Met de methode `AddAuthentication` de service geconfigureerd voor het toevoegen van verificatie op basis van cookies, die wordt gebruikt in browser scenario's en voor het instellen van de uitdaging om verbinding te maken met OpenID Connect. 

De regel met `.AddAzureAd` de micro soft Identity platform-verificatie toevoegen aan uw toepassing. Deze wordt vervolgens geconfigureerd om u aan te melden met het micro soft Identity platform-eind punt.

> |Waar  |  |
> |---------|---------|
> | ClientId  | Toepassings-id (client-id) van de toepassing die is geregistreerd in de Azure-portal. |
> | Instantie | Het STS-eindpunt voor gebruikersverificatie. Meestal is dit <https://login.microsoftonline.com/{tenant}/v2.0> voor openbare cloud, waarbij {tenant} de naam is van uw tenant, uw tenant-id of *common* voor een verwijzing naar het algemene eindpunt (gebruikt voor toepassingen met meerdere tenants) |
> | TokenValidationParameters | Een lijst met parameters voor de validatie van tokens. In dit geval is `ValidateIssuer` ingesteld op `false` om aan te geven dat aanmeldingen vanaf persoonlijke, werk- of schoolaccounts kunnen worden geaccepteerd. |


> [!NOTE]
> Het instellen van `ValidateIssuer = false` is een vereenvoudiging voor deze Quick Start. In echte toepassingen moet u de uitgever valideren.
> Bekijk de voor beelden om te begrijpen hoe u dit doet.

### <a name="protect-a-controller-or-a-controllers-method"></a>Een controller of de methode van een controller beveiligen

U kunt een controller of controllermethoden beveiligen met behulp van het kenmerk `[Authorize]`. Met dit kenmerk wordt de toegang tot de controller of methoden beperkt door alleen geverifieerde gebruikers toe te staan, wat betekent dat de verificatie vraag kan worden gestart om toegang te krijgen tot de controller als de gebruiker niet is geverifieerd.

[!INCLUDE [Help and support](../../../includes/active-directory-develop-help-support-include.md)]

## <a name="next-steps"></a>Volgende stappen

Bekijk de GitHub-opslag plaats voor deze ASP.NET Core-zelf studie voor meer informatie, waaronder instructies over het toevoegen van verificatie aan een gloed nieuwe ASP.NET Core-webtoepassing, het aanroepen van Microsoft Graph en andere micro soft-Api's, het aanroepen van uw eigen Api's, hoe u deze kunt toevoegen autorisatie, het aanmelden van gebruikers in nationale Clouds of met sociale identiteiten en meer:

> [!div class="nextstepaction"]
> [Zelf studie voor ASP.NET Core web-app](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/)

Help ons het micro soft Identity-platform te verbeteren. Vertel ons wat u denkt door een korte enquête met twee vragen te volt ooien.

> [!div class="nextstepaction"]
> [Micro soft Identity platform-enquête](https://forms.office.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbRyKrNDMV_xBIiPGgSvnbQZdUQjFIUUFGUE1SMEVFTkdaVU5YT0EyOEtJVi4u)