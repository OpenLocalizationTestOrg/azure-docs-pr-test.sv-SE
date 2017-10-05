---
title: "Felsöka Spark med Azure HDInsight | Microsoft Docs"
description: "Få svar på vanliga frågor om hur du arbetar med Apache Spark och Azure HDInsight."
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
ms.openlocfilehash: cfed5f0f4f703821e83e3d365810c0e5ad22f035
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="troubleshoot-spark-by-using-azure-hdinsight"></a><span data-ttu-id="546dc-104">Felsöka Spark med Azure HDInsight</span><span class="sxs-lookup"><span data-stu-id="546dc-104">Troubleshoot Spark by using Azure HDInsight</span></span>

<span data-ttu-id="546dc-105">Läs mer om de vanligaste problemen och sina lösningar när du arbetar med Apache Spark nyttolaster i Apache Ambari.</span><span class="sxs-lookup"><span data-stu-id="546dc-105">Learn about the top issues and their resolutions when working with Apache Spark payloads in Apache Ambari.</span></span>

## <a name="how-do-i-configure-a-spark-application-by-using-ambari-on-clusters"></a><span data-ttu-id="546dc-106">Hur konfigurerar jag ett Spark-program genom att använda Ambari på kluster</span><span class="sxs-lookup"><span data-stu-id="546dc-106">How do I configure a Spark application by using Ambari on clusters</span></span>

### <a name="resolution-steps"></a><span data-ttu-id="546dc-107">Lösningssteg</span><span class="sxs-lookup"><span data-stu-id="546dc-107">Resolution steps</span></span>

