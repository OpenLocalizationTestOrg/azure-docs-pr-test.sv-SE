---
title: aaaIndexing tabeller i SQL Data Warehouse | Microsoft Azure
description: "Komma igång med tabellen indexering i Azure SQL Data Warehouse."
services: sql-data-warehouse
documentationcenter: NA
author: shivaniguptamsft
manager: barbkess
editor: 
ms.assetid: 3e617674-7b62-43ab-9ca2-3f40c41d5a88
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: tables
ms.date: 07/12/2016
ms.author: shigu;barbkess
ms.openlocfilehash: e614d63c8fb871f2ba388f14576cf9f282d4b818
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="indexing-tables-in-sql-data-warehouse"></a>Indexering tabeller i SQL Data Warehouse
> [!div class="op_single_selector"]
> * [Översikt över][Overview]
> * [Datatyper][Data Types]
> * [Distribuera][Distribute]
> * [Index][Index]
> * [Partition][Partition]
> * [Statistik][Statistics]
> * [Tillfällig][Temporary]
> 
> 

SQL Data Warehouse erbjuder flera indexeringsalternativ inklusive [grupperat kolumnlagringsindex][clustered columnstore indexes], [grupperat index och icke-grupperade index] [ clustered indexes and nonclustered indexes].  Dessutom det ger också en inga indexalternativet kallas även [heap][heap].  Den här artikeln beskriver hello fördelarna med varje Indextypen samt tips toogetting hello de flesta prestanda från ditt index. Se [skapa table-syntax] [ create table syntax] för mer information om hur toocreate en tabell i SQL Data Warehouse.

## <a name="clustered-columnstore-indexes"></a>Grupperade columnstore-index
Som standard skapar SQL Data Warehouse grupperade columnstore-indexet när inga Indexalternativ för har angetts för en tabell. Grupperade columnstore-tabeller erbjuder båda hello högsta nivån av komprimering som hello bästa övergripande prestanda för frågor.  Grupperade columnstore-tabeller kommer vanligtvis överträffar grupperade index eller heap-tabeller och är vanligtvis hello bästa valet för stora tabeller.  Därför är grupperade columnstore hello bästa plats toostart när du är osäker på hur tooindex tabellen.  

toocreate ett grupperat columnstore-tabellen kan du bara ange GRUPPERAT COLUMNSTORE-INDEX i hello WITH-satsen, eller lämna hello WITH-satsen:

```SQL
CREATE TABLE myTable   
  (  
    id int NOT NULL,  
    lastName varchar(20),  
    zipCode varchar(6)  
  )  
WITH ( CLUSTERED COLUMNSTORE INDEX );
```

Det finns några scenarier där det grupperade columnstore inte får vara ett bra alternativ:

* Columnstore-tabeller stöder inte varchar(max), nvarchar(max) och varbinary(max).  Överväg att heap eller grupperat index i stället.
* Columnstore-tabeller kan vara mindre effektiv för tillfälliga data.  Överväg heap och eventuellt även temporära tabeller.
* Små tabeller med mindre än 100 miljoner rader.  Överväg att heap-tabeller.

## <a name="heap-tables"></a>Heap-tabeller
När du tillfälligt hamnar data i SQL Data Warehouse, kan du använda en heap-tabell kommer att hello övergripande processen snabbare.  Detta beror på att belastningar tooheaps är snabbare än tooindex tabeller och i vissa fall hello efterföljande Läs kan göras från cachen.  Om du läser in klustrad data endast toostage innan du kör flera transformationer som läser in hello tabellen tooheap blir mycket snabbare än inläsning hello data tooa columnstore-tabellen. Dessutom kan läsa in data tooa [tillfällig tabell] [ Temporary] läses också mycket snabbare än en tabell toopermanent lagringen lästes in.  

Ofta heap tabeller vara klokt med små sökning tabeller, mindre än 100 miljoner rader.  Klustret columnstore-tabeller börjar tooachieve optimala komprimering när det finns fler än 100 miljoner rader.

