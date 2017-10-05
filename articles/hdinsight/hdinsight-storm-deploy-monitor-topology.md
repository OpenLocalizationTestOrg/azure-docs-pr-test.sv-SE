---
title: "Distribuera och hantera Apache Storm-topologier på HDInsight | Microsoft Docs"
description: "Lär dig mer om att distribuera, övervaka och hantera Apache Storm-topologier med Storm-instrumentpanelen på HDInsight. Använda Hadoop-verktyg för Visual Studio."
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
ms.openlocfilehash: 34072574f83b51280e60e2f8766c6c5d5a33c307
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="deploy-and-manage-apache-storm-topologies-on-windows-based-hdinsight"></a><span data-ttu-id="900bf-104">Distribuera och hantera Apache Storm-topologier på Windows-baserade HDInsight</span><span class="sxs-lookup"><span data-stu-id="900bf-104">Deploy and manage Apache Storm topologies on Windows-based HDInsight</span></span>

<span data-ttu-id="900bf-105">Storm-instrumentpanelen kan du enkelt distribuera och köra Apache Storm-topologier i HDInsight-kluster med hjälp av webbläsaren.</span><span class="sxs-lookup"><span data-stu-id="900bf-105">The Storm Dashboard allows you to easily deploy and run Apache Storm topologies to your HDInsight cluster by using your web browser.</span></span> <span data-ttu-id="900bf-106">Du kan också använda instrumentpanelen för att övervaka och hantera topologier som körs.</span><span class="sxs-lookup"><span data-stu-id="900bf-106">You can also use the dashboard to monitor and manage running topologies.</span></span> <span data-ttu-id="900bf-107">Om du använder Visual Studio innehåller HDInsight Tools för Visual Studio liknande funktioner i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="900bf-107">If you use Visual Studio, the HDInsight Tools for Visual Studio provide similar features in Visual Studio.</span></span>

