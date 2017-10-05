---
title: "Azure DB Cosmos global distributionsplatsen självstudier för MongoDB API | Microsoft Docs"
description: "Lär dig hur du ställer in Azure Cosmos DB global distributionsplatsen med hjälp av MongoDB-API."
services: cosmos-db
keywords: Global distributionsplatsen, MongoDB
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
ms.openlocfilehash: a2747102f4d8cac412b67abc3fd07cfa3661bcee
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="how-to-setup-azure-cosmos-db-global-distribution-using-the-mongodb-api"></a><span data-ttu-id="8ce6d-104">Hur du konfigurerar Azure Cosmos DB global distributionsplatsen med hjälp av MongoDB-API</span><span class="sxs-lookup"><span data-stu-id="8ce6d-104">How to setup Azure Cosmos DB global distribution using the MongoDB API</span></span>

<span data-ttu-id="8ce6d-105">I den här artikeln visar vi hur du använder Azure-portalen för att konfigurera Azure Cosmos DB global distributionsplatsen och ansluter sedan med hjälp av MongoDB-API.</span><span class="sxs-lookup"><span data-stu-id="8ce6d-105">In this article, we show how to use the Azure portal to setup Azure Cosmos DB global distribution and then connect using the MongoDB API.</span></span>

<span data-ttu-id="8ce6d-106">Den här artikeln omfattar följande aktiviteter:</span><span class="sxs-lookup"><span data-stu-id="8ce6d-106">This article covers the following tasks:</span></span> 

> [!div class="checklist"]
> * <span data-ttu-id="8ce6d-107">Konfigurera distributionslistor med Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="8ce6d-107">Configure global distribution using the Azure portal</span></span>
> * <span data-ttu-id="8ce6d-108">Konfigurera distributionslistor med hjälp av den [MongoDB-API](mongodb-introduction.md)</span><span class="sxs-lookup"><span data-stu-id="8ce6d-108">Configure global distribution using the [MongoDB API](mongodb-introduction.md)</span></span>

[!INCLUDE [cosmos-db-tutorial-global-distribution-portal](../../includes/cosmos-db-tutorial-global-distribution-portal.md)]

## <a name="verifying-your-regional-setup-using-the-mongodb-api"></a><span data-ttu-id="8ce6d-109">Verifiera dina nationella inställningar med hjälp av MongoDB-API</span><span class="sxs-lookup"><span data-stu-id="8ce6d-109">Verifying your regional setup using the MongoDB API</span></span>
<span data-ttu-id="8ce6d-110">Det enklaste sättet för double globala konfigurationen inom API kontrolleras för MongoDB är att köra den *isMaster()* från Mongo-gränssnittet.</span><span class="sxs-lookup"><span data-stu-id="8ce6d-110">The simplest way of double checking your global configuration within API for MongoDB is to run the *isMaster()* command from the Mongo Shell.</span></span>

<span data-ttu-id="8ce6d-111">Från din Mongo Shell:</span><span class="sxs-lookup"><span data-stu-id="8ce6d-111">From your Mongo Shell:</span></span>

   ```
      db.isMaster()
   ```
   
<span data-ttu-id="8ce6d-112">Exempel resultat:</span><span class="sxs-lookup"><span data-stu-id="8ce6d-112">Example results:</span></span>

   ```JSON
      {
         "_t": "IsMasterResponse",
         "ok": 1,
         "ismaster": true,
         "maxMessageSizeBytes": 4194304,
         "maxWriteBatchSize": 1000,
         "minWireVersion": 0,
         "maxWireVersion": 2,
         "tags": {
            "region": "South India"
         },
         "hosts": [
            "vishi-api-for-mongodb-southcentralus.documents.azure.com:10255",
            "vishi-api-for-mongodb-westeurope.documents.azure.com:10255",
            "vishi-api-for-mongodb-southindia.documents.azure.com:10255"
         ],
         "setName": "globaldb",
         "setVersion": 1,
         "primary": "vishi-api-for-mongodb-southindia.documents.azure.com:10255",
         "me": "vishi-api-for-mongodb-southindia.documents.azure.com:10255"
      }
   ```

## <a name="connecting-to-a-preferred-region-using-the-mongodb-api"></a><span data-ttu-id="8ce6d-113">Ansluter till en önskad region med MongoDB-API</span><span class="sxs-lookup"><span data-stu-id="8ce6d-113">Connecting to a preferred region using the MongoDB API</span></span>

