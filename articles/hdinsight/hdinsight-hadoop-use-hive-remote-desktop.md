---
title: "Använda Hadoop Hive och fjärrskrivbord i HDInsight - Azure | Microsoft Docs"
description: "Lär dig hur du ansluter till Hadoop-kluster i HDInsight med hjälp av fjärrskrivbord och köra Hive-frågor med hjälp av kommandoradsgränssnittet Hive."
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
ms.openlocfilehash: 187c7cb413b3707e58eea387857375053d267189
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="use-hive-with-hadoop-on-hdinsight-with-remote-desktop"></a><span data-ttu-id="12e1a-103">Använda Hive med Hadoop i HDInsight med fjärrskrivbord</span><span class="sxs-lookup"><span data-stu-id="12e1a-103">Use Hive with Hadoop on HDInsight with Remote Desktop</span></span>
[!INCLUDE [hive-selector](../../includes/hdinsight-selector-use-hive.md)]

<span data-ttu-id="12e1a-104">I den här artikeln kommer du lär dig hur du ansluter till ett HDInsight-kluster med hjälp av fjärrskrivbord och köra Hive-frågor med hjälp av Hive kommandoradsgränssnittet (CLI).</span><span class="sxs-lookup"><span data-stu-id="12e1a-104">In this article, you will learn how to connect to an HDInsight cluster by using Remote Desktop, and then run Hive queries by using the Hive Command-Line Interface (CLI).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="12e1a-105">Fjärrskrivbord är bara tillgängligt på HDInsight-kluster som använder Windows som operativsystem.</span><span class="sxs-lookup"><span data-stu-id="12e1a-105">Remote Desktop is only available on HDInsight clusters that use Windows as the operating system.</span></span> <span data-ttu-id="12e1a-106">Linux är det enda operativsystemet som används med HDInsight version 3.4 och senare.</span><span class="sxs-lookup"><span data-stu-id="12e1a-106">Linux is the only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="12e1a-107">Mer information finns i [HDInsight-avveckling på Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="12e1a-107">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>
>
> <span data-ttu-id="12e1a-108">För HDInsight 3.4 eller större finns [använda Hive med HDInsight och Beeline](hdinsight-hadoop-use-hive-beeline.md) information om hur du kör Hive-frågor direkt på klustret från en kommandorad.</span><span class="sxs-lookup"><span data-stu-id="12e1a-108">For HDInsight 3.4 or greater, see [Use Hive with HDInsight and Beeline](hdinsight-hadoop-use-hive-beeline.md) for information on running Hive queries directly on the cluster from a command-line.</span></span>

## <span data-ttu-id="12e1a-109"><a id="prereq"></a>Förhandskrav</span><span class="sxs-lookup"><span data-stu-id="12e1a-109"><a id="prereq"></a>Prerequisites</span></span>
<span data-ttu-id="12e1a-110">Du behöver följande för att slutföra stegen i den här artikeln:</span><span class="sxs-lookup"><span data-stu-id="12e1a-110">To complete the steps in this article, you will need the following:</span></span>

* <span data-ttu-id="12e1a-111">Ett kluster med Windows-baserade HDInsight (Hadoop på HDInsight)</span><span class="sxs-lookup"><span data-stu-id="12e1a-111">A Windows-based HDInsight (Hadoop on HDInsight) cluster</span></span>
* <span data-ttu-id="12e1a-112">En klientdator som kör Windows 10, Windows 8 eller Windows 7</span><span class="sxs-lookup"><span data-stu-id="12e1a-112">A client computer running Windows 10, Window 8, or Windows 7</span></span>

## <span data-ttu-id="12e1a-113"><a id="connect"></a>Ansluta med fjärrskrivbord</span><span class="sxs-lookup"><span data-stu-id="12e1a-113"><a id="connect"></a>Connect with Remote Desktop</span></span>
<span data-ttu-id="12e1a-114">Aktivera Fjärrskrivbord för HDInsight-klustret och sedan ansluta till den genom att följa anvisningarna i [Anslut till HDInsight-kluster med RDP](hdinsight-administer-use-management-portal.md#connect-to-clusters-using-rdp).</span><span class="sxs-lookup"><span data-stu-id="12e1a-114">Enable Remote Desktop for the HDInsight cluster, then connect to it by following the instructions at [Connect to HDInsight clusters using RDP](hdinsight-administer-use-management-portal.md#connect-to-clusters-using-rdp).</span></span>

## <span data-ttu-id="12e1a-115"><a id="hive"></a>Använd kommandot Hive</span><span class="sxs-lookup"><span data-stu-id="12e1a-115"><a id="hive"></a>Use the Hive command</span></span>
<span data-ttu-id="12e1a-116">När du har anslutit till skrivbordet för HDInsight-kluster, kan du använda följande steg för att arbeta med Hive:</span><span class="sxs-lookup"><span data-stu-id="12e1a-116">When you have connected to the desktop for the HDInsight cluster, use the following steps to work with Hive:</span></span>

1. <span data-ttu-id="12e1a-117">HDInsight-skrivbordet och starta den **Hadoop kommandoraden**.</span><span class="sxs-lookup"><span data-stu-id="12e1a-117">From the HDInsight desktop, start the **Hadoop Command Line**.</span></span>
2. <span data-ttu-id="12e1a-118">Ange följande kommando för att starta Hive-CLI:</span><span class="sxs-lookup"><span data-stu-id="12e1a-118">Enter the following command to start the Hive CLI:</span></span>

        %hive_home%\bin\hive

    <span data-ttu-id="12e1a-119">När CLI har startats visas CLI Hive-fråga: `hive>`.</span><span class="sxs-lookup"><span data-stu-id="12e1a-119">When the CLI has started, you will see the Hive CLI prompt: `hive>`.</span></span>
3. <span data-ttu-id="12e1a-120">Med hjälp av CLI, ange följande instruktioner för att skapa en ny tabell med namnet **log4jLogs** med exempeldata:</span><span class="sxs-lookup"><span data-stu-id="12e1a-120">Using the CLI, enter the following statements to create a new table named **log4jLogs** using sample data:</span></span>

        set hive.execution.engine=tez;
        DROP TABLE log4jLogs;
        CREATE EXTERNAL TABLE log4jLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string)
        ROW FORMAT DELIMITED FIELDS TERMINATED BY ' '
        STORED AS TEXTFILE LOCATION 'wasb:///example/data/';
        SELECT t4 AS sev, COUNT(*) AS count FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log' GROUP BY t4;

    <span data-ttu-id="12e1a-121">Dessa instruktioner utför följande åtgärder:</span><span class="sxs-lookup"><span data-stu-id="12e1a-121">These statements perform the following actions:</span></span>

   * <span data-ttu-id="12e1a-122">**DROP TABLE**: tar bort tabellen och datafilen om tabellen redan finns.</span><span class="sxs-lookup"><span data-stu-id="12e1a-122">**DROP TABLE**: Deletes the table and the data file if the table already exists.</span></span>
   * <span data-ttu-id="12e1a-123">**Skapa extern tabell**: skapar en ny ”externa” tabell i Hive.</span><span class="sxs-lookup"><span data-stu-id="12e1a-123">**CREATE EXTERNAL TABLE**: Creates a new 'external' table in Hive.</span></span> <span data-ttu-id="12e1a-124">Externa tabeller lagra endast tabelldefinitionen i Hive (data finns kvar i den ursprungliga platsen).</span><span class="sxs-lookup"><span data-stu-id="12e1a-124">External tables store only the table definition in Hive (the data is left in the original location).</span></span>

     > [!NOTE]
     > <span data-ttu-id="12e1a-125">Externa tabeller ska användas när du förväntar dig underliggande data uppdateras av en extern källa (till exempel en automatisk överföring av data) eller av en annan MapReduce-åtgärd, men du vill använda Hive-frågor för att använda den senaste informationen.</span><span class="sxs-lookup"><span data-stu-id="12e1a-125">External tables should be used when you expect the underlying data to be updated by an external source (such as an automated data upload process) or by another MapReduce operation, but you always want Hive queries to use the latest data.</span></span>
     >
     > <span data-ttu-id="12e1a-126">Släppa en extern tabell har **inte** ta bort data, endast tabelldefinitionen.</span><span class="sxs-lookup"><span data-stu-id="12e1a-126">Dropping an external table does **not** delete the data, only the table definition.</span></span>
     >
     >
   * <span data-ttu-id="12e1a-127">**RADEN FORMAT**: talar om Hive hur data ska formateras.</span><span class="sxs-lookup"><span data-stu-id="12e1a-127">**ROW FORMAT**: Tells Hive how the data is formatted.</span></span> <span data-ttu-id="12e1a-128">I det här fallet avgränsas fälten i varje logg med ett blanksteg.</span><span class="sxs-lookup"><span data-stu-id="12e1a-128">In this case, the fields in each log are separated by a space.</span></span>
   * <span data-ttu-id="12e1a-129">**LAGRAS AS TEXTFILE plats**: talar om Hive där data är lagras (exempel/datakatalog) och som den lagras som text.</span><span class="sxs-lookup"><span data-stu-id="12e1a-129">**STORED AS TEXTFILE LOCATION**: Tells Hive where the data is stored (the example/data directory) and that it is stored as text.</span></span>
   * <span data-ttu-id="12e1a-130">**Välj**: väljer en uppräkning av alla rader där kolumnen **t4** innehåller värdet **[fel]**.</span><span class="sxs-lookup"><span data-stu-id="12e1a-130">**SELECT**: Selects a count of all rows where column **t4** contains the value **[ERROR]**.</span></span> <span data-ttu-id="12e1a-131">Detta bör returnera ett värde av **3** eftersom det finns tre rader som innehåller det här värdet.</span><span class="sxs-lookup"><span data-stu-id="12e1a-131">This should return a value of **3** because there are three rows that contain this value.</span></span>
   * <span data-ttu-id="12e1a-132">**INPUT__FILE__NAME som '%.log'** -talar om Hive som vi ska bara returnera data från filer som slutar på. log.</span><span class="sxs-lookup"><span data-stu-id="12e1a-132">**INPUT__FILE__NAME LIKE '%.log'** - Tells Hive that we should only return data from files ending in .log.</span></span> <span data-ttu-id="12e1a-133">Detta begränsar sökningen till sample.log-filen som innehåller data och håller den från att returnera data från andra exempel filer som inte matchar det schema som vi har definierat.</span><span class="sxs-lookup"><span data-stu-id="12e1a-133">This restricts the search to the sample.log file that contains the data, and keeps it from returning data from other example data files that do not match the schema we defined.</span></span>
4. <span data-ttu-id="12e1a-134">Använd följande instruktioner för att skapa en ny ”interna” tabell med namnet **errorLogs**:</span><span class="sxs-lookup"><span data-stu-id="12e1a-134">Use the following statements to create a new 'internal' table named **errorLogs**:</span></span>

        CREATE TABLE IF NOT EXISTS errorLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string) STORED AS ORC;
        INSERT OVERWRITE TABLE errorLogs SELECT t1, t2, t3, t4, t5, t6, t7 FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log';

    <span data-ttu-id="12e1a-135">Dessa instruktioner utför följande åtgärder:</span><span class="sxs-lookup"><span data-stu-id="12e1a-135">These statements perform the following actions:</span></span>

   * <span data-ttu-id="12e1a-136">**Skapa tabell om inte finns**: skapar en tabell om den inte redan finns.</span><span class="sxs-lookup"><span data-stu-id="12e1a-136">**CREATE TABLE IF NOT EXISTS**: Creates a table if it does not already exist.</span></span> <span data-ttu-id="12e1a-137">Eftersom den **externa** nyckelordet används inte, det här är en intern tabell som lagras i datalagret Hive och hanteras helt av Hive.</span><span class="sxs-lookup"><span data-stu-id="12e1a-137">Because the **EXTERNAL** keyword is not used, this is an internal table, which is stored in the Hive data warehouse and is managed completely by Hive.</span></span>

     > [!NOTE]
     > <span data-ttu-id="12e1a-138">Till skillnad från **externa** tabeller, släppa en intern tabell även tar bort den underliggande data.</span><span class="sxs-lookup"><span data-stu-id="12e1a-138">Unlike **EXTERNAL** tables, dropping an internal table also deletes the underlying data.</span></span>
     >
     >
   * <span data-ttu-id="12e1a-139">**LAGRAS AS ORC**: lagrar data i optimerad raden (ORC) kolumnformat.</span><span class="sxs-lookup"><span data-stu-id="12e1a-139">**STORED AS ORC**: Stores the data in optimized row columnar (ORC) format.</span></span> <span data-ttu-id="12e1a-140">Detta är ett mycket optimerad och effektiv format för att lagra data med Hive.</span><span class="sxs-lookup"><span data-stu-id="12e1a-140">This is a highly optimized and efficient format for storing Hive data.</span></span>
   * <span data-ttu-id="12e1a-141">**INFOGA ÖVER... Välj**: väljer rader från den **log4jLogs** tabellen som innehåller **[fel]**, infogar data till den **errorLogs** tabell.</span><span class="sxs-lookup"><span data-stu-id="12e1a-141">**INSERT OVERWRITE ... SELECT**: Selects rows from the **log4jLogs** table that contain **[ERROR]**, then inserts the data into the **errorLogs** table.</span></span>

     <span data-ttu-id="12e1a-142">Kontrollera att endast rader som innehåller **[fel]** i kolumnen t4 har lagrats till den **errorLogs** tabell, använder du följande instruktion returnera alla rader från **errorLogs**:</span><span class="sxs-lookup"><span data-stu-id="12e1a-142">To verify that only rows that contain **[ERROR]** in column t4 were stored to the **errorLogs** table, use the following statement to return all the rows from **errorLogs**:</span></span>

       <span data-ttu-id="12e1a-143">Välj * från errorLogs;</span><span class="sxs-lookup"><span data-stu-id="12e1a-143">SELECT * from errorLogs;</span></span>

     <span data-ttu-id="12e1a-144">Tre raderna med data ska returneras, som innehåller alla **[fel]** i kolumnen t4.</span><span class="sxs-lookup"><span data-stu-id="12e1a-144">Three rows of data should be returned, all containing **[ERROR]** in column t4.</span></span>

