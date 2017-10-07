---
title: aaaOverview tabeller i SQL Data Warehouse | Microsoft Docs
description: "Komma igång med Azure SQL Data Warehouse-tabeller."
services: sql-data-warehouse
documentationcenter: NA
author: shivaniguptamsft
manager: jhubbard
editor: 
ms.assetid: 2114d9ad-c113-43da-859f-419d72604bdf
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: tables
ms.date: 06/29/2016
ms.author: shigu;jrj
ms.openlocfilehash: 4edabcb4b0754bf6c99c2b6b3f0c077749051d9e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="overview-of-tables-in-sql-data-warehouse"></a><span data-ttu-id="58627-103">Översikt över tabeller i SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="58627-103">Overview of tables in SQL Data Warehouse</span></span>
> [!div class="op_single_selector"]
> * <span data-ttu-id="58627-104">[Översikt över][Overview]</span><span class="sxs-lookup"><span data-stu-id="58627-104">[Overview][Overview]</span></span>
> * <span data-ttu-id="58627-105">[Datatyper][Data Types]</span><span class="sxs-lookup"><span data-stu-id="58627-105">[Data Types][Data Types]</span></span>
> * <span data-ttu-id="58627-106">[Distribuera][Distribute]</span><span class="sxs-lookup"><span data-stu-id="58627-106">[Distribute][Distribute]</span></span>
> * <span data-ttu-id="58627-107">[Index][Index]</span><span class="sxs-lookup"><span data-stu-id="58627-107">[Index][Index]</span></span>
> * <span data-ttu-id="58627-108">[Partition][Partition]</span><span class="sxs-lookup"><span data-stu-id="58627-108">[Partition][Partition]</span></span>
> * <span data-ttu-id="58627-109">[Statistik][Statistics]</span><span class="sxs-lookup"><span data-stu-id="58627-109">[Statistics][Statistics]</span></span>
> * <span data-ttu-id="58627-110">[Tillfällig][Temporary]</span><span class="sxs-lookup"><span data-stu-id="58627-110">[Temporary][Temporary]</span></span>
> 
> 

<span data-ttu-id="58627-111">Det är enkelt att komma igång med att skapa tabeller i SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="58627-111">Getting started with creating tables in SQL Data Warehouse is simple.</span></span>  <span data-ttu-id="58627-112">grundläggande hello [CREATE TABLE] [ CREATE TABLE] syntaxen följer hello samma syntax som du är mest sannolika redan bekant med från att arbeta med andra databaser.</span><span class="sxs-lookup"><span data-stu-id="58627-112">hello basic [CREATE TABLE][CREATE TABLE] syntax follows hello common syntax you are most likely already familiar with from working with other databases.</span></span>  <span data-ttu-id="58627-113">toocreate en tabell, du behöver tooname din tabell namnge din kolumner och definiera datatyper för varje kolumn.</span><span class="sxs-lookup"><span data-stu-id="58627-113">toocreate a table, you simply need tooname your table, name your columns and define data types for each column.</span></span>  <span data-ttu-id="58627-114">Om du har skapar tabeller i andra databaser, bör det se ut insatt tooyou.</span><span class="sxs-lookup"><span data-stu-id="58627-114">If you've create tables in other databases, this should look very familiar tooyou.</span></span>

```sql  
CREATE TABLE Customers (FirstName VARCHAR(25), LastName VARCHAR(25))
 ``` 

<span data-ttu-id="58627-115">hello ovanstående exempel skapar en tabell med namnet kunder med två kolumner, Förnamn och efternamn.</span><span class="sxs-lookup"><span data-stu-id="58627-115">hello above example creates a table named Customers with two columns, FirstName and LastName.</span></span>  <span data-ttu-id="58627-116">Varje kolumn har definierats med datatypen VARCHAR(25) som begränsar hello data too25 tecken.</span><span class="sxs-lookup"><span data-stu-id="58627-116">Each column is defined with a data type of VARCHAR(25), which limits hello data too25 characters.</span></span>  <span data-ttu-id="58627-117">Attributen grundläggande för en tabell, samt andra, är främst hello samma som andra databaser.</span><span class="sxs-lookup"><span data-stu-id="58627-117">These fundamental attributes of a table, as well as others, are mostly hello same as other databases.</span></span>  <span data-ttu-id="58627-118">Datatyper har definierats för varje kolumn och kontrollera hello integriteten hos dina data.</span><span class="sxs-lookup"><span data-stu-id="58627-118">Data types are defined for each column and ensure hello integrity of your data.</span></span>  <span data-ttu-id="58627-119">Index kan läggas tooimprove prestanda genom att minska i/o.</span><span class="sxs-lookup"><span data-stu-id="58627-119">Indexes can be added tooimprove performance by reducing I/O.</span></span>  <span data-ttu-id="58627-120">Partitionering kan läggas till tooimprove prestanda när du behöver toomodify data.</span><span class="sxs-lookup"><span data-stu-id="58627-120">Partitioning can be added tooimprove performance when you need toomodify data.</span></span>

