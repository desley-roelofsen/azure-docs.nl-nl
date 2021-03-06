---
title: Heap-dumps inschakelen voor Apache Hadoop Services op HDInsight-Azure
description: Schakel heap-dumps in voor Apache Hadoop services van HDInsight-clusters op basis van Linux voor fout opsporing en analyse.
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 02/27/2018
ms.author: hrasheed
ms.openlocfilehash: 90de0b4bfad4c5096ebc38eb3d31fc41bca6649b
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/04/2019
ms.locfileid: "73494855"
---
# <a name="enable-heap-dumps-for-apache-hadoop-services-on-linux-based-hdinsight"></a>Heap-dumps inschakelen voor Apache Hadoop Services op HDInsight op basis van Linux

[!INCLUDE [heapdump-selector](../../includes/hdinsight-selector-heap-dump.md)]

Heap-dumps bevatten een moment opname van het geheugen van de toepassing, met inbegrip van de waarden van variabelen op het moment dat de dump werd gemaakt. Ze zijn dus handig voor het vaststellen van problemen die zich tijdens runtime voordoen.

## <a name="whichServices"></a>Onderzoeksservices

U kunt heap-dumps inschakelen voor de volgende services:

* **Apache hcatalog** -Tempelton
* **Apache Hive** -hiveserver2, meta Store, derbyserver
* **MapReduce** -jobhistoryserver
* **Apache garens** -Resource Manager, nodemanager, timelineserver
* **Apache hdfs** -DataNode, secondarynamenode, namenode

U kunt ook heap-dumps inschakelen voor de kaart en processen verminderen die door HDInsight worden uitgevoerd.

## <a name="configuration"></a>Informatie over heap-dump configuratie

