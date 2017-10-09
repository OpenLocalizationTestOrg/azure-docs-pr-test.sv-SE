---
title: aaaElastic databasen verktyg ordlista | Microsoft Docs
description: "Förklaring av termer som används för elastiska Databasverktyg"
services: sql-database
documentationcenter: 
manager: jhubbard
author: ddove
editor: 
ms.assetid: a23a4e81-6706-452d-afc1-a550e5e47af9
ms.service: sql-database
ms.custom: scale out apps
ms.workload: sql-database
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/24/2016
ms.author: ddove
ms.openlocfilehash: d6573aad9a097e07135b0a64d1dafec19bb8cc7c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="elastic-database-tools-glossary"></a><span data-ttu-id="b3f1b-103">Ordlista för verktyg för elastisk databas</span><span class="sxs-lookup"><span data-stu-id="b3f1b-103">Elastic Database tools glossary</span></span>
<span data-ttu-id="b3f1b-104">hello följande villkor definieras för hello [elastisk Databasverktyg](sql-database-elastic-scale-introduction.md), en funktion i Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="b3f1b-104">hello following terms are defined for hello [Elastic Database tools](sql-database-elastic-scale-introduction.md), a feature of Azure SQL Database.</span></span> <span data-ttu-id="b3f1b-105">hello verktyg är används toomanage [Fragmentera mappar](sql-database-elastic-scale-shard-map-management.md), och inkludera hello [klientbiblioteket](sql-database-elastic-database-client-library.md), hello [för delade sökvägssammanslagning](sql-database-elastic-scale-overview-split-and-merge.md), [elastiska pooler](sql-database-elastic-pool.md), och [frågor](sql-database-elastic-query-overview.md).</span><span class="sxs-lookup"><span data-stu-id="b3f1b-105">hello tools are used toomanage [shard maps](sql-database-elastic-scale-shard-map-management.md), and include hello [client library](sql-database-elastic-database-client-library.md), hello [split-merge tool](sql-database-elastic-scale-overview-split-and-merge.md), [elastic pools](sql-database-elastic-pool.md), and [queries](sql-database-elastic-query-overview.md).</span></span> 

<span data-ttu-id="b3f1b-106">Dessa villkor används i [att lägga till en Fragmentera med elastiska Databasverktyg](sql-database-elastic-scale-add-a-shard.md) och [med hello RecoveryManager klassen toofix Fragmentera kartan problem](sql-database-elastic-database-recovery-manager.md).</span><span class="sxs-lookup"><span data-stu-id="b3f1b-106">These terms are used in [Adding a shard using Elastic Database tools](sql-database-elastic-scale-add-a-shard.md) and [Using hello RecoveryManager class toofix shard map problems](sql-database-elastic-database-recovery-manager.md).</span></span>

![Elastisk skala villkor][1]

<span data-ttu-id="b3f1b-108">**Databasen**: en Azure SQL-databas.</span><span class="sxs-lookup"><span data-stu-id="b3f1b-108">**Database**: An Azure SQL database.</span></span> 

<span data-ttu-id="b3f1b-109">**Data beroende routning**: hello funktioner som gör att ett program tooconnect tooa Fragmentera ges en specifik horisontell partitionering nyckel.</span><span class="sxs-lookup"><span data-stu-id="b3f1b-109">**Data dependent routing**: hello functionality that enables an application tooconnect tooa shard given a specific sharding key.</span></span> <span data-ttu-id="b3f1b-110">Se [Data beroende routning](sql-database-elastic-scale-data-dependent-routing.md).</span><span class="sxs-lookup"><span data-stu-id="b3f1b-110">See [Data dependent routing](sql-database-elastic-scale-data-dependent-routing.md).</span></span> <span data-ttu-id="b3f1b-111">Jämför för**[flera Fragmentera frågan](sql-database-elastic-scale-multishard-querying.md)**.</span><span class="sxs-lookup"><span data-stu-id="b3f1b-111">Compare too**[Multi-Shard Query](sql-database-elastic-scale-multishard-querying.md)**.</span></span>

<span data-ttu-id="b3f1b-112">**Globala Fragmentera kartan**: hello mappning mellan horisontell partitionering nycklar och deras respektive shards inom en **Fragmentera set**.</span><span class="sxs-lookup"><span data-stu-id="b3f1b-112">**Global shard map**: hello map between sharding keys and their respective shards within a **shard set**.</span></span> <span data-ttu-id="b3f1b-113">hello globala Fragmentera kartan lagras i hello **Fragmentera kartan manager**.</span><span class="sxs-lookup"><span data-stu-id="b3f1b-113">hello global shard map is stored in hello **shard map manager**.</span></span> <span data-ttu-id="b3f1b-114">Jämför för**lokala Fragmentera kartan**.</span><span class="sxs-lookup"><span data-stu-id="b3f1b-114">Compare too**local shard map**.</span></span>

