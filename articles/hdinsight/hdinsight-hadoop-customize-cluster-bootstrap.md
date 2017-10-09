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
# <a name="customize-hdinsight-clusters-using-bootstrap"></a><span data-ttu-id="f8796-103">Anpassa HDInsight-kluster med starttjänsten</span><span class="sxs-lookup"><span data-stu-id="f8796-103">Customize HDInsight clusters using Bootstrap</span></span>

<span data-ttu-id="f8796-104">Ibland vill du tooconfigure hello konfigurationsfiler, bland annat:</span><span class="sxs-lookup"><span data-stu-id="f8796-104">Sometimes, you want tooconfigure hello configuration files, which include:</span></span>

* <span data-ttu-id="f8796-105">clusterIdentity.xml</span><span class="sxs-lookup"><span data-stu-id="f8796-105">clusterIdentity.xml</span></span>
* <span data-ttu-id="f8796-106">Core-site.xml</span><span class="sxs-lookup"><span data-stu-id="f8796-106">core-site.xml</span></span>
* <span data-ttu-id="f8796-107">gateway.XML</span><span class="sxs-lookup"><span data-stu-id="f8796-107">gateway.xml</span></span>
* <span data-ttu-id="f8796-108">hbase env.xml</span><span class="sxs-lookup"><span data-stu-id="f8796-108">hbase-env.xml</span></span>
* <span data-ttu-id="f8796-109">hbase-site.xml</span><span class="sxs-lookup"><span data-stu-id="f8796-109">hbase-site.xml</span></span>
* <span data-ttu-id="f8796-110">hdfs-site.xml</span><span class="sxs-lookup"><span data-stu-id="f8796-110">hdfs-site.xml</span></span>
* <span data-ttu-id="f8796-111">hive-env.xml</span><span class="sxs-lookup"><span data-stu-id="f8796-111">hive-env.xml</span></span>
* <span data-ttu-id="f8796-112">hive-site.xml</span><span class="sxs-lookup"><span data-stu-id="f8796-112">hive-site.xml</span></span>
* <span data-ttu-id="f8796-113">mapred-plats</span><span class="sxs-lookup"><span data-stu-id="f8796-113">mapred-site</span></span>
* <span data-ttu-id="f8796-114">oozie-site.xml</span><span class="sxs-lookup"><span data-stu-id="f8796-114">oozie-site.xml</span></span>
* <span data-ttu-id="f8796-115">oozie env.xml</span><span class="sxs-lookup"><span data-stu-id="f8796-115">oozie-env.xml</span></span>
* <span data-ttu-id="f8796-116">storm-site.xml</span><span class="sxs-lookup"><span data-stu-id="f8796-116">storm-site.xml</span></span>
* <span data-ttu-id="f8796-117">tez-site.xml</span><span class="sxs-lookup"><span data-stu-id="f8796-117">tez-site.xml</span></span>
* <span data-ttu-id="f8796-118">webhcat-site.xml</span><span class="sxs-lookup"><span data-stu-id="f8796-118">webhcat-site.xml</span></span>
* <span data-ttu-id="f8796-119">yarn-site.xml</span><span class="sxs-lookup"><span data-stu-id="f8796-119">yarn-site.xml</span></span>

<span data-ttu-id="f8796-120">Det finns tre metoder toouse bootstrap:</span><span class="sxs-lookup"><span data-stu-id="f8796-120">There are three methods toouse bootstrap:</span></span>

* <span data-ttu-id="f8796-121">Använda Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="f8796-121">Use Azure PowerShell</span></span>
* <span data-ttu-id="f8796-122">Använd .NET SDK</span><span class="sxs-lookup"><span data-stu-id="f8796-122">Use .NET SDK</span></span>
* <span data-ttu-id="f8796-123">Använd Azure Resource Manager-mallar</span><span class="sxs-lookup"><span data-stu-id="f8796-123">Use Azure Resource Manager template</span></span>

