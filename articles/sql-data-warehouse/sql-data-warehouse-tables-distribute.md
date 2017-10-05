---
title: Distribuera tabeller i SQL Data Warehouse | Microsoft Docs
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
ms.openlocfilehash: d0e12bf821a81826a20b8db84e76c48fa60ad9b5
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="distributing-tables-in-sql-data-warehouse"></a><span data-ttu-id="7adfb-103">Distribuera tabeller i SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="7adfb-103">Distributing tables in SQL Data Warehouse</span></span>
> [!div class="op_single_selector"]
> * <span data-ttu-id="7adfb-104">[Översikt över][Overview]</span><span class="sxs-lookup"><span data-stu-id="7adfb-104">[Overview][Overview]</span></span>
> * <span data-ttu-id="7adfb-105">[Datatyper][Data Types]</span><span class="sxs-lookup"><span data-stu-id="7adfb-105">[Data Types][Data Types]</span></span>
> * <span data-ttu-id="7adfb-106">[Distribuera][Distribute]</span><span class="sxs-lookup"><span data-stu-id="7adfb-106">[Distribute][Distribute]</span></span>
> * <span data-ttu-id="7adfb-107">[Index][Index]</span><span class="sxs-lookup"><span data-stu-id="7adfb-107">[Index][Index]</span></span>
> * <span data-ttu-id="7adfb-108">[Partition][Partition]</span><span class="sxs-lookup"><span data-stu-id="7adfb-108">[Partition][Partition]</span></span>
> * <span data-ttu-id="7adfb-109">[Statistik][Statistics]</span><span class="sxs-lookup"><span data-stu-id="7adfb-109">[Statistics][Statistics]</span></span>
> * <span data-ttu-id="7adfb-110">[Tillfällig][Temporary]</span><span class="sxs-lookup"><span data-stu-id="7adfb-110">[Temporary][Temporary]</span></span>
>
>

<span data-ttu-id="7adfb-111">SQL Data Warehouse är ett distribuerat MPP-databassystem.</span><span class="sxs-lookup"><span data-stu-id="7adfb-111">SQL Data Warehouse is a massively parallel processing (MPP) distributed database system.</span></span>  <span data-ttu-id="7adfb-112">Genom att dela upp data- och bearbetningsfunktioner på flera noder kan SQL Data Warehouse erbjuda fantastisk skalbarhet – långt över den i ett enskilt system.</span><span class="sxs-lookup"><span data-stu-id="7adfb-112">By dividing data and processing capability across multiple nodes, SQL Data Warehouse can offer huge scalability - far beyond any single system.</span></span>  <span data-ttu-id="7adfb-113">Bestämmer hur du distribuerar dina data i ditt SQL Data Warehouse är en av de viktigaste faktorerna att uppnå bästa prestanda.</span><span class="sxs-lookup"><span data-stu-id="7adfb-113">Deciding how to distribute your data within your SQL Data Warehouse is one of the most important factors to achieving optimal performance.</span></span>   <span data-ttu-id="7adfb-114">Nyckeln till optimala prestanda är minimera dataförflyttning och i sin tur på för att minimera dataflyttning är att välja rätt distributionsstrategi.</span><span class="sxs-lookup"><span data-stu-id="7adfb-114">The key to optimal performance is minimizing data movement and in turn the key to minimizing data movement is selecting the right distribution strategy.</span></span>

## <a name="understanding-data-movement"></a><span data-ttu-id="7adfb-115">Förstå dataflyttning</span><span class="sxs-lookup"><span data-stu-id="7adfb-115">Understanding data movement</span></span>
<span data-ttu-id="7adfb-116">Data från varje tabell fördelas på flera underliggande databaser i en MPP-system.</span><span class="sxs-lookup"><span data-stu-id="7adfb-116">In an MPP system, the data from each table is divided across several underlying databases.</span></span>  <span data-ttu-id="7adfb-117">De mest optimerade frågorna på ett MPP-system kan bara skickas via ska köras på enskilda distribuerade databaser utan interaktion mellan andra databaser.</span><span class="sxs-lookup"><span data-stu-id="7adfb-117">The most optimized queries on an MPP system can simply be passed through to execute on the individual distributed databases with no interaction between the other databases.</span></span>  <span data-ttu-id="7adfb-118">Anta exempelvis att du har en databas med försäljningsdata som innehåller två tabeller, försäljning och kunder.</span><span class="sxs-lookup"><span data-stu-id="7adfb-118">For example, let's say you have a database with sales data which contains two tables, sales and customers.</span></span>  <span data-ttu-id="7adfb-119">Om du har en fråga som ska ansluta till försäljning tabellen kundtabellen och du dividerar både försäljnings- och kunden tabeller upp med kundnummer, lägga till varje kund i en separat databas, lösas frågor som förenar försäljning och kunder i varje databas med ingen kunskap om de andra databaserna.</span><span class="sxs-lookup"><span data-stu-id="7adfb-119">If you have a query that needs to join your sales table to your customer table and you divide both your sales and customer tables up by customer number, putting each customer in a separate database, any queries which join sales and customer can be solved within each database with no knowledge of the other databases.</span></span>  <span data-ttu-id="7adfb-120">Däremot om du har delat din försäljningsdata genom ordningsnummer och din kundinformation på kundnummer sedan alla angivna databasen inte motsvarande data för varje kund och därmed om du vill ansluta din försäljningsdata till din kundinformation skulle du behöva hämta data för varje kund från andra databaser.</span><span class="sxs-lookup"><span data-stu-id="7adfb-120">In contrast, if you divided your sales data by order number and your customer data by customer number, then any given database will not have the corresponding data for each customer and thus if you wanted to join your sales data to your customer data, you would need to get the data for each customer from the other databases.</span></span>  <span data-ttu-id="7adfb-121">I den här andra exemplet måste dataflyttning inträffa om du vill flytta kundinformation till försäljning data, så att de kan anslutas.</span><span class="sxs-lookup"><span data-stu-id="7adfb-121">In this second example, data movement would need to occur to move the customer data to the sales data, so that the two tables can be joined.</span></span>  

