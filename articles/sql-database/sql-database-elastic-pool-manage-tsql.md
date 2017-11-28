---
title: 'T-SQL: Hantera en Azure SQL Database-elastisk pool | Microsoft Docs'
description: "Använda T-SQL toomanage en Azure SQL Database-elastisk pool."
services: sql-database
documentationcenter: 
author: srinia
manager: jhubbard
editor: 
ms.assetid: 4e288e17-bc3e-4255-9fbe-0a2ac0dbd7dd
ms.service: sql-database
ms.custom: DBs & servers
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-management
ms.date: 05/27/2016
ms.author: srinia
ms.openlocfilehash: 666f131b2c88849a1a9ea83381bbc27548e93599
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-and-manage-an-elastic-pool-with-transact-sql"></a><span data-ttu-id="f5aaa-103">Övervaka och hantera en elastisk pool med Transact-SQL</span><span class="sxs-lookup"><span data-stu-id="f5aaa-103">Monitor and manage an elastic pool with Transact-SQL</span></span>
<span data-ttu-id="f5aaa-104">Det här avsnittet beskrivs hur du toomanage skalbara [elastiska pooler](sql-database-elastic-pool.md) med Transact-SQL.</span><span class="sxs-lookup"><span data-stu-id="f5aaa-104">This topic shows you how toomanage scalable [elastic pools](sql-database-elastic-pool.md) with Transact-SQL.</span></span>  <span data-ttu-id="f5aaa-105">Du kan också skapa och hantera en Azure elastisk pool hello [Azure-portalen](https://portal.azure.com/), [PowerShell](sql-database-elastic-pool-manage-powershell.md), hello REST API eller [C#](sql-database-elastic-pool-manage-csharp.md).</span><span class="sxs-lookup"><span data-stu-id="f5aaa-105">You can also create and manage an Azure elastic pool hello [Azure portal](https://portal.azure.com/), [PowerShell](sql-database-elastic-pool-manage-powershell.md), hello REST API, or [C#](sql-database-elastic-pool-manage-csharp.md).</span></span> <span data-ttu-id="f5aaa-106">Du kan också skapa och flytta databaser till och från elastiska pooler med [Transact-SQL](sql-database-elastic-pool-manage-tsql.md).</span><span class="sxs-lookup"><span data-stu-id="f5aaa-106">You can also create and move databases into and out of elastic pools using [Transact-SQL](sql-database-elastic-pool-manage-tsql.md).</span></span>


<span data-ttu-id="f5aaa-107">Använd hello [Create Database (Azure SQL Database)](https://msdn.microsoft.com/library/dn268335.aspx) och [Alter Database(Azure SQL Database)](https://msdn.microsoft.com/library/mt574871.aspx) kommandon toocreate och flytta databaser till och från elastiska pooler.</span><span class="sxs-lookup"><span data-stu-id="f5aaa-107">Use hello [Create Database (Azure SQL Database)](https://msdn.microsoft.com/library/dn268335.aspx) and [Alter Database(Azure SQL Database)](https://msdn.microsoft.com/library/mt574871.aspx) commands toocreate and move databases into and out of elastic pools.</span></span> <span data-ttu-id="f5aaa-108">hello elastiska poolen måste finnas innan du kan använda dessa kommandon.</span><span class="sxs-lookup"><span data-stu-id="f5aaa-108">hello elastic pool must exist before you can use these commands.</span></span> <span data-ttu-id="f5aaa-109">Dessa kommandon påverkar endast databaser.</span><span class="sxs-lookup"><span data-stu-id="f5aaa-109">These commands affect only databases.</span></span> <span data-ttu-id="f5aaa-110">Skapandet av nya pooler och hello-inställningarna för pool-egenskaper (till exempel min och max edtu: er) kan inte ändras med T-SQL-kommandon.</span><span class="sxs-lookup"><span data-stu-id="f5aaa-110">Creation of new pools and hello setting of pool properties (such as min and max eDTUs) cannot be changed with T-SQL commands.</span></span>

## <a name="create-a-pooled-database-in-an-elastic-pool"></a><span data-ttu-id="f5aaa-111">Skapa en delad databas i en elastisk pool</span><span class="sxs-lookup"><span data-stu-id="f5aaa-111">Create a pooled database in an elastic pool</span></span>
<span data-ttu-id="f5aaa-112">Använd hello CREATE DATABASE-kommandot med hello SERVICE_OBJECTIVE alternativet.</span><span class="sxs-lookup"><span data-stu-id="f5aaa-112">Use hello CREATE DATABASE command with hello SERVICE_OBJECTIVE option.</span></span>   

    CREATE DATABASE db1 ( SERVICE_OBJECTIVE = ELASTIC_POOL (name = [S3M100] ));
    -- Create a database named db1 in an elastic named S3M100.

<span data-ttu-id="f5aaa-113">Alla databaser i en elastisk pool ärver hello tjänstnivån för hello elastiska poolen (Basic, Standard, Premium).</span><span class="sxs-lookup"><span data-stu-id="f5aaa-113">All databases in an elastic pool inherit hello service tier of hello elastic pool (Basic, Standard, Premium).</span></span> 

## <a name="move-a-database-between-elastic-pools"></a><span data-ttu-id="f5aaa-114">Flytta en databas mellan elastiska pooler</span><span class="sxs-lookup"><span data-stu-id="f5aaa-114">Move a database between elastic pools</span></span>
<span data-ttu-id="f5aaa-115">Använd hello ALTER DATABASE-kommandot med hello ändra och ange tjänsten\_mål alternativet som ELASTISK\_POOL.</span><span class="sxs-lookup"><span data-stu-id="f5aaa-115">Use hello ALTER DATABASE command with hello MODIFY and set SERVICE\_OBJECTIVE option as ELASTIC\_POOL.</span></span> <span data-ttu-id="f5aaa-116">Ange hello namn toohello namnet på hello målpool.</span><span class="sxs-lookup"><span data-stu-id="f5aaa-116">Set hello name toohello name of hello target pool.</span></span>

    ALTER DATABASE db1 MODIFY ( SERVICE_OBJECTIVE = ELASTIC_POOL (name = [PM125] ));
    -- Move hello database named db1 tooan elastic named P1M125  

## <a name="move-a-database-into-an-elastic-pool"></a><span data-ttu-id="f5aaa-117">Flytta en databas till en elastisk pool</span><span class="sxs-lookup"><span data-stu-id="f5aaa-117">Move a database into an elastic pool</span></span>
<span data-ttu-id="f5aaa-118">Använd hello ALTER DATABASE-kommandot med hello ändra och ange tjänsten\_mål alternativet som ELASTIC_POOL.</span><span class="sxs-lookup"><span data-stu-id="f5aaa-118">Use hello ALTER DATABASE command with hello MODIFY and set SERVICE\_OBJECTIVE option as ELASTIC_POOL.</span></span> <span data-ttu-id="f5aaa-119">Ange hello namn toohello namnet på hello målpool.</span><span class="sxs-lookup"><span data-stu-id="f5aaa-119">Set hello name toohello name of hello target pool.</span></span>

    ALTER DATABASE db1 MODIFY ( SERVICE_OBJECTIVE = ELASTIC_POOL (name = [S3100] ));
    -- Move hello database named db1 tooan elastic named S3100.

## <a name="move-a-database-out-of-an-elastic-pool"></a><span data-ttu-id="f5aaa-120">Flytta en databas från en elastisk pool</span><span class="sxs-lookup"><span data-stu-id="f5aaa-120">Move a database out of an elastic pool</span></span>
<span data-ttu-id="f5aaa-121">Använda hello ALTER DATABASE-kommandot och ange hello SERVICE_OBJECTIVE tooone av hello prestandanivåer (till exempel S0 eller S1).</span><span class="sxs-lookup"><span data-stu-id="f5aaa-121">Use hello ALTER DATABASE command and set hello SERVICE_OBJECTIVE tooone of hello performance levels (such as S0 or S1).</span></span>

    ALTER DATABASE db1 MODIFY ( SERVICE_OBJECTIVE = 'S1');
    -- Changes hello database into a stand-alone database with hello service objective S1.

## <a name="list-databases-in-an-elastic-pool"></a><span data-ttu-id="f5aaa-122">Lista över databaser i en elastisk pool</span><span class="sxs-lookup"><span data-stu-id="f5aaa-122">List databases in an elastic pool</span></span>
<span data-ttu-id="f5aaa-123">Använd hello [sys.database\_service \_mål visa](https://msdn.microsoft.com/library/mt712619) toolist alla hello databaser i en elastisk pool.</span><span class="sxs-lookup"><span data-stu-id="f5aaa-123">Use hello [sys.database\_service \_objectives view](https://msdn.microsoft.com/library/mt712619) toolist all hello databases in an elastic pool.</span></span> <span data-ttu-id="f5aaa-124">Logga in toohello master databasvyn tooquery hello.</span><span class="sxs-lookup"><span data-stu-id="f5aaa-124">Log in toohello master database tooquery hello view.</span></span>

    SELECT d.name, slo.*  
    FROM sys.databases d 
    JOIN sys.database_service_objectives slo  
    ON d.database_id = slo.database_id
    WHERE elastic_pool_name = 'MyElasticPool'; 

## <a name="get-resource-usage-data-for-an-elastic-pool"></a><span data-ttu-id="f5aaa-125">Hämta resursanvändningsdata för en elastisk pool</span><span class="sxs-lookup"><span data-stu-id="f5aaa-125">Get resource usage data for an elastic pool</span></span>
<span data-ttu-id="f5aaa-126">Använd hello [sys.elastic\_pool \_resurs \_stats visa](https://msdn.microsoft.com/library/mt280062.aspx) tooexamine hello resurs användningsstatistik för en elastisk pool på en logisk server.</span><span class="sxs-lookup"><span data-stu-id="f5aaa-126">Use hello [sys.elastic\_pool \_resource \_stats view](https://msdn.microsoft.com/library/mt280062.aspx) tooexamine hello resource usage statistics of an elastic pool on a logical server.</span></span> <span data-ttu-id="f5aaa-127">Logga in toohello master databasvyn tooquery hello.</span><span class="sxs-lookup"><span data-stu-id="f5aaa-127">Log in toohello master database tooquery hello view.</span></span>

    SELECT * FROM sys.elastic_pool_resource_stats 
    WHERE elastic_pool_name = 'MyElasticPool'
    ORDER BY end_time DESC;

## <a name="get-resource-usage-for-a-pooled-database"></a><span data-ttu-id="f5aaa-128">Hämta resursanvändningen för en delad databas</span><span class="sxs-lookup"><span data-stu-id="f5aaa-128">Get resource usage for a pooled database</span></span>
<span data-ttu-id="f5aaa-129">Använd hello [sys.dm\_ db\_ resurs\_stats visa](https://msdn.microsoft.com/library/dn800981.aspx) eller [sys.resource \_stats visa](https://msdn.microsoft.com/library/dn269979.aspx) tooexamine hello resurs användningsstatistik för en databas i en elastisk pool.</span><span class="sxs-lookup"><span data-stu-id="f5aaa-129">Use hello [sys.dm\_ db\_ resource\_stats view](https://msdn.microsoft.com/library/dn800981.aspx) or [sys.resource \_stats view](https://msdn.microsoft.com/library/dn269979.aspx) tooexamine hello resource usage statistics of a database in an elastic pool.</span></span> <span data-ttu-id="f5aaa-130">Den här processen är liknande tooquerying resursanvändningen för en enskild databas.</span><span class="sxs-lookup"><span data-stu-id="f5aaa-130">This process is similar tooquerying resource usage for a single database.</span></span>

## <a name="next-steps"></a><span data-ttu-id="f5aaa-131">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="f5aaa-131">Next steps</span></span>
<span data-ttu-id="f5aaa-132">Du kan hantera elastiska databaser i poolen hello genom att skapa elastiska jobb när du har skapat en elastisk pool.</span><span class="sxs-lookup"><span data-stu-id="f5aaa-132">After creating an elastic pool, you can manage elastic databases in hello pool by creating elastic jobs.</span></span> <span data-ttu-id="f5aaa-133">Elastiska jobb underlätta köra T-SQL-skript mot valfritt antal databaser i hello pool.</span><span class="sxs-lookup"><span data-stu-id="f5aaa-133">Elastic jobs facilitate running T-SQL scripts against any number of databases in hello pool.</span></span> <span data-ttu-id="f5aaa-134">Mer information finns i [elastisk databas översikt över](sql-database-elastic-jobs-overview.md).</span><span class="sxs-lookup"><span data-stu-id="f5aaa-134">For more information, see [Elastic database jobs overview](sql-database-elastic-jobs-overview.md).</span></span> 

<span data-ttu-id="f5aaa-135">Se [skala ut med Azure SQL Database](sql-database-elastic-scale-introduction.md): Använd elastisk databas verktyg tooscale ut, flytta data, fråga eller skapa transaktioner.</span><span class="sxs-lookup"><span data-stu-id="f5aaa-135">See [Scaling out with Azure SQL Database](sql-database-elastic-scale-introduction.md): use elastic database tools tooscale out, move data, query, or create transactions.</span></span>

