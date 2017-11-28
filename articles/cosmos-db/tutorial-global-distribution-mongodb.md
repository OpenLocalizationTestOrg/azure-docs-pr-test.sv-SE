---
title: "aaaAzure Cosmos DB global distributionsplatsen självstudier för MongoDB API | Microsoft Docs"
description: "Lär dig hur toosetup Azure Cosmos DB global distributionsplatsen med hello MongoDB-API."
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
ms.openlocfilehash: 0fc2d670bb4e21ac5f813f9586b407ba06ccf354
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toosetup-azure-cosmos-db-global-distribution-using-hello-mongodb-api"></a><span data-ttu-id="5e4be-104">Hur toosetup Azure Cosmos DB global distributionsplatsen med hello MongoDB-API</span><span class="sxs-lookup"><span data-stu-id="5e4be-104">How toosetup Azure Cosmos DB global distribution using hello MongoDB API</span></span>

<span data-ttu-id="5e4be-105">I den här artikeln visar vi hur toouse hello Azure portal toosetup Azure Cosmos DB global distributionsplatsen och sedan ansluta med hello MongoDB-API.</span><span class="sxs-lookup"><span data-stu-id="5e4be-105">In this article, we show how toouse hello Azure portal toosetup Azure Cosmos DB global distribution and then connect using hello MongoDB API.</span></span>

<span data-ttu-id="5e4be-106">Den här artikeln beskriver hello följande uppgifter:</span><span class="sxs-lookup"><span data-stu-id="5e4be-106">This article covers hello following tasks:</span></span> 

> [!div class="checklist"]
> * <span data-ttu-id="5e4be-107">Konfigurera distributionslistor med hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="5e4be-107">Configure global distribution using hello Azure portal</span></span>
> * <span data-ttu-id="5e4be-108">Konfigurera distributionslistor med hello [MongoDB-API](mongodb-introduction.md)</span><span class="sxs-lookup"><span data-stu-id="5e4be-108">Configure global distribution using hello [MongoDB API](mongodb-introduction.md)</span></span>

[!INCLUDE [cosmos-db-tutorial-global-distribution-portal](../../includes/cosmos-db-tutorial-global-distribution-portal.md)]

## <a name="verifying-your-regional-setup-using-hello-mongodb-api"></a><span data-ttu-id="5e4be-109">Verifiera din regionala installationen med hjälp av hello MongoDB-API</span><span class="sxs-lookup"><span data-stu-id="5e4be-109">Verifying your regional setup using hello MongoDB API</span></span>
<span data-ttu-id="5e4be-110">hello enklaste sättet för double globala konfigurationen inom API kontrolleras för MongoDB är toorun hello *isMaster()* från hello Mongo-gränssnittet.</span><span class="sxs-lookup"><span data-stu-id="5e4be-110">hello simplest way of double checking your global configuration within API for MongoDB is toorun hello *isMaster()* command from hello Mongo Shell.</span></span>

<span data-ttu-id="5e4be-111">Från din Mongo Shell:</span><span class="sxs-lookup"><span data-stu-id="5e4be-111">From your Mongo Shell:</span></span>

   ```
      db.isMaster()
   ```
   
<span data-ttu-id="5e4be-112">Exempel resultat:</span><span class="sxs-lookup"><span data-stu-id="5e4be-112">Example results:</span></span>

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

## <a name="connecting-tooa-preferred-region-using-hello-mongodb-api"></a><span data-ttu-id="5e4be-113">Ansluta tooa önskad region med hello MongoDB-API</span><span class="sxs-lookup"><span data-stu-id="5e4be-113">Connecting tooa preferred region using hello MongoDB API</span></span>

<span data-ttu-id="5e4be-114">Hej MongoDB-API kan du toospecify din samling Läs inställningar för ett globalt distribuerad databas.</span><span class="sxs-lookup"><span data-stu-id="5e4be-114">hello MongoDB API enables you toospecify your collection's read preference for a globally distributed database.</span></span> <span data-ttu-id="5e4be-115">För både låg latens läsningar och globala hög tillgänglighet rekommenderar vi att ange din samling läsa inställning för*närmaste*.</span><span class="sxs-lookup"><span data-stu-id="5e4be-115">For both low latency reads and global high availability, we recommend setting your collection's read preference too*nearest*.</span></span> <span data-ttu-id="5e4be-116">Läs inställningen för *närmaste* är konfigurerade tooread från hello närmaste region.</span><span class="sxs-lookup"><span data-stu-id="5e4be-116">A read preference of *nearest* is configured tooread from hello closest region.</span></span>

