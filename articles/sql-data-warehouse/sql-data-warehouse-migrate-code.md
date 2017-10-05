---
title: Migrera dina SQL-kod till SQL Data Warehouse | Microsoft Docs
description: "Tips för att migrera SQL-kod till Azure SQL Data Warehouse för utveckling av lösningar."
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
ms.openlocfilehash: c6e6b890f5e2d0e31b10bbb6803adad02bf60248
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="migrate-your-sql-code-to-sql-data-warehouse"></a><span data-ttu-id="d2e42-103">Migrera dina SQL-kod till SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="d2e42-103">Migrate your SQL code to SQL Data Warehouse</span></span>
<span data-ttu-id="d2e42-104">Den här artikeln förklarar kodändringar behöver du antagligen se när du migrerar din kod från en annan databas till SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="d2e42-104">This article explains code changes you will probably need to make when migrating your code from another database to SQL Data Warehouse.</span></span> <span data-ttu-id="d2e42-105">Vissa funktioner i SQL Data Warehouse förbättrar prestanda eftersom de är avsedda att fungera i ett distribuerat sätt.</span><span class="sxs-lookup"><span data-stu-id="d2e42-105">Some SQL Data Warehouse features can significantly improve performance as they are designed to work in a distributed fashion.</span></span> <span data-ttu-id="d2e42-106">För att upprätthålla prestanda och skalning så är vissa funktioner dock också inte tillgänglig.</span><span class="sxs-lookup"><span data-stu-id="d2e42-106">However, to maintain performance and scale, some features are also not available.</span></span>

## <a name="common-t-sql-limitations"></a><span data-ttu-id="d2e42-107">Vanliga T-SQL-begränsningar</span><span class="sxs-lookup"><span data-stu-id="d2e42-107">Common T-SQL Limitations</span></span>
<span data-ttu-id="d2e42-108">I följande lista sammanfattas de vanligaste funktionerna som inte har stöd för SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="d2e42-108">The following list summarizes the most common features that SQL Data Warehouse does not support.</span></span> <span data-ttu-id="d2e42-109">Länkarna om du vill lösningar för funktionerna som inte stöds:</span><span class="sxs-lookup"><span data-stu-id="d2e42-109">The links take you to workarounds for the unsupported features:</span></span>

