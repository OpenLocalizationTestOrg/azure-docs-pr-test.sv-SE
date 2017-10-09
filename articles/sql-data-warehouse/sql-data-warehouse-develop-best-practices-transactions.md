---
title: "aaaOptimizing transaktioner för SQL Data Warehouse | Microsoft Docs"
description: "Bästa praxis riktlinjer för att skriva effektiva transaktionsuppdateringar i Azure SQL Data Warehouse"
services: sql-data-warehouse
documentationcenter: NA
author: jrowlandjones
manager: jhubbard
editor: 
ms.assetid: 6f326f26-8a54-49df-a482-9c96a58db371
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: t-sql
ms.date: 10/31/2016
ms.author: jrj;barbkess
ms.openlocfilehash: 1a821161711db9460b7e10d3cf7ba498d711448b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="optimizing-transactions-for-sql-data-warehouse"></a><span data-ttu-id="d6c63-103">Optimera transaktioner för SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="d6c63-103">Optimizing transactions for SQL Data Warehouse</span></span>
<span data-ttu-id="d6c63-104">Den här artikeln förklarar hur toooptimize hello prestanda för transaktionell koden och minimerar risken för lång återställningar.</span><span class="sxs-lookup"><span data-stu-id="d6c63-104">This article explains how toooptimize hello performance of your transactional code while minimizing risk for long rollbacks.</span></span>

## <a name="transactions-and-logging"></a><span data-ttu-id="d6c63-105">Transaktioner och loggning</span><span class="sxs-lookup"><span data-stu-id="d6c63-105">Transactions and logging</span></span>
<span data-ttu-id="d6c63-106">Transaktioner är en viktig komponent i en relationsdatabas-motor.</span><span class="sxs-lookup"><span data-stu-id="d6c63-106">Transactions are an important component of a relational database engine.</span></span> <span data-ttu-id="d6c63-107">SQL Data Warehouse använder transaktioner under dataändringar.</span><span class="sxs-lookup"><span data-stu-id="d6c63-107">SQL Data Warehouse uses transactions during data modification.</span></span> <span data-ttu-id="d6c63-108">Dessa transaktioner kan vara uttryckliga eller underförstådda.</span><span class="sxs-lookup"><span data-stu-id="d6c63-108">These transactions can be explicit or implicit.</span></span> <span data-ttu-id="d6c63-109">Enskild `INSERT`, `UPDATE` och `DELETE` -satser är alla exempel implicit transaktioner.</span><span class="sxs-lookup"><span data-stu-id="d6c63-109">Single `INSERT`, `UPDATE` and `DELETE` statements are all examples of implicit transactions.</span></span> <span data-ttu-id="d6c63-110">Skrivs explicita transaktioner explicit av en utvecklare som använder `BEGIN TRAN`, `COMMIT TRAN` eller `ROLLBACK TRAN` och används vanligtvis när flera ändringar instruktioner måste toobe knutna tillsammans i en atomisk enhet.</span><span class="sxs-lookup"><span data-stu-id="d6c63-110">Explicit transactions are written explicitly by a developer using `BEGIN TRAN`, `COMMIT TRAN` or `ROLLBACK TRAN` and are typically used when multiple modification statements need toobe tied together in a single atomic unit.</span></span> 

<span data-ttu-id="d6c63-111">Azure SQL Data Warehouse genomför ändringar toohello databasen med hjälp av transaktionsloggar.</span><span class="sxs-lookup"><span data-stu-id="d6c63-111">Azure SQL Data Warehouse commits changes toohello database using transaction logs.</span></span> <span data-ttu-id="d6c63-112">Varje distribution har sin egen transaktionsloggen.</span><span class="sxs-lookup"><span data-stu-id="d6c63-112">Each distribution has its own transaction log.</span></span> <span data-ttu-id="d6c63-113">Transaktionen sparas loggen automatiskt.</span><span class="sxs-lookup"><span data-stu-id="d6c63-113">Transaction log writes are automatic.</span></span> <span data-ttu-id="d6c63-114">Det finns ingen konfiguration krävs.</span><span class="sxs-lookup"><span data-stu-id="d6c63-114">There is no configuration required.</span></span> <span data-ttu-id="d6c63-115">Dock samtidigt som den här processen garanterar hello skrivåtgärder införs en kostnader i hello system.</span><span class="sxs-lookup"><span data-stu-id="d6c63-115">However, whilst this process guarantees hello write it does introduce an overhead in hello system.</span></span> <span data-ttu-id="d6c63-116">Du kan minimera den här effekten genom att skriva ett effektivt kod.</span><span class="sxs-lookup"><span data-stu-id="d6c63-116">You can minimize this impact by writing transactionally efficient code.</span></span> <span data-ttu-id="d6c63-117">Transaktionellt effektiv kod hamnar brett i två kategorier.</span><span class="sxs-lookup"><span data-stu-id="d6c63-117">Transactionally efficient code broadly falls into two categories.</span></span>

* <span data-ttu-id="d6c63-118">Använd minimal loggning konstruktioner där det är möjligt</span><span class="sxs-lookup"><span data-stu-id="d6c63-118">Leverage minimal logging constructs where possible</span></span>
* <span data-ttu-id="d6c63-119">Bearbetning av data med hjälp av omfång batchar tooavoid enda tidskrävande transaktioner</span><span class="sxs-lookup"><span data-stu-id="d6c63-119">Process data using scoped batches tooavoid singular long running transactions</span></span>
* <span data-ttu-id="d6c63-120">Anta en partitionsväxling mönster för stora ändringar tooa angivna partition</span><span class="sxs-lookup"><span data-stu-id="d6c63-120">Adopt a partition switching pattern for large modifications tooa given partition</span></span>

