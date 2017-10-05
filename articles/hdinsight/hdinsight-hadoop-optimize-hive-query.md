---
title: "Optimera Hive-frågor i Azure HDInsight | Microsoft Docs"
description: "Lär dig hur du optimerar dina Hive-frågor för Hadoop i HDInsight."
services: hdinsight
documentationcenter: 
author: mumian
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: d6174c08-06aa-42ac-8e9b-8b8718d9978e
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 04/26/2016
ms.author: jgao
ms.openlocfilehash: edbf797e6277a65b5311e4939f5ab72776b11557
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="optimize-hive-queries-in-azure-hdinsight"></a><span data-ttu-id="6d5ca-103">Optimera Hive-frågor i Azure HDInsight</span><span class="sxs-lookup"><span data-stu-id="6d5ca-103">Optimize Hive queries in Azure HDInsight</span></span>

<span data-ttu-id="6d5ca-104">Hadoop-kluster är som standard inte optimerats för prestanda.</span><span class="sxs-lookup"><span data-stu-id="6d5ca-104">By default, Hadoop clusters are not optimized for performance.</span></span> <span data-ttu-id="6d5ca-105">Den här artikeln beskriver vissa vanligaste Hive prestanda optimering-metoder som du kan tillämpa på dina frågor.</span><span class="sxs-lookup"><span data-stu-id="6d5ca-105">This article covers some most common Hive performance optimization methods that you can apply to your queries.</span></span>

## <a name="scale-out-worker-nodes"></a><span data-ttu-id="6d5ca-106">Skala ut arbetsnoder</span><span class="sxs-lookup"><span data-stu-id="6d5ca-106">Scale out worker nodes</span></span>

<span data-ttu-id="6d5ca-107">Öka antalet arbetarnoder i ett kluster kan utnyttja mer mappers och förminskningsapparater köras parallellt.</span><span class="sxs-lookup"><span data-stu-id="6d5ca-107">Increasing the number of worker nodes in a cluster can leverage more mappers and reducers to be run in parallel.</span></span> <span data-ttu-id="6d5ca-108">Det finns två sätt som du kan öka skala ut i HDInsight:</span><span class="sxs-lookup"><span data-stu-id="6d5ca-108">There are two ways you can increase scale out in HDInsight:</span></span>

* <span data-ttu-id="6d5ca-109">Du kan ange antalet arbetarnoder med Azure-portalen, Azure PowerShell eller plattformsoberoende kommandoradsgränssnittet när etablera.</span><span class="sxs-lookup"><span data-stu-id="6d5ca-109">At the provision time, you can specify the number of worker nodes using the Azure portal, Azure PowerShell, or Cross-platform command-line interface.</span></span>  <span data-ttu-id="6d5ca-110">Mer information finns i [Skapa HDInsight-kluster](hdinsight-hadoop-provision-linux-clusters.md).</span><span class="sxs-lookup"><span data-stu-id="6d5ca-110">For more information, see [Create HDInsight clusters](hdinsight-hadoop-provision-linux-clusters.md).</span></span> <span data-ttu-id="6d5ca-111">Följande skärmbild visar worker nodkonfiguration på Azure portal:</span><span class="sxs-lookup"><span data-stu-id="6d5ca-111">The following screenshot shows the worker node configuration on the Azure portal:</span></span>
  
    ![scaleout_1][image-hdi-optimize-hive-scaleout_1]
* <span data-ttu-id="6d5ca-113">Vid körning, kan du även skala ut ett kluster utan att återskapa en:</span><span class="sxs-lookup"><span data-stu-id="6d5ca-113">At the run time, you can also scale out a cluster without recreating one:</span></span>

    ![scaleout_1][image-hdi-optimize-hive-scaleout_2]

