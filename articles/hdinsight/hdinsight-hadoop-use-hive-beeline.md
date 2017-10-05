---
title: "Använda Beeline med Apache Hive - Azure HDInsight | Microsoft Docs"
description: "Lär dig hur du använder Beeline klienten för att köra Hive-frågor med Hadoop i HDInsight. Beeline är ett verktyg för att arbeta med HiveServer2 över JDBC."
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
ms.openlocfilehash: 153044aafa3a67ee85bb1997beb821777c938563
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="use-the-beeline-client-with-apache-hive"></a><span data-ttu-id="aac03-105">Använda Beeline klienten med Apache Hive</span><span class="sxs-lookup"><span data-stu-id="aac03-105">Use the Beeline client with Apache Hive</span></span>

<span data-ttu-id="aac03-106">Lär dig hur du använder [Beeline](https://cwiki.apache.org/confluence/display/Hive/HiveServer2+Clients#HiveServer2Clients-Beeline–NewCommandLineShell) att köra Hive-frågor i HDInsight.</span><span class="sxs-lookup"><span data-stu-id="aac03-106">Learn how to use [Beeline](https://cwiki.apache.org/confluence/display/Hive/HiveServer2+Clients#HiveServer2Clients-Beeline–NewCommandLineShell) to run Hive queries on HDInsight.</span></span>

<span data-ttu-id="aac03-107">Beeline är en Hive-klient som ingår i huvudnoderna för ditt HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="aac03-107">Beeline is a Hive client that is included on the head nodes of your HDInsight cluster.</span></span> <span data-ttu-id="aac03-108">Beeline använder JDBC för att ansluta till HiveServer2, en tjänst som finns på ditt HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="aac03-108">Beeline uses JDBC to connect to HiveServer2, a service hosted on your HDInsight cluster.</span></span> <span data-ttu-id="aac03-109">Du kan också använda Beeline för att fjärransluta Hive i HDInsight via internet.</span><span class="sxs-lookup"><span data-stu-id="aac03-109">You can also use Beeline to access Hive on HDInsight remotely over the internet.</span></span> <span data-ttu-id="aac03-110">Följande tabell innehåller anslutningssträngar för användning med Beeline:</span><span class="sxs-lookup"><span data-stu-id="aac03-110">The following table provides connection strings for use with Beeline:</span></span>

| <span data-ttu-id="aac03-111">Där du kör Beeline från</span><span class="sxs-lookup"><span data-stu-id="aac03-111">Where you run Beeline from</span></span> | <span data-ttu-id="aac03-112">Parametrar</span><span class="sxs-lookup"><span data-stu-id="aac03-112">Parameters</span></span> |
| --- | --- | --- |
| <span data-ttu-id="aac03-113">En SSH-anslutning till en headnode eller edge nod</span><span class="sxs-lookup"><span data-stu-id="aac03-113">An SSH connection to a headnode or edge node</span></span> | `-u 'jdbc:hive2://headnodehost:10001/;transportMode=http'` |
| <span data-ttu-id="aac03-114">Utanför klustret</span><span class="sxs-lookup"><span data-stu-id="aac03-114">Outside the cluster</span></span> | `-u 'jdbc:hive2://clustername.azurehdinsight.net:443/;ssl=true;transportMode=http;httpPath=/hive2' -n admin -p password` |

> [!NOTE]
> <span data-ttu-id="aac03-115">Ersätt `admin` med klustret inloggningskontot för klustret.</span><span class="sxs-lookup"><span data-stu-id="aac03-115">Replace `admin` with the cluster login account for your cluster.</span></span>
>
> <span data-ttu-id="aac03-116">Ersätt `password` med lösenordet för inloggningskontot för klustret.</span><span class="sxs-lookup"><span data-stu-id="aac03-116">Replace `password` with the password for the cluster login account.</span></span>
>
> <span data-ttu-id="aac03-117">Ersätt `clustername` med namnet på HDInsight-klustret.</span><span class="sxs-lookup"><span data-stu-id="aac03-117">Replace `clustername` with the name of your HDInsight cluster.</span></span>

## <span data-ttu-id="aac03-118"><a id="prereq"></a>Förhandskrav</span><span class="sxs-lookup"><span data-stu-id="aac03-118"><a id="prereq"></a>Prerequisites</span></span>

* <span data-ttu-id="aac03-119">En Linux-baserade Hadoop på HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="aac03-119">A Linux-based Hadoop on HDInsight cluster.</span></span>

  > [!IMPORTANT]
  > <span data-ttu-id="aac03-120">Linux är det enda operativsystemet som används med HDInsight version 3.4 och senare.</span><span class="sxs-lookup"><span data-stu-id="aac03-120">Linux is the only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="aac03-121">Mer information finns i [HDInsight-avveckling på Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="aac03-121">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

* <span data-ttu-id="aac03-122">En SSH-klient eller en lokal Beeline-klient.</span><span class="sxs-lookup"><span data-stu-id="aac03-122">An SSH client or a local Beeline client.</span></span> <span data-ttu-id="aac03-123">De flesta av stegen i det här dokumentet förutsätter att du använder Beeline från en SSH-session till klustret.</span><span class="sxs-lookup"><span data-stu-id="aac03-123">Most of the steps in this document assume that you are using Beeline from an SSH session to the cluster.</span></span> <span data-ttu-id="aac03-124">Information om hur du kör Beeline från utanför klustret finns i [använder Beeline via fjärranslutning](#remote) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="aac03-124">For information on running Beeline from outside the cluster, see the [use Beeline remotely](#remote) section.</span></span>

    <span data-ttu-id="aac03-125">Mer information om hur du använder SSH finns [använda SSH med HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="aac03-125">For more information on using SSH, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

## <span data-ttu-id="aac03-126"><a id="beeline"></a>Använd Beeline</span><span class="sxs-lookup"><span data-stu-id="aac03-126"><a id="beeline"></a>Use Beeline</span></span>

1. <span data-ttu-id="aac03-127">När du startar Beeline, måste du ange en anslutningssträng för HiveServer2 på ditt HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="aac03-127">When starting Beeline, you must provide a connection string for HiveServer2 on your HDInsight cluster.</span></span> <span data-ttu-id="aac03-128">Om du vill köra kommandot från utanför klustret måste du även ange klusternamnet inloggningen konto (standard `admin`) och lösenord.</span><span class="sxs-lookup"><span data-stu-id="aac03-128">To run the command from outside the cluster, you must also provide the cluster login account name (default `admin`) and password.</span></span> <span data-ttu-id="aac03-129">Använd följande tabell för att hitta Anslutningssträngens format och parametrar som ska användas:</span><span class="sxs-lookup"><span data-stu-id="aac03-129">Use the following table to find the connection string format and parameters to use:</span></span>

    | <span data-ttu-id="aac03-130">Där du kör Beeline från</span><span class="sxs-lookup"><span data-stu-id="aac03-130">Where you run Beeline from</span></span> | <span data-ttu-id="aac03-131">Parametrar</span><span class="sxs-lookup"><span data-stu-id="aac03-131">Parameters</span></span> |
    | --- | --- | --- |
    | <span data-ttu-id="aac03-132">En SSH-anslutning till en headnode eller edge nod</span><span class="sxs-lookup"><span data-stu-id="aac03-132">An SSH connection to a headnode or edge node</span></span> | `-u 'jdbc:hive2://headnodehost:10001/;transportMode=http'` |
    | <span data-ttu-id="aac03-133">Utanför klustret</span><span class="sxs-lookup"><span data-stu-id="aac03-133">Outside the cluster</span></span> | `-u 'jdbc:hive2://clustername.azurehdinsight.net:443/;ssl=true;transportMode=http;httpPath=/hive2' -n admin -p password` |

    <span data-ttu-id="aac03-134">Till exempel kan följande kommando användas för att starta Beeline från en SSH-session till klustret:</span><span class="sxs-lookup"><span data-stu-id="aac03-134">For example, the following command can be used to start Beeline from an SSH session to the cluster:</span></span>

    ```bash
    beeline -u 'jdbc:hive2://headnodehost:10001/;transportMode=http'
    ```

    <span data-ttu-id="aac03-135">Detta kommando startar Beeline-klienten och ansluter till HiveServer2 på klustrets huvudnod.</span><span class="sxs-lookup"><span data-stu-id="aac03-135">This command starts the Beeline client, and connects to HiveServer2 on the cluster head node.</span></span> <span data-ttu-id="aac03-136">När kommandot har slutförts kommer du till en `jdbc:hive2://headnodehost:10001/>` prompt.</span><span class="sxs-lookup"><span data-stu-id="aac03-136">Once the command completes, you arrive at a `jdbc:hive2://headnodehost:10001/>` prompt.</span></span>

2. <span data-ttu-id="aac03-137">Beeline kommandon som börjar med en `!` tecken, till exempel `!help` visar hjälpen.</span><span class="sxs-lookup"><span data-stu-id="aac03-137">Beeline commands begin with a `!` character, for example `!help` displays help.</span></span> <span data-ttu-id="aac03-138">Men den `!` kan utelämnas för vissa kommandon.</span><span class="sxs-lookup"><span data-stu-id="aac03-138">However the `!` can be omitted for some commands.</span></span> <span data-ttu-id="aac03-139">Till exempel `help` fungerar även.</span><span class="sxs-lookup"><span data-stu-id="aac03-139">For example, `help` also works.</span></span>

    <span data-ttu-id="aac03-140">Det finns en `!sql`, som används för att köra HiveQL-instruktioner.</span><span class="sxs-lookup"><span data-stu-id="aac03-140">There is a `!sql`, which is used to execute HiveQL statements.</span></span> <span data-ttu-id="aac03-141">HiveQL så ofta används dock att du kan hoppa över den föregående `!sql`.</span><span class="sxs-lookup"><span data-stu-id="aac03-141">However, HiveQL is so commonly used that you can omit the preceding `!sql`.</span></span> <span data-ttu-id="aac03-142">Följande två satser är likvärdiga:</span><span class="sxs-lookup"><span data-stu-id="aac03-142">The following two statements are equivalent:</span></span>

    ```hiveql
    !sql show tables;
    show tables;
    ```

    <span data-ttu-id="aac03-143">Endast en tabell anges på ett nytt kluster: **hivesampletable**.</span><span class="sxs-lookup"><span data-stu-id="aac03-143">On a new cluster, only one table is listed: **hivesampletable**.</span></span>

3. <span data-ttu-id="aac03-144">Använd följande kommando för att visa schemat för hivesampletable:</span><span class="sxs-lookup"><span data-stu-id="aac03-144">Use the following command to display the schema for the hivesampletable:</span></span>

    ```hiveql
    describe hivesampletable;
    ```

    <span data-ttu-id="aac03-145">Det här kommandot returnerar följande information:</span><span class="sxs-lookup"><span data-stu-id="aac03-145">This command returns the following information:</span></span>

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

    <span data-ttu-id="aac03-146">Den här informationen beskriver kolumnerna i tabellen.</span><span class="sxs-lookup"><span data-stu-id="aac03-146">This information describes the columns in the table.</span></span> <span data-ttu-id="aac03-147">Medan vi kunde utföra några frågor mot dessa data vi istället skapa en helt ny tabell för att demonstrera hur du läser in data i Hive och använda ett schema.</span><span class="sxs-lookup"><span data-stu-id="aac03-147">While we could perform some queries against this data, let's instead create a brand new table to demonstrate how to load data into Hive and apply a schema.</span></span>

4. <span data-ttu-id="aac03-148">Ange följande instruktioner om du vill skapa en tabell med namnet **log4jLogs** med exempeldata som medföljer HDInsight-kluster:</span><span class="sxs-lookup"><span data-stu-id="aac03-148">Enter the following statements to create a table named **log4jLogs** by using sample data provided with the HDInsight cluster:</span></span>

    ```hiveql
    DROP TABLE log4jLogs;
    CREATE EXTERNAL TABLE log4jLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string)
    ROW FORMAT DELIMITED FIELDS TERMINATED BY ' '
    STORED AS TEXTFILE LOCATION 'wasb:///example/data/';
    SELECT t4 AS sev, COUNT(*) AS count FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log' GROUP BY t4;
    ```

    <span data-ttu-id="aac03-149">Dessa instruktioner utför följande åtgärder:</span><span class="sxs-lookup"><span data-stu-id="aac03-149">These statements perform the following actions:</span></span>

    * <span data-ttu-id="aac03-150">`DROP TABLE`– Om tabellen finns tas bort.</span><span class="sxs-lookup"><span data-stu-id="aac03-150">`DROP TABLE` - If the table exists, it is deleted.</span></span>

    * <span data-ttu-id="aac03-151">`CREATE EXTERNAL TABLE`-Skapar en **externa** tabellen i Hive.</span><span class="sxs-lookup"><span data-stu-id="aac03-151">`CREATE EXTERNAL TABLE` - Creates an **external** table in Hive.</span></span> <span data-ttu-id="aac03-152">Externa tabeller kan du bara lagra tabelldefinitionen i Hive.</span><span class="sxs-lookup"><span data-stu-id="aac03-152">External tables only store the table definition in Hive.</span></span> <span data-ttu-id="aac03-153">Data finns kvar i den ursprungliga platsen.</span><span class="sxs-lookup"><span data-stu-id="aac03-153">The data is left in the original location.</span></span>

    * <span data-ttu-id="aac03-154">`ROW FORMAT`-Hur data ska formateras.</span><span class="sxs-lookup"><span data-stu-id="aac03-154">`ROW FORMAT` - How the data is formatted.</span></span> <span data-ttu-id="aac03-155">I det här fallet avgränsas fälten i varje logg med ett blanksteg.</span><span class="sxs-lookup"><span data-stu-id="aac03-155">In this case, the fields in each log are separated by a space.</span></span>

    * <span data-ttu-id="aac03-156">`STORED AS TEXTFILE LOCATION`-Där data lagras och i vilka filformat.</span><span class="sxs-lookup"><span data-stu-id="aac03-156">`STORED AS TEXTFILE LOCATION` - Where the data is stored and in what file format.</span></span>

    * <span data-ttu-id="aac03-157">`SELECT`-Väljer en uppräkning av alla rader där kolumnen **t4** innehåller värdet **[fel]**.</span><span class="sxs-lookup"><span data-stu-id="aac03-157">`SELECT` - Selects a count of all rows where column **t4** contains the value **[ERROR]**.</span></span> <span data-ttu-id="aac03-158">Den här frågan returnerar ett värde för **3** som det finns tre rader som innehåller det här värdet.</span><span class="sxs-lookup"><span data-stu-id="aac03-158">This query returns a value of **3** as there are three rows that contain this value.</span></span>

    * <span data-ttu-id="aac03-159">`INPUT__FILE__NAME LIKE '%.log'`-Hive försöker använda schemat för alla filer i katalogen.</span><span class="sxs-lookup"><span data-stu-id="aac03-159">`INPUT__FILE__NAME LIKE '%.log'` - Hive attempts to apply the schema to all files in the directory.</span></span> <span data-ttu-id="aac03-160">I det här fallet innehåller katalogen filer som inte matchar schemat.</span><span class="sxs-lookup"><span data-stu-id="aac03-160">In this case, the directory contains files that do not match the schema.</span></span> <span data-ttu-id="aac03-161">För att förhindra ogiltiga data i resultaten visar den här instruktionen Hive vi bör endast returnera data från filer som slutar på. log.</span><span class="sxs-lookup"><span data-stu-id="aac03-161">To prevent garbage data in the results, this statement tells Hive that we should only return data from files ending in .log.</span></span>

  > [!NOTE]
  > <span data-ttu-id="aac03-162">Externa tabeller ska användas när du förväntar dig underliggande data uppdateras av en extern källa.</span><span class="sxs-lookup"><span data-stu-id="aac03-162">External tables should be used when you expect the underlying data to be updated by an external source.</span></span> <span data-ttu-id="aac03-163">Till exempel en automatisk överföring av data eller en MapReduce-åtgärd.</span><span class="sxs-lookup"><span data-stu-id="aac03-163">For example, an automated data upload process or a MapReduce operation.</span></span>
  >
  > <span data-ttu-id="aac03-164">Släppa en extern tabell har **inte** ta bort data, endast tabelldefinitionen.</span><span class="sxs-lookup"><span data-stu-id="aac03-164">Dropping an external table does **not** delete the data, only the table definition.</span></span>

    <span data-ttu-id="aac03-165">Kommandots utdata liknar följande:</span><span class="sxs-lookup"><span data-stu-id="aac03-165">The output of this command is similar to the following text:</span></span>

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

5. <span data-ttu-id="aac03-166">Om du vill avsluta Beeline använder `!exit`.</span><span class="sxs-lookup"><span data-stu-id="aac03-166">To exit Beeline, use `!exit`.</span></span>

## <span data-ttu-id="aac03-167"><a id="file"></a>Använd Beeline för att köra en HiveQL-fil</span><span class="sxs-lookup"><span data-stu-id="aac03-167"><a id="file"></a>Use Beeline to run a HiveQL file</span></span>

<span data-ttu-id="aac03-168">Använd följande steg för att skapa en fil och kör den med hjälp av Beeline.</span><span class="sxs-lookup"><span data-stu-id="aac03-168">Use the following steps to create a file, then run it using Beeline.</span></span>

1. <span data-ttu-id="aac03-169">Använd följande kommando för att skapa en fil med namnet **query.hql**:</span><span class="sxs-lookup"><span data-stu-id="aac03-169">Use the following command to create a file named **query.hql**:</span></span>

    ```bash
    nano query.hql
    ```

2. <span data-ttu-id="aac03-170">Använd följande text som innehållet i filen.</span><span class="sxs-lookup"><span data-stu-id="aac03-170">Use the following text as the contents of the file.</span></span> <span data-ttu-id="aac03-171">Den här frågan skapar en ny ”interna” tabell med namnet **errorLogs**:</span><span class="sxs-lookup"><span data-stu-id="aac03-171">This query creates a new 'internal' table named **errorLogs**:</span></span>

    ```hiveql
    CREATE TABLE IF NOT EXISTS errorLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string) STORED AS ORC;
    INSERT OVERWRITE TABLE errorLogs SELECT t1, t2, t3, t4, t5, t6, t7 FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log';
    ```

    <span data-ttu-id="aac03-172">Dessa instruktioner utför följande åtgärder:</span><span class="sxs-lookup"><span data-stu-id="aac03-172">These statements perform the following actions:</span></span>

    * <span data-ttu-id="aac03-173">**Skapa tabell om inte finns** -om tabellen inte redan finns, skapas den.</span><span class="sxs-lookup"><span data-stu-id="aac03-173">**CREATE TABLE IF NOT EXISTS** - If the table does not already exist, it is created.</span></span> <span data-ttu-id="aac03-174">Eftersom den **externa** nyckelordet används inte, skapar en intern tabell för den här instruktionen.</span><span class="sxs-lookup"><span data-stu-id="aac03-174">Since the **EXTERNAL** keyword is not used, this statement creates an internal table.</span></span> <span data-ttu-id="aac03-175">Interna register lagras i datalagret Hive och hanteras helt av Hive.</span><span class="sxs-lookup"><span data-stu-id="aac03-175">Internal tables are stored in the Hive data warehouse and are managed completely by Hive.</span></span>
    * <span data-ttu-id="aac03-176">**LAGRAS AS ORC** -lagrar data i optimerad raden kolumner (ORC)-format.</span><span class="sxs-lookup"><span data-stu-id="aac03-176">**STORED AS ORC** - Stores the data in Optimized Row Columnar (ORC) format.</span></span> <span data-ttu-id="aac03-177">ORC är ett mycket optimerad och effektiv format för att lagra data med Hive.</span><span class="sxs-lookup"><span data-stu-id="aac03-177">ORC format is a highly optimized and efficient format for storing Hive data.</span></span>
    * <span data-ttu-id="aac03-178">**INFOGA ÖVER... Välj** -väljer rader från den **log4jLogs** tabellen som innehåller **[fel]**, infogar data till den **errorLogs** tabell.</span><span class="sxs-lookup"><span data-stu-id="aac03-178">**INSERT OVERWRITE ... SELECT** - Selects rows from the **log4jLogs** table that contain **[ERROR]**, then inserts the data into the **errorLogs** table.</span></span>

    > [!NOTE]
    > <span data-ttu-id="aac03-179">Till skillnad från externa tabeller kan släppa en intern tabell tar bort de underliggande data.</span><span class="sxs-lookup"><span data-stu-id="aac03-179">Unlike external tables, dropping an internal table deletes the underlying data as well.</span></span>

3. <span data-ttu-id="aac03-180">Om du vill spara filen, Använd **Ctrl**+**_X**, ange **Y**, och slutligen **RETUR**.</span><span class="sxs-lookup"><span data-stu-id="aac03-180">To save the file, use **Ctrl**+**_X**, then enter **Y**, and finally **Enter**.</span></span>

4. <span data-ttu-id="aac03-181">Använd följande för att köra filen med Beeline:</span><span class="sxs-lookup"><span data-stu-id="aac03-181">Use the following to run the file using Beeline:</span></span>

    ```bash
    beeline -u 'jdbc:hive2://headnodehost:10001/;transportMode=http' -i query.hql
    ```

    > [!NOTE]
    > <span data-ttu-id="aac03-182">Den `-i` parametern börjar Beeline, kör instruktionerna i filen query.hql.</span><span class="sxs-lookup"><span data-stu-id="aac03-182">The `-i` parameter starts Beeline, runs the statements in the query.hql file.</span></span> <span data-ttu-id="aac03-183">När frågan har slutförts kan du komma fram till den `jdbc:hive2://headnodehost:10001/>` prompt.</span><span class="sxs-lookup"><span data-stu-id="aac03-183">Once the query completes, you arrive at the `jdbc:hive2://headnodehost:10001/>` prompt.</span></span> <span data-ttu-id="aac03-184">Du kan också köra en fil med hjälp av den `-f` som avslutas Beeline när frågan har slutförts.</span><span class="sxs-lookup"><span data-stu-id="aac03-184">You can also run a file using the `-f` parameter, which exits Beeline after the query completes.</span></span>

5. <span data-ttu-id="aac03-185">Kontrollera att den **errorLogs** tabellen skapades, Använd följande-instruktion för att returnera alla rader från **errorLogs**:</span><span class="sxs-lookup"><span data-stu-id="aac03-185">To verify that the **errorLogs** table was created, use the following statement to return all the rows from **errorLogs**:</span></span>

    ```hiveql
    SELECT * from errorLogs;
    ```

    <span data-ttu-id="aac03-186">Tre raderna med data ska returneras, som innehåller alla **[fel]** i kolumnen t4:</span><span class="sxs-lookup"><span data-stu-id="aac03-186">Three rows of data should be returned, all containing **[ERROR]** in column t4:</span></span>

        +---------------+---------------+---------------+---------------+---------------+---------------+---------------+--+
        | errorlogs.t1  | errorlogs.t2  | errorlogs.t3  | errorlogs.t4  | errorlogs.t5  | errorlogs.t6  | errorlogs.t7  |
        +---------------+---------------+---------------+---------------+---------------+---------------+---------------+--+
        | 2012-02-03    | 18:35:34      | SampleClass0  | [ERROR]       | incorrect     | id            |               |
        | 2012-02-03    | 18:55:54      | SampleClass1  | [ERROR]       | incorrect     | id            |               |
        | 2012-02-03    | 19:25:27      | SampleClass4  | [ERROR]       | incorrect     | id            |               |
        +---------------+---------------+---------------+---------------+---------------+---------------+---------------+--+
        3 rows selected (1.538 seconds)

## <span data-ttu-id="aac03-187"><a id="remote"></a>Använd Beeline via fjärranslutning</span><span class="sxs-lookup"><span data-stu-id="aac03-187"><a id="remote"></a>Use Beeline remotely</span></span>

<span data-ttu-id="aac03-188">Om du har installerat lokalt Beeline eller använder den via en Docker-avbildning som [sutoiku/beeline](https://hub.docker.com/r/sutoiku/beeline/), måste du använda följande parametrar:</span><span class="sxs-lookup"><span data-stu-id="aac03-188">If you have Beeline installed locally, or are using it through a Docker image such as [sutoiku/beeline](https://hub.docker.com/r/sutoiku/beeline/), you must use the following parameters:</span></span>

* <span data-ttu-id="aac03-189">__Anslutningssträngen__:`-u 'jdbc:hive2://clustername.azurehdinsight.net:443/;ssl=true;transportMode=http;httpPath=/hive2'`</span><span class="sxs-lookup"><span data-stu-id="aac03-189">__Connection string__: `-u 'jdbc:hive2://clustername.azurehdinsight.net:443/;ssl=true;transportMode=http;httpPath=/hive2'`</span></span>

* <span data-ttu-id="aac03-190">__Klustrets inloggningsnamn__:`-n admin`</span><span class="sxs-lookup"><span data-stu-id="aac03-190">__Cluster login name__: `-n admin`</span></span>

* <span data-ttu-id="aac03-191">__Klustret inloggningslösenordet__`-p 'password'`</span><span class="sxs-lookup"><span data-stu-id="aac03-191">__Cluster login password__ `-p 'password'`</span></span>

<span data-ttu-id="aac03-192">Ersätt den `clustername` i anslutningssträngen med namnet på ditt HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="aac03-192">Replace the `clustername` in the connection string with the name of your HDInsight cluster.</span></span>

<span data-ttu-id="aac03-193">Ersätt `admin` med namnet på din klusterinloggning och Ersätt `password` med lösenordet för ditt klusterinloggning.</span><span class="sxs-lookup"><span data-stu-id="aac03-193">Replace `admin` with the name of your cluster login, and replace `password` with the password for your cluster login.</span></span>

## <span data-ttu-id="aac03-194"><a id="sparksql"></a>Använda Beeline med Spark</span><span class="sxs-lookup"><span data-stu-id="aac03-194"><a id="sparksql"></a>Use Beeline with Spark</span></span>

<span data-ttu-id="aac03-195">Spark innehåller sin egen implementering av HiveServer2, vilket är ofta kallade Spark Thrift-servern.</span><span class="sxs-lookup"><span data-stu-id="aac03-195">Spark provides its own implementation of HiveServer2, which is often refered to as the Spark Thrift server.</span></span> <span data-ttu-id="aac03-196">Den här tjänsten använder Spark SQL för att lösa frågor i stället för Hive och kan ge bättre prestanda beroende på din fråga.</span><span class="sxs-lookup"><span data-stu-id="aac03-196">This service uses Spark SQL to resolve queries instead of Hive, and may provide better performance depending on your query.</span></span>

<span data-ttu-id="aac03-197">Anslut till Spark Thrift-servern i ett Spark på HDInsight-kluster genom att använda port `10002` i stället för `10001`.</span><span class="sxs-lookup"><span data-stu-id="aac03-197">To connect to the Spark Thrift server of a Spark on HDInsight cluster, use port `10002` instead of `10001`.</span></span> <span data-ttu-id="aac03-198">Till exempel `beeline -u 'jdbc:hive2://headnodehost:10002/;transportMode=http'`.</span><span class="sxs-lookup"><span data-stu-id="aac03-198">For example, `beeline -u 'jdbc:hive2://headnodehost:10002/;transportMode=http'`.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="aac03-199">Spark Thrift-servern är inte tillgänglig direkt via internet.</span><span class="sxs-lookup"><span data-stu-id="aac03-199">The Spark Thrift server is not directly accessible over the internet.</span></span> <span data-ttu-id="aac03-200">Du kan bara ansluta till den från en SSH-session eller i Azure samma virtuella nätverk som HDInsight-klustret.</span><span class="sxs-lookup"><span data-stu-id="aac03-200">You can only connect to it from an SSH session or inside the same Azure Virtual Network as the HDInsight cluster.</span></span>

## <span data-ttu-id="aac03-201"><a id="summary"></a><a id="nextsteps"></a>Nästa steg</span><span class="sxs-lookup"><span data-stu-id="aac03-201"><a id="summary"></a><a id="nextsteps"></a>Next steps</span></span>

<span data-ttu-id="aac03-202">Mer allmän information om Hive i HDInsight finns i följande dokument:</span><span class="sxs-lookup"><span data-stu-id="aac03-202">For more general information on Hive in HDInsight, see the following document:</span></span>

* [<span data-ttu-id="aac03-203">Använda Hive med Hadoop i HDInsight</span><span class="sxs-lookup"><span data-stu-id="aac03-203">Use Hive with Hadoop on HDInsight</span></span>](hdinsight-use-hive.md)

<span data-ttu-id="aac03-204">Mer information om andra sätt kan du arbeta med Hadoop i HDInsight finns i följande dokument:</span><span class="sxs-lookup"><span data-stu-id="aac03-204">For more information on other ways you can work with Hadoop on HDInsight, see the following documents:</span></span>

* [<span data-ttu-id="aac03-205">Använda Pig med Hadoop i HDInsight</span><span class="sxs-lookup"><span data-stu-id="aac03-205">Use Pig with Hadoop on HDInsight</span></span>](hdinsight-use-pig.md)
* [<span data-ttu-id="aac03-206">Använda MapReduce med Hadoop i HDInsight</span><span class="sxs-lookup"><span data-stu-id="aac03-206">Use MapReduce with Hadoop on HDInsight</span></span>](hdinsight-use-mapreduce.md)

<span data-ttu-id="aac03-207">Om du använder Tez med Hive finns i följande dokument:</span><span class="sxs-lookup"><span data-stu-id="aac03-207">If you are using Tez with Hive, see the following documents:</span></span>

* [<span data-ttu-id="aac03-208">Använda Tez-Användargränssnittet på Windows-baserade HDInsight</span><span class="sxs-lookup"><span data-stu-id="aac03-208">Use the Tez UI on Windows-based HDInsight</span></span>](hdinsight-debug-tez-ui.md)
* [<span data-ttu-id="aac03-209">Använd vyn Ambari Tez på Linux-baserat HDInsight</span><span class="sxs-lookup"><span data-stu-id="aac03-209">Use the Ambari Tez view on Linux-based HDInsight</span></span>](hdinsight-debug-ambari-tez-view.md)

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
