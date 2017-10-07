---
title: aaaUse Beeline med Apache Hive - Azure HDInsight | Microsoft Docs
description: "Lär dig hur toouse hello Beeline klienten toorun Hive-frågor med Hadoop i HDInsight. Beeline är ett verktyg för att arbeta med HiveServer2 över JDBC."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
keywords: beeline hive, hive beeline
ms.assetid: 3adfb1ba-8924-4a13-98db-10a67ab24fca
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/26/2017
ms.author: larryfr
ms.openlocfilehash: e788ff39f33d928808cfcb83a92f62ac9ae8ca09
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-beeline-client-with-apache-hive"></a><span data-ttu-id="68f19-105">Använda hello Beeline klienten med Apache Hive</span><span class="sxs-lookup"><span data-stu-id="68f19-105">Use hello Beeline client with Apache Hive</span></span>

<span data-ttu-id="68f19-106">Lär dig hur toouse [Beeline](https://cwiki.apache.org/confluence/display/Hive/HiveServer2+Clients#HiveServer2Clients-Beeline–NewCommandLineShell) toorun Hive-frågor i HDInsight.</span><span class="sxs-lookup"><span data-stu-id="68f19-106">Learn how toouse [Beeline](https://cwiki.apache.org/confluence/display/Hive/HiveServer2+Clients#HiveServer2Clients-Beeline–NewCommandLineShell) toorun Hive queries on HDInsight.</span></span>

<span data-ttu-id="68f19-107">Beeline är en Hive-klient som ingår i hello huvudnoderna av ditt HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="68f19-107">Beeline is a Hive client that is included on hello head nodes of your HDInsight cluster.</span></span> <span data-ttu-id="68f19-108">Beeline använder JDBC tooconnect tooHiveServer2, en tjänst som finns på ditt HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="68f19-108">Beeline uses JDBC tooconnect tooHiveServer2, a service hosted on your HDInsight cluster.</span></span> <span data-ttu-id="68f19-109">Du kan också använda Beeline tooaccess Hive i HDInsight via fjärranslutning över hello internet.</span><span class="sxs-lookup"><span data-stu-id="68f19-109">You can also use Beeline tooaccess Hive on HDInsight remotely over hello internet.</span></span> <span data-ttu-id="68f19-110">hello följande tabell innehåller anslutningssträngar för användning med Beeline:</span><span class="sxs-lookup"><span data-stu-id="68f19-110">hello following table provides connection strings for use with Beeline:</span></span>

| <span data-ttu-id="68f19-111">Där du kör Beeline från</span><span class="sxs-lookup"><span data-stu-id="68f19-111">Where you run Beeline from</span></span> | <span data-ttu-id="68f19-112">Parametrar</span><span class="sxs-lookup"><span data-stu-id="68f19-112">Parameters</span></span> |
| --- | --- | --- |
| <span data-ttu-id="68f19-113">En SSH-anslutning tooa headnode eller edge-nod</span><span class="sxs-lookup"><span data-stu-id="68f19-113">An SSH connection tooa headnode or edge node</span></span> | `-u 'jdbc:hive2://headnodehost:10001/;transportMode=http'` |
| <span data-ttu-id="68f19-114">Utanför hello-kluster</span><span class="sxs-lookup"><span data-stu-id="68f19-114">Outside hello cluster</span></span> | `-u 'jdbc:hive2://clustername.azurehdinsight.net:443/;ssl=true;transportMode=http;httpPath=/hive2' -n admin -p password` |

> [!NOTE]
> <span data-ttu-id="68f19-115">Ersätt `admin` med hello klustret inloggningskonto för klustret.</span><span class="sxs-lookup"><span data-stu-id="68f19-115">Replace `admin` with hello cluster login account for your cluster.</span></span>
>
> <span data-ttu-id="68f19-116">Ersätt `password` med hello lösenord för hello inloggningskonto för klustret.</span><span class="sxs-lookup"><span data-stu-id="68f19-116">Replace `password` with hello password for hello cluster login account.</span></span>
>
> <span data-ttu-id="68f19-117">Ersätt `clustername` med hello namnet på ditt HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="68f19-117">Replace `clustername` with hello name of your HDInsight cluster.</span></span>

## <span data-ttu-id="68f19-118"><a id="prereq"></a>Förhandskrav</span><span class="sxs-lookup"><span data-stu-id="68f19-118"><a id="prereq"></a>Prerequisites</span></span>

* <span data-ttu-id="68f19-119">En Linux-baserade Hadoop på HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="68f19-119">A Linux-based Hadoop on HDInsight cluster.</span></span>

  > [!IMPORTANT]
  > <span data-ttu-id="68f19-120">Linux är hello endast operativsystem på HDInsight version 3.4 eller senare.</span><span class="sxs-lookup"><span data-stu-id="68f19-120">Linux is hello only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="68f19-121">Mer information finns i [HDInsight-avveckling på Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="68f19-121">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

* <span data-ttu-id="68f19-122">En SSH-klient eller en lokal Beeline-klient.</span><span class="sxs-lookup"><span data-stu-id="68f19-122">An SSH client or a local Beeline client.</span></span> <span data-ttu-id="68f19-123">De flesta av hello stegen i det här dokumentet förutsätter att du använder Beeline från ett kluster med SSH-session toohello.</span><span class="sxs-lookup"><span data-stu-id="68f19-123">Most of hello steps in this document assume that you are using Beeline from an SSH session toohello cluster.</span></span> <span data-ttu-id="68f19-124">Information om hur du kör Beeline från utanför hello-kluster finns i hello [använder Beeline via fjärranslutning](#remote) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="68f19-124">For information on running Beeline from outside hello cluster, see hello [use Beeline remotely](#remote) section.</span></span>

    <span data-ttu-id="68f19-125">Mer information om hur du använder SSH finns [använda SSH med HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="68f19-125">For more information on using SSH, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

## <span data-ttu-id="68f19-126"><a id="beeline"></a>Använd Beeline</span><span class="sxs-lookup"><span data-stu-id="68f19-126"><a id="beeline"></a>Use Beeline</span></span>

1. <span data-ttu-id="68f19-127">När du startar Beeline, måste du ange en anslutningssträng för HiveServer2 på ditt HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="68f19-127">When starting Beeline, you must provide a connection string for HiveServer2 on your HDInsight cluster.</span></span> <span data-ttu-id="68f19-128">toorun hello kommando från utanför hello kluster, måste du även ange hello klusternamnet inloggningen konto (standard `admin`) och lösenord.</span><span class="sxs-lookup"><span data-stu-id="68f19-128">toorun hello command from outside hello cluster, you must also provide hello cluster login account name (default `admin`) and password.</span></span> <span data-ttu-id="68f19-129">Använd följande tabell toofind hello anslutning sträng format och parametrar toouse hello:</span><span class="sxs-lookup"><span data-stu-id="68f19-129">Use hello following table toofind hello connection string format and parameters toouse:</span></span>

    | <span data-ttu-id="68f19-130">Där du kör Beeline från</span><span class="sxs-lookup"><span data-stu-id="68f19-130">Where you run Beeline from</span></span> | <span data-ttu-id="68f19-131">Parametrar</span><span class="sxs-lookup"><span data-stu-id="68f19-131">Parameters</span></span> |
    | --- | --- | --- |
    | <span data-ttu-id="68f19-132">En SSH-anslutning tooa headnode eller edge-nod</span><span class="sxs-lookup"><span data-stu-id="68f19-132">An SSH connection tooa headnode or edge node</span></span> | `-u 'jdbc:hive2://headnodehost:10001/;transportMode=http'` |
    | <span data-ttu-id="68f19-133">Utanför hello-kluster</span><span class="sxs-lookup"><span data-stu-id="68f19-133">Outside hello cluster</span></span> | `-u 'jdbc:hive2://clustername.azurehdinsight.net:443/;ssl=true;transportMode=http;httpPath=/hive2' -n admin -p password` |

    <span data-ttu-id="68f19-134">Följande kommando hello kan till exempel vara används toostart Beeline från ett kluster med SSH-session toohello:</span><span class="sxs-lookup"><span data-stu-id="68f19-134">For example, hello following command can be used toostart Beeline from an SSH session toohello cluster:</span></span>

    ```bash
    beeline -u 'jdbc:hive2://headnodehost:10001/;transportMode=http'
    ```

    <span data-ttu-id="68f19-135">Detta kommando startar hello Beeline klienten och ansluter tooHiveServer2 i hello klustrets huvudnod.</span><span class="sxs-lookup"><span data-stu-id="68f19-135">This command starts hello Beeline client, and connects tooHiveServer2 on hello cluster head node.</span></span> <span data-ttu-id="68f19-136">När hello-kommandot har slutförts kommer du till en `jdbc:hive2://headnodehost:10001/>` prompt.</span><span class="sxs-lookup"><span data-stu-id="68f19-136">Once hello command completes, you arrive at a `jdbc:hive2://headnodehost:10001/>` prompt.</span></span>

2. <span data-ttu-id="68f19-137">Beeline kommandon som börjar med en `!` tecken, till exempel `!help` visar hjälpen.</span><span class="sxs-lookup"><span data-stu-id="68f19-137">Beeline commands begin with a `!` character, for example `!help` displays help.</span></span> <span data-ttu-id="68f19-138">Men hello `!` kan utelämnas för vissa kommandon.</span><span class="sxs-lookup"><span data-stu-id="68f19-138">However hello `!` can be omitted for some commands.</span></span> <span data-ttu-id="68f19-139">Till exempel `help` fungerar även.</span><span class="sxs-lookup"><span data-stu-id="68f19-139">For example, `help` also works.</span></span>

    <span data-ttu-id="68f19-140">Det finns en `!sql`, vilket är används tooexecute HiveQL-instruktioner.</span><span class="sxs-lookup"><span data-stu-id="68f19-140">There is a `!sql`, which is used tooexecute HiveQL statements.</span></span> <span data-ttu-id="68f19-141">Dock HiveQL så används ofta kan du utelämna hello föregående `!sql`.</span><span class="sxs-lookup"><span data-stu-id="68f19-141">However, HiveQL is so commonly used that you can omit hello preceding `!sql`.</span></span> <span data-ttu-id="68f19-142">hello följande två rapporter är likvärdiga:</span><span class="sxs-lookup"><span data-stu-id="68f19-142">hello following two statements are equivalent:</span></span>

    ```hiveql
    !sql show tables;
    show tables;
    ```

    <span data-ttu-id="68f19-143">Endast en tabell anges på ett nytt kluster: **hivesampletable**.</span><span class="sxs-lookup"><span data-stu-id="68f19-143">On a new cluster, only one table is listed: **hivesampletable**.</span></span>

3. <span data-ttu-id="68f19-144">Använd följande kommando toodisplay hello schemat för hello hivesampletable hello:</span><span class="sxs-lookup"><span data-stu-id="68f19-144">Use hello following command toodisplay hello schema for hello hivesampletable:</span></span>

    ```hiveql
    describe hivesampletable;
    ```

    <span data-ttu-id="68f19-145">Det här kommandot returnerar hello följande information:</span><span class="sxs-lookup"><span data-stu-id="68f19-145">This command returns hello following information:</span></span>

        +-----------------------+------------+----------+--+
        |       col_name        | data_type  | comment  |
        +-----------------------+------------+----------+--+
        | clientid              | string     |          |
        | querytime             | string     |          |
        | market                | string     |          |
        | deviceplatform        | string     |          |
        | devicemake            | string     |          |
        | devicemodel           | string     |          |
        | state                 | string     |          |
        | country               | string     |          |
        | querydwelltime        | double     |          |
        | sessionid             | bigint     |          |
        | sessionpagevieworder  | bigint     |          |
        +-----------------------+------------+----------+--+

    <span data-ttu-id="68f19-146">Den här informationen beskriver hello kolumner i hello tabellen.</span><span class="sxs-lookup"><span data-stu-id="68f19-146">This information describes hello columns in hello table.</span></span> <span data-ttu-id="68f19-147">Medan vi kunde utföra några frågor mot dessa data i stället skapar vi en helt ny tabell toodemonstrate hur tooload data till Hive och tillämpa ett schema.</span><span class="sxs-lookup"><span data-stu-id="68f19-147">While we could perform some queries against this data, let's instead create a brand new table toodemonstrate how tooload data into Hive and apply a schema.</span></span>

4. <span data-ttu-id="68f19-148">Ange hello följande instruktioner toocreate en tabell med namnet **log4jLogs** med exempeldata medföljer hello HDInsight-kluster:</span><span class="sxs-lookup"><span data-stu-id="68f19-148">Enter hello following statements toocreate a table named **log4jLogs** by using sample data provided with hello HDInsight cluster:</span></span>

    ```hiveql
    DROP TABLE log4jLogs;
    CREATE EXTERNAL TABLE log4jLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string)
    ROW FORMAT DELIMITED FIELDS TERMINATED BY ' '
    STORED AS TEXTFILE LOCATION 'wasb:///example/data/';
    SELECT t4 AS sev, COUNT(*) AS count FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log' GROUP BY t4;
    ```

    <span data-ttu-id="68f19-149">Dessa instruktioner utför hello följande åtgärder:</span><span class="sxs-lookup"><span data-stu-id="68f19-149">These statements perform hello following actions:</span></span>

    * <span data-ttu-id="68f19-150">`DROP TABLE`– Om hello tabellen finns, tas bort.</span><span class="sxs-lookup"><span data-stu-id="68f19-150">`DROP TABLE` - If hello table exists, it is deleted.</span></span>

    * <span data-ttu-id="68f19-151">`CREATE EXTERNAL TABLE`-Skapar en **externa** tabellen i Hive.</span><span class="sxs-lookup"><span data-stu-id="68f19-151">`CREATE EXTERNAL TABLE` - Creates an **external** table in Hive.</span></span> <span data-ttu-id="68f19-152">Externa tabeller kan du bara lagra hello tabelldefinition i Hive.</span><span class="sxs-lookup"><span data-stu-id="68f19-152">External tables only store hello table definition in Hive.</span></span> <span data-ttu-id="68f19-153">hello data finns kvar i hello ursprungliga plats.</span><span class="sxs-lookup"><span data-stu-id="68f19-153">hello data is left in hello original location.</span></span>

    * <span data-ttu-id="68f19-154">`ROW FORMAT`-Hur hello data formateras.</span><span class="sxs-lookup"><span data-stu-id="68f19-154">`ROW FORMAT` - How hello data is formatted.</span></span> <span data-ttu-id="68f19-155">I det här fallet avgränsas hello fälten i varje logg med ett blanksteg.</span><span class="sxs-lookup"><span data-stu-id="68f19-155">In this case, hello fields in each log are separated by a space.</span></span>

    * <span data-ttu-id="68f19-156">`STORED AS TEXTFILE LOCATION`-Där hello data lagras och i vilka filformat.</span><span class="sxs-lookup"><span data-stu-id="68f19-156">`STORED AS TEXTFILE LOCATION` - Where hello data is stored and in what file format.</span></span>

    * <span data-ttu-id="68f19-157">`SELECT`-Väljer en uppräkning av alla rader där kolumnen **t4** innehåller hello värdet **[fel]**.</span><span class="sxs-lookup"><span data-stu-id="68f19-157">`SELECT` - Selects a count of all rows where column **t4** contains hello value **[ERROR]**.</span></span> <span data-ttu-id="68f19-158">Den här frågan returnerar ett värde för **3** som det finns tre rader som innehåller det här värdet.</span><span class="sxs-lookup"><span data-stu-id="68f19-158">This query returns a value of **3** as there are three rows that contain this value.</span></span>

    * <span data-ttu-id="68f19-159">`INPUT__FILE__NAME LIKE '%.log'`-Hive försöker tooapply hello schemafiler tooall i hello directory.</span><span class="sxs-lookup"><span data-stu-id="68f19-159">`INPUT__FILE__NAME LIKE '%.log'` - Hive attempts tooapply hello schema tooall files in hello directory.</span></span> <span data-ttu-id="68f19-160">I det här fallet innehåller hello katalogen filer som inte matchar hello schemat.</span><span class="sxs-lookup"><span data-stu-id="68f19-160">In this case, hello directory contains files that do not match hello schema.</span></span> <span data-ttu-id="68f19-161">tooprevent ogiltiga data i hello resultat instruktionen anger Hive vi bör endast returnera data från filer som slutar på. log.</span><span class="sxs-lookup"><span data-stu-id="68f19-161">tooprevent garbage data in hello results, this statement tells Hive that we should only return data from files ending in .log.</span></span>

  > [!NOTE]
  > <span data-ttu-id="68f19-162">Externa tabeller ska användas när du förväntar dig hello underliggande data toobe uppdateras av en extern källa.</span><span class="sxs-lookup"><span data-stu-id="68f19-162">External tables should be used when you expect hello underlying data toobe updated by an external source.</span></span> <span data-ttu-id="68f19-163">Till exempel en automatisk överföring av data eller en MapReduce-åtgärd.</span><span class="sxs-lookup"><span data-stu-id="68f19-163">For example, an automated data upload process or a MapReduce operation.</span></span>
  >
  > <span data-ttu-id="68f19-164">Släppa en extern tabell har **inte** ta bort data hello, bara hello tabelldefinitionen.</span><span class="sxs-lookup"><span data-stu-id="68f19-164">Dropping an external table does **not** delete hello data, only hello table definition.</span></span>

    <span data-ttu-id="68f19-165">hello utdata från kommandot liknande toohello följer text:</span><span class="sxs-lookup"><span data-stu-id="68f19-165">hello output of this command is similar toohello following text:</span></span>

        INFO  : Tez session hasn't been created yet. Opening session
        INFO  :

        INFO  : Status: Running (Executing on YARN cluster with App id application_1443698635933_0001)

        INFO  : Map 1: -/-      Reducer 2: 0/1
        INFO  : Map 1: 0/1      Reducer 2: 0/1
        INFO  : Map 1: 0/1      Reducer 2: 0/1
        INFO  : Map 1: 0/1      Reducer 2: 0/1
        INFO  : Map 1: 0/1      Reducer 2: 0/1
        INFO  : Map 1: 0(+1)/1  Reducer 2: 0/1
        INFO  : Map 1: 0(+1)/1  Reducer 2: 0/1
        INFO  : Map 1: 1/1      Reducer 2: 0/1
        INFO  : Map 1: 1/1      Reducer 2: 0(+1)/1
        INFO  : Map 1: 1/1      Reducer 2: 1/1
        +----------+--------+--+
        |   sev    | count  |
        +----------+--------+--+
        | [ERROR]  | 3      |
        +----------+--------+--+
        1 row selected (47.351 seconds)

5. <span data-ttu-id="68f19-166">tooexit Beeline, Använd `!exit`.</span><span class="sxs-lookup"><span data-stu-id="68f19-166">tooexit Beeline, use `!exit`.</span></span>

## <span data-ttu-id="68f19-167"><a id="file"></a>Använd Beeline toorun en HiveQL-fil</span><span class="sxs-lookup"><span data-stu-id="68f19-167"><a id="file"></a>Use Beeline toorun a HiveQL file</span></span>

<span data-ttu-id="68f19-168">Använd följande steg toocreate en fil, kör den med hjälp av Beeline hello.</span><span class="sxs-lookup"><span data-stu-id="68f19-168">Use hello following steps toocreate a file, then run it using Beeline.</span></span>

1. <span data-ttu-id="68f19-169">Använd hello följande kommando toocreate en fil med namnet **query.hql**:</span><span class="sxs-lookup"><span data-stu-id="68f19-169">Use hello following command toocreate a file named **query.hql**:</span></span>

    ```bash
    nano query.hql
    ```

2. <span data-ttu-id="68f19-170">Använd hello följande text som hello innehållet i hello-fil.</span><span class="sxs-lookup"><span data-stu-id="68f19-170">Use hello following text as hello contents of hello file.</span></span> <span data-ttu-id="68f19-171">Den här frågan skapar en ny ”interna” tabell med namnet **errorLogs**:</span><span class="sxs-lookup"><span data-stu-id="68f19-171">This query creates a new 'internal' table named **errorLogs**:</span></span>

    ```hiveql
    CREATE TABLE IF NOT EXISTS errorLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string) STORED AS ORC;
    INSERT OVERWRITE TABLE errorLogs SELECT t1, t2, t3, t4, t5, t6, t7 FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log';
    ```

    <span data-ttu-id="68f19-172">Dessa instruktioner utför hello följande åtgärder:</span><span class="sxs-lookup"><span data-stu-id="68f19-172">These statements perform hello following actions:</span></span>

    * <span data-ttu-id="68f19-173">**Skapa tabell om inte finns** -om hello tabellen inte redan finns, skapas den.</span><span class="sxs-lookup"><span data-stu-id="68f19-173">**CREATE TABLE IF NOT EXISTS** - If hello table does not already exist, it is created.</span></span> <span data-ttu-id="68f19-174">Eftersom hello **externa** nyckelordet används inte, skapar en intern tabell för den här instruktionen.</span><span class="sxs-lookup"><span data-stu-id="68f19-174">Since hello **EXTERNAL** keyword is not used, this statement creates an internal table.</span></span> <span data-ttu-id="68f19-175">Interna register lagras i datalagret för hello Hive och hanteras helt av Hive.</span><span class="sxs-lookup"><span data-stu-id="68f19-175">Internal tables are stored in hello Hive data warehouse and are managed completely by Hive.</span></span>
    * <span data-ttu-id="68f19-176">**LAGRAS AS ORC** -lagrar hello data i optimerad raden kolumner (ORC)-format.</span><span class="sxs-lookup"><span data-stu-id="68f19-176">**STORED AS ORC** - Stores hello data in Optimized Row Columnar (ORC) format.</span></span> <span data-ttu-id="68f19-177">ORC är ett mycket optimerad och effektiv format för att lagra data med Hive.</span><span class="sxs-lookup"><span data-stu-id="68f19-177">ORC format is a highly optimized and efficient format for storing Hive data.</span></span>
    * <span data-ttu-id="68f19-178">**INFOGA ÖVER... Välj** -väljer rader från hello **log4jLogs** tabellen som innehåller **[fel]**, och sedan infogningar hello data i hello **errorLogs** tabell.</span><span class="sxs-lookup"><span data-stu-id="68f19-178">**INSERT OVERWRITE ... SELECT** - Selects rows from hello **log4jLogs** table that contain **[ERROR]**, then inserts hello data into hello **errorLogs** table.</span></span>

    > [!NOTE]
    > <span data-ttu-id="68f19-179">Till skillnad från externa tabeller kan släppa en intern tabell tar bort hello samt underliggande data.</span><span class="sxs-lookup"><span data-stu-id="68f19-179">Unlike external tables, dropping an internal table deletes hello underlying data as well.</span></span>

3. <span data-ttu-id="68f19-180">toosave hello-fil, Använd **Ctrl**+**_X**, ange **Y**, och slutligen **RETUR**.</span><span class="sxs-lookup"><span data-stu-id="68f19-180">toosave hello file, use **Ctrl**+**_X**, then enter **Y**, and finally **Enter**.</span></span>

4. <span data-ttu-id="68f19-181">Använd följande toorun hello-fil med Beeline hello:</span><span class="sxs-lookup"><span data-stu-id="68f19-181">Use hello following toorun hello file using Beeline:</span></span>

    ```bash
    beeline -u 'jdbc:hive2://headnodehost:10001/;transportMode=http' -i query.hql
    ```

    > [!NOTE]
    > <span data-ttu-id="68f19-182">Hej `-i` parametern börjar Beeline, körs hello instruktioner i hello query.hql fil.</span><span class="sxs-lookup"><span data-stu-id="68f19-182">hello `-i` parameter starts Beeline, runs hello statements in hello query.hql file.</span></span> <span data-ttu-id="68f19-183">När hello frågan har slutförts kan du når hello `jdbc:hive2://headnodehost:10001/>` prompt.</span><span class="sxs-lookup"><span data-stu-id="68f19-183">Once hello query completes, you arrive at hello `jdbc:hive2://headnodehost:10001/>` prompt.</span></span> <span data-ttu-id="68f19-184">Du kan också köra en fil med hello `-f` som avslutas Beeline när hello frågan har slutförts.</span><span class="sxs-lookup"><span data-stu-id="68f19-184">You can also run a file using hello `-f` parameter, which exits Beeline after hello query completes.</span></span>

5. <span data-ttu-id="68f19-185">tooverify hello **errorLogs** tabellen har skapats använder hello följande instruktion tooreturn alla hello rader från **errorLogs**:</span><span class="sxs-lookup"><span data-stu-id="68f19-185">tooverify that hello **errorLogs** table was created, use hello following statement tooreturn all hello rows from **errorLogs**:</span></span>

    ```hiveql
    SELECT * from errorLogs;
    ```

    <span data-ttu-id="68f19-186">Tre raderna med data ska returneras, som innehåller alla **[fel]** i kolumnen t4:</span><span class="sxs-lookup"><span data-stu-id="68f19-186">Three rows of data should be returned, all containing **[ERROR]** in column t4:</span></span>

        +---------------+---------------+---------------+---------------+---------------+---------------+---------------+--+
        | errorlogs.t1  | errorlogs.t2  | errorlogs.t3  | errorlogs.t4  | errorlogs.t5  | errorlogs.t6  | errorlogs.t7  |
        +---------------+---------------+---------------+---------------+---------------+---------------+---------------+--+
        | 2012-02-03    | 18:35:34      | SampleClass0  | [ERROR]       | incorrect     | id            |               |
        | 2012-02-03    | 18:55:54      | SampleClass1  | [ERROR]       | incorrect     | id            |               |
        | 2012-02-03    | 19:25:27      | SampleClass4  | [ERROR]       | incorrect     | id            |               |
        +---------------+---------------+---------------+---------------+---------------+---------------+---------------+--+
        3 rows selected (1.538 seconds)

## <span data-ttu-id="68f19-187"><a id="remote"></a>Använd Beeline via fjärranslutning</span><span class="sxs-lookup"><span data-stu-id="68f19-187"><a id="remote"></a>Use Beeline remotely</span></span>

<span data-ttu-id="68f19-188">Om du har installerat lokalt Beeline eller använder den via en Docker-avbildning som [sutoiku/beeline](https://hub.docker.com/r/sutoiku/beeline/), måste du använda hello följande parametrar:</span><span class="sxs-lookup"><span data-stu-id="68f19-188">If you have Beeline installed locally, or are using it through a Docker image such as [sutoiku/beeline](https://hub.docker.com/r/sutoiku/beeline/), you must use hello following parameters:</span></span>

* <span data-ttu-id="68f19-189">__Anslutningssträngen__:`-u 'jdbc:hive2://clustername.azurehdinsight.net:443/;ssl=true;transportMode=http;httpPath=/hive2'`</span><span class="sxs-lookup"><span data-stu-id="68f19-189">__Connection string__: `-u 'jdbc:hive2://clustername.azurehdinsight.net:443/;ssl=true;transportMode=http;httpPath=/hive2'`</span></span>

* <span data-ttu-id="68f19-190">__Klustrets inloggningsnamn__:`-n admin`</span><span class="sxs-lookup"><span data-stu-id="68f19-190">__Cluster login name__: `-n admin`</span></span>

* <span data-ttu-id="68f19-191">__Klustret inloggningslösenordet__`-p 'password'`</span><span class="sxs-lookup"><span data-stu-id="68f19-191">__Cluster login password__ `-p 'password'`</span></span>

<span data-ttu-id="68f19-192">Ersätt hello `clustername` i hello-anslutningssträngen med hello namnet på ditt HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="68f19-192">Replace hello `clustername` in hello connection string with hello name of your HDInsight cluster.</span></span>

<span data-ttu-id="68f19-193">Ersätt `admin` med hello namn för klusterinloggning och Ersätt `password` med hello lösenord för kluster-inloggningen.</span><span class="sxs-lookup"><span data-stu-id="68f19-193">Replace `admin` with hello name of your cluster login, and replace `password` with hello password for your cluster login.</span></span>

## <span data-ttu-id="68f19-194"><a id="sparksql"></a>Använda Beeline med Spark</span><span class="sxs-lookup"><span data-stu-id="68f19-194"><a id="sparksql"></a>Use Beeline with Spark</span></span>

<span data-ttu-id="68f19-195">Spark innehåller sin egen implementering av HiveServer2, vilket ofta är hänvisade tooas hello Spark Thrift-servern.</span><span class="sxs-lookup"><span data-stu-id="68f19-195">Spark provides its own implementation of HiveServer2, which is often refered tooas hello Spark Thrift server.</span></span> <span data-ttu-id="68f19-196">Den här tjänsten använder Spark SQL tooresolve frågor i stället för Hive och kan ge bättre prestanda beroende på din fråga.</span><span class="sxs-lookup"><span data-stu-id="68f19-196">This service uses Spark SQL tooresolve queries instead of Hive, and may provide better performance depending on your query.</span></span>

<span data-ttu-id="68f19-197">tooconnect toohello Spark Thrift-servern i ett Spark på HDInsight-kluster, Använd port `10002` i stället för `10001`.</span><span class="sxs-lookup"><span data-stu-id="68f19-197">tooconnect toohello Spark Thrift server of a Spark on HDInsight cluster, use port `10002` instead of `10001`.</span></span> <span data-ttu-id="68f19-198">Till exempel `beeline -u 'jdbc:hive2://headnodehost:10002/;transportMode=http'`.</span><span class="sxs-lookup"><span data-stu-id="68f19-198">For example, `beeline -u 'jdbc:hive2://headnodehost:10002/;transportMode=http'`.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="68f19-199">hello Spark Thrift-servern är inte direkt tillgänglig över hello internet.</span><span class="sxs-lookup"><span data-stu-id="68f19-199">hello Spark Thrift server is not directly accessible over hello internet.</span></span> <span data-ttu-id="68f19-200">Du kan bara ansluta tooit från en SSH-session eller i hello samma virtuella Azure-nätverk som hello HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="68f19-200">You can only connect tooit from an SSH session or inside hello same Azure Virtual Network as hello HDInsight cluster.</span></span>

## <span data-ttu-id="68f19-201"><a id="summary"></a><a id="nextsteps"></a>Nästa steg</span><span class="sxs-lookup"><span data-stu-id="68f19-201"><a id="summary"></a><a id="nextsteps"></a>Next steps</span></span>

<span data-ttu-id="68f19-202">Mer allmän information om Hive i HDInsight finns i hello följande dokument:</span><span class="sxs-lookup"><span data-stu-id="68f19-202">For more general information on Hive in HDInsight, see hello following document:</span></span>

* [<span data-ttu-id="68f19-203">Använda Hive med Hadoop i HDInsight</span><span class="sxs-lookup"><span data-stu-id="68f19-203">Use Hive with Hadoop on HDInsight</span></span>](hdinsight-use-hive.md)

<span data-ttu-id="68f19-204">Mer information om andra sätt kan du arbeta med Hadoop i HDInsight finns i hello följande dokument:</span><span class="sxs-lookup"><span data-stu-id="68f19-204">For more information on other ways you can work with Hadoop on HDInsight, see hello following documents:</span></span>

* [<span data-ttu-id="68f19-205">Använda Pig med Hadoop i HDInsight</span><span class="sxs-lookup"><span data-stu-id="68f19-205">Use Pig with Hadoop on HDInsight</span></span>](hdinsight-use-pig.md)
* [<span data-ttu-id="68f19-206">Använda MapReduce med Hadoop i HDInsight</span><span class="sxs-lookup"><span data-stu-id="68f19-206">Use MapReduce with Hadoop on HDInsight</span></span>](hdinsight-use-mapreduce.md)

<span data-ttu-id="68f19-207">Om du använder Tez med Hive finns hello följande dokument:</span><span class="sxs-lookup"><span data-stu-id="68f19-207">If you are using Tez with Hive, see hello following documents:</span></span>

* [<span data-ttu-id="68f19-208">Använda hello Tez UI på Windows-baserade HDInsight</span><span class="sxs-lookup"><span data-stu-id="68f19-208">Use hello Tez UI on Windows-based HDInsight</span></span>](hdinsight-debug-tez-ui.md)
* [<span data-ttu-id="68f19-209">Använd hello Ambari Tez vy på Linux-baserat HDInsight</span><span class="sxs-lookup"><span data-stu-id="68f19-209">Use hello Ambari Tez view on Linux-based HDInsight</span></span>](hdinsight-debug-ambari-tez-view.md)

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

[putty]: http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html


[hdinsight-provision]: hdinsight-hadoop-provision-linux-clusters.md
[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md
[hdinsight-upload-data]: hdinsight-upload-data.md


[powershell-here-strings]: http://technet.microsoft.com/library/ee692792.aspx
