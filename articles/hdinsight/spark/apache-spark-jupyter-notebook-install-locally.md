---
title: Jupyter lokaal installeren en verbinding maken met Spark in azure HDInsight
description: Meer informatie over hoe u Jupyter notebook lokaal installeert op uw computer en hoe u het kunt verbinden met een Apache Spark-cluster.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 11/07/2019
ms.openlocfilehash: 225ee7028b9610a4974f9bee05da667d78d3355e
ms.sourcegitcommit: bc193bc4df4b85d3f05538b5e7274df2138a4574
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/10/2019
ms.locfileid: "73903737"
---
# <a name="install-jupyter-notebook-on-your-computer-and-connect-to-apache-spark-on-hdinsight"></a>Jupyter notebook op uw computer installeren en verbinding maken met Apache Spark op HDInsight

In dit artikel leert u hoe u Jupyter notebook installeert met de aangepaste PySpark (voor python) en Apache Spark (voor scala)-kernels met Spark Magic en de notebook verbindt met een HDInsight-cluster. Er kunnen verschillende redenen zijn om Jupyter te installeren op uw lokale computer en er kunnen ook enkele uitdagingen zijn. Zie voor meer informatie de sectie [Waarom moet ik Jupyter installeren op mijn computer](#why-should-i-install-jupyter-on-my-computer) aan het einde van dit artikel.

Er zijn vier belang rijke stappen bij het installeren van Jupyter en het verbinden met Apache Spark op HDInsight.

* Spark-cluster configureren.
* Installeer Jupyter-notebook.
* Installeer de PySpark-en Spark-kernels met Spark Magic.
* Spark Magic configureren voor toegang tot het Spark-cluster in HDInsight.

Zie [kernels die beschikbaar zijn voor Jupyter-notebooks met Apache Spark Linux-clusters in hdinsight](apache-spark-jupyter-notebook-kernels.md)voor meer informatie over de aangepaste kernels en de Spark Magic die beschikbaar is voor Jupyter-notebooks met hdinsight-cluster.

## <a name="prerequisites"></a>Vereisten

* Een Apache Spark-cluster in HDInsight. Zie [Apache Spark-clusters maken in Azure HDInsight](apache-spark-jupyter-spark-sql.md) voor instructies. Dit is een vereiste om de Jupyter-notebook te koppelen aan een HDInsight-cluster zodra het notitie blok is geïnstalleerd.

* Weten hoe u Jupyter Notebook gebruikt met Spark on HDInsight.

## <a name="install-jupyter-notebook-on-your-computer"></a>Jupyter notebook op uw computer installeren

U moet python installeren voordat u Jupyter-notebooks kunt installeren. De [Anaconda-distributie](https://www.anaconda.com/download/) installeert zowel Python als Jupyter notebook.

Down load het [Anaconda-installatie programma](https://www.anaconda.com/download/) voor uw platform en voer de installatie uit. Zorg er tijdens het uitvoeren van de installatie wizard voor dat u de optie selecteert om Anaconda toe te voegen aan uw padvariabele.  Zie ook [Jupyter installeren met behulp van Anaconda](https://jupyter.readthedocs.io/en/latest/install.html).

## <a name="install-spark-magic"></a>Spark Magic installeren

1. Voer een van de volgende opdrachten in om Spark Magic te installeren. Zie ook [sparkmagic-documentatie](https://github.com/jupyter-incubator/sparkmagic#installation).

    |Cluster versie | Installatie opdracht |
    |---|---|
    |v 3.6 en v 3.5 |`pip install sparkmagic==0.13.1`|
    |v 3.4|`pip install sparkmagic==0.2.3`|

1. Zorg ervoor dat `ipywidgets` juist is geïnstalleerd door de volgende opdracht uit te voeren:

    ```cmd
    jupyter nbextension enable --py --sys-prefix widgetsnbextension
    ```

## <a name="install-pyspark-and-spark-kernels"></a>PySpark-en Spark-kernels installeren

1. Geef aan waar `sparkmagic` is geïnstalleerd door de volgende opdracht in te voeren:

    ```cmd
    pip show sparkmagic
    ```

    Wijzig vervolgens uw werkmap in de locatie die is geïdentificeerd met de bovenstaande opdracht.

1. Voer in de nieuwe werkmap een of meer van de onderstaande opdrachten in om de gewenste kernel (s) te installeren:

    |Kerneldebugger | Opdracht |
    |---|---|
    |Spark|`jupyter-kernelspec install sparkmagic/kernels/sparkkernel`|
    |SparkR|`jupyter-kernelspec install sparkmagic/kernels/sparkrkernel`|
    |PySpark|`jupyter-kernelspec install sparkmagic/kernels/pysparkkernel`|
    |PySpark3|`jupyter-kernelspec install sparkmagic/kernels/pyspark3kernel`|

1. Optioneel. Voer de onderstaande opdracht in om de server extensie in te scha kelen:

    ```cmd
    jupyter serverextension enable --py sparkmagic
    ```

## <a name="configure-spark-magic-to-connect-to-hdinsight-spark-cluster"></a>Spark Magic configureren om verbinding te maken met een HDInsight Spark-cluster

In deze sectie configureert u de Spark Magic die u eerder hebt geïnstalleerd om verbinding te maken met een Apache Spark-cluster.

1. Start de python-shell met de volgende opdracht:

    ```cmd
    python
    ```

2. De Jupyter-configuratie gegevens worden doorgaans opgeslagen in de basismap van de gebruiker. Voer de volgende opdracht in om de basismap aan te duiden en maak een map met de naam **. sparkmagic**.  Het volledige pad wordt gegenereerd.

    ```python
    import os
    path = os.path.expanduser('~') + "\\.sparkmagic"
    os.makedirs(path)
    print(path)
    exit()
    ```

3. Maak in de map `.sparkmagic`een bestand met de naam **config. json** en voeg het volgende JSON-fragment daarin toe.  

    ```json
    {
      "kernel_python_credentials" : {
        "username": "{USERNAME}",
        "base64_password": "{BASE64ENCODEDPASSWORD}",
        "url": "https://{CLUSTERDNSNAME}.azurehdinsight.net/livy"
      },

      "kernel_scala_credentials" : {
        "username": "{USERNAME}",
        "base64_password": "{BASE64ENCODEDPASSWORD}",
        "url": "https://{CLUSTERDNSNAME}.azurehdinsight.net/livy"
      },

      "custom_headers" : {
        "X-Requested-By": "livy"
      },

      "heartbeat_refresh_seconds": 5,
      "livy_server_heartbeat_timeout_seconds": 60,
      "heartbeat_retry_seconds": 1
    }
    ```

4. Voer de volgende bewerkingen uit in het bestand:

    |Sjabloon waarde | Nieuwe waarde |
    |---|---|
    |GEBRUIKERS|Cluster aanmelding, standaard is `admin`.|
    |CLUSTERDNSNAME|Clusternaam|
    |{BASE64ENCODEDPASSWORD}|Een base64-gecodeerd wacht woord voor uw werkelijke wacht woord.  U kunt een base64-wacht woord genereren op [https://www.url-encode-decode.com/base64-encode-decode/](https://www.url-encode-decode.com/base64-encode-decode/).|
    |`"livy_server_heartbeat_timeout_seconds": 60`|Bewaar het gebruik van `sparkmagic 0.12.7` (clusters v 3.5 en v 3.6).  Als u `sparkmagic 0.2.3` (clusters v 3.4) gebruikt, vervangt u door `"should_heartbeat": true`.|

    U kunt een volledig voorbeeld bestand zien bij [voorbeeld configuratie. json](https://github.com/jupyter-incubator/sparkmagic/blob/master/sparkmagic/example_config.json).

   > [!TIP]  
   > Er worden heartbeats verzonden om ervoor te zorgen dat sessies niet worden gelekt. Wanneer een computer naar de slaap stand gaat of wordt uitgeschakeld, wordt de heartbeat niet verzonden, waardoor de sessie wordt opgeschoond. Als u dit gedrag wilt uitschakelen, kunt u voor clusters v 3.4 de livy-configuratie `livy.server.interactive.heartbeat.timeout` instellen op `0` vanuit de Ambari-gebruikers interface. Als u de 3,5-configuratie hierboven niet instelt voor clusters v 3.5, wordt de sessie niet verwijderd.

5. Jupyter starten. Gebruik de volgende opdracht vanaf de opdracht prompt.

    ```cmd
    jupyter notebook
    ```

6. Controleer of u de Spark Magic kunt gebruiken die beschikbaar is bij de kernels. Voer de volgende stappen uit.

    a. Maak een nieuwe notebook. Selecteer in de rechter bovenhoek de optie **Nieuw**. De standaard-kernel **python 2** of **python 3** en de kernels die u hebt geïnstalleerd, worden weer geven. De werkelijke waarden kunnen variëren, afhankelijk van uw installatie opties.  Selecteer **PySpark**.

    ![Beschik bare kernels in Jupyter notebook](./media/apache-spark-jupyter-notebook-install-locally/jupyter-kernels-notebook.png "Kernels in Jupyter notebook")

    > [!IMPORTANT]  
    > Nadat u **nieuwe** beoordeling van uw shell hebt geselecteerd, zijn er fouten opgetreden.  Als u de fout `TypeError: __init__() got an unexpected keyword argument 'io_loop'` ziet u mogelijk een bekend probleem met bepaalde versies van Tornado.  Als dit het geval is, stopt u de kernel en downgradet u de Tornado-installatie met de volgende opdracht: `pip install tornado==4.5.3`.

    b. Voer het volgende code fragment uit.

    ```sql
    %%sql
    SELECT * FROM hivesampletable LIMIT 5
    ```  

    Als u de uitvoer kunt ophalen, wordt de verbinding met het HDInsight-cluster getest.

    Als u de notitieblok configuratie wilt bijwerken om verbinding te maken met een ander cluster, werkt u het bestand config. json bij met de nieuwe set waarden, zoals wordt weer gegeven in stap 3.

## <a name="why-should-i-install-jupyter-on-my-computer"></a>Waarom moet ik Jupyter installeren op mijn computer?

Er kunnen verschillende redenen zijn waarom u Jupyter op uw computer wilt installeren en vervolgens verbinding wilt maken met een Apache Spark cluster op HDInsight.

* Hoewel er al Jupyter-notebooks beschikbaar zijn in het Spark-cluster in azure HDInsight, is het installeren van Jupyter op uw computer de mogelijkheid om uw notitie blokken lokaal te maken, uw toepassing te testen op een actief cluster en vervolgens de notebooks naar het cluster. Als u de notitie blokken naar het cluster wilt uploaden, kunt u deze uploaden met behulp van de Jupyter-notebook met of het cluster of deze opslaan in de map/HdiNotebooks in het opslag account dat aan het cluster is gekoppeld. Zie [waar worden Jupyter-notebooks opgeslagen](apache-spark-jupyter-notebook-kernels.md#where-are-the-notebooks-stored)? voor meer informatie over hoe notitie blokken op het cluster worden opgeslagen.
* Met de notitie blokken die lokaal beschikbaar zijn, kunt u verbinding maken met verschillende Spark-clusters op basis van uw toepassings vereiste.
* U kunt GitHub gebruiken om een broncode beheer systeem te implementeren en versie beheer voor de notitie blokken te hebben. U kunt ook een samenwerkings omgeving hebben waarin meerdere gebruikers met hetzelfde notitie blok kunnen werken.
* U kunt lokaal met notitie blokken werken zonder dat u zelf een cluster hebt. U hebt alleen een cluster nodig om uw notitie blokken te testen, zodat u uw notitie blokken of een ontwikkel omgeving niet hand matig kunt beheren.
* Het is eenvoudiger om uw eigen lokale ontwikkel omgeving te configureren dan het configureren van de Jupyter-installatie op het cluster.  U kunt profiteren van alle software die u lokaal hebt geïnstalleerd zonder dat u een of meer externe clusters hebt geconfigureerd.

> [!WARNING]  
> Als Jupyter is geïnstalleerd op uw lokale computer, kunnen meerdere gebruikers tegelijkertijd hetzelfde notebook op hetzelfde Spark-cluster uitvoeren. In een dergelijke situatie worden meerdere livy-sessies gemaakt. Als u een probleem ondervindt en u fouten wilt opsporen, is dit een complexe taak om bij te houden welke livy-sessie bij welke gebruiker hoort.  

## <a name="next-steps"></a>Volgende stappen

* [Overzicht: Apache Spark in Azure HDInsight](apache-spark-overview.md)
* [Apache Spark met BI: interactieve gegevens analyses uitvoeren met behulp van Spark in HDInsight met BI-hulpprogram ma's](apache-spark-use-bi-tools.md)
* [Apache Spark met Machine Learning: Spark in HDInsight gebruiken voor het analyseren van de gebouw temperatuur met behulp van HVAC-gegevens](apache-spark-ipython-notebook-machine-learning.md)
