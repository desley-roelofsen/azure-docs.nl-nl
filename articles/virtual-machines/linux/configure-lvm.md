---
title: LVM configureren op een virtuele machine met Linux
description: Meer informatie over het configureren van LVM op Linux in Azure.
services: virtual-machines-linux
documentationcenter: na
author: szarkos
manager: gwallace
editor: tysonn
tag: azure-service-management,azure-resource-manager
ms.assetid: 7f533725-1484-479d-9472-6b3098d0aecc
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.topic: article
ms.date: 09/27/2018
ms.author: szark
ms.subservice: disks
ms.openlocfilehash: f2774f0037d2655071b605c0cbcdf8122e66f6e7
ms.sourcegitcommit: 49cf9786d3134517727ff1e656c4d8531bbbd332
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/13/2019
ms.locfileid: "74036672"
---
# <a name="configure-lvm-on-a-linux-vm-in-azure"></a>LVM configureren op een virtuele Linux-machine in azure
In dit document wordt beschreven hoe u Logical Volume Manager (LVM) configureert in uw virtuele Azure-machine. LVM kan worden gebruikt op de besturingssysteem schijf of gegevens schijven in azure-Vm's, maar in de meeste Cloud installatie kopieën is LVM niet geconfigureerd op de besturingssysteem schijf. Met de volgende stappen wordt de nadruk gelegd op het configureren van LVM voor uw gegevens schijven.

## <a name="linear-vs-striped-logical-volumes"></a>Lineaire versus gestripte logische volumes
LVM kan worden gebruikt om een aantal fysieke schijven te combi neren in één opslag volume. Standaard worden in LVM doorgaans lineaire logische volumes gemaakt. Dit betekent dat de fysieke opslag samen wordt samengevoegd. In dit geval worden lees-en schrijf bewerkingen normaal gesp roken alleen naar één schijf verzonden. Met contrast kunnen we ook gestripde logische volumes maken waarbij Lees-en schrijf bewerkingen worden gedistribueerd naar meerdere schijven die zijn opgenomen in de volume groep (vergelijkbaar met RAID0). Uit het oogpunt van prestaties is het waarschijnlijk dat u uw logische volumes wilt verwijderen zodat Lees-en schrijf bewerkingen gebruikmaken van alle gekoppelde gegevens schijven.

In dit document wordt beschreven hoe u verschillende gegevens schijven kunt combi neren in één volume groep en vervolgens een gesegmenteerd logisch volume maakt. De onderstaande stappen zijn gegeneraliseerd voor het werken met de meeste distributies. In de meeste gevallen zijn de hulpprogram ma's en werk stromen voor het beheren van LVM in azure niet fundamenteel anders dan andere omgevingen. Zoals gebruikelijk raadpleegt u ook uw Linux-leverancier voor documentatie en aanbevolen procedures voor het gebruik van LVM met uw specifieke distributie.

## <a name="attaching-data-disks"></a>Gegevens schijven koppelen
Normaal gesp roken wilt u met twee of meer lege gegevens schijven beginnen wanneer LVM wordt gebruikt. Op basis van uw IO-behoeften kunt u ervoor kiezen om schijven die zijn opgeslagen in onze standaard opslag te koppelen, met Maxi maal 500 IO/PS per schijf of onze Premium-opslag, met Maxi maal 5000 IO/PS per schijf. In dit artikel vindt u geen gedetailleerde informatie over het inrichten en koppelen van gegevens schijven aan een virtuele Linux-machine. Raadpleeg het Microsoft Azure artikel [een schijf koppelen](add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) voor gedetailleerde instructies over het koppelen van een lege gegevens schijf aan een virtuele Linux-machine in Azure.

## <a name="install-the-lvm-utilities"></a>De LVM-hulpprogram ma's installeren
* **Ubuntu**

    ```bash  
    sudo apt-get update
    sudo apt-get install lvm2
    ```

* **RHEL, CentOS & Oracle Linux**

    ```bash   
    sudo yum install lvm2
    ```

* **SLES 12 en openSUSE**

    ```bash   
    sudo zypper install lvm2
    ```

* **SLES 11**

    ```bash   
    sudo zypper install lvm2
    ```

    Op SLES11 moet u ook `/etc/sysconfig/lvm` bewerken en `LVM_ACTIVATED_ON_DISCOVERED` instellen op ' inschakelen ':

    ```sh   
    LVM_ACTIVATED_ON_DISCOVERED="enable" 
    ```

## <a name="configure-lvm"></a>LVM configureren
In deze hand leiding wordt ervan uitgegaan dat u drie gegevens schijven hebt aangesloten, waarnaar wordt verwezen als `/dev/sdc`, `/dev/sdd` en `/dev/sde`. Deze paden komen mogelijk niet overeen met de namen van schijf paden in uw VM. U kunt '`sudo fdisk -l`' of een vergelijk bare opdracht uitvoeren om een lijst met beschik bare schijven weer te geven.

