---
title: Facturerings model voor Azure Active Directory B2C
description: Meer informatie over het facturerings model voor het maandelijkse actieve gebruikers (MAU) van Azure AD B2C's en het inschakelen van facturering voor een specifiek Azure-abonnement.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.date: 10/25/2019
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: 844b62f9575249c7b99672e9e67c94cea7ec9f99
ms.sourcegitcommit: 4c3d6c2657ae714f4a042f2c078cf1b0ad20b3a4
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/25/2019
ms.locfileid: "72931417"
---
# <a name="billing-model-for-azure-active-directory-b2c"></a>Facturerings model voor Azure Active Directory B2C

Het gebruik van Azure Active Directory B2C (Azure AD B2C) wordt gefactureerd aan een gekoppeld Azure-abonnement en maakt gebruik van een maandelijks MAU-facturerings model (actieve gebruikers). Leer hoe u een Azure AD B2C resource koppelt aan een abonnement en hoe het facturerings model van MAU werkt in de volgende secties.

> [!IMPORTANT]
> Dit artikel bevat geen prijs informatie. Zie [Azure Active Directory B2C prijzen](https://azure.microsoft.com/pricing/details/active-directory-b2c/)voor de meest recente informatie over het gebruik van facturering en prijzen.

## <a name="monthly-active-users-mau-billing"></a>Facturering van maandelijkse actieve gebruikers (MAU)

Azure AD B2C facturering wordt gemeten op basis van het aantal unieke gebruikers met verificatie activiteiten binnen een kalender maand, ook wel maandelijks actieve gebruikers (MAU)-facturering.

Vanaf **01 November 2019**worden alle nieuw gemaakte Azure AD B2C tenants gefactureerd per maandelijks actieve gebruikers (Mau). Bestaande tenants die zijn [gekoppeld aan een abonnement](#link-an-azure-ad-b2c-tenant-to-a-subscription) op of na 01 november 2019 worden gefactureerd per maandelijks actieve gebruikers (Mau).

Als u een bestaande Azure AD B2C-Tenant hebt die is gekoppeld aan een abonnement van vóór 01 november 2019, kunt u kiezen uit een van de volgende opties:

* Voer een upgrade uit naar het maandelijkse actieve gebruikers (MAU) facturerings model of
* Blijf op het facturerings model per verificatie

### <a name="upgrade-to-monthly-active-users-billing-model"></a>Upgrade uitvoeren naar het facturerings model voor maandelijkse actieve gebruikers

Eigen aars van Azure-abonnementen met beheerders toegang tot de Azure AD B2C resource kunnen overschakelen naar het facturerings model MAU. Facturerings opties worden geconfigureerd in uw Azure AD B2C-resource.

De schakel optie voor facturering van maandelijkse actieve gebruikers (MAU) is **onomkeerbaar**. Nadat u een Azure AD B2C resource hebt geconverteerd naar het MAU-factuur model, kunt u die resource niet herstellen naar het facturerings model per verificatie.

Ga als volgt te werk om de overschakeling naar MAU facturering voor een bestaande Azure AD B2C resource te maken:

1. Meld u aan bij de [Azure Portal](https://portal.azure.com) als eigenaar van het abonnement.
1. Selecteer het filter **Directory + abonnement** in het bovenste menu en selecteer vervolgens de Azure AD B2C Directory die u wilt bijwerken naar Mau-facturering.<br/>
    ![Directory-en abonnements filter in Azure Portal](media/active-directory-b2c-how-to-enable-billing/portal-mau-01-select-b2c-directory.png)
1. Selecteer in het linkermenu **Azure AD B2C**. U kunt ook **alle services** selecteren en **Azure AD B2C**zoeken en selecteren.
1. Selecteer op de pagina **overzicht** van de Azure AD B2C Tenant de koppeling onder **resource naam**. U wordt omgeleid naar de Azure AD B2C-resource in uw Azure AD-Tenant.<br/>
    ![Azure AD B2C resource koppeling gemarkeerd in Azure Portal](media/active-directory-b2c-how-to-enable-billing/portal-mau-02-b2c-resource-link.png)
1. Selecteer op de pagina **overzicht** van de Azure AD B2C resource onder **factureer bare eenheden**de koppeling **per verificatie (wijzigen in Mau)** .<br/>
    ![gewijzigd in de MAU-koppeling is gemarkeerd in Azure Portal](media/active-directory-b2c-how-to-enable-billing/portal-mau-03-change-to-mau-link.png)
1. Selecteer **bevestigen** om de upgrade naar Mau-facturering te volt ooien.<br/>
    ![dialoog venster voor facturerings bevestiging op basis van MAU in Azure Portal](media/active-directory-b2c-how-to-enable-billing/portal-mau-04-confirm-change-to-mau.png)

### <a name="what-to-expect-when-you-transition-to-mau-billing-from-per-authentication-billing"></a>Wat u kunt verwachten wanneer u overstapt naar MAU-facturering vanuit facturering per verificatie

MAU Metering is ingeschakeld zodra u, de eigenaar van het abonnement/de resource, de wijziging bevestigt. Op uw maandelijkse factuur worden de gefactureerde verificatie-eenheden weer gegeven totdat de wijziging en de nieuwe eenheden van MAU beginnen met de wijziging.

Gebruikers worden niet dubbel geteld tijdens de overgangs maand. Unieke actieve gebruikers die zich vóór de wijziging verifiëren, worden in rekening gebracht per verificatie tarief in een kalender maand. Dezelfde gebruikers worden niet opgenomen in de berekening van de MAU voor de rest van de facturerings cyclus van het abonnement. Bijvoorbeeld:

* De contoso B2C-Tenant heeft 1.000 gebruikers. 250 gebruikers zijn in een bepaalde maand actief. De abonnements beheerder verandert van per verificatie voor maandelijkse actieve gebruikers (MAU) op de tiende van de maand.
* Facturering voor 1e tiende wordt gefactureerd met het model per verificatie.
  * Als 100 gebruikers zich aanmelden tijdens deze periode (1 tot en met 10), worden deze gebruikers gemarkeerd als *betaald voor de maand*.
* Facturering van de tiende (de geldigheids duur van de overgang) wordt gefactureerd op basis van het MAU tarief.
  * Als een aanvullend 150-gebruiker zich tijdens deze periode (tiende-30) aanmeldt, wordt alleen de extra 150 gefactureerd.
  * De continue activiteit van de eerste 100-gebruikers heeft geen invloed op de facturering voor de rest van de kalender maand.

Tijdens de facturerings periode van de overgang ziet de eigenaar van het abonnement waarschijnlijk vermeldingen voor beide methoden (per verificatie en per MAU) in hun facturerings overzicht van het Azure-abonnement:

* Een vermelding voor het gebruik tot de datum/tijd van de wijziging die per verificatie weerspiegelt.
* Een vermelding voor het gebruik na de wijziging die maandelijkse actieve gebruikers (MAU) weergeeft.

Zie [Azure Active Directory B2C prijzen](https://azure.microsoft.com/pricing/details/active-directory-b2c/)voor de meest recente informatie over het factureren van gebruik en de prijzen voor Azure AD B2C.

## <a name="link-an-azure-ad-b2c-tenant-to-a-subscription"></a>Een Azure AD B2C-Tenant koppelen aan een abonnement

Gebruiks kosten voor Azure Active Directory B2C (Azure AD B2C) worden gefactureerd voor een Azure-abonnement. Wanneer een Azure AD B2C-Tenant wordt gemaakt, moet de Tenant beheerder de Azure AD B2C Tenant expliciet koppelen aan een Azure-abonnement.

De koppeling van het abonnement wordt bereikt door een Azure AD B2C *resource* te maken binnen het doel-Azure-abonnement. Verschillende Azure AD B2C resources kunnen worden gemaakt in één Azure-abonnement, samen met andere Azure-resources, zoals virtuele machines, opslag accounts en Logic Apps. U kunt alle resources in een abonnement zien door te gaan naar de Azure Active Directory (Azure AD)-Tenant waaraan het abonnement is gekoppeld.

Een abonnement dat is gekoppeld aan een Azure AD B2C-Tenant kan worden gebruikt voor het factureren van Azure AD B2C gebruik of andere Azure-resources, inclusief aanvullende Azure AD B2C resources. Het kan niet worden gebruikt voor het toevoegen van andere Azure-licentie services of Office 365-licenties in de Azure AD B2C Tenant.

### <a name="prerequisites"></a>Vereisten

* [Azure-abonnement](https://azure.microsoft.com/free/)
* [Azure AD B2C Tenant](active-directory-b2c-get-started.md) die u wilt koppelen aan een abonnement
  * U moet een Tenant beheerder zijn
  * De Tenant mag niet al zijn gekoppeld aan een abonnement

### <a name="create-the-link"></a>De koppeling maken

1. Meld u aan bij de [Azure-portal](https://portal.azure.com).
1. Selecteer het filter **Directory + abonnement** in het bovenste menu en selecteer vervolgens de map met het Azure-abonnement dat u wilt gebruiken (*niet* de map die de Azure AD B2C Tenant bevat).
1. Selecteer **een resource maken**, voer `Active Directory B2C` in het veld **Marketplace doorzoeken** in en selecteer vervolgens **Azure Active Directory B2C**.
1. Selecteer **Maken**
1. Selecteer **een bestaande Azure AD B2C-Tenant koppelen aan mijn Azure-abonnement**.
1. Selecteer een **Azure AD B2C Tenant** in de vervolg keuzelijst. Alleen tenants waarvoor u een globale beheerder bent en die nog niet aan een abonnement zijn gekoppeld, worden weer gegeven. Het veld **Azure AD B2C resource naam** wordt ingevuld met de domein naam van de Azure AD B2C-Tenant die u selecteert.
1. Selecteer een actief Azure- **abonnement** waarvan u een beheerder bent.
1. Selecteer onder **resource groep**de optie **nieuwe maken**en geef vervolgens de **locatie van de resource groep**op. De instellingen voor de resource groep zijn hier niet van invloed op uw Azure AD B2C Tenant locatie, prestaties of facturerings status.
1. Selecteer **Maken**.
    ![de pagina voor het maken van Azure AD B2C resources in Azure Portal](./media/active-directory-b2c-how-to-enable-billing/portal-01-create-b2c-resource-page.png)

Nadat u deze stappen voor een Azure AD B2C Tenant hebt voltooid, wordt uw Azure-abonnement gefactureerd volgens uw Azure direct-of Enterprise Agreement-gegevens, indien van toepassing.

### <a name="manage-your-azure-ad-b2c-tenant-resources"></a>Uw Azure AD B2C-Tenant resources beheren

Nadat u de Azure AD B2C resource in een Azure-abonnement hebt gemaakt, ziet u een nieuwe resource van het type ' B2C-Tenant ' weer gegeven met uw andere Azure-resources.

U kunt deze resource gebruiken voor het volgende:

* Navigeer naar het abonnement om facturerings gegevens te bekijken
* De Tenant-ID van de Azure AD B2C Tenant in GUID-indeling ophalen
* Ga naar uw Azure AD B2C-Tenant
* Een ondersteuningsaanvraag indienen
* Uw Azure AD B2C Tenant resource verplaatsen naar een ander Azure-abonnement of een andere resource groep

![De pagina B2C-bron instellingen in de Azure Portal](./media/active-directory-b2c-how-to-enable-billing/portal-02-b2c-resource-overview.png)

### <a name="regional-restrictions"></a>Regionale beperkingen

Als u regionale beperkingen hebt ingesteld voor het maken van Azure-resources in uw abonnement, kan deze beperking ertoe leiden dat u de Azure AD B2C resource niet kunt maken.

Om dit probleem te verhelpen, kunt u uw regionale beperkingen versoepelen.

## <a name="azure-cloud-solution-providers-csp-subscriptions"></a>Azure Cloud Solution Providers (CSP)-abonnementen

Azure Cloud Solution Providers (CSP)-abonnementen worden ondersteund in Azure AD B2C. De functionaliteit is beschikbaar via Api's of de Azure Portal voor Azure AD B2C en voor alle Azure-resources. Beheerders van CSP-abonnementen kunnen relaties met Azure AD B2C koppelen, verplaatsen en verwijderen, net als bij andere Azure-resources.

Het beheer van Azure AD B2C met op rollen gebaseerd toegangs beheer wordt niet beïnvloed door de koppeling tussen de Azure AD B2C Tenant en een Azure CSP-abonnement. Toegangs beheer op basis van rollen wordt bereikt door gebruik te maken van op tenants gebaseerde rollen, niet op abonnementen gebaseerde rollen.

## <a name="change-the-azure-ad-b2c-tenant-billing-subscription"></a>Het facturerings abonnement voor de Azure AD B2C-Tenant wijzigen

Azure AD B2C-tenants kunnen worden verplaatst naar een ander abonnement als de bron-en doel abonnementen binnen dezelfde Azure Active Directory Tenant bestaan.

Zie [resources verplaatsen naar een nieuwe resource groep of een nieuw abonnement](../azure-resource-manager/resource-group-move-resources.md)voor meer informatie over het verplaatsen van Azure-resources, zoals uw Azure AD B2C-Tenant naar een ander abonnement.

Lees voordat u begint met verplaatsen het hele artikel om de beperkingen en vereisten voor een dergelijke verplaatsing volledig te begrijpen. Naast instructies voor het verplaatsen van resources bevat deze essentiële informatie zoals een controle lijst voorafgaand aan het verplaatsen en het valideren van de verplaatsings bewerking.

## <a name="next-steps"></a>Volgende stappen

Naast het controleren van het gebruik en de facturerings gegevens binnen een geselecteerd Azure-abonnement, kunt u gedetailleerde dagelijkse gebruiks rapporten bekijken met de API voor [gebruiks rapportage](active-directory-b2c-reference-usage-reporting-api.md).
