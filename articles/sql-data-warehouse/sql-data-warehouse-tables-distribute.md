---
title: aaaDistributing tabeller i SQL Data Warehouse | Microsoft Docs
description: "Komma igång med att distribuera tabeller i Azure SQL Data Warehouse."
services: sql-data-warehouse
documentationcenter: NA
author: shivaniguptamsft
manager: barbkess
editor: 
ms.assetid: 5ed4337f-7262-4ef6-8fd6-1809ce9634fc
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: tables
ms.date: 10/31/2016
ms.author: shigu;barbkess
ms.openlocfilehash: 65093eeaeb00fef85aaa6070da2c976fed3f4bbe
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="distributing-tables-in-sql-data-warehouse"></a><span data-ttu-id="51769-103">Distribuera tabeller i SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="51769-103">Distributing tables in SQL Data Warehouse</span></span>
> [!div class="op_single_selector"]
> * <span data-ttu-id="51769-104">[Översikt över][Overview]</span><span class="sxs-lookup"><span data-stu-id="51769-104">[Overview][Overview]</span></span>
> * <span data-ttu-id="51769-105">[Datatyper][Data Types]</span><span class="sxs-lookup"><span data-stu-id="51769-105">[Data Types][Data Types]</span></span>
> * <span data-ttu-id="51769-106">[Distribuera][Distribute]</span><span class="sxs-lookup"><span data-stu-id="51769-106">[Distribute][Distribute]</span></span>
> * <span data-ttu-id="51769-107">[Index][Index]</span><span class="sxs-lookup"><span data-stu-id="51769-107">[Index][Index]</span></span>
> * <span data-ttu-id="51769-108">[Partition][Partition]</span><span class="sxs-lookup"><span data-stu-id="51769-108">[Partition][Partition]</span></span>
> * <span data-ttu-id="51769-109">[Statistik][Statistics]</span><span class="sxs-lookup"><span data-stu-id="51769-109">[Statistics][Statistics]</span></span>
> * <span data-ttu-id="51769-110">[Tillfällig][Temporary]</span><span class="sxs-lookup"><span data-stu-id="51769-110">[Temporary][Temporary]</span></span>
>
>

<span data-ttu-id="51769-111">SQL Data Warehouse är ett distribuerat MPP-databassystem.</span><span class="sxs-lookup"><span data-stu-id="51769-111">SQL Data Warehouse is a massively parallel processing (MPP) distributed database system.</span></span>  <span data-ttu-id="51769-112">Genom att dela upp data- och bearbetningsfunktioner på flera noder kan SQL Data Warehouse erbjuda fantastisk skalbarhet – långt över den i ett enskilt system.</span><span class="sxs-lookup"><span data-stu-id="51769-112">By dividing data and processing capability across multiple nodes, SQL Data Warehouse can offer huge scalability - far beyond any single system.</span></span>  <span data-ttu-id="51769-113">Bestämmer hur toodistribute dina data i ditt SQL Data Warehouse är en av de viktigaste hello faktorer tooachieving optimala prestanda.</span><span class="sxs-lookup"><span data-stu-id="51769-113">Deciding how toodistribute your data within your SQL Data Warehouse is one of hello most important factors tooachieving optimal performance.</span></span>   <span data-ttu-id="51769-114">hello viktiga toooptimal prestanda minimera dataförflyttning och i sin tur hello viktiga toominimizing dataflyttning väljer hello rätt distributionsstrategi.</span><span class="sxs-lookup"><span data-stu-id="51769-114">hello key toooptimal performance is minimizing data movement and in turn hello key toominimizing data movement is selecting hello right distribution strategy.</span></span>

## <a name="understanding-data-movement"></a><span data-ttu-id="51769-115">Förstå dataflyttning</span><span class="sxs-lookup"><span data-stu-id="51769-115">Understanding data movement</span></span>
<span data-ttu-id="51769-116">Hello data från varje tabell fördelas på flera underliggande databaser i en MPP-system.</span><span class="sxs-lookup"><span data-stu-id="51769-116">In an MPP system, hello data from each table is divided across several underlying databases.</span></span>  <span data-ttu-id="51769-117">hello mest optimerade frågor på ett MPP-system kan bara skickas via tooexecute på hello enskilda distribuerade databaser utan interaktion mellan hello andra databaser.</span><span class="sxs-lookup"><span data-stu-id="51769-117">hello most optimized queries on an MPP system can simply be passed through tooexecute on hello individual distributed databases with no interaction between hello other databases.</span></span>  <span data-ttu-id="51769-118">Anta exempelvis att du har en databas med försäljningsdata som innehåller två tabeller, försäljning och kunder.</span><span class="sxs-lookup"><span data-stu-id="51769-118">For example, let's say you have a database with sales data which contains two tables, sales and customers.</span></span>  <span data-ttu-id="51769-119">Om du har en fråga som måste toojoin din försäljning tooyour kundtabellen och du dividerar både försäljnings- och kunden tabeller upp med kundnummer, lägga till varje kund i en separat databas, kan frågor som förenar försäljning och kunder lösas inom varje databasen med ingen kunskap om hello andra databaser.</span><span class="sxs-lookup"><span data-stu-id="51769-119">If you have a query that needs toojoin your sales table tooyour customer table and you divide both your sales and customer tables up by customer number, putting each customer in a separate database, any queries which join sales and customer can be solved within each database with no knowledge of hello other databases.</span></span>  <span data-ttu-id="51769-120">Däremot om du har delat din försäljningsdata genom ordningsnummer och din kundinformation på kundnummer sedan alla angivna databasen inte hello motsvarande data för varje kund och därmed om du vill ha toojoin kunddata försäljningsdata tooyour behöver du tooget hello data för varje kund från hello andra databaser.</span><span class="sxs-lookup"><span data-stu-id="51769-120">In contrast, if you divided your sales data by order number and your customer data by customer number, then any given database will not have hello corresponding data for each customer and thus if you wanted toojoin your sales data tooyour customer data, you would need tooget hello data for each customer from hello other databases.</span></span>  <span data-ttu-id="51769-121">I den här andra exemplet behöver dataflyttning toooccur toomove hello data toohello försäljning kundinformation, så att hello två tabeller kan vara ansluten.</span><span class="sxs-lookup"><span data-stu-id="51769-121">In this second example, data movement would need toooccur toomove hello customer data toohello sales data, so that hello two tables can be joined.</span></span>  