## <a name="minimal-vs-full-logging"></a><span data-ttu-id="d6c63-121">Minimal kontra fullständig loggning</span><span class="sxs-lookup"><span data-stu-id="d6c63-121">Minimal vs. full logging</span></span>
<span data-ttu-id="d6c63-122">Till skillnad från fullständigt loggade åtgärder som använder hello transaction log tookeep reda på varje rad ändring, hålla minimalt loggade åtgärder reda på utsträckning allokeringar och endast metadata ändringar.</span><span class="sxs-lookup"><span data-stu-id="d6c63-122">Unlike fully logged operations, which use hello transaction log tookeep track of every row change, minimally logged operations keep track of extent allocations and meta-data changes only.</span></span> <span data-ttu-id="d6c63-123">Därför minimal loggning innebär att loggning endast hello-information som är nödvändiga toorollback hello transaktion i hello händelse av ett fel eller en uttrycklig begäran (`ROLLBACK TRAN`).</span><span class="sxs-lookup"><span data-stu-id="d6c63-123">Therefore, minimal logging involves logging only hello information that is required toorollback hello transaction in hello event of a failure or an explicit request (`ROLLBACK TRAN`).</span></span> <span data-ttu-id="d6c63-124">Som mycket mindre information spåras i hello transaktionsloggen, presterar minimalt loggade åtgärden bättre än på samma sätt storlek fullständigt loggade åtgärden.</span><span class="sxs-lookup"><span data-stu-id="d6c63-124">As much less information is tracked in hello transaction log, a minimally logged operation performs better than a similarly sized fully logged operation.</span></span> <span data-ttu-id="d6c63-125">Dessutom eftersom färre skrivningar går hello transaktionsloggen, en mycket mindre mängd loggdata genereras och det är flera i/o effektivt.</span><span class="sxs-lookup"><span data-stu-id="d6c63-125">Furthermore, because fewer writes go hello transaction log, a much smaller amount of log data is generated and so is more I/O efficient.</span></span>

<span data-ttu-id="d6c63-126">hello transaktion säkerhetsgränsen gäller endast toofully loggade åtgärder.</span><span class="sxs-lookup"><span data-stu-id="d6c63-126">hello transaction safety limits only apply toofully logged operations.</span></span>

> [!NOTE]
> <span data-ttu-id="d6c63-127">Minimalt loggade åtgärder kan delta i explicita transaktioner.</span><span class="sxs-lookup"><span data-stu-id="d6c63-127">Minimally logged operations can participate in explicit transactions.</span></span> <span data-ttu-id="d6c63-128">Alla ändringar i allokering strukturer spåras, är det möjligt tooroll tillbaka minimalt loggade åtgärder.</span><span class="sxs-lookup"><span data-stu-id="d6c63-128">As all changes in allocation structures are tracked, it is possible tooroll back minimally logged operations.</span></span> <span data-ttu-id="d6c63-129">Det är viktigt toounderstand hello ändringen har ”minimalt” loggade den inte är icke loggas.</span><span class="sxs-lookup"><span data-stu-id="d6c63-129">It is important toounderstand that hello change is "minimally" logged it is not un-logged.</span></span>
> 
> 

## <a name="minimally-logged-operations"></a><span data-ttu-id="d6c63-130">Minimalt loggade åtgärder</span><span class="sxs-lookup"><span data-stu-id="d6c63-130">Minimally logged operations</span></span>
<span data-ttu-id="d6c63-131">hello kan följande åtgärder loggas minimalt:</span><span class="sxs-lookup"><span data-stu-id="d6c63-131">hello following operations are capable of being minimally logged:</span></span>

* <span data-ttu-id="d6c63-132">SKAPA TABLE AS SELECT ([CTAS][CTAS])</span><span class="sxs-lookup"><span data-stu-id="d6c63-132">CREATE TABLE AS SELECT ([CTAS][CTAS])</span></span>
* <span data-ttu-id="d6c63-133">INSERT... VÄLJ</span><span class="sxs-lookup"><span data-stu-id="d6c63-133">INSERT..SELECT</span></span>
* <span data-ttu-id="d6c63-134">SKAPA INDEX</span><span class="sxs-lookup"><span data-stu-id="d6c63-134">CREATE INDEX</span></span>
* <span data-ttu-id="d6c63-135">ALTER INDEX REBUILD</span><span class="sxs-lookup"><span data-stu-id="d6c63-135">ALTER INDEX REBUILD</span></span>
* <span data-ttu-id="d6c63-136">DROP INDEX</span><span class="sxs-lookup"><span data-stu-id="d6c63-136">DROP INDEX</span></span>
* <span data-ttu-id="d6c63-137">TRUNKERA TABELLEN</span><span class="sxs-lookup"><span data-stu-id="d6c63-137">TRUNCATE TABLE</span></span>
* <span data-ttu-id="d6c63-138">SLÄPPA TABELLEN</span><span class="sxs-lookup"><span data-stu-id="d6c63-138">DROP TABLE</span></span>
* <span data-ttu-id="d6c63-139">ALTER TABLE SWITCH PARTITION</span><span class="sxs-lookup"><span data-stu-id="d6c63-139">ALTER TABLE SWITCH PARTITION</span></span>

<!--
- MERGE
- UPDATE on LOB Types .WRITE
- SELECT..INTO
-->