<span data-ttu-id="7adfb-122">Dataflyttning inte alltid dåliga konsekvenser, ibland är det nödvändigt för att lösa en fråga.</span><span class="sxs-lookup"><span data-stu-id="7adfb-122">Data movement isn't always a bad thing, sometimes it's necessary to solve a query.</span></span>  <span data-ttu-id="7adfb-123">Men när den här extra steg kan undvikas, naturligt frågan körs snabbare.</span><span class="sxs-lookup"><span data-stu-id="7adfb-123">But when this extra step can be avoided, naturally your query will run faster.</span></span>  <span data-ttu-id="7adfb-124">Dataflyttning uppstår vanligen när tabellerna eller aggregeringar utförs.</span><span class="sxs-lookup"><span data-stu-id="7adfb-124">Data Movement most commonly arises when tables are joined or aggregations are performed.</span></span>  <span data-ttu-id="7adfb-125">Ofta behöver du både, medan du optimera för ett scenario som en koppling, måste du fortfarande flytt av data för att lösa för andra scenarier, t.ex. en aggregering.</span><span class="sxs-lookup"><span data-stu-id="7adfb-125">Often you need to do both, so while you may be able to optimize for one scenario, like a join, you still need data movement to help you solve for the other scenario, like an aggregation.</span></span>  <span data-ttu-id="7adfb-126">Tips är att räkna ut som är mindre arbete.</span><span class="sxs-lookup"><span data-stu-id="7adfb-126">The trick is figuring out which is less work.</span></span>  <span data-ttu-id="7adfb-127">I de flesta fall är distribuerar stora faktatabeller i en kolumn som är ofta kopplade mest effektiva metod för att minska de flesta dataflyttning.</span><span class="sxs-lookup"><span data-stu-id="7adfb-127">In most cases, distributing large fact tables on a commonly joined column is the most effective method for reducing the most data movement.</span></span>  <span data-ttu-id="7adfb-128">Distribuera data på kopplingskolumner är ett mycket vanligt sätt att minska dataflyttning än distribuerar data på kolumner som ingår i en samling.</span><span class="sxs-lookup"><span data-stu-id="7adfb-128">Distributing data on join columns is a much more common method to reduce data movement than distributing data on columns involved in an aggregation.</span></span>

## <a name="select-distribution-method"></a><span data-ttu-id="7adfb-129">Välj distributionsmetod</span><span class="sxs-lookup"><span data-stu-id="7adfb-129">Select distribution method</span></span>
<span data-ttu-id="7adfb-130">I bakgrunden delar dina data i 60 databaser i SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="7adfb-130">Behind the scenes, SQL Data Warehouse divides your data into 60 databases.</span></span>  <span data-ttu-id="7adfb-131">Varje enskild databas kallas för en **distribution**.</span><span class="sxs-lookup"><span data-stu-id="7adfb-131">Each individual database is referred to as a **distribution**.</span></span>  <span data-ttu-id="7adfb-132">När data läses in i varje tabell, har SQL Data Warehouse kunskap om att dela data mellan dessa 60 distributioner.</span><span class="sxs-lookup"><span data-stu-id="7adfb-132">When data is loaded into each table, SQL Data Warehouse has to know how to divide your data across these 60 distributions.</span></span>  

<span data-ttu-id="7adfb-133">Distributionsmetod definieras på tabellnivå och det finns två alternativ:</span><span class="sxs-lookup"><span data-stu-id="7adfb-133">The distribution method is defined at the table level and currently there are two choices:</span></span>

1. <span data-ttu-id="7adfb-134">**Resursallokering** som distribuera data jämnt men slumpmässigt.</span><span class="sxs-lookup"><span data-stu-id="7adfb-134">**Round robin** which distribute data evenly but randomly.</span></span>
2. <span data-ttu-id="7adfb-135">**Hash-distribuerade** som distribuerar data baserat på hash-värden från en enda kolumn</span><span class="sxs-lookup"><span data-stu-id="7adfb-135">**Hash Distributed** which distributes data based on hashing values from a single column</span></span>

<span data-ttu-id="7adfb-136">Som standard när du inte definierar en data-distributionsmetod tabellen kommer att distribueras med hjälp av den **resursallokering** distributionsmetod.</span><span class="sxs-lookup"><span data-stu-id="7adfb-136">By default, when you do not define a data distribution method, your table will be distributed using the **round robin** distribution method.</span></span>  <span data-ttu-id="7adfb-137">Men när du blir mer avancerade i din implementering, kommer du vilja bör du använda **hash distribuerade** tabeller för att minimera dataflyttning som i sin tur optimerar fråga prestanda.</span><span class="sxs-lookup"><span data-stu-id="7adfb-137">However, as you become more sophisticated in your implementation, you will want to consider using **hash distributed** tables to minimize data movement which will in turn optimize query performance.</span></span>

