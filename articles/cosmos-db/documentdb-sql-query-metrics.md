---
title: "aaaSQL fråga mätvärden för Azure Cosmos DB DocumentDB API | Microsoft Docs"
description: "Läs mer om hur tooinstrument och felsökningsloggar hello frågeprestanda för SQL Azure DB som Cosmos-begäranden."
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
ms.openlocfilehash: 2fee3786b7d48d254162699471943e316764b003
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tuning-query-performance-with-azure-cosmos-db"></a>Justera prestanda för frågor med Azure Cosmos DB
Azure Cosmos-DB tillhandahåller en [SQL API för datafrågor](documentdb-sql-query.md), utan att schemat eller sekundärindex. Den här artikeln innehåller följande information för utvecklare hello:

* Utförlig information om hur fungerar Azure Cosmos DB SQL-frågan
* Information om frågan begärande- och svarshuvuden och alternativ för klient-SDK
* Tips och bästa praxis för prestanda för frågor
* Exempel på hur tooutilize SQL körning statistik toodebug fråga prestanda

## <a name="about-sql-query-execution"></a>Om körning av SQL-fråga

I Azure Cosmos DB du lagra data i behållare, som kan växa tooany [storlek eller begäran genomflödet](partition-data.md). Azure Cosmos-DB skalas sömlöst data över fysiska partitioner under hello omfattar toohandle datatillväxt eller ökning dataflöde. Du kan utfärda SQL-frågor tooany behållare med hello REST API eller en av hello stöds [DocumentDB SDK: er](documentdb-sdk-dotnet.md).

En kort översikt över partitionering: du anger en partitionsnyckel som ”stad”, som avgör hur data delas mellan fysiska partitioner. Data som tillhör tooa enda partitionsnyckel (till exempel ”stad” == ”Seattle”) lagras i en fysiska partition, men en enda fysisk partition har vanligtvis flera partitionsnycklar. När en partition når lagringsstorlek hello service sömlöst delar hello partition i två nya partitioner och dividerar hello partitionsnyckel jämnt mellan dessa partitioner. Eftersom partitioner är tillfälligt använder hello API: er en abstraktion av en ”partitionsnyckelintervallet”, som anger hello intervallen för partition viktiga hashvärden. 

När du skickar en fråga tooAzure Cosmos DB utför stegen logiska hello SDK:

* Parsa hello SQL-frågan toodetermine hello körning frågeplanen. 
* Om hello frågan innehåller ett filter mot hello partitionsnyckel, som `SELECT * FROM c WHERE c.city = "Seattle"`, är det routade tooa enkel partition. Om hello frågan har inte ett filter på partitionsnyckel, sedan den körs i alla partitioner och resultat kombineras på klientsidan.
* hello frågan köras inom varje partition i serien eller parallell, baserat på klientens konfiguration. Inom varje partition hello fråga kan göra att en eller flera sändningar beroende på hello frågan komplexitet konfigurerad sidstorlek och etablerat dataflöde hello mängden. Varje körning returnerar hello antalet [programbegäran](request-units.md) används genom att köra frågor och eventuellt statistik för körning av frågan. 
* hello SDK utför en sammanfattning av hello frågeresultat över partitioner. Till exempel om hello fråga innehåller en ORDER BY över partitioner, är sedan resultaten från enskilda partitioner merge sorteras tooreturn resultat i globalt sorteringsordningen. Om hello frågan är en samling som `COUNT`, hello från enskilda partitioner är samlade tooproduce hello övergripande antal.

hello SDK tillhandahåller olika alternativ för frågekörning. Till exempel i .NET dessa alternativ är tillgängliga i hello `FeedOptions` klass. hello följande tabell beskrivs dessa alternativ och hur de påverkar körningstid för frågan. 

