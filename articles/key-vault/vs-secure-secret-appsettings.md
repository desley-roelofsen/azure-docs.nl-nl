---
title: Instellingen voor geheime toepassingen veilig opslaan voor een webtoepassing-Azure Key Vault | Microsoft Docs
description: Veilig opslaan van geheime toepassings instellingen, zoals Azure-referenties of API-sleutels van derden, met behulp van ASP.NET core Key Vault provider, gebruikers geheim of .NET 4.7.1-configuratie bouwers
services: visualstudio
author: cawaMS
manager: paulyuk
editor: ''
ms.service: key-vault
ms.topic: conceptual
ms.date: 07/17/2019
ms.author: cawa
ms.openlocfilehash: d5662fa3cae8ba0cec0fd76965597ccac7c83889
ms.sourcegitcommit: 36e9cbd767b3f12d3524fadc2b50b281458122dc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/20/2019
ms.locfileid: "69639482"
---
# <a name="securely-save-secret-application-settings-for-a-web-application"></a>Instellingen voor geheime toepassingen veilig opslaan voor een webtoepassing

## <a name="overview"></a>Overzicht
In dit artikel wordt beschreven hoe u veilig configuratie-instellingen voor een geheime toepassing opslaat voor Azure-toepassingen.

Doorgaans worden alle configuratie-instellingen van de webtoepassing opgeslagen in configuratie bestanden zoals web. config. Deze praktijk leidt ertoe dat geheime instellingen, zoals Cloud referenties, in de open bare broncode beheer systemen zoals GitHub, worden gecontroleerd. Ondertussen kan het lastig zijn om de beveiliging te volgen best practice vanwege de overhead die nodig is om de bron code te wijzigen en de ontwikkelings instellingen opnieuw te configureren.

Om ervoor te zorgen dat het ontwikkel proces veilig is, worden hulp middelen en Framework bibliotheken gemaakt om de instellingen voor toepassings geheim veilig op te slaan met minimale of geen bron code wijziging.

## <a name="aspnet-and-net-core-applications"></a>ASP.NET-en .NET core-toepassingen