<span data-ttu-id="51769-122">Dataflyttning inte alltid dåliga konsekvenser, ibland är det nödvändigt toosolve en fråga.</span><span class="sxs-lookup"><span data-stu-id="51769-122">Data movement isn't always a bad thing, sometimes it's necessary toosolve a query.</span></span>  <span data-ttu-id="51769-123">Men när den här extra steg kan undvikas, naturligt frågan körs snabbare.</span><span class="sxs-lookup"><span data-stu-id="51769-123">But when this extra step can be avoided, naturally your query will run faster.</span></span>  <span data-ttu-id="51769-124">Dataflyttning uppstår vanligen när tabellerna eller aggregeringar utförs.</span><span class="sxs-lookup"><span data-stu-id="51769-124">Data Movement most commonly arises when tables are joined or aggregations are performed.</span></span>  <span data-ttu-id="51769-125">Du behöver ofta toodo båda, så när du toooptimize för ett scenario som en koppling du fortfarande behöver data movement toohelp du lösa för hello andra scenarier, t.ex. en aggregering.</span><span class="sxs-lookup"><span data-stu-id="51769-125">Often you need toodo both, so while you may be able toooptimize for one scenario, like a join, you still need data movement toohelp you solve for hello other scenario, like an aggregation.</span></span>  <span data-ttu-id="51769-126">hello tips är att räkna ut som är mindre arbete.</span><span class="sxs-lookup"><span data-stu-id="51769-126">hello trick is figuring out which is less work.</span></span>  <span data-ttu-id="51769-127">I de flesta fall är distribuerar stora faktatabeller i en kolumn som är ofta kopplade hello effektivaste metoden för att minska hello de flesta flytt av data.</span><span class="sxs-lookup"><span data-stu-id="51769-127">In most cases, distributing large fact tables on a commonly joined column is hello most effective method for reducing hello most data movement.</span></span>  <span data-ttu-id="51769-128">Distribuera data på kopplingskolumner är en mycket mer vanliga tooreduce för metoden dataflyttning än distribuerar data på kolumner som ingår i en samling.</span><span class="sxs-lookup"><span data-stu-id="51769-128">Distributing data on join columns is a much more common method tooreduce data movement than distributing data on columns involved in an aggregation.</span></span>

## <a name="select-distribution-method"></a><span data-ttu-id="51769-129">Välj distributionsmetod</span><span class="sxs-lookup"><span data-stu-id="51769-129">Select distribution method</span></span>
<span data-ttu-id="51769-130">Hello bakgrunden delar dina data i 60 databaser i SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="51769-130">Behind hello scenes, SQL Data Warehouse divides your data into 60 databases.</span></span>  <span data-ttu-id="51769-131">Varje enskild databas är refererad tooas en **distribution**.</span><span class="sxs-lookup"><span data-stu-id="51769-131">Each individual database is referred tooas a **distribution**.</span></span>  <span data-ttu-id="51769-132">När data läses in i varje tabell, SQL Data Warehouse har tooknow hur toodivide data mellan dessa 60 distributioner.</span><span class="sxs-lookup"><span data-stu-id="51769-132">When data is loaded into each table, SQL Data Warehouse has tooknow how toodivide your data across these 60 distributions.</span></span>  

<span data-ttu-id="51769-133">hello distributionsmetod definieras på hello tabell nivå och det finns två alternativ:</span><span class="sxs-lookup"><span data-stu-id="51769-133">hello distribution method is defined at hello table level and currently there are two choices:</span></span>

1. <span data-ttu-id="51769-134">**Resursallokering** som distribuera data jämnt men slumpmässigt.</span><span class="sxs-lookup"><span data-stu-id="51769-134">**Round robin** which distribute data evenly but randomly.</span></span>
2. <span data-ttu-id="51769-135">**Hash-distribuerade** som distribuerar data baserat på hash-värden från en enda kolumn</span><span class="sxs-lookup"><span data-stu-id="51769-135">**Hash Distributed** which distributes data based on hashing values from a single column</span></span>

<span data-ttu-id="51769-136">Som standard när du inte definierar en data-distributionsmetod tabellen kommer att distribueras med hjälp av hello **resursallokering** distributionsmetod.</span><span class="sxs-lookup"><span data-stu-id="51769-136">By default, when you do not define a data distribution method, your table will be distributed using hello **round robin** distribution method.</span></span>  <span data-ttu-id="51769-137">När du blir mer avancerade i implementeringen av du kommer också tooconsider med **hash distribuerade** tabeller toominimize dataflyttning som i sin tur optimerar prestanda för frågor.</span><span class="sxs-lookup"><span data-stu-id="51769-137">However, as you become more sophisticated in your implementation, you will want tooconsider using **hash distributed** tables toominimize data movement which will in turn optimize query performance.</span></span>

### <a name="round-robin-tables"></a><span data-ttu-id="51769-138">Resursallokering tabeller</span><span class="sxs-lookup"><span data-stu-id="51769-138">Round Robin Tables</span></span>
<span data-ttu-id="51769-139">Använda hello resursallokering metod för att distribuera data är mycket hur det låter.</span><span class="sxs-lookup"><span data-stu-id="51769-139">Using hello Round Robin method of distributing data is very much how it sounds.</span></span>  <span data-ttu-id="51769-140">Eftersom dina data har lästs in, skickas bara toohello nästa distribution varje rad.</span><span class="sxs-lookup"><span data-stu-id="51769-140">As your data is loaded, each row is simply sent toohello next distribution.</span></span>  <span data-ttu-id="51769-141">Den här metoden för att distribuera hello data ska alltid slumpmässigt distribuera hello data mycket jämnt över alla hello-distributioner.</span><span class="sxs-lookup"><span data-stu-id="51769-141">This method of distributing hello data will always randomly distribute hello data very evenly across all of hello distributions.</span></span>  <span data-ttu-id="51769-142">Det är ingen sortering klart under hello round robin process som placerar dina data.</span><span class="sxs-lookup"><span data-stu-id="51769-142">That is, there is no sorting done during hello round robin process which places your data.</span></span>  <span data-ttu-id="51769-143">En distributionsplats för resursallokering kallas ibland ett slumpmässigt hash därför.</span><span class="sxs-lookup"><span data-stu-id="51769-143">A round robin distribution is sometimes called a random hash for this reason.</span></span>  <span data-ttu-id="51769-144">Med en distribuerad tabell resursallokering finns inga måste toounderstand hello data.</span><span class="sxs-lookup"><span data-stu-id="51769-144">With a round-robin distributed table there is no need toounderstand hello data.</span></span>  <span data-ttu-id="51769-145">Därför vara resursallokering tabeller en bra inläsning mål.</span><span class="sxs-lookup"><span data-stu-id="51769-145">For this reason, Round-Robin tables often make good loading targets.</span></span>

