---
title: "aaaOptimize Hive-frågor i Azure HDInsight | Microsoft Docs"
description: "Lär dig hur toooptimize dina Hive-frågor för Hadoop i HDInsight."
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
ms.openlocfilehash: d27f8100e1e9f4823040ff9f693e7b78d6192c6e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="optimize-hive-queries-in-azure-hdinsight"></a><span data-ttu-id="0534c-103">Optimera Hive-frågor i Azure HDInsight</span><span class="sxs-lookup"><span data-stu-id="0534c-103">Optimize Hive queries in Azure HDInsight</span></span>

<span data-ttu-id="0534c-104">Hadoop-kluster är som standard inte optimerats för prestanda.</span><span class="sxs-lookup"><span data-stu-id="0534c-104">By default, Hadoop clusters are not optimized for performance.</span></span> <span data-ttu-id="0534c-105">Den här artikeln beskriver vissa vanligaste Hive prestanda optimeringsmetoder som du kan använda tooyour frågor.</span><span class="sxs-lookup"><span data-stu-id="0534c-105">This article covers some most common Hive performance optimization methods that you can apply tooyour queries.</span></span>

## <a name="scale-out-worker-nodes"></a><span data-ttu-id="0534c-106">Skala ut arbetsnoder</span><span class="sxs-lookup"><span data-stu-id="0534c-106">Scale out worker nodes</span></span>

<span data-ttu-id="0534c-107">Öka hello antalet arbetarnoder i ett kluster kan utnyttja mer mappers och förminskningsapparater toobe körs parallellt.</span><span class="sxs-lookup"><span data-stu-id="0534c-107">Increasing hello number of worker nodes in a cluster can leverage more mappers and reducers toobe run in parallel.</span></span> <span data-ttu-id="0534c-108">Det finns två sätt som du kan öka skala ut i HDInsight:</span><span class="sxs-lookup"><span data-stu-id="0534c-108">There are two ways you can increase scale out in HDInsight:</span></span>

* <span data-ttu-id="0534c-109">Du kan ange hello antalet arbetarnoder använder hello Azure-portalen, Azure PowerShell eller plattformsoberoende kommandoradsgränssnittet samtidigt hello etablera.</span><span class="sxs-lookup"><span data-stu-id="0534c-109">At hello provision time, you can specify hello number of worker nodes using hello Azure portal, Azure PowerShell, or Cross-platform command-line interface.</span></span>  <span data-ttu-id="0534c-110">Mer information finns i [Skapa HDInsight-kluster](hdinsight-hadoop-provision-linux-clusters.md).</span><span class="sxs-lookup"><span data-stu-id="0534c-110">For more information, see [Create HDInsight clusters](hdinsight-hadoop-provision-linux-clusters.md).</span></span> <span data-ttu-id="0534c-111">hello följande skärmbild visar hello worker nodkonfiguration på hello Azure-portalen:</span><span class="sxs-lookup"><span data-stu-id="0534c-111">hello following screenshot shows hello worker node configuration on hello Azure portal:</span></span>
  
    ![scaleout_1][image-hdi-optimize-hive-scaleout_1]
* <span data-ttu-id="0534c-113">Vid hello körningstiden, kan du även skala ut ett kluster utan att återskapa en:</span><span class="sxs-lookup"><span data-stu-id="0534c-113">At hello run time, you can also scale out a cluster without recreating one:</span></span>

    ![scaleout_1][image-hdi-optimize-hive-scaleout_2]