toocreate en heap-tabell, ange bara HEAP i hello WITH-satsen:

```SQL
CREATE TABLE myTable   
  (  
    id int NOT NULL,  
    lastName varchar(20),  
    zipCode varchar(6)  
  )  
WITH ( HEAP );
```

## <a name="clustered-and-nonclustered-indexes"></a>Klustrade och icke-grupperat index
Grupperade index kan överträffar grupperade columnstore-tabeller när en rad måste toobe snabbt hämtas.  Frågor om en enda eller mycket få raden sökning är obligatoriska tooperformance med extrema hastighet, Överväg i ett klusterindex eller sekundärt icke-grupperat index.  hello nackdelen toousing ett grupperat index är att endast de frågor som använder ett starkt selektiv filter på hello grupperat indexkolumn drar nytta.  tooimprove filtret på andra kolumner som en icke-grupperat index kan läggas till tooother kolumner.  Varje index som läggs till tooa tabell läggs utrymme och bearbetning av tid tooloads.

toocreate ett grupperat index i tabellen anger bara GRUPPERAT INDEX i hello WITH-satsen:

```SQL
CREATE TABLE myTable   
  (  
    id int NOT NULL,  
    lastName varchar(20),  
    zipCode varchar(6)  
  )  
WITH ( CLUSTERED INDEX (id) );
```

tooadd ett icke-grupperade index för en tabell, Använd bara hello följande syntax:

```SQL
CREATE INDEX zipCodeIndex ON t1 (zipCode);
```

## <a name="optimizing-clustered-columnstore-indexes"></a>Optimera grupperade columnstore-index
Grupperade columnstore-tabeller är uppdelade i data i segment.  Har hög segment kvalitet är kritiska tooachieving optimala frågeprestanda för ett columnstore-tabellen.  Segment kvalitet kan mätas hello antal rader i en komprimerad radgrupp.  Segment kvalitet är mest optimala där det finns minst 100K rader per komprimerade rad gruppen och få prestanda som hello antalet rader per rad grupp metod 1 048 576 rader, vilket är hello de flesta rader som en grupp kan innehålla.

hello nedan vyn kan skapas och används på ditt system toocompute hello genomsnittlig rader per rad gruppen och identifiera eventuella optimalt klustret columnstore-index.  hello sista kolumnen i den här vyn genererar som SQL-uttryck som kan vara används toorebuild ditt index.

