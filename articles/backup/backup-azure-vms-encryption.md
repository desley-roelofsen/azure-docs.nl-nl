---
title: Back-up en herstel van versleutelde virtuele Azure-machines
description: Hierin wordt beschreven hoe u back-ups van versleutelde virtuele Azure-machines maakt en herstelt met de Azure Backup-service.
ms.topic: conceptual
ms.date: 04/03/2019
ms.openlocfilehash: c4bc413e70d8e19f8006580c0631641651dcaf92
ms.sourcegitcommit: 4821b7b644d251593e211b150fcafa430c1accf0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/19/2019
ms.locfileid: "74172525"
---
# <a name="back-up-and-restore-encrypted-azure-vm"></a>Back-up en herstel van versleutelde Azure VM

In dit artikel wordt beschreven hoe u back-ups maakt van Windows of Linux Azure virtual machines (Vm's) met versleutelde schijven met behulp van de [Azure backup](backup-overview.md) -service.

Als u meer wilt weten over de werking van Azure Backup interactie met Azure-Vm's voordat u begint, controleert u deze bronnen:

- [Bekijk](backup-architecture.md#architecture-built-in-azure-vm-backup) de Azure VM-back-uparchitectuur.
- [Meer informatie over](backup-azure-vms-introduction.md) Azure VM-back-up en de uitbrei ding Azure Backup.

## <a name="encryption-support"></a>Ondersteuning voor versleuteling

Azure Backup ondersteunt back-ups van virtuele Azure-machines waarvan het besturings systeem/de gegevens schijven zijn versleuteld met Azure Disk Encryption (ADE). ADE gebruikt BitLocker voor versleuteling van Windows-Vm's en de DM-cryptografie functie voor Linux Vm's. ADE kan worden geïntegreerd met Azure Key Vault voor het beheren van sleutels en geheimen voor het versleutelen van schijven. Key Vault Key Encryption Keys (KEKs) kunnen worden gebruikt om een extra beveiligingslaag toe te voegen, versleuteling van versleutelings geheimen te versleutelen voordat u ze naar Key Vault schrijft.

Azure Backup kunt back-ups maken van virtuele Azure-machines met en zonder de Azure AD-app, zoals beschreven in de volgende tabel.

**Type VM-schijf** | **ADE (BEK/DM-cryptografie)** | **ADE en KEK**
--- | --- | ---
**Unmanaged** | Ja | Ja
**Bijgehouden**  | Ja | Ja

- Meer informatie over [ADE](../security/azure-security-disk-encryption-overview.md), [Key Vault](../key-vault/key-vault-overview.md)en [KEKs](https://blogs.msdn.microsoft.com/cclayton/2017/01/03/creating-a-key-encrypting-key-kek/).
- Lees de [Veelgestelde vragen](../security/azure-security-disk-encryption-faq.md) over de schijf versleuteling van Azure VM.

### <a name="limitations"></a>Beperkingen

- U kunt back-ups maken van versleutelde Vm's binnen hetzelfde abonnement en dezelfde regio en deze herstellen.
- Azure Backup ondersteunt Vm's die zijn versleuteld met behulp van zelfstandige sleutels. Een sleutel die deel uitmaakt van een certificaat dat wordt gebruikt om een virtuele machine te versleutelen, wordt momenteel niet ondersteund.
- U kunt back-ups maken van versleutelde Vm's binnen hetzelfde abonnement en dezelfde regio als de Recovery Services back-upkluis.
- Versleutelde Vm's kunnen niet worden hersteld op het niveau van het bestand of de map. U moet de volledige VM herstellen om bestanden en mappen te herstellen.
- Bij het herstellen van een virtuele machine kunt u de optie [bestaande VM vervangen](backup-azure-arm-restore-vms.md#restore-options) niet gebruiken voor versleutelde vm's. Deze optie wordt alleen ondersteund voor niet-versleutelde beheerde schijven.

## <a name="before-you-start"></a>Voordat u begint

Ga als volgt te werk voordat u begint:

1. Zorg ervoor dat u een of meer virtuele [Windows](../security/azure-security-disk-encryption-windows.md) -of [Linux](../virtual-machines/linux/disk-encryption-overview.md) -vm's met ade ingeschakeld hebt.
2. [De ondersteunings matrix](backup-support-matrix-iaas.md) voor Azure VM backup bekijken
3. [Maak](backup-azure-arm-vms-prepare.md#create-a-vault) een Recovery Services back-upkluis als u er nog geen hebt.
4. Als u versleuteling inschakelt voor virtuele machines die al zijn ingeschakeld voor back-up, hoeft u alleen maar een back-up te maken met machtigingen voor toegang tot de Key Vault zodat back-ups zonder onderbrekingen kunnen worden voortgezet. Meer [informatie](#provide-permissions) over het toewijzen van deze machtigingen.

Daarnaast zijn er een aantal dingen die u in bepaalde omstandigheden mogelijk moet doen:

- **Installeer de VM-agent op de VM**: Azure backup maakt back-ups van virtuele Azure-machines door een uitbrei ding te installeren in de Azure VM-agent die op de computer wordt uitgevoerd. Als uw virtuele machine is gemaakt op basis van een installatie kopie van Azure Marketplace, wordt de agent geïnstalleerd en uitgevoerd. Als u een aangepaste VM maakt of een on-premises machine migreert, moet u [de agent mogelijk hand matig installeren](backup-azure-arm-vms-prepare.md#install-the-vm-agent).
- **Uitgaande toegang expliciet toestaan**: over het algemeen hoeft u geen expliciete netwerk toegang voor een virtuele Azure-machine toe te staan, zodat deze kan communiceren met Azure backup. Sommige Vm's kunnen echter verbindings problemen ondervinden, waarbij de **ExtensionSnapshotFailedNoNetwork** -fout wordt weer gegeven wanneer wordt geprobeerd verbinding te maken. Als dit het geval is, moet u [uitgaande toegang expliciet toestaan](backup-azure-arm-vms-prepare.md#explicitly-allow-outbound-access), zodat de Azure backup extensie kan communiceren met open bare IP-adressen van Azure voor back-upverkeer.

## <a name="configure-a-backup-policy"></a>Een back-upbeleid configureren

1. Als u nog geen Recovery Services back-upkluis hebt gemaakt, volgt u [deze instructies](backup-azure-arm-vms-prepare.md#create-a-vault)
2. Open de kluis in de portal en selecteer **back-up** in de sectie **aan** de slag.

    ![Blade back-up](./media/backup-azure-vms-encryption/select-backup.png)

3. In **back-updoel** > **waar wordt uw workload uitgevoerd?** Selecteer **Azure**.
4. In **waarvan wilt u een back-up maken?** Selecteer de **virtuele machine** > **OK**.

      ![Blade scenario](./media/backup-azure-vms-encryption/select-backup-goal-one.png)

5. Selecteer in **back-upbeleid** > **back-upbeleid kiezen**het beleid dat u aan de kluis wilt koppelen. Klik vervolgens op **OK**.
    - Een back-upbeleid geeft aan wanneer er back-ups worden gemaakt en hoe lang ze worden opgeslagen.
    - De details van het standaardbeleid worden onder de vervolgkeuzelijst weergegeven.

    ![Blade Scenario openen](./media/backup-azure-vms-encryption/select-backup-goal-two.png)

6. Als u het standaard beleid niet wilt gebruiken, selecteert u **nieuwe maken**en [maakt u een aangepast beleid](backup-azure-arm-vms-prepare.md#create-a-custom-policy).

7. Kies de versleutelde Vm's waarvan u een back-up wilt maken met het beleid selecteren en selecteer **OK**.

      ![Versleutelde Vm's selecteren](./media/backup-azure-vms-encryption/selected-encrypted-vms.png)

8. Als u Azure Key Vault gebruikt, ziet u op de pagina kluis een bericht dat Azure Backup alleen-lezen toegang nodig heeft tot de sleutels en geheimen in de Key Vault.

    - Als dit bericht wordt weer gegeven, is geen actie vereist.

        ![Toegang OK](./media/backup-azure-vms-encryption/access-ok.png)

    - Als dit bericht wordt weer gegeven, moet u machtigingen instellen, zoals beschreven in de [onderstaande procedure](#provide-permissions).

        ![Toegangs waarschuwing](./media/backup-azure-vms-encryption/access-warning.png)

9. Klik op **back-up inschakelen** om het back-upbeleid in de kluis te implementeren en back-up voor de geselecteerde vm's in te scha kelen.

## <a name="trigger-a-backup-job"></a>Een back-uptaak activeren

De eerste back-up wordt uitgevoerd volgens de planning, maar u kunt deze als volgt direct uitvoeren:

1. Klik in het menu kluis op **Back-upitems**.
2. Klik in **Back-upitems**op **virtuele machine van Azure**.
3. Klik in de lijst **Back-upitems** op het weglatings teken (...).
4. Klik op **Nu back-up maken**.
5. In **Nu back-up**kunt u het besturings element kalender gebruiken om de laatste dag te selecteren dat het herstel punt moet worden bewaard. Klik vervolgens op **OK**.
6. De portal meldingen bewaken. U kunt de voortgang van de taak in het kluis dashboard controleren > **back-uptaken** > worden **uitgevoerd**. Afhankelijk van de grootte van de virtuele machine kan het maken van de eerste back-up even duren.

## <a name="provide-permissions"></a>Machtigingen opgeven

Azure VM heeft alleen-lezen toegang nodig om een back-up te maken van de sleutels en geheimen, samen met de bijbehorende Vm's.

- Uw Key Vault is gekoppeld aan de Azure AD-Tenant van het Azure-abonnement. Als u lid bent van een **gebruiker**, verkrijgt Azure Backup toegang tot de Key Vault zonder verdere actie.
- Als u een **gast gebruiker**bent, moet u machtigingen voor Azure backup geven voor toegang tot de sleutel kluis.

Machtigingen instellen:

1. Selecteer in het Azure Portal **alle services**en zoek naar **sleutel kluizen**.
2. Selecteer de sleutel kluis die is gekoppeld aan de versleutelde virtuele machine waarvan u een back-up maakt.
3. Selecteer **toegangs beleid** > **Nieuw toevoegen**.
4. Selecteer **Principal selecteren**en typ **back-upbeheer**.
5. Selecteer **back-upbeheer Service** > **selecteren**.

    ![Back-upservice selecteren](./media/backup-azure-vms-encryption/select-backup-service.png)

6. Selecteer **Azure backup**in beleid voor het **toevoegen van toegang** > **van sjabloon configureren (optioneel)** .
    - De vereiste machtigingen zijn vooraf ingevuld voor **sleutel machtigingen** en **geheime machtigingen**.
    - Als uw virtuele machine is versleuteld met **alleen bek**, verwijdert u de selectie voor **sleutel machtigingen** , omdat u alleen machtigingen voor geheimen nodig hebt.

    ![Azure backup-selectie](./media/backup-azure-vms-encryption/select-backup-template.png)

7. Klik op **OK**. **Backup Management-service** is toegevoegd aan het **toegangs beleid**.

    ![Toegangsbeleid](./media/backup-azure-vms-encryption/backup-service-access-policy.png)

8. Klik op **Opslaan** om Azure backup met de machtigingen op te geven.

## <a name="restore-an-encrypted-vm"></a>Een versleutelde VM herstellen

U herstelt versleutelde Vm's als volgt:

1. [Herstel de VM-schijf](backup-azure-arm-restore-vms.md#restore-disks).
2. Voer vervolgens een van de volgende handelingen uit:
    - Gebruik de sjabloon die tijdens de herstel bewerking wordt gegenereerd om de VM-instellingen aan te passen en VM-implementatie te activeren. [Meer informatie](backup-azure-arm-restore-vms.md#use-templates-to-customize-a-restored-vm).
    - Maak een nieuwe virtuele machine op basis van de herstelde schijven met behulp van Power shell. [Meer informatie](backup-azure-vms-automation.md#create-a-vm-from-restored-disks).
    - Stel voor Linux-Vm's de ADE-extensie opnieuw in, zodat de gegevens schijven zijn geopend en gekoppeld.

## <a name="next-steps"></a>Volgende stappen

Als u problemen ondervindt, raadpleegt u de volgende artikelen:

- [Veelvoorkomende fouten](backup-azure-vms-troubleshoot.md) bij het maken van back-ups en herstellen van versleutelde virtuele Azure-machines.
- Problemen met de [Azure-VM-agent/back-upextensie](backup-azure-troubleshoot-vm-backup-fails-snapshot-timeout.md) .
