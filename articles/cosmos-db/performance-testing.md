---
title: Azure DB Cosmos skala och prestandatester | Microsoft Docs
description: "Lär dig hur du utföra skalnings- och prestandatester med Azure Cosmos DB"
keywords: Prestandatestning
services: cosmos-db
author: arramac
manager: jhubbard
editor: 
documentationcenter: 
ms.assetid: f4c96ebd-f53c-427d-a500-3f28fe7b11d0
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/19/2017
ms.author: arramac
ms.openlocfilehash: b5a1edd08819e82437c5b22d8eb131665d7c9645
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="performance-and-scale-testing-with-azure-cosmos-db"></a>Prestanda och skalning testa med Azure Cosmos DB
Prestanda och skalningstester är ett viktigt steg programutveckling. För många program på databasnivå har en betydande inverkan på övergripande prestanda och skalbarhet och är därför en kritisk komponent i prestandatester. [Azure Cosmos-DB](https://azure.microsoft.com/services/cosmos-db/) är specialbyggt för elastisk skalbarhet och förutsägbar prestanda och därför ett bra val för program som behöver en hög prestanda databasnivå. 

Den här artikeln är en referens för utvecklare implementera prestanda testpaket för sina Cosmos-DB-arbetsbelastningar eller utvärderar Cosmos DB för högpresterande Programscenarier. Den fokuserar huvudsakligen på isolerade prestandatester på databasen, men det innehåller även rekommenderade metoder för program i produktion.

När du har läst den här artikeln kommer du att kunna svara på följande frågor:   

* Var hittar jag exempel klientprogrammet för .NET för prestandatestning av Cosmos DB 
* Hur uppnå hög genomströmning nivåer med Cosmos DB från min klientprogrammet?

Om du vill komma igång med koden hämta projektet från [Azure Cosmos DB prestanda testning Sample](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/documentdb-benchmark). 

> [!NOTE]
> Målet med det här programmet är att visa Metodtips för att extrahera bättre prestanda från Cosmos-databas med ett litet antal klientdatorer. Detta gjordes inte att visa högsta kapacitet för tjänsten, som kan skalas utan begränsningar.
> 
> 

Om du letar efter klientsidan konfigurationsalternativ för att förbättra prestanda för Cosmos DB [Azure Cosmos DB prestandatips](performance-tips.md).

## <a name="run-the-performance-testing-application"></a>Kör programmet för prestandatestning
Det snabbaste sättet att komma igång är att kompilera och köra .NET-exemplet nedan, som beskrivs i stegen nedan. Du kan också granska källkoden och implementera liknande konfigurationer för egna klientprogram.

**Steg 1:** hämta projektet från [Azure Cosmos DB prestanda testning Sample](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/documentdb-benchmark), eller duplicera GitHub-lagringsplatsen.

**Steg 2:** ändra inställningarna för EndpointUrl, AuthorizationKey, CollectionThroughput och DocumentTemplate (valfritt) i App.config.

> [!NOTE]
> Innan du etablerar samlingar med hög genomströmning, referera till den [prissättning](https://azure.microsoft.com/pricing/details/cosmos-db/) att uppskatta kostnader per samling. Azure Cosmos databaslagring fakturor och genomströmning oberoende på timbasis, så kan du spara kostnader genom att radera eller sänker genomflödet i Azure DB som Cosmos-samlingar efter testning.
> 
> 

**Steg 3:** kompilera och köra konsolapp-från kommandoraden. Du bör se utdata som liknar följande:

    Summary:
    ---------------------------------------------------------------------
    Endpoint: https://docdb-scale-demo.documents.azure.com:443/
    Collection : db.testdata at 50000 request units per second
    Document Template*: Player.json
    Degree of parallelism*: 500
    ---------------------------------------------------------------------

    DocumentDBBenchmark starting...
    Creating database db
    Creating collection testdata
    Creating metric collection metrics
    Retrying after sleeping for 00:03:34.1720000
    Starting Inserts with 500 tasks
    Inserted 661 docs @ 656 writes/s, 6860 RU/s (18B max monthly 1KB reads)
    Inserted 6505 docs @ 2668 writes/s, 27962 RU/s (72B max monthly 1KB reads)
    Inserted 11756 docs @ 3240 writes/s, 33957 RU/s (88B max monthly 1KB reads)
    Inserted 17076 docs @ 3590 writes/s, 37627 RU/s (98B max monthly 1KB reads)
    Inserted 22106 docs @ 3748 writes/s, 39281 RU/s (102B max monthly 1KB reads)
    Inserted 28430 docs @ 3902 writes/s, 40897 RU/s (106B max monthly 1KB reads)
    Inserted 33492 docs @ 3928 writes/s, 41168 RU/s (107B max monthly 1KB reads)
    Inserted 38392 docs @ 3963 writes/s, 41528 RU/s (108B max monthly 1KB reads)
    Inserted 43371 docs @ 4012 writes/s, 42051 RU/s (109B max monthly 1KB reads)
    Inserted 48477 docs @ 4035 writes/s, 42282 RU/s (110B max monthly 1KB reads)
    Inserted 53845 docs @ 4088 writes/s, 42845 RU/s (111B max monthly 1KB reads)
    Inserted 59267 docs @ 4138 writes/s, 43364 RU/s (112B max monthly 1KB reads)
    Inserted 64703 docs @ 4197 writes/s, 43981 RU/s (114B max monthly 1KB reads)
    Inserted 70428 docs @ 4216 writes/s, 44181 RU/s (115B max monthly 1KB reads)
    Inserted 75868 docs @ 4247 writes/s, 44505 RU/s (115B max monthly 1KB reads)
    Inserted 81571 docs @ 4280 writes/s, 44852 RU/s (116B max monthly 1KB reads)
    Inserted 86271 docs @ 4273 writes/s, 44783 RU/s (116B max monthly 1KB reads)
    Inserted 91993 docs @ 4299 writes/s, 45056 RU/s (117B max monthly 1KB reads)
    Inserted 97469 docs @ 4292 writes/s, 44984 RU/s (117B max monthly 1KB reads)
    Inserted 99736 docs @ 4192 writes/s, 43930 RU/s (114B max monthly 1KB reads)
    Inserted 99997 docs @ 4013 writes/s, 42051 RU/s (109B max monthly 1KB reads)
    Inserted 100000 docs @ 3846 writes/s, 40304 RU/s (104B max monthly 1KB reads)

    Summary:
    ---------------------------------------------------------------------
    Inserted 100000 docs @ 3834 writes/s, 40180 RU/s (104B max monthly 1KB reads)
    ---------------------------------------------------------------------
    DocumentDBBenchmark completed successfully.


**Steg 4 (vid behov):** genomflödet rapporterade (RU/s) från verktyget ska vara samma eller högre än det tillhandahållna dataflödet i mängden. Om inte, öka DegreeOfParallelism i små steg kan hjälpa dig att nå gränsen. Om genomströmning från din klient högplaåter hjälper starta flera instanser av appen på samma eller olika datorer dig att nå etablerade gränsen mellan olika instanser. Om du behöver hjälp med det här steget, Skriv ett e-postmeddelande till askcosmosdb@microsoft.com eller filen ett supportärende från den [Azure Portal](https://portal.azure.com).

När du har appen körs försöker du att olika [indexering principer](indexing-policies.md) och [konsekvensnivåer](consistency-levels.md) att förstå deras inverkan på genomflöde och svarstid. Du kan också granska källkoden och implementera liknande konfigurationer till testpaket eller program i produktion.

## <a name="next-steps"></a>Nästa steg
I den här artikeln vi har tittat på hur du kan utföra prestanda och skalning testning med Cosmos-databasen med en .NET-konsolapp. Se länkarna nedan för mer information om hur du arbetar med Azure Cosmos DB.

* [Azure DB Cosmos prestandatestning exempel](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/documentdb-benchmark)
* [Klienten konfigurationsalternativ för att förbättra prestanda för Azure Cosmos DB](performance-tips.md)
* [Serversidan partitionering i Azure Cosmos DB](partition-data.md)


