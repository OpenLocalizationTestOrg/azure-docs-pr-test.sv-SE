---
title: aaaTroubleshoot problem med Apache Spark-klustret i Azure HDInsight | Microsoft Docs
description: "Lär dig mer om problem relaterade tooApache Spark-kluster i Azure HDInsight och hur toowork runt de."
services: hdinsight
documentationcenter: 
author: mumian
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 610c4103-ffc8-4ec0-ad06-fdaf3c4d7c10
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/10/2017
ms.author: nitinme
ms.openlocfilehash: 7373b90524ae5dbb10ab8ded593aa38d12c14b55
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="known-issues-for-apache-spark-cluster-on-hdinsight"></a><span data-ttu-id="761b0-103">Kända problem med Apache Spark-kluster i HDInsight</span><span class="sxs-lookup"><span data-stu-id="761b0-103">Known issues for Apache Spark cluster on HDInsight</span></span>

<span data-ttu-id="761b0-104">Det här dokumentet håller reda på alla hello kända problem för hello HDInsight Spark förhandsversion.</span><span class="sxs-lookup"><span data-stu-id="761b0-104">This document keeps track of all hello known issues for hello HDInsight Spark public preview.</span></span>  

## <a name="livy-leaks-interactive-session"></a><span data-ttu-id="761b0-105">Livius läcker interaktiva sessionen</span><span class="sxs-lookup"><span data-stu-id="761b0-105">Livy leaks interactive session</span></span>
<span data-ttu-id="761b0-106">När Livius startas (från Ambari eller på grund av tooheadnode 0 virtuella omstart) med en interaktiv session fortfarande lever läcka en interaktiv jobbet session.</span><span class="sxs-lookup"><span data-stu-id="761b0-106">When Livy is restarted (from Ambari or due tooheadnode 0 virtual machine reboot) with an interactive session still alive, an interactive job session will be leaked.</span></span> <span data-ttu-id="761b0-107">Nya jobb kan därför fastnat i hello godkända tillstånd och kan inte startas.</span><span class="sxs-lookup"><span data-stu-id="761b0-107">Because of this, new jobs can stuck in hello Accepted state, and cannot be started.</span></span>

<span data-ttu-id="761b0-108">**Lösning:**</span><span class="sxs-lookup"><span data-stu-id="761b0-108">**Mitigation:**</span></span>

<span data-ttu-id="761b0-109">Använd följande procedur tooworkaround hello problemet hello:</span><span class="sxs-lookup"><span data-stu-id="761b0-109">Use hello following procedure tooworkaround hello issue:</span></span>

1. <span data-ttu-id="761b0-110">SSH i headnode.</span><span class="sxs-lookup"><span data-stu-id="761b0-110">Ssh into headnode.</span></span> <span data-ttu-id="761b0-111">Mer information finns i [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md) (Använda SSH med HDInsight).</span><span class="sxs-lookup"><span data-stu-id="761b0-111">For information, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

2. <span data-ttu-id="761b0-112">Kör hello efter kommandot toofind hello program-ID för hello interaktiva jobb startas via Livius.</span><span class="sxs-lookup"><span data-stu-id="761b0-112">Run hello following command toofind hello application IDs of hello interactive jobs started through Livy.</span></span> 
   
        yarn application –list
   
    <span data-ttu-id="761b0-113">hello standardnamnen jobbet kommer att Livius om hello-jobb startades med en interaktiv session Livius med inga explicit namn som angetts för hello Livius-session som har startats med Jupyter-anteckningsbok hello jobbnamn startar med remotesparkmagics_ *.</span><span class="sxs-lookup"><span data-stu-id="761b0-113">hello default job names will be Livy if hello jobs were started with a Livy interactive session with no explicit names specified, For hello Livy session started by Jupyter notebook, hello job name will start with remotesparkmagics_*.</span></span> 
3. <span data-ttu-id="761b0-114">Kör följande kommando tookill hello dessa jobb.</span><span class="sxs-lookup"><span data-stu-id="761b0-114">Run hello following command tookill those jobs.</span></span> 
   
        yarn application –kill <Application ID>

