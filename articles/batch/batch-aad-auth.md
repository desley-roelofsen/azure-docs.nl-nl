---
title: Azure Active Directory gebruiken om Azure Batch-service oplossingen te verifiëren | Microsoft Docs
description: Batch ondersteunt Azure AD voor verificatie vanuit de batch-service.
services: batch
documentationcenter: .net
author: laurenhughes
manager: gwallace
editor: ''
tags: ''
ms.assetid: ''
ms.service: batch
ms.topic: article
ms.tgt_pltfrm: ''
ms.workload: big-compute
ms.date: 08/15/2019
ms.author: lahugh
ms.openlocfilehash: 4ec85078e6664a43dd31cd04c132d87681bda225
ms.sourcegitcommit: 44e85b95baf7dfb9e92fb38f03c2a1bc31765415
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/28/2019
ms.locfileid: "70095626"
---
# <a name="authenticate-batch-service-solutions-with-active-directory"></a>Batch-service oplossingen verifiëren met Active Directory

Azure Batch ondersteunt verificatie met [Azure Active Directory][aad_about] (Azure AD). Azure AD is de multi tenant-Cloud service van micro soft voor het beheren van Directory-en identiteits beheer. Azure zelf gebruikt Azure AD voor de verificatie van klanten, service beheerders en gebruikers van de organisatie.

Wanneer u Azure AD-verificatie gebruikt met Azure Batch, kunt u op een van de volgende twee manieren verifiëren:

- Door **geïntegreerde verificatie** te gebruiken om een gebruiker te verifiëren die met de toepassing communiceert. Een toepassing die gebruikmaakt van geïntegreerde verificatie, verzamelt de referenties van een gebruiker en gebruikt deze referenties om de toegang tot batch-resources te verifiëren.
- Door gebruik te maken van een **Service-Principal** voor het verifiëren van een toepassing zonder toezicht. Een Service-Principal definieert het beleid en de machtigingen voor een toepassing om de toepassing te vertegenwoordigen bij het uitvoeren van toegang tot resources tijdens runtime.

