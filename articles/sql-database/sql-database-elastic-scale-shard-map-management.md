---
title: aaaScale ut en Azure SQL database | Microsoft Docs
description: "Hur toouse hello ShardMapManager klientbibliotek för elastisk databas"
services: sql-database
documentationcenter: 
manager: jhubbard
author: ddove
editor: 
ms.assetid: 0e9d647a-9ba9-4875-aa22-662d01283439
ms.service: sql-database
ms.custom: scale out apps
ms.workload: sql-database
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/24/2016
ms.author: ddove
ms.openlocfilehash: 4cf670d42ad7ded98fb8d6f0830154587dd2c6f1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="scale-out-databases-with-hello-shard-map-manager"></a><span data-ttu-id="ca354-103">Skala ut databaser med hello Fragmentera kartan manager</span><span class="sxs-lookup"><span data-stu-id="ca354-103">Scale out databases with hello shard map manager</span></span>
<span data-ttu-id="ca354-104">tooeasily skala ut databaser i SQL Azure bör använda en Fragmentera kartan manager.</span><span class="sxs-lookup"><span data-stu-id="ca354-104">tooeasily scale out databases on SQL Azure, use a shard map manager.</span></span> <span data-ttu-id="ca354-105">hello Fragmentera kartan manager är en särskild databas som underhåller globala mappningsinformation om alla shards (databaser) i en Fragmentera.</span><span class="sxs-lookup"><span data-stu-id="ca354-105">hello shard map manager is a special database that maintains global mapping information about all shards (databases) in a shard set.</span></span> <span data-ttu-id="ca354-106">hello metadata kan en tooconnect toohello rätt databas baserat på hello värdet för hello **horisontell partitionering nyckeln**.</span><span class="sxs-lookup"><span data-stu-id="ca354-106">hello metadata allows an application tooconnect toohello correct database based upon hello value of hello **sharding key**.</span></span> <span data-ttu-id="ca354-107">Varje Fragmentera i hello uppsättningen innehåller dessutom maps som spårar hello lokala Fragmentera data (kallas även **shardlets**).</span><span class="sxs-lookup"><span data-stu-id="ca354-107">In addition, every shard in hello set contains maps that track hello local shard data (known as **shardlets**).</span></span> 

![Hantering av Fragmentera karta](./media/sql-database-elastic-scale-shard-map-management/glossary.png)

<span data-ttu-id="ca354-109">Förstå hur dessa mappningar skapas är viktigt tooshard kartan.</span><span class="sxs-lookup"><span data-stu-id="ca354-109">Understanding how these maps are constructed is essential tooshard map management.</span></span> <span data-ttu-id="ca354-110">Detta görs med hjälp av hello [ShardMapManager klassen](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.aspx)hittades i hello [klientbibliotek för elastisk databas](sql-database-elastic-database-client-library.md) toomanage Fragmentera mappar.</span><span class="sxs-lookup"><span data-stu-id="ca354-110">This is done using hello [ShardMapManager class](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.aspx), found in hello [Elastic Database client library](sql-database-elastic-database-client-library.md) toomanage shard maps.</span></span>  

## <a name="shard-maps-and-shard-mappings"></a><span data-ttu-id="ca354-111">Fragmentera maps och Fragmentera mappningar</span><span class="sxs-lookup"><span data-stu-id="ca354-111">Shard maps and shard mappings</span></span>
<span data-ttu-id="ca354-112">Du måste välja hello typ av Fragmentera kartan toocreate för varje Fragmentera.</span><span class="sxs-lookup"><span data-stu-id="ca354-112">For each shard, you must select hello type of shard map toocreate.</span></span> <span data-ttu-id="ca354-113">hello dig beror på hello database-arkitektur:</span><span class="sxs-lookup"><span data-stu-id="ca354-113">hello choice depends on hello database architecture:</span></span> 

1. <span data-ttu-id="ca354-114">En organisation per databas</span><span class="sxs-lookup"><span data-stu-id="ca354-114">Single tenant per database</span></span>  
2. <span data-ttu-id="ca354-115">Flera klienter per databas (två typer):</span><span class="sxs-lookup"><span data-stu-id="ca354-115">Multiple tenants per database (two types):</span></span>
   1. <span data-ttu-id="ca354-116">Lista över-mappning</span><span class="sxs-lookup"><span data-stu-id="ca354-116">List mapping</span></span>
   2. <span data-ttu-id="ca354-117">Mappning av intervallet</span><span class="sxs-lookup"><span data-stu-id="ca354-117">Range mapping</span></span>

<span data-ttu-id="ca354-118">En enskild klient-modell, skapa en **lista mappning** Fragmentera kartan.</span><span class="sxs-lookup"><span data-stu-id="ca354-118">For a single-tenant model, create a **list mapping** shard map.</span></span> <span data-ttu-id="ca354-119">modell för hello single-klienter tilldelas en databas per klient.</span><span class="sxs-lookup"><span data-stu-id="ca354-119">hello single-tenant model assigns one database per tenant.</span></span> <span data-ttu-id="ca354-120">Detta är en effektiv modell för SaaS-utvecklare som det förenklar hanteringen.</span><span class="sxs-lookup"><span data-stu-id="ca354-120">This is an effective model for SaaS developers as it simplifies management.</span></span>

![Lista över-mappning][1]

<span data-ttu-id="ca354-122">hello modell för flera klienter tilldelas flera innehavare tooa enskild databas (och du kan distribuera grupper av klienter över flera databaser).</span><span class="sxs-lookup"><span data-stu-id="ca354-122">hello multi-tenant model assigns several tenants tooa single database (and you can distribute groups of tenants across multiple databases).</span></span> <span data-ttu-id="ca354-123">Använd den här modellen när du förväntar dig varje klient toohave små databehov.</span><span class="sxs-lookup"><span data-stu-id="ca354-123">Use this model when you expect each tenant toohave small data needs.</span></span> <span data-ttu-id="ca354-124">I den här modellen vi tilldela ett intervall med klienter tooa en databas med hjälp av **intervallet mappning**.</span><span class="sxs-lookup"><span data-stu-id="ca354-124">In this model, we assign a range of tenants tooa database using **range mapping**.</span></span> 

![Mappning av intervallet][2]

<span data-ttu-id="ca354-126">Eller du kan implementera en databas för flera innehavare modellen med hjälp av en *lista mappning* tooassign flera innehavare tooa enskild databas.</span><span class="sxs-lookup"><span data-stu-id="ca354-126">Or you can implement a multi-tenant database model using a *list mapping* tooassign multiple tenants tooa single database.</span></span> <span data-ttu-id="ca354-127">Till exempel DB1 är används toostore information om klient-id 1 och 5 och DB2 lagrar data för klienten 7 och 10-klient.</span><span class="sxs-lookup"><span data-stu-id="ca354-127">For example, DB1 is used toostore information about tenant id 1 and 5, and DB2 stores data for tenant 7 and tenant 10.</span></span> 

![Muliple klienter i samma DB][3] 