<span data-ttu-id="761b0-115">Nya jobb ska börja köras.</span><span class="sxs-lookup"><span data-stu-id="761b0-115">New jobs will start running.</span></span> 

## <a name="spark-history-server-not-started"></a><span data-ttu-id="761b0-116">Spark historik Server inte har startats</span><span class="sxs-lookup"><span data-stu-id="761b0-116">Spark History Server not started</span></span>
<span data-ttu-id="761b0-117">Spark historik Server startas inte automatiskt efter ett kluster skapas.</span><span class="sxs-lookup"><span data-stu-id="761b0-117">Spark History Server is not started automatically after a cluster is created.</span></span>  

<span data-ttu-id="761b0-118">**Lösning:**</span><span class="sxs-lookup"><span data-stu-id="761b0-118">**Mitigation:**</span></span> 

<span data-ttu-id="761b0-119">Starta hello historik server manuellt från Ambari.</span><span class="sxs-lookup"><span data-stu-id="761b0-119">Manually start hello history server from Ambari.</span></span>

## <a name="permission-issue-in-spark-log-directory"></a><span data-ttu-id="761b0-120">Problem i Spark loggkatalogen</span><span class="sxs-lookup"><span data-stu-id="761b0-120">Permission issue in Spark log directory</span></span>
<span data-ttu-id="761b0-121">När hdiuser skickar ett jobb med spark-submit, det finns ett fel java.io.FileNotFoundException: /var/log/spark/sparkdriver_hdiuser.log (behörighet nekas) och hello drivrutinen inte loggen skapas.</span><span class="sxs-lookup"><span data-stu-id="761b0-121">When hdiuser submits a job with spark-submit, there is an error java.io.FileNotFoundException: /var/log/spark/sparkdriver_hdiuser.log (Permission denied) and hello driver log is not written.</span></span> 

<span data-ttu-id="761b0-122">**Lösning:**</span><span class="sxs-lookup"><span data-stu-id="761b0-122">**Mitigation:**</span></span>

1. <span data-ttu-id="761b0-123">Lägga till hdiuser toohello Hadoop grupp.</span><span class="sxs-lookup"><span data-stu-id="761b0-123">Add hdiuser toohello Hadoop group.</span></span> 
2. <span data-ttu-id="761b0-124">Ange 777 behörigheter på /var/log/spark när klustret skapas.</span><span class="sxs-lookup"><span data-stu-id="761b0-124">Provide 777 permissions on /var/log/spark after cluster creation.</span></span> 
3. <span data-ttu-id="761b0-125">Uppdatera hello spark loggplatsen med Ambari toobe en katalog med 777 behörigheter.</span><span class="sxs-lookup"><span data-stu-id="761b0-125">Update hello spark log location using Ambari toobe a directory with 777 permissions.</span></span>  
4. <span data-ttu-id="761b0-126">Kör spark-skicka som sudo.</span><span class="sxs-lookup"><span data-stu-id="761b0-126">Run spark-submit as sudo.</span></span>  

## <a name="spark-phoenix-connector-is-not-supported"></a><span data-ttu-id="761b0-127">Spark Phoenix anslutningen stöds inte</span><span class="sxs-lookup"><span data-stu-id="761b0-127">Spark-Phoenix connector is not supported</span></span>

<span data-ttu-id="761b0-128">Hello Spark Phoenix anslutningen stöds för närvarande inte med ett HDInsight Spark-kluster.</span><span class="sxs-lookup"><span data-stu-id="761b0-128">Currently, hello Spark-Phoenix connector is not supported with an HDInsight Spark cluster.</span></span>

<span data-ttu-id="761b0-129">**Lösning:**</span><span class="sxs-lookup"><span data-stu-id="761b0-129">**Mitigation:**</span></span>

