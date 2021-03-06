---
title: Azure Backup Recovery Services kluizen verplaatsen
description: Instructies voor het verplaatsen van de Recovery Services-kluis in azure-abonnementen en resource groepen.
ms.reviewer: sogup
ms.topic: conceptual
ms.date: 04/08/2019
ms.openlocfilehash: 7d0a6c47fa08774161d3a2b7507cdfecaf8c7197
ms.sourcegitcommit: 4821b7b644d251593e211b150fcafa430c1accf0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/19/2019
ms.locfileid: "74172893"
---
# <a name="move-a-recovery-services-vault-across-azure-subscriptions-and-resource-groups"></a>Een Recovery Services kluis verplaatsen over Azure-abonnementen en-resource groepen

In dit artikel wordt uitgelegd hoe u een Recovery Services kluis kunt verplaatsen die is geconfigureerd voor Azure Backup over Azure-abonnementen of naar een andere resource groep in hetzelfde abonnement. U kunt de Azure Portal of Power shell gebruiken om een Recovery Services kluis te verplaatsen.

## <a name="supported-regions"></a>Ondersteunde regio’s

Resource verplaatsing voor Recovery Services kluis wordt ondersteund in Australië-oost, Australië-Zuid-Oost, Canada-centraal, Canada-oost, Zuid-Azië-oost, Azië-oost, centraal VS, Noord-Centraal VS, VS-Oost, Oost-VS2, Zuid-Centraal VS, West-Centraal VS, West-Centraal VS2, VS-West, Centraal-India, India-zuid, Japan-Oost, Japan-West, Korea-centraal, Korea-zuid, Europa-noord, Europa-west, Zuid-Afrika-noord, Zuid-Afrika-west, UK-zuid en UK-west.

## <a name="prerequisites-for-moving-recovery-services-vault"></a>Vereisten voor het verplaatsen van Recovery Services kluis

