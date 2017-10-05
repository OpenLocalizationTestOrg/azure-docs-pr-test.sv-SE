---
title: Gruppera efter alternativ i SQL Data Warehouse | Microsoft Docs
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
ms.openlocfilehash: da71cb834c13da5d0f5690f471efc6c696163f30
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="group-by-options-in-sql-data-warehouse"></a><span data-ttu-id="97840-103">Gruppera efter alternativ i SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="97840-103">Group by options in SQL Data Warehouse</span></span>
<span data-ttu-id="97840-104">Den [GROUP BY] [ GROUP BY] satsen används för att samla in data till en sammanfattande uppsättning rader.</span><span class="sxs-lookup"><span data-stu-id="97840-104">The [GROUP BY][GROUP BY] clause is used to aggregate data to a summary set of rows.</span></span> <span data-ttu-id="97840-105">Det finns även några alternativ utökar dess funktioner som behöver bearbetas runt eftersom de inte direkt stöds av Azure SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="97840-105">It also has a few options that extend it's functionality that need to be worked around as they are not directly supported by Azure SQL Data Warehouse.</span></span>

<span data-ttu-id="97840-106">Dessa alternativ är</span><span class="sxs-lookup"><span data-stu-id="97840-106">These options are</span></span>

* <span data-ttu-id="97840-107">GROUP BY med Samlad</span><span class="sxs-lookup"><span data-stu-id="97840-107">GROUP BY with ROLLUP</span></span>
* <span data-ttu-id="97840-108">GROUPING SETS</span><span class="sxs-lookup"><span data-stu-id="97840-108">GROUPING SETS</span></span>
* <span data-ttu-id="97840-109">GROUP BY med kub</span><span class="sxs-lookup"><span data-stu-id="97840-109">GROUP BY with CUBE</span></span>

## <a name="rollup-and-grouping-sets-options"></a><span data-ttu-id="97840-110">Anger alternativ för insamling och gruppering</span><span class="sxs-lookup"><span data-stu-id="97840-110">Rollup and grouping sets options</span></span>
<span data-ttu-id="97840-111">Det enklaste alternativet är att använda `UNION ALL` i stället för samlade i stället för att förlita dig på explicit syntax.</span><span class="sxs-lookup"><span data-stu-id="97840-111">The simplest option here is to use `UNION ALL` instead to perform the rollup rather than relying on the explicit syntax.</span></span> <span data-ttu-id="97840-112">Resultatet är exakt samma</span><span class="sxs-lookup"><span data-stu-id="97840-112">The result is exactly the same</span></span>

<span data-ttu-id="97840-113">Nedan visas ett exempel på en grupp med hjälp av instruktionen den `ROLLUP` alternativ:</span><span class="sxs-lookup"><span data-stu-id="97840-113">Below is an example of a group by statement using the `ROLLUP` option:</span></span>

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

<span data-ttu-id="97840-114">Vi har begärt följande aggregeringar med hjälp av samlad:</span><span class="sxs-lookup"><span data-stu-id="97840-114">By using ROLLUP we have requested the following aggregations:</span></span>

* <span data-ttu-id="97840-115">Land och Region</span><span class="sxs-lookup"><span data-stu-id="97840-115">Country and Region</span></span>
* <span data-ttu-id="97840-116">Land/region</span><span class="sxs-lookup"><span data-stu-id="97840-116">Country</span></span>
* <span data-ttu-id="97840-117">Totalsumma</span><span class="sxs-lookup"><span data-stu-id="97840-117">Grand Total</span></span>

<span data-ttu-id="97840-118">Ersätt detta behöver du använda `UNION ALL`; ange aggregeringar måste uttryckligen returnera samma resultat:</span><span class="sxs-lookup"><span data-stu-id="97840-118">To replace this you will need to use `UNION ALL`; specifying the aggregations required explicitly to return the same results:</span></span>

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

<span data-ttu-id="97840-119">För GROUPING SETS alla vi behöver göra är antar samma huvudkonto men bara skapa UNION ALL avsnitt för aggregering nivåerna vi vill se</span><span class="sxs-lookup"><span data-stu-id="97840-119">For GROUPING SETS all we need to do is adopt the same principal but only create UNION ALL sections for the aggregation levels we want to see</span></span>

