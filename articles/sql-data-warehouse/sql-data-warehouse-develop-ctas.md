---
title: "aaaCreate tabellen som Välj (CTAS) i SQL Data Warehouse | Microsoft Docs"
description: "Tips för att koda med hello Skapa tabell som select-uttryck (CTAS) i Azure SQL Data Warehouse för utveckling av lösningar."
services: sql-data-warehouse
documentationcenter: NA
author: shivaniguptamsft
manager: jhubbard
editor: 
ms.assetid: 68ac9a94-09f9-424b-b536-06a125a653bd
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: queries
ms.date: 01/30/2017
ms.author: shigu;barbkess
ms.openlocfilehash: e381601a0a4d94e189d8f9115bf2e7593025410b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-table-as-select-ctas-in-sql-data-warehouse"></a><span data-ttu-id="29da9-103">Skapa en tabell som Select (CTAS) i SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="29da9-103">Create Table As Select (CTAS) in SQL Data Warehouse</span></span>
<span data-ttu-id="29da9-104">Skapa tabell som markerar eller `CTAS` är en av hello viktigaste T-SQL-funktioner tillgängliga.</span><span class="sxs-lookup"><span data-stu-id="29da9-104">Create table as select or `CTAS` is one of hello most important T-SQL features available.</span></span> <span data-ttu-id="29da9-105">Det är en helt parallelized åtgärd som skapar en ny tabell baserat på hello utdata från en SELECT-instruktion.</span><span class="sxs-lookup"><span data-stu-id="29da9-105">It is a fully parallelized operation that creates a new table based on hello output of a SELECT statement.</span></span> <span data-ttu-id="29da9-106">`CTAS`är hello enklaste och snabbaste sättet toocreate en kopia av en tabell.</span><span class="sxs-lookup"><span data-stu-id="29da9-106">`CTAS` is hello simplest and fastest way toocreate a copy of a table.</span></span> <span data-ttu-id="29da9-107">Det här dokumentet innehåller både exempel och bästa praxis för `CTAS`.</span><span class="sxs-lookup"><span data-stu-id="29da9-107">This document provides both examples and best practices for `CTAS`.</span></span>

## <a name="selectinto-vs-ctas"></a><span data-ttu-id="29da9-108">VÄLJ... I vs. CTAS</span><span class="sxs-lookup"><span data-stu-id="29da9-108">SELECT..INTO vs. CTAS</span></span>
<span data-ttu-id="29da9-109">Du kan överväga att `CTAS` som en mycket debiteras version av `SELECT..INTO`.</span><span class="sxs-lookup"><span data-stu-id="29da9-109">You can consider `CTAS` as a super-charged version of `SELECT..INTO`.</span></span>

<span data-ttu-id="29da9-110">Nedan visas ett exempel på en enkel `SELECT..INTO` instruktionen:</span><span class="sxs-lookup"><span data-stu-id="29da9-110">Below is an example of a simple `SELECT..INTO` statement:</span></span>

```sql
SELECT *
INTO    [dbo].[FactInternetSales_new]
FROM    [dbo].[FactInternetSales]
```

<span data-ttu-id="29da9-111">I ovanstående exempel hello `[dbo].[FactInternetSales_new]` skulle skapas som ROUND_ROBIN distribuerade tabell med ett GRUPPERAT COLUMNSTORE-INDEX på den som dessa hello tabell standardvärden i Azure SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="29da9-111">In hello example above `[dbo].[FactInternetSales_new]` would be created as ROUND_ROBIN distributed table with a CLUSTERED COLUMNSTORE INDEX on it as these are hello table defaults in Azure SQL Data Warehouse.</span></span>

<span data-ttu-id="29da9-112">`SELECT..INTO`dock kan du inte toochange skriver du antingen hello distribution metoden eller hello index som en del av hello igen.</span><span class="sxs-lookup"><span data-stu-id="29da9-112">`SELECT..INTO` however does not allow you toochange either hello distribution method or hello index type as part of hello operation.</span></span> <span data-ttu-id="29da9-113">Det är där `CTAS` kommer in.</span><span class="sxs-lookup"><span data-stu-id="29da9-113">This is where `CTAS` comes in.</span></span>

<span data-ttu-id="29da9-114">tooconvert hello ovan för`CTAS` är ganska enkel:</span><span class="sxs-lookup"><span data-stu-id="29da9-114">tooconvert hello above too`CTAS` is quite straight-forward:</span></span>

```sql
CREATE TABLE [dbo].[FactInternetSales_new]
WITH
(
    DISTRIBUTION = ROUND_ROBIN
,   CLUSTERED COLUMNSTORE INDEX
)
AS
SELECT  *
FROM    [dbo].[FactInternetSales]
;
```

<span data-ttu-id="29da9-115">Med `CTAS` du är kan toochange både hello distribution av hello tabelldata samt hello tabelltyp.</span><span class="sxs-lookup"><span data-stu-id="29da9-115">With `CTAS` you are able toochange both hello distribution of hello table data as well as hello table type.</span></span> 