<span data-ttu-id="51769-146">Som standard om du väljer Ingen distributionsmetod används hello resursallokering distributionsmetod.</span><span class="sxs-lookup"><span data-stu-id="51769-146">By default, if no distribution method is chosen, hello round robin distribution method will be used.</span></span>  <span data-ttu-id="51769-147">Medan resursallokering tabeller är enkla toouse eftersom data är slumpmässigt fördelad över hello system innebär det att hello system inte kan garantera vilken distribution är varje rad på.</span><span class="sxs-lookup"><span data-stu-id="51769-147">However, while round robin tables are easy toouse, because data is randomly distributed across hello system it means that hello system can't guarantee which distribution each row is on.</span></span>  <span data-ttu-id="51769-148">Som ett resultat, ibland hello-system måste tooinvoke ordna dina data i en data movement åtgärden toobetter innan den kan lösa en fråga.</span><span class="sxs-lookup"><span data-stu-id="51769-148">As a result, hello system sometimes needs tooinvoke a data movement operation toobetter organize your data before it can resolve a query.</span></span>  <span data-ttu-id="51769-149">Den här extra steg kan sakta ned dina frågor.</span><span class="sxs-lookup"><span data-stu-id="51769-149">This extra step can slow down your queries.</span></span>

<span data-ttu-id="51769-150">Överväg att använda resursallokering (Round robin) för en tabell i hello följande scenarier:</span><span class="sxs-lookup"><span data-stu-id="51769-150">Consider using Round Robin distribution for your table in hello following scenarios:</span></span>

* <span data-ttu-id="51769-151">När igång som en enkel startpunkt</span><span class="sxs-lookup"><span data-stu-id="51769-151">When getting started as a simple starting point</span></span>
* <span data-ttu-id="51769-152">Om det finns ingen uppenbara anslutande nyckel</span><span class="sxs-lookup"><span data-stu-id="51769-152">If there is no obvious joining key</span></span>
* <span data-ttu-id="51769-153">Om det inte är bra kandidat kolumn för hash-distribuerar hello tabell</span><span class="sxs-lookup"><span data-stu-id="51769-153">If there is not good candidate column for hash distributing hello table</span></span>
* <span data-ttu-id="51769-154">Om hello dela tabellen inte en gemensam join-nyckel med andra tabeller</span><span class="sxs-lookup"><span data-stu-id="51769-154">If hello table does not share a common join key with other tables</span></span>
* <span data-ttu-id="51769-155">Om hello koppling är mindre viktig än andra kopplingar i hello frågan</span><span class="sxs-lookup"><span data-stu-id="51769-155">If hello join is less significant than other joins in hello query</span></span>
* <span data-ttu-id="51769-156">När hello tabell är en tillfällig mellanlagring tabell</span><span class="sxs-lookup"><span data-stu-id="51769-156">When hello table is a temporary staging table</span></span>

<span data-ttu-id="51769-157">Båda dessa exempel skapar en resursallokering tabell:</span><span class="sxs-lookup"><span data-stu-id="51769-157">Both of these examples will create a Round Robin Table:</span></span>

```SQL
-- Round Robin created by default
CREATE TABLE [dbo].[FactInternetSales]
(   [ProductKey]            int          NOT NULL
,   [OrderDateKey]          int          NOT NULL
,   [CustomerKey]           int          NOT NULL
,   [PromotionKey]          int          NOT NULL
,   [SalesOrderNumber]      nvarchar(20) NOT NULL
,   [OrderQuantity]         smallint     NOT NULL
,   [UnitPrice]             money        NOT NULL
,   [SalesAmount]           money        NOT NULL
)
;

-- Explicitly Created Round Robin Table
CREATE TABLE [dbo].[FactInternetSales]
(   [ProductKey]            int          NOT NULL
,   [OrderDateKey]          int          NOT NULL
,   [CustomerKey]           int          NOT NULL
,   [PromotionKey]          int          NOT NULL
,   [SalesOrderNumber]      nvarchar(20) NOT NULL
,   [OrderQuantity]         smallint     NOT NULL
,   [UnitPrice]             money        NOT NULL
,   [SalesAmount]           money        NOT NULL
)
WITH
(   CLUSTERED COLUMNSTORE INDEX
,   DISTRIBUTION = ROUND_ROBIN
)
;
```

> [!NOTE]
> <span data-ttu-id="51769-158">Medan resursallokering hello tabell standardtypen är explicit i din DDL betraktas som bästa praxis så att din tabellayout hello avsikt Rensa tooothers.</span><span class="sxs-lookup"><span data-stu-id="51769-158">While round robin is hello default table type being explicit in your DDL is considered a best practice so that hello intentions of your table layout are clear tooothers.</span></span>
>
>

### <a name="hash-distributed-tables"></a><span data-ttu-id="51769-159">Hash-distribuerade tabeller</span><span class="sxs-lookup"><span data-stu-id="51769-159">Hash Distributed Tables</span></span>
<span data-ttu-id="51769-160">Med hjälp av en **Hash distribuerade** algoritmen toodistribute tabeller kan förbättra prestanda för många scenarier genom att minska dataflyttning frågan för närvarande.</span><span class="sxs-lookup"><span data-stu-id="51769-160">Using a **Hash distributed** algorithm toodistribute your tables can improve performance for many scenarios by reducing data movement at query time.</span></span>  <span data-ttu-id="51769-161">Hash distribuerade tabeller är tabeller som delas mellan hello distribuerade databaser med hjälp av en hash-algoritm på en enda kolumn som du väljer.</span><span class="sxs-lookup"><span data-stu-id="51769-161">Hash distributed tables are tables which are divided between hello distributed databases using a hashing algorithm on a single column which you select.</span></span>  <span data-ttu-id="51769-162">hello distribution kolumnen är vad avgör hur hello data fördelas på dina distribuerade databaser.</span><span class="sxs-lookup"><span data-stu-id="51769-162">hello distribution column is what determines how hello data is divided across your distributed databases.</span></span>  <span data-ttu-id="51769-163">hello hash-funktionen använder hello distribution kolumnen tooassign rader toodistributions.</span><span class="sxs-lookup"><span data-stu-id="51769-163">hello hash function uses hello distribution column tooassign rows toodistributions.</span></span>  <span data-ttu-id="51769-164">Hej hash-algoritm och resulterande distribution är deterministisk.</span><span class="sxs-lookup"><span data-stu-id="51769-164">hello hashing algorithm and resulting distribution is deterministic.</span></span>  <span data-ttu-id="51769-165">Hello som är identiskt med hello samma datatyp kommer alltid har toohello samma distribution.</span><span class="sxs-lookup"><span data-stu-id="51769-165">That is hello same value with hello same data type will always has toohello same distribution.</span></span>    