```sql
CREATE VIEW dbo.vColumnstoreDensity
AS
SELECT
        GETDATE()                                                               AS [execution_date]
,       DB_Name()                                                               AS [database_name]
,       s.name                                                                  AS [schema_name]
,       t.name                                                                  AS [table_name]
,    COUNT(DISTINCT rg.[partition_number])                    AS [table_partition_count]
,       SUM(rg.[total_rows])                                                    AS [row_count_total]
,       SUM(rg.[total_rows])/COUNT(DISTINCT rg.[distribution_id])               AS [row_count_per_distribution_MAX]
,    CEILING    ((SUM(rg.[total_rows])*1.0/COUNT(DISTINCT rg.[distribution_id]))/1048576) AS [rowgroup_per_distribution_MAX]
,       SUM(CASE WHEN rg.[State] = 0 THEN 1                   ELSE 0    END)    AS [INVISIBLE_rowgroup_count]
,       SUM(CASE WHEN rg.[State] = 0 THEN rg.[total_rows]     ELSE 0    END)    AS [INVISIBLE_rowgroup_rows]
,       MIN(CASE WHEN rg.[State] = 0 THEN rg.[total_rows]     ELSE NULL END)    AS [INVISIBLE_rowgroup_rows_MIN]
,       MAX(CASE WHEN rg.[State] = 0 THEN rg.[total_rows]     ELSE NULL END)    AS [INVISIBLE_rowgroup_rows_MAX]
,       AVG(CASE WHEN rg.[State] = 0 THEN rg.[total_rows]     ELSE NULL END)    AS [INVISIBLE_rowgroup_rows_AVG]
,       SUM(CASE WHEN rg.[State] = 1 THEN 1                   ELSE 0    END)    AS [OPEN_rowgroup_count]
,       SUM(CASE WHEN rg.[State] = 1 THEN rg.[total_rows]     ELSE 0    END)    AS [OPEN_rowgroup_rows]
,       MIN(CASE WHEN rg.[State] = 1 THEN rg.[total_rows]     ELSE NULL END)    AS [OPEN_rowgroup_rows_MIN]
,       MAX(CASE WHEN rg.[State] = 1 THEN rg.[total_rows]     ELSE NULL END)    AS [OPEN_rowgroup_rows_MAX]
,       AVG(CASE WHEN rg.[State] = 1 THEN rg.[total_rows]     ELSE NULL END)    AS [OPEN_rowgroup_rows_AVG]
,       SUM(CASE WHEN rg.[State] = 2 THEN 1                   ELSE 0    END)    AS [CLOSED_rowgroup_count]
,       SUM(CASE WHEN rg.[State] = 2 THEN rg.[total_rows]     ELSE 0    END)    AS [CLOSED_rowgroup_rows]
,       MIN(CASE WHEN rg.[State] = 2 THEN rg.[total_rows]     ELSE NULL END)    AS [CLOSED_rowgroup_rows_MIN]
,       MAX(CASE WHEN rg.[State] = 2 THEN rg.[total_rows]     ELSE NULL END)    AS [CLOSED_rowgroup_rows_MAX]
,       AVG(CASE WHEN rg.[State] = 2 THEN rg.[total_rows]     ELSE NULL END)    AS [CLOSED_rowgroup_rows_AVG]
,       SUM(CASE WHEN rg.[State] = 3 THEN 1                   ELSE 0    END)    AS [COMPRESSED_rowgroup_count]
,       SUM(CASE WHEN rg.[State] = 3 THEN rg.[total_rows]     ELSE 0    END)    AS [COMPRESSED_rowgroup_rows]
,       SUM(CASE WHEN rg.[State] = 3 THEN rg.[deleted_rows]   ELSE 0    END)    AS [COMPRESSED_rowgroup_rows_DELETED]
,       MIN(CASE WHEN rg.[State] = 3 THEN rg.[total_rows]     ELSE NULL END)    AS [COMPRESSED_rowgroup_rows_MIN]
,       MAX(CASE WHEN rg.[State] = 3 THEN rg.[total_rows]     ELSE NULL END)    AS [COMPRESSED_rowgroup_rows_MAX]
,       AVG(CASE WHEN rg.[State] = 3 THEN rg.[total_rows]     ELSE NULL END)    AS [COMPRESSED_rowgroup_rows_AVG]
,       'ALTER INDEX ALL ON ' + s.name + '.' + t.NAME + ' REBUILD;'             AS [Rebuild_Index_SQL]
FROM    sys.[pdw_nodes_column_store_row_groups] rg
JOIN    sys.[pdw_nodes_tables] nt                   ON  rg.[object_id]          = nt.[object_id]
                                                    AND rg.[pdw_node_id]        = nt.[pdw_node_id]
                                                    AND rg.[distribution_id]    = nt.[distribution_id]
JOIN    sys.[pdw_table_mappings] mp                 ON  nt.[name]               = mp.[physical_name]
JOIN    sys.[tables] t                              ON  mp.[object_id]          = t.[object_id]
JOIN    sys.[schemas] s                             ON t.[schema_id]            = s.[schema_id]
GROUP BY
        s.[name]
,       t.[name]
;
```

Nu när du har skapat hello vy, kör du den här frågan tooidentify tabeller med radgrupper med färre än 100K rader.  Naturligtvis kanske tooincrease hello tröskelvärdet för 100K om du behöver mer optimala segment kvalitet. 

```sql
SELECT    *
FROM    [dbo].[vColumnstoreDensity]
WHERE    COMPRESSED_rowgroup_rows_AVG < 100000
        OR INVISIBLE_rowgroup_rows_AVG < 100000
```