* <span data-ttu-id="d2e42-110">[ANSI kopplingar uppdateringar][ANSI joins on updates]</span><span class="sxs-lookup"><span data-stu-id="d2e42-110">[ANSI joins on updates][ANSI joins on updates]</span></span>
* <span data-ttu-id="d2e42-111">[ANSI-kopplingar på borttagningar][ANSI joins on deletes]</span><span class="sxs-lookup"><span data-stu-id="d2e42-111">[ANSI joins on deletes][ANSI joins on deletes]</span></span>
* <span data-ttu-id="d2e42-112">[merge-instruktion][merge statement]</span><span class="sxs-lookup"><span data-stu-id="d2e42-112">[merge statement][merge statement]</span></span>
* <span data-ttu-id="d2e42-113">flera databaser kopplingar</span><span class="sxs-lookup"><span data-stu-id="d2e42-113">cross-database joins</span></span>
* <span data-ttu-id="d2e42-114">[markörer][cursors]</span><span class="sxs-lookup"><span data-stu-id="d2e42-114">[cursors][cursors]</span></span>
* <span data-ttu-id="d2e42-115">[INSERT... EXEC][INSERT..EXEC]</span><span class="sxs-lookup"><span data-stu-id="d2e42-115">[INSERT..EXEC][INSERT..EXEC]</span></span>
* <span data-ttu-id="d2e42-116">Output-sats</span><span class="sxs-lookup"><span data-stu-id="d2e42-116">output clause</span></span>
* <span data-ttu-id="d2e42-117">infogade användardefinierade funktioner</span><span class="sxs-lookup"><span data-stu-id="d2e42-117">inline user-defined functions</span></span>
* <span data-ttu-id="d2e42-118">flera instruktioner funktioner</span><span class="sxs-lookup"><span data-stu-id="d2e42-118">multi-statement functions</span></span>
* [<span data-ttu-id="d2e42-119">vanliga tabelluttryck</span><span class="sxs-lookup"><span data-stu-id="d2e42-119">common table expressions</span></span>](#Common-table-expressions)
* <span data-ttu-id="d2e42-120">[rekursiva cte (CTE)] (#Recursive-common-table-expressions-(CTE)</span><span class="sxs-lookup"><span data-stu-id="d2e42-120">[recursive common table expressions (CTE)](#Recursive-common-table-expressions-(CTE)</span></span>
* <span data-ttu-id="d2e42-121">CLR-funktioner och procedurer</span><span class="sxs-lookup"><span data-stu-id="d2e42-121">CLR functions and procedures</span></span>
* <span data-ttu-id="d2e42-122">$partition funktion</span><span class="sxs-lookup"><span data-stu-id="d2e42-122">$partition function</span></span>
* <span data-ttu-id="d2e42-123">tabellvariabler</span><span class="sxs-lookup"><span data-stu-id="d2e42-123">table variables</span></span>
* <span data-ttu-id="d2e42-124">tabellen värdet parametrar</span><span class="sxs-lookup"><span data-stu-id="d2e42-124">table value parameters</span></span>
* <span data-ttu-id="d2e42-125">distribuerade transaktioner</span><span class="sxs-lookup"><span data-stu-id="d2e42-125">distributed transactions</span></span>
* <span data-ttu-id="d2e42-126">Commit / rollback arbete</span><span class="sxs-lookup"><span data-stu-id="d2e42-126">commit / rollback work</span></span>
* <span data-ttu-id="d2e42-127">Spara transaktionen</span><span class="sxs-lookup"><span data-stu-id="d2e42-127">save transaction</span></span>
* <span data-ttu-id="d2e42-128">körningen kontexter (EXECUTE AS)</span><span class="sxs-lookup"><span data-stu-id="d2e42-128">execution contexts (EXECUTE AS)</span></span>
* <span data-ttu-id="d2e42-129">[Group by-satser med Samlad / kub / grupperingsalternativ uppsättningar][group by clause with rollup / cube / grouping sets options]</span><span class="sxs-lookup"><span data-stu-id="d2e42-129">[group by clause with rollup / cube / grouping sets options][group by clause with rollup / cube / grouping sets options]</span></span>
* <span data-ttu-id="d2e42-130">[kapslingsnivåer utöver 8][nesting levels beyond 8]</span><span class="sxs-lookup"><span data-stu-id="d2e42-130">[nesting levels beyond 8][nesting levels beyond 8]</span></span>
* <span data-ttu-id="d2e42-131">[uppdatering i vyer][updating through views]</span><span class="sxs-lookup"><span data-stu-id="d2e42-131">[updating through views][updating through views]</span></span>
* <span data-ttu-id="d2e42-132">[användning av väljer för variabeltilldelning][use of select for variable assignment]</span><span class="sxs-lookup"><span data-stu-id="d2e42-132">[use of select for variable assignment][use of select for variable assignment]</span></span>
* <span data-ttu-id="d2e42-133">[Ingen MAX datatyp för dynamisk SQL-strängar][no MAX data type for dynamic SQL strings]</span><span class="sxs-lookup"><span data-stu-id="d2e42-133">[no MAX data type for dynamic SQL strings][no MAX data type for dynamic SQL strings]</span></span>

<span data-ttu-id="d2e42-134">De flesta av dessa begränsningar kan Lyckligtvis arbetat runt.</span><span class="sxs-lookup"><span data-stu-id="d2e42-134">Fortunately most of these limitations can be worked around.</span></span> <span data-ttu-id="d2e42-135">Förklaringar finns i de relevanta development artiklar som anges ovan.</span><span class="sxs-lookup"><span data-stu-id="d2e42-135">Explanations are provided in the relevant development articles referenced above.</span></span>

## <a name="supported-cte-features"></a><span data-ttu-id="d2e42-136">CTE-funktioner som stöds</span><span class="sxs-lookup"><span data-stu-id="d2e42-136">Supported CTE features</span></span>
<span data-ttu-id="d2e42-137">Cte (cte-referenser) stöds delvis i SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="d2e42-137">Common table expressions (CTEs) are partially supported in SQL Data Warehouse.</span></span>  <span data-ttu-id="d2e42-138">Följande CTE-funktioner stöds:</span><span class="sxs-lookup"><span data-stu-id="d2e42-138">The following CTE features are currently supported:</span></span>

* <span data-ttu-id="d2e42-139">En CTE kan anges i en SELECT-instruktion.</span><span class="sxs-lookup"><span data-stu-id="d2e42-139">A CTE can be specified in a SELECT statement.</span></span>
* <span data-ttu-id="d2e42-140">En CTE kan anges i instruktionen CREATE VIEW.</span><span class="sxs-lookup"><span data-stu-id="d2e42-140">A CTE can be specified in a CREATE VIEW statement.</span></span>
* <span data-ttu-id="d2e42-141">En CTE kan anges i instruktionen Skapa tabell AS Välj (CTAS).</span><span class="sxs-lookup"><span data-stu-id="d2e42-141">A CTE can be specified in a CREATE TABLE AS SELECT (CTAS) statement.</span></span>
* <span data-ttu-id="d2e42-142">En CTE kan anges i instruktionen skapa REMOTE TABLE AS Välj (CRTAS).</span><span class="sxs-lookup"><span data-stu-id="d2e42-142">A CTE can be specified in a CREATE REMOTE TABLE AS SELECT (CRTAS) statement.</span></span>
* <span data-ttu-id="d2e42-143">En CTE kan anges i instruktionen Skapa extern tabell AS Välj (CETAS).</span><span class="sxs-lookup"><span data-stu-id="d2e42-143">A CTE can be specified in a CREATE EXTERNAL TABLE AS SELECT (CETAS) statement.</span></span>
* <span data-ttu-id="d2e42-144">En fjärrtabell kan refereras från en CTE.</span><span class="sxs-lookup"><span data-stu-id="d2e42-144">A remote table can be referenced from a CTE.</span></span>
* <span data-ttu-id="d2e42-145">En extern tabell kan refereras från en CTE.</span><span class="sxs-lookup"><span data-stu-id="d2e42-145">An external table can be referenced from a CTE.</span></span>
* <span data-ttu-id="d2e42-146">Flera CTE frågan definitioner kan definieras i en CTE.</span><span class="sxs-lookup"><span data-stu-id="d2e42-146">Multiple CTE query definitions can be defined in a CTE.</span></span>

## <a name="cte-limitations"></a><span data-ttu-id="d2e42-147">CTE begränsningar</span><span class="sxs-lookup"><span data-stu-id="d2e42-147">CTE Limitations</span></span>
<span data-ttu-id="d2e42-148">Vanliga tabelluttryck har vissa begränsningar i SQL Data Warehouse, inklusive:</span><span class="sxs-lookup"><span data-stu-id="d2e42-148">Common table expressions have some limitations in SQL Data Warehouse including:</span></span>

* <span data-ttu-id="d2e42-149">En CTE måste följas av en enstaka SELECT-instruktion.</span><span class="sxs-lookup"><span data-stu-id="d2e42-149">A CTE must be followed by a single SELECT statement.</span></span> <span data-ttu-id="d2e42-150">INSERT-, UPDATE-, DELETE och MERGE-instruktioner stöds inte.</span><span class="sxs-lookup"><span data-stu-id="d2e42-150">INSERT, UPDATE, DELETE, and MERGE statements are not supported.</span></span>
* <span data-ttu-id="d2e42-151">Ett vanligt tabelluttryck som innehåller referenser till sig själv (ett rekursivt vanligt tabelluttryck) stöds inte (se nedan avsnitt).</span><span class="sxs-lookup"><span data-stu-id="d2e42-151">A common table expression that includes references to itself (a recursive common table expression) is not supported (see below section).</span></span>
* <span data-ttu-id="d2e42-152">Ange fler än en med-satsen i en CTE är inte tillåtet.</span><span class="sxs-lookup"><span data-stu-id="d2e42-152">Specifying more than one WITH clause in a CTE is not allowed.</span></span> <span data-ttu-id="d2e42-153">Om en CTE_query_definition innehåller en underfråga, kan till exempel denna underfråga innehåller en kapslad med-sats som definierar en annan CTE.</span><span class="sxs-lookup"><span data-stu-id="d2e42-153">For example, if a CTE_query_definition contains a subquery, that subquery cannot contain a nested WITH clause that defines another CTE.</span></span>
* <span data-ttu-id="d2e42-154">En ORDER BY-sats kan inte användas i CTE_query_definition, utom när en TOP-instruktion har angetts.</span><span class="sxs-lookup"><span data-stu-id="d2e42-154">An ORDER BY clause cannot be used in the CTE_query_definition, except when a TOP clause is specified.</span></span>
* <span data-ttu-id="d2e42-155">När en CTE används i en instruktion som är en del av en batch måste instruktionen innan den följas av ett semikolon.</span><span class="sxs-lookup"><span data-stu-id="d2e42-155">When a CTE is used in a statement that is part of a batch, the statement before it must be followed by a semicolon.</span></span>
* <span data-ttu-id="d2e42-156">När de används i rapporter med sp_prepare fungerar cte-referenser på samma sätt som andra SELECT-satser i PDW.</span><span class="sxs-lookup"><span data-stu-id="d2e42-156">When used in statements prepared by sp_prepare, CTEs will behave the same way as other SELECT statements in PDW.</span></span> <span data-ttu-id="d2e42-157">Men om cte-referenser används som en del av CETAS förberedda av sp_prepare beteendet kan skjuta upp från SQL Server och andra PDW-instruktioner på grund av hur bindning har implementerats för sp_prepare.</span><span class="sxs-lookup"><span data-stu-id="d2e42-157">However, if CTEs are used as part of CETAS prepared by sp_prepare, the behavior can defer from SQL Server and other PDW statements because of the way binding is implemented for sp_prepare.</span></span> <span data-ttu-id="d2e42-158">Om väljer att refererar till CTE använder en fel kolumn som inte finns i CTE, sp_prepare skickas utan att identifiera felet, men felet uppstod under sp_execute i stället.</span><span class="sxs-lookup"><span data-stu-id="d2e42-158">If SELECT that references CTE is using a wrong column that does not exist in CTE, the sp_prepare will pass without detecting the error, but the error will be thrown during sp_execute instead.</span></span>

## <a name="recursive-ctes"></a><span data-ttu-id="d2e42-159">Rekursiva cte-referenser</span><span class="sxs-lookup"><span data-stu-id="d2e42-159">Recursive CTEs</span></span>
<span data-ttu-id="d2e42-160">Rekursiva cte-referenser stöds inte i SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="d2e42-160">Recursive CTEs are not supported in SQL Data Warehouse.</span></span>  <span data-ttu-id="d2e42-161">Migrering av rekursiva CTE kan vara ganska komplicerat och på bästa sätt är att dela upp det i flera steg.</span><span class="sxs-lookup"><span data-stu-id="d2e42-161">The migration of recursive CTE can be somewhat complex and the best process is to break it into multiple steps.</span></span> <span data-ttu-id="d2e42-162">Du kan normalt använda en loop och fylla i en tillfällig tabell som du iterera över rekursiva tillfälliga frågor.</span><span class="sxs-lookup"><span data-stu-id="d2e42-162">You can typically use a loop and populate a temporary table as you iterate over the recursive interim queries.</span></span> <span data-ttu-id="d2e42-163">Du kan sedan returnera data som en enda resultatmängd när den temporära tabellen fylls i.</span><span class="sxs-lookup"><span data-stu-id="d2e42-163">Once the temporary table is populated you can then return the data as a single result set.</span></span> <span data-ttu-id="d2e42-164">Ett liknande sätt har använts för att lösa `GROUP BY WITH CUBE` i den [group by-satser med Samlad / kub / grupperingsalternativ anger] [ group by clause with rollup / cube / grouping sets options] artikel.</span><span class="sxs-lookup"><span data-stu-id="d2e42-164">A similar approach has been used to solve `GROUP BY WITH CUBE` in the [group by clause with rollup / cube / grouping sets options][group by clause with rollup / cube / grouping sets options] article.</span></span>

## <a name="unsupported-system-functions"></a><span data-ttu-id="d2e42-165">Systemfunktioner som inte stöds</span><span class="sxs-lookup"><span data-stu-id="d2e42-165">Unsupported system functions</span></span>
<span data-ttu-id="d2e42-166">Det finns även vissa systemfunktioner som inte stöds.</span><span class="sxs-lookup"><span data-stu-id="d2e42-166">There are also some system functions that are not supported.</span></span> <span data-ttu-id="d2e42-167">Några av de huvudsakliga som kanske vanligtvis används i informationslager är:</span><span class="sxs-lookup"><span data-stu-id="d2e42-167">Some of the main ones you might typically find used in data warehousing are:</span></span>

* <span data-ttu-id="d2e42-168">NEWSEQUENTIALID()</span><span class="sxs-lookup"><span data-stu-id="d2e42-168">NEWSEQUENTIALID()</span></span>
* <span data-ttu-id="d2e42-169">@@NESTLEVEL()</span><span class="sxs-lookup"><span data-stu-id="d2e42-169">@@NESTLEVEL()</span></span>
* <span data-ttu-id="d2e42-170">@@IDENTITY()</span><span class="sxs-lookup"><span data-stu-id="d2e42-170">@@IDENTITY()</span></span>
* <span data-ttu-id="d2e42-171">@@ROWCOUNT()</span><span class="sxs-lookup"><span data-stu-id="d2e42-171">@@ROWCOUNT()</span></span>
* <span data-ttu-id="d2e42-172">ROWCOUNT_BIG</span><span class="sxs-lookup"><span data-stu-id="d2e42-172">ROWCOUNT_BIG</span></span>
* <span data-ttu-id="d2e42-173">ERROR_LINE()</span><span class="sxs-lookup"><span data-stu-id="d2e42-173">ERROR_LINE()</span></span>

<span data-ttu-id="d2e42-174">Vissa av dessa problem kan du arbetade runt.</span><span class="sxs-lookup"><span data-stu-id="d2e42-174">Some of these issues can be worked around.</span></span>

## <a name="rowcount-workaround"></a><span data-ttu-id="d2e42-175">@@ROWCOUNT lösning</span><span class="sxs-lookup"><span data-stu-id="d2e42-175">@@ROWCOUNT workaround</span></span>
<span data-ttu-id="d2e42-176">Undvika bristande stöd för @@ROWCOUNT, skapa en lagrad procedur som hämtar det senaste antalet rader från sys.dm_pdw_request_steps och sedan köra `EXEC LastRowCount` efter en DML-instruktionen.</span><span class="sxs-lookup"><span data-stu-id="d2e42-176">To work around lack of support for @@ROWCOUNT, create a stored procedure that will retrieve the last row count from sys.dm_pdw_request_steps and then execute `EXEC LastRowCount` after a DML statement.</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="d2e42-177">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="d2e42-177">Next steps</span></span>
<span data-ttu-id="d2e42-178">En fullständig lista över alla T-SQL-instruktioner som stöds finns i [Transact-SQL-avsnitt][Transact-SQL topics].</span><span class="sxs-lookup"><span data-stu-id="d2e42-178">For a complete list of all supported T-SQL statements, see [Transact-SQL topics][Transact-SQL topics].</span></span>

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
