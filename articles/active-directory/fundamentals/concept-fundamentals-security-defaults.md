---
title: Azure Active Directory standaard instellingen voor beveiliging
description: Standaard beleid voor beveiliging waarmee organisaties worden beschermd tegen algemene aanvallen
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: conceptual
ms.date: 12/06/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: rogoya
ms.collection: M365-identity-device-management
ms.openlocfilehash: d899f477612e4c738314187f61551fe5c0b17f8d
ms.sourcegitcommit: a5ebf5026d9967c4c4f92432698cb1f8651c03bb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/08/2019
ms.locfileid: "74932409"
---
# <a name="what-are-security-defaults"></a>Wat zijn de standaard beveiligings instellingen?

Het beheren van beveiliging kan lastig zijn wanneer algemene identiteits-gerelateerde aanvallen meer en populairder zijn. Deze aanvallen omvatten wachtwoord spray, herhaling en phishing.

Met de standaard instellingen voor beveiliging in Azure Active Directory (Azure AD) kunt u uw organisatie beter beveiligen en beschermen. De standaard instellingen voor beveiliging bevatten vooraf geconfigureerde beveiligings instellingen voor veelvoorkomende aanvallen. 

Micro soft maakt standaard beveiligings instellingen voor iedereen beschikbaar. Het doel is om ervoor te zorgen dat alle organisaties een basis niveau van beveiliging hebben ingeschakeld zonder extra kosten. U kunt de standaard instellingen voor beveiliging inschakelen in de Azure Portal.

![Scherm afbeelding van de Azure Portal met de wissel knop voor het inschakelen van standaard instellingen voor beveiliging](./media/concept-fundamentals-security-defaults/security-defaults-azure-ad-portal.png)
 
De volgende beveiligings configuraties worden ingeschakeld in uw Tenant. 

## <a name="unified-multi-factor-authentication-registration"></a>Registratie Unified Multi-Factor Authentication

Alle gebruikers in uw Tenant moeten zich registreren voor multi-factor Authentication (MFA) in de vorm van de Azure Multi-Factor Authentication-service. Gebruikers hebben 14 dagen de tijd om zich te registreren voor Multi-Factor Authentication met behulp van de Microsoft Authenticator-app. Nadat de 14 dagen zijn verstreken, kan de gebruiker zich pas aanmelden nadat Multi-Factor Authentication registratie is voltooid.

We begrijpen dat sommige gebruikers mogelijk niet op kantoor zijn of zich niet meer in de 14 dagen aanmelden nadat de standaard instellingen voor beveiliging zijn ingeschakeld. Om ervoor te zorgen dat elke gebruiker over ruim genoeg tijd heeft om zich te registreren voor Multi-Factor Authentication, is de periode van 14 dagen uniek voor elke gebruiker. De 14-daagse periode van een gebruiker begint na de eerste geslaagde interactieve aanmelding nadat u de standaard instellingen voor beveiliging hebt ingeschakeld.

## <a name="multi-factor-authentication-enforcement"></a>Multi-Factor Authentication afdwingen

### <a name="protecting-administrators"></a>Beheerders beveiligen

Gebruikers met toegang tot geprivilegieerde accounts hebben meer toegang tot uw omgeving. Als gevolg van de kracht van deze accounts, moet u deze met speciale zorg behandelen. Een gemeen schappelijke methode voor het verbeteren van de beveiliging van geprivilegieerde accounts is het vereisen van een sterkere vorm van account verificatie voor het aanmelden. In azure AD kunt u een sterkere account verificatie krijgen door Multi-Factor Authentication te vereisen.

Nadat de registratie bij Multi-Factor Authentication is voltooid, moeten de volgende negen Azure AD-beheerders rollen extra authenticatie uitvoeren elke keer dat ze zich aanmelden:

- Globale beheerder
- SharePoint-beheerder
- Exchange-beheerder
- Beheerder van voorwaardelijke toegang
- Beveiligingsbeheerder
- Helpdesk beheerder of wachtwoord beheerder
- Factureringsbeheerder
- Gebruikers beheerder
- Verificatie beheerder

### <a name="protecting-all-users"></a>Alle gebruikers beveiligen

