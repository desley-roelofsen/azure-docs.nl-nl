---
title: Automatische back-up v2 voor SQL Server 2016/2017 Azure-Vm's | Microsoft Docs
description: Hierin wordt de functie voor automatische back-ups beschreven voor SQL Server 2016/2017 Vm's die worden uitgevoerd in Azure. Dit artikel is specifiek voor Vm's met behulp van Resource Manager.
services: virtual-machines-windows
documentationcenter: na
author: MashaMSFT
manager: craigg
tags: azure-resource-manager
ms.assetid: ebd23868-821c-475b-b867-06d4a2e310c7
ms.service: virtual-machines-sql
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 05/03/2018
ms.author: mathoma
ms.reviewer: jroth
ms.openlocfilehash: 009a480add9d808115f24a69a400118fec7cb293
ms.sourcegitcommit: 76b48a22257a2244024f05eb9fe8aa6182daf7e2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/03/2019
ms.locfileid: "74790588"
---
# <a name="automated-backup-v2-for-azure-virtual-machines-resource-manager"></a>Automatische back-up v2 voor Azure Virtual Machines (Resource Manager)

> [!div class="op_single_selector"]
> * [SQL Server 2014](virtual-machines-windows-sql-automated-backup.md)
> * [SQL Server 2016/2017](virtual-machines-windows-sql-automated-backup-v2.md)

