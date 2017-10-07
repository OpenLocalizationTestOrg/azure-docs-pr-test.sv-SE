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
# <a name="how-toosetup-azure-cosmos-db-global-distribution-using-hello-table-api"></a>Hur toosetup Azure Cosmos DB global distributionsplatsen med hello tabell-API

I den här artikeln visar vi hur toouse hello Azure portal toosetup Azure Cosmos DB global distributionsplatsen och sedan ansluta med hello tabell-API (förhandsversion).

Den här artikeln beskriver hello följande uppgifter: 

> [!div class="checklist"]
> * Konfigurera distributionslistor med hello Azure-portalen
> * Konfigurera distributionslistor med hello [tabell-API](table-introduction.md)

[!INCLUDE [cosmos-db-tutorial-global-distribution-portal](../../includes/cosmos-db-tutorial-global-distribution-portal.md)]


## <a name="connecting-tooa-preferred-region-using-hello-table-api"></a>Ansluta tooa önskad region med hello tabell-API

I ordning tootake nytta av [global distributionsplatsen](distribute-data-globally.md), klientprogram kan ange hello sorterade inställningar lista över regioner toobe används tooperform dokumentet åtgärder. Detta kan göras genom att ange hello `TablePreferredLocations` Konfigurationsvärdet i hello programkonfiguration för hello preview Azure Storage SDK: N. Baserat på hello Azure Cosmos DB kontokonfigurationen regional tillgänglighet och hello inställningar listan anges de flesta optimala endpoint väljas av hello Azure Storage SDK: N tooperform hello skriva och läsåtgärder.

Hej `TablePreferredLocations` ska innehålla en kommaavgränsad lista över önskade (flera homing) platser för läsning. Varje klientinstans kan ange en delmängd av dessa regioner hello rekommenderas för låg latens läsningar. hello regioner måste namnges med deras [visningsnamn](https://msdn.microsoft.com/library/azure/gg441293.aspx), till exempel `West US`.

Alla Läs skickas toohello första tillgängliga region i hello `TablePreferredLocations` lista. Om hello begäran misslyckas hello klienten misslyckas ned hello listan toohello nästa område, och så vidare.

hello SDK försöker tooread från hello regioner som anges i `TablePreferredLocations`. Så, till exempel om hello databaskonto finns tillgänglig i tre regioner, men hello klienten anger endast två av hello-write regioner för `TablePreferredLocations`, och sedan utan läsning hanteras utanför hello skrivåtgärder region, även i hello fall av redundans.

hello SDK skickar automatiskt alla skrivningar toohello aktuella skrivåtgärder region.

Om hello `TablePreferredLocations` egenskapen har inte angetts, alla begäranden hanteras från hello aktuella skrivåtgärder region.

```xml
    <appSettings>
      <add key="TablePreferredLocations" value="East US, West US, North Europe"/>           
    </appSettings>
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