We denken meestal dat beheerders accounts de enige accounts zijn die extra authenticatie lagen nodig hebben. Beheerders hebben brede toegang tot gevoelige informatie en kunnen wijzigingen aanbrengen in instellingen voor het hele abonnement. Maar aanvallers richten zich vaak op eind gebruikers. 

Wanneer deze aanvallers toegang krijgen, kunnen ze namens de houder van het oorspronkelijke account toegang tot bevoegde informatie vragen. Ze kunnen de volledige directory zelfs downloaden om een phishing-aanval uit te voeren op uw hele organisatie. 

Een gemeen schappelijke methode voor het verbeteren van de beveiliging van alle gebruikers is het vereisen van een sterkere vorm van account verificatie, zoals Multi-Factor Authentication, voor iedereen. Nadat gebruikers Multi-Factor Authentication registratie hebben voltooid, wordt de gebruiker gevraagd om extra verificatie wanneer dit nodig is.

### <a name="blocking-legacy-authentication"></a>Verouderde verificatie blok keren

Azure AD biedt ondersteuning voor diverse verificatie protocollen, waaronder verouderde verificatie, om uw gebruikers eenvoudig toegang te geven tot uw Cloud-apps. *Verouderde verificatie* is een term die verwijst naar een verificatie aanvraag die wordt gedaan door:

- Oudere Office-clients die geen gebruik kunnen maakt van moderne verificatie (bijvoorbeeld een Office 2010-client).
- Elke client die gebruikmaakt van oudere e-mail protocollen, zoals IMAP, SMTP of POP3.

De meeste pogingen om zich aan te melden, zijn van verouderde verificatie. Verouderde verificatie biedt geen ondersteuning voor Multi-Factor Authentication. Zelfs als er een Multi-Factor Authentication beleid is ingeschakeld in uw directory, kan een aanvaller zich verifiëren met behulp van een ouder protocol en Multi-Factor Authentication overs Laan. 

Nadat de standaard instellingen voor beveiliging zijn ingeschakeld in uw Tenant, worden alle verificatie aanvragen die door een ouder protocol worden uitgevoerd, geblokkeerd. Standaard instellingen voor beveiliging blok keren Exchange ActiveSync niet.

### <a name="protecting-privileged-actions"></a>Beschermde acties beveiligen

Organisaties gebruiken verschillende Azure-Services die worden beheerd via de Azure Resource Manager-API, waaronder:

- Azure Portal 
- Azure PowerShell 
- Azure CLI

Het gebruik van Azure Resource Manager voor het beheren van uw services is een zeer geprivilegieerde actie. Azure Resource Manager kunt de configuratie van de Tenant breed wijzigen, zoals service-instellingen en abonnements facturering. Verificatie met één factor is kwetsbaar voor diverse aanvallen zoals phishing en wachtwoord spray. 

Het is belang rijk om de identiteit te verifiëren van gebruikers die toegang willen hebben Azure Resource Manager en om de configuraties bij te werken. U kunt hun identiteit verifiëren door extra verificatie te vereisen voordat u toegang toestaat.

Nadat u de standaard instellingen voor beveiliging in uw Tenant hebt ingeschakeld, moet elke gebruiker die toegang heeft tot de Azure Portal, Azure PowerShell of de Azure CLI, aanvullende verificatie volt ooien. Dit beleid is van toepassing op alle gebruikers die toegang hebben tot Azure Resource Manager, of het nu een beheerder of een gebruiker is. 

Als de gebruiker niet is geregistreerd voor Multi-Factor Authentication, moet de gebruiker zich registreren met behulp van de Microsoft Authenticator-app om door te gaan. Er wordt geen Multi-Factor Authentication registratie periode van 14 dagen gegeven.

## <a name="deployment-considerations"></a>Overwegingen bij de implementatie

De volgende aanvullende overwegingen zijn gerelateerd aan de implementatie van de standaard instellingen voor de beveiliging van uw Tenant.

### <a name="older-protocols"></a>Oudere protocollen

E-mailclients gebruiken oudere verificatie protocollen (zoals IMAP, SMTP en POP3) om verificatie aanvragen te maken. Deze protocollen bieden geen ondersteuning voor Multi-Factor Authentication. Het grootste deel van het account is inbreuk op het verloopt van aanvallen op oudere protocollen die proberen Multi-Factor Authentication te omzeilen. 

