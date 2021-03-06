---
title: Overzicht van de agent voor virtuele Azure-machines
description: Overzicht van de agent voor virtuele Azure-machines
services: virtual-machines-windows
documentationcenter: virtual-machines
author: axayjo
manager: gwallace
editor: tysonn
tags: azure-resource-manager
ms.assetid: 0a1f212e-053e-4a39-9910-8d622959f594
ms.service: virtual-machines-windows
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 07/20/2019
ms.author: akjosh
ms.openlocfilehash: b003f2823ffceebecdb2af681a3bdbb4cf25704c
ms.sourcegitcommit: 003e73f8eea1e3e9df248d55c65348779c79b1d6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/02/2020
ms.locfileid: "75615075"
---
# <a name="azure-virtual-machine-agent-overview"></a>Overzicht van de agent voor virtuele Azure-machines
De Microsoft Azure-agent van de virtuele machine (VM-agent) is een veilig, licht gewicht proces dat interactie van de virtuele machine (VM) beheert met de Azure Fabric-controller. De VM-agent heeft een primaire rol bij het inschakelen en uitvoeren van extensies van virtuele Azure-machines. VM-extensies maken de configuratie van de na de implementatie van de VM mogelijk, zoals het installeren en configureren van software. VM-extensies bieden ook herstel functies, zoals het opnieuw instellen van het beheerders wachtwoord van een virtuele machine. Zonder de VM-agent van Azure kunnen VM-extensies niet worden uitgevoerd.

In dit artikel wordt de installatie, detectie en verwijdering van de Azure virtual machine-agent beschreven.

## <a name="install-the-vm-agent"></a>De VM-agent installeren

### <a name="azure-marketplace-image"></a>Azure Marketplace-installatie kopie

De Azure VM-agent wordt standaard geïnstalleerd op een virtuele Windows-machine die is geïmplementeerd vanuit een Azure Marketplace-installatie kopie. Wanneer u een installatie kopie van Azure Marketplace implementeert vanuit de portal, Power shell, de opdracht regel interface of een Azure Resource Manager sjabloon, wordt de Azure VM-agent ook geïnstalleerd.

Het pakket voor de Windows-gast agent is onderverdeeld in twee delen:

- Inrichtings agent (PA)
- Windows-gast agent (Vleugela)

Als u een virtuele machine wilt opstarten, moet u de PA geïnstalleerd hebben op de virtuele machine. de vleugels hoeven echter niet te worden geïnstalleerd. Op de implementatie tijd van de VM kunt u de Vleugela niet installeren. In het volgende voor beeld ziet u hoe u de optie *provisionVmAgent* selecteert met een Azure Resource Manager sjabloon:

```json
"resources": [{
"name": "[parameters('virtualMachineName')]",
"type": "Microsoft.Compute/virtualMachines",
"apiVersion": "2016-04-30-preview",
"location": "[parameters('location')]",
"dependsOn": ["[concat('Microsoft.Network/networkInterfaces/', parameters('networkInterfaceName'))]"],
"properties": {
    "osProfile": {
    "computerName": "[parameters('virtualMachineName')]",
    "adminUsername": "[parameters('adminUsername')]",
    "adminPassword": "[parameters('adminPassword')]",
    "windowsConfiguration": {
        "provisionVmAgent": "false"
}
```

Als u de agents niet hebt geïnstalleerd, kunt u bepaalde Azure-Services, zoals Azure Backup of Azure-beveiliging, niet gebruiken. Deze services vereisen een uitbrei ding die moet worden geïnstalleerd. Als u een virtuele machine zonder Vleugela hebt geïmplementeerd, kunt u later de meest recente versie van de agent installeren.

