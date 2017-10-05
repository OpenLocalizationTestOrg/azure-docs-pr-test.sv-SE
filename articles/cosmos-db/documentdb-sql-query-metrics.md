---
title: "SQL-frågan mätvärden för Azure Cosmos DB DocumentDB API | Microsoft Docs"
description: "Lär dig mer om hur du instrumentera och felsöka frågeprestanda SQL Azure DB som Cosmos-begäranden."
keywords: "SQL-syntax, sql-fråga, sql-frågor, json-frågespråket, databasbegrepp och sql-frågor, mängdfunktioner"
services: cosmos-db
documentationcenter: 
author: arramac
manager: jhubbard
editor: monicar
ms.assetid: b2fa8e8f-7291-45a3-9bd1-7284ed9077f8
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/15/2017
ms.author: arramac
ms.openlocfilehash: d928113e809e5ad43901e79dc256a8a39c210181
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="tuning-query-performance-with-azure-cosmos-db"></a><span data-ttu-id="b2582-104">Justera prestanda för frågor med Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="b2582-104">Tuning query performance with Azure Cosmos DB</span></span>
<span data-ttu-id="b2582-105">Azure Cosmos-DB tillhandahåller en [SQL API för datafrågor](documentdb-sql-query.md), utan att schemat eller sekundärindex.</span><span class="sxs-lookup"><span data-stu-id="b2582-105">Azure Cosmos DB provides a [SQL API for querying data](documentdb-sql-query.md), without requiring schema or secondary indexes.</span></span> <span data-ttu-id="b2582-106">Den här artikeln innehåller följande information för utvecklare:</span><span class="sxs-lookup"><span data-stu-id="b2582-106">This article provides the following information for developers:</span></span>

* <span data-ttu-id="b2582-107">Utförlig information om hur fungerar Azure Cosmos DB SQL-frågan</span><span class="sxs-lookup"><span data-stu-id="b2582-107">High-level details on how Azure Cosmos DB's SQL query execution works</span></span>
* <span data-ttu-id="b2582-108">Information om frågan begärande- och svarshuvuden och alternativ för klient-SDK</span><span class="sxs-lookup"><span data-stu-id="b2582-108">Details on query request and response headers, and client SDK options</span></span>
* <span data-ttu-id="b2582-109">Tips och bästa praxis för prestanda för frågor</span><span class="sxs-lookup"><span data-stu-id="b2582-109">Tips and best practices for query performance</span></span>
* <span data-ttu-id="b2582-110">Exempel på hur du använder SQL-körningen statistik för att felsöka prestanda för frågor</span><span class="sxs-lookup"><span data-stu-id="b2582-110">Examples of how to utilize SQL execution statistics to debug query performance</span></span>

## <a name="about-sql-query-execution"></a><span data-ttu-id="b2582-111">Om körning av SQL-fråga</span><span class="sxs-lookup"><span data-stu-id="b2582-111">About SQL query execution</span></span>

<span data-ttu-id="b2582-112">I Azure Cosmos DB du lagra data i behållare, som kan växa till någon [storlek eller begäran genomflödet](partition-data.md).</span><span class="sxs-lookup"><span data-stu-id="b2582-112">In Azure Cosmos DB, you store data in containers, which can grow to any [storage size or request throughput](partition-data.md).</span></span> <span data-ttu-id="b2582-113">Azure Cosmos-DB skalas sömlöst data över fysiska partitioner under försättsbladen att hantera datatillväxt eller ökar med etablerat dataflöde.</span><span class="sxs-lookup"><span data-stu-id="b2582-113">Azure Cosmos DB seamlessly scales data across physical partitions under the covers to handle data growth or increase in provisioned throughput.</span></span> <span data-ttu-id="b2582-114">Du kan utfärda SQL-frågor till en behållare med REST API eller något av de stöds [DocumentDB SDK: er](documentdb-sdk-dotnet.md).</span><span class="sxs-lookup"><span data-stu-id="b2582-114">You can issue SQL queries to any container using the REST API or one of the supported [DocumentDB SDKs](documentdb-sdk-dotnet.md).</span></span>

<span data-ttu-id="b2582-115">En kort översikt över partitionering: du anger en partitionsnyckel som ”stad”, som avgör hur data delas mellan fysiska partitioner.</span><span class="sxs-lookup"><span data-stu-id="b2582-115">A brief overview of partitioning: you define a partition key like "city", which determines how data is split across physical partitions.</span></span> <span data-ttu-id="b2582-116">Data som hör till en enda partitionsnyckel (till exempel ”stad” == ”Seattle”) lagras i en fysiska partition, men en enda fysisk partition har vanligtvis flera partitionsnycklar.</span><span class="sxs-lookup"><span data-stu-id="b2582-116">Data belonging to a single partition key (for example, "city" == "Seattle") is stored within a physical partition, but typically a single physical partition has multiple partition keys.</span></span> <span data-ttu-id="b2582-117">När en partition når lagringsstorlek, tjänsten sömlöst delar partitionen i två nya partitioner och dividerar Partitionsnyckeln jämnt mellan dessa partitioner.</span><span class="sxs-lookup"><span data-stu-id="b2582-117">When a partition reaches its storage size, the service seamlessly splits the partition into two new partitions, and divides the partition key evenly across these partitions.</span></span> <span data-ttu-id="b2582-118">Eftersom partitioner är tillfälligt använda API: er i en abstraktion av en ”partitionsnyckelintervallet”, som anger intervall för partition viktiga hashvärden.</span><span class="sxs-lookup"><span data-stu-id="b2582-118">Since partitions are transient, the APIs use an abstraction of a "partition key range", which denotes the ranges of partition key hashes.</span></span> 

<span data-ttu-id="b2582-119">När du skickar en fråga till Azure Cosmos DB utför stegen logiska SDK:</span><span class="sxs-lookup"><span data-stu-id="b2582-119">When you issue a query to Azure Cosmos DB, the SDK performs these logical steps:</span></span>

