---
title: Azure VMware-oplossing door CloudSimple-beveiligde Privécloud
description: Hierin wordt beschreven hoe u de Azure VMware-oplossing kunt beveiligen door CloudSimple Private Cloud
author: sharaths-cs
ms.author: b-shsury
ms.date: 08/19/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: c9d3b2858ea3d80836b280b795025f2ce2eb85c7
ms.sourcegitcommit: 9dec0358e5da3ceb0d0e9e234615456c850550f6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/14/2019
ms.locfileid: "72311771"
---
# <a name="how-to-secure-your-private-cloud-environment"></a>Uw persoonlijke cloud omgeving beveiligen

Definieer op rollen gebaseerd toegangs beheer (RBAC) voor de CloudSimple-service, de CloudSimple-Portal en de Privécloud van Azure.  Gebruikers, groepen en rollen voor toegang tot de vCenter van Privécloud zijn opgegeven met VMware SSO.  

## <a name="rbac-for-cloudsimple-service"></a>RBAC voor CloudSimple-service

Voor het maken van een CloudSimple-service is de rol **Owner** of **Inzender** vereist voor het Azure-abonnement.  Standaard kunnen alle eigen aren en mede werkers een CloudSimple-service maken en toegang krijgen tot CloudSimple-portal voor het maken en beheren van privé-Clouds.  Er kan slechts één CloudSimple-service per regio worden gemaakt.  Volg de onderstaande procedure om de toegang tot specifieke beheerders te beperken.

1. Een CloudSimple-service maken in een nieuwe **resource groep** op Azure Portal
2. Geef RBAC op voor de resource groep.
3. Knoop punten kopen en dezelfde resource groep gebruiken als de CloudSimple-service

Alleen de gebruikers die de rechten van de rol van **eigenaar** of **Inzender** hebben voor de resource groep, zien de CloudSimple-service en starten de CloudSimple-Portal.

Zie [Wat is op rollen gebaseerd toegangs beheer (RBAC) voor Azure-resources](../role-based-access-control/overview.md)voor meer informatie over RBAC.

## <a name="rbac-for-private-cloud-vcenter"></a>RBAC voor Private Cloud vCenter

Een standaard gebruiker `CloudOwner@cloudsimple.local` wordt in het vCenter-SSO-domein gemaakt wanneer een Privécloud wordt gemaakt.  CloudOwner-gebruiker heeft bevoegdheden voor het beheren van vCenter. Aanvullende identiteits bronnen worden toegevoegd aan de vCenter-SSO om toegang te verlenen aan verschillende gebruikers.  Vooraf gedefinieerde rollen en groepen worden ingesteld op de vCenter die kan worden gebruikt om extra gebruikers toe te voegen.

### <a name="add-new-users-to-vcenter"></a>Nieuwe gebruikers toevoegen aan vCenter

1. [Escalatie bevoegdheden](escalate-private-cloud-privileges.md) voor **CloudOwner@cloudsimple.local-** gebruiker op de privécloud.
2. Meld u aan bij vCenter met **CloudOwner@cloudsimple.local**
3. [Gebruikers met eenmalige aanmelding via VCenter toevoegen](https://docs.vmware.com/en/VMware-vSphere/5.5/com.vmware.vsphere.security.doc/GUID-72BFF98C-C530-4C50-BF31-B5779D2A4BBB.html).
4. Gebruikers toevoegen aan een [vCenter-groep voor eenmalige aanmelding](https://docs.vmware.com/en/VMware-vSphere/5.5/com.vmware.vsphere.security.doc/GUID-CDEA6F32-7581-4615-8572-E0B44C11D80D.html).

Zie [CloudSimple Private Cloud permission model of VMware vCenter](learn-private-cloud-permissions.md) -artikel voor meer informatie over vooraf gedefinieerde rollen en groepen.

### <a name="add-new-identity-sources"></a>Nieuwe identiteits bronnen toevoegen

U kunt aanvullende id-providers voor het vCenter SSO-domein van uw Privécloud toevoegen.  Id-providers bieden SSO-groepen voor verificatie en vCenter verificatie voor gebruikers.

* [Gebruik Active Directory als een id-provider](set-vcenter-identity.md) in Private Cloud vCenter.
* [Azure AD als id-provider gebruiken](azure-ad.md) op de Privécloud-vCenter

1. [Escalatie bevoegdheden](escalate-private-cloud-privileges.md) voor **CloudOwner@cloudsimple.local-** gebruiker op de privécloud.
2. Meld u aan bij vCenter met **CloudOwner@cloudsimple.local**
3. Gebruikers van de ID-provider toevoegen aan een [vCenter-groep voor eenmalige aanmelding](https://docs.vmware.com/en/VMware-vSphere/5.5/com.vmware.vsphere.security.doc/GUID-CDEA6F32-7581-4615-8572-E0B44C11D80D.html).

## <a name="secure-network-on-your-private-cloud-environment"></a>Netwerk beveiligen in uw Privécloud

De netwerk beveiliging van de particuliere cloud omgeving wordt beheerd door netwerk toegang te beveiligen en netwerk verkeer tussen bronnen te beheren.

### <a name="access-to-private-cloud-resources"></a>Toegang tot persoonlijke cloud resources

De toegang tot de vCenter-en bronnen van de privécloud is via een beveiligde netwerk verbinding:

* **[ExpressRoute-verbinding](on-premises-connection.md)** . ExpressRoute biedt een veilige verbinding met lage latentie met hoge band breedte vanuit uw on-premises omgeving.  Met de verbinding kunnen uw on-premises Services, netwerken en gebruikers toegang krijgen tot uw persoonlijke Cloud-vCenter.
* **[Site-naar-site VPN-gateway](vpn-gateway.md)** . Site-naar-site-VPN geeft vanuit on-premises toegang tot uw persoonlijke Cloud bronnen via een beveiligde tunnel.  U geeft aan welke on-premises netwerken netwerk verkeer naar uw Privécloud kunnen verzenden en ontvangen.
* **[Punt-naar-site-VPN-gateway](vpn-gateway.md#set-up-a-site-to-site-vpn-gateway)** . Gebruik punt-naar-site-VPN-verbinding voor snelle externe toegang tot uw Privécloud.

### <a name="control-network-traffic-in-private-cloud"></a>Netwerk verkeer in een Privécloud beheren

Firewall tabellen en-regels bepalen netwerk verkeer in de Privécloud.  In de tabel firewall kunt u het netwerk verkeer tussen een bron netwerk of IP-adres en een doelnet of IP-adres beheren op basis van de combi natie van regels die in de tabel zijn gedefinieerd.

1. Een [firewall tabel](firewall.md#add-a-new-firewall-table)maken.
2. [Regels toevoegen](firewall.md#create-a-firewall-rule) aan de firewall tabel.
3. [Een firewall tabel koppelen aan een VLAN/subnet] (firewall. MD # attach-vlanssubnet.
