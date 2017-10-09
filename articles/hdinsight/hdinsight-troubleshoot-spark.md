---
title: "aaaTroubleshoot Spark med hjälp av Azure HDInsight | Microsoft Docs"
description: "Få svar toocommon frågor om hur du arbetar med Apache Spark och Azure HDInsight."
keywords: "Azure HDInsight Spark, vanliga frågor och svar, felsökning guide, vanliga problem, konfiguration, Ambari"
services: Azure HDInsight
documentationcenter: na
author: arijitt
manager: 
editor: 
ms.assetid: 25D89586-DE5B-4268-B5D5-CC2CE12207ED
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 7/7/2017
ms.author: arijitt
ms.openlocfilehash: c9f910daf295462238a3143ae2589db6d383097f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-spark-by-using-azure-hdinsight"></a><span data-ttu-id="a2ec8-104">Felsöka Spark med Azure HDInsight</span><span class="sxs-lookup"><span data-stu-id="a2ec8-104">Troubleshoot Spark by using Azure HDInsight</span></span>

<span data-ttu-id="a2ec8-105">Läs mer om hello de främsta problemen och sina lösningar när du arbetar med Apache Spark nyttolaster i Apache Ambari.</span><span class="sxs-lookup"><span data-stu-id="a2ec8-105">Learn about hello top issues and their resolutions when working with Apache Spark payloads in Apache Ambari.</span></span>

## <a name="how-do-i-configure-a-spark-application-by-using-ambari-on-clusters"></a><span data-ttu-id="a2ec8-106">Hur konfigurerar jag ett Spark-program genom att använda Ambari på kluster</span><span class="sxs-lookup"><span data-stu-id="a2ec8-106">How do I configure a Spark application by using Ambari on clusters</span></span>

### <a name="resolution-steps"></a><span data-ttu-id="a2ec8-107">Lösningssteg</span><span class="sxs-lookup"><span data-stu-id="a2ec8-107">Resolution steps</span></span>