## <span data-ttu-id="12e1a-145"><a id="summary"></a>Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="12e1a-145"><a id="summary"></a>Summary</span></span>
<span data-ttu-id="12e1a-146">Som du ser i Hive-kommandot ger ett enkelt sätt att köra Hive-frågor på ett HDInsight-kluster, övervaka jobbstatus och hämta utdata interaktivt.</span><span class="sxs-lookup"><span data-stu-id="12e1a-146">As you can see, the the Hive command provides an easy way to interactively run Hive queries on an HDInsight cluster, monitor the job status, and retrieve the output.</span></span>

## <span data-ttu-id="12e1a-147"><a id="nextsteps"></a>Nästa steg</span><span class="sxs-lookup"><span data-stu-id="12e1a-147"><a id="nextsteps"></a>Next steps</span></span>
<span data-ttu-id="12e1a-148">Allmän information om Hive i HDInsight:</span><span class="sxs-lookup"><span data-stu-id="12e1a-148">For general information about Hive in HDInsight:</span></span>

* [<span data-ttu-id="12e1a-149">Använda Hive med Hadoop i HDInsight</span><span class="sxs-lookup"><span data-stu-id="12e1a-149">Use Hive with Hadoop on HDInsight</span></span>](hdinsight-use-hive.md)

<span data-ttu-id="12e1a-150">Information om andra sätt kan du arbeta med Hadoop i HDInsight:</span><span class="sxs-lookup"><span data-stu-id="12e1a-150">For information about other ways you can work with Hadoop on HDInsight:</span></span>

