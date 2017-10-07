---
title: aaaStored procedurer i SQL Data Warehouse | Microsoft Docs
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
ms.openlocfilehash: 416252dd3dea95c66aa5e886860b933b22578002
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="stored-procedures-in-sql-data-warehouse"></a><span data-ttu-id="2a65e-103">Lagrade procedurer i SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="2a65e-103">Stored procedures in SQL Data Warehouse</span></span>
<span data-ttu-id="2a65e-104">SQL Data Warehouse stöder många av hello Transact-SQL-funktioner som finns i SQL Server.</span><span class="sxs-lookup"><span data-stu-id="2a65e-104">SQL Data Warehouse supports many of hello Transact-SQL features found in SQL Server.</span></span> <span data-ttu-id="2a65e-105">Det finns viktigare skala ut specifika funktioner som vi tooleverage toomaximize hello prestandan för din lösning.</span><span class="sxs-lookup"><span data-stu-id="2a65e-105">More importantly there are scale out specific features that we will want tooleverage toomaximize hello performance of your solution.</span></span>

<span data-ttu-id="2a65e-106">Toomaintain hello skalning och prestanda för SQL Data Warehouse det är dock också vissa egenskaper och funktioner som har beteendebaserade skillnader och andra som inte stöds.</span><span class="sxs-lookup"><span data-stu-id="2a65e-106">However, toomaintain hello scale and performance of SQL Data Warehouse there are also some features and functionality that have behavioral differences and others that are not supported.</span></span>

<span data-ttu-id="2a65e-107">Den här artikeln förklarar hur tooimplement lagrade procedurer i SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="2a65e-107">This article explains how tooimplement stored procedures within SQL Data Warehouse.</span></span>

## <a name="introducing-stored-procedures"></a><span data-ttu-id="2a65e-108">Introduktion till lagrade procedurer</span><span class="sxs-lookup"><span data-stu-id="2a65e-108">Introducing stored procedures</span></span>
<span data-ttu-id="2a65e-109">En lagrad procedur är ett bra sätt för att kapsla in din SQL-kod. lagra det Stäng tooyour data i hello-datalagret.</span><span class="sxs-lookup"><span data-stu-id="2a65e-109">Stored procedures are a great way for encapsulating your SQL code; storing it close tooyour data in hello data warehouse.</span></span> <span data-ttu-id="2a65e-110">Genom att kapsla in hello koden i hanterbara enheter hjälper lagrade procedurer utvecklare modularize sina lösningar. underlätta större primärförpackningar i koden.</span><span class="sxs-lookup"><span data-stu-id="2a65e-110">By encapsulating hello code into manageable units stored procedures help developers modularize their solutions; facilitating greater re-usability of code.</span></span> <span data-ttu-id="2a65e-111">Alla lagrade procedurer kan också ta emot parametrar toomake dem ännu mer flexibel.</span><span class="sxs-lookup"><span data-stu-id="2a65e-111">Each stored procedure can also accept parameters toomake them even more flexible.</span></span>

<span data-ttu-id="2a65e-112">SQL Data Warehouse ger en förenklad och effektiv lagrade proceduren implementering.</span><span class="sxs-lookup"><span data-stu-id="2a65e-112">SQL Data Warehouse provides a simplified and streamlined stored procedure implementation.</span></span> <span data-ttu-id="2a65e-113">hello största skillnaden jämfört med tooSQL servern som hello lagrade proceduren är inte före kompilerad kod.</span><span class="sxs-lookup"><span data-stu-id="2a65e-113">hello biggest difference compared tooSQL Server is that hello stored procedure is not pre-compiled code.</span></span> <span data-ttu-id="2a65e-114">Vi är vanligtvis mindre berörda med hello Kompileringstid i datalager.</span><span class="sxs-lookup"><span data-stu-id="2a65e-114">In data warehouses we are generally less concerned with hello compilation time.</span></span> <span data-ttu-id="2a65e-115">Det är viktigare att hello lagrade proceduren koden är korrekt optimerad när mot stora datamängder.</span><span class="sxs-lookup"><span data-stu-id="2a65e-115">It is more important that hello stored procedure code is correctly optimised when operating against large data volumes.</span></span> <span data-ttu-id="2a65e-116">hello målet är toosave timmar, minuter och sekunder inte millisekunder.</span><span class="sxs-lookup"><span data-stu-id="2a65e-116">hello goal is toosave hours, minutes and seconds not milliseconds.</span></span> <span data-ttu-id="2a65e-117">Därför är det mer praktiskt toothink lagrade procedurer som behållare för SQL-logiken.</span><span class="sxs-lookup"><span data-stu-id="2a65e-117">It is therefore more helpful toothink of stored procedures as containers for SQL logic.</span></span>     

