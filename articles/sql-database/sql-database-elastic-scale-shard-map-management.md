---
title: Skala ut en Azure SQL database | Microsoft Docs
description: "Hur du använder ShardMapManager, klientbibliotek för elastisk databas"
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
ms.openlocfilehash: f626cf417d8b3f1761f3c900d49039b3ff83b093
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="scale-out-databases-with-the-shard-map-manager"></a><span data-ttu-id="a368c-103">Skala ut databaser med hanteraren Fragmentera karta</span><span class="sxs-lookup"><span data-stu-id="a368c-103">Scale out databases with the shard map manager</span></span>
<span data-ttu-id="a368c-104">Använda en Fragmentera kartan manager för att enkelt skala ut databaser i SQL Azure.</span><span class="sxs-lookup"><span data-stu-id="a368c-104">To easily scale out databases on SQL Azure, use a shard map manager.</span></span> <span data-ttu-id="a368c-105">Fragmentera kartan manager är en särskild databas som underhåller globala mappningsinformation om alla shards (databaser) i en Fragmentera.</span><span class="sxs-lookup"><span data-stu-id="a368c-105">The shard map manager is a special database that maintains global mapping information about all shards (databases) in a shard set.</span></span> <span data-ttu-id="a368c-106">Metadata kan programmet att ansluta till rätt databas baserat på värdet för den **horisontell partitionering nyckeln**.</span><span class="sxs-lookup"><span data-stu-id="a368c-106">The metadata allows an application to connect to the correct database based upon the value of the **sharding key**.</span></span> <span data-ttu-id="a368c-107">Dessutom kan varje Fragmentera i uppsättningen innehåller mappningar som spårar lokala Fragmentera data (kallas även **shardlets**).</span><span class="sxs-lookup"><span data-stu-id="a368c-107">In addition, every shard in the set contains maps that track the local shard data (known as **shardlets**).</span></span> 

![Hantering av Fragmentera karta](./media/sql-database-elastic-scale-shard-map-management/glossary.png)

<span data-ttu-id="a368c-109">Det är viktigt att fragmentera kartan management förstå hur dessa mappningar skapas.</span><span class="sxs-lookup"><span data-stu-id="a368c-109">Understanding how these maps are constructed is essential to shard map management.</span></span> <span data-ttu-id="a368c-110">Detta görs med hjälp av den [ShardMapManager klassen](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.aspx)hittades i den [klientbibliotek för elastisk databas](sql-database-elastic-database-client-library.md) att hantera Fragmentera maps.</span><span class="sxs-lookup"><span data-stu-id="a368c-110">This is done using the [ShardMapManager class](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.aspx), found in the [Elastic Database client library](sql-database-elastic-database-client-library.md) to manage shard maps.</span></span>  

## <a name="shard-maps-and-shard-mappings"></a><span data-ttu-id="a368c-111">Fragmentera maps och Fragmentera mappningar</span><span class="sxs-lookup"><span data-stu-id="a368c-111">Shard maps and shard mappings</span></span>
<span data-ttu-id="a368c-112">Du måste välja vilken typ av Fragmentera kartan för att skapa för varje Fragmentera.</span><span class="sxs-lookup"><span data-stu-id="a368c-112">For each shard, you must select the type of shard map to create.</span></span> <span data-ttu-id="a368c-113">Valet är beroende av databasens arkitektur:</span><span class="sxs-lookup"><span data-stu-id="a368c-113">The choice depends on the database architecture:</span></span> 

1. <span data-ttu-id="a368c-114">En organisation per databas</span><span class="sxs-lookup"><span data-stu-id="a368c-114">Single tenant per database</span></span>  
2. <span data-ttu-id="a368c-115">Flera klienter per databas (två typer):</span><span class="sxs-lookup"><span data-stu-id="a368c-115">Multiple tenants per database (two types):</span></span>
   1. <span data-ttu-id="a368c-116">Lista över-mappning</span><span class="sxs-lookup"><span data-stu-id="a368c-116">List mapping</span></span>
   2. <span data-ttu-id="a368c-117">Mappning av intervallet</span><span class="sxs-lookup"><span data-stu-id="a368c-117">Range mapping</span></span>

<span data-ttu-id="a368c-118">En enskild klient-modell, skapa en **lista mappning** Fragmentera kartan.</span><span class="sxs-lookup"><span data-stu-id="a368c-118">For a single-tenant model, create a **list mapping** shard map.</span></span> <span data-ttu-id="a368c-119">Stöd för en innehavare modellen tilldelar en databas per klient.</span><span class="sxs-lookup"><span data-stu-id="a368c-119">The single-tenant model assigns one database per tenant.</span></span> <span data-ttu-id="a368c-120">Detta är en effektiv modell för SaaS-utvecklare som det förenklar hanteringen.</span><span class="sxs-lookup"><span data-stu-id="a368c-120">This is an effective model for SaaS developers as it simplifies management.</span></span>

![Lista över-mappning][1]

<span data-ttu-id="a368c-122">Modell för flera klienter tilldelas en enskild databas flera innehavare (och du kan distribuera grupper av klienter över flera databaser).</span><span class="sxs-lookup"><span data-stu-id="a368c-122">The multi-tenant model assigns several tenants to a single database (and you can distribute groups of tenants across multiple databases).</span></span> <span data-ttu-id="a368c-123">Använd den här modellen när du förväntar dig varje innehavare har liten databehov.</span><span class="sxs-lookup"><span data-stu-id="a368c-123">Use this model when you expect each tenant to have small data needs.</span></span> <span data-ttu-id="a368c-124">I den här modellen vi tilldela en uppsättning klienter till en databas med hjälp av **intervallet mappning**.</span><span class="sxs-lookup"><span data-stu-id="a368c-124">In this model, we assign a range of tenants to a database using **range mapping**.</span></span> 

![Mappning av intervallet][2]

<span data-ttu-id="a368c-126">Eller du kan implementera en databas för flera innehavare modellen med hjälp av en *lista mappning* tilldela flera klienter till en enskild databas.</span><span class="sxs-lookup"><span data-stu-id="a368c-126">Or you can implement a multi-tenant database model using a *list mapping* to assign multiple tenants to a single database.</span></span> <span data-ttu-id="a368c-127">Till exempel DB1 används för att lagra information om klient-id 1 och 5 och DB2 lagrar data för klienten 7 och 10-klient.</span><span class="sxs-lookup"><span data-stu-id="a368c-127">For example, DB1 is used to store information about tenant id 1 and 5, and DB2 stores data for tenant 7 and tenant 10.</span></span> 

![Muliple klienter i samma DB][3] 

