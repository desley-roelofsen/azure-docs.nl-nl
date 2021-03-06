---
title: 'Azure-toolkit voor Eclipse: scala-apps maken voor HDInsight Spark'
description: Gebruik HDInsight-Hulpprogram Ma's in Azure-toolkit voor Eclipse om Spark-toepassingen te ontwikkelen die zijn geschreven in scala en deze rechtstreeks vanuit de eclips-IDE naar een HDInsight Spark-cluster te verzenden.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.custom: hdinsightactive
ms.date: 12/13/2019
ms.openlocfilehash: 6c82b8ce591bbacb6bf790c8b38635310b33263d
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/25/2019
ms.locfileid: "75435345"
---
# <a name="use-azure-toolkit-for-eclipse-to-create-apache-spark-applications-for-an-hdinsight-cluster"></a>Azure-toolkit voor Eclipse gebruiken om Apache Spark-toepassingen voor een HDInsight-cluster te maken

Gebruik HDInsight-Hulpprogram Ma's in azure Toolkit voor een [eclips](https://www.eclipse.org/) om [Apache Spark](https://spark.apache.org/) toepassingen te ontwikkelen die in [scala](https://www.scala-lang.org/) zijn geschreven en verzend deze rechtstreeks vanuit de eclips IDE naar een Azure HDInsight Spark-cluster. U kunt de HDInsight-Hulpprogram Ma's op een aantal verschillende manieren gebruiken:

* Voor het ontwikkelen en verzenden van een scala Spark-toepassing op een HDInsight Spark-cluster.
* Om toegang te krijgen tot uw Azure HDInsight Spark cluster resources.
* Om een scala Spark-toepassing lokaal te ontwikkelen en uit te voeren.

## <a name="prerequisites"></a>Vereisten

* Apache Spark cluster op HDInsight. Zie [Apache Spark-clusters maken in Azure HDInsight](apache-spark-jupyter-spark-sql.md) voor instructies.

