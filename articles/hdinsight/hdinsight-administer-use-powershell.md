---
title: aaaManage Hadoop-kluster i HDInsight med PowerShell - Azure | Microsoft Docs
description: "Lär dig hur tooperform administrativa uppgifter för hello Hadoop-kluster i HDInsight med hjälp av Azure PowerShell."
services: hdinsight
editor: cgronlun
manager: jhubbard
tags: azure-portal
author: mumian
documentationcenter: 
ms.assetid: bfdfa754-18e5-4ef9-b0d6-2dbdcebc0283
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/25/2017
ms.author: jgao
ms.openlocfilehash: 3df082d752fa8c703db82a54b82b740290af6729
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="manage-hadoop-clusters-in-hdinsight-by-using-azure-powershell"></a><span data-ttu-id="a57f4-103">Hantera Hadoop-kluster i HDInsight med hjälp av Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="a57f4-103">Manage Hadoop clusters in HDInsight by using Azure PowerShell</span></span>
[!INCLUDE [selector](../../includes/hdinsight-portal-management-selector.md)]

<span data-ttu-id="a57f4-104">Azure PowerShell är en kraftfull skriptmiljö som du kan använda toocontrol och automatisera hello distribution och hantering av dina arbetsbelastningar i Azure.</span><span class="sxs-lookup"><span data-stu-id="a57f4-104">Azure PowerShell is a powerful scripting environment that you can use toocontrol and automate hello deployment and management of your workloads in Azure.</span></span> <span data-ttu-id="a57f4-105">I den här artikeln får du lära dig hur toomanage Hadoop-kluster i Azure HDInsight med hjälp av den lokala Azure PowerShell-konsolen via hello använder Windows PowerShell.</span><span class="sxs-lookup"><span data-stu-id="a57f4-105">In this article, you will learn how toomanage Hadoop clusters in Azure HDInsight by using a local Azure PowerShell console through hello use of Windows PowerShell.</span></span> <span data-ttu-id="a57f4-106">Hello lista över hello HDInsight PowerShell-cmdlets, se [cmdlet-referens för HDInsight][hdinsight-powershell-reference].</span><span class="sxs-lookup"><span data-stu-id="a57f4-106">For hello list of hello HDInsight PowerShell cmdlets, see [HDInsight cmdlet reference][hdinsight-powershell-reference].</span></span>

<span data-ttu-id="a57f4-107">**Förutsättningar**</span><span class="sxs-lookup"><span data-stu-id="a57f4-107">**Prerequisites**</span></span>

<span data-ttu-id="a57f4-108">Du måste ha hello följande innan du börjar den här artikeln:</span><span class="sxs-lookup"><span data-stu-id="a57f4-108">Before you begin this article, you must have hello following:</span></span>

* <span data-ttu-id="a57f4-109">**En Azure-prenumeration**.</span><span class="sxs-lookup"><span data-stu-id="a57f4-109">**An Azure subscription**.</span></span> <span data-ttu-id="a57f4-110">Se [Hämta en kostnadsfri utvärderingsversion av Azure](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).</span><span class="sxs-lookup"><span data-stu-id="a57f4-110">See [Get Azure free trial](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).</span></span>