<span data-ttu-id="58627-121">[Byta namn på] [ RENAME] en tabell i SQL Data Warehouse ser ut så här:</span><span class="sxs-lookup"><span data-stu-id="58627-121">[Renaming][RENAME] a SQL Data Warehouse table looks like this:</span></span>

```sql  
RENAME OBJECT Customer tooCustomerOrig; 
 ```

## <a name="distributed-tables"></a><span data-ttu-id="58627-122">Distribuerade tabeller</span><span class="sxs-lookup"><span data-stu-id="58627-122">Distributed tables</span></span>
<span data-ttu-id="58627-123">Ett nytt grundläggande attribut som introducerades av distribuerade system som SQL Data Warehouse är hello **distribution kolumnen**.</span><span class="sxs-lookup"><span data-stu-id="58627-123">A new fundamental attribute introduced by distributed systems like SQL Data Warehouse is hello **distribution column**.</span></span>  <span data-ttu-id="58627-124">hello distribution kolumnen är mycket vad det låter som.</span><span class="sxs-lookup"><span data-stu-id="58627-124">hello distribution column is very much what it sounds like.</span></span>  <span data-ttu-id="58627-125">Det är hello-kolumn som avgör hur toodistribute, eller division data hello bakgrunden.</span><span class="sxs-lookup"><span data-stu-id="58627-125">It is hello column that determines how toodistribute, or divide, your data behind hello scenes.</span></span>  <span data-ttu-id="58627-126">När du skapar en tabell utan att ange hello distribution kolumn hello tabell distribueras automatiskt med hjälp av **resursallokering**.</span><span class="sxs-lookup"><span data-stu-id="58627-126">When you create a table without specifying hello distribution column, hello table is automatically distributed using **round robin**.</span></span>  <span data-ttu-id="58627-127">Resursallokering tabeller kan vara tillräckligt i vissa scenarier, kan definiera distributionskolumner avsevärt minska dataflytt under frågor, vilket optimerar prestanda.</span><span class="sxs-lookup"><span data-stu-id="58627-127">While round robin tables can be sufficient in some scenarios, defining distribution columns can greatly reduce data movement during queries, thus optimizing performance.</span></span>  <span data-ttu-id="58627-128">I situationer där det finns en liten mängd data i en tabell, väljer toocreate hello tabell med hello **replikera** distributionstypen kopierar data tooeach compute-nod och sparar flytt av data vid körning i frågan.</span><span class="sxs-lookup"><span data-stu-id="58627-128">In situations where there is a small amount of data in a table, choosing toocreate hello table with hello **replicate** distribution type copies data tooeach compute node and saves data movement at query execution time.</span></span> <span data-ttu-id="58627-129">Se [distribuerar en tabell] [ Distribute] toolearn mer om hur tooselect en kolumn för distribution.</span><span class="sxs-lookup"><span data-stu-id="58627-129">See [Distributing a Table][Distribute] toolearn more about how tooselect a distribution column.</span></span>

## <a name="indexing-and-partitioning-tables"></a><span data-ttu-id="58627-130">Indexering och partitionerar register</span><span class="sxs-lookup"><span data-stu-id="58627-130">Indexing and partitioning tables</span></span>
<span data-ttu-id="58627-131">När du blir mer avancerade med SQL Data Warehouse och vill toooptimize prestanda bör du toolearn mer om tabelldesign.</span><span class="sxs-lookup"><span data-stu-id="58627-131">As you become more advanced in using SQL Data Warehouse and want toooptimize performance, you'll want toolearn more about Table Design.</span></span>  <span data-ttu-id="58627-132">toolearn finns fler hello artiklar på [Data tabelltyper][Data Types], [distribuerar en tabell][Distribute], [indexering tabellen] [ Index] och [partitionering en tabell][Partition].</span><span class="sxs-lookup"><span data-stu-id="58627-132">toolearn more, see hello articles on [Table Data Types][Data Types], [Distributing a Table][Distribute], [Indexing a Table][Index] and  [Partitioning a Table][Partition].</span></span>

