---
title: Apache Hive bibliotheken tijdens het maken van het cluster-Azure HDInsight
description: Meer informatie over het toevoegen van Apache Hive bibliotheken (JAR-bestanden) aan een HDInsight-cluster tijdens het maken van het cluster.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.custom: H1Hack27Feb2017,hdinsightactive
ms.date: 12/23/2019
ms.openlocfilehash: 57b4440a29dde470f91bbaae091bf65a0d2a1b51
ms.sourcegitcommit: ec2eacbe5d3ac7878515092290722c41143f151d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/31/2019
ms.locfileid: "75552267"
---
# <a name="add-custom-apache-hive-libraries-when-creating-your-hdinsight-cluster"></a>Aangepaste Apache Hive bibliotheken toevoegen bij het maken van uw HDInsight-cluster

Meer informatie over het vooraf laden van [Apache Hive](https://hive.apache.org/) bibliotheken op HDInsight. Dit document bevat informatie over het gebruik van een script actie voor het vooraf laden van bibliotheken tijdens het maken van het cluster. Bibliotheken die zijn toegevoegd aan de hand van de stappen in dit document, zijn wereld wijd beschikbaar in Hive-er hoeft geen [jar](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+Cli) te worden gebruikt om ze te laden.

## <a name="how-it-works"></a>Het werkt als volgt

Wanneer u een cluster maakt, kunt u een script actie gebruiken om cluster knooppunten te wijzigen wanneer ze worden gemaakt. In het script in dit document wordt één para meter geaccepteerd. Dit is de locatie van de bibliotheken. Deze locatie moet zich in een Azure Storage-account bevinden en de bibliotheken moeten worden opgeslagen als jar-bestanden.

Tijdens het maken van het cluster worden de bestanden door het script geïnventariseerd, worden deze gekopieerd naar de map `/usr/lib/customhivelibs/` op de hoofd-en worker-knoop punten. vervolgens worden deze toegevoegd aan de eigenschap `hive.aux.jars.path` in het `core-site.xml`-bestand. Op Linux-gebaseerde clusters werkt ook het `hive-env.sh` bestand bij met de locatie van de bestanden.

Met de script actie in dit artikel maakt u de bibliotheken beschikbaar wanneer u een Hive-client gebruikt voor **WebHCat**en **HiveServer2**.

## <a name="the-script"></a>Het script

**Locatie van script**

[https://hdiconfigactions.blob.core.windows.net/setupcustomhivelibsv01/setup-customhivelibs-v01.ps1](https://hdiconfigactions.blob.core.windows.net/setupcustomhivelibsv01/setup-customhivelibs-v01.ps1)

**Vereisten**

* De scripts moeten worden toegepast op zowel de **hoofd knooppunten** als de **worker-knoop punten**.

* De potten die u wilt installeren, moeten worden opgeslagen in Azure Blob Storage in **één container**.

* Het opslag account met de bibliotheek van jar-bestanden **moet** worden gekoppeld aan het HDInsight-cluster tijdens het maken. Dit moet het standaard opslag account zijn of een account dat is toegevoegd via de instellingen voor het __opslag account__.

* Het WASB-pad naar de container moet worden opgegeven als een para meter voor de script actie. Als de potten bijvoorbeeld worden opgeslagen in een container met de naam **bibliotheken** op een opslag account met de naam **mijn opslag**, wordt de para meter `wasbs://libs@mystorage.blob.core.windows.net/`.

  > [!NOTE]  
  > In dit document wordt ervan uitgegaan dat u al een opslag account, Blob-container hebt gemaakt en de bestanden erin hebt geladen.
  >
  > Als u nog geen opslag account hebt gemaakt, kunt u dit doen via de [Azure Portal](https://portal.azure.com). U kunt vervolgens een hulp programma zoals [Azure Storage Explorer](https://storageexplorer.com/) gebruiken om een container in het account te maken en bestanden te uploaden.

## <a name="create-a-cluster-using-the-script"></a>Een cluster maken met behulp van het script

1. Begin met het inrichten van een cluster met behulp van de stappen bij het [inrichten van HDInsight-clusters in Linux](hdinsight-hadoop-provision-linux-clusters.md), maar het inrichten niet volt ooien. U kunt ook Azure PowerShell of de HDInsight .NET SDK gebruiken om een cluster te maken met behulp van dit script. Zie [HDInsight-clusters aanpassen met script acties](hdinsight-hadoop-customize-cluster-linux.md)voor meer informatie over het gebruik van deze methoden. Voor de Azure Portal, moet u de optie **Ga naar klassieke ervaring maken** selecteren en vervolgens **aangepast (grootte, instellingen, apps)** .

1. Als het opslag account met de bibliotheek van **jar-bestanden**anders is dan het account dat voor het cluster wordt gebruikt, moet u **extra opslag accounts**volt ooien.

1. Geef voor **script acties**de volgende informatie op:

    |Eigenschap |Waarde |
    |---|---|
    |Script type|-Aangepast|
    |Name|Bibliotheken |
    |Bash-script-URI|`https://hdiconfigactions.blob.core.windows.net/linuxsetupcustomhivelibsv01/setup-customhivelibs-v01.sh`|
    |Knooppunt type (n)|Hoofd, werk nemer|
    |Parameters|Voer het WASB-adres in voor de container en het opslag account dat de potten bevat. Bijvoorbeeld `wasbs://libs@mystorage.blob.core.windows.net/`.|

1. Ga door met het inrichten van het cluster zoals beschreven in [HDInsight-clusters inrichten op Linux](hdinsight-hadoop-provision-linux-clusters.md).

Nadat het maken van het cluster is voltooid, kunt u de potten die via dit script zijn toegevoegd, uit de Hive gebruiken zonder de `ADD JAR`-instructie te gebruiken.

## <a name="next-steps"></a>Volgende stappen

Zie [Apache Hive gebruiken met HDInsight](hadoop/hdinsight-use-hive.md) voor meer informatie over het werken met hive
