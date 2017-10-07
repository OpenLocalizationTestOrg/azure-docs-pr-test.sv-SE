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
# <a name="create-table-as-select-ctas-in-sql-data-warehouse"></a>Skapa en tabell som Select (CTAS) i SQL Data Warehouse
Skapa tabell som markerar eller `CTAS` är en av hello viktigaste T-SQL-funktioner tillgängliga. Det är en helt parallelized åtgärd som skapar en ny tabell baserat på hello utdata från en SELECT-instruktion. `CTAS`är hello enklaste och snabbaste sättet toocreate en kopia av en tabell. Det här dokumentet innehåller både exempel och bästa praxis för `CTAS`.

## <a name="selectinto-vs-ctas"></a>VÄLJ... I vs. CTAS
Du kan överväga att `CTAS` som en mycket debiteras version av `SELECT..INTO`.

Nedan visas ett exempel på en enkel `SELECT..INTO` instruktionen:

```sql
SELECT *
INTO    [dbo].[FactInternetSales_new]
FROM    [dbo].[FactInternetSales]
```

I ovanstående exempel hello `[dbo].[FactInternetSales_new]` skulle skapas som ROUND_ROBIN distribuerade tabell med ett GRUPPERAT COLUMNSTORE-INDEX på den som dessa hello tabell standardvärden i Azure SQL Data Warehouse.

`SELECT..INTO`dock kan du inte toochange skriver du antingen hello distribution metoden eller hello index som en del av hello igen. Det är där `CTAS` kommer in.

tooconvert hello ovan för`CTAS` är ganska enkel:

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

Med `CTAS` du är kan toochange både hello distribution av hello tabelldata samt hello tabelltyp. 

> [!NOTE]
> Om du försöker bara toochange hello index i din `CTAS` åtgärden och hello källtabellen är hash distribueras sedan din `CTAS` åtgärden utförs bäst om du underhåller hello samma kolumn- och distributionstypen. På så sätt undviker mellan distribution dataflyttning under hello åtgärden, vilket är mer effektivt.
> 
> 

## <a name="using-ctas-toocopy-a-table"></a>Med hjälp av CTAS toocopy en tabell
Kanske en av de vanligaste hello används av `CTAS` skapar en kopia av en tabell, så att du kan ändra hello DDL. Om till exempel din tabell som ursprungligen skapades `ROUND_ROBIN` och nu vill ändra den tooa tabellen distribueras på en kolumn, `CTAS` är hur du ändrar hello distribution kolumn. `CTAS`måste använda toochange partitionering, indexering eller kolumn typer.

Anta att du har skapat den här tabellen med hello standard fördelningstyp `ROUND_ROBIN` distribueras eftersom ingen distribution kolumn har angetts i hello `CREATE TABLE`.

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

Nu ska toocreate en ny kopia av den här tabellen med ett grupperat Columnstore-Index så att du kan dra nytta av hello prestanda för grupperade Columnstore-tabeller. Du bör också toodistribute tabellen på ProductKey eftersom du förutseende kopplingar på den här kolumnen och vill tooavoid dataflyttning under kopplingar på ProductKey. Slutligen bör du också tooadd partitionering på OrderDateKey så att du snabbt kan ta bort gamla data genom att släppa gamla partitioner. Här är hello CTAS-instruktionen som kopierar din gamla tabell till en ny tabell.

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

Slutligen kan du byta namn på tabeller-tooswap i den nya tabellen och ta bort din gamla registret.

```sql
RENAME OBJECT FactInternetSales tooFactInternetSales_old;
RENAME OBJECT FactInternetSales_new tooFactInternetSales;

DROP TABLE FactInternetSales_old;
```

> [!NOTE]
> SQL Data Warehouse stöder inte än automatiskt skapande eller uppdatering av statistik.  I ordning tooget hello bästa prestanda från dina frågor, är det viktigt att statistik skapas på alla kolumner i alla tabeller efter hello första inläsningen eller vid betydande dataändringar i hello data.  En detaljerad förklaring av statistik finns hello [statistik] [ Statistics] -avsnittet i hello ämnesgruppen.
> 
> 

## <a name="using-ctas-toowork-around-unsupported-features"></a>Med hjälp av CTAS toowork runt funktioner som inte stöds
`CTAS`också kan vara används toowork kring ett antal hello stöds inte funktionerna i listan nedan. Denna funktion kan ofta vara toobe win/win situation som inte bara din kod blir kompatibla men det ofta körs snabbare i SQL Data Warehouse. Detta är på grund av dess fullständigt parallelized design. Scenarier som fungerade runt med CTAS är:

* ANSI kopplingar uppdateringar
* ANSI-kopplingar på borttagningar
* MERGE-instruktion

> [!NOTE]
> Försök toothink ”CTAS första”. Om du tror att du kan lösa ett problem med `CTAS` hello bästa sätt tooapproach är vanligtvis den – även om du skriver mer data som ett resultat.
> 
> 

## <a name="ansi-join-replacement-for-update-statements"></a>ANSI join ersättning för update-instruktioner
Du kanske du har en komplex uppdatering som ansluter till fler än två tabeller med ANSI koppla syntax tooperform hello uppdatera eller ta bort.

Anta att du haft tooupdate den här tabellen:

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

hello ursprungliga frågan kanske har såg ut ungefär så här:

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

Eftersom SQL Data Warehouse inte stöder ANSI kopplingar i hello `FROM` -satsen i en `UPDATE` -instruktionen, du kan inte kopiera den här koden över utan att ändra något.

Du kan använda en kombination av en `CTAS` och en implicit ansluta tooreplace koden:

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