> [!NOTE]
> <span data-ttu-id="29da9-116">Om du försöker bara toochange hello index i din `CTAS` åtgärden och hello källtabellen är hash distribueras sedan din `CTAS` åtgärden utförs bäst om du underhåller hello samma kolumn- och distributionstypen.</span><span class="sxs-lookup"><span data-stu-id="29da9-116">If you are only trying toochange hello index in your `CTAS` operation and hello source table is hash distributed then your `CTAS` operation will perform best if you maintain hello same distribution column and data type.</span></span> <span data-ttu-id="29da9-117">På så sätt undviker mellan distribution dataflyttning under hello åtgärden, vilket är mer effektivt.</span><span class="sxs-lookup"><span data-stu-id="29da9-117">This will avoid cross distribution data movement during hello operation which is more efficient.</span></span>
> 
> 

## <a name="using-ctas-toocopy-a-table"></a><span data-ttu-id="29da9-118">Med hjälp av CTAS toocopy en tabell</span><span class="sxs-lookup"><span data-stu-id="29da9-118">Using CTAS toocopy a table</span></span>
<span data-ttu-id="29da9-119">Kanske en av de vanligaste hello används av `CTAS` skapar en kopia av en tabell, så att du kan ändra hello DDL.</span><span class="sxs-lookup"><span data-stu-id="29da9-119">Perhaps one of hello most common uses of `CTAS` is creating a copy of a table so that you can change hello DDL.</span></span> <span data-ttu-id="29da9-120">Om till exempel din tabell som ursprungligen skapades `ROUND_ROBIN` och nu vill ändra den tooa tabellen distribueras på en kolumn, `CTAS` är hur du ändrar hello distribution kolumn.</span><span class="sxs-lookup"><span data-stu-id="29da9-120">If for example you originally created your table as `ROUND_ROBIN` and now want change it tooa table distributed on a column, `CTAS` is how you would change hello distribution column.</span></span> <span data-ttu-id="29da9-121">`CTAS`måste använda toochange partitionering, indexering eller kolumn typer.</span><span class="sxs-lookup"><span data-stu-id="29da9-121">`CTAS` can also be used toochange partitioning, indexing, or column types.</span></span>

<span data-ttu-id="29da9-122">Anta att du har skapat den här tabellen med hello standard fördelningstyp `ROUND_ROBIN` distribueras eftersom ingen distribution kolumn har angetts i hello `CREATE TABLE`.</span><span class="sxs-lookup"><span data-stu-id="29da9-122">Let's say you created this table using hello default distribution type of `ROUND_ROBIN` distributed since no distribution column was specified in hello `CREATE TABLE`.</span></span>

```sql
CREATE TABLE FactInternetSales
(
    ProductKey int NOT NULL,
    OrderDateKey int NOT NULL,
    DueDateKey int NOT NULL,
    ShipDateKey int NOT NULL,
    CustomerKey int NOT NULL,
    PromotionKey int NOT NULL,
    CurrencyKey int NOT NULL,
    SalesTerritoryKey int NOT NULL,
    SalesOrderNumber nvarchar(20) NOT NULL,
    SalesOrderLineNumber tinyint NOT NULL,
    RevisionNumber tinyint NOT NULL,
    OrderQuantity smallint NOT NULL,
    UnitPrice money NOT NULL,
    ExtendedAmount money NOT NULL,
    UnitPriceDiscountPct float NOT NULL,
    DiscountAmount float NOT NULL,
    ProductStandardCost money NOT NULL,
    TotalProductCost money NOT NULL,
    SalesAmount money NOT NULL,
    TaxAmt money NOT NULL,
    Freight money NOT NULL,
    CarrierTrackingNumber nvarchar(25),
    CustomerPONumber nvarchar(25)
);
```

<span data-ttu-id="29da9-123">Nu ska toocreate en ny kopia av den här tabellen med ett grupperat Columnstore-Index så att du kan dra nytta av hello prestanda för grupperade Columnstore-tabeller.</span><span class="sxs-lookup"><span data-stu-id="29da9-123">Now you want toocreate a new copy of this table with a Clustered Columnstore Index so that you can take advantage of hello performance of Clustered Columnstore tables.</span></span> <span data-ttu-id="29da9-124">Du bör också toodistribute tabellen på ProductKey eftersom du förutseende kopplingar på den här kolumnen och vill tooavoid dataflyttning under kopplingar på ProductKey.</span><span class="sxs-lookup"><span data-stu-id="29da9-124">You also want toodistribute this table on ProductKey since you are anticipating joins on this column and want tooavoid data movement during joins on ProductKey.</span></span> <span data-ttu-id="29da9-125">Slutligen bör du också tooadd partitionering på OrderDateKey så att du snabbt kan ta bort gamla data genom att släppa gamla partitioner.</span><span class="sxs-lookup"><span data-stu-id="29da9-125">Lastly you also want tooadd partitioning on OrderDateKey so that you can quickly delete old data by dropping old partitions.</span></span> <span data-ttu-id="29da9-126">Här är hello CTAS-instruktionen som kopierar din gamla tabell till en ny tabell.</span><span class="sxs-lookup"><span data-stu-id="29da9-126">Here is hello CTAS statement which would copy your old table into a new table.</span></span>

