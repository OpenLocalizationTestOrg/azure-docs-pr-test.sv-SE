---
title: aaaAzure SQL databas i minnet tekniker | Microsoft Docs
description: "Azure SQL Database i minnet tekniker förbättra hello prestanda för transaktionell och analytics arbetsbelastningar. Lär dig hur tootake nytta av dessa tekniker."
services: sql-database
documentationCenter: 
author: jodebrui
manager: jhubbard
editor: 
ms.assetid: 250ef341-90e5-492f-b075-b4750d237c05
ms.service: sql-database
ms.custom: develop databases
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/24/2017
ms.author: jodebrui
ms.openlocfilehash: 1bacd7297b2f9b018853088eabf2a2ee66a9cb43
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="optimize-performance-by-using-in-memory-technologies-in-sql-database"></a><span data-ttu-id="db629-104">Optimera prestanda genom att använda InMemory-tekniker i SQL-databas</span><span class="sxs-lookup"><span data-stu-id="db629-104">Optimize performance by using In-Memory technologies in SQL Database</span></span>

<span data-ttu-id="db629-105">Du kan åstadkomma prestandaförbättringar med olika arbetsbelastningar med hjälp av InMemory-tekniker i Azure SQL Database: transaktionell (online transaktionsbearbetning (OLTP)), analytics (online analytical processing (OLAP)), och blandade (hybrid transaktion analytical processing (HTAP)).</span><span class="sxs-lookup"><span data-stu-id="db629-105">By using In-Memory technologies in Azure SQL Database, you can achieve performance improvements with various workloads: transactional (online transactional processing (OLTP)), analytics (online analytical processing (OLAP)), and mixed (hybrid transaction/analytical processing (HTAP)).</span></span> <span data-ttu-id="db629-106">Eftersom Hej effektivare fråga och transaktionsbearbetning, InMemory-tekniker hjälper dig också tooreduce kostnaden.</span><span class="sxs-lookup"><span data-stu-id="db629-106">Because of hello more efficient query and transaction processing, In-Memory technologies also help you tooreduce cost.</span></span> <span data-ttu-id="db629-107">Vanligtvis behöver du inte tooupgrade hello prisnivån för hello databasen tooachieve prestandavinster.</span><span class="sxs-lookup"><span data-stu-id="db629-107">You typically don't need tooupgrade hello pricing tier of hello database tooachieve performance gains.</span></span> <span data-ttu-id="db629-108">I vissa fall kan du även eventuellt minska hello prisnivån när ändå prestandaförbättringar med InMemory-teknik.</span><span class="sxs-lookup"><span data-stu-id="db629-108">In some cases, you might even be able reduce hello pricing tier, while still seeing performance improvements with In-Memory technologies.</span></span>

<span data-ttu-id="db629-109">Här är två exempel på hur Minnesintern OLTP hjälpt toosignificantly förbättra prestanda:</span><span class="sxs-lookup"><span data-stu-id="db629-109">Here are two examples of how In-Memory OLTP helped toosignificantly improve performance:</span></span>

