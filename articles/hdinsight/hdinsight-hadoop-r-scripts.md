---
title: "aaaUse R i HDInsight-kluster för toocustomize - Azure | Microsoft Docs"
description: "Lär dig hur tooinstall R med hjälp av skript och Använd R i HDInsight-kluster."
services: hdinsight
documentationcenter: 
tags: azure-portal
author: mumian
manager: jhubbard
editor: cgronlun
ms.assetid: be851270-afa5-4af0-a69e-2d343a4deeb7
ms.service: hdinsight
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/25/2017
ms.author: jgao
ROBOTS: NOINDEX
ms.openlocfilehash: bf5adf2e18dc43a743b29fd1567fad731b9c3ab7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="install-and-use-r-on-hdinsight-hadoop-clusters"></a><span data-ttu-id="b9b91-103">Installera och använda R på HDInsight Hadoop-kluster</span><span class="sxs-lookup"><span data-stu-id="b9b91-103">Install and use R on HDInsight Hadoop clusters</span></span>

<span data-ttu-id="b9b91-104">Lär dig hur toocustomize Windows baserade HDInsight-kluster med hjälp av skriptåtgärder R och hur toouse R på HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="b9b91-104">Learn how toocustomize Windows based HDInsight cluster with R using Script Action, and how toouse R on HDInsight clusters.</span></span> <span data-ttu-id="b9b91-105">Hej [HDInsight erbjudande](https://azure.microsoft.com/pricing/details/hdinsight/) innehåller R-Server som en del av ditt HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="b9b91-105">hello [HDInsight offering](https://azure.microsoft.com/pricing/details/hdinsight/) includes R Server as part of your HDInsight cluster.</span></span> <span data-ttu-id="b9b91-106">Detta kan R toouse MapReduce och Spark toorun distribuerade beräkningar.</span><span class="sxs-lookup"><span data-stu-id="b9b91-106">This allows R scripts toouse MapReduce and Spark toorun distributed computations.</span></span> <span data-ttu-id="b9b91-107">Mer information finns i [Komma igång med R Server på HDInsight](hdinsight-hadoop-r-server-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="b9b91-107">For more information, see [Get started using R Server on HDInsight](hdinsight-hadoop-r-server-get-started.md).</span></span> <span data-ttu-id="b9b91-108">Information om hur du använder R med ett Linux-baserade kluster finns i [installerar och använder R på HDinsight Hadoop-kluster (Linux)](hdinsight-hadoop-r-scripts-linux.md).</span><span class="sxs-lookup"><span data-stu-id="b9b91-108">For information on using R with a Linux-based cluster, see [Install and use R on HDinsight Hadoop clusters (Linux)](hdinsight-hadoop-r-scripts-linux.md).</span></span>

<span data-ttu-id="b9b91-109">Du kan installera R på någon typ av kluster (Hadoop, Storm, HBase, Spark) på Azure HDInsight med hjälp av *skriptåtgärd*.</span><span class="sxs-lookup"><span data-stu-id="b9b91-109">You can install R on any type of cluster (Hadoop, Storm, HBase, Spark) on Azure HDInsight by using *Script Action*.</span></span> <span data-ttu-id="b9b91-110">Ett exempel skriptet tooinstall R på ett HDInsight-kluster är tillgänglig från en skrivskyddad Azure storage blob [https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps1](https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps1).</span><span class="sxs-lookup"><span data-stu-id="b9b91-110">A sample script tooinstall R on an HDInsight cluster is available from a read-only Azure storage blob at [https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps1](https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps1).</span></span>

<span data-ttu-id="b9b91-111">**Relaterade artiklar**</span><span class="sxs-lookup"><span data-stu-id="b9b91-111">**Related articles**</span></span>

* [<span data-ttu-id="b9b91-112">Installera och använda R på HDinsight Hadoop-kluster (Linux)</span><span class="sxs-lookup"><span data-stu-id="b9b91-112">Install and use R on HDinsight Hadoop clusters (Linux)</span></span>](hdinsight-hadoop-r-scripts-linux.md)
* <span data-ttu-id="b9b91-113">[Skapa Hadoop-kluster i HDInsight](hdinsight-hadoop-provision-linux-clusters.md): allmän information om hur du skapar HDInsight-kluster</span><span class="sxs-lookup"><span data-stu-id="b9b91-113">[Create Hadoop clusters in HDInsight](hdinsight-hadoop-provision-linux-clusters.md): general information on creating HDInsight clusters</span></span>
* <span data-ttu-id="b9b91-114">[Anpassa HDInsight-kluster med skriptåtgärder][hdinsight-cluster-customize]: allmän information om hur du anpassar HDInsight-kluster med skriptåtgärder</span><span class="sxs-lookup"><span data-stu-id="b9b91-114">[Customize HDInsight cluster using Script Action][hdinsight-cluster-customize]: general information on customizing HDInsight clusters using Script Action</span></span>
* [<span data-ttu-id="b9b91-115">Utveckla skriptåtgärd skript för HDInsight</span><span class="sxs-lookup"><span data-stu-id="b9b91-115">Develop Script Action scripts for HDInsight</span></span>](hdinsight-hadoop-script-actions.md)

## <a name="what-is-r"></a><span data-ttu-id="b9b91-116">Vad är R?</span><span class="sxs-lookup"><span data-stu-id="b9b91-116">What is R?</span></span>
<span data-ttu-id="b9b91-117">Hej <a href="http://www.r-project.org/" target="_blank">R-projekt för statistisk databehandling</a> är en öppen källa och -miljö för statistisk databehandling.</span><span class="sxs-lookup"><span data-stu-id="b9b91-117">hello <a href="http://www.r-project.org/" target="_blank">R Project for Statistical Computing</a> is an open source language and environment for statistical computing.</span></span> <span data-ttu-id="b9b91-118">R innehåller hundratals inbyggda statistiska funktioner och sin egen programmeringsspråk som kombinerar aspekter av funktions- och objektorienterad programmering.</span><span class="sxs-lookup"><span data-stu-id="b9b91-118">R provides hundreds of build-in statistical functions and its own programming language that combines aspects of functional and object-oriented programming.</span></span> <span data-ttu-id="b9b91-119">Det ger också omfattande grafiskt funktioner.</span><span class="sxs-lookup"><span data-stu-id="b9b91-119">It also provides extensive graphical capabilities.</span></span> <span data-ttu-id="b9b91-120">R är hello prioriterade programmeringsmiljö mest professional statistiker och forskare i en mängd olika fält.</span><span class="sxs-lookup"><span data-stu-id="b9b91-120">R is hello preferred programming environment for most professional statisticians and scientists in a wide variety of fields.</span></span>

<span data-ttu-id="b9b91-121">R är kompatibelt med Azure Blob Storage (WASB) så att data som lagras det bearbetas med R på HDInsight.</span><span class="sxs-lookup"><span data-stu-id="b9b91-121">R is compatible with Azure Blob Storage (WASB) so that data that is stored there can be processed using R on HDInsight.</span></span>  

## <a name="install-r"></a><span data-ttu-id="b9b91-122">Installera R</span><span class="sxs-lookup"><span data-stu-id="b9b91-122">Install R</span></span>
<span data-ttu-id="b9b91-123">En [exempel på skript](https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps1) tooinstall R på ett HDInsight-kluster är tillgänglig från en skrivskyddad blobb i Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="b9b91-123">A [sample script](https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps1) tooinstall R on an HDInsight cluster is available from a read-only blob in Azure Storage.</span></span> <span data-ttu-id="b9b91-124">Det här avsnittet innehåller instruktioner om hur toouse hello exempelskript när du skapar hello-kluster med hjälp av hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="b9b91-124">This section provides instructions about how toouse hello sample script while creating hello cluster using hello Azure Portal.</span></span>

> [!NOTE]
> <span data-ttu-id="b9b91-125">hello exempelskript introducerades med HDInsight-kluster av version 3.1.</span><span class="sxs-lookup"><span data-stu-id="b9b91-125">hello sample script was introduced with HDInsight cluster version 3.1.</span></span> <span data-ttu-id="b9b91-126">Mer information om HDInsight-kluster-versioner finns [HDInsight-kluster-versioner](hdinsight-component-versioning.md).</span><span class="sxs-lookup"><span data-stu-id="b9b91-126">For more information about  HDInsight cluster versions, see [HDInsight cluster versions](hdinsight-component-versioning.md).</span></span>
>
>

1. <span data-ttu-id="b9b91-127">När du skapar ett HDInsight-kluster från hello Portal, klickar du på **valfri konfiguration**, och klicka sedan på **skriptåtgärder**.</span><span class="sxs-lookup"><span data-stu-id="b9b91-127">When you create an HDInsight cluster from hello Portal, click **Optional Configuration**, and then click **Script Actions**.</span></span>
2. <span data-ttu-id="b9b91-128">På hello **skriptåtgärder** anger hello följande värden:</span><span class="sxs-lookup"><span data-stu-id="b9b91-128">On hello **Script Actions** page, enter hello following values:</span></span>

    <span data-ttu-id="b9b91-129">![Använd skriptåtgärder toocustomize ett kluster](./media/hdinsight-hadoop-r-scripts/hdi-r-script-action.png "Använd skriptåtgärder toocustomize ett kluster")</span><span class="sxs-lookup"><span data-stu-id="b9b91-129">![Use Script Action toocustomize a cluster](./media/hdinsight-hadoop-r-scripts/hdi-r-script-action.png "Use Script Action toocustomize a cluster")</span></span>

    <table border='1'>
        <tr><th><span data-ttu-id="b9b91-130">Egenskap</span><span class="sxs-lookup"><span data-stu-id="b9b91-130">Property</span></span></th><th><span data-ttu-id="b9b91-131">Värde</span><span class="sxs-lookup"><span data-stu-id="b9b91-131">Value</span></span></th></tr>
        <tr><td><span data-ttu-id="b9b91-132">Namn</span><span class="sxs-lookup"><span data-stu-id="b9b91-132">Name</span></span></td>
            <td><span data-ttu-id="b9b91-133">Ange ett namn för hello skriptåtgärd, till exempel <b>installera R</b>.</span><span class="sxs-lookup"><span data-stu-id="b9b91-133">Specify a name for hello script action, for example, <b>Install R</b>.</span></span></td></tr>
        <tr><td><span data-ttu-id="b9b91-134">Skript-URI</span><span class="sxs-lookup"><span data-stu-id="b9b91-134">Script URI</span></span></td>
            <td><span data-ttu-id="b9b91-135">Ange hello URI toohello skript som är anropade toocustomize hello klustret, till exempel <i>https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps1</i></span><span class="sxs-lookup"><span data-stu-id="b9b91-135">Specify hello URI toohello script that is invoked toocustomize hello cluster, for example, <i>https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps1</i></span></span></td></tr>
        <tr><td><span data-ttu-id="b9b91-136">Nodtyp</span><span class="sxs-lookup"><span data-stu-id="b9b91-136">Node Type</span></span></td>
            <td><span data-ttu-id="b9b91-137">Ange hello noder som hello anpassning skript körs.</span><span class="sxs-lookup"><span data-stu-id="b9b91-137">Specify hello nodes on which hello customization script is run.</span></span> <span data-ttu-id="b9b91-138">Du kan välja <b>alla noder</b>, <b>huvudnoder endast</b>, eller <b>arbetarnoder</b> endast.</span><span class="sxs-lookup"><span data-stu-id="b9b91-138">You can choose <b>All Nodes</b>, <b>Head nodes only</b>, or <b>Worker nodes</b> only.</span></span>
        <tr><td><span data-ttu-id="b9b91-139">Parametrar</span><span class="sxs-lookup"><span data-stu-id="b9b91-139">Parameters</span></span></td>
            <td><span data-ttu-id="b9b91-140">Ange hello parametrar, om det krävs av hello skript.</span><span class="sxs-lookup"><span data-stu-id="b9b91-140">Specify hello parameters, if required by hello script.</span></span> <span data-ttu-id="b9b91-141">Hello skriptet tooinstall R kräver dock inte parametrar, så du kan lämna den tom.</span><span class="sxs-lookup"><span data-stu-id="b9b91-141">However, hello script tooinstall R does not require any parameters, so you can leave this blank.</span></span></td></tr>
    </table>

    <span data-ttu-id="b9b91-142">Du kan lägga till fler än en skript åtgärd tooinstall flera komponenter på hello klustret.</span><span class="sxs-lookup"><span data-stu-id="b9b91-142">You can add more than one script action tooinstall multiple components on hello cluster.</span></span> <span data-ttu-id="b9b91-143">När du har lagt till hello skript klickar du på hello markerat toostart crating hello klustret.</span><span class="sxs-lookup"><span data-stu-id="b9b91-143">After you have added hello scripts, click hello check mark toostart crating hello cluster.</span></span>

<span data-ttu-id="b9b91-144">Du kan också använda hello skriptet tooinstall R i HDInsight med hjälp av Azure PowerShell eller hello HDInsight .NET SDK.</span><span class="sxs-lookup"><span data-stu-id="b9b91-144">You can also use hello script tooinstall R on HDInsight by using Azure PowerShell or hello HDInsight .NET SDK.</span></span> <span data-ttu-id="b9b91-145">Anvisningar för de här procedurerna finns senare i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="b9b91-145">Instructions for these procedures are provided later in this article.</span></span>

## <a name="run-r-scripts"></a><span data-ttu-id="b9b91-146">R-skript körs</span><span class="sxs-lookup"><span data-stu-id="b9b91-146">Run R scripts</span></span>
<span data-ttu-id="b9b91-147">Det här avsnittet beskrivs hur toorun ett R-skriptet på hello Hadoop-kluster med HDInsight.</span><span class="sxs-lookup"><span data-stu-id="b9b91-147">This section describes how toorun an R script on hello Hadoop cluster with HDInsight.</span></span>

1. <span data-ttu-id="b9b91-148">**Upprätta ett fjärrskrivbord anslutning toohello kluster**: aktivera Fjärrskrivbord för hello-klustret som du skapade med R installerat från hello Portal och ansluter sedan toohello klustret.</span><span class="sxs-lookup"><span data-stu-id="b9b91-148">**Establish a Remote Desktop connection toohello cluster**: From hello Portal, enable Remote Desktop for hello cluster you created with R installed, and then connect toohello cluster.</span></span> <span data-ttu-id="b9b91-149">Instruktioner finns i [ansluta tooHDInsight kluster med RDP](hdinsight-administer-use-management-portal.md#connect-to-clusters-using-rdp).</span><span class="sxs-lookup"><span data-stu-id="b9b91-149">For instructions, see [Connect tooHDInsight clusters using RDP](hdinsight-administer-use-management-portal.md#connect-to-clusters-using-rdp).</span></span>
2. <span data-ttu-id="b9b91-150">**Öppna hello R konsolen**: hello R installationen placerar en länk toohello R-konsol på hello skrivbord hello huvudnod.</span><span class="sxs-lookup"><span data-stu-id="b9b91-150">**Open hello R console**: hello R installation puts a link toohello R console on hello desktop of hello head node.</span></span> <span data-ttu-id="b9b91-151">Klicka på den tooopen hello R-konsolen.</span><span class="sxs-lookup"><span data-stu-id="b9b91-151">Click on it tooopen hello R console.</span></span>
3. <span data-ttu-id="b9b91-152">**Kör hello R-skriptet**: hello R-skript kan köras direkt från hello R-konsolen genom att klistra in, markera den och trycka på RETUR.</span><span class="sxs-lookup"><span data-stu-id="b9b91-152">**Run hello R script**: hello R script can be run directly from hello R console by pasting it, selecting it, and pressing ENTER.</span></span> <span data-ttu-id="b9b91-153">Här är ett enkelt exempelskript som genererar hello talen 1 too100 och multiplicerar dem med 2.</span><span class="sxs-lookup"><span data-stu-id="b9b91-153">Here is a simple example script that generates hello numbers 1 too100 and then multiplies them by 2.</span></span>

        library(rmr2)
        library(rhdfs)
        ints = to.dfs(1:100)
        calc = mapreduce(input = ints, map = function(k, v) cbind(v, 2*v))
        from.dfs(calc)

<span data-ttu-id="b9b91-154">hello två första raderna anropet hello RHadoop bibliotek som installeras med R. hello sista raden utskrifter hello resultat toohello konsolen.</span><span class="sxs-lookup"><span data-stu-id="b9b91-154">hello first two lines call hello RHadoop libraries that are installed with R. hello final line prints hello results toohello console.</span></span> <span data-ttu-id="b9b91-155">hello utdata ska se ut så här:</span><span class="sxs-lookup"><span data-stu-id="b9b91-155">hello output should look like this:</span></span>

    [1,]  1 2
    [2,]  2 4
    .
    .
    .
    [98,]  98 196
    [99,]  99 198
    [100,] 100 200


## <a name="install-r-using-aure-powershell"></a><span data-ttu-id="b9b91-156">Installera R med Aure PowerShell</span><span class="sxs-lookup"><span data-stu-id="b9b91-156">Install R using Aure PowerShell</span></span>
<span data-ttu-id="b9b91-157">Se [anpassa HDInsight-kluster med skriptåtgärder](hdinsight-hadoop-customize-cluster.md#call-scripts-using-azure-powershell).</span><span class="sxs-lookup"><span data-stu-id="b9b91-157">See [Customize HDInsight clusters using Script Action](hdinsight-hadoop-customize-cluster.md#call-scripts-using-azure-powershell).</span></span>  <span data-ttu-id="b9b91-158">hello exemplet visar hur tooinstall Spark med hjälp av Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="b9b91-158">hello sample demonstrates how tooinstall Spark using Azure PowerShell.</span></span> <span data-ttu-id="b9b91-159">Du behöver toocustomize hello skriptet toouse [https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps1](https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps1).</span><span class="sxs-lookup"><span data-stu-id="b9b91-159">You need toocustomize hello script toouse [https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps1](https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps1).</span></span>

## <a name="install-r-using-net-sdk"></a><span data-ttu-id="b9b91-160">Installera R med .NET SDK</span><span class="sxs-lookup"><span data-stu-id="b9b91-160">Install R using .NET SDK</span></span>
<span data-ttu-id="b9b91-161">Se [anpassa HDInsight-kluster med skriptåtgärder](hdinsight-hadoop-customize-cluster.md#call-scripts-using-azure-powershell).</span><span class="sxs-lookup"><span data-stu-id="b9b91-161">See [Customize HDInsight clusters using Script Action](hdinsight-hadoop-customize-cluster.md#call-scripts-using-azure-powershell).</span></span> <span data-ttu-id="b9b91-162">hello exemplet visar hur tooinstall Spark med hello .NET SDK.</span><span class="sxs-lookup"><span data-stu-id="b9b91-162">hello sample demonstrates how tooinstall Spark using hello .NET SDK.</span></span> <span data-ttu-id="b9b91-163">Du behöver toocustomize hello skriptet toouse [https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps1](https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps11).</span><span class="sxs-lookup"><span data-stu-id="b9b91-163">You need toocustomize hello script toouse [https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps1](https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps11).</span></span>

## <a name="see-also"></a><span data-ttu-id="b9b91-164">Se även</span><span class="sxs-lookup"><span data-stu-id="b9b91-164">See also</span></span>
* [<span data-ttu-id="b9b91-165">Installera och använda R på HDinsight Hadoop-kluster (Linux)</span><span class="sxs-lookup"><span data-stu-id="b9b91-165">Install and use R on HDinsight Hadoop clusters (Linux)</span></span>](hdinsight-hadoop-r-scripts-linux.md)
* <span data-ttu-id="b9b91-166">[Skapa Hadoop-kluster i HDInsight](hdinsight-hadoop-provision-linux-clusters.md): allmän information om hur du skapar HDInsight-kluster</span><span class="sxs-lookup"><span data-stu-id="b9b91-166">[Create Hadoop clusters in HDInsight](hdinsight-hadoop-provision-linux-clusters.md): general information on creating HDInsight clusters</span></span>
* <span data-ttu-id="b9b91-167">[Anpassa HDInsight-kluster med skriptåtgärder][hdinsight-cluster-customize]: allmän information om hur du anpassar HDInsight-kluster med skriptåtgärder</span><span class="sxs-lookup"><span data-stu-id="b9b91-167">[Customize HDInsight cluster using Script Action][hdinsight-cluster-customize]: general information on customizing HDInsight clusters using Script Action</span></span>
* [<span data-ttu-id="b9b91-168">Utveckla skriptåtgärd skript för HDInsight</span><span class="sxs-lookup"><span data-stu-id="b9b91-168">Develop Script Action scripts for HDInsight</span></span>](hdinsight-hadoop-script-actions.md)
* <span data-ttu-id="b9b91-169">[Installera och använda Spark på HDInsight-kluster][hdinsight-install-spark]: skriptåtgärder exempel om hur du installerar Spark</span><span class="sxs-lookup"><span data-stu-id="b9b91-169">[Install and use Spark on HDInsight clusters][hdinsight-install-spark]: Script Action sample about installing Spark</span></span>
* <span data-ttu-id="b9b91-170">[Installera Giraph på HDInsight-kluster](hdinsight-hadoop-giraph-install.md): skriptåtgärder exempel om hur du installerar Giraph</span><span class="sxs-lookup"><span data-stu-id="b9b91-170">[Install Giraph on HDInsight clusters](hdinsight-hadoop-giraph-install.md): Script Action sample about installing Giraph</span></span>
* <span data-ttu-id="b9b91-171">[Installera Solr på HDInsight-kluster](hdinsight-hadoop-solr-install-linux.md): skriptåtgärder exempel om hur du installerar Solr.</span><span class="sxs-lookup"><span data-stu-id="b9b91-171">[Install Solr on HDInsight clusters](hdinsight-hadoop-solr-install-linux.md): Script Action sample about installing Solr.</span></span>

[powershell-install-configure]: /powershell/azureps-cmdlets-docs
[hdinsight-provision]: ../hdinsight-provision-clusters/
[hdinsight-cluster-customize]: hdinsight-hadoop-customize-cluster-linux.md
[hdinsight-install-spark]: hdinsight-apache-spark-jupyter-spark-sql.md