<span data-ttu-id="51769-166">Det här exemplet skapar en tabell som distribueras på-id:</span><span class="sxs-lookup"><span data-stu-id="51769-166">This example will create a table distributed on id:</span></span>

```SQL
CREATE TABLE [dbo].[FactInternetSales]
(   [ProductKey]            int          NOT NULL
,   [OrderDateKey]          int          NOT NULL
,   [CustomerKey]           int          NOT NULL
,   [PromotionKey]          int          NOT NULL
,   [SalesOrderNumber]      nvarchar(20) NOT NULL
,   [OrderQuantity]         smallint     NOT NULL
,   [UnitPrice]             money        NOT NULL
,   [SalesAmount]           money        NOT NULL
)
WITH
(   CLUSTERED COLUMNSTORE INDEX
,  DISTRIBUTION = HASH([ProductKey])
)
;
```

## <a name="select-distribution-column"></a><span data-ttu-id="51769-167">Välj distributionsplatser kolumn</span><span class="sxs-lookup"><span data-stu-id="51769-167">Select distribution column</span></span>
<span data-ttu-id="51769-168">När du väljer för**hash distribuera** en tabell, behöver du tooselect en enkel distribution-kolumn.</span><span class="sxs-lookup"><span data-stu-id="51769-168">When you choose too**hash distribute** a table, you will need tooselect a single distribution column.</span></span>  <span data-ttu-id="51769-169">När du väljer en kolumn för distribution, finns det tre viktiga faktorer tooconsider.</span><span class="sxs-lookup"><span data-stu-id="51769-169">When selecting a distribution column, there are three major factors tooconsider.</span></span>  

<span data-ttu-id="51769-170">Markera en kolumn som används för att:</span><span class="sxs-lookup"><span data-stu-id="51769-170">Select a single column which will:</span></span>

1. <span data-ttu-id="51769-171">Inte uppdateras</span><span class="sxs-lookup"><span data-stu-id="51769-171">Not be updated</span></span>
2. <span data-ttu-id="51769-172">Distribuera data jämnt, undvika data skeva</span><span class="sxs-lookup"><span data-stu-id="51769-172">Distribute data evenly, avoiding data skew</span></span>
3. <span data-ttu-id="51769-173">Minimera dataflyttning</span><span class="sxs-lookup"><span data-stu-id="51769-173">Minimize data movement</span></span>

### <a name="select-distribution-column-which-will-not-be-updated"></a><span data-ttu-id="51769-174">Välj distributionsplatser kolumn som inte kommer att uppdateras</span><span class="sxs-lookup"><span data-stu-id="51769-174">Select distribution column which will not be updated</span></span>
<span data-ttu-id="51769-175">Distributionskolumner kan inte uppdateras, därför, markera en kolumn med statiska värden.</span><span class="sxs-lookup"><span data-stu-id="51769-175">Distribution columns are not updatable, therefore, select a column with static values.</span></span>  <span data-ttu-id="51769-176">Om en kolumn måste toobe uppdateras, är vanligtvis inte kandidat bra distribution.</span><span class="sxs-lookup"><span data-stu-id="51769-176">If a column will need toobe updated, it is generally not a good distribution candidate.</span></span>  <span data-ttu-id="51769-177">Det finns ett fall där du måste uppdatera en kolumn för distribution, kan du göra det genom att först ta bort hello rad och sedan lägga till en ny rad.</span><span class="sxs-lookup"><span data-stu-id="51769-177">If there is a case where you must update a distribution column, this can be done by first deleting hello row and then inserting a new row.</span></span>

### <a name="select-distribution-column-which-will-distribute-data-evenly"></a><span data-ttu-id="51769-178">Välj distributionsplatser kolumn som ska distribuera data jämnt</span><span class="sxs-lookup"><span data-stu-id="51769-178">Select distribution column which will distribute data evenly</span></span>
<span data-ttu-id="51769-179">Eftersom ett distribuerat system utför bara så snabbt som motsvarande långsammaste distribution, är det viktigt toodivide hello arbete jämnt över hello distributioner ordning tooachieve belastningsutjämnade körning över hello system.</span><span class="sxs-lookup"><span data-stu-id="51769-179">Since a distributed system performs only as fast as its slowest distribution, it is important toodivide hello work evenly across hello distributions in order tooachieve balanced execution across hello system.</span></span>  <span data-ttu-id="51769-180">hello är hello arbete är uppdelad i ett distribuerat system utifrån där hello data för varje distribution finns.</span><span class="sxs-lookup"><span data-stu-id="51769-180">hello way hello work is divided on a distributed system is based on where hello data for each distribution lives.</span></span>  <span data-ttu-id="51769-181">Detta gör det mycket viktigt tooselect hello rätt distribution kolumnen för att distribuera hello data så att varje distribution har samma arbete och ska vidta hello samma tid toocomplete sin del av hello arbete.</span><span class="sxs-lookup"><span data-stu-id="51769-181">This makes it very important tooselect hello right distribution column for distributing hello data so that each distribution has equal work and will take hello same time toocomplete its portion of hello work.</span></span>  <span data-ttu-id="51769-182">När arbetet fördelas väl på hello system, fördelas hello data över hello-distributioner.</span><span class="sxs-lookup"><span data-stu-id="51769-182">When work is well divided across hello system, hello data is balanced across hello distributions.</span></span>  <span data-ttu-id="51769-183">När data inte är balanserade jämnt, vi kallar detta **data förskjutning**.</span><span class="sxs-lookup"><span data-stu-id="51769-183">When data is not evenly balanced, we call this **data skew**.</span></span>  

<span data-ttu-id="51769-184">toodivide data jämnt och undvika skeva data bör du hello följande när du väljer kolumnen distribution:</span><span class="sxs-lookup"><span data-stu-id="51769-184">toodivide data evenly and avoid data skew, consider hello following when selecting your distribution column:</span></span>

