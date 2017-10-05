---
title: "Skapa tabell som Välj (CTAS) i SQL Data Warehouse | Microsoft Docs"
description: "Tips för att koda med tabellen skapa som select (CTAS)-instruktionen i Azure SQL Data Warehouse för utveckling av lösningar."
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
ms.openlocfilehash: cb08313726e8135feaa9b413937c2197ea397f4b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="create-table-as-select-ctas-in-sql-data-warehouse"></a><span data-ttu-id="6178d-103">Skapa en tabell som Select (CTAS) i SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="6178d-103">Create Table As Select (CTAS) in SQL Data Warehouse</span></span>
<span data-ttu-id="6178d-104">Skapa tabell som markerar eller `CTAS` är en av de viktigaste T-SQL-funktionerna tillgängliga.</span><span class="sxs-lookup"><span data-stu-id="6178d-104">Create table as select or `CTAS` is one of the most important T-SQL features available.</span></span> <span data-ttu-id="6178d-105">Det är en helt parallelized åtgärd som skapar en ny tabell baserat på resultatet av en SELECT-instruktion.</span><span class="sxs-lookup"><span data-stu-id="6178d-105">It is a fully parallelized operation that creates a new table based on the output of a SELECT statement.</span></span> <span data-ttu-id="6178d-106">`CTAS`är det enklaste och snabbaste sättet att skapa en kopia av en tabell.</span><span class="sxs-lookup"><span data-stu-id="6178d-106">`CTAS` is the simplest and fastest way to create a copy of a table.</span></span> <span data-ttu-id="6178d-107">Det här dokumentet innehåller både exempel och bästa praxis för `CTAS`.</span><span class="sxs-lookup"><span data-stu-id="6178d-107">This document provides both examples and best practices for `CTAS`.</span></span>

## <a name="selectinto-vs-ctas"></a><span data-ttu-id="6178d-108">VÄLJ... I vs. CTAS</span><span class="sxs-lookup"><span data-stu-id="6178d-108">SELECT..INTO vs. CTAS</span></span>
<span data-ttu-id="6178d-109">Du kan överväga att `CTAS` som en mycket debiteras version av `SELECT..INTO`.</span><span class="sxs-lookup"><span data-stu-id="6178d-109">You can consider `CTAS` as a super-charged version of `SELECT..INTO`.</span></span>

<span data-ttu-id="6178d-110">Nedan visas ett exempel på en enkel `SELECT..INTO` instruktionen:</span><span class="sxs-lookup"><span data-stu-id="6178d-110">Below is an example of a simple `SELECT..INTO` statement:</span></span>

```sql
SELECT *
INTO    [dbo].[FactInternetSales_new]
FROM    [dbo].[FactInternetSales]
```

<span data-ttu-id="6178d-111">I exemplet ovan `[dbo].[FactInternetSales_new]` skulle skapas som ROUND_ROBIN distribuerade tabell med ett GRUPPERAT COLUMNSTORE-INDEX på den som dessa standardinställningar tabell i Azure SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="6178d-111">In the example above `[dbo].[FactInternetSales_new]` would be created as ROUND_ROBIN distributed table with a CLUSTERED COLUMNSTORE INDEX on it as these are the table defaults in Azure SQL Data Warehouse.</span></span>

<span data-ttu-id="6178d-112">`SELECT..INTO`dock tillåter inte att du kan ändra metoden distribution eller Indextypen som en del av åtgärden.</span><span class="sxs-lookup"><span data-stu-id="6178d-112">`SELECT..INTO` however does not allow you to change either the distribution method or the index type as part of the operation.</span></span> <span data-ttu-id="6178d-113">Det är där `CTAS` kommer in.</span><span class="sxs-lookup"><span data-stu-id="6178d-113">This is where `CTAS` comes in.</span></span>

<span data-ttu-id="6178d-114">Konvertera ovan för att `CTAS` är ganska enkel:</span><span class="sxs-lookup"><span data-stu-id="6178d-114">To convert the above to `CTAS` is quite straight-forward:</span></span>

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