* [Jdk-versie 8 (Java Developer Kit)](https://aka.ms/azure-jdks).

* [Eclips IDE](https://www.eclipse.org/downloads/). In dit artikel wordt gebruikgemaakt van de eclips IDE voor Java-Ontwikkel aars.

## <a name="install-required-plug-ins"></a>Vereiste invoeg toepassingen installeren

### <a name="install-azure-toolkit-for-eclipse"></a>De Azure Toolkit voor Eclipse installeren

Zie [Azure-Toolkit voor eclipse installeren](https://docs.microsoft.com/azure/java/eclipse/azure-toolkit-for-eclipse-installation)voor installatie-instructies.

### <a name="install-the-scala-plug-in"></a>De scala-invoeg toepassing installeren

Wanneer u eclips opent, detecteert HDInsight-Hulpprogram Ma's automatisch of u de scala-invoeg toepassing hebt geïnstalleerd. Selecteer **OK** om door te gaan en volg de instructies voor het installeren van de invoeg toepassing vanuit de eclips Marketplace. Start de IDE opnieuw nadat de installatie is voltooid.

![Automatische installatie van de scala-invoeg toepassing](./media/apache-spark-eclipse-tool-plugin/auto-installation-scala1.png)

### <a name="confirm-plug-ins"></a>Invoeg toepassingen bevestigen

1. Ga naar **Help** > **Marketplace voor de eclips...** .

1. Selecteer het tabblad **geïnstalleerd** .

1. U ziet ten minste het volgende:
    * Azure-toolkit voor Eclipse \<versie >.
    * Scala IDE \<versie >.

## <a name="sign-in-to-your-azure-subscription"></a>Meld u aan bij uw Azure-abonnement

1. Start eclips IDE.

1. Ga naar **venster** om **weer gave** >  weer te geven > **andere...**  > zich aan te **melden.** ..

1. Ga in het dialoog venster **weer gave weer geven** naar **Azure** > **Azure Verkenner**en selecteer **openen**.

   ![Apache Spark-eclips weer gave weer geven](./media/apache-spark-eclipse-tool-plugin/eclipse-view-explorer1.png)

1. Klik in **Azure Verkenner**met de rechter muisknop op het **Azure** -knoop punt en selecteer **Aanmelden**.

1. Kies in het dialoog venster **Aanmelden bij Azure** de verificatie methode, selecteer **Aanmelden**en voltooi het aanmeldings proces.

   ![Apache Spark eclips Azure Sign](./media/apache-spark-eclipse-tool-plugin/eclipse-view-explorer2.png)

1. Nadat u bent aangemeld, worden in het dialoog venster **abonnementen een** lijst weer gegeven met alle Azure-abonnementen die zijn gekoppeld aan de referenties. Druk op **selecteren** om het dialoog venster te sluiten.

   ![Het dialoog venster abonnementen selecteren](./media/apache-spark-eclipse-tool-plugin/Select-Subscriptions.png)

1. Ga in **Azure Verkenner**naar **Azure** >  **HDInsight** om de hdinsight Spark-clusters te bekijken onder uw abonnement.

   ![HDInsight Spark-clusters in azure Explorer3](./media/apache-spark-eclipse-tool-plugin/eclipse-view-explorer3.png)

1. U kunt een cluster naam knooppunt verder uitbreiden om de bronnen (bijvoorbeeld opslag accounts) te zien die zijn gekoppeld aan het cluster.

   ![Een cluster naam uitbreiden om de resources te bekijken](./media/apache-spark-eclipse-tool-plugin/eclipse-view-explorer4.png)

## <a name="link-a-cluster"></a>Een cluster koppelen

U kunt een normaal cluster koppelen met behulp van de beheerde gebruikers naam Ambari. Op dezelfde manier kunt u voor een HDInsight-cluster dat is gekoppeld aan een domein een koppeling maken met behulp van het domein en de gebruikers naam, zoals `user1@contoso.com`.

1. Klik in **Azure Verkenner**met de rechter muisknop op **HDInsight**en selecteer **koppelen van een cluster**.

   ![Menu koppeling van Azure Explorer-cluster](./media/apache-spark-eclipse-tool-plugin/link-a-cluster-context-menu.png)

1. Voer de **cluster naam**, de **gebruikers naam**en het **wacht woord**in en selecteer **OK**. Voer optioneel het opslag account en de opslag sleutel in en selecteer vervolgens opslag container voor Storage Explorer om in de linkernavigatiebalk te werken

   ![Dialoog venster Nieuw HDInsight-cluster koppelen](./media/apache-spark-eclipse-tool-plugin/link-cluster-dialog1.png)

   > [!NOTE]  
   > We gebruiken de gekoppelde opslag sleutel, gebruikers naam en wacht woord als het cluster beide zijn aangemeld bij het Azure-abonnement en een cluster heeft gekoppeld.
   > ![Azure Explorer-opslag accounts](./media/apache-spark-eclipse-tool-plugin/storage-explorer-in-Eclipse.png)
   >
   > Voor de gebruiker alleen toetsen bord, wanneer de huidige focus op de **opslag sleutel**is, moet u op het volgende veld in het dialoog venster op **CTRL + TAB** zijn gericht.

1. U kunt het gekoppelde cluster zien onder **HDInsight**. U kunt nu een toepassing verzenden naar dit gekoppelde cluster.

   ![HDI gekoppeld Azure Explorer-cluster](./media/apache-spark-eclipse-tool-plugin/hdinsight-linked-cluster.png)

1. U kunt ook een cluster ontkoppelen van **Azure Explorer**.

   ![Een cluster dat is gekoppeld aan Azure Verkenner](./media/apache-spark-eclipse-tool-plugin/hdi-unlinked-cluster.png)

## <a name="set-up-a-spark-scala-project-for-an-hdinsight-spark-cluster"></a>Een Spark scala-project instellen voor een HDInsight Spark-cluster

1. Selecteer in de werk ruimte eclips IDE **File** > **New** > **project...** .

1. Selecteer in de wizard **Nieuw project** **HDInsight-project** > **Spark op HDInsight (scala)** . Selecteer vervolgens **Volgende**.

   ![Het Spark in HDInsight-project (scala) selecteren](./media/apache-spark-eclipse-tool-plugin/create-hdi-scala-app-2.png)

1. Geef in het dialoog venster **Nieuw HDInsight scala-project** de volgende waarden op en selecteer **volgende**:
   * Voer een naam in voor het project.
   * Zorg er in het **jre** -gebied voor dat het **gebruik van een uitvoerings omgeving jre** is ingesteld op **Java-1,7** of hoger.
   * In het gebied **Spark-bibliotheek** kunt u **maven gebruiken kiezen voor het configureren van de Spark SDK** -optie.  Ons hulp programma integreert de juiste versie voor de Spark SDK en scala SDK. U kunt ook de optie **Spark SDK hand matig toevoegen** selecteren, downloaden en Spark SDK hand matig toevoegen.

   ![Het dialoog venster Nieuw HDInsight scala-project](./media/apache-spark-eclipse-tool-plugin/create-hdi-scala-app-3.png)

1. Controleer de details in het volgende dialoog venster en selecteer vervolgens **volt ooien**.

## <a name="create-a-scala-application-for-an-hdinsight-spark-cluster"></a>Een scala-toepassing maken voor een HDInsight Spark-cluster

1. Vouw in **pakket Verkenner**het project uit dat u eerder hebt gemaakt. Klik met de rechter muisknop op **src**, selecteer **Nieuw** > **Overige...** .

1. Selecteer in het dialoog venster **een wizard selecteren** de optie **scala wizards** > **scala-object**. Selecteer vervolgens **Volgende**.

   ![Een wizard selecteren een scala-object maken](./media/apache-spark-eclipse-tool-plugin/create-scala-project1.png)

1. Voer in het dialoog venster **nieuw bestand maken** een naam voor het object in en selecteer vervolgens **volt ooien**. Er wordt een tekst editor geopend.

   ![Wizard Nieuw bestand nieuwe bestand maken](./media/apache-spark-eclipse-tool-plugin/create-scala-project2.png)

1. Vervang de huidige inhoud in de tekst editor door de onderstaande code:

    ```scala
    import org.apache.spark.SparkConf
    import org.apache.spark.SparkContext

    object MyClusterApp{
        def main (arg: Array[String]): Unit = {
        val conf = new SparkConf().setAppName("MyClusterApp")
        val sc = new SparkContext(conf)

        val rdd = sc.textFile("wasbs:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv")

        //find the rows that have only one digit in the seventh column in the CSV
        val rdd1 =  rdd.filter(s => s.split(",")(6).length() == 1)

        rdd1.saveAsTextFile("wasbs:///HVACOut")
        }
    }
    ```

1. De toepassing uitvoeren op een HDInsight Spark-cluster:

   a. Klik in Package Explorer met de rechter muisknop op de naam van het project en selecteer vervolgens **Spark-toepassing verzenden naar HDInsight**.

   b. Geef in het dialoog venster **Spark-verzen ding** de volgende waarden op en selecteer vervolgens **verzenden**:

   * Selecteer bij **cluster naam**het HDInsight Spark-cluster waarop u de toepassing wilt uitvoeren.
   * Selecteer een artefact uit het project eclips of selecteer er een uit een harde schijf. De standaard waarde is afhankelijk van het item waarop u met de rechter muisknop klikt in Package Explorer.
   * In de vervolg keuzelijst **naam van hoofd klasse** worden in de wizard verzenden alle object namen van uw project weer gegeven. Selecteer of voer een naam in die u wilt uitvoeren. Als u een artefact van een harde schijf hebt geselecteerd, moet u de naam van de hoofd klasse hand matig invoeren. 
   * Omdat voor de toepassings code in dit voor beeld geen opdracht regel argumenten of verwijzings potten of bestanden zijn vereist, kunt u de resterende tekst vakken leeg laten.

     ![Het dialoog venster Apache Spark inzending](./media/apache-spark-eclipse-tool-plugin/create-scala-project3.png)

1. Het tabblad **verzen ding Spark** moet beginnen met het weer geven van de voortgang. U kunt de toepassing stoppen door de knop rood in het venster **verzen ding Spark** te selecteren. U kunt ook de logboeken voor deze specifieke uitvoering van de toepassing weer geven door het pictogram wereldbol te selecteren (dit wordt aangegeven door het blauwe vak in de afbeelding).

   ![Inzendings venster Apache Spark](./media/apache-spark-eclipse-tool-plugin/create-scala-project4.png)

## <a name="access-and-manage-hdinsight-spark-clusters-by-using-hdinsight-tools-in-azure-toolkit-for-eclipse"></a>HDInsight Spark-clusters openen en beheren met HDInsight-Hulpprogram Ma's in Azure-toolkit voor Eclipse

U kunt verschillende bewerkingen uitvoeren met HDInsight-Hulpprogram Ma's, waaronder het openen van de taak uitvoer.

### <a name="access-the-job-view"></a>De taak weergave openen

1. In **Azure Verkenner**vouwt u **HDInsight**uit, vervolgens de naam van het Spark-cluster en selecteert u vervolgens **taken**.

   ![Knoop punt taak weergave voor eclips van Azure Explorer](./media/apache-spark-eclipse-tool-plugin/eclipse-job-view-node.png)

1. Selecteer het knoop punt **taken** . Als Java-versie lager is dan **1,8**, worden de HDInsight-hulpprogram ma's automatisch geherinneringd om de **E (FX)-** invoeg toepassing te installeren. Selecteer **OK** om door te gaan en volg de wizard om deze te installeren via de eclips-Marketplace en de eclips opnieuw op te starten.

   ![Ontbrekende invoeg toepassing E (FX) uitknippen installeren](./media/apache-spark-eclipse-tool-plugin/auto-install-efxclipse.png)

1. Open de taak weergave vanuit het knoop punt **taken** . In het rechterdeel venster worden op het tabblad **Spark-taak weergave** alle toepassingen weer gegeven die op het cluster zijn uitgevoerd. Selecteer de naam van de toepassing waarvoor u meer details wilt weer geven.

   ![Apache-eclips taak logboeken weer geven](./media/apache-spark-eclipse-tool-plugin/eclipse-view-job-logs.png)

   U kunt vervolgens een van de volgende acties uitvoeren:

   * Plaats de muis aanwijzer op de taak grafiek. Er wordt basis informatie over de actieve taak weer gegeven. Selecteer de taak grafiek en u kunt de fasen en informatie zien die elke taak genereert.

     ![Informatie over fase van Apache Spark taak grafiek](./media/apache-spark-eclipse-tool-plugin/Job-graph-stage-info.png)

   * Selecteer het tabblad **logboek** om veelgebruikte logboeken weer te geven, waaronder **stuur programma stderr**, **stuur programma stdout**en **Directory-informatie**.

     ![Apache Spark eclips taak logboek informatie](./media/apache-spark-eclipse-tool-plugin/eclipse-job-log-info.png)

   * Open de gebruikers interface van Spark-geschiedenis en de gebruikers interface van Apache Hadoop garen (op toepassings niveau) door de Hyper links boven aan het venster te selecteren.

### <a name="access-the-storage-container-for-the-cluster"></a>Toegang tot de opslag container voor het cluster

1. Vouw in azure Verkenner het **HDInsight** -basis knooppunt uit om een lijst met hdinsight Spark-clusters weer te geven die beschikbaar zijn.

1. Vouw de cluster naam uit om het opslag account en de standaard opslag container voor het cluster weer te geven.

   ![Opslag account en standaard opslag container](./media/apache-spark-eclipse-tool-plugin/eclipse-view-explorer5.png)

1. Selecteer de naam van de opslag container die aan het cluster is gekoppeld. Dubbel klik in het rechterdeel venster op de map **HVACOut** . Open een van de **deel** bestanden om de uitvoer van de toepassing te bekijken.

### <a name="access-the-spark-history-server"></a>Toegang tot de Spark-geschiedenis server

1. Klik in azure Verkenner met de rechter muisknop op de naam van uw Spark-cluster en selecteer vervolgens **openen Spark-gebruikers interface**. Wanneer u hierom wordt gevraagd, voert u de beheerders referenties voor het cluster in. U hebt deze opgegeven tijdens het inrichten van het cluster.

1. In het Spark-geschiedenis server dashboard gebruikt u de naam van de toepassing om te zoeken naar de toepassing die u zojuist hebt uitgevoerd. In de voor gaande code stelt u de naam van de toepassing in met behulp van `val conf = new SparkConf().setAppName("MyClusterApp")`. Uw Spark-toepassings naam is dus **MyClusterApp**.

### <a name="start-the-apache-ambari-portal"></a>De Apache Ambari-Portal starten

1. Klik in azure Verkenner met de rechter muisknop op de naam van uw Spark-cluster en selecteer vervolgens **cluster Beheerportal openen (Ambari)** .

1. Wanneer u hierom wordt gevraagd, voert u de beheerders referenties voor het cluster in. U hebt deze opgegeven tijdens het inrichten van het cluster.

### <a name="manage-azure-subscriptions"></a>Azure-abonnementen beheren

Het hulp programma HDInsight in Azure-toolkit voor Eclipse bevat standaard de Spark-clusters van al uw Azure-abonnementen. Indien nodig kunt u de abonnementen opgeven waarvoor u toegang wilt krijgen tot het cluster.

1. Klik in azure Verkenner met de rechter muisknop op het **Azure** -hoofd knooppunt en selecteer vervolgens **Abonnementen beheren**.

1. Schakel in het dialoog venster de selectie vakjes uit voor het abonnement dat u niet wilt openen en selecteer vervolgens **sluiten**. U kunt ook **Afmelden** selecteren als u zich wilt afmelden bij uw Azure-abonnement.

## <a name="run-a-spark-scala-application-locally"></a>Een Spark scala-toepassing lokaal uitvoeren

U kunt HDInsight-Hulpprogram Ma's in Azure-toolkit voor Eclipse gebruiken om Spark scala-toepassingen lokaal op uw werk station uit te voeren. Normaal gesp roken hebben deze toepassingen geen toegang nodig tot cluster bronnen, zoals een opslag container, en kunt u deze lokaal uitvoeren en testen.

### <a name="prerequisite"></a>Vereisten

Terwijl u de lokale Spark scala-toepassing uitvoert op een Windows-computer, krijgt u mogelijk een uitzonde ring zoals uitgelegd in [Spark-2356](https://issues.apache.org/jira/browse/SPARK-2356). Deze uitzonde ring treedt op omdat **WinUtils. exe** ontbreekt in Windows.

U kunt deze fout oplossen door [het uitvoer bare bestand te downloaden](https://public-repo-1.hortonworks.com/hdp-win-alpha/winutils.exe) naar een locatie zoals **C:\WinUtils\bin**, en vervolgens de omgevings variabele **HADOOP_HOME** toe te voegen en de waarde van de variabele in te stellen op **C\WinUtils**.

### <a name="run-a-local-spark-scala-application"></a>Een lokale Spark scala-toepassing uitvoeren

1. De eclips starten en een project maken. Maak in het dialoog venster **Nieuw project** de volgende opties en selecteer **volgende**.

1. Selecteer in de wizard **Nieuw project** **HDInsight-project** > **Spark op Hdinsight Local run sample (scala)** . Selecteer vervolgens **Volgende**.

   ![Nieuw project selecteert een wizard dialoog venster](./media/apache-spark-eclipse-tool-plugin/hdi-spark-app-local-run.png)

1. Volg de stappen 3 tot en met 6 in de vorige sectie om [een Spark scala-project in te stellen voor een HDInsight Spark-cluster](#set-up-a-spark-scala-project-for-an-hdinsight-spark-cluster)om de project gegevens te geven.

1. De sjabloon voegt een voorbeeld code (**LogQuery**) toe aan de map **src** die u lokaal op uw computer kunt uitvoeren.

   ![Locatie van LogQuery lokale scala-toepassing](./media/apache-spark-eclipse-tool-plugin/local-scala-application.png)

1. Klik met de rechter muisknop op **LogQuery. scala** en selecteer **uitvoeren als** > **1 scala-toepassing**. Uitvoer zoals deze wordt weer gegeven op het tabblad **console** :

   ![Resultaat van lokale uitvoer van Spark-toepassing](./media/apache-spark-eclipse-tool-plugin/hdi-spark-app-local-run-result.png)

## <a name="reader-only-role"></a>Alleen-lezen rol

Wanneer gebruikers een taak verzenden naar een cluster met de machtiging alleen lezer, zijn Ambari referenties vereist.

### <a name="link-cluster-from-context-menu"></a>Cluster koppelen via het snelmenu

1. Meld u aan met een account met alleen-lezen rollen.

2. Vouw in **Azure Verkenner** **HDInsight** uit om hdinsight-clusters in uw abonnement weer te geven. De gemarkeerde clusters **"Role: Reader"** hebben alleen de rol alleen-lezen.

    ![HDInsight Spark-clusters in de functie lezer van Azure Explorer](./media/apache-spark-eclipse-tool-plugin/eclipse-view-explorer6.png)

3. Klik met de rechter muisknop op het cluster met de machtiging alleen lezer. Selecteer **dit cluster koppelen in het** context menu om het cluster te koppelen. Voer de Ambari-gebruikers naam en het wacht woord in.

    ![HDInsight Spark-clusters in azure Explorer-koppeling](./media/apache-spark-eclipse-tool-plugin/eclipse-view-explorer7.png)

4. Als het cluster is gekoppeld, wordt HDInsight vernieuwd.
   De fase van het cluster wordt gekoppeld.
  
    ![HDInsight Spark-clusters in azure Verkenner gekoppeld](./media/apache-spark-eclipse-tool-plugin/eclipse-view-explorer8.png)

### <a name="link-cluster-by-expanding-jobs-node"></a>Knoop punt voor koppeling van cluster door taken uitbreiden

1. Klik op het knoop punt **taken** , het venster **toegang tot cluster taken geweigerd** verschijnt.

2. Klik op **dit cluster koppelen om het** cluster te koppelen.

    ![HDInsight Spark-clusters in azure Explorer9](./media/apache-spark-eclipse-tool-plugin/eclipse-view-explorer9.png)

### <a name="link-cluster-from-spark-submission-window"></a>Het cluster koppelen vanuit het venster verzen ding van Spark

1. Een HDInsight-project maken.

2. Klik met de rechter muisknop op het pakket. Selecteer vervolgens **verzenden Spark-toepassing naar HDInsight**.

   ![HDInsight Spark-clusters in azure Verkenner verzenden](./media/apache-spark-eclipse-tool-plugin/eclipse-view-explorer11.png)

3. Selecteer een cluster met een machtiging voor alleen-lezen rollen voor de **cluster naam**. Er wordt een waarschuwings bericht weer gegeven. U kunt klikken op **dit cluster koppelen om het** cluster te koppelen.

   ![Koppeling van HDInsight Spark-clusters in azure Explorer](./media/apache-spark-eclipse-tool-plugin/eclipse-view-explorer15.png)

### <a name="view-storage-accounts"></a>Opslag accounts weer geven

* Voor clusters met de machtiging alleen lezer, klikt u op het knoop punt **opslag accounts** , het venster **opslag toegang geweigerd** verschijnt.

   ![HDInsight Spark-clusters in azure Verkenner-opslag](./media/apache-spark-eclipse-tool-plugin/eclipse-view-explorer13.png)

   ![HDInsight Spark-clusters in azure Verkenner geweigerd](./media/apache-spark-eclipse-tool-plugin/eclipse-view-explorer12.png)

* Voor gekoppelde clusters klikt u op het knoop punt **opslag accounts** , het venster **opslag toegang geweigerd** verschijnt.

   ![HDInsight Spark-clusters in azure Explorer denied2](./media/apache-spark-eclipse-tool-plugin/eclipse-view-explorer14.png)

## <a name="known-problems"></a>Bekende problemen

Wanneer u **een cluster koppelen**gebruikt, kunt u een referentie van de opslag ruimte opgeven.

![cluster koppelen met opslag referentie-eclips](./media/apache-spark-eclipse-tool-plugin/link-cluster-with-storage-credential-eclipse.png)

Er zijn twee modi voor het verzenden van de taken. Als opslag referentie is verstrekt, wordt de batch modus gebruikt voor het verzenden van de taak. Anders wordt de interactieve modus gebruikt. Als het cluster bezet is, wordt mogelijk de volgende fout weer gegeven.

![fout bij het ophalen van de eclips wanneer het cluster bezet is](./media/apache-spark-eclipse-tool-plugin/eclipse-interactive-cluster-busy-upload.png "fout bij het ophalen van de eclips wanneer het cluster bezet is")

![fout bij het ophalen van de eclips als cluster bezet-garens](./media/apache-spark-eclipse-tool-plugin/eclipse-interactive-cluster-busy-submit.png "fout bij het ophalen van de eclips als cluster bezet-garens")

## <a name="see-also"></a>Zie ook

* [Overzicht: Apache Spark in Azure HDInsight](apache-spark-overview.md)

### <a name="scenarios"></a>Scenario's

* [Apache Spark met BI: interactieve gegevens analyses uitvoeren met behulp van Spark in HDInsight met BI-hulpprogram ma's](apache-spark-use-bi-tools.md)
* [Apache Spark met Machine Learning: Spark in HDInsight gebruiken voor het analyseren van de gebouw temperatuur met behulp van HVAC-gegevens](apache-spark-ipython-notebook-machine-learning.md)
* [Apache Spark met Machine Learning: Spark in HDInsight gebruiken om voedsel inspectie resultaten te voors pellen](apache-spark-machine-learning-mllib-ipython.md)
* [Analyse van website logboeken met Apache Spark in HDInsight](apache-spark-custom-library-website-log-analysis.md)

### <a name="creating-and-running-applications"></a>Toepassingen maken en uitvoeren

* [Een zelfstandige toepassing maken met behulp van Scala](apache-spark-create-standalone-application.md)
* [Apache Livy gebruiken om taken op afstand uit te voeren in een Apache Spark-cluster](apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a>Tools en uitbreidingen

* [Azure-toolkit voor IntelliJ gebruiken om Spark scala-toepassingen te maken en in te dienen](apache-spark-intellij-tool-plugin.md)
* [Azure-toolkit voor IntelliJ gebruiken om fouten op te lossen Apache Spark toepassingen op afstand via VPN](../hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [Azure-toolkit voor IntelliJ gebruiken om fouten op te lossen Apache Spark toepassingen op afstand via SSH](../hdinsight-apache-spark-intellij-tool-debug-remotely-through-ssh.md)
* [Apache Zeppelin-notebooks gebruiken met een Apache Spark-cluster in HDInsight](apache-spark-zeppelin-notebook.md)
* [Kernels die beschikbaar zijn voor Jupyter notebook in Apache Spark cluster voor HDInsight](apache-spark-jupyter-notebook-kernels.md)
* [Externe pakketten gebruiken met Jupyter-notebooks](apache-spark-jupyter-notebook-use-external-packages.md)
* [Jupyter op uw computer installeren en verbinding maken met een HDInsight Spark-cluster](apache-spark-jupyter-notebook-install-locally.md)

### <a name="managing-resources"></a>Resources beheren

* [Resources beheren voor het Apache Spark-cluster in Azure HDInsight](apache-spark-resource-manager.md)
* [Taken die worden uitgevoerd in een Apache Spark-cluster in HDInsight, traceren en er fouten in oplossen](apache-spark-job-debugging.md)
