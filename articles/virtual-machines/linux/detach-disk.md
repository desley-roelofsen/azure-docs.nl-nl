---
title: Een gegevens schijf loskoppelen van een virtuele Linux-machine-Azure
description: Leer hoe u een gegevens schijf loskoppelt van een virtuele machine in azure met behulp van Azure CLI of de Azure Portal.
author: roygara
ms.service: virtual-machines-linux
ms.topic: conceptual
ms.date: 07/18/2018
ms.author: rogarana
ms.subservice: disks
ms.openlocfilehash: f8a0790169b17ad7755386f9bdd4f9372efc83e7
ms.sourcegitcommit: 49cf9786d3134517727ff1e656c4d8531bbbd332
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/13/2019
ms.locfileid: "74036371"
---
# <a name="how-to-detach-a-data-disk-from-a-linux-virtual-machine"></a>Een gegevens schijf loskoppelen van een virtuele Linux-machine

Wanneer u een gegevensschijf die is gekoppeld aan een virtuele machine niet meer nodig hebt, kunt u deze eenvoudig loskoppelen. Hiermee verwijdert u de schijf van de virtuele machine, maar verwijdert u deze niet uit de opslag. In dit artikel werken we met een Ubuntu LTS 16,04-distributie. Als u een andere distributie gebruikt, kunnen de instructies voor het ontkoppelen van de schijf afwijken.

> [!WARNING]
> Als u een schijf loskoppelt, wordt deze niet automatisch verwijderd. Als u bent geabonneerd op Premium-opslag, blijven er opslag kosten in rekening worden gebracht voor de schijf. Zie [prijzen en facturering bij het gebruik van Premium Storage](https://azure.microsoft.com/pricing/details/storage/page-blobs/)voor meer informatie.

Als u de bestaande gegevens op de schijf opnieuw wilt gebruiken, kunt u de schijf opnieuw koppelen aan dezelfde of een andere virtuele machine.  


## <a name="connect-to-the-vm-to-unmount-the-disk"></a>Verbinding maken met de virtuele machine om de schijf te ontkoppelen

Voordat u de schijf kunt loskoppelen met behulp van CLI of de portal, moet u de schijf ontkoppelen en verwijzingen naar verwijderen uit het fstab-bestand.

Maak verbinding met de VM. In dit voor beeld is het open bare IP-adres van de virtuele machine *10.0.1.4* met de gebruikers naam *azureuser*: 

```bash
ssh azureuser@10.0.1.4
```

Zoek eerst de gegevens schijf die u wilt loskoppelen. In het volgende voor beeld wordt dmesg gebruikt om te filteren op SCSI-schijven:

```bash
dmesg | grep SCSI
```

De uitvoer lijkt op die in het volgende voorbeeld:

```bash
[    0.294784] SCSI subsystem initialized
[    0.573458] Block layer SCSI generic (bsg) driver version 0.4 loaded (major 252)
[    7.110271] sd 2:0:0:0: [sda] Attached SCSI disk
[    8.079653] sd 3:0:1:0: [sdb] Attached SCSI disk
[ 1828.162306] sd 5:0:0:0: [sdc] Attached SCSI disk
```

Hier is *Dit SDC* de schijf die u wilt loskoppelen. U moet ook de UUID van de schijf halen.

```bash
sudo -i blkid
```

De uitvoer ziet er ongeveer uit als in het volgende voor beeld:

```bash
/dev/sda1: UUID="11111111-1b1b-1c1c-1d1d-1e1e1e1e1e1e" TYPE="ext4"
/dev/sdb1: UUID="22222222-2b2b-2c2c-2d2d-2e2e2e2e2e2e" TYPE="ext4"
/dev/sdc1: UUID="33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e" TYPE="ext4"
```


Bewerk het *bestand/etc/fstab* -bestand om verwijzingen naar de schijf te verwijderen. 

> [!NOTE]
> Het onjuist bewerken van het **bestand/etc/fstab** -bestand kan resulteren in een systeem dat niet kan worden opgestart. Als dat niet het geval is, raadpleegt u de documentatie van de distributie voor informatie over het correct bewerken van dit bestand. U wordt ook aangeraden een back-up van het bestand/etc/fstab-bestand te maken voordat u het bewerkt.

Open het *bestand/etc/fstab* -bestand in een tekst editor als volgt:

```bash
sudo vi /etc/fstab
```

In dit voor beeld moet de volgende regel worden verwijderd uit het *bestand/etc/fstab* -bestand:

```bash
UUID=33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e   /datadrive   ext4   defaults,nofail   1   2
```

Gebruik `umount` om de schijf te ontkoppelen. In het volgende voor beeld wordt de */dev/sdc1* -partitie losgekoppeld van het */datadrive* -koppel punt:

```bash
sudo umount /dev/sdc1 /datadrive
```


## <a name="detach-a-data-disk-using-azure-cli"></a>Een gegevens schijf loskoppelen met Azure CLI 

In dit voor beeld wordt de *myDataDisk* -schijf losgekoppeld van de virtuele machine met de naam *myVM* in *myResourceGroup*.

```azurecli
az vm disk detach \
    -g myResourceGroup \
    --vm-name myVm \
    -n myDataDisk
```

De schijf blijft in de opslag, maar is niet meer gekoppeld aan een virtuele machine.


## <a name="detach-a-data-disk-using-the-portal"></a>Een gegevensschijf ontkoppelen via de portal

1. Selecteer in het linkermenu **virtual machines**.
2. Selecteer de virtuele machine met de gegevens schijf die u wilt loskoppelen en klik op **stoppen** om de toewijzing van de VM ongedaan te maken.
3. Selecteer **schijven**in het deel venster virtuele machine.
4. Selecteer boven in het deel venster **schijven** de optie **bewerken**.
5. Klik in het deel venster **schijven** helemaal rechts van de gegevens schijf die u wilt loskoppelen op de knop ![loskoppelen](./media/detach-disk/detach.png) knop loskoppelen.
5. Nadat de schijf is verwijderd, klikt u boven aan het deel venster op opslaan.
6. Klik in het deel venster virtuele machine op **overzicht** en klik vervolgens op de knop **Start** boven aan het deel venster om de VM opnieuw op te starten.

De schijf blijft in de opslag, maar is niet meer gekoppeld aan een virtuele machine.



## <a name="next-steps"></a>Volgende stappen
Als u de gegevens schijf opnieuw wilt gebruiken, kunt u [deze gewoon koppelen aan een andere virtuele machine](add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

