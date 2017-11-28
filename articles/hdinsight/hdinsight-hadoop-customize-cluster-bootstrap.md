---
title: Anpassa HDInsight-kluster med bootstrap - Azure | Microsoft Docs
description: "Lär dig hur du anpassar HDInsight-kluster med starttjänsten."
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
ms.openlocfilehash: c7a6fafa90eac66774d564c82c926c662baf784c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="customize-hdinsight-clusters-using-bootstrap"></a><span data-ttu-id="76f6f-103">Anpassa HDInsight-kluster med starttjänsten</span><span class="sxs-lookup"><span data-stu-id="76f6f-103">Customize HDInsight clusters using Bootstrap</span></span>

<span data-ttu-id="76f6f-104">Ibland kan vill du konfigurera konfigurationsfiler, bland annat:</span><span class="sxs-lookup"><span data-stu-id="76f6f-104">Sometimes, you want to configure the configuration files, which include:</span></span>

* <span data-ttu-id="76f6f-105">clusterIdentity.xml</span><span class="sxs-lookup"><span data-stu-id="76f6f-105">clusterIdentity.xml</span></span>
* <span data-ttu-id="76f6f-106">Core-site.xml</span><span class="sxs-lookup"><span data-stu-id="76f6f-106">core-site.xml</span></span>
* <span data-ttu-id="76f6f-107">gateway.XML</span><span class="sxs-lookup"><span data-stu-id="76f6f-107">gateway.xml</span></span>
* <span data-ttu-id="76f6f-108">hbase env.xml</span><span class="sxs-lookup"><span data-stu-id="76f6f-108">hbase-env.xml</span></span>
* <span data-ttu-id="76f6f-109">hbase-site.xml</span><span class="sxs-lookup"><span data-stu-id="76f6f-109">hbase-site.xml</span></span>
* <span data-ttu-id="76f6f-110">hdfs-site.xml</span><span class="sxs-lookup"><span data-stu-id="76f6f-110">hdfs-site.xml</span></span>
* <span data-ttu-id="76f6f-111">hive-env.xml</span><span class="sxs-lookup"><span data-stu-id="76f6f-111">hive-env.xml</span></span>
* <span data-ttu-id="76f6f-112">hive-site.xml</span><span class="sxs-lookup"><span data-stu-id="76f6f-112">hive-site.xml</span></span>
* <span data-ttu-id="76f6f-113">mapred-plats</span><span class="sxs-lookup"><span data-stu-id="76f6f-113">mapred-site</span></span>
* <span data-ttu-id="76f6f-114">oozie-site.xml</span><span class="sxs-lookup"><span data-stu-id="76f6f-114">oozie-site.xml</span></span>
* <span data-ttu-id="76f6f-115">oozie env.xml</span><span class="sxs-lookup"><span data-stu-id="76f6f-115">oozie-env.xml</span></span>
* <span data-ttu-id="76f6f-116">storm-site.xml</span><span class="sxs-lookup"><span data-stu-id="76f6f-116">storm-site.xml</span></span>
* <span data-ttu-id="76f6f-117">tez-site.xml</span><span class="sxs-lookup"><span data-stu-id="76f6f-117">tez-site.xml</span></span>
* <span data-ttu-id="76f6f-118">webhcat-site.xml</span><span class="sxs-lookup"><span data-stu-id="76f6f-118">webhcat-site.xml</span></span>
* <span data-ttu-id="76f6f-119">yarn-site.xml</span><span class="sxs-lookup"><span data-stu-id="76f6f-119">yarn-site.xml</span></span>

<span data-ttu-id="76f6f-120">Det finns tre metoder för att använda bootstrap:</span><span class="sxs-lookup"><span data-stu-id="76f6f-120">There are three methods to use bootstrap:</span></span>

