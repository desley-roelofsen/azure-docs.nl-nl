---
title: Indexerings beleid in Azure Cosmos DB beheren
description: Meer informatie over het beheren van indexerings beleid, het opnemen of uitsluiten van een eigenschap van indexeren, het definiëren van indexering met verschillende Azure Cosmos DB Sdk's
author: ThomasWeiss
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 12/02/2019
ms.author: thweiss
ms.openlocfilehash: 3b98975df194af4625087e1beb556efb2a347f43
ms.sourcegitcommit: 9405aad7e39efbd8fef6d0a3c8988c6bf8de94eb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/05/2019
ms.locfileid: "74872057"
---
# <a name="manage-indexing-policies-in-azure-cosmos-db"></a>Indexerings beleid in Azure Cosmos DB beheren

In Azure Cosmos DB worden gegevens geïndexeerd na het [indexerings beleid](index-policy.md) dat voor elke container is gedefinieerd. Het standaard indexerings beleid voor nieuw gemaakte containers dwingt bereik indexen af voor een wille keurige teken reeks of getal. Dit beleid kan worden overschreven met uw eigen aangepaste indexerings beleid.

## <a name="indexing-policy-examples"></a>Voor beelden van Indexing-beleid

Hier volgen enkele voor beelden van beleids regels voor indexering die worden weer gegeven in de JSON-indeling. Dit is de manier waarop deze worden weer gegeven op de Azure Portal. Dezelfde para meters kunnen worden ingesteld via de Azure CLI of een SDK.

### <a name="opt-out-policy-to-selectively-exclude-some-property-paths"></a>Opt-out-beleid om selectief enkele eigenschaps paden uit te sluiten

```json
    {
        "indexingMode": "consistent",
        "includedPaths": [
            {
                "path": "/*"
            }
        ],
        "excludedPaths": [
            {
                "path": "/path/to/single/excluded/property/?"
            },
            {
                "path": "/path/to/root/of/multiple/excluded/properties/*"
            }
        ]
    }
```

Dit indexerings beleid is gelijk aan het onderstaande waarmee hand matig ```kind```, ```dataType```en ```precision``` worden ingesteld op de standaard waarden. Deze eigenschappen zijn niet langer nodig om expliciet in te stellen en u kunt ze volledig uit het indexerings beleid weglaten (zoals wordt weer gegeven in het bovenstaande voor beeld).

```json
    {
        "indexingMode": "consistent",
        "includedPaths": [
            {
                "path": "/*",
                "indexes": [
                    {
                        "kind": "Range",
                        "dataType": "Number",
                        "precision": -1
                    },
                    {
                        "kind": "Range",
                        "dataType": "String",
                        "precision": -1
                    }
                ]
            }
        ],
        "excludedPaths": [
            {
                "path": "/path/to/single/excluded/property/?"
            },
            {
                "path": "/path/to/root/of/multiple/excluded/properties/*"
            }
        ]
    }
```

### <a name="opt-in-policy-to-selectively-include-some-property-paths"></a>Beleid voor opt-in om selectief een aantal eigenschaps paden op te nemen

```json
    {
        "indexingMode": "consistent",
        "includedPaths": [
            {
                "path": "/path/to/included/property/?"
            },
            {
                "path": "/path/to/root/of/multiple/included/properties/*"
            }
        ],
        "excludedPaths": [
            {
                "path": "/*"
            }
        ]
    }
```

Dit indexerings beleid is gelijk aan het onderstaande waarmee hand matig ```kind```, ```dataType```en ```precision``` worden ingesteld op de standaard waarden. Deze eigenschappen zijn niet langer nodig om expliciet in te stellen en u kunt ze volledig uit het indexerings beleid weglaten (zoals wordt weer gegeven in het bovenstaande voor beeld).

```json
    {
        "indexingMode": "consistent",
        "includedPaths": [
            {
                "path": "/path/to/included/property/?",
                "indexes": [
                    {
                        "kind": "Range",
                        "dataType": "Number"
                    },
                    {
                        "kind": "Range",
                        "dataType": "String"
                    }
                ]
            },
            {
                "path": "/path/to/root/of/multiple/included/properties/*",
                "indexes": [
                    {
                        "kind": "Range",
                        "dataType": "Number"
                    },
                    {
                        "kind": "Range",
                        "dataType": "String"
                    }
                ]
            }
        ],
        "excludedPaths": [
            {
                "path": "/*"
            }
        ]
    }
```

