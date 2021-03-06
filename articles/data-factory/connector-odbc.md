---
title: Gegevens kopiëren van ODBC-bronnen met behulp van Azure Data Factory
description: Informatie over het kopiëren van gegevens van OData-bronnen naar ondersteunde Sink-gegevens archieven met behulp van een Kopieer activiteit in een Azure Data Factory-pijp lijn.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: shwang
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 09/04/2019
ms.author: jingwang
ms.openlocfilehash: b94dbb81b2ab5b7e4421357ee81d6c3ea8e8d3c0
ms.sourcegitcommit: a5ebf5026d9967c4c4f92432698cb1f8651c03bb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/08/2019
ms.locfileid: "74912488"
---
# <a name="copy-data-from-and-to-odbc-data-stores-using-azure-data-factory"></a>Gegevens kopiëren van en naar ODBC-gegevens archieven met behulp van Azure Data Factory
> [!div class="op_single_selector" title1="Selecteer de versie van Data Factory service die u gebruikt:"]
> * [Versie 1](v1/data-factory-odbc-connector.md)
> * [Huidige versie](connector-odbc.md)

In dit artikel wordt beschreven hoe u de Kopieer activiteit in Azure Data Factory kunt gebruiken om gegevens te kopiëren van en naar een ODBC-gegevens archief. Dit is gebaseerd op de [overzicht kopieeractiviteit](copy-activity-overview.md) artikel met daarin een algemeen overzicht van de kopieeractiviteit.

## <a name="supported-capabilities"></a>Ondersteunde mogelijkheden

Deze ODBC-Connector wordt ondersteund voor de volgende activiteiten:

- [Kopieer activiteit](copy-activity-overview.md) met een [ondersteunde bron/Sink-matrix](copy-activity-overview.md)
- [Activiteit Lookup](control-flow-lookup-activity.md)