[!INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

<span data-ttu-id="f8796-124">Information om hur du installerar ytterligare komponenter på HDInsight-kluster under skapandeprocessen hello finns i:</span><span class="sxs-lookup"><span data-stu-id="f8796-124">For information on installing additional components on HDInsight cluster during hello creation time, see:</span></span>

* [<span data-ttu-id="f8796-125">Anpassa HDInsight-kluster med skriptåtgärder (Linux)</span><span class="sxs-lookup"><span data-stu-id="f8796-125">Customize HDInsight clusters using Script Action (Linux)</span></span>](hdinsight-hadoop-customize-cluster-linux.md)

## <a name="use-azure-powershell"></a><span data-ttu-id="f8796-126">Använda Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="f8796-126">Use Azure PowerShell</span></span>
<span data-ttu-id="f8796-127">följande PowerShell-koden hello anpassar en Hive-konfiguration:</span><span class="sxs-lookup"><span data-stu-id="f8796-127">hello following PowerShell code customizes a Hive configuration:</span></span>

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

<span data-ttu-id="f8796-128">En fullständig fungerande PowerShell-skript finns i [bilaga A](#hdinsight-hadoop-customize-cluster-bootstrap.md/appx-a:-powershell-sample).</span><span class="sxs-lookup"><span data-stu-id="f8796-128">A complete working PowerShell script can be found in [Appendix-A](#hdinsight-hadoop-customize-cluster-bootstrap.md/appx-a:-powershell-sample).</span></span>

<span data-ttu-id="f8796-129">**tooverify hello ändringen:**</span><span class="sxs-lookup"><span data-stu-id="f8796-129">**tooverify hello change:**</span></span>

1. <span data-ttu-id="f8796-130">Logga in toohello [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="f8796-130">Sign on toohello [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="f8796-131">Hello vänstra menyn klickar du på **HDInsight-kluster**.</span><span class="sxs-lookup"><span data-stu-id="f8796-131">From hello left menu, click **HDInsight clusters**.</span></span> <span data-ttu-id="f8796-132">Om du inte ser det klickar du på **fler tjänster** första.</span><span class="sxs-lookup"><span data-stu-id="f8796-132">If you don't see it, click **More services** first.</span></span>
3. <span data-ttu-id="f8796-133">Klicka på hello kluster som du just har skapat med hjälp av hello PowerShell-skript.</span><span class="sxs-lookup"><span data-stu-id="f8796-133">Click hello cluster you just created using hello PowerShell script.</span></span>
4. <span data-ttu-id="f8796-134">Klicka på **instrumentpanelen** från hello överkant hello bladet tooopen hello Ambari UI.</span><span class="sxs-lookup"><span data-stu-id="f8796-134">Click **Dashboard** from hello top of hello blade tooopen hello Ambari UI.</span></span>
5. <span data-ttu-id="f8796-135">Klicka på **Hive** hello vänstra menyn.</span><span class="sxs-lookup"><span data-stu-id="f8796-135">Click **Hive** from hello left menu.</span></span>
6. <span data-ttu-id="f8796-136">Klicka på **HiveServer2** från **sammanfattning**.</span><span class="sxs-lookup"><span data-stu-id="f8796-136">Click **HiveServer2** from **Summary**.</span></span>
7. <span data-ttu-id="f8796-137">Klicka på hello **konfigurationerna** fliken.</span><span class="sxs-lookup"><span data-stu-id="f8796-137">Click hello **Configs** tab.</span></span>
8. <span data-ttu-id="f8796-138">Klicka på **Hive** hello vänstra menyn.</span><span class="sxs-lookup"><span data-stu-id="f8796-138">Click **Hive** from hello left menu.</span></span>
9. <span data-ttu-id="f8796-139">Klicka på hello **Avancerat** fliken.</span><span class="sxs-lookup"><span data-stu-id="f8796-139">Click hello **Advanced** tab.</span></span>
10. <span data-ttu-id="f8796-140">Bläddra nedåt och expandera sedan **avancerade hive-plats**.</span><span class="sxs-lookup"><span data-stu-id="f8796-140">Scroll down and then expand **Advanced hive-site**.</span></span>
11. <span data-ttu-id="f8796-141">Leta efter **hive.metastore.client.socket.timeout** i hello-avsnittet.</span><span class="sxs-lookup"><span data-stu-id="f8796-141">Look for **hive.metastore.client.socket.timeout** in hello section.</span></span>

<span data-ttu-id="f8796-142">Vissa flera exempel på hur du anpassar andra konfigurationsfiler:</span><span class="sxs-lookup"><span data-stu-id="f8796-142">Some more samples on customizing other configuration files:</span></span>

    # hdfs-site.xml configuration
    $HdfsConfigValues = @{ "dfs.blocksize"="64m" } #default is 128MB in HDI 3.0 and 256MB in HDI 2.1

    # core-site.xml configuration
    $CoreConfigValues = @{ "ipc.client.connect.max.retries"="60" } #default 50

    # mapred-site.xml configuration
    $MapRedConfigValues = @{ "mapreduce.task.timeout"="1200000" } #default 600000

    # oozie-site.xml configuration
    $OozieConfigValues = @{ "oozie.service.coord.normal.default.timeout"="150" }  # default 120

<span data-ttu-id="f8796-143">Mer information finns i Azim Uddin blogg med titeln [anpassa HDInsight-kluster skapas](http://blogs.msdn.com/b/bigdatasupport/archive/2014/04/15/customizing-hdinsight-cluster-provisioning-via-powershell-and-net-sdk.aspx).</span><span class="sxs-lookup"><span data-stu-id="f8796-143">For more information, see Azim Uddin's blog titled [Customizing HDInsight Cluster creation](http://blogs.msdn.com/b/bigdatasupport/archive/2014/04/15/customizing-hdinsight-cluster-provisioning-via-powershell-and-net-sdk.aspx).</span></span>

## <a name="use-net-sdk"></a><span data-ttu-id="f8796-144">Använd .NET SDK</span><span class="sxs-lookup"><span data-stu-id="f8796-144">Use .NET SDK</span></span>
<span data-ttu-id="f8796-145">Se [skapa Linux-baserade kluster i HDInsight med hello .NET SDK](hdinsight-hadoop-create-linux-clusters-dotnet-sdk.md#use-bootstrap).</span><span class="sxs-lookup"><span data-stu-id="f8796-145">See [Create Linux-based clusters in HDInsight using hello .NET SDK](hdinsight-hadoop-create-linux-clusters-dotnet-sdk.md#use-bootstrap).</span></span>

## <a name="use-resource-manager-template"></a><span data-ttu-id="f8796-146">Använd Resource Manager-mall</span><span class="sxs-lookup"><span data-stu-id="f8796-146">Use Resource Manager template</span></span>
<span data-ttu-id="f8796-147">Du kan använda bootstrap i Resource Manager-mall:</span><span class="sxs-lookup"><span data-stu-id="f8796-147">You can use bootstrap in Resource Manager template:</span></span>

    "configurations": {
        …
        "hive-site": {
            "hive.metastore.client.connect.retry.delay": "5",
            "hive.execution.engine": "mr",
            "hive.security.authorization.manager": "org.apache.hadoop.hive.ql.security.authorization.DefaultHiveAuthorizationProvider"
        }
    }


![HDInsight Hadoop anpassar klustret bootstrap Azure Resource Manager-mall](./media/hdinsight-hadoop-customize-cluster-bootstrap/hdinsight-customize-cluster-bootstrap-arm.png)

## <a name="see-also"></a><span data-ttu-id="f8796-149">Se även</span><span class="sxs-lookup"><span data-stu-id="f8796-149">See also</span></span>
* <span data-ttu-id="f8796-150">[Skapa Hadoop-kluster i HDInsight] [ hdinsight-provision-cluster] innehåller instruktioner om hur toocreate ett HDInsight-kluster med hjälp av andra anpassade alternativ.</span><span class="sxs-lookup"><span data-stu-id="f8796-150">[Create Hadoop clusters in HDInsight][hdinsight-provision-cluster] provides instructions on how toocreate an HDInsight cluster by using other custom options.</span></span>
* <span data-ttu-id="f8796-151">[Utveckla skriptåtgärd skript för HDInsight][hdinsight-write-script]</span><span class="sxs-lookup"><span data-stu-id="f8796-151">[Develop Script Action scripts for HDInsight][hdinsight-write-script]</span></span>
* <span data-ttu-id="f8796-152">[Installera och använda Spark på HDInsight-kluster][hdinsight-install-spark]</span><span class="sxs-lookup"><span data-stu-id="f8796-152">[Install and use Spark on HDInsight clusters][hdinsight-install-spark]</span></span>
* <span data-ttu-id="f8796-153">[Installera och använda R i HDInsight-kluster][hdinsight-install-r]</span><span class="sxs-lookup"><span data-stu-id="f8796-153">[Install and use R on HDInsight clusters][hdinsight-install-r]</span></span>
* <span data-ttu-id="f8796-154">[Installera och använda Solr på HDInsight-kluster](hdinsight-hadoop-solr-install.md).</span><span class="sxs-lookup"><span data-stu-id="f8796-154">[Install and use Solr on HDInsight clusters](hdinsight-hadoop-solr-install.md).</span></span>
* <span data-ttu-id="f8796-155">[Installera och använda Giraph på HDInsight-kluster](hdinsight-hadoop-giraph-install.md).</span><span class="sxs-lookup"><span data-stu-id="f8796-155">[Install and use Giraph on HDInsight clusters](hdinsight-hadoop-giraph-install.md).</span></span>

[hdinsight-install-spark]: hdinsight-hadoop-spark-install.md
[hdinsight-install-r]: hdinsight-hadoop-r-scripts.md
[hdinsight-write-script]: hdinsight-hadoop-script-actions.md
[hdinsight-provision-cluster]: hdinsight-hadoop-provision-linux-clusters.md
[powershell-install-configure]: /powershell/azureps-cmdlets-docs


[img-hdi-cluster-states]: ./media/hdinsight-hadoop-customize-cluster/HDI-Cluster-state.png "Steg när klustret skapas"

## <a name="appx-a-powershell-sample"></a><span data-ttu-id="f8796-157">Appx-A: PowerShell-exempel</span><span class="sxs-lookup"><span data-stu-id="f8796-157">Appx-A: PowerShell sample</span></span>
<span data-ttu-id="f8796-158">Detta PowerShell-skript skapar ett HDInsight-kluster och anpassar en Hive-inställning:</span><span class="sxs-lookup"><span data-stu-id="f8796-158">This PowerShell script creates an HDInsight cluster and customizes a Hive setting:</span></span>

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
