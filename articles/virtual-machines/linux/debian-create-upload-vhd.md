---
title: Een Debian Linux-VHD voorbereiden in azure
description: Meer informatie over het maken van Debian-VHD-installatie kopieën voor implementatie in Azure.
services: virtual-machines-linux
documentationcenter: ''
author: szarkos
manager: gwallace
editor: ''
tags: azure-resource-manager,azure-service-management
ms.assetid: a6de7a7c-cc70-44e7-aed0-2ae6884d401a
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.topic: article
ms.date: 11/13/2018
ms.author: szark
ms.openlocfilehash: 7f371e2681ecc2b3bb4bcb3e1a8b642061dc2449
ms.sourcegitcommit: 49cf9786d3134517727ff1e656c4d8531bbbd332
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/13/2019
ms.locfileid: "74036476"
---
# <a name="prepare-a-debian-vhd-for-azure"></a>Een Debian VHD voor Azure voorbereiden
## <a name="prerequisites"></a>Vereisten
In deze sectie wordt ervan uitgegaan dat u al een Debian Linux-besturings systeem hebt geïnstalleerd vanuit een ISO-bestand dat is gedownload van de [Debian-website](https://www.debian.org/distrib/) naar een virtuele harde schijf. Er zijn meerdere hulpprogram ma's voor het maken van VHD-bestanden. Hyper-V is maar een voor beeld. Zie [de hyper-v-functie installeren en een virtuele machine configureren](https://technet.microsoft.com/library/hh846766.aspx)voor instructies voor het gebruik van Hyper-v.

## <a name="installation-notes"></a>Installatie notities
* Zie ook [algemene Linux-installatie notities](create-upload-generic.md#general-linux-installation-notes) voor meer tips over het voorbereiden van Linux voor Azure.
* De nieuwere VHDX-indeling wordt niet ondersteund in Azure. U kunt de schijf converteren naar VHD-indeling met behulp van Hyper-V-beheer of de cmdlet **Convert-VHD** .
* Bij de installatie van het Linux-systeem wordt aanbevolen om standaard partities te gebruiken in plaats van LVM (vaak de standaard instelling voor veel installaties). Hiermee wordt voor komen dat LVM naam strijdig is met gekloonde Vm's, met name als een besturingssysteem schijf ooit moet worden gekoppeld aan een andere virtuele machine voor het oplossen van problemen. [LVM](configure-lvm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) of [RAID](configure-raid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) kan worden gebruikt op gegevens schijven als dit de voor keur heeft.
* Configureer geen swap partitie op de besturingssysteem schijf. De Azure Linux-agent kan worden geconfigureerd voor het maken van een wissel bestand op de tijdelijke bron schijf. Meer informatie vindt u in de volgende stappen.
* Alle Vhd's op Azure moeten een virtuele grootte hebben die is afgestemd op 1 MB. Wanneer u van een onbewerkte schijf naar VHD converteert, moet u ervoor zorgen dat de onbewerkte schijf grootte een meervoud van 1MB is vóór de conversie. Zie [installatie notities voor Linux](create-upload-generic.md#general-linux-installation-notes)voor meer informatie.

## <a name="use-azure-manage-to-create-debian-vhds"></a>Azure-Manage gebruiken om Debian-Vhd's te maken
Er zijn hulpprogram ma's beschikbaar voor het genereren van Debian-Vhd's voor Azure, zoals de [Azure-beheer](https://github.com/credativ/azure-manage) scripts van [Credativ](https://www.credativ.com/). Dit is de aanbevolen benadering en een volledig nieuwe installatie kopie maken. Als u bijvoorbeeld een Debian 8-VHD wilt maken, voert u de volgende opdrachten uit om het hulp programma `azure-manage` (en de afhankelijkheden) te downloaden en voert u het `azure_build_image` script uit:

    # sudo apt-get update
    # sudo apt-get install git qemu-utils mbr kpartx debootstrap

    # sudo apt-get install python3-pip python3-dateutil python3-cryptography
    # sudo pip3 install azure-storage azure-servicemanagement-legacy azure-common pytest pyyaml
    # git clone https://github.com/credativ/azure-manage.git
    # cd azure-manage
    # sudo pip3 install .

    # sudo azure_build_image --option release=jessie --option image_size_gb=30 --option image_prefix=debian-jessie-azure section


## <a name="manually-prepare-a-debian-vhd"></a>Een Debian-VHD hand matig voorbereiden
1. Selecteer de virtuele machine in Hyper-V-beheer.
2. Klik op **verbinding maken** om een console venster voor de virtuele machine te openen.
3. Als u het besturings systeem hebt geïnstalleerd met behulp van een ISO, kunt u een wille keurige regel met betrekking tot '`deb cdrom`' in `/etc/apt/source.list`.

4. Bewerk het `/etc/default/grub` bestand en wijzig de para meter **GRUB_CMDLINE_LINUX** als volgt om extra kernel-para meters voor Azure op te treffen.
   
        GRUB_CMDLINE_LINUX="console=tty0 console=ttyS0,115200n8 earlyprintk=ttyS0,115200"

5. Bouw de grub opnieuw op en voer de volgende handelingen uit:

        # sudo update-grub

6. Voeg Azure-opslag plaatsen van Debian toe aan/etc/apt/sources.List voor Debian 8 of 9:

    **Debian 8. x "Jessie"**

        deb http://debian-archive.trafficmanager.net/debian jessie main
        deb-src http://debian-archive.trafficmanager.net/debian jessie main
        deb http://debian-archive.trafficmanager.net/debian-security jessie/updates main
        deb-src http://debian-archive.trafficmanager.net/debian-security jessie/updates
        deb http://debian-archive.trafficmanager.net/debian jessie-updates main
        deb-src http://debian-archive.trafficmanager.net/debian jessie-updates main
        deb http://debian-archive.trafficmanager.net/debian jessie-backports main
        deb-src http://debian-archive.trafficmanager.net/debian jessie-backports main

    **Debian 9. x "stretch"**

        deb http://debian-archive.trafficmanager.net/debian stretch main
        deb-src http://debian-archive.trafficmanager.net/debian stretch main
        deb http://debian-archive.trafficmanager.net/debian-security stretch/updates main
        deb-src http://debian-archive.trafficmanager.net/debian-security stretch/updates main
        deb http://debian-archive.trafficmanager.net/debian stretch-updates main
        deb-src http://debian-archive.trafficmanager.net/debian stretch-updates main
        deb http://debian-archive.trafficmanager.net/debian stretch-backports main
        deb-src http://debian-archive.trafficmanager.net/debian stretch-backports main


7. De Azure Linux-agent installeren:
   
        # sudo apt-get update
        # sudo apt-get install waagent

8. Voor Debian 9 + is het raadzaam om de nieuwe Debian-Cloud kernel te gebruiken voor gebruik met virtuele machines in Azure. Als u deze nieuwe kernel wilt installeren, maakt u eerst een bestand met de naam/etc/apt/preferences.d/Linux.pref met de volgende inhoud:
   
        Package: linux-* initramfs-tools
        Pin: release n=stretch-backports
        Pin-Priority: 500
   
    Voer vervolgens ' sudo apt-down load linux-image-Cloud-amd64 ' uit om de nieuwe Debian Cloud kernel te installeren.

9. Maak de inrichting van de virtuele machine ongedaan en bereid deze voor op het inrichten van Azure en voer de volgende handelingen uit:
   
        # sudo waagent –force -deprovision
        # export HISTSIZE=0
        # logout

10. Klik op **actie** -> afgesloten in Hyper-V-beheer. Uw Linux-VHD is nu gereed om te worden geüpload naar Azure.

## <a name="next-steps"></a>Volgende stappen
U kunt nu de virtuele harde schijf van Debian gebruiken om nieuwe virtuele machines te maken in Azure. Zie [een virtuele Linux-machine maken op basis van een aangepaste schijf](upload-vhd.md#option-1-upload-a-vhd)als dit de eerste keer is dat u het VHD-bestand naar Azure uploadt.

