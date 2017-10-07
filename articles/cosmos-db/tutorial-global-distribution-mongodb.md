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
# <a name="how-toosetup-azure-cosmos-db-global-distribution-using-hello-mongodb-api"></a>Hur toosetup Azure Cosmos DB global distributionsplatsen med hello MongoDB-API

I den här artikeln visar vi hur toouse hello Azure portal toosetup Azure Cosmos DB global distributionsplatsen och sedan ansluta med hello MongoDB-API.

Den här artikeln beskriver hello följande uppgifter: 

> [!div class="checklist"]
> * Konfigurera distributionslistor med hello Azure-portalen
> * Konfigurera distributionslistor med hello [MongoDB-API](mongodb-introduction.md)

[!INCLUDE [cosmos-db-tutorial-global-distribution-portal](../../includes/cosmos-db-tutorial-global-distribution-portal.md)]

## <a name="verifying-your-regional-setup-using-hello-mongodb-api"></a>Verifiera din regionala installationen med hjälp av hello MongoDB-API
hello enklaste sättet för double globala konfigurationen inom API kontrolleras för MongoDB är toorun hello *isMaster()* från hello Mongo-gränssnittet.

Från din Mongo Shell:

   ```
      db.isMaster()
   ```
   
Exempel resultat:

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

## <a name="connecting-tooa-preferred-region-using-hello-mongodb-api"></a>Ansluta tooa önskad region med hello MongoDB-API

Hej MongoDB-API kan du toospecify din samling Läs inställningar för ett globalt distribuerad databas. För både låg latens läsningar och globala hög tillgänglighet rekommenderar vi att ange din samling läsa inställning för*närmaste*. Läs inställningen för *närmaste* är konfigurerade tooread från hello närmaste region.

```csharp
var collection = database.GetCollection<BsonDocument>(collectionName);
collection = collection.WithReadPreference(new ReadPreference(ReadPreferenceMode.Nearest));
```

För program med en primär läsning/skrivning och en sekundär region för katastrofåterställning (DR) scenarier, rekommenderar vi att ange din samling läsa inställning för*sekundära önskad*. Läs inställningen för *sekundära önskade* är konfigurerade tooread från hello sekundär region när hello primära regionen är inte tillgänglig.

```csharp
var collection = database.GetCollection<BsonDocument>(collectionName);
collection = collection.WithReadPreference(new ReadPreference(ReadPreferenceMode.SecondaryPreferred));
```

Slutligen, om du skulle t.ex toomanually anger din skrivskyddade regioner. Du kan ange hello region taggen inom önskemål skrivskyddade.

```csharp
var collection = database.GetCollection<BsonDocument>(collectionName);
var tag = new Tag("region", "Southeast Asia");
collection = collection.WithReadPreference(new ReadPreference(ReadPreferenceMode.Secondary, new[] { new TagSet(new[] { tag }) }));
```

Det är den som Slutför den här kursen. Du kan lära dig hur toomanage hello konsekvenskontroll av globalt replikerade kontot genom att läsa [konsekvensnivåer i Azure Cosmos DB](consistency-levels.md). Och för mer information om hur globala databasreplikering fungerar i Azure Cosmos DB, se [distribuera data globalt med Azure Cosmos DB](distribute-data-globally.md).

## <a name="next-steps"></a>Nästa steg

I den här kursen har du gjort hello följande:

> [!div class="checklist"]
> * Konfigurera distributionslistor med hello Azure-portalen
> * Konfigurera distributionslistor med hello DocumentDB APIs

Du kan nu fortsätta toohello nästa självstudiekurs toolearn hur toodevelop lokalt med hjälp av hello Azure Cosmos DB lokala emulatorn.

> [!div class="nextstepaction"]
> [Utveckla lokalt med hello-emulatorn](local-emulator.md)