### <a name="supported-net-types-for-sharding-keys"></a><span data-ttu-id="a368c-129">.Net-typer som stöds för horisontell partitionering nycklar</span><span class="sxs-lookup"><span data-stu-id="a368c-129">Supported .Net types for sharding keys</span></span>
<span data-ttu-id="a368c-130">Elastisk skala stöd följande .net Framework typer som horisontell partitionering nycklar:</span><span class="sxs-lookup"><span data-stu-id="a368c-130">Elastic Scale support the following .Net Framework types as sharding keys:</span></span>

* <span data-ttu-id="a368c-131">heltal</span><span class="sxs-lookup"><span data-stu-id="a368c-131">integer</span></span>
* <span data-ttu-id="a368c-132">lång</span><span class="sxs-lookup"><span data-stu-id="a368c-132">long</span></span>
* <span data-ttu-id="a368c-133">GUID</span><span class="sxs-lookup"><span data-stu-id="a368c-133">guid</span></span>
* <span data-ttu-id="a368c-134">byte]</span><span class="sxs-lookup"><span data-stu-id="a368c-134">byte[]</span></span>  
* <span data-ttu-id="a368c-135">Datum och tid</span><span class="sxs-lookup"><span data-stu-id="a368c-135">datetime</span></span>
* <span data-ttu-id="a368c-136">TimeSpan</span><span class="sxs-lookup"><span data-stu-id="a368c-136">timespan</span></span>
* <span data-ttu-id="a368c-137">DateTimeOffset</span><span class="sxs-lookup"><span data-stu-id="a368c-137">datetimeoffset</span></span>

### <a name="list-and-range-shard-maps"></a><span data-ttu-id="a368c-138">Lista och intervallet Fragmentera maps</span><span class="sxs-lookup"><span data-stu-id="a368c-138">List and range shard maps</span></span>
<span data-ttu-id="a368c-139">Fragmentera maps kan konstrueras med **listor över enskilda horisontell partitionering nyckeln värden**, eller så kan de vara konstruerade med **intervallen för horisontell partitionering nyckeln värden**.</span><span class="sxs-lookup"><span data-stu-id="a368c-139">Shard maps can be constructed using **lists of individual sharding key values**, or they can be constructed using **ranges of sharding key values**.</span></span> 

### <a name="list-shard-maps"></a><span data-ttu-id="a368c-140">Lista Fragmentera maps</span><span class="sxs-lookup"><span data-stu-id="a368c-140">List shard maps</span></span>
<span data-ttu-id="a368c-141">**Shards** innehåller **shardlets** och mappningen av shardlets till shards underhålls av en Fragmentera karta.</span><span class="sxs-lookup"><span data-stu-id="a368c-141">**Shards** contain **shardlets** and the mapping of shardlets to shards is maintained by a shard map.</span></span> <span data-ttu-id="a368c-142">En **lista Fragmentera kartan** är en association mellan enskilda nyckelvärden som identifierar shardlets och databaserna som fungerar som delar.</span><span class="sxs-lookup"><span data-stu-id="a368c-142">A **list shard map** is an association between the individual key values that identify the shardlets and the databases that serve as shards.</span></span>  <span data-ttu-id="a368c-143">**Visa en lista över mappningar** är explicit och andra viktiga värden kan mappas till samma databas.</span><span class="sxs-lookup"><span data-stu-id="a368c-143">**List mappings** are explicit and different key values can be mapped to the same database.</span></span> <span data-ttu-id="a368c-144">Till exempel nyckel 1 mappar till en-databas och nyckelvärden 3 och 6 referera databasen B.</span><span class="sxs-lookup"><span data-stu-id="a368c-144">For example, key 1 maps to Database A, and key values 3 and 6 both reference Database B.</span></span>

| <span data-ttu-id="a368c-145">Nyckel</span><span class="sxs-lookup"><span data-stu-id="a368c-145">Key</span></span> | <span data-ttu-id="a368c-146">Fragmentera plats</span><span class="sxs-lookup"><span data-stu-id="a368c-146">Shard Location</span></span> |
| --- | --- |
| <span data-ttu-id="a368c-147">1</span><span class="sxs-lookup"><span data-stu-id="a368c-147">1</span></span> |<span data-ttu-id="a368c-148">Database_A</span><span class="sxs-lookup"><span data-stu-id="a368c-148">Database_A</span></span> |
| <span data-ttu-id="a368c-149">3</span><span class="sxs-lookup"><span data-stu-id="a368c-149">3</span></span> |<span data-ttu-id="a368c-150">Database_B</span><span class="sxs-lookup"><span data-stu-id="a368c-150">Database_B</span></span> |
| <span data-ttu-id="a368c-151">4</span><span class="sxs-lookup"><span data-stu-id="a368c-151">4</span></span> |<span data-ttu-id="a368c-152">Database_C</span><span class="sxs-lookup"><span data-stu-id="a368c-152">Database_C</span></span> |
| <span data-ttu-id="a368c-153">6</span><span class="sxs-lookup"><span data-stu-id="a368c-153">6</span></span> |<span data-ttu-id="a368c-154">Database_B</span><span class="sxs-lookup"><span data-stu-id="a368c-154">Database_B</span></span> |
| <span data-ttu-id="a368c-155">...</span><span class="sxs-lookup"><span data-stu-id="a368c-155">...</span></span> |<span data-ttu-id="a368c-156">...</span><span class="sxs-lookup"><span data-stu-id="a368c-156">...</span></span> |

### <a name="range-shard-maps"></a><span data-ttu-id="a368c-157">Intervallet Fragmentera maps</span><span class="sxs-lookup"><span data-stu-id="a368c-157">Range shard maps</span></span>
<span data-ttu-id="a368c-158">I en **intervallet Fragmentera kartan**, viktiga intervallet anges med ett par **[lågt värde, högt värde)** där den *låg värdet* är den minsta nyckeln i intervallet, och *hög Värdet* är det första värdet som är högre än intervallet.</span><span class="sxs-lookup"><span data-stu-id="a368c-158">In a **range shard map**, the key range is described by a pair **[Low Value, High Value)** where the *Low Value* is the minimum key in the range, and the *High Value* is the first value higher than the range.</span></span> 

<span data-ttu-id="a368c-159">Till exempel **[0, 100)** innefattar alla heltal större än eller lika med 0 och mindre än 100.</span><span class="sxs-lookup"><span data-stu-id="a368c-159">For example, **[0, 100)** includes all integers greater than or equal 0 and less than 100.</span></span> <span data-ttu-id="a368c-160">Observera att flera adressintervall peka på samma databas och åtskilda intervall som stöds (t.ex. [100,200) och [400,600) pekar till databasen C i exemplet nedan.)</span><span class="sxs-lookup"><span data-stu-id="a368c-160">Note that multiple ranges can point to the same database, and disjoint ranges are supported (e.g., [100,200) and [400,600) both point to Database C in the example below.)</span></span>