<span data-ttu-id="b3f1b-115">**Lista Fragmentera kartan**: en Fragmentera karta i vilka horisontell partitionering nycklar är mappade individuellt.</span><span class="sxs-lookup"><span data-stu-id="b3f1b-115">**List shard map**: A shard map in which sharding keys are mapped individually.</span></span> <span data-ttu-id="b3f1b-116">Jämför för**intervallet Fragmentera kartan**.</span><span class="sxs-lookup"><span data-stu-id="b3f1b-116">Compare too**Range Shard Map**.</span></span>   

<span data-ttu-id="b3f1b-117">**Lokala Fragmentera kartan**: lagras på en Fragmentera hello lokala Fragmentera mappning som innehåller mappningar för hello shardlets som finns på hello Fragmentera.</span><span class="sxs-lookup"><span data-stu-id="b3f1b-117">**Local shard map**: Stored on a shard, hello local shard map contains mappings for hello shardlets that reside on hello shard.</span></span>

<span data-ttu-id="b3f1b-118">**Flera Fragmentera frågan**: hello möjlighet tooissue en fråga mot flera delar; anger resultat returneras med hjälp av UNION ALL semantik (även kallat ”fan-out query”).</span><span class="sxs-lookup"><span data-stu-id="b3f1b-118">**Multi-shard query**: hello ability tooissue a query against multiple shards; results sets are returned using UNION ALL semantics (also known as “fan-out query”).</span></span> <span data-ttu-id="b3f1b-119">Jämför för**data beroende routning**.</span><span class="sxs-lookup"><span data-stu-id="b3f1b-119">Compare too**data dependent routing**.</span></span>

<span data-ttu-id="b3f1b-120">**Flera innehavare** och **stöd för en innehavare**: här visas en enskild klient-databas och en databas med flera innehavare:</span><span class="sxs-lookup"><span data-stu-id="b3f1b-120">**Multi-tenant** and **Single-tenant**: This shows a single-tenant database and a multi-tenant database:</span></span>

![Enstaka och flera innehavare](./media/sql-database-elastic-scale-glossary/multi-single-simple.png)

<span data-ttu-id="b3f1b-122">Här är en representation av **delat** enstaka och flera innehavare.</span><span class="sxs-lookup"><span data-stu-id="b3f1b-122">Here is a representation of **sharded** single and multi-tenant databases.</span></span> 

![Enstaka och flera innehavare](./media/sql-database-elastic-scale-glossary/shards-single-multi.png)

<span data-ttu-id="b3f1b-124">**Intervallet Fragmentera kartan**: en Fragmentera karta i vilka hello Fragmentera distributionsstrategi baseras på flera intervall med sammanhängande värden.</span><span class="sxs-lookup"><span data-stu-id="b3f1b-124">**Range shard map**: A shard map in which hello shard distribution strategy is based on multiple ranges of contiguous values.</span></span> 

<span data-ttu-id="b3f1b-125">**Refererar till tabeller**: tabeller som inte är delat men replikeras över shards.</span><span class="sxs-lookup"><span data-stu-id="b3f1b-125">**Reference tables**: Tables that are not sharded but are replicated across shards.</span></span> <span data-ttu-id="b3f1b-126">Postnummer kan till exempel lagras i en tabell.</span><span class="sxs-lookup"><span data-stu-id="b3f1b-126">For example, zip codes can be stored in a reference table.</span></span> 

<span data-ttu-id="b3f1b-127">**Fragmentera**: en Azure SQL-databas som lagrar data i ett delat datamängd.</span><span class="sxs-lookup"><span data-stu-id="b3f1b-127">**Shard**: An Azure SQL database that stores data from a sharded data set.</span></span> 

<span data-ttu-id="b3f1b-128">**Fragmentera elasticitet**: hello möjlighet tooperform båda **teckenbredden** och **lodräta skalning**.</span><span class="sxs-lookup"><span data-stu-id="b3f1b-128">**Shard elasticity**: hello ability tooperform both **horizontal scaling** and **vertical scaling**.</span></span>

<span data-ttu-id="b3f1b-129">**Delat tabeller**: tabeller som är delat, d.v.s., distribueras vars data på shards baserat på deras nyckelvärden för horisontell partitionering.</span><span class="sxs-lookup"><span data-stu-id="b3f1b-129">**Sharded tables**: Tables that are sharded, i.e., whose data is distributed across shards based on their sharding key values.</span></span> 

<span data-ttu-id="b3f1b-130">**Horisontell partitionering nyckeln**: ett kolumnvärde som avgör hur data distribueras över shards.</span><span class="sxs-lookup"><span data-stu-id="b3f1b-130">**Sharding key**: A column value that determines how data is distributed across shards.</span></span> <span data-ttu-id="b3f1b-131">hello värdetypen kan vara något av följande hello: **int**, **bigint**, **varbinary**, eller **uniqueidentifier**.</span><span class="sxs-lookup"><span data-stu-id="b3f1b-131">hello value type can be one of hello following: **int**, **bigint**, **varbinary**, or **uniqueidentifier**.</span></span> 

