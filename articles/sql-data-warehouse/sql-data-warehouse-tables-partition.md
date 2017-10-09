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
# <a name="partitioning-tables-in-sql-data-warehouse"></a>Partitionering tabeller i SQL Data Warehouse
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

Partitionering stöds på alla SQL Data Warehouse tabelltyper; inklusive grupperade columnstore, grupperat index och heap.  Partitionering stöds även på alla typer av distribution, inklusive hash- eller resursallokering distribueras.  Partitionering kan du toodivide dina data i mindre grupper av data och i de flesta fall partitionering görs på ett datum.

## <a name="benefits-of-partitioning"></a>Fördelarna med partitionering
Partitionering kan dra prestandadata för underhåll och fråga.  Om den fördelar både eller bara en beror på hur data har lästs in och om hello samma kolumn kan användas för båda, eftersom partitionering kan bara utföras på en kolumn.

### <a name="benefits-tooloads"></a>Fördelar tooloads
hello största fördelen partitionering i SQL Data Warehouse är förbättra hello effektivitet och prestanda för inläsning av data med hjälp av partition borttagning, växlar och sammanslagning.  I de flesta fall data är partitionerad på ett datum knutna kolumn som är nära toohello sekvens som hello data är inlästa toohello databas.  En av hello största fördelarna med att använda partitioner toomaintain data den hello undvikande av transaktionsloggning.  Helt enkelt lägga till, uppdatera eller ta bort data kan vara hello den enklaste metoden med lite tankar och prestanda, kan använder partitionering under din inläsningen avsevärt förbättra prestanda.

Partition växlar kan använda tooquickly ta bort eller ersätta ett avsnitt i en tabell.  Till exempel kan försäljning faktatabellen innehålla bara data för hello senaste 36 månaderna.  Hello slutet av varje månad, tas hello äldsta månads försäljning data bort från hello tabell.  Dessa data kan tas bort med hjälp av en delete-instruktion toodelete hello data för hello äldsta månad.  Men kan om du tar bort en stor mängd data rad för rad med en delete-instruktion ta mycket lång tid, samt skapa hello risken för stora transaktioner, vilket kan ta en lång tid toorollback om något går fel.  En mer optimala metod är toosimply släpp hello äldsta partition av data.  När du tar bort hello enskilda rader kan ta timmar, ta ta bort en hel partition sekunder.

### <a name="benefits-tooqueries"></a>Fördelar tooqueries
Partitionering kan också vara används tooimprove frågeprestanda.  Om en fråga gäller ett filter för en partitionerad kolumn, kan detta begränsa hello genomsökning tooonly hello kvalificerande partitioner som kan vara en mycket mindre deluppsättning av hello data, att undvika en fullständig tabellgenomsökning.  Hello predikat eliminering prestandafördelarna är mindre användbara med hello införandet av grupperade columnstore-index, men i vissa fall det kan vara en fördel tooqueries.  Till exempel om hello försäljning faktatabell är partitionerad till 36 månader med hello försäljning datumfält sedan frågor som filtrerar på hello Försäljningsdatum kan hoppa över sökning i partitioner som inte matchar hello filter.

## <a name="partition-sizing-guidance"></a>Vägledning för partition storlek
När partitionering kan vara används tooimprove prestanda vissa scenarier, skapar en tabell med **för många** partitioner kan försämra prestanda under vissa omständigheter.  Dessa problem är särskilt för grupperade columnstore-tabeller.  Partitionering toobe användbara, är det viktigt toounderstand när toouse partitionering och hello antalet partitioner toocreate.  Det är ingen hårda snabb regel som toohow många partitioner är för många, det beror på dina data och hur många partitioner du läser in toosimultaneously.  Men som en allmän tumregel tror att för att lägga till 10-tal too100s av partitioner, inte 1 000-tal.

