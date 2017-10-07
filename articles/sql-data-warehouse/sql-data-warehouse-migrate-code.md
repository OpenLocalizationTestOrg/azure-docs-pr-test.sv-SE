---
title: aaaMigrate din SQL-kod tooSQL Data Warehouse | Microsoft Docs
description: "Tips för att migrera dina SQL-kod tooAzure SQL Data Warehouse för utveckling av lösningar."
services: sql-data-warehouse
documentationcenter: NA
author: sqlmojo
manager: jhubbard
editor: 
ms.assetid: 19c252a3-0e41-4eec-9d3e-09a68c7e7add
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: migrate
ms.date: 06/23/2017
ms.author: joeyong;barbkess
ms.openlocfilehash: 7a16d579d068e9df9aba3dc61e4a09bcaa551588
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="migrate-your-sql-code-toosql-data-warehouse"></a><span data-ttu-id="eec13-103">Migrera dina SQL-kod tooSQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="eec13-103">Migrate your SQL code tooSQL Data Warehouse</span></span>
<span data-ttu-id="eec13-104">Den här artikeln förklarar kodändringar du förmodligen ha toomake när du migrerar din kod från en annan databas tooSQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="eec13-104">This article explains code changes you will probably need toomake when migrating your code from another database tooSQL Data Warehouse.</span></span> <span data-ttu-id="eec13-105">Vissa funktioner i SQL Data Warehouse förbättrar prestandan som de är utformad toowork distribuerade överskrids.</span><span class="sxs-lookup"><span data-stu-id="eec13-105">Some SQL Data Warehouse features can significantly improve performance as they are designed toowork in a distributed fashion.</span></span> <span data-ttu-id="eec13-106">Toomaintain prestanda och skalning, vissa funktioner är dock också inte tillgänglig.</span><span class="sxs-lookup"><span data-stu-id="eec13-106">However, toomaintain performance and scale, some features are also not available.</span></span>

## <a name="common-t-sql-limitations"></a><span data-ttu-id="eec13-107">Vanliga T-SQL-begränsningar</span><span class="sxs-lookup"><span data-stu-id="eec13-107">Common T-SQL Limitations</span></span>
<span data-ttu-id="eec13-108">hello följande lista sammanfattas de vanligaste hello-funktioner som inte har stöd för SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="eec13-108">hello following list summarizes hello most common features that SQL Data Warehouse does not support.</span></span> <span data-ttu-id="eec13-109">hello länkar om tooworkarounds för hello stöds inte funktioner:</span><span class="sxs-lookup"><span data-stu-id="eec13-109">hello links take you tooworkarounds for hello unsupported features:</span></span>

