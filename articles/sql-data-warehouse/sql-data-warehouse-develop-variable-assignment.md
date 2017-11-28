---
title: aaaAssign variabler i SQL Data Warehouse | Microsoft Docs
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
ms.openlocfilehash: 9de48739bb0af80ff2a117704b31512c680f78d1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="assign-variables-in-sql-data-warehouse"></a><span data-ttu-id="0bf32-103">Tilldela variabler i SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="0bf32-103">Assign variables in SQL Data Warehouse</span></span>
<span data-ttu-id="0bf32-104">Variabler i SQL Data Warehouse anges med hjälp av hello `DECLARE` instruktion eller hello `SET` instruktionen.</span><span class="sxs-lookup"><span data-stu-id="0bf32-104">Variables in SQL Data Warehouse are set using hello `DECLARE` statement or hello `SET` statement.</span></span>

<span data-ttu-id="0bf32-105">Alla följande hello är perfekt ogiltig sätt tooset ett variabelvärde:</span><span class="sxs-lookup"><span data-stu-id="0bf32-105">All of hello following are perfectly valid ways tooset a variable value:</span></span>

## <a name="setting-variables-with-declare"></a><span data-ttu-id="0bf32-106">Ange variabler med DECLARE</span><span class="sxs-lookup"><span data-stu-id="0bf32-106">Setting variables with DECLARE</span></span>
<span data-ttu-id="0bf32-107">Initiera variabler med DECLARE är en av hello mest flexibla sätt tooset variabelvärdet i SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="0bf32-107">Initializing variables with DECLARE is one of hello most flexible ways tooset a variable value in SQL Data Warehouse.</span></span>

```sql
DECLARE @v  int = 0
;
```

<span data-ttu-id="0bf32-108">Du kan också använda DECLARE tooset mer än en variabel i taget.</span><span class="sxs-lookup"><span data-stu-id="0bf32-108">You can also use DECLARE tooset more than one variable at a time.</span></span> <span data-ttu-id="0bf32-109">Du kan inte använda `SELECT` eller `UPDATE` toodo detta:</span><span class="sxs-lookup"><span data-stu-id="0bf32-109">You cannot use `SELECT` or `UPDATE` toodo this:</span></span>

```sql
DECLARE @v  INT = (SELECT TOP 1 c_customer_sk FROM Customer where c_last_name = 'Smith')
,       @v1 INT = (SELECT TOP 1 c_customer_sk FROM Customer where c_last_name = 'Jones')
;
```

<span data-ttu-id="0bf32-110">Du kan inte initiera och använda en variabel i hello samma DECLARE-sats.</span><span class="sxs-lookup"><span data-stu-id="0bf32-110">You cannot initialise and use a variable in hello same DECLARE statement.</span></span> <span data-ttu-id="0bf32-111">tooillustrate hello punkt hello exemplet nedan har **inte** tillåtna som @p1 är både initierats och användas i hello samma DECLARE-sats.</span><span class="sxs-lookup"><span data-stu-id="0bf32-111">tooillustrate hello point hello example below is **not** allowed as @p1 is both initialized and used in hello same DECLARE statement.</span></span> <span data-ttu-id="0bf32-112">Detta resulterar i ett fel.</span><span class="sxs-lookup"><span data-stu-id="0bf32-112">This will result in an error.</span></span>

```sql
DECLARE @p1 int = 0
,       @p2 int = (SELECT COUNT (*) FROM sys.types where is_user_defined = @p1 )
;
```

## <a name="setting-values-with-set"></a><span data-ttu-id="0bf32-113">Ange värden med</span><span class="sxs-lookup"><span data-stu-id="0bf32-113">Setting values with SET</span></span>
<span data-ttu-id="0bf32-114">Är en mycket vanlig metod för att ställa in en enskild variabel.</span><span class="sxs-lookup"><span data-stu-id="0bf32-114">Set is a very common method for setting a single variable.</span></span>

<span data-ttu-id="0bf32-115">Alla hello exemplen nedan är giltiga sätt för en variabel med:</span><span class="sxs-lookup"><span data-stu-id="0bf32-115">All of hello examples below are valid ways of setting a variable with SET:</span></span>

```sql
SET     @v = (Select max(database_id) from sys.databases);
SET     @v = 1;
SET     @v = @v+1;
SET     @v +=1;
```

<span data-ttu-id="0bf32-116">Du kan bara ange en variabel i taget med.</span><span class="sxs-lookup"><span data-stu-id="0bf32-116">You can only set one variable at a time with SET.</span></span> <span data-ttu-id="0bf32-117">Men som kan ses över sammansatt operatorer är tillåtna.</span><span class="sxs-lookup"><span data-stu-id="0bf32-117">However, as can be seen above compound operators are permissable.</span></span>

## <a name="limitations"></a><span data-ttu-id="0bf32-118">Begränsningar</span><span class="sxs-lookup"><span data-stu-id="0bf32-118">Limitations</span></span>
<span data-ttu-id="0bf32-119">Du kan inte använda SELECT- eller UPDATE för variabeltilldelning.</span><span class="sxs-lookup"><span data-stu-id="0bf32-119">You cannot use SELECT or UPDATE for variable assignment.</span></span>

## <a name="next-steps"></a><span data-ttu-id="0bf32-120">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="0bf32-120">Next steps</span></span>
<span data-ttu-id="0bf32-121">För fler utvecklingstips, se [utvecklingsöversikt][development overview].</span><span class="sxs-lookup"><span data-stu-id="0bf32-121">For more development tips, see [development overview][development overview].</span></span>

<!--Image references-->

<!--Article references-->
[development overview]: sql-data-warehouse-overview-develop.md

<!--MSDN references-->

<!--Other Web references-->
