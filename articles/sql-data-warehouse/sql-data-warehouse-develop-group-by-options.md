---
title: aaaGroup alternativ i SQL Data Warehouse | Microsoft Docs
description: "Tips för gruppen av alternativen i Azure SQL Data Warehouse för utveckling av lösningar."
services: sql-data-warehouse
documentationcenter: NA
author: jrowlandjones
manager: jhubbard
editor: 
ms.assetid: f95a1e43-768f-4b7b-8a10-8a0509d0c871
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: queries
ms.date: 10/31/2016
ms.author: jrj;barbkess
ms.openlocfilehash: cc443c2af4e3ef2babd74d78aa6fb57bb3c1c7ea
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="group-by-options-in-sql-data-warehouse"></a><span data-ttu-id="9caf8-103">Gruppera efter alternativ i SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="9caf8-103">Group by options in SQL Data Warehouse</span></span>
<span data-ttu-id="9caf8-104">Hej [GROUP BY] [ GROUP BY] satsen används tooaggregate tooa sammanfattning datauppsättning av rader.</span><span class="sxs-lookup"><span data-stu-id="9caf8-104">hello [GROUP BY][GROUP BY] clause is used tooaggregate data tooa summary set of rows.</span></span> <span data-ttu-id="9caf8-105">Det finns även några alternativ och utökar dess funktioner som behöver toobe arbetat runt eftersom de inte direkt stöds av Azure SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="9caf8-105">It also has a few options that extend it's functionality that need toobe worked around as they are not directly supported by Azure SQL Data Warehouse.</span></span>

<span data-ttu-id="9caf8-106">Dessa alternativ är</span><span class="sxs-lookup"><span data-stu-id="9caf8-106">These options are</span></span>

* <span data-ttu-id="9caf8-107">GROUP BY med Samlad</span><span class="sxs-lookup"><span data-stu-id="9caf8-107">GROUP BY with ROLLUP</span></span>
* <span data-ttu-id="9caf8-108">GROUPING SETS</span><span class="sxs-lookup"><span data-stu-id="9caf8-108">GROUPING SETS</span></span>
* <span data-ttu-id="9caf8-109">GROUP BY med kub</span><span class="sxs-lookup"><span data-stu-id="9caf8-109">GROUP BY with CUBE</span></span>

## <a name="rollup-and-grouping-sets-options"></a><span data-ttu-id="9caf8-110">Anger alternativ för insamling och gruppering</span><span class="sxs-lookup"><span data-stu-id="9caf8-110">Rollup and grouping sets options</span></span>
<span data-ttu-id="9caf8-111">hello enklaste alternativet här är toouse `UNION ALL` i stället tooperform hello samlad i stället för att förlita dig på hello explicit syntax.</span><span class="sxs-lookup"><span data-stu-id="9caf8-111">hello simplest option here is toouse `UNION ALL` instead tooperform hello rollup rather than relying on hello explicit syntax.</span></span> <span data-ttu-id="9caf8-112">hello resultatet är exakt hello samma</span><span class="sxs-lookup"><span data-stu-id="9caf8-112">hello result is exactly hello same</span></span>

<span data-ttu-id="9caf8-113">Nedan visas ett exempel på ett group by-sats med hello `ROLLUP` alternativ:</span><span class="sxs-lookup"><span data-stu-id="9caf8-113">Below is an example of a group by statement using hello `ROLLUP` option:</span></span>

```sql
SELECT [SalesTerritoryCountry]
,      [SalesTerritoryRegion]
,      SUM(SalesAmount)             AS TotalSalesAmount
FROM  dbo.factInternetSales s
JOIN  dbo.DimSalesTerritory t       ON s.SalesTerritoryKey       = t.SalesTerritoryKey
GROUP BY ROLLUP (
                        [SalesTerritoryCountry]
                ,       [SalesTerritoryRegion]
                )
;
```

<span data-ttu-id="9caf8-114">Vi har begärt följande aggregeringar hello med hjälp av samlad:</span><span class="sxs-lookup"><span data-stu-id="9caf8-114">By using ROLLUP we have requested hello following aggregations:</span></span>

* <span data-ttu-id="9caf8-115">Land och Region</span><span class="sxs-lookup"><span data-stu-id="9caf8-115">Country and Region</span></span>
* <span data-ttu-id="9caf8-116">Land/region</span><span class="sxs-lookup"><span data-stu-id="9caf8-116">Country</span></span>
* <span data-ttu-id="9caf8-117">Totalsumma</span><span class="sxs-lookup"><span data-stu-id="9caf8-117">Grand Total</span></span>

