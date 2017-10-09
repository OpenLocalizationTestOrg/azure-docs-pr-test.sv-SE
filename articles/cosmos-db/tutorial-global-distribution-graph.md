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
# <a name="how-toosetup-azure-cosmos-db-global-distribution-using-hello-graph-api"></a>Hur toosetup Azure Cosmos DB global distributionsplatsen med hello Graph API

I den här artikeln visar vi hur toouse hello Azure portal toosetup Azure Cosmos DB global distributionsplatsen och sedan ansluta med hello Graph API (förhandsversion).

Den här artikeln beskriver hello följande uppgifter: 

> [!div class="checklist"]
> * Konfigurera distributionslistor med hello Azure-portalen
> * Konfigurera distributionslistor med hello [Graph API: er](graph-introduction.md) (förhandsgranskning)

[!INCLUDE [cosmos-db-tutorial-global-distribution-portal](../../includes/cosmos-db-tutorial-global-distribution-portal.md)]


## <a name="connecting-tooa-preferred-region-using-hello-graph-api-using-hello-net-sdk"></a>Ansluta tooa önskad region med hello Graph API: et med hello .NET SDK

hello Graph API exponeras som ett tillägg bibliotek ovanpå hello DocumentDB SDK.

I ordning tootake nytta av [global distributionsplatsen](distribute-data-globally.md), klientprogram kan ange hello sorterade inställningar lista över regioner toobe används tooperform dokumentet åtgärder. Detta kan göras genom att ange hello anslutningsprincip. Baserat på hello Azure Cosmos DB kontokonfigurationen anges regional tillgänglighet och hello inställningar lista hello de flesta optimala slutpunkten ska väljs av hello SDK tooperform skrivning och läsåtgärder.

Den här inställningen listan anges när initierar en anslutning med hello SDK: er. hello SDK acceptera en valfri parameter ”PreferredLocations” som en sorterad lista över Azure-regioner.

* **Skriver**: hello SDK skickar automatiskt alla skriver toohello aktuella skrivåtgärder region.
* **Läser**: alla Läs skickas toohello första tillgängliga region i hello PreferredLocations lista. Om hello begäran misslyckas hello klienten misslyckas ned hello listan toohello nästa område, och så vidare. hello SDK försöker tooread från hello regioner som anges i PreferredLocations. Så, till exempel om hello Cosmos-DB-konto finns i tre regioner, men hello klienten endast anger två hello-write regioner för PreferredLocations, sedan utan läsning hanteras utanför hello skrivåtgärder region, även i hello fall av redundans.

hello-programmet kan verifiera hello aktuella skrivåtgärder slutpunkt och läsa slutpunkt som valts av hello SDK av kontrollerar två egenskaper, WriteEndpoint och ReadEndpoint, finns i SDK-version 1.8 och senare. Om hello PreferredLocations egenskapen inte anges kommer alla begäranden hanteras från hello aktuella skrivåtgärder region.

### <a name="using-hello-sdk"></a>Med hjälp av hello SDK

Till exempel i hello .NET SDK, hello `ConnectionPolicy` parameter för hello `DocumentClient` konstruktorn har en egenskap som kallas `PreferredLocations`. Den här egenskapen kan anges tooa region namnlista. hello visningsnamnen för [Azure-regioner] [ regions] kan anges som en del av `PreferredLocations`.

> [!NOTE]
> hello URL: er för hello slutpunkter ska inte betraktas som långlivade konstanter. hello-tjänsten kan uppdatera dem när som helst. hello SDK hanterar automatiskt den här ändringen.
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

Det är den som Slutför den här kursen. Du kan lära dig hur toomanage hello konsekvenskontroll av globalt replikerade kontot genom att läsa [konsekvensnivåer i Azure Cosmos DB](consistency-levels.md). Och för mer information om hur globala databasreplikering fungerar i Azure Cosmos DB, se [distribuera data globalt med Azure Cosmos DB](distribute-data-globally.md).

## <a name="next-steps"></a>Nästa steg

I den här kursen har du gjort hello följande:

> [!div class="checklist"]
> * Konfigurera distributionslistor med hello Azure-portalen
> * Konfigurera distributionslistor med hello DocumentDB APIs

Du kan nu fortsätta toohello nästa självstudiekurs toolearn hur toodevelop lokalt med hjälp av hello Azure Cosmos DB lokala emulatorn.

> [!div class="nextstepaction"]
> [Utveckla lokalt med hello-emulatorn](local-emulator.md)

[regions]: https://azure.microsoft.com/regions/

