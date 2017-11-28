---
title: aaaPartitioning och skalning i Azure Cosmos DB | Microsoft Docs
description: "Lär dig mer om hur partitionering fungerar i Azure Cosmos DB hur tooconfigure partitionering och partitionsnycklar och hur toopick hello höger partitions-nyckeln för ditt program."
services: cosmos-db
author: arramac
manager: jhubbard
editor: monicar
documentationcenter: 
ms.assetid: 702c39b4-1798-48dd-9993-4493a2f6df9e
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/24/2017
ms.author: arramac
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 30621d2ba0b89efb72005680d5f3a73998347514
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="partitioning-in-azure-cosmos-db-using-hello-documentdb-api"></a><span data-ttu-id="f1000-103">Partitionering i Azure Cosmos-databasen med hello DocumentDB-API</span><span class="sxs-lookup"><span data-stu-id="f1000-103">Partitioning in Azure Cosmos DB using hello DocumentDB API</span></span>

<span data-ttu-id="f1000-104">[Microsoft Azure Cosmos DB](../cosmos-db/introduction.md) är en global distribuerade och flera olika modeller databasen syftar toohelp du uppnå snabb och förutsägbar prestanda och skalning sömlöst tillsammans med ditt program som de växer.</span><span class="sxs-lookup"><span data-stu-id="f1000-104">[Microsoft Azure Cosmos DB](../cosmos-db/introduction.md) is a global distributed, multi-model database service designed toohelp you achieve fast, predictable performance and scale seamlessly along with your application as it grows.</span></span> 

<span data-ttu-id="f1000-105">Den här artikeln innehåller en översikt över hur toowork med partitionering av Cosmos-DB-behållare med hello DocumentDB-API.</span><span class="sxs-lookup"><span data-stu-id="f1000-105">This article provides an overview of how toowork with partitioning of Cosmos DB containers with hello DocumentDB API.</span></span> <span data-ttu-id="f1000-106">Se [partitionering och teckenbredden](../cosmos-db/partition-data.md) en översikt över begrepp och bästa praxis för partitionering med Azure Cosmos DB API: er.</span><span class="sxs-lookup"><span data-stu-id="f1000-106">See [partitioning and horizontal scaling](../cosmos-db/partition-data.md) for an overview of concepts and best practices for partitioning with any Azure Cosmos DB API.</span></span> 