| Alternativ | Beskrivning |
| ------ | ----------- |
| `EnableCrossPartitionQuery` | Du måste ange tootrue för alla frågor som kräver toobe som körs i mer än en partition. Det här är en explicit flaggan tooenable du toomake medvetna prestanda kompromisser under utveckling. |
| `EnableScanInQuery` | Måste anges tootrue om du har valt att inte indexera, men ändå vill toorun hello frågan via en genomsökning. Endast tillgängligt för indexering för hello begärt filter sökväg är inaktiverad. | 
| `MaxItemCount` | hello maximalt antal objekt tooreturn per serveranrop toohello server. Du kan låta hello server hantera hello antal objekt genom att ange för 1. Eller så kan du sänka detta värde tooretrieve litet antal objekt per onödig kommunikation. 
| `MaxBufferedItemCount` | Detta är ett alternativ för klientsidan och används toolimit hello minnesförbrukning när du utför cross-partition ORDER BY. Ett högre värde hjälper till att minska hello svarstiden för cross-partition sortering. |
| `MaxDegreeOfParallelism` | Hämtar eller anger hello antalet samtidiga åtgärder som körs på klientsidan under parallell frågekörning i hello Azure DocumentDB database-tjänsten. Ett positivt egenskapsvärde begränsar hello antal samtidiga åtgärder toohello ange värde. Om den anges tooless än 0 beslutar hello system automatiskt hello antalet samtidiga åtgärder toorun. |
| `PopulateQueryMetrics` | Aktiverar detaljerad loggning av statistik tid som ägnats åt olika faser i Frågekörningen som kompileringstid, indexet loop tid och dokument laddas gång. Utdata från frågan statistik kan du dela med Azure-supporten toodiagnose frågan prestandaproblem. |
| `RequestContinuation` | Du kan återuppta körning av fråga genom att passera i täckande hello fortsättningstoken som returnerades av en fråga. Hej fortsättningstoken kapslar in alla tillstånd som krävs för frågekörning. |
| `ResponseContinuationTokenLimitInKb` | Du kan begränsa hello maxstorleken för hello fortsättningstoken som returnerades av hello-servern. Du kan behöva tooset detta om din programvärden har gränser för huvudstorlek. Anger det här kan öka hello övergripande varaktighet och RUs som används för hello frågan.  |

Till exempel ta en exempelfråga på partitionsnyckel som efterfrågas på en samling med `/city` som hello partition nyckel och etablerats med 100 000 RU/s genomströmning. Du begär den här frågan med `CreateDocumentQuery<T>` i .NET hello följande:

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

Hej SDK fragment ovan, motsvarar toohello på REST API-begäran:

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

Varje sida för körning av fråga motsvarar tooa REST API `POST` med hello `Accept: application/query+json` sidhuvud och hello SQL-frågan i hello brödtext. Varje fråga gör en eller flera avrunda resor toohello server med hello `x-ms-continuation` token eko mellan hello klienten och servern tooresume körning. hello konfigurationsalternativen i FeedOptions skickas toohello server i hello form av huvuden för begäran. Till exempel `MaxItemCount` motsvarar för`x-ms-max-item-count`. 

hello-begäran returnerar hello följande (trunkerad för läsbarhet) svar:

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

viktiga hello svarshuvuden som returnerades från frågan hello inkludera hello följande:

| Alternativ | Beskrivning |
| ------ | ----------- |
| `x-ms-item-count` | hello antal objekt som returneras i hello svar. Detta är beroende av hello angivna `x-ms-max-item-count`, hello antal objekt som ryms inom hello maximala nyttolasten svarsstorlek hello etablerat dataflöde och körningstid för frågan. |  
| `x-ms-continuation:` | hello fortsättning token tooresume körning av hello fråga om ytterligare resultat är tillgängliga. | 
| `x-ms-documentdb-query-metrics` | hello frågan statistik för hello körning. Det här är en avgränsad sträng som innehåller statistik över tid i hello olika faser i frågan. Returneras om `x-ms-documentdb-populatequerymetrics` har angetts för`True`. | 
| `x-ms-request-charge` | Hej antalet [programbegäran](request-units.md) förbrukas av hello frågan. | 

