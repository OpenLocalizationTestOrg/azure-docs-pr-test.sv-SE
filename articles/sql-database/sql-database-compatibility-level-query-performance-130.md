---
title: "Databasen kompatibilitetsnivå 130 - Azure SQL Database | Microsoft Docs"
description: "I den här artikeln förklarar vi fördelarna med att köra Azure SQL Database på kompatibilitetsnivå 130 och utnyttja fördelarna med den nya frågeoptimeraren och fråga processorfunktioner. Vi tar också upp möjliga sidoeffekter på frågeprestanda för de befintliga SQL-program."
services: sql-database
documentationcenter: 
author: alainlissoir
manager: jhubbard
editor: 
ms.assetid: 8619f90b-7516-46dc-9885-98429add0053
ms.service: sql-database
ms.custom: monitor & tune
ms.workload: data-management
ms.devlang: NA
ms.tgt_pltfrm: NA
ms.topic: article
ms.date: 08/08/2016
ms.author: alainl
ms.openlocfilehash: c08c0690df4f389416e4ed2e2df2dbb72d6fd567
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="improved-query-performance-with-compatibility-level-130-in-azure-sql-database"></a><span data-ttu-id="680f2-104">Förbättrad prestanda för frågor med nivå 130 i Azure SQL Database</span><span class="sxs-lookup"><span data-stu-id="680f2-104">Improved query performance with compatibility Level 130 in Azure SQL Database</span></span>
<span data-ttu-id="680f2-105">Azure SQL Database körs transparent hundratals tusentals databaser på många olika kompatibilitetsnivåer bevara och säkerställa bakåtkompatibilitet med motsvarande version av Microsoft SQL Server för alla kunder!</span><span class="sxs-lookup"><span data-stu-id="680f2-105">Azure SQL Database is running transparently hundreds of thousands of databases at many different compatibility levels, preserving and guaranteeing the backward compatibility to the corresponding version of Microsoft SQL Server for all its customers!</span></span>

<span data-ttu-id="680f2-106">I den här artikeln förklarar vi fördelarna med att köra din Azure SQL-Databse på kompatibilitetsnivå 130 och utnyttja fördelarna med den nya frågeoptimeraren och fråga processorfunktioner.</span><span class="sxs-lookup"><span data-stu-id="680f2-106">In this article, we explore the benefits of running your Azure SQL Databse at compatibility level 130, and leveraging the benefits of the new query optimizer and query processor features.</span></span> <span data-ttu-id="680f2-107">Vi tar också upp möjliga sidoeffekter på frågeprestanda för de befintliga SQL-program.</span><span class="sxs-lookup"><span data-stu-id="680f2-107">We also address the possible side-effects on the query performance for the existing SQL applications.</span></span>

<span data-ttu-id="680f2-108">Som en påminnelse om historik justeringen för SQL-versioner till standard kompatibilitetsnivåer är följande:</span><span class="sxs-lookup"><span data-stu-id="680f2-108">As a reminder of history, the alignment of SQL versions to default compatibility levels are as follows:</span></span>

* <span data-ttu-id="680f2-109">100: i SQL Server 2008 och Azure SQL Database V11.</span><span class="sxs-lookup"><span data-stu-id="680f2-109">100: in SQL Server 2008 and Azure SQL Database V11.</span></span>
* <span data-ttu-id="680f2-110">110: i SQL Server 2012 och Azure SQL Database V11.</span><span class="sxs-lookup"><span data-stu-id="680f2-110">110: in SQL Server 2012 and Azure SQL Database V11.</span></span>
* <span data-ttu-id="680f2-111">120: i SQL Server 2014 och Azure SQL Database V12.</span><span class="sxs-lookup"><span data-stu-id="680f2-111">120: in SQL Server 2014 and Azure SQL Database V12.</span></span>
* <span data-ttu-id="680f2-112">130: i SQL Server 2016 och Azure SQL-databasen (V12).</span><span class="sxs-lookup"><span data-stu-id="680f2-112">130: in SQL Server 2016 and Azure SQL Database V12.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="680f2-113">Från och med **mid juni 2016**, i Azure SQL Database kompatibilitetsnivån standard blir 130 i stället för 120 för **nyskapad** databaser.</span><span class="sxs-lookup"><span data-stu-id="680f2-113">Starting in **mid-June 2016**, in Azure SQL Database, the default compatibility level will be 130 instead of 120 for **newly created** databases.</span></span>
> 
> <span data-ttu-id="680f2-114">Databaser som skapats före mitten av juni 2016 kommer *inte* påverkas och kommer bibehålla sina aktuella kompatibilitetsnivån (100, 110 eller 120).</span><span class="sxs-lookup"><span data-stu-id="680f2-114">Databases created before mid-June 2016 will *not* be affected, and will maintain their current compatibility level (100, 110, or 120).</span></span> <span data-ttu-id="680f2-115">Databaser som migrerats från Azure SQL Database version V11 till V12 har en kompatibilitetsnivå 100 eller 110.</span><span class="sxs-lookup"><span data-stu-id="680f2-115">Databases that migrated from Azure SQL Database version V11 to V12 will have a compatibility level of either 100 or 110.</span></span> 
> 

## <a name="about-compatibility-level-130"></a><span data-ttu-id="680f2-116">Om kompatibilitetsnivåer 130</span><span class="sxs-lookup"><span data-stu-id="680f2-116">About compatibility level 130</span></span>
<span data-ttu-id="680f2-117">Först om du vill veta den aktuella kompatibilitetsnivån för din databas kör du följande Transact-SQL-sats.</span><span class="sxs-lookup"><span data-stu-id="680f2-117">First, if you want to know the current compatibility level of your database, execute the following Transact-SQL statement.</span></span>

```
SELECT compatibility_level
    FROM sys.databases
    WHERE name = '<YOUR DATABASE_NAME>’;
```


<span data-ttu-id="680f2-118">Innan den här ändringen till nivå 130 sker för **nyligen** skapat databaser, vi gå igenom den här ändringen handlar om till exempel grundläggande frågan och se hur vem som helst kan dra nytta av den.</span><span class="sxs-lookup"><span data-stu-id="680f2-118">Before this change to level 130 happens for **newly** created databases, let’s review what this change is all about through some very basic query examples, and see how anyone can benefit from it.</span></span>

<span data-ttu-id="680f2-119">Frågebearbetning i relationsdatabaser kan vara mycket komplex och kan leda till många datorvetenskap, räkning att förstå inbyggd designalternativen och beteenden.</span><span class="sxs-lookup"><span data-stu-id="680f2-119">Query processing in relational databases can be very complex and can lead to lots of computer science and mathematics to understand the inherent design choices and behaviors.</span></span> <span data-ttu-id="680f2-120">I det här dokumentet har innehållet avsiktligt förenklats så att alla med vissa minsta teknisk bakgrund förstå effekten av ändringen kompatibilitet nivå och ta reda på hur det kan göra program.</span><span class="sxs-lookup"><span data-stu-id="680f2-120">In this document, the content has been intentionally simplified to ensure that anyone with some minimum technical background can understand the impact of the compatibility level change and determine how it can benefit applications.</span></span>

