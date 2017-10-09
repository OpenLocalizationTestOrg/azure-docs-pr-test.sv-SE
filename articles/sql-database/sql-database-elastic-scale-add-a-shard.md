---
title: aaaAdding en Fragmentera med elastiska Databasverktyg | Microsoft Docs
description: "Hur ställer toouse Elastic Scale API: er tooadd nya shards tooa Fragmentera."
services: sql-database
documentationcenter: 
manager: jhubbard
author: ddove
editor: 
ms.assetid: 62a349db-bebe-406f-a120-2f1986f2b286
ms.service: sql-database
ms.custom: scale out apps
ms.workload: sql-database
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/24/2016
ms.author: ddove
ms.openlocfilehash: f44b59578376d1238b3012a3cb52339978079f0e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="adding-a-shard-using-elastic-database-tools"></a><span data-ttu-id="34733-103">Lägga till en Fragmentera med hjälp av verktyg för elastisk databas</span><span class="sxs-lookup"><span data-stu-id="34733-103">Adding a shard using Elastic Database tools</span></span>
## <a name="tooadd-a-shard-for-a-new-range-or-key"></a><span data-ttu-id="34733-104">tooadd en Fragmentera för ett nytt intervall eller en nyckel</span><span class="sxs-lookup"><span data-stu-id="34733-104">tooadd a shard for a new range or key</span></span>
<span data-ttu-id="34733-105">Program ofta behöver toosimply lägga till nya shards toohandle data som förväntas av nya nycklar eller viktiga områden för en Fragmentera karta som redan finns.</span><span class="sxs-lookup"><span data-stu-id="34733-105">Applications often need toosimply add new shards toohandle data that is expected from new keys or key ranges, for a shard map that already exists.</span></span> <span data-ttu-id="34733-106">Till exempel ett delat program genom att klient-ID kan behöva tooprovision nya Fragmentera för en ny klient eller varje månad delat data måste en ny Fragmentera etableras innan hello början av varje ny månad.</span><span class="sxs-lookup"><span data-stu-id="34733-106">For example, an application sharded by Tenant ID may need tooprovision a new shard for a new tenant, or data sharded monthly may need a new shard provisioned before hello start of each new month.</span></span> 

<span data-ttu-id="34733-107">Om hello nya viktiga värdeintervallet inte redan är en del av en befintlig mappning är det mycket enkelt tooadd hello nya Fragmentera och koppla hello ny nyckel eller ett område toothat Fragmentera.</span><span class="sxs-lookup"><span data-stu-id="34733-107">If hello new range of key values is not already part of an existing mapping, it is very simple tooadd hello new shard and associate hello new key or range toothat shard.</span></span> 

