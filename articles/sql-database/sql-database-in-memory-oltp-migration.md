---
title: "aaaIn minne OLTP förbättrar prestanda för SQL-txn | Microsoft Docs"
description: Anta Minnesintern OLTP tooimprove prestanda i en befintlig SQL-databas.
services: sql-database
documentationcenter: 
author: jodebrui
manager: jhubbard
editor: MightyPen
ms.assetid: c2bf14a0-905b-47b4-afb6-efe9a61147d5
ms.service: sql-database
ms.custom: develop databases
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/22/2016
ms.author: jodebrui
ms.openlocfilehash: 1bed796069565eb6741953837cf0477c2ddd8519
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-in-memory-oltp-tooimprove-your-application-performance-in-sql-database"></a><span data-ttu-id="0db69-103">Använd Minnesintern OLTP tooimprove ditt programprestanda i SQL-databas</span><span class="sxs-lookup"><span data-stu-id="0db69-103">Use In-Memory OLTP tooimprove your application performance in SQL Database</span></span>
<span data-ttu-id="0db69-104">[Minnesintern OLTP](sql-database-in-memory.md) kan ha använt tooimprove hello prestanda för transaktionsbearbetning, datapåfyllning och tillfälligt datascenarier [Premium](sql-database-service-tiers.md) Azure SQL-databaser utan att öka hello prisnivå.</span><span class="sxs-lookup"><span data-stu-id="0db69-104">[In-Memory OLTP](sql-database-in-memory.md) can be used tooimprove hello performance of transaction processing, data ingestion, and transient data scenarios, in  [Premium](sql-database-service-tiers.md) Azure SQL Databases without increasing hello pricing tier.</span></span> 

