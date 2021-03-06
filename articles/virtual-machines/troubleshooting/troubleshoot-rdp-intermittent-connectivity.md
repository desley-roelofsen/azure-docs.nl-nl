---
title: Extern bureaublad verbreekt regel matig de verbinding met de Azure-VM | Microsoft Docs
description: Meer informatie over het oplossen van problemen met een regel matige verbinding van Extern bureaublad in azure VM.
services: virtual-machines-windows
documentationCenter: ''
author: genlin
manager: dcscontentpm
editor: ''
ms.service: virtual-machines-windows
ms.topic: troubleshooting
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 10/24/2018
ms.author: genli
ms.openlocfilehash: be563e39ed1bfa405830999a96d8630b6f8254bb
ms.sourcegitcommit: ca359c0c2dd7a0229f73ba11a690e3384d198f40
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/17/2019
ms.locfileid: "71057975"
---
# <a name="remote-desktop-disconnects-frequently-in-azure-vm"></a>Extern bureaublad verbreekt regel matig de verbinding met Azure VM

In dit artikel wordt uitgelegd hoe u veelvuldige verbindingen met een virtuele Azure-machine (VM) kunt oplossen via Remote Desktop Protocol RDP).

> [!NOTE] 
> Azure heeft twee verschillende implementatiemodellen voor het maken van en werken met resources: [Resource Manager en het klassieke model](../../azure-resource-manager/resource-manager-deployment-model.md). In dit artikel wordt beschreven hoe u het Resource Manager-implementatie model gebruikt. U wordt aangeraden dit model te gebruiken voor nieuwe implementaties in plaats van het klassieke implementatie model te gebruiken.

## <a name="symptom"></a>Symptoom

Tijdens uw sessies worden er onregelmatige RDP-verbindings problemen gegezicht. U kunt eerst verbinding maken met de virtuele machine, maar vervolgens wordt de verbinding verbroken.

## <a name="cause"></a>Oorzaak

Dit probleem kan optreden als de RDP-listener onjuist is geconfigureerd. Dit probleem treedt meestal op bij een virtuele machine die gebruikmaakt van een aangepaste installatie kopie.

## <a name="solution"></a>Oplossing

Voordat u deze stappen volgt, moet u [een moment opname maken van de besturingssysteem schijf](../windows/snapshot-copy-managed-disk.md) van de betrokken VM als back-up. 