### <a name="round-robin-tables"></a><span data-ttu-id="7adfb-138">Resursallokering tabeller</span><span class="sxs-lookup"><span data-stu-id="7adfb-138">Round Robin Tables</span></span>
<span data-ttu-id="7adfb-139">Använda resursallokering metod distribuerar data är mycket hur det låter.</span><span class="sxs-lookup"><span data-stu-id="7adfb-139">Using the Round Robin method of distributing data is very much how it sounds.</span></span>  <span data-ttu-id="7adfb-140">När data har lästs in, skickas bara varje rad till nästa distributionen.</span><span class="sxs-lookup"><span data-stu-id="7adfb-140">As your data is loaded, each row is simply sent to the next distribution.</span></span>  <span data-ttu-id="7adfb-141">Den här metoden för att distribuera data kommer alltid slumpmässigt fördela data mycket jämnt över alla distributioner.</span><span class="sxs-lookup"><span data-stu-id="7adfb-141">This method of distributing the data will always randomly distribute the data very evenly across all of the distributions.</span></span>  <span data-ttu-id="7adfb-142">Det är ingen sortering under processen resursallokering som placerar dina data.</span><span class="sxs-lookup"><span data-stu-id="7adfb-142">That is, there is no sorting done during the round robin process which places your data.</span></span>  <span data-ttu-id="7adfb-143">En distributionsplats för resursallokering kallas ibland ett slumpmässigt hash därför.</span><span class="sxs-lookup"><span data-stu-id="7adfb-143">A round robin distribution is sometimes called a random hash for this reason.</span></span>  <span data-ttu-id="7adfb-144">Med en distribuerad tabell resursallokering finns inget behov av att förstå informationen.</span><span class="sxs-lookup"><span data-stu-id="7adfb-144">With a round-robin distributed table there is no need to understand the data.</span></span>  <span data-ttu-id="7adfb-145">Därför vara resursallokering tabeller en bra inläsning mål.</span><span class="sxs-lookup"><span data-stu-id="7adfb-145">For this reason, Round-Robin tables often make good loading targets.</span></span>

<span data-ttu-id="7adfb-146">Som standard om du väljer Ingen distributionsmetod används distributionsmetod resursallokering.</span><span class="sxs-lookup"><span data-stu-id="7adfb-146">By default, if no distribution method is chosen, the round robin distribution method will be used.</span></span>  <span data-ttu-id="7adfb-147">Medan resursallokering tabeller är enkla att använda, eftersom data distribueras slumpmässigt hela systemet innebär det att systemet inte kan garantera vilken distribution är varje rad på.</span><span class="sxs-lookup"><span data-stu-id="7adfb-147">However, while round robin tables are easy to use, because data is randomly distributed across the system it means that the system can't guarantee which distribution each row is on.</span></span>  <span data-ttu-id="7adfb-148">Därför måste systemet ibland att anropa en åtgärd för flytt av data för att organisera dina data innan den kan lösa en fråga.</span><span class="sxs-lookup"><span data-stu-id="7adfb-148">As a result, the system sometimes needs to invoke a data movement operation to better organize your data before it can resolve a query.</span></span>  <span data-ttu-id="7adfb-149">Den här extra steg kan sakta ned dina frågor.</span><span class="sxs-lookup"><span data-stu-id="7adfb-149">This extra step can slow down your queries.</span></span>

<span data-ttu-id="7adfb-150">Överväg att använda resursallokering (Round robin) för en tabell i följande scenarier:</span><span class="sxs-lookup"><span data-stu-id="7adfb-150">Consider using Round Robin distribution for your table in the following scenarios:</span></span>

* <span data-ttu-id="7adfb-151">När igång som en enkel startpunkt</span><span class="sxs-lookup"><span data-stu-id="7adfb-151">When getting started as a simple starting point</span></span>
* <span data-ttu-id="7adfb-152">Om det finns ingen uppenbara anslutande nyckel</span><span class="sxs-lookup"><span data-stu-id="7adfb-152">If there is no obvious joining key</span></span>
* <span data-ttu-id="7adfb-153">Om det inte är bra kandidat kolumn för hash-tabellen distribuerar</span><span class="sxs-lookup"><span data-stu-id="7adfb-153">If there is not good candidate column for hash distributing the table</span></span>
* <span data-ttu-id="7adfb-154">Om tabellen inte delar en gemensam join-nyckel med andra tabeller</span><span class="sxs-lookup"><span data-stu-id="7adfb-154">If the table does not share a common join key with other tables</span></span>
* <span data-ttu-id="7adfb-155">Om kopplingen är mindre viktig än andra kopplingar i frågan</span><span class="sxs-lookup"><span data-stu-id="7adfb-155">If the join is less significant than other joins in the query</span></span>
* <span data-ttu-id="7adfb-156">När tabellen är en tillfällig mellanlagring tabell</span><span class="sxs-lookup"><span data-stu-id="7adfb-156">When the table is a temporary staging table</span></span>

<span data-ttu-id="7adfb-157">Båda dessa exempel skapar en resursallokering tabell:</span><span class="sxs-lookup"><span data-stu-id="7adfb-157">Both of these examples will create a Round Robin Table:</span></span>

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
> <span data-ttu-id="7adfb-158">Resursallokering är bästa praxis anses vara standardtypen för tabell som är explicit i din DDL så att din tabellayout avsikt är tydliga till andra.</span><span class="sxs-lookup"><span data-stu-id="7adfb-158">While round robin is the default table type being explicit in your DDL is considered a best practice so that the intentions of your table layout are clear to others.</span></span>
>
>