<span data-ttu-id="546dc-108">Tidigare konfigurationsvärden för den här proceduren har ställts in i HDInsight.</span><span class="sxs-lookup"><span data-stu-id="546dc-108">The configuration values for this procedure were previously set in HDInsight.</span></span> <span data-ttu-id="546dc-109">För att fastställa vilka Spark måste anges och vilka värden, se [vad som orsakar en Spark OutofMemoryError undantag](#what-causes-a-spark-application-outofmemoryerror-exception).</span><span class="sxs-lookup"><span data-stu-id="546dc-109">To determine which Spark configurations need to be set and to what values, see [What causes a Spark application OutofMemoryError exception](#what-causes-a-spark-application-outofmemoryerror-exception).</span></span> 

1. <span data-ttu-id="546dc-110">Välj i listan över kluster **Spark2**.</span><span class="sxs-lookup"><span data-stu-id="546dc-110">In the list of clusters, select **Spark2**.</span></span>

    ![Markera klustret från listan](media/hdinsight-troubleshoot-spark/update-config-1.png)

2. <span data-ttu-id="546dc-112">Välj den **konfigurationerna** fliken.</span><span class="sxs-lookup"><span data-stu-id="546dc-112">Select the **Configs** tab.</span></span>

    ![Välj fliken konfigurationer](media/hdinsight-troubleshoot-spark/update-config-2.png)

3. <span data-ttu-id="546dc-114">Välj i listan över konfigurationer, **anpassad-spark2-standarder**.</span><span class="sxs-lookup"><span data-stu-id="546dc-114">In the list of configurations, select **Custom-spark2-defaults**.</span></span>

    ![Ange standardinställningar för anpassad spark](media/hdinsight-troubleshoot-spark/update-config-3.png)

4. <span data-ttu-id="546dc-116">Leta efter inställningen värdet som du måste justera som **spark.executor.memory**.</span><span class="sxs-lookup"><span data-stu-id="546dc-116">Look for the value setting that you need to adjust, such as **spark.executor.memory**.</span></span> <span data-ttu-id="546dc-117">I det här fallet är värdet för **4608m** är för hög.</span><span class="sxs-lookup"><span data-stu-id="546dc-117">In this case, the value of **4608m** is too high.</span></span>

    ![Markera fältet spark.executor.memory](media/hdinsight-troubleshoot-spark/update-config-4.png)

5. <span data-ttu-id="546dc-119">Ange värdet till den rekommenderade inställningen.</span><span class="sxs-lookup"><span data-stu-id="546dc-119">Set the value to the recommended setting.</span></span> <span data-ttu-id="546dc-120">Värdet **2 048 m** rekommenderas för den här inställningen.</span><span class="sxs-lookup"><span data-stu-id="546dc-120">The value **2048m** is recommended for this setting.</span></span>

    ![Ändra värdet till 2 048 m](media/hdinsight-troubleshoot-spark/update-config-5.png)

6. <span data-ttu-id="546dc-122">Spara värdet och spara sedan konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="546dc-122">Save the value, and then save the configuration.</span></span> <span data-ttu-id="546dc-123">I verktygsfältet väljer **spara**.</span><span class="sxs-lookup"><span data-stu-id="546dc-123">On the toolbar, select **Save**.</span></span>

    ![Spara inställningen och konfiguration](media/hdinsight-troubleshoot-spark/update-config-6a.png)

    <span data-ttu-id="546dc-125">Du meddelas om några konfigurationer behöver åtgärdas.</span><span class="sxs-lookup"><span data-stu-id="546dc-125">You are notified if any configurations need attention.</span></span> <span data-ttu-id="546dc-126">Observera objekt och välj sedan **fortsätta ändå**.</span><span class="sxs-lookup"><span data-stu-id="546dc-126">Note the items, and then select **Proceed Anyway**.</span></span> 

    ![Välj fortsätter ändå](media/hdinsight-troubleshoot-spark/update-config-6b.png)

    <span data-ttu-id="546dc-128">Skriv en anteckning om ändringar i konfigurationen och välj sedan **spara**.</span><span class="sxs-lookup"><span data-stu-id="546dc-128">Write a note about the configuration changes, and then select **Save**.</span></span>

    ![Ange en notering om de ändringar du gjort](media/hdinsight-troubleshoot-spark/update-config-6c.png)

7. <span data-ttu-id="546dc-130">När en konfiguration sparas, uppmanas du att starta om tjänsten.</span><span class="sxs-lookup"><span data-stu-id="546dc-130">Whenever a configuration is saved, you are prompted to restart the service.</span></span> <span data-ttu-id="546dc-131">Välj **starta om**.</span><span class="sxs-lookup"><span data-stu-id="546dc-131">Select **Restart**.</span></span>

    ![Välj omstart](media/hdinsight-troubleshoot-spark/update-config-7a.png)

    <span data-ttu-id="546dc-133">Bekräfta omstarten.</span><span class="sxs-lookup"><span data-stu-id="546dc-133">Confirm the restart.</span></span>

    ![Välj bekräfta starta om alla](media/hdinsight-troubleshoot-spark/update-config-7b.png)

    <span data-ttu-id="546dc-135">Du kan granska de processer som körs.</span><span class="sxs-lookup"><span data-stu-id="546dc-135">You can review the processes that are running.</span></span>

    ![Granska processer som körs](media/hdinsight-troubleshoot-spark/update-config-7c.png)

8. <span data-ttu-id="546dc-137">Du kan lägga till konfigurationer.</span><span class="sxs-lookup"><span data-stu-id="546dc-137">You can add configurations.</span></span> <span data-ttu-id="546dc-138">Välj i listan över konfigurationer, **anpassad-spark2-standarder**, och välj sedan **lägga till egenskapen**.</span><span class="sxs-lookup"><span data-stu-id="546dc-138">In the list of configurations, select **Custom-spark2-defaults**, and then select **Add Property**.</span></span>

    ![Välj Lägg till egenskap](media/hdinsight-troubleshoot-spark/update-config-8.png)

9. <span data-ttu-id="546dc-140">Definiera en ny egenskap.</span><span class="sxs-lookup"><span data-stu-id="546dc-140">Define a new property.</span></span> <span data-ttu-id="546dc-141">Du kan definiera en egenskap med hjälp av en dialogruta för specifika inställningar, till exempel datatypen.</span><span class="sxs-lookup"><span data-stu-id="546dc-141">You can define a single property by using a dialog box for specific settings such as the data type.</span></span> <span data-ttu-id="546dc-142">Eller så du kan definiera flera egenskaper med hjälp av en definition av per rad.</span><span class="sxs-lookup"><span data-stu-id="546dc-142">Or, you can define multiple properties by using one definition per line.</span></span> 

    <span data-ttu-id="546dc-143">I det här exemplet i **spark.driver.memory** egenskapen har definierats med värdet **4g**.</span><span class="sxs-lookup"><span data-stu-id="546dc-143">In this example, the **spark.driver.memory** property is defined with a value of **4g**.</span></span>

    ![Definiera nya egenskap](media/hdinsight-troubleshoot-spark/update-config-9.png)

10. <span data-ttu-id="546dc-145">Spara konfigurationen och sedan starta om tjänsten enligt beskrivningen i steg 6 och 7.</span><span class="sxs-lookup"><span data-stu-id="546dc-145">Save the configuration, and then restart the service as described in steps 6 and 7.</span></span>

<span data-ttu-id="546dc-146">De här ändringarna kan åsidosättas när du har skickat jobbet Spark är klustret.</span><span class="sxs-lookup"><span data-stu-id="546dc-146">These changes are cluster-wide but can be overridden when you submit the Spark job.</span></span>

### <a name="additional-reading"></a><span data-ttu-id="546dc-147">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="546dc-147">Additional reading</span></span>

[<span data-ttu-id="546dc-148">Jobbet för Spark i HDInsight-kluster</span><span class="sxs-lookup"><span data-stu-id="546dc-148">Spark job submission on HDInsight clusters</span></span>](https://blogs.msdn.microsoft.com/azuredatalake/2017/01/06/spark-job-submission-on-hdinsight-101/)


## <a name="how-do-i-configure-a-spark-application-by-using-a-jupyter-notebook-on-clusters"></a><span data-ttu-id="546dc-149">Hur konfigurerar jag ett Spark-program med hjälp av en Jupyter-anteckningsbok på kluster</span><span class="sxs-lookup"><span data-stu-id="546dc-149">How do I configure a Spark application by using a Jupyter notebook on clusters</span></span>

### <a name="resolution-steps"></a><span data-ttu-id="546dc-150">Lösningssteg</span><span class="sxs-lookup"><span data-stu-id="546dc-150">Resolution steps</span></span>

1. <span data-ttu-id="546dc-151">För att fastställa vilka Spark måste anges och vilka värden, se [vad som orsakar en Spark OutofMemoryError undantag](#what-causes-a-spark-application-outofmemoryerror-exception).</span><span class="sxs-lookup"><span data-stu-id="546dc-151">To determine which Spark configurations need to be set and to what values, see [What causes a Spark application OutofMemoryError exception](#what-causes-a-spark-application-outofmemoryerror-exception).</span></span>

2. <span data-ttu-id="546dc-152">I den första cellen i Jupyter-anteckningsbok när den **%% konfigurera** direktiv, ange Spark-konfigurationer i ett giltigt JSON-format.</span><span class="sxs-lookup"><span data-stu-id="546dc-152">In the first cell of the Jupyter notebook, after the **%%configure** directive, specify the Spark configurations in valid JSON format.</span></span> <span data-ttu-id="546dc-153">Ändra värden efter behov:</span><span class="sxs-lookup"><span data-stu-id="546dc-153">Change the actual values as necessary:</span></span>

    ![Lägga till en konfiguration](media/hdinsight-troubleshoot-spark/add-configuration-cell.png)

### <a name="additional-reading"></a><span data-ttu-id="546dc-155">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="546dc-155">Additional reading</span></span>

[<span data-ttu-id="546dc-156">Jobbet för Spark i HDInsight-kluster</span><span class="sxs-lookup"><span data-stu-id="546dc-156">Spark job submission on HDInsight clusters</span></span>](https://blogs.msdn.microsoft.com/azuredatalake/2017/01/06/spark-job-submission-on-hdinsight-101/)


## <a name="how-do-i-configure-a-spark-application-by-using-livy-on-clusters"></a><span data-ttu-id="546dc-157">Hur konfigurerar jag ett Spark-program med hjälp av Livius på kluster</span><span class="sxs-lookup"><span data-stu-id="546dc-157">How do I configure a Spark application by using Livy on clusters</span></span>

### <a name="resolution-steps"></a><span data-ttu-id="546dc-158">Lösningssteg</span><span class="sxs-lookup"><span data-stu-id="546dc-158">Resolution steps</span></span>

1. <span data-ttu-id="546dc-159">För att fastställa vilka Spark måste anges och vilka värden, se [vad som orsakar en Spark OutofMemoryError undantag](#what-causes-a-spark-application-outofmemoryerror-exception).</span><span class="sxs-lookup"><span data-stu-id="546dc-159">To determine which Spark configurations need to be set and to what values, see [What causes a Spark application OutofMemoryError exception](#what-causes-a-spark-application-outofmemoryerror-exception).</span></span> 

2. <span data-ttu-id="546dc-160">Skicka Spark-program till Livius med hjälp av REST-klient som cURL.</span><span class="sxs-lookup"><span data-stu-id="546dc-160">Submit the Spark application to Livy by using a REST client like cURL.</span></span> <span data-ttu-id="546dc-161">Använda ett kommando som liknar följande.</span><span class="sxs-lookup"><span data-stu-id="546dc-161">Use a command similar to the following.</span></span> <span data-ttu-id="546dc-162">Ändra värden efter behov:</span><span class="sxs-lookup"><span data-stu-id="546dc-162">Change the actual values as necessary:</span></span>

    ```apache
    curl -k --user 'username:password' -v -H 'Content-Type: application/json' -X POST -d '{ "file":"wasb://container@storageaccountname.blob.core.windows.net/example/jars/sparkapplication.jar", "className":"com.microsoft.spark.application", "numExecutors":4, "executorMemory":"4g", "executorCores":2, "driverMemory":"8g", "driverCores":4}'  
    ```

### <a name="additional-reading"></a><span data-ttu-id="546dc-163">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="546dc-163">Additional reading</span></span>

[<span data-ttu-id="546dc-164">Jobbet för Spark i HDInsight-kluster</span><span class="sxs-lookup"><span data-stu-id="546dc-164">Spark job submission on HDInsight clusters</span></span>](https://blogs.msdn.microsoft.com/azuredatalake/2017/01/06/spark-job-submission-on-hdinsight-101/)


## <a name="how-do-i-configure-a-spark-application-by-using-spark-submit-on-clusters"></a><span data-ttu-id="546dc-165">Hur konfigurerar ett program med hjälp av spark-skicka Spark på kluster</span><span class="sxs-lookup"><span data-stu-id="546dc-165">How do I configure a Spark application by using spark-submit on clusters</span></span>

### <a name="resolution-steps"></a><span data-ttu-id="546dc-166">Lösningssteg</span><span class="sxs-lookup"><span data-stu-id="546dc-166">Resolution steps</span></span>

1. <span data-ttu-id="546dc-167">För att fastställa vilka Spark måste anges och vilka värden, se [vad som orsakar en Spark OutofMemoryError undantag](#what-causes-a-spark-application-outofmemoryerror-exception).</span><span class="sxs-lookup"><span data-stu-id="546dc-167">To determine which Spark configurations need to be set and to what values, see [What causes a Spark application OutofMemoryError exception](#what-causes-a-spark-application-outofmemoryerror-exception).</span></span>

2. <span data-ttu-id="546dc-168">Starta spark-gränssnittet med ett kommando som liknar följande.</span><span class="sxs-lookup"><span data-stu-id="546dc-168">Launch spark-shell by using a command similar to the following.</span></span> <span data-ttu-id="546dc-169">Ändra det faktiska värdet för konfigurationerna som behövs:</span><span class="sxs-lookup"><span data-stu-id="546dc-169">Change the actual value of the configurations as necessary:</span></span> 

    ```apache
    spark-submit --master yarn-cluster --class com.microsoft.spark.application --num-executors 4 --executor-memory 4g --executor-cores 2 --driver-memory 8g --driver-cores 4 /home/user/spark/sparkapplication.jar
    ```

### <a name="additional-reading"></a><span data-ttu-id="546dc-170">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="546dc-170">Additional reading</span></span>

[<span data-ttu-id="546dc-171">Jobbet för Spark i HDInsight-kluster</span><span class="sxs-lookup"><span data-stu-id="546dc-171">Spark job submission on HDInsight clusters</span></span>](https://blogs.msdn.microsoft.com/azuredatalake/2017/01/06/spark-job-submission-on-hdinsight-101/)


## <a name="what-causes-a-spark-application-outofmemoryerror-exception"></a><span data-ttu-id="546dc-172">Vad som orsakar en Spark OutofMemoryError undantag</span><span class="sxs-lookup"><span data-stu-id="546dc-172">What causes a Spark application OutofMemoryError exception</span></span>

### <a name="detailed-description"></a><span data-ttu-id="546dc-173">Detaljerad beskrivning</span><span class="sxs-lookup"><span data-stu-id="546dc-173">Detailed description</span></span>

<span data-ttu-id="546dc-174">Spark-programmet misslyckas med följande typer av undantagsfel utan felhantering:</span><span class="sxs-lookup"><span data-stu-id="546dc-174">The Spark application fails, with the following types of uncaught exceptions:</span></span>

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

### <a name="probable-cause"></a><span data-ttu-id="546dc-175">Möjlig orsak</span><span class="sxs-lookup"><span data-stu-id="546dc-175">Probable cause</span></span>

<span data-ttu-id="546dc-176">Den mest troliga orsaken till det här undantaget är att det finns inte tillräckligt med minne för heap tilldelas Java virtuella datorer (JVMs).</span><span class="sxs-lookup"><span data-stu-id="546dc-176">The most likely cause of this exception is that not enough heap memory is allocated to the Java virtual machines (JVMs).</span></span> <span data-ttu-id="546dc-177">Dessa JVMs startas som executors eller drivrutiner som en del av Spark-program.</span><span class="sxs-lookup"><span data-stu-id="546dc-177">These JVMs are launched as executors or drivers as part of the Spark application.</span></span> 

### <a name="resolution-steps"></a><span data-ttu-id="546dc-178">Lösningssteg</span><span class="sxs-lookup"><span data-stu-id="546dc-178">Resolution steps</span></span>

1. <span data-ttu-id="546dc-179">Avgöra den maximala storleken på data i Spark programmet hanterar.</span><span class="sxs-lookup"><span data-stu-id="546dc-179">Determine the maximum size of the data the Spark application handles.</span></span> <span data-ttu-id="546dc-180">Du kan göra en gissning baserat på den maximala storleken för indata, mellanliggande data som produceras av omvandla indata och utdata som skapas när programmet ytterligare förändrar mellanliggande data.</span><span class="sxs-lookup"><span data-stu-id="546dc-180">You can make a guess based on the maximum size of the input data, the intermediate data that's produced by transforming the input data, and the output data that's produced when the application is further transforming the intermediate data.</span></span> <span data-ttu-id="546dc-181">Den här processen kan vara en iterativ om du inte göra en första formella gissning.</span><span class="sxs-lookup"><span data-stu-id="546dc-181">This process can be an iterative if you can't make an initial formal guess.</span></span> 

2. <span data-ttu-id="546dc-182">Se till att HDInsight-klustret som du ska använda har tillräckligt med resurser när det gäller minne och kärnor för Spark-program.</span><span class="sxs-lookup"><span data-stu-id="546dc-182">Make sure that the HDInsight cluster that you're going to use has enough resources in terms of memory and cores to accommodate the Spark application.</span></span> <span data-ttu-id="546dc-183">Du kan bestämma det genom att visa avsnittet kluster mått i YARN-Användargränssnittet för värden på **minne används** vs. **Minne totalt**, och **VCores används** vs. **Totalt antal VCores**.</span><span class="sxs-lookup"><span data-stu-id="546dc-183">You can determine this by viewing the cluster metrics section of the YARN UI for the values of **Memory Used** vs. **Memory Total**, and **VCores Used** vs. **VCores Total**.</span></span>

3. <span data-ttu-id="546dc-184">Ange följande Spark konfigurationer till lämpliga värden som inte får överstiga 90% av det tillgängliga minnet och kärnor.</span><span class="sxs-lookup"><span data-stu-id="546dc-184">Set the following Spark configurations to appropriate values, which should not exceed 90% of the available memory and cores.</span></span> <span data-ttu-id="546dc-185">Värdena bör vara bra i minneskrav för Spark-program:</span><span class="sxs-lookup"><span data-stu-id="546dc-185">The values should be well within the memory requirements of the Spark application:</span></span> 

    ```apache
    spark.executor.instances (Example: 8 for 8 executor count) 
    spark.executor.memory (Example: 4g for 4 GB) 
    spark.yarn.executor.memoryOverhead (Example: 384m for 384 MB) 
    spark.executor.cores (Example: 2 for 2 cores per executor) 
    spark.driver.memory (Example: 8g for 8GB) 
    spark.driver.cores (Example: 4 for 4 cores)   
    spark.yarn.driver.memoryOverhead (Example: 384m for 384MB) 
    ```

    <span data-ttu-id="546dc-186">För att få den totala mängden minne som används av alla executors, kör du följande kommando:</span><span class="sxs-lookup"><span data-stu-id="546dc-186">To get the total memory used by all executors, run the following command:</span></span> 
    
    ```apache
    spark.executor.instances * (spark.executor.memory + spark.yarn.executor.memoryOverhead) 
    ```
    <span data-ttu-id="546dc-187">För att få den totala mängden minne som används av drivrutinen, kör du följande kommando:</span><span class="sxs-lookup"><span data-stu-id="546dc-187">To get the total memory used by the driver, run the following command:</span></span>
    
    ```apache
    spark.driver.memory + spark.yarn.driver.memoryOverhead
    ```

### <a name="additional-reading"></a><span data-ttu-id="546dc-188">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="546dc-188">Additional reading</span></span>

- [<span data-ttu-id="546dc-189">Översikt över Spark minne hantering</span><span class="sxs-lookup"><span data-stu-id="546dc-189">Spark memory management overview</span></span>](http://spark.apache.org/docs/latest/tuning.html#memory-management-overview)
- [<span data-ttu-id="546dc-190">Felsöka ett Spark-program på ett HDInsight-kluster</span><span class="sxs-lookup"><span data-stu-id="546dc-190">Debug a Spark application on an HDInsight cluster</span></span>](https://blogs.msdn.microsoft.com/azuredatalake/2016/12/19/spark-debugging-101/)

