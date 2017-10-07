---
title: aaaMigrate befintliga databaser tooscale ut | Microsoft Docs
description: Konvertera delat databaser toouse elastisk Databasverktyg genom att skapa en Fragmentera kartan manager
services: sql-database
documentationcenter: 
author: ddove
manager: jhubbard
editor: 
ms.assetid: 8c851d8e-8fd5-4327-89c1-9178b20ddd69
ms.service: sql-database
ms.custom: scale out apps
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-management
ms.date: 10/24/2016
ms.author: ddove
ms.openlocfilehash: fa2c9e3699f30667cf547d1faadf4504609199be
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="migrate-existing-databases-tooscale-out"></a><span data-ttu-id="bf2c0-103">Migrera befintliga databaser tooscale ut</span><span class="sxs-lookup"><span data-stu-id="bf2c0-103">Migrate existing databases tooscale-out</span></span>
<span data-ttu-id="bf2c0-104">Enkelt hantera dina befintliga delat utskalat-databaser som använder Azure SQL Database databaser (till exempel hello [klientbibliotek för elastisk databas](sql-database-elastic-database-client-library.md)).</span><span class="sxs-lookup"><span data-stu-id="bf2c0-104">Easily manage your existing scaled-out sharded databases using Azure SQL Database database tools (such as hello [Elastic Database client library](sql-database-elastic-database-client-library.md)).</span></span> <span data-ttu-id="bf2c0-105">Först måste du konvertera en befintlig uppsättning databaser toouse hello [Fragmentera kartan manager](sql-database-elastic-scale-shard-map-management.md).</span><span class="sxs-lookup"><span data-stu-id="bf2c0-105">You must first convert an existing set of databases toouse hello [shard map manager](sql-database-elastic-scale-shard-map-management.md).</span></span> 

## <a name="overview"></a><span data-ttu-id="bf2c0-106">Översikt</span><span class="sxs-lookup"><span data-stu-id="bf2c0-106">Overview</span></span>
<span data-ttu-id="bf2c0-107">toomigrate en befintlig delat databas:</span><span class="sxs-lookup"><span data-stu-id="bf2c0-107">toomigrate an existing sharded database:</span></span> 

1. <span data-ttu-id="bf2c0-108">Förbereda hello [Fragmentera kartan manager-databasen](sql-database-elastic-scale-shard-map-management.md).</span><span class="sxs-lookup"><span data-stu-id="bf2c0-108">Prepare hello [shard map manager database](sql-database-elastic-scale-shard-map-management.md).</span></span>
2. <span data-ttu-id="bf2c0-109">Skapa hello Fragmentera karta.</span><span class="sxs-lookup"><span data-stu-id="bf2c0-109">Create hello shard map.</span></span>
3. <span data-ttu-id="bf2c0-110">Förbered hello enskilda delar.</span><span class="sxs-lookup"><span data-stu-id="bf2c0-110">Prepare hello individual shards.</span></span>  
4. <span data-ttu-id="bf2c0-111">Lägga till mappningar toohello Fragmentera karta.</span><span class="sxs-lookup"><span data-stu-id="bf2c0-111">Add mappings toohello shard map.</span></span>

