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
# <a name="loops-in-sql-data-warehouse"></a><span data-ttu-id="2bbf1-103">Slingor i SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="2bbf1-103">Loops in SQL Data Warehouse</span></span>
<span data-ttu-id="2bbf1-104">SQL Data Warehouse stöder hello [medan][medan] loop för att köra instruktionen block upprepade gånger.</span><span class="sxs-lookup"><span data-stu-id="2bbf1-104">SQL Data Warehouse supports hello [WHILE][WHILE] loop for repeatedly executing statement blocks.</span></span> <span data-ttu-id="2bbf1-105">Detta fortsätter för så länge hello angivna villkor är uppfyllda eller tills hello kod specifikt avslutar hello loop med hello `BREAK` nyckelord.</span><span class="sxs-lookup"><span data-stu-id="2bbf1-105">This will continue for as long as hello specified conditions are true or until hello code specifically terminates hello loop using hello `BREAK` keyword.</span></span> <span data-ttu-id="2bbf1-106">Slingor är särskilt användbara för att ersätta markörer som definierats i SQL-kod.</span><span class="sxs-lookup"><span data-stu-id="2bbf1-106">Loops are particularly useful for replacing cursors defined in SQL code.</span></span> <span data-ttu-id="2bbf1-107">Lyckligtvis läses nästan alla markörer som har skrivits i SQL-kod för snabb hello framåt, endast olika.</span><span class="sxs-lookup"><span data-stu-id="2bbf1-107">Fortunately, almost all cursors that are written in SQL code are of hello fast forward, read only variety.</span></span> <span data-ttu-id="2bbf1-108">Därför [medan] slingor är ett bra alternativ om du märker att tooreplace en.</span><span class="sxs-lookup"><span data-stu-id="2bbf1-108">Therefore [WHILE] loops are a great alternative if you find yourself having tooreplace one.</span></span>

## <a name="leveraging-loops-and-replacing-cursors-in-sql-data-warehouse"></a><span data-ttu-id="2bbf1-109">Utnyttja slingor och ersätta markörer i SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="2bbf1-109">Leveraging loops and replacing cursors in SQL Data Warehouse</span></span>
<span data-ttu-id="2bbf1-110">Men innan du dyker i huvudet först bör du fråga dig själv hello följande fråga: ”kan den här pekaren igen skriftligt toouse set baseras operations”?.</span><span class="sxs-lookup"><span data-stu-id="2bbf1-110">However, before diving in head first you should ask yourself hello following question: "Could this cursor be re-written toouse set based operations?".</span></span> <span data-ttu-id="2bbf1-111">I många fall hello svaret blir Ja och är ofta hello bäst.</span><span class="sxs-lookup"><span data-stu-id="2bbf1-111">In many cases hello answer will be yes and is often hello best approach.</span></span> <span data-ttu-id="2bbf1-112">En uppsättning baserad åtgärd ofta utför mycket snabbare än en iterativ, rad för rad-metod.</span><span class="sxs-lookup"><span data-stu-id="2bbf1-112">A set based operation often performs significantly faster than an iterative, row by row approach.</span></span>

<span data-ttu-id="2bbf1-113">Snabba framåtriktade skrivskyddade markörer kan enkelt ersättas med en slinga konstruktion.</span><span class="sxs-lookup"><span data-stu-id="2bbf1-113">Fast forward read-only cursors can be easily replaced with a looping construct.</span></span> <span data-ttu-id="2bbf1-114">Nedan visas ett enkelt exempel.</span><span class="sxs-lookup"><span data-stu-id="2bbf1-114">Below is a simple example.</span></span> <span data-ttu-id="2bbf1-115">Det här kodexemplet uppdaterar hello statistik för varje tabell i hello-databasen.</span><span class="sxs-lookup"><span data-stu-id="2bbf1-115">This code example updates hello statistics for every table in hello database.</span></span> <span data-ttu-id="2bbf1-116">Genom att gå över hello tabeller i hello loop vi är kan tooexecute varje kommando i följd.</span><span class="sxs-lookup"><span data-stu-id="2bbf1-116">By iterating over hello tables in hello loop we are able tooexecute each command in sequence.</span></span>

<span data-ttu-id="2bbf1-117">Börja med att skapa en tillfällig tabell som innehåller en unik rad nummer används tooidentify hello enskilda uttryck:</span><span class="sxs-lookup"><span data-stu-id="2bbf1-117">First, create a temporary table containing a unique row number used tooidentify hello individual statements:</span></span>

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

<span data-ttu-id="2bbf1-118">Andra, initiera hello variabler krävs tooperform hello slinga:</span><span class="sxs-lookup"><span data-stu-id="2bbf1-118">Second, initialize hello variables required tooperform hello loop:</span></span>

```
DECLARE @nbr_statements INT = (SELECT COUNT(*) FROM #tbl)
,       @i INT = 1
;
```

<span data-ttu-id="2bbf1-119">Nu slinga över instruktioner som körs på en i taget:</span><span class="sxs-lookup"><span data-stu-id="2bbf1-119">Now loop over statements executing them one at a time:</span></span>

```
WHILE   @i <= @nbr_statements
BEGIN
    DECLARE @sql_code NVARCHAR(4000) = (SELECT sql_code FROM #tbl WHERE Sequence = @i);
    EXEC    sp_executesql @sql_code;
    SET     @i +=1;
END
```

<span data-ttu-id="2bbf1-120">Slutligen släppa hello temporära tabellen skapas i första steget i hello</span><span class="sxs-lookup"><span data-stu-id="2bbf1-120">Finally drop hello temporary table created in hello first step</span></span>

```
DROP TABLE #tbl;
```


<!--Every topic should have next steps and links toohello next logical set of content tookeep hello customer engaged-->

## <a name="next-steps"></a><span data-ttu-id="2bbf1-121">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="2bbf1-121">Next steps</span></span>
<span data-ttu-id="2bbf1-122">För fler utvecklingstips, se [utvecklingsöversikt][development overview].</span><span class="sxs-lookup"><span data-stu-id="2bbf1-122">For more development tips, see [development overview][development overview].</span></span>

<!--Image references-->

<!--Article references-->
[development overview]: sql-data-warehouse-overview-develop.md

<!--MSDN references-->
[medan]: https://msdn.microsoft.com/library/ms178642.aspx


<!--Other Web references-->
