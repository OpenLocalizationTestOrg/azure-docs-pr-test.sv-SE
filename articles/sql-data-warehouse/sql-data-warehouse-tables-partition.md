---
title: aaaPartitioning tabeller i SQL Data Warehouse | Microsoft Docs
description: "Komma igång med Tabellpartitionering i Azure SQL Data Warehouse."
services: sql-data-warehouse
documentationcenter: NA
author: shivaniguptamsft
manager: jhubbard
editor: 
ms.assetid: 6cef870c-114f-470c-af10-02300c58885d
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: tables
ms.date: 10/31/2016
ms.author: shigu;barbkess
ms.openlocfilehash: aa63c51562f3e6f83063320860b195e135a721e1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="partitioning-tables-in-sql-data-warehouse"></a><span data-ttu-id="a7940-103">Partitionering tabeller i SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="a7940-103">Partitioning tables in SQL Data Warehouse</span></span>
> [!div class="op_single_selector"]
> * <span data-ttu-id="a7940-104">[Översikt över][Overview]</span><span class="sxs-lookup"><span data-stu-id="a7940-104">[Overview][Overview]</span></span>
> * <span data-ttu-id="a7940-105">[Datatyper][Data Types]</span><span class="sxs-lookup"><span data-stu-id="a7940-105">[Data Types][Data Types]</span></span>
> * <span data-ttu-id="a7940-106">[Distribuera][Distribute]</span><span class="sxs-lookup"><span data-stu-id="a7940-106">[Distribute][Distribute]</span></span>
> * <span data-ttu-id="a7940-107">[Index][Index]</span><span class="sxs-lookup"><span data-stu-id="a7940-107">[Index][Index]</span></span>
> * <span data-ttu-id="a7940-108">[Partition][Partition]</span><span class="sxs-lookup"><span data-stu-id="a7940-108">[Partition][Partition]</span></span>
> * <span data-ttu-id="a7940-109">[Statistik][Statistics]</span><span class="sxs-lookup"><span data-stu-id="a7940-109">[Statistics][Statistics]</span></span>
> * <span data-ttu-id="a7940-110">[Tillfällig][Temporary]</span><span class="sxs-lookup"><span data-stu-id="a7940-110">[Temporary][Temporary]</span></span>
> 
> 

<span data-ttu-id="a7940-111">Partitionering stöds på alla SQL Data Warehouse tabelltyper; inklusive grupperade columnstore, grupperat index och heap.</span><span class="sxs-lookup"><span data-stu-id="a7940-111">Partitioning is supported on all SQL Data Warehouse table types; including clustered columnstore, clustered index, and heap.</span></span>  <span data-ttu-id="a7940-112">Partitionering stöds även på alla typer av distribution, inklusive hash- eller resursallokering distribueras.</span><span class="sxs-lookup"><span data-stu-id="a7940-112">Partitioning is also supported on all distribution types, including both hash or round robin distributed.</span></span>  <span data-ttu-id="a7940-113">Partitionering kan du toodivide dina data i mindre grupper av data och i de flesta fall partitionering görs på ett datum.</span><span class="sxs-lookup"><span data-stu-id="a7940-113">Partitioning enables you toodivide your data into smaller groups of data and in most cases, partitioning is done on a date column.</span></span>

## <a name="benefits-of-partitioning"></a><span data-ttu-id="a7940-114">Fördelarna med partitionering</span><span class="sxs-lookup"><span data-stu-id="a7940-114">Benefits of partitioning</span></span>
<span data-ttu-id="a7940-115">Partitionering kan dra prestandadata för underhåll och fråga.</span><span class="sxs-lookup"><span data-stu-id="a7940-115">Partitioning can benefit data maintenance and query performance.</span></span>  <span data-ttu-id="a7940-116">Om den fördelar både eller bara en beror på hur data har lästs in och om hello samma kolumn kan användas för båda, eftersom partitionering kan bara utföras på en kolumn.</span><span class="sxs-lookup"><span data-stu-id="a7940-116">Whether it benefits both or just one is dependent on how data is loaded and whether hello same column can be used for both purposes, since partitioning can only be done on one column.</span></span>

### <a name="benefits-tooloads"></a><span data-ttu-id="a7940-117">Fördelar tooloads</span><span class="sxs-lookup"><span data-stu-id="a7940-117">Benefits tooloads</span></span>
<span data-ttu-id="a7940-118">hello största fördelen partitionering i SQL Data Warehouse är förbättra hello effektivitet och prestanda för inläsning av data med hjälp av partition borttagning, växlar och sammanslagning.</span><span class="sxs-lookup"><span data-stu-id="a7940-118">hello primary benefit of partitioning in SQL Data Warehouse is improve hello efficiency and performance of loading data by use of partition deletion, switching and merging.</span></span>  <span data-ttu-id="a7940-119">I de flesta fall data är partitionerad på ett datum knutna kolumn som är nära toohello sekvens som hello data är inlästa toohello databas.</span><span class="sxs-lookup"><span data-stu-id="a7940-119">In most cases data is partitioned on a date column that is closely tied toohello sequence which hello data is loaded toohello database.</span></span>  <span data-ttu-id="a7940-120">En av hello största fördelarna med att använda partitioner toomaintain data den hello undvikande av transaktionsloggning.</span><span class="sxs-lookup"><span data-stu-id="a7940-120">One of hello greatest benefits of using partitions toomaintain data it hello avoidance of transaction logging.</span></span>  <span data-ttu-id="a7940-121">Helt enkelt lägga till, uppdatera eller ta bort data kan vara hello den enklaste metoden med lite tankar och prestanda, kan använder partitionering under din inläsningen avsevärt förbättra prestanda.</span><span class="sxs-lookup"><span data-stu-id="a7940-121">While simply inserting, updating or deleting data can be hello most straightforward approach, with a little thought and effort, using partitioning during your load process can substantially improve performance.</span></span>

