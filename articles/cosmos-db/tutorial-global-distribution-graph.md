---
title: "aaaAzure Cosmos DB global distributionsplatsen självstudier för Graph API | Microsoft Docs"
description: "Lär dig hur toosetup Azure Cosmos DB global distributionsplatsen med hello Graph API."
services: cosmos-db
keywords: Global distributionsplatsen, diagram, gremlin
documentationcenter: 
author: dennyglee
manager: jhubbard
editor: cgronlun
ms.assetid: 8b815047-2868-4b10-af1d-40a1af419a70
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/10/2017
ms.author: denlee
ms.openlocfilehash: 1629a31e12a18079f63e07c4909862b36b5f4c0e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toosetup-azure-cosmos-db-global-distribution-using-hello-graph-api"></a><span data-ttu-id="36eba-104">Hur toosetup Azure Cosmos DB global distributionsplatsen med hello Graph API</span><span class="sxs-lookup"><span data-stu-id="36eba-104">How toosetup Azure Cosmos DB global distribution using hello Graph API</span></span>

<span data-ttu-id="36eba-105">I den här artikeln visar vi hur toouse hello Azure portal toosetup Azure Cosmos DB global distributionsplatsen och sedan ansluta med hello Graph API (förhandsversion).</span><span class="sxs-lookup"><span data-stu-id="36eba-105">In this article, we show how toouse hello Azure portal toosetup Azure Cosmos DB global distribution and then connect using hello Graph API (preview).</span></span>

<span data-ttu-id="36eba-106">Den här artikeln beskriver hello följande uppgifter:</span><span class="sxs-lookup"><span data-stu-id="36eba-106">This article covers hello following tasks:</span></span> 

> [!div class="checklist"]
> * <span data-ttu-id="36eba-107">Konfigurera distributionslistor med hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="36eba-107">Configure global distribution using hello Azure portal</span></span>
> * <span data-ttu-id="36eba-108">Konfigurera distributionslistor med hello [Graph API: er](graph-introduction.md) (förhandsgranskning)</span><span class="sxs-lookup"><span data-stu-id="36eba-108">Configure global distribution using hello [Graph APIs](graph-introduction.md) (preview)</span></span>

[!INCLUDE [cosmos-db-tutorial-global-distribution-portal](../../includes/cosmos-db-tutorial-global-distribution-portal.md)]


## <a name="connecting-tooa-preferred-region-using-hello-graph-api-using-hello-net-sdk"></a><span data-ttu-id="36eba-109">Ansluta tooa önskad region med hello Graph API: et med hello .NET SDK</span><span class="sxs-lookup"><span data-stu-id="36eba-109">Connecting tooa preferred region using hello Graph API using hello .NET SDK</span></span>

<span data-ttu-id="36eba-110">hello Graph API exponeras som ett tillägg bibliotek ovanpå hello DocumentDB SDK.</span><span class="sxs-lookup"><span data-stu-id="36eba-110">hello Graph API is exposed as an extension library on top of hello DocumentDB SDK.</span></span>

<span data-ttu-id="36eba-111">I ordning tootake nytta av [global distributionsplatsen](distribute-data-globally.md), klientprogram kan ange hello sorterade inställningar lista över regioner toobe används tooperform dokumentet åtgärder.</span><span class="sxs-lookup"><span data-stu-id="36eba-111">In order tootake advantage of [global distribution](distribute-data-globally.md), client applications can specify hello ordered preference list of regions toobe used tooperform document operations.</span></span> <span data-ttu-id="36eba-112">Detta kan göras genom att ange hello anslutningsprincip.</span><span class="sxs-lookup"><span data-stu-id="36eba-112">This can be done by setting hello connection policy.</span></span> <span data-ttu-id="36eba-113">Baserat på hello Azure Cosmos DB kontokonfigurationen anges regional tillgänglighet och hello inställningar lista hello de flesta optimala slutpunkten ska väljs av hello SDK tooperform skrivning och läsåtgärder.</span><span class="sxs-lookup"><span data-stu-id="36eba-113">Based on hello Azure Cosmos DB account configuration, current regional availability and hello preference list specified, hello most optimal endpoint will be chosen by hello SDK tooperform write and read operations.</span></span>

