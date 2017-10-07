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
# <a name="working-with-hello-change-feed-support-in-azure-cosmos-db"></a>Arbeta med hello ändra feed stöd i Azure Cosmos DB
[Azure Cosmos-DB](../cosmos-db/introduction.md) är ett snabbt och flexibel globalt replikeras databastjänsten som används för att lagra stora volymer transaktionella och operativa data med förutsägbar en siffra millisekunders latens för läsningar och skrivningar. Detta gör det väl lämpade för IoT, återförsäljnings, spel och operativa loggningsprogram. En gemensam designmönstret i dessa program är tootrack ändringar gjorts tooAzure Cosmos DB data och uppdatera materialiserade vyer, genomföra analys i realtid, arkivera toocold datalagring och utlösare meddelanden på vissa händelser baserat på dessa ändringar. Hej **ändra feed stöd** i Azure Cosmos-databasen kan du toobuild skalbara lösningar för var och en av dessa mönster.

Med ändringen feed support, tillhandahåller Azure Cosmos DB en sorterad lista över dokument inom en samling Azure Cosmos DB i hello ordning de ändrades. Denna feed kan använda toolisten för ändringar toodata inom hello samlingen och utföra åtgärder som:

* Utlös ett anrop tooan API när ett dokument infogas eller ändras
* Utföra realtid (ström) bearbetning av uppdateringar
* Synkronisera data med en cache, sökmotor eller datalagret

Ändringar i Azure Cosmos DB kan sparas och bearbetas asynkront och distribuerade i en eller flera konsumenter för parallell bearbetning. Nu ska vi titta på hello API: er för att ändra feed och hur du kan använda dem toobuild skalbara realtidsprogram. Den här artikeln visar hur toowork med Azure Cosmos DB ändra feed och hello DocumentDB-API. 

![Med hjälp av Azure Cosmos DB ändra feed toopower analys i realtid och händelsedriven datascenarier](./media/change-feed/changefeedoverview.png)

> [!NOTE]
> Ändra feed support tillhandahålls endast för hello DocumentDB API vid detta tillfälle. hello Graph API och tabellen API stöds inte för närvarande.

## <a name="use-cases-and-scenarios"></a>Användningsfall och scenarier
Ändra feed möjliggör effektiv bearbetning av stora datamängder med en stor volym med skrivningar och erbjuder ett alternativt tooquerying hela datauppsättningar tooidentify vad som har ändrats. Exempelvis kan du utföra följande uppgifter effektivt hello:

* Uppdatera ett cacheminne, sökindex eller ett datalager med data som lagras i Azure Cosmos DB.
* Implementera programnivå data lagringsnivåer och arkivering, som är ”varm data” lagras i Azure Cosmos DB och föråldrat ”kalldata” för[Azure Blob Storage](../storage/common/storage-introduction.md) eller [Azure Data Lake Store](../data-lake-store/data-lake-store-overview.md).
* Implementera batchanalyser på data med hjälp av [Apache Hadoop](run-hadoop-with-hdinsight.md).
* Implementera [lambda pipelines på Azure](https://blogs.technet.microsoft.com/msuspartner/2016/01/27/azure-partner-community-big-data-advanced-analytics-and-lambda-architecture/) med Azure Cosmos DB. Azure Cosmos-DB tillhandahåller en skalbar databaslösning som kan hantera både införandet och fråga och implementera lambda arkitekturer med låg TCO. 
* Utföra noll driftstopp migreringar tooanother Azure DB som Cosmos-konto med en annan partitioneringsschema.

**Lambda Pipelines med Azure Cosmos DB för införandet och fråga:**

![Azure Cosmos-DB baserat lambda pipeline för införandet och fråga](./media/change-feed/lambda.png)

Du kan använda Azure Cosmos DB tooreceive och lagra händelsedata från enheter, sensorer, infrastruktur och program och bearbeta händelserna i realtid med [Azure Stream Analytics](../stream-analytics/stream-analytics-documentdb-output.md), [Apache Storm](../hdinsight/hdinsight-storm-overview.md), eller [Apache Spark](../hdinsight/hdinsight-apache-spark-overview.md). 

Inom din [serverlösa](http://azure.com/serverless) webb- och mobilappar, kan du spåra händelser som ändringar tooyour kundens profil, inställningar eller plats tootrigger vissa åtgärder som att skicka push-meddelanden tootheir enheter med hjälp av [Azure Functions](../azure-functions/functions-bindings-documentdb.md) eller [Apptjänster](https://azure.microsoft.com/services/app-service/). Om du använder Azure Cosmos DB toobuild spel kan du till exempel använda ändra feed tooimplement realtid ledartavlor baserat på resultat från slutförda spel.

## <a name="how-change-feed-works-in-azure-cosmos-db"></a>Hur ändringen feed fungerar i Azure Cosmos DB
Azure Cosmos-DB tillhandahåller hello möjlighet tooincrementally läsa uppdateringar som görs tooan Azure DB som Cosmos-samling. Ändra feeden har hello följande egenskaper:

* Ändringar är beständiga i Azure Cosmos-databasen och går att bearbeta asynkront.
* Toodocuments ändringar i en samling är tillgängliga omedelbart i hello ändra feed.
* Varje ändring tooa dokumentet visas exakt en gång i hello ändra feed och hantera sina kontrollpunkter logik för klienter. hello ändra feed processor biblioteket innehåller automatiska kontrollpunkter och ”minst en gång” semantik.
* Endast hello senaste ändringen för ett visst dokument ingår i hello ändringsloggen. Mellanliggande ändringar kan inte användas.
* hello ändra feed sorteras ordning ändras inom varje partitionsnyckelvärde. Det finns ingen garanterad ordning över partitionsnyckel värden.
* Ändringar som kan synkroniseras från alla i tidpunkt, det är ingen fast Datalagringsperiod ändringar är tillgängliga.
* Ändringar är tillgängliga i mängder partition nyckelintervall. Den här funktionen kan ändringar från stora samlingar toobe bearbetas parallellt av flera konsumenter-servrar.
* Program kan begära för ändring av flera flöden samtidigt på hello samma samling.

Azure Cosmos DB ändra feeden är aktiverad som standard för alla konton. Du kan använda din [etablerat dataflöde](request-units.md) i din region skrivning eller någon [läsa region](distribute-data-globally.md) tooread från hello ändra feed, precis som andra igen från Azure Cosmos-databasen. hello ändra feed innehåller infogningar och uppdateringsåtgärder gjorts toodocuments inom hello samlingen. Du kan avbilda borttagningar genom att ange en ”soft-ta bort” flagga i dokument i stället för borttagningar. Alternativt kan du ställa en begränsad utgångsperioden för dina dokument via hello [TTL kapaciteten](time-to-live.md)för exempelvis 24 timmar och Använd hello värdet för egenskapen toocapture tas bort. Med den här lösningen har ändrats och tooprocess inom ett kortare tidsintervall än hello TTL förfallotid perioden. hello ändra feed är tillgänglig för varje partitionsnyckelintervallet inom hello dokumentsamlingen och därför kan fördelas på en eller flera konsumenter för parallell bearbetning. 

![Distribuerad bearbetning av Azure Cosmos DB ändra feed](./media/change-feed/changefeedvisual.png)

Du har några alternativ i hur du implementerar en ändring feeds i din klientkod. hello avsnitten omedelbart Följ beskriva hur tooimplement hello ändra feed med hello Azure Cosmos DB REST-API och hello DocumentDB-SDK. För .NET-program, rekommenderar vi dock använda hello nya [ändra feed processor biblioteket](#change-feed-processor) för att bearbeta händelser från hello ändra feed som förenklar läsning ändringar mellan partitioner och gör att flera trådar som arbetar i parallellt. 

## <a id="rest-apis"></a>Arbeta med hello REST-API och DocumentDB-SDK
Azure Cosmos-DB tillhandahåller elastiska behållare för lagring och genomströmning kallas **samlingar**. Data i samlingar är logiskt grupperade med [partitions nycklar](partition-data.md) för skalbarhet och prestanda. Azure Cosmos-DB tillhandahåller olika API: er för att komma åt dessa data, inklusive sökning med ID (Läs/Get), fråga och Läs-feeds (sökningar). hello ändra feeden kan erhållas genom att fylla i två nya begäran huvuden toohello DocumentDB `ReadDocumentFeed` API, och kan bearbetas parallellt över intervallen för partitionsnycklar.

### <a name="readdocumentfeed-api"></a>ReadDocumentFeed API
Nu ska vi titta kort hur ReadDocumentFeed fungerar. Azure Cosmos-DB stöder läsning av en feed av dokument i en samling via hello `ReadDocumentFeed` API. Till exempel hello följande begäran returnerar en sida av dokument i hello `serverlogs` samling. 

    GET https://mydocumentdb.documents.azure.com/dbs/smalldb/colls/serverlogs HTTP/1.1
    x-ms-date: Tue, 22 Nov 2016 17:05:14 GMT
    authorization: type%3dmaster%26ver%3d1.0%26sig%3dgo7JEogZDn6ritWhwc5hX%2fNTV4wwM1u9V2Is1H4%2bDRg%3d
    Cache-Control: no-cache
    x-ms-consistency-level: Strong
    User-Agent: Microsoft.Azure.Documents.Client/1.10.27.5
    x-ms-version: 2016-07-11
    Accept: application/json
    Host: mydocumentdb.documents.azure.com

Resultat kan begränsas med hjälp av hello `x-ms-max-item-count` sidhuvud och läser återupptas med skickar på nytt hello begäran med en `x-ms-continuation` huvud returneras i föregående hello-svar. Om den utförs från en enskild klient `ReadDocumentFeed` går igenom resultaten seriellt över partitioner. 

**Seriell läsa dokumentet feed**

Du kan också hämta hello feed dokument genom att använda en av hello stöds [Azure Cosmos DB SDK](documentdb-sdk-dotnet.md). Till exempel hello följande fragment visas hur toouse hello [ReadDocumentFeedAsync metoden](/dotnet/api/microsoft.azure.documents.client.documentclient.readdocumentfeedasync?view=azure-dotnet) i .NET.

```csharp
FeedResponse<dynamic> feedResponse = null;
do
{
    feedResponse = await client.ReadDocumentFeedAsync(collection, new FeedOptions { MaxItemCount = -1 });
}
while (feedResponse.ResponseContinuation != null);
```

### <a name="distributed-execution-of-readdocumentfeed"></a>Distribuerade körning av ReadDocumentFeed
Seriell körningen av skrivskyddade feed från en enskild klientdator kanske inte praktiska för samlingar som innehåller data eller flera terabyte eller mata in ett stort antal uppdateringar. I ordning toosupport scenarierna stordata Azure Cosmos DB innehåller API: er toodistribute `ReadDocumentFeed` anrop transparent över flera klienten läsare konsumenter. 

**Distribuerade läsa dokumentet Feed**

tooprovide skalbara bearbetning av inkrementella ändringar, Azure Cosmos DB stöder en skalbar modell för hello ändra feed API baserat på intervallen för partitionsnycklar.

* Du kan hämta en lista över partition nyckelintervall för en samling som utför en `ReadPartitionKeyRanges` anropa. 
* För varje partitionsnyckelintervallet kan du utföra en `ReadDocumentFeed` tooread dokument med partitionsnycklar inom det intervallet.

### <a name="retrieving-partition-key-ranges-for-a-collection"></a>Hämtar partition nyckelintervall för en samling
Du kan hämta hello partition nyckelintervall med begärande hello `pkranges` resurs i en samling. Till exempel hello följande begäran hämtar hello lista över partition nyckelintervall för hello `serverlogs` samling:

    GET https://querydemo.documents.azure.com/dbs/bigdb/colls/serverlogs/pkranges HTTP/1.1
    x-ms-date: Tue, 15 Nov 2016 07:26:51 GMT
    authorization: type%3dmaster%26ver%3d1.0%26sig%3dEConYmRgDExu6q%2bZ8GjfUGOH0AcOx%2behkancw3LsGQ8%3d
    x-ms-consistency-level: Session
    x-ms-version: 2016-07-11
    Accept: application/json
    Host: querydemo.documents.azure.com

Denna begäran returnerar hello efter svar som innehåller metadata om hello partition viktiga områden:

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


**Partitionera viktiga intervallet egenskaper**: varje partitionsnyckelintervallet innehåller hello metadataegenskaper i den följande tabellen hello:

<table>
    <tr>
        <th>Huvudets namn</th>
        <th>Beskrivning</th>
    </tr>
    <tr>
        <td>id</td>
        <td>
            <p>hello-ID för hello partitionsnyckelintervallet. Det här är ett stabilt och unikt ID i varje samling.</p>
            <p>Måste användas i hello följande anrop tooread ändringar av partitionsnyckelintervallet.</p>
        </td>
    </tr>
    <tr>
        <td>maxExclusive</td>
        <td>hello maximala partition nyckel-hash-värde för hello partitionsnyckelintervallet. För intern användning.</td>
    </tr>
    <tr>
        <td>minInclusive</td>
        <td>hello minsta partition nyckel-hash-värde för hello partitionsnyckelintervallet. För intern användning.</td>
    </tr>       
</table>

Du kan göra detta med hjälp av en av hello stöds [Azure Cosmos DB SDK](documentdb-sdk-dotnet.md). Till exempel hello följande utdrag visar hur tooretrieve partitionsnyckel intervall i .NET med hjälp av hello [ReadPartitionKeyRangeFeedAsync](/dotnet/api/microsoft.azure.documents.client.documentclient.readpartitionkeyrangefeedasync?view=azure-dotnet) metod.

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

Azure Cosmos-DB stöder hämtning av dokument per partitionsnyckelintervallet genom att ange hello valfria `x-ms-documentdb-partitionkeyrangeid` huvud. 

### <a name="performing-an-incremental-readdocumentfeed"></a>Utför en inkrementell ReadDocumentFeed
ReadDocumentFeed stöder hello följande scenarier/uppgifter för inkrementell behandling av ändringar i Azure Cosmos DB samlingar:

* Läs alla ändras toodocuments från början hello, det vill säga från skapa en samling.
* Läs alla ändringar toofuture uppdateringar toodocuments från aktuell tid eller ändringar sedan en tid som angetts av användaren.
* Läsa alla ändringar toodocuments från en logisk version av hello samling (ETag). Du kan kontrollpunkt som dina användare baserat på hello ETag som returnerades från inkrementella Läs-feed-begäranden.

hello ändringar är INSERT och Update toodocuments. tar bort toocapture, du måste använda en ”mjuk borttagning”-egenskap i dokument eller använda hello [inbyggd TTL-egenskap](time-to-live.md) toosignal en väntande borttagning i hello ändra feed.

följande tabell visar hello hello [begäran](/rest/api/documentdb/common-documentdb-rest-request-headers.md) och [svarshuvuden](/rest/api/documentdb/common-documentdb-rest-response-headers.md) för ReadDocumentFeed åtgärder.

**Begärandehuvuden för inkrementell ReadDocumentFeed**:

<table>
    <tr>
        <th>Huvudets namn</th>
        <th>Beskrivning</th>
    </tr>
    <tr>
        <td>EN IM</td>
        <td>Måste anges för ”stegvis feed”, eller på annat sätt utelämnad</td>
    </tr>
    <tr>
        <td>If-None-Match</td>
        <td>
            <p>Inget huvud: returnerar alla ändringar från hello från (skapa samling)</p>
            <p>”*”: returnerar alla nya ändringar toodata inom hello samlingen</p>         
            <p>&lt;ETag&gt;: om ange tooa samling ETag, returneras alla ändringar som gjorts sedan den logiska tidsstämpeln</p>
        </td>
    </tr>
    <tr>    
        <td>If-Modified-Since</td> 
        <td>RFC 1123 tidsformat; ignoreras om If-None-Match anges</td> 
    </tr> 
    <tr>
        <td>x-ms-documentdb-partitionkeyrangeid</td>
        <td>hello partition viktiga intervallet ID för läsning av data.</td>
    </tr>
</table>

**Svarshuvuden för inkrementell ReadDocumentFeed**:

<table> <tr>
        <th>Huvudets namn</th>
        <th>Beskrivning</th>
    </tr>
    <tr>
        <td>ETag</td>
        <td>
            <p>hello logiska sekvensnumret (LSN) för senaste dokument i hello svar.</p>
            <p>Inkrementell ReadDocumentFeed återupptas med det här värdet i If-None-Match skickar på nytt.</p>
        </td>
    </tr>
</table>

Här är ett exempel begäran tooreturn alla inkrementella ändringar i samlingen från hello logiska version/ETag `28535` och partitionera viktiga intervallet = `16`:

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

Ändringar ordnas efter tid inom varje partitionsnyckelvärde inom hello partitionsnyckelintervallet. Det finns ingen garanterad ordning över partitionsnyckel värden. Om det finns fler resultat än vad som får plats i en enstaka sida, du kan läsa hello nästa sida i resultatet av skickar på nytt hello begäran med hello `If-None-Match` huvud med värde lika toohello `etag` från föregående hello-svar. Om flera dokument har infogats eller uppdaterats transaktionellt inom en lagrad procedur eller utlösare, de alla returneras inom hello samma svar sidan.

> [!NOTE]
> Med ändringen feed du få fler objekt som returneras i en sida än i `x-ms-max-item-count` hello gäller flera dokument infogas eller uppdateras i lagrade procedurer eller utlösare. 

När du använder hello .NET SDK (1.17.0), ange hello fältet `StartTime` i `ChangeFeedOptions` toodirectly returnerade ändras dokument eftersom `StartTime` vid anrop av `CreateDocumentChangeFeedQuery`. Genom att ange `If-Modified-Since` med hello REST-API kan din begäran returnerar inte hello dokument själva, men i stället hello fortsättningstoken eller `etag` i hello-Svarsrubrik. tooreturn hello dokument ändrade hello angiven tid, hello fortsättningstoken `etag` sedan måste användas i hello nästa begäran med `If-None-Match` tooreturn hello faktiska dokument. 

hello .NET SDK innehåller hello [CreateDocumentChangeFeedQuery](/dotnet/api/microsoft.azure.documents.client.documentclient.createdocumentchangefeedquery?view=azure-dotnet) och [ChangeFeedOptions](/dotnet/api/microsoft.azure.documents.client.changefeedoptions?view=azure-dotnet) helper klasserna tooaccess ändringar tooa samling. hello följande utdrag visar hur tooretrieve alla ändras från hello börjar med hello .NET SDK från en enskild klient.

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
Och hello följande utdrag visar hur tooprocess ändras i realtid med Azure Cosmos DB med hjälp av hello ändra feed support och hello före funktionen. hello första anropet returnerar alla hello dokument i hello samling och hello andra returnerar bara hello två dokument som har skapats och som har skapats sedan den senaste kontrollpunkten för hello.

```csharp
// Returns all documents in hello collection.
Dictionary<string, string> checkpoints = await GetChanges(client, collection, new Dictionary<string, string>());

await client.CreateDocumentAsync(collection, new DeviceReading { DeviceId = "xsensr-201", MetricType = "Temperature", Unit = "Celsius", MetricValue = 1000 });
await client.CreateDocumentAsync(collection, new DeviceReading { DeviceId = "xsensr-212", MetricType = "Pressure", Unit = "psi", MetricValue = 1000 });

// Returns only hello two documents created above.
checkpoints = await GetChanges(client, collection, checkpoints);
```

Du kan också filtrera hello ändra feed med klienten sida logik tooselectively bearbeta händelser. Här är till exempel ett kodavsnitt som använder klienten sida LINQ tooprocess endast temperatur ändra händelser från enheten sensorer.

```csharp
FeedResponse<DeviceReading> readChangesResponse = query.ExecuteNextAsync<DeviceReading>().Result;

foreach (DeviceReading changedDocument in 
    readChangesResponse.AsEnumerable().Where(d => d.MetricType == "Temperature" && d.MetricValue > 1000L))
{
    // trigger an action, like call an API
}
```

## <a id="change-feed-processor"></a>Ändra Feed Processor bibliotek
Ett annat alternativ är toouse hello [Azure Cosmos DB ändra Feed Processor biblioteket](https://docs.microsoft.com/azure/cosmos-db/documentdb-sdk-dotnet-changefeed), som kan hjälpa dig att enkelt distribuera händelsebearbetning från en ändring feed över flera konsumenter. hello-biblioteket är bra för att skapa ändra feed läsare på hello .NET-plattformen. Vissa arbetsflöden som skulle förenklas genom att använda hello ändra Feed Processor bibliotek över hello-metoder som ingår i hello andra Cosmos DB SDK: er omfattar: 

* Ta emot uppdateringar från ändra feed när lagras data över flera partitioner
* Flytta eller replikering av data från en samling tooanother
* Parallell körning av åtgärder som utlöses av uppdateringar toodata och ändrar feed 

Medan med hello API: er i hello Cosmos SDK innehåller exakt åtkomst toochange feeds uppdateringar i varje partition, förenklar med hjälp av hello ändra Feed Processor bibliotek läsning ändringar i partitioner och flera trådar som arbetar parallellt. I stället för att manuellt läsa ändringarna från varje behållare och spara en fortsättningstoken för varje partition, hanterar hello ändra Feed Processor automatiskt läsning ändringar över partitioner som använder en mekanism för lånet.

hello bibliotek är tillgängligt som ett NuGet-paket: [Microsoft.Azure.Documents.ChangeFeedProcessor](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB.ChangeFeedProcessor/) och från källkoden som en Github [exempel](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/ChangeFeedProcessor). 

### <a name="understanding-change-feed-processor-library"></a>Förstå ändra Feed Processor bibliotek 

Det finns fyra huvudsakliga komponenter för att implementera hello ändra Feed Processor: hello övervakas samling, hello lån samling, hello värd för händelsebearbetning och hello konsumenter. 

**Insamling av övervakade:** hello övervakas samlingen är hello data från vilken hello ändra feed genereras. En INSERT och ändringar övervakas toohello samling återspeglas i hello ändra feed hello mängden. 

**Insamling av lånet:** hello lån samling koordinater bearbetning hello ändra feed över flera arbetare. En separat samling är används toostore hello lån med en leasing per partition. Är det fördelaktigt toostore samlingen lån på ett annat konto med hello skriva region närmare toowhere hello ändra Feed Processor körs. Ett lån-objekt innehåller hello följande attribut: 
* Ägare: Anger hello-värd som äger hello lån
* Fortsättning: Anger hello-läge (fortsättningstoken) i hello ändra flödet för en viss partition
* Tidsstämpel: Tid för senaste lån uppdaterades; Hej, tidsstämpel kan vara används toocheck om hello lån betraktas som upphört att gälla 

**Värd för händelsebearbetning:** varje värd anger hur många partitioner tooprocess baserat på hur många andra instanser av värdar har aktiva lån. 
1.  När en värd startas, skaffar lån toobalance hello arbetsbelastningen över alla värdar. En värd förnyas regelbundet lån, så att förbli lån. 
2.  Ett värden kontrollpunkter hello senaste fortsättning token tooits lån för varje läsning. tooensure samtidighet säkerhet en värd kontrollerar hello ETag för varje lease-uppdatering. Andra kontrollpunkt strategier stöds också.  
3.  En värd släpper alla lån vid avstängning, men behåller hello fortsättning information så att den kan återupptas senare läsning från hello lagrade kontrollpunkten. 

Hello antalet värdar kan inte vara större än hello antalet partitioner (lån) just nu.

**Konsumenterna:** konsumenter eller arbetare, är trådar som utför hello ändra feed bearbetning som initieras av varje värd. Varje värd för händelsebearbetning kan ha flera konsumenter. Varje konsument läser hello ändra feed från hello partition som den är tilldelad tooand meddelar dess värden för ändringar och gått lån.

toofurther förstå hur dessa fyra element i ändra Feed Processor tillsammans, ska vi titta på ett exempel i följande diagram hello. hello övervakas samling lagrar dokument och använder hello ”stad” som hello partitionsnyckel. Vi kan se att hello blå partition innehåller dokument med hello ”stad” från ”A E” och så vidare. Det finns två värdar med två konsumenter läsning från hello fyra partitioner parallellt. hello pilarna visar hello konsumenter läsning från en specifik plats i hello ändra feed. I hello första partition representerar hello mörkare blå oläst ändringar medan hello lätta blå representerar hello redan läsa ändringarna på hello ändra feed. hello värdar använder hello lån samling toostore ”fortsättning” värde tookeep reda på hello aktuella läsning placering för varje konsument. 

![Med hjälp av hello Azure Cosmos DB ändra feed värd för händelsebearbetning](./media/change-feed/changefeedprocessornew.png)

### <a name="using-change-feed-processor-library"></a>Med hjälp av ändra Feed Processor-bibliotek 
hello följande avsnitt förklarar hur toouse hello ändra Feed Processor bibliotek i hello kontexten för att replikera ändringar från en källa samling tooa mål-samling. Här är hello källsamling hello övervakas samling i ändra Feed Processor. 

**Installera och inkludera hello ändra Feed Processor NuGet-paketet** 

Innan du installerar ändra Feed Processor NuGet-paketet måste först installera: 
* Microsoft.Azure.DocumentDB, version 1.13.1 eller senare 
* Newtonsoft.Json, version 9.0.1 eller senare installera `Microsoft.Azure.DocumentDB.ChangeFeedProcessor` och inkludera den som en referens.

**Skapa en övervakas lån och mål-samling** 

I ordning toouse hello ändra Feed Processor bibliotek måste hello lån samling toobe skapats innan du kör hello processor värddatorer. Vi rekommenderar igen och lagra en samling lån på ett annat konto med en skrivning region närmare toowhere hello ändra Feed Processor körs. I exemplet data movement måste vi toocreate hello målsamling innan du kör hello ändra Feed Processor värden. I hello exempelkod vi kallar helper metoden toocreate hello övervakas, hyrd, och mål samlingar om de inte redan finns. 

> [!WARNING]
> Skapa en samling har prissättning effekter, som du reserverar genomströmning för hello programmet toocommunicate med Azure Cosmos DB. Mer information finns i hello [sida med priser](https://azure.microsoft.com/pricing/details/cosmos-db/)
> 
> 

*Skapa en värd för händelsebearbetning*

Hej `ChangeFeedProcessorHost` klassen innehåller en trådsäker, flerprocessig, säker körningsmiljö för händelseprocessorer som också ger kontrollpunkter och hantering av partitionsleasing. toouse hello `ChangeFeedProcessorHost` klassen, som du kan implementera `IChangeFeedObserver`. Det här gränssnittet innehåller tre metoder:

* `OpenAsync`: Den här funktionen kallas när ändringen feed person öppnas. Det kan vara ändrade tooperform en specifik åtgärd när konsumenten/person öppnas.  
* `CloseAsync`: Den här funktionen anropas när ändringen feed person avslutas. Det kan vara ändrade tooperform en specifik åtgärd när konsumenten/person är stängd.  
* `ProcessChangesAsync`: Den här funktionen kallas när dokumentet nya ändringar är tillgängliga på Ändra feed. Det kan vara ändrade tooperform en specifik åtgärd vid varje ändring feed-uppdatering.  

I vårt exempel vi implementera gränssnittet hello `IChangeFeedObserver` via hello `DocumentFeedObserver` klass. Här hello `ProcessChangesAsync` fungerar upserts (uppdateringar) ett dokument från ändra matas in hello målsamling. Det här exemplet är användbart för att flytta data från en samling tooanother i ordning toochange hello partitionsnyckel av en datamängd. 

*Kör hello värd för händelsebearbetning*

Innan du påbörjar händelsebearbetning både ändra alternativ för feed och ändra feed värdalternativ kan anpassas. 
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
hello specifika fält som kan anpassas sammanfattas i följande tabeller hello. 

**Ändra alternativ för feeds**:
<table>
    <tr>
        <th>Egenskapsnamn</th>
        <th>Beskrivning</th>
    </tr>
    <tr>
        <td>MaxItemCount</td>
        <td>Hämtar eller anger hello maximalt antal objekt toobe returneras i hello uppräkningsåtgärden avbröts i hello Azure Cosmos DB database-tjänsten.</td>
    </tr>
    <tr>
        <td>PartitionKeyRangeId</td>
        <td>Hämtar eller anger hello partitions viktiga intervallet-id för hello aktuella begäran i hello Azure Cosmos DB database-tjänsten.</td>
    </tr>
    <tr>
        <td>RequestContinuation</td>
        <td>Hämtar eller anger hello begäran fortsättningstoken i hello Azure Cosmos DB database-tjänsten.</td>
    </tr>
        <tr>
        <td>SessionToken</td>
        <td>Hämtar eller anger hello sessionstoken för användning med sessionskonsekvens i hello Azure Cosmos DB database-tjänsten.</td>
    </tr>
        <tr>
        <td>StartFromBeginning</td>
        <td>Hämtar eller anger om ändringen feeds i hello Azure Cosmos DB database-tjänsten ska börja från hello från (SANT) eller från aktuella (FALSKT). Som standard startar från aktuella (FALSKT).</td>
    </tr>
</table>

**Ändra värdalternativ för feeds**:
<table>
    <tr>
        <th>Egenskapsnamn</th>
        <th>Typ</th>
        <th>Beskrivning</th>
    </tr>
    <tr>
        <td>LeaseRenewInterval</td>
        <td>TimeSpan</td>
        <td>hello-intervall för alla lån för partitioner som för tillfället hålls av hello ChangeFeedEventHost instans.</td>
    </tr>
    <tr>
        <td>LeaseAcquireInterval</td>
        <td>TimeSpan</td>
        <td>Hej intervall tookick av en aktivitet toocompute om partitioner fördelas jämnt mellan kända värddatorinstanser.</td>
    </tr>
    <tr>
        <td>LeaseExpirationInterval</td>
        <td>TimeSpan</td>
        <td>hello-intervall för vilka hello lån utförs på ett lån som representerar en partition. Om hello leasen inte förnyas inom intervallet, den har upphört att gälla och ägarskapet för hello partition flyttas tooanother ChangeFeedEventHost instans.</td>
    </tr>
    <tr>
        <td>FeedPollDelay</td>
        <td>TimeSpan</td>
        <td>hello fördröjning mellan avsöker en partition för nya ändringar på hello feed när alla aktuella ändringar är tar slut.</td>
    </tr>
    <tr>
        <td>CheckpointFrequency</td>
        <td>CheckpointFrequency</td>
        <td>hello frekvens toocheckpoint lån.</td>
    </tr>
    <tr>
        <td>MinPartitionCount</td>
        <td>int</td>
        <td>hello minsta partitionsantal för hello värden.</td>
    </tr>
    <tr>
        <td>MaxPartitionCount</td>
        <td>int</td>
        <td>hello maximalt antal partitioner hello värden kan fungera.</td>
    </tr>
    <tr>
        <td>DiscardExistingLeases</td>
        <td>bool</td>
        <td>Om hello starta på hello värdens alla befintliga lån ska tas bort och hello värden ska börja från början.</td>
    </tr>
</table>


Skapa en instans av toostart händelsebearbetning, `ChangeFeedProcessorHost`, tillhandahåller hello lämpliga parametrar för din Azure DB som Cosmos-samling. Anropa sedan `RegisterObserverAsync` tooregister din `IChangeFeedObserver` (DocumentFeedObserver i det här exemplet) implementering med hello körning. Hello-värden försöker nu tooacquire ett lån på varje partitionsnyckelintervallet i hello Azure Cosmos DB samlingen med en ”girig” algoritm. De här leasingarna varar en viss tid och måste sedan förnyas. Allteftersom nya noder worker-instanser i det här fallet är online, de placerar den ut leasingreservationer och med tiden skiftar hello belastningen mellan noder som varje värd försöker tooacquire mer lån. 

I hello exempelkod använder vi en fabrik klass (DocumentFeedObserverFactory.cs) toocreate en person och hello `RegistObserverFactoryAsync` tooregister hello person. 

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
Med tiden etableras ett jämviktsläge. Den här dynamiska funktionen gör att processorbaserad automatisk skalning toobe tillämpas tooconsumers för både skala upp och skala ned. Om ändringarna är tillgängliga i Azure Cosmos DB i en snabbare takt än konsumenterna kan bearbeta kan hello CPU för konsumenterna vara används toocause Autoskala på worker-instanser.

## <a name="next-steps"></a>Nästa steg
* Försök hello [Azure Cosmos DB ändra feed kodexempel på GitHub](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/code-samples/ChangeFeed)
* Komma igång med programmeringen med hello [Azure Cosmos DB SDK](documentdb-sdk-dotnet.md) eller hello [REST API](/rest/api/documentdb/).