- Tijdens het verplaatsen van de kluis over resource groepen worden de bron-en doel resource groepen vergrendeld, waardoor er geen schrijf-en verwijder bewerkingen kunnen worden uitgevoerd. Zie [dit artikel](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-move-resources) voor meer informatie.
- Alleen beheerders abonnement heeft de machtigingen om een kluis te verplaatsen.
- Voor het verplaatsen van de kluis tussen abonnementen moet het doel abonnement zich in dezelfde Tenant bevinden als het bron abonnement en moet de status zijn ingeschakeld.
- U moet gemachtigd zijn om schrijf bewerkingen uit te voeren op de doel resource groep.
- Door de kluis te verplaatsen, wijzigt u alleen de resource groep. De Recovery Services kluis bevindt zich op dezelfde locatie en kan niet worden gewijzigd.
- U kunt slechts één Recovery Services kluis per regio tegelijk verplaatsen.
- Als een virtuele machine niet wordt verplaatst met de Recovery Services kluis in abonnementen of naar een nieuwe resource groep, blijven de huidige VM-herstel punten intact in de kluis totdat ze verlopen.
- Of de virtuele machine wordt verplaatst met de kluis of niet, u kunt de virtuele machine altijd herstellen met behulp van de behouden Back-upgeschiedenis in de kluis.
- De Azure Disk Encryption vereist dat de sleutel kluis en de virtuele machines zich in dezelfde Azure-regio en hetzelfde abonnement bevinden.
- Als u een virtuele machine wilt verplaatsen met beheerde schijven, raadpleegt u dit [artikel](https://azure.microsoft.com/blog/move-managed-disks-and-vms-now-available/).
- De opties voor het verplaatsen van resources die via het klassieke model zijn geïmplementeerd, zijn afhankelijk van het feit of u de resources in een abonnement verplaatst of naar een nieuw abonnement. Zie [dit artikel](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-move-resources) voor meer informatie.
- Het back-upbeleid dat voor de kluis is gedefinieerd, blijft behouden nadat de kluis over alle abonnementen of naar een nieuwe resource groep is verplaatst.
- Het verplaatsen van de kluis met de Azure Files, Azure File Sync of SQL in IaaS Vm's tussen abonnementen en resource groepen wordt niet ondersteund.
- Als u een kluis met back-upgegevens voor de VM verplaatst, moet u de virtuele machines verplaatsen naar hetzelfde abonnement en dezelfde doel-VM-resource groepsnaam gebruiken (zoals in het oude abonnement) om door te gaan met back-ups.

> [!NOTE]
>
> Recovery Services kluizen die zijn geconfigureerd voor gebruik met **Azure site Recovery** kunnen nog niet worden verplaatst. Als u virtuele machines (Azure IaaS, Hyper-V, VMware) of fysieke computers hebt geconfigureerd voor herstel na nood gevallen met behulp van de **Azure site Recovery**, wordt de verplaatsings bewerking geblokkeerd. De functie voor het verplaatsen van resources voor Site Recovery service is nog niet beschikbaar.

## <a name="use-azure-portal-to-move-recovery-services-vault-to-different-resource-group"></a>Azure Portal gebruiken om Recovery Services kluis te verplaatsen naar een andere resource groep

Een Recovery Services-kluis en de bijbehorende resources verplaatsen naar een andere resource groep

1. Meld u aan bij de [Azure Portal](https://portal.azure.com/).
2. Open de lijst met **Recovery Services kluizen** en selecteer de kluis die u wilt verplaatsen. Wanneer het kluis dashboard wordt geopend, wordt het weer gegeven zoals in de volgende afbeelding.

   ![Service kluis herstellen openen](./media/backup-azure-move-recovery-services/open-recover-service-vault.png)

   Als u de **essentiële** informatie voor uw kluis niet ziet, klikt u op het pictogram vervolg keuzelijst. U ziet nu de essentiële informatie voor uw kluis.

   ![Het tabblad informatie over Essentials](./media/backup-azure-move-recovery-services/essentials-information-tab.png)

3. Klik in het menu kluis overzicht op **wijzigen** naast de **resource groep**om de Blade **resources verplaatsen** te openen.

   ![Resource groep wijzigen](./media/backup-azure-move-recovery-services/change-resource-group.png)

4. In de Blade **resources verplaatsen** kunt u voor de geselecteerde kluis het beste de optionele gerelateerde resources verplaatsen door het selectie vakje te selecteren, zoals in de volgende afbeelding.

   ![Abonnement verplaatsen](./media/backup-azure-move-recovery-services/move-resource.png)

5. Als u de doel resource groep wilt toevoegen, selecteert u in de vervolg keuzelijst **resource groep** een bestaande resource groep of klikt u op **een nieuwe groep maken** optie.

   ![Resource maken](./media/backup-azure-move-recovery-services/create-a-new-resource.png)

6. Nadat u de resource groep hebt toegevoegd, **moet u weten dat de hulpprogram ma's en scripts die zijn gekoppeld aan verplaatste resources, niet werken totdat ze zijn bijgewerkt** met de optie nieuwe resource-id's en klik vervolgens op **OK** om het verplaatsen van de kluis te volt ooien.

   ![Bevestigings bericht](./media/backup-azure-move-recovery-services/confirmation-message.png)

## <a name="use-azure-portal-to-move-recovery-services-vault-to-a-different-subscription"></a>Azure Portal gebruiken om Recovery Services kluis te verplaatsen naar een ander abonnement

U kunt een Recovery Services kluis en de bijbehorende resources naar een ander abonnement verplaatsen

1. Meld u aan bij de [Azure Portal](https://portal.azure.com/).
2. Open de lijst met Recovery Services kluizen en selecteer de kluis die u wilt verplaatsen. Wanneer het kluis dashboard wordt geopend, wordt het weer gegeven zoals in de volgende afbeelding.

    ![Service kluis herstellen openen](./media/backup-azure-move-recovery-services/open-recover-service-vault.png)

    Als u de **essentiële** informatie voor uw kluis niet ziet, klikt u op het pictogram vervolg keuzelijst. U ziet nu de essentiële informatie voor uw kluis.

    ![Het tabblad informatie over Essentials](./media/backup-azure-move-recovery-services/essentials-information-tab.png)

3. Klik in het menu overzicht van de kluis op **wijzigen** naast **abonnement**om de Blade **resources verplaatsen** te openen.

   ![Abonnement wijzigen](./media/backup-azure-move-recovery-services/change-resource-subscription.png)

4. Selecteer de resources die u wilt verplaatsen. u wordt aangeraden de optie **Alles selecteren** te gebruiken om alle vermelde optionele resources te selecteren.

   ![resource verplaatsen](./media/backup-azure-move-recovery-services/move-resource-source-subscription.png)

5. Selecteer het doel abonnement in de vervolg keuzelijst **abonnement** , waar u de kluis wilt verplaatsen.
6. Als u de doel resource groep wilt toevoegen, selecteert u in de vervolg keuzelijst **resource groep** een bestaande resource groep of klikt u op **een nieuwe groep maken** optie.

   ![Abonnement toevoegen](./media/backup-azure-move-recovery-services/add-subscription.png)

7. Klik op **Ik begrijp dat hulp middelen en scripts die zijn gekoppeld aan verplaatste resources, niet werken totdat ze de optie nieuwe resource-id's gebruiken** om te bevestigen en klik vervolgens op **OK**.

> [!NOTE]
> Back-up tussen meerdere abonnementen (RS-kluis en beveiligde Vm's bevinden zich in verschillende abonnementen) is geen ondersteund scenario. Daarnaast kan de optie voor opslag redundantie van lokale redundante opslag (LRS) naar globale redundante opslag (GRS) en vice versa niet worden gewijzigd tijdens de bewerking voor het verplaatsen van de kluis.
>
>

## <a name="use-powershell-to-move-recovery-services-vault"></a>Power shell gebruiken om Recovery Services kluis te verplaatsen

Als u een Recovery Services kluis wilt verplaatsen naar een andere resource groep, gebruikt u de `Move-AzureRMResource`-cmdlet. `Move-AzureRMResource` vereist de resource naam en het resource type. U kunt beide van de `Get-AzureRmRecoveryServicesVault` cmdlet ophalen.

```powershell
$destinationRG = "<destinationResourceGroupName>"
$vault = Get-AzureRmRecoveryServicesVault -Name <vaultname> -ResourceGroupName <vaultRGname>
Move-AzureRmResource -DestinationResourceGroupName $destinationRG -ResourceId $vault.ID
```

Als u de resources naar een ander abonnement wilt verplaatsen, neemt u de para meter `-DestinationSubscriptionId` op.

```powershell
Move-AzureRmResource -DestinationSubscriptionId "<destinationSubscriptionID>" -DestinationResourceGroupName $destinationRG -ResourceId $vault.ID
```

Nadat u de bovenstaande cmdlets hebt uitgevoerd, wordt u gevraagd om te bevestigen dat u de opgegeven resources wilt verplaatsen. Typ **Y** om te bevestigen. Na een geslaagde validatie wordt de resource verplaatst.

## <a name="use-cli-to-move-recovery-services-vault"></a>CLI gebruiken om Recovery Services kluis te verplaatsen

Als u een Recovery Services kluis wilt verplaatsen naar een andere resource groep, gebruikt u de volgende cmdlet:

```azurecli
az resource move --destination-group <destinationResourceGroupName> --ids <VaultResourceID>
```

Als u wilt verplaatsen naar een nieuw abonnement, bieden de `--destination-subscription-id` parameter.

## <a name="post-migration"></a>Na de migratie

1. De toegangs controle voor de resource groepen instellen/controleren.  
2. De functie voor het maken van een back-up en controle moet opnieuw worden geconfigureerd voor de kluis die de verplaatsing heeft voltooid. De vorige configuratie gaat verloren tijdens de verplaatsings bewerking.

## <a name="next-steps"></a>Volgende stappen

U kunt verschillende soorten resources verplaatsen tussen resource groepen en abonnementen.

Zie voor meer informatie [Resources verplaatsen naar een nieuwe resourcegroep of een nieuw abonnement](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-move-resources).
