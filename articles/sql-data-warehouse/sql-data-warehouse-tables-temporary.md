---
title: aaaTemporary tabeller i SQL Data Warehouse | Microsoft Docs
description: "Komma igång med temporära tabeller i Azure SQL Data Warehouse."
services: sql-data-warehouse
documentationcenter: NA
author: shivaniguptamsft
manager: jhubbard
editor: 
ms.assetid: 9b1119eb-7f54-46d0-ad74-19c85a2a555a
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: tables
ms.date: 10/31/2016
ms.author: shigu;barbkess
ms.openlocfilehash: 2e8b122eb6d71d5bc0a99ce8a2ecab5dbe2d1b49
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="temporary-tables-in-sql-data-warehouse"></a>Temporära tabeller i SQL Data Warehouse
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

Temporära tabeller är mycket användbara vid bearbetning av data - särskilt under omvandling där hello mellanresultat är tillfälligt. Temporära tabeller finns på hello session nivå i SQL Data Warehouse.  De är bara synliga toohello session har skapats och tas bort automatiskt när den aktuella sessionen loggar ut.  Temporära tabeller erbjuder en prestandafördelar eftersom resultaten skrivs toolocal i stället för Fjärrlagring.  Temporära tabeller är något annorlunda i Azure SQL Data Warehouse än Azure SQL Database som de kan nås från var som helst i hello-sessionen, inklusive både i och utanför en lagrad procedur.

Den här artikeln innehåller grundläggande information om hur du använder temporära tabeller och visar hello principerna för sessionen nivån temporära tabeller. Med hjälp av hello information i den här artikeln kan hjälpa dig modularize koden, förbättra både återanvändning och enkelhet för underhåll av din kod.

## <a name="create-a-temporary-table"></a>Skapa en tillfällig tabell
Temporära tabeller skapas med bara din tabellnamn med en `#`.  Exempel:

```sql
CREATE TABLE #stats_ddl
(
    [schema_name]        NVARCHAR(128) NOT NULL
,    [table_name]            NVARCHAR(128) NOT NULL
,    [stats_name]            NVARCHAR(128) NOT NULL
,    [stats_is_filtered]     BIT           NOT NULL
,    [seq_nmbr]              BIGINT        NOT NULL
,    [two_part_name]         NVARCHAR(260) NOT NULL
,    [three_part_name]       NVARCHAR(400) NOT NULL
)
WITH
(
    DISTRIBUTION = HASH([seq_nmbr])
,    HEAP
)
```

Temporära tabeller kan även skapas med en `CTAS` med exakt hello samma metod:

```sql
CREATE TABLE #stats_ddl
WITH
(
    DISTRIBUTION = HASH([seq_nmbr])
,    HEAP
)
AS
(
SELECT
        sm.[name]                                                                AS [schema_name]
,        tb.[name]                                                                AS [table_name]
,        st.[name]                                                                AS [stats_name]
,        st.[has_filter]                                                            AS [stats_is_filtered]
,       ROW_NUMBER()
        OVER(ORDER BY (SELECT NULL))                                            AS [seq_nmbr]
,                                 QUOTENAME(sm.[name])+'.'+QUOTENAME(tb.[name])  AS [two_part_name]
,        QUOTENAME(DB_NAME())+'.'+QUOTENAME(sm.[name])+'.'+QUOTENAME(tb.[name])  AS [three_part_name]
FROM    sys.objects            AS ob
JOIN    sys.stats            AS st    ON    ob.[object_id]        = st.[object_id]
JOIN    sys.stats_columns    AS sc    ON    st.[stats_id]        = sc.[stats_id]
                                    AND st.[object_id]        = sc.[object_id]
JOIN    sys.columns            AS co    ON    sc.[column_id]        = co.[column_id]
                                    AND    sc.[object_id]        = co.[object_id]
JOIN    sys.tables            AS tb    ON    co.[object_id]        = tb.[object_id]
JOIN    sys.schemas            AS sm    ON    tb.[schema_id]        = sm.[schema_id]
WHERE    1=1
AND        st.[user_created]   = 1
GROUP BY
        sm.[name]
,        tb.[name]
,        st.[name]
,        st.[filter_definition]
,        st.[has_filter]
)
SELECT
    CASE @update_type
    WHEN 1
    THEN 'UPDATE STATISTICS '+[two_part_name]+'('+[stats_name]+');'
    WHEN 2
    THEN 'UPDATE STATISTICS '+[two_part_name]+'('+[stats_name]+') WITH FULLSCAN;'
    WHEN 3
    THEN 'UPDATE STATISTICS '+[two_part_name]+'('+[stats_name]+') WITH SAMPLE '+CAST(@sample_pct AS VARCHAR(20))+' PERCENT;'
    WHEN 4
    THEN 'UPDATE STATISTICS '+[two_part_name]+'('+[stats_name]+') WITH RESAMPLE;'
    END AS [update_stats_ddl]
,   [seq_nmbr]
FROM    t1
;
``` 