När du skapar partitionering på **grupperade columnstore** tabeller, är det viktigt tooconsider hur många rader som hamnar på varje partition.  För optimala komprimering och prestanda för grupperade columnstore-tabeller krävs minst 1 miljon rader per distribution och partition.  Innan partitioner skapas, delas SQL Data Warehouse redan varje tabell i 60 distribuerade databaser.  Alla partitionering tillagda tooa tabellen är dessutom toohello distributioner som skapats hello bakgrunden.  Med det här exemplet, om hello försäljning faktatabell innehåller 36 månatliga partitioner och med hänsyn till att SQL Data Warehouse har 60 distributioner sedan hello försäljning fakta tabellen ska innehålla 60 miljoner rader per månad eller 2.1 miljarder rader när alla månader fylls i.  Om en tabell innehåller avsevärt färre rader än hello Rekommenderat minsta antal rader per partition, Överväg att använda färre partitioner i ordning toomake öka hello antalet rader per partition.  Se även hello [indexering] [ Index] artikel som innehåller de förfrågningar som kan köras på SQL Data Warehouse tooassess hello kvaliteten på klustret columnstore-index.

## <a name="syntax-difference-from-sql-server"></a>Syntaxen skillnaden från SQL Server
SQL Data Warehouse introducerar en förenklad definition av partitioner som skiljer sig från SQL Server.  Partitionering funktioner och scheman används inte i SQL Data Warehouse som i SQL Server.  I stället är allt du behöver toodo identifierar partitionerade kolumn och hello gräns punkter.  Även om hello syntaxen för partitionering skiljer sig från SQLServer kan är hello grundläggande koncept hello samma.  SQL Server och SQL Data Warehouse stöder en partition kolumn per tabell, som kan vara låg.  toolearn mer information om partitionering, se [partitionerade tabeller och index][Partitioned Tables and Indexes].

hello nedan exempel på ett SQL Data Warehouse partitionerad [CREATE TABLE] [ CREATE TABLE] -instruktionen partitionerar hello FactInternetSales tabellen efter hello OrderDateKey kolumnen:

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

## <a name="migrating-partitioning-from-sql-server"></a>Migrera partitionering från SQL Server
toomigrate SQL Server partitions bara definitioner tooSQL Data Warehouse:

* Eliminera hello SQL Server [partitionsschema][partition scheme].
* Lägg till hello [partitionsfunktioner] [ partition function] definition tooyour Skapa tabell.

Om du migrerar en partitionerad tabell från en SQL Server-instansen hello nedan SQL kan hjälpa dig toointerrogate hello antalet rader som finns i varje partition.  Tänk på att om hello samma partitionering detaljnivå används i SQL Data Warehouse, minska hello antalet rader per partition med en faktor på 60.  

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

## <a name="workload-management"></a>Arbetsbelastningshantering
En sista övervägande toofactor i toohello tabell partition beslut är [hantering av arbetsbelastning][workload management].  Hantering av arbetsbelastning i SQL Data Warehouse är främst hello hantering av minne och samtidighet.  I SQL Data Warehouse hello är maximalt minne som allokerats tooeach distribution vid körning av fråga styrda resursklasser.  Helst ska partitionerna storleksändras med hänsyn till andra faktorer som hello minne som behövs för att skapa grupperade columnstore-index.  Grupperade columnstore-index förmånen avsevärt när de tilldelas mer minne.  Därför vill du tooensure återskapa ett Partitionsindex inte är för lite minne. Öka hello mängden minne tillgängligt tooyour fråga kan uppnås genom att byta från hello standardroll, smallrc, tooone av hello andra roller, till exempel largerc.

Information om hello fördelningen av minne per distribution är tillgänglig genom att fråga hello resurs resursstyrningen dynamiska hanteringsvyer. I verkligheten är din minnestilldelningen mindre än hello figurerna nedan. Detta ger dock en nivå som du kan använda vid bedömning av partitionerna för hanteringsåtgärder för data.  Försök tooavoid storleksanpassa partitionerna utöver hello minnestilldelningen som tillhandahålls av hello extra stor resursklassen. Om din partitioner växer utöver denna bild kör hello risken för minnesbelastning vilket i sin tur leder tooless optimala komprimering.

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