U kunt gegevens van een ODBC-bron naar een ondersteunde Sink-gegevens opslag kopiëren of vanuit elk ondersteund bron gegevens archief naar een ODBC-Sink kopiëren. Zie voor een lijst met gegevensarchieven die worden ondersteund als bronnen/put door de kopieeractiviteit, de [ondersteunde gegevensarchieven](copy-activity-overview.md#supported-data-stores-and-formats) tabel.

Met name deze ODBC-connector ondersteunt het kopiëren van gegevens van/naar **elk ODBC-compatibel gegevens archief** met behulp van **basis** -of **anonieme** verificatie.

## <a name="prerequisites"></a>Vereisten

Als u deze ODBC-Connector wilt gebruiken, moet u het volgende doen:

- Stel een zelf-hostende Integration Runtime in. Zie [zelfgehoste Cloudintegratieruntime](create-self-hosted-integration-runtime.md) artikel voor meer informatie.
- Installeer het ODBC-stuur programma voor het gegevens archief op de Integration Runtime computer.

## <a name="getting-started"></a>Aan de slag

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

De volgende secties bevatten informatie over eigenschappen die worden gebruikt voor het definiëren van Data Factory entiteiten die specifiek zijn voor ODBC-Connector.

## <a name="linked-service-properties"></a>Eigenschappen van de gekoppelde service

De volgende eigenschappen worden ondersteund voor ODBC-gekoppelde services:

| Eigenschap | Beschrijving | Verplicht |
|:--- |:--- |:--- |
| type | De eigenschap type moet worden ingesteld op: **ODBC** | Ja |
| connectionString | De connection string het referentie gedeelte niet uitsluiten. U kunt de connection string met een patroon als `"Driver={SQL Server};Server=Server.database.windows.net; Database=TestDatabase;"`opgeven, of de systeem-DSN (gegevens bron naam) gebruiken die u op de Integration Runtime machine hebt ingesteld met `"DSN=<name of the DSN on IR machine>;"` (u moet nog steeds het referentie deel opgeven in de gekoppelde service).<br>Markeer dit veld als een SecureString om het veilig op te slaan in Data Factory, of [verwijs naar een geheim dat is opgeslagen in Azure Key Vault](store-credentials-in-key-vault.md).| Ja |
| authenticationType | Type verificatie dat wordt gebruikt om verbinding te maken met het ODBC-gegevens archief.<br/>Toegestane waarden zijn: **Basic** en **Anonymous**. | Ja |
| userName | Geef de gebruikers naam op als u basis verificatie gebruikt. | Nee |
| wachtwoord | Geeft het wachtwoord op voor het gebruikersaccount dat u hebt opgegeven voor de userName. Markeer dit veld als een SecureString om het veilig op te slaan in Data Factory, of [verwijs naar een geheim dat is opgeslagen in Azure Key Vault](store-credentials-in-key-vault.md). | Nee |
| referenties | Het deel van de toegangs referentie van de connection string dat is opgegeven in de eigenschaps waarde-indeling van het stuur programma. Voorbeeld: `"RefreshToken=<secret refresh token>;"`. Dit veld markeren als SecureString. | Nee |
| connectVia | De [Integration Runtime](concepts-integration-runtime.md) moet worden gebruikt verbinding maken met het gegevensarchief. Een zelf-hostende Integration Runtime is vereist zoals vermeld in de [vereisten](#prerequisites). |Ja |

**Voor beeld 1: basis verificatie gebruiken**

```json
{
    "name": "ODBCLinkedService",
    "properties": {
        "type": "Odbc",
        "typeProperties": {
            "connectionString": {
                "type": "SecureString",
                "value": "<connection string>"
            },
            "authenticationType": "Basic",
            "userName": "<username>",
            "password": {
                "type": "SecureString",
                "value": "<password>"
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

**Voor beeld 2: anonieme verificatie gebruiken**

```json
{
    "name": "ODBCLinkedService",
    "properties": {
        "type": "Odbc",
        "typeProperties": {
            "connectionString": {
                "type": "SecureString",
                "value": "<connection string>"
            },
            "authenticationType": "Anonymous",
            "credential": {
                "type": "SecureString",
                "value": "RefreshToken=<secret refresh token>;"
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

## <a name="dataset-properties"></a>Eigenschappen van gegevensset

Zie voor een volledige lijst van de secties en eigenschappen die beschikbaar zijn voor het definiëren van gegevenssets, de [gegevenssets](concepts-datasets-linked-services.md) artikel. In deze sectie vindt u een lijst met eigenschappen die door de ODBC-gegevensset worden ondersteund.

Als u gegevens wilt kopiëren van/naar een ODBC-compatibel gegevens archief, worden de volgende eigenschappen ondersteund:

| Eigenschap | Beschrijving | Verplicht |
|:--- |:--- |:--- |
| type | De eigenschap type van de gegevensset moet worden ingesteld op: **OdbcTable** | Ja |
| tableName | De naam van de tabel in het ODBC-gegevens archief. | Nee voor bron (als "query" in activiteits bron is opgegeven);<br/>Ja voor Sink |

**Voorbeeld**

```json
{
    "name": "ODBCDataset",
    "properties": {
        "type": "OdbcTable",
        "schema": [],
        "linkedServiceName": {
            "referenceName": "<ODBC linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "tableName": "<table name>"
        }
    }
}
```

Als u `RelationalTable` getypte gegevensset gebruikt, wordt deze nog steeds ondersteund als-is, terwijl u wordt geadviseerd om het nieuwe item te gebruiken.

## <a name="copy-activity-properties"></a>Eigenschappen van de kopieeractiviteit

Zie voor een volledige lijst van de secties en eigenschappen die beschikbaar zijn voor het definiëren van activiteiten, de [pijplijnen](concepts-pipelines-activities.md) artikel. In deze sectie vindt u een lijst met eigenschappen die door de ODBC-bron worden ondersteund.

### <a name="odbc-as-source"></a>ODBC als bron

Als u gegevens wilt kopiëren vanuit een ODBC-compatibel gegevens archief, worden de volgende eigenschappen ondersteund in de sectie **bron** van de Kopieer activiteit:

| Eigenschap | Beschrijving | Verplicht |
|:--- |:--- |:--- |
| type | De eigenschap type van de bron van de Kopieer activiteit moet zijn ingesteld op: **OdbcSource** | Ja |
| query | Gebruik de aangepaste SQL-query om gegevens te lezen. Bijvoorbeeld: `"SELECT * FROM MyTable"`. | Nee (als de 'tableName' in de gegevensset is opgegeven) |

**Voorbeeld:**

```json
"activities":[
    {
        "name": "CopyFromODBC",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<ODBC input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "OdbcSource",
                "query": "SELECT * FROM MyTable"
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

Als u `RelationalSource` getypte bron gebruikt, wordt deze nog steeds ondersteund als-is, terwijl u wordt geadviseerd om het nieuwe item te gebruiken.

### <a name="odbc-as-sink"></a>ODBC als Sink

Als u gegevens wilt kopiëren naar een ODBC-compatibel gegevens archief, stelt u het sink-type in de Kopieer activiteit in op **OdbcSink**. De volgende eigenschappen worden ondersteund in de kopieeractiviteit **sink** sectie:

| Eigenschap | Beschrijving | Verplicht |
|:--- |:--- |:--- |
| type | De eigenschap type van de Sink voor kopieer activiteiten moet worden ingesteld op: **OdbcSink** | Ja |
| writeBatchTimeout |Wacht tijd voordat de batch INSERT-bewerking is voltooid voordat er een time-out optreedt.<br/>Toegestane waarden zijn: time span. Voorbeeld: "00: 30:00 ' (30 minuten). |Nee |
| writeBatchSize |Hiermee worden gegevens in de SQL-tabel ingevoegd wanneer de buffer grootte writeBatchSize bereikt.<br/>Toegestane waarden zijn: geheel getal (aantal rijen). |Nee (standaard is 0-automatisch gedetecteerd) |
| preCopyScript |Geef een SQL-query voor de Kopieer activiteit op die moet worden uitgevoerd voordat u gegevens naar het gegevens archief in elke run schrijft. U kunt deze eigenschap gebruiken om de vooraf geladen gegevens op te schonen. |Nee |

> [!NOTE]
> Als het ' writeBatchSize ' niet is ingesteld (automatisch gedetecteerd), detecteert de Kopieer activiteit eerst of het stuur programma batch bewerkingen ondersteunt, en stelt dit in 10000 op 1 als dit niet het geval is. Als u de waarde expliciet instelt op 0, wordt door de Kopieer activiteit de waarde nageleefd en mislukt de runtime als het stuur programma geen batch bewerkingen ondersteunt.

**Voorbeeld:**

```json
"activities":[
    {
        "name": "CopyToODBC",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<ODBC output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "<source type>"
            },
            "sink": {
                "type": "OdbcSink",
                "writeBatchSize": 100000
            }
        }
    }
]
```

## <a name="sap-hana-sink"></a>SAP HANA Sink

>[!NOTE]
>Als u gegevens wilt kopiëren uit SAP HANA gegevens archief, raadpleegt u de systeem eigen [SAP Hana-connector](connector-sap-hana.md). Als u gegevens wilt kopiëren naar SAP HANA, volgt u deze instructie voor het gebruik van ODBC-Connector. Houd er rekening mee dat de gekoppelde services voor SAP HANA connector en ODBC-Connector met een ander type dan niet opnieuw kunnen worden gebruikt.
>

U kunt gegevens kopiëren naar SAP HANA-data base met behulp van de algemene ODBC-Connector.

Stel een zelf-hostende Integration Runtime in op een computer met toegang tot uw gegevens archief. De Integration Runtime gebruikt het ODBC-stuur programma voor SAP HANA om verbinding te maken met het gegevens archief. Installeer daarom het stuur programma als dit nog niet op dezelfde computer is geïnstalleerd. Zie de sectie [vereisten](#prerequisites) voor meer informatie.

Voordat u de SAP HANA sink in een Data Factory oplossing gebruikt, controleert u of de Integration Runtime verbinding kan maken met het gegevens archief met behulp van instructies in het gedeelte [problemen met de verbinding oplossen](#troubleshoot-connectivity-issues) .

Maak een gekoppelde ODBC-service om een SAP HANA gegevens archief te koppelen aan een Azure-data factory zoals wordt weer gegeven in het volgende voor beeld:

```json
{
    "name": "SAPHANAViaODBCLinkedService",
    "properties": {
        "type": "Odbc",
        "typeProperties": {
            "connectionString": {
                "type": "SecureString",
                "value": "Driver={HDBODBC};servernode=<HANA server>.clouddatahub-int.net:30015"
            },
            "authenticationType": "Basic",
            "userName": "<username>",
            "password": {
                "type": "SecureString",
                "value": "<password>"
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

Lees het artikel van het begin voor een gedetailleerd overzicht van het gebruik van ODBC-gegevens archieven als bron/Sink-gegevens opslag in een Kopieer bewerking.

## <a name="lookup-activity-properties"></a>Eigenschappen van opzoek activiteit

Controleer de [opzoek activiteit](control-flow-lookup-activity.md)voor meer informatie over de eigenschappen.


## <a name="troubleshoot-connectivity-issues"></a>Verbindings problemen oplossen

Gebruik het tabblad **Diagnostische gegevens** van **Integration Runtime Configuration Manager**om verbindings problemen op te lossen.

1. Start **Integration Runtime Configuration Manager**.
2. Schakel over naar het tabblad **Diagnostische gegevens** .
3. Selecteer in de sectie ' verbinding testen ' het **type** gegevens archief (gekoppelde service).
4. Geef de **Connection String** op die wordt gebruikt om verbinding te maken met het gegevens archief, kies de **verificatie** en voer de **gebruikers naam**, het **wacht woord**en/of de **referenties**in.
5. Klik op **verbinding testen** om de verbinding met het gegevens archief te testen.

## <a name="next-steps"></a>Volgende stappen
Zie voor een lijst met gegevensarchieven die worden ondersteund als bronnen en sinks door de kopieeractiviteit in Azure Data Factory, [ondersteunde gegevensarchieven](copy-activity-overview.md##supported-data-stores-and-formats).
