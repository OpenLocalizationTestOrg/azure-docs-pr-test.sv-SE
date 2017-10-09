---
title: "aaaInstall och använda Giraph på HDInsight (Hadoop) - Azure | Microsoft Docs"
description: "Lär dig hur tooinstall Giraph på Linux-baserade HDInsight-kluster med skriptåtgärder. Skriptåtgärder kan du toocustomize hello klustret när skapas genom att ändra klusterkonfigurationen eller installera tjänster och verktyg."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 9fcac906-8f06-4002-9fe8-473e42f8fd0f
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/31/2017
ms.author: larryfr
ms.openlocfilehash: 0f195b65cebf5e24d1808ef33b95b4d362555521
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="install-giraph-on-hdinsight-hadoop-clusters-and-use-giraph-tooprocess-large-scale-graphs"></a><span data-ttu-id="8423f-104">Installera Giraph på HDInsight Hadoop-kluster och Använd Giraph tooprocess storskaliga diagram</span><span class="sxs-lookup"><span data-stu-id="8423f-104">Install Giraph on HDInsight Hadoop clusters, and use Giraph tooprocess large-scale graphs</span></span>

<span data-ttu-id="8423f-105">Lär dig hur tooinstall Apache Giraph på ett HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="8423f-105">Learn how tooinstall Apache Giraph on an HDInsight cluster.</span></span> <span data-ttu-id="8423f-106">hello skript åtgärd funktion i HDInsight kan du toocustomize klustret genom att köra ett bash-skript.</span><span class="sxs-lookup"><span data-stu-id="8423f-106">hello script action feature of HDInsight allows you toocustomize your cluster by running a bash script.</span></span> <span data-ttu-id="8423f-107">Skript kan vara används toocustomize kluster under och efter klustret skapas.</span><span class="sxs-lookup"><span data-stu-id="8423f-107">Scripts can be used toocustomize clusters during and after cluster creation.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="8423f-108">hello stegen i det här dokumentet kräver ett HDInsight-kluster som använder Linux.</span><span class="sxs-lookup"><span data-stu-id="8423f-108">hello steps in this document require an HDInsight cluster that uses Linux.</span></span> <span data-ttu-id="8423f-109">Linux är hello endast operativsystem på HDInsight version 3.4 eller senare.</span><span class="sxs-lookup"><span data-stu-id="8423f-109">Linux is hello only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="8423f-110">Mer information finns i [HDInsight-avveckling på Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="8423f-110">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

## <span data-ttu-id="8423f-111"><a name="whatis"></a>Vad är Giraph</span><span class="sxs-lookup"><span data-stu-id="8423f-111"><a name="whatis"></a>What is Giraph</span></span>

<span data-ttu-id="8423f-112">[Apache Giraph](http://giraph.apache.org/) kan du tooperform diagrammet bearbetning med hjälp av Hadoop och kan användas med Azure HDInsight.</span><span class="sxs-lookup"><span data-stu-id="8423f-112">[Apache Giraph](http://giraph.apache.org/) allows you tooperform graph processing by using Hadoop, and can be used with Azure HDInsight.</span></span> <span data-ttu-id="8423f-113">Diagram modell förhållanden mellan objekt.</span><span class="sxs-lookup"><span data-stu-id="8423f-113">Graphs model relationships between objects.</span></span> <span data-ttu-id="8423f-114">Hello anslutningar mellan routrar i ett större nätverk som till exempel hello Internet eller relationer mellan personer på sociala nätverk.</span><span class="sxs-lookup"><span data-stu-id="8423f-114">For example, hello connections between routers on a large network like hello Internet, or relationships between people on social networks.</span></span> <span data-ttu-id="8423f-115">Diagrammet bearbetning kan du tooreason om hello relationer mellan objekt i ett diagram som:</span><span class="sxs-lookup"><span data-stu-id="8423f-115">Graph processing allows you tooreason about hello relationships between objects in a graph, such as:</span></span>

* <span data-ttu-id="8423f-116">Identifiera potentiella vänner baserat på din aktuella relationer.</span><span class="sxs-lookup"><span data-stu-id="8423f-116">Identifying potential friends based on your current relationships.</span></span>

* <span data-ttu-id="8423f-117">Identifiera hello kortaste vägen mellan två datorer i ett nätverk.</span><span class="sxs-lookup"><span data-stu-id="8423f-117">Identifying hello shortest route between two computers in a network.</span></span>

* <span data-ttu-id="8423f-118">Beräkning av hello sidan rangordningen för webbsidor.</span><span class="sxs-lookup"><span data-stu-id="8423f-118">Calculating hello page rank of webpages.</span></span>

> [!WARNING]
> <span data-ttu-id="8423f-119">Komponenter som ingår i hello HDInsight-kluster stöds fullt ut - Microsoft-supporten hjälper tooisolate och lösa problem relaterade toothese komponenter.</span><span class="sxs-lookup"><span data-stu-id="8423f-119">Components provided with hello HDInsight cluster are fully supported - Microsoft Support helps tooisolate and resolve issues related toothese components.</span></span>
>
> <span data-ttu-id="8423f-120">Anpassade komponenter, till exempel Giraph, ta emot kommersiellt rimliga stöd toohelp du toofurther hello felsökning.</span><span class="sxs-lookup"><span data-stu-id="8423f-120">Custom components, such as Giraph, receive commercially reasonable support toohelp you toofurther troubleshoot hello issue.</span></span> <span data-ttu-id="8423f-121">Microsoft-supporten kanske kan tooresolving hello problemet.</span><span class="sxs-lookup"><span data-stu-id="8423f-121">Microsoft Support may be able tooresolving hello issue.</span></span> <span data-ttu-id="8423f-122">Om inte, måste du kontakta öppen källkod communities där djup expertis för att teknik finns.</span><span class="sxs-lookup"><span data-stu-id="8423f-122">If not, you must consult open source communities where deep expertise for that technology is found.</span></span> <span data-ttu-id="8423f-123">Det finns till exempel många community-webbplatser som kan användas, t.ex: [MSDN-forum för HDInsight](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=hdinsight), [http://stackoverflow.com](http://stackoverflow.com). Apache-projekt har också project-webbplatser [http://apache.org](http://apache.org), till exempel: [Hadoop](http://hadoop.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="8423f-123">For example, there are many community sites that can be used, like: [MSDN forum for HDInsight](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=hdinsight), [http://stackoverflow.com](http://stackoverflow.com). Also Apache projects have project sites on [http://apache.org](http://apache.org), for example: [Hadoop](http://hadoop.apache.org/).</span></span>


## <a name="what-hello-script-does"></a><span data-ttu-id="8423f-124">Vilka hello skriptet gör</span><span class="sxs-lookup"><span data-stu-id="8423f-124">What hello script does</span></span>

<span data-ttu-id="8423f-125">Det här skriptet utför hello följande åtgärder:</span><span class="sxs-lookup"><span data-stu-id="8423f-125">This script performs hello following actions:</span></span>

* <span data-ttu-id="8423f-126">Installerar Giraph för`/usr/hdp/current/giraph`</span><span class="sxs-lookup"><span data-stu-id="8423f-126">Installs Giraph too`/usr/hdp/current/giraph`</span></span>

* <span data-ttu-id="8423f-127">Kopior hello `giraph-examples.jar` filen toodefault storage (WASB) för klustret:`/example/jars/giraph-examples.jar`</span><span class="sxs-lookup"><span data-stu-id="8423f-127">Copies hello `giraph-examples.jar` file toodefault storage (WASB) for your cluster: `/example/jars/giraph-examples.jar`</span></span>

## <span data-ttu-id="8423f-128"><a name="install"></a>Installera Giraph med hjälp av skriptåtgärder</span><span class="sxs-lookup"><span data-stu-id="8423f-128"><a name="install"></a>Install Giraph using Script Actions</span></span>

<span data-ttu-id="8423f-129">Ett exempel skriptet tooinstall Giraph på ett HDInsight-kluster finns på följande plats hello:</span><span class="sxs-lookup"><span data-stu-id="8423f-129">A sample script tooinstall Giraph on an HDInsight cluster is available at hello following location:</span></span>

    https://hdiconfigactions.blob.core.windows.net/linuxgiraphconfigactionv01/giraph-installer-v01.sh

<span data-ttu-id="8423f-130">Det här avsnittet innehåller instruktioner om hur toouse hello exempelskript när du skapar hello kluster med hjälp av hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="8423f-130">This section provides instructions on how toouse hello sample script while creating hello cluster by using hello Azure portal.</span></span>

> [!NOTE]
> <span data-ttu-id="8423f-131">En skriptåtgärd kan tillämpas med hjälp av hello följande metoder:</span><span class="sxs-lookup"><span data-stu-id="8423f-131">A script action can be applied using any of hello following methods:</span></span>
> * <span data-ttu-id="8423f-132">Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="8423f-132">Azure PowerShell</span></span>
> * <span data-ttu-id="8423f-133">hello Azure CLI</span><span class="sxs-lookup"><span data-stu-id="8423f-133">hello Azure CLI</span></span>
> * <span data-ttu-id="8423f-134">Hej HDInsight .NET SDK</span><span class="sxs-lookup"><span data-stu-id="8423f-134">hello HDInsight .NET SDK</span></span>
> * <span data-ttu-id="8423f-135">Azure Resource Manager-mallar</span><span class="sxs-lookup"><span data-stu-id="8423f-135">Azure Resource Manager templates</span></span>
> 
> <span data-ttu-id="8423f-136">Du kan också använda skriptet åtgärder tooalready kluster som körs.</span><span class="sxs-lookup"><span data-stu-id="8423f-136">You can also apply script actions tooalready running clusters.</span></span> <span data-ttu-id="8423f-137">Mer information finns i [anpassa HDInsight-kluster med skriptåtgärder](hdinsight-hadoop-customize-cluster-linux.md).</span><span class="sxs-lookup"><span data-stu-id="8423f-137">For more information, see [Customize HDInsight clusters with Script Actions](hdinsight-hadoop-customize-cluster-linux.md).</span></span>

1. <span data-ttu-id="8423f-138">Skapa ett kluster med hjälp av hello stegen i [skapa Linux-baserade HDInsight-kluster](hdinsight-hadoop-create-linux-clusters-portal.md), men inte slutföra skapandet.</span><span class="sxs-lookup"><span data-stu-id="8423f-138">Start creating a cluster by using hello steps in [Create Linux-based HDInsight clusters](hdinsight-hadoop-create-linux-clusters-portal.md), but do not complete creation.</span></span>

2. <span data-ttu-id="8423f-139">På hello **valfri konfiguration** bladet väljer **skriptåtgärder**, och ange hello följande information:</span><span class="sxs-lookup"><span data-stu-id="8423f-139">On hello **Optional Configuration** blade, select **Script Actions**, and provide hello following information:</span></span>

   * <span data-ttu-id="8423f-140">**NAMNET**: Ange ett eget namn för hello skriptåtgärder.</span><span class="sxs-lookup"><span data-stu-id="8423f-140">**NAME**: Enter a friendly name for hello script action.</span></span>

   * <span data-ttu-id="8423f-141">**SKRIPT-URI**: https://hdiconfigactions.blob.core.windows.net/linuxgiraphconfigactionv01/giraph-installer-v01.sh</span><span class="sxs-lookup"><span data-stu-id="8423f-141">**SCRIPT URI**: https://hdiconfigactions.blob.core.windows.net/linuxgiraphconfigactionv01/giraph-installer-v01.sh</span></span>

   * <span data-ttu-id="8423f-142">**HEAD**: Kontrollera posten</span><span class="sxs-lookup"><span data-stu-id="8423f-142">**HEAD**: Check this entry</span></span>

   * <span data-ttu-id="8423f-143">**WORKER**: lämna det här alternativet är avmarkerat</span><span class="sxs-lookup"><span data-stu-id="8423f-143">**WORKER**: Leave this entry unchecked</span></span>

   * <span data-ttu-id="8423f-144">**ZOOKEEPER**: lämna det här alternativet är avmarkerat</span><span class="sxs-lookup"><span data-stu-id="8423f-144">**ZOOKEEPER**: Leave this entry unchecked</span></span>

   * <span data-ttu-id="8423f-145">**PARAMETRARNA**: lämna fältet tomt</span><span class="sxs-lookup"><span data-stu-id="8423f-145">**PARAMETERS**: Leave this field blank</span></span>

3. <span data-ttu-id="8423f-146">Längst ned hello hello **skriptåtgärder**, använda hello **Välj** toosave hello konfiguration.</span><span class="sxs-lookup"><span data-stu-id="8423f-146">At hello bottom of hello **Script Actions**, use hello **Select** button toosave hello configuration.</span></span> <span data-ttu-id="8423f-147">Använd slutligen hello **Välj** knappen längst ned hello hello **valfri konfiguration** bladet toosave hello ytterligare konfigurationsinformation.</span><span class="sxs-lookup"><span data-stu-id="8423f-147">Finally, use hello **Select** button at hello bottom of hello **Optional Configuration** blade toosave hello optional configuration information.</span></span>

4. <span data-ttu-id="8423f-148">Fortsätta skapa hello klustret enligt beskrivningen i [skapa Linux-baserade HDInsight-kluster](hdinsight-hadoop-create-linux-clusters-portal.md).</span><span class="sxs-lookup"><span data-stu-id="8423f-148">Continue creating hello cluster as described in [Create Linux-based HDInsight clusters](hdinsight-hadoop-create-linux-clusters-portal.md).</span></span>

## <span data-ttu-id="8423f-149"><a name="usegiraph"></a>Hur använder Giraph i HDInsight?</span><span class="sxs-lookup"><span data-stu-id="8423f-149"><a name="usegiraph"></a>How do I use Giraph in HDInsight?</span></span>

<span data-ttu-id="8423f-150">Använda hello följande steg toorun hello SimpleShortestPathsComputation exempel medföljer Giraph när hello klustret har skapats.</span><span class="sxs-lookup"><span data-stu-id="8423f-150">Once hello cluster has been created, use hello following steps toorun hello SimpleShortestPathsComputation example included with Giraph.</span></span> <span data-ttu-id="8423f-151">Det här exemplet används hello basic [Pregel](http://people.apache.org/~edwardyoon/documents/pregel.pdf) implementering för att hitta hello shortest path mellan objekt i ett diagram.</span><span class="sxs-lookup"><span data-stu-id="8423f-151">This example uses hello basic [Pregel](http://people.apache.org/~edwardyoon/documents/pregel.pdf) implementation for finding hello shortest path between objects in a graph.</span></span>

1. <span data-ttu-id="8423f-152">Anslut toohello HDInsight-kluster med SSH:</span><span class="sxs-lookup"><span data-stu-id="8423f-152">Connect toohello HDInsight cluster using SSH:</span></span>

    ```bash
    ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net
    ```

    <span data-ttu-id="8423f-153">Mer information finns i [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md) (Använda SSH med HDInsight).</span><span class="sxs-lookup"><span data-stu-id="8423f-153">For information, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

2. <span data-ttu-id="8423f-154">Använd hello följande kommando toocreate en fil med namnet **tiny_graph.txt**:</span><span class="sxs-lookup"><span data-stu-id="8423f-154">Use hello following command toocreate a file named **tiny_graph.txt**:</span></span>

    ```bash
    nano tiny_graph.txt
    ```

    <span data-ttu-id="8423f-155">Använd hello följande text som hello innehållet i den här filen:</span><span class="sxs-lookup"><span data-stu-id="8423f-155">Use hello following text as hello contents of this file:</span></span>

    ```text
    [0,0,[[1,1],[3,3]]]
    [1,0,[[0,1],[2,2],[3,1]]]
    [2,0,[[1,2],[4,4]]]
    [3,0,[[0,3],[1,1],[4,4]]]
    [4,0,[[3,4],[2,4]]]
    ```

    <span data-ttu-id="8423f-156">Informationen beskriver en relation mellan objekt i en riktat diagram hello formatet `[source_id, source_value,[[dest_id], [edge_value],...]]`.</span><span class="sxs-lookup"><span data-stu-id="8423f-156">This data describes a relationship between objects in a directed graph, by using hello format `[source_id, source_value,[[dest_id], [edge_value],...]]`.</span></span> <span data-ttu-id="8423f-157">Varje rad representerar en relation mellan en `source_id` objekt och en eller flera `dest_id` objekt.</span><span class="sxs-lookup"><span data-stu-id="8423f-157">Each line represents a relationship between a `source_id` object and one or more `dest_id` objects.</span></span> <span data-ttu-id="8423f-158">Hej `edge_value` kan betraktas som hello styrkan eller avståndet mellan hello anslutning mellan `source_id` och `dest\_id`.</span><span class="sxs-lookup"><span data-stu-id="8423f-158">hello `edge_value` can be thought of as hello strength or distance of hello connection between `source_id` and `dest\_id`.</span></span>

    <span data-ttu-id="8423f-159">Dras ut, och använder hello värde (eller vikt) som hello avståndet mellan objekt, hello data kan se ut hello följande diagram:</span><span class="sxs-lookup"><span data-stu-id="8423f-159">Drawn out, and using hello value (or weight) as hello distance between objects, hello data might look like hello following diagram:</span></span>

    ![tiny_graph.txt ritas som cirklar med rader med olika avståndet mellan](./media/hdinsight-hadoop-giraph-install-linux/giraph-graph.png)

3. <span data-ttu-id="8423f-161">toosave hello-fil, Använd **Ctrl + X**, sedan **Y**, och slutligen **RETUR** tooaccept hello-filnamn.</span><span class="sxs-lookup"><span data-stu-id="8423f-161">toosave hello file, use **Ctrl+X**, then **Y**, and finally **Enter** tooaccept hello file name.</span></span>

4. <span data-ttu-id="8423f-162">Använd hello följande toostore hello data till primära lagringsplatsen för HDInsight-kluster:</span><span class="sxs-lookup"><span data-stu-id="8423f-162">Use hello following toostore hello data into primary storage for your HDInsight cluster:</span></span>

    ```bash
    hdfs dfs -put tiny_graph.txt /example/data/tiny_graph.txt
    ```

5. <span data-ttu-id="8423f-163">Kör hello SimpleShortestPathsComputation exempel på användning av hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="8423f-163">Run hello SimpleShortestPathsComputation example using hello following command:</span></span>

    ```bash
    yarn jar /usr/hdp/current/giraph/giraph-examples.jar org.apache.giraph.GiraphRunner org.apache.giraph.examples.SimpleShortestPathsComputation -ca mapred.job.tracker=headnodehost:9010 -vif org.apache.giraph.io.formats.JsonLongDoubleFloatDoubleVertexInputFormat -vip /example/data/tiny_graph.txt -vof org.apache.giraph.io.formats.IdWithValueTextOutputFormat -op /example/output/shortestpaths -w 2
    ```

    <span data-ttu-id="8423f-164">hello-parametrar som används med det här kommandot beskrivs i följande tabell hello:</span><span class="sxs-lookup"><span data-stu-id="8423f-164">hello parameters used with this command are described in hello following table:</span></span>

   | <span data-ttu-id="8423f-165">Parameter</span><span class="sxs-lookup"><span data-stu-id="8423f-165">Parameter</span></span> | <span data-ttu-id="8423f-166">Vad verktyget gör</span><span class="sxs-lookup"><span data-stu-id="8423f-166">What it does</span></span> |
   | --- | --- |
   | `jar` |<span data-ttu-id="8423f-167">hello jar-fil som innehåller hello exempel.</span><span class="sxs-lookup"><span data-stu-id="8423f-167">hello jar file containing hello examples.</span></span> |
   | `org.apache.giraph.GiraphRunner` |<span data-ttu-id="8423f-168">hello klassen används toostart hello exempel.</span><span class="sxs-lookup"><span data-stu-id="8423f-168">hello class used toostart hello examples.</span></span> |
   | `org.apache.giraph.examples.SimpleShortestPathsCoputation` |<span data-ttu-id="8423f-169">hello-exempel som används.</span><span class="sxs-lookup"><span data-stu-id="8423f-169">hello example that is used.</span></span> <span data-ttu-id="8423f-170">I det här exemplet beräknar hello kortaste sökvägen mellan ID 1 och alla andra ID: N i hello graph.</span><span class="sxs-lookup"><span data-stu-id="8423f-170">In this example, it computes hello shortest path between ID 1 and all other IDs in hello graph.</span></span> |
   | `-ca mapred.job.tracker` |<span data-ttu-id="8423f-171">Hej headnode för hello-kluster.</span><span class="sxs-lookup"><span data-stu-id="8423f-171">hello headnode for hello cluster.</span></span> |
   | `-vif` |<span data-ttu-id="8423f-172">hello Indataformatet toouse för hello indata.</span><span class="sxs-lookup"><span data-stu-id="8423f-172">hello input format toouse for hello input data.</span></span> |
   | `-vip` |<span data-ttu-id="8423f-173">hello indatafilen.</span><span class="sxs-lookup"><span data-stu-id="8423f-173">hello input data file.</span></span> |
   | `-vof` |<span data-ttu-id="8423f-174">hello utdataformat.</span><span class="sxs-lookup"><span data-stu-id="8423f-174">hello output format.</span></span> <span data-ttu-id="8423f-175">I det här exemplet ID och värdet som oformaterad text.</span><span class="sxs-lookup"><span data-stu-id="8423f-175">In this example, ID and value as plain text.</span></span> |
   | `-op` |<span data-ttu-id="8423f-176">hello platsen.</span><span class="sxs-lookup"><span data-stu-id="8423f-176">hello output location.</span></span> |
   | `-w 2` |<span data-ttu-id="8423f-177">hello antal arbetare toouse.</span><span class="sxs-lookup"><span data-stu-id="8423f-177">hello number of workers toouse.</span></span> <span data-ttu-id="8423f-178">I det här exemplet 2.</span><span class="sxs-lookup"><span data-stu-id="8423f-178">In this example, 2.</span></span> |

    <span data-ttu-id="8423f-179">Mer information om dessa och andra parametrar som används med Giraph exempel finns hello [Giraph quickstart](http://giraph.apache.org/quick_start.html).</span><span class="sxs-lookup"><span data-stu-id="8423f-179">For more information on these, and other parameters used with Giraph samples, see hello [Giraph quickstart](http://giraph.apache.org/quick_start.html).</span></span>

6. <span data-ttu-id="8423f-180">När hello jobbet har slutförts, hello resultat lagras i hello **/example/out/shotestpaths** directory.</span><span class="sxs-lookup"><span data-stu-id="8423f-180">Once hello job has finished, hello results are stored in hello **/example/out/shotestpaths** directory.</span></span> <span data-ttu-id="8423f-181">hello utdata-filnamn börjar med **del-m -** och avslutas med ett tal som anger hello först andra, etc.-filen.</span><span class="sxs-lookup"><span data-stu-id="8423f-181">hello output file names begin with **part-m-** and end with a number indicating hello first, second, etc. file.</span></span> <span data-ttu-id="8423f-182">Använd följande kommandoutdata tooview hello hello:</span><span class="sxs-lookup"><span data-stu-id="8423f-182">Use hello following command tooview hello output:</span></span>

    ```bash
    hdfs dfs -text /example/output/shortestpaths/*
    ```

    <span data-ttu-id="8423f-183">hello utdata ska se ut ungefär toohello följande text:</span><span class="sxs-lookup"><span data-stu-id="8423f-183">hello output should appear similar toohello following text:</span></span>

        0    1.0
        4    5.0
        2    2.0
        1    0.0
        3    1.0

    <span data-ttu-id="8423f-184">Hej SimpleShortestPathComputation exempel är hårdkodad toostart med objekt-ID 1 och hitta hello shortest path tooother objekt.</span><span class="sxs-lookup"><span data-stu-id="8423f-184">hello SimpleShortestPathComputation example is hard coded toostart with object ID 1 and find hello shortest path tooother objects.</span></span> <span data-ttu-id="8423f-185">hello-utdata är i hello-format för `destination_id` och `distance`.</span><span class="sxs-lookup"><span data-stu-id="8423f-185">hello output is in hello format of `destination_id` and `distance`.</span></span> <span data-ttu-id="8423f-186">Hej `distance` är hello värde (eller vikt) av hello kanter obligatoriska mellan objekt-ID 1 och hello mål-ID.</span><span class="sxs-lookup"><span data-stu-id="8423f-186">hello `distance` is hello value (or weight) of hello edges traveled between object ID 1 and hello target ID.</span></span>

    <span data-ttu-id="8423f-187">Visualisera data, kan du kontrollera hello resultat av reser hello kortaste sökvägar mellan ID 1 och alla andra objekt.</span><span class="sxs-lookup"><span data-stu-id="8423f-187">Visualizing this data, you can verify hello results by traveling hello shortest paths between ID 1 and all other objects.</span></span> <span data-ttu-id="8423f-188">hello shortest path mellan ID 1 och 4-ID är 5.</span><span class="sxs-lookup"><span data-stu-id="8423f-188">hello shortest path between ID 1 and ID 4 is 5.</span></span> <span data-ttu-id="8423f-189">Det här värdet är hello totala avståndet mellan <span style="color:orange">ID 1 och 3</span>, och sedan <span style="color:red">ID 3 och 4</span>.</span><span class="sxs-lookup"><span data-stu-id="8423f-189">This value is hello total distance between <span style="color:orange">ID 1 and 3</span>, and then <span style="color:red">ID 3 and 4</span>.</span></span>

    ![Ritning av objekt som cirklar med kortast sökvägar mellan](./media/hdinsight-hadoop-giraph-install-linux/giraph-graph-out.png)

## <a name="next-steps"></a><span data-ttu-id="8423f-191">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="8423f-191">Next steps</span></span>

* <span data-ttu-id="8423f-192">[Installera och använda Hue på HDInsight-kluster](hdinsight-hadoop-hue-linux.md).</span><span class="sxs-lookup"><span data-stu-id="8423f-192">[Install and use Hue on HDInsight clusters](hdinsight-hadoop-hue-linux.md).</span></span>

* <span data-ttu-id="8423f-193">[Installera Solr på HDInsight-kluster](hdinsight-hadoop-solr-install-linux.md).</span><span class="sxs-lookup"><span data-stu-id="8423f-193">[Install Solr on HDInsight clusters](hdinsight-hadoop-solr-install-linux.md).</span></span>