1. De fysieke volumes voorbereiden:

    ```bash    
    sudo pvcreate /dev/sd[cde]
    Physical volume "/dev/sdc" successfully created
    Physical volume "/dev/sdd" successfully created
    Physical volume "/dev/sde" successfully created
    ```

2. Maak een volume groep. In dit voor beeld roept u de volume groep `data-vg01`:

    ```bash    
    sudo vgcreate data-vg01 /dev/sd[cde]
    Volume group "data-vg01" successfully created
    ```

3. Maak een of meer logische volumes. Met de onderstaande opdracht maakt u één logisch volume met de naam `data-lv01` om de hele volume groep te bezorgen, maar is het ook haalbaar om meerdere logische volumes in de volume groep te maken.

    ```bash   
    sudo lvcreate --extents 100%FREE --stripes 3 --name data-lv01 data-vg01
    Logical volume "data-lv01" created.
    ```

4. Het logische volume Format teren

    ```bash  
    sudo mkfs -t ext4 /dev/data-vg01/data-lv01
    ```
   
   > [!NOTE]
   > Met SLES11 wordt `-t ext3` gebruikt in plaats van ext4. SLES11 biedt alleen ondersteuning voor alleen-lezen toegang tot ext4-bestands systemen.

## <a name="add-the-new-file-system-to-etcfstab"></a>Het nieuwe bestands systeem toevoegen aan bestand/etc/fstab
> [!IMPORTANT]
> Het onjuist bewerken van het `/etc/fstab` bestand kan resulteren in een systeem dat niet kan worden opgestart. Als dat niet het geval is, raadpleegt u de documentatie van de distributie voor informatie over het correct bewerken van dit bestand. U wordt ook aangeraden een back-up van het `/etc/fstab`-bestand te maken voordat u het bewerkt.

1. Maak het gewenste koppel punt voor het nieuwe bestands systeem, bijvoorbeeld:

    ```bash  
    sudo mkdir /data
    ```

2. Het pad naar het logische volume zoeken

    ```bash    
    lvdisplay
    --- Logical volume ---
    LV Path                /dev/data-vg01/data-lv01
    ....
    ```

3. Open `/etc/fstab` in een tekst editor en voeg een vermelding toe voor het nieuwe bestands systeem, bijvoorbeeld:

    ```bash    
    /dev/data-vg01/data-lv01  /data  ext4  defaults  0  2
    ```   
    Sla `/etc/fstab`op en sluit het af.

4. Test of de `/etc/fstab` vermelding juist is:

    ```bash    
    sudo mount -a
    ```

    Als met deze opdracht een fout bericht wordt weer gegeven, controleert u de syntaxis van het `/etc/fstab`-bestand.
   
    Voer vervolgens de `mount` opdracht uit om te controleren of het bestands systeem is gekoppeld:

    ```bash    
    mount
    ......
    /dev/mapper/data--vg01-data--lv01 on /data type ext4 (rw)
    ```

5. Beschrijving Failsafe opstart parameters in `/etc/fstab`
   
    Veel distributies bevatten de `nobootwait`-of `nofail` koppel parameters die kunnen worden toegevoegd aan het `/etc/fstab` bestand. Met deze para meters kunnen fouten optreden bij het koppelen van een bepaald bestands systeem, waardoor het Linux-systeem kan blijven opstarten, zelfs als het RAID-bestands systeem niet goed kan worden gekoppeld. Raadpleeg de documentatie van uw distributie voor meer informatie over deze para meters.
   
    Voor beeld (Ubuntu):

    ```bash 
    /dev/data-vg01/data-lv01  /data  ext4  defaults,nobootwait  0  2
    ```

## <a name="trimunmap-support"></a>Ondersteuning voor knippen/ontkoppelen
Sommige Linux-kernels ondersteunen bewerkingen voor het verwijderen/ontkoppelen van ongebruikte blokken op de schijf. Deze bewerkingen zijn voornamelijk handig in de standaard opslag om Azure te informeren dat verwijderde pagina's niet meer geldig zijn en kunnen worden verwijderd. Bij het verwijderen van pagina's kunnen kosten worden bespaard als u grote bestanden maakt en deze vervolgens verwijdert.

Er zijn twee manieren om ondersteuning voor het verkleinen van de virtuele Linux-machine in te scha kelen. Zoals gebruikelijk, raadpleegt u de distributie voor de aanbevolen benadering:

- Gebruik de optie voor het koppelen van `discard` in `/etc/fstab`, bijvoorbeeld:

    ```bash 
    /dev/data-vg01/data-lv01  /data  ext4  defaults,discard  0  2
    ```

- In sommige gevallen kunnen de `discard`-optie prestatie implicaties hebben. U kunt ook de `fstrim` opdracht hand matig uitvoeren vanaf de opdracht regel of deze toevoegen aan uw crontab om regel matig uit te voeren:

    **Ubuntu**

    ```bash 
    # sudo apt-get install util-linux
    # sudo fstrim /datadrive
    ```

    **RHEL/CentOS**

    ```bash 
    # sudo yum install util-linux
    # sudo fstrim /datadrive
    ```