<span data-ttu-id="6178d-115">Med `CTAS` kan du ändra både fördelningen av tabelldata samt tabelltypen.</span><span class="sxs-lookup"><span data-stu-id="6178d-115">With `CTAS` you are able to change both the distribution of the table data as well as the table type.</span></span> 

> [!NOTE]
> <span data-ttu-id="6178d-116">Om du bara vill ändra index i din `CTAS` åtgärden och källtabellen är hash distribueras sedan din `CTAS` åtgärden fungerar bäst om du behåller samma kolumn- och distributionstypen.</span><span class="sxs-lookup"><span data-stu-id="6178d-116">If you are only trying to change the index in your `CTAS` operation and the source table is hash distributed then your `CTAS` operation will perform best if you maintain the same distribution column and data type.</span></span> <span data-ttu-id="6178d-117">På så sätt undviker mellan distribution dataflyttning under åtgärden, vilket är mer effektivt.</span><span class="sxs-lookup"><span data-stu-id="6178d-117">This will avoid cross distribution data movement during the operation which is more efficient.</span></span>
> 
> 

## <a name="using-ctas-to-copy-a-table"></a><span data-ttu-id="6178d-118">Använda CTAS för att kopiera en tabell</span><span class="sxs-lookup"><span data-stu-id="6178d-118">Using CTAS to copy a table</span></span>
<span data-ttu-id="6178d-119">Kanske en av de vanligaste använder av `CTAS` skapar en kopia av en tabell, så att du kan ändra DDL.</span><span class="sxs-lookup"><span data-stu-id="6178d-119">Perhaps one of the most common uses of `CTAS` is creating a copy of a table so that you can change the DDL.</span></span> <span data-ttu-id="6178d-120">Om till exempel din tabell som ursprungligen skapades `ROUND_ROBIN` och nu vill ändra den till en tabell som distribueras på en kolumn, `CTAS` är hur du ändrar kolumnen distribution.</span><span class="sxs-lookup"><span data-stu-id="6178d-120">If for example you originally created your table as `ROUND_ROBIN` and now want change it to a table distributed on a column, `CTAS` is how you would change the distribution column.</span></span> <span data-ttu-id="6178d-121">`CTAS`kan också användas för att ändra partitionering, indexering eller kolumn typer.</span><span class="sxs-lookup"><span data-stu-id="6178d-121">`CTAS` can also be used to change partitioning, indexing, or column types.</span></span>

<span data-ttu-id="6178d-122">Anta att du har skapat den här tabellen med standardtypen för distribution av `ROUND_ROBIN` distribueras eftersom ingen distribution kolumn har angetts i den `CREATE TABLE`.</span><span class="sxs-lookup"><span data-stu-id="6178d-122">Let's say you created this table using the default distribution type of `ROUND_ROBIN` distributed since no distribution column was specified in the `CREATE TABLE`.</span></span>

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

<span data-ttu-id="6178d-123">Nu vill du skapa en ny kopia av den här tabellen med ett grupperat Columnstore-Index så att du kan dra nytta av grupperade Columnstore-tabeller.</span><span class="sxs-lookup"><span data-stu-id="6178d-123">Now you want to create a new copy of this table with a Clustered Columnstore Index so that you can take advantage of the performance of Clustered Columnstore tables.</span></span> <span data-ttu-id="6178d-124">Du vill distribuera den här tabellen på ProductKey eftersom du förutser kopplingar på den här kolumnen och vill undvika dataflyttning under kopplingar på ProductKey.</span><span class="sxs-lookup"><span data-stu-id="6178d-124">You also want to distribute this table on ProductKey since you are anticipating joins on this column and want to avoid data movement during joins on ProductKey.</span></span> <span data-ttu-id="6178d-125">Till sist också du lägga till partitionering på OrderDateKey så att du snabbt kan ta bort gamla data genom att släppa gamla partitioner.</span><span class="sxs-lookup"><span data-stu-id="6178d-125">Lastly you also want to add partitioning on OrderDateKey so that you can quickly delete old data by dropping old partitions.</span></span> <span data-ttu-id="6178d-126">Det här är den CTAS-instruktionen som kopierar din gamla tabell till en ny tabell.</span><span class="sxs-lookup"><span data-stu-id="6178d-126">Here is the CTAS statement which would copy your old table into a new table.</span></span>

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