### <a name="supported-net-types-for-sharding-keys"></a><span data-ttu-id="ca354-129">.Net-typer som stöds för horisontell partitionering nycklar</span><span class="sxs-lookup"><span data-stu-id="ca354-129">Supported .Net types for sharding keys</span></span>
<span data-ttu-id="ca354-130">Elastisk skala stöd hello efter .net Framework typer som horisontell partitionering nycklar:</span><span class="sxs-lookup"><span data-stu-id="ca354-130">Elastic Scale support hello following .Net Framework types as sharding keys:</span></span>

* <span data-ttu-id="ca354-131">heltal</span><span class="sxs-lookup"><span data-stu-id="ca354-131">integer</span></span>
* <span data-ttu-id="ca354-132">lång</span><span class="sxs-lookup"><span data-stu-id="ca354-132">long</span></span>
* <span data-ttu-id="ca354-133">GUID</span><span class="sxs-lookup"><span data-stu-id="ca354-133">guid</span></span>
* <span data-ttu-id="ca354-134">byte]</span><span class="sxs-lookup"><span data-stu-id="ca354-134">byte[]</span></span>  
* <span data-ttu-id="ca354-135">Datum och tid</span><span class="sxs-lookup"><span data-stu-id="ca354-135">datetime</span></span>
* <span data-ttu-id="ca354-136">TimeSpan</span><span class="sxs-lookup"><span data-stu-id="ca354-136">timespan</span></span>
* <span data-ttu-id="ca354-137">DateTimeOffset</span><span class="sxs-lookup"><span data-stu-id="ca354-137">datetimeoffset</span></span>

### <a name="list-and-range-shard-maps"></a><span data-ttu-id="ca354-138">Lista och intervallet Fragmentera maps</span><span class="sxs-lookup"><span data-stu-id="ca354-138">List and range shard maps</span></span>
<span data-ttu-id="ca354-139">Fragmentera maps kan konstrueras med **listor över enskilda horisontell partitionering nyckeln värden**, eller så kan de vara konstruerade med **intervallen för horisontell partitionering nyckeln värden**.</span><span class="sxs-lookup"><span data-stu-id="ca354-139">Shard maps can be constructed using **lists of individual sharding key values**, or they can be constructed using **ranges of sharding key values**.</span></span> 

### <a name="list-shard-maps"></a><span data-ttu-id="ca354-140">Lista Fragmentera maps</span><span class="sxs-lookup"><span data-stu-id="ca354-140">List shard maps</span></span>
<span data-ttu-id="ca354-141">**Shards** innehåller **shardlets** och hello mappningen av shardlets tooshards underhålls av en Fragmentera karta.</span><span class="sxs-lookup"><span data-stu-id="ca354-141">**Shards** contain **shardlets** and hello mapping of shardlets tooshards is maintained by a shard map.</span></span> <span data-ttu-id="ca354-142">En **lista Fragmentera kartan** är en association mellan hello enskilda nyckelvärden som identifierar hello shardlets och hello-databaser som fungerar som delar.</span><span class="sxs-lookup"><span data-stu-id="ca354-142">A **list shard map** is an association between hello individual key values that identify hello shardlets and hello databases that serve as shards.</span></span>  <span data-ttu-id="ca354-143">**Visa en lista över mappningar** är explicit och annan nyckelvärden kan vara mappade toohello samma databas.</span><span class="sxs-lookup"><span data-stu-id="ca354-143">**List mappings** are explicit and different key values can be mapped toohello same database.</span></span> <span data-ttu-id="ca354-144">Till exempel nyckel 1 mappar tooDatabase A och nyckelvärden 3 och 6 referera databasen B.</span><span class="sxs-lookup"><span data-stu-id="ca354-144">For example, key 1 maps tooDatabase A, and key values 3 and 6 both reference Database B.</span></span>

| <span data-ttu-id="ca354-145">Nyckel</span><span class="sxs-lookup"><span data-stu-id="ca354-145">Key</span></span> | <span data-ttu-id="ca354-146">Fragmentera plats</span><span class="sxs-lookup"><span data-stu-id="ca354-146">Shard Location</span></span> |
| --- | --- |
| <span data-ttu-id="ca354-147">1</span><span class="sxs-lookup"><span data-stu-id="ca354-147">1</span></span> |<span data-ttu-id="ca354-148">Database_A</span><span class="sxs-lookup"><span data-stu-id="ca354-148">Database_A</span></span> |
| <span data-ttu-id="ca354-149">3</span><span class="sxs-lookup"><span data-stu-id="ca354-149">3</span></span> |<span data-ttu-id="ca354-150">Database_B</span><span class="sxs-lookup"><span data-stu-id="ca354-150">Database_B</span></span> |
| <span data-ttu-id="ca354-151">4</span><span class="sxs-lookup"><span data-stu-id="ca354-151">4</span></span> |<span data-ttu-id="ca354-152">Database_C</span><span class="sxs-lookup"><span data-stu-id="ca354-152">Database_C</span></span> |
| <span data-ttu-id="ca354-153">6</span><span class="sxs-lookup"><span data-stu-id="ca354-153">6</span></span> |<span data-ttu-id="ca354-154">Database_B</span><span class="sxs-lookup"><span data-stu-id="ca354-154">Database_B</span></span> |
| <span data-ttu-id="ca354-155">...</span><span class="sxs-lookup"><span data-stu-id="ca354-155">...</span></span> |<span data-ttu-id="ca354-156">...</span><span class="sxs-lookup"><span data-stu-id="ca354-156">...</span></span> |

### <a name="range-shard-maps"></a><span data-ttu-id="ca354-157">Intervallet Fragmentera maps</span><span class="sxs-lookup"><span data-stu-id="ca354-157">Range shard maps</span></span>
<span data-ttu-id="ca354-158">I en **intervallet Fragmentera kartan**, hello viktiga intervallet anges med ett par **[lågt värde, högt värde)** där hello *låg värdet* är hello minsta nyckel i hello intervall och hello *Värdefulla* är hello första värde som är högre än hello intervall.</span><span class="sxs-lookup"><span data-stu-id="ca354-158">In a **range shard map**, hello key range is described by a pair **[Low Value, High Value)** where hello *Low Value* is hello minimum key in hello range, and hello *High Value* is hello first value higher than hello range.</span></span> 

<span data-ttu-id="ca354-159">Till exempel **[0, 100)** innefattar alla heltal större än eller lika med 0 och mindre än 100.</span><span class="sxs-lookup"><span data-stu-id="ca354-159">For example, **[0, 100)** includes all integers greater than or equal 0 and less than 100.</span></span> <span data-ttu-id="ca354-160">Observera att flera adressintervall kan punkt toohello samma databasen och osammanhängande adressintervall stöds (t.ex. [100,200) och [400,600) både punkt tooDatabase C i hello exemplet nedan.)</span><span class="sxs-lookup"><span data-stu-id="ca354-160">Note that multiple ranges can point toohello same database, and disjoint ranges are supported (e.g., [100,200) and [400,600) both point tooDatabase C in hello example below.)</span></span>

