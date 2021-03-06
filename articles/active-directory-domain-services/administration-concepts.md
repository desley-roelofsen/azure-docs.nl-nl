---
title: Beheer concepten voor Azure AD Domain Services | Microsoft Docs
description: Meer informatie over het beheren van een Azure Active Directory Domain Services beheerd domein en het gedrag van gebruikers accounts en wacht woorden
services: active-directory-ds
author: iainfoulds
manager: daveba
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.topic: conceptual
ms.date: 10/08/2019
ms.author: iainfou
ms.openlocfilehash: f239bab48e732755361fe734fdc24b37d3823c63
ms.sourcegitcommit: 8cf199fbb3d7f36478a54700740eb2e9edb823e8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/25/2019
ms.locfileid: "74481014"
---
# <a name="management-concepts-for-user-accounts-passwords-and-administration-in-azure-active-directory-domain-services"></a>Beheer concepten voor gebruikers accounts, wacht woorden en beheer in Azure Active Directory Domain Services

Wanneer u een door Azure Active Directory Domain Services (AD DS) beheerd domein maakt en uitvoert, zijn er enkele verschillen in werking ten opzichte van een traditionele on-premises AD DS omgeving. U gebruikt dezelfde beheer Programma's in azure AD DS als een zelf beheerd domein, maar u hebt geen rechtstreekse toegang tot de domein controllers (DC). Er zijn ook enkele verschillen in gedrag voor wachtwoord beleid en wacht woord-hashes, afhankelijk van de bron van het maken van het gebruikers account.

In dit conceptuele artikel vindt u informatie over het beheren van een Azure AD DS beheerd domein en het andere gedrag van gebruikers accounts, afhankelijk van de manier waarop ze zijn gemaakt.

## <a name="domain-management"></a>Domein beheer

