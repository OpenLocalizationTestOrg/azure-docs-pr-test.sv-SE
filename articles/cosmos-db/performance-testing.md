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
# <a name="performance-and-scale-testing-with-azure-cosmos-db"></a><span data-ttu-id="fc134-104">Prestanda och skalning testa med Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="fc134-104">Performance and scale testing with Azure Cosmos DB</span></span>
<span data-ttu-id="fc134-105">Prestanda och skalningstester är ett viktigt steg programutveckling.</span><span class="sxs-lookup"><span data-stu-id="fc134-105">Performance and scale testing is a key step in application development.</span></span> <span data-ttu-id="fc134-106">För många program på databasnivå har en betydande inverkan på övergripande prestanda och skalbarhet och är därför en kritisk komponent i prestandatester.</span><span class="sxs-lookup"><span data-stu-id="fc134-106">For many applications, the database tier has a significant impact on the overall performance and scalability, and is therefore a critical component of performance testing.</span></span> <span data-ttu-id="fc134-107">[Azure Cosmos-DB](https://azure.microsoft.com/services/cosmos-db/) är specialbyggt för elastisk skalbarhet och förutsägbar prestanda och därför ett bra val för program som behöver en hög prestanda databasnivå.</span><span class="sxs-lookup"><span data-stu-id="fc134-107">[Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) is purpose-built for elastic scale and predictable performance, and therefore a great fit for applications that need a high-performance database tier.</span></span> 

<span data-ttu-id="fc134-108">Den här artikeln är en referens för utvecklare implementera prestanda testpaket för sina Cosmos-DB-arbetsbelastningar eller utvärderar Cosmos DB för högpresterande Programscenarier.</span><span class="sxs-lookup"><span data-stu-id="fc134-108">This article is a reference for developers implementing performance test suites for their Cosmos DB workloads, or evaluating Cosmos DB for high-performance application scenarios.</span></span> <span data-ttu-id="fc134-109">Den fokuserar huvudsakligen på isolerade prestandatester på databasen, men det innehåller även rekommenderade metoder för program i produktion.</span><span class="sxs-lookup"><span data-stu-id="fc134-109">It focuses primarily on isolated performance testing of the database, but also includes best practices for production applications.</span></span>

<span data-ttu-id="fc134-110">När du har läst den här artikeln kommer du att kunna svara på följande frågor:</span><span class="sxs-lookup"><span data-stu-id="fc134-110">After reading this article, you will be able to answer the following questions:</span></span>   

* <span data-ttu-id="fc134-111">Var hittar jag exempel klientprogrammet för .NET för prestandatestning av Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="fc134-111">Where can I find a sample .NET client application for performance testing of Cosmos DB?</span></span> 
* <span data-ttu-id="fc134-112">Hur uppnå hög genomströmning nivåer med Cosmos DB från min klientprogrammet?</span><span class="sxs-lookup"><span data-stu-id="fc134-112">How do I achieve high throughput levels with Cosmos DB from my client application?</span></span>

<span data-ttu-id="fc134-113">Om du vill komma igång med koden hämta projektet från [Azure Cosmos DB prestanda testning Sample](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/documentdb-benchmark).</span><span class="sxs-lookup"><span data-stu-id="fc134-113">To get started with code, please download the project from [Azure Cosmos DB Performance Testing Sample](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/documentdb-benchmark).</span></span> 

> [!NOTE]
> <span data-ttu-id="fc134-114">Målet med det här programmet är att visa Metodtips för att extrahera bättre prestanda från Cosmos-databas med ett litet antal klientdatorer.</span><span class="sxs-lookup"><span data-stu-id="fc134-114">The goal of this application is to demonstrate best practices for extracting better performance out of Cosmos DB with a small number of client machines.</span></span> <span data-ttu-id="fc134-115">Detta gjordes inte att visa högsta kapacitet för tjänsten, som kan skalas utan begränsningar.</span><span class="sxs-lookup"><span data-stu-id="fc134-115">This was not made to demonstrate the peak capacity of the service, which can scale limitlessly.</span></span>
> 
> 

<span data-ttu-id="fc134-116">Om du letar efter klientsidan konfigurationsalternativ för att förbättra prestanda för Cosmos DB [Azure Cosmos DB prestandatips](performance-tips.md).</span><span class="sxs-lookup"><span data-stu-id="fc134-116">If you're looking for client-side configuration options to improve Cosmos DB performance, see [Azure Cosmos DB performance tips](performance-tips.md).</span></span>

## <a name="run-the-performance-testing-application"></a><span data-ttu-id="fc134-117">Kör programmet för prestandatestning</span><span class="sxs-lookup"><span data-stu-id="fc134-117">Run the performance testing application</span></span>
<span data-ttu-id="fc134-118">Det snabbaste sättet att komma igång är att kompilera och köra .NET-exemplet nedan, som beskrivs i stegen nedan.</span><span class="sxs-lookup"><span data-stu-id="fc134-118">The quickest way to get started is to compile and run the .NET sample below, as described in the steps below.</span></span> <span data-ttu-id="fc134-119">Du kan också granska källkoden och implementera liknande konfigurationer för egna klientprogram.</span><span class="sxs-lookup"><span data-stu-id="fc134-119">You can also review the source code and implement similar configurations to your own client applications.</span></span>

<span data-ttu-id="fc134-120">**Steg 1:** hämta projektet från [Azure Cosmos DB prestanda testning Sample](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/documentdb-benchmark), eller duplicera GitHub-lagringsplatsen.</span><span class="sxs-lookup"><span data-stu-id="fc134-120">**Step 1:** Download the project from [Azure Cosmos DB Performance Testing Sample](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/documentdb-benchmark), or fork the GitHub repository.</span></span>

<span data-ttu-id="fc134-121">**Steg 2:** ändra inställningarna för EndpointUrl, AuthorizationKey, CollectionThroughput och DocumentTemplate (valfritt) i App.config.</span><span class="sxs-lookup"><span data-stu-id="fc134-121">**Step 2:** Modify the settings for EndpointUrl, AuthorizationKey, CollectionThroughput and DocumentTemplate (optional) in App.config.</span></span>

> [!NOTE]
> <span data-ttu-id="fc134-122">Innan du etablerar samlingar med hög genomströmning, referera till den [prissättning](https://azure.microsoft.com/pricing/details/cosmos-db/) att uppskatta kostnader per samling.</span><span class="sxs-lookup"><span data-stu-id="fc134-122">Before provisioning collections with high throughput, please refer to the [Pricing Page](https://azure.microsoft.com/pricing/details/cosmos-db/) to estimate the costs per collection.</span></span> <span data-ttu-id="fc134-123">Azure Cosmos databaslagring fakturor och genomströmning oberoende på timbasis, så kan du spara kostnader genom att radera eller sänker genomflödet i Azure DB som Cosmos-samlingar efter testning.</span><span class="sxs-lookup"><span data-stu-id="fc134-123">Azure Cosmos DB bills storage and throughput independently on an hourly basis, so you can save costs by deleting or lowering the throughput of your Azure Cosmos DB collections after testing.</span></span>
> 
> 

<span data-ttu-id="fc134-124">**Steg 3:** kompilera och köra konsolapp-från kommandoraden.</span><span class="sxs-lookup"><span data-stu-id="fc134-124">**Step 3:** Compile and run the console app from the command line.</span></span> <span data-ttu-id="fc134-125">Du bör se utdata som liknar följande:</span><span class="sxs-lookup"><span data-stu-id="fc134-125">You should see output like the following:</span></span>

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


<span data-ttu-id="fc134-126">**Steg 4 (vid behov):** genomflödet rapporterade (RU/s) från verktyget ska vara samma eller högre än det tillhandahållna dataflödet i mängden.</span><span class="sxs-lookup"><span data-stu-id="fc134-126">**Step 4 (if necessary):** The throughput reported (RU/s) from the tool should be the same or higher than the provisioned throughput of the collection.</span></span> <span data-ttu-id="fc134-127">Om inte, öka DegreeOfParallelism i små steg kan hjälpa dig att nå gränsen.</span><span class="sxs-lookup"><span data-stu-id="fc134-127">If not, increasing the DegreeOfParallelism in small increments may help you reach the limit.</span></span> <span data-ttu-id="fc134-128">Om genomströmning från din klient högplaåter hjälper starta flera instanser av appen på samma eller olika datorer dig att nå etablerade gränsen mellan olika instanser.</span><span class="sxs-lookup"><span data-stu-id="fc134-128">If the throughput from your client app plateaus, launching multiple instances of the app on the same or different machines will help you reach the provisioned limit across the different instances.</span></span> <span data-ttu-id="fc134-129">Om du behöver hjälp med det här steget, Skriv ett e-postmeddelande till askcosmosdb@microsoft.com eller filen ett supportärende från den [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="fc134-129">If you need help with this step, please, write an email to askcosmosdb@microsoft.com or file a support ticket from the [Azure Portal](https://portal.azure.com).</span></span>

<span data-ttu-id="fc134-130">När du har appen körs försöker du att olika [indexering principer](indexing-policies.md) och [konsekvensnivåer](consistency-levels.md) att förstå deras inverkan på genomflöde och svarstid.</span><span class="sxs-lookup"><span data-stu-id="fc134-130">Once you have the app running, you can try different [Indexing policies](indexing-policies.md) and [Consistency levels](consistency-levels.md) to understand their impact on throughput and latency.</span></span> <span data-ttu-id="fc134-131">Du kan också granska källkoden och implementera liknande konfigurationer till testpaket eller program i produktion.</span><span class="sxs-lookup"><span data-stu-id="fc134-131">You can also review the source code and implement similar configurations to your own test suites or production applications.</span></span>

## <a name="next-steps"></a><span data-ttu-id="fc134-132">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="fc134-132">Next steps</span></span>
<span data-ttu-id="fc134-133">I den här artikeln vi har tittat på hur du kan utföra prestanda och skalning testning med Cosmos-databasen med en .NET-konsolapp.</span><span class="sxs-lookup"><span data-stu-id="fc134-133">In this article, we looked at how you can perform performance and scale testing with Cosmos DB using a .NET console app.</span></span> <span data-ttu-id="fc134-134">Se länkarna nedan för mer information om hur du arbetar med Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="fc134-134">Please refer to the links below for additional information on working with Azure Cosmos DB.</span></span>

* [<span data-ttu-id="fc134-135">Azure DB Cosmos prestandatestning exempel</span><span class="sxs-lookup"><span data-stu-id="fc134-135">Azure Cosmos DB performance testing sample</span></span>](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/documentdb-benchmark)
* [<span data-ttu-id="fc134-136">Klienten konfigurationsalternativ för att förbättra prestanda för Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="fc134-136">Client configuration options to improve Azure Cosmos DB performance</span></span>](performance-tips.md)
* [<span data-ttu-id="fc134-137">Serversidan partitionering i Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="fc134-137">Server-side partitioning in Azure Cosmos DB</span></span>](partition-data.md)


