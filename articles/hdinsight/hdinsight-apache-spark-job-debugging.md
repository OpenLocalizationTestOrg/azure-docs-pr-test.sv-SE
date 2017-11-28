---
title: "aaaDebug Apache Spark jobb som körs på Azure HDInsight | Microsoft Docs"
description: "Använd YARN-Användargränssnittet, Spark UI och Spark tidigare server tootrack och felsöka jobb som körs på ett Spark-kluster i Azure HDInsight"
services: hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 59af05a7-2bd9-44b0-b55f-2438d294198b
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/21/2017
ms.author: nitinme
ms.openlocfilehash: 33d352a5773920735aa4e5e8532b78122f381377
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="debug-apache-spark-jobs-running-on-azure-hdinsight"></a><span data-ttu-id="b19c1-103">Felsöka Apache Spark-jobb som körs på Azure HDInsight</span><span class="sxs-lookup"><span data-stu-id="b19c1-103">Debug Apache Spark jobs running on Azure HDInsight</span></span>

<span data-ttu-id="b19c1-104">I den här artikeln du lära dig hur tootrack och felsökningsloggar Väck jobb som körs på HDInsight-kluster med hello YARN-Användargränssnittet, Spark-Gränssnittet och hello Spark historik Server.</span><span class="sxs-lookup"><span data-stu-id="b19c1-104">In this article you will learn how tootrack and debug Spark jobs running on HDInsight clusters using hello YARN UI, Spark UI, and hello Spark History Server.</span></span> <span data-ttu-id="b19c1-105">För den här artikeln har vi startar ett Spark-jobb med hjälp av en bärbar dator som är tillgängliga med hello Spark-kluster **Maskininlärning: förutsägbar analys på mat inspektion data med hjälp av MLLib**.</span><span class="sxs-lookup"><span data-stu-id="b19c1-105">For this article, we will start a Spark job using a notebook available with hello Spark cluster, **Machine learning: Predictive analysis on food inspection data using MLLib**.</span></span> <span data-ttu-id="b19c1-106">Du kan använda hello steg nedan tootrack ett program som du skickas med någon annan metod, till exempel **spark-skicka**.</span><span class="sxs-lookup"><span data-stu-id="b19c1-106">You can use hello steps below tootrack an application that you submitted using any other approach as well, for example, **spark-submit**.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="b19c1-107">Krav</span><span class="sxs-lookup"><span data-stu-id="b19c1-107">Prerequisites</span></span>
<span data-ttu-id="b19c1-108">Du måste ha hello följande:</span><span class="sxs-lookup"><span data-stu-id="b19c1-108">You must have hello following:</span></span>

* <span data-ttu-id="b19c1-109">En Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="b19c1-109">An Azure subscription.</span></span> <span data-ttu-id="b19c1-110">Se [Hämta en kostnadsfri utvärderingsversion av Azure](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).</span><span class="sxs-lookup"><span data-stu-id="b19c1-110">See [Get Azure free trial](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).</span></span>
* <span data-ttu-id="b19c1-111">Ett Apache Spark-kluster i HDInsight.</span><span class="sxs-lookup"><span data-stu-id="b19c1-111">An Apache Spark cluster on HDInsight.</span></span> <span data-ttu-id="b19c1-112">Instruktioner finns i [skapa Apache Spark-kluster i Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).</span><span class="sxs-lookup"><span data-stu-id="b19c1-112">For instructions, see [Create Apache Spark clusters in Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).</span></span>
* <span data-ttu-id="b19c1-113">Du bör ha startat körs hello anteckningsboken  **[Maskininlärning: förutsägbar analys på mat inspektion data med hjälp av MLLib](hdinsight-apache-spark-machine-learning-mllib-ipython.md)**.</span><span class="sxs-lookup"><span data-stu-id="b19c1-113">You should have started running hello notebook, **[Machine learning: Predictive analysis on food inspection data using MLLib](hdinsight-apache-spark-machine-learning-mllib-ipython.md)**.</span></span> <span data-ttu-id="b19c1-114">Anvisningar för hur toorun anteckningsboken Följ hello länk.</span><span class="sxs-lookup"><span data-stu-id="b19c1-114">For instructions on how toorun this notebook, follow hello link.</span></span>  

