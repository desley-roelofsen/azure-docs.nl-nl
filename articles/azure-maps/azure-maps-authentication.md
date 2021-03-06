---
title: Verificatie met Azure Maps | Microsoft Docs
description: Verificatie voor het gebruik van Azure Maps-services.
author: walsehgal
ms.author: v-musehg
ms.date: 10/24/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: philmea
ms.custom: mvc
ms.openlocfilehash: 84af496a92bd3c7b30062e965335782f7661aa4a
ms.sourcegitcommit: f4d8f4e48c49bd3bc15ee7e5a77bee3164a5ae1b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/04/2019
ms.locfileid: "73575655"
---
# <a name="authentication-with-azure-maps"></a>Verificatie met Azure Maps

Azure Maps ondersteunt twee manieren om aanvragen te verifiëren: gedeelde sleutel en Azure Active Directory (Azure AD). In dit artikel wordt uitgelegd hoe u deze verificatie methoden kunt gebruiken om uw implementatie te begeleiden.

## <a name="shared-key-authentication"></a>Gedeelde sleutel verificatie

Met gedeelde sleutel verificatie worden sleutels door een Azure Maps-account gegenereerd waarbij elke aanvraag wordt Azure Maps.  Wanneer uw Azure Maps-account wordt gemaakt, worden er twee sleutels gegenereerd. Voor elke aanvraag voor het Azure Maps van services moet de abonnements sleutel worden toegevoegd als een para meter voor de URL.

> [!Tip]
> U wordt aangeraden de sleutels regelmatig opnieuw te genereren. U hebt twee sleutels, zodat u verbindingen met één sleutel kunt onderhouden tijdens het opnieuw genereren van de andere. Wanneer u de sleutels opnieuw genereert, moet u alle toepassingen bijwerken die toegang hebben tot het account om de nieuwe sleutels te gebruiken.

Zie [verificatie details weer geven](https://aka.ms/amauthdetails)voor meer informatie over het weer geven van uw sleutels.

## <a name="authentication-with-azure-active-directory-preview"></a>Verificatie met Azure Active Directory (preview-versie)

Azure Maps biedt nu [Azure Active Directory (Azure AD)](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-whatis) integratie voor de verificatie van aanvragen voor Azure Maps Services. Azure AD biedt verificatie op basis van een identiteit, inclusief op [rollen gebaseerd toegangs beheer (RBAC)](https://docs.microsoft.com/azure/role-based-access-control/overview), om toegang op gebruikers niveau toe te kennen voor Azure Maps-resources. In de volgende secties vindt u meer informatie over de concepten en onderdelen van Azure Maps integratie met Azure AD.

## <a name="authentication-with-oauth-access-tokens"></a>Verificatie met OAuth-toegangstokens

Azure Maps accepteert **OAuth 2,0** -toegangs tokens voor Azure AD-tenants die zijn gekoppeld aan een Azure-abonnement dat een Azure Maps account bevat. Azure Maps accepteert tokens voor:

* Azure AD-gebruikers. 
* Partner toepassingen die gebruikmaken van machtigingen die door gebruikers worden gedelegeerd.
* Beheerde identiteiten voor Azure-resources.

Azure Maps genereert een *unieke id (client-id)* voor elk Azure Maps-account. Wanneer u deze client-ID combineert met aanvullende para meters, kunt u tokens aanvragen bij Azure AD door de waarden op te geven in de volgende tabel, afhankelijk van uw Azure-omgeving.

| Azure-omgeving   | Azure AD-token eindpunt |
| --------------------|-------------------------|
| Open bare Azure        | https://login.microsoftonline.com |
| Azure Government    | https://login.microsoftonline.us |


Zie [verificatie beheren in azure Maps](https://docs.microsoft.com/azure/azure-maps/how-to-manage-authentication)voor meer informatie over het configureren van Azure AD en het aanvragen van tokens voor Azure Maps.

Zie [Wat is verificatie?](https://docs.microsoft.com/azure/active-directory/develop/authentication-scenarios)voor algemene informatie over het aanvragen van tokens van Azure AD.

## <a name="request-azure-map-resources-with-oauth-tokens"></a>Azure-toewijzings resources aanvragen met OAuth-tokens

Nadat een token van Azure AD is ontvangen, kan een aanvraag worden verzonden naar Azure Maps met de volgende twee vereiste aanvraag headers zijn ingesteld:

| Aanvraagheader    |    Waarde    |
|:------------------|:------------|
| x-ms-client-id    | 30d7cc….9f55|
| Autorisatie     | Bearer-token eyJ0e….HNIVN |

> [!Note]
> `x-ms-client-id` is de Azure Maps op een account gebaseerde GUID die wordt weer gegeven op de pagina Azure Maps verificatie.

Hier volgt een voor beeld van een Azure Maps route aanvraag die gebruikmaakt van een OAuth-token:

```
GET /route/directions/json?api-version=1.0&query=52.50931,13.42936:52.50274,13.43872 
Host: atlas.microsoft.com 
x-ms-client-id: 30d7cc….9f55 
Authorization: Bearer eyJ0e….HNIVN 
```

Zie [verificatie details weer geven](https://aka.ms/amauthdetails)voor meer informatie over het weer geven van uw client-id.

## <a name="control-access-with-rbac"></a>Toegang beheren met RBAC

Met Azure AD kunt u de toegang tot beveiligde bronnen beheren door gebruik te maken van RBAC. Nadat u uw Azure Maps-account hebt gemaakt en uw Azure Maps Azure AD-toepassing hebt geregistreerd binnen uw Azure AD-Tenant, kunt u RBAC instellen voor een gebruiker, groep, toepassing of Azure-resource op de portal-pagina van de Azure Maps-account.

Azure Maps ondersteunt het lezen van toegangs beheer voor afzonderlijke Azure AD-gebruikers,-groepen,-toepassingen en Azure-Services via beheerde identiteiten voor Azure-resources.

![Azure Maps gegevens lezer (preview-versie)](./media/azure-maps-authentication/concept.png)

Zie voor meer informatie over het weer geven van uw RBAC-instellingen [RBAC configureren voor Azure Maps](https://aka.ms/amrbac).

## <a name="managed-identities-for-azure-resources-and-azure-maps"></a>Beheerde identiteiten voor Azure-resources en-Azure Maps

[Beheerde identiteiten voor Azure-resources](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/overview) bieden Azure-services (Azure App Service, Azure functions, Azure virtual machines, enzovoort) met een automatisch beheerde identiteit die kan worden gemachtigd voor toegang tot Azure Maps Services.  

## <a name="next-steps"></a>Volgende stappen

* Zie [verificatie beheren in azure Maps](https://docs.microsoft.com/azure/azure-maps/how-to-manage-authentication)voor meer informatie over het verifiëren van een toepassing met Azure AD en Azure Maps.

* Zie [de Azure Maps map Control gebruiken](https://aka.ms/amaadmc)voor meer informatie over het verifiëren van de Azure Maps map Control en Azure AD.
