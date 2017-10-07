---
title: "aaaInstall och använda Giraph på Hadoop-kluster i HDInsight - Azure | Microsoft Docs"
description: "Lär dig hur toocustomize HDInsight-kluster med Giraph och hur toouse Giraph."
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
ms.openlocfilehash: bd473faca9d3c87c29d7566a18fc94211c50f059
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="install-and-use-giraph-on-windows-based-hdinsight-clusters"></a><span data-ttu-id="2a115-103">Installera och använda Giraph på Windows-baserade HDInsight-kluster</span><span class="sxs-lookup"><span data-stu-id="2a115-103">Install and use Giraph on Windows-based HDInsight clusters</span></span>

<span data-ttu-id="2a115-104">Lär dig hur toocustomize Windows-baserade HDInsight-kluster med Giraph med skriptåtgärder och hur toouse Giraph tooprocess storskaliga diagram.</span><span class="sxs-lookup"><span data-stu-id="2a115-104">Learn how toocustomize Windows based HDInsight cluster with Giraph using Script Action, and how toouse Giraph tooprocess large-scale graphs.</span></span> <span data-ttu-id="2a115-105">Information om hur du använder Giraph med ett Linux-baserade kluster finns i [installera Giraph på HDInsight Hadoop-kluster (Linux)](hdinsight-hadoop-giraph-install-linux.md).</span><span class="sxs-lookup"><span data-stu-id="2a115-105">For information on using Giraph with a Linux-based cluster, see [Install Giraph on HDInsight Hadoop clusters (Linux)](hdinsight-hadoop-giraph-install-linux.md).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="2a115-106">hello stegen i det här dokumentet som endast fungerar med Windows-baserade HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="2a115-106">hello steps in this document only work with Windows-based HDInsight clusters.</span></span> <span data-ttu-id="2a115-107">HDInsight är endast tillgängligt i Windows för versioner som är lägre än HDInsight 3.4.</span><span class="sxs-lookup"><span data-stu-id="2a115-107">HDInsight is only available on Windows for versions lower than HDInsight 3.4.</span></span> <span data-ttu-id="2a115-108">Linux är hello endast operativsystem på HDInsight version 3.4 eller senare.</span><span class="sxs-lookup"><span data-stu-id="2a115-108">Linux is hello only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="2a115-109">Mer information finns i [HDInsight-avveckling på Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="2a115-109">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span> <span data-ttu-id="2a115-110">Mer information om hur tooinstall Giraph på en Linux-baserade HDInsight-kluster finns [installera Giraph på HDInsight Hadoop-kluster (Linux)](hdinsight-hadoop-giraph-install-linux.md).</span><span class="sxs-lookup"><span data-stu-id="2a115-110">For information on how tooinstall Giraph on a Linux-based HDInsight cluster, see [Install Giraph on HDInsight Hadoop clusters (Linux)](hdinsight-hadoop-giraph-install-linux.md).</span></span>


<span data-ttu-id="2a115-111">Du kan installera Giraph på någon typ av kluster (Hadoop, Storm, HBase, Spark) på Azure HDInsight med hjälp av *skriptåtgärd*.</span><span class="sxs-lookup"><span data-stu-id="2a115-111">You can install Giraph on any type of cluster (Hadoop, Storm, HBase, Spark) on Azure HDInsight by using *Script Action*.</span></span> <span data-ttu-id="2a115-112">Ett exempel skriptet tooinstall Giraph på ett HDInsight-kluster är tillgänglig från en skrivskyddad Azure storage blob [https://hdiconfigactions.blob.core.windows.net/giraphconfigactionv01/giraph-installer-v01.ps1](https://hdiconfigactions.blob.core.windows.net/giraphconfigactionv01/giraph-installer-v01.ps1).</span><span class="sxs-lookup"><span data-stu-id="2a115-112">A sample script tooinstall Giraph on an HDInsight cluster is available from a read-only Azure storage blob at [https://hdiconfigactions.blob.core.windows.net/giraphconfigactionv01/giraph-installer-v01.ps1](https://hdiconfigactions.blob.core.windows.net/giraphconfigactionv01/giraph-installer-v01.ps1).</span></span> <span data-ttu-id="2a115-113">hello exempelskript fungerar bara med HDInsight-kluster av version 3.1.</span><span class="sxs-lookup"><span data-stu-id="2a115-113">hello sample script works only with HDInsight cluster version 3.1.</span></span> <span data-ttu-id="2a115-114">Mer information om HDInsight-kluster-versioner finns [HDInsight-kluster-versioner](hdinsight-component-versioning.md).</span><span class="sxs-lookup"><span data-stu-id="2a115-114">For more information on HDInsight cluster versions, see [HDInsight cluster versions](hdinsight-component-versioning.md).</span></span>

<span data-ttu-id="2a115-115">**Relaterade artiklar**</span><span class="sxs-lookup"><span data-stu-id="2a115-115">**Related articles**</span></span>

* [<span data-ttu-id="2a115-116">Installera Giraph på HDInsight Hadoop-kluster (Linux)</span><span class="sxs-lookup"><span data-stu-id="2a115-116">Install Giraph on HDInsight Hadoop clusters (Linux)</span></span>](hdinsight-hadoop-giraph-install-linux.md)
* <span data-ttu-id="2a115-117">[Skapa Hadoop-kluster i HDInsight](hdinsight-provision-clusters.md): allmän information om hur du skapar HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="2a115-117">[Create Hadoop clusters in HDInsight](hdinsight-provision-clusters.md): general information on creating HDInsight clusters.</span></span>
* <span data-ttu-id="2a115-118">[Anpassa HDInsight-kluster med skriptåtgärder][hdinsight-cluster-customize]: allmän information om hur du anpassar HDInsight-kluster med skriptåtgärder.</span><span class="sxs-lookup"><span data-stu-id="2a115-118">[Customize HDInsight cluster using Script Action][hdinsight-cluster-customize]: general information on customizing HDInsight clusters using Script Action.</span></span>
* <span data-ttu-id="2a115-119">[Utveckla skriptåtgärd skript för HDInsight](hdinsight-hadoop-script-actions.md).</span><span class="sxs-lookup"><span data-stu-id="2a115-119">[Develop Script Action scripts for HDInsight](hdinsight-hadoop-script-actions.md).</span></span>

## <a name="what-is-giraph"></a><span data-ttu-id="2a115-120">Vad är Giraph?</span><span class="sxs-lookup"><span data-stu-id="2a115-120">What is Giraph?</span></span>
<span data-ttu-id="2a115-121"><a href="http://giraph.apache.org/" target="_blank">Apache Giraph</a> kan du tooperform diagrammet bearbetning med hjälp av Hadoop och kan användas med Azure HDInsight.</span><span class="sxs-lookup"><span data-stu-id="2a115-121"><a href="http://giraph.apache.org/" target="_blank">Apache Giraph</a> allows you tooperform graph processing by using Hadoop, and can be used with Azure HDInsight.</span></span> <span data-ttu-id="2a115-122">Diagram modellen relationer mellan objekt, till exempel hello anslutningar mellan routrar i ett större nätverk som hello Internet, eller relationer mellan personer på sociala nätverk (ibland hänvisade tooas sociala diagram).</span><span class="sxs-lookup"><span data-stu-id="2a115-122">Graphs model relationships between objects, such as hello connections between routers on a large network like hello Internet, or relationships between people on social networks (sometimes referred tooas a social graph).</span></span> <span data-ttu-id="2a115-123">Diagrammet bearbetning kan du tooreason om hello relationer mellan objekt i ett diagram som:</span><span class="sxs-lookup"><span data-stu-id="2a115-123">Graph processing allows you tooreason about hello relationships between objects in a graph, such as:</span></span>

* <span data-ttu-id="2a115-124">Identifiera potentiella vänner baserat på din aktuella relationer.</span><span class="sxs-lookup"><span data-stu-id="2a115-124">Identifying potential friends based on your current relationships.</span></span>
* <span data-ttu-id="2a115-125">Identifiera hello kortaste vägen mellan två datorer i ett nätverk.</span><span class="sxs-lookup"><span data-stu-id="2a115-125">Identifying hello shortest route between two computers in a network.</span></span>
* <span data-ttu-id="2a115-126">Beräkning av hello sidan rangordningen för webbsidor.</span><span class="sxs-lookup"><span data-stu-id="2a115-126">Calculating hello page rank of webpages.</span></span>

## <a name="install-giraph-using-portal"></a><span data-ttu-id="2a115-127">Installera Giraph med hjälp av portalen</span><span class="sxs-lookup"><span data-stu-id="2a115-127">Install Giraph using portal</span></span>
1. <span data-ttu-id="2a115-128">Skapa ett kluster med hjälp av hello **skapa anpassade** alternativ, enligt beskrivningen i [skapa Hadoop-kluster i HDInsight](hdinsight-provision-clusters.md).</span><span class="sxs-lookup"><span data-stu-id="2a115-128">Start creating a cluster by using hello **CUSTOM CREATE** option, as described at [Create Hadoop clusters in HDInsight](hdinsight-provision-clusters.md).</span></span>
2. <span data-ttu-id="2a115-129">På hello **skriptåtgärder** sidan hello guiden klickar du på **lägga till skriptåtgärd** tooprovide information om hello skriptåtgärd enligt nedan:</span><span class="sxs-lookup"><span data-stu-id="2a115-129">On hello **Script Actions** page of hello wizard, click **add script action** tooprovide details about hello script action, as shown below:</span></span>

    <span data-ttu-id="2a115-130">![Använd skriptåtgärder toocustomize ett kluster](./media/hdinsight-hadoop-giraph-install/hdi-script-action-giraph.png "Använd skriptåtgärder toocustomize ett kluster")</span><span class="sxs-lookup"><span data-stu-id="2a115-130">![Use Script Action toocustomize a cluster](./media/hdinsight-hadoop-giraph-install/hdi-script-action-giraph.png "Use Script Action toocustomize a cluster")</span></span>

    <table border='1'>
        <tr><th><span data-ttu-id="2a115-131">Egenskap</span><span class="sxs-lookup"><span data-stu-id="2a115-131">Property</span></span></th><th><span data-ttu-id="2a115-132">Värde</span><span class="sxs-lookup"><span data-stu-id="2a115-132">Value</span></span></th></tr>
        <tr><td><span data-ttu-id="2a115-133">Namn</span><span class="sxs-lookup"><span data-stu-id="2a115-133">Name</span></span></td>
            <td><span data-ttu-id="2a115-134">Ange ett namn för hello skriptåtgärder.</span><span class="sxs-lookup"><span data-stu-id="2a115-134">Specify a name for hello script action.</span></span> <span data-ttu-id="2a115-135">Till exempel <b>installera Giraph</b>.</span><span class="sxs-lookup"><span data-stu-id="2a115-135">For example, <b>Install Giraph</b>.</span></span></td></tr>
        <tr><td><span data-ttu-id="2a115-136">Skript-URI</span><span class="sxs-lookup"><span data-stu-id="2a115-136">Script URI</span></span></td>
            <td><span data-ttu-id="2a115-137">Ange hello identifierare URI (Uniform Resource) toohello skript som är anropade toocustomize hello-kluster.</span><span class="sxs-lookup"><span data-stu-id="2a115-137">Specify hello Uniform Resource Identifier (URI) toohello script that is invoked toocustomize hello cluster.</span></span> <span data-ttu-id="2a115-138">Till exempel <i>https://hdiconfigactions.blob.core.windows.net/giraphconfigactionv01/giraph-installer-v01.ps1</i></span><span class="sxs-lookup"><span data-stu-id="2a115-138">For example, <i>https://hdiconfigactions.blob.core.windows.net/giraphconfigactionv01/giraph-installer-v01.ps1</i></span></span></td></tr>
        <tr><td><span data-ttu-id="2a115-139">Nodtyp</span><span class="sxs-lookup"><span data-stu-id="2a115-139">Node Type</span></span></td>
            <td><span data-ttu-id="2a115-140">Ange hello noder som hello anpassning skript körs.</span><span class="sxs-lookup"><span data-stu-id="2a115-140">Specify hello nodes on which hello customization script is run.</span></span> <span data-ttu-id="2a115-141">Du kan välja <b>alla noder</b>, <b>huvudnoder endast</b>, eller <b>arbetarnoder endast</b>.</span><span class="sxs-lookup"><span data-stu-id="2a115-141">You can choose <b>All nodes</b>, <b>Head nodes only</b>, or <b>Worker nodes only</b>.</span></span>
        <tr><td><span data-ttu-id="2a115-142">Parametrar</span><span class="sxs-lookup"><span data-stu-id="2a115-142">Parameters</span></span></td>
            <td><span data-ttu-id="2a115-143">Ange hello parametrar, om det krävs av hello skript.</span><span class="sxs-lookup"><span data-stu-id="2a115-143">Specify hello parameters, if required by hello script.</span></span> <span data-ttu-id="2a115-144">hello skriptet tooinstall Giraph kräver inte parametrar, så du kan lämna den tom.</span><span class="sxs-lookup"><span data-stu-id="2a115-144">hello script tooinstall Giraph does not require any parameters, so you can leave this blank.</span></span></td></tr>
    </table>

    <span data-ttu-id="2a115-145">Du kan lägga till fler än en skript åtgärd tooinstall flera komponenter på hello klustret.</span><span class="sxs-lookup"><span data-stu-id="2a115-145">You can add more than one script action tooinstall multiple components on hello cluster.</span></span> <span data-ttu-id="2a115-146">När du har lagt till hello skript klickar du på hello markering toostart skapar hello kluster.</span><span class="sxs-lookup"><span data-stu-id="2a115-146">After you have added hello scripts, click hello checkmark toostart creating hello cluster.</span></span>

## <a name="use-giraph"></a><span data-ttu-id="2a115-147">Använd Giraph</span><span class="sxs-lookup"><span data-stu-id="2a115-147">Use Giraph</span></span>
<span data-ttu-id="2a115-148">Vi använder hello SimpleShortestPathsComputation exempel toodemonstrate hello basic <a href = "http://people.apache.org/~edwardyoon/documents/pregel.pdf">Pregel</a> implementering för att hitta hello shortest path mellan objekt i ett diagram.</span><span class="sxs-lookup"><span data-stu-id="2a115-148">We use hello SimpleShortestPathsComputation example toodemonstrate hello basic <a href = "http://people.apache.org/~edwardyoon/documents/pregel.pdf">Pregel</a> implementation for finding hello shortest path between objects in a graph.</span></span> <span data-ttu-id="2a115-149">Använd följande steg tooupload hello exempel data och hello exempel jar, köra ett jobb med hjälp av hello SimpleShortestPathsComputation exempel och visa hello resultat hello.</span><span class="sxs-lookup"><span data-stu-id="2a115-149">Use hello following steps tooupload hello sample data and hello sample jar, run a job by using hello SimpleShortestPathsComputation example, and then view hello results.</span></span>

1. <span data-ttu-id="2a115-150">Överför en exempel data filen tooAzure Blob storage.</span><span class="sxs-lookup"><span data-stu-id="2a115-150">Upload a sample data file tooAzure Blob storage.</span></span> <span data-ttu-id="2a115-151">Skapa en ny fil med namnet på din lokala arbetsstation **tiny_graph.txt**.</span><span class="sxs-lookup"><span data-stu-id="2a115-151">On your local workstation, create a new file named **tiny_graph.txt**.</span></span> <span data-ttu-id="2a115-152">Den ska innehålla hello följande rader:</span><span class="sxs-lookup"><span data-stu-id="2a115-152">It should contain hello following lines:</span></span>

        [0,0,[[1,1],[3,3]]]
        [1,0,[[0,1],[2,2],[3,1]]]
        [2,0,[[1,2],[4,4]]]
        [3,0,[[0,3],[1,1],[4,4]]]
        [4,0,[[3,4],[2,4]]]

    <span data-ttu-id="2a115-153">Överför hello tiny_graph.txt toohello primära fillagring för HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="2a115-153">Upload hello tiny_graph.txt file toohello primary storage for your HDInsight cluster.</span></span> <span data-ttu-id="2a115-154">Anvisningar för hur tooupload data finns i [överföra data för Hadoop-jobb i HDInsight](hdinsight-upload-data.md).</span><span class="sxs-lookup"><span data-stu-id="2a115-154">For instructions on how tooupload data, see [Upload data for Hadoop jobs in HDInsight](hdinsight-upload-data.md).</span></span>

    <span data-ttu-id="2a115-155">Informationen beskriver en relation mellan objekt i en riktat diagram hello formatet [källa\_id, källa\_värde, [[målet\_id], [edge\_värde],...]]. Varje rad representerar en relation mellan en **källa\_id** objekt och en eller flera **dest\_id** objekt.</span><span class="sxs-lookup"><span data-stu-id="2a115-155">This data describes a relationship between objects in a directed graph, by using hello format [source\_id, source\_value,[[dest\_id], [edge\_value],...]]. Each line represents a relationship between a **source\_id** object and one or more **dest\_id** objects.</span></span> <span data-ttu-id="2a115-156">Hej **kant\_värdet** (eller vikt) kan betraktas som hello styrkan eller avståndet mellan hello anslutning mellan **source_id** och **dest\_id**.</span><span class="sxs-lookup"><span data-stu-id="2a115-156">hello **edge\_value** (or weight) can be thought of as hello strength or distance of hello connection between **source_id** and **dest\_id**.</span></span>

    <span data-ttu-id="2a115-157">Dras ut, och använder hello värde (eller vikt) som hello avståndet mellan objekt, hello ovanför data kan se ut så här:</span><span class="sxs-lookup"><span data-stu-id="2a115-157">Drawn out, and using hello value (or weight) as hello distance between objects, hello above data might look like this:</span></span>

    ![tiny_graph.txt ritas som cirklar med rader med olika avståndet mellan](./media/hdinsight-hadoop-giraph-install/giraph-graph.png)
2. <span data-ttu-id="2a115-159">Kör hello SimpleShortestPathsComputation exempel.</span><span class="sxs-lookup"><span data-stu-id="2a115-159">Run hello SimpleShortestPathsComputation example.</span></span> <span data-ttu-id="2a115-160">Använd hello följande Azure PowerShell-cmdlets toorun hello exempel med hjälp av hello tiny_graph.txt fil som indata.</span><span class="sxs-lookup"><span data-stu-id="2a115-160">Use hello following Azure PowerShell cmdlets toorun hello example by using hello tiny_graph.txt file as input.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="2a115-161">Azure PowerShell-stöd för hantering av HDInsight-resurser med hjälp av Azure Service Manager **är föråldrat** och togs bort den 1 januari 2017.</span><span class="sxs-lookup"><span data-stu-id="2a115-161">Azure PowerShell support for managing HDInsight resources using Azure Service Manager is **deprecated**, and was removed on January 1, 2017.</span></span> <span data-ttu-id="2a115-162">hello stegen i det här dokumentet används hello nya HDInsight-cmdletarna som fungerar med Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="2a115-162">hello steps in this document use hello new HDInsight cmdlets that work with Azure Resource Manager.</span></span>
    >
    > <span data-ttu-id="2a115-163">Följ stegen hello i [installera och konfigurera Azure PowerShell](/powershell/azureps-cmdlets-docs) tooinstall hello senaste versionen av Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="2a115-163">Please follow hello steps in [Install and configure Azure PowerShell](/powershell/azureps-cmdlets-docs) tooinstall hello latest version of Azure PowerShell.</span></span> <span data-ttu-id="2a115-164">Om du har skript som behöver toobe ändras toouse hello nya cmdletarna som fungerar med Azure Resource Manager kan se [migrera tooAzure Resource Manager-baserade development tools för HDInsight-kluster](hdinsight-hadoop-development-using-azure-resource-manager.md) för mer information.</span><span class="sxs-lookup"><span data-stu-id="2a115-164">If you have scripts that need toobe modified toouse hello new cmdlets that work with Azure Resource Manager, see [Migrating tooAzure Resource Manager-based development tools for HDInsight clusters](hdinsight-hadoop-development-using-azure-resource-manager.md) for more information.</span></span>

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
    # Create hello definition
    $jobDefinition = New-AzureHDInsightMapReduceJobDefinition
        -JarFile $jarFile
        -ClassName "org.apache.giraph.GiraphRunner"
        -Arguments $jobArguments

    # Run hello job, write output toohello Azure PowerShell window
    $job = Start-AzureHDInsightJob -Cluster $clusterName -JobDefinition $jobDefinition
    Write-Host "Wait for hello job toocomplete ..." -ForegroundColor Green
    Wait-AzureHDInsightJob -Job $job
    Write-Host "STDERR"
    Get-AzureHDInsightJobOutput -Cluster $clusterName -JobId $job.JobId -StandardError
    Write-Host "Display hello standard output ..." -ForegroundColor Green
    Get-AzureHDInsightJobOutput -Cluster $clusterName -JobId $job.JobId -StandardOutput
    ```

    <span data-ttu-id="2a115-165">I ovanstående exempel hello, ersätter **klusternamn** med hello namnet på ditt HDInsight-kluster som har Giraph installerad.</span><span class="sxs-lookup"><span data-stu-id="2a115-165">In hello above example, replace **clustername** with hello name of your HDInsight cluster that has Giraph installed.</span></span>
3. <span data-ttu-id="2a115-166">Visa hello resultat.</span><span class="sxs-lookup"><span data-stu-id="2a115-166">View hello results.</span></span> <span data-ttu-id="2a115-167">När hello jobbet har slutförts, hello resultat lagras i två utdatafilerna i hello **wasb: / / / exempel/out/shotestpaths** mapp.</span><span class="sxs-lookup"><span data-stu-id="2a115-167">Once hello job has finished, hello results will be stored in two output files in hello **wasb:///example/out/shotestpaths** folder.</span></span> <span data-ttu-id="2a115-168">hello filerna kallas **del-m-00001** och **del-m-00002**.</span><span class="sxs-lookup"><span data-stu-id="2a115-168">hello files are called **part-m-00001** and **part-m-00002**.</span></span> <span data-ttu-id="2a115-169">Utför hello följande steg toodownload och visa hello utdata:</span><span class="sxs-lookup"><span data-stu-id="2a115-169">Perform hello following steps toodownload and view hello output:</span></span>

    ```powershell
    $subscriptionName = "<SubscriptionName>"       # Azure subscription name
    $storageAccountName = "<StorageAccountName>"   # Azure Storage account name
    $containerName = "<ContainerName>"             # Blob storage container name

    # Select hello current subscription
    Select-AzureSubscription $subscriptionName

    # Create hello Storage account context object
    $storageAccountKey = Get-AzureStorageKey $storageAccountName | %{ $_.Primary }
    $storageContext = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey

    # Download hello job output toohello workstation
    Get-AzureStorageBlobContent -Container $containerName -Blob example/output/shortestpaths/part-m-00001 -Context $storageContext -Force
    Get-AzureStorageBlobContent -Container $containerName -Blob example/output/shortestpaths/part-m-00002 -Context $storageContext -Force
    ```

    <span data-ttu-id="2a115-170">Detta skapar hello **shortestpaths-exempel/utdata** katalogstruktur i hello aktuell katalog på din arbetsstation och hämta hello två filer toothat platsen.</span><span class="sxs-lookup"><span data-stu-id="2a115-170">This will create hello **example/output/shortestpaths** directory structure in hello current directory on your workstation, and download hello two output files toothat location.</span></span>

    <span data-ttu-id="2a115-171">Använd hello **Cat** cmdlet toodisplay hello innehållet i hello filer:</span><span class="sxs-lookup"><span data-stu-id="2a115-171">Use hello **Cat** cmdlet toodisplay hello contents of hello files:</span></span>

        Cat example/output/shortestpaths/part*

    <span data-ttu-id="2a115-172">hello utdata ska se ut ungefär toohello följande:</span><span class="sxs-lookup"><span data-stu-id="2a115-172">hello output should appear similar toohello following:</span></span>

        0    1.0
        4    5.0
        2    2.0
        1    0.0
        3    1.0

    <span data-ttu-id="2a115-173">Hej SimpleShortestPathComputation exempel är hårdkodad toostart med objekt-ID 1 och hitta hello shortest path tooother objekt.</span><span class="sxs-lookup"><span data-stu-id="2a115-173">hello SimpleShortestPathComputation example is hard coded toostart with object ID 1 and find hello shortest path tooother objects.</span></span> <span data-ttu-id="2a115-174">Så hello utdata ska läsas som `destination_id distance`, där avstånd är hello värde (eller vikt) av hello kanter obligatoriska mellan objekt-ID 1 och hello mål-ID.</span><span class="sxs-lookup"><span data-stu-id="2a115-174">So hello output should be read as `destination_id distance`, where distance is hello value (or weight) of hello edges traveled between object ID 1 and hello target ID.</span></span>

    <span data-ttu-id="2a115-175">Du kan visualisera detta för att verifiera hello resultaten av reser hello kortaste sökvägar mellan ID 1 och alla andra objekt.</span><span class="sxs-lookup"><span data-stu-id="2a115-175">Visualizing this, you can verify hello results by traveling hello shortest paths between ID 1 and all other objects.</span></span> <span data-ttu-id="2a115-176">Observera att hello shortest path mellan ID 1 och 4-ID är 5.</span><span class="sxs-lookup"><span data-stu-id="2a115-176">Note that hello shortest path between ID 1 and ID 4 is 5.</span></span> <span data-ttu-id="2a115-177">Detta är hello totala avståndet mellan <span style="color:orange">ID 1 och 3</span>, och sedan <span style="color:red">ID 3 och 4</span>.</span><span class="sxs-lookup"><span data-stu-id="2a115-177">This is hello total distance between <span style="color:orange">ID 1 and 3</span>, and then <span style="color:red">ID 3 and 4</span>.</span></span>

    ![Ritning av objekt som cirklar med kortast sökvägar mellan](./media/hdinsight-hadoop-giraph-install/giraph-graph-out.png)

## <a name="install-giraph-using-aure-powershell"></a><span data-ttu-id="2a115-179">Installera Giraph med Aure PowerShell</span><span class="sxs-lookup"><span data-stu-id="2a115-179">Install Giraph using Aure PowerShell</span></span>
<span data-ttu-id="2a115-180">Se [anpassa HDInsight-kluster med skriptåtgärder](hdinsight-hadoop-customize-cluster.md#call-scripts-using-azure-powershell).</span><span class="sxs-lookup"><span data-stu-id="2a115-180">See [Customize HDInsight clusters using Script Action](hdinsight-hadoop-customize-cluster.md#call-scripts-using-azure-powershell).</span></span>  <span data-ttu-id="2a115-181">hello exemplet visar hur tooinstall Spark med hjälp av Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="2a115-181">hello sample demonstrates how tooinstall Spark using Azure PowerShell.</span></span> <span data-ttu-id="2a115-182">Du behöver toocustomize hello skriptet toouse [https://hdiconfigactions.blob.core.windows.net/giraphconfigactionv01/giraph-installer-v01.ps1](https://hdiconfigactions.blob.core.windows.net/giraphconfigactionv01/giraph-installer-v01.ps1).</span><span class="sxs-lookup"><span data-stu-id="2a115-182">You need toocustomize hello script toouse [https://hdiconfigactions.blob.core.windows.net/giraphconfigactionv01/giraph-installer-v01.ps1](https://hdiconfigactions.blob.core.windows.net/giraphconfigactionv01/giraph-installer-v01.ps1).</span></span>

## <a name="install-giraph-using-net-sdk"></a><span data-ttu-id="2a115-183">Installera Giraph med .NET SDK</span><span class="sxs-lookup"><span data-stu-id="2a115-183">Install Giraph using .NET SDK</span></span>
<span data-ttu-id="2a115-184">Se [anpassa HDInsight-kluster med skriptåtgärder](hdinsight-hadoop-customize-cluster.md#call-scripts-using-azure-powershell).</span><span class="sxs-lookup"><span data-stu-id="2a115-184">See [Customize HDInsight clusters using Script Action](hdinsight-hadoop-customize-cluster.md#call-scripts-using-azure-powershell).</span></span> <span data-ttu-id="2a115-185">hello exemplet visar hur tooinstall Spark med hello .NET SDK.</span><span class="sxs-lookup"><span data-stu-id="2a115-185">hello sample demonstrates how tooinstall Spark using hello .NET SDK.</span></span> <span data-ttu-id="2a115-186">Du behöver toocustomize hello skriptet toouse [https://hdiconfigactions.blob.core.windows.net/giraphconfigactionv01/giraph-installer-v01.ps1](https://hdiconfigactions.blob.core.windows.net/giraphconfigactionv01/giraph-installer-v01.ps1).</span><span class="sxs-lookup"><span data-stu-id="2a115-186">You need toocustomize hello script toouse [https://hdiconfigactions.blob.core.windows.net/giraphconfigactionv01/giraph-installer-v01.ps1](https://hdiconfigactions.blob.core.windows.net/giraphconfigactionv01/giraph-installer-v01.ps1).</span></span>

## <a name="see-also"></a><span data-ttu-id="2a115-187">Se även</span><span class="sxs-lookup"><span data-stu-id="2a115-187">See also</span></span>
* [<span data-ttu-id="2a115-188">Installera Giraph på HDInsight Hadoop-kluster (Linux)</span><span class="sxs-lookup"><span data-stu-id="2a115-188">Install Giraph on HDInsight Hadoop clusters (Linux)</span></span>](hdinsight-hadoop-giraph-install-linux.md)
* <span data-ttu-id="2a115-189">[Skapa Hadoop-kluster i HDInsight](hdinsight-provision-clusters.md): allmän information om hur du skapar HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="2a115-189">[Create Hadoop clusters in HDInsight](hdinsight-provision-clusters.md): general information on creating HDInsight clusters.</span></span>
* <span data-ttu-id="2a115-190">[Anpassa HDInsight-kluster med skriptåtgärder][hdinsight-cluster-customize]: allmän information om hur du anpassar HDInsight-kluster med skriptåtgärder.</span><span class="sxs-lookup"><span data-stu-id="2a115-190">[Customize HDInsight cluster using Script Action][hdinsight-cluster-customize]: general information on customizing HDInsight clusters using Script Action.</span></span>
* <span data-ttu-id="2a115-191">[Utveckla skriptåtgärd skript för HDInsight](hdinsight-hadoop-script-actions.md).</span><span class="sxs-lookup"><span data-stu-id="2a115-191">[Develop Script Action scripts for HDInsight](hdinsight-hadoop-script-actions.md).</span></span>
* <span data-ttu-id="2a115-192">[Installera och använda Spark på HDInsight-kluster][hdinsight-install-spark]: skriptåtgärder exempel om hur du installerar Spark.</span><span class="sxs-lookup"><span data-stu-id="2a115-192">[Install and use Spark on HDInsight clusters][hdinsight-install-spark]: Script Action sample about installing Spark.</span></span>
* <span data-ttu-id="2a115-193">[Installera R på HDInsight-kluster][hdinsight-install-r]: skriptåtgärder exempel om hur du installerar R.</span><span class="sxs-lookup"><span data-stu-id="2a115-193">[Install R on HDInsight clusters][hdinsight-install-r]: Script Action sample about installing R.</span></span>
* <span data-ttu-id="2a115-194">[Installera Solr på HDInsight-kluster](hdinsight-hadoop-solr-install.md): skriptåtgärder exempel om hur du installerar Solr.</span><span class="sxs-lookup"><span data-stu-id="2a115-194">[Install Solr on HDInsight clusters](hdinsight-hadoop-solr-install.md): Script Action sample about installing Solr.</span></span>

[tools]: https://github.com/Blackmist/hdinsight-tools
[aps]: http://azure.microsoft.com/documentation/articles/install-configure-powershell/

[powershell-install]: /powershell/azureps-cmdlets-docs
[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-install-r]: hdinsight-hadoop-r-scripts.md
[hdinsight-install-spark]: hdinsight-hadoop-spark-install.md
[hdinsight-cluster-customize]: hdinsight-hadoop-customize-cluster.md