## <a name="table-statistics"></a><span data-ttu-id="58627-133">Tabellstatistik</span><span class="sxs-lookup"><span data-stu-id="58627-133">Table statistics</span></span>
<span data-ttu-id="58627-134">Statistik är ett mycket viktigt toogetting hello bästa prestanda utanför ditt SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="58627-134">Statistics are an extremely important toogetting hello best performance out of your SQL Data Warehouse.</span></span>  <span data-ttu-id="58627-135">Eftersom SQL Data Warehouse ännu inte automatiskt skapa och uppdatera statistik, som du har kanske tooexpect i Azure SQL Database, läsa vår artikel [statistik] [ Statistics] kan vara en av hello de viktigaste artiklar som du kan läsa tooensure som du får hello bästa prestanda från dina frågor.</span><span class="sxs-lookup"><span data-stu-id="58627-135">Since SQL Data Warehouse does not yet automatically create and update statistics for you, like you may have come tooexpect in Azure SQL Database, reading our article on [Statistics][Statistics] might be one of hello most important articles you read tooensure that you get hello best performance from your queries.</span></span>

## <a name="temporary-tables"></a><span data-ttu-id="58627-136">Temporära tabeller</span><span class="sxs-lookup"><span data-stu-id="58627-136">Temporary tables</span></span>
<span data-ttu-id="58627-137">Temporära tabeller är tabeller som endast finns hello länge din inloggning och inte kan ses av andra användare.</span><span class="sxs-lookup"><span data-stu-id="58627-137">Temporary tables are tables which only exist for hello duration of your logon and cannot be seen by other users.</span></span>  <span data-ttu-id="58627-138">Temporära tabeller kan vara ett bra sätt tooprevent andra från tillfälliga resultat och också minska hello behovet av att rensa.</span><span class="sxs-lookup"><span data-stu-id="58627-138">Temporary tables can be a good way tooprevent others from seeing temporary results and also reduce hello need for cleanup.</span></span>  <span data-ttu-id="58627-139">Eftersom temporära tabeller kan du också använda lokal lagring, ger de snabbare prestanda för vissa åtgärder.</span><span class="sxs-lookup"><span data-stu-id="58627-139">Since temporary tables also utilize local storage, they can offer faster performance for some operations.</span></span>  <span data-ttu-id="58627-140">Se hello [tillfällig tabell] [ Temporary] artiklar för mer information om temporära tabeller.</span><span class="sxs-lookup"><span data-stu-id="58627-140">See hello [Temporary Table][Temporary] articles for more details about temporary tables.</span></span>

## <a name="external-tables"></a><span data-ttu-id="58627-141">Externa tabeller</span><span class="sxs-lookup"><span data-stu-id="58627-141">External tables</span></span>
<span data-ttu-id="58627-142">Externa tabeller, även kallat Polybase tabeller är tabeller som kan efterfrågas från SQL Data Warehouse men punkt toodata externa från SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="58627-142">External tables, also known as Polybase tables, are tables which can be queried from SQL Data Warehouse, but point toodata external from SQL Data Warehouse.</span></span>  <span data-ttu-id="58627-143">Du kan till exempel skapa en extern tabell toofiles som pekar på Azure Blob Storage.</span><span class="sxs-lookup"><span data-stu-id="58627-143">For example, you can create an external table which points toofiles on Azure Blob Storage.</span></span>  <span data-ttu-id="58627-144">Mer information om hur toocreate och fråga en extern tabell Se [Läs in data med Polybase][Load data with Polybase].</span><span class="sxs-lookup"><span data-stu-id="58627-144">For more details on how toocreate and query an external table, see [Load data with Polybase][Load data with Polybase].</span></span>  

