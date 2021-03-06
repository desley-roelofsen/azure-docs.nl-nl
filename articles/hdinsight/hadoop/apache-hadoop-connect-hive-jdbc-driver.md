---
title: Apache Hive opvragen via het JDBC-stuur programma-Azure HDInsight
description: Gebruik het JDBC-stuur programma van een Java-toepassing om Apache Hive query's naar Hadoop in HDInsight te verzenden. Verbinding maken via een programma en vanuit de SQuirrel SQL-client.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.topic: conceptual
ms.date: 10/24/2019
ms.openlocfilehash: 2250e41bffc26bd9ae59dfc652a06d08016d227a
ms.sourcegitcommit: 87efc325493b1cae546e4cc4b89d9a5e3df94d31
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/29/2019
ms.locfileid: "73053798"
---
# <a name="query-apache-hive-through-the-jdbc-driver-in-hdinsight"></a>Query's Apache Hive via het JDBC-stuur programma in HDInsight

[!INCLUDE [ODBC-JDBC-selector](../../../includes/hdinsight-selector-odbc-jdbc.md)]

Meer informatie over het gebruik van het JDBC-stuur programma van een Java-toepassing om Apache Hive query's in te dienen bij Apache Hadoop in azure HDInsight. De informatie in dit document laat zien hoe u via een programma verbinding maakt, en van de SQuirreL SQL-client.

