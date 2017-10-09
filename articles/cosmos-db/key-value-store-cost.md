---
title: "aaaAzure Cosmos DB som en nyckel/värde-lagring – kostnaden översikt | Microsoft Docs"
description: "Läs mer om hello låg kostnad för att använda Azure Cosmos DB som en nyckel/värde-arkivet."
keywords: "värdet för nyckeln store"
services: cosmos-db
author: mimig1
manager: jhubbard
editor: 
tags: 
documentationcenter: 
ms.assetid: 7f765c17-8549-4509-9475-46394fc3a218
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/28/2017
ms.author: mimig
ms.openlocfilehash: de7207760a8e1fca0e30f951109748835dabf4a3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-cosmos-db-as-a-key-value-store--cost-overview"></a>Azure Cosmos-DB som en nyckel/värde-lagring – kostnaden översikt

Azure Cosmos-DB är en global och flera olika modeller databastjänst för att skapa hög tillgänglighet, stor skala program enkelt. Som standard indexerar Azure Cosmos DB automatiskt alla hello data den en effektivt. Detta gör att snabb och konsekvent [SQL](documentdb-sql-query.md) (och [JavaScript](programming.md)) frågor för alla typer av data. 

Den här artikeln beskriver hello kostnaden för Azure Cosmos DB för att enkelt skriva och läsåtgärder när den används som en nyckel/värde-arkivet. Skriva operations inkluderar infogningar, ersätter, borttagningar och upserts av dokument. Förutom garanterar 99,99% tillgänglighet, erbjuder Azure Cosmos DB garanteras < 10 ms latens för läsning och < 15 ms fördröjning för hello (indexerade), skriver vid hello 99th percentilen. 

## <a name="why-we-use-request-units-rus"></a>Varför vi använda begära enheter (RUs)

Azure DB Cosmos-prestanda är baserad på hello mängden etablerade [begära enheter](request-units.md) (RU) för hello partition. hello etablering på en andra kornighet och köps i RUs per sekund och RUs per minut ([inte toobe förväxlas med hello varje timme fakturering](https://azure.microsoft.com/pricing/details/cosmos-db/)). RUs bör betraktas som en valuta som förenklar hello etablering av nödvändiga genomströmning för hello program. Våra kunder har inte toothink av skilja läsa och skriva kapacitetsenheter. hello enskild valuta modell för RUs skapar effektivitet tooshare hello etablerad kapacitet mellan läsningar och skrivningar. Den här modellen etablerad kapacitet kan hello service tooprovide en förutsägbar och konsekvent genomströmning garanteras låg latens och hög tillgänglighet. Slutligen vi använda RU toomodel genomflöde, men varje etablerade RU har också en definierad mängd resurser (minne, Core). RU/s är inte bara IOPS.

Som ett globalt distribuerad databassystem är Cosmos DB hello endast Azure-tjänst som tillhandahåller ett serviceavtal på svarstid, genomflöde och konsekvens i tillägg toohigh tillgänglighet. hello dataflödet som du etablerar är tillämpade tooeach hello regioner som är kopplad till ditt konto för Cosmos-DB-databasen. För läsåtgärder Cosmos DB erbjuder flera väldefinierade [konsekvensnivåer](consistency-levels.md) för toochoose från. 

hello följande tabell visar hello antalet RUs krävs tooperform läsa och skriva transaktioner som är baserade på dokumentstorlek på 1KB och 100KBs.

|Objektets storlek|1 Läs|1 skrivning|
|-------------|------|-------|
|1 KB|1 RU|5 RUs|
|100 KB|10 RUs|50 RUs|

## <a name="cost-of-reads-and-writes"></a>Kostnaden för läsning och skrivning

Om du etablerar 1 000 RU/s detta belopp too3.6m RU per timme och kostar $0.08 hello timme (i hello USA och Europa). För ett dokument med storleken 1KB, innebär detta att du kan använda 3,6 m läser eller skriver 0.72m (3.6mRU / 5) med hjälp av din dataflöde. Normaliserat toomillion läser och skriver, hello kostnaden skulle vara $0,022 /m läsningar ($0.08 / 3,6) och $0.111/ m skriver ($0.08 / 0.72). hello kostnad per miljoner blir minimal enligt hello tabellen nedan.

|Objektets storlek|1 miljon Läs|1 miljon skrivning|
|-------------|-------|--------|
|1 KB|$0.022|$0.111|
|100 KB|$0.222|$1.111|


De flesta av grundläggande hello-blob eller objektet lagrar services kostnad $0,40 per miljoner lästransaktioner och $5 per transaktion miljoner skrivåtgärder. Om används optimalt kan Cosmos DB vara upp too98% billigare än dessa lösningar (för 1KB transaktioner).

## <a name="next-steps"></a>Nästa steg

Håll ögonen öppna för nya artiklar om hur du optimerar Azure Cosmos DB resursetablering. I hello tiden känna sig fria toouse vår [RU Kalkylatorn](https://www.documentdb.com/capacityplanner).