### <a name="hash-distributed-tables"></a><span data-ttu-id="7adfb-159">Hash-distribuerade tabeller</span><span class="sxs-lookup"><span data-stu-id="7adfb-159">Hash Distributed Tables</span></span>
<span data-ttu-id="7adfb-160">Med hjälp av en **Hash distribuerade** algoritmen för att distribuera dina tabeller kan förbättra prestanda för många scenarier genom att minska dataflyttning frågan för närvarande.</span><span class="sxs-lookup"><span data-stu-id="7adfb-160">Using a **Hash distributed** algorithm to distribute your tables can improve performance for many scenarios by reducing data movement at query time.</span></span>  <span data-ttu-id="7adfb-161">Distribuerad hash-tabeller finns tabeller som delas mellan de distribuerade databaser med hjälp av en hash-algoritm på en enda kolumn som du väljer.</span><span class="sxs-lookup"><span data-stu-id="7adfb-161">Hash distributed tables are tables which are divided between the distributed databases using a hashing algorithm on a single column which you select.</span></span>  <span data-ttu-id="7adfb-162">Kolumnen distribution är vad avgör hur data delas över dina distribuerade databaser.</span><span class="sxs-lookup"><span data-stu-id="7adfb-162">The distribution column is what determines how the data is divided across your distributed databases.</span></span>  <span data-ttu-id="7adfb-163">Hash-funktionen använder kolumnen distribution för att tilldela distributioner rader.</span><span class="sxs-lookup"><span data-stu-id="7adfb-163">The hash function uses the distribution column to assign rows to distributions.</span></span>  <span data-ttu-id="7adfb-164">Hash-algoritm och resulterande distribution är deterministisk.</span><span class="sxs-lookup"><span data-stu-id="7adfb-164">The hashing algorithm and resulting distribution is deterministic.</span></span>  <span data-ttu-id="7adfb-165">Som har samma värde med samma datatyp kommer alltid till samma distributionsplats.</span><span class="sxs-lookup"><span data-stu-id="7adfb-165">That is the same value with the same data type will always has to the same distribution.</span></span>    

<span data-ttu-id="7adfb-166">Det här exemplet skapar en tabell som distribueras på-id:</span><span class="sxs-lookup"><span data-stu-id="7adfb-166">This example will create a table distributed on id:</span></span>

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

## <a name="select-distribution-column"></a><span data-ttu-id="7adfb-167">Välj distributionsplatser kolumn</span><span class="sxs-lookup"><span data-stu-id="7adfb-167">Select distribution column</span></span>
<span data-ttu-id="7adfb-168">När du väljer att **hash distribuera** en tabell, måste du markera en kolumn för enkel distribution.</span><span class="sxs-lookup"><span data-stu-id="7adfb-168">When you choose to **hash distribute** a table, you will need to select a single distribution column.</span></span>  <span data-ttu-id="7adfb-169">När du väljer en kolumn för distribution, finns det tre viktiga faktorer att tänka på.</span><span class="sxs-lookup"><span data-stu-id="7adfb-169">When selecting a distribution column, there are three major factors to consider.</span></span>  

<span data-ttu-id="7adfb-170">Markera en kolumn som används för att:</span><span class="sxs-lookup"><span data-stu-id="7adfb-170">Select a single column which will:</span></span>

1. <span data-ttu-id="7adfb-171">Inte uppdateras</span><span class="sxs-lookup"><span data-stu-id="7adfb-171">Not be updated</span></span>
2. <span data-ttu-id="7adfb-172">Distribuera data jämnt, undvika data skeva</span><span class="sxs-lookup"><span data-stu-id="7adfb-172">Distribute data evenly, avoiding data skew</span></span>
3. <span data-ttu-id="7adfb-173">Minimera dataflyttning</span><span class="sxs-lookup"><span data-stu-id="7adfb-173">Minimize data movement</span></span>

### <a name="select-distribution-column-which-will-not-be-updated"></a><span data-ttu-id="7adfb-174">Välj distributionsplatser kolumn som inte kommer att uppdateras</span><span class="sxs-lookup"><span data-stu-id="7adfb-174">Select distribution column which will not be updated</span></span>
<span data-ttu-id="7adfb-175">Distributionskolumner kan inte uppdateras, därför, markera en kolumn med statiska värden.</span><span class="sxs-lookup"><span data-stu-id="7adfb-175">Distribution columns are not updatable, therefore, select a column with static values.</span></span>  <span data-ttu-id="7adfb-176">Om en kolumn måste uppdateras, är vanligtvis inte kandidat bra distribution.</span><span class="sxs-lookup"><span data-stu-id="7adfb-176">If a column will need to be updated, it is generally not a good distribution candidate.</span></span>  <span data-ttu-id="7adfb-177">Det finns ett fall där du måste uppdatera en kolumn för distribution, kan du göra det genom att först ta bort raden och sedan lägga till en ny rad.</span><span class="sxs-lookup"><span data-stu-id="7adfb-177">If there is a case where you must update a distribution column, this can be done by first deleting the row and then inserting a new row.</span></span>