1. <span data-ttu-id="51769-185">Markera en kolumn som innehåller ett stort antal distinkta värden.</span><span class="sxs-lookup"><span data-stu-id="51769-185">Select a column which contains a significant number of distinct values.</span></span>
2. <span data-ttu-id="51769-186">Undvik att dela ut data på kolumner med några få distinkta värden.</span><span class="sxs-lookup"><span data-stu-id="51769-186">Avoid distributing data on columns with a few distinct values.</span></span>
3. <span data-ttu-id="51769-187">Undvik att dela ut data på kolumner med en hög frekvens av null-värden.</span><span class="sxs-lookup"><span data-stu-id="51769-187">Avoid distributing data on columns with a high frequency of nulls.</span></span>
4. <span data-ttu-id="51769-188">Undvik att dela ut data på datumkolumnerna.</span><span class="sxs-lookup"><span data-stu-id="51769-188">Avoid distributing data on date columns.</span></span>

<span data-ttu-id="51769-189">Eftersom varje värde är hashformaterats too1 av 60 distributioner, tooachieve jämn fördelning vill tooselect en kolumn som är mycket unik och innehåller mer än 60 unika värden.</span><span class="sxs-lookup"><span data-stu-id="51769-189">Since each value is hashed too1 of 60 distributions, tooachieve even distribution you will want tooselect a column that is highly unique and contains more than 60 unique values.</span></span>  <span data-ttu-id="51769-190">tooillustrate, Överväg att fall där en kolumn enbart har 40 unika värden.</span><span class="sxs-lookup"><span data-stu-id="51769-190">tooillustrate, consider a case where a column only has 40 unique values.</span></span>  <span data-ttu-id="51769-191">Om den här kolumnen har valts som hello distribution tangent skulle hello data för tabellen hamna på 40 distributioner mest lämnar 20 distributioner med inga data och ingen bearbetning toodo.</span><span class="sxs-lookup"><span data-stu-id="51769-191">If this column was selected as hello distribution key, hello data for that table would land on 40 distributions at most, leaving 20 distributions with no data and no processing toodo.</span></span>  <span data-ttu-id="51769-192">Däremot skulle hello andra 40 distributioner få mer arbete toodo att om hello data var jämnt fördelade över 60 distributioner.</span><span class="sxs-lookup"><span data-stu-id="51769-192">Conversely, hello other 40 distributions would have more work toodo that if hello data was evenly spread over 60 distributions.</span></span>  <span data-ttu-id="51769-193">Det här scenariot är ett exempel på data skeva.</span><span class="sxs-lookup"><span data-stu-id="51769-193">This scenario is an example of data skew.</span></span>

<span data-ttu-id="51769-194">I MPP-system väntar varje frågesteg på att alla distributioner toocomplete sin andel av hello arbete.</span><span class="sxs-lookup"><span data-stu-id="51769-194">In MPP system, each query step waits for all distributions toocomplete their share of hello work.</span></span>  <span data-ttu-id="51769-195">Om en distribution är att mer arbete än hello andra: hello resurs av hello andra distributioner är i stort sett gått förlorat bara väntar på hello upptagen distribution.</span><span class="sxs-lookup"><span data-stu-id="51769-195">If one distribution is doing more work than hello others, then hello resource of hello other distributions are essentially wasted just waiting on hello busy distribution.</span></span>  <span data-ttu-id="51769-196">När arbetet inte är jämnt fördelade över alla distributioner, vi kallar detta **bearbetning skeva**.</span><span class="sxs-lookup"><span data-stu-id="51769-196">When work is not evenly spread across all distributions, we call this **processing skew**.</span></span>  <span data-ttu-id="51769-197">Förskjutning av bearbetning kommer frågor toorun långsammare än om hello arbetsbelastningen kan vara jämnt fördelade över hello-distributioner.</span><span class="sxs-lookup"><span data-stu-id="51769-197">Processing skew will cause queries toorun slower than if hello workload can be evenly spread across hello distributions.</span></span>  <span data-ttu-id="51769-198">Förskjutning av data kommer att leda tooprocessing skeva.</span><span class="sxs-lookup"><span data-stu-id="51769-198">Data skew will lead tooprocessing skew.</span></span>

<span data-ttu-id="51769-199">Undvik att dela ut på hög nullbar kolumn som hello nullvärden alla hamnar på hello samma distribution.</span><span class="sxs-lookup"><span data-stu-id="51769-199">Avoid distributing on highly nullable column as hello null values will all land on hello same distribution.</span></span> <span data-ttu-id="51769-200">Distribuera på ett datum kan också orsakas av skeva bearbetning eftersom alla data för ett visst datum hamnar på hello samma distribution.</span><span class="sxs-lookup"><span data-stu-id="51769-200">Distributing on a date column can also cause processing skew because all data for a given date will land on hello same distribution.</span></span> <span data-ttu-id="51769-201">Om flera användare kör frågor alla filtrering på hello samma datum, och sedan endast 1 av hello 60 distributioner kommer att göra alla hello arbete eftersom ett speciellt datum ska på en distributionsplats.</span><span class="sxs-lookup"><span data-stu-id="51769-201">If several users are executing queries all filtering on hello same date, then only 1 of hello 60 distributions will be doing all of hello work since a given date will only be on one distribution.</span></span> <span data-ttu-id="51769-202">I det här scenariot körs sannolikt hello frågor 60 gånger långsammare än om hello data har sprida jämnt över alla hello-distributioner.</span><span class="sxs-lookup"><span data-stu-id="51769-202">In this scenario, hello queries will likely run 60 times slower than if hello data were equally spread over all of hello distributions.</span></span>

<span data-ttu-id="51769-203">När det finns inga bra kandidat kolumner, Överväg att använda resursallokering som hello distributionsmetod.</span><span class="sxs-lookup"><span data-stu-id="51769-203">When no good candidate columns exist, then consider using round robin as hello distribution method.</span></span>

### <a name="select-distribution-column-which-will-minimize-data-movement"></a><span data-ttu-id="51769-204">Välj distributionsplatser kolumn som minimerar dataflyttning</span><span class="sxs-lookup"><span data-stu-id="51769-204">Select distribution column which will minimize data movement</span></span>
<span data-ttu-id="51769-205">Minimera dataflyttning genom att markera hello rätt distribution kolumn är en hello viktigaste strategier för att optimera prestanda för ditt SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="51769-205">Minimizing data movement by selecting hello right distribution column is one of hello most important strategies for optimizing performance of your SQL Data Warehouse.</span></span>  <span data-ttu-id="51769-206">Dataflyttning uppstår vanligen när tabellerna eller aggregeringar utförs.</span><span class="sxs-lookup"><span data-stu-id="51769-206">Data Movement most commonly arises when tables are joined or aggregations are performed.</span></span>  <span data-ttu-id="51769-207">Kolumner som används i `JOIN`, `GROUP BY`, `DISTINCT`, `OVER` och `HAVING` satser alla gör för **bra** hash-kandidater för distribution.</span><span class="sxs-lookup"><span data-stu-id="51769-207">Columns used in `JOIN`, `GROUP BY`, `DISTINCT`, `OVER` and `HAVING` clauses all make for **good** hash distribution candidates.</span></span>