## <a name="unsupported-table-features"></a><span data-ttu-id="58627-145">Funktioner som inte stöds tabell</span><span class="sxs-lookup"><span data-stu-id="58627-145">Unsupported table features</span></span>
<span data-ttu-id="58627-146">SQL Data Warehouse innehåller många hello samma tabell funktioner som erbjuds av andra databaser, finns men det vissa funktioner som ännu inte stöds.</span><span class="sxs-lookup"><span data-stu-id="58627-146">While SQL Data Warehouse contains many of hello same table features offered by other databases, there are some features which are not yet supported.</span></span>  <span data-ttu-id="58627-147">Nedan visas en lista över några hello table-funktioner som ännu inte stöds.</span><span class="sxs-lookup"><span data-stu-id="58627-147">Below is a list of some of hello table features which are not yet supported.</span></span>

| <span data-ttu-id="58627-148">Funktioner som inte stöds</span><span class="sxs-lookup"><span data-stu-id="58627-148">Unsupported features</span></span> |
| --- |
| <span data-ttu-id="58627-149">Primär nyckel, sekundärnycklar unika och kontrollera [tabell begränsningar][Table Constraints]</span><span class="sxs-lookup"><span data-stu-id="58627-149">Primary key, Foreign keys, Unique and Check [Table Constraints][Table Constraints]</span></span> |
| <span data-ttu-id="58627-150">[Unika index][Unique Indexes]</span><span class="sxs-lookup"><span data-stu-id="58627-150">[Unique Indexes][Unique Indexes]</span></span> |
| <span data-ttu-id="58627-151">[Beräknade kolumner][Computed Columns]</span><span class="sxs-lookup"><span data-stu-id="58627-151">[Computed Columns][Computed Columns]</span></span> |
| <span data-ttu-id="58627-152">[Glesa kolumner][Sparse Columns]</span><span class="sxs-lookup"><span data-stu-id="58627-152">[Sparse Columns][Sparse Columns]</span></span> |
| <span data-ttu-id="58627-153">[Användardefinierade typer][User-Defined Types]</span><span class="sxs-lookup"><span data-stu-id="58627-153">[User-Defined Types][User-Defined Types]</span></span> |
| <span data-ttu-id="58627-154">[Sekvens][Sequence]</span><span class="sxs-lookup"><span data-stu-id="58627-154">[Sequence][Sequence]</span></span> |
| <span data-ttu-id="58627-155">[Utlösare][Triggers]</span><span class="sxs-lookup"><span data-stu-id="58627-155">[Triggers][Triggers]</span></span> |
| <span data-ttu-id="58627-156">[Indexerade vyer][Indexed Views]</span><span class="sxs-lookup"><span data-stu-id="58627-156">[Indexed Views][Indexed Views]</span></span> |
| <span data-ttu-id="58627-157">[Synonymer][Synonyms]</span><span class="sxs-lookup"><span data-stu-id="58627-157">[Synonyms][Synonyms]</span></span> |

## <a name="table-size-queries"></a><span data-ttu-id="58627-158">Tabell storlek frågor</span><span class="sxs-lookup"><span data-stu-id="58627-158">Table size queries</span></span>
<span data-ttu-id="58627-159">Ett enkelt sätt tooidentify utrymme och rader som används av en tabell i var och en av hello 60 distributioner är toouse [DBCC PDW_SHOWSPACEUSED][DBCC PDW_SHOWSPACEUSED].</span><span class="sxs-lookup"><span data-stu-id="58627-159">One simple way tooidentify space and rows consumed by a table in each of hello 60 distributions, is toouse [DBCC PDW_SHOWSPACEUSED][DBCC PDW_SHOWSPACEUSED].</span></span>

```sql
DBCC PDW_SHOWSPACEUSED('dbo.FactInternetSales');
```

<span data-ttu-id="58627-160">Men kan med hjälp av DBCC-kommandon ganska begränsa.</span><span class="sxs-lookup"><span data-stu-id="58627-160">However, using DBCC commands can be quite limiting.</span></span>  <span data-ttu-id="58627-161">Dynamiska hanteringsvyer (av DMV: er) kan du toosee mycket mer detaljerad information som ger dig mycket större kontroll över hello frågeresultat.</span><span class="sxs-lookup"><span data-stu-id="58627-161">Dynamic management views (DMVs) will allow you toosee much more detail as well as give you much greater control over hello query results.</span></span>  <span data-ttu-id="58627-162">Börja med att skapa den här vyn, blir refererad tooby många av våra exempel i den här och andra artiklar.</span><span class="sxs-lookup"><span data-stu-id="58627-162">Start by creating this view, which will be referred tooby many of our examples in this and other articles.</span></span>