<span data-ttu-id="a7940-122">Partition växlar kan använda tooquickly ta bort eller ersätta ett avsnitt i en tabell.</span><span class="sxs-lookup"><span data-stu-id="a7940-122">Partition switching can be used tooquickly remove or replace a section of a table.</span></span>  <span data-ttu-id="a7940-123">Till exempel kan försäljning faktatabellen innehålla bara data för hello senaste 36 månaderna.</span><span class="sxs-lookup"><span data-stu-id="a7940-123">For example, a sales fact table might contain just data for hello past 36 months.</span></span>  <span data-ttu-id="a7940-124">Hello slutet av varje månad, tas hello äldsta månads försäljning data bort från hello tabell.</span><span class="sxs-lookup"><span data-stu-id="a7940-124">At hello end of every month, hello oldest month of sales data is deleted from hello table.</span></span>  <span data-ttu-id="a7940-125">Dessa data kan tas bort med hjälp av en delete-instruktion toodelete hello data för hello äldsta månad.</span><span class="sxs-lookup"><span data-stu-id="a7940-125">This data could be deleted by using a delete statement toodelete hello data for hello oldest month.</span></span>  <span data-ttu-id="a7940-126">Men kan om du tar bort en stor mängd data rad för rad med en delete-instruktion ta mycket lång tid, samt skapa hello risken för stora transaktioner, vilket kan ta en lång tid toorollback om något går fel.</span><span class="sxs-lookup"><span data-stu-id="a7940-126">However, deleting a large amount of data row-by-row with a delete statement can take a very long time, as well as create hello risk of large transactions which could take a long time toorollback if something goes wrong.</span></span>  <span data-ttu-id="a7940-127">En mer optimala metod är toosimply släpp hello äldsta partition av data.</span><span class="sxs-lookup"><span data-stu-id="a7940-127">A more optimal approach is toosimply drop hello oldest partition of data.</span></span>  <span data-ttu-id="a7940-128">När du tar bort hello enskilda rader kan ta timmar, ta ta bort en hel partition sekunder.</span><span class="sxs-lookup"><span data-stu-id="a7940-128">Where deleting hello individual rows could take hours, deleting an entire partition could take seconds.</span></span>

### <a name="benefits-tooqueries"></a><span data-ttu-id="a7940-129">Fördelar tooqueries</span><span class="sxs-lookup"><span data-stu-id="a7940-129">Benefits tooqueries</span></span>
<span data-ttu-id="a7940-130">Partitionering kan också vara används tooimprove frågeprestanda.</span><span class="sxs-lookup"><span data-stu-id="a7940-130">Partitioning can also be used tooimprove query performance.</span></span>  <span data-ttu-id="a7940-131">Om en fråga gäller ett filter för en partitionerad kolumn, kan detta begränsa hello genomsökning tooonly hello kvalificerande partitioner som kan vara en mycket mindre deluppsättning av hello data, att undvika en fullständig tabellgenomsökning.</span><span class="sxs-lookup"><span data-stu-id="a7940-131">If a query applies a filter on a partitioned column, this can limit hello scan tooonly hello qualifying partitions which may be a much smaller subset of hello data, avoiding a full table scan.</span></span>  <span data-ttu-id="a7940-132">Hello predikat eliminering prestandafördelarna är mindre användbara med hello införandet av grupperade columnstore-index, men i vissa fall det kan vara en fördel tooqueries.</span><span class="sxs-lookup"><span data-stu-id="a7940-132">With hello introduction of clustered columnstore indexes, hello predicate elimination performance benefits are less beneficial, but in some cases there can be a benefit tooqueries.</span></span>  <span data-ttu-id="a7940-133">Till exempel om hello försäljning faktatabell är partitionerad till 36 månader med hello försäljning datumfält sedan frågor som filtrerar på hello Försäljningsdatum kan hoppa över sökning i partitioner som inte matchar hello filter.</span><span class="sxs-lookup"><span data-stu-id="a7940-133">For example, if hello sales fact table is partitioned into 36 months using hello sales date field, then queries that filter on hello sale date can skip searching in partitions that don’t match hello filter.</span></span>

