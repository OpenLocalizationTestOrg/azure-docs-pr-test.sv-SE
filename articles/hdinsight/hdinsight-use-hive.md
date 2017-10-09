---
title: "aaaWhat är Apache Hive och HiveQL - Azure HDInsight | Microsoft Docs"
description: "Apache Hive är ett data warehouse system för Hadoop. Du kan fråga data som lagras i Hive med HiveQL, liknande tooTransact-SQL. I detta dokument, lär du dig hur toouse Hive och HiveQL med Azure HDInsight."
keywords: "hiveql, vad är hive, hadoop hiveql hur toouse hive Läs hive, vad är hive"
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 2c10f989-7636-41bf-b7f7-c4b67ec0814f
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 08/03/2017
ms.author: larryfr
ms.openlocfilehash: a2772312263895ff99b499898264c2e6d5e816e9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="what-is-apache-hive-and-hiveql-on-azure-hdinsight"></a><span data-ttu-id="17025-106">Vad är Apache Hive och HiveQL på Azure HDInsight?</span><span class="sxs-lookup"><span data-stu-id="17025-106">What is Apache Hive and HiveQL on Azure HDInsight?</span></span>

<span data-ttu-id="17025-107">[Apache Hive](http://hive.apache.org/) är ett data warehouse system för Hadoop.</span><span class="sxs-lookup"><span data-stu-id="17025-107">[Apache Hive](http://hive.apache.org/) is a data warehouse system for Hadoop.</span></span> <span data-ttu-id="17025-108">Hive kan sammanfatta data, frågor och analys av data.</span><span class="sxs-lookup"><span data-stu-id="17025-108">Hive enables data summarization, querying, and analysis of data.</span></span> <span data-ttu-id="17025-109">Hive-frågor är skrivna i HiveQL, vilket är en liknande tooSQL språk för frågan.</span><span class="sxs-lookup"><span data-stu-id="17025-109">Hive queries are written in HiveQL, which is a query language similar tooSQL.</span></span>

<span data-ttu-id="17025-110">Hive tillåter tooproject strukturen på i stort sett Ostrukturerade data.</span><span class="sxs-lookup"><span data-stu-id="17025-110">Hive allows you tooproject structure on largely unstructured data.</span></span> <span data-ttu-id="17025-111">När du har definierat strukturen hello kan du använda HiveQL tooquery hello data utan kännedom om Java eller MapReduce.</span><span class="sxs-lookup"><span data-stu-id="17025-111">After you define hello structure, you can use HiveQL tooquery hello data without knowledge of Java or MapReduce.</span></span>

<span data-ttu-id="17025-112">HDInsight tillhandahåller flera klustertyper som är anpassade för specifika arbetsbelastningar.</span><span class="sxs-lookup"><span data-stu-id="17025-112">HDInsight provides several cluster types, which are tuned for specific workloads.</span></span> <span data-ttu-id="17025-113">följande klustertyper hello används oftast för Hive-frågor:</span><span class="sxs-lookup"><span data-stu-id="17025-113">hello following cluster types are most often used for Hive queries:</span></span>

* <span data-ttu-id="17025-114">__Interaktiva Hive__: A Hadoop-kluster som innehåller [låg latens analytisk behandling (LLAP)](https://cwiki.apache.org/confluence/display/Hive/LLAP) funktioner tooimprove svarstider för interaktiva frågor.</span><span class="sxs-lookup"><span data-stu-id="17025-114">__Interactive Hive__: A Hadoop cluster that provides [Low Latency Analytical Processing (LLAP)](https://cwiki.apache.org/confluence/display/Hive/LLAP) functionality tooimprove response times for interactive queries.</span></span> <span data-ttu-id="17025-115">Mer information finns i hello [börja med interaktiva Hive i HDInsight](hdinsight-hadoop-use-interactive-hive.md) dokumentet.</span><span class="sxs-lookup"><span data-stu-id="17025-115">For more information, see hello [Start with Interactive Hive in HDInsight](hdinsight-hadoop-use-interactive-hive.md) document.</span></span>

* <span data-ttu-id="17025-116">__Hadoop__: A Hadoop-kluster som är anpassad för batchbearbetning arbetsbelastningar.</span><span class="sxs-lookup"><span data-stu-id="17025-116">__Hadoop__: A Hadoop cluster that is tuned for batch processing workloads.</span></span> <span data-ttu-id="17025-117">Mer information finns i hello [starta med Hadoop i HDInsight](hdinsight-hadoop-linux-tutorial-get-started.md) dokumentet.</span><span class="sxs-lookup"><span data-stu-id="17025-117">For more information, see hello [Start with Hadoop in HDInsight](hdinsight-hadoop-linux-tutorial-get-started.md) document.</span></span>

* <span data-ttu-id="17025-118">__Spark__: Apache Spark har inbyggda funktioner för att arbeta med Hive.</span><span class="sxs-lookup"><span data-stu-id="17025-118">__Spark__: Apache Spark has built-in functionality for working with Hive.</span></span> <span data-ttu-id="17025-119">Mer information finns i hello [börja med Spark i HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md) dokumentet.</span><span class="sxs-lookup"><span data-stu-id="17025-119">For more information, see hello [Start with Spark on HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md) document.</span></span>

* <span data-ttu-id="17025-120">__HBase__: HiveQL kan vara används tooquery data som lagras i HBase.</span><span class="sxs-lookup"><span data-stu-id="17025-120">__HBase__: HiveQL can be used tooquery data stored in HBase.</span></span> <span data-ttu-id="17025-121">Mer information finns i hello [börjar med HBase på HDInsight](hdinsight-hbase-tutorial-get-started-linux.md) dokumentet.</span><span class="sxs-lookup"><span data-stu-id="17025-121">For more information, see hello [Start with HBase on HDInsight](hdinsight-hbase-tutorial-get-started-linux.md) document.</span></span>

## <a name="how-toouse-hive"></a><span data-ttu-id="17025-122">Hur toouse Hive</span><span class="sxs-lookup"><span data-stu-id="17025-122">How toouse Hive</span></span>

<span data-ttu-id="17025-123">Använd följande tabell toodiscover hur toouse Hive med HDInsight hello:</span><span class="sxs-lookup"><span data-stu-id="17025-123">Use hello following table toodiscover how toouse Hive with HDInsight:</span></span>

| <span data-ttu-id="17025-124">**Använd den här metoden** om du vill...</span><span class="sxs-lookup"><span data-stu-id="17025-124">**Use this method** if you want...</span></span> | <span data-ttu-id="17025-125">.. .an **interaktiva** shell</span><span class="sxs-lookup"><span data-stu-id="17025-125">...an **interactive** shell</span></span> | <span data-ttu-id="17025-126">... **batch** bearbetning</span><span class="sxs-lookup"><span data-stu-id="17025-126">...**batch** processing</span></span> | <span data-ttu-id="17025-127">... med detta **klustret operativsystem**</span><span class="sxs-lookup"><span data-stu-id="17025-127">...with this **cluster operating system**</span></span> | <span data-ttu-id="17025-128">.. .from detta **klientoperativsystem**</span><span class="sxs-lookup"><span data-stu-id="17025-128">...from this **client operating system**</span></span> |
|:--- |:---:|:---:|:--- |:--- |
| [<span data-ttu-id="17025-129">Hive-vyn</span><span class="sxs-lookup"><span data-stu-id="17025-129">Hive View</span></span>](hdinsight-hadoop-use-hive-ambari-view.md) |<span data-ttu-id="17025-130">✔</span><span class="sxs-lookup"><span data-stu-id="17025-130">✔</span></span> |<span data-ttu-id="17025-131">✔</span><span class="sxs-lookup"><span data-stu-id="17025-131">✔</span></span> |<span data-ttu-id="17025-132">Linux</span><span class="sxs-lookup"><span data-stu-id="17025-132">Linux</span></span> |<span data-ttu-id="17025-133">Alla (webbläsarbaserade)</span><span class="sxs-lookup"><span data-stu-id="17025-133">Any (browser based)</span></span> |
| [<span data-ttu-id="17025-134">Beeline klienten</span><span class="sxs-lookup"><span data-stu-id="17025-134">Beeline client</span></span>](hdinsight-hadoop-use-hive-beeline.md) |<span data-ttu-id="17025-135">✔</span><span class="sxs-lookup"><span data-stu-id="17025-135">✔</span></span> |<span data-ttu-id="17025-136">✔</span><span class="sxs-lookup"><span data-stu-id="17025-136">✔</span></span> |<span data-ttu-id="17025-137">Linux</span><span class="sxs-lookup"><span data-stu-id="17025-137">Linux</span></span> |<span data-ttu-id="17025-138">Linux, Unix, Mac OS X eller Windows</span><span class="sxs-lookup"><span data-stu-id="17025-138">Linux, Unix, Mac OS X, or Windows</span></span> |
| [<span data-ttu-id="17025-139">REST-API</span><span class="sxs-lookup"><span data-stu-id="17025-139">REST API</span></span>](hdinsight-hadoop-use-hive-curl.md) |&nbsp; |<span data-ttu-id="17025-140">✔</span><span class="sxs-lookup"><span data-stu-id="17025-140">✔</span></span> |<span data-ttu-id="17025-141">Linux- eller Windows *</span><span class="sxs-lookup"><span data-stu-id="17025-141">Linux or Windows*</span></span> |<span data-ttu-id="17025-142">Linux, Unix, Mac OS X eller Windows</span><span class="sxs-lookup"><span data-stu-id="17025-142">Linux, Unix, Mac OS X, or Windows</span></span> |
| [<span data-ttu-id="17025-143">HDInsight tools för Visual Studio</span><span class="sxs-lookup"><span data-stu-id="17025-143">HDInsight tools for Visual Studio</span></span>](hdinsight-hadoop-use-hive-visual-studio.md) |&nbsp; |<span data-ttu-id="17025-144">✔</span><span class="sxs-lookup"><span data-stu-id="17025-144">✔</span></span> |<span data-ttu-id="17025-145">Linux- eller Windows *</span><span class="sxs-lookup"><span data-stu-id="17025-145">Linux or Windows*</span></span> |<span data-ttu-id="17025-146">Windows</span><span class="sxs-lookup"><span data-stu-id="17025-146">Windows</span></span> |
| [<span data-ttu-id="17025-147">Windows PowerShell</span><span class="sxs-lookup"><span data-stu-id="17025-147">Windows PowerShell</span></span>](hdinsight-hadoop-use-hive-powershell.md) |&nbsp; |<span data-ttu-id="17025-148">✔</span><span class="sxs-lookup"><span data-stu-id="17025-148">✔</span></span> |<span data-ttu-id="17025-149">Linux- eller Windows *</span><span class="sxs-lookup"><span data-stu-id="17025-149">Linux or Windows*</span></span> |<span data-ttu-id="17025-150">Windows</span><span class="sxs-lookup"><span data-stu-id="17025-150">Windows</span></span> |

> [!IMPORTANT]
> <span data-ttu-id="17025-151">\*Linux är hello endast operativsystem på HDInsight version 3.4 eller senare.</span><span class="sxs-lookup"><span data-stu-id="17025-151">\* Linux is hello only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="17025-152">Mer information finns i [HDInsight-avveckling på Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="17025-152">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>
>
> <span data-ttu-id="17025-153">Om du använder ett Windows-baserade HDInsight-kluster kan du använda hello [frågan konsolen](hdinsight-hadoop-use-hive-query-console.md) från din webbläsare eller [fjärrskrivbord](hdinsight-hadoop-use-hive-remote-desktop.md) toorun Hive-frågor.</span><span class="sxs-lookup"><span data-stu-id="17025-153">If you are using a Windows-based HDInsight cluster, you can use hello [Query console](hdinsight-hadoop-use-hive-query-console.md) from your browser or [Remote Desktop](hdinsight-hadoop-use-hive-remote-desktop.md) toorun Hive queries.</span></span>

## <a name="hiveql-language-reference"></a><span data-ttu-id="17025-154">Språkreferens för HiveQL</span><span class="sxs-lookup"><span data-stu-id="17025-154">HiveQL language reference</span></span>

<span data-ttu-id="17025-155">Språkreferens för HiveQL är tillgängliga i hello [språk manuell (https://cwiki.apache.org/confluence/display/Hive/LanguageManual)](https://cwiki.apache.org/confluence/display/Hive/LanguageManual).</span><span class="sxs-lookup"><span data-stu-id="17025-155">HiveQL language reference is available in hello [language manual (https://cwiki.apache.org/confluence/display/Hive/LanguageManual)](https://cwiki.apache.org/confluence/display/Hive/LanguageManual).</span></span>

## <a name="hive-and-data-structure"></a><span data-ttu-id="17025-156">Hive och data struktur</span><span class="sxs-lookup"><span data-stu-id="17025-156">Hive and data structure</span></span>

<span data-ttu-id="17025-157">Hive förstår hur toowork med strukturerade och halvstrukturerade data.</span><span class="sxs-lookup"><span data-stu-id="17025-157">Hive understands how toowork with structured and semi-structured data.</span></span> <span data-ttu-id="17025-158">Till exempel textfiler där hello fälten avgränsas med tecken.</span><span class="sxs-lookup"><span data-stu-id="17025-158">For example, text files where hello fields are delimited by specific characters.</span></span> <span data-ttu-id="17025-159">hello följande HiveQL-uttrycket skapar en tabell över blankstegsavgränsad data:</span><span class="sxs-lookup"><span data-stu-id="17025-159">hello following HiveQL statement creates a table over space-delimited data:</span></span>

```hiveql
CREATE EXTERNAL TABLE log4jLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string)
ROW FORMAT DELIMITED FIELDS TERMINATED BY ' '
STORED AS TEXTFILE LOCATION '/example/data/';
```

<span data-ttu-id="17025-160">Hive även stöder anpassad **serialiseraren/deserializers (SerDe)** för komplex eller oregelbundet strukturerade data.</span><span class="sxs-lookup"><span data-stu-id="17025-160">Hive also supports custom **serializer/deserializers (SerDe)** for complex or irregularly structured data.</span></span> <span data-ttu-id="17025-161">Mer information finns i hello [hur toouse en anpassad JSON-SerDe med HDInsight](http://blogs.msdn.com/b/bigdatasupport/archive/2014/06/18/how-to-use-a-custom-json-serde-with-microsoft-azure-hdinsight.aspx) dokumentet.</span><span class="sxs-lookup"><span data-stu-id="17025-161">For more information, see hello [How toouse a custom JSON SerDe with HDInsight](http://blogs.msdn.com/b/bigdatasupport/archive/2014/06/18/how-to-use-a-custom-json-serde-with-microsoft-azure-hdinsight.aspx) document.</span></span>

<span data-ttu-id="17025-162">Mer information om filformat som stöds av Hive finns hello [Language manual (https://cwiki.apache.org/confluence/display/Hive/LanguageManual)](https://cwiki.apache.org/confluence/display/Hive/LanguageManual)</span><span class="sxs-lookup"><span data-stu-id="17025-162">For more information on file formats supported by Hive, see hello [Language manual (https://cwiki.apache.org/confluence/display/Hive/LanguageManual)](https://cwiki.apache.org/confluence/display/Hive/LanguageManual)</span></span>

## <a name="hive-internal-tables-vs-external-tables"></a><span data-ttu-id="17025-163">Hive interna register vs externa tabeller</span><span class="sxs-lookup"><span data-stu-id="17025-163">Hive internal tables vs external tables</span></span>

<span data-ttu-id="17025-164">Det finns två typer av tabeller som du kan skapa med Hive:</span><span class="sxs-lookup"><span data-stu-id="17025-164">There are two types of tables that you can create with Hive:</span></span>

* <span data-ttu-id="17025-165">__Internt__: Data lagras i datalagret för hello Hive.</span><span class="sxs-lookup"><span data-stu-id="17025-165">__Internal__: Data is stored in hello Hive data warehouse.</span></span> <span data-ttu-id="17025-166">hello datalagret finns i `/hive/warehouse/` på hello standardlagring för hello-kluster.</span><span class="sxs-lookup"><span data-stu-id="17025-166">hello data warehouse is located at `/hive/warehouse/` on hello default storage for hello cluster.</span></span>

    <span data-ttu-id="17025-167">Använder interna tabeller när:</span><span class="sxs-lookup"><span data-stu-id="17025-167">Use internal tables when:</span></span>

    * <span data-ttu-id="17025-168">Data är tillfällig.</span><span class="sxs-lookup"><span data-stu-id="17025-168">Data is temporary.</span></span>
    * <span data-ttu-id="17025-169">Vill du Hive toomanage hello livscykeln för hello tabellen och data.</span><span class="sxs-lookup"><span data-stu-id="17025-169">You want Hive toomanage hello lifecycle of hello table and data.</span></span>

* <span data-ttu-id="17025-170">__Externa__: Data lagras utanför hello-datalagret.</span><span class="sxs-lookup"><span data-stu-id="17025-170">__External__: Data is stored outside hello data warehouse.</span></span> <span data-ttu-id="17025-171">hello data kan lagras på all tillgänglig lagring av hello-kluster.</span><span class="sxs-lookup"><span data-stu-id="17025-171">hello data can be stored on any storage accessible by hello cluster.</span></span>

    <span data-ttu-id="17025-172">Använd externa tabeller när:</span><span class="sxs-lookup"><span data-stu-id="17025-172">Use external tables when:</span></span>

    * <span data-ttu-id="17025-173">hello data används också utanför Hive.</span><span class="sxs-lookup"><span data-stu-id="17025-173">hello data is also used outside of Hive.</span></span> <span data-ttu-id="17025-174">Till exempel har hello datafiler uppdaterats av en annan process (som inte låser hello filer.)</span><span class="sxs-lookup"><span data-stu-id="17025-174">For example, hello data files are updated by another process (that does not lock hello files.)</span></span>
    * <span data-ttu-id="17025-175">Data måste tooremain i hello underliggande plats, även efter att släppa hello tabell.</span><span class="sxs-lookup"><span data-stu-id="17025-175">Data needs tooremain in hello underlying location, even after dropping hello table.</span></span>
    * <span data-ttu-id="17025-176">Du behöver en anpassad plats, till exempel ett lagringskonto som inte är standard.</span><span class="sxs-lookup"><span data-stu-id="17025-176">You need a custom location, such as a non-default storage account.</span></span>
    * <span data-ttu-id="17025-177">Ett annat program än hive hanterar hello dataformat, platsen, osv.</span><span class="sxs-lookup"><span data-stu-id="17025-177">A program other than hive manages hello data format, location, etc.</span></span>

<span data-ttu-id="17025-178">Mer information finns i hello [Hive interna och externa tabeller introduktion] [ cindygross-hive-tables] blogginlägg.</span><span class="sxs-lookup"><span data-stu-id="17025-178">For more information, see hello [Hive Internal and External Tables Intro][cindygross-hive-tables] blog post.</span></span>

## <a name="user-defined-functions-udf"></a><span data-ttu-id="17025-179">Användardefinierade funktioner (UDF)</span><span class="sxs-lookup"><span data-stu-id="17025-179">User-defined functions (UDF)</span></span>

<span data-ttu-id="17025-180">Hive kan också utökas genom **användardefinierade funktioner (UDF)**.</span><span class="sxs-lookup"><span data-stu-id="17025-180">Hive can also be extended through **user-defined functions (UDF)**.</span></span> <span data-ttu-id="17025-181">En UDF kan du tooimplement funktioner eller som inte är enkelt modelleras i HiveQL.</span><span class="sxs-lookup"><span data-stu-id="17025-181">A UDF allows you tooimplement functionality or logic that isn't easily modeled in HiveQL.</span></span> <span data-ttu-id="17025-182">Ett exempel på hur UDF: er med Hive finns hello följande dokument:</span><span class="sxs-lookup"><span data-stu-id="17025-182">For an example of using UDFs with Hive, see hello following documents:</span></span>

* [<span data-ttu-id="17025-183">Använda en användardefinierad funktion i Java med Hive</span><span class="sxs-lookup"><span data-stu-id="17025-183">Use a Java user-defined function with Hive</span></span>](hdinsight-hadoop-hive-java-udf.md)

* [<span data-ttu-id="17025-184">Använda Python användardefinierad funktion med Hive och Pig</span><span class="sxs-lookup"><span data-stu-id="17025-184">Use a Python user-defined function with Hive and Pig</span></span>](hdinsight-python.md)

* [<span data-ttu-id="17025-185">Använda en C# användardefinierad funktion med Hive och Pig</span><span class="sxs-lookup"><span data-stu-id="17025-185">Use a C# user-defined function with Hive and Pig</span></span>](hdinsight-hadoop-hive-pig-udf-dotnet-csharp.md)

* [<span data-ttu-id="17025-186">Hur tooadd anpassade registreringsdata användardefinierade funktionen tooHDInsight</span><span class="sxs-lookup"><span data-stu-id="17025-186">How tooadd a custom Hive user-defined function tooHDInsight</span></span>](http://blogs.msdn.com/b/bigdatasupport/archive/2014/01/14/how-to-add-custom-hive-udfs-to-hdinsight.aspx)

* [<span data-ttu-id="17025-187">Ett exempel Hive användardefinierad funktion tooconvert datum/tid-format tooHive tidsstämpel</span><span class="sxs-lookup"><span data-stu-id="17025-187">An example Hive user-defined function tooconvert date/time formats tooHive timestamp</span></span>](https://github.com/Azure-Samples/hdinsight-java-hive-udf)

## <span data-ttu-id="17025-188"><a id="data"></a>Exempeldata</span><span class="sxs-lookup"><span data-stu-id="17025-188"><a id="data"></a>Example data</span></span>

<span data-ttu-id="17025-189">Hive i HDInsight levereras förinstallerade med en intern tabell med namnet `hivesampletable`.</span><span class="sxs-lookup"><span data-stu-id="17025-189">Hive on HDInsight comes pre-loaded with an internal table named `hivesampletable`.</span></span> <span data-ttu-id="17025-190">HDInsight tillhandahåller även exempel datamängder som kan användas med Hive.</span><span class="sxs-lookup"><span data-stu-id="17025-190">HDInsight also provides example data sets that can be used with Hive.</span></span> <span data-ttu-id="17025-191">Dessa uppgifter som lagras i hello `/example/data` och `/HdiSamples` kataloger.</span><span class="sxs-lookup"><span data-stu-id="17025-191">These data sets are stored in hello `/example/data` and `/HdiSamples` directories.</span></span> <span data-ttu-id="17025-192">Dessa kataloger finns i hello standardlagring för klustret.</span><span class="sxs-lookup"><span data-stu-id="17025-192">These directories exist in hello default storage for your cluster.</span></span>

## <span data-ttu-id="17025-193"><a id="job"></a>Exempel Hive-fråga</span><span class="sxs-lookup"><span data-stu-id="17025-193"><a id="job"></a>Example Hive query</span></span>

<span data-ttu-id="17025-194">Hej följande HiveQL-instruktioner projektkolumner till hello `/example/data/sample.log` fil:</span><span class="sxs-lookup"><span data-stu-id="17025-194">hello following HiveQL statements project columns onto hello `/example/data/sample.log` file:</span></span>

    set hive.execution.engine=tez;
    DROP TABLE log4jLogs;
    CREATE EXTERNAL TABLE log4jLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string)
    ROW FORMAT DELIMITED FIELDS TERMINATED BY ' '
    STORED AS TEXTFILE LOCATION '/example/data/';
    SELECT t4 AS sev, COUNT(*) AS count FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log' GROUP BY t4;

<span data-ttu-id="17025-195">I föregående exempel hello utföra hello HiveQL-instruktioner hello följande åtgärder:</span><span class="sxs-lookup"><span data-stu-id="17025-195">In hello previous example, hello HiveQL statements perform hello following actions:</span></span>

* <span data-ttu-id="17025-196">`set hive.execution.engine=tez;`: Uppsättningarna hello körning av motorn toouse Tez.</span><span class="sxs-lookup"><span data-stu-id="17025-196">`set hive.execution.engine=tez;`: Sets hello execution engine toouse Tez.</span></span> <span data-ttu-id="17025-197">Tez ger i stället för MapReduce en ökning i frågeprestanda.</span><span class="sxs-lookup"><span data-stu-id="17025-197">Using Tez instead of MapReduce can provide an increase in query performance.</span></span> <span data-ttu-id="17025-198">Mer information om Tez finns hello [använda Apache Tez för bättre prestanda](#usetez) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="17025-198">For more information on Tez, see hello [Use Apache Tez for improved performance](#usetez) section.</span></span>

    > [!NOTE]
    > <span data-ttu-id="17025-199">Den här instruktionen är bara krävs när du använder ett Windows-baserade HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="17025-199">This statement is only required when using a Windows-based HDInsight cluster.</span></span> <span data-ttu-id="17025-200">Tez är motorn för körning av hello standard för Linux-baserade HDInsight.</span><span class="sxs-lookup"><span data-stu-id="17025-200">Tez is hello default execution engine for Linux-based HDInsight.</span></span>

* <span data-ttu-id="17025-201">`DROP TABLE`: Ta bort om hello tabellen redan finns.</span><span class="sxs-lookup"><span data-stu-id="17025-201">`DROP TABLE`: If hello table already exists, delete it.</span></span>

* <span data-ttu-id="17025-202">`CREATE EXTERNAL TABLE`: Skapar en ny **externa** tabellen i Hive.</span><span class="sxs-lookup"><span data-stu-id="17025-202">`CREATE EXTERNAL TABLE`: Creates a new **external** table in Hive.</span></span> <span data-ttu-id="17025-203">Externa tabeller kan du bara lagra hello tabelldefinition i Hive.</span><span class="sxs-lookup"><span data-stu-id="17025-203">External tables only store hello table definition in Hive.</span></span> <span data-ttu-id="17025-204">hello data finns kvar i hello ursprungliga plats och hello ursprungliga format.</span><span class="sxs-lookup"><span data-stu-id="17025-204">hello data is left in hello original location and in hello original format.</span></span>

* <span data-ttu-id="17025-205">`ROW FORMAT`: Anger hur hello data är formaterad Hive.</span><span class="sxs-lookup"><span data-stu-id="17025-205">`ROW FORMAT`: Tells Hive how hello data is formatted.</span></span> <span data-ttu-id="17025-206">I det här fallet avgränsas hello fälten i varje logg med ett blanksteg.</span><span class="sxs-lookup"><span data-stu-id="17025-206">In this case, hello fields in each log are separated by a space.</span></span>

* <span data-ttu-id="17025-207">`STORED AS TEXTFILE LOCATION`: Talar om Hive där hello data lagras (hello `example/data` directory) och att den lagras som text.</span><span class="sxs-lookup"><span data-stu-id="17025-207">`STORED AS TEXTFILE LOCATION`: Tells Hive where hello data is stored (hello `example/data` directory) and that it is stored as text.</span></span> <span data-ttu-id="17025-208">hello data kan vara i en fil eller sprida över flera filer i hello katalog.</span><span class="sxs-lookup"><span data-stu-id="17025-208">hello data can be in one file or spread across multiple files within hello directory.</span></span>

* <span data-ttu-id="17025-209">`SELECT`: Väljer en uppräkning av alla rader där hello kolumnen **t4** innehåller hello värdet **[fel]**.</span><span class="sxs-lookup"><span data-stu-id="17025-209">`SELECT`: Selects a count of all rows where hello column **t4** contains hello value **[ERROR]**.</span></span> <span data-ttu-id="17025-210">Den här instruktionen returnerar ett värde för **3** eftersom det finns tre rader som innehåller det här värdet.</span><span class="sxs-lookup"><span data-stu-id="17025-210">This statement returns a value of **3** because there are three rows that contain this value.</span></span>

* <span data-ttu-id="17025-211">`INPUT__FILE__NAME LIKE '%.log'`-Hive försöker tooapply hello schemafiler tooall i hello directory.</span><span class="sxs-lookup"><span data-stu-id="17025-211">`INPUT__FILE__NAME LIKE '%.log'` - Hive attempts tooapply hello schema tooall files in hello directory.</span></span> <span data-ttu-id="17025-212">I det här fallet innehåller hello katalogen filer som inte matchar hello schemat.</span><span class="sxs-lookup"><span data-stu-id="17025-212">In this case, hello directory contains files that do not match hello schema.</span></span> <span data-ttu-id="17025-213">tooprevent ogiltiga data i hello resultat instruktionen anger Hive vi bör endast returnera data från filer som slutar på. log.</span><span class="sxs-lookup"><span data-stu-id="17025-213">tooprevent garbage data in hello results, this statement tells Hive that we should only return data from files ending in .log.</span></span>

> [!NOTE]
> <span data-ttu-id="17025-214">Externa tabeller ska användas när du förväntar dig hello underliggande data toobe uppdateras av en extern källa.</span><span class="sxs-lookup"><span data-stu-id="17025-214">External tables should be used when you expect hello underlying data toobe updated by an external source.</span></span> <span data-ttu-id="17025-215">Till exempel en automatisk överföring av data eller MapReduce-åtgärden.</span><span class="sxs-lookup"><span data-stu-id="17025-215">For example, an automated data upload process, or MapReduce operation.</span></span>
>
> <span data-ttu-id="17025-216">Släppa en extern tabell har **inte** bort hello data tar endast bort hello tabelldefinitionen.</span><span class="sxs-lookup"><span data-stu-id="17025-216">Dropping an external table does **not** delete hello data, it only deletes hello table definition.</span></span>

<span data-ttu-id="17025-217">toocreate en **interna** tabellen i stället för externa använder hello följande HiveQL:</span><span class="sxs-lookup"><span data-stu-id="17025-217">toocreate an **internal** table instead of external, use hello following HiveQL:</span></span>

    set hive.execution.engine=tez;
    CREATE TABLE IF NOT EXISTS errorLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string)
    STORED AS ORC;
    INSERT OVERWRITE TABLE errorLogs
    SELECT t1, t2, t3, t4, t5, t6, t7 FROM log4jLogs WHERE t4 = '[ERROR]';

<span data-ttu-id="17025-218">Dessa instruktioner utför hello följande åtgärder:</span><span class="sxs-lookup"><span data-stu-id="17025-218">These statements perform hello following actions:</span></span>

* <span data-ttu-id="17025-219">`CREATE TABLE IF NOT EXISTS`: Skapa den om hello tabellen inte finns.</span><span class="sxs-lookup"><span data-stu-id="17025-219">`CREATE TABLE IF NOT EXISTS`: If hello table does not exist, create it.</span></span> <span data-ttu-id="17025-220">Eftersom hello **externa** nyckelordet används inte, skapar en intern tabell för den här instruktionen.</span><span class="sxs-lookup"><span data-stu-id="17025-220">Because hello **EXTERNAL** keyword is not used, this statement creates an internal table.</span></span> <span data-ttu-id="17025-221">hello tabell lagras i datalagret för hello Hive och hanteras helt av Hive.</span><span class="sxs-lookup"><span data-stu-id="17025-221">hello table is stored in hello Hive data warehouse and is managed completely by Hive.</span></span>

* <span data-ttu-id="17025-222">`STORED AS ORC`: Lagrar hello data i optimerad raden kolumner (ORC)-format.</span><span class="sxs-lookup"><span data-stu-id="17025-222">`STORED AS ORC`: Stores hello data in Optimized Row Columnar (ORC) format.</span></span> <span data-ttu-id="17025-223">ORC är ett mycket optimerad och effektiv format för att lagra data med Hive.</span><span class="sxs-lookup"><span data-stu-id="17025-223">ORC is a highly optimized and efficient format for storing Hive data.</span></span>

* <span data-ttu-id="17025-224">`INSERT OVERWRITE ... SELECT`: Om du väljer rader från hello **log4jLogs** tabell som innehåller **[fel]**, och sedan infogningar hello data i hello **errorLogs** tabell.</span><span class="sxs-lookup"><span data-stu-id="17025-224">`INSERT OVERWRITE ... SELECT`: Selects rows from hello **log4jLogs** table that contains **[ERROR]**, and then inserts hello data into hello **errorLogs** table.</span></span>

> [!NOTE]
> <span data-ttu-id="17025-225">Till skillnad från externa tabeller kan släppa en intern tabell tas även bort hello underliggande data.</span><span class="sxs-lookup"><span data-stu-id="17025-225">Unlike external tables, dropping an internal table also deletes hello underlying data.</span></span>

## <a name="improve-hive-query-performance"></a><span data-ttu-id="17025-226">Förbättra prestanda för Hive-frågor</span><span class="sxs-lookup"><span data-stu-id="17025-226">Improve Hive query performance</span></span>

### <span data-ttu-id="17025-227"><a id="usetez"></a>Apache Tez</span><span class="sxs-lookup"><span data-stu-id="17025-227"><a id="usetez"></a>Apache Tez</span></span>

<span data-ttu-id="17025-228">[Apache Tez](http://tez.apache.org) är ett ramverk som gör att dataintensiva applikationer, som Hive, toorun mer effektivt i större skala.</span><span class="sxs-lookup"><span data-stu-id="17025-228">[Apache Tez](http://tez.apache.org) is a framework that allows data intensive applications, such as Hive, toorun much more efficiently at scale.</span></span> <span data-ttu-id="17025-229">Tez är aktiverad som standard för Linux-baserade HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="17025-229">Tez is enabled by default for Linux-based HDInsight clusters.</span></span>

> [!NOTE]
> <span data-ttu-id="17025-230">Tez är inaktiverat som standard för Windows-baserade HDInsight-kluster och den måste aktiveras.</span><span class="sxs-lookup"><span data-stu-id="17025-230">Tez is currently off by default for Windows-based HDInsight clusters and it must be enabled.</span></span> <span data-ttu-id="17025-231">tootake nytta av Tez hello följande värde måste anges för en Hive-fråga:</span><span class="sxs-lookup"><span data-stu-id="17025-231">tootake advantage of Tez, hello following value must be set for a Hive query:</span></span>
>
> `set hive.execution.engine=tez;`
>
> <span data-ttu-id="17025-232">Tez är hello standard motor för Linux-baserade HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="17025-232">Tez is hello default engine for Linux-based HDInsight clusters.</span></span>

<span data-ttu-id="17025-233">Hej [Hive i Tez designdokument](https://cwiki.apache.org/confluence/display/Hive/Hive+on+Tez) innehåller information om hello implementering alternativ och prestandajustering konfigurationer.</span><span class="sxs-lookup"><span data-stu-id="17025-233">hello [Hive on Tez design documents](https://cwiki.apache.org/confluence/display/Hive/Hive+on+Tez) contains details about hello implementation choices and tuning configurations.</span></span>

<span data-ttu-id="17025-234">tooaid felsöka jobb kördes med Tez, HDInsight tillhandahåller följande web-användargränssnitt som gör att du tooview information om jobb på Tez hello:</span><span class="sxs-lookup"><span data-stu-id="17025-234">tooaid in debugging jobs ran using Tez, HDInsight provides hello following web UIs that allow you tooview details of Tez jobs:</span></span>

* [<span data-ttu-id="17025-235">Använd hello Ambari Tez vy på Linux-baserat HDInsight</span><span class="sxs-lookup"><span data-stu-id="17025-235">Use hello Ambari Tez view on Linux-based HDInsight</span></span>](hdinsight-debug-ambari-tez-view.md)

* [<span data-ttu-id="17025-236">Använda hello Tez UI på Windows-baserade HDInsight</span><span class="sxs-lookup"><span data-stu-id="17025-236">Use hello Tez UI on Windows-based HDInsight</span></span>](hdinsight-debug-tez-ui.md)

### <a name="low-latency-analytical-processing-llap"></a><span data-ttu-id="17025-237">Låg latens Analytical Processing (LLAP)</span><span class="sxs-lookup"><span data-stu-id="17025-237">Low Latency Analytical Processing (LLAP)</span></span>

<span data-ttu-id="17025-238">[LLAP](https://cwiki.apache.org/confluence/display/Hive/LLAP) (kallas ibland Live långa och processen) är en ny funktion i Hive 2.0 som kan cachelagra i minnet för frågor.</span><span class="sxs-lookup"><span data-stu-id="17025-238">[LLAP](https://cwiki.apache.org/confluence/display/Hive/LLAP) (sometimes known as Live Long and Process) is a new feature in Hive 2.0 that allows in-memory caching of queries.</span></span> <span data-ttu-id="17025-239">LLAP gör Hive-frågor som är mycket snabbare upp för[26 x snabbare än Hive 1.x i vissa fall](https://hortonworks.com/blog/announcing-apache-hive-2-1-25x-faster-queries-much/).</span><span class="sxs-lookup"><span data-stu-id="17025-239">LLAP makes Hive queries much faster, up too[26x faster than Hive 1.x in some cases](https://hortonworks.com/blog/announcing-apache-hive-2-1-25x-faster-queries-much/).</span></span>

<span data-ttu-id="17025-240">HDInsight ger LLAP hello interaktiva Hive typ av kluster.</span><span class="sxs-lookup"><span data-stu-id="17025-240">HDInsight provides LLAP in hello Interactive Hive cluster type.</span></span> <span data-ttu-id="17025-241">Mer information finns i hello [börja med interaktiva Hive](hdinsight-hadoop-use-interactive-hive.md) dokumentet.</span><span class="sxs-lookup"><span data-stu-id="17025-241">For more information, see hello [Start with Interactive Hive](hdinsight-hadoop-use-interactive-hive.md) document.</span></span>

## <a name="hive-jobs-and-sql-server-integration-services"></a><span data-ttu-id="17025-242">Hive-jobb och SQL Server Integration Services</span><span class="sxs-lookup"><span data-stu-id="17025-242">Hive jobs and SQL Server Integration Services</span></span>

<span data-ttu-id="17025-243">Du kan använda SQL Server Integration Services (SSIS) toorun ett Hive-jobb.</span><span class="sxs-lookup"><span data-stu-id="17025-243">You can use SQL Server Integration Services (SSIS) toorun a Hive job.</span></span> <span data-ttu-id="17025-244">hello Azure Funktionspaket för SSIS tillhandahåller hello följande komponenter som arbetar med Hive-jobb i HDInsight.</span><span class="sxs-lookup"><span data-stu-id="17025-244">hello Azure Feature Pack for SSIS provides hello following components that work with Hive jobs on HDInsight.</span></span>

* <span data-ttu-id="17025-245">[Azure HDInsight Hive-aktivitet][hivetask]</span><span class="sxs-lookup"><span data-stu-id="17025-245">[Azure HDInsight Hive Task][hivetask]</span></span>

* <span data-ttu-id="17025-246">[Azure prenumeration Anslutningshanteraren][connectionmanager]</span><span class="sxs-lookup"><span data-stu-id="17025-246">[Azure Subscription Connection Manager][connectionmanager]</span></span>

<span data-ttu-id="17025-247">Läs mer om hello Azure Funktionspaket för SSIS [här][ssispack].</span><span class="sxs-lookup"><span data-stu-id="17025-247">Learn more about hello Azure Feature Pack for SSIS [here][ssispack].</span></span>

## <span data-ttu-id="17025-248"><a id="nextsteps"></a>Nästa steg</span><span class="sxs-lookup"><span data-stu-id="17025-248"><a id="nextsteps"></a>Next steps</span></span>

<span data-ttu-id="17025-249">Nu när du har lärt dig Hive är och hur toouse den med Hadoop i HDInsight, Använd hello följande länkar tooexplore andra sätt toowork med Azure HDInsight.</span><span class="sxs-lookup"><span data-stu-id="17025-249">Now that you've learned what Hive is and how toouse it with Hadoop in HDInsight, use hello following links tooexplore other ways toowork with Azure HDInsight.</span></span>

* <span data-ttu-id="17025-250">[Ladda upp data tooHDInsight][hdinsight-upload-data]</span><span class="sxs-lookup"><span data-stu-id="17025-250">[Upload data tooHDInsight][hdinsight-upload-data]</span></span>
* <span data-ttu-id="17025-251">[Använda Pig med HDInsight][hdinsight-use-pig]</span><span class="sxs-lookup"><span data-stu-id="17025-251">[Use Pig with HDInsight][hdinsight-use-pig]</span></span>
* <span data-ttu-id="17025-252">[Använda MapReduce-jobb med HDInsight][hdinsight-use-mapreduce]</span><span class="sxs-lookup"><span data-stu-id="17025-252">[Use MapReduce jobs with HDInsight][hdinsight-use-mapreduce]</span></span>

[hdinsight-sdk-documentation]: http://msdnstage.redmond.corp.microsoft.com/library/dn479185.aspx

[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[apache-tez]: http://tez.apache.org
[apache-hive]: http://hive.apache.org/
[apache-log4j]: http://en.wikipedia.org/wiki/Log4j
[hive-on-tez-wiki]: https://cwiki.apache.org/confluence/display/Hive/Hive+on+Tez
[import-to-excel]: http://azure.microsoft.com/documentation/articles/hdinsight-connect-excel-power-query/
[hivetask]: http://msdn.microsoft.com/library/mt146771(v=sql.120).aspx
[connectionmanager]: http://msdn.microsoft.com/library/mt146773(v=sql.120).aspx
[ssispack]: http://msdn.microsoft.com/library/mt146770(v=sql.120).aspx

[hdinsight-use-pig]: hdinsight-use-pig.md
[hdinsight-use-oozie]: hdinsight-use-oozie.md
[hdinsight-analyze-flight-data]: hdinsight-analyze-flight-delay-data.md
[hdinsight-use-mapreduce]: hdinsight-use-mapreduce.md


[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md

[hdinsight-provision]: hdinsight-hadoop-provision-linux-clusters.md
[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md
[hdinsight-upload-data]: hdinsight-upload-data.md

[Powershell-install-configure]: /powershell/azureps-cmdlets-docs
[powershell-here-strings]: http://technet.microsoft.com/library/ee692792.aspx


[cindygross-hive-tables]: http://blogs.msdn.com/b/cindygross/archive/2013/02/06/hdinsight-hive-internal-and-external-tables-intro.aspx
