---
title: Groottes van Azure Windows VM-GPU
description: Geeft een lijst van de verschillende geoptimaliseerde GPU-grootten die beschikbaar zijn voor virtuele Windows-machines in Azure. Bevat informatie over het aantal Vcpu's, gegevens schijven en Nic's en de opslag doorvoer en netwerk bandbreedte voor grootten in deze serie.
services: virtual-machines-windows
documentationcenter: ''
author: jonbeck7
manager: gwallace
editor: ''
tags: azure-resource-manager,azure-service-management
ms.assetid: ''
ms.service: virtual-machines-windows
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 06/11/2019
ms.author: jonbeck
ms.openlocfilehash: 5b2da3076f3970d2f11301b3ea412ce52c257111
ms.sourcegitcommit: a107430549622028fcd7730db84f61b0064bf52f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/14/2019
ms.locfileid: "74075355"
---
# <a name="gpu-optimized-virtual-machine-sizes"></a>Grootte van door GPU geoptimaliseerde virtuele machines

[!INCLUDE [virtual-machines-common-sizes-gpu](../../../includes/virtual-machines-common-sizes-gpu.md)]

[!INCLUDE [virtual-machines-common-sizes-table-defs](../../../includes/virtual-machines-common-sizes-table-defs.md)]

## <a name="supported-operating-systems-and-drivers"></a>Ondersteunde besturingssystemen en stuurprogramma’s

Als u gebruik wilt maken van de GPU-mogelijkheden van virtuele machines uit de Azure N-serie die worden uitgevoerd op Windows, moeten de NVIDIA GPU-Stuur Programma's zijn geïnstalleerd. Met de [uitbrei ding NVIDIA GPU-stuur programma](../extensions/hpccompute-gpu-windows.md) worden de juiste NVIDIA-CUDA of raster Stuur Programma's geïnstalleerd op een virtuele machine uit de N-serie. De uitbrei ding installeren of beheren met de Azure Portal of hulpprogram ma's, zoals Azure PowerShell of Azure Resource Manager sjablonen. Zie de [documentatie over NVIDIA GPU-Stuur Programma's](../extensions/hpccompute-gpu-windows.md) voor ondersteunde besturings systemen en implementaties tappen. Zie [extensies en functies van virtuele Azure-machines](../extensions/overview.md)voor algemene informatie over VM-extensies.

Als u ervoor kiest om de NVIDIA GPU-Stuur Programma's hand matig te installeren, raadpleegt u [het stuur programma voor de installatie van de N-Series GPU voor Windows](n-series-driver-setup.md) voor de ondersteunde besturings systemen, stuur Programma's en installatie-en verificatie stappen.

[!INCLUDE [virtual-machines-n-series-considerations](../../../includes/virtual-machines-n-series-considerations.md)]

## <a name="other-sizes"></a>Andere grootten
- [Algemeen doel](sizes-general.md)
- [Geoptimaliseerde rekenkracht](sizes-compute.md)
- [Krachtig rekenvermogen](sizes-hpc.md)
- [Geoptimaliseerd geheugen](sizes-memory.md)
- [Geoptimaliseerde opslag](sizes-storage.md)
- [Vorige generaties](sizes-previous-gen.md)

## <a name="next-steps"></a>Volgende stappen
Meer informatie over hoe [Azure Compute units (ACU)](acu.md) u kan helpen bij het vergelijken van de reken prestaties in azure-sku's.