> [!NOTE]
> <span data-ttu-id="d6c63-140">Internt dataflyttsåtgärderna (exempelvis `BROADCAST` och `SHUFFLE`) påverkas inte av hello transaktion säkerhet gränsen.</span><span class="sxs-lookup"><span data-stu-id="d6c63-140">Internal data movement operations (such as `BROADCAST` and `SHUFFLE`) are not affected by hello transaction safety limit.</span></span>
> 
> 

## <a name="minimal-logging-with-bulk-load"></a><span data-ttu-id="d6c63-141">Minimal loggning med massinläsning</span><span class="sxs-lookup"><span data-stu-id="d6c63-141">Minimal logging with bulk load</span></span>
<span data-ttu-id="d6c63-142">`CTAS`och `INSERT...SELECT` är både Massredigera belastningen åtgärder.</span><span class="sxs-lookup"><span data-stu-id="d6c63-142">`CTAS` and `INSERT...SELECT` are both bulk load operations.</span></span> <span data-ttu-id="d6c63-143">Dock både påverkas av hello mål tabelldefinitionen och beror på hello belastningen scenario.</span><span class="sxs-lookup"><span data-stu-id="d6c63-143">However, both are influenced by hello target table definition and depend on hello load scenario.</span></span> <span data-ttu-id="d6c63-144">Nedan finns en tabell med information om din massåtgärd helt eller minimalt loggas:</span><span class="sxs-lookup"><span data-stu-id="d6c63-144">Below is a table that explains if your bulk operation will be fully or minimally logged:</span></span>  

| <span data-ttu-id="d6c63-145">Primärt Index</span><span class="sxs-lookup"><span data-stu-id="d6c63-145">Primary Index</span></span> | <span data-ttu-id="d6c63-146">Läs in Scenario</span><span class="sxs-lookup"><span data-stu-id="d6c63-146">Load Scenario</span></span> | <span data-ttu-id="d6c63-147">Loggningsläge</span><span class="sxs-lookup"><span data-stu-id="d6c63-147">Logging Mode</span></span> |
| --- | --- | --- |
| <span data-ttu-id="d6c63-148">Heap</span><span class="sxs-lookup"><span data-stu-id="d6c63-148">Heap</span></span> |<span data-ttu-id="d6c63-149">Alla</span><span class="sxs-lookup"><span data-stu-id="d6c63-149">Any</span></span> |<span data-ttu-id="d6c63-150">**Minimal**</span><span class="sxs-lookup"><span data-stu-id="d6c63-150">**Minimal**</span></span> |
| <span data-ttu-id="d6c63-151">Grupperat Index</span><span class="sxs-lookup"><span data-stu-id="d6c63-151">Clustered Index</span></span> |<span data-ttu-id="d6c63-152">Tom måltabellen</span><span class="sxs-lookup"><span data-stu-id="d6c63-152">Empty target table</span></span> |<span data-ttu-id="d6c63-153">**Minimal**</span><span class="sxs-lookup"><span data-stu-id="d6c63-153">**Minimal**</span></span> |
| <span data-ttu-id="d6c63-154">Grupperat Index</span><span class="sxs-lookup"><span data-stu-id="d6c63-154">Clustered Index</span></span> |<span data-ttu-id="d6c63-155">Inlästa rader inte överlappa befintliga sidor i mål</span><span class="sxs-lookup"><span data-stu-id="d6c63-155">Loaded rows do not overlap with existing pages in target</span></span> |<span data-ttu-id="d6c63-156">**Minimal**</span><span class="sxs-lookup"><span data-stu-id="d6c63-156">**Minimal**</span></span> |
| <span data-ttu-id="d6c63-157">Grupperat Index</span><span class="sxs-lookup"><span data-stu-id="d6c63-157">Clustered Index</span></span> |<span data-ttu-id="d6c63-158">Inlästa rader överlappar befintliga sidor i mål</span><span class="sxs-lookup"><span data-stu-id="d6c63-158">Loaded rows overlap with existing pages in target</span></span> |<span data-ttu-id="d6c63-159">Fullständig</span><span class="sxs-lookup"><span data-stu-id="d6c63-159">Full</span></span> |
| <span data-ttu-id="d6c63-160">Grupperat Columnstore-Index</span><span class="sxs-lookup"><span data-stu-id="d6c63-160">Clustered Columnstore Index</span></span> |<span data-ttu-id="d6c63-161">Batchstorlek > = 102,400 per partition justerad distribution</span><span class="sxs-lookup"><span data-stu-id="d6c63-161">Batch size >= 102,400 per partition aligned distribution</span></span> |<span data-ttu-id="d6c63-162">**Minimal**</span><span class="sxs-lookup"><span data-stu-id="d6c63-162">**Minimal**</span></span> |
| <span data-ttu-id="d6c63-163">Grupperat Columnstore-Index</span><span class="sxs-lookup"><span data-stu-id="d6c63-163">Clustered Columnstore Index</span></span> |<span data-ttu-id="d6c63-164">Batch-storlek < 102,400 per partition justerad distribution</span><span class="sxs-lookup"><span data-stu-id="d6c63-164">Batch size < 102,400 per partition aligned distribution</span></span> |<span data-ttu-id="d6c63-165">Fullständig</span><span class="sxs-lookup"><span data-stu-id="d6c63-165">Full</span></span> |