## <a name="partition-sizing-guidance"></a><span data-ttu-id="a7940-134">Vägledning för partition storlek</span><span class="sxs-lookup"><span data-stu-id="a7940-134">Partition sizing guidance</span></span>
<span data-ttu-id="a7940-135">När partitionering kan vara används tooimprove prestanda vissa scenarier, skapar en tabell med **för många** partitioner kan försämra prestanda under vissa omständigheter.</span><span class="sxs-lookup"><span data-stu-id="a7940-135">While partitioning can be used tooimprove performance some scenarios, creating a table with **too many** partitions can hurt performance under some circumstances.</span></span>  <span data-ttu-id="a7940-136">Dessa problem är särskilt för grupperade columnstore-tabeller.</span><span class="sxs-lookup"><span data-stu-id="a7940-136">These concerns are especially true for clustered columnstore tables.</span></span>  <span data-ttu-id="a7940-137">Partitionering toobe användbara, är det viktigt toounderstand när toouse partitionering och hello antalet partitioner toocreate.</span><span class="sxs-lookup"><span data-stu-id="a7940-137">For partitioning toobe helpful, it is important toounderstand when toouse partitioning and hello number of partitions toocreate.</span></span>  <span data-ttu-id="a7940-138">Det är ingen hårda snabb regel som toohow många partitioner är för många, det beror på dina data och hur många partitioner du läser in toosimultaneously.</span><span class="sxs-lookup"><span data-stu-id="a7940-138">There is no hard fast rule as toohow many partitions are too many, it depends on your data and how many partitions you are loading toosimultaneously.</span></span>  <span data-ttu-id="a7940-139">Men som en allmän tumregel tror att för att lägga till 10-tal too100s av partitioner, inte 1 000-tal.</span><span class="sxs-lookup"><span data-stu-id="a7940-139">But as a general rule of thumb, think of adding 10s too100s of partitions, not 1000s.</span></span>

<span data-ttu-id="a7940-140">När du skapar partitionering på **grupperade columnstore** tabeller, är det viktigt tooconsider hur många rader som hamnar på varje partition.</span><span class="sxs-lookup"><span data-stu-id="a7940-140">When creating partitioning on **clustered columnstore** tables, it is important tooconsider how many rows will land in each partition.</span></span>  <span data-ttu-id="a7940-141">För optimala komprimering och prestanda för grupperade columnstore-tabeller krävs minst 1 miljon rader per distribution och partition.</span><span class="sxs-lookup"><span data-stu-id="a7940-141">For optimal compression and performance of clustered columnstore tables, a minimum of 1 million rows per distribution and partition is needed.</span></span>  <span data-ttu-id="a7940-142">Innan partitioner skapas, delas SQL Data Warehouse redan varje tabell i 60 distribuerade databaser.</span><span class="sxs-lookup"><span data-stu-id="a7940-142">Before partitions are created, SQL Data Warehouse already divides each table into 60 distributed databases.</span></span>  <span data-ttu-id="a7940-143">Alla partitionering tillagda tooa tabellen är dessutom toohello distributioner som skapats hello bakgrunden.</span><span class="sxs-lookup"><span data-stu-id="a7940-143">Any partitioning added tooa table is in addition toohello distributions created behind hello scenes.</span></span>  <span data-ttu-id="a7940-144">Med det här exemplet, om hello försäljning faktatabell innehåller 36 månatliga partitioner och med hänsyn till att SQL Data Warehouse har 60 distributioner sedan hello försäljning fakta tabellen ska innehålla 60 miljoner rader per månad eller 2.1 miljarder rader när alla månader fylls i.</span><span class="sxs-lookup"><span data-stu-id="a7940-144">Using this example, if hello sales fact table contained 36 monthly partitions, and given that SQL Data Warehouse has 60 distributions, then hello sales fact table should contain 60 million rows per month, or 2.1 billion rows when all months are populated.</span></span>  <span data-ttu-id="a7940-145">Om en tabell innehåller avsevärt färre rader än hello Rekommenderat minsta antal rader per partition, Överväg att använda färre partitioner i ordning toomake öka hello antalet rader per partition.</span><span class="sxs-lookup"><span data-stu-id="a7940-145">If a table contains significantly less rows than hello recommended minimum number of rows per partition, consider using fewer partitions in order toomake increase hello number of rows per partition.</span></span>  <span data-ttu-id="a7940-146">Se även hello [indexering] [ Index] artikel som innehåller de förfrågningar som kan köras på SQL Data Warehouse tooassess hello kvaliteten på klustret columnstore-index.</span><span class="sxs-lookup"><span data-stu-id="a7940-146">Also see hello [Indexing][Index] article which includes queries that can be run on SQL Data Warehouse tooassess hello quality of cluster columnstore indexes.</span></span>

## <a name="syntax-difference-from-sql-server"></a><span data-ttu-id="a7940-147">Syntaxen skillnaden från SQL Server</span><span class="sxs-lookup"><span data-stu-id="a7940-147">Syntax difference from SQL Server</span></span>
<span data-ttu-id="a7940-148">SQL Data Warehouse introducerar en förenklad definition av partitioner som skiljer sig från SQL Server.</span><span class="sxs-lookup"><span data-stu-id="a7940-148">SQL Data Warehouse introduces a simplified definition of partitions which is slightly different from SQL Server.</span></span>  <span data-ttu-id="a7940-149">Partitionering funktioner och scheman används inte i SQL Data Warehouse som i SQL Server.</span><span class="sxs-lookup"><span data-stu-id="a7940-149">Partitioning functions and schemes are not used in SQL Data Warehouse as they are in SQL Server.</span></span>  <span data-ttu-id="a7940-150">I stället är allt du behöver toodo identifierar partitionerade kolumn och hello gräns punkter.</span><span class="sxs-lookup"><span data-stu-id="a7940-150">Instead, all you need toodo is identify partitioned column and hello boundary points.</span></span>  <span data-ttu-id="a7940-151">Även om hello syntaxen för partitionering skiljer sig från SQLServer kan är hello grundläggande koncept hello samma.</span><span class="sxs-lookup"><span data-stu-id="a7940-151">While hello syntax of partitioning may be slightly different from SQL Server, hello basic concepts are hello same.</span></span>  <span data-ttu-id="a7940-152">SQL Server och SQL Data Warehouse stöder en partition kolumn per tabell, som kan vara låg.</span><span class="sxs-lookup"><span data-stu-id="a7940-152">SQL Server and SQL Data Warehouse support one partition column per table, which can be ranged partition.</span></span>  <span data-ttu-id="a7940-153">toolearn mer information om partitionering, se [partitionerade tabeller och index][Partitioned Tables and Indexes].</span><span class="sxs-lookup"><span data-stu-id="a7940-153">toolearn more about partitioning, see [Partitioned Tables and Indexes][Partitioned Tables and Indexes].</span></span>