### <a name="select-distribution-column-which-will-distribute-data-evenly"></a><span data-ttu-id="7adfb-178">Välj distributionsplatser kolumn som ska distribuera data jämnt</span><span class="sxs-lookup"><span data-stu-id="7adfb-178">Select distribution column which will distribute data evenly</span></span>
<span data-ttu-id="7adfb-179">Eftersom ett distribuerat system utför bara så snabbt som motsvarande långsammaste distribution, är det viktigt att dela upp arbetet jämnt mellan distributioner för att uppnå belastningsutjämnade körning i hela systemet.</span><span class="sxs-lookup"><span data-stu-id="7adfb-179">Since a distributed system performs only as fast as its slowest distribution, it is important to divide the work evenly across the distributions in order to achieve balanced execution across the system.</span></span>  <span data-ttu-id="7adfb-180">Hur arbetet är uppdelad i ett distribuerat system baseras på där data för varje distribution finns.</span><span class="sxs-lookup"><span data-stu-id="7adfb-180">The way the work is divided on a distributed system is based on where the data for each distribution lives.</span></span>  <span data-ttu-id="7adfb-181">Detta gör det mycket viktigt att välja rätt distribution kolumnen för att distribuera data så att varje distribution har samma arbete och tar samma tid att slutföra sin del av arbetet.</span><span class="sxs-lookup"><span data-stu-id="7adfb-181">This makes it very important to select the right distribution column for distributing the data so that each distribution has equal work and will take the same time to complete its portion of the work.</span></span>  <span data-ttu-id="7adfb-182">När arbetet är väl indelat i hela systemet, fördelas data över distributioner.</span><span class="sxs-lookup"><span data-stu-id="7adfb-182">When work is well divided across the system, the data is balanced across the distributions.</span></span>  <span data-ttu-id="7adfb-183">När data inte är balanserade jämnt, vi kallar detta **data förskjutning**.</span><span class="sxs-lookup"><span data-stu-id="7adfb-183">When data is not evenly balanced, we call this **data skew**.</span></span>  

<span data-ttu-id="7adfb-184">För att dela upp data jämnt och undvika skeva data, Tänk på följande när du väljer din distribution kolumnen:</span><span class="sxs-lookup"><span data-stu-id="7adfb-184">To divide data evenly and avoid data skew, consider the following when selecting your distribution column:</span></span>

1. <span data-ttu-id="7adfb-185">Markera en kolumn som innehåller ett stort antal distinkta värden.</span><span class="sxs-lookup"><span data-stu-id="7adfb-185">Select a column which contains a significant number of distinct values.</span></span>
2. <span data-ttu-id="7adfb-186">Undvik att dela ut data på kolumner med några få distinkta värden.</span><span class="sxs-lookup"><span data-stu-id="7adfb-186">Avoid distributing data on columns with a few distinct values.</span></span>
3. <span data-ttu-id="7adfb-187">Undvik att dela ut data på kolumner med en hög frekvens av null-värden.</span><span class="sxs-lookup"><span data-stu-id="7adfb-187">Avoid distributing data on columns with a high frequency of nulls.</span></span>
4. <span data-ttu-id="7adfb-188">Undvik att dela ut data på datumkolumnerna.</span><span class="sxs-lookup"><span data-stu-id="7adfb-188">Avoid distributing data on date columns.</span></span>

<span data-ttu-id="7adfb-189">Eftersom varje värde hash-kodas till 1 i 60 distributioner, för att uppnå jämn fördelning vill du markera en kolumn som är mycket unik och innehåller mer än 60 unika värden.</span><span class="sxs-lookup"><span data-stu-id="7adfb-189">Since each value is hashed to 1 of 60 distributions, to achieve even distribution you will want to select a column that is highly unique and contains more than 60 unique values.</span></span>  <span data-ttu-id="7adfb-190">Överväg att fall där en kolumn har 40 unika värden bara för att illustrera.</span><span class="sxs-lookup"><span data-stu-id="7adfb-190">To illustrate, consider a case where a column only has 40 unique values.</span></span>  <span data-ttu-id="7adfb-191">Om den här kolumnen har valts som distribution tangent skulle data för tabellen hamna på 40 distributioner mest lämnar 20 distributioner med inga data och ingen bearbetning för att göra.</span><span class="sxs-lookup"><span data-stu-id="7adfb-191">If this column was selected as the distribution key, the data for that table would land on 40 distributions at most, leaving 20 distributions with no data and no processing to do.</span></span>  <span data-ttu-id="7adfb-192">Däremot måste de 40 distributioner mer behöver arbeta som om data har jämnt fördelade över 60 distributioner.</span><span class="sxs-lookup"><span data-stu-id="7adfb-192">Conversely, the other 40 distributions would have more work to do that if the data was evenly spread over 60 distributions.</span></span>  <span data-ttu-id="7adfb-193">Det här scenariot är ett exempel på data skeva.</span><span class="sxs-lookup"><span data-stu-id="7adfb-193">This scenario is an example of data skew.</span></span>

<span data-ttu-id="7adfb-194">Varje frågesteg väntar på alla distributioner att slutföra sin del av arbetet i MPP-system.</span><span class="sxs-lookup"><span data-stu-id="7adfb-194">In MPP system, each query step waits for all distributions to complete their share of the work.</span></span>  <span data-ttu-id="7adfb-195">Om en distribution att mer arbete än de andra, sedan till resursen om de andra distributioner är i stort sett nytta bara väntar på upptagen distribution.</span><span class="sxs-lookup"><span data-stu-id="7adfb-195">If one distribution is doing more work than the others, then the resource of the other distributions are essentially wasted just waiting on the busy distribution.</span></span>  <span data-ttu-id="7adfb-196">När arbetet inte är jämnt fördelade över alla distributioner, vi kallar detta **bearbetning skeva**.</span><span class="sxs-lookup"><span data-stu-id="7adfb-196">When work is not evenly spread across all distributions, we call this **processing skew**.</span></span>  <span data-ttu-id="7adfb-197">Förskjutning av bearbetning kommer frågor till långsammare än om arbetsbelastningen kan vara jämnt fördelade över distributioner.</span><span class="sxs-lookup"><span data-stu-id="7adfb-197">Processing skew will cause queries to run slower than if the workload can be evenly spread across the distributions.</span></span>  <span data-ttu-id="7adfb-198">Data skeva leder till skeva bearbetning.</span><span class="sxs-lookup"><span data-stu-id="7adfb-198">Data skew will lead to processing skew.</span></span>

