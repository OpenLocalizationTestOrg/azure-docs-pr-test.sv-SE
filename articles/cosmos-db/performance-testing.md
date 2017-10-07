---
title: aaaAzure Cosmos DB skala och prestandatester | Microsoft Docs
description: "Lär dig hur tooperform skala och prestandatester med Azure Cosmos DB"
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
ms.openlocfilehash: 46d1217e11a39ee970a868de9a5c5dfcf52cedf3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="performance-and-scale-testing-with-azure-cosmos-db"></a><span data-ttu-id="58a0a-104">Prestanda och skalning testa med Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="58a0a-104">Performance and scale testing with Azure Cosmos DB</span></span>
<span data-ttu-id="58a0a-105">Prestanda och skalningstester är ett viktigt steg programutveckling.</span><span class="sxs-lookup"><span data-stu-id="58a0a-105">Performance and scale testing is a key step in application development.</span></span> <span data-ttu-id="58a0a-106">För många program hello databasnivå har en betydande inverkan på hello övergripande prestanda och skalbarhet och är därför en kritisk komponent i prestanda testning.</span><span class="sxs-lookup"><span data-stu-id="58a0a-106">For many applications, hello database tier has a significant impact on hello overall performance and scalability, and is therefore a critical component of performance testing.</span></span> <span data-ttu-id="58a0a-107">[Azure Cosmos-DB](https://azure.microsoft.com/services/cosmos-db/) är specialbyggt för elastisk skalbarhet och förutsägbar prestanda och därför ett bra val för program som behöver en hög prestanda databasnivå.</span><span class="sxs-lookup"><span data-stu-id="58a0a-107">[Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) is purpose-built for elastic scale and predictable performance, and therefore a great fit for applications that need a high-performance database tier.</span></span> 

<span data-ttu-id="58a0a-108">Den här artikeln är en referens för utvecklare implementera prestanda testpaket för sina Cosmos-DB-arbetsbelastningar eller utvärderar Cosmos DB för högpresterande Programscenarier.</span><span class="sxs-lookup"><span data-stu-id="58a0a-108">This article is a reference for developers implementing performance test suites for their Cosmos DB workloads, or evaluating Cosmos DB for high-performance application scenarios.</span></span> <span data-ttu-id="58a0a-109">Den fokuserar huvudsakligen på isolerade prestandatester av hello-databasen, men det innehåller även rekommenderade metoder för program i produktion.</span><span class="sxs-lookup"><span data-stu-id="58a0a-109">It focuses primarily on isolated performance testing of hello database, but also includes best practices for production applications.</span></span>

<span data-ttu-id="58a0a-110">När du har läst den här artikeln kommer du att kunna tooanswer hello följande frågor:</span><span class="sxs-lookup"><span data-stu-id="58a0a-110">After reading this article, you will be able tooanswer hello following questions:</span></span>   

* <span data-ttu-id="58a0a-111">Var hittar jag exempel klientprogrammet för .NET för prestandatestning av Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="58a0a-111">Where can I find a sample .NET client application for performance testing of Cosmos DB?</span></span> 
* <span data-ttu-id="58a0a-112">Hur uppnå hög genomströmning nivåer med Cosmos DB från min klientprogrammet?</span><span class="sxs-lookup"><span data-stu-id="58a0a-112">How do I achieve high throughput levels with Cosmos DB from my client application?</span></span>

<span data-ttu-id="58a0a-113">tooget igång med kod, ladda ned hello projektet från [Azure Cosmos DB prestanda testning Sample](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/documentdb-benchmark).</span><span class="sxs-lookup"><span data-stu-id="58a0a-113">tooget started with code, please download hello project from [Azure Cosmos DB Performance Testing Sample](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/documentdb-benchmark).</span></span> 

> [!NOTE]
> <span data-ttu-id="58a0a-114">hello syftet med det här programmet är toodemonstrate bästa praxis för att extrahera bättre prestanda från Cosmos-databas med ett litet antal klientdatorer.</span><span class="sxs-lookup"><span data-stu-id="58a0a-114">hello goal of this application is toodemonstrate best practices for extracting better performance out of Cosmos DB with a small number of client machines.</span></span> <span data-ttu-id="58a0a-115">Detta gjordes inte toodemonstrate hello högsta kapacitet för hello-tjänsten, som kan skalas utan begränsningar.</span><span class="sxs-lookup"><span data-stu-id="58a0a-115">This was not made toodemonstrate hello peak capacity of hello service, which can scale limitlessly.</span></span>
> 
> 

<span data-ttu-id="58a0a-116">Om du letar efter klientsidans konfiguration alternativ tooimprove Cosmos DB prestanda, se [Azure Cosmos DB prestandatips](performance-tips.md).</span><span class="sxs-lookup"><span data-stu-id="58a0a-116">If you're looking for client-side configuration options tooimprove Cosmos DB performance, see [Azure Cosmos DB performance tips](performance-tips.md).</span></span>

## <a name="run-hello-performance-testing-application"></a><span data-ttu-id="58a0a-117">Köra hello testa program</span><span class="sxs-lookup"><span data-stu-id="58a0a-117">Run hello performance testing application</span></span>
<span data-ttu-id="58a0a-118">Hej snabbaste sättet tooget är igång toocompile och kör hello .NET exemplet nedan, enligt beskrivningen i hello stegen nedan.</span><span class="sxs-lookup"><span data-stu-id="58a0a-118">hello quickest way tooget started is toocompile and run hello .NET sample below, as described in hello steps below.</span></span> <span data-ttu-id="58a0a-119">Du kan också granska hello källkoden och implementera liknande konfigurationer tooyour egna klientprogram.</span><span class="sxs-lookup"><span data-stu-id="58a0a-119">You can also review hello source code and implement similar configurations tooyour own client applications.</span></span>

<span data-ttu-id="58a0a-120">**Steg 1:** Download hello projektet från [Azure Cosmos DB prestanda testning Sample](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/documentdb-benchmark), eller förgrening hello GitHub-lagringsplatsen.</span><span class="sxs-lookup"><span data-stu-id="58a0a-120">**Step 1:** Download hello project from [Azure Cosmos DB Performance Testing Sample](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/documentdb-benchmark), or fork hello GitHub repository.</span></span>

<span data-ttu-id="58a0a-121">**Steg 2:** ändra hello inställningar för EndpointUrl, AuthorizationKey, CollectionThroughput och DocumentTemplate (valfritt) i App.config.</span><span class="sxs-lookup"><span data-stu-id="58a0a-121">**Step 2:** Modify hello settings for EndpointUrl, AuthorizationKey, CollectionThroughput and DocumentTemplate (optional) in App.config.</span></span>

> [!NOTE]
> <span data-ttu-id="58a0a-122">Innan du etablerar samlingar med hög genomströmning Se toohello [prissättning](https://azure.microsoft.com/pricing/details/cosmos-db/) tooestimate hello kostnader per samling.</span><span class="sxs-lookup"><span data-stu-id="58a0a-122">Before provisioning collections with high throughput, please refer toohello [Pricing Page](https://azure.microsoft.com/pricing/details/cosmos-db/) tooestimate hello costs per collection.</span></span> <span data-ttu-id="58a0a-123">Azure Cosmos databaslagring fakturor och genomströmning oberoende på timbasis, så kan du spara kostnader genom att radera eller sänker hello genomflödet i Azure DB som Cosmos-samlingar efter testning.</span><span class="sxs-lookup"><span data-stu-id="58a0a-123">Azure Cosmos DB bills storage and throughput independently on an hourly basis, so you can save costs by deleting or lowering hello throughput of your Azure Cosmos DB collections after testing.</span></span>
> 
> 

<span data-ttu-id="58a0a-124">**Steg 3:** kompilera och köra hello-konsolen från kommandoraden hello.</span><span class="sxs-lookup"><span data-stu-id="58a0a-124">**Step 3:** Compile and run hello console app from hello command line.</span></span> <span data-ttu-id="58a0a-125">Du bör se utdata som liknar hello följande:</span><span class="sxs-lookup"><span data-stu-id="58a0a-125">You should see output like hello following:</span></span>

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


<span data-ttu-id="58a0a-126">**Steg 4 (vid behov):** hello genomströmning rapporterade (RU/s) från hello verktyget ska vara hello samma eller högre än hello etablerat dataflöde hello mängden.</span><span class="sxs-lookup"><span data-stu-id="58a0a-126">**Step 4 (if necessary):** hello throughput reported (RU/s) from hello tool should be hello same or higher than hello provisioned throughput of hello collection.</span></span> <span data-ttu-id="58a0a-127">Om inte, stigande hello DegreeOfParallelism i små steg kan hjälpa dig att nå hello gränsen.</span><span class="sxs-lookup"><span data-stu-id="58a0a-127">If not, increasing hello DegreeOfParallelism in small increments may help you reach hello limit.</span></span> <span data-ttu-id="58a0a-128">Om hello genomströmning från din klient högplaåter starta flera instanser av hello app på hello samma eller olika datorer hjälper dig att nå ut hello etablerats gränsen på hello olika instanser.</span><span class="sxs-lookup"><span data-stu-id="58a0a-128">If hello throughput from your client app plateaus, launching multiple instances of hello app on hello same or different machines will help you reach hello provisioned limit across hello different instances.</span></span> <span data-ttu-id="58a0a-129">Om du behöver hjälp med det här steget, Skriv ett e-postmeddelande tooaskcosmosdb@microsoft.com eller filen ett supportärende från hello [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="58a0a-129">If you need help with this step, please, write an email tooaskcosmosdb@microsoft.com or file a support ticket from hello [Azure Portal](https://portal.azure.com).</span></span>

<span data-ttu-id="58a0a-130">När du har hello appen körs försöker du att olika [indexering principer](indexing-policies.md) och [konsekvensnivåer](consistency-levels.md) toounderstand deras inverkan på genomflöde och svarstid.</span><span class="sxs-lookup"><span data-stu-id="58a0a-130">Once you have hello app running, you can try different [Indexing policies](indexing-policies.md) and [Consistency levels](consistency-levels.md) toounderstand their impact on throughput and latency.</span></span> <span data-ttu-id="58a0a-131">Du kan också granska hello källkoden och implementera liknande konfigurationer tooyour egna testpaket eller program i produktion.</span><span class="sxs-lookup"><span data-stu-id="58a0a-131">You can also review hello source code and implement similar configurations tooyour own test suites or production applications.</span></span>

## <a name="next-steps"></a><span data-ttu-id="58a0a-132">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="58a0a-132">Next steps</span></span>
<span data-ttu-id="58a0a-133">I den här artikeln vi har tittat på hur du kan utföra prestanda och skalning testning med Cosmos-databasen med en .NET-konsolapp.</span><span class="sxs-lookup"><span data-stu-id="58a0a-133">In this article, we looked at how you can perform performance and scale testing with Cosmos DB using a .NET console app.</span></span> <span data-ttu-id="58a0a-134">Toohello länkarna nedan finns mer information om hur du arbetar med Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="58a0a-134">Please refer toohello links below for additional information on working with Azure Cosmos DB.</span></span>

* [<span data-ttu-id="58a0a-135">Azure DB Cosmos prestandatestning exempel</span><span class="sxs-lookup"><span data-stu-id="58a0a-135">Azure Cosmos DB performance testing sample</span></span>](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/documentdb-benchmark)
* [<span data-ttu-id="58a0a-136">Klienten configuration alternativ tooimprove Azure Cosmos DB prestanda</span><span class="sxs-lookup"><span data-stu-id="58a0a-136">Client configuration options tooimprove Azure Cosmos DB performance</span></span>](performance-tips.md)
* [<span data-ttu-id="58a0a-137">Serversidan partitionering i Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="58a0a-137">Server-side partitioning in Azure Cosmos DB</span></span>](partition-data.md)