<span data-ttu-id="680f2-121">Vi har en titt på kompatibilitetsnivån 130 ger på tabellen.</span><span class="sxs-lookup"><span data-stu-id="680f2-121">Let’s have a quick look at what the compatibility level 130 brings at the table.</span></span>  <span data-ttu-id="680f2-122">Du hittar mer information i [ändra DATABASENS kompatibilitetsnivån (Transact-SQL)](https://msdn.microsoft.com/library/bb510680.aspx), men här är en kort sammanfattning:</span><span class="sxs-lookup"><span data-stu-id="680f2-122">You can find more details at [ALTER DATABASE Compatibility Level (Transact-SQL)](https://msdn.microsoft.com/library/bb510680.aspx), but here is a short summary:</span></span>

* <span data-ttu-id="680f2-123">Insert-åtgärden för en Insert select-instruktion kan vara flertrådade eller en parallell plan medan innan den här åtgärden var Enkeltrådig.</span><span class="sxs-lookup"><span data-stu-id="680f2-123">The Insert operation of an Insert-select statement can be multi-threaded or can have a parallel plan, while before this operation was single-threaded.</span></span>
* <span data-ttu-id="680f2-124">Optimerad tabell och tabellen variabler frågor kan nu använda parallella planer minne medan innan den här åtgärden även var Enkeltrådig.</span><span class="sxs-lookup"><span data-stu-id="680f2-124">Memory Optimized table and table variables queries can now have parallel plans, while before this operation was also single-threaded .</span></span>
* <span data-ttu-id="680f2-125">Statistik för Minnesoptimerade tabellen kan nu samlas in och kan uppdateras automatiskt.</span><span class="sxs-lookup"><span data-stu-id="680f2-125">Statistics for Memory Optimized table can now be sampled and are auto-updated.</span></span> <span data-ttu-id="680f2-126">Se [vad är nytt i Database Engine: Minnesintern OLTP](https://msdn.microsoft.com/library/bb510411.aspx#InMemory) för mer information.</span><span class="sxs-lookup"><span data-stu-id="680f2-126">See [What's New in Database Engine: In-Memory OLTP](https://msdn.microsoft.com/library/bb510411.aspx#InMemory) for more details.</span></span>
* <span data-ttu-id="680f2-127">Batch-läge v/s radläge ändras med kolumnen index</span><span class="sxs-lookup"><span data-stu-id="680f2-127">Batch mode v/s Row Mode changes with Column Store indexes</span></span>
  * <span data-ttu-id="680f2-128">Sorteringar för en tabell med en kolumn index är nu i batch-läge.</span><span class="sxs-lookup"><span data-stu-id="680f2-128">Sorts on a table with a Column Store index are now in batch mode.</span></span>
  * <span data-ttu-id="680f2-129">Fönsterhantering mängder fungerar nu i batchläge, till exempel TSQL FÖRDRÖJNING/LEAD instruktioner.</span><span class="sxs-lookup"><span data-stu-id="680f2-129">Windowing aggregates now operate in batch mode such as TSQL LAG/LEAD statements.</span></span>
  * <span data-ttu-id="680f2-130">Frågor om kolumnen Store tabeller med flera distinct-satser fungerar i Batch-läge.</span><span class="sxs-lookup"><span data-stu-id="680f2-130">Queries on Column Store tables with Multiple distinct clauses operate in Batch mode.</span></span>
  * <span data-ttu-id="680f2-131">Frågor som körs under DOP = 1 eller med en seriell plan också köras i Batch-läge.</span><span class="sxs-lookup"><span data-stu-id="680f2-131">Queries running under DOP=1 or with a serial plan also execute in Batch Mode.</span></span>
* <span data-ttu-id="680f2-132">Senast, kardinalitet beräkning av förbättringar som faktiskt kommer med kompatibilitetsnivå 120, men för de körs på en lägre kompatibilitetsnivå (d.v.s. 100 eller 110), flytta kompatibilitet nivå 130 kommer också att få dessa förbättringar, och dessa kan också dra frågeprestandan för dina program.</span><span class="sxs-lookup"><span data-stu-id="680f2-132">Last, Cardinality Estimation improvements are actually coming with compatibility level 120, but for those of you running at a lower Compatibility level (i.e. 100, or 110), the move to compatibility level 130 will also bring these improvements, and these can also benefit the query performance of your applications.</span></span>

## <a name="practicing-compatibility-level-130"></a><span data-ttu-id="680f2-133">Öva kompatibilitetsnivå 130</span><span class="sxs-lookup"><span data-stu-id="680f2-133">Practicing compatibility level 130</span></span>
<span data-ttu-id="680f2-134">Första ska få några tabeller, index och slumpmässiga data som skapats för att öva några av de nya funktionerna.</span><span class="sxs-lookup"><span data-stu-id="680f2-134">First let’s get some tables, indexes and random data created to practice some of these new capabilities.</span></span> <span data-ttu-id="680f2-135">Exempel för TSQL-skript kan köras under SQL Server 2016 eller under Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="680f2-135">The TSQL script examples can be executed under SQL Server 2016, or under Azure SQL Database.</span></span> <span data-ttu-id="680f2-136">Men när du skapar en Azure SQL database, kontrollera att du väljer minst en P2 databas eftersom du måste ha minst ett antal kärnor tillåta flertrådsteknik och därför utnyttja dessa funktioner.</span><span class="sxs-lookup"><span data-stu-id="680f2-136">However, when creating an Azure SQL database, make sure you choose at the minimum a P2 database because you need at least a couple of cores to allow multi-threading and therefore benefit from these features.</span></span>

```
-- Create a Premium P2 Database in Azure SQL Database

CREATE DATABASE MyTestDB
    (EDITION=’Premium’, SERVICE_OBJECTIVE=’P2′);
GO

-- Create 2 tables with a column store index on
-- the second one (only available on Premium databases)

CREATE TABLE T_source
    (Color varchar(10), c1 bigint, c2 bigint);

CREATE TABLE T_target
    (c1 bigint, c2 bigint);

CREATE CLUSTERED COLUMNSTORE INDEX CCI ON T_target;
GO

-- Insert few rows.

INSERT T_source VALUES
    (‘Blue’, RAND() * 100000, RAND() * 100000),
    (‘Yellow’, RAND() * 100000, RAND() * 100000),
    (‘Red’, RAND() * 100000, RAND() * 100000),
    (‘Green’, RAND() * 100000, RAND() * 100000),
    (‘Black’, RAND() * 100000, RAND() * 100000);

GO 200

INSERT T_source SELECT * FROM T_source;

GO 10
```


<span data-ttu-id="680f2-137">Nu ska vi ta en titt att vissa funktioner för bearbetning av frågan kommer med kompatibilitetsnivå 130.</span><span class="sxs-lookup"><span data-stu-id="680f2-137">Now, let’s have a look to some of the Query Processing features coming with compatibility level 130.</span></span>

## <a name="parallel-insert"></a><span data-ttu-id="680f2-138">Parallell INSERT</span><span class="sxs-lookup"><span data-stu-id="680f2-138">Parallel INSERT</span></span>
<span data-ttu-id="680f2-139">Köra TSQL instruktionerna nedan kör INSERT-åtgärden under kompatibilitetsnivå 120 och 130, som utför respektive INSERT-åtgärden i en enda trådad modell (120) och i ett flertrådat modellen (130).</span><span class="sxs-lookup"><span data-stu-id="680f2-139">Executing the TSQL statements below executes the INSERT operation under compatibility level 120 and 130, which respectively executes the INSERT operation in a single threaded model (120), and in a multi-threaded model (130).</span></span>

```
-- Parallel INSERT … SELECT … in heap or CCI
-- is available under 130 only

SET STATISTICS XML ON;

ALTER DATABASE MyTestDB
    SET COMPATIBILITY_LEVEL = 120;
GO 

-- The INSERT part is in serial

INSERT t_target WITH (tablock)
    SELECT C1, COUNT(C2) * 10 * RAND()
        FROM T_source
        GROUP BY C1
    OPTION (RECOMPILE);

ALTER DATABASE MyTestDB
    SET COMPATIBILITY_LEVEL = 130
GO

-- The INSERT part is in parallel

INSERT t_target WITH (tablock)
    SELECT C1, COUNT(C2) * 10 * RAND()
        FROM T_source
        GROUP BY C1
    OPTION (RECOMPILE);

SET STATISTICS XML OFF;
```


<span data-ttu-id="680f2-140">Du kan bestämma vilka kardinalitet beräkning av funktionen är i play genom att begära den faktiska frågeplanen tittar på dess grafisk representation eller dess XML-innehåll.</span><span class="sxs-lookup"><span data-stu-id="680f2-140">By requesting the actual the query plan, looking at its graphical representation or its XML content, you can determine which Cardinality Estimation function is at play.</span></span> <span data-ttu-id="680f2-141">Granskar planer sida-vid-sida i figur 1 tydligt ser vi att kolumnen Store Infoga körningen går från seriella i 120 till parallellt i 130.</span><span class="sxs-lookup"><span data-stu-id="680f2-141">Looking at the plans side-by-side on figure 1, we can clearly see that the Column Store INSERT execution goes from serial in 120 to parallel in 130.</span></span> <span data-ttu-id="680f2-142">Observera också att ändringen av iterator-ikonen i 130 planen visar två parallella pilar illustrerar faktumet att nu iterator-körningen är verkligen parallellt.</span><span class="sxs-lookup"><span data-stu-id="680f2-142">Also, note that the change of the iterator icon in the 130 plan showing two parallel arrows, illustrating the fact that now the iterator execution is indeed parallel.</span></span> <span data-ttu-id="680f2-143">Om du har stora INSERT-åtgärder att utföra fungerar parallell körning, länkade till antalet kärnor som du har på tillgänglig för databasen bättre; upp till 100 gånger snabbare beroende din situation!</span><span class="sxs-lookup"><span data-stu-id="680f2-143">If you have large INSERT operations to complete, the parallel execution, linked to the number of core you have at your disposal for the database, will perform better; up to a 100 times faster depending your situation!</span></span>

<span data-ttu-id="680f2-144">*Bild 1: INSERT-åtgärden ändras från seriella till parallell kompatibilitet med nivå 130.*</span><span class="sxs-lookup"><span data-stu-id="680f2-144">*Figure 1: INSERT operation changes from serial to parallel with compatibility level 130.*</span></span>

![Bild 1](./media/sql-database-compatibility-level-query-performance-130/figure-1.jpg)

## <a name="serial-batch-mode"></a><span data-ttu-id="680f2-146">SERIELL Batch-läge</span><span class="sxs-lookup"><span data-stu-id="680f2-146">SERIAL Batch Mode</span></span>
<span data-ttu-id="680f2-147">Flytta till kompatibilitetsnivå 130 vid bearbetning av datarader aktiveras på motsvarande sätt, batchbearbetning läge.</span><span class="sxs-lookup"><span data-stu-id="680f2-147">Similarly, moving to compatibility level 130 when processing rows of data enables batch mode processing.</span></span> <span data-ttu-id="680f2-148">Först finns batchåtgärder läge bara när du har ett columnstore-index på plats.</span><span class="sxs-lookup"><span data-stu-id="680f2-148">First, batch mode operations  are only available when you have a column store index in place.</span></span> <span data-ttu-id="680f2-149">Därefter en batch representerar vanligen ~ 900 rader, och använder en kod logik som är optimerade för flera kärnor CPU, högre dataflödet i minnet och direkt utnyttjar komprimerade data för kolumnen Arkiv när det är möjligt.</span><span class="sxs-lookup"><span data-stu-id="680f2-149">Second, a batch typically represents ~900 rows, and uses a code logic optimized for multicore CPU, higher memory throughput and directly leverages the compressed data of the Column Store whenever possible.</span></span> <span data-ttu-id="680f2-150">Under dessa förhållanden SQL Server 2016 kan bearbeta ~ 900 rader samtidigt, i stället för 1 rad vid tidpunkt och följaktligen övergripande omkostnaden för åtgärden nu delas av hela batchen, vilket minskar den totala kostnaden per rad.</span><span class="sxs-lookup"><span data-stu-id="680f2-150">Under these conditions, SQL Server 2016 can process ~900 rows at once, instead of 1 row at the time, and as a consequence, the overall overhead cost of the operation is now shared by the entire batch, reducing the overall cost by row.</span></span> <span data-ttu-id="680f2-151">Den här delade mängd operationer som kombineras med kolumnen store komprimering i princip minskar svarstiden ingår i en SELECT batch-läge.</span><span class="sxs-lookup"><span data-stu-id="680f2-151">This shared amount of operations combined with the column store compression basically reduces the latency involved in a SELECT batch mode operation.</span></span> <span data-ttu-id="680f2-152">Du kan hitta mer information om kolumnen store och batchläge på [Columnstore-index guiden](https://msdn.microsoft.com/library/gg492088.aspx).</span><span class="sxs-lookup"><span data-stu-id="680f2-152">You can find more details about the column store and batch mode at [Columnstore Indexes Guide](https://msdn.microsoft.com/library/gg492088.aspx).</span></span>

```
-- Serial batch mode execution

SET STATISTICS XML ON;

ALTER DATABASE MyTestDB
    SET COMPATIBILITY_LEVEL = 120;
GO

-- The scan and aggregate are in row mode

SELECT C1, COUNT (C2)
    FROM T_target
    GROUP BY C1
    OPTION (MAXDOP 1, RECOMPILE);
GO

ALTER DATABASE MyTestDB
    SET COMPATIBILITY_LEVEL = 130;
GO 

-- The scan and aggregate are in batch mode,
-- and force MAXDOP to 1 to show that batch mode
-- also now works in serial mode.

SELECT C1, COUNT(C2)
    FROM T_target
    GROUP BY C1
    OPTION (MAXDOP 1, RECOMPILE);
GO

SET STATISTICS XML OFF;
```


<span data-ttu-id="680f2-153">Som visas nedan, genom att följa frågan planer sida vid sida på bild 2 kan vi används se att bearbetningsläget har ändrats med en kompatibilitetsnivå och följaktligen när du kör frågor i båda kompatibilitetsnivå helt, kan vi se att det mesta av bearbetningstiden som i radläge (86%) jämfört med batch-läge (14%), där 2 batchar har bearbetats.</span><span class="sxs-lookup"><span data-stu-id="680f2-153">As visible below, by observing the query plans side-by-side on figure 2, we can see that the processing mode has changed with the compatibility level, and as a consequence, when executing the queries in both compatibility level altogether, we can see that most of the processing time is spent in row mode (86%) compared to the batch mode (14%), where 2 batches have been processed.</span></span> <span data-ttu-id="680f2-154">Öka dataset, fördelen ökar.</span><span class="sxs-lookup"><span data-stu-id="680f2-154">Increase the dataset, the benefit will increase.</span></span>

<span data-ttu-id="680f2-155">*Bild 2: Välj åtgärden ändringar seriella till batchläge med en kompatibilitetsnivå 130.*</span><span class="sxs-lookup"><span data-stu-id="680f2-155">*Figure 2: SELECT operation changes from serial to batch mode with compatibility level 130.*</span></span>

![Bild 2](./media/sql-database-compatibility-level-query-performance-130/figure-2.jpg)

## <a name="batch-mode-on-sort-execution"></a><span data-ttu-id="680f2-157">Batch-läge på Sortera körning</span><span class="sxs-lookup"><span data-stu-id="680f2-157">Batch mode on Sort Execution</span></span>
<span data-ttu-id="680f2-158">Övergången från radläge (kompatibilitetsnivå 120) till batch-läge (kompatibilitetsnivå 130) liknar ovan, men tillämpas sortering, förbättrar prestanda på sorteringen av samma skäl.</span><span class="sxs-lookup"><span data-stu-id="680f2-158">Similar to the above, but applied to a sort operation, the transition from row mode (compatibility level 120) to batch mode (compatibility level 130) improves the performance of the SORT operation for the same reasons.</span></span>

```
-- Batch mode on sort execution

SET STATISTICS XML ON;

ALTER DATABASE MyTestDB
    SET COMPATIBILITY_LEVEL = 120;
GO

-- The scan and aggregate are in row mode

SELECT C1, COUNT(C2)
    FROM T_target
    GROUP BY C1
    ORDER BY C1
    OPTION (MAXDOP 1, RECOMPILE);
GO

ALTER DATABASE MyTestDB
    SET COMPATIBILITY_LEVEL = 130;
GO

-- The scan and aggregate are in batch mode,
-- and force MAXDOP to 1 to show that batch mode
-- also now works in serial mode.

SELECT C1, COUNT(C2)
    FROM T_target
    GROUP BY C1
    ORDER BY C1
    OPTION (MAXDOP 1, RECOMPILE);
GO

SET STATISTICS XML OFF;
```


<span data-ttu-id="680f2-159">Synliga sida-vid-sida på figur 3, vi se att sorteringen i radläge innebär 81% av kostnaden, medan batch-läge innebär bara 19% av kostnaden (respektive 81% och 56% på sorteringen sig själv).</span><span class="sxs-lookup"><span data-stu-id="680f2-159">Visible side-by-side on figure 3, we can see that the sort operation in row mode represents 81% of the cost, while the batch mode only represents 19% of the cost (respectively 81% and 56% on the sort itself).</span></span>

<span data-ttu-id="680f2-160">*Bild 3: Sorteringsåtgärden ändras från raden till batch-läge med en kompatibilitetsnivå 130.*</span><span class="sxs-lookup"><span data-stu-id="680f2-160">*Figure 3: SORT operation changes from row to batch mode with compatibility level 130.*</span></span>

![Bild 3](./media/sql-database-compatibility-level-query-performance-130/figure-3.png)

<span data-ttu-id="680f2-162">Naturligtvis är innehålla exemplen endast rader, tiotusentals som ingenting när du tittar på data som är tillgängliga i de flesta SQL Servers dessa dagar.</span><span class="sxs-lookup"><span data-stu-id="680f2-162">Obviously, these samples only contain tens of thousands of rows, which is nothing when looking at the data available in most SQL Servers these days.</span></span> <span data-ttu-id="680f2-163">Projektet bara dessa mot miljoner rader i stället detta kan översätta i flera minuter för körning av undantas varje dag väntande arten av din arbetsbelastning.</span><span class="sxs-lookup"><span data-stu-id="680f2-163">Just project these against millions of rows instead, and this can translate in several minutes of execution spared every day pending the nature of your workload.</span></span>

## <a name="cardinality-estimation-ce-improvements"></a><span data-ttu-id="680f2-164">Förbättringar av kardinalitet uppskattning (CE)</span><span class="sxs-lookup"><span data-stu-id="680f2-164">Cardinality Estimation (CE) improvements</span></span>
<span data-ttu-id="680f2-165">Introducerades i SQL Server 2014 någon databas som körs på en kompatibilitetsnivå 120 eller över kommer genom att utnyttja nya kardinalitet beräkning av funktioner.</span><span class="sxs-lookup"><span data-stu-id="680f2-165">Introduced with SQL Server 2014, any database running at a compatibility level 120 or above will make use of the new Cardinality Estimation functionality.</span></span> <span data-ttu-id="680f2-166">Beräkning av kardinalitet är i princip den logik som används för att avgöra hur SQLServer ska köra en fråga baserat på dess uppskattade kostnaden.</span><span class="sxs-lookup"><span data-stu-id="680f2-166">Essentially, cardinality estimation is the logic used to determine how SQL server will execute a query based on its estimated cost.</span></span> <span data-ttu-id="680f2-167">Uppskattningen beräknas med hjälp av indata från statistik som är associerade med objekt som är inblandade i frågan.</span><span class="sxs-lookup"><span data-stu-id="680f2-167">The estimation is calculated using input from statistics associated with objects involved in that query.</span></span> <span data-ttu-id="680f2-168">Praktiskt taget, på en hög nivå kardinalitet beräkning av funktioner är raden antal uppskattningar tillsammans med information om distribution av värden, distinkta värdet inventeringar, och dubbla räknas som finns i de tabeller och objekten som refereras i frågan.</span><span class="sxs-lookup"><span data-stu-id="680f2-168">Practically, at a high-level, Cardinality Estimation functions are row count estimates along with information about the distribution of the values, distinct value counts, and duplicate counts contained in the tables and objects referenced in the query.</span></span> <span data-ttu-id="680f2-169">Hämta dessa uppskattningar fel och kan leda till onödiga disk i/o på grund av otillräckligt minne ger (d.v.s. TempDB spill) eller till ett urval av en seriell plan körning via en plan för parallell körning till mera.</span><span class="sxs-lookup"><span data-stu-id="680f2-169">Getting these estimates wrong, can lead to unnecessary disk I/O due to insufficient memory grants (i.e. TempDB spills), or to a selection of a serial plan execution over a parallel plan execution, to name a few.</span></span> <span data-ttu-id="680f2-170">Slutsats, felaktig beräknar kan leda till en övergripande prestandaförsämring i frågan.</span><span class="sxs-lookup"><span data-stu-id="680f2-170">Conclusion, incorrect estimates can lead to an overall performance degradation of the query execution.</span></span> <span data-ttu-id="680f2-171">På den andra sidan bättre uppskattningar, exaktare beräknar leder till att bättre frågan körningar!</span><span class="sxs-lookup"><span data-stu-id="680f2-171">On the other side, better estimates, more accurate estimates, leads to better query executions!</span></span>

<span data-ttu-id="680f2-172">Som tidigare nämnts, fråga optimeringar och beräknar är en komplex fråga, men om du vill veta mer om frågeplaner och kardinalitet exteriörbedömning du kan referera till dokumentet på [optimera din frågeplaner med SQL Server 2014 kardinalitet exteriörbedömning](https://msdn.microsoft.com/library/dn673537.aspx) för en mer grundlig genomgång.</span><span class="sxs-lookup"><span data-stu-id="680f2-172">As mentioned before, query optimizations and estimates are a complex matter, but if you want to learn more about query plans and cardinality estimator, you can refer to the document at [Optimizing Your Query Plans with the SQL Server 2014 Cardinality Estimator](https://msdn.microsoft.com/library/dn673537.aspx) for a deeper dive.</span></span>

## <a name="which-cardinality-estimation-do-you-currently-use"></a><span data-ttu-id="680f2-173">Vilka kardinalitet beräkning av du för närvarande använder?</span><span class="sxs-lookup"><span data-stu-id="680f2-173">Which Cardinality Estimation do you currently use?</span></span>
<span data-ttu-id="680f2-174">Att fastställa under vilka kardinalitet beräkning av dina frågor körs, vi bara använda frågan exemplen nedan.</span><span class="sxs-lookup"><span data-stu-id="680f2-174">To determine under which Cardinality Estimation your queries are running, let’s just use the query samples below.</span></span> <span data-ttu-id="680f2-175">Observera att det här första exemplet ska köras under kompatibilitetsnivån 110, innebär användning av de gamla kardinalitet beräkning av funktionerna.</span><span class="sxs-lookup"><span data-stu-id="680f2-175">Note that this first example will run under compatibility level 110, implying the use of the old Cardinality Estimation functions.</span></span>

```
-- Old CE

ALTER DATABASE MyTestDB
    SET COMPATIBILITY_LEVEL = 110;
GO

SET STATISTICS XML ON;

SELECT [c1]
    FROM [dbo].[T_target]
    WHERE [c1] > 20000;
GO

SET STATISTICS XML OFF;
```


<span data-ttu-id="680f2-176">När körningen har slutförts, klicka på länken XML och titta på egenskaperna för den första iterator enligt nedan.</span><span class="sxs-lookup"><span data-stu-id="680f2-176">Once execution is complete, click on the XML link, and look at the properties of the first iterator as shown below.</span></span> <span data-ttu-id="680f2-177">Observera egenskapsnamnet kallas CardinalityEstimationModelVersion som för närvarande inställd på 70.</span><span class="sxs-lookup"><span data-stu-id="680f2-177">Note the property name called CardinalityEstimationModelVersion currently set on 70.</span></span> <span data-ttu-id="680f2-178">Det innebär inte att databasens kompatibilitetsnivå har angetts till SQL Server 7.0-version (den är inställd på 110 som visas i TSQL-uttryck ovanför), men värdet 70 bara representerar bakåtkompatibla kardinalitet beräkning av funktionerna eftersom SQL Server 7.0 som hade ingen större ändringar förrän SQL Server 2014 (som levereras med en kompatibilitetsnivå på 120).</span><span class="sxs-lookup"><span data-stu-id="680f2-178">It does not mean that the database compatibility level is set to the SQL Server 7.0 version (it is set on 110 as visible in the TSQL statements above), but the value 70 simply represents the legacy Cardinality Estimation functionality available since SQL Server 7.0, which had no major revisions until SQL Server 2014 (which comes with a compatibility level of 120).</span></span>

<span data-ttu-id="680f2-179">*Bild 4: CardinalityEstimationModelVersion har angetts till 70 när du använder en kompatibilitetsnivån 110 eller nedan.*</span><span class="sxs-lookup"><span data-stu-id="680f2-179">*Figure 4: The CardinalityEstimationModelVersion is set to 70 when using a compatibility level of 110 or below.*</span></span>

![Bild 4](./media/sql-database-compatibility-level-query-performance-130/figure-4.png)

<span data-ttu-id="680f2-181">Du kan också ändra kompatibilitetsnivån till 130 och inaktivera användningen av funktionen kardinalitet uppskattning med hjälp av LEGACY_CARDINALITY_ESTIMATION inställt på ON med [ALTER OMFÅNG DATABASKONFIGURATION](https://msdn.microsoft.com/library/mt629158.aspx).</span><span class="sxs-lookup"><span data-stu-id="680f2-181">Alternatively, you can change the compatibility level to 130, and disable the use of the new Cardinality Estimation function by using the LEGACY_CARDINALITY_ESTIMATION set to ON with [ALTER DATABASE SCOPED CONFIGURATION](https://msdn.microsoft.com/library/mt629158.aspx).</span></span> <span data-ttu-id="680f2-182">Det här är exakt detsamma som att använda 110 från en kardinalitet beräkning av funktionen synsätt, när du använder den senaste frågebearbetning kompatibilitetsnivå.</span><span class="sxs-lookup"><span data-stu-id="680f2-182">This will be exactly the same as using 110 from a Cardinality Estimation function point of view, while using the latest query processing compatibility level.</span></span> <span data-ttu-id="680f2-183">Då kan du dra nytta av nya funktioner kommer med den senaste kompatibilitetsnivån (d.v.s. batchläge) för frågebearbetning, men fortfarande är beroende av gamla kardinalitet beräkning av funktionerna om det behövs.</span><span class="sxs-lookup"><span data-stu-id="680f2-183">Doing so, you can benefit from the new query processing features coming with the latest compatibility level (i.e. batch mode), but still rely on the old Cardinality Estimation functionality if necessary.</span></span>

```
-- Old CE

ALTER DATABASE MyTestDB
    SET COMPATIBILITY_LEVEL = 130;
GO

ALTER DATABASE
    SCOPED CONFIGURATION
    SET LEGACY_CARDINALITY_ESTIMATION = ON;
GO

SET STATISTICS XML ON;

SELECT [c1]
    FROM [dbo].[T_target]
    WHERE [c1] > 20000;
GO

SET STATISTICS XML OFF;
```


<span data-ttu-id="680f2-184">Bara flyttas till kompatibilitetsnivå 120 eller 130 kan de nya funktionerna för beräkning av kardinalitet.</span><span class="sxs-lookup"><span data-stu-id="680f2-184">Simply moving to the compatibility level 120 or 130 enables the new Cardinality Estimation functionality.</span></span> <span data-ttu-id="680f2-185">I sådana fall standard CardinalityEstimationModelVersion anges därför till 120 eller 130 som visas nedan.</span><span class="sxs-lookup"><span data-stu-id="680f2-185">In such a case, the default CardinalityEstimationModelVersion will be set accordingly to 120 or 130 as visible below.</span></span>

```
-- New CE

ALTER DATABASE MyTestDB
    SET COMPATIBILITY_LEVEL = 130;
GO

ALTER DATABASE
    SCOPED CONFIGURATION
    SET LEGACY_CARDINALITY_ESTIMATION = OFF;
GO

SET STATISTICS XML ON;

SELECT [c1]
    FROM [dbo].[T_target]
    WHERE [c1] > 20000;
GO

SET STATISTICS XML OFF;
```


<span data-ttu-id="680f2-186">*Bild 5: CardinalityEstimationModelVersion har angetts till 130 när du använder en kompatibilitetsnivå 130.*</span><span class="sxs-lookup"><span data-stu-id="680f2-186">*Figure 5: The CardinalityEstimationModelVersion is set to 130 when using a compatibility level of 130.*</span></span>

![Bild 5](./media/sql-database-compatibility-level-query-performance-130/figure-5.jpg)

## <a name="witnessing-the-cardinality-estimation-differences"></a><span data-ttu-id="680f2-188">Bevittnat kardinalitet beräkning av skillnaderna</span><span class="sxs-lookup"><span data-stu-id="680f2-188">Witnessing the Cardinality Estimation differences</span></span>
<span data-ttu-id="680f2-189">Nu ska vi kör lite mer komplex fråga som involverar en INNER JOIN med en WHERE-sats med vissa predikat och titta vi på raden antal uppskattning från den gamla kardinalitet beräkning av funktionen först.</span><span class="sxs-lookup"><span data-stu-id="680f2-189">Now, let’s run a slightly more complex query involving an INNER JOIN with a WHERE clause with some predicates, and let’s look at the row count estimate from the old Cardinality Estimation function first.</span></span>

```
-- Old CE row estimate with INNER JOIN and WHERE clause

ALTER DATABASE MyTestDB
    SET COMPATIBILITY_LEVEL = 130;
GO

ALTER DATABASE
    SCOPED CONFIGURATION
    SET LEGACY_CARDINALITY_ESTIMATION = ON;
GO

SET STATISTICS XML ON;

SELECT T.[c2]
    FROM
                   [dbo].[T_source] S
        INNER JOIN [dbo].[T_target] T  ON T.c1=S.c1
    WHERE
        S.[Color] = ‘Red’  AND
        S.[c2] > 2000  AND
        T.[c2] > 2000
    OPTION (RECOMPILE);
GO

SET STATISTICS XML OFF;
```


<span data-ttu-id="680f2-190">Den här frågan körs effektivt returnerar 200,704 rader när raden uppskattning med funktionen gamla kardinalitet beräkning av anspråk 194,284 rader.</span><span class="sxs-lookup"><span data-stu-id="680f2-190">Executing this query effectively returns 200,704 rows, while the row estimate with the old Cardinality Estimation functionality claims 194,284 rows.</span></span> <span data-ttu-id="680f2-191">Naturligtvis, som säger innan dessa rad antal resultat beror också hur ofta du körde tidigare exemplen som fyller tabellerna exempel upprepade gånger vid varje körning.</span><span class="sxs-lookup"><span data-stu-id="680f2-191">Obviously, as said before, these row count results will also depend how often you ran the previous samples, which populates the sample tables over and over again at each run.</span></span> <span data-ttu-id="680f2-192">Naturligtvis är predikat i frågan har också en inverkan på den faktiska uppskattningen utöver tabellform innehåll, data och hur dessa data faktiskt korrelera med varandra.</span><span class="sxs-lookup"><span data-stu-id="680f2-192">Obviously, the predicates in your query will also have an influence on the actual estimation aside from the table shape, data content, and how this data actually correlate with each other.</span></span>

<span data-ttu-id="680f2-193">*Bild 6: Raden antal uppskattning är 194,284 eller 6 000 rader av från 200,704 rader som förväntat.*</span><span class="sxs-lookup"><span data-stu-id="680f2-193">*Figure 6: The row count estimate is 194,284 or 6,000 rows off from the 200,704 rows expected.*</span></span>

![Bild 6](./media/sql-database-compatibility-level-query-performance-130/figure-6.jpg)

<span data-ttu-id="680f2-195">På samma sätt du vi nu köra samma fråga med de nya funktionerna för beräkning av kardinalitet.</span><span class="sxs-lookup"><span data-stu-id="680f2-195">In the same way, let’s now execute the same query with the new Cardinality Estimation functionality.</span></span>

```
-- New CE row estimate with INNER JOIN and WHERE clause

ALTER DATABASE MyTestDB
    SET COMPATIBILITY_LEVEL = 130;
GO

ALTER DATABASE
    SCOPED CONFIGURATION
    SET LEGACY_CARDINALITY_ESTIMATION = OFF;
GO

SET STATISTICS XML ON;

SELECT T.[c2]
    FROM
                   [dbo].[T_source] S
        INNER JOIN [dbo].[T_target] T  ON T.c1=S.c1
    WHERE
        S.[Color] = ‘Red’  AND
        S.[c2] > 2000  AND
        T.[c2] > 2000
    OPTION (RECOMPILE);
GO

SET STATISTICS XML OFF;
```


<span data-ttu-id="680f2-196">Titta på den nedan, vi nu se att raden uppskattning är 202,877, eller mycket närmare och högre än den gamla kardinalitet uppskattning.</span><span class="sxs-lookup"><span data-stu-id="680f2-196">Looking at the below, we now see that the row estimate is 202,877, or much closer and higher than the old Cardinality Estimation.</span></span>

<span data-ttu-id="680f2-197">*Bild 7: Raden antal uppskattning är nu 202,877 i stället för 194,284.*</span><span class="sxs-lookup"><span data-stu-id="680f2-197">*Figure 7: The row count estimate is now 202,877, instead of 194,284.*</span></span>

![Bild 7](./media/sql-database-compatibility-level-query-performance-130/figure-7.jpg)

<span data-ttu-id="680f2-199">I verkligheten kan resultatet är 200,704 rader (men alla beror på hur ofta du körde frågorna av föregående exempel, men viktigare, eftersom TSQL använder instruktionen RAND(), de faktiska värden som returneras kan variera från att köras som en till nästa).</span><span class="sxs-lookup"><span data-stu-id="680f2-199">In reality, the result set is 200,704 rows (but all of it depends how often you did run the queries of the previous samples, but more importantly, because the TSQL uses the RAND() statement, the actual values returned can vary from one run to the next).</span></span> <span data-ttu-id="680f2-200">Därför i det här exemplet är nya kardinalitet uppskattning bättre på att uppskatta hur många rader eftersom 202,877 är mycket närmare 200,704, än 194,284!</span><span class="sxs-lookup"><span data-stu-id="680f2-200">Therefore, in this particular example, the new Cardinality Estimation does a better job at estimating the number of rows because 202,877 is much closer to 200,704, than 194,284!</span></span> <span data-ttu-id="680f2-201">Senaste om du ändrar WHERE-satsen predikat till likheten (snarare än ”>” för instans), detta kan vara en beräknar mellan gammalt och nya kardinalitet funktionen ännu mer olika beroende på hur många matchningar du kan hämta.</span><span class="sxs-lookup"><span data-stu-id="680f2-201">Last, if you change the WHERE clause predicates to equality (rather than “>” for instance), this could make the estimates between the old and new Cardinality function even more different, depending on how many matches you can get.</span></span>

<span data-ttu-id="680f2-202">Naturligtvis är i det här fallet representerar som ~ 6000 rader ut från det faktiska antalet inte en stor mängd information i vissa situationer.</span><span class="sxs-lookup"><span data-stu-id="680f2-202">Obviously, in this case, being ~6000 rows off from actual count does not represent a lot of data in some situations.</span></span> <span data-ttu-id="680f2-203">TRANSPONERA detta miljoner rader över flera tabeller och mer komplexa frågor och ibland uppskattning kan vara inaktiverat av miljoner rader och därmed risken för fel åtgärdsplan plockning upp eller begär inte tillräckligt med minne ger leder till TempDB spill och därför flera i/o är nu mycket högre.</span><span class="sxs-lookup"><span data-stu-id="680f2-203">Now, transpose this to millions of rows across several tables and more complex queries, and at times the estimate can be off by millions of rows , and therefore, the risk of picking-up the wrong execution plan, or requesting insufficient memory grants leading to TempDB spills, and so more I/O, are much higher.</span></span>

<span data-ttu-id="680f2-204">Om du har möjlighet praxis denna jämförelse med vanliga frågor och datauppsättningar, och se själv av hur mycket några av de gamla och nya beräkningarna påverkas, medan vissa bara kan bli mer ut från de i verkligheten använder eller några andra bara bara närmare till den faktiska raden räknar faktiskt returnerade i resultatmängder.</span><span class="sxs-lookup"><span data-stu-id="680f2-204">If you have the opportunity, practice this comparison with your most typical queries and datasets, and see for yourself by how much some of the old and new estimates are affected, while some could just become more off from the reality, or some others just simply closer to the actual row counts actually returned in the result sets.</span></span> <span data-ttu-id="680f2-205">Alla beror på formen av dina frågor, Azure SQL database-egenskaper, natur och storleken på dina datauppsättningar och tillgänglig om dem statistik.</span><span class="sxs-lookup"><span data-stu-id="680f2-205">All of it will depend of the shape of your queries, the Azure SQL database characteristics, the nature and the size of your datasets, and the statistics available about them.</span></span> <span data-ttu-id="680f2-206">Om du just har skapat din Azure SQL Database-instans måste Frågeoptimeringen skapa sin kunskap från grunden i stället för att återanvända statistik av de föregående frågan körs.</span><span class="sxs-lookup"><span data-stu-id="680f2-206">If you just created your Azure SQL Database instance, the query optimizer will have to build its knowledge from scratch instead of reusing statistics made of the previous query runs.</span></span> <span data-ttu-id="680f2-207">Beräkningarna är mycket kontextuella och nästan specifika för varje server- och situation.</span><span class="sxs-lookup"><span data-stu-id="680f2-207">So, the estimates are very contextual and almost specific to every server and application situation.</span></span> <span data-ttu-id="680f2-208">Det är en viktig aspekt att tänka på!</span><span class="sxs-lookup"><span data-stu-id="680f2-208">It is an important aspect to keep in mind!</span></span>

## <a name="some-considerations-to-take-into-account"></a><span data-ttu-id="680f2-209">Vissa aspekter att beakta</span><span class="sxs-lookup"><span data-stu-id="680f2-209">Some considerations to take into account</span></span>
<span data-ttu-id="680f2-210">De flesta arbetsbelastningar lönar sig att kompatibilitetsnivån 130, innan du anta kompatibilitetsnivån för produktionsmiljön, men du har i princip 3 alternativ:</span><span class="sxs-lookup"><span data-stu-id="680f2-210">Although most workloads would benefit from the compatibility level 130, before you adopting the compatibility level for your production environment, you basically have 3 options:</span></span>

1. <span data-ttu-id="680f2-211">Du flyttar till kompatibilitetsnivå 130 och se hur saker utföra.</span><span class="sxs-lookup"><span data-stu-id="680f2-211">You move to compatibility level 130, and see how things perform.</span></span> <span data-ttu-id="680f2-212">Om du märker att vissa regressioner du bara bara ange kompatibilitet tillbaka till den ursprungliga nivån eller behålla 130 och endast omvänd kardinalitet uppskattning tillbaka till bakåtkompatibelt läge (som beskrivs ovan, detta fristående kan åtgärda problemet).</span><span class="sxs-lookup"><span data-stu-id="680f2-212">In case you notice some regressions, you just simply set the compatibility level back to its original level, or keep 130, and only reverse the Cardinality Estimation back to the legacy mode (As explained above, this alone could address the issue).</span></span>
2. <span data-ttu-id="680f2-213">Testa din befintliga program under liknande produktion belastning, finjustera och validera prestanda innan du fortsätter till produktionen.</span><span class="sxs-lookup"><span data-stu-id="680f2-213">You thoroughly test your existing applications under similar production load, fine tune, and validate the performance before going to production.</span></span> <span data-ttu-id="680f2-214">Vid problem, samma som ovan, du kan alltid gå tillbaka till den ursprungliga kompatibilitetsnivån eller helt enkelt Återför kardinalitet uppskattning till bakåtkompatibelt läge.</span><span class="sxs-lookup"><span data-stu-id="680f2-214">In case of issues, same as above, you can always go back to the original compatibility level, or simply reverse the Cardinality Estimation back to the legacy mode.</span></span>
3. <span data-ttu-id="680f2-215">Sista alternativet, och det senaste sättet att åtgärda dessa frågor, är att använda Query Store.</span><span class="sxs-lookup"><span data-stu-id="680f2-215">As a final option, and the most recent way to address these questions, is to leverage the Query Store.</span></span> <span data-ttu-id="680f2-216">Det är dagens rekommenderade alternativet!</span><span class="sxs-lookup"><span data-stu-id="680f2-216">That’s today’s recommended option!</span></span> <span data-ttu-id="680f2-217">För att hjälpa analys av dina frågor under kompatibilitet nivå 120 eller nedan jämfört med 130, kan du gärna nog att använda Query Store.</span><span class="sxs-lookup"><span data-stu-id="680f2-217">To assist the analysis of your queries under compatibility level 120 or below versus 130, we cannot encourage you enough to use Query Store.</span></span> <span data-ttu-id="680f2-218">Query Store är tillgänglig med den senaste versionen av Azure SQL Database V12 och den har utformats för att hjälpa dig med felsökning av prestanda i frågan.</span><span class="sxs-lookup"><span data-stu-id="680f2-218">Query Store is available with the latest version of Azure SQL Database V12, and it’s designed to help you with query performance troubleshooting.</span></span> <span data-ttu-id="680f2-219">Query Store kan liknas vid en svarta lådan för data för din databas för att samla in och presentera historisk information om alla frågor.</span><span class="sxs-lookup"><span data-stu-id="680f2-219">Think of the Query Store as a flight data recorder for your database collecting and presenting detailed historic information about all queries.</span></span> <span data-ttu-id="680f2-220">Detta underlättar avsevärt dataforensik prestanda genom att minska tiden för att diagnostisera och lösa problem.</span><span class="sxs-lookup"><span data-stu-id="680f2-220">This greatly simplifies performance forensics by reducing the time to diagnose and resolve issues.</span></span> <span data-ttu-id="680f2-221">Du hittar mer information i [Query Store: en svarta lådan för data för databasen](https://azure.microsoft.com/blog/query-store-a-flight-data-recorder-for-your-database/).</span><span class="sxs-lookup"><span data-stu-id="680f2-221">You can find more information at [Query Store: A flight data recorder for your database](https://azure.microsoft.com/blog/query-store-a-flight-data-recorder-for-your-database/).</span></span>

<span data-ttu-id="680f2-222">AT på hög nivå, om du redan har en uppsättning databaser som körs i kompatibilitetsnivå 120 eller under och planerar att flytta en del av dem till 130 eller eftersom din arbetsbelastning automatiskt etablera nya databaser som kommer att vara snart anges som standard till 130, Överväg följande:</span><span class="sxs-lookup"><span data-stu-id="680f2-222">At the high-level, if you already have a set of databases running at compatibility level 120 or below, and plan to move some of them to 130, or because your workload automatically provision new databases that will be soon be set by default to 130, please consider the followings:</span></span>

* <span data-ttu-id="680f2-223">Aktivera Query Store innan du ändrar till nya kompatibilitetsnivån i produktion.</span><span class="sxs-lookup"><span data-stu-id="680f2-223">Before changing to the new compatibility level in production, enable Query Store.</span></span> <span data-ttu-id="680f2-224">Du kan referera till [ändra databasens kompatibilitetsläge och använda Frågearkivet](https://msdn.microsoft.com/library/bb895281.aspx) för mer information.</span><span class="sxs-lookup"><span data-stu-id="680f2-224">You can refer to [Change the Database Compatibility Mode and Use the Query Store](https://msdn.microsoft.com/library/bb895281.aspx) for more information.</span></span>
* <span data-ttu-id="680f2-225">Sedan testa alla kritiska arbetsbelastningar som använder representativt data och frågor av produktions-liknande miljö och jämföra prestanda erfarenhet och som rapporterades av Query Store.</span><span class="sxs-lookup"><span data-stu-id="680f2-225">Next, test all critical workloads using representative data and queries of a production-like environment, and compare the performance experienced and as reported by Query Store.</span></span> <span data-ttu-id="680f2-226">Om det uppstår några regressioner identifiera regressed frågor med Query Store och använda planen tvinga alternativet från Query Store (aka planera fästning).</span><span class="sxs-lookup"><span data-stu-id="680f2-226">If you experience some regressions, you can identify the regressed queries with the Query Store and use the plan forcing option from Query Store (aka plan pinning).</span></span> <span data-ttu-id="680f2-227">I sådana fall du slutgiltigt förblir med en kompatibilitetsnivå 130 och använda tidigare frågeplanen föreslaget av Query Store.</span><span class="sxs-lookup"><span data-stu-id="680f2-227">In such a case, you definitively stay with the compatibility level 130, and use the former query plan as suggested by the Query Store.</span></span>
* <span data-ttu-id="680f2-228">Om du vill utnyttja nya funktioner och funktioner i Azure SQL Database (som kör SQL Server 2016), men är känsliga för ändringar av kompatibilitetsnivån 130, i nödfall, kan du tvinga kompatibilitetsnivån tillbaka till den nivå som passar din arbetsbelastning med hjälp av ALTER DATABASE-instruktionen.</span><span class="sxs-lookup"><span data-stu-id="680f2-228">If you want to leverage new features and capabilities of Azure SQL Database (which is running SQL Server 2016), but are sensitive to changes brought by the compatibility level 130, as a last resort, you could consider forcing the compatibility level back to the level that suits your workload by using an ALTER DATABASE statement.</span></span> <span data-ttu-id="680f2-229">Men först måste vara medveten om att Query Store planen fästning alternativet är det bästa alternativet eftersom du inte använder 130 i princip används funktionalitetsnivån en äldre version av SQL Server.</span><span class="sxs-lookup"><span data-stu-id="680f2-229">But first, be aware that the Query Store plan pinning option is your best option because not using 130 is basically staying at the functionality level of an older SQL Server version.</span></span>
* <span data-ttu-id="680f2-230">Om du har flera program som sträcker sig över flera databaser kan vara det nödvändigt att uppdatera etablering logiken för dina databaser för att säkerställa en konsekvent kompatibilitetsnivå över alla databaser. de gamla och nya etablerats.</span><span class="sxs-lookup"><span data-stu-id="680f2-230">If you have multitenant applications spanning multiple databases, it may be necessary to update the provisioning logic of your databases to ensure a consistent compatibility level across all databases; old and newly provisioned ones.</span></span> <span data-ttu-id="680f2-231">Din arbetsbelastning programprestanda kan vara känslig till att vissa databaser körs vid olika kompatibilitetsnivåer och därför kompatibilitet nivå konsekvent på alla databaser kan vara nödvändiga för att erbjuda kunderna samma upplevelse alla i alla.</span><span class="sxs-lookup"><span data-stu-id="680f2-231">Your application workload performance could be sensitive to the fact that some databases are running at different compatibility levels, and therefore, compatibility level consistency across any database could be required in order to provide the same experience to your customers all across the board.</span></span> <span data-ttu-id="680f2-232">Observera att det inte är uppdrag, det beror på hur ditt program påverkas av kompatibilitetsnivå.</span><span class="sxs-lookup"><span data-stu-id="680f2-232">Note that it is not a mandate, it really depends on how your application is affected by the compatibility level.</span></span>
* <span data-ttu-id="680f2-233">Senast, om kardinalitet uppskattning och precis som ändrar kompatibilitetsnivå innan du fortsätter i produktion, rekommenderas att testa din produktion arbetsbelastning på nya villkor för att avgöra om ditt program fördelar från kardinalitet uppskattning förbättringar.</span><span class="sxs-lookup"><span data-stu-id="680f2-233">Last, regarding the Cardinality Estimation, and just like changing the compatibility level, before proceeding in production, it is recommended to test your production workload under the new conditions to determine if your application benefits from the Cardinality Estimation improvements.</span></span>

## <a name="conclusion"></a><span data-ttu-id="680f2-234">Slutsats</span><span class="sxs-lookup"><span data-stu-id="680f2-234">Conclusion</span></span>
<span data-ttu-id="680f2-235">Med Azure SQL Database för att dra nytta av alla SQL Server 2016 förbättringar kan du förbättra din fråga körningar tydligt.</span><span class="sxs-lookup"><span data-stu-id="680f2-235">Using Azure SQL Database to benefit from all SQL Server 2016 enhancements can clearly improve your query executions.</span></span> <span data-ttu-id="680f2-236">Precis som-är!</span><span class="sxs-lookup"><span data-stu-id="680f2-236">Just as-is!</span></span> <span data-ttu-id="680f2-237">Naturligtvis som en ny funktion, en ordentlig utvärdering måste du göra för att fastställa exakta villkor under vilka arbetsbelastningen databasen fungerar bäst.</span><span class="sxs-lookup"><span data-stu-id="680f2-237">Of course, like any new feature, a proper evaluation must be done to determine the exact conditions under which your database workload operates the best.</span></span> <span data-ttu-id="680f2-238">Erfarenheterna visar att de flesta arbetsbelastning förväntas minst köras transparent kompatibilitetsnivå 130, samtidigt utnyttja nya frågebearbetning funktioner och ny kardinalitet uppskattning.</span><span class="sxs-lookup"><span data-stu-id="680f2-238">Experience shows that most workload are expected to at least run transparently under compatibility level 130, while leveraging new query processing functions, and new Cardinality Estimation.</span></span> <span data-ttu-id="680f2-239">Som säger normalt är alltid är vissa undantag och göra korrekt betalning fordringar är en viktig bedömning att avgöra hur mycket du kan dra nytta av dessa förbättringar.</span><span class="sxs-lookup"><span data-stu-id="680f2-239">That said, realistically, there are always some exceptions and doing proper due diligence is an important assessment to determine how much you can benefit from these enhancements.</span></span> <span data-ttu-id="680f2-240">Och igen, Frågearkivet kan vara av en stor hjälp om du gör detta verk!</span><span class="sxs-lookup"><span data-stu-id="680f2-240">And again, the Query Store can be of a great help in doing this work!</span></span>

<span data-ttu-id="680f2-241">Då SQL Azure utvecklas, du kan förvänta dig en kompatibilitetsnivå 140 i framtiden.</span><span class="sxs-lookup"><span data-stu-id="680f2-241">As SQL Azure evolves, you can expect a compatibility level 140 in the future.</span></span> <span data-ttu-id="680f2-242">När tid är lämpligt, startas vi pratar om vad den här framtida kompatibilitetsnivå 140 kommer att få, precis som vi kort beskrivs här vilka kompatibilitetsnivå 130 gör i dag.</span><span class="sxs-lookup"><span data-stu-id="680f2-242">When time is appropriate, we will start talking about what this future compatibility level 140 will bring, just as we briefly discussed here what compatibility level 130 is bringing today.</span></span>

<span data-ttu-id="680f2-243">Nu ska vi inte glömma, från juni 2016 är Azure SQL Database ändras kompatibilitetsnivån standard från 120 till 130 för nyskapade databaser.</span><span class="sxs-lookup"><span data-stu-id="680f2-243">For now, let’s not forget, starting June 2016, Azure SQL Database will change the default compatibility level from 120 to 130 for newly created databases.</span></span> <span data-ttu-id="680f2-244">Tänk!</span><span class="sxs-lookup"><span data-stu-id="680f2-244">Be aware!</span></span>

## <a name="references"></a><span data-ttu-id="680f2-245">Referenser</span><span class="sxs-lookup"><span data-stu-id="680f2-245">References</span></span>
* [<span data-ttu-id="680f2-246">Vad är nytt i databasmotor</span><span class="sxs-lookup"><span data-stu-id="680f2-246">What’s New in Database Engine</span></span>](https://msdn.microsoft.com/library/bb510411.aspx#InMemory)
* [<span data-ttu-id="680f2-247">Blogg: Query Store: en svarta lådan för data för databasen, av Borko Novakovic, juni 8 2016</span><span class="sxs-lookup"><span data-stu-id="680f2-247">Blog: Query Store: A flight data recorder for your database, by Borko Novakovic, June 8 2016</span></span>](https://azure.microsoft.com/blog/query-store-a-flight-data-recorder-for-your-database/)
* [<span data-ttu-id="680f2-248">Ändra DATABASENS kompatibilitetsnivån (Transact-SQL)</span><span class="sxs-lookup"><span data-stu-id="680f2-248">ALTER DATABASE Compatibility Level (Transact-SQL)</span></span>](https://msdn.microsoft.com/library/bb510680.aspx)
* [<span data-ttu-id="680f2-249">ALTER DATABASE OMFÅNG KONFIGURATION</span><span class="sxs-lookup"><span data-stu-id="680f2-249">ALTER DATABASE SCOPED CONFIGURATION</span></span>](https://msdn.microsoft.com/library/mt629158.aspx)
* [<span data-ttu-id="680f2-250">Kompatibilitetsnivån 130 för Azure SQL Database V12</span><span class="sxs-lookup"><span data-stu-id="680f2-250">Compatibility Level 130 for Azure SQL Database V12</span></span>](https://azure.microsoft.com/updates/compatibility-level-130-for-azure-sql-database-v12/)
* [<span data-ttu-id="680f2-251">Optimera din fråga planer med SQLServer 2014 kardinalitet exteriörbedömning</span><span class="sxs-lookup"><span data-stu-id="680f2-251">Optimizing Your Query Plans with the SQL Server 2014 Cardinality Estimator</span></span>](https://msdn.microsoft.com/library/dn673537.aspx)
* [<span data-ttu-id="680f2-252">Guide för Columnstore-index</span><span class="sxs-lookup"><span data-stu-id="680f2-252">Columnstore Indexes Guide</span></span>](https://msdn.microsoft.com/library/gg492088.aspx)
* [<span data-ttu-id="680f2-253">Blogg: Förbättrad prestanda för frågor med kompatibilitetsnivå 130 i Azure SQL-databas med Alain Lissoir maj 6 2016</span><span class="sxs-lookup"><span data-stu-id="680f2-253">Blog: Improved Query Performance with Compatibility Level 130 in Azure SQL Database, by Alain Lissoir, May 6 2016</span></span>](https://blogs.msdn.microsoft.com/sqlserverstorageengine/2016/05/06/improved-query-performance-with-compatibility-level-130-in-azure-sql-database/)

<!--
Improved Query Performance with Compatibility Level 130 in Azure SQL Database

May 6, 2016 by Alain Lissoir (AlainL), on GitHub 'alainlissoir'.

https://blogs.msdn.microsoft.com/sqlserverstorageengine/2016/05/06/improved-query-performance-with-compatibility-level-130-in-azure-sql-database/

..... Now, above.
....................
..... Soon, below?

CAPS / MSDN ideally, but instead on ACom:
.. # Assess effects of latest compatibility level on query performance, how to

sql-database-compatibility-level-query-performance-130.md

genemi = MightyPen , 2016-05-20  Friday  17:00pm
-->
