---
title: Een virtueel bestands systeem koppelen aan een pool-Azure Batch | Microsoft Docs
description: Meer informatie over het koppelen van een virtueel bestands systeem aan een batch-pool.
services: batch
documentationcenter: ''
author: laurenhughes
manager: gwallace
ms.service: batch
ms.workload: big-compute
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 08/13/2019
ms.author: lahugh
ms.openlocfilehash: 1c990c864f9daa98460832166b31f43fece1ed15
ms.sourcegitcommit: 44e85b95baf7dfb9e92fb38f03c2a1bc31765415
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/28/2019
ms.locfileid: "70093846"
---
# <a name="mount-a-virtual-file-system-on-a-batch-pool"></a>Een virtueel bestands systeem koppelen aan een batch-pool

Azure Batch ondersteunt nu het koppelen van Cloud opslag of een extern bestands systeem op Windows-of Linux-reken knooppunten in uw batch-Pools. Wanneer een reken knooppunt wordt toegevoegd aan een pool, wordt het virtuele bestands systeem gekoppeld en behandeld als een lokaal station op het knoop punt. U kunt bestands systemen zoals Azure Files, Azure Blob Storage, Network File System (NFS), waaronder een [avere vFXT-cache](../avere-vfxt/avere-vfxt-overview.md)of een common Internet File System (CIFS) koppelen.

