---
title: "aaaUse Hadoop Hive och fjärrskrivbord i HDInsight - Azure | Microsoft Docs"
description: "Lär dig hur tooconnect tooHadoop kluster i HDInsight med hjälp av fjärrskrivbord och köra Hive-frågor med hjälp av hello Hive kommandoradsgränssnitt."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 8c228e35-d58a-4f22-917a-1d20c9da89b4
ms.service: hdinsight
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 01/12/2017
ms.author: larryfr
ROBOTS: NOINDEX
ms.openlocfilehash: f86ffc1be33a8b0b2346d1a1388e5dfa6d0f8777
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-hive-with-hadoop-on-hdinsight-with-remote-desktop"></a><span data-ttu-id="5396a-103">Använda Hive med Hadoop i HDInsight med fjärrskrivbord</span><span class="sxs-lookup"><span data-stu-id="5396a-103">Use Hive with Hadoop on HDInsight with Remote Desktop</span></span>
[!INCLUDE [hive-selector](../../includes/hdinsight-selector-use-hive.md)]

<span data-ttu-id="5396a-104">I den här artikeln får du lära dig hur tooconnect tooan HDInsight-kluster med hjälp av fjärrskrivbord och sedan köra Hive-frågor med hjälp av hello Hive kommandoradsgränssnittet (CLI).</span><span class="sxs-lookup"><span data-stu-id="5396a-104">In this article, you will learn how tooconnect tooan HDInsight cluster by using Remote Desktop, and then run Hive queries by using hello Hive Command-Line Interface (CLI).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="5396a-105">Fjärrskrivbord är bara tillgängligt på HDInsight-kluster som använder Windows som hello-operativsystem.</span><span class="sxs-lookup"><span data-stu-id="5396a-105">Remote Desktop is only available on HDInsight clusters that use Windows as hello operating system.</span></span> <span data-ttu-id="5396a-106">Linux är hello endast operativsystem på HDInsight version 3.4 eller senare.</span><span class="sxs-lookup"><span data-stu-id="5396a-106">Linux is hello only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="5396a-107">Mer information finns i [HDInsight-avveckling på Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="5396a-107">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>
>
> <span data-ttu-id="5396a-108">För HDInsight 3.4 eller större finns [använda Hive med HDInsight och Beeline](hdinsight-hadoop-use-hive-beeline.md) information om hur du kör Hive-frågor direkt på hello kluster från en kommandorad.</span><span class="sxs-lookup"><span data-stu-id="5396a-108">For HDInsight 3.4 or greater, see [Use Hive with HDInsight and Beeline](hdinsight-hadoop-use-hive-beeline.md) for information on running Hive queries directly on hello cluster from a command-line.</span></span>

## <span data-ttu-id="5396a-109"><a id="prereq"></a>Förhandskrav</span><span class="sxs-lookup"><span data-stu-id="5396a-109"><a id="prereq"></a>Prerequisites</span></span>
<span data-ttu-id="5396a-110">toocomplete hello stegen i den här artikeln, behöver du hello följande:</span><span class="sxs-lookup"><span data-stu-id="5396a-110">toocomplete hello steps in this article, you will need hello following:</span></span>

* <span data-ttu-id="5396a-111">Ett kluster med Windows-baserade HDInsight (Hadoop på HDInsight)</span><span class="sxs-lookup"><span data-stu-id="5396a-111">A Windows-based HDInsight (Hadoop on HDInsight) cluster</span></span>
* <span data-ttu-id="5396a-112">En klientdator som kör Windows 10, Windows 8 eller Windows 7</span><span class="sxs-lookup"><span data-stu-id="5396a-112">A client computer running Windows 10, Window 8, or Windows 7</span></span>

## <span data-ttu-id="5396a-113"><a id="connect"></a>Ansluta med fjärrskrivbord</span><span class="sxs-lookup"><span data-stu-id="5396a-113"><a id="connect"></a>Connect with Remote Desktop</span></span>
<span data-ttu-id="5396a-114">Aktivera Fjärrskrivbord för hello HDInsight-kluster och sedan ansluta tooit genom att följa anvisningarna hello på [ansluta tooHDInsight kluster med RDP](hdinsight-administer-use-management-portal.md#connect-to-clusters-using-rdp).</span><span class="sxs-lookup"><span data-stu-id="5396a-114">Enable Remote Desktop for hello HDInsight cluster, then connect tooit by following hello instructions at [Connect tooHDInsight clusters using RDP](hdinsight-administer-use-management-portal.md#connect-to-clusters-using-rdp).</span></span>

## <span data-ttu-id="5396a-115"><a id="hive"></a>Använd hello Hive-kommando</span><span class="sxs-lookup"><span data-stu-id="5396a-115"><a id="hive"></a>Use hello Hive command</span></span>
<span data-ttu-id="5396a-116">När du har anslutit toohello desktop för hello HDInsight-kluster Använd följande steg toowork med Hive hello:</span><span class="sxs-lookup"><span data-stu-id="5396a-116">When you have connected toohello desktop for hello HDInsight cluster, use hello following steps toowork with Hive:</span></span>

1. <span data-ttu-id="5396a-117">Starta från hello HDInsight desktop hello **Hadoop kommandoraden**.</span><span class="sxs-lookup"><span data-stu-id="5396a-117">From hello HDInsight desktop, start hello **Hadoop Command Line**.</span></span>
2. <span data-ttu-id="5396a-118">Ange följande kommando toostart hello Hive CLI hello:</span><span class="sxs-lookup"><span data-stu-id="5396a-118">Enter hello following command toostart hello Hive CLI:</span></span>

        %hive_home%\bin\hive

    <span data-ttu-id="5396a-119">När hello CLI har startats visas hello Hive CLI prompten: `hive>`.</span><span class="sxs-lookup"><span data-stu-id="5396a-119">When hello CLI has started, you will see hello Hive CLI prompt: `hive>`.</span></span>
3. <span data-ttu-id="5396a-120">Med hjälp av hello CLI, ange hello följande instruktioner toocreate en ny tabell med namnet **log4jLogs** med exempeldata:</span><span class="sxs-lookup"><span data-stu-id="5396a-120">Using hello CLI, enter hello following statements toocreate a new table named **log4jLogs** using sample data:</span></span>

        set hive.execution.engine=tez;
        DROP TABLE log4jLogs;
        CREATE EXTERNAL TABLE log4jLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string)
        ROW FORMAT DELIMITED FIELDS TERMINATED BY ' '
        STORED AS TEXTFILE LOCATION 'wasb:///example/data/';
        SELECT t4 AS sev, COUNT(*) AS count FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log' GROUP BY t4;

    <span data-ttu-id="5396a-121">Dessa instruktioner utför hello följande åtgärder:</span><span class="sxs-lookup"><span data-stu-id="5396a-121">These statements perform hello following actions:</span></span>

   * <span data-ttu-id="5396a-122">**DROP TABLE**: tar bort hello och hello data om hello tabellen finns redan.</span><span class="sxs-lookup"><span data-stu-id="5396a-122">**DROP TABLE**: Deletes hello table and hello data file if hello table already exists.</span></span>
   * <span data-ttu-id="5396a-123">**Skapa extern tabell**: skapar en ny ”externa” tabell i Hive.</span><span class="sxs-lookup"><span data-stu-id="5396a-123">**CREATE EXTERNAL TABLE**: Creates a new 'external' table in Hive.</span></span> <span data-ttu-id="5396a-124">Externa tabeller lagra endast hello tabelldefinition i Hive (hello data finns kvar i hello ursprungsplatsen).</span><span class="sxs-lookup"><span data-stu-id="5396a-124">External tables store only hello table definition in Hive (hello data is left in hello original location).</span></span>

     > [!NOTE]
     > <span data-ttu-id="5396a-125">Externa tabeller ska användas när du förväntar dig hello underliggande data toobe uppdateras av en extern källa (till exempel en automatisk överföring av data) eller av en annan MapReduce-åtgärd, men du vill använda Hive frågor toouse hello senaste data.</span><span class="sxs-lookup"><span data-stu-id="5396a-125">External tables should be used when you expect hello underlying data toobe updated by an external source (such as an automated data upload process) or by another MapReduce operation, but you always want Hive queries toouse hello latest data.</span></span>
     >
     > <span data-ttu-id="5396a-126">Släppa en extern tabell har **inte** ta bort data hello, bara hello tabelldefinitionen.</span><span class="sxs-lookup"><span data-stu-id="5396a-126">Dropping an external table does **not** delete hello data, only hello table definition.</span></span>
     >
     >
   * <span data-ttu-id="5396a-127">**RADEN FORMAT**: talar om Hive hur hello data formateras.</span><span class="sxs-lookup"><span data-stu-id="5396a-127">**ROW FORMAT**: Tells Hive how hello data is formatted.</span></span> <span data-ttu-id="5396a-128">I det här fallet avgränsas hello fälten i varje logg med ett blanksteg.</span><span class="sxs-lookup"><span data-stu-id="5396a-128">In this case, hello fields in each log are separated by a space.</span></span>
   * <span data-ttu-id="5396a-129">**LAGRAS AS TEXTFILE plats**: talar om Hive där hello data är lagras (hello exempel/datakatalog) och som den lagras som text.</span><span class="sxs-lookup"><span data-stu-id="5396a-129">**STORED AS TEXTFILE LOCATION**: Tells Hive where hello data is stored (hello example/data directory) and that it is stored as text.</span></span>
   * <span data-ttu-id="5396a-130">**Välj**: väljer en uppräkning av alla rader där kolumnen **t4** innehåller hello värdet **[fel]**.</span><span class="sxs-lookup"><span data-stu-id="5396a-130">**SELECT**: Selects a count of all rows where column **t4** contains hello value **[ERROR]**.</span></span> <span data-ttu-id="5396a-131">Detta bör returnera ett värde av **3** eftersom det finns tre rader som innehåller det här värdet.</span><span class="sxs-lookup"><span data-stu-id="5396a-131">This should return a value of **3** because there are three rows that contain this value.</span></span>
   * <span data-ttu-id="5396a-132">**INPUT__FILE__NAME som '%.log'** -talar om Hive som vi ska bara returnera data från filer som slutar på. log.</span><span class="sxs-lookup"><span data-stu-id="5396a-132">**INPUT__FILE__NAME LIKE '%.log'** - Tells Hive that we should only return data from files ending in .log.</span></span> <span data-ttu-id="5396a-133">Detta begränsar hello sökning toohello sample.log-fil som innehåller hello data och håller den från att returnera data från andra exempel filer som inte matchar hello schemat som vi har definierat.</span><span class="sxs-lookup"><span data-stu-id="5396a-133">This restricts hello search toohello sample.log file that contains hello data, and keeps it from returning data from other example data files that do not match hello schema we defined.</span></span>
4. <span data-ttu-id="5396a-134">Använd hello följande instruktioner toocreate en ny ”interna” tabell med namnet **errorLogs**:</span><span class="sxs-lookup"><span data-stu-id="5396a-134">Use hello following statements toocreate a new 'internal' table named **errorLogs**:</span></span>

        CREATE TABLE IF NOT EXISTS errorLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string) STORED AS ORC;
        INSERT OVERWRITE TABLE errorLogs SELECT t1, t2, t3, t4, t5, t6, t7 FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log';

    <span data-ttu-id="5396a-135">Dessa instruktioner utför hello följande åtgärder:</span><span class="sxs-lookup"><span data-stu-id="5396a-135">These statements perform hello following actions:</span></span>

   * <span data-ttu-id="5396a-136">**Skapa tabell om inte finns**: skapar en tabell om den inte redan finns.</span><span class="sxs-lookup"><span data-stu-id="5396a-136">**CREATE TABLE IF NOT EXISTS**: Creates a table if it does not already exist.</span></span> <span data-ttu-id="5396a-137">Eftersom hello **externa** nyckelordet används inte, det här är en intern tabell som lagras i datalagret för hello Hive och hanteras helt av Hive.</span><span class="sxs-lookup"><span data-stu-id="5396a-137">Because hello **EXTERNAL** keyword is not used, this is an internal table, which is stored in hello Hive data warehouse and is managed completely by Hive.</span></span>

     > [!NOTE]
     > <span data-ttu-id="5396a-138">Till skillnad från **externa** tabeller, släppa en intern tabell även tar bort hello underliggande data.</span><span class="sxs-lookup"><span data-stu-id="5396a-138">Unlike **EXTERNAL** tables, dropping an internal table also deletes hello underlying data.</span></span>
     >
     >
   * <span data-ttu-id="5396a-139">**LAGRAS AS ORC**: lagrar hello data i optimerad raden (ORC) kolumnformat.</span><span class="sxs-lookup"><span data-stu-id="5396a-139">**STORED AS ORC**: Stores hello data in optimized row columnar (ORC) format.</span></span> <span data-ttu-id="5396a-140">Detta är ett mycket optimerad och effektiv format för att lagra data med Hive.</span><span class="sxs-lookup"><span data-stu-id="5396a-140">This is a highly optimized and efficient format for storing Hive data.</span></span>
   * <span data-ttu-id="5396a-141">**INFOGA ÖVER... Välj**: väljer rader från hello **log4jLogs** tabellen som innehåller **[fel]**, och sedan infogningar hello data i hello **errorLogs** tabell.</span><span class="sxs-lookup"><span data-stu-id="5396a-141">**INSERT OVERWRITE ... SELECT**: Selects rows from hello **log4jLogs** table that contain **[ERROR]**, then inserts hello data into hello **errorLogs** table.</span></span>

     <span data-ttu-id="5396a-142">tooverify som endast rader som innehåller **[fel]** i kolumnen t4 var lagrade toohello **errorLogs** tabell använder hello följande instruktion tooreturn alla hello rader från **errorLogs**:</span><span class="sxs-lookup"><span data-stu-id="5396a-142">tooverify that only rows that contain **[ERROR]** in column t4 were stored toohello **errorLogs** table, use hello following statement tooreturn all hello rows from **errorLogs**:</span></span>

       <span data-ttu-id="5396a-143">Välj * från errorLogs;</span><span class="sxs-lookup"><span data-stu-id="5396a-143">SELECT * from errorLogs;</span></span>

     <span data-ttu-id="5396a-144">Tre raderna med data ska returneras, som innehåller alla **[fel]** i kolumnen t4.</span><span class="sxs-lookup"><span data-stu-id="5396a-144">Three rows of data should be returned, all containing **[ERROR]** in column t4.</span></span>