<span data-ttu-id="6d5ca-115">Läs mer om de olika virtuella datorer som stöds av HDInsight [HDInsight priser](https://azure.microsoft.com/pricing/details/hdinsight/).</span><span class="sxs-lookup"><span data-stu-id="6d5ca-115">For more information about the different virtual machines supported by HDInsight, see [HDInsight pricing](https://azure.microsoft.com/pricing/details/hdinsight/).</span></span>

## <a name="enable-tez"></a><span data-ttu-id="6d5ca-116">Aktivera Tez</span><span class="sxs-lookup"><span data-stu-id="6d5ca-116">Enable Tez</span></span>

<span data-ttu-id="6d5ca-117">[Apache Tez](http://hortonworks.com/hadoop/tez/) är en alternativ Körningsmotor för motorn MapReduce:</span><span class="sxs-lookup"><span data-stu-id="6d5ca-117">[Apache Tez](http://hortonworks.com/hadoop/tez/) is an alternative execution engine to the MapReduce engine:</span></span>

![tez_1][image-hdi-optimize-hive-tez_1]

<span data-ttu-id="6d5ca-119">Tez går snabbare eftersom:</span><span class="sxs-lookup"><span data-stu-id="6d5ca-119">Tez is faster because:</span></span>

* <span data-ttu-id="6d5ca-120">**Köra dirigeras acykliska diagram (DAG) som ett enda jobb i motorn MapReduce**.</span><span class="sxs-lookup"><span data-stu-id="6d5ca-120">**Execute Directed Acyclic Graph (DAG) as a single job in the MapReduce engine**.</span></span> <span data-ttu-id="6d5ca-121">Gruppen för Databastillgänglighet kräver varje uppsättning mappers ska följas av en uppsättning förminskningsapparater.</span><span class="sxs-lookup"><span data-stu-id="6d5ca-121">The DAG requires each set of mappers to be followed by one set of reducers.</span></span> <span data-ttu-id="6d5ca-122">Detta medför flera MapReduce-jobb kan vara roterade för varje Hive-fråga.</span><span class="sxs-lookup"><span data-stu-id="6d5ca-122">This causes multiple MapReduce jobs to be spun off for each Hive query.</span></span> <span data-ttu-id="6d5ca-123">Tez har inte sådan begränsning och kan bearbeta komplexa DAG som ett jobb, vilket minimerar jobbet startade kostnader.</span><span class="sxs-lookup"><span data-stu-id="6d5ca-123">Tez does not have such constraint and can process complex DAG as one job thus minimizing job startup overhead.</span></span>
* <span data-ttu-id="6d5ca-124">**Undviker onödig skrivningar**.</span><span class="sxs-lookup"><span data-stu-id="6d5ca-124">**Avoids unnecessary writes**.</span></span> <span data-ttu-id="6d5ca-125">På grund av flera jobb som skapas för samma Hive-fråga i motorn MapReduce, skrivs utdata från varje jobb till HDFS för mellanliggande data.</span><span class="sxs-lookup"><span data-stu-id="6d5ca-125">Due to multiple jobs being spun for the same Hive query in the MapReduce engine, the output of each job is written to HDFS for intermediate data.</span></span> <span data-ttu-id="6d5ca-126">Eftersom Tez minimerar antalet jobb för varje Hive-fråga är det kunna undvika onödiga skrivåtgärder.</span><span class="sxs-lookup"><span data-stu-id="6d5ca-126">Since Tez minimizes number of jobs for each Hive query it is able to avoid unnecessary write.</span></span>
* <span data-ttu-id="6d5ca-127">**Minimerar uppstart fördröjningar**.</span><span class="sxs-lookup"><span data-stu-id="6d5ca-127">**Minimizes start-up delays**.</span></span> <span data-ttu-id="6d5ca-128">Tez är bättre att minimera uppstart fördröjning genom att minska antalet mappers som behövs för att starta och även förbättra optimering i hela.</span><span class="sxs-lookup"><span data-stu-id="6d5ca-128">Tez is better able to minimize start-up delay by reducing the number of mappers it needs to start and also improving optimization throughout.</span></span>
* <span data-ttu-id="6d5ca-129">**Återanvänder behållare**.</span><span class="sxs-lookup"><span data-stu-id="6d5ca-129">**Reuses containers**.</span></span> <span data-ttu-id="6d5ca-130">När möjliga Tez kan återanvända behållare för att säkerställa att fördröjning på grund av att starta behållare minskas.</span><span class="sxs-lookup"><span data-stu-id="6d5ca-130">Whenever possible Tez is able to reuse containers to ensure that latency due to starting up containers is reduced.</span></span>
* <span data-ttu-id="6d5ca-131">**Kontinuerlig optimeringstekniker**.</span><span class="sxs-lookup"><span data-stu-id="6d5ca-131">**Continuous optimization techniques**.</span></span> <span data-ttu-id="6d5ca-132">Traditionellt gjordes optimering under kompilering fasen.</span><span class="sxs-lookup"><span data-stu-id="6d5ca-132">Traditionally optimization was done during compilation phase.</span></span> <span data-ttu-id="6d5ca-133">Men mer information om den angivna informationen är tillgänglig som möjliggör bättre optimering under körningen.</span><span class="sxs-lookup"><span data-stu-id="6d5ca-133">However more information about the inputs is available that allow for better optimization during runtime.</span></span> <span data-ttu-id="6d5ca-134">Tez använder kontinuerlig optimeringstekniker som gör att de kan optimera plan i runtime-fasen.</span><span class="sxs-lookup"><span data-stu-id="6d5ca-134">Tez uses continuous optimization techniques that allows it to optimize the plan further into the runtime phase.</span></span>

<span data-ttu-id="6d5ca-135">Mer information om dessa koncept finns [Apache TEZ](http://hortonworks.com/hadoop/tez/).</span><span class="sxs-lookup"><span data-stu-id="6d5ca-135">For more details on these concepts, see [Apache TEZ](http://hortonworks.com/hadoop/tez/).</span></span>

<span data-ttu-id="6d5ca-136">Du kan göra en Hive-fråga Tez aktiverad med frågan med inställningen nedan:</span><span class="sxs-lookup"><span data-stu-id="6d5ca-136">You can make any Hive query Tez enabled by prefixing the query with the setting below:</span></span>

    set hive.execution.engine=tez;

<span data-ttu-id="6d5ca-137">Linux-baserade HDInsight-kluster har Tez aktiverad som standard.</span><span class="sxs-lookup"><span data-stu-id="6d5ca-137">Linux-based HDInsight clusters have Tez enabled by default.</span></span>


## <a name="hive-partitioning"></a><span data-ttu-id="6d5ca-138">Hive partitionering</span><span class="sxs-lookup"><span data-stu-id="6d5ca-138">Hive partitioning</span></span>

<span data-ttu-id="6d5ca-139">I/o-åtgärden är större flaskhals för att köra Hive-frågor.</span><span class="sxs-lookup"><span data-stu-id="6d5ca-139">I/O operation is the major performance bottleneck for running Hive queries.</span></span> <span data-ttu-id="6d5ca-140">Prestanda kan förbättras om du kan minska mängden data som ska läsas.</span><span class="sxs-lookup"><span data-stu-id="6d5ca-140">The performance can be improved if the amount of data that needs to be read can be reduced.</span></span> <span data-ttu-id="6d5ca-141">Sök igenom hela Hive-tabeller som standard Hive-frågor.</span><span class="sxs-lookup"><span data-stu-id="6d5ca-141">By default, Hive queries scan entire Hive tables.</span></span> <span data-ttu-id="6d5ca-142">Det här är bra för frågor som tabellsökningar.</span><span class="sxs-lookup"><span data-stu-id="6d5ca-142">This is great for queries like table scans.</span></span> <span data-ttu-id="6d5ca-143">Men för frågor som behöver bara söka igenom en liten mängd data (till exempel frågor med filtrering), detta skapar onödiga omkostnader.</span><span class="sxs-lookup"><span data-stu-id="6d5ca-143">However for queries that only need to scan a small amount of data (for example, queries with filtering), this behavior creates unnecessary overhead.</span></span> <span data-ttu-id="6d5ca-144">Hive partitionering gör Hive-frågor att få åtkomst till endast nödvändiga mängden data i Hive-tabeller.</span><span class="sxs-lookup"><span data-stu-id="6d5ca-144">Hive partitioning allows Hive queries to access only the necessary amount of data in Hive tables.</span></span>

<span data-ttu-id="6d5ca-145">Hive partitionering implementeras genom att ordna rådata i nya kataloger med varje partition har en egen katalog - där den definieras av användaren.</span><span class="sxs-lookup"><span data-stu-id="6d5ca-145">Hive partitioning is implemented by reorganizing the raw data into new directories with each partition having its own directory - where the partition is defined by the user.</span></span> <span data-ttu-id="6d5ca-146">Följande diagram illustrerar partitionering en Hive-tabell som kolumnen *år*.</span><span class="sxs-lookup"><span data-stu-id="6d5ca-146">The following diagram illustrates partitioning a Hive table by the column *Year*.</span></span> <span data-ttu-id="6d5ca-147">En ny katalog skapas för varje år.</span><span class="sxs-lookup"><span data-stu-id="6d5ca-147">A new directory is created for each year.</span></span>

![Partitionering][image-hdi-optimize-hive-partitioning_1]

<span data-ttu-id="6d5ca-149">Vissa partitionering att tänka på:</span><span class="sxs-lookup"><span data-stu-id="6d5ca-149">Some partitioning considerations:</span></span>

* <span data-ttu-id="6d5ca-150">**Gör inte under partition** -partitionering på kolumner med bara några värden kan orsaka några partitioner.</span><span class="sxs-lookup"><span data-stu-id="6d5ca-150">**Do not under partition** - Partitioning on columns with only a few values can cause few partitions.</span></span> <span data-ttu-id="6d5ca-151">Till exempel skapar partitionering på kön bara två partitioner som ska skapas (han och hondjur) kan därför endast minska fördröjningen med högst hälften.</span><span class="sxs-lookup"><span data-stu-id="6d5ca-151">For example, partitioning on gender only creates two partitions to be created (male and female), thus only reduce the latency by a maximum of half.</span></span>
* <span data-ttu-id="6d5ca-152">**Gör inte över partition** , på andra extreme, skapa en partition på en kolumn med ett unikt värde (t.ex, användar-ID) medför flera partitioner.</span><span class="sxs-lookup"><span data-stu-id="6d5ca-152">**Do not over partition** - On the other extreme, creating a partition on a column with a unique value (for example, userid) causes multiple partitions.</span></span> <span data-ttu-id="6d5ca-153">Över partitionen gör att mycket belastningen på klustret namenode eftersom den har att hantera ett stort antal kataloger.</span><span class="sxs-lookup"><span data-stu-id="6d5ca-153">Over partition causes much stress on the cluster namenode as it has to handle the large number of directories.</span></span>
* <span data-ttu-id="6d5ca-154">**Undvika data skeva** -Välj din partitionsnyckel klokt så att alla partitioner har även storlek.</span><span class="sxs-lookup"><span data-stu-id="6d5ca-154">**Avoid data skew** - Choose your partitioning key wisely so that all partitions are even size.</span></span> <span data-ttu-id="6d5ca-155">Ett exempel partitionering på *tillstånd* kan orsaka antalet poster under California ska nästan 30 x som Vermont på grund av skillnader i populationen.</span><span class="sxs-lookup"><span data-stu-id="6d5ca-155">An example is partitioning on *State* may cause the number of records under California to be almost 30x that of Vermont due to the difference in population.</span></span>

<span data-ttu-id="6d5ca-156">Så här skapar du en partitionstabell den *partitioneras av* sats:</span><span class="sxs-lookup"><span data-stu-id="6d5ca-156">To create a partition table, use the *Partitioned By* clause:</span></span>

    CREATE TABLE lineitem_part
        (L_ORDERKEY INT, L_PARTKEY INT, L_SUPPKEY INT,L_LINENUMBER INT,
         L_QUANTITY DOUBLE, L_EXTENDEDPRICE DOUBLE, L_DISCOUNT DOUBLE,
         L_TAX DOUBLE, L_RETURNFLAG STRING, L_LINESTATUS STRING,
         L_SHIPDATE_PS STRING, L_COMMITDATE STRING, L_RECEIPTDATE            STRING, L_SHIPINSTRUCT STRING, L_SHIPMODE STRING,
         L_COMMENT STRING)
    PARTITIONED BY(L_SHIPDATE STRING)
    ROW FORMAT DELIMITED FIELDS TERMINATED BY '\t'
    STORED AS TEXTFILE;

<span data-ttu-id="6d5ca-157">Du kan antingen skapa partitionering statisk eller dynamisk partitionering när partitionerade tabellen har skapats.</span><span class="sxs-lookup"><span data-stu-id="6d5ca-157">Once the partitioned table is created, you can either create static partitioning or dynamic partitioning.</span></span>

* <span data-ttu-id="6d5ca-158">**Statisk partitionering** innebär att du har redan shardade data i rätt kataloger och du kan be Hive partitioner manuellt utifrån katalogplatsen.</span><span class="sxs-lookup"><span data-stu-id="6d5ca-158">**Static partitioning** means that you have already sharded data in the appropriate directories and you can ask Hive partitions manually based on the directory location.</span></span> <span data-ttu-id="6d5ca-159">Följande kodavsnitt är ett exempel.</span><span class="sxs-lookup"><span data-stu-id="6d5ca-159">The following code snippet is an example.</span></span>
  
        INSERT OVERWRITE TABLE lineitem_part
        PARTITION (L_SHIPDATE = ‘5/23/1996 12:00:00 AM’)
        SELECT * FROM lineitem 
        WHERE lineitem.L_SHIPDATE = ‘5/23/1996 12:00:00 AM’
  
        ALTER TABLE lineitem_part ADD PARTITION (L_SHIPDATE = ‘5/23/1996 12:00:00 AM’))
        LOCATION ‘wasb://sampledata@ignitedemo.blob.core.windows.net/partitions/5_23_1996/'
* <span data-ttu-id="6d5ca-160">**Dynamisk partitionering** innebär det att du vill Hive för att skapa partitioner automatiskt åt dig.</span><span class="sxs-lookup"><span data-stu-id="6d5ca-160">**Dynamic partitioning** means that you want Hive to create partitions automatically for you.</span></span> <span data-ttu-id="6d5ca-161">Eftersom det redan har skapat tabellen partitionering från mellanlagringstabellen är vi behöver infoga data i partitionerade tabellen:</span><span class="sxs-lookup"><span data-stu-id="6d5ca-161">Since we have already created the partitioning table from the staging table, all we need to do is insert data to the partitioned table:</span></span>
  
        SET hive.exec.dynamic.partition = true;
        SET hive.exec.dynamic.partition.mode = nonstrict;
        INSERT INTO TABLE lineitem_part
        PARTITION (L_SHIPDATE)
        SELECT L_ORDERKEY as L_ORDERKEY, L_PARTKEY as L_PARTKEY , 
             L_SUPPKEY as L_SUPPKEY, L_LINENUMBER as L_LINENUMBER,
              L_QUANTITY as L_QUANTITY, L_EXTENDEDPRICE as L_EXTENDEDPRICE,
             L_DISCOUNT as L_DISCOUNT, L_TAX as L_TAX, L_RETURNFLAG as           L_RETURNFLAG, L_LINESTATUS as L_LINESTATUS, L_SHIPDATE as           L_SHIPDATE_PS, L_COMMITDATE as L_COMMITDATE, L_RECEIPTDATE as      L_RECEIPTDATE, L_SHIPINSTRUCT as L_SHIPINSTRUCT, L_SHIPMODE as      L_SHIPMODE, L_COMMENT as L_COMMENT, L_SHIPDATE as L_SHIPDATE FROM lineitem;

<span data-ttu-id="6d5ca-162">Mer information finns i [partitionerade tabeller](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+DDL#LanguageManualDDL-PartitionedTables).</span><span class="sxs-lookup"><span data-stu-id="6d5ca-162">For more details, see [Partitioned Tables](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+DDL#LanguageManualDDL-PartitionedTables).</span></span>

## <a name="use-the-orcfile-format"></a><span data-ttu-id="6d5ca-163">Använd formatet ORCFile</span><span class="sxs-lookup"><span data-stu-id="6d5ca-163">Use the ORCFile format</span></span>
<span data-ttu-id="6d5ca-164">Hive stöder olika filformat.</span><span class="sxs-lookup"><span data-stu-id="6d5ca-164">Hive supports different file formats.</span></span> <span data-ttu-id="6d5ca-165">Exempel:</span><span class="sxs-lookup"><span data-stu-id="6d5ca-165">For example:</span></span>

* <span data-ttu-id="6d5ca-166">**Text**: Detta är standardfilformatet och fungerar med de flesta scenarier</span><span class="sxs-lookup"><span data-stu-id="6d5ca-166">**Text**: this is the default file format and works with most scenarios</span></span>
* <span data-ttu-id="6d5ca-167">**Avro**: fungerar bra för heterogena miljöer</span><span class="sxs-lookup"><span data-stu-id="6d5ca-167">**Avro**: works well for interoperability scenarios</span></span>
* <span data-ttu-id="6d5ca-168">**ORC/parkettgolv**: bäst prestanda</span><span class="sxs-lookup"><span data-stu-id="6d5ca-168">**ORC/Parquet**: best suited for performance</span></span>

<span data-ttu-id="6d5ca-169">ORC (optimerad raden kolumner)-formatet är ett mycket effektivt sätt att lagra data med Hive.</span><span class="sxs-lookup"><span data-stu-id="6d5ca-169">ORC (Optimized Row Columnar) format is a highly efficient way to store Hive data.</span></span> <span data-ttu-id="6d5ca-170">ORC har jämfört med andra format, följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="6d5ca-170">Compared to other formats, ORC has the following advantages:</span></span>

* <span data-ttu-id="6d5ca-171">stöd för komplexa typer inklusive DateTime och komplicerade och halvstrukturerade typer</span><span class="sxs-lookup"><span data-stu-id="6d5ca-171">support for complex types including DateTime and complex and semi-structured types</span></span>
* <span data-ttu-id="6d5ca-172">upp till 70% komprimering</span><span class="sxs-lookup"><span data-stu-id="6d5ca-172">up to 70% compression</span></span>
* <span data-ttu-id="6d5ca-173">indexerar var 10 000 rader, som tillåter hoppar över rader</span><span class="sxs-lookup"><span data-stu-id="6d5ca-173">indexes every 10,000 rows, which allow skipping rows</span></span>
* <span data-ttu-id="6d5ca-174">en betydande minskning av körning körning</span><span class="sxs-lookup"><span data-stu-id="6d5ca-174">a significant drop in run-time execution</span></span>

<span data-ttu-id="6d5ca-175">Om du vill aktivera ORC format, skapar du först en tabell med satsen *lagras som ORC*:</span><span class="sxs-lookup"><span data-stu-id="6d5ca-175">To enable ORC format, you first create a table with the clause *Stored as ORC*:</span></span>

    CREATE TABLE lineitem_orc_part
        (L_ORDERKEY INT, L_PARTKEY INT,L_SUPPKEY INT, L_LINENUMBER INT,
         L_QUANTITY DOUBLE, L_EXTENDEDPRICE DOUBLE, L_DISCOUNT DOUBLE,
         L_TAX DOUBLE, L_RETURNFLAG STRING, L_LINESTATUS STRING,
         L_SHIPDATE_PS STRING, L_COMMITDATE STRING, L_RECEIPTDATE STRING,
         L_SHIPINSTRUCT STRING, L_SHIPMODE STRING, L_COMMENT      STRING)
    PARTITIONED BY(L_SHIPDATE STRING)
    STORED AS ORC;

<span data-ttu-id="6d5ca-176">Bredvid, infoga data till tabellen ORC från mellanlagringstabellen.</span><span class="sxs-lookup"><span data-stu-id="6d5ca-176">Next, you insert data to the ORC table from the staging table.</span></span> <span data-ttu-id="6d5ca-177">Exempel:</span><span class="sxs-lookup"><span data-stu-id="6d5ca-177">For example:</span></span>

    INSERT INTO TABLE lineitem_orc
    SELECT L_ORDERKEY as L_ORDERKEY, 
           L_PARTKEY as L_PARTKEY , 
           L_SUPPKEY as L_SUPPKEY,
           L_LINENUMBER as L_LINENUMBER,
            L_QUANTITY as L_QUANTITY, 
           L_EXTENDEDPRICE as L_EXTENDEDPRICE,
           L_DISCOUNT as L_DISCOUNT,
           L_TAX as L_TAX,
           L_RETURNFLAG as L_RETURNFLAG,
           L_LINESTATUS as L_LINESTATUS,
           L_SHIPDATE as L_SHIPDATE,
           L_COMMITDATE as L_COMMITDATE,
           L_RECEIPTDATE as L_RECEIPTDATE, 
           L_SHIPINSTRUCT as L_SHIPINSTRUCT,
           L_SHIPMODE as L_SHIPMODE,
           L_COMMENT as L_COMMENT
    FROM lineitem;

<span data-ttu-id="6d5ca-178">Du kan läsa mer i formatet ORC [här](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+ORC).</span><span class="sxs-lookup"><span data-stu-id="6d5ca-178">You can read more on the ORC format [here](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+ORC).</span></span>

## <a name="vectorization"></a><span data-ttu-id="6d5ca-179">Vectorization</span><span class="sxs-lookup"><span data-stu-id="6d5ca-179">Vectorization</span></span>

<span data-ttu-id="6d5ca-180">Vectorization kan Hive att bearbeta en batch med 1024 rader tillsammans i stället för bearbetning av en rad i taget.</span><span class="sxs-lookup"><span data-stu-id="6d5ca-180">Vectorization allows Hive to process a batch of 1024 rows together instead of processing one row at a time.</span></span> <span data-ttu-id="6d5ca-181">Det innebär att enkla åtgärder utförs snabbare eftersom mindre intern kod måste köras.</span><span class="sxs-lookup"><span data-stu-id="6d5ca-181">It means that simple operations are done faster because less internal code needs to run.</span></span>

<span data-ttu-id="6d5ca-182">Om du vill aktivera prefixet vectorization Hive-fråga med följande inställning:</span><span class="sxs-lookup"><span data-stu-id="6d5ca-182">To enable vectorization prefix your Hive query with the following setting:</span></span>

    set hive.vectorized.execution.enabled = true;

<span data-ttu-id="6d5ca-183">Mer information finns i [Vectorized Frågekörningen](https://cwiki.apache.org/confluence/display/Hive/Vectorized+Query+Execution).</span><span class="sxs-lookup"><span data-stu-id="6d5ca-183">For more information, see [Vectorized query execution](https://cwiki.apache.org/confluence/display/Hive/Vectorized+Query+Execution).</span></span>

## <a name="other-optimization-methods"></a><span data-ttu-id="6d5ca-184">Andra optimeringsmetoder för</span><span class="sxs-lookup"><span data-stu-id="6d5ca-184">Other optimization methods</span></span>
<span data-ttu-id="6d5ca-185">Det finns flera optimeringsmetoder som du kan tänka på, till exempel:</span><span class="sxs-lookup"><span data-stu-id="6d5ca-185">There are more optimization methods that you can consider, for example:</span></span>

* <span data-ttu-id="6d5ca-186">**Hive bucketing:** en teknik som gör att klustret eller segmentera stora anger data för att optimera prestanda för frågor.</span><span class="sxs-lookup"><span data-stu-id="6d5ca-186">**Hive bucketing:** a technique that allows to cluster or segment large sets of data to optimize query performance.</span></span>
* <span data-ttu-id="6d5ca-187">**Anslut optimering:** optimering av registreringsdata Frågekörningen planering att förbättra effektiviteten för kopplingar och minska behovet av att användaren tips.</span><span class="sxs-lookup"><span data-stu-id="6d5ca-187">**Join optimization:** optimization of Hive's query execution planning to improve the efficiency of joins and reduce the need for user hints.</span></span> <span data-ttu-id="6d5ca-188">Mer information finns i [ansluta optimering](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+JoinOptimization#LanguageManualJoinOptimization-JoinOptimization).</span><span class="sxs-lookup"><span data-stu-id="6d5ca-188">For more information, see [Join optimization](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+JoinOptimization#LanguageManualJoinOptimization-JoinOptimization).</span></span>
* <span data-ttu-id="6d5ca-189">**Öka förminskningsapparater**.</span><span class="sxs-lookup"><span data-stu-id="6d5ca-189">**Increase Reducers**.</span></span>

## <a name="next-steps"></a><span data-ttu-id="6d5ca-190">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="6d5ca-190">Next steps</span></span>
<span data-ttu-id="6d5ca-191">I den här artikeln har du lärt dig flera vanliga Hive-fråga optimeringsmetoder.</span><span class="sxs-lookup"><span data-stu-id="6d5ca-191">In this article, you have learned several common Hive query optimization methods.</span></span> <span data-ttu-id="6d5ca-192">Mer information finns i följande artiklar:</span><span class="sxs-lookup"><span data-stu-id="6d5ca-192">To learn more, see the following articles:</span></span>

* [<span data-ttu-id="6d5ca-193">Använda Apache Hive i HDInsight</span><span class="sxs-lookup"><span data-stu-id="6d5ca-193">Use Apache Hive in HDInsight</span></span>](hdinsight-use-hive.md)
* [<span data-ttu-id="6d5ca-194">Analysera svarta fördröjning data med hjälp av Hive i HDInsight</span><span class="sxs-lookup"><span data-stu-id="6d5ca-194">Analyze flight delay data by using Hive in HDInsight</span></span>](hdinsight-analyze-flight-delay-data.md)
* [<span data-ttu-id="6d5ca-195">Analysera Twitter-data med Hive i HDInsight</span><span class="sxs-lookup"><span data-stu-id="6d5ca-195">Analyze Twitter data using Hive in HDInsight</span></span>](hdinsight-analyze-twitter-data.md)
* [<span data-ttu-id="6d5ca-196">Analysera sensordata med Hive-fråga konsolen på Hadoop i HDInsight</span><span class="sxs-lookup"><span data-stu-id="6d5ca-196">Analyze sensor data using the Hive Query Console on Hadoop in HDInsight</span></span>](hdinsight-hive-analyze-sensor-data.md)
* [<span data-ttu-id="6d5ca-197">Använda Hive med HDInsight för att analysera loggar från webbplatser</span><span class="sxs-lookup"><span data-stu-id="6d5ca-197">Use Hive with HDInsight to analyze logs from websites</span></span>](hdinsight-hive-analyze-website-log.md)

[image-hdi-optimize-hive-scaleout_1]: ./media/hdinsight-hadoop-optimize-hive-query/scaleout_1.png
[image-hdi-optimize-hive-scaleout_2]: ./media/hdinsight-hadoop-optimize-hive-query/scaleout_2.png
[image-hdi-optimize-hive-tez_1]: ./media/hdinsight-hadoop-optimize-hive-query/tez_1.png
[image-hdi-optimize-hive-partitioning_1]: ./media/hdinsight-hadoop-optimize-hive-query/partitioning_1.png