<span data-ttu-id="d6c63-166">Det är värt att nämna att alla skrivningar tooupdate sekundär eller icke-grupperade index kommer alltid att vara helt loggade åtgärder.</span><span class="sxs-lookup"><span data-stu-id="d6c63-166">It is worth noting that any writes tooupdate secondary or non-clustered indexes will always be fully logged operations.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="d6c63-167">SQL Data Warehouse har 60-distributioner.</span><span class="sxs-lookup"><span data-stu-id="d6c63-167">SQL Data Warehouse has 60 distributions.</span></span> <span data-ttu-id="d6c63-168">Därför, förutsatt att alla rader fördelas jämnt och hamnar i en enda partition din batch måste toocontain 6,144,000 rader eller större toobe minimalt loggas när du skriver tooa klustrade Kolumnlagringsindexet.</span><span class="sxs-lookup"><span data-stu-id="d6c63-168">Therefore, assuming all rows are evenly distributed and landing in a single partition, your batch will need toocontain 6,144,000 rows or larger toobe minimally logged when writing tooa Clustered Columnstore Index.</span></span> <span data-ttu-id="d6c63-169">Om hello tabellen är partitionerad och hello rader infogas span partitionsgränser, behöver du 6,144,000 rader per partition gräns förutsatt att data fördelas jämnt.</span><span class="sxs-lookup"><span data-stu-id="d6c63-169">If hello table is partitioned and hello rows being inserted span partition boundaries, then you will need 6,144,000 rows per partition boundary assuming even data distribution.</span></span> <span data-ttu-id="d6c63-170">Varje partition i varje distribution måste oberoende överskrids hello 102 400 raden för hello insert toobe minimalt loggat in hello-distribution.</span><span class="sxs-lookup"><span data-stu-id="d6c63-170">Each partition in each distribution must independently exceed hello 102,400 row threshold for hello insert toobe minimally logged into hello distribution.</span></span>
> 
> 

<span data-ttu-id="d6c63-171">Läsa in data i en icke-tom tabell med ett grupperat index innehåller ofta en blandning av fullständigt loggade och minimalt loggade rader.</span><span class="sxs-lookup"><span data-stu-id="d6c63-171">Loading data into a non-empty table with a clustered index can often contain a mixture of fully logged and minimally logged rows.</span></span> <span data-ttu-id="d6c63-172">Ett grupperat index är ett belastningsutjämnade träd (b-trädet) sidor.</span><span class="sxs-lookup"><span data-stu-id="d6c63-172">A clustered index is a balanced tree (b-tree) of pages.</span></span> <span data-ttu-id="d6c63-173">Om hello sida skrivs tooalready innehåller rader från en annan transaktion, sedan loggas dessa skrivningar fullständigt.</span><span class="sxs-lookup"><span data-stu-id="d6c63-173">If hello page being written tooalready contains rows from another transaction, then these writes will be fully logged.</span></span> <span data-ttu-id="d6c63-174">Men om hello sida är tom loggas sedan hello skrivåtgärder toothat sidan minimalt.</span><span class="sxs-lookup"><span data-stu-id="d6c63-174">However, if hello page is empty then hello write toothat page will be minimally logged.</span></span>

## <a name="optimizing-deletes"></a><span data-ttu-id="d6c63-175">Optimera borttagningar</span><span class="sxs-lookup"><span data-stu-id="d6c63-175">Optimizing deletes</span></span>
<span data-ttu-id="d6c63-176">`DELETE`är fullständigt loggade åtgärden.</span><span class="sxs-lookup"><span data-stu-id="d6c63-176">`DELETE` is a fully logged operation.</span></span>  <span data-ttu-id="d6c63-177">Om du behöver toodelete en stor mängd data i en tabell eller en partition kan det ofta passar bättre för`SELECT` hello data du vill tookeep som kan köras som ett minimalt loggade åtgärden.</span><span class="sxs-lookup"><span data-stu-id="d6c63-177">If you need toodelete a large amount of data in a table or a partition, it often makes more sense too`SELECT` hello data you wish tookeep, which can be run as a minimally logged operation.</span></span>  <span data-ttu-id="d6c63-178">tooaccomplish, skapa en ny tabell med [CTAS][CTAS].</span><span class="sxs-lookup"><span data-stu-id="d6c63-178">tooaccomplish this, create a new table with [CTAS][CTAS].</span></span>  <span data-ttu-id="d6c63-179">När du skapat använda [Byt namn på] [ RENAME] tooswap ut din gamla tabell med hello nyskapad tabell.</span><span class="sxs-lookup"><span data-stu-id="d6c63-179">Once created, use [RENAME][RENAME] tooswap out your old table with hello newly created table.</span></span>

```sql
-- Delete all sales transactions for Promotions except PromotionKey 2.

--Step 01. Create a new table select only hello records we want tookep (PromotionKey 2)
CREATE TABLE [dbo].[FactInternetSales_d]
WITH
(    CLUSTERED COLUMNSTORE INDEX
,    DISTRIBUTION = HASH([ProductKey])
,     PARTITION     (    [OrderDateKey] RANGE RIGHT 
                                    FOR VALUES    (    20000101, 20010101, 20020101, 20030101, 20040101, 20050101
                                                ,    20060101, 20070101, 20080101, 20090101, 20100101, 20110101
                                                ,    20120101, 20130101, 20140101, 20150101, 20160101, 20170101
                                                ,    20180101, 20190101, 20200101, 20210101, 20220101, 20230101
                                                ,    20240101, 20250101, 20260101, 20270101, 20280101, 20290101
                                                )
)
AS
SELECT     *
FROM     [dbo].[FactInternetSales]
WHERE    [PromotionKey] = 2
OPTION (LABEL = 'CTAS : Delete')
;

--Step 02. Rename hello Tables tooreplace hello 
RENAME OBJECT [dbo].[FactInternetSales]   too[FactInternetSales_old];
RENAME OBJECT [dbo].[FactInternetSales_d] too[FactInternetSales];
```

