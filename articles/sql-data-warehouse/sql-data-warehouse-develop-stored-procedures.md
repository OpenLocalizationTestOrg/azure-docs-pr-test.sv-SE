---
title: Lagrade procedurer i SQL Data Warehouse | Microsoft Docs
description: "Tips för att använda lagrade procedurer i Azure SQL Data Warehouse för utveckling av lösningar."
services: sql-data-warehouse
documentationcenter: NA
author: jrowlandjones
manager: jhubbard
editor: 
ms.assetid: 9b238789-6efe-4820-bf77-5a5da2afa0e8
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: t-sql
ms.date: 10/31/2016
ms.author: jrj;barbkess
ms.openlocfilehash: e42d80f0ca35f3fbb67389c66d072bc40d8a8d2c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="stored-procedures-in-sql-data-warehouse"></a><span data-ttu-id="9ad52-103">Lagrade procedurer i SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="9ad52-103">Stored procedures in SQL Data Warehouse</span></span>
<span data-ttu-id="9ad52-104">SQL Data Warehouse stöder många Transact-SQL-funktioner som finns i SQL Server.</span><span class="sxs-lookup"><span data-stu-id="9ad52-104">SQL Data Warehouse supports many of the Transact-SQL features found in SQL Server.</span></span> <span data-ttu-id="9ad52-105">Det finns viktigare skala ut specifika funktioner som vi ska använda för att maximera prestandan för din lösning.</span><span class="sxs-lookup"><span data-stu-id="9ad52-105">More importantly there are scale out specific features that we will want to leverage to maximize the performance of your solution.</span></span>

<span data-ttu-id="9ad52-106">Om du vill behålla är skalning och prestanda för SQL Data Warehouse det dock också vissa egenskaper och funktioner som har beteendebaserade skillnader och andra som inte stöds.</span><span class="sxs-lookup"><span data-stu-id="9ad52-106">However, to maintain the scale and performance of SQL Data Warehouse there are also some features and functionality that have behavioral differences and others that are not supported.</span></span>

<span data-ttu-id="9ad52-107">Den här artikeln förklarar hur du implementerar lagrade procedurer i SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="9ad52-107">This article explains how to implement stored procedures within SQL Data Warehouse.</span></span>

## <a name="introducing-stored-procedures"></a><span data-ttu-id="9ad52-108">Introduktion till lagrade procedurer</span><span class="sxs-lookup"><span data-stu-id="9ad52-108">Introducing stored procedures</span></span>
<span data-ttu-id="9ad52-109">En lagrad procedur är ett bra sätt för att kapsla in din SQL-kod. lagra det nära dina data i datalagret.</span><span class="sxs-lookup"><span data-stu-id="9ad52-109">Stored procedures are a great way for encapsulating your SQL code; storing it close to your data in the data warehouse.</span></span> <span data-ttu-id="9ad52-110">Genom att kapsla in koden i hanterbara enheter hjälper lagrade procedurer utvecklare modularize sina lösningar. underlätta större primärförpackningar i koden.</span><span class="sxs-lookup"><span data-stu-id="9ad52-110">By encapsulating the code into manageable units stored procedures help developers modularize their solutions; facilitating greater re-usability of code.</span></span> <span data-ttu-id="9ad52-111">Alla lagrade procedurer kan också accepterar parametrar för att göra dem ännu mer flexibel.</span><span class="sxs-lookup"><span data-stu-id="9ad52-111">Each stored procedure can also accept parameters to make them even more flexible.</span></span>

<span data-ttu-id="9ad52-112">SQL Data Warehouse ger en förenklad och effektiv lagrade proceduren implementering.</span><span class="sxs-lookup"><span data-stu-id="9ad52-112">SQL Data Warehouse provides a simplified and streamlined stored procedure implementation.</span></span> <span data-ttu-id="9ad52-113">Den största skillnaden jämfört med SQL Server är att den lagrade proceduren inte före kompilerad kod.</span><span class="sxs-lookup"><span data-stu-id="9ad52-113">The biggest difference compared to SQL Server is that the stored procedure is not pre-compiled code.</span></span> <span data-ttu-id="9ad52-114">Vi är vanligtvis mindre berörda med Kompileringstid i datalager.</span><span class="sxs-lookup"><span data-stu-id="9ad52-114">In data warehouses we are generally less concerned with the compilation time.</span></span> <span data-ttu-id="9ad52-115">Det är mer viktigt att den lagrade procedur koden är korrekt optimerad när mot stora datamängder.</span><span class="sxs-lookup"><span data-stu-id="9ad52-115">It is more important that the stored procedure code is correctly optimised when operating against large data volumes.</span></span> <span data-ttu-id="9ad52-116">Målet är att spara timmar, minuter och sekunder inte millisekunder.</span><span class="sxs-lookup"><span data-stu-id="9ad52-116">The goal is to save hours, minutes and seconds not milliseconds.</span></span> <span data-ttu-id="9ad52-117">Därför är det mer bra att fundera över lagrade procedurer som behållare för SQL-logiken.</span><span class="sxs-lookup"><span data-stu-id="9ad52-117">It is therefore more helpful to think of stored procedures as containers for SQL logic.</span></span>     