## <span data-ttu-id="5396a-145"><a id="summary"></a>Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="5396a-145"><a id="summary"></a>Summary</span></span>
<span data-ttu-id="5396a-146">Som du ser hello hello Hive kommandot ger ett enkelt sätt toointeractively köra Hive-frågor på ett HDInsight-kluster, övervaka hello jobbstatusen och hämta hello utdata.</span><span class="sxs-lookup"><span data-stu-id="5396a-146">As you can see, hello hello Hive command provides an easy way toointeractively run Hive queries on an HDInsight cluster, monitor hello job status, and retrieve hello output.</span></span>

## <span data-ttu-id="5396a-147"><a id="nextsteps"></a>Nästa steg</span><span class="sxs-lookup"><span data-stu-id="5396a-147"><a id="nextsteps"></a>Next steps</span></span>
<span data-ttu-id="5396a-148">Allmän information om Hive i HDInsight:</span><span class="sxs-lookup"><span data-stu-id="5396a-148">For general information about Hive in HDInsight:</span></span>

* [<span data-ttu-id="5396a-149">Använda Hive med Hadoop i HDInsight</span><span class="sxs-lookup"><span data-stu-id="5396a-149">Use Hive with Hadoop on HDInsight</span></span>](hdinsight-use-hive.md)

<span data-ttu-id="5396a-150">Information om andra sätt kan du arbeta med Hadoop i HDInsight:</span><span class="sxs-lookup"><span data-stu-id="5396a-150">For information about other ways you can work with Hadoop on HDInsight:</span></span>

