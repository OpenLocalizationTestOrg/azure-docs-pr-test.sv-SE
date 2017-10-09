---
title: aaaCustomize HDInsight-kluster med bootstrap - Azure | Microsoft Docs
description: "Lär dig hur toocustomize HDInsight-kluster med starttjänsten."
services: hdinsight
documentationcenter: 
author: mumian
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: ab2ebf0c-e961-4e95-8151-9724ee22d769
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/25/2017
ms.author: jgao
ms.openlocfilehash: 0029680fd1aa0e9e6aa9cdf667256c31b7ddc565
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="customize-hdinsight-clusters-using-bootstrap"></a>Anpassa HDInsight-kluster med starttjänsten

Ibland vill du tooconfigure hello konfigurationsfiler, bland annat:

* clusterIdentity.xml
* Core-site.xml
* gateway.XML
* hbase env.xml
* hbase-site.xml
* hdfs-site.xml
* hive-env.xml
* hive-site.xml
* mapred-plats
* oozie-site.xml
* oozie env.xml
* storm-site.xml
* tez-site.xml
* webhcat-site.xml
* yarn-site.xml

Det finns tre metoder toouse bootstrap:

* Använda Azure PowerShell
* Använd .NET SDK
* Använd Azure Resource Manager-mallar

[!INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

Information om hur du installerar ytterligare komponenter på HDInsight-kluster under skapandeprocessen hello finns i:

* [Anpassa HDInsight-kluster med skriptåtgärder (Linux)](hdinsight-hadoop-customize-cluster-linux.md)

## <a name="use-azure-powershell"></a>Använda Azure PowerShell
följande PowerShell-koden hello anpassar en Hive-konfiguration:

    # hive-site.xml configuration
    $hiveConfigValues = @{ "hive.metastore.client.socket.timeout"="90" }

    $config = New-AzureRmHDInsightClusterConfig `
        | Set-AzureRmHDInsightDefaultStorage `
            -StorageAccountName "$defaultStorageAccountName.blob.core.windows.net" `
            -StorageAccountKey $defaultStorageAccountKey `
        | Add-AzureRmHDInsightConfigValues `
            -HiveSite $hiveConfigValues 

    New-AzureRmHDInsightCluster `
        -ResourceGroupName $existingResourceGroupName `
        -ClusterName $clusterName `
        -Location $location `
        -ClusterSizeInNodes $clusterSizeInNodes `
        -ClusterType Hadoop `
        -OSType Linux `
        -Version "3.5" `
        -HttpCredential $httpCredential `
        -Config $config 

En fullständig fungerande PowerShell-skript finns i [bilaga A](#hdinsight-hadoop-customize-cluster-bootstrap.md/appx-a:-powershell-sample).

**tooverify hello ändringen:**

1. Logga in toohello [Azure-portalen](https://portal.azure.com).
2. Hello vänstra menyn klickar du på **HDInsight-kluster**. Om du inte ser det klickar du på **fler tjänster** första.
3. Klicka på hello kluster som du just har skapat med hjälp av hello PowerShell-skript.
4. Klicka på **instrumentpanelen** från hello överkant hello bladet tooopen hello Ambari UI.
5. Klicka på **Hive** hello vänstra menyn.
6. Klicka på **HiveServer2** från **sammanfattning**.
7. Klicka på hello **konfigurationerna** fliken.
8. Klicka på **Hive** hello vänstra menyn.
9. Klicka på hello **Avancerat** fliken.
10. Bläddra nedåt och expandera sedan **avancerade hive-plats**.
11. Leta efter **hive.metastore.client.socket.timeout** i hello-avsnittet.

Vissa flera exempel på hur du anpassar andra konfigurationsfiler:

    # hdfs-site.xml configuration
    $HdfsConfigValues = @{ "dfs.blocksize"="64m" } #default is 128MB in HDI 3.0 and 256MB in HDI 2.1

    # core-site.xml configuration
    $CoreConfigValues = @{ "ipc.client.connect.max.retries"="60" } #default 50

    # mapred-site.xml configuration
    $MapRedConfigValues = @{ "mapreduce.task.timeout"="1200000" } #default 600000

    # oozie-site.xml configuration
    $OozieConfigValues = @{ "oozie.service.coord.normal.default.timeout"="150" }  # default 120

Mer information finns i Azim Uddin blogg med titeln [anpassa HDInsight-kluster skapas](http://blogs.msdn.com/b/bigdatasupport/archive/2014/04/15/customizing-hdinsight-cluster-provisioning-via-powershell-and-net-sdk.aspx).

## <a name="use-net-sdk"></a>Använd .NET SDK
Se [skapa Linux-baserade kluster i HDInsight med hello .NET SDK](hdinsight-hadoop-create-linux-clusters-dotnet-sdk.md#use-bootstrap).

## <a name="use-resource-manager-template"></a>Använd Resource Manager-mall
Du kan använda bootstrap i Resource Manager-mall:

    "configurations": {
        …
        "hive-site": {
            "hive.metastore.client.connect.retry.delay": "5",
            "hive.execution.engine": "mr",
            "hive.security.authorization.manager": "org.apache.hadoop.hive.ql.security.authorization.DefaultHiveAuthorizationProvider"
        }
    }


![HDInsight Hadoop anpassar klustret bootstrap Azure Resource Manager-mall](./media/hdinsight-hadoop-customize-cluster-bootstrap/hdinsight-customize-cluster-bootstrap-arm.png)

## <a name="see-also"></a>Se även
* [Skapa Hadoop-kluster i HDInsight] [ hdinsight-provision-cluster] innehåller instruktioner om hur toocreate ett HDInsight-kluster med hjälp av andra anpassade alternativ.
* [Utveckla skriptåtgärd skript för HDInsight][hdinsight-write-script]
* [Installera och använda Spark på HDInsight-kluster][hdinsight-install-spark]
* [Installera och använda R i HDInsight-kluster][hdinsight-install-r]
* [Installera och använda Solr på HDInsight-kluster](hdinsight-hadoop-solr-install.md).
* [Installera och använda Giraph på HDInsight-kluster](hdinsight-hadoop-giraph-install.md).

[hdinsight-install-spark]: hdinsight-hadoop-spark-install.md
[hdinsight-install-r]: hdinsight-hadoop-r-scripts.md
[hdinsight-write-script]: hdinsight-hadoop-script-actions.md
[hdinsight-provision-cluster]: hdinsight-hadoop-provision-linux-clusters.md
[powershell-install-configure]: /powershell/azureps-cmdlets-docs


[img-hdi-cluster-states]: ./media/hdinsight-hadoop-customize-cluster/HDI-Cluster-state.png "Steg när klustret skapas"

## <a name="appx-a-powershell-sample"></a>Appx-A: PowerShell-exempel
Detta PowerShell-skript skapar ett HDInsight-kluster och anpassar en Hive-inställning:

    ####################################
    # Set these variables
    ####################################
    #region - used for creating Azure service names
    $nameToken = "<ENTER AN ALIAS>" 
    #endregion

    #region - cluster user accounts
    $httpUserName = "admin"  #HDInsight cluster username
    $httpPassword = "<ENTER A PASSWORD>" #"<Enter a Password>"

    $sshUserName = "sshuser" #HDInsight ssh user name
    $sshPassword = "<ENTER A PASSWORD>" #"<Enter a Password>"
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

    $location = "East US 2"
    #endregion

    # Treat all errors as terminating
    $ErrorActionPreference = "Stop"

    ####################################
    # Connect tooAzure
    ####################################
    #region - Connect tooAzure subscription
    Write-Host "`nConnecting tooyour Azure subscription ..." -ForegroundColor Green
    try{Get-AzureRmContext}
    catch{Login-AzureRmAccount}
    #endregion

    #region - Create an HDInsight cluster
    ####################################
    # Create dependent components
    ####################################
    Write-Host "Creating a resource group ..." -ForegroundColor Green
    New-AzureRmResourceGroup `
        -Name  $resourceGroupName `
        -Location $location

    Write-Host "Creating hello default storage account and default blob container ..."  -ForegroundColor Green
    New-AzureRmStorageAccount `
        -ResourceGroupName $resourceGroupName `
        -Name $defaultStorageAccountName `
        -Location $location `
        -Type Standard_GRS

    $defaultStorageAccountKey = (Get-AzureRmStorageAccountKey `
                                    -ResourceGroupName $resourceGroupName `
                                    -Name $defaultStorageAccountName)[0].Value
    $defaultStorageContext = New-AzureStorageContext `
                                    -StorageAccountName $defaultStorageAccountName `
                                    -StorageAccountKey $defaultStorageAccountKey
    New-AzureStorageContainer `
        -Name $defaultBlobContainerName `
        -Context $defaultStorageContext #use hello cluster name as hello container name

    ####################################
    # Create a configuration object
    ####################################
    $hiveConfigValues = @{ "hive.metastore.client.socket.timeout"="90" }

    $config = New-AzureRmHDInsightClusterConfig `
        | Set-AzureRmHDInsightDefaultStorage `
            -StorageAccountName "$defaultStorageAccountName.blob.core.windows.net" `
            -StorageAccountKey $defaultStorageAccountKey `
        | Add-AzureRmHDInsightConfigValues `
            -HiveSite $hiveConfigValues 

    ####################################
    # Create an HDInsight cluster
    ####################################
    $httpPW = ConvertTo-SecureString -String $httpPassword -AsPlainText -Force
    $httpCredential = New-Object System.Management.Automation.PSCredential($httpUserName,$httpPW)

    $sshPW = ConvertTo-SecureString -String $sshPassword -AsPlainText -Force
    $sshCredential = New-Object System.Management.Automation.PSCredential($sshUserName,$sshPW)

    New-AzureRmHDInsightCluster `
        -ResourceGroupName $resourceGroupName `
        -ClusterName $hdinsightClusterName `
        -Location $location `
        -ClusterSizeInNodes 1 `
        -ClusterType Hadoop `
        -OSType Linux `
        -Version "3.5" `
        -HttpCredential $httpCredential `
        -SshCredential $sshCredential `
        -Config $config

    ####################################
    # Verify hello cluster
    ####################################
    Get-AzureRmHDInsightCluster -ClusterName $hdinsightClusterName

    #endregion
