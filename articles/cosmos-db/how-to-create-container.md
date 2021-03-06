---
title: Een container maken in Azure Cosmos DB
description: Meer informatie over het maken van een container in Azure Cosmos DB met behulp van Azure Portal, .net, Java, Python, node. js en andere Sdk's.
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 12/02/2019
ms.author: mjbrown
ms.openlocfilehash: 4eaa2974817bfcd8bef83e5139d75a2d4c2ec107
ms.sourcegitcommit: 9405aad7e39efbd8fef6d0a3c8988c6bf8de94eb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/05/2019
ms.locfileid: "74873706"
---
# <a name="create-an-azure-cosmos-container"></a>Een Azure Cosmos-container maken

In dit artikel worden de verschillende manieren beschreven voor het maken van een Azure Cosmos-container (verzameling, tabel of grafiek). U kunt hiervoor Azure Portal, Azure CLI of ondersteunde Sdk's gebruiken. In dit artikel ziet u hoe u een container maakt, de partitiesleutel opgeeft en doorvoer inricht.

## <a name="create-a-container-using-azure-portal"></a>Een container maken met behulp van de Azure-portal

### <a id="portal-sql"></a>SQL-API

1. Meld u aan bij de [Azure-portal](https://portal.azure.com/).

1. [Maak een nieuw Azure Cosmos-account](create-sql-api-dotnet.md#create-account)of selecteer een bestaand account.

1. Open het deel venster **Data Explorer** en selecteer **nieuwe container**. Geef de volgende gegevens op:

   * Geef aan of u een nieuwe database maakt of een bestaande database gebruikt.
   * Voer een container-ID in.
   * Voer een partitiesleutel in.
   * Geef een door Voer op die moet worden ingericht (bijvoorbeeld 1000 RUs).
   * Selecteer **OK**.

    ![Scherm afbeelding van Data Explorer deel venster, met nieuwe container gemarkeerd](./media/how-to-create-container/partitioned-collection-create-sql.png)

### <a id="portal-mongodb"></a>Azure Cosmos DB-API voor MongoDB

1. Meld u aan bij de [Azure-portal](https://portal.azure.com/).

1. [Maak een nieuw Azure Cosmos-account](create-mongodb-dotnet.md#create-a-database-account)of selecteer een bestaand account.

1. Open het deel venster **Data Explorer** en selecteer **nieuwe container**. Geef de volgende gegevens op:

   * Geef aan of u een nieuwe database maakt of een bestaande database gebruikt.
   * Voer een container-ID in.
   * Voer een shardsleutel in.
   * Geef een door Voer op die moet worden ingericht (bijvoorbeeld 1000 RUs).
   * Selecteer **OK**.

    ![Scherm opname van Azure Cosmos DB-API voor MongoDB, container toevoegen dialoog venster](./media/how-to-create-container/partitioned-collection-create-mongodb.png)

### <a id="portal-cassandra"></a>Cassandra-API

1. Meld u aan bij de [Azure-portal](https://portal.azure.com/).

1. [Maak een nieuw Azure Cosmos-account](create-cassandra-dotnet.md#create-a-database-account)of selecteer een bestaand account.

1. Open het deelvenster **Data Explorer** en selecteer **Nieuwe tabel**. Geef de volgende gegevens op:

   * Geef aan of u een nieuwe keyspace maakt of een bestaande keyspace gebruikt.
   * Voer een tabelnaam in.
   * Voer de eigenschappen in en geef een primaire sleutel op.
   * Geef een door Voer op die moet worden ingericht (bijvoorbeeld 1000 RUs).
   * Selecteer **OK**.

    ![Schermopname van Cassandra-API, dialoogvenster Tabel toevoegen](./media/how-to-create-container/partitioned-collection-create-cassandra.png)

> [!NOTE]
> Voor de Cassandra-API wordt de primaire sleutel gebruikt als de partitiesleutel.

### <a id="portal-gremlin"></a>Gremlin-API

1. Meld u aan bij de [Azure-portal](https://portal.azure.com/).

1. [Maak een nieuw Azure Cosmos-account](create-graph-dotnet.md#create-a-database-account)of selecteer een bestaand account.

1. Open het deelvenster **Data Explorer** en selecteer **Nieuwe grafiek**. Geef de volgende gegevens op:

   * Geef aan of u een nieuwe database maakt of een bestaande database gebruikt.
   * Voer een grafiek-ID in.
   * Selecteer **onbeperkte** opslagcapaciteit.
   * Voer een partitiesleutel in voor hoekpunten.
   * Geef een door Voer op die moet worden ingericht (bijvoorbeeld 1000 RUs).
   * Selecteer **OK**.

    ![Schermopname van Gremlin-API, dialoogvenster Grafiek toevoegen](./media/how-to-create-container/partitioned-collection-create-gremlin.png)

### <a id="portal-table"></a>Table-API

1. Meld u aan bij de [Azure-portal](https://portal.azure.com/).

1. [Maak een nieuw Azure Cosmos-account](create-table-dotnet.md#create-a-database-account)of selecteer een bestaand account.

1. Open het deelvenster **Data Explorer** en selecteer **Nieuwe tabel**. Geef de volgende gegevens op:

   * Voer een tabel-ID in.
   * Geef een door Voer op die moet worden ingericht (bijvoorbeeld 1000 RUs).
   * Selecteer **OK**.

    ![Schermopname van Table-API, dialoogvenster Grafiek toevoegen](./media/how-to-create-container/partitioned-collection-create-table.png)

> [!Note]
> Voor de Table-API wordt de partitiesleutel opgegeven telkens wanneer u een nieuwe rij toevoegt.

## Een container maken met behulp van Azure CLI<a id="cli-sql"></a><a id="cli-mongodb"></a><a id="cli-cassandra"></a><a id="cli-gremlin"></a><a id="cli-table"></a>

De onderstaande koppelingen laten zien hoe u container bronnen voor Azure Cosmos DB maakt met behulp van Azure CLI.

Voor een lijst met alle Azure CLI-voor beelden voor alle Azure Cosmos DB Api's raadpleegt u, [SQL API](cli-samples.md), [CASSANDRA-API](cli-samples-cassandra.md), [MongoDb api](cli-samples-mongodb.md), [Gremlin API](cli-samples-gremlin.md)en [Table-API](cli-samples-table.md)

* [Een container maken met Azure CLI](manage-with-cli.md#create-a-container)
* [Een verzameling voor Azure Cosmos DB voor MongoDB-API maken met Azure CLI](./scripts/cli/mongodb/create.md)
* [Een Cassandra-tabel maken met Azure CLI](./scripts/cli/cassandra/create.md)
* [Een Gremlin-grafiek maken met Azure CLI](./scripts/cli/gremlin/create.md)
* [Een Table-API tabel maken met Azure CLI](./scripts/cli/table/create.md)

## Een container maken met behulp van Power shell<a id="ps-sql"> </a> -<a id="ps-mongodb"><a id="ps-cassandra"></a><a id="ps-gremlin"><a id="ps-table"></a>

De onderstaande koppelingen laten zien hoe u container resources voor Azure Cosmos DB maakt met behulp van Power shell.

Voor een lijst met alle Azure CLI-voor beelden voor alle Azure Cosmos DB Api's raadpleegt u, [SQL API](powershell-samples-sql.md), [CASSANDRA-API](powershell-samples-cassandra.md), [MongoDb api](powershell-samples-mongodb.md), [Gremlin API](powershell-samples-gremlin.md)en [Table-API](powershell-samples-table.md)

* [Een container maken met Power shell](manage-with-powershell.md#create-container)
* [Een verzameling maken voor Azure Cosmos DB voor MongoDB-API met Power shell](./scripts/powershell/mongodb/ps-mongodb-create.md)
* [Een Cassandra-tabel maken met Power shell](./scripts/powershell/cassandra/ps-cassandra-create.md)
* [Een Gremlin-grafiek maken met Power shell](./scripts/powershell/gremlin/ps-gremlin-create.md)
* [Een Table-API tabel maken met Power shell](./scripts/powershell/table/ps-table-create.md)

## <a name="create-a-container-using-net-sdk"></a>Een container maken met behulp van .NET SDK

### <a id="dotnet-sql-graph"></a>SQL-API en Gremlin-API

```csharp
// Create a container with a partition key and provision 1000 RU/s throughput.
DocumentCollection myCollection = new DocumentCollection();
myCollection.Id = "myContainerName";
myCollection.PartitionKey.Paths.Add("/myPartitionKey");

await client.CreateDocumentCollectionAsync(
    UriFactory.CreateDatabaseUri("myDatabaseName"),
    myCollection,
    new RequestOptions { OfferThroughput = 1000 });
```

### <a id="dotnet-mongodb"></a>Azure Cosmos DB-API voor MongoDB

```csharp
// Create a collection with a partition key by using Mongo Shell:
db.runCommand( { shardCollection: "myDatabase.myCollection", key: { myShardKey: "hashed" } } )
```

> [!Note]
> Het MongoDB-draad protocol begrijpt het concept van [aanvraag eenheden](request-units.md)niet. Als u een nieuwe verzameling wilt maken met een ingerichte door Voer, gebruikt u de Azure Portal-of Cosmos DB Sdk's voor SQL API.

### <a id="dotnet-cassandra"></a>Cassandra-API

```csharp
// Create a Cassandra table with a partition/primary key and provision 1000 RU/s throughput.
session.Execute(CREATE TABLE myKeySpace.myTable(
    user_id int PRIMARY KEY,
    firstName text,
    lastName text) WITH cosmosdb_provisioned_throughput=1000);
```

## <a name="next-steps"></a>Volgende stappen

* [Partitionering in Azure Cosmos DB](partitioning-overview.md)
* [Aanvraageenheden in Azure Cosmos DB](request-units.md)
* [Doorvoer voor containers en databases inrichten](set-throughput.md)
* [Werken met een Azure Cosmos-account](account-overview.md)