<span data-ttu-id="7adfb-199">Undvik att distribuera på hög nullbara kolumnen som null-värden hamnar på samma distribution.</span><span class="sxs-lookup"><span data-stu-id="7adfb-199">Avoid distributing on highly nullable column as the null values will all land on the same distribution.</span></span> <span data-ttu-id="7adfb-200">Distribuera på ett datum också göra förskjutning av bearbetning eftersom alla data för ett visst datum hamnar på samma distribution.</span><span class="sxs-lookup"><span data-stu-id="7adfb-200">Distributing on a date column can also cause processing skew because all data for a given date will land on the same distribution.</span></span> <span data-ttu-id="7adfb-201">Om flera användare kör frågor blir bara alla filtrering på samma dag, och sedan endast 1 i 60 distributioner kommer att göra allt arbete sedan det angivna datumet på en distributionsplats.</span><span class="sxs-lookup"><span data-stu-id="7adfb-201">If several users are executing queries all filtering on the same date, then only 1 of the 60 distributions will be doing all of the work since a given date will only be on one distribution.</span></span> <span data-ttu-id="7adfb-202">Frågorna körs sannolikt 60 gånger långsammare än om data har sprida jämnt över alla distributioner i det här scenariot.</span><span class="sxs-lookup"><span data-stu-id="7adfb-202">In this scenario, the queries will likely run 60 times slower than if the data were equally spread over all of the distributions.</span></span>

<span data-ttu-id="7adfb-203">När det finns inga bra kandidat kolumner, Överväg att använda resursallokering (round robin) som distributionsmetod för.</span><span class="sxs-lookup"><span data-stu-id="7adfb-203">When no good candidate columns exist, then consider using round robin as the distribution method.</span></span>

### <a name="select-distribution-column-which-will-minimize-data-movement"></a><span data-ttu-id="7adfb-204">Välj distributionsplatser kolumn som minimerar dataflyttning</span><span class="sxs-lookup"><span data-stu-id="7adfb-204">Select distribution column which will minimize data movement</span></span>
<span data-ttu-id="7adfb-205">Minimera dataflyttning genom att välja rätt distribution kolumnen är en av de viktigaste strategierna för att optimera prestanda för ditt SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="7adfb-205">Minimizing data movement by selecting the right distribution column is one of the most important strategies for optimizing performance of your SQL Data Warehouse.</span></span>  <span data-ttu-id="7adfb-206">Dataflyttning uppstår vanligen när tabellerna eller aggregeringar utförs.</span><span class="sxs-lookup"><span data-stu-id="7adfb-206">Data Movement most commonly arises when tables are joined or aggregations are performed.</span></span>  <span data-ttu-id="7adfb-207">Kolumner som används i `JOIN`, `GROUP BY`, `DISTINCT`, `OVER` och `HAVING` satser alla gör för **bra** hash-kandidater för distribution.</span><span class="sxs-lookup"><span data-stu-id="7adfb-207">Columns used in `JOIN`, `GROUP BY`, `DISTINCT`, `OVER` and `HAVING` clauses all make for **good** hash distribution candidates.</span></span>

<span data-ttu-id="7adfb-208">Å andra sidan kolumner i den `WHERE` satsen gör **inte** gör för bra hash-kolumnen kandidater eftersom de begränsa vilka distributioner delta i frågan, orsakar bearbetning skeva.</span><span class="sxs-lookup"><span data-stu-id="7adfb-208">On the other hand, columns in the `WHERE` clause do **not** make for good hash column candidates because they limit which distributions participate in the query, causing processing skew.</span></span>  <span data-ttu-id="7adfb-209">Ett bra exempel på en kolumn som kan vara nära till hands att distribuera på, men ofta kan orsaka denna skeva bearbetning är en datumkolumn.</span><span class="sxs-lookup"><span data-stu-id="7adfb-209">A good example of a column which might be tempting to distribute on, but often can cause this processing skew is a date column.</span></span>

<span data-ttu-id="7adfb-210">Generellt sett om du har två stora-faktatabeller är ofta ingår i en koppling kommer du få de flesta prestanda genom att distribuera båda tabellerna på en av de kopplade kolumnerna.</span><span class="sxs-lookup"><span data-stu-id="7adfb-210">Generally speaking, if you have two large fact tables frequently involved in a join, you will gain the most performance by distributing both tables on one of the join columns.</span></span>  <span data-ttu-id="7adfb-211">Om du har en tabell som inte är ansluten till en annan stor faktatabell leta till kolumner som är vanliga i den `GROUP BY` satsen.</span><span class="sxs-lookup"><span data-stu-id="7adfb-211">If you have a table that is never joined to another large fact table, then look to columns that are frequently in the `GROUP BY` clause.</span></span>

<span data-ttu-id="7adfb-212">Det finns några viktiga villkor som måste uppfyllas för att undvika dataflyttning under en koppling:</span><span class="sxs-lookup"><span data-stu-id="7adfb-212">There are a few key criteria which must be met to avoid data movement during a join:</span></span>

1. <span data-ttu-id="7adfb-213">Tabeller som ingår i kopplingen måste vara hash distribueras vid **en** av kolumner som ingår i kopplingen.</span><span class="sxs-lookup"><span data-stu-id="7adfb-213">The tables involved in the join must be hash distributed on **one** of the columns participating in the join.</span></span>
2. <span data-ttu-id="7adfb-214">Datatyperna för kopplingskolumnerna måste matcha mellan båda tabellerna.</span><span class="sxs-lookup"><span data-stu-id="7adfb-214">The data types of the join columns must match between both tables.</span></span>
3. <span data-ttu-id="7adfb-215">Kolumnerna måste kopplas till en equals-operatorn.</span><span class="sxs-lookup"><span data-stu-id="7adfb-215">The columns must be joined with an equals operator.</span></span>
4. <span data-ttu-id="7adfb-216">Kopplingstyp får inte vara en `CROSS JOIN`.</span><span class="sxs-lookup"><span data-stu-id="7adfb-216">The join type may not be a `CROSS JOIN`.</span></span>