```csharp
var collection = database.GetCollection<BsonDocument>(collectionName);
collection = collection.WithReadPreference(new ReadPreference(ReadPreferenceMode.Nearest));
```

<span data-ttu-id="5e4be-117">För program med en primär läsning/skrivning och en sekundär region för katastrofåterställning (DR) scenarier, rekommenderar vi att ange din samling läsa inställning för*sekundära önskad*.</span><span class="sxs-lookup"><span data-stu-id="5e4be-117">For applications with a primary read/write region and a secondary region for disaster recovery (DR) scenarios, we recommend setting your collection's read preference too*secondary preferred*.</span></span> <span data-ttu-id="5e4be-118">Läs inställningen för *sekundära önskade* är konfigurerade tooread från hello sekundär region när hello primära regionen är inte tillgänglig.</span><span class="sxs-lookup"><span data-stu-id="5e4be-118">A read preference of *secondary preferred* is configured tooread from hello secondary region when hello primary region is unavailable.</span></span>

```csharp
var collection = database.GetCollection<BsonDocument>(collectionName);
collection = collection.WithReadPreference(new ReadPreference(ReadPreferenceMode.SecondaryPreferred));
```

<span data-ttu-id="5e4be-119">Slutligen, om du skulle t.ex toomanually anger din skrivskyddade regioner.</span><span class="sxs-lookup"><span data-stu-id="5e4be-119">Lastly, if you would like toomanually specify your read regions.</span></span> <span data-ttu-id="5e4be-120">Du kan ange hello region taggen inom önskemål skrivskyddade.</span><span class="sxs-lookup"><span data-stu-id="5e4be-120">You can set hello region Tag within your read preference.</span></span>

```csharp
var collection = database.GetCollection<BsonDocument>(collectionName);
var tag = new Tag("region", "Southeast Asia");
collection = collection.WithReadPreference(new ReadPreference(ReadPreferenceMode.Secondary, new[] { new TagSet(new[] { tag }) }));
```

<span data-ttu-id="5e4be-121">Det är den som Slutför den här kursen.</span><span class="sxs-lookup"><span data-stu-id="5e4be-121">That's it, that completes this tutorial.</span></span> <span data-ttu-id="5e4be-122">Du kan lära dig hur toomanage hello konsekvenskontroll av globalt replikerade kontot genom att läsa [konsekvensnivåer i Azure Cosmos DB](consistency-levels.md).</span><span class="sxs-lookup"><span data-stu-id="5e4be-122">You can learn how toomanage hello consistency of your globally replicated account by reading [Consistency levels in Azure Cosmos DB](consistency-levels.md).</span></span> <span data-ttu-id="5e4be-123">Och för mer information om hur globala databasreplikering fungerar i Azure Cosmos DB, se [distribuera data globalt med Azure Cosmos DB](distribute-data-globally.md).</span><span class="sxs-lookup"><span data-stu-id="5e4be-123">And for more information about how global database replication works in Azure Cosmos DB, see [Distribute data globally with Azure Cosmos DB](distribute-data-globally.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="5e4be-124">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="5e4be-124">Next steps</span></span>

<span data-ttu-id="5e4be-125">I den här kursen har du gjort hello följande:</span><span class="sxs-lookup"><span data-stu-id="5e4be-125">In this tutorial, you've done hello following:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="5e4be-126">Konfigurera distributionslistor med hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="5e4be-126">Configure global distribution using hello Azure portal</span></span>
> * <span data-ttu-id="5e4be-127">Konfigurera distributionslistor med hello DocumentDB APIs</span><span class="sxs-lookup"><span data-stu-id="5e4be-127">Configure global distribution using hello DocumentDB APIs</span></span>

<span data-ttu-id="5e4be-128">Du kan nu fortsätta toohello nästa självstudiekurs toolearn hur toodevelop lokalt med hjälp av hello Azure Cosmos DB lokala emulatorn.</span><span class="sxs-lookup"><span data-stu-id="5e4be-128">You can now proceed toohello next tutorial toolearn how toodevelop locally using hello Azure Cosmos DB local emulator.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="5e4be-129">Utveckla lokalt med hello-emulatorn</span><span class="sxs-lookup"><span data-stu-id="5e4be-129">Develop locally with hello emulator</span></span>](local-emulator.md)
