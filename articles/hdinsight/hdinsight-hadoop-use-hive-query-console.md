---
title: "aaaUse Hadoop Hive på hello frågan konsolen i HDInsight - Azure | Microsoft Docs"
description: "Lär dig hur toouse hello webbaserade konsolen frågan toorun Hive-frågor på ett HDInsight Hadoop-kluster från din webbläsare."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 5ae074b0-f55e-472d-94a7-005b0e79f779
ms.service: hdinsight
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 01/12/2017
ms.author: larryfr
ROBOTS: NOINDEX
ms.openlocfilehash: 621882082c9a07655d34b8dc980b8e47dd04b745
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="run-hive-queries-using-hello-query-console"></a><span data-ttu-id="f5300-103">Köra Hive-frågor med hello frågan konsolen</span><span class="sxs-lookup"><span data-stu-id="f5300-103">Run Hive queries using hello Query Console</span></span>
[!INCLUDE [hive-selector](../../includes/hdinsight-selector-use-hive.md)]

<span data-ttu-id="f5300-104">I den här artikeln får du lära dig hur toouse hello HDInsight frågan konsolen toorun Hive-frågor på ett HDInsight Hadoop-kluster från din webbläsare.</span><span class="sxs-lookup"><span data-stu-id="f5300-104">In this article, you will learn how toouse hello HDInsight Query Console toorun Hive queries on an HDInsight Hadoop cluster from your browser.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="f5300-105">Hej HDInsight frågan konsolen är bara tillgängligt på Windows-baserade HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="f5300-105">hello HDInsight Query Console is only available on Windows-based HDInsight clusters.</span></span> <span data-ttu-id="f5300-106">Linux är hello endast operativsystem på HDInsight version 3.4 eller senare.</span><span class="sxs-lookup"><span data-stu-id="f5300-106">Linux is hello only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="f5300-107">Mer information finns i [HDInsight-avveckling på Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="f5300-107">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>
>
> <span data-ttu-id="f5300-108">För HDInsight 3.4 eller större finns [köra Hive-frågor i Ambari Hive-vy](hdinsight-hadoop-use-hive-ambari-view.md) information om hur du kör Hive-frågor från en webbläsare.</span><span class="sxs-lookup"><span data-stu-id="f5300-108">For HDInsight 3.4 or greater, see [Run Hive queries in Ambari Hive View](hdinsight-hadoop-use-hive-ambari-view.md) for information on running Hive queries from a web browser.</span></span>

## <span data-ttu-id="f5300-109"><a id="prereq"></a>Förhandskrav</span><span class="sxs-lookup"><span data-stu-id="f5300-109"><a id="prereq"></a>Prerequisites</span></span>
<span data-ttu-id="f5300-110">toocomplete hello stegen i den här artikeln, behöver du hello följande.</span><span class="sxs-lookup"><span data-stu-id="f5300-110">toocomplete hello steps in this article, you will need hello following.</span></span>

* <span data-ttu-id="f5300-111">Ett Windows-baserade HDInsight Hadoop-kluster</span><span class="sxs-lookup"><span data-stu-id="f5300-111">A Windows-based HDInsight Hadoop cluster</span></span>
* <span data-ttu-id="f5300-112">En modern webbläsare</span><span class="sxs-lookup"><span data-stu-id="f5300-112">A modern web browser</span></span>

## <span data-ttu-id="f5300-113"><a id="run"></a>Köra Hive-frågor med hello frågan konsolen</span><span class="sxs-lookup"><span data-stu-id="f5300-113"><a id="run"></a> Run Hive queries using hello Query Console</span></span>
1. <span data-ttu-id="f5300-114">Öppna en webbläsare och gå för**https://CLUSTERNAME.azurehdinsight.net**, där **KLUSTERNAMN** är hello namnet på ditt HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="f5300-114">Open a web browser and navigate too**https://CLUSTERNAME.azurehdinsight.net**, where **CLUSTERNAME** is hello name of your HDInsight cluster.</span></span> <span data-ttu-id="f5300-115">Om du uppmanas ange hello användarnamn och lösenord som du använde när du skapade hello.</span><span class="sxs-lookup"><span data-stu-id="f5300-115">If prompted, enter hello user name and password that you used when you created hello cluster.</span></span>
2. <span data-ttu-id="f5300-116">Hello länkar hello överst på hello sidan, Välj **Hive-redigeraren**.</span><span class="sxs-lookup"><span data-stu-id="f5300-116">From hello links at hello top of hello page, select **Hive Editor**.</span></span> <span data-ttu-id="f5300-117">Detta visar ett formulär som kan använda tooenter hello HiveQL-instruktioner som du vill toorun i hello HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="f5300-117">This displays a form that can be used tooenter hello HiveQL statements that you want toorun in hello HDInsight cluster.</span></span>

    ![hello hive-redigeraren](./media/hdinsight-hadoop-use-hive-query-console/queryconsole.png)

    <span data-ttu-id="f5300-119">Ersätt texten hello `Select * from hivesampletable` med hello följande HiveQL-instruktioner:</span><span class="sxs-lookup"><span data-stu-id="f5300-119">Replace hello text `Select * from hivesampletable` with hello following HiveQL statements:</span></span>

        set hive.execution.engine=tez;
        DROP TABLE log4jLogs;
        CREATE EXTERNAL TABLE log4jLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string)
        ROW FORMAT DELIMITED FIELDS TERMINATED BY ' '
        STORED AS TEXTFILE LOCATION 'wasb:///example/data/';
        SELECT t4 AS sev, COUNT(*) AS count FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log' GROUP BY t4;

    <span data-ttu-id="f5300-120">Dessa instruktioner utför hello följande åtgärder:</span><span class="sxs-lookup"><span data-stu-id="f5300-120">These statements perform hello following actions:</span></span>

   * <span data-ttu-id="f5300-121">**DROP TABLE**: tar bort hello och hello data om hello tabellen finns redan.</span><span class="sxs-lookup"><span data-stu-id="f5300-121">**DROP TABLE**: Deletes hello table and hello data file if hello table already exists.</span></span>
   * <span data-ttu-id="f5300-122">**Skapa extern tabell**: skapar en ny ”externa” tabell i Hive.</span><span class="sxs-lookup"><span data-stu-id="f5300-122">**CREATE EXTERNAL TABLE**: Creates a new 'external' table in Hive.</span></span> <span data-ttu-id="f5300-123">Externa tabeller lagra endast hello tabelldefinition i Hive; hello data finns kvar i hello ursprungliga plats.</span><span class="sxs-lookup"><span data-stu-id="f5300-123">External tables store only hello table definition in Hive; hello data is left in hello original location.</span></span>

     > [!NOTE]
     > <span data-ttu-id="f5300-124">Externa tabeller ska användas när du förväntar dig hello underliggande data toobe uppdateras av en extern källa (till exempel en automatisk överföring av data) eller av en annan MapReduce-åtgärd, men du vill använda Hive frågor toouse hello senaste data.</span><span class="sxs-lookup"><span data-stu-id="f5300-124">External tables should be used when you expect hello underlying data toobe updated by an external source (such as an automated data upload process) or by another MapReduce operation, but you always want Hive queries toouse hello latest data.</span></span>
     >
     > <span data-ttu-id="f5300-125">Släppa en extern tabell har **inte** ta bort data hello, bara hello tabelldefinitionen.</span><span class="sxs-lookup"><span data-stu-id="f5300-125">Dropping an external table does **not** delete hello data, only hello table definition.</span></span>
     >
     >
   * <span data-ttu-id="f5300-126">**RADEN FORMAT**: talar om Hive hur hello data formateras.</span><span class="sxs-lookup"><span data-stu-id="f5300-126">**ROW FORMAT**: Tells Hive how hello data is formatted.</span></span> <span data-ttu-id="f5300-127">I det här fallet avgränsas hello fälten i varje logg med ett blanksteg.</span><span class="sxs-lookup"><span data-stu-id="f5300-127">In this case, hello fields in each log are separated by a space.</span></span>
   * <span data-ttu-id="f5300-128">**LAGRAS AS TEXTFILE plats**: talar om Hive där hello data är lagras (hello exempel/datakatalog) och som den lagras som text</span><span class="sxs-lookup"><span data-stu-id="f5300-128">**STORED AS TEXTFILE LOCATION**: Tells Hive where hello data is stored (hello example/data directory) and that it is stored as text</span></span>
   * <span data-ttu-id="f5300-129">**Välj**: Välj en uppräkning av alla rader där kolumnen **t4** innehåller hello värde **[fel]**.</span><span class="sxs-lookup"><span data-stu-id="f5300-129">**SELECT**: Select a count of all rows where column **t4** contain hello value **[ERROR]**.</span></span> <span data-ttu-id="f5300-130">Detta bör returnera ett värde av **3** eftersom det finns tre rader som innehåller det här värdet.</span><span class="sxs-lookup"><span data-stu-id="f5300-130">This should return a value of **3** because there are three rows that contain this value.</span></span>
   * <span data-ttu-id="f5300-131">**INPUT__FILE__NAME som '%.log'** -talar om Hive som vi ska bara returnera data från filer som slutar på. log.</span><span class="sxs-lookup"><span data-stu-id="f5300-131">**INPUT__FILE__NAME LIKE '%.log'** - Tells Hive that we should only return data from files ending in .log.</span></span> <span data-ttu-id="f5300-132">Detta begränsar hello sökning toohello sample.log-fil som innehåller hello data och håller den från att returnera data från andra exempel filer som inte matchar hello schemat som vi har definierat.</span><span class="sxs-lookup"><span data-stu-id="f5300-132">This restricts hello search toohello sample.log file that contains hello data, and keeps it from returning data from other example data files that do not match hello schema we defined.</span></span>
