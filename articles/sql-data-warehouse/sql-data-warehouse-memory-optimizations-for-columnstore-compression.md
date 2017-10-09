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
# <a name="maximizing-rowgroup-quality-for-columnstore"></a>Maximera radgrupps kvalitet för columnstore

Radgrupps kvalitet bestäms av hello antalet rader i en radgrupps. Sänk minneskrav eller öka hello tillgängligt minne toomaximize hello antalet rader som ett columnstore-index komprimeras i varje radgrupps.  Använd dessa metoder tooimprove komprimering priser och frågeprestanda för columnstore-index.

## <a name="why-hello-rowgroup-size-matters"></a>Hej radgrupps storlek
Eftersom ett columnstore-index skannar en tabell genom att skanna kolumnsegment med enskilda rowgroups, förbättrar maximera hello antalet rader i varje radgrupps prestanda för frågor. När rowgroups har ett stort antal rader, förbättrar datakomprimering vilket innebär att det finns mindre data tooread från disken.

Läs mer om rowgroups [Columnstore-index guiden](https://msdn.microsoft.com/library/gg492088.aspx).

## <a name="target-size-for-rowgroups"></a>Målstorlek för rowgroups
För bästa möjliga prestanda är hello målet toomaximize hello antalet rader per radgrupps i ett columnstore-index. En radgrupps kan ha högst 1 048 576 rader. Bra toonot har hello maximalt antal rader per radgrupps. Columnstore-index uppnå goda prestanda när rowgroups har minst 100 000 rader.

## <a name="rowgroups-can-get-trimmed-during-compression"></a>Rowgroups kan hämta trimmade under komprimering

Under en bulk load eller columnstore-index återskapning, ibland det finns inte tillräckligt med minne tillgängligt toocompress alla hello rader som anges för varje radgrupps. När minnesbelastning trim columnstore-index hello radgrupps storlekar så komprimering i hello columnstore kan genomföras. 

När det finns inte tillräckligt med minne toocompress minst 10 000 rader i varje radgrupps, genererar ett fel i SQL Data Warehouse.

Mer information om massinläsning finns [massinläsning till ett grupperat columnstore-index](https://msdn.microsoft.com/en-us/library/dn935008.aspx#Bulk load into a clustered columnstore index).

## <a name="how-toomonitor-rowgroup-quality"></a>Hur toomonitor radgrupps kvalitet

Det finns en DMV (sys.dm_pdw_nodes_db_column_store_row_group_physical_stats) som visar användbar information, till exempel antalet rader i rowgroups och hello orsak för trimning om det var trimning. Du kan skapa följande vy som ett praktiskt sätt tooquery hello DMV tooget informationen på radgrupps trimning.

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

Hej trim_reason_desc talar om hello radgrupps har trimmade (trim_reason_desc = NO_TRIM innebär det fanns ingen beskrivande text och raden är optimal kvalitet). hello anger trim följande tidigt Trimning av hello radgrupps:
- BULKLOAD: Trim därför används när hello inkommande batchen med rader för hello har mindre än 1 miljoner rader. hello motorn skapar komprimerade radgrupper om det är större än 100 000 rader som ska infogas (som skillnad från tooinserting till hello delta arkivet) men anger hello trim orsak tooBULKLOAD. I det här scenariot kan du överväga att öka din batch belastningen fönstret tooaccumulate fler rader. Dessutom omvärdera din partitionering schemat tooensure inte är det för detaljerade eftersom radgrupper inte får omfatta partitionsgränser.
- MEMORY_LIMITATION: toocreate radgrupper med 1 miljon rader, en viss mängd arbetsminne krävs av hello-motorn. När tillgängligt minne på hello inläsning sessionen är mindre än hello krävs fungerar minne, hämta tidigt radgrupper bort. hello följande avsnitt förklarar hur tooestimate minne krävs och allokera mer minne.
- DICTIONARY_SIZE: Trim därför anger att radgrupps trimning inträffat eftersom det har minst en strängkolumn med bred och/eller hög kardinalitet strängar. hello storlek är begränsad too16 MB i minnet och när den här gränsen har nåtts hello grupp komprimeras. Överväg att isolera hello problematiska kolumnen till en separat tabell om du kör i den här situationen.

## <a name="how-tooestimate-memory-requirements"></a>Hur tooestimate minneskrav

<!--
tooview an estimate of hello memory requirements toocompress a rowgroup of maximum size into a columnstore index, download and run hello view [dbo.vCS_mon_mem_grant](). This view shows hello size of hello memory grant that a rowgroup requires for compression in toohello columnstore.
-->

hello maximalt minne toocompress en radgrupps är ungefär

- 72 MB +
- \#rader \* \#kolumner \* 8 byte +
- \#rader \* \#kort-sträng-kolumner \* 32 byte +
- \#lång sträng staplar \* 16 MB för komprimering

där kort-sträng-kolumner använder strängdatatyper av < = 32 byte och lång sträng staplar Använd strängdatatyper > 32 byte.

Lång sträng har komprimerats med en komprimeringsmetod som utformats för att komprimera text. Den här komprimeringsmetoden använder en *ordlista* toostore textmönster. hello maximal storlek för en ordlista är 16 MB. Det finns endast en ordlista för varje kolumn för lång sträng i hello radgrupps.

En detaljerad beskrivning av columnstore minneskrav finns i videon [Azure SQL Data Warehouse skalning: konfiguration och vägledning](https://myignite.microsoft.com/videos/14822).

## <a name="ways-tooreduce-memory-requirements"></a>Sätt tooreduce minneskrav

Använd hello följande tekniker tooreduce hello minneskrav för att komprimera rowgroups columnstore-index.

### <a name="use-fewer-columns"></a>Använd färre kolumner
Om möjligt skapa hello med färre kolumner. När en radgrupps komprimeras i hello columnstore komprimerar hello kolumnlagringsindexet varje kolumn segment separat. Därför hello minneskrav toocompress en radgrupps öka som hello antal kolumner ökar.


### <a name="use-fewer-string-columns"></a>Använd färre sträng kolumner
Kolumner i strängdatatyper kräver mer minne än numeriska datatyperna och date. ta bort sträng kolumner från faktatabeller och placera dem i mindre dimensionstabeller bör tooreduce minneskrav.

Ytterligare minneskrav för sträng komprimering:

- Strängdatatyper in too32 tecken kan kräva 32 ytterligare byte per värde.
- Strängdatatyper med mer än 32 tecken komprimeras med ordlistan metoder.  Varje kolumn i hello radgrupps kan kräva upp tooan ytterligare 16 MB toobuild hello ordlistan.

### <a name="avoid-over-partitioning"></a>Undvika över partitionering

Columnstore-index skapa en eller flera rowgroups per partition. I SQL Data Warehouse hello antalet partitioner växer snabbt eftersom hello data distribueras och varje distribution är partitionerad. Om hello tabellen har för många partitioner, kanske det inte finns tillräckligt med rader toofill hello rowgroups. hello bristande rader skapar inte minnesbelastning under komprimeringen, men det leder toorowgroups som inte får hello bästa frågeprestanda för columnstore.

En annan orsak tooavoid över partitionering är finns det en minne omkostnader för att läsa in rader i ett columnstore-index för en partitionerad tabell. Under inläsningen av en kan många partitioner få hello inkommande rader som är kvar i minnet tills varje partition har tillräckligt med rader toobe komprimeras. Med för många partitioner skapar ytterligare minnesbelastning.

### <a name="simplify-hello-load-query"></a>Förenkla hello hämta fråga

hello databasen delar hello minnestilldelningen för en fråga bland alla hello operatorer i hello frågan. När en fråga belastningen har komplexa sorteringar och kopplingar, minskas hello minne för komprimering.

Utforma hello belastningen frågan toofocus endast för inläsning av hello fråga. Om du behöver toorun omformningar på hello data kan köra dem separat från hello hämta fråga. Mellanlagra hello data i en heap-tabell, kör hello omvandlingar och läsa in hello mellanlagringstabell till hello columnstore-index. Du kan också ladda hello data först och sedan använda hello MPP system tootransform hello data.

### <a name="adjust-maxdop"></a>Justera MAXDOP

Varje distributionsplats komprimerar rowgroups till hello columnstore parallellt om det inte finns fler än en CPU-kärnor per distribution. hello parallellitet kräver ytterligare minne, vilket kan leda toomemory tryck och radgrupps trimning.

tooreduce minnesbelastning, du kan använda hello MAXDOP frågan tipset tooforce hello belastningen åtgärden toorun seriella läget i varje distribution.

```
CREATE TABLE MyFactSalesQuota
WITH (DISTRIBUTION = ROUND_ROBIN)
AS SELECT * FROM FactSalesQUota
OPTION (MAXDOP 1);
```

## <a name="ways-tooallocate-more-memory"></a>Sätt tooallocate mer minne

DWU storlek och hello användaren resursklassen tillsammans bestämmer hur mycket minne som är tillgängligt för en användarfråga. tooincrease hello minne bevilja för en load-fråga kan du antingen öka hello antalet dwu: er eller öka hello resursklassen.

- tooincrease hello dwu: er, se [hur skala prestanda?](sql-data-warehouse-manage-compute-overview.md#scale-compute)
- toochange hello resursklassen för en fråga, se [ändra ett exempel på användaren resurs klassen](sql-data-warehouse-develop-concurrency.md#changing-user-resource-class-example).

Till exempel på DWU 100 kan användare i hello smallrc resursklassen använda 100 MB minne för varje distribution. Hello information, se [samtidighet i SQL Data Warehouse](sql-data-warehouse-develop-concurrency.md).

Anta att du kontrollera att du behöver 700 MB minne tooget hög kvalitet radgrupps storlekar. De här exemplen visar hur du kan köra hello hämta fråga med tillräckligt med minne.

- Använder DWU 1000 och mediumrc, är din minnestilldelningen 800 MB
- Använder DWU 600 och largerc, är din minnestilldelningen 800 MB.


## <a name="next-steps"></a>Nästa steg

toofind mer sätt tooimprove prestanda i SQL Data Warehouse finns hello [översikt över prestanda](sql-data-warehouse-overview-manage-user-queries.md).

<!--Image references-->

<!--Article references-->


<!--MSDN references-->

<!--Other Web references-->