* <span data-ttu-id="b2582-120">Parsa SQL-frågan för att fastställa körning frågeplanen.</span><span class="sxs-lookup"><span data-stu-id="b2582-120">Parse the SQL query to determine the query execution plan.</span></span> 
* <span data-ttu-id="b2582-121">Om frågan innehåller ett filter mot Partitionsnyckeln, som `SELECT * FROM c WHERE c.city = "Seattle"`, dirigeras till en enskild partition.</span><span class="sxs-lookup"><span data-stu-id="b2582-121">If the query includes a filter against the partition key, like `SELECT * FROM c WHERE c.city = "Seattle"`, it is routed to a single partition.</span></span> <span data-ttu-id="b2582-122">Om frågan inte har ett filter på partitionsnyckel, sedan den körs i alla partitioner och resultat kombineras på klientsidan.</span><span class="sxs-lookup"><span data-stu-id="b2582-122">If the query does not have a filter on partition key, then it is executed in all partitions, and results are merged client side.</span></span>
* <span data-ttu-id="b2582-123">Frågan köras inom varje partition i serien eller parallell, baserat på klientens konfiguration.</span><span class="sxs-lookup"><span data-stu-id="b2582-123">The query is executed within each partition in series or parallel, based on client configuration.</span></span> <span data-ttu-id="b2582-124">Inom varje partition frågan kan göra att en eller flera sändningar beroende på hur komplex fråga konfigurerats sidstorlek och etableras genomflöde i mängden.</span><span class="sxs-lookup"><span data-stu-id="b2582-124">Within each partition, the query might make one or more round trips depending on the query complexity, configured page size, and provisioned throughput of the collection.</span></span> <span data-ttu-id="b2582-125">Varje körning returnerar antalet [programbegäran](request-units.md) används genom att köra frågor och eventuellt statistik för körning av frågan.</span><span class="sxs-lookup"><span data-stu-id="b2582-125">Each execution returns the number of [request units](request-units.md) consumed by query execution, and optionally, query execution statistics.</span></span> 
* <span data-ttu-id="b2582-126">SDK utför en sammanfattning av frågeresultatet över partitioner.</span><span class="sxs-lookup"><span data-stu-id="b2582-126">The SDK performs a summarization of the query results across partitions.</span></span> <span data-ttu-id="b2582-127">Till exempel om frågan innefattar en ORDER BY över partitioner, är sedan resultaten från enskilda partitioner merge sorteras du returnerar resultat på globalt sorteringsordningen.</span><span class="sxs-lookup"><span data-stu-id="b2582-127">For example, if the query involves an ORDER BY across partitions, then results from individual partitions are merge-sorted to return results in globally sorted order.</span></span> <span data-ttu-id="b2582-128">Om frågan är en samling som `COUNT`, antal från enskilda partitioner summeras för att skapa det totala antalet.</span><span class="sxs-lookup"><span data-stu-id="b2582-128">If the query is an aggregation like `COUNT`, the counts from individual partitions are summed to produce the overall count.</span></span>

<span data-ttu-id="b2582-129">SDK: erna tillhandahåller olika alternativ för frågekörning.</span><span class="sxs-lookup"><span data-stu-id="b2582-129">The SDKs provide various options for query execution.</span></span> <span data-ttu-id="b2582-130">Till exempel i .NET dessa alternativ är tillgängliga i den `FeedOptions` klass.</span><span class="sxs-lookup"><span data-stu-id="b2582-130">For example, in .NET these options are available in the `FeedOptions` class.</span></span> <span data-ttu-id="b2582-131">I följande tabell beskrivs dessa alternativ och hur de påverkar körningstid för frågan.</span><span class="sxs-lookup"><span data-stu-id="b2582-131">The following table describes these options and how they impact query execution time.</span></span> 

| <span data-ttu-id="b2582-132">Alternativ</span><span class="sxs-lookup"><span data-stu-id="b2582-132">Option</span></span> | <span data-ttu-id="b2582-133">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="b2582-133">Description</span></span> |
| ------ | ----------- |
| `EnableCrossPartitionQuery` | <span data-ttu-id="b2582-134">Måste anges till true för en fråga som måste köras på mer än en partition.</span><span class="sxs-lookup"><span data-stu-id="b2582-134">Must be set to true for any query that requires to be executed across more than one partition.</span></span> <span data-ttu-id="b2582-135">Det här är en explicit flagga så att du kompromissa medvetna prestanda under utveckling.</span><span class="sxs-lookup"><span data-stu-id="b2582-135">This is an explicit flag to enable you to make conscious performance tradeoffs during development time.</span></span> |
| `EnableScanInQuery` | <span data-ttu-id="b2582-136">Måste anges till true om du har valt att inte indexera, men vill ändå kör frågan via en genomsökning.</span><span class="sxs-lookup"><span data-stu-id="b2582-136">Must be set to true if you have opted out of indexing, but want to run the query via a scan anyway.</span></span> <span data-ttu-id="b2582-137">Endast är tillgängligt för indexering för den begärda filtret sökvägen inaktiverad.</span><span class="sxs-lookup"><span data-stu-id="b2582-137">Only applicable if indexing for the requested filter path is disabled.</span></span> | 
| `MaxItemCount` | <span data-ttu-id="b2582-138">Maximalt antal objekt som ska returneras per tur och RETUR till servern.</span><span class="sxs-lookup"><span data-stu-id="b2582-138">The maximum number of items to return per round trip to the server.</span></span> <span data-ttu-id="b2582-139">Genom att ange-1, kan du låta servern hantera antalet objekt.</span><span class="sxs-lookup"><span data-stu-id="b2582-139">By setting to -1, you can let the server manage the number of items.</span></span> <span data-ttu-id="b2582-140">Eller så kan du sänka detta värde att hämta litet antal objekt per onödig kommunikation.</span><span class="sxs-lookup"><span data-stu-id="b2582-140">Or, you can lower this value to retrieve only a small number of items per round trip.</span></span> 
| `MaxBufferedItemCount` | <span data-ttu-id="b2582-141">Detta är ett alternativ för klientsidan och används för att begränsa minnesförbrukningen när du utför cross-partition ORDER BY.</span><span class="sxs-lookup"><span data-stu-id="b2582-141">This is a client-side option, and used to limit the memory consumption when performing cross-partition ORDER BY.</span></span> <span data-ttu-id="b2582-142">Ett högre värde hjälper till att minska svarstiden för cross-partition sortering.</span><span class="sxs-lookup"><span data-stu-id="b2582-142">A higher value helps reduce the latency of cross-partition sorting.</span></span> |
| `MaxDegreeOfParallelism` | <span data-ttu-id="b2582-143">Hämtar eller anger antalet samtidiga åtgärder som körs på klientsidan under parallell frågekörning i tjänsten Azure DocumentDB-databasen.</span><span class="sxs-lookup"><span data-stu-id="b2582-143">Gets or sets the number of concurrent operations run client side during parallel query execution in the Azure DocumentDB database service.</span></span> <span data-ttu-id="b2582-144">Ett positivt egenskapsvärde begränsar antalet samtidiga åtgärder att ange värdet.</span><span class="sxs-lookup"><span data-stu-id="b2582-144">A positive property value limits the number of concurrent operations to the set value.</span></span> <span data-ttu-id="b2582-145">Om det är mindre än 0, bestämmer systemet automatiskt antalet samtidiga åtgärder att köra.</span><span class="sxs-lookup"><span data-stu-id="b2582-145">If it is set to less than 0, the system automatically decides the number of concurrent operations to run.</span></span> |
| `PopulateQueryMetrics` | <span data-ttu-id="b2582-146">Aktiverar detaljerad loggning av statistik tid som ägnats åt olika faser i Frågekörningen som kompileringstid, indexet loop tid och dokument laddas gång.</span><span class="sxs-lookup"><span data-stu-id="b2582-146">Enables detailed logging of statistics of time spent in various phases of query execution like compilation time, index loop time, and document load time.</span></span> <span data-ttu-id="b2582-147">Utdata från frågan statistik kan du dela med Azure-supporten att diagnostisera prestandaproblem för frågan.</span><span class="sxs-lookup"><span data-stu-id="b2582-147">You can share output from query statistics with Azure Support to diagnose query performance issues.</span></span> |
| `RequestContinuation` | <span data-ttu-id="b2582-148">Du kan återuppta körning av fråga genom att passera i täckande fortsättningstoken som returnerades av en fråga.</span><span class="sxs-lookup"><span data-stu-id="b2582-148">You can resume query execution by passing in the opaque continuation token returned by any query.</span></span> <span data-ttu-id="b2582-149">Fortsättningstoken kapslar in alla tillstånd som krävs för frågekörning.</span><span class="sxs-lookup"><span data-stu-id="b2582-149">The continuation token encapsulates all state required for query execution.</span></span> |
| `ResponseContinuationTokenLimitInKb` | <span data-ttu-id="b2582-150">Du kan begränsa den maximala storleken för fortsättningstoken som returnerades av servern.</span><span class="sxs-lookup"><span data-stu-id="b2582-150">You can limit the maximum size of the continuation token returned by the server.</span></span> <span data-ttu-id="b2582-151">Du kan behöva ange detta om din programvärden har gränser för huvudstorlek.</span><span class="sxs-lookup"><span data-stu-id="b2582-151">You might need to set this if your application host has limits on response header size.</span></span> <span data-ttu-id="b2582-152">Anger det här kan öka den övergripande varaktighet och RUs som används för frågan.</span><span class="sxs-lookup"><span data-stu-id="b2582-152">Setting this may increase the overall duration and RUs consumed for the query.</span></span>  |