<span data-ttu-id="6178d-127">Slutligen kan du byta namn på tabellerna för att växla i den nya tabellen och ta bort din gamla registret.</span><span class="sxs-lookup"><span data-stu-id="6178d-127">Finally you can rename your tables to swap in your new table and then drop your old table.</span></span>

```sql
RENAME OBJECT FactInternetSales TO FactInternetSales_old;
RENAME OBJECT FactInternetSales_new TO FactInternetSales;

DROP TABLE FactInternetSales_old;
```

> [!NOTE]
> <span data-ttu-id="6178d-128">SQL Data Warehouse stöder inte än automatiskt skapande eller uppdatering av statistik.</span><span class="sxs-lookup"><span data-stu-id="6178d-128">Azure SQL Data Warehouse does not yet support auto create or auto update statistics.</span></span>  <span data-ttu-id="6178d-129">För att få bästa möjliga prestanda från dina frågor, är det viktigt att statistik skapas på alla kolumner i alla tabeller efter den första inläsningen eller vid betydande dataändringar.</span><span class="sxs-lookup"><span data-stu-id="6178d-129">In order to get the best performance from your queries, it's important that statistics be created on all columns of all tables after the first load or any substantial changes occur in the data.</span></span>  <span data-ttu-id="6178d-130">En detaljerad förklaring av statistik finns i ämnet [Statistik][Statistics] i ämnesgruppen Utveckla.</span><span class="sxs-lookup"><span data-stu-id="6178d-130">For a detailed explanation of statistics, see the [Statistics][Statistics] topic in the Develop group of topics.</span></span>
> 
> 

## <a name="using-ctas-to-work-around-unsupported-features"></a><span data-ttu-id="6178d-131">Använda CTAS för att undvika funktioner som inte stöds</span><span class="sxs-lookup"><span data-stu-id="6178d-131">Using CTAS to work around unsupported features</span></span>
<span data-ttu-id="6178d-132">`CTAS`kan också användas för att undvika ett antal stöds inte funktionerna i listan nedan.</span><span class="sxs-lookup"><span data-stu-id="6178d-132">`CTAS` can also be used to work around a number of the unsupported features listed below.</span></span> <span data-ttu-id="6178d-133">Detta kan ofta visar sig vara en win/win-situation som inte bara din kod blir kompatibla men det ofta körs snabbare i SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="6178d-133">This can often prove to be a win/win situation as not only will your code be compliant but it will often execute faster on SQL Data Warehouse.</span></span> <span data-ttu-id="6178d-134">Detta är på grund av dess fullständigt parallelized design.</span><span class="sxs-lookup"><span data-stu-id="6178d-134">This is as a result of its fully parallelized design.</span></span> <span data-ttu-id="6178d-135">Scenarier som fungerade runt med CTAS är:</span><span class="sxs-lookup"><span data-stu-id="6178d-135">Scenarios that can be worked around with CTAS include:</span></span>

* <span data-ttu-id="6178d-136">ANSI kopplingar uppdateringar</span><span class="sxs-lookup"><span data-stu-id="6178d-136">ANSI JOINS on UPDATEs</span></span>
* <span data-ttu-id="6178d-137">ANSI-kopplingar på borttagningar</span><span class="sxs-lookup"><span data-stu-id="6178d-137">ANSI JOINs on DELETEs</span></span>
* <span data-ttu-id="6178d-138">MERGE-instruktion</span><span class="sxs-lookup"><span data-stu-id="6178d-138">MERGE statement</span></span>