### <a name="save-secret-settings-in-user-secret-store-that-is-outside-of-source-control-folder"></a>Bewaar geheime instellingen in het geheime archief van de gebruiker die zich buiten de map voor broncode beheer bevindt
Als u een snel prototype uitvoert of als u geen toegang hebt tot internet, start u met het verplaatsen van de geheime instellingen buiten de map voor het opslaan van gebruikers naar het geheime archief van de gebruiker. Geheim archief van de gebruiker is een bestand dat is opgeslagen in de map gebruikers Profiler, waardoor geheimen niet zijn ingecheckt bij broncode beheer. In het volgende diagram ziet u hoe [gebruikers geheimen](https://docs.microsoft.com/aspnet/core/security/app-secrets?tabs=visual-studio) werkt.

![Gebruikers geheim houdt geheime instellingen buiten broncode beheer](./media/vs-secure-secret-appsettings/aspnetcore-usersecret.PNG)

Als u een .NET core-console toepassing uitvoert, gebruikt u Key Vault om uw geheim veilig op te slaan.

### <a name="save-secret-settings-in-azure-key-vault"></a>Geheime instellingen in Azure Key Vault opslaan
Als u een project ontwikkelt en de bron code veilig moet delen, gebruikt u [Azure Key Vault](https://azure.microsoft.com/services/key-vault/).

1. Maak een Key Vault in uw Azure-abonnement. Vul alle vereiste velden in de gebruikers interface in en klik onder aan de Blade op *maken* .

    ![Azure Key Vault maken](./media/vs-secure-secret-appsettings/create-keyvault.PNG)

2. Ken u en uw team leden toegang tot de Key Vault. Als u een groot team hebt, kunt u een [Azure Active Directory groep](../active-directory/active-directory-groups-create-azure-portal.md) maken en die beveiligings groep toevoegen aan de Key Vault. Selecteer in de vervolg keuzelijst *geheime machtigingen* de optie *ophalen* en *weer geven* onder *geheime beheer bewerkingen*.

    ![Key Vault toegangs beleid toevoegen](./media/vs-secure-secret-appsettings/add-keyvault-access-policy.png)

3. Voeg uw geheim toe aan Key Vault op de Azure Portal. Voor geneste configuratie-instellingen vervangt u ': ' door '--' zodat de naam van het Key Vault geheim geldig is. ': ' mag niet de naam van een Key Vault geheim hebben.

    ![Key Vault geheim toevoegen](./media/vs-secure-secret-appsettings/add-keyvault-secret.png)

    > [!NOTE] 
    > Voorafgaand aan Visual Studio 2017 V 15,6 is het raadzaam om de Azure Services-verificatie-extensie voor Visual Studio te installeren. Maar het is nu afgeschaft omdat de functionaliteit is geïntegreerd in de Visual Studio. Als u een oudere versie van Visual Studio 2017 gebruikt, wordt u aangeraden om een update uit te brengen naar ten minste VS 2017 15,6 of hoger, zodat u deze functionaliteit systeem eigen kunt gebruiken en toegang krijgt tot de sleutel kluis via de aanmeldings identiteit van Visual Studio zelf.
    >
 
4. Voeg de volgende NuGet-pakketten toe aan uw project:

    ```
    Microsoft.Azure.Services.AppAuthentication
    ```
5. Voeg de volgende code toe aan het Program.cs-bestand:

    ```csharp
    public static IWebHost BuildWebHost(string[] args) =>
        WebHost.CreateDefaultBuilder(args)
            .ConfigureAppConfiguration((ctx, builder) =>
            {
                var keyVaultEndpoint = GetKeyVaultEndpoint();
                if (!string.IsNullOrEmpty(keyVaultEndpoint))
                {
                    var azureServiceTokenProvider = new AzureServiceTokenProvider();
                    var keyVaultClient = new KeyVaultClient(
                        new KeyVaultClient.AuthenticationCallback(
                            azureServiceTokenProvider.KeyVaultTokenCallback));
                            builder.AddAzureKeyVault(
                            keyVaultEndpoint, keyVaultClient, new DefaultKeyVaultSecretManager());
                        }
                    })
                    .UseStartup<Startup>()
                    .Build();

        private static string GetKeyVaultEndpoint() => Environment.GetEnvironmentVariable("KEYVAULT_ENDPOINT");
    ```
6. Voeg uw Key Vault URL toe aan het bestand launchsettings. json. De naam van de omgevings variabele *KEYVAULT_ENDPOINT* wordt gedefinieerd in de code die u in stap 6 hebt toegevoegd.

    ![Key Vault URL toevoegen als een project omgevings variabele](./media/vs-secure-secret-appsettings/add-keyvault-url.png)

7. Start de fout opsporing van het project. Het moet worden uitgevoerd.

## <a name="aspnet-and-net-applications"></a>ASP.NET en .NET-toepassingen

.NET 4.7.1 ondersteunt Key Vault en geheime configuratie bouwers, waardoor geheimen kunnen worden verplaatst buiten de map voor broncode beheer zonder code wijzigingen.
Als u wilt door gaan, kunt u [.net 4.7.1 downloaden](https://www.microsoft.com/download/details.aspx?id=56115) en uw toepassing migreren als deze gebruikmaakt van een oudere versie van .NET Framework.

### <a name="save-secret-settings-in-a-secret-file-that-is-outside-of-source-control-folder"></a>Bewaar geheime instellingen in een geheim bestand dat zich buiten de map voor broncode beheer bevindt
Als u een snel prototype schrijft en Azure-resources niet wilt inrichten, gaat u naar deze optie.

1. Het volgende NuGet-pakket installeren op uw project
    ```
    Microsoft.Configuration.ConfigurationBuilders.Base
    ```

2. Maak een bestand dat er ongeveer als volgt uitziet. Sla het bestand op onder een locatie buiten de projectmap.

    ```xml
    <root>
        <secrets ver="1.0">
            <secret name="secret1" value="foo_one" />
            <secret name="secret2" value="foo_two" />
        </secrets>
    </root>
    ```

3. Definieer het geheime bestand als Configuration builder in het bestand Web. config. Plaats deze sectie voor de sectie *appSettings* .

    ```xml
    <configBuilders>
        <builders>
            <add name="Secrets"
                 secretsFile="C:\Users\AppData\MyWebApplication1\secret.xml" type="Microsoft.Configuration.ConfigurationBuilders.UserSecretsConfigBuilder,
                    Microsoft.Configuration.ConfigurationBuilders, Version=1.0.0.0, Culture=neutral" />
        </builders>
    </configBuilders>
    ```

4. De sectie appSettings opgeven maakt gebruik van de functie voor het maken van geheime configuratie. Zorg ervoor dat er een vermelding voor de geheime instelling met een dummy waarde is.

    ```xml
        <appSettings configBuilders="Secrets">
            <add key="webpages:Version" value="3.0.0.0" />
            <add key="webpages:Enabled" value="false" />
            <add key="ClientValidationEnabled" value="true" />
            <add key="UnobtrusiveJavaScriptEnabled" value="true" />
            <add key="secret" value="" />
        </appSettings>
    ```

5. Fouten opsporen in uw app. Het moet worden uitgevoerd.

### <a name="save-secret-settings-in-an-azure-key-vault"></a>Geheime instellingen in een Azure Key Vault opslaan
Volg de instructies in de sectie ASP.NET-kern om een Key Vault voor uw project te configureren.

1. Het volgende NuGet-pakket installeren op uw project
   ```
   Microsoft.Configuration.ConfigurationBuilders.UserSecrets
   ```

2. Definieer Key Vault Configuration builder in web. config. Plaats deze sectie voor de sectie *appSettings* . Vervang de kluisnaam door de Key Vault naam als uw Key Vault zich in open bare Azure of volledige URI bevindt als u een soevereine Cloud gebruikt.

    ```xml
    <configSections>
        <section name="configBuilders" type="System.Configuration.ConfigurationBuildersSection, System.Configuration, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b03f5f7f11d50a3a" restartOnExternalChanges="false" requirePermission="false" />
    </configSections>
    <configBuilders>
        <builders>
            <add name="AzureKeyVault" vaultName="Test911" type="Microsoft.Configuration.ConfigurationBuilders.AzureKeyVaultConfigBuilder, ConfigurationBuilders, Version=1.0.0.0, Culture=neutral" />
        </builders>
    </configBuilders>
    ```
3. De sectie appSettings opgeven maakt gebruik van de Key Vault Configuration Builder. Zorg ervoor dat er een vermelding voor de geheime instelling met een dummy waarde is.

   ```xml
   <appSettings configBuilders="AzureKeyVault">
       <add key="webpages:Version" value="3.0.0.0" />
       <add key="webpages:Enabled" value="false" />
       <add key="ClientValidationEnabled" value="true" />
       <add key="UnobtrusiveJavaScriptEnabled" value="true" />
       <add key="secret" value="" />
   </appSettings>
   ```

4. Start de fout opsporing van het project. Het moet worden uitgevoerd.