| <span data-ttu-id="ca354-161">Nyckel</span><span class="sxs-lookup"><span data-stu-id="ca354-161">Key</span></span> | <span data-ttu-id="ca354-162">Fragmentera plats</span><span class="sxs-lookup"><span data-stu-id="ca354-162">Shard Location</span></span> |
| --- | --- |
| <span data-ttu-id="ca354-163">[1,50)</span><span class="sxs-lookup"><span data-stu-id="ca354-163">[1,50)</span></span> |<span data-ttu-id="ca354-164">Database_A</span><span class="sxs-lookup"><span data-stu-id="ca354-164">Database_A</span></span> |
| <span data-ttu-id="ca354-165">[50,100)</span><span class="sxs-lookup"><span data-stu-id="ca354-165">[50,100)</span></span> |<span data-ttu-id="ca354-166">Database_B</span><span class="sxs-lookup"><span data-stu-id="ca354-166">Database_B</span></span> |
| <span data-ttu-id="ca354-167">[100,200)</span><span class="sxs-lookup"><span data-stu-id="ca354-167">[100,200)</span></span> |<span data-ttu-id="ca354-168">Database_C</span><span class="sxs-lookup"><span data-stu-id="ca354-168">Database_C</span></span> |
| <span data-ttu-id="ca354-169">[400,600)</span><span class="sxs-lookup"><span data-stu-id="ca354-169">[400,600)</span></span> |<span data-ttu-id="ca354-170">Database_C</span><span class="sxs-lookup"><span data-stu-id="ca354-170">Database_C</span></span> |
| <span data-ttu-id="ca354-171">...</span><span class="sxs-lookup"><span data-stu-id="ca354-171">...</span></span> |<span data-ttu-id="ca354-172">...</span><span class="sxs-lookup"><span data-stu-id="ca354-172">...</span></span> |

<span data-ttu-id="ca354-173">Hello-tabeller som visas ovan är ett grundläggande exempel på en **ShardMap** objekt.</span><span class="sxs-lookup"><span data-stu-id="ca354-173">Each of hello tables shown above is a conceptual example of a **ShardMap** object.</span></span> <span data-ttu-id="ca354-174">Varje rad är ett förenklat exempel på en enskild **PointMapping** (för hello listan Fragmentera kartan) eller **RangeMapping** (för hello intervallet Fragmentera kartan) objekt.</span><span class="sxs-lookup"><span data-stu-id="ca354-174">Each row is a simplified example of an individual **PointMapping** (for hello list shard map) or **RangeMapping** (for hello range shard map) object.</span></span>

## <a name="shard-map-manager"></a><span data-ttu-id="ca354-175">Fragmentera kartan manager</span><span class="sxs-lookup"><span data-stu-id="ca354-175">Shard map manager</span></span>
<span data-ttu-id="ca354-176">I hello klientbiblioteket är hello Fragmentera kartan manager en samling Fragmentera maps.</span><span class="sxs-lookup"><span data-stu-id="ca354-176">In hello client library, hello shard map  manager is a collection of shard maps.</span></span> <span data-ttu-id="ca354-177">Hej data som hanteras av en **ShardMapManager** instans sparas på tre platser:</span><span class="sxs-lookup"><span data-stu-id="ca354-177">hello data managed by a **ShardMapManager** instance is kept in three places:</span></span> 

1. <span data-ttu-id="ca354-178">**Globala Fragmentera karta (GSM)**: du anger en databas tooserve som hello lagringsplatsen för alla Fragmentera maps och mappningar.</span><span class="sxs-lookup"><span data-stu-id="ca354-178">**Global Shard Map (GSM)**: You specify a database tooserve as hello repository for all of its shard maps and mappings.</span></span> <span data-ttu-id="ca354-179">Särskilda tabeller och lagrade procedurer skapas automatiskt toomanage hello information.</span><span class="sxs-lookup"><span data-stu-id="ca354-179">Special tables and stored procedures are automatically created toomanage hello information.</span></span> <span data-ttu-id="ca354-180">Detta är vanligtvis en liten databas och lätt öppnas och ska inte användas för andra behov av programmet hello.</span><span class="sxs-lookup"><span data-stu-id="ca354-180">This is typically a small database and lightly accessed, and it should not be used for other needs of hello application.</span></span> <span data-ttu-id="ca354-181">hello tabeller är i ett särskilt schema med namnet **__ShardManagement**.</span><span class="sxs-lookup"><span data-stu-id="ca354-181">hello tables are in a special schema named **__ShardManagement**.</span></span> 
2. <span data-ttu-id="ca354-182">**Lokala Fragmentera karta (LSM)**: alla databaser som du anger en Fragmentera är toobe ändrade toocontain flera små tabeller och särskilda lagrade procedurer som innehåller och hantera Fragmentera kartan information specifik toothat Fragmentera.</span><span class="sxs-lookup"><span data-stu-id="ca354-182">**Local Shard Map (LSM)**: Every database that you specify toobe a shard is modified toocontain several small tables and special stored procedures that contain and manage shard map information specific toothat shard.</span></span> <span data-ttu-id="ca354-183">Den här informationen är redundant hello informationen i hello GSM och låter hello toovalidate cachelagras Fragmentera kartan programinformation utan att placera belastning på hello GSM; hello program använder hello LSM toodetermine om en cachelagrad mappning fortfarande är giltigt.</span><span class="sxs-lookup"><span data-stu-id="ca354-183">This information is redundant with hello information in hello GSM, and it allows hello application toovalidate cached shard map information without placing any load on hello GSM; hello application uses hello LSM toodetermine if a cached mapping is still valid.</span></span> <span data-ttu-id="ca354-184">hello tabeller motsvarande toohello LSM på varje Fragmentera finns också i hello schemat **__ShardManagement**.</span><span class="sxs-lookup"><span data-stu-id="ca354-184">hello tables corresponding toohello LSM on each shard are also in hello schema **__ShardManagement**.</span></span>
3. <span data-ttu-id="ca354-185">**Cacheminne**: varje instans åtkomst programmet till en **ShardMapManager** objekt upprätthåller en lokal minnescache av dess mappningar.</span><span class="sxs-lookup"><span data-stu-id="ca354-185">**Application cache**: Each application instance accessing a **ShardMapManager** object maintains a local in-memory cache of its mappings.</span></span> <span data-ttu-id="ca354-186">Lagrar den routningsinformation som nyligen har hämtats.</span><span class="sxs-lookup"><span data-stu-id="ca354-186">It stores routing information that has recently been retrieved.</span></span> 

## <a name="constructing-a-shardmapmanager"></a><span data-ttu-id="ca354-187">Hur du skapar en ShardMapManager</span><span class="sxs-lookup"><span data-stu-id="ca354-187">Constructing a ShardMapManager</span></span>
<span data-ttu-id="ca354-188">En **ShardMapManager** objektet har skapats med hjälp av en [factory](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerfactory.aspx) mönster.</span><span class="sxs-lookup"><span data-stu-id="ca354-188">A **ShardMapManager** object is constructed using a [factory](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerfactory.aspx) pattern.</span></span> <span data-ttu-id="ca354-189">Hej  **[ShardMapManagerFactory.GetSqlShardMapManager](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerfactory.getsqlshardmapmanager.aspx)**  metoden tar autentiseringsuppgifter (inklusive hello servernamnet och databasnamnet hålla hello GSM) i hello form av en  **ConnectionString** och returnerar en instans av en **ShardMapManager**.</span><span class="sxs-lookup"><span data-stu-id="ca354-189">hello **[ShardMapManagerFactory.GetSqlShardMapManager](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerfactory.getsqlshardmapmanager.aspx)** method takes credentials (including hello server name and database name holding hello GSM) in hello form of a **ConnectionString** and returns an instance of a **ShardMapManager**.</span></span>  

