---
title: Windows Remote Management via HTTPS voor Azure | Azure Marketplace
description: In dit artikel wordt uitgelegd hoe u een door Azure gehoste, op Windows gebaseerde VM configureert zodat deze extern kan worden beheerd met Power shell.
services: Azure, Marketplace, Cloud Partner Portal,
author: v-miclar
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 11/26/2018
ms.author: pabutler
ms.openlocfilehash: e39f83b2ed715afbfff69770c151cfc4d527105d
ms.sourcegitcommit: 2d3740e2670ff193f3e031c1e22dcd9e072d3ad9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/16/2019
ms.locfileid: "74132259"
---
# <a name="windows-remote-management-over-https"></a>Windows Remote Management via HTTPS

In deze sectie wordt uitgelegd hoe u een door Azure gehoste, op Windows gebaseerde VM configureert zodat deze extern kan worden beheerd en geïmplementeerd met Power shell.  Om externe communicatie met Power shell in te scha kelen, moet de doel-VM een HTTPS-eind punt voor Windows Remote Management (WinRM) beschikbaar stellen.  Zie [externe opdrachten uitvoeren](https://docs.microsoft.com/powershell/scripting/learn/remoting/running-remote-commands)voor meer informatie over communicatie met Power shell.  Zie [Windows Remote Management](https://docs.microsoft.com/windows/desktop/WinRM/portal)(Engelstalig) voor meer informatie over WinRM.

Als u een VM hebt gemaakt met behulp van een van de ' klassieke ' Azure-benaderingen, ofwel de Azure Service Manager portal of de afgeschafte [Azure-Service Management-API](https://docs.microsoft.com/previous-versions/azure/ee460799(v=azure.100)), wordt deze automatisch geconfigureerd met een WinRM-eind punt.  Als u echter een VM maakt met behulp van een van de volgende ' moderne ' Azure-benaderingen, wordt uw virtuele machine *niet* geconfigureerd voor WINRM via https.

- Het gebruik van de [Azure Portal](https://portal.azure.com/), doorgaans vanuit een goedgekeurde basis, zoals beschreven in de sectie [een Azure-compatibele VHD maken](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/virtual-machine/cpp-create-vhd)
- [De Azure Resource Manager sjablonen gebruiken](https://docs.microsoft.com/azure/virtual-machines/windows/ps-template)
- De Azure PowerShell-of Azure CLI-opdracht shell gebruiken.  Zie [Quick Start: een virtuele Windows-machine maken in azure met Power shell](https://docs.microsoft.com/azure/virtual-machines/windows/quick-create-powershell) en [Snelstartgids: een virtuele Linux-machine maken met de Azure cli](https://docs.microsoft.com/azure/virtual-machines/linux/quick-create-cli).

Dit WinRM-eind punt is ook vereist voor het uitvoeren van de Certification Tool Kit voor het voorbereiden van de virtuele machine, zoals wordt beschreven in [certificeren van uw VM-installatie kopie](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/virtual-machine/cpp-certify-vm).

Virtuele Linux-machines worden daarentegen extern beheerd met behulp van [Azure cli](https://docs.microsoft.com/cli/azure) -of Linux-opdrachten vanuit een SSH-console.  Azure biedt ook verschillende alternatieve methoden voor [het uitvoeren van scripts in uw Linux-VM](https://docs.microsoft.com/azure/virtual-machines/linux/run-scripts-in-vm).  Voor complexere scenario's zijn er een aantal automatiserings-en integratie oplossingen beschikbaar voor Vm's op basis van Windows of Linux.


## <a name="configure-and-deploy-with-winrm"></a>Configureren en implementeren met WinRM

Het WinRM-eind punt voor een VM op basis van Windows kan worden geconfigureerd tijdens twee verschillende fasen van de ontwikkeling:

- Tijdens het maken: tijdens de implementatie van een virtuele machine naar een bestaande VHD.  Dit is de aanbevolen benadering voor nieuwe aanbiedingen.  Voor deze benadering moet u een Azure-certificaat maken met behulp van de geleverde Azure Resource Manager sjablonen, en aangepaste Power shell-scripts uitvoeren.
- Na de implementatie: op een bestaande virtuele machine die wordt gehost op Azure.  Gebruik deze methode als u al een VM-oplossing hebt geïmplementeerd in azure, en hiervoor het venster extern beheer moet inschakelen.  Deze aanpak vereist hand matige wijzigingen in de Azure Portal en de uitvoering van een script op de doel-VM.


## <a name="next-steps"></a>Volgende stappen
Als u een nieuwe virtuele machine maakt, kunt u WinRM inschakelen tijdens [de implementatie van uw virtuele machine vanaf de vhd's](./cpp-deploy-vm-vhd.md).  Zo niet, dan kan WinRM worden ingeschakeld in een bestaande virtuele machine