```sql
CREATE TABLE FactInternetSales_new
WITH
(
    CLUSTERED COLUMNSTORE INDEX,
    DISTRIBUTION = HASH(ProductKey),
    PARTITION
    (
        OrderDateKey RANGE RIGHT FOR VALUES
        (
        20000101,20010101,20020101,20030101,20040101,20050101,20060101,20070101,20080101,20090101,
        20100101,20110101,20120101,20130101,20140101,20150101,20160101,20170101,20180101,20190101,
        20200101,20210101,20220101,20230101,20240101,20250101,20260101,20270101,20280101,20290101
        )
    )
)
AS SELECT * FROM FactInternetSales;
```

<span data-ttu-id="29da9-127">Slutligen kan du byta namn på tabeller-tooswap i den nya tabellen och ta bort din gamla registret.</span><span class="sxs-lookup"><span data-stu-id="29da9-127">Finally you can rename your tables tooswap in your new table and then drop your old table.</span></span>

```sql
RENAME OBJECT FactInternetSales tooFactInternetSales_old;
RENAME OBJECT FactInternetSales_new tooFactInternetSales;

DROP TABLE FactInternetSales_old;
```

> [!NOTE]
> <span data-ttu-id="29da9-128">SQL Data Warehouse stöder inte än automatiskt skapande eller uppdatering av statistik.</span><span class="sxs-lookup"><span data-stu-id="29da9-128">Azure SQL Data Warehouse does not yet support auto create or auto update statistics.</span></span>  <span data-ttu-id="29da9-129">I ordning tooget hello bästa prestanda från dina frågor, är det viktigt att statistik skapas på alla kolumner i alla tabeller efter hello första inläsningen eller vid betydande dataändringar i hello data.</span><span class="sxs-lookup"><span data-stu-id="29da9-129">In order tooget hello best performance from your queries, it's important that statistics be created on all columns of all tables after hello first load or any substantial changes occur in hello data.</span></span>  <span data-ttu-id="29da9-130">En detaljerad förklaring av statistik finns hello [statistik] [ Statistics] -avsnittet i hello ämnesgruppen.</span><span class="sxs-lookup"><span data-stu-id="29da9-130">For a detailed explanation of statistics, see hello [Statistics][Statistics] topic in hello Develop group of topics.</span></span>
> 
> 

## <a name="using-ctas-toowork-around-unsupported-features"></a><span data-ttu-id="29da9-131">Med hjälp av CTAS toowork runt funktioner som inte stöds</span><span class="sxs-lookup"><span data-stu-id="29da9-131">Using CTAS toowork around unsupported features</span></span>
<span data-ttu-id="29da9-132">`CTAS`också kan vara används toowork kring ett antal hello stöds inte funktionerna i listan nedan.</span><span class="sxs-lookup"><span data-stu-id="29da9-132">`CTAS` can also be used toowork around a number of hello unsupported features listed below.</span></span> <span data-ttu-id="29da9-133">Denna funktion kan ofta vara toobe win/win situation som inte bara din kod blir kompatibla men det ofta körs snabbare i SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="29da9-133">This can often prove toobe a win/win situation as not only will your code be compliant but it will often execute faster on SQL Data Warehouse.</span></span> <span data-ttu-id="29da9-134">Detta är på grund av dess fullständigt parallelized design.</span><span class="sxs-lookup"><span data-stu-id="29da9-134">This is as a result of its fully parallelized design.</span></span> <span data-ttu-id="29da9-135">Scenarier som fungerade runt med CTAS är:</span><span class="sxs-lookup"><span data-stu-id="29da9-135">Scenarios that can be worked around with CTAS include:</span></span>

* <span data-ttu-id="29da9-136">ANSI kopplingar uppdateringar</span><span class="sxs-lookup"><span data-stu-id="29da9-136">ANSI JOINS on UPDATEs</span></span>
* <span data-ttu-id="29da9-137">ANSI-kopplingar på borttagningar</span><span class="sxs-lookup"><span data-stu-id="29da9-137">ANSI JOINs on DELETEs</span></span>
* <span data-ttu-id="29da9-138">MERGE-instruktion</span><span class="sxs-lookup"><span data-stu-id="29da9-138">MERGE statement</span></span>

> [!NOTE]
> <span data-ttu-id="29da9-139">Försök toothink ”CTAS första”.</span><span class="sxs-lookup"><span data-stu-id="29da9-139">Try toothink "CTAS first".</span></span> <span data-ttu-id="29da9-140">Om du tror att du kan lösa ett problem med `CTAS` hello bästa sätt tooapproach är vanligtvis den – även om du skriver mer data som ett resultat.</span><span class="sxs-lookup"><span data-stu-id="29da9-140">If you think you can solve a problem using `CTAS` then that is generally hello best way tooapproach it - even if you are writing more data as a result.</span></span>
> 
> 

