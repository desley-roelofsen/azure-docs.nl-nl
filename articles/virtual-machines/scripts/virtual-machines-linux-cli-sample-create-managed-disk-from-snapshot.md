---
title: 'Azure CLI-voorbeeldscript: een beheerde schijf maken op basis van een momentopname'
description: 'Azure CLI-voorbeeldscript: een beheerde schijf maken op basis van een momentopname'
services: virtual-machines-linux
documentationcenter: storage
author: ramankumarlive
manager: kavithag
editor: tysonn
tags: azure-service-management
ms.assetid: ''
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/19/2017
ms.author: ramankum
ms.custom: mvc
ms.openlocfilehash: 8a219b24c4d4b82363094ccba74db22e3b4018c4
ms.sourcegitcommit: 49cf9786d3134517727ff1e656c4d8531bbbd332
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/13/2019
ms.locfileid: "74023319"
---
# <a name="create-a-managed-disk-from-a-snapshot-with-cli"></a>Een beheerde schijf maken met CLI op basis van een momentopname

Met dit script maakt u een beheerde schijf op basis van een momentopname. Gebruik het script om een virtuele machine te herstellen op basis van momentopnamen van het besturingssysteem en gegevensschijven. Maak beheerde OS- en gegevensschijven op basis van de respectieve momentopnamen en maak vervolgens een nieuwe virtuele machine door beheerde schijven te koppelen. U kunt ook gegevensschijven van een bestaande virtuele machine herstellen door het koppelen van gegevensschijven die zijn gemaakt op basis van momentopnamen.


[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Voorbeeldscript

[!code-azurecli[main](../../../cli_scripts/virtual-machine/create-managed-disks-from-snapshot/create-managed-disks-from-snapshot.sh "Create managed disk from snapshot")]


## <a name="script-explanation"></a>Uitleg van het script

Dit script gebruikt de volgende opdrachten om een beheerde schijf te maken op basis van een momentopname. Elke opdracht in de tabel is een koppeling naar specifieke documentatie over de opdracht.

| Opdracht | Opmerkingen |
|---|---|
| [az snapshot show](https://docs.microsoft.com/cli/azure/snapshot) | Hiermee haalt u alle eigenschappen van een momentopname op aan de hand van de naam en de eigenschappen van de resourcegroep van de momentopname. Id-eigenschap wordt gebruikt om de beheerde schijf te maken.  |
| [az disk create](https://docs.microsoft.com/cli/azure/disk) | Hiermee maakt u een beheerde met behulp van de momentopname-id van een beheerde momentopname |

## <a name="next-steps"></a>Volgende stappen

[Een virtuele machine maken door een beheerde schijf te koppelen als besturingssysteemschijf](./virtual-machines-linux-cli-sample-create-vm-from-managed-os-disks.md?toc=%2fcli%2fmodule%2ftoc.json)

Zie de [documentatie van Azure CLI](https://docs.microsoft.com/cli/azure) voor meer informatie over de Azure CLI.

Aanvullende CLI-scriptvoorbeelden voor virtuele machines en beheerde schijven vindt u in de [Azure-documentatie voor Linux-VM's](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
