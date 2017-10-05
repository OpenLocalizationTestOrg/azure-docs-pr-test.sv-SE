---
title: "Distribuera och hantera Apache Storm-topologier på Linux-baserade HDInsight | Microsoft Docs"
description: "Lär dig mer om att distribuera, övervaka och hantera Apache Storm-topologier med Storm-instrumentpanelen på Linux-baserade HDInsight. Använda Hadoop-verktyg för Visual Studio."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.assetid: 35086e62-d6d8-4ccf-8cae-00073464a1e1
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/16/2017
ms.author: larryfr
ms.openlocfilehash: b9e82463030807d2674594e73f762fe93515d423
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="deploy-and-manage-apache-storm-topologies-on-hdinsight"></a><span data-ttu-id="d6260-104">Distribuera och hantera Apache Storm-topologier på HDInsight</span><span class="sxs-lookup"><span data-stu-id="d6260-104">Deploy and manage Apache Storm topologies on HDInsight</span></span>

<span data-ttu-id="d6260-105">I detta dokument, lär du dig grunderna för att hantera och övervaka Storm-topologier som körs på Storm på HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="d6260-105">In this document, learn the basics of managing and monitoring Storm topologies running on Storm on HDInsight clusters.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="d6260-106">Stegen i den här artikeln kräver ett Linux-baserade Storm på HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="d6260-106">The steps in this article require a Linux-based Storm on HDInsight cluster.</span></span> <span data-ttu-id="d6260-107">Linux är det enda operativsystemet som används med HDInsight version 3.4 och senare.</span><span class="sxs-lookup"><span data-stu-id="d6260-107">Linux is the only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="d6260-108">Mer information finns i [HDInsight-avveckling på Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="d6260-108">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span> 
>
> <span data-ttu-id="d6260-109">Mer information om distribuera och övervaka topologier på Windows-baserade HDInsight finns [distribuera och hantera Apache Storm-topologier på Windows-baserade HDInsight](hdinsight-storm-deploy-monitor-topology.md)</span><span class="sxs-lookup"><span data-stu-id="d6260-109">For information on deploying and monitoring topologies on Windows-based HDInsight, see [Deploy and manage Apache Storm topologies on Windows-based HDInsight](hdinsight-storm-deploy-monitor-topology.md)</span></span>


## <a name="prerequisites"></a><span data-ttu-id="d6260-110">Krav</span><span class="sxs-lookup"><span data-stu-id="d6260-110">Prerequisites</span></span>

* <span data-ttu-id="d6260-111">**En Linux-baserade Storm på HDInsight-kluster**: se [Kom igång med Apache Storm på HDInsight](hdinsight-apache-storm-tutorial-get-started-linux.md) anvisningar om hur du skapar ett kluster</span><span class="sxs-lookup"><span data-stu-id="d6260-111">**A Linux-based Storm on HDInsight cluster**: see [Get started with Apache Storm on HDInsight](hdinsight-apache-storm-tutorial-get-started-linux.md) for steps on creating a cluster</span></span>

* <span data-ttu-id="d6260-112">(Valfritt) **Kunskap om SSH och SCP**: Mer information finns i [använda SSH med HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="d6260-112">(Optional) **Familiarity with SSH and SCP**: For more information, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

* <span data-ttu-id="d6260-113">(Valfritt) **Visual Studio**: Azure SDK 2.5.1 eller senare och Data Lake-verktyg för Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="d6260-113">(Optional) **Visual Studio**: Azure SDK 2.5.1 or newer and the Data Lake Tools for Visual Studio.</span></span> <span data-ttu-id="d6260-114">Mer information finns i [komma igång med Data Lake-verktyg för Visual Studio](hdinsight-hadoop-visual-studio-tools-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="d6260-114">For more information, see [Get started using Data Lake Tools for Visual Studio](hdinsight-hadoop-visual-studio-tools-get-started.md).</span></span>

    <span data-ttu-id="d6260-115">En av följande versioner av Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="d6260-115">One of the following versions of Visual Studio:</span></span>

  * <span data-ttu-id="d6260-116">Visual Studio 2012 med [Update 4](http://www.microsoft.com/download/details.aspx?id=39305)</span><span class="sxs-lookup"><span data-stu-id="d6260-116">Visual Studio 2012 with [Update 4](http://www.microsoft.com/download/details.aspx?id=39305)</span></span>

  * <span data-ttu-id="d6260-117">Visual Studio 2013 med [uppdatering 4](http://www.microsoft.com/download/details.aspx?id=44921) eller [Visual Studio Community 2013](http://go.microsoft.com/fwlink/?LinkId=517284)</span><span class="sxs-lookup"><span data-stu-id="d6260-117">Visual Studio 2013 with [Update 4](http://www.microsoft.com/download/details.aspx?id=44921) or [Visual Studio 2013 Community](http://go.microsoft.com/fwlink/?LinkId=517284)</span></span>
  * [<span data-ttu-id="d6260-118">Visual Studio 2015</span><span class="sxs-lookup"><span data-stu-id="d6260-118">Visual Studio 2015</span></span>](https://www.visualstudio.com/downloads/)

  * <span data-ttu-id="d6260-119">Visual Studio 2015 (någon utgåva)</span><span class="sxs-lookup"><span data-stu-id="d6260-119">Visual Studio 2015 (any edition)</span></span>

  * <span data-ttu-id="d6260-120">2017 för Visual Studio (någon utgåva).</span><span class="sxs-lookup"><span data-stu-id="d6260-120">Visual Studio 2017 (any edition).</span></span> <span data-ttu-id="d6260-121">Data Lake-verktyg för Visual Studio 2017 installeras som en del av Azure-arbetsbelastning.</span><span class="sxs-lookup"><span data-stu-id="d6260-121">Data Lake Tools for Visual Studio 2017 are installed as part of the Azure Workload.</span></span>

## <a name="submit-a-topology-visual-studio"></a><span data-ttu-id="d6260-122">Skicka en topologi: Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d6260-122">Submit a topology: Visual Studio</span></span>

<span data-ttu-id="d6260-123">HDInsight-verktyg kan användas för att skicka eller hybrid C#-topologier till ditt Storm-kluster.</span><span class="sxs-lookup"><span data-stu-id="d6260-123">The HDInsight Tools can be used to submit C# or hybrid topologies to your Storm cluster.</span></span> <span data-ttu-id="d6260-124">Följande steg använder ett exempelprogram.</span><span class="sxs-lookup"><span data-stu-id="d6260-124">The following steps use a sample application.</span></span> <span data-ttu-id="d6260-125">Information om hur du skapar egna topologier med HDInsight Tools finns [utveckla C#-topologier med HDInsight Tools för Visual Studio](hdinsight-storm-develop-csharp-visual-studio-topology.md).</span><span class="sxs-lookup"><span data-stu-id="d6260-125">For information about creating your own topologies by using the HDInsight Tools, see [Develop C# topologies using the HDInsight Tools for Visual Studio](hdinsight-storm-develop-csharp-visual-studio-topology.md).</span></span>

1. <span data-ttu-id="d6260-126">Om du inte redan har installerat den senaste versionen av Data Lake-verktyg för Visual Studio finns [komma igång med Data Lake-verktyg för Visual Studio](hdinsight-hadoop-visual-studio-tools-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="d6260-126">If you have not already installed the latest version of the Data Lake tools for Visual Studio, see [Get started using Data Lake Tools for Visual Studio](hdinsight-hadoop-visual-studio-tools-get-started.md).</span></span>

    > [!NOTE]
    > <span data-ttu-id="d6260-127">Data Lake-verktyg för Visual Studio kallades HDInsight Tools för Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="d6260-127">The Data Lake Tools for Visual Studio were formerly called the HDInsight Tools for Visual Studio.</span></span>
    >
    > <span data-ttu-id="d6260-128">Data Lake-verktyg för Visual Studio ingår i den __Azure arbetsbelastning__ för Visual Studio-2017.</span><span class="sxs-lookup"><span data-stu-id="d6260-128">Data Lake Tools for Visual Studio are included in the __Azure Workload__ for Visual Studio 2017.</span></span>

2. <span data-ttu-id="d6260-129">Öppna Visual Studio, markera **filen** > **ny** > **projekt**.</span><span class="sxs-lookup"><span data-stu-id="d6260-129">Open Visual Studio, select **File** > **New** > **Project**.</span></span>

3. <span data-ttu-id="d6260-130">I den **nytt projekt** dialogrutan Expandera **installerad** > **mallar**, och välj sedan **HDInsight**.</span><span class="sxs-lookup"><span data-stu-id="d6260-130">In the **New Project** dialog box, expand **Installed** > **Templates**, and then select **HDInsight**.</span></span> <span data-ttu-id="d6260-131">Välj i listan över mallar **Storm exempel**.</span><span class="sxs-lookup"><span data-stu-id="d6260-131">From the list of templates, select **Storm Sample**.</span></span> <span data-ttu-id="d6260-132">Ange ett namn för programmet längst ned i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="d6260-132">At the bottom of the dialog box, type a name for the application.</span></span>

    ![Bild](./media/hdinsight-storm-deploy-monitor-topology-linux/sample.png)

4. <span data-ttu-id="d6260-134">I **Solution Explorer**, högerklicka på projektet och välj **skicka till Storm på HDInsight**.</span><span class="sxs-lookup"><span data-stu-id="d6260-134">In **Solution Explorer**, right-click the project, and select **Submit to Storm on HDInsight**.</span></span>

   > [!NOTE]
   > <span data-ttu-id="d6260-135">Om du uppmanas ange inloggningsuppgifterna för din Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="d6260-135">If prompted, enter the login credentials for your Azure subscription.</span></span> <span data-ttu-id="d6260-136">Om du har mer än en prenumeration kan du logga in på den tabell som innehåller ditt Storm på HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="d6260-136">If you have more than one subscription, log in to the one that contains your Storm on HDInsight cluster.</span></span>

5. <span data-ttu-id="d6260-137">Välj ditt Storm på HDInsight-kluster från de **Storm-kluster** listrutan och välj sedan **skicka**.</span><span class="sxs-lookup"><span data-stu-id="d6260-137">Select your Storm on HDInsight cluster from the **Storm Cluster** drop-down list, and then select **Submit**.</span></span> <span data-ttu-id="d6260-138">Du kan övervaka om överföring har genomförts med den **utdata** fönster.</span><span class="sxs-lookup"><span data-stu-id="d6260-138">You can monitor whether the submission is successful by using the **Output** window.</span></span>

## <a name="submit-a-topology-ssh-and-the-storm-command"></a><span data-ttu-id="d6260-139">Skicka en topologi: SSH och Storm-kommando</span><span class="sxs-lookup"><span data-stu-id="d6260-139">Submit a topology: SSH and the Storm command</span></span>

1. <span data-ttu-id="d6260-140">Använda SSH för att ansluta till HDInsight-klustret.</span><span class="sxs-lookup"><span data-stu-id="d6260-140">Use SSH to connect to the HDInsight cluster.</span></span> <span data-ttu-id="d6260-141">Ersätt **användarnamn** namnet på SSH-inloggning.</span><span class="sxs-lookup"><span data-stu-id="d6260-141">Replace **USERNAME** the name of your SSH login.</span></span> <span data-ttu-id="d6260-142">Ersätt **KLUSTERNAMN** med ditt HDInsight-klustrets namn:</span><span class="sxs-lookup"><span data-stu-id="d6260-142">Replace **CLUSTERNAME** with your HDInsight cluster name:</span></span>

        ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net

    <span data-ttu-id="d6260-143">Mer information om hur du använder SSH för att ansluta till ditt HDInsight-kluster finns [använda SSH med HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="d6260-143">For more information on using SSH to connect to your HDInsight cluster, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

2. <span data-ttu-id="d6260-144">Använd följande kommando för att starta en exempeltopologi:</span><span class="sxs-lookup"><span data-stu-id="d6260-144">Use the following command to start an example topology:</span></span>

        storm jar /usr/hdp/current/storm-client/contrib/storm-starter/storm-starter-topologies-*.jar org.apache.storm.starter.WordCountTopology WordCount

    <span data-ttu-id="d6260-145">Detta kommando startar exempel WordCount-topologi i klustret.</span><span class="sxs-lookup"><span data-stu-id="d6260-145">This command starts the example WordCount topology on the cluster.</span></span> <span data-ttu-id="d6260-146">Den här topologin genererar meningar slumpmässigt och räknar antal förekomster av varje ord i meningarna.</span><span class="sxs-lookup"><span data-stu-id="d6260-146">This topology randomly generate sentences and count the occurrence of each word in the sentences.</span></span>

   > [!NOTE]
   > <span data-ttu-id="d6260-147">När du skickar in topologin till klustret måste du först kopiera jar-filen som innehåller klustret innan du använder kommandot `storm`.</span><span class="sxs-lookup"><span data-stu-id="d6260-147">When submitting topology to the cluster, you must first copy the jar file containing the cluster before using the `storm` command.</span></span> <span data-ttu-id="d6260-148">Du kan använda för att kopiera filen till klustret i `scp` kommando.</span><span class="sxs-lookup"><span data-stu-id="d6260-148">To copy the file to the cluster, you can use the `scp` command.</span></span> <span data-ttu-id="d6260-149">Till exempel, `scp FILENAME.jar USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:FILENAME.jar`</span><span class="sxs-lookup"><span data-stu-id="d6260-149">For example, `scp FILENAME.jar USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:FILENAME.jar`</span></span>
   >
   > <span data-ttu-id="d6260-150">WordCount-exemplet och andra exempel i Storm Starter ingår redan i klustret på `/usr/hdp/current/storm-client/contrib/storm-starter/`.</span><span class="sxs-lookup"><span data-stu-id="d6260-150">The WordCount example, and other storm starter examples, are already included on your cluster at `/usr/hdp/current/storm-client/contrib/storm-starter/`.</span></span>

## <a name="submit-a-topology-programmatically"></a><span data-ttu-id="d6260-151">Skicka en topologi: programmässigt</span><span class="sxs-lookup"><span data-stu-id="d6260-151">Submit a topology: programmatically</span></span>

<span data-ttu-id="d6260-152">Programmässigt kan du distribuera en topologi till Storm på HDInsight genom att kommunicera med tjänsten Nimbus värd i klustret.</span><span class="sxs-lookup"><span data-stu-id="d6260-152">You can programmatically deploy a topology to Storm on HDInsight by communicating with the Nimbus service hosted in your cluster.</span></span> <span data-ttu-id="d6260-153">[https://github.com/Azure-Samples/hdinsight-Java-Deploy-storm-topology](https://github.com/Azure-Samples/hdinsight-java-deploy-storm-topology) innehåller ett exempel på Java-program som visar hur du distribuerar och startar en topologi via Nimbus-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="d6260-153">[https://github.com/Azure-Samples/hdinsight-java-deploy-storm-topology](https://github.com/Azure-Samples/hdinsight-java-deploy-storm-topology) provides an example Java application that demonstrates how to deploy and start a topology through the Nimbus service.</span></span>

## <a name="monitor-and-manage-visual-studio"></a><span data-ttu-id="d6260-154">Övervaka och hantera: Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d6260-154">Monitor and manage: Visual Studio</span></span>

<span data-ttu-id="d6260-155">När en topologi har skickats med Visual Studio den **Storm-topologier** visa för klustret visas.</span><span class="sxs-lookup"><span data-stu-id="d6260-155">When a topology has been successfully submitted using Visual Studio, the **Storm Topologies** view for the cluster appears.</span></span> <span data-ttu-id="d6260-156">Välj topologi från listan för att visa information om topologin som körs.</span><span class="sxs-lookup"><span data-stu-id="d6260-156">Select the topology from the list to view information about the running topology.</span></span>

![Övervakare för Visual studio](./media/hdinsight-storm-deploy-monitor-topology/vsmonitor.png)

> [!NOTE]
> <span data-ttu-id="d6260-158">Du kan också visa **Storm-topologier** från **Server Explorer** genom att expandera **Azure** > **HDInsight**, och sedan Högerklicka på ett Storm på HDInsight-kluster och välja **visa Storm-topologier**.</span><span class="sxs-lookup"><span data-stu-id="d6260-158">You can also view **Storm Topologies** from **Server Explorer** by expanding **Azure** > **HDInsight**, and then right-clicking a Storm on HDInsight cluster, and selecting **View Storm Topologies**.</span></span>

<span data-ttu-id="d6260-159">Markera formen för kanaler eller bultar att visa information om dessa komponenter.</span><span class="sxs-lookup"><span data-stu-id="d6260-159">Select the shape for the spouts or bolts to view information about these components.</span></span> <span data-ttu-id="d6260-160">För varje vald öppnas ett nytt fönster.</span><span class="sxs-lookup"><span data-stu-id="d6260-160">A new window opens for each item selected.</span></span>

### <a name="deactivate-and-reactivate"></a><span data-ttu-id="d6260-161">Inaktivera och återaktivera</span><span class="sxs-lookup"><span data-stu-id="d6260-161">Deactivate and reactivate</span></span>

<span data-ttu-id="d6260-162">Inaktivera en topologi pausar den förrän den har avslutats eller återaktivera.</span><span class="sxs-lookup"><span data-stu-id="d6260-162">Deactivating a topology pauses it until it is killed or reactivated.</span></span> <span data-ttu-id="d6260-163">Använd för att utföra dessa åtgärder i __inaktivera__ och __återaktivera__ knappar överst i den __Topology Summary__.</span><span class="sxs-lookup"><span data-stu-id="d6260-163">To perform these operations, use the __Deactivate__ and __Reactivate__ buttons at the top of the __Topology Summary__.</span></span>

### <a name="rebalance"></a><span data-ttu-id="d6260-164">Balansera</span><span class="sxs-lookup"><span data-stu-id="d6260-164">Rebalance</span></span>

<span data-ttu-id="d6260-165">Ombalansering en topologi kan ändra topologins parallellitet.</span><span class="sxs-lookup"><span data-stu-id="d6260-165">Rebalancing a topology allows the system to revise the parallelism of the topology.</span></span> <span data-ttu-id="d6260-166">Om du har ändrat storlek klustret för att lägga till anteckningar kan ombalansering exempelvis en topologi att se de nya noderna.</span><span class="sxs-lookup"><span data-stu-id="d6260-166">For example, if you have resized the cluster to add more notes, rebalancing allows a topology to see the new nodes.</span></span>

<span data-ttu-id="d6260-167">För att balansera en topologi använder den __ombalansera__ längst upp i den __Topology Summary__.</span><span class="sxs-lookup"><span data-stu-id="d6260-167">To rebalance a topology, use the __Rebalance__ button at the top of the __Topology Summary__.</span></span>

> [!WARNING]
> <span data-ttu-id="d6260-168">Ombalansering en topologi först inaktiverar topologi, sedan distribuerar arbetare jämnt över klustret och sedan returnerar slutligen topologin till tillståndet den var i innan ombalansering inträffade.</span><span class="sxs-lookup"><span data-stu-id="d6260-168">Rebalancing a topology first deactivates the topology, then redistributes workers evenly across the cluster, then finally returns the topology to the state it was in before rebalancing occurred.</span></span> <span data-ttu-id="d6260-169">Så om topologin var aktiv blir aktiva igen.</span><span class="sxs-lookup"><span data-stu-id="d6260-169">So if the topology was active, it becomes active again.</span></span> <span data-ttu-id="d6260-170">Om det har inaktiverats, förblir den inaktiverade.</span><span class="sxs-lookup"><span data-stu-id="d6260-170">If it was deactivated, it remains deactivated.</span></span>

### <a name="kill-a-topology"></a><span data-ttu-id="d6260-171">Avsluta en topologi</span><span class="sxs-lookup"><span data-stu-id="d6260-171">Kill a topology</span></span>

<span data-ttu-id="d6260-172">Storm-topologier fortsätta köras förrän de har stoppats eller ta bort klustret.</span><span class="sxs-lookup"><span data-stu-id="d6260-172">Storm topologies continue running until they are stopped or the cluster is deleted.</span></span> <span data-ttu-id="d6260-173">Om du vill stoppa en topologi använder den __Kill__ längst upp i den __Topology Summary__.</span><span class="sxs-lookup"><span data-stu-id="d6260-173">To stop a topology, use the __Kill__ button at the top of the __Topology Summary__.</span></span>

## <a name="monitor-and-manage-ssh-and-the-storm-command"></a><span data-ttu-id="d6260-174">Övervaka och hantera: SSH och Storm-kommando</span><span class="sxs-lookup"><span data-stu-id="d6260-174">Monitor and manage: SSH and the Storm command</span></span>

<span data-ttu-id="d6260-175">Den `storm` verktyget kan du arbeta med topologier som körs från kommandoraden.</span><span class="sxs-lookup"><span data-stu-id="d6260-175">The `storm` utility allows you to work with running topologies from the command line.</span></span> <span data-ttu-id="d6260-176">Använd `storm -h` för en fullständig lista över kommandon.</span><span class="sxs-lookup"><span data-stu-id="d6260-176">Use `storm -h` for a full list of commands.</span></span>

### <a name="list-topologies"></a><span data-ttu-id="d6260-177">Lista över topologier</span><span class="sxs-lookup"><span data-stu-id="d6260-177">List topologies</span></span>

<span data-ttu-id="d6260-178">Använd följande kommando för att lista alla topologier som körs:</span><span class="sxs-lookup"><span data-stu-id="d6260-178">Use the following command to list all running topologies:</span></span>

    storm list

<span data-ttu-id="d6260-179">Det här kommandot returnerar information liknande följande text:</span><span class="sxs-lookup"><span data-stu-id="d6260-179">This command returns information similar to the following text:</span></span>

    Topology_name        Status     Num_tasks  Num_workers  Uptime_secs
    -------------------------------------------------------------------
    WordCount            ACTIVE     29         2            263

### <a name="deactivate-and-reactivate"></a><span data-ttu-id="d6260-180">Inaktivera och återaktivera</span><span class="sxs-lookup"><span data-stu-id="d6260-180">Deactivate and reactivate</span></span>

<span data-ttu-id="d6260-181">Inaktivera en topologi pausar den förrän den har avslutats eller återaktivera.</span><span class="sxs-lookup"><span data-stu-id="d6260-181">Deactivating a topology pauses it until it is killed or reactivated.</span></span> <span data-ttu-id="d6260-182">Använd följande kommando för att inaktivera och återaktivera:</span><span class="sxs-lookup"><span data-stu-id="d6260-182">Use the following command to deactivate and reactivate:</span></span>

    storm Deactivate TOPOLOGYNAME

    storm Activate TOPOLOGYNAME

### <a name="kill-a-running-topology"></a><span data-ttu-id="d6260-183">Avsluta en topologi som körs</span><span class="sxs-lookup"><span data-stu-id="d6260-183">Kill a running topology</span></span>

<span data-ttu-id="d6260-184">Storm-topologier, startas en gång, fortsätta köras tills stoppades.</span><span class="sxs-lookup"><span data-stu-id="d6260-184">Storm topologies, once started, continue running until stopped.</span></span> <span data-ttu-id="d6260-185">Om du vill stoppa en topologi, använder du följande kommando:</span><span class="sxs-lookup"><span data-stu-id="d6260-185">To stop a topology, use the following command:</span></span>

    storm kill TOPOLOGYNAME

### <a name="rebalance"></a><span data-ttu-id="d6260-186">Balansera</span><span class="sxs-lookup"><span data-stu-id="d6260-186">Rebalance</span></span>

<span data-ttu-id="d6260-187">Ombalansering en topologi kan ändra topologins parallellitet.</span><span class="sxs-lookup"><span data-stu-id="d6260-187">Rebalancing a topology allows the system to revise the parallelism of the topology.</span></span> <span data-ttu-id="d6260-188">Om du har ändrat storlek klustret för att lägga till anteckningar kan ombalansering exempelvis en topologi att se de nya noderna.</span><span class="sxs-lookup"><span data-stu-id="d6260-188">For example, if you have resized the cluster to add more notes, rebalancing allows a topology to see the new nodes.</span></span>

> [!WARNING]
> <span data-ttu-id="d6260-189">Ombalansering en topologi först inaktiverar topologi, sedan distribuerar arbetare jämnt över klustret och sedan returnerar slutligen topologin till tillståndet den var i innan ombalansering inträffade.</span><span class="sxs-lookup"><span data-stu-id="d6260-189">Rebalancing a topology first deactivates the topology, then redistributes workers evenly across the cluster, then finally returns the topology to the state it was in before rebalancing occurred.</span></span> <span data-ttu-id="d6260-190">Så om topologin var aktiv blir aktiva igen.</span><span class="sxs-lookup"><span data-stu-id="d6260-190">So if the topology was active, it becomes active again.</span></span> <span data-ttu-id="d6260-191">Om det har inaktiverats, förblir den inaktiverade.</span><span class="sxs-lookup"><span data-stu-id="d6260-191">If it was deactivated, it remains deactivated.</span></span>

    storm rebalance TOPOLOGYNAME

## <a name="monitor-and-manage-storm-ui"></a><span data-ttu-id="d6260-192">Övervaka och hantera: Storm UI</span><span class="sxs-lookup"><span data-stu-id="d6260-192">Monitor and manage: Storm UI</span></span>

<span data-ttu-id="d6260-193">Storm-användargränssnittet innehåller ett webbgränssnitt för att arbeta med topologier som körs och ingår i ditt HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="d6260-193">The Storm UI provides a web interface for working with running topologies, and is included on your HDInsight cluster.</span></span> <span data-ttu-id="d6260-194">Använda en webbläsare för att visa Användargränssnittet för Storm, öppna **https://CLUSTERNAME.azurehdinsight.net/stormui**, där **KLUSTERNAMN** är namnet på klustret.</span><span class="sxs-lookup"><span data-stu-id="d6260-194">To view the Storm UI, use a web browser to open **https://CLUSTERNAME.azurehdinsight.net/stormui**, where **CLUSTERNAME** is the name of your cluster.</span></span>

> [!NOTE]
> <span data-ttu-id="d6260-195">Om du uppmanas att ange ett användarnamn och lösenord, anger du klusteradministratören (admin) och lösenordet du använde när du skapade klustret.</span><span class="sxs-lookup"><span data-stu-id="d6260-195">If asked to provide a user name and password, enter the cluster administrator (admin) and password that you used when creating the cluster.</span></span>

### <a name="main-page"></a><span data-ttu-id="d6260-196">Huvudsida</span><span class="sxs-lookup"><span data-stu-id="d6260-196">Main page</span></span>

<span data-ttu-id="d6260-197">Huvudsidan för Storm-Användargränssnittet innehåller följande information:</span><span class="sxs-lookup"><span data-stu-id="d6260-197">The main page of the Storm UI provides the following information:</span></span>

* <span data-ttu-id="d6260-198">**Översikt över kluster**: grundläggande information om Storm-kluster.</span><span class="sxs-lookup"><span data-stu-id="d6260-198">**Cluster summary**: Basic information about the Storm cluster.</span></span>
* <span data-ttu-id="d6260-199">**Översikt över topologi**: en lista över topologier som körs.</span><span class="sxs-lookup"><span data-stu-id="d6260-199">**Topology summary**: A list of running topologies.</span></span> <span data-ttu-id="d6260-200">Använd länkarna i det här avsnittet om du vill visa mer information om specifika topologier.</span><span class="sxs-lookup"><span data-stu-id="d6260-200">Use the links in this section to view more information about specific topologies.</span></span>
* <span data-ttu-id="d6260-201">**Översikt över handledarens**: Information om Storm-administratören.</span><span class="sxs-lookup"><span data-stu-id="d6260-201">**Supervisor summary**: Information about the Storm supervisor.</span></span>
* <span data-ttu-id="d6260-202">**Nimbus configuration**: Nimbus-konfiguration för klustret.</span><span class="sxs-lookup"><span data-stu-id="d6260-202">**Nimbus configuration**: Nimbus configuration for the cluster.</span></span>

### <a name="topology-summary"></a><span data-ttu-id="d6260-203">Topologi sammanfattning</span><span class="sxs-lookup"><span data-stu-id="d6260-203">Topology summary</span></span>

<span data-ttu-id="d6260-204">Att välja en länk från den **Topology summary** avsnitt visar följande information om topologin:</span><span class="sxs-lookup"><span data-stu-id="d6260-204">Selecting a link from the **Topology summary** section displays the following information about the topology:</span></span>

* <span data-ttu-id="d6260-205">**Översikt över topologi**: grundläggande information om topologin.</span><span class="sxs-lookup"><span data-stu-id="d6260-205">**Topology summary**: Basic information about the topology.</span></span>
* <span data-ttu-id="d6260-206">**Topologi åtgärder**: hanteringsåtgärder som du kan utföra för topologin.</span><span class="sxs-lookup"><span data-stu-id="d6260-206">**Topology actions**: Management actions that you can perform for the topology.</span></span>

  * <span data-ttu-id="d6260-207">**Aktivera**: återupptar bearbetningen av en inaktiverad topologi.</span><span class="sxs-lookup"><span data-stu-id="d6260-207">**Activate**: Resumes processing of a deactivated topology.</span></span>
  * <span data-ttu-id="d6260-208">**Inaktivera**: pausar en topologi som körs.</span><span class="sxs-lookup"><span data-stu-id="d6260-208">**Deactivate**: Pauses a running topology.</span></span>
  * <span data-ttu-id="d6260-209">**Balansera**: justerar topologins parallellitet.</span><span class="sxs-lookup"><span data-stu-id="d6260-209">**Rebalance**: Adjusts the parallelism of the topology.</span></span> <span data-ttu-id="d6260-210">Du bör balansera om topologier som körs när du har ändrat antalet noder i klustret.</span><span class="sxs-lookup"><span data-stu-id="d6260-210">You should rebalance running topologies after you have changed the number of nodes in the cluster.</span></span> <span data-ttu-id="d6260-211">Den här åtgärden gör att topologin kan justera parallelliteten och kompensera för ökar eller minskar antalet noder i klustret.</span><span class="sxs-lookup"><span data-stu-id="d6260-211">This operation allows the topology to adjust parallelism to compensate for the increased or decreased number of nodes in the cluster.</span></span>

    <span data-ttu-id="d6260-212">Mer information finns i <a href="http://storm.apache.org/documentation/Understanding-the-parallelism-of-a-Storm-topology.html" target="_blank">Förstå parallellitet i en Storm-topologi</a>.</span><span class="sxs-lookup"><span data-stu-id="d6260-212">For more information, see <a href="http://storm.apache.org/documentation/Understanding-the-parallelism-of-a-Storm-topology.html" target="_blank">Understanding the parallelism of a Storm topology</a>.</span></span>
  * <span data-ttu-id="d6260-213">**Avsluta**: avslutar en Storm-topologi efter en angiven tidsgräns.</span><span class="sxs-lookup"><span data-stu-id="d6260-213">**Kill**: Terminates a Storm topology after the specified timeout.</span></span>
* <span data-ttu-id="d6260-214">**Topology stats**: statistik om topologin.</span><span class="sxs-lookup"><span data-stu-id="d6260-214">**Topology stats**: Statistics about the topology.</span></span> <span data-ttu-id="d6260-215">Om du vill ange tidsperioden för de återstående posterna på sidan i länkarna i den **fönstret** kolumn.</span><span class="sxs-lookup"><span data-stu-id="d6260-215">To set the timeframe for the remaining entries on the page, use the links in the **Window** column.</span></span>
* <span data-ttu-id="d6260-216">**Spouts**: kanaler som används av topologin.</span><span class="sxs-lookup"><span data-stu-id="d6260-216">**Spouts**: The spouts used by the topology.</span></span> <span data-ttu-id="d6260-217">Använd länkarna i det här avsnittet om du vill visa mer information om specifika kanaler.</span><span class="sxs-lookup"><span data-stu-id="d6260-217">Use the links in this section to view more information about specific spouts.</span></span>
* <span data-ttu-id="d6260-218">**Bolts**: bultar som används av topologin.</span><span class="sxs-lookup"><span data-stu-id="d6260-218">**Bolts**: The bolts used by the topology.</span></span> <span data-ttu-id="d6260-219">Använd länkarna i det här avsnittet om du vill visa mer information om specifika bultar.</span><span class="sxs-lookup"><span data-stu-id="d6260-219">Use the links in this section to view more information about specific bolts.</span></span>
* <span data-ttu-id="d6260-220">**Topologi configuration**: konfigurationen av den valda topologin.</span><span class="sxs-lookup"><span data-stu-id="d6260-220">**Topology configuration**: The configuration of the selected topology.</span></span>

### <a name="spout-and-bolt-summary"></a><span data-ttu-id="d6260-221">Kanal och bult sammanfattning</span><span class="sxs-lookup"><span data-stu-id="d6260-221">Spout and Bolt summary</span></span>

<span data-ttu-id="d6260-222">Att välja en kanal från den **Spouts** eller **Bolts** avsnitt visar följande information om det markerade objektet:</span><span class="sxs-lookup"><span data-stu-id="d6260-222">Selecting a spout from the **Spouts** or **Bolts** sections displays the following information about the selected item:</span></span>

* <span data-ttu-id="d6260-223">**Sammanfattning**: grundläggande information om den kanal eller en bult.</span><span class="sxs-lookup"><span data-stu-id="d6260-223">**Component summary**: Basic information about the spout or bolt.</span></span>
* <span data-ttu-id="d6260-224">**Kanal/bult stats**: statistik om den kanal eller en bult.</span><span class="sxs-lookup"><span data-stu-id="d6260-224">**Spout/Bolt stats**: Statistics about the spout or bolt.</span></span> <span data-ttu-id="d6260-225">Om du vill ange tidsperioden för de återstående posterna på sidan i länkarna i den **fönstret** kolumn.</span><span class="sxs-lookup"><span data-stu-id="d6260-225">To set the timeframe for the remaining entries on the page, use the links in the **Window** column.</span></span>
* <span data-ttu-id="d6260-226">**Input stats** (endast bultar): Information om de inkommande dataströmmar som används av bulten.</span><span class="sxs-lookup"><span data-stu-id="d6260-226">**Input stats** (bolt only): Information about the input streams consumed by the bolt.</span></span>
* <span data-ttu-id="d6260-227">**Utdata stats**: Information om strömmar orsakat av detta kanal eller bultar.</span><span class="sxs-lookup"><span data-stu-id="d6260-227">**Output stats**: Information about the streams emitted by this spout or bolt.</span></span>
* <span data-ttu-id="d6260-228">**Executors**: Information om instanser av den kanal eller en bult.</span><span class="sxs-lookup"><span data-stu-id="d6260-228">**Executors**: Information about the instances of the spout or bolt.</span></span> <span data-ttu-id="d6260-229">Välj den **Port** post för en specifik utförare att visa en logg över diagnostikinformation produceras för den här instansen.</span><span class="sxs-lookup"><span data-stu-id="d6260-229">Select the **Port** entry for a specific executor to view a log of diagnostic information produced for this instance.</span></span>
* <span data-ttu-id="d6260-230">**Fel**: felinformation för den här kanalen eller bultar.</span><span class="sxs-lookup"><span data-stu-id="d6260-230">**Errors**: Any error information for this spout or bolt.</span></span>

## <a name="monitor-and-manage-rest-api"></a><span data-ttu-id="d6260-231">Övervaka och hantera: REST API</span><span class="sxs-lookup"><span data-stu-id="d6260-231">Monitor and manage: REST API</span></span>

<span data-ttu-id="d6260-232">Storm-Användargränssnittet är byggt på REST-API, så att du kan göra liknande hantering och övervakning av funktioner med hjälp av REST API.</span><span class="sxs-lookup"><span data-stu-id="d6260-232">The Storm UI is built on top of the REST API, so you can perform similar management and monitoring functionality by using the REST API.</span></span> <span data-ttu-id="d6260-233">Du kan använda REST API för att skapa anpassade verktyg för att hantera och övervaka Storm-topologier.</span><span class="sxs-lookup"><span data-stu-id="d6260-233">You can use the REST API to create custom tools for managing and monitoring Storm topologies.</span></span>

<span data-ttu-id="d6260-234">Mer information finns i [Storm UI REST API](http://storm.apache.org/releases/0.9.6/STORM-UI-REST-API.html).</span><span class="sxs-lookup"><span data-stu-id="d6260-234">For more information, see [Storm UI REST API](http://storm.apache.org/releases/0.9.6/STORM-UI-REST-API.html).</span></span> <span data-ttu-id="d6260-235">Följande information gäller med hjälp av REST-API med Apache Storm på HDInsight.</span><span class="sxs-lookup"><span data-stu-id="d6260-235">The following information is specific to using the REST API with Apache Storm on HDInsight.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="d6260-236">Storm REST API är inte offentligt tillgängliga via internet och måste användas med en SSH-tunnel till HDInsight-klustrets huvudnod.</span><span class="sxs-lookup"><span data-stu-id="d6260-236">The Storm REST API is not publicly available over the internet, and must be accessed using an SSH tunnel to the HDInsight cluster head node.</span></span> <span data-ttu-id="d6260-237">Mer information om hur du skapar och använder en SSH-tunnel finns [Använd SSH-tunnlar för att komma åt Ambari-webbgränssnittet, resurshanteraren, jobbhistorik, NameNode, Oozie och andra webb-gränssnittet för](hdinsight-linux-ambari-ssh-tunnel.md).</span><span class="sxs-lookup"><span data-stu-id="d6260-237">For information on creating and using an SSH tunnel, see [Use SSH Tunneling to access Ambari web UI, ResourceManager, JobHistory, NameNode, Oozie, and other web UIs](hdinsight-linux-ambari-ssh-tunnel.md).</span></span>

### <a name="base-uri"></a><span data-ttu-id="d6260-238">Bas-URI</span><span class="sxs-lookup"><span data-stu-id="d6260-238">Base URI</span></span>

<span data-ttu-id="d6260-239">Bas-URI för REST-API på Linux-baserade HDInsight-kluster finns i huvudnod vid **v1-https://HEADNODEFQDN:8744/api/**.</span><span class="sxs-lookup"><span data-stu-id="d6260-239">The base URI for the REST API on Linux-based HDInsight clusters is available on the head node at **https://HEADNODEFQDN:8744/api/v1/**.</span></span> <span data-ttu-id="d6260-240">Domännamnet för huvudnoden genereras när klustret skapas och är inte statisk.</span><span class="sxs-lookup"><span data-stu-id="d6260-240">The domain name of the head node is generated during cluster creation and is not static.</span></span>

<span data-ttu-id="d6260-241">Du hittar det fullständigt kvalificerade domännamnet (FQDN) för klustrets huvudnod på flera olika sätt:</span><span class="sxs-lookup"><span data-stu-id="d6260-241">You can find the fully qualified domain name (FQDN) for the cluster head node in several different ways:</span></span>

* <span data-ttu-id="d6260-242">**Från en SSH-session**: Använd kommandot `headnode -f` från en SSH-session till klustret.</span><span class="sxs-lookup"><span data-stu-id="d6260-242">**From an SSH session**: Use the command `headnode -f` from an SSH session to the cluster.</span></span>
* <span data-ttu-id="d6260-243">**Ambari Web**: Välj **Services** högst upp på sidan Välj **Storm**.</span><span class="sxs-lookup"><span data-stu-id="d6260-243">**From Ambari Web**: Select **Services** from the top of the page, then select **Storm**.</span></span> <span data-ttu-id="d6260-244">Från den **sammanfattning** väljer **Storm UI Server**.</span><span class="sxs-lookup"><span data-stu-id="d6260-244">From the **Summary** tab, select **Storm UI Server**.</span></span> <span data-ttu-id="d6260-245">FQDN för den nod som kör Storm-Användargränssnittet och REST API är överst på sidan.</span><span class="sxs-lookup"><span data-stu-id="d6260-245">The FQDN of the node that the Storm UI and REST API is running is at the top of the page.</span></span>
* <span data-ttu-id="d6260-246">**Från Ambari REST API**: Använd kommandot `curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/STORM/components/STORM_UI_SERVER"` att hämta information om den nod som Storm-Användargränssnittet och REST API körs på.</span><span class="sxs-lookup"><span data-stu-id="d6260-246">**From Ambari REST API**: Use the command `curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/STORM/components/STORM_UI_SERVER"` to retrieve information about the node that the Storm UI and REST API are running on.</span></span> <span data-ttu-id="d6260-247">Ersätt **lösenord** med administratörslösenordet för klustret.</span><span class="sxs-lookup"><span data-stu-id="d6260-247">Replace **PASSWORD** with the admin password for the cluster.</span></span> <span data-ttu-id="d6260-248">Ersätt **KLUSTERNAMN** med klustrets namn.</span><span class="sxs-lookup"><span data-stu-id="d6260-248">Replace **CLUSTERNAME** with the cluster name.</span></span> <span data-ttu-id="d6260-249">I svaret innehåller posten ”värddatornamn” FQDN för noden.</span><span class="sxs-lookup"><span data-stu-id="d6260-249">In the response, the "host_name" entry contains the FQDN of the node.</span></span>

### <a name="authentication"></a><span data-ttu-id="d6260-250">Autentisering</span><span class="sxs-lookup"><span data-stu-id="d6260-250">Authentication</span></span>

<span data-ttu-id="d6260-251">REST API-begäranden måste använda **grundläggande autentisering**, så att du använder HDInsight-klustrets administratörsnamn och lösenord.</span><span class="sxs-lookup"><span data-stu-id="d6260-251">Requests to the REST API must use **basic authentication**, so you use the HDInsight cluster administrator name and password.</span></span>

> [!NOTE]
> <span data-ttu-id="d6260-252">Eftersom grundläggande autentisering skickas i klartext, bör du **alltid** använder HTTPS för att skydda kommunikationen med klustret.</span><span class="sxs-lookup"><span data-stu-id="d6260-252">Because basic authentication is sent by using clear text, you should **always** use HTTPS to secure communications with the cluster.</span></span>

### <a name="return-values"></a><span data-ttu-id="d6260-253">Returnera värden</span><span class="sxs-lookup"><span data-stu-id="d6260-253">Return values</span></span>

<span data-ttu-id="d6260-254">Information som returneras från REST-API kan endast användas från i kluster eller virtuella datorer i samma virtuella nätverk med Azure som klustret.</span><span class="sxs-lookup"><span data-stu-id="d6260-254">Information that is returned from the REST API may only be usable from within the cluster or virtual machines on the same Azure Virtual Network as the cluster.</span></span> <span data-ttu-id="d6260-255">Till exempel namnet fullständigt kvalificerade domännamnet (FQDN) returnerade Zookeeper-servrar är inte tillgänglig från Internet.</span><span class="sxs-lookup"><span data-stu-id="d6260-255">For example, the fully qualified domain name (FQDN) returned for Zookeeper servers is not be accessible from the Internet.</span></span>

## <a name="next-steps"></a><span data-ttu-id="d6260-256">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="d6260-256">Next Steps</span></span>

<span data-ttu-id="d6260-257">Nu när du har lärt dig hur du distribuerar och övervakar topologier med hjälp av Storm-instrumentpanelen, Läs mer om hur du [utveckla Java-baserade topologier med Maven](hdinsight-storm-develop-java-topology.md).</span><span class="sxs-lookup"><span data-stu-id="d6260-257">Now that you've learned how to deploy and monitor topologies by using the Storm Dashboard, learn how to [Develop Java-based topologies using Maven](hdinsight-storm-develop-java-topology.md).</span></span>

<span data-ttu-id="d6260-258">En lista över flera exempeltopologier finns [exempeltopologier för Storm på HDInsight](hdinsight-storm-example-topology.md).</span><span class="sxs-lookup"><span data-stu-id="d6260-258">For a list of more example topologies, see [Example topologies for Storm on HDInsight](hdinsight-storm-example-topology.md).</span></span>