Om ervoor te zorgen dat Multi-Factor Authentication vereist is om u aan te melden bij een beheerders account en dat aanvallers deze niet kunnen negeren, worden in standaard instellingen alle verificatie aanvragen geblokkeerd voor beheerders accounts uit oudere protocollen.

> [!WARNING]
> Voordat u deze instelling inschakelt, moet u ervoor zorgen dat uw beheerders geen oudere verificatie protocollen gebruiken. Zie voor meer informatie [hoe u de verouderde verificatie verlaat](concept-fundamentals-block-legacy-authentication.md).

### <a name="conditional-access"></a>Voorwaardelijke toegang

U kunt voorwaardelijke toegang gebruiken voor het configureren van beleids regels die hetzelfde gedrag bieden als ingeschakeld door de standaard instellingen van de beveiliging. Als u voorwaardelijke toegang gebruikt en beleid voor voorwaardelijke toegang hebt ingeschakeld in uw omgeving, zijn de standaard instellingen voor beveiliging niet voor u beschikbaar. Als u een licentie hebt die voorwaardelijke toegang biedt, maar geen beleid voor voorwaardelijke toegang hebt ingeschakeld in uw omgeving, kunt u de standaard instellingen voor beveiliging gebruiken totdat u beleid voor voorwaardelijke toegang inschakelt.

![Waarschuwings bericht dat u standaard instellingen of voorwaardelijke toegang kunt hebben](./media/concept-fundamentals-security-defaults/security-defaults-conditional-access.png)

Hier vindt u stapsgewijze hand leidingen over hoe u voorwaardelijke toegang kunt gebruiken om gelijkwaardige beleids regels te configureren:

- [MFA vereisen voor beheerders](../conditional-access/howto-conditional-access-policy-admin-mfa.md)
- [MFA vereisen voor Azure-beheer](../conditional-access/howto-conditional-access-policy-azure-management.md)
- [Verouderde verificatie blok keren](../conditional-access/howto-conditional-access-policy-block-legacy.md)
- [MFA vereisen voor alle gebruikers](../conditional-access/howto-conditional-access-policy-all-users-mfa.md)
- [Azure MFA-registratie vereisen](../identity-protection/howto-identity-protection-configure-mfa-policy.md) : vereist Azure AD Identity Protection

## <a name="enabling-security-defaults"></a>Standaard instellingen voor beveiliging inschakelen

Standaard instellingen voor beveiliging in uw Directory inschakelen:

1. Meld u aan bij de [Azure Portal](https://portal.azure.com) als een beveiligings beheerder, een beheerder voor voorwaardelijke toegang of een globale beheerder.
1. Blader naar **Azure Active Directory** > **Eigenschappen**.
1. Selecteer **standaard instellingen voor beveiliging beheren**.
1. Stel de **Schakel optie standaard instellingen inschakelen** in op **Ja**.
1. Selecteer **Opslaan**.

## <a name="disabling-security-defaults"></a>Standaard instellingen voor beveiliging uitschakelen

Organisaties die kiezen voor het implementeren van beleid voor voorwaardelijke toegang waarbij de standaard instellingen voor beveiliging worden vervangen, moeten de standaard instellingen voor beveiliging uitschakelen. 

![Waarschuwings bericht voor het uitschakelen van de standaard instellingen voor het inschakelen van voorwaardelijke toegang](./media/concept-fundamentals-security-defaults/security-defaults-disable-before-conditional-access.png)

Standaard instellingen voor beveiliging in uw Directory uitschakelen:

1. Meld u aan bij de [Azure Portal](https://portal.azure.com) als een beveiligings beheerder, een beheerder voor voorwaardelijke toegang of een globale beheerder.
1. Blader naar **Azure Active Directory** > **Eigenschappen**.
1. Selecteer **standaard instellingen voor beveiliging beheren**.
1. Stel de **standaard instellingen** voor het inschakelen van de beveiliging in op **Nee**.
1. Selecteer **Opslaan**.

## <a name="next-steps"></a>Volgende stappen

[Algemeen beleid voor voorwaardelijke toegang](../conditional-access/concept-conditional-access-policy-common.md)
