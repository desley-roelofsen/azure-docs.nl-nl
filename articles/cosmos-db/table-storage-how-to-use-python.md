---
title: Aan de slag met Azure Table Storage en de Azure Cosmos DB Table-API met behulp van Python
description: Sla gestructureerde gegevens op in de cloud met Azure-tabelopslag of de Azure Cosmos DB Table-API.
ms.service: cosmos-db
ms.subservice: cosmosdb-table
ms.devlang: python
ms.topic: sample
ms.date: 04/05/2018
author: wmengmsft
ms.author: wmeng
ms.reviewer: sngun
ms.openlocfilehash: 883965d1d59e5523527a6aab1e83521d7491bf82
ms.sourcegitcommit: 9a4296c56beca63430fcc8f92e453b2ab068cc62
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/20/2019
ms.locfileid: "72675702"
---
# <a name="get-started-with-azure-table-storage-and-the-azure-cosmos-db-table-api-using-python"></a>Aan de slag met Azure Table Storage en de Azure Cosmos DB Table-API met behulp van Python

[!INCLUDE [storage-selector-table-include](../../includes/storage-selector-table-include.md)]
[!INCLUDE [storage-table-applies-to-storagetable-and-cosmos](../../includes/storage-table-applies-to-storagetable-and-cosmos.md)]

Azure Table Storage en Azure Cosmos DB zijn services die gestructureerde NoSQL-gegevens opslaan in de cloud en zo een sleutel/kenmerkarchiefontwerp zonder schema bieden. Omdat Table Storage en Azure Cosmos DB schemaloos zijn, kunt u uw gegevens eenvoudig aanpassen naarmate de behoeften van uw toepassing veranderen. Toegang tot Table Storage- en Table API-gegevens is snel en kostenefficiënt voor veel soorten toepassingen en doorgaans goedkoper dan traditionele SQL voor vergelijkbare gegevensvolumes.

U kunt Table Storage of Azure Cosmos DB gebruiken voor de opslag van flexibele gegevenssets, zoals gebruikersgegevens voor webtoepassingen, adresboeken, apparaatgegevens of andere soorten metagegevens die uw service nodig heeft. In elke tabel kunt u een willekeurig aantal entiteiten opslaan. Een opslagaccount kan een onbeperkt aantal tabellen bevatten, tot de maximale capaciteit van het opslagaccount.

