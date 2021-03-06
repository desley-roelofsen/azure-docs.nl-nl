---
title: Een Azure Files-implementatie plannen | Microsoft Docs
description: Meer informatie over hoe u rekening moet houden bij het plannen van een Azure Files-implementatie.
author: roygara
ms.service: storage
ms.topic: conceptual
ms.date: 10/16/2019
ms.author: rogarana
ms.subservice: files
ms.openlocfilehash: fdfa01a45c0dd35da65b2ad7ce8b0d291148af1a
ms.sourcegitcommit: a5ebf5026d9967c4c4f92432698cb1f8651c03bb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/08/2019
ms.locfileid: "74931107"
---
# <a name="planning-for-an-azure-files-deployment"></a>Planning voor de implementatie van Azure Files

[Azure Files](storage-files-introduction.md) biedt volledig beheerde bestandsshares in de cloud die toegankelijk zijn via het industriestandaard SMB-protocol. Omdat Azure Files volledig wordt beheerd, is het implementeren van de implementatie in productie scenario's veel eenvoudiger dan het implementeren en beheren van een bestands server of een NAS-apparaat. In dit artikel worden de onderwerpen behandeld waarmee u rekening moet houden bij het implementeren van een Azure-bestands share voor productie gebruik binnen uw organisatie.

## <a name="management-concepts"></a>Beheer concepten

 In het volgende diagram ziet u de Azure Files-beheer constructies:

![Bestandsstructuur](./media/storage-files-introduction/files-concepts.png)

* **Opslagaccount**: alle toegang tot Azure Storage vindt plaats via een opslagaccount. Zie [Azure Storage Scalability and Performance Targets](../common/storage-scalability-targets.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json) (Schaalbaarheids- en prestatiedoelen in Azure Storage) voor meer informatie over opslagaccountcapaciteit.

* **Share**: een File Storage-share is een SMB-bestandsshare in Azure. Alle mappen en bestanden moeten worden gemaakt in een bovenliggende share. Een account kan een onbeperkt aantal shares bevatten en een share kan een onbeperkt aantal bestanden opslaan, tot aan de totale capaciteit van de bestands share. De totale capaciteit voor Premium-en standaard bestands shares is 100 TiB.

* **Map**: een optionele hiërarchie van mappen.

* **Bestand**: een bestand in de share. Een bestand kan Maxi maal 1 TiB groot zijn.

* **URL-indeling**: voor aanvragen voor een Azure-bestands share die is gemaakt met het File rest-protocol, kunnen bestanden worden adresseerd met behulp van de volgende URL-indeling:

    ```
    https://<storage account>.file.core.windows.net/<share>/<directory>/<file>
    ```

## <a name="data-access-method"></a>Methode voor gegevens toegang

Azure Files biedt twee ingebouwde, handige methoden voor gegevens toegang die u afzonderlijk kunt gebruiken of in combi natie met elkaar om toegang te krijgen tot uw gegevens:

1. **Directe Cloud toegang**: elke Azure-bestands share kan worden gekoppeld door [Windows](storage-how-to-use-files-windows.md), [macOS](storage-how-to-use-files-mac.md)en/of [Linux](storage-how-to-use-files-linux.md) met het standaard SMB-protocol (Server Message Block) of via de bestands rest API. Met SMB worden lees-en schrijf bewerkingen naar bestanden op de share rechtstreeks gemaakt op de bestands share in Azure. Als u wilt koppelen aan een virtuele machine in azure, moet de SMB-client in het besturings systeem ten minste SMB 2,1 ondersteunen. Voor het koppelen van on-premises, zoals op het werk station van een gebruiker, moet de door het werk station ondersteunde SMB-client ten minste SMB 3,0 ondersteunen (met versleuteling). Naast SMB kunnen nieuwe toepassingen of services rechtstreeks toegang krijgen tot de bestands share via de REST van het bestand. Dit biedt een eenvoudige en schaal bare Application Programming Interface voor software ontwikkeling.
2. **Azure file sync**: met Azure file sync kunnen shares worden gerepliceerd naar Windows-servers, on-premises of in Azure. Uw gebruikers hebben toegang tot de bestands share via de Windows-Server, zoals via een SMB-of NFS-share. Dit is handig voor scenario's waarin gegevens van een Azure-Data Center worden geopend en gewijzigd, zoals in een filiaal scenario. Gegevens kunnen worden gerepliceerd tussen meerdere Windows Server-eind punten, zoals tussen meerdere filialen. Ten slotte kunnen gegevens worden getierd op Azure Files, zodat alle gegevens nog steeds toegankelijk zijn via de server, maar de server heeft geen volledige kopie van de gegevens. Gegevens worden in plaats daarvan naadloos ingetrokken wanneer ze door uw gebruiker worden geopend.

