---
title: API-verificatie en-autorisatie-Azure Time Series Insights | Microsoft Docs
description: In dit artikel wordt beschreven hoe u verificatie en autorisatie configureert voor een aangepaste toepassing die de Azure Time Series Insights-API aanroept.
ms.service: time-series-insights
services: time-series-insights
author: deepakpalled
ms.author: dpalled
manager: cshankar
ms.reviewer: v-mamcge, jasonh, kfile
ms.devlang: csharp
ms.workload: big-data
ms.topic: conceptual
ms.date: 11/14/2019
ms.custom: seodec18
ms.openlocfilehash: d47f846f77d3552288dfea43b417d8c60856f41a
ms.sourcegitcommit: b77e97709663c0c9f84d95c1f0578fcfcb3b2a6c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/22/2019
ms.locfileid: "74327895"
---
# <a name="authentication-and-authorization-for-azure-time-series-insights-api"></a>Verificatie en autorisatie voor Azure Time Series Insights-API

In dit document wordt beschreven hoe u een app in Azure Active Directory kunt registreren met behulp van de nieuwe Azure Active Directory-Blade. Met apps die zijn geregistreerd in Azure Active Directory kunnen gebruikers verifiëren en worden gemachtigd om de Azure time series Insight-API te gebruiken die is gekoppeld aan een Time Series Insights omgeving.

## <a name="service-principal"></a>Service-Principal

In de volgende secties wordt beschreven hoe u een toepassing kunt configureren voor toegang tot de Time Series Insights-API namens een app. De toepassing kan vervolgens met behulp van de referenties van de eigen toepassing een query uitvoeren op of publiceren naar referentie gegevens in de Time Series Insights omgeving via Azure Active Directory.

## <a name="summary-and-best-practices"></a>Samen vatting en aanbevolen procedures

De registratie stroom van de Azure Active Directory-app bestaat uit drie belang rijke stappen.