### <a name="about-this-sample"></a>Over dit voorbeeld
In dit voorbeeld ziet u hoe u de [Azure Cosmos DB Table SDK voor Python](https://pypi.python.org/pypi/azure-cosmosdb-table/) in algemene scenario's voor Azure Table-opslag gebruikt. De naam van de SDK geeft aan dat deze bedoeld is voor gebruik met Azure Cosmos DB, maar hij werkt met zowel Azure Cosmos DB- als Azure Tables-opslag. Alleen heeft elke service een uniek eindpunt. Deze scenario's worden doorgenomen met behulp van Python-voorbeelden die laten zien hoe u het volgende doet:
* Tabellen maken en verwijderen
* Entiteiten invoegen en query's erop uitvoeren
* Entiteiten wijzigen

Tijdens het werken met de scenario's in dit voorbeeld kunt u het [referentiemateriaal bij de Azure Cosmos DB SDK voor Python-API](https://docs.microsoft.com/python/api/overview/azure/cosmosdb?view=azure-python) raadplegen.

## <a name="prerequisites"></a>Vereisten

U hebt het volgende nodig voor dit voorbeeld:

- [Python](https://www.python.org/downloads/) 2.7, 3.3, 3.4, 3.5 of 3.6
- [Azure Cosmos DB Table SDK voor Python](https://pypi.python.org/pypi/azure-cosmosdb-table/). Deze SDK is verbonden met zowel Azure Table Storage als de Azure Cosmos DB Table-API.
- Een [Azure Storage-account](../storage/common/storage-quickstart-create-account.md) of [Azure Cosmos DB-account](https://azure.microsoft.com/try/cosmosdb/)

## <a name="create-an-azure-service-account"></a>Een Azure-serviceaccount maken
[!INCLUDE [cosmos-db-create-azure-service-account](../../includes/cosmos-db-create-azure-service-account.md)]

### <a name="create-an-azure-storage-account"></a>Een Azure-opslagaccount maken
[!INCLUDE [cosmos-db-create-storage-account](../../includes/cosmos-db-create-storage-account.md)]

### <a name="create-an-azure-cosmos-db-table-api-account"></a>Een Azure Cosmos DB Table-API-account maken
[!INCLUDE [cosmos-db-create-tableapi-account](../../includes/cosmos-db-create-tableapi-account.md)]

## <a name="install-the-azure-cosmos-db-table-sdk-for-python"></a>De Azure Cosmos DB Table SDK voor Python installeren

Nadat u een opslagaccount hebt gemaakt, gaat u de [Microsoft Azure Cosmos DB Table SDK voor Python](https://pypi.python.org/pypi/azure-cosmosdb-table/) installeren. Raadpleeg voor meer informatie over het installeren van de SDK het bestand [README.rst](https://github.com/Azure/azure-cosmosdb-python/blob/master/azure-cosmosdb-table/README.rst) in de Cosmos DB Table SDK voor de Python-opslagplaats op GitHub.

## <a name="import-the-tableservice-and-entity-classes"></a>De TableService- en Entity-klassen importeren

Als u wilt werken met entiteiten in de Azure Table service in Python, gebruikt u de klassen [TableService][py_TableService] en [entity][py_Entity] . Voeg deze code aan het begin van uw Python-bestand toe om beide te importeren:

```python
from azure.cosmosdb.table.tableservice import TableService
from azure.cosmosdb.table.models import Entity
```

## <a name="connect-to-azure-table-service"></a>Verbinding maken met Azure Table-service

Voor verbinding met Azure Storage Table-service maakt u een [TableService][py_TableService]-object en geeft u uw Storage-accountnaam en accountsleutel door. Vervang `myaccount` en `mykey` door uw accountnaam en -sleutel.

```python
table_service = TableService(account_name='myaccount', account_key='mykey')
```

## <a name="connect-to-azure-cosmos-db"></a>Verbinding maken met Azure Cosmos DB

Wanneer u verbinding wilt maken met Azure Cosmos DB, kopieert u de primaire verbindingsreeks vanuit Azure Portal en maakt u een [TableService][py_TableService]-object met de gekopieerde verbindingsreeks:

```python
table_service = TableService(connection_string='DefaultEndpointsProtocol=https;AccountName=myaccount;AccountKey=mykey;TableEndpoint=myendpoint;')
```

## <a name="create-a-table"></a>Een tabel maken

Roep [create_table][py_create_table] aan om de tabel te maken.

```python
table_service.create_table('tasktable')
```

## <a name="add-an-entity-to-a-table"></a>Een entiteit toevoegen aan een tabel

Als u een entiteit wilt toevoegen, maakt u eerst een-object dat uw entiteit vertegenwoordigt. vervolgens geeft u het object door aan de [methode TableService. insert_entity][py_TableService]. Het entiteits object kan een woorden lijst of een object van het type [entity][py_Entity]zijn en definieert de eigenschaps namen en waarden van uw entiteit. Elke entiteit moet de vereiste [PartitionKey- en RowKey](#partitionkey-and-rowkey)-eigenschappen bevatten, naast de andere eigenschappen die u voor de entiteit definieert.

In dit voor beeld wordt een Dictionary-object gemaakt dat een entiteit vertegenwoordigt, waarna het wordt door gegeven aan de methode [insert_entity][py_insert_entity] om deze aan de tabel toe te voegen:

```python
task = {'PartitionKey': 'tasksSeattle', 'RowKey': '001',
        'description': 'Take out the trash', 'priority': 200}
table_service.insert_entity('tasktable', task)
```

In dit voor beeld wordt een [entiteits][py_Entity] object gemaakt en vervolgens door gegeven aan de methode [insert_entity][py_insert_entity] om dit aan de tabel toe te voegen:

```python
task = Entity()
task.PartitionKey = 'tasksSeattle'
task.RowKey = '002'
task.description = 'Wash the car'
task.priority = 100
table_service.insert_entity('tasktable', task)
```

### <a name="partitionkey-and-rowkey"></a>PartitionKey en RowKey

U moet zowel een **PartitionKey**- als een **RowKey**-eigenschap opgeven voor elke entiteit. Dit zijn de unieke id's van uw entiteiten, aangezien ze samen de primaire sleutel van een entiteit vormen. U kunt met deze waarden veel sneller een query uitvoeren dan een query met andere entiteitseigenschappen, omdat alleen deze eigenschappen zijn geïndexeerd.

De Table-service gebruikt **PartitionKey** voor intelligente distributie van tabelentiteiten op opslagknooppunten. Entiteiten met dezelfde **PartitionKey** worden opgeslagen op hetzelfde knooppunt. **RowKey** is de unieke id van de entiteit binnen de partitie waar deze bij hoort.

## <a name="update-an-entity"></a>Een entiteit bijwerken

Als u alle eigenschaps waarden van een entiteit wilt bijwerken, roept u de methode [update_entity][py_update_entity] aan. In dit voorbeeld ziet u hoe u een bestaande entiteit vervangt door een bijgewerkte versie:

```python
task = {'PartitionKey': 'tasksSeattle', 'RowKey': '001',
        'description': 'Take out the garbage', 'priority': 250}
table_service.update_entity('tasktable', task)
```

Als de entiteit die wordt bijgewerkt, niet al bestaat, mislukt het de updatebewerking. Als u een entiteit wilt opslaan, ongeacht of deze bestaat, gebruikt u [insert_or_replace_entity][py_insert_or_replace_entity]. In het volgende voorbeeld vervangt de eerste aanroep van de bestaande entiteit. Bij de tweede aanroep wordt een nieuwe entiteit ingevoegd, omdat er geen entiteit met de opgegeven PartitionKey en RowKey bestaat in de tabel.

```python
# Replace the entity created earlier
task = {'PartitionKey': 'tasksSeattle', 'RowKey': '001',
        'description': 'Take out the garbage again', 'priority': 250}
table_service.insert_or_replace_entity('tasktable', task)

# Insert a new entity
task = {'PartitionKey': 'tasksSeattle', 'RowKey': '003',
        'description': 'Buy detergent', 'priority': 300}
table_service.insert_or_replace_entity('tasktable', task)
```

> [!TIP]
> De methode [update_entity][py_update_entity] vervangt alle eigenschappen en waarden van een bestaande entiteit, die u ook kunt gebruiken om eigenschappen van een bestaande entiteit te verwijderen. U kunt de [merge_entity][py_merge_entity] -methode gebruiken om een bestaande entiteit bij te werken met nieuwe of gewijzigde eigenschaps waarden zonder de entiteit volledig te vervangen.

## <a name="modify-multiple-entities"></a>Meerdere entiteiten wijzigen

Om te zorgen dat een aanvraag atomisch door de Table-service wordt verwerkt, kunt u meerdere bewerkingen in een batch verzenden. Gebruik eerst de [TableBatch][py_TableBatch] -klasse om meerdere bewerkingen aan één batch toe te voegen. Roep vervolgens [TableService][py_TableService]aan. [commit_batch][py_commit_batch] om de bewerkingen in een Atomic-bewerking te verzenden. Alle te wijzigen entiteiten in de batch, moeten zich in dezelfde partitie bevinden.

In dit voorbeeld worden twee entiteiten samen aan een batch toegevoegd:

```python
from azure.cosmosdb.table.tablebatch import TableBatch
batch = TableBatch()
task004 = {'PartitionKey': 'tasksSeattle', 'RowKey': '004',
           'description': 'Go grocery shopping', 'priority': 400}
task005 = {'PartitionKey': 'tasksSeattle', 'RowKey': '005',
           'description': 'Clean the bathroom', 'priority': 100}
batch.insert_entity(task004)
batch.insert_entity(task005)
table_service.commit_batch('tasktable', batch)
```

Batches kunnen ook worden gebruikt met de syntaxis van de contextmanager:

```python
task006 = {'PartitionKey': 'tasksSeattle', 'RowKey': '006',
           'description': 'Go grocery shopping', 'priority': 400}
task007 = {'PartitionKey': 'tasksSeattle', 'RowKey': '007',
           'description': 'Clean the bathroom', 'priority': 100}

with table_service.batch('tasktable') as batch:
    batch.insert_entity(task006)
    batch.insert_entity(task007)
```

## <a name="query-for-an-entity"></a>Een query uitvoeren voor een entiteit

Als u een query wilt uitvoeren voor een entiteit in een tabel, geeft u de PartitionKey en RowKey door aan de [TableService][py_TableService]. methode [get_entity][py_get_entity] .

```python
task = table_service.get_entity('tasktable', 'tasksSeattle', '001')
print(task.description)
print(task.priority)
```

## <a name="query-a-set-of-entities"></a>Een query uitvoeren voor een aantal entiteiten

U kunt een query voor een aantal entiteiten uitvoeren door een filtertekenreeks op te geven met de **filter**-parameter. In dit voorbeeld worden alle taken in Haarlem gevonden door een filter toepassen op PartitionKey:

```python
tasks = table_service.query_entities(
    'tasktable', filter="PartitionKey eq 'tasksSeattle'")
for task in tasks:
    print(task.description)
    print(task.priority)
```

## <a name="query-a-subset-of-entity-properties"></a>Een query uitvoeren op een subset van entiteitseigenschappen

U kunt ook beperken welke eigenschappen voor elke entiteit in een query worden geretourneerd. Deze methode, *projectie* genoemd, verbruikt minder bandbreedte en kan de queryprestaties verbeteren, vooral bij grote entiteiten or resultatensets. Gebruik de parameter **select** en geef de namen door van de eigenschappen die u wilt retourneren naar de client.

De query in de volgende code retourneert alleen de beschrijvingen van de entiteiten in de tabel.

> [!NOTE]
> Het volgende fragment werkt alleen op basis van de Azure Storage. Dit wordt niet ondersteund door de opslagemulator.

```python
tasks = table_service.query_entities(
    'tasktable', filter="PartitionKey eq 'tasksSeattle'", select='description')
for task in tasks:
    print(task.description)
```

## <a name="delete-an-entity"></a>Een entiteit verwijderen

Verwijder een entiteit door de **PartitionKey** en **RowKey** door te geven aan de methode [delete_entity][py_delete_entity] .

```python
table_service.delete_entity('tasktable', 'tasksSeattle', '001')
```

## <a name="delete-a-table"></a>Een tabel verwijderen

Als u een tabel of een van de entiteiten niet meer nodig hebt, roept u de [delete_table][py_delete_table] -methode op om de tabel permanent uit Azure Storage te verwijderen.

```python
table_service.delete_table('tasktable')
```

## <a name="next-steps"></a>Volgende stappen

* [Veelgestelde vragen - Ontwikkelen met de Table-API](https://docs.microsoft.com/azure/cosmos-db/faq)
* [Referentiemateriaal voor Azure Cosmos DB SDK voor Python-API](https://docs.microsoft.com/python/api/overview/azure/cosmosdb?view=azure-python)
* [Python Developer Center](https://azure.microsoft.com/develop/python/)
* [Microsoft Azure Storage Explorer](../vs-azure-tools-storage-manage-with-storage-explorer.md): een gratis, platformoverschrijdende-toepassing voor visueel werken met Azure Storage-gegevens op Windows, Mac OS en Linux.
* [Werken met Python in Visual Studio (Windows)](https://docs.microsoft.com/visualstudio/python/overview-of-python-tools-for-visual-studio)


[py_commit_batch]: https://docs.microsoft.com/python/api/azure-cosmosdb-table/azure.cosmosdb.table.tableservice.tableservice?view=azure-python
[py_create_table]: https://docs.microsoft.com/python/api/azure-cosmosdb-table/azure.cosmosdb.table.tableservice.tableservice?view=azure-python
[py_delete_entity]: https://docs.microsoft.com/python/api/azure-cosmosdb-table/azure.cosmosdb.table.tableservice.tableservice?view=azure-python
[py_get_entity]: https://docs.microsoft.com/python/api/azure-cosmosdb-table/azure.cosmosdb.table.tableservice.tableservice?view=azure-python
[py_insert_entity]: https://docs.microsoft.com/python/api/azure-cosmosdb-table/azure.cosmosdb.table.tableservice.tableservice?view=azure-python
[py_insert_or_replace_entity]: https://docs.microsoft.com/python/api/azure-cosmosdb-table/azure.cosmosdb.table.tableservice.tableservice?view=azure-python
[py_Entity]: https://docs.microsoft.com/python/api/azure-cosmosdb-table/azure.cosmosdb.table.models.entity?view=azure-python
[py_merge_entity]: https://docs.microsoft.com/python/api/azure-cosmosdb-table/azure.cosmosdb.table.tableservice.tableservice?view=azure-python
[py_update_entity]: https://docs.microsoft.com/python/api/azure-cosmosdb-table/azure.cosmosdb.table.tableservice.tableservice?view=azure-python
[py_delete_table]: https://docs.microsoft.com/python/api/azure-cosmosdb-table/azure.cosmosdb.table.tableservice.tableservice?view=azure-python
[py_TableService]: https://docs.microsoft.com/python/api/azure-cosmosdb-table/azure.cosmosdb.table.tableservice.tableservice?view=azure-python
[py_TableBatch]: https://docs.microsoft.com/python/api/azure-cosmosdb-table/azure.cosmosdb.table.tableservice.tableservice?view=azure-python