## <a name="track-an-application-in-hello-yarn-ui"></a><span data-ttu-id="b19c1-115">Spåra ett program i hello YARN-Användargränssnittet</span><span class="sxs-lookup"><span data-stu-id="b19c1-115">Track an application in hello YARN UI</span></span>
1. <span data-ttu-id="b19c1-116">Starta hello YARN-Användargränssnittet.</span><span class="sxs-lookup"><span data-stu-id="b19c1-116">Launch hello YARN UI.</span></span> <span data-ttu-id="b19c1-117">Hello kluster-bladet, klickar du på **Klusterinstrumentpanel**, och klicka sedan på **YARN**.</span><span class="sxs-lookup"><span data-stu-id="b19c1-117">From hello cluster blade, click **Cluster Dashboard**, and then click **YARN**.</span></span>
   
    ![Starta YARN-Användargränssnittet](./media/hdinsight-apache-spark-job-debugging/launch-yarn-ui.png)
   
   > [!TIP]
   > <span data-ttu-id="b19c1-119">Alternativt kan du starta hello YARN-Användargränssnittet från hello Ambari UI.</span><span class="sxs-lookup"><span data-stu-id="b19c1-119">Alternatively, you can also launch hello YARN UI from hello Ambari UI.</span></span> <span data-ttu-id="b19c1-120">toolaunch hello Ambari UI från hello kluster-bladet, klickar du på **Klusterinstrumentpanel**, och klicka sedan på **instrumentpanelen för HDInsight-klustret**.</span><span class="sxs-lookup"><span data-stu-id="b19c1-120">toolaunch hello Ambari UI, from hello cluster blade, click **Cluster Dashboard**, and then click **HDInsight Cluster Dashboard**.</span></span> <span data-ttu-id="b19c1-121">Hello Ambari UI, klicka på **YARN**, klickar du på **snabblänkar**, klicka på hello active resource manager och klicka sedan på **ResourceManager UI**.</span><span class="sxs-lookup"><span data-stu-id="b19c1-121">From hello Ambari UI, click **YARN**, click **Quick Links**, click hello active resource manager, and then click **ResourceManager UI**.</span></span>    
   > 
   > 
2. <span data-ttu-id="b19c1-122">Eftersom du startade hello Spark jobb med hjälp av Jupyter-anteckningsböcker hello program har hello namn **remotesparkmagics** (detta är hello namn för alla program som startas från hello bärbara datorer).</span><span class="sxs-lookup"><span data-stu-id="b19c1-122">Because you started hello Spark job using Jupyter notebooks, hello application has hello name **remotesparkmagics** (this is hello name for all applications that are started from hello notebooks).</span></span> <span data-ttu-id="b19c1-123">Klicka på hello program-ID mot hello programmet namnet tooget mer information om hello jobb.</span><span class="sxs-lookup"><span data-stu-id="b19c1-123">Click hello application ID against hello application name tooget more information about hello job.</span></span> <span data-ttu-id="b19c1-124">Detta startar hello programvy.</span><span class="sxs-lookup"><span data-stu-id="b19c1-124">This launches hello application view.</span></span>
   
    ![Hitta Spark-program-ID](./media/hdinsight-apache-spark-job-debugging/find-application-id.png)
   
    <span data-ttu-id="b19c1-126">Sådana program som startas från hello Jupyter-anteckningsböcker hello status är alltid **kör** tills du avslutar hello bärbar dator.</span><span class="sxs-lookup"><span data-stu-id="b19c1-126">For such applications that are launched from hello Jupyter notebooks, hello status is always **RUNNING** until you exit hello notebook.</span></span>
3. <span data-ttu-id="b19c1-127">Hello programmet vyn detaljnivån du ytterligare toofind ut hello behållare som är associerade med programmet hello och hello-loggar (stdout/stderr).</span><span class="sxs-lookup"><span data-stu-id="b19c1-127">From hello application view, you can drill down further toofind out hello containers associated with hello application and hello logs (stdout/stderr).</span></span> <span data-ttu-id="b19c1-128">Du kan också starta hello Spark-Användargränssnittet genom att klicka på Länka motsvarande toohello hello **spårning URL**, enligt nedan.</span><span class="sxs-lookup"><span data-stu-id="b19c1-128">You can also launch hello Spark UI by clicking hello linking corresponding toohello **Tracking URL**, as shown below.</span></span> 
   
    ![Hämta loggar för behållaren](./media/hdinsight-apache-spark-job-debugging/download-container-logs.png)