## <a name="ansi-join-replacement-for-update-statements"></a><span data-ttu-id="29da9-141">ANSI join ersättning för update-instruktioner</span><span class="sxs-lookup"><span data-stu-id="29da9-141">ANSI join replacement for update statements</span></span>
<span data-ttu-id="29da9-142">Du kanske du har en komplex uppdatering som ansluter till fler än två tabeller med ANSI koppla syntax tooperform hello uppdatera eller ta bort.</span><span class="sxs-lookup"><span data-stu-id="29da9-142">You may find you have a complex update that joins more than two tables together using ANSI joining syntax tooperform hello UPDATE or DELETE.</span></span>

<span data-ttu-id="29da9-143">Anta att du haft tooupdate den här tabellen:</span><span class="sxs-lookup"><span data-stu-id="29da9-143">Imagine you had tooupdate this table:</span></span>

```sql
CREATE TABLE [dbo].[AnnualCategorySales]
(    [EnglishProductCategoryName]    NVARCHAR(50)    NOT NULL
,    [CalendarYear]                    SMALLINT        NOT NULL
,    [TotalSalesAmount]                MONEY            NOT NULL
)
WITH
(
    DISTRIBUTION = ROUND_ROBIN
)
;
```

<span data-ttu-id="29da9-144">hello ursprungliga frågan kanske har såg ut ungefär så här:</span><span class="sxs-lookup"><span data-stu-id="29da9-144">hello original query might have looked something like this:</span></span>

```sql
UPDATE    acs
SET        [TotalSalesAmount] = [fis].[TotalSalesAmount]
FROM    [dbo].[AnnualCategorySales]     AS acs
JOIN    (
        SELECT    [EnglishProductCategoryName]
        ,        [CalendarYear]
        ,        SUM([SalesAmount])                AS [TotalSalesAmount]
        FROM    [dbo].[FactInternetSales]        AS s
        JOIN    [dbo].[DimDate]                    AS d    ON s.[OrderDateKey]                = d.[DateKey]
        JOIN    [dbo].[DimProduct]                AS p    ON s.[ProductKey]                = p.[ProductKey]
        JOIN    [dbo].[DimProductSubCategory]    AS u    ON p.[ProductSubcategoryKey]    = u.[ProductSubcategoryKey]
        JOIN    [dbo].[DimProductCategory]        AS c    ON u.[ProductCategoryKey]        = c.[ProductCategoryKey]
        WHERE     [CalendarYear] = 2004
        GROUP BY
                [EnglishProductCategoryName]
        ,        [CalendarYear]
        ) AS fis
ON    [acs].[EnglishProductCategoryName]    = [fis].[EnglishProductCategoryName]
AND    [acs].[CalendarYear]                = [fis].[CalendarYear]
;
```

<span data-ttu-id="29da9-145">Eftersom SQL Data Warehouse inte stöder ANSI kopplingar i hello `FROM` -satsen i en `UPDATE` -instruktionen, du kan inte kopiera den här koden över utan att ändra något.</span><span class="sxs-lookup"><span data-stu-id="29da9-145">Since SQL Data Warehouse does not support ANSI joins in hello `FROM` clause of an `UPDATE` statement, you cannot copy this code over without changing it slightly.</span></span>

<span data-ttu-id="29da9-146">Du kan använda en kombination av en `CTAS` och en implicit ansluta tooreplace koden:</span><span class="sxs-lookup"><span data-stu-id="29da9-146">You can use a combination of a `CTAS` and an implicit join tooreplace this code:</span></span>

```sql
-- Create an interim table
CREATE TABLE CTAS_acs
WITH (DISTRIBUTION = ROUND_ROBIN)
AS
SELECT    ISNULL(CAST([EnglishProductCategoryName] AS NVARCHAR(50)),0)    AS [EnglishProductCategoryName]
,        ISNULL(CAST([CalendarYear] AS SMALLINT),0)                         AS [CalendarYear]
,        ISNULL(CAST(SUM([SalesAmount]) AS MONEY),0)                        AS [TotalSalesAmount]
FROM    [dbo].[FactInternetSales]        AS s
JOIN    [dbo].[DimDate]                    AS d    ON s.[OrderDateKey]                = d.[DateKey]
JOIN    [dbo].[DimProduct]                AS p    ON s.[ProductKey]                = p.[ProductKey]
JOIN    [dbo].[DimProductSubCategory]    AS u    ON p.[ProductSubcategoryKey]    = u.[ProductSubcategoryKey]
JOIN    [dbo].[DimProductCategory]        AS c    ON u.[ProductCategoryKey]        = c.[ProductCategoryKey]
WHERE     [CalendarYear] = 2004
GROUP BY
        [EnglishProductCategoryName]
,        [CalendarYear]
;

-- Use an implicit join tooperform hello update
UPDATE  AnnualCategorySales
SET     AnnualCategorySales.TotalSalesAmount = CTAS_ACS.TotalSalesAmount
FROM    CTAS_acs
WHERE   CTAS_acs.[EnglishProductCategoryName] = AnnualCategorySales.[EnglishProductCategoryName]
AND     CTAS_acs.[CalendarYear]               = AnnualCategorySales.[CalendarYear]
;

--Drop hello interim table
DROP TABLE CTAS_acs
;
```

