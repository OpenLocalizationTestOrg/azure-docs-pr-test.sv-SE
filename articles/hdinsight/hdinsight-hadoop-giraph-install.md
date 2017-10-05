---
title: "Installera och använda Giraph på Hadoop-kluster i HDInsight - Azure | Microsoft Docs"
description: "Lär dig hur du anpassar HDInsight-kluster med Giraph och hur du använder Giraph."
services: hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 77a1d0e0-55de-4e61-98a0-060914fb7ca0
ms.service: hdinsight
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/05/2016
ms.author: nitinme
ROBOTS: NOINDEX
ms.openlocfilehash: f0eb5c1f457380600463a370043f03e6d655a02c
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="install-and-use-giraph-on-windows-based-hdinsight-clusters"></a><span data-ttu-id="0aff0-103">Installera och använda Giraph på Windows-baserade HDInsight-kluster</span><span class="sxs-lookup"><span data-stu-id="0aff0-103">Install and use Giraph on Windows-based HDInsight clusters</span></span>

<span data-ttu-id="0aff0-104">Lär dig hur du anpassar Windows-baserat HDInsight-kluster med Giraph med skriptåtgärder och hur du använder Giraph för att bearbeta stora diagram.</span><span class="sxs-lookup"><span data-stu-id="0aff0-104">Learn how to customize Windows based HDInsight cluster with Giraph using Script Action, and how to use Giraph to process large-scale graphs.</span></span> <span data-ttu-id="0aff0-105">Information om hur du använder Giraph med ett Linux-baserade kluster finns i [installera Giraph på HDInsight Hadoop-kluster (Linux)](hdinsight-hadoop-giraph-install-linux.md).</span><span class="sxs-lookup"><span data-stu-id="0aff0-105">For information on using Giraph with a Linux-based cluster, see [Install Giraph on HDInsight Hadoop clusters (Linux)](hdinsight-hadoop-giraph-install-linux.md).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="0aff0-106">Stegen i det här dokumentet fungerar endast med Windows-baserade HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="0aff0-106">The steps in this document only work with Windows-based HDInsight clusters.</span></span> <span data-ttu-id="0aff0-107">HDInsight är endast tillgängligt i Windows för versioner som är lägre än HDInsight 3.4.</span><span class="sxs-lookup"><span data-stu-id="0aff0-107">HDInsight is only available on Windows for versions lower than HDInsight 3.4.</span></span> <span data-ttu-id="0aff0-108">Linux är det enda operativsystemet som används med HDInsight version 3.4 och senare.</span><span class="sxs-lookup"><span data-stu-id="0aff0-108">Linux is the only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="0aff0-109">Mer information finns i [HDInsight-avveckling på Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="0aff0-109">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span> <span data-ttu-id="0aff0-110">Information om hur du installerar Giraph på en Linux-baserade HDInsight-kluster finns i [installera Giraph på HDInsight Hadoop-kluster (Linux)](hdinsight-hadoop-giraph-install-linux.md).</span><span class="sxs-lookup"><span data-stu-id="0aff0-110">For information on how to install Giraph on a Linux-based HDInsight cluster, see [Install Giraph on HDInsight Hadoop clusters (Linux)](hdinsight-hadoop-giraph-install-linux.md).</span></span>


<span data-ttu-id="0aff0-111">Du kan installera Giraph på någon typ av kluster (Hadoop, Storm, HBase, Spark) på Azure HDInsight med hjälp av *skriptåtgärd*.</span><span class="sxs-lookup"><span data-stu-id="0aff0-111">You can install Giraph on any type of cluster (Hadoop, Storm, HBase, Spark) on Azure HDInsight by using *Script Action*.</span></span> <span data-ttu-id="0aff0-112">Ett exempelskript för att installera Giraph på ett HDInsight-kluster är tillgänglig från en skrivskyddad Azure storage blob [https://hdiconfigactions.blob.core.windows.net/giraphconfigactionv01/giraph-installer-v01.ps1](https://hdiconfigactions.blob.core.windows.net/giraphconfigactionv01/giraph-installer-v01.ps1).</span><span class="sxs-lookup"><span data-stu-id="0aff0-112">A sample script to install Giraph on an HDInsight cluster is available from a read-only Azure storage blob at [https://hdiconfigactions.blob.core.windows.net/giraphconfigactionv01/giraph-installer-v01.ps1](https://hdiconfigactions.blob.core.windows.net/giraphconfigactionv01/giraph-installer-v01.ps1).</span></span> <span data-ttu-id="0aff0-113">Exempelskriptet fungerar bara med HDInsight-kluster av version 3.1.</span><span class="sxs-lookup"><span data-stu-id="0aff0-113">The sample script works only with HDInsight cluster version 3.1.</span></span> <span data-ttu-id="0aff0-114">Mer information om HDInsight-kluster-versioner finns [HDInsight-kluster-versioner](hdinsight-component-versioning.md).</span><span class="sxs-lookup"><span data-stu-id="0aff0-114">For more information on HDInsight cluster versions, see [HDInsight cluster versions](hdinsight-component-versioning.md).</span></span>

<span data-ttu-id="0aff0-115">**Relaterade artiklar**</span><span class="sxs-lookup"><span data-stu-id="0aff0-115">**Related articles**</span></span>

* [<span data-ttu-id="0aff0-116">Installera Giraph på HDInsight Hadoop-kluster (Linux)</span><span class="sxs-lookup"><span data-stu-id="0aff0-116">Install Giraph on HDInsight Hadoop clusters (Linux)</span></span>](hdinsight-hadoop-giraph-install-linux.md)
* <span data-ttu-id="0aff0-117">[Skapa Hadoop-kluster i HDInsight](hdinsight-provision-clusters.md): allmän information om hur du skapar HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="0aff0-117">[Create Hadoop clusters in HDInsight](hdinsight-provision-clusters.md): general information on creating HDInsight clusters.</span></span>
* <span data-ttu-id="0aff0-118">[Anpassa HDInsight-kluster med skriptåtgärder][hdinsight-cluster-customize]: allmän information om hur du anpassar HDInsight-kluster med skriptåtgärder.</span><span class="sxs-lookup"><span data-stu-id="0aff0-118">[Customize HDInsight cluster using Script Action][hdinsight-cluster-customize]: general information on customizing HDInsight clusters using Script Action.</span></span>
* <span data-ttu-id="0aff0-119">[Utveckla skriptåtgärd skript för HDInsight](hdinsight-hadoop-script-actions.md).</span><span class="sxs-lookup"><span data-stu-id="0aff0-119">[Develop Script Action scripts for HDInsight](hdinsight-hadoop-script-actions.md).</span></span>

## <a name="what-is-giraph"></a><span data-ttu-id="0aff0-120">Vad är Giraph?</span><span class="sxs-lookup"><span data-stu-id="0aff0-120">What is Giraph?</span></span>
<span data-ttu-id="0aff0-121"><a href="http://giraph.apache.org/" target="_blank">Apache Giraph</a> kan du utföra bearbetning med hjälp av Hadoop och kan användas med Azure HDInsight.</span><span class="sxs-lookup"><span data-stu-id="0aff0-121"><a href="http://giraph.apache.org/" target="_blank">Apache Giraph</a> allows you to perform graph processing by using Hadoop, and can be used with Azure HDInsight.</span></span> <span data-ttu-id="0aff0-122">Diagram modell relationer mellan objekt, till exempel anslutningar mellan routrar i ett större nätverk som Internet eller relationer mellan personer på sociala nätverk (kallas ibland ett sociala diagram).</span><span class="sxs-lookup"><span data-stu-id="0aff0-122">Graphs model relationships between objects, such as the connections between routers on a large network like the Internet, or relationships between people on social networks (sometimes referred to as a social graph).</span></span> <span data-ttu-id="0aff0-123">Diagrammet bearbetning kan du därför om förhållanden mellan objekt i ett diagram som:</span><span class="sxs-lookup"><span data-stu-id="0aff0-123">Graph processing allows you to reason about the relationships between objects in a graph, such as:</span></span>

* <span data-ttu-id="0aff0-124">Identifiera potentiella vänner baserat på din aktuella relationer.</span><span class="sxs-lookup"><span data-stu-id="0aff0-124">Identifying potential friends based on your current relationships.</span></span>
* <span data-ttu-id="0aff0-125">Identifiera den kortaste vägen mellan två datorer i ett nätverk.</span><span class="sxs-lookup"><span data-stu-id="0aff0-125">Identifying the shortest route between two computers in a network.</span></span>
* <span data-ttu-id="0aff0-126">Beräkning av sidan rangordningen för webbsidor.</span><span class="sxs-lookup"><span data-stu-id="0aff0-126">Calculating the page rank of webpages.</span></span>

## <a name="install-giraph-using-portal"></a><span data-ttu-id="0aff0-127">Installera Giraph med hjälp av portalen</span><span class="sxs-lookup"><span data-stu-id="0aff0-127">Install Giraph using portal</span></span>
1. <span data-ttu-id="0aff0-128">Skapa ett kluster med hjälp av den **skapa anpassade** alternativ, enligt beskrivningen i [skapa Hadoop-kluster i HDInsight](hdinsight-provision-clusters.md).</span><span class="sxs-lookup"><span data-stu-id="0aff0-128">Start creating a cluster by using the **CUSTOM CREATE** option, as described at [Create Hadoop clusters in HDInsight](hdinsight-provision-clusters.md).</span></span>
2. <span data-ttu-id="0aff0-129">På den **skriptåtgärder** sidan i guiden klickar du på **lägga till skriptåtgärd** att ge information om skriptåtgärd, enligt nedan:</span><span class="sxs-lookup"><span data-stu-id="0aff0-129">On the **Script Actions** page of the wizard, click **add script action** to provide details about the script action, as shown below:</span></span>

    <span data-ttu-id="0aff0-130">![Använd skriptåtgärder för att anpassa ett kluster](./media/hdinsight-hadoop-giraph-install/hdi-script-action-giraph.png "Använd skriptåtgärder att anpassa ett kluster")</span><span class="sxs-lookup"><span data-stu-id="0aff0-130">![Use Script Action to customize a cluster](./media/hdinsight-hadoop-giraph-install/hdi-script-action-giraph.png "Use Script Action to customize a cluster")</span></span>

    <table border='1'>
        <tr><th><span data-ttu-id="0aff0-131">Egenskap</span><span class="sxs-lookup"><span data-stu-id="0aff0-131">Property</span></span></th><th><span data-ttu-id="0aff0-132">Värde</span><span class="sxs-lookup"><span data-stu-id="0aff0-132">Value</span></span></th></tr>
        <tr><td><span data-ttu-id="0aff0-133">Namn</span><span class="sxs-lookup"><span data-stu-id="0aff0-133">Name</span></span></td>
            <td><span data-ttu-id="0aff0-134">Ange ett namn för skriptåtgärden.</span><span class="sxs-lookup"><span data-stu-id="0aff0-134">Specify a name for the script action.</span></span> <span data-ttu-id="0aff0-135">Till exempel <b>installera Giraph</b>.</span><span class="sxs-lookup"><span data-stu-id="0aff0-135">For example, <b>Install Giraph</b>.</span></span></td></tr>
        <tr><td><span data-ttu-id="0aff0-136">Skript-URI</span><span class="sxs-lookup"><span data-stu-id="0aff0-136">Script URI</span></span></td>
            <td><span data-ttu-id="0aff0-137">Ange den identifierare URI (Uniform Resource) till det skript som anropas för att anpassa klustret.</span><span class="sxs-lookup"><span data-stu-id="0aff0-137">Specify the Uniform Resource Identifier (URI) to the script that is invoked to customize the cluster.</span></span> <span data-ttu-id="0aff0-138">Till exempel <i>https://hdiconfigactions.blob.core.windows.net/giraphconfigactionv01/giraph-installer-v01.ps1</i></span><span class="sxs-lookup"><span data-stu-id="0aff0-138">For example, <i>https://hdiconfigactions.blob.core.windows.net/giraphconfigactionv01/giraph-installer-v01.ps1</i></span></span></td></tr>
        <tr><td><span data-ttu-id="0aff0-139">Nodtyp</span><span class="sxs-lookup"><span data-stu-id="0aff0-139">Node Type</span></span></td>
            <td><span data-ttu-id="0aff0-140">Ange de noder som anpassning skriptet körs.</span><span class="sxs-lookup"><span data-stu-id="0aff0-140">Specify the nodes on which the customization script is run.</span></span> <span data-ttu-id="0aff0-141">Du kan välja <b>alla noder</b>, <b>huvudnoder endast</b>, eller <b>arbetarnoder endast</b>.</span><span class="sxs-lookup"><span data-stu-id="0aff0-141">You can choose <b>All nodes</b>, <b>Head nodes only</b>, or <b>Worker nodes only</b>.</span></span>
        <tr><td><span data-ttu-id="0aff0-142">Parametrar</span><span class="sxs-lookup"><span data-stu-id="0aff0-142">Parameters</span></span></td>
            <td><span data-ttu-id="0aff0-143">Ange parametrar, om det krävs av skriptet.</span><span class="sxs-lookup"><span data-stu-id="0aff0-143">Specify the parameters, if required by the script.</span></span> <span data-ttu-id="0aff0-144">Skript för att installera Giraph kräver inte parametrar, så du kan lämna den tom.</span><span class="sxs-lookup"><span data-stu-id="0aff0-144">The script to install Giraph does not require any parameters, so you can leave this blank.</span></span></td></tr>
    </table>

    <span data-ttu-id="0aff0-145">Du kan lägga till fler än en skriptåtgärd för att installera flera komponenter i klustret.</span><span class="sxs-lookup"><span data-stu-id="0aff0-145">You can add more than one script action to install multiple components on the cluster.</span></span> <span data-ttu-id="0aff0-146">När du har lagt till skripten, klicka på bockmarkeringen om du vill skapa klustret.</span><span class="sxs-lookup"><span data-stu-id="0aff0-146">After you have added the scripts, click the checkmark to start creating the cluster.</span></span>

## <a name="use-giraph"></a><span data-ttu-id="0aff0-147">Använd Giraph</span><span class="sxs-lookup"><span data-stu-id="0aff0-147">Use Giraph</span></span>
<span data-ttu-id="0aff0-148">Vi använder SimpleShortestPathsComputation-exempel för att demonstrera grundläggande <a href = "http://people.apache.org/~edwardyoon/documents/pregel.pdf">Pregel</a> implementering för att hitta den kortaste sökvägen mellan objekt i ett diagram.</span><span class="sxs-lookup"><span data-stu-id="0aff0-148">We use the SimpleShortestPathsComputation example to demonstrate the basic <a href = "http://people.apache.org/~edwardyoon/documents/pregel.pdf">Pregel</a> implementation for finding the shortest path between objects in a graph.</span></span> <span data-ttu-id="0aff0-149">Använd följande steg för att ladda upp exempeldata och exempel jar genom att köra ett jobb med hjälp av SimpleShortestPathsComputation exempel och visa sedan resultaten.</span><span class="sxs-lookup"><span data-stu-id="0aff0-149">Use the following steps to upload the sample data and the sample jar, run a job by using the SimpleShortestPathsComputation example, and then view the results.</span></span>

1. <span data-ttu-id="0aff0-150">Överför en exempeldatafil till Azure Blob storage.</span><span class="sxs-lookup"><span data-stu-id="0aff0-150">Upload a sample data file to Azure Blob storage.</span></span> <span data-ttu-id="0aff0-151">Skapa en ny fil med namnet på din lokala arbetsstation **tiny_graph.txt**.</span><span class="sxs-lookup"><span data-stu-id="0aff0-151">On your local workstation, create a new file named **tiny_graph.txt**.</span></span> <span data-ttu-id="0aff0-152">Den bör innehålla följande rader:</span><span class="sxs-lookup"><span data-stu-id="0aff0-152">It should contain the following lines:</span></span>

        [0,0,[[1,1],[3,3]]]
        [1,0,[[0,1],[2,2],[3,1]]]
        [2,0,[[1,2],[4,4]]]
        [3,0,[[0,3],[1,1],[4,4]]]
        [4,0,[[3,4],[2,4]]]

    <span data-ttu-id="0aff0-153">Överför filen tiny_graph.txt till den primära lagringsplatsen för ditt HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="0aff0-153">Upload the tiny_graph.txt file to the primary storage for your HDInsight cluster.</span></span> <span data-ttu-id="0aff0-154">Instruktioner om hur du överför data finns [överföra data för Hadoop-jobb i HDInsight](hdinsight-upload-data.md).</span><span class="sxs-lookup"><span data-stu-id="0aff0-154">For instructions on how to upload data, see [Upload data for Hadoop jobs in HDInsight](hdinsight-upload-data.md).</span></span>

    <span data-ttu-id="0aff0-155">Informationen beskriver en relation mellan objekt i en riktat diagram i formatet [källa\_id, källa\_värde [[målet\_id], [kant\_värde],...]].</span><span class="sxs-lookup"><span data-stu-id="0aff0-155">This data describes a relationship between objects in a directed graph, by using the format [source\_id, source\_value,[[dest\_id], [edge\_value],...]].</span></span> <span data-ttu-id="0aff0-156">Varje rad representerar en relation mellan en **källa\_id** objekt och en eller flera **dest\_id** objekt.</span><span class="sxs-lookup"><span data-stu-id="0aff0-156">Each line represents a relationship between a **source\_id** object and one or more **dest\_id** objects.</span></span> <span data-ttu-id="0aff0-157">Den **kant\_värdet** (eller vikt) kan betraktas som styrkan eller avståndet mellan anslutningen mellan **source_id** och **dest\_id**.</span><span class="sxs-lookup"><span data-stu-id="0aff0-157">The **edge\_value** (or weight) can be thought of as the strength or distance of the connection between **source_id** and **dest\_id**.</span></span>

    <span data-ttu-id="0aff0-158">Dras ut, och använder värdet (eller vikt) som avståndet mellan objekt, informationen som beskrivs ovan kan se ut så här:</span><span class="sxs-lookup"><span data-stu-id="0aff0-158">Drawn out, and using the value (or weight) as the distance between objects, the above data might look like this:</span></span>

    ![tiny_graph.txt ritas som cirklar med rader med olika avståndet mellan](./media/hdinsight-hadoop-giraph-install/giraph-graph.png)
2. <span data-ttu-id="0aff0-160">Kör SimpleShortestPathsComputation exemplet.</span><span class="sxs-lookup"><span data-stu-id="0aff0-160">Run the SimpleShortestPathsComputation example.</span></span> <span data-ttu-id="0aff0-161">Använd följande Azure PowerShell-cmdlets för att köra exemplet med hjälp av filen tiny_graph.txt som indata.</span><span class="sxs-lookup"><span data-stu-id="0aff0-161">Use the following Azure PowerShell cmdlets to run the example by using the tiny_graph.txt file as input.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="0aff0-162">Azure PowerShell-stöd för hantering av HDInsight-resurser med hjälp av Azure Service Manager **är föråldrat** och togs bort den 1 januari 2017.</span><span class="sxs-lookup"><span data-stu-id="0aff0-162">Azure PowerShell support for managing HDInsight resources using Azure Service Manager is **deprecated**, and was removed on January 1, 2017.</span></span> <span data-ttu-id="0aff0-163">I stegen i det här dokumentet används de nya HDInsight-cmdletarna som fungerar med Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="0aff0-163">The steps in this document use the new HDInsight cmdlets that work with Azure Resource Manager.</span></span>
    >
    > <span data-ttu-id="0aff0-164">Följ stegen i [Installera och konfigurera Azure PowerShell](/powershell/azureps-cmdlets-docs) för att installera den senaste versionen av Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="0aff0-164">Please follow the steps in [Install and configure Azure PowerShell](/powershell/azureps-cmdlets-docs) to install the latest version of Azure PowerShell.</span></span> <span data-ttu-id="0aff0-165">Om du har skript som behöver ändras för att använda de nya cmdletarna som fungerar med Azure Resource Manager, hittar du mer information i [Migrera till Azure Resource Manager-baserade utvecklingsverktyg för HDInsight-kluster](hdinsight-hadoop-development-using-azure-resource-manager.md).</span><span class="sxs-lookup"><span data-stu-id="0aff0-165">If you have scripts that need to be modified to use the new cmdlets that work with Azure Resource Manager, see [Migrating to Azure Resource Manager-based development tools for HDInsight clusters](hdinsight-hadoop-development-using-azure-resource-manager.md) for more information.</span></span>

    ```powershell
    $clusterName = "clustername"
    # Giraph examples jar
    $jarFile = "wasb:///example/jars/giraph-examples.jar"
    # Arguments for this job
    $jobArguments = "org.apache.giraph.examples.SimpleShortestPathsComputation",
                    "-ca", "mapred.job.tracker=headnodehost:9010",
                    "-vif", "org.apache.giraph.io.formats.JsonLongDoubleFloatDoubleVertexInputFormat",
                    "-vip", "wasb:///example/data/tiny_graph.txt",
                    "-vof", "org.apache.giraph.io.formats.IdWithValueTextOutputFormat",
                    "-op",  "wasb:///example/output/shortestpaths",
                    "-w", "2"
    # Create the definition
    $jobDefinition = New-AzureHDInsightMapReduceJobDefinition
        -JarFile $jarFile
        -ClassName "org.apache.giraph.GiraphRunner"
        -Arguments $jobArguments

    # Run the job, write output to the Azure PowerShell window
    $job = Start-AzureHDInsightJob -Cluster $clusterName -JobDefinition $jobDefinition
    Write-Host "Wait for the job to complete ..." -ForegroundColor Green
    Wait-AzureHDInsightJob -Job $job
    Write-Host "STDERR"
    Get-AzureHDInsightJobOutput -Cluster $clusterName -JobId $job.JobId -StandardError
    Write-Host "Display the standard output ..." -ForegroundColor Green
    Get-AzureHDInsightJobOutput -Cluster $clusterName -JobId $job.JobId -StandardOutput
    ```

    <span data-ttu-id="0aff0-166">I exemplet ovan ersätter **klusternamn** med namnet på ditt HDInsight-kluster som har Giraph installerad.</span><span class="sxs-lookup"><span data-stu-id="0aff0-166">In the above example, replace **clustername** with the name of your HDInsight cluster that has Giraph installed.</span></span>
3. <span data-ttu-id="0aff0-167">Granska resultaten.</span><span class="sxs-lookup"><span data-stu-id="0aff0-167">View the results.</span></span> <span data-ttu-id="0aff0-168">När jobbet har slutförts resultatet kommer att lagras i två utgående filer i den **wasb: / / / exempel/out/shotestpaths** mapp.</span><span class="sxs-lookup"><span data-stu-id="0aff0-168">Once the job has finished, the results will be stored in two output files in the **wasb:///example/out/shotestpaths** folder.</span></span> <span data-ttu-id="0aff0-169">Filerna kallas **del-m-00001** och **del-m-00002**.</span><span class="sxs-lookup"><span data-stu-id="0aff0-169">The files are called **part-m-00001** and **part-m-00002**.</span></span> <span data-ttu-id="0aff0-170">Utför följande steg för att hämta och visa utdata:</span><span class="sxs-lookup"><span data-stu-id="0aff0-170">Perform the following steps to download and view the output:</span></span>

    ```powershell
    $subscriptionName = "<SubscriptionName>"       # Azure subscription name
    $storageAccountName = "<StorageAccountName>"   # Azure Storage account name
    $containerName = "<ContainerName>"             # Blob storage container name

    # Select the current subscription
    Select-AzureSubscription $subscriptionName

    # Create the Storage account context object
    $storageAccountKey = Get-AzureStorageKey $storageAccountName | %{ $_.Primary }
    $storageContext = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey

    # Download the job output to the workstation
    Get-AzureStorageBlobContent -Container $containerName -Blob example/output/shortestpaths/part-m-00001 -Context $storageContext -Force
    Get-AzureStorageBlobContent -Container $containerName -Blob example/output/shortestpaths/part-m-00002 -Context $storageContext -Force
    ```

    <span data-ttu-id="0aff0-171">Detta skapar den **shortestpaths-exempel/utdata** katalogstruktur i aktuell katalog på din arbetsstation och hämta två utdatafilerna till den platsen.</span><span class="sxs-lookup"><span data-stu-id="0aff0-171">This will create the **example/output/shortestpaths** directory structure in the current directory on your workstation, and download the two output files to that location.</span></span>

    <span data-ttu-id="0aff0-172">Använd den **Cat** för att visa innehållet i filerna:</span><span class="sxs-lookup"><span data-stu-id="0aff0-172">Use the **Cat** cmdlet to display the contents of the files:</span></span>

        Cat example/output/shortestpaths/part*

    <span data-ttu-id="0aff0-173">Resultatet bör likna följande:</span><span class="sxs-lookup"><span data-stu-id="0aff0-173">The output should appear similar to the following:</span></span>

        0    1.0
        4    5.0
        2    2.0
        1    0.0
        3    1.0

    <span data-ttu-id="0aff0-174">SimpleShortestPathComputation exempel är hårddisken kodat börja med objekt-ID 1 och hitta den kortaste sökvägen till andra objekt.</span><span class="sxs-lookup"><span data-stu-id="0aff0-174">The SimpleShortestPathComputation example is hard coded to start with object ID 1 and find the shortest path to other objects.</span></span> <span data-ttu-id="0aff0-175">Så att utdata ska läsas som `destination_id distance`, där avstånd är värdet (eller vikt) av kanter förflyttat sig mellan objekt-ID 1 och mål-ID.</span><span class="sxs-lookup"><span data-stu-id="0aff0-175">So the output should be read as `destination_id distance`, where distance is the value (or weight) of the edges traveled between object ID 1 and the target ID.</span></span>

    <span data-ttu-id="0aff0-176">Visualisera detta kan du kontrollera resultatet av reser kortaste sökvägar mellan ID 1 och alla andra objekt.</span><span class="sxs-lookup"><span data-stu-id="0aff0-176">Visualizing this, you can verify the results by traveling the shortest paths between ID 1 and all other objects.</span></span> <span data-ttu-id="0aff0-177">Observera att den kortaste sökvägen mellan ID 1 och 4-ID är 5.</span><span class="sxs-lookup"><span data-stu-id="0aff0-177">Note that the shortest path between ID 1 and ID 4 is 5.</span></span> <span data-ttu-id="0aff0-178">Detta är det totala avståndet mellan <span style="color:orange">ID 1 och 3</span>, och sedan <span style="color:red">ID 3 och 4</span>.</span><span class="sxs-lookup"><span data-stu-id="0aff0-178">This is the total distance between <span style="color:orange">ID 1 and 3</span>, and then <span style="color:red">ID 3 and 4</span>.</span></span>

    ![Ritning av objekt som cirklar med kortast sökvägar mellan](./media/hdinsight-hadoop-giraph-install/giraph-graph-out.png)

## <a name="install-giraph-using-aure-powershell"></a><span data-ttu-id="0aff0-180">Installera Giraph med Aure PowerShell</span><span class="sxs-lookup"><span data-stu-id="0aff0-180">Install Giraph using Aure PowerShell</span></span>
<span data-ttu-id="0aff0-181">Se [anpassa HDInsight-kluster med skriptåtgärder](hdinsight-hadoop-customize-cluster.md#call-scripts-using-azure-powershell).</span><span class="sxs-lookup"><span data-stu-id="0aff0-181">See [Customize HDInsight clusters using Script Action](hdinsight-hadoop-customize-cluster.md#call-scripts-using-azure-powershell).</span></span>  <span data-ttu-id="0aff0-182">Exemplet visar hur du installerar Spark med hjälp av Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="0aff0-182">The sample demonstrates how to install Spark using Azure PowerShell.</span></span> <span data-ttu-id="0aff0-183">Du måste anpassa skript om du vill använda [https://hdiconfigactions.blob.core.windows.net/giraphconfigactionv01/giraph-installer-v01.ps1](https://hdiconfigactions.blob.core.windows.net/giraphconfigactionv01/giraph-installer-v01.ps1).</span><span class="sxs-lookup"><span data-stu-id="0aff0-183">You need to customize the script to use [https://hdiconfigactions.blob.core.windows.net/giraphconfigactionv01/giraph-installer-v01.ps1](https://hdiconfigactions.blob.core.windows.net/giraphconfigactionv01/giraph-installer-v01.ps1).</span></span>

## <a name="install-giraph-using-net-sdk"></a><span data-ttu-id="0aff0-184">Installera Giraph med .NET SDK</span><span class="sxs-lookup"><span data-stu-id="0aff0-184">Install Giraph using .NET SDK</span></span>
<span data-ttu-id="0aff0-185">Se [anpassa HDInsight-kluster med skriptåtgärder](hdinsight-hadoop-customize-cluster.md#call-scripts-using-azure-powershell).</span><span class="sxs-lookup"><span data-stu-id="0aff0-185">See [Customize HDInsight clusters using Script Action](hdinsight-hadoop-customize-cluster.md#call-scripts-using-azure-powershell).</span></span> <span data-ttu-id="0aff0-186">Exemplet visar hur du installerar Spark med .NET SDK.</span><span class="sxs-lookup"><span data-stu-id="0aff0-186">The sample demonstrates how to install Spark using the .NET SDK.</span></span> <span data-ttu-id="0aff0-187">Du måste anpassa skript om du vill använda [https://hdiconfigactions.blob.core.windows.net/giraphconfigactionv01/giraph-installer-v01.ps1](https://hdiconfigactions.blob.core.windows.net/giraphconfigactionv01/giraph-installer-v01.ps1).</span><span class="sxs-lookup"><span data-stu-id="0aff0-187">You need to customize the script to use [https://hdiconfigactions.blob.core.windows.net/giraphconfigactionv01/giraph-installer-v01.ps1](https://hdiconfigactions.blob.core.windows.net/giraphconfigactionv01/giraph-installer-v01.ps1).</span></span>

## <a name="see-also"></a><span data-ttu-id="0aff0-188">Se även</span><span class="sxs-lookup"><span data-stu-id="0aff0-188">See also</span></span>
* [<span data-ttu-id="0aff0-189">Installera Giraph på HDInsight Hadoop-kluster (Linux)</span><span class="sxs-lookup"><span data-stu-id="0aff0-189">Install Giraph on HDInsight Hadoop clusters (Linux)</span></span>](hdinsight-hadoop-giraph-install-linux.md)
* <span data-ttu-id="0aff0-190">[Skapa Hadoop-kluster i HDInsight](hdinsight-provision-clusters.md): allmän information om hur du skapar HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="0aff0-190">[Create Hadoop clusters in HDInsight](hdinsight-provision-clusters.md): general information on creating HDInsight clusters.</span></span>
* <span data-ttu-id="0aff0-191">[Anpassa HDInsight-kluster med skriptåtgärder][hdinsight-cluster-customize]: allmän information om hur du anpassar HDInsight-kluster med skriptåtgärder.</span><span class="sxs-lookup"><span data-stu-id="0aff0-191">[Customize HDInsight cluster using Script Action][hdinsight-cluster-customize]: general information on customizing HDInsight clusters using Script Action.</span></span>
* <span data-ttu-id="0aff0-192">[Utveckla skriptåtgärd skript för HDInsight](hdinsight-hadoop-script-actions.md).</span><span class="sxs-lookup"><span data-stu-id="0aff0-192">[Develop Script Action scripts for HDInsight](hdinsight-hadoop-script-actions.md).</span></span>
* <span data-ttu-id="0aff0-193">[Installera och använda Spark på HDInsight-kluster][hdinsight-install-spark]: skriptåtgärder exempel om hur du installerar Spark.</span><span class="sxs-lookup"><span data-stu-id="0aff0-193">[Install and use Spark on HDInsight clusters][hdinsight-install-spark]: Script Action sample about installing Spark.</span></span>
* <span data-ttu-id="0aff0-194">[Installera R på HDInsight-kluster][hdinsight-install-r]: skriptåtgärder exempel om hur du installerar R.</span><span class="sxs-lookup"><span data-stu-id="0aff0-194">[Install R on HDInsight clusters][hdinsight-install-r]: Script Action sample about installing R.</span></span>
* <span data-ttu-id="0aff0-195">[Installera Solr på HDInsight-kluster](hdinsight-hadoop-solr-install.md): skriptåtgärder exempel om hur du installerar Solr.</span><span class="sxs-lookup"><span data-stu-id="0aff0-195">[Install Solr on HDInsight clusters](hdinsight-hadoop-solr-install.md): Script Action sample about installing Solr.</span></span>

[tools]: https://github.com/Blackmist/hdinsight-tools
[aps]: http://azure.microsoft.com/documentation/articles/install-configure-powershell/

[powershell-install]: /powershell/azureps-cmdlets-docs
[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-install-r]: hdinsight-hadoop-r-scripts.md
[hdinsight-install-spark]: hdinsight-hadoop-spark-install.md
[hdinsight-cluster-customize]: hdinsight-hadoop-customize-cluster.md