<span data-ttu-id="a7940-154">hello nedan exempel på ett SQL Data Warehouse partitionerad [CREATE TABLE] [ CREATE TABLE] -instruktionen partitionerar hello FactInternetSales tabellen efter hello OrderDateKey kolumnen:</span><span class="sxs-lookup"><span data-stu-id="a7940-154">hello below example of a SQL Data Warehouse partitioned [CREATE TABLE][CREATE TABLE] statement, partitions hello FactInternetSales table on hello OrderDateKey column:</span></span>

```sql
CREATE TABLE [dbo].[FactInternetSales]
(
    [ProductKey]            int          NOT NULL
,   [OrderDateKey]          int          NOT NULL
,   [CustomerKey]           int          NOT NULL
,   [PromotionKey]          int          NOT NULL
,   [SalesOrderNumber]      nvarchar(20) NOT NULL
,   [OrderQuantity]         smallint     NOT NULL
,   [UnitPrice]             money        NOT NULL
,   [SalesAmount]           money        NOT NULL
)
WITH
(   CLUSTERED COLUMNSTORE INDEX
,   DISTRIBUTION = HASH([ProductKey])
,   PARTITION   (   [OrderDateKey] RANGE RIGHT FOR VALUES
                    (20000101,20010101,20020101
                    ,20030101,20040101,20050101
                    )
                )
)
;
```

## <a name="migrating-partitioning-from-sql-server"></a><span data-ttu-id="a7940-155">Migrera partitionering från SQL Server</span><span class="sxs-lookup"><span data-stu-id="a7940-155">Migrating partitioning from SQL Server</span></span>
<span data-ttu-id="a7940-156">toomigrate SQL Server partitions bara definitioner tooSQL Data Warehouse:</span><span class="sxs-lookup"><span data-stu-id="a7940-156">toomigrate SQL Server partition definitions tooSQL Data Warehouse simply:</span></span>

* <span data-ttu-id="a7940-157">Eliminera hello SQL Server [partitionsschema][partition scheme].</span><span class="sxs-lookup"><span data-stu-id="a7940-157">Eliminate hello SQL Server [partition scheme][partition scheme].</span></span>
* <span data-ttu-id="a7940-158">Lägg till hello [partitionsfunktioner] [ partition function] definition tooyour Skapa tabell.</span><span class="sxs-lookup"><span data-stu-id="a7940-158">Add hello [partition function][partition function] definition tooyour CREATE TABLE.</span></span>

<span data-ttu-id="a7940-159">Om du migrerar en partitionerad tabell från en SQL Server-instansen hello nedan SQL kan hjälpa dig toointerrogate hello antalet rader som finns i varje partition.</span><span class="sxs-lookup"><span data-stu-id="a7940-159">If you are migrating a partitioned table from a SQL Server instance hello below SQL can help you toointerrogate hello number of rows that are in each partition.</span></span>  <span data-ttu-id="a7940-160">Tänk på att om hello samma partitionering detaljnivå används i SQL Data Warehouse, minska hello antalet rader per partition med en faktor på 60.</span><span class="sxs-lookup"><span data-stu-id="a7940-160">Keep in mind that if hello same partitioning granularity is used on SQL Data Warehouse, hello number of rows per partition will decrease by a factor of 60.</span></span>  

```sql
-- Partition information for a SQL Server Database
SELECT      s.[name]                        AS      [schema_name]
,           t.[name]                        AS      [table_name]
,           i.[name]                        AS      [index_name]
,           p.[partition_number]            AS      [partition_number]
,           SUM(a.[used_pages]*8.0)         AS      [partition_size_kb]
,           SUM(a.[used_pages]*8.0)/1024    AS      [partition_size_mb]
,           SUM(a.[used_pages]*8.0)/1048576 AS      [partition_size_gb]
,           p.[rows]                        AS      [partition_row_count]
,           rv.[value]                      AS      [partition_boundary_value]
,           p.[data_compression_desc]       AS      [partition_compression_desc]
FROM        sys.schemas s
JOIN        sys.tables t                    ON      t.[schema_id]         = s.[schema_id]
JOIN        sys.partitions p                ON      p.[object_id]         = t.[object_id]
JOIN        sys.allocation_units a          ON      a.[container_id]      = p.[partition_id]
JOIN        sys.indexes i                   ON      i.[object_id]         = p.[object_id]
                                            AND     i.[index_id]          = p.[index_id]
JOIN        sys.data_spaces ds              ON      ds.[data_space_id]    = i.[data_space_id]
LEFT JOIN   sys.partition_schemes ps        ON      ps.[data_space_id]    = ds.[data_space_id]
LEFT JOIN   sys.partition_functions pf      ON      pf.[function_id]      = ps.[function_id]
LEFT JOIN   sys.partition_range_values rv   ON      rv.[function_id]      = pf.[function_id]
                                            AND     rv.[boundary_id]      = p.[partition_number]
WHERE       p.[index_id] <=1
GROUP BY    s.[name]
,           t.[name]
,           i.[name]
,           p.[partition_number]
,           p.[rows]
,           rv.[value]
,           p.[data_compression_desc]
;
```