När du har kört frågan hello du börja toolook hello data och analysera dina resultat. Den här tabellen beskrivs vilka toolook för i din grupp analys för raden.

| Kolumn | Hur toouse data |
| --- | --- |
| [table_partition_count] |Om hello tabellen är partitionerad kan du förvänta dig toosee högre öppna grupp radantal. Varje partition i hello distribution kan ha en öppen grupp som är kopplade till den i teorin. Ta hänsyn till det i dina analyser. En mindre tabell som har har partitionerats kan optimeras genom att ta bort hello partitionering helt och hållet eftersom detta skulle förbättra komprimering. |
| [row_count_total] |Totalt radantal för hello tabellen. Exempelvis kan du använda det här värdet toocalculate procentandelen rader i hello komprimerade tillstånd. |
| [row_count_per_distribution_MAX] |Om alla rader fördelas jämnt skulle värdet vara hello mål antalet rader per distribution. Jämför värdet med hello compressed_rowgroup_count. |
| [COMPRESSED_rowgroup_rows] |Totalt antal rader i columnstore-format för hello tabellen. |
| [COMPRESSED_rowgroup_rows_AVG] |Om hello Genomsnittligt antal rader är betydligt mindre än hello maximalt antal rader för en grupp, bör du använda CTAS eller ALTER INDEX REBUILD toorecompress hello data |
| [COMPRESSED_rowgroup_count] |Antal radgrupper i columnstore-format. Om värdet är mycket hög i relationen toohello tabell är en indikator att hello columnstore densitet är låg. |
| [COMPRESSED_rowgroup_rows_DELETED] |Logiskt ta bort rader i columnstore-format. Om antalet hello hög relativa tootable storleken kan du återskapa hello partition eller återskapa hello indexet eftersom det tar bort dem fysiskt. |
| [COMPRESSED_rowgroup_rows_MIN] |Använd detta tillsammans med hello AVG och MAX kolumner toounderstand hello värdeintervallet för hello radgrupper i din columnstore. Ett lågt värde över hello belastningen tröskelvärdet (102,400 per partition justerad distribution) föreslår att optimeringar är tillgängliga i hello datainläsning |
| [COMPRESSED_rowgroup_rows_MAX] |Som ovan |
| [OPEN_rowgroup_count] |Öppna radgrupper är normalt. En förväntar rimligen en öppen grupp per tabell distribution (60). Alltför stora tal föreslår datainläsning över partitioner. Kontrollera hello partitionering strategi toomake att det är bra |
| [OPEN_rowgroup_rows] |Varje grupp kan innehålla 1 048 576 rader som maximalt. Använd det här värdet toosee hur full hello öppna radgrupper är för närvarande |
| [OPEN_rowgroup_rows_MIN] |Öppna grupper tyda på att data är antingen som takt läses in i hello tabell eller som hello tidigare last ut återstående rader i den här raden-gruppen. Använd hello MIN, MAX, AVG kolumner toosee hur mycket data är satt i öppna radgrupper. Det kan vara 100% av alla hello-data för små tabeller! ALTER INDEX REBUILD tooforce Hej då data toocolumnstore. |
| [OPEN_rowgroup_rows_MAX] |Som ovan |
| [OPEN_rowgroup_rows_AVG] |Som ovan |
| [CLOSED_rowgroup_rows] |Titta på hello stängd rad gruppera rader som förstånd kontroll. |
| [CLOSED_rowgroup_count] |hello antalet stängd radgrupper ska vara låg om någon ses alls. Stängd radgrupper kan vara konverterade toocompressed rowg roups med hello ALTER INDEX... REKONSTRUERA kommando. Detta är dock inte normalt krävs. Stängd grupper är automatiskt konverterade toocolumnstore radgrupper av hello ”tuppel till tuppelflyttar” bakgrunden. |
| [CLOSED_rowgroup_rows_MIN] |Stängd radgrupper ska ha en mycket hög fill hastighet. Om hello fill hastigheten för en stängd grupp är låg, krävs ytterligare analys av hello columnstore. |
| [CLOSED_rowgroup_rows_MAX] |Som ovan |
| [CLOSED_rowgroup_rows_AVG] |Som ovan |
| [Rebuild_Index_SQL] |SQL toorebuild columnstore-index för en tabell |