> [!NOTE]
> <span data-ttu-id="6178d-139">Försök att tänka ”CTAS första”.</span><span class="sxs-lookup"><span data-stu-id="6178d-139">Try to think "CTAS first".</span></span> <span data-ttu-id="6178d-140">Om du tror att du kan lösa ett problem med `CTAS` sedan som är vanligtvis det bästa sättet att närma sig den – även om du skriver mer data som ett resultat.</span><span class="sxs-lookup"><span data-stu-id="6178d-140">If you think you can solve a problem using `CTAS` then that is generally the best way to approach it - even if you are writing more data as a result.</span></span>
> 
> 

## <a name="ansi-join-replacement-for-update-statements"></a><span data-ttu-id="6178d-141">ANSI join ersättning för update-instruktioner</span><span class="sxs-lookup"><span data-stu-id="6178d-141">ANSI join replacement for update statements</span></span>
<span data-ttu-id="6178d-142">Du kanske du har en komplex uppdatering som ansluter till fler än två tabeller tillsammans använder ANSI koppla syntax för att utföra UPDATE- eller DELETE.</span><span class="sxs-lookup"><span data-stu-id="6178d-142">You may find you have a complex update that joins more than two tables together using ANSI joining syntax to perform the UPDATE or DELETE.</span></span>

<span data-ttu-id="6178d-143">Anta att du var tvungen att uppdatera den här tabellen:</span><span class="sxs-lookup"><span data-stu-id="6178d-143">Imagine you had to update this table:</span></span>

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

<span data-ttu-id="6178d-144">Den ursprungliga frågan kanske har såg ut ungefär så här:</span><span class="sxs-lookup"><span data-stu-id="6178d-144">The original query might have looked something like this:</span></span>

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

<span data-ttu-id="6178d-145">Eftersom SQL Data Warehouse inte stöder ANSI kopplingar i den `FROM` -satsen i en `UPDATE` -instruktionen, du kan inte kopiera den här koden över utan att ändra något.</span><span class="sxs-lookup"><span data-stu-id="6178d-145">Since SQL Data Warehouse does not support ANSI joins in the `FROM` clause of an `UPDATE` statement, you cannot copy this code over without changing it slightly.</span></span>

<span data-ttu-id="6178d-146">Du kan använda en kombination av en `CTAS` och en indirekt koppling att ersätta den här koden:</span><span class="sxs-lookup"><span data-stu-id="6178d-146">You can use a combination of a `CTAS` and an implicit join to replace this code:</span></span>

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

-- Use an implicit join to perform the update
UPDATE  AnnualCategorySales
SET     AnnualCategorySales.TotalSalesAmount = CTAS_ACS.TotalSalesAmount
FROM    CTAS_acs
WHERE   CTAS_acs.[EnglishProductCategoryName] = AnnualCategorySales.[EnglishProductCategoryName]
AND     CTAS_acs.[CalendarYear]               = AnnualCategorySales.[CalendarYear]
;