<span data-ttu-id="761b0-130">I stället måste du använda hello Spark-HBase-anslutningen.</span><span class="sxs-lookup"><span data-stu-id="761b0-130">You must use hello Spark-HBase connector instead.</span></span> <span data-ttu-id="761b0-131">Instruktioner finns i [hur toouse Spark HBase connector](https://blogs.msdn.microsoft.com/azuredatalake/2016/07/25/hdinsight-how-to-use-spark-hbase-connector/).</span><span class="sxs-lookup"><span data-stu-id="761b0-131">For instructions see [How toouse Spark-HBase connector](https://blogs.msdn.microsoft.com/azuredatalake/2016/07/25/hdinsight-how-to-use-spark-hbase-connector/).</span></span>

## <a name="issues-related-toojupyter-notebooks"></a><span data-ttu-id="761b0-132">Problem med tooJupyter bärbara datorer</span><span class="sxs-lookup"><span data-stu-id="761b0-132">Issues related tooJupyter notebooks</span></span>
<span data-ttu-id="761b0-133">Följande är några kända problem relaterade tooJupyter bärbara datorer.</span><span class="sxs-lookup"><span data-stu-id="761b0-133">Following are some known issues related tooJupyter notebooks.</span></span>

### <a name="notebooks-with-non-ascii-characters-in-filenames"></a><span data-ttu-id="761b0-134">Bärbara datorer med icke-ASCII-tecken i filnamn</span><span class="sxs-lookup"><span data-stu-id="761b0-134">Notebooks with non-ASCII characters in filenames</span></span>
<span data-ttu-id="761b0-135">Jupyter-anteckningsböcker som kan användas i HDInsight Spark-kluster ska inte ha icke-ASCII-tecken i filnamn.</span><span class="sxs-lookup"><span data-stu-id="761b0-135">Jupyter notebooks that can be used in Spark HDInsight clusters should not have non-ASCII characters in filenames.</span></span> <span data-ttu-id="761b0-136">Om du försöker tooupload en fil genom hello Jupyter UI som har ett icke-ASCII-filnamn, kommer inte tyst (som är Jupyter kan du inte överföra hello-fil, men det kommer inte att utlösa visas fel antingen).</span><span class="sxs-lookup"><span data-stu-id="761b0-136">If you try tooupload a file through hello Jupyter UI which has a non-ASCII filename, it will fail silently (that is, Jupyter won’t let you upload hello file, but it won’t throw a visible error either).</span></span> 

### <a name="error-while-loading-notebooks-of-larger-sizes"></a><span data-ttu-id="761b0-137">Fel vid inläsning av bärbara datorer av större storlek</span><span class="sxs-lookup"><span data-stu-id="761b0-137">Error while loading notebooks of larger sizes</span></span>
<span data-ttu-id="761b0-138">Ett fel kan uppstå  **`Error loading notebook`**  när du läser in anteckningsböcker som är större.</span><span class="sxs-lookup"><span data-stu-id="761b0-138">You might see an error **`Error loading notebook`** when you load notebooks that are larger in size.</span></span>  

<span data-ttu-id="761b0-139">**Lösning:**</span><span class="sxs-lookup"><span data-stu-id="761b0-139">**Mitigation:**</span></span>

<span data-ttu-id="761b0-140">Om du får det här felet kan innebär det inte dina data är skadad eller förlorade.</span><span class="sxs-lookup"><span data-stu-id="761b0-140">If you get this error, it does not mean your data is corrupt or lost.</span></span>  <span data-ttu-id="761b0-141">Dina anteckningsböcker finns kvar på disken i `/var/lib/jupyter`, och du kan SSH till hello klustret tooaccess dem.</span><span class="sxs-lookup"><span data-stu-id="761b0-141">Your notebooks are still on disk in `/var/lib/jupyter`, and you can SSH into hello cluster tooaccess them.</span></span> <span data-ttu-id="761b0-142">Mer information finns i [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md) (Använda SSH med HDInsight).</span><span class="sxs-lookup"><span data-stu-id="761b0-142">For information, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

<span data-ttu-id="761b0-143">När du har anslutit toohello kluster med SSH kopiera du dina anteckningsböcker från ditt kluster tooyour lokal dator (med SCP eller WinSCP) som en säkerhetskopiering tooprevent hello förlust av viktiga data i hello anteckningsbok.</span><span class="sxs-lookup"><span data-stu-id="761b0-143">Once you have connected toohello cluster using SSH, you can copy your notebooks from your cluster tooyour local machine (using SCP or WinSCP) as a backup tooprevent hello loss of any important data in hello notebook.</span></span> <span data-ttu-id="761b0-144">Du kan sedan SSH-tunnel i din headnode på port 8001 tooaccess Jupyter utan att gå via hello gateway.</span><span class="sxs-lookup"><span data-stu-id="761b0-144">You can then SSH tunnel into your headnode at port 8001 tooaccess Jupyter without going through hello gateway.</span></span>  <span data-ttu-id="761b0-145">Därifrån kan du rensa hello utdata från din bärbara dator och spara om den toominimize hello anteckningsboken storlek.</span><span class="sxs-lookup"><span data-stu-id="761b0-145">From there, you can clear hello output of your notebook and re-save it toominimize hello notebook’s size.</span></span>

<span data-ttu-id="761b0-146">tooprevent det här felet inträffar i hello framtida, måste du följa några rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="761b0-146">tooprevent this error from happening in hello future, you must follow some best practices:</span></span>

* <span data-ttu-id="761b0-147">Det är viktigt tookeep hello anteckningsboken storlek små.</span><span class="sxs-lookup"><span data-stu-id="761b0-147">It is important tookeep hello notebook size small.</span></span> <span data-ttu-id="761b0-148">Alla utdata från Spark-jobb som skickas tillbaka tooJupyter sparas i hello bärbar dator.</span><span class="sxs-lookup"><span data-stu-id="761b0-148">Any output from your Spark jobs that is sent back tooJupyter is persisted in hello notebook.</span></span>  <span data-ttu-id="761b0-149">Det är bäst med Jupyter i allmänna tooavoid kör `.collect()` på stora RDDS eller dataframes; om du vill toopeek på ett RDD innehållet i stället körs `.take()` eller `.sample()` så att dina utdata inte ger för stort.</span><span class="sxs-lookup"><span data-stu-id="761b0-149">It is a best practice with Jupyter in general tooavoid running `.collect()` on large RDD’s or dataframes; instead, if you want toopeek at an RDD’s contents, consider running `.take()` or `.sample()` so that your output doesn’t get too big.</span></span>
* <span data-ttu-id="761b0-150">Dessutom när du sparar en bärbar dator, utdata avmarkera alla celler tooreduce hello storlek.</span><span class="sxs-lookup"><span data-stu-id="761b0-150">Also, when you save a notebook, clear all output cells tooreduce hello size.</span></span>

### <a name="notebook-initial-startup-takes-longer-than-expected"></a><span data-ttu-id="761b0-151">Bärbar dator första start tar längre tid än väntat</span><span class="sxs-lookup"><span data-stu-id="761b0-151">Notebook initial startup takes longer than expected</span></span>
<span data-ttu-id="761b0-152">Den första koden instruktionen i Jupyter-anteckningsbok med Spark magic kan ta mer än en minut.</span><span class="sxs-lookup"><span data-stu-id="761b0-152">First code statement in Jupyter notebook using Spark magic could take more than a minute.</span></span>  

<span data-ttu-id="761b0-153">**Förklaring:**</span><span class="sxs-lookup"><span data-stu-id="761b0-153">**Explanation:**</span></span>

<span data-ttu-id="761b0-154">Detta beror på att när hello första kodcellen körs.</span><span class="sxs-lookup"><span data-stu-id="761b0-154">This happens because when hello first code cell is run.</span></span> <span data-ttu-id="761b0-155">I bakgrunden hello detta startar sessionskonfigurationen och Spark, SQL och Hive sammanhang anges.</span><span class="sxs-lookup"><span data-stu-id="761b0-155">In hello background this initiates session configuration and Spark, SQL, and Hive contexts are set.</span></span> <span data-ttu-id="761b0-156">När dessa kontexter ställs hello första satsen körs och det ger hello intryck av att hello instruktionen tog toocomplete en lång tid.</span><span class="sxs-lookup"><span data-stu-id="761b0-156">After these contexts are set, hello first statement is run and this gives hello impression that hello statement took a long time toocomplete.</span></span>

### <a name="jupyter-notebook-timeout-in-creating-hello-session"></a><span data-ttu-id="761b0-157">Jupyter-anteckningsbok timeout i hello session skapas</span><span class="sxs-lookup"><span data-stu-id="761b0-157">Jupyter notebook timeout in creating hello session</span></span>
<span data-ttu-id="761b0-158">När Spark-kluster har slut på resurser, kommer hello Spark och Pyspark-kernel i hello Jupyter-anteckningsbok att tidsgränsen nåddes vid försök toocreate hello session.</span><span class="sxs-lookup"><span data-stu-id="761b0-158">When Spark cluster is out of resources, hello Spark and Pyspark kernels in hello Jupyter notebook will timeout trying toocreate hello session.</span></span> 

<span data-ttu-id="761b0-159">**Åtgärder:**</span><span class="sxs-lookup"><span data-stu-id="761b0-159">**Mitigations:**</span></span> 

1. <span data-ttu-id="761b0-160">Fri resurser i Spark-kluster genom att:</span><span class="sxs-lookup"><span data-stu-id="761b0-160">Free up some resources in your Spark cluster by:</span></span>
   
   * <span data-ttu-id="761b0-161">Stoppa andra Spark bärbara datorer genom att gå toohello Stäng och stopp-menyn eller klicka Stäng i hello anteckningsboken explorer.</span><span class="sxs-lookup"><span data-stu-id="761b0-161">Stopping other Spark notebooks by going toohello Close and Halt menu or clicking Shutdown in hello notebook explorer.</span></span>
   * <span data-ttu-id="761b0-162">Stoppa andra Spark-program från YARN.</span><span class="sxs-lookup"><span data-stu-id="761b0-162">Stopping other Spark applications from YARN.</span></span>
2. <span data-ttu-id="761b0-163">Starta om hello anteckningsboken skulle toostart upp.</span><span class="sxs-lookup"><span data-stu-id="761b0-163">Restart hello notebook you were trying toostart up.</span></span> <span data-ttu-id="761b0-164">Tillräckligt med resurser ska vara tillgänglig för du toocreate nu en session.</span><span class="sxs-lookup"><span data-stu-id="761b0-164">Enough resources should be available for you toocreate a session now.</span></span>

## <a name="see-also"></a><span data-ttu-id="761b0-165">Se även</span><span class="sxs-lookup"><span data-stu-id="761b0-165">See also</span></span>
* [<span data-ttu-id="761b0-166">Översikt: Apache Spark i Azure HDInsight</span><span class="sxs-lookup"><span data-stu-id="761b0-166">Overview: Apache Spark on Azure HDInsight</span></span>](hdinsight-apache-spark-overview.md)

### <a name="scenarios"></a><span data-ttu-id="761b0-167">Scenarier</span><span class="sxs-lookup"><span data-stu-id="761b0-167">Scenarios</span></span>
* [<span data-ttu-id="761b0-168">Spark med BI: Utföra interaktiv dataanalys med hjälp av Spark i HDInsight med BI-verktyg</span><span class="sxs-lookup"><span data-stu-id="761b0-168">Spark with BI: Perform interactive data analysis using Spark in HDInsight with BI tools</span></span>](hdinsight-apache-spark-use-bi-tools.md)
* [<span data-ttu-id="761b0-169">Spark med Machine Learning: Använda Spark i HDInsight för analys av byggnadstemperatur med HVAC-data</span><span class="sxs-lookup"><span data-stu-id="761b0-169">Spark with Machine Learning: Use Spark in HDInsight for analyzing building temperature using HVAC data</span></span>](hdinsight-apache-spark-ipython-notebook-machine-learning.md)
* [<span data-ttu-id="761b0-170">Spark med Machine Learning: använda Spark i HDInsight toopredict livsmedelskontroll</span><span class="sxs-lookup"><span data-stu-id="761b0-170">Spark with Machine Learning: Use Spark in HDInsight toopredict food inspection results</span></span>](hdinsight-apache-spark-machine-learning-mllib-ipython.md)
* [<span data-ttu-id="761b0-171">Spark Streaming: Använda Spark i HDInsight för att bygga program för strömning i realtid</span><span class="sxs-lookup"><span data-stu-id="761b0-171">Spark Streaming: Use Spark in HDInsight for building real-time streaming applications</span></span>](hdinsight-apache-spark-eventhub-streaming.md)
* [<span data-ttu-id="761b0-172">Webbplatslogganalys med Spark i HDInsight</span><span class="sxs-lookup"><span data-stu-id="761b0-172">Website log analysis using Spark in HDInsight</span></span>](hdinsight-apache-spark-custom-library-website-log-analysis.md)

### <a name="create-and-run-applications"></a><span data-ttu-id="761b0-173">Skapa och köra program</span><span class="sxs-lookup"><span data-stu-id="761b0-173">Create and run applications</span></span>
* [<span data-ttu-id="761b0-174">Skapa ett fristående program med hjälp av Scala</span><span class="sxs-lookup"><span data-stu-id="761b0-174">Create a standalone application using Scala</span></span>](hdinsight-apache-spark-create-standalone-application.md)
* [<span data-ttu-id="761b0-175">Köra jobb via fjärranslutning på ett Spark-kluster med Livy</span><span class="sxs-lookup"><span data-stu-id="761b0-175">Run jobs remotely on a Spark cluster using Livy</span></span>](hdinsight-apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a><span data-ttu-id="761b0-176">Verktyg och tillägg</span><span class="sxs-lookup"><span data-stu-id="761b0-176">Tools and extensions</span></span>
* [<span data-ttu-id="761b0-177">Använda HDInsight Tools-Plugin för IntelliJ IDEA toocreate och skicka Spark Scala-appar</span><span class="sxs-lookup"><span data-stu-id="761b0-177">Use HDInsight Tools Plugin for IntelliJ IDEA toocreate and submit Spark Scala applicatons</span></span>](hdinsight-apache-spark-intellij-tool-plugin.md)
* [<span data-ttu-id="761b0-178">Använda HDInsight Tools-Plugin för IntelliJ IDEA toodebug Spark-program via fjärranslutning</span><span class="sxs-lookup"><span data-stu-id="761b0-178">Use HDInsight Tools Plugin for IntelliJ IDEA toodebug Spark applications remotely</span></span>](hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [<span data-ttu-id="761b0-179">Använda Zeppelin-anteckningsböcker med ett Spark-kluster i HDInsight</span><span class="sxs-lookup"><span data-stu-id="761b0-179">Use Zeppelin notebooks with a Spark cluster on HDInsight</span></span>](hdinsight-apache-spark-zeppelin-notebook.md)
* [<span data-ttu-id="761b0-180">Kernlar som är tillgängliga för Jupyter Notebook i Spark-klustret för HDInsight</span><span class="sxs-lookup"><span data-stu-id="761b0-180">Kernels available for Jupyter notebook in Spark cluster for HDInsight</span></span>](hdinsight-apache-spark-jupyter-notebook-kernels.md)
* [<span data-ttu-id="761b0-181">Använda externa paket med Jupyter-anteckningsböcker</span><span class="sxs-lookup"><span data-stu-id="761b0-181">Use external packages with Jupyter notebooks</span></span>](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md)
* [<span data-ttu-id="761b0-182">Installera Jupyter på datorn och ansluta tooan HDInsight Spark-kluster</span><span class="sxs-lookup"><span data-stu-id="761b0-182">Install Jupyter on your computer and connect tooan HDInsight Spark cluster</span></span>](hdinsight-apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a><span data-ttu-id="761b0-183">Hantera resurser</span><span class="sxs-lookup"><span data-stu-id="761b0-183">Manage resources</span></span>
* [<span data-ttu-id="761b0-184">Hantera resurser för hello Apache Spark-kluster i Azure HDInsight</span><span class="sxs-lookup"><span data-stu-id="761b0-184">Manage resources for hello Apache Spark cluster in Azure HDInsight</span></span>](hdinsight-apache-spark-resource-manager.md)
* [<span data-ttu-id="761b0-185">Följa och felsöka jobb som körs i ett Apache Spark-kluster i HDInsight</span><span class="sxs-lookup"><span data-stu-id="761b0-185">Track and debug jobs running on an Apache Spark cluster in HDInsight</span></span>](hdinsight-apache-spark-job-debugging.md)