## <a name="causes-of-poor-columnstore-index-quality"></a>Orsaker till dåliga columnstore-index kvalitet
Om du har identifierat tabeller med dålig segment kvalitet, vill du tooidentify hello orsaken.  Nedan visas några andra vanliga orsaker till dåliga segment quaility:

1. Minnesbelastning när indexet har skapats
2. Stor volym med DML-operationer
3. Liten eller sipprar belastningen åtgärder
4. För många partitioner

Dessa faktorer kan orsaka ett columnstore-index toohave betydligt mindre än hello optimala 1 miljon rader per grupp.  De kan också medföra rader toogo toohello delta raden gruppen i stället för en komprimerad grupp. 

### <a name="memory-pressure-when-index-was-built"></a>Minnesbelastning när indexet har skapats
hello antalet rader per komprimerade grupp är direkt relaterade toohello bredden på hello raden och hello mängden minne tillgängligt tooprocess hello grupp.  När rader skrivs toocolumnstore tabeller minnesbelastning, drabbas columnstore segment kvalitet.  Därför är bästa praxis för hello toogive hello session, som skriver tooyour columnstore-index tabeller komma åt tooas mycket minne som möjligt.  Eftersom det är en kompromiss mellan minne och samtidighet, kan hello vägledning om hello rätt minne allokering beror på hello data i varje rad i tabellen, hello många DWU du har tilldelat tooyour system och hello mängden samtidighet fack du ge toohello sessionen som skriver data tooyour tabell.  Som bästa praxis rekommenderar vi börjar med xlargerc om du använder DW300 eller mindre largerc om du använder DW400 tooDW600 och mediumrc om du använder DW1000 och senare.

### <a name="high-volume-of-dml-operations"></a>Stor volym med DML-operationer
En stor volym med DML-operationer som uppdatera och ta bort rader kan installera funktionen i hello columnstore. Detta gäller särskilt när hello merparten av hello rader i en grupp ändras.

* Ta bort en rad från en komprimerad grupp bara logiskt markerar hello raden som tagits bort. hello raden finns kvar i hello komprimerade grupp tills hello partition eller tabellen har återskapats.
* Infoga en rad lägger till hello raden tootooan interna rowstore tabell som kallas en rad delta-grupp. hello Infoga rad är inte konverterade toocolumnstore förrän hello delta grupp är full och har markerats som stängda. Radgrupper stängs när de når hello maximal kapacitet för 1 048 576 rader. 
* Uppdatera en rad i columnstore format behandlas som en logisk delete och insert. hello Infoga rad kan lagras i hello delta store.

Batchar update och infoga åtgärder som överskrider hello bulk tröskelvärdet 102 400 rader per partition justerade distribution ska skrivas direkt toohello columnstore-format. Men behöver om en jämn fördelning, du toobe ändra överstiger 6.144 miljoner rader i en enda åtgärd för den här toooccur. Om hello antalet rader i en given partition justerade distribution är mindre än 102,400 hello rader skickas toohello delta store och förblir det tills tillräckligt raderna har infogats eller ändrade tooclose hello grupp eller hello radindexet har återskapats.

### <a name="small-or-trickle-load-operations"></a>Liten eller sipprar belastningen åtgärder
Liten läser in flödet i SQL Data Warehouse kallas även ibland som sipprar belastning. De representerar vanligtvis en nära konstant ström av data som inhämtas av hello system. Kontinuerlig hello volymen rader är dock inte särskilt stor som närmar sig den här dataströmmen. Hello data är oftast avsevärt under hello tröskelvärdet som krävs för ett direkt belastningen toocolumnstore format.

I sådana fall är ofta bättre tooland hello data först i Azure blob storage och låt den ackumuleras tidigare tooloading. Den här metoden kallas ofta *micro batchbearbetning*.