U kunt dit probleem oplossen met behulp van serieel beheer of [de virtuele machine offline herstellen](#repair-the-vm-offline) door de besturingssysteem schijf van de virtuele machine aan een herstel-VM te koppelen.

### <a name="serial-control"></a>Seriële controle

1. Verbinding maken met [seriële Console- en CMD-instantie openen](./serial-console-windows.md). Voer vervolgens de volgende opdrachten uit om de RDP-configuraties opnieuw in te stellen. Als de seriële console niet is ingeschakeld op uw virtuele machine, gaat u naar de volgende stap.
2. Verminder de RDP-beveiligingslaag in 0. Bij deze instelling wordt de communicatie tussen de server en de client gebruikt voor de systeem eigen RDP-versleuteling.

        REG ADD "HKLM\SYSTEM\CurrentControlSet\control\Terminal Server\Winstations\RDP-Tcp" /v 'SecurityLayer' /t REG_DWORD /d 0 /f
3. Verlaag het versleutelings niveau tot de minimale instelling zodat verouderde RDP-clients verbinding kunnen maken.

        REG ADD "HKLM\SYSTEM\CurrentControlSet\control\Terminal Server\Winstations\RDP-Tcp" /v 'MinEncryptionLevel' /t REG_DWORD /d 1 /f
4. Stel RDP in om de gebruikers configuratie van de client computer te laden.

        REG ADD "HKLM\SYSTEM\CurrentControlSet\control\Terminal Server\Winstations\RDP-Tcp" /v 'fQueryUserConfigFromLocalMachine' /t REG_DWORD /d 1 /f
5. Schakel het besturings element RDP Keep-Alive in:

        REG ADD "HKLM\SYSTEM\CurrentControlSet\control\Terminal Server\Winstations\RDP-Tcp" /v 'KeepAliveTimeout' /t REG_DWORD /d 1 /f

        REG ADD "HKLM\SOFTWARE\Policies\Microsoft\Windows NT\Terminal Services" /v 'KeepAliveEnable' /t REG_DWORD /d 1 /f

        REG ADD "HKLM\SOFTWARE\Policies\Microsoft\Windows NT\Terminal Services" /v 'KeepAliveInterval' /t REG_DWORD /d 1 /f
6. Beheer van RDP-reconnect instellen:

        REG ADD "HKLM\SYSTEM\CurrentControlSet\control\Terminal Server\Winstations\RDP-Tcp" /v 'fInheritReconnectSame' /t REG_DWORD /d 0 /f

        REG ADD "HKLM\SYSTEM\CurrentControlSet\control\Terminal Server\Winstations\RDP-Tcp" /v 'fReconnectSame' /t REG_DWORD /d 1 /f

        REG ADD "HKLM\SOFTWARE\Policies\Microsoft\Windows NT\Terminal Services" /v 'fDisableAutoReconnect' /t REG_DWORD /d 0 /f
7. Het besturings element RDP-sessie tijd instellen:

        REG ADD "HKLM\SYSTEM\CurrentControlSet\control\Terminal Server\Winstations\RDP-Tcp" /v 'fInheritMaxSessionTime' /t REG_DWORD /d 1 /f
8. Het besturings element RDP-verbindings tijd instellen: 

        REG ADD "HKLM\SYSTEM\CurrentControlSet\control\Terminal Server\Winstations\RDP-Tcp" /v 'fInheritMaxDisconnectionTime' /t REG_DWORD /d 1 /f
        REG ADD "HKLM\SYSTEM\CurrentControlSet\control\Terminal Server\Winstations\RDP-Tcp" /v 'MaxDisconnectionTime' /t REG_DWORD /d 0 /f
9. Het besturings element RDP-verbindings tijd instellen:

        REG ADD "HKLM\SYSTEM\CurrentControlSet\control\Terminal Server\Winstations\RDP-Tcp" /v 'MaxConnectionTime' /t REG_DWORD /d 0 /f
10. Het besturings element voor niet-actieve tijd van de RDP-sessie instellen:

        REG ADD "HKLM\SYSTEM\CurrentControlSet\control\Terminal Server\Winstations\RDP-Tcp" /v 'fInheritMaxIdleTime' /t REG_DWORD /d 1 /f

        REG ADD "HKLM\SYSTEM\CurrentControlSet\control\Terminal Server\Winstations\RDP-Tcp" /v 'MaxIdleTime' /t REG_DWORD /d 0 /f
11. Stel het besturings element maximum aantal gelijktijdige verbindingen beperken in:

        REG ADD "HKLM\SYSTEM\CurrentControlSet\control\Terminal Server\Winstations\RDP-Tcp" /v 'MaxInstanceCount' /t REG_DWORD /d 4294967295 /f

12. Start de virtuele machine opnieuw op en probeer opnieuw verbinding te maken met behulp van RDP.

### <a name="repair-the-vm-offline"></a>Herstel de virtuele machine offline

1. [De besturingssysteemschijf koppelen aan een virtuele machine voor herstel](../windows/troubleshoot-recovery-disks-portal.md).
2. Nadat de besturingssysteemschijf is gekoppeld aan de virtuele machine voor herstel, zorg ervoor dat de schijf is gemarkeerd als **Online** in de Schijfbeheer-console. Houd er rekening mee de stationsletter die is toegewezen aan de gekoppelde besturingssysteemschijf.
3. Ga naar de map **\Windows\System32\Config** op de besturingssysteem schijf die u hebt gekoppeld. Kopieer alle bestanden in deze map als back-up als een terugdraai actie vereist is.
4. Start de REGI ster-editor (Regedit. exe).
5. Selecteer de sleutel **HKEY_LOCAL_MACHINE** . Selecteer in het menu **bestand** > **laden component**:
6. Blader naar de map **\windows\system32\config\SYSTEM** op de besturingssysteem schijf die u hebt toegevoegd. Voer **BROKENSYSTEM**in voor de naam van de component. De nieuwe register component wordt weer gegeven onder de sleutel **HKEY_LOCAL_MACHINE** . Laad vervolgens de software component **\windows\system32\config\SOFTWARE** onder de sleutel **HKEY_LOCAL_MACHINE** . Voer **BROKENSOFTWARE**in als de naam van de Hive-software. 
7. Open een opdracht prompt venster met verhoogde bevoegdheid (**als administrator uitvoeren**) en voer opdrachten uit in de resterende stappen om de RDP-configuraties opnieuw in te stellen. 
8. Verlaag de RDP-beveiligingslaag tot 0 zodat de communicatie tussen de server en de client de systeem eigen RDP-versleuteling gebruikt:

        REG ADD "HKLM\BROKENSYSTEM\ControlSet001\control\Terminal Server\Winstations\RDP-Tcp" /v 'SecurityLayer' /t REG_DWORD /d 0 /f

        REG ADD "HKLM\BROKENSYSTEM\ControlSet002\control\Terminal Server\Winstations\RDP-Tcp" /v 'SecurityLayer' /t REG_DWORD /d 0 /f
9. Verlaag het versleutelings niveau tot de minimale instelling zodat verouderde RDP-clients verbinding kunnen maken:

        REG ADD "HKLM\BROKENSYSTEM\ControlSet001\control\Terminal Server\Winstations\RDP-Tcp" /v 'MinEncryptionLevel' /t REG_DWORD /d 1 /f

        REG ADD "HKLM\BROKENSYSTEM\ControlSet002\control\Terminal Server\Winstations\RDP-Tcp" /v 'MinEncryptionLevel' /t REG_DWORD /d 1 /f
10. Stel RDP in om de gebruikers configuratie van de client computer te laden.

        REG ADD "HKLM\BROKENSYSTEM\ControlSet001\control\Terminal Server\Winstations\RDP-Tcp" /v 'fQueryUserConfigFromLocalMachine' /t REG_DWORD /d 1 /f

        REG ADD "HKLM\BROKENSYSTEM\ControlSet002\control\Terminal Server\Winstations\RDP-Tcp" /v 'fQueryUserConfigFromLocalMachine' /t REG_DWORD /d 1 /f
11. Schakel het besturings element RDP Keep-Alive in:

        REG ADD "HKLM\BROKENSYSTEM\ControlSet001\control\Terminal Server\Winstations\RDP-Tcp" /v 'KeepAliveTimeout' /t REG_DWORD /d 1 /f 
        
        REG ADD "HKLM\BROKENSYSTEM\ControlSet002\control\Terminal Server\Winstations\RDP-Tcp" /v 'KeepAliveTimeout' /t REG_DWORD /d 1 /f 

        REG ADD "HKLM\BROKENSOFTWARE\Policies\Microsoft\Windows NT\Terminal Services" /v 'KeepAliveEnable' /t REG_DWORD /d 1 /f 

        REG ADD "HKLM\BROKENSOFTWARE\Policies\Microsoft\Windows NT\Terminal Services" /v 'KeepAliveInterval' /t REG_DWORD /d 1 /f
12. Beheer van RDP-reconnect instellen:

        REG ADD "HKLM\BROKENSYSTEM\ControlSet001\control\Terminal Server\Winstations\RDP-Tcp" /v 'fInheritReconnectSame' /t REG_DWORD /d 0 /f 
        
        REG ADD "HKLM\BROKENSYSTEM\ControlSet001\control\Terminal Server\Winstations\RDP-Tcp" /v 'fReconnectSame' /t REG_DWORD /d 1 /f 

        REG ADD "HKLM\BROKENSYSTEM\ControlSet002\control\Terminal Server\Winstations\RDP-Tcp" /v 'fInheritReconnectSame' /t REG_DWORD /d 0 /f 

        REG ADD "HKLM\BROKENSYSTEM\ControlSet002\control\Terminal Server\Winstations\RDP-Tcp" /v 'fReconnectSame' /t REG_DWORD /d 1 /f 

        REG ADD "HKLM\BROKENSOFTWARE\Policies\Microsoft\Windows NT\Terminal Services" /v 'fDisableAutoReconnect' /t REG_DWORD /d 0 /f

13. Het besturings element RDP-sessie tijd instellen:

        REG ADD "HKLM\BROKENSYSTEM\ControlSet001\control\Terminal Server\Winstations\RDP-Tcp" /v 'fInheritMaxSessionTime' /t REG_DWORD /d 1 /f

        REG ADD "HKLM\BROKENSYSTEM\ControlSet002\control\Terminal Server\Winstations\RDP-Tcp" /v 'fInheritMaxSessionTime' /t REG_DWORD /d 1 /f
14. Het besturings element RDP-verbindings tijd instellen:

        REG ADD "HKLM\BROKENSYSTEM\ControlSet001\control\Terminal Server\Winstations\RDP-Tcp" /v 'fInheritMaxDisconnectionTime' /t REG_DWORD /d 1 /f 

        REG ADD "HKLM\BROKENSYSTEM\ControlSet001\control\Terminal Server\Winstations\RDP-Tcp" /v 'MaxDisconnectionTime' /t REG_DWORD /d 0 /f

        REG ADD "HKLM\BROKENSYSTEM\ControlSet002\control\Terminal Server\Winstations\RDP-Tcp" /v 'fInheritMaxDisconnectionTime' /t REG_DWORD /d 1 /f 

        REG ADD "HKLM\BROKENSYSTEM\ControlSet002\control\Terminal Server\Winstations\RDP-Tcp" /v 'MaxDisconnectionTime' /t REG_DWORD /d 0 /f
15. Het besturings element RDP-verbindings tijd instellen:

        REG ADD "HKLM\BROKENSYSTEM\ControlSet001\control\Terminal Server\Winstations\RDP-Tcp" /v 'MaxConnectionTime' /t REG_DWORD /d 0 /f

        REG ADD "HKLM\BROKENSYSTEM\ControlSet002\control\Terminal Server\Winstations\RDP-Tcp" /v 'MaxConnectionTime' /t REG_DWORD /d 0 /f
16. Het besturings element voor niet-actieve tijd van de RDP-sessie instellen:     REG ADD "HKLM\BROKENSYSTEM\ControlSet001\control\Terminal Server\Winstations\RDP-Tcp" /v 'fInheritMaxIdleTime' /t REG_DWORD /d 1 /f 

        REG ADD "HKLM\BROKENSYSTEM\ControlSet001\control\Terminal Server\Winstations\RDP-Tcp" /v ' MaxIdleTime' /t REG_DWORD /d 0 /f

        REG ADD "HKLM\BROKENSYSTEM\ControlSet002\control\Terminal Server\Winstations\RDP-Tcp" /v 'fInheritMaxIdleTime' /t REG_DWORD /d 1 /f 

        REG ADD "HKLM\BROKENSYSTEM\ControlSet002\control\Terminal Server\Winstations\RDP-Tcp" /v ' MaxIdleTime' /t REG_DWORD /d 0 /f
17. Stel het besturings element maximum aantal gelijktijdige verbindingen beperken in:

        REG ADD "HKLM\BROKENSYSTEM\ControlSet001\control\Terminal Server\Winstations\RDP-Tcp" /v 'MaxInstanceCount' /t REG_DWORD /d ffffffff /f

        REG ADD "HKLM\BROKENSYSTEM\ControlSet002\control\Terminal Server\Winstations\RDP-Tcp" /v 'MaxInstanceCount' /t REG_DWORD /d ffffffff /f
18. Start de virtuele machine opnieuw op en probeer opnieuw verbinding te maken met behulp van RDP.

## <a name="need-help"></a>Hulp nodig? 
Neem contact op met ondersteuning. Als u nog steeds hulp nodig hebt, neemt u [contact op met de ondersteuning](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) om uw probleem snel op te lossen.





