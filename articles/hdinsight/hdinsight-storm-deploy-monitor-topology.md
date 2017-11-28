---
title: "aaaDeploy och hantera Apache Storm-topologier på HDInsight | Microsoft Docs"
description: "Lär dig hur toodeploy, övervaka och hantera Apache Storm-topologier med hello Storm-instrumentpanelen på HDInsight. Använda Hadoop-verktyg för Visual Studio."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 5e542072-f014-42aa-82d6-2694a76df520
ms.service: hdinsight
ms.devlang: java
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 03/01/2017
ms.author: larryfr
ROBOTS: NOINDEX
ms.openlocfilehash: 05f05fe8dd519fe99fb771d36bfc3d28168ca85f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-and-manage-apache-storm-topologies-on-windows-based-hdinsight"></a><span data-ttu-id="31143-104">Distribuera och hantera Apache Storm-topologier på Windows-baserade HDInsight</span><span class="sxs-lookup"><span data-stu-id="31143-104">Deploy and manage Apache Storm topologies on Windows-based HDInsight</span></span>

<span data-ttu-id="31143-105">hello Storm-instrumentpanelen kan du tooeasily distribuera och köra Apache Storm-topologier tooyour HDInsight-kluster med hjälp av webbläsaren.</span><span class="sxs-lookup"><span data-stu-id="31143-105">hello Storm Dashboard allows you tooeasily deploy and run Apache Storm topologies tooyour HDInsight cluster by using your web browser.</span></span> <span data-ttu-id="31143-106">Du kan också använda hello instrumentpanelen toomonitor och hantera topologier som körs.</span><span class="sxs-lookup"><span data-stu-id="31143-106">You can also use hello dashboard toomonitor and manage running topologies.</span></span> <span data-ttu-id="31143-107">Om du använder Visual Studio innehåller hello HDInsight Tools för Visual Studio liknande funktioner i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="31143-107">If you use Visual Studio, hello HDInsight Tools for Visual Studio provide similar features in Visual Studio.</span></span>