<span data-ttu-id="ca354-190">**Obs:** hello **ShardMapManager** instansieras bara en gång per app-domän, inom hello initieringen kod för ett program.</span><span class="sxs-lookup"><span data-stu-id="ca354-190">**Please Note:** hello **ShardMapManager** should be instantiated only once per app domain, within hello initialization code for an application.</span></span> <span data-ttu-id="ca354-191">Skapandet av ytterligare instanser av ShardMapManager i hello samma appdomain resulterar i ökad minne och CPU-användning av programmet hello.</span><span class="sxs-lookup"><span data-stu-id="ca354-191">Creation of additional instances of ShardMapManager in hello same appdomain, will result in increased memory and CPU utilization of hello application.</span></span> <span data-ttu-id="ca354-192">En **ShardMapManager** kan innehålla valfritt antal Fragmentera maps.</span><span class="sxs-lookup"><span data-stu-id="ca354-192">A **ShardMapManager** can contain any number of shard maps.</span></span> <span data-ttu-id="ca354-193">Även om en enda Fragmentera karta är tillräcklig för många program, finns det tillfällen när olika uppsättningar av databaser som används för olika schemat eller för unika ändamål. i sådana fall kan det vara bättre att ange flera Fragmentera maps.</span><span class="sxs-lookup"><span data-stu-id="ca354-193">While a single shard map may be sufficient for many applications, there are times when different sets of databases are used for different schema or for unique purposes; in those cases multiple shard maps may be preferable.</span></span> 

<span data-ttu-id="ca354-194">I den här koden, ett program försöker tooopen en befintlig **ShardMapManager** med hello [TryGetSqlShardMapManager metoden](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerfactory.trygetsqlshardmapmanager.aspx).</span><span class="sxs-lookup"><span data-stu-id="ca354-194">In this code, an application tries tooopen an existing **ShardMapManager** with hello [TryGetSqlShardMapManager method](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerfactory.trygetsqlshardmapmanager.aspx).</span></span>  <span data-ttu-id="ca354-195">Om objekt som representerar en Global **ShardMapManager** (GSM) inte ännu finnas inuti hello-databasen, hello klientbiblioteket skapar dem det med hjälp av hello [CreateSqlShardMapManager metoden](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerfactory.createsqlshardmapmanager.aspx).</span><span class="sxs-lookup"><span data-stu-id="ca354-195">If objects representing a Global **ShardMapManager** (GSM) do not yet exist inside hello database, hello client library creates them there using hello [CreateSqlShardMapManager method](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerfactory.createsqlshardmapmanager.aspx).</span></span>

    // Try tooget a reference toohello Shard Map Manager 
     // via hello Shard Map Manager database.  
    // If it doesn't already exist, then create it. 
    ShardMapManager shardMapManager; 
    bool shardMapManagerExists = ShardMapManagerFactory.TryGetSqlShardMapManager(
                                        connectionString, 
                                        ShardMapManagerLoadPolicy.Lazy, 
                                        out shardMapManager); 

    if (shardMapManagerExists) 
     { 
        Console.WriteLine("Shard Map Manager already exists");
    } 
    else
    {
        // Create hello Shard Map Manager. 
        ShardMapManagerFactory.CreateSqlShardMapManager(connectionString);
        Console.WriteLine("Created SqlShardMapManager"); 

        shardMapManager = ShardMapManagerFactory.GetSqlShardMapManager(
            connectionString, 
            ShardMapManagerLoadPolicy.Lazy);

        // hello connectionString contains server name, database name, and admin credentials 
        // for privileges on both hello GSM and hello shards themselves.
    } 

