---
title: aaaIntroduction tooAzure Cosmos DB | Microsoft Docs
description: "Läs om Azure Cosmos DB. Den här globalt distribuerade databasen med flera modeller har skapats för låg svarstid, elastisk skalbarhet och hög tillgänglighet."
services: cosmos-db
author: mimig1
manager: jhubbard
editor: monicar
documentationcenter: 
ms.assetid: a855183f-34d4-49cc-9609-1478e465c3b7
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/14/2017
ms.author: mimig
ms.custom: mvc
ms.openlocfilehash: f2acbe99f425b2f07a62bbbb4324aa48f1037481
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="welcome-tooazure-cosmos-db"></a>Välkommen till tooAzure Cosmos DB

Azure Cosmos DB är Microsofts globalt distribuerade databas för flera datamodeller. Med hello på en knapp kan Azure Cosmos DB du tooelastically och oberoende skalbara dataflöden och lagringsutrymmen till alla Azures geografiska områden. Det erbjuder garantier när det gäller dataflöde, svarstider, tillgänglighet och konsekvens med omfattande [serviceavtal](https://aka.ms/acdbsla) (SLA:er) något som ingen annan databastjänst kan erbjuda.

![Azure Cosmos DB är Microsofts globalt distribuerade databastjänst med elastisk utskalning, garanterat låga svarstider, fem konsekvensmodeller och omfattande garanterade serviceavtal](./media/introduction/azure-cosmos-db.png)

## <a name="solutions-that-benefit-from-azure-cosmos-db"></a>Lösningar som har nytta av Azure Cosmos DB

Alla [webb, mobilt, spel- och IoT-appar](use-cases.md) som behöver toohandle enorma mängder läsning och skrivning på en [globala](distribute-data-globally.md) skala med låga svarstider för en mängd olika data drar nytta av Azure Cosmos DB [garanteras](https://azure.microsoft.com/support/legal/sla/cosmos-db/) tillgänglighet, hög genomströmning, låg latens och justerbara konsekvenskontroll.

## <a name="key-capabilities"></a>De viktigaste funktionerna
Som ett globalt distribuerad databas-tjänst ger Azure Cosmos DB hello följande funktioner toohelp du skapa skalbara och mycket dynamisk program:

* **Nyckelfärdig global distribution**
    * Du kan [distribuera dina data](distribute-data-globally.md) tooany antalet [Azure-regioner](https://azure.microsoft.com/regions/), med hello [klickar du på en knapp](tutorial-global-distribution-documentdb.md). Detta gör du tooput data där användarna kan säkerställa hello lägsta möjliga tidsfördröjning tooyour kunder. 
    * Med hjälp av Azure Cosmos DB vet flera API: er, hello app alltid var hello närmsta region är och skickar begäranden toohello närmsta datacenter. Allt detta är möjligt utan config ändringar, ställer du skriva region och många läsa regioner som du vill och hello rest hanteras för dig.

* **Flera datamodeller och populära API:er för att komma åt och fråga efter data**
    * hello atom-postsekvensen (ARS) baserat datamodell som Azure Cosmos DB bygger på internt stöd för flera datamodeller, inklusive men inte begränsat toodocument, diagram, nyckel / värde-, tabeller och kolumner datamodeller.
    * API: er för följande datamodeller hello stöds med SDK: er som är tillgängligt på flera språk:
        * [DocumentDB API](documentdb-introduction.md)
        * [MongoDB API](mongodb-introduction.md)
        * [Table API](table-introduction.md)
        * [Graph (Gremlin) API](graph-introduction.md)
        * Ytterligare datamodeller kommer snart 

* **Skala elastiskt dataflöde och lagring på begäran, globalt**
    * Skala enkelt databasflödet med [sekundprecision](request-units.md) och ändra inställningarna när som helst. 
    * Skala lagringsstorlek [transparent och automatiskt](partition-data.md) toohandle din storlekskraven nu och alltid.

* **Bygg högdynamiska och verksamhetskritiska program**
    * Azure Cosmos-DB garanterar slutpunkt till slutpunkt låg svarstid hello 99th percentil tooits kunder. 
    * För en typisk 1 KB-artikel Cosmos DB garanterar fördröjningen slutpunkt till slutpunkt för läsningar under 10 ms och indexerade skrivningar under 15 ms vid hello 99th percentilen, hello inom samma Azure-region. hello median befinner sig avsevärt lägre (under 5 ms).

* **Se till att tillgängligheten alltid är så hög som möjligt**
    * 99.99 % tillgänglighet inom en region.
    * Distribuera tooany antalet [Azure-regioner](https://azure.microsoft.com/regions) för högre tillgänglighet.
    * [Simulera ett fel](regional-failover.md) i en eller flera regioner med garantier om noll dataförlust. 

* **Skriva globalt distribuerade program hello Högerklicka sätt**
    * Fem [konsekvenskontroll modeller](consistency-levels.md) modeller får du ett spektrum av stark SQL-liknande konsekvens alla hello sätt tooNoSQL-liknande slutliga konsekvensen och varje sak mellan. 
  
* **Pengarna tillbaka-garanti**
    * Dina data kommer dit de ska snabbt, eller pengarna tillbaka. 
    * [Serviceavtal](https://aka.ms/acdbsla) för tillgänglighet, svarstid, dataflöde och konsekvens. 

* **Ingen hantering av databasscheman/-index**
    * Sluta oroa dig för att hålla dina databasscheman och -index synkroniserade med schemat för ditt program. Vi är schema-fria. 
    * Azure Cosmos-databasen databasmotor är fullständigt schema-oberoende – indexerar automatiskt alla hello data den en utan att något schema eller index och fungerar blixtsnabb snabb frågor. 

* **Låg totalkostnad**
    * Fem tooten gånger [mer kostnadseffektivt](https://aka.ms/cosmos-db-tco-paper) än en icke-hanterad lösning.
    * Tre gånger billigare än DynamoDB.

## <a name="capability-comparison"></a>Jämförelse av funktioner

Azure Cosmos-DB innehåller hello bästa relationella och icke-relationella databaser.

| Funktioner | Relationsdatabaser   | Icke-relationella databaser (NoSQL) |    Azure Cosmos DB |
| --- | --- | --- | --- |
| Global distribution | Nej | Nej | Ja, nyckelfärdig distribution i fler än 30 områden, med flera API: er|
| Horisontell skalning | Nej | Ja | Ja, oberoende skalning av lagring och dataflöde är möjligt | 
| Svarstidsgarantier | Nej | Ja | Ja, 99 % läsningar på < 10 ms och skrivningar på < 15 ms | 
| Hög tillgänglighet | Nej | Ja | Ja, Cosmos DB är alltid aktiverat, har PACELC-kompromisser och innehåller alternativ för automatisk och manuell redundans vid fel|
| Datamodell + API | Relations + SQL | Multi-modell + OSS API | Multi-modell + SQL + OSS API (mer kommer snart) |
| Serviceavtal | Ja | Nej | Ja, omfattande serviceavtal för svarstider, dataflöde, konsekvens och tillgänglighet |


## <a name="next-steps"></a>Nästa steg
Kom igång med Azure Cosmos DB med någon av våra snabbstarter:

* [Kom igång med DocumentDB-API:n för Azure Cosmos DB](create-documentdb-dotnet.md)
* [Kom igång med MongoDB-API:n för Azure Cosmos DB](create-mongodb-nodejs.md)
* [Kom igång med Graph-API:n för Azure Cosmos DB](create-graph-dotnet.md)
* [Kom igång med Table-API:n för Azure Cosmos DB](create-table-dotnet.md)