<span data-ttu-id="bf2c0-112">Dessa tekniker kan implementeras med hjälp av antingen hello [klientbiblioteket för .NET Framework](http://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/), eller hello PowerShell-skript som finns på [Azure SQL DB - skript för elastisk databas verktyg](https://gallery.technet.microsoft.com/scriptcenter/Azure-SQL-DB-Elastic-731883db).</span><span class="sxs-lookup"><span data-stu-id="bf2c0-112">These techniques can be implemented using either hello [.NET Framework client library](http://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/), or hello PowerShell scripts found at [Azure SQL DB - Elastic Database tools scripts](https://gallery.technet.microsoft.com/scriptcenter/Azure-SQL-DB-Elastic-731883db).</span></span> <span data-ttu-id="bf2c0-113">här hello exemplen använder hello PowerShell-skript.</span><span class="sxs-lookup"><span data-stu-id="bf2c0-113">hello examples here use hello PowerShell scripts.</span></span>

<span data-ttu-id="bf2c0-114">Läs mer om hello ShardMapManager [Fragmentera kartan management](sql-database-elastic-scale-shard-map-management.md).</span><span class="sxs-lookup"><span data-stu-id="bf2c0-114">For more information about hello ShardMapManager, see [Shard map management](sql-database-elastic-scale-shard-map-management.md).</span></span> <span data-ttu-id="bf2c0-115">En översikt över hello elastisk Databasverktyg finns [elastisk databas funktionsöversikt](sql-database-elastic-scale-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="bf2c0-115">For an overview of hello elastic database tools, see [Elastic Database features overview](sql-database-elastic-scale-introduction.md).</span></span>

## <a name="prepare-hello-shard-map-manager-database"></a><span data-ttu-id="bf2c0-116">Förbereda hello Fragmentera kartan manager-databasen</span><span class="sxs-lookup"><span data-stu-id="bf2c0-116">Prepare hello shard map manager database</span></span>
<span data-ttu-id="bf2c0-117">hello Fragmentera kartan manager är en särskild databas som innehåller hello data toomanage utskalat databaser.</span><span class="sxs-lookup"><span data-stu-id="bf2c0-117">hello shard map manager is a special database that contains hello data toomanage scaled-out databases.</span></span> <span data-ttu-id="bf2c0-118">Du kan använda en befintlig databas eller skapa en ny databas.</span><span class="sxs-lookup"><span data-stu-id="bf2c0-118">You can use an existing database, or create a new database.</span></span> <span data-ttu-id="bf2c0-119">Observera att en databas som fungerar som Fragmentera kartan manager inte får vara hello samma databas som en Fragmentera.</span><span class="sxs-lookup"><span data-stu-id="bf2c0-119">Note that a database acting as shard map manager should not be hello same database as a shard.</span></span> <span data-ttu-id="bf2c0-120">Observera också att hello PowerShell-skript inte skapar hello databasen för dig.</span><span class="sxs-lookup"><span data-stu-id="bf2c0-120">Also note that hello PowerShell script does not create hello database for you.</span></span> 

## <a name="step-1-create-a-shard-map-manager"></a><span data-ttu-id="bf2c0-121">Steg 1: skapa en Fragmentera kartan manager</span><span class="sxs-lookup"><span data-stu-id="bf2c0-121">Step 1: create a shard map manager</span></span>
    # Create a shard map manager. 
    New-ShardMapManager -UserName '<user_name>' 
    -Password '<password>' 
    -SqlServerName '<server_name>' 
    -SqlDatabaseName '<smm_db_name>' 
    #<server_name> and <smm_db_name> are hello server name and database name 
    # for hello new or existing database that should be used for storing 
    # tenant-database mapping information.

### <a name="tooretrieve-hello-shard-map-manager"></a><span data-ttu-id="bf2c0-122">tooretrieve hello Fragmentera kartan manager</span><span class="sxs-lookup"><span data-stu-id="bf2c0-122">tooretrieve hello shard map manager</span></span>
<span data-ttu-id="bf2c0-123">När den har skapats, kan du hämta hello Fragmentera kartan manager med denna cmdlet.</span><span class="sxs-lookup"><span data-stu-id="bf2c0-123">After creation, you can retrieve hello shard map manager with this cmdlet.</span></span> <span data-ttu-id="bf2c0-124">Det här steget krävs varje gång du behöver toouse hello ShardMapManager objekt.</span><span class="sxs-lookup"><span data-stu-id="bf2c0-124">This step is needed every time you need toouse hello ShardMapManager object.</span></span>

    # Try tooget a reference toohello Shard Map Manager  
    $ShardMapManager = Get-ShardMapManager -UserName '<user_name>' 
    -Password '<password>' 
    -SqlServerName '<server_name>' 
    -SqlDatabaseName '<smm_db_name>' 


## <a name="step-2-create-hello-shard-map"></a><span data-ttu-id="bf2c0-125">Steg 2: skapa hello Fragmentera karta</span><span class="sxs-lookup"><span data-stu-id="bf2c0-125">Step 2: create hello shard map</span></span>
<span data-ttu-id="bf2c0-126">Du måste ange hello Fragmentera kartan toocreate.</span><span class="sxs-lookup"><span data-stu-id="bf2c0-126">You must select hello type of shard map toocreate.</span></span> <span data-ttu-id="bf2c0-127">hello dig beror på hello database-arkitektur:</span><span class="sxs-lookup"><span data-stu-id="bf2c0-127">hello choice depends on hello database architecture:</span></span> 

1. <span data-ttu-id="bf2c0-128">En organisation per databas (villkor, finns hello [ordlista](sql-database-elastic-scale-glossary.md).)</span><span class="sxs-lookup"><span data-stu-id="bf2c0-128">Single tenant per database (For terms, see hello [glossary](sql-database-elastic-scale-glossary.md).)</span></span> 
2. <span data-ttu-id="bf2c0-129">Flera klienter per databas (två typer):</span><span class="sxs-lookup"><span data-stu-id="bf2c0-129">Multiple tenants per database (two types):</span></span>
   1. <span data-ttu-id="bf2c0-130">Lista över-mappning</span><span class="sxs-lookup"><span data-stu-id="bf2c0-130">List mapping</span></span>
   2. <span data-ttu-id="bf2c0-131">Mappning av intervallet</span><span class="sxs-lookup"><span data-stu-id="bf2c0-131">Range mapping</span></span>

<span data-ttu-id="bf2c0-132">En enskild klient-modell, skapa en **lista mappning** Fragmentera kartan.</span><span class="sxs-lookup"><span data-stu-id="bf2c0-132">For a single-tenant model, create a **list mapping** shard map.</span></span> <span data-ttu-id="bf2c0-133">modell för hello single-klienter tilldelas en databas per klient.</span><span class="sxs-lookup"><span data-stu-id="bf2c0-133">hello single-tenant model assigns one database per tenant.</span></span> <span data-ttu-id="bf2c0-134">Detta är en effektiv modell för SaaS-utvecklare som det förenklar hanteringen.</span><span class="sxs-lookup"><span data-stu-id="bf2c0-134">This is an effective model for SaaS developers as it simplifies management.</span></span>

![Lista över-mappning][1]

<span data-ttu-id="bf2c0-136">hello modell för flera klienter tilldelas flera innehavare tooa enskild databas (och du kan distribuera grupper av klienter över flera databaser).</span><span class="sxs-lookup"><span data-stu-id="bf2c0-136">hello multi-tenant model assigns several tenants tooa single database (and you can distribute groups of tenants across multiple databases).</span></span> <span data-ttu-id="bf2c0-137">Använd den här modellen när du förväntar dig varje klient toohave små databehov.</span><span class="sxs-lookup"><span data-stu-id="bf2c0-137">Use this model when you expect each tenant toohave small data needs.</span></span> <span data-ttu-id="bf2c0-138">I den här modellen vi tilldela ett intervall med klienter tooa en databas med hjälp av **intervallet mappning**.</span><span class="sxs-lookup"><span data-stu-id="bf2c0-138">In this model, we assign a range of tenants tooa database using **range mapping**.</span></span> 

![Mappning av intervallet][2]

<span data-ttu-id="bf2c0-140">Eller du kan implementera en databas för flera innehavare modellen med hjälp av en *lista mappning* tooassign flera innehavare tooa enskild databas.</span><span class="sxs-lookup"><span data-stu-id="bf2c0-140">Or you can implement a multi-tenant database model using a *list mapping* tooassign multiple tenants tooa single database.</span></span> <span data-ttu-id="bf2c0-141">Till exempel DB1 är används toostore information om klient-id 1 och 5 och DB2 lagrar data för klienten 7 och 10-klient.</span><span class="sxs-lookup"><span data-stu-id="bf2c0-141">For example, DB1 is used toostore information about tenant id 1 and 5, and DB2 stores data for tenant 7 and tenant 10.</span></span> 

![Muliple klienter i samma DB][3] 

<span data-ttu-id="bf2c0-143">**Baserat på ditt val, välja ett av följande alternativ:**</span><span class="sxs-lookup"><span data-stu-id="bf2c0-143">**Based on your choice, choose one of these options:**</span></span>

### <a name="option-1-create-a-shard-map-for-a-list-mapping"></a><span data-ttu-id="bf2c0-144">Alternativ 1: skapa en Fragmentera karta för en lista över-mappning</span><span class="sxs-lookup"><span data-stu-id="bf2c0-144">Option 1: create a shard map for a list mapping</span></span>
<span data-ttu-id="bf2c0-145">Skapa en Fragmentera karta med hello ShardMapManager-objektet.</span><span class="sxs-lookup"><span data-stu-id="bf2c0-145">Create a shard map using hello ShardMapManager object.</span></span> 

    # $ShardMapManager is hello shard map manager object. 
    $ShardMap = New-ListShardMap -KeyType $([int]) 
    -ListShardMapName 'ListShardMap' 
    -ShardMapManager $ShardMapManager 


### <a name="option-2-create-a-shard-map-for-a-range-mapping"></a><span data-ttu-id="bf2c0-146">Alternativ 2: skapa en Fragmentera karta för mappning av intervallet</span><span class="sxs-lookup"><span data-stu-id="bf2c0-146">Option 2: create a shard map for a range mapping</span></span>
<span data-ttu-id="bf2c0-147">Observera att tooutilize det här mönstret för mappning, klient-ID-värden måste toobe kontinuerlig intervall och det är acceptabel toohave lucka i hello intervall genom att helt enkelt hoppa hello intervallet när du skapar hello databaser.</span><span class="sxs-lookup"><span data-stu-id="bf2c0-147">Note that tooutilize this mapping pattern, tenant id values needs toobe continuous ranges, and it is acceptable toohave gap in hello ranges by simply skipping hello range when creating hello databases.</span></span>

    # $ShardMapManager is hello shard map manager object 
    # 'RangeShardMap' is hello unique identifier for hello range shard map.  
    $ShardMap = New-RangeShardMap 
    -KeyType $([int]) 
    -RangeShardMapName 'RangeShardMap' 
    -ShardMapManager $ShardMapManager 

### <a name="option-3-list-mappings-on-a-single-database"></a><span data-ttu-id="bf2c0-148">Alternativ 3: Visa en lista över mappningar på en enskild databas</span><span class="sxs-lookup"><span data-stu-id="bf2c0-148">Option 3: List mappings on a single database</span></span>
<span data-ttu-id="bf2c0-149">Ställa in det här mönstret kräver också att skapa en karta i listan som visas i steg 2, alternativ 1.</span><span class="sxs-lookup"><span data-stu-id="bf2c0-149">Setting up this pattern also requires creation of a list map as shown in step 2, option 1.</span></span>

## <a name="step-3-prepare-individual-shards"></a><span data-ttu-id="bf2c0-150">Steg 3: Förbered enskilda delar</span><span class="sxs-lookup"><span data-stu-id="bf2c0-150">Step 3: Prepare individual shards</span></span>
<span data-ttu-id="bf2c0-151">Lägg till varje Fragmentera (databasen) toohello Fragmentera kartan manager.</span><span class="sxs-lookup"><span data-stu-id="bf2c0-151">Add each shard (database) toohello shard map manager.</span></span> <span data-ttu-id="bf2c0-152">Detta förbereder hello enskilda databaser för att lagra mappningsinformation.</span><span class="sxs-lookup"><span data-stu-id="bf2c0-152">This prepares hello individual databases for storing mapping information.</span></span> <span data-ttu-id="bf2c0-153">Köra den här metoden på varje Fragmentera.</span><span class="sxs-lookup"><span data-stu-id="bf2c0-153">Execute this method on each shard.</span></span>

    Add-Shard 
    -ShardMap $ShardMap 
    -SqlServerName '<shard_server_name>' 
    -SqlDatabaseName '<shard_database_name>'
    # hello $ShardMap is hello shard map created in step 2.


## <a name="step-4-add-mappings"></a><span data-ttu-id="bf2c0-154">Steg 4: Lägga till mappningar</span><span class="sxs-lookup"><span data-stu-id="bf2c0-154">Step 4: Add mappings</span></span>
<span data-ttu-id="bf2c0-155">hello lägga till mappningar beror på hello slags Fragmentera karta som du skapade.</span><span class="sxs-lookup"><span data-stu-id="bf2c0-155">hello addition of mappings depends on hello kind of shard map you created.</span></span> <span data-ttu-id="bf2c0-156">Om du har skapat en karta i listan kan du lägga till listan mappningar.</span><span class="sxs-lookup"><span data-stu-id="bf2c0-156">If you created a list map, you add list mappings.</span></span> <span data-ttu-id="bf2c0-157">Om du har skapat en karta intervallet du lägga till mappningar för intervallet.</span><span class="sxs-lookup"><span data-stu-id="bf2c0-157">If you created a range map, you add range mappings.</span></span>

### <a name="option-1-map-hello-data-for-a-list-mapping"></a><span data-ttu-id="bf2c0-158">Alternativ 1: mappa hello data för en lista över-mappning</span><span class="sxs-lookup"><span data-stu-id="bf2c0-158">Option 1: map hello data for a list mapping</span></span>
<span data-ttu-id="bf2c0-159">Mappa hello data genom att lägga till en lista över-mappning för varje klient.</span><span class="sxs-lookup"><span data-stu-id="bf2c0-159">Map hello data by adding a list mapping for each tenant.</span></span>  

    # Create hello mappings and associate it with hello new shards 
    Add-ListMapping 
    -KeyType $([int]) 
    -ListPoint '<tenant_id>' 
    -ListShardMap $ShardMap 
    -SqlServerName '<shard_server_name>' 
    -SqlDatabaseName '<shard_database_name>' 

### <a name="option-2-map-hello-data-for-a-range-mapping"></a><span data-ttu-id="bf2c0-160">Alternativ 2: mappa hello data för mappning av intervallet</span><span class="sxs-lookup"><span data-stu-id="bf2c0-160">Option 2: map hello data for a range mapping</span></span>
<span data-ttu-id="bf2c0-161">Lägg till hello intervallet mappningar för alla hello klient-id-intervallet - kopplingarna till databasen:</span><span class="sxs-lookup"><span data-stu-id="bf2c0-161">Add hello range mappings for all hello tenant id range - database associations:</span></span>

    # Create hello mappings and associate it with hello new shards 
    Add-RangeMapping 
    -KeyType $([int]) 
    -RangeHigh '5' 
    -RangeLow '1' 
    -RangeShardMap $ShardMap 
    -SqlServerName '<shard_server_name>' 
    -SqlDatabaseName '<shard_database_name>' 


### <a name="step-4-option-3-map-hello-data-for-multiple-tenants-on-a-single-database"></a><span data-ttu-id="bf2c0-162">Steg 4 alternativ 3: mappa hello data för flera klienter på en enskild databas</span><span class="sxs-lookup"><span data-stu-id="bf2c0-162">Step 4 option 3: map hello data for multiple tenants on a single database</span></span>
<span data-ttu-id="bf2c0-163">För varje klient kör hello Lägg till ListMapping (alternativ 1, ovan).</span><span class="sxs-lookup"><span data-stu-id="bf2c0-163">For each tenant, run hello Add-ListMapping (option 1, above).</span></span> 

## <a name="checking-hello-mappings"></a><span data-ttu-id="bf2c0-164">Kontrollera hello mappningar</span><span class="sxs-lookup"><span data-stu-id="bf2c0-164">Checking hello mappings</span></span>
<span data-ttu-id="bf2c0-165">Information om hello befintliga delar och hello mappningar som är associerade med dem kan efterfrågas med följande kommandon:</span><span class="sxs-lookup"><span data-stu-id="bf2c0-165">Information about hello existing shards and hello mappings associated with them can be queried using following commands:</span></span>  

    # List hello shards and mappings 
    Get-Shards -ShardMap $ShardMap 
    Get-Mappings -ShardMap $ShardMap 

## <a name="summary"></a><span data-ttu-id="bf2c0-166">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="bf2c0-166">Summary</span></span>
<span data-ttu-id="bf2c0-167">När du har slutfört hello installationen, kan du börja toouse hello-klientbibliotek för elastisk databas.</span><span class="sxs-lookup"><span data-stu-id="bf2c0-167">Once you have completed hello setup, you can begin toouse hello Elastic Database client library.</span></span> <span data-ttu-id="bf2c0-168">Du kan också använda [data beroende routning](sql-database-elastic-scale-data-dependent-routing.md) och [flera Fragmentera frågan](sql-database-elastic-scale-multishard-querying.md).</span><span class="sxs-lookup"><span data-stu-id="bf2c0-168">You can also use [data dependent routing](sql-database-elastic-scale-data-dependent-routing.md) and [multi-shard query](sql-database-elastic-scale-multishard-querying.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="bf2c0-169">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="bf2c0-169">Next steps</span></span>
<span data-ttu-id="bf2c0-170">Hämta hello PowerShell-skript från [Azure SQL DB-elastiska Database tools sripts](https://gallery.technet.microsoft.com/scriptcenter/Azure-SQL-DB-Elastic-731883db).</span><span class="sxs-lookup"><span data-stu-id="bf2c0-170">Get hello PowerShell scripts from [Azure SQL DB-Elastic Database tools sripts](https://gallery.technet.microsoft.com/scriptcenter/Azure-SQL-DB-Elastic-731883db).</span></span>

<span data-ttu-id="bf2c0-171">hello verktyg finns även på GitHub: [Azure/elastisk-db-tools](https://github.com/Azure/elastic-db-tools).</span><span class="sxs-lookup"><span data-stu-id="bf2c0-171">hello tools are also on GitHub: [Azure/elastic-db-tools](https://github.com/Azure/elastic-db-tools).</span></span>

<span data-ttu-id="bf2c0-172">Använd hello dela merge tool toomove data tooor från en modell för flera klienter tooa enda innehavare modell.</span><span class="sxs-lookup"><span data-stu-id="bf2c0-172">Use hello split-merge tool toomove data tooor from a multi-tenant model tooa single tenant model.</span></span> <span data-ttu-id="bf2c0-173">Se [dela merge tool](sql-database-elastic-scale-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="bf2c0-173">See [Split merge tool](sql-database-elastic-scale-get-started.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="bf2c0-174">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="bf2c0-174">Additional resources</span></span>
<span data-ttu-id="bf2c0-175">Information om vanliga mönster för dataarkitekturen i SaaS-databasprogram (Software-as-a-Service) med flera klienter finns i [Designmönster för SaaS-program med flera klienter i Azure SQL Database](sql-database-design-patterns-multi-tenancy-saas-applications.md).</span><span class="sxs-lookup"><span data-stu-id="bf2c0-175">For information on common data architecture patterns of multi-tenant software-as-a-service (SaaS) database applications, see [Design Patterns for Multi-tenant SaaS Applications with Azure SQL Database](sql-database-design-patterns-multi-tenancy-saas-applications.md).</span></span>

## <a name="questions-and-feature-requests"></a><span data-ttu-id="bf2c0-176">Frågor och Funktionsbegäranden</span><span class="sxs-lookup"><span data-stu-id="bf2c0-176">Questions and Feature Requests</span></span>
<span data-ttu-id="bf2c0-177">För frågor kan du nå toous på hello [SQL Database-forum](http://social.msdn.microsoft.com/forums/azure/home?forum=ssdsgetstarted) och för funktionsbegäranden, Lägg till dem toohello [SQL-databas Feedbackforum](https://feedback.azure.com/forums/217321-sql-database/).</span><span class="sxs-lookup"><span data-stu-id="bf2c0-177">For questions, please reach out toous on hello [SQL Database forum](http://social.msdn.microsoft.com/forums/azure/home?forum=ssdsgetstarted) and for feature requests, please add them toohello [SQL Database feedback forum](https://feedback.azure.com/forums/217321-sql-database/).</span></span>

<!--Image references-->
[1]: ./media/sql-database-elastic-convert-to-use-elastic-tools/listmapping.png
[2]: ./media/sql-database-elastic-convert-to-use-elastic-tools/rangemapping.png
[3]: ./media/sql-database-elastic-convert-to-use-elastic-tools/multipleonsingledb.png