3. <span data-ttu-id="f5300-133">Klicka på **skicka**.</span><span class="sxs-lookup"><span data-stu-id="f5300-133">Click **Submit**.</span></span> <span data-ttu-id="f5300-134">Hej **jobbet Session** på hello längst ned på sidan hello ska visa information om hello jobb.</span><span class="sxs-lookup"><span data-stu-id="f5300-134">hello **Job Session** at hello bottom of hello page should display details for hello job.</span></span>
4. <span data-ttu-id="f5300-135">När hello **Status** fältet ändringar för**slutförd**väljer **visa information om** för hello jobbet.</span><span class="sxs-lookup"><span data-stu-id="f5300-135">When hello **Status** field changes too**Completed**, select **View Details** for hello job.</span></span> <span data-ttu-id="f5300-136">Hello information på sidan hello **Jobbutdata** innehåller `[ERROR]    3`.</span><span class="sxs-lookup"><span data-stu-id="f5300-136">On hello details page, hello **Job Output** contains `[ERROR]    3`.</span></span> <span data-ttu-id="f5300-137">Du kan använda hello **hämta** knappen under det här fältet toodownload en fil som innehåller hello utdata för hello jobb.</span><span class="sxs-lookup"><span data-stu-id="f5300-137">You can use hello **Download** button under this field toodownload a file that contains hello output of hello job.</span></span>