* <span data-ttu-id="76f6f-121">Använda Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="76f6f-121">Use Azure PowerShell</span></span>
* <span data-ttu-id="76f6f-122">Använd .NET SDK</span><span class="sxs-lookup"><span data-stu-id="76f6f-122">Use .NET SDK</span></span>
* <span data-ttu-id="76f6f-123">Använd Azure Resource Manager-mallar</span><span class="sxs-lookup"><span data-stu-id="76f6f-123">Use Azure Resource Manager template</span></span>

[!INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

<span data-ttu-id="76f6f-124">Information om hur du installerar ytterligare komponenter på HDInsight-kluster under tiden för skapandet av finns i:</span><span class="sxs-lookup"><span data-stu-id="76f6f-124">For information on installing additional components on HDInsight cluster during the creation time, see:</span></span>

* [<span data-ttu-id="76f6f-125">Anpassa HDInsight-kluster med skriptåtgärder (Linux)</span><span class="sxs-lookup"><span data-stu-id="76f6f-125">Customize HDInsight clusters using Script Action (Linux)</span></span>](hdinsight-hadoop-customize-cluster-linux.md)

## <a name="use-azure-powershell"></a><span data-ttu-id="76f6f-126">Använda Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="76f6f-126">Use Azure PowerShell</span></span>
<span data-ttu-id="76f6f-127">Följande PowerShell-kod anpassar en Hive-konfiguration:</span><span class="sxs-lookup"><span data-stu-id="76f6f-127">The following PowerShell code customizes a Hive configuration:</span></span>

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

<span data-ttu-id="76f6f-128">En fullständig fungerande PowerShell-skript finns i [bilaga A](#hdinsight-hadoop-customize-cluster-bootstrap.md/appx-a:-powershell-sample).</span><span class="sxs-lookup"><span data-stu-id="76f6f-128">A complete working PowerShell script can be found in [Appendix-A](#hdinsight-hadoop-customize-cluster-bootstrap.md/appx-a:-powershell-sample).</span></span>

<span data-ttu-id="76f6f-129">**Så här kontrollerar du ändringen:**</span><span class="sxs-lookup"><span data-stu-id="76f6f-129">**To verify the change:**</span></span>

1. <span data-ttu-id="76f6f-130">Logga in på [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="76f6f-130">Sign on to the [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="76f6f-131">I den vänstra menyn klickar du på **HDInsight-kluster**.</span><span class="sxs-lookup"><span data-stu-id="76f6f-131">From the left menu, click **HDInsight clusters**.</span></span> <span data-ttu-id="76f6f-132">Om du inte ser det klickar du på **fler tjänster** första.</span><span class="sxs-lookup"><span data-stu-id="76f6f-132">If you don't see it, click **More services** first.</span></span>
3. <span data-ttu-id="76f6f-133">Klicka på det kluster som du just har skapat med hjälp av PowerShell-skript.</span><span class="sxs-lookup"><span data-stu-id="76f6f-133">Click the cluster you just created using the PowerShell script.</span></span>
4. <span data-ttu-id="76f6f-134">Klicka på **instrumentpanelen** högst upp på bladet för att öppna Ambari UI.</span><span class="sxs-lookup"><span data-stu-id="76f6f-134">Click **Dashboard** from the top of the blade to open the Ambari UI.</span></span>
5. <span data-ttu-id="76f6f-135">Klicka på **Hive** i den vänstra menyn.</span><span class="sxs-lookup"><span data-stu-id="76f6f-135">Click **Hive** from the left menu.</span></span>
6. <span data-ttu-id="76f6f-136">Klicka på **HiveServer2** från **sammanfattning**.</span><span class="sxs-lookup"><span data-stu-id="76f6f-136">Click **HiveServer2** from **Summary**.</span></span>
7. <span data-ttu-id="76f6f-137">Klicka på den **konfigurationerna** fliken.</span><span class="sxs-lookup"><span data-stu-id="76f6f-137">Click the **Configs** tab.</span></span>
8. <span data-ttu-id="76f6f-138">Klicka på **Hive** i den vänstra menyn.</span><span class="sxs-lookup"><span data-stu-id="76f6f-138">Click **Hive** from the left menu.</span></span>
9. <span data-ttu-id="76f6f-139">Klicka på den **Avancerat** fliken.</span><span class="sxs-lookup"><span data-stu-id="76f6f-139">Click the **Advanced** tab.</span></span>
10. <span data-ttu-id="76f6f-140">Bläddra nedåt och expandera sedan **avancerade hive-plats**.</span><span class="sxs-lookup"><span data-stu-id="76f6f-140">Scroll down and then expand **Advanced hive-site**.</span></span>
11. <span data-ttu-id="76f6f-141">Leta efter **hive.metastore.client.socket.timeout** i avsnittet.</span><span class="sxs-lookup"><span data-stu-id="76f6f-141">Look for **hive.metastore.client.socket.timeout** in the section.</span></span>

<span data-ttu-id="76f6f-142">Vissa flera exempel på hur du anpassar andra konfigurationsfiler:</span><span class="sxs-lookup"><span data-stu-id="76f6f-142">Some more samples on customizing other configuration files:</span></span>

    # hdfs-site.xml configuration
    $HdfsConfigValues = @{ "dfs.blocksize"="64m" } #default is 128MB in HDI 3.0 and 256MB in HDI 2.1

    # core-site.xml configuration
    $CoreConfigValues = @{ "ipc.client.connect.max.retries"="60" } #default 50

    # mapred-site.xml configuration
    $MapRedConfigValues = @{ "mapreduce.task.timeout"="1200000" } #default 600000

    # oozie-site.xml configuration
    $OozieConfigValues = @{ "oozie.service.coord.normal.default.timeout"="150" }  # default 120

<span data-ttu-id="76f6f-143">Mer information finns i Azim Uddin blogg med titeln [anpassa HDInsight-kluster skapas](http://blogs.msdn.com/b/bigdatasupport/archive/2014/04/15/customizing-hdinsight-cluster-provisioning-via-powershell-and-net-sdk.aspx).</span><span class="sxs-lookup"><span data-stu-id="76f6f-143">For more information, see Azim Uddin's blog titled [Customizing HDInsight Cluster creation](http://blogs.msdn.com/b/bigdatasupport/archive/2014/04/15/customizing-hdinsight-cluster-provisioning-via-powershell-and-net-sdk.aspx).</span></span>

## <a name="use-net-sdk"></a><span data-ttu-id="76f6f-144">Använd .NET SDK</span><span class="sxs-lookup"><span data-stu-id="76f6f-144">Use .NET SDK</span></span>
<span data-ttu-id="76f6f-145">Se [skapa Linux-baserade kluster i HDInsight med hjälp av .NET SDK](hdinsight-hadoop-create-linux-clusters-dotnet-sdk.md#use-bootstrap).</span><span class="sxs-lookup"><span data-stu-id="76f6f-145">See [Create Linux-based clusters in HDInsight using the .NET SDK](hdinsight-hadoop-create-linux-clusters-dotnet-sdk.md#use-bootstrap).</span></span>

## <a name="use-resource-manager-template"></a><span data-ttu-id="76f6f-146">Använd Resource Manager-mall</span><span class="sxs-lookup"><span data-stu-id="76f6f-146">Use Resource Manager template</span></span>
<span data-ttu-id="76f6f-147">Du kan använda bootstrap i Resource Manager-mall:</span><span class="sxs-lookup"><span data-stu-id="76f6f-147">You can use bootstrap in Resource Manager template:</span></span>

    "configurations": {
        …
        "hive-site": {
            "hive.metastore.client.connect.retry.delay": "5",
            "hive.execution.engine": "mr",
            "hive.security.authorization.manager": "org.apache.hadoop.hive.ql.security.authorization.DefaultHiveAuthorizationProvider"
        }
    }


![HDInsight Hadoop anpassar klustret bootstrap Azure Resource Manager-mall](./media/hdinsight-hadoop-customize-cluster-bootstrap/hdinsight-customize-cluster-bootstrap-arm.png)

## <a name="see-also"></a><span data-ttu-id="76f6f-149">Se även</span><span class="sxs-lookup"><span data-stu-id="76f6f-149">See also</span></span>
* <span data-ttu-id="76f6f-150">[Skapa Hadoop-kluster i HDInsight] [ hdinsight-provision-cluster] innehåller instruktioner om hur du skapar ett HDInsight-kluster med hjälp av andra anpassade alternativ.</span><span class="sxs-lookup"><span data-stu-id="76f6f-150">[Create Hadoop clusters in HDInsight][hdinsight-provision-cluster] provides instructions on how to create an HDInsight cluster by using other custom options.</span></span>
* <span data-ttu-id="76f6f-151">[Utveckla skriptåtgärd skript för HDInsight][hdinsight-write-script]</span><span class="sxs-lookup"><span data-stu-id="76f6f-151">[Develop Script Action scripts for HDInsight][hdinsight-write-script]</span></span>
* <span data-ttu-id="76f6f-152">[Installera och använda Spark på HDInsight-kluster][hdinsight-install-spark]</span><span class="sxs-lookup"><span data-stu-id="76f6f-152">[Install and use Spark on HDInsight clusters][hdinsight-install-spark]</span></span>
* <span data-ttu-id="76f6f-153">[Installera och använda R i HDInsight-kluster][hdinsight-install-r]</span><span class="sxs-lookup"><span data-stu-id="76f6f-153">[Install and use R on HDInsight clusters][hdinsight-install-r]</span></span>
* <span data-ttu-id="76f6f-154">[Installera och använda Solr på HDInsight-kluster](hdinsight-hadoop-solr-install.md).</span><span class="sxs-lookup"><span data-stu-id="76f6f-154">[Install and use Solr on HDInsight clusters](hdinsight-hadoop-solr-install.md).</span></span>
* <span data-ttu-id="76f6f-155">[Installera och använda Giraph på HDInsight-kluster](hdinsight-hadoop-giraph-install.md).</span><span class="sxs-lookup"><span data-stu-id="76f6f-155">[Install and use Giraph on HDInsight clusters](hdinsight-hadoop-giraph-install.md).</span></span>

[hdinsight-install-spark]: hdinsight-hadoop-spark-install.md
[hdinsight-install-r]: hdinsight-hadoop-r-scripts.md
[hdinsight-write-script]: hdinsight-hadoop-script-actions.md
[hdinsight-provision-cluster]: hdinsight-hadoop-provision-linux-clusters.md
[powershell-install-configure]: /powershell/azureps-cmdlets-docs


<span data-ttu-id="76f6f-156">[img-hdi-cluster-states]: ./media/hdinsight-hadoop-customize-cluster/HDI-Cluster-state.png "Steg när klustret skapas"</span><span class="sxs-lookup"><span data-stu-id="76f6f-156">[img-hdi-cluster-states]: ./media/hdinsight-hadoop-customize-cluster/HDI-Cluster-state.png "Stages during cluster creation"</span></span>

## <a name="appx-a-powershell-sample"></a><span data-ttu-id="76f6f-157">Appx-A: PowerShell-exempel</span><span class="sxs-lookup"><span data-stu-id="76f6f-157">Appx-A: PowerShell sample</span></span>
<span data-ttu-id="76f6f-158">Detta PowerShell-skript skapar ett HDInsight-kluster och anpassar en Hive-inställning:</span><span class="sxs-lookup"><span data-stu-id="76f6f-158">This PowerShell script creates an HDInsight cluster and customizes a Hive setting:</span></span>

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
    # Connect to Azure
    ####################################
    #region - Connect to Azure subscription
    Write-Host "`nConnecting to your Azure subscription ..." -ForegroundColor Green
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

    Write-Host "Creating the default storage account and default blob container ..."  -ForegroundColor Green
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
        -Context $defaultStorageContext #use the cluster name as the container name

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
    # Verify the cluster
    ####################################
    Get-AzureRmHDInsightCluster -ClusterName $hdinsightClusterName

    #endregion