## <a name="optimizing-updates"></a><span data-ttu-id="d6c63-180">Optimera uppdateringar</span><span class="sxs-lookup"><span data-stu-id="d6c63-180">Optimizing updates</span></span>
<span data-ttu-id="d6c63-181">`UPDATE`är fullständigt loggade åtgärden.</span><span class="sxs-lookup"><span data-stu-id="d6c63-181">`UPDATE` is a fully logged operation.</span></span>  <span data-ttu-id="d6c63-182">Om du behöver tooupdate ett stort antal rader i en tabell eller en partition det ofta vara mycket mer effektivt toouse minimalt loggade åtgärden som [CTAS] [ CTAS] toodo så.</span><span class="sxs-lookup"><span data-stu-id="d6c63-182">If you need tooupdate a large number of rows in a table or a partition it can often be far more efficient toouse a minimally logged operation such as [CTAS][CTAS] toodo so.</span></span>

<span data-ttu-id="d6c63-183">I hello exempel under en fullständig uppdatering har konverterade tooa `CTAS` så att minimal loggning är möjligt.</span><span class="sxs-lookup"><span data-stu-id="d6c63-183">In hello example below a full table update has been converted tooa `CTAS` so that minimal logging is possible.</span></span>

<span data-ttu-id="d6c63-184">I det här fallet vi efterhand lägger till en rabatt toohello försäljning i hello tabell:</span><span class="sxs-lookup"><span data-stu-id="d6c63-184">In this case we are retrospectively adding a discount amount toohello sales in hello table:</span></span>

```sql
--Step 01. Create a new table containing hello "Update". 
CREATE TABLE [dbo].[FactInternetSales_u]
WITH
(    CLUSTERED INDEX
,    DISTRIBUTION = HASH([ProductKey])
,     PARTITION     (    [OrderDateKey] RANGE RIGHT 
                                    FOR VALUES    (    20000101, 20010101, 20020101, 20030101, 20040101, 20050101
                                                ,    20060101, 20070101, 20080101, 20090101, 20100101, 20110101
                                                ,    20120101, 20130101, 20140101, 20150101, 20160101, 20170101
                                                ,    20180101, 20190101, 20200101, 20210101, 20220101, 20230101
                                                ,    20240101, 20250101, 20260101, 20270101, 20280101, 20290101
                                                )
                )
)
AS 
SELECT
    [ProductKey]  
,    [OrderDateKey] 
,    [DueDateKey]  
,    [ShipDateKey] 
,    [CustomerKey] 
,    [PromotionKey] 
,    [CurrencyKey] 
,    [SalesTerritoryKey]
,    [SalesOrderNumber]
,    [SalesOrderLineNumber]
,    [RevisionNumber]
,    [OrderQuantity]
,    [UnitPrice]
,    [ExtendedAmount]
,    [UnitPriceDiscountPct]
,    ISNULL(CAST(5 as float),0) AS [DiscountAmount]
,    [ProductStandardCost]
,    [TotalProductCost]
,    ISNULL(CAST(CASE WHEN [SalesAmount] <=5 THEN 0
         ELSE [SalesAmount] - 5
         END AS MONEY),0) AS [SalesAmount]
,    [TaxAmt]
,    [Freight]
,    [CarrierTrackingNumber] 
,    [CustomerPONumber]
FROM    [dbo].[FactInternetSales]
OPTION (LABEL = 'CTAS : Update')
;

--Step 02. Rename hello tables
RENAME OBJECT [dbo].[FactInternetSales]   too[FactInternetSales_old];
RENAME OBJECT [dbo].[FactInternetSales_u] too[FactInternetSales];

--Step 03. Drop hello old table
DROP TABLE [dbo].[FactInternetSales_old]
```

> [!NOTE]
> <span data-ttu-id="d6c63-185">Skapa nytt stora tabeller kan dra nytta av funktioner för SQL Data Warehouse arbetsbelastning.</span><span class="sxs-lookup"><span data-stu-id="d6c63-185">Re-creating large tables can benefit from using SQL Data Warehouse workload management features.</span></span> <span data-ttu-id="d6c63-186">För mer information finns toohello arbetsbelastning management-avsnittet i hello [samtidighet] [ concurrency] artikel.</span><span class="sxs-lookup"><span data-stu-id="d6c63-186">For more details please refer toohello workload management section in hello [concurrency][concurrency] article.</span></span>
> 
> 

## <a name="optimizing-with-partition-switching"></a><span data-ttu-id="d6c63-187">Optimera med Växla partition</span><span class="sxs-lookup"><span data-stu-id="d6c63-187">Optimizing with partition switching</span></span>
<span data-ttu-id="d6c63-188">När inför storskaliga ändringar i en [tabell partition][table partition], och sedan en partitionsväxling mönstret gör mycket bra.</span><span class="sxs-lookup"><span data-stu-id="d6c63-188">When faced with large scale modifications inside a [table partition][table partition], then a partition switching pattern makes a lot of sense.</span></span> <span data-ttu-id="d6c63-189">Om hello dataändring är viktig och spänner över flera partitioner, bara iterera över hello partitioner uppnår hello samma resultat.</span><span class="sxs-lookup"><span data-stu-id="d6c63-189">If hello data modification is significant and spans multiple partitions, then simply iterating over hello partitions achieves hello same result.</span></span>

