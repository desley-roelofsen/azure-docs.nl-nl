---
title: Azure CLI-voorbeelden
description: Azure CLI-voorbeelden
services: virtual-machines-linux
documentationcenter: virtual-machines
author: cynthn
manager: gwallace
editor: tysonn
tags: azure-service-management
ms.assetid: ''
ms.service: virtual-machines-linux
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 03/01/2019
ms.author: cynthn
ms.custom: mvc
ms.openlocfilehash: eda3154bb921a46bbe3b768713d4e72f1cc45d5f
ms.sourcegitcommit: 49cf9786d3134517727ff1e656c4d8531bbbd332
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/13/2019
ms.locfileid: "74036850"
---
# <a name="azure-cli-samples-for-linux-virtual-machines"></a>Azure CLI-voor beelden voor virtuele Linux-machines

De volgende tabel bevat koppelingen naar bash-scripts die zijn gebouwd met behulp van de Azure CLI.

| | |
|---|---|
|**Virtuele machines maken**||
| [Maak een virtuele machine](./../scripts/virtual-machines-linux-cli-sample-create-vm-quick-create.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) | Hiermee maakt u een virtuele Linux-machine met een minimale configuratie. |
| [Een volledig geconfigureerde virtuele machine maken](./../scripts/virtual-machines-linux-cli-sample-create-vm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) | Hiermee maakt u een resource groep, een virtuele machine en alle gerelateerde resources.|
| [Maxi maal beschik bare virtuele machines maken](./../scripts/virtual-machines-linux-cli-sample-nlb.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) | Hiermee maakt u meerdere virtuele machines in een maximaal beschikbare en configuratie van taakverdeling. |
| [Een virtuele machine maken en een configuratie script uitvoeren](./../scripts/virtual-machines-linux-cli-sample-create-vm-nginx.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) | Hiermee maakt u een virtuele machine en gebruikt u de aangepaste script extensie van Azure om NGINX te installeren. |
| [Een virtuele machine maken waarop WordPress is geïnstalleerd](./../scripts/virtual-machines-linux-cli-sample-create-vm-wordpress.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) | Hiermee maakt u een virtuele machine en gebruikt u de aangepaste script extensie van Azure om WordPress te installeren. |
| [Een virtuele machine maken op basis van een beheerde besturingssysteem schijf](./../scripts/virtual-machines-linux-cli-sample-create-vm-from-managed-os-disks.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) | Hiermee maakt u een virtuele machine door een bestaande beheerde schijf te koppelen als besturingssysteem schijf. |
| [Een virtuele machine maken op basis van een moment opname](./../scripts/virtual-machines-linux-cli-sample-create-vm-from-snapshot.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) | Hiermee maakt u een virtuele machine op basis van een moment opname door eerst een beheerde schijf te maken op basis van een moment opname en vervolgens de nieuwe beheerde schijf te koppelen als besturingssysteem schijf. |
|**Opslag beheren**||
| [Een beheerde schijf maken op basis van een VHD](../scripts/virtual-machines-linux-cli-sample-create-managed-disk-from-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) | Hiermee maakt u een beheerde schijf van een speciale VHD als een besturingssysteem schijf of van een gegevens-VHD als gegevens schijf.  |
| [Een beheerde schijf maken op basis van een moment opname](../scripts/virtual-machines-linux-cli-sample-create-managed-disk-from-snapshot.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) | Hiermee maakt u een beheerde schijf op basis van een moment opname. |
| [Een beheerde schijf kopiëren naar hetzelfde of een ander abonnement](../scripts/virtual-machines-linux-cli-sample-copy-managed-disks-to-same-or-different-subscription.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) | Hiermee wordt de beheerde schijf gekopieerd naar hetzelfde of een ander abonnement, maar in dezelfde regio als de bovenliggende beheerde schijf. 
| [Een moment opname exporteren als VHD naar een opslag account](../scripts/virtual-machines-linux-cli-sample-copy-snapshot-to-storage-account.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) | Exporteert een beheerde moment opname als VHD naar een opslag account in een andere regio. |
| [De VHD van een beheerde schijf exporteren naar een opslag account](../scripts/virtual-machines-linux-cli-sample-copy-managed-disks-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) | Exporteert de onderliggende VHD van een beheerde schijf naar een opslag account in een andere regio. |
| [Moment opname kopiëren naar hetzelfde of een ander abonnement](../scripts/virtual-machines-linux-cli-sample-copy-snapshot-to-same-or-different-subscription.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) | De moment opname wordt gekopieerd naar hetzelfde of een ander abonnement, maar in dezelfde regio als de bovenliggende moment opname. |
|**Virtuele netwerk machines**||
| [Netwerk verkeer tussen virtuele machines beveiligen](./../scripts/virtual-machines-linux-cli-sample-create-vm-nsg.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) | Maakt twee virtuele machines, alle gerelateerde resources en een interne en externe netwerk beveiligings groepen (NSG). |
|**Virtuele machines beveiligen**||
| [Een virtuele machine en gegevens schijven versleutelen](./../scripts/virtual-machines-linux-cli-sample-encrypt-vm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) | Maakt een Azure Key Vault, versleutelings sleutel en Service-Principal en versleutelt vervolgens een virtuele machine. |
|**Virtuele machines bewaken**||
| [Een virtuele machine bewaken met Azure Monitor-logboeken](./../scripts/virtual-machines-linux-cli-sample-create-vm-oms.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) | Hiermee maakt u een virtuele machine, installeert u de Log Analytics-agent en registreert u de VM in een Log Analytics-werk ruimte.  |
|**Problemen met virtuele machines oplossen**||
| [Problemen met de besturingssysteem schijf van een virtuele machine oplossen](./../scripts/virtual-machines-linux-cli-sample-mount-os-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) | Koppelt de besturingssysteem schijf van de ene VM als een gegevens schijf op een tweede virtuele machine. |
| | |
