---
title: aaaUse Ambari Views toowork med Hive i HDInsight (Hadoop) - Azure | Microsoft Docs
description: "Lär dig hur toouse hello Hive-vy från dina webb webbläsare toosubmit Hive-frågor. hello Hive-vy är en del av hello Ambari-Webbgränssnittet medföljer ditt Linux-baserade HDInsight-kluster."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 1abe9104-f4b2-41b9-9161-abbc43de8294
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 07/31/2017
ms.author: larryfr
ms.openlocfilehash: f9a77b652e70d34a0ff9165fbb8c2e16d3401ae0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-hive-view-with-hadoop-in-hdinsight"></a><span data-ttu-id="aa267-104">Använda hello Hive-vy med Hadoop i HDInsight</span><span class="sxs-lookup"><span data-stu-id="aa267-104">Use hello Hive View with Hadoop in HDInsight</span></span>

[!INCLUDE [hive-selector](../../includes/hdinsight-selector-use-hive.md)]

<span data-ttu-id="aa267-105">Lär dig hur toorun Hive-frågor med Ambari Hive-vy.</span><span class="sxs-lookup"><span data-stu-id="aa267-105">Learn how toorun Hive queries using Ambari Hive View.</span></span> <span data-ttu-id="aa267-106">Ambari är ett hantering och övervakning verktyg som medföljer Linux-baserade HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="aa267-106">Ambari is a management and monitoring utility provided with Linux-based HDInsight clusters.</span></span> <span data-ttu-id="aa267-107">En av hello-funktioner som tillhandahålls via Ambari är ett Webbgränssnitt som kan använda toorun Hive-frågor.</span><span class="sxs-lookup"><span data-stu-id="aa267-107">One of hello features provided through Ambari is a Web UI that can be used toorun Hive queries.</span></span>