### <a name="example--adding-a-shard-and-its-range-tooan-existing-shard-map"></a><span data-ttu-id="34733-108">Exempel: lägga till en Fragmentera och dess intervallet tooan Fragmentera karta</span><span class="sxs-lookup"><span data-stu-id="34733-108">Example:  adding a shard and its range tooan existing shard map</span></span>
<span data-ttu-id="34733-109">Det här exemplet använder hello [TryGetShard](https://msdn.microsoft.com/library/azure/dn823929.aspx) hello [CreateShard](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmap.createshard.aspx), [CreateRangeMapping](https://msdn.microsoft.com/library/azure/dn807221.aspx#M:Microsoft.Azure.SqlDatabase.ElasticScale.ShardManagement.RangeShardMap`1.CreateRangeMapping\(Microsoft.Azure.SqlDatabase.ElasticScale.ShardManagement.RangeMappingCreationInfo{`0}\)) metoder, och skapar en instans av hello [ShardLocation](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardlocation.shardlocation.aspx#M:Microsoft.Azure.SqlDatabase.ElasticScale.ShardManagement.ShardLocation.) klass.</span><span class="sxs-lookup"><span data-stu-id="34733-109">This sample uses hello [TryGetShard](https://msdn.microsoft.com/library/azure/dn823929.aspx) hello [CreateShard](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmap.createshard.aspx), [CreateRangeMapping](https://msdn.microsoft.com/library/azure/dn807221.aspx#M:Microsoft.Azure.SqlDatabase.ElasticScale.ShardManagement.RangeShardMap`1.CreateRangeMapping\(Microsoft.Azure.SqlDatabase.ElasticScale.ShardManagement.RangeMappingCreationInfo{`0}\)) methods, and creates an instance of hello [ShardLocation](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardlocation.shardlocation.aspx#M:Microsoft.Azure.SqlDatabase.ElasticScale.ShardManagement.ShardLocation.) class.</span></span> <span data-ttu-id="34733-110">I exemplet hello nedan, en databas med namnet **sample_shard_2** och alla nödvändiga schemaobjekt inuti den har skapats toohold intervallet [300, 400).</span><span class="sxs-lookup"><span data-stu-id="34733-110">In hello sample below, a database named **sample_shard_2** and all necessary schema objects inside of it have been created toohold range [300, 400).</span></span>  

    // sm is a RangeShardMap object.
    // Add a new shard toohold hello range being added. 
    Shard shard2 = null; 

    if (!sm.TryGetShard(new ShardLocation(shardServer, "sample_shard_2"),out shard2)) 
    { 
        shard2 = sm.CreateShard(new ShardLocation(shardServer, "sample_shard_2"));  
    } 

    // Create hello mapping and associate it with hello new shard 
    sm.CreateRangeMapping(new RangeMappingCreationInfo<long> 
                            (new Range<long>(300, 400), shard2, MappingStatus.Online)); 


<span data-ttu-id="34733-111">Alternativt kan använda du Powershell toocreate en ny Fragmentera kartan chef.</span><span class="sxs-lookup"><span data-stu-id="34733-111">As an alternative, you can use Powershell toocreate a new Shard Map Manager.</span></span> <span data-ttu-id="34733-112">Ett exempel är tillgänglig [här](https://gallery.technet.microsoft.com/scriptcenter/Azure-SQL-DB-Elastic-731883db).</span><span class="sxs-lookup"><span data-stu-id="34733-112">An example is available [here](https://gallery.technet.microsoft.com/scriptcenter/Azure-SQL-DB-Elastic-731883db).</span></span>

## <a name="tooadd-a-shard-for-an-empty-part-of-an-existing-range"></a><span data-ttu-id="34733-113">tooadd en Fragmentera för en tom del av ett befintligt intervall</span><span class="sxs-lookup"><span data-stu-id="34733-113">tooadd a shard for an empty part of an existing range</span></span>
<span data-ttu-id="34733-114">I vissa fall kan du redan mappats en intervallet tooa Fragmentera och delvis fylls det med data, men nu vill kommande data toobe dirigerad tooa olika Fragmentera.</span><span class="sxs-lookup"><span data-stu-id="34733-114">In some circumstances, you may have already mapped a range tooa shard and partially filled it with data, but you now want upcoming data toobe directed tooa different shard.</span></span> <span data-ttu-id="34733-115">Till exempel du Fragmentera per dag dataintervall och redan har allokerats 50 dagar tooa Fragmentera men dag 24 tooland framtida data i en annan Fragmentera.</span><span class="sxs-lookup"><span data-stu-id="34733-115">For example, you shard by day range and have already allocated 50 days tooa shard, but on day 24, you want future data tooland in a different shard.</span></span> <span data-ttu-id="34733-116">hello elastisk databas [för delade sökvägssammanslagning](sql-database-elastic-scale-overview-split-and-merge.md) kan utföra denna åtgärd, men om dataflyttning inte är nödvändigt (t.ex, data för hello intervall [25, 50), d.v.s., dag 25 inklusiva too50 uteslutande, ännu inte finns) du kan utföra Det här helt med hello Fragmentera kartan Management API: er direkt.</span><span class="sxs-lookup"><span data-stu-id="34733-116">hello elastic database [split-merge tool](sql-database-elastic-scale-overview-split-and-merge.md) can perform this operation, but if data movement is not necessary (for example, data for hello range of days [25, 50), i.e., day 25 inclusive too50 exclusive, does not yet exist) you can perform this entirely using hello Shard Map Management APIs directly.</span></span>

### <a name="example-splitting-a-range-and-assigning-hello-empty-portion-tooa-newly-added-shard"></a><span data-ttu-id="34733-117">Exempel: dela ett intervall och tilldela hello tom del tooa nyligen tillagda Fragmentera</span><span class="sxs-lookup"><span data-stu-id="34733-117">Example: splitting a range and assigning hello empty portion tooa newly-added shard</span></span>
<span data-ttu-id="34733-118">En databas med namnet ”sample_shard_2” och alla nödvändiga schemaobjekt inuti den har skapats.</span><span class="sxs-lookup"><span data-stu-id="34733-118">A database named “sample_shard_2” and all necessary schema objects inside of it have been created.</span></span>  

    // sm is a RangeShardMap object.
    // Add a new shard toohold hello range we will move 
    Shard shard2 = null; 

    if (!sm.TryGetShard(new ShardLocation(shardServer, "sample_shard_2"),out shard2)) 
    { 

        shard2 = sm.CreateShard(new ShardLocation(shardServer, "sample_shard_2"));  
    } 

    // Split hello Range holding Key 25 

    sm.SplitMapping(sm.GetMappingForKey(25), 25); 

    // Map new range holding [25-50) toodifferent shard: 
    // first take existing mapping offline 
    sm.MarkMappingOffline(sm.GetMappingForKey(25)); 
    // now map while offline tooa different shard and take online 
    RangeMappingUpdate upd = new RangeMappingUpdate(); 
    upd.Shard = shard2; 
    sm.MarkMappingOnline(sm.UpdateMapping(sm.GetMappingForKey(25), upd)); 

<span data-ttu-id="34733-119">**Viktiga**: Använd den här tekniken bara om du är att hello intervallet för hello uppdateras mappningen är tom.</span><span class="sxs-lookup"><span data-stu-id="34733-119">**Important**:  Use this technique only if you are certain that hello range for hello updated mapping is empty.</span></span>  <span data-ttu-id="34733-120">Kontrollera hello metoderna ovan inte data för hello-intervall som ska flyttas, så det är bäst tooinclude kontrollerar i kod.</span><span class="sxs-lookup"><span data-stu-id="34733-120">hello methods above do not check data for hello range being moved, so it is best tooinclude checks in your code.</span></span>  <span data-ttu-id="34733-121">Om det finns rader i hello-intervall som ska flyttas, matchar hello faktiska Datadistribution inte hello uppdaterade Fragmentera kartan.</span><span class="sxs-lookup"><span data-stu-id="34733-121">If rows exist in hello range being moved, hello actual data distribution will not match hello updated shard map.</span></span> <span data-ttu-id="34733-122">Använd hello [för delade sökvägssammanslagning](sql-database-elastic-scale-overview-split-and-merge.md) tooperform hello åtgärden i stället i dessa fall.</span><span class="sxs-lookup"><span data-stu-id="34733-122">Use hello [split-merge tool](sql-database-elastic-scale-overview-split-and-merge.md) tooperform hello operation instead in these cases.</span></span>  

[!INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

