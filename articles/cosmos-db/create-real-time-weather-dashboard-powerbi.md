---
title: Maak een real-time dash board met behulp van Azure Cosmos DB, Azure Analysis Services en Power BI
description: Meer informatie over het maken van een live weers-dash board in Power BI met behulp van Azure Cosmos DB en Azure Analysis Services.
author: bharathsreenivas
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 09/04/2019
ms.author: bharathb
ms.reviewer: sngun
ms.openlocfilehash: d225a14edddcad58c08094dbc758d67df8f834e6
ms.sourcegitcommit: aebe5a10fa828733bbfb95296d400f4bc579533c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/05/2019
ms.locfileid: "70376591"
---
# <a name="create-a-real-time-dashboard-using-azure-cosmos-db-and-power-bi"></a>Maak een realtime-dash board met behulp van Azure Cosmos DB en Power BI

In dit artikel worden de stappen beschreven die nodig zijn om een live weers-dash board te maken in Power BI met behulp van Azure Cosmos DB en Azure Analysis Services. In het Power BI-dash board worden grafieken weer gegeven om realtime informatie over de Tempe ratuur en neer slag in een regio weer te geven.

## <a name="reporting-scenarios"></a>Rapportage scenario's

Er zijn meerdere manieren om rapportage dashboards in te stellen voor gegevens die zijn opgeslagen in Azure Cosmos DB. Afhankelijk van de vereisten voor veroudering en de grootte van de gegevens, wordt in de volgende tabel de rapportage-instellingen voor elk scenario beschreven:


|Scenario |Instellen |
|---------|---------|
|1. Ad-hocrapporten genereren (niet vernieuwen)    |  [Azure Cosmos DB connector Power BI met de import modus](powerbi-visualize.md)       |
|2. Ad-hocrapporten genereren met periodiek vernieuwen   |  [Power BI Azure Cosmos DB-connector met import modus (geplande periodieke vernieuwing)](powerbi-visualize.md)       |
|3. Rapportage over grote gegevens sets (< 10 GB)     |  Power BI Azure Cosmos DB-connector met incrementeel vernieuwen       |
|4. Real-time rapportages voor grote gegevens sets    |  Power BI Azure Analysis Services-connector met direct query + Azure Analysis Services (Azure Cosmos DB-connector)       |
|5. Rapportage over Live-gegevens met aggregaties     |  [Power BI Spark-connector met direct query + Azure Databricks + Cosmos DB Spark-connector.](https://github.com/Azure/azure-cosmosdb-spark/wiki/Connecting-Cosmos-DB-with-PowerBI-using-spark-and-databricks-premium)       |
|6. Rapportage over Live-gegevens met aggregaties voor grote gegevens sets   |  Power BI Azure Analysis Services-connector met direct query + Azure Analysis Services + Azure Databricks + Cosmos DB Spark-connector.       |

Scenario's 1 en 2 kunnen eenvoudig worden ingesteld met behulp van de Azure Cosmos DB Power BI-connector. In dit artikel worden de instellingen voor de scenario's 3 en 4 beschreven.

### <a name="power-bi-with-incremental-refresh"></a>Power BI met incrementeel vernieuwen

Power BI heeft een modus waarin incrementeel vernieuwen kan worden geconfigureerd. Deze modus elimineert de nood zaak om Azure Analysis Services partities te maken en te beheren. Incrementeel vernieuwen kan worden ingesteld voor het filteren van alleen de meest recente updates in grote gegevens sets. Deze modus werkt echter alleen met Power BI Premium-service met een limiet van 10 GB voor de gegevensset.

### <a name="power-bi-azure-analysis-connector--azure-analysis-services"></a>Power BI Azure Analysis connector + Azure Analysis Services

Azure Analysis Services biedt een volledig beheerd platform als een service die als host fungeert voor gegevens modellen op ondernemings niveau in de Cloud. Enorme gegevens sets kunnen worden geladen van Azure Cosmos DB in Azure Analysis Services. Om te voor komen dat er altijd een query op de volledige gegevensset wordt doorgevoerd, kunnen de gegevens sets worden onderverdeeld in Azure Analysis Services partities, die onafhankelijk kunnen worden vernieuwd op verschillende frequenties.

## <a name="power-bi-incremental-refresh"></a>Power BI incrementeel vernieuwen

### <a name="ingest-weather-data-into-azure-cosmos-db"></a>Weer gegevens opnemen in Azure Cosmos DB

Stel een opname pijplijn in om [weer gegevens](https://catalog.data.gov/dataset/local-weather-archive) naar Azure Cosmos DB te laden. U kunt een Azure Data Factory-taak [(ADF)](../data-factory/connector-azure-cosmos-db.md) instellen om periodiek de meest recente weer gegevens in azure Cosmos DB te laden met behulp van de HTTP-bron en Cosmos DB sink.


### <a name="connect-power-bi-to-azure-cosmos-db"></a>Power BI verbinden met Azure Cosmos DB

1. **Verbinding maken met Azure Cosmos-account om Power bi** : Open de Power bi Desktop en gebruik de Azure Cosmos DB connector om de juiste data base en container te selecteren.

   ![Azure Cosmos DB - Power BI-connector](./media/create-real-time-weather-dashboard-powerbi/cosmosdb-powerbi-connector.png)

1. **Incrementeel vernieuwen configureren** : Volg de stappen in [Incrementeel vernieuwen met Power bi](/power-bi/service-premium-incremental-refresh) artikel om incrementeel vernieuwen voor de gegevensset in te stellen. Voeg de para meter **Range** en **RangeEnd** toe, zoals wordt weer gegeven in de volgende scherm afbeelding:

   ![Bereik parameters configureren](./media/create-real-time-weather-dashboard-powerbi/configure-range-parameters.png)

   Omdat de gegevensset een datum kolom heeft die in tekst vorm is, moeten de **Range Start** -en **RangeEnd** -para meters worden getransformeerd om het volgende filter te gebruiken. Wijzig in het deel venster **Geavanceerde editor** de volgende tekst door de query toe te voegen om de rijen te filteren op basis van de Range Start-en RangeEnd-para meters:

   ```
   #"Filtered Rows" = Table.SelectRows(#"Expanded Document", each [Document.date] > DateTime.ToText(RangeStart,"yyyy-MM-dd") and [Document.date] < DateTime.ToText(RangeEnd,"yyyy-MM-dd"))
   ```
   
   Afhankelijk van welke kolom en welk gegevens type aanwezig zijn in de bron-gegevensset, kunt u de velden Range Start en RangeEnd dienovereenkomstig wijzigen

   
   |Eigenschap  |Gegevenstype  |Filteren  |
   |---------|---------|---------|
   |_ts     |   Numeric      |  [_ts] > Duration. TotalSeconds (Range Start-#datetime (1970, 1, 1, 0, 0, 0)) en [_ts] < Duration. TotalSeconds (RangeEnd-#datetime (1970, 1, 1, 0, 0, 0))       |
   |Datum (bijvoorbeeld:-2019-08-19)     |   Tekenreeks      | [Document. date] > DateTime. ToText (Range Start, "JJJJ-MM-DD") en [document. date] < DateTime. ToText (RangeEnd, "JJJJ-MM-DD")        |
   |Datum (bijvoorbeeld:-2019-08-11 12:00:00)   |  Tekenreeks       |  [Document. date] > DateTime. ToText (Range Start, "jjjj-mm-dd uu: mm: SS") en [document. date] < DateTime. ToText (RangeEnd, "jjjj-mm-dd uu: mm: SS")       |


1. **Het vernieuwings beleid definiëren** : Definieer het vernieuwings beleid door te navigeren naar het tabblad **Incrementeel vernieuwen** in het **context** menu voor de tabel. Stel het vernieuwings beleid in op Vernieuwen **elke dag** en sla de gegevens van de afgelopen maand op.

   ![Beleid voor vernieuwen definiëren](./media/create-real-time-weather-dashboard-powerbi/define-refresh-policy.png)

   Negeer de waarschuwing die aangeeft dat *de M-query niet kan worden bevestigd om te worden gevouwen*. De Azure Cosmos DB-connector vouwt filter query's uit.

1. **De gegevens laden en de rapporten genereren** -met behulp van de gegevens die u eerder hebt geladen, maakt u de grafieken om te rapporteren over de Tempe ratuur en neer slag.

   ![Gegevens laden en rapport genereren](./media/create-real-time-weather-dashboard-powerbi/load-data-generate-report.png)

1. **Publiceer het rapport naar Power bi Premium** -omdat incrementeel vernieuwen een functie is die alleen Premium is, kan het dialoog venster publiceren alleen een selectie van een werk ruimte met Premium-capaciteit toestaan. De eerste vernieuwing kan langer duren om de historische gegevens te importeren. Volgende gegevens vernieuwingen zijn veel sneller omdat ze incrementeel vernieuwen gebruiken.


## <a name="power-bi-azure-analysis-connector--azure-analysis-services"></a>Power BI Azure Analysis connector + Azure Analysis Services 

### <a name="ingest-weather-data-into-azure-cosmos-db"></a>Weer gegevens opnemen in Azure Cosmos DB 

Stel een opname pijplijn in om [weer gegevens](https://catalog.data.gov/dataset/local-weather-archive) naar Azure Cosmos DB te laden. U kunt een Azure Data Factory-taak (ADF) instellen om periodiek de meest recente weer gegevens in Azure Cosmos DB te laden met behulp van de HTTP-bron en Cosmos DB sink.

### <a name="connect-azure-analysis-services-to-azure-cosmos-account"></a>Azure Analysis Services verbinden met een Azure Cosmos-account

1. **Een nieuw Azure Analysis Services-cluster** - maken[Maak een exemplaar van Azure Analysis Services](../analysis-services/analysis-services-create-server.md) in dezelfde regio als het Azure Cosmos-account en het Databricks-cluster.

1. **Maak een nieuw Analysis Services tabellaire project in Visual Studio** -  [Installeer de SQL Server Data tools (SSDT)](/sql/ssdt/download-sql-server-data-tools-ssdt?view=sql-server-2017) en maak een Analysis Services tabellaire project in Visual Studio.

   ![Azure Analysis Services project maken](./media/create-real-time-weather-dashboard-powerbi/create-analysis-services-project.png)

   Kies het **geïntegreerde werkruimte** exemplaar en het compatibiliteits niveau instellen op **SQL Server 2017/Azure Analysis Services (1400)**

   ![Ontwerper van Azure Analysis Services tabellair model](./media/create-real-time-weather-dashboard-powerbi/tabular-model-designer.png)

1. **Voeg de Azure Cosmos DB gegevens bron toe** : Navigeer naar **model**> **gegevens bronnen** > **nieuwe gegevens bron** en voeg de Azure Cosmos DB gegevens bron toe zoals weer gegeven in de volgende scherm afbeelding:

   ![Cosmos DB gegevens bron toevoegen](./media/create-real-time-weather-dashboard-powerbi/add-data-source.png)

   Maak verbinding met Azure Cosmos DB door de **account-URI**, de naam van de **Data Base**en de **container naam**op te geven. U kunt nu zien dat de gegevens uit de Azure Cosmos-container worden geïmporteerd in Power BI.

   ![Azure Cosmos DB gegevens bekijken](./media/create-real-time-weather-dashboard-powerbi/preview-cosmosdb-data.png)

1. **Het Analysis Services model bouwen** : Open de query-editor en voer de vereiste bewerkingen uit om de geladen gegevensset te optimaliseren:

   * Alleen de kolommen met de weers relatie uitpakken (Tempe ratuur en neer slag)

   * De maand gegevens uit de tabel ophalen. Deze gegevens zijn handig voor het maken van partities zoals beschreven in de volgende sectie.

   * De temperatuur kolommen naar getal converteren

   De resulterende M-expressie is als volgt:

   ```
    let
        Source=#"DocumentDB/https://[ACCOUNTNAME].documents.azure.com:443/",
        #"Expanded Document" = Table.ExpandRecordColumn(Source, "Document", {"id", "_rid", "_self", "_etag", "fogground", "snowfall", "dust", "snowdepth", "mist", "drizzle", "hail", "fastest2minwindspeed", "thunder", "glaze", "snow", "ice", "fog", "temperaturemin", "fastest5secwindspeed", "freezingfog", "temperaturemax", "blowingsnow", "freezingrain", "rain", "highwind", "date", "precipitation", "fogheavy", "smokehaze", "avgwindspeed", "fastest2minwinddir", "fastest5secwinddir", "_attachments", "_ts"}, {"Document.id", "Document._rid", "Document._self", "Document._etag", "Document.fogground", "Document.snowfall", "Document.dust", "Document.snowdepth", "Document.mist", "Document.drizzle", "Document.hail", "Document.fastest2minwindspeed", "Document.thunder", "Document.glaze", "Document.snow", "Document.ice", "Document.fog", "Document.temperaturemin", "Document.fastest5secwindspeed", "Document.freezingfog", "Document.temperaturemax", "Document.blowingsnow", "Document.freezingrain", "Document.rain", "Document.highwind", "Document.date", "Document.precipitation", "Document.fogheavy", "Document.smokehaze", "Document.avgwindspeed", "Document.fastest2minwinddir", "Document.fastest5secwinddir", "Document._attachments", "Document._ts"}),
        #"Select Columns" = Table.SelectColumns(#"Expanded Document",{"Document.temperaturemin", "Document.temperaturemax", "Document.rain", "Document.date"}),
        #"Duplicated Column" = Table.DuplicateColumn(#"Select Columns", "Document.date", "Document.month"),
        #"Extracted First Characters" = Table.TransformColumns(#"Duplicated Column", {{"Document.month", each Text.Start(_, 7), type text}}),
        #"Sorted Rows" = Table.Sort(#"Extracted First Characters",{{"Document.date", Order.Ascending}}),
        #"Changed Type" = Table.TransformColumnTypes(#"Sorted Rows",{{"Document.temperaturemin", type number}, {"Document.temperaturemax", type number}}),
        #"Filtered Rows" = Table.SelectRows(#"Changed Type", each [Document.month] = "2019-07")
    in
        #"Filtered Rows"
   ```

   Wijzig ook het gegevens type van de temperatuur kolommen in decimal om ervoor te zorgen dat deze waarden kunnen worden weer gegeven in Power BI.

1. **Azure-analyse partities maken** : Maak partities in azure Analysis Services om de gegevensset te verdelen in logische partities die onafhankelijk van elkaar kunnen worden vernieuwd en met verschillende frequenties. In dit voor beeld maakt u twee partities die de gegevensset zouden verdelen in de gegevens van de meest recente maand en alle andere.

   ![Analysis Services-partities maken](./media/create-real-time-weather-dashboard-powerbi/create-analysis-services-partitions.png)

   Maak de volgende twee partities in Azure Analysis Services:

   * **Laatste maand** - `#"Filtered Rows" = Table.SelectRows(#"Sorted Rows", each [Document.month] = "2019-07")`
   * **Historische** -  `#"Filtered Rows" = Table.SelectRows(#"Sorted Rows", each [Document.month] <> "2019-07")`

1. **Implementeer het model in Azure analyseserver** -Klik met de rechter muisknop op het Azure Analysis Services project en kies **implementeren**. Voeg de server naam toe in het eigenschappen venster van de **implementatie server** .

   ![Azure Analysis Services model implementeren](./media/create-real-time-weather-dashboard-powerbi/analysis-services-deploy-model.png)

1. **Partitie vernieuwingen en samen voegingen configureren** : Azure Analysis Services kunt u een onafhankelijke verwerking van partities toestaan. Omdat we willen dat de **laatste maand** partitie voortdurend wordt bijgewerkt met de meest recente gegevens, stelt u het Vernieuwings interval in op 5 minuten. Het is niet vereist voor het vernieuwen van de gegevens in de historische partitie. Daarnaast moet u een aantal code schrijven om de laatste maand partitie te consolideren naar de historische partitie en een nieuwe laatste partitie van de maand te maken.


## <a name="connect-power-bi-to-analysis-services"></a>Power BI verbinden met Analysis Services

1. **Verbinding maken met de Azure-analyseserver met behulp van de Azure Analysis Services Data Base-connector** : Kies de **modus Live** en maak verbinding met het Azure Analysis Services-exemplaar, zoals wordt weer gegeven in de volgende scherm afbeelding:

   ![Gegevens ophalen uit Azure Analysis Services](./media/create-real-time-weather-dashboard-powerbi/analysis-services-get-data.png)

1. **De gegevens laden en rapporten genereren** : met behulp van de gegevens die u eerder hebt geladen, maakt u grafieken om te rapporteren over de Tempe ratuur en neer slag. Omdat u een live-verbinding maakt, moeten de query's worden uitgevoerd op de gegevens in het Azure Analysis Services model dat u in de vorige stap hebt geïmplementeerd. De temperatuur grafieken worden binnen vijf minuten na het laden van de nieuwe gegevens in Azure Cosmos DB bijgewerkt.

   ![De gegevens laden en rapporten genereren](./media/create-real-time-weather-dashboard-powerbi/load-data-generate-report.png)

## <a name="next-steps"></a>Volgende stappen

* Zie aan de [slag met Power bi](https://powerbi.microsoft.com/documentation/powerbi-service-get-started/)voor meer informatie over Power bi.

* [Verbind Qlik Sense om uw gegevens te Azure Cosmos DB en te visualiseren](visualize-qlik-sense.md)