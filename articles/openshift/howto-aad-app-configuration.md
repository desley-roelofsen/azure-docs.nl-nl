---
title: Azure Active Directory-integratie voor Azure Red Hat OpenShift | Microsoft Docs
description: Informatie over het maken van een Azure AD-beveiligingsgroep en de gebruiker voor het testen van apps op uw Microsoft Azure Red Hat OpenShift-cluster.
author: jimzim
ms.author: jzim
ms.service: container-service
manager: jeconnoc
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 05/13/2019
ms.openlocfilehash: 00609905d09f8d414660c21805c6efca5eb30843
ms.sourcegitcommit: 2e4b99023ecaf2ea3d6d3604da068d04682a8c2d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/09/2019
ms.locfileid: "67669385"
---
# <a name="azure-active-directory-integration-for-azure-red-hat-openshift"></a>Azure Active Directory-integratie voor Azure Red Hat OpenShift

Als u al een tenant Azure Active Directory (Azure AD) dat nog niet hebt gemaakt, volgt u de aanwijzingen in [maken van een Azure AD-tenant voor Azure Red Hat OpenShift](howto-create-tenant.md) voordat u doorgaat met deze instructies.

Microsoft Azure Red Hat OpenShift moet machtigingen voor het uitvoeren van taken namens uw cluster. Als uw organisatie nog niet over een Azure AD-gebruiker, Azure AD-beveiligingsgroep of een Azure AD-app-registratie als de service-principal wilt gebruiken, volgt u deze instructies om ze te maken.

## <a name="create-a-new-azure-active-directory-user"></a>Een nieuwe Azure Active Directory-gebruiker maken

