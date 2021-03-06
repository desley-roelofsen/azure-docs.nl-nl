---
title: Aan de slag met het integreren van Azure AD met apps | Microsoft Docs
description: Dit artikel is een aan de slag-hand leiding voor het integreren van Azure Active Directory (AD) met on-premises toepassingen en Cloud toepassingen.
services: active-directory
documentationcenter: ''
author: msmimart
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-mgmt
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/16/2018
ms.author: mimart
ms.reviewer: asteen
ms.collection: M365-identity-device-management
ms.openlocfilehash: 1291db8cd895a1104183ae2b5388eb6a23305874
ms.sourcegitcommit: 5acd8f33a5adce3f5ded20dff2a7a48a07be8672
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/24/2019
ms.locfileid: "72895935"
---
# <a name="integrating-azure-active-directory-with-applications-getting-started-guide"></a>Hand leiding voor het integreren van Azure Active Directory met toepassingen

Dit onderwerp bevat een overzicht van het proces voor het integreren van toepassingen met Azure Active Directory (AD). Elk van de onderstaande secties bevat een korte samen vatting van een gedetailleerd onderwerp, zodat u kunt zien welke onderdelen van deze aan de slag-hand leiding relevant zijn voor u.

Zie [volgende stappen](#next-steps)voor het downloaden van gedetailleerde implementatie plannen.

## <a name="take-inventory"></a>Inventaris maken
Voordat u toepassingen met Azure AD integreert, is het belang rijk om te weten waar u zich bevindt en waar u wilt gaan.  De volgende vragen zijn bedoeld om u te helpen bij het integratie project van Azure AD-toepassingen.

### <a name="application-inventory"></a>Toepassings inventaris
* Waar zijn al uw toepassingen? Wie is eigenaar van de gebruikers?
* Wat voor soort verificatie vereisen uw toepassingen?
* Wie moet toegang hebben tot de toepassingen?
* Wilt u een nieuwe toepassing implementeren?
  * Maakt u deze intern en implementeert u deze in een Azure Compute-instantie?
  * Gebruikt u een die beschikbaar is in de Azure-toepassing galerie?

### <a name="user-and-group-inventory"></a>Gebruikers-en groeps inventaris
* Waar bevinden uw gebruikers accounts zich?
  * On-premises Active Directory
  * Azure AD
  * Binnen een afzonderlijke toepassings database waarvan u de eigenaar bent
  * In niet-goedgekeurde toepassingen
  * Alle bovenstaande
* Welke machtigingen en roltoewijzingen hebben momenteel afzonderlijke gebruikers? Moet u de toegang controleren of weet u zeker dat uw gebruikers toegang en roltoewijzingen nu geschikt zijn?
* Worden er al groepen in uw on-premises Active Directory gemaakt?
  * Hoe worden uw groepen ingedeeld?
  * Wie zijn de groeps leden?
  * Welke machtigingen/roltoewijzingen hebben de groepen momenteel?
* Moet u de gebruikers-of groeps databases opschonen voordat u de integratie kunt door gaan?  (Dit is een belang rijke vraag. Opschonen, opschonen.)

### <a name="access-management-inventory"></a>Inventaris van toegangs beheer
* Hoe beheert u momenteel de gebruikers toegang tot toepassingen? Moet dat veranderen?  Hebt u andere manieren om toegang te beheren, zoals met [RBAC](../../role-based-access-control/role-assignments-portal.md) bijvoorbeeld?
* Wie moet er toegang tot hebben?

Misschien hebt u niet de antwoorden op al deze vragen, maar dat is wel goed.  Deze hand leiding kan u helpen bij het beantwoorden van enkele van deze vragen en een aantal weloverwogen beslissingen te nemen.

### <a name="find-unsanctioned-cloud-applications-with-cloud-discovery"></a>Niet-goedgekeurde Cloud toepassingen met Cloud Discovery zoeken

Zoals hierboven vermeld, zijn er mogelijk toepassingen die niet zijn beheerd door uw organisatie tot nu toe.  Als onderdeel van het inventaris proces is het mogelijk om niet-goedgekeurde Cloud toepassingen te vinden. Zie [Cloud Discovery instellen](/cloud-app-security/set-up-cloud-discovery).

## <a name="integrating-applications-with-azure-ad"></a>Toepassingen integreren met Azure AD
In de volgende artikelen vindt u informatie over de verschillende manieren waarop toepassingen kunnen worden geïntegreerd met Azure AD en bieden u enkele richt lijnen.

* [Bepalen welke Active Directory moeten worden gebruikt](../fundamentals/active-directory-administer.md)
* [Toepassingen gebruiken in de Azure-toepassings galerie](what-is-single-sign-on.md)
* [Lijst met zelf studies voor SaaS-toepassingen integreren](../active-directory-saas-tutorial-list.md)

### <a name="authentication-types"></a>Verificatie typen
Elk van uw toepassingen heeft mogelijk andere verificatie vereisten. Met Azure AD kunnen handtekening certificaten worden gebruikt met toepassingen die gebruikmaken van SAML 2,0-, WS-Federation-of OpenID Connect Connect-protocollen en een eenmalige aanmelding met een wacht woord. Zie [certificaten beheren voor federatieve eenmalige aanmelding in azure Active Directory](manage-certificates-for-federated-single-sign-on.md) en [op wacht woord gebaseerde eenmalige aanmelding](what-is-single-sign-on.md)voor meer informatie over toepassings verificatie typen voor gebruik met Azure AD.

### <a name="enabling-sso-with-azure-ad-app-proxy"></a>Eenmalige aanmelding met Azure AD-app proxy inschakelen
Met Microsoft Azure AD toepassings proxy kunt u vanaf elke locatie en op elk apparaat toegang bieden tot toepassingen die zich in uw particuliere netwerk bevinden. Nadat u een toepassings proxy connector hebt geïnstalleerd in uw omgeving, kunt u deze eenvoudig configureren met Azure AD.

### <a name="integrating-custom-applications"></a>Aangepaste toepassingen integreren
Als u een nieuwe toepassing schrijft en ontwikkel aars wilt helpen om gebruik te maken van de kracht van Azure AD, raadpleegt u [ontwikkel aars](../active-directory-applications-guiding-developers-for-lob-applications.md)voor het maken van guid's.

Als u uw aangepaste toepassing wilt toevoegen aan de Azure-toepassing galerie, raadpleegt u [' uw eigen app meenemen ' met Azure AD self-service SAML-configuratie](https://cloudblogs.microsoft.com/enterprisemobility/2015/06/17/bring-your-own-app-with-azure-ad-self-service-saml-configuration-now-in-preview/).

## <a name="managing-access-to-applications"></a>Toegang tot toepassingen beheren
In de volgende artikelen worden de manieren beschreven waarop u de toegang tot toepassingen kunt beheren wanneer ze zijn geïntegreerd met Azure AD met behulp van Azure AD-connectors en Azure AD.

* [Toegang tot apps beheren met Azure AD](what-is-access-management.md)
* [Automatiseren met Azure AD-connectors](user-provisioning.md)
* [Gebruikers toewijzen aan een toepassing](../active-directory-applications-guiding-developers-assigning-users.md)
* [Groepen toewijzen aan een toepassing](../active-directory-applications-guiding-developers-assigning-groups.md)
* [Accounts delen](../active-directory-sharing-accounts.md)

## <a name="next-steps"></a>Volgende stappen
Voor gedetailleerde informatie kunt u Azure Active Directory implementatie plannen downloaden van [github](https://aka.ms/deploymentplans). Voor galerie toepassingen kunt u implementatie plannen voor eenmalige aanmelding, voorwaardelijke toegang en gebruikers inrichten via de [Azure Portal](https://portal.azure.com)downloaden. 

Een implementatie plan downloaden van de Azure Portal:

1. Meld u aan bij de [Azure-portal](https://portal.azure.com).
2. Selecteer **bedrijfs toepassingen** | **kies een app** | **implementatie plan**.

Geef feedback over implementatie plannen door gebruik te maken van de enquête van het [implementatie plan](https://aka.ms/DeploymentPlanFeedback).