## <a name="workload-management"></a><span data-ttu-id="a7940-161">Arbetsbelastningshantering</span><span class="sxs-lookup"><span data-stu-id="a7940-161">Workload management</span></span>
<span data-ttu-id="a7940-162">En sista övervägande toofactor i toohello tabell partition beslut är [hantering av arbetsbelastning][workload management].</span><span class="sxs-lookup"><span data-stu-id="a7940-162">One final piece consideration toofactor in toohello table partition decision is [workload management][workload management].</span></span>  <span data-ttu-id="a7940-163">Hantering av arbetsbelastning i SQL Data Warehouse är främst hello hantering av minne och samtidighet.</span><span class="sxs-lookup"><span data-stu-id="a7940-163">Workload management in SQL Data Warehouse is primarily hello management of memory and concurrency.</span></span>  <span data-ttu-id="a7940-164">I SQL Data Warehouse hello är maximalt minne som allokerats tooeach distribution vid körning av fråga styrda resursklasser.</span><span class="sxs-lookup"><span data-stu-id="a7940-164">In SQL Data Warehouse hello maximum memory allocated tooeach distribution during query execution is governed resource classes.</span></span>  <span data-ttu-id="a7940-165">Helst ska partitionerna storleksändras med hänsyn till andra faktorer som hello minne som behövs för att skapa grupperade columnstore-index.</span><span class="sxs-lookup"><span data-stu-id="a7940-165">Ideally your partitions will be sized in consideration of other factors like hello memory needs of building clustered columnstore indexes.</span></span>  <span data-ttu-id="a7940-166">Grupperade columnstore-index förmånen avsevärt när de tilldelas mer minne.</span><span class="sxs-lookup"><span data-stu-id="a7940-166">Clustered columnstore indexes benefit greatly when they are allocated more memory.</span></span>  <span data-ttu-id="a7940-167">Därför vill du tooensure återskapa ett Partitionsindex inte är för lite minne.</span><span class="sxs-lookup"><span data-stu-id="a7940-167">Therefore, you will want tooensure that a partition index rebuild is not starved of memory.</span></span> <span data-ttu-id="a7940-168">Öka hello mängden minne tillgängligt tooyour fråga kan uppnås genom att byta från hello standardroll, smallrc, tooone av hello andra roller, till exempel largerc.</span><span class="sxs-lookup"><span data-stu-id="a7940-168">Increasing hello amount of memory available tooyour query can be achieved by switching from hello default role, smallrc, tooone of hello other roles such as largerc.</span></span>

<span data-ttu-id="a7940-169">Information om hello fördelningen av minne per distribution är tillgänglig genom att fråga hello resurs resursstyrningen dynamiska hanteringsvyer.</span><span class="sxs-lookup"><span data-stu-id="a7940-169">Information on hello allocation of memory per distribution is available by querying hello resource governor dynamic management views.</span></span> <span data-ttu-id="a7940-170">I verkligheten är din minnestilldelningen mindre än hello figurerna nedan.</span><span class="sxs-lookup"><span data-stu-id="a7940-170">In reality your memory grant will be less than hello figures below.</span></span> <span data-ttu-id="a7940-171">Detta ger dock en nivå som du kan använda vid bedömning av partitionerna för hanteringsåtgärder för data.</span><span class="sxs-lookup"><span data-stu-id="a7940-171">However, this provides a level of guidance that you can use when sizing your partitions for data management operations.</span></span>  <span data-ttu-id="a7940-172">Försök tooavoid storleksanpassa partitionerna utöver hello minnestilldelningen som tillhandahålls av hello extra stor resursklassen.</span><span class="sxs-lookup"><span data-stu-id="a7940-172">Try tooavoid sizing your partitions beyond hello memory grant provided by hello extra large resource class.</span></span> <span data-ttu-id="a7940-173">Om din partitioner växer utöver denna bild kör hello risken för minnesbelastning vilket i sin tur leder tooless optimala komprimering.</span><span class="sxs-lookup"><span data-stu-id="a7940-173">If your partitions grow beyond this figure you run hello risk of memory pressure which in turn leads tooless optimal compression.</span></span>

```sql
SELECT  rp.[name]                                AS [pool_name]
,       rp.[max_memory_kb]                        AS [max_memory_kb]
,       rp.[max_memory_kb]/1024                    AS [max_memory_mb]
,       rp.[max_memory_kb]/1048576                AS [mex_memory_gb]
,       rp.[max_memory_percent]                    AS [max_memory_percent]
,       wg.[name]                                AS [group_name]
,       wg.[importance]                            AS [group_importance]
,       wg.[request_max_memory_grant_percent]    AS [request_max_memory_grant_percent]
FROM    sys.dm_pdw_nodes_resource_governor_workload_groups    wg
JOIN    sys.dm_pdw_nodes_resource_governor_resource_pools    rp ON wg.[pool_id] = rp.[pool_id]
WHERE   wg.[name] like 'SloDWGroup%'
AND     rp.[name]    = 'SloDWPool'
;
```