Zie de [Azure Active Directory-documentatie](https://docs.microsoft.com/azure/active-directory/)voor meer informatie over Azure AD.

## <a name="endpoints-for-authentication"></a>Eind punten voor verificatie

Als u batch-toepassingen met Azure AD wilt verifiëren, moet u een aantal bekende eind punten in uw code toevoegen.

### <a name="azure-ad-endpoint"></a>Azure AD-eind punt

Het eind punt van de basis-CA van Azure AD is:

`https://login.microsoftonline.com/`

Voor verificatie met Azure AD gebruikt u dit eind punt samen met de Tenant-ID (Directory-ID). De tenant-ID geeft de Azure AD-tenant te gebruiken voor verificatie. Volg de stappen die worden beschreven in [de Tenant-id ophalen voor uw Azure Active Directory](#get-the-tenant-id-for-your-active-directory)om de Tenant-id op te halen:

`https://login.microsoftonline.com/<tenant-id>`

> [!NOTE] 
> Het Tenant-specifieke eind punt is vereist wanneer u verifieert met behulp van een service-principal. 
> 
> Het Tenant-specifieke eind punt is optioneel wanneer u verifieert met behulp van geïntegreerde verificatie, maar wordt aanbevolen. U kunt echter ook het gemeen schappelijke eind punt van Azure AD gebruiken. Het algemene eind punt biedt een algemene interface voor het verzamelen van referenties wanneer er geen specifieke Tenant is opgegeven. Het algemene eind punt `https://login.microsoftonline.com/common`is.
>
>

Zie [verificatie scenario's voor Azure AD][aad_auth_scenarios]voor meer informatie over Azure AD-eind punten.

### <a name="batch-resource-endpoint"></a>Batch-resource-eind punt

Gebruik het **Azure batch resource-eind punt** om een token te verkrijgen voor het verifiëren van aanvragen voor de batch-service:

`https://batch.core.windows.net/`

## <a name="register-your-application-with-a-tenant"></a>Uw toepassing registreren bij een Tenant

De eerste stap bij het gebruik van Azure AD voor verificatie is het registreren van uw toepassing in een Azure AD-Tenant. Registreren van uw toepassing, kunt u voor het aanroepen van de Azure [Active Directory Authentication Library][aad_adal] (ADAL) vanuit uw code. De ADAL-bibliotheek biedt een API voor verificatie met Azure AD van uw toepassing. Het registreren van uw toepassing is vereist, ongeacht of u een geïntegreerde verificatie of een Service-Principal wilt gebruiken.

Als u uw toepassing registreert, kunt u informatie opgeven over uw toepassing naar Azure AD. Azure AD biedt vervolgens een toepassings-ID (ook wel een *client-id*genoemd) die u gebruikt om uw toepassing te koppelen aan Azure ad tijdens runtime. Zie voor meer informatie over de toepassings-ID [toepassings-en Service-Principal-objecten in azure Active Directory](../active-directory/develop/app-objects-and-service-principals.md).

Volg de stappen in de sectie [een toepassing toevoegen](../active-directory/develop/quickstart-register-app.md) in [toepassingen integreren met Azure Active Directory][aad_integrate]om uw batch-toepassing te registreren. Als u uw toepassing registreert als een systeem eigen toepassing, kunt u een geldige URI voor de omleidings- **URI**opgeven. Het hoeft geen echt eind punt te zijn.

Nadat u uw toepassing hebt geregistreerd, ziet u de toepassings-ID:

![Uw batch-toepassing registreren bij Azure AD](./media/batch-aad-auth/app-registration-data-plane.png)

Zie [verificatie scenario's voor Azure AD](../active-directory/develop/authentication-scenarios.md)voor meer informatie over het registreren van een toepassing met Azure AD.

## <a name="get-the-tenant-id-for-your-active-directory"></a>De Tenant-ID voor uw Active Directory ophalen

Met de Tenant-ID wordt de Azure AD-Tenant geïdentificeerd waarmee verificatie services voor uw toepassing worden geleverd. Als u de tenant-ID, de volgende stappen uit:

1. Selecteer uw Active Directory in de Azure-portal.
1. Selecteer **eigenschappen**.
1. Kopieer de GUID-waarde opgegeven voor de **map-ID**. Deze waarde wordt ook aangeroepen voor de tenant-ID.

![De map-ID kopiëren](./media/batch-aad-auth/aad-directory-id.png)

## <a name="use-integrated-authentication"></a>Geïntegreerde verificatie gebruiken

Als u verificatie met geïntegreerde verificatie wilt uitvoeren, moet u de toepassings machtigingen verlenen om verbinding te maken met de API van de batch-service. Met deze stap kan uw toepassing aanroepen naar de API van de batch-service met Azure AD verifiëren.

Nadat u uw toepassing hebt geregistreerd, voert u de volgende stappen uit in de Azure Portal om deze toegang tot de batch-service te verlenen:

1. Kies in het navigatie deel venster aan de linkerkant van de Azure Portal **alle services**. Selecteer **app**-registraties.
1. Zoek de naam van uw toepassing in de lijst met app-registraties:

    ![Zoek naar de naam van uw toepassing](./media/batch-aad-auth/search-app-registration.png)

1. Selecteer de toepassing en selecteer **API-machtigingen**.
1. Selecteer in de sectie **API-machtigingen** de optie **een machtiging toevoegen**.
1. In **een API selecteren zoekt u**naar de batch-API. Zoek deze tekenreeksen totdat u de API hebt gevonden:
    1. **Microsoft Azure Batch**
    1. **ddbf3205-c6bd-46ae-8127-60eb93363864** is de id voor de Batch-API.
1. Wanneer u de batch-API hebt gevonden, selecteert u deze en selecteert u **selecteren**.
1. In **machtigingen selecteren**selecteert u het selectie vakje naast **toegang tot Azure batch service** en selecteert u vervolgens **machtigingen toevoegen**.

De sectie **API-machtigingen** geeft nu aan dat uw Azure AD-toepassing toegang heeft tot zowel Microsoft Graph als de API van de batch-service. Er worden automatisch machtigingen verleend aan Microsoft Graph wanneer u uw app voor het eerst registreert bij Azure AD.

![API-machtigingen verlenen](./media/batch-aad-auth/required-permissions-data-plane.png)

## <a name="use-a-service-principal"></a>Een Service-Principal gebruiken

Als u een toepassing wilt verifiëren die zonder toezicht wordt uitgevoerd, gebruikt u een service-principal. Nadat u uw toepassing hebt geregistreerd, voert u de volgende stappen uit in de Azure Portal om een service-principal te configureren:

1. Vraag een geheim aan uw toepassing.
1. Wijs op rollen gebaseerd toegangs beheer (RBAC) toe aan uw toepassing.

### <a name="request-a-secret-for-your-application"></a>Een geheim aanvragen voor uw toepassing

Wanneer uw toepassing wordt geverifieerd met een Service-Principal, verzendt deze zowel de toepassings-ID als een geheim naar Azure AD. U moet de geheime sleutel maken en kopiëren voor gebruik vanuit uw code.

Volg deze stappen in de Azure Portal:

1. Kies in het navigatie deel venster aan de linkerkant van de Azure Portal **alle services**. Selecteer **app**-registraties.
1. Selecteer uw toepassing in de lijst met app-registraties.
1. Selecteer de toepassing en selecteer vervolgens **certificaten & geheimen**. Selecteer in de sectie **client geheimen** de optie **Nieuw client geheim**.
1. Als u een geheim wilt maken, voert u een beschrijving in voor het geheim. Selecteer vervolgens een verloop voor het geheim van één jaar, twee jaar of geen verval datum.
1. Selecteer **toevoegen** om het geheim te maken en weer te geven. Kopieer de geheime waarde naar een veilige locatie, omdat u deze niet meer kunt openen nadat u de pagina verlaat.

    ![Een geheime sleutel maken](./media/batch-aad-auth/secret-key.png)

### <a name="assign-rbac-to-your-application"></a>RBAC toewijzen aan uw toepassing

Als u een Service-Principal wilt verifiëren, moet u RBAC toewijzen aan uw toepassing. Volg deze stappen:

1. Navigeer in het Azure Portal naar het batch-account dat door uw toepassing wordt gebruikt.
1. Selecteer **Access Control (IAM)** in het gedeelte **instellingen** van het batch-account.
1. Selecteer het tabblad **roltoewijzingen** .
1. Selecteer **roltoewijzing toevoegen**.
1. Kies in de vervolg keuzelijst **functie** de rol *Inzender* of *lezer* voor uw toepassing. Zie [aan de slag met op rollen gebaseerde Access Control in de Azure Portal](../role-based-access-control/overview.md)voor meer informatie over deze rollen.  
1. Voer in het veld **selecteren** de naam van uw toepassing in. Selecteer uw toepassing in de lijst en selecteer vervolgens **Opslaan**.

Uw toepassing moet nu worden weer gegeven in de instellingen voor toegangs beheer waaraan een RBAC-rol is toegewezen.

![Een RBAC-rol toewijzen aan uw toepassing](./media/batch-aad-auth/app-rbac-role.png)

### <a name="get-the-tenant-id-for-your-azure-active-directory"></a>De tenant-ID niet ophalen voor uw Azure Active Directory

Met de Tenant-ID wordt de Azure AD-Tenant geïdentificeerd waarmee verificatie services voor uw toepassing worden geleverd. Als u de tenant-ID, de volgende stappen uit:

1. Selecteer uw Active Directory in de Azure-portal.
1. Selecteer **eigenschappen**.
1. Kopieer de GUID-waarde opgegeven voor de **map-ID**. Deze waarde wordt ook aangeroepen voor de tenant-ID.

![De map-ID kopiëren](./media/batch-aad-auth/aad-directory-id.png)

## <a name="code-examples"></a>Code voorbeelden

De code voorbeelden in deze sectie laten zien hoe u met Azure AD verifieert met geïntegreerde verificatie en met een service-principal. De meeste van deze code voorbeelden gebruiken .NET, maar de concepten zijn vergelijkbaar voor andere talen.

> [!NOTE]
> Een Azure AD-verificatie token verloopt na één uur. Wanneer u een lang bewaard **BatchClient** -object gebruikt, wordt u aangeraden een token op te halen uit ADAL op elke aanvraag om ervoor te zorgen dat u altijd een geldig token hebt. 
>
>
> Als u dit wilt doen in .NET, schrijft u een methode die het token uit Azure AD ophaalt en geeft u deze methode door aan een **BatchTokenCredentials** -object als gemachtigde. De gemachtigde methode wordt aangeroepen voor elke aanvraag bij de batch-service om ervoor te zorgen dat er een geldig token wordt gegeven. ADAL-tokens worden standaard in de cache opgeslagen, zodat een nieuw token alleen wordt opgehaald uit Azure AD als dat nodig is. Zie [verificatie scenario's voor Azure AD][aad_auth_scenarios]voor meer informatie over tokens in azure AD.
>
>

### <a name="code-example-using-azure-ad-integrated-authentication-with-batch-net"></a>Voorbeeld van code: Geïntegreerde Azure AD-verificatie met batch .NET gebruiken

Als u wilt verifiëren met geïntegreerde verificatie vanuit batch .NET, verwijst u naar het [Azure batch .net](https://www.nuget.org/packages/Microsoft.Azure.Batch/) -pakket en het [ADAL](https://www.nuget.org/packages/Microsoft.IdentityModel.Clients.ActiveDirectory/) -pakket.

Neem de volgende `using` instructies op in uw code:

```csharp
using Microsoft.Azure.Batch;
using Microsoft.Azure.Batch.Auth;
using Microsoft.IdentityModel.Clients.ActiveDirectory;
```

Raadpleeg het Azure AD-eind punt in uw code, inclusief de Tenant-ID. Volg de stappen die worden beschreven in [de Tenant-id ophalen voor uw Azure Active Directory](#get-the-tenant-id-for-your-active-directory)om de Tenant-id op te halen:

```csharp
private const string AuthorityUri = "https://login.microsoftonline.com/<tenant-id>";
```

Naslag het eind punt voor de batch-Service Resource:

```csharp
private const string BatchResourceUri = "https://batch.core.windows.net/";
```

Referentie voor uw batch-account:

```csharp
private const string BatchAccountUrl = "https://myaccount.mylocation.batch.azure.com";
```

Geef de toepassings-ID (client-ID) op voor uw toepassing. De toepassings-ID is beschikbaar via de registratie van uw app in de Azure Portal:

```csharp
private const string ClientId = "<application-id>";
```

Kopieer ook de omleidings-URI die u hebt opgegeven als u uw toepassing hebt geregistreerd als een systeem eigen toepassing. De omleidings-URI die is opgegeven in de code moet overeenkomen met de omleidings-URI die u hebt opgegeven tijdens het registreren van de toepassing:

```csharp
private const string RedirectUri = "http://mybatchdatasample";
```

Schrijf een call back-methode om het verificatie token van Azure AD te verkrijgen. De **GetAuthenticationTokenAsync** call back methode die hier wordt weer gegeven, roept ADAL aan om een gebruiker te verifiëren die met de toepassing werkt. De **AcquireTokenAsync** -methode die door ADAL wordt verstrekt, vraagt de gebruiker om zijn referenties en de toepassing wordt uitgevoerd zodra de gebruiker deze heeft verstrekt (tenzij de referenties al in de cache zijn opgeslagen):

```csharp
public static async Task<string> GetAuthenticationTokenAsync()
{
    var authContext = new AuthenticationContext(AuthorityUri);

    // Acquire the authentication token from Azure AD.
    var authResult = await authContext.AcquireTokenAsync(BatchResourceUri, 
                                                        ClientId, 
                                                        new Uri(RedirectUri), 
                                                        new PlatformParameters(PromptBehavior.Auto));

    return authResult.AccessToken;
}
```

Maak een **BatchTokenCredentials** -object dat de gemachtigde als para meter gebruikt. Gebruik deze referenties om een **BatchClient** -object te openen. U kunt dat **BatchClient** -object gebruiken voor volgende bewerkingen voor de batch-service:

```csharp
public static async Task PerformBatchOperations()
{
    Func<Task<string>> tokenProvider = () => GetAuthenticationTokenAsync();

    using (var client = await BatchClient.OpenAsync(new BatchTokenCredentials(BatchAccountUrl, tokenProvider)))
    {
        await client.JobOperations.ListJobs().ToListAsync();
    }
}
```

### <a name="code-example-using-an-azure-ad-service-principal-with-batch-net"></a>Voorbeeld van code: Een Azure AD-Service-Principal gebruiken met batch .NET

Als u wilt verifiëren met een Service-Principal vanuit batch .NET, verwijst u naar het [Azure batch .net](https://www.nuget.org/packages/Azure.Batch/) -pakket en het [ADAL](https://www.nuget.org/packages/Microsoft.IdentityModel.Clients.ActiveDirectory/) -pakket.

Neem de volgende `using` instructies op in uw code:

```csharp
using Microsoft.Azure.Batch;
using Microsoft.Azure.Batch.Auth;
using Microsoft.IdentityModel.Clients.ActiveDirectory;
```

Raadpleeg het Azure AD-eind punt in uw code, inclusief de Tenant-ID. Wanneer u een Service-Principal gebruikt, moet u een Tenant-specifiek eind punt opgeven. Volg de stappen die worden beschreven in [de Tenant-id ophalen voor uw Azure Active Directory](#get-the-tenant-id-for-your-active-directory)om de Tenant-id op te halen:

```csharp
private const string AuthorityUri = "https://login.microsoftonline.com/<tenant-id>";
```

Naslag het eind punt voor de batch-Service Resource:  

```csharp
private const string BatchResourceUri = "https://batch.core.windows.net/";
```

Referentie voor uw batch-account:

```csharp
private const string BatchAccountUrl = "https://myaccount.mylocation.batch.azure.com";
```

Geef de toepassings-ID (client-ID) op voor uw toepassing. De toepassings-ID is beschikbaar via de registratie van uw app in de Azure Portal:

```csharp
private const string ClientId = "<application-id>";
```

Geef de geheime sleutel op die u hebt gekopieerd uit de Azure Portal:

```csharp
private const string ClientKey = "<secret-key>";
```

Schrijf een call back-methode om het verificatie token van Azure AD te verkrijgen. De **GetAuthenticationTokenAsync** call back methode die hier wordt weer gegeven, roept ADAL aan voor verificatie zonder toezicht:

```csharp
public static async Task<string> GetAuthenticationTokenAsync()
{
    AuthenticationContext authContext = new AuthenticationContext(AuthorityUri);
    AuthenticationResult authResult = await authContext.AcquireTokenAsync(BatchResourceUri, new ClientCredential(ClientId, ClientKey));

    return authResult.AccessToken;
}
```

Maak een **BatchTokenCredentials** -object dat de gemachtigde als para meter gebruikt. Gebruik deze referenties om een **BatchClient** -object te openen. Gebruik vervolgens dat **BatchClient** -object voor volgende bewerkingen voor de batch-service:

```csharp
public static async Task PerformBatchOperations()
{
    Func<Task<string>> tokenProvider = () => GetAuthenticationTokenAsync();

    using (var client = await BatchClient.OpenAsync(new BatchTokenCredentials(BatchAccountUrl, tokenProvider)))
    {
        await client.JobOperations.ListJobs().ToListAsync();
    }
}
```

### <a name="code-example-using-an-azure-ad-service-principal-with-batch-python"></a>Voorbeeld van code: Een Azure AD-Service-Principal gebruiken met batch python

Als u wilt verifiëren met een Service-Principal vanuit batch python, installeert en raadpleegt u de [Azure-batch](https://pypi.org/project/azure-batch/) -en [Azure-algemene](https://pypi.org/project/azure-common/) modules.

```python
from azure.batch import BatchServiceClient
from azure.common.credentials import ServicePrincipalCredentials
```

Wanneer u een Service-Principal gebruikt, moet u de Tenant-ID opgeven. Volg de stappen die worden beschreven in [de Tenant-id ophalen voor uw Azure Active Directory](#get-the-tenant-id-for-your-active-directory)om de Tenant-id op te halen:

```python
TENANT_ID = "<tenant-id>"
```

Naslag het eind punt voor de batch-Service Resource:  

```python
RESOURCE = "https://batch.core.windows.net/"
```

Referentie voor uw batch-account:

```python
BATCH_ACCOUNT_URL = "https://myaccount.mylocation.batch.azure.com"
```

Geef de toepassings-ID (client-ID) op voor uw toepassing. De toepassings-ID is beschikbaar via de registratie van uw app in de Azure Portal:

```python
CLIENT_ID = "<application-id>"
```

Geef de geheime sleutel op die u hebt gekopieerd uit de Azure Portal:

```python
SECRET = "<secret-key>"
```

Een **ServicePrincipalCredentials** -object maken:

```python
credentials = ServicePrincipalCredentials(
    client_id=CLIENT_ID,
    secret=SECRET,
    tenant=TENANT_ID,
    resource=RESOURCE
)
```

Gebruik de referenties van de Service-Principal om een **BatchServiceClient** -object te openen. Gebruik vervolgens dat **BatchServiceClient** -object voor volgende bewerkingen voor de batch-service.

```python
    batch_client = BatchServiceClient(
    credentials,
    base_url=BATCH_ACCOUNT_URL
)
```

## <a name="next-steps"></a>Volgende stappen

- Zie de [Azure Active Directory-documentatie](https://docs.microsoft.com/azure/active-directory/)voor meer informatie over Azure AD. Gedetailleerde voor beelden waarin wordt getoond hoe u ADAL kunt gebruiken, zijn beschikbaar in de [Azure code samples](https://azure.microsoft.com/resources/samples/?service=active-directory) -bibliotheek.

- Zie [toepassings-en Service-Principal-objecten in azure Active Directory](../active-directory/develop/app-objects-and-service-principals.md)voor meer informatie over service-principals. Als u een Service-Principal wilt maken met behulp van de Azure Portal, raadpleegt u [Portal gebruiken om Active Directory toepassing en Service-Principal te maken die toegang hebben tot resources](../active-directory/develop/howto-create-service-principal-portal.md). U kunt ook een service-principal maken met Power shell of Azure CLI.

- Zie [oplossingen voor Batch Management verifiëren met Active Directory](batch-aad-auth-management.md)voor het verifiëren van toepassingen voor batch beheer met Azure AD.

- Voor een python-voor beeld van het maken van een batch-client die is geverifieerd met behulp van een Azure AD-token, zie de [implementatie Azure batch aangepaste installatie kopie met een python](https://github.com/azurebigcompute/recipes/blob/master/Azure%20Batch/CustomImages/CustomImagePython.md) -voorbeeld script.

[aad_about]:../active-directory/fundamentals/active-directory-whatis.md "Wat is Azure Active Directory?"
[aad_adal]: ../active-directory/active-directory-authentication-libraries.md
[aad_auth_scenarios]: ../active-directory/active-directory-authentication-scenarios.md "Verificatie Scenario's voor Azure AD"
[aad_integrate]: ../active-directory/active-directory-integrating-applications.md "Toepassingen integreren met Azure Active Directory"
[azure_portal]: https://portal.azure.com