| <span data-ttu-id="a368c-161">Nyckel</span><span class="sxs-lookup"><span data-stu-id="a368c-161">Key</span></span> | <span data-ttu-id="a368c-162">Fragmentera plats</span><span class="sxs-lookup"><span data-stu-id="a368c-162">Shard Location</span></span> |
| --- | --- |
| <span data-ttu-id="a368c-163">[1,50)</span><span class="sxs-lookup"><span data-stu-id="a368c-163">[1,50)</span></span> |<span data-ttu-id="a368c-164">Database_A</span><span class="sxs-lookup"><span data-stu-id="a368c-164">Database_A</span></span> |
| <span data-ttu-id="a368c-165">[50,100)</span><span class="sxs-lookup"><span data-stu-id="a368c-165">[50,100)</span></span> |<span data-ttu-id="a368c-166">Database_B</span><span class="sxs-lookup"><span data-stu-id="a368c-166">Database_B</span></span> |
| <span data-ttu-id="a368c-167">[100,200)</span><span class="sxs-lookup"><span data-stu-id="a368c-167">[100,200)</span></span> |<span data-ttu-id="a368c-168">Database_C</span><span class="sxs-lookup"><span data-stu-id="a368c-168">Database_C</span></span> |
| <span data-ttu-id="a368c-169">[400,600)</span><span class="sxs-lookup"><span data-stu-id="a368c-169">[400,600)</span></span> |<span data-ttu-id="a368c-170">Database_C</span><span class="sxs-lookup"><span data-stu-id="a368c-170">Database_C</span></span> |
| <span data-ttu-id="a368c-171">...</span><span class="sxs-lookup"><span data-stu-id="a368c-171">...</span></span> |<span data-ttu-id="a368c-172">...</span><span class="sxs-lookup"><span data-stu-id="a368c-172">...</span></span> |

<span data-ttu-id="a368c-173">Varje tabell som visas ovan är ett grundläggande exempel på en **ShardMap** objekt.</span><span class="sxs-lookup"><span data-stu-id="a368c-173">Each of the tables shown above is a conceptual example of a **ShardMap** object.</span></span> <span data-ttu-id="a368c-174">Varje rad är ett förenklat exempel på en enskild **PointMapping** (för listan Fragmentera kartan) eller **RangeMapping** (för intervallet Fragmentera kartan) objekt.</span><span class="sxs-lookup"><span data-stu-id="a368c-174">Each row is a simplified example of an individual **PointMapping** (for the list shard map) or **RangeMapping** (for the range shard map) object.</span></span>

## <a name="shard-map-manager"></a><span data-ttu-id="a368c-175">Fragmentera kartan manager</span><span class="sxs-lookup"><span data-stu-id="a368c-175">Shard map manager</span></span>
<span data-ttu-id="a368c-176">I klientbiblioteket är Fragmentera kartan manager en samling Fragmentera maps.</span><span class="sxs-lookup"><span data-stu-id="a368c-176">In the client library, the shard map  manager is a collection of shard maps.</span></span> <span data-ttu-id="a368c-177">De data som hanteras av en **ShardMapManager** instans sparas på tre platser:</span><span class="sxs-lookup"><span data-stu-id="a368c-177">The data managed by a **ShardMapManager** instance is kept in three places:</span></span> 

1. <span data-ttu-id="a368c-178">**Globala Fragmentera karta (GSM)**: du anger en databas som fungerar som lagringsplats för alla Fragmentera maps och mappningar.</span><span class="sxs-lookup"><span data-stu-id="a368c-178">**Global Shard Map (GSM)**: You specify a database to serve as the repository for all of its shard maps and mappings.</span></span> <span data-ttu-id="a368c-179">Särskilda tabeller och lagrade procedurer skapas automatiskt för att hantera informationen.</span><span class="sxs-lookup"><span data-stu-id="a368c-179">Special tables and stored procedures are automatically created to manage the information.</span></span> <span data-ttu-id="a368c-180">Detta är vanligtvis en liten databas och lätt öppnas och ska inte användas för andra behov av programmet.</span><span class="sxs-lookup"><span data-stu-id="a368c-180">This is typically a small database and lightly accessed, and it should not be used for other needs of the application.</span></span> <span data-ttu-id="a368c-181">Tabeller är i ett särskilt schema med namnet **__ShardManagement**.</span><span class="sxs-lookup"><span data-stu-id="a368c-181">The tables are in a special schema named **__ShardManagement**.</span></span> 
2. <span data-ttu-id="a368c-182">**Lokala Fragmentera karta (LSM)**: alla databaser som du anger ska vara en Fragmentera ändras till att innehålla flera små tabeller och särskilda lagrade procedurer som innehåller och hantera Fragmentera kartan information för att fragmentera.</span><span class="sxs-lookup"><span data-stu-id="a368c-182">**Local Shard Map (LSM)**: Every database that you specify to be a shard is modified to contain several small tables and special stored procedures that contain and manage shard map information specific to that shard.</span></span> <span data-ttu-id="a368c-183">Den här informationen är redundant med informationen i GSM och låter programmet för att verifiera informationen i cachelagrade Fragmentera kartan utan att placera belastning på GSM; programmet använder LSM för att avgöra om en cachelagrad mappning fortfarande är giltigt.</span><span class="sxs-lookup"><span data-stu-id="a368c-183">This information is redundant with the information in the GSM, and it allows the application to validate cached shard map information without placing any load on the GSM; the application uses the LSM to determine if a cached mapping is still valid.</span></span> <span data-ttu-id="a368c-184">Tabeller som motsvarar LSM på varje Fragmentera finns också i schemat **__ShardManagement**.</span><span class="sxs-lookup"><span data-stu-id="a368c-184">The tables corresponding to the LSM on each shard are also in the schema **__ShardManagement**.</span></span>
3. <span data-ttu-id="a368c-185">**Cacheminne**: varje instans åtkomst programmet till en **ShardMapManager** objekt upprätthåller en lokal minnescache av dess mappningar.</span><span class="sxs-lookup"><span data-stu-id="a368c-185">**Application cache**: Each application instance accessing a **ShardMapManager** object maintains a local in-memory cache of its mappings.</span></span> <span data-ttu-id="a368c-186">Lagrar den routningsinformation som nyligen har hämtats.</span><span class="sxs-lookup"><span data-stu-id="a368c-186">It stores routing information that has recently been retrieved.</span></span> 