<span data-ttu-id="2a65e-118">När SQL Data Warehouse kör är lagrad procedur hello SQL-uttryck parsas, översättas och optimerad vid körning.</span><span class="sxs-lookup"><span data-stu-id="2a65e-118">When SQL Data Warehouse executes your stored procedure hello SQL statements are parsed, translated and optimized at run time.</span></span> <span data-ttu-id="2a65e-119">Under den här processen konvertera varje instruktion i distribuerade frågor.</span><span class="sxs-lookup"><span data-stu-id="2a65e-119">During this process each statement is converted into distributed queries.</span></span> <span data-ttu-id="2a65e-120">hello SQL-kod som faktiskt körs mot hello data är olika toohello fråga skickas.</span><span class="sxs-lookup"><span data-stu-id="2a65e-120">hello SQL code that is actually executed against hello data is different toohello query submitted.</span></span>

## <a name="nesting-stored-procedures"></a><span data-ttu-id="2a65e-121">Kapsling lagrade procedurer</span><span class="sxs-lookup"><span data-stu-id="2a65e-121">Nesting stored procedures</span></span>
<span data-ttu-id="2a65e-122">När lagrade procedurer anropa andra lagrade procedurer eller köra dynamisk sql hello inre lagrad procedur eller kod anrop sägs toobe kapslade.</span><span class="sxs-lookup"><span data-stu-id="2a65e-122">When stored procedures call other stored procedures or execute dynamic sql then hello inner stored procedure or code invocation is said toobe nested.</span></span>

<span data-ttu-id="2a65e-123">SQL Data Warehouse stöder maximalt 8 kapslingsnivåer.</span><span class="sxs-lookup"><span data-stu-id="2a65e-123">SQL Data Warehouse support a maximum of 8 nesting levels.</span></span> <span data-ttu-id="2a65e-124">Detta är något annorlunda tooSQL Server.</span><span class="sxs-lookup"><span data-stu-id="2a65e-124">This is slightly different tooSQL Server.</span></span> <span data-ttu-id="2a65e-125">hello kapsla nivå i SQL Server är 32.</span><span class="sxs-lookup"><span data-stu-id="2a65e-125">hello nest level in SQL Server is 32.</span></span>

<span data-ttu-id="2a65e-126">hello lagrat proceduranrop med översta nivån är lika toonest nivå 1</span><span class="sxs-lookup"><span data-stu-id="2a65e-126">hello top level stored procedure call equates toonest level 1</span></span>

```sql
EXEC prc_nesting
```
<span data-ttu-id="2a65e-127">Om hello lagrade proceduren är också en annan EXEC anropa sedan detta ökar hello kapsla nivå too2</span><span class="sxs-lookup"><span data-stu-id="2a65e-127">If hello stored procedure also makes another EXEC call then this will increase hello nest level too2</span></span>

```sql
CREATE PROCEDURE prc_nesting
AS
EXEC prc_nesting_2  -- This call is nest level 2
GO
EXEC prc_nesting
```
<span data-ttu-id="2a65e-128">Om hello andra proceduren utför vissa dynamisk sql sedan ökar detta hello kapsla nivå too3</span><span class="sxs-lookup"><span data-stu-id="2a65e-128">If hello second procedure then executes some dynamic sql then this will increase hello nest level too3</span></span>

```sql
CREATE PROCEDURE prc_nesting_2
AS
EXEC sp_executesql 'SELECT 'another nest level'  -- This call is nest level 2
GO
EXEC prc_nesting
```

<span data-ttu-id="2a65e-129">Obs SQL Data Warehouse stöder för närvarande inte@NESTLEVEL.</span><span class="sxs-lookup"><span data-stu-id="2a65e-129">Note SQL Data Warehouse does not currently support @@NESTLEVEL.</span></span> <span data-ttu-id="2a65e-130">Du behöver tookeep reda på nest-nivå själv.</span><span class="sxs-lookup"><span data-stu-id="2a65e-130">You will need tookeep a track of your nest level yourself.</span></span> <span data-ttu-id="2a65e-131">Det är inte troligt träffar du hello 8 kapsla begränsningen men om du vill du kommer behöver toore fungerar koden och ”förenkla” den så att den passar i den här gränsen.</span><span class="sxs-lookup"><span data-stu-id="2a65e-131">It is unlikely you will hit hello 8 nest level limit but if you do you will need toore-work your code and "flatten" it so that it fits within this limit.</span></span>

