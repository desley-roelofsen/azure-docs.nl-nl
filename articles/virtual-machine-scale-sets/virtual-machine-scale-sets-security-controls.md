---
title: Beveiligings controles voor Azure Virtual Machine Scale Sets
description: Een controle lijst met beveiligings controles voor het evalueren van Azure Virtual Machine Scale Sets
services: virtual-machine-scale-sets
ms.service: virtual-machine-scale-sets
documentationcenter: ''
author: msmbaldwin
manager: rkarlin
ms.topic: conceptual
ms.date: 09/05/2019
ms.author: mbaldwin
ms.openlocfilehash: 6aee9edaf3c594a2e7083416726da8e57bbf5c6b
ms.sourcegitcommit: f29fec8ec945921cc3a89a6e7086127cc1bc1759
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/17/2019
ms.locfileid: "72529151"
---
# <a name="security-controls-for-azure-virtual-machine-scale-sets"></a>Beveiligings controles voor Azure Virtual Machine Scale Sets

In dit artikel worden de beveiligings besturings elementen gedocumenteerd die zijn ingebouwd in azure Virtual Machine Scale Sets.

[!INCLUDE [Security controls header](../../includes/security-controls-header.md)]

## <a name="network"></a>Netwerk

| Beveiligings beheer | Ja/Nee | Opmerkingen |
|---|---|--|
| Ondersteuning voor service-eind punten| Ja | |
| Ondersteuning voor VNet-injectie| Ja | |
| Ondersteuning voor netwerk isolatie en firewalling| Ja |  |
| Ondersteuning voor geforceerde tunneling| Ja | Zie [geforceerde tunneling configureren met het Azure Resource Manager-implementatie model](/azure/vpn-gateway/vpn-gateway-forced-tunneling-rm). |

## <a name="monitoring--logging"></a>& Logboek registratie controleren

| Beveiligings beheer | Ja/Nee | Opmerkingen|
|---|---|--|
| Ondersteuning voor Azure-bewaking (log Analytics, app Insights, enz.)| Ja | Zie [een virtuele Linux-machine bewaken en bijwerken in azure](/azure/virtual-machines/linux/tutorial-monitoring) en [een virtuele Windows-machine bewaken en bijwerken in azure](/azure/virtual-machines/windows/tutorial-monitoring). |
| Logboek registratie en controle op het vlak van controle en beheer| Ja |  |
| Logboek registratie en controle van het gegevens vlak | Nee |  |

## <a name="identity"></a>Identity

| Beveiligings beheer | Ja/Nee | Opmerkingen|
|---|---|--|
| Verificatie| Ja |  |
| Autorisatie| Ja |  |

## <a name="data-protection"></a>Databeveiliging

| Beveiligings beheer | Ja/Nee | Opmerkingen |
|---|---|--|
| Versleuteling aan server zijde op rest: door micro soft beheerde sleutels | Ja | Zie [Azure Disk Encryption voor Virtual Machine Scale sets](disk-encryption-overview.md). |
| Versleuteling in transit (zoals ExpressRoute-versleuteling, in VNet-versleuteling en VNet-VNet-versleuteling)| Ja | Azure Virtual Machines ondersteunt [ExpressRoute](/azure/expressroute) -en VNet-versleuteling. Zie [in-transit versleuteling in vm's](/azure/security/security-azure-encryption-overview#in-transit-encryption-in-vms). |
| Versleuteling aan server zijde op rest: door de klant beheerde sleutels (BYOK) | Ja | Door de klant beheerde sleutels is een ondersteund Azure-versleutelings scenario. Zie [Azure Disk Encryption voor Virtual Machine Scale sets](disk-encryption-overview.md)|
| Versleuteling op kolom niveau (Azure Data Services)| N/A | |
| Versleutelde API-aanroepen| Ja | Via HTTPS en SSL. |

## <a name="configuration-management"></a>Configuratiebeheer

| Beveiligings beheer | Ja/Nee | Opmerkingen|
|---|---|--|
| Ondersteuning voor configuratie beheer (versie van configuratie, enz.)| Ja |  | 

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over de [ingebouwde beveiligings controles in Azure-Services](../security/fundamentals/security-controls.md).