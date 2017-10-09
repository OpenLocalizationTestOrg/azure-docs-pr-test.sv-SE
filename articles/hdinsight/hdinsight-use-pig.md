---
title: aaaUse Hadoop Pig i HDInsight | Microsoft Docs
description: "Lär dig hur toouse svin med Hadoop i HDInsight."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: acfeb52b-4b81-4a7d-af77-3e9908407404
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 08/15/2017
ms.author: larryfr
ms.openlocfilehash: 90850f2c742b8954c66ce277127e01b14fc3906f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-pig-with-hadoop-on-hdinsight"></a><span data-ttu-id="32a8b-103">Använda Pig med Hadoop i HDInsight</span><span class="sxs-lookup"><span data-stu-id="32a8b-103">Use Pig with Hadoop on HDInsight</span></span>

<span data-ttu-id="32a8b-104">Lär dig hur toouse [Apache Pig](http://pig.apache.org/) med HDInsight...</span><span class="sxs-lookup"><span data-stu-id="32a8b-104">Learn how toouse [Apache Pig](http://pig.apache.org/) with HDInsight...</span></span>

<span data-ttu-id="32a8b-105">Pig är en plattform för att skapa program för Hadoop med hjälp av en procedurmässig språk som kallas *Pig Latin*.</span><span class="sxs-lookup"><span data-stu-id="32a8b-105">Pig is a platform for creating programs for Hadoop by using a procedural language known as *Pig Latin*.</span></span> <span data-ttu-id="32a8b-106">Pig är en alternativ tooJava för att skapa *MapReduce* lösningar och ingår i Azure HDInsight.</span><span class="sxs-lookup"><span data-stu-id="32a8b-106">Pig is an alternative tooJava for creating *MapReduce* solutions, and it is included with Azure HDInsight.</span></span> <span data-ttu-id="32a8b-107">Använd följande tabell toodiscover hello olika sätt att du kan använda Pig med HDInsight hello:</span><span class="sxs-lookup"><span data-stu-id="32a8b-107">Use hello following table toodiscover hello various ways that Pig can be used with HDInsight:</span></span>

| <span data-ttu-id="32a8b-108">**Använd den här** om du vill...</span><span class="sxs-lookup"><span data-stu-id="32a8b-108">**Use this** if you want...</span></span> | <span data-ttu-id="32a8b-109">.. .an **interaktiva** shell</span><span class="sxs-lookup"><span data-stu-id="32a8b-109">...an **interactive** shell</span></span> | <span data-ttu-id="32a8b-110">... **batch** bearbetning</span><span class="sxs-lookup"><span data-stu-id="32a8b-110">...**batch** processing</span></span> | <span data-ttu-id="32a8b-111">... med detta **klustret operativsystem**</span><span class="sxs-lookup"><span data-stu-id="32a8b-111">...with this **cluster operating system**</span></span> | <span data-ttu-id="32a8b-112">.. .from detta **klientoperativsystem**</span><span class="sxs-lookup"><span data-stu-id="32a8b-112">...from this **client operating system**</span></span> |
|:--- |:---:|:---:|:--- |:--- |
| [<span data-ttu-id="32a8b-113">SSH</span><span class="sxs-lookup"><span data-stu-id="32a8b-113">SSH</span></span>](hdinsight-hadoop-use-pig-ssh.md) |<span data-ttu-id="32a8b-114">✔</span><span class="sxs-lookup"><span data-stu-id="32a8b-114">✔</span></span> |<span data-ttu-id="32a8b-115">✔</span><span class="sxs-lookup"><span data-stu-id="32a8b-115">✔</span></span> |<span data-ttu-id="32a8b-116">Linux</span><span class="sxs-lookup"><span data-stu-id="32a8b-116">Linux</span></span> |<span data-ttu-id="32a8b-117">Linux, Unix, Mac OS X eller Windows</span><span class="sxs-lookup"><span data-stu-id="32a8b-117">Linux, Unix, Mac OS X, or Windows</span></span> |
| [<span data-ttu-id="32a8b-118">REST-API</span><span class="sxs-lookup"><span data-stu-id="32a8b-118">REST API</span></span>](hdinsight-hadoop-use-pig-curl.md) |&nbsp; |<span data-ttu-id="32a8b-119">✔</span><span class="sxs-lookup"><span data-stu-id="32a8b-119">✔</span></span> |<span data-ttu-id="32a8b-120">Linux- eller Windows</span><span class="sxs-lookup"><span data-stu-id="32a8b-120">Linux or Windows</span></span> |<span data-ttu-id="32a8b-121">Linux, Unix, Mac OS X eller Windows</span><span class="sxs-lookup"><span data-stu-id="32a8b-121">Linux, Unix, Mac OS X, or Windows</span></span> |
| [<span data-ttu-id="32a8b-122">.NET SDK för Hadoop</span><span class="sxs-lookup"><span data-stu-id="32a8b-122">.NET SDK for Hadoop</span></span>](hdinsight-hadoop-use-pig-dotnet-sdk.md) |&nbsp; |<span data-ttu-id="32a8b-123">✔</span><span class="sxs-lookup"><span data-stu-id="32a8b-123">✔</span></span> |<span data-ttu-id="32a8b-124">Linux- eller Windows</span><span class="sxs-lookup"><span data-stu-id="32a8b-124">Linux or Windows</span></span> |<span data-ttu-id="32a8b-125">Windows (för tillfället)</span><span class="sxs-lookup"><span data-stu-id="32a8b-125">Windows (for now)</span></span> |
| [<span data-ttu-id="32a8b-126">Windows PowerShell</span><span class="sxs-lookup"><span data-stu-id="32a8b-126">Windows PowerShell</span></span>](hdinsight-hadoop-use-pig-powershell.md) |&nbsp; |<span data-ttu-id="32a8b-127">✔</span><span class="sxs-lookup"><span data-stu-id="32a8b-127">✔</span></span> |<span data-ttu-id="32a8b-128">Linux- eller Windows</span><span class="sxs-lookup"><span data-stu-id="32a8b-128">Linux or Windows</span></span> |<span data-ttu-id="32a8b-129">Windows</span><span class="sxs-lookup"><span data-stu-id="32a8b-129">Windows</span></span> |
| <span data-ttu-id="32a8b-130">[Fjärrskrivbord](hdinsight-hadoop-use-pig-remote-desktop.md) (HDInsight 3.2 och 3.3)</span><span class="sxs-lookup"><span data-stu-id="32a8b-130">[Remote Desktop](hdinsight-hadoop-use-pig-remote-desktop.md) (HDInsight 3.2 and 3.3)</span></span> |<span data-ttu-id="32a8b-131">✔</span><span class="sxs-lookup"><span data-stu-id="32a8b-131">✔</span></span> |<span data-ttu-id="32a8b-132">✔</span><span class="sxs-lookup"><span data-stu-id="32a8b-132">✔</span></span> |<span data-ttu-id="32a8b-133">Windows</span><span class="sxs-lookup"><span data-stu-id="32a8b-133">Windows</span></span> |<span data-ttu-id="32a8b-134">Windows</span><span class="sxs-lookup"><span data-stu-id="32a8b-134">Windows</span></span> |

> [!IMPORTANT]
> <span data-ttu-id="32a8b-135">Linux är hello endast operativsystem på HDInsight version 3.4 eller senare.</span><span class="sxs-lookup"><span data-stu-id="32a8b-135">Linux is hello only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="32a8b-136">Mer information finns i [HDInsight-avveckling på Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="32a8b-136">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

## <span data-ttu-id="32a8b-137"><a id="why"></a>Varför använda Pig</span><span class="sxs-lookup"><span data-stu-id="32a8b-137"><a id="why"></a>Why use Pig</span></span>

<span data-ttu-id="32a8b-138">En av hello utmaningar för bearbetning av data genom att använda MapReduce i Hadoop implementerar logik för bearbetning med hjälp av endast en karta och minska funktionen.</span><span class="sxs-lookup"><span data-stu-id="32a8b-138">One of hello challenges of processing data by using MapReduce in Hadoop is implementing your processing logic by using only a map and a reduce function.</span></span> <span data-ttu-id="32a8b-139">För komplexa bearbetning av du ofta har sammankopplade toobreak bearbetning till flera MapReduce-åtgärder som är tooachieve hello önskat resultat.</span><span class="sxs-lookup"><span data-stu-id="32a8b-139">For complex processing, you often have toobreak processing into multiple MapReduce operations that are chained together tooachieve hello desired result.</span></span>

<span data-ttu-id="32a8b-140">Pig kan toodefine bearbetning som en serie transformationer som hello dataflöden via tooproduce hello önskad utdata.</span><span class="sxs-lookup"><span data-stu-id="32a8b-140">Pig allows you toodefine processing as a series of transformations that hello data flows through tooproduce hello desired output.</span></span>

<span data-ttu-id="32a8b-141">Hej Pig Latin språk kan du toodescribe hello dataflöde från rådata indata via en eller flera omvandlingar tooproduce hello önskad utdata.</span><span class="sxs-lookup"><span data-stu-id="32a8b-141">hello Pig Latin language allows you toodescribe hello data flow from raw input, through one or more transformations, tooproduce hello desired output.</span></span> <span data-ttu-id="32a8b-142">Pig Latin program följer detta allmänna mönster:</span><span class="sxs-lookup"><span data-stu-id="32a8b-142">Pig Latin programs follow this general pattern:</span></span>

* <span data-ttu-id="32a8b-143">**Läs in**: Läs data toobe ändras från hello-filsystem</span><span class="sxs-lookup"><span data-stu-id="32a8b-143">**Load**: Read data toobe manipulated from hello file system</span></span>

* <span data-ttu-id="32a8b-144">**Transformera**: ändra hello-data</span><span class="sxs-lookup"><span data-stu-id="32a8b-144">**Transform**: Manipulate hello data</span></span>

* <span data-ttu-id="32a8b-145">**Dump eller lagra**: utdata data toohello skärmbild eller lagra för bearbetning</span><span class="sxs-lookup"><span data-stu-id="32a8b-145">**Dump or store**: Output data toohello screen or store it for processing</span></span>

### <a name="user-defined-functions"></a><span data-ttu-id="32a8b-146">Användardefinierade funktioner</span><span class="sxs-lookup"><span data-stu-id="32a8b-146">User-defined functions</span></span>

<span data-ttu-id="32a8b-147">Pig Latin stöder även användardefinierade funktioner (UDF), vilket gör att du tooinvoke externa komponenter som implementerar logik som är svår toomodel i Pig Latin.</span><span class="sxs-lookup"><span data-stu-id="32a8b-147">Pig Latin also supports user-defined functions (UDF), which allows you tooinvoke external components that implement logic that is difficult toomodel in Pig Latin.</span></span>

<span data-ttu-id="32a8b-148">Läs mer om Pig Latin [Pig Latin referens manuell 1](http://pig.apache.org/docs/r0.7.0/piglatin_ref1.html) och [Pig Latin referens manuell 2](http://pig.apache.org/docs/r0.7.0/piglatin_ref2.html).</span><span class="sxs-lookup"><span data-stu-id="32a8b-148">For more information about Pig Latin, see [Pig Latin Reference Manual 1](http://pig.apache.org/docs/r0.7.0/piglatin_ref1.html) and [Pig Latin Reference Manual 2](http://pig.apache.org/docs/r0.7.0/piglatin_ref2.html).</span></span>

<span data-ttu-id="32a8b-149">Ett exempel på med Pig UDF: er, finns i hello följande dokument:</span><span class="sxs-lookup"><span data-stu-id="32a8b-149">For an example of using UDFs with Pig, see hello following documents:</span></span>

* <span data-ttu-id="32a8b-150">[Använda DataFu med Pig i HDInsight](hdinsight-hadoop-use-pig-datafu-udf.md) -DataFu är en samling användbar UDF: er som underhålls av Apache</span><span class="sxs-lookup"><span data-stu-id="32a8b-150">[Use DataFu with Pig in HDInsight](hdinsight-hadoop-use-pig-datafu-udf.md) - DataFu is a collection of useful UDFs maintained by Apache</span></span>
* [<span data-ttu-id="32a8b-151">Använda Python med Pig och Hive i HDInsight</span><span class="sxs-lookup"><span data-stu-id="32a8b-151">Use Python with Pig and Hive in HDInsight</span></span>](hdinsight-python.md)
* [<span data-ttu-id="32a8b-152">Använda C# med Hive och Pig i HDInsight</span><span class="sxs-lookup"><span data-stu-id="32a8b-152">Use C# with Hive and Pig in HDInsight</span></span>](hdinsight-hadoop-hive-pig-udf-dotnet-csharp.md)

## <span data-ttu-id="32a8b-153"><a id="data"></a>Exempeldata</span><span class="sxs-lookup"><span data-stu-id="32a8b-153"><a id="data"></a>Example data</span></span>

<span data-ttu-id="32a8b-154">HDInsight tillhandahåller olika exempel datauppsättningar som lagras i hello `/example/data` och `/HdiSamples` kataloger.</span><span class="sxs-lookup"><span data-stu-id="32a8b-154">HDInsight provides various example data sets, which are stored in hello `/example/data` and `/HdiSamples` directories.</span></span> <span data-ttu-id="32a8b-155">Dessa kataloger finns i hello standardlagring för klustret.</span><span class="sxs-lookup"><span data-stu-id="32a8b-155">These directories are in hello default storage for your cluster.</span></span> <span data-ttu-id="32a8b-156">Hej Pig exemplet i det här dokumentet använder hello *log4j* filen från `/example/data/sample.log`.</span><span class="sxs-lookup"><span data-stu-id="32a8b-156">hello Pig example in this document uses hello *log4j* file from `/example/data/sample.log`.</span></span>

<span data-ttu-id="32a8b-157">Varje logg i hello fil består av en rad med fält som innehåller en `[LOG LEVEL]` fältet tooshow hello-typ och hello allvarlighetsgrad, till exempel:</span><span class="sxs-lookup"><span data-stu-id="32a8b-157">Each log inside hello file consists of a line of fields that contains a `[LOG LEVEL]` field tooshow hello type and hello severity, for example:</span></span>

    2012-02-03 20:26:41 SampleClass3 [ERROR] verbose detail for id 1527353937

<span data-ttu-id="32a8b-158">I föregående exempel hello är hello loggningsnivån fel.</span><span class="sxs-lookup"><span data-stu-id="32a8b-158">In hello previous example, hello log level is ERROR.</span></span>

> [!NOTE]
> <span data-ttu-id="32a8b-159">Du kan också generera en log4j-fil med hjälp av hello [Apache Log4j](http://en.wikipedia.org/wiki/Log4j) loggning av verktyget och sedan ladda upp filen tooyour blobben.</span><span class="sxs-lookup"><span data-stu-id="32a8b-159">You can also generate a log4j file by using hello [Apache Log4j](http://en.wikipedia.org/wiki/Log4j) logging tool and then upload that file tooyour blob.</span></span> <span data-ttu-id="32a8b-160">Se [överför Data tooHDInsight](hdinsight-upload-data.md) anvisningar.</span><span class="sxs-lookup"><span data-stu-id="32a8b-160">See [Upload Data tooHDInsight](hdinsight-upload-data.md) for instructions.</span></span> <span data-ttu-id="32a8b-161">Mer information om hur du använder blobbar i Azure storage med HDInsight finns [använda Azure Blob Storage med HDInsight](hdinsight-hadoop-use-blob-storage.md).</span><span class="sxs-lookup"><span data-stu-id="32a8b-161">For more information about how blobs in Azure storage are used with HDInsight, see [Use Azure Blob Storage with HDInsight](hdinsight-hadoop-use-blob-storage.md).</span></span>

## <span data-ttu-id="32a8b-162"><a id="job"></a>Exempel jobb</span><span class="sxs-lookup"><span data-stu-id="32a8b-162"><a id="job"></a>Example job</span></span>

<span data-ttu-id="32a8b-163">hello följande Pig Latin jobbet läses in hello `sample.log` filen från hello standardlagring för HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="32a8b-163">hello following Pig Latin job loads hello `sample.log` file from hello default storage for your HDInsight cluster.</span></span> <span data-ttu-id="32a8b-164">Därefter utför en serie transformationer som resulterar i en uppräkning av hur många gånger varje nivå i hello inkommande loggdata.</span><span class="sxs-lookup"><span data-stu-id="32a8b-164">Then it performs a series of transformations that result in a count of how many times each log level occurred in hello input data.</span></span> <span data-ttu-id="32a8b-165">hello resultat skrivs tooSTDOUT.</span><span class="sxs-lookup"><span data-stu-id="32a8b-165">hello results are written tooSTDOUT.</span></span>

    LOGS = LOAD 'wasb:///example/data/sample.log';
    LEVELS = foreach LOGS generate REGEX_EXTRACT($0, '(TRACE|DEBUG|INFO|WARN|ERROR|FATAL)', 1)  as LOGLEVEL;
    FILTEREDLEVELS = FILTER LEVELS by LOGLEVEL is not null;
    GROUPEDLEVELS = GROUP FILTEREDLEVELS by LOGLEVEL;
    FREQUENCIES = foreach GROUPEDLEVELS generate group as LOGLEVEL, COUNT(FILTEREDLEVELS.LOGLEVEL) as COUNT;
    RESULT = order FREQUENCIES by COUNT desc;
    DUMP RESULT;

<span data-ttu-id="32a8b-166">hello visar följande bild en sammanfattning av vad varje transformation har toohello data.</span><span class="sxs-lookup"><span data-stu-id="32a8b-166">hello following image shows a summary of what each transformation does toohello data.</span></span>

![Grafisk representation av hello omvandlingar][image-hdi-pig-data-transformation]

## <span data-ttu-id="32a8b-168"><a id="run"></a>Kör jobb för hello Pig Latin</span><span class="sxs-lookup"><span data-stu-id="32a8b-168"><a id="run"></a>Run hello Pig Latin job</span></span>

<span data-ttu-id="32a8b-169">HDInsight kan köra Pig Latin jobb med hjälp av olika metoder.</span><span class="sxs-lookup"><span data-stu-id="32a8b-169">HDInsight can run Pig Latin jobs by using a variety of methods.</span></span> <span data-ttu-id="32a8b-170">Använd hello efter tabellen toodecide vilken metod som passar dig sedan länken hello en genomgång.</span><span class="sxs-lookup"><span data-stu-id="32a8b-170">Use hello following table toodecide which method is right for you, then follow hello link for a walkthrough.</span></span>

| <span data-ttu-id="32a8b-171">**Använd den här** om du vill...</span><span class="sxs-lookup"><span data-stu-id="32a8b-171">**Use this** if you want...</span></span> | <span data-ttu-id="32a8b-172">.. .an **interaktiva** shell</span><span class="sxs-lookup"><span data-stu-id="32a8b-172">...an **interactive** shell</span></span> | <span data-ttu-id="32a8b-173">... **batch** bearbetning</span><span class="sxs-lookup"><span data-stu-id="32a8b-173">...**batch** processing</span></span> | <span data-ttu-id="32a8b-174">... med detta **klustret operativsystem**</span><span class="sxs-lookup"><span data-stu-id="32a8b-174">...with this **cluster operating system**</span></span> | <span data-ttu-id="32a8b-175">.. .from detta **klienten**</span><span class="sxs-lookup"><span data-stu-id="32a8b-175">...from this **client**</span></span> |
|:--- |:---:|:---:|:--- |:--- |
| [<span data-ttu-id="32a8b-176">SSH</span><span class="sxs-lookup"><span data-stu-id="32a8b-176">SSH</span></span>](hdinsight-hadoop-use-pig-ssh.md) |<span data-ttu-id="32a8b-177">✔</span><span class="sxs-lookup"><span data-stu-id="32a8b-177">✔</span></span> |<span data-ttu-id="32a8b-178">✔</span><span class="sxs-lookup"><span data-stu-id="32a8b-178">✔</span></span> |<span data-ttu-id="32a8b-179">Linux</span><span class="sxs-lookup"><span data-stu-id="32a8b-179">Linux</span></span> |<span data-ttu-id="32a8b-180">Linux, Unix, Mac OS X eller Windows</span><span class="sxs-lookup"><span data-stu-id="32a8b-180">Linux, Unix, Mac OS X, or Windows</span></span> |
| [<span data-ttu-id="32a8b-181">CURL</span><span class="sxs-lookup"><span data-stu-id="32a8b-181">Curl</span></span>](hdinsight-hadoop-use-pig-curl.md) |&nbsp; |<span data-ttu-id="32a8b-182">✔</span><span class="sxs-lookup"><span data-stu-id="32a8b-182">✔</span></span> |<span data-ttu-id="32a8b-183">Linux- eller Windows</span><span class="sxs-lookup"><span data-stu-id="32a8b-183">Linux or Windows</span></span> |<span data-ttu-id="32a8b-184">Linux, Unix, Mac OS X eller Windows</span><span class="sxs-lookup"><span data-stu-id="32a8b-184">Linux, Unix, Mac OS X, or Windows</span></span> |
| [<span data-ttu-id="32a8b-185">.NET SDK för Hadoop</span><span class="sxs-lookup"><span data-stu-id="32a8b-185">.NET SDK for Hadoop</span></span>](hdinsight-hadoop-use-pig-dotnet-sdk.md) |&nbsp; |<span data-ttu-id="32a8b-186">✔</span><span class="sxs-lookup"><span data-stu-id="32a8b-186">✔</span></span> |<span data-ttu-id="32a8b-187">Linux- eller Windows</span><span class="sxs-lookup"><span data-stu-id="32a8b-187">Linux or Windows</span></span> |<span data-ttu-id="32a8b-188">Windows (för tillfället)</span><span class="sxs-lookup"><span data-stu-id="32a8b-188">Windows (for now)</span></span> |
| [<span data-ttu-id="32a8b-189">Windows PowerShell</span><span class="sxs-lookup"><span data-stu-id="32a8b-189">Windows PowerShell</span></span>](hdinsight-hadoop-use-pig-powershell.md) |&nbsp; |<span data-ttu-id="32a8b-190">✔</span><span class="sxs-lookup"><span data-stu-id="32a8b-190">✔</span></span> |<span data-ttu-id="32a8b-191">Linux- eller Windows</span><span class="sxs-lookup"><span data-stu-id="32a8b-191">Linux or Windows</span></span> |<span data-ttu-id="32a8b-192">Windows</span><span class="sxs-lookup"><span data-stu-id="32a8b-192">Windows</span></span> |
| <span data-ttu-id="32a8b-193">[Fjärrskrivbord](hdinsight-hadoop-use-pig-remote-desktop.md) (HDInsight 3.2 och 3.3)</span><span class="sxs-lookup"><span data-stu-id="32a8b-193">[Remote Desktop](hdinsight-hadoop-use-pig-remote-desktop.md) (HDInsight 3.2 and 3.3)</span></span> |<span data-ttu-id="32a8b-194">✔</span><span class="sxs-lookup"><span data-stu-id="32a8b-194">✔</span></span> |<span data-ttu-id="32a8b-195">✔</span><span class="sxs-lookup"><span data-stu-id="32a8b-195">✔</span></span> |<span data-ttu-id="32a8b-196">Windows</span><span class="sxs-lookup"><span data-stu-id="32a8b-196">Windows</span></span> |<span data-ttu-id="32a8b-197">Windows</span><span class="sxs-lookup"><span data-stu-id="32a8b-197">Windows</span></span> |

> [!IMPORTANT]
> <span data-ttu-id="32a8b-198">Linux är hello endast operativsystem på HDInsight version 3.4 eller senare.</span><span class="sxs-lookup"><span data-stu-id="32a8b-198">Linux is hello only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="32a8b-199">Mer information finns i [HDInsight-avveckling på Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="32a8b-199">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

## <a name="pig-and-sql-server-integration-services"></a><span data-ttu-id="32a8b-200">Pig och SQL Server Integration Services</span><span class="sxs-lookup"><span data-stu-id="32a8b-200">Pig and SQL Server Integration Services</span></span>

<span data-ttu-id="32a8b-201">Du kan använda SQL Server Integration Services (SSIS) toorun Pig-jobbet.</span><span class="sxs-lookup"><span data-stu-id="32a8b-201">You can use SQL Server Integration Services (SSIS) toorun a Pig job.</span></span> <span data-ttu-id="32a8b-202">hello Azure Funktionspaket för SSIS tillhandahåller hello följande komponenter som arbetar med Pig-jobb i HDInsight.</span><span class="sxs-lookup"><span data-stu-id="32a8b-202">hello Azure Feature Pack for SSIS provides hello following components that work with Pig jobs on HDInsight.</span></span>

* <span data-ttu-id="32a8b-203">[Azure HDInsight Pig-aktivitet][pigtask]</span><span class="sxs-lookup"><span data-stu-id="32a8b-203">[Azure HDInsight Pig Task][pigtask]</span></span>

* <span data-ttu-id="32a8b-204">[Azure prenumeration Anslutningshanteraren][connectionmanager]</span><span class="sxs-lookup"><span data-stu-id="32a8b-204">[Azure Subscription Connection Manager][connectionmanager]</span></span>

<span data-ttu-id="32a8b-205">Läs mer om hello Azure Funktionspaket för SSIS [här][ssispack].</span><span class="sxs-lookup"><span data-stu-id="32a8b-205">Learn more about hello Azure Feature Pack for SSIS [here][ssispack].</span></span>

## <span data-ttu-id="32a8b-206"><a id="nextsteps"></a>Nästa steg</span><span class="sxs-lookup"><span data-stu-id="32a8b-206"><a id="nextsteps"></a>Next steps</span></span>
<span data-ttu-id="32a8b-207">Nu när du har lärt dig hur länkar toouse Pig med HDInsight, Använd hello följande tooexplore andra sätt toowork med Azure HDInsight.</span><span class="sxs-lookup"><span data-stu-id="32a8b-207">Now that you have learned how toouse Pig with HDInsight, use hello following links tooexplore other ways toowork with Azure HDInsight.</span></span>

* <span data-ttu-id="32a8b-208">[Ladda upp data tooHDInsight][hdinsight-upload-data]</span><span class="sxs-lookup"><span data-stu-id="32a8b-208">[Upload data tooHDInsight][hdinsight-upload-data]</span></span>
* <span data-ttu-id="32a8b-209">[Använda Hive med HDInsight][hdinsight-use-hive]</span><span class="sxs-lookup"><span data-stu-id="32a8b-209">[Use Hive with HDInsight][hdinsight-use-hive]</span></span>
* [<span data-ttu-id="32a8b-210">Använda Sqoop med HDInsight</span><span class="sxs-lookup"><span data-stu-id="32a8b-210">Use Sqoop with HDInsight</span></span>](hdinsight-use-sqoop.md)
* [<span data-ttu-id="32a8b-211">Använda Oozie med HDInsight</span><span class="sxs-lookup"><span data-stu-id="32a8b-211">Use Oozie with HDInsight</span></span>](hdinsight-use-oozie.md)
* <span data-ttu-id="32a8b-212">[Använda MapReduce-jobb med HDInsight][hdinsight-use-mapreduce]</span><span class="sxs-lookup"><span data-stu-id="32a8b-212">[Use MapReduce jobs with HDInsight][hdinsight-use-mapreduce]</span></span>

[apachepig-home]: http://pig.apache.org/
[putty]: http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html
[curl]: http://curl.haxx.se/
[pigtask]: http://msdn.microsoft.com/library/mt146781(v=sql.120).aspx
[connectionmanager]: http://msdn.microsoft.com/library/mt146773(v=sql.120).aspx
[ssispack]: http://msdn.microsoft.com/library/mt146770(v=sql.120).aspx


[hdinsight-upload-data]: hdinsight-upload-data.md

[hdinsight-admin-powershell]: hdinsight-administer-use-powershell.md

[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-use-mapreduce]: hdinsight-use-mapreduce.md

[hdinsight-provision]: hdinsight-hadoop-provision-linux-clusters.md
[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md#mapreduce-sdk

[Powershell-install-configure]: /powershell/azureps-cmdlets-docs

[powershell-start]: http://technet.microsoft.com/library/hh847889.aspx


[image-hdi-pig-data-transformation]: ./media/hdinsight-use-pig/HDI.DataTransformation.gif