In azure AD DS maken de domein controllers (Dc's) die alle resources zoals gebruikers en groepen, referenties en beleids regels bevatten onderdeel van de beheerde service. Voor redundantie worden twee Dc's gemaakt als onderdeel van een door Azure AD DS beheerd domein. U kunt zich niet aanmelden bij deze Dc's om beheer taken uit te voeren. In plaats daarvan maakt u een beheer-VM die is gekoppeld aan het beheerde domein van Azure AD DS en installeert u vervolgens uw reguliere AD DS-beheer hulpprogramma's. U kunt bijvoorbeeld de Active Directory-beheercentrum-of MMC-modules (micro soft Management Console), zoals DNS of groepsbeleid objecten, gebruiken.

## <a name="user-account-creation"></a>Gebruikers account maken

Gebruikers accounts kunnen op verschillende manieren in azure AD DS worden gemaakt. De meeste gebruikers accounts worden gesynchroniseerd vanuit Azure AD, waarmee ook gebruikers accounts kunnen worden gesynchroniseerd vanuit een on-premises AD DS omgeving. U kunt ook hand matig accounts rechtstreeks in azure AD DS maken. Sommige functies, zoals initiële wachtwoord synchronisatie of wachtwoord beleid, gedragen zich anders afhankelijk van hoe en waar gebruikers accounts worden gemaakt.

* Het gebruikers account kan worden gesynchroniseerd vanuit Azure AD. Dit omvat alleen Cloud gebruikers accounts die rechtstreeks zijn gemaakt in azure AD, en hybride gebruikers accounts die zijn gesynchroniseerd vanuit een on-premises AD DS omgeving met behulp van Azure AD Connect.
    * Het meren deel van gebruikers accounts in azure AD DS worden gemaakt via het synchronisatie proces van Azure AD.
* Het gebruikers account kan hand matig worden gemaakt in een beheerd domein van Azure AD DS en bestaat niet in azure AD.
    * Als u service accounts moet maken voor toepassingen die alleen worden uitgevoerd in azure AD DS, kunt u deze hand matig maken in het beheerde domein. Omdat de synchronisatie in één richting vanuit Azure AD is, worden gebruikers accounts die zijn gemaakt in azure AD DS niet weer gesynchroniseerd met Azure AD.

## <a name="password-policy"></a>Wachtwoord beleid

Azure AD DS bevat een standaard wachtwoord beleid dat instellingen definieert voor zaken als account vergrendeling, maximale wachtwoord duur en wachtwoord complexiteit. Instellingen als account vergrendelings beleid gelden voor alle gebruikers in azure AD DS, ongeacht de manier waarop de gebruiker is gemaakt, zoals beschreven in de vorige sectie. Enkele instellingen, zoals minimale wachtwoord lengte en wachtwoord complexiteit, zijn alleen van toepassing op gebruikers die rechtstreeks zijn gemaakt in azure AD DS.

U kunt uw eigen aangepaste wachtwoord beleidsregels maken om het standaard beleid in azure AD DS te negeren. Deze aangepaste beleids regels kunnen vervolgens worden toegepast op specifieke groepen gebruikers als dat nodig is.

Zie [wacht woord-en account vergrendelings beleid in beheerde domeinen][password-policy]voor meer informatie over de verschillen in de manier waarop wachtwoord beleid wordt toegepast, afhankelijk van de bron van het maken van een gebruiker.

## <a name="password-hashes"></a>Wacht woord-hashes

Voor het verifiëren van gebruikers in het beheerde domein heeft Azure AD DS wacht woord-hashes nodig in een indeling die geschikt is voor NT LAN Manager (NTLM) en Kerberos-verificatie. Azure AD genereert of slaat geen wacht woord-hashes in de vereiste indeling voor NTLM-of Kerberos-authenticatie totdat u Azure AD DS voor uw Tenant inschakelt. Uit veiligheids overwegingen slaat Azure AD ook geen wachtwoord referenties op in een normale tekst vorm. Daarom kan Azure AD deze NTLM-of Kerberos-wachtwoord hashes niet automatisch genereren op basis van de bestaande referenties van gebruikers.

Voor Cloud gebruikers accounts moeten gebruikers hun wacht woord wijzigen voordat ze Azure AD DS kunnen gebruiken. Door deze wachtwoord wijziging worden de wacht woord-hashes voor Kerberos-en NTLM-verificatie gegenereerd en opgeslagen in azure AD.

[Schakel synchronisatie van wacht woord-hashes][hybrid-phs]in voor gebruikers die zijn gesynchroniseerd vanuit een on-premises AD DS omgeving met Azure AD Connect.

> [!IMPORTANT]
> Azure AD Connect synchroniseert alleen verouderde wachtwoord-hashes wanneer u Azure AD DS voor uw Azure AD-Tenant inschakelt. Verouderde wachtwoord hashes worden niet gebruikt als u alleen Azure AD Connect gebruikt om een on-premises AD DS omgeving te synchroniseren met Azure AD.
>
> Als uw verouderde toepassingen geen gebruikmaken van NTLM-verificatie of LDAP-eenvoudige bindingen, raden we u aan NTLM-wachtwoord hash-synchronisatie voor Azure AD DS uit te scha kelen. Zie voor meer informatie [zwak coderings suites en hash-synchronisatie van NTLM-referenties uitschakelen][secure-domain].

Na de juiste configuratie worden de bruikbare wachtwoord hashes opgeslagen in het beheerde domein van Azure AD DS. Als u het door Azure AD DS beheerde domein verwijdert, worden ook de wacht woord-hashes verwijderd die op dat punt zijn opgeslagen. Gesynchroniseerde referentie gegevens in azure AD kunnen niet opnieuw worden gebruikt als u later een door Azure AD DS beheerd domein maakt, moet u de wachtwoord-hash-synchronisatie opnieuw configureren om de wacht woord-hashes opnieuw op te slaan. Eerder aan een domein gekoppelde Vm's of gebruikers kunnen zich niet onmiddellijk verifiëren: Azure AD moet de wacht woord-hashes genereren en opslaan in het nieuwe Azure AD DS beheerde domein. Zie [wacht woord-hash synchronisatie proces voor Azure AD DS en Azure AD Connect][azure-ad-password-sync]voor meer informatie.

> [!IMPORTANT]
> Azure AD Connect mag alleen worden geïnstalleerd en geconfigureerd voor synchronisatie met on-premises AD DS omgevingen. Het is niet mogelijk om Azure AD Connect te installeren in een beheerd domein van Azure AD DS om objecten terug te synchroniseren naar Azure AD.

## <a name="forests-and-trusts"></a>Forests en vertrouwensrelaties

Een *forest* is een logische constructie die door Active Directory Domain Services (AD DS) wordt gebruikt om een of meer *domeinen*te groeperen. De domeinen slaan objecten vervolgens op voor gebruikers of groepen en bieden verificatie services.

In azure AD DS bevat het forest slechts één domein. On-premises AD DS bossen bevatten vaak veel domeinen. In grote organisaties, met name na fusies en acquisities, kunt u meerdere on-premises forests gebruiken die elk meerdere domeinen bevatten.

Standaard wordt een door Azure AD DS beheerd domein gemaakt als een *gebruikers* forest. Dit type forest synchroniseert alle objecten van Azure AD, met inbegrip van gebruikers accounts die zijn gemaakt in een on-premises AD DS omgeving. Gebruikers accounts kunnen rechtstreeks worden geverifieerd aan de hand van het beheerde domein van Azure AD DS, bijvoorbeeld om zich aan te melden bij een VM die lid is van een domein. Een gebruikers forest werkt wanneer de wacht woord-hashes kunnen worden gesynchroniseerd en gebruikers gebruiken geen exclusieve aanmeldings methoden zoals smartcard verificatie.

In een Azure AD DS- *bron* -forest verifiëren *gebruikers via een eenrichtings forestvertrouwensrelatie van hun* on-premises AD DS. Met deze benadering worden de gebruikers objecten en wacht woord-hashes niet gesynchroniseerd met Azure AD DS. De gebruikers objecten en referenties bestaan alleen in het on-premises AD DS. Met deze aanpak kunnen bedrijven hosten van resources en toepassings platforms in azure die afhankelijk zijn van klassieke verificatie, zoals LDAPS, Kerberos of NTLM, maar eventuele verificatie problemen of problemen worden verwijderd. Azure AD DS resource-forests zijn momenteel beschikbaar als preview-versie.

Zie [Wat zijn resource][concepts-forest] -forests? en [Hoe worden forest-vertrouwens relaties in azure AD DS?][concepts-trust] voor meer informatie over forest-typen in azure AD DS?

## <a name="next-steps"></a>Volgende stappen

Als u aan de slag wilt gaan, [maakt u een door Azure AD DS beheerd domein][create-instance].

<!-- INTERNAL LINKS -->
[password-policy]: password-policy.md
[hybrid-phs]: tutorial-configure-password-hash-sync.md#enable-synchronization-of-password-hashes
[secure-domain]: secure-your-domain.md
[azure-ad-password-sync]: ../active-directory/hybrid/how-to-connect-password-hash-synchronization.md#password-hash-sync-process-for-azure-ad-domain-services
[create-instance]: tutorial-create-instance.md
[tutorial-create-instance-advanced]: tutorial-create-instance-advanced.md
[concepts-forest]: concepts-resource-forest.md
[concepts-trust]: concepts-forest-trust.md
