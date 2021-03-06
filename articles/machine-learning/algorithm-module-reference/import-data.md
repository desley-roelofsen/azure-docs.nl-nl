---
title: 'Gegevens importeren: module verwijzing'
titleSuffix: Azure Machine Learning
description: Leer hoe u de module gegevens importeren in Azure Machine Learning kunt gebruiken om gegevens in een machine learning pijp lijn te laden vanuit bestaande Cloud Data Services.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: xiaoharper
ms.author: zhanxia
ms.date: 10/22/2019
ms.openlocfilehash: 5fa8d3984c758d0bf95372864f3bffeb6f302c83
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/04/2019
ms.locfileid: "73497784"
---
# <a name="import-data-module"></a>Gegevens module importeren

In dit artikel wordt een module in Azure Machine Learning Designer (preview) beschreven.

Gebruik deze module om gegevens te laden in een machine learning pijp lijn vanuit bestaande Cloud Data Services. 

> [!Note]
> Alle functionaliteit van deze module kan worden uitgevoerd door **gegevens opslag** en **gegevens sets** op de werk ruimte-landings pagina. U wordt aangeraden om data **Store** en **DataSet** te gebruiken. Dit omvat aanvullende functies zoals gegevens bewaking. Zie voor meer informatie [hoe u toegang krijgt tot gegevens](../service/how-to-access-data.md) en het artikel gegevens [sets registreren](../service/how-to-create-register-datasets.md) .
> Nadat u een gegevensset hebt geregistreerd, kunt u deze vinden in de categorie **gegevens sets** -> **mijn gegevens sets** in de ontwerp interface. Deze module is gereserveerd voor Studio-gebruikers (klassiek) voor een vertrouwde ervaring. 
>

Kies eerst de bron waarvan u een lees bewerking hebt uitgevoerd en voltooi de aanvullende instellingen. De module **gegevens importeren** ondersteunt het lezen van gegevens uit de volgende bronnen:

- URL via HTTP
- Azure-Cloud opslag via [**data stores**](../service/how-to-access-data.md))
    - Azure Blob-container
    - Azure-bestandsshare
    - Azure Data Lake
    - Azure Data Lake Gen2
    - Azure SQL Database
    - Azure-PostgreSQL    

Voordat u Cloud opslag gebruikt, moet u eerst een gegevens opslag registreren in uw Azure Machine Learning-werk ruimte. Zie [toegang tot gegevens](../service/how-to-access-data.md)voor meer informatie. 

Nadat u de gewenste gegevens hebt gedefinieerd en verbinding wilt maken met de bron, wordt het gegevens type van elke kolom afgeleid op basis van de waarden die erin worden **[weer gegeven en](./import-data.md)** worden de gegevens in uw ontwerp pijplijn geladen. De uitvoer van **import gegevens** is een gegevensset die kan worden gebruikt met een wille keurige ontwerp pijplijn.

Als de bron gegevens worden gewijzigd, kunt u de gegevensset vernieuwen en nieuwe gegevens toevoegen door [import gegevens](./import-data.md)opnieuw uit te voeren. Als u echter niet wilt dat elke keer dat u de pijp lijn uitvoert, de bron opnieuw moet worden gelezen, stelt u de optie **in cache opgeslagen resultaten gebruiken** in op True. Als deze optie is geselecteerd, controleert de module of de pijp lijn eerder is uitgevoerd met dezelfde bron en dezelfde invoer opties. Als er een vorige run wordt gevonden, worden de gegevens in de cache gebruikt, in plaats van de gegevens uit de bron opnieuw te laden.

## <a name="how-to-configure-import-data"></a>Import gegevens configureren

1. Voeg de module **gegevens importeren** toe aan de pijp lijn. U kunt deze module vinden in de categorie **gegevens invoer en uitvoer** in de ontwerp functie.