<span data-ttu-id="a2ec8-108">tidigare hello konfigurationsvärden för den här proceduren har ställts in i HDInsight.</span><span class="sxs-lookup"><span data-stu-id="a2ec8-108">hello configuration values for this procedure were previously set in HDInsight.</span></span> <span data-ttu-id="a2ec8-109">toodetermine vilka Spark-konfigurationer måste toobe uppsättningen och toowhat värden, se [vad som orsakar en Spark OutofMemoryError undantag](#what-causes-a-spark-application-outofmemoryerror-exception).</span><span class="sxs-lookup"><span data-stu-id="a2ec8-109">toodetermine which Spark configurations need toobe set and toowhat values, see [What causes a Spark application OutofMemoryError exception](#what-causes-a-spark-application-outofmemoryerror-exception).</span></span> 

1. <span data-ttu-id="a2ec8-110">I hello kluster väljer du **Spark2**.</span><span class="sxs-lookup"><span data-stu-id="a2ec8-110">In hello list of clusters, select **Spark2**.</span></span>

    ![Markera klustret från listan](media/hdinsight-troubleshoot-spark/update-config-1.png)

2. <span data-ttu-id="a2ec8-112">Välj hello **konfigurationerna** fliken.</span><span class="sxs-lookup"><span data-stu-id="a2ec8-112">Select hello **Configs** tab.</span></span>

    ![Fliken hello-konfigurationer](media/hdinsight-troubleshoot-spark/update-config-2.png)

3. <span data-ttu-id="a2ec8-114">Markera i hello lista över konfigurationer, **anpassad-spark2-standarder**.</span><span class="sxs-lookup"><span data-stu-id="a2ec8-114">In hello list of configurations, select **Custom-spark2-defaults**.</span></span>

    ![Ange standardinställningar för anpassad spark](media/hdinsight-troubleshoot-spark/update-config-3.png)

4. <span data-ttu-id="a2ec8-116">Leta efter hello-värdet som du behöver tooadjust, som **spark.executor.memory**.</span><span class="sxs-lookup"><span data-stu-id="a2ec8-116">Look for hello value setting that you need tooadjust, such as **spark.executor.memory**.</span></span> <span data-ttu-id="a2ec8-117">I det här fallet hello värdet för **4608m** är för hög.</span><span class="sxs-lookup"><span data-stu-id="a2ec8-117">In this case, hello value of **4608m** is too high.</span></span>

    ![Markera hello spark.executor.memory fält](media/hdinsight-troubleshoot-spark/update-config-4.png)

5. <span data-ttu-id="a2ec8-119">Ange hello värdet toohello rekommenderad inställning.</span><span class="sxs-lookup"><span data-stu-id="a2ec8-119">Set hello value toohello recommended setting.</span></span> <span data-ttu-id="a2ec8-120">Hej värdet **2 048 m** rekommenderas för den här inställningen.</span><span class="sxs-lookup"><span data-stu-id="a2ec8-120">hello value **2048m** is recommended for this setting.</span></span>

    ![Ändra värdet too2048m](media/hdinsight-troubleshoot-spark/update-config-5.png)

6. <span data-ttu-id="a2ec8-122">Spara hello värde och spara hello konfiguration.</span><span class="sxs-lookup"><span data-stu-id="a2ec8-122">Save hello value, and then save hello configuration.</span></span> <span data-ttu-id="a2ec8-123">Välj hello verktygsfältet **spara**.</span><span class="sxs-lookup"><span data-stu-id="a2ec8-123">On hello toolbar, select **Save**.</span></span>

    ![Spara inställningen hello och konfiguration](media/hdinsight-troubleshoot-spark/update-config-6a.png)

    <span data-ttu-id="a2ec8-125">Du meddelas om några konfigurationer behöver åtgärdas.</span><span class="sxs-lookup"><span data-stu-id="a2ec8-125">You are notified if any configurations need attention.</span></span> <span data-ttu-id="a2ec8-126">Observera hello objekt och välj sedan **fortsätta ändå**.</span><span class="sxs-lookup"><span data-stu-id="a2ec8-126">Note hello items, and then select **Proceed Anyway**.</span></span> 

    ![Välj fortsätter ändå](media/hdinsight-troubleshoot-spark/update-config-6b.png)

    <span data-ttu-id="a2ec8-128">Skriv en anteckning om hello konfigurationsändringar och välj sedan **spara**.</span><span class="sxs-lookup"><span data-stu-id="a2ec8-128">Write a note about hello configuration changes, and then select **Save**.</span></span>

    ![Ange en notering om hello ändringar](media/hdinsight-troubleshoot-spark/update-config-6c.png)

7. <span data-ttu-id="a2ec8-130">När en konfiguration sparas uppmanas du toorestart hello-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="a2ec8-130">Whenever a configuration is saved, you are prompted toorestart hello service.</span></span> <span data-ttu-id="a2ec8-131">Välj **starta om**.</span><span class="sxs-lookup"><span data-stu-id="a2ec8-131">Select **Restart**.</span></span>

    ![Välj omstart](media/hdinsight-troubleshoot-spark/update-config-7a.png)

    <span data-ttu-id="a2ec8-133">Bekräfta hello omstart.</span><span class="sxs-lookup"><span data-stu-id="a2ec8-133">Confirm hello restart.</span></span>

    ![Välj bekräfta starta om alla](media/hdinsight-troubleshoot-spark/update-config-7b.png)

    <span data-ttu-id="a2ec8-135">Du kan granska hello processer som körs.</span><span class="sxs-lookup"><span data-stu-id="a2ec8-135">You can review hello processes that are running.</span></span>

    ![Granska processer som körs](media/hdinsight-troubleshoot-spark/update-config-7c.png)

8. <span data-ttu-id="a2ec8-137">Du kan lägga till konfigurationer.</span><span class="sxs-lookup"><span data-stu-id="a2ec8-137">You can add configurations.</span></span> <span data-ttu-id="a2ec8-138">Markera i hello lista över konfigurationer, **anpassad-spark2-standarder**, och välj sedan **Lägg till egenskap**.</span><span class="sxs-lookup"><span data-stu-id="a2ec8-138">In hello list of configurations, select **Custom-spark2-defaults**, and then select **Add Property**.</span></span>

    ![Välj Lägg till egenskap](media/hdinsight-troubleshoot-spark/update-config-8.png)

9. <span data-ttu-id="a2ec8-140">Definiera en ny egenskap.</span><span class="sxs-lookup"><span data-stu-id="a2ec8-140">Define a new property.</span></span> <span data-ttu-id="a2ec8-141">Du kan definiera en egenskap med hjälp av en dialogruta för specifika inställningar, till exempel hello-datatypen.</span><span class="sxs-lookup"><span data-stu-id="a2ec8-141">You can define a single property by using a dialog box for specific settings such as hello data type.</span></span> <span data-ttu-id="a2ec8-142">Eller så du kan definiera flera egenskaper med hjälp av en definition av per rad.</span><span class="sxs-lookup"><span data-stu-id="a2ec8-142">Or, you can define multiple properties by using one definition per line.</span></span> 

    <span data-ttu-id="a2ec8-143">I det här exemplet hello **spark.driver.memory** egenskapen har definierats med värdet **4g**.</span><span class="sxs-lookup"><span data-stu-id="a2ec8-143">In this example, hello **spark.driver.memory** property is defined with a value of **4g**.</span></span>

    ![Definiera nya egenskap](media/hdinsight-troubleshoot-spark/update-config-9.png)

10. <span data-ttu-id="a2ec8-145">Spara hello konfigurationen och starta om tjänsten hello enligt beskrivningen i steg 6 och 7.</span><span class="sxs-lookup"><span data-stu-id="a2ec8-145">Save hello configuration, and then restart hello service as described in steps 6 and 7.</span></span>

<span data-ttu-id="a2ec8-146">Dessa ändringar är hela klustret, men kan åsidosättas när du skickar hello Spark jobb.</span><span class="sxs-lookup"><span data-stu-id="a2ec8-146">These changes are cluster-wide but can be overridden when you submit hello Spark job.</span></span>

### <a name="additional-reading"></a><span data-ttu-id="a2ec8-147">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="a2ec8-147">Additional reading</span></span>

[<span data-ttu-id="a2ec8-148">Jobbet för Spark i HDInsight-kluster</span><span class="sxs-lookup"><span data-stu-id="a2ec8-148">Spark job submission on HDInsight clusters</span></span>](https://blogs.msdn.microsoft.com/azuredatalake/2017/01/06/spark-job-submission-on-hdinsight-101/)


## <a name="how-do-i-configure-a-spark-application-by-using-a-jupyter-notebook-on-clusters"></a><span data-ttu-id="a2ec8-149">Hur konfigurerar jag ett Spark-program med hjälp av en Jupyter-anteckningsbok på kluster</span><span class="sxs-lookup"><span data-stu-id="a2ec8-149">How do I configure a Spark application by using a Jupyter notebook on clusters</span></span>

### <a name="resolution-steps"></a><span data-ttu-id="a2ec8-150">Lösningssteg</span><span class="sxs-lookup"><span data-stu-id="a2ec8-150">Resolution steps</span></span>

1. <span data-ttu-id="a2ec8-151">toodetermine vilka Spark-konfigurationer måste toobe uppsättningen och toowhat värden, se [vad som orsakar en Spark OutofMemoryError undantag](#what-causes-a-spark-application-outofmemoryerror-exception).</span><span class="sxs-lookup"><span data-stu-id="a2ec8-151">toodetermine which Spark configurations need toobe set and toowhat values, see [What causes a Spark application OutofMemoryError exception](#what-causes-a-spark-application-outofmemoryerror-exception).</span></span>

2. <span data-ttu-id="a2ec8-152">I hello första cellen i hello Jupyter notebook efter hello **%% konfigurera** direktiv, ange hello Spark konfigurationer i ett giltigt JSON-format.</span><span class="sxs-lookup"><span data-stu-id="a2ec8-152">In hello first cell of hello Jupyter notebook, after hello **%%configure** directive, specify hello Spark configurations in valid JSON format.</span></span> <span data-ttu-id="a2ec8-153">Ändra hello faktiska värden efter behov:</span><span class="sxs-lookup"><span data-stu-id="a2ec8-153">Change hello actual values as necessary:</span></span>

    ![Lägga till en konfiguration](media/hdinsight-troubleshoot-spark/add-configuration-cell.png)

### <a name="additional-reading"></a><span data-ttu-id="a2ec8-155">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="a2ec8-155">Additional reading</span></span>

[<span data-ttu-id="a2ec8-156">Jobbet för Spark i HDInsight-kluster</span><span class="sxs-lookup"><span data-stu-id="a2ec8-156">Spark job submission on HDInsight clusters</span></span>](https://blogs.msdn.microsoft.com/azuredatalake/2017/01/06/spark-job-submission-on-hdinsight-101/)


## <a name="how-do-i-configure-a-spark-application-by-using-livy-on-clusters"></a><span data-ttu-id="a2ec8-157">Hur konfigurerar jag ett Spark-program med hjälp av Livius på kluster</span><span class="sxs-lookup"><span data-stu-id="a2ec8-157">How do I configure a Spark application by using Livy on clusters</span></span>

### <a name="resolution-steps"></a><span data-ttu-id="a2ec8-158">Lösningssteg</span><span class="sxs-lookup"><span data-stu-id="a2ec8-158">Resolution steps</span></span>

1. <span data-ttu-id="a2ec8-159">toodetermine vilka Spark-konfigurationer måste toobe uppsättningen och toowhat värden, se [vad som orsakar en Spark OutofMemoryError undantag](#what-causes-a-spark-application-outofmemoryerror-exception).</span><span class="sxs-lookup"><span data-stu-id="a2ec8-159">toodetermine which Spark configurations need toobe set and toowhat values, see [What causes a Spark application OutofMemoryError exception](#what-causes-a-spark-application-outofmemoryerror-exception).</span></span> 

2. <span data-ttu-id="a2ec8-160">Skicka hello Spark programmet tooLivy med hjälp av REST-klient som cURL.</span><span class="sxs-lookup"><span data-stu-id="a2ec8-160">Submit hello Spark application tooLivy by using a REST client like cURL.</span></span> <span data-ttu-id="a2ec8-161">Använda en liknande toohello följande i kommandot.</span><span class="sxs-lookup"><span data-stu-id="a2ec8-161">Use a command similar toohello following.</span></span> <span data-ttu-id="a2ec8-162">Ändra hello faktiska värden efter behov:</span><span class="sxs-lookup"><span data-stu-id="a2ec8-162">Change hello actual values as necessary:</span></span>

    ```apache
    curl -k --user 'username:password' -v -H 'Content-Type: application/json' -X POST -d '{ "file":"wasb://container@storageaccountname.blob.core.windows.net/example/jars/sparkapplication.jar", "className":"com.microsoft.spark.application", "numExecutors":4, "executorMemory":"4g", "executorCores":2, "driverMemory":"8g", "driverCores":4}'  
    ```

### <a name="additional-reading"></a><span data-ttu-id="a2ec8-163">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="a2ec8-163">Additional reading</span></span>

[<span data-ttu-id="a2ec8-164">Jobbet för Spark i HDInsight-kluster</span><span class="sxs-lookup"><span data-stu-id="a2ec8-164">Spark job submission on HDInsight clusters</span></span>](https://blogs.msdn.microsoft.com/azuredatalake/2017/01/06/spark-job-submission-on-hdinsight-101/)


## <a name="how-do-i-configure-a-spark-application-by-using-spark-submit-on-clusters"></a><span data-ttu-id="a2ec8-165">Hur konfigurerar ett program med hjälp av spark-skicka Spark på kluster</span><span class="sxs-lookup"><span data-stu-id="a2ec8-165">How do I configure a Spark application by using spark-submit on clusters</span></span>

### <a name="resolution-steps"></a><span data-ttu-id="a2ec8-166">Lösningssteg</span><span class="sxs-lookup"><span data-stu-id="a2ec8-166">Resolution steps</span></span>

1. <span data-ttu-id="a2ec8-167">toodetermine vilka Spark-konfigurationer måste toobe uppsättningen och toowhat värden, se [vad som orsakar en Spark OutofMemoryError undantag](#what-causes-a-spark-application-outofmemoryerror-exception).</span><span class="sxs-lookup"><span data-stu-id="a2ec8-167">toodetermine which Spark configurations need toobe set and toowhat values, see [What causes a Spark application OutofMemoryError exception](#what-causes-a-spark-application-outofmemoryerror-exception).</span></span>

2. <span data-ttu-id="a2ec8-168">Starta spark-shell med hjälp av ett kommando liknande toohello som följer.</span><span class="sxs-lookup"><span data-stu-id="a2ec8-168">Launch spark-shell by using a command similar toohello following.</span></span> <span data-ttu-id="a2ec8-169">Ändra hello faktiska värdet för hello konfigurationer efter behov:</span><span class="sxs-lookup"><span data-stu-id="a2ec8-169">Change hello actual value of hello configurations as necessary:</span></span> 

    ```apache
    spark-submit --master yarn-cluster --class com.microsoft.spark.application --num-executors 4 --executor-memory 4g --executor-cores 2 --driver-memory 8g --driver-cores 4 /home/user/spark/sparkapplication.jar
    ```

### <a name="additional-reading"></a><span data-ttu-id="a2ec8-170">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="a2ec8-170">Additional reading</span></span>

[<span data-ttu-id="a2ec8-171">Jobbet för Spark i HDInsight-kluster</span><span class="sxs-lookup"><span data-stu-id="a2ec8-171">Spark job submission on HDInsight clusters</span></span>](https://blogs.msdn.microsoft.com/azuredatalake/2017/01/06/spark-job-submission-on-hdinsight-101/)


## <a name="what-causes-a-spark-application-outofmemoryerror-exception"></a><span data-ttu-id="a2ec8-172">Vad som orsakar en Spark OutofMemoryError undantag</span><span class="sxs-lookup"><span data-stu-id="a2ec8-172">What causes a Spark application OutofMemoryError exception</span></span>

### <a name="detailed-description"></a><span data-ttu-id="a2ec8-173">Detaljerad beskrivning</span><span class="sxs-lookup"><span data-stu-id="a2ec8-173">Detailed description</span></span>

<span data-ttu-id="a2ec8-174">hello Spark programmet misslyckas med följande typer av undantagsfel utan felhantering hello:</span><span class="sxs-lookup"><span data-stu-id="a2ec8-174">hello Spark application fails, with hello following types of uncaught exceptions:</span></span>

```apache
ERROR Executor: Exception in task 7.0 in stage 6.0 (TID 439) 

java.lang.OutOfMemoryError 
    at java.io.ByteArrayOutputStream.hugeCapacity(Unknown Source) 
    at java.io.ByteArrayOutputStream.grow(Unknown Source) 
    at java.io.ByteArrayOutputStream.ensureCapacity(Unknown Source) 
    at java.io.ByteArrayOutputStream.write(Unknown Source) 
    at java.io.ObjectOutputStream$BlockDataOutputStream.drain(Unknown Source) 
    at java.io.ObjectOutputStream$BlockDataOutputStream.setBlockDataMode(Unknown Source) 
    at java.io.ObjectOutputStream.writeObject0(Unknown Source) 
    at java.io.ObjectOutputStream.writeObject(Unknown Source) 
    at org.apache.spark.serializer.JavaSerializationStream.writeObject(JavaSerializer.scala:44) 
    at org.apache.spark.serializer.JavaSerializerInstance.serialize(JavaSerializer.scala:101) 
    at org.apache.spark.executor.Executor$TaskRunner.run(Executor.scala:239) 
    at java.util.concurrent.ThreadPoolExecutor.runWorker(Unknown Source) 
    at java.util.concurrent.ThreadPoolExecutor$Worker.run(Unknown Source) 
    at java.lang.Thread.run(Unknown Source) 
```

```apache
ERROR SparkUncaughtExceptionHandler: Uncaught exception in thread Thread[Executor task launch worker-0,5,main] 

java.lang.OutOfMemoryError 
    at java.io.ByteArrayOutputStream.hugeCapacity(Unknown Source) 
    at java.io.ByteArrayOutputStream.grow(Unknown Source) 
    at java.io.ByteArrayOutputStream.ensureCapacity(Unknown Source) 
    at java.io.ByteArrayOutputStream.write(Unknown Source) 
    at java.io.ObjectOutputStream$BlockDataOutputStream.drain(Unknown Source) 
    at java.io.ObjectOutputStream$BlockDataOutputStream.setBlockDataMode(Unknown Source) 
    at java.io.ObjectOutputStream.writeObject0(Unknown Source) 
    at java.io.ObjectOutputStream.writeObject(Unknown Source) 
    at org.apache.spark.serializer.JavaSerializationStream.writeObject(JavaSerializer.scala:44) 
    at org.apache.spark.serializer.JavaSerializerInstance.serialize(JavaSerializer.scala:101) 
    at org.apache.spark.executor.Executor$TaskRunner.run(Executor.scala:239) 
    at java.util.concurrent.ThreadPoolExecutor.runWorker(Unknown Source) 
    at java.util.concurrent.ThreadPoolExecutor$Worker.run(Unknown Source) 
    at java.lang.Thread.run(Unknown Source) 
```

### <a name="probable-cause"></a><span data-ttu-id="a2ec8-175">Möjlig orsak</span><span class="sxs-lookup"><span data-stu-id="a2ec8-175">Probable cause</span></span>

<span data-ttu-id="a2ec8-176">hello troligaste orsaken till det här undantaget är att det finns inte tillräckligt med minne för heap fördelas toohello Java virtuella datorer (JVMs).</span><span class="sxs-lookup"><span data-stu-id="a2ec8-176">hello most likely cause of this exception is that not enough heap memory is allocated toohello Java virtual machines (JVMs).</span></span> <span data-ttu-id="a2ec8-177">Dessa JVMs startas som executors eller drivrutiner som en del av hello Spark-program.</span><span class="sxs-lookup"><span data-stu-id="a2ec8-177">These JVMs are launched as executors or drivers as part of hello Spark application.</span></span> 

### <a name="resolution-steps"></a><span data-ttu-id="a2ec8-178">Lösningssteg</span><span class="sxs-lookup"><span data-stu-id="a2ec8-178">Resolution steps</span></span>

1. <span data-ttu-id="a2ec8-179">Fastställa hello maxstorleken för hello data hello Spark-program hanterar.</span><span class="sxs-lookup"><span data-stu-id="a2ec8-179">Determine hello maximum size of hello data hello Spark application handles.</span></span> <span data-ttu-id="a2ec8-180">Du kan göra en gissning baserat på hello maxstorleken för hello indata, hello mellanliggande data som produceras av omvandla hello indata och utdata för hello som skapas när programmet hello ytterligare förändrar hello mellanliggande data.</span><span class="sxs-lookup"><span data-stu-id="a2ec8-180">You can make a guess based on hello maximum size of hello input data, hello intermediate data that's produced by transforming hello input data, and hello output data that's produced when hello application is further transforming hello intermediate data.</span></span> <span data-ttu-id="a2ec8-181">Den här processen kan vara en iterativ om du inte göra en första formella gissning.</span><span class="sxs-lookup"><span data-stu-id="a2ec8-181">This process can be an iterative if you can't make an initial formal guess.</span></span> 

2. <span data-ttu-id="a2ec8-182">Se till att du ska toouse har tillräckligt med resurser när det gäller minne och kärnor tooaccommodate hello Spark programmet hello HDInsight klustret.</span><span class="sxs-lookup"><span data-stu-id="a2ec8-182">Make sure that hello HDInsight cluster that you're going toouse has enough resources in terms of memory and cores tooaccommodate hello Spark application.</span></span> <span data-ttu-id="a2ec8-183">Du kan bestämma det genom att visa hello klustret mått avsnitt i hello YARN-Användargränssnittet för hello värden av **minne används** vs. **Minne totalt**, och **VCores används** vs. **Totalt antal VCores**.</span><span class="sxs-lookup"><span data-stu-id="a2ec8-183">You can determine this by viewing hello cluster metrics section of hello YARN UI for hello values of **Memory Used** vs. **Memory Total**, and **VCores Used** vs. **VCores Total**.</span></span>

3. <span data-ttu-id="a2ec8-184">Ange följande Spark hello konfigurationer tooappropriate värden som inte får överstiga 90% av hello tillgängligt minne och kärnor.</span><span class="sxs-lookup"><span data-stu-id="a2ec8-184">Set hello following Spark configurations tooappropriate values, which should not exceed 90% of hello available memory and cores.</span></span> <span data-ttu-id="a2ec8-185">hello-värden ska vara bra i hello minneskrav för hello Spark-program:</span><span class="sxs-lookup"><span data-stu-id="a2ec8-185">hello values should be well within hello memory requirements of hello Spark application:</span></span> 

    ```apache
    spark.executor.instances (Example: 8 for 8 executor count) 
    spark.executor.memory (Example: 4g for 4 GB) 
    spark.yarn.executor.memoryOverhead (Example: 384m for 384 MB) 
    spark.executor.cores (Example: 2 for 2 cores per executor) 
    spark.driver.memory (Example: 8g for 8GB) 
    spark.driver.cores (Example: 4 for 4 cores)   
    spark.yarn.driver.memoryOverhead (Example: 384m for 384MB) 
    ```

    <span data-ttu-id="a2ec8-186">tooget hello totalt minne som används av alla executors, kör hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="a2ec8-186">tooget hello total memory used by all executors, run hello following command:</span></span> 
    
    ```apache
    spark.executor.instances * (spark.executor.memory + spark.yarn.executor.memoryOverhead) 
    ```
    <span data-ttu-id="a2ec8-187">tooget hello totalt minne som används av hello drivrutin, kör hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="a2ec8-187">tooget hello total memory used by hello driver, run hello following command:</span></span>
    
    ```apache
    spark.driver.memory + spark.yarn.driver.memoryOverhead
    ```

### <a name="additional-reading"></a><span data-ttu-id="a2ec8-188">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="a2ec8-188">Additional reading</span></span>

- [<span data-ttu-id="a2ec8-189">Översikt över Spark minne hantering</span><span class="sxs-lookup"><span data-stu-id="a2ec8-189">Spark memory management overview</span></span>](http://spark.apache.org/docs/latest/tuning.html#memory-management-overview)
- [<span data-ttu-id="a2ec8-190">Felsöka ett Spark-program på ett HDInsight-kluster</span><span class="sxs-lookup"><span data-stu-id="a2ec8-190">Debug a Spark application on an HDInsight cluster</span></span>](https://blogs.msdn.microsoft.com/azuredatalake/2016/12/19/spark-debugging-101/)