- <span data-ttu-id="db629-110">Med hjälp av Minnesintern OLTP [kvorum affärslösningar var kan toodouble arbetsbelastningen samtidigt förbättra dtu: er med 70%](https://customers.microsoft.com/story/quorum-doubles-key-databases-workload-while-lowering-dtu-with-sql-database).</span><span class="sxs-lookup"><span data-stu-id="db629-110">By using In-Memory OLTP, [Quorum Business Solutions was able toodouble their workload while improving DTUs by 70%](https://customers.microsoft.com/story/quorum-doubles-key-databases-workload-while-lowering-dtu-with-sql-database).</span></span>
    - <span data-ttu-id="db629-111">DTU innebär *databasen genomflödesenhet*, och innehåller en mesurement resursförbrukning.</span><span class="sxs-lookup"><span data-stu-id="db629-111">DTU means *database throughput unit*, and it includes a mesurement of resource consumption.</span></span>
- <span data-ttu-id="db629-112">hello följande videoklipp visar viktig förbättringar i resursanvändningen med ett exempel på arbetsbelastning: [Minnesintern OLTP i Azure SQL Database Video](https://channel9.msdn.com/Shows/Data-Exposed/In-Memory-OTLP-in-Azure-SQL-DB).</span><span class="sxs-lookup"><span data-stu-id="db629-112">hello following video demonstrates significant improvement in resource consumption with a sample workload: [In-Memory OLTP in Azure SQL Database Video](https://channel9.msdn.com/Shows/Data-Exposed/In-Memory-OTLP-in-Azure-SQL-DB).</span></span>
    - <span data-ttu-id="db629-113">Mer information finns i blogginlägget hello: [Minnesintern OLTP i Azure SQL Database-blogginlägg](https://azure.microsoft.com/blog/in-memory-oltp-in-azure-sql-database/)</span><span class="sxs-lookup"><span data-stu-id="db629-113">For more details, see hello blog post: [In-Memory OLTP in Azure SQL Database Blog Post](https://azure.microsoft.com/blog/in-memory-oltp-in-azure-sql-database/)</span></span>

<span data-ttu-id="db629-114">InMemory-teknik finns i alla databaser i hello Premium-nivån, inklusive databaser i Premium elastiska pooler.</span><span class="sxs-lookup"><span data-stu-id="db629-114">In-Memory technologies are available in all databases in hello Premium tier, including databases in Premium elastic pools.</span></span>

<span data-ttu-id="db629-115">hello förklarar följande videon potentiella prestandafördelar med InMemory-tekniker i Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="db629-115">hello following video explains potential performance gains with In-Memory technologies in Azure SQL Database.</span></span> <span data-ttu-id="db629-116">Kom ihåg att hello prestanda får du alltid ser beror på många faktorer, inklusive hello uppbyggnad hello arbetsbelastning och data, mönster för dataåtkomst på hello-databasen, och så vidare.</span><span class="sxs-lookup"><span data-stu-id="db629-116">Remember that hello performance gain that you see always depends on many factors, including hello nature of hello workload and data, access pattern of hello database, and so on.</span></span>

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Azure-SQL-Database-In-Memory-Technologies/player]
>
>

<span data-ttu-id="db629-117">Azure SQL Database har hello följande InMemory-tekniker:</span><span class="sxs-lookup"><span data-stu-id="db629-117">Azure SQL Database has hello following In-Memory technologies:</span></span>

- <span data-ttu-id="db629-118">*Minnesintern OLTP* ökar dataflödet och minskar svarstiden för transaktionsbearbetning.</span><span class="sxs-lookup"><span data-stu-id="db629-118">*In-Memory OLTP* increases throughput and reduces latency for transaction processing.</span></span> <span data-ttu-id="db629-119">Scenarier som har nytta i minnet OLTP är: hög genomströmning transaktionsbearbetning, till exempel handel och spel, datapåfyllning från händelser eller IoT-enheter, cachelagring datainläsning, och tillfällig tabell och tabellen variabeln scenarier.</span><span class="sxs-lookup"><span data-stu-id="db629-119">Scenarios that benefit from In-Memory OLTP are: high-throughput transaction processing such as trading and gaming, data ingestion from events or IoT devices, caching, data load, and temporary table and table variable scenarios.</span></span>
- <span data-ttu-id="db629-120">*Grupperade columnstore-index* minska din lagring storleken (upp too10 gånger) och förbättra prestanda för frågor för rapportering och analys.</span><span class="sxs-lookup"><span data-stu-id="db629-120">*Clustered columnstore indexes* reduce your storage footprint (up too10 times) and improve performance for reporting and analytics queries.</span></span> <span data-ttu-id="db629-121">Du kan använda den med faktatabeller i dina data dataarkiv toofit mer data i databasen och förbättra prestanda.</span><span class="sxs-lookup"><span data-stu-id="db629-121">You can use it with fact tables in your data marts toofit more data in your database and improve performance.</span></span> <span data-ttu-id="db629-122">Du kan också använda den med historiska data i din databas tooarchive och vara kan tooquery in too10 gånger mer data.</span><span class="sxs-lookup"><span data-stu-id="db629-122">Also, you can use it with historical data in your operational database tooarchive and be able tooquery up too10 times more data.</span></span>
- <span data-ttu-id="db629-123">*Icke-grupperat columnstore-index* HTAP hjälp du toogain realtid insikter om ditt företag genom att fråga hello operativa databasen direkt, utan hello måste toorun en dyr extrahering, transformering och laddning (ETL) processen och vänta för hello data warehouse toobe fylls i.</span><span class="sxs-lookup"><span data-stu-id="db629-123">*Nonclustered columnstore indexes* for HTAP help you toogain real-time insights into your business through querying hello operational database directly, without hello need toorun an expensive extract, transform, and load (ETL) process and wait for hello data warehouse toobe populated.</span></span> <span data-ttu-id="db629-124">Icke-grupperat columnstore-index fjärrkörning mycket snabbt analytics frågor på hello OLTP-databasen samtidigt minska hello inverkan på hello operativa arbetsbelastning.</span><span class="sxs-lookup"><span data-stu-id="db629-124">Nonclustered columnstore indexes allow very fast execution of analytics queries on hello OLTP database, while reducing hello impact on hello operational workload.</span></span>
- <span data-ttu-id="db629-125">Du kan också ha hello kombination av en minnesoptimerad tabell med ett columnstore-index.</span><span class="sxs-lookup"><span data-stu-id="db629-125">You can also have hello combination of a memory-optimized table with a columnstore index.</span></span> <span data-ttu-id="db629-126">Den här kombinationen kan tooperform mycket snabbt transaktionsbearbetning, och för*samtidigt* kör analytics frågar snabbt på hello samma data.</span><span class="sxs-lookup"><span data-stu-id="db629-126">This combination enables you tooperform very fast transaction processing, and too*concurrently* run analytics queries very quickly on hello same data.</span></span>

<span data-ttu-id="db629-127">Både columnstore-index och Minnesintern OLTP har varit en del av SQL Server-produkt för hello sedan 2012 och 2014, respektive.</span><span class="sxs-lookup"><span data-stu-id="db629-127">Both columnstore indexes and In-Memory OLTP have been part of hello SQL Server product since 2012 and 2014, respectively.</span></span> <span data-ttu-id="db629-128">Azure SQL Database och SQL Server dela hello samma implementering av InMemory-tekniker.</span><span class="sxs-lookup"><span data-stu-id="db629-128">Azure SQL Database and SQL Server share hello same implementation of In-Memory technologies.</span></span> <span data-ttu-id="db629-129">Framöver, släpps nya funktioner för dessa tekniker i Azure SQL Database först innan de blir tillgängliga i SQL Server.</span><span class="sxs-lookup"><span data-stu-id="db629-129">Going forward, new capabilities for these technologies are released in Azure SQL Database first, before they are released in SQL Server.</span></span>

<span data-ttu-id="db629-130">Det här avsnittet beskrivs aspekter av Minnesintern OLTP och columnstore-index som är specifika tooAzure SQL-databas och även exempel:</span><span class="sxs-lookup"><span data-stu-id="db629-130">This topic describes aspects of In-Memory OLTP and columnstore indexes that are specific tooAzure SQL Database and also includes samples:</span></span>
- <span data-ttu-id="db629-131">Hello effekten av dessa tekniker visas på storleksbegränsningar för lagring och data.</span><span class="sxs-lookup"><span data-stu-id="db629-131">You'll see hello impact of these technologies on storage and data size limits.</span></span>
- <span data-ttu-id="db629-132">Du ser hur toomanage hello flytt av databaser som använder dessa tekniker mellan hello olika prisnivåer.</span><span class="sxs-lookup"><span data-stu-id="db629-132">You'll see how toomanage hello movement of databases that use these technologies between hello different pricing tiers.</span></span>
- <span data-ttu-id="db629-133">Ser du två exempel som visar hello användning av Minnesintern OLTP, samt columnstore-index i Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="db629-133">You'll see two samples that illustrate hello use of In-Memory OLTP, as well as columnstore indexes in Azure SQL Database.</span></span>

<span data-ttu-id="db629-134">Se hello följande resurser för mer information.</span><span class="sxs-lookup"><span data-stu-id="db629-134">See hello following resources for more information.</span></span>

<span data-ttu-id="db629-135">Detaljerad information om hello tekniker:</span><span class="sxs-lookup"><span data-stu-id="db629-135">In-depth information about hello technologies:</span></span>

- <span data-ttu-id="db629-136">[Minnesintern OLTP översikt och Användningsscenarier](https://msdn.microsoft.com/library/mt774593.aspx) (omfattar referenser toocustomer fallstudier och information tooget igång)</span><span class="sxs-lookup"><span data-stu-id="db629-136">[In-Memory OLTP Overview and Usage Scenarios](https://msdn.microsoft.com/library/mt774593.aspx) (includes references toocustomer case studies and information tooget started)</span></span>
- [<span data-ttu-id="db629-137">Dokumentation för OLTP i minnet</span><span class="sxs-lookup"><span data-stu-id="db629-137">Documentation for In-Memory OLTP</span></span>](http://msdn.microsoft.com/library/dn133186.aspx)
- [<span data-ttu-id="db629-138">Guide för Columnstore-index</span><span class="sxs-lookup"><span data-stu-id="db629-138">Columnstore Indexes Guide</span></span>](https://msdn.microsoft.com/library/gg492088.aspx)
- <span data-ttu-id="db629-139">Hybrid transaktionella/analytisk behandling (HTAP), även kallat [operativa analys i realtid](https://msdn.microsoft.com/library/dn817827.aspx)</span><span class="sxs-lookup"><span data-stu-id="db629-139">Hybrid transactional/analytical processing (HTAP), also known as [real-time operational analytics](https://msdn.microsoft.com/library/dn817827.aspx)</span></span>

<span data-ttu-id="db629-140">En kort introduktion på Minnesintern OLTP: [snabb Start 1: I minnet OLTP-teknik för snabbare prestanda för T-SQL](http://msdn.microsoft.com/library/mt694156.aspx) (en annan artikel toohelp dig att komma igång)</span><span class="sxs-lookup"><span data-stu-id="db629-140">A quick primer on In-Memory OLTP: [Quick Start 1: In-Memory OLTP Technologies for Faster T-SQL Performance](http://msdn.microsoft.com/library/mt694156.aspx) (another article toohelp you get started)</span></span>

<span data-ttu-id="db629-141">Djupgående video om hello tekniker:</span><span class="sxs-lookup"><span data-stu-id="db629-141">In-depth videos about hello technologies:</span></span>

- <span data-ttu-id="db629-142">[Minnesintern OLTP i Azure SQL Database](https://channel9.msdn.com/Shows/Data-Exposed/In-Memory-OTLP-in-Azure-SQL-DB) (som innehåller en demonstration av prestanda fördelar och steg tooreproduce resultaten och själv)</span><span class="sxs-lookup"><span data-stu-id="db629-142">[In-Memory OLTP in Azure SQL Database](https://channel9.msdn.com/Shows/Data-Exposed/In-Memory-OTLP-in-Azure-SQL-DB) (which contains a demo of performance benefits and steps tooreproduce these results yourself)</span></span>
- [<span data-ttu-id="db629-143">Minnesintern OLTP video: Vad det är och när/hur toouse den</span><span class="sxs-lookup"><span data-stu-id="db629-143">In-Memory OLTP Videos: What it is and When/How toouse it</span></span>](https://blogs.msdn.microsoft.com/sqlserverstorageengine/2016/10/03/in-memory-oltp-video-what-it-is-and-whenhow-to-use-it/)
- [<span data-ttu-id="db629-144">Kolumnlagringsindexet: I minnet Analytics videor från Ignite 2016</span><span class="sxs-lookup"><span data-stu-id="db629-144">Columnstore Index: In-Memory Analytics Videos from Ignite 2016</span></span>](https://blogs.msdn.microsoft.com/sqlserverstorageengine/2016/10/04/columnstore-index-in-memory-analytics-i-e-columnstore-index-videos-from-ignite-2016/)

## <a name="storage-and-data-size"></a><span data-ttu-id="db629-145">Lagrings- och storlek</span><span class="sxs-lookup"><span data-stu-id="db629-145">Storage and data size</span></span>

### <a name="data-size-and-storage-cap-for-in-memory-oltp"></a><span data-ttu-id="db629-146">Storlek och lagring cap för OLTP i minnet</span><span class="sxs-lookup"><span data-stu-id="db629-146">Data size and storage cap for In-Memory OLTP</span></span>

<span data-ttu-id="db629-147">Minnesintern OLTP innehåller minnesoptimerade tabeller som används för lagring av användardata.</span><span class="sxs-lookup"><span data-stu-id="db629-147">In-Memory OLTP includes memory-optimized tables, which are used for storing user data.</span></span> <span data-ttu-id="db629-148">Dessa tabeller är obligatoriska toofit i minnet.</span><span class="sxs-lookup"><span data-stu-id="db629-148">These tables are required toofit in memory.</span></span> <span data-ttu-id="db629-149">Eftersom du hantera minne direkt i hello SQL Database-tjänsten har vi hello begreppet en kvot för användardata.</span><span class="sxs-lookup"><span data-stu-id="db629-149">Because you manage memory directly in hello SQL Database service, we have hello  concept of a quota for user data.</span></span> <span data-ttu-id="db629-150">Den här idén är refererad tooas *Minnesintern OLTP lagring*.</span><span class="sxs-lookup"><span data-stu-id="db629-150">This idea is referred tooas *In-Memory OLTP storage*.</span></span>

<span data-ttu-id="db629-151">Varje stöds fristående databas prisnivå och varje elastisk pool prisnivån innehåller mängden lagringsutrymme som OLTP i minnet.</span><span class="sxs-lookup"><span data-stu-id="db629-151">Each supported standalone database pricing tier and each elastic pool pricing tier includes a certain amount of In-Memory OLTP storage.</span></span> <span data-ttu-id="db629-152">När hello skrivning får du ett GB lagringsutrymme för varje 125 datatransaktionsenheter (Dtu) eller en elastisk databas transaktionsenheter (edtu: er).</span><span class="sxs-lookup"><span data-stu-id="db629-152">At hello time of writing, you get a gigabyte of storage for every 125 database transaction units (DTUs) or elastic database transaction units (eDTUs).</span></span>

<span data-ttu-id="db629-153">Hej [SQL Database servicenivåer](sql-database-service-tiers.md) artikeln har hello officiella lista över hello Minnesintern OLTP lagring som är tillgängliga för varje fristående databasen och stöds elastisk pool prisnivån.</span><span class="sxs-lookup"><span data-stu-id="db629-153">hello [SQL Database service tiers](sql-database-service-tiers.md) article has hello official list of hello In-Memory OLTP storage that is available for each supported standalone database and elastic pool pricing tier.</span></span>

<span data-ttu-id="db629-154">följande objekt räknas in din Minnesintern OLTP lagring cap hello:</span><span class="sxs-lookup"><span data-stu-id="db629-154">hello following items count toward your In-Memory OLTP storage cap:</span></span>

- <span data-ttu-id="db629-155">Aktiva användare datarader i minnesoptimerade tabeller och tabellvariabler.</span><span class="sxs-lookup"><span data-stu-id="db629-155">Active user data rows in memory-optimized tables and table variables.</span></span> <span data-ttu-id="db629-156">Observera att den gamla raden versioner inte räknas in hello fjärrskrivbordsanslutning.</span><span class="sxs-lookup"><span data-stu-id="db629-156">Note that old row versions don't count toward hello cap.</span></span>
- <span data-ttu-id="db629-157">Index för minnesoptimerade tabeller.</span><span class="sxs-lookup"><span data-stu-id="db629-157">Indexes on memory-optimized tables.</span></span>
- <span data-ttu-id="db629-158">Operativ tillsyn ALTER TABLE-åtgärder.</span><span class="sxs-lookup"><span data-stu-id="db629-158">Operational overhead of ALTER TABLE operations.</span></span>

<span data-ttu-id="db629-159">Om du träffar hello cap felmeddelandet out av kvot och du inte längre kan tooinsert eller uppdatera data.</span><span class="sxs-lookup"><span data-stu-id="db629-159">If you hit hello cap, you receive an out-of-quota error, and you are no longer able tooinsert or update data.</span></span> <span data-ttu-id="db629-160">toomitigate felet, ta bort data eller öka hello prisnivån hello databas eller poolen.</span><span class="sxs-lookup"><span data-stu-id="db629-160">toomitigate this error, delete data or increase hello pricing tier of hello database or pool.</span></span>

<span data-ttu-id="db629-161">Mer information om övervakning i minnet OLTP lagringsanvändningen och konfigurera aviseringar när du nästan träffar hello cap finns [övervakaren InMemory-lagringen](sql-database-in-memory-oltp-monitoring.md).</span><span class="sxs-lookup"><span data-stu-id="db629-161">For details about monitoring In-Memory OLTP storage utilization and configuring alerts when you almost hit hello cap, see [Monitor In-Memory storage](sql-database-in-memory-oltp-monitoring.md).</span></span>

#### <a name="about-elastic-pools"></a><span data-ttu-id="db629-162">Om elastiska pooler</span><span class="sxs-lookup"><span data-stu-id="db629-162">About elastic pools</span></span>

<span data-ttu-id="db629-163">Hello OLTP InMemory-lagringen delas mellan alla databaser i poolen hello med elastiska pooler.</span><span class="sxs-lookup"><span data-stu-id="db629-163">With elastic pools, hello In-Memory OLTP storage is shared across all databases in hello pool.</span></span> <span data-ttu-id="db629-164">Därför kan hello användning i en databas potentiellt påverka andra databaser.</span><span class="sxs-lookup"><span data-stu-id="db629-164">Therefore, hello usage in one database can potentially affect other databases.</span></span> <span data-ttu-id="db629-165">Det finns två åtgärder för den här:</span><span class="sxs-lookup"><span data-stu-id="db629-165">Two mitigations for this are:</span></span>

- <span data-ttu-id="db629-166">Konfigurera en Max-eDTU för databaser som är lägre än hello eDTU-antal för hello poolen som helhet.</span><span class="sxs-lookup"><span data-stu-id="db629-166">Configure a Max-eDTU for databases that is lower than hello eDTU count for hello pool as a whole.</span></span> <span data-ttu-id="db629-167">Den här högsta värdet caps hello lagringsanvändningen i minnet OLTP, i en databas i hello pool, toohello storlek som motsvarar toohello eDTU antal.</span><span class="sxs-lookup"><span data-stu-id="db629-167">This maximum caps hello In-Memory OLTP storage utilization, in any database in hello pool, toohello size that corresponds toohello eDTU count.</span></span>
- <span data-ttu-id="db629-168">Konfigurera en minsta eDTU som är större än 0.</span><span class="sxs-lookup"><span data-stu-id="db629-168">Configure a Min-eDTU that is greater than 0.</span></span> <span data-ttu-id="db629-169">Detta minsta garanterar att varje databas i hello poolen har hello mängden tillgängligt lagringsutrymme för OLTP i minnet som motsvarar toohello konfigurerad Min eDTU.</span><span class="sxs-lookup"><span data-stu-id="db629-169">This minimum guarantees that each database in hello pool has hello amount of available In-Memory OLTP storage that corresponds toohello configured Min-eDTU.</span></span>

### <a name="data-size-and-storage-for-columnstore-indexes"></a><span data-ttu-id="db629-170">Datastorlek och lagring för columnstore-index</span><span class="sxs-lookup"><span data-stu-id="db629-170">Data size and storage for columnstore indexes</span></span>

<span data-ttu-id="db629-171">Columnstore-index är inte obligatoriska toofit i minnet.</span><span class="sxs-lookup"><span data-stu-id="db629-171">Columnstore indexes aren't required toofit in memory.</span></span> <span data-ttu-id="db629-172">Därför hello endast fästpunkten på hello storlek hello index är hello största övergripande databasens storlek som dokumenteras i hello [SQL Database servicenivåer](sql-database-service-tiers.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="db629-172">Therefore, hello only cap on hello size of hello indexes is hello maximum overall database size, which is documented in hello [SQL Database service tiers](sql-database-service-tiers.md) article.</span></span>

<span data-ttu-id="db629-173">När du använder grupperade columnstore-index används kolumner komprimering för hello bastabellen lagring.</span><span class="sxs-lookup"><span data-stu-id="db629-173">When you use clustered columnstore indexes, columnar compression is used for hello base table storage.</span></span> <span data-ttu-id="db629-174">Den här komprimeringen kan avsevärt minska hello lagring storleken på användardata, vilket innebär att du får plats mer data i hello-databasen.</span><span class="sxs-lookup"><span data-stu-id="db629-174">This compression can significantly reduce hello storage footprint of your user data, which means that you can fit more data in hello database.</span></span> <span data-ttu-id="db629-175">Och hello komprimering kan förlängas med [kolumner arkivering komprimering](https://msdn.microsoft.com/library/cc280449.aspx#Using Columnstore and Columnstore Archive Compression).</span><span class="sxs-lookup"><span data-stu-id="db629-175">And hello compression can be further increased with [columnar archival compression](https://msdn.microsoft.com/library/cc280449.aspx#Using Columnstore and Columnstore Archive Compression).</span></span> <span data-ttu-id="db629-176">hello mängden komprimering som du kan åstadkomma beror på hello uppbyggnad hello data, men 10 gånger hello komprimering är inte ovanligt.</span><span class="sxs-lookup"><span data-stu-id="db629-176">hello amount of compression that you can achieve depends on hello nature of hello data, but 10 times hello compression is not uncommon.</span></span>

<span data-ttu-id="db629-177">Om du har en databas med en maximal storlek på 1 terabyte (TB) och du uppnå 10 gånger hello komprimering med hjälp av columnstore-index kan anpassa du totalt 10 TB användardata i hello-databasen.</span><span class="sxs-lookup"><span data-stu-id="db629-177">For example, if you have a database with a maximum size of 1 terabyte (TB) and you achieve 10 times hello compression by using columnstore indexes, you can fit a total of 10 TB of user data in hello database.</span></span>

<span data-ttu-id="db629-178">När du använder icke-grupperat columnstore-index, lagras fortfarande hello bastabellen hello traditionella rowstore format.</span><span class="sxs-lookup"><span data-stu-id="db629-178">When you use nonclustered columnstore indexes, hello base table is still stored in hello traditional rowstore format.</span></span> <span data-ttu-id="db629-179">Hej lagringsbesparingar är därför inte lika stor som med grupperade columnstore-index.</span><span class="sxs-lookup"><span data-stu-id="db629-179">Therefore, hello storage savings aren't as big as with clustered columnstore indexes.</span></span> <span data-ttu-id="db629-180">Om du ersätter ett antal traditionella icke-grupperat index med en enda columnstore-index, se du fortfarande en övergripande besparingar i hello lagring storleken för hello tabell.</span><span class="sxs-lookup"><span data-stu-id="db629-180">However, if you're replacing a number of traditional nonclustered indexes with a single columnstore index, you can still see an overall savings in hello storage footprint for hello table.</span></span>

## <a name="moving-databases-that-use-in-memory-technologies-between-pricing-tiers"></a><span data-ttu-id="db629-181">Flytta databaser som använder minnesinterna tekniker mellan prisnivåer</span><span class="sxs-lookup"><span data-stu-id="db629-181">Moving databases that use In-Memory technologies between pricing tiers</span></span>

<span data-ttu-id="db629-182">Det finns aldrig alla inkompatibiliteter eller andra problem när du uppgraderar tooa högre prisnivå, såsom från Standard tooPremium.</span><span class="sxs-lookup"><span data-stu-id="db629-182">There are never any incompatibilities or other problems when you upgrade tooa higher pricing tier, such as from Standard tooPremium.</span></span> <span data-ttu-id="db629-183">hello tillgängliga funktioner och resurser bara öka.</span><span class="sxs-lookup"><span data-stu-id="db629-183">hello available functionality and resources only increase.</span></span>

<span data-ttu-id="db629-184">Men lägre hello prisnivån kan påverka din databas.</span><span class="sxs-lookup"><span data-stu-id="db629-184">But downgrading hello pricing tier can negatively impact your database.</span></span> <span data-ttu-id="db629-185">hello påverkan är särskilt tydligt när du Nedgradera från tooStandard Premium eller Basic när databasen innehåller Minnesintern OLTP-objekt.</span><span class="sxs-lookup"><span data-stu-id="db629-185">hello impact is especially apparent when you downgrade from Premium tooStandard or Basic when your database contains In-Memory OLTP objects.</span></span> <span data-ttu-id="db629-186">Minnesoptimerade tabeller och columnstore-index är inte tillgänglig efter hello nedgradering (även om de är synligt).</span><span class="sxs-lookup"><span data-stu-id="db629-186">Memory-optimized tables, and columnstore indexes, are unavailable after hello downgrade (even if they remain visible).</span></span> <span data-ttu-id="db629-187">hello samma förutsättningar gäller när du sänker hello prisnivån för en elastisk pool eller flytta en databas med InMemory-tekniker, till en Standard eller grundläggande elastisk pool.</span><span class="sxs-lookup"><span data-stu-id="db629-187">hello same considerations apply when you're lowering hello pricing tier of an elastic pool, or moving a database with In-Memory technologies, into a Standard or Basic elastic pool.</span></span>

### <a name="in-memory-oltp"></a><span data-ttu-id="db629-188">Minnesintern OLTP</span><span class="sxs-lookup"><span data-stu-id="db629-188">In-Memory OLTP</span></span>

<span data-ttu-id="db629-189">*Nedgradera tooBasic/Standard*: Minnesintern OLTP stöds inte i databaser i hello Standard- eller Basic-nivån.</span><span class="sxs-lookup"><span data-stu-id="db629-189">*Downgrading tooBasic/Standard*: In-Memory OLTP isn't supported in databases in hello Standard or Basic tier.</span></span> <span data-ttu-id="db629-190">Det är dessutom inte möjligt toomove en databas som har alla Minnesintern OLTP objekt toohello Standard- eller Basic-nivån.</span><span class="sxs-lookup"><span data-stu-id="db629-190">In addition, it isn't possible toomove a database that has any In-Memory OLTP objects toohello Standard or Basic tier.</span></span>

<span data-ttu-id="db629-191">Ta bort alla minnesoptimerade tabeller och tabelltyper samt alla internt kompilerade moduler för T-SQL innan du nedgradera hello databasen tooStandard/enkel.</span><span class="sxs-lookup"><span data-stu-id="db629-191">Before you downgrade hello database tooStandard/Basic, remove all memory-optimized tables and table types, as well as all natively compiled T-SQL modules.</span></span>

<span data-ttu-id="db629-192">Det finns en programmässiga sätt toounderstand om en viss databas stöder Minnesintern OLTP.</span><span class="sxs-lookup"><span data-stu-id="db629-192">There is a programmatic way toounderstand whether a given database supports In-Memory OLTP.</span></span> <span data-ttu-id="db629-193">Du kan köra hello följande Transact-SQL-fråga:</span><span class="sxs-lookup"><span data-stu-id="db629-193">You can execute hello following Transact-SQL query:</span></span>

```
SELECT DatabasePropertyEx(DB_NAME(), 'IsXTPSupported');
```

<span data-ttu-id="db629-194">Om hello frågan returnerar **1**, i minnet OLTP stöds i den här databasen.</span><span class="sxs-lookup"><span data-stu-id="db629-194">If hello query returns **1**, In-Memory OLTP is supported in this database.</span></span>


<span data-ttu-id="db629-195">*Nedgradera tooa lägre premiumnivån*: Data i minnesoptimerade tabeller måste rymmas inom hello Minnesintern OLTP lagring som är associerad med hello prisnivån hello databasen eller som är tillgängliga i hello elastisk pool.</span><span class="sxs-lookup"><span data-stu-id="db629-195">*Downgrading tooa lower Premium tier*: Data in memory-optimized tables must fit within hello In-Memory OLTP storage that is associated with hello pricing tier of hello database or is available in hello elastic pool.</span></span> <span data-ttu-id="db629-196">Om du försöker toolower hello prisnivån eller flytta hello-databas till en pool som inte har tillräckligt med lagringsutrymme för OLTP i minnet, hello åtgärden misslyckas.</span><span class="sxs-lookup"><span data-stu-id="db629-196">If you try toolower hello pricing tier or move hello database into a pool that doesn't have enough available In-Memory OLTP storage, hello operation fails.</span></span>

### <a name="columnstore-indexes"></a><span data-ttu-id="db629-197">Columnstore-index</span><span class="sxs-lookup"><span data-stu-id="db629-197">Columnstore indexes</span></span>

<span data-ttu-id="db629-198">*Nedgradera tooBasic eller Standard*: Columnstore-index stöds endast på hello Premium-prisnivån och inte på hello Standard eller Basic nivåer.</span><span class="sxs-lookup"><span data-stu-id="db629-198">*Downgrading tooBasic or Standard*: Columnstore indexes are supported only on hello Premium pricing tier, and not on hello Standard or Basic tiers.</span></span> <span data-ttu-id="db629-199">När du nedgradera din databas tooStandard eller Basic otillgänglig columnstore-index.</span><span class="sxs-lookup"><span data-stu-id="db629-199">When you downgrade your database tooStandard or Basic, your columnstore index becomes unavailable.</span></span> <span data-ttu-id="db629-200">hello system behålls columnstore-index, men den använder aldrig hello index.</span><span class="sxs-lookup"><span data-stu-id="db629-200">hello system maintains your columnstore index, but it never leverages hello index.</span></span> <span data-ttu-id="db629-201">Om du senare uppgradera tillbaka tooPremium är columnstore-index klar toobe utnyttjas igen.</span><span class="sxs-lookup"><span data-stu-id="db629-201">If you later upgrade back tooPremium, your columnstore index is immediately ready toobe leveraged again.</span></span>

<span data-ttu-id="db629-202">Om du har en **klustrade** columnstore-index hello hela tabellen blir tillgänglig efter nivå nedgradering.</span><span class="sxs-lookup"><span data-stu-id="db629-202">If you have a **clustered** columnstore index, hello whole table becomes unavailable after tier downgrade.</span></span> <span data-ttu-id="db629-203">Därför rekommenderar vi att du ta bort alla *klustrade* columnstore-index innan du nedgradera databasen under hello Premium-nivån.</span><span class="sxs-lookup"><span data-stu-id="db629-203">Therefore we recommend that you drop all *clustered* columnstore indexes before you downgrade your database below hello Premium tier.</span></span>

<span data-ttu-id="db629-204">*Nedgradera tooa lägre premiumnivån*: den här nedgradering lyckas om hello hela databasen ryms inom hello maximala databasstorleken för hello mål prisnivån eller inom hello tillgängligt lagringsutrymme i hello elastisk pool.</span><span class="sxs-lookup"><span data-stu-id="db629-204">*Downgrading tooa lower Premium tier*: This downgrade succeeds if hello whole database fits within hello maximum database size for hello target pricing tier, or within hello available storage in hello elastic pool.</span></span> <span data-ttu-id="db629-205">Det finns ingen specifik inverkan från hello columnstore-index.</span><span class="sxs-lookup"><span data-stu-id="db629-205">There is no specific impact from hello columnstore indexes.</span></span>


<a id="install_oltp_manuallink" name="install_oltp_manuallink"></a>

&nbsp;

## <a name="1-install-hello-in-memory-oltp-sample"></a><span data-ttu-id="db629-206">1. Installera hello Minnesintern OLTP-exempel</span><span class="sxs-lookup"><span data-stu-id="db629-206">1. Install hello In-Memory OLTP sample</span></span>

<span data-ttu-id="db629-207">Du kan skapa hello AdventureWorksLT exempeldatabasen med ett par klick i hello [Azure-portalen](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="db629-207">You can create hello AdventureWorksLT sample database with a few clicks in hello [Azure portal](https://portal.azure.com/).</span></span> <span data-ttu-id="db629-208">Sedan beskrivs hello stegen i det här avsnittet hur du kan utöka AdventureWorksLT databasen med InMemory-OLTP-objekt och visa prestandafördelarna.</span><span class="sxs-lookup"><span data-stu-id="db629-208">Then, hello steps in this section explain how you can enrich your AdventureWorksLT database with In-Memory OLTP objects and demonstrate performance benefits.</span></span>

<span data-ttu-id="db629-209">En mer simplistic, men mer tilltalande prestanda demo för InMemory-OLTP finns:</span><span class="sxs-lookup"><span data-stu-id="db629-209">For a more simplistic, but more visually appealing performance demo for In-Memory OLTP, see:</span></span>

- <span data-ttu-id="db629-210">Utgåvan: [i-minne-oltp-demo-v1.0](https://github.com/Microsoft/sql-server-samples/releases/tag/in-memory-oltp-demo-v1.0)</span><span class="sxs-lookup"><span data-stu-id="db629-210">Release: [in-memory-oltp-demo-v1.0](https://github.com/Microsoft/sql-server-samples/releases/tag/in-memory-oltp-demo-v1.0)</span></span>
- <span data-ttu-id="db629-211">Källkod: [in-memory-oltp-demo-source-code](https://github.com/Microsoft/sql-server-samples/tree/master/samples/features/in-memory/ticket-reservations)</span><span class="sxs-lookup"><span data-stu-id="db629-211">Source code: [in-memory-oltp-demo-source-code](https://github.com/Microsoft/sql-server-samples/tree/master/samples/features/in-memory/ticket-reservations)</span></span>

#### <a name="installation-steps"></a><span data-ttu-id="db629-212">Installationssteg</span><span class="sxs-lookup"><span data-stu-id="db629-212">Installation steps</span></span>

1. <span data-ttu-id="db629-213">I hello [Azure-portalen](https://portal.azure.com/), skapar en Premium-databas på en server.</span><span class="sxs-lookup"><span data-stu-id="db629-213">In hello [Azure portal](https://portal.azure.com/), create a Premium database on a server.</span></span> <span data-ttu-id="db629-214">Ange hello **källa** toohello AdventureWorksLT exempeldatabasen.</span><span class="sxs-lookup"><span data-stu-id="db629-214">Set hello **Source** toohello AdventureWorksLT sample database.</span></span> <span data-ttu-id="db629-215">Detaljerade instruktioner finns [skapa din första Azure SQL-databas](sql-database-get-started-portal.md).</span><span class="sxs-lookup"><span data-stu-id="db629-215">For detailed instructions, see [Create your first Azure SQL database](sql-database-get-started-portal.md).</span></span>

2. <span data-ttu-id="db629-216">Anslut toohello databasen med SQL Server Management Studio [(SSMS.exe)](http://msdn.microsoft.com/library/mt238290.aspx).</span><span class="sxs-lookup"><span data-stu-id="db629-216">Connect toohello database with SQL Server Management Studio [(SSMS.exe)](http://msdn.microsoft.com/library/mt238290.aspx).</span></span>

3. <span data-ttu-id="db629-217">Kopiera hello [Minnesintern OLTP Transact-SQL-skript](https://raw.githubusercontent.com/Microsoft/sql-server-samples/master/samples/features/in-memory/t-sql-scripts/sql_in-memory_oltp_sample.sql) tooyour Urklipp.</span><span class="sxs-lookup"><span data-stu-id="db629-217">Copy hello [In-Memory OLTP Transact-SQL script](https://raw.githubusercontent.com/Microsoft/sql-server-samples/master/samples/features/in-memory/t-sql-scripts/sql_in-memory_oltp_sample.sql) tooyour clipboard.</span></span> <span data-ttu-id="db629-218">Hej T-SQL-skript skapar hello nödvändiga InMemory-objekt i hello AdventureWorksLT exempeldatabasen som du skapade i steg 1.</span><span class="sxs-lookup"><span data-stu-id="db629-218">hello T-SQL script creates hello necessary In-Memory objects in hello AdventureWorksLT sample database that you created in step 1.</span></span>

4. <span data-ttu-id="db629-219">Klistra in hello T-SQL-skript i SSMS och sedan köra hello skript.</span><span class="sxs-lookup"><span data-stu-id="db629-219">Paste hello T-SQL script into SSMS, and then execute hello script.</span></span> <span data-ttu-id="db629-220">Hej `MEMORY_OPTIMIZED = ON` instruktionen CREATE TABLE-satser är avgörande.</span><span class="sxs-lookup"><span data-stu-id="db629-220">hello `MEMORY_OPTIMIZED = ON` clause CREATE TABLE statements are crucial.</span></span> <span data-ttu-id="db629-221">Exempel:</span><span class="sxs-lookup"><span data-stu-id="db629-221">For example:</span></span>


```
CREATE TABLE [SalesLT].[SalesOrderHeader_inmem](
    [SalesOrderID] int IDENTITY NOT NULL PRIMARY KEY NONCLUSTERED ...,
    ...
) WITH (MEMORY_OPTIMIZED = ON);
```


#### <a name="error-40536"></a><span data-ttu-id="db629-222">Fel 40536</span><span class="sxs-lookup"><span data-stu-id="db629-222">Error 40536</span></span>


<span data-ttu-id="db629-223">Om det uppstår fel 40536 när du kör hello T-SQL-skript kör följande T-SQL-skript tooverify om hello databasen stöder i minnet hello:</span><span class="sxs-lookup"><span data-stu-id="db629-223">If you get error 40536 when you run hello T-SQL script, run hello following T-SQL script tooverify whether hello database supports In-Memory:</span></span>


```
SELECT DatabasePropertyEx(DB_Name(), 'IsXTPSupported');
```


<span data-ttu-id="db629-224">Ett resultat av **0** innebär att i minnet inte stöds och **1** innebär att stöds.</span><span class="sxs-lookup"><span data-stu-id="db629-224">A result of **0** means that In-Memory isn't supported, and **1** means that it is supported.</span></span> <span data-ttu-id="db629-225">toodiagnose hello problemet, se till att hello-databasen i hello premiumnivån.</span><span class="sxs-lookup"><span data-stu-id="db629-225">toodiagnose hello problem, ensure that hello database is at hello Premium service tier.</span></span>


#### <a name="about-hello-created-memory-optimized-items"></a><span data-ttu-id="db629-226">Om hello skapade minnesoptimerade objekt</span><span class="sxs-lookup"><span data-stu-id="db629-226">About hello created memory-optimized items</span></span>

<span data-ttu-id="db629-227">**Tabeller**: hello exemplet innehåller hello följande minnesoptimerade tabeller:</span><span class="sxs-lookup"><span data-stu-id="db629-227">**Tables**: hello sample contains hello following memory-optimized tables:</span></span>

- <span data-ttu-id="db629-228">SalesLT.Product_inmem</span><span class="sxs-lookup"><span data-stu-id="db629-228">SalesLT.Product_inmem</span></span>
- <span data-ttu-id="db629-229">SalesLT.SalesOrderHeader_inmem</span><span class="sxs-lookup"><span data-stu-id="db629-229">SalesLT.SalesOrderHeader_inmem</span></span>
- <span data-ttu-id="db629-230">SalesLT.SalesOrderDetail_inmem</span><span class="sxs-lookup"><span data-stu-id="db629-230">SalesLT.SalesOrderDetail_inmem</span></span>
- <span data-ttu-id="db629-231">Demo.DemoSalesOrderHeaderSeed</span><span class="sxs-lookup"><span data-stu-id="db629-231">Demo.DemoSalesOrderHeaderSeed</span></span>
- <span data-ttu-id="db629-232">Demo.DemoSalesOrderDetailSeed</span><span class="sxs-lookup"><span data-stu-id="db629-232">Demo.DemoSalesOrderDetailSeed</span></span>


<span data-ttu-id="db629-233">Du kan inspektera minnesoptimerade tabeller via hello **Object Explorer** i SSMS.</span><span class="sxs-lookup"><span data-stu-id="db629-233">You can inspect memory-optimized tables through hello **Object Explorer** in SSMS.</span></span> <span data-ttu-id="db629-234">Högerklicka på **tabeller** > **Filter** > **filtrera inställningar** > **är Minnesoptimerad**.</span><span class="sxs-lookup"><span data-stu-id="db629-234">Right-click **Tables** > **Filter** > **Filter Settings** > **Is Memory Optimized**.</span></span> <span data-ttu-id="db629-235">hello-värdet är lika med 1.</span><span class="sxs-lookup"><span data-stu-id="db629-235">hello value equals 1.</span></span>


<span data-ttu-id="db629-236">Eller fråga hello katalogvyer, exempelvis:</span><span class="sxs-lookup"><span data-stu-id="db629-236">Or you can query hello catalog views, such as:</span></span>


```
SELECT is_memory_optimized, name, type_desc, durability_desc
    FROM sys.tables
    WHERE is_memory_optimized = 1;
```


<span data-ttu-id="db629-237">**Internt kompilerade lagrade proceduren**: du kan inspektera SalesLT.usp_InsertSalesOrder_inmem via en katalog Visa fråga:</span><span class="sxs-lookup"><span data-stu-id="db629-237">**Natively compiled stored procedure**: You can inspect SalesLT.usp_InsertSalesOrder_inmem through a catalog view query:</span></span>


```
SELECT uses_native_compilation, OBJECT_NAME(object_id), definition
    FROM sys.sql_modules
    WHERE uses_native_compilation = 1;
```


&nbsp;

### <a name="run-hello-sample-oltp-workload"></a><span data-ttu-id="db629-238">Kör hello exempel OLTP-arbetsbelastning</span><span class="sxs-lookup"><span data-stu-id="db629-238">Run hello sample OLTP workload</span></span>

<span data-ttu-id="db629-239">Hej endast skillnaden mellan hello följande två *lagrade procedurer* är att hello första proceduren använder minnesoptimerade tabeller hello-versioner, när hello använder den andra proceduren hello vanliga på disken tabeller:</span><span class="sxs-lookup"><span data-stu-id="db629-239">hello only difference between hello following two *stored procedures* is that hello first procedure uses memory-optimized versions of hello tables, while hello second procedure uses hello regular on-disk tables:</span></span>

- <span data-ttu-id="db629-240">SalesLT**.** usp_InsertSalesOrder**_inmem**</span><span class="sxs-lookup"><span data-stu-id="db629-240">SalesLT**.**usp_InsertSalesOrder**_inmem**</span></span>
- <span data-ttu-id="db629-241">SalesLT**.** usp_InsertSalesOrder**_ondisk**</span><span class="sxs-lookup"><span data-stu-id="db629-241">SalesLT**.**usp_InsertSalesOrder**_ondisk**</span></span>


<span data-ttu-id="db629-242">I det här avsnittet visas hur toouse hello praktiska **ostress.exe** verktyget tooexecute hello två lagrade procedurer på stress nivåer.</span><span class="sxs-lookup"><span data-stu-id="db629-242">In this section, you see how toouse hello handy **ostress.exe** utility tooexecute hello two stored procedures at stressful levels.</span></span> <span data-ttu-id="db629-243">Du kan jämföra hur lång tid det tar för hello två stress körs toofinish.</span><span class="sxs-lookup"><span data-stu-id="db629-243">You can compare how long it takes for hello two stress runs toofinish.</span></span>


<span data-ttu-id="db629-244">När du kör ostress.exe, rekommenderar vi att du skickar parametervärden som utformats för både hello följande:</span><span class="sxs-lookup"><span data-stu-id="db629-244">When you run ostress.exe, we recommend that you pass parameter values designed for both of hello following:</span></span>

- <span data-ttu-id="db629-245">Kör ett stort antal samtidiga anslutningar med hjälp av - n100.</span><span class="sxs-lookup"><span data-stu-id="db629-245">Run a large number of concurrent connections, by using -n100.</span></span>
- <span data-ttu-id="db629-246">Har varje anslutning slinga hundratals gånger, med hjälp av - r500.</span><span class="sxs-lookup"><span data-stu-id="db629-246">Have each connection loop hundreds of times, by using -r500.</span></span>


<span data-ttu-id="db629-247">Däremot kanske du vill toostart med mycket mindre värdena så - n10 och -r50 tooensure att allt fungerar.</span><span class="sxs-lookup"><span data-stu-id="db629-247">However, you might want toostart with much smaller values like -n10 and -r50 tooensure that everything is working.</span></span>


### <a name="script-for-ostressexe"></a><span data-ttu-id="db629-248">Skriptet för ostress.exe</span><span class="sxs-lookup"><span data-stu-id="db629-248">Script for ostress.exe</span></span>


<span data-ttu-id="db629-249">Det här avsnittet visar hello T-SQL-skript som är inbäddad i vår ostress.exe-kommandoraden.</span><span class="sxs-lookup"><span data-stu-id="db629-249">This section displays hello T-SQL script that is embedded in our ostress.exe command line.</span></span> <span data-ttu-id="db629-250">hello-skript som använder objekt som har skapats av hello T-SQL-skript som du tidigare har installerat.</span><span class="sxs-lookup"><span data-stu-id="db629-250">hello script uses items that were created by hello T-SQL script that you installed earlier.</span></span>


<span data-ttu-id="db629-251">hello följande skript infogar ett exempel ordern fem artiklar i hello följande minnesoptimerade *tabeller*:</span><span class="sxs-lookup"><span data-stu-id="db629-251">hello following script inserts a sample sales order with five line items into hello following memory-optimized *tables*:</span></span>

- <span data-ttu-id="db629-252">SalesLT.SalesOrderHeader_inmem</span><span class="sxs-lookup"><span data-stu-id="db629-252">SalesLT.SalesOrderHeader_inmem</span></span>
- <span data-ttu-id="db629-253">SalesLT.SalesOrderDetail_inmem</span><span class="sxs-lookup"><span data-stu-id="db629-253">SalesLT.SalesOrderDetail_inmem</span></span>


```
DECLARE
    @i int = 0,
    @od SalesLT.SalesOrderDetailType_inmem,
    @SalesOrderID int,
    @DueDate datetime2 = sysdatetime(),
    @CustomerID int = rand() * 8000,
    @BillToAddressID int = rand() * 10000,
    @ShipToAddressID int = rand() * 10000;

INSERT INTO @od
    SELECT OrderQty, ProductID
    FROM Demo.DemoSalesOrderDetailSeed
    WHERE OrderID= cast((rand()*60) as int);

WHILE (@i < 20)
begin;
    EXECUTE SalesLT.usp_InsertSalesOrder_inmem @SalesOrderID OUTPUT,
        @DueDate, @CustomerID, @BillToAddressID, @ShipToAddressID, @od;
    SET @i = @i + 1;
end
```


<span data-ttu-id="db629-254">toomake hello *_ondisk* version av Hej föregående T-SQL-skript för ostress.exe, ska du ersätta båda förekomster av hello *_inmem* substring med *_ondisk*.</span><span class="sxs-lookup"><span data-stu-id="db629-254">toomake hello *_ondisk* version of hello preceding T-SQL script for ostress.exe, you would replace both occurrences of hello *_inmem* substring with *_ondisk*.</span></span> <span data-ttu-id="db629-255">Dessa ersättningar påverkar hello namnen på tabeller och lagrade procedurer.</span><span class="sxs-lookup"><span data-stu-id="db629-255">These replacements affect hello names of tables and stored procedures.</span></span>


### <a name="install-rml-utilities-and-ostress"></a><span data-ttu-id="db629-256">Installera RML verktyg och ostress</span><span class="sxs-lookup"><span data-stu-id="db629-256">Install RML utilities and ostress</span></span>


<span data-ttu-id="db629-257">Du skulle du bör planera toorun ostress.exe på en Azure-dator (VM).</span><span class="sxs-lookup"><span data-stu-id="db629-257">Ideally, you would plan toorun ostress.exe on an Azure virtual machine (VM).</span></span> <span data-ttu-id="db629-258">Du skapar en [Azure VM](https://azure.microsoft.com/documentation/services/virtual-machines/) i hello samma Azure geografiska region där AdventureWorksLT databasen finns.</span><span class="sxs-lookup"><span data-stu-id="db629-258">You would create an [Azure VM](https://azure.microsoft.com/documentation/services/virtual-machines/) in hello same Azure geographic region where your AdventureWorksLT database resides.</span></span> <span data-ttu-id="db629-259">Men du kan köra ostress.exe på din bärbara dator i stället.</span><span class="sxs-lookup"><span data-stu-id="db629-259">But you can run ostress.exe on your laptop instead.</span></span>


<span data-ttu-id="db629-260">På hello VM, eller oavsett värd du väljer, installera verktyg för hello Replay Markup Language (RML).</span><span class="sxs-lookup"><span data-stu-id="db629-260">On hello VM, or on whatever host you choose, install hello Replay Markup Language (RML) utilities.</span></span> <span data-ttu-id="db629-261">hello verktyg inkluderar ostress.exe.</span><span class="sxs-lookup"><span data-stu-id="db629-261">hello utilities include ostress.exe.</span></span>

<span data-ttu-id="db629-262">Mer information finns i:</span><span class="sxs-lookup"><span data-stu-id="db629-262">For more information, see:</span></span>
- <span data-ttu-id="db629-263">Hej ostress.exe diskussion i [exempeldatabasen för InMemory-OLTP](http://msdn.microsoft.com/library/mt465764.aspx).</span><span class="sxs-lookup"><span data-stu-id="db629-263">hello ostress.exe discussion in [Sample Database for In-Memory OLTP](http://msdn.microsoft.com/library/mt465764.aspx).</span></span>
- <span data-ttu-id="db629-264">[Exempel på databasen för InMemory-OLTP](http://msdn.microsoft.com/library/mt465764.aspx).</span><span class="sxs-lookup"><span data-stu-id="db629-264">[Sample Database for In-Memory OLTP](http://msdn.microsoft.com/library/mt465764.aspx).</span></span>
- <span data-ttu-id="db629-265">Hej [bloggen för att installera ostress.exe](http://blogs.msdn.com/b/psssql/archive/2013/10/29/cumulative-update-2-to-the-rml-utilities-for-microsoft-sql-server-released.aspx).</span><span class="sxs-lookup"><span data-stu-id="db629-265">hello [blog for installing ostress.exe](http://blogs.msdn.com/b/psssql/archive/2013/10/29/cumulative-update-2-to-the-rml-utilities-for-microsoft-sql-server-released.aspx).</span></span>



<!--
dn511655.aspx is for SQL 2014,
[Extensions tooAdventureWorks tooDemonstrate In-Memory OLTP]
(http://msdn.microsoft.com/library/dn511655&#x28;v=sql.120&#x29;.aspx)

whereas for SQL 2016+
[Sample Database for In-Memory OLTP]
(http://msdn.microsoft.com/library/mt465764.aspx)
-->



### <a name="run-hello-inmem-stress-workload-first"></a><span data-ttu-id="db629-266">Kör hello *_inmem* betonar arbetsbelastning först</span><span class="sxs-lookup"><span data-stu-id="db629-266">Run hello *_inmem* stress workload first</span></span>


<span data-ttu-id="db629-267">Du kan använda en *RML Cmd-Prompt* fönstret toorun våra ostress.exe-kommandoraden.</span><span class="sxs-lookup"><span data-stu-id="db629-267">You can use an *RML Cmd Prompt* window toorun our ostress.exe command line.</span></span> <span data-ttu-id="db629-268">hello kommandoradsparametrar direkt ostress till:</span><span class="sxs-lookup"><span data-stu-id="db629-268">hello command-line parameters direct ostress to:</span></span>

- <span data-ttu-id="db629-269">Kör 100 anslutningar samtidigt (-n100).</span><span class="sxs-lookup"><span data-stu-id="db629-269">Run 100 connections concurrently (-n100).</span></span>
- <span data-ttu-id="db629-270">Varje anslutning och kör hello T-SQL-skript 50 gånger (-r50).</span><span class="sxs-lookup"><span data-stu-id="db629-270">Have each connection run hello T-SQL script 50 times (-r50).</span></span>


```
ostress.exe -n100 -r50 -S<servername>.database.windows.net -U<login> -P<password> -d<database> -q -Q"DECLARE @i int = 0, @od SalesLT.SalesOrderDetailType_inmem, @SalesOrderID int, @DueDate datetime2 = sysdatetime(), @CustomerID int = rand() * 8000, @BillToAddressID int = rand() * 10000, @ShipToAddressID int = rand()* 10000; INSERT INTO @od SELECT OrderQty, ProductID FROM Demo.DemoSalesOrderDetailSeed WHERE OrderID= cast((rand()*60) as int); WHILE (@i < 20) begin; EXECUTE SalesLT.usp_InsertSalesOrder_inmem @SalesOrderID OUTPUT, @DueDate, @CustomerID, @BillToAddressID, @ShipToAddressID, @od; set @i += 1; end"
```


<span data-ttu-id="db629-271">toorun hello föregående ostress.exe kommandoraden:</span><span class="sxs-lookup"><span data-stu-id="db629-271">toorun hello preceding ostress.exe command line:</span></span>


1. <span data-ttu-id="db629-272">Återställ hello databasen datainnehåll genom att köra följande kommando i SSMS toodelete hello alla hello-data som infogats av alla tidigare körs:</span><span class="sxs-lookup"><span data-stu-id="db629-272">Reset hello database data content by running hello following command in SSMS, toodelete all hello data that was inserted by any previous runs:</span></span>

    ``` tsql
    EXECUTE Demo.usp_DemoReset;
    ```

2. <span data-ttu-id="db629-273">Kopiera hello texten i hello föregående ostress.exe kommandoraden tooyour Urklipp.</span><span class="sxs-lookup"><span data-stu-id="db629-273">Copy hello text of hello preceding ostress.exe command line tooyour clipboard.</span></span>

3. <span data-ttu-id="db629-274">Ersätt hello `<placeholders>` för hello parametrar -S - U -P -d med hello korrigera verkliga värden.</span><span class="sxs-lookup"><span data-stu-id="db629-274">Replace hello `<placeholders>` for hello parameters -S -U -P -d with hello correct real values.</span></span>

4. <span data-ttu-id="db629-275">Kör redigerade kommandoraden i ett RML Cmd-fönster.</span><span class="sxs-lookup"><span data-stu-id="db629-275">Run your edited command line in an RML Cmd window.</span></span>


#### <a name="result-is-a-duration"></a><span data-ttu-id="db629-276">Resultatet är en varaktighet</span><span class="sxs-lookup"><span data-stu-id="db629-276">Result is a duration</span></span>


<span data-ttu-id="db629-277">När ostress.exe är klar skriver hello kör varaktighet som den sista raden i utdata i hello RML kommandofönstret.</span><span class="sxs-lookup"><span data-stu-id="db629-277">When ostress.exe finishes, it writes hello run duration as its final line of output in hello RML Cmd window.</span></span> <span data-ttu-id="db629-278">Till exempel varade en kortare testkörning ungefär 1,5 minuter:</span><span class="sxs-lookup"><span data-stu-id="db629-278">For example, a shorter test run lasted about 1.5 minutes:</span></span>

`11/12/15 00:35:00.873 [0x000030A8] OSTRESS exiting normally, elapsed time: 00:01:31.867`


#### <a name="reset-edit-for-ondisk-then-rerun"></a><span data-ttu-id="db629-279">Återställ, redigera för *_ondisk*, kör sedan</span><span class="sxs-lookup"><span data-stu-id="db629-279">Reset, edit for *_ondisk*, then rerun</span></span>


<span data-ttu-id="db629-280">När du har hello resultatet från hello *_inmem* kör, utför följande steg för hello hello *_ondisk* kör:</span><span class="sxs-lookup"><span data-stu-id="db629-280">After you have hello result from hello *_inmem* run, perform hello following steps for hello *_ondisk* run:</span></span>


1. <span data-ttu-id="db629-281">Återställ hello databasen genom att köra följande kommando i SSMS toodelete hello alla hello-data som infogats av hello tidigare kör:</span><span class="sxs-lookup"><span data-stu-id="db629-281">Reset hello database by running hello following command in SSMS toodelete all hello data that was inserted by hello previous run:</span></span>
```
EXECUTE Demo.usp_DemoReset;
```

2. <span data-ttu-id="db629-282">Redigera hello ostress.exe kommandoraden tooreplace alla *_inmem* med *_ondisk*.</span><span class="sxs-lookup"><span data-stu-id="db629-282">Edit hello ostress.exe command line tooreplace all *_inmem* with *_ondisk*.</span></span>

3. <span data-ttu-id="db629-283">Kör ostress.exe för hello gång och avbilda hello varaktighet resultat.</span><span class="sxs-lookup"><span data-stu-id="db629-283">Rerun ostress.exe for hello second time, and capture hello duration result.</span></span>

4. <span data-ttu-id="db629-284">Igen och återställa hello-databasen (för ansvarsfullt om du tar bort kan vara en stor mängd testdata).</span><span class="sxs-lookup"><span data-stu-id="db629-284">Again, reset hello database (for responsibly deleting what can be a large amount of test data).</span></span>


#### <a name="expected-comparison-results"></a><span data-ttu-id="db629-285">Förväntade Jämförelseresultat</span><span class="sxs-lookup"><span data-stu-id="db629-285">Expected comparison results</span></span>

<span data-ttu-id="db629-286">Våra tester i minnet har visat att prestanda förbättras av **nio gånger** för den här simplistic arbetsbelastning med ostress körs på en Azure VM i hello samma Azure-region som hello-databas.</span><span class="sxs-lookup"><span data-stu-id="db629-286">Our In-Memory tests have shown that performance improved by **nine times** for this simplistic workload, with ostress running on an Azure VM in hello same Azure region as hello database.</span></span>

<a id="install_analytics_manuallink" name="install_analytics_manuallink"></a>

&nbsp;

## <a name="2-install-hello-in-memory-analytics-sample"></a><span data-ttu-id="db629-287">2. Installera hello Analytics InMemory-exempel</span><span class="sxs-lookup"><span data-stu-id="db629-287">2. Install hello In-Memory Analytics sample</span></span>


<span data-ttu-id="db629-288">I det här avsnittet kan jämföra du hello i/o och resultat när du använder ett columnstore-index jämfört med traditionell b-trädindex.</span><span class="sxs-lookup"><span data-stu-id="db629-288">In this section, you compare hello IO and statistics results when you're using a columnstore index versus a traditional b-tree index.</span></span>


<span data-ttu-id="db629-289">För analys i realtid på en OLTP-arbetsbelastning är det ofta bäst toouse ett icke-grupperat columnstore-index.</span><span class="sxs-lookup"><span data-stu-id="db629-289">For real-time analytics on an OLTP workload, it's often best toouse a nonclustered columnstore index.</span></span> <span data-ttu-id="db629-290">Mer information finns i [Columnstore-index beskrivs](http://msdn.microsoft.com/library/gg492088.aspx).</span><span class="sxs-lookup"><span data-stu-id="db629-290">For details, see [Columnstore Indexes Described](http://msdn.microsoft.com/library/gg492088.aspx).</span></span>



### <a name="prepare-hello-columnstore-analytics-test"></a><span data-ttu-id="db629-291">Förbereda hello columnstore analytics test</span><span class="sxs-lookup"><span data-stu-id="db629-291">Prepare hello columnstore analytics test</span></span>


1. <span data-ttu-id="db629-292">Använd hello Azure portal toocreate en ny AdventureWorksLT databas från hello exemplet.</span><span class="sxs-lookup"><span data-stu-id="db629-292">Use hello Azure portal toocreate a fresh AdventureWorksLT database from hello sample.</span></span>
 - <span data-ttu-id="db629-293">Använda exakt samma namn.</span><span class="sxs-lookup"><span data-stu-id="db629-293">Use that exact name.</span></span>
 - <span data-ttu-id="db629-294">Välj alla premiumnivån.</span><span class="sxs-lookup"><span data-stu-id="db629-294">Choose any Premium service tier.</span></span>

2. <span data-ttu-id="db629-295">Kopiera hello [sql_in memory_analytics_sample](https://raw.githubusercontent.com/Microsoft/sql-server-samples/master/samples/features/in-memory/t-sql-scripts/sql_in-memory_analytics_sample.sql) tooyour Urklipp.</span><span class="sxs-lookup"><span data-stu-id="db629-295">Copy hello [sql_in-memory_analytics_sample](https://raw.githubusercontent.com/Microsoft/sql-server-samples/master/samples/features/in-memory/t-sql-scripts/sql_in-memory_analytics_sample.sql) tooyour clipboard.</span></span>
 - <span data-ttu-id="db629-296">Hej T-SQL-skript skapar hello nödvändiga InMemory-objekt i hello AdventureWorksLT exempeldatabasen som du skapade i steg 1.</span><span class="sxs-lookup"><span data-stu-id="db629-296">hello T-SQL script creates hello necessary In-Memory objects in hello AdventureWorksLT sample database that you created in step 1.</span></span>
 - <span data-ttu-id="db629-297">hello skriptet skapar hälsningspaket dimensionstabellen och två faktatabeller.</span><span class="sxs-lookup"><span data-stu-id="db629-297">hello script creates hello Dimension table and two fact tables.</span></span> <span data-ttu-id="db629-298">hello faktatabeller fylls i med 3.5 miljoner rader.</span><span class="sxs-lookup"><span data-stu-id="db629-298">hello fact tables are populated with 3.5 million rows each.</span></span>
 - <span data-ttu-id="db629-299">hello skript kan ta 15 minuter toocomplete.</span><span class="sxs-lookup"><span data-stu-id="db629-299">hello script might take 15 minutes toocomplete.</span></span>

3. <span data-ttu-id="db629-300">Klistra in hello T-SQL-skript i SSMS och sedan köra hello skript.</span><span class="sxs-lookup"><span data-stu-id="db629-300">Paste hello T-SQL script into SSMS, and then execute hello script.</span></span> <span data-ttu-id="db629-301">Hej **COLUMNSTORE** nyckelord i hello **CREATE INDEX** -instruktionen är mycket viktigt, som:</span><span class="sxs-lookup"><span data-stu-id="db629-301">hello **COLUMNSTORE** keyword in hello **CREATE INDEX** statement is crucial, as in:</span></span><br/>`CREATE NONCLUSTERED COLUMNSTORE INDEX ...;`

4. <span data-ttu-id="db629-302">Ange AdventureWorksLT toocompatibility nivå 130:</span><span class="sxs-lookup"><span data-stu-id="db629-302">Set AdventureWorksLT toocompatibility level 130:</span></span><br/>`ALTER DATABASE AdventureworksLT SET compatibility_level = 130;`

    <span data-ttu-id="db629-303">Nivå 130 är inte direkt relaterat tooIn minnet funktioner.</span><span class="sxs-lookup"><span data-stu-id="db629-303">Level 130 is not directly related tooIn-Memory features.</span></span> <span data-ttu-id="db629-304">Men nivå 130 erbjuder bättre frågeprestanda än 120.</span><span class="sxs-lookup"><span data-stu-id="db629-304">But level 130 generally provides faster query performance than 120.</span></span>


#### <a name="key-tables-and-columnstore-indexes"></a><span data-ttu-id="db629-305">Viktiga tabeller och columnstore-index</span><span class="sxs-lookup"><span data-stu-id="db629-305">Key tables and columnstore indexes</span></span>


- <span data-ttu-id="db629-306">dbo. FactResellerSalesXL_CCI är en tabell som har ett grupperat columnstore-index som har avancerade komprimering vid hello *data* nivå.</span><span class="sxs-lookup"><span data-stu-id="db629-306">dbo.FactResellerSalesXL_CCI is a table that has a clustered columnstore index, which has advanced compression at hello *data* level.</span></span>

- <span data-ttu-id="db629-307">dbo. FactResellerSalesXL_PageCompressed är en tabell som har en motsvarande reguljära grupperat index, som komprimeras bara på hello *sidan* nivå.</span><span class="sxs-lookup"><span data-stu-id="db629-307">dbo.FactResellerSalesXL_PageCompressed is a table that has an equivalent regular clustered index, which is compressed only at hello *page* level.</span></span>


#### <a name="key-queries-toocompare-hello-columnstore-index"></a><span data-ttu-id="db629-308">Viktiga frågor toocompare hello columnstore-index</span><span class="sxs-lookup"><span data-stu-id="db629-308">Key queries toocompare hello columnstore index</span></span>


<span data-ttu-id="db629-309">Det finns [flera T-SQL-fråga typer som du kan köra](https://raw.githubusercontent.com/Microsoft/sql-server-samples/master/samples/features/in-memory/t-sql-scripts/clustered_columnstore_sample_queries.sql) toosee prestandaförbättringar.</span><span class="sxs-lookup"><span data-stu-id="db629-309">There are [several T-SQL query types that you can run](https://raw.githubusercontent.com/Microsoft/sql-server-samples/master/samples/features/in-memory/t-sql-scripts/clustered_columnstore_sample_queries.sql) toosee performance improvements.</span></span> <span data-ttu-id="db629-310">Betala uppmärksamhet toothis par frågor i steg 2 i hello T-SQL-skript.</span><span class="sxs-lookup"><span data-stu-id="db629-310">In step 2 in hello T-SQL script, pay attention toothis pair of queries.</span></span> <span data-ttu-id="db629-311">De skiljer sig bara på en rad:</span><span class="sxs-lookup"><span data-stu-id="db629-311">They differ only on one line:</span></span>


- `FROM FactResellerSalesXL_PageCompressed a`
- `FROM FactResellerSalesXL_CCI a`


<span data-ttu-id="db629-312">Ett grupperat columnstore-index är i hello FactResellerSalesXL\_CCI tabell.</span><span class="sxs-lookup"><span data-stu-id="db629-312">A clustered columnstore index is in hello FactResellerSalesXL\_CCI table.</span></span>

<span data-ttu-id="db629-313">hello följande T-SQL-skript utdrag skrivs ut statistik för i/o och tid för hello frågan för varje tabell.</span><span class="sxs-lookup"><span data-stu-id="db629-313">hello following T-SQL script excerpt prints statistics for IO and TIME for hello query of each table.</span></span>


```
/*********************************************************************
Step 2 -- Overview
-- Page Compressed BTree table v/s Columnstore table performance differences
-- Enable actual Query Plan in order toosee Plan differences when Executing
*/
-- Ensure Database is in 130 compatibility mode
ALTER DATABASE AdventureworksLT SET compatibility_level = 130
GO

-- Execute a typical query that joins hello Fact Table with dimension tables
-- Note this query will run on hello Page Compressed table, Note down hello time
SET STATISTICS IO ON
SET STATISTICS TIME ON
GO

SELECT c.Year
    ,e.ProductCategoryKey
    ,FirstName + ' ' + LastName AS FullName
    ,count(SalesOrderNumber) AS NumSales
    ,sum(SalesAmount) AS TotalSalesAmt
    ,Avg(SalesAmount) AS AvgSalesAmt
    ,count(DISTINCT SalesOrderNumber) AS NumOrders
    ,count(DISTINCT a.CustomerKey) AS CountCustomers
FROM FactResellerSalesXL_PageCompressed a
INNER JOIN DimProduct b ON b.ProductKey = a.ProductKey
INNER JOIN DimCustomer d ON d.CustomerKey = a.CustomerKey
Inner JOIN DimProductSubCategory e on e.ProductSubcategoryKey = b.ProductSubcategoryKey
INNER JOIN DimDate c ON c.DateKey = a.OrderDateKey
GROUP BY e.ProductCategoryKey,c.Year,d.CustomerKey,d.FirstName,d.LastName
GO
SET STATISTICS IO OFF
SET STATISTICS TIME OFF
GO


-- This is hello same Prior query on a table with a clustered columnstore index CCI
-- hello comparison numbers are even more dramatic hello larger hello table is (this is an 11 million row table only)
SET STATISTICS IO ON
SET STATISTICS TIME ON
GO
SELECT c.Year
    ,e.ProductCategoryKey
    ,FirstName + ' ' + LastName AS FullName
    ,count(SalesOrderNumber) AS NumSales
    ,sum(SalesAmount) AS TotalSalesAmt
    ,Avg(SalesAmount) AS AvgSalesAmt
    ,count(DISTINCT SalesOrderNumber) AS NumOrders
    ,count(DISTINCT a.CustomerKey) AS CountCustomers
FROM FactResellerSalesXL_CCI a
INNER JOIN DimProduct b ON b.ProductKey = a.ProductKey
INNER JOIN DimCustomer d ON d.CustomerKey = a.CustomerKey
Inner JOIN DimProductSubCategory e on e.ProductSubcategoryKey = b.ProductSubcategoryKey
INNER JOIN DimDate c ON c.DateKey = a.OrderDateKey
GROUP BY e.ProductCategoryKey,c.Year,d.CustomerKey,d.FirstName,d.LastName
GO

SET STATISTICS IO OFF
SET STATISTICS TIME OFF
GO
```

<span data-ttu-id="db629-314">Du kan förvänta dig om nio gånger hello prestandafördelar för den här frågan med hjälp av hello grupperade columnstore-indexet jämfört med traditionell hello-index i en databas med hello P2 prisnivå.</span><span class="sxs-lookup"><span data-stu-id="db629-314">In a database with hello P2 pricing tier, you can expect about nine times hello performance gain for this query by using hello clustered columnstore index compared with hello traditional index.</span></span> <span data-ttu-id="db629-315">Du kan förvänta dig ungefär 57 gånger hello prestandafördelar med P15, med hjälp av hello columnstore-index.</span><span class="sxs-lookup"><span data-stu-id="db629-315">With P15, you can expect about 57 times hello performance gain by using hello columnstore index.</span></span>



## <a name="next-steps"></a><span data-ttu-id="db629-316">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="db629-316">Next steps</span></span>

- [<span data-ttu-id="db629-317">Snabbstart 1: Minnesintern OLTP-teknik för snabbare T-SQL-prestanda</span><span class="sxs-lookup"><span data-stu-id="db629-317">Quick Start 1: In-Memory OLTP Technologies for Faster T-SQL Performance</span></span>](http://msdn.microsoft.com/library/mt694156.aspx)

- [<span data-ttu-id="db629-318">Använd InMemory-OLTP i ett befintligt Azure SQL-program</span><span class="sxs-lookup"><span data-stu-id="db629-318">Use In-Memory OLTP in an existing Azure SQL application</span></span>](sql-database-in-memory-oltp-migration.md)

- <span data-ttu-id="db629-319">[Övervakaren i minnet OLTP lagring](sql-database-in-memory-oltp-monitoring.md) för OLTP i minnet</span><span class="sxs-lookup"><span data-stu-id="db629-319">[Monitor In-Memory OLTP storage](sql-database-in-memory-oltp-monitoring.md) for In-Memory OLTP</span></span>


## <a name="additional-resources"></a><span data-ttu-id="db629-320">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="db629-320">Additional resources</span></span>

#### <a name="deeper-information"></a><span data-ttu-id="db629-321">Mer detaljerad information</span><span class="sxs-lookup"><span data-stu-id="db629-321">Deeper information</span></span>

- [<span data-ttu-id="db629-322">Lär dig hur kvorum fördubblar viktiga databasen arbetsbelastning och sänka DTU med 70% med InMemory-OLTP i SQL-databas</span><span class="sxs-lookup"><span data-stu-id="db629-322">Learn how Quorum doubles key database’s workload while lowering DTU by 70% with In-Memory OLTP in SQL Database</span></span>](https://customers.microsoft.com/story/quorum-doubles-key-databases-workload-while-lowering-dtu-with-sql-database)

- [<span data-ttu-id="db629-323">Minnesintern OLTP i Azure SQL Database blogginlägget</span><span class="sxs-lookup"><span data-stu-id="db629-323">In-Memory OLTP in Azure SQL Database Blog Post</span></span>](https://azure.microsoft.com/blog/in-memory-oltp-in-azure-sql-database/)

- [<span data-ttu-id="db629-324">Lär dig mer om OLTP i minnet</span><span class="sxs-lookup"><span data-stu-id="db629-324">Learn about In-Memory OLTP</span></span>](http://msdn.microsoft.com/library/dn133186.aspx)

- [<span data-ttu-id="db629-325">Lär dig mer om columnstore-index</span><span class="sxs-lookup"><span data-stu-id="db629-325">Learn about columnstore indexes</span></span>](https://msdn.microsoft.com/library/gg492088.aspx)

- [<span data-ttu-id="db629-326">Lär dig mer om drift analys i realtid</span><span class="sxs-lookup"><span data-stu-id="db629-326">Learn about real-time operational analytics</span></span>](http://msdn.microsoft.com/library/dn817827.aspx)

- <span data-ttu-id="db629-327">Se [vanliga arbetsbelastning mönster och överväganden vid migrering](http://msdn.microsoft.com/library/dn673538.aspx) (som beskriver arbetsbelastning mönster där Minnesintern OLTP ofta ger betydande prestandavinster)</span><span class="sxs-lookup"><span data-stu-id="db629-327">See [Common Workload Patterns and Migration Considerations](http://msdn.microsoft.com/library/dn673538.aspx) (which describes workload patterns where In-Memory OLTP commonly provides significant performance gains)</span></span>

#### <a name="application-design"></a><span data-ttu-id="db629-328">Programmet design</span><span class="sxs-lookup"><span data-stu-id="db629-328">Application design</span></span>

- [<span data-ttu-id="db629-329">OLTP (i minnet optimering) i minnet</span><span class="sxs-lookup"><span data-stu-id="db629-329">In-Memory OLTP (In-Memory Optimization)</span></span>](http://msdn.microsoft.com/library/dn133186.aspx)

- [<span data-ttu-id="db629-330">Använd InMemory-OLTP i ett befintligt Azure SQL-program</span><span class="sxs-lookup"><span data-stu-id="db629-330">Use In-Memory OLTP in an existing Azure SQL application</span></span>](sql-database-in-memory-oltp-migration.md)

#### <a name="tools"></a><span data-ttu-id="db629-331">Verktyg</span><span class="sxs-lookup"><span data-stu-id="db629-331">Tools</span></span>

- [<span data-ttu-id="db629-332">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="db629-332">Azure portal</span></span>](https://portal.azure.com/)

- [<span data-ttu-id="db629-333">SQL Server Management Studio (SSMS)</span><span class="sxs-lookup"><span data-stu-id="db629-333">SQL Server Management Studio (SSMS)</span></span>](https://msdn.microsoft.com/library/mt238290.aspx)

- [<span data-ttu-id="db629-334">SQL Server Data Tools (SSDT)</span><span class="sxs-lookup"><span data-stu-id="db629-334">SQL Server Data Tools (SSDT)</span></span>](http://msdn.microsoft.com/library/mt204009.aspx)