## <a name="install-azure-powershell"></a><span data-ttu-id="a57f4-111">Installera Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="a57f4-111">Install Azure PowerShell</span></span>
[!INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

<span data-ttu-id="a57f4-112">Om du har installerat Azure PowerShell version 0,9 x, måste du avinstallera den innan du installerar en senare version.</span><span class="sxs-lookup"><span data-stu-id="a57f4-112">If you have installed Azure PowerShell version 0.9x, you must uninstall it before installing a newer version.</span></span>

<span data-ttu-id="a57f4-113">toocheck hello version av hello installerad PowerShell:</span><span class="sxs-lookup"><span data-stu-id="a57f4-113">toocheck hello version of hello installed PowerShell:</span></span>

    Get-Module *azure*

<span data-ttu-id="a57f4-114">toouninstall hello äldre version, köra program och funktioner på Kontrollpanelen för hello.</span><span class="sxs-lookup"><span data-stu-id="a57f4-114">toouninstall hello older version, run Programs and Features in hello control panel.</span></span>

## <a name="create-clusters"></a><span data-ttu-id="a57f4-115">Skapa kluster</span><span class="sxs-lookup"><span data-stu-id="a57f4-115">Create clusters</span></span>
<span data-ttu-id="a57f4-116">Se [skapa Linux-baserade kluster i HDInsight med hjälp av Azure PowerShell](hdinsight-hadoop-create-linux-clusters-azure-powershell.md)</span><span class="sxs-lookup"><span data-stu-id="a57f4-116">See [Create Linux-based clusters in HDInsight using Azure PowerShell](hdinsight-hadoop-create-linux-clusters-azure-powershell.md)</span></span>

## <a name="list-clusters"></a><span data-ttu-id="a57f4-117">Lista över kluster</span><span class="sxs-lookup"><span data-stu-id="a57f4-117">List clusters</span></span>
<span data-ttu-id="a57f4-118">Använd följande kommando toolist hello alla kluster i hello aktuell prenumeration:</span><span class="sxs-lookup"><span data-stu-id="a57f4-118">Use hello following command toolist all clusters in hello current subscription:</span></span>

    Get-AzureRmHDInsightCluster

## <a name="show-cluster"></a><span data-ttu-id="a57f4-119">Visa kluster</span><span class="sxs-lookup"><span data-stu-id="a57f4-119">Show cluster</span></span>
<span data-ttu-id="a57f4-120">Använd följande kommando tooshow information i ett visst kluster i hello aktuell prenumeration hello:</span><span class="sxs-lookup"><span data-stu-id="a57f4-120">Use hello following command tooshow details of a specific cluster in hello current subscription:</span></span>

    Get-AzureRmHDInsightCluster -ClusterName <Cluster Name>

## <a name="delete-clusters"></a><span data-ttu-id="a57f4-121">Ta bort kluster</span><span class="sxs-lookup"><span data-stu-id="a57f4-121">Delete clusters</span></span>
<span data-ttu-id="a57f4-122">Använd följande kommando toodelete ett kluster hello:</span><span class="sxs-lookup"><span data-stu-id="a57f4-122">Use hello following command toodelete a cluster:</span></span>

    Remove-AzureRmHDInsightCluster -ClusterName <Cluster Name>

<span data-ttu-id="a57f4-123">Du kan också ta bort ett kluster genom att ta bort hello resursgruppen som innehåller hello klustret.</span><span class="sxs-lookup"><span data-stu-id="a57f4-123">You can also delete a cluster by removing hello resource group that contains hello cluster.</span></span> <span data-ttu-id="a57f4-124">Observera detta tar bort alla hello resurser i hello grupp, inklusive hello standardkontot för lagring.</span><span class="sxs-lookup"><span data-stu-id="a57f4-124">Please note, this will delete all hello resources in hello group including hello default storage account.</span></span>

    Remove-AzureRmResourceGroup -Name <Resource Group Name>

## <a name="scale-clusters"></a><span data-ttu-id="a57f4-125">Skala kluster</span><span class="sxs-lookup"><span data-stu-id="a57f4-125">Scale clusters</span></span>
<span data-ttu-id="a57f4-126">hello klustret skalning funktionen kan du toochange hello antalet arbetarnoder som används av ett kluster som körs i Azure HDInsight utan toore-skapa hello-kluster.</span><span class="sxs-lookup"><span data-stu-id="a57f4-126">hello cluster scaling feature allows you toochange hello number of worker nodes used by a cluster that is running in Azure HDInsight without having toore-create hello cluster.</span></span>

> [!NOTE]
> <span data-ttu-id="a57f4-127">Endast kluster med HDInsight version 3.1.3 eller högre stöds.</span><span class="sxs-lookup"><span data-stu-id="a57f4-127">Only clusters with HDInsight version 3.1.3 or higher are supported.</span></span> <span data-ttu-id="a57f4-128">Du kan kontrollera hello-egenskapssidan om du är osäker på hello version av klustret.</span><span class="sxs-lookup"><span data-stu-id="a57f4-128">If you are unsure of hello version of your cluster, you can check hello Properties page.</span></span>  <span data-ttu-id="a57f4-129">Se [listan och visa](hdinsight-administer-use-portal-linux.md#list-and-show-clusters).</span><span class="sxs-lookup"><span data-stu-id="a57f4-129">See [List and show clusters](hdinsight-administer-use-portal-linux.md#list-and-show-clusters).</span></span>
>
>

<span data-ttu-id="a57f4-130">hello konsekvenser av att ändra hello antalet datanoder för varje typ av kluster som stöds av HDInsight:</span><span class="sxs-lookup"><span data-stu-id="a57f4-130">hello impact of changing hello number of data nodes for each type of cluster supported by HDInsight:</span></span>

* <span data-ttu-id="a57f4-131">Hadoop</span><span class="sxs-lookup"><span data-stu-id="a57f4-131">Hadoop</span></span>

    <span data-ttu-id="a57f4-132">Sömlöst kan du öka hello antalet arbetarnoder i ett Hadoop-kluster som körs utan att påverka alla väntande eller körs jobb.</span><span class="sxs-lookup"><span data-stu-id="a57f4-132">You can seamlessly increase hello number of worker nodes in a Hadoop cluster that is running without impacting any pending or running jobs.</span></span> <span data-ttu-id="a57f4-133">Nya jobb kan också skicka medan hello-åtgärd pågår.</span><span class="sxs-lookup"><span data-stu-id="a57f4-133">New jobs can also be submitted while hello operation is in progress.</span></span> <span data-ttu-id="a57f4-134">Fel i en åtgärd för skalning hanteras korrekt så att hello klustret alltid lämnas i ett fungerande tillstånd.</span><span class="sxs-lookup"><span data-stu-id="a57f4-134">Failures in a scaling operation are gracefully handled so that hello cluster is always left in a functional state.</span></span>

    <span data-ttu-id="a57f4-135">När ett Hadoop-kluster skalas genom att minska hello antalet datanoder hello tjänster i hello kluster har startats om.</span><span class="sxs-lookup"><span data-stu-id="a57f4-135">When a Hadoop cluster is scaled down by reducing hello number of data nodes, some of hello services in hello cluster are restarted.</span></span> <span data-ttu-id="a57f4-136">Detta medför att alla körs och väntande jobb toofail på hello hello skalning åtgärden har slutförts.</span><span class="sxs-lookup"><span data-stu-id="a57f4-136">This causes all running and pending jobs toofail at hello completion of hello scaling operation.</span></span> <span data-ttu-id="a57f4-137">Du kan dock skicka hello jobb när hello åtgärden är klar.</span><span class="sxs-lookup"><span data-stu-id="a57f4-137">You can, however, resubmit hello jobs once hello operation is complete.</span></span>
* <span data-ttu-id="a57f4-138">HBase</span><span class="sxs-lookup"><span data-stu-id="a57f4-138">HBase</span></span>

    <span data-ttu-id="a57f4-139">Sömlöst kan du lägga till eller ta bort noder tooyour HBase-kluster medan den körs.</span><span class="sxs-lookup"><span data-stu-id="a57f4-139">You can seamlessly add or remove nodes tooyour HBase cluster while it is running.</span></span> <span data-ttu-id="a57f4-140">Regional servrar balanseras automatiskt inom några minuter efter att du avslutat hello skalning igen.</span><span class="sxs-lookup"><span data-stu-id="a57f4-140">Regional Servers are automatically balanced within a few minutes of completing hello scaling operation.</span></span> <span data-ttu-id="a57f4-141">Du kan också manuellt balansera hello regionala servrar genom att logga in toohello headnode för klustret och köra hello följande kommandon från en kommandotolk:</span><span class="sxs-lookup"><span data-stu-id="a57f4-141">However, you can also manually balance hello regional servers by logging in toohello headnode of cluster and running hello following commands from a command prompt window:</span></span>

        >pushd %HBASE_HOME%\bin
        >hbase shell
        >balancer
* <span data-ttu-id="a57f4-142">Storm</span><span class="sxs-lookup"><span data-stu-id="a57f4-142">Storm</span></span>

    <span data-ttu-id="a57f4-143">Sömlöst kan du lägga till eller ta bort data noder tooyour Storm-kluster medan den körs.</span><span class="sxs-lookup"><span data-stu-id="a57f4-143">You can seamlessly add or remove data nodes tooyour Storm cluster while it is running.</span></span> <span data-ttu-id="a57f4-144">Men när en slutförd hello skalning åtgärden måste toorebalance hello-topologi.</span><span class="sxs-lookup"><span data-stu-id="a57f4-144">But after a successful completion of hello scaling operation, you will need toorebalance hello topology.</span></span>

    <span data-ttu-id="a57f4-145">Ombalansering kan utföras på två sätt:</span><span class="sxs-lookup"><span data-stu-id="a57f4-145">Rebalancing can be accomplished in two ways:</span></span>

  * <span data-ttu-id="a57f4-146">Storm webbgränssnittet</span><span class="sxs-lookup"><span data-stu-id="a57f4-146">Storm web UI</span></span>
  * <span data-ttu-id="a57f4-147">Verktyget kommandoradsgränssnittet (CLI)</span><span class="sxs-lookup"><span data-stu-id="a57f4-147">Command-line interface (CLI) tool</span></span>

    <span data-ttu-id="a57f4-148">Se toohello [Apache Storm dokumentationen](http://storm.apache.org/documentation/Understanding-the-parallelism-of-a-Storm-topology.html) för mer information.</span><span class="sxs-lookup"><span data-stu-id="a57f4-148">Please refer toohello [Apache Storm documentation](http://storm.apache.org/documentation/Understanding-the-parallelism-of-a-Storm-topology.html) for more details.</span></span>

    <span data-ttu-id="a57f4-149">hello Storm webbgränssnittet är tillgänglig på hello HDInsight-kluster:</span><span class="sxs-lookup"><span data-stu-id="a57f4-149">hello Storm web UI is available on hello HDInsight cluster:</span></span>

    ![HDInsight storm skala balansera](./media/hdinsight-administer-use-management-portal/hdinsight.portal.scale.cluster.png)

    <span data-ttu-id="a57f4-151">Här är ett exempel hur toouse hello CLI kommandot toorebalance hello Storm-topologi:</span><span class="sxs-lookup"><span data-stu-id="a57f4-151">Here is an example how toouse hello CLI command toorebalance hello Storm topology:</span></span>

        ## Reconfigure hello topology "mytopology" toouse 5 worker processes,
        ## hello spout "blue-spout" toouse 3 executors, and
        ## hello bolt "yellow-bolt" toouse 10 executors
        $ storm rebalance mytopology -n 5 -e blue-spout=3 -e yellow-bolt=10

<span data-ttu-id="a57f4-152">toochange hello Hadoop klusterstorleken med hjälp av Azure PowerShell, kör följande kommando från en klientdator hello:</span><span class="sxs-lookup"><span data-stu-id="a57f4-152">toochange hello Hadoop cluster size by using Azure PowerShell, run hello following command from a client machine:</span></span>

    Set-AzureRmHDInsightClusterSize -ClusterName <Cluster Name> -TargetInstanceCount <NewSize>


## <a name="grantrevoke-access"></a><span data-ttu-id="a57f4-153">Bevilja/återkalla åtkomst</span><span class="sxs-lookup"><span data-stu-id="a57f4-153">Grant/revoke access</span></span>
<span data-ttu-id="a57f4-154">HDInsight-kluster har hello efter http-webbtjänster (alla dessa tjänster har RESTful slutpunkter):</span><span class="sxs-lookup"><span data-stu-id="a57f4-154">HDInsight clusters have hello following HTTP web services (all of these services have RESTful endpoints):</span></span>

* <span data-ttu-id="a57f4-155">ODBC</span><span class="sxs-lookup"><span data-stu-id="a57f4-155">ODBC</span></span>
* <span data-ttu-id="a57f4-156">JDBC</span><span class="sxs-lookup"><span data-stu-id="a57f4-156">JDBC</span></span>
* <span data-ttu-id="a57f4-157">Ambari</span><span class="sxs-lookup"><span data-stu-id="a57f4-157">Ambari</span></span>
* <span data-ttu-id="a57f4-158">Oozie</span><span class="sxs-lookup"><span data-stu-id="a57f4-158">Oozie</span></span>
* <span data-ttu-id="a57f4-159">Templeton</span><span class="sxs-lookup"><span data-stu-id="a57f4-159">Templeton</span></span>

<span data-ttu-id="a57f4-160">Som standard beviljas dessa tjänster för åtkomst.</span><span class="sxs-lookup"><span data-stu-id="a57f4-160">By default, these services are granted for access.</span></span> <span data-ttu-id="a57f4-161">Du kan återkalla/bevilja hello åtkomst.</span><span class="sxs-lookup"><span data-stu-id="a57f4-161">You can revoke/grant hello access.</span></span> <span data-ttu-id="a57f4-162">toorevoke:</span><span class="sxs-lookup"><span data-stu-id="a57f4-162">toorevoke:</span></span>

    Revoke-AzureRmHDInsightHttpServicesAccess -ClusterName <Cluster Name>

<span data-ttu-id="a57f4-163">toogrant:</span><span class="sxs-lookup"><span data-stu-id="a57f4-163">toogrant:</span></span>

    $clusterName = "<HDInsight Cluster Name>"

    # Credential option 1
    $hadoopUserName = "admin"
    $hadoopUserPassword = "<Enter hello Password>"
    $hadoopUserPW = ConvertTo-SecureString -String $hadoopUserPassword -AsPlainText -Force
    $credential = New-Object System.Management.Automation.PSCredential($hadoopUserName,$hadoopUserPW)

    # Credential option 2
    #$credential = Get-Credential -Message "Enter hello HTTP username and password:" -UserName "admin"

    Grant-AzureRmHDInsightHttpServicesAccess -ClusterName $clusterName -HttpCredential $credential

> [!NOTE]
> <span data-ttu-id="a57f4-164">Genom att bevilja/återkalla hello åtkomst, återställs hello klustret användarnamn och lösenord.</span><span class="sxs-lookup"><span data-stu-id="a57f4-164">By granting/revoking hello access, you will reset hello cluster user name and password.</span></span>
>
>

<span data-ttu-id="a57f4-165">Detta kan även göras via hello Portal.</span><span class="sxs-lookup"><span data-stu-id="a57f4-165">This can also be done via hello Portal.</span></span> <span data-ttu-id="a57f4-166">Se [administrera HDInsight med hjälp av hello Azure-portalen][hdinsight-admin-portal].</span><span class="sxs-lookup"><span data-stu-id="a57f4-166">See [Administer HDInsight by using hello Azure portal][hdinsight-admin-portal].</span></span>

## <a name="update-http-user-credentials"></a><span data-ttu-id="a57f4-167">Uppdatera autentiseringsuppgifterna för HTTP</span><span class="sxs-lookup"><span data-stu-id="a57f4-167">Update HTTP user credentials</span></span>
<span data-ttu-id="a57f4-168">Det är hello samma procedur som [HTTP att bevilja/återkalla åtkomst](#grant/revoke-access). Om hello klustret har beviljats hello HTTP-åtkomst, måste du först återkalla den.</span><span class="sxs-lookup"><span data-stu-id="a57f4-168">It is hello same procedure as [Grant/revoke HTTP access](#grant/revoke-access).If hello cluster has been granted hello HTTP access, you must first revoke it.</span></span>  <span data-ttu-id="a57f4-169">Och sedan ge hello åtkomst med nya HTTP-autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="a57f4-169">And then grant hello access with new HTTP user credentials.</span></span>

## <a name="find-hello-default-storage-account"></a><span data-ttu-id="a57f4-170">Hitta hello standardkontot för lagring</span><span class="sxs-lookup"><span data-stu-id="a57f4-170">Find hello default storage account</span></span>
<span data-ttu-id="a57f4-171">hello följande Powershell-skript visar hur tooget hello standard lagringskontonamn och hello standard lagringskontonyckel för ett kluster.</span><span class="sxs-lookup"><span data-stu-id="a57f4-171">hello following Powershell script demonstrates how tooget hello default storage account name and hello default storage account key for a cluster.</span></span>

    $clusterName = "<HDInsight Cluster Name>"

    $cluster = Get-AzureRmHDInsightCluster -ClusterName $clusterName
    $resourceGroupName = $cluster.ResourceGroup
    $defaultStorageAccountName = ($cluster.DefaultStorageAccount).Replace(".blob.core.windows.net", "")
    $defaultBlobContainerName = $cluster.DefaultStorageContainer
    $defaultStorageAccountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $resourceGroupName -Name $defaultStorageAccountName)[0].Value
    $defaultStorageAccountContext = New-AzureStorageContext -StorageAccountName $defaultStorageAccountName -StorageAccountKey $defaultStorageAccountKey

## <a name="find-hello-resource-group"></a><span data-ttu-id="a57f4-172">Hitta hello resursgruppen</span><span class="sxs-lookup"><span data-stu-id="a57f4-172">Find hello resource group</span></span>
<span data-ttu-id="a57f4-173">Varje HDInsight-klustret tillhör tooan Azure-resursgrupp i hello Resource Manager-läge.</span><span class="sxs-lookup"><span data-stu-id="a57f4-173">In hello Resource Manager mode, each HDInsight cluster belongs tooan Azure resource group.</span></span>  <span data-ttu-id="a57f4-174">toofind hello resursgrupp:</span><span class="sxs-lookup"><span data-stu-id="a57f4-174">toofind hello resource group:</span></span>

    $clusterName = "<HDInsight Cluster Name>"

    $cluster = Get-AzureRmHDInsightCluster -ClusterName $clusterName
    $resourceGroupName = $cluster.ResourceGroup


## <a name="submit-jobs"></a><span data-ttu-id="a57f4-175">Skicka jobb</span><span class="sxs-lookup"><span data-stu-id="a57f4-175">Submit jobs</span></span>
<span data-ttu-id="a57f4-176">**toosubmit MapReduce-jobb**</span><span class="sxs-lookup"><span data-stu-id="a57f4-176">**toosubmit MapReduce jobs**</span></span>

<span data-ttu-id="a57f4-177">Se [kör Hadoop-MapReduce-exempel i Windows-baserade HDInsight](hdinsight-run-samples.md).</span><span class="sxs-lookup"><span data-stu-id="a57f4-177">See [Run Hadoop MapReduce samples in Windows-based HDInsight](hdinsight-run-samples.md).</span></span>

<span data-ttu-id="a57f4-178">**toosubmit Hive-jobb**</span><span class="sxs-lookup"><span data-stu-id="a57f4-178">**toosubmit Hive jobs**</span></span>

<span data-ttu-id="a57f4-179">Se [köra Hive-frågor med hjälp av PowerShell](hdinsight-hadoop-use-hive-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="a57f4-179">See [Run Hive queries using PowerShell](hdinsight-hadoop-use-hive-powershell.md).</span></span>

<span data-ttu-id="a57f4-180">**toosubmit Pig-jobb**</span><span class="sxs-lookup"><span data-stu-id="a57f4-180">**toosubmit Pig jobs**</span></span>

<span data-ttu-id="a57f4-181">Se [köra Pig-jobb med hjälp av PowerShell](hdinsight-hadoop-use-pig-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="a57f4-181">See [Run Pig jobs using PowerShell](hdinsight-hadoop-use-pig-powershell.md).</span></span>

<span data-ttu-id="a57f4-182">**toosubmit Sqoop jobb**</span><span class="sxs-lookup"><span data-stu-id="a57f4-182">**toosubmit Sqoop jobs**</span></span>

<span data-ttu-id="a57f4-183">Se [använda Sqoop med HDInsight](hdinsight-use-sqoop.md).</span><span class="sxs-lookup"><span data-stu-id="a57f4-183">See [Use Sqoop with HDInsight](hdinsight-use-sqoop.md).</span></span>

<span data-ttu-id="a57f4-184">**toosubmit Oozie jobb**</span><span class="sxs-lookup"><span data-stu-id="a57f4-184">**toosubmit Oozie jobs**</span></span>

<span data-ttu-id="a57f4-185">Se [Använd Oozie med Hadoop toodefine och kör ett arbetsflöde i HDInsight](hdinsight-use-oozie.md).</span><span class="sxs-lookup"><span data-stu-id="a57f4-185">See [Use Oozie with Hadoop toodefine and run a workflow in HDInsight](hdinsight-use-oozie.md).</span></span>

## <a name="upload-data-tooazure-blob-storage"></a><span data-ttu-id="a57f4-186">Ladda upp data tooAzure Blob storage</span><span class="sxs-lookup"><span data-stu-id="a57f4-186">Upload data tooAzure Blob storage</span></span>
<span data-ttu-id="a57f4-187">Se [överför data tooHDInsight][hdinsight-upload-data].</span><span class="sxs-lookup"><span data-stu-id="a57f4-187">See [Upload data tooHDInsight][hdinsight-upload-data].</span></span>

## <a name="see-also"></a><span data-ttu-id="a57f4-188">Se även</span><span class="sxs-lookup"><span data-stu-id="a57f4-188">See Also</span></span>
* <span data-ttu-id="a57f4-189">[Cmdlet-referensdokumentationen för HDInsight][hdinsight-powershell-reference]</span><span class="sxs-lookup"><span data-stu-id="a57f4-189">[HDInsight cmdlet reference documentation][hdinsight-powershell-reference]</span></span>
* <span data-ttu-id="a57f4-190">[Administrera HDInsight med hjälp av hello Azure-portalen][hdinsight-admin-portal]</span><span class="sxs-lookup"><span data-stu-id="a57f4-190">[Administer HDInsight by using hello Azure portal][hdinsight-admin-portal]</span></span>
* <span data-ttu-id="a57f4-191">[Administrera HDInsight med ett kommandoradsgränssnitt][hdinsight-admin-cli]</span><span class="sxs-lookup"><span data-stu-id="a57f4-191">[Administer HDInsight using a command-line interface][hdinsight-admin-cli]</span></span>
* <span data-ttu-id="a57f4-192">[Skapa HDInsight-kluster][hdinsight-provision]</span><span class="sxs-lookup"><span data-stu-id="a57f4-192">[Create HDInsight clusters][hdinsight-provision]</span></span>
* <span data-ttu-id="a57f4-193">[Ladda upp data tooHDInsight][hdinsight-upload-data]</span><span class="sxs-lookup"><span data-stu-id="a57f4-193">[Upload data tooHDInsight][hdinsight-upload-data]</span></span>
* <span data-ttu-id="a57f4-194">[Skicka Hadoop-jobb via programmering][hdinsight-submit-jobs]</span><span class="sxs-lookup"><span data-stu-id="a57f4-194">[Submit Hadoop jobs programmatically][hdinsight-submit-jobs]</span></span>
* <span data-ttu-id="a57f4-195">[Kom igång med Azure HDInsight][hdinsight-get-started]</span><span class="sxs-lookup"><span data-stu-id="a57f4-195">[Get started with Azure HDInsight][hdinsight-get-started]</span></span>

[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md
[hdinsight-provision]: hdinsight-hadoop-provision-linux-clusters.md
[hdinsight-provision-custom-options]: hdinsight-hadoop-provision-linux-clusters.md#configuration
[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md

[hdinsight-admin-cli]: hdinsight-administer-use-command-line.md
[hdinsight-admin-portal]: hdinsight-administer-use-management-portal.md
[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md
[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-use-mapreduce]: hdinsight-use-mapreduce.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-flight]: hdinsight-analyze-flight-delay-data.md

[hdinsight-powershell-reference]: https://msdn.microsoft.com/library/dn858087.aspx

[powershell-install-configure]: /powershell/azureps-cmdlets-docs

[image-hdi-ps-provision]: ./media/hdinsight-administer-use-powershell/HDI.PS.Provision.png