## <a name="track-an-application-in-hello-spark-ui"></a><span data-ttu-id="b19c1-130">Spåra ett program i hello Spark UI</span><span class="sxs-lookup"><span data-stu-id="b19c1-130">Track an application in hello Spark UI</span></span>
<span data-ttu-id="b19c1-131">I hello Spark UI, kan du gå till hello Spark jobb som skapas av hello-program som du startade tidigare.</span><span class="sxs-lookup"><span data-stu-id="b19c1-131">In hello Spark UI, you can drill down into hello Spark jobs that are spawned by hello application you started earlier.</span></span>

1. <span data-ttu-id="b19c1-132">toolaunch hello Spark-Gränssnittet från hello programvy Klicka hello länken mot hello **spårning URL**som visas i hello skärmdump ovan.</span><span class="sxs-lookup"><span data-stu-id="b19c1-132">toolaunch hello Spark UI, from hello application view, click hello link against hello **Tracking URL**, as shown in hello screen capture above.</span></span> <span data-ttu-id="b19c1-133">Du kan se alla hello Spark jobb som startats av hello-program som körs i hello Jupyter-anteckningsbok.</span><span class="sxs-lookup"><span data-stu-id="b19c1-133">You can see all hello Spark jobs that are launched by hello application running in hello Jupyter notebook.</span></span>
   
    ![Visa Spark jobb](./media/hdinsight-apache-spark-job-debugging/view-spark-jobs.png)
2. <span data-ttu-id="b19c1-135">Klicka på hello **Executors** fliken information om toosee bearbetning och lagring för varje utförare.</span><span class="sxs-lookup"><span data-stu-id="b19c1-135">Click hello **Executors** tab toosee processing and storage information for each executor.</span></span> <span data-ttu-id="b19c1-136">Du kan också hämta hello anropsstacken genom att klicka på hello **tråd Dump** länk.</span><span class="sxs-lookup"><span data-stu-id="b19c1-136">You can also retrieve hello call stack by clicking on hello **Thread Dump** link.</span></span>
   
    ![Visa Spark executors](./media/hdinsight-apache-spark-job-debugging/view-spark-executors.png)
3. <span data-ttu-id="b19c1-138">Klicka på hello **steg** fliken toosee hello steg som är associerade med programmet hello.</span><span class="sxs-lookup"><span data-stu-id="b19c1-138">Click hello **Stages** tab toosee hello stages associated with hello application.</span></span>
   
    ![Visa Spark faser](./media/hdinsight-apache-spark-job-debugging/view-spark-stages.png)
   
    <span data-ttu-id="b19c1-140">Varje steg kan ha flera uppgifter som du kan visa statistik för körning, som visas nedan.</span><span class="sxs-lookup"><span data-stu-id="b19c1-140">Each stage can have multiple tasks for which you can view execution statistics, like shown below.</span></span>
   
    ![Visa Spark faser](./media/hdinsight-apache-spark-job-debugging/view-spark-stages-details.png) 
4. <span data-ttu-id="b19c1-142">Du kan starta DAG visualiseringen från hello steg på sidan.</span><span class="sxs-lookup"><span data-stu-id="b19c1-142">From hello stage details page, you can launch DAG Visualization.</span></span> <span data-ttu-id="b19c1-143">Expandera hello **DAG visualiseringen** länka hello överst i hello sida som visas nedan.</span><span class="sxs-lookup"><span data-stu-id="b19c1-143">Expand hello **DAG Visualization** link at hello top of hello page, as shown below.</span></span>
   
    ![Visa Spark steg DAG visualiseringen](./media/hdinsight-apache-spark-job-debugging/view-spark-stages-dag-visualization.png)
   
    <span data-ttu-id="b19c1-145">Representerar hello olika faser i programmet hello DAG eller direkt Aclyic diagram.</span><span class="sxs-lookup"><span data-stu-id="b19c1-145">DAG or Direct Aclyic Graph represents hello different stages in hello application.</span></span> <span data-ttu-id="b19c1-146">Varje ruta i hello graph representerar en Spark-åtgärd som anropas från hello program.</span><span class="sxs-lookup"><span data-stu-id="b19c1-146">Each blue box in hello graph represents a Spark operation invoked from hello application.</span></span>
