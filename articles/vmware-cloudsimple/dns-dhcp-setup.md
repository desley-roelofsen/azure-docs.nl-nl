---
title: 'Azure VMware-oplossing door CloudSimple: Stel de werk belasting-DNS en DHCP in voor de Privécloud'
description: Hierin wordt beschreven hoe u DNS en DHCP instelt voor toepassingen en werk belastingen die worden uitgevoerd in uw persoonlijke cloud omgeving van CloudSimple
author: sharaths-cs
ms.author: b-shsury
ms.date: 08/16/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: ce2394805bfc302179da807c798445c30ce9daff
ms.sourcegitcommit: e42c778d38fd623f2ff8850bb6b1718cdb37309f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/19/2019
ms.locfileid: "69619545"
---
# <a name="set-up-dns-and-dhcp-applications-and-workloads-in-your-cloudsimple-private-cloud"></a>DNS-en DHCP-toepassingen en werk belastingen instellen in uw CloudSimple-Privécloud

Toepassingen en werk belastingen die worden uitgevoerd in een Privécloud-omgeving, moeten naam omzetting en DHCP-services voor lookup-en IP-adres toewijzing hebben.  U hebt de juiste DHCP-en DNS-infra structuur nodig om deze services te kunnen leveren.  U kunt een virtuele machine configureren om deze services te leveren in uw Privécloud.  

## <a name="prerequisites"></a>Vereisten

* Een gedistribueerde poort groep waarvoor VLAN is geconfigureerd
* De installatie naar on-premises of op internet gebaseerde DNS-servers routeren
* Virtuele-machine sjabloon of ISO voor het maken van een virtuele machine

## <a name="linux-based-dns-server-setup"></a>Installatie van DNS-server op basis van Linux

Linux biedt verschillende pakketten voor het instellen van DNS-servers.  Hier volgt een [voor beeld van een installatie van DigitalOcean](https://www.digitalocean.com/community/tutorials/how-to-configure-bind-as-a-private-network-dns-server-on-centos-7) met instructies voor het instellen van een open-source BIND-DNS-server.

## <a name="windows-based-setup"></a>Installatie op basis van Windows

In deze micro soft-onderwerpen wordt beschreven hoe u een Windows-Server als een DNS-server en als een DHCP-server instelt.

* [Windows Server als DNS-server](https://docs.microsoft.com/windows-server/networking/dns/dns-top)
* [Windows Server als DHCP-server](https://docs.microsoft.com/windows-server/networking/technologies/dhcp/dhcp-top)
