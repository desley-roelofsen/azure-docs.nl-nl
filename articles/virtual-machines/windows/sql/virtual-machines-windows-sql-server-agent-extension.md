---
title: Beheer taken automatiseren met de uitbrei ding IaaS agent
description: In dit artikel wordt beschreven hoe u de SQL Server IaaS agent-extensie beheert, waarmee specifieke SQL Server beheer taken worden geautomatiseerd. Dit zijn onder andere automatische back-ups, automatische patching en integratie van Azure Key Vault.
services: virtual-machines-windows
documentationcenter: ''
author: MashaMSFT
manager: jroth
editor: ''
tags: azure-resource-manager
ms.assetid: effe4e2f-35b5-490a-b5ef-b06746083da4
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 08/30/2019
ms.author: mathoma
ms.reviewer: jroth
ms.custom: seo-lt-2019
ms.openlocfilehash: 9aae386e21df6711fc4984a7abfd34f418399f76
ms.sourcegitcommit: 49cf9786d3134517727ff1e656c4d8531bbbd332
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/13/2019
ms.locfileid: "74034203"
---
# <a name="automate-management-tasks-on-azure-virtual-machines-by-using-the-sql-server-iaas-agent-extension"></a>Beheer taken automatiseren op virtuele machines van Azure met behulp van de SQL Server IaaS agent-extensie
> [!div class="op_single_selector"]
> * [Resource Manager](virtual-machines-windows-sql-server-agent-extension.md)
> * [Klassiek](../sqlclassic/virtual-machines-windows-classic-sql-server-agent-extension.md)

De SQL Server IaaS Agent-extensie (SqlIaasExtension) wordt uitgevoerd op virtuele Azure-machines beheertaken te automatiseren. Dit artikel bevat een overzicht van de services die door de extensie worden ondersteund. Dit artikel bevat ook instructies voor de installatie, status en verwijdering van de uitbrei ding.

[!INCLUDE [learn-about-deployment-models](../../../../includes/learn-about-deployment-models-rm-include.md)]