<span data-ttu-id="d6c63-190">hello steg tooperform en partition växel är följande:</span><span class="sxs-lookup"><span data-stu-id="d6c63-190">hello steps tooperform a partition switch are as follows:</span></span>

1. <span data-ttu-id="d6c63-191">Skapa en tom av partition</span><span class="sxs-lookup"><span data-stu-id="d6c63-191">Create an empty out partition</span></span>
2. <span data-ttu-id="d6c63-192">Utför hello uppdatering som en CTAS</span><span class="sxs-lookup"><span data-stu-id="d6c63-192">Perform hello 'update' as a CTAS</span></span>
3. <span data-ttu-id="d6c63-193">Byta ut hello befintliga data toohello ut tabell</span><span class="sxs-lookup"><span data-stu-id="d6c63-193">Switch out hello existing data toohello out table</span></span>
4. <span data-ttu-id="d6c63-194">Växla i hello nya data</span><span class="sxs-lookup"><span data-stu-id="d6c63-194">Switch in hello new data</span></span>
5. <span data-ttu-id="d6c63-195">Rensa hello data</span><span class="sxs-lookup"><span data-stu-id="d6c63-195">Clean up hello data</span></span>

<span data-ttu-id="d6c63-196">Dock identifiera toohelp hello partitioner tooswitch vi måste först toobuild en helper-procedur som hello en nedan.</span><span class="sxs-lookup"><span data-stu-id="d6c63-196">However, toohelp identify hello partitions tooswitch we will first need toobuild a helper procedure such as hello one below.</span></span> 

```sql
CREATE PROCEDURE dbo.partition_data_get
    @schema_name           NVARCHAR(128)
,    @table_name               NVARCHAR(128)
,    @boundary_value           INT
AS
IF OBJECT_ID('tempdb..#ptn_data') IS NOT NULL
BEGIN
    DROP TABLE #ptn_data
END
CREATE TABLE #ptn_data
WITH    (    DISTRIBUTION = ROUND_ROBIN
        ,    HEAP
        )
AS
WITH CTE
AS
(
SELECT     s.name                            AS [schema_name]
,        t.name                            AS [table_name]
,         p.partition_number                AS [ptn_nmbr]
,        p.[rows]                        AS [ptn_rows]
,        CAST(r.[value] AS INT)            AS [boundary_value]
FROM        sys.schemas                    AS s
JOIN        sys.tables                    AS t    ON  s.[schema_id]        = t.[schema_id]
JOIN        sys.indexes                    AS i    ON     t.[object_id]        = i.[object_id]
JOIN        sys.partitions                AS p    ON     i.[object_id]        = p.[object_id] 
                                                AND i.[index_id]        = p.[index_id] 
JOIN        sys.partition_schemes        AS h    ON     i.[data_space_id]    = h.[data_space_id]
JOIN        sys.partition_functions        AS f    ON     h.[function_id]        = f.[function_id]
LEFT JOIN    sys.partition_range_values    AS r     ON     f.[function_id]        = r.[function_id] 
                                                AND r.[boundary_id]        = p.[partition_number]
WHERE i.[index_id] <= 1
)
SELECT    *
FROM    CTE
WHERE    [schema_name]        = @schema_name
AND        [table_name]        = @table_name
AND        [boundary_value]    = @boundary_value
OPTION (LABEL = 'dbo.partition_data_get : CTAS : #ptn_data')
;
GO
```

<span data-ttu-id="d6c63-197">Den här proceduren maximerar återanvändning av kod och behåller hello partitionsväxling exempel mer kompakt.</span><span class="sxs-lookup"><span data-stu-id="d6c63-197">This procedure maximizes code re-use and keeps hello partition switching example more compact.</span></span>

<span data-ttu-id="d6c63-198">hello koden nedan visar hello fem steg som nämns ovan tooachieve en fullständig partitionsväxling rutinen.</span><span class="sxs-lookup"><span data-stu-id="d6c63-198">hello code below demonstrates hello five steps mentioned above tooachieve a full partition switching routine.</span></span>