Met automatische back-up v2 wordt [beheerde back-up](https://msdn.microsoft.com/library/dn449496.aspx) automatisch geconfigureerd voor Microsoft Azure voor alle bestaande en nieuwe data bases op een virtuele Azure-machine met SQL Server 2016/2017 Standard-, Enter prise-of Developer-edities. Zo kunt u regel matige back-ups van de data base configureren die gebruikmaken van duurzame Azure Blob-opslag. Automatische back-up v2 is afhankelijk van de [uitbrei ding IaaS agent van SQL Server](virtual-machines-windows-sql-server-agent-extension.md).

[!INCLUDE [learn-about-deployment-models](../../../../includes/learn-about-deployment-models-rm-include.md)]

## <a name="prerequisites"></a>Vereisten
Als u automatische back-up v2 wilt gebruiken, controleert u de volgende vereisten:

**Besturings systeem**:

- Windows Server 2012 R2
- Windows Server 2016

**SQL Server versie/editie**:

- SQL Server 2016: Developer, Standard of ENTER prise
- SQL Server 2017: Developer, Standard of ENTER prise

> [!IMPORTANT]
> Automatische back-up v2 werkt met SQL Server 2016 of hoger. Als u SQL Server 2014 gebruikt, kunt u automatische back-up v1 gebruiken om een back-up van uw data bases te maken. Zie voor meer informatie [automatische back-up voor SQL Server 2014 Azure virtual machines](virtual-machines-windows-sql-automated-backup.md).

**Database configuratie**:

- Doel databases moeten het volledige herstel model gebruiken. Zie [back-up onder het volledige herstel model](https://technet.microsoft.com/library/ms190217.aspx)voor meer informatie over de impact van het volledige herstel model op back-ups.
- Systeem databases hoeven geen volledig herstel model te gebruiken. Als u echter wilt dat logboek back-ups moeten worden gemaakt voor model of MSDB, moet u het volledige herstel model gebruiken.
- Doel databases moeten zich bevinden op het standaard SQL Server-exemplaar of een [correct geïnstalleerd](virtual-machines-windows-sql-server-iaas-faq.md#administration) benoemd exemplaar. 

> [!NOTE]
> Automatische back-up is afhankelijk van de **SQL Server IaaS agent-extensie**. Huidige installatie kopieën van de virtuele machine van SQL-machines deze extensie standaard toevoegen. Zie [SQL Server IaaS agent extension](virtual-machines-windows-sql-server-agent-extension.md)(Engelstalig) voor meer informatie.

## <a name="settings"></a>Instellingen
In de volgende tabel worden de opties beschreven die kunnen worden geconfigureerd voor automatische back-ups v2. De werkelijke configuratie stappen variëren, afhankelijk van of u de Azure Portal-of Azure Windows Power shell-opdrachten gebruikt.

### <a name="basic-settings"></a>Basis instellingen

| Instelling | Bereik (standaard) | Beschrijving |
| --- | --- | --- |
| **Automatische back-up** | Inschakelen/uitschakelen (uitgeschakeld) | Hiermee wordt automatische back-ups voor een Azure-VM met SQL Server 2016/2017 Developer, Standard of ENTER prise in-of uitgeschakeld. |
| **Bewaar periode** | 1-30 dagen (30 dagen) | Het aantal dagen dat back-ups moeten worden bewaard. |
| **Opslagaccount** | Azure Storage-account | Een Azure-opslag account dat moet worden gebruikt voor het opslaan van automatische back-upbestanden in Blob Storage. Er wordt een container gemaakt op deze locatie om alle back-upbestanden op te slaan. De naamgevings regels voor back-upbestanden bevatten de datum, tijd en data base-GUID. |
| **Versleuteling** |Inschakelen/uitschakelen (uitgeschakeld) | Hiermee wordt versleuteling in-of uitgeschakeld. Wanneer versleuteling is ingeschakeld, bevinden de certificaten die worden gebruikt voor het herstellen van de back-up zich in het opgegeven opslag account. Deze maakt gebruik van dezelfde **automatische back-** upcontainer met dezelfde naam Conventie. Als het wacht woord wordt gewijzigd, wordt er een nieuw certificaat met dat wacht woord gegenereerd, maar blijft het oude certificaat voor het herstellen van eerdere back-ups. |
| **Wachtwoord** |Wachtwoord tekst | Een wacht woord voor versleutelings sleutels. Dit wacht woord is alleen vereist als versleuteling is ingeschakeld. Als u een versleutelde back-up wilt herstellen, moet u het juiste wacht woord en het bijbehorende certificaat hebben dat is gebruikt op het moment dat de back-up werd gemaakt. |

### <a name="advanced-settings"></a>Geavanceerde instellingen

| Instelling | Bereik (standaard) | Beschrijving |
| --- | --- | --- |
| **Back-ups van de systeem database** | Inschakelen/uitschakelen (uitgeschakeld) | Als deze functie is ingeschakeld, wordt ook een back-up gemaakt van de systeem databases: Master, MSDB en model. Controleer voor de MSDB-en model databases of ze zich in de modus voor volledig herstel bevinden als u wilt dat logboek back-ups worden gemaakt. Logboek back-ups worden nooit uitgevoerd voor de hoofd database. Er worden geen back-ups gemaakt voor TempDB. |
| **Back-upschema** | Hand matig/automatisch (automatisch) | Het back-upschema wordt standaard automatisch bepaald op basis van de logboek groei. Met hand matig back-upschema kan de gebruiker het tijd venster voor back-ups opgeven. In dit geval worden back-ups alleen uitgevoerd met de opgegeven frequentie en tijdens het opgegeven tijd venster van een bepaalde dag. |
| **Frequentie van volledige back-up** | Dagelijks/wekelijks | De frequentie van volledige back-ups. In beide gevallen beginnen volledige back-ups tijdens het volgende geplande tijd venster. Wanneer wekelijks is geselecteerd, kunnen back-ups meerdere dagen duren totdat alle data bases zijn voorzien van een back-up. |
| **Begin tijd volledige back-up** | 00:00 – 23:00 (01:00) | Begin tijd van een bepaalde dag waarin volledige back-ups kunnen worden uitgevoerd. |
| **Tijd venster volledige back-up** | 1 – 23 uur (1 uur) | De duur van het tijd venster van een bepaalde dag waarin volledige back-ups kunnen worden uitgevoerd. |
| **Frequentie van logboek back-up** | 5 – 60 minuten (60 minuten) | Frequentie van logboek back-ups. |

## <a name="understanding-full-backup-frequency"></a>Meer informatie over de frequentie van volledige back-ups
Het is belang rijk dat u begrijpt wat het verschil is tussen dagelijkse en wekelijkse volledige back-ups. Houd rekening met de volgende twee voorbeeld scenario's.

### <a name="scenario-1-weekly-backups"></a>Scenario 1: wekelijkse back-ups
U hebt een SQL Server-VM die een aantal grote data bases bevat.

Op maandag schakelt u automatische back-up v2 in met de volgende instellingen:

- Back-upschema: **hand matig**
- Frequentie van volledige back-up: **wekelijks**
- Begin tijd volledige back-up: **01:00**
- Tijd venster voor volledige back-up: **1 uur**

Dit betekent dat het volgende beschik bare venster voor back-ups dinsdag om 1 uur 's MIDDAGs is. Op dat moment wordt een back-up van uw data bases per keer met geautomatiseerde back-ups gemaakt. In dit scenario zijn uw data bases groot genoeg die volledige back-ups voor de eerste paar data bases zijn voltooid. Na één uur is er echter een back-up van de data bases gemaakt.

Als dit gebeurt, wordt met automatische back-up de volgende dag woensdag om 1 uur een back-up van de resterende data bases gemaakt. Als niet voor alle data bases een back-up is gemaakt, wordt de volgende dag op dit moment opnieuw geprobeerd. Dit gaat door totdat alle data bases zijn gemaakt.

Nadat de app opnieuw is ingesteld, wordt er opnieuw een back-up van alle data bases gemaakt.

Dit scenario laat zien dat automatische back-up alleen binnen het opgegeven tijd venster werkt en dat elke Data Base eenmaal per week een back-up wordt gemaakt. Dit betekent ook dat het mogelijk is dat back-ups meerdere dagen in beslag nemen wanneer het niet mogelijk is om alle back-ups op één dag te volt ooien.

### <a name="scenario-2-daily-backups"></a>Scenario 2: dagelijkse back-ups
U hebt een SQL Server-VM die een aantal grote data bases bevat.

Op maandag schakelt u automatische back-up v2 in met de volgende instellingen:

- Back-upschema: hand matig
- Frequentie van volledige back-up: dagelijks
- Begin tijd volledige back-up: 22:00
- Tijd venster voor volledige back-up: 6 uur

Dit betekent dat het volgende beschik bare back-upvenster maandag om 10 uur voor 6 uren is. Op dat moment wordt een back-up van uw data bases per keer met geautomatiseerde back-ups gemaakt.

Vervolgens wordt de volledige back-up van alle data bases opnieuw gestart, op dinsdag 10 gedurende 6 uur.

> [!IMPORTANT]
> Bij het plannen van dagelijkse back-ups wordt aangeraden een breed tijd venster te plannen om ervoor te zorgen dat er binnen deze tijd een back-up van alle data bases kan worden gemaakt. Dit is vooral belang rijk wanneer u een grote hoeveelheid gegevens hebt waarvan u een back-up wilt maken.

## <a name="configure-in-the-portal"></a>Configureren in de portal

U kunt de Azure Portal gebruiken om automatische back-ups v2 te configureren tijdens het inrichten of voor bestaande SQL Server 2016/2017 Vm's.

## <a name="configure-for-new-vms"></a>Configureren voor nieuwe Vm's

Gebruik de Azure Portal voor het configureren van automatische back-up v2 wanneer u een nieuwe SQL Server 2016-of 2017-Virtuele machine maakt in het Resource Manager-implementatie model.

Selecteer op het tabblad **SQL Server instellingen** de optie **inschakelen** onder **automatische back-up**. De volgende Azure Portal scherm afbeelding toont de **automatische back-** upinstellingen voor SQL.

![Configuratie van automatische back-up van SQL in Azure Portal](./media/virtual-machines-windows-sql-automated-backup-v2/automated-backup-blade.png)

> [!NOTE]
> Automatische back-up v2 is standaard uitgeschakeld.

## <a name="configure-existing-vms"></a>Bestaande Vm's configureren

[!INCLUDE [windows-virtual-machines-sql-use-new-management-blade](../../../../includes/windows-virtual-machines-sql-new-resource.md)]

Voor bestaande SQL Server virtuele machines navigeert u naar de [resource virtuele SQL-machines](virtual-machines-windows-sql-manage-portal.md#access-the-sql-virtual-machines-resource) en selecteert u vervolgens **back-ups** om uw automatische back-ups te configureren.

![Automatische back-up van SQL voor bestaande Vm's](./media/virtual-machines-windows-sql-automated-backup-v2/sql-server-configuration.png)


Wanneer u klaar bent, klikt u op de knop **Toep assen** aan de onderkant van de pagina instellingen voor **back-ups** om uw wijzigingen op te slaan.

Als u automatische back-up voor de eerste keer inschakelt, configureert Azure de SQL Server IaaS-agent op de achtergrond. Gedurende deze periode wordt mogelijk niet weer gegeven dat de automatische back-up is geconfigureerd in de Azure Portal. Wacht enkele minuten totdat de agent is geïnstalleerd en geconfigureerd. Daarna worden de nieuwe instellingen weer gegeven in de Azure Portal.

## <a name="configure-with-powershell"></a>Configureren met Power shell

U kunt Power shell gebruiken voor het configureren van geautomatiseerde back-ups v2. Voordat u begint, moet u het volgende doen:

- [Down load en installeer de nieuwste Azure PowerShell](https://aka.ms/webpi-azps).
- Open Windows Power shell en koppel deze aan uw account met de opdracht **Connect-AzAccount** .

[!INCLUDE [updated-for-az.md](../../../../includes/updated-for-az.md)]

### <a name="install-the-sql-iaas-extension"></a>De SQL IaaS-extensie installeren
Als u een SQL Server virtuele machine hebt ingericht vanuit de Azure Portal, moet de uitbrei ding SQL Server IaaS al zijn geïnstalleerd. U kunt bepalen of deze is geïnstalleerd voor uw virtuele machine door de opdracht **Get-AzVM** aan te roepen en de eigenschap **Extensions** te controleren.

```powershell
$vmname = "vmname"
$resourcegroupname = "resourcegroupname"

(Get-AzVM -Name $vmname -ResourceGroupName $resourcegroupname).Extensions 
```

Als de uitbrei ding voor de SQL Server IaaS-agent is geïnstalleerd, wordt deze weer gegeven als ' SqlIaaSAgent ' of ' SQLIaaSExtension '. **ProvisioningState** voor de uitbrei ding moet ook ' geslaagd ' weer geven. 

Als de app niet is geïnstalleerd of niet is ingericht, kunt u deze installeren met de volgende opdracht. Naast de naam van de virtuele machine en de resource groep moet u ook de regio ( **$Region**) opgeven waarin uw VM zich bevindt.

```powershell
$region = "EASTUS2"
Set-AzVMSqlServerExtension -VMName $vmname `
    -ResourceGroupName $resourcegroupname -Name "SQLIaasExtension" `
    -Version "2.0" -Location $region 
```

### <a id="verifysettings"></a>Huidige instellingen verifiëren
Als u automatische back-up tijdens het inrichten hebt ingeschakeld, kunt u Power shell gebruiken om de huidige configuratie te controleren. Voer de opdracht **Get-AzVMSqlServerExtension** uit en controleer de eigenschap **AutoBackupSettings** :

```powershell
(Get-AzVMSqlServerExtension -VMName $vmname -ResourceGroupName $resourcegroupname).AutoBackupSettings
```

U ziet dat de uitvoer er ongeveer als volgt uitziet:

```
Enable                      : True
EnableEncryption            : False
RetentionPeriod             : 30
StorageUrl                  : https://test.blob.core.windows.net/
StorageAccessKey            :  
Password                    : 
BackupSystemDbs             : False
BackupScheduleType          : Manual
FullBackupFrequency         : WEEKLY
FullBackupStartTime         : 2
FullBackupWindowHours       : 2
LogBackupFrequency          : 60
```

Als uw uitvoer laat zien dat **inschakelen** is ingesteld op **Onwaar**, moet u automatische back-up inschakelen. Het goede nieuws is dat u op dezelfde manier automatische back-ups inschakelt en configureert. Zie de volgende sectie voor deze informatie.

> [!NOTE] 
> Als u de instellingen direct na het aanbrengen van een wijziging controleert, is het mogelijk dat u de oude configuratie waarden terugkrijgt. Wacht enkele minuten en controleer de instellingen opnieuw om er zeker van te zijn dat uw wijzigingen zijn toegepast.

### <a name="configure-automated-backup-v2"></a>Automatische back-up v2 configureren
U kunt Power shell gebruiken om automatische back-ups in te scha kelen en de configuratie en het gedrag op elk gewenst moment te wijzigen. 

Selecteer eerst of maak een opslag account voor de back-upbestanden. Met het volgende script wordt een opslag account geselecteerd of wordt het gemaakt als het niet bestaat.

```powershell
$storage_accountname = "yourstorageaccount"
$storage_resourcegroupname = $resourcegroupname

$storage = Get-AzStorageAccount -ResourceGroupName $resourcegroupname `
    -Name $storage_accountname -ErrorAction SilentlyContinue
If (-Not $storage)
    { $storage = New-AzStorageAccount -ResourceGroupName $storage_resourcegroupname `
    -Name $storage_accountname -SkuName Standard_GRS -Location $region } 
```

> [!NOTE]
> Automatische back-up biedt geen ondersteuning voor het opslaan van back-ups in Premium Storage, maar er kunnen wel back-ups worden gemaakt van VM-schijven die gebruikmaken van Premium Storage.

Gebruik vervolgens de opdracht **New-AzVMSqlServerAutoBackupConfig** om de automatische back-up v2-instellingen in te scha kelen en te configureren om back-ups op te slaan in het Azure-opslag account. In dit voor beeld worden de back-ups 10 dagen bewaard. Back-ups van de systeem database zijn ingeschakeld. Volledige back-ups worden wekelijks gepland met een tijd venster dat begint bij 20:00 gedurende twee uur. Logboek back-ups worden elke 30 minuten gepland. Met de tweede opdracht **set-AzVMSqlServerExtension**wordt de opgegeven virtuele machine van Azure bijgewerkt met deze instellingen.

```powershell
$autobackupconfig = New-AzVMSqlServerAutoBackupConfig -Enable `
    -RetentionPeriodInDays 10 -StorageContext $storage.Context `
    -ResourceGroupName $storage_resourcegroupname -BackupSystemDbs `
    -BackupScheduleType Manual -FullBackupFrequency Weekly `
    -FullBackupStartHour 20 -FullBackupWindowInHours 2 `
    -LogBackupFrequencyInMinutes 30 

Set-AzVMSqlServerExtension -AutoBackupSettings $autobackupconfig `
    -VMName $vmname -ResourceGroupName $resourcegroupname 
```

Het kan enkele minuten duren om de SQL Server IaaS-agent te installeren en configureren. 

Als u versleuteling wilt inschakelen, wijzigt u het vorige script om de para meter **EnableEncryption** samen met een wacht woord (beveiligde teken reeks) door te geven voor de para meter **CertificatePassword** . Met het volgende script kunt u de automatische back-upinstellingen in het vorige voor beeld inschakelen en versleuteling toevoegen.

```powershell
$password = "P@ssw0rd"
$encryptionpassword = $password | ConvertTo-SecureString -AsPlainText -Force  

$autobackupconfig = New-AzVMSqlServerAutoBackupConfig -Enable `
    -EnableEncryption -CertificatePassword $encryptionpassword `
    -RetentionPeriodInDays 10 -StorageContext $storage.Context `
    -ResourceGroupName $storage_resourcegroupname -BackupSystemDbs `
    -BackupScheduleType Manual -FullBackupFrequency Weekly `
    -FullBackupStartHour 20 -FullBackupWindowInHours 2 `
    -LogBackupFrequencyInMinutes 30 

Set-AzVMSqlServerExtension -AutoBackupSettings $autobackupconfig `
    -VMName $vmname -ResourceGroupName $resourcegroupname
```

[Controleer de automatische back-upconfiguratie](#verifysettings)om te bevestigen dat uw instellingen zijn toegepast.

### <a name="disable-automated-backup"></a>Automatische back-up uitschakelen
Als u automatische back-ups wilt uitschakelen, voert u hetzelfde script uit zonder de para meter **-Enable** voor de opdracht **New-AzVMSqlServerAutoBackupConfig** . Als de para meter **-Enable ontbreekt,** wordt de opdracht voor het uitschakelen van de functie door gesignaleerd. Net als bij de installatie kan het enkele minuten duren om automatische back-ups uit te scha kelen.

```powershell
$autobackupconfig = New-AzVMSqlServerAutoBackupConfig -ResourceGroupName $storage_resourcegroupname

Set-AzVMSqlServerExtension -AutoBackupSettings $autobackupconfig `
    -VMName $vmname -ResourceGroupName $resourcegroupname
```

### <a name="example-script"></a>Voorbeeldscript
Het volgende script bevat een set variabelen die u kunt aanpassen om automatische back-ups voor uw virtuele machine in te scha kelen en te configureren. In uw geval moet u het script mogelijk aanpassen op basis van uw vereisten. U moet bijvoorbeeld wijzigingen aanbrengen als u de back-up van systeem databases wilt uitschakelen of versleuteling wilt inschakelen.

```powershell
$vmname = "yourvmname"
$resourcegroupname = "vmresourcegroupname"
$region = "Azure region name such as EASTUS2"
$storage_accountname = "storageaccountname"
$storage_resourcegroupname = $resourcegroupname
$retentionperiod = 10
$backupscheduletype = "Manual"
$fullbackupfrequency = "Weekly"
$fullbackupstarthour = "20"
$fullbackupwindow = "2"
$logbackupfrequency = "30"

# ResourceGroupName is the resource group which is hosting the VM where you are deploying the SQL IaaS Extension 

Set-AzVMSqlServerExtension -VMName $vmname `
    -ResourceGroupName $resourcegroupname -Name "SQLIaasExtension" `
    -Version "2.0" -Location $region

# Creates/use a storage account to store the backups

$storage = Get-AzStorageAccount -ResourceGroupName $resourcegroupname `
    -Name $storage_accountname -ErrorAction SilentlyContinue
If (-Not $storage)
    { $storage = New-AzStorageAccount -ResourceGroupName $storage_resourcegroupname `
    -Name $storage_accountname -SkuName Standard_GRS -Location $region }

# Configure Automated Backup settings

$autobackupconfig = New-AzVMSqlServerAutoBackupConfig -Enable `
    -RetentionPeriodInDays $retentionperiod -StorageContext $storage.Context `
    -ResourceGroupName $storage_resourcegroupname -BackupSystemDbs `
    -BackupScheduleType $backupscheduletype -FullBackupFrequency $fullbackupfrequency `
    -FullBackupStartHour $fullbackupstarthour -FullBackupWindowInHours $fullbackupwindow `
    -LogBackupFrequencyInMinutes $logbackupfrequency

# Apply the Automated Backup settings to the VM

Set-AzVMSqlServerExtension -AutoBackupSettings $autobackupconfig `
    -VMName $vmname -ResourceGroupName $resourcegroupname
```

## <a name="monitoring"></a>Controleren

Als u automatische back-ups wilt bewaken op SQL Server 2016/2017, hebt u twee belang rijke opties. Omdat automatische back-up gebruikmaakt van de SQL Server beheerde back-upfunctie, zijn dezelfde bewakings technieken van toepassing op beide.

Eerst kunt u de status navragen door [msdb. managed_backup. sp_get_backup_diagnostics](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/managed-backup-sp-get-backup-diagnostics-transact-sql)aan te roepen. Of query's uitvoeren op de functie [msdb. managed_backup. fn_get_health_status](https://docs.microsoft.com/sql/relational-databases/system-functions/managed-backup-fn-get-health-status-transact-sql) .

Een andere optie is om te profiteren van de ingebouwde Database Mail functie voor meldingen.

1. Roep de procedure [msdb. managed_backup. sp_set_parameter](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/managed-backup-sp-set-parameter-transact-sql) aan om een e-mail adres toe te wijzen aan de para meter **SSMBackup2WANotificationEmailIds** . 
1. Schakel [SendGrid](../../../sendgrid-dotnet-how-to-send-email.md) in om de e-mail berichten van de Azure-VM te verzenden.
1. Gebruik de SMTP-server en de gebruikers naam om Database Mail te configureren. U kunt Database Mail configureren in SQL Server Management Studio of met Transact-SQL-opdrachten. Zie [database mail](https://docs.microsoft.com/sql/relational-databases/database-mail/database-mail)voor meer informatie.
1. [Configureer SQL Server Agent om database mail te gebruiken](https://docs.microsoft.com/sql/relational-databases/database-mail/configure-sql-server-agent-mail-to-use-database-mail).
1. Controleer of de SMTP-poort is toegestaan via de lokale VM-firewall en de netwerk beveiligings groep voor de virtuele machine.

## <a name="next-steps"></a>Volgende stappen
Met geautomatiseerde back-up v2 wordt beheerde back-up geconfigureerd op virtuele machines van Azure. Het is dus belang rijk dat u [de documentatie voor beheerde back-up bekijkt](https://msdn.microsoft.com/library/dn449496.aspx) om het gedrag en de implicaties ervan te begrijpen.

Meer richt lijnen voor back-up en herstel van SQL Server op virtuele Azure-machines vindt u in het volgende artikel: [back-up en herstel voor SQL Server in Azure virtual machines](virtual-machines-windows-sql-backup-recovery.md).

Zie [SQL Server IaaS agent extension](virtual-machines-windows-sql-server-agent-extension.md)(Engelstalig) voor meer informatie over andere beschik bare automatiserings taken.

Zie [SQL Server op azure virtual machines Overview](virtual-machines-windows-sql-server-iaas-overview.md)voor meer informatie over het uitvoeren van SQL Server op virtuele machines in Azure.

