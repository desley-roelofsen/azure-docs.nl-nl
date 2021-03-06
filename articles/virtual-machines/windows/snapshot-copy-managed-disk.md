---
title: Een moment opname van een VHD maken in azure
description: Meer informatie over het maken van een kopie van een virtuele Azure-machine die u kunt gebruiken als back-up of voor problemen oplossen.
documentationcenter: ''
author: roygara
manager: twooley
editor: ''
tags: azure-resource-manager
ms.assetid: 15eb778e-fc07-45ef-bdc8-9090193a6d20
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.topic: article
ms.date: 10/08/2018
ms.author: rogarana
ms.subservice: disks
ms.openlocfilehash: b564e20ca8aa5acd7fbd4ea69ac2b1cd72e66d5e
ms.sourcegitcommit: a107430549622028fcd7730db84f61b0064bf52f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/14/2019
ms.locfileid: "74075340"
---
# <a name="create-a-snapshot"></a>Een momentopname maken

Een moment opname is een volledige, alleen-lezen kopie van een virtuele harde schijf (VHD). U kunt een moment opname maken van een besturings systeem of gegevens schijf VHD om te gebruiken als back-up, of om problemen met virtuele machine (VM) op te lossen.

Als u de moment opname wilt gebruiken om een nieuwe virtuele machine te maken, kunt u het beste de virtuele machine op een schone manier afsluiten voordat u een moment opname maakt, zodat alle processen die worden uitgevoerd, worden gewist.

## <a name="use-the-azure-portal"></a>De Azure-portal gebruiken 

1. Meld u aan bij de [Azure Portal](https://portal.azure.com).
2. Selecteer in het menu links de optie **een resource maken**en zoek naar en selecteer **moment opname**.
3. Selecteer in het venster **moment opname** **maken**. Het venster **moment opname maken** wordt weer gegeven.
4. Voer een **naam** in voor de moment opname.
5. Selecteer een bestaande [resource groep](../../azure-resource-manager/resource-group-overview.md#resource-groups) of voer de naam van een nieuwe in. 
6. Selecteer de **Locatie** van een Azure-datacenter.  
7. Voor de **bron schijf**selecteert u de beheerde schijf voor de moment opname.
8. Het **account type** selecteren dat moet worden gebruikt voor het opslaan van de moment opname. Selecteer **Standard_HDD**, tenzij u wilt dat de moment opname wordt opgeslagen op een hoogwaardige schijf.
9. Selecteer **Maken**.

## <a name="use-powershell"></a>PowerShell gebruiken

De volgende stappen laten zien hoe u de VHD-schijf kopieert, de configuratie van de moment opname maakt en een moment opname van de schijf gebruikt met behulp van de cmdlet [New-AzSnapshot](https://docs.microsoft.com/powershell/module/az.compute/new-azsnapshot) . 

 

1. Stel een aantal para meters in: 

   ```azurepowershell-interactive
   $resourceGroupName = 'myResourceGroup' 
   $location = 'eastus' 
   $vmName = 'myVM'
   $snapshotName = 'mySnapshot'  
   ```

2. De VM ophalen:

   ```azurepowershell-interactive
   $vm = get-azvm `
   -ResourceGroupName $resourceGroupName 
   -Name $vmName
   ```

3. De configuratie van de moment opname maken. Voor dit voor beeld is de moment opname van de besturingssysteem schijf:

   ```azurepowershell-interactive
   $snapshot =  New-AzSnapshotConfig 
   -SourceUri $vm.StorageProfile.OsDisk.ManagedDisk.Id 
   -Location $location 
   -CreateOption copy
   ```
   
   > [!NOTE]
   > Als u uw moment opname wilt opslaan in zone-flexibele opslag, maakt u deze in een regio die [beschikbaarheids zones](../../availability-zones/az-overview.md) ondersteunt en de para meter `-SkuName Standard_ZRS` bevatten.   
   
4. De moment opname maken:

   ```azurepowershell-interactive
   New-AzSnapshot 
   -Snapshot $snapshot 
   -SnapshotName $snapshotName 
   -ResourceGroupName $resourceGroupName 
   ```


## <a name="next-steps"></a>Volgende stappen

Een virtuele machine maken op basis van een moment opname door een beheerde schijf te maken op basis van een moment opname en vervolgens de nieuwe beheerde schijf als de besturingssysteem schijf te koppelen. Zie het voor beeld in [een VM maken van een moment opname met Power shell](./../scripts/virtual-machines-windows-powershell-sample-create-vm-from-snapshot.md?toc=%2fpowershell%2fmodule%2ftoc.json)voor meer informatie.