<span data-ttu-id="9caf8-118">tooreplace detta behöver du toouse `UNION ALL`; ange hello aggregeringar krävs explicit tooreturn hello samma resultat:</span><span class="sxs-lookup"><span data-stu-id="9caf8-118">tooreplace this you will need toouse `UNION ALL`; specifying hello aggregations required explicitly tooreturn hello same results:</span></span>

```sql
SELECT [SalesTerritoryCountry]
,      [SalesTerritoryRegion]
,      SUM(SalesAmount) AS TotalSalesAmount
FROM  dbo.factInternetSales s
JOIN  dbo.DimSalesTerritory t     ON s.SalesTerritoryKey       = t.SalesTerritoryKey
GROUP BY
       [SalesTerritoryCountry]
,      [SalesTerritoryRegion]
UNION ALL
SELECT [SalesTerritoryCountry]
,      NULL
,      SUM(SalesAmount) AS TotalSalesAmount
FROM  dbo.factInternetSales s
JOIN  dbo.DimSalesTerritory t     ON s.SalesTerritoryKey       = t.SalesTerritoryKey
GROUP BY
       [SalesTerritoryCountry]
UNION ALL
SELECT NULL
,      NULL
,      SUM(SalesAmount) AS TotalSalesAmount
FROM  dbo.factInternetSales s
JOIN  dbo.DimSalesTerritory t     ON s.SalesTerritoryKey       = t.SalesTerritoryKey;
```

<span data-ttu-id="9caf8-119">GROUPING SETS är allt vi behöver toodo anta hello samma primära men bara skapa UNION ALL avsnitt för hello aggregering nivåer vill vi toosee</span><span class="sxs-lookup"><span data-stu-id="9caf8-119">For GROUPING SETS all we need toodo is adopt hello same principal but only create UNION ALL sections for hello aggregation levels we want toosee</span></span>

## <a name="cube-options"></a><span data-ttu-id="9caf8-120">Kubalternativ för</span><span class="sxs-lookup"><span data-stu-id="9caf8-120">Cube options</span></span>
<span data-ttu-id="9caf8-121">Det är möjligt toocreate en grupp av med kub med hello UNION ALL-metoden.</span><span class="sxs-lookup"><span data-stu-id="9caf8-121">It is possible toocreate a GROUP BY WITH CUBE using hello UNION ALL approach.</span></span> <span data-ttu-id="9caf8-122">hello problemet är att hello kod kan snabbt bli besvärlig och svårhanterliga.</span><span class="sxs-lookup"><span data-stu-id="9caf8-122">hello problem is that hello code can quickly become cumbersome and unwieldy.</span></span> <span data-ttu-id="9caf8-123">toomitigate detta kan du använda den mer avancerade metod.</span><span class="sxs-lookup"><span data-stu-id="9caf8-123">toomitigate this you can use this more advanced approach.</span></span>

<span data-ttu-id="9caf8-124">Nu ska vi använda hello-exemplet ovan.</span><span class="sxs-lookup"><span data-stu-id="9caf8-124">Let's use hello example above.</span></span>

<span data-ttu-id="9caf8-125">hello första steget är toodefine hello 'cube-som definierar alla hello nivåer av aggregation som vi vill toocreate.</span><span class="sxs-lookup"><span data-stu-id="9caf8-125">hello first step is toodefine hello 'cube' that defines all hello levels of aggregation that we want toocreate.</span></span> <span data-ttu-id="9caf8-126">Det är viktigt tootake anteckna hello CROSS JOIN hello härledda tabellerna.</span><span class="sxs-lookup"><span data-stu-id="9caf8-126">It is important tootake note of hello CROSS JOIN of hello two derived tables.</span></span> <span data-ttu-id="9caf8-127">Detta genererar alla hello nivåer för oss.</span><span class="sxs-lookup"><span data-stu-id="9caf8-127">This generates all hello levels for us.</span></span> <span data-ttu-id="9caf8-128">hello resten av hello koden är verkligen det för formatering.</span><span class="sxs-lookup"><span data-stu-id="9caf8-128">hello rest of hello code is really there for formatting.</span></span>