<span data-ttu-id="51769-208">Hello å andra sidan kolumner i hello `WHERE` satsen gör **inte** gör för bra hash-kolumnen kandidater eftersom de begränsa vilka distributioner delta i hello-frågan som orsakar bearbetning skeva.</span><span class="sxs-lookup"><span data-stu-id="51769-208">On hello other hand, columns in hello `WHERE` clause do **not** make for good hash column candidates because they limit which distributions participate in hello query, causing processing skew.</span></span>  <span data-ttu-id="51769-209">Ett bra exempel på en kolumn som kan vara nära till hands toodistribute på, men ofta kan orsaka denna skeva bearbetning är en datumkolumn.</span><span class="sxs-lookup"><span data-stu-id="51769-209">A good example of a column which might be tempting toodistribute on, but often can cause this processing skew is a date column.</span></span>

<span data-ttu-id="51769-210">Generellt sett om du har två stora-faktatabeller är ofta ingår i en koppling du kommer att få hello de flesta prestanda genom att distribuera båda tabellerna på en av hello kopplingskolumner.</span><span class="sxs-lookup"><span data-stu-id="51769-210">Generally speaking, if you have two large fact tables frequently involved in a join, you will gain hello most performance by distributing both tables on one of hello join columns.</span></span>  <span data-ttu-id="51769-211">Om du har en tabell som inte är domänanslutna tooanother stora faktatabell leta toocolumns som är vanliga i hello `GROUP BY` satsen.</span><span class="sxs-lookup"><span data-stu-id="51769-211">If you have a table that is never joined tooanother large fact table, then look toocolumns that are frequently in hello `GROUP BY` clause.</span></span>

<span data-ttu-id="51769-212">Det finns några viktiga villkor som måste vara uppfyllda tooavoid dataflyttning under en koppling:</span><span class="sxs-lookup"><span data-stu-id="51769-212">There are a few key criteria which must be met tooavoid data movement during a join:</span></span>

1. <span data-ttu-id="51769-213">hello tabeller som ingår i hello koppling måste vara hash distribueras vid **en** av hello kolumner som ingår i hello koppling.</span><span class="sxs-lookup"><span data-stu-id="51769-213">hello tables involved in hello join must be hash distributed on **one** of hello columns participating in hello join.</span></span>
2. <span data-ttu-id="51769-214">hello datatyperna hello kopplingskolumner måste matcha mellan båda tabellerna.</span><span class="sxs-lookup"><span data-stu-id="51769-214">hello data types of hello join columns must match between both tables.</span></span>
3. <span data-ttu-id="51769-215">hello-kolumner måste vara ansluten med en equals-operatorn.</span><span class="sxs-lookup"><span data-stu-id="51769-215">hello columns must be joined with an equals operator.</span></span>
4. <span data-ttu-id="51769-216">hello kopplingstyp får inte vara en `CROSS JOIN`.</span><span class="sxs-lookup"><span data-stu-id="51769-216">hello join type may not be a `CROSS JOIN`.</span></span>

## <a name="troubleshooting-data-skew"></a><span data-ttu-id="51769-217">Felsökningsdata skeva</span><span class="sxs-lookup"><span data-stu-id="51769-217">Troubleshooting data skew</span></span>
<span data-ttu-id="51769-218">När tabelldata distribueras med hjälp av hello hash distributionssätt finns förvrängd en risk att vissa distributioner kommer att toohave oproportionerligt mer data än andra.</span><span class="sxs-lookup"><span data-stu-id="51769-218">When table data is distributed using hello hash distribution method there is a chance that some distributions will be skewed toohave disproportionately more data than others.</span></span> <span data-ttu-id="51769-219">Onödigt stora datamängder skeva kan påverka prestanda för frågor eftersom hello slutresultatet av distribuerade frågor måste vänta på hello längsta körs distribution toofinish.</span><span class="sxs-lookup"><span data-stu-id="51769-219">Excessive data skew can impact query performance because hello final result of a distributed query must wait for hello longest running distribution toofinish.</span></span> <span data-ttu-id="51769-220">Beroende på hello graden av hello data skeva måste du kanske tooaddress den.</span><span class="sxs-lookup"><span data-stu-id="51769-220">Depending on hello degree of hello data skew you might need tooaddress it.</span></span>

### <a name="identifying-skew"></a><span data-ttu-id="51769-221">Identifiera förskjutning</span><span class="sxs-lookup"><span data-stu-id="51769-221">Identifying skew</span></span>
<span data-ttu-id="51769-222">Ett enkelt sätt tooidentify en tabell som förvrängd är toouse `DBCC PDW_SHOWSPACEUSED`.</span><span class="sxs-lookup"><span data-stu-id="51769-222">A simple way tooidentify a table as skewed is toouse `DBCC PDW_SHOWSPACEUSED`.</span></span>  <span data-ttu-id="51769-223">Det här är en mycket snabbt och enkelt sätt toosee hello antalet rader som lagras i varje hello 60 distributioner av databasen.</span><span class="sxs-lookup"><span data-stu-id="51769-223">This is a very quick and simple way toosee hello number of table rows that are stored in each of hello 60 distributions of your database.</span></span>  <span data-ttu-id="51769-224">Kom ihåg att hello mest belastningsutjämnade prestanda hello rader i tabellen distribuerade ska spridas jämnt över alla hello-distributioner.</span><span class="sxs-lookup"><span data-stu-id="51769-224">Remember that for hello most balanced performance, hello rows in your distributed table should be spread evenly across all hello distributions.</span></span>

```sql
-- Find data skew for a distributed table
DBCC PDW_SHOWSPACEUSED('dbo.FactInternetSales');
```

