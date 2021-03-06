---
title: Belangrijkste concepten voor het beheer van de Azure VMware-oplossing door CloudSimple
description: Hierin worden de belangrijkste concepten beschreven voor het beheren van de Azure VMware-oplossing op CloudSimple
author: sharaths-cs
ms.author: b-shsury
ms.date: 04/24/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 120ab94ce62010acf4e742bd5220405961dad6db
ms.sourcegitcommit: 47b00a15ef112c8b513046c668a33e20fd3b3119
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/22/2019
ms.locfileid: "69972731"
---
# <a name="key-concepts-for-administration-of-azure-vmware-solution-by-cloudsimple"></a>Belangrijkste concepten voor het beheer van de Azure VMware-oplossing door CloudSimple

Voor het beheren van de Azure VMware-oplossing door CloudSimple is een goed idee van de volgende concepten:

* CloudSimple-service, die wordt weer gegeven als Azure VMware-oplossing door CloudSimple-service
* CloudSimple-knoop punt, dat wordt weer gegeven als Azure VMware-oplossing door CloudSimple-node
* CloudSimple-privécloud
* Service netwerken
* CloudSimple virtuele machine, die wordt weer gegeven als Azure VMware-oplossing door CloudSimple-virtual machine

## <a name="cloudsimple-service"></a>CloudSimple-service

Met de CloudSimple-service kunt u alle resources die zijn gekoppeld aan VMware-oplossingen maken en beheren via CloudSimple van de Azure Portal. Maak een service resource in elke regio waar u de service wilt gebruiken.

Meer informatie over de [CloudSimple-service](cloudsimple-service.md).

## <a name="cloudsimple-node"></a>CloudSimple-knoop punt

Een CloudSimple-knoop punt is een speciale, Bare-Metal, hypergeconvergeerd Compute-en opslag host waarop de VMware ESXi Hyper Visor is geïmplementeerd. Dit knoop punt wordt vervolgens opgenomen in de VMware vSphere-, vCenter-, vSAN-en NSX-platforms. CloudSimple Network Services en Edge Network Services zijn ook ingeschakeld. Elk knoop punt fungeert als een eenheid voor reken-en opslag capaciteit die u kunt inrichten voor het maken van [CloudSimple-persoonlijke Clouds](cloudsimple-private-cloud.md). U kunt knoop punten inrichten of reserveren in een regio waar de CloudSimple-service beschikbaar is.

Meer informatie over [CloudSimple-knoop punten](cloudsimple-node.md).

## <a name="cloudsimple-private-cloud"></a>CloudSimple-privécloud

Een CloudSimple-privécloud is een geïsoleerde VMware-stack omgeving die wordt beheerd door een vCenter-Server in een eigen beheer domein. De VMware-stack bevat ESXi-hosts, vSphere, vCenter, vSAN en NSX. De stack wordt uitgevoerd op toegewezen knoop punten (toegewezen en geïsoleerde bare-metal hardware) en wordt door gebruikers gebruikt via systeem eigen VMware-hulpprogram ma's die vCenter en NSX manager bevatten. Toegewezen knoop punten worden geïmplementeerd op Azure-locaties en worden beheerd door Azure. Elke privécloud kan worden gesegmenteerd en beveiligd met behulp van netwerk services, zoals VLAN'S en subnetten en firewall tabellen. Verbindingen met uw on-premises omgeving en het Azure-netwerk worden gemaakt met behulp van beveiligde, particuliere VPN-en Azure ExpressRoute-verbindingen.

Meer informatie over [CloudSimple Private Cloud](cloudsimple-private-cloud.md).

## <a name="service-networking"></a>Service netwerken

De CloudSimple-service biedt een netwerk per regio waar uw CloudSimple-service wordt geïmplementeerd. Het netwerk is één adres ruimte van de TCP-laag 3 waarvoor route ring standaard is ingeschakeld. Alle persoonlijke Clouds en subnetten die in deze regio worden gemaakt, communiceren zonder aanvullende configuratie. U maakt gedistribueerde poort groepen op de vCenter met behulp van de VLAN'S. U kunt de volgende netwerk functies gebruiken voor het configureren en beveiligen van uw werkbelasting resources in uw privécloud:

* [VLAN'S en subnetten](cloudsimple-vlans-subnets.md)
* [Firewall tabellen](cloudsimple-firewall-tables.md)
* [VPN-gateways](cloudsimple-vpn-gateways.md)
* [Openbare IP](cloudsimple-public-ip-address.md)
* [Azure-netwerk verbinding](cloudsimple-azure-network-connection.md)

## <a name="cloudsimple-virtual-machine"></a>Virtuele machine CloudSimple

Met de CloudSimple-service kunt u virtuele VMware-machines beheren vanuit het Azure Portal. Een of meer clusters of resource groepen uit uw vSphere-omgeving kunnen worden toegewezen aan het abonnement waarop de service is gemaakt.

Meer informatie over:

* [Virtuele CloudSimple-machines](cloudsimple-virtual-machines.md)
* [Toewijzing van Azure-abonnement](https://docs.azure.cloudsimple.com/azure-subscription-mapping/)