## <a name="cube-options"></a><span data-ttu-id="97840-120">Kubalternativ för</span><span class="sxs-lookup"><span data-stu-id="97840-120">Cube options</span></span>
<span data-ttu-id="97840-121">Det är möjligt att skapa en grupp av med kub med hjälp av UNION ALL-metoden.</span><span class="sxs-lookup"><span data-stu-id="97840-121">It is possible to create a GROUP BY WITH CUBE using the UNION ALL approach.</span></span> <span data-ttu-id="97840-122">Problemet är att koden kan snabbt bli besvärlig och svårhanterliga.</span><span class="sxs-lookup"><span data-stu-id="97840-122">The problem is that the code can quickly become cumbersome and unwieldy.</span></span> <span data-ttu-id="97840-123">Om du vill undvika detta kan du använda den mer avancerade metod.</span><span class="sxs-lookup"><span data-stu-id="97840-123">To mitigate this you can use this more advanced approach.</span></span>

<span data-ttu-id="97840-124">Nu ska vi använda exemplet ovan.</span><span class="sxs-lookup"><span data-stu-id="97840-124">Let's use the example above.</span></span>

<span data-ttu-id="97840-125">Det första steget är att definiera 'kuben' som definierar alla nivåer av aggregation som vi vill skapa.</span><span class="sxs-lookup"><span data-stu-id="97840-125">The first step is to define the 'cube' that defines all the levels of aggregation that we want to create.</span></span> <span data-ttu-id="97840-126">Det är viktigt att notera CROSS JOIN av två härledda tabeller.</span><span class="sxs-lookup"><span data-stu-id="97840-126">It is important to take note of the CROSS JOIN of the two derived tables.</span></span> <span data-ttu-id="97840-127">Detta genererar alla nivåer för oss.</span><span class="sxs-lookup"><span data-stu-id="97840-127">This generates all the levels for us.</span></span> <span data-ttu-id="97840-128">Resten av koden är verkligen det för formatering.</span><span class="sxs-lookup"><span data-stu-id="97840-128">The rest of the code is really there for formatting.</span></span>

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

<span data-ttu-id="97840-129">Resultaten av CTAS visas nedan:</span><span class="sxs-lookup"><span data-stu-id="97840-129">The results of the CTAS can be seen below:</span></span>

![][1]

<span data-ttu-id="97840-130">Det andra steget är att ange en måltabell för att lagra tillfälliga resultat:</span><span class="sxs-lookup"><span data-stu-id="97840-130">The second step is to specify a target table to store interim results:</span></span>

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

<span data-ttu-id="97840-131">Det tredje steget är att köras i slinga över våra kub med kolumner som utför aggregering.</span><span class="sxs-lookup"><span data-stu-id="97840-131">The third step is to loop over our cube of columns performing the aggregation.</span></span> <span data-ttu-id="97840-132">Frågan ska köras en gång för varje rad i den temporära tabellen #Cube och spara resultatet i den temporära tabellen #Results</span><span class="sxs-lookup"><span data-stu-id="97840-132">The query will run once for every row in the #Cube temporary table and store the results in the #Results temp table</span></span>

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

<span data-ttu-id="97840-133">Till sist kan vi returnera resultat genom att bara läsa från den temporära tabellen #Results</span><span class="sxs-lookup"><span data-stu-id="97840-133">Lastly we can return the results by simply reading from the #Results temporary table</span></span>

```sql
SELECT *
FROM #Results
ORDER BY 1,2,3
;
```

<span data-ttu-id="97840-134">Genom att dela koden upp i avsnitt och genererar en slinga konstruktion blir koden mer användarvänlig och hanterbar.</span><span class="sxs-lookup"><span data-stu-id="97840-134">By breaking the code up into sections and generating a looping construct the code becomes more manageable and maintainable.</span></span>

## <a name="next-steps"></a><span data-ttu-id="97840-135">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="97840-135">Next steps</span></span>
<span data-ttu-id="97840-136">För fler utvecklingstips, se [utvecklingsöversikt][development overview].</span><span class="sxs-lookup"><span data-stu-id="97840-136">For more development tips, see [development overview][development overview].</span></span>

<!--Image references-->
[1]: media/sql-data-warehouse-develop-group-by-options/sql-data-warehouse-develop-group-by-cube.png

<!--Article references-->
[development overview]: sql-data-warehouse-overview-develop.md

<!--MSDN references-->
[GROUP BY]: https://msdn.microsoft.com/library/ms177673.aspx


<!--Other Web references-->