<span data-ttu-id="51769-225">Om du frågar hello Azure SQL Data Warehouse dynamiska hanteringsvyer (DMV) kan du utföra en mer detaljerad analys.</span><span class="sxs-lookup"><span data-stu-id="51769-225">However, if you query hello Azure SQL Data Warehouse dynamic management views (DMV) you can perform a more detailed analysis.</span></span>  <span data-ttu-id="51769-226">toostart, skapa hello vy [dbo.vTableSizes] [ dbo.vTableSizes] visas med hjälp av hello SQL från [tabell översikt] [ Overview] artikel.</span><span class="sxs-lookup"><span data-stu-id="51769-226">toostart, create hello view [dbo.vTableSizes][dbo.vTableSizes] view using hello SQL from [Table Overview][Overview] article.</span></span>  <span data-ttu-id="51769-227">Kör den här frågan tooidentify vilka tabeller har mer än 10% data förskjutning när hello vyn har skapats.</span><span class="sxs-lookup"><span data-stu-id="51769-227">Once hello view is created, run this query tooidentify which tables have more than 10% data skew.</span></span>

```sql
select *
from dbo.vTableSizes
where two_part_name in
    (
    select two_part_name
    from dbo.vTableSizes
    where row_count > 0
    group by two_part_name
    having min(row_count * 1.000)/max(row_count * 1.000) > .10
    )
order by two_part_name, row_count
;
```

### <a name="resolving-data-skew"></a><span data-ttu-id="51769-228">Matcha data skeva</span><span class="sxs-lookup"><span data-stu-id="51769-228">Resolving data skew</span></span>
<span data-ttu-id="51769-229">Inte alla skeva är tillräckligt med toowarrant en korrigering.</span><span class="sxs-lookup"><span data-stu-id="51769-229">Not all skew is enough toowarrant a fix.</span></span>  <span data-ttu-id="51769-230">I vissa fall uppväger hello prestanda för en tabell i några frågor hello skada data skeva.</span><span class="sxs-lookup"><span data-stu-id="51769-230">In some cases, hello performance of a table in some queries can outweigh hello harm of data skew.</span></span>  <span data-ttu-id="51769-231">toodecide om du ska lösa data skeva i en tabell, bör du känna till så mycket som möjligt om hello datavolymer och frågor i din arbetsbelastning.</span><span class="sxs-lookup"><span data-stu-id="51769-231">toodecide if you should resolve data skew in a table, you should understand as much as possible about hello data volumes and queries in your workload.</span></span>   <span data-ttu-id="51769-232">Enkelriktade toolook på hello effekten av förskjutning är toouse hello stegen i hello [frågan övervakning] [ Query Monitoring] artikel toomonitor hello effekten av skeva på frågeprestanda och specifikt hello påverkan toohow långa frågor ta toocomplete på enskilda hello-distributioner.</span><span class="sxs-lookup"><span data-stu-id="51769-232">One way toolook at hello impact of skew is toouse hello steps in hello [Query Monitoring][Query Monitoring] article toomonitor hello impact of skew on query performance and specifically hello impact toohow long queries take toocomplete on hello individual distributions.</span></span>

<span data-ttu-id="51769-233">Distribuera data är en fråga om att hitta hello rätt balans mellan minimera förskjutning av data och minimera dataflyttning.</span><span class="sxs-lookup"><span data-stu-id="51769-233">Distributing data is a matter of finding hello right balance between minimizing data skew and minimizing data movement.</span></span> <span data-ttu-id="51769-234">Dessa kan motverkar mål och ibland vill du tookeep data skeva i ordning tooreduce dataflyttning.</span><span class="sxs-lookup"><span data-stu-id="51769-234">These can be opposing goals, and sometimes you will want tookeep data skew in order tooreduce data movement.</span></span> <span data-ttu-id="51769-235">Till exempel när hello distribution kolumnen är ofta hello delade kolumn i kopplingar och aggregeringar, kommer du att minimera dataflyttning.</span><span class="sxs-lookup"><span data-stu-id="51769-235">For example, when hello distribution column is frequently hello shared column in joins and aggregations, you will be minimizing data movement.</span></span> <span data-ttu-id="51769-236">hello kan fördelen med att ha hello minimal dataflyttning uppväger hello effekten av att data skeva.</span><span class="sxs-lookup"><span data-stu-id="51769-236">hello benefit of having hello minimal data movement might outweigh hello impact of having data skew.</span></span>

<span data-ttu-id="51769-237">hello vanligt tooresolve data förskjutning är toore-skapa hello tabell med en annan distributionsplats-kolumn.</span><span class="sxs-lookup"><span data-stu-id="51769-237">hello typical way tooresolve data skew is toore-create hello table with a different distribution column.</span></span> <span data-ttu-id="51769-238">Eftersom det inte finns något sätt toochange hello distribution kolumn i en befintlig tabell, hello sätt toochange hello distribution av en tabell den toorecreate den med en [CTAS] [].</span><span class="sxs-lookup"><span data-stu-id="51769-238">Since there is no way toochange hello distribution column on an existing table, hello way toochange hello distribution of a table it toorecreate it with a [CTAS][].</span></span>  <span data-ttu-id="51769-239">Här är två exempel på hur lösa skeva data:</span><span class="sxs-lookup"><span data-stu-id="51769-239">Here are two examples of how resolve data skew:</span></span>

### <a name="example-1-re-create-hello-table-with-a-new-distribution-column"></a><span data-ttu-id="51769-240">Exempel 1: Skapa nytt hello tabell med en ny kolumn för distribution</span><span class="sxs-lookup"><span data-stu-id="51769-240">Example 1: Re-create hello table with a new distribution column</span></span>
<span data-ttu-id="51769-241">Det här exemplet används [CTAS] [] toore-skapa en tabell med en annan hash-distribution kolumn.</span><span class="sxs-lookup"><span data-stu-id="51769-241">This example uses [CTAS][] toore-create a table with a different hash distribution column.</span></span>

