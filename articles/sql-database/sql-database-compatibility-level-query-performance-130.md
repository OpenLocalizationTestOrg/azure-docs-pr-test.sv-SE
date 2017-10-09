---
title: "aaaDatabase kompatibilitetsnivå 130 - Azure SQL Database | Microsoft Docs"
description: "I den här artikeln vi utforska hello fördelarna med att köra Azure SQL Database på kompatibilitetsnivå 130 och utnyttja hello fördelarna med hello nya frågeoptimeraren och fråga processorfunktioner. Vi tar också upp hello möjliga sidoeffekter på hello frågeprestanda för hello befintliga SQL-program."
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
ms.openlocfilehash: 25693c5f7b01405b7073fa7d4cc2833fbe125e2f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="improved-query-performance-with-compatibility-level-130-in-azure-sql-database"></a><span data-ttu-id="c2ea7-104">Förbättrad prestanda för frågor med nivå 130 i Azure SQL Database</span><span class="sxs-lookup"><span data-stu-id="c2ea7-104">Improved query performance with compatibility Level 130 in Azure SQL Database</span></span>
<span data-ttu-id="c2ea7-105">Azure SQL Database körs transparent hundratals tusentals databaser på många olika kompatibilitetsnivåer bevara och garanterar hello bakåtkompatibilitet toohello motsvarande version av Microsoft SQL Server för alla kunder!</span><span class="sxs-lookup"><span data-stu-id="c2ea7-105">Azure SQL Database is running transparently hundreds of thousands of databases at many different compatibility levels, preserving and guaranteeing hello backward compatibility toohello corresponding version of Microsoft SQL Server for all its customers!</span></span>

<span data-ttu-id="c2ea7-106">I den här artikeln vi utforska hello fördelarna med kör din Azure SQL-Databse på kompatibilitetsnivå 130 och utnyttja hello fördelarna med hello nya frågeoptimeraren och fråga processorfunktioner.</span><span class="sxs-lookup"><span data-stu-id="c2ea7-106">In this article, we explore hello benefits of running your Azure SQL Databse at compatibility level 130, and leveraging hello benefits of hello new query optimizer and query processor features.</span></span> <span data-ttu-id="c2ea7-107">Vi tar också upp hello möjliga sidoeffekter på hello frågeprestanda för hello befintliga SQL-program.</span><span class="sxs-lookup"><span data-stu-id="c2ea7-107">We also address hello possible side-effects on hello query performance for hello existing SQL applications.</span></span>

<span data-ttu-id="c2ea7-108">Som en påminnelse om historik hello justeringen för SQL-versioner toodefault kompatibilitetsnivåer är följande:</span><span class="sxs-lookup"><span data-stu-id="c2ea7-108">As a reminder of history, hello alignment of SQL versions toodefault compatibility levels are as follows:</span></span>

* <span data-ttu-id="c2ea7-109">100: i SQL Server 2008 och Azure SQL Database V11.</span><span class="sxs-lookup"><span data-stu-id="c2ea7-109">100: in SQL Server 2008 and Azure SQL Database V11.</span></span>
* <span data-ttu-id="c2ea7-110">110: i SQL Server 2012 och Azure SQL Database V11.</span><span class="sxs-lookup"><span data-stu-id="c2ea7-110">110: in SQL Server 2012 and Azure SQL Database V11.</span></span>
* <span data-ttu-id="c2ea7-111">120: i SQL Server 2014 och Azure SQL Database V12.</span><span class="sxs-lookup"><span data-stu-id="c2ea7-111">120: in SQL Server 2014 and Azure SQL Database V12.</span></span>
* <span data-ttu-id="c2ea7-112">130: i SQL Server 2016 och Azure SQL-databasen (V12).</span><span class="sxs-lookup"><span data-stu-id="c2ea7-112">130: in SQL Server 2016 and Azure SQL Database V12.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="c2ea7-113">Från och med **mid juni 2016**, i Azure SQL Database hello standard kompatibilitetsnivå blir 130 i stället för 120 för **nyskapad** databaser.</span><span class="sxs-lookup"><span data-stu-id="c2ea7-113">Starting in **mid-June 2016**, in Azure SQL Database, hello default compatibility level will be 130 instead of 120 for **newly created** databases.</span></span>
> 
> <span data-ttu-id="c2ea7-114">Databaser som skapats före mitten av juni 2016 kommer *inte* påverkas och kommer bibehålla sina aktuella kompatibilitetsnivån (100, 110 eller 120).</span><span class="sxs-lookup"><span data-stu-id="c2ea7-114">Databases created before mid-June 2016 will *not* be affected, and will maintain their current compatibility level (100, 110, or 120).</span></span> <span data-ttu-id="c2ea7-115">Databaser som migrerats från Azure SQL Database version V11 tooV12 har en kompatibilitetsnivå 100 eller 110.</span><span class="sxs-lookup"><span data-stu-id="c2ea7-115">Databases that migrated from Azure SQL Database version V11 tooV12 will have a compatibility level of either 100 or 110.</span></span> 
> 

## <a name="about-compatibility-level-130"></a><span data-ttu-id="c2ea7-116">Om kompatibilitetsnivåer 130</span><span class="sxs-lookup"><span data-stu-id="c2ea7-116">About compatibility level 130</span></span>
<span data-ttu-id="c2ea7-117">Om du vill tooknow hello aktuella kompatibilitetsnivån för databasen du först köra hello följande Transact-SQL-instruktion.</span><span class="sxs-lookup"><span data-stu-id="c2ea7-117">First, if you want tooknow hello current compatibility level of your database, execute hello following Transact-SQL statement.</span></span>

```
SELECT compatibility_level
    FROM sys.databases
    WHERE name = '<YOUR DATABASE_NAME>’;
```


<span data-ttu-id="c2ea7-118">Före den här ändringen sker toolevel 130 för **nyligen** skapat databaser, vi gå igenom den här ändringen handlar om till exempel grundläggande frågan och se hur vem som helst kan dra nytta av den.</span><span class="sxs-lookup"><span data-stu-id="c2ea7-118">Before this change toolevel 130 happens for **newly** created databases, let’s review what this change is all about through some very basic query examples, and see how anyone can benefit from it.</span></span>

<span data-ttu-id="c2ea7-119">Frågebearbetning i relationsdatabaser kan vara väldigt komplexa och kan leda till toolots datorn vetenskap, räkning toounderstand hello inbyggd designalternativen och beteenden.</span><span class="sxs-lookup"><span data-stu-id="c2ea7-119">Query processing in relational databases can be very complex and can lead toolots of computer science and mathematics toounderstand hello inherent design choices and behaviors.</span></span> <span data-ttu-id="c2ea7-120">I det här dokumentet har hello innehåll avsiktligt förenklad tooensure att vem som helst med vissa minsta teknisk bakgrund förstår hello effekten av hello kompatibilitet förändring och ta reda på hur det kan göra program.</span><span class="sxs-lookup"><span data-stu-id="c2ea7-120">In this document, hello content has been intentionally simplified tooensure that anyone with some minimum technical background can understand hello impact of hello compatibility level change and determine how it can benefit applications.</span></span>