## <a name="troubleshooting-data-skew"></a><span data-ttu-id="7adfb-217">Felsökningsdata skeva</span><span class="sxs-lookup"><span data-stu-id="7adfb-217">Troubleshooting data skew</span></span>
<span data-ttu-id="7adfb-218">När tabelldata distribueras med hjälp av metoden hash-distribution finns en risk att vissa distributioner kommer förvrängd om du vill att oproportionerligt mer data än andra.</span><span class="sxs-lookup"><span data-stu-id="7adfb-218">When table data is distributed using the hash distribution method there is a chance that some distributions will be skewed to have disproportionately more data than others.</span></span> <span data-ttu-id="7adfb-219">Förskjutning av onödigt stora datamängder kan påverka prestanda för frågor eftersom slutresultatet av distribuerade frågor måste vänta på längst körningstid distributionen är klar.</span><span class="sxs-lookup"><span data-stu-id="7adfb-219">Excessive data skew can impact query performance because the final result of a distributed query must wait for the longest running distribution to finish.</span></span> <span data-ttu-id="7adfb-220">Du kan behöva åtgärda detta beroende på mängden data förskjutning.</span><span class="sxs-lookup"><span data-stu-id="7adfb-220">Depending on the degree of the data skew you might need to address it.</span></span>

### <a name="identifying-skew"></a><span data-ttu-id="7adfb-221">Identifiera förskjutning</span><span class="sxs-lookup"><span data-stu-id="7adfb-221">Identifying skew</span></span>
<span data-ttu-id="7adfb-222">Ett enkelt sätt att identifiera en tabell som förvrängd är att använda `DBCC PDW_SHOWSPACEUSED`.</span><span class="sxs-lookup"><span data-stu-id="7adfb-222">A simple way to identify a table as skewed is to use `DBCC PDW_SHOWSPACEUSED`.</span></span>  <span data-ttu-id="7adfb-223">Detta är ett mycket snabbt och enkelt sätt att se hur många rader som lagras i var och en av de 60 distributioner av databasen.</span><span class="sxs-lookup"><span data-stu-id="7adfb-223">This is a very quick and simple way to see the number of table rows that are stored in each of the 60 distributions of your database.</span></span>  <span data-ttu-id="7adfb-224">Kom ihåg att mest belastningsutjämnade prestanda raderna i tabellen distribuerade ska spridas jämnt över alla distributioner.</span><span class="sxs-lookup"><span data-stu-id="7adfb-224">Remember that for the most balanced performance, the rows in your distributed table should be spread evenly across all the distributions.</span></span>

```sql
-- Find data skew for a distributed table
DBCC PDW_SHOWSPACEUSED('dbo.FactInternetSales');
```

<span data-ttu-id="7adfb-225">Om du frågar de dynamiska hanteringsvyer (DMV) för Azure SQL Data Warehouse kan du utföra en mer detaljerad analys.</span><span class="sxs-lookup"><span data-stu-id="7adfb-225">However, if you query the Azure SQL Data Warehouse dynamic management views (DMV) you can perform a more detailed analysis.</span></span>  <span data-ttu-id="7adfb-226">Starta genom att skapa vyn [dbo.vTableSizes] [ dbo.vTableSizes] visa med SQL från [tabell översikt] [ Overview] artikel.</span><span class="sxs-lookup"><span data-stu-id="7adfb-226">To start, create the view [dbo.vTableSizes][dbo.vTableSizes] view using the SQL from [Table Overview][Overview] article.</span></span>  <span data-ttu-id="7adfb-227">När vyn har skapats kan du köra den här frågan för att identifiera vilka tabeller har mer än 10% data förskjutning.</span><span class="sxs-lookup"><span data-stu-id="7adfb-227">Once the view is created, run this query to identify which tables have more than 10% data skew.</span></span>

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

### <a name="resolving-data-skew"></a><span data-ttu-id="7adfb-228">Matcha data skeva</span><span class="sxs-lookup"><span data-stu-id="7adfb-228">Resolving data skew</span></span>
<span data-ttu-id="7adfb-229">Inte alla skeva räcker till garanterar en korrigering.</span><span class="sxs-lookup"><span data-stu-id="7adfb-229">Not all skew is enough to warrant a fix.</span></span>  <span data-ttu-id="7adfb-230">I vissa fall kan prestanda för en tabell i några frågor uppväger skada data skeva.</span><span class="sxs-lookup"><span data-stu-id="7adfb-230">In some cases, the performance of a table in some queries can outweigh the harm of data skew.</span></span>  <span data-ttu-id="7adfb-231">Om du vill avgöra om du ska lösa data skeva i en tabell, bör du känna till så mycket som möjligt om datavolymer och frågor i din arbetsbelastning.</span><span class="sxs-lookup"><span data-stu-id="7adfb-231">To decide if you should resolve data skew in a table, you should understand as much as possible about the data volumes and queries in your workload.</span></span>   <span data-ttu-id="7adfb-232">Ett sätt att titta på effekten av förskjutning är att använda stegen i den [frågan övervakning] [ Query Monitoring] artikel för att övervaka effekten av skeva på frågeprestanda och specifikt påverkan på hur länge frågar ta att slutföra på individuella distributioner.</span><span class="sxs-lookup"><span data-stu-id="7adfb-232">One way to look at the impact of skew is to use the steps in the [Query Monitoring][Query Monitoring] article to monitor the impact of skew on query performance and specifically the impact to how long queries take to complete on the individual distributions.</span></span>