In de [Azure-portal](https://portal.azure.com), zorg ervoor dat uw tenant wordt weergegeven onder de naam van de gebruiker in de rechterbovenhoek van de portal:

![Schermafbeelding van portal met de tenant die worden vermeld in de rechterbovenhoek](./media/howto-create-tenant/tenant-callout.png) als de verkeerde tenant wordt weergegeven, klikt u op de naam van de gebruiker in de rechterbovenhoek en klik vervolgens op **schakelen tussen mappen**, en selecteer de juiste tenant van de **alle Mappen** lijst.

Maak een nieuwe gebruiker van de globale beheerder van Azure Active Directory te melden bij uw Azure Red Hat OpenShift-cluster.

1. Ga naar de [gebruikers alle gebruikers](https://portal.azure.com/#blade/Microsoft_AAD_IAM/UsersManagementMenuBlade/AllUsers) blade.
2. Klik op **+ nieuwe gebruiker** openen de **gebruiker** deelvenster.
3. Voer een **naam** voor deze gebruiker.
4. Maak een **gebruikersnaam** op basis van de naam van de tenant die u hebt gemaakt, met `.onmicrosoft.com` toegevoegd aan het einde. Bijvoorbeeld `yourUserName@yourTenantName.onmicrosoft.com`. Noteer de naam van deze gebruiker. U moet deze zich aanmeldt bij uw cluster.
5. Klik op **maprol** naar het deelvenster van de rol van map openen en selecteer **hoofdbeheerder** en klik vervolgens op **Ok** aan de onderkant van het deelvenster.
6. In de **gebruiker** deelvenster, klikt u op **wachtwoord weergeven** en noteer het tijdelijke wachtwoord. Nadat u de eerste keer aanmelden, wordt u gevraagd opnieuw in te stellen.
7. Klik aan de onderkant van het deelvenster **maken** om de gebruiker te maken.

## <a name="create-an-azure-ad-security-group"></a>Een Azure AD-beveiligingsgroep maken

Cluster-beheerder om toegang te verlenen, worden de lidmaatschappen in een Azure AD-beveiligingsgroep gesynchroniseerd in de OpenShift groep 'osa-klant-admins'. Indien niet opgegeven, wordt er geen toegang tot de beheerder van het cluster worden verleend.

1. Open de [Azure Active Directory-groepen](https://portal.azure.com/#blade/Microsoft_AAD_IAM/GroupsManagementMenuBlade/AllGroups) blade.
2. Klik op **+ nieuwe groep**.
3. Geef een naam en beschrijving.
4. Stel **groepstype** naar **Security**.
5. Stel **lidmaatschapstype** naar **toegewezen**.

    De Azure AD-gebruiker die u hebt gemaakt in de vorige stap aan deze beveiligingsgroep toevoegen.

6. Klik op **leden** openen de **leden selecteren** deelvenster.
7. Selecteer de Azure AD-gebruiker die u hierboven hebt gemaakt in de lijst met leden.
8. Klik aan de onderkant van de portal en op **Selecteer** en vervolgens **maken** om de beveiligingsgroep te maken.

    Noteer de groeps-ID-waarde.

9. Wanneer de groep is gemaakt, ziet u deze in de lijst van alle groepen. Klik op de nieuwe groep.
10. Op de pagina die wordt weergegeven, noteert de **Object-ID**. Er wordt verwezen naar deze waarde als `GROUPID` in de [maken van een cluster Azure Red Hat OpenShift](tutorial-create-cluster.md) zelfstudie.

## <a name="create-an-azure-ad-app-registration"></a>De registratie van een Azure AD-app maken

U kunt automatisch een Azure Active Directory (Azure AD) app-registratie-client als onderdeel van het cluster is gemaakt door weg te laten maken de `--aad-client-app-id` vlag in op de `az openshift create` opdracht. Deze zelfstudie leert u over het maken van de Azure AD-app-registratie voor de volledigheid.

Als uw organisatie niet al een app-registratie voor Azure Active Directory (Azure AD) om te gebruiken als een service-principal hebt, volgt u deze instructies voor het maken van een.

1. Open de [blade App-registraties](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/RegisteredAppsPreview) en klikt u op **+ nieuwe registreren**.
2. In de **registreren van een toepassing** deelvenster, voer een naam voor de registratie van uw toepassing.
3. Zorg ervoor dat onder **ondersteund accounttypen** die **Accounts in deze organisatie-map alleen** is geselecteerd. Dit is de veiligste optie.
4. Zodra we weten de URI van het cluster dat, zullen we later een omleidings-URI toevoegen. Klik op de **registreren** om te maken van de registratie van de Azure AD-toepassing.
5. Op de pagina die wordt weergegeven, noteert de **(client) toepassings-ID**. Er wordt verwezen naar deze waarde als `APPID` in de [maken van een cluster Azure Red Hat OpenShift](tutorial-create-cluster.md) zelfstudie.

![Schermafbeelding van pagina voor app-object](./media/howto-create-tenant/get-app-id.png)

### <a name="create-a-client-secret"></a>Een clientgeheim maken

Genereren van een clientgeheim voor het verifiëren van uw app naar Azure Active Directory.

1. In de **beheren** sectie van de app-registraties pagina, klikt u op **certificaten en geheimen**.
2. Op de **certificaten en geheimen** deelvenster, klikt u op **+ nieuwe clientgeheim**.  De **toevoegen van een clientgeheim** deelvenster wordt weergegeven.
3. Geef een **beschrijving**.
4. Stel **verloopt** aan de duur die u, bijvoorbeeld wilt **na twee jaar**.
5. Klik op **toevoegen** en waarde van de sleutel wordt weergegeven in de **Client geheimen** sectie van de pagina.
6. Noteer de sleutelwaarde. Er wordt verwezen naar deze waarde als `SECRET` in de [maken van een cluster Azure Red Hat OpenShift](tutorial-create-cluster.md) zelfstudie.

![Schermafbeelding van het deelvenster certificaten en geheimen](./media/howto-create-tenant/create-key.png)

Zie voor meer informatie over Azure-toepassingsobjecten [toepassing en service-principalobjecten in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/develop/app-objects-and-service-principals).

Voor meer informatie over het maken van een nieuwe Azure AD-toepassing, Zie [een app registreren met het eindpunt van de Azure Active Directory v1.0](https://docs.microsoft.com/azure/active-directory/develop/quickstart-v1-add-azure-ad-app).

## <a name="add-api-permissions"></a>API-machtigingen toevoegen

1. In de **beheren** sectie Klik **API-machtigingen**.
2. Klik op **machtiging toevoegen** en selecteer **Azure Active Directory Graph** vervolgens **gedelegeerde machtigingen**
3. Vouw **gebruiker** op de onderstaande lijst en zorg ervoor dat **User.Read** is ingeschakeld.
4. Blader omhoog en selecteer **Toepassingsmachtigingen**.
5. Vouw **Directory** op de onderstaande lijst en inschakelen **Directory.ReadAll**
6. Klik op **machtigingen toevoegen** om de wijzigingen te accepteren.
7. Het deelvenster van de machtigingen API moet beide nu weergeven *User.Read* en *Directory.ReadAll*. Houd er rekening mee de waarschuwing in **beheerderstoestemming vereist** kolom naast *Directory.ReadAll*.
8. Als u de *de beheerder van de Azure-abonnement*, klikt u op **beheerder toestemming voor *abonnementsnaam***  hieronder. Als u niet de *de beheerder van de Azure-abonnement*, vragen de toestemming van uw beheerder.
![Schermopname van het paneel API-machtigingen. Machtigingen voor User.Read en Directory.ReadAll toegevoegd, beheerderstoestemming vereist voor Directory.ReadAll](./media/howto-aad-app-configuration/permissions-required.png)

> [!IMPORTANT]
> Synchronisatie van de beheerdersgroep cluster werkt alleen als toestemming heeft gekregen. Ziet u een groene cirkel met een vinkje en een bericht "verleend voor *abonnementsnaam*' in de *beheerderstoestemming vereist* kolom.

Zie voor meer informatie over het beheren van beheerders en andere functies [toevoegen of wijzigen Azure-abonnementbeheerders](https://docs.microsoft.com/azure/billing/billing-add-change-azure-subscription-administrator).

## <a name="resources"></a>Resources

* [Toepassingen en service-principalobjecten in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/develop/app-objects-and-service-principals)
* [Snelstart: Een app registreren met het eindpunt van de Azure Active Directory v1.0](https://docs.microsoft.com/azure/active-directory/develop/quickstart-v1-add-azure-ad-app)

## <a name="next-steps"></a>Volgende stappen

Als u al hebt voldaan de [Azure Red Hat OpenShift vereisten](howto-setup-environment.md), u bent klaar om uw eerste cluster te maken!

Raadpleeg de zelfstudie:
> [!div class="nextstepaction"]
> [Een Azure Red Hat OpenShift-cluster maken](tutorial-create-cluster.md)