```sql
CREATE VIEW dbo.vTableSizes
AS
WITH base
AS
(
SELECT 
 GETDATE()                                                             AS  [execution_time]
, DB_NAME()                                                            AS  [database_name]
, s.name                                                               AS  [schema_name]
, t.name                                                               AS  [table_name]
, QUOTENAME(s.name)+'.'+QUOTENAME(t.name)                              AS  [two_part_name]
, nt.[name]                                                            AS  [node_table_name]
, ROW_NUMBER() OVER(PARTITION BY nt.[name] ORDER BY (SELECT NULL))     AS  [node_table_name_seq]
, tp.[distribution_policy_desc]                                        AS  [distribution_policy_name]
, c.[name]                                                             AS  [distribution_column]
, nt.[distribution_id]                                                 AS  [distribution_id]
, i.[type]                                                             AS  [index_type]
, i.[type_desc]                                                        AS  [index_type_desc]
, nt.[pdw_node_id]                                                     AS  [pdw_node_id]
, pn.[type]                                                            AS  [pdw_node_type]
, pn.[name]                                                            AS  [pdw_node_name]
, di.name                                                              AS  [dist_name]
, di.position                                                          AS  [dist_position]
, nps.[partition_number]                                               AS  [partition_nmbr]
, nps.[reserved_page_count]                                            AS  [reserved_space_page_count]
, nps.[reserved_page_count] - nps.[used_page_count]                    AS  [unused_space_page_count]
, nps.[in_row_data_page_count] 
    + nps.[row_overflow_used_page_count] 
    + nps.[lob_used_page_count]                                        AS  [data_space_page_count]
, nps.[reserved_page_count] 
 - (nps.[reserved_page_count] - nps.[used_page_count]) 
 - ([in_row_data_page_count] 
         + [row_overflow_used_page_count]+[lob_used_page_count])       AS  [index_space_page_count]
, nps.[row_count]                                                      AS  [row_count]
from 
    sys.schemas s
INNER JOIN sys.tables t
    ON s.[schema_id] = t.[schema_id]
INNER JOIN sys.indexes i
    ON  t.[object_id] = i.[object_id]
    AND i.[index_id] <= 1
INNER JOIN sys.pdw_table_distribution_properties tp
    ON t.[object_id] = tp.[object_id]
INNER JOIN sys.pdw_table_mappings tm
    ON t.[object_id] = tm.[object_id]
INNER JOIN sys.pdw_nodes_tables nt
    ON tm.[physical_name] = nt.[name]
INNER JOIN sys.dm_pdw_nodes pn
    ON  nt.[pdw_node_id] = pn.[pdw_node_id]
INNER JOIN sys.pdw_distributions di
    ON  nt.[distribution_id] = di.[distribution_id]
INNER JOIN sys.dm_pdw_nodes_db_partition_stats nps
    ON nt.[object_id] = nps.[object_id]
    AND nt.[pdw_node_id] = nps.[pdw_node_id]
    AND nt.[distribution_id] = nps.[distribution_id]
LEFT OUTER JOIN (select * from sys.pdw_column_distribution_properties where distribution_ordinal = 1) cdp
    ON t.[object_id] = cdp.[object_id]
LEFT OUTER JOIN sys.columns c
    ON cdp.[object_id] = c.[object_id]
    AND cdp.[column_id] = c.[column_id]
)
, size
AS
(
SELECT
   [execution_time]
,  [database_name]
,  [schema_name]
,  [table_name]
,  [two_part_name]
,  [node_table_name]
,  [node_table_name_seq]
,  [distribution_policy_name]
,  [distribution_column]
,  [distribution_id]
,  [index_type]
,  [index_type_desc]
,  [pdw_node_id]
,  [pdw_node_type]
,  [pdw_node_name]
,  [dist_name]
,  [dist_position]
,  [partition_nmbr]
,  [reserved_space_page_count]
,  [unused_space_page_count]
,  [data_space_page_count]
,  [index_space_page_count]
,  [row_count]
,  ([reserved_space_page_count] * 8.0)                                 AS [reserved_space_KB]
,  ([reserved_space_page_count] * 8.0)/1000                            AS [reserved_space_MB]
,  ([reserved_space_page_count] * 8.0)/1000000                         AS [reserved_space_GB]
,  ([reserved_space_page_count] * 8.0)/1000000000                      AS [reserved_space_TB]
,  ([unused_space_page_count]   * 8.0)                                 AS [unused_space_KB]
,  ([unused_space_page_count]   * 8.0)/1000                            AS [unused_space_MB]
,  ([unused_space_page_count]   * 8.0)/1000000                         AS [unused_space_GB]
,  ([unused_space_page_count]   * 8.0)/1000000000                      AS [unused_space_TB]
,  ([data_space_page_count]     * 8.0)                                 AS [data_space_KB]
,  ([data_space_page_count]     * 8.0)/1000                            AS [data_space_MB]
,  ([data_space_page_count]     * 8.0)/1000000                         AS [data_space_GB]
,  ([data_space_page_count]     * 8.0)/1000000000                      AS [data_space_TB]
,  ([index_space_page_count]  * 8.0)                                   AS [index_space_KB]
,  ([index_space_page_count]  * 8.0)/1000                              AS [index_space_MB]
,  ([index_space_page_count]  * 8.0)/1000000                           AS [index_space_GB]
,  ([index_space_page_count]  * 8.0)/1000000000                        AS [index_space_TB]
FROM base
)
SELECT * 
FROM size
;
```

