---
title: VMware nood herstel-vereisten voor de configuratie server in Azure Site Recovery
description: In dit artikel worden de ondersteuning en vereisten beschreven voor het implementeren van de configuratie server voor VMware nood herstel op Azure met Azure Site Recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
services: site-recovery
ms.topic: article
ms.date: 09/09/2019
ms.author: raynew
ms.openlocfilehash: 0b0942b517c8dc83c048bd1203a58d9861515dfb
ms.sourcegitcommit: 609d4bdb0467fd0af40e14a86eb40b9d03669ea1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/06/2019
ms.locfileid: "73663048"
---
# <a name="configuration-server-requirements-for-vmware-disaster-recovery-to-azure"></a>Vereisten voor de configuratie server voor VMware nood herstel naar Azure

U implementeert een on-premises configuratie server wanneer u [Azure site Recovery](site-recovery-overview.md) gebruikt voor herstel na nood gevallen van virtuele VMware-machines en fysieke servers naar Azure.

- De configuratie server coördineert de communicatie tussen on-premises VMware en Azure. Het beheert ook de gegevens replicatie.
- Meer [informatie](vmware-azure-architecture.md) over de onderdelen en processen van de configuratie server.

## <a name="configuration-server-deployment"></a>Implementatie van de configuratie server

Voor herstel na nood gevallen van virtuele VMware-machines naar Azure, implementeert u de configuratie server als een virtuele VMware-machine.

- Site Recovery biedt een eicellen-sjabloon die u downloadt van de Azure Portal en die u importeert in vCenter Server voor het instellen van de VM van de configuratie server.
- Wanneer u de configuratie server implementeert met behulp van de sjabloon van de eicellen, voldoet de virtuele machine automatisch aan de vereisten die in dit artikel worden vermeld.
- We raden u ten zeerste aan de configuratie server in te stellen met behulp van de eicellen-sjabloon. Als u echter nood herstel instelt voor VMware-Vm's en de eicellen-sjabloon niet kunt gebruiken, kunt u de configuratie server implementeren met behulp [van deze instructies](physical-azure-set-up-source.md).
- Als u de configuratie server implementeert voor herstel na nood gevallen van on-premises fysieke machines naar Azure, volgt u de instructies in [dit artikel](physical-azure-set-up-source.md). 

[!INCLUDE [site-recovery-configuration-server-requirements](../../includes/site-recovery-configuration-and-scaleout-process-server-requirements.md)]

## <a name="next-steps"></a>Volgende stappen
Stel herstel na nood gevallen van [virtuele VMware-machines](vmware-azure-tutorial.md) in op Azure.
