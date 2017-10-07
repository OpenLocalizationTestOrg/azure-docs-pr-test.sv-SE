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
# <a name="partitioning-in-azure-cosmos-db-using-hello-documentdb-api"></a>Partitionering i Azure Cosmos-databasen med hello DocumentDB-API

[Microsoft Azure Cosmos DB](../cosmos-db/introduction.md) är en global distribuerade och flera olika modeller databasen syftar toohelp du uppnå snabb och förutsägbar prestanda och skalning sömlöst tillsammans med ditt program som de växer. 

Den här artikeln innehåller en översikt över hur toowork med partitionering av Cosmos-DB-behållare med hello DocumentDB-API. Se [partitionering och teckenbredden](../cosmos-db/partition-data.md) en översikt över begrepp och bästa praxis för partitionering med Azure Cosmos DB API: er. 

tooget igång med kod, kan du hämta hello projektet från [Github](https://github.com/Azure/azure-documentdb-dotnet/tree/a2d61ddb53f8ab2a23d3ce323c77afcf5a608f52/samples/documentdb-benchmark). 

När du har läst den här artikeln kommer du att kunna tooanswer hello följande frågor:   

* Hur fungerar partitionering arbete i Azure Cosmos DB?
* Hur konfigurerar jag partitionering i Azure Cosmos DB
* Vad är partitionsnycklar och hur jag välja hello rätt partitionsnyckel för Mina program?

tooget igång med kod, kan du hämta hello projektet från [Azure Cosmos DB prestanda testa drivrutinen Sample](https://github.com/Azure/azure-documentdb-dotnet/tree/a2d61ddb53f8ab2a23d3ce323c77afcf5a608f52/samples/documentdb-benchmark). 

<!-- placeholder until we have a permanent solution-->
<a name="partition-keys"></a>
<a name="single-partition-and-partitioned-collections"></a>
<a name="migrating-from-single-partition"></a>

## <a name="partition-keys"></a>Partitionsnycklar

Ange hello partition definitionen i hello form av en JSON-sökvägen i hello DocumentDB-API. hello visas följande tabell exempel på partitionen viktiga definitioner och hello värden motsvarande tooeach. hello partitionsnyckel har angetts som en sökväg, t.ex. `/department` representerar hello egenskapen avdelning. 

<table border="0" cellspacing="0" cellpadding="0">
    <tbody>
        <tr>
            <td valign="top"><p><strong>Partitionsnyckeln</strong></p></td>
            <td valign="top"><p><strong>Beskrivning</strong></p></td>
        </tr>
        <tr>
            <td valign="top"><p>/Department</p></td>
            <td valign="top"><p>Motsvarar toohello värdet för doc.department där doc är hello-objektet.</p></td>
        </tr>
        <tr>
            <td valign="top"><p>/ Egenskaper/namn</p></td>
            <td valign="top"><p>Motsvarar toohello värdet för doc.properties.name där doc är hello artikel (kapslade egenskap).</p></td>
        </tr>
        <tr>
            <td valign="top"><p>/ID</p></td>
            <td valign="top"><p>Motsvarar toohello värdet för doc.id (id och partitions-nyckeln är hello samma egenskap).</p></td>
        </tr>
        <tr>
            <td valign="top"><p>/ ”avdelningsnamn”</p></td>
            <td valign="top"><p>Motsvarar toohello värdet för doc [”” avdelningsnamn] där doc är hello-objektet.</p></td>
        </tr>
    </tbody>
</table>

> [!NOTE]
> hello syntax för Partitionsnyckeln är liknande toohello sökvägen för indexering princip sökvägar med hello viktigaste skillnaden som hello sökvägen motsvarar toohello-egenskapen i stället för hello värde, dvs. det finns inga jokertecken hello slutet. Exempelvis anger du/avdelning /? tooindex hello värden under avdelning, men ange /department som hello definition av partition. hello partitionsnyckel indexeras implicit och kan inte uteslutas från att indexera med indexering princip åsidosättningar.
> 
> 

Nu ska vi titta på hur hello valet av partitionsnyckel påverkar hello prestanda för ditt program.

## <a name="working-with-hello-azure-cosmos-db-sdks"></a>Arbeta med hello Azure Cosmos DB SDK
Azure Cosmos-DB tillagt stöd för automatisk partitionering med [REST API-version 2015-12-16](/rest/api/documentdb/). I ordning toocreate partitionerad behållare, måste du hämta SDK-versioner 1.6.0 eller senare på en av hello stöds SDK-plattformar (.NET, Node.js, Java, Python, MongoDB). 

### <a name="creating-containers"></a>Skapa behållare
hello följande exempel visas en .NET fragment toocreate en behållare toostore telemetri enhetsdata för 20 000 frågeenheter per sekund genomströmning. hello SDK anger hello OfferThroughput värde (som i sin tur anger hello `x-ms-offer-throughput` huvudet i begäran i hello REST-API). Här ska du ange hello `/deviceId` som hello partitionsnyckel. hello valet av partitionsnyckel sparas tillsammans med hello resten av hello behållaren metadata som namn och indexprincip.

För det här exemplet vi plockats `deviceId` eftersom vi vet att (a) eftersom det finns ett stort antal enheter skrivningar kan fördelas jämnt mellan partitioner och att tillåta oss tooscale hello databasen tooingest massiva mängder data och (b) många hello begäranden som hämtar hello senaste avläsningen för en enhet är begränsade tooa enda deviceId och kan hämtas från en enda partition.

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

Den här metoden gör en REST-API som anropar tooCosmos DB och hello-tjänst etablerar ett antal partitioner baserat på hello begärda genomflöde. Du kan ändra hello genomströmningen i en behållare prestandan behov utvecklas. 

### <a name="reading-and-writing-items"></a>Läsning och skrivning av objekt
Nu ska vi infoga data i Cosmos DB. Här är ett exempel på klass som innehåller en enhet läsning och en anropet tooCreateDocumentAsync tooinsert en ny enhet som läses in en behållare. Detta är ett exempel som utnyttjar hello DocumentDB-API:

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

Vi läsa hello-objekt efter dess partitionsnyckel och id, uppdatera och som ett sista steg kan ta bort den av partitionsnyckel och ID: t. Observera att hello läsningar innehåller ett värde för PartitionKey (motsvarande toohello `x-ms-documentdb-partitionkey` huvudet i begäran i hello REST-API).

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

### <a name="querying-partitioned-containers"></a>Frågar partitionerade behållare
När du frågar efter data i partitionerade behållare, Cosmos DB automatiskt hello vägar toohello frågepartitioner motsvarande toohello partitionsnyckelvärden som angavs i hello filter (om det finns några). Den här frågan är till exempel routade toojust hello partition som innehåller hello partitionsnyckel ”XMS 0001”.

```csharp
// Query using partition key
IQueryable<DeviceReading> query = client.CreateDocumentQuery<DeviceReading>(
    UriFactory.CreateDocumentCollectionUri("db", "coll"))
    .Where(m => m.MetricType == "Temperature" && m.DeviceId == "XMS-0001");
```
    
hello följande fråga har inte ett filter på hello partitionsnyckel (DeviceId) och fanned ut tooall partitioner där den körs mot hello partition index. Observera att du har toospecify hello EnableCrossPartitionQuery (`x-ms-documentdb-query-enablecrosspartition` i hello REST-API) toohave hello SDK tooexecute en fråga över partitioner.

```csharp
// Query across partition keys
IQueryable<DeviceReading> crossPartitionQuery = client.CreateDocumentQuery<DeviceReading>(
    UriFactory.CreateDocumentCollectionUri("db", "coll"), 
    new FeedOptions { EnableCrossPartitionQuery = true })
    .Where(m => m.MetricType == "Temperature" && m.MetricValue > 100);
```

Har stöd för cosmos DB [mängdfunktionerna](documentdb-sql-query.md#Aggregates) `COUNT`, `MIN`, `MAX`, `SUM` och `AVG` över partitionerad behållare med SQL som börjar med SDK: er 1.12.0 och högre. Frågor måste innehålla en sammanställd operator och måste innehålla ett värde i hello projektion.

### <a name="parallel-query-execution"></a>Parallell frågekörning
Hej Cosmos DB SDK 1.9.0 och ovan stöd parallell körning frågealternativ, som gör att du tooperform låg latens frågor mot partitionerade samlingar, även när de behöver tootouch ett stort antal partitioner. Följande fråga hello är till exempel konfigurerade toorun parallellt över partitioner.

```csharp
// Cross-partition Order By Queries
IQueryable<DeviceReading> crossPartitionQuery = client.CreateDocumentQuery<DeviceReading>(
    UriFactory.CreateDocumentCollectionUri("db", "coll"), 
    new FeedOptions { EnableCrossPartitionQuery = true, MaxDegreeOfParallelism = 10, MaxBufferedItemCount = 100})
    .Where(m => m.MetricType == "Temperature" && m.MetricValue > 100)
    .OrderBy(m => m.MetricValue);
```
    
Du kan hantera parallell frågekörning genom att justera hello följande parametrar:

* Genom att ange `MaxDegreeOfParallelism`, du kan styra hello grad av parallellitet d.v.s. hello högsta tillåtna antalet samtidiga nätverket anslutningar toohello behållarens partitioner. Om du väljer för 1, hanteras hello grad av parallellitet av hello SDK. Om hello `MaxDegreeOfParallelism` har inte angetts eller ange too0, vilket är standardvärdet för hello, finns en enda nätverk anslutning toohello behållares partitioner.
* Genom att ange `MaxBufferedItemCount`, du kan handlar av frågan latens och klientsidan minnesanvändning. Om du utelämnar den här parametern eller Ställ in för 1 hanteras hello antal objekt som buffras under parallell frågekörning av hello SDK.

Angivna hello samma tillstånd mängden hello en parallell fråga returnerar resultat i hello samma ordning som seriella körning. När du utför fråga cross-partition som innehåller sortering (ORDER BY och/eller TOP), hello hello Azure Cosmos DB SDK problem fråga parallellt över partitioner och slår ihop delvis sorterade resulterar i hello klienten sida tooproduce globalt sorterade resultat.

### <a name="executing-stored-procedures"></a>Köra lagrade procedurer
Du kan också köra atomiska transaktioner mot dokument med hello samma enhets-ID, t.ex. Om du underhålla mängder eller hello senaste tillståndet för en enhet i ett enskilt objekt. 

```csharp
await client.ExecuteStoredProcedureAsync<DeviceReading>(
    UriFactory.CreateStoredProcedureUri("db", "coll", "SetLatestStateAcrossReadings"),
    new RequestOptions { PartitionKey = new PartitionKey("XMS-001") }, 
    "XMS-001-FE24C");
```
   
I nästa avsnitt om hello titta vi på hur du kan flytta toopartitioned behållare från enskild partition behållare.

## <a name="next-steps"></a>Nästa steg
I den här artikeln har sammanställt vi en översikt över hur toowork med partitionering av Azure Cosmos DB behållare med hello DocumentDB-API. Se även [partitionering och teckenbredden](../cosmos-db/partition-data.md) en översikt över begrepp och bästa praxis för partitionering med Azure Cosmos DB API: er. 

* Utföra skalnings- och prestandatester med Azure Cosmos DB. Se [prestanda och Skalningstester med Azure Cosmos DB](performance-testing.md) ett exempel.
* Komma igång med programmeringen med hello [SDK](documentdb-sdk-dotnet.md) eller hello [REST API](/rest/api/documentdb/)
* Lär dig mer om [etablerat dataflöde i Azure Cosmos DB](request-units.md)

