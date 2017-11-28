---
title: "aaaWorking med hello ändringen feeds stöd i Azure Cosmos DB | Microsoft Docs"
description: "Använd Azure Cosmos DB ändra feed stöd tootrack ändringar i dokument och utföra händelsebaserat bearbetning som utlösare och uppdatera cacheminnen och analyser system kontinuerligt."
keywords: "Ändra feed"
services: cosmos-db
author: arramac
manager: jhubbard
editor: mimig
documentationcenter: 
ms.assetid: 2d7798db-857f-431a-b10f-3ccbc7d93b50
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: rest-api
ms.topic: article
ms.date: 08/15/2017
ms.author: arramac
ms.openlocfilehash: a4dcf4ceb476e3e08266dbcdcbee1d75e1d1eed4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="working-with-hello-change-feed-support-in-azure-cosmos-db"></a><span data-ttu-id="1025c-104">Arbeta med hello ändra feed stöd i Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="1025c-104">Working with hello change feed support in Azure Cosmos DB</span></span>
<span data-ttu-id="1025c-105">[Azure Cosmos-DB](../cosmos-db/introduction.md) är ett snabbt och flexibel globalt replikeras databastjänsten som används för att lagra stora volymer transaktionella och operativa data med förutsägbar en siffra millisekunders latens för läsningar och skrivningar.</span><span class="sxs-lookup"><span data-stu-id="1025c-105">[Azure Cosmos DB](../cosmos-db/introduction.md) is a fast and flexible globally replicated database service that is used for storing high-volume transactional and operational data with predictable single-digit millisecond latency for reads and writes.</span></span> <span data-ttu-id="1025c-106">Detta gör det väl lämpade för IoT, återförsäljnings, spel och operativa loggningsprogram.</span><span class="sxs-lookup"><span data-stu-id="1025c-106">This makes it well-suited for IoT, gaming, retail, and operational logging applications.</span></span> <span data-ttu-id="1025c-107">En gemensam designmönstret i dessa program är tootrack ändringar gjorts tooAzure Cosmos DB data och uppdatera materialiserade vyer, genomföra analys i realtid, arkivera toocold datalagring och utlösare meddelanden på vissa händelser baserat på dessa ändringar.</span><span class="sxs-lookup"><span data-stu-id="1025c-107">A common design pattern in these applications is tootrack changes made tooAzure Cosmos DB data, and update materialized views, perform real-time analytics, archive data toocold storage, and trigger notifications on certain events based on these changes.</span></span> <span data-ttu-id="1025c-108">Hej **ändra feed stöd** i Azure Cosmos-databasen kan du toobuild skalbara lösningar för var och en av dessa mönster.</span><span class="sxs-lookup"><span data-stu-id="1025c-108">hello **change feed support** in Azure Cosmos DB enables you toobuild efficient and scalable solutions for each of these patterns.</span></span>

<span data-ttu-id="1025c-109">Med ändringen feed support, tillhandahåller Azure Cosmos DB en sorterad lista över dokument inom en samling Azure Cosmos DB i hello ordning de ändrades.</span><span class="sxs-lookup"><span data-stu-id="1025c-109">With change feed support, Azure Cosmos DB provides a sorted list of documents within an Azure Cosmos DB collection in hello order in which they were modified.</span></span> <span data-ttu-id="1025c-110">Denna feed kan använda toolisten för ändringar toodata inom hello samlingen och utföra åtgärder som:</span><span class="sxs-lookup"><span data-stu-id="1025c-110">This feed can be used toolisten for modifications toodata within hello collection and perform actions such as:</span></span>

* <span data-ttu-id="1025c-111">Utlös ett anrop tooan API när ett dokument infogas eller ändras</span><span class="sxs-lookup"><span data-stu-id="1025c-111">Trigger a call tooan API when a document is inserted or modified</span></span>
* <span data-ttu-id="1025c-112">Utföra realtid (ström) bearbetning av uppdateringar</span><span class="sxs-lookup"><span data-stu-id="1025c-112">Perform real-time (stream) processing on updates</span></span>
* <span data-ttu-id="1025c-113">Synkronisera data med en cache, sökmotor eller datalagret</span><span class="sxs-lookup"><span data-stu-id="1025c-113">Synchronize data with a cache, search engine, or data warehouse</span></span>

<span data-ttu-id="1025c-114">Ändringar i Azure Cosmos DB kan sparas och bearbetas asynkront och distribuerade i en eller flera konsumenter för parallell bearbetning.</span><span class="sxs-lookup"><span data-stu-id="1025c-114">Changes in Azure Cosmos DB are persisted and can be processed asynchronously, and distributed across one or more consumers for parallel processing.</span></span> <span data-ttu-id="1025c-115">Nu ska vi titta på hello API: er för att ändra feed och hur du kan använda dem toobuild skalbara realtidsprogram.</span><span class="sxs-lookup"><span data-stu-id="1025c-115">Let's look at hello APIs for change feed and how you can use them toobuild scalable real-time applications.</span></span> <span data-ttu-id="1025c-116">Den här artikeln visar hur toowork med Azure Cosmos DB ändra feed och hello DocumentDB-API.</span><span class="sxs-lookup"><span data-stu-id="1025c-116">This article shows how toowork with Azure Cosmos DB change feed and hello DocumentDB API.</span></span> 

![Med hjälp av Azure Cosmos DB ändra feed toopower analys i realtid och händelsedriven datascenarier](./media/change-feed/changefeedoverview.png)

> [!NOTE]
> <span data-ttu-id="1025c-118">Ändra feed support tillhandahålls endast för hello DocumentDB API vid detta tillfälle. hello Graph API och tabellen API stöds inte för närvarande.</span><span class="sxs-lookup"><span data-stu-id="1025c-118">Change feed support is only provided for hello DocumentDB API at this time; hello Graph API and Table API are not currently supported.</span></span>

## <a name="use-cases-and-scenarios"></a><span data-ttu-id="1025c-119">Användningsfall och scenarier</span><span class="sxs-lookup"><span data-stu-id="1025c-119">Use cases and scenarios</span></span>
<span data-ttu-id="1025c-120">Ändra feed möjliggör effektiv bearbetning av stora datamängder med en stor volym med skrivningar och erbjuder ett alternativt tooquerying hela datauppsättningar tooidentify vad som har ändrats.</span><span class="sxs-lookup"><span data-stu-id="1025c-120">Change feed allows for efficient processing of large datasets with a high volume of writes, and offers an alternative tooquerying entire datasets tooidentify what has changed.</span></span> <span data-ttu-id="1025c-121">Exempelvis kan du utföra följande uppgifter effektivt hello:</span><span class="sxs-lookup"><span data-stu-id="1025c-121">For example, you can perform hello following tasks efficiently:</span></span>