<span data-ttu-id="ca354-196">Alternativt kan använda du Powershell toocreate en ny Fragmentera kartan chef.</span><span class="sxs-lookup"><span data-stu-id="ca354-196">As an alternative, you can use Powershell toocreate a new Shard Map Manager.</span></span> <span data-ttu-id="ca354-197">Ett exempel är tillgänglig [här](https://gallery.technet.microsoft.com/scriptcenter/Azure-SQL-DB-Elastic-731883db).</span><span class="sxs-lookup"><span data-stu-id="ca354-197">An example is available [here](https://gallery.technet.microsoft.com/scriptcenter/Azure-SQL-DB-Elastic-731883db).</span></span>

## <a name="get-a-rangeshardmap-or-listshardmap"></a><span data-ttu-id="ca354-198">Hämta en RangeShardMap eller ListShardMap</span><span class="sxs-lookup"><span data-stu-id="ca354-198">Get a RangeShardMap or ListShardMap</span></span>
<span data-ttu-id="ca354-199">När du har skapat en Fragmentera kartan manager, du kan hämta hello [RangeShardMap](https://msdn.microsoft.com/library/azure/dn807318.aspx) eller [ListShardMap](https://msdn.microsoft.com/library/azure/dn807370.aspx) med hello [TryGetRangeShardMap](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.trygetrangeshardmap.aspx), hello [ TryGetListShardMap](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.trygetlistshardmap.aspx), eller hello [GetShardMap](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.getshardmap.aspx) metod.</span><span class="sxs-lookup"><span data-stu-id="ca354-199">After creating a shard map manager, you can get hello [RangeShardMap](https://msdn.microsoft.com/library/azure/dn807318.aspx) or [ListShardMap](https://msdn.microsoft.com/library/azure/dn807370.aspx) using hello [TryGetRangeShardMap](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.trygetrangeshardmap.aspx), hello [TryGetListShardMap](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.trygetlistshardmap.aspx), or hello [GetShardMap](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.getshardmap.aspx) method.</span></span>

    /// <summary>
    /// Creates a new Range Shard Map with hello specified name, or gets hello Range Shard Map if it already exists.
    /// </summary>
    public static RangeShardMap<T> CreateOrGetRangeShardMap<T>(ShardMapManager shardMapManager, string shardMapName)
    {
        // Try tooget a reference toohello Shard Map.
        RangeShardMap<T> shardMap;
        bool shardMapExists = shardMapManager.TryGetRangeShardMap(shardMapName, out shardMap);

        if (shardMapExists)
        {
            ConsoleUtils.WriteInfo("Shard Map {0} already exists", shardMap.Name);
        }
        else
        {
            // hello Shard Map does not exist, so create it
            shardMap = shardMapManager.CreateRangeShardMap<T>(shardMapName);
            ConsoleUtils.WriteInfo("Created Shard Map {0}", shardMap.Name);
        }

        return shardMap;
    } 

### <a name="shard-map-administration-credentials"></a><span data-ttu-id="ca354-200">Fragmentera kartan administration autentiseringsuppgifter</span><span class="sxs-lookup"><span data-stu-id="ca354-200">Shard map administration credentials</span></span>
<span data-ttu-id="ca354-201">Program som administrera och ändra Fragmentera maps skiljer sig från de som använder hello Fragmentera maps tooroute anslutningar.</span><span class="sxs-lookup"><span data-stu-id="ca354-201">Applications that administer and manipulate shard maps are different from those that use hello shard maps tooroute connections.</span></span> 

<span data-ttu-id="ca354-202">tooadminister Fragmentera mappar (lägga till eller ändra shards, Fragmentera kartor, Fragmentera mappningar osv) måste du initiera hello **ShardMapManager** med **autentiseringsuppgifter som har behörighet för båda hello GSM databasen och på varje för läsning och skrivning databasen som fungerar som en Fragmentera**.</span><span class="sxs-lookup"><span data-stu-id="ca354-202">tooadminister shard maps (add or change shards, shard maps, shard mappings, etc.) you must instantiate hello **ShardMapManager** using **credentials that have read/write privileges on both hello GSM database and on each database that serves as a shard**.</span></span> <span data-ttu-id="ca354-203">hello autentiseringsuppgifter måste tillåta skrivningar mot hello tabeller i båda hello GSM och LSM som Fragmentera kartan information anges eller ändras, samt som skapar tabeller för LSM på nya delar.</span><span class="sxs-lookup"><span data-stu-id="ca354-203">hello credentials must allow for writes against hello tables in both hello GSM and LSM as shard map information is entered or changed, as well as for creating LSM tables on new shards.</span></span>  

<span data-ttu-id="ca354-204">Se [autentiseringsuppgifter används tooaccess hello-klientbibliotek för elastisk databas](sql-database-elastic-scale-manage-credentials.md).</span><span class="sxs-lookup"><span data-stu-id="ca354-204">See [Credentials used tooaccess hello Elastic Database client library](sql-database-elastic-scale-manage-credentials.md).</span></span>

### <a name="only-metadata-affected"></a><span data-ttu-id="ca354-205">Endast de metadata som påverkas</span><span class="sxs-lookup"><span data-stu-id="ca354-205">Only metadata affected</span></span>
<span data-ttu-id="ca354-206">Metoder som används för att fylla eller ändra hello **ShardMapManager** data inte ändrar hello data som lagras i hello shards sig själva.</span><span class="sxs-lookup"><span data-stu-id="ca354-206">Methods used for populating or changing hello **ShardMapManager** data do not alter hello user data stored in hello shards themselves.</span></span> <span data-ttu-id="ca354-207">Till exempel metoder som **CreateShard**, **DeleteShard**, **UpdateMapping**osv påverkar hello Fragmentera kartan metadata.</span><span class="sxs-lookup"><span data-stu-id="ca354-207">For example, methods such as **CreateShard**, **DeleteShard**, **UpdateMapping**, etc. affect hello shard map metadata only.</span></span> <span data-ttu-id="ca354-208">Ta inte bort, lägga till eller ändra användardata i hello delar.</span><span class="sxs-lookup"><span data-stu-id="ca354-208">They do not remove, add, or alter user data contained in hello shards.</span></span> <span data-ttu-id="ca354-209">Dessa metoder är i stället utformad toobe används tillsammans med olika åtgärder du utföra toocreate eller ta bort faktiska databaser, eller att flytta rader från en Fragmentera tooanother toorebalance delat miljö.</span><span class="sxs-lookup"><span data-stu-id="ca354-209">Instead, these methods are designed toobe used in conjunction with separate operations you perform toocreate or remove actual databases, or that move rows from one shard tooanother toorebalance a sharded environment.</span></span>  <span data-ttu-id="ca354-210">(hello **delade dokument** verktyget som medföljer elastisk Databasverktyg gör användning av API: erna tillsammans med samordna faktiska dataflytten mellan shards.) Se [skalning hello elastisk databas dela sammanslagna verktyget](sql-database-elastic-scale-overview-split-and-merge.md).</span><span class="sxs-lookup"><span data-stu-id="ca354-210">(hello **split-merge** tool included with elastic database tools makes use of these APIs along with orchestrating actual data movement between shards.) See [Scaling using hello Elastic Database split-merge tool](sql-database-elastic-scale-overview-split-and-merge.md).</span></span>

## <a name="populating-a-shard-map-example"></a><span data-ttu-id="ca354-211">Fyller på en karta Fragmentera-exempel</span><span class="sxs-lookup"><span data-stu-id="ca354-211">Populating a shard map example</span></span>
<span data-ttu-id="ca354-212">En Exempelsekvens med åtgärder toopopulate en specifik Fragmentera karta visas nedan.</span><span class="sxs-lookup"><span data-stu-id="ca354-212">An example sequence of operations toopopulate a specific shard map is shown below.</span></span> <span data-ttu-id="ca354-213">hello koden utför de här stegen:</span><span class="sxs-lookup"><span data-stu-id="ca354-213">hello code performs these steps:</span></span> 

1. <span data-ttu-id="ca354-214">En ny Fragmentera karta skapas inom en Fragmentera kartan manager.</span><span class="sxs-lookup"><span data-stu-id="ca354-214">A new shard map is created within a shard map manager.</span></span> 
2. <span data-ttu-id="ca354-215">hello metadata för två olika delar läggs toohello Fragmentera kartan.</span><span class="sxs-lookup"><span data-stu-id="ca354-215">hello metadata for two different shards is added toohello shard map.</span></span> 
3. <span data-ttu-id="ca354-216">En mängd viktiga intervallet mappningar läggs och hello övergripande innehållet i hello Fragmentera karta visas.</span><span class="sxs-lookup"><span data-stu-id="ca354-216">A variety of key range mappings are added, and hello overall contents of hello shard map are displayed.</span></span> 

<span data-ttu-id="ca354-217">hello koden är skriven så att hello-metoden kan köras igen om ett fel inträffar.</span><span class="sxs-lookup"><span data-stu-id="ca354-217">hello code is written so that hello method can be rerun if an error occurs.</span></span> <span data-ttu-id="ca354-218">Varje begäran testar om en Fragmentera eller mappning finns redan, innan du försöker toocreate den.</span><span class="sxs-lookup"><span data-stu-id="ca354-218">Each request tests whether a shard or mapping already exists, before attempting toocreate it.</span></span> <span data-ttu-id="ca354-219">hello koden förutsätter att databaser med namnet **sample_shard_0**, **sample_shard_1** och **sample_shard_2** har redan skapats i hello-server som refereras av strängen **shardServer**.</span><span class="sxs-lookup"><span data-stu-id="ca354-219">hello code assumes that databases named **sample_shard_0**, **sample_shard_1** and **sample_shard_2** have already been created in hello server referenced by string **shardServer**.</span></span> 

    public void CreatePopulatedRangeMap(ShardMapManager smm, string mapName) 
        {            
            RangeShardMap<long> sm = null; 

            // check if shardmap exists and if not, create it 
            if (!smm.TryGetRangeShardMap(mapName, out sm)) 
            { 
                sm = smm.CreateRangeShardMap<long>(mapName); 
            } 

            Shard shard0 = null, shard1=null; 
            // Check if shard exists and if not, 
            // create it (Idempotent / tolerant of re-execute) 
            if (!sm.TryGetShard(new ShardLocation(
                                     shardServer, 
                                     "sample_shard_0"), 
                                     out shard0)) 
            { 
                Shard0 = sm.CreateShard(new ShardLocation(
                                            shardServer, 
                                            "sample_shard_0")); 
            } 

            if (!sm.TryGetShard(new ShardLocation(
                                    shardServer, 
                                    "sample_shard_1"), 
                                    out shard1)) 
            { 
                Shard1 = sm.CreateShard(new ShardLocation(
                                             shardServer, 
                                            "sample_shard_1"));  
            } 

            RangeMapping<long> rmpg=null; 

            // Check if mapping exists and if not,
            // create it (Idempotent / tolerant of re-execute) 
            if (!sm.TryGetMappingForKey(0, out rmpg)) 
            { 
                sm.CreateRangeMapping(
                          new RangeMappingCreationInfo<long>
                          (new Range<long>(0, 50), 
                          shard0, 
                          MappingStatus.Online)); 
            } 

            if (!sm.TryGetMappingForKey(50, out rmpg)) 
            { 
                sm.CreateRangeMapping(
                         new RangeMappingCreationInfo<long> 
                         (new Range<long>(50, 100), 
                         shard1, 
                         MappingStatus.Online)); 
            } 

            if (!sm.TryGetMappingForKey(100, out rmpg)) 
            { 
                sm.CreateRangeMapping(
                         new RangeMappingCreationInfo<long>
                         (new Range<long>(100, 150), 
                         shard0, 
                         MappingStatus.Online)); 
            } 

            if (!sm.TryGetMappingForKey(150, out rmpg)) 
            { 
                sm.CreateRangeMapping(
                         new RangeMappingCreationInfo<long> 
                         (new Range<long>(150, 200), 
                         shard1, 
                         MappingStatus.Online)); 
            } 

            if (!sm.TryGetMappingForKey(200, out rmpg)) 
            { 
               sm.CreateRangeMapping(
                         new RangeMappingCreationInfo<long> 
                         (new Range<long>(200, 300), 
                         shard0, 
                         MappingStatus.Online)); 
            } 

            // List hello shards and mappings 
            foreach (Shard s in sm.GetShards()
                         .OrderBy(s => s.Location.DataSource)
                         .ThenBy(s => s.Location.Database))
            { 
               Console.WriteLine("shard: "+ s.Location); 
            } 

            foreach (RangeMapping<long> rm in sm.GetMappings()) 
            { 
                Console.WriteLine("range: [" + rm.Value.Low.ToString() + ":" 
                        + rm.Value.High.ToString()+ ")  ==>" +rm.Shard.Location); 
            } 
        } 

<span data-ttu-id="ca354-220">Som ett alternativ som du kan använda PowerShell-skript tooachieve hello resultat samma.</span><span class="sxs-lookup"><span data-stu-id="ca354-220">As an alternative you can use PowerShell scripts tooachieve hello same result.</span></span> <span data-ttu-id="ca354-221">Vissa hello exempel PowerShell-exemplen finns [här](https://gallery.technet.microsoft.com/scriptcenter/Azure-SQL-DB-Elastic-731883db).</span><span class="sxs-lookup"><span data-stu-id="ca354-221">Some of hello sample PowerShell examples are available [here](https://gallery.technet.microsoft.com/scriptcenter/Azure-SQL-DB-Elastic-731883db).</span></span>     

<span data-ttu-id="ca354-222">När Fragmentera maps är ifyllda, data access-program kan skapas eller anpassade toowork med hello maps.</span><span class="sxs-lookup"><span data-stu-id="ca354-222">Once shard maps have been populated, data access applications can be created or adapted toowork with hello maps.</span></span> <span data-ttu-id="ca354-223">Fylla eller ändra hello maps behöver inte uppstår igen förrän **kartan layout** måste toochange.</span><span class="sxs-lookup"><span data-stu-id="ca354-223">Populating or manipulating hello maps need not occur again until **map layout** needs toochange.</span></span>  

## <a name="data-dependent-routing"></a><span data-ttu-id="ca354-224">Databeroende routning</span><span class="sxs-lookup"><span data-stu-id="ca354-224">Data dependent routing</span></span>
<span data-ttu-id="ca354-225">hello Fragmentera kartan manager används mest i program som kräver anslutningar tooperform hello app-specifik data databasåtgärder.</span><span class="sxs-lookup"><span data-stu-id="ca354-225">hello shard map manager will be most used in applications that require database connections tooperform hello app-specific data operations.</span></span> <span data-ttu-id="ca354-226">Dessa anslutningar måste vara kopplad till hello rätt databas.</span><span class="sxs-lookup"><span data-stu-id="ca354-226">Those connections must be associated with hello correct database.</span></span> <span data-ttu-id="ca354-227">Detta kallas **Data beroende routning**.</span><span class="sxs-lookup"><span data-stu-id="ca354-227">This is known as **Data Dependent Routing**.</span></span> <span data-ttu-id="ca354-228">Initiera en Fragmentera kartan manager objekt från hello factory med hjälp av autentiseringsuppgifter som har skrivskyddad åtkomst på hello GSM databas för dessa program.</span><span class="sxs-lookup"><span data-stu-id="ca354-228">For these applications, instantiate a shard map manager object from hello factory using credentials that have read-only access on hello GSM database.</span></span> <span data-ttu-id="ca354-229">Enskilda förfrågningar för senare anslutningar ange autentiseringsuppgifter som krävs för att ansluta toohello lämplig Fragmentera databasen.</span><span class="sxs-lookup"><span data-stu-id="ca354-229">Individual requests for later connections supply credentials necessary for connecting toohello appropriate shard database.</span></span>

<span data-ttu-id="ca354-230">Observera att dessa program (med hjälp av **ShardMapManager** öppnas med skrivskyddad autentiseringsuppgifter) kan inte göra ändringar toohello maps eller mappningar.</span><span class="sxs-lookup"><span data-stu-id="ca354-230">Note that these applications (using **ShardMapManager** opened with read-only credentials) cannot make changes toohello maps or mappings.</span></span> <span data-ttu-id="ca354-231">Skapa administrativa specifika program eller PowerShell-skript som anger högre Privilegierade autentiseringsuppgifter som tidigare diskuterats för dessa behov.</span><span class="sxs-lookup"><span data-stu-id="ca354-231">For those needs, create administrative-specific applications or PowerShell scripts that supply higher-privileged credentials as discussed earlier.</span></span> <span data-ttu-id="ca354-232">Se [autentiseringsuppgifter används tooaccess hello-klientbibliotek för elastisk databas](sql-database-elastic-scale-manage-credentials.md).</span><span class="sxs-lookup"><span data-stu-id="ca354-232">See [Credentials used tooaccess hello Elastic Database client library](sql-database-elastic-scale-manage-credentials.md).</span></span>

<span data-ttu-id="ca354-233">Mer information finns i [Data beroende routning](sql-database-elastic-scale-data-dependent-routing.md).</span><span class="sxs-lookup"><span data-stu-id="ca354-233">For more details, see [Data dependent routing](sql-database-elastic-scale-data-dependent-routing.md).</span></span> 

## <a name="modifying-a-shard-map"></a><span data-ttu-id="ca354-234">Ändra en Fragmentera karta</span><span class="sxs-lookup"><span data-stu-id="ca354-234">Modifying a shard map</span></span>
<span data-ttu-id="ca354-235">En Fragmentera karta kan ändras på olika sätt.</span><span class="sxs-lookup"><span data-stu-id="ca354-235">A shard map can be changed in different ways.</span></span> <span data-ttu-id="ca354-236">Alla följande metoder hello ändra hello metadata som beskriver hello delar och deras mappningar, men de kan inte ändra data i hello shards fysiskt eller gör de skapa eller ta bort hello faktiska databaser.</span><span class="sxs-lookup"><span data-stu-id="ca354-236">All of hello following methods modify hello metadata describing hello shards and their mappings, but they do not physically modify data within hello shards, nor do they create or delete hello actual databases.</span></span>  <span data-ttu-id="ca354-237">Vissa av hello åtgärder på hello Fragmentera karta som beskrivs nedan behöva toobe samordnas med administrativa åtgärder som fysiskt flytta data eller att lägga till och ta bort databaser som fungerar som delar.</span><span class="sxs-lookup"><span data-stu-id="ca354-237">Some of hello operations on hello shard map described below may need toobe coordinated with administrative actions that physically move data or that add and remove databases serving as shards.</span></span>

<span data-ttu-id="ca354-238">Metoderna tillsammans fungerar som hello byggblock görs tillgängliga för att ändra hello övergripande fördelning av data i din databasmiljö för delat.</span><span class="sxs-lookup"><span data-stu-id="ca354-238">These methods work together as hello building blocks available for modifying hello overall distribution of data in your sharded database environment.</span></span>  

* <span data-ttu-id="ca354-239">tooadd eller ta bort delar: använda  **[CreateShard](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmap.createshard.aspx)**  och  **[DeleteShard](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmap.deleteshard.aspx)**  av hello [Shardmap klassen](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmap.aspx).</span><span class="sxs-lookup"><span data-stu-id="ca354-239">tooadd or remove shards: use **[CreateShard](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmap.createshard.aspx)** and **[DeleteShard](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmap.deleteshard.aspx)** of hello [Shardmap class](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmap.aspx).</span></span> 
  
    <span data-ttu-id="ca354-240">hello måste server och databas som representerar hello mål Fragmentera finnas för dessa åtgärder tooexecute.</span><span class="sxs-lookup"><span data-stu-id="ca354-240">hello server and database representing hello target shard must already exist for these operations tooexecute.</span></span> <span data-ttu-id="ca354-241">Dessa metoder har inte någon effekt på hello själva databaserna, endast för metadata i hello Fragmentera kartan.</span><span class="sxs-lookup"><span data-stu-id="ca354-241">These methods do not have any impact on hello databases themselves, only on metadata in hello shard map.</span></span>
* <span data-ttu-id="ca354-242">toocreate eller ta bort punkter eller intervall som är mappad toohello shards: Använd  **[CreateRangeMapping](https://msdn.microsoft.com/library/azure/dn841993.aspx)**,  **[DeleteMapping](https://msdn.microsoft.com/library/azure/dn824200.aspx)**  av hello [RangeShardMapping klassen](https://msdn.microsoft.com/library/azure/dn807318.aspx), och  **[CreatePointMapping](https://msdn.microsoft.com/library/azure/dn807218.aspx)**  av hello [ListShardMap](https://msdn.microsoft.com/library/azure/dn842123.aspx)</span><span class="sxs-lookup"><span data-stu-id="ca354-242">toocreate or remove points or ranges that are mapped toohello shards: use **[CreateRangeMapping](https://msdn.microsoft.com/library/azure/dn841993.aspx)**, **[DeleteMapping](https://msdn.microsoft.com/library/azure/dn824200.aspx)** of hello [RangeShardMapping class](https://msdn.microsoft.com/library/azure/dn807318.aspx), and **[CreatePointMapping](https://msdn.microsoft.com/library/azure/dn807218.aspx)** of hello [ListShardMap](https://msdn.microsoft.com/library/azure/dn842123.aspx)</span></span>
  
    <span data-ttu-id="ca354-243">Många olika punkter eller intervall kan vara mappade toohello samma Fragmentera.</span><span class="sxs-lookup"><span data-stu-id="ca354-243">Many different points or ranges can be mapped toohello same shard.</span></span> <span data-ttu-id="ca354-244">Dessa metoder påverkar endast metadata - de påverkar inte data som kanske redan finns i shards.</span><span class="sxs-lookup"><span data-stu-id="ca354-244">These methods only affect metadata - they do not affect any data that may already be present in shards.</span></span> <span data-ttu-id="ca354-245">Om data måste toobe tas bort från databasen hello i ordning toobe konsekvent med **DeleteMapping** åtgärder, behöver du tooperform dessa åtgärder separat men tillsammans med hjälp av dessa metoder.</span><span class="sxs-lookup"><span data-stu-id="ca354-245">If data needs toobe removed from hello database in order toobe consistent with **DeleteMapping** operations, you will need tooperform those operations separately but in conjunction with using these methods.</span></span>  
* <span data-ttu-id="ca354-246">toosplit befintliga områden i två eller merge intilliggande adressintervall till ett: använda  **[SplitMapping](https://msdn.microsoft.com/library/azure/dn824205.aspx)**  och  **[MergeMappings](https://msdn.microsoft.com/library/azure/dn824201.aspx)**.</span><span class="sxs-lookup"><span data-stu-id="ca354-246">toosplit existing ranges into two, or merge adjacent ranges into one: use **[SplitMapping](https://msdn.microsoft.com/library/azure/dn824205.aspx)** and **[MergeMappings](https://msdn.microsoft.com/library/azure/dn824201.aspx)**.</span></span>  
  
    <span data-ttu-id="ca354-247">Observera att dela och slå samman operations **inte ändra hello Fragmentera toowhich nyckel värden mappas**.</span><span class="sxs-lookup"><span data-stu-id="ca354-247">Note that split and merge operations **do not change hello shard toowhich key values are mapped**.</span></span> <span data-ttu-id="ca354-248">En delning delar ett befintligt intervall i två delar, men lämnar både som mappade toohello samma Fragmentera.</span><span class="sxs-lookup"><span data-stu-id="ca354-248">A split breaks an existing range into two parts, but leaves both as mapped toohello same shard.</span></span> <span data-ttu-id="ca354-249">En koppling fungerar på två angränsande områden som är redan mappad toohello samma fragment, mottagarsidan dem till ett område.</span><span class="sxs-lookup"><span data-stu-id="ca354-249">A merge operates on two adjacent ranges that are already mapped toohello same shard, coalescing them into a single range.</span></span>  <span data-ttu-id="ca354-250">hello flödet av punkter eller intervall själva mellan shards måste toobe samordnas med hjälp av **UpdateMapping** tillsammans med faktiska dataflytten.</span><span class="sxs-lookup"><span data-stu-id="ca354-250">hello movement of points or ranges themselves between shards needs toobe coordinated by using **UpdateMapping** in conjunction with actual data movement.</span></span>  <span data-ttu-id="ca354-251">Du kan använda hello **dela/Merge** tjänst som är en del av elastisk databas verktygen toocoordinate Fragmentera kartan ändringar med dataflyttning flytt krävs.</span><span class="sxs-lookup"><span data-stu-id="ca354-251">You can use hello **Split/Merge** service that is part of elastic database tools toocoordinate shard map changes with data movement, when movement is needed.</span></span> 
* <span data-ttu-id="ca354-252">toore karta (eller flytta) enskilda punkter eller intervall toodifferent shards: Använd  **[UpdateMapping](https://msdn.microsoft.com/library/azure/dn824207.aspx)**.</span><span class="sxs-lookup"><span data-stu-id="ca354-252">toore-map (or move) individual points or ranges toodifferent shards: use **[UpdateMapping](https://msdn.microsoft.com/library/azure/dn824207.aspx)**.</span></span>  
  
    <span data-ttu-id="ca354-253">Eftersom data kanske måste flyttas från en Fragmentera tooanother i ordning toobe konsekvent med toobe **UpdateMapping** åtgärder, behöver du tooperform som flytt separat men tillsammans med hjälp av dessa metoder.</span><span class="sxs-lookup"><span data-stu-id="ca354-253">Since data may need toobe moved from one shard tooanother in order toobe consistent with **UpdateMapping** operations, you will need tooperform that movement separately but in conjunction with using these methods.</span></span>
* <span data-ttu-id="ca354-254">tootake mappningar online och offline: använda  **[MarkMappingOffline](https://msdn.microsoft.com/library/azure/dn824202.aspx)**  och  **[MarkMappingOnline](https://msdn.microsoft.com/library/azure/dn807225.aspx)**  toocontrol hello online tillståndet för en mappning.</span><span class="sxs-lookup"><span data-stu-id="ca354-254">tootake mappings online and offline: use **[MarkMappingOffline](https://msdn.microsoft.com/library/azure/dn824202.aspx)** and **[MarkMappingOnline](https://msdn.microsoft.com/library/azure/dn807225.aspx)** toocontrol hello online state of a mapping.</span></span> 
  
    <span data-ttu-id="ca354-255">Vissa åtgärder på Fragmentera mappningar tillåts endast när en mappning är i tillståndet ”offline” inklusive **UpdateMapping** och **DeleteMapping**.</span><span class="sxs-lookup"><span data-stu-id="ca354-255">Certain operations on shard mappings are only allowed when a mapping is in an “offline” state, including **UpdateMapping** and **DeleteMapping**.</span></span> <span data-ttu-id="ca354-256">När en mappning är offline, returneras ett fel en data-beroende begäran baserat på en nyckel som ingår i mappningen.</span><span class="sxs-lookup"><span data-stu-id="ca354-256">When a mapping is offline, a data-dependent request based on a key included in that mapping will return an error.</span></span> <span data-ttu-id="ca354-257">När ett intervall först kopplas från avslutats dessutom alla anslutningar toohello påverkas Fragmentera automatiskt i ordning tooprevent inkonsekventa eller ofullständiga resultat för frågor som riktar sig mot områden som ändras.</span><span class="sxs-lookup"><span data-stu-id="ca354-257">In addition, when a range is first taken offline, all connections toohello affected shard are automatically killed in order tooprevent inconsistent or incomplete results for queries directed against ranges being changed.</span></span> 

<span data-ttu-id="ca354-258">Mappningar är oföränderliga objekt i .net.</span><span class="sxs-lookup"><span data-stu-id="ca354-258">Mappings are immutable objects in .Net.</span></span>  <span data-ttu-id="ca354-259">Alla hello metoder ovan ändrar mappningar ogiltig även eventuella referenser toothem i koden.</span><span class="sxs-lookup"><span data-stu-id="ca354-259">All of hello methods above that change mappings also invalidate any references toothem in your code.</span></span> <span data-ttu-id="ca354-260">toomake it enklare tooperform sekvenser av åtgärder som ändrar tillstånd för en mappning, alla hello-metoder som ändrar en mappning returnera en ny mappning referens, så kan vara härledda åtgärder.</span><span class="sxs-lookup"><span data-stu-id="ca354-260">toomake it easier tooperform sequences of operations that change a mapping’s state, all of hello methods that change a mapping return a new mapping reference, so operations can be chained.</span></span> <span data-ttu-id="ca354-261">Till exempel toodelete en befintlig mappning i shardmap sm som innehåller hello nyckeln 25, kan du köra hello följande:</span><span class="sxs-lookup"><span data-stu-id="ca354-261">For example, toodelete an existing mapping in shardmap sm that contains hello key 25, you can execute hello following:</span></span> 

        sm.DeleteMapping(sm.MarkMappingOffline(sm.GetMappingForKey(25)));

## <a name="adding-a-shard"></a><span data-ttu-id="ca354-262">Lägga till en Fragmentera</span><span class="sxs-lookup"><span data-stu-id="ca354-262">Adding a shard</span></span>
<span data-ttu-id="ca354-263">Program ofta behöver toosimply lägga till nya shards toohandle data som förväntas av nya nycklar eller viktiga områden för en Fragmentera karta som redan finns.</span><span class="sxs-lookup"><span data-stu-id="ca354-263">Applications often need toosimply add new shards toohandle data that is expected from new keys or key ranges, for a shard map that already exists.</span></span> <span data-ttu-id="ca354-264">Till exempel ett delat program genom att klient-ID kan behöva tooprovision nya Fragmentera för en ny klient eller varje månad delat data måste en ny Fragmentera etableras innan hello början av varje ny månad.</span><span class="sxs-lookup"><span data-stu-id="ca354-264">For example, an application sharded by Tenant ID may need tooprovision a new shard for a new tenant, or data sharded monthly may need a new shard provisioned before hello start of each new month.</span></span> 

<span data-ttu-id="ca354-265">Om hello nytt intervall nyckel inte är en del av en befintlig mappning och inga data flyttas krävs, är det mycket enkelt tooadd hello nya Fragmentera och associera hello ny nyckel eller intervallet toothat Fragmentera.</span><span class="sxs-lookup"><span data-stu-id="ca354-265">If hello new range of key values is not already part of an existing mapping and no data movement is necessary, it is very simple tooadd hello new shard and associate hello new key or range toothat shard.</span></span> <span data-ttu-id="ca354-266">Mer information om att lägga till nya shards finns [att lägga till en ny Fragmentera](sql-database-elastic-scale-add-a-shard.md).</span><span class="sxs-lookup"><span data-stu-id="ca354-266">For details on adding new shards, see [Adding a new shard](sql-database-elastic-scale-add-a-shard.md).</span></span>

<span data-ttu-id="ca354-267">För scenarier som kräver dataflyttning är hello dela merge tool dock nödvändiga tooorchestrate hello dataförflyttning mellan shards i kombination med hello nödvändiga Fragmentera kartan uppdateringar.</span><span class="sxs-lookup"><span data-stu-id="ca354-267">For scenarios that require data movement, however, hello split-merge tool is needed tooorchestrate hello data movement between shards in combination with hello necessary shard map updates.</span></span> <span data-ttu-id="ca354-268">Information om hur du använder hello yool dela dokument, finns [översikt över delade dokument](sql-database-elastic-scale-overview-split-and-merge.md)</span><span class="sxs-lookup"><span data-stu-id="ca354-268">For details on using hello split-merge yool, see [Overview of split-merge](sql-database-elastic-scale-overview-split-and-merge.md)</span></span> 

[!INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Image references-->
[1]: ./media/sql-database-elastic-scale-shard-map-management/listmapping.png
[2]: ./media/sql-database-elastic-scale-shard-map-management/rangemapping.png
[3]: ./media/sql-database-elastic-scale-shard-map-management/multipleonsingledb.png
