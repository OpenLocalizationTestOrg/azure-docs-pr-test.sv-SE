---
title: aaaAzure Cosmos DB Node.js API SDK & resurser | Microsoft Docs
description: "Läs mer om hello Node.js API och SDK inklusive frisläppningsdatum, tillbakadragning datum och ändringar mellan varje version av hello Azure Cosmos DB Node.js SDK."
services: cosmos-db
documentationcenter: nodejs
author: rnagpal
manager: jhubbard
editor: cgronlun
ms.assetid: 9d5621fa-0e11-4619-a28b-a19d872bcf37
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: article
ms.date: 08/14/2017
ms.author: rnagpal
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: d450b9a9ea7b0f4717ddae8940121fc458ea3744
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-cosmos-db-nodejs-sdk-release-notes-and-resources"></a><span data-ttu-id="af70b-103">Azure Cosmos DB Node.js SDK: Viktig information och resurser</span><span class="sxs-lookup"><span data-stu-id="af70b-103">Azure Cosmos DB Node.js SDK: Release notes and resources</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="af70b-104">.NET</span><span class="sxs-lookup"><span data-stu-id="af70b-104">.NET</span></span>](documentdb-sdk-dotnet.md)
> * [<span data-ttu-id="af70b-105">.NET ändra Feed</span><span class="sxs-lookup"><span data-stu-id="af70b-105">.NET Change Feed</span></span>](documentdb-sdk-dotnet-changefeed.md)
> * [<span data-ttu-id="af70b-106">.NET Core</span><span class="sxs-lookup"><span data-stu-id="af70b-106">.NET Core</span></span>](documentdb-sdk-dotnet-core.md)
> * [<span data-ttu-id="af70b-107">Node.js</span><span class="sxs-lookup"><span data-stu-id="af70b-107">Node.js</span></span>](documentdb-sdk-node.md)
> * [<span data-ttu-id="af70b-108">Java</span><span class="sxs-lookup"><span data-stu-id="af70b-108">Java</span></span>](documentdb-sdk-java.md)
> * [<span data-ttu-id="af70b-109">Python</span><span class="sxs-lookup"><span data-stu-id="af70b-109">Python</span></span>](documentdb-sdk-python.md)
> * [<span data-ttu-id="af70b-110">REST</span><span class="sxs-lookup"><span data-stu-id="af70b-110">REST</span></span>](https://docs.microsoft.com/rest/api/documentdb/)
> * [<span data-ttu-id="af70b-111">REST-resursprovider</span><span class="sxs-lookup"><span data-stu-id="af70b-111">REST Resource Provider</span></span>](https://docs.microsoft.com/rest/api/documentdbresourceprovider/)
> * [<span data-ttu-id="af70b-112">SQL</span><span class="sxs-lookup"><span data-stu-id="af70b-112">SQL</span></span>](https://msdn.microsoft.com/library/azure/dn782250.aspx)
> 
> 

<table>

<tr><td><span data-ttu-id="af70b-113">**Ladda ned SDK**</span><span class="sxs-lookup"><span data-stu-id="af70b-113">**Download SDK**</span></span></td><td>[<span data-ttu-id="af70b-114">NPM</span><span class="sxs-lookup"><span data-stu-id="af70b-114">NPM</span></span>](https://www.npmjs.com/package/documentdb)</td></tr>

<tr><td><span data-ttu-id="af70b-115">**API-dokumentationen**</span><span class="sxs-lookup"><span data-stu-id="af70b-115">**API documentation**</span></span></td><td>[<span data-ttu-id="af70b-116">Node.js API-referensdokumentation</span><span class="sxs-lookup"><span data-stu-id="af70b-116">Node.js API reference documentation</span></span>](http://azure.github.io/azure-documentdb-node/DocumentClient.html)</td></tr>

<tr><td><span data-ttu-id="af70b-117">**Installationsinstruktioner för SDK**</span><span class="sxs-lookup"><span data-stu-id="af70b-117">**SDK installation instructions**</span></span></td><td>[<span data-ttu-id="af70b-118">Instruktioner för installation</span><span class="sxs-lookup"><span data-stu-id="af70b-118">Installation instructions</span></span>](http://azure.github.io/azure-documentdb-node/)</td></tr>

<tr><td><span data-ttu-id="af70b-119">**Bidra tooSDK**</span><span class="sxs-lookup"><span data-stu-id="af70b-119">**Contribute tooSDK**</span></span></td><td>[<span data-ttu-id="af70b-120">GitHub</span><span class="sxs-lookup"><span data-stu-id="af70b-120">GitHub</span></span>](https://github.com/Azure/azure-documentdb-node/tree/master/source)</td></tr>

<tr><td><span data-ttu-id="af70b-121">**Exempel**</span><span class="sxs-lookup"><span data-stu-id="af70b-121">**Samples**</span></span></td><td>[<span data-ttu-id="af70b-122">Node.js-kodexempel</span><span class="sxs-lookup"><span data-stu-id="af70b-122">Node.js code samples</span></span>](documentdb-nodejs-samples.md)</td></tr>

<tr><td><span data-ttu-id="af70b-123">**Självstudier för att komma igång**</span><span class="sxs-lookup"><span data-stu-id="af70b-123">**Get started tutorial**</span></span></td><td>[<span data-ttu-id="af70b-124">Kom igång med hello Node.js SDK</span><span class="sxs-lookup"><span data-stu-id="af70b-124">Get started with hello Node.js SDK</span></span>](documentdb-nodejs-get-started.md)</td></tr>

<tr><td><span data-ttu-id="af70b-125">**Självstudier för Web app**</span><span class="sxs-lookup"><span data-stu-id="af70b-125">**Web app tutorial**</span></span></td><td>[<span data-ttu-id="af70b-126">Skapa en Node.js-webbapp med Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="af70b-126">Build a Node.js web application using Azure Cosmos DB</span></span>](documentdb-nodejs-application.md)</td></tr>

<tr><td><span data-ttu-id="af70b-127">**Aktuella plattform som stöds**</span><span class="sxs-lookup"><span data-stu-id="af70b-127">**Current supported platform**</span></span></td><td> 
[<span data-ttu-id="af70b-128">Node.js v6.x</span><span class="sxs-lookup"><span data-stu-id="af70b-128">Node.js v6.x</span></span>](https://nodejs.org/en/blog/release/v6.10.3/)<br/> 
[<span data-ttu-id="af70b-129">Node.js v4.2.0</span><span class="sxs-lookup"><span data-stu-id="af70b-129">Node.js v4.2.0</span></span>](https://nodejs.org/en/blog/release/v4.2.0/)<br/> 
[<span data-ttu-id="af70b-130">Node.js v0.12</span><span class="sxs-lookup"><span data-stu-id="af70b-130">Node.js v0.12</span></span>](https://nodejs.org/en/blog/release/v0.12.0/)<br/> 
<span data-ttu-id="af70b-131">[Node.js v0.10](https://nodejs.org/en/blog/release/v0.10.0/) 
</span><span class="sxs-lookup"><span data-stu-id="af70b-131">[Node.js v0.10](https://nodejs.org/en/blog/release/v0.10.0/) 
</span></span></td></tr>
</table></br>

## <a name="release-notes"></a><span data-ttu-id="af70b-132">Viktig information</span><span class="sxs-lookup"><span data-stu-id="af70b-132">Release notes</span></span>

### <span data-ttu-id="af70b-133"><a name="1.12.2"/>1.12.2</a></span><span class="sxs-lookup"><span data-stu-id="af70b-133"><a name="1.12.2"/>1.12.2</a></span></span>
*   <span data-ttu-id="af70b-134">fast npm-dokumentationen.</span><span class="sxs-lookup"><span data-stu-id="af70b-134">npm documentation fixed.</span></span>

### <span data-ttu-id="af70b-135"><a name="1.12.1"/>1.12.1</a></span><span class="sxs-lookup"><span data-stu-id="af70b-135"><a name="1.12.1"/>1.12.1</a></span></span>
* <span data-ttu-id="af70b-136">Fast ett programfel i executeStoredProcedure där dokument för hade särskilda Unicode-tecken (LS PS).</span><span class="sxs-lookup"><span data-stu-id="af70b-136">Fixed a bug in executeStoredProcedure where documents involved had special Unicode characters (LS, PS).</span></span>
* <span data-ttu-id="af70b-137">Fast ett programfel i dokument med Unicode-tecken i hello partitionsnyckel-hantering.</span><span class="sxs-lookup"><span data-stu-id="af70b-137">Fixed a bug in handling documents with Unicode characters in hello partition key.</span></span>
* <span data-ttu-id="af70b-138">Fast stöd för att skapa samlingar med hello namn media.</span><span class="sxs-lookup"><span data-stu-id="af70b-138">Fixed support for creating collections with hello name media.</span></span> <span data-ttu-id="af70b-139">Github problemet #114.</span><span class="sxs-lookup"><span data-stu-id="af70b-139">Github issue #114.</span></span>
* <span data-ttu-id="af70b-140">Fast stöd för behörighet autentiseringstoken.</span><span class="sxs-lookup"><span data-stu-id="af70b-140">Fixed support for permission authorization token.</span></span> <span data-ttu-id="af70b-141">Github problemet #178.</span><span class="sxs-lookup"><span data-stu-id="af70b-141">Github issue #178.</span></span>

### <span data-ttu-id="af70b-142"><a name="1.12.0"/>1.12.0</a></span><span class="sxs-lookup"><span data-stu-id="af70b-142"><a name="1.12.0"/>1.12.0</a></span></span>
* <span data-ttu-id="af70b-143">Tillagt stöd för en ny [konsekvensnivå](consistency-levels.md) kallas ConsistentPrefix.</span><span class="sxs-lookup"><span data-stu-id="af70b-143">Added support for a new [consistency level](consistency-levels.md) called ConsistentPrefix.</span></span>
* <span data-ttu-id="af70b-144">Stöd för UriFactory har lagts till.</span><span class="sxs-lookup"><span data-stu-id="af70b-144">Added support for UriFactory.</span></span>
* <span data-ttu-id="af70b-145">Fast ett programfel för Unicode-stöd.</span><span class="sxs-lookup"><span data-stu-id="af70b-145">Fixed a Unicode support bug.</span></span> <span data-ttu-id="af70b-146">GitHub problemet #171.</span><span class="sxs-lookup"><span data-stu-id="af70b-146">GitHub issue #171.</span></span>

### <span data-ttu-id="af70b-147"><a name="1.11.0"/>1.11.0</a></span><span class="sxs-lookup"><span data-stu-id="af70b-147"><a name="1.11.0"/>1.11.0</a></span></span>
* <span data-ttu-id="af70b-148">Tillagda hello stöd för aggregering frågor (COUNT, MIN, MAX, SUM och AVG).</span><span class="sxs-lookup"><span data-stu-id="af70b-148">Added hello support for aggregation queries (COUNT, MIN, MAX, SUM, and AVG).</span></span>
* <span data-ttu-id="af70b-149">Tillagda hello alternativ för att styra graden av parallellitet för mellan partition frågor.</span><span class="sxs-lookup"><span data-stu-id="af70b-149">Added hello option for controlling degree of parallelism for cross partition queries.</span></span>
* <span data-ttu-id="af70b-150">Tillagda hello alternativet för att inaktivera SSL-kontroll när du kör mot Azure Cosmos DB-emulatorn.</span><span class="sxs-lookup"><span data-stu-id="af70b-150">Added hello option for disabling SSL verification when running against Azure Cosmos DB Emulator.</span></span>
* <span data-ttu-id="af70b-151">Sänks minsta dataflöde på partitionerade samlingar från 10,100 RU/s too2500 RU/s.</span><span class="sxs-lookup"><span data-stu-id="af70b-151">Lowered minimum throughput on partitioned collections from 10,100 RU/s too2500 RU/s.</span></span>
* <span data-ttu-id="af70b-152">Fast hello fortsättning token programfel för enskild partition samling.</span><span class="sxs-lookup"><span data-stu-id="af70b-152">Fixed hello continuation token bug for single partition collection.</span></span> <span data-ttu-id="af70b-153">Github problemet #107.</span><span class="sxs-lookup"><span data-stu-id="af70b-153">Github issue #107.</span></span>
* <span data-ttu-id="af70b-154">Fast hello executeStoredProcedure programfel i hantering 0 som enda param.</span><span class="sxs-lookup"><span data-stu-id="af70b-154">Fixed hello executeStoredProcedure bug in handling 0 as single param.</span></span> <span data-ttu-id="af70b-155">Github problemet #155.</span><span class="sxs-lookup"><span data-stu-id="af70b-155">Github issue #155.</span></span>

### <span data-ttu-id="af70b-156"><a name="1.10.2"/>1.10.2</a></span><span class="sxs-lookup"><span data-stu-id="af70b-156"><a name="1.10.2"/>1.10.2</a></span></span>
* <span data-ttu-id="af70b-157">Fast användaragent huvud tooinclude hello SDK-version.</span><span class="sxs-lookup"><span data-stu-id="af70b-157">Fixed user-agent header tooinclude hello SDK version.</span></span>
* <span data-ttu-id="af70b-158">Rensningen av mindre kod.</span><span class="sxs-lookup"><span data-stu-id="af70b-158">Minor code cleanup.</span></span>

### <span data-ttu-id="af70b-159"><a name="1.10.1"/>1.10.1</a></span><span class="sxs-lookup"><span data-stu-id="af70b-159"><a name="1.10.1"/>1.10.1</a></span></span>
* <span data-ttu-id="af70b-160">Inaktivera SSL-kontroll när du använder hello SDK tootarget hello emulator(hostname=localhost).</span><span class="sxs-lookup"><span data-stu-id="af70b-160">Disabling SSL verification when using hello SDK tootarget hello emulator(hostname=localhost).</span></span>
* <span data-ttu-id="af70b-161">Stöd har lagts till för att aktivera loggning skript under körning av lagrad procedur.</span><span class="sxs-lookup"><span data-stu-id="af70b-161">Added support for enabling script logging during stored procedure execution.</span></span>

### <span data-ttu-id="af70b-162"><a name="1.10.0"/>1.10.0</a></span><span class="sxs-lookup"><span data-stu-id="af70b-162"><a name="1.10.0"/>1.10.0</a></span></span>
* <span data-ttu-id="af70b-163">Tillagt stöd för mellan partition parallella frågor.</span><span class="sxs-lookup"><span data-stu-id="af70b-163">Added support for cross partition parallel queries.</span></span>
* <span data-ttu-id="af70b-164">Stöd för upp/ORDER BY-frågor för partitionerade samlingar har lagts till.</span><span class="sxs-lookup"><span data-stu-id="af70b-164">Added support for TOP/ORDER BY queries for partitioned collections.</span></span>

### <span data-ttu-id="af70b-165"><a name="1.9.0"/>1.9.0</a></span><span class="sxs-lookup"><span data-stu-id="af70b-165"><a name="1.9.0"/>1.9.0</a></span></span>
* <span data-ttu-id="af70b-166">Tillagda försök princip stöd för begränsad begäranden.</span><span class="sxs-lookup"><span data-stu-id="af70b-166">Added retry policy support for throttled requests.</span></span> <span data-ttu-id="af70b-167">(Begränsad begäranden får en förfrågan hastighet för stor undantag, felkod 429.) Standard Azure Cosmos DB återförsök nio gånger för varje begäran när felkoden 429 påträffas respektera hello retryAfter tid i hello-Svarsrubrik.</span><span class="sxs-lookup"><span data-stu-id="af70b-167">(Throttled requests receive a request rate too large exception, error code 429.) By default, Azure Cosmos DB retries nine times for each request when error code 429 is encountered, honoring hello retryAfter time in hello response header.</span></span> <span data-ttu-id="af70b-168">En fast försök tidsintervall kan nu anges som en del av hello RetryOptions egenskapen på hello ConnectionPolicy objekt om du vill tooignore hello retryAfter tid mellan hello försök som returnerades av servern.</span><span class="sxs-lookup"><span data-stu-id="af70b-168">A fixed retry interval time can now be set as part of hello RetryOptions property on hello ConnectionPolicy object if you want tooignore hello retryAfter time returned by server between hello retries.</span></span> <span data-ttu-id="af70b-169">Azure Cosmos-DB väntar nu högst 30 sekunder för varje begäran som har begränsats (oavsett antal försök) och returnerar hello svar med felkoden 429.</span><span class="sxs-lookup"><span data-stu-id="af70b-169">Azure Cosmos DB now waits for a maximum of 30 seconds for each request that is being throttled (irrespective of retry count) and returns hello response with error code 429.</span></span> <span data-ttu-id="af70b-170">Nu kan åsidosättas i hello RetryOptions egenskapen i ConnectionPolicy-objektet.</span><span class="sxs-lookup"><span data-stu-id="af70b-170">This time can also be overridden in hello RetryOptions property on ConnectionPolicy object.</span></span>
* <span data-ttu-id="af70b-171">Cosmos DB Returnerar nu x-ms-begränsning--antal försök och x-ms-throttle-retry-wait-time-ms som hello svarshuvuden i varje begäran toodenote hello begränsning försök antal och hello kumulativa hello begära väntat mellan hello försök.</span><span class="sxs-lookup"><span data-stu-id="af70b-171">Cosmos DB now returns x-ms-throttle-retry-count and x-ms-throttle-retry-wait-time-ms as hello response headers in every request toodenote hello throttle retry count and hello cumulative time hello request waited between hello retries.</span></span>
* <span data-ttu-id="af70b-172">Hej RetryOptions klass har lagts till exponera hello RetryOptions egenskapen på hello ConnectionPolicy klass som kan vara används toooverride vissa hello standard alternativen för återförsök.</span><span class="sxs-lookup"><span data-stu-id="af70b-172">hello RetryOptions class was added, exposing hello RetryOptions property on hello ConnectionPolicy class that can be used toooverride some of hello default retry options.</span></span>

### <span data-ttu-id="af70b-173"><a name="1.8.0"/>1.8.0</a></span><span class="sxs-lookup"><span data-stu-id="af70b-173"><a name="1.8.0"/>1.8.0</a></span></span>
* <span data-ttu-id="af70b-174">Tillagda hello stöd för flera regioner databasen konton.</span><span class="sxs-lookup"><span data-stu-id="af70b-174">Added hello support for multi-region database accounts.</span></span>

### <span data-ttu-id="af70b-175"><a name="1.7.0"/>1.7.0</a></span><span class="sxs-lookup"><span data-stu-id="af70b-175"><a name="1.7.0"/>1.7.0</a></span></span>
* <span data-ttu-id="af70b-176">Tillagda hello stöd för tid tooLive(TTL) funktionen för dokument.</span><span class="sxs-lookup"><span data-stu-id="af70b-176">Added hello support for Time tooLive(TTL) feature for documents.</span></span>

### <span data-ttu-id="af70b-177"><a name="1.6.0"/>1.6.0</a></span><span class="sxs-lookup"><span data-stu-id="af70b-177"><a name="1.6.0"/>1.6.0</a></span></span>
* <span data-ttu-id="af70b-178">Implementerad [partitionerade samlingar](partition-data.md) och [användardefinierade prestandanivåer](performance-levels.md).</span><span class="sxs-lookup"><span data-stu-id="af70b-178">Implemented [partitioned collections](partition-data.md) and [user-defined performance levels](performance-levels.md).</span></span>

### <span data-ttu-id="af70b-179"><a name="1.5.6"/>1.5.6</a></span><span class="sxs-lookup"><span data-stu-id="af70b-179"><a name="1.5.6"/>1.5.6</a></span></span>
* <span data-ttu-id="af70b-180">Fast RangePartitionResolver.resolveForRead bugg där den inte returnerade länkar på grund av tooa felaktiga concat av resultaten.</span><span class="sxs-lookup"><span data-stu-id="af70b-180">Fixed RangePartitionResolver.resolveForRead bug where it was not returning links due tooa bad concat of results.</span></span>

### <span data-ttu-id="af70b-181"><a name="1.5.5"/>1.5.5</a></span><span class="sxs-lookup"><span data-stu-id="af70b-181"><a name="1.5.5"/>1.5.5</a></span></span>
* <span data-ttu-id="af70b-182">Fast hashParitionResolver resolveForRead(): när inga partitionsnyckel angavs att undantag, i stället för att returnera en lista över alla registrerade länkar.</span><span class="sxs-lookup"><span data-stu-id="af70b-182">Fixed hashParitionResolver resolveForRead(): When no partition key supplied was throwing exception, instead of returning a list of all registered links.</span></span>

### <span data-ttu-id="af70b-183"><a name="1.5.4"/>1.5.4</a></span><span class="sxs-lookup"><span data-stu-id="af70b-183"><a name="1.5.4"/>1.5.4</a></span></span>
* <span data-ttu-id="af70b-184">För att rätta till problemet [#100](https://github.com/Azure/azure-documentdb-node/issues/100) -dedikerad HTTPS Agent: undvika att ändra hello globala agent för Azure Cosmos DB syften.</span><span class="sxs-lookup"><span data-stu-id="af70b-184">Fixes issue [#100](https://github.com/Azure/azure-documentdb-node/issues/100) - Dedicated HTTPS Agent: Avoid modifying hello global agent for Azure Cosmos DB purposes.</span></span> <span data-ttu-id="af70b-185">Använd en dedikerad agent för alla hello lib begäranden.</span><span class="sxs-lookup"><span data-stu-id="af70b-185">Use a dedicated agent for all of hello lib’s requests.</span></span>

### <span data-ttu-id="af70b-186"><a name="1.5.3"/>1.5.3</a></span><span class="sxs-lookup"><span data-stu-id="af70b-186"><a name="1.5.3"/>1.5.3</a></span></span>
* <span data-ttu-id="af70b-187">För att rätta till problemet [#81](https://github.com/Azure/azure-documentdb-node/issues/81) - korrekt hantera bindestreck i media-ID: n.</span><span class="sxs-lookup"><span data-stu-id="af70b-187">Fixes issue [#81](https://github.com/Azure/azure-documentdb-node/issues/81) - Properly handle dashes in media ids.</span></span>

### <span data-ttu-id="af70b-188"><a name="1.5.2"/>1.5.2</a></span><span class="sxs-lookup"><span data-stu-id="af70b-188"><a name="1.5.2"/>1.5.2</a></span></span>
* <span data-ttu-id="af70b-189">För att rätta till problemet [#95](https://github.com/Azure/azure-documentdb-node/issues/95) -EventEmitter lyssnare läcka varning.</span><span class="sxs-lookup"><span data-stu-id="af70b-189">Fixes issue [#95](https://github.com/Azure/azure-documentdb-node/issues/95) - EventEmitter listener leak warning.</span></span>

### <span data-ttu-id="af70b-190"><a name="1.5.1"/>1.5.1</a></span><span class="sxs-lookup"><span data-stu-id="af70b-190"><a name="1.5.1"/>1.5.1</a></span></span>
* <span data-ttu-id="af70b-191">För att rätta till problemet [#92](https://github.com/Azure/azure-documentdb-node/issues/90) -Byt namn på mappen Hash toohash för skiftlägeskänsligt system.</span><span class="sxs-lookup"><span data-stu-id="af70b-191">Fixes issue [#92](https://github.com/Azure/azure-documentdb-node/issues/90) - rename folder Hash toohash for case-sensitive systems.</span></span>

### <span data-ttu-id="af70b-192"><a name="1.5.0"/>1.5.0</a></span><span class="sxs-lookup"><span data-stu-id="af70b-192"><a name="1.5.0"/>1.5.0</a></span></span>
* <span data-ttu-id="af70b-193">Implementera stöd för horisontell partitionering genom att lägga till intervallet & hash-matchare för partitionen.</span><span class="sxs-lookup"><span data-stu-id="af70b-193">Implement sharding support by adding hash & range partition resolvers.</span></span>

### <span data-ttu-id="af70b-194"><a name="1.4.0"/>1.4.0</a></span><span class="sxs-lookup"><span data-stu-id="af70b-194"><a name="1.4.0"/>1.4.0</a></span></span>
* <span data-ttu-id="af70b-195">Implementera Upsert.</span><span class="sxs-lookup"><span data-stu-id="af70b-195">Implement Upsert.</span></span> <span data-ttu-id="af70b-196">Nya upsertXXX-metoder på documentClient.</span><span class="sxs-lookup"><span data-stu-id="af70b-196">New upsertXXX methods on documentClient.</span></span>

### <span data-ttu-id="af70b-197"><a name="1.3.0"/>1.3.0</a></span><span class="sxs-lookup"><span data-stu-id="af70b-197"><a name="1.3.0"/>1.3.0</a></span></span>
* <span data-ttu-id="af70b-198">Hoppas över toobring versionsnummer i enlighet med andra SDK: er.</span><span class="sxs-lookup"><span data-stu-id="af70b-198">Skipped toobring version numbers in alignment with other SDKs.</span></span>

### <span data-ttu-id="af70b-199"><a name="1.2.2"/>1.2.2</a></span><span class="sxs-lookup"><span data-stu-id="af70b-199"><a name="1.2.2"/>1.2.2</a></span></span>
* <span data-ttu-id="af70b-200">Dela frågor lovar wrapper toonew databasen.</span><span class="sxs-lookup"><span data-stu-id="af70b-200">Split Q promises wrapper toonew repository.</span></span>
* <span data-ttu-id="af70b-201">Uppdatera toopackage-filen för npm-registret.</span><span class="sxs-lookup"><span data-stu-id="af70b-201">Update toopackage file for npm registry.</span></span>

### <span data-ttu-id="af70b-202"><a name="1.2.1"/>1.2.1</a></span><span class="sxs-lookup"><span data-stu-id="af70b-202"><a name="1.2.1"/>1.2.1</a></span></span>
* <span data-ttu-id="af70b-203">Implementerar ID baserat routning.</span><span class="sxs-lookup"><span data-stu-id="af70b-203">Implements ID Based Routing.</span></span>
* <span data-ttu-id="af70b-204">För att rätta till problemet [#49](https://github.com/Azure/azure-documentdb-node/issues/49) -aktuella egenskapen står i konflikt med metoden current().</span><span class="sxs-lookup"><span data-stu-id="af70b-204">Fixes Issue [#49](https://github.com/Azure/azure-documentdb-node/issues/49) - current property conflicts with method current().</span></span>

### <span data-ttu-id="af70b-205"><a name="1.2.0"/>1.2.0</a></span><span class="sxs-lookup"><span data-stu-id="af70b-205"><a name="1.2.0"/>1.2.0</a></span></span>
* <span data-ttu-id="af70b-206">Stöd för GeoSpatial indexet har lagts till.</span><span class="sxs-lookup"><span data-stu-id="af70b-206">Added support for GeoSpatial index.</span></span>
* <span data-ttu-id="af70b-207">Verifierar id-egenskapen för alla resurser.</span><span class="sxs-lookup"><span data-stu-id="af70b-207">Validates id property for all resources.</span></span> <span data-ttu-id="af70b-208">ID för resurser kan inte innehålla?, /, #, &#47; &#47; tecken eller sluta med ett blanksteg.</span><span class="sxs-lookup"><span data-stu-id="af70b-208">Ids for resources cannot contain ?, /, #, &#47;&#47;, characters or end with a space.</span></span>
* <span data-ttu-id="af70b-209">Lägger till nya rubriken ”index omvandling pågår” tooResourceResponse.</span><span class="sxs-lookup"><span data-stu-id="af70b-209">Adds new header "index transformation progress" tooResourceResponse.</span></span>

### <span data-ttu-id="af70b-210"><a name="1.1.0"/>1.1.0</a></span><span class="sxs-lookup"><span data-stu-id="af70b-210"><a name="1.1.0"/>1.1.0</a></span></span>
* <span data-ttu-id="af70b-211">Implementerar V2 indexprincip.</span><span class="sxs-lookup"><span data-stu-id="af70b-211">Implements V2 indexing policy.</span></span>

### <span data-ttu-id="af70b-212"><a name="1.0.3"/>1.0.3</a></span><span class="sxs-lookup"><span data-stu-id="af70b-212"><a name="1.0.3"/>1.0.3</a></span></span>
* <span data-ttu-id="af70b-213">Problemet [#40](https://github.com/Azure/azure-documentdb-node/issues/40) - implementerat eslint och grunt konfigurationer i hello kärnor och lovar SDK.</span><span class="sxs-lookup"><span data-stu-id="af70b-213">Issue [#40](https://github.com/Azure/azure-documentdb-node/issues/40) - Implemented eslint and grunt configurations in hello core and promise SDK.</span></span>

### <span data-ttu-id="af70b-214"><a name="1.0.2"/>1.0.2</a></span><span class="sxs-lookup"><span data-stu-id="af70b-214"><a name="1.0.2"/>1.0.2</a></span></span>
* <span data-ttu-id="af70b-215">Problemet [#45](https://github.com/Azure/azure-documentdb-node/issues/45) -löftena wrapper inkluderar inte-rubrik med fel.</span><span class="sxs-lookup"><span data-stu-id="af70b-215">Issue [#45](https://github.com/Azure/azure-documentdb-node/issues/45) - Promises wrapper does not include header with error.</span></span>

### <span data-ttu-id="af70b-216"><a name="1.0.1"/>1.0.1</a></span><span class="sxs-lookup"><span data-stu-id="af70b-216"><a name="1.0.1"/>1.0.1</a></span></span>
* <span data-ttu-id="af70b-217">Implementerad möjlighet tooquery för konflikter genom att lägga till readConflicts och readConflictAsync queryConflicts.</span><span class="sxs-lookup"><span data-stu-id="af70b-217">Implemented ability tooquery for conflicts by adding readConflicts, readConflictAsync, and queryConflicts.</span></span>
* <span data-ttu-id="af70b-218">Uppdaterade API-dokumentationen.</span><span class="sxs-lookup"><span data-stu-id="af70b-218">Updated API documentation.</span></span>
* <span data-ttu-id="af70b-219">Problemet [#41](https://github.com/Azure/azure-documentdb-node/issues/41) -client.createDocumentAsync fel.</span><span class="sxs-lookup"><span data-stu-id="af70b-219">Issue [#41](https://github.com/Azure/azure-documentdb-node/issues/41) - client.createDocumentAsync error.</span></span>

### <span data-ttu-id="af70b-220"><a name="1.0.0"/>1.0.0</a></span><span class="sxs-lookup"><span data-stu-id="af70b-220"><a name="1.0.0"/>1.0.0</a></span></span>
* <span data-ttu-id="af70b-221">GA SDK.</span><span class="sxs-lookup"><span data-stu-id="af70b-221">GA SDK.</span></span>

## <a name="release--retirement-dates"></a><span data-ttu-id="af70b-222">Versionen & pensionering datum</span><span class="sxs-lookup"><span data-stu-id="af70b-222">Release & Retirement Dates</span></span>
<span data-ttu-id="af70b-223">Microsoft meddelar minst **12 månader** innan du tar bort en SDK i ordning toosmooth hello övergången tooa nyare/stöds version.</span><span class="sxs-lookup"><span data-stu-id="af70b-223">Microsoft provides notification at least **12 months** in advance of retiring an SDK in order toosmooth hello transition tooa newer/supported version.</span></span>

<span data-ttu-id="af70b-224">Nya funktioner och funktionalitet och optimeringar läggs endast toohello aktuella SDK, som vi rekommenderar att du alltid uppgradera toohello senaste SDK version så snart som möjligt.</span><span class="sxs-lookup"><span data-stu-id="af70b-224">New features and functionality and optimizations are only added toohello current SDK, as such it is  recommended that you always upgrade toohello latest SDK version as early as possible.</span></span>

<span data-ttu-id="af70b-225">Alla förfrågningar tooCosmos databasen med en pensionerad SDK är avvisas av hello-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="af70b-225">Any request tooCosmos DB using a retired SDK is be rejected by hello service.</span></span>

<br/>

| <span data-ttu-id="af70b-226">Version</span><span class="sxs-lookup"><span data-stu-id="af70b-226">Version</span></span> | <span data-ttu-id="af70b-227">Utgivningsdatum</span><span class="sxs-lookup"><span data-stu-id="af70b-227">Release Date</span></span> | <span data-ttu-id="af70b-228">Datumet för tillbakadragandet</span><span class="sxs-lookup"><span data-stu-id="af70b-228">Retirement Date</span></span> |
| --- | --- | --- |
| [<span data-ttu-id="af70b-229">1.12.2</span><span class="sxs-lookup"><span data-stu-id="af70b-229">1.12.2</span></span>](#1.12.2) |<span data-ttu-id="af70b-230">10 augusti 2017</span><span class="sxs-lookup"><span data-stu-id="af70b-230">August 10, 2017</span></span> |--- |
| [<span data-ttu-id="af70b-231">1.12.1</span><span class="sxs-lookup"><span data-stu-id="af70b-231">1.12.1</span></span>](#1.12.1) |<span data-ttu-id="af70b-232">10 augusti 2017</span><span class="sxs-lookup"><span data-stu-id="af70b-232">August 10, 2017</span></span> |--- |
| [<span data-ttu-id="af70b-233">1.12.0</span><span class="sxs-lookup"><span data-stu-id="af70b-233">1.12.0</span></span>](#1.12.0) |<span data-ttu-id="af70b-234">10 maj 2017</span><span class="sxs-lookup"><span data-stu-id="af70b-234">May 10, 2017</span></span> |--- |
| [<span data-ttu-id="af70b-235">1.11.0</span><span class="sxs-lookup"><span data-stu-id="af70b-235">1.11.0</span></span>](#1.11.0) |<span data-ttu-id="af70b-236">16 mars 2017</span><span class="sxs-lookup"><span data-stu-id="af70b-236">March 16, 2017</span></span> |--- |
| [<span data-ttu-id="af70b-237">1.10.2</span><span class="sxs-lookup"><span data-stu-id="af70b-237">1.10.2</span></span>](#1.10.2) |<span data-ttu-id="af70b-238">27 januari 2017</span><span class="sxs-lookup"><span data-stu-id="af70b-238">January 27, 2017</span></span> |--- |
| [<span data-ttu-id="af70b-239">1.10.1</span><span class="sxs-lookup"><span data-stu-id="af70b-239">1.10.1</span></span>](#1.10.1) |<span data-ttu-id="af70b-240">Den 22 december 2016</span><span class="sxs-lookup"><span data-stu-id="af70b-240">December 22, 2016</span></span> |--- |
| [<span data-ttu-id="af70b-241">1.10.0</span><span class="sxs-lookup"><span data-stu-id="af70b-241">1.10.0</span></span>](#1.10.0) |<span data-ttu-id="af70b-242">03 oktober 2016</span><span class="sxs-lookup"><span data-stu-id="af70b-242">October 03, 2016</span></span> |--- |
| [<span data-ttu-id="af70b-243">1.9.0</span><span class="sxs-lookup"><span data-stu-id="af70b-243">1.9.0</span></span>](#1.9.0) |<span data-ttu-id="af70b-244">07 juli 2016</span><span class="sxs-lookup"><span data-stu-id="af70b-244">July 07, 2016</span></span> |--- |
| [<span data-ttu-id="af70b-245">1.8.0</span><span class="sxs-lookup"><span data-stu-id="af70b-245">1.8.0</span></span>](#1.8.0) |<span data-ttu-id="af70b-246">14 juni 2016</span><span class="sxs-lookup"><span data-stu-id="af70b-246">June 14, 2016</span></span> |--- |
| [<span data-ttu-id="af70b-247">1.7.0</span><span class="sxs-lookup"><span data-stu-id="af70b-247">1.7.0</span></span>](#1.7.0) |<span data-ttu-id="af70b-248">26 april 2016</span><span class="sxs-lookup"><span data-stu-id="af70b-248">April 26, 2016</span></span> |--- |
| [<span data-ttu-id="af70b-249">1.6.0</span><span class="sxs-lookup"><span data-stu-id="af70b-249">1.6.0</span></span>](#1.6.0) |<span data-ttu-id="af70b-250">Den 29 mars 2016</span><span class="sxs-lookup"><span data-stu-id="af70b-250">March 29, 2016</span></span> |--- |
| [<span data-ttu-id="af70b-251">1.5.6</span><span class="sxs-lookup"><span data-stu-id="af70b-251">1.5.6</span></span>](#1.5.6) |<span data-ttu-id="af70b-252">08 mars 2016</span><span class="sxs-lookup"><span data-stu-id="af70b-252">March 08, 2016</span></span> |--- |
| [<span data-ttu-id="af70b-253">1.5.5</span><span class="sxs-lookup"><span data-stu-id="af70b-253">1.5.5</span></span>](#1.5.5) |<span data-ttu-id="af70b-254">02 februari 2016</span><span class="sxs-lookup"><span data-stu-id="af70b-254">February 02, 2016</span></span> |--- |
| [<span data-ttu-id="af70b-255">1.5.4</span><span class="sxs-lookup"><span data-stu-id="af70b-255">1.5.4</span></span>](#1.5.4) |<span data-ttu-id="af70b-256">01 februari 2016</span><span class="sxs-lookup"><span data-stu-id="af70b-256">February 01, 2016</span></span> |--- |
| [<span data-ttu-id="af70b-257">1.5.2</span><span class="sxs-lookup"><span data-stu-id="af70b-257">1.5.2</span></span>](#1.5.2) |<span data-ttu-id="af70b-258">26 januari 2016</span><span class="sxs-lookup"><span data-stu-id="af70b-258">January 26, 2016</span></span> |--- |
| [<span data-ttu-id="af70b-259">1.5.2</span><span class="sxs-lookup"><span data-stu-id="af70b-259">1.5.2</span></span>](#1.5.2) |<span data-ttu-id="af70b-260">Den 22 januari 2016</span><span class="sxs-lookup"><span data-stu-id="af70b-260">January 22, 2016</span></span> |--- |
| [<span data-ttu-id="af70b-261">1.5.1</span><span class="sxs-lookup"><span data-stu-id="af70b-261">1.5.1</span></span>](#1.5.1) |<span data-ttu-id="af70b-262">4 januari 2016</span><span class="sxs-lookup"><span data-stu-id="af70b-262">January 4, 2016</span></span> |--- |
| [<span data-ttu-id="af70b-263">1.5.0</span><span class="sxs-lookup"><span data-stu-id="af70b-263">1.5.0</span></span>](#1.5.0) |<span data-ttu-id="af70b-264">Den 31 december 2015</span><span class="sxs-lookup"><span data-stu-id="af70b-264">December 31, 2015</span></span> |--- |
| [<span data-ttu-id="af70b-265">1.4.0</span><span class="sxs-lookup"><span data-stu-id="af70b-265">1.4.0</span></span>](#1.4.0) |<span data-ttu-id="af70b-266">06 oktober 2015</span><span class="sxs-lookup"><span data-stu-id="af70b-266">October 06, 2015</span></span> |--- |
| [<span data-ttu-id="af70b-267">1.3.0</span><span class="sxs-lookup"><span data-stu-id="af70b-267">1.3.0</span></span>](#1.3.0) |<span data-ttu-id="af70b-268">06 oktober 2015</span><span class="sxs-lookup"><span data-stu-id="af70b-268">October 06, 2015</span></span> |--- |
| [<span data-ttu-id="af70b-269">1.2.2</span><span class="sxs-lookup"><span data-stu-id="af70b-269">1.2.2</span></span>](#1.2.2) |<span data-ttu-id="af70b-270">10 september 2015</span><span class="sxs-lookup"><span data-stu-id="af70b-270">September 10, 2015</span></span> |--- |
| [<span data-ttu-id="af70b-271">1.2.1</span><span class="sxs-lookup"><span data-stu-id="af70b-271">1.2.1</span></span>](#1.2.1) |<span data-ttu-id="af70b-272">15 augusti 2015</span><span class="sxs-lookup"><span data-stu-id="af70b-272">August 15, 2015</span></span> |--- |
| [<span data-ttu-id="af70b-273">1.2.0</span><span class="sxs-lookup"><span data-stu-id="af70b-273">1.2.0</span></span>](#1.2.0) |<span data-ttu-id="af70b-274">05 augusti 2015</span><span class="sxs-lookup"><span data-stu-id="af70b-274">August 05, 2015</span></span> |--- |
| [<span data-ttu-id="af70b-275">1.1.0</span><span class="sxs-lookup"><span data-stu-id="af70b-275">1.1.0</span></span>](#1.1.0) |<span data-ttu-id="af70b-276">09 juli 2015</span><span class="sxs-lookup"><span data-stu-id="af70b-276">July 09, 2015</span></span> |--- |
| [<span data-ttu-id="af70b-277">1.0.3</span><span class="sxs-lookup"><span data-stu-id="af70b-277">1.0.3</span></span>](#1.0.3) |<span data-ttu-id="af70b-278">04 juni 2015</span><span class="sxs-lookup"><span data-stu-id="af70b-278">June 04, 2015</span></span> |--- |
| [<span data-ttu-id="af70b-279">1.0.2</span><span class="sxs-lookup"><span data-stu-id="af70b-279">1.0.2</span></span>](#1.0.2) |<span data-ttu-id="af70b-280">23 maj 2015</span><span class="sxs-lookup"><span data-stu-id="af70b-280">May 23, 2015</span></span> |--- |
| [<span data-ttu-id="af70b-281">1.0.1</span><span class="sxs-lookup"><span data-stu-id="af70b-281">1.0.1</span></span>](#1.0.1) |<span data-ttu-id="af70b-282">Den 15 maj 2015</span><span class="sxs-lookup"><span data-stu-id="af70b-282">May 15, 2015</span></span> |--- |
| [<span data-ttu-id="af70b-283">1.0.0</span><span class="sxs-lookup"><span data-stu-id="af70b-283">1.0.0</span></span>](#1.0.0) |<span data-ttu-id="af70b-284">08 april 2015</span><span class="sxs-lookup"><span data-stu-id="af70b-284">April 08, 2015</span></span> |--- |

## <a name="faq"></a><span data-ttu-id="af70b-285">VANLIGA FRÅGOR OCH SVAR</span><span class="sxs-lookup"><span data-stu-id="af70b-285">FAQ</span></span>
[!INCLUDE [cosmos-db-sdk-faq](../../includes/cosmos-db-sdk-faq.md)]

## <a name="see-also"></a><span data-ttu-id="af70b-286">Se även</span><span class="sxs-lookup"><span data-stu-id="af70b-286">See also</span></span>
<span data-ttu-id="af70b-287">toolearn mer om Cosmos DB finns [Microsoft Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) sida.</span><span class="sxs-lookup"><span data-stu-id="af70b-287">toolearn more about Cosmos DB, see [Microsoft Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) service page.</span></span>