--Drop the interim table
DROP TABLE CTAS_acs
;
```

## <a name="ansi-join-replacement-for-delete-statements"></a><span data-ttu-id="6178d-147">ANSI join ersättning för delete-instruktioner</span><span class="sxs-lookup"><span data-stu-id="6178d-147">ANSI join replacement for delete statements</span></span>
<span data-ttu-id="6178d-148">Ibland den bästa metoden för att ta bort data är att använda `CTAS`.</span><span class="sxs-lookup"><span data-stu-id="6178d-148">Sometimes the best approach for deleting data is to use `CTAS`.</span></span> <span data-ttu-id="6178d-149">Välj de data som du vill behålla i stället för att bara ta bort data.</span><span class="sxs-lookup"><span data-stu-id="6178d-149">Rather than deleting the data simply select the data you want to keep.</span></span> <span data-ttu-id="6178d-150">Den här särskilt för `DELETE` som använder ansi som sammanbinder syntax eftersom SQL Data Warehouse inte stöder ANSI kopplingar i den `FROM` -satsen i en `DELETE` instruktion.</span><span class="sxs-lookup"><span data-stu-id="6178d-150">This especially true for `DELETE` statements that use ansi joining syntax since SQL Data Warehouse does not support ANSI joins in the `FROM` clause of a `DELETE` statement.</span></span>

<span data-ttu-id="6178d-151">Ett exempel på en konverterade DELETE-instruktion finns nedan:</span><span class="sxs-lookup"><span data-stu-id="6178d-151">An example of a converted DELETE statement is available below:</span></span>

```sql
CREATE TABLE dbo.DimProduct_upsert
WITH
(   Distribution=HASH(ProductKey)
,   CLUSTERED INDEX (ProductKey)
)
AS -- Select Data you wish to keep
SELECT     p.ProductKey
,          p.EnglishProductName
,          p.Color
FROM       dbo.DimProduct p
RIGHT JOIN dbo.stg_DimProduct s
ON         p.ProductKey = s.ProductKey
;

RENAME OBJECT dbo.DimProduct        TO DimProduct_old;
RENAME OBJECT dbo.DimProduct_upsert TO DimProduct;
```

## <a name="replace-merge-statements"></a><span data-ttu-id="6178d-152">Ersätt merge-instruktioner</span><span class="sxs-lookup"><span data-stu-id="6178d-152">Replace merge statements</span></span>
<span data-ttu-id="6178d-153">Merge-instruktioner kan ersättas, minst del, med hjälp av `CTAS`.</span><span class="sxs-lookup"><span data-stu-id="6178d-153">Merge statements can be replaced, at least in part, by using `CTAS`.</span></span> <span data-ttu-id="6178d-154">Du kan konsolidera den `INSERT` och `UPDATE` i en enskild instruktion.</span><span class="sxs-lookup"><span data-stu-id="6178d-154">You can consolidate the `INSERT` and the `UPDATE` into a single statement.</span></span> <span data-ttu-id="6178d-155">Alla borttagna poster måste vara stängda i ett andra uttryck.</span><span class="sxs-lookup"><span data-stu-id="6178d-155">Any deleted records would need to be closed off in a second statement.</span></span>

<span data-ttu-id="6178d-156">Ett exempel på en `UPSERT` finns nedan:</span><span class="sxs-lookup"><span data-stu-id="6178d-156">An example of an `UPSERT` is available below:</span></span>

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

RENAME OBJECT dbo.[DimProduct]          TO [DimProduct_old];
RENAME OBJECT dbo.[DimpProduct_upsert]  TO [DimProduct];

```

## <a name="ctas-recommendation-explicitly-state-data-type-and-nullability-of-output"></a><span data-ttu-id="6178d-157">CTAS rekommendation: uttryckligen ange datatyp och Nullbarheten för utdata</span><span class="sxs-lookup"><span data-stu-id="6178d-157">CTAS recommendation: Explicitly state data type and nullability of output</span></span>
<span data-ttu-id="6178d-158">När du migrerar kod kanske körs i den här typen av kodning mönster:</span><span class="sxs-lookup"><span data-stu-id="6178d-158">When migrating code you might find you run across this type of coding pattern:</span></span>

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

<span data-ttu-id="6178d-159">Du kanske instinktivt tror du bör migrera koden till en CTAS och du skulle vara korrekt.</span><span class="sxs-lookup"><span data-stu-id="6178d-159">Instinctively you might think you should migrate this code to a CTAS and you would be correct.</span></span> <span data-ttu-id="6178d-160">Det finns emellertid ett dolt problem.</span><span class="sxs-lookup"><span data-stu-id="6178d-160">However, there is a hidden issue here.</span></span>

<span data-ttu-id="6178d-161">Följande kod ger inte samma resultat:</span><span class="sxs-lookup"><span data-stu-id="6178d-161">The following code does NOT yield the same result:</span></span>

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

