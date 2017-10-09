---
title: aaaImprove columnstore-index prestanda i Azure SQL | Microsoft Docs
description: "Sänk minneskrav eller öka hello tillgängligt minne toomaximize hello antalet rader som ett columnstore-index komprimeras i varje radgrupps."
services: sql-data-warehouse
documentationcenter: NA
author: shivaniguptamsft
manager: jhubbard
editor: 
ms.assetid: ef170f39-ae24-4b04-af76-53bb4c4d16d3
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: performance
ms.date: 6/2/2017
ms.author: shigu;barbkess
ms.openlocfilehash: 2c5a68435aa200236a2dc8538aa4638b52a59093
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="maximizing-rowgroup-quality-for-columnstore"></a><span data-ttu-id="1b639-103">Maximera radgrupps kvalitet för columnstore</span><span class="sxs-lookup"><span data-stu-id="1b639-103">Maximizing rowgroup quality for columnstore</span></span>

<span data-ttu-id="1b639-104">Radgrupps kvalitet bestäms av hello antalet rader i en radgrupps.</span><span class="sxs-lookup"><span data-stu-id="1b639-104">Rowgroup quality is determined by hello number of rows in a rowgroup.</span></span> <span data-ttu-id="1b639-105">Sänk minneskrav eller öka hello tillgängligt minne toomaximize hello antalet rader som ett columnstore-index komprimeras i varje radgrupps.</span><span class="sxs-lookup"><span data-stu-id="1b639-105">Reduce memory requirements or increase hello available memory toomaximize hello number of rows a columnstore index compresses into each rowgroup.</span></span>  <span data-ttu-id="1b639-106">Använd dessa metoder tooimprove komprimering priser och frågeprestanda för columnstore-index.</span><span class="sxs-lookup"><span data-stu-id="1b639-106">Use these methods tooimprove compression rates and query performance for columnstore indexes.</span></span>

## <a name="why-hello-rowgroup-size-matters"></a><span data-ttu-id="1b639-107">Hej radgrupps storlek</span><span class="sxs-lookup"><span data-stu-id="1b639-107">Why hello rowgroup size matters</span></span>
<span data-ttu-id="1b639-108">Eftersom ett columnstore-index skannar en tabell genom att skanna kolumnsegment med enskilda rowgroups, förbättrar maximera hello antalet rader i varje radgrupps prestanda för frågor.</span><span class="sxs-lookup"><span data-stu-id="1b639-108">Since a columnstore index scans a table by scanning column segments of individual rowgroups, maximizing hello number of rows in each rowgroup enhances query performance.</span></span> <span data-ttu-id="1b639-109">När rowgroups har ett stort antal rader, förbättrar datakomprimering vilket innebär att det finns mindre data tooread från disken.</span><span class="sxs-lookup"><span data-stu-id="1b639-109">When rowgroups have a high number of rows, data compression improves which means there is less data tooread from disk.</span></span>

