---
title: "Lägga till en Fragmentera med elastiska Databasverktyg | Microsoft Docs"
description: "Ange hur du använder Elastic Scale API: er för att lägga till nya delar en Fragmentera."
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
ms.openlocfilehash: 6a91ea2251ea3b748faba5c97765bfded9c00234
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="adding-a-shard-using-elastic-database-tools"></a><span data-ttu-id="2776d-103">Lägga till en Fragmentera med hjälp av verktyg för elastisk databas</span><span class="sxs-lookup"><span data-stu-id="2776d-103">Adding a shard using Elastic Database tools</span></span>
## <a name="to-add-a-shard-for-a-new-range-or-key"></a><span data-ttu-id="2776d-104">Att lägga till en Fragmentera för ett nytt intervall eller en nyckel</span><span class="sxs-lookup"><span data-stu-id="2776d-104">To add a shard for a new range or key</span></span>
<span data-ttu-id="2776d-105">Program behöver ofta helt enkelt lägga till nya delar för att hantera data som förväntas av nya nycklar eller nyckelintervall för en Fragmentera som redan finns.</span><span class="sxs-lookup"><span data-stu-id="2776d-105">Applications often need to simply add new shards to handle data that is expected from new keys or key ranges, for a shard map that already exists.</span></span> <span data-ttu-id="2776d-106">Till exempel ett delat program genom att klient-ID kan behöva etablera en ny Fragmentera för en ny klient eller varje månad delat data måste en ny Fragmentera etablerats före varje ny månad.</span><span class="sxs-lookup"><span data-stu-id="2776d-106">For example, an application sharded by Tenant ID may need to provision a new shard for a new tenant, or data sharded monthly may need a new shard provisioned before the start of each new month.</span></span> 

<span data-ttu-id="2776d-107">Om nya värdeintervallet nycklar inte redan är en del av en befintlig mappning är det mycket enkelt att lägga till nya Fragmentera och associera den nya nyckeln eller området till att fragmentera.</span><span class="sxs-lookup"><span data-stu-id="2776d-107">If the new range of key values is not already part of an existing mapping, it is very simple to add the new shard and associate the new key or range to that shard.</span></span> 

