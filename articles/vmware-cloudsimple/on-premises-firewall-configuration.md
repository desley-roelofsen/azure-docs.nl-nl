---
title: Toegang tot de Azure VMware-oplossing via CloudSimple van on-premises
description: Toegang tot uw Azure VMware-oplossing via CloudSimple van uw on-premises netwerk via een firewall
author: sharaths-cs
ms.author: dikamath
ms.date: 08/08/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: c0f575417819f0e2d46565ad15aaa23a04fd7cf1
ms.sourcegitcommit: 47b00a15ef112c8b513046c668a33e20fd3b3119
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/22/2019
ms.locfileid: "69972642"
---
# <a name="accessing-your-cloudsimple-private-cloud-environment-and-applications-from-on-premises"></a>Toegang tot uw persoonlijke cloud omgeving en-toepassingen van CloudSimple vanuit on-premises

Een verbinding kan worden ingesteld vanuit een on-premises netwerk naar CloudSimple met behulp van Azure ExpressRoute of site-naar-site-VPN.  Open uw CloudSimple Private Cloud vCenter en alle workloads die u op de Privécloud uitvoert met behulp van de verbinding.  U kunt bepalen welke poorten op de verbinding worden geopend met behulp van een firewall in uw on-premises netwerk.  In dit artikel worden enkele van de typische poort vereisten voor toepassingen besproken.  Raadpleeg de documentatie van de toepassing voor de poort vereisten voor andere toepassingen.

## <a name="ports-required-for-accessing-vcenter"></a>Poorten die nodig zijn voor toegang tot vCenter

Om toegang te krijgen tot uw Privécloud en NSX-T-beheer, moeten de poorten die in de onderstaande tabel zijn gedefinieerd, worden geopend op de on-premises firewall.  

| Port       | Source                           | Bestemming                      | Doel                                                                                                                |
|------------|----------------------------------|----------------------------------|------------------------------------------------------------------------------------------------------------------------|
| 53 (UDP)   | On-premises DNS-servers          | DNS-servers in privécloud        | Vereist voor het door sturen van de DNS-zoek opdracht van *AZ.cloudsimple.io* naar DNS-servers in de privécloud vanuit het on-premises netwerk.       |
| 53 (UDP)   | DNS-servers in privécloud        | On-premises DNS-servers          | Vereist voor het door sturen van DNS-zoek acties van on-premises domein namen van de Privécloud naar on-premises DNS-servers. |
| 80 (TCP)   | On-premises netwerk              | Particulier Cloud beheer netwerk | Vereist voor het omleiden van de vCenter *-* URL van http naar *https*.                                                           |
| 443 (TCP)  | On-premises netwerk              | Particulier Cloud beheer netwerk | Vereist voor toegang tot vCenter en NSX-T-beheer vanuit een on-premises netwerk.                                             |
| 8000 (TCP) | On-premises netwerk              | Particulier Cloud beheer netwerk | Vereist voor vMotion van virtuele machines van on-premises naar een Privécloud.                                            |
| 8000 (TCP) | Particulier Cloud beheer netwerk | On-premises netwerk              | Vereist voor vMotion van virtuele machines van de Privécloud naar on-premises.                                            |

## <a name="ports-required-for-using-on-premises-active-directory-as-an-identity-source"></a>Poorten die vereist zijn voor het gebruik van on-premises Active Directory als een identiteits bron

Als u een on-premises Active Directory als een identiteits bron wilt configureren op een Privécloud-vCenter, moeten de poorten die in de tabel zijn gedefinieerd, worden geopend.  Zie [Azure AD gebruiken als een id-provider voor vCenter op CloudSimple Private Cloud](https://docs.azure.cloudsimple.com/azure-ad/) voor configuratie stappen.

| Port         | Source                           | Bestemming                                         | Doel                                                                                                                                          |
|--------------|----------------------------------|-----------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------|
| 53 (UDP)      | DNS-servers in privécloud        | On-premises DNS-servers                             | Vereist voor het door sturen van DNS-zoek acties van on-premises Active Directory-domein namen van Privécloud naar on-premises DNS-servers.          |
| 389 (TCP/UDP) | Particulier Cloud beheer netwerk | On-premises Active Directory-domein controllers     | Vereist voor LDAP-communicatie van particuliere cloud vCenter-Server naar Active Directory-domein controllers voor gebruikers verificatie.                |
| 636 (TCP)     | Particulier Cloud beheer netwerk | On-premises Active Directory-domein controllers     | Vereist voor secure LDAP-communicatie (LDAPS) van de Privécloud-server naar Active Directory-domein controllers voor gebruikers verificatie. |
| 3268 (TCP)    | Particulier Cloud beheer netwerk | On-premises Active Directory Global Catalog-servers | Vereist voor LDAP-communicatie in een implementatie van meerdere domein controllers.                                                                        |
| 3269 (TCP)    | Particulier Cloud beheer netwerk | On-premises Active Directory Global Catalog-servers | Vereist voor LDAPS-communicatie in een implementatie van meerdere domein controllers.                                                                       |                                           |

## <a name="common-ports-required-for-accessing-workload-virtual-machines"></a>Algemene poorten die vereist zijn voor toegang tot virtuele workload-machines

Voor toegang tot virtuele machines van de werk belasting die worden uitgevoerd op een Privécloud, moeten poorten worden geopend op uw on-premises firewall.  In de onderstaande tabel ziet u een aantal algemene poorten die vereist zijn en hun doel.  Raadpleeg de documentatie van de toepassing voor alle toepassingsspecifieke poort vereisten.

| Port         | Source                         | Bestemming                          | Doel                                                                              |
|--------------|--------------------------------|--------------------------------------|--------------------------------------------------------------------------------------|
| 22 (TCP)      | On-premises netwerk            | Werkbelasting netwerk voor de privécloud       | Beveiligde shell toegang tot virtuele Linux-machines die worden uitgevoerd op een Privécloud.              |
| 3389 (TCP)    | On-premises netwerk            | Werkbelasting netwerk voor de privécloud       | Extern bureau blad naar virtuele Windows-machines die worden uitgevoerd op een Privécloud.                 |
| 80 (TCP)      | On-premises netwerk            | Werkbelasting netwerk voor de privécloud       | Toegang tot alle webservers die zijn geïmplementeerd op virtuele machines die worden uitgevoerd op een Privécloud.        |
| 443 (TCP)     | On-premises netwerk            | Werkbelasting netwerk voor de privécloud       | Toegang krijgen tot beveiligde webservers die zijn geïmplementeerd op virtuele machines die worden uitgevoerd op een Privécloud. |
| 389 (TCP/UDP) | Werkbelasting netwerk voor de privécloud | On-premises Active Directory-netwerk | Virtuele Windows-werk belasting machines toevoegen aan een on-premises Active Directory-domein.       |
| 53 (UDP)      | Werkbelasting netwerk voor de privécloud | On-premises netwerk                  | Toegang tot de DNS-service voor virtuele werkbelasting machines naar on-premises DNS-servers.         |

## <a name="next-steps"></a>Volgende stappen

* [VLAN'S en subnetten maken en beheren](https://docs.azure.cloudsimple.com/create-vlan-subnet/)
* [Verbinding maken met een on-premises netwerk met behulp van Azure ExpressRoute](https://docs.azure.cloudsimple.com/on-premises-connection/)
* [Site-naar-site-VPN van on-premises instellen](https://docs.azure.cloudsimple.com/vpn-gateway/)