Mer information om hello REST API-huvuden för begäran och alternativ finns [förfrågan efter resurser med hjälp av hello DocumentDB REST API](https://docs.microsoft.com/rest/api/documentdb/querying-documentdb-resources-using-the-rest-api).

## <a name="best-practices-for-query-performance"></a>Bästa praxis för prestanda för frågor
hello följande är hello vanligaste faktorer som påverkar Azure Cosmos DB frågeprestanda. Vi gräva djupare i var och en av dessa avsnitt i den här artikeln.

| Faktor | Tips | 
| ------ | -----| 
| Etablerat dataflöde | Mät RU per fråga och se till att du har etablerat dataflöde för hello krävs för dina frågor. | 
| Partitionering och partitionsnycklar | Ge företräde åt frågor med hello partitionsnyckelvärde i hello filtersatsen för låg latens. |
| SDK-och frågealternativ | Följ Metodtips för SDK som direkt anslutning och finjustera frågealternativ körning på klientsidan. |
| Svarstid för nätverk | Kontot för nätverk omkostnader i mått och använda flera API: er tooread från hello närmsta region. |
| Indexprincip | Se till att du har hello krävs sökvägar/indexprincip för hello frågan. |
| Mätvärden för körning av frågan | Analysera hello frågan körning mått tooidentify potentiella omskrivningar av frågor och former.  |

### <a name="provisioned-throughput"></a>Etablerat dataflöde
Skapa behållare av data med reserverat dataflöde, uttryckt i begäran enheter (RU) per sekund i Cosmos DB. En läsning av ett dokument på 1 KB är 1 RU och varje åtgärd (inklusive frågor) är normaliserat tooa fast antal RUs baserat på dess komplexitet. Till exempel om du har 1000 RU/s som skapats för din behållare och du har en fråga som `SELECT * FROM c WHERE c.city = 'Seattle'` som förbrukar 5 RUs och du kan utföra (1000 RU/s) / (5 RU/query) = 200 frågan/s sådana frågor per sekund. 

Om du skickar fler än 200 per sekund startar hello-tjänsten hastighetsbegränsande inkommande begäranden över 200/s. hello SDK: er hanterar automatiskt det här fallet genom att utföra en backoff/försök, därför ser du kanske en högre latens för dessa frågor. Öka hello etablerat dataflöde toohello krävs värdet förbättrar dina svarstid och genomströmning. 

toolearn mer information om frågeenheter, se [programbegäran](request-units.md).

### <a name="partitioning-and-partition-keys"></a>Partitionering och partitionsnycklar
Med Azure Cosmos DB vanligtvis utför frågor i hello ordning från snabbaste/mest effektiva tooslower/mindre effektiva. 

* HÄMTA på en enda partitionsnyckel och objektet nyckel
* Frågan med en filtersats på en enda partitionsnyckel
* Fråga utan en likhet eller intervallet filtersats om en egenskap
* Fråga utan filter

Frågor som behöver tooconsult alla partitioner behovet av högre latens och kan använda högre RUs. Eftersom varje partition har automatisk indexering mot alla egenskaper, kan hello fråga hanteras effektivt från hello index i det här fallet. Du kan skapa frågor som sträcker sig över partitioner snabbare genom att använda hello parallellitet alternativ.

toolearn mer information om partitionering och partitionsnycklar, se [partitionering i Azure Cosmos DB](partition-data.md).

### <a name="sdk-and-query-options"></a>SDK-och frågealternativ
Se [prestandatips](performance-tips.md) och [prestandatester](performance-testing.md) för hur tooget hello bäst klientsidans prestanda från Azure Cosmos DB. Detta innefattar att använda hello senaste SDK: er, konfigurera plattformsspecifika konfigurationer som Standardantal anslutningar, frekvensen för skräpinsamling, och använda lightweight anslutningsalternativ som direkt/TCP. 


#### <a name="max-item-count"></a>Max antal
Hej värdet för frågor `MaxItemCount` kan ha en betydande inverkan på Frågetid för slutpunkt till slutpunkt. Varje rundtur toohello returnerar servern högst hello antal objekt i `MaxItemCount` (standard 100 objekt). Tooa högre värdet (-1 är högsta och rekommenderade) förbättrar din övergripande varaktighet för frågan genom att begränsa hello antal turer mellan servern och klienten, särskilt för frågor med stora resultatuppsättningar.

```cs
IDocumentQuery<dynamic> query = client.CreateDocumentQuery(
    UriFactory.CreateDocumentCollectionUri(DatabaseName, CollectionName), 
    "SELECT * FROM c WHERE c.city = 'Seattle'", 
    new FeedOptions 
    { 
        MaxItemCount = -1, 
    }).AsDocumentQuery();
```

#### <a name="max-degree-of-parallelism"></a>Max grad av parallellitet
För frågor finjustera hello `MaxDegreeOfParallelism` tooidentify hello rekommenderade konfigurationer för programmet, särskilt om du utför mellan partition-frågor (utan ett filter på hello partitionsnyckel värdet). `MaxDegreeOfParallelism`styr hello maximala antalet parallella aktiviteter, d.v.s. hello maximalt antal partitioner toobe besökt parallellt. 

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

Vi antar som
* D = standard maximalt antal parallella aktiviteter (= totalt antal processorn i hello klientdatorn)
* P = användaren har angett maximalt antal parallella aktiviteter
* N = antalet partitioner som behöver toobe besökt för att svara på en fråga

Följande är effekterna av hur hello parallella frågor skulle fungera för olika värden för P.
* (P == 0) = > Serial-läge
* (P == 1) = > maximalt en uppgift
* (P > 1) = > Min (P, N) parallella aktiviteter 
* (P < 1) = > Min (N, D) parallella aktiviteter

För SDK viktig information och information om implementerade klasser och metoder finns [DocumentDB-SDK](documentdb-sdk-dotnet.md)

### <a name="network-latency"></a>Svarstid för nätverk
Se [Azure Cosmos DB global distributionsplatsen](tutorial-global-distribution-documentdb.md) för hur tooset upp global distributionsplatsen och ansluta toohello närmaste region. Nätverksfördröjningen har en betydande inverkan på prestanda när du behöver toomake flera turer eller hämtar en stor resultatmängd från hello fråga. 

hello på mått för körning av frågan förklaras hur tooretrieve hello server körningstid för frågor ( `totalExecutionTimeInMs`), så att du kan skilja mellan tid i Frågekörningen och tid i nätverket överföring.

### <a name="indexing-policy"></a>Indexprincip
Se [konfigurera indexprincip](indexing-policies.md) för indexering sökvägar, typer, och lägen och hur de påverkar Frågekörningen. Hello indexering principen används som standard hash-indexering för strängar, vilket är effektiv för likhetsfrågor, men inte för intervall frågor/order by-frågor. Om du behöver intervallet frågor efter strängar, rekommenderar vi att ange hello intervallet Indextypen för alla strängar. 

## <a name="query-execution-metrics"></a>Mätvärden för körning av frågan
Du kan hämta detaljerade mått i frågan genom att passera i hello valfria `x-ms-documentdb-populatequerymetrics` huvud (`FeedOptions.PopulateQueryMetrics` i hello .NET SDK). Hej värde som returneras i `x-ms-documentdb-query-metrics` har hello följande nyckel-värdepar avsett för avancerad felsökning av Frågekörningen. 

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

| Mått | Enhet | Beskrivning | 
| ------ | -----| ----------- |
| `totalExecutionTimeInMs` | millisekunder | Körningstid för frågan | 
| `queryCompileTimeInMs` | millisekunder | Frågan kompilering  | 
| `queryLogicalPlanBuildTimeInMs` | millisekunder | Tid toobuild logiska frågeplan | 
| `queryPhysicalPlanBuildTimeInMs` | millisekunder | Tid toobuild fysiska frågeplan | 
| `queryOptimizationTimeInMs` | millisekunder | Tid som ägnats åt att optimera frågan | 
| `VMExecutionTimeInMs` | millisekunder | Tid i frågan runtime | 
| `indexLookupTimeInMs` | millisekunder | Tid i fysiska index lager | 
| `documentLoadTimeInMs` | millisekunder | Tid i inläsning av dokument  | 
| `systemFunctionExecuteTimeInMs` | millisekunder | Total tid som ägnats verkställande system (inbyggda) funktioner i millisekunder  | 
| `userFunctionExecuteTimeInMs` | millisekunder | Total tid som ägnats verkställande användardefinierade funktioner i millisekunder | 
| `retrievedDocumentCount` | millisekunder | Totalt antal hämtade dokument  | 
| `retrievedDocumentSize` | Byte | Total storlek på hämtade dokument i byte  | 
| `outputDocumentCount` | Antal | Antal dokument som utdata | 
| `writeOutputTimeInMs` | millisekunder | Tid i millisekunder för att köra frågan | 
| `indexUtilizationRatio` | förhållandet mellan (< = 1) | Förhållandet mellan antalet dokument som matchas av hello filter toohello antal dokument som läses in  | 

hello klienten SDK: er kan internt göra flera åtgärder tooserve hello fråga inom varje partition. hello klient gör flera anrop per partition om hello totalt resultat överskrider `x-ms-max-item-count`om hello frågan överskrider hello etablerat dataflöde för hello partition, eller om hello frågan nyttolast når hello maximal storlek per sida eller om hello frågan når hello system allokerade timeout-gränsen. Varje partiella Frågekörningen returnerar en `x-ms-documentdb-query-metrics` för sidan. 

Här följer några exempelfrågor och hur toointerpret vissa hello mått returnerades från frågan: 

| Fråga | Exempel mått | Beskrivning | 
| ------ | -----| ----------- |
| `SELECT TOP 100 * FROM c` | `"RetrievedDocumentCount": 101` | hello antal dokument som hämtats är 100 + 1 toomatch hello TOP-instruktion. Frågetid används främst i `WriteOutputTime` och `DocumentLoadTime` eftersom det är en genomsökning. | 
| `SELECT TOP 500 * FROM c` | `"RetrievedDocumentCount": 501` | RetrievedDocumentCount är nu högre (500 + 1 toomatch hello TOP-instruktion). | 
| `SELECT * FROM c WHERE c.N = 55` | `"IndexLookupTime": "00:00:00.0009500"` | Om 0,9 ms används i IndexLookupTime för en nyckel sökning, eftersom det är ett index sökning `/N/?`. | 
| `SELECT * FROM c WHERE c.N > 55` | `"IndexLookupTime": "00:00:00.0017700"` | Något mer tid (1,7 ms) som används IndexLookupTime via en genomsökning för intervallet, eftersom det är ett index sökning `/N/?`. | 
| `SELECT TOP 500 c.N FROM c` | `"IndexLookupTime": "00:00:00.0017700"` | Samma tid som ägnats `DocumentLoadTime` som tidigare frågor men lägre `WriteOutputTime` eftersom vi projicerar bara en egenskap. | 
| `SELECT TOP 500 udf.toPercent(c.N) FROM c` | `"UserDefinedFunctionExecutionTime": "00:00:00.2136500"` | Om 213 ms ägnats åt `UserDefinedFunctionExecutionTime` körs hello UDF på varje värde i `c.N`. |
| `SELECT TOP 500 c.Name FROM c WHERE STARTSWITH(c.Name, 'Den')` | `"IndexLookupTime": "00:00:00.0006400", "SystemFunctionExecutionTime": "00:00:00.0074100"` | Om 0,6 ms ägnats åt `IndexLookupTime` på `/Name/?`. De flesta av hello fråga körningstid (ms ~ 7) i `SystemFunctionExecutionTime`. |
| `SELECT TOP 500 c.Name FROM c WHERE STARTSWITH(LOWER(c.Name), 'den')` | `"IndexLookupTime": "00:00:00", "RetrievedDocumentCount": 2491,  "OutputDocumentCount": 500` | Frågan utförs som en genomsökning eftersom den använder `LOWER`, och 500 utanför 2491 hämtade dokument som returneras. |


## <a name="next-steps"></a>Nästa steg
* toolearn om hello stöds SQL frågeoperatorer och nyckelord, se [SQL-frågan](documentdb-sql-query.md). 
* toolearn om frågeenheter, se [programbegäran](request-units.md).
* toolearn om indexprincip, se [indexering princip](indexing-policies.md) 