<span data-ttu-id="f1000-107">tooget igång med kod, kan du hämta hello projektet från [Github](https://github.com/Azure/azure-documentdb-dotnet/tree/a2d61ddb53f8ab2a23d3ce323c77afcf5a608f52/samples/documentdb-benchmark).</span><span class="sxs-lookup"><span data-stu-id="f1000-107">tooget started with code, download hello project from [Github](https://github.com/Azure/azure-documentdb-dotnet/tree/a2d61ddb53f8ab2a23d3ce323c77afcf5a608f52/samples/documentdb-benchmark).</span></span> 

<span data-ttu-id="f1000-108">När du har läst den här artikeln kommer du att kunna tooanswer hello följande frågor:</span><span class="sxs-lookup"><span data-stu-id="f1000-108">After reading this article, you will be able tooanswer hello following questions:</span></span>   

* <span data-ttu-id="f1000-109">Hur fungerar partitionering arbete i Azure Cosmos DB?</span><span class="sxs-lookup"><span data-stu-id="f1000-109">How does partitioning work in Azure Cosmos DB?</span></span>
* <span data-ttu-id="f1000-110">Hur konfigurerar jag partitionering i Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="f1000-110">How do I configure partitioning in Azure Cosmos DB</span></span>
* <span data-ttu-id="f1000-111">Vad är partitionsnycklar och hur jag välja hello rätt partitionsnyckel för Mina program?</span><span class="sxs-lookup"><span data-stu-id="f1000-111">What are partition keys, and how do I pick hello right partition key for my application?</span></span>

<span data-ttu-id="f1000-112">tooget igång med kod, kan du hämta hello projektet från [Azure Cosmos DB prestanda testa drivrutinen Sample](https://github.com/Azure/azure-documentdb-dotnet/tree/a2d61ddb53f8ab2a23d3ce323c77afcf5a608f52/samples/documentdb-benchmark).</span><span class="sxs-lookup"><span data-stu-id="f1000-112">tooget started with code, download hello project from [Azure Cosmos DB Performance Testing Driver Sample](https://github.com/Azure/azure-documentdb-dotnet/tree/a2d61ddb53f8ab2a23d3ce323c77afcf5a608f52/samples/documentdb-benchmark).</span></span> 

<!-- placeholder until we have a permanent solution-->
<a name="partition-keys"></a>
<a name="single-partition-and-partitioned-collections"></a>
<a name="migrating-from-single-partition"></a>

## <a name="partition-keys"></a><span data-ttu-id="f1000-113">Partitionsnycklar</span><span class="sxs-lookup"><span data-stu-id="f1000-113">Partition keys</span></span>

<span data-ttu-id="f1000-114">Ange hello partition definitionen i hello form av en JSON-sökvägen i hello DocumentDB-API.</span><span class="sxs-lookup"><span data-stu-id="f1000-114">In hello DocumentDB API, you specify hello partition key definition in hello form of a JSON path.</span></span> <span data-ttu-id="f1000-115">hello visas följande tabell exempel på partitionen viktiga definitioner och hello värden motsvarande tooeach.</span><span class="sxs-lookup"><span data-stu-id="f1000-115">hello following table shows examples of partition key definitions and hello values corresponding tooeach.</span></span> <span data-ttu-id="f1000-116">hello partitionsnyckel har angetts som en sökväg, t.ex. `/department` representerar hello egenskapen avdelning.</span><span class="sxs-lookup"><span data-stu-id="f1000-116">hello partition key is specified as a path, e.g. `/department` represents hello property department.</span></span> 

<table border="0" cellspacing="0" cellpadding="0">
    <tbody>
        <tr>
            <td valign="top"><p><span data-ttu-id="f1000-117"><strong>Partitionsnyckeln</strong></span><span class="sxs-lookup"><span data-stu-id="f1000-117"><strong>Partition Key</strong></span></span></p></td>
            <td valign="top"><p><span data-ttu-id="f1000-118"><strong>Beskrivning</strong></span><span class="sxs-lookup"><span data-stu-id="f1000-118"><strong>Description</strong></span></span></p></td>
        </tr>
        <tr>
            <td valign="top"><p><span data-ttu-id="f1000-119">/Department</span><span class="sxs-lookup"><span data-stu-id="f1000-119">/department</span></span></p></td>
            <td valign="top"><p><span data-ttu-id="f1000-120">Motsvarar toohello värdet för doc.department där doc är hello-objektet.</span><span class="sxs-lookup"><span data-stu-id="f1000-120">Corresponds toohello value of doc.department where doc is hello item.</span></span></p></td>
        </tr>
        <tr>
            <td valign="top"><p><span data-ttu-id="f1000-121">/ Egenskaper/namn</span><span class="sxs-lookup"><span data-stu-id="f1000-121">/properties/name</span></span></p></td>
            <td valign="top"><p><span data-ttu-id="f1000-122">Motsvarar toohello värdet för doc.properties.name där doc är hello artikel (kapslade egenskap).</span><span class="sxs-lookup"><span data-stu-id="f1000-122">Corresponds toohello value of doc.properties.name where doc is hello item (nested property).</span></span></p></td>
        </tr>
        <tr>
            <td valign="top"><p><span data-ttu-id="f1000-123">/ID</span><span class="sxs-lookup"><span data-stu-id="f1000-123">/id</span></span></p></td>
            <td valign="top"><p><span data-ttu-id="f1000-124">Motsvarar toohello värdet för doc.id (id och partitions-nyckeln är hello samma egenskap).</span><span class="sxs-lookup"><span data-stu-id="f1000-124">Corresponds toohello value of doc.id (id and partition key are hello same property).</span></span></p></td>
        </tr>
        <tr>
            <td valign="top"><p><span data-ttu-id="f1000-125">/ ”avdelningsnamn”</span><span class="sxs-lookup"><span data-stu-id="f1000-125">/"department name"</span></span></p></td>
            <td valign="top"><p><span data-ttu-id="f1000-126">Motsvarar toohello värdet för doc [”” avdelningsnamn] där doc är hello-objektet.</span><span class="sxs-lookup"><span data-stu-id="f1000-126">Corresponds toohello value of doc["department name"] where doc is hello item.</span></span></p></td>
        </tr>
    </tbody>
</table>

> [!NOTE]
> <span data-ttu-id="f1000-127">hello syntax för Partitionsnyckeln är liknande toohello sökvägen för indexering princip sökvägar med hello viktigaste skillnaden som hello sökvägen motsvarar toohello-egenskapen i stället för hello värde, dvs. det finns inga jokertecken hello slutet.</span><span class="sxs-lookup"><span data-stu-id="f1000-127">hello syntax for partition key is similar toohello path specification for indexing policy paths with hello key difference that hello path corresponds toohello property instead of hello value, i.e. there is no wild card at hello end.</span></span> <span data-ttu-id="f1000-128">Exempelvis anger du/avdelning /?</span><span class="sxs-lookup"><span data-stu-id="f1000-128">For example, you would specify /department/?</span></span> <span data-ttu-id="f1000-129">tooindex hello värden under avdelning, men ange /department som hello definition av partition.</span><span class="sxs-lookup"><span data-stu-id="f1000-129">tooindex hello values under department, but specify /department as hello partition key definition.</span></span> <span data-ttu-id="f1000-130">hello partitionsnyckel indexeras implicit och kan inte uteslutas från att indexera med indexering princip åsidosättningar.</span><span class="sxs-lookup"><span data-stu-id="f1000-130">hello partition key is implicitly indexed and cannot be excluded from indexing using indexing policy overrides.</span></span>
> 
> 

<span data-ttu-id="f1000-131">Nu ska vi titta på hur hello valet av partitionsnyckel påverkar hello prestanda för ditt program.</span><span class="sxs-lookup"><span data-stu-id="f1000-131">Let's look at how hello choice of partition key impacts hello performance of your application.</span></span>

## <a name="working-with-hello-azure-cosmos-db-sdks"></a><span data-ttu-id="f1000-132">Arbeta med hello Azure Cosmos DB SDK</span><span class="sxs-lookup"><span data-stu-id="f1000-132">Working with hello Azure Cosmos DB SDKs</span></span>
<span data-ttu-id="f1000-133">Azure Cosmos-DB tillagt stöd för automatisk partitionering med [REST API-version 2015-12-16](/rest/api/documentdb/).</span><span class="sxs-lookup"><span data-stu-id="f1000-133">Azure Cosmos DB added support for automatic partitioning with [REST API version 2015-12-16](/rest/api/documentdb/).</span></span> <span data-ttu-id="f1000-134">I ordning toocreate partitionerad behållare, måste du hämta SDK-versioner 1.6.0 eller senare på en av hello stöds SDK-plattformar (.NET, Node.js, Java, Python, MongoDB).</span><span class="sxs-lookup"><span data-stu-id="f1000-134">In order toocreate partitioned containers, you must download SDK versions 1.6.0 or newer in one of hello supported SDK platforms (.NET, Node.js, Java, Python, MongoDB).</span></span> 

### <a name="creating-containers"></a><span data-ttu-id="f1000-135">Skapa behållare</span><span class="sxs-lookup"><span data-stu-id="f1000-135">Creating containers</span></span>
<span data-ttu-id="f1000-136">hello följande exempel visas en .NET fragment toocreate en behållare toostore telemetri enhetsdata för 20 000 frågeenheter per sekund genomströmning.</span><span class="sxs-lookup"><span data-stu-id="f1000-136">hello following sample shows a .NET snippet toocreate a container toostore device telemetry data of 20,000 request units per second of throughput.</span></span> <span data-ttu-id="f1000-137">hello SDK anger hello OfferThroughput värde (som i sin tur anger hello `x-ms-offer-throughput` huvudet i begäran i hello REST-API).</span><span class="sxs-lookup"><span data-stu-id="f1000-137">hello SDK sets hello OfferThroughput value (which in turn sets hello `x-ms-offer-throughput` request header in hello REST API).</span></span> <span data-ttu-id="f1000-138">Här ska du ange hello `/deviceId` som hello partitionsnyckel.</span><span class="sxs-lookup"><span data-stu-id="f1000-138">Here we set hello `/deviceId` as hello partition key.</span></span> <span data-ttu-id="f1000-139">hello valet av partitionsnyckel sparas tillsammans med hello resten av hello behållaren metadata som namn och indexprincip.</span><span class="sxs-lookup"><span data-stu-id="f1000-139">hello choice of partition key is saved along with hello rest of hello container metadata like name and indexing policy.</span></span>

<span data-ttu-id="f1000-140">För det här exemplet vi plockats `deviceId` eftersom vi vet att (a) eftersom det finns ett stort antal enheter skrivningar kan fördelas jämnt mellan partitioner och att tillåta oss tooscale hello databasen tooingest massiva mängder data och (b) många hello begäranden som hämtar hello senaste avläsningen för en enhet är begränsade tooa enda deviceId och kan hämtas från en enda partition.</span><span class="sxs-lookup"><span data-stu-id="f1000-140">For this sample, we picked `deviceId` since we know that (a) since there are a large number of devices, writes can be distributed across partitions evenly and allowing us tooscale hello database tooingest massive volumes of data and (b) many of hello requests like fetching hello latest reading for a device are scoped tooa single deviceId and can be retrieved from a single partition.</span></span>

```csharp
DocumentClient client = new DocumentClient(new Uri(endpoint), authKey);
await client.CreateDatabaseAsync(new Database { Id = "db" });

// Container for device telemetry. Here hello property deviceId will be used as hello partition key too
// spread across partitions. Configured for 10K RU/s throughput and an indexing policy that supports 
// sorting against any number or string property.
DocumentCollection myCollection = new DocumentCollection();
myCollection.Id = "coll";
myCollection.PartitionKey.Paths.Add("/deviceId");

await client.CreateDocumentCollectionAsync(
    UriFactory.CreateDatabaseUri("db"),
    myCollection,
    new RequestOptions { OfferThroughput = 20000 });
```

<span data-ttu-id="f1000-141">Den här metoden gör en REST-API som anropar tooCosmos DB och hello-tjänst etablerar ett antal partitioner baserat på hello begärda genomflöde.</span><span class="sxs-lookup"><span data-stu-id="f1000-141">This method makes a REST API call tooCosmos DB, and hello service will provision a number of partitions based on hello requested throughput.</span></span> <span data-ttu-id="f1000-142">Du kan ändra hello genomströmningen i en behållare prestandan behov utvecklas.</span><span class="sxs-lookup"><span data-stu-id="f1000-142">You can change hello throughput of a container as your performance needs evolve.</span></span> 

### <a name="reading-and-writing-items"></a><span data-ttu-id="f1000-143">Läsning och skrivning av objekt</span><span class="sxs-lookup"><span data-stu-id="f1000-143">Reading and writing items</span></span>
<span data-ttu-id="f1000-144">Nu ska vi infoga data i Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="f1000-144">Now, let's insert data into Cosmos DB.</span></span> <span data-ttu-id="f1000-145">Här är ett exempel på klass som innehåller en enhet läsning och en anropet tooCreateDocumentAsync tooinsert en ny enhet som läses in en behållare.</span><span class="sxs-lookup"><span data-stu-id="f1000-145">Here's a sample class containing a device reading, and a call tooCreateDocumentAsync tooinsert a new device reading into a container.</span></span> <span data-ttu-id="f1000-146">Detta är ett exempel som utnyttjar hello DocumentDB-API:</span><span class="sxs-lookup"><span data-stu-id="f1000-146">This is an example leveraging hello DocumentDB API:</span></span>

```csharp
public class DeviceReading
{
    [JsonProperty("id")]
    public string Id;

    [JsonProperty("deviceId")]
    public string DeviceId;

    [JsonConverter(typeof(IsoDateTimeConverter))]
    [JsonProperty("readingTime")]
    public DateTime ReadingTime;

    [JsonProperty("metricType")]
    public string MetricType;

    [JsonProperty("unit")]
    public string Unit;

    [JsonProperty("metricValue")]
    public double MetricValue;
  }

// Create a document. Here hello partition key is extracted as "XMS-0001" based on hello collection definition
await client.CreateDocumentAsync(
    UriFactory.CreateDocumentCollectionUri("db", "coll"),
    new DeviceReading
    {
        Id = "XMS-001-FE24C",
        DeviceId = "XMS-0001",
        MetricType = "Temperature",
        MetricValue = 105.00,
        Unit = "Fahrenheit",
        ReadingTime = DateTime.UtcNow
    });
```

<span data-ttu-id="f1000-147">Vi läsa hello-objekt efter dess partitionsnyckel och id, uppdatera och som ett sista steg kan ta bort den av partitionsnyckel och ID: t. Observera att hello läsningar innehåller ett värde för PartitionKey (motsvarande toohello `x-ms-documentdb-partitionkey` huvudet i begäran i hello REST-API).</span><span class="sxs-lookup"><span data-stu-id="f1000-147">Let's read hello item by its partition key and id, update it, and then as a final step, delete it by partition key and id. Note that hello reads include a PartitionKey value (corresponding toohello `x-ms-documentdb-partitionkey` request header in hello REST API).</span></span>

```csharp
// Read document. Needs hello partition key and hello ID toobe specified
Document result = await client.ReadDocumentAsync(
  UriFactory.CreateDocumentUri("db", "coll", "XMS-001-FE24C"), 
  new RequestOptions { PartitionKey = new PartitionKey("XMS-0001") });

DeviceReading reading = (DeviceReading)(dynamic)result;

// Update hello document. Partition key is not required, again extracted from hello document
reading.MetricValue = 104;
reading.ReadingTime = DateTime.UtcNow;

await client.ReplaceDocumentAsync(
  UriFactory.CreateDocumentUri("db", "coll", "XMS-001-FE24C"), 
  reading);

// Delete document. Needs partition key
await client.DeleteDocumentAsync(
  UriFactory.CreateDocumentUri("db", "coll", "XMS-001-FE24C"), 
  new RequestOptions { PartitionKey = new PartitionKey("XMS-0001") });
```

### <a name="querying-partitioned-containers"></a><span data-ttu-id="f1000-148">Frågar partitionerade behållare</span><span class="sxs-lookup"><span data-stu-id="f1000-148">Querying partitioned containers</span></span>
<span data-ttu-id="f1000-149">När du frågar efter data i partitionerade behållare, Cosmos DB automatiskt hello vägar toohello frågepartitioner motsvarande toohello partitionsnyckelvärden som angavs i hello filter (om det finns några).</span><span class="sxs-lookup"><span data-stu-id="f1000-149">When you query data in partitioned containers, Cosmos DB automatically routes hello query toohello partitions corresponding toohello partition key values specified in hello filter (if there are any).</span></span> <span data-ttu-id="f1000-150">Den här frågan är till exempel routade toojust hello partition som innehåller hello partitionsnyckel ”XMS 0001”.</span><span class="sxs-lookup"><span data-stu-id="f1000-150">For example, this query is routed toojust hello partition containing hello partition key "XMS-0001".</span></span>

```csharp
// Query using partition key
IQueryable<DeviceReading> query = client.CreateDocumentQuery<DeviceReading>(
    UriFactory.CreateDocumentCollectionUri("db", "coll"))
    .Where(m => m.MetricType == "Temperature" && m.DeviceId == "XMS-0001");
```
    
<span data-ttu-id="f1000-151">hello följande fråga har inte ett filter på hello partitionsnyckel (DeviceId) och fanned ut tooall partitioner där den körs mot hello partition index.</span><span class="sxs-lookup"><span data-stu-id="f1000-151">hello following query does not have a filter on hello partition key (DeviceId) and is fanned out tooall partitions where it is executed against hello partition's index.</span></span> <span data-ttu-id="f1000-152">Observera att du har toospecify hello EnableCrossPartitionQuery (`x-ms-documentdb-query-enablecrosspartition` i hello REST-API) toohave hello SDK tooexecute en fråga över partitioner.</span><span class="sxs-lookup"><span data-stu-id="f1000-152">Note that you have toospecify hello EnableCrossPartitionQuery (`x-ms-documentdb-query-enablecrosspartition` in hello REST API) toohave hello SDK tooexecute a query across partitions.</span></span>

```csharp
// Query across partition keys
IQueryable<DeviceReading> crossPartitionQuery = client.CreateDocumentQuery<DeviceReading>(
    UriFactory.CreateDocumentCollectionUri("db", "coll"), 
    new FeedOptions { EnableCrossPartitionQuery = true })
    .Where(m => m.MetricType == "Temperature" && m.MetricValue > 100);
```

<span data-ttu-id="f1000-153">Har stöd för cosmos DB [mängdfunktionerna](documentdb-sql-query.md#Aggregates) `COUNT`, `MIN`, `MAX`, `SUM` och `AVG` över partitionerad behållare med SQL som börjar med SDK: er 1.12.0 och högre.</span><span class="sxs-lookup"><span data-stu-id="f1000-153">Cosmos DB supports [aggregate functions](documentdb-sql-query.md#Aggregates) `COUNT`, `MIN`, `MAX`, `SUM` and `AVG` over partitioned containers using SQL starting with SDKs 1.12.0 and above.</span></span> <span data-ttu-id="f1000-154">Frågor måste innehålla en sammanställd operator och måste innehålla ett värde i hello projektion.</span><span class="sxs-lookup"><span data-stu-id="f1000-154">Queries must include a single aggregate operator, and must include a single value in hello projection.</span></span>

### <a name="parallel-query-execution"></a><span data-ttu-id="f1000-155">Parallell frågekörning</span><span class="sxs-lookup"><span data-stu-id="f1000-155">Parallel query execution</span></span>
<span data-ttu-id="f1000-156">Hej Cosmos DB SDK 1.9.0 och ovan stöd parallell körning frågealternativ, som gör att du tooperform låg latens frågor mot partitionerade samlingar, även när de behöver tootouch ett stort antal partitioner.</span><span class="sxs-lookup"><span data-stu-id="f1000-156">hello Cosmos DB SDKs 1.9.0 and above support parallel query execution options, which allow you tooperform low latency queries against partitioned collections, even when they need tootouch a large number of partitions.</span></span> <span data-ttu-id="f1000-157">Följande fråga hello är till exempel konfigurerade toorun parallellt över partitioner.</span><span class="sxs-lookup"><span data-stu-id="f1000-157">For example, hello following query is configured toorun in parallel across partitions.</span></span>

```csharp
// Cross-partition Order By Queries
IQueryable<DeviceReading> crossPartitionQuery = client.CreateDocumentQuery<DeviceReading>(
    UriFactory.CreateDocumentCollectionUri("db", "coll"), 
    new FeedOptions { EnableCrossPartitionQuery = true, MaxDegreeOfParallelism = 10, MaxBufferedItemCount = 100})
    .Where(m => m.MetricType == "Temperature" && m.MetricValue > 100)
    .OrderBy(m => m.MetricValue);
```
    
<span data-ttu-id="f1000-158">Du kan hantera parallell frågekörning genom att justera hello följande parametrar:</span><span class="sxs-lookup"><span data-stu-id="f1000-158">You can manage parallel query execution by tuning hello following parameters:</span></span>

* <span data-ttu-id="f1000-159">Genom att ange `MaxDegreeOfParallelism`, du kan styra hello grad av parallellitet d.v.s. hello högsta tillåtna antalet samtidiga nätverket anslutningar toohello behållarens partitioner.</span><span class="sxs-lookup"><span data-stu-id="f1000-159">By setting `MaxDegreeOfParallelism`, you can control hello degree of parallelism i.e., hello maximum number of simultaneous network connections toohello container's partitions.</span></span> <span data-ttu-id="f1000-160">Om du väljer för 1, hanteras hello grad av parallellitet av hello SDK.</span><span class="sxs-lookup"><span data-stu-id="f1000-160">If you set this too-1, hello degree of parallelism is managed by hello SDK.</span></span> <span data-ttu-id="f1000-161">Om hello `MaxDegreeOfParallelism` har inte angetts eller ange too0, vilket är standardvärdet för hello, finns en enda nätverk anslutning toohello behållares partitioner.</span><span class="sxs-lookup"><span data-stu-id="f1000-161">If hello `MaxDegreeOfParallelism` is not specified or set too0, which is hello default value, there will be a single network connection toohello container's partitions.</span></span>
* <span data-ttu-id="f1000-162">Genom att ange `MaxBufferedItemCount`, du kan handlar av frågan latens och klientsidan minnesanvändning.</span><span class="sxs-lookup"><span data-stu-id="f1000-162">By setting `MaxBufferedItemCount`, you can trade off query latency and client-side memory utilization.</span></span> <span data-ttu-id="f1000-163">Om du utelämnar den här parametern eller Ställ in för 1 hanteras hello antal objekt som buffras under parallell frågekörning av hello SDK.</span><span class="sxs-lookup"><span data-stu-id="f1000-163">If you omit this parameter or set this too-1, hello number of items buffered during parallel query execution is managed by hello SDK.</span></span>

<span data-ttu-id="f1000-164">Angivna hello samma tillstånd mängden hello en parallell fråga returnerar resultat i hello samma ordning som seriella körning.</span><span class="sxs-lookup"><span data-stu-id="f1000-164">Given hello same state of hello collection, a parallel query will return results in hello same order as in serial execution.</span></span> <span data-ttu-id="f1000-165">När du utför fråga cross-partition som innehåller sortering (ORDER BY och/eller TOP), hello hello Azure Cosmos DB SDK problem fråga parallellt över partitioner och slår ihop delvis sorterade resulterar i hello klienten sida tooproduce globalt sorterade resultat.</span><span class="sxs-lookup"><span data-stu-id="f1000-165">When performing a cross-partition query that includes sorting (ORDER BY and/or TOP), hello Azure Cosmos DB SDK issues hello query in parallel across partitions and merges partially sorted results in hello client side tooproduce globally ordered results.</span></span>

### <a name="executing-stored-procedures"></a><span data-ttu-id="f1000-166">Köra lagrade procedurer</span><span class="sxs-lookup"><span data-stu-id="f1000-166">Executing stored procedures</span></span>
<span data-ttu-id="f1000-167">Du kan också köra atomiska transaktioner mot dokument med hello samma enhets-ID, t.ex. Om du underhålla mängder eller hello senaste tillståndet för en enhet i ett enskilt objekt.</span><span class="sxs-lookup"><span data-stu-id="f1000-167">You can also execute atomic transactions against documents with hello same device ID, e.g. if you're maintaining aggregates or hello latest state of a device in a single item.</span></span> 

```csharp
await client.ExecuteStoredProcedureAsync<DeviceReading>(
    UriFactory.CreateStoredProcedureUri("db", "coll", "SetLatestStateAcrossReadings"),
    new RequestOptions { PartitionKey = new PartitionKey("XMS-001") }, 
    "XMS-001-FE24C");
```
   
<span data-ttu-id="f1000-168">I nästa avsnitt om hello titta vi på hur du kan flytta toopartitioned behållare från enskild partition behållare.</span><span class="sxs-lookup"><span data-stu-id="f1000-168">In hello next section, we look at how you can move toopartitioned containers from single-partition containers.</span></span>

## <a name="next-steps"></a><span data-ttu-id="f1000-169">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="f1000-169">Next steps</span></span>
<span data-ttu-id="f1000-170">I den här artikeln har sammanställt vi en översikt över hur toowork med partitionering av Azure Cosmos DB behållare med hello DocumentDB-API.</span><span class="sxs-lookup"><span data-stu-id="f1000-170">In this article, we provided an overview of how toowork with partitioning of Azure Cosmos DB containers with hello DocumentDB API.</span></span> <span data-ttu-id="f1000-171">Se även [partitionering och teckenbredden](../cosmos-db/partition-data.md) en översikt över begrepp och bästa praxis för partitionering med Azure Cosmos DB API: er.</span><span class="sxs-lookup"><span data-stu-id="f1000-171">Also see [partitioning and horizontal scaling](../cosmos-db/partition-data.md) for an overview of concepts and best practices for partitioning with any Azure Cosmos DB API.</span></span> 

* <span data-ttu-id="f1000-172">Utföra skalnings- och prestandatester med Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="f1000-172">Perform scale and performance testing with Azure Cosmos DB.</span></span> <span data-ttu-id="f1000-173">Se [prestanda och Skalningstester med Azure Cosmos DB](performance-testing.md) ett exempel.</span><span class="sxs-lookup"><span data-stu-id="f1000-173">See [Performance and Scale Testing with Azure Cosmos DB](performance-testing.md) for a sample.</span></span>
* <span data-ttu-id="f1000-174">Komma igång med programmeringen med hello [SDK](documentdb-sdk-dotnet.md) eller hello [REST API](/rest/api/documentdb/)</span><span class="sxs-lookup"><span data-stu-id="f1000-174">Get started coding with hello [SDKs](documentdb-sdk-dotnet.md) or hello [REST API](/rest/api/documentdb/)</span></span>
* <span data-ttu-id="f1000-175">Lär dig mer om [etablerat dataflöde i Azure Cosmos DB](request-units.md)</span><span class="sxs-lookup"><span data-stu-id="f1000-175">Learn about [provisioned throughput in Azure Cosmos DB](request-units.md)</span></span>