## <a name="partition-switching"></a><span data-ttu-id="a7940-174">Växla partition</span><span class="sxs-lookup"><span data-stu-id="a7940-174">Partition switching</span></span>
<span data-ttu-id="a7940-175">SQL Data Warehouse stöder partition dela sammanslagning och växlar.</span><span class="sxs-lookup"><span data-stu-id="a7940-175">SQL Data Warehouse supports partition splitting, merging, and switching.</span></span> <span data-ttu-id="a7940-176">Dessa funktioner är excuted med hello [ALTER TABLE] [ ALTER TABLE] instruktionen.</span><span class="sxs-lookup"><span data-stu-id="a7940-176">Each of these functions is excuted using hello [ALTER TABLE][ALTER TABLE] statement.</span></span>

<span data-ttu-id="a7940-177">tooswitch partitioner mellan två tabeller måste du kontrollera att justera hello partitioner på deras respektive gränser och att det matchar hello definitioner.</span><span class="sxs-lookup"><span data-stu-id="a7940-177">tooswitch partitions between two tables you must ensure that hello partitions align on their respective boundaries and that hello table definitions match.</span></span> <span data-ttu-id="a7940-178">Eftersom de inte tillgängliga Kontrollbegränsningar tooenforce hello värdeintervallet i en tabell hello källtabellen måste innehålla hello samma partitions gränser som hello måltabellen.</span><span class="sxs-lookup"><span data-stu-id="a7940-178">As check constraints are not available tooenforce hello range of values in a table hello source table must contain hello same partition boundaries as hello target table.</span></span> <span data-ttu-id="a7940-179">Om detta inte är fallet hello misslyckas hello partitionsväxeln som hello partition metadata inte synkroniseras.</span><span class="sxs-lookup"><span data-stu-id="a7940-179">If this is not hello case, then hello partition switch will fail as hello partition metadata will not be synchronized.</span></span>

### <a name="how-toosplit-a-partition-that-contains-data"></a><span data-ttu-id="a7940-180">Hur toosplit en partition som innehåller data</span><span class="sxs-lookup"><span data-stu-id="a7940-180">How toosplit a partition that contains data</span></span>
<span data-ttu-id="a7940-181">hello effektivaste metoden toosplit en partition som redan innehåller data är toouse en `CTAS` instruktion.</span><span class="sxs-lookup"><span data-stu-id="a7940-181">hello most efficient method toosplit a partition that already contains data is toouse a `CTAS` statement.</span></span> <span data-ttu-id="a7940-182">Om hello partitionerad tabell är ett grupperat columnstore sedan hello tabell partitionen måste vara tom innan du kan dela.</span><span class="sxs-lookup"><span data-stu-id="a7940-182">If hello partitioned table is a clustered columnstore then hello table partition must be empty before it can be split.</span></span>

<span data-ttu-id="a7940-183">Nedan visas partitionerade columnstore exempeltabell som innehåller en rad i varje partition:</span><span class="sxs-lookup"><span data-stu-id="a7940-183">Below is a sample partitioned columnstore table containing one row in each partition:</span></span>

```sql
CREATE TABLE [dbo].[FactInternetSales]
(
        [ProductKey]            int          NOT NULL
    ,   [OrderDateKey]          int          NOT NULL
    ,   [CustomerKey]           int          NOT NULL
    ,   [PromotionKey]          int          NOT NULL
    ,   [SalesOrderNumber]      nvarchar(20) NOT NULL
    ,   [OrderQuantity]         smallint     NOT NULL
    ,   [UnitPrice]             money        NOT NULL
    ,   [SalesAmount]           money        NOT NULL
)
WITH
(   CLUSTERED COLUMNSTORE INDEX
,   DISTRIBUTION = HASH([ProductKey])
,   PARTITION   (   [OrderDateKey] RANGE RIGHT FOR VALUES
                    (20000101
                    )
                )
)
;

INSERT INTO dbo.FactInternetSales
VALUES (1,19990101,1,1,1,1,1,1);
INSERT INTO dbo.FactInternetSales
VALUES (1,20000101,1,1,1,1,1,1);


CREATE STATISTICS Stat_dbo_FactInternetSales_OrderDateKey ON dbo.FactInternetSales(OrderDateKey);
```

> [!NOTE]
> <span data-ttu-id="a7940-184">Genom att skapa hello statistik objektet Kontrollera att tabellmetadata är mer exakt.</span><span class="sxs-lookup"><span data-stu-id="a7940-184">By Creating hello statistic object, we ensure that table metadata is more accurate.</span></span> <span data-ttu-id="a7940-185">Om vi utelämnar statistik skapas sedan använder SQL Data Warehouse standardvärden.</span><span class="sxs-lookup"><span data-stu-id="a7940-185">If we omit creating statistics, then SQL Data Warehouse will use default values.</span></span> <span data-ttu-id="a7940-186">För information om statistik hittar [statistik][statistics].</span><span class="sxs-lookup"><span data-stu-id="a7940-186">For details on statistics please review [statistics][statistics].</span></span>
> 
> 

<span data-ttu-id="a7940-187">Vi kan sedan fråga efter hello radantal med hello `sys.partitions` vyn katalog:</span><span class="sxs-lookup"><span data-stu-id="a7940-187">We can then query for hello row count using hello `sys.partitions` catalog view:</span></span>

```sql
SELECT  QUOTENAME(s.[name])+'.'+QUOTENAME(t.[name]) as Table_name
,       i.[name] as Index_name
,       p.partition_number as Partition_nmbr
,       p.[rows] as Row_count
,       p.[data_compression_desc] as Data_Compression_desc
FROM    sys.partitions p
JOIN    sys.tables     t    ON    p.[object_id]   = t.[object_id]
JOIN    sys.schemas    s    ON    t.[schema_id]   = s.[schema_id]
JOIN    sys.indexes    i    ON    p.[object_id]   = i.[object_Id]
                            AND   p.[index_Id]    = i.[index_Id]
WHERE t.[name] = 'FactInternetSales'
;
```