<span data-ttu-id="7adfb-233">Distribuera data är en fråga för att hitta rätt balans mellan minimera förskjutning av data och minimera dataflyttning.</span><span class="sxs-lookup"><span data-stu-id="7adfb-233">Distributing data is a matter of finding the right balance between minimizing data skew and minimizing data movement.</span></span> <span data-ttu-id="7adfb-234">Dessa kan motverkar mål och ibland du vill behålla data skeva för att minska dataflyttning.</span><span class="sxs-lookup"><span data-stu-id="7adfb-234">These can be opposing goals, and sometimes you will want to keep data skew in order to reduce data movement.</span></span> <span data-ttu-id="7adfb-235">Till exempel när kolumnen distribution är ofta delade kolumnen kopplingar och aggregeringar kan kommer du att minimera dataflyttning.</span><span class="sxs-lookup"><span data-stu-id="7adfb-235">For example, when the distribution column is frequently the shared column in joins and aggregations, you will be minimizing data movement.</span></span> <span data-ttu-id="7adfb-236">Fördelen med minimal dataflyttning kan uppväger effekten av att data skeva.</span><span class="sxs-lookup"><span data-stu-id="7adfb-236">The benefit of having the minimal data movement might outweigh the impact of having data skew.</span></span>

<span data-ttu-id="7adfb-237">Det vanliga sättet att lösa förskjutning av data är att återskapa tabellen med en annan distributionsplats-kolumn.</span><span class="sxs-lookup"><span data-stu-id="7adfb-237">The typical way to resolve data skew is to re-create the table with a different distribution column.</span></span> <span data-ttu-id="7adfb-238">Eftersom det inte går att ändra kolumnen distribution på en befintlig tabell, ett sätt att ändra distributionen av en tabell att återskapa den med en [CTAS] [].</span><span class="sxs-lookup"><span data-stu-id="7adfb-238">Since there is no way to change the distribution column on an existing table, the way to change the distribution of a table it to recreate it with a [CTAS][].</span></span>  <span data-ttu-id="7adfb-239">Här är två exempel på hur lösa skeva data:</span><span class="sxs-lookup"><span data-stu-id="7adfb-239">Here are two examples of how resolve data skew:</span></span>

### <a name="example-1-re-create-the-table-with-a-new-distribution-column"></a><span data-ttu-id="7adfb-240">Exempel 1: Återskapa tabellen med en ny kolumn för distribution</span><span class="sxs-lookup"><span data-stu-id="7adfb-240">Example 1: Re-create the table with a new distribution column</span></span>
<span data-ttu-id="7adfb-241">Det här exemplet används [CTAS] [] för att återskapa en tabell med en annan hash-distribution kolumn.</span><span class="sxs-lookup"><span data-stu-id="7adfb-241">This example uses [CTAS][] to re-create a table with a different hash distribution column.</span></span>

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

--Rename the tables
RENAME OBJECT [dbo].[FactInternetSales] TO [FactInternetSales_ProductKey];
RENAME OBJECT [dbo].[FactInternetSales_CustomerKey] TO [FactInternetSales];
```

### <a name="example-2-re-create-the-table-using-round-robin-distribution"></a><span data-ttu-id="7adfb-242">Exempel 2: Återskapa tabellen med resursallokering (round robin)</span><span class="sxs-lookup"><span data-stu-id="7adfb-242">Example 2: Re-create the table using round robin distribution</span></span>
<span data-ttu-id="7adfb-243">Det här exemplet används [CTAS] [] för att återskapa en tabell med resursallokering i stället för en hash-distribution.</span><span class="sxs-lookup"><span data-stu-id="7adfb-243">This example uses [CTAS][] to re-create a table with round robin instead of a hash distribution.</span></span> <span data-ttu-id="7adfb-244">Den här ändringen genererar data fördelas jämnt på bekostnad av ökade dataflyttning.</span><span class="sxs-lookup"><span data-stu-id="7adfb-244">This change will produce even data distribution at the cost of increased data movement.</span></span>

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

--Rename the tables
RENAME OBJECT [dbo].[FactInternetSales] TO [FactInternetSales_HASH];
RENAME OBJECT [dbo].[FactInternetSales_ROUND_ROBIN] TO [FactInternetSales];
```

## <a name="next-steps"></a><span data-ttu-id="7adfb-245">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="7adfb-245">Next steps</span></span>
<span data-ttu-id="7adfb-246">Läs mer om tabelldesign i den [fördela][Distribute], [Index][Index], [Partition][Partition], [datatyper][Data Types], [statistik] [ Statistics] och [temporära tabeller] [ Temporary] artiklar.</span><span class="sxs-lookup"><span data-stu-id="7adfb-246">To learn more about table design, see the [Distribute][Distribute], [Index][Index], [Partition][Partition], [Data Types][Data Types], [Statistics][Statistics] and [Temporary Tables][Temporary] articles.</span></span>

<span data-ttu-id="7adfb-247">En översikt över bästa praxis, se [Metodtips för SQL Data Warehouse][SQL Data Warehouse Best Practices].</span><span class="sxs-lookup"><span data-stu-id="7adfb-247">For an overview of best practices, see [SQL Data Warehouse Best Practices][SQL Data Warehouse Best Practices].</span></span>

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
