---
title: Virtuele Azure-machines migreren naar Managed Disks
description: Virtuele Azure-machines die zijn gemaakt met onbeheerde schijven in opslag accounts migreren om Managed Disks te gebruiken.
author: roygara
ms.service: virtual-machines-windows
ms.topic: conceptual
ms.date: 05/30/2019
ms.author: rogarana
ms.subservice: disks
ms.openlocfilehash: 7b907cf5bf64aa1a8a458a8d2b42c30dffd0dfd1
ms.sourcegitcommit: 49cf9786d3134517727ff1e656c4d8531bbbd332
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/13/2019
ms.locfileid: "74029799"
---
# <a name="migrate-azure-vms-to-managed-disks-in-azure"></a>Virtuele Azure-machines migreren naar Managed Disks in azure

Azure Managed Disks vereenvoudigt uw opslag beheer door de nood zaak om opslag accounts afzonderlijk te beheren, te verwijderen.  U kunt ook uw bestaande Azure-Vm's migreren naar Managed Disks om te profiteren van betere betrouw baarheid van Vm's in een Beschikbaarheidsset. Zo zorgt u ervoor dat de schijven van verschillende virtuele machines in een Beschikbaarheidsset voldoende van elkaar zijn geïsoleerd om één storings punt te voor komen. Er worden automatisch schijven van verschillende virtuele machines in een Beschikbaarheidsset in verschillende opslag schaal eenheden (stempels) geplaatst, waardoor de impact van de enkelvoudige opslag schaal eenheid wordt beperkt als gevolg van hardware-en software fouten.
Op basis van uw behoeften kunt u kiezen uit vier typen opslag opties. Voor meer informatie over de beschik bare schijf typen raadpleegt u ons artikel [een schijf type selecteren](disks-types.md)

## <a name="migration-scenarios"></a>Migratie scenario's

In de volgende scenario's kunt u migreren naar Managed Disks:

|Scenario  |Artikel  |
|---------|---------|
|Zelfstandige virtuele machines en virtuele machines in een beschikbaarheidsset naar beheerde schijven converteren     |[Vm's converteren voor het gebruik van beheerde schijven](convert-unmanaged-to-managed-disks.md)         |
|Eén VM van het klassieke naar het Resource Manager-model op beheerde schijven converteren     |[Een virtuele machine maken op basis van een klassieke VHD](create-vm-specialized-portal.md)         |
|Alle virtuele machines in een vNet converteren van klassiek naar Resource Manager op beheerde schijven     |[Migreer IaaS-resources van klassiek naar Resource Manager](migration-classic-resource-manager-ps.md) en [Converteer vervolgens een virtuele machine van niet-beheerde schijven naar beheerde schijven](convert-unmanaged-to-managed-disks.md)         |
|Vm's upgraden met standaard niet-beheerde schijven naar virtuele machines met beheerde Premium-schijven     | Converteer eerst [een virtuele Windows-machine van niet-beheerde schijven naar beheerde schijven](convert-unmanaged-to-managed-disks.md). [Werk vervolgens het opslag type van een beheerde schijf bij](convert-disk-storage.md).         |

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over [Managed disks](managed-disks-overview.md)
- Bekijk de [prijzen voor Managed disks](https://azure.microsoft.com/pricing/details/managed-disks/).
