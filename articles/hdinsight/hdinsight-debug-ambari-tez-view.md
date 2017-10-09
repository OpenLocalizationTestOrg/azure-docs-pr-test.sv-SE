---
title: aaaUse Ambari Tez vy med HDInsight - Azure | Microsoft Docs
description: "Lär dig hur toouse hello Ambari Tez visa toodebug Tez jobb i HDInsight."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.assetid: 9c39ea56-670b-4699-aba0-0f64c261e411
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 07/12/2017
ms.author: larryfr
ms.openlocfilehash: 5d61bd0403c98284c86982073af91468ae62ac60
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-ambari-views-toodebug-tez-jobs-on-hdinsight"></a><span data-ttu-id="71a67-103">Använda Ambari Views toodebug Tez jobb i HDInsight</span><span class="sxs-lookup"><span data-stu-id="71a67-103">Use Ambari Views toodebug Tez Jobs on HDInsight</span></span>

<span data-ttu-id="71a67-104">Hej Ambari-Webbgränssnittet för HDInsight innehåller en vy i Tez som kan använda toounderstand och felsöka jobb som använder Tez.</span><span class="sxs-lookup"><span data-stu-id="71a67-104">hello Ambari Web UI for HDInsight contains a Tez view that can be used toounderstand and debug jobs that use Tez.</span></span> <span data-ttu-id="71a67-105">Hej Tez vy kan du toovisualize hello jobb som ett diagram över anslutna objekt detaljer om varje objekt och hämta statistik och loggningsinformation.</span><span class="sxs-lookup"><span data-stu-id="71a67-105">hello Tez view allows you toovisualize hello job as a graph of connected items, drill into each item, and retrieve statistics and logging information.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="71a67-106">hello stegen i det här dokumentet kräver ett HDInsight-kluster som använder Linux.</span><span class="sxs-lookup"><span data-stu-id="71a67-106">hello steps in this document require an HDInsight cluster that uses Linux.</span></span> <span data-ttu-id="71a67-107">Linux är hello endast operativsystem på HDInsight version 3.4 eller senare.</span><span class="sxs-lookup"><span data-stu-id="71a67-107">Linux is hello only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="71a67-108">Mer information finns i [HDInsight component-versioning](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="71a67-108">For more information, see [HDInsight component versioning](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="71a67-109">Krav</span><span class="sxs-lookup"><span data-stu-id="71a67-109">Prerequisites</span></span>

* <span data-ttu-id="71a67-110">Ett Linux-baserat HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="71a67-110">A Linux-based HDInsight cluster.</span></span> <span data-ttu-id="71a67-111">Anvisningar om hur du skapar ett kluster finns [komma igång med Linux-baserat HDInsight](hdinsight-hadoop-linux-tutorial-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="71a67-111">For steps on creating a cluster, see [Get started using Linux-based HDInsight](hdinsight-hadoop-linux-tutorial-get-started.md).</span></span>
* <span data-ttu-id="71a67-112">En modern webbläsare som stöder HTML5.</span><span class="sxs-lookup"><span data-stu-id="71a67-112">A modern web browser that supports HTML5.</span></span>

## <a name="understanding-tez"></a><span data-ttu-id="71a67-113">Förstå Tez</span><span class="sxs-lookup"><span data-stu-id="71a67-113">Understanding Tez</span></span>

<span data-ttu-id="71a67-114">Tez är ett utökningsbart ramverk för databearbetning i Hadoop och som ger högre hastigheter än traditionella MapReduce-bearbetning.</span><span class="sxs-lookup"><span data-stu-id="71a67-114">Tez is an extensible framework for data processing in Hadoop that provides greater speeds than traditional MapReduce processing.</span></span> <span data-ttu-id="71a67-115">För Linux-baserade HDInsight-kluster är det hello standardmotorn för Hive.</span><span class="sxs-lookup"><span data-stu-id="71a67-115">For Linux-based HDInsight clusters, it is hello default engine for Hive.</span></span>

<span data-ttu-id="71a67-116">Tez skapar en dirigeras acykliska diagram (DAG) som beskriver hello ordning för åtgärder som krävs av jobb.</span><span class="sxs-lookup"><span data-stu-id="71a67-116">Tez creates a Directed Acyclic Graph (DAG) that describes hello order of actions required by jobs.</span></span> <span data-ttu-id="71a67-117">Enskilda åtgärder kallas formhörnen och köra en typ av hello övergripande jobb.</span><span class="sxs-lookup"><span data-stu-id="71a67-117">Individual actions are called vertices, and execute a piece of hello overall job.</span></span> <span data-ttu-id="71a67-118">hello faktiska utförande av hello beskrivs av en nod kallas för en aktivitet och kan distribueras över flera noder i klustret hello.</span><span class="sxs-lookup"><span data-stu-id="71a67-118">hello actual execution of hello work described by a vertex is called a task, and may be distributed across multiple nodes in hello cluster.</span></span>

### <a name="understanding-hello-tez-view"></a><span data-ttu-id="71a67-119">Förstå hello Tez vy</span><span class="sxs-lookup"><span data-stu-id="71a67-119">Understanding hello Tez view</span></span>

<span data-ttu-id="71a67-120">Hej Tez-vyn innehåller både historisk information och information om processer som körs.</span><span class="sxs-lookup"><span data-stu-id="71a67-120">hello Tez view provides both historical information and information on processes that are running.</span></span> <span data-ttu-id="71a67-121">Den här informationen visar hur ett jobb är fördelad över kluster.</span><span class="sxs-lookup"><span data-stu-id="71a67-121">This information shows how a job is distributed across clusters.</span></span> <span data-ttu-id="71a67-122">Visar även räknare som används av uppgifter och formhörnen och information om fel relaterade toohello jobb.</span><span class="sxs-lookup"><span data-stu-id="71a67-122">It also displays counters used by tasks and vertices, and error information related toohello job.</span></span> <span data-ttu-id="71a67-123">Den kan erbjuda användbar information i hello följande scenarier:</span><span class="sxs-lookup"><span data-stu-id="71a67-123">It may offer useful information in hello following scenarios:</span></span>

* <span data-ttu-id="71a67-124">Övervaka tidskrävande processer, visa hello förloppet för kartan och minska uppgifter.</span><span class="sxs-lookup"><span data-stu-id="71a67-124">Monitoring long-running processes, viewing hello progress of map and reduce tasks.</span></span>
* <span data-ttu-id="71a67-125">Analysera historiska data för lyckade eller misslyckade processer toolearn hur bearbetning kan förbättras eller orsaken till felet.</span><span class="sxs-lookup"><span data-stu-id="71a67-125">Analyzing historical data for successful or failed processes toolearn how processing could be improved or why it failed.</span></span>

## <a name="generate-a-dag"></a><span data-ttu-id="71a67-126">Generera en DAG</span><span class="sxs-lookup"><span data-stu-id="71a67-126">Generate a DAG</span></span>

<span data-ttu-id="71a67-127">hello Tez-vyn innehåller bara data om ett jobb som använder hello Tez-motorn är för närvarande körs eller har varit kördes tidigare.</span><span class="sxs-lookup"><span data-stu-id="71a67-127">hello Tez view only contains data if a job that uses hello Tez engine is currently running, or has been ran previously.</span></span> <span data-ttu-id="71a67-128">Enkel Hive-frågor kan lösas utan att använda Tez.</span><span class="sxs-lookup"><span data-stu-id="71a67-128">Simple Hive queries can be resolved without using Tez.</span></span> <span data-ttu-id="71a67-129">Mer komplexa frågor som vill filtrering, gruppering, sortering, kopplingar och så vidare.</span><span class="sxs-lookup"><span data-stu-id="71a67-129">More complex queries that do filtering, grouping, ordering, joins, etc.</span></span> <span data-ttu-id="71a67-130">Använd hello Tez-motorn.</span><span class="sxs-lookup"><span data-stu-id="71a67-130">use hello Tez engine.</span></span>

<span data-ttu-id="71a67-131">Använd följande steg toorun en Hive-fråga som använder Tez hello:</span><span class="sxs-lookup"><span data-stu-id="71a67-131">Use hello following steps toorun a Hive query that uses Tez:</span></span>

1. <span data-ttu-id="71a67-132">I en webbläsare, navigerar du toohttps://CLUSTERNAME.azurehdinsight.net, där **KLUSTERNAMN** är hello namnet på ditt HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="71a67-132">In a web browser, navigate toohttps://CLUSTERNAME.azurehdinsight.net, where **CLUSTERNAME** is hello name of your HDInsight cluster.</span></span>

2. <span data-ttu-id="71a67-133">Välj hello hello menyn hello överst på sidan hello **vyer** ikon.</span><span class="sxs-lookup"><span data-stu-id="71a67-133">From hello menu at hello top of hello page, select hello **Views** icon.</span></span> <span data-ttu-id="71a67-134">Den här ikonen som ser ut som en serie rutor.</span><span class="sxs-lookup"><span data-stu-id="71a67-134">This icon looks like a series of squares.</span></span> <span data-ttu-id="71a67-135">I hello listrutan som visas väljer du **Hive visa**.</span><span class="sxs-lookup"><span data-stu-id="71a67-135">In hello dropdown that appears, select **Hive view**.</span></span>

    ![Du har markerat Hive-vy](./media/hdinsight-debug-ambari-tez-view/selecthive.png)

3. <span data-ttu-id="71a67-137">När hello Hive-vyn läses in, klistra in hello följande fråga i hello frågeredigeraren och klicka sedan på **köra**.</span><span class="sxs-lookup"><span data-stu-id="71a67-137">When hello Hive view loads, paste hello following query into hello Query Editor, and then click **execute**.</span></span>

        select market, state, country from hivesampletable where deviceplatform='Android' group by market, country, state;

    <span data-ttu-id="71a67-138">När hello jobbet har slutförts visas hello utdata som visas i hello **Frågeprocessresultat** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="71a67-138">Once hello job has completed, you should see hello output displayed in hello **Query Process Results** section.</span></span> <span data-ttu-id="71a67-139">hello resultaten ska vara liknande toohello följande text:</span><span class="sxs-lookup"><span data-stu-id="71a67-139">hello results should be similar toohello following text:</span></span>

        market  state       country
        en-GB   Hessen      Germany
        en-GB   Kingston    Jamaica

4. <span data-ttu-id="71a67-140">Välj hello **loggen** fliken. Du kan se information liknande toohello följande text:</span><span class="sxs-lookup"><span data-stu-id="71a67-140">Select hello **Log** tab. You see information similar toohello following text:</span></span>

        INFO : Session is already open
        INFO :

        INFO : Status: Running (Executing on YARN cluster with App id application_1454546500517_0063)

    <span data-ttu-id="71a67-141">Spara hello **App-id** värdet som det här värdet används i nästa avsnitt om hello.</span><span class="sxs-lookup"><span data-stu-id="71a67-141">Save hello **App id** value, as this value is used in hello next section.</span></span>

## <a name="use-hello-tez-view"></a><span data-ttu-id="71a67-142">Använd hello Tez vy</span><span class="sxs-lookup"><span data-stu-id="71a67-142">Use hello Tez View</span></span>

1. <span data-ttu-id="71a67-143">Välj hello hello menyn hello överst på sidan hello **vyer** ikon.</span><span class="sxs-lookup"><span data-stu-id="71a67-143">From hello menu at hello top of hello page, select hello **Views** icon.</span></span> <span data-ttu-id="71a67-144">I hello listrutan som visas väljer du **Tez visa**.</span><span class="sxs-lookup"><span data-stu-id="71a67-144">In hello dropdown that appears, select **Tez view**.</span></span>

    ![Du har markerat Tez vy](./media/hdinsight-debug-ambari-tez-view/selecttez.png)

2. <span data-ttu-id="71a67-146">När hello Tez vyn läses in, visas en lista över hive-frågor som körs eller har körts på hello klustret.</span><span class="sxs-lookup"><span data-stu-id="71a67-146">When hello Tez view loads, you see a list of hive queries that are currently running, or have been ran on hello cluster.</span></span>

    ![Alla dag](./media/hdinsight-debug-ambari-tez-view/tez-view-home.png)

3. <span data-ttu-id="71a67-148">Om du har en enda post, är det för hello-fråga som du körde hello föregående avsnitt.</span><span class="sxs-lookup"><span data-stu-id="71a67-148">If you have only one entry, it is for hello query that you ran in hello previous section.</span></span> <span data-ttu-id="71a67-149">Om du har flera poster kan söka du genom att använda hello fält hello överst på hello sidan.</span><span class="sxs-lookup"><span data-stu-id="71a67-149">If you have multiple entries, you can search by using hello fields at hello top of hello page.</span></span>

4. <span data-ttu-id="71a67-150">Välj hello **fråge-ID** för en Hive-fråga.</span><span class="sxs-lookup"><span data-stu-id="71a67-150">Select hello **Query ID** for a Hive query.</span></span> <span data-ttu-id="71a67-151">Information om hello frågan visas.</span><span class="sxs-lookup"><span data-stu-id="71a67-151">Information about hello query is displayed.</span></span>

    ![DAG-information](./media/hdinsight-debug-ambari-tez-view/query-details.png)

5. <span data-ttu-id="71a67-153">hello flikarna på den här sidan kan du tooview hello följande information:</span><span class="sxs-lookup"><span data-stu-id="71a67-153">hello tabs on this page allow you tooview hello following information:</span></span>

    * <span data-ttu-id="71a67-154">**Fråga information**: information om hello Hive-fråga.</span><span class="sxs-lookup"><span data-stu-id="71a67-154">**Query Details**: Details about hello Hive query.</span></span>
    * <span data-ttu-id="71a67-155">**Tidslinjen**: Information om hur lång tid tog av varje steg i processen.</span><span class="sxs-lookup"><span data-stu-id="71a67-155">**Timeline**: Information about how long each stage of processing took.</span></span>
    * <span data-ttu-id="71a67-156">**Konfigurationer**: hello-konfiguration som använts för den här frågan.</span><span class="sxs-lookup"><span data-stu-id="71a67-156">**Configurations**: hello configuration used for this query.</span></span>

    <span data-ttu-id="71a67-157">Från __Frågedetaljer__ du kan använda hello länkar toofind information om hello __programmet__ eller hello __DAG__ för den här frågan.</span><span class="sxs-lookup"><span data-stu-id="71a67-157">From __Query Details__ you can use hello links toofind information about hello __Application__ or hello __DAG__ for this query.</span></span>
    
    * <span data-ttu-id="71a67-158">Hej __programmet__ länken visar information om hello YARN-programmet för den här frågan.</span><span class="sxs-lookup"><span data-stu-id="71a67-158">hello __Application__ link displays information about hello YARN application for this query.</span></span> <span data-ttu-id="71a67-159">Du kan komma åt programmet hello YARN-loggar från den här.</span><span class="sxs-lookup"><span data-stu-id="71a67-159">From here you can access hello YARN application logs.</span></span>
    * <span data-ttu-id="71a67-160">Hej __DAG__ länken visar information om hello riktat acykliskt diagram för den här frågan.</span><span class="sxs-lookup"><span data-stu-id="71a67-160">hello __DAG__ link displays information about hello directed acyclic graph for this query.</span></span> <span data-ttu-id="71a67-161">Härifrån kan du visa en grafisk representation av hello DAG.</span><span class="sxs-lookup"><span data-stu-id="71a67-161">From here you can view a graphical representation of hello DAG.</span></span> <span data-ttu-id="71a67-162">Du kan också hitta information om hello formhörnen i hello DAG.</span><span class="sxs-lookup"><span data-stu-id="71a67-162">You can also find information on hello vertices within hello DAG.</span></span>

## <a name="next-steps"></a><span data-ttu-id="71a67-163">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="71a67-163">Next Steps</span></span>

<span data-ttu-id="71a67-164">Nu när du har lärt dig hur toouse hello Tez visa, lär du dig mer om [med hjälp av Hive i HDInsight](hdinsight-use-hive.md).</span><span class="sxs-lookup"><span data-stu-id="71a67-164">Now that you have learned how toouse hello Tez view, learn more about [Using Hive on HDInsight](hdinsight-use-hive.md).</span></span>

<span data-ttu-id="71a67-165">Mer teknisk information om Tez finns hello [Tez sidan vid Hortonworks](http://hortonworks.com/hadoop/tez/).</span><span class="sxs-lookup"><span data-stu-id="71a67-165">For more detailed technical information on Tez, see hello [Tez page at Hortonworks](http://hortonworks.com/hadoop/tez/).</span></span>

<span data-ttu-id="71a67-166">Mer information om hur du använder Ambari med HDInsight finns [hantera HDInsight-kluster med hello Ambari-Webbgränssnittet](hdinsight-hadoop-manage-ambari.md)</span><span class="sxs-lookup"><span data-stu-id="71a67-166">For more information on using Ambari with HDInsight, see [Manage HDInsight clusters using hello Ambari Web UI](hdinsight-hadoop-manage-ambari.md)</span></span>