> [!NOTE] 
> <span data-ttu-id="0db69-105">Lär dig hur [kvorum fördubblar viktiga databasen arbetsbelastning och sänka DTU med 70% med SQL-databas](https://customers.microsoft.com/story/quorum-doubles-key-databases-workload-while-lowering-dtu-with-sql-database)</span><span class="sxs-lookup"><span data-stu-id="0db69-105">Learn how [Quorum doubles key database’s workload while lowering DTU by 70% with SQL Database](https://customers.microsoft.com/story/quorum-doubles-key-databases-workload-while-lowering-dtu-with-sql-database)</span></span>


<span data-ttu-id="0db69-106">Följ dessa steg tooadopt Minnesintern OLTP i den befintliga databasen.</span><span class="sxs-lookup"><span data-stu-id="0db69-106">Follow these steps tooadopt In-Memory OLTP in your existing database.</span></span>

## <a name="step-1-ensure-you-are-using-a-premium-database"></a><span data-ttu-id="0db69-107">Steg 1: Kontrollera att du använder en Premium-databas</span><span class="sxs-lookup"><span data-stu-id="0db69-107">Step 1: Ensure you are using a Premium database</span></span>
<span data-ttu-id="0db69-108">Minnesintern OLTP stöds endast i Premium-databaser.</span><span class="sxs-lookup"><span data-stu-id="0db69-108">In-Memory OLTP is supported only in Premium databases.</span></span> <span data-ttu-id="0db69-109">I minnet stöds om hello returnerade resultatet är 1 (inte 0):</span><span class="sxs-lookup"><span data-stu-id="0db69-109">In-Memory is supported if hello returned result is 1 (not 0):</span></span>

```
SELECT DatabasePropertyEx(Db_Name(), 'IsXTPSupported');
```

<span data-ttu-id="0db69-110">*XTP* står för *extrema transaktionsbearbetning*</span><span class="sxs-lookup"><span data-stu-id="0db69-110">*XTP* stands for *Extreme Transaction Processing*</span></span>



## <a name="step-2-identify-objects-toomigrate-tooin-memory-oltp"></a><span data-ttu-id="0db69-111">Steg 2: Identifiera objekt toomigrate tooIn minnet OLTP</span><span class="sxs-lookup"><span data-stu-id="0db69-111">Step 2: Identify objects toomigrate tooIn-Memory OLTP</span></span>
<span data-ttu-id="0db69-112">SSMS innehåller en **översikt över transaktion prestanda Analysis** rapport som du kan köra mot en databas med en aktiv arbetsbelastning.</span><span class="sxs-lookup"><span data-stu-id="0db69-112">SSMS includes a **Transaction Performance Analysis Overview** report that you can run against a database with an active workload.</span></span> <span data-ttu-id="0db69-113">hello rapporten identifieras tabeller och lagrade procedurer som lämpar sig för migrering tooIn minne OLTP.</span><span class="sxs-lookup"><span data-stu-id="0db69-113">hello report identifies tables and stored procedures that are candidates for migration tooIn-Memory OLTP.</span></span>

<span data-ttu-id="0db69-114">I SSMS toogenerate hello rapporten:</span><span class="sxs-lookup"><span data-stu-id="0db69-114">In SSMS, toogenerate hello report:</span></span>

* <span data-ttu-id="0db69-115">I hello **Object Explorer**, högerklicka på databasnoden för din.</span><span class="sxs-lookup"><span data-stu-id="0db69-115">In hello **Object Explorer**, right-click your database node.</span></span>
* <span data-ttu-id="0db69-116">Klicka på **rapporter** > **standardrapporter** > **översikt över transaktion prestanda Analysis**.</span><span class="sxs-lookup"><span data-stu-id="0db69-116">Click **Reports** > **Standard Reports** > **Transaction Performance Analysis Overview**.</span></span>

<span data-ttu-id="0db69-117">Mer information finns i [fastställa om en tabell eller en lagrad procedur ska flyttas tooIn minne OLTP](http://msdn.microsoft.com/library/dn205133.aspx).</span><span class="sxs-lookup"><span data-stu-id="0db69-117">For more information, see [Determining if a Table or Stored Procedure Should Be Ported tooIn-Memory OLTP](http://msdn.microsoft.com/library/dn205133.aspx).</span></span>

## <a name="step-3-create-a-comparable-test-database"></a><span data-ttu-id="0db69-118">Steg 3: Skapa en motsvarande testdatabas</span><span class="sxs-lookup"><span data-stu-id="0db69-118">Step 3: Create a comparable test database</span></span>
<span data-ttu-id="0db69-119">Anta att hello rapporten visar att databasen har en tabell som har nytta som konverteras tooa minnesoptimerade tabeller.</span><span class="sxs-lookup"><span data-stu-id="0db69-119">Suppose hello report indicates your database has a table that would benefit from being converted tooa memory-optimized table.</span></span> <span data-ttu-id="0db69-120">Vi rekommenderar att du först testar tooconfirm hello uppgift genom att testa.</span><span class="sxs-lookup"><span data-stu-id="0db69-120">We recommend that you first test tooconfirm hello indication by testing.</span></span>

<span data-ttu-id="0db69-121">Du behöver en test kopia av produktionsdatabasen.</span><span class="sxs-lookup"><span data-stu-id="0db69-121">You need a test copy of your production database.</span></span> <span data-ttu-id="0db69-122">hello testdatabasen ska finnas på hello tjänsten samma nivå som produktionsdatabasen.</span><span class="sxs-lookup"><span data-stu-id="0db69-122">hello test database should be at hello same service tier level as your production database.</span></span>

<span data-ttu-id="0db69-123">tooease testning, justera testa databasen på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="0db69-123">tooease testing, tweak your test database as follows:</span></span>

1. <span data-ttu-id="0db69-124">Anslut toohello Testa databas med SSMS.</span><span class="sxs-lookup"><span data-stu-id="0db69-124">Connect toohello test database by using SSMS.</span></span>
2. <span data-ttu-id="0db69-125">tooavoid behöver hello alternativet WITH (SNAPSHOT) i frågor genom att ange hello databasalternativet enligt hello följande T-SQL-instruktion:</span><span class="sxs-lookup"><span data-stu-id="0db69-125">tooavoid needing hello WITH (SNAPSHOT) option in queries, set hello database option as shown in hello following T-SQL statement:</span></span>
   
   ```
   ALTER DATABASE CURRENT
    SET
        MEMORY_OPTIMIZED_ELEVATE_TO_SNAPSHOT = ON;
   ```

## <a name="step-4-migrate-tables"></a><span data-ttu-id="0db69-126">Steg 4: Migrera tabeller</span><span class="sxs-lookup"><span data-stu-id="0db69-126">Step 4: Migrate tables</span></span>
<span data-ttu-id="0db69-127">Du måste skapa och fylla i en minnesoptimerad kopia hello tabell som du vill tootest.</span><span class="sxs-lookup"><span data-stu-id="0db69-127">You must create and populate a memory-optimized copy of hello table you want tootest.</span></span> <span data-ttu-id="0db69-128">Du kan skapa den med hjälp av antingen:</span><span class="sxs-lookup"><span data-stu-id="0db69-128">You can create it by using either:</span></span>

* <span data-ttu-id="0db69-129">hello praktiska minne optimering guiden i SSMS.</span><span class="sxs-lookup"><span data-stu-id="0db69-129">hello handy Memory Optimization Wizard in SSMS.</span></span>
* <span data-ttu-id="0db69-130">Manuell T-SQL.</span><span class="sxs-lookup"><span data-stu-id="0db69-130">Manual T-SQL.</span></span>

#### <a name="memory-optimization-wizard-in-ssms"></a><span data-ttu-id="0db69-131">Guiden för optimering av minne i SSMS</span><span class="sxs-lookup"><span data-stu-id="0db69-131">Memory Optimization Wizard in SSMS</span></span>
<span data-ttu-id="0db69-132">toouse det här alternativet för migrering:</span><span class="sxs-lookup"><span data-stu-id="0db69-132">toouse this migration option:</span></span>

1. <span data-ttu-id="0db69-133">Ansluta toohello Testa databas med SSMS.</span><span class="sxs-lookup"><span data-stu-id="0db69-133">Connect toohello test database with SSMS.</span></span>
2. <span data-ttu-id="0db69-134">I hello **Object Explorer**, högerklicka på hello tabell och klicka sedan på **minne optimering Advisor**.</span><span class="sxs-lookup"><span data-stu-id="0db69-134">In hello **Object Explorer**, right-click on hello table, and then click **Memory Optimization Advisor**.</span></span>
   
   * <span data-ttu-id="0db69-135">Hej **tabell minne optimering Advisor** guiden visas.</span><span class="sxs-lookup"><span data-stu-id="0db69-135">hello **Table Memory Optimizer Advisor** wizard is displayed.</span></span>
3. <span data-ttu-id="0db69-136">Klicka på hello guiden **migrering validering** (eller hello **nästa** knappen) toosee om hello tabell har någon stöds inte funktioner som inte stöds i minnesoptimerade tabeller.</span><span class="sxs-lookup"><span data-stu-id="0db69-136">In hello wizard, click **Migration validation** (or hello **Next** button) toosee if hello table has any unsupported features that are unsupported in memory-optimized tables.</span></span> <span data-ttu-id="0db69-137">Mer information finns i:</span><span class="sxs-lookup"><span data-stu-id="0db69-137">For more information, see:</span></span>
   
   * <span data-ttu-id="0db69-138">Hej *minne optimering checklista* i [minne optimering Advisor](http://msdn.microsoft.com/library/dn284308.aspx).</span><span class="sxs-lookup"><span data-stu-id="0db69-138">hello *memory optimization checklist* in [Memory Optimization Advisor](http://msdn.microsoft.com/library/dn284308.aspx).</span></span>
   * <span data-ttu-id="0db69-139">[Transact-SQL-konstruktioner som inte stöds av Minnesintern OLTP](http://msdn.microsoft.com/library/dn246937.aspx).</span><span class="sxs-lookup"><span data-stu-id="0db69-139">[Transact-SQL Constructs Not Supported by In-Memory OLTP](http://msdn.microsoft.com/library/dn246937.aspx).</span></span>
   * <span data-ttu-id="0db69-140">[Migrera tooIn minne OLTP](http://msdn.microsoft.com/library/dn247639.aspx).</span><span class="sxs-lookup"><span data-stu-id="0db69-140">[Migrating tooIn-Memory OLTP](http://msdn.microsoft.com/library/dn247639.aspx).</span></span>
4. <span data-ttu-id="0db69-141">Om hello tabellen har inga funktioner som inte stöds, utföra hello advisor hello faktiska schemat och datamigrering åt dig.</span><span class="sxs-lookup"><span data-stu-id="0db69-141">If hello table has no unsupported features, hello advisor can perform hello actual schema and data migration for you.</span></span>

#### <a name="manual-t-sql"></a><span data-ttu-id="0db69-142">Manuell T-SQL</span><span class="sxs-lookup"><span data-stu-id="0db69-142">Manual T-SQL</span></span>
<span data-ttu-id="0db69-143">toouse det här alternativet för migrering:</span><span class="sxs-lookup"><span data-stu-id="0db69-143">toouse this migration option:</span></span>

1. <span data-ttu-id="0db69-144">Anslut tooyour Testa databas med SSMS (eller en liknande funktion).</span><span class="sxs-lookup"><span data-stu-id="0db69-144">Connect tooyour test database by using SSMS (or a similar utility).</span></span>
2. <span data-ttu-id="0db69-145">Hämta hello fullständig T-SQL-skript för tabellen och dess index.</span><span class="sxs-lookup"><span data-stu-id="0db69-145">Obtain hello complete T-SQL script for your table and its indexes.</span></span>
   
   * <span data-ttu-id="0db69-146">Högerklicka på noden tabell i SSMS.</span><span class="sxs-lookup"><span data-stu-id="0db69-146">In SSMS, right-click your table node.</span></span>
   * <span data-ttu-id="0db69-147">Klicka på **skript tabell som** > **skapa till** > **nytt frågefönster**.</span><span class="sxs-lookup"><span data-stu-id="0db69-147">Click **Script Table As** > **CREATE To** > **New Query Window**.</span></span>
3. <span data-ttu-id="0db69-148">Lägga till WITH hello skriptfönstret (MEMORY_OPTIMIZED = ON) toohello CREATE TABLE-instruktion.</span><span class="sxs-lookup"><span data-stu-id="0db69-148">In hello script window, add WITH (MEMORY_OPTIMIZED = ON) toohello CREATE TABLE statement.</span></span>
4. <span data-ttu-id="0db69-149">Om det finns ett KLUSTRADE index måste du ändra den tooNONCLUSTERED.</span><span class="sxs-lookup"><span data-stu-id="0db69-149">If there is a CLUSTERED index, change it tooNONCLUSTERED.</span></span>
5. <span data-ttu-id="0db69-150">Byt namn på hello befintlig tabell med hjälp av SP_RENAME.</span><span class="sxs-lookup"><span data-stu-id="0db69-150">Rename hello existing table by using SP_RENAME.</span></span>
6. <span data-ttu-id="0db69-151">Skapa hello ny minnesoptimerade kopia av hello tabell genom att köra skriptet redigerade CREATE TABLE.</span><span class="sxs-lookup"><span data-stu-id="0db69-151">Create hello new memory-optimized copy of hello table by running your edited CREATE TABLE script.</span></span>
7. <span data-ttu-id="0db69-152">Kopiera hello tooyour minnesoptimerade tabellen med hjälp av INSERT... VÄLJ * TILL:</span><span class="sxs-lookup"><span data-stu-id="0db69-152">Copy hello data tooyour memory-optimized table by using INSERT...SELECT * INTO:</span></span>

```
INSERT INTO <new_memory_optimized_table>
        SELECT * FROM <old_disk_based_table>;
```


## <a name="step-5-optional-migrate-stored-procedures"></a><span data-ttu-id="0db69-153">Steg 5 (valfritt): migrera lagrade procedurer</span><span class="sxs-lookup"><span data-stu-id="0db69-153">Step 5 (optional): Migrate stored procedures</span></span>
<span data-ttu-id="0db69-154">hello InMemory-funktionen kan också ändra en lagrad procedur för bättre prestanda.</span><span class="sxs-lookup"><span data-stu-id="0db69-154">hello In-Memory feature can also modify a stored procedure for improved performance.</span></span>

### <a name="considerations-with-natively-compiled-stored-procedures"></a><span data-ttu-id="0db69-155">Överväganden med internt kompilerade lagrade procedurer</span><span class="sxs-lookup"><span data-stu-id="0db69-155">Considerations with natively compiled stored procedures</span></span>
<span data-ttu-id="0db69-156">En internt kompilerad lagrad procedur måste ha följande alternativ på dess T-SQL WITH-satsen hello:</span><span class="sxs-lookup"><span data-stu-id="0db69-156">A natively compiled stored procedure must have hello following options on its T-SQL WITH clause:</span></span>

* <span data-ttu-id="0db69-157">NATIVE_COMPILATION</span><span class="sxs-lookup"><span data-stu-id="0db69-157">NATIVE_COMPILATION</span></span>
* <span data-ttu-id="0db69-158">SCHEMABINDING: betydelse tabeller som hello lagrade proceduren kan inte ha sina kolumndefinitionerna ändras på något sätt som påverkar hello lagrad procedur om du släppa hello lagrade proceduren.</span><span class="sxs-lookup"><span data-stu-id="0db69-158">SCHEMABINDING: meaning tables that hello stored procedure cannot have their column definitions changed in any way that would affect hello stored procedure, unless you drop hello stored procedure.</span></span>

<span data-ttu-id="0db69-159">En inbyggd modul måste använda en stor [ATOMIC-block](http://msdn.microsoft.com/library/dn452281.aspx) för hantering av transaktioner.</span><span class="sxs-lookup"><span data-stu-id="0db69-159">A native module must use one big [ATOMIC blocks](http://msdn.microsoft.com/library/dn452281.aspx) for transaction management.</span></span> <span data-ttu-id="0db69-160">Det finns ingen roll för en explicit BEGIN TRANSACTION eller ROLLBACK TRANSACTION.</span><span class="sxs-lookup"><span data-stu-id="0db69-160">There is no role for an explicit BEGIN TRANSACTION, or for ROLLBACK TRANSACTION.</span></span> <span data-ttu-id="0db69-161">Om din kod upptäcker ett brott mot en affärsregel, det kan säga upp hello atomic-block med en [UTLÖSA](http://msdn.microsoft.com/library/ee677615.aspx) instruktionen.</span><span class="sxs-lookup"><span data-stu-id="0db69-161">If your code detects a violation of a business rule, it can terminate hello atomic block with a [THROW](http://msdn.microsoft.com/library/ee677615.aspx) statement.</span></span>

### <a name="typical-create-procedure-for-natively-compiled"></a><span data-ttu-id="0db69-162">Vanliga CREATE PROCEDURE för internt kompilerade</span><span class="sxs-lookup"><span data-stu-id="0db69-162">Typical CREATE PROCEDURE for natively compiled</span></span>
<span data-ttu-id="0db69-163">Hej T-SQL toocreate en internt kompilerad lagrad procedur är vanligtvis liknande toohello följande mall:</span><span class="sxs-lookup"><span data-stu-id="0db69-163">Typically hello T-SQL toocreate a natively compiled stored procedure is similar toohello following template:</span></span>

```
CREATE PROCEDURE schemaname.procedurename
    @param1 type1, …
    WITH NATIVE_COMPILATION, SCHEMABINDING
    AS
        BEGIN ATOMIC WITH
            (TRANSACTION ISOLATION LEVEL = SNAPSHOT,
            LANGUAGE = N'your_language__see_sys.languages'
            )
        …
        END;
```

* <span data-ttu-id="0db69-164">Hej TRANSACTION_ISOLATION_LEVEL är ÖGONBLICKSBILD hello vanligaste värde för hello internt kompilerade lagrade proceduren.</span><span class="sxs-lookup"><span data-stu-id="0db69-164">For hello TRANSACTION_ISOLATION_LEVEL, SNAPSHOT is hello most common value for hello natively compiled stored procedure.</span></span> <span data-ttu-id="0db69-165">Men en delmängd av hello andra värden stöds:</span><span class="sxs-lookup"><span data-stu-id="0db69-165">However,  a subset of hello other values are also supported:</span></span>
  
  * <span data-ttu-id="0db69-166">UPPREPBAR LÄSNING</span><span class="sxs-lookup"><span data-stu-id="0db69-166">REPEATABLE READ</span></span>
  * <span data-ttu-id="0db69-167">SERIALISERBARA</span><span class="sxs-lookup"><span data-stu-id="0db69-167">SERIALIZABLE</span></span>
* <span data-ttu-id="0db69-168">hello värdet LANGUAGE måste finnas i hello sys.languages vy.</span><span class="sxs-lookup"><span data-stu-id="0db69-168">hello LANGUAGE value must be present in hello sys.languages view.</span></span>

### <a name="how-toomigrate-a-stored-procedure"></a><span data-ttu-id="0db69-169">Hur toomigrate en lagrad procedur</span><span class="sxs-lookup"><span data-stu-id="0db69-169">How toomigrate a stored procedure</span></span>
<span data-ttu-id="0db69-170">hello migreringssteg är:</span><span class="sxs-lookup"><span data-stu-id="0db69-170">hello migration steps are:</span></span>

1. <span data-ttu-id="0db69-171">Hämta hello CREATE PROCEDURE skriptet toohello reguljära tolkad lagrad procedur.</span><span class="sxs-lookup"><span data-stu-id="0db69-171">Obtain hello CREATE PROCEDURE script toohello regular interpreted stored procedure.</span></span>
2. <span data-ttu-id="0db69-172">Skriv om dess huvud toomatch hello tidigare mallen.</span><span class="sxs-lookup"><span data-stu-id="0db69-172">Rewrite its header toomatch hello previous template.</span></span>
3. <span data-ttu-id="0db69-173">Fastställa om hello lagrade proceduren T-SQL-kod använder alla funktioner som inte stöds för internt kompilerade lagrade procedurer.</span><span class="sxs-lookup"><span data-stu-id="0db69-173">Ascertain whether hello stored procedure T-SQL code uses any features that are not supported for natively compiled stored procedures.</span></span> <span data-ttu-id="0db69-174">Implementera lösningar om det behövs.</span><span class="sxs-lookup"><span data-stu-id="0db69-174">Implement workarounds if necessary.</span></span>
   
   * <span data-ttu-id="0db69-175">Mer information finns i [migreringsproblem för internt kompilerade lagrade procedurer](http://msdn.microsoft.com/library/dn296678.aspx).</span><span class="sxs-lookup"><span data-stu-id="0db69-175">For details see [Migration Issues for Natively Compiled Stored Procedures](http://msdn.microsoft.com/library/dn296678.aspx).</span></span>
4. <span data-ttu-id="0db69-176">Byt namn på hello gamla lagrade proceduren med hjälp av SP_RENAME.</span><span class="sxs-lookup"><span data-stu-id="0db69-176">Rename hello old stored procedure by using SP_RENAME.</span></span> <span data-ttu-id="0db69-177">Eller helt enkelt.</span><span class="sxs-lookup"><span data-stu-id="0db69-177">Or simply DROP it.</span></span>
5. <span data-ttu-id="0db69-178">Kör skriptet redigerade skapa proceduren T-SQL.</span><span class="sxs-lookup"><span data-stu-id="0db69-178">Run your edited CREATE PROCEDURE T-SQL script.</span></span>

## <a name="step-6-run-your-workload-in-test"></a><span data-ttu-id="0db69-179">Steg 6: Kör din arbetsbelastning i test</span><span class="sxs-lookup"><span data-stu-id="0db69-179">Step 6: Run your workload in test</span></span>
<span data-ttu-id="0db69-180">Köra en arbetsbelastning i testdatabasen är liknande toohello arbetsbelastning som körs i produktionsdatabasen.</span><span class="sxs-lookup"><span data-stu-id="0db69-180">Run a workload in your test database that is similar toohello workload that runs in your production database.</span></span> <span data-ttu-id="0db69-181">Detta bör visa hello prestandafördelar uppnås genom din användning av hello InMemory-funktionen för tabeller och lagrade procedurer.</span><span class="sxs-lookup"><span data-stu-id="0db69-181">This should reveal hello performance gain achieved by your use of hello In-Memory feature for tables and stored procedures.</span></span>

<span data-ttu-id="0db69-182">Viktiga attribut för hello arbetsbelastning är:</span><span class="sxs-lookup"><span data-stu-id="0db69-182">Major attributes of hello workload are:</span></span>

* <span data-ttu-id="0db69-183">Antalet samtidiga anslutningar.</span><span class="sxs-lookup"><span data-stu-id="0db69-183">Number of concurrent connections.</span></span>
* <span data-ttu-id="0db69-184">Förhållandet mellan läsning och skrivning.</span><span class="sxs-lookup"><span data-stu-id="0db69-184">Read/write ratio.</span></span>

<span data-ttu-id="0db69-185">tootailor och kör hello test arbetsbelastning, Överväg att använda hello praktiska ostress.exe tool, som illustreras i [här](sql-database-in-memory.md).</span><span class="sxs-lookup"><span data-stu-id="0db69-185">tootailor and run hello test workload, consider using hello handy ostress.exe tool, which illustrated in [here](sql-database-in-memory.md).</span></span>

<span data-ttu-id="0db69-186">toominimize Nätverksfördröjningen, kör testet i hello samma Azure geografiska region där hello databasen finns.</span><span class="sxs-lookup"><span data-stu-id="0db69-186">toominimize network latency, run your test in hello same Azure geographic region where hello database exists.</span></span>

## <a name="step-7-post-implementation-monitoring"></a><span data-ttu-id="0db69-187">Steg 7: Efter implementering övervakning</span><span class="sxs-lookup"><span data-stu-id="0db69-187">Step 7: Post-implementation monitoring</span></span>
<span data-ttu-id="0db69-188">Överväg att övervaka hello prestanda effekterna av din InMemory-implementeringar i produktion:</span><span class="sxs-lookup"><span data-stu-id="0db69-188">Consider monitoring hello performance effects of your In-Memory implementations in production:</span></span>

* <span data-ttu-id="0db69-189">[Övervaka InMemory-lagringen](sql-database-in-memory-oltp-monitoring.md).</span><span class="sxs-lookup"><span data-stu-id="0db69-189">[Monitor In-Memory Storage](sql-database-in-memory-oltp-monitoring.md).</span></span>
* [<span data-ttu-id="0db69-190">Övervaka Azure SQL Database med dynamiska hanteringsvyer</span><span class="sxs-lookup"><span data-stu-id="0db69-190">Monitoring Azure SQL Database using dynamic management views</span></span>](sql-database-monitoring-with-dmvs.md)

## <a name="related-links"></a><span data-ttu-id="0db69-191">Relaterade länkar</span><span class="sxs-lookup"><span data-stu-id="0db69-191">Related links</span></span>
* [<span data-ttu-id="0db69-192">OLTP (i minnet optimering) i minnet</span><span class="sxs-lookup"><span data-stu-id="0db69-192">In-Memory OLTP (In-Memory Optimization)</span></span>](http://msdn.microsoft.com/library/dn133186.aspx)
* [<span data-ttu-id="0db69-193">Introduktion tooNatively kompilerade lagrade procedurer</span><span class="sxs-lookup"><span data-stu-id="0db69-193">Introduction tooNatively Compiled Stored Procedures</span></span>](http://msdn.microsoft.com/library/dn133184.aspx)
* [<span data-ttu-id="0db69-194">Klassificering av minne optimering</span><span class="sxs-lookup"><span data-stu-id="0db69-194">Memory Optimization Advisor</span></span>](http://msdn.microsoft.com/library/dn284308.aspx)