1. Klik op **wizard gegevens importeren starten** om de gegevens bron te configureren met behulp van een wizard.

    De wizard haalt de account naam en referenties op en helpt u bij het configureren van andere opties. Als u een bestaande configuratie bewerkt, worden eerst de huidige waarden geladen.

1. Selecteer **gegevens bron**en kies het type gegevens bron. Dit kan HTTP of gegevens opslag zijn.

    Als u gegevens opslag kiest, kunt u bestaande gegevens opslag selecteren die al in uw Azure Machine Learning-werk ruimte is geregistreerd of een nieuw gegevens archief maken. Definieer vervolgens het pad van de gegevens die moeten worden geïmporteerd in het gegevens archief. U kunt eenvoudig door het pad bladeren door te klikken op **Bladeren pad** ![import-data-Path](media/module/import-data-path.png)

1. Selecteer het voorbeeld schema voor het filteren van de kolommen die u wilt toevoegen. U kunt ook geavanceerde instellingen definiëren als scheidings teken bij het parseren van opties.

    ![import-data-preview](media/module/import-data.png)

1. Selecteer de optie **in cache opgeslagen resultaten gebruiken** als u de gegevensset in de cache wilt opslaan voor hergebruik op opeenvolgende uitvoeringen.

    Ervan uitgaande dat er geen andere wijzigingen zijn aangebracht in de module parameters, laadt de pijp lijn de gegevens alleen de eerste keer dat de module wordt uitgevoerd en wordt vervolgens een versie van de gegevensset in de cache gebruikt.

    Schakel deze optie uit als u de gegevens telkens opnieuw wilt laden wanneer u de pijp lijn uitvoert.

1. Voer de pijplijn uit.

    Wanneer gegevens importeren de gegevens in de ontwerp functie laadt, wordt het gegevens type van elke kolom afgeleid op basis van de waarden die het bevat, ofwel numeriek of categorische.

    Als er een header aanwezig is, wordt de header gebruikt om de kolommen van de uitvoer gegevensset te benoemen.

    Als er geen bestaande kolom koppen in de gegevens staan, worden nieuwe kolom namen gegenereerd met de indeling Kol1, col2,... , coln*.

## <a name="results"></a>Resultaten

Wanneer het importeren is voltooid, klikt u op de uitvoer gegevensset en selecteert u **visualiseren** om te zien of de gegevens zijn geïmporteerd.

Als u de gegevens wilt opslaan voor hergebruik in plaats van een nieuwe set gegevens te importeren telkens wanneer de pijp lijn wordt uitgevoerd, klikt u met de rechter muisknop op de uitvoer en selecteert u **Opslaan als gegevensset**. Kies een naam voor de gegevensset. De opgeslagen gegevensset behoudt de gegevens op het moment van opslaan en er worden geen gegevens bijgewerkt wanneer de pijp lijn opnieuw wordt uitgevoerd, zelfs als de gegevensset in de pijp lijn wordt gewijzigd. Dit kan handig zijn voor het maken van moment opnamen van gegevens.

Na het importeren van de gegevens zijn mogelijk extra voor bereid voor het maken en analyseren van modellen nodig:

- Gebruik [meta gegevens bewerken](./edit-metadata.md) om kolom namen te wijzigen, om een kolom te verwerken als een ander gegevens type, of om aan te geven dat sommige kolommen labels of functies zijn.

- Gebruik [select columns in dataset](./select-columns-in-dataset.md) om een subset van kolommen te selecteren die u wilt transformeren of gebruiken in model lering. De getransformeerde of verwijderde kolommen kunnen eenvoudig opnieuw worden samengevoegd met de oorspronkelijke gegevensset met behulp van de module [add columns](./add-columns.md) .  

- Gebruik [partitie en voor beeld](./partition-and-sample.md) om de gegevensset te verdelen, steek proeven uit te voeren of de bovenste n rijen te verkrijgen.

## <a name="next-steps"></a>Volgende stappen

Bekijk de [set met modules die beschikbaar zijn](module-reference.md) voor Azure machine learning. 