### <a name="table-space-summary"></a><span data-ttu-id="58627-163">Översikt över tabellutrymme</span><span class="sxs-lookup"><span data-stu-id="58627-163">Table space summary</span></span>
<span data-ttu-id="58627-164">Den här frågan returnerar hello rader och utrymme genom att tabellen.</span><span class="sxs-lookup"><span data-stu-id="58627-164">This query returns hello rows and space by table.</span></span>  <span data-ttu-id="58627-165">Det är en bra fråga toosee vilka tabeller som är dina största tabeller och om de är resursallokering replikerade eller distribuerade hash.</span><span class="sxs-lookup"><span data-stu-id="58627-165">It is a great query toosee which tables are your largest tables and whether they are round robin, replicated or hash distributed.</span></span>  <span data-ttu-id="58627-166">För distribuerade hash-tabeller visar även hello distribution kolumn.</span><span class="sxs-lookup"><span data-stu-id="58627-166">For hash distributed tables it also shows hello distribution column.</span></span>  <span data-ttu-id="58627-167">I de flesta fall ska största tabellerna hash som distribueras med ett grupperat columnstore-index.</span><span class="sxs-lookup"><span data-stu-id="58627-167">In most cases your largest tables should be hash distributed with a clustered columnstore index.</span></span>

```sql
SELECT 
     database_name
,    schema_name
,    table_name
,    distribution_policy_name
,      distribution_column
,    index_type_desc
,    COUNT(distinct partition_nmbr) as nbr_partitions
,    SUM(row_count)                 as table_row_count
,    SUM(reserved_space_GB)         as table_reserved_space_GB
,    SUM(data_space_GB)             as table_data_space_GB
,    SUM(index_space_GB)            as table_index_space_GB
,    SUM(unused_space_GB)           as table_unused_space_GB
FROM 
    dbo.vTableSizes
GROUP BY 
     database_name
,    schema_name
,    table_name
,    distribution_policy_name
,      distribution_column
,    index_type_desc
ORDER BY
    table_reserved_space_GB desc
;
```

### <a name="table-space-by-distribution-type"></a><span data-ttu-id="58627-168">Tabellutrymme av distributionstypen</span><span class="sxs-lookup"><span data-stu-id="58627-168">Table space by distribution type</span></span>
```sql
SELECT 
     distribution_policy_name
,    SUM(row_count)                as table_type_row_count
,    SUM(reserved_space_GB)        as table_type_reserved_space_GB
,    SUM(data_space_GB)            as table_type_data_space_GB
,    SUM(index_space_GB)           as table_type_index_space_GB
,    SUM(unused_space_GB)          as table_type_unused_space_GB
FROM dbo.vTableSizes
GROUP BY distribution_policy_name
;
```

