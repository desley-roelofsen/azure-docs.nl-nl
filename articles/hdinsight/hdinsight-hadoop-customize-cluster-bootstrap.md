---
title: Configuraties van Azure HDInsight-clusters aanpassen met Boots trap
description: Meer informatie over hoe u de configuratie van HDInsight-clusters programmatisch kunt aanpassen met behulp van .net-, Power shell-en Resource Manager-sjablonen.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.custom: hdinsightactive
ms.date: 11/21/2019
ms.openlocfilehash: baef54fc5c8fd03ea190da2023dcba2e96abb982
ms.sourcegitcommit: dd0304e3a17ab36e02cf9148d5fe22deaac18118
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/22/2019
ms.locfileid: "74406282"
---
# <a name="customize-hdinsight-clusters-using-bootstrap"></a>HDInsight-clusters aanpassen met Boots trap

Met Boots trap scripts kunt u programmatisch onderdelen installeren en configureren in azure HDInsight.

Er zijn drie benaderingen om instellingen van het configuratie bestand in te stellen wanneer uw HDInsight-cluster wordt gemaakt:

* Azure PowerShell gebruiken
* .NET SDK gebruiken
* Azure Resource Manager-sjabloon gebruiken

Met deze programmatische methoden kunt u bijvoorbeeld opties in deze bestanden configureren:

* clusterIdentity.xml
* bestand core-site. XML
* gateway. XML
* hbase-env.xml
* hbase-site.xml
* hdfs-site.xml
* hive-env.xml
* Hive-site. XML
* mapred-site
* oozie-site.xml
* oozie-env.xml
* Storm-site. XML
* tez-site.xml
* webhcat-site.xml
* yarn-site. XML
* server. Properties (Kafka-Broker-configuratie)

Zie [HDInsight-clusters aanpassen met script Action (Linux)](hdinsight-hadoop-customize-cluster-linux.md)voor meer informatie over het installeren van extra onderdelen in het HDInsight-cluster tijdens de aanmaak tijd.

## <a name="prerequisites"></a>Vereisten