## <a name="ansi-join-replacement-for-delete-statements"></a><span data-ttu-id="29da9-147">ANSI join ersättning för delete-instruktioner</span><span class="sxs-lookup"><span data-stu-id="29da9-147">ANSI join replacement for delete statements</span></span>
<span data-ttu-id="29da9-148">Ibland hello bästa metoden för att ta bort data är toouse `CTAS`.</span><span class="sxs-lookup"><span data-stu-id="29da9-148">Sometimes hello best approach for deleting data is toouse `CTAS`.</span></span> <span data-ttu-id="29da9-149">Välj hello data som du vill använda tookeep i stället för att ta bort hello data bara.</span><span class="sxs-lookup"><span data-stu-id="29da9-149">Rather than deleting hello data simply select hello data you want tookeep.</span></span> <span data-ttu-id="29da9-150">Den här särskilt för `DELETE` som använder ansi koppla syntax eftersom SQL Data Warehouse inte stöder ANSI-kopplingar i hello `FROM` -satsen i en `DELETE` instruktion.</span><span class="sxs-lookup"><span data-stu-id="29da9-150">This especially true for `DELETE` statements that use ansi joining syntax since SQL Data Warehouse does not support ANSI joins in hello `FROM` clause of a `DELETE` statement.</span></span>

<span data-ttu-id="29da9-151">Ett exempel på en konverterade DELETE-instruktion finns nedan:</span><span class="sxs-lookup"><span data-stu-id="29da9-151">An example of a converted DELETE statement is available below:</span></span>

```sql
CREATE TABLE dbo.DimProduct_upsert
WITH
(   Distribution=HASH(ProductKey)
,   CLUSTERED INDEX (ProductKey)
)
AS -- Select Data you wish tookeep
SELECT     p.ProductKey
,          p.EnglishProductName
,          p.Color
FROM       dbo.DimProduct p
RIGHT JOIN dbo.stg_DimProduct s
ON         p.ProductKey = s.ProductKey
;

RENAME OBJECT dbo.DimProduct        tooDimProduct_old;
RENAME OBJECT dbo.DimProduct_upsert tooDimProduct;
```

## <a name="replace-merge-statements"></a><span data-ttu-id="29da9-152">Ersätt merge-instruktioner</span><span class="sxs-lookup"><span data-stu-id="29da9-152">Replace merge statements</span></span>
<span data-ttu-id="29da9-153">Merge-instruktioner kan ersättas, minst del, med hjälp av `CTAS`.</span><span class="sxs-lookup"><span data-stu-id="29da9-153">Merge statements can be replaced, at least in part, by using `CTAS`.</span></span> <span data-ttu-id="29da9-154">Du kan konsolidera hello `INSERT` och hello `UPDATE` i en enskild instruktion.</span><span class="sxs-lookup"><span data-stu-id="29da9-154">You can consolidate hello `INSERT` and hello `UPDATE` into a single statement.</span></span> <span data-ttu-id="29da9-155">Alla borttagna poster måste toobe stängs av i andra instruktionen.</span><span class="sxs-lookup"><span data-stu-id="29da9-155">Any deleted records would need toobe closed off in a second statement.</span></span>

<span data-ttu-id="29da9-156">Ett exempel på en `UPSERT` finns nedan:</span><span class="sxs-lookup"><span data-stu-id="29da9-156">An example of an `UPSERT` is available below:</span></span>

```sql
CREATE TABLE dbo.[DimProduct_upsert]
WITH
(   DISTRIBUTION = HASH([ProductKey])
,   CLUSTERED INDEX ([ProductKey])
)
AS
-- New rows and new versions of rows
SELECT      s.[ProductKey]
,           s.[EnglishProductName]
,           s.[Color]
FROM      dbo.[stg_DimProduct] AS s
UNION ALL  
-- Keep rows that are not being touched
SELECT      p.[ProductKey]
,           p.[EnglishProductName]
,           p.[Color]
FROM      dbo.[DimProduct] AS p
WHERE NOT EXISTS
(   SELECT  *
    FROM    [dbo].[stg_DimProduct] s
    WHERE   s.[ProductKey] = p.[ProductKey]
)
;

RENAME OBJECT dbo.[DimProduct]          too[DimProduct_old];
RENAME OBJECT dbo.[DimpProduct_upsert]  too[DimProduct];

```

## <a name="ctas-recommendation-explicitly-state-data-type-and-nullability-of-output"></a><span data-ttu-id="29da9-157">CTAS rekommendation: uttryckligen ange datatyp och Nullbarheten för utdata</span><span class="sxs-lookup"><span data-stu-id="29da9-157">CTAS recommendation: Explicitly state data type and nullability of output</span></span>
<span data-ttu-id="29da9-158">När du migrerar kod kanske körs i den här typen av kodning mönster:</span><span class="sxs-lookup"><span data-stu-id="29da9-158">When migrating code you might find you run across this type of coding pattern:</span></span>