1. [Een toepassing registreren](#azure-active-directory-app-registration) in azure Active Directory.
1. De toepassing machtigen om [gegevens toegang te geven tot de time series Insights omgeving](#granting-data-access).
1. Gebruik de **toepassings-id** en het **client geheim** om een token te verkrijgen van `https://api.timeseries.azure.com/` in uw [client-app](#client-app-initialization). Het token kan vervolgens worden gebruikt om de Time Series Insights-API aan te roepen.

In het volgende voor **stap 3**kunt u met behulp van de referenties van uw toepassing en uw gebruikers gegevens:

* Wijs machtigingen toe aan de app-identiteit die verschilt van uw eigen machtigingen. Deze machtigingen zijn doorgaans beperkt tot wat de app nodig heeft. U kunt bijvoorbeeld toestaan dat de app alleen gegevens uit een bepaalde Time Series Insights omgeving kan lezen.
* De beveiliging van de app isoleren van de verificatie referenties van de gebruiker met behulp van een **client geheim** of beveiligings certificaat. Als gevolg hiervan zijn de referenties van de toepassing niet afhankelijk van de referenties van een specifieke gebruiker. Als de rol van de gebruiker wordt gewijzigd, is voor de toepassing niet noodzakelijkerwijs nieuwe referenties of verdere configuratie vereist. Als de gebruiker het wacht woord wijzigt, zijn geen nieuwe referenties of sleutels vereist voor toegang tot de toepassing.
* Voer een script zonder toezicht uit met behulp van een **client geheim** of beveiligings certificaat in plaats van een specifieke gebruikers referenties (vereist dat ze aanwezig zijn).
* Gebruik een beveiligings certificaat in plaats van een wacht woord om de toegang tot uw Azure Time Series Insights-API te beveiligen.

> [!IMPORTANT]
> Volg het principe van de **schei ding van de problemen** (beschreven in dit scenario hierboven) bij het configureren van uw Azure time series Insights-beveiligings beleid.

> [!NOTE]
> * Het artikel richt zich op een toepassing met één Tenant waarbij de toepassing in slechts één organisatie is bedoeld om te worden uitgevoerd.
> * Normaal gesp roken gebruikt u toepassingen met één Tenant voor line-of-business-toepassingen die worden uitgevoerd in uw organisatie.

## <a name="detailed-setup"></a>Gedetailleerde installatie

### <a name="azure-active-directory-app-registration"></a>App-registratie Azure Active Directory

[!INCLUDE [Azure Active Directory app registration](../../includes/time-series-insights-aad-registration.md)]

### <a name="granting-data-access"></a>Toegang tot gegevens verlenen

1. Selecteer voor de Time Series Insights omgeving **Data Access policies** en selecteer **toevoegen**.

   [![nieuw beleid voor gegevens toegang toevoegen aan de Time Series Insights omgeving](media/authentication-and-authorization/time-series-insights-data-access-policies-add.png)](media/authentication-and-authorization/time-series-insights-data-access-policies-add.png#lightbox)

1. Plak in het dialoog venster **gebruiker selecteren** de naam van de **toepassing** of de **toepassings-id** in het gedeelte Azure Active Directory app-registratie.

   [![een toepassing zoeken in het dialoog venster gebruiker selecteren](media/authentication-and-authorization/time-series-insights-data-access-policies-select-user.png)](media/authentication-and-authorization/time-series-insights-data-access-policies-select-user.png#lightbox)

1. Selecteer de rol. Selecteer **lezer** om gegevens op te vragen of **Inzender** om gegevens op te vragen en referentie gegevens te wijzigen. Selecteer **OK**.

   [![pick of Inzender in het dialoog venster gebruikersrol selecteren](media/authentication-and-authorization/time-series-insights-data-access-policies-select-role.png)](media/authentication-and-authorization/time-series-insights-data-access-policies-select-role.png#lightbox)

1. Sla het beleid op door **OK**te selecteren.

   > [!TIP]
   > Lees [granting Data Access](./time-series-insights-data-access.md)(geavanceerde opties voor gegevens toegang).

### <a name="client-app-initialization"></a>Initialisatie van client-app

1. Gebruik de **toepassings-id** en het **client geheim** (toepassings sleutel) uit de sectie app-registratie van Azure Active Directory om het token namens de toepassing te verkrijgen.

    In C#kan de volgende code het token namens de toepassing verkrijgen. Zie [query data using C# ](time-series-insights-query-data-csharp.md)(Engelstalig) voor een volledig voor beeld.

    ```csharp
    // Enter your Active Directory tenant domain name
    var tenant = "YOUR_AD_TENANT.onmicrosoft.com";
    var authenticationContext = new AuthenticationContext(
        $"https://login.microsoftonline.com/{tenant}",
        TokenCache.DefaultShared);

    AuthenticationResult token = await authenticationContext.AcquireTokenAsync(
        // Set the resource URI to the Azure Time Series Insights API
        resource: "https://api.timeseries.azure.com/",
        clientCredential: new ClientCredential(
            // Application ID of application registered in Azure Active Directory
            clientId: "YOUR_APPLICATION_ID",
            // Application key of the application that's registered in Azure Active Directory
            clientSecret: "YOUR_CLIENT_APPLICATION_KEY"));

    string accessToken = token.AccessToken;
    ```

1. Het token kan vervolgens worden door gegeven in de `Authorization`-header wanneer de toepassing de Time Series Insights-API aanroept.

## <a name="common-headers-and-parameters"></a>Algemene kopteksten en para meters

In deze sectie worden algemene HTTP-aanvraag headers en-para meters beschreven die worden gebruikt om query's uit te voeren op de Time Series Insights GA en preview-Api's. API-specifieke vereisten worden uitgebreid beschreven in de documentatie over het [Time Series Insights rest API](https://docs.microsoft.com/rest/api/time-series-insights/).

### <a name="authentication"></a>Verificatie

Als u geverifieerde query's wilt uitvoeren op de [Time Series INSIGHTS rest api's](https://docs.microsoft.com/rest/api/time-series-insights/), moet er een geldig OAuth 2,0 Bearer-token worden door gegeven in de [autorisatie-header](/rest/api/apimanagement/2019-01-01/authorizationserver/createorupdate) met behulp van een rest C#-client naar keuze (Postman, java script,). 

> [!IMPORTANT]
> Het token moet exact worden uitgegeven aan de `https://api.timeseries.azure.com/` resource (ook wel bekend als het ' publiek ' van het token).
> * Uw [postman](https://www.getpostman.com/) - **AuthURL** in overeenstemming met: `https://login.microsoftonline.com/microsoft.onmicrosoft.com/oauth2/authorize?resource=https://api.timeseries.azure.com/`

> [!TIP]
> Bekijk de voor [beeld-visualisatie](https://tsiclientsample.azurewebsites.net/) van de gehoste Azure time series Insights client-SDK om te zien hoe u via een programma met de time series Insights api's kunt verifiëren met behulp van de [Java script-client-SDK](https://github.com/microsoft/tsiclient/blob/master/docs/API.md) en grafieken en grafieken.

### <a name="http-headers"></a>HTTP-headers

Vereiste aanvraag headers:

- `Authorization` voor verificatie en autorisatie, moet een geldig OAuth 2,0 Bearer-token worden door gegeven in de autorisatie-header. Het token moet exact worden uitgegeven aan de `https://api.timeseries.azure.com/` resource (ook wel bekend als het ' publiek ' van het token).

Optionele aanvraag headers:

- alleen `Content-type` `application/json` wordt ondersteund.
- `x-ms-client-request-id`-een client aanvraag-ID. Deze waarde wordt door service vastgelegd. Hiermee kan de service bewerkingen volgen tussen services.
- `x-ms-client-session-id`-een client sessie-ID. Deze waarde wordt door service vastgelegd. Hiermee kan de service een groep gerelateerde bewerkingen in verschillende services traceren.
- `x-ms-client-application-name`-naam van de toepassing die deze aanvraag heeft gegenereerd. Deze waarde wordt door service vastgelegd.

Antwoord headers:

- alleen `Content-type` `application/json` wordt ondersteund.
- door de server gegenereerde aanvraag-ID `x-ms-request-id`. Kan worden gebruikt om contact op te nemen met micro soft om een aanvraag te onderzoeken.

### <a name="http-parameters"></a>HTTP-para meters

Vereiste URL-query teken reeks parameters:

- `api-version=2016-12-12`
- `api-version=2018-11-01-preview`

Optionele URL-query teken reeks parameters:

- `timeout=<timeout>`: de time-out aan de server zijde voor het uitvoeren van de aanvraag. Alleen van toepassing op de api's [Get Environment Events](https://docs.microsoft.com/rest/api/time-series-insights/ga-query-api#get-environment-events-api) en [Get Environment aggregaties](https://docs.microsoft.com/rest/api/time-series-insights/ga-query-api#get-environment-aggregates-api) . De time-outwaarde moet de ISO 8601-duur notatie hebben, bijvoorbeeld `"PT20S"` en moet in het bereik `1-30 s`zijn. De standaard waarde is `30 s`.

## <a name="next-steps"></a>Volgende stappen

- Zie [query data using C# ](./time-series-insights-query-data-csharp.md)(Engelstalig) voor voorbeeld code voor het aanroepen van de Ga Time Series Insights-API.

- Zie [query voorbeeld gegevens gebruiken C# ](./time-series-insights-update-query-data-csharp.md)voor preview-time series Insights-API-code voorbeelden.

- Zie [query-API-verwijzing](https://docs.microsoft.com/rest/api/time-series-insights/ga-query-api)voor informatie over API-verwijzingen.

- Meer informatie over het [maken van een Service-Principal](../active-directory/develop/howto-create-service-principal-portal.md).