## <a name="constructing-a-shardmapmanager"></a><span data-ttu-id="a368c-187">Hur du skapar en ShardMapManager</span><span class="sxs-lookup"><span data-stu-id="a368c-187">Constructing a ShardMapManager</span></span>
<span data-ttu-id="a368c-188">En **ShardMapManager** objektet har skapats med hjälp av en [factory](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerfactory.aspx) mönster.</span><span class="sxs-lookup"><span data-stu-id="a368c-188">A **ShardMapManager** object is constructed using a [factory](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerfactory.aspx) pattern.</span></span> <span data-ttu-id="a368c-189">Den  **[ShardMapManagerFactory.GetSqlShardMapManager](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerfactory.getsqlshardmapmanager.aspx)**  metoden tar autentiseringsuppgifter (inklusive servernamnet och databasnamnet hålla GSM) i form av en **ConnectionString** och returnerar en instans av en **ShardMapManager**.</span><span class="sxs-lookup"><span data-stu-id="a368c-189">The **[ShardMapManagerFactory.GetSqlShardMapManager](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerfactory.getsqlshardmapmanager.aspx)** method takes credentials (including the server name and database name holding the GSM) in the form of a **ConnectionString** and returns an instance of a **ShardMapManager**.</span></span>  

<span data-ttu-id="a368c-190">**Obs:** den **ShardMapManager** instansieras bara en gång per app-domän, inom initieringskoden för ett program.</span><span class="sxs-lookup"><span data-stu-id="a368c-190">**Please Note:** The **ShardMapManager** should be instantiated only once per app domain, within the initialization code for an application.</span></span> <span data-ttu-id="a368c-191">Skapa ytterligare instanser av ShardMapManager i samma appdomain resulterar i ökad minne och CPU-användning av programmet.</span><span class="sxs-lookup"><span data-stu-id="a368c-191">Creation of additional instances of ShardMapManager in the same appdomain, will result in increased memory and CPU utilization of the application.</span></span> <span data-ttu-id="a368c-192">En **ShardMapManager** kan innehålla valfritt antal Fragmentera maps.</span><span class="sxs-lookup"><span data-stu-id="a368c-192">A **ShardMapManager** can contain any number of shard maps.</span></span> <span data-ttu-id="a368c-193">Även om en enda Fragmentera karta är tillräcklig för många program, finns det tillfällen när olika uppsättningar av databaser som används för olika schemat eller för unika ändamål. i sådana fall kan det vara bättre att ange flera Fragmentera maps.</span><span class="sxs-lookup"><span data-stu-id="a368c-193">While a single shard map may be sufficient for many applications, there are times when different sets of databases are used for different schema or for unique purposes; in those cases multiple shard maps may be preferable.</span></span> 

<span data-ttu-id="a368c-194">I den här koden, ett program försöker öppna en befintlig **ShardMapManager** med den [TryGetSqlShardMapManager metoden](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerfactory.trygetsqlshardmapmanager.aspx).</span><span class="sxs-lookup"><span data-stu-id="a368c-194">In this code, an application tries to open an existing **ShardMapManager** with the [TryGetSqlShardMapManager method](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerfactory.trygetsqlshardmapmanager.aspx).</span></span>  <span data-ttu-id="a368c-195">Om objekt som representerar en Global **ShardMapManager** (GSM) inte ännu finns i databasen, klientbiblioteket skapar dem det med hjälp av den [CreateSqlShardMapManager metoden](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerfactory.createsqlshardmapmanager.aspx).</span><span class="sxs-lookup"><span data-stu-id="a368c-195">If objects representing a Global **ShardMapManager** (GSM) do not yet exist inside the database, the client library creates them there using the [CreateSqlShardMapManager method](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerfactory.createsqlshardmapmanager.aspx).</span></span>

    // Try to get a reference to the Shard Map Manager 
     // via the Shard Map Manager database.  
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
        // Create the Shard Map Manager. 
        ShardMapManagerFactory.CreateSqlShardMapManager(connectionString);
        Console.WriteLine("Created SqlShardMapManager"); 

        shardMapManager = ShardMapManagerFactory.GetSqlShardMapManager(
            connectionString, 
            ShardMapManagerLoadPolicy.Lazy);

        // The connectionString contains server name, database name, and admin credentials 
        // for privileges on both the GSM and the shards themselves.
    } 