```sql
DECLARE @d decimal(7,2) = 85.455
,       @f float(24)    = 85.455

CREATE TABLE result
(result DECIMAL(7,2) NOT NULL
)
WITH (DISTRIBUTION = ROUND_ROBIN)

INSERT INTO result
SELECT @d*@f
;
```

<span data-ttu-id="29da9-159">Du tror instinktivt migrerar du den här koden tooa CTAS och du skulle vara korrekt.</span><span class="sxs-lookup"><span data-stu-id="29da9-159">Instinctively you might think you should migrate this code tooa CTAS and you would be correct.</span></span> <span data-ttu-id="29da9-160">Det finns emellertid ett dolt problem.</span><span class="sxs-lookup"><span data-stu-id="29da9-160">However, there is a hidden issue here.</span></span>

<span data-ttu-id="29da9-161">hello följande kod ger inte hello samma resultat:</span><span class="sxs-lookup"><span data-stu-id="29da9-161">hello following code does NOT yield hello same result:</span></span>

```sql
DECLARE @d decimal(7,2) = 85.455
,       @f float(24)    = 85.455
;

CREATE TABLE ctas_r
WITH (DISTRIBUTION = ROUND_ROBIN)
AS
SELECT @d*@f as result
;
```

<span data-ttu-id="29da9-162">Observera att hello kolumnen ”resultat” vidarebefordra innebär hello typ och Nullbarheten datavärden för hello uttryck.</span><span class="sxs-lookup"><span data-stu-id="29da9-162">Notice that hello column "result" carries forward hello data type and nullability values of hello expression.</span></span> <span data-ttu-id="29da9-163">Detta kan leda toosubtle avvikelser i värden om du inte är försiktig.</span><span class="sxs-lookup"><span data-stu-id="29da9-163">This can lead toosubtle variances in values if you aren't careful.</span></span>

<span data-ttu-id="29da9-164">Försök hello följande exempel:</span><span class="sxs-lookup"><span data-stu-id="29da9-164">Try hello following as an example:</span></span>

```sql
SELECT result,result*@d
from result
;

SELECT result,result*@d
from ctas_r
;
```

<span data-ttu-id="29da9-165">hello-värde som lagras i resultatet är olika.</span><span class="sxs-lookup"><span data-stu-id="29da9-165">hello value stored for result is different.</span></span> <span data-ttu-id="29da9-166">Hello beständiga värdet i hello resultatkolumnen används i andra blir uttryck hello fel ännu större.</span><span class="sxs-lookup"><span data-stu-id="29da9-166">As hello persisted value in hello result column is used in other expressions hello error becomes even more significant.</span></span>

![][1]

<span data-ttu-id="29da9-167">Detta är särskilt viktigt för migrering av data.</span><span class="sxs-lookup"><span data-stu-id="29da9-167">This is particularly important for data migrations.</span></span> <span data-ttu-id="29da9-168">Även om hello andra frågan tvekan exaktare finns problemet.</span><span class="sxs-lookup"><span data-stu-id="29da9-168">Even though hello second query is arguably more accurate there is a problem.</span></span> <span data-ttu-id="29da9-169">hello data skulle vara olika jämfört med toohello källsystemet och som leder tooquestions för i hello migrering.</span><span class="sxs-lookup"><span data-stu-id="29da9-169">hello data would be different compared toohello source system and that leads tooquestions of integrity in hello migration.</span></span> <span data-ttu-id="29da9-170">Detta är en av de sällsynta fall där hello ”fel” svaret är faktiskt hello höger en!</span><span class="sxs-lookup"><span data-stu-id="29da9-170">This is one of those rare cases where hello "wrong" answer is actually hello right one!</span></span>

<span data-ttu-id="29da9-171">hello orsak ser vi den här skillnader mellan hello två resultat är nere tooimplicit skriver omvandling.</span><span class="sxs-lookup"><span data-stu-id="29da9-171">hello reason we see this disparity between hello two results is down tooimplicit type casting.</span></span> <span data-ttu-id="29da9-172">I hello definierar första exempeltabell hello hello kolumndefinitionen.</span><span class="sxs-lookup"><span data-stu-id="29da9-172">In hello first example hello table defines hello column definition.</span></span> <span data-ttu-id="29da9-173">En implicit typkonvertering inträffar när hello rad infogas.</span><span class="sxs-lookup"><span data-stu-id="29da9-173">When hello row is inserted an implicit type conversion occurs.</span></span> <span data-ttu-id="29da9-174">I andra hello-exemplet finns det ingen implicit typkonvertering som hello uttrycket definierar datatypen för kolumnen hello.</span><span class="sxs-lookup"><span data-stu-id="29da9-174">In hello second example there is no implicit type conversion as hello expression defines data type of hello column.</span></span> <span data-ttu-id="29da9-175">Observera också hello kolumnen i hello andra exemplet har definierats som en nullbar kolumn i hello första exemplet har den inte.</span><span class="sxs-lookup"><span data-stu-id="29da9-175">Notice also that hello column in hello second example has been defined as a NULLable column whereas in hello first example it has not.</span></span> <span data-ttu-id="29da9-176">När hello tabell skapades i definierades uttryckligen hello första exempel Nullbarhet.</span><span class="sxs-lookup"><span data-stu-id="29da9-176">When hello table was created in hello first example column nullability was explicitly defined.</span></span> <span data-ttu-id="29da9-177">I andra hello-exemplet lämnades bara toohello uttryck och som standard det skulle resultera i en NULL-definition.</span><span class="sxs-lookup"><span data-stu-id="29da9-177">In hello second example it was just left toohello expression and by default this would result in a NULL definition.</span></span>  