<span data-ttu-id="6178d-162">Observera att kolumnen ”resultat” vidarebefordra innebär datavärden för typ och Nullbarheten för uttrycket.</span><span class="sxs-lookup"><span data-stu-id="6178d-162">Notice that the column "result" carries forward the data type and nullability values of the expression.</span></span> <span data-ttu-id="6178d-163">Detta kan leda till diskret avvikelser i värden om du inte är försiktig.</span><span class="sxs-lookup"><span data-stu-id="6178d-163">This can lead to subtle variances in values if you aren't careful.</span></span>

<span data-ttu-id="6178d-164">Försök med följande exempel:</span><span class="sxs-lookup"><span data-stu-id="6178d-164">Try the following as an example:</span></span>

```sql
SELECT result,result*@d
from result
;

SELECT result,result*@d
from ctas_r
;
```

<span data-ttu-id="6178d-165">Det värde som lagras för resultatet är olika.</span><span class="sxs-lookup"><span data-stu-id="6178d-165">The value stored for result is different.</span></span> <span data-ttu-id="6178d-166">Det beständiga värdet i resultatkolumnen används i andra uttryck blir felet ännu större.</span><span class="sxs-lookup"><span data-stu-id="6178d-166">As the persisted value in the result column is used in other expressions the error becomes even more significant.</span></span>

![][1]

<span data-ttu-id="6178d-167">Detta är särskilt viktigt för migrering av data.</span><span class="sxs-lookup"><span data-stu-id="6178d-167">This is particularly important for data migrations.</span></span> <span data-ttu-id="6178d-168">Även om den andra frågan tvekan exaktare finns problemet.</span><span class="sxs-lookup"><span data-stu-id="6178d-168">Even though the second query is arguably more accurate there is a problem.</span></span> <span data-ttu-id="6178d-169">Data kan vara olika jämfört med källsystemet och som leder till frågor för migreringen.</span><span class="sxs-lookup"><span data-stu-id="6178d-169">The data would be different compared to the source system and that leads to questions of integrity in the migration.</span></span> <span data-ttu-id="6178d-170">Detta är en av de sällsynta fall där ”fel” svaret är verkligen det högra!</span><span class="sxs-lookup"><span data-stu-id="6178d-170">This is one of those rare cases where the "wrong" answer is actually the right one!</span></span>

<span data-ttu-id="6178d-171">Det ser vi den här skillnader mellan de två resultaten beror till implicit typ omvandling.</span><span class="sxs-lookup"><span data-stu-id="6178d-171">The reason we see this disparity between the two results is down to implicit type casting.</span></span> <span data-ttu-id="6178d-172">I det första exemplet definierar tabellen kolumndefinitionen.</span><span class="sxs-lookup"><span data-stu-id="6178d-172">In the first example the table defines the column definition.</span></span> <span data-ttu-id="6178d-173">En implicit typkonvertering inträffar när raden infogas.</span><span class="sxs-lookup"><span data-stu-id="6178d-173">When the row is inserted an implicit type conversion occurs.</span></span> <span data-ttu-id="6178d-174">I det andra exemplet finns det ingen implicit typkonvertering som uttrycket definierar datatypen för kolumnen.</span><span class="sxs-lookup"><span data-stu-id="6178d-174">In the second example there is no implicit type conversion as the expression defines data type of the column.</span></span> <span data-ttu-id="6178d-175">Observera också att kolumnen i det andra exemplet har definierats som en nullbar kolumn i det första exemplet har den inte.</span><span class="sxs-lookup"><span data-stu-id="6178d-175">Notice also that the column in the second example has been defined as a NULLable column whereas in the first example it has not.</span></span> <span data-ttu-id="6178d-176">När tabellen har skapats i den första exemplet Nullbarheten definierades explicit.</span><span class="sxs-lookup"><span data-stu-id="6178d-176">When the table was created in the first example column nullability was explicitly defined.</span></span> <span data-ttu-id="6178d-177">I andra resulterar exempel meddelandet är bara kvar i uttrycket och som standard detta i en NULL-definition.</span><span class="sxs-lookup"><span data-stu-id="6178d-177">In the second example it was just left to the expression and by default this would result in a NULL definition.</span></span>  