## <span data-ttu-id="f5300-138"><a id="summary"></a>Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="f5300-138"><a id="summary"></a>Summary</span></span>
<span data-ttu-id="f5300-139">Som du ser hello frågan konsolen ger ett enkelt sätt toorun Hive-frågor i ett HDInsight-kluster, övervaka hello jobbstatus och hämta hello utdata.</span><span class="sxs-lookup"><span data-stu-id="f5300-139">As you can see, hello Query Console provides an easy way toorun Hive queries in an HDInsight cluster, monitor hello job status, and retrieve hello output.</span></span>

<span data-ttu-id="f5300-140">toolearn mer information om hur du använder Hive-fråga konsolen toorun Hive-jobb, Välj **komma igång** överst hello i hello frågan konsolen, sedan använda hello-exempel som tillhandahålls.</span><span class="sxs-lookup"><span data-stu-id="f5300-140">toolearn more about using Hive Query Console toorun Hive jobs, select **Getting Started** at hello top of hello Query Console, then use hello samples that are provided.</span></span> <span data-ttu-id="f5300-141">Varje prov går igenom hello processen med att använda Hive tooanalyze data, inklusive beskrivningar av hello HiveQL-instruktioner som används i hello exempel.</span><span class="sxs-lookup"><span data-stu-id="f5300-141">Each sample walks through hello process of using Hive tooanalyze data, including explanations about hello HiveQL statements used in hello sample.</span></span>

## <span data-ttu-id="f5300-142"><a id="nextsteps"></a>Nästa steg</span><span class="sxs-lookup"><span data-stu-id="f5300-142"><a id="nextsteps"></a>Next steps</span></span>
<span data-ttu-id="f5300-143">Allmän information om Hive i HDInsight:</span><span class="sxs-lookup"><span data-stu-id="f5300-143">For general information about Hive in HDInsight:</span></span>

* [<span data-ttu-id="f5300-144">Använda Hive med Hadoop i HDInsight</span><span class="sxs-lookup"><span data-stu-id="f5300-144">Use Hive with Hadoop on HDInsight</span></span>](hdinsight-use-hive.md)

<span data-ttu-id="f5300-145">Information om andra sätt kan du arbeta med Hadoop i HDInsight:</span><span class="sxs-lookup"><span data-stu-id="f5300-145">For information about other ways you can work with Hadoop on HDInsight:</span></span>

* [<span data-ttu-id="f5300-146">Använda Pig med Hadoop i HDInsight</span><span class="sxs-lookup"><span data-stu-id="f5300-146">Use Pig with Hadoop on HDInsight</span></span>](hdinsight-use-pig.md)
* [<span data-ttu-id="f5300-147">Använda MapReduce med Hadoop i HDInsight</span><span class="sxs-lookup"><span data-stu-id="f5300-147">Use MapReduce with Hadoop on HDInsight</span></span>](hdinsight-use-mapreduce.md)

<span data-ttu-id="f5300-148">Om du använder Tez med Hive finns i följande dokument för felsökningsinformation hello:</span><span class="sxs-lookup"><span data-stu-id="f5300-148">If you are using Tez with Hive, see hello following documents for debugging information:</span></span>

* [<span data-ttu-id="f5300-149">Använda hello Tez UI på Windows-baserade HDInsight</span><span class="sxs-lookup"><span data-stu-id="f5300-149">Use hello Tez UI on Windows-based HDInsight</span></span>](hdinsight-debug-tez-ui.md)
* [<span data-ttu-id="f5300-150">Använd hello Ambari Tez vy på Linux-baserat HDInsight</span><span class="sxs-lookup"><span data-stu-id="f5300-150">Use hello Ambari Tez view on Linux-based HDInsight</span></span>](hdinsight-debug-ambari-tez-view.md)

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



[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md

[hdinsight-provision]: hdinsight-hadoop-provision-linux-clusters.md
[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md

[Powershell-install-configure]: /powershell/azureps-cmdlets-docs
[powershell-here-strings]: http://technet.microsoft.com/library/ee692792.aspx


[img-hdi-hive-powershell-output]: ./media/hdinsight-use-hive/HDI.Hive.PowerShell.Output.png