In de volgende tabel ziet u hoe uw gebruikers en toepassingen toegang hebben tot uw Azure-bestands share:

| | Directe Cloud toegang | Azure File Sync |
|------------------------|------------|-----------------|
| Welke protocollen moet u gebruiken? | Azure Files ondersteunt SMB 2,1, SMB 3,0 en file REST API. | Toegang tot uw Azure-bestands share via elk ondersteund protocol op Windows Server (SMB, NFS, FTPS, enzovoort) |  
| Waar wordt uw werk belasting uitgevoerd? | **In azure**: Azure files biedt directe toegang tot uw gegevens. | **On-premises met langzaam netwerk**: Windows-, Linux-en macOS-clients kunnen een lokale on-premises Windows-bestands share koppelen als een snelle cache van uw Azure-bestands share. |
| Welk toegangs niveau hebt u nodig? | Share-en bestands niveau. | Share-, bestands-en gebruikers niveau. |

## <a name="data-security"></a>Databeveiliging

Azure Files heeft verschillende ingebouwde opties voor het garanderen van gegevens beveiliging:

* Ondersteuning voor versleuteling in zowel over-the-Wire-protocollen: SMB 3,0-versleuteling en-bestand REST via HTTPS. Standaard: 
    * Clients die ondersteuning bieden voor SMB 3,0-versleuteling, kunnen gegevens verzenden en ontvangen via een versleuteld kanaal.
    * Clients die geen ondersteuning bieden voor SMB 3,0 met versleuteling, kunnen intra Data Center via SMB 2,1 of SMB 3,0 zonder versleuteling communiceren. SMB-clients mogen geen communicatie tussen-Data Center via SMB 2,1 of SMB 3,0 zonder versleuteling.
    * Clients kunnen via de REST van het bestand communiceren met HTTP of HTTPS.
* Versleuteling op rest ([Azure Storage-service versleuteling](../common/storage-service-encryption.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json)): Storage service Encryption (SSE) is ingeschakeld voor alle opslag accounts. Data-at-rest is versleuteld met volledig beheerde sleutels. Versleuteling op rest verhoogt de opslag kosten niet of vermindert de prestaties. 
* Optionele vereiste voor versleutelde gegevens in transit: wanneer deze optie is geselecteerd, wordt de toegang tot de gegevens via niet-versleutelde kanalen door Azure Files geweigerd. In het bijzonder worden alleen HTTPS-en SMB-3,0 met versleutelings verbindingen toegestaan.

    > [!Important]  
    > Het vereisen van een veilige overdracht van gegevens leidt ertoe dat oudere SMB-clients niet kunnen communiceren met SMB 3,0 en dat de versleuteling mislukt. Zie [mounten in Windows](storage-how-to-use-files-windows.md), [koppelen aan linux](storage-how-to-use-files-linux.md)en [koppelen aan macOS](storage-how-to-use-files-mac.md)voor meer informatie.

Voor een maximale beveiliging wordt het ten zeerste aangeraden om beide versleuteling in-rest in te scha kelen en versleuteling van gegevens in transit in te scha kelen wanneer u moderne clients gebruikt voor toegang tot uw gegevens. Als u bijvoorbeeld een share moet koppelen aan een virtuele machine met Windows Server 2008 R2, die alleen SMB 2,1 ondersteunt, moet u niet-versleuteld verkeer naar uw opslag account toestaan, omdat SMB 2,1 geen ondersteuning biedt voor versleuteling.