<span data-ttu-id="c2ea7-121">Vi har en titt på vilka hello kompatibilitetsnivå 130 ger när hello tabellen.</span><span class="sxs-lookup"><span data-stu-id="c2ea7-121">Let’s have a quick look at what hello compatibility level 130 brings at hello table.</span></span>  <span data-ttu-id="c2ea7-122">Du hittar mer information i [ändra DATABASENS kompatibilitetsnivån (Transact-SQL)](https://msdn.microsoft.com/library/bb510680.aspx), men här är en kort sammanfattning:</span><span class="sxs-lookup"><span data-stu-id="c2ea7-122">You can find more details at [ALTER DATABASE Compatibility Level (Transact-SQL)](https://msdn.microsoft.com/library/bb510680.aspx), but here is a short summary:</span></span>

* <span data-ttu-id="c2ea7-123">hello Insert-åtgärden för en Insert select-instruktion kan vara flertrådade eller en parallell plan medan innan den här åtgärden var Enkeltrådig.</span><span class="sxs-lookup"><span data-stu-id="c2ea7-123">hello Insert operation of an Insert-select statement can be multi-threaded or can have a parallel plan, while before this operation was single-threaded.</span></span>
* <span data-ttu-id="c2ea7-124">Optimerad tabell och tabellen variabler frågor kan nu använda parallella planer minne medan innan den här åtgärden även var Enkeltrådig.</span><span class="sxs-lookup"><span data-stu-id="c2ea7-124">Memory Optimized table and table variables queries can now have parallel plans, while before this operation was also single-threaded .</span></span>
* <span data-ttu-id="c2ea7-125">Statistik för Minnesoptimerade tabellen kan nu samlas in och kan uppdateras automatiskt.</span><span class="sxs-lookup"><span data-stu-id="c2ea7-125">Statistics for Memory Optimized table can now be sampled and are auto-updated.</span></span> <span data-ttu-id="c2ea7-126">Se [vad är nytt i Database Engine: Minnesintern OLTP](https://msdn.microsoft.com/library/bb510411.aspx#InMemory) för mer information.</span><span class="sxs-lookup"><span data-stu-id="c2ea7-126">See [What's New in Database Engine: In-Memory OLTP](https://msdn.microsoft.com/library/bb510411.aspx#InMemory) for more details.</span></span>
* <span data-ttu-id="c2ea7-127">Batch-läge v/s radläge ändras med kolumnen index</span><span class="sxs-lookup"><span data-stu-id="c2ea7-127">Batch mode v/s Row Mode changes with Column Store indexes</span></span>
  * <span data-ttu-id="c2ea7-128">Sorteringar för en tabell med en kolumn index är nu i batch-läge.</span><span class="sxs-lookup"><span data-stu-id="c2ea7-128">Sorts on a table with a Column Store index are now in batch mode.</span></span>
  * <span data-ttu-id="c2ea7-129">Fönsterhantering mängder fungerar nu i batchläge, till exempel TSQL FÖRDRÖJNING/LEAD instruktioner.</span><span class="sxs-lookup"><span data-stu-id="c2ea7-129">Windowing aggregates now operate in batch mode such as TSQL LAG/LEAD statements.</span></span>
  * <span data-ttu-id="c2ea7-130">Frågor om kolumnen Store tabeller med flera distinct-satser fungerar i Batch-läge.</span><span class="sxs-lookup"><span data-stu-id="c2ea7-130">Queries on Column Store tables with Multiple distinct clauses operate in Batch mode.</span></span>
  * <span data-ttu-id="c2ea7-131">Frågor som körs under DOP = 1 eller med en seriell plan också köras i Batch-läge.</span><span class="sxs-lookup"><span data-stu-id="c2ea7-131">Queries running under DOP=1 or with a serial plan also execute in Batch Mode.</span></span>
* <span data-ttu-id="c2ea7-132">Senast, kardinalitet beräkning av förbättringar som faktiskt kommer med kompatibilitetsnivå 120, men för de körs på en lägre kompatibilitet (d.v.s. 100 eller 110) hello flytta toocompatibility nivå 130 kommer också att få dessa förbättringar och de kan också Dra hello frågeprestandan för dina program.</span><span class="sxs-lookup"><span data-stu-id="c2ea7-132">Last, Cardinality Estimation improvements are actually coming with compatibility level 120, but for those of you running at a lower Compatibility level (i.e. 100, or 110), hello move toocompatibility level 130 will also bring these improvements, and these can also benefit hello query performance of your applications.</span></span>

## <a name="practicing-compatibility-level-130"></a><span data-ttu-id="c2ea7-133">Öva kompatibilitetsnivå 130</span><span class="sxs-lookup"><span data-stu-id="c2ea7-133">Practicing compatibility level 130</span></span>
<span data-ttu-id="c2ea7-134">Första ska få några tabeller, index och slumpmässiga data som har skapats toopractice några av de nya funktionerna.</span><span class="sxs-lookup"><span data-stu-id="c2ea7-134">First let’s get some tables, indexes and random data created toopractice some of these new capabilities.</span></span> <span data-ttu-id="c2ea7-135">exempel på hello TSQL-skript kan köras under SQL Server 2016 eller under Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="c2ea7-135">hello TSQL script examples can be executed under SQL Server 2016, or under Azure SQL Database.</span></span> <span data-ttu-id="c2ea7-136">Men när du skapar en Azure SQL database, kontrollera att du väljer på hello minsta en P2 databasen eftersom du måste ha minst ett antal kärnor tooallow flertrådsteknik och därför utnyttja dessa funktioner.</span><span class="sxs-lookup"><span data-stu-id="c2ea7-136">However, when creating an Azure SQL database, make sure you choose at hello minimum a P2 database because you need at least a couple of cores tooallow multi-threading and therefore benefit from these features.</span></span>

```
-- Create a Premium P2 Database in Azure SQL Database

CREATE DATABASE MyTestDB
    (EDITION=’Premium’, SERVICE_OBJECTIVE=’P2′);
GO

-- Create 2 tables with a column store index on
-- hello second one (only available on Premium databases)

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


<span data-ttu-id="c2ea7-137">Nu har vi en titt toosome hello bearbetning av frågan funktioner kommer med kompatibilitetsnivå 130.</span><span class="sxs-lookup"><span data-stu-id="c2ea7-137">Now, let’s have a look toosome of hello Query Processing features coming with compatibility level 130.</span></span>

## <a name="parallel-insert"></a><span data-ttu-id="c2ea7-138">Parallell INSERT</span><span class="sxs-lookup"><span data-stu-id="c2ea7-138">Parallel INSERT</span></span>
<span data-ttu-id="c2ea7-139">Köra hello TSQL instruktionerna nedan kör hello INSERT-åtgärden under kompatibilitetsnivå 120 och 130, som utför respektive hello INSERT-åtgärden i en enda trådad modell (120) och i ett flertrådat modellen (130).</span><span class="sxs-lookup"><span data-stu-id="c2ea7-139">Executing hello TSQL statements below executes hello INSERT operation under compatibility level 120 and 130, which respectively executes hello INSERT operation in a single threaded model (120), and in a multi-threaded model (130).</span></span>

```
-- Parallel INSERT … SELECT … in heap or CCI
-- is available under 130 only

SET STATISTICS XML ON;

ALTER DATABASE MyTestDB
    SET COMPATIBILITY_LEVEL = 120;
GO 

-- hello INSERT part is in serial

INSERT t_target WITH (tablock)
    SELECT C1, COUNT(C2) * 10 * RAND()
        FROM T_source
        GROUP BY C1
    OPTION (RECOMPILE);

ALTER DATABASE MyTestDB
    SET COMPATIBILITY_LEVEL = 130
GO

-- hello INSERT part is in parallel

INSERT t_target WITH (tablock)
    SELECT C1, COUNT(C2) * 10 * RAND()
        FROM T_source
        GROUP BY C1
    OPTION (RECOMPILE);

SET STATISTICS XML OFF;
```


<span data-ttu-id="c2ea7-140">Du kan bestämma vilka kardinalitet beräkning av funktionen är i play genom att begära hello faktiska hello frågeplan, titta på dess grafisk representation eller dess XML-innehåll.</span><span class="sxs-lookup"><span data-stu-id="c2ea7-140">By requesting hello actual hello query plan, looking at its graphical representation or its XML content, you can determine which Cardinality Estimation function is at play.</span></span> <span data-ttu-id="c2ea7-141">Granskar hello planer sida-vid-sida i figur 1 tydligt ser vi att hello kolumnen Store Infoga körning går från seriella i 120 tooparallel i 130.</span><span class="sxs-lookup"><span data-stu-id="c2ea7-141">Looking at hello plans side-by-side on figure 1, we can clearly see that hello Column Store INSERT execution goes from serial in 120 tooparallel in 130.</span></span> <span data-ttu-id="c2ea7-142">Observera också hello ändringen av hello iterator ikon i hello 130 plan för två parallella pilar illustrerar hello fakta som nu hello iterator-körningen är verkligen parallellt.</span><span class="sxs-lookup"><span data-stu-id="c2ea7-142">Also, note that hello change of hello iterator icon in hello 130 plan showing two parallel arrows, illustrating hello fact that now hello iterator execution is indeed parallel.</span></span> <span data-ttu-id="c2ea7-143">Om du har stora INSERT operations toocomplete fungerar hello parallell körning, länkade toohello antal kärnor som du har på tillgänglig för hello databasen bättre; beroende din situation in tooa 100 gånger snabbare!</span><span class="sxs-lookup"><span data-stu-id="c2ea7-143">If you have large INSERT operations toocomplete, hello parallel execution, linked toohello number of core you have at your disposal for hello database, will perform better; up tooa 100 times faster depending your situation!</span></span>

<span data-ttu-id="c2ea7-144">*Bild 1: Infoga åtgärd ändringarna från seriella tooparallel med kompatibilitetsnivå 130.*</span><span class="sxs-lookup"><span data-stu-id="c2ea7-144">*Figure 1: INSERT operation changes from serial tooparallel with compatibility level 130.*</span></span>

![Bild 1](./media/sql-database-compatibility-level-query-performance-130/figure-1.jpg)

## <a name="serial-batch-mode"></a><span data-ttu-id="c2ea7-146">SERIELL Batch-läge</span><span class="sxs-lookup"><span data-stu-id="c2ea7-146">SERIAL Batch Mode</span></span>
<span data-ttu-id="c2ea7-147">Flytta toocompatibility nivå 130 vid bearbetning av datarader aktiveras på motsvarande sätt, batchbearbetning läge.</span><span class="sxs-lookup"><span data-stu-id="c2ea7-147">Similarly, moving toocompatibility level 130 when processing rows of data enables batch mode processing.</span></span> <span data-ttu-id="c2ea7-148">Först finns batchåtgärder läge bara när du har ett columnstore-index på plats.</span><span class="sxs-lookup"><span data-stu-id="c2ea7-148">First, batch mode operations  are only available when you have a column store index in place.</span></span> <span data-ttu-id="c2ea7-149">Därefter en batch vanligtvis representerar ~ 900 rader och använder en kod logik som är optimerade för flera kärnor CPU, högre dataflödet i minnet och direkt använder hello komprimerade data för hello kolumnen Store när det är möjligt.</span><span class="sxs-lookup"><span data-stu-id="c2ea7-149">Second, a batch typically represents ~900 rows, and uses a code logic optimized for multicore CPU, higher memory throughput and directly leverages hello compressed data of hello Column Store whenever possible.</span></span> <span data-ttu-id="c2ea7-150">Under dessa förhållanden SQL Server 2016 kan bearbeta ~ 900 rader samtidigt, i stället för 1 rad hello tidpunkt och följaktligen hello övergripande omkostnaden för hello åtgärden nu delas av hello hela batchen, minska hello övergripande kostnader per rad.</span><span class="sxs-lookup"><span data-stu-id="c2ea7-150">Under these conditions, SQL Server 2016 can process ~900 rows at once, instead of 1 row at hello time, and as a consequence, hello overall overhead cost of hello operation is now shared by hello entire batch, reducing hello overall cost by row.</span></span> <span data-ttu-id="c2ea7-151">Den här delade mängd operationer som tillsammans med hello kolumnen store komprimering i princip minskar hello latens ingår i en SELECT batch-läge.</span><span class="sxs-lookup"><span data-stu-id="c2ea7-151">This shared amount of operations combined with hello column store compression basically reduces hello latency involved in a SELECT batch mode operation.</span></span> <span data-ttu-id="c2ea7-152">Du kan hitta mer information om hello kolumnen store och batchläge på [Columnstore-index guiden](https://msdn.microsoft.com/library/gg492088.aspx).</span><span class="sxs-lookup"><span data-stu-id="c2ea7-152">You can find more details about hello column store and batch mode at [Columnstore Indexes Guide](https://msdn.microsoft.com/library/gg492088.aspx).</span></span>

```
-- Serial batch mode execution

SET STATISTICS XML ON;

ALTER DATABASE MyTestDB
    SET COMPATIBILITY_LEVEL = 120;
GO

-- hello scan and aggregate are in row mode

SELECT C1, COUNT (C2)
    FROM T_target
    GROUP BY C1
    OPTION (MAXDOP 1, RECOMPILE);
GO

ALTER DATABASE MyTestDB
    SET COMPATIBILITY_LEVEL = 130;
GO 

-- hello scan and aggregate are in batch mode,
-- and force MAXDOP too1 tooshow that batch mode
-- also now works in serial mode.

SELECT C1, COUNT(C2)
    FROM T_target
    GROUP BY C1
    OPTION (MAXDOP 1, RECOMPILE);
GO

SET STATISTICS XML OFF;
```


<span data-ttu-id="c2ea7-153">Som visas nedan, ser genom att följa hello frågan planer sida-vid-sida på bild 2, vi att hello bearbetningsläget har ändrats med hello kompatibilitetsnivå och följaktligen när du kör hello frågor i båda kompatibilitetsnivå helt, kan vi se som de flesta av hello bearbetningstid används i rad läget (86%) jämfört med toohello batch-läge (14%), där 2 batchar har bearbetats.</span><span class="sxs-lookup"><span data-stu-id="c2ea7-153">As visible below, by observing hello query plans side-by-side on figure 2, we can see that hello processing mode has changed with hello compatibility level, and as a consequence, when executing hello queries in both compatibility level altogether, we can see that most of hello processing time is spent in row mode (86%) compared toohello batch mode (14%), where 2 batches have been processed.</span></span> <span data-ttu-id="c2ea7-154">Öka hello dataset, hello förmånen ökar.</span><span class="sxs-lookup"><span data-stu-id="c2ea7-154">Increase hello dataset, hello benefit will increase.</span></span>

<span data-ttu-id="c2ea7-155">*Bild 2: Välj åtgärden ändringar från seriella toobatch läge med en kompatibilitetsnivå 130.*</span><span class="sxs-lookup"><span data-stu-id="c2ea7-155">*Figure 2: SELECT operation changes from serial toobatch mode with compatibility level 130.*</span></span>

![Bild 2](./media/sql-database-compatibility-level-query-performance-130/figure-2.jpg)

## <a name="batch-mode-on-sort-execution"></a><span data-ttu-id="c2ea7-157">Batch-läge på Sortera körning</span><span class="sxs-lookup"><span data-stu-id="c2ea7-157">Batch mode on Sort Execution</span></span>
<span data-ttu-id="c2ea7-158">Liknande toohello ovan, men tillämpade tooa sorteringsåtgärden hello övergången från radläge läge (kompatibilitetsnivå 120) toobatch (kompatibilitetsnivå 130) förbättrar hello prestanda på hello sorteringsåtgärden för hello samma orsaker.</span><span class="sxs-lookup"><span data-stu-id="c2ea7-158">Similar toohello above, but applied tooa sort operation, hello transition from row mode (compatibility level 120) toobatch mode (compatibility level 130) improves hello performance of hello SORT operation for hello same reasons.</span></span>

```
-- Batch mode on sort execution

SET STATISTICS XML ON;

ALTER DATABASE MyTestDB
    SET COMPATIBILITY_LEVEL = 120;
GO

-- hello scan and aggregate are in row mode

SELECT C1, COUNT(C2)
    FROM T_target
    GROUP BY C1
    ORDER BY C1
    OPTION (MAXDOP 1, RECOMPILE);
GO

ALTER DATABASE MyTestDB
    SET COMPATIBILITY_LEVEL = 130;
GO

-- hello scan and aggregate are in batch mode,
-- and force MAXDOP too1 tooshow that batch mode
-- also now works in serial mode.

SELECT C1, COUNT(C2)
    FROM T_target
    GROUP BY C1
    ORDER BY C1
    OPTION (MAXDOP 1, RECOMPILE);
GO

SET STATISTICS XML OFF;
```


<span data-ttu-id="c2ea7-159">Synliga sida-vid-sida på figur 3, vi se att hello sorteringsåtgärden i radläge representerar 81% av hello kostnad, medan hello batchläge representerar bara 19% av hello kostnaden (respektive 81% och 56% på hello sortera sig själv).</span><span class="sxs-lookup"><span data-stu-id="c2ea7-159">Visible side-by-side on figure 3, we can see that hello sort operation in row mode represents 81% of hello cost, while hello batch mode only represents 19% of hello cost (respectively 81% and 56% on hello sort itself).</span></span>

<span data-ttu-id="c2ea7-160">*Bild 3: Sorteringsåtgärden ändras från toobatch radläge med kompatibilitetsnivå 130.*</span><span class="sxs-lookup"><span data-stu-id="c2ea7-160">*Figure 3: SORT operation changes from row toobatch mode with compatibility level 130.*</span></span>

![Bild 3](./media/sql-database-compatibility-level-query-performance-130/figure-3.png)

<span data-ttu-id="c2ea7-162">Naturligtvis är innehålla exemplen endast rader, tiotusentals som ingenting när du tittar på hello data är tillgängliga i de flesta SQL Servers dessa dagar.</span><span class="sxs-lookup"><span data-stu-id="c2ea7-162">Obviously, these samples only contain tens of thousands of rows, which is nothing when looking at hello data available in most SQL Servers these days.</span></span> <span data-ttu-id="c2ea7-163">Projektet bara dessa mot miljoner rader i stället detta kan översätta i flera minuter för körning av undantas varje dag väntande hello uppbyggnad din arbetsbelastning.</span><span class="sxs-lookup"><span data-stu-id="c2ea7-163">Just project these against millions of rows instead, and this can translate in several minutes of execution spared every day pending hello nature of your workload.</span></span>

## <a name="cardinality-estimation-ce-improvements"></a><span data-ttu-id="c2ea7-164">Förbättringar av kardinalitet uppskattning (CE)</span><span class="sxs-lookup"><span data-stu-id="c2ea7-164">Cardinality Estimation (CE) improvements</span></span>
<span data-ttu-id="c2ea7-165">Introducerades i SQL Server 2014, en databas som körs på en kompatibilitetsnivå 120 eller över kommer att använda hello nya kardinalitet beräkning av funktioner.</span><span class="sxs-lookup"><span data-stu-id="c2ea7-165">Introduced with SQL Server 2014, any database running at a compatibility level 120 or above will make use of hello new Cardinality Estimation functionality.</span></span> <span data-ttu-id="c2ea7-166">Beräkning av kardinalitet är i princip hello logik används toodetermine hur SQLServer ska köra en fråga baserat på dess uppskattade kostnaden.</span><span class="sxs-lookup"><span data-stu-id="c2ea7-166">Essentially, cardinality estimation is hello logic used toodetermine how SQL server will execute a query based on its estimated cost.</span></span> <span data-ttu-id="c2ea7-167">beräkning av hello beräknas med hjälp av indata från statistik som är associerade med objekt som är inblandade i frågan.</span><span class="sxs-lookup"><span data-stu-id="c2ea7-167">hello estimation is calculated using input from statistics associated with objects involved in that query.</span></span> <span data-ttu-id="c2ea7-168">Praktiskt taget, på en hög nivå kardinalitet beräkning av funktioner är raden antal uppskattningar tillsammans med information om hello distribution av hello värden, distinkta värdet inventeringar, och dubbla räknas som ingår i hello tabeller och objekt som refereras till i hello frågan.</span><span class="sxs-lookup"><span data-stu-id="c2ea7-168">Practically, at a high-level, Cardinality Estimation functions are row count estimates along with information about hello distribution of hello values, distinct value counts, and duplicate counts contained in hello tables and objects referenced in hello query.</span></span> <span data-ttu-id="c2ea7-169">Hämta dessa uppskattningar fel kan leda till att toounnecessary disk i/o på grund av tooinsufficient minne ger (d.v.s. TempDB spill) eller tooa val av en seriell plan körning via en parallell planerar körning och tooname några.</span><span class="sxs-lookup"><span data-stu-id="c2ea7-169">Getting these estimates wrong, can lead toounnecessary disk I/O due tooinsufficient memory grants (i.e. TempDB spills), or tooa selection of a serial plan execution over a parallel plan execution, tooname a few.</span></span> <span data-ttu-id="c2ea7-170">Slutsats, felaktig beräknar leda tooan övergripande prestanda försämras av hello Frågekörningen.</span><span class="sxs-lookup"><span data-stu-id="c2ea7-170">Conclusion, incorrect estimates can lead tooan overall performance degradation of hello query execution.</span></span> <span data-ttu-id="c2ea7-171">Hej på andra sidan, bättre uppskattningar, exaktare beräknar, leads toobetter frågan körningar!</span><span class="sxs-lookup"><span data-stu-id="c2ea7-171">On hello other side, better estimates, more accurate estimates, leads toobetter query executions!</span></span>

<span data-ttu-id="c2ea7-172">Som tidigare nämnts, fråga optimeringar och beräknar är en komplex fråga, men om du vill toolearn mer om frågeplaner och kardinalitet exteriörbedömning, kan du läsa toohello dokumentet på [optimera din frågeplaner med hello SQL Server 2014 Kardinalitet exteriörbedömning](https://msdn.microsoft.com/library/dn673537.aspx) för en mer grundlig genomgång.</span><span class="sxs-lookup"><span data-stu-id="c2ea7-172">As mentioned before, query optimizations and estimates are a complex matter, but if you want toolearn more about query plans and cardinality estimator, you can refer toohello document at [Optimizing Your Query Plans with hello SQL Server 2014 Cardinality Estimator](https://msdn.microsoft.com/library/dn673537.aspx) for a deeper dive.</span></span>

## <a name="which-cardinality-estimation-do-you-currently-use"></a><span data-ttu-id="c2ea7-173">Vilka kardinalitet beräkning av du för närvarande använder?</span><span class="sxs-lookup"><span data-stu-id="c2ea7-173">Which Cardinality Estimation do you currently use?</span></span>
<span data-ttu-id="c2ea7-174">toodetermine under vilka kardinalitet beräkning av dina frågor körs, vi bara Använd hello frågan exempel nedan.</span><span class="sxs-lookup"><span data-stu-id="c2ea7-174">toodetermine under which Cardinality Estimation your queries are running, let’s just use hello query samples below.</span></span> <span data-ttu-id="c2ea7-175">Observera att det här första exemplet ska köras under kompatibilitetsnivån 110, innebär hello användning av hello gamla kardinalitet beräkning av funktioner.</span><span class="sxs-lookup"><span data-stu-id="c2ea7-175">Note that this first example will run under compatibility level 110, implying hello use of hello old Cardinality Estimation functions.</span></span>

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


<span data-ttu-id="c2ea7-176">När körningen har slutförts klickar du på hello XML-länk och titta på hello egenskaper för hello första iterator enligt nedan.</span><span class="sxs-lookup"><span data-stu-id="c2ea7-176">Once execution is complete, click on hello XML link, and look at hello properties of hello first iterator as shown below.</span></span> <span data-ttu-id="c2ea7-177">Observera hello egenskapsnamn som kallas CardinalityEstimationModelVersion som för närvarande inställd på 70.</span><span class="sxs-lookup"><span data-stu-id="c2ea7-177">Note hello property name called CardinalityEstimationModelVersion currently set on 70.</span></span> <span data-ttu-id="c2ea7-178">Det innebär inte att hello databasens kompatibilitetsnivå är toohello SQL Server 7.0 version (den är inställd på 110 som visas i hello TSQL-uttryck ovanför), men hello värdet 70 representerar bara hello äldre kardinalitet beräkning av funktionerna eftersom SQL Server 7.0, som hade ingen större ändringar förrän SQL Server 2014 (som levereras med en kompatibilitetsnivå på 120).</span><span class="sxs-lookup"><span data-stu-id="c2ea7-178">It does not mean that hello database compatibility level is set toohello SQL Server 7.0 version (it is set on 110 as visible in hello TSQL statements above), but hello value 70 simply represents hello legacy Cardinality Estimation functionality available since SQL Server 7.0, which had no major revisions until SQL Server 2014 (which comes with a compatibility level of 120).</span></span>

<span data-ttu-id="c2ea7-179">*Bild 4: hello CardinalityEstimationModelVersion anges too70 när du använder en kompatibilitetsnivån 110 eller nedan.*</span><span class="sxs-lookup"><span data-stu-id="c2ea7-179">*Figure 4: hello CardinalityEstimationModelVersion is set too70 when using a compatibility level of 110 or below.*</span></span>

![Bild 4](./media/sql-database-compatibility-level-query-performance-130/figure-4.png)

<span data-ttu-id="c2ea7-181">Du kan också du kan ändra hello kompatibilitet nivå too130 och inaktiverar hello användning av hello nya kardinalitet beräkning av funktionen med hjälp av hello LEGACY_CARDINALITY_ESTIMATION ange tooON med [ALTER OMFÅNG DATABASKONFIGURATION](https://msdn.microsoft.com/library/mt629158.aspx).</span><span class="sxs-lookup"><span data-stu-id="c2ea7-181">Alternatively, you can change hello compatibility level too130, and disable hello use of hello new Cardinality Estimation function by using hello LEGACY_CARDINALITY_ESTIMATION set tooON with [ALTER DATABASE SCOPED CONFIGURATION](https://msdn.microsoft.com/library/mt629158.aspx).</span></span> <span data-ttu-id="c2ea7-182">Detta kommer att exakt hello samma som med 110 från en kardinalitet beräkning av funktionen synsätt, när du använder hello senaste frågebearbetning kompatibilitetsnivå.</span><span class="sxs-lookup"><span data-stu-id="c2ea7-182">This will be exactly hello same as using 110 from a Cardinality Estimation function point of view, while using hello latest query processing compatibility level.</span></span> <span data-ttu-id="c2ea7-183">Då kan du dra nytta av hello nya frågebearbetning funktioner kommer med hello senaste kompatibilitetsnivå (d.v.s. batchläge), men fortfarande är beroende av hello gamla kardinalitet beräkning av funktionerna om det behövs.</span><span class="sxs-lookup"><span data-stu-id="c2ea7-183">Doing so, you can benefit from hello new query processing features coming with hello latest compatibility level (i.e. batch mode), but still rely on hello old Cardinality Estimation functionality if necessary.</span></span>

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


<span data-ttu-id="c2ea7-184">Bara flytta toohello kompatibilitetsnivå 120 eller 130 kan hello nya kardinalitet beräkning av funktioner.</span><span class="sxs-lookup"><span data-stu-id="c2ea7-184">Simply moving toohello compatibility level 120 or 130 enables hello new Cardinality Estimation functionality.</span></span> <span data-ttu-id="c2ea7-185">I sådana fall anges hello standard CardinalityEstimationModelVersion därför too120 eller 130 som visas nedan.</span><span class="sxs-lookup"><span data-stu-id="c2ea7-185">In such a case, hello default CardinalityEstimationModelVersion will be set accordingly too120 or 130 as visible below.</span></span>

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


<span data-ttu-id="c2ea7-186">*Bild 5: hello CardinalityEstimationModelVersion anges too130 när du använder en kompatibilitetsnivå 130.*</span><span class="sxs-lookup"><span data-stu-id="c2ea7-186">*Figure 5: hello CardinalityEstimationModelVersion is set too130 when using a compatibility level of 130.*</span></span>

![Bild 5](./media/sql-database-compatibility-level-query-performance-130/figure-5.jpg)

## <a name="witnessing-hello-cardinality-estimation-differences"></a><span data-ttu-id="c2ea7-188">Bevittnat hello kardinalitet uppskattning skillnader</span><span class="sxs-lookup"><span data-stu-id="c2ea7-188">Witnessing hello Cardinality Estimation differences</span></span>
<span data-ttu-id="c2ea7-189">Nu ska vi kör lite mer komplex fråga som involverar en INNER JOIN med en WHERE-sats med vissa predikat och till hur känna hello rad antal uppskattning från hello gamla kardinalitet beräkning av funktionen först.</span><span class="sxs-lookup"><span data-stu-id="c2ea7-189">Now, let’s run a slightly more complex query involving an INNER JOIN with a WHERE clause with some predicates, and let’s look at hello row count estimate from hello old Cardinality Estimation function first.</span></span>

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


<span data-ttu-id="c2ea7-190">Den här frågan körs effektivt returnerar 200,704 rader medan hello raden uppskattning med hello gamla kardinalitet uppskattning funktionalitet anspråk 194,284 rader.</span><span class="sxs-lookup"><span data-stu-id="c2ea7-190">Executing this query effectively returns 200,704 rows, while hello row estimate with hello old Cardinality Estimation functionality claims 194,284 rows.</span></span> <span data-ttu-id="c2ea7-191">Naturligtvis, som säger innan dessa rad antal resultat beror också hur ofta du körde hello tidigare prov som fyller hello Exempeltabeller upprepade gånger vid varje körning.</span><span class="sxs-lookup"><span data-stu-id="c2ea7-191">Obviously, as said before, these row count results will also depend how often you ran hello previous samples, which populates hello sample tables over and over again at each run.</span></span> <span data-ttu-id="c2ea7-192">Naturligtvis, har också hello predikat i frågan påverkar hello faktiska uppskattning utöver hello tabellform datainnehållet och hur dessa data faktiskt korrelera med varandra.</span><span class="sxs-lookup"><span data-stu-id="c2ea7-192">Obviously, hello predicates in your query will also have an influence on hello actual estimation aside from hello table shape, data content, and how this data actually correlate with each other.</span></span>

<span data-ttu-id="c2ea7-193">*Bild 6: hello rad antal uppskattning är 194,284 eller 6 000 rader ut från hello 200,704 rader som förväntat.*</span><span class="sxs-lookup"><span data-stu-id="c2ea7-193">*Figure 6: hello row count estimate is 194,284 or 6,000 rows off from hello 200,704 rows expected.*</span></span>

![Bild 6](./media/sql-database-compatibility-level-query-performance-130/figure-6.jpg)

<span data-ttu-id="c2ea7-195">I hello samma sätt kan vi nu köra hello samma fråga med hello nya kardinalitet beräkning av funktioner.</span><span class="sxs-lookup"><span data-stu-id="c2ea7-195">In hello same way, let’s now execute hello same query with hello new Cardinality Estimation functionality.</span></span>

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


<span data-ttu-id="c2ea7-196">Titta på hello nedan Se vi nu att hello raden uppskattning är 202,877, eller mycket närmare och högre än hello gamla kardinalitet uppskattning.</span><span class="sxs-lookup"><span data-stu-id="c2ea7-196">Looking at hello below, we now see that hello row estimate is 202,877, or much closer and higher than hello old Cardinality Estimation.</span></span>

<span data-ttu-id="c2ea7-197">*Bild 7: hello rad antal uppskattning är nu 202,877 i stället för 194,284.*</span><span class="sxs-lookup"><span data-stu-id="c2ea7-197">*Figure 7: hello row count estimate is now 202,877, instead of 194,284.*</span></span>

![Bild 7](./media/sql-database-compatibility-level-query-performance-130/figure-7.jpg)

<span data-ttu-id="c2ea7-199">I själva verket hello resultatmängden är 200,704 rader (men alla beror på hur ofta du körde hello frågor för hello föregående exempel, men viktigare, eftersom hello TSQL använder hello RAND() instruktionen, hello faktiska värden som returneras kan variera från en kör toohello nästa).</span><span class="sxs-lookup"><span data-stu-id="c2ea7-199">In reality, hello result set is 200,704 rows (but all of it depends how often you did run hello queries of hello previous samples, but more importantly, because hello TSQL uses hello RAND() statement, hello actual values returned can vary from one run toohello next).</span></span> <span data-ttu-id="c2ea7-200">Därför i det här exemplet är hello nya kardinalitet uppskattning bättre på att uppskatta hello antalet rader eftersom 202,877 är mycket närmare too200, 704, än 194,284!</span><span class="sxs-lookup"><span data-stu-id="c2ea7-200">Therefore, in this particular example, hello new Cardinality Estimation does a better job at estimating hello number of rows because 202,877 is much closer too200,704, than 194,284!</span></span> <span data-ttu-id="c2ea7-201">Senaste om du ändrar hello WHERE-satsen predikat tooequality (snarare än ”>” för instans), det gick att hello beräknar mellan hello gamla och nya kardinalitet funktionen ännu mer olika, beroende på hur många matchningar som du kan hämta.</span><span class="sxs-lookup"><span data-stu-id="c2ea7-201">Last, if you change hello WHERE clause predicates tooequality (rather than “>” for instance), this could make hello estimates between hello old and new Cardinality function even more different, depending on how many matches you can get.</span></span>

<span data-ttu-id="c2ea7-202">Naturligtvis är i det här fallet representerar som ~ 6000 rader ut från det faktiska antalet inte en stor mängd information i vissa situationer.</span><span class="sxs-lookup"><span data-stu-id="c2ea7-202">Obviously, in this case, being ~6000 rows off from actual count does not represent a lot of data in some situations.</span></span> <span data-ttu-id="c2ea7-203">Nu kan införliva denna toomillions rader över flera tabeller och mer komplexa frågor och ibland hello uppskattning kan vara inaktiverat av miljoner rader och därför hello risken för plockning upp hello fel åtgärdsplan eller begär inte tillräckligt med minne ger inledande tooTempDB spill och därför flera i/o är mycket högre.</span><span class="sxs-lookup"><span data-stu-id="c2ea7-203">Now, transpose this toomillions of rows across several tables and more complex queries, and at times hello estimate can be off by millions of rows , and therefore, hello risk of picking-up hello wrong execution plan, or requesting insufficient memory grants leading tooTempDB spills, and so more I/O, are much higher.</span></span>

<span data-ttu-id="c2ea7-204">Om du har möjlighet hello öva denna jämförelse med vanliga frågor och datauppsättningar och se själv med hur mycket vissa hello gamla och nya beräknar påverkas, medan vissa bara kan bli mer ut från hello verkligheten eller några andra bara bara närmare radantalet toohello faktiska faktiskt returneras i hello resultatmängder.</span><span class="sxs-lookup"><span data-stu-id="c2ea7-204">If you have hello opportunity, practice this comparison with your most typical queries and datasets, and see for yourself by how much some of hello old and new estimates are affected, while some could just become more off from hello reality, or some others just simply closer toohello actual row counts actually returned in hello result sets.</span></span> <span data-ttu-id="c2ea7-205">Alla beror på hello form av dina frågor, hello Azure SQL database egenskaper, hello hello storlek och dina datauppsättningar och hello statistik om dem.</span><span class="sxs-lookup"><span data-stu-id="c2ea7-205">All of it will depend of hello shape of your queries, hello Azure SQL database characteristics, hello nature and hello size of your datasets, and hello statistics available about them.</span></span> <span data-ttu-id="c2ea7-206">Om du just har skapat din Azure SQL Database-instans, hello fråga optimering har toobuild körs sin kunskap från grunden i stället för att återanvända statistik av hello föregående frågan.</span><span class="sxs-lookup"><span data-stu-id="c2ea7-206">If you just created your Azure SQL Database instance, hello query optimizer will have toobuild its knowledge from scratch instead of reusing statistics made of hello previous query runs.</span></span> <span data-ttu-id="c2ea7-207">Hello uppskattningar är så mycket kontextuella och nästan specifika tooevery server- och situation.</span><span class="sxs-lookup"><span data-stu-id="c2ea7-207">So, hello estimates are very contextual and almost specific tooevery server and application situation.</span></span> <span data-ttu-id="c2ea7-208">Det är en viktig aspekt tookeep i åtanke!</span><span class="sxs-lookup"><span data-stu-id="c2ea7-208">It is an important aspect tookeep in mind!</span></span>

## <a name="some-considerations-tootake-into-account"></a><span data-ttu-id="c2ea7-209">Vissa överväganden tootake hänsyn</span><span class="sxs-lookup"><span data-stu-id="c2ea7-209">Some considerations tootake into account</span></span>
<span data-ttu-id="c2ea7-210">De flesta arbetsbelastningar skulle utnyttja hello kompatibilitetsnivå 130, innan du använder hello kompatibilitetsnivån för produktionsmiljön, men du har i princip 3 alternativ:</span><span class="sxs-lookup"><span data-stu-id="c2ea7-210">Although most workloads would benefit from hello compatibility level 130, before you adopting hello compatibility level for your production environment, you basically have 3 options:</span></span>

1. <span data-ttu-id="c2ea7-211">Du flytta toocompatibility nivå 130 och se hur saker utföra.</span><span class="sxs-lookup"><span data-stu-id="c2ea7-211">You move toocompatibility level 130, and see how things perform.</span></span> <span data-ttu-id="c2ea7-212">Om du upptäcker några regressioner bara bara ange hello kompatibilitet nivån tillbaka tooits ursprungliga nivå eller behålla 130 och endast återföra hello kardinalitet uppskattning tillbaka toohello bakåtkompatibelt läge (som beskrivs ovan, detta fristående kunde adress hello problemet).</span><span class="sxs-lookup"><span data-stu-id="c2ea7-212">In case you notice some regressions, you just simply set hello compatibility level back tooits original level, or keep 130, and only reverse hello Cardinality Estimation back toohello legacy mode (As explained above, this alone could address hello issue).</span></span>
2. <span data-ttu-id="c2ea7-213">Testa din befintliga program under liknande produktion belastning, finjustera och validera hello prestanda innan pågående tooproduction.</span><span class="sxs-lookup"><span data-stu-id="c2ea7-213">You thoroughly test your existing applications under similar production load, fine tune, and validate hello performance before going tooproduction.</span></span> <span data-ttu-id="c2ea7-214">Vid problem, samma som ovan, du kan alltid gå tillbaka toohello ursprungliga kompatibilitetsnivå eller helt enkelt omvänd hello kardinalitet uppskattning tillbaka toohello bakåtkompatibelt läge.</span><span class="sxs-lookup"><span data-stu-id="c2ea7-214">In case of issues, same as above, you can always go back toohello original compatibility level, or simply reverse hello Cardinality Estimation back toohello legacy mode.</span></span>
3. <span data-ttu-id="c2ea7-215">Som en sista alternativet och hello senaste sätt tooaddress dessa frågor är tooleverage hello Query Store.</span><span class="sxs-lookup"><span data-stu-id="c2ea7-215">As a final option, and hello most recent way tooaddress these questions, is tooleverage hello Query Store.</span></span> <span data-ttu-id="c2ea7-216">Det är dagens rekommenderade alternativet!</span><span class="sxs-lookup"><span data-stu-id="c2ea7-216">That’s today’s recommended option!</span></span> <span data-ttu-id="c2ea7-217">tooassist hello analys av dina frågor under kompatibilitet nivå 120 eller nedan jämfört med 130, kan du gärna tillräckligt med toouse Query Store.</span><span class="sxs-lookup"><span data-stu-id="c2ea7-217">tooassist hello analysis of your queries under compatibility level 120 or below versus 130, we cannot encourage you enough toouse Query Store.</span></span> <span data-ttu-id="c2ea7-218">Query Store är tillgänglig med hello senaste versionen av Azure SQL Database V12 och den är utformad för toohelp du felsökningen av prestanda i frågan.</span><span class="sxs-lookup"><span data-stu-id="c2ea7-218">Query Store is available with hello latest version of Azure SQL Database V12, and it’s designed toohelp you with query performance troubleshooting.</span></span> <span data-ttu-id="c2ea7-219">Se hello Query Store som ett svarta lådan för data för din databas för att samla in och presentera historisk information om alla frågor.</span><span class="sxs-lookup"><span data-stu-id="c2ea7-219">Think of hello Query Store as a flight data recorder for your database collecting and presenting detailed historic information about all queries.</span></span> <span data-ttu-id="c2ea7-220">Detta förenklar dataforensik prestanda genom att minska hello tid toodiagnose avsevärt och lösa problem.</span><span class="sxs-lookup"><span data-stu-id="c2ea7-220">This greatly simplifies performance forensics by reducing hello time toodiagnose and resolve issues.</span></span> <span data-ttu-id="c2ea7-221">Du hittar mer information i [Query Store: en svarta lådan för data för databasen](https://azure.microsoft.com/blog/query-store-a-flight-data-recorder-for-your-database/).</span><span class="sxs-lookup"><span data-stu-id="c2ea7-221">You can find more information at [Query Store: A flight data recorder for your database](https://azure.microsoft.com/blog/query-store-a-flight-data-recorder-for-your-database/).</span></span>

<span data-ttu-id="c2ea7-222">På hög nivå om du redan har en uppsättning databaser som körs i kompatibilitetsnivå 120 eller under och planera toomove hello några av dem too130, eller att din arbetsbelastning automatiskt etablera nya databaser som ska snart vara som standard too130, Överväg hello följande:</span><span class="sxs-lookup"><span data-stu-id="c2ea7-222">At hello high-level, if you already have a set of databases running at compatibility level 120 or below, and plan toomove some of them too130, or because your workload automatically provision new databases that will be soon be set by default too130, please consider hello followings:</span></span>

* <span data-ttu-id="c2ea7-223">Aktivera Query Store innan du ändrar toohello nya kompatibilitetsnivå i produktion.</span><span class="sxs-lookup"><span data-stu-id="c2ea7-223">Before changing toohello new compatibility level in production, enable Query Store.</span></span> <span data-ttu-id="c2ea7-224">Du kan se för[ändra hello databasens kompatibilitetsläge och använda hello Query Store](https://msdn.microsoft.com/library/bb895281.aspx) för mer information.</span><span class="sxs-lookup"><span data-stu-id="c2ea7-224">You can refer too[Change hello Database Compatibility Mode and Use hello Query Store](https://msdn.microsoft.com/library/bb895281.aspx) for more information.</span></span>
* <span data-ttu-id="c2ea7-225">Sedan testa alla kritiska arbetsbelastningar som använder representativt data och frågor av produktions-liknande miljö och jämföra hello prestanda erfarenhet och som rapporterades av Query Store.</span><span class="sxs-lookup"><span data-stu-id="c2ea7-225">Next, test all critical workloads using representative data and queries of a production-like environment, and compare hello performance experienced and as reported by Query Store.</span></span> <span data-ttu-id="c2ea7-226">Om det uppstår några regressioner, kan du identifiera hello regressed frågor med hello Query Store och använda hello plan för att tvinga alternativet från Query Store (aka planera fästning).</span><span class="sxs-lookup"><span data-stu-id="c2ea7-226">If you experience some regressions, you can identify hello regressed queries with hello Query Store and use hello plan forcing option from Query Store (aka plan pinning).</span></span> <span data-ttu-id="c2ea7-227">I så fall kan du slutgiltigt förblir med hello kompatibilitetsnivå 130 och använda hello tidigare frågeplan som föreslagna av hello Query Store.</span><span class="sxs-lookup"><span data-stu-id="c2ea7-227">In such a case, you definitively stay with hello compatibility level 130, and use hello former query plan as suggested by hello Query Store.</span></span>
* <span data-ttu-id="c2ea7-228">Om du vill tooleverage nya funktioner och funktioner i Azure SQL Database (som kör SQL Server 2016), men är känsliga toochanges som hello kompatibilitetsnivå 130, som en sista utväg kan du tvinga hello kompatibilitetsnivå tillbaka toohello nivå som passar din arbetsbelastning med hjälp av ALTER DATABASE-instruktionen.</span><span class="sxs-lookup"><span data-stu-id="c2ea7-228">If you want tooleverage new features and capabilities of Azure SQL Database (which is running SQL Server 2016), but are sensitive toochanges brought by hello compatibility level 130, as a last resort, you could consider forcing hello compatibility level back toohello level that suits your workload by using an ALTER DATABASE statement.</span></span> <span data-ttu-id="c2ea7-229">Men Tänk först hello Query Store planen fästning alternativet är det bästa alternativet eftersom du inte använder 130 i princip används funktionalitetsnivån hello en äldre version av SQL Server.</span><span class="sxs-lookup"><span data-stu-id="c2ea7-229">But first, be aware that hello Query Store plan pinning option is your best option because not using 130 is basically staying at hello functionality level of an older SQL Server version.</span></span>
* <span data-ttu-id="c2ea7-230">Om du har flera program som sträcker sig över flera databaser kan det vara nödvändigt tooupdate hello etablering logiken för dina databaser tooensure en konsekvent kompatibilitetsnivå över alla databaser. de gamla och nya etablerats.</span><span class="sxs-lookup"><span data-stu-id="c2ea7-230">If you have multitenant applications spanning multiple databases, it may be necessary tooupdate hello provisioning logic of your databases tooensure a consistent compatibility level across all databases; old and newly provisioned ones.</span></span> <span data-ttu-id="c2ea7-231">Din arbetsbelastning programprestanda kan vara känslig toohello faktum att vissa databaser körs vid olika kompatibilitetsnivåer och därför kompatibilitet nivå konsekvent på alla databaser kan krävas för tooprovide hello samma få tooyour kunder alla över hello-kort.</span><span class="sxs-lookup"><span data-stu-id="c2ea7-231">Your application workload performance could be sensitive toohello fact that some databases are running at different compatibility levels, and therefore, compatibility level consistency across any database could be required in order tooprovide hello same experience tooyour customers all across hello board.</span></span> <span data-ttu-id="c2ea7-232">Observera att det inte är uppdrag, det beror på hur ditt program påverkas av hello kompatibilitetsnivå.</span><span class="sxs-lookup"><span data-stu-id="c2ea7-232">Note that it is not a mandate, it really depends on how your application is affected by hello compatibility level.</span></span>
* <span data-ttu-id="c2ea7-233">Senast, om hello kardinalitet uppskattning och precis som ändrar hello kompatibilitetsnivå innan du fortsätter i produktion, är det rekommenderade tootest produktion arbetsbelastningen under hello nya villkor toodetermine om tillämpningsprogrammet fördelar från hello kardinalitet uppskattning förbättringar.</span><span class="sxs-lookup"><span data-stu-id="c2ea7-233">Last, regarding hello Cardinality Estimation, and just like changing hello compatibility level, before proceeding in production, it is recommended tootest your production workload under hello new conditions toodetermine if your application benefits from hello Cardinality Estimation improvements.</span></span>

## <a name="conclusion"></a><span data-ttu-id="c2ea7-234">Slutsats</span><span class="sxs-lookup"><span data-stu-id="c2ea7-234">Conclusion</span></span>
<span data-ttu-id="c2ea7-235">Med Azure SQL Database kan toobenefit från alla SQL Server 2016 förbättringar tydligt förbättra din fråga körningar.</span><span class="sxs-lookup"><span data-stu-id="c2ea7-235">Using Azure SQL Database toobenefit from all SQL Server 2016 enhancements can clearly improve your query executions.</span></span> <span data-ttu-id="c2ea7-236">Precis som-är!</span><span class="sxs-lookup"><span data-stu-id="c2ea7-236">Just as-is!</span></span> <span data-ttu-id="c2ea7-237">Naturligtvis som en ny funktion, måste en ordentlig utvärdering göras toodetermine hello exakt villkor under vilka arbetsbelastningen databasen fungerar hello bäst.</span><span class="sxs-lookup"><span data-stu-id="c2ea7-237">Of course, like any new feature, a proper evaluation must be done toodetermine hello exact conditions under which your database workload operates hello best.</span></span> <span data-ttu-id="c2ea7-238">Erfarenheterna visar att de flesta arbetsbelastningen är förväntade tooat minst köras transparent kompatibilitetsnivå 130, samtidigt utnyttja nya frågebearbetning funktioner och ny kardinalitet uppskattning.</span><span class="sxs-lookup"><span data-stu-id="c2ea7-238">Experience shows that most workload are expected tooat least run transparently under compatibility level 130, while leveraging new query processing functions, and new Cardinality Estimation.</span></span> <span data-ttu-id="c2ea7-239">Som säger normalt är alltid är vissa undantag och göra korrekt betalning fordringar är ett viktigt assessment toodetermine hur mycket du kan dra nytta av dessa förbättringar.</span><span class="sxs-lookup"><span data-stu-id="c2ea7-239">That said, realistically, there are always some exceptions and doing proper due diligence is an important assessment toodetermine how much you can benefit from these enhancements.</span></span> <span data-ttu-id="c2ea7-240">Och igen, hello Query Store kan vara av en stor hjälp om du gör detta verk!</span><span class="sxs-lookup"><span data-stu-id="c2ea7-240">And again, hello Query Store can be of a great help in doing this work!</span></span>

<span data-ttu-id="c2ea7-241">Då SQL Azure utvecklas, du kan förvänta dig en kompatibilitetsnivå 140 i hello framtida.</span><span class="sxs-lookup"><span data-stu-id="c2ea7-241">As SQL Azure evolves, you can expect a compatibility level 140 in hello future.</span></span> <span data-ttu-id="c2ea7-242">När tid är lämpligt, startas vi pratar om vad den här framtida kompatibilitetsnivå 140 kommer att få, precis som vi kort beskrivs här vilka kompatibilitetsnivå 130 gör i dag.</span><span class="sxs-lookup"><span data-stu-id="c2ea7-242">When time is appropriate, we will start talking about what this future compatibility level 140 will bring, just as we briefly discussed here what compatibility level 130 is bringing today.</span></span>

<span data-ttu-id="c2ea7-243">Nu ska vi inte glömma, från juni 2016 är Azure SQL Database ändras hello standard kompatibilitetsnivå från 120 too130 för nyskapade databaser.</span><span class="sxs-lookup"><span data-stu-id="c2ea7-243">For now, let’s not forget, starting June 2016, Azure SQL Database will change hello default compatibility level from 120 too130 for newly created databases.</span></span> <span data-ttu-id="c2ea7-244">Tänk!</span><span class="sxs-lookup"><span data-stu-id="c2ea7-244">Be aware!</span></span>

## <a name="references"></a><span data-ttu-id="c2ea7-245">Referenser</span><span class="sxs-lookup"><span data-stu-id="c2ea7-245">References</span></span>
* [<span data-ttu-id="c2ea7-246">Vad är nytt i databasmotor</span><span class="sxs-lookup"><span data-stu-id="c2ea7-246">What’s New in Database Engine</span></span>](https://msdn.microsoft.com/library/bb510411.aspx#InMemory)
* [<span data-ttu-id="c2ea7-247">Blogg: Query Store: en svarta lådan för data för databasen, av Borko Novakovic, juni 8 2016</span><span class="sxs-lookup"><span data-stu-id="c2ea7-247">Blog: Query Store: A flight data recorder for your database, by Borko Novakovic, June 8 2016</span></span>](https://azure.microsoft.com/blog/query-store-a-flight-data-recorder-for-your-database/)
* [<span data-ttu-id="c2ea7-248">Ändra DATABASENS kompatibilitetsnivån (Transact-SQL)</span><span class="sxs-lookup"><span data-stu-id="c2ea7-248">ALTER DATABASE Compatibility Level (Transact-SQL)</span></span>](https://msdn.microsoft.com/library/bb510680.aspx)
* [<span data-ttu-id="c2ea7-249">ALTER DATABASE OMFÅNG KONFIGURATION</span><span class="sxs-lookup"><span data-stu-id="c2ea7-249">ALTER DATABASE SCOPED CONFIGURATION</span></span>](https://msdn.microsoft.com/library/mt629158.aspx)
* [<span data-ttu-id="c2ea7-250">Kompatibilitetsnivån 130 för Azure SQL Database V12</span><span class="sxs-lookup"><span data-stu-id="c2ea7-250">Compatibility Level 130 for Azure SQL Database V12</span></span>](https://azure.microsoft.com/updates/compatibility-level-130-for-azure-sql-database-v12/)
* [<span data-ttu-id="c2ea7-251">Optimera din fråga planer med hello SQL Server 2014 kardinalitet exteriörbedömning</span><span class="sxs-lookup"><span data-stu-id="c2ea7-251">Optimizing Your Query Plans with hello SQL Server 2014 Cardinality Estimator</span></span>](https://msdn.microsoft.com/library/dn673537.aspx)
* [<span data-ttu-id="c2ea7-252">Guide för Columnstore-index</span><span class="sxs-lookup"><span data-stu-id="c2ea7-252">Columnstore Indexes Guide</span></span>](https://msdn.microsoft.com/library/gg492088.aspx)
* [<span data-ttu-id="c2ea7-253">Blogg: Förbättrad prestanda för frågor med kompatibilitetsnivå 130 i Azure SQL-databas med Alain Lissoir maj 6 2016</span><span class="sxs-lookup"><span data-stu-id="c2ea7-253">Blog: Improved Query Performance with Compatibility Level 130 in Azure SQL Database, by Alain Lissoir, May 6 2016</span></span>](https://blogs.msdn.microsoft.com/sqlserverstorageengine/2016/05/06/improved-query-performance-with-compatibility-level-130-in-azure-sql-database/)

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