* [<span data-ttu-id="12e1a-151">Använda Pig med Hadoop i HDInsight</span><span class="sxs-lookup"><span data-stu-id="12e1a-151">Use Pig with Hadoop on HDInsight</span></span>](hdinsight-use-pig.md)
* [<span data-ttu-id="12e1a-152">Använda MapReduce med Hadoop i HDInsight</span><span class="sxs-lookup"><span data-stu-id="12e1a-152">Use MapReduce with Hadoop on HDInsight</span></span>](hdinsight-use-mapreduce.md)

<span data-ttu-id="12e1a-153">Om du använder Tez med Hive finns i följande dokument för felsökningsinformation:</span><span class="sxs-lookup"><span data-stu-id="12e1a-153">If you are using Tez with Hive, see the following documents for debugging information:</span></span>

* [<span data-ttu-id="12e1a-154">Använda Tez-Användargränssnittet på Windows-baserade HDInsight</span><span class="sxs-lookup"><span data-stu-id="12e1a-154">Use the Tez UI on Windows-based HDInsight</span></span>](hdinsight-debug-tez-ui.md)
* [<span data-ttu-id="12e1a-155">Använd vyn Ambari Tez på Linux-baserat HDInsight</span><span class="sxs-lookup"><span data-stu-id="12e1a-155">Use the Ambari Tez view on Linux-based HDInsight</span></span>](hdinsight-debug-ambari-tez-view.md)

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