Als u Azure File Sync gebruikt om toegang te krijgen tot uw Azure-bestands share, zullen we altijd HTTPS en SMB 3,0 gebruiken met versleuteling om uw gegevens te synchroniseren met uw Windows-servers, ongeacht of u versleuteling van gegevens nodig hebt.

## <a name="file-share-performance-tiers"></a>Prestatie lagen voor bestands shares

Azure Files biedt twee prestatie lagen: Standard en Premium.

### <a name="standard-file-shares"></a>Standaard bestands shares

Standaard bestands shares worden ondersteund door harde schijven (Hdd's). Standaard bestands shares bieden betrouw bare prestaties voor i/o-workloads die minder gevoelig zijn voor prestatie verschillen, zoals bestands shares voor algemene doel einden en ontwikkel-en test omgevingen. Standaard bestands shares zijn alleen beschikbaar in een facturerings model met betalen per gebruik.

> [!IMPORTANT]
> Als u bestands shares wilt gebruiken die groter zijn dan 5 TiB, raadpleegt u de sectie [onboarding to largeed file shares (Standard-laag)](#onboard-to-larger-file-shares-standard-tier) voor de stappen voor onboarding, evenals regionale Beschik baarheid en beperkingen.

### <a name="premium-file-shares"></a>Premium-bestands shares

Premium-bestands shares worden ondersteund door Ssd's (Solid-state drives). Premium-bestands shares bieden consistente hoge prestaties en lage latentie in milliseconden voor de meeste i/o-bewerkingen, voor i/o-intensieve workloads. Dit maakt ze geschikt voor een groot aantal werk belastingen, zoals data bases, hosting van websites en ontwikkel omgevingen. Premium-bestands shares zijn alleen beschikbaar in een ingericht facturerings model. Premium-bestands shares gebruiken een implementatie model gescheiden van standaard bestands shares.

Azure Backup is beschikbaar voor Premium-bestands shares en Azure Kubernetes-service biedt ondersteuning voor Premium-bestands shares in versie 1,13 en hoger.

Als u wilt weten hoe u een Premium-bestands share maakt, raadpleegt u ons artikel over het onderwerp: [een Azure Premium File Storage-account maken](storage-how-to-create-premium-fileshare.md).

Op dit moment kunt u niet rechtstreeks converteren tussen een standaard bestands share en een Premium-bestands share. Als u wilt overschakelen naar een van beide lagen, moet u een nieuwe bestands share in die laag maken en de gegevens van de oorspronkelijke share hand matig kopiëren naar de nieuwe share die u hebt gemaakt. U kunt dit doen met behulp van een van de Azure Files ondersteunde Kopieer hulpprogramma's, zoals Robocopy of AzCopy.

> [!IMPORTANT]
> Premium-bestands shares zijn beschikbaar voor LRS in de meeste regio's die opslag accounts en ZRS in een kleinere subset van regio's aanbieden. Zie de pagina [producten beschikbaar per regio](https://azure.microsoft.com/global-infrastructure/services/?products=storage) voor Azure als u wilt weten of Premium-bestands shares op dit moment beschikbaar zijn in uw regio. Zie [ondersteuning voor dekking en regionale Beschik baarheid](../common/storage-redundancy-zrs.md#support-coverage-and-regional-availability)als u wilt weten welke regio's ZRS ondersteunen.
>
> Vul deze [enquête](https://aka.ms/pfsfeedback)in om u te helpen bij het bepalen van de prioriteit van nieuwe regio's en de functies van de Premium-laag.

#### <a name="provisioned-shares"></a>Ingerichte shares

Premium-bestands shares worden ingericht op basis van een vaste GiB/IOPS/doorvoer ratio. Voor elke GiB die is ingericht, wordt de share één IOPS en 0,1 MiB/s-door Voer verleend tot het maximum aantal limieten per share. De mini maal toegestane inrichting is 100 GiB met een minimale IOPS/door voer.

Op basis van de beste inspanningen kunnen alle shares Maxi maal drie IOPS per GiB aan ingerichte opslag gedurende 60 minuten of langer, afhankelijk van de grootte van de share. Nieuwe shares beginnen met het volledige burst-tegoed op basis van de ingerichte capaciteit.

Shares moeten worden ingericht in 1 GiB-stappen. De minimale grootte is 100 GiB, de volgende grootte is 101 GiB, enzovoort.

> [!TIP]
> Basis lijn IOPS = 1 * ingerichte GiB. (Maxi maal 100.000 IOPS).
>
> Burst-limiet = 3 * IOPS basis lijn. (Maxi maal 100.000 IOPS).
>
> uitgangs bedrag = 60 MiB/s + 0,06 * ingerichte GiB
>
> Ingangs frequentie = 40 MiB/s + 0,04 * ingerichte GiB

De inrichtings share grootte wordt opgegeven door het share quotum. Share quotum kan op elk moment worden verhoogd, maar kan pas na 24 uur na de laatste toename worden verlaagd. Nadat u 24 uur hebt gewacht zonder een quota verhoging, kunt u het share quotum zo vaak als u wilt verlagen totdat u het opnieuw verhoogt. Wijzigingen in IOPS/doorvoer schaal worden binnen een paar minuten na de grootte gewijzigd.

Het is mogelijk om de grootte van uw ingerichte share onder uw gebruikte GiB te verkleinen. Als u dit doet, verliest u geen gegevens, maar worden er nog steeds kosten in rekening gebracht voor de gebruikte grootte en worden de prestaties (de basis-IOPS, door Voer en burst IOPS) van de ingerichte share ontvangen, niet de gebruikte grootte.

In de volgende tabel ziet u enkele voor beelden van deze formules voor de ingerichte grootte van de shares:

|Capaciteit (GiB) | IOPS-basislijn | Burst IOPS | Uitgang (MiB/s) | Ingangs (MiB/s) |
|---------|---------|---------|---------|---------|
|100         | 100     | Maxi maal 300     | 66   | 44   |
|500         | 500     | Maxi maal 1.500   | 90   | 60   |
|1,024       | 1,024   | Maxi maal 3.072   | 122   | 81   |
|5\.120       | 5\.120   | Maxi maal 15.360  | 368   | 245   |
|10.240      | 10.240  | Maxi maal 30.720  | 675 | 450   |
|33.792      | 33.792  | Maxi maal 100.000 | 2\.088 | 1\.392   |
|51.200      | 51.200  | Maxi maal 100.000 | 3\.132 | 2\.088   |
|102.400     | 100.000 | Maxi maal 100.000 | 6\.204 | 4\.136   |

> [!NOTE]
> De prestaties van bestands shares zijn afhankelijk van de netwerk limieten van de computer, de beschik bare netwerk bandbreedte, i/o-grootte, de parallelle uitvoering, onder andere factoren. Een voor beeld: op basis van een interne test met 8 KiB i/o-schrijf grootten, kan één virtuele Windows-machine, *standaard F16s_v2*, die is verbonden met Premium file share over SMB, 20.000 Read IOPS en Maxi maal 15.000 schrijf-IOPS behaalt. Met 512 MiB lees/schrijf-i/o-groottes kan dezelfde virtuele machine 1,1 GiB/s uitgaand worden gemaakt en 370 MiB/s door voer. U kunt de maximale prestaties schalen door de belasting over meerdere virtuele machines uit te breiden. Raadpleeg de [gids voor probleem oplossing](storage-troubleshooting-files-performance.md) voor enkele veelvoorkomende prestatie problemen en tijdelijke oplossingen.

#### <a name="bursting"></a>Toepassingen

Premium-bestands shares kunnen de IOPS opbursten tot een factor van drie. Bursting wordt geautomatiseerd en werkt op basis van een tegoed systeem. Bursting werkt op basis van de beste inspanningen en de burst-limiet is geen garantie. bestands shares *kunnen de limiet* overschrijden.

De tegoeden worden in een burst-Bucket verzameld wanneer het verkeer voor uw bestands share onder IOPS voor de basis lijn valt. Een GiB-share van 100 heeft bijvoorbeeld 100 Baseline IOPS. Als het werkelijke verkeer op de share 40 IOPS is voor een specifiek interval van 1 seconde, worden 60 de ongebruikte IOPS gecrediteerd op een burst-Bucket. Deze tegoeden worden vervolgens later gebruikt wanneer bewerkingen de basis lijn van IOPs overschrijden.

> [!TIP]
> Grootte van de burst Bucket = basislijn IOPS * 2 * 3600.

Wanneer een share de limiet van de basis lijn overschrijdt en er tegoed in een burst-Bucket staat, wordt deze gesplitst. Shares kunnen blijven worden opgedeeld zolang de tegoeden resteren, terwijl de shares die kleiner zijn dan 50 TiB alleen tot een uur in de burst-limiet blijven. Shares groter dan 50 TiB kan technisch deze limiet van één uur overschrijden, tot twee uur, maar dit is gebaseerd op het aantal burst-tegoeden dat is samengevoegd. Elke i/o-limiet van meer dan Baseline IOPS maakt gebruik van één tegoed en wanneer alle tegoeden worden verbruikt, wordt de share teruggestuurd naar de limiet van de basis lijn

Aandelen tegoeden hebben drie statussen:

- Samen, wanneer de bestands share minder dan de basis lijn voor IOPS gebruikt.
- Weigeren, wanneer de bestands share bursting is.
- De resterende constante wanneer er geen tegoeden of basis-IOPS in gebruik zijn.

Nieuwe bestands shares beginnen met het volledige aantal tegoeden in de burst-Bucket. Burst-tegoed wordt niet in rekening gebracht als de delen IOPS onder de basis lijn IOPS vallen vanwege het beperken van de server.

## <a name="file-share-redundancy"></a>Redundantie van bestands share

Azure Files standaard shares ondersteunt vier opties voor gegevens redundantie: lokaal redundante opslag (LRS), zone redundant Storage (ZRS), geografisch redundante opslag (GRS) en geo-zone-redundante opslag (GZRS) (preview).

Azure Files Premium-shares ondersteunen zowel LRS als ZRS. ZRS is momenteel beschikbaar in een kleinere subset van regio's.

In de volgende secties worden de verschillen tussen de verschillende redundantie opties beschreven:

### <a name="locally-redundant-storage"></a>Lokaal redundante opslag

[!INCLUDE [storage-common-redundancy-LRS](../../../includes/storage-common-redundancy-LRS.md)]

### <a name="zone-redundant-storage"></a>Zone redundante opslag

[!INCLUDE [storage-common-redundancy-ZRS](../../../includes/storage-common-redundancy-ZRS.md)]

### <a name="geo-redundant-storage"></a>Geografisch redundante opslag

> [!Warning]  
> Als u uw Azure-bestands share als een Cloud-eind punt in een GRS-opslag account gebruikt, mag u de failover van het opslag account niet starten. Als u dat wel doet, werkt de synchronisatie niet meer en kan dit leiden tot onverwachte gegevens verlies in het geval van nieuwe gelaagde bestanden. In het geval van een verlies van een Azure-regio, zal micro soft de failover van het opslag account activeren op een manier die compatibel is met Azure File Sync.

Geografisch redundante opslag (GRS) is ontworpen om ten minste 99.99999999999999% (16 9) duurzaamheid van objecten over een bepaald jaar te bieden door uw gegevens te repliceren naar een secundaire regio die honderden kilo meters van de primaire regio is. Als voor uw opslag account GRS is ingeschakeld, zijn uw gegevens duurzaam, zelfs in het geval van een volledige regionale onderbreking of een ramp waarbij de primaire regio niet kan worden hersteld.

Als u kiest voor geografisch redundante opslag met lees toegang (RA-GRS), moet u weten dat Azure-bestand op dit moment geen ondersteuning biedt voor geografisch redundante opslag met lees toegang (RA-GRS). Bestands shares in het RA-GRS-opslag account werken zoals in GRS-accounts en worden GRS-prijzen in rekening gebracht.

GRS repliceert uw gegevens naar een ander Data Center in een secundaire regio, maar deze gegevens zijn alleen-lezen als micro soft een failover initieert van de primaire naar de secundaire regio.

Voor een opslag account waarvoor GRS is ingeschakeld, worden alle gegevens eerst gerepliceerd met lokaal redundante opslag (LRS). Een update wordt eerst doorgevoerd op de primaire locatie en gerepliceerd met behulp van LRS. De update wordt vervolgens asynchroon gerepliceerd naar de secundaire regio met behulp van GRS. Wanneer gegevens naar de secundaire locatie worden geschreven, worden deze ook gerepliceerd binnen die locatie met behulp van LRS.

De primaire en secundaire regio's beheren replica's in afzonderlijke fout domeinen en upgraden domeinen binnen een opslag schaal eenheid. De eenheid voor opslag schaal is de basis replicatie-eenheid binnen het Data Center. Replicatie op dit niveau wordt verzorgd door LRS. Zie voor meer informatie [lokaal redundante opslag (LRS): lage kosten voor gegevens redundantie voor Azure Storage](../common/storage-redundancy-lrs.md).

Houd bij het bepalen van de te gebruiken replicatie optie de volgende punten in acht:

* Geo-zone-redundante opslag (GZRS) biedt een hoge Beschik baarheid en maximale duurzaamheid door gegevens synchroon te repliceren over drie Azure-beschikbaarheids zones en vervolgens gegevens asynchroon te repliceren naar de secundaire regio. U kunt ook lees toegang tot de secundaire regio inschakelen. GZRS is ontworpen om ten minste 99.99999999999999% (16 9) duurzaamheid van objecten in een bepaald jaar te bieden. Zie [geo-zone-redundante opslag voor hoge Beschik baarheid en maximale duurzaamheid (preview)](../common/storage-redundancy-gzrs.md)voor meer informatie over GZRS.
* Zone-redundante opslag (ZRS) biedt een hoge Beschik baarheid met synchrone replicatie en is mogelijk een betere keuze voor sommige scenario's dan GRS. Zie [ZRS](../common/storage-redundancy-zrs.md)voor meer informatie over ZRS.
* Asynchrone replicatie vergt een vertraging van de tijd dat gegevens naar de primaire regio worden geschreven, naar wanneer deze wordt gerepliceerd naar de secundaire regio. In het geval van een regionale ramp kunnen wijzigingen die nog niet zijn gerepliceerd naar de secundaire regio, verloren gaan als de gegevens niet kunnen worden hersteld vanuit de primaire regio.
* Met GRS is de replica niet beschikbaar voor lees-of schrijf toegang tenzij micro soft een failover naar de secundaire regio initieert. In het geval van een failover hebt u lees-en schrijf toegang tot deze gegevens nadat de failover is voltooid. Zie [richt lijnen voor herstel na nood gevallen](../common/storage-disaster-recovery-guidance.md)voor meer informatie.

## <a name="onboard-to-larger-file-shares-standard-tier"></a>Onboarding naar grotere bestands shares (Standard-laag)

Deze sectie is alleen van toepassing op de standaard bestands shares. Alle Premium-bestands shares zijn beschikbaar met een capaciteit van 100 TiB.

### <a name="restrictions"></a>Beperkingen

- LRS/ZRS naar GRS/GZRS-account conversie is niet mogelijk voor opslag accounts waarvoor grote bestands shares zijn ingeschakeld.

### <a name="regional-availability"></a>Regionale beschikbaarheid

Standaard bestands shares zijn beschikbaar in alle regio's tot 5 TiB. In bepaalde regio's zijn ze beschikbaar met een limiet van 100 TiB. deze regio's worden weer gegeven in de volgende tabel:

|Regio |Ondersteunde redundantie |
|-------|---------|
|Australië Oost |LRS     |
|Australië Zuidoost|LRS |
|Canada-Midden  |LRS     |
|Canada-Oost     |LRS     |
|India - centraal  |LRS     |
|VS-Centraal *   |LRS     |
|Azië - oost      |LRS     |
|VS-Oost *        |LRS     |
|VS-Oost 2 *      |LRS     |
|Frankrijk - centraal |LRS, ZRS|
|Frankrijk - zuid   |LRS     |
|VS - noord-centraal |LRS     |
|Europa - noord   |LRS     |
|India - zuid    |LRS     |
|Azië - zuidoost |LRS, ZRS|
|VS - west-centraal|LRS     |
|Europa-west *    |LRS, ZRS|
|VS-West *        |LRS     |
|VS - west 2      |LRS, ZRS|

\* ondersteund voor nieuwe accounts, niet alle bestaande accounts hebben het upgrade proces voltooid. U kunt controleren of uw bestaande opslag accounts het upgrade proces hebben voltooid door te proberen [grote bestands shares in te scha kelen](storage-files-how-to-create-large-file-share.md).

Vul deze [enquête](https://aka.ms/azurefilesatscalesurvey)in om u te helpen bij het bepalen van de prioriteit van nieuwe regio's en functies.

### <a name="enable-and-create-larger-file-shares"></a>Grotere bestands shares inschakelen en maken

Als u grotere bestands shares wilt gaan gebruiken, raadpleegt u ons artikel een [grote bestands share inschakelen en maken](storage-files-how-to-create-large-file-share.md).

## <a name="data-growth-pattern"></a>Gegevens groei patroon

De maximale grootte voor een Azure-bestands share is nu 100 TiB. Vanwege deze huidige beperking moet u rekening houden met de verwachte groei van de gegevens bij het implementeren van een Azure-bestands share.

Het is mogelijk om meerdere Azure-bestands shares te synchroniseren met één Windows-Bestands server met Azure File Sync. Op deze manier kunt u ervoor zorgen dat oudere, grote bestands shares die on-premises zijn, in Azure File Sync kunnen worden gebracht. Zie [planning voor een Azure file sync-implementatie](storage-files-planning.md)voor meer informatie.

## <a name="data-transfer-method"></a>Methode voor gegevens overdracht

Er zijn veel gemakkelijke opties voor het bulksgewijs overdragen van gegevens van een bestaande bestands share, zoals een on-premises bestands share, naar Azure Files. Enkele populaire items zijn (niet-limitatieve lijst):

* **Azure file sync**: als onderdeel van een eerste synchronisatie tussen een Azure-bestands share (een ' Cloud-eind punt ') en een Windows-directory naam ruimte (een ' server-eind punt ') Azure file sync, worden alle gegevens van de bestaande bestands share gerepliceerd naar Azure files.
* **[Azure import/export](../common/storage-import-export-service.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json)** : met de Azure import/export-service kunt u veilig grote hoeveel heden gegevens overdragen naar een Azure-bestands share door harde schijven naar een Azure-Data Center te verzenden. 
* **[Robocopy](https://technet.microsoft.com/library/cc733145.aspx)** : Robocopy is een bekende kopieer hulp programma dat wordt geleverd bij Windows en Windows Server. Robocopy kan worden gebruikt om gegevens over te brengen naar Azure Files door de bestands share lokaal te koppelen en vervolgens de gekoppelde locatie als bestemming in de Robocopy-opdracht te gebruiken.
* **[AzCopy](../common/storage-use-azcopy-v10.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json)** : AzCopy is een opdracht regel programma dat is ontworpen voor het kopiëren van gegevens van en naar Azure files, evenals Azure Blob-opslag, met behulp van eenvoudige opdrachten met optimale prestaties.

## <a name="next-steps"></a>Volgende stappen
* [Een Azure File Sync-implementatie plannen](storage-sync-files-planning.md)
* [Azure Files implementeren](storage-files-deployment-guide.md)
* [Azure File Sync implementeren](storage-sync-files-deployment-guide.md)