<span data-ttu-id="6178d-178">För att lösa dessa problem måste anges explicit typkonvertering och Nullbarheten i den `SELECT` del av den `CTAS` instruktionen.</span><span class="sxs-lookup"><span data-stu-id="6178d-178">To resolve these issues you must explicitly set the type conversion and nullability in the `SELECT` portion of the `CTAS` statement.</span></span> <span data-ttu-id="6178d-179">Du kan inte ange dessa egenskaper i create table-delen.</span><span class="sxs-lookup"><span data-stu-id="6178d-179">You cannot set these properties in the create table part.</span></span>

<span data-ttu-id="6178d-180">Exemplet nedan visar hur du löser koden:</span><span class="sxs-lookup"><span data-stu-id="6178d-180">The example below demonstrates how to fix the code:</span></span>

```sql
DECLARE @d decimal(7,2) = 85.455
,       @f float(24)    = 85.455

CREATE TABLE ctas_r
WITH (DISTRIBUTION = ROUND_ROBIN)
AS
SELECT ISNULL(CAST(@d*@f AS DECIMAL(7,2)),0) as result
```

<span data-ttu-id="6178d-181">Observera följande:</span><span class="sxs-lookup"><span data-stu-id="6178d-181">Note the following:</span></span>

* <span data-ttu-id="6178d-182">CAST eller CONVERT kunde har använts</span><span class="sxs-lookup"><span data-stu-id="6178d-182">CAST or CONVERT could have been used</span></span>
* <span data-ttu-id="6178d-183">ISNULL används för att tvinga Nullbarheten inte COALESCE</span><span class="sxs-lookup"><span data-stu-id="6178d-183">ISNULL is used to force NULLability not COALESCE</span></span>
* <span data-ttu-id="6178d-184">ISNULL är funktionen yttersta</span><span class="sxs-lookup"><span data-stu-id="6178d-184">ISNULL is the outermost function</span></span>
* <span data-ttu-id="6178d-185">Den andra delen av ISNULL är en konstant d.v.s. 0</span><span class="sxs-lookup"><span data-stu-id="6178d-185">The second part of the ISNULL is a constant i.e. 0</span></span>

> [!NOTE]
> <span data-ttu-id="6178d-186">För Nullbarheten anges korrekt det är mycket viktigt att `ISNULL` och inte `COALESCE`.</span><span class="sxs-lookup"><span data-stu-id="6178d-186">For the nullability to be correctly set it is vital to use `ISNULL` and not `COALESCE`.</span></span> <span data-ttu-id="6178d-187">`COALESCE`är inte en deterministisk funktion så att resultatet av uttrycket kommer alltid att kan ha värdet null.</span><span class="sxs-lookup"><span data-stu-id="6178d-187">`COALESCE` is not a deterministic function and so the result of the expression will always be NULLable.</span></span> <span data-ttu-id="6178d-188">`ISNULL`är olika.</span><span class="sxs-lookup"><span data-stu-id="6178d-188">`ISNULL` is different.</span></span> <span data-ttu-id="6178d-189">Det är deterministisk.</span><span class="sxs-lookup"><span data-stu-id="6178d-189">It is deterministic.</span></span> <span data-ttu-id="6178d-190">Därför när den andra delen av den `ISNULL` funktionen är en konstant eller en litteral sedan resultatvärdet blir NOT NULL.</span><span class="sxs-lookup"><span data-stu-id="6178d-190">Therefore when the second part of the `ISNULL` function is a constant or a literal then the resulting value will be NOT NULL.</span></span>
> 
> 