<span data-ttu-id="a368c-196">Alternativt kan använda du Powershell för att skapa en ny Fragmentera kartan chef.</span><span class="sxs-lookup"><span data-stu-id="a368c-196">As an alternative, you can use Powershell to create a new Shard Map Manager.</span></span> <span data-ttu-id="a368c-197">Ett exempel är tillgänglig [här](https://gallery.technet.microsoft.com/scriptcenter/Azure-SQL-DB-Elastic-731883db).</span><span class="sxs-lookup"><span data-stu-id="a368c-197">An example is available [here](https://gallery.technet.microsoft.com/scriptcenter/Azure-SQL-DB-Elastic-731883db).</span></span>

## <a name="get-a-rangeshardmap-or-listshardmap"></a><span data-ttu-id="a368c-198">Hämta en RangeShardMap eller ListShardMap</span><span class="sxs-lookup"><span data-stu-id="a368c-198">Get a RangeShardMap or ListShardMap</span></span>
<span data-ttu-id="a368c-199">När du har skapat en Fragmentera kartan manager, som du kan hämta den [RangeShardMap](https://msdn.microsoft.com/library/azure/dn807318.aspx) eller [ListShardMap](https://msdn.microsoft.com/library/azure/dn807370.aspx) med hjälp av den [TryGetRangeShardMap](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.trygetrangeshardmap.aspx), [TryGetListShardMap ](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.trygetlistshardmap.aspx), eller [GetShardMap](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.getshardmap.aspx) metod.</span><span class="sxs-lookup"><span data-stu-id="a368c-199">After creating a shard map manager, you can get the [RangeShardMap](https://msdn.microsoft.com/library/azure/dn807318.aspx) or [ListShardMap](https://msdn.microsoft.com/library/azure/dn807370.aspx) using the [TryGetRangeShardMap](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.trygetrangeshardmap.aspx), the [TryGetListShardMap](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.trygetlistshardmap.aspx), or the [GetShardMap](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.getshardmap.aspx) method.</span></span>

    /// <summary>
    /// Creates a new Range Shard Map with the specified name, or gets the Range Shard Map if it already exists.
    /// </summary>
    public static RangeShardMap<T> CreateOrGetRangeShardMap<T>(ShardMapManager shardMapManager, string shardMapName)
    {
        // Try to get a reference to the Shard Map.
        RangeShardMap<T> shardMap;
        bool shardMapExists = shardMapManager.TryGetRangeShardMap(shardMapName, out shardMap);

        if (shardMapExists)
        {
            ConsoleUtils.WriteInfo("Shard Map {0} already exists", shardMap.Name);
        }
        else
        {
            // The Shard Map does not exist, so create it
            shardMap = shardMapManager.CreateRangeShardMap<T>(shardMapName);
            ConsoleUtils.WriteInfo("Created Shard Map {0}", shardMap.Name);
        }

        return shardMap;
    } 

### <a name="shard-map-administration-credentials"></a><span data-ttu-id="a368c-200">Fragmentera kartan administration autentiseringsuppgifter</span><span class="sxs-lookup"><span data-stu-id="a368c-200">Shard map administration credentials</span></span>
<span data-ttu-id="a368c-201">Program som administrera och ändra Fragmentera maps skiljer sig från de som använder Fragmentera mappar till flödet anslutningar.</span><span class="sxs-lookup"><span data-stu-id="a368c-201">Applications that administer and manipulate shard maps are different from those that use the shard maps to route connections.</span></span> 

<span data-ttu-id="a368c-202">Att administrera Fragmentera maps (lägga till eller ändra shards, Fragmentera kartor, Fragmentera mappningar osv) måste du initiera den **ShardMapManager** med **autentiseringsuppgifter som har behörighet för båda GSM databasen och på varje för läsning och skrivning databasen som fungerar som en Fragmentera**.</span><span class="sxs-lookup"><span data-stu-id="a368c-202">To administer shard maps (add or change shards, shard maps, shard mappings, etc.) you must instantiate the **ShardMapManager** using **credentials that have read/write privileges on both the GSM database and on each database that serves as a shard**.</span></span> <span data-ttu-id="a368c-203">Autentiseringsuppgifterna måste tillåta skrivningar till tabeller i både GSM och LSM som Fragmentera kartan information anges eller ändras, samt som skapar tabeller för LSM på nya delar.</span><span class="sxs-lookup"><span data-stu-id="a368c-203">The credentials must allow for writes against the tables in both the GSM and LSM as shard map information is entered or changed, as well as for creating LSM tables on new shards.</span></span>  

<span data-ttu-id="a368c-204">Se [autentiseringsuppgifter används för att komma åt klientbibliotek för elastisk databas](sql-database-elastic-scale-manage-credentials.md).</span><span class="sxs-lookup"><span data-stu-id="a368c-204">See [Credentials used to access the Elastic Database client library](sql-database-elastic-scale-manage-credentials.md).</span></span>

### <a name="only-metadata-affected"></a><span data-ttu-id="a368c-205">Endast de metadata som påverkas</span><span class="sxs-lookup"><span data-stu-id="a368c-205">Only metadata affected</span></span>
<span data-ttu-id="a368c-206">Metoder som används för att fylla eller ändra den **ShardMapManager** data inte ändrar de data som lagras i själva shards.</span><span class="sxs-lookup"><span data-stu-id="a368c-206">Methods used for populating or changing the **ShardMapManager** data do not alter the user data stored in the shards themselves.</span></span> <span data-ttu-id="a368c-207">Till exempel metoder som **CreateShard**, **DeleteShard**, **UpdateMapping**osv påverkar den Fragmentera kartan endast metadata.</span><span class="sxs-lookup"><span data-stu-id="a368c-207">For example, methods such as **CreateShard**, **DeleteShard**, **UpdateMapping**, etc. affect the shard map metadata only.</span></span> <span data-ttu-id="a368c-208">Ta inte bort, lägga till eller ändra informationen i delar.</span><span class="sxs-lookup"><span data-stu-id="a368c-208">They do not remove, add, or alter user data contained in the shards.</span></span> <span data-ttu-id="a368c-209">I stället dessa metoder är avsedda att användas tillsammans med olika åtgärder som du utför för att skapa eller ta bort faktiska databaser eller flytta rader från en Fragmentera till en annan att balansera delat miljö.</span><span class="sxs-lookup"><span data-stu-id="a368c-209">Instead, these methods are designed to be used in conjunction with separate operations you perform to create or remove actual databases, or that move rows from one shard to another to rebalance a sharded environment.</span></span>  <span data-ttu-id="a368c-210">(Den **delade dokument** verktyget som medföljer elastisk Databasverktyg gör användning av API: erna tillsammans med samordna faktiska dataflytten mellan shards.) Se [skalning med hjälp av verktyget för elastisk databas delade dokument](sql-database-elastic-scale-overview-split-and-merge.md).</span><span class="sxs-lookup"><span data-stu-id="a368c-210">(The **split-merge** tool included with elastic database tools makes use of these APIs along with orchestrating actual data movement between shards.) See [Scaling using the Elastic Database split-merge tool](sql-database-elastic-scale-overview-split-and-merge.md).</span></span>

## <a name="populating-a-shard-map-example"></a><span data-ttu-id="a368c-211">Fyller på en karta Fragmentera-exempel</span><span class="sxs-lookup"><span data-stu-id="a368c-211">Populating a shard map example</span></span>
<span data-ttu-id="a368c-212">En Exempelsekvens med åtgärder för att fylla i en specifik Fragmentera karta visas nedan.</span><span class="sxs-lookup"><span data-stu-id="a368c-212">An example sequence of operations to populate a specific shard map is shown below.</span></span> <span data-ttu-id="a368c-213">Kod som utför de här stegen:</span><span class="sxs-lookup"><span data-stu-id="a368c-213">The code performs these steps:</span></span> 

1. <span data-ttu-id="a368c-214">En ny Fragmentera karta skapas inom en Fragmentera kartan manager.</span><span class="sxs-lookup"><span data-stu-id="a368c-214">A new shard map is created within a shard map manager.</span></span> 
2. <span data-ttu-id="a368c-215">Metadata för två olika delar läggs till Fragmentera kartan.</span><span class="sxs-lookup"><span data-stu-id="a368c-215">The metadata for two different shards is added to the shard map.</span></span> 
3. <span data-ttu-id="a368c-216">En mängd viktiga intervallet mappningar läggs och övergripande innehållet i kartan Fragmentera visas.</span><span class="sxs-lookup"><span data-stu-id="a368c-216">A variety of key range mappings are added, and the overall contents of the shard map are displayed.</span></span> 

<span data-ttu-id="a368c-217">Koden är skriven så att metoden som kan köras igen om ett fel inträffar.</span><span class="sxs-lookup"><span data-stu-id="a368c-217">The code is written so that the method can be rerun if an error occurs.</span></span> <span data-ttu-id="a368c-218">Varje begäran testar om en Fragmentera eller mappning finns redan, innan du försöker att skapa den.</span><span class="sxs-lookup"><span data-stu-id="a368c-218">Each request tests whether a shard or mapping already exists, before attempting to create it.</span></span> <span data-ttu-id="a368c-219">Koden förutsätter att databaser med namnet **sample_shard_0**, **sample_shard_1** och **sample_shard_2** redan har skapats på servern som refereras av strängen **shardServer**.</span><span class="sxs-lookup"><span data-stu-id="a368c-219">The code assumes that databases named **sample_shard_0**, **sample_shard_1** and **sample_shard_2** have already been created in the server referenced by string **shardServer**.</span></span> 

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

            // List the shards and mappings 
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

<span data-ttu-id="a368c-220">Du kan använda PowerShell-skript som ett alternativ för att uppnå samma resultat.</span><span class="sxs-lookup"><span data-stu-id="a368c-220">As an alternative you can use PowerShell scripts to achieve the same result.</span></span> <span data-ttu-id="a368c-221">Vissa exempel PowerShell-exemplen finns [här](https://gallery.technet.microsoft.com/scriptcenter/Azure-SQL-DB-Elastic-731883db).</span><span class="sxs-lookup"><span data-stu-id="a368c-221">Some of the sample PowerShell examples are available [here](https://gallery.technet.microsoft.com/scriptcenter/Azure-SQL-DB-Elastic-731883db).</span></span>     

<span data-ttu-id="a368c-222">När Fragmentera maps är ifyllda, program för åtkomst av data skapas eller anpassas för att fungera med mappningarna.</span><span class="sxs-lookup"><span data-stu-id="a368c-222">Once shard maps have been populated, data access applications can be created or adapted to work with the maps.</span></span> <span data-ttu-id="a368c-223">Fylla eller ändra på maps behöver inte uppstår igen förrän **kartan layout** behöver ändras.</span><span class="sxs-lookup"><span data-stu-id="a368c-223">Populating or manipulating the maps need not occur again until **map layout** needs to change.</span></span>  

## <a name="data-dependent-routing"></a><span data-ttu-id="a368c-224">Databeroende routning</span><span class="sxs-lookup"><span data-stu-id="a368c-224">Data dependent routing</span></span>
<span data-ttu-id="a368c-225">Hanteraren för kartan Fragmentera används mest i program som kräver databasanslutningar utföra dataåtgärder som app-specifik.</span><span class="sxs-lookup"><span data-stu-id="a368c-225">The shard map manager will be most used in applications that require database connections to perform the app-specific data operations.</span></span> <span data-ttu-id="a368c-226">Dessa anslutningar måste vara kopplad till rätt databas.</span><span class="sxs-lookup"><span data-stu-id="a368c-226">Those connections must be associated with the correct database.</span></span> <span data-ttu-id="a368c-227">Detta kallas **Data beroende routning**.</span><span class="sxs-lookup"><span data-stu-id="a368c-227">This is known as **Data Dependent Routing**.</span></span> <span data-ttu-id="a368c-228">Initiera en Fragmentera kartan manager objekt från fabriken med hjälp av autentiseringsuppgifter som har skrivskyddad åtkomst på GSM databasen för dessa program.</span><span class="sxs-lookup"><span data-stu-id="a368c-228">For these applications, instantiate a shard map manager object from the factory using credentials that have read-only access on the GSM database.</span></span> <span data-ttu-id="a368c-229">Enskilda förfrågningar för senare anslutningar ange autentiseringsuppgifter som krävs för att ansluta till databasen lämpliga Fragmentera.</span><span class="sxs-lookup"><span data-stu-id="a368c-229">Individual requests for later connections supply credentials necessary for connecting to the appropriate shard database.</span></span>

<span data-ttu-id="a368c-230">Observera att dessa program (med hjälp av **ShardMapManager** öppnas med skrivskyddad autentiseringsuppgifter) kan inte göra ändringar i maps eller mappningar.</span><span class="sxs-lookup"><span data-stu-id="a368c-230">Note that these applications (using **ShardMapManager** opened with read-only credentials) cannot make changes to the maps or mappings.</span></span> <span data-ttu-id="a368c-231">Skapa administrativa specifika program eller PowerShell-skript som anger högre Privilegierade autentiseringsuppgifter som tidigare diskuterats för dessa behov.</span><span class="sxs-lookup"><span data-stu-id="a368c-231">For those needs, create administrative-specific applications or PowerShell scripts that supply higher-privileged credentials as discussed earlier.</span></span> <span data-ttu-id="a368c-232">Se [autentiseringsuppgifter används för att komma åt klientbibliotek för elastisk databas](sql-database-elastic-scale-manage-credentials.md).</span><span class="sxs-lookup"><span data-stu-id="a368c-232">See [Credentials used to access the Elastic Database client library](sql-database-elastic-scale-manage-credentials.md).</span></span>

<span data-ttu-id="a368c-233">Mer information finns i [Data beroende routning](sql-database-elastic-scale-data-dependent-routing.md).</span><span class="sxs-lookup"><span data-stu-id="a368c-233">For more details, see [Data dependent routing](sql-database-elastic-scale-data-dependent-routing.md).</span></span> 

## <a name="modifying-a-shard-map"></a><span data-ttu-id="a368c-234">Ändra en Fragmentera karta</span><span class="sxs-lookup"><span data-stu-id="a368c-234">Modifying a shard map</span></span>
<span data-ttu-id="a368c-235">En Fragmentera karta kan ändras på olika sätt.</span><span class="sxs-lookup"><span data-stu-id="a368c-235">A shard map can be changed in different ways.</span></span> <span data-ttu-id="a368c-236">Alla följande metoder ändra de metadata som beskriver delar och deras mappningar, men de kan inte ändra data i shards fysiskt eller gör de skapa eller ta bort faktiska databaser.</span><span class="sxs-lookup"><span data-stu-id="a368c-236">All of the following methods modify the metadata describing the shards and their mappings, but they do not physically modify data within the shards, nor do they create or delete the actual databases.</span></span>  <span data-ttu-id="a368c-237">Vissa åtgärder på kartan Fragmentera beskrivs nedan kan behöva samordnas med administrativa åtgärder som fysiskt flytta data eller att lägga till och ta bort databaser som fungerar som delar.</span><span class="sxs-lookup"><span data-stu-id="a368c-237">Some of the operations on the shard map described below may need to be coordinated with administrative actions that physically move data or that add and remove databases serving as shards.</span></span>

<span data-ttu-id="a368c-238">Dessa metoder fungerar tillsammans som byggblock som är tillgängliga för att ändra den övergripande fördelningen av data i din databasmiljö för delat.</span><span class="sxs-lookup"><span data-stu-id="a368c-238">These methods work together as the building blocks available for modifying the overall distribution of data in your sharded database environment.</span></span>  

* <span data-ttu-id="a368c-239">Lägg till eller ta bort delar: använda  **[CreateShard](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmap.createshard.aspx)**  och  **[DeleteShard](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmap.deleteshard.aspx)**  av den [Shardmap klassen](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmap.aspx).</span><span class="sxs-lookup"><span data-stu-id="a368c-239">To add or remove shards: use **[CreateShard](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmap.createshard.aspx)** and **[DeleteShard](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmap.deleteshard.aspx)** of the [Shardmap class](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmap.aspx).</span></span> 
  
    <span data-ttu-id="a368c-240">Servern och databasen som representerar målet Fragmentera måste redan finnas för dessa åtgärder att köra.</span><span class="sxs-lookup"><span data-stu-id="a368c-240">The server and database representing the target shard must already exist for these operations to execute.</span></span> <span data-ttu-id="a368c-241">Dessa metoder har inte någon effekt på databaserna som fristående, endast för metadata i kartan Fragmentera.</span><span class="sxs-lookup"><span data-stu-id="a368c-241">These methods do not have any impact on the databases themselves, only on metadata in the shard map.</span></span>
* <span data-ttu-id="a368c-242">Skapa eller ta bort punkter eller intervall som mappas till delar: använda  **[CreateRangeMapping](https://msdn.microsoft.com/library/azure/dn841993.aspx)**,  **[DeleteMapping](https://msdn.microsoft.com/library/azure/dn824200.aspx)**  av [ RangeShardMapping klassen](https://msdn.microsoft.com/library/azure/dn807318.aspx), och  **[CreatePointMapping](https://msdn.microsoft.com/library/azure/dn807218.aspx)**  av den [ListShardMap](https://msdn.microsoft.com/library/azure/dn842123.aspx)</span><span class="sxs-lookup"><span data-stu-id="a368c-242">To create or remove points or ranges that are mapped to the shards: use **[CreateRangeMapping](https://msdn.microsoft.com/library/azure/dn841993.aspx)**, **[DeleteMapping](https://msdn.microsoft.com/library/azure/dn824200.aspx)** of the [RangeShardMapping class](https://msdn.microsoft.com/library/azure/dn807318.aspx), and **[CreatePointMapping](https://msdn.microsoft.com/library/azure/dn807218.aspx)** of the [ListShardMap](https://msdn.microsoft.com/library/azure/dn842123.aspx)</span></span>
  
    <span data-ttu-id="a368c-243">Många olika punkter eller intervall kan mappas till samma Fragmentera.</span><span class="sxs-lookup"><span data-stu-id="a368c-243">Many different points or ranges can be mapped to the same shard.</span></span> <span data-ttu-id="a368c-244">Dessa metoder påverkar endast metadata - de påverkar inte data som kanske redan finns i shards.</span><span class="sxs-lookup"><span data-stu-id="a368c-244">These methods only affect metadata - they do not affect any data that may already be present in shards.</span></span> <span data-ttu-id="a368c-245">Om data måste tas bort från databasen för att överensstämma med **DeleteMapping** åtgärder du måste utföra dessa åtgärder separat men tillsammans med hjälp av dessa metoder.</span><span class="sxs-lookup"><span data-stu-id="a368c-245">If data needs to be removed from the database in order to be consistent with **DeleteMapping** operations, you will need to perform those operations separately but in conjunction with using these methods.</span></span>  
* <span data-ttu-id="a368c-246">Att dela befintliga områden i två eller sammanfoga intilliggande adressintervall till ett: använda  **[SplitMapping](https://msdn.microsoft.com/library/azure/dn824205.aspx)**  och  **[MergeMappings](https://msdn.microsoft.com/library/azure/dn824201.aspx)**.</span><span class="sxs-lookup"><span data-stu-id="a368c-246">To split existing ranges into two, or merge adjacent ranges into one: use **[SplitMapping](https://msdn.microsoft.com/library/azure/dn824205.aspx)** and **[MergeMappings](https://msdn.microsoft.com/library/azure/dn824201.aspx)**.</span></span>  
  
    <span data-ttu-id="a368c-247">Observera att dela och slå samman operations **inte ändra Fragmentera som nyckelvärden mappas**.</span><span class="sxs-lookup"><span data-stu-id="a368c-247">Note that split and merge operations **do not change the shard to which key values are mapped**.</span></span> <span data-ttu-id="a368c-248">En delning delar ett befintligt intervall i två delar, men lämnar både som mappas till samma Fragmentera.</span><span class="sxs-lookup"><span data-stu-id="a368c-248">A split breaks an existing range into two parts, but leaves both as mapped to the same shard.</span></span> <span data-ttu-id="a368c-249">En koppling fungerar på två angränsande områden som redan är mappade till samma fragment, mottagarsidan dem till ett område.</span><span class="sxs-lookup"><span data-stu-id="a368c-249">A merge operates on two adjacent ranges that are already mapped to the same shard, coalescing them into a single range.</span></span>  <span data-ttu-id="a368c-250">Punkter eller intervall själva mellan shards måste samordnas med hjälp av **UpdateMapping** tillsammans med faktiska dataflytten.</span><span class="sxs-lookup"><span data-stu-id="a368c-250">The movement of points or ranges themselves between shards needs to be coordinated by using **UpdateMapping** in conjunction with actual data movement.</span></span>  <span data-ttu-id="a368c-251">Du kan använda den **dela/Merge** tjänst som är en del av elastiska Databasverktyg för att samordna Fragmentera kartan ändringar med dataflyttning flytt krävs.</span><span class="sxs-lookup"><span data-stu-id="a368c-251">You can use the **Split/Merge** service that is part of elastic database tools to coordinate shard map changes with data movement, when movement is needed.</span></span> 
* <span data-ttu-id="a368c-252">Att mappa (eller flytta) enskilda punkter eller intervall för olika delar: använda  **[UpdateMapping](https://msdn.microsoft.com/library/azure/dn824207.aspx)**.</span><span class="sxs-lookup"><span data-stu-id="a368c-252">To re-map (or move) individual points or ranges to different shards: use **[UpdateMapping](https://msdn.microsoft.com/library/azure/dn824207.aspx)**.</span></span>  
  
    <span data-ttu-id="a368c-253">Eftersom data kan behöva flyttas från en Fragmentera till en annan för att överensstämma med **UpdateMapping** åtgärder, du måste utföra den flytt separat men tillsammans med hjälp av dessa metoder.</span><span class="sxs-lookup"><span data-stu-id="a368c-253">Since data may need to be moved from one shard to another in order to be consistent with **UpdateMapping** operations, you will need to perform that movement separately but in conjunction with using these methods.</span></span>
* <span data-ttu-id="a368c-254">Göra mappningar online och offline: använda  **[MarkMappingOffline](https://msdn.microsoft.com/library/azure/dn824202.aspx)**  och  **[MarkMappingOnline](https://msdn.microsoft.com/library/azure/dn807225.aspx)**  att styra online tillståndet för en mappning.</span><span class="sxs-lookup"><span data-stu-id="a368c-254">To take mappings online and offline: use **[MarkMappingOffline](https://msdn.microsoft.com/library/azure/dn824202.aspx)** and **[MarkMappingOnline](https://msdn.microsoft.com/library/azure/dn807225.aspx)** to control the online state of a mapping.</span></span> 
  
    <span data-ttu-id="a368c-255">Vissa åtgärder på Fragmentera mappningar tillåts endast när en mappning är i tillståndet ”offline” inklusive **UpdateMapping** och **DeleteMapping**.</span><span class="sxs-lookup"><span data-stu-id="a368c-255">Certain operations on shard mappings are only allowed when a mapping is in an “offline” state, including **UpdateMapping** and **DeleteMapping**.</span></span> <span data-ttu-id="a368c-256">När en mappning är offline, returneras ett fel en data-beroende begäran baserat på en nyckel som ingår i mappningen.</span><span class="sxs-lookup"><span data-stu-id="a368c-256">When a mapping is offline, a data-dependent request based on a key included in that mapping will return an error.</span></span> <span data-ttu-id="a368c-257">När ett intervall först kopplas från avslutats dessutom alla anslutningar till den berörda Fragmentera automatiskt för att förhindra inkonsekvent eller ofullständig resultat för frågor som riktar sig mot områden som ändras.</span><span class="sxs-lookup"><span data-stu-id="a368c-257">In addition, when a range is first taken offline, all connections to the affected shard are automatically killed in order to prevent inconsistent or incomplete results for queries directed against ranges being changed.</span></span> 

<span data-ttu-id="a368c-258">Mappningar är oföränderliga objekt i .net.</span><span class="sxs-lookup"><span data-stu-id="a368c-258">Mappings are immutable objects in .Net.</span></span>  <span data-ttu-id="a368c-259">Alla de metoder som ändrar mappningar ogiltig även alla referenser till dem i din kod.</span><span class="sxs-lookup"><span data-stu-id="a368c-259">All of the methods above that change mappings also invalidate any references to them in your code.</span></span> <span data-ttu-id="a368c-260">Om du vill göra det enklare att utföra sekvenser av åtgärder som ändrar tillstånd för en mappning, returnera alla metoder som ändrar en mappning för en ny mappning referens så kan vara att härleda operations.</span><span class="sxs-lookup"><span data-stu-id="a368c-260">To make it easier to perform sequences of operations that change a mapping’s state, all of the methods that change a mapping return a new mapping reference, so operations can be chained.</span></span> <span data-ttu-id="a368c-261">Om du vill ta bort en befintlig mappning i shardmap sm som innehåller nyckeln 25, kan du till exempel köra följande:</span><span class="sxs-lookup"><span data-stu-id="a368c-261">For example, to delete an existing mapping in shardmap sm that contains the key 25, you can execute the following:</span></span> 

        sm.DeleteMapping(sm.MarkMappingOffline(sm.GetMappingForKey(25)));

## <a name="adding-a-shard"></a><span data-ttu-id="a368c-262">Lägga till en Fragmentera</span><span class="sxs-lookup"><span data-stu-id="a368c-262">Adding a shard</span></span>
<span data-ttu-id="a368c-263">Program behöver ofta helt enkelt lägga till nya delar för att hantera data som förväntas av nya nycklar eller nyckelintervall för en Fragmentera som redan finns.</span><span class="sxs-lookup"><span data-stu-id="a368c-263">Applications often need to simply add new shards to handle data that is expected from new keys or key ranges, for a shard map that already exists.</span></span> <span data-ttu-id="a368c-264">Till exempel ett delat program genom att klient-ID kan behöva etablera en ny Fragmentera för en ny klient eller varje månad delat data måste en ny Fragmentera etablerats före varje ny månad.</span><span class="sxs-lookup"><span data-stu-id="a368c-264">For example, an application sharded by Tenant ID may need to provision a new shard for a new tenant, or data sharded monthly may need a new shard provisioned before the start of each new month.</span></span> 

<span data-ttu-id="a368c-265">Om nya värdeintervallet nycklar inte är en del av en befintlig mappning och inga dataflyttning krävs, är det mycket enkelt att lägga till nya Fragmentera och associera den nya nyckeln eller området till att fragmentera.</span><span class="sxs-lookup"><span data-stu-id="a368c-265">If the new range of key values is not already part of an existing mapping and no data movement is necessary, it is very simple to add the new shard and associate the new key or range to that shard.</span></span> <span data-ttu-id="a368c-266">Mer information om att lägga till nya shards finns [att lägga till en ny Fragmentera](sql-database-elastic-scale-add-a-shard.md).</span><span class="sxs-lookup"><span data-stu-id="a368c-266">For details on adding new shards, see [Adding a new shard](sql-database-elastic-scale-add-a-shard.md).</span></span>

<span data-ttu-id="a368c-267">För scenarier som kräver dataflyttning dock behövs verktyget delade dokument för att samordna dataförflyttning mellan shards i kombination med de nödvändiga Fragmentera karta uppdateringarna.</span><span class="sxs-lookup"><span data-stu-id="a368c-267">For scenarios that require data movement, however, the split-merge tool is needed to orchestrate the data movement between shards in combination with the necessary shard map updates.</span></span> <span data-ttu-id="a368c-268">Mer information om hur du använder delade dokument yool finns [översikt över delade dokument](sql-database-elastic-scale-overview-split-and-merge.md)</span><span class="sxs-lookup"><span data-stu-id="a368c-268">For details on using the split-merge yool, see [Overview of split-merge](sql-database-elastic-scale-overview-split-and-merge.md)</span></span> 

[!INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Image references-->
[1]: ./media/sql-database-elastic-scale-shard-map-management/listmapping.png
[2]: ./media/sql-database-elastic-scale-shard-map-management/rangemapping.png
[3]: ./media/sql-database-elastic-scale-shard-map-management/multipleonsingledb.png
