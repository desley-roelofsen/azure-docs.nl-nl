---
title: Quickstart voor Azure-app-configuratie met ASP.NET Core | Microsoft Docs
description: Een quickstart voor het gebruik van Azure-app-configuratie met ASP.NET Core-apps
services: azure-app-configuration
documentationcenter: ''
author: yegu-ms
manager: balans
editor: ''
ms.assetid: ''
ms.service: azure-app-configuration
ms.devlang: csharp
ms.topic: quickstart
ms.tgt_pltfrm: ASP.NET Core
ms.workload: tbd
ms.date: 10/11/2019
ms.author: yegu
ms.openlocfilehash: 91712b3f730317e65cda7b48c8f5636b2fb9ab2c
ms.sourcegitcommit: dbde4aed5a3188d6b4244ff7220f2f75fce65ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/19/2019
ms.locfileid: "74185093"
---
# <a name="quickstart-create-an-aspnet-core-app-with-azure-app-configuration"></a>Snelstartgids: een ASP.NET Core-app maken met Azure-app configuratie

In deze Snelstartgids neemt u Azure-app configuratie op in een ASP.NET Core-app om de opslag en het beheer van toepassings instellingen gescheiden van uw code te centraliseren. ASP.NET Core bouwt één op sleutel waarde gebaseerd configuratie object met behulp van instellingen uit een of meer gegevens bronnen die door een toepassing worden opgegeven. Deze gegevens bronnen worden *configuratie providers*genoemd. Omdat de .NET core-client van de app-configuratie wordt geïmplementeerd als een dergelijke provider, wordt de service weer gegeven als een andere gegevens bron.

## <a name="prerequisites"></a>Vereisten