<span data-ttu-id="29da9-178">tooresolve dessa problem som du måste uttryckligen ange hello typkonvertering och Nullbarheten i hello `SELECT` del av hello `CTAS` instruktionen.</span><span class="sxs-lookup"><span data-stu-id="29da9-178">tooresolve these issues you must explicitly set hello type conversion and nullability in hello `SELECT` portion of hello `CTAS` statement.</span></span> <span data-ttu-id="29da9-179">Du kan inte ange dessa egenskaper i hello Skapa tabell del.</span><span class="sxs-lookup"><span data-stu-id="29da9-179">You cannot set these properties in hello create table part.</span></span>

<span data-ttu-id="29da9-180">hello exemplet nedan demonstrerar hur toofix hello kod:</span><span class="sxs-lookup"><span data-stu-id="29da9-180">hello example below demonstrates how toofix hello code:</span></span>

```sql
DECLARE @d decimal(7,2) = 85.455
,       @f float(24)    = 85.455

CREATE TABLE ctas_r
WITH (DISTRIBUTION = ROUND_ROBIN)
AS
SELECT ISNULL(CAST(@d*@f AS DECIMAL(7,2)),0) as result
```

<span data-ttu-id="29da9-181">Observera följande hello:</span><span class="sxs-lookup"><span data-stu-id="29da9-181">Note hello following:</span></span>

* <span data-ttu-id="29da9-182">CAST eller CONVERT kunde har använts</span><span class="sxs-lookup"><span data-stu-id="29da9-182">CAST or CONVERT could have been used</span></span>
* <span data-ttu-id="29da9-183">ISNULL är används tooforce Nullbarheten inte COALESCE</span><span class="sxs-lookup"><span data-stu-id="29da9-183">ISNULL is used tooforce NULLability not COALESCE</span></span>
* <span data-ttu-id="29da9-184">ISNULL är hello yttersta funktion</span><span class="sxs-lookup"><span data-stu-id="29da9-184">ISNULL is hello outermost function</span></span>
* <span data-ttu-id="29da9-185">hello andra delen av hello ISNULL är en konstant d.v.s. 0</span><span class="sxs-lookup"><span data-stu-id="29da9-185">hello second part of hello ISNULL is a constant i.e. 0</span></span>

> [!NOTE]
> <span data-ttu-id="29da9-186">Hello Nullbarheten toobe inställd på rätt sätt är avgörande toouse `ISNULL` och inte `COALESCE`.</span><span class="sxs-lookup"><span data-stu-id="29da9-186">For hello nullability toobe correctly set it is vital toouse `ISNULL` and not `COALESCE`.</span></span> <span data-ttu-id="29da9-187">`COALESCE`är inte en deterministisk funktion och kan därför hello resultatet av hello uttryck alltid tas kan ha värdet null.</span><span class="sxs-lookup"><span data-stu-id="29da9-187">`COALESCE` is not a deterministic function and so hello result of hello expression will always be NULLable.</span></span> <span data-ttu-id="29da9-188">`ISNULL`är olika.</span><span class="sxs-lookup"><span data-stu-id="29da9-188">`ISNULL` is different.</span></span> <span data-ttu-id="29da9-189">Det är deterministisk.</span><span class="sxs-lookup"><span data-stu-id="29da9-189">It is deterministic.</span></span> <span data-ttu-id="29da9-190">Därför hello när andra delen av hello `ISNULL` funktionen är en konstant eller en litteral sedan hello resultatvärdet blir NOT NULL.</span><span class="sxs-lookup"><span data-stu-id="29da9-190">Therefore when hello second part of hello `ISNULL` function is a constant or a literal then hello resulting value will be NOT NULL.</span></span>
> 
> 

<span data-ttu-id="29da9-191">Den här beskrivningen är inte bara användbar för att säkerställa hello integriteten hos dina beräkningar.</span><span class="sxs-lookup"><span data-stu-id="29da9-191">This tip is not just useful for ensuring hello integrity of your calculations.</span></span> <span data-ttu-id="29da9-192">Det är också viktigt för växling av partition i tabellen.</span><span class="sxs-lookup"><span data-stu-id="29da9-192">It is also important for table partition switching.</span></span> <span data-ttu-id="29da9-193">Anta att du har den här tabellen definieras som din faktum:</span><span class="sxs-lookup"><span data-stu-id="29da9-193">Imagine you have this table defined as your fact:</span></span>