### <a name="manual-installation"></a>Handmatige installatie
De Windows VM-agent kan hand matig worden geïnstalleerd met een Windows Installer-pakket. Hand matige installatie kan nodig zijn wanneer u een aangepaste VM-installatie kopie maakt die is geïmplementeerd in Azure. Als u de Windows VM-agent hand matig wilt installeren, [downloadt u het installatie programma van de VM-agent](https://go.microsoft.com/fwlink/?LinkID=394789). De VM-agent wordt ondersteund op Windows Server 2008 R2 of hoger.

### <a name="prerequisites"></a>Vereisten
De Windows VM-agent moet ten minste Windows Server 2008 R2 (64-bits) uitvoeren, met .NET Framework 4,0. Bekijk de [minimale versie ondersteuning voor Virtual Machine agents in azure](https://support.microsoft.com/en-us/help/4049215/extensions-and-virtual-machine-agent-minimum-version-support)

## <a name="detect-the-vm-agent"></a>De VM-agent detecteren

### <a name="powershell"></a>PowerShell

De Azure Resource Manager Power shell-module kan worden gebruikt om informatie over virtuele Azure-machines op te halen. Als u informatie wilt weer geven over een virtuele machine, zoals de inrichtings status van de Azure VM-agent, gebruikt u [Get-AzVM](https://docs.microsoft.com/powershell/module/az.compute/get-azvm):

```powershell
Get-AzVM
```

In de volgende gecomprimeerde voorbeeld uitvoer ziet u de eigenschap *ProvisionVMAgent* geneste in *OSProfile*. Deze eigenschap kan worden gebruikt om te bepalen of de VM-agent is geïmplementeerd op de VM:

```powershell
OSProfile                  :
  ComputerName             : myVM
  AdminUsername            : myUserName
  WindowsConfiguration     :
    ProvisionVMAgent       : True
    EnableAutomaticUpdates : True
```

Het volgende script kan worden gebruikt om een beknopt overzicht van de VM-namen en de status van de VM-agent te retour neren:

```powershell
$vms = Get-AzVM

foreach ($vm in $vms) {
    $agent = $vm | Select -ExpandProperty OSProfile | Select -ExpandProperty Windowsconfiguration | Select ProvisionVMAgent
    Write-Host $vm.Name $agent.ProvisionVMAgent
}
```

### <a name="manual-detection"></a>Hand matige detectie

Wanneer u bent aangemeld bij een Windows-VM, kan taak beheer worden gebruikt voor het onderzoeken van actieve processen. Als u wilt controleren of de Azure VM-agent is, opent u taak beheer, klikt u op het tabblad *Details* en zoekt u naar een proces naam **WindowsAzureGuestAgent. exe**. De aanwezigheid van dit proces geeft aan dat de VM-agent is geïnstalleerd.


## <a name="upgrade-the-vm-agent"></a>De VM-agent bijwerken
De Azure VM-agent voor Windows wordt automatisch bijgewerkt. Wanneer er nieuwe virtuele machines worden geïmplementeerd in azure, ontvangen ze de nieuwste VM-agent op de VM-inrichtings tijd. Aangepaste VM-installatie kopieën moeten hand matig worden bijgewerkt met de nieuwe VM-agent op het moment waarop de installatie kopie wordt gemaakt.

## <a name="windows-guest-agent-automatic-logs-collection"></a>Verzameling automatische logboeken voor Windows-gast agent
Windows Guest agent heeft een functie voor het automatisch verzamelen van Logboeken. Deze functie is controller door het proces CollectGuestLogs. exe. Deze bestaat zowel voor PaaS Cloud Services als IaaS Virtual Machines en het doel is om & snel een aantal Diagnostische logboeken te verzamelen van een VM, zodat deze kunnen worden gebruikt voor offline analyse. De verzamelde logboeken zijn gebeurtenis logboeken, logboeken van het besturings systeem, Azure-logboeken en bepaalde register sleutels. Er wordt een ZIP-bestand gegenereerd dat wordt overgedragen naar de host van de virtuele machine. Dit ZIP-bestand kan vervolgens worden bekeken door technische teams en ondersteunings medewerkers om problemen te onderzoeken op verzoek van de klant die eigenaar is van de virtuele machine.

## <a name="next-steps"></a>Volgende stappen
Zie [overzicht van virtuele machines en functies van Azure](overview.md)voor meer informatie over VM-uitbrei dingen.
