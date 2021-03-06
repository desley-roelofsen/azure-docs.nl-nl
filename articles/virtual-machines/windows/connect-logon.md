---
title: Verbinding maken met een Windows Server-VM
description: Meer informatie over hoe u verbinding maakt met een Windows-VM met behulp van het Azure Portal en het Resource Manager-implementatie model.
services: virtual-machines-windows
documentationcenter: ''
author: cynthn
manager: gwallace
editor: tysonn
tags: azure-resource-manager
ms.assetid: ef62b02e-bf35-468d-b4c3-71b63fe7f409
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.topic: article
ms.date: 11/26/2018
ms.author: cynthn
ms.openlocfilehash: 152df830f11cd5a73235559c5c5d65ced44f22fa
ms.sourcegitcommit: c38a1f55bed721aea4355a6d9289897a4ac769d2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/05/2019
ms.locfileid: "74842115"
---
# <a name="how-to-connect-and-sign-on-to-an-azure-virtual-machine-running-windows"></a>Verbinding maken en aanmelden bij een virtuele machine van Azure waarop Windows wordt uitgevoerd
U gebruikt de knop **Verbinden** in Azure Portal om een Extern bureaublad-sessie (RDP) te starten vanaf een Windows-computer. Eerst maakt u verbinding met de virtuele machine en meldt u zich aan.

Als u vanaf een Mac verbinding wilt maken met een Windows-VM, moet u een RDP-client voor Mac, zoals [Microsoft extern bureaublad](https://aka.ms/rdmac), installeren.

## <a name="connect-to-the-virtual-machine"></a>Verbinding maken met de virtuele machine
1. Ga naar de [Azure Portal](https://portal.azure.com/) om verbinding te maken met een virtuele machine. Zoek en selecteer **virtuele machines**.
2. Selecteer de virtuele machine in de lijst.
3. Selecteer op het begin van de pagina virtuele machine **verbinding maken**.
4. Selecteer op de pagina **verbinding maken met virtuele machine** **RDP**en selecteer vervolgens het juiste **IP-adres** en **poort nummer**. In de meeste gevallen moeten het IP-adres en de standaard poort worden gebruikt. Selecteer **RDP-bestand downloaden**. Als er een just-in-time-beleid is ingesteld voor de VM, moet u eerst de knop **toegang aanvragen** selecteren om toegang aan te vragen voordat u het RDP-bestand kunt downloaden. Zie [toegang tot virtuele machines beheren met de just-in-time-beleids regels](../../security-center/security-center-just-in-time.md)voor meer informatie over het just-in-time-beleid.
5. Open het gedownloade RDP-bestand en selecteer **Verbinden** wanneer dit wordt gevraagd. U krijgt een waarschuwing dat het `.rdp` bestand van een onbekende uitgever is. Dit is normaal gedrag. Selecteer in het venster **verbinding met extern bureaublad** de optie **verbinding maken** om door te gaan.
   
    ![Schermafbeelding met waarschuwing over een onbekende uitgever](./media/connect-logon/rdp-warn.png)
3. Selecteer in het venster **Windows-beveiliging** **Meer opties** en vervolgens **Een ander account gebruiken**. Voer de referenties voor een account op de virtuele machine in en selecteer **OK**.
   
     **Lokaal account**: dit is doorgaans de gebruikers naam en het wacht woord voor het lokale account dat u hebt opgegeven tijdens het maken van de virtuele machine. In dit geval is het domein de naam van de virtuele machine en het is ingevoerd als *vmname*&#92;*gebruikersnaam*.  
   
    Aan een **domein gekoppelde VM**: als de virtuele machine bij een domein hoort, voert u de gebruikers naam in de notatie *domein*&#92;*gebruikers*naam in. Het account moet bovendien in de groep Administrators staan of er moet een machtiging voor externe toegang zijn verleend aan de VM.
   
    **Domein controller**: als de VM een domein controller is, voert u de gebruikers naam en het wacht woord in van een domein beheerders account voor dat domein.
4. Selecteer **Ja** om de identiteit van de virtuele machine te controleren en de registratie te volt ooien.
   
   ![Schermafbeelding met een bericht over het verifiëren van de identiteit van de virtuele machine](./media/connect-logon/cert-warning.png)


   > [!TIP]
   > Als de knop **verbinden** in de portal grijs wordt weer gegeven en u niet met Azure bent verbonden via een [snelle route](../../expressroute/expressroute-introduction.md) of een [site-naar-site-VPN-](../../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md) verbinding, moet u een openbaar IP-adres maken en toewijzen aan uw virtuele machine voordat u RDP kunt gebruiken. Zie [open bare IP-adressen in azure](../../virtual-network/virtual-network-ip-addresses-overview-arm.md)voor meer informatie.
   > 
   > 

## <a name="connect-to-the-virtual-machine-using-powershell"></a>Verbinding maken met de virtuele machine met behulp van Power shell

 

Als u Power shell gebruikt en de module Azure PowerShell geïnstalleerd hebt, kunt u ook verbinding maken via de `Get-AzRemoteDesktopFile`-cmdlet, zoals hieronder wordt weer gegeven.

In dit voor beeld wordt de RDP-verbinding onmiddellijk gestart, waarbij u dezelfde prompts krijgt als hierboven.

```powershell
Get-AzRemoteDesktopFile -ResourceGroupName "RgName" -Name "VmName" -Launch
```

U kunt het RDP-bestand ook opslaan voor toekomstig gebruik.

```powershell
Get-AzRemoteDesktopFile -ResourceGroupName "RgName" -Name "VmName" -LocalPath "C:\Path\to\folder"
```

## <a name="next-steps"></a>Volgende stappen
Zie [problemen met extern bureaublad verbindingen oplossen](troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)als u problemen ondervindt bij het maken van verbinding. 