* <span data-ttu-id="1025c-122">Uppdatera ett cacheminne, sökindex eller ett datalager med data som lagras i Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="1025c-122">Update a cache, search index, or a data warehouse with data stored in Azure Cosmos DB.</span></span>
* <span data-ttu-id="1025c-123">Implementera programnivå data lagringsnivåer och arkivering, som är ”varm data” lagras i Azure Cosmos DB och föråldrat ”kalldata” för[Azure Blob Storage](../storage/common/storage-introduction.md) eller [Azure Data Lake Store](../data-lake-store/data-lake-store-overview.md).</span><span class="sxs-lookup"><span data-stu-id="1025c-123">Implement application-level data tiering and archival, that is, store "hot data" in Azure Cosmos DB, and age out "cold data" too[Azure Blob Storage](../storage/common/storage-introduction.md) or [Azure Data Lake Store](../data-lake-store/data-lake-store-overview.md).</span></span>
* <span data-ttu-id="1025c-124">Implementera batchanalyser på data med hjälp av [Apache Hadoop](run-hadoop-with-hdinsight.md).</span><span class="sxs-lookup"><span data-stu-id="1025c-124">Implement batch analytics on data using [Apache Hadoop](run-hadoop-with-hdinsight.md).</span></span>
* <span data-ttu-id="1025c-125">Implementera [lambda pipelines på Azure](https://blogs.technet.microsoft.com/msuspartner/2016/01/27/azure-partner-community-big-data-advanced-analytics-and-lambda-architecture/) med Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="1025c-125">Implement [lambda pipelines on Azure](https://blogs.technet.microsoft.com/msuspartner/2016/01/27/azure-partner-community-big-data-advanced-analytics-and-lambda-architecture/) with Azure Cosmos DB.</span></span> <span data-ttu-id="1025c-126">Azure Cosmos-DB tillhandahåller en skalbar databaslösning som kan hantera både införandet och fråga och implementera lambda arkitekturer med låg TCO.</span><span class="sxs-lookup"><span data-stu-id="1025c-126">Azure Cosmos DB provides a scalable database solution that can handle both ingestion and query, and implement lambda architectures with low TCO.</span></span> 
* <span data-ttu-id="1025c-127">Utföra noll driftstopp migreringar tooanother Azure DB som Cosmos-konto med en annan partitioneringsschema.</span><span class="sxs-lookup"><span data-stu-id="1025c-127">Perform zero down-time migrations tooanother Azure Cosmos DB account with a different partitioning scheme.</span></span>

<span data-ttu-id="1025c-128">**Lambda Pipelines med Azure Cosmos DB för införandet och fråga:**</span><span class="sxs-lookup"><span data-stu-id="1025c-128">**Lambda Pipelines with Azure Cosmos DB for ingestion and query:**</span></span>

![Azure Cosmos-DB baserat lambda pipeline för införandet och fråga](./media/change-feed/lambda.png)

<span data-ttu-id="1025c-130">Du kan använda Azure Cosmos DB tooreceive och lagra händelsedata från enheter, sensorer, infrastruktur och program och bearbeta händelserna i realtid med [Azure Stream Analytics](../stream-analytics/stream-analytics-documentdb-output.md), [Apache Storm](../hdinsight/hdinsight-storm-overview.md), eller [Apache Spark](../hdinsight/hdinsight-apache-spark-overview.md).</span><span class="sxs-lookup"><span data-stu-id="1025c-130">You can use Azure Cosmos DB tooreceive and store event data from devices, sensors, infrastructure, and applications, and process these events in real-time with [Azure Stream Analytics](../stream-analytics/stream-analytics-documentdb-output.md), [Apache Storm](../hdinsight/hdinsight-storm-overview.md), or [Apache Spark](../hdinsight/hdinsight-apache-spark-overview.md).</span></span> 

<span data-ttu-id="1025c-131">Inom din [serverlösa](http://azure.com/serverless) webb- och mobilappar, kan du spåra händelser som ändringar tooyour kundens profil, inställningar eller plats tootrigger vissa åtgärder som att skicka push-meddelanden tootheir enheter med hjälp av [Azure Functions](../azure-functions/functions-bindings-documentdb.md) eller [Apptjänster](https://azure.microsoft.com/services/app-service/).</span><span class="sxs-lookup"><span data-stu-id="1025c-131">Within your [serverless](http://azure.com/serverless) web and mobile apps, you can track events such as changes tooyour customer's profile, preferences, or location tootrigger certain actions like sending push notifications tootheir devices using [Azure Functions](../azure-functions/functions-bindings-documentdb.md) or [App Services](https://azure.microsoft.com/services/app-service/).</span></span> <span data-ttu-id="1025c-132">Om du använder Azure Cosmos DB toobuild spel kan du till exempel använda ändra feed tooimplement realtid ledartavlor baserat på resultat från slutförda spel.</span><span class="sxs-lookup"><span data-stu-id="1025c-132">If you're using Azure Cosmos DB toobuild a game, you can, for example, use change feed tooimplement real-time leaderboards based on scores from completed games.</span></span>

## <a name="how-change-feed-works-in-azure-cosmos-db"></a><span data-ttu-id="1025c-133">Hur ändringen feed fungerar i Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="1025c-133">How change feed works in Azure Cosmos DB</span></span>
<span data-ttu-id="1025c-134">Azure Cosmos-DB tillhandahåller hello möjlighet tooincrementally läsa uppdateringar som görs tooan Azure DB som Cosmos-samling.</span><span class="sxs-lookup"><span data-stu-id="1025c-134">Azure Cosmos DB provides hello ability tooincrementally read updates made tooan Azure Cosmos DB collection.</span></span> <span data-ttu-id="1025c-135">Ändra feeden har hello följande egenskaper:</span><span class="sxs-lookup"><span data-stu-id="1025c-135">This change feed has hello following properties:</span></span>

* <span data-ttu-id="1025c-136">Ändringar är beständiga i Azure Cosmos-databasen och går att bearbeta asynkront.</span><span class="sxs-lookup"><span data-stu-id="1025c-136">Changes are persistent in Azure Cosmos DB and can be processed asynchronously.</span></span>
* <span data-ttu-id="1025c-137">Toodocuments ändringar i en samling är tillgängliga omedelbart i hello ändra feed.</span><span class="sxs-lookup"><span data-stu-id="1025c-137">Changes toodocuments within a collection are available immediately in hello change feed.</span></span>
* <span data-ttu-id="1025c-138">Varje ändring tooa dokumentet visas exakt en gång i hello ändra feed och hantera sina kontrollpunkter logik för klienter.</span><span class="sxs-lookup"><span data-stu-id="1025c-138">Each change tooa document appears exactly once in hello change feed, and clients manage their checkpointing logic.</span></span> <span data-ttu-id="1025c-139">hello ändra feed processor biblioteket innehåller automatiska kontrollpunkter och ”minst en gång” semantik.</span><span class="sxs-lookup"><span data-stu-id="1025c-139">hello change feed processor library provides automatic checkpointing and "at least once" semantics.</span></span>
* <span data-ttu-id="1025c-140">Endast hello senaste ändringen för ett visst dokument ingår i hello ändringsloggen.</span><span class="sxs-lookup"><span data-stu-id="1025c-140">Only hello most recent change for a given document is included in hello change log.</span></span> <span data-ttu-id="1025c-141">Mellanliggande ändringar kan inte användas.</span><span class="sxs-lookup"><span data-stu-id="1025c-141">Intermediate changes may not be available.</span></span>
* <span data-ttu-id="1025c-142">hello ändra feed sorteras ordning ändras inom varje partitionsnyckelvärde.</span><span class="sxs-lookup"><span data-stu-id="1025c-142">hello change feed is sorted by order of modification within each partition key value.</span></span> <span data-ttu-id="1025c-143">Det finns ingen garanterad ordning över partitionsnyckel värden.</span><span class="sxs-lookup"><span data-stu-id="1025c-143">There is no guaranteed order across partition-key values.</span></span>
* <span data-ttu-id="1025c-144">Ändringar som kan synkroniseras från alla i tidpunkt, det är ingen fast Datalagringsperiod ändringar är tillgängliga.</span><span class="sxs-lookup"><span data-stu-id="1025c-144">Changes can be synchronized from any point-in-time, that is, there is no fixed data retention period for which changes are available.</span></span>
* <span data-ttu-id="1025c-145">Ändringar är tillgängliga i mängder partition nyckelintervall.</span><span class="sxs-lookup"><span data-stu-id="1025c-145">Changes are available in chunks of partition key ranges.</span></span> <span data-ttu-id="1025c-146">Den här funktionen kan ändringar från stora samlingar toobe bearbetas parallellt av flera konsumenter-servrar.</span><span class="sxs-lookup"><span data-stu-id="1025c-146">This capability allows changes from large collections toobe processed in parallel by multiple consumers/servers.</span></span>
* <span data-ttu-id="1025c-147">Program kan begära för ändring av flera flöden samtidigt på hello samma samling.</span><span class="sxs-lookup"><span data-stu-id="1025c-147">Applications can request for multiple change feeds simultaneously on hello same collection.</span></span>

<span data-ttu-id="1025c-148">Azure Cosmos DB ändra feeden är aktiverad som standard för alla konton.</span><span class="sxs-lookup"><span data-stu-id="1025c-148">Azure Cosmos DB's change feed is enabled by default for all accounts.</span></span> <span data-ttu-id="1025c-149">Du kan använda din [etablerat dataflöde](request-units.md) i din region skrivning eller någon [läsa region](distribute-data-globally.md) tooread från hello ändra feed, precis som andra igen från Azure Cosmos-databasen.</span><span class="sxs-lookup"><span data-stu-id="1025c-149">You can use your [provisioned throughput](request-units.md) in your write region or any [read region](distribute-data-globally.md) tooread from hello change feed, just like any other operation from Azure Cosmos DB.</span></span> <span data-ttu-id="1025c-150">hello ändra feed innehåller infogningar och uppdateringsåtgärder gjorts toodocuments inom hello samlingen.</span><span class="sxs-lookup"><span data-stu-id="1025c-150">hello change feed includes inserts and update operations made toodocuments within hello collection.</span></span> <span data-ttu-id="1025c-151">Du kan avbilda borttagningar genom att ange en ”soft-ta bort” flagga i dokument i stället för borttagningar.</span><span class="sxs-lookup"><span data-stu-id="1025c-151">You can capture deletes by setting a "soft-delete" flag within your documents in place of deletes.</span></span> <span data-ttu-id="1025c-152">Alternativt kan du ställa en begränsad utgångsperioden för dina dokument via hello [TTL kapaciteten](time-to-live.md)för exempelvis 24 timmar och Använd hello värdet för egenskapen toocapture tas bort.</span><span class="sxs-lookup"><span data-stu-id="1025c-152">Alternatively, you can set a finite expiration period for your documents via hello [TTL capability](time-to-live.md), for example, 24 hours and use hello value of that property toocapture deletes.</span></span> <span data-ttu-id="1025c-153">Med den här lösningen har ändrats och tooprocess inom ett kortare tidsintervall än hello TTL förfallotid perioden.</span><span class="sxs-lookup"><span data-stu-id="1025c-153">With this solution, you have tooprocess changes within a shorter time interval than hello TTL expiration period.</span></span> <span data-ttu-id="1025c-154">hello ändra feed är tillgänglig för varje partitionsnyckelintervallet inom hello dokumentsamlingen och därför kan fördelas på en eller flera konsumenter för parallell bearbetning.</span><span class="sxs-lookup"><span data-stu-id="1025c-154">hello change feed is available for each partition key range within hello document collection, and thus can be distributed across one or more consumers for parallel processing.</span></span> 

![Distribuerad bearbetning av Azure Cosmos DB ändra feed](./media/change-feed/changefeedvisual.png)

<span data-ttu-id="1025c-156">Du har några alternativ i hur du implementerar en ändring feeds i din klientkod.</span><span class="sxs-lookup"><span data-stu-id="1025c-156">You have a few options in how you implement a change feed in your client code.</span></span> <span data-ttu-id="1025c-157">hello avsnitten omedelbart Följ beskriva hur tooimplement hello ändra feed med hello Azure Cosmos DB REST-API och hello DocumentDB-SDK.</span><span class="sxs-lookup"><span data-stu-id="1025c-157">hello sections that immediately follow describe how tooimplement hello change feed using hello Azure Cosmos DB REST API and hello DocumentDB SDKs.</span></span> <span data-ttu-id="1025c-158">För .NET-program, rekommenderar vi dock använda hello nya [ändra feed processor biblioteket](#change-feed-processor) för att bearbeta händelser från hello ändra feed som förenklar läsning ändringar mellan partitioner och gör att flera trådar som arbetar i parallellt.</span><span class="sxs-lookup"><span data-stu-id="1025c-158">However, for .NET applications, we recommend using hello new [Change feed processor library](#change-feed-processor) for processing events from hello change feed as it simplifies reading changes across partitions and enables multiple threads working in parallel.</span></span> 

## <span data-ttu-id="1025c-159"><a id="rest-apis"></a>Arbeta med hello REST-API och DocumentDB-SDK</span><span class="sxs-lookup"><span data-stu-id="1025c-159"><a id="rest-apis"></a>Working with hello REST API and DocumentDB SDKs</span></span>
<span data-ttu-id="1025c-160">Azure Cosmos-DB tillhandahåller elastiska behållare för lagring och genomströmning kallas **samlingar**.</span><span class="sxs-lookup"><span data-stu-id="1025c-160">Azure Cosmos DB provides elastic containers of storage and throughput called **collections**.</span></span> <span data-ttu-id="1025c-161">Data i samlingar är logiskt grupperade med [partitions nycklar](partition-data.md) för skalbarhet och prestanda.</span><span class="sxs-lookup"><span data-stu-id="1025c-161">Data within collections is logically grouped using [partition keys](partition-data.md) for scalability and performance.</span></span> <span data-ttu-id="1025c-162">Azure Cosmos-DB tillhandahåller olika API: er för att komma åt dessa data, inklusive sökning med ID (Läs/Get), fråga och Läs-feeds (sökningar).</span><span class="sxs-lookup"><span data-stu-id="1025c-162">Azure Cosmos DB provides various APIs for accessing this data, including lookup by ID (Read/Get), query, and read-feeds (scans).</span></span> <span data-ttu-id="1025c-163">hello ändra feeden kan erhållas genom att fylla i två nya begäran huvuden toohello DocumentDB `ReadDocumentFeed` API, och kan bearbetas parallellt över intervallen för partitionsnycklar.</span><span class="sxs-lookup"><span data-stu-id="1025c-163">hello change feed can be obtained by populating two new request headers toohello DocumentDB `ReadDocumentFeed` API, and can be processed in parallel across ranges of partition keys.</span></span>

### <a name="readdocumentfeed-api"></a><span data-ttu-id="1025c-164">ReadDocumentFeed API</span><span class="sxs-lookup"><span data-stu-id="1025c-164">ReadDocumentFeed API</span></span>
<span data-ttu-id="1025c-165">Nu ska vi titta kort hur ReadDocumentFeed fungerar.</span><span class="sxs-lookup"><span data-stu-id="1025c-165">Let's take a brief look at how ReadDocumentFeed works.</span></span> <span data-ttu-id="1025c-166">Azure Cosmos-DB stöder läsning av en feed av dokument i en samling via hello `ReadDocumentFeed` API.</span><span class="sxs-lookup"><span data-stu-id="1025c-166">Azure Cosmos DB supports reading a feed of documents within a collection via hello `ReadDocumentFeed` API.</span></span> <span data-ttu-id="1025c-167">Till exempel hello följande begäran returnerar en sida av dokument i hello `serverlogs` samling.</span><span class="sxs-lookup"><span data-stu-id="1025c-167">For example, hello following request returns a page of documents inside hello `serverlogs` collection.</span></span> 

    GET https://mydocumentdb.documents.azure.com/dbs/smalldb/colls/serverlogs HTTP/1.1
    x-ms-date: Tue, 22 Nov 2016 17:05:14 GMT
    authorization: type%3dmaster%26ver%3d1.0%26sig%3dgo7JEogZDn6ritWhwc5hX%2fNTV4wwM1u9V2Is1H4%2bDRg%3d
    Cache-Control: no-cache
    x-ms-consistency-level: Strong
    User-Agent: Microsoft.Azure.Documents.Client/1.10.27.5
    x-ms-version: 2016-07-11
    Accept: application/json
    Host: mydocumentdb.documents.azure.com

<span data-ttu-id="1025c-168">Resultat kan begränsas med hjälp av hello `x-ms-max-item-count` sidhuvud och läser återupptas med skickar på nytt hello begäran med en `x-ms-continuation` huvud returneras i föregående hello-svar.</span><span class="sxs-lookup"><span data-stu-id="1025c-168">Results can be limited by using hello `x-ms-max-item-count` header, and reads can be resumed by resubmitting hello request with a `x-ms-continuation` header returned in hello previous response.</span></span> <span data-ttu-id="1025c-169">Om den utförs från en enskild klient `ReadDocumentFeed` går igenom resultaten seriellt över partitioner.</span><span class="sxs-lookup"><span data-stu-id="1025c-169">When performed from a single client, `ReadDocumentFeed` iterates through results across partitions serially.</span></span> 

<span data-ttu-id="1025c-170">**Seriell läsa dokumentet feed**</span><span class="sxs-lookup"><span data-stu-id="1025c-170">**Serial read document feed**</span></span>

<span data-ttu-id="1025c-171">Du kan också hämta hello feed dokument genom att använda en av hello stöds [Azure Cosmos DB SDK](documentdb-sdk-dotnet.md).</span><span class="sxs-lookup"><span data-stu-id="1025c-171">You can also retrieve hello feed of documents using one of hello supported [Azure Cosmos DB SDKs](documentdb-sdk-dotnet.md).</span></span> <span data-ttu-id="1025c-172">Till exempel hello följande fragment visas hur toouse hello [ReadDocumentFeedAsync metoden](/dotnet/api/microsoft.azure.documents.client.documentclient.readdocumentfeedasync?view=azure-dotnet) i .NET.</span><span class="sxs-lookup"><span data-stu-id="1025c-172">For example, hello following snippet shows how toouse hello [ReadDocumentFeedAsync method](/dotnet/api/microsoft.azure.documents.client.documentclient.readdocumentfeedasync?view=azure-dotnet) in .NET.</span></span>

```csharp
FeedResponse<dynamic> feedResponse = null;
do
{
    feedResponse = await client.ReadDocumentFeedAsync(collection, new FeedOptions { MaxItemCount = -1 });
}
while (feedResponse.ResponseContinuation != null);
```

### <a name="distributed-execution-of-readdocumentfeed"></a><span data-ttu-id="1025c-173">Distribuerade körning av ReadDocumentFeed</span><span class="sxs-lookup"><span data-stu-id="1025c-173">Distributed execution of ReadDocumentFeed</span></span>
<span data-ttu-id="1025c-174">Seriell körningen av skrivskyddade feed från en enskild klientdator kanske inte praktiska för samlingar som innehåller data eller flera terabyte eller mata in ett stort antal uppdateringar.</span><span class="sxs-lookup"><span data-stu-id="1025c-174">For collections that contain terabytes of data or more, or ingest a large volume of updates, serial execution of read feed from a single client machine might not be practical.</span></span> <span data-ttu-id="1025c-175">I ordning toosupport scenarierna stordata Azure Cosmos DB innehåller API: er toodistribute `ReadDocumentFeed` anrop transparent över flera klienten läsare konsumenter.</span><span class="sxs-lookup"><span data-stu-id="1025c-175">In order toosupport these big data scenarios, Azure Cosmos DB provides APIs toodistribute `ReadDocumentFeed` calls transparently across multiple client readers/consumers.</span></span> 

<span data-ttu-id="1025c-176">**Distribuerade läsa dokumentet Feed**</span><span class="sxs-lookup"><span data-stu-id="1025c-176">**Distributed Read Document Feed**</span></span>

<span data-ttu-id="1025c-177">tooprovide skalbara bearbetning av inkrementella ändringar, Azure Cosmos DB stöder en skalbar modell för hello ändra feed API baserat på intervallen för partitionsnycklar.</span><span class="sxs-lookup"><span data-stu-id="1025c-177">tooprovide scalable processing of incremental changes, Azure Cosmos DB supports a scale-out model for hello change feed API based on ranges of partition keys.</span></span>

* <span data-ttu-id="1025c-178">Du kan hämta en lista över partition nyckelintervall för en samling som utför en `ReadPartitionKeyRanges` anropa.</span><span class="sxs-lookup"><span data-stu-id="1025c-178">You can obtain a list of partition key ranges for a collection performing a `ReadPartitionKeyRanges` call.</span></span> 
* <span data-ttu-id="1025c-179">För varje partitionsnyckelintervallet kan du utföra en `ReadDocumentFeed` tooread dokument med partitionsnycklar inom det intervallet.</span><span class="sxs-lookup"><span data-stu-id="1025c-179">For each partition key range, you can perform a `ReadDocumentFeed` tooread documents with partition keys within that range.</span></span>

### <a name="retrieving-partition-key-ranges-for-a-collection"></a><span data-ttu-id="1025c-180">Hämtar partition nyckelintervall för en samling</span><span class="sxs-lookup"><span data-stu-id="1025c-180">Retrieving partition key ranges for a collection</span></span>
<span data-ttu-id="1025c-181">Du kan hämta hello partition nyckelintervall med begärande hello `pkranges` resurs i en samling.</span><span class="sxs-lookup"><span data-stu-id="1025c-181">You can retrieve hello partition key ranges by requesting hello `pkranges` resource within a collection.</span></span> <span data-ttu-id="1025c-182">Till exempel hello följande begäran hämtar hello lista över partition nyckelintervall för hello `serverlogs` samling:</span><span class="sxs-lookup"><span data-stu-id="1025c-182">For example hello following request retrieves hello list of partition key ranges for hello `serverlogs` collection:</span></span>

    GET https://querydemo.documents.azure.com/dbs/bigdb/colls/serverlogs/pkranges HTTP/1.1
    x-ms-date: Tue, 15 Nov 2016 07:26:51 GMT
    authorization: type%3dmaster%26ver%3d1.0%26sig%3dEConYmRgDExu6q%2bZ8GjfUGOH0AcOx%2behkancw3LsGQ8%3d
    x-ms-consistency-level: Session
    x-ms-version: 2016-07-11
    Accept: application/json
    Host: querydemo.documents.azure.com

<span data-ttu-id="1025c-183">Denna begäran returnerar hello efter svar som innehåller metadata om hello partition viktiga områden:</span><span class="sxs-lookup"><span data-stu-id="1025c-183">This request returns hello following response containing metadata about hello partition key ranges:</span></span>

    HTTP/1.1 200 Ok
    Content-Type: application/json
    x-ms-item-count: 25
    x-ms-schemaversion: 1.1
    Date: Tue, 15 Nov 2016 07:26:51 GMT

    {
       "_rid":"qYcAAPEvJBQ=",
       "PartitionKeyRanges":[
          {
             "_rid":"qYcAAPEvJBQCAAAAAAAAUA==",
             "id":"0",
             "_etag":"\"00002800-0000-0000-0000-580ac4ea0000\"",
             "minInclusive":"",
             "maxExclusive":"05C1CFFFFFFFF8",
             "_self":"dbs\/qYcAAA==\/colls\/qYcAAPEvJBQ=\/pkranges\/qYcAAPEvJBQCAAAAAAAAUA==\/",
             "_ts":1477100776
          },
          ...
       ],
       "_count": 25
    }


<span data-ttu-id="1025c-184">**Partitionera viktiga intervallet egenskaper**: varje partitionsnyckelintervallet innehåller hello metadataegenskaper i den följande tabellen hello:</span><span class="sxs-lookup"><span data-stu-id="1025c-184">**Partition key range properties**: Each partition key range includes hello metadata properties in hello following table:</span></span>

<table>
    <tr>
        <th><span data-ttu-id="1025c-185">Huvudets namn</span><span class="sxs-lookup"><span data-stu-id="1025c-185">Header name</span></span></th>
        <th><span data-ttu-id="1025c-186">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="1025c-186">Description</span></span></th>
    </tr>
    <tr>
        <td><span data-ttu-id="1025c-187">id</span><span class="sxs-lookup"><span data-stu-id="1025c-187">id</span></span></td>
        <td>
            <p><span data-ttu-id="1025c-188">hello-ID för hello partitionsnyckelintervallet.</span><span class="sxs-lookup"><span data-stu-id="1025c-188">hello ID for hello partition key range.</span></span> <span data-ttu-id="1025c-189">Det här är ett stabilt och unikt ID i varje samling.</span><span class="sxs-lookup"><span data-stu-id="1025c-189">This is a stable and unique ID within each collection.</span></span></p>
            <p><span data-ttu-id="1025c-190">Måste användas i hello följande anrop tooread ändringar av partitionsnyckelintervallet.</span><span class="sxs-lookup"><span data-stu-id="1025c-190">Must be used in hello following call tooread changes by partition key range.</span></span></p>
        </td>
    </tr>
    <tr>
        <td><span data-ttu-id="1025c-191">maxExclusive</span><span class="sxs-lookup"><span data-stu-id="1025c-191">maxExclusive</span></span></td>
        <td><span data-ttu-id="1025c-192">hello maximala partition nyckel-hash-värde för hello partitionsnyckelintervallet.</span><span class="sxs-lookup"><span data-stu-id="1025c-192">hello maximum partition key hash value for hello partition key range.</span></span> <span data-ttu-id="1025c-193">För intern användning.</span><span class="sxs-lookup"><span data-stu-id="1025c-193">For internal use.</span></span></td>
    </tr>
    <tr>
        <td><span data-ttu-id="1025c-194">minInclusive</span><span class="sxs-lookup"><span data-stu-id="1025c-194">minInclusive</span></span></td>
        <td><span data-ttu-id="1025c-195">hello minsta partition nyckel-hash-värde för hello partitionsnyckelintervallet.</span><span class="sxs-lookup"><span data-stu-id="1025c-195">hello minimum partition key hash value for hello partition key range.</span></span> <span data-ttu-id="1025c-196">För intern användning.</span><span class="sxs-lookup"><span data-stu-id="1025c-196">For internal use.</span></span></td>
    </tr>       
</table>

<span data-ttu-id="1025c-197">Du kan göra detta med hjälp av en av hello stöds [Azure Cosmos DB SDK](documentdb-sdk-dotnet.md).</span><span class="sxs-lookup"><span data-stu-id="1025c-197">You can do this using one of hello supported [Azure Cosmos DB SDKs](documentdb-sdk-dotnet.md).</span></span> <span data-ttu-id="1025c-198">Till exempel hello följande utdrag visar hur tooretrieve partitionsnyckel intervall i .NET med hjälp av hello [ReadPartitionKeyRangeFeedAsync](/dotnet/api/microsoft.azure.documents.client.documentclient.readpartitionkeyrangefeedasync?view=azure-dotnet) metod.</span><span class="sxs-lookup"><span data-stu-id="1025c-198">For example, hello following snippet shows how tooretrieve partition key ranges in .NET using hello [ReadPartitionKeyRangeFeedAsync](/dotnet/api/microsoft.azure.documents.client.documentclient.readpartitionkeyrangefeedasync?view=azure-dotnet) method.</span></span>

```csharp
string pkRangesResponseContinuation = null;
List<PartitionKeyRange> partitionKeyRanges = new List<PartitionKeyRange>();

do
{
    FeedResponse<PartitionKeyRange> pkRangesResponse = await client.ReadPartitionKeyRangeFeedAsync(
        collectionUri, 
        new FeedOptions { RequestContinuation = pkRangesResponseContinuation });

    partitionKeyRanges.AddRange(pkRangesResponse);
    pkRangesResponseContinuation = pkRangesResponse.ResponseContinuation;
}
while (pkRangesResponseContinuation != null);
```

<span data-ttu-id="1025c-199">Azure Cosmos-DB stöder hämtning av dokument per partitionsnyckelintervallet genom att ange hello valfria `x-ms-documentdb-partitionkeyrangeid` huvud.</span><span class="sxs-lookup"><span data-stu-id="1025c-199">Azure Cosmos DB supports retrieval of documents per partition key range by setting hello optional `x-ms-documentdb-partitionkeyrangeid` header.</span></span> 

### <a name="performing-an-incremental-readdocumentfeed"></a><span data-ttu-id="1025c-200">Utför en inkrementell ReadDocumentFeed</span><span class="sxs-lookup"><span data-stu-id="1025c-200">Performing an incremental ReadDocumentFeed</span></span>
<span data-ttu-id="1025c-201">ReadDocumentFeed stöder hello följande scenarier/uppgifter för inkrementell behandling av ändringar i Azure Cosmos DB samlingar:</span><span class="sxs-lookup"><span data-stu-id="1025c-201">ReadDocumentFeed supports hello following scenarios/tasks for incremental processing of changes in Azure Cosmos DB collections:</span></span>

* <span data-ttu-id="1025c-202">Läs alla ändras toodocuments från början hello, det vill säga från skapa en samling.</span><span class="sxs-lookup"><span data-stu-id="1025c-202">Read all changes toodocuments from hello beginning, that is, from collection creation.</span></span>
* <span data-ttu-id="1025c-203">Läs alla ändringar toofuture uppdateringar toodocuments från aktuell tid eller ändringar sedan en tid som angetts av användaren.</span><span class="sxs-lookup"><span data-stu-id="1025c-203">Read all changes toofuture updates toodocuments from current time, or any changes since a user-specified time.</span></span>
* <span data-ttu-id="1025c-204">Läsa alla ändringar toodocuments från en logisk version av hello samling (ETag).</span><span class="sxs-lookup"><span data-stu-id="1025c-204">Read all changes toodocuments from a logical version of hello collection (ETag).</span></span> <span data-ttu-id="1025c-205">Du kan kontrollpunkt som dina användare baserat på hello ETag som returnerades från inkrementella Läs-feed-begäranden.</span><span class="sxs-lookup"><span data-stu-id="1025c-205">You can checkpoint your consumers based on hello returned ETag from incremental read-feed requests.</span></span>

<span data-ttu-id="1025c-206">hello ändringar är INSERT och Update toodocuments.</span><span class="sxs-lookup"><span data-stu-id="1025c-206">hello changes include inserts and updates toodocuments.</span></span> <span data-ttu-id="1025c-207">tar bort toocapture, du måste använda en ”mjuk borttagning”-egenskap i dokument eller använda hello [inbyggd TTL-egenskap](time-to-live.md) toosignal en väntande borttagning i hello ändra feed.</span><span class="sxs-lookup"><span data-stu-id="1025c-207">toocapture deletes, you must use a "soft delete" property within your documents, or use hello [built-in TTL property](time-to-live.md) toosignal a pending deletion in hello change feed.</span></span>

<span data-ttu-id="1025c-208">följande tabell visar hello hello [begäran](/rest/api/documentdb/common-documentdb-rest-request-headers.md) och [svarshuvuden](/rest/api/documentdb/common-documentdb-rest-response-headers.md) för ReadDocumentFeed åtgärder.</span><span class="sxs-lookup"><span data-stu-id="1025c-208">hello following table lists hello [request](/rest/api/documentdb/common-documentdb-rest-request-headers.md) and [response headers](/rest/api/documentdb/common-documentdb-rest-response-headers.md) for ReadDocumentFeed operations.</span></span>

<span data-ttu-id="1025c-209">**Begärandehuvuden för inkrementell ReadDocumentFeed**:</span><span class="sxs-lookup"><span data-stu-id="1025c-209">**Request headers for incremental ReadDocumentFeed**:</span></span>

<table>
    <tr>
        <th><span data-ttu-id="1025c-210">Huvudets namn</span><span class="sxs-lookup"><span data-stu-id="1025c-210">Header name</span></span></th>
        <th><span data-ttu-id="1025c-211">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="1025c-211">Description</span></span></th>
    </tr>
    <tr>
        <td><span data-ttu-id="1025c-212">EN IM</span><span class="sxs-lookup"><span data-stu-id="1025c-212">A-IM</span></span></td>
        <td><span data-ttu-id="1025c-213">Måste anges för ”stegvis feed”, eller på annat sätt utelämnad</span><span class="sxs-lookup"><span data-stu-id="1025c-213">Must be set too"Incremental feed", or omitted otherwise</span></span></td>
    </tr>
    <tr>
        <td><span data-ttu-id="1025c-214">If-None-Match</span><span class="sxs-lookup"><span data-stu-id="1025c-214">If-None-Match</span></span></td>
        <td>
            <p><span data-ttu-id="1025c-215">Inget huvud: returnerar alla ändringar från hello från (skapa samling)</span><span class="sxs-lookup"><span data-stu-id="1025c-215">No header: returns all changes from hello beginning (collection creation)</span></span></p>
            <p><span data-ttu-id="1025c-216">”*”: returnerar alla nya ändringar toodata inom hello samlingen</span><span class="sxs-lookup"><span data-stu-id="1025c-216">"*": returns all new changes toodata within hello collection</span></span></p>         
            <p><span data-ttu-id="1025c-217">&lt;ETag&gt;: om ange tooa samling ETag, returneras alla ändringar som gjorts sedan den logiska tidsstämpeln</span><span class="sxs-lookup"><span data-stu-id="1025c-217">&lt;etag&gt;: If set tooa collection ETag, returns all changes made since that logical timestamp</span></span></p>
        </td>
    </tr>
    <tr>    
        <td><span data-ttu-id="1025c-218">If-Modified-Since</span><span class="sxs-lookup"><span data-stu-id="1025c-218">If-Modified-Since</span></span></td> 
        <td><span data-ttu-id="1025c-219">RFC 1123 tidsformat; ignoreras om If-None-Match anges</span><span class="sxs-lookup"><span data-stu-id="1025c-219">RFC 1123 time format; ignored if If-None-Match is specified</span></span></td> 
    </tr> 
    <tr>
        <td><span data-ttu-id="1025c-220">x-ms-documentdb-partitionkeyrangeid</span><span class="sxs-lookup"><span data-stu-id="1025c-220">x-ms-documentdb-partitionkeyrangeid</span></span></td>
        <td><span data-ttu-id="1025c-221">hello partition viktiga intervallet ID för läsning av data.</span><span class="sxs-lookup"><span data-stu-id="1025c-221">hello partition key range ID for reading data.</span></span></td>
    </tr>
</table>

<span data-ttu-id="1025c-222">**Svarshuvuden för inkrementell ReadDocumentFeed**:</span><span class="sxs-lookup"><span data-stu-id="1025c-222">**Response headers for incremental ReadDocumentFeed**:</span></span>

<table> <tr>
        <th><span data-ttu-id="1025c-223">Huvudets namn</span><span class="sxs-lookup"><span data-stu-id="1025c-223">Header name</span></span></th>
        <th><span data-ttu-id="1025c-224">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="1025c-224">Description</span></span></th>
    </tr>
    <tr>
        <td><span data-ttu-id="1025c-225">ETag</span><span class="sxs-lookup"><span data-stu-id="1025c-225">etag</span></span></td>
        <td>
            <p><span data-ttu-id="1025c-226">hello logiska sekvensnumret (LSN) för senaste dokument i hello svar.</span><span class="sxs-lookup"><span data-stu-id="1025c-226">hello logical sequence number (LSN) of last document returned in hello response.</span></span></p>
            <p><span data-ttu-id="1025c-227">Inkrementell ReadDocumentFeed återupptas med det här värdet i If-None-Match skickar på nytt.</span><span class="sxs-lookup"><span data-stu-id="1025c-227">Incremental ReadDocumentFeed can be resumed by resubmitting this value in If-None-Match.</span></span></p>
        </td>
    </tr>
</table>

<span data-ttu-id="1025c-228">Här är ett exempel begäran tooreturn alla inkrementella ändringar i samlingen från hello logiska version/ETag `28535` och partitionera viktiga intervallet = `16`:</span><span class="sxs-lookup"><span data-stu-id="1025c-228">Here's a sample request tooreturn all incremental changes in collection from hello logical version/ETag `28535` and partition key range = `16`:</span></span>

    GET https://mydocumentdb.documents.azure.com/dbs/bigdb/colls/bigcoll/docs HTTP/1.1
    x-ms-max-item-count: 1
    If-None-Match: "28535"
    A-IM: Incremental feed
    x-ms-documentdb-partitionkeyrangeid: 16
    x-ms-date: Tue, 22 Nov 2016 20:43:01 GMT
    authorization: type%3dmaster%26ver%3d1.0%26sig%3dzdpL2QQ8TCfiNbW%2fEcT88JHNvWeCgDA8gWeRZ%2btfN5o%3d
    x-ms-version: 2016-07-11
    Accept: application/json
    Host: mydocumentdb.documents.azure.com

<span data-ttu-id="1025c-229">Ändringar ordnas efter tid inom varje partitionsnyckelvärde inom hello partitionsnyckelintervallet.</span><span class="sxs-lookup"><span data-stu-id="1025c-229">Changes are ordered by time within each partition key value within hello partition key range.</span></span> <span data-ttu-id="1025c-230">Det finns ingen garanterad ordning över partitionsnyckel värden.</span><span class="sxs-lookup"><span data-stu-id="1025c-230">There is no guaranteed order across partition-key values.</span></span> <span data-ttu-id="1025c-231">Om det finns fler resultat än vad som får plats i en enstaka sida, du kan läsa hello nästa sida i resultatet av skickar på nytt hello begäran med hello `If-None-Match` huvud med värde lika toohello `etag` från föregående hello-svar.</span><span class="sxs-lookup"><span data-stu-id="1025c-231">If there are more results than can fit in a single page, you can read hello next page of results by resubmitting hello request with hello `If-None-Match` header with value equal toohello `etag` from hello previous response.</span></span> <span data-ttu-id="1025c-232">Om flera dokument har infogats eller uppdaterats transaktionellt inom en lagrad procedur eller utlösare, de alla returneras inom hello samma svar sidan.</span><span class="sxs-lookup"><span data-stu-id="1025c-232">If multiple documents were inserted or updated transactionally within a stored procedure or trigger, they will all be returned within hello same response page.</span></span>

> [!NOTE]
> <span data-ttu-id="1025c-233">Med ändringen feed du få fler objekt som returneras i en sida än i `x-ms-max-item-count` hello gäller flera dokument infogas eller uppdateras i lagrade procedurer eller utlösare.</span><span class="sxs-lookup"><span data-stu-id="1025c-233">With change feed, you might get more items returned in a page than specified in `x-ms-max-item-count` in hello case of multiple documents inserted or updated inside a stored procedures or triggers.</span></span> 

<span data-ttu-id="1025c-234">När du använder hello .NET SDK (1.17.0), ange hello fältet `StartTime` i `ChangeFeedOptions` toodirectly returnerade ändras dokument eftersom `StartTime` vid anrop av `CreateDocumentChangeFeedQuery`.</span><span class="sxs-lookup"><span data-stu-id="1025c-234">When using hello .NET SDK (1.17.0), set hello field `StartTime` in `ChangeFeedOptions` toodirectly return changed documents since `StartTime` when calling  `CreateDocumentChangeFeedQuery`.</span></span> <span data-ttu-id="1025c-235">Genom att ange `If-Modified-Since` med hello REST-API kan din begäran returnerar inte hello dokument själva, men i stället hello fortsättningstoken eller `etag` i hello-Svarsrubrik.</span><span class="sxs-lookup"><span data-stu-id="1025c-235">By specifying `If-Modified-Since` using hello REST API, your request will return not hello documents themselves, but rather hello continuation token or `etag` in hello response header.</span></span> <span data-ttu-id="1025c-236">tooreturn hello dokument ändrade hello angiven tid, hello fortsättningstoken `etag` sedan måste användas i hello nästa begäran med `If-None-Match` tooreturn hello faktiska dokument.</span><span class="sxs-lookup"><span data-stu-id="1025c-236">tooreturn hello documents modified hello specified time, hello continuation token `etag` must then be used in hello next request with `If-None-Match` tooreturn hello actual documents.</span></span> 

<span data-ttu-id="1025c-237">hello .NET SDK innehåller hello [CreateDocumentChangeFeedQuery](/dotnet/api/microsoft.azure.documents.client.documentclient.createdocumentchangefeedquery?view=azure-dotnet) och [ChangeFeedOptions](/dotnet/api/microsoft.azure.documents.client.changefeedoptions?view=azure-dotnet) helper klasserna tooaccess ändringar tooa samling.</span><span class="sxs-lookup"><span data-stu-id="1025c-237">hello .NET SDK provides hello [CreateDocumentChangeFeedQuery](/dotnet/api/microsoft.azure.documents.client.documentclient.createdocumentchangefeedquery?view=azure-dotnet) and [ChangeFeedOptions](/dotnet/api/microsoft.azure.documents.client.changefeedoptions?view=azure-dotnet) helper classes tooaccess changes made tooa collection.</span></span> <span data-ttu-id="1025c-238">hello följande utdrag visar hur tooretrieve alla ändras från hello börjar med hello .NET SDK från en enskild klient.</span><span class="sxs-lookup"><span data-stu-id="1025c-238">hello following snippet shows how tooretrieve all changes from hello beginning using hello .NET SDK from a single client.</span></span>

```csharp
private async Task<Dictionary<string, string>> GetChanges(
    DocumentClient client,
    string collection,
    Dictionary<string, string> checkpoints)
{
    string pkRangesResponseContinuation = null;
    List<PartitionKeyRange> partitionKeyRanges = new List<PartitionKeyRange>();

    do
    {
        FeedResponse<PartitionKeyRange> pkRangesResponse = await client.ReadPartitionKeyRangeFeedAsync(
            collectionUri, 
            new FeedOptions { RequestContinuation = pkRangesResponseContinuation });

        partitionKeyRanges.AddRange(pkRangesResponse);
        pkRangesResponseContinuation = pkRangesResponse.ResponseContinuation;
    }
    while (pkRangesResponseContinuation != null);

    foreach (PartitionKeyRange pkRange in partitionKeyRanges)
    {
        string continuation = null;
        checkpoints.TryGetValue(pkRange.Id, out continuation);

        IDocumentQuery<Document> query = client.CreateDocumentChangeFeedQuery(
            collection,
            new ChangeFeedOptions
            {
                PartitionKeyRangeId = pkRange.Id,
                StartFromBeginning = true,
                RequestContinuation = continuation,
                MaxItemCount = 1
            });

        while (query.HasMoreResults)
        {
            FeedResponse<DeviceReading> readChangesResponse = query.ExecuteNextAsync<DeviceReading>().Result;

            foreach (DeviceReading changedDocument in readChangesResponse)
            {
                Console.WriteLine(changedDocument.Id);
            }

            checkpoints[pkRange.Id] = readChangesResponse.ResponseContinuation;
        }
    }

    return checkpoints;
}
```
<span data-ttu-id="1025c-239">Och hello följande utdrag visar hur tooprocess ändras i realtid med Azure Cosmos DB med hjälp av hello ändra feed support och hello före funktionen.</span><span class="sxs-lookup"><span data-stu-id="1025c-239">And hello following snippet shows how tooprocess changes in real-time with Azure Cosmos DB by using hello change feed support and hello preceding function.</span></span> <span data-ttu-id="1025c-240">hello första anropet returnerar alla hello dokument i hello samling och hello andra returnerar bara hello två dokument som har skapats och som har skapats sedan den senaste kontrollpunkten för hello.</span><span class="sxs-lookup"><span data-stu-id="1025c-240">hello first call returns all hello documents in hello collection, and hello second only returns hello two documents created that were created since hello last checkpoint.</span></span>

```csharp
// Returns all documents in hello collection.
Dictionary<string, string> checkpoints = await GetChanges(client, collection, new Dictionary<string, string>());

await client.CreateDocumentAsync(collection, new DeviceReading { DeviceId = "xsensr-201", MetricType = "Temperature", Unit = "Celsius", MetricValue = 1000 });
await client.CreateDocumentAsync(collection, new DeviceReading { DeviceId = "xsensr-212", MetricType = "Pressure", Unit = "psi", MetricValue = 1000 });

// Returns only hello two documents created above.
checkpoints = await GetChanges(client, collection, checkpoints);
```

<span data-ttu-id="1025c-241">Du kan också filtrera hello ändra feed med klienten sida logik tooselectively bearbeta händelser.</span><span class="sxs-lookup"><span data-stu-id="1025c-241">You can also filter hello change feed using client side logic tooselectively process events.</span></span> <span data-ttu-id="1025c-242">Här är till exempel ett kodavsnitt som använder klienten sida LINQ tooprocess endast temperatur ändra händelser från enheten sensorer.</span><span class="sxs-lookup"><span data-stu-id="1025c-242">For example, here's a snippet that uses client side LINQ tooprocess only temperature change events from device sensors.</span></span>

```csharp
FeedResponse<DeviceReading> readChangesResponse = query.ExecuteNextAsync<DeviceReading>().Result;

foreach (DeviceReading changedDocument in 
    readChangesResponse.AsEnumerable().Where(d => d.MetricType == "Temperature" && d.MetricValue > 1000L))
{
    // trigger an action, like call an API
}
```

## <span data-ttu-id="1025c-243"><a id="change-feed-processor"></a>Ändra Feed Processor bibliotek</span><span class="sxs-lookup"><span data-stu-id="1025c-243"><a id="change-feed-processor"></a>Change Feed Processor library</span></span>
<span data-ttu-id="1025c-244">Ett annat alternativ är toouse hello [Azure Cosmos DB ändra Feed Processor biblioteket](https://docs.microsoft.com/azure/cosmos-db/documentdb-sdk-dotnet-changefeed), som kan hjälpa dig att enkelt distribuera händelsebearbetning från en ändring feed över flera konsumenter.</span><span class="sxs-lookup"><span data-stu-id="1025c-244">Another option is toouse hello [Azure Cosmos DB Change Feed Processor library](https://docs.microsoft.com/azure/cosmos-db/documentdb-sdk-dotnet-changefeed), which can help you easily distribute event processing from a change feed across multiple consumers.</span></span> <span data-ttu-id="1025c-245">hello-biblioteket är bra för att skapa ändra feed läsare på hello .NET-plattformen.</span><span class="sxs-lookup"><span data-stu-id="1025c-245">hello library is great for building change feed readers on hello .NET platform.</span></span> <span data-ttu-id="1025c-246">Vissa arbetsflöden som skulle förenklas genom att använda hello ändra Feed Processor bibliotek över hello-metoder som ingår i hello andra Cosmos DB SDK: er omfattar:</span><span class="sxs-lookup"><span data-stu-id="1025c-246">Some workflows that would be simplified by using hello Change Feed Processor library over hello methods included in hello other Cosmos DB SDKs include:</span></span> 

* <span data-ttu-id="1025c-247">Ta emot uppdateringar från ändra feed när lagras data över flera partitioner</span><span class="sxs-lookup"><span data-stu-id="1025c-247">Pulling updates from change feed when data is stored across multiple partitions</span></span>
* <span data-ttu-id="1025c-248">Flytta eller replikering av data från en samling tooanother</span><span class="sxs-lookup"><span data-stu-id="1025c-248">Moving or replicating data from one collection tooanother</span></span>
* <span data-ttu-id="1025c-249">Parallell körning av åtgärder som utlöses av uppdateringar toodata och ändrar feed</span><span class="sxs-lookup"><span data-stu-id="1025c-249">Parallel execution of actions triggered by updates toodata and change feed</span></span> 

<span data-ttu-id="1025c-250">Medan med hello API: er i hello Cosmos SDK innehåller exakt åtkomst toochange feeds uppdateringar i varje partition, förenklar med hjälp av hello ändra Feed Processor bibliotek läsning ändringar i partitioner och flera trådar som arbetar parallellt.</span><span class="sxs-lookup"><span data-stu-id="1025c-250">While using hello APIs in hello Cosmos SDKs provides precise access toochange feed updates in each partition, using hello Change Feed Processor library simplifies reading changes across partitions and multiple threads working in parallel.</span></span> <span data-ttu-id="1025c-251">I stället för att manuellt läsa ändringarna från varje behållare och spara en fortsättningstoken för varje partition, hanterar hello ändra Feed Processor automatiskt läsning ändringar över partitioner som använder en mekanism för lånet.</span><span class="sxs-lookup"><span data-stu-id="1025c-251">Instead of manually reading changes from each container and saving a continuation token for each partition, hello Change Feed Processor automatically manages reading changes across partitions using a lease mechanism.</span></span>

<span data-ttu-id="1025c-252">hello bibliotek är tillgängligt som ett NuGet-paket: [Microsoft.Azure.Documents.ChangeFeedProcessor](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB.ChangeFeedProcessor/) och från källkoden som en Github [exempel](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/ChangeFeedProcessor).</span><span class="sxs-lookup"><span data-stu-id="1025c-252">hello library is available as a NuGet Package: [Microsoft.Azure.Documents.ChangeFeedProcessor](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB.ChangeFeedProcessor/) and from source code as a Github [sample](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/ChangeFeedProcessor).</span></span> 

### <a name="understanding-change-feed-processor-library"></a><span data-ttu-id="1025c-253">Förstå ändra Feed Processor bibliotek</span><span class="sxs-lookup"><span data-stu-id="1025c-253">Understanding Change Feed Processor library</span></span> 

<span data-ttu-id="1025c-254">Det finns fyra huvudsakliga komponenter för att implementera hello ändra Feed Processor: hello övervakas samling, hello lån samling, hello värd för händelsebearbetning och hello konsumenter.</span><span class="sxs-lookup"><span data-stu-id="1025c-254">There are four main components of implementing hello Change Feed Processor: hello monitored collection, hello lease collection, hello processor host, and hello consumers.</span></span> 

<span data-ttu-id="1025c-255">**Insamling av övervakade:** hello övervakas samlingen är hello data från vilken hello ändra feed genereras.</span><span class="sxs-lookup"><span data-stu-id="1025c-255">**Monitored Collection:** hello monitored collection is hello data from which hello change feed is generated.</span></span> <span data-ttu-id="1025c-256">En INSERT och ändringar övervakas toohello samling återspeglas i hello ändra feed hello mängden.</span><span class="sxs-lookup"><span data-stu-id="1025c-256">Any inserts and changes toohello monitored collection are reflected in hello change feed of hello collection.</span></span> 

<span data-ttu-id="1025c-257">**Insamling av lånet:** hello lån samling koordinater bearbetning hello ändra feed över flera arbetare.</span><span class="sxs-lookup"><span data-stu-id="1025c-257">**Lease Collection:** hello lease collection coordinates processing hello change feed across multiple workers.</span></span> <span data-ttu-id="1025c-258">En separat samling är används toostore hello lån med en leasing per partition.</span><span class="sxs-lookup"><span data-stu-id="1025c-258">A separate collection is used toostore hello leases with one lease per partition.</span></span> <span data-ttu-id="1025c-259">Är det fördelaktigt toostore samlingen lån på ett annat konto med hello skriva region närmare toowhere hello ändra Feed Processor körs.</span><span class="sxs-lookup"><span data-stu-id="1025c-259">It is advantageous toostore this lease collection on a different account with hello write region closer toowhere hello Change Feed Processor is running.</span></span> <span data-ttu-id="1025c-260">Ett lån-objekt innehåller hello följande attribut:</span><span class="sxs-lookup"><span data-stu-id="1025c-260">A lease object contains hello following attributes:</span></span> 
* <span data-ttu-id="1025c-261">Ägare: Anger hello-värd som äger hello lån</span><span class="sxs-lookup"><span data-stu-id="1025c-261">Owner: Specifies hello host that owns hello lease</span></span>
* <span data-ttu-id="1025c-262">Fortsättning: Anger hello-läge (fortsättningstoken) i hello ändra flödet för en viss partition</span><span class="sxs-lookup"><span data-stu-id="1025c-262">Continuation: Specifies hello position (continuation token) in hello change feed for a particular partition</span></span>
* <span data-ttu-id="1025c-263">Tidsstämpel: Tid för senaste lån uppdaterades; Hej, tidsstämpel kan vara används toocheck om hello lån betraktas som upphört att gälla</span><span class="sxs-lookup"><span data-stu-id="1025c-263">Timestamp: Last time lease was updated; hello timestamp can be used toocheck whether hello lease is considered expired</span></span> 

<span data-ttu-id="1025c-264">**Värd för händelsebearbetning:** varje värd anger hur många partitioner tooprocess baserat på hur många andra instanser av värdar har aktiva lån.</span><span class="sxs-lookup"><span data-stu-id="1025c-264">**Processor Host:** Each host determines how many partitions tooprocess based on how many other instances of hosts have active leases.</span></span> 
1.  <span data-ttu-id="1025c-265">När en värd startas, skaffar lån toobalance hello arbetsbelastningen över alla värdar.</span><span class="sxs-lookup"><span data-stu-id="1025c-265">When a host starts up, it acquires leases toobalance hello workload across all hosts.</span></span> <span data-ttu-id="1025c-266">En värd förnyas regelbundet lån, så att förbli lån.</span><span class="sxs-lookup"><span data-stu-id="1025c-266">A host periodically renews leases, so leases remain active.</span></span> 
2.  <span data-ttu-id="1025c-267">Ett värden kontrollpunkter hello senaste fortsättning token tooits lån för varje läsning.</span><span class="sxs-lookup"><span data-stu-id="1025c-267">A host checkpoints hello last continuation token tooits lease for each read.</span></span> <span data-ttu-id="1025c-268">tooensure samtidighet säkerhet en värd kontrollerar hello ETag för varje lease-uppdatering.</span><span class="sxs-lookup"><span data-stu-id="1025c-268">tooensure concurrency safety, a host checks hello ETag for each lease update.</span></span> <span data-ttu-id="1025c-269">Andra kontrollpunkt strategier stöds också.</span><span class="sxs-lookup"><span data-stu-id="1025c-269">Other checkpoint strategies are also supported.</span></span>  
3.  <span data-ttu-id="1025c-270">En värd släpper alla lån vid avstängning, men behåller hello fortsättning information så att den kan återupptas senare läsning från hello lagrade kontrollpunkten.</span><span class="sxs-lookup"><span data-stu-id="1025c-270">Upon shutdown, a host releases all leases but keeps hello continuation information, so it can resume reading from hello stored checkpoint later.</span></span> 

<span data-ttu-id="1025c-271">Hello antalet värdar kan inte vara större än hello antalet partitioner (lån) just nu.</span><span class="sxs-lookup"><span data-stu-id="1025c-271">At this time hello number of hosts cannot be greater than hello number of partitions (leases).</span></span>

<span data-ttu-id="1025c-272">**Konsumenterna:** konsumenter eller arbetare, är trådar som utför hello ändra feed bearbetning som initieras av varje värd.</span><span class="sxs-lookup"><span data-stu-id="1025c-272">**Consumers:** Consumers, or workers, are threads that perform hello change feed processing initiated by each host.</span></span> <span data-ttu-id="1025c-273">Varje värd för händelsebearbetning kan ha flera konsumenter.</span><span class="sxs-lookup"><span data-stu-id="1025c-273">Each processor host can have multiple consumers.</span></span> <span data-ttu-id="1025c-274">Varje konsument läser hello ändra feed från hello partition som den är tilldelad tooand meddelar dess värden för ändringar och gått lån.</span><span class="sxs-lookup"><span data-stu-id="1025c-274">Each consumer reads hello change feed from hello partition it is assigned tooand notifies its host of changes and expired leases.</span></span>

<span data-ttu-id="1025c-275">toofurther förstå hur dessa fyra element i ändra Feed Processor tillsammans, ska vi titta på ett exempel i följande diagram hello.</span><span class="sxs-lookup"><span data-stu-id="1025c-275">toofurther understand how these four elements of Change Feed Processor work together, let's look at an example in hello following diagram.</span></span> <span data-ttu-id="1025c-276">hello övervakas samling lagrar dokument och använder hello ”stad” som hello partitionsnyckel.</span><span class="sxs-lookup"><span data-stu-id="1025c-276">hello monitored collection stores documents and uses hello "city" as hello partition key.</span></span> <span data-ttu-id="1025c-277">Vi kan se att hello blå partition innehåller dokument med hello ”stad” från ”A E” och så vidare.</span><span class="sxs-lookup"><span data-stu-id="1025c-277">We see that hello blue partition contains documents with hello "city" field from "A-E" and so on.</span></span> <span data-ttu-id="1025c-278">Det finns två värdar med två konsumenter läsning från hello fyra partitioner parallellt.</span><span class="sxs-lookup"><span data-stu-id="1025c-278">There are two hosts, each with two consumers reading from hello four partitions in parallel.</span></span> <span data-ttu-id="1025c-279">hello pilarna visar hello konsumenter läsning från en specifik plats i hello ändra feed.</span><span class="sxs-lookup"><span data-stu-id="1025c-279">hello arrows show hello consumers reading from a specific spot in hello change feed.</span></span> <span data-ttu-id="1025c-280">I hello första partition representerar hello mörkare blå oläst ändringar medan hello lätta blå representerar hello redan läsa ändringarna på hello ändra feed.</span><span class="sxs-lookup"><span data-stu-id="1025c-280">In hello first partition, hello darker blue represents unread changes while hello light blue represents hello already read changes on hello change feed.</span></span> <span data-ttu-id="1025c-281">hello värdar använder hello lån samling toostore ”fortsättning” värde tookeep reda på hello aktuella läsning placering för varje konsument.</span><span class="sxs-lookup"><span data-stu-id="1025c-281">hello hosts use hello lease collection toostore a "continuation" value tookeep track of hello current reading position for each consumer.</span></span> 

![Med hjälp av hello Azure Cosmos DB ändra feed värd för händelsebearbetning](./media/change-feed/changefeedprocessornew.png)

### <a name="using-change-feed-processor-library"></a><span data-ttu-id="1025c-283">Med hjälp av ändra Feed Processor-bibliotek</span><span class="sxs-lookup"><span data-stu-id="1025c-283">Using Change Feed Processor Library</span></span> 
<span data-ttu-id="1025c-284">hello följande avsnitt förklarar hur toouse hello ändra Feed Processor bibliotek i hello kontexten för att replikera ändringar från en källa samling tooa mål-samling.</span><span class="sxs-lookup"><span data-stu-id="1025c-284">hello following section explains how toouse hello Change Feed Processor library in hello context of replicating changes from a source collection tooa destination collection.</span></span> <span data-ttu-id="1025c-285">Här är hello källsamling hello övervakas samling i ändra Feed Processor.</span><span class="sxs-lookup"><span data-stu-id="1025c-285">Here, hello source collection is hello monitored collection in Change Feed Processor.</span></span> 

<span data-ttu-id="1025c-286">**Installera och inkludera hello ändra Feed Processor NuGet-paketet**</span><span class="sxs-lookup"><span data-stu-id="1025c-286">**Install and include hello Change Feed Processor NuGet package**</span></span> 

<span data-ttu-id="1025c-287">Innan du installerar ändra Feed Processor NuGet-paketet måste först installera:</span><span class="sxs-lookup"><span data-stu-id="1025c-287">Before installing Change Feed Processor NuGet Package, first install:</span></span> 
* <span data-ttu-id="1025c-288">Microsoft.Azure.DocumentDB, version 1.13.1 eller senare</span><span class="sxs-lookup"><span data-stu-id="1025c-288">Microsoft.Azure.DocumentDB, version 1.13.1 or above</span></span> 
* <span data-ttu-id="1025c-289">Newtonsoft.Json, version 9.0.1 eller senare installera `Microsoft.Azure.DocumentDB.ChangeFeedProcessor` och inkludera den som en referens.</span><span class="sxs-lookup"><span data-stu-id="1025c-289">Newtonsoft.Json, version 9.0.1 or above Install `Microsoft.Azure.DocumentDB.ChangeFeedProcessor` and include it as a reference.</span></span>

<span data-ttu-id="1025c-290">**Skapa en övervakas lån och mål-samling**</span><span class="sxs-lookup"><span data-stu-id="1025c-290">**Create a monitored, lease and destination collection**</span></span> 

<span data-ttu-id="1025c-291">I ordning toouse hello ändra Feed Processor bibliotek måste hello lån samling toobe skapats innan du kör hello processor värddatorer.</span><span class="sxs-lookup"><span data-stu-id="1025c-291">In order toouse hello Change Feed Processor Library, hello lease collection needs toobe created before running hello processor host(s).</span></span> <span data-ttu-id="1025c-292">Vi rekommenderar igen och lagra en samling lån på ett annat konto med en skrivning region närmare toowhere hello ändra Feed Processor körs.</span><span class="sxs-lookup"><span data-stu-id="1025c-292">Again, we recommend storing a lease collection on a different account with a write region closer toowhere hello Change Feed Processor is running.</span></span> <span data-ttu-id="1025c-293">I exemplet data movement måste vi toocreate hello målsamling innan du kör hello ändra Feed Processor värden.</span><span class="sxs-lookup"><span data-stu-id="1025c-293">In this data movement example, we need toocreate hello destination collection before running hello Change Feed Processor host.</span></span> <span data-ttu-id="1025c-294">I hello exempelkod vi kallar helper metoden toocreate hello övervakas, hyrd, och mål samlingar om de inte redan finns.</span><span class="sxs-lookup"><span data-stu-id="1025c-294">In hello sample code we call a helper method toocreate hello monitored, leased, and destination collections if they do not already exist.</span></span> 

> [!WARNING]
> <span data-ttu-id="1025c-295">Skapa en samling har prissättning effekter, som du reserverar genomströmning för hello programmet toocommunicate med Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="1025c-295">Creating a collection has pricing implications, as you are reserving throughput for hello application toocommunicate with Azure Cosmos DB.</span></span> <span data-ttu-id="1025c-296">Mer information finns i hello [sida med priser](https://azure.microsoft.com/pricing/details/cosmos-db/)</span><span class="sxs-lookup"><span data-stu-id="1025c-296">For more details, please visit hello [pricing page](https://azure.microsoft.com/pricing/details/cosmos-db/)</span></span>
> 
> 

<span data-ttu-id="1025c-297">*Skapa en värd för händelsebearbetning*</span><span class="sxs-lookup"><span data-stu-id="1025c-297">*Creating a processor host*</span></span>

<span data-ttu-id="1025c-298">Hej `ChangeFeedProcessorHost` klassen innehåller en trådsäker, flerprocessig, säker körningsmiljö för händelseprocessorer som också ger kontrollpunkter och hantering av partitionsleasing.</span><span class="sxs-lookup"><span data-stu-id="1025c-298">hello `ChangeFeedProcessorHost` class provides a thread-safe, multi-process, safe runtime environment for event processor implementations that also provides checkpointing and partition lease management.</span></span> <span data-ttu-id="1025c-299">toouse hello `ChangeFeedProcessorHost` klassen, som du kan implementera `IChangeFeedObserver`.</span><span class="sxs-lookup"><span data-stu-id="1025c-299">toouse hello `ChangeFeedProcessorHost` class, you can implement `IChangeFeedObserver`.</span></span> <span data-ttu-id="1025c-300">Det här gränssnittet innehåller tre metoder:</span><span class="sxs-lookup"><span data-stu-id="1025c-300">This interface contains three methods:</span></span>

* <span data-ttu-id="1025c-301">`OpenAsync`: Den här funktionen kallas när ändringen feed person öppnas.</span><span class="sxs-lookup"><span data-stu-id="1025c-301">`OpenAsync`: This function is called when change feed observer is opened.</span></span> <span data-ttu-id="1025c-302">Det kan vara ändrade tooperform en specifik åtgärd när konsumenten/person öppnas.</span><span class="sxs-lookup"><span data-stu-id="1025c-302">It can be modified tooperform a specific action when consumer/observer is opened.</span></span>  
* <span data-ttu-id="1025c-303">`CloseAsync`: Den här funktionen anropas när ändringen feed person avslutas.</span><span class="sxs-lookup"><span data-stu-id="1025c-303">`CloseAsync`: This function is called when change feed observer is terminated.</span></span> <span data-ttu-id="1025c-304">Det kan vara ändrade tooperform en specifik åtgärd när konsumenten/person är stängd.</span><span class="sxs-lookup"><span data-stu-id="1025c-304">It can be modified tooperform a specific action when consumer/observer is closed.</span></span>  
* <span data-ttu-id="1025c-305">`ProcessChangesAsync`: Den här funktionen kallas när dokumentet nya ändringar är tillgängliga på Ändra feed.</span><span class="sxs-lookup"><span data-stu-id="1025c-305">`ProcessChangesAsync`: This function is called when document new changes are available on change feed.</span></span> <span data-ttu-id="1025c-306">Det kan vara ändrade tooperform en specifik åtgärd vid varje ändring feed-uppdatering.</span><span class="sxs-lookup"><span data-stu-id="1025c-306">It can be modified tooperform a specific action upon every change feed update.</span></span>  

<span data-ttu-id="1025c-307">I vårt exempel vi implementera gränssnittet hello `IChangeFeedObserver` via hello `DocumentFeedObserver` klass.</span><span class="sxs-lookup"><span data-stu-id="1025c-307">In our example, we implement hello interface `IChangeFeedObserver` through hello `DocumentFeedObserver` class.</span></span> <span data-ttu-id="1025c-308">Här hello `ProcessChangesAsync` fungerar upserts (uppdateringar) ett dokument från ändra matas in hello målsamling.</span><span class="sxs-lookup"><span data-stu-id="1025c-308">Here, hello `ProcessChangesAsync` function upserts (updates) a document from change feed into hello destination collection.</span></span> <span data-ttu-id="1025c-309">Det här exemplet är användbart för att flytta data från en samling tooanother i ordning toochange hello partitionsnyckel av en datamängd.</span><span class="sxs-lookup"><span data-stu-id="1025c-309">This example is useful for moving data from one collection tooanother in order toochange hello partition key of a data set.</span></span> 

<span data-ttu-id="1025c-310">*Kör hello värd för händelsebearbetning*</span><span class="sxs-lookup"><span data-stu-id="1025c-310">*Running hello Processor Host*</span></span>

<span data-ttu-id="1025c-311">Innan du påbörjar händelsebearbetning både ändra alternativ för feed och ändra feed värdalternativ kan anpassas.</span><span class="sxs-lookup"><span data-stu-id="1025c-311">Before beginning event processing, both change feed options and change feed host options can be customized.</span></span> 
```csharp
    // Customizable change feed option and host options 
    ChangeFeedOptions feedOptions = new ChangeFeedOptions();

    // ie customize StartFromBeginning so change feed reads from beginning
    // can customize MaxItemCount, PartitonKeyRangeId, RequestContinuation, SessionToken and StartFromBeginning
    feedOptions.StartFromBeginning = true;

    ChangeFeedHostOptions feedHostOptions = new ChangeFeedHostOptions();

    // ie. customizing lease renewal interval too15 seconds
    // can customize LeaseRenewInterval, LeaseAcquireInterval, LeaseExpirationInterval, FeedPollDelay 
    feedHostOptions.LeaseRenewInterval = TimeSpan.FromSeconds(15);

```
<span data-ttu-id="1025c-312">hello specifika fält som kan anpassas sammanfattas i följande tabeller hello.</span><span class="sxs-lookup"><span data-stu-id="1025c-312">hello specific fields that can be customized are summarized in hello following tables.</span></span> 

<span data-ttu-id="1025c-313">**Ändra alternativ för feeds**:</span><span class="sxs-lookup"><span data-stu-id="1025c-313">**Change Feed Options**:</span></span>
<table>
    <tr>
        <th><span data-ttu-id="1025c-314">Egenskapsnamn</span><span class="sxs-lookup"><span data-stu-id="1025c-314">Property Name</span></span></th>
        <th><span data-ttu-id="1025c-315">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="1025c-315">Description</span></span></th>
    </tr>
    <tr>
        <td><span data-ttu-id="1025c-316">MaxItemCount</span><span class="sxs-lookup"><span data-stu-id="1025c-316">MaxItemCount</span></span></td>
        <td><span data-ttu-id="1025c-317">Hämtar eller anger hello maximalt antal objekt toobe returneras i hello uppräkningsåtgärden avbröts i hello Azure Cosmos DB database-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="1025c-317">Gets or sets hello maximum number of items toobe returned in hello enumeration operation in hello Azure Cosmos DB database service.</span></span></td>
    </tr>
    <tr>
        <td><span data-ttu-id="1025c-318">PartitionKeyRangeId</span><span class="sxs-lookup"><span data-stu-id="1025c-318">PartitionKeyRangeId</span></span></td>
        <td><span data-ttu-id="1025c-319">Hämtar eller anger hello partitions viktiga intervallet-id för hello aktuella begäran i hello Azure Cosmos DB database-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="1025c-319">Gets or sets hello partition key range id for hello current request in hello Azure Cosmos DB database service.</span></span></td>
    </tr>
    <tr>
        <td><span data-ttu-id="1025c-320">RequestContinuation</span><span class="sxs-lookup"><span data-stu-id="1025c-320">RequestContinuation</span></span></td>
        <td><span data-ttu-id="1025c-321">Hämtar eller anger hello begäran fortsättningstoken i hello Azure Cosmos DB database-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="1025c-321">Gets or sets hello request continuation token in hello Azure Cosmos DB database service.</span></span></td>
    </tr>
        <tr>
        <td><span data-ttu-id="1025c-322">SessionToken</span><span class="sxs-lookup"><span data-stu-id="1025c-322">SessionToken</span></span></td>
        <td><span data-ttu-id="1025c-323">Hämtar eller anger hello sessionstoken för användning med sessionskonsekvens i hello Azure Cosmos DB database-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="1025c-323">Gets or sets hello session token for use with session consistency in hello Azure Cosmos DB database service.</span></span></td>
    </tr>
        <tr>
        <td><span data-ttu-id="1025c-324">StartFromBeginning</span><span class="sxs-lookup"><span data-stu-id="1025c-324">StartFromBeginning</span></span></td>
        <td><span data-ttu-id="1025c-325">Hämtar eller anger om ändringen feeds i hello Azure Cosmos DB database-tjänsten ska börja från hello från (SANT) eller från aktuella (FALSKT).</span><span class="sxs-lookup"><span data-stu-id="1025c-325">Gets or sets whether change feed in hello Azure Cosmos DB database service should start from hello beginning (true) or from current (false).</span></span> <span data-ttu-id="1025c-326">Som standard startar från aktuella (FALSKT).</span><span class="sxs-lookup"><span data-stu-id="1025c-326">By default, it starts from current (false).</span></span></td>
    </tr>
</table>

<span data-ttu-id="1025c-327">**Ändra värdalternativ för feeds**:</span><span class="sxs-lookup"><span data-stu-id="1025c-327">**Change Feed Host Options**:</span></span>
<table>
    <tr>
        <th><span data-ttu-id="1025c-328">Egenskapsnamn</span><span class="sxs-lookup"><span data-stu-id="1025c-328">Property Name</span></span></th>
        <th><span data-ttu-id="1025c-329">Typ</span><span class="sxs-lookup"><span data-stu-id="1025c-329">Type</span></span></th>
        <th><span data-ttu-id="1025c-330">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="1025c-330">Description</span></span></th>
    </tr>
    <tr>
        <td><span data-ttu-id="1025c-331">LeaseRenewInterval</span><span class="sxs-lookup"><span data-stu-id="1025c-331">LeaseRenewInterval</span></span></td>
        <td><span data-ttu-id="1025c-332">TimeSpan</span><span class="sxs-lookup"><span data-stu-id="1025c-332">TimeSpan</span></span></td>
        <td><span data-ttu-id="1025c-333">hello-intervall för alla lån för partitioner som för tillfället hålls av hello ChangeFeedEventHost instans.</span><span class="sxs-lookup"><span data-stu-id="1025c-333">hello interval for all leases for partitions currently held by hello ChangeFeedEventHost instance.</span></span></td>
    </tr>
    <tr>
        <td><span data-ttu-id="1025c-334">LeaseAcquireInterval</span><span class="sxs-lookup"><span data-stu-id="1025c-334">LeaseAcquireInterval</span></span></td>
        <td><span data-ttu-id="1025c-335">TimeSpan</span><span class="sxs-lookup"><span data-stu-id="1025c-335">TimeSpan</span></span></td>
        <td><span data-ttu-id="1025c-336">Hej intervall tookick av en aktivitet toocompute om partitioner fördelas jämnt mellan kända värddatorinstanser.</span><span class="sxs-lookup"><span data-stu-id="1025c-336">hello interval tookick off a task toocompute whether partitions are distributed evenly among known host instances.</span></span></td>
    </tr>
    <tr>
        <td><span data-ttu-id="1025c-337">LeaseExpirationInterval</span><span class="sxs-lookup"><span data-stu-id="1025c-337">LeaseExpirationInterval</span></span></td>
        <td><span data-ttu-id="1025c-338">TimeSpan</span><span class="sxs-lookup"><span data-stu-id="1025c-338">TimeSpan</span></span></td>
        <td><span data-ttu-id="1025c-339">hello-intervall för vilka hello lån utförs på ett lån som representerar en partition.</span><span class="sxs-lookup"><span data-stu-id="1025c-339">hello interval for which hello lease is taken on a lease representing a partition.</span></span> <span data-ttu-id="1025c-340">Om hello leasen inte förnyas inom intervallet, den har upphört att gälla och ägarskapet för hello partition flyttas tooanother ChangeFeedEventHost instans.</span><span class="sxs-lookup"><span data-stu-id="1025c-340">If hello lease is not renewed within this interval, it is expired and ownership of hello partition moves tooanother ChangeFeedEventHost instance.</span></span></td>
    </tr>
    <tr>
        <td><span data-ttu-id="1025c-341">FeedPollDelay</span><span class="sxs-lookup"><span data-stu-id="1025c-341">FeedPollDelay</span></span></td>
        <td><span data-ttu-id="1025c-342">TimeSpan</span><span class="sxs-lookup"><span data-stu-id="1025c-342">TimeSpan</span></span></td>
        <td><span data-ttu-id="1025c-343">hello fördröjning mellan avsöker en partition för nya ändringar på hello feed när alla aktuella ändringar är tar slut.</span><span class="sxs-lookup"><span data-stu-id="1025c-343">hello delay between polling a partition for new changes on hello feed, after all current changes are drained.</span></span></td>
    </tr>
    <tr>
        <td><span data-ttu-id="1025c-344">CheckpointFrequency</span><span class="sxs-lookup"><span data-stu-id="1025c-344">CheckpointFrequency</span></span></td>
        <td><span data-ttu-id="1025c-345">CheckpointFrequency</span><span class="sxs-lookup"><span data-stu-id="1025c-345">CheckpointFrequency</span></span></td>
        <td><span data-ttu-id="1025c-346">hello frekvens toocheckpoint lån.</span><span class="sxs-lookup"><span data-stu-id="1025c-346">hello frequency toocheckpoint leases.</span></span></td>
    </tr>
    <tr>
        <td><span data-ttu-id="1025c-347">MinPartitionCount</span><span class="sxs-lookup"><span data-stu-id="1025c-347">MinPartitionCount</span></span></td>
        <td><span data-ttu-id="1025c-348">int</span><span class="sxs-lookup"><span data-stu-id="1025c-348">Int</span></span></td>
        <td><span data-ttu-id="1025c-349">hello minsta partitionsantal för hello värden.</span><span class="sxs-lookup"><span data-stu-id="1025c-349">hello minimum partition count for hello host.</span></span></td>
    </tr>
    <tr>
        <td><span data-ttu-id="1025c-350">MaxPartitionCount</span><span class="sxs-lookup"><span data-stu-id="1025c-350">MaxPartitionCount</span></span></td>
        <td><span data-ttu-id="1025c-351">int</span><span class="sxs-lookup"><span data-stu-id="1025c-351">Int</span></span></td>
        <td><span data-ttu-id="1025c-352">hello maximalt antal partitioner hello värden kan fungera.</span><span class="sxs-lookup"><span data-stu-id="1025c-352">hello maximum number of partitions hello host can serve.</span></span></td>
    </tr>
    <tr>
        <td><span data-ttu-id="1025c-353">DiscardExistingLeases</span><span class="sxs-lookup"><span data-stu-id="1025c-353">DiscardExistingLeases</span></span></td>
        <td><span data-ttu-id="1025c-354">bool</span><span class="sxs-lookup"><span data-stu-id="1025c-354">Bool</span></span></td>
        <td><span data-ttu-id="1025c-355">Om hello starta på hello värdens alla befintliga lån ska tas bort och hello värden ska börja från början.</span><span class="sxs-lookup"><span data-stu-id="1025c-355">Whether on hello start of hello host all existing leases should be deleted and hello host should start from scratch.</span></span></td>
    </tr>
</table>


<span data-ttu-id="1025c-356">Skapa en instans av toostart händelsebearbetning, `ChangeFeedProcessorHost`, tillhandahåller hello lämpliga parametrar för din Azure DB som Cosmos-samling.</span><span class="sxs-lookup"><span data-stu-id="1025c-356">toostart event processing, instantiate `ChangeFeedProcessorHost`, providing hello appropriate parameters for your Azure Cosmos DB collection.</span></span> <span data-ttu-id="1025c-357">Anropa sedan `RegisterObserverAsync` tooregister din `IChangeFeedObserver` (DocumentFeedObserver i det här exemplet) implementering med hello körning.</span><span class="sxs-lookup"><span data-stu-id="1025c-357">Then, call `RegisterObserverAsync` tooregister your `IChangeFeedObserver` (DocumentFeedObserver in this example) implementation with hello runtime.</span></span> <span data-ttu-id="1025c-358">Hello-värden försöker nu tooacquire ett lån på varje partitionsnyckelintervallet i hello Azure Cosmos DB samlingen med en ”girig” algoritm.</span><span class="sxs-lookup"><span data-stu-id="1025c-358">At this point, hello host attempts tooacquire a lease on every partition key range in hello Azure Cosmos DB collection using a "greedy" algorithm.</span></span> <span data-ttu-id="1025c-359">De här leasingarna varar en viss tid och måste sedan förnyas.</span><span class="sxs-lookup"><span data-stu-id="1025c-359">These leases last for a given timeframe and must then be renewed.</span></span> <span data-ttu-id="1025c-360">Allteftersom nya noder worker-instanser i det här fallet är online, de placerar den ut leasingreservationer och med tiden skiftar hello belastningen mellan noder som varje värd försöker tooacquire mer lån.</span><span class="sxs-lookup"><span data-stu-id="1025c-360">As new nodes, worker instances, in this case, come online, they place lease reservations and over time hello load shifts between nodes as each host attempts tooacquire more leases.</span></span> 

<span data-ttu-id="1025c-361">I hello exempelkod använder vi en fabrik klass (DocumentFeedObserverFactory.cs) toocreate en person och hello `RegistObserverFactoryAsync` tooregister hello person.</span><span class="sxs-lookup"><span data-stu-id="1025c-361">In hello sample code, we use a factory class (DocumentFeedObserverFactory.cs) toocreate an observer and hello `RegistObserverFactoryAsync` tooregister hello observer.</span></span> 

```csharp
using (DocumentClient destClient = new DocumentClient(destCollInfo.Uri, destCollInfo.MasterKey))
    {
        DocumentFeedObserverFactory docObserverFactory = new DocumentFeedObserverFactory(destClient, destCollInfo);
        ChangeFeedEventHost host = new ChangeFeedEventHost(hostName, documentCollectionLocation, leaseCollectionLocation, feedOptions, feedHostOptions);

        await host.RegisterObserverFactoryAsync(docObserverFactory);

        Console.WriteLine("Running... Press enter toostop.");
        Console.ReadLine();

        await host.UnregisterObserversAsync();
    }
```
<span data-ttu-id="1025c-362">Med tiden etableras ett jämviktsläge.</span><span class="sxs-lookup"><span data-stu-id="1025c-362">Over time, an equilibrium is established.</span></span> <span data-ttu-id="1025c-363">Den här dynamiska funktionen gör att processorbaserad automatisk skalning toobe tillämpas tooconsumers för både skala upp och skala ned.</span><span class="sxs-lookup"><span data-stu-id="1025c-363">This dynamic capability enables CPU-based auto-scaling toobe applied tooconsumers for both scale-up and scale-down.</span></span> <span data-ttu-id="1025c-364">Om ändringarna är tillgängliga i Azure Cosmos DB i en snabbare takt än konsumenterna kan bearbeta kan hello CPU för konsumenterna vara används toocause Autoskala på worker-instanser.</span><span class="sxs-lookup"><span data-stu-id="1025c-364">If changes are available in Azure Cosmos DB at a faster rate than consumers can process, hello CPU increase on consumers can be used toocause an auto-scale on worker instance count.</span></span>

## <a name="next-steps"></a><span data-ttu-id="1025c-365">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="1025c-365">Next steps</span></span>
* <span data-ttu-id="1025c-366">Försök hello [Azure Cosmos DB ändra feed kodexempel på GitHub](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/code-samples/ChangeFeed)</span><span class="sxs-lookup"><span data-stu-id="1025c-366">Try hello [Azure Cosmos DB Change feed code samples on GitHub](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/code-samples/ChangeFeed)</span></span>
* <span data-ttu-id="1025c-367">Komma igång med programmeringen med hello [Azure Cosmos DB SDK](documentdb-sdk-dotnet.md) eller hello [REST API](/rest/api/documentdb/).</span><span class="sxs-lookup"><span data-stu-id="1025c-367">Get started coding with hello [Azure Cosmos DB SDKs](documentdb-sdk-dotnet.md) or hello [REST API](/rest/api/documentdb/).</span></span>