<span data-ttu-id="31143-108">hello Storm-instrumentpanelen och hello Storm-funktioner i hello HDInsight Tools förlitar sig på hello Storm REST-API som kan vara används toocreate övervakning och hantering av lösningar.</span><span class="sxs-lookup"><span data-stu-id="31143-108">hello Storm Dashboard and hello Storm features in hello HDInsight Tools rely on hello Storm REST API, which can be used toocreate your own monitoring and management solutions.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="31143-109">hello stegen i det här dokumentet kräver ett Storm på HDInsight-kluster som använder Windows hello-operativsystemet.</span><span class="sxs-lookup"><span data-stu-id="31143-109">hello steps in this document require a Storm on HDInsight cluster that uses Windows as hello operating system.</span></span> <span data-ttu-id="31143-110">Linux är hello endast operativsystem på HDInsight version 3.4 eller senare.</span><span class="sxs-lookup"><span data-stu-id="31143-110">Linux is hello only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="31143-111">Mer information finns i [HDInsight-avveckling på Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="31143-111">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>
>
> <span data-ttu-id="31143-112">Mer information om distribuera och hantera Storm-topologier med ett HDInsight-kluster som använder Linux finns [distribuera och hantera Apache Storm-topologier på Linux-baserat HDInsight](hdinsight-storm-deploy-monitor-topology-linux.md)</span><span class="sxs-lookup"><span data-stu-id="31143-112">For information on deploying and managing Storm topologies with an HDInsight cluster that uses Linux, see [Deploy and manage Apache Storm topologies on Linux-based HDInsight](hdinsight-storm-deploy-monitor-topology-linux.md)</span></span>

## <a name="prerequisites"></a><span data-ttu-id="31143-113">Krav</span><span class="sxs-lookup"><span data-stu-id="31143-113">Prerequisites</span></span>

* <span data-ttu-id="31143-114">**Apache Storm på HDInsight** -finns [Kom igång med Apache Storm på HDInsight](hdinsight-apache-storm-tutorial-get-started.md) anvisningar om hur du skapar ett kluster.</span><span class="sxs-lookup"><span data-stu-id="31143-114">**Apache Storm on HDInsight** - see [Get started with Apache Storm on HDInsight](hdinsight-apache-storm-tutorial-get-started.md) for steps on creating a cluster.</span></span>

* <span data-ttu-id="31143-115">För hello **Storm-instrumentpanelen**: en modern webbläsare som stöder HTML5.</span><span class="sxs-lookup"><span data-stu-id="31143-115">For hello **Storm Dashboard**: A modern web browser that supports HTML5.</span></span>

* <span data-ttu-id="31143-116">För **Visual Studio** -Azure SDK 2.5.1 eller senare och hello HDInsight Tools för Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="31143-116">For **Visual Studio** - Azure SDK 2.5.1 or newer and hello HDInsight Tools for Visual Studio.</span></span> <span data-ttu-id="31143-117">Se [komma igång med HDInsight Tools för Visual Studio](hdinsight-hadoop-visual-studio-tools-get-started.md) tooinstall och konfigurera hello HDInsight tools för Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="31143-117">See [Get started using HDInsight Tools for Visual Studio](hdinsight-hadoop-visual-studio-tools-get-started.md) tooinstall and configure hello HDInsight tools for Visual Studio.</span></span>

    <span data-ttu-id="31143-118">En av följande versioner av Visual Studio hello:</span><span class="sxs-lookup"><span data-stu-id="31143-118">One of hello following versions of Visual Studio:</span></span>

  * <span data-ttu-id="31143-119">Visual Studio 2012 med Update 4</span><span class="sxs-lookup"><span data-stu-id="31143-119">Visual Studio 2012 with Update 4</span></span>

  * <span data-ttu-id="31143-120">Visual Studio 2013 med Update 4 eller Visual Studio Community 2013</span><span class="sxs-lookup"><span data-stu-id="31143-120">Visual Studio 2013 with Update 4 or Visual Studio 2013 Community</span></span>

  * <span data-ttu-id="31143-121">Visual Studio 2015 (någon utgåva)</span><span class="sxs-lookup"><span data-stu-id="31143-121">Visual Studio 2015 (any edition)</span></span>

  * <span data-ttu-id="31143-122">2017 för Visual Studio (någon utgåva)</span><span class="sxs-lookup"><span data-stu-id="31143-122">Visual Studio 2017 (any edition)</span></span>

## <a name="storm-dashboard"></a><span data-ttu-id="31143-123">Storm-instrumentpanelen</span><span class="sxs-lookup"><span data-stu-id="31143-123">Storm Dashboard</span></span>

<span data-ttu-id="31143-124">hello Storm-instrumentpanelen är en webbsida som är tillgängliga på Storm-kluster.</span><span class="sxs-lookup"><span data-stu-id="31143-124">hello Storm Dashboard is a web page available on your Storm cluster.</span></span> <span data-ttu-id="31143-125">hello URL: en är **https://&lt;klusternamn >.azurehdinsight.net/**, där **klusternamn** är hello namnet på ditt Storm på HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="31143-125">hello URL is **https://&lt;clustername>.azurehdinsight.net/**, where **clustername** is hello name of your Storm on HDInsight cluster.</span></span>

<span data-ttu-id="31143-126">Hello överkant hello Storm-instrumentpanelen, Välj **skicka topologi**.</span><span class="sxs-lookup"><span data-stu-id="31143-126">From hello top of hello Storm Dashboard, select **Submit Topology**.</span></span> <span data-ttu-id="31143-127">Följ hello anvisningar på hello sidan toorun en exempeltopologi eller tooupload och kör en topologi som du skapade.</span><span class="sxs-lookup"><span data-stu-id="31143-127">Follow hello instructions on hello page toorun a sample topology or tooupload and run a topology that you created.</span></span>

![hello skicka topologi sida][storm-dashboard-submit]

### <a name="storm-ui"></a><span data-ttu-id="31143-129">Storm UI</span><span class="sxs-lookup"><span data-stu-id="31143-129">Storm UI</span></span>

<span data-ttu-id="31143-130">Hello Storm-instrumentpanelen, Välj hello **Storm-Användargränssnittet** länk.</span><span class="sxs-lookup"><span data-stu-id="31143-130">From hello Storm Dashboard, select hello **Storm UI** link.</span></span> <span data-ttu-id="31143-131">Då visas information om hello-klustret i tillägg tooany topologier som körs.</span><span class="sxs-lookup"><span data-stu-id="31143-131">This displays information about hello cluster, in addition tooany running topologies.</span></span>

![hello storm-användargränssnittet][storm-dashboard-ui]

> [!NOTE]
> <span data-ttu-id="31143-133">I vissa versioner av Internet Explorer kan du identifiera den hello Storm-Användargränssnittet inte kan uppdatera när du först har besökt.</span><span class="sxs-lookup"><span data-stu-id="31143-133">With some versions of Internet Explorer, you may discover that hello Storm UI does not refresh after you have first visited it.</span></span> <span data-ttu-id="31143-134">Det kan till exempel inte visa hello nya topologier du skickat, eller den kan visa en topologi som aktiv när du tidigare har inaktiverats.</span><span class="sxs-lookup"><span data-stu-id="31143-134">For example, it may not show hello new topologies you submitted, or it may show a topology as active when you previously deactivated it.</span></span> <span data-ttu-id="31143-135">Microsoft är medveten om det här problemet och arbetar på en lösning.</span><span class="sxs-lookup"><span data-stu-id="31143-135">Microsoft is aware of this issue and is working on a solution.</span></span>

#### <a name="main-page"></a><span data-ttu-id="31143-136">Huvudsida</span><span class="sxs-lookup"><span data-stu-id="31143-136">Main page</span></span>

<span data-ttu-id="31143-137">hello huvudsidan för hello Storm-Användargränssnittet innehåller hello följande information:</span><span class="sxs-lookup"><span data-stu-id="31143-137">hello main page of hello Storm UI provides hello following information:</span></span>

* <span data-ttu-id="31143-138">**Översikt över kluster**: grundläggande information om hello Storm-kluster.</span><span class="sxs-lookup"><span data-stu-id="31143-138">**Cluster summary**: Basic information about hello Storm cluster.</span></span>

* <span data-ttu-id="31143-139">**Översikt över topologi**: en lista över topologier som körs.</span><span class="sxs-lookup"><span data-stu-id="31143-139">**Topology summary**: A list of running topologies.</span></span> <span data-ttu-id="31143-140">Använd hello länkar i det här avsnittet tooview mer information om specifika topologier.</span><span class="sxs-lookup"><span data-stu-id="31143-140">Use hello links in this section tooview more information about specific topologies.</span></span>

* <span data-ttu-id="31143-141">**Översikt över handledarens**: Information om hello Storm chef.</span><span class="sxs-lookup"><span data-stu-id="31143-141">**Supervisor summary**: Information about hello Storm supervisor.</span></span>

* <span data-ttu-id="31143-142">**Nimbus configuration**: Nimbus-konfigurationen för hello kluster.</span><span class="sxs-lookup"><span data-stu-id="31143-142">**Nimbus configuration**: Nimbus configuration for hello cluster.</span></span>

#### <a name="topology-summary"></a><span data-ttu-id="31143-143">Topologi sammanfattning</span><span class="sxs-lookup"><span data-stu-id="31143-143">Topology summary</span></span>

<span data-ttu-id="31143-144">Att välja en länk från hello **Topology summary** avsnitt visar följande information om hello topologi hello:</span><span class="sxs-lookup"><span data-stu-id="31143-144">Selecting a link from hello **Topology summary** section displays hello following information about hello topology:</span></span>

* <span data-ttu-id="31143-145">**Översikt över topologi**: grundläggande information om hello-topologi.</span><span class="sxs-lookup"><span data-stu-id="31143-145">**Topology summary**: Basic information about hello topology.</span></span>

* <span data-ttu-id="31143-146">**Topologi åtgärder**: hanteringsåtgärder som du kan utföra för hello-topologi.</span><span class="sxs-lookup"><span data-stu-id="31143-146">**Topology actions**: Management actions that you can perform for hello topology.</span></span>

  * <span data-ttu-id="31143-147">**Aktivera**: återupptar bearbetningen av en inaktiverad topologi.</span><span class="sxs-lookup"><span data-stu-id="31143-147">**Activate**: Resumes processing of a deactivated topology.</span></span>

  * <span data-ttu-id="31143-148">**Inaktivera**: pausar en topologi som körs.</span><span class="sxs-lookup"><span data-stu-id="31143-148">**Deactivate**: Pauses a running topology.</span></span>

  * <span data-ttu-id="31143-149">**Balansera**: justerar hello parallellitet hello-topologi.</span><span class="sxs-lookup"><span data-stu-id="31143-149">**Rebalance**: Adjusts hello parallelism of hello topology.</span></span> <span data-ttu-id="31143-150">Du bör balansera om topologier som körs när du har ändrat hello antalet noder i klustret hello.</span><span class="sxs-lookup"><span data-stu-id="31143-150">You should rebalance running topologies after you have changed hello number of nodes in hello cluster.</span></span> <span data-ttu-id="31143-151">Detta gör hello topologi tooadjust parallellitet toocompensate för hello ökas eller minskas antalet noder i klustret hello.</span><span class="sxs-lookup"><span data-stu-id="31143-151">This allows hello topology tooadjust parallelism toocompensate for hello increased or decreased number of nodes in hello cluster.</span></span>

      <span data-ttu-id="31143-152">Mer information finns i [förstå hello parallellitet i en Storm-topologi (http://storm.apache.org/documentation/Understanding-the-parallelism-of-a-Storm-topology.html)](http://storm.apache.org/documentation/Understanding-the-parallelism-of-a-Storm-topology.html).</span><span class="sxs-lookup"><span data-stu-id="31143-152">For more information, see [Understanding hello parallelism of a Storm topology (http://storm.apache.org/documentation/Understanding-the-parallelism-of-a-Storm-topology.html)](http://storm.apache.org/documentation/Understanding-the-parallelism-of-a-Storm-topology.html).</span></span>

  * <span data-ttu-id="31143-153">**Avsluta**: avslutar en Storm-topologi efter hello angivna timeout.</span><span class="sxs-lookup"><span data-stu-id="31143-153">**Kill**: Terminates a Storm topology after hello specified timeout.</span></span>

* <span data-ttu-id="31143-154">**Topology stats**: statistik om hello-topologi.</span><span class="sxs-lookup"><span data-stu-id="31143-154">**Topology stats**: Statistics about hello topology.</span></span> <span data-ttu-id="31143-155">Hello länkarna i hello **fönstret** kolumnen tooset hello tidsperioden för hello återstående poster på hello-sidan.</span><span class="sxs-lookup"><span data-stu-id="31143-155">Use hello links in hello **Window** column tooset hello timeframe for hello remaining entries on hello page.</span></span>

* <span data-ttu-id="31143-156">**Spouts**: hello kanaler som används av hello-topologi.</span><span class="sxs-lookup"><span data-stu-id="31143-156">**Spouts**: hello spouts used by hello topology.</span></span> <span data-ttu-id="31143-157">Använd hello länkar i det här avsnittet tooview mer information om specifika kanaler.</span><span class="sxs-lookup"><span data-stu-id="31143-157">Use hello links in this section tooview more information about specific spouts.</span></span>

* <span data-ttu-id="31143-158">**Bolts**: hello bultar som används av hello-topologi.</span><span class="sxs-lookup"><span data-stu-id="31143-158">**Bolts**: hello bolts used by hello topology.</span></span> <span data-ttu-id="31143-159">Använd hello länkar i det här avsnittet tooview mer information om specifika bultar.</span><span class="sxs-lookup"><span data-stu-id="31143-159">Use hello links in this section tooview more information about specific bolts.</span></span>

* <span data-ttu-id="31143-160">**Topologi configuration**: hello konfigurationen av hello valt topologi.</span><span class="sxs-lookup"><span data-stu-id="31143-160">**Topology configuration**: hello configuration of hello selected topology.</span></span>

#### <a name="spout-and-bolt-summary"></a><span data-ttu-id="31143-161">Kanal och bult sammanfattning</span><span class="sxs-lookup"><span data-stu-id="31143-161">Spout and Bolt summary</span></span>

<span data-ttu-id="31143-162">Att välja en kanal hello **Spouts** eller **Bolts** avsnitt visar hello följande information om hello markerat objekt:</span><span class="sxs-lookup"><span data-stu-id="31143-162">Selecting a spout from hello **Spouts** or **Bolts** sections displays hello following information about hello selected item:</span></span>

* <span data-ttu-id="31143-163">**Sammanfattning**: grundläggande information om hello-kanal eller en bult.</span><span class="sxs-lookup"><span data-stu-id="31143-163">**Component summary**: Basic information about hello spout or bolt.</span></span>

* <span data-ttu-id="31143-164">**Kanal/bult stats**: statistik om hello prata eller bultar.</span><span class="sxs-lookup"><span data-stu-id="31143-164">**Spout/Bolt stats**: Statistics about hello spout or bolt.</span></span> <span data-ttu-id="31143-165">Hello länkarna i hello **fönstret** kolumnen tooset hello tidsperioden för hello återstående poster på hello-sidan.</span><span class="sxs-lookup"><span data-stu-id="31143-165">Use hello links in hello **Window** column tooset hello timeframe for hello remaining entries on hello page.</span></span>

* <span data-ttu-id="31143-166">**Input stats** (endast bultar): Information om hello indataström som används av hello bulten.</span><span class="sxs-lookup"><span data-stu-id="31143-166">**Input stats** (bolt only): Information about hello input streams consumed by hello bolt.</span></span>

* <span data-ttu-id="31143-167">**Utdata stats**: Information om hello-dataströmmar som orsakat av detta kanal eller bultar.</span><span class="sxs-lookup"><span data-stu-id="31143-167">**Output stats**: Information about hello streams emitted by this spout or bolt.</span></span>

* <span data-ttu-id="31143-168">**Executors**: Information om hello instanser av hello-kanal eller en bult.</span><span class="sxs-lookup"><span data-stu-id="31143-168">**Executors**: Information about hello instances of hello spout or bolt.</span></span> <span data-ttu-id="31143-169">Välj hello **Port** post för en specifik utförare tooview genereras en logg över diagnostisk information för den här instansen.</span><span class="sxs-lookup"><span data-stu-id="31143-169">Select hello **Port** entry for a specific executor tooview a log of diagnostic information produced for this instance.</span></span>

* <span data-ttu-id="31143-170">**Fel**: felinformation för den här kanalen eller bultar.</span><span class="sxs-lookup"><span data-stu-id="31143-170">**Errors**: Any error information for this spout or bolt.</span></span>

## <a name="hdinsight-tools-for-visual-studio"></a><span data-ttu-id="31143-171">HDInsight Tools för Visual Studio</span><span class="sxs-lookup"><span data-stu-id="31143-171">HDInsight Tools for Visual Studio</span></span>

<span data-ttu-id="31143-172">Hej HDInsight-verktyg kan vara används toosubmit eller hybrid C#-topologier tooyour Storm-kluster.</span><span class="sxs-lookup"><span data-stu-id="31143-172">hello HDInsight Tools can be used toosubmit C# or hybrid topologies tooyour Storm cluster.</span></span> <span data-ttu-id="31143-173">hello följande använder ett exempelprogram.</span><span class="sxs-lookup"><span data-stu-id="31143-173">hello following steps use a sample application.</span></span> <span data-ttu-id="31143-174">Information om hur du skapar egna topologier med hjälp av hello HDInsight Tools finns [utveckla C#-topologier med hello HDInsight Tools för Visual Studio](hdinsight-storm-develop-csharp-visual-studio-topology.md).</span><span class="sxs-lookup"><span data-stu-id="31143-174">For information about creating your own topologies by using hello HDInsight Tools, see [Develop C# topologies using hello HDInsight Tools for Visual Studio](hdinsight-storm-develop-csharp-visual-studio-topology.md).</span></span>

<span data-ttu-id="31143-175">Använd hello följande steg toodeploy exempel-tooyour Storm på HDInsight-kluster och sedan visa och hantera hello-topologi.</span><span class="sxs-lookup"><span data-stu-id="31143-175">Use hello following steps toodeploy a sample tooyour Storm on HDInsight cluster, then view and manage hello topology.</span></span>

1. <span data-ttu-id="31143-176">Om du inte redan har installerat hello senaste versionen av hello HDInsight Tools för Visual Studio finns [komma igång med HDInsight Tools för Visual Studio](hdinsight-hadoop-visual-studio-tools-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="31143-176">If you have not already installed hello latest version of hello HDInsight Tools for Visual Studio, see [Get started using HDInsight Tools for Visual Studio](hdinsight-hadoop-visual-studio-tools-get-started.md).</span></span>

2. <span data-ttu-id="31143-177">Öppna Visual Studio, markera **filen** > **ny** > **projekt**.</span><span class="sxs-lookup"><span data-stu-id="31143-177">Open Visual Studio, select **File** > **New** > **Project**.</span></span>

3. <span data-ttu-id="31143-178">I hello **nytt projekt** dialogrutan Expandera **installerad** > **mallar**, och välj sedan **HDInsight**.</span><span class="sxs-lookup"><span data-stu-id="31143-178">In hello **New Project** dialog box, expand **Installed** > **Templates**, and then select **HDInsight**.</span></span> <span data-ttu-id="31143-179">Välj hello listan över mallar **Storm exempel**.</span><span class="sxs-lookup"><span data-stu-id="31143-179">From hello list of templates, select **Storm Sample**.</span></span> <span data-ttu-id="31143-180">Ange ett namn för programmet hello hello längst ned i dialogrutan för hello i.</span><span class="sxs-lookup"><span data-stu-id="31143-180">At hello bottom of hello dialog box, type a name for hello application.</span></span>

    ![Bild](./media/hdinsight-storm-deploy-monitor-topology/sample.png)

4. <span data-ttu-id="31143-182">I **Solution Explorer**, högerklicka på hello-projektet och välj **skicka tooStorm på HDInsight**.</span><span class="sxs-lookup"><span data-stu-id="31143-182">In **Solution Explorer**, right-click hello project, and select **Submit tooStorm on HDInsight**.</span></span>

   > [!NOTE]
   > <span data-ttu-id="31143-183">Om du uppmanas ange hello inloggningsuppgifterna för din Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="31143-183">If prompted, enter hello login credentials for your Azure subscription.</span></span> <span data-ttu-id="31143-184">Om du har mer än en prenumeration kan du logga in toohello en som innehåller ditt Storm på HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="31143-184">If you have more than one subscription, log in toohello one that contains your Storm on HDInsight cluster.</span></span>

5. <span data-ttu-id="31143-185">Välj ditt Storm på HDInsight-kluster från hello **Storm-kluster** listrutan och välj sedan **skicka**.</span><span class="sxs-lookup"><span data-stu-id="31143-185">Select your Storm on HDInsight cluster from hello **Storm Cluster** drop-down list, and then select **Submit**.</span></span> <span data-ttu-id="31143-186">Du kan övervaka om hello ansökan har genomförts med hello **utdata** fönster.</span><span class="sxs-lookup"><span data-stu-id="31143-186">You can monitor whether hello submission is successful by using hello **Output** window.</span></span>

6. <span data-ttu-id="31143-187">När hello-topologi har skickats, hello **Storm-topologier** för hello klustret ska visas.</span><span class="sxs-lookup"><span data-stu-id="31143-187">When hello topology has been successfully submitted, hello **Storm Topologies** for hello cluster should appear.</span></span> <span data-ttu-id="31143-188">Välj hello topologi hello listan tooview information om hello kör topologi.</span><span class="sxs-lookup"><span data-stu-id="31143-188">Select hello topology from hello list tooview information about hello running topology.</span></span>

    ![Övervakare för Visual studio](./media/hdinsight-storm-deploy-monitor-topology/vsmonitor.png)

   > [!NOTE]
   > <span data-ttu-id="31143-190">Du kan också visa **Storm-topologier** från **Server Explorer** genom att expandera **Azure** > **HDInsight**, och sedan Högerklicka på ett Storm på HDInsight-kluster och välja **visa Storm-topologier**.</span><span class="sxs-lookup"><span data-stu-id="31143-190">You can also view **Storm Topologies** from **Server Explorer** by expanding **Azure** > **HDInsight**, and then right-clicking a Storm on HDInsight cluster, and selecting **View Storm Topologies**.</span></span>

    <span data-ttu-id="31143-191">Markera hello form för hello spouts eller bolts tooview information om dessa komponenter.</span><span class="sxs-lookup"><span data-stu-id="31143-191">Select hello shape for hello spouts or bolts tooview information about these components.</span></span> <span data-ttu-id="31143-192">För varje vald öppnas ett nytt fönster.</span><span class="sxs-lookup"><span data-stu-id="31143-192">A new window opens for each item selected.</span></span>

   > [!NOTE]
   > <span data-ttu-id="31143-193">hello hello topologi heter hello klassnamnet för hello-topologi (i det här fallet `HelloWord`,) med en tidstämpel läggas till.</span><span class="sxs-lookup"><span data-stu-id="31143-193">hello name of hello topology is hello class name of hello topology (in this case, `HelloWord`,) with a timestamp appended.</span></span>

7. <span data-ttu-id="31143-194">Från hello **Topology Summary** väljer **Kill** toostop hello-topologi.</span><span class="sxs-lookup"><span data-stu-id="31143-194">From hello **Topology Summary** view, select **Kill** toostop hello topology.</span></span>

   > [!NOTE]
   > <span data-ttu-id="31143-195">Storm-topologier fortsätta köras förrän de har stoppats eller hello klustret tas bort.</span><span class="sxs-lookup"><span data-stu-id="31143-195">Storm topologies continue running until they are stopped or hello cluster is deleted.</span></span>


## <a name="rest-api"></a><span data-ttu-id="31143-196">REST API</span><span class="sxs-lookup"><span data-stu-id="31143-196">REST API</span></span>

<span data-ttu-id="31143-197">hello Storm-Användargränssnittet är byggt på hello REST-API, så att du kan göra liknande hantering och övervakning av funktioner med hjälp av hello REST API.</span><span class="sxs-lookup"><span data-stu-id="31143-197">hello Storm UI is built on top of hello REST API, so you can perform similar management and monitoring functionality by using hello REST API.</span></span> <span data-ttu-id="31143-198">Du kan använda hello REST API toocreate anpassade verktyg för att hantera och övervaka Storm-topologier.</span><span class="sxs-lookup"><span data-stu-id="31143-198">You can use hello REST API toocreate custom tools for managing and monitoring Storm topologies.</span></span>

<span data-ttu-id="31143-199">Mer information finns i [Storm UI REST API](https://github.com/apache/storm/blob/0.9.3-branch/STORM-UI-REST-API.md).</span><span class="sxs-lookup"><span data-stu-id="31143-199">For more information, see [Storm UI REST API](https://github.com/apache/storm/blob/0.9.3-branch/STORM-UI-REST-API.md).</span></span> <span data-ttu-id="31143-200">hello är följande uppgifter specifika toousing hello REST-API med Apache Storm på HDInsight.</span><span class="sxs-lookup"><span data-stu-id="31143-200">hello following information is specific toousing hello REST API with Apache Storm on HDInsight.</span></span>

### <a name="base-uri"></a><span data-ttu-id="31143-201">Bas-URI</span><span class="sxs-lookup"><span data-stu-id="31143-201">Base URI</span></span>

<span data-ttu-id="31143-202">hello bas-URI för hello REST API på HDInsight-kluster är **https://&lt;klusternamn >.azurehdinsight.net/stormui/api/v1/**, där **klusternamn** är hello namnet på ditt Storm på HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="31143-202">hello base URI for hello REST API on HDInsight clusters is **https://&lt;clustername>.azurehdinsight.net/stormui/api/v1/**, where **clustername** is hello name of your Storm on HDInsight cluster.</span></span>

### <a name="authentication"></a><span data-ttu-id="31143-203">Autentisering</span><span class="sxs-lookup"><span data-stu-id="31143-203">Authentication</span></span>

<span data-ttu-id="31143-204">Begäranden toohello REST API måste använda **grundläggande autentisering**, så du använda hello HDInsight-kluster administratörens namn och lösenord.</span><span class="sxs-lookup"><span data-stu-id="31143-204">Requests toohello REST API must use **basic authentication**, so you use hello HDInsight cluster administrator name and password.</span></span>

> [!NOTE]
> <span data-ttu-id="31143-205">Eftersom grundläggande autentisering skickas i klartext, bör du **alltid** använder toosecure HTTPS-kommunikation med hello-kluster.</span><span class="sxs-lookup"><span data-stu-id="31143-205">Because basic authentication is sent by using clear text, you should **always** use HTTPS toosecure communications with hello cluster.</span></span>

### <a name="return-values"></a><span data-ttu-id="31143-206">Returnera värden</span><span class="sxs-lookup"><span data-stu-id="31143-206">Return values</span></span>

<span data-ttu-id="31143-207">Information som returneras från hello REST-API kan endast användas från inom hello kluster eller virtuella datorer på hello samma virtuella Azure-nätverk som hello kluster.</span><span class="sxs-lookup"><span data-stu-id="31143-207">Information that is returned from hello REST API may only be usable from within hello cluster or virtual machines on hello same Azure Virtual Network as hello cluster.</span></span> <span data-ttu-id="31143-208">Hello fullständigt kvalificerade domännamnet (FQDN) returneras för Zookeeper-servrar är till exempel inte att komma åt från hello Internet.</span><span class="sxs-lookup"><span data-stu-id="31143-208">For example, hello fully qualified domain name (FQDN) returned for Zookeeper servers are not be accessible from hello Internet.</span></span>

## <a name="next-steps"></a><span data-ttu-id="31143-209">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="31143-209">Next Steps</span></span>

<span data-ttu-id="31143-210">Nu när du har lärt dig hur toodeploy och övervaka topologier med hjälp av hello Storm-instrumentpanelen, lär du dig hur du:</span><span class="sxs-lookup"><span data-stu-id="31143-210">Now that you've learned how toodeploy and monitor topologies by using hello Storm Dashboard, learn how to:</span></span>

* [<span data-ttu-id="31143-211">Utveckla C#-topologier med hello HDInsight Tools för Visual Studio</span><span class="sxs-lookup"><span data-stu-id="31143-211">Develop C# topologies using hello HDInsight Tools for Visual Studio</span></span>](hdinsight-storm-develop-csharp-visual-studio-topology.md)

* [<span data-ttu-id="31143-212">Utveckla Java-baserad topologier med Maven</span><span class="sxs-lookup"><span data-stu-id="31143-212">Develop Java-based topologies using Maven</span></span>](hdinsight-storm-develop-java-topology.md)

<span data-ttu-id="31143-213">En lista över flera exempeltopologier finns [exempeltopologier för Storm på HDInsight](hdinsight-storm-example-topology.md).</span><span class="sxs-lookup"><span data-stu-id="31143-213">For a list of more example topologies, see [Example topologies for Storm on HDInsight](hdinsight-storm-example-topology.md).</span></span>

[hdinsight-dashboard]: ./media/hdinsight-storm-deploy-monitor-topology/dashboard-link.png
[storm-dashboard-submit]: ./media/hdinsight-storm-deploy-monitor-topology/submit.png
[storm-dashboard-ui]: ./media/hdinsight-storm-deploy-monitor-topology/storm-ui-summary.png
