---
title: Migrera befintliga databaser att skala ut | Microsoft Docs
description: "Konvertera delat databaser för att använda elastisk Databasverktyg genom att skapa en Fragmentera kartan manager"
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
ms.openlocfilehash: 099f40d00753b7c86ba726a818f17d440a125221
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="migrate-existing-databases-to-scale-out"></a><span data-ttu-id="a0e54-103">Migrera befintliga databaser för att skala ut</span><span class="sxs-lookup"><span data-stu-id="a0e54-103">Migrate existing databases to scale-out</span></span>
<span data-ttu-id="a0e54-104">Enkelt hantera dina befintliga delat utskalat-databaser som använder Azure SQL Database databaser (som den [klientbibliotek för elastisk databas](sql-database-elastic-database-client-library.md)).</span><span class="sxs-lookup"><span data-stu-id="a0e54-104">Easily manage your existing scaled-out sharded databases using Azure SQL Database database tools (such as the [Elastic Database client library](sql-database-elastic-database-client-library.md)).</span></span> <span data-ttu-id="a0e54-105">Först måste du konvertera en befintlig uppsättning databaser som ska användas i [Fragmentera kartan manager](sql-database-elastic-scale-shard-map-management.md).</span><span class="sxs-lookup"><span data-stu-id="a0e54-105">You must first convert an existing set of databases to use the [shard map manager](sql-database-elastic-scale-shard-map-management.md).</span></span> 

## <a name="overview"></a><span data-ttu-id="a0e54-106">Översikt</span><span class="sxs-lookup"><span data-stu-id="a0e54-106">Overview</span></span>
<span data-ttu-id="a0e54-107">För att migrera en befintlig delat databas.</span><span class="sxs-lookup"><span data-stu-id="a0e54-107">To migrate an existing sharded database:</span></span> 

1. <span data-ttu-id="a0e54-108">Förbereda den [Fragmentera kartan manager-databasen](sql-database-elastic-scale-shard-map-management.md).</span><span class="sxs-lookup"><span data-stu-id="a0e54-108">Prepare the [shard map manager database](sql-database-elastic-scale-shard-map-management.md).</span></span>
2. <span data-ttu-id="a0e54-109">Skapa Fragmentera kartan.</span><span class="sxs-lookup"><span data-stu-id="a0e54-109">Create the shard map.</span></span>
3. <span data-ttu-id="a0e54-110">Förbered enskilda delar.</span><span class="sxs-lookup"><span data-stu-id="a0e54-110">Prepare the individual shards.</span></span>  
4. <span data-ttu-id="a0e54-111">Lägga till avbildningar Fragmentera kartan.</span><span class="sxs-lookup"><span data-stu-id="a0e54-111">Add mappings to the shard map.</span></span>

