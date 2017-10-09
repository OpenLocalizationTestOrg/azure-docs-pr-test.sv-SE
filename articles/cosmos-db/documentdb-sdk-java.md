---
title: 'Azure Cosmos DB: DocumentDB Java API, SDK & resurser | Microsoft Docs'
description: "Läs mer om hello Java API och SDK inklusive frisläppningsdatum, tillbakadragning datum och ändringar mellan varje version av hello Azure Cosmos DB DocumentDB Java SDK."
services: cosmos-db
documentationcenter: java
author: rnagpal
manager: jhubbard
editor: cgronlun
ms.assetid: 7861cadf-2a05-471a-9925-0fec0599351b
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: java
ms.topic: article
ms.date: 07/11/2017
ms.author: khdang
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 8ef43ebeb7ae1bfc55512c4a7489c1b7930122d6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-cosmos-db-documentdb-java-sdk-release-notes-and-resources"></a><span data-ttu-id="ed16a-103">Azure Cosmos DB: DocumentDB Java SDK viktig information och resurser</span><span class="sxs-lookup"><span data-stu-id="ed16a-103">Azure Cosmos DB: DocumentDB Java SDK release notes and resources</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="ed16a-104">.NET</span><span class="sxs-lookup"><span data-stu-id="ed16a-104">.NET</span></span>](documentdb-sdk-dotnet.md)
> * [<span data-ttu-id="ed16a-105">.NET ändra Feed</span><span class="sxs-lookup"><span data-stu-id="ed16a-105">.NET Change Feed</span></span>](documentdb-sdk-dotnet-changefeed.md)
> * [<span data-ttu-id="ed16a-106">.NET Core</span><span class="sxs-lookup"><span data-stu-id="ed16a-106">.NET Core</span></span>](documentdb-sdk-dotnet-core.md)
> * [<span data-ttu-id="ed16a-107">Node.js</span><span class="sxs-lookup"><span data-stu-id="ed16a-107">Node.js</span></span>](documentdb-sdk-node.md)
> * [<span data-ttu-id="ed16a-108">Java</span><span class="sxs-lookup"><span data-stu-id="ed16a-108">Java</span></span>](documentdb-sdk-java.md)
> * [<span data-ttu-id="ed16a-109">Python</span><span class="sxs-lookup"><span data-stu-id="ed16a-109">Python</span></span>](documentdb-sdk-python.md)
> * [<span data-ttu-id="ed16a-110">REST</span><span class="sxs-lookup"><span data-stu-id="ed16a-110">REST</span></span>](https://docs.microsoft.com/rest/api/documentdb/)
> * [<span data-ttu-id="ed16a-111">REST-resursprovider</span><span class="sxs-lookup"><span data-stu-id="ed16a-111">REST Resource Provider</span></span>](https://docs.microsoft.com/rest/api/documentdbresourceprovider/)
> * [<span data-ttu-id="ed16a-112">SQL</span><span class="sxs-lookup"><span data-stu-id="ed16a-112">SQL</span></span>](https://msdn.microsoft.com/library/azure/dn782250.aspx)
> 
> 

<table>

<tr><td><span data-ttu-id="ed16a-113">**SDK-hämtningen**</span><span class="sxs-lookup"><span data-stu-id="ed16a-113">**SDK Download**</span></span></td><td>[<span data-ttu-id="ed16a-114">Maven 3.</span><span class="sxs-lookup"><span data-stu-id="ed16a-114">Maven</span></span>](http://search.maven.org/#search%7Cgav%7C1%7Cg%3A%22com.microsoft.azure%22%20AND%20a%3A%22azure-documentdb%22)</td></tr>

<tr><td><span data-ttu-id="ed16a-115">**API-dokumentationen**</span><span class="sxs-lookup"><span data-stu-id="ed16a-115">**API documentation**</span></span></td><td>[<span data-ttu-id="ed16a-116">Java API-referensdokumentation</span><span class="sxs-lookup"><span data-stu-id="ed16a-116">Java API reference documentation</span></span>](/java/api/com.microsoft.azure.documentdb)</td></tr>

<tr><td><span data-ttu-id="ed16a-117">**Bidra tooSDK**</span><span class="sxs-lookup"><span data-stu-id="ed16a-117">**Contribute tooSDK**</span></span></td><td>[<span data-ttu-id="ed16a-118">GitHub</span><span class="sxs-lookup"><span data-stu-id="ed16a-118">GitHub</span></span>](https://github.com/Azure/azure-documentdb-java/)</td></tr>

<tr><td><span data-ttu-id="ed16a-119">**Kom igång**</span><span class="sxs-lookup"><span data-stu-id="ed16a-119">**Get started**</span></span></td><td>[<span data-ttu-id="ed16a-120">Kom igång med hello Java SDK</span><span class="sxs-lookup"><span data-stu-id="ed16a-120">Get started with hello Java SDK</span></span>](documentdb-java-get-started.md)</td></tr>

<tr><td><span data-ttu-id="ed16a-121">**Självstudier för Web app**</span><span class="sxs-lookup"><span data-stu-id="ed16a-121">**Web app tutorial**</span></span></td><td>[<span data-ttu-id="ed16a-122">Utveckling av webbappar med Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="ed16a-122">Web application development with Azure Cosmos DB</span></span>](documentdb-java-application.md)</td></tr>

<tr><td><span data-ttu-id="ed16a-123">**Aktuella stöds runtime**</span><span class="sxs-lookup"><span data-stu-id="ed16a-123">**Current supported runtime**</span></span></td><td>[<span data-ttu-id="ed16a-124">JDK 7</span><span class="sxs-lookup"><span data-stu-id="ed16a-124">JDK 7</span></span>](http://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html)</td></tr>
</table></br>

## <a name="release-notes"></a><span data-ttu-id="ed16a-125">Viktig information</span><span class="sxs-lookup"><span data-stu-id="ed16a-125">Release Notes</span></span>

### <a name="a-name11201120"></a><span data-ttu-id="ed16a-126"><a name="1.12.0"/>1.12.0</span><span class="sxs-lookup"><span data-stu-id="ed16a-126"><a name="1.12.0"/>1.12.0</span></span>
* <span data-ttu-id="ed16a-127">Kritiska felkorrigeringar toorequest bearbetning under partition delar.</span><span class="sxs-lookup"><span data-stu-id="ed16a-127">Critical bug fixes toorequest processing during partition splits.</span></span>
* <span data-ttu-id="ed16a-128">Ett problem har åtgärdats med hello starka och BoundedStaleness konsekvensnivåer.</span><span class="sxs-lookup"><span data-stu-id="ed16a-128">Fixed an issue with hello Strong and BoundedStaleness consistency levels.</span></span>

### <a name="a-name11101110"></a><span data-ttu-id="ed16a-129"><a name="1.11.0"/>1.11.0</span><span class="sxs-lookup"><span data-stu-id="ed16a-129"><a name="1.11.0"/>1.11.0</span></span>
* <span data-ttu-id="ed16a-130">Tillagt stöd för en ny konsekvensnivå kallas ConsistentPrefix.</span><span class="sxs-lookup"><span data-stu-id="ed16a-130">Added support for a new consistency level called ConsistentPrefix.</span></span>
* <span data-ttu-id="ed16a-131">Fast ett fel vid läsning av samlingen i sessionsläge.</span><span class="sxs-lookup"><span data-stu-id="ed16a-131">Fixed a bug in reading collection in session mode.</span></span>

### <a name="a-name11001100"></a><span data-ttu-id="ed16a-132"><a name="1.10.0"/>1.10.0</span><span class="sxs-lookup"><span data-stu-id="ed16a-132"><a name="1.10.0"/>1.10.0</span></span>
* <span data-ttu-id="ed16a-133">Aktiverar stöd för partitionerade samling med som låga som 2 500 RU/s och skala i steg om 100 RU/s.</span><span class="sxs-lookup"><span data-stu-id="ed16a-133">Enabled support for partitioned collection with as low as 2,500 RU/sec and scale in increments of 100 RU/sec.</span></span>
* <span data-ttu-id="ed16a-134">Fast ett programfel i hello interna sammansättning som kan orsaka NullRef undantag i några frågor.</span><span class="sxs-lookup"><span data-stu-id="ed16a-134">Fixed a bug in hello native assembly which can cause NullRef exception in some queries.</span></span>

### <a name="a-name196196"></a><span data-ttu-id="ed16a-135"><a name="1.9.6"/>1.9.6</span><span class="sxs-lookup"><span data-stu-id="ed16a-135"><a name="1.9.6"/>1.9.6</span></span>
* <span data-ttu-id="ed16a-136">Fast ett programfel i hello frågan konfigurationen för motorns som kan orsaka undantag för frågor i Gateway-läge.</span><span class="sxs-lookup"><span data-stu-id="ed16a-136">Fixed a bug in hello query engine configuration that may cause exceptions for queries in Gateway mode.</span></span>
* <span data-ttu-id="ed16a-137">Korrigerat några fel i hello session behållaren som kan orsaka ett ”ägare resursen hittades inte” undantag för begäranden omedelbart efter att samlingen har skapats.</span><span class="sxs-lookup"><span data-stu-id="ed16a-137">Fixed a few bugs in hello session container that may cause an "Owner resource not found" exception for requests immediately after collection creation.</span></span>

### <a name="a-name195195"></a><span data-ttu-id="ed16a-138"><a name="1.9.5"/>1.9.5</span><span class="sxs-lookup"><span data-stu-id="ed16a-138"><a name="1.9.5"/>1.9.5</span></span>
* <span data-ttu-id="ed16a-139">Stöd för aggregering frågor (COUNT, MIN, MAX, SUM och AVG) har lagts till.</span><span class="sxs-lookup"><span data-stu-id="ed16a-139">Added support for aggregation queries (COUNT, MIN, MAX, SUM, and AVG).</span></span> <span data-ttu-id="ed16a-140">Se [aggregering support](documentdb-sql-query.md#Aggregates).</span><span class="sxs-lookup"><span data-stu-id="ed16a-140">See [Aggregation support](documentdb-sql-query.md#Aggregates).</span></span>
* <span data-ttu-id="ed16a-141">Tillagt stöd för ändring feed.</span><span class="sxs-lookup"><span data-stu-id="ed16a-141">Added support for change feed.</span></span>
* <span data-ttu-id="ed16a-142">Stöd för samlingen-kvotinformation via RequestOptions.setPopulateQuotaInfo har lagts till.</span><span class="sxs-lookup"><span data-stu-id="ed16a-142">Added support for collection quota information through RequestOptions.setPopulateQuotaInfo.</span></span>
* <span data-ttu-id="ed16a-143">Stöd för lagrad procedur skript loggning med RequestOptions.setScriptLoggingEnabled har lagts till.</span><span class="sxs-lookup"><span data-stu-id="ed16a-143">Added support for stored procedure script logging through RequestOptions.setScriptLoggingEnabled.</span></span>
* <span data-ttu-id="ed16a-144">Fast en bugg där frågan i DirectHttps läge låser sig när den påträffar fel begränsning.</span><span class="sxs-lookup"><span data-stu-id="ed16a-144">Fixed a bug where query in DirectHttps mode may hang when encountering throttle failures.</span></span>
* <span data-ttu-id="ed16a-145">Fast ett programfel i sessionsläge för konsekvenskontroll.</span><span class="sxs-lookup"><span data-stu-id="ed16a-145">Fixed a bug in session consistency mode.</span></span>
* <span data-ttu-id="ed16a-146">Fast ett programfel som kan orsaka NullReferenceException i HttpContext när frågehastigheten är för hög.</span><span class="sxs-lookup"><span data-stu-id="ed16a-146">Fixed a bug which may cause NullReferenceException in HttpContext when request rate is high.</span></span>
* <span data-ttu-id="ed16a-147">Förbättrad prestanda för DirectHttps läge.</span><span class="sxs-lookup"><span data-stu-id="ed16a-147">Improved performance of DirectHttps mode.</span></span>

### <a name="a-name194194"></a><span data-ttu-id="ed16a-148"><a name="1.9.4"/>1.9.4</span><span class="sxs-lookup"><span data-stu-id="ed16a-148"><a name="1.9.4"/>1.9.4</span></span>
* <span data-ttu-id="ed16a-149">Tillagda enkel instans-baserade proxyservrar klientstöd med ConnectionPolicy.setProxy() API.</span><span class="sxs-lookup"><span data-stu-id="ed16a-149">Added simple client instance-based proxy support with ConnectionPolicy.setProxy() API.</span></span>
* <span data-ttu-id="ed16a-150">Tillagda DocumentClient.close() API tooproperly avstängning DocumentClient-instans.</span><span class="sxs-lookup"><span data-stu-id="ed16a-150">Added DocumentClient.close() API tooproperly shutdown DocumentClient instance.</span></span>
* <span data-ttu-id="ed16a-151">Förbättrad prestanda i direktanslutning läge av härledda hello frågeplan från hello interna sammansättningen i stället för hello Gateway.</span><span class="sxs-lookup"><span data-stu-id="ed16a-151">Improved query performance in direct connectivity mode by deriving hello query plan from hello native assembly instead of hello Gateway.</span></span>
* <span data-ttu-id="ed16a-152">Ange FAIL_ON_UNKNOWN_PROPERTIES = false så att användarna inte behöver toodefine JsonIgnoreProperties i sina POJO.</span><span class="sxs-lookup"><span data-stu-id="ed16a-152">Set FAIL_ON_UNKNOWN_PROPERTIES = false so users don't need toodefine JsonIgnoreProperties in their POJO.</span></span>
* <span data-ttu-id="ed16a-153">Omstrukturerade loggning toouse SLF4J.</span><span class="sxs-lookup"><span data-stu-id="ed16a-153">Refactored logging toouse SLF4J.</span></span>
* <span data-ttu-id="ed16a-154">Korrigerat några andra fel i konsekvenskontroll läsaren.</span><span class="sxs-lookup"><span data-stu-id="ed16a-154">Fixed a few other bugs in consistency reader.</span></span>

### <a name="a-name193193"></a><span data-ttu-id="ed16a-155"><a name="1.9.3"/>1.9.3</span><span class="sxs-lookup"><span data-stu-id="ed16a-155"><a name="1.9.3"/>1.9.3</span></span>
* <span data-ttu-id="ed16a-156">Fast ett programfel i hello anslutning management tooprevent anslutning minnesläckor i direktanslutning läge.</span><span class="sxs-lookup"><span data-stu-id="ed16a-156">Fixed a bug in hello connection management tooprevent connection leaks in direct connectivity mode.</span></span>
* <span data-ttu-id="ed16a-157">Fast ett programfel i hello ÖVERSTA fråga där den kan utlösa NullReferenece undantag.</span><span class="sxs-lookup"><span data-stu-id="ed16a-157">Fixed a bug in hello TOP query where it may throw NullReferenece exception.</span></span>
* <span data-ttu-id="ed16a-158">Förbättrad prestanda genom att minska hello antalet nätverk för hello internt.</span><span class="sxs-lookup"><span data-stu-id="ed16a-158">Improved performance by reducing hello number of network call for hello internal caches.</span></span>
* <span data-ttu-id="ed16a-159">Tillagda statuskoden, ActivityID och Begärd URI i DocumentClientException för bättre felsökning.</span><span class="sxs-lookup"><span data-stu-id="ed16a-159">Added status code, ActivityID and Request URI in DocumentClientException for better troubleshooting.</span></span>

### <a name="a-name192192"></a><span data-ttu-id="ed16a-160"><a name="1.9.2"/>1.9.2</span><span class="sxs-lookup"><span data-stu-id="ed16a-160"><a name="1.9.2"/>1.9.2</span></span>
* <span data-ttu-id="ed16a-161">Ett problem har åtgärdats i hello anslutningshanteringen för stabilitet.</span><span class="sxs-lookup"><span data-stu-id="ed16a-161">Fixed an issue in hello connection management for stability.</span></span>

### <a name="a-name191191"></a><span data-ttu-id="ed16a-162"><a name="1.9.1"/>1.9.1</span><span class="sxs-lookup"><span data-stu-id="ed16a-162"><a name="1.9.1"/>1.9.1</span></span>
* <span data-ttu-id="ed16a-163">Stöd för BoundedStaleness konsekvensnivå har lagts till.</span><span class="sxs-lookup"><span data-stu-id="ed16a-163">Added support for BoundedStaleness consistency level.</span></span>
* <span data-ttu-id="ed16a-164">Stöd för direkt anslutning för CRUD-åtgärder för partitionerade samlingar har lagts till.</span><span class="sxs-lookup"><span data-stu-id="ed16a-164">Added support for direct connectivity for CRUD operations for partitioned collections.</span></span>
* <span data-ttu-id="ed16a-165">Fast ett programfel i frågar en SQL-databas.</span><span class="sxs-lookup"><span data-stu-id="ed16a-165">Fixed a bug in querying a database with SQL.</span></span>
* <span data-ttu-id="ed16a-166">Fast ett programfel i hello sessionscachen där sessionstoken kan ställas in felaktigt.</span><span class="sxs-lookup"><span data-stu-id="ed16a-166">Fixed a bug in hello session cache where session token may be set incorrectly.</span></span>

### <a name="a-name190190"></a><span data-ttu-id="ed16a-167"><a name="1.9.0"/>1.9.0</span><span class="sxs-lookup"><span data-stu-id="ed16a-167"><a name="1.9.0"/>1.9.0</span></span>
* <span data-ttu-id="ed16a-168">Tillagt stöd för mellan partition parallella frågor.</span><span class="sxs-lookup"><span data-stu-id="ed16a-168">Added support for cross partition parallel queries.</span></span>
* <span data-ttu-id="ed16a-169">Stöd för upp/ORDER BY-frågor för partitionerade samlingar har lagts till.</span><span class="sxs-lookup"><span data-stu-id="ed16a-169">Added support for TOP/ORDER BY queries for partitioned collections.</span></span>
* <span data-ttu-id="ed16a-170">Tillagt stöd för stark konsekvens.</span><span class="sxs-lookup"><span data-stu-id="ed16a-170">Added support for strong consistency.</span></span>
* <span data-ttu-id="ed16a-171">Stöd för namn baserat på begäranden när du använder direkt anslutning har lagts till.</span><span class="sxs-lookup"><span data-stu-id="ed16a-171">Added support for name based requests when using direct connectivity.</span></span>
* <span data-ttu-id="ed16a-172">Fast toomake ActivityId förblir konsekvent över alla anropsförsök.</span><span class="sxs-lookup"><span data-stu-id="ed16a-172">Fixed toomake ActivityId stay consistent across all request retries.</span></span>
* <span data-ttu-id="ed16a-173">Fast ett programfel relaterade toohello sessionscachen när återskapa en samling med hello samma namn.</span><span class="sxs-lookup"><span data-stu-id="ed16a-173">Fixed a bug related toohello session cache when recreating a collection with hello same name.</span></span>
* <span data-ttu-id="ed16a-174">Tillagda Polygon och LineString DataTypes när du anger samling indexering princip för geografiska avgränsningar spatial frågor.</span><span class="sxs-lookup"><span data-stu-id="ed16a-174">Added Polygon and LineString DataTypes while specifying collection indexing policy for geo-fencing spatial queries.</span></span>
* <span data-ttu-id="ed16a-175">Fast problem med Java-dokument för Java 1.8.</span><span class="sxs-lookup"><span data-stu-id="ed16a-175">Fixed issues with Java Doc for Java 1.8.</span></span>

### <a name="a-name181181"></a><span data-ttu-id="ed16a-176"><a name="1.8.1"/>1.8.1</span><span class="sxs-lookup"><span data-stu-id="ed16a-176"><a name="1.8.1"/>1.8.1</span></span>
* <span data-ttu-id="ed16a-177">Fast ett programfel i PartitionKeyDefinitionMap hämta toocache enskilda partitionssamlingar och inte göra extra partitionen viktiga förfrågningar.</span><span class="sxs-lookup"><span data-stu-id="ed16a-177">Fixed a bug in PartitionKeyDefinitionMap toocache single partition collections and not make extra fetch partition key requests.</span></span>
* <span data-ttu-id="ed16a-178">Fast bugg toonot försök igen när en felaktig partitionsnyckelvärde har angetts.</span><span class="sxs-lookup"><span data-stu-id="ed16a-178">Fixed a bug toonot retry when an incorrect partition key value is provided.</span></span>

### <a name="a-name180180"></a><span data-ttu-id="ed16a-179"><a name="1.8.0"/>1.8.0</span><span class="sxs-lookup"><span data-stu-id="ed16a-179"><a name="1.8.0"/>1.8.0</span></span>
* <span data-ttu-id="ed16a-180">Tillagda hello stöd för flera regioner databasen konton.</span><span class="sxs-lookup"><span data-stu-id="ed16a-180">Added hello support for multi-region database accounts.</span></span>
* <span data-ttu-id="ed16a-181">Tillagt stöd för automatiska försök igen på begränsad begäranden med alternativ toocustomize Hej max antal försök och försök max väntetid.</span><span class="sxs-lookup"><span data-stu-id="ed16a-181">Added support for automatic retry on throttled requests with options toocustomize hello max retry attempts and max retry wait time.</span></span>  <span data-ttu-id="ed16a-182">Se RetryOptions och ConnectionPolicy.getRetryOptions().</span><span class="sxs-lookup"><span data-stu-id="ed16a-182">See RetryOptions and ConnectionPolicy.getRetryOptions().</span></span>
* <span data-ttu-id="ed16a-183">Föråldrad IPartitionResolver baserad Anpassad partitionering kod.</span><span class="sxs-lookup"><span data-stu-id="ed16a-183">Deprecated IPartitionResolver based custom partitioning code.</span></span> <span data-ttu-id="ed16a-184">Använd partitionerade samlingar för högre lagring och genomflöde.</span><span class="sxs-lookup"><span data-stu-id="ed16a-184">Please use partitioned collections for higher storage and throughput.</span></span>

### <a name="a-name171171"></a><span data-ttu-id="ed16a-185"><a name="1.7.1"/>1.7.1</span><span class="sxs-lookup"><span data-stu-id="ed16a-185"><a name="1.7.1"/>1.7.1</span></span>
* <span data-ttu-id="ed16a-186">Tillagda försök princip stöd för begränsning.</span><span class="sxs-lookup"><span data-stu-id="ed16a-186">Added retry policy support for throttling.</span></span>  

### <a name="a-name170170"></a><span data-ttu-id="ed16a-187"><a name="1.7.0"/>1.7.0</span><span class="sxs-lookup"><span data-stu-id="ed16a-187"><a name="1.7.0"/>1.7.0</span></span>
* <span data-ttu-id="ed16a-188">Extra tid toolive (TTL) stöd för dokument.</span><span class="sxs-lookup"><span data-stu-id="ed16a-188">Added time toolive (TTL) support for documents.</span></span>

### <a name="a-name160160"></a><span data-ttu-id="ed16a-189"><a name="1.6.0"/>1.6.0</span><span class="sxs-lookup"><span data-stu-id="ed16a-189"><a name="1.6.0"/>1.6.0</span></span>
* <span data-ttu-id="ed16a-190">Implementerad [partitionerade samlingar](partition-data.md) och [användardefinierade prestandanivåer](performance-levels.md).</span><span class="sxs-lookup"><span data-stu-id="ed16a-190">Implemented [partitioned collections](partition-data.md) and [user-defined performance levels](performance-levels.md).</span></span>

### <a name="a-name151151"></a><span data-ttu-id="ed16a-191"><a name="1.5.1"/>1.5.1</span><span class="sxs-lookup"><span data-stu-id="ed16a-191"><a name="1.5.1"/>1.5.1</span></span>
* <span data-ttu-id="ed16a-192">Fast ett programfel i HashPartitionResolver toogenerate hash-värden i little endian toobe konsekvent med andra SDK: er.</span><span class="sxs-lookup"><span data-stu-id="ed16a-192">Fixed a bug in HashPartitionResolver toogenerate hash values in little-endian toobe consistent with other SDKs.</span></span>

### <a name="a-name150150"></a><span data-ttu-id="ed16a-193"><a name="1.5.0"/>1.5.0</span><span class="sxs-lookup"><span data-stu-id="ed16a-193"><a name="1.5.0"/>1.5.0</span></span>
* <span data-ttu-id="ed16a-194">Lägg till hash- & intervall partition matchare tooassist med horisontell partitionering program över flera partitioner.</span><span class="sxs-lookup"><span data-stu-id="ed16a-194">Add Hash & Range partition resolvers tooassist with sharding applications across multiple partitions.</span></span>

### <a name="a-name140140"></a><span data-ttu-id="ed16a-195"><a name="1.4.0"/>1.4.0</span><span class="sxs-lookup"><span data-stu-id="ed16a-195"><a name="1.4.0"/>1.4.0</span></span>
* <span data-ttu-id="ed16a-196">Implementera Upsert.</span><span class="sxs-lookup"><span data-stu-id="ed16a-196">Implement Upsert.</span></span> <span data-ttu-id="ed16a-197">Nya metoder för upsertXXX lägga toosupport Upsert funktion.</span><span class="sxs-lookup"><span data-stu-id="ed16a-197">New upsertXXX methods added toosupport Upsert feature.</span></span>
* <span data-ttu-id="ed16a-198">Implementera ID-baserat routning.</span><span class="sxs-lookup"><span data-stu-id="ed16a-198">Implement ID Based Routing.</span></span> <span data-ttu-id="ed16a-199">Inga offentliga API-ändringar, alla ändringar som är interna.</span><span class="sxs-lookup"><span data-stu-id="ed16a-199">No public API changes, all changes internal.</span></span>

### <a name="a-name130130"></a><span data-ttu-id="ed16a-200"><a name="1.3.0"/>1.3.0</span><span class="sxs-lookup"><span data-stu-id="ed16a-200"><a name="1.3.0"/>1.3.0</span></span>
* <span data-ttu-id="ed16a-201">Versionen hoppas över toobring versionsnumret i enlighet med andra SDK:</span><span class="sxs-lookup"><span data-stu-id="ed16a-201">Release skipped toobring version number in alignment with other SDKs</span></span>

### <a name="a-name120120"></a><span data-ttu-id="ed16a-202"><a name="1.2.0"/>1.2.0</span><span class="sxs-lookup"><span data-stu-id="ed16a-202"><a name="1.2.0"/>1.2.0</span></span>
* <span data-ttu-id="ed16a-203">Stöder geospatiala Index</span><span class="sxs-lookup"><span data-stu-id="ed16a-203">Supports GeoSpatial Index</span></span>
* <span data-ttu-id="ed16a-204">Verifierar id-egenskapen för alla resurser.</span><span class="sxs-lookup"><span data-stu-id="ed16a-204">Validates id property for all resources.</span></span> <span data-ttu-id="ed16a-205">ID för resurser kan inte innehålla?, /, #, \, tecken eller sluta med ett blanksteg.</span><span class="sxs-lookup"><span data-stu-id="ed16a-205">Ids for resources cannot contain ?, /, #, \, characters or end with a space.</span></span>
* <span data-ttu-id="ed16a-206">Lägger till nya rubriken ”index omvandling pågår” tooResourceResponse.</span><span class="sxs-lookup"><span data-stu-id="ed16a-206">Adds new header "index transformation progress" tooResourceResponse.</span></span>

### <a name="a-name110110"></a><span data-ttu-id="ed16a-207"><a name="1.1.0"/>1.1.0</span><span class="sxs-lookup"><span data-stu-id="ed16a-207"><a name="1.1.0"/>1.1.0</span></span>
* <span data-ttu-id="ed16a-208">Implementerar V2 indexprincip</span><span class="sxs-lookup"><span data-stu-id="ed16a-208">Implements V2 indexing policy</span></span>

### <a name="a-name100100"></a><span data-ttu-id="ed16a-209"><a name="1.0.0"/>1.0.0</span><span class="sxs-lookup"><span data-stu-id="ed16a-209"><a name="1.0.0"/>1.0.0</span></span>
* <span data-ttu-id="ed16a-210">GA-SDK</span><span class="sxs-lookup"><span data-stu-id="ed16a-210">GA SDK</span></span>

## <a name="release--retirement-dates"></a><span data-ttu-id="ed16a-211">Versionen & pensionering datum</span><span class="sxs-lookup"><span data-stu-id="ed16a-211">Release & Retirement Dates</span></span>
<span data-ttu-id="ed16a-212">Microsoft meddelar notification minst **12 månader** innan du tar bort en SDK i ordning toosmooth hello övergången tooa nyare/stöds version.</span><span class="sxs-lookup"><span data-stu-id="ed16a-212">Microsoft will provide notification at least **12 months** in advance of retiring an SDK in order toosmooth hello transition tooa newer/supported version.</span></span>

<span data-ttu-id="ed16a-213">Nya funktioner och funktionalitet och optimeringar läggs endast toohello aktuella SDK, som sådan rekommenderas det att du alltid uppgradera toohello senaste SDK version så snart som möjligt.</span><span class="sxs-lookup"><span data-stu-id="ed16a-213">New features and functionality and optimizations are only added toohello current SDK, as such it is  recommend that you always upgrade toohello latest SDK version as early as possible.</span></span>

<span data-ttu-id="ed16a-214">Alla begäran tooCosmos databasen med en pensionerad SDK avvisas av hello-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="ed16a-214">Any request tooCosmos DB using a retired SDK will be rejected by hello service.</span></span>

> [!WARNING]
> <span data-ttu-id="ed16a-215">Alla versioner av hello DocumentDB SDK för Java tidigare tooversion **1.0.0** ska tas bort på **29 februari 2016**.</span><span class="sxs-lookup"><span data-stu-id="ed16a-215">All versions of hello DocumentDB SDK for Java prior tooversion **1.0.0** will be retired on **February 29, 2016**.</span></span>
> 
> 

<br/>

| <span data-ttu-id="ed16a-216">Version</span><span class="sxs-lookup"><span data-stu-id="ed16a-216">Version</span></span> | <span data-ttu-id="ed16a-217">Utgivningsdatum</span><span class="sxs-lookup"><span data-stu-id="ed16a-217">Release Date</span></span> | <span data-ttu-id="ed16a-218">Datumet för tillbakadragandet</span><span class="sxs-lookup"><span data-stu-id="ed16a-218">Retirement Date</span></span> |
| --- | --- | --- |
| [<span data-ttu-id="ed16a-219">1.12.0</span><span class="sxs-lookup"><span data-stu-id="ed16a-219">1.12.0</span></span>](#1.12.0) |<span data-ttu-id="ed16a-220">11 juli 2017</span><span class="sxs-lookup"><span data-stu-id="ed16a-220">July 11, 2017</span></span> |--- |
| [<span data-ttu-id="ed16a-221">1.11.0</span><span class="sxs-lookup"><span data-stu-id="ed16a-221">1.11.0</span></span>](#1.11.0) |<span data-ttu-id="ed16a-222">10 maj 2017</span><span class="sxs-lookup"><span data-stu-id="ed16a-222">May 10, 2017</span></span> |--- |
| [<span data-ttu-id="ed16a-223">1.10.0</span><span class="sxs-lookup"><span data-stu-id="ed16a-223">1.10.0</span></span>](#1.10.0) |<span data-ttu-id="ed16a-224">11 mars 2017</span><span class="sxs-lookup"><span data-stu-id="ed16a-224">March 11, 2017</span></span> |--- |
| [<span data-ttu-id="ed16a-225">1.9.6</span><span class="sxs-lookup"><span data-stu-id="ed16a-225">1.9.6</span></span>](#1.9.6) |<span data-ttu-id="ed16a-226">Den 21 februari 2017</span><span class="sxs-lookup"><span data-stu-id="ed16a-226">February 21, 2017</span></span> |--- |
| [<span data-ttu-id="ed16a-227">1.9.5</span><span class="sxs-lookup"><span data-stu-id="ed16a-227">1.9.5</span></span>](#1.9.5) |<span data-ttu-id="ed16a-228">31 januari 2017</span><span class="sxs-lookup"><span data-stu-id="ed16a-228">January 31, 2017</span></span> |--- |
| [<span data-ttu-id="ed16a-229">1.9.4</span><span class="sxs-lookup"><span data-stu-id="ed16a-229">1.9.4</span></span>](#1.9.4) |<span data-ttu-id="ed16a-230">24 november 2016</span><span class="sxs-lookup"><span data-stu-id="ed16a-230">November 24, 2016</span></span> |--- |
| [<span data-ttu-id="ed16a-231">1.9.3</span><span class="sxs-lookup"><span data-stu-id="ed16a-231">1.9.3</span></span>](#1.9.3) |<span data-ttu-id="ed16a-232">30 oktober 2016</span><span class="sxs-lookup"><span data-stu-id="ed16a-232">October 30, 2016</span></span> |--- |
| [<span data-ttu-id="ed16a-233">1.9.2</span><span class="sxs-lookup"><span data-stu-id="ed16a-233">1.9.2</span></span>](#1.9.2) |<span data-ttu-id="ed16a-234">28 oktober 2016</span><span class="sxs-lookup"><span data-stu-id="ed16a-234">October 28, 2016</span></span> |--- |
| [<span data-ttu-id="ed16a-235">1.9.1</span><span class="sxs-lookup"><span data-stu-id="ed16a-235">1.9.1</span></span>](#1.9.1) |<span data-ttu-id="ed16a-236">26 oktober 2016</span><span class="sxs-lookup"><span data-stu-id="ed16a-236">October 26, 2016</span></span> |--- |
| [<span data-ttu-id="ed16a-237">1.9.0</span><span class="sxs-lookup"><span data-stu-id="ed16a-237">1.9.0</span></span>](#1.9.0) |<span data-ttu-id="ed16a-238">03 oktober 2016</span><span class="sxs-lookup"><span data-stu-id="ed16a-238">October 03, 2016</span></span> |--- |
| [<span data-ttu-id="ed16a-239">1.8.1</span><span class="sxs-lookup"><span data-stu-id="ed16a-239">1.8.1</span></span>](#1.8.1) |<span data-ttu-id="ed16a-240">30 juni 2016</span><span class="sxs-lookup"><span data-stu-id="ed16a-240">June 30, 2016</span></span> |--- |
| [<span data-ttu-id="ed16a-241">1.8.0</span><span class="sxs-lookup"><span data-stu-id="ed16a-241">1.8.0</span></span>](#1.8.0) |<span data-ttu-id="ed16a-242">14 juni 2016</span><span class="sxs-lookup"><span data-stu-id="ed16a-242">June 14, 2016</span></span> |--- |
| [<span data-ttu-id="ed16a-243">1.7.1</span><span class="sxs-lookup"><span data-stu-id="ed16a-243">1.7.1</span></span>](#1.7.1) |<span data-ttu-id="ed16a-244">30 april 2016</span><span class="sxs-lookup"><span data-stu-id="ed16a-244">April 30, 2016</span></span> |--- |
| [<span data-ttu-id="ed16a-245">1.7.0</span><span class="sxs-lookup"><span data-stu-id="ed16a-245">1.7.0</span></span>](#1.7.0) |<span data-ttu-id="ed16a-246">27 april 2016</span><span class="sxs-lookup"><span data-stu-id="ed16a-246">April 27, 2016</span></span> |--- |
| [<span data-ttu-id="ed16a-247">1.6.0</span><span class="sxs-lookup"><span data-stu-id="ed16a-247">1.6.0</span></span>](#1.6.0) |<span data-ttu-id="ed16a-248">Den 29 mars 2016</span><span class="sxs-lookup"><span data-stu-id="ed16a-248">March 29, 2016</span></span> |--- |
| [<span data-ttu-id="ed16a-249">1.5.1</span><span class="sxs-lookup"><span data-stu-id="ed16a-249">1.5.1</span></span>](#1.5.1) |<span data-ttu-id="ed16a-250">Den 31 december 2015</span><span class="sxs-lookup"><span data-stu-id="ed16a-250">December 31, 2015</span></span> |--- |
| [<span data-ttu-id="ed16a-251">1.5.0</span><span class="sxs-lookup"><span data-stu-id="ed16a-251">1.5.0</span></span>](#1.5.0) |<span data-ttu-id="ed16a-252">04 december 2015</span><span class="sxs-lookup"><span data-stu-id="ed16a-252">December 04, 2015</span></span> |--- |
| [<span data-ttu-id="ed16a-253">1.4.0</span><span class="sxs-lookup"><span data-stu-id="ed16a-253">1.4.0</span></span>](#1.4.0) |<span data-ttu-id="ed16a-254">05 oktober 2015</span><span class="sxs-lookup"><span data-stu-id="ed16a-254">October 05, 2015</span></span> |--- |
| [<span data-ttu-id="ed16a-255">1.3.0</span><span class="sxs-lookup"><span data-stu-id="ed16a-255">1.3.0</span></span>](#1.3.0) |<span data-ttu-id="ed16a-256">05 oktober 2015</span><span class="sxs-lookup"><span data-stu-id="ed16a-256">October 05, 2015</span></span> |--- |
| [<span data-ttu-id="ed16a-257">1.2.0</span><span class="sxs-lookup"><span data-stu-id="ed16a-257">1.2.0</span></span>](#1.2.0) |<span data-ttu-id="ed16a-258">05 augusti 2015</span><span class="sxs-lookup"><span data-stu-id="ed16a-258">August 05, 2015</span></span> |--- |
| [<span data-ttu-id="ed16a-259">1.1.0</span><span class="sxs-lookup"><span data-stu-id="ed16a-259">1.1.0</span></span>](#1.1.0) |<span data-ttu-id="ed16a-260">09 juli 2015</span><span class="sxs-lookup"><span data-stu-id="ed16a-260">July 09, 2015</span></span> |--- |
| [<span data-ttu-id="ed16a-261">1.0.1</span><span class="sxs-lookup"><span data-stu-id="ed16a-261">1.0.1</span></span>](#1.0.1) |<span data-ttu-id="ed16a-262">12 maj 2015</span><span class="sxs-lookup"><span data-stu-id="ed16a-262">May 12, 2015</span></span> |--- |
| [<span data-ttu-id="ed16a-263">1.0.0</span><span class="sxs-lookup"><span data-stu-id="ed16a-263">1.0.0</span></span>](#1.0.0) |<span data-ttu-id="ed16a-264">07 april 2015</span><span class="sxs-lookup"><span data-stu-id="ed16a-264">April 07, 2015</span></span> |--- |
| <span data-ttu-id="ed16a-265">0.9.5-prelease</span><span class="sxs-lookup"><span data-stu-id="ed16a-265">0.9.5-prelease</span></span> |<span data-ttu-id="ed16a-266">09 mar 2015</span><span class="sxs-lookup"><span data-stu-id="ed16a-266">Mar 09, 2015</span></span> |<span data-ttu-id="ed16a-267">Den 29 februari 2016</span><span class="sxs-lookup"><span data-stu-id="ed16a-267">February 29, 2016</span></span> |
| <span data-ttu-id="ed16a-268">0.9.4-prelease</span><span class="sxs-lookup"><span data-stu-id="ed16a-268">0.9.4-prelease</span></span> |<span data-ttu-id="ed16a-269">17 februari 2015</span><span class="sxs-lookup"><span data-stu-id="ed16a-269">February 17, 2015</span></span> |<span data-ttu-id="ed16a-270">Den 29 februari 2016</span><span class="sxs-lookup"><span data-stu-id="ed16a-270">February 29, 2016</span></span> |
| <span data-ttu-id="ed16a-271">0.9.3-prelease</span><span class="sxs-lookup"><span data-stu-id="ed16a-271">0.9.3-prelease</span></span> |<span data-ttu-id="ed16a-272">Den 13 januari 2015</span><span class="sxs-lookup"><span data-stu-id="ed16a-272">January 13, 2015</span></span> |<span data-ttu-id="ed16a-273">Den 29 februari 2016</span><span class="sxs-lookup"><span data-stu-id="ed16a-273">February 29, 2016</span></span> |
| <span data-ttu-id="ed16a-274">0.9.2-prelease</span><span class="sxs-lookup"><span data-stu-id="ed16a-274">0.9.2-prelease</span></span> |<span data-ttu-id="ed16a-275">19 december 2014</span><span class="sxs-lookup"><span data-stu-id="ed16a-275">December 19, 2014</span></span> |<span data-ttu-id="ed16a-276">Den 29 februari 2016</span><span class="sxs-lookup"><span data-stu-id="ed16a-276">February 29, 2016</span></span> |
| <span data-ttu-id="ed16a-277">0.9.1-prelease</span><span class="sxs-lookup"><span data-stu-id="ed16a-277">0.9.1-prelease</span></span> |<span data-ttu-id="ed16a-278">19 december 2014</span><span class="sxs-lookup"><span data-stu-id="ed16a-278">December 19, 2014</span></span> |<span data-ttu-id="ed16a-279">Den 29 februari 2016</span><span class="sxs-lookup"><span data-stu-id="ed16a-279">February 29, 2016</span></span> |
| <span data-ttu-id="ed16a-280">0.9.0-prelease</span><span class="sxs-lookup"><span data-stu-id="ed16a-280">0.9.0-prelease</span></span> |<span data-ttu-id="ed16a-281">10 december 2014</span><span class="sxs-lookup"><span data-stu-id="ed16a-281">December 10, 2014</span></span> |<span data-ttu-id="ed16a-282">Den 29 februari 2016</span><span class="sxs-lookup"><span data-stu-id="ed16a-282">February 29, 2016</span></span> |

## <a name="faq"></a><span data-ttu-id="ed16a-283">VANLIGA FRÅGOR OCH SVAR</span><span class="sxs-lookup"><span data-stu-id="ed16a-283">FAQ</span></span>
[!INCLUDE [cosmos-db-sdk-faq](../../includes/cosmos-db-sdk-faq.md)]

## <a name="see-also"></a><span data-ttu-id="ed16a-284">Se även</span><span class="sxs-lookup"><span data-stu-id="ed16a-284">See Also</span></span>
<span data-ttu-id="ed16a-285">toolearn mer om Cosmos DB finns [Microsoft Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) sida.</span><span class="sxs-lookup"><span data-stu-id="ed16a-285">toolearn more about Cosmos DB, see [Microsoft Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) service page.</span></span>