<span data-ttu-id="b2582-153">Till exempel ta en exempelfråga på partitionsnyckel som efterfrågas på en samling med `/city` som partition nyckel och etablerats med 100 000 RU/s genomströmning.</span><span class="sxs-lookup"><span data-stu-id="b2582-153">For example, let's take an example query on partition key requested on a collection with `/city` as the partition key and provisioned with 100,000 RU/s of throughput.</span></span> <span data-ttu-id="b2582-154">Du begär den här frågan med `CreateDocumentQuery<T>` i .NET på följande:</span><span class="sxs-lookup"><span data-stu-id="b2582-154">You request this query using `CreateDocumentQuery<T>` in .NET like the following:</span></span>

```cs
IDocumentQuery<dynamic> query = client.CreateDocumentQuery(
    UriFactory.CreateDocumentCollectionUri(DatabaseName, CollectionName), 
    "SELECT * FROM c WHERE c.city = 'Seattle'", 
    new FeedOptions 
    { 
        PopulateQueryMetrics = true, 
        MaxItemCount = -1, 
        MaxDegreeOfParallelism = -1, 
        EnableCrossPartitionQuery = true 
    }).AsDocumentQuery();

FeedResponse<dynamic> result = await query.ExecuteNextAsync();
```

<span data-ttu-id="b2582-155">SDK-fragment ovan motsvarar följande REST API-begäran:</span><span class="sxs-lookup"><span data-stu-id="b2582-155">The SDK snippet shown above, corresponds to the following REST API request:</span></span>

```
POST https://arramacquerymetrics-westus.documents.azure.com/dbs/db/colls/sample/docs HTTP/1.1
x-ms-continuation: 
x-ms-documentdb-isquery: True
x-ms-max-item-count: -1
x-ms-documentdb-query-enablecrosspartition: True
x-ms-documentdb-query-parallelizecrosspartitionquery: True
x-ms-documentdb-query-iscontinuationexpected: True
x-ms-documentdb-populatequerymetrics: True
x-ms-date: Tue, 27 Jun 2017 21:52:18 GMT
authorization: type%3dmaster%26ver%3d1.0%26sig%3drp1Hi83Y8aVV5V6LzZ6xhtQVXRAMz0WNMnUuvriUv%2b4%3d
x-ms-session-token: 7:8,6:2008,5:8,4:2008,3:8,2:2008,1:8,0:8,9:8,8:4008
Cache-Control: no-cache
x-ms-consistency-level: Session
User-Agent: documentdb-dotnet-sdk/1.14.1 Host/32-bit MicrosoftWindowsNT/6.2.9200.0
x-ms-version: 2017-02-22
Accept: application/json
Content-Type: application/query+json
Host: arramacquerymetrics-westus.documents.azure.com
Content-Length: 52
Expect: 100-continue

{"query":"SELECT * FROM c WHERE c.city = 'Seattle'"}
```

<span data-ttu-id="b2582-156">Varje sida för körning av frågan som motsvarar en REST-API `POST` med den `Accept: application/query+json` sidhuvud och SQL-frågan i brödtexten.</span><span class="sxs-lookup"><span data-stu-id="b2582-156">Each query execution page corresponds to a REST API `POST` with the `Accept: application/query+json` header, and the SQL query in the body.</span></span> <span data-ttu-id="b2582-157">Varje fråga gör en eller flera förfrågningar till servern med den `x-ms-continuation` token eko mellan klienten och servern för att återuppta körning.</span><span class="sxs-lookup"><span data-stu-id="b2582-157">Each query makes one or more round trips to the server with the `x-ms-continuation` token echoed between the client and server to resume execution.</span></span> <span data-ttu-id="b2582-158">Konfigurationsalternativen i FeedOptions skickas till servern i form av huvuden för begäran.</span><span class="sxs-lookup"><span data-stu-id="b2582-158">The configuration options in FeedOptions are passed to the server in the form of request headers.</span></span> <span data-ttu-id="b2582-159">Till exempel `MaxItemCount` motsvarar `x-ms-max-item-count`.</span><span class="sxs-lookup"><span data-stu-id="b2582-159">For example, `MaxItemCount` corresponds to `x-ms-max-item-count`.</span></span> 

<span data-ttu-id="b2582-160">Begäran returnerar följande (trunkerad för läsbarhet) svar:</span><span class="sxs-lookup"><span data-stu-id="b2582-160">The request returns the following (truncated for readability) response:</span></span>

```
HTTP/1.1 200 Ok
Cache-Control: no-store, no-cache
Pragma: no-cache
Transfer-Encoding: chunked
Content-Type: application/json
Server: Microsoft-HTTPAPI/2.0
Strict-Transport-Security: max-age=31536000
x-ms-last-state-change-utc: Tue, 27 Jun 2017 21:01:57.561 GMT
x-ms-resource-quota: documentSize=10240;documentsSize=10485760;documentsCount=-1;collectionSize=10485760;
x-ms-resource-usage: documentSize=1;documentsSize=884;documentsCount=2000;collectionSize=1408;
x-ms-item-count: 2000
x-ms-schemaversion: 1.3
x-ms-alt-content-path: dbs/db/colls/sample
x-ms-content-path: +9kEANVq0wA=
x-ms-xp-role: 1
x-ms-documentdb-query-metrics: totalExecutionTimeInMs=33.67;queryCompileTimeInMs=0.06;queryLogicalPlanBuildTimeInMs=0.02;queryPhysicalPlanBuildTimeInMs=0.10;queryOptimizationTimeInMs=0.00;VMExecutionTimeInMs=32.56;indexLookupTimeInMs=0.36;documentLoadTimeInMs=9.58;systemFunctionExecuteTimeInMs=0.00;userFunctionExecuteTimeInMs=0.00;retrievedDocumentCount=2000;retrievedDocumentSize=1125600;outputDocumentCount=2000;writeOutputTimeInMs=18.10;indexUtilizationRatio=1.00
x-ms-request-charge: 604.42
x-ms-serviceversion: version=1.14.34.4
x-ms-activity-id: 0df8b5f6-83b9-4493-abda-cce6d0f91486
x-ms-session-token: 2:2008
x-ms-gatewayversion: version=1.14.33.2
Date: Tue, 27 Jun 2017 21:59:49 GMT
```

<span data-ttu-id="b2582-161">Nyckel-svarshuvuden som returnerades från frågan inkluderar följande:</span><span class="sxs-lookup"><span data-stu-id="b2582-161">The key response headers returned from the query include the following:</span></span>

