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
# <a name="group-by-options-in-sql-data-warehouse"></a>Gruppera efter alternativ i SQL Data Warehouse
Hej [GROUP BY] [ GROUP BY] satsen används tooaggregate tooa sammanfattning datauppsättning av rader. Det finns även några alternativ och utökar dess funktioner som behöver toobe arbetat runt eftersom de inte direkt stöds av Azure SQL Data Warehouse.

Dessa alternativ är

* GROUP BY med Samlad
* GROUPING SETS
* GROUP BY med kub

## <a name="rollup-and-grouping-sets-options"></a>Anger alternativ för insamling och gruppering
hello enklaste alternativet här är toouse `UNION ALL` i stället tooperform hello samlad i stället för att förlita dig på hello explicit syntax. hello resultatet är exakt hello samma

Nedan visas ett exempel på ett group by-sats med hello `ROLLUP` alternativ:

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

Vi har begärt följande aggregeringar hello med hjälp av samlad:

* Land och Region
* Land/region
* Totalsumma

tooreplace detta behöver du toouse `UNION ALL`; ange hello aggregeringar krävs explicit tooreturn hello samma resultat:

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

GROUPING SETS är allt vi behöver toodo anta hello samma primära men bara skapa UNION ALL avsnitt för hello aggregering nivåer vill vi toosee

## <a name="cube-options"></a>Kubalternativ för
Det är möjligt toocreate en grupp av med kub med hello UNION ALL-metoden. hello problemet är att hello kod kan snabbt bli besvärlig och svårhanterliga. toomitigate detta kan du använda den mer avancerade metod.

Nu ska vi använda hello-exemplet ovan.

hello första steget är toodefine hello 'cube-som definierar alla hello nivåer av aggregation som vi vill toocreate. Det är viktigt tootake anteckna hello CROSS JOIN hello härledda tabellerna. Detta genererar alla hello nivåer för oss. hello resten av hello koden är verkligen det för formatering.

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

hello resultaten av hello CTAS visas nedan:

![][1]

hello andra steget är toospecify resulterar i en target tabell toostore interim:

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

hello tredje steget är tooloop över våra kub med kolumner som utför hello aggregering. hello frågan ska köras en gång för varje rad i hello #Cube tillfällig tabell och lagra hello resultat i hello #Results temporära tabellen

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

Till sist kan vi returnera hello resultat genom att bara läsa från hello #Results tillfällig tabell

```sql
SELECT *
FROM #Results
ORDER BY 1,2,3
;
```

Dela hello koden upp i avsnitt och generera en slinga konstruktion hello blir kod mer användarvänlig och hanterbar.

## <a name="next-steps"></a>Nästa steg
För fler utvecklingstips, se [utvecklingsöversikt][development overview].

<!--Image references-->
[1]: media/sql-data-warehouse-develop-group-by-options/sql-data-warehouse-develop-group-by-cube.png

<!--Article references-->
[development overview]: sql-data-warehouse-overview-develop.md

<!--MSDN references-->
[GROUP BY]: https://msdn.microsoft.com/library/ms177673.aspx


<!--Other Web references-->