<span data-ttu-id="8ce6d-114">MongoDB-API kan du ange din samling Läs inställningar för ett globalt distribuerad databas.</span><span class="sxs-lookup"><span data-stu-id="8ce6d-114">The MongoDB API enables you to specify your collection's read preference for a globally distributed database.</span></span> <span data-ttu-id="8ce6d-115">För både låg latens läsningar och globala hög tillgänglighet rekommenderar vi att ställa in din samling skrivskyddade inställningar till *närmaste*.</span><span class="sxs-lookup"><span data-stu-id="8ce6d-115">For both low latency reads and global high availability, we recommend setting your collection's read preference to *nearest*.</span></span> <span data-ttu-id="8ce6d-116">Läs inställningen för *närmaste* har konfigurerats för att läsa från den närmaste regionen.</span><span class="sxs-lookup"><span data-stu-id="8ce6d-116">A read preference of *nearest* is configured to read from the closest region.</span></span>

```csharp
var collection = database.GetCollection<BsonDocument>(collectionName);
collection = collection.WithReadPreference(new ReadPreference(ReadPreferenceMode.Nearest));
```

<span data-ttu-id="8ce6d-117">För program med en primär läsning/skrivning och en sekundär region för katastrofåterställning (DR) scenarier, rekommenderar vi att ställa in din samling skrivskyddade inställningar till *sekundära önskad*.</span><span class="sxs-lookup"><span data-stu-id="8ce6d-117">For applications with a primary read/write region and a secondary region for disaster recovery (DR) scenarios, we recommend setting your collection's read preference to *secondary preferred*.</span></span> <span data-ttu-id="8ce6d-118">Läs inställningen för *sekundära önskade* har konfigurerats för att läsa från den sekundära regionen när den primära regionen är inte tillgänglig.</span><span class="sxs-lookup"><span data-stu-id="8ce6d-118">A read preference of *secondary preferred* is configured to read from the secondary region when the primary region is unavailable.</span></span>

```csharp
var collection = database.GetCollection<BsonDocument>(collectionName);
collection = collection.WithReadPreference(new ReadPreference(ReadPreferenceMode.SecondaryPreferred));
```

<span data-ttu-id="8ce6d-119">Slutligen, om du vill att manuellt ange din skrivskyddade regioner.</span><span class="sxs-lookup"><span data-stu-id="8ce6d-119">Lastly, if you would like to manually specify your read regions.</span></span> <span data-ttu-id="8ce6d-120">Du kan ange region taggen inom önskemål skrivskyddade.</span><span class="sxs-lookup"><span data-stu-id="8ce6d-120">You can set the region Tag within your read preference.</span></span>

```csharp
var collection = database.GetCollection<BsonDocument>(collectionName);
var tag = new Tag("region", "Southeast Asia");
collection = collection.WithReadPreference(new ReadPreference(ReadPreferenceMode.Secondary, new[] { new TagSet(new[] { tag }) }));
```

<span data-ttu-id="8ce6d-121">Det är den som Slutför den här kursen.</span><span class="sxs-lookup"><span data-stu-id="8ce6d-121">That's it, that completes this tutorial.</span></span> <span data-ttu-id="8ce6d-122">Du kan lära dig hur du hanterar konsekvensen för globalt replikerade kontot genom att läsa [konsekvensnivåer i Azure Cosmos DB](consistency-levels.md).</span><span class="sxs-lookup"><span data-stu-id="8ce6d-122">You can learn how to manage the consistency of your globally replicated account by reading [Consistency levels in Azure Cosmos DB](consistency-levels.md).</span></span> <span data-ttu-id="8ce6d-123">Och för mer information om hur globala databasreplikering fungerar i Azure Cosmos DB, se [distribuera data globalt med Azure Cosmos DB](distribute-data-globally.md).</span><span class="sxs-lookup"><span data-stu-id="8ce6d-123">And for more information about how global database replication works in Azure Cosmos DB, see [Distribute data globally with Azure Cosmos DB](distribute-data-globally.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="8ce6d-124">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="8ce6d-124">Next steps</span></span>

<span data-ttu-id="8ce6d-125">I den här självstudiekursen kommer du har gjort följande:</span><span class="sxs-lookup"><span data-stu-id="8ce6d-125">In this tutorial, you've done the following:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="8ce6d-126">Konfigurera distributionslistor med Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="8ce6d-126">Configure global distribution using the Azure portal</span></span>
> * <span data-ttu-id="8ce6d-127">Konfigurera globala distribution via DocumentDB APIs</span><span class="sxs-lookup"><span data-stu-id="8ce6d-127">Configure global distribution using the DocumentDB APIs</span></span>

<span data-ttu-id="8ce6d-128">Du kan nu fortsätta till nästa kurs att lära dig hur du utvecklar lokalt med hjälp av lokala Azure DB som Cosmos-emulatorn.</span><span class="sxs-lookup"><span data-stu-id="8ce6d-128">You can now proceed to the next tutorial to learn how to develop locally using the Azure Cosmos DB local emulator.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="8ce6d-129">Utveckla lokalt med emulatorn</span><span class="sxs-lookup"><span data-stu-id="8ce6d-129">Develop locally with the emulator</span></span>](local-emulator.md)