<span data-ttu-id="0534c-115">Läs mer om hello olika virtuella datorer som stöds av HDInsight [HDInsight priser](https://azure.microsoft.com/pricing/details/hdinsight/).</span><span class="sxs-lookup"><span data-stu-id="0534c-115">For more information about hello different virtual machines supported by HDInsight, see [HDInsight pricing](https://azure.microsoft.com/pricing/details/hdinsight/).</span></span>

## <a name="enable-tez"></a><span data-ttu-id="0534c-116">Aktivera Tez</span><span class="sxs-lookup"><span data-stu-id="0534c-116">Enable Tez</span></span>

<span data-ttu-id="0534c-117">[Apache Tez](http://hortonworks.com/hadoop/tez/) är en alternativ motorn toohello MapReduce Körningsmotor:</span><span class="sxs-lookup"><span data-stu-id="0534c-117">[Apache Tez](http://hortonworks.com/hadoop/tez/) is an alternative execution engine toohello MapReduce engine:</span></span>

![tez_1][image-hdi-optimize-hive-tez_1]

<span data-ttu-id="0534c-119">Tez går snabbare eftersom:</span><span class="sxs-lookup"><span data-stu-id="0534c-119">Tez is faster because:</span></span>

* <span data-ttu-id="0534c-120">**Köra dirigeras acykliska diagram (DAG) som ett enda jobb i hello-motorn MapReduce**.</span><span class="sxs-lookup"><span data-stu-id="0534c-120">**Execute Directed Acyclic Graph (DAG) as a single job in hello MapReduce engine**.</span></span> <span data-ttu-id="0534c-121">hello DAG kräver varje uppsättning mappers toobe följt av en uppsättning förminskningsapparater.</span><span class="sxs-lookup"><span data-stu-id="0534c-121">hello DAG requires each set of mappers toobe followed by one set of reducers.</span></span> <span data-ttu-id="0534c-122">Detta medför flera MapReduce-jobb toobe roterade för varje Hive-fråga.</span><span class="sxs-lookup"><span data-stu-id="0534c-122">This causes multiple MapReduce jobs toobe spun off for each Hive query.</span></span> <span data-ttu-id="0534c-123">Tez har inte sådan begränsning och kan bearbeta komplexa DAG som ett jobb, vilket minimerar jobbet startade kostnader.</span><span class="sxs-lookup"><span data-stu-id="0534c-123">Tez does not have such constraint and can process complex DAG as one job thus minimizing job startup overhead.</span></span>
* <span data-ttu-id="0534c-124">**Undviker onödig skrivningar**.</span><span class="sxs-lookup"><span data-stu-id="0534c-124">**Avoids unnecessary writes**.</span></span> <span data-ttu-id="0534c-125">På grund av toomultiple jobb som skapas för hello skrivs samma Hive-fråga i motorn MapReduce hello, hello utdata för varje jobb tooHDFS för mellanliggande data.</span><span class="sxs-lookup"><span data-stu-id="0534c-125">Due toomultiple jobs being spun for hello same Hive query in hello MapReduce engine, hello output of each job is written tooHDFS for intermediate data.</span></span> <span data-ttu-id="0534c-126">Eftersom Tez minimerar antalet jobb för varje Hive-fråga är kan tooavoid onödiga skrivåtgärder.</span><span class="sxs-lookup"><span data-stu-id="0534c-126">Since Tez minimizes number of jobs for each Hive query it is able tooavoid unnecessary write.</span></span>
* <span data-ttu-id="0534c-127">**Minimerar uppstart fördröjningar**.</span><span class="sxs-lookup"><span data-stu-id="0534c-127">**Minimizes start-up delays**.</span></span> <span data-ttu-id="0534c-128">Tez är bättre kan toominimize uppstart fördröjning genom att minska hello antal mappers måste toostart och även förbättra optimering i hela.</span><span class="sxs-lookup"><span data-stu-id="0534c-128">Tez is better able toominimize start-up delay by reducing hello number of mappers it needs toostart and also improving optimization throughout.</span></span>
* <span data-ttu-id="0534c-129">**Återanvänder behållare**.</span><span class="sxs-lookup"><span data-stu-id="0534c-129">**Reuses containers**.</span></span> <span data-ttu-id="0534c-130">När möjliga Tez kan tooreuse behållare tooensure som fördröjning på grund av minskas toostarting upp behållare.</span><span class="sxs-lookup"><span data-stu-id="0534c-130">Whenever possible Tez is able tooreuse containers tooensure that latency due toostarting up containers is reduced.</span></span>
* <span data-ttu-id="0534c-131">**Kontinuerlig optimeringstekniker**.</span><span class="sxs-lookup"><span data-stu-id="0534c-131">**Continuous optimization techniques**.</span></span> <span data-ttu-id="0534c-132">Traditionellt gjordes optimering under kompilering fasen.</span><span class="sxs-lookup"><span data-stu-id="0534c-132">Traditionally optimization was done during compilation phase.</span></span> <span data-ttu-id="0534c-133">Men det finns mer information om hello indata som innebär att bättre optimering under körningen.</span><span class="sxs-lookup"><span data-stu-id="0534c-133">However more information about hello inputs is available that allow for better optimization during runtime.</span></span> <span data-ttu-id="0534c-134">Tez använder kontinuerlig optimeringstekniker som tillåter toooptimize hello planen ytterligare i hello runtime-fasen.</span><span class="sxs-lookup"><span data-stu-id="0534c-134">Tez uses continuous optimization techniques that allows it toooptimize hello plan further into hello runtime phase.</span></span>

<span data-ttu-id="0534c-135">Mer information om dessa koncept finns [Apache TEZ](http://hortonworks.com/hadoop/tez/).</span><span class="sxs-lookup"><span data-stu-id="0534c-135">For more details on these concepts, see [Apache TEZ](http://hortonworks.com/hadoop/tez/).</span></span>

<span data-ttu-id="0534c-136">Du kan göra en Hive-fråga Tez aktiverad med hello frågan med hello inställningen nedan:</span><span class="sxs-lookup"><span data-stu-id="0534c-136">You can make any Hive query Tez enabled by prefixing hello query with hello setting below:</span></span>

    set hive.execution.engine=tez;

<span data-ttu-id="0534c-137">Linux-baserade HDInsight-kluster har Tez aktiverad som standard.</span><span class="sxs-lookup"><span data-stu-id="0534c-137">Linux-based HDInsight clusters have Tez enabled by default.</span></span>


## <a name="hive-partitioning"></a><span data-ttu-id="0534c-138">Hive partitionering</span><span class="sxs-lookup"><span data-stu-id="0534c-138">Hive partitioning</span></span>

<span data-ttu-id="0534c-139">I/o-åtgärden är hello större flaskhalsar för att köra Hive-frågor.</span><span class="sxs-lookup"><span data-stu-id="0534c-139">I/O operation is hello major performance bottleneck for running Hive queries.</span></span> <span data-ttu-id="0534c-140">hello prestanda förbättras om hello mängden data som behöver toobe läsa kan minskas.</span><span class="sxs-lookup"><span data-stu-id="0534c-140">hello performance can be improved if hello amount of data that needs toobe read can be reduced.</span></span> <span data-ttu-id="0534c-141">Sök igenom hela Hive-tabeller som standard Hive-frågor.</span><span class="sxs-lookup"><span data-stu-id="0534c-141">By default, Hive queries scan entire Hive tables.</span></span> <span data-ttu-id="0534c-142">Det här är bra för frågor som tabellsökningar.</span><span class="sxs-lookup"><span data-stu-id="0534c-142">This is great for queries like table scans.</span></span> <span data-ttu-id="0534c-143">Men för frågor som behöver bara tooscan en liten mängd data (till exempel frågor med filtrering), detta skapar onödiga omkostnader.</span><span class="sxs-lookup"><span data-stu-id="0534c-143">However for queries that only need tooscan a small amount of data (for example, queries with filtering), this behavior creates unnecessary overhead.</span></span> <span data-ttu-id="0534c-144">Hive partitionering tillåter Hive-frågor tooaccess endast hello nödvändiga mängden data i Hive-tabeller.</span><span class="sxs-lookup"><span data-stu-id="0534c-144">Hive partitioning allows Hive queries tooaccess only hello necessary amount of data in Hive tables.</span></span>

<span data-ttu-id="0534c-145">Hive partitionering implementeras genom att ordna hello rådata i nya kataloger med varje partition har en egen katalog - där hello definieras av hello användare.</span><span class="sxs-lookup"><span data-stu-id="0534c-145">Hive partitioning is implemented by reorganizing hello raw data into new directories with each partition having its own directory - where hello partition is defined by hello user.</span></span> <span data-ttu-id="0534c-146">hello följande diagram illustrerar partitionering en Hive-tabell som hello kolumnen *år*.</span><span class="sxs-lookup"><span data-stu-id="0534c-146">hello following diagram illustrates partitioning a Hive table by hello column *Year*.</span></span> <span data-ttu-id="0534c-147">En ny katalog skapas för varje år.</span><span class="sxs-lookup"><span data-stu-id="0534c-147">A new directory is created for each year.</span></span>

![Partitionering][image-hdi-optimize-hive-partitioning_1]

<span data-ttu-id="0534c-149">Vissa partitionering att tänka på:</span><span class="sxs-lookup"><span data-stu-id="0534c-149">Some partitioning considerations:</span></span>

* <span data-ttu-id="0534c-150">**Gör inte under partition** -partitionering på kolumner med bara några värden kan orsaka några partitioner.</span><span class="sxs-lookup"><span data-stu-id="0534c-150">**Do not under partition** - Partitioning on columns with only a few values can cause few partitions.</span></span> <span data-ttu-id="0534c-151">Till exempel bara partitionering på kön skapas två partitioner toobe skapade (han och hondjur), därför bara minska hello svarstid med högst hälften.</span><span class="sxs-lookup"><span data-stu-id="0534c-151">For example, partitioning on gender only creates two partitions toobe created (male and female), thus only reduce hello latency by a maximum of half.</span></span>
* <span data-ttu-id="0534c-152">**Gör inte över partition** - på Hej andra extreme, skapa en partition på en kolumn med ett unikt värde (t.ex, användar-ID) medför flera partitioner.</span><span class="sxs-lookup"><span data-stu-id="0534c-152">**Do not over partition** - On hello other extreme, creating a partition on a column with a unique value (for example, userid) causes multiple partitions.</span></span> <span data-ttu-id="0534c-153">Gör mycket stress på hello klustret namenode över partitionen eftersom den har toohandle hello stort antal mappar.</span><span class="sxs-lookup"><span data-stu-id="0534c-153">Over partition causes much stress on hello cluster namenode as it has toohandle hello large number of directories.</span></span>
* <span data-ttu-id="0534c-154">**Undvika data skeva** -Välj din partitionsnyckel klokt så att alla partitioner har även storlek.</span><span class="sxs-lookup"><span data-stu-id="0534c-154">**Avoid data skew** - Choose your partitioning key wisely so that all partitions are even size.</span></span> <span data-ttu-id="0534c-155">Ett exempel partitionering på *tillstånd* kan orsaka hello antalet poster under California toobe nästan 30 x som Vermont på grund av toohello skillnad i populationen.</span><span class="sxs-lookup"><span data-stu-id="0534c-155">An example is partitioning on *State* may cause hello number of records under California toobe almost 30x that of Vermont due toohello difference in population.</span></span>

<span data-ttu-id="0534c-156">toocreate en partitionstabell använda hello *partitioneras av* sats:</span><span class="sxs-lookup"><span data-stu-id="0534c-156">toocreate a partition table, use hello *Partitioned By* clause:</span></span>

    CREATE TABLE lineitem_part
        (L_ORDERKEY INT, L_PARTKEY INT, L_SUPPKEY INT,L_LINENUMBER INT,
         L_QUANTITY DOUBLE, L_EXTENDEDPRICE DOUBLE, L_DISCOUNT DOUBLE,
         L_TAX DOUBLE, L_RETURNFLAG STRING, L_LINESTATUS STRING,
         L_SHIPDATE_PS STRING, L_COMMITDATE STRING, L_RECEIPTDATE            STRING, L_SHIPINSTRUCT STRING, L_SHIPMODE STRING,
         L_COMMENT STRING)
    PARTITIONED BY(L_SHIPDATE STRING)
    ROW FORMAT DELIMITED FIELDS TERMINATED BY '\t'
    STORED AS TEXTFILE;

<span data-ttu-id="0534c-157">Du kan antingen skapa partitionering statisk eller dynamisk partitionering när hello partitionerad tabell skapas.</span><span class="sxs-lookup"><span data-stu-id="0534c-157">Once hello partitioned table is created, you can either create static partitioning or dynamic partitioning.</span></span>

* <span data-ttu-id="0534c-158">**Statisk partitionering** innebär att du har redan shardade data i hello lämpliga kataloger och du kan be Hive partitioner manuellt utifrån hello katalogplatsen.</span><span class="sxs-lookup"><span data-stu-id="0534c-158">**Static partitioning** means that you have already sharded data in hello appropriate directories and you can ask Hive partitions manually based on hello directory location.</span></span> <span data-ttu-id="0534c-159">hello följande kodavsnitt är ett exempel.</span><span class="sxs-lookup"><span data-stu-id="0534c-159">hello following code snippet is an example.</span></span>
  
        INSERT OVERWRITE TABLE lineitem_part
        PARTITION (L_SHIPDATE = ‘5/23/1996 12:00:00 AM’)
        SELECT * FROM lineitem 
        WHERE lineitem.L_SHIPDATE = ‘5/23/1996 12:00:00 AM’
  
        ALTER TABLE lineitem_part ADD PARTITION (L_SHIPDATE = ‘5/23/1996 12:00:00 AM’))
        LOCATION ‘wasb://sampledata@ignitedemo.blob.core.windows.net/partitions/5_23_1996/'
* <span data-ttu-id="0534c-160">**Dynamisk partitionering** innebär det att du vill Hive toocreate partitioner automatiskt åt dig.</span><span class="sxs-lookup"><span data-stu-id="0534c-160">**Dynamic partitioning** means that you want Hive toocreate partitions automatically for you.</span></span> <span data-ttu-id="0534c-161">Eftersom det redan har skapat hello partitionering tabell från hello mellanlagringstabell finns allt vi behöver toodo infoga data toohello partitionerad tabell:</span><span class="sxs-lookup"><span data-stu-id="0534c-161">Since we have already created hello partitioning table from hello staging table, all we need toodo is insert data toohello partitioned table:</span></span>
  
        SET hive.exec.dynamic.partition = true;
        SET hive.exec.dynamic.partition.mode = nonstrict;
        INSERT INTO TABLE lineitem_part
        PARTITION (L_SHIPDATE)
        SELECT L_ORDERKEY as L_ORDERKEY, L_PARTKEY as L_PARTKEY , 
             L_SUPPKEY as L_SUPPKEY, L_LINENUMBER as L_LINENUMBER,
              L_QUANTITY as L_QUANTITY, L_EXTENDEDPRICE as L_EXTENDEDPRICE,
             L_DISCOUNT as L_DISCOUNT, L_TAX as L_TAX, L_RETURNFLAG as           L_RETURNFLAG, L_LINESTATUS as L_LINESTATUS, L_SHIPDATE as           L_SHIPDATE_PS, L_COMMITDATE as L_COMMITDATE, L_RECEIPTDATE as      L_RECEIPTDATE, L_SHIPINSTRUCT as L_SHIPINSTRUCT, L_SHIPMODE as      L_SHIPMODE, L_COMMENT as L_COMMENT, L_SHIPDATE as L_SHIPDATE FROM lineitem;

<span data-ttu-id="0534c-162">Mer information finns i [partitionerade tabeller](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+DDL#LanguageManualDDL-PartitionedTables).</span><span class="sxs-lookup"><span data-stu-id="0534c-162">For more details, see [Partitioned Tables](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+DDL#LanguageManualDDL-PartitionedTables).</span></span>

## <a name="use-hello-orcfile-format"></a><span data-ttu-id="0534c-163">Använd hello ORCFile format</span><span class="sxs-lookup"><span data-stu-id="0534c-163">Use hello ORCFile format</span></span>
<span data-ttu-id="0534c-164">Hive stöder olika filformat.</span><span class="sxs-lookup"><span data-stu-id="0534c-164">Hive supports different file formats.</span></span> <span data-ttu-id="0534c-165">Exempel:</span><span class="sxs-lookup"><span data-stu-id="0534c-165">For example:</span></span>

* <span data-ttu-id="0534c-166">**Text**: Detta är hello standardfilformat och fungerar med de flesta scenarier</span><span class="sxs-lookup"><span data-stu-id="0534c-166">**Text**: this is hello default file format and works with most scenarios</span></span>
* <span data-ttu-id="0534c-167">**Avro**: fungerar bra för heterogena miljöer</span><span class="sxs-lookup"><span data-stu-id="0534c-167">**Avro**: works well for interoperability scenarios</span></span>
* <span data-ttu-id="0534c-168">**ORC/parkettgolv**: bäst prestanda</span><span class="sxs-lookup"><span data-stu-id="0534c-168">**ORC/Parquet**: best suited for performance</span></span>

<span data-ttu-id="0534c-169">ORC (optimerad raden kolumner)-formatet är ett mycket effektivt sätt toostore Hive-data.</span><span class="sxs-lookup"><span data-stu-id="0534c-169">ORC (Optimized Row Columnar) format is a highly efficient way toostore Hive data.</span></span> <span data-ttu-id="0534c-170">Jämfört med tooother format, ORC har hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="0534c-170">Compared tooother formats, ORC has hello following advantages:</span></span>

* <span data-ttu-id="0534c-171">stöd för komplexa typer inklusive DateTime och komplicerade och halvstrukturerade typer</span><span class="sxs-lookup"><span data-stu-id="0534c-171">support for complex types including DateTime and complex and semi-structured types</span></span>
* <span data-ttu-id="0534c-172">Konfigurera too70% komprimering</span><span class="sxs-lookup"><span data-stu-id="0534c-172">up too70% compression</span></span>
* <span data-ttu-id="0534c-173">indexerar var 10 000 rader, som tillåter hoppar över rader</span><span class="sxs-lookup"><span data-stu-id="0534c-173">indexes every 10,000 rows, which allow skipping rows</span></span>
* <span data-ttu-id="0534c-174">en betydande minskning av körning körning</span><span class="sxs-lookup"><span data-stu-id="0534c-174">a significant drop in run-time execution</span></span>

<span data-ttu-id="0534c-175">tooenable ORC format du först skapa en tabell med hello-satsen *lagras som ORC*:</span><span class="sxs-lookup"><span data-stu-id="0534c-175">tooenable ORC format, you first create a table with hello clause *Stored as ORC*:</span></span>

    CREATE TABLE lineitem_orc_part
        (L_ORDERKEY INT, L_PARTKEY INT,L_SUPPKEY INT, L_LINENUMBER INT,
         L_QUANTITY DOUBLE, L_EXTENDEDPRICE DOUBLE, L_DISCOUNT DOUBLE,
         L_TAX DOUBLE, L_RETURNFLAG STRING, L_LINESTATUS STRING,
         L_SHIPDATE_PS STRING, L_COMMITDATE STRING, L_RECEIPTDATE STRING,
         L_SHIPINSTRUCT STRING, L_SHIPMODE STRING, L_COMMENT      STRING)
    PARTITIONED BY(L_SHIPDATE STRING)
    STORED AS ORC;

<span data-ttu-id="0534c-176">Sätt datatabell toohello ORC från hello mellanlagringstabell.</span><span class="sxs-lookup"><span data-stu-id="0534c-176">Next, you insert data toohello ORC table from hello staging table.</span></span> <span data-ttu-id="0534c-177">Exempel:</span><span class="sxs-lookup"><span data-stu-id="0534c-177">For example:</span></span>

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

<span data-ttu-id="0534c-178">Du kan läsa mer om hello ORC format [här](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+ORC).</span><span class="sxs-lookup"><span data-stu-id="0534c-178">You can read more on hello ORC format [here](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+ORC).</span></span>

## <a name="vectorization"></a><span data-ttu-id="0534c-179">Vectorization</span><span class="sxs-lookup"><span data-stu-id="0534c-179">Vectorization</span></span>

<span data-ttu-id="0534c-180">Vectorization gör Hive tooprocess en batch med 1024 rader tillsammans i stället för bearbetning av en rad i taget.</span><span class="sxs-lookup"><span data-stu-id="0534c-180">Vectorization allows Hive tooprocess a batch of 1024 rows together instead of processing one row at a time.</span></span> <span data-ttu-id="0534c-181">Det innebär att enkla åtgärder utförs snabbare eftersom mindre intern kod måste toorun.</span><span class="sxs-lookup"><span data-stu-id="0534c-181">It means that simple operations are done faster because less internal code needs toorun.</span></span>

<span data-ttu-id="0534c-182">tooenable vectorization prefixet Hive-fråga med följande inställning hello:</span><span class="sxs-lookup"><span data-stu-id="0534c-182">tooenable vectorization prefix your Hive query with hello following setting:</span></span>

    set hive.vectorized.execution.enabled = true;

<span data-ttu-id="0534c-183">Mer information finns i [Vectorized Frågekörningen](https://cwiki.apache.org/confluence/display/Hive/Vectorized+Query+Execution).</span><span class="sxs-lookup"><span data-stu-id="0534c-183">For more information, see [Vectorized query execution](https://cwiki.apache.org/confluence/display/Hive/Vectorized+Query+Execution).</span></span>

## <a name="other-optimization-methods"></a><span data-ttu-id="0534c-184">Andra optimeringsmetoder för</span><span class="sxs-lookup"><span data-stu-id="0534c-184">Other optimization methods</span></span>
<span data-ttu-id="0534c-185">Det finns flera optimeringsmetoder som du kan tänka på, till exempel:</span><span class="sxs-lookup"><span data-stu-id="0534c-185">There are more optimization methods that you can consider, for example:</span></span>

* <span data-ttu-id="0534c-186">**Hive bucketing:** en teknik som möjliggör toocluster eller segment stora mängder data toooptimize frågeprestanda.</span><span class="sxs-lookup"><span data-stu-id="0534c-186">**Hive bucketing:** a technique that allows toocluster or segment large sets of data toooptimize query performance.</span></span>
* <span data-ttu-id="0534c-187">**Anslut optimering:** optimering av registreringsdata Frågekörningen planera tooimprove hello effektiviteten för kopplingar och minska hello behovet av att användaren tips.</span><span class="sxs-lookup"><span data-stu-id="0534c-187">**Join optimization:** optimization of Hive's query execution planning tooimprove hello efficiency of joins and reduce hello need for user hints.</span></span> <span data-ttu-id="0534c-188">Mer information finns i [ansluta optimering](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+JoinOptimization#LanguageManualJoinOptimization-JoinOptimization).</span><span class="sxs-lookup"><span data-stu-id="0534c-188">For more information, see [Join optimization](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+JoinOptimization#LanguageManualJoinOptimization-JoinOptimization).</span></span>
* <span data-ttu-id="0534c-189">**Öka förminskningsapparater**.</span><span class="sxs-lookup"><span data-stu-id="0534c-189">**Increase Reducers**.</span></span>

## <a name="next-steps"></a><span data-ttu-id="0534c-190">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="0534c-190">Next steps</span></span>
<span data-ttu-id="0534c-191">I den här artikeln har du lärt dig flera vanliga Hive-fråga optimeringsmetoder.</span><span class="sxs-lookup"><span data-stu-id="0534c-191">In this article, you have learned several common Hive query optimization methods.</span></span> <span data-ttu-id="0534c-192">toolearn se fler hello följande artiklar:</span><span class="sxs-lookup"><span data-stu-id="0534c-192">toolearn more, see hello following articles:</span></span>

* [<span data-ttu-id="0534c-193">Använda Apache Hive i HDInsight</span><span class="sxs-lookup"><span data-stu-id="0534c-193">Use Apache Hive in HDInsight</span></span>](hdinsight-use-hive.md)
* [<span data-ttu-id="0534c-194">Analysera svarta fördröjning data med hjälp av Hive i HDInsight</span><span class="sxs-lookup"><span data-stu-id="0534c-194">Analyze flight delay data by using Hive in HDInsight</span></span>](hdinsight-analyze-flight-delay-data.md)
* [<span data-ttu-id="0534c-195">Analysera Twitter-data med Hive i HDInsight</span><span class="sxs-lookup"><span data-stu-id="0534c-195">Analyze Twitter data using Hive in HDInsight</span></span>](hdinsight-analyze-twitter-data.md)
* [<span data-ttu-id="0534c-196">Analysera sensordata med hello Hive-fråga konsolen på Hadoop i HDInsight</span><span class="sxs-lookup"><span data-stu-id="0534c-196">Analyze sensor data using hello Hive Query Console on Hadoop in HDInsight</span></span>](hdinsight-hive-analyze-sensor-data.md)
* [<span data-ttu-id="0534c-197">Använda Hive med HDInsight tooanalyze loggar från webbplatser</span><span class="sxs-lookup"><span data-stu-id="0534c-197">Use Hive with HDInsight tooanalyze logs from websites</span></span>](hdinsight-hive-analyze-website-log.md)

[image-hdi-optimize-hive-scaleout_1]: ./media/hdinsight-hadoop-optimize-hive-query/scaleout_1.png
[image-hdi-optimize-hive-scaleout_2]: ./media/hdinsight-hadoop-optimize-hive-query/scaleout_2.png
[image-hdi-optimize-hive-tez_1]: ./media/hdinsight-hadoop-optimize-hive-query/tez_1.png
[image-hdi-optimize-hive-partitioning_1]: ./media/hdinsight-hadoop-optimize-hive-query/partitioning_1.png