<span data-ttu-id="a7940-188">Om vi försöker toosplit den här tabellen, kommer vi får ett felmeddelande:</span><span class="sxs-lookup"><span data-stu-id="a7940-188">If we try toosplit this table, we will get an error:</span></span>

```sql
ALTER TABLE FactInternetSales SPLIT RANGE (20010101);
```

<span data-ttu-id="a7940-189">Ignorerad 35346 nivå 15, tillstånd 1, rad 44 SPLIT-satsen i ALTER PARTITION-instruktionen misslyckades eftersom hello partitionen inte är tom.</span><span class="sxs-lookup"><span data-stu-id="a7940-189">Msg 35346, Level 15, State 1, Line 44 SPLIT clause of ALTER PARTITION statement failed because hello partition is not empty.</span></span>  <span data-ttu-id="a7940-190">Endast tomma partitioner kan delas i när ett columnstore-index finns på hello tabell.</span><span class="sxs-lookup"><span data-stu-id="a7940-190">Only empty partitions can be split in when a columnstore index exists on hello table.</span></span> <span data-ttu-id="a7940-191">Överväg att inaktivera hello columnstore-indexet innan ALTER PARTITION-instruktionen hello och återskapa hello columnstore-indexet när ALTER PARTITION har slutförts.</span><span class="sxs-lookup"><span data-stu-id="a7940-191">Consider disabling hello columnstore index before issuing hello ALTER PARTITION statement, then rebuilding hello columnstore index after ALTER PARTITION is complete.</span></span>

<span data-ttu-id="a7940-192">Vi kan dock använda `CTAS` toocreate en ny tabell toohold våra data.</span><span class="sxs-lookup"><span data-stu-id="a7940-192">However, we can use `CTAS` toocreate a new table toohold our data.</span></span>

```sql
CREATE TABLE dbo.FactInternetSales_20000101
    WITH    (   DISTRIBUTION = HASH(ProductKey)
            ,   CLUSTERED COLUMNSTORE INDEX
            ,   PARTITION   (   [OrderDateKey] RANGE RIGHT FOR VALUES
                                (20000101
                                )
                            )
            )
AS
SELECT *
FROM    FactInternetSales
WHERE   1=2
;
```

<span data-ttu-id="a7940-193">En växel tillåts som hello partitionsgränser justeras.</span><span class="sxs-lookup"><span data-stu-id="a7940-193">As hello partition boundaries are aligned a switch is permitted.</span></span> <span data-ttu-id="a7940-194">Detta lämnar hello källtabellen med en tom partition som senare kan vi dela.</span><span class="sxs-lookup"><span data-stu-id="a7940-194">This will leave hello source table with an empty partition that we can subsequently split.</span></span>

```sql
ALTER TABLE FactInternetSales SWITCH PARTITION 2 too FactInternetSales_20000101 PARTITION 2;

ALTER TABLE FactInternetSales SPLIT RANGE (20010101);
```

<span data-ttu-id="a7940-195">Återstår toodo är tooalign våra data toohello partitionera nya gränser med `CTAS` och växla våra data i toohello huvudtabell</span><span class="sxs-lookup"><span data-stu-id="a7940-195">All that is left toodo is tooalign our data toohello new partition boundaries using `CTAS` and switch our data back in toohello main table</span></span>

```sql
CREATE TABLE [dbo].[FactInternetSales_20000101_20010101]
    WITH    (   DISTRIBUTION = HASH([ProductKey])
            ,   CLUSTERED COLUMNSTORE INDEX
            ,   PARTITION   (   [OrderDateKey] RANGE RIGHT FOR VALUES
                                (20000101,20010101
                                )
                            )
            )
AS
SELECT  *
FROM    [dbo].[FactInternetSales_20000101]
WHERE   [OrderDateKey] >= 20000101
AND     [OrderDateKey] <  20010101
;

ALTER TABLE dbo.FactInternetSales_20000101_20010101 SWITCH PARTITION 2 toodbo.FactInternetSales PARTITION 2;
```

<span data-ttu-id="a7940-196">När du har slutfört hello flödet av hello uppgifter är det en bra idé toorefresh hello statistik på hello mål tabell tooensure de motsvarar hello ny distribution av hello data i deras respektive partitioner:</span><span class="sxs-lookup"><span data-stu-id="a7940-196">Once you have completed hello movement of hello data it is a good idea toorefresh hello statistics on hello target table tooensure they accurately reflect hello new distribution of hello data in their respective partitions:</span></span>

```sql
UPDATE STATISTICS [dbo].[FactInternetSales];
```

### <a name="table-partitioning-source-control"></a><span data-ttu-id="a7940-197">Tabell partitionering källkontroll</span><span class="sxs-lookup"><span data-stu-id="a7940-197">Table partitioning source control</span></span>
<span data-ttu-id="a7940-198">tooavoid din tabelldefinitionen från **rostangripet** i ditt källkontrollsystem kanske du vill tooconsider hello följande metod:</span><span class="sxs-lookup"><span data-stu-id="a7940-198">tooavoid your table definition from **rusting** in your source control system you may want tooconsider hello following approach:</span></span>

