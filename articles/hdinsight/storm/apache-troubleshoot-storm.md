---
title: Met Storm oplossen met behulp van Azure HDInsight
description: Vind antwoorden op veelgestelde vragen over het gebruik van Apache Storm met Azure HDInsight.
keywords: HDInsight, Storm, veelgestelde vragen over Azure, problemen oplossen handleiding worden veelvoorkomende problemen
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: troubleshooting
ms.date: 11/08/2019
ms.custom: seodec18
ms.openlocfilehash: e2cc9dd81a0bbefa5cf37facb6067bda07117eaf
ms.sourcegitcommit: bc193bc4df4b85d3f05538b5e7274df2138a4574
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/10/2019
ms.locfileid: "73903726"
---
# <a name="troubleshoot-apache-storm-by-using-azure-hdinsight"></a>Apache Storm oplossen met behulp van Azure HDInsight

Meer informatie over de meest voorkomende problemen en hun oplossingen voor het werken met [Apache Storm](https://storm.apache.org/) nettoladingen in [Apache Ambari](https://ambari.apache.org/).

## <a name="how-do-i-access-the-storm-ui-on-a-cluster"></a>Hoe krijg ik toegang tot de Storm-gebruikersinterface op een cluster?

U hebt twee opties om de Storm-gebruikersinterface vanuit een browser te openen:

### <a name="apache-ambari-ui"></a>Apache Ambari-gebruikers interface

1. Ga naar de Ambari-dashboard.
2. Selecteer in de lijst met services, **Storm**.
3. In de **snelkoppelingen** in het menu **Storm-gebruikersinterface**.

### <a name="direct-link"></a>Directe koppeling

U kunt toegang tot de Storm-gebruikersinterface op de volgende URL:

`https://<cluster DNS name>/stormui`

Voorbeeld: `https://stormcluster.azurehdinsight.net/stormui`

## <a name="how-do-i-transfer-storm-event-hub-spout-checkpoint-information-from-one-topology-to-another"></a>Hoe ik Storm event hub spout controlepunt gegevens van één topologie overbrengen naar een andere?

Bij het ontwikkelen van topologieën die uit Azure Event Hubs gelezen met behulp van de HDInsight Storm event hub spout JAR-bestand, moet u een topologie met dezelfde naam op een nieuw cluster implementeren. Echter, moet u de gegevens van de controlepunten die doorgevoerd naar is bewaren [Apache ZooKeeper](https://zookeeper.apache.org/) op het oude cluster.

### <a name="where-checkpoint-data-is-stored"></a>Waar controlepuntgegevens worden opgeslagen

Controlepuntgegevens voor verschuiving is opgeslagen door de event hub spout in ZooKeeper in twee paden van de hoofdmap:

- Niet-transactionele Spout-controle punten worden opgeslagen in `/eventhubspout`.

- Transactionele Spout-controlepunt gegevens worden opgeslagen in `/transactional`.

### <a name="how-to-restore"></a>Herstellen

Als u de scripts en -bibliotheken die u gebruikt om te exporteren van gegevens buiten ZooKeeper en importeer de gegevens terug naar ZooKeeper met een nieuwe naam, Zie [HDInsight Storm-voorbeelden](https://github.com/hdinsight/hdinsight-storm-examples/tree/master/tools/zkdatatool-1.0).

De bibliotheekmap heeft JAR-bestanden met de implementatie voor de bewerking voor exporteren/importeren. De bash-map bevat een voorbeeldscript dat laat zien hoe u gegevens uit de ZooKeeper-server op het oude cluster exporteren en vervolgens importeren naar de ZooKeeper-server op het nieuwe cluster.

Voer de [stormmeta.sh](https://github.com/hdinsight/hdinsight-storm-examples/blob/master/tools/zkdatatool-1.0/bash/stormmeta.sh) script uit de ZooKeeper-knooppunten om te exporteren en vervolgens gegevens te importeren. Het script bijwerken naar de juiste versie van Hortonworks Data Platform (HDP). (We werken op het maken van deze scripts algemene in HDInsight. Algemene scripts kunnen uitvoeren op elk knooppunt in het cluster zonder wijzigingen door de gebruiker.)

De export opdracht schrijft de meta gegevens naar een Apache Hadoop Distributed File System (HDFS) (in Azure Blob Storage of Azure Data Lake Storage) op een locatie die u hebt ingesteld.

### <a name="examples"></a>Voorbeelden

#### <a name="export-offset-metadata"></a>Offset metagegevens exporteren

1. SSH gebruiken om te gaan naar de ZooKeeper-cluster op het cluster waaruit het controlepunt offset moet worden geëxporteerd.
2. Voer de volgende opdracht uit (nadat u de teken reeks voor de HDP-versie hebt bijgewerkt) om ZooKeeper offset-gegevens te exporteren naar het `/stormmetadta/zkdata` HDFS-pad:

    ```apache
    java -cp ./*:/etc/hadoop/conf/*:/usr/hdp/2.5.1.0-56/hadoop/*:/usr/hdp/2.5.1.0-56/hadoop/lib/*:/usr/hdp/2.5.1.0-56/hadoop-hdfs/*:/usr/hdp/2.5.1.0-56/hadoop-hdfs/lib/*:/etc/failover-controller/conf/*:/etc/hadoop/* com.microsoft.storm.zkdatatool.ZkdataImporter export /eventhubspout /stormmetadata/zkdata
    ```

#### <a name="import-offset-metadata"></a>Offset metagegevens importeren

1. SSH gebruiken om te gaan naar de ZooKeeper-cluster op het cluster waaruit het controlepunt offset moet worden geïmporteerd.
2. Voer de volgende opdracht uit (nadat u de teken reeks voor de HDP-versie hebt bijgewerkt) om ZooKeeper offset gegevens te importeren van het pad HDFS `/stormmetadata/zkdata` naar de ZooKeeper-server op het doel cluster:

    ```apache
    java -cp ./*:/etc/hadoop/conf/*:/usr/hdp/2.5.1.0-56/hadoop/*:/usr/hdp/2.5.1.0-56/hadoop/lib/*:/usr/hdp/2.5.1.0-56/hadoop-hdfs/*:/usr/hdp/2.5.1.0-56/hadoop-hdfs/lib/*:/etc/failover-controller/conf/*:/etc/hadoop/* com.microsoft.storm.zkdatatool.ZkdataImporter import /eventhubspout /home/sshadmin/zkdata
    ```

#### <a name="delete-offset-metadata-so-that-topologies-can-start-processing-data-from-the-beginning-or-from-a-timestamp-that-the-user-chooses"></a>Offset metagegevens verwijderen zodat topologieën kunnen verwerken van gegevens vanaf het begin of in een timestamp die de gebruiker ervoor kiest starten

1. SSH gebruiken om te gaan naar de ZooKeeper-cluster op het cluster waaruit het controlepunt offset moet worden verwijderd.
2. Voer de volgende opdracht (na het bijwerken van de tekenreeks van de versie van de HDP) om alle offset ZooKeeper-gegevens in het huidige cluster te verwijderen:

    ```apache
    java -cp ./*:/etc/hadoop/conf/*:/usr/hdp/2.5.1.0-56/hadoop/*:/usr/hdp/2.5.1.0-56/hadoop/lib/*:/usr/hdp/2.5.1.0-56/hadoop-hdfs/*:/usr/hdp/2.5.1.0-56/hadoop-hdfs/lib/*:/etc/failover-controller/conf/*:/etc/hadoop/* com.microsoft.storm.zkdatatool.ZkdataImporter delete /eventhubspout
    ```

## <a name="how-do-i-locate-storm-binaries-on-a-cluster"></a>Hoe vind ik binaire Storm-bestanden op een cluster?

De binaire Storm-bestanden voor de huidige HDP-stack bevinden zich in `/usr/hdp/current/storm-client`. De locatie is hetzelfde voor hoofdknooppunten en voor worker-knooppunten.

Er zijn mogelijk meerdere binaire bestanden voor specifieke HDP-versies in/usr/HDP (bijvoorbeeld `/usr/hdp/2.5.0.1233/storm`). De map `/usr/hdp/current/storm-client` is symlinked naar de meest recente versie die op het cluster wordt uitgevoerd.

Zie [verbinding maken met een HDInsight-cluster met behulp van SSH](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-linux-use-ssh-unix) en [Apache Storm](https://storm.apache.org/)voor meer informatie.

## <a name="how-do-i-determine-the-deployment-topology-of-a-storm-cluster"></a>Hoe bepaal ik de topologie van de implementatie van een Storm-cluster?

Bepaal eerst alle onderdelen die zijn geïnstalleerd met HDInsight Storm. Een Storm-cluster bestaat uit vier categorieën van knooppunt:

* Gateway-knooppunten
* Hoofdknooppunten
* ZooKeeper-knooppunten
* Werkknooppunten

### <a name="gateway-nodes"></a>Gateway-knooppunten

Een gateway-knooppunt is een gateway en de reverse proxyservice waarmee u openbare toegang tot een actieve Ambari management-service. Ook verwerkt deze Ambari selectie van leider.

### <a name="head-nodes"></a>Hoofdknooppunten

Storm-hoofdknooppunten, voer de volgende services:
* Nimbus
* Ambari-server
* Metrische gegevens voor Ambari-server
* Ambari metrische gegevens verzamelen
 
### <a name="zookeeper-nodes"></a>ZooKeeper-knooppunten

HDInsight wordt geleverd met een ZooKeeper-quorum drie knooppunten. De grootte van het quorum is vast en kan niet opnieuw worden geconfigureerd.

Storm-services in het cluster zijn geconfigureerd voor het gebruik van automatisch de ZooKeeper-quorum.

### <a name="worker-nodes"></a>Werkknooppunten

Storm worker-knooppunten uitvoeren van de volgende services:
* Supervisor
* Werknemer Java virtual machines (JVMs), voor het actieve topologieën
* Ambari-agent

## <a name="how-do-i-locate-storm-event-hub-spout-binaries-for-development"></a>Hoe vind ik de binaire Storm event hub spout-bestanden voor ontwikkeling?

Zie de volgende bronnen voor meer informatie over het gebruik van Storm event hub spout JAR-bestanden met uw topologie.

### <a name="java-based-topology"></a>Op basis van een Java-topologie

[Process events from Azure Event Hubs met Apache Storm op HDInsight (Java) gebeurtenissen](https://github.com/Azure-Samples/hdinsight-java-storm-eventhub)

### <a name="c-based-topology-mono-on-hdinsight-34-linux-storm-clusters"></a>C#-topologie (Mono op Storm voor HDInsight 3.4 + Linux-clusters) op basis van

[Verwerken van gebeurtenissen uit Azure Event Hubs met Apache Storm op HDInsight (C#)](https://docs.microsoft.com/azure/hdinsight/hdinsight-storm-develop-csharp-event-hub-topology)

### <a name="latest-apache-storm-event-hub-spout-binaries-for-hdinsight-35-linux-storm-clusters"></a>Meest recente Apache Storm event hub spout binaire bestanden voor HDInsight 3.5 + Linux Storm-clusters

Zie het [Leesmij-bestand voor MVN-opslag plaats](https://github.com/hdinsight/mvn-repo/blob/master/README.md)voor meer informatie over het gebruik van de nieuwste Storm Event hub Spout die werkt met HDInsight 3.5-en Linux Storm-clusters.

### <a name="source-code-examples"></a>Bron-codevoorbeelden

Zie [voorbeelden](https://github.com/Azure-Samples/hdinsight-java-storm-eventhub) van lezen en schrijven uit Azure Event Hub met behulp van een Apache Storm-topologie (geschreven in Java) op een Azure HDInsight-cluster.

## <a name="how-do-i-locate-storm-log4j-2-configuration-files-on-clusters"></a>Hoe vind ik de Storm Log4J-2-configuratiebestanden op clusters?

Om te identificeren [Apache Log4j 2](https://logging.apache.org/log4j/2.x/) configuratiebestanden voor Storm-services.

### <a name="on-head-nodes"></a>Op de hoofdknooppunten

De configuratie van de Nimbus Log4J wordt gelezen uit `/usr/hdp/\<HDP version>/storm/log4j2/cluster.xml`.

### <a name="on-worker-nodes"></a>Op de worker-knooppunten

De configuratie van de Super Visor Log4J wordt gelezen uit `/usr/hdp/\<HDP version>/storm/log4j2/cluster.xml`.

Het configuratie bestand van de worker-Log4J wordt gelezen uit `/usr/hdp/\<HDP version>/storm/log4j2/worker.xml`.

Voor beelden: `/usr/hdp/2.6.0.2-76/storm/log4j2/cluster.xml`
`/usr/hdp/2.6.0.2-76/storm/log4j2/worker.xml`

---

## <a name="not-a-leader-exception"></a>Geen uitzonde ring voor Leader

Bij het verzenden van een topologie kan een fout bericht van de gebruiker worden weer gegeven die er ongeveer als volgt uitziet: `Topology submission exception, cause not a leader, the current leader is NimbusInfo`.

Om dit probleem op te lossen moet de gebruiker mogelijk een ticket indienen om de knoop punten opnieuw te starten of opnieuw op te starten. Zie [https://community.hortonworks.com/content/supportkb/150287/error-ignoring-exception-while-trying-to-get-leade.html](https://community.hortonworks.com/content/supportkb/150287/error-ignoring-exception-while-trying-to-get-leade.html)voor meer informatie.

---

## <a name="next-steps"></a>Volgende stappen

Als u het probleem niet ziet of als u het probleem niet kunt oplossen, gaat u naar een van de volgende kanalen voor meer ondersteuning:

- Krijg antwoorden van Azure-experts via de [ondersteuning van Azure Community](https://azure.microsoft.com/support/community/).

- Maak verbinding met [@AzureSupport](https://twitter.com/azuresupport) -het officiële Microsoft Azure account voor het verbeteren van de gebruikers ervaring. Verbinding maken met de Azure-community met de juiste resources: antwoorden, ondersteuning en experts.

- Als u meer hulp nodig hebt, kunt u een ondersteunings aanvraag indienen via de [Azure Portal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/). Selecteer **ondersteuning** in de menu balk of open de hub **Help en ondersteuning** . Lees [hoe u een ondersteunings aanvraag voor Azure kunt maken](https://docs.microsoft.com/azure/azure-supportability/how-to-create-azure-support-request)voor meer informatie. Toegang tot abonnementsbeheer en factuurbeheer is in uw Microsoft Azure-abonnement inbegrepen, en technische ondersteuning wordt verstrekt via een van de [Azure-ondersteuningsplannen](https://azure.microsoft.com/support/plans/).