### <a name="too-many-partitions"></a>För många partitioner
En annan sak tooconsider påverkas hello partitionering på grupperade columnstore-tabeller.  Innan du partitionering delar SQL Data Warehouse redan dina data i 60 databaser.  Partitionering ytterligare delar upp dina data.  Om du partitionera data kommer du förmodligen tooconsider som **varje** partition behöver toohave minst 1 miljoner rader toobenefit från ett grupperat columnstore-index.  Om du partitionera tabellen till 100 partitioner kommer din tabell måste toohave minst 6 miljarder rader toobenefit från ett grupperat columnstore-index (60 distributioner * 100 partitioner * 1 miljoner rader). Om 100 partitionstabellen saknar 6 miljarder rader, minska hello antalet partitioner eller Överväg att använda en heap-tabellen i stället.

När dina tabeller har lästs in med vissa data, följer du hello under steg tooidentify och återskapa tabeller med kolumnlagringsindex optimalt klustret.

## <a name="rebuilding-indexes-tooimprove-segment-quality"></a>Återskapa index tooimprove segment kvalitet
### <a name="step-1-identify-or-create-user-which-uses-hello-right-resource-class"></a>Steg 1: Identifiera eller skapa användare som använder hello rätt resursklassen
Ett snabbt sätt tooimmediately förbättra kvaliteten i segmentet är toorebuild hello index.  hello SQL som returneras av hello ovan visa returneras en ALTER INDEX REBUILD-instruktion som kan vara används toorebuild ditt index.  När ditt index, måste du allokera tillräckligt med minne toohello session som kommer att återskapa index.  toodo detta, öka hello resursklassen för en användare som har behörigheter toorebuild hello index på den här tabellen toohello rekommenderade minimum.  hello resursklassen för hello databasanvändare ägare kan inte ändras, så om du inte har skapat en användare på hello system måste toodo så först.  hello minsta rekommenderar vi är xlargerc om du använder DW300 eller mindre largerc om du använder DW400 tooDW600 och mediumrc om du använder DW1000 och senare.

Nedan visas ett exempel på hur tooallocate mer minne tooa användare genom att öka sin resursklassen.  Mer information om resursklasser och hur toocreate en ny användare finns i hello [samtidighet och arbetsbelastningen] [ Concurrency] artikel.

```sql
EXEC sp_addrolemember 'xlargerc', 'LoadUser'
```

### <a name="step-2-rebuild-clustered-columnstore-indexes-with-higher-resource-class-user"></a>Steg 2: Återskapa grupperade columnstore-index med högre resurs klassen användare
Logga in som hello användaren från steg 1 (t.ex. LoadUser), vilket är nu med en högre resursklassen och kör hello ALTER INDEX-satser.  Var noga med att den här användaren har ALTER behörighet toohello tabeller där hello index återskapas.  De här exemplen visar hur toorebuild hello hela columnstore-index eller hur toorebuild en partition. För stora tabeller är det mer praktiskt toorebuild index som en enda partition i taget.

Du kan också i stället för att bygga hello index, du kan kopiera hello tabell tooa nytt tabell med [CTAS][CTAS].  Hur är bäst? För stora mängder data, [CTAS] [ CTAS] är normalt snabbare än [ALTER INDEX][ALTER INDEX]. För mindre datamängder [ALTER INDEX] [ ALTER INDEX] är enklare toouse och behöver inte någon tooswap ut hello tabell.  Se **bygga om index med CTAS och växla partition** nedan för mer information om hur toorebuild index med CTAS.

```sql
-- Rebuild hello entire clustered index
ALTER INDEX ALL ON [dbo].[DimProduct] REBUILD
```

```sql
-- Rebuild a single partition
ALTER INDEX ALL ON [dbo].[FactInternetSales] REBUILD Partition = 5
```

```sql
-- Rebuild a single partition with archival compression
ALTER INDEX ALL ON [dbo].[FactInternetSales] REBUILD Partition = 5 WITH (DATA_COMPRESSION = COLUMNSTORE_ARCHIVE)
```