## <a name="ansi-join-replacement-for-delete-statements"></a>ANSI join ersättning för delete-instruktioner
Ibland hello bästa metoden för att ta bort data är toouse `CTAS`. Välj hello data som du vill använda tookeep i stället för att ta bort hello data bara. Den här särskilt för `DELETE` som använder ansi koppla syntax eftersom SQL Data Warehouse inte stöder ANSI-kopplingar i hello `FROM` -satsen i en `DELETE` instruktion.

Ett exempel på en konverterade DELETE-instruktion finns nedan:

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

## <a name="replace-merge-statements"></a>Ersätt merge-instruktioner
Merge-instruktioner kan ersättas, minst del, med hjälp av `CTAS`. Du kan konsolidera hello `INSERT` och hello `UPDATE` i en enskild instruktion. Alla borttagna poster måste toobe stängs av i andra instruktionen.

Ett exempel på en `UPSERT` finns nedan:

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

## <a name="ctas-recommendation-explicitly-state-data-type-and-nullability-of-output"></a>CTAS rekommendation: uttryckligen ange datatyp och Nullbarheten för utdata
När du migrerar kod kanske körs i den här typen av kodning mönster:

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

Du tror instinktivt migrerar du den här koden tooa CTAS och du skulle vara korrekt. Det finns emellertid ett dolt problem.

hello följande kod ger inte hello samma resultat:

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

Observera att hello kolumnen ”resultat” vidarebefordra innebär hello typ och Nullbarheten datavärden för hello uttryck. Detta kan leda toosubtle avvikelser i värden om du inte är försiktig.

Försök hello följande exempel:

```sql
SELECT result,result*@d
from result
;

SELECT result,result*@d
from ctas_r
;
```

hello-värde som lagras i resultatet är olika. Hello beständiga värdet i hello resultatkolumnen används i andra blir uttryck hello fel ännu större.

![][1]

Detta är särskilt viktigt för migrering av data. Även om hello andra frågan tvekan exaktare finns problemet. hello data skulle vara olika jämfört med toohello källsystemet och som leder tooquestions för i hello migrering. Detta är en av de sällsynta fall där hello ”fel” svaret är faktiskt hello höger en!

hello orsak ser vi den här skillnader mellan hello två resultat är nere tooimplicit skriver omvandling. I hello definierar första exempeltabell hello hello kolumndefinitionen. En implicit typkonvertering inträffar när hello rad infogas. I andra hello-exemplet finns det ingen implicit typkonvertering som hello uttrycket definierar datatypen för kolumnen hello. Observera också hello kolumnen i hello andra exemplet har definierats som en nullbar kolumn i hello första exemplet har den inte. När hello tabell skapades i definierades uttryckligen hello första exempel Nullbarhet. I andra hello-exemplet lämnades bara toohello uttryck och som standard det skulle resultera i en NULL-definition.  

tooresolve dessa problem som du måste uttryckligen ange hello typkonvertering och Nullbarheten i hello `SELECT` del av hello `CTAS` instruktionen. Du kan inte ange dessa egenskaper i hello Skapa tabell del.

hello exemplet nedan demonstrerar hur toofix hello kod:

```sql
DECLARE @d decimal(7,2) = 85.455
,       @f float(24)    = 85.455

CREATE TABLE ctas_r
WITH (DISTRIBUTION = ROUND_ROBIN)
AS
SELECT ISNULL(CAST(@d*@f AS DECIMAL(7,2)),0) as result
```

Observera följande hello:

* CAST eller CONVERT kunde har använts
* ISNULL är används tooforce Nullbarheten inte COALESCE
* ISNULL är hello yttersta funktion
* hello andra delen av hello ISNULL är en konstant d.v.s. 0

> [!NOTE]
> Hello Nullbarheten toobe inställd på rätt sätt är avgörande toouse `ISNULL` och inte `COALESCE`. `COALESCE`är inte en deterministisk funktion och kan därför hello resultatet av hello uttryck alltid tas kan ha värdet null. `ISNULL`är olika. Det är deterministisk. Därför hello när andra delen av hello `ISNULL` funktionen är en konstant eller en litteral sedan hello resultatvärdet blir NOT NULL.
> 
> 

Den här beskrivningen är inte bara användbar för att säkerställa hello integriteten hos dina beräkningar. Det är också viktigt för växling av partition i tabellen. Anta att du har den här tabellen definieras som din faktum:

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

Hello värdefältet är dock ett beräknat uttryck inte är en del av hello källdata.

toocreate partitionerade datamängden du kanske vill toodo detta:

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

hello frågan körs perfekt bra. hello problemet kommer när du försöker tooperform hello partition växel. Hej tabelldefinitionerna stämmer inte överens. toomake hello definitioner matchar hello CTAS måste toobe ändras.

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

Du kan se därför att typen konsekvens och underhålla Nullbarheten egenskaper för en CTAS är det engineering bästa praxis. Den hjälper toomaintain integritet i beräkningarna och säkerställer också att växla partition är möjligt.

Mer information om hur du använder finns tooMSDN [CTAS][CTAS]. Det är en av de viktigaste hello-instruktioner i Azure SQL Data Warehouse. Kontrollera att du verkligen förstår.

## <a name="next-steps"></a>Nästa steg
För fler utvecklingstips, se [utvecklingsöversikt][development overview].

<!--Image references-->
[1]: media/sql-data-warehouse-develop-ctas/ctas-results.png

<!--Article references-->
[development overview]: sql-data-warehouse-overview-develop.md
[Statistics]: ./sql-data-warehouse-tables-statistics.md

<!--MSDN references-->
[CTAS]: https://msdn.microsoft.com/library/mt204041.aspx

<!--Other Web references-->
