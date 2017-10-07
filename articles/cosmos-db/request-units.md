---
title: "aaaRequest enheter & uppskattning genomströmning - Azure Cosmos DB | Microsoft Docs"
description: "Läs mer om hur toounderstand, ange och beräkna begäran enhet krav i Azure Cosmos DB."
services: cosmos-db
author: mimig1
manager: jhubbard
editor: mimig
documentationcenter: 
ms.assetid: d0a3c310-eb63-4e45-8122-b7724095c32f
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/10/2017
ms.author: mimig
ms.openlocfilehash: 13c4e7aeb6222fa14ef982e238716e15a0159fd5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="request-units-in-azure-cosmos-db"></a><span data-ttu-id="77ff8-103">Enheter för programbegäran i Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="77ff8-103">Request Units in Azure Cosmos DB</span></span>
<span data-ttu-id="77ff8-104">Nu tillgängligt: Azure Cosmos-DB [begäran enhet Kalkylatorn](https://www.documentdb.com/capacityplanner).</span><span class="sxs-lookup"><span data-stu-id="77ff8-104">Now available: Azure Cosmos DB [request unit calculator](https://www.documentdb.com/capacityplanner).</span></span> <span data-ttu-id="77ff8-105">Läs mer i [uppskatta dina genomströmning måste](request-units.md#estimating-throughput-needs).</span><span class="sxs-lookup"><span data-stu-id="77ff8-105">Learn more in [Estimating your throughput needs](request-units.md#estimating-throughput-needs).</span></span>

![Genomströmning Kalkylatorn][5]

## <a name="introduction"></a><span data-ttu-id="77ff8-107">Introduktion</span><span class="sxs-lookup"><span data-stu-id="77ff8-107">Introduction</span></span>
<span data-ttu-id="77ff8-108">[Azure Cosmos-DB](https://azure.microsoft.com/services/cosmos-db/) är Microsofts globalt distribuerad flera modellen databas.</span><span class="sxs-lookup"><span data-stu-id="77ff8-108">[Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) is Microsoft's globally distributed multi-model database.</span></span> <span data-ttu-id="77ff8-109">Du inte har toorent virtuella datorer, distribuera programvara eller övervaka databaser med Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="77ff8-109">With Azure Cosmos DB, you don't have toorent virtual machines, deploy software, or monitor databases.</span></span> <span data-ttu-id="77ff8-110">Azure Cosmos-DB drivs och övervakas kontinuerligt av Microsoft översta engineers toodeliver world klassen tillgänglighet, prestanda och data protection.</span><span class="sxs-lookup"><span data-stu-id="77ff8-110">Azure Cosmos DB is operated and continuously monitored by Microsoft top engineers toodeliver world class availability, performance, and data protection.</span></span> <span data-ttu-id="77ff8-111">Du kan komma åt dina data med hjälp av API: er för ditt val som [DocumentDB SQL](documentdb-sql-query.md) (dokument), MongoDB (dokument), [Azure Table Storage](https://azure.microsoft.com/services/storage/tables/) (nyckelvärde) och [Gremlin](https://tinkerpop.apache.org/gremlin.html) (kurva) alla Inbyggt stöd för.</span><span class="sxs-lookup"><span data-stu-id="77ff8-111">You can access your data using APIs of your choice, as [DocumentDB SQL](documentdb-sql-query.md) (document), MongoDB (document), [Azure Table Storage](https://azure.microsoft.com/services/storage/tables/) (key-value), and [Gremlin](https://tinkerpop.apache.org/gremlin.html) (graph) are all natively supported.</span></span> <span data-ttu-id="77ff8-112">hello valuta Azure Cosmos DB är hello begära enhet (RU).</span><span class="sxs-lookup"><span data-stu-id="77ff8-112">hello currency of Azure Cosmos DB is hello Request Unit (RU).</span></span> <span data-ttu-id="77ff8-113">Med RUs behöver du inte tooreserve läsning och skrivning kapacitet eller etablera CPU, minne och IOPS.</span><span class="sxs-lookup"><span data-stu-id="77ff8-113">With RUs, you do not need tooreserve read/write capacities or provision CPU, Memory and IOPS.</span></span>

<span data-ttu-id="77ff8-114">Azure Cosmos-DB stöder ett antal API: er med olika åtgärder, från enkla läser och skriver toocomplex graph-frågor.</span><span class="sxs-lookup"><span data-stu-id="77ff8-114">Azure Cosmos DB supports a number of APIs with different operations ranging from simple reads and writes toocomplex graph queries.</span></span> <span data-ttu-id="77ff8-115">Eftersom inte alla begäranden som är lika med de tilldelas en normaliserade mängd **programbegäran** baserat på hello mängden beräkning krävs tooserve hello begäran.</span><span class="sxs-lookup"><span data-stu-id="77ff8-115">Since not all requests are equal, they are assigned a normalized quantity of **request units** based on hello amount of computation required tooserve hello request.</span></span> <span data-ttu-id="77ff8-116">Du kan spåra hello antalet frågeenheter som används av alla åtgärder i Azure Cosmos DB via en svarshuvud hello antal begäran för en åtgärd är deterministiska.</span><span class="sxs-lookup"><span data-stu-id="77ff8-116">hello number of request units for an operation is deterministic, and you can track hello number of request units consumed by any operation in Azure Cosmos DB via a response header.</span></span> 

<span data-ttu-id="77ff8-117">tooprovide förutsägbar prestanda, behöver du tooreserve dataflöde i enheter av 100 RU per sekund.</span><span class="sxs-lookup"><span data-stu-id="77ff8-117">tooprovide predictable performance, you need tooreserve throughput in units of 100 RU/second.</span></span> 

<span data-ttu-id="77ff8-118">När du har läst den här artikeln att du kunna tooanswer hello följande frågor:</span><span class="sxs-lookup"><span data-stu-id="77ff8-118">After reading this article, you'll be able tooanswer hello following questions:</span></span>  

* <span data-ttu-id="77ff8-119">Vad är programbegäran och begära avgifter?</span><span class="sxs-lookup"><span data-stu-id="77ff8-119">What are request units and request charges?</span></span>
* <span data-ttu-id="77ff8-120">Hur jag för att ange begäran enhet kapacitet för en samling?</span><span class="sxs-lookup"><span data-stu-id="77ff8-120">How do I specify request unit capacity for a collection?</span></span>
* <span data-ttu-id="77ff8-121">Hur jag beräkna måste mitt program begäran enhet?</span><span class="sxs-lookup"><span data-stu-id="77ff8-121">How do I estimate my application's request unit needs?</span></span>
* <span data-ttu-id="77ff8-122">Vad händer om jag överskrider begäran enhet kapacitet för en samling?</span><span class="sxs-lookup"><span data-stu-id="77ff8-122">What happens if I exceed request unit capacity for a collection?</span></span>

<span data-ttu-id="77ff8-123">Azure Cosmos DB är en databas med flera olika modeller, är det viktigt toonote att tooa samling dokument kallas för ett dokument API, diagram/nod för graph API och tabellen/entiteten för tabell-API.</span><span class="sxs-lookup"><span data-stu-id="77ff8-123">As Azure Cosmos DB is a multi-model database, it is important toonote that we will refer tooa collection/document for a document API, a graph/node for a graph API and a table/entity for table API.</span></span> <span data-ttu-id="77ff8-124">Genomströmning dokumentet kommer vi generalisera toohello begreppet behållare/artikel.</span><span class="sxs-lookup"><span data-stu-id="77ff8-124">Throughput this document we will generalize toohello concepts of container/item.</span></span>

## <a name="request-units-and-request-charges"></a><span data-ttu-id="77ff8-125">Frågeenheter och kostnader för begäran</span><span class="sxs-lookup"><span data-stu-id="77ff8-125">Request units and request charges</span></span>
<span data-ttu-id="77ff8-126">Azure Cosmos-DB ger snabb och förutsägbar prestanda av *reservera* resurser toosatisfy genomströmning för ditt program behöver.</span><span class="sxs-lookup"><span data-stu-id="77ff8-126">Azure Cosmos DB delivers fast, predictable performance by *reserving* resources toosatisfy your application's throughput needs.</span></span>  <span data-ttu-id="77ff8-127">Eftersom program läsa in och komma åt mönster ändras över tiden, Azure Cosmos DB kan du tooeasily öka eller minska hello reserverat dataflöde tillgängliga tooyour program.</span><span class="sxs-lookup"><span data-stu-id="77ff8-127">Because application load and access patterns change over time, Azure Cosmos DB allows you tooeasily increase or decrease hello amount of reserved throughput available tooyour application.</span></span>

<span data-ttu-id="77ff8-128">Med Azure Cosmos DB anges reserverat dataflöde som frågeenheter som bearbetades per sekund.</span><span class="sxs-lookup"><span data-stu-id="77ff8-128">With Azure Cosmos DB, reserved throughput is specified in terms of request units processing per second.</span></span> <span data-ttu-id="77ff8-129">Du kan se frågeenheter som dataflöde valuta, där du *reservera* en mängd garanteras frågeenheter tillgängliga tooyour program på grundval av per sekund.</span><span class="sxs-lookup"><span data-stu-id="77ff8-129">You can think of request units as throughput currency, whereby you *reserve* an amount of guaranteed request units available tooyour application on per second basis.</span></span>  <span data-ttu-id="77ff8-130">Varje åtgärd i Azure DB som Cosmos - skriva ett dokument, utför en fråga, uppdatera ett dokument - förbrukar CPU, minne och IOPS.</span><span class="sxs-lookup"><span data-stu-id="77ff8-130">Each operation in Azure Cosmos DB - writing a document, performing a query, updating a document - consumes CPU, memory, and IOPS.</span></span>  <span data-ttu-id="77ff8-131">Det vill säga varje åtgärd har en *begära kostnad*, som uttrycks i *programbegäran*.</span><span class="sxs-lookup"><span data-stu-id="77ff8-131">That is, each operation incurs a *request charge*, which is expressed in *request units*.</span></span>  <span data-ttu-id="77ff8-132">Förstå hello faktorer som påverkar begäran enhet kostnader, tillsammans med ditt program genomströmning krav, kan du toorun tillämpningsprogrammet som kostnad effektiv som möjligt.</span><span class="sxs-lookup"><span data-stu-id="77ff8-132">Understanding hello factors which impact request unit charges, along with your application's throughput requirements, enables you toorun your application as cost effectively as possible.</span></span> <span data-ttu-id="77ff8-133">Hej frågeutforskaren är också en fantastiska verktyget tootest hello kärnan i en fråga.</span><span class="sxs-lookup"><span data-stu-id="77ff8-133">hello query explorer is also a wonderful tool tootest hello core of a query.</span></span>

<span data-ttu-id="77ff8-134">Vi rekommenderar att komma igång genom att titta på hello följande video, där Aravind Ramachandran förklarar frågeenheter och förutsägbar prestanda med Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="77ff8-134">We recommend getting started by watching hello following video, where Aravind Ramachandran explains request units and predictable performance with Azure Cosmos DB.</span></span>

> [!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/Predictable-Performance-with-DocumentDB/player]
> 
> 

## <a name="specifying-request-unit-capacity-in-azure-cosmos-db"></a><span data-ttu-id="77ff8-135">Ange kapacitet för begäran-enhet i Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="77ff8-135">Specifying request unit capacity in Azure Cosmos DB</span></span>
<span data-ttu-id="77ff8-136">När du startar en ny samling, tabell eller diagram, kan du ange hello många frågeenheter per sekund (RU per sekund) som du vill reserverade.</span><span class="sxs-lookup"><span data-stu-id="77ff8-136">When starting a new collection, table or graph, you specify hello number of request units per second (RU per second) you want reserved.</span></span> <span data-ttu-id="77ff8-137">Baserat på etablerat dataflöde för hello Azure Cosmos DB allokerar fysiska partitioner toohost samlingen och delningar/rebalances data över partitioner när det växer.</span><span class="sxs-lookup"><span data-stu-id="77ff8-137">Based on hello provisioned throughput, Azure Cosmos DB allocates physical partitions toohost your collection and splits/rebalances data across partitions as it grows.</span></span>

<span data-ttu-id="77ff8-138">Azure Cosmos-DB kräver en nyckel toobe partition anges när en samling är etablerad med 2 500 frågeenheter eller högre.</span><span class="sxs-lookup"><span data-stu-id="77ff8-138">Azure Cosmos DB requires a partition key toobe specified when a collection is provisioned with 2,500 request units or higher.</span></span> <span data-ttu-id="77ff8-139">En partitionsnyckel är också nödvändig tooscale din samling genomflöde utöver 2 500 frågeenheter i hello framtida.</span><span class="sxs-lookup"><span data-stu-id="77ff8-139">A partition key is also required tooscale your collection's throughput beyond 2,500 request units in hello future.</span></span> <span data-ttu-id="77ff8-140">Därför rekommenderas tooconfigure en [partitionsnyckel](partition-data.md) när du skapar en behållare oavsett din första genomflöde.</span><span class="sxs-lookup"><span data-stu-id="77ff8-140">Therefore, it is highly recommended tooconfigure a [partition key](partition-data.md) when creating a container regardless of your initial throughput.</span></span> <span data-ttu-id="77ff8-141">Eftersom dina data kan ha toobe dela upp på flera partitioner, är det nödvändigt toopick en partitionsnyckel som har en hög kardinalitet (100 toomillions distinkta värden) så att dina graph-samling/tabell och begäranden kan skalas enhetligt med Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="77ff8-141">Since your data might have toobe split across multiple partitions, it is necessary toopick a partition key that has a high cardinality (100 toomillions of distinct values) so that your collection/table/graph and requests can be scaled uniformly by Azure Cosmos DB.</span></span> 

> [!NOTE]
> <span data-ttu-id="77ff8-142">En partitionsnyckel är en logisk gräns och inte en fysisk.</span><span class="sxs-lookup"><span data-stu-id="77ff8-142">A partition key is a logical boundary, and not a physical one.</span></span> <span data-ttu-id="77ff8-143">Därför behöver inte toolimit hello antalet distinkta partitionsnyckelvärden.</span><span class="sxs-lookup"><span data-stu-id="77ff8-143">Therefore, you do not need toolimit hello number of distinct partition key values.</span></span> <span data-ttu-id="77ff8-144">Det är i själva verket bättre toohave tydligare partitions nyckelvärden än mindre, eftersom Azure Cosmos DB har flera alternativ för belastningsutjämning.</span><span class="sxs-lookup"><span data-stu-id="77ff8-144">It is in fact better toohave more distinct partition key values than less, as Azure Cosmos DB has more load balancing options.</span></span>

<span data-ttu-id="77ff8-145">Här är ett kodfragment för att skapa en samling med 3 000 begäran enheter per andra med hello .NET SDK:</span><span class="sxs-lookup"><span data-stu-id="77ff8-145">Here is a code snippet for creating a collection with 3,000 request units per second using hello .NET SDK:</span></span>

```csharp
DocumentCollection myCollection = new DocumentCollection();
myCollection.Id = "coll";
myCollection.PartitionKey.Paths.Add("/deviceId");

await client.CreateDocumentCollectionAsync(
    UriFactory.CreateDatabaseUri("db"),
    myCollection,
    new RequestOptions { OfferThroughput = 3000 });
```

<span data-ttu-id="77ff8-146">Azure Cosmos-DB fungerar på en modell för reservation på genomflöde.</span><span class="sxs-lookup"><span data-stu-id="77ff8-146">Azure Cosmos DB operates on a reservation model on throughput.</span></span> <span data-ttu-id="77ff8-147">Det vill säga du debiteras för hello mängden genomströmning *reserverade*, oavsett hur mycket av den genomströmningen är aktivt *används*.</span><span class="sxs-lookup"><span data-stu-id="77ff8-147">That is, you are billed for hello amount of throughput *reserved*, regardless of how much of that throughput is actively *used*.</span></span> <span data-ttu-id="77ff8-148">Hej mängden reserverad RUs via SDK: er som ditt program load, data och användning mönster du kan enkelt skala upp eller ned eller med hjälp av hello [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="77ff8-148">As your application's load, data, and usage patterns change you can easily scale up and down hello amount of reserved RUs through SDKs or using hello [Azure Portal](https://portal.azure.com).</span></span>

<span data-ttu-id="77ff8-149">Varje samling/tabellen/diagram mappas tooan `Offer` resurs i Azure Cosmos DB som innehåller metadata om hello etablerat dataflöde.</span><span class="sxs-lookup"><span data-stu-id="77ff8-149">Each collection/table/graph are mapped tooan `Offer` resource in Azure Cosmos DB, which has metadata about hello provisioned throughput.</span></span> <span data-ttu-id="77ff8-150">Du kan ändra hello allokerade genomströmning genom att slå upp hello motsvarande erbjudande resurs för en behållare och sedan uppdaterar med hello nya genomströmning värdet.</span><span class="sxs-lookup"><span data-stu-id="77ff8-150">You can change hello allocated throughput by looking up hello corresponding offer resource for a container, then updating it with hello new throughput value.</span></span> <span data-ttu-id="77ff8-151">Här är ett kodfragment för att ändra hello genomströmningen i en samling too5, 000 frågeenheter per andra med hello .NET SDK:</span><span class="sxs-lookup"><span data-stu-id="77ff8-151">Here is a code snippet for changing hello throughput of a collection too5,000 request units per second using hello .NET SDK:</span></span>

```csharp
// Fetch hello resource toobe updated
Offer offer = client.CreateOfferQuery()
                .Where(r => r.ResourceLink == collection.SelfLink)    
                .AsEnumerable()
                .SingleOrDefault();

// Set hello throughput too5000 request units per second
offer = new OfferV2(offer, 5000);

// Now persist these changes toohello database by replacing hello original resource
await client.ReplaceOfferAsync(offer);
```

<span data-ttu-id="77ff8-152">Det finns ingen inverkan toohello tillgänglighet för din behållare när du ändrar hello genomflöde.</span><span class="sxs-lookup"><span data-stu-id="77ff8-152">There is no impact toohello availability of your container when you change hello throughput.</span></span> <span data-ttu-id="77ff8-153">Hello nya reserverat dataflöde är vanligtvis effektiva i sekunder för programmet hello nya genomströmning.</span><span class="sxs-lookup"><span data-stu-id="77ff8-153">Typically hello new reserved throughput is effective within seconds on application of hello new throughput.</span></span>

## <a name="request-unit-considerations"></a><span data-ttu-id="77ff8-154">Överväganden för begäran-enhet</span><span class="sxs-lookup"><span data-stu-id="77ff8-154">Request unit considerations</span></span>
<span data-ttu-id="77ff8-155">När du uppskattar hello antalet begäran enheter tooreserve för Azure DB som Cosmos-behållaren är viktiga tootake hello följande variabler i beräkningen:</span><span class="sxs-lookup"><span data-stu-id="77ff8-155">When estimating hello number of request units tooreserve for your Azure Cosmos DB container, it is important tootake hello following variables into consideration:</span></span>

* <span data-ttu-id="77ff8-156">**Objektet storlek**.</span><span class="sxs-lookup"><span data-stu-id="77ff8-156">**Item size**.</span></span> <span data-ttu-id="77ff8-157">Då ökar storleken hello enheter används tooread eller skriva hello data ökar också.</span><span class="sxs-lookup"><span data-stu-id="77ff8-157">As size increases hello units consumed tooread or write hello data will also increase.</span></span>
* <span data-ttu-id="77ff8-158">**Objektet antal egenskaper**.</span><span class="sxs-lookup"><span data-stu-id="77ff8-158">**Item property count**.</span></span> <span data-ttu-id="77ff8-159">Under förutsättning att standard indexering av alla egenskaper, hello enheter används toowrite dokument/nod/ntity ökar när hello egenskapen Antal ökar.</span><span class="sxs-lookup"><span data-stu-id="77ff8-159">Assuming default indexing of all properties, hello units consumed toowrite a document/node/ntity will increase as hello property count increases.</span></span>
* <span data-ttu-id="77ff8-160">**Datakonsekvens**.</span><span class="sxs-lookup"><span data-stu-id="77ff8-160">**Data consistency**.</span></span> <span data-ttu-id="77ff8-161">När du använder data konsekvensnivåer av starka eller begränsas föråldrad, kommer ytterligare enheter att förbrukade tooread objekt.</span><span class="sxs-lookup"><span data-stu-id="77ff8-161">When using data consistency levels of Strong or Bounded Staleness, additional units will be consumed tooread items.</span></span>
* <span data-ttu-id="77ff8-162">**Indexerade egenskaper**.</span><span class="sxs-lookup"><span data-stu-id="77ff8-162">**Indexed properties**.</span></span> <span data-ttu-id="77ff8-163">En princip för index varje behållare avgör vilka egenskaper som indexeras som standard.</span><span class="sxs-lookup"><span data-stu-id="77ff8-163">An index policy on each container determines which properties are indexed by default.</span></span> <span data-ttu-id="77ff8-164">Du kan minska konsumtion din begäran enheten genom att begränsa hello antal indexerade egenskaper eller genom att aktivera lazy indexering.</span><span class="sxs-lookup"><span data-stu-id="77ff8-164">You can reduce your request unit consumption by limiting hello number of indexed properties or by enabling lazy indexing.</span></span>
* <span data-ttu-id="77ff8-165">**Dokumentindexering**.</span><span class="sxs-lookup"><span data-stu-id="77ff8-165">**Document indexing**.</span></span> <span data-ttu-id="77ff8-166">Som standard varje objekt indexeras automatiskt kommer du att använda färre frågeenheter om du väljer inte tooindex några av dina objekt.</span><span class="sxs-lookup"><span data-stu-id="77ff8-166">By default each item is automatically indexed, you will consume fewer request units if you choose not tooindex some of your items.</span></span>
* <span data-ttu-id="77ff8-167">**Fråga mönster**.</span><span class="sxs-lookup"><span data-stu-id="77ff8-167">**Query patterns**.</span></span> <span data-ttu-id="77ff8-168">hello komplexiteten i en fråga påverkar hur många enheter som begär förbrukas för en åtgärd.</span><span class="sxs-lookup"><span data-stu-id="77ff8-168">hello complexity of a query impacts how many Request Units are consumed for an operation.</span></span> <span data-ttu-id="77ff8-169">hello antalet predikat, uppbyggnad hello predikat, projektioner, antalet UDF: er och hello storleken på hello källa datauppsättning alla påverka hello kostnaden för fråga åtgärder.</span><span class="sxs-lookup"><span data-stu-id="77ff8-169">hello number of predicates, nature of hello predicates, projections, number of UDFs, and hello size of hello source data set all influence hello cost of query operations.</span></span>
* <span data-ttu-id="77ff8-170">**Script användning**.</span><span class="sxs-lookup"><span data-stu-id="77ff8-170">**Script usage**.</span></span>  <span data-ttu-id="77ff8-171">Precis som med frågor, lagrade procedurer och utlösare kan du använda frågeenheter utifrån hello komplext hello åtgärder som utförs.</span><span class="sxs-lookup"><span data-stu-id="77ff8-171">As with queries, stored procedures and triggers consume request units based on hello complexity of hello operations being performed.</span></span> <span data-ttu-id="77ff8-172">När du utvecklar programmet inspektera hello begäran kostnad huvud toobetter förstå hur varje åtgärd förbrukar begäran enhet kapacitet.</span><span class="sxs-lookup"><span data-stu-id="77ff8-172">As you develop your application, inspect hello request charge header toobetter understand how each operation is consuming request unit capacity.</span></span>

## <a name="estimating-throughput-needs"></a><span data-ttu-id="77ff8-173">Uppskattning behov för genomströmning</span><span class="sxs-lookup"><span data-stu-id="77ff8-173">Estimating throughput needs</span></span>
<span data-ttu-id="77ff8-174">En begäran-enhet är ett normaliserat mått på kostnaden för begäranden.</span><span class="sxs-lookup"><span data-stu-id="77ff8-174">A request unit is a normalized measure of request processing cost.</span></span> <span data-ttu-id="77ff8-175">En enskild begäran enhet representerar hello bearbetning kapacitet som krävs tooread (via self länk eller id) en enda 1KB artikel bestående av 10 unika värden (exklusive Systemegenskaper).</span><span class="sxs-lookup"><span data-stu-id="77ff8-175">A single request unit represents hello processing capacity required tooread (via self link or id) a single 1KB item consisting of 10 unique property values (excluding system properties).</span></span> <span data-ttu-id="77ff8-176">En begäran toocreate (insert), ersätta eller ta bort hello samma objekt förbrukar mer bearbetning från hello-tjänsten och därmed mer programbegäran.</span><span class="sxs-lookup"><span data-stu-id="77ff8-176">A request toocreate (insert), replace or delete hello same item will consume more processing from hello service and thereby more request units.</span></span>   

> [!NOTE]
> <span data-ttu-id="77ff8-177">hello baslinje för begäran om 1 enhet för en 1KB objekt motsvarar tooa enkelt få self länk eller hello objekt-id.</span><span class="sxs-lookup"><span data-stu-id="77ff8-177">hello baseline of 1 request unit for a 1KB item corresponds tooa simple GET by self link or id of hello item.</span></span>
> 
> 

<span data-ttu-id="77ff8-178">Här är till exempel en tabell som visar hur många begära tooprovision för enheter på tre olika objekt-storlekar (1KB 4KB och 64KB) och två olika prestandanivåer (500 läsningar per sekund + 100 skrivningar per sekund och 500 läsningar per sekund + 500 skrivningar per sekund).</span><span class="sxs-lookup"><span data-stu-id="77ff8-178">For example, here's a table that shows how many request units tooprovision at three different item sizes (1KB, 4KB, and 64KB) and at two different performance levels (500 reads/second + 100 writes/second and 500 reads/second + 500 writes/second).</span></span> <span data-ttu-id="77ff8-179">hello datakonsekvens konfigurerades på sessionen och hello indexering princip har angetts tooNone.</span><span class="sxs-lookup"><span data-stu-id="77ff8-179">hello data consistency was configured at Session, and hello indexing policy was set tooNone.</span></span>

<table border="0" cellspacing="0" cellpadding="0">
    <tbody>
        <tr>
            <td valign="top"><p><span data-ttu-id="77ff8-180"><strong>Objektets storlek</strong></span><span class="sxs-lookup"><span data-stu-id="77ff8-180"><strong>Item size</strong></span></span></p></td>
            <td valign="top"><p><span data-ttu-id="77ff8-181"><strong>Läsningar per sekund</strong></span><span class="sxs-lookup"><span data-stu-id="77ff8-181"><strong>Reads/second</strong></span></span></p></td>
            <td valign="top"><p><span data-ttu-id="77ff8-182"><strong>Skrivningar per sekund</strong></span><span class="sxs-lookup"><span data-stu-id="77ff8-182"><strong>Writes/second</strong></span></span></p></td>
            <td valign="top"><p><span data-ttu-id="77ff8-183"><strong>Enheter för programbegäran</strong></span><span class="sxs-lookup"><span data-stu-id="77ff8-183"><strong>Request units</strong></span></span></p></td>
        </tr>
        <tr>
            <td valign="top"><p><span data-ttu-id="77ff8-184">1 KB</span><span class="sxs-lookup"><span data-stu-id="77ff8-184">1 KB</span></span></p></td>
            <td valign="top"><p><span data-ttu-id="77ff8-185">500</span><span class="sxs-lookup"><span data-stu-id="77ff8-185">500</span></span></p></td>
            <td valign="top"><p><span data-ttu-id="77ff8-186">100</span><span class="sxs-lookup"><span data-stu-id="77ff8-186">100</span></span></p></td>
            <td valign="top"><p><span data-ttu-id="77ff8-187">(500 * 1) + (100 * 5) = 1 000 RU/s</span><span class="sxs-lookup"><span data-stu-id="77ff8-187">(500 * 1) + (100 * 5) = 1,000 RU/s</span></span></p></td>
        </tr>
        <tr>
            <td valign="top"><p><span data-ttu-id="77ff8-188">1 KB</span><span class="sxs-lookup"><span data-stu-id="77ff8-188">1 KB</span></span></p></td>
            <td valign="top"><p><span data-ttu-id="77ff8-189">500</span><span class="sxs-lookup"><span data-stu-id="77ff8-189">500</span></span></p></td>
            <td valign="top"><p><span data-ttu-id="77ff8-190">500</span><span class="sxs-lookup"><span data-stu-id="77ff8-190">500</span></span></p></td>
            <td valign="top"><p><span data-ttu-id="77ff8-191">(500 * 1) + (500 * 5) = 3 000 RU/s</span><span class="sxs-lookup"><span data-stu-id="77ff8-191">(500 * 1) + (500 * 5) = 3,000 RU/s</span></span></p></td>
        </tr>
        <tr>
            <td valign="top"><p><span data-ttu-id="77ff8-192">4 KB</span><span class="sxs-lookup"><span data-stu-id="77ff8-192">4 KB</span></span></p></td>
            <td valign="top"><p><span data-ttu-id="77ff8-193">500</span><span class="sxs-lookup"><span data-stu-id="77ff8-193">500</span></span></p></td>
            <td valign="top"><p><span data-ttu-id="77ff8-194">100</span><span class="sxs-lookup"><span data-stu-id="77ff8-194">100</span></span></p></td>
            <td valign="top"><p><span data-ttu-id="77ff8-195">(500 * 1.3) + (100 * 7) = 1,350 RU/s</span><span class="sxs-lookup"><span data-stu-id="77ff8-195">(500 * 1.3) + (100 * 7) = 1,350 RU/s</span></span></p></td>
        </tr>
        <tr>
            <td valign="top"><p><span data-ttu-id="77ff8-196">4 KB</span><span class="sxs-lookup"><span data-stu-id="77ff8-196">4 KB</span></span></p></td>
            <td valign="top"><p><span data-ttu-id="77ff8-197">500</span><span class="sxs-lookup"><span data-stu-id="77ff8-197">500</span></span></p></td>
            <td valign="top"><p><span data-ttu-id="77ff8-198">500</span><span class="sxs-lookup"><span data-stu-id="77ff8-198">500</span></span></p></td>
            <td valign="top"><p><span data-ttu-id="77ff8-199">(500 * 1.3) + (500 * 7) = 4,150 RU/s</span><span class="sxs-lookup"><span data-stu-id="77ff8-199">(500 * 1.3) + (500 * 7) = 4,150 RU/s</span></span></p></td>
        </tr>
        <tr>
            <td valign="top"><p><span data-ttu-id="77ff8-200">64 kB</span><span class="sxs-lookup"><span data-stu-id="77ff8-200">64 KB</span></span></p></td>
            <td valign="top"><p><span data-ttu-id="77ff8-201">500</span><span class="sxs-lookup"><span data-stu-id="77ff8-201">500</span></span></p></td>
            <td valign="top"><p><span data-ttu-id="77ff8-202">100</span><span class="sxs-lookup"><span data-stu-id="77ff8-202">100</span></span></p></td>
            <td valign="top"><p><span data-ttu-id="77ff8-203">(500 * 10) + (100 * 48) = 9,800 RU/s</span><span class="sxs-lookup"><span data-stu-id="77ff8-203">(500 * 10) + (100 * 48) = 9,800 RU/s</span></span></p></td>
        </tr>
        <tr>
            <td valign="top"><p><span data-ttu-id="77ff8-204">64 kB</span><span class="sxs-lookup"><span data-stu-id="77ff8-204">64 KB</span></span></p></td>
            <td valign="top"><p><span data-ttu-id="77ff8-205">500</span><span class="sxs-lookup"><span data-stu-id="77ff8-205">500</span></span></p></td>
            <td valign="top"><p><span data-ttu-id="77ff8-206">500</span><span class="sxs-lookup"><span data-stu-id="77ff8-206">500</span></span></p></td>
            <td valign="top"><p><span data-ttu-id="77ff8-207">(500 * 10) + (500 * 48) = 29,000 RU/s</span><span class="sxs-lookup"><span data-stu-id="77ff8-207">(500 * 10) + (500 * 48) = 29,000 RU/s</span></span></p></td>
        </tr>
    </tbody>
</table>

### <a name="use-hello-request-unit-calculator"></a><span data-ttu-id="77ff8-208">Använda hello begäran enhet Kalkylatorn</span><span class="sxs-lookup"><span data-stu-id="77ff8-208">Use hello request unit calculator</span></span>
<span data-ttu-id="77ff8-209">toohelp kunder bra justera sina genomströmning uppskattningar, finns det en webbaserad [begäran enhet Kalkylatorn](https://www.documentdb.com/capacityplanner) toohelp uppskattning hello begäran enhet krav för vanliga åtgärder, inklusive:</span><span class="sxs-lookup"><span data-stu-id="77ff8-209">toohelp customers fine tune their throughput estimations, there is a web based [request unit calculator](https://www.documentdb.com/capacityplanner) toohelp estimate hello request unit requirements for typical operations, including:</span></span>

* <span data-ttu-id="77ff8-210">Objektet skapar (skriver)</span><span class="sxs-lookup"><span data-stu-id="77ff8-210">Item creates (writes)</span></span>
* <span data-ttu-id="77ff8-211">Läsningar av objektet</span><span class="sxs-lookup"><span data-stu-id="77ff8-211">Item reads</span></span>
* <span data-ttu-id="77ff8-212">Tar bort objekt</span><span class="sxs-lookup"><span data-stu-id="77ff8-212">Item deletes</span></span>
* <span data-ttu-id="77ff8-213">Objektet uppdateringar</span><span class="sxs-lookup"><span data-stu-id="77ff8-213">Item updates</span></span>

<span data-ttu-id="77ff8-214">hello verktyget omfattar också stöd för att uppskatta lagringsbehov baserat på hello exempelobjekt som du anger.</span><span class="sxs-lookup"><span data-stu-id="77ff8-214">hello tool also includes support for estimating data storage needs based on hello sample items you provide.</span></span>

<span data-ttu-id="77ff8-215">Verktyget hello är enkel:</span><span class="sxs-lookup"><span data-stu-id="77ff8-215">Using hello tool is simple:</span></span>

1. <span data-ttu-id="77ff8-216">Ladda upp en eller flera representativa objekt.</span><span class="sxs-lookup"><span data-stu-id="77ff8-216">Upload one or more representative items.</span></span>
   
    ![Överför objekt toohello begäran enhet Kalkylatorn][2]
2. <span data-ttu-id="77ff8-218">krav för datalagring tooestimate, ange hello Totalt antal objekt du förväntar dig toostore.</span><span class="sxs-lookup"><span data-stu-id="77ff8-218">tooestimate data storage requirements, enter hello total number of items you expect toostore.</span></span>
3. <span data-ttu-id="77ff8-219">Ange hello antal objekt skapa, läsa, uppdatera och delete-åtgärder som du behöver (på grundval av per sekund).</span><span class="sxs-lookup"><span data-stu-id="77ff8-219">Enter hello number of items create, read, update, and delete operations you require (on a per-second basis).</span></span> <span data-ttu-id="77ff8-220">tooestimate hello begäran enhet avgifter för objektet uppdateringsåtgärder överför en kopia av hello exempel objekt från steg 1 ovan som innehåller vanliga fältuppdateringar.</span><span class="sxs-lookup"><span data-stu-id="77ff8-220">tooestimate hello request unit charges of item update operations, upload a copy of hello sample item from step 1 above that includes typical field updates.</span></span>  <span data-ttu-id="77ff8-221">Till exempel om objektet uppdateringar ändra vanligtvis två egenskaper med namnet lastLogin och userVisits och sedan enkelt kopiera hello exempel objekt, uppdatera hello värden för dessa två egenskaper och överför hello kopieras objektet.</span><span class="sxs-lookup"><span data-stu-id="77ff8-221">For example, if item updates typically modify two properties named lastLogin and userVisits, then simply copy hello sample item, update hello values for those two properties, and upload hello copied item.</span></span>
   
    ![Ange kraven för genomflöde i hello begäran enhet Kalkylatorn][3]
4. <span data-ttu-id="77ff8-223">Klicka på Beräkna och undersöka hello resultat.</span><span class="sxs-lookup"><span data-stu-id="77ff8-223">Click calculate and examine hello results.</span></span>
   
    ![Begära enhet Kalkylatorn resultat][4]

> [!NOTE]
> <span data-ttu-id="77ff8-225">Om du har objekttyper som varierar kraftigt vad gäller storlek och hello antal indexerade egenskaper, sedan överföra ett exempel på varje *typen* för vanliga objekt toohello verktyget och sedan beräkna hello resultat.</span><span class="sxs-lookup"><span data-stu-id="77ff8-225">If you have item types which will differ dramatically in terms of size and hello number of indexed properties, then upload a sample of each *type* of typical item toohello tool and then calculate hello results.</span></span>
> 
> 

### <a name="use-hello-azure-cosmos-db-request-charge-response-header"></a><span data-ttu-id="77ff8-226">Använd hello Azure Cosmos DB begäran kostnad svarshuvud</span><span class="sxs-lookup"><span data-stu-id="77ff8-226">Use hello Azure Cosmos DB request charge response header</span></span>
<span data-ttu-id="77ff8-227">Varje svar från hello Azure Cosmos DB tjänsten omfattar en anpassad rubrik (`x-ms-request-charge`) som innehåller hello frågeenheter som används för hello-begäran.</span><span class="sxs-lookup"><span data-stu-id="77ff8-227">Every response from hello Azure Cosmos DB service includes a custom header (`x-ms-request-charge`) that contains hello request units consumed for hello request.</span></span> <span data-ttu-id="77ff8-228">Det här sidhuvudet är också tillgängligt via hello Azure Cosmos DB SDK: er.</span><span class="sxs-lookup"><span data-stu-id="77ff8-228">This header is also accessible through hello Azure Cosmos DB SDKs.</span></span> <span data-ttu-id="77ff8-229">I hello .NET SDK är RequestCharge en egenskap hos hello ResourceResponse objekt.</span><span class="sxs-lookup"><span data-stu-id="77ff8-229">In hello .NET SDK, RequestCharge is a property of hello ResourceResponse object.</span></span>  <span data-ttu-id="77ff8-230">För frågor visar hello Azure Cosmos DB Frågeutforskaren i hello Azure-portalen begäran kostnad information om utförs frågor.</span><span class="sxs-lookup"><span data-stu-id="77ff8-230">For queries, hello Azure Cosmos DB Query Explorer in hello Azure portal provides request charge information for executed queries.</span></span>

![Undersöka RU tillägg i hello Frågeutforskaren][1]

<span data-ttu-id="77ff8-232">Med detta i åtanke, en metod för att uppskatta reserverat dataflöde som krävs av programmet hello mycket kostar toorecord hello begäran enhet som är associerade med vanliga åtgärder som körs mot ett representativt objekt som används av ditt program och sedan uppskatta hello antal åtgärder vill du utföra varje sekund.</span><span class="sxs-lookup"><span data-stu-id="77ff8-232">With this in mind, one method for estimating hello amount of reserved throughput required by your application is toorecord hello request unit charge associated with running typical operations against a representative item used by your application and then estimating hello number of operations you anticipate performing each second.</span></span>  <span data-ttu-id="77ff8-233">Vara säker på att toomeasure och innehåller vanliga frågor och Azure Cosmos DB skript samt användning.</span><span class="sxs-lookup"><span data-stu-id="77ff8-233">Be sure toomeasure and include typical queries and Azure Cosmos DB script usage as well.</span></span>

> [!NOTE]
> <span data-ttu-id="77ff8-234">Om du har objekttyper som varierar kraftigt vad gäller storlek och hello antal indexerade egenskaper kan sedan registrera hello tillämpliga åtgärden begäran enhetsavgift som associeras med varje *typen* för vanliga objekt.</span><span class="sxs-lookup"><span data-stu-id="77ff8-234">If you have item types which will differ dramatically in terms of size and hello number of indexed properties, then record hello applicable operation request unit charge associated with each *type* of typical item.</span></span>
> 
> 

<span data-ttu-id="77ff8-235">Exempel:</span><span class="sxs-lookup"><span data-stu-id="77ff8-235">For example:</span></span>

1. <span data-ttu-id="77ff8-236">Registrera hello begäran enhet kostnad för att skapa (Infoga) en typisk objektet.</span><span class="sxs-lookup"><span data-stu-id="77ff8-236">Record hello request unit charge of creating (inserting) a typical item.</span></span> 
2. <span data-ttu-id="77ff8-237">Registrera hello begäran enhet kostnad läsa vanliga objekt.</span><span class="sxs-lookup"><span data-stu-id="77ff8-237">Record hello request unit charge of reading a typical item.</span></span>
3. <span data-ttu-id="77ff8-238">Registrera hello begäran enhet kostnad för att uppdatera en typisk objektet.</span><span class="sxs-lookup"><span data-stu-id="77ff8-238">Record hello request unit charge of updating a typical item.</span></span>
4. <span data-ttu-id="77ff8-239">Registrera hello begäran enhetsavgift för vanliga, vanliga artikel frågor.</span><span class="sxs-lookup"><span data-stu-id="77ff8-239">Record hello request unit charge of typical, common item queries.</span></span>
5. <span data-ttu-id="77ff8-240">Registrera hello begäran enhet kostnad för alla anpassade skript (lagrade procedurer, utlösare, användardefinierade funktioner) kan användas av programmet hello</span><span class="sxs-lookup"><span data-stu-id="77ff8-240">Record hello request unit charge of any custom scripts (stored procedures, triggers, user-defined functions) leveraged by hello application</span></span>
6. <span data-ttu-id="77ff8-241">Beräkna hello krävs begäran enheter som anges hello Uppskattat antal åtgärder du förutse toorun varje sekund.</span><span class="sxs-lookup"><span data-stu-id="77ff8-241">Calculate hello required request units given hello estimated number of operations you anticipate toorun each second.</span></span>

### <span data-ttu-id="77ff8-242"><a id="GetLastRequestStatistics"></a>Använd API för Mongodb's GetLastRequestStatistics kommando</span><span class="sxs-lookup"><span data-stu-id="77ff8-242"><a id="GetLastRequestStatistics"></a>Use API for MongoDB's GetLastRequestStatistics command</span></span>
<span data-ttu-id="77ff8-243">API: et för MongoDB stöder ett anpassat kommando *getLastRequestStatistics*, för att hämta hello begäran kostnad för angivna åtgärder.</span><span class="sxs-lookup"><span data-stu-id="77ff8-243">API for MongoDB supports a custom command, *getLastRequestStatistics*, for retrieving hello request charge for specified operations.</span></span>

<span data-ttu-id="77ff8-244">Till exempel köra hello åtgärden som du vill tooverify hello begäran kostnad för i hello Mongo-gränssnittet.</span><span class="sxs-lookup"><span data-stu-id="77ff8-244">For example, in hello Mongo Shell, execute hello operation you want tooverify hello request charge for.</span></span>
```
> db.sample.find()
```

<span data-ttu-id="77ff8-245">Sedan köra kommandot hello *getLastRequestStatistics*.</span><span class="sxs-lookup"><span data-stu-id="77ff8-245">Next, execute hello command *getLastRequestStatistics*.</span></span>
```
> db.runCommand({getLastRequestStatistics: 1})
{
    "_t": "GetRequestStatisticsResponse",
    "ok": 1,
    "CommandName": "OP_QUERY",
    "RequestCharge": 2.48,
    "RequestDurationInMilliSeconds" : 4.0048
}
```

<span data-ttu-id="77ff8-246">Med detta i åtanke, en metod för att uppskatta reserverat dataflöde som krävs av programmet hello mycket kostar toorecord hello begäran enhet som är associerade med vanliga åtgärder som körs mot ett representativt objekt som används av ditt program och sedan uppskatta hello antal åtgärder vill du utföra varje sekund.</span><span class="sxs-lookup"><span data-stu-id="77ff8-246">With this in mind, one method for estimating hello amount of reserved throughput required by your application is toorecord hello request unit charge associated with running typical operations against a representative item used by your application and then estimating hello number of operations you anticipate performing each second.</span></span>

> [!NOTE]
> <span data-ttu-id="77ff8-247">Om du har objekttyper som varierar kraftigt vad gäller storlek och hello antal indexerade egenskaper kan sedan registrera hello tillämpliga åtgärden begäran enhetsavgift som associeras med varje *typen* för vanliga objekt.</span><span class="sxs-lookup"><span data-stu-id="77ff8-247">If you have item types which will differ dramatically in terms of size and hello number of indexed properties, then record hello applicable operation request unit charge associated with each *type* of typical item.</span></span>
> 
> 

## <a name="use-api-for-mongodbs-portal-metrics"></a><span data-ttu-id="77ff8-248">Använd API för Mongodb's portal mätvärden</span><span class="sxs-lookup"><span data-stu-id="77ff8-248">Use API for MongoDB's portal metrics</span></span>
<span data-ttu-id="77ff8-249">Hej enklaste sättet tooget en bra uppskattning av begäran enhet avgifter för ditt API för MongoDB-databas är toouse hello [Azure-portalen](https://portal.azure.com) mått.</span><span class="sxs-lookup"><span data-stu-id="77ff8-249">hello simplest way tooget a good estimation of request unit charges for your API for MongoDB database is toouse hello [Azure portal](https://portal.azure.com) metrics.</span></span> <span data-ttu-id="77ff8-250">Med hello *antal begäranden* och *begäran kostnad* diagram, du kan få en uppskattning av hur många frågeenheter varje åtgärd är förbrukar och hur många frågeenheter de förbruka relativa tooone en annan.</span><span class="sxs-lookup"><span data-stu-id="77ff8-250">With hello *Number of requests* and *Request Charge* charts, you can get an estimation of how many request units each operation is consuming and how many request units they consume relative tooone another.</span></span>

![API för MongoDB portal mått][6]

## <a name="a-request-unit-estimation-example"></a><span data-ttu-id="77ff8-252">Exempel på beräkning av en begäran</span><span class="sxs-lookup"><span data-stu-id="77ff8-252">A request unit estimation example</span></span>
<span data-ttu-id="77ff8-253">Överväg följande ~ 1KB dokumentet hello:</span><span class="sxs-lookup"><span data-stu-id="77ff8-253">Consider hello following ~1KB document:</span></span>

```json
{
 "id": "08259",
  "description": "Cereals ready-to-eat, KELLOGG, KELLOGG'S CRISPIX",
  "tags": [
    {
      "name": "cereals ready-to-eat"
    },
    {
      "name": "kellogg"
    },
    {
      "name": "kellogg's crispix"
    }
  ],
  "version": 1,
  "commonName": "Includes USDA Commodity B855",
  "manufacturerName": "Kellogg, Co.",
  "isFromSurvey": false,
  "foodGroup": "Breakfast Cereals",
  "nutrients": [
    {
      "id": "262",
      "description": "Caffeine",
      "nutritionValue": 0,
      "units": "mg"
    },
    {
      "id": "307",
      "description": "Sodium, Na",
      "nutritionValue": 611,
      "units": "mg"
    },
    {
      "id": "309",
      "description": "Zinc, Zn",
      "nutritionValue": 5.2,
      "units": "mg"
    }
  ],
  "servings": [
    {
      "amount": 1,
      "description": "cup (1 NLEA serving)",
      "weightInGrams": 29
    }
  ]
}
```

> [!NOTE]
> <span data-ttu-id="77ff8-254">Dokument är minified i Azure Cosmos DB så hello system beräknas hello dokument ovan är något mindre än 1KB.</span><span class="sxs-lookup"><span data-stu-id="77ff8-254">Documents are minified in Azure Cosmos DB, so hello system calculated size of hello document above is slightly less than 1KB.</span></span>
> 
> 

<span data-ttu-id="77ff8-255">hello följande tabell visas ungefärlig begäran enhet kostnader för vanliga åtgärder på det här objektet (hello ungefärliga begäran enhet kostnad förutsätter att hello kontonivå konsekvenskontroll har angetts för ”Session” och att alla objekt som indexeras automatiskt):</span><span class="sxs-lookup"><span data-stu-id="77ff8-255">hello following table shows approximate request unit charges for typical operations on this item (hello approximate request unit charge assumes that hello account consistency level is set too“Session” and that all items are automatically indexed):</span></span>

| <span data-ttu-id="77ff8-256">Åtgärd</span><span class="sxs-lookup"><span data-stu-id="77ff8-256">Operation</span></span> | <span data-ttu-id="77ff8-257">Begäran enhet kostnad</span><span class="sxs-lookup"><span data-stu-id="77ff8-257">Request Unit Charge</span></span> |
| --- | --- |
| <span data-ttu-id="77ff8-258">Skapa objekt</span><span class="sxs-lookup"><span data-stu-id="77ff8-258">Create item</span></span> |<span data-ttu-id="77ff8-259">~ 15 RU</span><span class="sxs-lookup"><span data-stu-id="77ff8-259">~15 RU</span></span> |
| <span data-ttu-id="77ff8-260">Läs objekt</span><span class="sxs-lookup"><span data-stu-id="77ff8-260">Read item</span></span> |<span data-ttu-id="77ff8-261">~ 1 RU</span><span class="sxs-lookup"><span data-stu-id="77ff8-261">~1 RU</span></span> |
| <span data-ttu-id="77ff8-262">Frågan objekt-ID: t</span><span class="sxs-lookup"><span data-stu-id="77ff8-262">Query item by id</span></span> |<span data-ttu-id="77ff8-263">~2.5 RU</span><span class="sxs-lookup"><span data-stu-id="77ff8-263">~2.5 RU</span></span> |

<span data-ttu-id="77ff8-264">Den här tabellen visas dessutom ungefärliga begäran enhet kostnader för vanliga frågor som används i programmet hello:</span><span class="sxs-lookup"><span data-stu-id="77ff8-264">Additionally, this table shows approximate request unit charges for typical queries used in hello application:</span></span>

| <span data-ttu-id="77ff8-265">Fråga</span><span class="sxs-lookup"><span data-stu-id="77ff8-265">Query</span></span> | <span data-ttu-id="77ff8-266">Begäran enhet kostnad</span><span class="sxs-lookup"><span data-stu-id="77ff8-266">Request Unit Charge</span></span> | <span data-ttu-id="77ff8-267">antal returnerade artiklar</span><span class="sxs-lookup"><span data-stu-id="77ff8-267"># of Returned Items</span></span> |
| --- | --- | --- |
| <span data-ttu-id="77ff8-268">Välj mat-ID: t</span><span class="sxs-lookup"><span data-stu-id="77ff8-268">Select food by id</span></span> |<span data-ttu-id="77ff8-269">~2.5 RU</span><span class="sxs-lookup"><span data-stu-id="77ff8-269">~2.5 RU</span></span> |<span data-ttu-id="77ff8-270">1</span><span class="sxs-lookup"><span data-stu-id="77ff8-270">1</span></span> |
| <span data-ttu-id="77ff8-271">Välj livsmedel per tillverkare</span><span class="sxs-lookup"><span data-stu-id="77ff8-271">Select foods by manufacturer</span></span> |<span data-ttu-id="77ff8-272">~ 7 RU</span><span class="sxs-lookup"><span data-stu-id="77ff8-272">~7 RU</span></span> |<span data-ttu-id="77ff8-273">7</span><span class="sxs-lookup"><span data-stu-id="77ff8-273">7</span></span> |
| <span data-ttu-id="77ff8-274">Välj av mat gruppen och ordning baserat på vikt</span><span class="sxs-lookup"><span data-stu-id="77ff8-274">Select by food group and order by weight</span></span> |<span data-ttu-id="77ff8-275">~ 70 RU</span><span class="sxs-lookup"><span data-stu-id="77ff8-275">~70 RU</span></span> |<span data-ttu-id="77ff8-276">100</span><span class="sxs-lookup"><span data-stu-id="77ff8-276">100</span></span> |
| <span data-ttu-id="77ff8-277">Välj översta 10 livsmedel i en Matgrupp</span><span class="sxs-lookup"><span data-stu-id="77ff8-277">Select top 10 foods in a food group</span></span> |<span data-ttu-id="77ff8-278">~ 10 RU</span><span class="sxs-lookup"><span data-stu-id="77ff8-278">~10 RU</span></span> |<span data-ttu-id="77ff8-279">10</span><span class="sxs-lookup"><span data-stu-id="77ff8-279">10</span></span> |

> [!NOTE]
> <span data-ttu-id="77ff8-280">RU avgifter variera beroende på hello antal objekt som returneras.</span><span class="sxs-lookup"><span data-stu-id="77ff8-280">RU charges vary based on hello number of items returned.</span></span>
> 
> 

<span data-ttu-id="77ff8-281">Med den här informationen kan uppskattar vi hello RU krav för det här programmet får hello antal åtgärder och frågor som vi räknar per sekund:</span><span class="sxs-lookup"><span data-stu-id="77ff8-281">With this information, we can estimate hello RU requirements for this application given hello number of operations and queries we expect per second:</span></span>

| <span data-ttu-id="77ff8-282">Åtgärden-fråga</span><span class="sxs-lookup"><span data-stu-id="77ff8-282">Operation/Query</span></span> | <span data-ttu-id="77ff8-283">Uppskattat antal per sekund</span><span class="sxs-lookup"><span data-stu-id="77ff8-283">Estimated number per second</span></span> | <span data-ttu-id="77ff8-284">Nödvändiga RUs</span><span class="sxs-lookup"><span data-stu-id="77ff8-284">Required RUs</span></span> |
| --- | --- | --- |
| <span data-ttu-id="77ff8-285">Skapa objekt</span><span class="sxs-lookup"><span data-stu-id="77ff8-285">Create item</span></span> |<span data-ttu-id="77ff8-286">10</span><span class="sxs-lookup"><span data-stu-id="77ff8-286">10</span></span> |<span data-ttu-id="77ff8-287">150</span><span class="sxs-lookup"><span data-stu-id="77ff8-287">150</span></span> |
| <span data-ttu-id="77ff8-288">Läs objekt</span><span class="sxs-lookup"><span data-stu-id="77ff8-288">Read item</span></span> |<span data-ttu-id="77ff8-289">100</span><span class="sxs-lookup"><span data-stu-id="77ff8-289">100</span></span> |<span data-ttu-id="77ff8-290">100</span><span class="sxs-lookup"><span data-stu-id="77ff8-290">100</span></span> |
| <span data-ttu-id="77ff8-291">Välj livsmedel per tillverkare</span><span class="sxs-lookup"><span data-stu-id="77ff8-291">Select foods by manufacturer</span></span> |<span data-ttu-id="77ff8-292">25</span><span class="sxs-lookup"><span data-stu-id="77ff8-292">25</span></span> |<span data-ttu-id="77ff8-293">175</span><span class="sxs-lookup"><span data-stu-id="77ff8-293">175</span></span> |
| <span data-ttu-id="77ff8-294">Välj av Matgrupp</span><span class="sxs-lookup"><span data-stu-id="77ff8-294">Select by food group</span></span> |<span data-ttu-id="77ff8-295">10</span><span class="sxs-lookup"><span data-stu-id="77ff8-295">10</span></span> |<span data-ttu-id="77ff8-296">700</span><span class="sxs-lookup"><span data-stu-id="77ff8-296">700</span></span> |
| <span data-ttu-id="77ff8-297">Välj Topp 10</span><span class="sxs-lookup"><span data-stu-id="77ff8-297">Select top 10</span></span> |<span data-ttu-id="77ff8-298">15</span><span class="sxs-lookup"><span data-stu-id="77ff8-298">15</span></span> |<span data-ttu-id="77ff8-299">150 totalt</span><span class="sxs-lookup"><span data-stu-id="77ff8-299">150 Total</span></span> |

<span data-ttu-id="77ff8-300">I det här fallet räknar vi med en genomsnittlig genomströmning krav på 1,275 RU/s.</span><span class="sxs-lookup"><span data-stu-id="77ff8-300">In this case, we expect an average throughput requirement of 1,275 RU/s.</span></span>  <span data-ttu-id="77ff8-301">Vi vill etablera 1 300 RU/s för det här programmet samling avrundning toohello närmast 100.</span><span class="sxs-lookup"><span data-stu-id="77ff8-301">Rounding up toohello nearest 100, we would provision 1,300 RU/s for this application's collection.</span></span>

## <span data-ttu-id="77ff8-302"><a id="RequestRateTooLarge"></a>Reserverat dataflöde överskreds i Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="77ff8-302"><a id="RequestRateTooLarge"></a> Exceeding reserved throughput limits in Azure Cosmos DB</span></span>
<span data-ttu-id="77ff8-303">Kom ihåg att konsumtion av begäran enheten utvärderas som en sats per sekund om hello budget är tom.</span><span class="sxs-lookup"><span data-stu-id="77ff8-303">Recall that request unit consumption is evaluated as a rate per second if hello budget is empty.</span></span> <span data-ttu-id="77ff8-304">För program som överskrider hello begär etablerad enhet förfrågningar för en behållare toothat samlingen kommer att begränsas förrän hello frekvensen sjunker under hello reserverade nivå.</span><span class="sxs-lookup"><span data-stu-id="77ff8-304">For applications that exceed hello provisioned request unit rate for a container, requests toothat collection will be throttled until hello rate drops below hello reserved level.</span></span> <span data-ttu-id="77ff8-305">När en begränsning inträffar avslutas hello server förebyggande syfte hello begäran med RequestRateTooLargeException (HTTP-statuskod 429) och returnerar hello x-ms-retry-efter-ms-huvud som anger hello lång tid i millisekunder som hello användare måste vänta innan ett nytt försök hello begäran.</span><span class="sxs-lookup"><span data-stu-id="77ff8-305">When a throttle occurs, hello server will preemptively end hello request with RequestRateTooLargeException (HTTP status code 429) and return hello x-ms-retry-after-ms header indicating hello amount of time, in milliseconds, that hello user must wait before reattempting hello request.</span></span>

    HTTP Status 429
    Status Line: RequestRateTooLarge
    x-ms-retry-after-ms :100

<span data-ttu-id="77ff8-306">Om du använder hello klient-SDK för .NET och LINQ-frågor och de flesta hello tid du aldrig har toodeal med det här undantaget som hello nuvarande version av hello .NET klient-SDK fångar implicit svaret värnar hello server angetts försök igen efter rubrik och begäran om återförsök hello.</span><span class="sxs-lookup"><span data-stu-id="77ff8-306">If you are using hello .NET Client SDK and LINQ queries, then most of hello time you never have toodeal with this exception, as hello current version of hello .NET Client SDK implicitly catches this response, respects hello server-specified retry-after header, and retries hello request.</span></span> <span data-ttu-id="77ff8-307">Om ditt konto används samtidigt av flera klienter, lyckas hello nästa försök.</span><span class="sxs-lookup"><span data-stu-id="77ff8-307">Unless your account is being accessed concurrently by multiple clients, hello next retry will succeed.</span></span>

<span data-ttu-id="77ff8-308">Om du har mer än en klient kumulativt drift ovan hello förfrågningar hello försök standardbeteendet finns tillräckligt inte och hello klienten genereras en DocumentClientException med status kod 429 toohello program.</span><span class="sxs-lookup"><span data-stu-id="77ff8-308">If you have more than one client cumulatively operating above hello request rate, hello default retry behavior may not suffice, and hello client will throw a DocumentClientException with status code 429 toohello application.</span></span> <span data-ttu-id="77ff8-309">I sådana fall, kan du hantera försök beteende och logik i ditt program fel hantering rutiner eller att öka hello reserverat dataflöde för hello behållare.</span><span class="sxs-lookup"><span data-stu-id="77ff8-309">In cases such as this, you may consider handling retry behavior and logic in your application's error handling routines or increasing hello reserved throughput for hello container.</span></span>

## <span data-ttu-id="77ff8-310"><a id="RequestRateTooLargeAPIforMongoDB"></a>Överskrider reserverat dataflöde gränser i API för MongoDB</span><span class="sxs-lookup"><span data-stu-id="77ff8-310"><a id="RequestRateTooLargeAPIforMongoDB"></a> Exceeding reserved throughput limits in API for MongoDB</span></span>
<span data-ttu-id="77ff8-311">Program som överskrider hello etableras frågeenheter för en samling kommer att begränsas förrän hello frekvensen sjunker under hello reserverade nivå.</span><span class="sxs-lookup"><span data-stu-id="77ff8-311">Applications that exceed hello provisioned request units for a collection will be throttled until hello rate drops below hello reserved level.</span></span> <span data-ttu-id="77ff8-312">När en begränsning inträffar hello backend förebyggande syfte avslutas hello begäran med en *16500* felkoden - *för många begäranden*.</span><span class="sxs-lookup"><span data-stu-id="77ff8-312">When a throttle occurs, hello backend will preemptively end hello request with a *16500* error code - *Too Many Requests*.</span></span> <span data-ttu-id="77ff8-313">Som standard API för MongoDB kommer automatiskt att försöka in too10 gånger innan det returneras en *för många begäranden* felkoden.</span><span class="sxs-lookup"><span data-stu-id="77ff8-313">By default, API for MongoDB will automatically retry up too10 times before returning a *Too Many Requests* error code.</span></span> <span data-ttu-id="77ff8-314">Om du tar emot många *för många begäranden* felkoder, kan du antingen lägga till försök beteende i ditt program felhantering rutiner eller [öka hello reserverat dataflöde för hello samling](set-throughput.md).</span><span class="sxs-lookup"><span data-stu-id="77ff8-314">If you are receiving many *Too Many Requests* error codes, you may consider either adding retry behavior in your application's error handling routines or [increasing hello reserved throughput for hello collection](set-throughput.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="77ff8-315">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="77ff8-315">Next steps</span></span>
<span data-ttu-id="77ff8-316">toolearn mer om reserverat dataflöde med Azure Cosmos DB databaser utforska gärna dessa resurser:</span><span class="sxs-lookup"><span data-stu-id="77ff8-316">toolearn more about reserved throughput with Azure Cosmos DB databases, explore these resources:</span></span>

* [<span data-ttu-id="77ff8-317">Azure DB Cosmos-priser</span><span class="sxs-lookup"><span data-stu-id="77ff8-317">Azure Cosmos DB pricing</span></span>](https://azure.microsoft.com/pricing/details/cosmos-db/)
* [<span data-ttu-id="77ff8-318">Partitionering data i Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="77ff8-318">Partitioning data in Azure Cosmos DB</span></span>](partition-data.md)

<span data-ttu-id="77ff8-319">toolearn mer om Azure Cosmos DB finns hello Azure Cosmos DB [dokumentationen](https://azure.microsoft.com/documentation/services/cosmos-db/).</span><span class="sxs-lookup"><span data-stu-id="77ff8-319">toolearn more about Azure Cosmos DB, see hello Azure Cosmos DB [documentation](https://azure.microsoft.com/documentation/services/cosmos-db/).</span></span> 

<span data-ttu-id="77ff8-320">tooget igång med skalning och prestandatester med Azure Cosmos DB finns [prestanda och Skalningstester med Azure Cosmos DB](performance-testing.md).</span><span class="sxs-lookup"><span data-stu-id="77ff8-320">tooget started with scale and performance testing with Azure Cosmos DB, see [Performance and Scale Testing with Azure Cosmos DB](performance-testing.md).</span></span>

[1]: ./media/request-units/queryexplorer.png 
[2]: ./media/request-units/RUEstimatorUpload.png
[3]: ./media/request-units/RUEstimatorDocuments.png
[4]: ./media/request-units/RUEstimatorResults.png
[5]: ./media/request-units/RUCalculator2.png
[6]: ./media/request-units/api-for-mongodb-metrics.png
