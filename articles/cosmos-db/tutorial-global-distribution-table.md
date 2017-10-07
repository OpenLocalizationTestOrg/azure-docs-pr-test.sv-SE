---
title: "aaaAzure Cosmos DB global distributionsplatsen självstudier för tabellen API | Microsoft Docs"
description: "Lär dig hur toosetup Azure Cosmos DB global distributionsplatsen med hello tabell API."
services: cosmos-db
keywords: Global distributionsplatsen, tabell
documentationcenter: 
author: mimig1
manager: jhubbard
editor: cgronlun
ms.assetid: 8b815047-2868-4b10-af1d-40a1af419a70
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/10/2017
ms.author: mimig
ms.openlocfilehash: d2a995e09c37f9449856aef2ab707e95eb8a540c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toosetup-azure-cosmos-db-global-distribution-using-hello-table-api"></a><span data-ttu-id="14010-104">Hur toosetup Azure Cosmos DB global distributionsplatsen med hello tabell-API</span><span class="sxs-lookup"><span data-stu-id="14010-104">How toosetup Azure Cosmos DB global distribution using hello Table API</span></span>

<span data-ttu-id="14010-105">I den här artikeln visar vi hur toouse hello Azure portal toosetup Azure Cosmos DB global distributionsplatsen och sedan ansluta med hello tabell-API (förhandsversion).</span><span class="sxs-lookup"><span data-stu-id="14010-105">In this article, we show how toouse hello Azure portal toosetup Azure Cosmos DB global distribution and then connect using hello Table API (preview).</span></span>

<span data-ttu-id="14010-106">Den här artikeln beskriver hello följande uppgifter:</span><span class="sxs-lookup"><span data-stu-id="14010-106">This article covers hello following tasks:</span></span> 

> [!div class="checklist"]
> * <span data-ttu-id="14010-107">Konfigurera distributionslistor med hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="14010-107">Configure global distribution using hello Azure portal</span></span>
> * <span data-ttu-id="14010-108">Konfigurera distributionslistor med hello [tabell-API](table-introduction.md)</span><span class="sxs-lookup"><span data-stu-id="14010-108">Configure global distribution using hello [Table API](table-introduction.md)</span></span>

[!INCLUDE [cosmos-db-tutorial-global-distribution-portal](../../includes/cosmos-db-tutorial-global-distribution-portal.md)]


## <a name="connecting-tooa-preferred-region-using-hello-table-api"></a><span data-ttu-id="14010-109">Ansluta tooa önskad region med hello tabell-API</span><span class="sxs-lookup"><span data-stu-id="14010-109">Connecting tooa preferred region using hello Table API</span></span>

<span data-ttu-id="14010-110">I ordning tootake nytta av [global distributionsplatsen](distribute-data-globally.md), klientprogram kan ange hello sorterade inställningar lista över regioner toobe används tooperform dokumentet åtgärder.</span><span class="sxs-lookup"><span data-stu-id="14010-110">In order tootake advantage of [global distribution](distribute-data-globally.md), client applications can specify hello ordered preference list of regions toobe used tooperform document operations.</span></span> <span data-ttu-id="14010-111">Detta kan göras genom att ange hello `TablePreferredLocations` Konfigurationsvärdet i hello programkonfiguration för hello preview Azure Storage SDK: N.</span><span class="sxs-lookup"><span data-stu-id="14010-111">This can be done by setting hello `TablePreferredLocations` configuration value in hello app config for hello preview Azure Storage SDK.</span></span> <span data-ttu-id="14010-112">Baserat på hello Azure Cosmos DB kontokonfigurationen regional tillgänglighet och hello inställningar listan anges de flesta optimala endpoint väljas av hello Azure Storage SDK: N tooperform hello skriva och läsåtgärder.</span><span class="sxs-lookup"><span data-stu-id="14010-112">Based on hello Azure Cosmos DB account configuration, current regional availability and hello preference list specified, hello most optimal endpoint will be chosen by hello Azure Storage SDK tooperform write and read operations.</span></span>