5. <span data-ttu-id="b19c1-147">Du kan starta hello tidslinjevyn för programmet från hello steg på sidan.</span><span class="sxs-lookup"><span data-stu-id="b19c1-147">From hello stage details page, you can also launch hello application timeline view.</span></span> <span data-ttu-id="b19c1-148">Expandera hello **händelse tidslinjen** länka hello överst i hello sida som visas nedan.</span><span class="sxs-lookup"><span data-stu-id="b19c1-148">Expand hello **Event Timeline** link at hello top of hello page, as shown below.</span></span>
   
    ![Visa Spark steg händelse tidslinjen](./media/hdinsight-apache-spark-job-debugging/view-spark-stages-event-timeline.png)
   
    <span data-ttu-id="b19c1-150">Detta visar hello Spark händelser i hello form av en tidslinje.</span><span class="sxs-lookup"><span data-stu-id="b19c1-150">This displays hello Spark events in hello form of a timeline.</span></span> <span data-ttu-id="b19c1-151">hello tidslinjevyn är tillgänglig på tre nivåer över jobb inom ett jobb, och ett steg.</span><span class="sxs-lookup"><span data-stu-id="b19c1-151">hello timeline view is available at three levels, across jobs, within a job, and within a stage.</span></span> <span data-ttu-id="b19c1-152">hello bilden ovan avbildar hello tidslinjevyn för ett visst stadium.</span><span class="sxs-lookup"><span data-stu-id="b19c1-152">hello image above captures hello timeline view for a given stage.</span></span>
   
   > [!TIP]
   > <span data-ttu-id="b19c1-153">Om du väljer hello **aktivera** kryssrutan, du kan rulla åt vänster och höger över hello tidslinjevyn.</span><span class="sxs-lookup"><span data-stu-id="b19c1-153">If you select hello **Enable zooming** check box, you can scroll left and right across hello timeline view.</span></span>
   > 
   > 
6. <span data-ttu-id="b19c1-154">Andra flikar i hello Spark Användargränssnittet innehåller användbar information om samt hello Spark-instans.</span><span class="sxs-lookup"><span data-stu-id="b19c1-154">Other tabs in hello Spark UI provide useful information about hello Spark instance as well.</span></span>
   
   * <span data-ttu-id="b19c1-155">Fliken lagring – om programmet skapar en RDDs hittar du information om de på fliken för hello lagring.</span><span class="sxs-lookup"><span data-stu-id="b19c1-155">Storage tab - If your application creates an RDDs, you can find information about those in hello Storage tab.</span></span>
   * <span data-ttu-id="b19c1-156">Fliken miljö - den här fliken ger mycket användbar information om din Spark-instans, till exempel hello</span><span class="sxs-lookup"><span data-stu-id="b19c1-156">Environment tab - This tab provides a lot of useful information about your Spark instance such as hello</span></span> 
     * <span data-ttu-id="b19c1-157">Scala version</span><span class="sxs-lookup"><span data-stu-id="b19c1-157">Scala version</span></span>
     * <span data-ttu-id="b19c1-158">Händelseloggen katalog som är associerad med hello-kluster</span><span class="sxs-lookup"><span data-stu-id="b19c1-158">Event log directory associated with hello cluster</span></span>
     * <span data-ttu-id="b19c1-159">Antal kärnor utförare för hello program</span><span class="sxs-lookup"><span data-stu-id="b19c1-159">Number of executor cores for hello application</span></span>
     * <span data-ttu-id="b19c1-160">O.s.v.</span><span class="sxs-lookup"><span data-stu-id="b19c1-160">Etc.</span></span>