<span data-ttu-id="36eba-114">Den här inställningen listan anges när initierar en anslutning med hello SDK: er.</span><span class="sxs-lookup"><span data-stu-id="36eba-114">This preference list is specified when initializing a connection using hello SDKs.</span></span> <span data-ttu-id="36eba-115">hello SDK acceptera en valfri parameter ”PreferredLocations” som en sorterad lista över Azure-regioner.</span><span class="sxs-lookup"><span data-stu-id="36eba-115">hello SDKs accept an optional parameter "PreferredLocations" that is an ordered list of Azure regions.</span></span>

* <span data-ttu-id="36eba-116">**Skriver**: hello SDK skickar automatiskt alla skriver toohello aktuella skrivåtgärder region.</span><span class="sxs-lookup"><span data-stu-id="36eba-116">**Writes**: hello SDK will automatically send all writes toohello current write region.</span></span>
* <span data-ttu-id="36eba-117">**Läser**: alla Läs skickas toohello första tillgängliga region i hello PreferredLocations lista.</span><span class="sxs-lookup"><span data-stu-id="36eba-117">**Reads**: All reads will be sent toohello first available region in hello PreferredLocations list.</span></span> <span data-ttu-id="36eba-118">Om hello begäran misslyckas hello klienten misslyckas ned hello listan toohello nästa område, och så vidare.</span><span class="sxs-lookup"><span data-stu-id="36eba-118">If hello request fails, hello client will fail down hello list toohello next region, and so on.</span></span> <span data-ttu-id="36eba-119">hello SDK försöker tooread från hello regioner som anges i PreferredLocations.</span><span class="sxs-lookup"><span data-stu-id="36eba-119">hello SDKs will only attempt tooread from hello regions specified in PreferredLocations.</span></span> <span data-ttu-id="36eba-120">Så, till exempel om hello Cosmos-DB-konto finns i tre regioner, men hello klienten endast anger två hello-write regioner för PreferredLocations, sedan utan läsning hanteras utanför hello skrivåtgärder region, även i hello fall av redundans.</span><span class="sxs-lookup"><span data-stu-id="36eba-120">So, for example, if hello Cosmos DB account is available in three regions, but hello client only specifies two of hello non-write regions for PreferredLocations, then no reads will be served out of hello write region, even in hello case of failover.</span></span>

<span data-ttu-id="36eba-121">hello-programmet kan verifiera hello aktuella skrivåtgärder slutpunkt och läsa slutpunkt som valts av hello SDK av kontrollerar två egenskaper, WriteEndpoint och ReadEndpoint, finns i SDK-version 1.8 och senare.</span><span class="sxs-lookup"><span data-stu-id="36eba-121">hello application can verify hello current write endpoint and read endpoint chosen by hello SDK by checking two properties, WriteEndpoint and ReadEndpoint, available in SDK version 1.8 and above.</span></span> <span data-ttu-id="36eba-122">Om hello PreferredLocations egenskapen inte anges kommer alla begäranden hanteras från hello aktuella skrivåtgärder region.</span><span class="sxs-lookup"><span data-stu-id="36eba-122">If hello PreferredLocations property is not set, all requests will be served from hello current write region.</span></span>

### <a name="using-hello-sdk"></a><span data-ttu-id="36eba-123">Med hjälp av hello SDK</span><span class="sxs-lookup"><span data-stu-id="36eba-123">Using hello SDK</span></span>

<span data-ttu-id="36eba-124">Till exempel i hello .NET SDK, hello `ConnectionPolicy` parameter för hello `DocumentClient` konstruktorn har en egenskap som kallas `PreferredLocations`.</span><span class="sxs-lookup"><span data-stu-id="36eba-124">For example, in hello .NET SDK, hello `ConnectionPolicy` parameter for hello `DocumentClient` constructor has a property called `PreferredLocations`.</span></span> <span data-ttu-id="36eba-125">Den här egenskapen kan anges tooa region namnlista.</span><span class="sxs-lookup"><span data-stu-id="36eba-125">This property can be set tooa list of region names.</span></span> <span data-ttu-id="36eba-126">hello visningsnamnen för [Azure-regioner] [ regions] kan anges som en del av `PreferredLocations`.</span><span class="sxs-lookup"><span data-stu-id="36eba-126">hello display names for [Azure Regions][regions] can be specified as part of `PreferredLocations`.</span></span>

