---
title: Back-ups van Azure-VM'S beheren en bewaken
description: Meer informatie over het beheren en bewaken van back-ups van Azure-VM'S met behulp van de Azure Backup-service.
ms.reviewer: sogup
ms.topic: conceptual
ms.date: 09/18/2019
ms.openlocfilehash: 3c4c4341286b7ee4f382192d291e08c83ff64994
ms.sourcegitcommit: 4821b7b644d251593e211b150fcafa430c1accf0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/19/2019
ms.locfileid: "74172979"
---
# <a name="manage-azure-vm-backups-with-azure-backup-service"></a>Back-ups van Azure-VM'S beheren met Azure Backup-Service

In dit artikel wordt beschreven hoe u virtuele Azure-machines (Vm's) beheert waarvan een back-up is gemaakt met behulp van de [Azure backup-service](backup-overview.md). Het artikel bevat ook een overzicht van de back-upgegevens die u op het kluis dashboard kunt vinden.

In de Azure Portal biedt het Recovery Services kluis-dash board toegang tot kluis gegevens, waaronder:

* De meest recente back-up, die ook het meest recente herstel punt is.
* Het back-upbeleid.
* De totale grootte van alle back-upmomentopnamen.
* Het aantal Vm's dat is ingeschakeld voor back-ups.

U kunt back-ups beheren met behulp van het dash board en door in te zoomen op afzonderlijke Vm's. Open de kluis op het dash board om te beginnen met het maken van back-ups van de machine.

![Volledige dashboard weergave met schuif regelaar](./media/backup-azure-manage-vms/bottom-slider.png)

## <a name="view-vms-on-the-dashboard"></a>Vm's in het dash board weer geven

Vm's op het kluis dashboard weer geven:

1. Meld u aan bij de [Azure Portal](https://portal.azure.com/).
2. Klik in het menu hub op **Bladeren**. Typ in de lijst met resources **Recovery Services**. Terwijl u typt, wordt de lijst gefilterd op basis van uw invoer. Selecteer **Recovery Services kluizen**.

    ![Een Recovery Services-kluis maken](./media/backup-azure-manage-vms/browse-to-rs-vaults.png)

3. Voor gebruiks gemak klikt u met de rechter muisknop op de kluis en selecteert **u vastmaken aan dash board**.
4. Open het kluis dashboard.

    ![Open de Blade kluis dashboard en instellingen](./media/backup-azure-manage-vms/full-view-rs-vault.png)

5. Selecteer **Azure virtual machines**op de tegel **Back-upitems** .

    ![De tegel back-upitems openen](./media/backup-azure-manage-vms/contoso-vault-1606.png)

6. Op de Blade **Back-upitems** kunt u de lijst met beveiligde vm's weer geven. In dit voor beeld beveiligt de kluis één virtuele machine: demobackup.  

    ![De Blade back-upitems weer geven](./media/backup-azure-manage-vms/backup-items-blade-select-item.png)

7. In het dash board van het kluis-item kunt u het back-upbeleid wijzigen, een back-up op aanvraag uitvoeren, de beveiliging van Vm's stoppen of hervatten, back-upgegevens verwijderen, herstel punten weer geven en een herstel bewerking uitvoeren.

    ![Het dash board back-upitems en de Blade instellingen](./media/backup-azure-manage-vms/item-dashboard-settings.png)

## <a name="manage-backup-policy-for-a-vm"></a>Back-upbeleid voor een VM beheren

Een back-upbeleid beheren:

1. Meld u aan bij de [Azure Portal](https://portal.azure.com/). Open het kluis dashboard.
2. Selecteer **Azure virtual machines**op de tegel **Back-upitems** .

    ![De tegel back-upitems openen](./media/backup-azure-manage-vms/contoso-vault-1606.png)

3. Op de Blade **Back-upitems** kunt u de lijst met beveiligde vm's en de laatste back-upstatus met de meest recente tijd voor herstel punten weer geven.

    ![De Blade back-upitems weer geven](./media/backup-azure-manage-vms/backup-items-blade-select-item.png)

4. Vanuit het dash board van het kluis item kunt u een back-upbeleid selecteren.

   * Als u wilt scha kelen tussen beleid, selecteert u een ander beleid en selecteert u vervolgens **Opslaan**. Het nieuwe beleid wordt onmiddellijk op de kluis toegepast.

     ![Een back-upbeleid kiezen](./media/backup-azure-manage-vms/backup-policy-create-new.png)

## <a name="run-an-on-demand-backup"></a>Een back-up op aanvraag uitvoeren

U kunt een back-up op aanvraag uitvoeren van een virtuele machine nadat u de beveiliging hebt ingesteld. Houd deze details in acht:

* Als de eerste back-up in behandeling is, maakt back-ups op aanvraag een volledige kopie van de virtuele machine in de Recovery Services kluis.
* Als de eerste back-up is voltooid, worden met een back-up op aanvraag alleen wijzigingen van de vorige moment opname naar de Recovery Services kluis verzonden. Dat wil zeggen dat latere back-ups altijd incrementeel zijn.
* De Bewaar termijn voor een back-up op aanvraag is de Bewaar waarde die u opgeeft wanneer u de back-up triggert.

Een back-up op aanvraag activeren:

1. Selecteer **back-upitem**onder **beveiligd item**op het [kluis-item dashboard](#view-vms-on-the-dashboard).

    ![De optie nu back-up maken](./media/backup-azure-manage-vms/backup-now-button.png)

2. Selecteer **Azure virtual machine**in het **type back-upbeheer**. De Blade **back-upitem (virtuele machine van Azure)** wordt weer gegeven.
3. Selecteer een virtuele machine en selecteer **Nu back-up** om een back-up op aanvraag te maken. De Blade **nu een back-up maken** wordt weer gegeven.
4. Geef in het veld **Backup-kassa bewaren** een datum op voor de back-up die moet worden bewaard.

    ![De agenda nu een back-up maken](./media/backup-azure-manage-vms/backup-now-check.png)

5. Selecteer **OK** om de back-uptaak uit te voeren.

Als u de voortgang van de taak wilt bijhouden, selecteert u in het kluis dashboard de tegel **back-uptaken** .

## <a name="stop-protecting-a-vm"></a>Beveiliging van een virtuele machine stoppen

Er zijn twee manieren om het beveiligen van een virtuele machine te stoppen:

* **Stop de beveiliging en behoud de back-upgegevens**. Met deze optie worden alle toekomstige back-uptaken gestopt om uw VM te beveiligen. Azure Backup-Service behoudt echter de herstel punten waarvan een back-up is gemaakt.  U moet betalen om de herstel punten in de kluis te blijven (Zie [Azure backup prijzen](https://azure.microsoft.com/pricing/details/backup/) voor meer informatie). U kunt de VM zo nodig herstellen. Als u besluit de beveiliging van de virtuele machine te hervatten, kunt u de *back-* upoptie voor hervatten gebruiken.
* **Stop de beveiliging en verwijder de back-upgegevens**. Met deze optie worden alle toekomstige back-uptaken gestopt om uw virtuele machine te beschermen en alle herstel punten te verwijderen. U kunt de virtuele machine niet herstellen of u kunt de *back-* upoptie voor hervatten niet gebruiken.

>[!NOTE]
>Als u een gegevens bron verwijdert zonder back-ups te stoppen, zullen nieuwe back-ups mislukken. Oude herstel punten verlopen volgens het beleid, maar er wordt altijd één laatste herstel punt bewaard totdat u de back-ups stopt en de gegevens verwijdert.
>

### <a name="stop-protection-and-retain-backup-data"></a>Beveiliging stoppen en back-upgegevens behouden

De beveiliging stoppen en gegevens van een virtuele machine behouden:

1. Op het [dash board van het kluis item](#view-vms-on-the-dashboard)selecteert u **back-up stoppen**.
2. Kies **back-upgegevens behouden**en bevestig uw selectie als dat nodig is. Voeg een opmerking toe als u wilt. Als u niet zeker weet wat de naam van het item is, houdt u de muis aanwijzer boven het uitroep teken om de naam weer te geven.

    ![Back-upgegevens behouden](./media/backup-azure-manage-vms/retain-backup-data.png)

Een melding laat u weten dat de back-uptaken zijn gestopt.

### <a name="stop-protection-and-delete-backup-data"></a>Beveiliging stoppen en back-upgegevens verwijderen

De beveiliging stoppen en gegevens van een virtuele machine verwijderen:

1. Op het [dash board van het kluis item](#view-vms-on-the-dashboard)selecteert u **back-up stoppen**.
2. Kies **back-upgegevens verwijderen**en bevestig uw selectie als dat nodig is. Voer de naam van het back-upitem in en voeg indien gewenst een opmerking toe.

    ![Back-upgegevens verwijderen](./media/backup-azure-manage-vms/delete-backup-data1.png)

## <a name="resume-protection-of-a-vm"></a>De beveiliging van een virtuele machine hervatten

Als u de optie [beveiliging stoppen en back-upgegevens behouden](#stop-protection-and-retain-backup-data) hebt gekozen tijdens het stoppen van de beveiliging van de virtuele machine, kunt u **back-up hervatten**gebruiken. Deze optie is niet beschikbaar als u de optie [beveiliging stoppen en back-upgegevens verwijderen](#stop-protection-and-delete-backup-data) kiest of [back-upgegevens verwijdert](#delete-backup-data).

De beveiliging van een virtuele machine hervatten:

1. Op het [dash board van het kluis item](#view-vms-on-the-dashboard)selecteert u **back-up hervatten**.

2. Volg de stappen in [back-upbeleid beheren](#manage-backup-policy-for-a-vm) om het beleid voor de virtuele machine toe te wijzen. U hoeft het oorspronkelijke beveiligings beleid van de virtuele machine niet te kiezen.
3. Nadat u het back-upbeleid op de virtuele machine hebt toegepast, wordt het volgende bericht weer gegeven:

    ![Bericht dat een beveiligde VM aangeeft](./media/backup-azure-manage-vms/success-message.png)

## <a name="delete-backup-data"></a>Back-upgegevens verwijderen

Er zijn twee manieren om de back-upgegevens van een VM te verwijderen:

* Selecteer in het dash board van het kluis-item back-up stoppen en volg de instructies voor het stoppen van de [beveiliging en de optie back-upgegevens verwijderen](#stop-protection-and-delete-backup-data) .

  ![Back-up stoppen selecteren](./media/backup-azure-manage-vms/stop-backup-buttom.png)

* Selecteer back-upgegevens verwijderen uit het kluis-item dashboard. Deze optie is ingeschakeld als u hebt gekozen voor het stoppen van de [beveiliging en de optie back-upgegevens behouden](#stop-protection-and-retain-backup-data) tijdens het stoppen van de VM-beveiliging

  ![Selecteer back-up verwijderen](./media/backup-azure-manage-vms/delete-backup-buttom.png)

  * Selecteer **back-upgegevens verwijderen**op het [kluis-item dashboard](#view-vms-on-the-dashboard).
  * Typ de naam van het back-upitem om te bevestigen dat u de herstel punten wilt verwijderen.

    ![Back-upgegevens verwijderen](./media/backup-azure-manage-vms/delete-backup-data1.png)

  * Selecteer **verwijderen**als u de back-upgegevens voor het item wilt verwijderen. Een meldings bericht laat u weten dat de back-upgegevens zijn verwijderd.

  > [!NOTE]
  > Wanneer u back-upgegevens verwijdert, verwijdert u alle gekoppelde herstel punten. U kunt geen specifieke herstel punten kiezen die u wilt verwijderen.

### <a name="backup-item-where-primary-data-source-no-longer-exists"></a>Back-upitem waarbij primaire gegevens bron niet meer bestaat

* Als Azure-Vm's die zijn geconfigureerd voor Azure backup, worden verwijderd of verplaatst zonder de beveiliging te stoppen, mislukken de back-uptaken van geplande back-uptaken en op aanvraag (ad-hoc) met de fout UserErrorVmNotFoundV2. De pre-controle van de back-up wordt alleen als kritiek weer gegeven voor mislukte back-uptaken op aanvraag (mislukte geplande taken worden niet weer gegeven).
* Deze back-upitems blijven actief in het systeem dat voldoet aan het back-up-en bewaar beleid dat door de gebruiker is ingesteld. De back-upgegevens voor deze Azure-Vm's worden bewaard volgens het Bewaar beleid. De verlopen herstel punten (met uitzonde ring van het laatste herstel punt) worden gereinigd op basis van de Bewaar termijn die in het back-upbeleid is ingesteld.
* Gebruikers wordt aangeraden om de back-upitems te verwijderen waar de primaire gegevens bron niet meer bestaat om extra kosten te voor komen. als het back-upbestand/de gegevens voor de verwijderings resources niet langer zijn vereist, wordt het laatste herstel punt permanent bewaard en wordt de gebruiker in rekening gebracht volgens de prijzen voor back-ups die van toepassing zijn.

## <a name="next-steps"></a>Volgende stappen

* Meer informatie over [het maken van een back-up van Azure-vm's vanuit de instellingen van de virtuele machine](backup-azure-vms-first-look-arm.md).
* Meer informatie over het [herstellen van vm's](backup-azure-arm-restore-vms.md).
* Meer informatie over het [bewaken van back-ups van Azure-vm's](backup-azure-monitor-vms.md).