### <a name="example--adding-a-shard-and-its-range-to-an-existing-shard-map"></a><span data-ttu-id="2776d-108">Exempel: lägga till en Fragmentera och räckvidd i en befintlig Fragmentera karta</span><span class="sxs-lookup"><span data-stu-id="2776d-108">Example:  adding a shard and its range to an existing shard map</span></span>
<span data-ttu-id="2776d-109">Det här exemplet använder den [TryGetShard](https://msdn.microsoft.com/library/azure/dn823929.aspx) den [CreateShard](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmap.createshard.aspx), [CreateRangeMapping](https://msdn.microsoft.com/library/azure/dn807221.aspx#M:Microsoft.Azure.SqlDatabase.ElasticScale.ShardManagement.RangeShardMap`1.CreateRangeMapping\(Microsoft.Azure.SqlDatabase.ElasticScale.ShardManagement.RangeMappingCreationInfo{`0}\)) metoder, och skapar en instans av den [ShardLocation](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardlocation.shardlocation.aspx#M:Microsoft.Azure.SqlDatabase.ElasticScale.ShardManagement.ShardLocation.)klass.</span><span class="sxs-lookup"><span data-stu-id="2776d-109">This sample uses the [TryGetShard](https://msdn.microsoft.com/library/azure/dn823929.aspx) the [CreateShard](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmap.createshard.aspx), [CreateRangeMapping](https://msdn.microsoft.com/library/azure/dn807221.aspx#M:Microsoft.Azure.SqlDatabase.ElasticScale.ShardManagement.RangeShardMap`1.CreateRangeMapping\(Microsoft.Azure.SqlDatabase.ElasticScale.ShardManagement.RangeMappingCreationInfo{`0}\)) methods, and creates an instance of the [ShardLocation](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardlocation.shardlocation.aspx#M:Microsoft.Azure.SqlDatabase.ElasticScale.ShardManagement.ShardLocation.) class.</span></span> <span data-ttu-id="2776d-110">I exemplet nedan, en databas med namnet **sample_shard_2** och alla nödvändiga schemaobjekt inuti den har skapats för att rymma intervallet [300, 400).</span><span class="sxs-lookup"><span data-stu-id="2776d-110">In the sample below, a database named **sample_shard_2** and all necessary schema objects inside of it have been created to hold range [300, 400).</span></span>  

    // sm is a RangeShardMap object.
    // Add a new shard to hold the range being added. 
    Shard shard2 = null; 

    if (!sm.TryGetShard(new ShardLocation(shardServer, "sample_shard_2"),out shard2)) 
    { 
        shard2 = sm.CreateShard(new ShardLocation(shardServer, "sample_shard_2"));  
    } 

    // Create the mapping and associate it with the new shard 
    sm.CreateRangeMapping(new RangeMappingCreationInfo<long> 
                            (new Range<long>(300, 400), shard2, MappingStatus.Online)); 


<span data-ttu-id="2776d-111">Alternativt kan använda du Powershell för att skapa en ny Fragmentera kartan chef.</span><span class="sxs-lookup"><span data-stu-id="2776d-111">As an alternative, you can use Powershell to create a new Shard Map Manager.</span></span> <span data-ttu-id="2776d-112">Ett exempel är tillgänglig [här](https://gallery.technet.microsoft.com/scriptcenter/Azure-SQL-DB-Elastic-731883db).</span><span class="sxs-lookup"><span data-stu-id="2776d-112">An example is available [here](https://gallery.technet.microsoft.com/scriptcenter/Azure-SQL-DB-Elastic-731883db).</span></span>

## <a name="to-add-a-shard-for-an-empty-part-of-an-existing-range"></a><span data-ttu-id="2776d-113">Att lägga till en Fragmentera för en tom del av ett befintligt intervall</span><span class="sxs-lookup"><span data-stu-id="2776d-113">To add a shard for an empty part of an existing range</span></span>
<span data-ttu-id="2776d-114">I vissa fall kan du ett intervall redan har mappats till en Fragmentera och delvis fylls det med data, men du vill nu att dirigeras till en annan Fragmentera kommande data.</span><span class="sxs-lookup"><span data-stu-id="2776d-114">In some circumstances, you may have already mapped a range to a shard and partially filled it with data, but you now want upcoming data to be directed to a different shard.</span></span> <span data-ttu-id="2776d-115">Till exempel du Fragmentera av dagsintervall har redan den allokerade 50 dagar till en Fragmentera och dag 24 du vill att hamna i en annan Fragmentera framtida data.</span><span class="sxs-lookup"><span data-stu-id="2776d-115">For example, you shard by day range and have already allocated 50 days to a shard, but on day 24, you want future data to land in a different shard.</span></span> <span data-ttu-id="2776d-116">Den elastiska databasen [för delade sökvägssammanslagning](sql-database-elastic-scale-overview-split-and-merge.md) kan utföra denna åtgärd, men om dataflyttning inte är nödvändigt (t.ex, data för antal dagar [25, 50), d.v.s., dag 25 sammanlagt 50 uteslutande, ännu inte finns) du kan göra detta helt med hjälp av Fragmentera kartan Management-API: er direkt.</span><span class="sxs-lookup"><span data-stu-id="2776d-116">The elastic database [split-merge tool](sql-database-elastic-scale-overview-split-and-merge.md) can perform this operation, but if data movement is not necessary (for example, data for the range of days [25, 50), i.e., day 25 inclusive to 50 exclusive, does not yet exist) you can perform this entirely using the Shard Map Management APIs directly.</span></span>

### <a name="example-splitting-a-range-and-assigning-the-empty-portion-to-a-newly-added-shard"></a><span data-ttu-id="2776d-117">Exempel: dela ett intervall och tilldela en nyligen tillagda Fragmentera tom del</span><span class="sxs-lookup"><span data-stu-id="2776d-117">Example: splitting a range and assigning the empty portion to a newly-added shard</span></span>
<span data-ttu-id="2776d-118">En databas med namnet ”sample_shard_2” och alla nödvändiga schemaobjekt inuti den har skapats.</span><span class="sxs-lookup"><span data-stu-id="2776d-118">A database named “sample_shard_2” and all necessary schema objects inside of it have been created.</span></span>  

    // sm is a RangeShardMap object.
    // Add a new shard to hold the range we will move 
    Shard shard2 = null; 

    if (!sm.TryGetShard(new ShardLocation(shardServer, "sample_shard_2"),out shard2)) 
    { 

        shard2 = sm.CreateShard(new ShardLocation(shardServer, "sample_shard_2"));  
    } 

    // Split the Range holding Key 25 

    sm.SplitMapping(sm.GetMappingForKey(25), 25); 

    // Map new range holding [25-50) to different shard: 
    // first take existing mapping offline 
    sm.MarkMappingOffline(sm.GetMappingForKey(25)); 
    // now map while offline to a different shard and take online 
    RangeMappingUpdate upd = new RangeMappingUpdate(); 
    upd.Shard = shard2; 
    sm.MarkMappingOnline(sm.UpdateMapping(sm.GetMappingForKey(25), upd)); 

<span data-ttu-id="2776d-119">**Viktiga**: Använd den här tekniken bara om du är säker på att intervallet för den uppdaterade avbildningen är tom.</span><span class="sxs-lookup"><span data-stu-id="2776d-119">**Important**:  Use this technique only if you are certain that the range for the updated mapping is empty.</span></span>  <span data-ttu-id="2776d-120">Metoderna ovan Kontrollera inte data för intervallet som ska flyttas, så det är bäst att lägga till kontroller i din kod.</span><span class="sxs-lookup"><span data-stu-id="2776d-120">The methods above do not check data for the range being moved, so it is best to include checks in your code.</span></span>  <span data-ttu-id="2776d-121">Om raderna finns inom intervallet som ska flyttas, distribution för faktiska data inte att matcha uppdaterade Fragmentera kartan.</span><span class="sxs-lookup"><span data-stu-id="2776d-121">If rows exist in the range being moved, the actual data distribution will not match the updated shard map.</span></span> <span data-ttu-id="2776d-122">Använd den [för delade sökvägssammanslagning](sql-database-elastic-scale-overview-split-and-merge.md) att utföra åtgärden i stället i dessa fall.</span><span class="sxs-lookup"><span data-stu-id="2776d-122">Use the [split-merge tool](sql-database-elastic-scale-overview-split-and-merge.md) to perform the operation instead in these cases.</span></span>  

[!INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

