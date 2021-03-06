---
title: Apache Maven gebruiken voor het bouwen van een Java HBase-client voor Azure HDInsight
description: Leer hoe u Apache Maven gebruikt om een op Java gebaseerde Apache HBase-toepassing te bouwen en deze vervolgens te implementeren op HBase in azure HDInsight.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive,seodec18
ms.topic: conceptual
ms.date: 04/16/2019
ms.openlocfilehash: c948d07bed99f1286e27d645fde7b96fdc699c02
ms.sourcegitcommit: 9dec0358e5da3ceb0d0e9e234615456c850550f6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/14/2019
ms.locfileid: "72311704"
---
# <a name="build-java-applications-for-apache-hbase"></a>Ontwikkel Java-toepassingen voor Apache HBase

Meer informatie over het maken van een [Apache HBase](https://hbase.apache.org/) -toepassing in Java. Gebruik vervolgens de toepassing met HBase in azure HDInsight.

De stappen in dit document gebruiken [Apache Maven](https://maven.apache.org/) om het project te maken en te bouwen. Maven is een hulp programma voor het beheer en het beheren van software projecten waarmee u software, documentatie en rapporten voor Java-projecten kunt bouwen.

## <a name="prerequisites"></a>Vereisten

* Een Apache HBase-cluster in HDInsight. Zie [aan de slag met Apache HBase](./apache-hbase-tutorial-get-started-linux.md).

* [Jdk-versie 8 (Java Developer Kit)](https://aka.ms/azure-jdks).

* [Apache Maven](https://maven.apache.org/download.cgi) is op de juiste wijze [geïnstalleerd](https://maven.apache.org/install.html) volgens Apache.  Maven is een project voor het maken van een systeem voor Java-projecten.

* Een SSH-client. Zie voor meer informatie [Verbinding maken met HDInsight (Apache Hadoop) via SSH](../hdinsight-hadoop-linux-use-ssh-unix.md).

* Als u Power shell gebruikt, hebt u de [AZ-module](https://docs.microsoft.com/powershell/azure/overview)nodig.

* Een teksteditor. In dit artikel wordt micro soft Klad blok gebruikt.

## <a name="test-environment"></a>Test omgeving
De omgeving die voor dit artikel wordt gebruikt, is een computer met Windows 10.  De opdrachten zijn uitgevoerd in een opdracht prompt en de verschillende bestanden zijn bewerkt met Klad blok. Wijzig dienovereenkomstig voor uw omgeving.

Voer vanaf een opdracht prompt de onderstaande opdrachten in om een werk omgeving te maken:

```cmd
IF NOT EXIST C:\HDI MKDIR C:\HDI
cd C:\HDI
```

## <a name="create-a-maven-project"></a>Een Maven-project maken

1. Voer de volgende opdracht in om een Maven-project met de naam **hbaseapp**te maken:

    ```cmd
    mvn archetype:generate -DgroupId=com.microsoft.examples -DartifactId=hbaseapp -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false

    cd hbaseapp
    mkdir conf
    ```

    Met deze opdracht maakt u een map met de naam `hbaseapp` op de huidige locatie, die een Basic Maven-project bevat. Met de tweede opdracht wordt de werkmap gewijzigd in `hbaseapp`. Met de derde opdracht maakt u een nieuwe map, `conf`, die later wordt gebruikt. De map `hbaseapp` bevat de volgende items:

    * `pom.xml`: het object model van het project ([pom](https://maven.apache.org/guides/introduction/introduction-to-the-pom.html)) bevat informatie en configuratie details die zijn gebruikt om het project te bouwen.
    * `src\main\java\com\microsoft\examples`: bevat de code van uw toepassing.
    * `src\test\java\com\microsoft\examples`: bevat tests voor uw toepassing.

2. Verwijder de gegenereerde voorbeeld code. Verwijder de gegenereerde test-en toepassings bestanden `AppTest.java`en `App.java` door de onderstaande opdrachten in te voeren:

    ```cmd
    DEL src\main\java\com\microsoft\examples\App.java
    DEL src\test\java\com\microsoft\examples\AppTest.java
    ```

## <a name="update-the-project-object-model"></a>Het object model van het project bijwerken

Zie https://maven.apache.org/pom.htmlvoor een volledige referentie van het bestand pom. XML.  Open `pom.xml` door de volgende opdracht in te voeren:

```cmd
notepad pom.xml
```

### <a name="add-dependencies"></a>Afhankelijkheden toevoegen

In `pom.xml`voegt u de volgende tekst toe aan de sectie `<dependencies>`:

```xml
<dependency>
    <groupId>org.apache.hbase</groupId>
    <artifactId>hbase-client</artifactId>
    <version>1.1.2</version>
</dependency>
<dependency>
    <groupId>org.apache.phoenix</groupId>
    <artifactId>phoenix-core</artifactId>
    <version>4.14.1-HBase-1.1</version>
</dependency>
```  

In deze sectie wordt aangegeven dat het project **hbase-client** -en **Phoenix-kern** onderdelen nodig heeft. Tijdens het compileren worden deze afhankelijkheden gedownload uit de standaard-maven-opslag plaats. U kunt de [maven Central repository search](https://search.maven.org/artifact/org.apache.hbase/hbase-client/1.1.2/jar) gebruiken voor meer informatie over deze afhankelijkheid.

> [!IMPORTANT]  
> Het versie nummer van de hbase-client moet overeenkomen met de versie van Apache HBase die bij uw HDInsight-cluster wordt meegeleverd. Gebruik de volgende tabel om het juiste versie nummer te vinden.

| Versie van HDInsight-cluster | Te gebruiken Apache HBase-versie |
| --- | --- |
| 3.6 | 1.1.2 |
| 4.0 | 2.0.0 |

Zie [Wat zijn de verschillende Apache Hadoop onderdelen die beschikbaar zijn met hdinsight](../hdinsight-component-versioning.md)voor meer informatie over hdinsight-versies en-onderdelen.

### <a name="build-configuration"></a>Configuratie maken

Met maven-invoeg toepassingen kunt u de build-fasen van het project aanpassen. Deze sectie wordt gebruikt om invoeg toepassingen, resources en andere configuratie opties voor het maken van een toepassing toe te voegen.

Voeg de volgende code toe aan het `pom.xml`-bestand, sla het bestand op en sluit het. Deze tekst moet zich in de `<project>...</project>` Tags in het bestand bevindt, bijvoorbeeld tussen `</dependencies>` en `</project>`.

```xml
<build>
    <sourceDirectory>src</sourceDirectory>
    <resources>
    <resource>
        <directory>${basedir}/conf</directory>
        <filtering>false</filtering>
        <includes>
        <include>hbase-site.xml</include>
        </includes>
    </resource>
    </resources>
    <plugins>
    <plugin>
        <groupId>org.apache.maven.plugins</groupId>
        <artifactId>maven-compiler-plugin</artifactId>
                <version>3.8.0</version>
        <configuration>
            <source>1.8</source>
            <target>1.8</target>
        </configuration>
        </plugin>
    <plugin>
        <groupId>org.apache.maven.plugins</groupId>
        <artifactId>maven-shade-plugin</artifactId>
        <version>3.2.1</version>
        <configuration>
        <transformers>
            <transformer implementation="org.apache.maven.plugins.shade.resource.ApacheLicenseResourceTransformer">
            </transformer>
        </transformers>
        </configuration>
        <executions>
        <execution>
            <phase>package</phase>
            <goals>
            <goal>shade</goal>
            </goals>
        </execution>
        </executions>
    </plugin>
    </plugins>
</build>
```

In deze sectie wordt een resource (`conf/hbase-site.xml`) geconfigureerd die configuratie-informatie voor HBase bevat.

> [!NOTE]  
> U kunt ook configuratie waarden instellen via code. Bekijk de opmerkingen in het `CreateTable` voor beeld.

In deze sectie worden ook de [invoeg toepassing Apache Maven compiler](https://maven.apache.org/plugins/maven-compiler-plugin/) en [Apache Maven Shad](https://maven.apache.org/plugins/maven-shade-plugin/)geconfigureerd. De compiler-invoeg toepassing wordt gebruikt voor het compileren van de topologie. De arcerings-invoeg toepassing wordt gebruikt om te voor komen dat licentie duplicatie in het JAR-pakket dat is gebouwd door maven. Deze invoeg toepassing wordt gebruikt om te voor komen dat een ' dubbele licentie bestanden ' tijdens runtime op het HDInsight-cluster wordt uitgevoerd. Als u maven-Shad-plugin met de `ApacheLicenseResourceTransformer`-implementatie gebruikt, wordt de fout verhinderd.

De Maven-Shade-invoeg toepassing produceert ook een uber jar dat alle afhankelijkheden bevat die vereist zijn voor de toepassing.

### <a name="download-the-hbase-sitexml"></a>De hbase-site. xml downloaden

Gebruik de volgende opdracht om de HBase-configuratie van het HBase-cluster naar de map `conf` te kopiëren. Vervang `CLUSTERNAME` door de naam van uw HDInsight-cluster en voer de volgende opdracht in:

```cmd
scp sshuser@CLUSTERNAME-ssh.azurehdinsight.net:/etc/hbase/conf/hbase-site.xml ./conf/hbase-site.xml
```

## <a name="create-the-application"></a>De toepassing maken

### <a name="implement-a-createtable-class"></a>Een CreateTable-klasse implementeren

Voer de onderstaande opdracht in om een nieuw bestand te maken en te openen `CreateTable.java`. Selecteer **Ja** wanneer u wordt gevraagd een nieuw bestand te maken.

```cmd
notepad src\main\java\com\microsoft\examples\CreateTable.java
```

Kopieer en plak de Java-code hieronder in het nieuwe bestand. Sluit het bestand.

```java
package com.microsoft.examples;
import java.io.IOException;

import org.apache.hadoop.conf.Configuration;
import org.apache.hadoop.hbase.HBaseConfiguration;
import org.apache.hadoop.hbase.client.HBaseAdmin;
import org.apache.hadoop.hbase.HTableDescriptor;
import org.apache.hadoop.hbase.TableName;
import org.apache.hadoop.hbase.HColumnDescriptor;
import org.apache.hadoop.hbase.client.HTable;
import org.apache.hadoop.hbase.client.Put;
import org.apache.hadoop.hbase.util.Bytes;

public class CreateTable {
    public static void main(String[] args) throws IOException {
    Configuration config = HBaseConfiguration.create();

    // Example of setting zookeeper values for HDInsight
    // in code instead of an hbase-site.xml file
    //
    // config.set("hbase.zookeeper.quorum",
    //            "zookeepernode0,zookeepernode1,zookeepernode2");
    //config.set("hbase.zookeeper.property.clientPort", "2181");
    //config.set("hbase.cluster.distributed", "true");
    //
    //NOTE: Actual zookeeper host names can be found using Ambari:
    //curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/hosts"

    //Linux-based HDInsight clusters use /hbase-unsecure as the znode parent
    config.set("zookeeper.znode.parent","/hbase-unsecure");

    // create an admin object using the config
    HBaseAdmin admin = new HBaseAdmin(config);

    // create the table...
    HTableDescriptor tableDescriptor = new HTableDescriptor(TableName.valueOf("people"));
    // ... with two column families
    tableDescriptor.addFamily(new HColumnDescriptor("name"));
    tableDescriptor.addFamily(new HColumnDescriptor("contactinfo"));
    admin.createTable(tableDescriptor);

    // define some people
    String[][] people = {
        { "1", "Marcel", "Haddad", "marcel@fabrikam.com"},
        { "2", "Franklin", "Holtz", "franklin@contoso.com" },
        { "3", "Dwayne", "McKee", "dwayne@fabrikam.com" },
        { "4", "Rae", "Schroeder", "rae@contoso.com" },
        { "5", "Rosalie", "burton", "rosalie@fabrikam.com"},
        { "6", "Gabriela", "Ingram", "gabriela@contoso.com"} };

    HTable table = new HTable(config, "people");

    // Add each person to the table
    //   Use the `name` column family for the name
    //   Use the `contactinfo` column family for the email
    for (int i = 0; i< people.length; i++) {
        Put person = new Put(Bytes.toBytes(people[i][0]));
        person.add(Bytes.toBytes("name"), Bytes.toBytes("first"), Bytes.toBytes(people[i][1]));
        person.add(Bytes.toBytes("name"), Bytes.toBytes("last"), Bytes.toBytes(people[i][2]));
        person.add(Bytes.toBytes("contactinfo"), Bytes.toBytes("email"), Bytes.toBytes(people[i][3]));
        table.put(person);
    }
    // flush commits and close the table
    table.flushCommits();
    table.close();
    }
}
```

Deze code is de `CreateTable` klasse, waarmee een tabel met de naam `people` wordt gemaakt en gevuld met enkele vooraf gedefinieerde gebruikers.

### <a name="implement-a-searchbyemail-class"></a>Een SearchByEmail-klasse implementeren

Voer de onderstaande opdracht in om een nieuw bestand te maken en te openen `SearchByEmail.java`. Selecteer **Ja** wanneer u wordt gevraagd een nieuw bestand te maken.

```cmd
notepad src\main\java\com\microsoft\examples\SearchByEmail.java
```

Kopieer en plak de Java-code hieronder in het nieuwe bestand. Sluit het bestand.

```java
package com.microsoft.examples;
import java.io.IOException;

import org.apache.hadoop.conf.Configuration;
import org.apache.hadoop.hbase.HBaseConfiguration;
import org.apache.hadoop.hbase.client.HTable;
import org.apache.hadoop.hbase.client.Scan;
import org.apache.hadoop.hbase.client.ResultScanner;
import org.apache.hadoop.hbase.client.Result;
import org.apache.hadoop.hbase.filter.RegexStringComparator;
import org.apache.hadoop.hbase.filter.SingleColumnValueFilter;
import org.apache.hadoop.hbase.filter.CompareFilter.CompareOp;
import org.apache.hadoop.hbase.util.Bytes;
import org.apache.hadoop.util.GenericOptionsParser;

public class SearchByEmail {
    public static void main(String[] args) throws IOException {
    Configuration config = HBaseConfiguration.create();

    // Use GenericOptionsParser to get only the parameters to the class
    // and not all the parameters passed (when using WebHCat for example)
    String[] otherArgs = new GenericOptionsParser(config, args).getRemainingArgs();
    if (otherArgs.length != 1) {
        System.out.println("usage: [regular expression]");
        System.exit(-1);
    }

    // Open the table
    HTable table = new HTable(config, "people");

    // Define the family and qualifiers to be used
    byte[] contactFamily = Bytes.toBytes("contactinfo");
    byte[] emailQualifier = Bytes.toBytes("email");
    byte[] nameFamily = Bytes.toBytes("name");
    byte[] firstNameQualifier = Bytes.toBytes("first");
    byte[] lastNameQualifier = Bytes.toBytes("last");

    // Create a regex filter
    RegexStringComparator emailFilter = new RegexStringComparator(otherArgs[0]);
    // Attach the regex filter to a filter
    //   for the email column
    SingleColumnValueFilter filter = new SingleColumnValueFilter(
        contactFamily,
        emailQualifier,
        CompareOp.EQUAL,
        emailFilter
    );

    // Create a scan and set the filter
    Scan scan = new Scan();
    scan.setFilter(filter);

    // Get the results
    ResultScanner results = table.getScanner(scan);
    // Iterate over results and print  values
    for (Result result : results ) {
        String id = new String(result.getRow());
        byte[] firstNameObj = result.getValue(nameFamily, firstNameQualifier);
        String firstName = new String(firstNameObj);
        byte[] lastNameObj = result.getValue(nameFamily, lastNameQualifier);
        String lastName = new String(lastNameObj);
        System.out.println(firstName + " " + lastName + " - ID: " + id);
        byte[] emailObj = result.getValue(contactFamily, emailQualifier);
        String email = new String(emailObj);
        System.out.println(firstName + " " + lastName + " - " + email + " - ID: " + id);
    }
    results.close();
    table.close();
    }
}
```

De klasse `SearchByEmail` kan worden gebruikt om rijen op basis van een e-mail adres op te vragen. Omdat er gebruik wordt gemaakt van een reguliere-expressie filter, kunt u een teken reeks of een reguliere expressie opgeven wanneer u de klasse gebruikt.

### <a name="implement-a-deletetable-class"></a>Een DeleteTable-klasse implementeren

Voer de onderstaande opdracht in om een nieuw bestand te maken en te openen `DeleteTable.java`. Selecteer **Ja** wanneer u wordt gevraagd een nieuw bestand te maken.

```cmd
notepad src\main\java\com\microsoft\examples\DeleteTable.java
```

Kopieer en plak de Java-code hieronder in het nieuwe bestand. Sluit het bestand.

```java
package com.microsoft.examples;
import java.io.IOException;

import org.apache.hadoop.conf.Configuration;
import org.apache.hadoop.hbase.HBaseConfiguration;
import org.apache.hadoop.hbase.client.HBaseAdmin;

public class DeleteTable {
    public static void main(String[] args) throws IOException {
    Configuration config = HBaseConfiguration.create();

    // Create an admin object using the config
    HBaseAdmin admin = new HBaseAdmin(config);

    // Disable, and then delete the table
    admin.disableTable("people");
    admin.deleteTable("people");
    }
}
```

Met de klasse `DeleteTable` worden de HBase-tabellen opgeschoond die in dit voor beeld zijn gemaakt door de tabel die door de `CreateTable` klasse is gemaakt, uit te scha kelen en neer te zetten.

## <a name="build-and-package-the-application"></a>De toepassing bouwen en inpakken

1. Gebruik in de `hbaseapp`-map de volgende opdracht om een JAR-bestand te maken dat de toepassing bevat:

    ```cmd
    mvn clean package
    ```

    Met deze opdracht wordt de toepassing gebouwd en verpakt in een jar-bestand.

2. Wanneer de opdracht is voltooid, bevat de `hbaseapp/target` Directory een bestand met de naam `hbaseapp-1.0-SNAPSHOT.jar`.

   > [!NOTE]  
   > Het `hbaseapp-1.0-SNAPSHOT.jar` bestand is een uber jar. Het bevat alle afhankelijkheden die nodig zijn om de toepassing uit te voeren.

## <a name="upload-the-jar-and-run-jobs-ssh"></a>De JAR en run Jobs uploaden (SSH)

De volgende stappen gebruiken `scp` om het JAR te kopiëren naar het primaire hoofd knooppunt van uw Apache HBase in HDInsight-cluster. De opdracht `ssh` wordt vervolgens gebruikt om verbinding te maken met het cluster en het voor beeld rechtstreeks op het hoofd knooppunt uit te voeren.

1. Upload het jar naar het cluster. Vervang `CLUSTERNAME` door de naam van uw HDInsight-cluster en voer de volgende opdracht in:

    ```cmd
    scp ./target/hbaseapp-1.0-SNAPSHOT.jar sshuser@CLUSTERNAME-ssh.azurehdinsight.net:hbaseapp-1.0-SNAPSHOT.jar
    ```

2. Maak verbinding met het HBase-cluster. Vervang `CLUSTERNAME` door de naam van uw HDInsight-cluster en voer de volgende opdracht in:

    ```cmd
    ssh sshuser@CLUSTERNAME-ssh.azurehdinsight.net
    ```

 3. Als u een HBase-tabel wilt maken met behulp van de Java-toepassing, gebruikt u de volgende opdracht in uw open SSH-verbinding:

    ```bash
    yarn jar hbaseapp-1.0-SNAPSHOT.jar com.microsoft.examples.CreateTable
    ```

    Met deze opdracht wordt een HBase-tabel met de naam **personen**gemaakt en gevuld met gegevens.

4. Als u wilt zoeken naar e-mail adressen die zijn opgeslagen in de tabel, gebruikt u de volgende opdracht:

    ```bash
    yarn jar hbaseapp-1.0-SNAPSHOT.jar com.microsoft.examples.SearchByEmail contoso.com
    ```

    U ontvangt de volgende resultaten:

        Franklin Holtz - ID: 2
        Franklin Holtz - franklin@contoso.com - ID: 2
        Rae Schroeder - ID: 4
        Rae Schroeder - rae@contoso.com - ID: 4
        Gabriela Ingram - ID: 6
        Gabriela Ingram - gabriela@contoso.com - ID: 6

5. Als u de tabel wilt verwijderen, gebruikt u de volgende opdracht:

    ```bash
    yarn jar hbaseapp-1.0-SNAPSHOT.jar com.microsoft.examples.DeleteTable
    ```

## <a name="upload-the-jar-and-run-jobs-powershell"></a>De JAR en run Jobs uploaden (Power shell)

In de volgende stappen wordt gebruikgemaakt van de Azure PowerShell [AZ-module](https://docs.microsoft.com/powershell/azure/new-azureps-module-az) voor het uploaden van het jar naar de standaard opslag voor uw Apache HBase-cluster. HDInsight-cmdlets worden vervolgens gebruikt om de voor beelden op afstand uit te voeren.

1. Nadat u de AZ-module hebt geïnstalleerd en geconfigureerd, maakt u een bestand met de naam `hbase-runner.psm1`. Gebruik de volgende tekst als de inhoud van dit bestand:

   ```powershell
    <#
    .SYNOPSIS
    Copies a file to the primary storage of an HDInsight cluster.
    .DESCRIPTION
    Copies a file from a local directory to the blob container for
    the HDInsight cluster.
    .EXAMPLE
    Start-HBaseExample -className "com.microsoft.examples.CreateTable"
    -clusterName "MyHDInsightCluster"

    .EXAMPLE
    Start-HBaseExample -className "com.microsoft.examples.SearchByEmail"
    -clusterName "MyHDInsightCluster"
    -emailRegex "contoso.com"

    .EXAMPLE
    Start-HBaseExample -className "com.microsoft.examples.SearchByEmail"
    -clusterName "MyHDInsightCluster"
    -emailRegex "^r" -showErr
    #>

    function Start-HBaseExample {
    [CmdletBinding(SupportsShouldProcess = $true)]
    param(
    #The class to run
    [Parameter(Mandatory = $true)]
    [String]$className,

    #The name of the HDInsight cluster
    [Parameter(Mandatory = $true)]
    [String]$clusterName,

    #Only used when using SearchByEmail
    [Parameter(Mandatory = $false)]
    [String]$emailRegex,

    #Use if you want to see stderr output
    [Parameter(Mandatory = $false)]
    [Switch]$showErr
    )

    Set-StrictMode -Version 3

    # Is the Azure module installed?
    FindAzure

    # Get the login for the HDInsight cluster
    $creds=Get-Credential -Message "Enter the login for the cluster" -UserName "admin"

    # The JAR
    $jarFile = "wasb:///example/jars/hbaseapp-1.0-SNAPSHOT.jar"

    # The job definition
    $jobDefinition = New-AzHDInsightMapReduceJobDefinition `
        -JarFile $jarFile `
        -ClassName $className `
        -Arguments $emailRegex

    # Get the job output
    $job = Start-AzHDInsightJob `
        -ClusterName $clusterName `
        -JobDefinition $jobDefinition `
        -HttpCredential $creds
    Write-Host "Wait for the job to complete ..." -ForegroundColor Green
    Wait-AzHDInsightJob `
        -ClusterName $clusterName `
        -JobId $job.JobId `
        -HttpCredential $creds
    if($showErr)
    {
    Write-Host "STDERR"
    Get-AzHDInsightJobOutput `
                -Clustername $clusterName `
                -JobId $job.JobId `
                -HttpCredential $creds `
                -DisplayOutputType StandardError
    }
    Write-Host "Display the standard output ..." -ForegroundColor Green
    Get-AzHDInsightJobOutput `
                -Clustername $clusterName `
                -JobId $job.JobId `
                -HttpCredential $creds
    }

    <#
    .SYNOPSIS
    Copies a file to the primary storage of an HDInsight cluster.
    .DESCRIPTION
    Copies a file from a local directory to the blob container for
    the HDInsight cluster.
    .EXAMPLE
    Add-HDInsightFile -localPath "C:\temp\data.txt"
    -destinationPath "example/data/data.txt"
    -ClusterName "MyHDInsightCluster"
    .EXAMPLE
    Add-HDInsightFile -localPath "C:\temp\data.txt"
    -destinationPath "example/data/data.txt"
    -ClusterName "MyHDInsightCluster"
    -Container "MyContainer"
    #>

    function Add-HDInsightFile {
        [CmdletBinding(SupportsShouldProcess = $true)]
        param(
            #The path to the local file.
            [Parameter(Mandatory = $true)]
            [String]$localPath,

            #The destination path and file name, relative to the root of the container.
            [Parameter(Mandatory = $true)]
            [String]$destinationPath,

            #The name of the HDInsight cluster
            [Parameter(Mandatory = $true)]
            [String]$clusterName,

            #If specified, overwrites existing files without prompting
            [Parameter(Mandatory = $false)]
            [Switch]$force
        )

        Set-StrictMode -Version 3

        # Is the Azure module installed?
        FindAzure

        # Get authentication for the cluster
        $creds=Get-Credential

        # Does the local path exist?
        if (-not (Test-Path $localPath))
        {
            throw "Source path '$localPath' does not exist."
        }

        # Get the primary storage container
        $storage = GetStorage -clusterName $clusterName

        # Upload file to storage, overwriting existing files if -force was used.
        Set-AzStorageBlobContent -File $localPath `
            -Blob $destinationPath `
            -force:$force `
            -Container $storage.container `
            -Context $storage.context
    }

    function FindAzure {
        # Is there an active Azure subscription?
        $sub = Get-AzSubscription -ErrorAction SilentlyContinue
        if(-not($sub))
        {
            Connect-AzAccount
        }
    }

    function GetStorage {
        param(
            [Parameter(Mandatory = $true)]
            [String]$clusterName
        )
        $hdi = Get-AzHDInsightCluster -ClusterName $clusterName
        # Does the cluster exist?
        if (!$hdi)
        {
            throw "HDInsight cluster '$clusterName' does not exist."
        }
        # Create a return object for context & container
        $return = @{}
        $storageAccounts = @{}

        # Get storage information
        $resourceGroup = $hdi.ResourceGroup
        $storageAccountName=$hdi.DefaultStorageAccount.split('.')[0]
        $container=$hdi.DefaultStorageContainer
        $storageAccountKey=(Get-AzStorageAccountKey `
            -Name $storageAccountName `
        -ResourceGroupName $resourceGroup)[0].Value
        # Get the resource group, in case we need that
        $return.resourceGroup = $resourceGroup
        # Get the storage context, as we can't depend
        # on using the default storage context
        $return.context = New-AzStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey
        # Get the container, so we know where to
        # find/store blobs
        $return.container = $container
        # Return storage accounts to support finding all accounts for
        # a cluster
        $return.storageAccount = $storageAccountName
        $return.storageAccountKey = $storageAccountKey

        return $return
    }
    # Only export the verb-phrase things
    export-modulemember *-*
   ```

    Dit bestand bevat twee modules:

   * **Add-HDInsightFile** : wordt gebruikt voor het uploaden van bestanden naar het cluster
   * **Start-HBaseExample** : wordt gebruikt voor het uitvoeren van de klassen die u eerder hebt gemaakt

2. Sla het `hbase-runner.psm1`-bestand op in de `hbaseapp` map.

3. Registreer de modules met Azure PowerShell. Open een nieuw Azure PowerShell venster en bewerk de onderstaande opdracht door `CLUSTERNAME` te vervangen door de naam van uw cluster. Voer vervolgens de volgende opdrachten in:

    ```powershell
    cd C:\HDI\hbaseapp
    $myCluster = "CLUSTERNAME"
    Import-Module .\hbase-runner.psm1
    ```

4. Gebruik de volgende opdracht om de `hbaseapp-1.0-SNAPSHOT.jar` te uploaden naar uw cluster.

    ```powershell
    Add-HDInsightFile -localPath target\hbaseapp-1.0-SNAPSHOT.jar -destinationPath example/jars/hbaseapp-1.0-SNAPSHOT.jar -clusterName $myCluster
    ```

    Wanneer u hierom wordt gevraagd, voert u de naam en het wacht woord voor het cluster aanmelding (admin) in. Met de opdracht wordt de `hbaseapp-1.0-SNAPSHOT.jar` geüpload naar de `example/jars` locatie in de primaire opslag voor uw cluster.

5. Gebruik de volgende opdracht om een tabel te maken met behulp van de `hbaseapp`:

    ```powershell
    Start-HBaseExample -className com.microsoft.examples.CreateTable -clusterName $myCluster
    ```

    Wanneer u hierom wordt gevraagd, voert u de naam en het wacht woord voor het cluster aanmelding (admin) in.

    Met deze opdracht maakt u een tabel met de naam **personen** in HBase op uw HDInsight-cluster. Met deze opdracht wordt geen uitvoer weer gegeven in het console venster.

6. Als u wilt zoeken naar vermeldingen in de tabel, gebruikt u de volgende opdracht:

    ```powershell
    Start-HBaseExample -className com.microsoft.examples.SearchByEmail -clusterName $myCluster -emailRegex contoso.com
    ```

    Wanneer u hierom wordt gevraagd, voert u de naam en het wacht woord voor het cluster aanmelding (admin) in.

    Met deze opdracht wordt gebruikgemaakt van de `SearchByEmail`-klasse om rijen te zoeken waar de `contactinformation` kolom familie en de `email` kolom de teken reeks `contoso.com`bevat. De volgende resultaten moeten worden weer gegeven:

          Franklin Holtz - ID: 2
          Franklin Holtz - franklin@contoso.com - ID: 2
          Rae Schroeder - ID: 4
          Rae Schroeder - rae@contoso.com - ID: 4
          Gabriela Ingram - ID: 6
          Gabriela Ingram - gabriela@contoso.com - ID: 6

    Als u **fabrikam.com** gebruikt voor de `-emailRegex` waarde, worden de gebruikers met **fabrikam.com** in het veld e-mail geretourneerd. U kunt ook reguliere expressies gebruiken als zoek term. Zo retourneert **^ r** e-mail adressen die beginnen met de letter r.

7. Als u de tabel wilt verwijderen, gebruikt u de volgende opdracht:

    ```PowerShell
    Start-HBaseExample -className com.microsoft.examples.DeleteTable -clusterName $myCluster
    ```

### <a name="no-results-or-unexpected-results-when-using-start-hbaseexample"></a>Geen resultaten of onverwachte resultaten bij het gebruik van start-HBaseExample

Gebruik de para meter `-showErr` om de standaard fout (STDERR) weer te geven die wordt geproduceerd tijdens het uitvoeren van de taak.

## <a name="next-steps"></a>Volgende stappen

[Meer informatie over het gebruik van sqlline gebruiken met Apache HBase](apache-hbase-query-with-phoenix.md)
