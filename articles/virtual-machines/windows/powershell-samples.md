---
title: Power shell-voor beelden voor virtuele Azure-machines
description: Power shell-voor beelden voor virtuele Azure-machines
services: virtual-machines-windows
documentationcenter: virtual-machines
author: cynthn
manager: gwallace
editor: tysonn
tags: azure-service-management
ms.assetid: ''
ms.service: virtual-machines-windows
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 03/01/2019
ms.author: cynthn
ms.custom: mvc
ms.openlocfilehash: f068b79f1b1eaa9a11df70052619c8e3993101cb
ms.sourcegitcommit: 49cf9786d3134517727ff1e656c4d8531bbbd332
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/13/2019
ms.locfileid: "74032998"
---
# <a name="azure-virtual-machine-powershell-samples"></a>Power shell-voor beelden voor virtuele Azure-machines

De volgende tabel bevat koppelingen naar Power shell-voorbeeld scripts voor het maken en beheren van virtuele Windows-machines (Vm's).

| | |
|---|---|
|**Virtuele machines maken**||
| [Snel een virtuele machine maken](./../scripts/virtual-machines-windows-powershell-sample-create-vm-quick.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) | Hiermee maakt u een resource groep, een virtuele machine en alle gerelateerde resources, met een minimum aan prompts.|
| [Een volledig geconfigureerde virtuele machine maken](./../scripts/virtual-machines-windows-powershell-sample-create-vm.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) | Hiermee maakt u een resource groep, een virtuele machine en alle gerelateerde resources.|
| [Maxi maal beschik bare virtuele machines maken](./../scripts/virtual-machines-windows-powershell-sample-create-nlb-vm.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) | Maakt verschillende virtuele machines in een configuratie met hoge Beschik baarheid en taak verdeling.|
| [Een virtuele machine maken en een configuratie script uitvoeren](./../scripts/virtual-machines-windows-powershell-sample-create-vm-iis.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) | Hiermee maakt u een virtuele machine en gebruikt u de aangepaste script extensie van Azure om IIS te installeren. |
| [Een virtuele machine maken en een DSC-configuratie uitvoeren](./../scripts/virtual-machines-windows-powershell-sample-create-iis-using-dsc.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) | Hiermee maakt u een virtuele machine en gebruikt u de extensie Azure desired state Configuration (DSC) om IIS te installeren. |
| [Een VHD uploaden en Vm's maken](./../scripts/virtual-machines-windows-powershell-upload-generalized-script.md) | Hiermee wordt een lokaal VHD-bestand geüpload naar Azure, een installatie kopie van de VHD gemaakt en vervolgens een virtuele machine van die installatie kopie gemaakt. |
| [Een virtuele machine maken op basis van een beheerde besturingssysteem schijf](./../scripts/virtual-machines-windows-powershell-sample-create-vm-from-managed-os-disks.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) | Hiermee maakt u een virtuele machine door een bestaande beheerde schijf te koppelen als besturingssysteem schijf. |
| [Een virtuele machine maken op basis van een moment opname](./../scripts/virtual-machines-windows-powershell-sample-create-vm-from-snapshot.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) | Hiermee maakt u een virtuele machine op basis van een moment opname door eerst een beheerde schijf te maken op basis van de moment opname en vervolgens de nieuwe beheerde schijf te koppelen als besturingssysteem schijf. |
|**Opslag beheren**||
| [Een beheerde schijf maken op basis van een VHD in hetzelfde of een ander abonnement](../scripts/virtual-machines-windows-powershell-sample-create-managed-disk-from-vhd.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) | Hiermee maakt u een beheerde schijf van een speciale VHD als een besturingssysteem schijf, of van een gegevens-VHD als een gegevens schijf, in hetzelfde of een ander abonnement.  |
| [Een beheerde schijf maken op basis van een moment opname](../scripts/virtual-machines-windows-powershell-sample-create-managed-disk-from-snapshot.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) | Hiermee maakt u een beheerde schijf op basis van een moment opname. |
| [Een beheerde schijf kopiëren naar hetzelfde of een ander abonnement](../scripts/virtual-machines-windows-powershell-sample-copy-managed-disks-to-same-or-different-subscription.md?toc=%2fcli%2fmodule%2ftoc.json) | Hiermee wordt een beheerde schijf gekopieerd naar hetzelfde of een ander abonnement dat zich in dezelfde regio bevindt als de bovenliggende beheerde schijf.
| [Een moment opname exporteren als VHD naar een opslag account](../scripts/virtual-machines-windows-powershell-sample-copy-snapshot-to-storage-account.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) | Exporteert een beheerde moment opname als VHD naar een opslag account in een andere regio. |
| [De VHD van een beheerde schijf exporteren naar een opslag account](../scripts/virtual-machines-windows-powershell-sample-copy-managed-disks-vhd.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) | Exporteert de onderliggende VHD van een beheerde schijf naar een opslag account in een andere regio. |
| [Een moment opname maken van een VHD](../scripts/virtual-machines-windows-powershell-sample-create-snapshot-from-vhd.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) | Hiermee maakt u een moment opname van een VHD en gebruikt u die moment opname om snel meerdere identieke beheerde schijven te maken.  |
| [Een moment opname naar hetzelfde of een ander abonnement kopiëren](../scripts/virtual-machines-windows-powershell-sample-copy-snapshot-to-same-or-different-subscription.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) | De moment opname wordt gekopieerd naar hetzelfde of een ander abonnement dat zich in dezelfde regio bevindt als de bovenliggende moment opname. |
|**Virtuele machines beveiligen**||
| [Een virtuele machine en de bijbehorende gegevens schijven versleutelen](./../scripts/virtual-machines-windows-powershell-sample-encrypt-vm.md?toc=%2fpowershell%2fazure%2ftoc.json) | Hiermee maakt u een Azure-sleutel kluis, een versleutelings sleutel en een Service-Principal, en versleutelt u een virtuele machine. |
|**Virtuele machines bewaken**||
| [Een virtuele machine met Azure Monitor bewaken](./../scripts/virtual-machines-windows-powershell-sample-create-vm-oms.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) | Hiermee maakt u een virtuele machine, installeert u de Azure Log Analytics-agent en registreert u de VM in een Log Analytics-werk ruimte.  |
| [Details verzamelen over alle virtuele machines in een abonnement met Power shell](../scripts/virtual-machines-powershell-sample-collect-vm-details.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) | Hiermee maakt u een CSV met de VM-naam, de naam van de resource groep, de regio, de Virtual Network, het subnet, het privé-IP-adres, het type besturings systeem en het open bare IP-adres van de virtuele machines in het abonnement.
| | |