<span data-ttu-id="9ad52-118">När SQL Data Warehouse kör den lagrade proceduren parsas SQL-instruktioner, översatta och optimerad vid körning.</span><span class="sxs-lookup"><span data-stu-id="9ad52-118">When SQL Data Warehouse executes your stored procedure the SQL statements are parsed, translated and optimized at run time.</span></span> <span data-ttu-id="9ad52-119">Under den här processen konvertera varje instruktion i distribuerade frågor.</span><span class="sxs-lookup"><span data-stu-id="9ad52-119">During this process each statement is converted into distributed queries.</span></span> <span data-ttu-id="9ad52-120">SQL-kod som faktiskt körs mot data skiljer sig i frågan har skickats.</span><span class="sxs-lookup"><span data-stu-id="9ad52-120">The SQL code that is actually executed against the data is different to the query submitted.</span></span>

## <a name="nesting-stored-procedures"></a><span data-ttu-id="9ad52-121">Kapsling lagrade procedurer</span><span class="sxs-lookup"><span data-stu-id="9ad52-121">Nesting stored procedures</span></span>
<span data-ttu-id="9ad52-122">När lagrade procedurer anropa andra lagrade procedurer eller köra dynamisk sql sedan inre lagrade proceduren eller kod anrop anses vara kapslade.</span><span class="sxs-lookup"><span data-stu-id="9ad52-122">When stored procedures call other stored procedures or execute dynamic sql then the inner stored procedure or code invocation is said to be nested.</span></span>

<span data-ttu-id="9ad52-123">SQL Data Warehouse stöder maximalt 8 kapslingsnivåer.</span><span class="sxs-lookup"><span data-stu-id="9ad52-123">SQL Data Warehouse support a maximum of 8 nesting levels.</span></span> <span data-ttu-id="9ad52-124">Detta är något annorlunda till SQL Server.</span><span class="sxs-lookup"><span data-stu-id="9ad52-124">This is slightly different to SQL Server.</span></span> <span data-ttu-id="9ad52-125">Den kapslade i SQL Server är 32.</span><span class="sxs-lookup"><span data-stu-id="9ad52-125">The nest level in SQL Server is 32.</span></span>

<span data-ttu-id="9ad52-126">Det översta nivå lagrade proceduranropet är lika om du vill kapsla nivå 1</span><span class="sxs-lookup"><span data-stu-id="9ad52-126">The top level stored procedure call equates to nest level 1</span></span>

```sql
EXEC prc_nesting
```
<span data-ttu-id="9ad52-127">Om den lagrade proceduren är också en annan EXEC anropa sedan ökar detta kapsla nivå 2</span><span class="sxs-lookup"><span data-stu-id="9ad52-127">If the stored procedure also makes another EXEC call then this will increase the nest level to 2</span></span>

```sql
CREATE PROCEDURE prc_nesting
AS
EXEC prc_nesting_2  -- This call is nest level 2
GO
EXEC prc_nesting
```
<span data-ttu-id="9ad52-128">Om den andra proceduren utför vissa dynamisk sql sedan ökar detta kapsla nivå 3</span><span class="sxs-lookup"><span data-stu-id="9ad52-128">If the second procedure then executes some dynamic sql then this will increase the nest level to 3</span></span>

```sql
CREATE PROCEDURE prc_nesting_2
AS
EXEC sp_executesql 'SELECT 'another nest level'  -- This call is nest level 2
GO
EXEC prc_nesting
```

<span data-ttu-id="9ad52-129">Obs SQL Data Warehouse stöder för närvarande inte@NESTLEVEL.</span><span class="sxs-lookup"><span data-stu-id="9ad52-129">Note SQL Data Warehouse does not currently support @@NESTLEVEL.</span></span> <span data-ttu-id="9ad52-130">Du måste hålla reda på nest-nivå dig.</span><span class="sxs-lookup"><span data-stu-id="9ad52-130">You will need to keep a track of your nest level yourself.</span></span> <span data-ttu-id="9ad52-131">Det är inte troligt träffar du begränsningen på 8 kapsla men i så fall behöver du igen att fungera och ”förenkla” den så att den passar i den här gränsen.</span><span class="sxs-lookup"><span data-stu-id="9ad52-131">It is unlikely you will hit the 8 nest level limit but if you do you will need to re-work your code and "flatten" it so that it fits within this limit.</span></span>