### <a name="table-space-by-index-type"></a><span data-ttu-id="58627-169">Tabellutrymme av Indextypen</span><span class="sxs-lookup"><span data-stu-id="58627-169">Table space by index type</span></span>
```sql
SELECT 
     index_type_desc
,    SUM(row_count)                as table_type_row_count
,    SUM(reserved_space_GB)        as table_type_reserved_space_GB
,    SUM(data_space_GB)            as table_type_data_space_GB
,    SUM(index_space_GB)           as table_type_index_space_GB
,    SUM(unused_space_GB)          as table_type_unused_space_GB
FROM dbo.vTableSizes
GROUP BY index_type_desc
;
```

### <a name="distribution-space-summary"></a><span data-ttu-id="58627-170">Distribution utrymme sammanfattning</span><span class="sxs-lookup"><span data-stu-id="58627-170">Distribution space summary</span></span>
```sql
SELECT 
    distribution_id
,    SUM(row_count)                as total_node_distribution_row_count
,    SUM(reserved_space_MB)        as total_node_distribution_reserved_space_MB
,    SUM(data_space_MB)            as total_node_distribution_data_space_MB
,    SUM(index_space_MB)           as total_node_distribution_index_space_MB
,    SUM(unused_space_MB)          as total_node_distribution_unused_space_MB
FROM dbo.vTableSizes
GROUP BY     distribution_id
ORDER BY    distribution_id
;
```

## <a name="next-steps"></a><span data-ttu-id="58627-171">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="58627-171">Next steps</span></span>
<span data-ttu-id="58627-172">toolearn finns fler hello artiklar på [Data tabelltyper][Data Types], [distribuerar en tabell][Distribute], [indexering tabellen] [ Index], [Partitionering en tabell][Partition], [underhålla tabellstatistik] [ Statistics] och [ Temporära tabeller][Temporary].</span><span class="sxs-lookup"><span data-stu-id="58627-172">toolearn more, see hello articles on [Table Data Types][Data Types], [Distributing a Table][Distribute], [Indexing a Table][Index],  [Partitioning a Table][Partition], [Maintaining Table Statistics][Statistics] and [Temporary Tables][Temporary].</span></span>  <span data-ttu-id="58627-173">Mer information om metodtips finns [Metodtips för SQL Data Warehouse][SQL Data Warehouse Best Practices].</span><span class="sxs-lookup"><span data-stu-id="58627-173">For more about best practices, see [SQL Data Warehouse Best Practices][SQL Data Warehouse Best Practices].</span></span>

<!--Image references-->

<!--Article references-->
[Overview]: ./sql-data-warehouse-tables-overview.md
[Data Types]: ./sql-data-warehouse-tables-data-types.md
[Distribute]: ./sql-data-warehouse-tables-distribute.md
[Index]: ./sql-data-warehouse-tables-index.md
[Partition]: ./sql-data-warehouse-tables-partition.md
[Statistics]: ./sql-data-warehouse-tables-statistics.md
[Temporary]: ./sql-data-warehouse-tables-temporary.md
[SQL Data Warehouse Best Practices]: ./sql-data-warehouse-best-practices.md
[Load data with Polybase]: ./sql-data-warehouse-load-from-azure-blob-storage-with-polybase.md

<!--MSDN references-->
[CREATE TABLE]: https://msdn.microsoft.com/library/mt203953.aspx
[RENAME]: https://msdn.microsoft.com/library/mt631611.aspx
[DBCC PDW_SHOWSPACEUSED]: https://msdn.microsoft.com/library/mt204028.aspx
[Table Constraints]: https://msdn.microsoft.com/library/ms188066.aspx
[Computed Columns]: https://msdn.microsoft.com/library/ms186241.aspx
[Sparse Columns]: https://msdn.microsoft.com/library/cc280604.aspx
[User-Defined Types]: https://msdn.microsoft.com/library/ms131694.aspx
[Sequence]: https://msdn.microsoft.com/library/ff878091.aspx
[Triggers]: https://msdn.microsoft.com/library/ms189799.aspx
[Indexed Views]: https://msdn.microsoft.com/library/ms191432.aspx
[Synonyms]: https://msdn.microsoft.com/library/ms177544.aspx
[Unique Indexes]: https://msdn.microsoft.com/library/ms188783.aspx

<!--Other Web references-->
