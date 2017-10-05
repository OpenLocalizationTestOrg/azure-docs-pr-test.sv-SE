---
title: "Azure DB Cosmos global distributionsplatsen självstudier för tabellen API | Microsoft Docs"
description: "Lär dig hur du ställer in Azure Cosmos DB global distributionsplatsen med tabell-API."
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
ms.openlocfilehash: 63c9e530a4982e2e6e478fea56e015fc77851e1d
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-setup-azure-cosmos-db-global-distribution-using-the-table-api"></a><span data-ttu-id="2b29a-104">Hur du konfigurerar Azure Cosmos DB global distributionsplatsen med tabell-API</span><span class="sxs-lookup"><span data-stu-id="2b29a-104">How to setup Azure Cosmos DB global distribution using the Table API</span></span>

<span data-ttu-id="2b29a-105">I den här artikeln visar vi hur du använder Azure portal för att installationen Azure Cosmos DB global distributionsplatsen och ansluter sedan med tabell-API (förhandsversion).</span><span class="sxs-lookup"><span data-stu-id="2b29a-105">In this article, we show how to use the Azure portal to setup Azure Cosmos DB global distribution and then connect using the Table API (preview).</span></span>

<span data-ttu-id="2b29a-106">Den här artikeln omfattar följande aktiviteter:</span><span class="sxs-lookup"><span data-stu-id="2b29a-106">This article covers the following tasks:</span></span> 

> [!div class="checklist"]
> * <span data-ttu-id="2b29a-107">Konfigurera distributionslistor med Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="2b29a-107">Configure global distribution using the Azure portal</span></span>
> * <span data-ttu-id="2b29a-108">Konfigurera distributionslistor med hjälp av den [tabellen API](table-introduction.md)</span><span class="sxs-lookup"><span data-stu-id="2b29a-108">Configure global distribution using the [Table API](table-introduction.md)</span></span>

[!INCLUDE [cosmos-db-tutorial-global-distribution-portal](../../includes/cosmos-db-tutorial-global-distribution-portal.md)]


## <a name="connecting-to-a-preferred-region-using-the-table-api"></a><span data-ttu-id="2b29a-109">Ansluter till en önskad region med tabell-API</span><span class="sxs-lookup"><span data-stu-id="2b29a-109">Connecting to a preferred region using the Table API</span></span>

<span data-ttu-id="2b29a-110">För att kunna dra nytta av [global distributionsplatsen](distribute-data-globally.md), klientprogram kan ange beställda inställningar listan över regioner som används för att utföra åtgärder för dokumentet.</span><span class="sxs-lookup"><span data-stu-id="2b29a-110">In order to take advantage of [global distribution](distribute-data-globally.md), client applications can specify the ordered preference list of regions to be used to perform document operations.</span></span> <span data-ttu-id="2b29a-111">Detta kan göras genom att ange den `TablePreferredLocations` Konfigurationsvärdet i programmets konfiguration för förhandsversionen av Azure Storage SDK: N.</span><span class="sxs-lookup"><span data-stu-id="2b29a-111">This can be done by setting the `TablePreferredLocations` configuration value in the app config for the preview Azure Storage SDK.</span></span> <span data-ttu-id="2b29a-112">Baserat på konfigurationen av Azure DB som Cosmos-kontot, aktuella regional tillgänglighet och inställningar listan som anges, kommer den mest optimala slutpunkten väljas av Azure Storage SDK: N att utföra skrivåtgärder och läsåtgärder.</span><span class="sxs-lookup"><span data-stu-id="2b29a-112">Based on the Azure Cosmos DB account configuration, current regional availability and the preference list specified, the most optimal endpoint will be chosen by the Azure Storage SDK to perform write and read operations.</span></span>