Als u de klassieke versie van dit artikel wilt bekijken, raadpleegt u [SQL Server IaaS agent extension for SQL Server vm's (klassiek)](../sqlclassic/virtual-machines-windows-classic-sql-server-agent-extension.md).


## <a name="supported-services"></a>Ondersteunde services
De SQL Server IaaS agent-extensie ondersteunt de volgende beheer taken:

| Beheer functie | Beschrijving |
| --- | --- |
| **Automatische back-up SQL Server** |Automatiseert het plannen van back-ups voor alle data bases voor het standaard exemplaar of een [correct geïnstalleerd](virtual-machines-windows-sql-server-iaas-faq.md#administration) benoemd exemplaar van SQL Server op de virtuele machine. Zie [automatische back-up voor SQL Server in azure virtual machines (Resource Manager)](virtual-machines-windows-sql-automated-backup.md)voor meer informatie. |
| **Automatische patching SQL Server** |Hiermee configureert u een onderhouds venster waarin belang rijke Windows-updates voor uw virtuele machine kunnen worden uitgevoerd, zodat u updates kunt voor komen tijdens piek tijden voor uw werk belasting. Zie voor meer informatie [automatische patching voor SQL Server in azure virtual machines (Resource Manager)](virtual-machines-windows-sql-automated-patching.md). |
| **Integratie van Azure Key Vault** |Hiermee kunt u Azure Key Vault automatisch installeren en configureren op uw SQL Server-VM. Zie [Azure Key Vault-integratie configureren voor SQL Server op Azure virtual machines (Resource Manager)](virtual-machines-windows-ps-sql-keyvault.md)voor meer informatie. |

Nadat de uitbrei ding voor de SQL Server IaaS-agent is geïnstalleerd en wordt uitgevoerd, worden de beheer functies beschikbaar:

* In het SQL Server paneel van de virtuele machine in de Azure Portal en via Azure PowerShell voor SQL Server installatie kopieën op Azure Marketplace.
* Via Azure PowerShell voor hand matige installaties van de uitbrei ding. 

## <a name="prerequisites"></a>Vereisten
Hier volgen de vereisten voor het gebruik van de SQL Server IaaS agent-extensie op uw VM:

**Besturingssysteem**:

* Windows Server 2008 R2
* Windows Server 2012
* Windows Server 2012 R2
* Windows Server 2016
* Windows Server 2019 

**SQL Server versie**:

* SQL Server 2008 
* SQL Server 2008 R2
* SQL Server 2012
* SQL Server 2014
* SQL Server 2016
* SQL Server 2017

**Azure PowerShell**:

* [De meest recente Azure PowerShell-opdrachten downloaden en configureren](/powershell/azure/overview)

[!INCLUDE [updated-for-az.md](../../../../includes/updated-for-az.md)]


##  <a name="installation"></a>Installeren
De SQL Server IaaS-extensie wordt geïnstalleerd wanneer u uw SQL Server virtuele machine registreert bij de [resource provider](virtual-machines-windows-sql-register-with-resource-provider.md)van de SQL-VM. Indien nodig kunt u de SQL Server IaaS-agent hand matig installeren met behulp van de onderstaande Power shell-opdracht: 

  ```powershell-interactive
    Set-AzVMExtension -ResourceGroupName "<ResourceGroupName>" `
    -Location "<VMLocation>" -VMName "<VMName>" `
    -Name "SqlIaasExtension" -Publisher "Microsoft.SqlServer.Management" `
    -ExtensionType "SqlIaaSAgent" -TypeHandlerVersion "2.0";  
  ```

> [!NOTE]
> Als u de uitbrei ding installeert, wordt de SQL Server-service opnieuw gestart. 


### <a name="install-on-a-vm-with-a-single-named-sql-server-instance"></a>Installeren op een virtuele machine met één benoemde SQL Server instantie
De uitbrei ding SQL Server IaaS werkt met een benoemd exemplaar op SQL Server als het standaard exemplaar wordt verwijderd en de uitbrei ding IaaS opnieuw wordt geïnstalleerd.

Als u een benoemd exemplaar van SQL Server wilt gebruiken, volgt u deze stappen:
   1. Implementeer een SQL Server-VM vanuit Azure Marketplace. 
   1. Verwijder de IaaS-extensie uit het [Azure Portal](https://portal.azure.com).
   1. Verwijder SQL Server volledig in de SQL Server VM.
   1. Installeer SQL Server met een benoemd exemplaar binnen de SQL Server VM. 
   1. Installeer de IaaS-extensie vanuit het Azure Portal.  


## <a name="get-the-status-of-the-sql-server-iaas-extension"></a>De status van de SQL Server IaaS-extensie ophalen
Een manier om te controleren of de uitbrei ding is geïnstalleerd, is de agent status weer geven in de Azure Portal. Selecteer **alle instellingen** in het venster virtuele machine en selecteer vervolgens **uitbrei dingen**. De uitbrei ding **SqlIaasExtension** wordt weer gegeven.

![Status van de uitbrei ding van de SQL Server IaaS-agent in de Azure Portal](./media/virtual-machines-windows-sql-server-agent-extension/azure-rm-sql-server-iaas-agent-portal.png)

U kunt ook de cmdlet **Get-AzVMSqlServerExtension** Azure PowerShell gebruiken:

   ```powershell-interactive
   Get-AzVMSqlServerExtension -VMName "vmname" -ResourceGroupName "resourcegroupname"
   ```

Met de vorige opdracht wordt bevestigd dat de agent is geïnstalleerd en algemene status informatie bevat. U krijgt specifieke status informatie over automatische back-ups en patching met behulp van de volgende opdrachten:

   ```powershell-interactive
    $sqlext = Get-AzVMSqlServerExtension -VMName "vmname" -ResourceGroupName "resourcegroupname"
    $sqlext.AutoPatchingSettings
    $sqlext.AutoBackupSettings
   ```

## <a name="removal"></a>Procedure
In de Azure Portal, kunt u de uitbrei ding verwijderen door het beletsel teken te selecteren in het venster **extensies** van de eigenschappen van de virtuele machine. Selecteer vervolgens **Verwijderen**.

![De uitbrei ding van de SQL Server IaaS-agent in Azure Portal verwijderen](./media/virtual-machines-windows-sql-server-agent-extension/azure-rm-sql-server-iaas-agent-uninstall.png)

U kunt ook de Power shell **-cmdlet Remove-AzVMSqlServerExtension** gebruiken:

   ```powershell-interactive
    Remove-AzVMSqlServerExtension -ResourceGroupName "resourcegroupname" -VMName "vmname" -Name "SqlIaasExtension"
   ```

## <a name="next-steps"></a>Volgende stappen
Begin met het gebruik van een van de services die door de extensie worden ondersteund. Zie de artikelen waarnaar wordt verwezen in de sectie [ondersteunde services](#supported-services) van dit artikel voor meer informatie.

Zie voor meer informatie over het uitvoeren van SQL Server op Azure Virtual Machines de [Wat is SQL Server op azure virtual machines?](virtual-machines-windows-sql-server-iaas-overview.md).