<span data-ttu-id="14010-113">Hej `TablePreferredLocations` ska innehålla en kommaavgränsad lista över önskade (flera homing) platser för läsning.</span><span class="sxs-lookup"><span data-stu-id="14010-113">hello `TablePreferredLocations` should contain a comma-separated list of preferred (multi-homing) locations for reads.</span></span> <span data-ttu-id="14010-114">Varje klientinstans kan ange en delmängd av dessa regioner hello rekommenderas för låg latens läsningar.</span><span class="sxs-lookup"><span data-stu-id="14010-114">Each client instance can specify a subset of these regions in hello preferred order for low latency reads.</span></span> <span data-ttu-id="14010-115">hello regioner måste namnges med deras [visningsnamn](https://msdn.microsoft.com/library/azure/gg441293.aspx), till exempel `West US`.</span><span class="sxs-lookup"><span data-stu-id="14010-115">hello regions must be named using their [display names](https://msdn.microsoft.com/library/azure/gg441293.aspx), for example, `West US`.</span></span>

<span data-ttu-id="14010-116">Alla Läs skickas toohello första tillgängliga region i hello `TablePreferredLocations` lista.</span><span class="sxs-lookup"><span data-stu-id="14010-116">All reads will be sent toohello first available region in hello `TablePreferredLocations` list.</span></span> <span data-ttu-id="14010-117">Om hello begäran misslyckas hello klienten misslyckas ned hello listan toohello nästa område, och så vidare.</span><span class="sxs-lookup"><span data-stu-id="14010-117">If hello request fails, hello client will fail down hello list toohello next region, and so on.</span></span>

<span data-ttu-id="14010-118">hello SDK försöker tooread från hello regioner som anges i `TablePreferredLocations`.</span><span class="sxs-lookup"><span data-stu-id="14010-118">hello SDK will only attempt tooread from hello regions specified in `TablePreferredLocations`.</span></span> <span data-ttu-id="14010-119">Så, till exempel om hello databaskonto finns tillgänglig i tre regioner, men hello klienten anger endast två av hello-write regioner för `TablePreferredLocations`, och sedan utan läsning hanteras utanför hello skrivåtgärder region, även i hello fall av redundans.</span><span class="sxs-lookup"><span data-stu-id="14010-119">So, for example, if hello Database Account is available in three regions, but hello client only specifies two of hello non-write regions for `TablePreferredLocations`, then no reads will be served out of hello write region, even in hello case of failover.</span></span>

<span data-ttu-id="14010-120">hello SDK skickar automatiskt alla skrivningar toohello aktuella skrivåtgärder region.</span><span class="sxs-lookup"><span data-stu-id="14010-120">hello SDK will automatically send all writes toohello current write region.</span></span>

<span data-ttu-id="14010-121">Om hello `TablePreferredLocations` egenskapen har inte angetts, alla begäranden hanteras från hello aktuella skrivåtgärder region.</span><span class="sxs-lookup"><span data-stu-id="14010-121">If hello `TablePreferredLocations` property is not set, all requests will be served from hello current write region.</span></span>

```xml
    <appSettings>
      <add key="TablePreferredLocations" value="East US, West US, North Europe"/>           
    </appSettings>
```

<span data-ttu-id="14010-122">Det är den som Slutför den här kursen.</span><span class="sxs-lookup"><span data-stu-id="14010-122">That's it, that completes this tutorial.</span></span> <span data-ttu-id="14010-123">Du kan lära dig hur toomanage hello konsekvenskontroll av globalt replikerade kontot genom att läsa [konsekvensnivåer i Azure Cosmos DB](consistency-levels.md).</span><span class="sxs-lookup"><span data-stu-id="14010-123">You can learn how toomanage hello consistency of your globally replicated account by reading [Consistency levels in Azure Cosmos DB](consistency-levels.md).</span></span> <span data-ttu-id="14010-124">Och för mer information om hur globala databasreplikering fungerar i Azure Cosmos DB, se [distribuera data globalt med Azure Cosmos DB](distribute-data-globally.md).</span><span class="sxs-lookup"><span data-stu-id="14010-124">And for more information about how global database replication works in Azure Cosmos DB, see [Distribute data globally with Azure Cosmos DB](distribute-data-globally.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="14010-125">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="14010-125">Next steps</span></span>

<span data-ttu-id="14010-126">I den här kursen har du gjort hello följande:</span><span class="sxs-lookup"><span data-stu-id="14010-126">In this tutorial, you've done hello following:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="14010-127">Konfigurera distributionslistor med hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="14010-127">Configure global distribution using hello Azure portal</span></span>
> * <span data-ttu-id="14010-128">Konfigurera distributionslistor med hello DocumentDB APIs</span><span class="sxs-lookup"><span data-stu-id="14010-128">Configure global distribution using hello DocumentDB APIs</span></span>

<span data-ttu-id="14010-129">Du kan nu fortsätta toohello nästa självstudiekurs toolearn hur toodevelop lokalt med hjälp av hello Azure Cosmos DB lokala emulatorn.</span><span class="sxs-lookup"><span data-stu-id="14010-129">You can now proceed toohello next tutorial toolearn how toodevelop locally using hello Azure Cosmos DB local emulator.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="14010-130">Utveckla lokalt med hello-emulatorn</span><span class="sxs-lookup"><span data-stu-id="14010-130">Develop locally with hello emulator</span></span>](local-emulator.md)