<span data-ttu-id="1b639-110">Läs mer om rowgroups [Columnstore-index guiden](https://msdn.microsoft.com/library/gg492088.aspx).</span><span class="sxs-lookup"><span data-stu-id="1b639-110">For more information about rowgroups, see [Columnstore Indexes Guide](https://msdn.microsoft.com/library/gg492088.aspx).</span></span>

## <a name="target-size-for-rowgroups"></a><span data-ttu-id="1b639-111">Målstorlek för rowgroups</span><span class="sxs-lookup"><span data-stu-id="1b639-111">Target size for rowgroups</span></span>
<span data-ttu-id="1b639-112">För bästa möjliga prestanda är hello målet toomaximize hello antalet rader per radgrupps i ett columnstore-index.</span><span class="sxs-lookup"><span data-stu-id="1b639-112">For best query performance, hello goal is toomaximize hello number of rows per rowgroup in a columnstore index.</span></span> <span data-ttu-id="1b639-113">En radgrupps kan ha högst 1 048 576 rader.</span><span class="sxs-lookup"><span data-stu-id="1b639-113">A rowgroup can have a maximum of 1,048,576 rows.</span></span> <span data-ttu-id="1b639-114">Bra toonot har hello maximalt antal rader per radgrupps.</span><span class="sxs-lookup"><span data-stu-id="1b639-114">It's okay toonot have hello maximum number of rows per rowgroup.</span></span> <span data-ttu-id="1b639-115">Columnstore-index uppnå goda prestanda när rowgroups har minst 100 000 rader.</span><span class="sxs-lookup"><span data-stu-id="1b639-115">Columnstore indexes achieve good performance when rowgroups have at least 100,000 rows.</span></span>

## <a name="rowgroups-can-get-trimmed-during-compression"></a><span data-ttu-id="1b639-116">Rowgroups kan hämta trimmade under komprimering</span><span class="sxs-lookup"><span data-stu-id="1b639-116">Rowgroups can get trimmed during compression</span></span>

<span data-ttu-id="1b639-117">Under en bulk load eller columnstore-index återskapning, ibland det finns inte tillräckligt med minne tillgängligt toocompress alla hello rader som anges för varje radgrupps.</span><span class="sxs-lookup"><span data-stu-id="1b639-117">During a bulk load or columnstore index rebuild, sometimes there isn't enough memory available toocompress all hello rows designated for each rowgroup.</span></span> <span data-ttu-id="1b639-118">När minnesbelastning trim columnstore-index hello radgrupps storlekar så komprimering i hello columnstore kan genomföras.</span><span class="sxs-lookup"><span data-stu-id="1b639-118">When there is memory pressure, columnstore indexes trim hello rowgroup sizes so compression into hello columnstore can succeed.</span></span> 

<span data-ttu-id="1b639-119">När det finns inte tillräckligt med minne toocompress minst 10 000 rader i varje radgrupps, genererar ett fel i SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="1b639-119">When there is insufficient memory toocompress at least 10,000 rows into each rowgroup, SQL Data Warehouse generates an error.</span></span>

<span data-ttu-id="1b639-120">Mer information om massinläsning finns [massinläsning till ett grupperat columnstore-index](https://msdn.microsoft.com/en-us/library/dn935008.aspx#Bulk load into a clustered columnstore index).</span><span class="sxs-lookup"><span data-stu-id="1b639-120">For more information on bulk loading, see [Bulk load into a clustered columnstore index](https://msdn.microsoft.com/en-us/library/dn935008.aspx#Bulk load into a clustered columnstore index).</span></span>

## <a name="how-toomonitor-rowgroup-quality"></a><span data-ttu-id="1b639-121">Hur toomonitor radgrupps kvalitet</span><span class="sxs-lookup"><span data-stu-id="1b639-121">How toomonitor rowgroup quality</span></span>

<span data-ttu-id="1b639-122">Det finns en DMV (sys.dm_pdw_nodes_db_column_store_row_group_physical_stats) som visar användbar information, till exempel antalet rader i rowgroups och hello orsak för trimning om det var trimning.</span><span class="sxs-lookup"><span data-stu-id="1b639-122">There is a DMV (sys.dm_pdw_nodes_db_column_store_row_group_physical_stats) that exposes useful information such as number of rows in rowgroups and hello reason for trimming if there was trimming.</span></span> <span data-ttu-id="1b639-123">Du kan skapa följande vy som ett praktiskt sätt tooquery hello DMV tooget informationen på radgrupps trimning.</span><span class="sxs-lookup"><span data-stu-id="1b639-123">You can create hello following view as a handy way tooquery this DMV tooget information on rowgroup trimming.</span></span>

```sql
create view dbo.vCS_rg_physical_stats
as 
with cte
as
(
select   tb.[name]                    AS [logical_table_name]
,        rg.[row_group_id]            AS [row_group_id]
,        rg.[state]                   AS [state]
,        rg.[state_desc]              AS [state_desc]
,        rg.[total_rows]              AS [total_rows]
,        rg.[trim_reason_desc]        AS trim_reason_desc
,        mp.[physical_name]           AS physical_name
FROM    sys.[schemas] sm
JOIN    sys.[tables] tb               ON  sm.[schema_id]          = tb.[schema_id]                             
JOIN    sys.[pdw_table_mappings] mp   ON  tb.[object_id]          = mp.[object_id]
JOIN    sys.[pdw_nodes_tables] nt     ON  nt.[name]               = mp.[physical_name]
JOIN    sys.[dm_pdw_nodes_db_column_store_row_group_physical_stats] rg      ON  rg.[object_id]     = nt.[object_id]
                                                                            AND rg.[pdw_node_id]   = nt.[pdw_node_id]
                                        AND rg.[distribution_id]    = nt.[distribution_id]                                          
)
select *
from cte;
```

<span data-ttu-id="1b639-124">Hej trim_reason_desc talar om hello radgrupps har trimmade (trim_reason_desc = NO_TRIM innebär det fanns ingen beskrivande text och raden är optimal kvalitet).</span><span class="sxs-lookup"><span data-stu-id="1b639-124">hello trim_reason_desc tells whether hello rowgroup was trimmed(trim_reason_desc = NO_TRIM implies there was no trimming and row group is of optimal quality).</span></span> <span data-ttu-id="1b639-125">hello anger trim följande tidigt Trimning av hello radgrupps:</span><span class="sxs-lookup"><span data-stu-id="1b639-125">hello following trim reasons indicate premature trimming of hello rowgroup:</span></span>
- <span data-ttu-id="1b639-126">BULKLOAD: Trim därför används när hello inkommande batchen med rader för hello har mindre än 1 miljoner rader.</span><span class="sxs-lookup"><span data-stu-id="1b639-126">BULKLOAD: This trim reason is used when hello incoming batch of rows for hello load had less than 1 million rows.</span></span> <span data-ttu-id="1b639-127">hello motorn skapar komprimerade radgrupper om det är större än 100 000 rader som ska infogas (som skillnad från tooinserting till hello delta arkivet) men anger hello trim orsak tooBULKLOAD.</span><span class="sxs-lookup"><span data-stu-id="1b639-127">hello engine will create compressed row groups if there are greater than 100,000 rows being inserted (as opposed tooinserting into hello delta store) but sets hello trim reason tooBULKLOAD.</span></span> <span data-ttu-id="1b639-128">I det här scenariot kan du överväga att öka din batch belastningen fönstret tooaccumulate fler rader.</span><span class="sxs-lookup"><span data-stu-id="1b639-128">In this scenario, consider increasing your batch load window tooaccumulate more rows.</span></span> <span data-ttu-id="1b639-129">Dessutom omvärdera din partitionering schemat tooensure inte är det för detaljerade eftersom radgrupper inte får omfatta partitionsgränser.</span><span class="sxs-lookup"><span data-stu-id="1b639-129">Also, reevaluate your partitioning scheme tooensure it is not too granular as row groups cannot span partition boundaries.</span></span>
- <span data-ttu-id="1b639-130">MEMORY_LIMITATION: toocreate radgrupper med 1 miljon rader, en viss mängd arbetsminne krävs av hello-motorn.</span><span class="sxs-lookup"><span data-stu-id="1b639-130">MEMORY_LIMITATION: toocreate row groups with 1 million rows, a certain amount of working memory is required by hello engine.</span></span> <span data-ttu-id="1b639-131">När tillgängligt minne på hello inläsning sessionen är mindre än hello krävs fungerar minne, hämta tidigt radgrupper bort.</span><span class="sxs-lookup"><span data-stu-id="1b639-131">When available memory of hello loading session is less than hello required working memory, row groups get prematurely trimmed.</span></span> <span data-ttu-id="1b639-132">hello följande avsnitt förklarar hur tooestimate minne krävs och allokera mer minne.</span><span class="sxs-lookup"><span data-stu-id="1b639-132">hello following sections explain how tooestimate memory required and allocate more memory.</span></span>
- <span data-ttu-id="1b639-133">DICTIONARY_SIZE: Trim därför anger att radgrupps trimning inträffat eftersom det har minst en strängkolumn med bred och/eller hög kardinalitet strängar.</span><span class="sxs-lookup"><span data-stu-id="1b639-133">DICTIONARY_SIZE: This trim reason indicates that rowgroup trimming occurred because there was at least one string column with wide and/or high cardinality strings.</span></span> <span data-ttu-id="1b639-134">hello storlek är begränsad too16 MB i minnet och när den här gränsen har nåtts hello grupp komprimeras.</span><span class="sxs-lookup"><span data-stu-id="1b639-134">hello dictionary size is limited too16 MB in memory and once this limit is reached hello row group is compressed.</span></span> <span data-ttu-id="1b639-135">Överväg att isolera hello problematiska kolumnen till en separat tabell om du kör i den här situationen.</span><span class="sxs-lookup"><span data-stu-id="1b639-135">If you do run into this situation, consider isolating hello problematic column into a separate table.</span></span>

## <a name="how-tooestimate-memory-requirements"></a><span data-ttu-id="1b639-136">Hur tooestimate minneskrav</span><span class="sxs-lookup"><span data-stu-id="1b639-136">How tooestimate memory requirements</span></span>

<!--
tooview an estimate of hello memory requirements toocompress a rowgroup of maximum size into a columnstore index, download and run hello view [dbo.vCS_mon_mem_grant](). This view shows hello size of hello memory grant that a rowgroup requires for compression in toohello columnstore.
-->

<span data-ttu-id="1b639-137">hello maximalt minne toocompress en radgrupps är ungefär</span><span class="sxs-lookup"><span data-stu-id="1b639-137">hello maximum required memory toocompress one rowgroup is approximately</span></span>

- <span data-ttu-id="1b639-138">72 MB +</span><span class="sxs-lookup"><span data-stu-id="1b639-138">72 MB +</span></span>
- <span data-ttu-id="1b639-139">\#rader \* \#kolumner \* 8 byte +</span><span class="sxs-lookup"><span data-stu-id="1b639-139">\#rows \* \#columns \* 8 bytes +</span></span>
- <span data-ttu-id="1b639-140">\#rader \* \#kort-sträng-kolumner \* 32 byte +</span><span class="sxs-lookup"><span data-stu-id="1b639-140">\#rows \* \#short-string-columns \* 32 bytes +</span></span>
- <span data-ttu-id="1b639-141">\#lång sträng staplar \* 16 MB för komprimering</span><span class="sxs-lookup"><span data-stu-id="1b639-141">\#long-string-columns \* 16 MB for compression dictionary</span></span>

<span data-ttu-id="1b639-142">där kort-sträng-kolumner använder strängdatatyper av < = 32 byte och lång sträng staplar Använd strängdatatyper > 32 byte.</span><span class="sxs-lookup"><span data-stu-id="1b639-142">where short-string-columns use string data types of <= 32 bytes and long-string-columns use string data types of > 32 bytes.</span></span>

<span data-ttu-id="1b639-143">Lång sträng har komprimerats med en komprimeringsmetod som utformats för att komprimera text.</span><span class="sxs-lookup"><span data-stu-id="1b639-143">Long strings are compressed with a compression method designed for compressing text.</span></span> <span data-ttu-id="1b639-144">Den här komprimeringsmetoden använder en *ordlista* toostore textmönster.</span><span class="sxs-lookup"><span data-stu-id="1b639-144">This compression method uses a *dictionary* toostore text patterns.</span></span> <span data-ttu-id="1b639-145">hello maximal storlek för en ordlista är 16 MB.</span><span class="sxs-lookup"><span data-stu-id="1b639-145">hello maximum size of a dictionary is 16 MB.</span></span> <span data-ttu-id="1b639-146">Det finns endast en ordlista för varje kolumn för lång sträng i hello radgrupps.</span><span class="sxs-lookup"><span data-stu-id="1b639-146">There is only one dictionary for each long string column in hello rowgroup.</span></span>

<span data-ttu-id="1b639-147">En detaljerad beskrivning av columnstore minneskrav finns i videon [Azure SQL Data Warehouse skalning: konfiguration och vägledning](https://myignite.microsoft.com/videos/14822).</span><span class="sxs-lookup"><span data-stu-id="1b639-147">For an in-depth discussion of columnstore memory requirements, see the video [Azure SQL Data Warehouse scaling: configuration and guidance](https://myignite.microsoft.com/videos/14822).</span></span>

## <a name="ways-tooreduce-memory-requirements"></a><span data-ttu-id="1b639-148">Sätt tooreduce minneskrav</span><span class="sxs-lookup"><span data-stu-id="1b639-148">Ways tooreduce memory requirements</span></span>

<span data-ttu-id="1b639-149">Använd hello följande tekniker tooreduce hello minneskrav för att komprimera rowgroups columnstore-index.</span><span class="sxs-lookup"><span data-stu-id="1b639-149">Use hello following techniques tooreduce hello memory requirements for compressing rowgroups into columnstore indexes.</span></span>

### <a name="use-fewer-columns"></a><span data-ttu-id="1b639-150">Använd färre kolumner</span><span class="sxs-lookup"><span data-stu-id="1b639-150">Use fewer columns</span></span>
<span data-ttu-id="1b639-151">Om möjligt skapa hello med färre kolumner.</span><span class="sxs-lookup"><span data-stu-id="1b639-151">If possible, design hello table with fewer columns.</span></span> <span data-ttu-id="1b639-152">När en radgrupps komprimeras i hello columnstore komprimerar hello kolumnlagringsindexet varje kolumn segment separat.</span><span class="sxs-lookup"><span data-stu-id="1b639-152">When a rowgroup is compressed into hello columnstore, hello columnstore index compresses each column segment separately.</span></span> <span data-ttu-id="1b639-153">Därför hello minneskrav toocompress en radgrupps öka som hello antal kolumner ökar.</span><span class="sxs-lookup"><span data-stu-id="1b639-153">Therefore hello memory requirements toocompress a rowgroup increase as hello number of columns increases.</span></span>


### <a name="use-fewer-string-columns"></a><span data-ttu-id="1b639-154">Använd färre sträng kolumner</span><span class="sxs-lookup"><span data-stu-id="1b639-154">Use fewer string columns</span></span>
<span data-ttu-id="1b639-155">Kolumner i strängdatatyper kräver mer minne än numeriska datatyperna och date.</span><span class="sxs-lookup"><span data-stu-id="1b639-155">Columns of string data types require more memory than numeric and date data types.</span></span> <span data-ttu-id="1b639-156">ta bort sträng kolumner från faktatabeller och placera dem i mindre dimensionstabeller bör tooreduce minneskrav.</span><span class="sxs-lookup"><span data-stu-id="1b639-156">tooreduce memory requirements, consider removing string columns from fact tables and putting them in smaller dimension tables.</span></span>

<span data-ttu-id="1b639-157">Ytterligare minneskrav för sträng komprimering:</span><span class="sxs-lookup"><span data-stu-id="1b639-157">Additional memory requirements for string compression:</span></span>

- <span data-ttu-id="1b639-158">Strängdatatyper in too32 tecken kan kräva 32 ytterligare byte per värde.</span><span class="sxs-lookup"><span data-stu-id="1b639-158">String data types up too32 characters can require 32 additional bytes per value.</span></span>
- <span data-ttu-id="1b639-159">Strängdatatyper med mer än 32 tecken komprimeras med ordlistan metoder.</span><span class="sxs-lookup"><span data-stu-id="1b639-159">String data types with more than 32 characters are compressed using dictionary methods.</span></span>  <span data-ttu-id="1b639-160">Varje kolumn i hello radgrupps kan kräva upp tooan ytterligare 16 MB toobuild hello ordlistan.</span><span class="sxs-lookup"><span data-stu-id="1b639-160">Each column in hello rowgroup can require up tooan additional 16 MB toobuild hello dictionary.</span></span>

### <a name="avoid-over-partitioning"></a><span data-ttu-id="1b639-161">Undvika över partitionering</span><span class="sxs-lookup"><span data-stu-id="1b639-161">Avoid over-partitioning</span></span>

<span data-ttu-id="1b639-162">Columnstore-index skapa en eller flera rowgroups per partition.</span><span class="sxs-lookup"><span data-stu-id="1b639-162">Columnstore indexes create one or more rowgroups per partition.</span></span> <span data-ttu-id="1b639-163">I SQL Data Warehouse hello antalet partitioner växer snabbt eftersom hello data distribueras och varje distribution är partitionerad.</span><span class="sxs-lookup"><span data-stu-id="1b639-163">In SQL Data Warehouse, hello number of partitions grows quickly because hello data is distributed and each distribution is partitioned.</span></span> <span data-ttu-id="1b639-164">Om hello tabellen har för många partitioner, kanske det inte finns tillräckligt med rader toofill hello rowgroups.</span><span class="sxs-lookup"><span data-stu-id="1b639-164">If hello table has too many partitions, there might not be enough rows toofill hello rowgroups.</span></span> <span data-ttu-id="1b639-165">hello bristande rader skapar inte minnesbelastning under komprimeringen, men det leder toorowgroups som inte får hello bästa frågeprestanda för columnstore.</span><span class="sxs-lookup"><span data-stu-id="1b639-165">hello lack of rows does not create memory pressure during compression, but it leads toorowgroups that do not achieve hello best columnstore query performance.</span></span>

<span data-ttu-id="1b639-166">En annan orsak tooavoid över partitionering är finns det en minne omkostnader för att läsa in rader i ett columnstore-index för en partitionerad tabell.</span><span class="sxs-lookup"><span data-stu-id="1b639-166">Another reason tooavoid over-partitioning is there is a memory overhead for loading rows into a columnstore index on a partitioned table.</span></span> <span data-ttu-id="1b639-167">Under inläsningen av en kan många partitioner få hello inkommande rader som är kvar i minnet tills varje partition har tillräckligt med rader toobe komprimeras.</span><span class="sxs-lookup"><span data-stu-id="1b639-167">During a load, many partitions could receive hello incoming rows, which are held in memory until each partition has enough rows toobe compressed.</span></span> <span data-ttu-id="1b639-168">Med för många partitioner skapar ytterligare minnesbelastning.</span><span class="sxs-lookup"><span data-stu-id="1b639-168">Having too many partitions creates additional memory pressure.</span></span>

### <a name="simplify-hello-load-query"></a><span data-ttu-id="1b639-169">Förenkla hello hämta fråga</span><span class="sxs-lookup"><span data-stu-id="1b639-169">Simplify hello load query</span></span>

<span data-ttu-id="1b639-170">hello databasen delar hello minnestilldelningen för en fråga bland alla hello operatorer i hello frågan.</span><span class="sxs-lookup"><span data-stu-id="1b639-170">hello database shares hello memory grant for a query among all hello operators in hello query.</span></span> <span data-ttu-id="1b639-171">När en fråga belastningen har komplexa sorteringar och kopplingar, minskas hello minne för komprimering.</span><span class="sxs-lookup"><span data-stu-id="1b639-171">When a load query has complex sorts and joins, hello memory available for compression is reduced.</span></span>

<span data-ttu-id="1b639-172">Utforma hello belastningen frågan toofocus endast för inläsning av hello fråga.</span><span class="sxs-lookup"><span data-stu-id="1b639-172">Design hello load query toofocus only on loading hello query.</span></span> <span data-ttu-id="1b639-173">Om du behöver toorun omformningar på hello data kan köra dem separat från hello hämta fråga.</span><span class="sxs-lookup"><span data-stu-id="1b639-173">If you need toorun transformations on hello data, run them separate from hello load query.</span></span> <span data-ttu-id="1b639-174">Mellanlagra hello data i en heap-tabell, kör hello omvandlingar och läsa in hello mellanlagringstabell till hello columnstore-index.</span><span class="sxs-lookup"><span data-stu-id="1b639-174">For example, stage hello data in a heap table, run hello transformations, and then load hello staging table into hello columnstore index.</span></span> <span data-ttu-id="1b639-175">Du kan också ladda hello data först och sedan använda hello MPP system tootransform hello data.</span><span class="sxs-lookup"><span data-stu-id="1b639-175">You can also load hello data first and then use hello MPP system tootransform hello data.</span></span>

### <a name="adjust-maxdop"></a><span data-ttu-id="1b639-176">Justera MAXDOP</span><span class="sxs-lookup"><span data-stu-id="1b639-176">Adjust MAXDOP</span></span>

<span data-ttu-id="1b639-177">Varje distributionsplats komprimerar rowgroups till hello columnstore parallellt om det inte finns fler än en CPU-kärnor per distribution.</span><span class="sxs-lookup"><span data-stu-id="1b639-177">Each distribution compresses rowgroups into hello columnstore in parallel when there is more than one CPU core available per distribution.</span></span> <span data-ttu-id="1b639-178">hello parallellitet kräver ytterligare minne, vilket kan leda toomemory tryck och radgrupps trimning.</span><span class="sxs-lookup"><span data-stu-id="1b639-178">hello parallelism requires additional memory resources, which can lead toomemory pressure and rowgroup trimming.</span></span>

<span data-ttu-id="1b639-179">tooreduce minnesbelastning, du kan använda hello MAXDOP frågan tipset tooforce hello belastningen åtgärden toorun seriella läget i varje distribution.</span><span class="sxs-lookup"><span data-stu-id="1b639-179">tooreduce memory pressure, you can use hello MAXDOP query hint tooforce hello load operation toorun in serial mode within each distribution.</span></span>

```
CREATE TABLE MyFactSalesQuota
WITH (DISTRIBUTION = ROUND_ROBIN)
AS SELECT * FROM FactSalesQUota
OPTION (MAXDOP 1);
```

## <a name="ways-tooallocate-more-memory"></a><span data-ttu-id="1b639-180">Sätt tooallocate mer minne</span><span class="sxs-lookup"><span data-stu-id="1b639-180">Ways tooallocate more memory</span></span>

<span data-ttu-id="1b639-181">DWU storlek och hello användaren resursklassen tillsammans bestämmer hur mycket minne som är tillgängligt för en användarfråga.</span><span class="sxs-lookup"><span data-stu-id="1b639-181">DWU size and hello user resource class together determine how much memory is available for a user query.</span></span> <span data-ttu-id="1b639-182">tooincrease hello minne bevilja för en load-fråga kan du antingen öka hello antalet dwu: er eller öka hello resursklassen.</span><span class="sxs-lookup"><span data-stu-id="1b639-182">tooincrease hello memory grant for a load query, you can either increase hello number of DWUs or increase hello resource class.</span></span>

- <span data-ttu-id="1b639-183">tooincrease hello dwu: er, se [hur skala prestanda?](sql-data-warehouse-manage-compute-overview.md#scale-compute)</span><span class="sxs-lookup"><span data-stu-id="1b639-183">tooincrease hello DWUs, see [How do I scale performance?](sql-data-warehouse-manage-compute-overview.md#scale-compute)</span></span>
- <span data-ttu-id="1b639-184">toochange hello resursklassen för en fråga, se [ändra ett exempel på användaren resurs klassen](sql-data-warehouse-develop-concurrency.md#changing-user-resource-class-example).</span><span class="sxs-lookup"><span data-stu-id="1b639-184">toochange hello resource class for a query, see [Change a user resource class example](sql-data-warehouse-develop-concurrency.md#changing-user-resource-class-example).</span></span>

<span data-ttu-id="1b639-185">Till exempel på DWU 100 kan användare i hello smallrc resursklassen använda 100 MB minne för varje distribution.</span><span class="sxs-lookup"><span data-stu-id="1b639-185">For example, on DWU 100 a user in hello smallrc resource class can use 100 MB of memory for each distribution.</span></span> <span data-ttu-id="1b639-186">Hello information, se [samtidighet i SQL Data Warehouse](sql-data-warehouse-develop-concurrency.md).</span><span class="sxs-lookup"><span data-stu-id="1b639-186">For hello details, see [Concurrency in SQL Data Warehouse](sql-data-warehouse-develop-concurrency.md).</span></span>

<span data-ttu-id="1b639-187">Anta att du kontrollera att du behöver 700 MB minne tooget hög kvalitet radgrupps storlekar.</span><span class="sxs-lookup"><span data-stu-id="1b639-187">Suppose you determine that you need 700 MB of memory tooget high-quality rowgroup sizes.</span></span> <span data-ttu-id="1b639-188">De här exemplen visar hur du kan köra hello hämta fråga med tillräckligt med minne.</span><span class="sxs-lookup"><span data-stu-id="1b639-188">These examples show how you can run hello load query with enough memory.</span></span>

- <span data-ttu-id="1b639-189">Använder DWU 1000 och mediumrc, är din minnestilldelningen 800 MB</span><span class="sxs-lookup"><span data-stu-id="1b639-189">Using DWU 1000 and mediumrc, your memory grant is 800 MB</span></span>
- <span data-ttu-id="1b639-190">Använder DWU 600 och largerc, är din minnestilldelningen 800 MB.</span><span class="sxs-lookup"><span data-stu-id="1b639-190">Using DWU 600 and largerc, your memory grant is 800 MB.</span></span>


## <a name="next-steps"></a><span data-ttu-id="1b639-191">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="1b639-191">Next steps</span></span>

<span data-ttu-id="1b639-192">toofind mer sätt tooimprove prestanda i SQL Data Warehouse finns hello [översikt över prestanda](sql-data-warehouse-overview-manage-user-queries.md).</span><span class="sxs-lookup"><span data-stu-id="1b639-192">toofind more ways tooimprove performance in SQL Data Warehouse, see hello [Performance overview](sql-data-warehouse-overview-manage-user-queries.md).</span></span>

<!--Image references-->

<!--Article references-->


<!--MSDN references-->

<!--Other Web references-->