> [!NOTE] 
> U wordt aangeraden een **opt-out-** indexerings beleid te gebruiken om Azure Cosmos DB proactief een nieuwe eigenschap te indexeren die kan worden toegevoegd aan uw model.

### <a name="using-a-spatial-index-on-a-specific-property-path-only"></a>Alleen een ruimtelijke index gebruiken voor een specifiek pad naar een eigenschap

```json
{
    "indexingMode": "consistent",
    "automatic": true,
    "includedPaths": [
        {
            "path": "/*"
        }
    ],
    "excludedPaths": [
        {
            "path": "/\"_etag\"/?"
        }
    ],
    "spatialIndexes": [
        {
            "path": "/path/to/geojson/property/?",
            "types": [
                "Point",
                "Polygon",
                "MultiPolygon",
                "LineString"
            ]
        }
    ]
}
```

## <a name="composite-indexing-policy-examples"></a>Voor beelden van samengesteld indexerings beleid

Naast het opnemen of uitsluiten van paden voor afzonderlijke eigenschappen, kunt u ook een samengestelde index opgeven. Als u een query wilt uitvoeren met een `ORDER BY`-component voor meerdere eigenschappen, is een [samengestelde index](index-policy.md#composite-indexes) voor die eigenschappen vereist. Daarnaast hebben samengestelde indexen een prestatie voordelen voor query's met een filter en een component ORDER BY op verschillende eigenschappen.

### <a name="composite-index-defined-for-name-asc-age-desc"></a>Samengestelde index gedefinieerd voor (naam ASC, leeftijd DESC):

```json
    {  
        "automatic":true,
        "indexingMode":"Consistent",
        "includedPaths":[  
            {  
                "path":"/*"
            }
        ],
        "excludedPaths":[],
        "compositeIndexes":[  
            [  
                {  
                    "path":"/name",
                    "order":"ascending"
                },
                {  
                    "path":"/age",
                    "order":"descending"
                }
            ]
        ]
    }
```

De bovenstaande samengestelde index op naam en leeftijd is vereist voor query #1 en query #2:

Query #1:

```sql
    SELECT *
    FROM c
    ORDER BY c.name ASC, c.age DESC
```

Query #2:

```sql
    SELECT *
    FROM c
    ORDER BY c.name DESC, c.age ASC
```

In deze samengestelde index vindt u een voor deel #3 Query's en Query's #4 en optimaliseert u de filters:

Query #3:

```sql
SELECT *
FROM c
WHERE c.name = "Tim"
ORDER BY c.name DESC, c.age ASC
```

Query #4:

```sql
SELECT *
FROM c
WHERE c.name = "Tim" AND c.age > 18
```

### <a name="composite-index-defined-for-name-asc-age-asc-and-name-asc-age-desc"></a>Samengestelde index gedefinieerd voor (naam ASC, Age ASC) en (naam ASC, leeftijd DESC):

U kunt meerdere verschillende samengestelde indexen binnen hetzelfde indexerings beleid definiëren.

```json
    {  
        "automatic":true,
        "indexingMode":"Consistent",
        "includedPaths":[  
            {  
                "path":"/*"
            }
        ],
        "excludedPaths":[],
        "compositeIndexes":[  
            [  
                {  
                    "path":"/name",
                    "order":"ascending"
                },
                {  
                    "path":"/age",
                    "order":"ascending"
                }
            ],
            [  
                {  
                    "path":"/name",
                    "order":"ascending"
                },
                {  
                    "path":"/age",
                    "order":"descending"
                }
            ]
        ]
    }
```

### <a name="composite-index-defined-for-name-asc-age-asc"></a>Samengestelde index gedefinieerd voor (naam ASC, leeftijd ASC):

Het is optioneel om de volg orde op te geven. Als u niets opgeeft, wordt de volg orde Oplopend.

```json
{  
        "automatic":true,
        "indexingMode":"Consistent",
        "includedPaths":[  
            {  
                "path":"/*"
            }
        ],
        "excludedPaths":[],
        "compositeIndexes":[  
            [  
                {  
                    "path":"/name",
                },
                {  
                    "path":"/age",
                }
            ]
        ]
}
```

### <a name="excluding-all-property-paths-but-keeping-indexing-active"></a>Alle paden uitsluiten, maar indexeren actief houden

Dit beleid kan worden gebruikt in situaties waarin de [TTL-functie (time-to-Live)](time-to-live.md) actief is, maar geen secundaire index vereist is (om Azure Cosmos DB als zuivere sleutel waarde-archief te gebruiken).

```json
    {
        "indexingMode": "consistent",
        "includedPaths": [],
        "excludedPaths": [{
            "path": "/*"
        }]
    }
```

### <a name="no-indexing"></a>Geen indexering

Met dit beleid wordt indexeren uitgeschakeld. Als `indexingMode` is ingesteld op `none`, kunt u geen TTL instellen in de container.

```json
    {
        "indexingMode": "none"
    }
```

## <a name="updating-indexing-policy"></a>Indexerings beleid bijwerken

In Azure Cosmos DB kan het indexerings beleid worden bijgewerkt met behulp van een van de volgende methoden:

- Van de Azure Portal
- de Azure CLI gebruiken
- Power shell gebruiken
- een van de Sdk's gebruiken

Met een [indexerings beleid-update](index-policy.md#modifying-the-indexing-policy) wordt een index transformatie gegenereerd. De voortgang van deze trans formatie kan ook worden bijgehouden van de Sdk's.

> [!NOTE]
> Wanneer u het indexerings beleid bijwerkt, wordt de schrijf bewerking naar Azure Cosmos DB onderbroken. Tijdens het opnieuw indexeren kunnen query's gedeeltelijke resultaten retour neren als de index wordt bijgewerkt.

## <a name="use-the-azure-portal"></a>De Azure Portal gebruiken

Azure Cosmos-containers slaan hun indexerings beleid op als een JSON-document waarmee u de Azure Portal rechtstreeks kunt bewerken.

1. Meld u aan bij de [Azure-portal](https://portal.azure.com/).

1. Maak een nieuw Azure Cosmos DB-account of selecteer een bestaand account.

1. Open het deel venster **Data Explorer** en selecteer de container waaraan u wilt werken.

1. Klik op **schaal & instellingen**.

1. Het JSON-document van het indexerings beleid wijzigen (Zie de [volgende](#indexing-policy-examples)voor beelden)

1. Klik op **Opslaan** wanneer u klaar bent.

![Indexeren beheren met behulp van de Azure-portal](./media/how-to-manage-indexing-policy/indexing-policy-portal.png)

## <a name="use-the-azure-cli"></a>Azure CLI gebruiken

Als u een container met een aangepast indexerings beleid wilt maken, [maakt u een container met een aangepast index beleid met behulp van CLI](manage-with-cli.md#create-a-container-with-a-custom-index-policy)

## <a name="use-powershell"></a>PowerShell gebruiken

Als u een container met een aangepast indexerings beleid wilt maken, [maakt u een container met een aangepast index beleid met behulp van Power shell](manage-with-powershell.md#create-container-custom-index)

## <a name="use-the-net-sdk-v2"></a>De .NET SDK v2 gebruiken

Het `DocumentCollection`-object van de [.NET SDK v2](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/) bevat een `IndexingPolicy` eigenschap waarmee u de `IndexingMode` kunt wijzigen en `IncludedPaths` en `ExcludedPaths`toevoegt of verwijdert.

```csharp
// Retrieve the container's details
ResourceResponse<DocumentCollection> containerResponse = await client.ReadDocumentCollectionAsync(UriFactory.CreateDocumentCollectionUri("database", "container"));
// Set the indexing mode to consistent
containerResponse.Resource.IndexingPolicy.IndexingMode = IndexingMode.Consistent;
// Add an included path
containerResponse.Resource.IndexingPolicy.IncludedPaths.Add(new IncludedPath { Path = "/*" });
// Add an excluded path
containerResponse.Resource.IndexingPolicy.ExcludedPaths.Add(new ExcludedPath { Path = "/name/*" });
// Add a spatial index
containerResponse.Resource.IndexingPolicy.SpatialIndexes.Add(new SpatialSpec() { Path = "/locations/*", SpatialTypes = new Collection<SpatialType>() { SpatialType.Point } } );
// Add a composite index
containerResponse.Resource.IndexingPolicy.CompositeIndexes.Add(new Collection<CompositePath> {new CompositePath() { Path = "/name", Order = CompositePathSortOrder.Ascending }, new CompositePath() { Path = "/age", Order = CompositePathSortOrder.Descending }});
// Update container with changes
await client.ReplaceDocumentCollectionAsync(containerResponse.Resource);
```

Als u de voortgang van de index transformatie wilt bijhouden, geeft u een `RequestOptions`-object door waarmee de eigenschap `PopulateQuotaInfo` wordt ingesteld op `true`.

```csharp
// retrieve the container's details
ResourceResponse<DocumentCollection> container = await client.ReadDocumentCollectionAsync(UriFactory.CreateDocumentCollectionUri("database", "container"), new RequestOptions { PopulateQuotaInfo = true });
// retrieve the index transformation progress from the result
long indexTransformationProgress = container.IndexTransformationProgress;
```

## <a name="use-the-net-sdk-v3"></a>De .NET SDK v3 gebruiken

Het `ContainerProperties`-object van de [.NET SDK v3](https://www.nuget.org/packages/Microsoft.Azure.Cosmos/) (Zie [deze Snelstartgids](create-sql-api-dotnet.md) met betrekking tot het gebruik) toont een `IndexingPolicy` eigenschap waarmee u de `IndexingMode` kunt wijzigen en `IncludedPaths` en `ExcludedPaths`toevoegt of verwijdert.

```csharp
// Retrieve the container's details
ContainerResponse containerResponse = await client.GetContainer("database", "container").ReadContainerAsync();
// Set the indexing mode to consistent
containerResponse.Resource.IndexingPolicy.IndexingMode = IndexingMode.Consistent;
// Add an included path
containerResponse.Resource.IndexingPolicy.IncludedPaths.Add(new IncludedPath { Path = "/*" });
// Add an excluded path
containerResponse.Resource.IndexingPolicy.ExcludedPaths.Add(new ExcludedPath { Path = "/name/*" });
// Add a spatial index
SpatialPath spatialPath = new SpatialPath
{
    Path = "/locations/*"
};
spatialPath.SpatialTypes.Add(SpatialType.Point);
containerResponse.Resource.IndexingPolicy.SpatialIndexes.Add(spatialPath);
// Add a composite index
containerResponse.Resource.IndexingPolicy.CompositeIndexes.Add(new Collection<CompositePath> { new CompositePath() { Path = "/name", Order = CompositePathSortOrder.Ascending }, new CompositePath() { Path = "/age", Order = CompositePathSortOrder.Descending } });
// Update container with changes
await client.GetContainer("database", "container").ReplaceContainerAsync(containerResponse.Resource);
```

Als u de voortgang van de index transformatie wilt bijhouden, geeft u een `RequestOptions`-object door waarmee de eigenschap `PopulateQuotaInfo` wordt ingesteld op `true`en haalt u vervolgens de waarde op uit de kop van het `x-ms-documentdb-collection-index-transformation-progress`-antwoord.

```csharp
// retrieve the container's details
ContainerResponse containerResponse = await client.GetContainer("database", "container").ReadContainerAsync(new ContainerRequestOptions { PopulateQuotaInfo = true });
// retrieve the index transformation progress from the result
long indexTransformationProgress = long.Parse(containerResponse.Headers["x-ms-documentdb-collection-index-transformation-progress"]);
```

Wanneer u tijdens het maken van een nieuwe container een aangepast indexerings beleid definieert, kunt u met de SDK V3's fluent API op een beknopte en efficiënte manier deze definitie schrijven:

```csharp
await client.GetDatabase("database").DefineContainer(name: "container", partitionKeyPath: "/myPartitionKey")
    .WithIndexingPolicy()
        .WithIncludedPaths()
            .Path("/*")
        .Attach()
        .WithExcludedPaths()
            .Path("/name/*")
        .Attach()
        .WithSpatialIndex()
            .Path("/locations/*", SpatialType.Point)
        .Attach()
        .WithCompositeIndex()
            .Path("/name", CompositePathSortOrder.Ascending)
            .Path("/age", CompositePathSortOrder.Descending)
        .Attach()
    .Attach()
    .CreateIfNotExistsAsync();
```

## <a name="use-the-java-sdk"></a>De Java-SDK gebruiken

Het `DocumentCollection`-object van de [Java-SDK](https://mvnrepository.com/artifact/com.microsoft.azure/azure-cosmosdb) (Zie [deze Snelstartgids](create-sql-api-java.md) met betrekking tot het gebruik) toont `getIndexingPolicy()` en `setIndexingPolicy()` methoden. Met het `IndexingPolicy`-object dat ze manipuleren, kunt u de indexerings modus wijzigen en opgenomen en uitgesloten paden toevoegen of verwijderen.

```java
// Retrieve the container's details
Observable<ResourceResponse<DocumentCollection>> containerResponse = client.readCollection(String.format("/dbs/%s/colls/%s", "database", "container"), null);
containerResponse.subscribe(result -> {
DocumentCollection container = result.getResource();
IndexingPolicy indexingPolicy = container.getIndexingPolicy();

// Set the indexing mode to consistent
indexingPolicy.setIndexingMode(IndexingMode.Consistent);

// Add an included path

Collection<IncludedPath> includedPaths = new ArrayList<>();
ExcludedPath includedPath = new IncludedPath();
includedPath.setPath("/*");
includedPaths.add(includedPath);
indexingPolicy.setIncludedPaths(includedPaths);

// Add an excluded path

Collection<ExcludedPath> excludedPaths = new ArrayList<>();
ExcludedPath excludedPath = new ExcludedPath();
excludedPath.setPath("/name/*");
excludedPaths.add(excludedPath);
indexingPolicy.setExcludedPaths(excludedPaths);

// Add a spatial index

Collection<SpatialSpec> spatialIndexes = new ArrayList<SpatialSpec>();
Collection<SpatialType> collectionOfSpatialTypes = new ArrayList<SpatialType>();

SpatialSpec spec = new SpatialSpec();
spec.setPath("/locations/*");
collectionOfSpatialTypes.add(SpatialType.Point);
spec.setSpatialTypes(collectionOfSpatialTypes);
spatialIndexes.add(spec);

indexingPolicy.setSpatialIndexes(spatialIndexes);

// Add a composite index

Collection<ArrayList<CompositePath>> compositeIndexes = new ArrayList<>();
ArrayList<CompositePath> compositePaths = new ArrayList<>();

CompositePath nameCompositePath = new CompositePath();
nameCompositePath.setPath("/name");
nameCompositePath.setOrder(CompositePathSortOrder.Ascending);

CompositePath ageCompositePath = new CompositePath();
ageCompositePath.setPath("/age");
ageCompositePath.setOrder(CompositePathSortOrder.Descending);

compositePaths.add(ageCompositePath);
compositePaths.add(nameCompositePath);

compositeIndexes.add(compositePaths);
indexingPolicy.setCompositeIndexes(compositeIndexes);

// Update the container with changes

 client.replaceCollection(container, null);
});
```

Als u de voortgang van de index transformatie op een container wilt bijhouden, geeft u een `RequestOptions`-object door waarmee de quotum gegevens moeten worden ingevuld. vervolgens haalt u de waarde op uit de `x-ms-documentdb-collection-index-transformation-progress`-antwoord header.

```java
// set the RequestOptions object
RequestOptions requestOptions = new RequestOptions();
requestOptions.setPopulateQuotaInfo(true);
// retrieve the container's details
Observable<ResourceResponse<DocumentCollection>> containerResponse = client.readCollection(String.format("/dbs/%s/colls/%s", "database", "container"), requestOptions);
containerResponse.subscribe(result -> {
    // retrieve the index transformation progress from the response headers
    String indexTransformationProgress = result.getResponseHeaders().get("x-ms-documentdb-collection-index-transformation-progress");
});
```

## <a name="use-the-nodejs-sdk"></a>De node. js-SDK gebruiken

De `ContainerDefinition`-interface van de [node. js-SDK](https://www.npmjs.com/package/@azure/cosmos) (Zie [deze Snelstartgids](create-sql-api-nodejs.md) met betrekking tot het gebruik) toont een `indexingPolicy` eigenschap waarmee u de `indexingMode` kunt wijzigen en `includedPaths` en `excludedPaths`toevoegt of verwijdert.

De details van de container ophalen

```javascript
const containerResponse = await client.database('database').container('container').read();
```

Stel de indexerings modus consistent in

```javascript
containerResponse.body.indexingPolicy.indexingMode = "consistent";
```

Opgenomen pad inclusief een ruimtelijke index toevoegen

```javascript
containerResponse.body.indexingPolicy.includedPaths.push({
    includedPaths: [
      {
        path: "/age/*",
        indexes: [
          {
            kind: cosmos.DocumentBase.IndexKind.Range,
            dataType: cosmos.DocumentBase.DataType.String
          },
          {
            kind: cosmos.DocumentBase.IndexKind.Range,
            dataType: cosmos.DocumentBase.DataType.Number
          }
        ]
      },
      {
        path: "/locations/*",
        indexes: [
          {
            kind: cosmos.DocumentBase.IndexKind.Spatial,
            dataType: cosmos.DocumentBase.DataType.Point
          }
        ]
      }
    ]
  });
```

Uitgesloten pad toevoegen

```javascript
containerResponse.body.indexingPolicy.excludedPaths.push({ path: '/name/*' });
```

De container bijwerken met wijzigingen

```javascript
const replaceResponse = await client.database('database').container('container').replace(containerResponse.body);
```

Als u de voortgang van de index transformatie op een container wilt bijhouden, geeft u een `RequestOptions`-object door waarmee de eigenschap `populateQuotaInfo` wordt ingesteld op `true`en haalt u vervolgens de waarde op uit de kop van het `x-ms-documentdb-collection-index-transformation-progress`-antwoord.

```javascript
// retrieve the container's details
const containerResponse = await client.database('database').container('container').read({
    populateQuotaInfo: true
});
// retrieve the index transformation progress from the response headers
const indexTransformationProgress = replaceResponse.headers['x-ms-documentdb-collection-index-transformation-progress'];
```

## <a name="use-the-python-sdk"></a>De python-SDK gebruiken

Wanneer u de [python-SDK](https://pypi.org/project/azure-cosmos/) gebruikt (Zie [deze Snelstartgids](create-sql-api-python.md) met betrekking tot het gebruik), wordt de container configuratie als een woorden boek beheerd. Vanuit deze woorden lijst is het mogelijk om toegang te krijgen tot het indexerings beleid en alle bijbehorende kenmerken.

De details van de container ophalen

```python
containerPath = 'dbs/database/colls/collection'
container = client.ReadContainer(containerPath)
```

Stel de indexerings modus consistent in

```python
container['indexingPolicy']['indexingMode'] = 'consistent'
```

Een indexerings beleid definiëren met een opgenomen pad en een ruimtelijke index

```python
container["indexingPolicy"] = {

    "indexingMode":"consistent",
    "spatialIndexes":[
                {"path":"/location/*","types":["Point"]}
             ],
    "includedPaths":[{"path":"/age/*","indexes":[]}],
    "excludedPaths":[{"path":"/*"}]
}
```

Een indexerings beleid definiëren met een uitgesloten pad

```python
container["indexingPolicy"] = {
    "indexingMode":"consistent",
    "includedPaths":[{"path":"/*","indexes":[]}],
    "excludedPaths":[{"path":"/name/*"}]
}
```

Een samengestelde index toevoegen

```python
container['indexingPolicy']['compositeIndexes'] = [
                [
                    {
                        "path": "/name",
                        "order": "ascending"
                    },
                    {
                        "path": "/age",
                        "order": "descending"
                    }
                ]
                ]
```

De container bijwerken met wijzigingen

```python
response = client.ReplaceContainer(containerPath, container)
```

## <a name="next-steps"></a>Volgende stappen

Lees meer over indexeren in de volgende artikelen:

- [Overzicht van indexeren](index-overview.md)
- [Indexeringsbeleid](index-policy.md)