| <span data-ttu-id="b2582-162">Alternativ</span><span class="sxs-lookup"><span data-stu-id="b2582-162">Option</span></span> | <span data-ttu-id="b2582-163">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="b2582-163">Description</span></span> |
| ------ | ----------- |
| `x-ms-item-count` | <span data-ttu-id="b2582-164">Antal objekt som returneras i svaret.</span><span class="sxs-lookup"><span data-stu-id="b2582-164">The number of items returned in the response.</span></span> <span data-ttu-id="b2582-165">Detta är beroende av den angivna `x-ms-max-item-count`, antalet objekt som får plats i nyttolasten för Maximal svarsstorlek, dataflöde och körningstid för frågan.</span><span class="sxs-lookup"><span data-stu-id="b2582-165">This is dependent on the supplied `x-ms-max-item-count`, the number of items that can be fit within the maximum response payload size, the provisioned throughput, and query execution time.</span></span> |  
| `x-ms-continuation:` | <span data-ttu-id="b2582-166">Fortsättningstoken att återuppta körning av frågan, om ytterligare resultat är tillgängliga.</span><span class="sxs-lookup"><span data-stu-id="b2582-166">The continuation token to resume execution of the query, if additional results are available.</span></span> | 
| `x-ms-documentdb-query-metrics` | <span data-ttu-id="b2582-167">Frågan statistik för körning.</span><span class="sxs-lookup"><span data-stu-id="b2582-167">The query statistics for the execution.</span></span> <span data-ttu-id="b2582-168">Det här är en avgränsad sträng som innehåller statistik över tid i olika faser i frågan.</span><span class="sxs-lookup"><span data-stu-id="b2582-168">This is a delimited string containing statistics of time spent in the various phases of query execution.</span></span> <span data-ttu-id="b2582-169">Returneras om `x-ms-documentdb-populatequerymetrics` är inställd på `True`.</span><span class="sxs-lookup"><span data-stu-id="b2582-169">Returned if `x-ms-documentdb-populatequerymetrics` is set to `True`.</span></span> | 
| `x-ms-request-charge` | <span data-ttu-id="b2582-170">Antalet [programbegäran](request-units.md) förbrukas av frågan.</span><span class="sxs-lookup"><span data-stu-id="b2582-170">The number of [request units](request-units.md) consumed by the query.</span></span> | 

