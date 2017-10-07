---
title: "aaaLeverage T-SQL körs i en loop i Azure SQL Data Warehouse | Microsoft Docs"
description: "Tips för Transact-SQL-slingor och ersätta markörer i Azure SQL Data Warehouse för utveckling av lösningar."
services: sql-data-warehouse
documentationcenter: NA
author: jrowlandjones
manager: jhubbard
editor: 
ms.assetid: f3384b81-b943-431b-bc73-90e47e4c195f
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: t-sql
ms.date: 10/31/2016
ms.author: jrj;barbkess
ms.openlocfilehash: c7e8f71b910d00d0dfc30f6e5eba190fd05014b3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="loops-in-sql-data-warehouse"></a>Slingor i SQL Data Warehouse
SQL Data Warehouse stöder hello [medan][medan] loop för att köra instruktionen block upprepade gånger. Detta fortsätter för så länge hello angivna villkor är uppfyllda eller tills hello kod specifikt avslutar hello loop med hello `BREAK` nyckelord. Slingor är särskilt användbara för att ersätta markörer som definierats i SQL-kod. Lyckligtvis läses nästan alla markörer som har skrivits i SQL-kod för snabb hello framåt, endast olika. Därför [medan] slingor är ett bra alternativ om du märker att tooreplace en.

## <a name="leveraging-loops-and-replacing-cursors-in-sql-data-warehouse"></a>Utnyttja slingor och ersätta markörer i SQL Data Warehouse
Men innan du dyker i huvudet först bör du fråga dig själv hello följande fråga: ”kan den här pekaren igen skriftligt toouse set baseras operations”?. I många fall hello svaret blir Ja och är ofta hello bäst. En uppsättning baserad åtgärd ofta utför mycket snabbare än en iterativ, rad för rad-metod.

Snabba framåtriktade skrivskyddade markörer kan enkelt ersättas med en slinga konstruktion. Nedan visas ett enkelt exempel. Det här kodexemplet uppdaterar hello statistik för varje tabell i hello-databasen. Genom att gå över hello tabeller i hello loop vi är kan tooexecute varje kommando i följd.

Börja med att skapa en tillfällig tabell som innehåller en unik rad nummer används tooidentify hello enskilda uttryck:

```
CREATE TABLE #tbl
WITH
( DISTRIBUTION = ROUND_ROBIN
)
AS
SELECT  ROW_NUMBER() OVER(ORDER BY (SELECT NULL)) AS Sequence
,       [name]
,       'UPDATE STATISTICS '+QUOTENAME([name]) AS sql_code
FROM    sys.tables
;
```

Andra, initiera hello variabler krävs tooperform hello slinga:

```
DECLARE @nbr_statements INT = (SELECT COUNT(*) FROM #tbl)
,       @i INT = 1
;
```

Nu slinga över instruktioner som körs på en i taget:

```
WHILE   @i <= @nbr_statements
BEGIN
    DECLARE @sql_code NVARCHAR(4000) = (SELECT sql_code FROM #tbl WHERE Sequence = @i);
    EXEC    sp_executesql @sql_code;
    SET     @i +=1;
END
```

Slutligen släppa hello temporära tabellen skapas i första steget i hello

```
DROP TABLE #tbl;
```


<!--Every topic should have next steps and links toohello next logical set of content tookeep hello customer engaged-->

## <a name="next-steps"></a>Nästa steg
För fler utvecklingstips, se [utvecklingsöversikt][development overview].

<!--Image references-->

<!--Article references-->
[development overview]: sql-data-warehouse-overview-develop.md

<!--MSDN references-->
[medan]: https://msdn.microsoft.com/library/ms178642.aspx


<!--Other Web references-->