1. <span data-ttu-id="a7940-199">Skapa hello tabell som en partitionerad tabell men saknar värden för partition</span><span class="sxs-lookup"><span data-stu-id="a7940-199">Create hello table as a partitioned table but with no partition values</span></span>

```sql
CREATE TABLE [dbo].[FactInternetSales]
(
    [ProductKey]            int          NOT NULL
,   [OrderDateKey]          int          NOT NULL
,   [CustomerKey]           int          NOT NULL
,   [PromotionKey]          int          NOT NULL
,   [SalesOrderNumber]      nvarchar(20) NOT NULL
,   [OrderQuantity]         smallint     NOT NULL
,   [UnitPrice]             money        NOT NULL
,   [SalesAmount]           money        NOT NULL
)
WITH
(   CLUSTERED COLUMNSTORE INDEX
,   DISTRIBUTION = HASH([ProductKey])
,   PARTITION   (   [OrderDateKey] RANGE RIGHT FOR VALUES
                    ()
                )
)
;
```

1. <span data-ttu-id="a7940-200">`SPLIT`hello tabell som en del av processen för distribution av hello:</span><span class="sxs-lookup"><span data-stu-id="a7940-200">`SPLIT` hello table as part of hello deployment process:</span></span>

```sql
-- Create a table containing hello partition boundaries

CREATE TABLE #partitions
WITH
(
    LOCATION = USER_DB
,   DISTRIBUTION = HASH(ptn_no)
)
AS
SELECT  ptn_no
,       ROW_NUMBER() OVER (ORDER BY (ptn_no)) as seq_no
FROM    (
        SELECT CAST(20000101 AS INT) ptn_no
        UNION ALL
        SELECT CAST(20010101 AS INT)
        UNION ALL
        SELECT CAST(20020101 AS INT)
        UNION ALL
        SELECT CAST(20030101 AS INT)
        UNION ALL
        SELECT CAST(20040101 AS INT)
        ) a
;

-- Iterate over hello partition boundaries and split hello table

DECLARE @c INT = (SELECT COUNT(*) FROM #partitions)
,       @i INT = 1                                 --iterator for while loop
,       @q NVARCHAR(4000)                          --query
,       @p NVARCHAR(20)     = N''                  --partition_number
,       @s NVARCHAR(128)    = N'dbo'               --schema
,       @t NVARCHAR(128)    = N'FactInternetSales' --table
;

WHILE @i <= @c
BEGIN
    SET @p = (SELECT ptn_no FROM #partitions WHERE seq_no = @i);
    SET @q = (SELECT N'ALTER TABLE '+@s+N'.'+@t+N' SPLIT RANGE ('+@p+N');');

    -- PRINT @q;
    EXECUTE sp_executesql @q;

    SET @i+=1;
END

-- Code clean-up

DROP TABLE #partitions;
```

<span data-ttu-id="a7940-201">Med den här metoden hello koden i källkontroll är statisk och hello partitionering gränsvärden tillåts toobe dynamiska; under utveckling med hello datalager över tid.</span><span class="sxs-lookup"><span data-stu-id="a7940-201">With this approach hello code in source control remains static and hello partitioning boundary values are allowed toobe dynamic; evolving with hello warehouse over time.</span></span>

## <a name="next-steps"></a><span data-ttu-id="a7940-202">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="a7940-202">Next steps</span></span>
<span data-ttu-id="a7940-203">toolearn finns fler hello artiklar på [tabell översikt][Overview], [Data tabelltyper][Data Types], [distribuerar en tabell] [ Distribute], [Indexering av en tabell][Index], [underhålla tabellstatistik] [ Statistics] och [ Temporära tabeller][Temporary].</span><span class="sxs-lookup"><span data-stu-id="a7940-203">toolearn more, see hello articles on [Table Overview][Overview], [Table Data Types][Data Types], [Distributing a Table][Distribute], [Indexing a Table][Index], [Maintaining Table Statistics][Statistics] and [Temporary Tables][Temporary].</span></span>  <span data-ttu-id="a7940-204">Mer information om metodtips finns [Metodtips för SQL Data Warehouse][SQL Data Warehouse Best Practices].</span><span class="sxs-lookup"><span data-stu-id="a7940-204">For more about best practices, see [SQL Data Warehouse Best Practices][SQL Data Warehouse Best Practices].</span></span>

<!--Image references-->

<!--Article references-->
[Overview]: ./sql-data-warehouse-tables-overview.md
[Data Types]: ./sql-data-warehouse-tables-data-types.md
[Distribute]: ./sql-data-warehouse-tables-distribute.md
[Index]: ./sql-data-warehouse-tables-index.md
[Partition]: ./sql-data-warehouse-tables-partition.md
[Statistics]: ./sql-data-warehouse-tables-statistics.md
[Temporary]: ./sql-data-warehouse-tables-temporary.md
[workload management]: ./sql-data-warehouse-develop-concurrency.md
[SQL Data Warehouse Best Practices]: ./sql-data-warehouse-best-practices.md

<!-- MSDN Articles -->
[Partitioned Tables and Indexes]: https://msdn.microsoft.com/library/ms190787.aspx
[ALTER TABLE]: https://msdn.microsoft.com/en-us/library/ms190273.aspx
[CREATE TABLE]: https://msdn.microsoft.com/library/mt203953.aspx
[partition function]: https://msdn.microsoft.com/library/ms187802.aspx
[partition scheme]: https://msdn.microsoft.com/library/ms179854.aspx


<!-- Other web references -->
