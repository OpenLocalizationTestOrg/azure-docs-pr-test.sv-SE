---
title: "aaaHive med Data Lake (Hadoop) tools för Visual Studio - Azure HDInsight | Microsoft Docs"
description: "Lär dig hur toouse hello Data Lake-verktyg för Visual Studio toorun Apache Hive-frågor med Apache Hadoop på Azure HDInsight."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 2b3e672a-1195-4fa5-afb7-b7b73937bfbe
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 07/07/2017
ms.author: larryfr
ms.openlocfilehash: dc76974c02cf68bcf701b2b155842c9e9c5cb988
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="run-hive-queries-using-hello-data-lake-tools-for-visual-studio"></a><span data-ttu-id="271eb-103">Köra Hive-frågor med hello Data Lake-verktyg för Visual Studio</span><span class="sxs-lookup"><span data-stu-id="271eb-103">Run Hive queries using hello Data Lake tools for Visual Studio</span></span>

<span data-ttu-id="271eb-104">Lär dig hur toouse hello Data Lake-verktyg för Visual Studio tooquery Apache Hive.</span><span class="sxs-lookup"><span data-stu-id="271eb-104">Learn how toouse hello Data Lake tools for Visual Studio tooquery Apache Hive.</span></span> <span data-ttu-id="271eb-105">hello Data Lake-verktyg kan du tooeasily skapa, skicka och övervaka Hive-frågor tooHadoop på Azure HDInsight.</span><span class="sxs-lookup"><span data-stu-id="271eb-105">hello Data Lake tools allow you tooeasily create, submit, and monitor Hive queries tooHadoop on Azure HDInsight.</span></span>

## <span data-ttu-id="271eb-106"><a id="prereq"></a>Förhandskrav</span><span class="sxs-lookup"><span data-stu-id="271eb-106"><a id="prereq"></a>Prerequisites</span></span>

