---
title: Windows 7 virtuele machine implementeren Windows virtueel bureau blad-Azure
description: Een virtuele machine met Windows 7 configureren en implementeren op het virtuele bureau blad van Windows.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: conceptual
ms.date: 10/03/2019
ms.author: helohr
ms.openlocfilehash: 90b871c2b75f7ed40c290231ef822258c6b4e6d4
ms.sourcegitcommit: c62a68ed80289d0daada860b837c31625b0fa0f0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/05/2019
ms.locfileid: "73606876"
---
# <a name="deploy-a-windows-7-virtual-machine-on-windows-virtual-desktop"></a>Een virtuele Windows 7-machine implementeren in Windows Virtual Desktop

Het proces voor het implementeren van een virtuele machine met Windows 7 op Windows virtueel bureau blad wijkt enigszins af van de virtuele machines waarop latere versies van Windows worden uitgevoerd. In deze hand leiding wordt uitgelegd hoe u Windows 7 implementeert.

## <a name="prerequisites"></a>Vereisten

Voordat u begint, volgt u de instructies in [een hostgroep met Power shell maken](create-host-pools-powershell.md) om een hostgroep te maken. Volg daarna de instructies in [host Pools maken in azure Marketplace](create-host-pools-azure-marketplace.md#optional-assign-additional-users-to-the-desktop-application-group) om een of meer gebruikers toe te wijzen aan de groep bureau blad-toepassingen.

## <a name="configure-a-windows-7-virtual-machine"></a>Een virtuele machine met Windows 7 configureren

Wanneer u klaar bent met de vereisten, kunt u uw Windows 7-VM configureren voor implementatie op virtueel bureau blad van Windows.

Een Windows 7-VM instellen op het virtuele bureau blad van Windows:

1. Meld u aan bij de Azure Portal en zoek naar de installatie kopie van Windows 7 Enter prise of upload uw eigen aangepaste Windows 7 Enter prise (x64)-installatie kopie.  
2. Implementeer een of meer virtuele machines met Windows 7 Enter prise als hostbesturingssysteem. Zorg ervoor dat de virtuele machines Remote Desktop Protocol (RDP) toestaan (de TCP/3389-poort).
3. Maak verbinding met de Windows 7 Enter prise-host met behulp van het RDP en verificatie met de referenties die u hebt gedefinieerd tijdens het configureren van uw implementatie. 
4. Voeg het account dat u hebt gebruikt bij het maken van een verbinding met de host met RDP toe aan de groep Extern bureaublad gebruiker. Als u dit niet doet, kunt u mogelijk geen verbinding maken met de virtuele machine nadat u deze hebt toegevoegd aan uw Active Directory domein.
5. Ga naar Windows Update op uw VM.
6. Installeer alle Windows-updates in de belang rijke categorie.
7. Installeer alle Windows-updates in de optionele categorie (exclusief taal pakketten). Hiermee installeert u de Remote Desktop Protocol 8,0-update ([KB2592687](https://www.microsoft.com/download/details.aspx?id=35393)) die u nodig hebt om deze instructies te volt ooien.
8. Open de Lokale groepsbeleidsobjecteditor en navigeer naar **computer configuratie** > **Beheersjablonen** > **Windows-onderdelen** > **extern bureaublad-services** > extern bureaublad **sessiehost**  > **externe sessie omgeving**.
9. Schakel het beleid voor Remote Desktop Protocol 8,0 in.
10. Start de virtuele machine opnieuw op door de volgende opdracht uit te voeren:
    
     ```cmd
     shutdown /r /t 0
     ```
    
11. Volg de instructies [hier](https://docs.microsoft.com/powershell/module/windowsvirtualdesktop/export-rdsregistrationinfo) om een registratie token op te halen.
12. [Down load de Windows-agent voor virtueel bureau blad voor Windows 7](https://query.prod.cms.rt.microsoft.com/cms/api/am/binary/RE3JZCm).
13. [Down load de Windows Virtual Desktop Agent Manager voor Windows 7](https://query.prod.cms.rt.microsoft.com/cms/api/am/binary/RE3K2e3).
14. Open het installatie programma voor de virtueel-bureaublad agent van Windows en volg de instructies. Wanneer u hierom wordt gevraagd, geeft u de registratie sleutel op die u in stap 11 hebt gemaakt.
15. Open het Windows-installatie programma voor virtueel bureau blad en volg de instructies.
16. U kunt desgewenst de TCP/3389-poort blok keren om direct Remote Desktop Protocol toegang tot de virtuele machine te verwijderen.

## <a name="next-steps"></a>Volgende stappen

Uw Windows-implementatie voor virtueel bureau blad is nu klaar voor gebruik. [Down load de nieuwste versie van de Windows Virtual Desktop-Client](https://aka.ms/wvd/clients/windows) om aan de slag te gaan.

Voor een lijst met bekende problemen en instructies voor het oplossen van problemen met Windows 7 op virtueel bureau blad van Windows, raadpleegt u ons artikel over probleem oplossing bij het [oplossen van problemen met Windows 7 virtual machines in Windows virtueel bureau blad](troubleshoot-windows-7-vm.md).
