---
title: aaaAzure Cosmos DB Python API SDK & resurser | Microsoft Docs
description: "Läs mer om hello Python-API och SDK inklusive frisläppningsdatum, tillbakadragning datum och ändringar mellan varje version av hello Azure Cosmos DB Python SDK."
services: cosmos-db
documentationcenter: python
author: rnagpal
manager: jhubbard
editor: cgronlun
ms.assetid: 3ac344a9-b2fa-4a3f-a4cc-02d287e05469
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: python
ms.topic: article
ms.date: 05/24/2017
ms.author: rnagpal
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 1a164b72d2bd819de87df0229357b82e2177af2f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-cosmos-db-python-sdk-release-notes-and-resources"></a><span data-ttu-id="4c9da-103">Azure Cosmos DB Python SDK: Viktig information och resurser</span><span class="sxs-lookup"><span data-stu-id="4c9da-103">Azure Cosmos DB Python SDK: Release notes and resources</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="4c9da-104">.NET</span><span class="sxs-lookup"><span data-stu-id="4c9da-104">.NET</span></span>](documentdb-sdk-dotnet.md)
> * [<span data-ttu-id="4c9da-105">.NET ändra Feed</span><span class="sxs-lookup"><span data-stu-id="4c9da-105">.NET Change Feed</span></span>](documentdb-sdk-dotnet-changefeed.md)
> * [<span data-ttu-id="4c9da-106">.NET Core</span><span class="sxs-lookup"><span data-stu-id="4c9da-106">.NET Core</span></span>](documentdb-sdk-dotnet-core.md)
> * [<span data-ttu-id="4c9da-107">Node.js</span><span class="sxs-lookup"><span data-stu-id="4c9da-107">Node.js</span></span>](documentdb-sdk-node.md)
> * [<span data-ttu-id="4c9da-108">Java</span><span class="sxs-lookup"><span data-stu-id="4c9da-108">Java</span></span>](documentdb-sdk-java.md)
> * [<span data-ttu-id="4c9da-109">Python</span><span class="sxs-lookup"><span data-stu-id="4c9da-109">Python</span></span>](documentdb-sdk-python.md)
> * [<span data-ttu-id="4c9da-110">REST</span><span class="sxs-lookup"><span data-stu-id="4c9da-110">REST</span></span>](https://docs.microsoft.com/rest/api/documentdb/)
> * [<span data-ttu-id="4c9da-111">REST-resursprovider</span><span class="sxs-lookup"><span data-stu-id="4c9da-111">REST Resource Provider</span></span>](https://docs.microsoft.com/rest/api/documentdbresourceprovider/)
> * [<span data-ttu-id="4c9da-112">SQL</span><span class="sxs-lookup"><span data-stu-id="4c9da-112">SQL</span></span>](https://msdn.microsoft.com/library/azure/dn782250.aspx)
> 
> 

<table>

<tr><td><span data-ttu-id="4c9da-113">**Ladda ned SDK**</span><span class="sxs-lookup"><span data-stu-id="4c9da-113">**Download SDK**</span></span></td><td>[<span data-ttu-id="4c9da-114">PyPI</span><span class="sxs-lookup"><span data-stu-id="4c9da-114">PyPI</span></span>](https://pypi.python.org/pypi/pydocumentdb)</td></tr>

<tr><td><span data-ttu-id="4c9da-115">**API-dokumentationen**</span><span class="sxs-lookup"><span data-stu-id="4c9da-115">**API documentation**</span></span></td><td>[<span data-ttu-id="4c9da-116">Python-API-referensdokumentation</span><span class="sxs-lookup"><span data-stu-id="4c9da-116">Python API reference documentation</span></span>](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.html)</td></tr>

<tr><td><span data-ttu-id="4c9da-117">**Installationsinstruktioner för SDK**</span><span class="sxs-lookup"><span data-stu-id="4c9da-117">**SDK installation instructions**</span></span></td><td>[<span data-ttu-id="4c9da-118">Installationsinstruktioner för Python SDK</span><span class="sxs-lookup"><span data-stu-id="4c9da-118">Python SDK installation instructions</span></span>](http://azure.github.io/azure-documentdb-python/)</td></tr>

<tr><td><span data-ttu-id="4c9da-119">**Bidra tooSDK**</span><span class="sxs-lookup"><span data-stu-id="4c9da-119">**Contribute tooSDK**</span></span></td><td>[<span data-ttu-id="4c9da-120">GitHub</span><span class="sxs-lookup"><span data-stu-id="4c9da-120">GitHub</span></span>](https://github.com/Azure/azure-documentdb-python)</td></tr>

<tr><td><span data-ttu-id="4c9da-121">**Kom igång**</span><span class="sxs-lookup"><span data-stu-id="4c9da-121">**Get started**</span></span></td><td>[<span data-ttu-id="4c9da-122">Kom igång med hello Python SDK</span><span class="sxs-lookup"><span data-stu-id="4c9da-122">Get started with hello Python SDK</span></span>](documentdb-python-application.md)</td></tr>

<tr><td><span data-ttu-id="4c9da-123">**Aktuella plattform som stöds**</span><span class="sxs-lookup"><span data-stu-id="4c9da-123">**Current supported platform**</span></span></td><td><span data-ttu-id="4c9da-124">[Python 2.7](https://www.python.org/downloads/) och [Python 3.5](https://www.python.org/downloads/)</span><span class="sxs-lookup"><span data-stu-id="4c9da-124">[Python 2.7](https://www.python.org/downloads/) and [Python 3.5](https://www.python.org/downloads/)</span></span></td></tr>
</table></br>

## <a name="release-notes"></a><span data-ttu-id="4c9da-125">Viktig information</span><span class="sxs-lookup"><span data-stu-id="4c9da-125">Release notes</span></span>
### <a name="a-name220220"></a><span data-ttu-id="4c9da-126"><a name="2.2.0"/>2.2.0</span><span class="sxs-lookup"><span data-stu-id="4c9da-126"><a name="2.2.0"/>2.2.0</span></span>
* <span data-ttu-id="4c9da-127">Tillagt stöd för en ny konsekvensnivå kallas ConsistentPrefix.</span><span class="sxs-lookup"><span data-stu-id="4c9da-127">Added support for a new consistency level called ConsistentPrefix.</span></span>


### <a name="a-name210210"></a><span data-ttu-id="4c9da-128"><a name="2.1.0"/>2.1.0</span><span class="sxs-lookup"><span data-stu-id="4c9da-128"><a name="2.1.0"/>2.1.0</span></span>
* <span data-ttu-id="4c9da-129">Stöd för aggregering frågor (COUNT, MIN, MAX, SUM och AVG) har lagts till.</span><span class="sxs-lookup"><span data-stu-id="4c9da-129">Added support for aggregation queries (COUNT, MIN, MAX, SUM, and AVG).</span></span>
* <span data-ttu-id="4c9da-130">Lägga till ett alternativ för att inaktivera SSL-kontroll när du kör mot Cosmos DB-emulatorn.</span><span class="sxs-lookup"><span data-stu-id="4c9da-130">Added an option for disabling SSL verification when running against Cosmos DB Emulator.</span></span>
* <span data-ttu-id="4c9da-131">Ta bort hello begränsning för beroende begäranden modulen toobe exakt 2.10.0.</span><span class="sxs-lookup"><span data-stu-id="4c9da-131">Removed hello restriction of dependent requests module toobe exactly 2.10.0.</span></span>
* <span data-ttu-id="4c9da-132">Sänks minsta dataflöde på partitionerade samlingar från 10,100 RU/s too2500 RU/s.</span><span class="sxs-lookup"><span data-stu-id="4c9da-132">Lowered minimum throughput on partitioned collections from 10,100 RU/s too2500 RU/s.</span></span>
* <span data-ttu-id="4c9da-133">Stöd har lagts till för att aktivera loggning skript under körning av lagrad procedur.</span><span class="sxs-lookup"><span data-stu-id="4c9da-133">Added support for enabling script logging during stored procedure execution.</span></span>
* <span data-ttu-id="4c9da-134">REST API-version för stötar ' 2017-01-19' med den här versionen.</span><span class="sxs-lookup"><span data-stu-id="4c9da-134">REST API version bumped too'2017-01-19' with this release.</span></span>

### <a name="a-name201201"></a><span data-ttu-id="4c9da-135"><a name="2.0.1"/>2.0.1</span><span class="sxs-lookup"><span data-stu-id="4c9da-135"><a name="2.0.1"/>2.0.1</span></span>
* <span data-ttu-id="4c9da-136">Gjort redigeringsändringar toodocumentation kommentarer.</span><span class="sxs-lookup"><span data-stu-id="4c9da-136">Made editorial changes toodocumentation comments.</span></span>

### <a name="a-name200200"></a><span data-ttu-id="4c9da-137"><a name="2.0.0"/>2.0.0</span><span class="sxs-lookup"><span data-stu-id="4c9da-137"><a name="2.0.0"/>2.0.0</span></span>
* <span data-ttu-id="4c9da-138">Stöd för Python 3.5 har lagts till.</span><span class="sxs-lookup"><span data-stu-id="4c9da-138">Added support for Python 3.5.</span></span>
* <span data-ttu-id="4c9da-139">Stöd för anslutningspooler modulen begäranden har lagts till.</span><span class="sxs-lookup"><span data-stu-id="4c9da-139">Added support for connection pooling using a requests module.</span></span>
* <span data-ttu-id="4c9da-140">Stöd för sessionskonsekvens har lagts till.</span><span class="sxs-lookup"><span data-stu-id="4c9da-140">Added support for session consistency.</span></span>
* <span data-ttu-id="4c9da-141">Stöd för upp/ORDERBY för partitionerade samlingar har lagts till.</span><span class="sxs-lookup"><span data-stu-id="4c9da-141">Added support for TOP/ORDERBY queries for partitioned collections.</span></span>

### <a name="a-name190190"></a><span data-ttu-id="4c9da-142"><a name="1.9.0"/>1.9.0</span><span class="sxs-lookup"><span data-stu-id="4c9da-142"><a name="1.9.0"/>1.9.0</span></span>
* <span data-ttu-id="4c9da-143">Tillagda försök princip stöd för begränsad begäranden.</span><span class="sxs-lookup"><span data-stu-id="4c9da-143">Added retry policy support for throttled requests.</span></span> <span data-ttu-id="4c9da-144">(Begränsad begäranden får en förfrågan hastighet för stor undantag, felkod 429.) Standard Azure Cosmos DB återförsök nio gånger för varje begäran när felkoden 429 påträffas respektera hello retryAfter tid i hello-Svarsrubrik.</span><span class="sxs-lookup"><span data-stu-id="4c9da-144">(Throttled requests receive a request rate too large exception, error code 429.) By default, Azure Cosmos DB retries nine times for each request when error code 429 is encountered, honoring hello retryAfter time in hello response header.</span></span> <span data-ttu-id="4c9da-145">En fast försök tidsintervall kan nu anges som en del av hello RetryOptions egenskapen på hello ConnectionPolicy objekt om du vill tooignore hello retryAfter tid mellan hello försök som returnerades av servern.</span><span class="sxs-lookup"><span data-stu-id="4c9da-145">A fixed retry interval time can now be set as part of hello RetryOptions property on hello ConnectionPolicy object if you want tooignore hello retryAfter time returned by server between hello retries.</span></span> <span data-ttu-id="4c9da-146">Azure Cosmos-DB väntar nu högst 30 sekunder för varje begäran som har begränsats (oavsett antal försök) och returnerar hello svar med felkoden 429.</span><span class="sxs-lookup"><span data-stu-id="4c9da-146">Azure Cosmos DB now waits for a maximum of 30 seconds for each request that is being throttled (irrespective of retry count) and returns hello response with error code 429.</span></span> <span data-ttu-id="4c9da-147">Nu kan också åsidosättas i hello RetryOptions egenskapen i ConnectionPolicy-objektet.</span><span class="sxs-lookup"><span data-stu-id="4c9da-147">This time can also be overriden in hello RetryOptions property on ConnectionPolicy object.</span></span>
* <span data-ttu-id="4c9da-148">Cosmos DB Returnerar nu x-ms-begränsning--antal försök och x-ms-throttle-retry-wait-time-ms som hello svarshuvuden i varje begäran toodenote hello begränsning försök antal och hello cummulative hello begära väntat mellan hello försök.</span><span class="sxs-lookup"><span data-stu-id="4c9da-148">Cosmos DB now returns x-ms-throttle-retry-count and x-ms-throttle-retry-wait-time-ms as hello response headers in every request toodenote hello throttle retry count and hello cummulative time hello request waited between hello retries.</span></span>
* <span data-ttu-id="4c9da-149">Borttagna hello RetryPolicy klass och motsvarande hello-egenskap (retry_policy) visas på hello document_client klass och introducerades i stället en RetryOptions klass exponera hello RetryOptions egenskapen ConnectionPolicy klass som kan vara används toooverride Vissa av hello standard alternativen för återförsök.</span><span class="sxs-lookup"><span data-stu-id="4c9da-149">Removed hello RetryPolicy class and hello corresponding property (retry_policy) exposed on hello document_client class and instead introduced a RetryOptions class exposing hello RetryOptions property on ConnectionPolicy class that can be used toooverride some of hello default retry options.</span></span>

### <a name="a-name180180"></a><span data-ttu-id="4c9da-150"><a name="1.8.0"/>1.8.0</span><span class="sxs-lookup"><span data-stu-id="4c9da-150"><a name="1.8.0"/>1.8.0</span></span>
* <span data-ttu-id="4c9da-151">Tillagda hello stöd för flera regioner databasen konton.</span><span class="sxs-lookup"><span data-stu-id="4c9da-151">Added hello support for multi-region database accounts.</span></span>

### <a name="a-name170170"></a><span data-ttu-id="4c9da-152"><a name="1.7.0"/>1.7.0</span><span class="sxs-lookup"><span data-stu-id="4c9da-152"><a name="1.7.0"/>1.7.0</span></span>
* <span data-ttu-id="4c9da-153">Tillagda hello stöd för tid tooLive(TTL) funktionen för dokument.</span><span class="sxs-lookup"><span data-stu-id="4c9da-153">Added hello support for Time tooLive(TTL) feature for documents.</span></span>

### <a name="a-name161161"></a><span data-ttu-id="4c9da-154"><a name="1.6.1"/>1.6.1</span><span class="sxs-lookup"><span data-stu-id="4c9da-154"><a name="1.6.1"/>1.6.1</span></span>
* <span data-ttu-id="4c9da-155">Felkorrigeringar relaterade tooserver sida partitionering tooallow specialtecken i partitionkey sökväg.</span><span class="sxs-lookup"><span data-stu-id="4c9da-155">Bug fixes related tooserver side partitioning tooallow special characters in partitionkey path.</span></span>

### <a name="a-name160160"></a><span data-ttu-id="4c9da-156"><a name="1.6.0"/>1.6.0</span><span class="sxs-lookup"><span data-stu-id="4c9da-156"><a name="1.6.0"/>1.6.0</span></span>
* <span data-ttu-id="4c9da-157">Implementerad [partitionerade samlingar](partition-data.md) och [användardefinierade prestandanivåer](performance-levels.md).</span><span class="sxs-lookup"><span data-stu-id="4c9da-157">Implemented [partitioned collections](partition-data.md) and [user-defined performance levels](performance-levels.md).</span></span> 

### <a name="a-name150150"></a><span data-ttu-id="4c9da-158"><a name="1.5.0"/>1.5.0</span><span class="sxs-lookup"><span data-stu-id="4c9da-158"><a name="1.5.0"/>1.5.0</span></span>
* <span data-ttu-id="4c9da-159">Lägg till hash- & intervall partition matchare tooassist med horisontell partitionering program över flera partitioner.</span><span class="sxs-lookup"><span data-stu-id="4c9da-159">Add Hash & Range partition resolvers tooassist with sharding applications across multiple partitions.</span></span>

### <a name="a-name142142"></a><span data-ttu-id="4c9da-160"><a name="1.4.2"/>1.4.2</span><span class="sxs-lookup"><span data-stu-id="4c9da-160"><a name="1.4.2"/>1.4.2</span></span>
* <span data-ttu-id="4c9da-161">Implementera Upsert.</span><span class="sxs-lookup"><span data-stu-id="4c9da-161">Implement Upsert.</span></span> <span data-ttu-id="4c9da-162">Nya metoder för UpsertXXX lägga toosupport Upsert funktion.</span><span class="sxs-lookup"><span data-stu-id="4c9da-162">New UpsertXXX methods added toosupport Upsert feature.</span></span>
* <span data-ttu-id="4c9da-163">Implementera ID-baserat routning.</span><span class="sxs-lookup"><span data-stu-id="4c9da-163">Implement ID Based Routing.</span></span> <span data-ttu-id="4c9da-164">Inga offentliga API-ändringar, alla ändringar som är interna.</span><span class="sxs-lookup"><span data-stu-id="4c9da-164">No public API changes, all changes internal.</span></span>

### <a name="a-name120120"></a><span data-ttu-id="4c9da-165"><a name="1.2.0"/>1.2.0</span><span class="sxs-lookup"><span data-stu-id="4c9da-165"><a name="1.2.0"/>1.2.0</span></span>
* <span data-ttu-id="4c9da-166">Stöder geospatiala index.</span><span class="sxs-lookup"><span data-stu-id="4c9da-166">Supports GeoSpatial index.</span></span>
* <span data-ttu-id="4c9da-167">Verifierar id-egenskapen för alla resurser.</span><span class="sxs-lookup"><span data-stu-id="4c9da-167">Validates id property for all resources.</span></span> <span data-ttu-id="4c9da-168">ID för resurser kan inte innehålla?, /, #, \, tecken eller sluta med ett blanksteg.</span><span class="sxs-lookup"><span data-stu-id="4c9da-168">Ids for resources cannot contain ?, /, #, \, characters or end with a space.</span></span>
* <span data-ttu-id="4c9da-169">Lägger till nya rubriken ”index omvandling pågår” tooResourceResponse.</span><span class="sxs-lookup"><span data-stu-id="4c9da-169">Adds new header "index transformation progress" tooResourceResponse.</span></span>

### <a name="a-name110110"></a><span data-ttu-id="4c9da-170"><a name="1.1.0"/>1.1.0</span><span class="sxs-lookup"><span data-stu-id="4c9da-170"><a name="1.1.0"/>1.1.0</span></span>
* <span data-ttu-id="4c9da-171">Implementerar V2 indexprincip.</span><span class="sxs-lookup"><span data-stu-id="4c9da-171">Implements V2 indexing policy.</span></span>

### <a name="a-name101101"></a><span data-ttu-id="4c9da-172"><a name="1.0.1"/>1.0.1</span><span class="sxs-lookup"><span data-stu-id="4c9da-172"><a name="1.0.1"/>1.0.1</span></span>
* <span data-ttu-id="4c9da-173">Stöd för proxyanslutning.</span><span class="sxs-lookup"><span data-stu-id="4c9da-173">Supports proxy connection.</span></span>

### <a name="a-name100100"></a><span data-ttu-id="4c9da-174"><a name="1.0.0"/>1.0.0</span><span class="sxs-lookup"><span data-stu-id="4c9da-174"><a name="1.0.0"/>1.0.0</span></span>
* <span data-ttu-id="4c9da-175">GA SDK.</span><span class="sxs-lookup"><span data-stu-id="4c9da-175">GA SDK.</span></span>

## <a name="release--retirement-dates"></a><span data-ttu-id="4c9da-176">Versionen & pensionering datum</span><span class="sxs-lookup"><span data-stu-id="4c9da-176">Release & retirement dates</span></span>
<span data-ttu-id="4c9da-177">Microsoft meddelar notification minst **12 månader** innan du tar bort en SDK i ordning toosmooth hello övergången tooa nyare/stöds version.</span><span class="sxs-lookup"><span data-stu-id="4c9da-177">Microsoft will provide notification at least **12 months** in advance of retiring an SDK in order toosmooth hello transition tooa newer/supported version.</span></span>

<span data-ttu-id="4c9da-178">Nya funktioner och funktionalitet och optimeringar läggs endast toohello aktuella SDK, som sådan rekommenderas det att du alltid uppgradera toohello senaste SDK version så snart som möjligt.</span><span class="sxs-lookup"><span data-stu-id="4c9da-178">New features and functionality and optimizations are only added toohello current SDK, as such it is  recommend that you always upgrade toohello latest SDK version as early as possible.</span></span> 

<span data-ttu-id="4c9da-179">Alla begäran tooCosmos databasen med en pensionerad SDK avvisas av hello-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="4c9da-179">Any request tooCosmos DB using a retired SDK will be rejected by hello service.</span></span>

> [!WARNING]
> <span data-ttu-id="4c9da-180">Alla versioner av hello Azure DocumentDB SDK för Python tidigare tooversion **1.0.0** ska tas bort på **29 februari 2016**.</span><span class="sxs-lookup"><span data-stu-id="4c9da-180">All versions of hello Azure DocumentDB SDK for Python prior tooversion **1.0.0** will be retired on **February 29, 2016**.</span></span> 
> 
> 

<br/>

| <span data-ttu-id="4c9da-181">Version</span><span class="sxs-lookup"><span data-stu-id="4c9da-181">Version</span></span> | <span data-ttu-id="4c9da-182">Utgivningsdatum</span><span class="sxs-lookup"><span data-stu-id="4c9da-182">Release Date</span></span> | <span data-ttu-id="4c9da-183">Datumet för tillbakadragandet</span><span class="sxs-lookup"><span data-stu-id="4c9da-183">Retirement Date</span></span> |
| --- | --- | --- |
| [<span data-ttu-id="4c9da-184">2.2.0</span><span class="sxs-lookup"><span data-stu-id="4c9da-184">2.2.0</span></span>](#2.2.0) |<span data-ttu-id="4c9da-185">10 maj 2017</span><span class="sxs-lookup"><span data-stu-id="4c9da-185">May 10, 2017</span></span> |--- |
| [<span data-ttu-id="4c9da-186">2.1.0</span><span class="sxs-lookup"><span data-stu-id="4c9da-186">2.1.0</span></span>](#2.1.0) |<span data-ttu-id="4c9da-187">01 kan 2017</span><span class="sxs-lookup"><span data-stu-id="4c9da-187">May 01, 2017</span></span> |--- |
| [<span data-ttu-id="4c9da-188">2.0.1</span><span class="sxs-lookup"><span data-stu-id="4c9da-188">2.0.1</span></span>](#2.0.1) |<span data-ttu-id="4c9da-189">30 oktober 2016</span><span class="sxs-lookup"><span data-stu-id="4c9da-189">October 30, 2016</span></span> |--- |
| [<span data-ttu-id="4c9da-190">2.0.0</span><span class="sxs-lookup"><span data-stu-id="4c9da-190">2.0.0</span></span>](#2.0.0) |<span data-ttu-id="4c9da-191">Den 29 september 2016</span><span class="sxs-lookup"><span data-stu-id="4c9da-191">September 29, 2016</span></span> |--- |
| [<span data-ttu-id="4c9da-192">1.9.0</span><span class="sxs-lookup"><span data-stu-id="4c9da-192">1.9.0</span></span>](#1.9.0) |<span data-ttu-id="4c9da-193">07 juli 2016</span><span class="sxs-lookup"><span data-stu-id="4c9da-193">July 07, 2016</span></span> |--- |
| [<span data-ttu-id="4c9da-194">1.8.0</span><span class="sxs-lookup"><span data-stu-id="4c9da-194">1.8.0</span></span>](#1.8.0) |<span data-ttu-id="4c9da-195">14 juni 2016</span><span class="sxs-lookup"><span data-stu-id="4c9da-195">June 14, 2016</span></span> |--- |
| [<span data-ttu-id="4c9da-196">1.7.0</span><span class="sxs-lookup"><span data-stu-id="4c9da-196">1.7.0</span></span>](#1.7.0) |<span data-ttu-id="4c9da-197">26 april 2016</span><span class="sxs-lookup"><span data-stu-id="4c9da-197">April 26, 2016</span></span> |--- |
| [<span data-ttu-id="4c9da-198">1.6.1</span><span class="sxs-lookup"><span data-stu-id="4c9da-198">1.6.1</span></span>](#1.6.1) |<span data-ttu-id="4c9da-199">08 april 2016</span><span class="sxs-lookup"><span data-stu-id="4c9da-199">April 08, 2016</span></span> |--- |
| [<span data-ttu-id="4c9da-200">1.6.0</span><span class="sxs-lookup"><span data-stu-id="4c9da-200">1.6.0</span></span>](#1.6.0) |<span data-ttu-id="4c9da-201">Den 29 mars 2016</span><span class="sxs-lookup"><span data-stu-id="4c9da-201">March 29, 2016</span></span> |--- |
| [<span data-ttu-id="4c9da-202">1.5.0</span><span class="sxs-lookup"><span data-stu-id="4c9da-202">1.5.0</span></span>](#1.5.0) |<span data-ttu-id="4c9da-203">03 januari 2016</span><span class="sxs-lookup"><span data-stu-id="4c9da-203">January 03, 2016</span></span> |--- |
| [<span data-ttu-id="4c9da-204">1.4.2</span><span class="sxs-lookup"><span data-stu-id="4c9da-204">1.4.2</span></span>](#1.4.2) |<span data-ttu-id="4c9da-205">06 oktober 2015</span><span class="sxs-lookup"><span data-stu-id="4c9da-205">October 06, 2015</span></span> |--- |
| [<span data-ttu-id="4c9da-206">1.4.1</span><span class="sxs-lookup"><span data-stu-id="4c9da-206">1.4.1</span></span>](#1.4.1) |<span data-ttu-id="4c9da-207">06 oktober 2015</span><span class="sxs-lookup"><span data-stu-id="4c9da-207">October 06, 2015</span></span> |--- |
| [<span data-ttu-id="4c9da-208">1.2.0</span><span class="sxs-lookup"><span data-stu-id="4c9da-208">1.2.0</span></span>](#1.2.0) |<span data-ttu-id="4c9da-209">06 augusti 2015</span><span class="sxs-lookup"><span data-stu-id="4c9da-209">August 06, 2015</span></span> |--- |
| [<span data-ttu-id="4c9da-210">1.1.0</span><span class="sxs-lookup"><span data-stu-id="4c9da-210">1.1.0</span></span>](#1.1.0) |<span data-ttu-id="4c9da-211">09 juli 2015</span><span class="sxs-lookup"><span data-stu-id="4c9da-211">July 09, 2015</span></span> |--- |
| [<span data-ttu-id="4c9da-212">1.0.1</span><span class="sxs-lookup"><span data-stu-id="4c9da-212">1.0.1</span></span>](#1.0.1) |<span data-ttu-id="4c9da-213">25 maj 2015</span><span class="sxs-lookup"><span data-stu-id="4c9da-213">May 25, 2015</span></span> |--- |
| [<span data-ttu-id="4c9da-214">1.0.0</span><span class="sxs-lookup"><span data-stu-id="4c9da-214">1.0.0</span></span>](#1.0.0) |<span data-ttu-id="4c9da-215">07 april 2015</span><span class="sxs-lookup"><span data-stu-id="4c9da-215">April 07, 2015</span></span> |--- |
| <span data-ttu-id="4c9da-216">0.9.4-prelease</span><span class="sxs-lookup"><span data-stu-id="4c9da-216">0.9.4-prelease</span></span> |<span data-ttu-id="4c9da-217">14 januari 2015</span><span class="sxs-lookup"><span data-stu-id="4c9da-217">January 14, 2015</span></span> |<span data-ttu-id="4c9da-218">Den 29 februari 2016</span><span class="sxs-lookup"><span data-stu-id="4c9da-218">February 29, 2016</span></span> |
| <span data-ttu-id="4c9da-219">0.9.3-prelease</span><span class="sxs-lookup"><span data-stu-id="4c9da-219">0.9.3-prelease</span></span> |<span data-ttu-id="4c9da-220">09 december 2014</span><span class="sxs-lookup"><span data-stu-id="4c9da-220">December 09, 2014</span></span> |<span data-ttu-id="4c9da-221">Den 29 februari 2016</span><span class="sxs-lookup"><span data-stu-id="4c9da-221">February 29, 2016</span></span> |
| <span data-ttu-id="4c9da-222">0.9.2-prelease</span><span class="sxs-lookup"><span data-stu-id="4c9da-222">0.9.2-prelease</span></span> |<span data-ttu-id="4c9da-223">25 november 2014</span><span class="sxs-lookup"><span data-stu-id="4c9da-223">November 25, 2014</span></span> |<span data-ttu-id="4c9da-224">Den 29 februari 2016</span><span class="sxs-lookup"><span data-stu-id="4c9da-224">February 29, 2016</span></span> |
| <span data-ttu-id="4c9da-225">0.9.1-prelease</span><span class="sxs-lookup"><span data-stu-id="4c9da-225">0.9.1-prelease</span></span> |<span data-ttu-id="4c9da-226">23 september 2014</span><span class="sxs-lookup"><span data-stu-id="4c9da-226">September 23, 2014</span></span> |<span data-ttu-id="4c9da-227">Den 29 februari 2016</span><span class="sxs-lookup"><span data-stu-id="4c9da-227">February 29, 2016</span></span> |
| <span data-ttu-id="4c9da-228">0.9.0-prelease</span><span class="sxs-lookup"><span data-stu-id="4c9da-228">0.9.0-prelease</span></span> |<span data-ttu-id="4c9da-229">21 augusti 2014</span><span class="sxs-lookup"><span data-stu-id="4c9da-229">August 21, 2014</span></span> |<span data-ttu-id="4c9da-230">Den 29 februari 2016</span><span class="sxs-lookup"><span data-stu-id="4c9da-230">February 29, 2016</span></span> |

## <a name="faq"></a><span data-ttu-id="4c9da-231">VANLIGA FRÅGOR OCH SVAR</span><span class="sxs-lookup"><span data-stu-id="4c9da-231">FAQ</span></span>
[!INCLUDE [cosmos-db-sdk-faq](../../includes/cosmos-db-sdk-faq.md)]

## <a name="see-also"></a><span data-ttu-id="4c9da-232">Se även</span><span class="sxs-lookup"><span data-stu-id="4c9da-232">See also</span></span>
<span data-ttu-id="4c9da-233">toolearn mer om Cosmos DB finns [Microsoft Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) sida.</span><span class="sxs-lookup"><span data-stu-id="4c9da-233">toolearn more about Cosmos DB, see [Microsoft Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) service page.</span></span> 