* <span data-ttu-id="eec13-110">[ANSI kopplingar uppdateringar][ANSI joins on updates]</span><span class="sxs-lookup"><span data-stu-id="eec13-110">[ANSI joins on updates][ANSI joins on updates]</span></span>
* <span data-ttu-id="eec13-111">[ANSI-kopplingar på borttagningar][ANSI joins on deletes]</span><span class="sxs-lookup"><span data-stu-id="eec13-111">[ANSI joins on deletes][ANSI joins on deletes]</span></span>
* <span data-ttu-id="eec13-112">[merge-instruktion][merge statement]</span><span class="sxs-lookup"><span data-stu-id="eec13-112">[merge statement][merge statement]</span></span>
* <span data-ttu-id="eec13-113">flera databaser kopplingar</span><span class="sxs-lookup"><span data-stu-id="eec13-113">cross-database joins</span></span>
* <span data-ttu-id="eec13-114">[markörer][cursors]</span><span class="sxs-lookup"><span data-stu-id="eec13-114">[cursors][cursors]</span></span>
* <span data-ttu-id="eec13-115">[INSERT... EXEC][INSERT..EXEC]</span><span class="sxs-lookup"><span data-stu-id="eec13-115">[INSERT..EXEC][INSERT..EXEC]</span></span>
* <span data-ttu-id="eec13-116">Output-sats</span><span class="sxs-lookup"><span data-stu-id="eec13-116">output clause</span></span>
* <span data-ttu-id="eec13-117">infogade användardefinierade funktioner</span><span class="sxs-lookup"><span data-stu-id="eec13-117">inline user-defined functions</span></span>
* <span data-ttu-id="eec13-118">flera instruktioner funktioner</span><span class="sxs-lookup"><span data-stu-id="eec13-118">multi-statement functions</span></span>
* [<span data-ttu-id="eec13-119">vanliga tabelluttryck</span><span class="sxs-lookup"><span data-stu-id="eec13-119">common table expressions</span></span>](#Common-table-expressions)
* <span data-ttu-id="eec13-120">[rekursiva cte (CTE)] (#Recursive-common-table-expressions-(CTE)</span><span class="sxs-lookup"><span data-stu-id="eec13-120">[recursive common table expressions (CTE)](#Recursive-common-table-expressions-(CTE)</span></span>
* <span data-ttu-id="eec13-121">CLR-funktioner och procedurer</span><span class="sxs-lookup"><span data-stu-id="eec13-121">CLR functions and procedures</span></span>
* <span data-ttu-id="eec13-122">$partition funktion</span><span class="sxs-lookup"><span data-stu-id="eec13-122">$partition function</span></span>
* <span data-ttu-id="eec13-123">tabellvariabler</span><span class="sxs-lookup"><span data-stu-id="eec13-123">table variables</span></span>
* <span data-ttu-id="eec13-124">tabellen värdet parametrar</span><span class="sxs-lookup"><span data-stu-id="eec13-124">table value parameters</span></span>
* <span data-ttu-id="eec13-125">distribuerade transaktioner</span><span class="sxs-lookup"><span data-stu-id="eec13-125">distributed transactions</span></span>
* <span data-ttu-id="eec13-126">Commit / rollback arbete</span><span class="sxs-lookup"><span data-stu-id="eec13-126">commit / rollback work</span></span>
* <span data-ttu-id="eec13-127">Spara transaktionen</span><span class="sxs-lookup"><span data-stu-id="eec13-127">save transaction</span></span>
* <span data-ttu-id="eec13-128">körningen kontexter (EXECUTE AS)</span><span class="sxs-lookup"><span data-stu-id="eec13-128">execution contexts (EXECUTE AS)</span></span>
* <span data-ttu-id="eec13-129">[Group by-satser med Samlad / kub / grupperingsalternativ uppsättningar][group by clause with rollup / cube / grouping sets options]</span><span class="sxs-lookup"><span data-stu-id="eec13-129">[group by clause with rollup / cube / grouping sets options][group by clause with rollup / cube / grouping sets options]</span></span>
* <span data-ttu-id="eec13-130">[kapslingsnivåer utöver 8][nesting levels beyond 8]</span><span class="sxs-lookup"><span data-stu-id="eec13-130">[nesting levels beyond 8][nesting levels beyond 8]</span></span>
* <span data-ttu-id="eec13-131">[uppdatering i vyer][updating through views]</span><span class="sxs-lookup"><span data-stu-id="eec13-131">[updating through views][updating through views]</span></span>
* <span data-ttu-id="eec13-132">[användning av väljer för variabeltilldelning][use of select for variable assignment]</span><span class="sxs-lookup"><span data-stu-id="eec13-132">[use of select for variable assignment][use of select for variable assignment]</span></span>
* <span data-ttu-id="eec13-133">[Ingen MAX datatyp för dynamisk SQL-strängar][no MAX data type for dynamic SQL strings]</span><span class="sxs-lookup"><span data-stu-id="eec13-133">[no MAX data type for dynamic SQL strings][no MAX data type for dynamic SQL strings]</span></span>

<span data-ttu-id="eec13-134">De flesta av dessa begränsningar kan Lyckligtvis arbetat runt.</span><span class="sxs-lookup"><span data-stu-id="eec13-134">Fortunately most of these limitations can be worked around.</span></span> <span data-ttu-id="eec13-135">Förklaringar finns i hello relevanta development artiklar som anges ovan.</span><span class="sxs-lookup"><span data-stu-id="eec13-135">Explanations are provided in hello relevant development articles referenced above.</span></span>

## <a name="supported-cte-features"></a><span data-ttu-id="eec13-136">CTE-funktioner som stöds</span><span class="sxs-lookup"><span data-stu-id="eec13-136">Supported CTE features</span></span>
<span data-ttu-id="eec13-137">Cte (cte-referenser) stöds delvis i SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="eec13-137">Common table expressions (CTEs) are partially supported in SQL Data Warehouse.</span></span>  <span data-ttu-id="eec13-138">hello följande CTE funktioner stöds:</span><span class="sxs-lookup"><span data-stu-id="eec13-138">hello following CTE features are currently supported:</span></span>

* <span data-ttu-id="eec13-139">En CTE kan anges i en SELECT-instruktion.</span><span class="sxs-lookup"><span data-stu-id="eec13-139">A CTE can be specified in a SELECT statement.</span></span>
* <span data-ttu-id="eec13-140">En CTE kan anges i instruktionen CREATE VIEW.</span><span class="sxs-lookup"><span data-stu-id="eec13-140">A CTE can be specified in a CREATE VIEW statement.</span></span>
* <span data-ttu-id="eec13-141">En CTE kan anges i instruktionen Skapa tabell AS Välj (CTAS).</span><span class="sxs-lookup"><span data-stu-id="eec13-141">A CTE can be specified in a CREATE TABLE AS SELECT (CTAS) statement.</span></span>
* <span data-ttu-id="eec13-142">En CTE kan anges i instruktionen skapa REMOTE TABLE AS Välj (CRTAS).</span><span class="sxs-lookup"><span data-stu-id="eec13-142">A CTE can be specified in a CREATE REMOTE TABLE AS SELECT (CRTAS) statement.</span></span>
* <span data-ttu-id="eec13-143">En CTE kan anges i instruktionen Skapa extern tabell AS Välj (CETAS).</span><span class="sxs-lookup"><span data-stu-id="eec13-143">A CTE can be specified in a CREATE EXTERNAL TABLE AS SELECT (CETAS) statement.</span></span>
* <span data-ttu-id="eec13-144">En fjärrtabell kan refereras från en CTE.</span><span class="sxs-lookup"><span data-stu-id="eec13-144">A remote table can be referenced from a CTE.</span></span>
* <span data-ttu-id="eec13-145">En extern tabell kan refereras från en CTE.</span><span class="sxs-lookup"><span data-stu-id="eec13-145">An external table can be referenced from a CTE.</span></span>
* <span data-ttu-id="eec13-146">Flera CTE frågan definitioner kan definieras i en CTE.</span><span class="sxs-lookup"><span data-stu-id="eec13-146">Multiple CTE query definitions can be defined in a CTE.</span></span>

## <a name="cte-limitations"></a><span data-ttu-id="eec13-147">CTE begränsningar</span><span class="sxs-lookup"><span data-stu-id="eec13-147">CTE Limitations</span></span>
<span data-ttu-id="eec13-148">Vanliga tabelluttryck har vissa begränsningar i SQL Data Warehouse, inklusive:</span><span class="sxs-lookup"><span data-stu-id="eec13-148">Common table expressions have some limitations in SQL Data Warehouse including:</span></span>

* <span data-ttu-id="eec13-149">En CTE måste följas av en enstaka SELECT-instruktion.</span><span class="sxs-lookup"><span data-stu-id="eec13-149">A CTE must be followed by a single SELECT statement.</span></span> <span data-ttu-id="eec13-150">INSERT-, UPDATE-, DELETE och MERGE-instruktioner stöds inte.</span><span class="sxs-lookup"><span data-stu-id="eec13-150">INSERT, UPDATE, DELETE, and MERGE statements are not supported.</span></span>
* <span data-ttu-id="eec13-151">Ett vanligt tabelluttryck som innehåller referenser tooitself (ett rekursivt vanligt tabelluttryck) stöds inte (se nedan avsnitt).</span><span class="sxs-lookup"><span data-stu-id="eec13-151">A common table expression that includes references tooitself (a recursive common table expression) is not supported (see below section).</span></span>
* <span data-ttu-id="eec13-152">Ange fler än en med-satsen i en CTE är inte tillåtet.</span><span class="sxs-lookup"><span data-stu-id="eec13-152">Specifying more than one WITH clause in a CTE is not allowed.</span></span> <span data-ttu-id="eec13-153">Om en CTE_query_definition innehåller en underfråga, kan till exempel denna underfråga innehåller en kapslad med-sats som definierar en annan CTE.</span><span class="sxs-lookup"><span data-stu-id="eec13-153">For example, if a CTE_query_definition contains a subquery, that subquery cannot contain a nested WITH clause that defines another CTE.</span></span>
* <span data-ttu-id="eec13-154">En ORDER BY-sats kan inte användas i hello CTE_query_definition, utom när en TOP-instruktion har angetts.</span><span class="sxs-lookup"><span data-stu-id="eec13-154">An ORDER BY clause cannot be used in hello CTE_query_definition, except when a TOP clause is specified.</span></span>
* <span data-ttu-id="eec13-155">När en CTE används i en instruktion som är en del av en batch måste hello instruktionen innan den följas av ett semikolon.</span><span class="sxs-lookup"><span data-stu-id="eec13-155">When a CTE is used in a statement that is part of a batch, hello statement before it must be followed by a semicolon.</span></span>
* <span data-ttu-id="eec13-156">När de används i rapporter med sp_prepare cte-referenser fungerar hello samma sätt som andra SELECT-satser i PDW.</span><span class="sxs-lookup"><span data-stu-id="eec13-156">When used in statements prepared by sp_prepare, CTEs will behave hello same way as other SELECT statements in PDW.</span></span> <span data-ttu-id="eec13-157">Men om cte-referenser används som en del av CETAS förberedda av sp_prepare hello beteende kan skjuta upp från SQL Server och andra PDW-instruktioner på grund av hello sätt bindning har implementerats för sp_prepare.</span><span class="sxs-lookup"><span data-stu-id="eec13-157">However, if CTEs are used as part of CETAS prepared by sp_prepare, hello behavior can defer from SQL Server and other PDW statements because of hello way binding is implemented for sp_prepare.</span></span> <span data-ttu-id="eec13-158">Om väljer att refererar till CTE använder en fel kolumn som inte finns i CTE hello sp_prepare skickas utan att identifiera hello fel, men hello-fel inträffade under sp_execute i stället.</span><span class="sxs-lookup"><span data-stu-id="eec13-158">If SELECT that references CTE is using a wrong column that does not exist in CTE, hello sp_prepare will pass without detecting hello error, but hello error will be thrown during sp_execute instead.</span></span>

## <a name="recursive-ctes"></a><span data-ttu-id="eec13-159">Rekursiva cte-referenser</span><span class="sxs-lookup"><span data-stu-id="eec13-159">Recursive CTEs</span></span>
<span data-ttu-id="eec13-160">Rekursiva cte-referenser stöds inte i SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="eec13-160">Recursive CTEs are not supported in SQL Data Warehouse.</span></span>  <span data-ttu-id="eec13-161">hello migrering av rekursiva CTE kan vara ganska komplicerat och hello bästa processen är toobreak det i flera steg.</span><span class="sxs-lookup"><span data-stu-id="eec13-161">hello migration of recursive CTE can be somewhat complex and hello best process is toobreak it into multiple steps.</span></span> <span data-ttu-id="eec13-162">Du kan normalt använda en loop och fylla i en tillfällig tabell som du iterera över hello rekursiva tillfälliga frågor.</span><span class="sxs-lookup"><span data-stu-id="eec13-162">You can typically use a loop and populate a temporary table as you iterate over hello recursive interim queries.</span></span> <span data-ttu-id="eec13-163">Du kan sedan återvända hello data som en enda resultatmängd när hello tillfällig tabell fylls i.</span><span class="sxs-lookup"><span data-stu-id="eec13-163">Once hello temporary table is populated you can then return hello data as a single result set.</span></span> <span data-ttu-id="eec13-164">Ett liknande sätt har använt toosolve `GROUP BY WITH CUBE` i hello [group by-satser med Samlad / kub / grupperingsalternativ anger] [ group by clause with rollup / cube / grouping sets options] artikel.</span><span class="sxs-lookup"><span data-stu-id="eec13-164">A similar approach has been used toosolve `GROUP BY WITH CUBE` in hello [group by clause with rollup / cube / grouping sets options][group by clause with rollup / cube / grouping sets options] article.</span></span>

## <a name="unsupported-system-functions"></a><span data-ttu-id="eec13-165">Systemfunktioner som inte stöds</span><span class="sxs-lookup"><span data-stu-id="eec13-165">Unsupported system functions</span></span>
<span data-ttu-id="eec13-166">Det finns även vissa systemfunktioner som inte stöds.</span><span class="sxs-lookup"><span data-stu-id="eec13-166">There are also some system functions that are not supported.</span></span> <span data-ttu-id="eec13-167">Några av hello huvudsakliga de kanske vanligtvis används i informationslager är:</span><span class="sxs-lookup"><span data-stu-id="eec13-167">Some of hello main ones you might typically find used in data warehousing are:</span></span>

* <span data-ttu-id="eec13-168">NEWSEQUENTIALID()</span><span class="sxs-lookup"><span data-stu-id="eec13-168">NEWSEQUENTIALID()</span></span>
* <span data-ttu-id="eec13-169">@@NESTLEVEL()</span><span class="sxs-lookup"><span data-stu-id="eec13-169">@@NESTLEVEL()</span></span>
* <span data-ttu-id="eec13-170">@@IDENTITY()</span><span class="sxs-lookup"><span data-stu-id="eec13-170">@@IDENTITY()</span></span>
* <span data-ttu-id="eec13-171">@@ROWCOUNT()</span><span class="sxs-lookup"><span data-stu-id="eec13-171">@@ROWCOUNT()</span></span>
* <span data-ttu-id="eec13-172">ROWCOUNT_BIG</span><span class="sxs-lookup"><span data-stu-id="eec13-172">ROWCOUNT_BIG</span></span>
* <span data-ttu-id="eec13-173">ERROR_LINE()</span><span class="sxs-lookup"><span data-stu-id="eec13-173">ERROR_LINE()</span></span>

<span data-ttu-id="eec13-174">Vissa av dessa problem kan du arbetade runt.</span><span class="sxs-lookup"><span data-stu-id="eec13-174">Some of these issues can be worked around.</span></span>

## <a name="rowcount-workaround"></a><span data-ttu-id="eec13-175">@@ROWCOUNT lösning</span><span class="sxs-lookup"><span data-stu-id="eec13-175">@@ROWCOUNT workaround</span></span>
<span data-ttu-id="eec13-176">toowork runt bristande stöd för @@ROWCOUNT, skapa en lagrad procedur som hämtar hello senaste radantal från sys.dm_pdw_request_steps och sedan köra `EXEC LastRowCount` efter en DML-instruktionen.</span><span class="sxs-lookup"><span data-stu-id="eec13-176">toowork around lack of support for @@ROWCOUNT, create a stored procedure that will retrieve hello last row count from sys.dm_pdw_request_steps and then execute `EXEC LastRowCount` after a DML statement.</span></span>

```sql
CREATE PROCEDURE LastRowCount AS
WITH LastRequest as 
(   SELECT TOP 1    request_id
    FROM            sys.dm_pdw_exec_requests
    WHERE           session_id = SESSION_ID()
    AND             resource_class IS NOT NULL
    ORDER BY end_time DESC
),
LastRequestRowCounts as
(
    SELECT  step_index, row_count
    FROM    sys.dm_pdw_request_steps
    WHERE   row_count >= 0
    AND     request_id IN (SELECT request_id from LastRequest)
)
SELECT TOP 1 row_count FROM LastRequestRowCounts ORDER BY step_index DESC
;
```

## <a name="next-steps"></a><span data-ttu-id="eec13-177">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="eec13-177">Next steps</span></span>
<span data-ttu-id="eec13-178">En fullständig lista över alla T-SQL-instruktioner som stöds finns i [Transact-SQL-avsnitt][Transact-SQL topics].</span><span class="sxs-lookup"><span data-stu-id="eec13-178">For a complete list of all supported T-SQL statements, see [Transact-SQL topics][Transact-SQL topics].</span></span>

<!--Image references-->

<!--Article references-->
[ANSI joins on updates]: ./sql-data-warehouse-develop-ctas.md#ansi-join-replacement-for-update-statements
[ANSI joins on deletes]: ./sql-data-warehouse-develop-ctas.md#ansi-join-replacement-for-delete-statements
[merge statement]: ./sql-data-warehouse-develop-ctas.md#replace-merge-statements
[INSERT..EXEC]: ./sql-data-warehouse-tables-temporary.md#modularizing-code
[Transact-SQL topics]: ./sql-data-warehouse-reference-tsql-statements.md

[cursors]: ./sql-data-warehouse-develop-loops.md
[group by clause with rollup / cube / grouping sets options]: ./sql-data-warehouse-develop-group-by-options.md
[nesting levels beyond 8]: ./sql-data-warehouse-develop-transactions.md
[updating through views]: ./sql-data-warehouse-develop-views.md
[use of select for variable assignment]: ./sql-data-warehouse-develop-variable-assignment.md
[no MAX data type for dynamic SQL strings]: ./sql-data-warehouse-develop-dynamic-sql.md

<!--MSDN references-->

<!--Other Web references-->