```sql
-- Rebuild a single partition with columnstore compression
ALTER INDEX ALL ON [dbo].[FactInternetSales] REBUILD Partition = 5 WITH (DATA_COMPRESSION = COLUMNSTORE)
```

När ett index i SQL Data Warehouse är en åtgärd som är offline.  Mer information om att bygga om index finns hello ALTER INDEX REBUILD i avsnittet [Columnstore-index defragmentering] [ Columnstore Indexes Defragmentation] hello syntax ämnet och [ALTER INDEX] [ALTER INDEX].

### <a name="step-3-verify-clustered-columnstore-segment-quality-has-improved"></a>Steg 3: Kontrollera grupperade columnstore segment kvalitet har förbättrats
Kör hello frågan som identifierats register med dålig segmentera kvalitet och kontrollera segment kvalitet har förbättrats.  Om segment kvalitet inte förbättrar, kan det vara att hello rader i tabellen är extra långa.  Överväg att använda en högre resursklassen eller DWU när ditt index.

## <a name="rebuilding-indexes-with-ctas-and-partition-switching"></a>Bygga om index med CTAS och växla partition
Det här exemplet används [CTAS] [ CTAS] och partitionsväxling toorebuild en partition table. 

```sql
-- Step 1: Select hello partition of data and write it out tooa new table using CTAS
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
FROM    [dbo].[FactInternetSales]
WHERE   [OrderDateKey] >= 20000101
AND     [OrderDateKey] <  20010101
;

-- Step 2: Create a SWITCH out table
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
FROM    [dbo].[FactInternetSales]
WHERE   1=2 -- Note this table will be empty

-- Step 3: Switch OUT hello data 
ALTER TABLE [dbo].[FactInternetSales] SWITCH PARTITION 2 too [dbo].[FactInternetSales_20000101] PARTITION 2;

-- Step 4: Switch IN hello rebuilt data
ALTER TABLE [dbo].[FactInternetSales_20000101_20010101] SWITCH PARTITION 2 too [dbo].[FactInternetSales] PARTITION 2;
```

Mer information om att återskapa partitioner som använder `CTAS`, se hello [Partition] [ Partition] artikel.

## <a name="next-steps"></a>Nästa steg
toolearn finns fler hello artiklar på [tabell översikt][Overview], [Data tabelltyper][Data Types], [distribuerar en tabell] [ Distribute], [Partitionering en tabell][Partition], [underhålla tabellstatistik] [ Statistics] och [ Temporära tabeller][Temporary].  toolearn mer om bästa praxis, se [Metodtips för SQL Data Warehouse][SQL Data Warehouse Best Practices].

<!--Image references-->

<!--Article references-->
[Overview]: ./sql-data-warehouse-tables-overview.md
[Data Types]: ./sql-data-warehouse-tables-data-types.md
[Distribute]: ./sql-data-warehouse-tables-distribute.md
[Index]: ./sql-data-warehouse-tables-index.md
[Partition]: ./sql-data-warehouse-tables-partition.md
[Statistics]: ./sql-data-warehouse-tables-statistics.md
[Temporary]: ./sql-data-warehouse-tables-temporary.md
[Concurrency]: ./sql-data-warehouse-develop-concurrency.md
[CTAS]: ./sql-data-warehouse-develop-ctas.md
[SQL Data Warehouse Best Practices]: ./sql-data-warehouse-best-practices.md

<!--MSDN references-->
[ALTER INDEX]: https://msdn.microsoft.com/library/ms188388.aspx
[heap]: https://msdn.microsoft.com/library/hh213609.aspx
[clustered indexes and nonclustered indexes]: https://msdn.microsoft.com/library/ms190457.aspx
[create table syntax]: https://msdn.microsoft.com/library/mt203953.aspx
[Columnstore Indexes Defragmentation]: https://msdn.microsoft.com/library/dn935013.aspx#Anchor_1
[clustered columnstore indexes]: https://msdn.microsoft.com/library/gg492088.aspx

<!--Other Web references-->
