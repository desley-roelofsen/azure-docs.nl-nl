---
author: roygara
ms.service: storage
ms.topic: include
ms.date: 06/07/2019
ms.author: rogarana
ms.openlocfilehash: b28427b3ede0cfaeb9e08d3c73b15ea7f2961f1b
ms.sourcegitcommit: 83df2aed7cafb493b36d93b1699d24f36c1daa45
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/22/2019
ms.locfileid: "71180043"
---
#### <a name="additional-premium-file-share-level-limits"></a>Extra limieten voor Premium file share-niveaus

|Onderwerp  |Doel  |
|---------|---------|
|Toename van minimale grootte/afname    |1 GiB      |
|IOPS-basislijn    |1 IOPS per GiB, Maxi maal 100.000|
|IOPS-bursting    |3x IOPS per GiB, Maxi maal 100.000|
|Uitgangs frequentie         |60-MiB/s + 0,06 * ingerichte GiB        |
|Ingangs frequentie| 40-MiB/s + 0,04 * ingerichte GiB |

#### <a name="file-level-limits"></a>Limieten voor bestands niveau

|Onderwerp  |Premium-bestand  |Standaard bestand |
|---------|---------|---------|
|Size                  |1 TiB         |1 TiB|
|Maximum aantal IOPS per bestand     |5,000         |1000|
|Gelijktijdige ingangen    |2,000         |2,000|
|Uitgaand verkeer  |300-MiB per seconde|      Zie standaard waarden voor doorvoer bestanden|
|Inkomend verkeer  |200-MiB per seconde| Zie standaard waarden voor doorvoer bestanden|
|Doorvoer| Zie Premium file ingress/uitgangs waarden| Maxi maal 60 MiB per seconde|