<span data-ttu-id="b3f1b-132">**Fragmentera set**: hello samling delar som är attributet toohello samma Fragmentera kartan i hello Fragmentera kartan manager.</span><span class="sxs-lookup"><span data-stu-id="b3f1b-132">**Shard set**: hello collection of shards that are attributed toohello same shard map in hello shard map manager.</span></span>  

<span data-ttu-id="b3f1b-133">**Shardlet**: alla hello data som är associerade med ett värde för en delning nyckel på en Fragmentera.</span><span class="sxs-lookup"><span data-stu-id="b3f1b-133">**Shardlet**: All of hello data associated with a single value of a sharding key on a shard.</span></span> <span data-ttu-id="b3f1b-134">En shardlet är hello minsta möjliga dataflyttning när omdistribuera delat tabeller.</span><span class="sxs-lookup"><span data-stu-id="b3f1b-134">A shardlet is hello smallest unit of data movement possible when redistributing sharded tables.</span></span> 

<span data-ttu-id="b3f1b-135">**Fragmentera kartan**: hello uppsättning mappningar mellan horisontell partitionering nycklar och deras respektive shards.</span><span class="sxs-lookup"><span data-stu-id="b3f1b-135">**Shard map**: hello set of mappings between sharding keys and their respective shards.</span></span>

<span data-ttu-id="b3f1b-136">**Fragmentera kartan manager**: en management-objekt och i datalagret som innehåller hello Fragmentera mappningen, Fragmentera platser och mappningar för en eller flera Fragmentera uppsättningar.</span><span class="sxs-lookup"><span data-stu-id="b3f1b-136">**Shard map manager**: A management object and data store that contains hello shard map(s), shard locations, and mappings for one or more shard sets.</span></span>

![Mappningar][2]

## <a name="verbs"></a><span data-ttu-id="b3f1b-138">Verb</span><span class="sxs-lookup"><span data-stu-id="b3f1b-138">Verbs</span></span>
<span data-ttu-id="b3f1b-139">**Teckenbredden**: hello åtgärd skala ut (eller i) en mängd shards genom att lägga till eller ta bort delar tooa Fragmentera kartan enligt nedan.</span><span class="sxs-lookup"><span data-stu-id="b3f1b-139">**Horizontal scaling**: hello act of scaling out (or in) a collection of shards by adding or removing shards tooa shard map, as shown below.</span></span>

![Vågräta och lodräta skalning][3]

<span data-ttu-id="b3f1b-141">**Sammanfoga**: hello act flyttar shardlets från två delar tooone Fragmentera och uppdaterar hello Fragmentera kartan därefter.</span><span class="sxs-lookup"><span data-stu-id="b3f1b-141">**Merge**: hello act of moving shardlets from two shards tooone shard and updating hello shard map accordingly.</span></span>

<span data-ttu-id="b3f1b-142">**Shardlet flytta**: hello act för att flytta en enda shardlet tooa olika Fragmentera.</span><span class="sxs-lookup"><span data-stu-id="b3f1b-142">**Shardlet move**: hello act of moving a single shardlet tooa different shard.</span></span> 

<span data-ttu-id="b3f1b-143">**Fragmentera**: hello act vågrätt partitionering identiskt strukturerade data över flera databaser baserat på en nyckel för horisontell partitionering.</span><span class="sxs-lookup"><span data-stu-id="b3f1b-143">**Shard**: hello act of horizontally partitioning identically structured data across multiple databases based on a sharding key.</span></span>

<span data-ttu-id="b3f1b-144">**Dela**: hello act flyttas flera shardlets från en Fragmentera tooanother (vanligtvis nya) Fragmentera.</span><span class="sxs-lookup"><span data-stu-id="b3f1b-144">**Split**: hello act of moving several shardlets from one shard tooanother (typically new) shard.</span></span> <span data-ttu-id="b3f1b-145">Hello användaren som en nyckel för horisontell partitionering som hello dela punkt.</span><span class="sxs-lookup"><span data-stu-id="b3f1b-145">A sharding key is provided by hello user as hello split point.</span></span>

<span data-ttu-id="b3f1b-146">**Lodrät skalning**: hello åtgärd skala upp (eller ned) hello prestandanivåerna för en enskild Fragmentera.</span><span class="sxs-lookup"><span data-stu-id="b3f1b-146">**Vertical Scaling**: hello act of scaling up (or down) hello performance level of an individual shard.</span></span> <span data-ttu-id="b3f1b-147">Till exempel ändrar en Fragmentera från Standard tooPremium (vilket resulterar i mer datorresurser).</span><span class="sxs-lookup"><span data-stu-id="b3f1b-147">For example, changing a shard from Standard tooPremium (which results in more computing resources).</span></span> 

[!INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Image references-->
[1]: ./media/sql-database-elastic-scale-glossary/glossary.png
[2]: ./media/sql-database-elastic-scale-glossary/mappings.png
[3]: ./media/sql-database-elastic-scale-glossary/h_versus_vert.png

