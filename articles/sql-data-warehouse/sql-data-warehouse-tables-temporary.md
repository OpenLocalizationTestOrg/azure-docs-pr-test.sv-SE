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
# <a name="temporary-tables-in-sql-data-warehouse"></a><span data-ttu-id="1dbef-103">Temporära tabeller i SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="1dbef-103">Temporary tables in SQL Data Warehouse</span></span>
> [!div class="op_single_selector"]
> * <span data-ttu-id="1dbef-104">[Översikt över][Overview]</span><span class="sxs-lookup"><span data-stu-id="1dbef-104">[Overview][Overview]</span></span>
> * <span data-ttu-id="1dbef-105">[Datatyper][Data Types]</span><span class="sxs-lookup"><span data-stu-id="1dbef-105">[Data Types][Data Types]</span></span>
> * <span data-ttu-id="1dbef-106">[Distribuera][Distribute]</span><span class="sxs-lookup"><span data-stu-id="1dbef-106">[Distribute][Distribute]</span></span>
> * <span data-ttu-id="1dbef-107">[Index][Index]</span><span class="sxs-lookup"><span data-stu-id="1dbef-107">[Index][Index]</span></span>
> * <span data-ttu-id="1dbef-108">[Partition][Partition]</span><span class="sxs-lookup"><span data-stu-id="1dbef-108">[Partition][Partition]</span></span>
> * <span data-ttu-id="1dbef-109">[Statistik][Statistics]</span><span class="sxs-lookup"><span data-stu-id="1dbef-109">[Statistics][Statistics]</span></span>
> * <span data-ttu-id="1dbef-110">[Tillfällig][Temporary]</span><span class="sxs-lookup"><span data-stu-id="1dbef-110">[Temporary][Temporary]</span></span>
> 
> 

<span data-ttu-id="1dbef-111">Temporära tabeller är mycket användbara vid bearbetning av data - särskilt under omvandling där hello mellanresultat är tillfälligt.</span><span class="sxs-lookup"><span data-stu-id="1dbef-111">Temporary tables are very useful when processing data - especially during transformation where hello intermediate results are transient.</span></span> <span data-ttu-id="1dbef-112">Temporära tabeller finns på hello session nivå i SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="1dbef-112">In SQL Data Warehouse temporary tables exist at hello session level.</span></span>  <span data-ttu-id="1dbef-113">De är bara synliga toohello session har skapats och tas bort automatiskt när den aktuella sessionen loggar ut.</span><span class="sxs-lookup"><span data-stu-id="1dbef-113">They are only visible toohello session in which they were created and are automatically dropped when that session logs off.</span></span>  <span data-ttu-id="1dbef-114">Temporära tabeller erbjuder en prestandafördelar eftersom resultaten skrivs toolocal i stället för Fjärrlagring.</span><span class="sxs-lookup"><span data-stu-id="1dbef-114">Temporary tables offer a performance benefit because their results are written toolocal rather than remote storage.</span></span>  <span data-ttu-id="1dbef-115">Temporära tabeller är något annorlunda i Azure SQL Data Warehouse än Azure SQL Database som de kan nås från var som helst i hello-sessionen, inklusive både i och utanför en lagrad procedur.</span><span class="sxs-lookup"><span data-stu-id="1dbef-115">Temporary tables are slightly different in Azure SQL Data Warehouse than Azure SQL Database as they can be accessed from anywhere inside hello session, including both inside and outside of a stored procedure.</span></span>

<span data-ttu-id="1dbef-116">Den här artikeln innehåller grundläggande information om hur du använder temporära tabeller och visar hello principerna för sessionen nivån temporära tabeller.</span><span class="sxs-lookup"><span data-stu-id="1dbef-116">This article contains essential guidance for using temporary tables and highlights hello principles of session level temporary tables.</span></span> <span data-ttu-id="1dbef-117">Med hjälp av hello information i den här artikeln kan hjälpa dig modularize koden, förbättra både återanvändning och enkelhet för underhåll av din kod.</span><span class="sxs-lookup"><span data-stu-id="1dbef-117">Using hello information in this article can help you modularize your code, improving both reusability and ease of maintenance of your code.</span></span>

