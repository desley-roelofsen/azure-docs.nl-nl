---
title: Meer informatie over Apache Spark code concepten voor Azure Data Lake Analytics U-SQL-ontwikkel aars.
description: In dit artikel worden Apache Spark concepten beschreven om U-SQL-ontwikkel aars te helpen bij het begrijpen van Spark-code concepten.
author: guyhay
ms.author: guyhay
ms.reviewer: jasonh
ms.service: data-lake-analytics
ms.topic: conceptual
ms.custom: Understand-apache-spark-code-concepts
ms.date: 10/15/2019
ms.openlocfilehash: bdb38e36a9f1344a3adde15d349a2ec176c0fe95
ms.sourcegitcommit: 4c831e768bb43e232de9738b363063590faa0472
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/23/2019
ms.locfileid: "74424001"
---
# <a name="understand-apache-spark-code-for-u-sql-developers"></a>Apache Spark code voor U-SQL-ontwikkel aars begrijpen

In deze sectie vindt U informatie op hoog niveau over het transformeren van U-SQL-scripts naar Apache Spark.

- Het begint met een [vergelijking van de verwerkings modellen van de twee talen](#understand-the-u-sql-and-spark-language-and-processing-paradigms)
- Hier vindt u tips voor het volgende:
   - [Scripts transformeren](#transform-u-sql-scripts) met de [Rijset-expressies](#transform-u-sql-rowset-expressions-and-sql-based-scalar-expressions) van U-SQL
   - [.NET-code](#transform-net-code)
   - [Gegevenstypen](#transform-typed-values)
   - [Catalogus objecten](#transform-u-sql-catalog-objects).

## <a name="understand-the-u-sql-and-spark-language-and-processing-paradigms"></a>Meer informatie over de U-SQL-en Spark-taal en verwerkings modellen

Voordat u begint met het migreren van Azure Data Lake Analytics ' U-SQL-scripts naar Spark ', is het handig om inzicht te krijgen in de algemene taal en de verwerkings aanpakken van de twee systemen.

U-SQL is een SQL-achtige declaratieve query taal die gebruikmaakt van een paradigma voor gegevens stromen en waarmee u gemakkelijk gebruikers code kunt insluiten en uitschalen in .NET (bijvoorbeeld C#), python en R. Met de gebruikers extensies kunnen eenvoudige expressies of door de gebruiker gedefinieerde functies worden geïmplementeerd, maar kan de gebruiker ook de mogelijkheid bieden om de door de gebruiker gedefinieerde Opera tors te implementeren waarmee aangepaste Opera tors worden geïmplementeerd voor het uitvoeren van een Rijset-trans formaties, uittreksels en het schrijven van uitvoer.

Spark is een scale-out-Framework met verschillende taal bindingen in scala, Java, Python, .NET enzovoort. Wanneer u uw code voornamelijk schrijft in een van deze talen, maakt u gegevens abstracties, de naam flexibele gedistribueerde gegevens sets (RDD), dataframes en gegevens sets. gebruik vervolgens een LINQ-achtige domein-specifieke taal (DSL) om ze te transformeren. Het biedt ook SparkSQL als een declaratieve subtaal in de samen vattingen van data frame en DataSet. De DSL biedt twee categorieën bewerkingen, trans formaties en acties. Door trans formaties toe te passen op de gegevens abstracties, wordt de omzetting niet uitgevoerd, maar wordt in plaats daarvan het uitvoerings plan gemaakt dat wordt verzonden voor evaluatie met een actie (bijvoorbeeld het resultaat in een tijdelijke tabel of bestand wordt geschreven of wordt het resultaat).

Wanneer u een U-SQL-script naar een Spark-programma vertaalt, moet u bepalen welke taal u wilt gebruiken om ten minste de abstractie van het gegevens frame te genereren (dit is de meest gebruikte gegevens abstractie) en of u de declaratief wilt schrijven trans formaties voor gegevens stroom met behulp van DSL of SparkSQL. In sommige complexere gevallen moet u mogelijk uw U-SQL-script splitsen in een reeks Spark en andere stappen die zijn geïmplementeerd met Azure Batch of Azure Functions.

Daarnaast biedt Azure Data Lake Analytics U-SQL in een serverloze taak Service omgeving, terwijl zowel Azure Databricks als Azure HDInsight Spark in de vorm van een cluster service bieden. Wanneer u uw toepassing transformeert, moet u rekening houden met de implicaties van het maken, verg Roten/verkleinen, schalen en buiten gebruik stellen van de clusters.

## <a name="transform-u-sql-scripts"></a>U-SQL-scripts transformeren

U-SQL-scripts volgen het volgende verwerkings patroon:

1. Gegevens worden gelezen van ongestructureerde bestanden, met behulp van de `EXTRACT`-instructie, een locatie of een specificatie van de set en het ingebouwde of door de gebruiker gedefinieerde Extractor en gewenst schema, of vanuit U-SQL-tabellen (beheerde of externe tabellen). Deze wordt weer gegeven als een rijenset.
2. De rijen sets worden getransformeerd in meerdere U-SQL-instructies waarmee U-SQL-expressies worden toegepast op de rijen sets en nieuwe rijen sets worden geproduceerd.
3. Ten slotte worden de resulterende rijen sets in beide bestanden uitgevoerd met behulp van de `OUTPUT`-instructie waarmee de locatie (s) en een ingebouwde of door de gebruiker gedefinieerde outputter of een U-SQL-tabel worden opgegeven.

Het script wordt geëvalueerd vertraagd, wat inhoudt dat elke extractie-en transformatie stap is samengesteld in een expressie structuur en wereld wijd geëvalueerd (de gegevens stroom).

Spark-Program ma's zijn vergelijkbaar met het gebruik van Spark-connectors om de gegevens te lezen en de dataframes te maken en vervolgens de trans formaties op de dataframes toe te passen met behulp van de LINQ-achtige DSL-of SparkSQL en vervolgens het resultaat te schrijven naar bestanden, tijdelijke Spark-tabellen, Sommige typen programmeer talen of de-console.

## <a name="transform-net-code"></a>.NET-code transformeren

De expressie taal van U-SQL C# is en biedt diverse manieren om aangepaste .net-code uit te schalen.

Aangezien Spark momenteel geen systeem eigen ondersteuning biedt voor het uitvoeren van .NET-code, moet u uw expressies herschrijven naar een equivalente Spark-, scala-, Java-of python-expressie of een manier vinden om uw .NET-code aan te roepen. Als uw script .NET-bibliotheken gebruikt, hebt u de volgende opties:

- Vertaal uw .NET-code naar scala of python.
- Splits uw U-SQL-script in verschillende stappen, waarbij u Azure Batch-processen gebruikt om de .NET-trans formaties toe te passen (als u een acceptabele schaal kunt krijgen)
- Gebruik een .NET-taal binding die beschikbaar is in open source met de naam Moebius. Dit project heeft een niet-ondersteunde status.

Als u in uw U-SQL-scripts een grote hoeveelheid .NET-logica hebt, kunt u contact met ons opnemen via de vertegenwoordiger van uw micro soft-account voor meer informatie.

De volgende details zijn voor de verschillende scenario's van .NET en C# gebruik in U-SQL-scripts.

### <a name="transform-scalar-inline-u-sql-c-expressions"></a>Scalaire inline U-SQL C# -expressies transformeren

De expressie taal van U-SQL C#is. Veel van de scalaire, inline U-SQL-expressies worden standaard geïmplementeerd voor betere prestaties, terwijl complexere expressies kunnen worden uitgevoerd via het aanroepen van .NET Framework.

Spark heeft een eigen scalaire expressie taal (als onderdeel van de DSL of in SparkSQL) en kan aanroepen naar door de gebruiker gedefinieerde functies die zijn geschreven in de hosting taal.

Als u scalaire expressies in U-SQL hebt, moet u eerst de meest geschikte systeem eigen scalaire Spark-expressie vinden om de meeste prestaties te verkrijgen. vervolgens wijst u de andere expressies toe aan een door de gebruiker gedefinieerde functie van de Spark-hosting taal van uw keuze.

Houd er rekening mee dat C# .net en een ander type semantiek heeft dan de Spark-hosting talen en de DSL van Spark. Zie [hieronder](#transform-typed-values) voor meer informatie over het type systeem verschillen.

### <a name="transform-user-defined-scalar-net-functions-and-user-defined-aggregators"></a>Door de gebruiker gedefinieerde scalaire .NET-functies en door de gebruiker gedefinieerde aggregaties transformeren

U-SQL biedt manieren om wille keurige scalaire .NET-functies aan te roepen en door de gebruiker gedefinieerde aggregators aan te roepen die in .NET zijn geschreven.

Spark biedt ook ondersteuning voor door de gebruiker gedefinieerde functies en door de gebruiker gedefinieerde aggregaties die in de meeste hosting talen zijn geschreven en die kunnen worden aangeroepen vanuit de DSL-en SparkSQL van Spark.

### <a name="transform-user-defined-operators-udos"></a>Door de gebruiker gedefinieerde Opera tors transformeren (Udo's)

U-SQL biedt verschillende categorieën door de gebruiker gedefinieerde Opera tors (Udo's), zoals uittreksels, preputters, verminderers, processors, appliers en combi naties die in .NET kunnen worden geschreven (en-naar enige mate-in Python en R).

Spark biedt niet hetzelfde uitbreidings model voor Opera Tors, maar heeft een vergelijk bare functionaliteit voor sommige.

De Spark-equivalent voor extract oren en outputten is Spark-connectors. Voor veel U-SQL-Extracts kunt u een equivalente connector in de Spark-Community vinden. Voor anderen moet u een aangepaste connector schrijven. Als de U-SQL-Extractor complex is en verschillende .NET-bibliotheken gebruikt, is het wellicht beter om een connector te bouwen in scala die gebruikmaakt van interop om de .NET-bibliotheek op te roepen die de daad werkelijke verwerking van de gegevens uitvoert. In dat geval moet u de .NET core-runtime implementeren in het Spark-cluster en ervoor zorgen dat de .net-bibliotheken waarnaar wordt verwezen .NET Standard 2,0 compatibel zijn.

De andere typen U-SQL-Udo's moeten opnieuw worden geschreven met door de gebruiker gedefinieerde functies en aggregators en de semantisch juiste geschikte Spark-DLS of SparkSQL-expressie. Een processor kan bijvoorbeeld worden toegewezen aan een selectie van een groot aantal UDF-aanroepen, verpakt als een functie die een data frame als argument gebruikt en een data frame retourneert.

### <a name="transform-u-sqls-optional-libraries"></a>Optionele bibliotheken voor transformeren U-SQL

U-SQL biedt een reeks optionele en demo bibliotheken die [python](data-lake-analytics-u-sql-python-extensions.md)-, [R](data-lake-analytics-u-sql-r-extensions.md)-, JSON-, [XML-, AVRO-ondersteuning](https://github.com/Azure/usql/tree/master/Examples/DataFormats)bieden en enkele [functies voor cognitieve Services](data-lake-analytics-u-sql-cognitive.md).

Spark biedt respectievelijk een eigen python-en R-integratie, pySpark en Spark, en biedt connectors voor het lezen en schrijven van JSON, XML en AVRO.

Als u een script moet transformeren dat verwijst naar de cognitieve Services-bibliotheken, raden we u aan contact met ons op te nemen via de vertegenwoordiger van uw micro soft-account.

## <a name="transform-typed-values"></a>Getypte waarden transformeren

Omdat het type systeem van U-SQL is gebaseerd op het .NET-type systeem en Spark een eigen type systeem heeft dat wordt beïnvloed door de binding van de host-taal, moet u ervoor zorgen dat de typen die u aan het werk bent, worden gesloten en voor bepaalde typen, het type bereik, de precisie en/of de schaal iets anders kan zijn. Daarnaast behandelen U-SQL en Spark `null` waarden anders.

### <a name="data-types"></a>Gegevenstypen

De volgende tabel geeft de equivalente typen in Spark, scala en PySpark voor de opgegeven U-SQL-typen.

| U-SQL | Spark |  Scala | PySpark |
| ------ | ------ | ------ | ------ |
|`byte`       ||||
|`sbyte`      |`ByteType` |`Byte` | `ByteType`|
|`int`        |`IntegerType` |`Int` | `IntegerType`|
|`uint`       ||||
|`long`       |`LongType` |`Long` | `LongType`|
|`ulong`      ||||
|`float`      |`FloatType` |`Float` | `FloatType`|
|`double`     |`DoubleType` |`Double` | `DoubleType`|
|`decimal`    |`DecimalType` |`java.math.BigDecimal` | `DecimalType`|
|`short`      |`ShortType` |`Short` | `ShortType`|
|`ushort`     ||||
|`char`   | |`Char`||
|`string` |`StringType` |`String` |`StringType` |
|`DateTime`   |`DateType`, `TimestampType` |`java.sql.Date`, `java.sql.Timestamp` | `DateType`, `TimestampType`|
|`bool`   |`BooleanType` |`Boolean` | `BooleanType`|
|`Guid`   ||||
|`byte[]` |`BinaryType` |`Array[Byte]` | `BinaryType`|
|`SQL.MAP<K,V>`   |`MapType(keyType, valueType, valueContainsNull)` |`scala.collection.Map` | `MapType(keyType, valueType, valueContainsNull=True)`|
|`SQL.ARRAY<T>`   |`ArrayType(elementType, containsNull)` |`scala.collection.Seq` | `ArrayType(elementType, containsNull=True)`|

Ga voor meer informatie naar:

- [org. apache. Spark. SQL. types](https://spark.apache.org/docs/latest/api/scala/index.html#org.apache.spark.sql.types.package)
- [Spark SQL-en DataFrames-typen](https://spark.apache.org/docs/latest/sql-reference.html#data-types)
- [Scala-waardetypen](https://www.scala-lang.org/api/current/scala/AnyVal.html)
- [pyspark. SQL. types](https://spark.apache.org/docs/latest/api/python/pyspark.sql.html#module-pyspark.sql.types)

### <a name="treatment-of-null"></a>Behandeling van NULL

In Spark, typen per standaard NULL-waarden toestaan in U-SQL, kunt u scalair, niet-object, als null-waarde markeren. Hoewel Spark in staat is om een kolom te definiëren als niet-nullbaar, wordt de beperking niet afgedwongen en [kan dit leiden tot een onjuist resultaat](https://medium.com/@weshoffman/apache-spark-parquet-and-troublesome-nulls-28712b06f836).

In Spark geeft NULL aan dat de waarde onbekend is. Een vonk NULL-waarde wijkt af van een wille keurige waarde, waaronder zichzelf. Vergelijkingen tussen twee Spark NULL-waarden, of tussen een NULL-waarde en een andere waarde, retour neren Unknown, omdat de waarde van elke NULL onbekend is.  

Dit gedrag wijkt af van U-SQL. Dit is C# de semantiek waarbij `null` afwijkt van een wille keurige waarde, maar gelijk is aan zichzelf.  

Een SparkSQL `SELECT`-instructie die gebruikmaakt van `WHERE column_name = NULL` nul rijen retourneert, zelfs als er NULL-waarden in `column_name`zijn, wordt in U-SQL de rijen geretourneerd waarin `column_name` is ingesteld op `null`. Op dezelfde manier wordt een Spark-`SELECT`-instructie die gebruikmaakt van `WHERE column_name != NULL` nul rijen retourneert, zelfs als er niet-Null-waarden in `column_name`zijn, terwijl U in U-SQL de rijen zou retour neren die niet null zijn. Als u wilt dat de u-SQL-controle semantiek is, moet u de respectievelijk [IsNull](https://spark.apache.org/docs/2.3.0/api/sql/index.html#isnull) en [isnotnull](https://spark.apache.org/docs/2.3.0/api/sql/index.html#isnotnull) (of het bijbehorende DSL-equivalent) gebruiken.

## <a name="transform-u-sql-catalog-objects"></a>U-SQL-catalogus objecten transformeren

Een belang rijk verschil is dat U-SQL-scripts gebruik kunnen maken van de catalogus objecten, waarvan er veel zijn die geen direct Spark-equivalent hebben.

Spark biedt ondersteuning voor de concepten van het meta Store-Hive, voornamelijk data bases en tabellen, zodat u U-SQL-data bases en-schema's kunt toewijzen aan Hive-data bases en U-SQL-tabellen in Spark-tabellen (Zie [gegevens verplaatsen die zijn opgeslagen in U-SQL-tabellen](understand-spark-data-formats.md#move-data-stored-in-u-sql-tables)), maar deze geen ondersteuning biedt voor weer gaven, functies met tabel waarden (TVFs), opgeslagen procedures, U-SQL-assembly's, externe gegevens bronnen, enzovoort.

De U-SQL-code objecten zoals weer gaven, TVFs, opgeslagen procedures en assembly's kunnen worden gemodelleerd via code functies en bibliotheken in Spark en ernaar wordt verwezen met behulp van de functie en procedurele abstractie mechanismen van de host-taal (bijvoorbeeld door middel van importeren Python-modules of verwijzen naar scala-functies).

Als de U-SQL-catalogus is gebruikt voor het delen van gegevens en code objecten in projecten en teams, moeten gelijkwaardige mechanismen voor delen worden gebruikt (bijvoorbeeld maven voor het delen van code objecten).

## <a name="transform-u-sql-rowset-expressions-and-sql-based-scalar-expressions"></a>Transformeer U-SQL Rijset-expressies en scalaire SQL-expressies

De kern taal van U-SQL is het transformeren van rijsets en is gebaseerd op SQL. Hier volgt een niet-limitatieve lijst met de meest voorkomende Rijset-expressies die worden aangeboden in U-SQL:

- `SELECT`/`FROM`/`WHERE`/`GROUP BY`+ aggregaties +`HAVING`/`ORDER BY`+`FETCH`
- `INNER`/`OUTER`/`CROSS`/`SEMI` `JOIN`-expressies
- `CROSS`/`OUTER` `APPLY` expressies
- `PIVOT`/`UNPIVOT` expressies
- constructor van `VALUES` Rijset

- Expressies instellen `UNION`/`OUTER UNION`/`INTERSECT`/`EXCEPT`

Daarnaast biedt U-SQL een aantal scalaire expressies op basis van SQL, zoals

- expressies voor `OVER` venster
- een aantal ingebouwde aggregaties en rangorde functies (`SUM`, `FIRST` enz.)
- Enkele van de meest bekende scalaire SQL-expressies: `CASE`, `LIKE`, (`NOT`) `IN`, `AND`, `OR`, enzovoort.

Spark biedt gelijkwaardige expressies in het DSL-en SparkSQL-formulier voor de meeste van deze expressies. Sommige van de expressies die standaard niet worden ondersteund in Spark, moeten opnieuw worden geschreven met een combi natie van de systeem eigen Spark-expressies en semantisch gelijkwaardige patronen. `OUTER UNION` moet bijvoorbeeld worden vertaald naar de equivalente combi natie van projecties en samen voegingen.

Als gevolg van de andere verwerking van NULL-waarden, komt een U-SQL-samen voeging altijd overeen met een rij als beide kolommen die worden vergeleken een NULL-waarde bevatten, terwijl een koppeling in Spark niet overeenkomt met dergelijke kolommen, tenzij expliciete null-controles worden toegevoegd.

## <a name="transform-other-u-sql-concepts"></a>Andere U-SQL-concepten transformeren

U-SQL biedt ook allerlei andere functies en concepten, zoals federatieve query's voor SQL Server data bases, para meters, scalaire en lambda-expressie variabelen, systeem variabelen, `OPTION` hints.

### <a name="federated-queries-against-sql-server-databasesexternal-tables"></a>Federatieve Query's voor SQL Server data bases/externe tabellen

U-SQL biedt gegevens bron en externe tabellen, evenals directe query's voor Azure SQL Database. Hoewel Spark niet dezelfde object abstractie biedt, biedt het een Spark- [Connector voor Azure SQL database](../sql-database/sql-database-spark-connector.md) dat kan worden gebruikt om SQL-data bases op te vragen.

### <a name="u-sql-parameters-and-variables"></a>U-SQL-para meters en-variabelen

Para meters en gebruikers variabelen hebben gelijkwaardige concepten in Spark en hun hosting talen.

In scala kunt u bijvoorbeeld een variabele definiëren met het sleutel woord `var`:

```
var x = 2 * 3;
println(x)
```

De systeem variabelen van U-SQL (variabelen die beginnen met `@@`) kunnen worden onderverdeeld in twee categorieën:

- Instel bare systeem variabelen die kunnen worden ingesteld op specifieke waarden om het gedrag van scripts te beïnvloeden
- Informatief systeem variabelen die informatie over systeem-en taak niveau opvragen

De meeste instel bare systeem variabelen hebben geen direct equivalent in Spark. Sommige informatie systeem variabelen kunnen worden gemodelleerd door de informatie als argumenten tijdens het uitvoeren van taken door te geven, maar andere kunnen een equivalente functie in de taal van Spark hebben.

### <a name="u-sql-hints"></a>U-SQL-hints

U-SQL biedt verschillende manieren om hints te bieden aan de query Optimizer en uitvoerings Engine:  

- Een U-SQL-systeem variabele instellen
- een `OPTION`-component die is gekoppeld aan de Rijset-expressie om een gegevens-of plan hint op te geven
- een samenvoegings Hint in de syntaxis van de expressie voor samen voegen (bijvoorbeeld `BROADCASTLEFT`)

De op kosten gebaseerde query optimalisatie van Spark heeft zijn eigen mogelijkheden om hints te bieden en de query prestaties af te stemmen. Raadpleeg de bijbehorende documentatie.

## <a name="next-steps"></a>Volgende stappen

- [Informatie over Spark-gegevens indelingen voor U-SQL-ontwikkel aars](understand-spark-data-formats.md)
- [.NET voor Apache Spark](https://docs.microsoft.com/dotnet/spark/what-is-apache-spark-dotnet)
- [Upgrade uw big data Analytics-oplossingen van Azure Data Lake Storage Gen1 naar Azure Data Lake Storage Gen2](../storage/blobs/data-lake-storage-upgrade.md)
- [Gegevens transformeren met behulp van Spark-activiteit in Azure Data Factory](../data-factory/transform-data-using-spark.md)
- [Gegevens transformeren met behulp van Hadoop Hive-activiteit in Azure Data Factory](../data-factory/transform-data-using-hadoop-hive.md)
- [Wat is Apache Spark in azure HDInsight?](../hdinsight/spark/apache-spark-overview.md)
