---
title: "aaaDeploy och hantera Apache Storm-topologier på Linux-baserade HDInsight | Microsoft Docs"
description: "Lär dig hur toodeploy, övervaka och hantera Apache Storm-topologier med hello Storm-instrumentpanelen på Linux-baserade HDInsight. Använda Hadoop-verktyg för Visual Studio."
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
ms.openlocfilehash: 3a1edb773089cc596fea423710aa88cf83c7b841
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-and-manage-apache-storm-topologies-on-hdinsight"></a><span data-ttu-id="4b37b-104">Distribuera och hantera Apache Storm-topologier på HDInsight</span><span class="sxs-lookup"><span data-stu-id="4b37b-104">Deploy and manage Apache Storm topologies on HDInsight</span></span>

<span data-ttu-id="4b37b-105">I detta dokument, lär du dig hello grunderna för att hantera och övervaka Storm-topologier som körs på Storm på HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="4b37b-105">In this document, learn hello basics of managing and monitoring Storm topologies running on Storm on HDInsight clusters.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="4b37b-106">hello krävs steg i den här artikeln ett Linux-baserade Storm på HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="4b37b-106">hello steps in this article require a Linux-based Storm on HDInsight cluster.</span></span> <span data-ttu-id="4b37b-107">Linux är hello endast operativsystem på HDInsight version 3.4 eller senare.</span><span class="sxs-lookup"><span data-stu-id="4b37b-107">Linux is hello only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="4b37b-108">Mer information finns i [HDInsight-avveckling på Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="4b37b-108">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span> 
>
> <span data-ttu-id="4b37b-109">Mer information om distribuera och övervaka topologier på Windows-baserade HDInsight finns [distribuera och hantera Apache Storm-topologier på Windows-baserade HDInsight](hdinsight-storm-deploy-monitor-topology.md)</span><span class="sxs-lookup"><span data-stu-id="4b37b-109">For information on deploying and monitoring topologies on Windows-based HDInsight, see [Deploy and manage Apache Storm topologies on Windows-based HDInsight](hdinsight-storm-deploy-monitor-topology.md)</span></span>


## <a name="prerequisites"></a><span data-ttu-id="4b37b-110">Krav</span><span class="sxs-lookup"><span data-stu-id="4b37b-110">Prerequisites</span></span>

* <span data-ttu-id="4b37b-111">**En Linux-baserade Storm på HDInsight-kluster**: se [Kom igång med Apache Storm på HDInsight](hdinsight-apache-storm-tutorial-get-started-linux.md) anvisningar om hur du skapar ett kluster</span><span class="sxs-lookup"><span data-stu-id="4b37b-111">**A Linux-based Storm on HDInsight cluster**: see [Get started with Apache Storm on HDInsight](hdinsight-apache-storm-tutorial-get-started-linux.md) for steps on creating a cluster</span></span>

* <span data-ttu-id="4b37b-112">(Valfritt) **Kunskap om SSH och SCP**: Mer information finns i [använda SSH med HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="4b37b-112">(Optional) **Familiarity with SSH and SCP**: For more information, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

* <span data-ttu-id="4b37b-113">(Valfritt) **Visual Studio**: Azure SDK 2.5.1 eller senare och hello Data Lake-verktyg för Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="4b37b-113">(Optional) **Visual Studio**: Azure SDK 2.5.1 or newer and hello Data Lake Tools for Visual Studio.</span></span> <span data-ttu-id="4b37b-114">Mer information finns i [komma igång med Data Lake-verktyg för Visual Studio](hdinsight-hadoop-visual-studio-tools-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="4b37b-114">For more information, see [Get started using Data Lake Tools for Visual Studio](hdinsight-hadoop-visual-studio-tools-get-started.md).</span></span>

    <span data-ttu-id="4b37b-115">En av följande versioner av Visual Studio hello:</span><span class="sxs-lookup"><span data-stu-id="4b37b-115">One of hello following versions of Visual Studio:</span></span>

  * <span data-ttu-id="4b37b-116">Visual Studio 2012 med [Update 4](http://www.microsoft.com/download/details.aspx?id=39305)</span><span class="sxs-lookup"><span data-stu-id="4b37b-116">Visual Studio 2012 with [Update 4](http://www.microsoft.com/download/details.aspx?id=39305)</span></span>

  * <span data-ttu-id="4b37b-117">Visual Studio 2013 med [uppdatering 4](http://www.microsoft.com/download/details.aspx?id=44921) eller [Visual Studio Community 2013](http://go.microsoft.com/fwlink/?LinkId=517284)</span><span class="sxs-lookup"><span data-stu-id="4b37b-117">Visual Studio 2013 with [Update 4](http://www.microsoft.com/download/details.aspx?id=44921) or [Visual Studio 2013 Community](http://go.microsoft.com/fwlink/?LinkId=517284)</span></span>
  * [<span data-ttu-id="4b37b-118">Visual Studio 2015</span><span class="sxs-lookup"><span data-stu-id="4b37b-118">Visual Studio 2015</span></span>](https://www.visualstudio.com/downloads/)

  * <span data-ttu-id="4b37b-119">Visual Studio 2015 (någon utgåva)</span><span class="sxs-lookup"><span data-stu-id="4b37b-119">Visual Studio 2015 (any edition)</span></span>

  * <span data-ttu-id="4b37b-120">2017 för Visual Studio (någon utgåva).</span><span class="sxs-lookup"><span data-stu-id="4b37b-120">Visual Studio 2017 (any edition).</span></span> <span data-ttu-id="4b37b-121">Data Lake-verktyg för Visual Studio 2017 installeras som en del av hello Azure arbetsbelastning.</span><span class="sxs-lookup"><span data-stu-id="4b37b-121">Data Lake Tools for Visual Studio 2017 are installed as part of hello Azure Workload.</span></span>

## <a name="submit-a-topology-visual-studio"></a><span data-ttu-id="4b37b-122">Skicka en topologi: Visual Studio</span><span class="sxs-lookup"><span data-stu-id="4b37b-122">Submit a topology: Visual Studio</span></span>

<span data-ttu-id="4b37b-123">Hej HDInsight-verktyg kan vara används toosubmit eller hybrid C#-topologier tooyour Storm-kluster.</span><span class="sxs-lookup"><span data-stu-id="4b37b-123">hello HDInsight Tools can be used toosubmit C# or hybrid topologies tooyour Storm cluster.</span></span> <span data-ttu-id="4b37b-124">hello följande använder ett exempelprogram.</span><span class="sxs-lookup"><span data-stu-id="4b37b-124">hello following steps use a sample application.</span></span> <span data-ttu-id="4b37b-125">Information om hur du skapar egna topologier med hjälp av hello HDInsight Tools finns [utveckla C#-topologier med hello HDInsight Tools för Visual Studio](hdinsight-storm-develop-csharp-visual-studio-topology.md).</span><span class="sxs-lookup"><span data-stu-id="4b37b-125">For information about creating your own topologies by using hello HDInsight Tools, see [Develop C# topologies using hello HDInsight Tools for Visual Studio](hdinsight-storm-develop-csharp-visual-studio-topology.md).</span></span>

1. <span data-ttu-id="4b37b-126">Om du inte redan har installerat hello senaste versionen av hello Data Lake-verktyg för Visual Studio finns [komma igång med Data Lake-verktyg för Visual Studio](hdinsight-hadoop-visual-studio-tools-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="4b37b-126">If you have not already installed hello latest version of hello Data Lake tools for Visual Studio, see [Get started using Data Lake Tools for Visual Studio](hdinsight-hadoop-visual-studio-tools-get-started.md).</span></span>

    > [!NOTE]
    > <span data-ttu-id="4b37b-127">hello Data Lake-verktyg för Visual Studio kallades hello HDInsight Tools för Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="4b37b-127">hello Data Lake Tools for Visual Studio were formerly called hello HDInsight Tools for Visual Studio.</span></span>
    >
    > <span data-ttu-id="4b37b-128">Data Lake-verktyg för Visual Studio ingår i hello __Azure arbetsbelastning__ för Visual Studio-2017.</span><span class="sxs-lookup"><span data-stu-id="4b37b-128">Data Lake Tools for Visual Studio are included in hello __Azure Workload__ for Visual Studio 2017.</span></span>

2. <span data-ttu-id="4b37b-129">Öppna Visual Studio, markera **filen** > **ny** > **projekt**.</span><span class="sxs-lookup"><span data-stu-id="4b37b-129">Open Visual Studio, select **File** > **New** > **Project**.</span></span>

3. <span data-ttu-id="4b37b-130">I hello **nytt projekt** dialogrutan Expandera **installerad** > **mallar**, och välj sedan **HDInsight**.</span><span class="sxs-lookup"><span data-stu-id="4b37b-130">In hello **New Project** dialog box, expand **Installed** > **Templates**, and then select **HDInsight**.</span></span> <span data-ttu-id="4b37b-131">Välj hello listan över mallar **Storm exempel**.</span><span class="sxs-lookup"><span data-stu-id="4b37b-131">From hello list of templates, select **Storm Sample**.</span></span> <span data-ttu-id="4b37b-132">Ange ett namn för programmet hello hello längst ned i dialogrutan för hello i.</span><span class="sxs-lookup"><span data-stu-id="4b37b-132">At hello bottom of hello dialog box, type a name for hello application.</span></span>

    ![Bild](./media/hdinsight-storm-deploy-monitor-topology-linux/sample.png)

4. <span data-ttu-id="4b37b-134">I **Solution Explorer**, högerklicka på hello-projektet och välj **skicka tooStorm på HDInsight**.</span><span class="sxs-lookup"><span data-stu-id="4b37b-134">In **Solution Explorer**, right-click hello project, and select **Submit tooStorm on HDInsight**.</span></span>

   > [!NOTE]
   > <span data-ttu-id="4b37b-135">Om du uppmanas ange hello inloggningsuppgifterna för din Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="4b37b-135">If prompted, enter hello login credentials for your Azure subscription.</span></span> <span data-ttu-id="4b37b-136">Om du har mer än en prenumeration kan du logga in toohello en som innehåller ditt Storm på HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="4b37b-136">If you have more than one subscription, log in toohello one that contains your Storm on HDInsight cluster.</span></span>

5. <span data-ttu-id="4b37b-137">Välj ditt Storm på HDInsight-kluster från hello **Storm-kluster** listrutan och välj sedan **skicka**.</span><span class="sxs-lookup"><span data-stu-id="4b37b-137">Select your Storm on HDInsight cluster from hello **Storm Cluster** drop-down list, and then select **Submit**.</span></span> <span data-ttu-id="4b37b-138">Du kan övervaka om hello ansökan har genomförts med hello **utdata** fönster.</span><span class="sxs-lookup"><span data-stu-id="4b37b-138">You can monitor whether hello submission is successful by using hello **Output** window.</span></span>

## <a name="submit-a-topology-ssh-and-hello-storm-command"></a><span data-ttu-id="4b37b-139">Skicka en topologi: SSH och hello Storm-kommando</span><span class="sxs-lookup"><span data-stu-id="4b37b-139">Submit a topology: SSH and hello Storm command</span></span>

1. <span data-ttu-id="4b37b-140">Använda SSH tooconnect toohello HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="4b37b-140">Use SSH tooconnect toohello HDInsight cluster.</span></span> <span data-ttu-id="4b37b-141">Ersätt **användarnamn** hello namnet på SSH-inloggning.</span><span class="sxs-lookup"><span data-stu-id="4b37b-141">Replace **USERNAME** hello name of your SSH login.</span></span> <span data-ttu-id="4b37b-142">Ersätt **KLUSTERNAMN** med ditt HDInsight-klustrets namn:</span><span class="sxs-lookup"><span data-stu-id="4b37b-142">Replace **CLUSTERNAME** with your HDInsight cluster name:</span></span>

        ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net

    <span data-ttu-id="4b37b-143">Mer information om hur du använder SSH tooconnect tooyour HDInsight-kluster, se [använda SSH med HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="4b37b-143">For more information on using SSH tooconnect tooyour HDInsight cluster, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

2. <span data-ttu-id="4b37b-144">Använd följande kommando toostart en exempeltopologi hello:</span><span class="sxs-lookup"><span data-stu-id="4b37b-144">Use hello following command toostart an example topology:</span></span>

        storm jar /usr/hdp/current/storm-client/contrib/storm-starter/storm-starter-topologies-*.jar org.apache.storm.starter.WordCountTopology WordCount

    <span data-ttu-id="4b37b-145">Detta kommando startar hello exempel WordCount-topologi på hello klustret.</span><span class="sxs-lookup"><span data-stu-id="4b37b-145">This command starts hello example WordCount topology on hello cluster.</span></span> <span data-ttu-id="4b37b-146">Den här topologin generera slumpmässigt meningar och antal hello förekomsten av varje ord i hello meningar.</span><span class="sxs-lookup"><span data-stu-id="4b37b-146">This topology randomly generate sentences and count hello occurrence of each word in hello sentences.</span></span>

   > [!NOTE]
   > <span data-ttu-id="4b37b-147">När du skickar in topologin toohello klustret måste du först kopiera hello jar-filen som innehåller hello klustret innan du använder hello `storm` kommando.</span><span class="sxs-lookup"><span data-stu-id="4b37b-147">When submitting topology toohello cluster, you must first copy hello jar file containing hello cluster before using hello `storm` command.</span></span> <span data-ttu-id="4b37b-148">toocopy hello toohello filkluster, du kan använda hello `scp` kommando.</span><span class="sxs-lookup"><span data-stu-id="4b37b-148">toocopy hello file toohello cluster, you can use hello `scp` command.</span></span> <span data-ttu-id="4b37b-149">Till exempel, `scp FILENAME.jar USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:FILENAME.jar`</span><span class="sxs-lookup"><span data-stu-id="4b37b-149">For example, `scp FILENAME.jar USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:FILENAME.jar`</span></span>
   >
   > <span data-ttu-id="4b37b-150">Hej WordCount-exemplet och andra i storm Starter ingår redan i klustret på `/usr/hdp/current/storm-client/contrib/storm-starter/`.</span><span class="sxs-lookup"><span data-stu-id="4b37b-150">hello WordCount example, and other storm starter examples, are already included on your cluster at `/usr/hdp/current/storm-client/contrib/storm-starter/`.</span></span>

## <a name="submit-a-topology-programmatically"></a><span data-ttu-id="4b37b-151">Skicka en topologi: programmässigt</span><span class="sxs-lookup"><span data-stu-id="4b37b-151">Submit a topology: programmatically</span></span>

<span data-ttu-id="4b37b-152">Du kan distribuera en topologi tooStorm på HDInsight via programmering genom att kommunicera med hello Nimbus-tjänst som finns i klustret.</span><span class="sxs-lookup"><span data-stu-id="4b37b-152">You can programmatically deploy a topology tooStorm on HDInsight by communicating with hello Nimbus service hosted in your cluster.</span></span> <span data-ttu-id="4b37b-153">[https://github.com/Azure-Samples/hdinsight-Java-Deploy-storm-topology](https://github.com/Azure-Samples/hdinsight-java-deploy-storm-topology) innehåller ett exempel på Java-program som visar hur toodeploy och starta en topologi via hello Nimbus-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="4b37b-153">[https://github.com/Azure-Samples/hdinsight-java-deploy-storm-topology](https://github.com/Azure-Samples/hdinsight-java-deploy-storm-topology) provides an example Java application that demonstrates how toodeploy and start a topology through hello Nimbus service.</span></span>

## <a name="monitor-and-manage-visual-studio"></a><span data-ttu-id="4b37b-154">Övervaka och hantera: Visual Studio</span><span class="sxs-lookup"><span data-stu-id="4b37b-154">Monitor and manage: Visual Studio</span></span>

<span data-ttu-id="4b37b-155">När en topologi har skickats med Visual Studio, hello **Storm-topologier** visa för hello kluster visas.</span><span class="sxs-lookup"><span data-stu-id="4b37b-155">When a topology has been successfully submitted using Visual Studio, hello **Storm Topologies** view for hello cluster appears.</span></span> <span data-ttu-id="4b37b-156">Välj hello topologi hello listan tooview information om hello kör topologi.</span><span class="sxs-lookup"><span data-stu-id="4b37b-156">Select hello topology from hello list tooview information about hello running topology.</span></span>

![Övervakare för Visual studio](./media/hdinsight-storm-deploy-monitor-topology/vsmonitor.png)

> [!NOTE]
> <span data-ttu-id="4b37b-158">Du kan också visa **Storm-topologier** från **Server Explorer** genom att expandera **Azure** > **HDInsight**, och sedan Högerklicka på ett Storm på HDInsight-kluster och välja **visa Storm-topologier**.</span><span class="sxs-lookup"><span data-stu-id="4b37b-158">You can also view **Storm Topologies** from **Server Explorer** by expanding **Azure** > **HDInsight**, and then right-clicking a Storm on HDInsight cluster, and selecting **View Storm Topologies**.</span></span>

<span data-ttu-id="4b37b-159">Markera hello form för hello spouts eller bolts tooview information om dessa komponenter.</span><span class="sxs-lookup"><span data-stu-id="4b37b-159">Select hello shape for hello spouts or bolts tooview information about these components.</span></span> <span data-ttu-id="4b37b-160">För varje vald öppnas ett nytt fönster.</span><span class="sxs-lookup"><span data-stu-id="4b37b-160">A new window opens for each item selected.</span></span>

### <a name="deactivate-and-reactivate"></a><span data-ttu-id="4b37b-161">Inaktivera och återaktivera</span><span class="sxs-lookup"><span data-stu-id="4b37b-161">Deactivate and reactivate</span></span>

<span data-ttu-id="4b37b-162">Inaktivera en topologi pausar den förrän den har avslutats eller återaktivera.</span><span class="sxs-lookup"><span data-stu-id="4b37b-162">Deactivating a topology pauses it until it is killed or reactivated.</span></span> <span data-ttu-id="4b37b-163">tooperform dessa åtgärder använder hello __inaktivera__ och __återaktivera__ knappar hello överst i hello __Topology Summary__.</span><span class="sxs-lookup"><span data-stu-id="4b37b-163">tooperform these operations, use hello __Deactivate__ and __Reactivate__ buttons at hello top of hello __Topology Summary__.</span></span>

### <a name="rebalance"></a><span data-ttu-id="4b37b-164">Balansera</span><span class="sxs-lookup"><span data-stu-id="4b37b-164">Rebalance</span></span>

<span data-ttu-id="4b37b-165">Ombalansering en topologi kan hello system toorevise hello parallellitet hello-topologi.</span><span class="sxs-lookup"><span data-stu-id="4b37b-165">Rebalancing a topology allows hello system toorevise hello parallelism of hello topology.</span></span> <span data-ttu-id="4b37b-166">Till exempel om du har ändrat storlek hello klustret tooadd anteckningar, kan ombalansering en topologi toosee hello nya noder.</span><span class="sxs-lookup"><span data-stu-id="4b37b-166">For example, if you have resized hello cluster tooadd more notes, rebalancing allows a topology toosee hello new nodes.</span></span>

<span data-ttu-id="4b37b-167">toorebalance en topologi använder hello __ombalansera__ knappen hello överst i hello __Topology Summary__.</span><span class="sxs-lookup"><span data-stu-id="4b37b-167">toorebalance a topology, use hello __Rebalance__ button at hello top of hello __Topology Summary__.</span></span>

> [!WARNING]
> <span data-ttu-id="4b37b-168">Ombalansering en topologi först inaktiverar hello topologi, och sedan distribuerar arbetare jämnt över hello kluster slutligen och returnerar sedan hello topologi toohello tillstånd innan ombalansering inträffade.</span><span class="sxs-lookup"><span data-stu-id="4b37b-168">Rebalancing a topology first deactivates hello topology, then redistributes workers evenly across hello cluster, then finally returns hello topology toohello state it was in before rebalancing occurred.</span></span> <span data-ttu-id="4b37b-169">Så om hello topologi var aktiv blir aktiva igen.</span><span class="sxs-lookup"><span data-stu-id="4b37b-169">So if hello topology was active, it becomes active again.</span></span> <span data-ttu-id="4b37b-170">Om det har inaktiverats, förblir den inaktiverade.</span><span class="sxs-lookup"><span data-stu-id="4b37b-170">If it was deactivated, it remains deactivated.</span></span>

### <a name="kill-a-topology"></a><span data-ttu-id="4b37b-171">Avsluta en topologi</span><span class="sxs-lookup"><span data-stu-id="4b37b-171">Kill a topology</span></span>

<span data-ttu-id="4b37b-172">Storm-topologier fortsätta köras förrän de har stoppats eller hello klustret tas bort.</span><span class="sxs-lookup"><span data-stu-id="4b37b-172">Storm topologies continue running until they are stopped or hello cluster is deleted.</span></span> <span data-ttu-id="4b37b-173">toostop en topologi använder hello __Kill__ knappen hello överst i hello __Topology Summary__.</span><span class="sxs-lookup"><span data-stu-id="4b37b-173">toostop a topology, use hello __Kill__ button at hello top of hello __Topology Summary__.</span></span>

## <a name="monitor-and-manage-ssh-and-hello-storm-command"></a><span data-ttu-id="4b37b-174">Övervaka och hantera: SSH och hello Storm-kommando</span><span class="sxs-lookup"><span data-stu-id="4b37b-174">Monitor and manage: SSH and hello Storm command</span></span>

<span data-ttu-id="4b37b-175">Hej `storm` verktyget kan du toowork med topologier som körs från hello-kommandoraden.</span><span class="sxs-lookup"><span data-stu-id="4b37b-175">hello `storm` utility allows you toowork with running topologies from hello command line.</span></span> <span data-ttu-id="4b37b-176">Använd `storm -h` för en fullständig lista över kommandon.</span><span class="sxs-lookup"><span data-stu-id="4b37b-176">Use `storm -h` for a full list of commands.</span></span>

### <a name="list-topologies"></a><span data-ttu-id="4b37b-177">Lista över topologier</span><span class="sxs-lookup"><span data-stu-id="4b37b-177">List topologies</span></span>

<span data-ttu-id="4b37b-178">Använd följande kommando toolist hello alla topologier som körs:</span><span class="sxs-lookup"><span data-stu-id="4b37b-178">Use hello following command toolist all running topologies:</span></span>

    storm list

<span data-ttu-id="4b37b-179">Det här kommandot returnerar informationen liknande toohello följande text:</span><span class="sxs-lookup"><span data-stu-id="4b37b-179">This command returns information similar toohello following text:</span></span>

    Topology_name        Status     Num_tasks  Num_workers  Uptime_secs
    -------------------------------------------------------------------
    WordCount            ACTIVE     29         2            263

### <a name="deactivate-and-reactivate"></a><span data-ttu-id="4b37b-180">Inaktivera och återaktivera</span><span class="sxs-lookup"><span data-stu-id="4b37b-180">Deactivate and reactivate</span></span>

<span data-ttu-id="4b37b-181">Inaktivera en topologi pausar den förrän den har avslutats eller återaktivera.</span><span class="sxs-lookup"><span data-stu-id="4b37b-181">Deactivating a topology pauses it until it is killed or reactivated.</span></span> <span data-ttu-id="4b37b-182">Använd följande kommando toodeactivate hello och återaktivera:</span><span class="sxs-lookup"><span data-stu-id="4b37b-182">Use hello following command toodeactivate and reactivate:</span></span>

    storm Deactivate TOPOLOGYNAME

    storm Activate TOPOLOGYNAME

### <a name="kill-a-running-topology"></a><span data-ttu-id="4b37b-183">Avsluta en topologi som körs</span><span class="sxs-lookup"><span data-stu-id="4b37b-183">Kill a running topology</span></span>

<span data-ttu-id="4b37b-184">Storm-topologier, startas en gång, fortsätta köras tills stoppades.</span><span class="sxs-lookup"><span data-stu-id="4b37b-184">Storm topologies, once started, continue running until stopped.</span></span> <span data-ttu-id="4b37b-185">toostop en topologi med hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="4b37b-185">toostop a topology, use hello following command:</span></span>

    storm kill TOPOLOGYNAME

### <a name="rebalance"></a><span data-ttu-id="4b37b-186">Balansera</span><span class="sxs-lookup"><span data-stu-id="4b37b-186">Rebalance</span></span>

<span data-ttu-id="4b37b-187">Ombalansering en topologi kan hello system toorevise hello parallellitet hello-topologi.</span><span class="sxs-lookup"><span data-stu-id="4b37b-187">Rebalancing a topology allows hello system toorevise hello parallelism of hello topology.</span></span> <span data-ttu-id="4b37b-188">Till exempel om du har ändrat storlek hello klustret tooadd anteckningar, kan ombalansering en topologi toosee hello nya noder.</span><span class="sxs-lookup"><span data-stu-id="4b37b-188">For example, if you have resized hello cluster tooadd more notes, rebalancing allows a topology toosee hello new nodes.</span></span>

> [!WARNING]
> <span data-ttu-id="4b37b-189">Ombalansering en topologi först inaktiverar hello topologi, och sedan distribuerar arbetare jämnt över hello kluster slutligen och returnerar sedan hello topologi toohello tillstånd innan ombalansering inträffade.</span><span class="sxs-lookup"><span data-stu-id="4b37b-189">Rebalancing a topology first deactivates hello topology, then redistributes workers evenly across hello cluster, then finally returns hello topology toohello state it was in before rebalancing occurred.</span></span> <span data-ttu-id="4b37b-190">Så om hello topologi var aktiv blir aktiva igen.</span><span class="sxs-lookup"><span data-stu-id="4b37b-190">So if hello topology was active, it becomes active again.</span></span> <span data-ttu-id="4b37b-191">Om det har inaktiverats, förblir den inaktiverade.</span><span class="sxs-lookup"><span data-stu-id="4b37b-191">If it was deactivated, it remains deactivated.</span></span>

    storm rebalance TOPOLOGYNAME

## <a name="monitor-and-manage-storm-ui"></a><span data-ttu-id="4b37b-192">Övervaka och hantera: Storm UI</span><span class="sxs-lookup"><span data-stu-id="4b37b-192">Monitor and manage: Storm UI</span></span>

<span data-ttu-id="4b37b-193">hello Storm-Användargränssnittet innehåller ett webbgränssnitt för att arbeta med topologier som körs och ingår i ditt HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="4b37b-193">hello Storm UI provides a web interface for working with running topologies, and is included on your HDInsight cluster.</span></span> <span data-ttu-id="4b37b-194">tooview hello Storm-Användargränssnittet, använda en web webbläsare tooopen **https://CLUSTERNAME.azurehdinsight.net/stormui**, där **KLUSTERNAMN** är hello namnet på klustret.</span><span class="sxs-lookup"><span data-stu-id="4b37b-194">tooview hello Storm UI, use a web browser tooopen **https://CLUSTERNAME.azurehdinsight.net/stormui**, where **CLUSTERNAME** is hello name of your cluster.</span></span>

> [!NOTE]
> <span data-ttu-id="4b37b-195">Om du blir tillfrågad tooprovide ett användarnamn och lösenord, ange hello Klusteradministratören (admin) och lösenord som du använde när du skapar hello-klustret.</span><span class="sxs-lookup"><span data-stu-id="4b37b-195">If asked tooprovide a user name and password, enter hello cluster administrator (admin) and password that you used when creating hello cluster.</span></span>

### <a name="main-page"></a><span data-ttu-id="4b37b-196">Huvudsida</span><span class="sxs-lookup"><span data-stu-id="4b37b-196">Main page</span></span>

<span data-ttu-id="4b37b-197">hello huvudsidan för hello Storm-Användargränssnittet innehåller hello följande information:</span><span class="sxs-lookup"><span data-stu-id="4b37b-197">hello main page of hello Storm UI provides hello following information:</span></span>

* <span data-ttu-id="4b37b-198">**Översikt över kluster**: grundläggande information om hello Storm-kluster.</span><span class="sxs-lookup"><span data-stu-id="4b37b-198">**Cluster summary**: Basic information about hello Storm cluster.</span></span>
* <span data-ttu-id="4b37b-199">**Översikt över topologi**: en lista över topologier som körs.</span><span class="sxs-lookup"><span data-stu-id="4b37b-199">**Topology summary**: A list of running topologies.</span></span> <span data-ttu-id="4b37b-200">Använd hello länkar i det här avsnittet tooview mer information om specifika topologier.</span><span class="sxs-lookup"><span data-stu-id="4b37b-200">Use hello links in this section tooview more information about specific topologies.</span></span>
* <span data-ttu-id="4b37b-201">**Översikt över handledarens**: Information om hello Storm chef.</span><span class="sxs-lookup"><span data-stu-id="4b37b-201">**Supervisor summary**: Information about hello Storm supervisor.</span></span>
* <span data-ttu-id="4b37b-202">**Nimbus configuration**: Nimbus-konfigurationen för hello kluster.</span><span class="sxs-lookup"><span data-stu-id="4b37b-202">**Nimbus configuration**: Nimbus configuration for hello cluster.</span></span>

### <a name="topology-summary"></a><span data-ttu-id="4b37b-203">Topologi sammanfattning</span><span class="sxs-lookup"><span data-stu-id="4b37b-203">Topology summary</span></span>

<span data-ttu-id="4b37b-204">Att välja en länk från hello **Topology summary** avsnitt visar följande information om hello topologi hello:</span><span class="sxs-lookup"><span data-stu-id="4b37b-204">Selecting a link from hello **Topology summary** section displays hello following information about hello topology:</span></span>

* <span data-ttu-id="4b37b-205">**Översikt över topologi**: grundläggande information om hello-topologi.</span><span class="sxs-lookup"><span data-stu-id="4b37b-205">**Topology summary**: Basic information about hello topology.</span></span>
* <span data-ttu-id="4b37b-206">**Topologi åtgärder**: hanteringsåtgärder som du kan utföra för hello-topologi.</span><span class="sxs-lookup"><span data-stu-id="4b37b-206">**Topology actions**: Management actions that you can perform for hello topology.</span></span>

  * <span data-ttu-id="4b37b-207">**Aktivera**: återupptar bearbetningen av en inaktiverad topologi.</span><span class="sxs-lookup"><span data-stu-id="4b37b-207">**Activate**: Resumes processing of a deactivated topology.</span></span>
  * <span data-ttu-id="4b37b-208">**Inaktivera**: pausar en topologi som körs.</span><span class="sxs-lookup"><span data-stu-id="4b37b-208">**Deactivate**: Pauses a running topology.</span></span>
  * <span data-ttu-id="4b37b-209">**Balansera**: justerar hello parallellitet hello-topologi.</span><span class="sxs-lookup"><span data-stu-id="4b37b-209">**Rebalance**: Adjusts hello parallelism of hello topology.</span></span> <span data-ttu-id="4b37b-210">Du bör balansera om topologier som körs när du har ändrat hello antalet noder i klustret hello.</span><span class="sxs-lookup"><span data-stu-id="4b37b-210">You should rebalance running topologies after you have changed hello number of nodes in hello cluster.</span></span> <span data-ttu-id="4b37b-211">Den här åtgärden tillåter hello topologi tooadjust parallellitet toocompensate för hello ökas eller minskas antalet noder i klustret hello.</span><span class="sxs-lookup"><span data-stu-id="4b37b-211">This operation allows hello topology tooadjust parallelism toocompensate for hello increased or decreased number of nodes in hello cluster.</span></span>

    <span data-ttu-id="4b37b-212">Mer information finns i <a href="http://storm.apache.org/documentation/Understanding-the-parallelism-of-a-Storm-topology.html" target="_blank">förstå hello parallellitet i en Storm-topologi</a>.</span><span class="sxs-lookup"><span data-stu-id="4b37b-212">For more information, see <a href="http://storm.apache.org/documentation/Understanding-the-parallelism-of-a-Storm-topology.html" target="_blank">Understanding hello parallelism of a Storm topology</a>.</span></span>
  * <span data-ttu-id="4b37b-213">**Avsluta**: avslutar en Storm-topologi efter hello angivna timeout.</span><span class="sxs-lookup"><span data-stu-id="4b37b-213">**Kill**: Terminates a Storm topology after hello specified timeout.</span></span>
* <span data-ttu-id="4b37b-214">**Topology stats**: statistik om hello-topologi.</span><span class="sxs-lookup"><span data-stu-id="4b37b-214">**Topology stats**: Statistics about hello topology.</span></span> <span data-ttu-id="4b37b-215">tooset hello tidsperioden för hello återstående poster på hello sidan kan du använda hello länkar i hello **fönstret** kolumn.</span><span class="sxs-lookup"><span data-stu-id="4b37b-215">tooset hello timeframe for hello remaining entries on hello page, use hello links in hello **Window** column.</span></span>
* <span data-ttu-id="4b37b-216">**Spouts**: hello kanaler som används av hello-topologi.</span><span class="sxs-lookup"><span data-stu-id="4b37b-216">**Spouts**: hello spouts used by hello topology.</span></span> <span data-ttu-id="4b37b-217">Använd hello länkar i det här avsnittet tooview mer information om specifika kanaler.</span><span class="sxs-lookup"><span data-stu-id="4b37b-217">Use hello links in this section tooview more information about specific spouts.</span></span>
* <span data-ttu-id="4b37b-218">**Bolts**: hello bultar som används av hello-topologi.</span><span class="sxs-lookup"><span data-stu-id="4b37b-218">**Bolts**: hello bolts used by hello topology.</span></span> <span data-ttu-id="4b37b-219">Använd hello länkar i det här avsnittet tooview mer information om specifika bultar.</span><span class="sxs-lookup"><span data-stu-id="4b37b-219">Use hello links in this section tooview more information about specific bolts.</span></span>
* <span data-ttu-id="4b37b-220">**Topologi configuration**: hello konfigurationen av hello valt topologi.</span><span class="sxs-lookup"><span data-stu-id="4b37b-220">**Topology configuration**: hello configuration of hello selected topology.</span></span>

### <a name="spout-and-bolt-summary"></a><span data-ttu-id="4b37b-221">Kanal och bult sammanfattning</span><span class="sxs-lookup"><span data-stu-id="4b37b-221">Spout and Bolt summary</span></span>

<span data-ttu-id="4b37b-222">Att välja en kanal hello **Spouts** eller **Bolts** avsnitt visar hello följande information om hello markerat objekt:</span><span class="sxs-lookup"><span data-stu-id="4b37b-222">Selecting a spout from hello **Spouts** or **Bolts** sections displays hello following information about hello selected item:</span></span>

* <span data-ttu-id="4b37b-223">**Sammanfattning**: grundläggande information om hello-kanal eller en bult.</span><span class="sxs-lookup"><span data-stu-id="4b37b-223">**Component summary**: Basic information about hello spout or bolt.</span></span>
* <span data-ttu-id="4b37b-224">**Kanal/bult stats**: statistik om hello prata eller bultar.</span><span class="sxs-lookup"><span data-stu-id="4b37b-224">**Spout/Bolt stats**: Statistics about hello spout or bolt.</span></span> <span data-ttu-id="4b37b-225">tooset hello tidsperioden för hello återstående poster på hello sidan kan du använda hello länkar i hello **fönstret** kolumn.</span><span class="sxs-lookup"><span data-stu-id="4b37b-225">tooset hello timeframe for hello remaining entries on hello page, use hello links in hello **Window** column.</span></span>
* <span data-ttu-id="4b37b-226">**Input stats** (endast bultar): Information om hello indataström som används av hello bulten.</span><span class="sxs-lookup"><span data-stu-id="4b37b-226">**Input stats** (bolt only): Information about hello input streams consumed by hello bolt.</span></span>
* <span data-ttu-id="4b37b-227">**Utdata stats**: Information om hello-dataströmmar som orsakat av detta kanal eller bultar.</span><span class="sxs-lookup"><span data-stu-id="4b37b-227">**Output stats**: Information about hello streams emitted by this spout or bolt.</span></span>
* <span data-ttu-id="4b37b-228">**Executors**: Information om hello instanser av hello-kanal eller en bult.</span><span class="sxs-lookup"><span data-stu-id="4b37b-228">**Executors**: Information about hello instances of hello spout or bolt.</span></span> <span data-ttu-id="4b37b-229">Välj hello **Port** post för en specifik utförare tooview genereras en logg över diagnostisk information för den här instansen.</span><span class="sxs-lookup"><span data-stu-id="4b37b-229">Select hello **Port** entry for a specific executor tooview a log of diagnostic information produced for this instance.</span></span>
* <span data-ttu-id="4b37b-230">**Fel**: felinformation för den här kanalen eller bultar.</span><span class="sxs-lookup"><span data-stu-id="4b37b-230">**Errors**: Any error information for this spout or bolt.</span></span>

## <a name="monitor-and-manage-rest-api"></a><span data-ttu-id="4b37b-231">Övervaka och hantera: REST API</span><span class="sxs-lookup"><span data-stu-id="4b37b-231">Monitor and manage: REST API</span></span>

<span data-ttu-id="4b37b-232">hello Storm-Användargränssnittet är byggt på hello REST-API, så att du kan göra liknande hantering och övervakning av funktioner med hjälp av hello REST API.</span><span class="sxs-lookup"><span data-stu-id="4b37b-232">hello Storm UI is built on top of hello REST API, so you can perform similar management and monitoring functionality by using hello REST API.</span></span> <span data-ttu-id="4b37b-233">Du kan använda hello REST API toocreate anpassade verktyg för att hantera och övervaka Storm-topologier.</span><span class="sxs-lookup"><span data-stu-id="4b37b-233">You can use hello REST API toocreate custom tools for managing and monitoring Storm topologies.</span></span>

<span data-ttu-id="4b37b-234">Mer information finns i [Storm UI REST API](http://storm.apache.org/releases/0.9.6/STORM-UI-REST-API.html).</span><span class="sxs-lookup"><span data-stu-id="4b37b-234">For more information, see [Storm UI REST API](http://storm.apache.org/releases/0.9.6/STORM-UI-REST-API.html).</span></span> <span data-ttu-id="4b37b-235">hello är följande uppgifter specifika toousing hello REST-API med Apache Storm på HDInsight.</span><span class="sxs-lookup"><span data-stu-id="4b37b-235">hello following information is specific toousing hello REST API with Apache Storm on HDInsight.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="4b37b-236">hello Storm REST API är inte offentligt tillgänglig över hello internet och måste användas med en SSH-tunnel toohello HDInsight-klustrets huvudnod.</span><span class="sxs-lookup"><span data-stu-id="4b37b-236">hello Storm REST API is not publicly available over hello internet, and must be accessed using an SSH tunnel toohello HDInsight cluster head node.</span></span> <span data-ttu-id="4b37b-237">Mer information om hur du skapar och använder en SSH-tunnel finns [använda SSH-tunnlar tooaccess Ambari web UI, resurshanteraren, jobbhistorik, NameNode, Oozie och andra webb-användargränssnitt](hdinsight-linux-ambari-ssh-tunnel.md).</span><span class="sxs-lookup"><span data-stu-id="4b37b-237">For information on creating and using an SSH tunnel, see [Use SSH Tunneling tooaccess Ambari web UI, ResourceManager, JobHistory, NameNode, Oozie, and other web UIs](hdinsight-linux-ambari-ssh-tunnel.md).</span></span>

### <a name="base-uri"></a><span data-ttu-id="4b37b-238">Bas-URI</span><span class="sxs-lookup"><span data-stu-id="4b37b-238">Base URI</span></span>

<span data-ttu-id="4b37b-239">hello bas-URI för hello REST API på Linux-baserade HDInsight-kluster finns i hello huvudnod vid **v1-https://HEADNODEFQDN:8744/api/**.</span><span class="sxs-lookup"><span data-stu-id="4b37b-239">hello base URI for hello REST API on Linux-based HDInsight clusters is available on hello head node at **https://HEADNODEFQDN:8744/api/v1/**.</span></span> <span data-ttu-id="4b37b-240">hello domännamnet för hello huvudnod genereras när klustret skapas och är inte statisk.</span><span class="sxs-lookup"><span data-stu-id="4b37b-240">hello domain name of hello head node is generated during cluster creation and is not static.</span></span>

<span data-ttu-id="4b37b-241">Du kan hitta hello fullständigt kvalificerade domännamnet (FQDN) för hello klustrets huvudnod på flera olika sätt:</span><span class="sxs-lookup"><span data-stu-id="4b37b-241">You can find hello fully qualified domain name (FQDN) for hello cluster head node in several different ways:</span></span>

* <span data-ttu-id="4b37b-242">**Från en SSH-session**: kommandot hello `headnode -f` från ett kluster med SSH-session toohello.</span><span class="sxs-lookup"><span data-stu-id="4b37b-242">**From an SSH session**: Use hello command `headnode -f` from an SSH session toohello cluster.</span></span>
* <span data-ttu-id="4b37b-243">**Ambari Web**: Välj **Services** från hello överst på hello-sidan, Välj **Storm**.</span><span class="sxs-lookup"><span data-stu-id="4b37b-243">**From Ambari Web**: Select **Services** from hello top of hello page, then select **Storm**.</span></span> <span data-ttu-id="4b37b-244">Från hello **sammanfattning** väljer **Storm UI Server**.</span><span class="sxs-lookup"><span data-stu-id="4b37b-244">From hello **Summary** tab, select **Storm UI Server**.</span></span> <span data-ttu-id="4b37b-245">hello FQDN för hello-nod som kör hello Storm-Användargränssnittet och REST API är hello överst på hello sidan.</span><span class="sxs-lookup"><span data-stu-id="4b37b-245">hello FQDN of hello node that hello Storm UI and REST API is running is at hello top of hello page.</span></span>
* <span data-ttu-id="4b37b-246">**Från Ambari REST API**: kommandot hello `curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/STORM/components/STORM_UI_SERVER"` tooretrieve information om hello-nod som hello Storm-Användargränssnittet och REST-API som körs på.</span><span class="sxs-lookup"><span data-stu-id="4b37b-246">**From Ambari REST API**: Use hello command `curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/STORM/components/STORM_UI_SERVER"` tooretrieve information about hello node that hello Storm UI and REST API are running on.</span></span> <span data-ttu-id="4b37b-247">Ersätt **lösenord** med hello administratörslösenord för hello-kluster.</span><span class="sxs-lookup"><span data-stu-id="4b37b-247">Replace **PASSWORD** with hello admin password for hello cluster.</span></span> <span data-ttu-id="4b37b-248">Ersätt **KLUSTERNAMN** med hello klusternamnet.</span><span class="sxs-lookup"><span data-stu-id="4b37b-248">Replace **CLUSTERNAME** with hello cluster name.</span></span> <span data-ttu-id="4b37b-249">Hello svar innehåller hello ”värddatornamn” post hello FQDN för hello-nod.</span><span class="sxs-lookup"><span data-stu-id="4b37b-249">In hello response, hello "host_name" entry contains hello FQDN of hello node.</span></span>

### <a name="authentication"></a><span data-ttu-id="4b37b-250">Autentisering</span><span class="sxs-lookup"><span data-stu-id="4b37b-250">Authentication</span></span>

<span data-ttu-id="4b37b-251">Begäranden toohello REST API måste använda **grundläggande autentisering**, så du använda hello HDInsight-kluster administratörens namn och lösenord.</span><span class="sxs-lookup"><span data-stu-id="4b37b-251">Requests toohello REST API must use **basic authentication**, so you use hello HDInsight cluster administrator name and password.</span></span>

> [!NOTE]
> <span data-ttu-id="4b37b-252">Eftersom grundläggande autentisering skickas i klartext, bör du **alltid** använder toosecure HTTPS-kommunikation med hello-kluster.</span><span class="sxs-lookup"><span data-stu-id="4b37b-252">Because basic authentication is sent by using clear text, you should **always** use HTTPS toosecure communications with hello cluster.</span></span>

### <a name="return-values"></a><span data-ttu-id="4b37b-253">Returnera värden</span><span class="sxs-lookup"><span data-stu-id="4b37b-253">Return values</span></span>

<span data-ttu-id="4b37b-254">Information som returneras från hello REST-API kan endast användas från inom hello kluster eller virtuella datorer på hello samma virtuella Azure-nätverk som hello kluster.</span><span class="sxs-lookup"><span data-stu-id="4b37b-254">Information that is returned from hello REST API may only be usable from within hello cluster or virtual machines on hello same Azure Virtual Network as hello cluster.</span></span> <span data-ttu-id="4b37b-255">Hello fullständigt kvalificerade domännamnet (FQDN) returneras för Zookeeper-servrar är till exempel inte att komma åt från hello Internet.</span><span class="sxs-lookup"><span data-stu-id="4b37b-255">For example, hello fully qualified domain name (FQDN) returned for Zookeeper servers is not be accessible from hello Internet.</span></span>

## <a name="next-steps"></a><span data-ttu-id="4b37b-256">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="4b37b-256">Next Steps</span></span>

<span data-ttu-id="4b37b-257">Nu när du har lärt dig hur toodeploy och övervaka topologier med hjälp av hello Storm-instrumentpanelen, lär du dig hur för[utveckla Java-baserade topologier med Maven](hdinsight-storm-develop-java-topology.md).</span><span class="sxs-lookup"><span data-stu-id="4b37b-257">Now that you've learned how toodeploy and monitor topologies by using hello Storm Dashboard, learn how too[Develop Java-based topologies using Maven](hdinsight-storm-develop-java-topology.md).</span></span>

<span data-ttu-id="4b37b-258">En lista över flera exempeltopologier finns [exempeltopologier för Storm på HDInsight](hdinsight-storm-example-topology.md).</span><span class="sxs-lookup"><span data-stu-id="4b37b-258">For a list of more example topologies, see [Example topologies for Storm on HDInsight](hdinsight-storm-example-topology.md).</span></span>
