---
title: Toevoegen of verwijderen van gebruikers - Azure Active Directory | Microsoft Docs
description: Instructies over hoe u nieuwe gebruikers toevoegen of verwijderen van bestaande gebruikers met Azure Active Directory.
services: active-directory
author: msaburnley
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.subservice: fundamentals
ms.topic: conceptual
ms.date: 11/12/2019
ms.author: ajburnle
ms.reviewer: jeffsta
ms.custom: it-pro, seodec18
ms.collection: M365-identity-device-management
ms.openlocfilehash: 3d72616422934501e042375edfb10a25aa27c527
ms.sourcegitcommit: a107430549622028fcd7730db84f61b0064bf52f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/14/2019
ms.locfileid: "74073504"
---
# <a name="add-or-delete-users-using-azure-active-directory"></a>Toevoegen of verwijderen van gebruikers met Azure Active Directory

Voeg nieuwe gebruikers toe of verwijder bestaande gebruikers uit uw Azure Active Directory-organisatie (Azure AD). U moet een gebruikers beheerder of globale beheerder zijn om gebruikers toe te voegen of te verwijderen.

## <a name="add-a-new-user"></a>Een nieuwe gebruiker toevoegen

U kunt een nieuwe gebruiker met behulp van de Azure Active Directory-portal maken.

Voer de volgende stappen uit om een nieuwe gebruiker toe te voegen:

1. Meld u aan bij de [Azure Portal](https://portal.azure.com/) als een gebruikers beheerder voor de organisatie.

1. Zoek en selecteer *Azure Active Directory* op elke pagina.

1. Selecteer **gebruikers**en selecteer vervolgens **nieuwe gebruiker**.

    ![Een gebruiker toevoegen via gebruikers: alle gebruikers in azure AD](media/add-users-azure-active-directory/add-user-in-users-all-users.png)

1. Voer op de pagina **gebruiker** de gegevens voor deze gebruiker in:

   - **Naam**. Vereist. De eerste en laatste naam van de nieuwe gebruiker. Bijvoorbeeld, *Mary Parker*.

   - **Gebruikers naam**. Vereist. De gebruikersnaam van de nieuwe gebruiker. Bijvoorbeeld `mary@contoso.com`.

     Het domein deel van de gebruikers naam moet ofwel de oorspronkelijke standaard domeinnaam, *\<domein naam >. onmicrosoft. com*of een aangepaste domein naam, zoals *contoso.com*, gebruiken. Zie [uw aangepaste domein naam toevoegen met behulp van de Azure Active Directory-Portal](add-custom-domain.md)voor meer informatie over het maken van een aangepaste domein naam.

   - **Groepen**. U kunt eventueel de gebruiker toevoegen aan een of meer bestaande groepen. U kunt ook de gebruiker toevoegen aan groepen op een later tijdstip. Zie [een basis groep maken en leden toevoegen met Azure Active Directory](active-directory-groups-create-azure-portal.md)voor meer informatie over het toevoegen van gebruikers aan groepen.

   - **Directory-rol**: als u Azure AD-beheerders machtigingen voor de gebruiker nodig hebt, kunt u deze toevoegen aan een Azure AD-rol. U kunt de gebruiker toewijzen als globale beheerder of een of meer van de beperkte beheerders rollen in azure AD. Zie voor meer informatie over het toewijzen van rollen [rollen toewijzen aan gebruikers](active-directory-users-assign-role-azure-portal.md).

   - **Taak info**: u kunt hier meer informatie over de gebruiker toevoegen of dit later doen. Zie voor meer informatie over het toevoegen van gebruikersgegevens [toevoegen of wijzigen van de informatie uit gebruikersprofielen](active-directory-users-profile-azure-portal.md).

1. Kopieer het automatisch gegenereerde wacht woord dat is opgegeven in het vak **wacht woord** . U moet dit wacht woord aan de gebruiker geven om zich voor de eerste keer aan te melden.

1. Selecteer **Maken**.

De gebruiker wordt gemaakt en toegevoegd aan uw Azure AD-organisatie.

## <a name="add-a-new-guest-user"></a>Een nieuwe gast gebruiker toevoegen

U kunt ook een nieuwe gast gebruiker uitnodigen om samen te werken met uw organisatie door **gebruikers uitnodigen** te selecteren op de pagina **nieuwe gebruiker** . Als de instellingen voor externe samen werking van uw organisatie zodanig zijn geconfigureerd dat u gasten kunt uitnodigen, wordt de gebruiker een uitnodiging per e-mail verzonden om te beginnen met samen werking. Zie [B2B-gebruikers uitnodigen voor Azure Active Directory](../b2b/add-users-administrator.md) voor meer informatie over het uitnodigen van B2B-samenwerkings gebruikers

## <a name="add-a-consumer-user"></a>Een consument gebruiker toevoegen

Er zijn mogelijk scenario's waarin u hand matig consumenten accounts wilt maken in de map Azure Active Directory B2C (Azure AD B2C). Zie voor meer informatie over het maken van consumenten accounts [consumenten gebruikers maken en verwijderen in azure AD B2C](../../active-directory-b2c/manage-users-portal.md).

## <a name="add-a-new-user-within-a-hybrid-environment"></a>Voeg een nieuwe gebruiker in een hybride omgeving

Als u een omgeving met zowel Azure Active Directory (cloud) en Windows Server Active Directory (on-premises) hebt, kunt u nieuwe gebruikers toevoegen door te synchroniseren van de bestaande gegevens van de gebruiker-account. Zie voor meer informatie over hybride omgevingen en gebruikers [uw on-premises directory's integreren met Azure Active Directory](../hybrid/whatis-hybrid-identity.md).

## <a name="delete-a-user"></a>Een gebruiker verwijderen

U kunt een bestaande gebruiker met behulp van Azure Active Directory-portal verwijderen.

Voer de volgende stappen uit om een gebruiker te verwijderen:

1. Meld u aan bij de [Azure Portal](https://portal.azure.com/) met behulp van een gebruikers beheerders account voor de organisatie.

1. Zoek en selecteer *Azure Active Directory* op elke pagina.

1. Zoek en selecteer de gebruiker die u wilt verwijderen uit uw Azure AD-Tenant. Bijvoorbeeld, _Mary Parker_.

1. Selecteer **gebruiker verwijderen**.

    ![Gebruikers - pagina voor alle gebruikers met gebruiker verwijderen gemarkeerd](media/add-users-azure-active-directory/delete-user-all-users-blade.png)

De gebruiker wordt verwijderd en niet meer wordt weergegeven op de **gebruikers: alle gebruikers** pagina. De gebruiker kan worden weergegeven in de **verwijderde gebruikers** pagina voor de komende 30 dagen en gedurende deze tijd kunnen worden hersteld. Zie [een onlangs verwijderde gebruiker herstellen of verwijderen met Azure Active Directory](active-directory-users-restore.md)voor meer informatie over het herstellen van een gebruiker.

Wanneer een gebruiker wordt verwijderd, worden alle licenties die door de gebruiker worden gebruikt, beschikbaar gesteld voor andere gebruikers.

>[!Note]
>U moet Windows Server Active Directory gebruiken om de identiteit, contact gegevens of taak gegevens bij te werken voor gebruikers waarvan de bron van de autoriteit Windows Server Active Directory is. Nadat u de update hebt voltooid, moet u de volgende synchronisatiecyclus uitvoeren voordat u de wijzigingen ziet wachten.

## <a name="next-steps"></a>Volgende stappen

Nadat u uw gebruikers hebt toegevoegd, kunt u de volgende basis processen uitvoeren:

- [Toevoegen of wijzigen van de profielgegevens](active-directory-users-profile-azure-portal.md)

- [Rollen toewijzen aan gebruikers](active-directory-users-assign-role-azure-portal.md)

- [Een basisgroep maken en leden toevoegen](active-directory-groups-create-azure-portal.md)

- [Werken met dynamische groepen en gebruikers](../users-groups-roles/groups-create-rule.md)

U kunt ook andere gebruikers beheer taken uitvoeren, zoals het [toevoegen van gast gebruikers vanuit een andere map](../b2b/what-is-b2b.md) of [het herstellen van een verwijderde gebruiker](active-directory-users-restore.md). Zie voor meer informatie over andere beschikbare acties [Azure Active Directory management gebruikersdocumentatie](../users-groups-roles/index.yml).