```sql
CREATE TABLE #Cube
WITH
(   DISTRIBUTION = ROUND_ROBIN
,   LOCATION = USER_DB
)
AS
WITH GrpCube AS
(SELECT    CAST(ISNULL(Country,'NULL')+','+ISNULL(Region,'NULL') AS NVARCHAR(50)) as 'Cols'
,          CAST(ISNULL(Country+',','')+ISNULL(Region,'') AS NVARCHAR(50))  as 'GroupBy'
,          ROW_NUMBER() OVER (ORDER BY Country) as 'Seq'
FROM       ( SELECT 'SalesTerritoryCountry' as Country
             UNION ALL
             SELECT NULL
           ) c
CROSS JOIN ( SELECT 'SalesTerritoryRegion' as Region
             UNION ALL
             SELECT NULL
           ) r
)
SELECT Cols
,      CASE WHEN SUBSTRING(GroupBy,LEN(GroupBy),1) = ','
            THEN SUBSTRING(GroupBy,1,LEN(GroupBy)-1)
            ELSE GroupBy
       END AS GroupBy  --Remove Trailing Comma
,Seq
FROM GrpCube;
```

<span data-ttu-id="9caf8-129">hello resultaten av hello CTAS visas nedan:</span><span class="sxs-lookup"><span data-stu-id="9caf8-129">hello results of hello CTAS can be seen below:</span></span>

![][1]

<span data-ttu-id="9caf8-130">hello andra steget är toospecify resulterar i en target tabell toostore interim:</span><span class="sxs-lookup"><span data-stu-id="9caf8-130">hello second step is toospecify a target table toostore interim results:</span></span>

```sql
DECLARE
 @SQL NVARCHAR(4000)
,@Columns NVARCHAR(4000)
,@GroupBy NVARCHAR(4000)
,@i INT = 1
,@nbr INT = 0
;
CREATE TABLE #Results
(
 [SalesTerritoryCountry] NVARCHAR(50)
,[SalesTerritoryRegion]  NVARCHAR(50)
,[TotalSalesAmount]      MONEY
)
WITH
(   DISTRIBUTION = ROUND_ROBIN
,   LOCATION = USER_DB
)
;
```

<span data-ttu-id="9caf8-131">hello tredje steget är tooloop över våra kub med kolumner som utför hello aggregering.</span><span class="sxs-lookup"><span data-stu-id="9caf8-131">hello third step is tooloop over our cube of columns performing hello aggregation.</span></span> <span data-ttu-id="9caf8-132">hello frågan ska köras en gång för varje rad i hello #Cube tillfällig tabell och lagra hello resultat i hello #Results temporära tabellen</span><span class="sxs-lookup"><span data-stu-id="9caf8-132">hello query will run once for every row in hello #Cube temporary table and store hello results in hello #Results temp table</span></span>

```sql
SET @nbr =(SELECT MAX(Seq) FROM #Cube);

WHILE @i<=@nbr
BEGIN
    SET @Columns = (SELECT Cols    FROM #Cube where seq = @i);
    SET @GroupBy = (SELECT GroupBy FROM #Cube where seq = @i);

    SET @SQL ='INSERT INTO #Results
              SELECT '+@Columns+'
              ,      SUM(SalesAmount) AS TotalSalesAmount
              FROM  dbo.factInternetSales s
              JOIN  dbo.DimSalesTerritory t  
              ON s.SalesTerritoryKey = t.SalesTerritoryKey
              '+CASE WHEN @GroupBy <>''
                     THEN 'GROUP BY '+@GroupBy ELSE '' END

    EXEC sp_executesql @SQL;
    SET @i +=1;
END
```

<span data-ttu-id="9caf8-133">Till sist kan vi returnera hello resultat genom att bara läsa från hello #Results tillfällig tabell</span><span class="sxs-lookup"><span data-stu-id="9caf8-133">Lastly we can return hello results by simply reading from hello #Results temporary table</span></span>

```sql
SELECT *
FROM #Results
ORDER BY 1,2,3
;
```

<span data-ttu-id="9caf8-134">Dela hello koden upp i avsnitt och generera en slinga konstruktion hello blir kod mer användarvänlig och hanterbar.</span><span class="sxs-lookup"><span data-stu-id="9caf8-134">By breaking hello code up into sections and generating a looping construct hello code becomes more manageable and maintainable.</span></span>

## <a name="next-steps"></a><span data-ttu-id="9caf8-135">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="9caf8-135">Next steps</span></span>
<span data-ttu-id="9caf8-136">För fler utvecklingstips, se [utvecklingsöversikt][development overview].</span><span class="sxs-lookup"><span data-stu-id="9caf8-136">For more development tips, see [development overview][development overview].</span></span>

<!--Image references-->
[1]: media/sql-data-warehouse-develop-group-by-options/sql-data-warehouse-develop-group-by-cube.png

<!--Article references-->
[development overview]: sql-data-warehouse-overview-develop.md

<!--MSDN references-->
[GROUP BY]: https://msdn.microsoft.com/library/ms177673.aspx


<!--Other Web references-->