In dit artikel leert u hoe u een virtueel bestands systeem koppelt aan een pool van reken knooppunten met behulp [van de batch-beheer bibliotheek voor .net](https://docs.microsoft.com/dotnet/api/overview/azure/batch?view=azure-dotnet).

> [!NOTE]
> Het koppelen van een virtueel bestands systeem wordt ondersteund op batch-Pools die zijn gemaakt op of na 2019-08-19. Batch-Pools die zijn gemaakt vóór 2019-08-19 bieden geen ondersteuning voor deze functie.
> 
> De Api's voor het koppelen van bestands systemen op een reken knooppunt maken deel uit van de [batch .net](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch?view=azure-dotnet) -bibliotheek.

## <a name="benefits-of-mounting-on-a-pool"></a>Voor delen van koppelen aan een groep

Het bestands systeem koppelen aan de groep, in plaats van taken om hun eigen gegevens uit een grote gegevensset op te halen, is het eenvoudiger en efficiënter voor taken om toegang te krijgen tot de gegevens die nodig zijn.

Overweeg een scenario met meerdere taken waarvoor toegang tot een gemeen schappelijke set gegevens is vereist, zoals het renderen van een film. Met elke taak worden een of meer frames tegelijk uit de scène bestanden weer gegeven. Door een station met de scène bestanden te koppelen, is het eenvoudiger voor reken knooppunten om toegang te krijgen tot gedeelde gegevens. Daarnaast kan het onderliggende bestands systeem afzonderlijk worden gekozen en geschaald op basis van de prestaties en de schaal (door Voer en IOPS) die vereist zijn voor het aantal reken knooppunten dat gelijktijdig toegang heeft tot de gegevens. Een gedistribueerde in-memory cache van [avere](../avere-vfxt/avere-vfxt-overview.md) kan bijvoorbeeld worden gebruikt voor het ondersteunen van grootschalige Renders op schaal van een afbeelding met duizenden gelijktijdige weergave knooppunten, waarbij toegang wordt verkregen tot bron gegevens die on-premises zijn opgeslagen. Voor gegevens die zich al in Cloud opslag op basis van een BLOB bevinden, kan [blobfuse](../storage/blobs/storage-how-to-mount-container-linux.md) ook worden gebruikt om deze gegevens als een lokaal bestands systeem te koppelen. Blobfuse is alleen beschikbaar op Linux-knoop punten. [Azure files](https://azure.microsoft.com/blog/a-new-era-for-azure-files-bigger-faster-better/) biedt echter een vergelijk bare werk stroom en is beschikbaar op Windows en Linux.

## <a name="mount-a-virtual-file-system-on-a-pool"></a>Een virtueel bestands systeem koppelen aan een groep  

Als u een virtueel bestands systeem op een groep koppelt, wordt het bestands systeem beschikbaar voor elk reken knooppunt in de pool. Het bestands systeem wordt geconfigureerd wanneer een reken knooppunt wordt toegevoegd aan een groep of wanneer het knoop punt opnieuw wordt opgestart of de installatie kopie wordt gerecycled.

Als u een bestands systeem in een groep wilt koppelen, `MountConfiguration` maakt u een-object. Kies het object dat bij uw virtuele bestands systeem past `AzureBlobFileSystemConfiguration`: `AzureFileShareConfiguration` `NfsMountConfiguration`,,, `CifsMountConfiguration`of.

Voor alle koppel configuratie objecten zijn de volgende basis parameters vereist. Sommige koppel configuraties hebben specifieke para meters voor het bestands systeem dat wordt gebruikt. dit wordt in meer detail beschreven in de code voorbeelden.

- **Account naam of-bron**: Als u een virtuele bestands share wilt koppelen, hebt u de naam van het opslag account of de bron nodig.
- **Relatief pad of bron van koppeling**: De locatie van het bestands systeem dat is gekoppeld aan het reken knooppunt, ten `fsmounts` opzichte van de standaard `AZ_BATCH_NODE_MOUNTS_DIR`Directory die toegankelijk is via het knoop punt. De exacte locatie varieert, afhankelijk van het besturings systeem dat op het knoop punt wordt gebruikt. Bijvoorbeeld, de fysieke locatie op een Ubuntu-knoop punt wordt toegewezen `mnt\batch\tasks\fsmounts`aan en op een CentOS-knoop punt waaraan deze `mnt\resources\batch\tasks\fsmounts`is toegewezen.
- Opties voor **koppelen of blobfuse**: Deze opties beschrijven specifieke para meters voor het koppelen van een bestands systeem.

Wanneer het `MountConfiguration` object is gemaakt, moet u het object toewijzen `MountConfigurationList` aan de eigenschap wanneer u de groep maakt. Het bestands systeem wordt gekoppeld wanneer een knoop punt wordt toegevoegd aan een pool of wanneer het knoop punt opnieuw wordt opgestart of een installatie kopie wordt gemaakt.

Wanneer het bestands systeem is gekoppeld, wordt er een `AZ_BATCH_NODE_MOUNTS_DIR` omgevings variabele gemaakt die verwijst naar de locatie van de gekoppelde bestands systemen en de logboek bestanden, die handig zijn voor het oplossen van problemen en fout opsporing. Logboek bestanden worden uitgebreid beschreven in het gedeelte fout bij het vaststellen van de [koppeling](#diagnose-mount-errors) .  

> [!IMPORTANT]
> Het maximum aantal gekoppelde bestands systemen in een groep is 10. Zie [quota's en limieten](batch-quota-limit.md#other-limits) voor de batch-service voor meer informatie en andere beperkingen.

## <a name="examples"></a>Voorbeelden

De volgende code voorbeelden laten zien hoe u een verscheidenheid aan bestands shares koppelt aan een pool van reken knooppunten.

### <a name="azure-files-share"></a>Azure Files share

Azure Files is de standaard aanbieding van het Azure Cloud-bestands systeem. Zie [een Azure Files share gebruiken](../storage/files/storage-how-to-use-files-windows.md)voor meer informatie over het verkrijgen van een van de para meters in het koppel configuratie code voorbeeld.

```csharp
new PoolAddParameter
{
    Id = poolId,
    MountConfiguration = new[]
    {
        new MountConfiguration
        {
            AzureFileShareConfiguration = new AzureFileShareConfiguration
            {
                AccountName = "AccountName",
                AzureFileUrl = "AzureFileShareUrl",
                AccountKey = "StorageAccountKey",
                RelativeMountPath = "RelativeMountPath",
                MountOptions = "-o vers=3.0,dir_mode=0777,file_mode=0777,sec=ntlmssp"
            },
        }
    }
}
```

### <a name="azure-blob-file-system"></a>Azure Blob-bestands systeem

Een andere optie is het gebruik van Azure Blob Storage via [blobfuse](../storage/blobs/storage-how-to-mount-container-linux.md). Voor het koppelen van een BLOB- `AccountKey` bestands `SasKey` systeem is een or vereist voor uw opslag account. Zie [account sleutels weer geven](../storage/common/storage-account-manage.md#view-account-keys-and-connection-string)of [Shared Access signatures (SAS) gebruiken](../storage/common/storage-dotnet-shared-access-signature-part-1.md)voor meer informatie over het ophalen van deze sleutels. Zie de blobfuse [Troubleshooting FAQ (Engelstalig](https://github.com/Azure/azure-storage-fuse/wiki/3.-Troubleshoot-FAQ)) voor meer informatie over het gebruik van blobfuse. Als u standaard toegang wilt krijgen tot de gekoppelde blobfuse Directory, voert u de taak uit als **beheerder**. Blobfuse koppelt de directory aan de gebruikers ruimte en bij het maken van de groep wordt deze als root gekoppeld. In Linux zijn alle **beheerders** taken hoofdmap. Alle opties voor de ZEKERing-module worden beschreven op de [pagina zekerheid](http://manpages.ubuntu.com/manpages/xenial/man8/mount.fuse.8.html).

Naast de hand leiding voor het oplossen van problemen, zijn GitHub problemen in de blobfuse-opslag plaats een handige manier om te controleren of er problemen zijn met blobfuse en oplossingen. Zie [blobfuse issues](https://github.com/Azure/azure-storage-fuse/issues)(Engelstalig) voor meer informatie.

> [!NOTE]
> Blobfuse wordt momenteel niet ondersteund op Debian. Zie [ondersteunde sku's](#supported-skus) voor meer informatie.

```csharp
new PoolAddParameter
{
    Id = poolId,
    MountConfiguration = new[]
    {
        new MountConfiguration
        {
            AzureBlobFileSystemConfiguration = new AzureBlobFileSystemConfiguration
            {
                AccountName = "StorageAccountName",
                ContainerName = "containerName",
                AccountKey = "StorageAccountKey",
                SasKey = "",
                RelativeMountPath = "RelativeMountPath",
                BlobfuseOptions = "-o attr_timeout=240 -o entry_timeout=240 -o negative_timeout=120 "
            },
        }
    }
}
```

### <a name="network-file-system"></a>Netwerk bestandssysteem

Network File System (NFS) kan ook worden gekoppeld aan groeps knooppunten, zodat traditionele bestands systemen eenvoudig kunnen worden benaderd door Azure Batch knooppunten. Dit kan een enkele NFS-server zijn die is geïmplementeerd in de Cloud, of een on-premises NFS-server die toegankelijk is via een virtueel netwerk. U kunt ook profiteren van de [avere vFXT](../avere-vfxt/avere-vfxt-overview.md) gedistribueerde in-memory-cache oplossing, waarmee naadloze connectiviteit mogelijk is met on-premises opslag, het lezen van gegevens op aanvraag in de cache en het bieden van hoge prestaties en schaal baarheid tot Cloud compute punt.

```csharp
new PoolAddParameter
{
    Id = poolId,
    MountConfiguration = new[]
    {
        new MountConfiguration
        {
            NfsMountConfiguration = new NFSMountConfiguration
            {
                Source = "source",
                RelativeMountPath = "RelativeMountPath",
                MountOptions = "options ver=1.0"
            },
        }
    }
}
```

### <a name="common-internet-file-system"></a>Common Internet File System

Common Internet File Systems (CIFS) kan ook worden gekoppeld aan groeps knooppunten, zodat traditionele bestands systemen eenvoudig kunnen worden geopend door Azure Batch knooppunten. CIFS is een protocol voor het delen van bestanden dat een open en platformoverschrijdende mechanisme biedt voor het aanvragen van netwerk server bestanden en-services. CIFS is gebaseerd op de verbeterde versie van het SMB-protocol (Server Message Block) van micro soft voor het delen van Internet-en intranet bestanden en wordt gebruikt om externe bestands systemen te koppelen aan Windows-knoop punten. Zie [File Server and SMB](https://docs.microsoft.com/windows-server/storage/file-server/file-server-smb-overview)(Engelstalig) voor meer informatie over SMB.

```csharp
new PoolAddParameter
{
    Id = poolId,
    MountConfiguration = new[]
    {
        new MountConfiguration
        {
            CifsMountConfiguration = new CIFSMountConfiguration
            {
                Username = "StorageAccountName",
                RelativeMountPath = "cifsmountpoint",
                Source = "source",
                Password = "StorageAccountKey",
                MountOptions = "-o vers=3.0,dir_mode=0777,file_mode=0777,serverino"
            },
        }
    }
}
```

## <a name="diagnose-mount-errors"></a>Fouten bij koppelen vaststellen

Als een koppelings configuratie mislukt, mislukt het reken knooppunt in de groep en wordt de status van het knoop punt onbruikbaar. Als u een fout bij het koppelen van een configuratie [`ComputeNodeError`](https://docs.microsoft.com/rest/api/batchservice/computenode/get#computenodeerror) wilt vaststellen, inspecteert u de eigenschap voor meer informatie over de fout.

Als u de logboek bestanden voor fout opsporing wilt [](batch-task-output-files.md) ophalen, gebruikt u `*.log` OutputFiles om de bestanden te uploaden. De `*.log` bestanden bevatten informatie over de bestandssysteem koppeling op de `AZ_BATCH_NODE_MOUNTS_DIR` locatie. Het koppelen van logboek bestanden heeft de `<type>-<mountDirOrDrive>.log` volgende indeling: voor elke koppeling. Een `cifs` koppeling op een koppelings Directory met de naam `test` heeft bijvoorbeeld een koppel logboek bestand met de `cifs-test.log`naam:.

## <a name="supported-skus"></a>Ondersteunde Sku's

| Uitgever | Aanbieding | SKU | Azure Files share | Blobfuse | NFS-koppeling | CIFS koppelen |
|---|---|---|---|---|---|---|
| batch | rendering-centos73 | aanwijzer | :heavy_check_mark: <br>Opmerking: Compatibel met CentOS 7,7</br>| :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| Canonical | UbuntuServer | 16,04-LTS, 18,04-LTS | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| credativ | Debian | 8, 9 | :heavy_check_mark: | BxDxH | :heavy_check_mark: | :heavy_check_mark: |
| microsoft-ads | linux-data-science-vm | linuxdsvm | :heavy_check_mark: <br>Opmerking: Compatibel met CentOS 7,4. </br> | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| microsoft-azure-batch | CentOS-container | 7,6 | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| microsoft-azure-batch | CentOS-container-RDMA | 7.4 | :heavy_check_mark: <br>Opmerking: Ondersteunt A_8-of 9-opslag</br> | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| microsoft-azure-batch | Ubuntu-Server-container | 16.04-LTS | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| microsoft-dsvm | linux-data-science-vm-ubuntu | linuxdsvmubuntu | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| OpenLogic | CentOS | 7,6 | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| OpenLogic | CentOS-HPC | 7,4, 7,3, 7,1 | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| Oracle | Oracle-Linux | 7,6 | BxDxH | BxDxH | BxDxH | BxDxH |
| Windows | WindowsServer | 2012, 2016, 2019 | :heavy_check_mark: | BxDxH | BxDxH | BxDxH |

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over het koppelen van een Azure Files share met [Windows](../storage/files/storage-how-to-use-files-windows.md) of [Linux](../storage/files/storage-how-to-use-files-linux.md).
- Meer informatie over het gebruik van en koppelen van [blobfuse](https://github.com/Azure/azure-storage-fuse) Virtual File Systems.
- Zie [overzicht Network File System](https://docs.microsoft.com/windows-server/storage/nfs/nfs-overview) voor meer informatie over NFS en de bijbehorende toepassingen.
- Zie het [overzicht van micro soft SMB-protocol en CIFS-protocol](https://docs.microsoft.com/windows/desktop/fileio/microsoft-smb-protocol-and-cifs-protocol-overview) voor meer informatie over CIFS.