<span data-ttu-id="b2582-171">Mer information om REST API-huvuden för begäran och alternativ finns [förfrågan efter resurser med hjälp av REST-API DocumentDB](https://docs.microsoft.com/rest/api/documentdb/querying-documentdb-resources-using-the-rest-api).</span><span class="sxs-lookup"><span data-stu-id="b2582-171">For details on the REST API request headers and options, see [Querying resources using the DocumentDB REST API](https://docs.microsoft.com/rest/api/documentdb/querying-documentdb-resources-using-the-rest-api).</span></span>

## <a name="best-practices-for-query-performance"></a><span data-ttu-id="b2582-172">Bästa praxis för prestanda för frågor</span><span class="sxs-lookup"><span data-stu-id="b2582-172">Best practices for query performance</span></span>
<span data-ttu-id="b2582-173">Följande är de vanligaste faktorer som påverkar Azure Cosmos DB frågeprestanda.</span><span class="sxs-lookup"><span data-stu-id="b2582-173">The following are the most common factors that impact Azure Cosmos DB query performance.</span></span> <span data-ttu-id="b2582-174">Vi gräva djupare i var och en av dessa avsnitt i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="b2582-174">We dig deeper into each of these topics in this article.</span></span>

| <span data-ttu-id="b2582-175">Faktor</span><span class="sxs-lookup"><span data-stu-id="b2582-175">Factor</span></span> | <span data-ttu-id="b2582-176">Tips</span><span class="sxs-lookup"><span data-stu-id="b2582-176">Tip</span></span> | 
| ------ | -----| 
| <span data-ttu-id="b2582-177">Etablerat dataflöde</span><span class="sxs-lookup"><span data-stu-id="b2582-177">Provisioned throughput</span></span> | <span data-ttu-id="b2582-178">Mät RU per fråga och se till att du har det tillhandahållna dataflödet som krävs för dina frågor.</span><span class="sxs-lookup"><span data-stu-id="b2582-178">Measure RU per query, and ensure that you have the required provisioned throughput for your queries.</span></span> | 
| <span data-ttu-id="b2582-179">Partitionering och partitionsnycklar</span><span class="sxs-lookup"><span data-stu-id="b2582-179">Partitioning and partition keys</span></span> | <span data-ttu-id="b2582-180">Ge företräde åt frågor med partitionsnyckelvärde i filterinstruktionen för låg fördröjning.</span><span class="sxs-lookup"><span data-stu-id="b2582-180">Favor queries with the partition key value in the filter clause for low latency.</span></span> |
| <span data-ttu-id="b2582-181">SDK-och frågealternativ</span><span class="sxs-lookup"><span data-stu-id="b2582-181">SDK and query options</span></span> | <span data-ttu-id="b2582-182">Följ Metodtips för SDK som direkt anslutning och finjustera frågealternativ körning på klientsidan.</span><span class="sxs-lookup"><span data-stu-id="b2582-182">Follow SDK best practices like direct connectivity, and tune client-side query execution options.</span></span> |
| <span data-ttu-id="b2582-183">Svarstid för nätverk</span><span class="sxs-lookup"><span data-stu-id="b2582-183">Network latency</span></span> | <span data-ttu-id="b2582-184">Kontot för nätverk omkostnader i mått och använda flera API: er för att läsa från den närmaste regionen.</span><span class="sxs-lookup"><span data-stu-id="b2582-184">Account for network overhead in measurement, and use multi-homing APIs to read from the nearest region.</span></span> |
| <span data-ttu-id="b2582-185">Indexprincip</span><span class="sxs-lookup"><span data-stu-id="b2582-185">Indexing Policy</span></span> | <span data-ttu-id="b2582-186">Se till att du har den nödvändiga sökvägar/indexprincip för frågan.</span><span class="sxs-lookup"><span data-stu-id="b2582-186">Ensure that you have the required indexing paths/policy for the query.</span></span> |
| <span data-ttu-id="b2582-187">Mätvärden för körning av frågan</span><span class="sxs-lookup"><span data-stu-id="b2582-187">Query execution metrics</span></span> | <span data-ttu-id="b2582-188">Analysera de mått för körning av frågan för att identifiera potentiella omskrivningar av frågor och former.</span><span class="sxs-lookup"><span data-stu-id="b2582-188">Analyze the query execution metrics to identify potential rewrites of query and data shapes.</span></span>  |

### <a name="provisioned-throughput"></a><span data-ttu-id="b2582-189">Etablerat dataflöde</span><span class="sxs-lookup"><span data-stu-id="b2582-189">Provisioned throughput</span></span>
<span data-ttu-id="b2582-190">Skapa behållare av data med reserverat dataflöde, uttryckt i begäran enheter (RU) per sekund i Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="b2582-190">In Cosmos DB, you create containers of data, each with reserved throughput expressed in request units (RU) per-second.</span></span> <span data-ttu-id="b2582-191">En läsning av ett dokument på 1 KB är 1 RU och varje åtgärd (inklusive frågor) är normaliserat till ett fast antal RUs baserat på dess komplexitet.</span><span class="sxs-lookup"><span data-stu-id="b2582-191">A read of a 1-KB document is 1 RU, and every operation (including queries) is normalized to a fixed number of RUs based on its complexity.</span></span> <span data-ttu-id="b2582-192">Till exempel om du har 1000 RU/s som skapats för din behållare och du har en fråga som `SELECT * FROM c WHERE c.city = 'Seattle'` som förbrukar 5 RUs och du kan utföra (1000 RU/s) / (5 RU/query) = 200 frågan/s sådana frågor per sekund.</span><span class="sxs-lookup"><span data-stu-id="b2582-192">For example, if you have 1000 RU/s provisioned for your container, and you have a query like `SELECT * FROM c WHERE c.city = 'Seattle'` that consumes 5 RUs, then you can perform (1000 RU/s) / (5 RU/query) = 200 query/s such queries per second.</span></span> 

<span data-ttu-id="b2582-193">Om du skickar fler än 200 per sekund startas tjänsten hastighetsbegränsande inkommande begäranden över 200/s.</span><span class="sxs-lookup"><span data-stu-id="b2582-193">If you submit more than 200 queries/sec, the service starts rate-limiting incoming requests above 200/s.</span></span> <span data-ttu-id="b2582-194">SDK: erna hanterar automatiskt det här fallet genom att utföra en backoff/försök, därför ser du kanske en högre latens för dessa frågor.</span><span class="sxs-lookup"><span data-stu-id="b2582-194">The SDKs automatically handle this case by performing a backoff/retry, therefore you might notice a higher latency for these queries.</span></span> <span data-ttu-id="b2582-195">Öka dataflöde till det obligatoriska värdet förbättrar dina svarstid och genomströmning.</span><span class="sxs-lookup"><span data-stu-id="b2582-195">Increasing the provisioned throughput to the required value improves your query latency and throughput.</span></span> 

<span data-ttu-id="b2582-196">Mer information om frågeenheter finns [programbegäran](request-units.md).</span><span class="sxs-lookup"><span data-stu-id="b2582-196">To learn more about request units, see [Request units](request-units.md).</span></span>

### <a name="partitioning-and-partition-keys"></a><span data-ttu-id="b2582-197">Partitionering och partitionsnycklar</span><span class="sxs-lookup"><span data-stu-id="b2582-197">Partitioning and partition keys</span></span>
<span data-ttu-id="b2582-198">Med Azure Cosmos DB brukar utföra frågor i följande ordning från snabbaste/mest effektiva till långsammare/mindre effektiva.</span><span class="sxs-lookup"><span data-stu-id="b2582-198">With Azure Cosmos DB, typically queries perform in the following order from fastest/most efficient to slower/less efficient.</span></span> 

* <span data-ttu-id="b2582-199">HÄMTA på en enda partitionsnyckel och objektet nyckel</span><span class="sxs-lookup"><span data-stu-id="b2582-199">GET on a single partition key and item key</span></span>
* <span data-ttu-id="b2582-200">Frågan med en filtersats på en enda partitionsnyckel</span><span class="sxs-lookup"><span data-stu-id="b2582-200">Query with a filter clause on a single partition key</span></span>
* <span data-ttu-id="b2582-201">Fråga utan en likhet eller intervallet filtersats om en egenskap</span><span class="sxs-lookup"><span data-stu-id="b2582-201">Query without an equality or range filter clause on any property</span></span>
* <span data-ttu-id="b2582-202">Fråga utan filter</span><span class="sxs-lookup"><span data-stu-id="b2582-202">Query without filters</span></span>

<span data-ttu-id="b2582-203">Frågor som behöver kontakta alla partitioner måste högre latens och kan använda högre RUs.</span><span class="sxs-lookup"><span data-stu-id="b2582-203">Queries that need to consult all partitions need higher latency, and can consume higher RUs.</span></span> <span data-ttu-id="b2582-204">Eftersom varje partition har automatisk indexering mot alla egenskaper, kan frågan hanteras effektivt från indexet i det här fallet.</span><span class="sxs-lookup"><span data-stu-id="b2582-204">Since each partition has automatic indexing against all properties, the query can be served efficiently from the index in this case.</span></span> <span data-ttu-id="b2582-205">Du kan skapa frågor som sträcker sig över partitioner snabbare genom att använda parallellitet.</span><span class="sxs-lookup"><span data-stu-id="b2582-205">You can make queries that span partitions faster by using the parallelism options.</span></span>

<span data-ttu-id="b2582-206">Mer information om partitionering och partitionsnycklar finns [partitionering i Azure Cosmos DB](partition-data.md).</span><span class="sxs-lookup"><span data-stu-id="b2582-206">To learn more about partitioning and partition keys, see [Partitioning in Azure Cosmos DB](partition-data.md).</span></span>

### <a name="sdk-and-query-options"></a><span data-ttu-id="b2582-207">SDK-och frågealternativ</span><span class="sxs-lookup"><span data-stu-id="b2582-207">SDK and query options</span></span>
<span data-ttu-id="b2582-208">Se [prestandatips](performance-tips.md) och [prestandatester](performance-testing.md) att få bästa möjliga prestanda för klientsidan från Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="b2582-208">See [Performance Tips](performance-tips.md) and [Performance testing](performance-testing.md) for how to get the best client-side performance from Azure Cosmos DB.</span></span> <span data-ttu-id="b2582-209">Detta inkluderar med de senaste SDK: er, konfigurera plattformsspecifika konfigurationer som Standardantal anslutningar, frekvensen för skräpinsamling, samt med lightweight anslutningsalternativ som direkt/TCP.</span><span class="sxs-lookup"><span data-stu-id="b2582-209">This includes using the latest SDKs, configuring platform-specific configurations like default number of connections, frequency of garbage collection, and using lightweight connectivity options like Direct/TCP.</span></span> 


#### <a name="max-item-count"></a><span data-ttu-id="b2582-210">Max antal</span><span class="sxs-lookup"><span data-stu-id="b2582-210">Max Item Count</span></span>
<span data-ttu-id="b2582-211">För frågor med värdet för `MaxItemCount` kan ha en betydande inverkan på Frågetid för slutpunkt till slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="b2582-211">For queries, the value of `MaxItemCount` can have a significant impact on end-to-end query time.</span></span> <span data-ttu-id="b2582-212">Varje onödig kommunikation till servern returnerar inga fler än antalet objekt i `MaxItemCount` (standard 100 objekt).</span><span class="sxs-lookup"><span data-stu-id="b2582-212">Each round trip to the server will return no more than the number of items in `MaxItemCount` (Default of 100 items).</span></span> <span data-ttu-id="b2582-213">Ange detta till ett högre värde (-1 är högsta och rekommenderade) förbättrar din övergripande varaktighet för frågan genom att begränsa antalet sändningar mellan servern och klienten, särskilt för frågor med stora resultatuppsättningar.</span><span class="sxs-lookup"><span data-stu-id="b2582-213">Setting this to a higher value (-1 is maximum, and recommended) will improve your query duration overall by limiting the number of round trips between server and client, especially for queries with large result sets.</span></span>

```cs
IDocumentQuery<dynamic> query = client.CreateDocumentQuery(
    UriFactory.CreateDocumentCollectionUri(DatabaseName, CollectionName), 
    "SELECT * FROM c WHERE c.city = 'Seattle'", 
    new FeedOptions 
    { 
        MaxItemCount = -1, 
    }).AsDocumentQuery();
```

#### <a name="max-degree-of-parallelism"></a><span data-ttu-id="b2582-214">Max grad av parallellitet</span><span class="sxs-lookup"><span data-stu-id="b2582-214">Max Degree of Parallelism</span></span>
<span data-ttu-id="b2582-215">Frågor, finjustera den `MaxDegreeOfParallelism` att identifiera de bästa konfigurationerna för programmet, särskilt om du utför mellan partition-frågor (utan ett filter på partitionsnyckel värdet).</span><span class="sxs-lookup"><span data-stu-id="b2582-215">For queries, tune the `MaxDegreeOfParallelism` to identify the best configurations for your application, especially if you perform cross-partition queries (without a filter on the partition-key value).</span></span> <span data-ttu-id="b2582-216">`MaxDegreeOfParallelism`Anger det maximala antalet parallella aktiviteter, t.ex. maximalt antalet partitioner som ska användas parallellt.</span><span class="sxs-lookup"><span data-stu-id="b2582-216">`MaxDegreeOfParallelism`  controls the maximum number of parallel tasks, i.e., the maximum of partitions to be visited in parallel.</span></span> 

```cs
IDocumentQuery<dynamic> query = client.CreateDocumentQuery(
    UriFactory.CreateDocumentCollectionUri(DatabaseName, CollectionName), 
    "SELECT * FROM c WHERE c.city = 'Seattle'", 
    new FeedOptions 
    { 
        MaxDegreeOfParallelism = -1, 
        EnableCrossPartitionQuery = true 
    }).AsDocumentQuery();
```

<span data-ttu-id="b2582-217">Vi antar som</span><span class="sxs-lookup"><span data-stu-id="b2582-217">Let’s assume that</span></span>
* <span data-ttu-id="b2582-218">D = standard maximalt antal parallella aktiviteter (= totalt antal processorn i klientdatorn)</span><span class="sxs-lookup"><span data-stu-id="b2582-218">D = Default Maximum number of parallel tasks (= total number of processor in the client machine)</span></span>
* <span data-ttu-id="b2582-219">P = användaren har angett maximalt antal parallella aktiviteter</span><span class="sxs-lookup"><span data-stu-id="b2582-219">P = User-specified maximum number of parallel tasks</span></span>
* <span data-ttu-id="b2582-220">N = antalet partitioner som ska användas för att svara på en fråga</span><span class="sxs-lookup"><span data-stu-id="b2582-220">N = Number of partitions that needs  to be visited for answering a query</span></span>

<span data-ttu-id="b2582-221">Följande är effekterna av hur de parallella frågorna skulle fungera för olika värden för P.</span><span class="sxs-lookup"><span data-stu-id="b2582-221">Following are implications of how the parallel queries would behave for different values of P.</span></span>
* <span data-ttu-id="b2582-222">(P == 0) = > Serial-läge</span><span class="sxs-lookup"><span data-stu-id="b2582-222">(P == 0) => Serial Mode</span></span>
* <span data-ttu-id="b2582-223">(P == 1) = > maximalt en uppgift</span><span class="sxs-lookup"><span data-stu-id="b2582-223">(P == 1) => Maximum of one task</span></span>
* <span data-ttu-id="b2582-224">(P > 1) = > Min (P, N) parallella aktiviteter</span><span class="sxs-lookup"><span data-stu-id="b2582-224">(P > 1) => Min (P, N) parallel tasks</span></span> 
* <span data-ttu-id="b2582-225">(P < 1) = > Min (N, D) parallella aktiviteter</span><span class="sxs-lookup"><span data-stu-id="b2582-225">(P < 1) => Min (N, D) parallel tasks</span></span>

<span data-ttu-id="b2582-226">För SDK viktig information och information om implementerade klasser och metoder finns [DocumentDB-SDK](documentdb-sdk-dotnet.md)</span><span class="sxs-lookup"><span data-stu-id="b2582-226">For SDK release notes, and details on implemented classes and methods see [DocumentDB SDKs](documentdb-sdk-dotnet.md)</span></span>

### <a name="network-latency"></a><span data-ttu-id="b2582-227">Svarstid för nätverk</span><span class="sxs-lookup"><span data-stu-id="b2582-227">Network latency</span></span>
<span data-ttu-id="b2582-228">Se [Azure Cosmos DB global distributionsplatsen](tutorial-global-distribution-documentdb.md) att konfigurera distributionslistor och ansluta till den närmaste regionen.</span><span class="sxs-lookup"><span data-stu-id="b2582-228">See [Azure Cosmos DB global distribution](tutorial-global-distribution-documentdb.md) for how to set up global distribution, and connect to the closest region.</span></span> <span data-ttu-id="b2582-229">Nätverksfördröjningen har en betydande inverkan på prestanda när du behöver göra flera turer eller hämta en stor resultatmängd från frågan.</span><span class="sxs-lookup"><span data-stu-id="b2582-229">Network latency has a significant impact on query performance when you need to make multiple round-trips or retrieve a large result set from the query.</span></span> 

<span data-ttu-id="b2582-230">Avsnitt om frågan körning mått förklarar hur du hämta server körningstiden för frågor ( `totalExecutionTimeInMs`), så att du kan skilja mellan tid i Frågekörningen och tid i nätverket överföring.</span><span class="sxs-lookup"><span data-stu-id="b2582-230">The section on query execution metrics explains how to retrieve the server execution time of queries ( `totalExecutionTimeInMs`), so that you can differentiate between time spent in query execution and time spent in network transit.</span></span>

### <a name="indexing-policy"></a><span data-ttu-id="b2582-231">Indexprincip</span><span class="sxs-lookup"><span data-stu-id="b2582-231">Indexing policy</span></span>
<span data-ttu-id="b2582-232">Se [konfigurera indexprincip](indexing-policies.md) för indexering sökvägar, typer, och lägen och hur de påverkar Frågekörningen.</span><span class="sxs-lookup"><span data-stu-id="b2582-232">See [Configuring indexing policy](indexing-policies.md) for indexing paths, kinds, and modes, and how they impact query execution.</span></span> <span data-ttu-id="b2582-233">Som standard indexprincip använder hash-indexering för strängar, vilket är effektiv för likhetsfrågor, men inte för intervall frågor/order by-frågor.</span><span class="sxs-lookup"><span data-stu-id="b2582-233">By default, the indexing policy uses Hash indexing for strings, which is effective for equality queries, but not for range queries/order by queries.</span></span> <span data-ttu-id="b2582-234">Om du behöver intervallet frågor efter strängar, rekommenderar vi att ange intervallet index för alla strängar.</span><span class="sxs-lookup"><span data-stu-id="b2582-234">If you need range queries for strings, we recommend specifying the Range index type for all strings.</span></span> 

## <a name="query-execution-metrics"></a><span data-ttu-id="b2582-235">Mätvärden för körning av frågan</span><span class="sxs-lookup"><span data-stu-id="b2582-235">Query execution metrics</span></span>
<span data-ttu-id="b2582-236">Du kan få detaljerad mått på körning av fråga om den valfria `x-ms-documentdb-populatequerymetrics` huvud (`FeedOptions.PopulateQueryMetrics` i .NET SDK).</span><span class="sxs-lookup"><span data-stu-id="b2582-236">You can obtain detailed metrics on query execution by passing in the optional `x-ms-documentdb-populatequerymetrics` header (`FeedOptions.PopulateQueryMetrics` in the .NET SDK).</span></span> <span data-ttu-id="b2582-237">Det värde som returneras i `x-ms-documentdb-query-metrics` har följande nyckel-värdepar avsett för avancerad felsökning av Frågekörningen.</span><span class="sxs-lookup"><span data-stu-id="b2582-237">The value returned in `x-ms-documentdb-query-metrics` has the following key-value pairs meant for advanced troubleshooting of query execution.</span></span> 

```cs
IDocumentQuery<dynamic> query = client.CreateDocumentQuery(
    UriFactory.CreateDocumentCollectionUri(DatabaseName, CollectionName), 
    "SELECT * FROM c WHERE c.city = 'Seattle'", 
    new FeedOptions 
    { 
        PopulateQueryMetrics = true, 
    }).AsDocumentQuery();

FeedResponse<dynamic> result = await query.ExecuteNextAsync();

// Returns metrics by partition key range Id
IReadOnlyDictionary<string, QueryMetrics> metrics = result.QueryMetrics;

```

| <span data-ttu-id="b2582-238">Mått</span><span class="sxs-lookup"><span data-stu-id="b2582-238">Metric</span></span> | <span data-ttu-id="b2582-239">Enhet</span><span class="sxs-lookup"><span data-stu-id="b2582-239">Unit</span></span> | <span data-ttu-id="b2582-240">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="b2582-240">Description</span></span> | 
| ------ | -----| ----------- |
| `totalExecutionTimeInMs` | <span data-ttu-id="b2582-241">millisekunder</span><span class="sxs-lookup"><span data-stu-id="b2582-241">milliseconds</span></span> | <span data-ttu-id="b2582-242">Körningstid för frågan</span><span class="sxs-lookup"><span data-stu-id="b2582-242">Query execution time</span></span> | 
| `queryCompileTimeInMs` | <span data-ttu-id="b2582-243">millisekunder</span><span class="sxs-lookup"><span data-stu-id="b2582-243">milliseconds</span></span> | <span data-ttu-id="b2582-244">Frågan kompilering</span><span class="sxs-lookup"><span data-stu-id="b2582-244">Query compile time</span></span>  | 
| `queryLogicalPlanBuildTimeInMs` | <span data-ttu-id="b2582-245">millisekunder</span><span class="sxs-lookup"><span data-stu-id="b2582-245">milliseconds</span></span> | <span data-ttu-id="b2582-246">Tid att skapa logiska frågeplan</span><span class="sxs-lookup"><span data-stu-id="b2582-246">Time to build logical query plan</span></span> | 
| `queryPhysicalPlanBuildTimeInMs` | <span data-ttu-id="b2582-247">millisekunder</span><span class="sxs-lookup"><span data-stu-id="b2582-247">milliseconds</span></span> | <span data-ttu-id="b2582-248">Tid att skapa fysisk frågeplan</span><span class="sxs-lookup"><span data-stu-id="b2582-248">Time to build physical query plan</span></span> | 
| `queryOptimizationTimeInMs` | <span data-ttu-id="b2582-249">millisekunder</span><span class="sxs-lookup"><span data-stu-id="b2582-249">milliseconds</span></span> | <span data-ttu-id="b2582-250">Tid som ägnats åt att optimera frågan</span><span class="sxs-lookup"><span data-stu-id="b2582-250">Time spent in optimizing query</span></span> | 
| `VMExecutionTimeInMs` | <span data-ttu-id="b2582-251">millisekunder</span><span class="sxs-lookup"><span data-stu-id="b2582-251">milliseconds</span></span> | <span data-ttu-id="b2582-252">Tid i frågan runtime</span><span class="sxs-lookup"><span data-stu-id="b2582-252">Time spent in query runtime</span></span> | 
| `indexLookupTimeInMs` | <span data-ttu-id="b2582-253">millisekunder</span><span class="sxs-lookup"><span data-stu-id="b2582-253">milliseconds</span></span> | <span data-ttu-id="b2582-254">Tid i fysiska index lager</span><span class="sxs-lookup"><span data-stu-id="b2582-254">Time spent in physical index layer</span></span> | 
| `documentLoadTimeInMs` | <span data-ttu-id="b2582-255">millisekunder</span><span class="sxs-lookup"><span data-stu-id="b2582-255">milliseconds</span></span> | <span data-ttu-id="b2582-256">Tid i inläsning av dokument</span><span class="sxs-lookup"><span data-stu-id="b2582-256">Time spent in loading documents</span></span>  | 
| `systemFunctionExecuteTimeInMs` | <span data-ttu-id="b2582-257">millisekunder</span><span class="sxs-lookup"><span data-stu-id="b2582-257">milliseconds</span></span> | <span data-ttu-id="b2582-258">Total tid som ägnats verkställande system (inbyggda) funktioner i millisekunder</span><span class="sxs-lookup"><span data-stu-id="b2582-258">Total time spent executing system (built-in) functions in milliseconds</span></span>  | 
| `userFunctionExecuteTimeInMs` | <span data-ttu-id="b2582-259">millisekunder</span><span class="sxs-lookup"><span data-stu-id="b2582-259">milliseconds</span></span> | <span data-ttu-id="b2582-260">Total tid som ägnats verkställande användardefinierade funktioner i millisekunder</span><span class="sxs-lookup"><span data-stu-id="b2582-260">Total time spent executing user-defined functions in milliseconds</span></span> | 
| `retrievedDocumentCount` | <span data-ttu-id="b2582-261">millisekunder</span><span class="sxs-lookup"><span data-stu-id="b2582-261">milliseconds</span></span> | <span data-ttu-id="b2582-262">Totalt antal hämtade dokument</span><span class="sxs-lookup"><span data-stu-id="b2582-262">Total number of retrieved documents</span></span>  | 
| `retrievedDocumentSize` | <span data-ttu-id="b2582-263">Byte</span><span class="sxs-lookup"><span data-stu-id="b2582-263">bytes</span></span> | <span data-ttu-id="b2582-264">Total storlek på hämtade dokument i byte</span><span class="sxs-lookup"><span data-stu-id="b2582-264">Total size of retrieved documents in bytes</span></span>  | 
| `outputDocumentCount` | <span data-ttu-id="b2582-265">Antal</span><span class="sxs-lookup"><span data-stu-id="b2582-265">count</span></span> | <span data-ttu-id="b2582-266">Antal dokument som utdata</span><span class="sxs-lookup"><span data-stu-id="b2582-266">Number of output documents</span></span> | 
| `writeOutputTimeInMs` | <span data-ttu-id="b2582-267">millisekunder</span><span class="sxs-lookup"><span data-stu-id="b2582-267">milliseconds</span></span> | <span data-ttu-id="b2582-268">Tid i millisekunder för att köra frågan</span><span class="sxs-lookup"><span data-stu-id="b2582-268">Query execution time in milliseconds</span></span> | 
| `indexUtilizationRatio` | <span data-ttu-id="b2582-269">förhållandet mellan (< = 1)</span><span class="sxs-lookup"><span data-stu-id="b2582-269">ratio (<=1)</span></span> | <span data-ttu-id="b2582-270">Förhållandet mellan antalet dokument som matchar filtret till antal dokument som läses in</span><span class="sxs-lookup"><span data-stu-id="b2582-270">Ratio of number of documents matched by the filter to the number of documents loaded</span></span>  | 

<span data-ttu-id="b2582-271">Klient-SDK: internt göra flera frågeåtgärder att utföra frågan inom varje partition.</span><span class="sxs-lookup"><span data-stu-id="b2582-271">The client SDKs may internally make multiple query operations to serve the query within each partition.</span></span> <span data-ttu-id="b2582-272">Klienten gör flera anrop per partition om totalt resultat överskrider `x-ms-max-item-count`, om frågan överskrider det tillhandahållna dataflödet för partition, eller om frågan nyttolasten når maximal storlek per sida, eller om frågan når systemets allokerade Timeout-gränsen.</span><span class="sxs-lookup"><span data-stu-id="b2582-272">The client makes more than one call per-partition if the total results exceed `x-ms-max-item-count`, if the query exceeds the provisioned throughput for the partition, or if the query payload reaches the maximum size per page, or if the query reaches the system allocated timeout limit.</span></span> <span data-ttu-id="b2582-273">Varje partiella Frågekörningen returnerar en `x-ms-documentdb-query-metrics` för sidan.</span><span class="sxs-lookup"><span data-stu-id="b2582-273">Each partial query execution returns a `x-ms-documentdb-query-metrics` for that page.</span></span> 

<span data-ttu-id="b2582-274">Här är några exempelfrågor och hur du tolkar vissa av mätvärdena som returnerades från Frågekörningen:</span><span class="sxs-lookup"><span data-stu-id="b2582-274">Here are some sample queries, and how to interpret some of the metrics returned from query execution:</span></span> 

| <span data-ttu-id="b2582-275">Fråga</span><span class="sxs-lookup"><span data-stu-id="b2582-275">Query</span></span> | <span data-ttu-id="b2582-276">Exempel mått</span><span class="sxs-lookup"><span data-stu-id="b2582-276">Sample Metric</span></span> | <span data-ttu-id="b2582-277">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="b2582-277">Description</span></span> | 
| ------ | -----| ----------- |
| `SELECT TOP 100 * FROM c` | `"RetrievedDocumentCount": 101` | <span data-ttu-id="b2582-278">Antal dokument som hämtats är 100 + 1 för att matcha TOP-instruktion.</span><span class="sxs-lookup"><span data-stu-id="b2582-278">The number of documents retrieved is 100+1 to match the TOP clause.</span></span> <span data-ttu-id="b2582-279">Frågetid används främst i `WriteOutputTime` och `DocumentLoadTime` eftersom det är en genomsökning.</span><span class="sxs-lookup"><span data-stu-id="b2582-279">Query time is mostly spent in `WriteOutputTime` and `DocumentLoadTime` since it is a scan.</span></span> | 
| `SELECT TOP 500 * FROM c` | `"RetrievedDocumentCount": 501` | <span data-ttu-id="b2582-280">RetrievedDocumentCount är nu högre (500 + 1 för att matcha TOP-instruktion).</span><span class="sxs-lookup"><span data-stu-id="b2582-280">RetrievedDocumentCount is now higher (500+1 to match the TOP clause).</span></span> | 
| `SELECT * FROM c WHERE c.N = 55` | `"IndexLookupTime": "00:00:00.0009500"` | <span data-ttu-id="b2582-281">Om 0,9 ms används i IndexLookupTime för en nyckel sökning, eftersom det är ett index sökning `/N/?`.</span><span class="sxs-lookup"><span data-stu-id="b2582-281">About 0.9 ms is spent in IndexLookupTime for a key lookup, because it's an index lookup on `/N/?`.</span></span> | 
| `SELECT * FROM c WHERE c.N > 55` | `"IndexLookupTime": "00:00:00.0017700"` | <span data-ttu-id="b2582-282">Något mer tid (1,7 ms) som används IndexLookupTime via en genomsökning för intervallet, eftersom det är ett index sökning `/N/?`.</span><span class="sxs-lookup"><span data-stu-id="b2582-282">Slightly more time (1.7 ms) spent in IndexLookupTime over a range scan, because it's an index lookup on `/N/?`.</span></span> | 
| `SELECT TOP 500 c.N FROM c` | `"IndexLookupTime": "00:00:00.0017700"` | <span data-ttu-id="b2582-283">Samma tid som ägnats `DocumentLoadTime` som tidigare frågor men lägre `WriteOutputTime` eftersom vi projicerar bara en egenskap.</span><span class="sxs-lookup"><span data-stu-id="b2582-283">Same time spent on `DocumentLoadTime` as previous queries, but lower `WriteOutputTime` because we're projecting only one property.</span></span> | 
| `SELECT TOP 500 udf.toPercent(c.N) FROM c` | `"UserDefinedFunctionExecutionTime": "00:00:00.2136500"` | <span data-ttu-id="b2582-284">Om 213 ms ägnats åt `UserDefinedFunctionExecutionTime` köra en användardefinierad funktion på varje värde i `c.N`.</span><span class="sxs-lookup"><span data-stu-id="b2582-284">About 213 ms is spent in `UserDefinedFunctionExecutionTime` executing the UDF on each value of `c.N`.</span></span> |
| `SELECT TOP 500 c.Name FROM c WHERE STARTSWITH(c.Name, 'Den')` | `"IndexLookupTime": "00:00:00.0006400", "SystemFunctionExecutionTime": "00:00:00.0074100"` | <span data-ttu-id="b2582-285">Om 0,6 ms ägnats åt `IndexLookupTime` på `/Name/?`.</span><span class="sxs-lookup"><span data-stu-id="b2582-285">About 0.6 ms is spent in `IndexLookupTime` on `/Name/?`.</span></span> <span data-ttu-id="b2582-286">De flesta av frågan körningstid (ms ~ 7) i `SystemFunctionExecutionTime`.</span><span class="sxs-lookup"><span data-stu-id="b2582-286">Most of the query execution time (~7 ms) in `SystemFunctionExecutionTime`.</span></span> |
| `SELECT TOP 500 c.Name FROM c WHERE STARTSWITH(LOWER(c.Name), 'den')` | `"IndexLookupTime": "00:00:00", "RetrievedDocumentCount": 2491,  "OutputDocumentCount": 500` | <span data-ttu-id="b2582-287">Frågan utförs som en genomsökning eftersom den använder `LOWER`, och 500 utanför 2491 hämtade dokument som returneras.</span><span class="sxs-lookup"><span data-stu-id="b2582-287">Query is performed as a scan because it uses `LOWER`, and 500 out of 2491 retrieved documents are returned.</span></span> |


## <a name="next-steps"></a><span data-ttu-id="b2582-288">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="b2582-288">Next steps</span></span>
* <span data-ttu-id="b2582-289">Läs om stöds SQL frågeoperatorer och nyckelord i [SQL-frågan](documentdb-sql-query.md).</span><span class="sxs-lookup"><span data-stu-id="b2582-289">To learn about the supported SQL query operators and keywords, see [SQL query](documentdb-sql-query.md).</span></span> 
* <span data-ttu-id="b2582-290">Mer information om frågeenheter, se [programbegäran](request-units.md).</span><span class="sxs-lookup"><span data-stu-id="b2582-290">To learn about request units, see [request units](request-units.md).</span></span>
* <span data-ttu-id="b2582-291">Läs om indexprincip i [indexering princip](indexing-policies.md)</span><span class="sxs-lookup"><span data-stu-id="b2582-291">To learn about indexing policy, see [indexing policy](indexing-policies.md)</span></span> 