> [!NOTE]
> <span data-ttu-id="aa267-108">Ambari har många funktioner som inte beskrivs i det här dokumentet.</span><span class="sxs-lookup"><span data-stu-id="aa267-108">Ambari has many capabilities that are not discussed in this document.</span></span> <span data-ttu-id="aa267-109">Mer information finns i [hantera HDInsight-kluster med hjälp av hello Ambari-Webbgränssnittet](hdinsight-hadoop-manage-ambari.md).</span><span class="sxs-lookup"><span data-stu-id="aa267-109">For more information, see [Manage HDInsight clusters by using hello Ambari Web UI](hdinsight-hadoop-manage-ambari.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="aa267-110">Krav</span><span class="sxs-lookup"><span data-stu-id="aa267-110">Prerequisites</span></span>

* <span data-ttu-id="aa267-111">Ett Linux-baserat HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="aa267-111">A Linux-based HDInsight cluster.</span></span> <span data-ttu-id="aa267-112">Information om hur du skapar klustret finns [komma igång med Linux-baserade HDInsight](hdinsight-hadoop-linux-tutorial-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="aa267-112">For information on creating cluster, see [Get started with Linux-based HDInsight](hdinsight-hadoop-linux-tutorial-get-started.md).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="aa267-113">hello stegen i det här dokumentet kräver ett HDInsight-kluster som använder Linux.</span><span class="sxs-lookup"><span data-stu-id="aa267-113">hello steps in this document require an HDInsight cluster that uses Linux.</span></span> <span data-ttu-id="aa267-114">Linux är hello endast operativsystem på HDInsight version 3.4 eller senare.</span><span class="sxs-lookup"><span data-stu-id="aa267-114">Linux is hello only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="aa267-115">Mer information finns i [HDInsight-avveckling på Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="aa267-115">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

## <a name="open-hello-hive-view"></a><span data-ttu-id="aa267-116">Öppna hello Hive-vyn</span><span class="sxs-lookup"><span data-stu-id="aa267-116">Open hello Hive view</span></span>

<span data-ttu-id="aa267-117">Du kan Ambari-vyer från hello Azure-portalen; Välj ditt HDInsight-kluster och välj sedan **Ambari Views** från hello **snabblänkar** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="aa267-117">You can Ambari Views from hello Azure portal; select your HDInsight cluster and then select **Ambari Views** from hello **Quick Links** section.</span></span>

![Snabblänkar hello-portalen](./media/hdinsight-hadoop-use-hive-ambari-view/quicklinks.png)

<span data-ttu-id="aa267-119">Välj hello hello listan vyer __Hive-vy__.</span><span class="sxs-lookup"><span data-stu-id="aa267-119">From hello list of views, select hello __Hive View__.</span></span>

![hello Hive-vy markerat](./media/hdinsight-hadoop-use-hive-ambari-view/select-hive-view.png)

> [!NOTE]
> <span data-ttu-id="aa267-121">När åt Ambari, kan du ange tooauthenticate toohello plats.</span><span class="sxs-lookup"><span data-stu-id="aa267-121">When accessing Ambari, you are prompted tooauthenticate toohello site.</span></span> <span data-ttu-id="aa267-122">Ange Hej administratör (standard `admin`) konto och lösenord som du använde när du skapar hello klustret.</span><span class="sxs-lookup"><span data-stu-id="aa267-122">Enter hello admin (default `admin`) account name and password you used when creating hello cluster.</span></span>

<span data-ttu-id="aa267-123">Du bör se en sida liknande toohello följande bild:</span><span class="sxs-lookup"><span data-stu-id="aa267-123">You should see a page similar toohello following image:</span></span>

![Bild av hello frågan kalkylblad för hello Hive-vyn](./media/hdinsight-hadoop-use-hive-ambari-view/ambari-hive-view.png)

## <span data-ttu-id="aa267-125"><a name="hivequery"></a>Köra en fråga</span><span class="sxs-lookup"><span data-stu-id="aa267-125"><a name="hivequery"></a>Run a query</span></span>

<span data-ttu-id="aa267-126">toorun en hive-fråga använda hello följande från hello Hive-vyn.</span><span class="sxs-lookup"><span data-stu-id="aa267-126">toorun a hive query, use hello following steps from hello Hive view.</span></span>

1. <span data-ttu-id="aa267-127">Från hello __frågan__ och klistra in följande HiveQL-instruktioner i kalkylbladet med hello hello:</span><span class="sxs-lookup"><span data-stu-id="aa267-127">From hello __Query__ tab, paste hello following HiveQL statements into hello worksheet:</span></span>

    ```hiveql
    DROP TABLE log4jLogs;
    CREATE EXTERNAL TABLE log4jLogs(t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string)
    ROW FORMAT DELIMITED FIELDS TERMINATED BY ' '
    STORED AS TEXTFILE LOCATION '/example/data/';
    SELECT t4 AS sev, COUNT(*) AS cnt FROM log4jLogs WHERE t4 = '[ERROR]' GROUP BY t4;
    ```

    <span data-ttu-id="aa267-128">Dessa instruktioner utför hello följande åtgärder:</span><span class="sxs-lookup"><span data-stu-id="aa267-128">These statements perform hello following actions:</span></span>

   * <span data-ttu-id="aa267-129">`DROP TABLE`-Tar bort hello tabell och hello datafilen hello tabellen redan finns.</span><span class="sxs-lookup"><span data-stu-id="aa267-129">`DROP TABLE` - Deletes hello table and hello data file, in case hello table already exists.</span></span>

   * <span data-ttu-id="aa267-130">`CREATE EXTERNAL TABLE`– Skapar en ny ”externa” tabell i Hive.</span><span class="sxs-lookup"><span data-stu-id="aa267-130">`CREATE EXTERNAL TABLE` - Creates a new "external" table in Hive.</span></span>
   <span data-ttu-id="aa267-131">Externa tabeller lagra endast hello tabelldefinition i Hive.</span><span class="sxs-lookup"><span data-stu-id="aa267-131">External tables store only hello table definition in Hive.</span></span> <span data-ttu-id="aa267-132">hello data finns kvar i hello ursprungliga plats.</span><span class="sxs-lookup"><span data-stu-id="aa267-132">hello data is left in hello original location.</span></span>

   * <span data-ttu-id="aa267-133">`ROW FORMAT`-Hur hello data formateras.</span><span class="sxs-lookup"><span data-stu-id="aa267-133">`ROW FORMAT` - How hello data is formatted.</span></span> <span data-ttu-id="aa267-134">I det här fallet avgränsas hello fälten i varje logg med ett blanksteg.</span><span class="sxs-lookup"><span data-stu-id="aa267-134">In this case, hello fields in each log are separated by a space.</span></span>

   * <span data-ttu-id="aa267-135">`STORED AS TEXTFILE LOCATION`-Där hello data lagras och som det lagras som text.</span><span class="sxs-lookup"><span data-stu-id="aa267-135">`STORED AS TEXTFILE LOCATION` - Where hello data is stored, and that it is stored as text.</span></span>

   * <span data-ttu-id="aa267-136">`SELECT`-Väljer en uppräkning av alla rader där kolumnen t4 innehåller hello värde [fel].</span><span class="sxs-lookup"><span data-stu-id="aa267-136">`SELECT` - Selects a count of all rows where column t4 contains hello value [ERROR].</span></span>

     > [!NOTE]
     > <span data-ttu-id="aa267-137">Externa tabeller ska användas när du förväntar dig hello underliggande data toobe uppdateras av en extern källa.</span><span class="sxs-lookup"><span data-stu-id="aa267-137">External tables should be used when you expect hello underlying data toobe updated by an external source.</span></span> <span data-ttu-id="aa267-138">Till exempel en automatiserad dataöverföringen process, eller av en annan MapReduce-åtgärd.</span><span class="sxs-lookup"><span data-stu-id="aa267-138">For example, an automated data upload process, or by another MapReduce operation.</span></span> <span data-ttu-id="aa267-139">Släppa en extern tabell har *inte* ta bort data hello, bara hello tabelldefinitionen.</span><span class="sxs-lookup"><span data-stu-id="aa267-139">Dropping an external table does *not* delete hello data, only hello table definition.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="aa267-140">Lämna hello __databasen__ valet __standard__.</span><span class="sxs-lookup"><span data-stu-id="aa267-140">Leave hello __Database__ selection at __default__.</span></span> <span data-ttu-id="aa267-141">hello exemplen i det här dokumentet används hello standarddatabasen som ingår i HDInsight.</span><span class="sxs-lookup"><span data-stu-id="aa267-141">hello examples in this document use hello default database included with HDInsight.</span></span>

2. <span data-ttu-id="aa267-142">toostart hello fråga, Använd hello **Execute** nedan hello kalkylblad.</span><span class="sxs-lookup"><span data-stu-id="aa267-142">toostart hello query, use hello **Execute** button below hello worksheet.</span></span> <span data-ttu-id="aa267-143">Den blir orange och hello textändringar för**stoppa**.</span><span class="sxs-lookup"><span data-stu-id="aa267-143">It turns orange and hello text changes too**Stop**.</span></span>

3. <span data-ttu-id="aa267-144">När hello frågan har slutförts hello **resultat** visar hello resultaten av hello igen.</span><span class="sxs-lookup"><span data-stu-id="aa267-144">Once hello query has finished, hello **Results** tab displays hello results of hello operation.</span></span> <span data-ttu-id="aa267-145">hello efter texten är hello resultatet av hello fråga:</span><span class="sxs-lookup"><span data-stu-id="aa267-145">hello following text is hello result of hello query:</span></span>

        sev       cnt
        [ERROR]   3

    <span data-ttu-id="aa267-146">Hej **loggar** flik kan vara används tooview hello loggningsinformation som skapats av hello-jobbet.</span><span class="sxs-lookup"><span data-stu-id="aa267-146">hello **Logs** tab can be used tooview hello logging information created by hello job.</span></span>

   > [!TIP]
   > <span data-ttu-id="aa267-147">Hej **spara resultaten** listrutan dialogrutan i hello övre vänstra hörnet hello **Frågeprocessresultat** avsnitt kan du toodownload eller spara resultaten.</span><span class="sxs-lookup"><span data-stu-id="aa267-147">hello **Save results** drop-down dialog in hello upper left of hello **Query Process Results** section allows you toodownload or save results.</span></span>

4. <span data-ttu-id="aa267-148">Välj hello fyra första raderna i den här frågan väljer **Execute**.</span><span class="sxs-lookup"><span data-stu-id="aa267-148">Select hello first four lines of this query, then select **Execute**.</span></span> <span data-ttu-id="aa267-149">Observera att det finns inga resultat när hello jobbet har slutförts.</span><span class="sxs-lookup"><span data-stu-id="aa267-149">Notice that there are no results when hello job completes.</span></span> <span data-ttu-id="aa267-150">Med hjälp av hello **Execute** knappen när det ingår i hello frågan markeras bara körs hello markerade rapporter.</span><span class="sxs-lookup"><span data-stu-id="aa267-150">Using hello **Execute** button when part of hello query is selected only runs hello selected statements.</span></span> <span data-ttu-id="aa267-151">I det här fallet skickat hello markeringen hello sista instruktionen som hämtar rader från hello tabell.</span><span class="sxs-lookup"><span data-stu-id="aa267-151">In this case, hello selection didn't include hello final statement that retrieves rows from hello table.</span></span> <span data-ttu-id="aa267-152">Om du väljer bara den raden och använda **kör**, bör du se hello förväntat resultat.</span><span class="sxs-lookup"><span data-stu-id="aa267-152">If you select just that line and use **Execute**, you should see hello expected results.</span></span>

5. <span data-ttu-id="aa267-153">tooadd ett kalkylblad använder hello **nytt kalkylblad** knappen längst ned hello hello **frågeredigeraren**.</span><span class="sxs-lookup"><span data-stu-id="aa267-153">tooadd a worksheet, use hello **New Worksheet** button at hello bottom of hello **Query Editor**.</span></span> <span data-ttu-id="aa267-154">Ange hello följande HiveQL-instruktioner i hello nytt kalkylblad:</span><span class="sxs-lookup"><span data-stu-id="aa267-154">In hello new worksheet, enter hello following HiveQL statements:</span></span>

    ```hiveql
    CREATE TABLE IF NOT EXISTS errorLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string) STORED AS ORC;
    INSERT OVERWRITE TABLE errorLogs SELECT t1, t2, t3, t4, t5, t6, t7 FROM log4jLogs WHERE t4 = '[ERROR]';
    ```

  <span data-ttu-id="aa267-155">Dessa instruktioner utför hello följande åtgärder:</span><span class="sxs-lookup"><span data-stu-id="aa267-155">These statements perform hello following actions:</span></span>

   * <span data-ttu-id="aa267-156">**Skapa tabell om inte finns** -skapar en tabell om den inte redan finns.</span><span class="sxs-lookup"><span data-stu-id="aa267-156">**CREATE TABLE IF NOT EXISTS** - Creates a table, if it does not already exist.</span></span> <span data-ttu-id="aa267-157">Eftersom hello **externa** nyckelordet används inte, en intern tabell skapas.</span><span class="sxs-lookup"><span data-stu-id="aa267-157">Since hello **EXTERNAL** keyword is not used, an internal table is created.</span></span> <span data-ttu-id="aa267-158">En intern tabell lagras i datalagret för hello Hive och hanteras helt av Hive.</span><span class="sxs-lookup"><span data-stu-id="aa267-158">An internal table is stored in hello Hive data warehouse and is managed completely by Hive.</span></span> <span data-ttu-id="aa267-159">Till skillnad från externa tabeller kan släppa en intern tabell tar bort hello samt underliggande data.</span><span class="sxs-lookup"><span data-stu-id="aa267-159">Unlike external tables, dropping an internal table deletes hello underlying data as well.</span></span>

   * <span data-ttu-id="aa267-160">**LAGRAS AS ORC** -lagrar hello data i optimerad raden kolumner (ORC)-format.</span><span class="sxs-lookup"><span data-stu-id="aa267-160">**STORED AS ORC** - Stores hello data in Optimized Row Columnar (ORC) format.</span></span> <span data-ttu-id="aa267-161">ORC är ett mycket optimerad och effektiv format för att lagra data med Hive.</span><span class="sxs-lookup"><span data-stu-id="aa267-161">ORC is a highly optimized and efficient format for storing Hive data.</span></span>

   * <span data-ttu-id="aa267-162">**INFOGA ÖVER... Välj** -väljer rader från hello **log4jLogs** tabellen som innehåller `[ERROR]`, och sedan infogningar hello data i hello **errorLogs** tabell.</span><span class="sxs-lookup"><span data-stu-id="aa267-162">**INSERT OVERWRITE ... SELECT** - Selects rows from hello **log4jLogs** table that contain `[ERROR]`, and then inserts hello data into hello **errorLogs** table.</span></span>

     <span data-ttu-id="aa267-163">Använd hello **Execute** knappen toorun den här frågan.</span><span class="sxs-lookup"><span data-stu-id="aa267-163">Use hello **Execute** button toorun this query.</span></span> <span data-ttu-id="aa267-164">Hej **resultat** fliken innehåller inte någon information när hello frågan returnerar noll rader.</span><span class="sxs-lookup"><span data-stu-id="aa267-164">hello **Results** tab does not contain any information when hello query returns zero rows.</span></span> <span data-ttu-id="aa267-165">hello status ska visa **lyckades** när hello frågan har slutförts.</span><span class="sxs-lookup"><span data-stu-id="aa267-165">hello status should show as **SUCCEEDED** once hello query completes.</span></span>

### <a name="visual-explain"></a><span data-ttu-id="aa267-166">Visual förklarar</span><span class="sxs-lookup"><span data-stu-id="aa267-166">Visual explain</span></span>

<span data-ttu-id="aa267-167">toodisplay en visualisering av hello frågeplan, Välj hello **Visual förklarar** tabbtangenten hello kalkylblad.</span><span class="sxs-lookup"><span data-stu-id="aa267-167">toodisplay a visualization of hello query plan, select hello **Visual Explain** tab below hello worksheet.</span></span>

<span data-ttu-id="aa267-168">Hej **Visual förklarar** vy över hello frågan kan vara lättare att förstå hello flödet av komplexa frågor.</span><span class="sxs-lookup"><span data-stu-id="aa267-168">hello **Visual Explain** view of hello query can be helpful in understanding hello flow of complex queries.</span></span> <span data-ttu-id="aa267-169">Du kan visa en textrepresentation motsvarigheten till den här vyn genom att använda hello **förklara** knapp i hello frågeredigeraren.</span><span class="sxs-lookup"><span data-stu-id="aa267-169">You can view a textual equivalent of this view by using hello **Explain** button in hello Query Editor.</span></span>

### <a name="tez-ui"></a><span data-ttu-id="aa267-170">Tez-Gränssnittet</span><span class="sxs-lookup"><span data-stu-id="aa267-170">Tez UI</span></span>

<span data-ttu-id="aa267-171">toodisplay hello Tez UI hello frågan, Välj hello **Tez** tabbtangenten hello kalkylblad.</span><span class="sxs-lookup"><span data-stu-id="aa267-171">toodisplay hello Tez UI for hello query, select hello **Tez** tab below hello worksheet.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="aa267-172">Tez är används inte tooresolve alla frågor.</span><span class="sxs-lookup"><span data-stu-id="aa267-172">Tez is not used tooresolve all queries.</span></span> <span data-ttu-id="aa267-173">Många frågor kan lösas utan att använda Tez.</span><span class="sxs-lookup"><span data-stu-id="aa267-173">Many queries can be resolved without using Tez.</span></span> 

<span data-ttu-id="aa267-174">Om Tez var används tooresolve hello fråga, hello dirigeras acykliska diagram (DAG) visas.</span><span class="sxs-lookup"><span data-stu-id="aa267-174">If Tez was used tooresolve hello query, hello Directed Acyclic Graph (DAG) is displayed.</span></span> <span data-ttu-id="aa267-175">Om du vill tooview hello DAG för frågor som du har kört i hello senaste eller felsöka hello Tez-processen, Använd hello [Tez visa](hdinsight-debug-ambari-tez-view.md) i stället.</span><span class="sxs-lookup"><span data-stu-id="aa267-175">If you want tooview hello DAG for queries you've ran in hello past, or debug hello Tez process, use hello [Tez View](hdinsight-debug-ambari-tez-view.md) instead.</span></span>

## <a name="view-job-history"></a><span data-ttu-id="aa267-176">Visa jobbhistorik</span><span class="sxs-lookup"><span data-stu-id="aa267-176">View job history</span></span>

<span data-ttu-id="aa267-177">Hej __jobb__ visar en historik över Hive-frågor.</span><span class="sxs-lookup"><span data-stu-id="aa267-177">hello __Jobs__ tab displays a history of Hive queries.</span></span>

![Bild av hello jobbhistorik](./media/hdinsight-hadoop-use-hive-ambari-view/job-history.png)

## <a name="database-tables"></a><span data-ttu-id="aa267-179">Databastabeller</span><span class="sxs-lookup"><span data-stu-id="aa267-179">Database tables</span></span>

<span data-ttu-id="aa267-180">Du kan använda hello __tabeller__ fliken toowork med tabeller i en databas för Hive.</span><span class="sxs-lookup"><span data-stu-id="aa267-180">You can use hello __Tables__ tab toowork with tables within a Hive database.</span></span>

![Bild av hello tabeller fliken](./media/hdinsight-hadoop-use-hive-ambari-view/tables.png)

## <a name="saved-queries"></a><span data-ttu-id="aa267-182">Sparade frågor</span><span class="sxs-lookup"><span data-stu-id="aa267-182">Saved queries</span></span>

<span data-ttu-id="aa267-183">Du kan du kan också spara frågor från hello frågan på fliken.</span><span class="sxs-lookup"><span data-stu-id="aa267-183">From hello Query tab, you can optionally save queries.</span></span> <span data-ttu-id="aa267-184">När du sparat du kan återanvända hello frågan från hello __sparade frågor__ fliken.</span><span class="sxs-lookup"><span data-stu-id="aa267-184">Once saved, you can reuse hello query from hello __Saved Queries__ tab.</span></span>

![Bild av fliken sparade frågor](./media/hdinsight-hadoop-use-hive-ambari-view/saved-queries.png)

## <a name="user-defined-functions"></a><span data-ttu-id="aa267-186">Användardefinierade funktioner</span><span class="sxs-lookup"><span data-stu-id="aa267-186">User-defined functions</span></span>

<span data-ttu-id="aa267-187">Hive utökas även via användardefinierade funktioner (UDF).</span><span class="sxs-lookup"><span data-stu-id="aa267-187">Hive can also be extended through user-defined functions (UDF).</span></span> <span data-ttu-id="aa267-188">En UDF kan du tooimplement funktioner eller som inte är enkelt modelleras i HiveQL.</span><span class="sxs-lookup"><span data-stu-id="aa267-188">A UDF allows you tooimplement functionality or logic that isn't easily modeled in HiveQL.</span></span>

<span data-ttu-id="aa267-189">hello UDF fliken hello överst i hello Hive-vy kan du toodeclare och spara en uppsättning UDF: er.</span><span class="sxs-lookup"><span data-stu-id="aa267-189">hello UDF tab at hello top of hello Hive View allows you toodeclare and save a set of UDFs.</span></span> <span data-ttu-id="aa267-190">Dessa UDF: er kan användas med hello **frågeredigeraren**.</span><span class="sxs-lookup"><span data-stu-id="aa267-190">These UDFs can be used with hello **Query Editor**.</span></span>

![Bild av UDF-fliken](./media/hdinsight-hadoop-use-hive-ambari-view/user-defined-functions.png)

<span data-ttu-id="aa267-192">När du har lagt till UDF-toohello Hive-vy, en **Infoga UDF: er** visas knappen längst ned hello hello **frågeredigeraren**.</span><span class="sxs-lookup"><span data-stu-id="aa267-192">Once you have added a UDF toohello Hive View, an **Insert udfs** button appears at hello bottom of hello **Query Editor**.</span></span> <span data-ttu-id="aa267-193">Om du markerar den här posten visas listrutan för hello UDF: er som definierats i hello Hive-vy.</span><span class="sxs-lookup"><span data-stu-id="aa267-193">Selecting this entry displays a drop-down list of hello UDFs defined in hello Hive View.</span></span> <span data-ttu-id="aa267-194">Markerar en UDF läggs HiveQL-instruktioner tooyour frågan tooenable hello UDF.</span><span class="sxs-lookup"><span data-stu-id="aa267-194">Selecting a UDF adds HiveQL statements tooyour query tooenable hello UDF.</span></span>

<span data-ttu-id="aa267-195">Till exempel om du har definierat en UDF med hello följande egenskaper:</span><span class="sxs-lookup"><span data-stu-id="aa267-195">For example, if you have defined a UDF with hello following properties:</span></span>

* <span data-ttu-id="aa267-196">Resursnamnet: myudfs</span><span class="sxs-lookup"><span data-stu-id="aa267-196">Resource name: myudfs</span></span>

* <span data-ttu-id="aa267-197">Resursens sökväg: /myudfs.jar</span><span class="sxs-lookup"><span data-stu-id="aa267-197">Resource path: /myudfs.jar</span></span>

* <span data-ttu-id="aa267-198">UDF-namn: myawesomeudf</span><span class="sxs-lookup"><span data-stu-id="aa267-198">UDF name: myawesomeudf</span></span>

* <span data-ttu-id="aa267-199">Klassnamn för UDF: com.myudfs.Awesome</span><span class="sxs-lookup"><span data-stu-id="aa267-199">UDF class name: com.myudfs.Awesome</span></span>

<span data-ttu-id="aa267-200">Med hjälp av hello **Infoga UDF: er** knappen visar en post med namnet **myudfs**, med en annan listrutan för varje UDF har definierats för den här resursen.</span><span class="sxs-lookup"><span data-stu-id="aa267-200">Using hello **Insert udfs** button displays an entry named **myudfs**, with another drop-down for each UDF defined for that resource.</span></span> <span data-ttu-id="aa267-201">I det här fallet **myawesomeudf**.</span><span class="sxs-lookup"><span data-stu-id="aa267-201">In this case, **myawesomeudf**.</span></span> <span data-ttu-id="aa267-202">Markerar den här posten läggs hello efter toohello början av hello-frågan:</span><span class="sxs-lookup"><span data-stu-id="aa267-202">Selecting this entry adds hello following toohello beginning of hello query:</span></span>

```hiveql
add jar /myudfs.jar;
create temporary function myawesomeudf as 'com.myudfs.Awesome';
```

<span data-ttu-id="aa267-203">Du kan sedan använda hello UDF i frågan.</span><span class="sxs-lookup"><span data-stu-id="aa267-203">You can then use hello UDF in your query.</span></span> <span data-ttu-id="aa267-204">Till exempel `SELECT myawesomeudf(name) FROM people;`.</span><span class="sxs-lookup"><span data-stu-id="aa267-204">For example, `SELECT myawesomeudf(name) FROM people;`.</span></span>

<span data-ttu-id="aa267-205">Mer information om hur du använder UDF: er med Hive i HDInsight finns i följande dokument hello:</span><span class="sxs-lookup"><span data-stu-id="aa267-205">For more information on using UDFs with Hive on HDInsight, see hello following documents:</span></span>

* [<span data-ttu-id="aa267-206">Med hjälp av Python med Hive och Pig i HDInsight</span><span class="sxs-lookup"><span data-stu-id="aa267-206">Using Python with Hive and Pig in HDInsight</span></span>](hdinsight-python.md)
* [<span data-ttu-id="aa267-207">Hur tooadd en anpassad Hive UDF-tooHDInsight</span><span class="sxs-lookup"><span data-stu-id="aa267-207">How tooadd a custom Hive UDF tooHDInsight</span></span>](http://blogs.msdn.com/b/bigdatasupport/archive/2014/01/14/how-to-add-custom-hive-udfs-to-hdinsight.aspx)

## <a name="hive-settings"></a><span data-ttu-id="aa267-208">Hive-inställningar</span><span class="sxs-lookup"><span data-stu-id="aa267-208">Hive settings</span></span>

<span data-ttu-id="aa267-209">Inställningarna kan vara används toochange olika Hive-inställningar.</span><span class="sxs-lookup"><span data-stu-id="aa267-209">Settings can be used toochange various Hive settings.</span></span> <span data-ttu-id="aa267-210">Till exempel ändra hello Körningsmotor för Hive från tooMapReduce Tez (hello standard).</span><span class="sxs-lookup"><span data-stu-id="aa267-210">For example, changing hello execution engine for Hive from Tez (hello default) tooMapReduce.</span></span>

## <span data-ttu-id="aa267-211"><a id="nextsteps"></a>Nästa steg</span><span class="sxs-lookup"><span data-stu-id="aa267-211"><a id="nextsteps"></a>Next steps</span></span>

<span data-ttu-id="aa267-212">Allmän information om Hive i HDInsight:</span><span class="sxs-lookup"><span data-stu-id="aa267-212">For general information on Hive in HDInsight:</span></span>

* [<span data-ttu-id="aa267-213">Använda Hive med Hadoop i HDInsight</span><span class="sxs-lookup"><span data-stu-id="aa267-213">Use Hive with Hadoop on HDInsight</span></span>](hdinsight-use-hive.md)

<span data-ttu-id="aa267-214">Information om andra sätt kan du arbeta med Hadoop i HDInsight:</span><span class="sxs-lookup"><span data-stu-id="aa267-214">For information on other ways you can work with Hadoop on HDInsight:</span></span>

* [<span data-ttu-id="aa267-215">Använda Pig med Hadoop i HDInsight</span><span class="sxs-lookup"><span data-stu-id="aa267-215">Use Pig with Hadoop on HDInsight</span></span>](hdinsight-use-pig.md)
* [<span data-ttu-id="aa267-216">Använda MapReduce med Hadoop i HDInsight</span><span class="sxs-lookup"><span data-stu-id="aa267-216">Use MapReduce with Hadoop on HDInsight</span></span>](hdinsight-use-mapreduce.md)