<span data-ttu-id="a0e54-112">Dessa tekniker kan implementeras med hjälp av antingen den [klientbiblioteket för .NET Framework](http://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/), eller PowerShell-skript som finns på [Azure SQL DB - skript för elastisk databas verktyg](https://gallery.technet.microsoft.com/scriptcenter/Azure-SQL-DB-Elastic-731883db).</span><span class="sxs-lookup"><span data-stu-id="a0e54-112">These techniques can be implemented using either the [.NET Framework client library](http://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/), or the PowerShell scripts found at [Azure SQL DB - Elastic Database tools scripts](https://gallery.technet.microsoft.com/scriptcenter/Azure-SQL-DB-Elastic-731883db).</span></span> <span data-ttu-id="a0e54-113">De här exemplen använder PowerShell-skript.</span><span class="sxs-lookup"><span data-stu-id="a0e54-113">The examples here use the PowerShell scripts.</span></span>

<span data-ttu-id="a0e54-114">Läs mer om ShardMapManager [Fragmentera kartan management](sql-database-elastic-scale-shard-map-management.md).</span><span class="sxs-lookup"><span data-stu-id="a0e54-114">For more information about the ShardMapManager, see [Shard map management](sql-database-elastic-scale-shard-map-management.md).</span></span> <span data-ttu-id="a0e54-115">En översikt över elastisk Databasverktyg finns [elastisk databas funktionsöversikt](sql-database-elastic-scale-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="a0e54-115">For an overview of the elastic database tools, see [Elastic Database features overview](sql-database-elastic-scale-introduction.md).</span></span>

## <a name="prepare-the-shard-map-manager-database"></a><span data-ttu-id="a0e54-116">Förbereda Fragmentera kartan manager-databasen</span><span class="sxs-lookup"><span data-stu-id="a0e54-116">Prepare the shard map manager database</span></span>
<span data-ttu-id="a0e54-117">Fragmentera kartan manager är en särskild databas som innehåller data för att hantera databaser som skalats ut.</span><span class="sxs-lookup"><span data-stu-id="a0e54-117">The shard map manager is a special database that contains the data to manage scaled-out databases.</span></span> <span data-ttu-id="a0e54-118">Du kan använda en befintlig databas eller skapa en ny databas.</span><span class="sxs-lookup"><span data-stu-id="a0e54-118">You can use an existing database, or create a new database.</span></span> <span data-ttu-id="a0e54-119">Observera att en databas som fungerar som Fragmentera kartan manager inte får vara samma databas som en Fragmentera.</span><span class="sxs-lookup"><span data-stu-id="a0e54-119">Note that a database acting as shard map manager should not be the same database as a shard.</span></span> <span data-ttu-id="a0e54-120">Observera också att PowerShell-skriptet inte skapa databasen för dig.</span><span class="sxs-lookup"><span data-stu-id="a0e54-120">Also note that the PowerShell script does not create the database for you.</span></span> 

## <a name="step-1-create-a-shard-map-manager"></a><span data-ttu-id="a0e54-121">Steg 1: skapa en Fragmentera kartan manager</span><span class="sxs-lookup"><span data-stu-id="a0e54-121">Step 1: create a shard map manager</span></span>
    # Create a shard map manager. 
    New-ShardMapManager -UserName '<user_name>' 
    -Password '<password>' 
    -SqlServerName '<server_name>' 
    -SqlDatabaseName '<smm_db_name>' 
    #<server_name> and <smm_db_name> are the server name and database name 
    # for the new or existing database that should be used for storing 
    # tenant-database mapping information.

### <a name="to-retrieve-the-shard-map-manager"></a><span data-ttu-id="a0e54-122">Att hämta Fragmentera kartan manager</span><span class="sxs-lookup"><span data-stu-id="a0e54-122">To retrieve the shard map manager</span></span>
<span data-ttu-id="a0e54-123">När den har skapats, kan du hämta hanteraren Fragmentera karta med denna cmdlet.</span><span class="sxs-lookup"><span data-stu-id="a0e54-123">After creation, you can retrieve the shard map manager with this cmdlet.</span></span> <span data-ttu-id="a0e54-124">Det här steget krävs varje gång du behöver använda ShardMapManager-objektet.</span><span class="sxs-lookup"><span data-stu-id="a0e54-124">This step is needed every time you need to use the ShardMapManager object.</span></span>

    # Try to get a reference to the Shard Map Manager  
    $ShardMapManager = Get-ShardMapManager -UserName '<user_name>' 
    -Password '<password>' 
    -SqlServerName '<server_name>' 
    -SqlDatabaseName '<smm_db_name>' 


## <a name="step-2-create-the-shard-map"></a><span data-ttu-id="a0e54-125">Steg 2: skapa Fragmentera kartan</span><span class="sxs-lookup"><span data-stu-id="a0e54-125">Step 2: create the shard map</span></span>
<span data-ttu-id="a0e54-126">Du måste välja vilken typ av Fragmentera kartan för att skapa.</span><span class="sxs-lookup"><span data-stu-id="a0e54-126">You must select the type of shard map to create.</span></span> <span data-ttu-id="a0e54-127">Valet är beroende av databasens arkitektur:</span><span class="sxs-lookup"><span data-stu-id="a0e54-127">The choice depends on the database architecture:</span></span> 

1. <span data-ttu-id="a0e54-128">En organisation per databas (villkor, finns det [ordlista](sql-database-elastic-scale-glossary.md).)</span><span class="sxs-lookup"><span data-stu-id="a0e54-128">Single tenant per database (For terms, see the [glossary](sql-database-elastic-scale-glossary.md).)</span></span> 
2. <span data-ttu-id="a0e54-129">Flera klienter per databas (två typer):</span><span class="sxs-lookup"><span data-stu-id="a0e54-129">Multiple tenants per database (two types):</span></span>
   1. <span data-ttu-id="a0e54-130">Lista över-mappning</span><span class="sxs-lookup"><span data-stu-id="a0e54-130">List mapping</span></span>
   2. <span data-ttu-id="a0e54-131">Mappning av intervallet</span><span class="sxs-lookup"><span data-stu-id="a0e54-131">Range mapping</span></span>

<span data-ttu-id="a0e54-132">En enskild klient-modell, skapa en **lista mappning** Fragmentera kartan.</span><span class="sxs-lookup"><span data-stu-id="a0e54-132">For a single-tenant model, create a **list mapping** shard map.</span></span> <span data-ttu-id="a0e54-133">Stöd för en innehavare modellen tilldelar en databas per klient.</span><span class="sxs-lookup"><span data-stu-id="a0e54-133">The single-tenant model assigns one database per tenant.</span></span> <span data-ttu-id="a0e54-134">Detta är en effektiv modell för SaaS-utvecklare som det förenklar hanteringen.</span><span class="sxs-lookup"><span data-stu-id="a0e54-134">This is an effective model for SaaS developers as it simplifies management.</span></span>

![Lista över-mappning][1]

<span data-ttu-id="a0e54-136">Modell för flera klienter tilldelas en enskild databas flera innehavare (och du kan distribuera grupper av klienter över flera databaser).</span><span class="sxs-lookup"><span data-stu-id="a0e54-136">The multi-tenant model assigns several tenants to a single database (and you can distribute groups of tenants across multiple databases).</span></span> <span data-ttu-id="a0e54-137">Använd den här modellen när du förväntar dig varje innehavare har liten databehov.</span><span class="sxs-lookup"><span data-stu-id="a0e54-137">Use this model when you expect each tenant to have small data needs.</span></span> <span data-ttu-id="a0e54-138">I den här modellen vi tilldela en uppsättning klienter till en databas med hjälp av **intervallet mappning**.</span><span class="sxs-lookup"><span data-stu-id="a0e54-138">In this model, we assign a range of tenants to a database using **range mapping**.</span></span> 

![Mappning av intervallet][2]

<span data-ttu-id="a0e54-140">Eller du kan implementera en databas för flera innehavare modellen med hjälp av en *lista mappning* tilldela flera klienter till en enskild databas.</span><span class="sxs-lookup"><span data-stu-id="a0e54-140">Or you can implement a multi-tenant database model using a *list mapping* to assign multiple tenants to a single database.</span></span> <span data-ttu-id="a0e54-141">Till exempel DB1 används för att lagra information om klient-id 1 och 5 och DB2 lagrar data för klienten 7 och 10-klient.</span><span class="sxs-lookup"><span data-stu-id="a0e54-141">For example, DB1 is used to store information about tenant id 1 and 5, and DB2 stores data for tenant 7 and tenant 10.</span></span> 

![Muliple klienter i samma DB][3] 

<span data-ttu-id="a0e54-143">**Baserat på ditt val, välja ett av följande alternativ:**</span><span class="sxs-lookup"><span data-stu-id="a0e54-143">**Based on your choice, choose one of these options:**</span></span>

### <a name="option-1-create-a-shard-map-for-a-list-mapping"></a><span data-ttu-id="a0e54-144">Alternativ 1: skapa en Fragmentera karta för en lista över-mappning</span><span class="sxs-lookup"><span data-stu-id="a0e54-144">Option 1: create a shard map for a list mapping</span></span>
<span data-ttu-id="a0e54-145">Skapa en Fragmentera karta med ShardMapManager-objektet.</span><span class="sxs-lookup"><span data-stu-id="a0e54-145">Create a shard map using the ShardMapManager object.</span></span> 

    # $ShardMapManager is the shard map manager object. 
    $ShardMap = New-ListShardMap -KeyType $([int]) 
    -ListShardMapName 'ListShardMap' 
    -ShardMapManager $ShardMapManager 


### <a name="option-2-create-a-shard-map-for-a-range-mapping"></a><span data-ttu-id="a0e54-146">Alternativ 2: skapa en Fragmentera karta för mappning av intervallet</span><span class="sxs-lookup"><span data-stu-id="a0e54-146">Option 2: create a shard map for a range mapping</span></span>
<span data-ttu-id="a0e54-147">Observera att om du vill använda den här mappningen mönster, klient-id värden måste vara sammanhängande intervall och det är acceptabelt att ha lucka i intervall genom att helt enkelt hoppa över intervallet när du skapar databaserna.</span><span class="sxs-lookup"><span data-stu-id="a0e54-147">Note that to utilize this mapping pattern, tenant id values needs to be continuous ranges, and it is acceptable to have gap in the ranges by simply skipping the range when creating the databases.</span></span>

    # $ShardMapManager is the shard map manager object 
    # 'RangeShardMap' is the unique identifier for the range shard map.  
    $ShardMap = New-RangeShardMap 
    -KeyType $([int]) 
    -RangeShardMapName 'RangeShardMap' 
    -ShardMapManager $ShardMapManager 

### <a name="option-3-list-mappings-on-a-single-database"></a><span data-ttu-id="a0e54-148">Alternativ 3: Visa en lista över mappningar på en enskild databas</span><span class="sxs-lookup"><span data-stu-id="a0e54-148">Option 3: List mappings on a single database</span></span>
<span data-ttu-id="a0e54-149">Ställa in det här mönstret kräver också att skapa en karta i listan som visas i steg 2, alternativ 1.</span><span class="sxs-lookup"><span data-stu-id="a0e54-149">Setting up this pattern also requires creation of a list map as shown in step 2, option 1.</span></span>

## <a name="step-3-prepare-individual-shards"></a><span data-ttu-id="a0e54-150">Steg 3: Förbered enskilda delar</span><span class="sxs-lookup"><span data-stu-id="a0e54-150">Step 3: Prepare individual shards</span></span>
<span data-ttu-id="a0e54-151">Lägg till varje Fragmentera (databasen) hanteraren Fragmentera kartan.</span><span class="sxs-lookup"><span data-stu-id="a0e54-151">Add each shard (database) to the shard map manager.</span></span> <span data-ttu-id="a0e54-152">Detta förbereder enskilda databaser för att lagra mappningsinformation.</span><span class="sxs-lookup"><span data-stu-id="a0e54-152">This prepares the individual databases for storing mapping information.</span></span> <span data-ttu-id="a0e54-153">Köra den här metoden på varje Fragmentera.</span><span class="sxs-lookup"><span data-stu-id="a0e54-153">Execute this method on each shard.</span></span>

    Add-Shard 
    -ShardMap $ShardMap 
    -SqlServerName '<shard_server_name>' 
    -SqlDatabaseName '<shard_database_name>'
    # The $ShardMap is the shard map created in step 2.


## <a name="step-4-add-mappings"></a><span data-ttu-id="a0e54-154">Steg 4: Lägga till mappningar</span><span class="sxs-lookup"><span data-stu-id="a0e54-154">Step 4: Add mappings</span></span>
<span data-ttu-id="a0e54-155">För att lägga till mappningar beror på vilken typ av Fragmentera karta som du skapade.</span><span class="sxs-lookup"><span data-stu-id="a0e54-155">The addition of mappings depends on the kind of shard map you created.</span></span> <span data-ttu-id="a0e54-156">Om du har skapat en karta i listan kan du lägga till listan mappningar.</span><span class="sxs-lookup"><span data-stu-id="a0e54-156">If you created a list map, you add list mappings.</span></span> <span data-ttu-id="a0e54-157">Om du har skapat en karta intervallet du lägga till mappningar för intervallet.</span><span class="sxs-lookup"><span data-stu-id="a0e54-157">If you created a range map, you add range mappings.</span></span>

### <a name="option-1-map-the-data-for-a-list-mapping"></a><span data-ttu-id="a0e54-158">Alternativ 1: mappa data för en lista över-mappning</span><span class="sxs-lookup"><span data-stu-id="a0e54-158">Option 1: map the data for a list mapping</span></span>
<span data-ttu-id="a0e54-159">Mappa dina data genom att lägga till en lista över-mappning för varje klient.</span><span class="sxs-lookup"><span data-stu-id="a0e54-159">Map the data by adding a list mapping for each tenant.</span></span>  

    # Create the mappings and associate it with the new shards 
    Add-ListMapping 
    -KeyType $([int]) 
    -ListPoint '<tenant_id>' 
    -ListShardMap $ShardMap 
    -SqlServerName '<shard_server_name>' 
    -SqlDatabaseName '<shard_database_name>' 

### <a name="option-2-map-the-data-for-a-range-mapping"></a><span data-ttu-id="a0e54-160">Alternativ 2: mappa data för mappning av intervallet</span><span class="sxs-lookup"><span data-stu-id="a0e54-160">Option 2: map the data for a range mapping</span></span>
<span data-ttu-id="a0e54-161">Lägg till intervallet mappningar för alla klient-id intervallet - kopplingarna till databasen:</span><span class="sxs-lookup"><span data-stu-id="a0e54-161">Add the range mappings for all the tenant id range - database associations:</span></span>

    # Create the mappings and associate it with the new shards 
    Add-RangeMapping 
    -KeyType $([int]) 
    -RangeHigh '5' 
    -RangeLow '1' 
    -RangeShardMap $ShardMap 
    -SqlServerName '<shard_server_name>' 
    -SqlDatabaseName '<shard_database_name>' 


### <a name="step-4-option-3-map-the-data-for-multiple-tenants-on-a-single-database"></a><span data-ttu-id="a0e54-162">Steg 4 alternativ 3: mappa data för flera klienter på en enskild databas</span><span class="sxs-lookup"><span data-stu-id="a0e54-162">Step 4 option 3: map the data for multiple tenants on a single database</span></span>
<span data-ttu-id="a0e54-163">För varje klient kör du Add-ListMapping (alternativ 1, ovan).</span><span class="sxs-lookup"><span data-stu-id="a0e54-163">For each tenant, run the Add-ListMapping (option 1, above).</span></span> 

## <a name="checking-the-mappings"></a><span data-ttu-id="a0e54-164">Kontrollera mappningar</span><span class="sxs-lookup"><span data-stu-id="a0e54-164">Checking the mappings</span></span>
<span data-ttu-id="a0e54-165">Information om befintliga delar och mappningar som är associerade med dem kan efterfrågas med följande kommandon:</span><span class="sxs-lookup"><span data-stu-id="a0e54-165">Information about the existing shards and the mappings associated with them can be queried using following commands:</span></span>  

    # List the shards and mappings 
    Get-Shards -ShardMap $ShardMap 
    Get-Mappings -ShardMap $ShardMap 

## <a name="summary"></a><span data-ttu-id="a0e54-166">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="a0e54-166">Summary</span></span>
<span data-ttu-id="a0e54-167">När du har slutfört installationen, kan du börja använda klientbibliotek för elastisk databas.</span><span class="sxs-lookup"><span data-stu-id="a0e54-167">Once you have completed the setup, you can begin to use the Elastic Database client library.</span></span> <span data-ttu-id="a0e54-168">Du kan också använda [data beroende routning](sql-database-elastic-scale-data-dependent-routing.md) och [flera Fragmentera frågan](sql-database-elastic-scale-multishard-querying.md).</span><span class="sxs-lookup"><span data-stu-id="a0e54-168">You can also use [data dependent routing](sql-database-elastic-scale-data-dependent-routing.md) and [multi-shard query](sql-database-elastic-scale-multishard-querying.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="a0e54-169">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="a0e54-169">Next steps</span></span>
<span data-ttu-id="a0e54-170">Hämta PowerShell-skript från [Azure SQL DB-elastiska Database tools sripts](https://gallery.technet.microsoft.com/scriptcenter/Azure-SQL-DB-Elastic-731883db).</span><span class="sxs-lookup"><span data-stu-id="a0e54-170">Get the PowerShell scripts from [Azure SQL DB-Elastic Database tools sripts](https://gallery.technet.microsoft.com/scriptcenter/Azure-SQL-DB-Elastic-731883db).</span></span>

<span data-ttu-id="a0e54-171">Verktygen finns också på GitHub: [Azure/elastisk-db-tools](https://github.com/Azure/elastic-db-tools).</span><span class="sxs-lookup"><span data-stu-id="a0e54-171">The tools are also on GitHub: [Azure/elastic-db-tools](https://github.com/Azure/elastic-db-tools).</span></span>

<span data-ttu-id="a0e54-172">Använda verktyget delade dokument för att flytta data till eller från en modell för flera klienter till en enskild klient-modell.</span><span class="sxs-lookup"><span data-stu-id="a0e54-172">Use the split-merge tool to move data to or from a multi-tenant model to a single tenant model.</span></span> <span data-ttu-id="a0e54-173">Se [dela merge tool](sql-database-elastic-scale-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="a0e54-173">See [Split merge tool](sql-database-elastic-scale-get-started.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="a0e54-174">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="a0e54-174">Additional resources</span></span>
<span data-ttu-id="a0e54-175">Information om vanliga mönster för dataarkitekturen i SaaS-databasprogram (Software-as-a-Service) med flera klienter finns i [Designmönster för SaaS-program med flera klienter i Azure SQL Database](sql-database-design-patterns-multi-tenancy-saas-applications.md).</span><span class="sxs-lookup"><span data-stu-id="a0e54-175">For information on common data architecture patterns of multi-tenant software-as-a-service (SaaS) database applications, see [Design Patterns for Multi-tenant SaaS Applications with Azure SQL Database](sql-database-design-patterns-multi-tenancy-saas-applications.md).</span></span>

## <a name="questions-and-feature-requests"></a><span data-ttu-id="a0e54-176">Frågor och Funktionsbegäranden</span><span class="sxs-lookup"><span data-stu-id="a0e54-176">Questions and Feature Requests</span></span>
<span data-ttu-id="a0e54-177">För frågor kan du nå oss på den [SQL Database-forum](http://social.msdn.microsoft.com/forums/azure/home?forum=ssdsgetstarted) och för funktionsbegäranden, Lägg till dem till den [SQL-databas Feedbackforum](https://feedback.azure.com/forums/217321-sql-database/).</span><span class="sxs-lookup"><span data-stu-id="a0e54-177">For questions, please reach out to us on the [SQL Database forum](http://social.msdn.microsoft.com/forums/azure/home?forum=ssdsgetstarted) and for feature requests, please add them to the [SQL Database feedback forum](https://feedback.azure.com/forums/217321-sql-database/).</span></span>

<!--Image references-->
[1]: ./media/sql-database-elastic-convert-to-use-elastic-tools/listmapping.png
[2]: ./media/sql-database-elastic-convert-to-use-elastic-tools/rangemapping.png
[3]: ./media/sql-database-elastic-convert-to-use-elastic-tools/multipleonsingledb.png