```sql
CREATE TABLE [dbo].[Sales]
(
    [date]      INT     NOT NULL
,   [product]   INT     NOT NULL
,   [store]     INT     NOT NULL
,   [quantity]  INT     NOT NULL
,   [price]     MONEY   NOT NULL
,   [amount]    MONEY   NOT NULL
)
WITH
(   DISTRIBUTION = HASH([product])
,   PARTITION   (   [date] RANGE RIGHT FOR VALUES
                    (20000101,20010101,20020101
                    ,20030101,20040101,20050101
                    )
                )
)
;
```

<span data-ttu-id="29da9-194">Hello värdefältet är dock ett beräknat uttryck inte är en del av hello källdata.</span><span class="sxs-lookup"><span data-stu-id="29da9-194">However, hello value field is a calculated expression it is not part of hello source data.</span></span>

<span data-ttu-id="29da9-195">toocreate partitionerade datamängden du kanske vill toodo detta:</span><span class="sxs-lookup"><span data-stu-id="29da9-195">toocreate your partitioned dataset you might want toodo this:</span></span>

```sql
CREATE TABLE [dbo].[Sales_in]
WITH    
(   DISTRIBUTION = HASH([product])
,   PARTITION   (   [date] RANGE RIGHT FOR VALUES
                    (20000101,20010101
                    )
                )
)
AS
SELECT
    [date]    
,   [product]
,   [store]
,   [quantity]
,   [price]   
,   [quantity]*[price]  AS [amount]
FROM [stg].[source]
OPTION (LABEL = 'CTAS : Partition IN table : Create')
;
```

<span data-ttu-id="29da9-196">hello frågan körs perfekt bra.</span><span class="sxs-lookup"><span data-stu-id="29da9-196">hello query would run perfectly fine.</span></span> <span data-ttu-id="29da9-197">hello problemet kommer när du försöker tooperform hello partition växel.</span><span class="sxs-lookup"><span data-stu-id="29da9-197">hello problem comes when you try tooperform hello partition switch.</span></span> <span data-ttu-id="29da9-198">Hej tabelldefinitionerna stämmer inte överens.</span><span class="sxs-lookup"><span data-stu-id="29da9-198">hello table definitions do not match.</span></span> <span data-ttu-id="29da9-199">toomake hello definitioner matchar hello CTAS måste toobe ändras.</span><span class="sxs-lookup"><span data-stu-id="29da9-199">toomake hello table definitions match hello CTAS needs toobe modified.</span></span>

```sql
CREATE TABLE [dbo].[Sales_in]
WITH    
(   DISTRIBUTION = HASH([product])
,   PARTITION   (   [date] RANGE RIGHT FOR VALUES
                    (20000101,20010101
                    )
                )
)
AS
SELECT
    [date]    
,   [product]
,   [store]
,   [quantity]
,   [price]   
,   ISNULL(CAST([quantity]*[price] AS MONEY),0) AS [amount]
FROM [stg].[source]
OPTION (LABEL = 'CTAS : Partition IN table : Create');
```

<span data-ttu-id="29da9-200">Du kan se därför att typen konsekvens och underhålla Nullbarheten egenskaper för en CTAS är det engineering bästa praxis.</span><span class="sxs-lookup"><span data-stu-id="29da9-200">You can see therefore that type consistency and maintaining nullability properties on a CTAS is a good engineering best practice.</span></span> <span data-ttu-id="29da9-201">Den hjälper toomaintain integritet i beräkningarna och säkerställer också att växla partition är möjligt.</span><span class="sxs-lookup"><span data-stu-id="29da9-201">It helps toomaintain integrity in your calculations and also ensures that partition switching is possible.</span></span>

<span data-ttu-id="29da9-202">Mer information om hur du använder finns tooMSDN [CTAS][CTAS].</span><span class="sxs-lookup"><span data-stu-id="29da9-202">Please refer tooMSDN for more information on using [CTAS][CTAS].</span></span> <span data-ttu-id="29da9-203">Det är en av de viktigaste hello-instruktioner i Azure SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="29da9-203">It is one of hello most important statements in Azure SQL Data Warehouse.</span></span> <span data-ttu-id="29da9-204">Kontrollera att du verkligen förstår.</span><span class="sxs-lookup"><span data-stu-id="29da9-204">Make sure you thoroughly understand it.</span></span>

## <a name="next-steps"></a><span data-ttu-id="29da9-205">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="29da9-205">Next steps</span></span>
<span data-ttu-id="29da9-206">För fler utvecklingstips, se [utvecklingsöversikt][development overview].</span><span class="sxs-lookup"><span data-stu-id="29da9-206">For more development tips, see [development overview][development overview].</span></span>

<!--Image references-->
[1]: media/sql-data-warehouse-develop-ctas/ctas-results.png

<!--Article references-->
[development overview]: sql-data-warehouse-overview-develop.md
[Statistics]: ./sql-data-warehouse-tables-statistics.md

<!--MSDN references-->
[CTAS]: https://msdn.microsoft.com/library/mt204041.aspx

<!--Other Web references-->
