---
title: Key Vault sleutel & geheim voor versleutelde VM herstellen
description: Meer informatie over het herstellen van Key Vault sleutel en geheim in Azure Backup met behulp van Power shell
ms.topic: conceptual
ms.date: 08/28/2017
ms.openlocfilehash: 55e20d861eedde19946d2c99dfc1cd8ff33f6b0b
ms.sourcegitcommit: 4821b7b644d251593e211b150fcafa430c1accf0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/19/2019
ms.locfileid: "74172771"
---
# <a name="restore-key-vault-key-and-secret-for-encrypted-vms-using-azure-backup"></a>Key Vault sleutel en geheim voor versleutelde Vm's herstellen met Azure Backup

In dit artikel vindt u informatie over het gebruik van Azure VM backup voor het uitvoeren van herstel van versleutelde virtuele machines in azure, als uw sleutel en geheim niet aanwezig zijn in de sleutel kluis. Deze stappen kunnen ook worden gebruikt als u een afzonderlijke kopie van de sleutel (sleutel versleutelings sleutel) en geheim (BitLocker-versleutelings sleutel) wilt onderhouden voor de herstelde VM.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="prerequisites"></a>Vereisten

* **Back-up van versleutelde vm's met virtuele machines** -versleutelde Azure vm's zijn gemaakt met Azure backup. Raadpleeg het artikel [back-ups en herstel van Azure-Vm's beheren met Power shell](backup-azure-vms-automation.md) voor meer informatie over het maken van een back-up van versleutelde virtuele machines in Azure.
* **Azure Key Vault configureren** : Zorg ervoor dat sleutel kluis waarmee sleutels en geheimen moeten worden hersteld, al aanwezig is. Raadpleeg het artikel aan de [slag met Azure Key Vault](../key-vault/key-vault-get-started.md) voor meer informatie over het beheer van de sleutel kluis.
* **Herstel schijf** : Zorg ervoor dat u de herstel taak hebt geactiveerd voor het herstellen van schijven voor een versleutelde VM met behulp van [Power shell-stappen](backup-azure-vms-automation.md#restore-an-azure-vm). Dit komt doordat met deze taak een JSON-bestand wordt gegenereerd in uw opslag account met sleutels en geheimen voor de versleutelde virtuele machine die moet worden hersteld.

## <a name="get-key-and-secret-from-azure-backup"></a>Sleutel en geheim ophalen uit Azure Backup

> [!NOTE]
> Nadat de schijf is hersteld voor de versleutelde VM, moet u het volgende doen:
>
> * $details is gevuld met herstel schijf taak Details, zoals vermeld in [Power shell-stappen in de sectie schijven herstellen](backup-azure-vms-automation.md#restore-an-azure-vm)
> * De virtuele machine moet worden gemaakt van de herstelde schijven pas **nadat de sleutel en het geheim zijn hersteld in de sleutel kluis**.

De herstelde schijf eigenschappen voor de taak Details opvragen.

```powershell
$properties = $details.properties
$storageAccountName = $properties["Target Storage Account Name"]
$containerName = $properties["Config Blob Container Name"]
$encryptedBlobName = $properties["Encryption Info Blob Name"]
```

Stel de Azure Storage-context in en herstel het JSON-configuratie bestand met sleutel-en geheim gegevens voor versleutelde virtuele machine.

```powershell
Set-AzCurrentStorageAccount -Name $storageaccountname -ResourceGroupName '<rg-name>'
$destination_path = 'C:\vmencryption_config.json'
Get-AzStorageBlobContent -Blob $encryptedBlobName -Container $containerName -Destination $destination_path
$encryptionObject = Get-Content -Path $destination_path  | ConvertFrom-Json
```

## <a name="restore-key"></a>Sleutel herstellen

Zodra het JSON-bestand is gegenereerd in het hierboven vermelde doelpad, Genereer dan het sleutel-blobbestand van de JSON en voer het in om de sleutel-cmdlet te herstellen om de sleutel (KEK) terug te zetten in de sleutel kluis.

```powershell
$keyDestination = 'C:\keyDetails.blob'
[io.file]::WriteAllBytes($keyDestination, [System.Convert]::FromBase64String($encryptionObject.OsDiskKeyAndSecretDetails.KeyBackupData))
Restore-AzureKeyVaultKey -VaultName '<target_key_vault_name>' -InputFile $keyDestination
```

## <a name="restore-secret"></a>Geheim herstellen

Gebruik het hierboven gegenereerde JSON-bestand om een geheime naam en waarde op te halen en de feed in te stellen voor het instellen van een geheime cmdlet om het geheim (BEK) terug te zetten in de sleutel kluis. Gebruik deze cmdlets als uw **virtuele machine is versleuteld met bek en Kek**.

**Gebruik deze cmdlets als uw Windows-virtuele machine is versleuteld met BEK en KEK.**

```powershell
$secretdata = $encryptionObject.OsDiskKeyAndSecretDetails.SecretData
$Secret = ConvertTo-SecureString -String $secretdata -AsPlainText -Force
$secretname = 'B3284AAA-DAAA-4AAA-B393-60CAA848AAAA'
$Tags = @{'DiskEncryptionKeyEncryptionAlgorithm' = 'RSA-OAEP';'DiskEncryptionKeyFileName' = 'B3284AAA-DAAA-4AAA-B393-60CAA848AAAA.BEK';'DiskEncryptionKeyEncryptionKeyURL' = $encryptionObject.OsDiskKeyAndSecretDetails.KeyUrl;'MachineName' = 'vm-name'}
Set-AzureKeyVaultSecret -VaultName '<target_key_vault_name>' -Name $secretname -SecretValue $Secret -ContentType  'Wrapped BEK' -Tags $Tags
```

**Gebruik deze cmdlets als uw virtuele Linux-machine is versleuteld met BEK en KEK.**

```powershell
$secretdata = $encryptionObject.OsDiskKeyAndSecretDetails.SecretData
$Secret = ConvertTo-SecureString -String $secretdata -AsPlainText -Force
$secretname = 'B3284AAA-DAAA-4AAA-B393-60CAA848AAAA'
$Tags = @{'DiskEncryptionKeyEncryptionAlgorithm' = 'RSA-OAEP';'DiskEncryptionKeyFileName' = 'LinuxPassPhraseFileName';'DiskEncryptionKeyEncryptionKeyURL' = $encryptionObject.OsDiskKeyAndSecretDetails.KeyUrl;'MachineName' = 'vm-name'}
Set-AzureKeyVaultSecret -VaultName '<target_key_vault_name>' -Name $secretname -SecretValue $Secret -ContentType  'Wrapped BEK' -Tags $Tags
```

Gebruik het hierboven gegenereerde JSON-bestand om een geheime naam en waarde op te halen en de feed in te stellen voor het instellen van een geheime cmdlet om het geheim (BEK) terug te zetten in de sleutel kluis. Gebruik deze cmdlets als uw **virtuele machine is versleuteld met alleen bek** .

```powershell
$secretDestination = 'C:\secret.blob'
[io.file]::WriteAllBytes($secretDestination, [System.Convert]::FromBase64String($encryptionObject.OsDiskKeyAndSecretDetails.KeyVaultSecretBackupData))
Restore-AzureKeyVaultSecret -VaultName '<target_key_vault_name>' -InputFile $secretDestination -Verbose
  ```

> [!NOTE]
>
> * De waarde voor $secretname kan worden verkregen door te verwijzen naar de uitvoer van $encryptionObject. OsDiskKeyAndSecretDetails. SecretUrl en door gebruik te maken van tekst na geheimen/bijv. de URL voor uitvoer geheim is https://keyvaultname.vault.azure.net/secrets/B3284AAA-DAAA-4AAA-B393-60CAA848AAAA/xx000000xx0849999f3xx30000003163 en de naam van het geheim is B3284AAA-DAAA-4AAA-B393-60CAA848AAAA
> * De waarde van de tag DiskEncryptionKeyFileName is hetzelfde als de geheime naam.
>
>

## <a name="create-virtual-machine-from-restored-disk"></a>Een virtuele machine maken op basis van de herstelde schijf

Als u een back-up hebt gemaakt van een versleutelde VM met behulp van Azure VM backup, kunt u de Power shell-cmdlets die hierboven worden genoemd, gebruiken om de sleutel en het geheim terug te zetten Nadat u ze hebt teruggezet, raadpleegt u het artikel [back-up en herstel van Azure-Vm's beheren met Power shell](backup-azure-vms-automation.md#create-a-vm-from-restored-disks) om versleutelde vm's te maken op basis van de herstelde schijf, sleutel en geheim.

## <a name="legacy-approach"></a>Verouderde aanpak

De hierboven genoemde benadering werkt voor alle herstel punten. De oudere benadering van het ophalen van sleutel-en geheime gegevens van het herstel punt is echter geldig voor herstel punten die ouder zijn dan 11 juli 2017 voor virtuele machines die zijn versleuteld met BEK en KEK. Zodra de herstel schijf taak is voltooid voor een versleutelde VM met behulp van [Power shell-stappen](backup-azure-vms-automation.md#restore-an-azure-vm), moet $RP worden gevuld met een geldige waarde.

### <a name="restore-key"></a>Sleutel herstellen

Gebruik de volgende cmdlets om informatie over de sleutel (KEK) op te halen uit het herstel punt en de feed in te voeren om de sleutel-cmdlet terug te zetten in de sleutel kluis.

```powershell
$rp1 = Get-AzRecoveryServicesBackupRecoveryPoint -RecoveryPointId $rp[0].RecoveryPointId -Item $backupItem -KeyFileDownloadLocation 'C:\Users\downloads'
Restore-AzureKeyVaultKey -VaultName '<target_key_vault_name>' -InputFile 'C:\Users\downloads'
```

### <a name="restore-secret"></a>Geheim herstellen

Gebruik de volgende cmdlets voor het verkrijgen van geheime informatie (BEK) van het herstel punt en voer deze in om de geheime cmdlet in te stellen om deze terug te zetten in de sleutel kluis.

```powershell
$secretname = 'B3284AAA-DAAA-4AAA-B393-60CAA848AAAA'
$secretdata = $rp1.KeyAndSecretDetails.SecretData
$Secret = ConvertTo-SecureString -String $secretdata -AsPlainText -Force
$Tags = @{'DiskEncryptionKeyEncryptionAlgorithm' = 'RSA-OAEP';'DiskEncryptionKeyFileName' = 'B3284AAA-DAAA-4AAA-B393-60CAA848AAAA.BEK';'DiskEncryptionKeyEncryptionKeyURL' = 'https://mykeyvault.vault.azure.net:443/keys/KeyName/84daaac999949999030bf99aaa5a9f9';'MachineName' = 'vm-name'}
Set-AzureKeyVaultSecret -VaultName '<target_key_vault_name>' -Name $secretname -SecretValue $secret -Tags $Tags -SecretValue $Secret -ContentType  'Wrapped BEK'
```

> [!NOTE]
>
> * De waarde voor $secretname kan worden verkregen door te verwijzen naar de uitvoer van $rp 1. KeyAndSecretDetails. SecretUrl en het gebruik van tekst na geheimen/bijv. de URL voor uitvoer geheim is https://keyvaultname.vault.azure.net/secrets/B3284AAA-DAAA-4AAA-B393-60CAA848AAAA/xx000000xx0849999f3xx30000003163 en de geheime naam is B3284AAA-DAAA-4AAA-B393-60CAA848AAAA
> * De waarde van de tag DiskEncryptionKeyFileName is hetzelfde als de geheime naam.
> * De waarde voor DiskEncryptionKeyEncryptionKeyURL kan worden verkregen uit de sleutel kluis nadat de sleutels terug zijn hersteld en de cmdlet [Get-AzureKeyVaultKey](/powershell/module/azurerm.keyvault/get-azurekeyvaultkey) wordt gebruikt
>
>

## <a name="next-steps"></a>Volgende stappen

Nadat u de sleutel en het geheim terug naar de sleutel kluis hebt teruggezet, raadpleegt u het artikel [back-ups en herstel van virtuele Azure-machines beheren met Power shell](backup-azure-vms-automation.md#create-a-vm-from-restored-disks) om versleutelde vm's te maken op basis van de herstelde schijf, sleutel
