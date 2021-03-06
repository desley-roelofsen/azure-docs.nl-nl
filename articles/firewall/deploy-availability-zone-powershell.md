---
title: Azure Firewall implementeren met Beschikbaarheidszones met behulp van Power shell
description: In dit artikel leert u hoe u een Azure Firewall implementeert met Beschikbaarheidszones met behulp van de Azure PowerShell.
services: firewall
author: vhorne
ms.service: firewall
ms.topic: article
ms.date: 11/19/2019
ms.author: victorh
ms.openlocfilehash: 33dcebf14f4d534962783a30ec94f7ff6529ae0d
ms.sourcegitcommit: 8e31a82c6da2ee8dafa58ea58ca4a7dd3ceb6132
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/19/2019
ms.locfileid: "74195914"
---
# <a name="deploy-an-azure-firewall-with-availability-zones-using-azure-powershell"></a>Een Azure Firewall implementeren met Beschikbaarheidszones met behulp van Azure PowerShell

Azure Firewall kunnen tijdens de implementatie worden geconfigureerd om meerdere Beschikbaarheidszones voor een hogere Beschik baarheid te beslaan.

Deze functie biedt de volgende scenario's:

- U kunt de beschik baarheid verhogen tot 99,99% uptime. Zie de Azure Firewall- [Service Level Agreement (Sla)](https://azure.microsoft.com/support/legal/sla/azure-firewall/v1_0/)voor meer informatie. De SLA voor de uptime van 99,99% wordt aangeboden wanneer er twee of meer Beschikbaarheidszones zijn geselecteerd.
- U kunt Azure Firewall ook koppelen aan een specifieke zone, net om redenen voor nabijheid, met behulp van Service Standard 99,95% SLA.

Zie [Wat is Azure firewall?](overview.md) voor meer informatie over Azure firewall Beschikbaarheidszones.

In het volgende Azure PowerShell voor beeld ziet u hoe u een Azure Firewall kunt implementeren met Beschikbaarheidszones.

## <a name="create-a-firewall-with-availability-zones"></a>Een firewall maken met Beschikbaarheidszones

In dit voor beeld wordt een firewall gemaakt in zones 1, 2 en 3.

Wanneer het standaard open bare IP-adres wordt gemaakt, wordt er geen specifieke zone opgegeven. Hiermee maakt u standaard een zone-redundant IP-adres. Standaard open bare IP-adressen kunnen in alle zones of in één zone worden geconfigureerd.

Het is belang rijk te weten dat u geen firewall hebt in zone 1 en een IP-adres in zone 2. Maar u kunt een firewall hebben in zone 1 en IP-adres in alle zones, of een firewall en een IP-adres in dezelfde zone voor proximity-doel einden.

```azurepowershell
$rgName = "resourceGroupName"

$vnet = Get-AzVirtualNetwork `
  -Name "vnet" `
  -ResourceGroupName $rgName

$pip1 = New-AzPublicIpAddress `
  -Name "AzFwPublicIp1" `
  -ResourceGroupName "rg" `
  -Sku "Standard" `
  -Location "centralus" `
  -AllocationMethod Static

New-AzFirewall `
  -Name "azFw" `
  -ResourceGroupName $rgName `
  -Location centralus `
  -VirtualNetwork $vnet `
  -PublicIpAddress @($pip1)
  -Zone 1,2,3
```

## <a name="next-steps"></a>Volgende stappen

- [Zelfstudie: Azure Firewall-logboeken bewaken](./tutorial-diagnostics.md)