```sql
--Create a partitioned aligned empty table tooswitch out hello data 
IF OBJECT_ID('[dbo].[FactInternetSales_out]') IS NOT NULL
BEGIN
    DROP TABLE [dbo].[FactInternetSales_out]
END

CREATE TABLE [dbo].[FactInternetSales_out]
WITH
(    DISTRIBUTION = HASH([ProductKey])
,    CLUSTERED COLUMNSTORE INDEX
,     PARTITION     (    [OrderDateKey] RANGE RIGHT 
                                    FOR VALUES    (    20020101, 20030101
                                                )
                )
)
AS
SELECT *
FROM    [dbo].[FactInternetSales]
WHERE 1=2
OPTION (LABEL = 'CTAS : Partition Switch IN : UPDATE')
;

--Create a partitioned aligned table and update hello data in hello select portion of hello CTAS
IF OBJECT_ID('[dbo].[FactInternetSales_in]') IS NOT NULL
BEGIN
    DROP TABLE [dbo].[FactInternetSales_in]
END

CREATE TABLE [dbo].[FactInternetSales_in]
WITH
(    DISTRIBUTION = HASH([ProductKey])
,    CLUSTERED COLUMNSTORE INDEX
,     PARTITION     (    [OrderDateKey] RANGE RIGHT 
                                    FOR VALUES    (    20020101, 20030101
                                                )
                )
)
AS 
SELECT
    [ProductKey]  
,    [OrderDateKey] 
,    [DueDateKey]  
,    [ShipDateKey] 
,    [CustomerKey] 
,    [PromotionKey] 
,    [CurrencyKey] 
,    [SalesTerritoryKey]
,    [SalesOrderNumber]
,    [SalesOrderLineNumber]
,    [RevisionNumber]
,    [OrderQuantity]
,    [UnitPrice]
,    [ExtendedAmount]
,    [UnitPriceDiscountPct]
,    ISNULL(CAST(5 as float),0) AS [DiscountAmount]
,    [ProductStandardCost]
,    [TotalProductCost]
,    ISNULL(CAST(CASE WHEN [SalesAmount] <=5 THEN 0
         ELSE [SalesAmount] - 5
         END AS MONEY),0) AS [SalesAmount]
,    [TaxAmt]
,    [Freight]
,    [CarrierTrackingNumber] 
,    [CustomerPONumber]
FROM    [dbo].[FactInternetSales]
WHERE    OrderDateKey BETWEEN 20020101 AND 20021231
OPTION (LABEL = 'CTAS : Partition Switch IN : UPDATE')
;

--Use hello helper procedure tooidentify hello partitions
--hello source table
EXEC dbo.partition_data_get 'dbo','FactInternetSales',20030101
DECLARE @ptn_nmbr_src INT = (SELECT ptn_nmbr FROM #ptn_data)
SELECT @ptn_nmbr_src

--hello "in" table
EXEC dbo.partition_data_get 'dbo','FactInternetSales_in',20030101
DECLARE @ptn_nmbr_in INT = (SELECT ptn_nmbr FROM #ptn_data)
SELECT @ptn_nmbr_in

--hello "out" table
EXEC dbo.partition_data_get 'dbo','FactInternetSales_out',20030101
DECLARE @ptn_nmbr_out INT = (SELECT ptn_nmbr FROM #ptn_data)
SELECT @ptn_nmbr_out

--Switch hello partitions over
DECLARE @SQL NVARCHAR(4000) = '
ALTER TABLE [dbo].[FactInternetSales]    SWITCH PARTITION '+CAST(@ptn_nmbr_src AS VARCHAR(20))    +' too[dbo].[FactInternetSales_out] PARTITION '    +CAST(@ptn_nmbr_out AS VARCHAR(20))+';
ALTER TABLE [dbo].[FactInternetSales_in] SWITCH PARTITION '+CAST(@ptn_nmbr_in AS VARCHAR(20))    +' too[dbo].[FactInternetSales] PARTITION '        +CAST(@ptn_nmbr_src AS VARCHAR(20))+';'
EXEC sp_executesql @SQL

--Perform hello clean-up
TRUNCATE TABLE dbo.FactInternetSales_out;
TRUNCATE TABLE dbo.FactInternetSales_in;

DROP TABLE dbo.FactInternetSales_out
DROP TABLE dbo.FactInternetSales_in
DROP TABLE #ptn_data
```

## <a name="minimize-logging-with-small-batches"></a><span data-ttu-id="d6c63-199">Minimera loggning med små batchar</span><span class="sxs-lookup"><span data-stu-id="d6c63-199">Minimize logging with small batches</span></span>
<span data-ttu-id="d6c63-200">För stora mängder data ändras, kan det vara klokt toodivide hello åtgärden i segment eller batchar tooscope hello arbete.</span><span class="sxs-lookup"><span data-stu-id="d6c63-200">For large data modification operations, it may make sense toodivide hello operation into chunks or batches tooscope hello unit of work.</span></span>

<span data-ttu-id="d6c63-201">Ett fungerande exempel finns nedan.</span><span class="sxs-lookup"><span data-stu-id="d6c63-201">A working example is provided below.</span></span> <span data-ttu-id="d6c63-202">hello batchstorlek har ställts in tooa trivial nummer toohighlight hello tekniken.</span><span class="sxs-lookup"><span data-stu-id="d6c63-202">hello batch size has been set tooa trivial number toohighlight hello technique.</span></span> <span data-ttu-id="d6c63-203">I verkligheten är hello batchstorlek betydligt större.</span><span class="sxs-lookup"><span data-stu-id="d6c63-203">In reality hello batch size would be significantly larger.</span></span> 

```sql
SET NO_COUNT ON;
IF OBJECT_ID('tempdb..#t') IS NOT NULL
BEGIN
    DROP TABLE #t;
    PRINT '#t dropped';
END

CREATE TABLE #t
WITH    (    DISTRIBUTION = ROUND_ROBIN
        ,    HEAP
        )
AS
SELECT    ROW_NUMBER() OVER(ORDER BY (SELECT NULL)) AS seq_nmbr
,        SalesOrderNumber
,        SalesOrderLineNumber
FROM    dbo.FactInternetSales
WHERE    [OrderDateKey] BETWEEN 20010101 and 20011231
;

DECLARE    @seq_start        INT = 1
,        @batch_iterator    INT = 1
,        @batch_size        INT = 50
,        @max_seq_nmbr    INT = (SELECT MAX(seq_nmbr) FROM dbo.#t)
;

DECLARE    @batch_count    INT = (SELECT CEILING((@max_seq_nmbr*1.0)/@batch_size))
,        @seq_end        INT = @batch_size
;

SELECT COUNT(*)
FROM    dbo.FactInternetSales f

PRINT 'MAX_seq_nmbr '+CAST(@max_seq_nmbr AS VARCHAR(20))
PRINT 'MAX_Batch_count '+CAST(@batch_count AS VARCHAR(20))

WHILE    @batch_iterator <= @batch_count
BEGIN
    DELETE
    FROM    dbo.FactInternetSales
    WHERE EXISTS
    (
            SELECT    1
            FROM    #t t
            WHERE    seq_nmbr BETWEEN  @seq_start AND @seq_end
            AND        FactInternetSales.SalesOrderNumber        = t.SalesOrderNumber
            AND        FactInternetSales.SalesOrderLineNumber    = t.SalesOrderLineNumber
    )
    ;

    SET @seq_start = @seq_end
    SET @seq_end = (@seq_start+@batch_size);
    SET @batch_iterator +=1;
END
```