* Als u Power shell gebruikt, hebt u de [AZ-module](https://docs.microsoft.com/powershell/azure/overview)nodig.

## <a name="use-azure-powershell"></a>Azure PowerShell gebruiken

Met de volgende Power shell-code wordt een [Apache Hive](https://hive.apache.org/) configuratie aangepast:

> [!IMPORTANT]  
> De para meter `Spark2Defaults` moet mogelijk worden gebruikt met [add-AzHDInsightConfigValue](https://docs.microsoft.com/powershell/module/az.hdinsight/add-azhdinsightconfigvalue). U kunt lege waarden door geven aan de para meter, zoals wordt weer gegeven in het code voorbeeld hieronder.

```powershell
# hive-site.xml configuration
$hiveConfigValues = @{ "hive.metastore.client.socket.timeout"="90s" }

$config = New-AzHDInsightClusterConfig `
    | Set-AzHDInsightDefaultStorage `
        -StorageAccountName "$defaultStorageAccountName.blob.core.windows.net" `
        -StorageAccountKey $defaultStorageAccountKey `
    | Add-AzHDInsightConfigValue `
        -HiveSite $hiveConfigValues `
        -Spark2Defaults @{}

New-AzHDInsightCluster `
    -ResourceGroupName $existingResourceGroupName `
    -ClusterName $clusterName `
    -Location $location `
    -ClusterSizeInNodes $clusterSizeInNodes `
    -ClusterType Hadoop `
    -OSType Linux `
    -Version "3.6" `
    -HttpCredential $httpCredential `
    -Config $config
```

Een volledig werkend Power shell-script vindt u in [bijlage](#appendix-powershell-sample).

**De wijziging controleren:**

1. Ga naar `https://CLUSTERNAME.azurehdinsight.net/` waarbij `CLUSTERNAME` de naam van het cluster is.
1. Ga in het menu links naar **Hive** > **configs** > **Geavanceerd**.
1. Vouw **Geavanceerde Hive-site**uit.
1. Zoek **Hive. MailStore. client. socket. timeout** en bevestig dat de waarde **90s**is.

Meer voor beelden over het aanpassen van andere configuratie bestanden:

```xml
# hdfs-site.xml configuration
$HdfsConfigValues = @{ "dfs.blocksize"="64m" } #default is 128MB in HDI 3.0 and 256MB in HDI 2.1

# core-site.xml configuration
$CoreConfigValues = @{ "ipc.client.connect.max.retries"="60" } #default 50

# mapred-site.xml configuration
$MapRedConfigValues = @{ "mapreduce.task.timeout"="1200000" } #default 600000

# oozie-site.xml configuration
$OozieConfigValues = @{ "oozie.service.coord.normal.default.timeout"="150" }  # default 120
```

## <a name="use-net-sdk"></a>.NET SDK gebruiken

Zie [Linux-gebaseerde clusters maken in HDInsight met behulp van de .NET SDK](hdinsight-hadoop-create-linux-clusters-dotnet-sdk.md#use-bootstrap).

## <a name="use-resource-manager-template"></a>Resource Manager-sjabloon gebruiken

U kunt Boots trap gebruiken in Resource Manager-sjabloon:

```json
"configurations": {
    "hive-site": {
        "hive.metastore.client.connect.retry.delay": "5",
        "hive.execution.engine": "mr",
        "hive.security.authorization.manager": "org.apache.hadoop.hive.ql.security.authorization.DefaultHiveAuthorizationProvider"
    }
}
```

![Hadoop-cluster Azure Resource Manager sjabloon aanpassen](./media/hdinsight-hadoop-customize-cluster-bootstrap/hdinsight-customize-cluster-bootstrap-arm.png)

## <a name="see-also"></a>Zie ook

* [Apache Hadoop clusters maken in HDInsight](hdinsight-hadoop-provision-linux-clusters.md) vindt u instructies voor het maken van een HDInsight-cluster met behulp van andere aangepaste opties.
* [Script actie scripts voor HDInsight ontwikkelen](hdinsight-hadoop-script-actions-linux.md)
* [Apache Spark op HDInsight-clusters installeren en gebruiken](spark/apache-spark-jupyter-spark-sql-use-portal.md)
* [Installeer en gebruik Apache Giraph in HDInsight-clusters](hdinsight-hadoop-giraph-install.md).

## <a name="appendix-powershell-sample"></a>Bijlage: Power shell-voor beeld

Met dit Power shell-script maakt u een HDInsight-cluster en past u een Hive-instelling aan. Zorg ervoor dat u waarden invoert voor `$nameToken`, `$httpPassword`en `$sshPassword`.

```powershell
####################################
# Set these variables
####################################
#region - used for creating Azure service names
$nameToken = "<ENTER AN ALIAS>"
#endregion

#region - cluster user accounts
$httpUserName = "admin"  #HDInsight cluster username
$httpPassword = '<ENTER A PASSWORD>'

$sshUserName = "sshuser" #HDInsight ssh user name
$sshPassword = '<ENTER A PASSWORD>'
#endregion

####################################
# Service names and varialbes
####################################
#region - service names
$namePrefix = $nameToken.ToLower() + (Get-Date -Format "MMdd")

$resourceGroupName = $namePrefix + "rg"
$hdinsightClusterName = $namePrefix + "hdi"
$defaultStorageAccountName = $namePrefix + "store"
$defaultBlobContainerName = $hdinsightClusterName

$location = "East US"
#endregion


####################################
# Connect to Azure
####################################
#region - Connect to Azure subscription
Write-Host "`nConnecting to your Azure subscription ..." -ForegroundColor Green
$sub = Get-AzSubscription -ErrorAction SilentlyContinue
if(-not($sub))
{
    Connect-AzAccount
}

# If you have multiple subscriptions, set the one to use
# Select-AzSubscription -SubscriptionId "<SUBSCRIPTIONID>"

#endregion

#region - Create an HDInsight cluster
####################################
# Create dependent components
####################################
Write-Host "Creating a resource group ..." -ForegroundColor Green
New-AzResourceGroup `
    -Name  $resourceGroupName `
    -Location $location

Write-Host "Creating the default storage account and default blob container ..."  -ForegroundColor Green
New-AzStorageAccount `
    -ResourceGroupName $resourceGroupName `
    -Name $defaultStorageAccountName `
    -Location $location `
    -SkuName Standard_LRS `
    -Kind StorageV2 `
    -EnableHttpsTrafficOnly 1

# Note: Storage account kind BlobStorage cannot be used as primary storage.

$defaultStorageAccountKey = (Get-AzStorageAccountKey `
                                -ResourceGroupName $resourceGroupName `
                                -Name $defaultStorageAccountName)[0].Value

$defaultStorageContext = New-AzStorageContext `
                                -StorageAccountName $defaultStorageAccountName `
                                -StorageAccountKey $defaultStorageAccountKey

New-AzStorageContainer `
    -Name $defaultBlobContainerName `
    -Context $defaultStorageContext #use the cluster name as the container name

####################################
# Create a configuration object
####################################
$hiveConfigValues = @{"hive.metastore.client.socket.timeout"="90s"}

$config = New-AzHDInsightClusterConfig `
    | Set-AzHDInsightDefaultStorage `
        -StorageAccountName "$defaultStorageAccountName.blob.core.windows.net" `
        -StorageAccountKey $defaultStorageAccountKey `
    | Add-AzHDInsightConfigValue `
        -HiveSite $hiveConfigValues `
        -Spark2Defaults @{}

####################################
# Create an HDInsight cluster
####################################
$httpPW = ConvertTo-SecureString -String $httpPassword -AsPlainText -Force
$httpCredential = New-Object System.Management.Automation.PSCredential($httpUserName,$httpPW)

$sshPW = ConvertTo-SecureString -String $sshPassword -AsPlainText -Force
$sshCredential = New-Object System.Management.Automation.PSCredential($sshUserName,$sshPW)

New-AzHDInsightCluster `
    -ResourceGroupName $resourceGroupName `
    -ClusterName $hdinsightClusterName `
    -Location $location `
    -ClusterSizeInNodes 1 `
    -ClusterType Hadoop `
    -OSType Linux `
    -Version "3.6" `
    -HttpCredential $httpCredential `
    -SshCredential $sshCredential `
    -Config $config

####################################
# Verify the cluster
####################################
Get-AzHDInsightCluster `
    -ClusterName $hdinsightClusterName

#endregion
```