> [!NOTE]
> <span data-ttu-id="36eba-127">hello URL: er för hello slutpunkter ska inte betraktas som långlivade konstanter.</span><span class="sxs-lookup"><span data-stu-id="36eba-127">hello URLs for hello endpoints should not be considered as long-lived constants.</span></span> <span data-ttu-id="36eba-128">hello-tjänsten kan uppdatera dem när som helst.</span><span class="sxs-lookup"><span data-stu-id="36eba-128">hello service may update these at any point.</span></span> <span data-ttu-id="36eba-129">hello SDK hanterar automatiskt den här ändringen.</span><span class="sxs-lookup"><span data-stu-id="36eba-129">hello SDK handles this change automatically.</span></span>
>
>

```cs
// Getting endpoints from application settings or other configuration location
Uri accountEndPoint = new Uri(Properties.Settings.Default.GlobalDatabaseUri);
string accountKey = Properties.Settings.Default.GlobalDatabaseKey;

ConnectionPolicy connectionPolicy = new ConnectionPolicy();

//Setting read region selection preference
connectionPolicy.PreferredLocations.Add(LocationNames.WestUS); // first preference
connectionPolicy.PreferredLocations.Add(LocationNames.EastUS); // second preference
connectionPolicy.PreferredLocations.Add(LocationNames.NorthEurope); // third preference

// initialize connection
DocumentClient docClient = new DocumentClient(
    accountEndPoint,
    accountKey,
    connectionPolicy);

// connect tooAzure Cosmos DB
await docClient.OpenAsync().ConfigureAwait(false);
```

<span data-ttu-id="36eba-130">Det är den som Slutför den här kursen.</span><span class="sxs-lookup"><span data-stu-id="36eba-130">That's it, that completes this tutorial.</span></span> <span data-ttu-id="36eba-131">Du kan lära dig hur toomanage hello konsekvenskontroll av globalt replikerade kontot genom att läsa [konsekvensnivåer i Azure Cosmos DB](consistency-levels.md).</span><span class="sxs-lookup"><span data-stu-id="36eba-131">You can learn how toomanage hello consistency of your globally replicated account by reading [Consistency levels in Azure Cosmos DB](consistency-levels.md).</span></span> <span data-ttu-id="36eba-132">Och för mer information om hur globala databasreplikering fungerar i Azure Cosmos DB, se [distribuera data globalt med Azure Cosmos DB](distribute-data-globally.md).</span><span class="sxs-lookup"><span data-stu-id="36eba-132">And for more information about how global database replication works in Azure Cosmos DB, see [Distribute data globally with Azure Cosmos DB](distribute-data-globally.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="36eba-133">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="36eba-133">Next steps</span></span>

<span data-ttu-id="36eba-134">I den här kursen har du gjort hello följande:</span><span class="sxs-lookup"><span data-stu-id="36eba-134">In this tutorial, you've done hello following:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="36eba-135">Konfigurera distributionslistor med hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="36eba-135">Configure global distribution using hello Azure portal</span></span>
> * <span data-ttu-id="36eba-136">Konfigurera distributionslistor med hello DocumentDB APIs</span><span class="sxs-lookup"><span data-stu-id="36eba-136">Configure global distribution using hello DocumentDB APIs</span></span>

<span data-ttu-id="36eba-137">Du kan nu fortsätta toohello nästa självstudiekurs toolearn hur toodevelop lokalt med hjälp av hello Azure Cosmos DB lokala emulatorn.</span><span class="sxs-lookup"><span data-stu-id="36eba-137">You can now proceed toohello next tutorial toolearn how toodevelop locally using hello Azure Cosmos DB local emulator.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="36eba-138">Utveckla lokalt med hello-emulatorn</span><span class="sxs-lookup"><span data-stu-id="36eba-138">Develop locally with hello emulator</span></span>](local-emulator.md)

[regions]: https://azure.microsoft.com/regions/

