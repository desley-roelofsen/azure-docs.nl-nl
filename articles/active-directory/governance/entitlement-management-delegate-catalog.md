---
title: Delegeren toegang tot de catalogus makers in het beheer van rechten van Azure AD-Azure Active Directory
description: Meer informatie over het overdragen van het toegangs beheer van IT-beheerders naar het catalogiseren van makers en project managers, zodat ze de toegang zelf kunnen beheren.
services: active-directory
documentationCenter: ''
author: msaburnley
manager: daveba
editor: markwahl-msft
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.subservice: compliance
ms.date: 10/26/2019
ms.author: ajburnle
ms.reviewer: mwahl
ms.collection: M365-identity-device-management
ms.openlocfilehash: f71007b886d3cc25a7cf9dc23d784144ed4e1fbd
ms.sourcegitcommit: 98ce5583e376943aaa9773bf8efe0b324a55e58c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/30/2019
ms.locfileid: "73174378"
---
# <a name="delegate-access-governance-to-catalog-creators-in-azure-ad-entitlement-management"></a>Beheer van toegang tot catalogussen van de maker van het Azure AD-recht

Als u wilt delegeren aan gebruikers die geen beheerder zijn, zodat ze hun eigen catalogi kunnen maken, kunt u deze gebruikers toevoegen aan de rol van Azure AD-bevoegdheids beheer-gedefinieerde catalogus Maker. U kunt afzonderlijke gebruikers toevoegen, maar u kunt ook een groep toevoegen, waarvan de leden vervolgens catalogi kunnen maken.

## <a name="as-an-it-administrator-delegate-to-a-catalog-creator"></a>Als IT-beheerder delegeren aan een maker van de catalogus

Volg deze stappen om een gebruiker toe te wijzen aan de maker van de catalogus.

**Vereiste rol:** Globale beheerder of gebruikers beheerder

1. Klik in de Azure Portal op **Azure Active Directory** en klik vervolgens op **Identity governance**.

1. Klik in het menu links in het gedeelte **beheer rechten** op **instellingen**.

1. Klik op **Bewerken**.

    ![Instellingen voor het toevoegen van catalogus makers](./media/entitlement-management-delegate-catalog/settings-delegate.png)

1. Klik in de sectie bevoegdheids **beheer voor gemachtigden** op **catalogus makers toevoegen** om de gebruikers of groepen te selecteren aan wie u deze rechten wilt delegeren.

1. Klik op **Selecteren**.

1. Klik op **Opslaan**.

## <a name="allow-delegated-roles-to-access-the-azure-portal"></a>Gedelegeerde rollen toestaan om toegang te krijgen tot de Azure Portal

Als u gedelegeerde functies, zoals catalogus makers en toegangs pakket beheerders, wilt toestaan om toegang te krijgen tot de Azure Portal voor het beheren van toegangs pakketten, moet u de instelling van de beheer Portal controleren.

**Vereiste rol:** Globale beheerder of gebruikers beheerder

1. Klik in de Azure Portal op **Azure Active Directory** en klik vervolgens op **gebruikers**.

1. Klik in het menu links op **gebruikers instellingen**.

1. Zorg ervoor dat de toegang tot de **Azure AD-beheer Portal beperken** is ingesteld op **Nee**.

    ![Gebruikers instellingen van Azure AD-beheer Portal](./media/entitlement-management-delegate-catalog/user-settings.png)

## <a name="next-steps"></a>Volgende stappen

- [Een catalogus met resources maken en beheren](entitlement-management-catalog-create.md)
- [Toegangs beheer delegeren voor toegang tot pakket beheerders](entitlement-management-delegate-managers.md)