## <a name="create-a-temporary-table"></a><span data-ttu-id="1dbef-118">Skapa en tillfällig tabell</span><span class="sxs-lookup"><span data-stu-id="1dbef-118">Create a temporary table</span></span>
<span data-ttu-id="1dbef-119">Temporära tabeller skapas med bara din tabellnamn med en `#`.</span><span class="sxs-lookup"><span data-stu-id="1dbef-119">Temporary tables are created by simply prefixing your table name with a `#`.</span></span>  <span data-ttu-id="1dbef-120">Exempel:</span><span class="sxs-lookup"><span data-stu-id="1dbef-120">For example:</span></span>

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

<span data-ttu-id="1dbef-121">Temporära tabeller kan även skapas med en `CTAS` med exakt hello samma metod:</span><span class="sxs-lookup"><span data-stu-id="1dbef-121">Temporary tables can also be created with a `CTAS` using exactly hello same approach:</span></span>

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
> <span data-ttu-id="1dbef-122">`CTAS`är ett mycket kraftfullt kommando och har hello läggas till är mycket effektivt använda loggutrymme för transaktionen.</span><span class="sxs-lookup"><span data-stu-id="1dbef-122">`CTAS` is a very powerful command and has hello added advantage of being very efficient in its use of transaction log space.</span></span> 
> 
> 

## <a name="dropping-temporary-tables"></a><span data-ttu-id="1dbef-123">Släppa temporära tabeller</span><span class="sxs-lookup"><span data-stu-id="1dbef-123">Dropping temporary tables</span></span>
<span data-ttu-id="1dbef-124">När en ny session skapas ska inga temporära tabeller finnas.</span><span class="sxs-lookup"><span data-stu-id="1dbef-124">When a new session is created, no temporary tables should exist.</span></span>  <span data-ttu-id="1dbef-125">Men om du anropar hello samma lagrade proceduren skapas en tillfällig med hello samma namn, tooensure som din `CREATE TABLE` -satser är lyckad en enkel före kontroll med en `DROP` kan användas som hello exemplet nedan:</span><span class="sxs-lookup"><span data-stu-id="1dbef-125">However, if you are calling hello same stored procedure, which creates a temporary with hello same name, tooensure that your `CREATE TABLE` statements are successful a simple pre-existence check with a `DROP` can be used as in hello below example:</span></span>

```sql
IF OBJECT_ID('tempdb..#stats_ddl') IS NOT NULL
BEGIN
    DROP TABLE #stats_ddl
END
```

<span data-ttu-id="1dbef-126">För att koda konsekvens, är det en bra rutin toouse för det här mönstret för både tabeller och temporära tabeller.</span><span class="sxs-lookup"><span data-stu-id="1dbef-126">For coding consistency, it is a good practice toouse this pattern for both tables and temporary tables.</span></span>  <span data-ttu-id="1dbef-127">Det är också en bra idé toouse `DROP TABLE` tooremove temporära tabeller när du är klar med dem i din kod.</span><span class="sxs-lookup"><span data-stu-id="1dbef-127">It is also a good idea toouse `DROP TABLE` tooremove temporary tables when you have finished with them in your code.</span></span>  <span data-ttu-id="1dbef-128">I lagrade proceduren utveckling är det vanligt att toosee hello drop-kommandon buntas ihop hello slutet av en procedur tooensure dessa objekt rensas bort.</span><span class="sxs-lookup"><span data-stu-id="1dbef-128">In stored procedure development it is quite common toosee hello drop commands bundled together at hello end of a procedure tooensure these objects are cleaned up.</span></span>

```sql
DROP TABLE #stats_ddl
```

## <a name="modularizing-code"></a><span data-ttu-id="1dbef-129">Modularizing kod</span><span class="sxs-lookup"><span data-stu-id="1dbef-129">Modularizing code</span></span>
<span data-ttu-id="1dbef-130">Eftersom temporära tabeller kan visas någonstans i en användarsession, kan det vara utnyttjade toohelp modularize av programkoden.</span><span class="sxs-lookup"><span data-stu-id="1dbef-130">Since temporary tables can be seen anywhere in a user session, this can be exploited toohelp you modularize your application code.</span></span>  <span data-ttu-id="1dbef-131">Till exempel samlar hello lagrade proceduren nedan hello rekommenderade metoder för ovan toogenerate DDL som kommer att uppdatera all statistik i hello databasen efter Statistiknamn.</span><span class="sxs-lookup"><span data-stu-id="1dbef-131">For example, hello stored procedure below brings together hello recommended practices from above toogenerate DDL which will update all statistics in hello database by statistic name.</span></span>

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