<span data-ttu-id="2b29a-113">Den `TablePreferredLocations` ska innehålla en kommaavgränsad lista över önskade (flera homing) platser för läsning.</span><span class="sxs-lookup"><span data-stu-id="2b29a-113">The `TablePreferredLocations` should contain a comma-separated list of preferred (multi-homing) locations for reads.</span></span> <span data-ttu-id="2b29a-114">Varje klientinstans kan ange en delmängd av dessa regioner i önskad ordning för låg latens läsningar.</span><span class="sxs-lookup"><span data-stu-id="2b29a-114">Each client instance can specify a subset of these regions in the preferred order for low latency reads.</span></span> <span data-ttu-id="2b29a-115">Regionerna måste namnges med deras [visningsnamn](https://msdn.microsoft.com/library/azure/gg441293.aspx), till exempel `West US`.</span><span class="sxs-lookup"><span data-stu-id="2b29a-115">The regions must be named using their [display names](https://msdn.microsoft.com/library/azure/gg441293.aspx), for example, `West US`.</span></span>

<span data-ttu-id="2b29a-116">Alla Läs kommer att skickas till den första tillgängliga regionen i den `TablePreferredLocations` lista.</span><span class="sxs-lookup"><span data-stu-id="2b29a-116">All reads will be sent to the first available region in the `TablePreferredLocations` list.</span></span> <span data-ttu-id="2b29a-117">Om begäran inte klienten misslyckas nedåt i listan till nästa region, och så vidare.</span><span class="sxs-lookup"><span data-stu-id="2b29a-117">If the request fails, the client will fail down the list to the next region, and so on.</span></span>

<span data-ttu-id="2b29a-118">SDK görs endast försök att läsa från de regioner som anges i `TablePreferredLocations`.</span><span class="sxs-lookup"><span data-stu-id="2b29a-118">The SDK will only attempt to read from the regions specified in `TablePreferredLocations`.</span></span> <span data-ttu-id="2b29a-119">I så fall till exempel om det konto som är tillgänglig i tre regioner, men klienten anger endast två av regionerna som icke-och skrivbehörighet för `TablePreferredLocations`, och sedan utan läsning som inte hanteras out-of-skrivåtgärder region, även i händelse av växling vid fel.</span><span class="sxs-lookup"><span data-stu-id="2b29a-119">So, for example, if the Database Account is available in three regions, but the client only specifies two of the non-write regions for `TablePreferredLocations`, then no reads will be served out of the write region, even in the case of failover.</span></span>

<span data-ttu-id="2b29a-120">SDK skickar automatiskt alla skrivningar till aktuellt skriva region.</span><span class="sxs-lookup"><span data-stu-id="2b29a-120">The SDK will automatically send all writes to the current write region.</span></span>

<span data-ttu-id="2b29a-121">Om den `TablePreferredLocations` egenskapen har inte angetts, alla begäranden hanteras från det aktuella området för skrivning.</span><span class="sxs-lookup"><span data-stu-id="2b29a-121">If the `TablePreferredLocations` property is not set, all requests will be served from the current write region.</span></span>

```xml
    <appSettings>
      <add key="TablePreferredLocations" value="East US, West US, North Europe"/>           
    </appSettings>
```

<span data-ttu-id="2b29a-122">Det är den som Slutför den här kursen.</span><span class="sxs-lookup"><span data-stu-id="2b29a-122">That's it, that completes this tutorial.</span></span> <span data-ttu-id="2b29a-123">Du kan lära dig hur du hanterar konsekvensen för globalt replikerade kontot genom att läsa [konsekvensnivåer i Azure Cosmos DB](consistency-levels.md).</span><span class="sxs-lookup"><span data-stu-id="2b29a-123">You can learn how to manage the consistency of your globally replicated account by reading [Consistency levels in Azure Cosmos DB](consistency-levels.md).</span></span> <span data-ttu-id="2b29a-124">Och för mer information om hur globala databasreplikering fungerar i Azure Cosmos DB, se [distribuera data globalt med Azure Cosmos DB](distribute-data-globally.md).</span><span class="sxs-lookup"><span data-stu-id="2b29a-124">And for more information about how global database replication works in Azure Cosmos DB, see [Distribute data globally with Azure Cosmos DB](distribute-data-globally.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="2b29a-125">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="2b29a-125">Next steps</span></span>

<span data-ttu-id="2b29a-126">I den här självstudiekursen kommer du har gjort följande:</span><span class="sxs-lookup"><span data-stu-id="2b29a-126">In this tutorial, you've done the following:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="2b29a-127">Konfigurera distributionslistor med Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="2b29a-127">Configure global distribution using the Azure portal</span></span>
> * <span data-ttu-id="2b29a-128">Konfigurera globala distribution via DocumentDB APIs</span><span class="sxs-lookup"><span data-stu-id="2b29a-128">Configure global distribution using the DocumentDB APIs</span></span>

<span data-ttu-id="2b29a-129">Du kan nu fortsätta till nästa kurs att lära dig hur du utvecklar lokalt med hjälp av lokala Azure DB som Cosmos-emulatorn.</span><span class="sxs-lookup"><span data-stu-id="2b29a-129">You can now proceed to the next tutorial to learn how to develop locally using the Azure Cosmos DB local emulator.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="2b29a-130">Utveckla lokalt med emulatorn</span><span class="sxs-lookup"><span data-stu-id="2b29a-130">Develop locally with the emulator</span></span>](local-emulator.md)