## <a name="insertexecute"></a><span data-ttu-id="2a65e-132">INSERT... KÖRA</span><span class="sxs-lookup"><span data-stu-id="2a65e-132">INSERT..EXECUTE</span></span>
<span data-ttu-id="2a65e-133">SQL Data Warehouse tillåter inte att tooconsume hello resultatet av en lagrad procedur med en INSERT-instruktion.</span><span class="sxs-lookup"><span data-stu-id="2a65e-133">SQL Data Warehouse does not permit you tooconsume hello result set of a stored procedure with an INSERT statement.</span></span> <span data-ttu-id="2a65e-134">Det finns en annan metod som du kan använda.</span><span class="sxs-lookup"><span data-stu-id="2a65e-134">However, there is an alternative approach you can use.</span></span>

<span data-ttu-id="2a65e-135">Se följande artikel toohello [temporära tabeller] ett exempel på hur toodo detta.</span><span class="sxs-lookup"><span data-stu-id="2a65e-135">Please refer toohello following article on [temporary tables] for an example on how toodo this.</span></span>

## <a name="limitations"></a><span data-ttu-id="2a65e-136">Begränsningar</span><span class="sxs-lookup"><span data-stu-id="2a65e-136">Limitations</span></span>
<span data-ttu-id="2a65e-137">Det finns vissa aspekter av Transact-SQL-lagrade procedurer som inte har implementerats i SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="2a65e-137">There are some aspects of Transact-SQL stored procedures that are not implemented in SQL Data Warehouse.</span></span>

<span data-ttu-id="2a65e-138">De är:</span><span class="sxs-lookup"><span data-stu-id="2a65e-138">They are:</span></span>

* <span data-ttu-id="2a65e-139">tillfälliga lagrade procedurer</span><span class="sxs-lookup"><span data-stu-id="2a65e-139">temporary stored procedures</span></span>
* <span data-ttu-id="2a65e-140">numrerade lagrade procedurer</span><span class="sxs-lookup"><span data-stu-id="2a65e-140">numbered stored procedures</span></span>
* <span data-ttu-id="2a65e-141">utökade lagrade procedurer</span><span class="sxs-lookup"><span data-stu-id="2a65e-141">extended stored procedures</span></span>
* <span data-ttu-id="2a65e-142">CLR lagrade procedurer</span><span class="sxs-lookup"><span data-stu-id="2a65e-142">CLR stored procedures</span></span>
* <span data-ttu-id="2a65e-143">Krypteringsalternativ</span><span class="sxs-lookup"><span data-stu-id="2a65e-143">encryption option</span></span>
* <span data-ttu-id="2a65e-144">replikeringsalternativet</span><span class="sxs-lookup"><span data-stu-id="2a65e-144">replication option</span></span>
* <span data-ttu-id="2a65e-145">tabellvärdeparametrar</span><span class="sxs-lookup"><span data-stu-id="2a65e-145">table-valued parameters</span></span>
* <span data-ttu-id="2a65e-146">skrivskyddad parametrar</span><span class="sxs-lookup"><span data-stu-id="2a65e-146">read-only parameters</span></span>
* <span data-ttu-id="2a65e-147">standardparametrar</span><span class="sxs-lookup"><span data-stu-id="2a65e-147">default parameters</span></span>
* <span data-ttu-id="2a65e-148">körningen kontexter</span><span class="sxs-lookup"><span data-stu-id="2a65e-148">execution contexts</span></span>
* <span data-ttu-id="2a65e-149">returnera instruktionen</span><span class="sxs-lookup"><span data-stu-id="2a65e-149">return statement</span></span>

## <a name="next-steps"></a><span data-ttu-id="2a65e-150">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="2a65e-150">Next steps</span></span>
<span data-ttu-id="2a65e-151">För fler utvecklingstips, se [utvecklingsöversikt][development overview].</span><span class="sxs-lookup"><span data-stu-id="2a65e-151">For more development tips, see [development overview][development overview].</span></span>

<!--Image references-->

<!--Article references-->
[temporära tabeller]: ./sql-data-warehouse-tables-temporary.md#modularizing-code
[development overview]: ./sql-data-warehouse-overview-develop.md

<!--MSDN references-->
[nest level]: https://msdn.microsoft.com/library/ms187371.aspx

<!--Other Web references-->