* <span data-ttu-id="271eb-107">Ett kluster i Azure HDInsight (Hadoop på HDInsight)</span><span class="sxs-lookup"><span data-stu-id="271eb-107">An Azure HDInsight (Hadoop on HDInsight) cluster</span></span>

  > [!IMPORTANT]
  > <span data-ttu-id="271eb-108">Linux är hello endast operativsystem på HDInsight version 3.4 eller senare.</span><span class="sxs-lookup"><span data-stu-id="271eb-108">Linux is hello only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="271eb-109">Mer information finns i [HDInsight-avveckling på Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="271eb-109">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

* <span data-ttu-id="271eb-110">Visual Studio (ett av följande versioner hello):</span><span class="sxs-lookup"><span data-stu-id="271eb-110">Visual Studio (one of hello following versions):</span></span>

    * <span data-ttu-id="271eb-111">Visual Studio Community 2013/Professional/Premium/Ultimate med Update 4</span><span class="sxs-lookup"><span data-stu-id="271eb-111">Visual Studio 2013 Community/Professional/Premium/Ultimate with Update 4</span></span>

    * <span data-ttu-id="271eb-112">Visual Studio 2015 (någon utgåva)</span><span class="sxs-lookup"><span data-stu-id="271eb-112">Visual Studio 2015 (any edition)</span></span>

    * <span data-ttu-id="271eb-113">2017 för Visual Studio (någon utgåva)</span><span class="sxs-lookup"><span data-stu-id="271eb-113">Visual Studio 2017 (any edition)</span></span>

* <span data-ttu-id="271eb-114">HDInsight tools för Visual Studio eller Azure Data Lake-verktyg för Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="271eb-114">HDInsight tools for Visual Studio or Azure Data Lake tools for Visual Studio.</span></span> <span data-ttu-id="271eb-115">Se [komma igång med Visual Studio Hadoop-verktygen för HDInsight](hdinsight-hadoop-visual-studio-tools-get-started.md) för information om hur du installerar och konfigurerar hello verktyg.</span><span class="sxs-lookup"><span data-stu-id="271eb-115">See [Get started using Visual Studio Hadoop tools for HDInsight](hdinsight-hadoop-visual-studio-tools-get-started.md) for information on installing and configuring hello tools.</span></span>

## <span data-ttu-id="271eb-116"><a id="run"></a>Köra Hive-frågor med hello Visual Studio</span><span class="sxs-lookup"><span data-stu-id="271eb-116"><a id="run"></a> Run Hive queries using hello Visual Studio</span></span>

1. <span data-ttu-id="271eb-117">Öppna **Visual Studio** och välj **ny** > **projekt** > **Azure Data Lake** > **HIVE** > **Hive-program**.</span><span class="sxs-lookup"><span data-stu-id="271eb-117">Open **Visual Studio** and select **New** > **Project** > **Azure Data Lake** > **HIVE** > **Hive Application**.</span></span> <span data-ttu-id="271eb-118">Ange ett namn för det här projektet.</span><span class="sxs-lookup"><span data-stu-id="271eb-118">Provide a name for this project.</span></span>

2. <span data-ttu-id="271eb-119">Öppna hello **Script.hql** filen som skapas med det här projektet och klistra in följande HiveQL-instruktioner hello:</span><span class="sxs-lookup"><span data-stu-id="271eb-119">Open hello **Script.hql** file that is created with this project, and paste in hello following HiveQL statements:</span></span>

   ```hiveql
   set hive.execution.engine=tez;
   DROP TABLE log4jLogs;
   CREATE EXTERNAL TABLE log4jLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string)
   ROW FORMAT DELIMITED FIELDS TERMINATED BY ' '
   STORED AS TEXTFILE LOCATION '/example/data/';
   SELECT t4 AS sev, COUNT(*) AS count FROM log4jLogs WHERE t4 = '[ERROR]' AND  INPUT__FILE__NAME LIKE '%.log' GROUP BY t4;
   ```

    <span data-ttu-id="271eb-120">Dessa instruktioner utför hello följande åtgärder:</span><span class="sxs-lookup"><span data-stu-id="271eb-120">These statements perform hello following actions:</span></span>

   * <span data-ttu-id="271eb-121">`DROP TABLE`: Om hello tabellen finns den här instruktionen tar bort den.</span><span class="sxs-lookup"><span data-stu-id="271eb-121">`DROP TABLE`: If hello table exists, this statement deletes it.</span></span>

   * <span data-ttu-id="271eb-122">`CREATE EXTERNAL TABLE`: Skapar en ny ”externa” tabell i Hive.</span><span class="sxs-lookup"><span data-stu-id="271eb-122">`CREATE EXTERNAL TABLE`: Creates a new 'external' table in Hive.</span></span> <span data-ttu-id="271eb-123">Externa tabeller kan du bara lagra hello tabelldefinition i Hive (hello data finns kvar i hello ursprungsplatsen).</span><span class="sxs-lookup"><span data-stu-id="271eb-123">External tables only store hello table definition in Hive (hello data is left in hello original location).</span></span>

     > [!NOTE]
     > <span data-ttu-id="271eb-124">Externa tabeller ska användas när du förväntar dig hello underliggande data toobe uppdateras av en extern källa.</span><span class="sxs-lookup"><span data-stu-id="271eb-124">External tables should be used when you expect hello underlying data toobe updated by an external source.</span></span> <span data-ttu-id="271eb-125">Till exempel en MapReduce-jobb eller Azure-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="271eb-125">For example, a MapReduce job or Azure service.</span></span>
     >
     > <span data-ttu-id="271eb-126">Släppa en extern tabell har **inte** ta bort data hello, bara hello tabelldefinitionen.</span><span class="sxs-lookup"><span data-stu-id="271eb-126">Dropping an external table does **not** delete hello data, only hello table definition.</span></span>

   * <span data-ttu-id="271eb-127">`ROW FORMAT`: Anger hur hello data är formaterad Hive.</span><span class="sxs-lookup"><span data-stu-id="271eb-127">`ROW FORMAT`: Tells Hive how hello data is formatted.</span></span> <span data-ttu-id="271eb-128">I det här fallet avgränsas hello fälten i varje logg med ett blanksteg.</span><span class="sxs-lookup"><span data-stu-id="271eb-128">In this case, hello fields in each log are separated by a space.</span></span>

   * <span data-ttu-id="271eb-129">`STORED AS TEXTFILE LOCATION`: Talar om Hive där hello data lagras (hello exempel/datakatalog) och att den lagras som text.</span><span class="sxs-lookup"><span data-stu-id="271eb-129">`STORED AS TEXTFILE LOCATION`: Tells Hive where hello data is stored (hello example/data directory) and that it is stored as text.</span></span>

   * <span data-ttu-id="271eb-130">`SELECT`: Välj en uppräkning av alla rader där kolumnen `t4` innehåller hello värdet `[ERROR]`.</span><span class="sxs-lookup"><span data-stu-id="271eb-130">`SELECT`: Select a count of all rows where column `t4` contains hello value `[ERROR]`.</span></span> <span data-ttu-id="271eb-131">Den här instruktionen returnerar ett värde för `3` eftersom det finns tre rader som innehåller det här värdet.</span><span class="sxs-lookup"><span data-stu-id="271eb-131">This statement returns a value of `3` because there are three rows that contain this value.</span></span>

   * <span data-ttu-id="271eb-132">`INPUT__FILE__NAME LIKE '%.log'`-Talar om Hive vi bör endast returnera data från filer som slutar på. log.</span><span class="sxs-lookup"><span data-stu-id="271eb-132">`INPUT__FILE__NAME LIKE '%.log'` - Tells Hive that we should only return data from files ending in .log.</span></span> <span data-ttu-id="271eb-133">Den här satsen begränsar hello sökning toohello sample.log-fil som innehåller hello data.</span><span class="sxs-lookup"><span data-stu-id="271eb-133">This clause restricts hello search toohello sample.log file that contains hello data.</span></span>

3. <span data-ttu-id="271eb-134">Välj hello hello verktygsfältet **HDInsight-kluster** som du vill toouse för den här frågan.</span><span class="sxs-lookup"><span data-stu-id="271eb-134">From hello toolbar, select hello **HDInsight Cluster** that you want toouse for this query.</span></span> <span data-ttu-id="271eb-135">Välj **skicka** toorun hello uttrycken som ett Hive-jobb.</span><span class="sxs-lookup"><span data-stu-id="271eb-135">Select **Submit** toorun hello statements as a Hive job.</span></span>

   ![Skicka stapel](./media/hdinsight-hadoop-use-hive-visual-studio/toolbar.png)

4. <span data-ttu-id="271eb-137">Hej **Hive-jobbsammanfattning** visas information om hello köra jobb.</span><span class="sxs-lookup"><span data-stu-id="271eb-137">hello **Hive Job Summary** appears and displays information about hello running job.</span></span> <span data-ttu-id="271eb-138">Använd hello **uppdatera** länka toorefresh hello jobbinformation tills hello **jobbstatus** ändras för**slutförd**.</span><span class="sxs-lookup"><span data-stu-id="271eb-138">Use hello **Refresh** link toorefresh hello job information, until hello **Job Status** changes too**Completed**.</span></span>

   ![jobbsammanfattning visa slutförda jobb](./media/hdinsight-hadoop-use-hive-visual-studio/jobsummary.png)

5. <span data-ttu-id="271eb-140">Använd hello **Jobbutdata** länka tooview hello utdata för jobbet.</span><span class="sxs-lookup"><span data-stu-id="271eb-140">Use hello **Job Output** link tooview hello output of this job.</span></span> <span data-ttu-id="271eb-141">Den visar `[ERROR] 3`, vilket är hello-värdet som returneras av den här frågan.</span><span class="sxs-lookup"><span data-stu-id="271eb-141">It displays `[ERROR] 3`, which is hello value returned by this query.</span></span>

6. <span data-ttu-id="271eb-142">Du kan också köra Hive-frågor utan att skapa ett projekt.</span><span class="sxs-lookup"><span data-stu-id="271eb-142">You can also run Hive queries without creating a project.</span></span> <span data-ttu-id="271eb-143">Med hjälp av **Server Explorer**, expandera **Azure** > **HDInsight**, högerklicka på ditt HDInsight-servern och välj sedan **skriva en Hive-fråga**.</span><span class="sxs-lookup"><span data-stu-id="271eb-143">Using **Server Explorer**, expand **Azure** > **HDInsight**, right-click your HDInsight server, and then select **Write a Hive Query**.</span></span>

7. <span data-ttu-id="271eb-144">I hello **temp.hql** dokument som visas, Lägg till följande HiveQL-instruktioner hello:</span><span class="sxs-lookup"><span data-stu-id="271eb-144">In hello **temp.hql** document that appears, add hello following HiveQL statements:</span></span>

   ```hiveql
   set hive.execution.engine=tez;
   CREATE TABLE IF NOT EXISTS errorLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string) STORED AS ORC;
   INSERT OVERWRITE TABLE errorLogs SELECT t1, t2, t3, t4, t5, t6, t7 FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log';
   ```

    <span data-ttu-id="271eb-145">Dessa instruktioner utför hello följande åtgärder:</span><span class="sxs-lookup"><span data-stu-id="271eb-145">These statements perform hello following actions:</span></span>

   * <span data-ttu-id="271eb-146">`CREATE TABLE IF NOT EXISTS`: Skapar en tabell om den inte redan finns.</span><span class="sxs-lookup"><span data-stu-id="271eb-146">`CREATE TABLE IF NOT EXISTS`: Creates a table if it does not already exist.</span></span> <span data-ttu-id="271eb-147">Eftersom hello `EXTERNAL` nyckelordet används inte, skapar en intern tabell för den här instruktionen.</span><span class="sxs-lookup"><span data-stu-id="271eb-147">Because hello `EXTERNAL` keyword is not used, this statement creates an internal table.</span></span> <span data-ttu-id="271eb-148">Interna register lagras i datalagret för hello Hive och hanteras av Hive.</span><span class="sxs-lookup"><span data-stu-id="271eb-148">Internal tables are stored in hello Hive data warehouse and are managed by Hive.</span></span>

     > [!NOTE]
     > <span data-ttu-id="271eb-149">Till skillnad från `EXTERNAL` tabeller, släppa en intern tabell även tar bort hello underliggande data.</span><span class="sxs-lookup"><span data-stu-id="271eb-149">Unlike `EXTERNAL` tables, dropping an internal table also deletes hello underlying data.</span></span>

   * <span data-ttu-id="271eb-150">`STORED AS ORC`: Lagrar hello data i optimerad raden (ORC) kolumnformat.</span><span class="sxs-lookup"><span data-stu-id="271eb-150">`STORED AS ORC`: Stores hello data in optimized row columnar (ORC) format.</span></span> <span data-ttu-id="271eb-151">ORC är ett mycket optimerad och effektiv format för att lagra data med Hive.</span><span class="sxs-lookup"><span data-stu-id="271eb-151">ORC is a highly optimized and efficient format for storing Hive data.</span></span>

   * <span data-ttu-id="271eb-152">`INSERT OVERWRITE ... SELECT`: Om du väljer rader från hello `log4jLogs` tabellen som innehåller `[ERROR]`, och sedan infogningar hello data i hello `errorLogs` tabell.</span><span class="sxs-lookup"><span data-stu-id="271eb-152">`INSERT OVERWRITE ... SELECT`: Selects rows from hello `log4jLogs` table that contain `[ERROR]`, then inserts hello data into hello `errorLogs` table.</span></span>

8. <span data-ttu-id="271eb-153">Hello-verktygsfältet, Välj **skicka** toorun hello jobb.</span><span class="sxs-lookup"><span data-stu-id="271eb-153">From hello toolbar, select **Submit** toorun hello job.</span></span> <span data-ttu-id="271eb-154">Använd hello **jobbstatus** toodetermine hello jobbet har slutförts.</span><span class="sxs-lookup"><span data-stu-id="271eb-154">Use hello **Job Status** toodetermine that hello job has completed successfully.</span></span>

9. <span data-ttu-id="271eb-155">tooverify som hello jobbet skapat hello tabell, Använd **Server Explorer** och expandera **Azure** > **HDInsight** > ditt HDInsight-kluster > **Hive-databaser** > **standard**.</span><span class="sxs-lookup"><span data-stu-id="271eb-155">tooverify that hello job created hello table, use **Server Explorer** and expand **Azure** > **HDInsight** > your HDInsight cluster > **Hive Databases** > **default**.</span></span> <span data-ttu-id="271eb-156">Hej **errorLogs** tabell och hello **log4jLogs** tabellen visas.</span><span class="sxs-lookup"><span data-stu-id="271eb-156">hello **errorLogs** table and hello **log4jLogs** table are listed.</span></span>

## <span data-ttu-id="271eb-157"><a id="nextsteps"></a>Nästa steg</span><span class="sxs-lookup"><span data-stu-id="271eb-157"><a id="nextsteps"></a>Next steps</span></span>

<span data-ttu-id="271eb-158">Som du ser tillhandahåller hello HDInsight tools för Visual Studio ett enkelt sätt toowork med Hive-frågor på HDInsight.</span><span class="sxs-lookup"><span data-stu-id="271eb-158">As you can see, hello HDInsight tools for Visual Studio provide an easy way toowork with Hive queries on HDInsight.</span></span>

<span data-ttu-id="271eb-159">Allmän information om Hive i HDInsight:</span><span class="sxs-lookup"><span data-stu-id="271eb-159">For general information about Hive in HDInsight:</span></span>

* [<span data-ttu-id="271eb-160">Använda Hive med Hadoop i HDInsight</span><span class="sxs-lookup"><span data-stu-id="271eb-160">Use Hive with Hadoop on HDInsight</span></span>](hdinsight-use-hive.md)

<span data-ttu-id="271eb-161">Information om andra sätt kan du arbeta med Hadoop i HDInsight:</span><span class="sxs-lookup"><span data-stu-id="271eb-161">For information about other ways you can work with Hadoop on HDInsight:</span></span>

* [<span data-ttu-id="271eb-162">Använda Pig med Hadoop i HDInsight</span><span class="sxs-lookup"><span data-stu-id="271eb-162">Use Pig with Hadoop on HDInsight</span></span>](hdinsight-use-pig.md)

* [<span data-ttu-id="271eb-163">Använda MapReduce med Hadoop i HDInsight</span><span class="sxs-lookup"><span data-stu-id="271eb-163">Use MapReduce with Hadoop on HDInsight</span></span>](hdinsight-use-mapreduce.md)

<span data-ttu-id="271eb-164">Mer information om hello HDInsight tools för Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="271eb-164">For more information about hello HDInsight tools for Visual Studio:</span></span>

* [<span data-ttu-id="271eb-165">Komma igång med HDInsight tools för Visual Studio</span><span class="sxs-lookup"><span data-stu-id="271eb-165">Getting started with HDInsight tools for Visual Studio</span></span>](hdinsight-hadoop-visual-studio-tools-get-started.md)

[hdinsight-sdk-documentation]: http://msdnstage.redmond.corp.microsoft.com/library/dn479185.aspx

[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[apache-tez]: http://tez.apache.org
[apache-hive]: http://hive.apache.org/
[apache-log4j]: http://en.wikipedia.org/wiki/Log4j
[hive-on-tez-wiki]: https://cwiki.apache.org/confluence/display/Hive/Hive+on+Tez
[import-to-excel]: http://azure.microsoft.com/documentation/articles/hdinsight-connect-excel-power-query/


[hdinsight-use-oozie]: hdinsight-use-oozie.md
[hdinsight-analyze-flight-data]: hdinsight-analyze-flight-delay-data.md



[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md

[hdinsight-provision]: hdinsight-hadoop-provision-linux-clusters.md
[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md

[powershell-here-strings]: http://technet.microsoft.com/library/ee692792.aspx

[image-hdi-hive-powershell]: ./media/hdinsight-use-hive/HDI.HIVE.PowerShell.png
[img-hdi-hive-powershell-output]: ./media/hdinsight-use-hive/HDI.Hive.PowerShell.Output.png
[image-hdi-hive-architecture]: ./media/hdinsight-use-hive/HDI.Hive.Architecture.png
