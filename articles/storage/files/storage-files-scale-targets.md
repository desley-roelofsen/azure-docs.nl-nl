---
title: Azure Files schaalbaarheids-en prestatie doelen | Microsoft Docs
description: Meer informatie over de schaalbaarheids-en prestatie doelen voor Azure Files, met inbegrip van de capaciteit, aanvraag snelheid en binnenkomende en uitgaande bandbreedte limieten.
author: roygara
ms.service: storage
ms.topic: conceptual
ms.date: 10/16/2019
ms.author: rogarana
ms.subservice: files
ms.openlocfilehash: 267a63eba90c74b79078a7c04c1d2d8929cf2a44
ms.sourcegitcommit: b2fb32ae73b12cf2d180e6e4ffffa13a31aa4c6f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/05/2019
ms.locfileid: "73615780"
---
# <a name="azure-files-scalability-and-performance-targets"></a>Schaalbaarheids-en prestatie doelen Azure Files

[Azure files](storage-files-introduction.md) biedt volledig beheerde bestands shares in de cloud die toegankelijk zijn via het industrie standaard SMB-protocol. In dit artikel worden de schaalbaarheids-en prestatie doelen voor Azure Files en Azure File Sync beschreven.

De schaal baarheid en prestatie doelen die hier worden weer gegeven, zijn hoogwaardige doelen, maar kunnen worden beïnvloed door andere variabelen in uw implementatie. De door Voer voor een bestand kan bijvoorbeeld ook worden beperkt door de beschik bare netwerk bandbreedte, niet alleen de servers die als host fungeren voor de Azure Files-service. We raden u ten zeerste aan om uw gebruiks patroon te testen om te bepalen of de schaal baarheid en prestaties van Azure Files voldoen aan uw vereisten. We zijn ook van belang om deze limieten in de loop van de tijd te verhogen. Aarzel niet om ons feedback te geven, hetzij in de opmerkingen hieronder of op het [Azure files UserVoice](https://feedback.azure.com/forums/217298-storage/category/180670-files), over welke limieten u zou willen zien.

## <a name="azure-storage-account-scale-targets"></a>Schaal doelen Azure Storage-account

De bovenliggende resource voor een Azure-bestands share is een Azure-opslag account. Een opslag account vertegenwoordigt een opslag groep in azure die kan worden gebruikt door meerdere opslag Services, waaronder Azure Files, om gegevens op te slaan. Andere services voor het opslaan van gegevens in opslag accounts zijn Azure Blob-opslag, Azure Queue-opslag en Azure-tabel opslag. De volgende doelen zijn van toepassing op alle opslag services die gegevens opslaan in een opslag account:

[!INCLUDE [azure-storage-limits](../../../includes/azure-storage-limits.md)]

[!INCLUDE [azure-storage-limits-azure-resource-manager](../../../includes/azure-storage-limits-azure-resource-manager.md)]

> [!Important]  
> Het gebruik van het opslag account voor algemene doel einden van andere opslag Services is van invloed op uw Azure-bestands shares in uw opslag account. Als u bijvoorbeeld de maximale opslag capaciteit met Azure Blob-opslag bereikt, kunt u geen nieuwe bestanden maken op uw Azure-bestands share, zelfs als uw Azure-bestands share lager is dan de maximale share grootte.

## <a name="azure-files-scale-targets"></a>Azure Files schaal doelen

Er zijn drie categorieën beperkingen waarmee u rekening moet houden voor Azure Files: opslag accounts, shares en bestanden.

Bijvoorbeeld: met Premium-bestands shares kan een enkele share 100.000 IOPS behaalt en kan één bestand tot 5.000 IOPS worden geschaald. Als u dus drie bestanden in één share hebt, is het maximale aantal IOPS dat u van deze share kunt verkrijgen 15.000.

### <a name="standard-storage-account-limits"></a>Limieten voor standaard opslag accounts

Zie de sectie [bereik schaal doelen van Azure Storage](#azure-storage-account-scale-targets) voor deze limieten.

### <a name="premium-filestorage-account-limits"></a>Premium FileStorage-account limieten

[!INCLUDE [azure-storage-limits-filestorage](../../../includes/azure-storage-limits-filestorage.md)]

> [!IMPORTANT]
> Limieten voor opslag accounts zijn van toepassing op alle shares. Het omhoog schalen naar het maximum voor FileStorage-accounts wordt alleen behaald als er slechts één share per FileStorage-account is.

### <a name="file-share-and-file-scale-targets"></a>Bestands shares en bestands schaal doelen

> [!NOTE]
> Standaard bestands shares groter dan 5 TiB hebben bepaalde beperkingen en regionale beperkingen.
> Zie de sectie [onboarding to large shares](storage-files-planning.md#onboard-to-larger-file-shares-standard-tier) in de plannings handleiding voor een lijst met beperkingen, regionale informatie en instructies voor het inschakelen van deze grotere grootte van de bestands share.

[!INCLUDE [storage-files-scale-targets](../../../includes/storage-files-scale-targets.md)]

[!INCLUDE [storage-files-premium-scale-targets](../../../includes/storage-files-premium-scale-targets.md)]

## <a name="azure-file-sync-scale-targets"></a>Azure File Sync schaal doelen

Azure File Sync is ontworpen met het doel van oneindig gebruik, maar oneindig gebruik is niet altijd mogelijk. De volgende tabel geeft de grenzen van de test van micro soft aan en geeft ook aan welke doelen vaste limieten zijn:

[!INCLUDE [storage-sync-files-scale-targets](../../../includes/storage-sync-files-scale-targets.md)]

### <a name="azure-file-sync-performance-metrics"></a>Metrische gegevens voor Azure File Sync prestaties

Omdat de Azure File Sync-agent wordt uitgevoerd op een Windows Server-computer die verbinding maakt met de Azure-bestands shares, is de daad werkelijke synchronisatie prestaties afhankelijk van een aantal factoren in uw infra structuur: Windows Server en de onderliggende schijf configuratie, netwerk bandbreedte tussen de server en de Azure-opslag, de bestands grootte, de totale grootte van de gegevensset en de activiteit op de gegevensset. Omdat Azure File Sync op bestands niveau werkt, worden de prestatie kenmerken van een oplossing op basis van een Azure File Sync beter gemeten in het aantal objecten (bestanden en mappen) dat per seconde wordt verwerkt.

Voor Azure File Sync zijn de prestaties in twee fasen kritiek:

1. **Eerste**eenmalige inrichting: als u de prestaties van de eerste inrichting wilt optimaliseren, raadpleegt u de voor bereiding op [Azure file sync](storage-sync-files-deployment-guide.md#onboarding-with-azure-file-sync) voor de optimale implementatie details.
2. **Voortdurende synchronisatie**: nadat de gegevens in eerste instantie zijn geseed in de Azure-bestands shares, Azure File Sync meerdere eind punten in de synchronisatie bewaard.

Om u te helpen bij het plannen van uw implementatie voor elk van de fasen, zijn de resultaten die zijn waargenomen tijdens de interne test op een systeem met een configuratie

| Systeem configuratie |  |
|-|-|
| CPU | 64 virtuele kernen met 64 MiB L3-cache |
| Geheugen | 128 GiB |
| Schijf | SAS-schijven met RAID 10 met cache met batterij back-ups |
| Netwerk | 1 Gbps-netwerk |
| Workload | Algemeen Bestands server|

| Eerste eenmalige inrichting  |  |
|-|-|
| Aantal objecten | 25.000.000-objecten |
| Grootte van gegevensset| ~ 4,7 TiB |
| Gemiddelde bestands grootte | ~ 200 KiB (grootste bestand: 100 GiB) |
| Upload doorvoer | 20 objecten per seconde |
| Door Voer van naam ruimte downloaden * | 400 objecten per seconde |

\* Wanneer een nieuw server eindpunt wordt gemaakt, wordt de bestands inhoud niet gedownload door de Azure File Sync-agent. Eerst wordt de volledige naam ruimte gesynchroniseerd en vervolgens wordt de achtergrond terugroepen geactiveerd om de bestanden te downloaden, in hun geheel of, als Cloud lagen zijn ingeschakeld, naar het beleid voor Cloud lagen dat is ingesteld op het server eindpunt.

| Voortdurende synchronisatie  |   |
|-|--|
| Aantal gesynchroniseerde objecten| 125.000-objecten (~ 1% verloop) |
| Grootte van gegevensset| 50 GiB |
| Gemiddelde bestands grootte | ~ 500 KiB |
| Upload doorvoer | 20 objecten per seconde |
| Volledige down load door Voer * | 60 objecten per seconde |

\* Als Cloud lagen zijn ingeschakeld, ziet u waarschijnlijk betere prestaties omdat slechts een deel van de bestands gegevens worden gedownload. Met Azure File Sync worden alleen de gegevens van bestanden in de cache gedownload wanneer deze op een van de eind punten worden gewijzigd. Voor alle gelaagde of zojuist gemaakte bestanden worden de bestands gegevens niet gedownload door de agent en wordt de naam ruimte in plaats daarvan gesynchroniseerd met alle server eindpunten. De agent biedt ook ondersteuning voor gedeeltelijke down loads van gelaagde bestanden, aangezien deze door de gebruiker worden geopend. 

> [!Note]  
> De bovenstaande getallen zijn geen indicatie van de prestaties die u zult ervaren. De daad werkelijke prestaties zijn afhankelijk van meerdere factoren die aan het begin van deze sectie worden beschreven.

Als algemene hand leiding voor uw implementatie moet u een aantal zaken in acht nemen:

- De object doorvoer wordt ongeveer proportioneel geschaald naar rato van het aantal synchronisatie groepen op de server. Het splitsen van gegevens in meerdere synchronisatie groepen op een server levert een betere door Voer, die ook wordt beperkt door de server en het netwerk.
- De object doorvoer is omgekeerd evenredig met de MiB per tweede door voer. Voor kleinere bestanden krijgt u een hogere door Voer in termen van het aantal verwerkte objecten per seconde, maar lagere MiB per seconde door voer. Voor grotere bestanden krijgt u echter minder objecten die per seconde worden verwerkt, maar een hogere MiB per seconde door voer. De MiB per tweede door Voer wordt beperkt door de Azure Files schaal doelen.

## <a name="see-also"></a>Zie ook

- [Implementatie van Azure Files plannen](storage-files-planning.md)
- [Planning voor een Azure Files Sync-implementatie](storage-sync-files-planning.md)
- [Schaalbaarheids-en prestatie doelen voor andere opslag Services](../common/storage-scalability-targets.md)
