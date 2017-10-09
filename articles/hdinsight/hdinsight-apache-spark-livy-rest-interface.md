---
title: aaaUse Livius Spark toosubmit jobb tooSpark-kluster i Azure HDInsight | Microsoft Docs
description: "Lär dig hur toouse Apache Spark REST API toosubmit Spark jobb via fjärranslutning tooan Azure HDInsight-kluster."
keywords: Apache spark rest-api, Livius spark
services: hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 2817b779-1594-486b-8759-489379ca907d
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/25/2017
ms.author: nitinme
ms.openlocfilehash: 417213b5f1db1522373188002fe05117faea5243
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-apache-spark-rest-api-toosubmit-remote-jobs-tooan-hdinsight-spark-cluster"></a><span data-ttu-id="703b3-104">Använda Apache Spark REST API toosubmit remote jobb tooan HDInsight Spark-kluster</span><span class="sxs-lookup"><span data-stu-id="703b3-104">Use Apache Spark REST API toosubmit remote jobs tooan HDInsight Spark cluster</span></span>

<span data-ttu-id="703b3-105">Lär dig hur toouse Livius hello Apache Spark REST-API som används toosubmit remote jobb tooan Azure HDInsight Spark-kluster.</span><span class="sxs-lookup"><span data-stu-id="703b3-105">Learn how toouse Livy, hello Apache Spark REST API, which is used toosubmit remote jobs tooan Azure HDInsight Spark cluster.</span></span> <span data-ttu-id="703b3-106">Mer detaljerad dokumentation finns [Livius](https://github.com/cloudera/hue/tree/master/apps/spark/java#welcome-to-livy-the-rest-spark-server).</span><span class="sxs-lookup"><span data-stu-id="703b3-106">For detailed documentation, see [Livy](https://github.com/cloudera/hue/tree/master/apps/spark/java#welcome-to-livy-the-rest-spark-server).</span></span>

<span data-ttu-id="703b3-107">Du kan använda Livius toorun interaktiva Spark tankar eller skicka batch jobb toobe körs på Spark.</span><span class="sxs-lookup"><span data-stu-id="703b3-107">You can use Livy toorun interactive Spark shells or submit batch jobs toobe run on Spark.</span></span> <span data-ttu-id="703b3-108">Den här artikeln handlar om med hjälp av Livius toosubmit batchjobb.</span><span class="sxs-lookup"><span data-stu-id="703b3-108">This article talks about using Livy toosubmit batch jobs.</span></span> <span data-ttu-id="703b3-109">hello kodavsnitt i den här artikeln använder cURL toomake REST API-anrop toohello Livius Spark-slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="703b3-109">hello snippets in this article use cURL toomake REST API calls toohello Livy Spark endpoint.</span></span>

<span data-ttu-id="703b3-110">**Krav:**</span><span class="sxs-lookup"><span data-stu-id="703b3-110">**Prerequisites:**</span></span>

* <span data-ttu-id="703b3-111">Ett Apache Spark-kluster i HDInsight.</span><span class="sxs-lookup"><span data-stu-id="703b3-111">An Apache Spark cluster on HDInsight.</span></span> <span data-ttu-id="703b3-112">Instruktioner finns i [skapa Apache Spark-kluster i Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).</span><span class="sxs-lookup"><span data-stu-id="703b3-112">For instructions, see [Create Apache Spark clusters in Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).</span></span>

* <span data-ttu-id="703b3-113">[cURL](http://curl.haxx.se/).</span><span class="sxs-lookup"><span data-stu-id="703b3-113">[cURL](http://curl.haxx.se/).</span></span> <span data-ttu-id="703b3-114">Den här artikeln använder cURL toodemonstrate hur toomake REST API-anrop mot ett HDInsight Spark-kluster.</span><span class="sxs-lookup"><span data-stu-id="703b3-114">This article uses cURL toodemonstrate how toomake REST API calls against an HDInsight Spark cluster.</span></span>

## <a name="submit-a-livy-spark-batch-job"></a><span data-ttu-id="703b3-115">Skicka ett Livius Spark batchjobb</span><span class="sxs-lookup"><span data-stu-id="703b3-115">Submit a Livy Spark batch job</span></span>
<span data-ttu-id="703b3-116">Du måste överföra hello programmet jar på hello klusterlagringen som associerats med hello kluster innan du skickar ett batchjobb.</span><span class="sxs-lookup"><span data-stu-id="703b3-116">Before you submit a batch job, you must upload hello application jar on hello cluster storage associated with hello cluster.</span></span> <span data-ttu-id="703b3-117">Du kan använda [ **AzCopy**](../storage/common/storage-use-azcopy.md), ett kommandoradsverktyg, toodo så.</span><span class="sxs-lookup"><span data-stu-id="703b3-117">You can use [**AzCopy**](../storage/common/storage-use-azcopy.md), a command-line utility, toodo so.</span></span> <span data-ttu-id="703b3-118">Det finns olika klienter du kan använda tooupload data.</span><span class="sxs-lookup"><span data-stu-id="703b3-118">There are various other clients you can use tooupload data.</span></span> <span data-ttu-id="703b3-119">Du kan hitta mer om dem i [överföra data för Hadoop-jobb i HDInsight](hdinsight-upload-data.md).</span><span class="sxs-lookup"><span data-stu-id="703b3-119">You can find more about them at [Upload data for Hadoop jobs in HDInsight](hdinsight-upload-data.md).</span></span>

    curl -k --user "<hdinsight user>:<user password>" -v -H <content-type> -X POST -d '{ "file":"<path tooapplication jar>", "className":"<classname in jar>" }' 'https://<spark_cluster_name>.azurehdinsight.net/livy/batches'

<span data-ttu-id="703b3-120">**Exempel**:</span><span class="sxs-lookup"><span data-stu-id="703b3-120">**Examples**:</span></span>

* <span data-ttu-id="703b3-121">Om hello jar-filen finns på hello klustret storage (WASB)</span><span class="sxs-lookup"><span data-stu-id="703b3-121">If hello jar file is on hello cluster storage (WASB)</span></span>
  
        curl -k --user "admin:mypassword1!" -v -H 'Content-Type: application/json' -X POST -d '{ "file":"wasb://mycontainer@mystorageaccount.blob.core.windows.net/data/SparkSimpleTest.jar", "className":"com.microsoft.spark.test.SimpleFile" }' "https://mysparkcluster.azurehdinsight.net/livy/batches"
* <span data-ttu-id="703b3-122">Om du vill toopass hello jar filnamn och hello klassnamn som en del av en indatafil (i det här exemplet input.txt)</span><span class="sxs-lookup"><span data-stu-id="703b3-122">If you want toopass hello jar filename and hello classname as part of an input file (in this example, input.txt)</span></span>
  
        curl -k  --user "admin:mypassword1!" -v -H "Content-Type: application/json" -X POST --data @C:\Temp\input.txt "https://mysparkcluster.azurehdinsight.net/livy/batches"

## <a name="get-information-on-livy-spark-batches-running-on-hello-cluster"></a><span data-ttu-id="703b3-123">Hämta information om Livius Spark batchar körs på hello kluster</span><span class="sxs-lookup"><span data-stu-id="703b3-123">Get information on Livy Spark batches running on hello cluster</span></span>
    curl -k --user "<hdinsight user>:<user password>" -v -X GET "https://<spark_cluster_name>.azurehdinsight.net/livy/batches"

<span data-ttu-id="703b3-124">**Exempel**:</span><span class="sxs-lookup"><span data-stu-id="703b3-124">**Examples**:</span></span>

* <span data-ttu-id="703b3-125">Om du vill tooretrieve alla hello Livius Spark batchar som körs på klustret hello:</span><span class="sxs-lookup"><span data-stu-id="703b3-125">If you want tooretrieve all hello Livy Spark batches running on hello cluster:</span></span>
  
        curl -k --user "admin:mypassword1!" -v -X GET "https://mysparkcluster.azurehdinsight.net/livy/batches"
* <span data-ttu-id="703b3-126">Om du vill tooretrieve en specifik grupp med en viss batch-ID</span><span class="sxs-lookup"><span data-stu-id="703b3-126">If you want tooretrieve a specific batch with a given batchId</span></span>
  
        curl -k --user "admin:mypassword1!" -v -X GET "https://mysparkcluster.azurehdinsight.net/livy/batches/{batchId}"

## <a name="delete-a-livy-spark-batch-job"></a><span data-ttu-id="703b3-127">Ta bort batchjobb Livius Spark</span><span class="sxs-lookup"><span data-stu-id="703b3-127">Delete a Livy Spark batch job</span></span>
    curl -k --user "<hdinsight user>:<user password>" -v -X DELETE "https://<spark_cluster_name>.azurehdinsight.net/livy/batches/{batchId}"

<span data-ttu-id="703b3-128">**Exempel**:</span><span class="sxs-lookup"><span data-stu-id="703b3-128">**Example**:</span></span>

    curl -k --user "admin:mypassword1!" -v -X DELETE "https://mysparkcluster.azurehdinsight.net/livy/batches/{batchId}"

## <a name="livy-spark-and-high-availability"></a><span data-ttu-id="703b3-129">Livius Spark och hög tillgänglighet</span><span class="sxs-lookup"><span data-stu-id="703b3-129">Livy Spark and high-availability</span></span>
<span data-ttu-id="703b3-130">Livius ger hög tillgänglighet för Spark-jobb som körs på hello klustret.</span><span class="sxs-lookup"><span data-stu-id="703b3-130">Livy provides high-availability for Spark jobs running on hello cluster.</span></span> <span data-ttu-id="703b3-131">Här är några exempel.</span><span class="sxs-lookup"><span data-stu-id="703b3-131">Here is a couple of examples.</span></span>

* <span data-ttu-id="703b3-132">Om hello Livius tjänsten stängs av när du har skickat ett jobb via fjärranslutning tooa Spark-kluster, hello jobbet fortsätter toorun i hello bakgrund.</span><span class="sxs-lookup"><span data-stu-id="703b3-132">If hello Livy service goes down after you have submitted a job remotely tooa Spark cluster, hello job continues toorun in hello background.</span></span> <span data-ttu-id="703b3-133">När Livius säkerhetskopiera återställer hello status för hello jobb och rapporter som det igen.</span><span class="sxs-lookup"><span data-stu-id="703b3-133">When Livy is back up, it restores hello status of hello job and reports it back.</span></span>
* <span data-ttu-id="703b3-134">Jupyter-anteckningsböcker för HDInsight är drivs av Livius i hello backend.</span><span class="sxs-lookup"><span data-stu-id="703b3-134">Jupyter notebooks for HDInsight are powered by Livy in hello backend.</span></span> <span data-ttu-id="703b3-135">Om en bärbar dator med ett Spark-jobb och hello Livius service hämtar startas om, fortsätter hello anteckningsboken toorun hello kod celler.</span><span class="sxs-lookup"><span data-stu-id="703b3-135">If a notebook is running a Spark job and hello Livy service gets restarted, hello notebook continues toorun hello code cells.</span></span> 

## <a name="show-me-an-example"></a><span data-ttu-id="703b3-136">Visa ett exempel</span><span class="sxs-lookup"><span data-stu-id="703b3-136">Show me an example</span></span>
<span data-ttu-id="703b3-137">I det här avsnittet vi titta på exempel toouse Livius Spark toosubmit batchjobb, övervaka hello hello jobb och tar bort den.</span><span class="sxs-lookup"><span data-stu-id="703b3-137">In this section, we look at examples toouse Livy Spark toosubmit batch job, monitor hello progress of hello job, and then delete it.</span></span> <span data-ttu-id="703b3-138">hello programmet vi använder i det här exemplet är hello en utvecklats i hello artikeln [skapa en fristående Scala program och toorun på HDInsight Spark-kluster](hdinsight-apache-spark-create-standalone-application.md).</span><span class="sxs-lookup"><span data-stu-id="703b3-138">hello application we use in this example is hello one developed in hello article [Create a standalone Scala application and toorun on HDInsight Spark cluster](hdinsight-apache-spark-create-standalone-application.md).</span></span> <span data-ttu-id="703b3-139">hello stegen här förutsätter som:</span><span class="sxs-lookup"><span data-stu-id="703b3-139">hello steps here assume that:</span></span>

* <span data-ttu-id="703b3-140">Du har redan kopieras över hello programmet jar toohello storage-konto som är associerade med hello-kluster.</span><span class="sxs-lookup"><span data-stu-id="703b3-140">You have already copied over hello application jar toohello storage account associated with hello cluster.</span></span>
* <span data-ttu-id="703b3-141">Du har CuRL som installerats på hello datorn där du försöker de här stegen.</span><span class="sxs-lookup"><span data-stu-id="703b3-141">You have CuRL installed on hello computer where you are trying these steps.</span></span>

<span data-ttu-id="703b3-142">Utför följande steg hello:</span><span class="sxs-lookup"><span data-stu-id="703b3-142">Perform hello following steps:</span></span>

1. <span data-ttu-id="703b3-143">Låt oss först kontrollera att Livius Spark körs på hello klustret.</span><span class="sxs-lookup"><span data-stu-id="703b3-143">Let us first verify that Livy Spark is running on hello cluster.</span></span> <span data-ttu-id="703b3-144">Vi kan göra det genom att hämta en lista över aktiva batchar.</span><span class="sxs-lookup"><span data-stu-id="703b3-144">We can do so by getting a list of running batches.</span></span> <span data-ttu-id="703b3-145">Om du kör ett jobb med Livy för hello första gången ska hello utdata returnera noll.</span><span class="sxs-lookup"><span data-stu-id="703b3-145">If you are running a job using Livy for hello first time, hello output should return zero.</span></span>
   
        curl -k --user "admin:mypassword1!" -v -X GET "https://mysparkcluster.azurehdinsight.net/livy/batches"
   
    <span data-ttu-id="703b3-146">Du bör få en liknande utdata-toohello följande fragment:</span><span class="sxs-lookup"><span data-stu-id="703b3-146">You should get an output similar toohello following snippet:</span></span>
   
        < HTTP/1.1 200 OK
        < Content-Type: application/json; charset=UTF-8
        < Server: Microsoft-IIS/8.5
        < X-Powered-By: ARR/2.5
        < X-Powered-By: ASP.NET
        < Date: Fri, 20 Nov 2015 23:47:53 GMT
        < Content-Length: 34
        <
        {"from":0,"total":0,"sessions":[]}* Connection #0 toohost mysparkcluster.azurehdinsight.net left intact
   
    <span data-ttu-id="703b3-147">Observera hur hello sista raden i hello utdata står **totalt: 0**, som föreslås inga körs batchar.</span><span class="sxs-lookup"><span data-stu-id="703b3-147">Notice how hello last line in hello output says **total:0**, which suggests no running batches.</span></span>

2. <span data-ttu-id="703b3-148">Låt oss nu skicka batchjobb.</span><span class="sxs-lookup"><span data-stu-id="703b3-148">Let us now submit a batch job.</span></span> <span data-ttu-id="703b3-149">följande fragment hello använder en indatafilen (input.txt) toopass hello jar namn och hello-klassnamn som parametrar.</span><span class="sxs-lookup"><span data-stu-id="703b3-149">hello following snippet uses an input file (input.txt) toopass hello jar name and hello class name as parameters.</span></span> <span data-ttu-id="703b3-150">Om du använder de här stegen från en Windows-dator är med hjälp av en indatafil hello rekommendationer.</span><span class="sxs-lookup"><span data-stu-id="703b3-150">If you are running these steps from a Windows computer, using an input file is hello recommended approach.</span></span>
   
        curl -k --user "admin:mypassword1!" -v -H "Content-Type: application/json" -X POST --data @C:\Temp\input.txt "https://mysparkcluster.azurehdinsight.net/livy/batches"
   
    <span data-ttu-id="703b3-151">Hej parametrar i hello filen **input.txt** definieras enligt följande:</span><span class="sxs-lookup"><span data-stu-id="703b3-151">hello parameters in hello file **input.txt** are defined as follows:</span></span>
   
        { "file":"wasb:///example/jars/SparkSimpleApp.jar", "className":"com.microsoft.spark.example.WasbIOTest" }
   
    <span data-ttu-id="703b3-152">Du bör se en utdata liknande toohello följande utdrag:</span><span class="sxs-lookup"><span data-stu-id="703b3-152">You should see an output similar toohello  following snippet:</span></span>
   
        < HTTP/1.1 201 Created
        < Content-Type: application/json; charset=UTF-8
        < Location: /0
        < Server: Microsoft-IIS/8.5
        < X-Powered-By: ARR/2.5
        < X-Powered-By: ASP.NET
        < Date: Fri, 20 Nov 2015 23:51:30 GMT
        < Content-Length: 36
        <
        {"id":0,"state":"starting","log":[]}* Connection #0 toohost mysparkcluster.azurehdinsight.net left intact
   
    <span data-ttu-id="703b3-153">Observera hur hello sista raden i hello resultatet säger **tillstånd: startar**.</span><span class="sxs-lookup"><span data-stu-id="703b3-153">Notice how hello last line of hello output says **state:starting**.</span></span> <span data-ttu-id="703b3-154">Det också står **-id: 0**.</span><span class="sxs-lookup"><span data-stu-id="703b3-154">It also says, **id:0**.</span></span> <span data-ttu-id="703b3-155">Här, **0** är hello batch-ID.</span><span class="sxs-lookup"><span data-stu-id="703b3-155">Here, **0** is hello batch ID.</span></span>

3. <span data-ttu-id="703b3-156">Du kan nu hämta hello status för den här specifika batch med hello batch-ID.</span><span class="sxs-lookup"><span data-stu-id="703b3-156">You can now retrieve hello status of this specific batch using hello batch ID.</span></span>
   
        curl -k --user "admin:mypassword1!" -v -X GET "https://mysparkcluster.azurehdinsight.net/livy/batches/0"
   
    <span data-ttu-id="703b3-157">Du bör se en utdata liknande toohello följande utdrag:</span><span class="sxs-lookup"><span data-stu-id="703b3-157">You should see an output similar toohello following snippet:</span></span>
   
        < HTTP/1.1 200 OK
        < Content-Type: application/json; charset=UTF-8
        < Server: Microsoft-IIS/8.5
        < X-Powered-By: ARR/2.5
        < X-Powered-By: ASP.NET
        < Date: Fri, 20 Nov 2015 23:54:42 GMT
        < Content-Length: 509
        <
        {"id":0,"state":"success","log":["\t diagnostics: N/A","\t ApplicationMaster host: 10.0.0.4","\t ApplicationMaster RPC port: 0","\t queue: default","\t start time: 1448063505350","\t final status: SUCCEEDED","\t tracking URL: http://hn0-myspar.lpel1gnnvxne3gwzqkfq5u5uzh.jx.internal.cloudapp.net:8088/proxy/application_1447984474852_0002/","\t user: root","15/11/20 23:52:47 INFO Utils: Shutdown hook called","15/11/20 23:52:47 INFO Utils: Deleting directory /tmp/spark-b72cd2bf-280b-4c57-8ceb-9e3e69ac7d0c"]}* Connection #0 toohost mysparkcluster.azurehdinsight.net left intact
   
    <span data-ttu-id="703b3-158">hello utdata nu **tillstånd: lyckade**, vilket tyder på hello jobbet har slutförts.</span><span class="sxs-lookup"><span data-stu-id="703b3-158">hello output now shows **state:success**, which suggests that hello job was successfully completed.</span></span>

4. <span data-ttu-id="703b3-159">Du kan nu ta hello batch.</span><span class="sxs-lookup"><span data-stu-id="703b3-159">If you want, you can now delete hello batch.</span></span>
   
        curl -k --user "admin:mypassword1!" -v -X DELETE "https://mysparkcluster.azurehdinsight.net/livy/batches/0"
   
    <span data-ttu-id="703b3-160">Du bör se en utdata liknande toohello följande utdrag:</span><span class="sxs-lookup"><span data-stu-id="703b3-160">You should see an output similar toohello following snippet:</span></span>
   
        < HTTP/1.1 200 OK
        < Content-Type: application/json; charset=UTF-8
        < Server: Microsoft-IIS/8.5
        < X-Powered-By: ARR/2.5
        < X-Powered-By: ASP.NET
        < Date: Sat, 21 Nov 2015 18:51:54 GMT
        < Content-Length: 17
        <
        {"msg":"deleted"}* Connection #0 toohost mysparkcluster.azurehdinsight.net left intact
   
    <span data-ttu-id="703b3-161">hello sista raden i hello utdata visar att hello batch har tagits bort.</span><span class="sxs-lookup"><span data-stu-id="703b3-161">hello last line of hello output shows that hello batch was successfully deleted.</span></span> <span data-ttu-id="703b3-162">Om du tar bort ett jobb när den körs stoppar också hello jobb.</span><span class="sxs-lookup"><span data-stu-id="703b3-162">Deleting a job, while it is running, also kills hello job.</span></span> <span data-ttu-id="703b3-163">Om du tar bort ett jobb som har slutförts utan problem, eller i annat fall tas bort hello jobbinformation helt.</span><span class="sxs-lookup"><span data-stu-id="703b3-163">If you delete a job that has completed, successfully or otherwise, it deletes hello job information completely.</span></span>

## <a name="using-livy-spark-on-hdinsight-35-clusters"></a><span data-ttu-id="703b3-164">Använda Livius Spark i HDInsight 3.5-kluster</span><span class="sxs-lookup"><span data-stu-id="703b3-164">Using Livy Spark on HDInsight 3.5 clusters</span></span>

<span data-ttu-id="703b3-165">3.5 HDInsight-kluster inaktiverar som standard användningen av lokal fil sökvägar tooaccess exempeldatafiler eller burkar.</span><span class="sxs-lookup"><span data-stu-id="703b3-165">HDInsight 3.5 cluster, by default, disables use of local file paths tooaccess sample data files or jars.</span></span> <span data-ttu-id="703b3-166">Vi rekommenderar att du toouse hello `wasb://` sökväg i stället tooaccess burkar och exempeldata filer från hello kluster.</span><span class="sxs-lookup"><span data-stu-id="703b3-166">We encourage you toouse hello `wasb://` path instead tooaccess jars or sample data files from hello cluster.</span></span> <span data-ttu-id="703b3-167">Om du vill toouse lokal sökväg, måste du uppdatera hello Ambari configuration därefter.</span><span class="sxs-lookup"><span data-stu-id="703b3-167">If you do want toouse local path, you must update hello Ambari configuration accordingly.</span></span> <span data-ttu-id="703b3-168">toodo så:</span><span class="sxs-lookup"><span data-stu-id="703b3-168">toodo so:</span></span>

1. <span data-ttu-id="703b3-169">Gå toohello Ambari portal för hello-kluster.</span><span class="sxs-lookup"><span data-stu-id="703b3-169">Go toohello Ambari portal for hello cluster.</span></span> <span data-ttu-id="703b3-170">Hej Ambari-Webbgränssnittet är tillgängligt på ditt HDInsight-kluster på https://**KLUSTERNAMN**. azurehdidnsight.net, där KLUSTERNAMN är hello namnet på klustret.</span><span class="sxs-lookup"><span data-stu-id="703b3-170">hello Ambari Web UI is available on your HDInsight cluster at https://**CLUSTERNAME**.azurehdidnsight.net, where CLUSTERNAME is hello name of your cluster.</span></span>

2. <span data-ttu-id="703b3-171">Hello vänstra navigeringsrutan, klicka på **Livius**, och klicka sedan på **konfigurationerna**.</span><span class="sxs-lookup"><span data-stu-id="703b3-171">From hello left navigation, click **Livy**, and then click **Configs**.</span></span>

3. <span data-ttu-id="703b3-172">Under **Livius standard** lägga till hello egenskapsnamn `livy.file.local-dir-whitelist` och anger dess värde för**”/”** om du vill ha tooallow fullständig åtkomst toofile system.</span><span class="sxs-lookup"><span data-stu-id="703b3-172">Under **livy-default** add hello property name `livy.file.local-dir-whitelist` and set it's value too**"/"** if you want tooallow full access toofile system.</span></span> <span data-ttu-id="703b3-173">Om du vill tooallow åtkomst endast tooa specifik katalog, ange hello sökvägen toothat directory som hello-värde.</span><span class="sxs-lookup"><span data-stu-id="703b3-173">If you want tooallow access only tooa specific directory, provide hello path toothat directory as hello value.</span></span>

## <a name="submitting-livy-jobs-for-a-cluster-within-an-azure-virtual-network"></a><span data-ttu-id="703b3-174">Skickar Livius jobben för ett kluster i Azure-nätverk</span><span class="sxs-lookup"><span data-stu-id="703b3-174">Submitting Livy jobs for a cluster within an Azure virtual network</span></span>

<span data-ttu-id="703b3-175">Om du ansluter tooan HDInsight Spark-kluster från ett Azure Virtual Network, kan du direkt ansluta tooLivy på hello klustret.</span><span class="sxs-lookup"><span data-stu-id="703b3-175">If you connect tooan HDInsight Spark cluster from within an Azure Virtual Network, you can directly connect tooLivy on hello cluster.</span></span> <span data-ttu-id="703b3-176">I sådana fall hello URL för Livius slutpunkten är `http://<IP address of hello headnode>:8998/batches`.</span><span class="sxs-lookup"><span data-stu-id="703b3-176">In such a case, hello URL for Livy endpoint is `http://<IP address of hello headnode>:8998/batches`.</span></span> <span data-ttu-id="703b3-177">Här, **8998** är hello-port som Livius körs på hello klustret headnode.</span><span class="sxs-lookup"><span data-stu-id="703b3-177">Here, **8998** is hello port on which Livy runs on hello cluster headnode.</span></span> <span data-ttu-id="703b3-178">Mer information om att komma åt tjänster på icke-offentliga portar finns [portar som används av Hadoop-tjänster på HDInsight](hdinsight-hadoop-port-settings-for-services.md).</span><span class="sxs-lookup"><span data-stu-id="703b3-178">For more information on accessing services on non-public ports, see [Ports used by Hadoop services on HDInsight](hdinsight-hadoop-port-settings-for-services.md).</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="703b3-179">Felsökning</span><span class="sxs-lookup"><span data-stu-id="703b3-179">Troubleshooting</span></span>

<span data-ttu-id="703b3-180">Här följer några problem som kan uppstå när du använder Livius för fjärranslutna jobbet skicka tooSpark kluster.</span><span class="sxs-lookup"><span data-stu-id="703b3-180">Here are some issues that you might run into while using Livy for remote job submission tooSpark clusters.</span></span>

### <a name="using-an-external-jar-from-hello-additional-storage-is-not-supported"></a><span data-ttu-id="703b3-181">Med hjälp av en extern jar från hello ytterligare lagring stöds inte</span><span class="sxs-lookup"><span data-stu-id="703b3-181">Using an external jar from hello additional storage is not supported</span></span>

<span data-ttu-id="703b3-182">**Problem:** om Livius Spark-jobbet refererar till en extern jar från hello ytterligare lagringskonto som associerats med hello kluster, hello jobb misslyckas.</span><span class="sxs-lookup"><span data-stu-id="703b3-182">**Problem:** If your Livy Spark job references an external jar from hello additional storage account associated with hello cluster, hello job fails.</span></span>

<span data-ttu-id="703b3-183">**Lösning:** se till att hello jar som du vill använda toouse är tillgängliga i hello standardlagring som är associerade med hello HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="703b3-183">**Resolution:** Make sure that hello jar you want toouse is available in hello default storage associated with hello HDInsight cluster.</span></span>





## <a name="next-step"></a><span data-ttu-id="703b3-184">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="703b3-184">Next step</span></span>

* [<span data-ttu-id="703b3-185">Hantera resurser för hello Apache Spark-kluster i Azure HDInsight</span><span class="sxs-lookup"><span data-stu-id="703b3-185">Manage resources for hello Apache Spark cluster in Azure HDInsight</span></span>](hdinsight-apache-spark-resource-manager.md)
* [<span data-ttu-id="703b3-186">Följa och felsöka jobb som körs i ett Apache Spark-kluster i HDInsight</span><span class="sxs-lookup"><span data-stu-id="703b3-186">Track and debug jobs running on an Apache Spark cluster in HDInsight</span></span>](hdinsight-apache-spark-job-debugging.md)