- Azure-abonnement: [Maak er gratis een](https://azure.microsoft.com/free/)
- [.NET Core-SDK](https://dotnet.microsoft.com/download)

## <a name="create-an-app-configuration-store"></a>Een app-configuratie archief maken

[!INCLUDE [azure-app-configuration-create](../../includes/azure-app-configuration-create.md)]

6. Selecteer **configuratie verkenner** >  **+ maken** om de volgende sleutel-waardeparen toe te voegen:

    | Sleutel | Waarde |
    |---|---|
    | TestApp:Settings:BackgroundColor | Wit |
    | TestApp:Settings:FontSize | 24 |
    | TestApp:Settings:FontColor | Zwart |
    | TestApp:Settings:Message | Gegevens van Azure App Configuration |

    Laat het **Label** en het **inhouds type** nu leeg.

## <a name="create-an-aspnet-core-web-app"></a>Een ASP.NET Core-web-app maken

U gebruikt de [.net core-opdracht regel interface (CLI)](https://docs.microsoft.com/dotnet/core/tools/) om een nieuw ASP.net core MVC-Web-app-project te maken. Het voor deel van het gebruik van de .NET Core SLI over Visual Studio is dat het beschikbaar is via de Windows-, macOS-en Linux-platformen.

1. Maak een nieuwe map voor uw project. Geef voor deze Snelstartgids de naam *TestAppConfig*.

2. Voer in de nieuwe map de volgende opdracht uit om een nieuw ASP.NET Core MVC-Web-app-project te maken:

    ```CLI
        dotnet new mvc --no-https
    ```

## <a name="add-secret-manager"></a>Secret Manager toevoegen

Als u een geheim Manager wilt gebruiken, voegt u een `UserSecretsId`-element toe aan uw *. csproj* -bestand.

- Open het *. csproj* -bestand. Voeg een `UserSecretsId`-element toe, zoals hier wordt weer gegeven. U kunt dezelfde GUID gebruiken, maar u kunt deze waarde ook vervangen door uw eigen waarden. Sla het bestand op.

    ```xml
    <Project Sdk="Microsoft.NET.Sdk.Web">

    <PropertyGroup>
        <TargetFramework>netcoreapp2.1</TargetFramework>
        <UserSecretsId>79a3edd0-2092-40a2-a04d-dcb46d5ca9ed</UserSecretsId>
    </PropertyGroup>

    <ItemGroup>
        <PackageReference Include="Microsoft.AspNetCore.App" />
        <PackageReference Include="Microsoft.AspNetCore.Razor.Design" Version="2.1.2" PrivateAssets="All" />
    </ItemGroup>

    </Project>
    ```

Dit hulpprogramma slaat gevoelige gegevens voor ontwikkeltaken op buiten de projectstructuur. Deze aanpak voorkomt dat er per ongeluk appgeheimen worden gedeeld in de broncode. Voor meer informatie over de geheime beheerder raadpleegt u de [veilige opslag van app-geheimen in de ontwikkeling van ASP.net core](https://docs.microsoft.com/aspnet/core/security/app-secrets)

## <a name="connect-to-an-app-configuration-store"></a>Verbinding maken met een app-configuratie archief

1. Voeg een verwijzing naar het NuGet-pakket van `Microsoft.Azure.AppConfiguration.AspNetCore` toe door de volgende opdracht uit te voeren:

    ```CLI
        dotnet add package Microsoft.Azure.AppConfiguration.AspNetCore --version 2.0.0-preview-010060003-1250
    ```
2. Voer de volgende opdracht uit om de pakketten voor uw project te herstellen:

    ```CLI
        dotnet restore
    ```
3. Voeg een geheim met de naam *ConnectionStrings:AppConfig* toe aan Secret Manager.

    Dit geheim bevat de connection string voor toegang tot uw app-configuratie opslag. Vervang de waarde in de volgende opdracht door de connection string voor uw app-configuratie archief.

    Deze opdracht moet worden uitgevoerd in de map met het bestand *.csproj*.

    ```CLI
        dotnet user-secrets set ConnectionStrings:AppConfig <your_connection_string>
    ```

    > [!IMPORTANT]
    > Bij sommige shells worden de connection string afgekapt, tenzij deze tussen aanhalings tekens staan. Zorg ervoor dat de uitvoer van de `dotnet user-secrets` opdracht de volledige connection string toont. Als dat niet het geval is, voert u de opdracht opnieuw uit en plaatst u de connection string tussen aanhalings tekens.

    Geheime beheerder wordt alleen gebruikt om de web-app lokaal te testen. Wanneer de app is geïmplementeerd op [Azure app service](https://azure.microsoft.com/services/app-service/web), gebruikt u bijvoorbeeld een toepassings instelling **verbindings reeksen** in app service in plaats van met de geheime beheerder om de Connection String op te slaan.

    Dit geheim wordt geopend met de configuratie-API. Een dubbele punt (:) werkt in de configuratie naam met de configuratie-API op alle ondersteunde platforms. Zie [configuratie per omgeving](https://docs.microsoft.com/aspnet/core/fundamentals/configuration/index?tabs=basicconfiguration&view=aspnetcore-2.0).

4. Open *Program.cs*en voeg een verwijzing toe naar de .net core-app configuratie provider.

    ```csharp
    using Microsoft.Extensions.Configuration.AzureAppConfiguration;
    ```

5. Werk de `CreateWebHostBuilder`-methode bij om app-configuratie te gebruiken door de `config.AddAzureAppConfiguration()`-methode aan te roepen.
    
    > [!IMPORTANT]
    > `CreateHostBuilder` vervangt `CreateWebHostBuilder` in .NET Core 3,0.  Selecteer de juiste syntaxis op basis van uw omgeving.

    ### <a name="update-createwebhostbuilder-for-net-core-2x"></a>Update `CreateWebHostBuilder` voor .NET Core 2. x

    ```csharp
    public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
        WebHost.CreateDefaultBuilder(args)
            .ConfigureAppConfiguration((hostingContext, config) =>
            {
                var settings = config.Build();
                config.AddAzureAppConfiguration(settings["ConnectionStrings:AppConfig"]);
            })
            .UseStartup<Startup>();
    ```

    ### <a name="update-createhostbuilder-for-net-core-3x"></a>Update `CreateHostBuilder` voor .NET Core 3. x

    ```csharp
    public static IHostBuilder CreateHostBuilder(string[] args) =>
        Host.CreateDefaultBuilder(args)
        .ConfigureWebHostDefaults(webBuilder =>
        webBuilder.ConfigureAppConfiguration((hostingContext, config) =>
        {
            var settings = config.Build();
            config.AddAzureAppConfiguration(settings["ConnectionStrings:AppConfig"]);
        })
        .UseStartup<Startup>());
    ```

6. Open *index. cshtml* in de weer gaven > basismap en vervang de inhoud door de volgende code:

    ```HTML
    @using Microsoft.Extensions.Configuration
    @inject IConfiguration Configuration

    <style>
        body {
            background-color: @Configuration["TestApp:Settings:BackgroundColor"]
        }
        h1 {
            color: @Configuration["TestApp:Settings:FontColor"];
            font-size: @Configuration["TestApp:Settings:FontSize"];
        }
    </style>

    <h1>@Configuration["TestApp:Settings:Message"]</h1>
    ```

7. Open *_Layout. cshtml* in de weer gaven > gedeelde map en vervang de inhoud door de volgende code:

    ```HTML
    <!DOCTYPE html>
    <html>
    <head>
        <meta charset="utf-8" />
        <meta name="viewport" content="width=device-width, initial-scale=1.0" />
        <title>@ViewData["Title"] - hello_world</title>

        <link rel="stylesheet" href="~/lib/bootstrap/dist/css/bootstrap.css" />
        <link rel="stylesheet" href="~/css/site.css" />
    </head>
    <body>
        <div class="container body-content">
            @RenderBody()
        </div>

        <script src="~/lib/jquery/dist/jquery.js"></script>
        <script src="~/lib/bootstrap/dist/js/bootstrap.js"></script>
        <script src="~/js/site.js" asp-append-version="true"></script>

        @RenderSection("Scripts", required: false)
    </body>
    </html>
    ```

## <a name="build-and-run-the-app-locally"></a>De app lokaal bouwen en uitvoeren

1. Als u de app wilt bouwen met behulp van de .NET Core SLI, voert u de volgende opdracht uit in de opdracht shell:

    ```CLI
       dotnet build
    ```

2. Wanneer de build is voltooid, voert u de volgende opdracht uit om de web-app lokaal uit te voeren:

    ```CLI
        dotnet run
    ```

3. Open een browser venster en ga naar `http://localhost:5000`. Dit is de standaard-URL voor de web-app die lokaal wordt gehost.

## <a name="clean-up-resources"></a>Resources opschonen

[!INCLUDE [azure-app-configuration-cleanup](../../includes/azure-app-configuration-cleanup.md)]

## <a name="next-steps"></a>Volgende stappen

In deze Snelstartgids hebt u een nieuwe app-configuratie opgeslagen gemaakt en gebruikt met een ASP.NET Core-web-app via de [app-configuratie provider](https://go.microsoft.com/fwlink/?linkid=2074664). Ga door naar de volgende zelf studie voor meer informatie over het configureren van uw ASP.NET Core-app om configuratie-instellingen dynamisch te vernieuwen.

> [!div class="nextstepaction"]
> [Dynamische configuratie inschakelen](./enable-dynamic-configuration-aspnet-core.md)