## <a name="find-information-about-completed-jobs-using-hello-spark-history-server"></a><span data-ttu-id="b19c1-161">Hitta information om slutförda jobb med hjälp av hello Spark historik Server</span><span class="sxs-lookup"><span data-stu-id="b19c1-161">Find information about completed jobs using hello Spark History Server</span></span>
<span data-ttu-id="b19c1-162">När ett jobb har slutförts beständig hello information om hello jobb i hello Spark historik Server.</span><span class="sxs-lookup"><span data-stu-id="b19c1-162">Once a job is completed, hello information about hello job is persisted in hello Spark History Server.</span></span>

1. <span data-ttu-id="b19c1-163">toolaunch hello Spark historik Server från hello kluster-bladet, klickar du på **Klusterinstrumentpanel**, och klicka sedan på **Spark historik Server**.</span><span class="sxs-lookup"><span data-stu-id="b19c1-163">toolaunch hello Spark History Server, from hello cluster blade, click **Cluster Dashboard**, and then click **Spark History Server**.</span></span>
   
    ![Starta Spark historik Server](./media/hdinsight-apache-spark-job-debugging/launch-spark-history-server.png)
   
   > [!TIP]
   > <span data-ttu-id="b19c1-165">Alternativt kan du starta hello Spark historik Server UI från hello Ambari UI.</span><span class="sxs-lookup"><span data-stu-id="b19c1-165">Alternatively, you can also launch hello Spark History Server UI from hello Ambari UI.</span></span> <span data-ttu-id="b19c1-166">toolaunch hello Ambari UI från hello kluster-bladet, klickar du på **Klusterinstrumentpanel**, och klicka sedan på **instrumentpanelen för HDInsight-klustret**.</span><span class="sxs-lookup"><span data-stu-id="b19c1-166">toolaunch hello Ambari UI, from hello cluster blade, click **Cluster Dashboard**, and then click **HDInsight Cluster Dashboard**.</span></span> <span data-ttu-id="b19c1-167">Hello Ambari UI, klicka på **Spark**, klickar du på **snabblänkar**, och klicka sedan på **Spark historik Server UI**.</span><span class="sxs-lookup"><span data-stu-id="b19c1-167">From hello Ambari UI, click **Spark**, click **Quick Links**, and then click **Spark History Server UI**.</span></span>
   > 
   > 
2. <span data-ttu-id="b19c1-168">Alla hello slutförts program i listan visas.</span><span class="sxs-lookup"><span data-stu-id="b19c1-168">You will see all hello completed applications listed.</span></span> <span data-ttu-id="b19c1-169">Klicka på ett program-ID toodrill nedåt till ett program för mer information.</span><span class="sxs-lookup"><span data-stu-id="b19c1-169">Click an application ID toodrill down into an application for more info.</span></span>
   
    ![Starta Spark historik Server](./media/hdinsight-apache-spark-job-debugging/view-completed-applications.png)

## <a name="see-also"></a><span data-ttu-id="b19c1-171">Se även</span><span class="sxs-lookup"><span data-stu-id="b19c1-171">See also</span></span>
*  [<span data-ttu-id="b19c1-172">Hantera resurser för hello Apache Spark-kluster i Azure HDInsight</span><span class="sxs-lookup"><span data-stu-id="b19c1-172">Manage resources for hello Apache Spark cluster in Azure HDInsight</span></span>](hdinsight-apache-spark-resource-manager.md)

### <a name="for-data-analysts"></a><span data-ttu-id="b19c1-173">För dataanalytiker</span><span class="sxs-lookup"><span data-stu-id="b19c1-173">For data analysts</span></span>