## <a name="pause-and-scaling-guidance"></a><span data-ttu-id="d6c63-204">Pausa och skalning vägledning</span><span class="sxs-lookup"><span data-stu-id="d6c63-204">Pause and scaling guidance</span></span>
<span data-ttu-id="d6c63-205">Azure SQL Data Warehouse kan du pausa, fortsätta och skala ditt informationslager på begäran.</span><span class="sxs-lookup"><span data-stu-id="d6c63-205">Azure SQL Data Warehouse lets you pause, resume and scale your data warehouse on demand.</span></span> <span data-ttu-id="d6c63-206">När du pausar du eller skala ditt SQL Data Warehouse är det viktigt toounderstand att alla pågående transaktioner avslutas omedelbart. orsakar toobe alla öppna transaktioner återställs.</span><span class="sxs-lookup"><span data-stu-id="d6c63-206">When you pause or scale your SQL Data Warehouse it is important toounderstand that any in-flight transactions are terminated immediately; causing any open transactions toobe rolled back.</span></span> <span data-ttu-id="d6c63-207">Om din arbetsbelastning har utfärdats tidskrävande och ofullständiga data ändras tidigare måste toohello paus eller skalningsåtgärden sedan detta verk toobe ångra.</span><span class="sxs-lookup"><span data-stu-id="d6c63-207">If your workload had issued a long running and incomplete data modification prior toohello pause or scale operation, then this work will need toobe undone.</span></span> <span data-ttu-id="d6c63-208">Detta kan påverka hello tid det tar toopause eller skala din Azure SQL Data Warehouse-databas.</span><span class="sxs-lookup"><span data-stu-id="d6c63-208">This may impact hello time it takes toopause or scale your Azure SQL Data Warehouse database.</span></span> 

> [!IMPORTANT]
> <span data-ttu-id="d6c63-209">Båda `UPDATE` och `DELETE` är fullständigt loggade åtgärder och så att dessa Ångra/Gör om kan ta betydligt längre tid än motsvarande minimalt loggade åtgärder.</span><span class="sxs-lookup"><span data-stu-id="d6c63-209">Both `UPDATE` and `DELETE` are fully logged operations and so these undo/redo operations can take significantly longer than equivalent minimally logged operations.</span></span> 
> 
> 

<span data-ttu-id="d6c63-210">hello bästa scenario är toolet i svarta data ändras transaktioner fullständig tidigare toopausing eller skalning SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="d6c63-210">hello best scenario is toolet in flight data modification transactions complete prior toopausing or scaling SQL Data Warehouse.</span></span> <span data-ttu-id="d6c63-211">Men kan det inte alltid praktiskt.</span><span class="sxs-lookup"><span data-stu-id="d6c63-211">However, this may not always be practical.</span></span> <span data-ttu-id="d6c63-212">toomitigate hello risken för en lång återställning betrakta ett av hello följande alternativ:</span><span class="sxs-lookup"><span data-stu-id="d6c63-212">toomitigate hello risk of a long rollback, consider one of hello following options:</span></span>

* <span data-ttu-id="d6c63-213">Skriv långvariga åtgärder med hjälp av [CTAS][CTAS]</span><span class="sxs-lookup"><span data-stu-id="d6c63-213">Re-write long running operations using [CTAS][CTAS]</span></span>
* <span data-ttu-id="d6c63-214">Nedbrytning hello åtgärden i segment; operativsystem för en delmängd av hello rader</span><span class="sxs-lookup"><span data-stu-id="d6c63-214">Break hello operation down into chunks; operating on a subset of hello rows</span></span>

## <a name="next-steps"></a><span data-ttu-id="d6c63-215">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="d6c63-215">Next steps</span></span>
<span data-ttu-id="d6c63-216">Se [transaktioner i SQL Data Warehouse] [ Transactions in SQL Data Warehouse] toolearn mer om isoleringsnivåer och transaktionell gränser.</span><span class="sxs-lookup"><span data-stu-id="d6c63-216">See [Transactions in SQL Data Warehouse][Transactions in SQL Data Warehouse] toolearn more about isolation levels and transactional limits.</span></span>  <span data-ttu-id="d6c63-217">En översikt över andra metodtips finns [Metodtips för SQL Data Warehouse][SQL Data Warehouse Best Practices].</span><span class="sxs-lookup"><span data-stu-id="d6c63-217">For an overview of other Best Practices, see [SQL Data Warehouse Best Practices][SQL Data Warehouse Best Practices].</span></span>

<!--Image references-->

<!--Article references-->
[Transactions in SQL Data Warehouse]: ./sql-data-warehouse-develop-transactions.md
[table partition]: ./sql-data-warehouse-tables-partition.md
[Concurrency]: ./sql-data-warehouse-develop-concurrency.md
[CTAS]: ./sql-data-warehouse-develop-ctas.md
[SQL Data Warehouse Best Practices]: ./sql-data-warehouse-best-practices.md

<!--MSDN references-->
[alter index]:https://msdn.microsoft.com/library/ms188388.aspx
[RENAME]: https://msdn.microsoft.com/library/mt631611.aspx

<!-- Other web references -->