<span data-ttu-id="6178d-191">Den här beskrivningen är inte bara användbar för att säkerställa integriteten hos dina beräkningar.</span><span class="sxs-lookup"><span data-stu-id="6178d-191">This tip is not just useful for ensuring the integrity of your calculations.</span></span> <span data-ttu-id="6178d-192">Det är också viktigt för växling av partition i tabellen.</span><span class="sxs-lookup"><span data-stu-id="6178d-192">It is also important for table partition switching.</span></span> <span data-ttu-id="6178d-193">Anta att du har den här tabellen definieras som din faktum:</span><span class="sxs-lookup"><span data-stu-id="6178d-193">Imagine you have this table defined as your fact:</span></span>

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

<span data-ttu-id="6178d-194">Värdefältet är dock ett beräknat uttryck inte är en del av datakällan.</span><span class="sxs-lookup"><span data-stu-id="6178d-194">However, the value field is a calculated expression it is not part of the source data.</span></span>

<span data-ttu-id="6178d-195">Om du vill skapa partitionerade datamängden kanske du vill göra detta:</span><span class="sxs-lookup"><span data-stu-id="6178d-195">To create your partitioned dataset you might want to do this:</span></span>

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

<span data-ttu-id="6178d-196">Frågan körs perfekt bra.</span><span class="sxs-lookup"><span data-stu-id="6178d-196">The query would run perfectly fine.</span></span> <span data-ttu-id="6178d-197">Problemet kommer när du försöker utföra partitionsväxeln.</span><span class="sxs-lookup"><span data-stu-id="6178d-197">The problem comes when you try to perform the partition switch.</span></span> <span data-ttu-id="6178d-198">Tabelldefinitionerna stämmer inte överens.</span><span class="sxs-lookup"><span data-stu-id="6178d-198">The table definitions do not match.</span></span> <span data-ttu-id="6178d-199">Om du vill göra tabelldefinitionerna matchar CTAS ska ändras.</span><span class="sxs-lookup"><span data-stu-id="6178d-199">To make the table definitions match the CTAS needs to be modified.</span></span>

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

<span data-ttu-id="6178d-200">Du kan se därför att typen konsekvens och underhålla Nullbarheten egenskaper för en CTAS är det engineering bästa praxis.</span><span class="sxs-lookup"><span data-stu-id="6178d-200">You can see therefore that type consistency and maintaining nullability properties on a CTAS is a good engineering best practice.</span></span> <span data-ttu-id="6178d-201">Det hjälper dig för att upprätthålla integriteten i beräkningarna och säkerställer också att växla partition är möjligt.</span><span class="sxs-lookup"><span data-stu-id="6178d-201">It helps to maintain integrity in your calculations and also ensures that partition switching is possible.</span></span>

<span data-ttu-id="6178d-202">Mer information om hur du använder finns i MSDN [CTAS][CTAS].</span><span class="sxs-lookup"><span data-stu-id="6178d-202">Please refer to MSDN for more information on using [CTAS][CTAS].</span></span> <span data-ttu-id="6178d-203">Det är en av de viktigaste rapporterna i Azure SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="6178d-203">It is one of the most important statements in Azure SQL Data Warehouse.</span></span> <span data-ttu-id="6178d-204">Kontrollera att du verkligen förstår.</span><span class="sxs-lookup"><span data-stu-id="6178d-204">Make sure you thoroughly understand it.</span></span>

## <a name="next-steps"></a><span data-ttu-id="6178d-205">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="6178d-205">Next steps</span></span>
<span data-ttu-id="6178d-206">För fler utvecklingstips, se [utvecklingsöversikt][development overview].</span><span class="sxs-lookup"><span data-stu-id="6178d-206">For more development tips, see [development overview][development overview].</span></span>

<!--Image references-->
[1]: media/sql-data-warehouse-develop-ctas/ctas-results.png

<!--Article references-->
[development overview]: sql-data-warehouse-overview-develop.md
[Statistics]: ./sql-data-warehouse-tables-statistics.md

<!--MSDN references-->
[CTAS]: https://msdn.microsoft.com/library/mt204041.aspx

<!--Other Web references-->
