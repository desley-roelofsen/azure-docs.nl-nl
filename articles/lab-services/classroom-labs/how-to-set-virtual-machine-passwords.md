---
title: Wacht woorden instellen voor virtuele machines in Azure Lab Services | Microsoft Docs
description: Meer informatie over het instellen en opnieuw instellen van wacht woorden voor virtuele machines (Vm's) in klassikale Labs van Azure Lab Services.
services: lab-services
documentationcenter: na
author: spelluru
manager: ''
editor: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/31/2019
ms.author: spelluru
ms.openlocfilehash: d89d506d8912706bbdb802801b16d01036ecb8e2
ms.sourcegitcommit: f4d8f4e48c49bd3bc15ee7e5a77bee3164a5ae1b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/04/2019
ms.locfileid: "73583493"
---
# <a name="set-up-and-manage-virtual-machine-pool"></a>VM-pool instellen en beheren 
In dit artikel leest u hoe u de volgende taken kunt uitvoeren:

- Het aantal virtuele machines (Vm's) in het lab verhogen
- Alle Vm's of geselecteerde Vm's starten 
- Vm's opnieuw instellen

## <a name="update-the-lab-capacity"></a>De lab-capaciteit bijwerken
Voer de volgende stappen uit om de lab-capaciteit (het aantal virtuele machines in een Lab) te verhogen of te verlagen:

1. Selecteer op de pagina **groep van virtuele machines** de optie **lab-capaciteit: &lt;aantal&gt; machines**.
2. Voer het nieuwe **aantal vm's** in dat u wilt in het lab. Dit getal moet groter zijn dan of gelijk zijn aan het aantal gebruikers dat is geregistreerd in het lab. 
3. Selecteer vervolgens **Opslaan**. 

    ![Knop Start alles](../media/how-to-set-virtual-machine-passwords/number-of-vms-in-lab.png)
4. Als u de capaciteit hebt verhoogd, kunt u zien welke virtuele machine of Vm's er worden gemaakt. Als u de nieuwe virtuele machine in de lijst niet ziet, vernieuwt u de pagina. 

    ![Virtuele machine die wordt gemaakt](../media/how-to-set-virtual-machine-passwords/vm-being-created.png)

## <a name="start-vms"></a>Vm's starten

### <a name="start-ot-stop-all-vms"></a>Alle Vm's stoppen
1. Schakel over naar de pagina met de **virtuele-machine groep** . 
2. Selecteer **start alles** op de werk balk. 

    ![Knop Start alles](../media/how-to-set-virtual-machine-passwords/start-all-vms-button.png)
3. Nadat alle Vm's zijn gestart, kunt u alle Vm's stoppen door de knop **Alles stoppen** te selecteren op de werk balk. 

    ![Knop stoppen](../media/how-to-set-virtual-machine-passwords/stop-all-vms-button.png)

### <a name="start-selected-vms"></a>Geselecteerde Vm's starten
Er zijn twee manieren om geselecteerde Vm's te starten (een of meer). De eerste manier is om de virtuele machine of Vm's te selecteren in de lijst en vervolgens **Start** te selecteren op de werk balk. 

De tweede manier is om een of meer virtuele machines te selecteren in de lijst en te scha kelen tussen de knop in de kolom **status** . 

![Geselecteerde Vm's starten](../media/how-to-set-virtual-machine-passwords/start-selected-vms.png)

Op dezelfde manier kunt u een of meer Vm's stoppen door te klikken op de knop in de kolom **status** of **Stop** te selecteren op de werk balk. 

## <a name="reset-vms"></a>Vm's opnieuw instellen
Als u een of meer Vm's opnieuw wilt instellen, selecteert u deze in de lijst en selecteert u vervolgens **opnieuw instellen** op de werk balk. 

![Geselecteerde Vm's opnieuw instellen](../media/how-to-set-virtual-machine-passwords/reset-vm-button.png)

Selecteer **opnieuw instellen**in het dialoog venster **virtuele machine (s) opnieuw instellen** . 

![Het dialoog venster VM opnieuw instellen](../media/how-to-set-virtual-machine-passwords/reset-vms-dialog.png)



## <a name="set-password-for-vms"></a>Wacht woord instellen voor Vm's
Een Lab-eigenaar (docent) kan het wacht woord voor Vm's instellen of opnieuw instellen op het moment van het maken van de Lab (wizard Lab maken) of nadat u het Lab hebt gemaakt op de **sjabloon** pagina. 

### <a name="set-password-at-the-time-of-lab-creation"></a>Wacht woord instellen op het moment dat het lab wordt gemaakt
Een Lab-eigenaar (docent) kan een wacht woord voor virtuele machines in het lab instellen op de pagina referenties van de **virtuele machine** van de wizard voor het maken van het lab.

![Nieuw Lab-venster](../media/tutorial-setup-classroom-lab/virtual-machine-credentials.png)

Door het **gebruik van hetzelfde wacht woord voor alle virtuele machines** op deze pagina in-of uit te scha kelen, kan een docent ervoor kiezen hetzelfde wacht woord te gebruiken voor alle vm's in het lab of dat studenten wacht woorden kunnen instellen voor hun vm's. Deze instelling is standaard ingeschakeld voor alle installatie kopieën van het Windows-en Linux-besturings systeem, met uitzonde ring van Ubuntu. Als deze instelling is uitgeschakeld, worden studenten gevraagd om een wacht woord in te stellen wanneer ze voor de eerste keer verbinding proberen te maken met de virtuele machine. 

### <a name="reset-password-later"></a>Wacht woord later opnieuw instellen

1. Selecteer op de pagina **sjabloon** van het lab de optie **wacht woord opnieuw instellen** op de werk balk. 
1. Voer in het dialoog venster **wacht woord opnieuw instellen** een wacht woord in en selecteer **wacht woord opnieuw instellen**.
    
    ![Het dialoog venster wacht woord instellen](../media/how-to-set-virtual-machine-passwords/set-password.png)

## <a name="connect-to-student-vms"></a>Verbinding maken met Vm's van studenten
De Lab Creator (docent/docent) kan verbinding maken met een student-VM als aan de volgende voor waarden wordt voldaan: 

- De optie **hetzelfde wacht woord voor alle virtuele machines gebruiken** is geselecteerd tijdens het maken van het lab
- De virtuele machine wordt uitgevoerd 

 Als u verbinding wilt maken met de student-VM, houdt u de muis aanwijzer op de virtuele machine in de lijst en selecteert u de knop computer.  

![Knop verbinding maken met de VM van student](../media/how-to-set-virtual-machine-passwords/connect-student-vm.png)

## <a name="next-steps"></a>Volgende stappen
Zie het volgende artikel: het [gebruik van studenten configureren](how-to-configure-student-usage.md)voor meer informatie over andere gebruiks opties voor studenten die u (als een Lab-eigenaar) kunt configureren.

Zie [wacht woord instellen of opnieuw instellen voor virtuele machines in klassikale Labs (studenten)](how-to-set-virtual-machine-passwords-student.md)voor meer informatie over hoe studenten wacht woorden opnieuw kunnen instellen voor hun vm's.