Zie [HiveJDBCInterface](https://cwiki.apache.org/confluence/display/Hive/HiveJDBCInterface)voor meer informatie over de component JDBC-interface.

## <a name="prerequisites"></a>Vereisten

* An HDInsight Hadoop-cluster. Zie aan de [slag met Azure HDInsight](apache-hadoop-linux-tutorial-get-started.md)om een account te maken.
* De [jdk-versie 11 of hoger van Java Developer Kit](https://www.oracle.com/technetwork/java/javase/downloads/jdk11-downloads-5066655.html) .
* [Squirrel SQL](http://squirrel-sql.sourceforge.net/). SQuirreL is een JDBC-client toepassing.

## <a name="jdbc-connection-string"></a>JDBC-verbindingsreeks

JDBC-verbindingen met een HDInsight-cluster op Azure worden gemaakt via poort 443 en het verkeer wordt beveiligd met SSL. De open bare gateway die de clusters achter bevindt, stuurt het verkeer naar de poort waarop HiveServer2 wordt geluisterd. In het volgende connection string ziet u de indeling die moet worden gebruikt voor HDInsight:

    jdbc:hive2://CLUSTERNAME.azurehdinsight.net:443/default;transportMode=http;ssl=true;httpPath=/hive2

Vervang `CLUSTERNAME` door de naam van uw HDInsight-cluster.

## <a name="authentication"></a>Verificatie

Wanneer u de verbinding tot stand brengt, moet u de naam en het wacht woord van het HDInsight-cluster beheerder gebruiken om de cluster gateway te verifiëren. Wanneer u verbinding maakt vanuit JDBC-clients, zoals SQuirreL SQL, moet u de beheerders naam en het wacht woord opgeven in de client instellingen.

Vanuit een Java-toepassing moet u de naam en het wacht woord gebruiken om een verbinding tot stand te brengen. Met de volgende Java-code wordt bijvoorbeeld een nieuwe verbinding geopend met de connection string, de naam van de beheerder en het wacht woord:

```java
DriverManager.getConnection(connectionString,clusterAdmin,clusterPassword);
```

## <a name="connect-with-squirrel-sql-client"></a>Verbinding maken met SQuirreL SQL-client

SQuirreL SQL is een JDBC-client die kan worden gebruikt om Hive-query's op afstand uit te voeren met uw HDInsight-cluster. Bij de volgende stappen wordt ervan uitgegaan dat u SQuirreL SQL al hebt geïnstalleerd.

1. Maak een map die bepaalde bestanden bevat die uit uw cluster moeten worden gekopieerd.

2. Vervang in het volgende script `sshuser` door de naam van het SSH-gebruikers account voor het cluster.  Vervang `CLUSTERNAME` door de naam van het HDInsight-cluster.  Wijzig vanaf een opdracht regel uw werkmap naar de map die is gemaakt in de vorige stap en voer de volgende opdracht in om bestanden te kopiëren vanuit een HDInsight-cluster:

    ```cmd
    scp sshuser@CLUSTERNAME-ssh.azurehdinsight.net:/usr/hdp/current/hadoop-client/{hadoop-auth.jar,hadoop-common.jar,lib/log4j-*.jar,lib/slf4j-*.jar,lib/curator-*.jar} .

    scp sshuser@CLUSTERNAME-ssh.azurehdinsight.net:/usr/hdp/current/hive-client/lib/{commons-codec*.jar,commons-logging-*.jar,hive-*-*.jar,httpclient-*.jar,httpcore-*.jar,libfb*.jar,libthrift-*.jar} .
    ```

3. Start de SQuirreL-SQL-toepassing. Selecteer links in het venster **Stuur Programma's**.

    ![Het tabblad Stuur Programma's aan de linkerkant van het venster](./media/apache-hadoop-connect-hive-jdbc-driver/hdinsight-squirreldrivers.png)

4. Selecteer in de pictogrammen boven in het dialoog venster **Stuur Programma's** het **+** pictogram om een stuur programma te maken.

    ![Pictogram voor SQuirreL SQL-toepassings Stuur Programma's](./media/apache-hadoop-connect-hive-jdbc-driver/hdinsight-driversicons.png)

5. Voeg in het dialoog venster stuur programma toevoegen de volgende informatie toe:

    |Eigenschap | Waarde |
    |---|---|
    |Naam|Hive|
    |Voor beeld-URL|JDBC: hive2://localhost: 443/default; transportMode = HTTP; SSL = True; httpPath =/hive2|
    |Extra klassepad|Gebruik de knop **toevoegen** om alle eerder gedownloade jar-bestanden toe te voegen.|
    |Klassenaam|org. apache. Hive. JDBC. HiveDriver|

   ![dialoog venster stuur programma toevoegen met para meters](./media/apache-hadoop-connect-hive-jdbc-driver/hdinsight-add-driver.png)

   Selecteer **OK** om deze instellingen op te slaan.

6. Selecteer aan de linkerkant van het SQuirreL SQL-venster **aliassen**. Selecteer vervolgens het **+** pictogram om een verbindings alias te maken.

    ![Dialoog venster nieuwe alias voor SQuirreL SQL toevoegen](./media/apache-hadoop-connect-hive-jdbc-driver/hdinsight-new-aliases.png)

7. Gebruik de volgende waarden voor het dialoog venster **alias toevoegen** :

    |Eigenschap |Waarde |
    |---|---|
    |Naam|Hive in HDInsight|
    |Stuurprogramma|Gebruik de vervolg keuzelijst om het **Hive** -stuur programma te selecteren.|
    |URL|JDBC: hive2://CLUSTERNAME.azurehdinsight.net: 443/default; transportMode = HTTP; SSL = True; httpPath =/hive2. Vervang **CLUSTERNAME** door de naam van uw HDInsight-cluster.|
    |Gebruikersnaam|De naam van het cluster aanmeldings account voor uw HDInsight-cluster. De standaard instelling is **admin**.|
    |Wachtwoord|Het wacht woord voor het cluster aanmeldings account.|

    ![dialoog venster alias toevoegen met para meters](./media/apache-hadoop-connect-hive-jdbc-driver/hdinsight-addalias-dialog.png)

    > [!IMPORTANT]
    > Gebruik de knop **testen** om te controleren of de verbinding werkt. Wanneer **verbinding maken met: Hive in HDInsight** dialoog venster wordt weer gegeven, selecteert u **verbinding maken** om de test uit te voeren. Als de test is geslaagd, ziet u een dialoog venster **verbinding geslaagd** . Zie [probleem oplossing](#troubleshooting)als er een fout optreedt.

    Als u de verbindings alias wilt opslaan, klikt u op de knop **OK** onder aan het dialoog venster **alias toevoegen** .

8. Selecteer in de vervolg keuzelijst **verbinding maken** met de bovenkant van SQuirreL SQL **Hive op HDInsight**. Selecteer **verbinding maken**wanneer u hierom wordt gevraagd.

    ![dialoog venster verbinding met para meters](./media/apache-hadoop-connect-hive-jdbc-driver/hdinsight-connect-dialog.png)

9. Als de verbinding tot stand is gebracht, voert u de volgende query in het dialoog venster SQL-query in en selecteert u vervolgens het pictogram **uitvoeren** (een actieve persoon). In het resultaten gebied moeten de resultaten van de query worden weer gegeven.

    ```hql
    select * from hivesampletable limit 10;
    ```

    ![dialoog venster SQL-query, inclusief resultaten](./media/apache-hadoop-connect-hive-jdbc-driver/hdinsight-sqlquery-dialog.png)

## <a name="connect-from-an-example-java-application"></a>Verbinding maken via een Java-voorbeeld toepassing

Een voor beeld van het gebruik van een Java-client naar een query op HDInsight is beschikbaar op [https://github.com/Azure-Samples/hdinsight-java-hive-jdbc](https://github.com/Azure-Samples/hdinsight-java-hive-jdbc). Volg de instructies in de opslag plaats om het voor beeld te bouwen en uit te voeren.

## <a name="troubleshooting"></a>Problemen oplossen

### <a name="unexpected-error-occurred-attempting-to-open-an-sql-connection"></a>Er is een onverwachte fout opgetreden bij het openen van een SQL-verbinding

**Symptomen**: wanneer u verbinding maakt met een HDInsight-cluster met versie 3,3 of hoger, wordt er mogelijk een fout bericht weer gegeven dat er een onverwachte fout is opgetreden. De stack-tracering voor deze fout begint met de volgende regels:

```java
java.util.concurrent.ExecutionException: java.lang.RuntimeException: java.lang.NoSuchMethodError: org.apache.commons.codec.binary.Base64.<init>(I)V
at java.util.concurrent.FutureTas...(FutureTask.java:122)
at java.util.concurrent.FutureTask.get(FutureTask.java:206)
```

**Oorzaak**: deze fout wordt veroorzaakt door een oudere versie van het bestand Commons-codec. jar dat is opgenomen in SQuirreL.

**Oplossing**: gebruik de volgende stappen om deze fout op te lossen:

1. Sluit SQuirreL af en ga naar de map waarin SQuirreL op uw systeem is geïnstalleerd, mogelijk `C:\Program Files\squirrel-sql-4.0.0\lib`. Vervang in de SquirreL-map, onder de map `lib`, de bestaande Commons-codec. jar door de naam van het HDInsight-cluster dat u hebt gedownload.

1. Start SQuirreL opnieuw. De fout mag niet langer optreden wanneer u verbinding maakt met hive op HDInsight.

## <a name="next-steps"></a>Volgende stappen

Nu u hebt geleerd hoe u JDBC kunt gebruiken om te werken met Hive, gebruikt u de volgende koppelingen om andere manieren te verkennen om met Azure HDInsight te werken.

* [Visualiseer Apache Hive gegevens met micro soft-power bi in azure HDInsight](apache-hadoop-connect-hive-power-bi.md).
* [Visualisatie gegevens van interactieve Query's visualiseren met Power bi in azure HDInsight](../interactive-query/apache-hadoop-connect-hive-power-bi-directquery.md).
* [Gebruik Apache Zeppelin om Apache Hive query's uit te voeren in azure HDInsight](../interactive-query/hdinsight-connect-hive-zeppelin.md).
* [Koppel Excel aan HDInsight met het micro soft Hive ODBC-stuur programma](apache-hadoop-connect-excel-hive-odbc-driver.md).
* [Verbinding maken tussen Excel en Apache Hadoop met behulp van Power query](apache-hadoop-connect-excel-power-query.md).
* [Verbinding maken met Azure HDInsight en Apache Hive query's uitvoeren met behulp van data Lake-Hulpprogram ma's voor Visual Studio](apache-hadoop-visual-studio-tools-get-started.md).
* [Gebruik het hulp programma Azure HDInsight voor Visual Studio code](../hdinsight-for-vscode.md).
* [Gegevens uploaden naar HDInsight](../hdinsight-upload-data.md)
* [Apache Hive gebruiken met HDInsight](hdinsight-use-hive.md)
* [Apache Pig gebruiken met HDInsight](hdinsight-use-pig.md)
* [MapReduce-taken gebruiken met HDInsight](hdinsight-use-mapreduce.md)
