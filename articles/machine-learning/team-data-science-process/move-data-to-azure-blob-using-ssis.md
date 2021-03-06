---
title: Blob Storage-gegevens verplaatsen met SSIS-connectors-team data Science process
description: Meer informatie over het verplaatsen van gegevens naar of van Azure Blob Storage met SQL Server Integration Services Feature Pack voor Azure.
services: machine-learning
author: marktab
manager: cgronlun
editor: cgronlun
ms.service: machine-learning
ms.subservice: team-data-science-process
ms.topic: article
ms.date: 11/04/2017
ms.author: tdsp
ms.custom: seodec18, previous-author=deguhath, previous-ms.author=deguhath
ms.openlocfilehash: 8fb802905070ab1ddc8af93e501e8e11e262cf4a
ms.sourcegitcommit: 87efc325493b1cae546e4cc4b89d9a5e3df94d31
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/29/2019
ms.locfileid: "73053934"
---
# <a name="move-data-to-or-from-azure-blob-storage-using-ssis-connectors"></a>Gegevens verplaatsen van of naar Azure Blob Storage met SSIS-connectors
Het [SQL Server Integration Services Feature Pack voor Azure](https://msdn.microsoft.com/library/mt146770.aspx) biedt onderdelen om verbinding te maken met Azure, gegevens over te dragen tussen Azure-en on-premises gegevens bronnen en gegevens die zijn opgeslagen in azure te verwerken.

[!INCLUDE [blob-storage-tool-selector](../../../includes/machine-learning-blob-storage-tool-selector.md)]

Zodra klanten on-premises gegevens naar de Cloud hebben verplaatst, kunnen ze toegang krijgen tot de Azure-service om de volledige kracht van de Azure-technologieën te benutten. Het kan bijvoorbeeld worden gebruikt in Azure Machine Learning of op een HDInsight-cluster.

Dit is normaal gesp roken de eerste stap voor de [SQL](sql-walkthrough.md) -en [HDInsight](hive-walkthrough.md) -scenario's.

Zie [meer met SQL Server Integration Services Feature Pack voor Azure](https://blogs.msdn.com/b/ssis/archive/2015/06/25/doing-more-with-sql-server-integration-services-feature-pack-for-azure.aspx) blog voor een bespreking van canonieke scenario's die gebruikmaken van SSIS om bedrijfs behoeften te bereiken die gemeen schappelijk zijn voor hybride gegevens integratie scenario's.

> [!NOTE]
> Raadpleeg Azure Blob- [basis beginselen](../../storage/blobs/storage-dotnet-how-to-use-blobs.md) en [Azure Blob service](https://msdn.microsoft.com/library/azure/dd179376.aspx)voor een volledige inleiding tot Azure Blob-opslag.
> 
> 

## <a name="prerequisites"></a>Vereisten
Als u de taken die in dit artikel worden beschreven wilt uitvoeren, moet u een Azure-abonnement en een Azure Storage-account instellen. U moet de naam en de account sleutel van uw Azure Storage-account kennen om gegevens te uploaden of te downloaden.

* Zie [een gratis proef versie van één maand](https://azure.microsoft.com/pricing/free-trial/)voor meer informatie over het instellen van een **Azure-abonnement**.
* Zie [over Azure Storage-accounts](../../storage/common/storage-create-storage-account.md)voor instructies over het maken van een **opslag account** en voor het ophalen van account-en sleutel gegevens.

Als u de **SSIS-connectors**wilt gebruiken, moet u het volgende downloaden:

* **SQL Server 2014 of 2016 Standard (of hoger)** : de installatie bevat SQL Server Integration Services.
* **Microsoft SQL Server 2014 of 2016 Integration Services Feature Pack voor Azure**: deze kunnen respectievelijk worden gedownload via de pagina's [SQL Server 2014 integration services](https://www.microsoft.com/download/details.aspx?id=47366) en [SQL Server 2016 Integration Services](https://www.microsoft.com/download/details.aspx?id=49492) .

> [!NOTE]
> SSIS wordt geïnstalleerd met SQL Server, maar is niet opgenomen in de snelle versie. Zie [SQL Server-edities](https://www.microsoft.com/en-us/server-cloud/products/sql-server-editions/) voor meer informatie over welke toepassingen zijn opgenomen in de verschillende edities van SQL Server.
> 
> 

Voor trainings materiaal op SSIS, Zie [handen voor training voor SSIS](https://www.microsoft.com/sql-server/training-certification)

Zie [SSIS-zelf studie: een eenvoudig ETL-pakket maken](https://msdn.microsoft.com/library/ms169917.aspx)voor informatie over het ophalen van en het uitvoeren van SISS voor het bouwen van eenvoudige pakketten voor het uitpakken, transformeren en laden (ETL).

## <a name="download-nyc-taxi-dataset"></a>NYC taxi-gegevensset downloaden
In het voor beeld dat hier wordt beschreven, wordt een openbaar beschik bare gegevensset gebruikt: de NYC van de [taxi trips](https://www.andresmh.com/nyctaxitrips/) . De gegevensset bestaat uit ongeveer 173.000.000 taxi-onderdrukkingen in NYC van het jaar 2013. Er zijn twee soorten gegevens: reis gegevens en gegevens over ritbedrag. Als er een bestand is voor elke maand, hebben we 24 bestanden in alle, die elk ongeveer 2 GB ongecomprimeerd zijn.

## <a name="upload-data-to-azure-blob-storage"></a>Gegevens uploaden naar Azure Blob Storage
Als u gegevens wilt verplaatsen met behulp van het SSIS-functie pakket van on-premises naar Azure Blob-opslag, wordt een exemplaar van de [**Azure Blob upload-taak**](https://msdn.microsoft.com/library/mt146776.aspx)gebruikt, zoals hier wordt weer gegeven:

![configure-data-Science-VM](./media/move-data-to-azure-blob-using-ssis/ssis-azure-blob-upload-task.png)

De para meters die de taak gebruikt, worden hier beschreven:

| Veld | Beschrijving |
| --- | --- |
| **AzureStorageConnection** |Hiermee geeft u een bestaand Azure Storage verbindings beheer of maakt u een nieuw item dat verwijst naar een Azure Storage-account waarnaar wordt verwezen naar de locatie van de BLOB-bestanden. |
| **Zijn** |Hiermee geeft u de naam van de BLOB-container die de geüploade bestanden als blobs bevat. |
| **BlobDirectory** |Hiermee geeft u de BLOB-map op waarin het geüploade bestand wordt opgeslagen als een blok-blob. De BLOB-map is een virtuele hiërarchische structuur. Als de BLOB al bestaat, wordt de IA vervangen. |
| **LocalDirectory** |Hiermee geeft u de lokale map met de bestanden die moeten worden geüpload. |
| **Bestands** |Hiermee geeft u een naam filter op waarmee bestanden met het opgegeven naam patroon worden geselecteerd. Zo bevat MySheet\*. xls\* bestanden zoals MySheet001. xls en MySheetABC. xlsx |
| **TimeRangeFrom/TimeRangeTo** |Hiermee geeft u een tijds bereik filter op. Bestanden die zijn gewijzigd na *TimeRangeFrom* en voordat *TimeRangeTo* zijn opgenomen. |

> [!NOTE]
> De **AzureStorageConnection** -referenties moeten juist zijn en de **zijn** moeten bestaan voordat de overdracht wordt uitgevoerd.
> 
> 

## <a name="download-data-from-azure-blob-storage"></a>Gegevens downloaden van Azure Blob-opslag
Als u gegevens van Azure Blob-opslag naar on-premises opslag met SSIS wilt downloaden, gebruikt u een exemplaar van de [Azure Blob-Download taak](https://msdn.microsoft.com/library/mt146779.aspx).

## <a name="more-advanced-ssis-azure-scenarios"></a>Meer geavanceerde SSIS-Azure-scenario's
Met het SSIS Feature Pack kunnen complexere stromen worden verwerkt door taken samen te pakken. De BLOB-gegevens kunnen bijvoorbeeld rechtstreeks in een HDInsight-cluster worden gefeedd waarvan de uitvoer kan worden gedownload naar een BLOB en vervolgens naar de on-premises opslag. SSIS kan Hive-en Pig-taken uitvoeren op een HDInsight-cluster met extra SSIS-connectors:

* Als u een Hive-script op een Azure HDInsight-cluster met SSIS wilt uitvoeren, gebruikt u de [component taak van Azure hdinsight](https://msdn.microsoft.com/library/mt146771.aspx).
* Als u een Pig-script wilt uitvoeren op een Azure HDInsight-cluster met SSIS, gebruikt u de [Azure Hdinsight Pig-taak](https://msdn.microsoft.com/library/mt146781.aspx).