> [!NOTE]
> `CTAS`är ett mycket kraftfullt kommando och har hello läggas till är mycket effektivt använda loggutrymme för transaktionen. 
> 
> 

## <a name="dropping-temporary-tables"></a>Släppa temporära tabeller
När en ny session skapas ska inga temporära tabeller finnas.  Men om du anropar hello samma lagrade proceduren skapas en tillfällig med hello samma namn, tooensure som din `CREATE TABLE` -satser är lyckad en enkel före kontroll med en `DROP` kan användas som hello exemplet nedan:

```sql
IF OBJECT_ID('tempdb..#stats_ddl') IS NOT NULL
BEGIN
    DROP TABLE #stats_ddl
END
```

För att koda konsekvens, är det en bra rutin toouse för det här mönstret för både tabeller och temporära tabeller.  Det är också en bra idé toouse `DROP TABLE` tooremove temporära tabeller när du är klar med dem i din kod.  I lagrade proceduren utveckling är det vanligt att toosee hello drop-kommandon buntas ihop hello slutet av en procedur tooensure dessa objekt rensas bort.

```sql
DROP TABLE #stats_ddl
```

## <a name="modularizing-code"></a>Modularizing kod
Eftersom temporära tabeller kan visas någonstans i en användarsession, kan det vara utnyttjade toohelp modularize av programkoden.  Till exempel samlar hello lagrade proceduren nedan hello rekommenderade metoder för ovan toogenerate DDL som kommer att uppdatera all statistik i hello databasen efter Statistiknamn.

