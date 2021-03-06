---
title: Uitvoerings weergave vertex in Data Lake-Hulpprogram Ma's voor Visual Studio
description: In dit artikel wordt beschreven hoe u de vertex-uitvoerings weergave kunt gebruiken om Data Lake Analytics taken te examenen.
services: data-lake-analytics
ms.service: data-lake-analytics
author: jasonwhowell
ms.author: jasonh
ms.assetid: 5366d852-e7d6-44cf-a88c-e9f52f15f7df
ms.topic: conceptual
ms.date: 10/13/2016
ms.openlocfilehash: f5adbb75e6852551976aa040a1a1c723d2e3f59b
ms.sourcegitcommit: 0486aba120c284157dfebbdaf6e23e038c8a5a15
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/26/2019
ms.locfileid: "71309714"
---
# <a name="use-the-vertex-execution-view-in-data-lake-tools-for-visual-studio"></a>De vertex Execution View gebruiken in Data Lake-Hulpprogram Ma's voor Visual Studio
Meer informatie over het gebruik van de vertex-uitvoerings weergave om Data Lake Analytics taken te examen.


## <a name="open-the-vertex-execution-view"></a>De vertex Execution View openen
Open een U-SQL-taak in Data Lake-Hulpprogram Ma's voor Visual Studio. Klik in de linkerbenedenhoek op **vertex Execution View** . U wordt mogelijk gevraagd om eerst profielen te laden. Dit kan enige tijd duren, afhankelijk van de netwerk verbinding.

![Uitvoerings weergave van Data Lake Analytics tools vertex](./media/data-lake-analytics-data-lake-tools-use-vertex-execution-view/data-lake-tools-open-vertex-execution-view.png)

## <a name="understand-vertex-execution-view"></a>Meer informatie over vertex Execution View
De uitvoerings weergave van het vertex bestaat uit drie delen:

![Uitvoerings weergave van Data Lake Analytics tools vertex](./media/data-lake-analytics-data-lake-tools-use-vertex-execution-view/data-lake-tools-vertex-execution-view.png)

Met de **hoek punt kiezer** links kunt u hoek punten selecteren op basis van functies (zoals de Top 10 van gegevens lezen of kiezen op werk gebied). Een van de meest gebruikte filters is het weer geven van de **hoek punten op het kritieke pad**. Het **kritieke pad** is de langste keten van hoek punten van een U-SQL-taak. Meer informatie over het kritieke pad is handig voor het optimaliseren van uw taken door te controleren welk hoek punt de langste tijd kost.
  
![Uitvoerings weergave van Data Lake Analytics tools vertex](./media/data-lake-analytics-data-lake-tools-use-vertex-execution-view/data-lake-tools-vertex-execution-view-pane2.png)

In het bovenste middelste deel venster wordt de **uitvoerings status van alle hoek punten**weer gegeven.
  
![Uitvoerings weergave van Data Lake Analytics tools vertex](./media/data-lake-analytics-data-lake-tools-use-vertex-execution-view/data-lake-tools-vertex-execution-view-pane3.png)

Het onderste middelste deel venster toont informatie over elk hoek punt:
* Proces naam: De naam van het vertex exemplaar. Het bestaat uit verschillende onderdelen in de naam van de fase | Hoek punt | VertexRunInstance. De SV7_Split [62]. v1 hoekpunt staat bijvoorbeeld voor het tweede uitgevoerde exemplaar (. v1, index vanaf 0) van het hoekpunt nummer 62 in fase SV7_Split.
* Totaal aantal gelezen/geschreven gegevens: De gegevens zijn door dit hoek punt gelezen/geschreven.
* Status/afsluiten: De uiteindelijke status wanneer het hoek punt is beëindigd.
* Afsluit code/type fout: De fout wanneer het hoek punt is mislukt.
* Reden voor maken: Waarom het hoek punt is gemaakt.
* Vertraging van resource latentie/verwerkings latentie/PN-wachtrij: de tijd die nodig is om te wachten op resources, het verwerken van gegevens en het blijven van de wachtrij.
* GUID van proces/maker: GUID voor het huidige actieve hoek punt of de maker.
* Versie: het N-ste exemplaar van het knoop punt dat wordt uitgevoerd (het systeem kan om verschillende redenen nieuwe exemplaren van een hoek punt plannen, bijvoorbeeld failover, reken redundantie, enzovoort)
* Tijdstip waarop de versie is gemaakt.
* Begin tijd/proces van het proces voor het maken van de tijd/het proces in de wachtrij geplaatst/begin tijd/proces is voltooid: wanneer het vertex proces wordt gemaakt. Wanneer het vertex proces begint met de wachtrij; Wanneer het bepaalde vertex proces wordt gestart; Wanneer het bepaalde hoek punt is voltooid.

## <a name="next-steps"></a>Volgende stappen
* Zie [Accessing diagnostics logs for Azure Data Lake Analytics](data-lake-analytics-diagnostic-logs.md) (Diagnostische logboeken openen voor Azure Data Lake Analytics) voor logboekregistratie van diagnostische informatie.
* Zie [Websitelogboeken analyseren met Azure Data Lake Analytics](data-lake-analytics-analyze-weblogs.md) voor een complexere query.
* Zie [taak browser en taak weergave gebruiken voor Azure data Lake Analytics-taken voor](data-lake-analytics-data-lake-tools-view-jobs.md) meer informatie over het weer geven van taken
