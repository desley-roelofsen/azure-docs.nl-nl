---
title: Roltoewijzingen weer geven met behulp van Azure RBAC en de Azure Portal
description: Meer informatie over hoe u kunt bepalen welke resources gebruikers, groepen, service-principals of beheerde identiteiten hebben toegang tot het gebruik van op rollen gebaseerd toegangs beheer (RBAC) en de Azure Portal.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.assetid: 8078f366-a2c4-4fbb-a44b-fc39fd89df81
ms.service: role-based-access-control
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 11/25/2019
ms.author: rolyon
ms.reviewer: bagovind
ms.openlocfilehash: 25e11ae1311df9d0392340b32e0691298f78ee1c
ms.sourcegitcommit: c69c8c5c783db26c19e885f10b94d77ad625d8b4
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/03/2019
ms.locfileid: "74710423"
---
# <a name="list-role-assignments-using-azure-rbac-and-the-azure-portal"></a>Roltoewijzingen weer geven met behulp van Azure RBAC en de Azure Portal

[!INCLUDE [Azure RBAC definition list access](../../includes/role-based-access-control-definition-list.md)] in dit artikel wordt beschreven hoe u roltoewijzingen kunt weer geven met behulp van de Azure Portal.

## <a name="list-role-assignments-for-a-user-or-group"></a>Roltoewijzingen voor een gebruiker of groep weer geven

De eenvoudigste manier om de rollen weer te geven die zijn toegewezen aan een gebruiker of groep in een abonnement is het gebruik van het deel venster **Azure-resources** .

1. Klik in de Azure Portal op **alle services** en selecteer vervolgens **gebruikers** of **groepen**.

1. Klik op de gebruiker of groep waarvoor u de roltoewijzingen wilt weer geven.

1. Klik op **Azure-resources**.

    U ziet een lijst met de rollen die zijn toegewezen aan de geselecteerde gebruiker of groep in verschillende bereiken, zoals de beheer groep, het abonnement, de resource groep of de resource. Deze lijst bevat alle roltoewijzingen waarvoor u lees machtigingen hebt.

    ![Roltoewijzingen voor een gebruiker](./media/role-assignments-list-portal/azure-resources-user.png)    

1. Als u het abonnement wilt wijzigen, klikt u op de lijst **abonnementen** .

## <a name="list-role-assignments-at-a-scope"></a>Roltoewijzingen in een bereik weer geven

1. Klik in de Azure Portal op **alle services** en selecteer vervolgens het bereik. U kunt bijvoorbeeld **beheer groepen**, **abonnementen**, **resource groepen**of een resource selecteren.

1. Klik op de specifieke resource.

1. Klik op **Toegangsbeheer (IAM)** .

1. Klik op **het tabblad roltoewijzingen om** alle roltoewijzingen in dit bereik weer te geven.

   ![Toegangs beheer-tabblad roltoewijzingen](./media/role-assignments-list-portal/access-control-role-assignments.png)

   Op het tabblad roltoewijzingen kunt u zien wie toegang heeft tot dit bereik. U ziet dat voor sommige rollen het bereik is ingesteld op **Deze resource**, terwijl andere zijn **(Overgenomen)** uit een ander bereik. De toegang wordt specifiek aan deze resource toegewezen of overgenomen van een toewijzing aan het bovenliggende bereik.

## <a name="list-role-assignments-for-a-user-at-a-scope"></a>Roltoewijzingen voor een gebruiker in een bereik weer geven

Als u de toegang wilt weer geven voor een gebruiker, groep, Service-Principal of beheerde identiteit, vermeldt u hun roltoewijzingen. Volg deze stappen om de roltoewijzingen voor één gebruiker, groep, Service-Principal of beheerde identiteit in een bepaald bereik weer te geven.

1. Klik in de Azure Portal op **alle services** en selecteer vervolgens het bereik. U kunt bijvoorbeeld **beheer groepen**, **abonnementen**, **resource groepen**of een resource selecteren.

1. Klik op de specifieke resource.

1. Klik op **Toegangsbeheer (IAM)** .

1. Klik op het tabblad **Toegang controleren**.

    ![Toegangsbeheer - Tabblad Toegang controleren](./media/role-assignments-list-portal/access-control-check-access.png)

1. Selecteer in de lijst **Zoeken** het type beveiligings-principal waarvoor u de toegang wilt controleren.

1. Voer in het zoekvak een tekenreeks in om de map te doorzoeken op weergavenamen, e-mailadressen of object-id's.

    ![De toegangsselectielijst controleren](./media/role-assignments-list-portal/check-access-select.png)

1. Klik op de beveiligings-principal om het deelvenster **Toewijzingen** te openen.

    ![Deelvenster Toewijzingen](./media/role-assignments-list-portal/check-access-assignments.png)

    In dit deelvenster ziet u de rollen die zijn toegewezen aan de geselecteerde beveiligings-principal en het bereik. Als er een in dit bereik toewijzingen zijn geweigerd of zijn overgenomen, worden deze weergegeven.

## <a name="next-steps"></a>Volgende stappen

- [Roltoewijzingen toevoegen of verwijderen met behulp van Azure RBAC en de Azure Portal](role-assignments-portal.md)
- [Problemen met RBAC voor Azure-resources oplossen](troubleshooting.md)
