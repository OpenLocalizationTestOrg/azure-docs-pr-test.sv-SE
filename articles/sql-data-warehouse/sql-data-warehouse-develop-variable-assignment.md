---
title: Tilldela variabler i SQL Data Warehouse | Microsoft Docs
description: "Tips för att tilldela Transact-SQL-variabler i Azure SQL Data Warehouse för utveckling av lösningar."
services: sql-data-warehouse
documentationcenter: NA
author: jrowlandjones
manager: jhubbard
editor: 
ms.assetid: 81ddc7cf-a6ba-4585-91a3-b6ea50f49227
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: t-sql
ms.date: 10/31/2016
ms.author: jrj;barbkess
ms.openlocfilehash: 045d5148cd3f12dac63c961ccf7c953d355ed725
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="assign-variables-in-sql-data-warehouse"></a><span data-ttu-id="f892f-103">Tilldela variabler i SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="f892f-103">Assign variables in SQL Data Warehouse</span></span>
<span data-ttu-id="f892f-104">Variabler i SQL Data Warehouse anges med hjälp av den `DECLARE` instruktionen eller `SET` instruktionen.</span><span class="sxs-lookup"><span data-stu-id="f892f-104">Variables in SQL Data Warehouse are set using the `DECLARE` statement or the `SET` statement.</span></span>

<span data-ttu-id="f892f-105">Följande är perfekt giltiga sätt att ange ett variabelvärde:</span><span class="sxs-lookup"><span data-stu-id="f892f-105">All of the following are perfectly valid ways to set a variable value:</span></span>

## <a name="setting-variables-with-declare"></a><span data-ttu-id="f892f-106">Ange variabler med DECLARE</span><span class="sxs-lookup"><span data-stu-id="f892f-106">Setting variables with DECLARE</span></span>
<span data-ttu-id="f892f-107">Initiera variabler med DECLARE är en av de mest flexibla sätten att ange ett värde för variabeln i SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="f892f-107">Initializing variables with DECLARE is one of the most flexible ways to set a variable value in SQL Data Warehouse.</span></span>

```sql
DECLARE @v  int = 0
;
```

<span data-ttu-id="f892f-108">Du kan också använda DECLARE för att ange fler än en variabel i taget.</span><span class="sxs-lookup"><span data-stu-id="f892f-108">You can also use DECLARE to set more than one variable at a time.</span></span> <span data-ttu-id="f892f-109">Du kan inte använda `SELECT` eller `UPDATE` att göra detta:</span><span class="sxs-lookup"><span data-stu-id="f892f-109">You cannot use `SELECT` or `UPDATE` to do this:</span></span>

```sql
DECLARE @v  INT = (SELECT TOP 1 c_customer_sk FROM Customer where c_last_name = 'Smith')
,       @v1 INT = (SELECT TOP 1 c_customer_sk FROM Customer where c_last_name = 'Jones')
;
```

<span data-ttu-id="f892f-110">Du kan inte initiera och använda en variabel i samma DECLARE-sats.</span><span class="sxs-lookup"><span data-stu-id="f892f-110">You cannot initialise and use a variable in the same DECLARE statement.</span></span> <span data-ttu-id="f892f-111">Att illustrera den punkt som i exemplet nedan har **inte** tillåtna som @p1 är både initierats och användas i samma DECLARE-sats.</span><span class="sxs-lookup"><span data-stu-id="f892f-111">To illustrate the point the example below is **not** allowed as @p1 is both initialized and used in the same DECLARE statement.</span></span> <span data-ttu-id="f892f-112">Detta resulterar i ett fel.</span><span class="sxs-lookup"><span data-stu-id="f892f-112">This will result in an error.</span></span>

```sql
DECLARE @p1 int = 0
,       @p2 int = (SELECT COUNT (*) FROM sys.types where is_user_defined = @p1 )
;
```

## <a name="setting-values-with-set"></a><span data-ttu-id="f892f-113">Ange värden med</span><span class="sxs-lookup"><span data-stu-id="f892f-113">Setting values with SET</span></span>
<span data-ttu-id="f892f-114">Är en mycket vanlig metod för att ställa in en enskild variabel.</span><span class="sxs-lookup"><span data-stu-id="f892f-114">Set is a very common method for setting a single variable.</span></span>

<span data-ttu-id="f892f-115">Alla exemplen nedan är giltiga sätt för en variabel med:</span><span class="sxs-lookup"><span data-stu-id="f892f-115">All of the examples below are valid ways of setting a variable with SET:</span></span>

```sql
SET     @v = (Select max(database_id) from sys.databases);
SET     @v = 1;
SET     @v = @v+1;
SET     @v +=1;
```

<span data-ttu-id="f892f-116">Du kan bara ange en variabel i taget med.</span><span class="sxs-lookup"><span data-stu-id="f892f-116">You can only set one variable at a time with SET.</span></span> <span data-ttu-id="f892f-117">Men som kan ses över sammansatt operatorer är tillåtna.</span><span class="sxs-lookup"><span data-stu-id="f892f-117">However, as can be seen above compound operators are permissable.</span></span>

## <a name="limitations"></a><span data-ttu-id="f892f-118">Begränsningar</span><span class="sxs-lookup"><span data-stu-id="f892f-118">Limitations</span></span>
<span data-ttu-id="f892f-119">Du kan inte använda SELECT- eller UPDATE för variabeltilldelning.</span><span class="sxs-lookup"><span data-stu-id="f892f-119">You cannot use SELECT or UPDATE for variable assignment.</span></span>

## <a name="next-steps"></a><span data-ttu-id="f892f-120">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="f892f-120">Next steps</span></span>
<span data-ttu-id="f892f-121">För fler utvecklingstips, se [utvecklingsöversikt][development overview].</span><span class="sxs-lookup"><span data-stu-id="f892f-121">For more development tips, see [development overview][development overview].</span></span>

<!--Image references-->

<!--Article references-->
[development overview]: sql-data-warehouse-overview-develop.md

<!--MSDN references-->

<!--Other Web references-->