Heap-dumps worden ingeschakeld door de opties voor het door geven (ook wel "kiest of para meters" genoemd) aan de JVM toe te staan wanneer een service wordt gestart. Voor de meeste [Apache Hadoop](https://hadoop.apache.org/) Services kunt u het shell script dat wordt gebruikt om de service te starten, zodanig wijzigen dat deze opties worden door gegeven.

In elk script is er een export voor **\*\_kiest**, die de opties bevat die zijn door gegeven aan de JVM. In het **Hadoop-env.sh** -script bevat de regel die begint met `export HADOOP_NAMENODE_OPTS=` bijvoorbeeld de opties voor de NameNode-service.

Het toewijzen en verminderen van processen is iets anders, omdat deze bewerkingen een onderliggend proces van de MapReduce-service zijn. Elke kaart of vermindert proces wordt uitgevoerd in een onderliggende container en er zijn twee vermeldingen die de JVM-opties bevatten. Beide zijn opgenomen in **mapred-site. XML**:

* **MapReduce. admin. map. Child. java. kiest**
* **MapReduce. admin. reduce. Child. java. kiest**

> [!NOTE]  
> We raden u aan [Apache Ambari](https://ambari.apache.org/) te gebruiken om de instellingen voor scripts en mapred-site. XML te wijzigen, omdat Ambari de replicatie van wijzigingen tussen knoop punten in het cluster moet afhandelen. Zie de sectie [Apache Ambari gebruiken](#using-apache-ambari) voor specifieke stappen.

### <a name="enable-heap-dumps"></a>Heapdumps inschakelen

Met de volgende optie worden heap-dumps ingeschakeld wanneer er een OutOfMemoryError optreedt:

    -XX:+HeapDumpOnOutOfMemoryError

De **+** geeft aan dat deze optie is ingeschakeld. Uitgeschakeld is de standaardinstelling.

> [!WARNING]  
> Heap-services worden niet standaard ingeschakeld op HDInsight wanneer de dump bestanden groot kunnen zijn. Als u deze optie inschakelt voor het oplossen van problemen, vergeet dan niet om deze uit te scha kelen wanneer u het probleem hebt verveelvoudigd en de dump bestanden hebt verzameld.

### <a name="dump-location"></a>Dump locatie

De standaard locatie voor het dump bestand is de huidige werkmap. U kunt bepalen waar het bestand wordt opgeslagen met behulp van de volgende optie:

    -XX:HeapDumpPath=/path

Als u bijvoorbeeld `-XX:HeapDumpPath=/tmp` gebruikt, worden de dumps opgeslagen in de map/tmp-map.

### <a name="scripts"></a>Scripts

U kunt ook een script activeren als er een **OutOfMemoryError** optreedt. U kunt bijvoorbeeld een melding activeren, zodat u weet dat de fout is opgetreden. Gebruik de volgende optie om een script op een __OutOfMemoryError__te starten:

    -XX:OnOutOfMemoryError=/path/to/script

> [!NOTE]  
> Omdat Apache Hadoop een gedistribueerd systeem is, moet elk gebruikt script op alle knoop punten in het cluster worden geplaatst waarop de-service wordt uitgevoerd.
> 
> Het script moet zich ook bevindt op een locatie die toegankelijk is voor het account waarop de service wordt uitgevoerd, en moet machtigingen voor uitvoeren opgeven. U kunt bijvoorbeeld scripts opslaan in `/usr/local/bin` en `chmod go+rx /usr/local/bin/filename.sh` gebruiken om machtigingen voor lezen en uitvoeren te verlenen.

## <a name="using-apache-ambari"></a>Apache Ambari gebruiken

Als u de configuratie voor een service wilt wijzigen, gebruikt u de volgende stappen:

1. Open de Ambari-webgebruikersinterface voor uw cluster. De URL is https://YOURCLUSTERNAME.azurehdinsight.net.

    Wanneer u hierom wordt gevraagd, moet u zich bij de site aanmelden met de HTTP-account naam (standaard: beheerder) en het wacht woord voor uw cluster.

   > [!NOTE]  
   > U wordt mogelijk een tweede keer gevraagd door Ambari voor de gebruikers naam en het wacht woord. Als dit het geval is, voert u dezelfde account naam en hetzelfde wacht woord in.

2. Selecteer in de lijst met aan de linkerkant het service gebied dat u wilt wijzigen. Bijvoorbeeld **HDFS**. Selecteer in het middelste gebied het tabblad **configuratie** .

    ![Afbeelding van het tabblad Ambari web met HDFS-configuraties geselecteerd](./media/hdinsight-hadoop-collect-debug-heap-dump-linux/hdi-service-config-tab.png)

3. Voer in het veld **filter...** de waarde **kiest**. Alleen items met deze tekst worden weer gegeven.

    ![Gefilterde lijst Apache Ambari config](./media/hdinsight-hadoop-collect-debug-heap-dump-linux/hdinsight-filter-list.png)

4. Zoek de vermelding **\*\_** veld voor de service waarvoor u de opslag van heap-dumps wilt inschakelen en voeg de opties toe die u wilt inschakelen. In de volgende afbeelding hebt u `-XX:+HeapDumpOnOutOfMemoryError -XX:HeapDumpPath=/tmp/` toegevoegd aan de **HADOOP-\_NAMENODE\_** veld veld:

    ![Apache Ambari Hadoop-namenode-kiest](./media/hdinsight-hadoop-collect-debug-heap-dump-linux/hadoop-namenode-opts.png)

   > [!NOTE]  
   > Wanneer u heap-dumps inschakelt voor de kaart of een onderliggend proces verminderen, zoekt u naar de velden met de naam **MapReduce. admin. map. Child. java. kiest** en **MapReduce. admin. reduce. Child. java. kiest**.

    Gebruik de knop **Opslaan** om de wijzigingen op te slaan. U kunt een korte notitie invoeren waarin de wijzigingen worden beschreven.

5. Zodra de wijzigingen zijn toegepast, wordt het pictogram **opnieuw opstarten vereist** weer gegeven naast een of meer services.

    ![pictogram opnieuw opstarten vereist en opnieuw starten](./media/hdinsight-hadoop-collect-debug-heap-dump-linux/restart-required-icon.png)

6. Selecteer elke service die opnieuw moet worden opgestart en gebruik de knop **service acties** om de **onderhouds modus in te scha kelen**. Onderhouds modus voor komt dat waarschuwingen worden gegenereerd van de service wanneer u deze opnieuw opstart.

    ![Menu onderhouds modus HDI inschakelen](./media/hdinsight-hadoop-collect-debug-heap-dump-linux/hdi-maintenance-mode.png)

7. Wanneer u de onderhouds modus hebt ingeschakeld, gebruikt u de knop **opnieuw opstarten** zodat de service **alle gevolgen opnieuw start**

    ![Apache Ambari alle betrokken vermeldingen opnieuw opstarten](./media/hdinsight-hadoop-collect-debug-heap-dump-linux/hdi-restart-all-button.png)

   > [!NOTE]  
   > De vermeldingen voor de knop **opnieuw opstarten** kunnen verschillen voor andere services.

8. Zodra de services opnieuw zijn opgestart, gebruikt u de knop **service acties** om de **onderhouds modus uit te scha kelen**. Deze Ambari om de bewaking van waarschuwingen voor de service te hervatten.