<span data-ttu-id="900bf-108">Storm-instrumentpanelen och Storm-funktioner i HDInsight Tools beroende Storm REST-API, som kan användas för att skapa en egen övervakning och av hanteringslösningar.</span><span class="sxs-lookup"><span data-stu-id="900bf-108">The Storm Dashboard and the Storm features in the HDInsight Tools rely on the Storm REST API, which can be used to create your own monitoring and management solutions.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="900bf-109">Stegen i det här dokumentet kräver ett Storm på HDInsight-kluster som använder Windows som operativsystemet.</span><span class="sxs-lookup"><span data-stu-id="900bf-109">The steps in this document require a Storm on HDInsight cluster that uses Windows as the operating system.</span></span> <span data-ttu-id="900bf-110">Linux är det enda operativsystemet som används med HDInsight version 3.4 och senare.</span><span class="sxs-lookup"><span data-stu-id="900bf-110">Linux is the only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="900bf-111">Mer information finns i [HDInsight-avveckling på Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="900bf-111">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>
>
> <span data-ttu-id="900bf-112">Mer information om distribuera och hantera Storm-topologier med ett HDInsight-kluster som använder Linux finns [distribuera och hantera Apache Storm-topologier på Linux-baserat HDInsight](hdinsight-storm-deploy-monitor-topology-linux.md)</span><span class="sxs-lookup"><span data-stu-id="900bf-112">For information on deploying and managing Storm topologies with an HDInsight cluster that uses Linux, see [Deploy and manage Apache Storm topologies on Linux-based HDInsight](hdinsight-storm-deploy-monitor-topology-linux.md)</span></span>

## <a name="prerequisites"></a><span data-ttu-id="900bf-113">Krav</span><span class="sxs-lookup"><span data-stu-id="900bf-113">Prerequisites</span></span>

* <span data-ttu-id="900bf-114">**Apache Storm på HDInsight** -finns [Kom igång med Apache Storm på HDInsight](hdinsight-apache-storm-tutorial-get-started.md) anvisningar om hur du skapar ett kluster.</span><span class="sxs-lookup"><span data-stu-id="900bf-114">**Apache Storm on HDInsight** - see [Get started with Apache Storm on HDInsight](hdinsight-apache-storm-tutorial-get-started.md) for steps on creating a cluster.</span></span>

* <span data-ttu-id="900bf-115">För den **Storm-instrumentpanelen**: en modern webbläsare som stöder HTML5.</span><span class="sxs-lookup"><span data-stu-id="900bf-115">For the **Storm Dashboard**: A modern web browser that supports HTML5.</span></span>

* <span data-ttu-id="900bf-116">För **Visual Studio** -Azure SDK 2.5.1 eller senare och HDInsight Tools för Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="900bf-116">For **Visual Studio** - Azure SDK 2.5.1 or newer and the HDInsight Tools for Visual Studio.</span></span> <span data-ttu-id="900bf-117">Se [komma igång med HDInsight Tools för Visual Studio](hdinsight-hadoop-visual-studio-tools-get-started.md) att installera och konfigurera HDInsight tools för Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="900bf-117">See [Get started using HDInsight Tools for Visual Studio](hdinsight-hadoop-visual-studio-tools-get-started.md) to install and configure the HDInsight tools for Visual Studio.</span></span>

    <span data-ttu-id="900bf-118">En av följande versioner av Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="900bf-118">One of the following versions of Visual Studio:</span></span>

  * <span data-ttu-id="900bf-119">Visual Studio 2012 med Update 4</span><span class="sxs-lookup"><span data-stu-id="900bf-119">Visual Studio 2012 with Update 4</span></span>

  * <span data-ttu-id="900bf-120">Visual Studio 2013 med Update 4 eller Visual Studio Community 2013</span><span class="sxs-lookup"><span data-stu-id="900bf-120">Visual Studio 2013 with Update 4 or Visual Studio 2013 Community</span></span>

  * <span data-ttu-id="900bf-121">Visual Studio 2015 (någon utgåva)</span><span class="sxs-lookup"><span data-stu-id="900bf-121">Visual Studio 2015 (any edition)</span></span>

  * <span data-ttu-id="900bf-122">2017 för Visual Studio (någon utgåva)</span><span class="sxs-lookup"><span data-stu-id="900bf-122">Visual Studio 2017 (any edition)</span></span>

## <a name="storm-dashboard"></a><span data-ttu-id="900bf-123">Storm-instrumentpanelen</span><span class="sxs-lookup"><span data-stu-id="900bf-123">Storm Dashboard</span></span>

<span data-ttu-id="900bf-124">Storm-instrumentpanelen är en webbsida som är tillgängliga på Storm-kluster.</span><span class="sxs-lookup"><span data-stu-id="900bf-124">The Storm Dashboard is a web page available on your Storm cluster.</span></span> <span data-ttu-id="900bf-125">URL-Adressen är **https://&lt;klusternamn >.azurehdinsight.net/**, där **klusternamn** är namnet på ditt Storm på HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="900bf-125">The URL is **https://&lt;clustername>.azurehdinsight.net/**, where **clustername** is the name of your Storm on HDInsight cluster.</span></span>

<span data-ttu-id="900bf-126">Överkant Storm-instrumentpanelen, Välj **skicka topologi**.</span><span class="sxs-lookup"><span data-stu-id="900bf-126">From the top of the Storm Dashboard, select **Submit Topology**.</span></span> <span data-ttu-id="900bf-127">Följ anvisningarna på sidan för att köra en exempeltopologi eller för att överföra och köra en topologi som du skapade.</span><span class="sxs-lookup"><span data-stu-id="900bf-127">Follow the instructions on the page to run a sample topology or to upload and run a topology that you created.</span></span>

![sidan skicka topologi][storm-dashboard-submit]

### <a name="storm-ui"></a><span data-ttu-id="900bf-129">Storm UI</span><span class="sxs-lookup"><span data-stu-id="900bf-129">Storm UI</span></span>

<span data-ttu-id="900bf-130">Storm-instrumentpanelen, Välj den **Storm-Användargränssnittet** länk.</span><span class="sxs-lookup"><span data-stu-id="900bf-130">From the Storm Dashboard, select the **Storm UI** link.</span></span> <span data-ttu-id="900bf-131">Visar information om klustret, förutom alla topologier som körs.</span><span class="sxs-lookup"><span data-stu-id="900bf-131">This displays information about the cluster, in addition to any running topologies.</span></span>

![storm-användargränssnittet][storm-dashboard-ui]

> [!NOTE]
> <span data-ttu-id="900bf-133">I vissa versioner av Internet Explorer kan du identifiera Storm-Användargränssnittet inte uppdaterar efter att först har besökt.</span><span class="sxs-lookup"><span data-stu-id="900bf-133">With some versions of Internet Explorer, you may discover that the Storm UI does not refresh after you have first visited it.</span></span> <span data-ttu-id="900bf-134">Det kan till exempel inte visa nya topologier du skickat, eller den kan visa en topologi som aktiv när du tidigare har inaktiverats.</span><span class="sxs-lookup"><span data-stu-id="900bf-134">For example, it may not show the new topologies you submitted, or it may show a topology as active when you previously deactivated it.</span></span> <span data-ttu-id="900bf-135">Microsoft är medveten om det här problemet och arbetar på en lösning.</span><span class="sxs-lookup"><span data-stu-id="900bf-135">Microsoft is aware of this issue and is working on a solution.</span></span>

#### <a name="main-page"></a><span data-ttu-id="900bf-136">Huvudsida</span><span class="sxs-lookup"><span data-stu-id="900bf-136">Main page</span></span>

<span data-ttu-id="900bf-137">Huvudsidan för Storm-Användargränssnittet innehåller följande information:</span><span class="sxs-lookup"><span data-stu-id="900bf-137">The main page of the Storm UI provides the following information:</span></span>

* <span data-ttu-id="900bf-138">**Översikt över kluster**: grundläggande information om Storm-kluster.</span><span class="sxs-lookup"><span data-stu-id="900bf-138">**Cluster summary**: Basic information about the Storm cluster.</span></span>

* <span data-ttu-id="900bf-139">**Översikt över topologi**: en lista över topologier som körs.</span><span class="sxs-lookup"><span data-stu-id="900bf-139">**Topology summary**: A list of running topologies.</span></span> <span data-ttu-id="900bf-140">Använd länkarna i det här avsnittet om du vill visa mer information om specifika topologier.</span><span class="sxs-lookup"><span data-stu-id="900bf-140">Use the links in this section to view more information about specific topologies.</span></span>

* <span data-ttu-id="900bf-141">**Översikt över handledarens**: Information om Storm-administratören.</span><span class="sxs-lookup"><span data-stu-id="900bf-141">**Supervisor summary**: Information about the Storm supervisor.</span></span>

* <span data-ttu-id="900bf-142">**Nimbus configuration**: Nimbus-konfiguration för klustret.</span><span class="sxs-lookup"><span data-stu-id="900bf-142">**Nimbus configuration**: Nimbus configuration for the cluster.</span></span>

#### <a name="topology-summary"></a><span data-ttu-id="900bf-143">Topologi sammanfattning</span><span class="sxs-lookup"><span data-stu-id="900bf-143">Topology summary</span></span>

<span data-ttu-id="900bf-144">Att välja en länk från den **Topology summary** avsnitt visar följande information om topologin:</span><span class="sxs-lookup"><span data-stu-id="900bf-144">Selecting a link from the **Topology summary** section displays the following information about the topology:</span></span>

* <span data-ttu-id="900bf-145">**Översikt över topologi**: grundläggande information om topologin.</span><span class="sxs-lookup"><span data-stu-id="900bf-145">**Topology summary**: Basic information about the topology.</span></span>

* <span data-ttu-id="900bf-146">**Topologi åtgärder**: hanteringsåtgärder som du kan utföra för topologin.</span><span class="sxs-lookup"><span data-stu-id="900bf-146">**Topology actions**: Management actions that you can perform for the topology.</span></span>

  * <span data-ttu-id="900bf-147">**Aktivera**: återupptar bearbetningen av en inaktiverad topologi.</span><span class="sxs-lookup"><span data-stu-id="900bf-147">**Activate**: Resumes processing of a deactivated topology.</span></span>

  * <span data-ttu-id="900bf-148">**Inaktivera**: pausar en topologi som körs.</span><span class="sxs-lookup"><span data-stu-id="900bf-148">**Deactivate**: Pauses a running topology.</span></span>

  * <span data-ttu-id="900bf-149">**Balansera**: justerar topologins parallellitet.</span><span class="sxs-lookup"><span data-stu-id="900bf-149">**Rebalance**: Adjusts the parallelism of the topology.</span></span> <span data-ttu-id="900bf-150">Du bör balansera om topologier som körs när du har ändrat antalet noder i klustret.</span><span class="sxs-lookup"><span data-stu-id="900bf-150">You should rebalance running topologies after you have changed the number of nodes in the cluster.</span></span> <span data-ttu-id="900bf-151">Detta gör att topologin kan justera parallelliteten och kompensera för ökar eller minskar antalet noder i klustret.</span><span class="sxs-lookup"><span data-stu-id="900bf-151">This allows the topology to adjust parallelism to compensate for the increased or decreased number of nodes in the cluster.</span></span>

      <span data-ttu-id="900bf-152">Mer information finns i [förstå parallellitet i en Storm-topologi (http://storm.apache.org/documentation/Understanding-the-parallelism-of-a-Storm-topology.html)](http://storm.apache.org/documentation/Understanding-the-parallelism-of-a-Storm-topology.html).</span><span class="sxs-lookup"><span data-stu-id="900bf-152">For more information, see [Understanding the parallelism of a Storm topology (http://storm.apache.org/documentation/Understanding-the-parallelism-of-a-Storm-topology.html)](http://storm.apache.org/documentation/Understanding-the-parallelism-of-a-Storm-topology.html).</span></span>

  * <span data-ttu-id="900bf-153">**Avsluta**: avslutar en Storm-topologi efter en angiven tidsgräns.</span><span class="sxs-lookup"><span data-stu-id="900bf-153">**Kill**: Terminates a Storm topology after the specified timeout.</span></span>

* <span data-ttu-id="900bf-154">**Topology stats**: statistik om topologin.</span><span class="sxs-lookup"><span data-stu-id="900bf-154">**Topology stats**: Statistics about the topology.</span></span> <span data-ttu-id="900bf-155">Länkarna i den **fönstret** kolumnen för att ange tidsperioden för de återstående posterna på sidan.</span><span class="sxs-lookup"><span data-stu-id="900bf-155">Use the links in the **Window** column to set the timeframe for the remaining entries on the page.</span></span>

* <span data-ttu-id="900bf-156">**Spouts**: kanaler som används av topologin.</span><span class="sxs-lookup"><span data-stu-id="900bf-156">**Spouts**: The spouts used by the topology.</span></span> <span data-ttu-id="900bf-157">Använd länkarna i det här avsnittet om du vill visa mer information om specifika kanaler.</span><span class="sxs-lookup"><span data-stu-id="900bf-157">Use the links in this section to view more information about specific spouts.</span></span>

* <span data-ttu-id="900bf-158">**Bolts**: bultar som används av topologin.</span><span class="sxs-lookup"><span data-stu-id="900bf-158">**Bolts**: The bolts used by the topology.</span></span> <span data-ttu-id="900bf-159">Använd länkarna i det här avsnittet om du vill visa mer information om specifika bultar.</span><span class="sxs-lookup"><span data-stu-id="900bf-159">Use the links in this section to view more information about specific bolts.</span></span>

* <span data-ttu-id="900bf-160">**Topologi configuration**: konfigurationen av den valda topologin.</span><span class="sxs-lookup"><span data-stu-id="900bf-160">**Topology configuration**: The configuration of the selected topology.</span></span>

#### <a name="spout-and-bolt-summary"></a><span data-ttu-id="900bf-161">Kanal och bult sammanfattning</span><span class="sxs-lookup"><span data-stu-id="900bf-161">Spout and Bolt summary</span></span>

<span data-ttu-id="900bf-162">Att välja en kanal från den **Spouts** eller **Bolts** avsnitt visar följande information om det markerade objektet:</span><span class="sxs-lookup"><span data-stu-id="900bf-162">Selecting a spout from the **Spouts** or **Bolts** sections displays the following information about the selected item:</span></span>

* <span data-ttu-id="900bf-163">**Sammanfattning**: grundläggande information om den kanal eller en bult.</span><span class="sxs-lookup"><span data-stu-id="900bf-163">**Component summary**: Basic information about the spout or bolt.</span></span>

* <span data-ttu-id="900bf-164">**Kanal/bult stats**: statistik om den kanal eller en bult.</span><span class="sxs-lookup"><span data-stu-id="900bf-164">**Spout/Bolt stats**: Statistics about the spout or bolt.</span></span> <span data-ttu-id="900bf-165">Länkarna i den **fönstret** kolumnen för att ange tidsperioden för de återstående posterna på sidan.</span><span class="sxs-lookup"><span data-stu-id="900bf-165">Use the links in the **Window** column to set the timeframe for the remaining entries on the page.</span></span>

* <span data-ttu-id="900bf-166">**Input stats** (endast bultar): Information om de inkommande dataströmmar som används av bulten.</span><span class="sxs-lookup"><span data-stu-id="900bf-166">**Input stats** (bolt only): Information about the input streams consumed by the bolt.</span></span>

* <span data-ttu-id="900bf-167">**Utdata stats**: Information om strömmar orsakat av detta kanal eller bultar.</span><span class="sxs-lookup"><span data-stu-id="900bf-167">**Output stats**: Information about the streams emitted by this spout or bolt.</span></span>

* <span data-ttu-id="900bf-168">**Executors**: Information om instanser av den kanal eller en bult.</span><span class="sxs-lookup"><span data-stu-id="900bf-168">**Executors**: Information about the instances of the spout or bolt.</span></span> <span data-ttu-id="900bf-169">Välj den **Port** post för en specifik utförare att visa en logg över diagnostikinformation produceras för den här instansen.</span><span class="sxs-lookup"><span data-stu-id="900bf-169">Select the **Port** entry for a specific executor to view a log of diagnostic information produced for this instance.</span></span>

* <span data-ttu-id="900bf-170">**Fel**: felinformation för den här kanalen eller bultar.</span><span class="sxs-lookup"><span data-stu-id="900bf-170">**Errors**: Any error information for this spout or bolt.</span></span>

## <a name="hdinsight-tools-for-visual-studio"></a><span data-ttu-id="900bf-171">HDInsight Tools för Visual Studio</span><span class="sxs-lookup"><span data-stu-id="900bf-171">HDInsight Tools for Visual Studio</span></span>

<span data-ttu-id="900bf-172">HDInsight-verktyg kan användas för att skicka eller hybrid C#-topologier till ditt Storm-kluster.</span><span class="sxs-lookup"><span data-stu-id="900bf-172">The HDInsight Tools can be used to submit C# or hybrid topologies to your Storm cluster.</span></span> <span data-ttu-id="900bf-173">Följande steg använder ett exempelprogram.</span><span class="sxs-lookup"><span data-stu-id="900bf-173">The following steps use a sample application.</span></span> <span data-ttu-id="900bf-174">Information om hur du skapar egna topologier med HDInsight Tools finns [utveckla C#-topologier med HDInsight Tools för Visual Studio](hdinsight-storm-develop-csharp-visual-studio-topology.md).</span><span class="sxs-lookup"><span data-stu-id="900bf-174">For information about creating your own topologies by using the HDInsight Tools, see [Develop C# topologies using the HDInsight Tools for Visual Studio](hdinsight-storm-develop-csharp-visual-studio-topology.md).</span></span>

<span data-ttu-id="900bf-175">Använd följande steg när du distribuerar ett exempel på till ditt Storm på HDInsight-kluster och sedan visa och hantera topologin.</span><span class="sxs-lookup"><span data-stu-id="900bf-175">Use the following steps to deploy a sample to your Storm on HDInsight cluster, then view and manage the topology.</span></span>

1. <span data-ttu-id="900bf-176">Om du inte redan har installerat den senaste versionen av HDInsight Tools för Visual Studio finns [komma igång med HDInsight Tools för Visual Studio](hdinsight-hadoop-visual-studio-tools-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="900bf-176">If you have not already installed the latest version of the HDInsight Tools for Visual Studio, see [Get started using HDInsight Tools for Visual Studio](hdinsight-hadoop-visual-studio-tools-get-started.md).</span></span>

2. <span data-ttu-id="900bf-177">Öppna Visual Studio, markera **filen** > **ny** > **projekt**.</span><span class="sxs-lookup"><span data-stu-id="900bf-177">Open Visual Studio, select **File** > **New** > **Project**.</span></span>

3. <span data-ttu-id="900bf-178">I den **nytt projekt** dialogrutan Expandera **installerad** > **mallar**, och välj sedan **HDInsight**.</span><span class="sxs-lookup"><span data-stu-id="900bf-178">In the **New Project** dialog box, expand **Installed** > **Templates**, and then select **HDInsight**.</span></span> <span data-ttu-id="900bf-179">Välj i listan över mallar **Storm exempel**.</span><span class="sxs-lookup"><span data-stu-id="900bf-179">From the list of templates, select **Storm Sample**.</span></span> <span data-ttu-id="900bf-180">Ange ett namn för programmet längst ned i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="900bf-180">At the bottom of the dialog box, type a name for the application.</span></span>

    ![Bild](./media/hdinsight-storm-deploy-monitor-topology/sample.png)

4. <span data-ttu-id="900bf-182">I **Solution Explorer**, högerklicka på projektet och välj **skicka till Storm på HDInsight**.</span><span class="sxs-lookup"><span data-stu-id="900bf-182">In **Solution Explorer**, right-click the project, and select **Submit to Storm on HDInsight**.</span></span>

   > [!NOTE]
   > <span data-ttu-id="900bf-183">Om du uppmanas ange inloggningsuppgifterna för din Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="900bf-183">If prompted, enter the login credentials for your Azure subscription.</span></span> <span data-ttu-id="900bf-184">Om du har mer än en prenumeration kan du logga in på den tabell som innehåller ditt Storm på HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="900bf-184">If you have more than one subscription, log in to the one that contains your Storm on HDInsight cluster.</span></span>

5. <span data-ttu-id="900bf-185">Välj ditt Storm på HDInsight-kluster från de **Storm-kluster** listrutan och välj sedan **skicka**.</span><span class="sxs-lookup"><span data-stu-id="900bf-185">Select your Storm on HDInsight cluster from the **Storm Cluster** drop-down list, and then select **Submit**.</span></span> <span data-ttu-id="900bf-186">Du kan övervaka om överföring har genomförts med den **utdata** fönster.</span><span class="sxs-lookup"><span data-stu-id="900bf-186">You can monitor whether the submission is successful by using the **Output** window.</span></span>

6. <span data-ttu-id="900bf-187">När topologin har skickats, den **Storm-topologier** för klustret ska visas.</span><span class="sxs-lookup"><span data-stu-id="900bf-187">When the topology has been successfully submitted, the **Storm Topologies** for the cluster should appear.</span></span> <span data-ttu-id="900bf-188">Välj topologi från listan för att visa information om topologin som körs.</span><span class="sxs-lookup"><span data-stu-id="900bf-188">Select the topology from the list to view information about the running topology.</span></span>

    ![Övervakare för Visual studio](./media/hdinsight-storm-deploy-monitor-topology/vsmonitor.png)

   > [!NOTE]
   > <span data-ttu-id="900bf-190">Du kan också visa **Storm-topologier** från **Server Explorer** genom att expandera **Azure** > **HDInsight**, och sedan Högerklicka på ett Storm på HDInsight-kluster och välja **visa Storm-topologier**.</span><span class="sxs-lookup"><span data-stu-id="900bf-190">You can also view **Storm Topologies** from **Server Explorer** by expanding **Azure** > **HDInsight**, and then right-clicking a Storm on HDInsight cluster, and selecting **View Storm Topologies**.</span></span>

    <span data-ttu-id="900bf-191">Markera formen för kanaler eller bultar att visa information om dessa komponenter.</span><span class="sxs-lookup"><span data-stu-id="900bf-191">Select the shape for the spouts or bolts to view information about these components.</span></span> <span data-ttu-id="900bf-192">För varje vald öppnas ett nytt fönster.</span><span class="sxs-lookup"><span data-stu-id="900bf-192">A new window opens for each item selected.</span></span>

   > [!NOTE]
   > <span data-ttu-id="900bf-193">Namnet på topologin är klassnamnet för topologin (i det här fallet `HelloWord`,) med en tidstämpel läggas till.</span><span class="sxs-lookup"><span data-stu-id="900bf-193">The name of the topology is the class name of the topology (in this case, `HelloWord`,) with a timestamp appended.</span></span>

7. <span data-ttu-id="900bf-194">Från den **Topology Summary** väljer **Kill** att stoppa topologin.</span><span class="sxs-lookup"><span data-stu-id="900bf-194">From the **Topology Summary** view, select **Kill** to stop the topology.</span></span>

   > [!NOTE]
   > <span data-ttu-id="900bf-195">Storm-topologier fortsätta köras förrän de har stoppats eller ta bort klustret.</span><span class="sxs-lookup"><span data-stu-id="900bf-195">Storm topologies continue running until they are stopped or the cluster is deleted.</span></span>


## <a name="rest-api"></a><span data-ttu-id="900bf-196">REST API</span><span class="sxs-lookup"><span data-stu-id="900bf-196">REST API</span></span>

<span data-ttu-id="900bf-197">Storm-Användargränssnittet är byggt på REST-API, så att du kan göra liknande hantering och övervakning av funktioner med hjälp av REST API.</span><span class="sxs-lookup"><span data-stu-id="900bf-197">The Storm UI is built on top of the REST API, so you can perform similar management and monitoring functionality by using the REST API.</span></span> <span data-ttu-id="900bf-198">Du kan använda REST API för att skapa anpassade verktyg för att hantera och övervaka Storm-topologier.</span><span class="sxs-lookup"><span data-stu-id="900bf-198">You can use the REST API to create custom tools for managing and monitoring Storm topologies.</span></span>

<span data-ttu-id="900bf-199">Mer information finns i [Storm UI REST API](https://github.com/apache/storm/blob/0.9.3-branch/STORM-UI-REST-API.md).</span><span class="sxs-lookup"><span data-stu-id="900bf-199">For more information, see [Storm UI REST API](https://github.com/apache/storm/blob/0.9.3-branch/STORM-UI-REST-API.md).</span></span> <span data-ttu-id="900bf-200">Följande information gäller med hjälp av REST-API med Apache Storm på HDInsight.</span><span class="sxs-lookup"><span data-stu-id="900bf-200">The following information is specific to using the REST API with Apache Storm on HDInsight.</span></span>

### <a name="base-uri"></a><span data-ttu-id="900bf-201">Bas-URI</span><span class="sxs-lookup"><span data-stu-id="900bf-201">Base URI</span></span>

<span data-ttu-id="900bf-202">Bas-URI för REST-API på HDInsight-kluster är **https://&lt;klusternamn >.azurehdinsight.net/stormui/api/v1/**, där **klusternamn** är namnet på ditt Storm på HDInsight kluster.</span><span class="sxs-lookup"><span data-stu-id="900bf-202">The base URI for the REST API on HDInsight clusters is **https://&lt;clustername>.azurehdinsight.net/stormui/api/v1/**, where **clustername** is the name of your Storm on HDInsight cluster.</span></span>

### <a name="authentication"></a><span data-ttu-id="900bf-203">Autentisering</span><span class="sxs-lookup"><span data-stu-id="900bf-203">Authentication</span></span>

<span data-ttu-id="900bf-204">REST API-begäranden måste använda **grundläggande autentisering**, så att du använder HDInsight-klustrets administratörsnamn och lösenord.</span><span class="sxs-lookup"><span data-stu-id="900bf-204">Requests to the REST API must use **basic authentication**, so you use the HDInsight cluster administrator name and password.</span></span>

> [!NOTE]
> <span data-ttu-id="900bf-205">Eftersom grundläggande autentisering skickas i klartext, bör du **alltid** använder HTTPS för att skydda kommunikationen med klustret.</span><span class="sxs-lookup"><span data-stu-id="900bf-205">Because basic authentication is sent by using clear text, you should **always** use HTTPS to secure communications with the cluster.</span></span>

### <a name="return-values"></a><span data-ttu-id="900bf-206">Returnera värden</span><span class="sxs-lookup"><span data-stu-id="900bf-206">Return values</span></span>

<span data-ttu-id="900bf-207">Information som returneras från REST-API kan endast användas från i kluster eller virtuella datorer i samma virtuella nätverk med Azure som klustret.</span><span class="sxs-lookup"><span data-stu-id="900bf-207">Information that is returned from the REST API may only be usable from within the cluster or virtual machines on the same Azure Virtual Network as the cluster.</span></span> <span data-ttu-id="900bf-208">Exempelvis det fullständigt kvalificerade domännamnet (FQDN) returneras för Zookeeper-servrar är inte tillgänglig från Internet.</span><span class="sxs-lookup"><span data-stu-id="900bf-208">For example, the fully qualified domain name (FQDN) returned for Zookeeper servers are not be accessible from the Internet.</span></span>

## <a name="next-steps"></a><span data-ttu-id="900bf-209">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="900bf-209">Next Steps</span></span>

<span data-ttu-id="900bf-210">Nu när du har lärt dig hur du distribuerar och övervakar topologier med hjälp av Storm-instrumentpanelen, Läs mer om hur du:</span><span class="sxs-lookup"><span data-stu-id="900bf-210">Now that you've learned how to deploy and monitor topologies by using the Storm Dashboard, learn how to:</span></span>

* [<span data-ttu-id="900bf-211">Utveckla C#-topologier med HDInsight Tools för Visual Studio</span><span class="sxs-lookup"><span data-stu-id="900bf-211">Develop C# topologies using the HDInsight Tools for Visual Studio</span></span>](hdinsight-storm-develop-csharp-visual-studio-topology.md)

* [<span data-ttu-id="900bf-212">Utveckla Java-baserad topologier med Maven</span><span class="sxs-lookup"><span data-stu-id="900bf-212">Develop Java-based topologies using Maven</span></span>](hdinsight-storm-develop-java-topology.md)

<span data-ttu-id="900bf-213">En lista över flera exempeltopologier finns [exempeltopologier för Storm på HDInsight](hdinsight-storm-example-topology.md).</span><span class="sxs-lookup"><span data-stu-id="900bf-213">For a list of more example topologies, see [Example topologies for Storm on HDInsight](hdinsight-storm-example-topology.md).</span></span>

[hdinsight-dashboard]: ./media/hdinsight-storm-deploy-monitor-topology/dashboard-link.png
[storm-dashboard-submit]: ./media/hdinsight-storm-deploy-monitor-topology/submit.png
[storm-dashboard-ui]: ./media/hdinsight-storm-deploy-monitor-topology/storm-ui-summary.png