## <a name="insertexecute"></a><span data-ttu-id="9ad52-132">INSERT... KÖRA</span><span class="sxs-lookup"><span data-stu-id="9ad52-132">INSERT..EXECUTE</span></span>
<span data-ttu-id="9ad52-133">SQL Data Warehouse tillåter inte att du kan använda resultatet av en lagrad procedur med en INSERT-instruktion.</span><span class="sxs-lookup"><span data-stu-id="9ad52-133">SQL Data Warehouse does not permit you to consume the result set of a stored procedure with an INSERT statement.</span></span> <span data-ttu-id="9ad52-134">Det finns en annan metod som du kan använda.</span><span class="sxs-lookup"><span data-stu-id="9ad52-134">However, there is an alternative approach you can use.</span></span>

<span data-ttu-id="9ad52-135">Se följande artikel på [temporära tabeller] ett exempel på hur du gör detta.</span><span class="sxs-lookup"><span data-stu-id="9ad52-135">Please refer to the following article on [temporary tables] for an example on how to do this.</span></span>

## <a name="limitations"></a><span data-ttu-id="9ad52-136">Begränsningar</span><span class="sxs-lookup"><span data-stu-id="9ad52-136">Limitations</span></span>
<span data-ttu-id="9ad52-137">Det finns vissa aspekter av Transact-SQL-lagrade procedurer som inte har implementerats i SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="9ad52-137">There are some aspects of Transact-SQL stored procedures that are not implemented in SQL Data Warehouse.</span></span>

<span data-ttu-id="9ad52-138">De är:</span><span class="sxs-lookup"><span data-stu-id="9ad52-138">They are:</span></span>

* <span data-ttu-id="9ad52-139">tillfälliga lagrade procedurer</span><span class="sxs-lookup"><span data-stu-id="9ad52-139">temporary stored procedures</span></span>
* <span data-ttu-id="9ad52-140">numrerade lagrade procedurer</span><span class="sxs-lookup"><span data-stu-id="9ad52-140">numbered stored procedures</span></span>
* <span data-ttu-id="9ad52-141">utökade lagrade procedurer</span><span class="sxs-lookup"><span data-stu-id="9ad52-141">extended stored procedures</span></span>
* <span data-ttu-id="9ad52-142">CLR lagrade procedurer</span><span class="sxs-lookup"><span data-stu-id="9ad52-142">CLR stored procedures</span></span>
* <span data-ttu-id="9ad52-143">Krypteringsalternativ</span><span class="sxs-lookup"><span data-stu-id="9ad52-143">encryption option</span></span>
* <span data-ttu-id="9ad52-144">replikeringsalternativet</span><span class="sxs-lookup"><span data-stu-id="9ad52-144">replication option</span></span>
* <span data-ttu-id="9ad52-145">tabellvärdeparametrar</span><span class="sxs-lookup"><span data-stu-id="9ad52-145">table-valued parameters</span></span>
* <span data-ttu-id="9ad52-146">skrivskyddad parametrar</span><span class="sxs-lookup"><span data-stu-id="9ad52-146">read-only parameters</span></span>
* <span data-ttu-id="9ad52-147">standardparametrar</span><span class="sxs-lookup"><span data-stu-id="9ad52-147">default parameters</span></span>
* <span data-ttu-id="9ad52-148">körningen kontexter</span><span class="sxs-lookup"><span data-stu-id="9ad52-148">execution contexts</span></span>
* <span data-ttu-id="9ad52-149">returnera instruktionen</span><span class="sxs-lookup"><span data-stu-id="9ad52-149">return statement</span></span>

## <a name="next-steps"></a><span data-ttu-id="9ad52-150">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="9ad52-150">Next steps</span></span>
<span data-ttu-id="9ad52-151">För fler utvecklingstips, se [utvecklingsöversikt][development overview].</span><span class="sxs-lookup"><span data-stu-id="9ad52-151">For more development tips, see [development overview][development overview].</span></span>

<!--Image references-->

<!--Article references-->
<span data-ttu-id="9ad52-152">[temporära tabeller]: ./sql-data-warehouse-tables-temporary.md#modularizing-code</span><span class="sxs-lookup"><span data-stu-id="9ad52-152">[temporary tables]: ./sql-data-warehouse-tables-temporary.md#modularizing-code</span></span>
[development overview]: ./sql-data-warehouse-overview-develop.md

<!--MSDN references-->
[nest level]: https://msdn.microsoft.com/library/ms187371.aspx

<!--Other Web references-->