```sql
CREATE TABLE [dbo].[FactInternetSales_CustomerKey]
WITH (  CLUSTERED COLUMNSTORE INDEX
     ,  DISTRIBUTION =  HASH([CustomerKey])
     ,  PARTITION       ( [OrderDateKey] RANGE RIGHT FOR VALUES (   20000101, 20010101, 20020101, 20030101
                                                                ,   20040101, 20050101, 20060101, 20070101
                                                                ,   20080101, 20090101, 20100101, 20110101
                                                                ,   20120101, 20130101, 20140101, 20150101
                                                                ,   20160101, 20170101, 20180101, 20190101
                                                                ,   20200101, 20210101, 20220101, 20230101
                                                                ,   20240101, 20250101, 20260101, 20270101
                                                                ,   20280101, 20290101
                                                                )
                        )
    )
AS
SELECT  *
FROM    [dbo].[FactInternetSales]
OPTION  (LABEL  = 'CTAS : FactInternetSales_CustomerKey')
;

--Create statistics on new table
CREATE STATISTICS [ProductKey] ON [FactInternetSales_CustomerKey] ([ProductKey]);
CREATE STATISTICS [OrderDateKey] ON [FactInternetSales_CustomerKey] ([OrderDateKey]);
CREATE STATISTICS [CustomerKey] ON [FactInternetSales_CustomerKey] ([CustomerKey]);
CREATE STATISTICS [PromotionKey] ON [FactInternetSales_CustomerKey] ([PromotionKey]);
CREATE STATISTICS [SalesOrderNumber] ON [FactInternetSales_CustomerKey] ([SalesOrderNumber]);
CREATE STATISTICS [OrderQuantity] ON [FactInternetSales_CustomerKey] ([OrderQuantity]);
CREATE STATISTICS [UnitPrice] ON [FactInternetSales_CustomerKey] ([UnitPrice]);
CREATE STATISTICS [SalesAmount] ON [FactInternetSales_CustomerKey] ([SalesAmount]);

--Rename hello tables
RENAME OBJECT [dbo].[FactInternetSales] too[FactInternetSales_ProductKey];
RENAME OBJECT [dbo].[FactInternetSales_CustomerKey] too[FactInternetSales];
```

### <a name="example-2-re-create-hello-table-using-round-robin-distribution"></a><span data-ttu-id="51769-242">Exempel 2: Skapa nytt hello tabell med resursallokering (round robin)</span><span class="sxs-lookup"><span data-stu-id="51769-242">Example 2: Re-create hello table using round robin distribution</span></span>
<span data-ttu-id="51769-243">Det här exemplet används [CTAS] [] toore-skapa en tabell med resursallokering i stället för en hash-distribution.</span><span class="sxs-lookup"><span data-stu-id="51769-243">This example uses [CTAS][] toore-create a table with round robin instead of a hash distribution.</span></span> <span data-ttu-id="51769-244">Den här ändringen ger även Datadistribution hello kostnad för ökad dataflyttning.</span><span class="sxs-lookup"><span data-stu-id="51769-244">This change will produce even data distribution at hello cost of increased data movement.</span></span>

```sql
CREATE TABLE [dbo].[FactInternetSales_ROUND_ROBIN]
WITH (  CLUSTERED COLUMNSTORE INDEX
     ,  DISTRIBUTION =  ROUND_ROBIN
     ,  PARTITION       ( [OrderDateKey] RANGE RIGHT FOR VALUES (   20000101, 20010101, 20020101, 20030101
                                                                ,   20040101, 20050101, 20060101, 20070101
                                                                ,   20080101, 20090101, 20100101, 20110101
                                                                ,   20120101, 20130101, 20140101, 20150101
                                                                ,   20160101, 20170101, 20180101, 20190101
                                                                ,   20200101, 20210101, 20220101, 20230101
                                                                ,   20240101, 20250101, 20260101, 20270101
                                                                ,   20280101, 20290101
                                                                )
                        )
    )
AS
SELECT  *
FROM    [dbo].[FactInternetSales]
OPTION  (LABEL  = 'CTAS : FactInternetSales_ROUND_ROBIN')
;

--Create statistics on new table
CREATE STATISTICS [ProductKey] ON [FactInternetSales_ROUND_ROBIN] ([ProductKey]);
CREATE STATISTICS [OrderDateKey] ON [FactInternetSales_ROUND_ROBIN] ([OrderDateKey]);
CREATE STATISTICS [CustomerKey] ON [FactInternetSales_ROUND_ROBIN] ([CustomerKey]);
CREATE STATISTICS [PromotionKey] ON [FactInternetSales_ROUND_ROBIN] ([PromotionKey]);
CREATE STATISTICS [SalesOrderNumber] ON [FactInternetSales_ROUND_ROBIN] ([SalesOrderNumber]);
CREATE STATISTICS [OrderQuantity] ON [FactInternetSales_ROUND_ROBIN] ([OrderQuantity]);
CREATE STATISTICS [UnitPrice] ON [FactInternetSales_ROUND_ROBIN] ([UnitPrice]);
CREATE STATISTICS [SalesAmount] ON [FactInternetSales_ROUND_ROBIN] ([SalesAmount]);

--Rename hello tables
RENAME OBJECT [dbo].[FactInternetSales] too[FactInternetSales_HASH];
RENAME OBJECT [dbo].[FactInternetSales_ROUND_ROBIN] too[FactInternetSales];
```

## <a name="next-steps"></a><span data-ttu-id="51769-245">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="51769-245">Next steps</span></span>
<span data-ttu-id="51769-246">toolearn mer om tabelldesign, se hello [fördela][Distribute], [Index][Index], [Partition] [ Partition], [Datatyper][Data Types], [statistik] [ Statistics] och [temporära tabeller] [ Temporary] artiklar.</span><span class="sxs-lookup"><span data-stu-id="51769-246">toolearn more about table design, see hello [Distribute][Distribute], [Index][Index], [Partition][Partition], [Data Types][Data Types], [Statistics][Statistics] and [Temporary Tables][Temporary] articles.</span></span>

<span data-ttu-id="51769-247">En översikt över bästa praxis, se [Metodtips för SQL Data Warehouse][SQL Data Warehouse Best Practices].</span><span class="sxs-lookup"><span data-stu-id="51769-247">For an overview of best practices, see [SQL Data Warehouse Best Practices][SQL Data Warehouse Best Practices].</span></span>

<!--Image references-->

<!--Article references-->
[Overview]: ./sql-data-warehouse-tables-overview.md
[Data Types]: ./sql-data-warehouse-tables-data-types.md
[Distribute]: ./sql-data-warehouse-tables-distribute.md
[Index]: ./sql-data-warehouse-tables-index.md
[Partition]: ./sql-data-warehouse-tables-partition.md
[Statistics]: ./sql-data-warehouse-tables-statistics.md
[Temporary]: ./sql-data-warehouse-tables-temporary.md
[SQL Data Warehouse Best Practices]: ./sql-data-warehouse-best-practices.md
[Query Monitoring]: ./sql-data-warehouse-manage-monitor.md
[dbo.vTableSizes]: ./sql-data-warehouse-tables-overview.md#table-size-queries

<!--MSDN references-->
[DBCC PDW_SHOWSPACEUSED()]: https://msdn.microsoft.com/library/mt204028.aspx

<!--Other Web references-->