```sql
CREATE PROCEDURE    [dbo].[prc_sqldw_update_stats]
(   @update_type    tinyint -- 1 default 2 fullscan 3 sample 4 resample
    ,@sample_pct     tinyint
)
AS

IF @update_type NOT IN (1,2,3,4)
BEGIN;
    THROW 151000,'Invalid value for @update_type parameter. Valid range 1 (default), 2 (fullscan), 3 (sample) or 4 (resample).',1;
END;

IF @sample_pct IS NULL
BEGIN;
    SET @sample_pct = 20;
END;

IF OBJECT_ID('tempdb..#stats_ddl') IS NOT NULL
BEGIN
    DROP TABLE #stats_ddl
END

CREATE TABLE #stats_ddl
WITH
(
    DISTRIBUTION = HASH([seq_nmbr])
)
AS
(
SELECT
        sm.[name]                                                                AS [schema_name]
,        tb.[name]                                                                AS [table_name]
,        st.[name]                                                                AS [stats_name]
,        st.[has_filter]                                                            AS [stats_is_filtered]
,       ROW_NUMBER()
        OVER(ORDER BY (SELECT NULL))                                            AS [seq_nmbr]
,                                 QUOTENAME(sm.[name])+'.'+QUOTENAME(tb.[name])  AS [two_part_name]
,        QUOTENAME(DB_NAME())+'.'+QUOTENAME(sm.[name])+'.'+QUOTENAME(tb.[name])  AS [three_part_name]
FROM    sys.objects            AS ob
JOIN    sys.stats            AS st    ON    ob.[object_id]        = st.[object_id]
JOIN    sys.stats_columns    AS sc    ON    st.[stats_id]        = sc.[stats_id]
                                    AND st.[object_id]        = sc.[object_id]
JOIN    sys.columns            AS co    ON    sc.[column_id]        = co.[column_id]
                                    AND    sc.[object_id]        = co.[object_id]
JOIN    sys.tables            AS tb    ON    co.[object_id]        = tb.[object_id]
JOIN    sys.schemas            AS sm    ON    tb.[schema_id]        = sm.[schema_id]
WHERE    1=1
AND        st.[user_created]   = 1
GROUP BY
        sm.[name]
,        tb.[name]
,        st.[name]
,        st.[filter_definition]
,        st.[has_filter]
)
SELECT
    CASE @update_type
    WHEN 1
    THEN 'UPDATE STATISTICS '+[two_part_name]+'('+[stats_name]+');'
    WHEN 2
    THEN 'UPDATE STATISTICS '+[two_part_name]+'('+[stats_name]+') WITH FULLSCAN;'
    WHEN 3
    THEN 'UPDATE STATISTICS '+[two_part_name]+'('+[stats_name]+') WITH SAMPLE '+CAST(@sample_pct AS VARCHAR(20))+' PERCENT;'
    WHEN 4
    THEN 'UPDATE STATISTICS '+[two_part_name]+'('+[stats_name]+') WITH RESAMPLE;'
    END AS [update_stats_ddl]
,   [seq_nmbr]
FROM    t1
;
GO
```

I det här skedet är hello enda åtgärden som har inträffat hello skapandet av en lagrad procedur som kommer att genereras bara en tillfällig tabell #stats_ddl med DDL-instruktioner.  Den här lagrade proceduren kommer att släppa #stats_ddl om det redan finns tooensure inte misslyckas om mer än en gång i en session.  Men eftersom det inte finns några `DROP TABLE` hello slutet av hello lagrad procedur, när hello lagrade proceduren slutförs den lämnar hello skapat tabellen så att den kan läsas utanför hello lagrade proceduren.  I SQL Data Warehouse är till skillnad från andra SQL Server-databaser, det möjligt toouse hello tillfällig tabell utanför hello-procedur som den skapades.  SQL Data Warehouse temporära tabeller kan användas för **var som helst** i hello-session. Detta kan leda toomore modulära och hanterbar kod som hello exemplet nedan:

```sql
EXEC [dbo].[prc_sqldw_update_stats] @update_type = 1, @sample_pct = NULL;

DECLARE @i INT              = 1
,       @t INT              = (SELECT COUNT(*) FROM #stats_ddl)
,       @s NVARCHAR(4000)   = N''

WHILE @i <= @t
BEGIN
    SET @s=(SELECT update_stats_ddl FROM #stats_ddl WHERE seq_nmbr = @i);

    PRINT @s
    EXEC sp_executesql @s
    SET @i+=1;
END

DROP TABLE #stats_ddl;
```

## <a name="temporary-table-limitations"></a>Tillfällig tabell begränsningar
SQL Data Warehouse införa några begränsningar när du implementerar temporära tabeller.  För närvarande bara begränsade temporära tabeller stöds.  Globala temporära tabeller stöds inte.  Dessutom kan vyer inte skapas på temporära tabeller.

## <a name="next-steps"></a>Nästa steg
toolearn finns fler hello artiklar på [tabell översikt][Overview], [Data tabelltyper][Data Types], [distribuerar en tabell] [ Distribute], [Indexering av en tabell][Index], [partitionering en tabell] [ Partition] och [ Underhåll av tabellstatistik][Statistics].  Mer information om metodtips finns [Metodtips för SQL Data Warehouse][SQL Data Warehouse Best Practices].

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

<!--MSDN references-->

<!--Other Web references-->