* [<span data-ttu-id="b19c1-174">Spark med Machine Learning: Använda Spark i HDInsight för analys av byggnadstemperatur med HVAC-data</span><span class="sxs-lookup"><span data-stu-id="b19c1-174">Spark with Machine Learning: Use Spark in HDInsight for analyzing building temperature using HVAC data</span></span>](hdinsight-apache-spark-ipython-notebook-machine-learning.md)
* [<span data-ttu-id="b19c1-175">Spark med Machine Learning: använda Spark i HDInsight toopredict livsmedelskontroll</span><span class="sxs-lookup"><span data-stu-id="b19c1-175">Spark with Machine Learning: Use Spark in HDInsight toopredict food inspection results</span></span>](hdinsight-apache-spark-machine-learning-mllib-ipython.md)
* [<span data-ttu-id="b19c1-176">Webbplatslogganalys med Spark i HDInsight</span><span class="sxs-lookup"><span data-stu-id="b19c1-176">Website log analysis using Spark in HDInsight</span></span>](hdinsight-apache-spark-custom-library-website-log-analysis.md)
* [<span data-ttu-id="b19c1-177">Application Insight-telemetridataanalys i HDInsight</span><span class="sxs-lookup"><span data-stu-id="b19c1-177">Application Insight telemetry data analysis using Spark in HDInsight</span></span>](hdinsight-spark-analyze-application-insight-logs.md)
* [<span data-ttu-id="b19c1-178">Använda Caffe för distribuerade djup learning på Azure HDInsight Spark</span><span class="sxs-lookup"><span data-stu-id="b19c1-178">Use Caffe on Azure HDInsight Spark for distributed deep learning</span></span>](hdinsight-deep-learning-caffe-spark.md)

### <a name="for-spark-developers"></a><span data-ttu-id="b19c1-179">För Spark-utvecklare</span><span class="sxs-lookup"><span data-stu-id="b19c1-179">For Spark developers</span></span>

* [<span data-ttu-id="b19c1-180">Skapa ett fristående program med hjälp av Scala</span><span class="sxs-lookup"><span data-stu-id="b19c1-180">Create a standalone application using Scala</span></span>](hdinsight-apache-spark-create-standalone-application.md)
* [<span data-ttu-id="b19c1-181">Köra jobb via fjärranslutning på ett Spark-kluster med Livy</span><span class="sxs-lookup"><span data-stu-id="b19c1-181">Run jobs remotely on a Spark cluster using Livy</span></span>](hdinsight-apache-spark-livy-rest-interface.md)
* [<span data-ttu-id="b19c1-182">Använda HDInsight Tools-Plugin för IntelliJ IDEA toocreate och skicka Spark Scala-program</span><span class="sxs-lookup"><span data-stu-id="b19c1-182">Use HDInsight Tools Plugin for IntelliJ IDEA toocreate and submit Spark Scala applications</span></span>](hdinsight-apache-spark-intellij-tool-plugin.md)
* [<span data-ttu-id="b19c1-183">Spark Streaming: Använda Spark i HDInsight för att bygga program för strömning i realtid</span><span class="sxs-lookup"><span data-stu-id="b19c1-183">Spark Streaming: Use Spark in HDInsight for building real-time streaming applications</span></span>](hdinsight-apache-spark-eventhub-streaming.md)
* [<span data-ttu-id="b19c1-184">Använda HDInsight Tools-Plugin för IntelliJ IDEA toodebug Spark-program via fjärranslutning</span><span class="sxs-lookup"><span data-stu-id="b19c1-184">Use HDInsight Tools Plugin for IntelliJ IDEA toodebug Spark applications remotely</span></span>](hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [<span data-ttu-id="b19c1-185">Använda Zeppelin-anteckningsböcker med ett Spark-kluster i HDInsight</span><span class="sxs-lookup"><span data-stu-id="b19c1-185">Use Zeppelin notebooks with a Spark cluster on HDInsight</span></span>](hdinsight-apache-spark-zeppelin-notebook.md)
* [<span data-ttu-id="b19c1-186">Kernlar som är tillgängliga för Jupyter Notebook i Spark-klustret för HDInsight</span><span class="sxs-lookup"><span data-stu-id="b19c1-186">Kernels available for Jupyter notebook in Spark cluster for HDInsight</span></span>](hdinsight-apache-spark-jupyter-notebook-kernels.md)
* [<span data-ttu-id="b19c1-187">Använda externa paket med Jupyter-anteckningsböcker</span><span class="sxs-lookup"><span data-stu-id="b19c1-187">Use external packages with Jupyter notebooks</span></span>](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md)
* [<span data-ttu-id="b19c1-188">Installera Jupyter på datorn och ansluta tooan HDInsight Spark-kluster</span><span class="sxs-lookup"><span data-stu-id="b19c1-188">Install Jupyter on your computer and connect tooan HDInsight Spark cluster</span></span>](hdinsight-apache-spark-jupyter-notebook-install-locally.md)