* [<span data-ttu-id="5396a-151">Använda Pig med Hadoop i HDInsight</span><span class="sxs-lookup"><span data-stu-id="5396a-151">Use Pig with Hadoop on HDInsight</span></span>](hdinsight-use-pig.md)
* [<span data-ttu-id="5396a-152">Använda MapReduce med Hadoop i HDInsight</span><span class="sxs-lookup"><span data-stu-id="5396a-152">Use MapReduce with Hadoop on HDInsight</span></span>](hdinsight-use-mapreduce.md)

<span data-ttu-id="5396a-153">Om du använder Tez med Hive finns i följande dokument för felsökningsinformation hello:</span><span class="sxs-lookup"><span data-stu-id="5396a-153">If you are using Tez with Hive, see hello following documents for debugging information:</span></span>

* [<span data-ttu-id="5396a-154">Använda hello Tez UI på Windows-baserade HDInsight</span><span class="sxs-lookup"><span data-stu-id="5396a-154">Use hello Tez UI on Windows-based HDInsight</span></span>](hdinsight-debug-tez-ui.md)
* [<span data-ttu-id="5396a-155">Använd hello Ambari Tez vy på Linux-baserat HDInsight</span><span class="sxs-lookup"><span data-stu-id="5396a-155">Use hello Ambari Tez view on Linux-based HDInsight</span></span>](hdinsight-debug-ambari-tez-view.md)

[1]: ../HDInsight/hdinsight-hadoop-visual-studio-tools-get-started.md

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





[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md
[hdinsight-upload-data]: hdinsight-upload-data.md


[Powershell-install-configure]: /powershell/azureps-cmdlets-docs
[powershell-here-strings]: http://technet.microsoft.com/library/ee692792.aspx
