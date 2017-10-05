---
title: Utnyttja T-SQL-loopar i Azure SQL Data Warehouse | Microsoft Docs
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
ms.openlocfilehash: 40a872ff310f48bfd543ac184fe7301b85b50258
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="loops-in-sql-data-warehouse"></a><span data-ttu-id="137ae-103">Slingor i SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="137ae-103">Loops in SQL Data Warehouse</span></span>
<span data-ttu-id="137ae-104">SQL Data Warehouse stöder den [medan][medan] loop för att köra instruktionen block upprepade gånger.</span><span class="sxs-lookup"><span data-stu-id="137ae-104">SQL Data Warehouse supports the [WHILE][WHILE] loop for repeatedly executing statement blocks.</span></span> <span data-ttu-id="137ae-105">Detta fortsätter för förutsatt att de angivna villkoren är SANT eller tills koden specifikt avslutar en loop med hjälp av den `BREAK` nyckelord.</span><span class="sxs-lookup"><span data-stu-id="137ae-105">This will continue for as long as the specified conditions are true or until the code specifically terminates the loop using the `BREAK` keyword.</span></span> <span data-ttu-id="137ae-106">Slingor är särskilt användbara för att ersätta markörer som definierats i SQL-kod.</span><span class="sxs-lookup"><span data-stu-id="137ae-106">Loops are particularly useful for replacing cursors defined in SQL code.</span></span> <span data-ttu-id="137ae-107">Lyckligtvis läses nästan alla markörer som har skrivits i SQL-kod för snabb framåtriktad endast olika.</span><span class="sxs-lookup"><span data-stu-id="137ae-107">Fortunately, almost all cursors that are written in SQL code are of the fast forward, read only variety.</span></span> <span data-ttu-id="137ae-108">Därför [medan] slingor är ett bra alternativ om du själv behöva ersätta någon.</span><span class="sxs-lookup"><span data-stu-id="137ae-108">Therefore [WHILE] loops are a great alternative if you find yourself having to replace one.</span></span>

## <a name="leveraging-loops-and-replacing-cursors-in-sql-data-warehouse"></a><span data-ttu-id="137ae-109">Utnyttja slingor och ersätta markörer i SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="137ae-109">Leveraging loops and replacing cursors in SQL Data Warehouse</span></span>
<span data-ttu-id="137ae-110">Men innan du dyker i huvudet först bör du fråga dig själv följande fråga: ”kunde markören nytt skrivas till använda uppsättning baserad operations”?.</span><span class="sxs-lookup"><span data-stu-id="137ae-110">However, before diving in head first you should ask yourself the following question: "Could this cursor be re-written to use set based operations?".</span></span> <span data-ttu-id="137ae-111">I många fall svaret blir Ja och är ofta det bästa sättet.</span><span class="sxs-lookup"><span data-stu-id="137ae-111">In many cases the answer will be yes and is often the best approach.</span></span> <span data-ttu-id="137ae-112">En uppsättning baserad åtgärd ofta utför mycket snabbare än en iterativ, rad för rad-metod.</span><span class="sxs-lookup"><span data-stu-id="137ae-112">A set based operation often performs significantly faster than an iterative, row by row approach.</span></span>

<span data-ttu-id="137ae-113">Snabba framåtriktade skrivskyddade markörer kan enkelt ersättas med en slinga konstruktion.</span><span class="sxs-lookup"><span data-stu-id="137ae-113">Fast forward read-only cursors can be easily replaced with a looping construct.</span></span> <span data-ttu-id="137ae-114">Nedan visas ett enkelt exempel.</span><span class="sxs-lookup"><span data-stu-id="137ae-114">Below is a simple example.</span></span> <span data-ttu-id="137ae-115">Det här kodexemplet Uppdaterar statistiken för alla tabeller i databasen.</span><span class="sxs-lookup"><span data-stu-id="137ae-115">This code example updates the statistics for every table in the database.</span></span> <span data-ttu-id="137ae-116">Genom att gå över tabeller i en slinga kommer du att köra varje kommando i sekvensen.</span><span class="sxs-lookup"><span data-stu-id="137ae-116">By iterating over the tables in the loop we are able to execute each command in sequence.</span></span>

<span data-ttu-id="137ae-117">Börja med att skapa en tillfällig tabell som innehåller ett unikt radnummer som används för att identifiera de enskilda rapporterna:</span><span class="sxs-lookup"><span data-stu-id="137ae-117">First, create a temporary table containing a unique row number used to identify the individual statements:</span></span>

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

<span data-ttu-id="137ae-118">Andra, initiera variablerna som krävs för att utföra slingan:</span><span class="sxs-lookup"><span data-stu-id="137ae-118">Second, initialize the variables required to perform the loop:</span></span>

```
DECLARE @nbr_statements INT = (SELECT COUNT(*) FROM #tbl)
,       @i INT = 1
;
```

<span data-ttu-id="137ae-119">Nu slinga över instruktioner som körs på en i taget:</span><span class="sxs-lookup"><span data-stu-id="137ae-119">Now loop over statements executing them one at a time:</span></span>

```
WHILE   @i <= @nbr_statements
BEGIN
    DECLARE @sql_code NVARCHAR(4000) = (SELECT sql_code FROM #tbl WHERE Sequence = @i);
    EXEC    sp_executesql @sql_code;
    SET     @i +=1;
END
```

<span data-ttu-id="137ae-120">Slutligen släppa den temporära tabellen skapas i det första steget</span><span class="sxs-lookup"><span data-stu-id="137ae-120">Finally drop the temporary table created in the first step</span></span>

```
DROP TABLE #tbl;
```


<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->

## <a name="next-steps"></a><span data-ttu-id="137ae-121">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="137ae-121">Next steps</span></span>
<span data-ttu-id="137ae-122">För fler utvecklingstips, se [utvecklingsöversikt][development overview].</span><span class="sxs-lookup"><span data-stu-id="137ae-122">For more development tips, see [development overview][development overview].</span></span>

<!--Image references-->

<!--Article references-->
[development overview]: sql-data-warehouse-overview-develop.md

<!--MSDN references-->
<span data-ttu-id="137ae-123">[medan]: https://msdn.microsoft.com/library/ms178642.aspx</span><span class="sxs-lookup"><span data-stu-id="137ae-123">[WHILE]: https://msdn.microsoft.com/library/ms178642.aspx</span></span>


<!--Other Web references-->