<span data-ttu-id="1dbef-132">I det här skedet är hello enda åtgärden som har inträffat hello skapandet av en lagrad procedur som kommer att genereras bara en tillfällig tabell #stats_ddl med DDL-instruktioner.</span><span class="sxs-lookup"><span data-stu-id="1dbef-132">At this stage hello only action that has occurred is hello creation of a stored procedure which will simply generated a temporary table, #stats_ddl, with DDL statements.</span></span>  <span data-ttu-id="1dbef-133">Den här lagrade proceduren kommer att släppa #stats_ddl om det redan finns tooensure inte misslyckas om mer än en gång i en session.</span><span class="sxs-lookup"><span data-stu-id="1dbef-133">This stored procedure will drop #stats_ddl if it already exists tooensure it does not fail if run more than once within a session.</span></span>  <span data-ttu-id="1dbef-134">Men eftersom det inte finns några `DROP TABLE` hello slutet av hello lagrad procedur, när hello lagrade proceduren slutförs den lämnar hello skapat tabellen så att den kan läsas utanför hello lagrade proceduren.</span><span class="sxs-lookup"><span data-stu-id="1dbef-134">However, since there is no `DROP TABLE` at hello end of hello stored procedure, when hello stored procedure completes, it will leave hello created table so that it can be read outside of hello stored procedure.</span></span>  <span data-ttu-id="1dbef-135">I SQL Data Warehouse är till skillnad från andra SQL Server-databaser, det möjligt toouse hello tillfällig tabell utanför hello-procedur som den skapades.</span><span class="sxs-lookup"><span data-stu-id="1dbef-135">In SQL Data Warehouse, unlike other SQL Server databases, it is possible toouse hello temporary table outside of hello procedure that created it.</span></span>  <span data-ttu-id="1dbef-136">SQL Data Warehouse temporära tabeller kan användas för **var som helst** i hello-session.</span><span class="sxs-lookup"><span data-stu-id="1dbef-136">SQL Data Warehouse temporary tables can be used **anywhere** inside hello session.</span></span> <span data-ttu-id="1dbef-137">Detta kan leda toomore modulära och hanterbar kod som hello exemplet nedan:</span><span class="sxs-lookup"><span data-stu-id="1dbef-137">This can lead toomore modular and manageable code as in hello below example:</span></span>

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

## <a name="temporary-table-limitations"></a><span data-ttu-id="1dbef-138">Tillfällig tabell begränsningar</span><span class="sxs-lookup"><span data-stu-id="1dbef-138">Temporary table limitations</span></span>
<span data-ttu-id="1dbef-139">SQL Data Warehouse införa några begränsningar när du implementerar temporära tabeller.</span><span class="sxs-lookup"><span data-stu-id="1dbef-139">SQL Data Warehouse does impose a couple of limitations when implementing temporary tables.</span></span>  <span data-ttu-id="1dbef-140">För närvarande bara begränsade temporära tabeller stöds.</span><span class="sxs-lookup"><span data-stu-id="1dbef-140">Currently, only session scoped temporary tables are supported.</span></span>  <span data-ttu-id="1dbef-141">Globala temporära tabeller stöds inte.</span><span class="sxs-lookup"><span data-stu-id="1dbef-141">Global Temporary Tables are not supported.</span></span>  <span data-ttu-id="1dbef-142">Dessutom kan vyer inte skapas på temporära tabeller.</span><span class="sxs-lookup"><span data-stu-id="1dbef-142">In addition, views cannot be created on temporary tables.</span></span>

## <a name="next-steps"></a><span data-ttu-id="1dbef-143">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="1dbef-143">Next steps</span></span>
<span data-ttu-id="1dbef-144">toolearn finns fler hello artiklar på [tabell översikt][Overview], [Data tabelltyper][Data Types], [distribuerar en tabell] [ Distribute], [Indexering av en tabell][Index], [partitionering en tabell] [ Partition] och [ Underhåll av tabellstatistik][Statistics].</span><span class="sxs-lookup"><span data-stu-id="1dbef-144">toolearn more, see hello articles on [Table Overview][Overview], [Table Data Types][Data Types], [Distributing a Table][Distribute], [Indexing a Table][Index],  [Partitioning a Table][Partition] and [Maintaining Table Statistics][Statistics].</span></span>  <span data-ttu-id="1dbef-145">Mer information om metodtips finns [Metodtips för SQL Data Warehouse][SQL Data Warehouse Best Practices].</span><span class="sxs-lookup"><span data-stu-id="1dbef-145">For more about best practices, see [SQL Data Warehouse Best Practices][SQL Data Warehouse Best Practices].</span></span>

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