## <a name="partition-switching"></a>Växla partition
SQL Data Warehouse stöder partition dela sammanslagning och växlar. Dessa funktioner är excuted med hello [ALTER TABLE] [ ALTER TABLE] instruktionen.

tooswitch partitioner mellan två tabeller måste du kontrollera att justera hello partitioner på deras respektive gränser och att det matchar hello definitioner. Eftersom de inte tillgängliga Kontrollbegränsningar tooenforce hello värdeintervallet i en tabell hello källtabellen måste innehålla hello samma partitions gränser som hello måltabellen. Om detta inte är fallet hello misslyckas hello partitionsväxeln som hello partition metadata inte synkroniseras.

### <a name="how-toosplit-a-partition-that-contains-data"></a>Hur toosplit en partition som innehåller data
hello effektivaste metoden toosplit en partition som redan innehåller data är toouse en `CTAS` instruktion. Om hello partitionerad tabell är ett grupperat columnstore sedan hello tabell partitionen måste vara tom innan du kan dela.

Nedan visas partitionerade columnstore exempeltabell som innehåller en rad i varje partition:

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
> Genom att skapa hello statistik objektet Kontrollera att tabellmetadata är mer exakt. Om vi utelämnar statistik skapas sedan använder SQL Data Warehouse standardvärden. För information om statistik hittar [statistik][statistics].
> 
> 

Vi kan sedan fråga efter hello radantal med hello `sys.partitions` vyn katalog:

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

Om vi försöker toosplit den här tabellen, kommer vi får ett felmeddelande:

```sql
ALTER TABLE FactInternetSales SPLIT RANGE (20010101);
```

Ignorerad 35346 nivå 15, tillstånd 1, rad 44 SPLIT-satsen i ALTER PARTITION-instruktionen misslyckades eftersom hello partitionen inte är tom.  Endast tomma partitioner kan delas i när ett columnstore-index finns på hello tabell. Överväg att inaktivera hello columnstore-indexet innan ALTER PARTITION-instruktionen hello och återskapa hello columnstore-indexet när ALTER PARTITION har slutförts.

Vi kan dock använda `CTAS` toocreate en ny tabell toohold våra data.

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

En växel tillåts som hello partitionsgränser justeras. Detta lämnar hello källtabellen med en tom partition som senare kan vi dela.

```sql
ALTER TABLE FactInternetSales SWITCH PARTITION 2 too FactInternetSales_20000101 PARTITION 2;

ALTER TABLE FactInternetSales SPLIT RANGE (20010101);
```

Återstår toodo är tooalign våra data toohello partitionera nya gränser med `CTAS` och växla våra data i toohello huvudtabell

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

När du har slutfört hello flödet av hello uppgifter är det en bra idé toorefresh hello statistik på hello mål tabell tooensure de motsvarar hello ny distribution av hello data i deras respektive partitioner:

```sql
UPDATE STATISTICS [dbo].[FactInternetSales];
```

### <a name="table-partitioning-source-control"></a>Tabell partitionering källkontroll
tooavoid din tabelldefinitionen från **rostangripet** i ditt källkontrollsystem kanske du vill tooconsider hello följande metod:

1. Skapa hello tabell som en partitionerad tabell men saknar värden för partition

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

1. `SPLIT`hello tabell som en del av processen för distribution av hello:

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

Med den här metoden hello koden i källkontroll är statisk och hello partitionering gränsvärden tillåts toobe dynamiska; under utveckling med hello datalager över tid.

## <a name="next-steps"></a>Nästa steg
toolearn finns fler hello artiklar på [tabell översikt][Overview], [Data tabelltyper][Data Types], [distribuerar en tabell] [ Distribute], [Indexering av en tabell][Index], [underhålla tabellstatistik] [ Statistics] och [ Temporära tabeller][Temporary].  Mer information om metodtips finns [Metodtips för SQL Data Warehouse][SQL Data Warehouse Best Practices].

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
