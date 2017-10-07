---
title: "aaaIndexing en Cosmos-DB-datakälla för Azure Search | Microsoft Docs"
description: "Den här artikeln beskrivs hur du toocreate Azure Search indexeraren med Cosmos DB som en datakälla."
services: search
documentationcenter: 
author: chaosrealm
manager: pablocas
editor: 
ms.assetid: 
ms.service: search
ms.devlang: rest-api
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: search
ms.date: 08/10/2017
ms.author: eugenesh
ms.openlocfilehash: 195c9bc026ee1591679dc425ef083a32a3c86be6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="connecting-cosmos-db-with-azure-search-using-indexers"></a>Ansluta Cosmos-databas med Azure Search med indexerare

Om du vill tooimplement får en bra sökning över Cosmos-DB-data, kan du använda en Azure Search indexeraren toopull data till ett Azure Search-index. I den här artikeln visar vi dig hur toointegrate Azure Cosmos DB med Azure Search utan toowrite all kod toomaintain indexerings-infrastruktur.

tooset upp en Cosmos-DB-indexeraren måste du ha en [Azure Search-tjänsten](search-create-service-portal.md), och skapa ett index, datasource och slutligen hello indexeraren. Du kan skapa dessa objekt med hjälp av hello [portal](search-import-data-portal.md), [.NET SDK](/dotnet/api/microsoft.azure.search), eller [REST API](/rest/api/searchservice/) för alla icke-.NET-språk. 

Om du väljer för hello portal hello [guiden Importera data](search-import-data-portal.md) hjälper dig att hello skapande av dessa resurser.

> [!NOTE]
> Cosmos DB är hello nästa generation av DocumentDB. Även om hello produktnamn ändras, är syntax hello samma som tidigare. Fortsätt toospecify `documentdb` enligt anvisningarna i den här artikeln indexeraren. 

> [!TIP]
> Du kan starta hello **dataimport** guiden från hello Cosmos DB instrumentpanelen toosimplify indexering för datakällan. I det vänstra navigationsfältet gå för**samlingar** > **lägga till Azure Search** tooget igång.

<a name="Concepts"></a>
## <a name="azure-search-indexer-concepts"></a>Azure Search indexeraren begrepp
Azure Search stöder hello skapande och hantering av datakällor (inklusive Cosmos DB) och indexerare som köras mot dessa datakällor.

En **datakällan** anger hello data tooindex, autentiseringsuppgifter och principer för att identifiera ändringar i hello data (till exempel ändrade eller borttagna dokument i din samling). hello datakälla har definierats som en oberoende resurs så att den kan användas av flera indexerare.

En **indexeraren** beskriver hur hello data flödar från din datakälla till en mål-sökindexet. En indexerare kan användas för att:

* Utför en enstaka kopia av hello data toopopulate ett index.
* Synkronisera ett index med ändringar i hello datakällan enligt ett schema. hello-schemat är en del av hello indexeraren definition.
* Anropa på begäran uppdateringar tooan index efter behov.

<a name="CreateDataSource"></a>
## <a name="step-1-create-a-data-source"></a>Steg 1: Skapa en datakälla
toocreate en datakälla, gör ett INLÄGG:

    POST https://[service name].search.windows.net/datasources?api-version=2016-09-01
    Content-Type: application/json
    api-key: [Search service admin key]

    {
        "name": "mydocdbdatasource",
        "type": "documentdb",
        "credentials": {
            "connectionString": "AccountEndpoint=https://myDocDbEndpoint.documents.azure.com;AccountKey=myDocDbAuthKey;Database=myDocDbDatabaseId"
        },
        "container": { "name": "myDocDbCollectionId", "query": null },
        "dataChangeDetectionPolicy": {
            "@odata.type": "#Microsoft.Azure.Search.HighWaterMarkChangeDetectionPolicy",
            "highWaterMarkColumnName": "_ts"
        }
    }

hello innehåller hello begäran hello definitionen av datakällan, som ska innehålla hello följande fält:

* **namnet**: Välj alla namn toorepresent Cosmos-DB-databasen.
* **typen**: måste vara `documentdb`.
* **autentiseringsuppgifter**:
  
  * **connectionString**: krävs. Ange hello info tooyour Azure Cosmos DB databas i hello följande format:`AccountEndpoint=<Cosmos DB endpoint url>;AccountKey=<Cosmos DB auth key>;Database=<Cosmos DB database id>`
* **behållaren**:
  
  * **namnet**: krävs. Ange hello-id för hello Cosmos DB samling toobe indexeras.
  * **frågan**: valfria. Du kan ange en fråga tooflatten ett godtyckliga JSON-dokument till en platt schemat som Azure Search kan indexera.
* **dataChangeDetectionPolicy**: rekommenderas. Se [indexering ändras dokument](#DataChangeDetectionPolicy) avsnitt.
* **dataDeletionDetectionPolicy**: valfria. Se [indexering bort dokument](#DataDeletionDetectionPolicy) avsnitt.

### <a name="using-queries-tooshape-indexed-data"></a>Med hjälp av frågor tooshape indexerade data
Du kan ange en Cosmos-DB-fråga tooflatten kapslas egenskaper eller matriser, projektet JSON-egenskaper och filtrera hello data toobe indexeras. 

Exempel dokument:

    {
        "userId": 10001,
        "contact": {
            "firstName": "andy",
            "lastName": "hoh"
        },
        "company": "microsoft",
        "tags": ["azure", "documentdb", "search"]
    }

Filtreringsfrågan:

    SELECT * FROM c WHERE c.company = "microsoft" and c._ts >= @HighWaterMark ORDER BY c._ts

Förenkla frågan:

    SELECT c.id, c.userId, c.contact.firstName, c.contact.lastName, c.company, c._ts FROM c WHERE c._ts >= @HighWaterMark ORDER BY c._ts
    
    
Projektion av frågan:

    SELECT VALUE { "id":c.id, "Name":c.contact.firstName, "Company":c.company, "_ts":c._ts } FROM c WHERE c._ts >= @HighWaterMark ORDER BY c._ts


Matrisen förenkling fråga:

    SELECT c.id, c.userId, tag, c._ts FROM c JOIN tag IN c.tags WHERE c._ts >= @HighWaterMark ORDER BY c._ts

<a name="CreateIndex"></a>
## <a name="step-2-create-an-index"></a>Steg 2: Skapa ett index
Skapa ett mål Azure Search index om du inte redan har en. Du kan skapa ett index med hello [Azure-portalen UI](search-create-index-portal.md), hello [skapa Index REST API](/rest/api/searchservice/create-index) eller [indexera klassen](/dotnet/api/microsoft.azure.search.models.index).

hello följande exempel skapar ett index med ett id och beskrivning fält:

    POST https://[service name].search.windows.net/indexes?api-version=2016-09-01
    Content-Type: application/json
    api-key: [Search service admin key]

    {
       "name": "mysearchindex",
       "fields": [{
         "name": "id",
         "type": "Edm.String",
         "key": true,
         "searchable": false
       }, {
         "name": "description",
         "type": "Edm.String",
         "filterable": false,
         "sortable": false,
         "facetable": false,
         "suggestions": true
       }]
     }

Se till att hello schemat för mål-index är kompatibel med hello schemat för hello källa JSON-dokument eller hello utdata för anpassad fråga-projektion.

> [!NOTE]
> För partitionerade samlingar hello dokumentet Standardnyckeln är Cosmos DB `_rid` -egenskap som hämtar har fått nytt namn för`rid` i Azure Search. Dessutom Cosmos DB'S `_rid` värden innehåller tecken som är ogiltiga i Azure Search-nycklar. Därför hello `_rid` värden är Base64-kodad.
> 
> 

### <a name="mapping-between-json-data-types-and-azure-search-data-types"></a>Mappning mellan JSON-datatyper och Azure Search-datatyper
| JSON-DATATYP | KOMPATIBEL INDEX FÄLTET MÅLTYPER |
| --- | --- |
| bool |Edm.Boolean Edm.String |
| Siffror som ser ut som heltal |Edm.Int32 Edm.Int64, Edm.String |
| Siffror som ser ut som flytande punkter |Edm.Double Edm.String |
| Sträng |Edm.String |
| Matriser av primitiva typer, till exempel [”a”, ”b”, ”c”] |Collection(Edm.String) |
| Strängar som ser ut som datum |Edm.DateTimeOffset Edm.String |
| GeoJSON objekt, till exempel {”typ”: ”Point”, ”coordinates”: [long, lat]} |Edm.GeographyPoint |
| Andra JSON-objekt |Saknas |

<a name="CreateIndexer"></a>
## <a name="step-3-create-an-indexer"></a>Steg 3: Skapa en indexerare

När hello index och datakälla har skapats kan är du klar toocreate hello indexeraren:

    POST https://[service name].search.windows.net/indexers?api-version=2016-09-01
    Content-Type: application/json
    api-key: [admin key]

    {
      "name" : "mydocdbindexer",
      "dataSourceName" : "mydocdbdatasource",
      "targetIndexName" : "mysearchindex",
      "schedule" : { "interval" : "PT2H" }
    }

Indexeraren körs varannan timme (schemaintervallet anges för ”PT2H”). toorun en indexerare var 30: e minut, ange hello-intervall för ”PT30M”. hello är kortaste stöds 5 minuter. hello schemat är valfritt - om detta utelämnas, indexeraren körs bara en gång när den skapas. Du kan dock köra en indexerare på begäran när som helst.   

Mer information om Hej skapa indexeraren API, kolla [skapa indexeraren](https://docs.microsoft.com/rest/api/searchservice/create-indexer).

<a id="RunIndexer"></a>
### <a name="running-indexer-on-demand"></a>Kör indexeraren på begäran
I tillägg toorunning regelbundet enligt ett schema, kan en indexerare anropas på begäran:

    POST https://[service name].search.windows.net/indexers/[indexer name]/run?api-version=2016-09-01
    api-key: [Search service admin key]

> [!NOTE]
> När du kör API returnerar har hello indexeraren anrop har schemalagts, men hello faktiska bearbetningen sker asynkront. 

Du kan övervaka hello indexeraren status i hello-portalen eller med hjälp av hello hämta indexeraren Status API, vilket beskrivs härnäst. 

<a name="GetIndexerStatus"></a>
### <a name="getting-indexer-status"></a>Hämtar status för indexerare
Du kan hämta hello status och körningen historiken för en indexerare:

    GET https://[service name].search.windows.net/indexers/[indexer name]/status?api-version=2016-09-01
    api-key: [Search service admin key]

hello svaret innehåller övergripande indexeraren status, hello sista (eller pågående) indexeraren anrop och hello historik för senaste indexeraren anrop.

    {
        "status":"running",
        "lastResult": {
            "status":"success",
            "errorMessage":null,
            "startTime":"2014-11-26T03:37:18.853Z",
            "endTime":"2014-11-26T03:37:19.012Z",
            "errors":[],
            "itemsProcessed":11,
            "itemsFailed":0,
            "initialTrackingState":null,
            "finalTrackingState":null
         },
        "executionHistory":[ {
            "status":"success",
             "errorMessage":null,
            "startTime":"2014-11-26T03:37:18.853Z",
            "endTime":"2014-11-26T03:37:19.012Z",
            "errors":[],
            "itemsProcessed":11,
            "itemsFailed":0,
            "initialTrackingState":null,
            "finalTrackingState":null
        }]
    }

Körningstiden som innehåller toohello 50 senaste slutförda körningar, vilket är sorterade i omvänd kronologisk ordning (så hello senaste körning kommer först hello svar).

<a name="DataChangeDetectionPolicy"></a>
## <a name="indexing-changed-documents"></a>Indexering ändrade dokument
hello syftet med en data ändra princip är tooefficiently identifiera ändrade dataobjekt. Hello stöds endast principen är för närvarande hello `High Water Mark` genom att använda hello `_ts` () tidsstämpelsegenskapen som tillhandahålls av Cosmos-DB som anges enligt följande:

    {
        "@odata.type" : "#Microsoft.Azure.Search.HighWaterMarkChangeDetectionPolicy",
        "highWaterMarkColumnName" : "_ts"
    }

Med den här principen rekommenderas starkt tooensure bra indexeraren prestanda. 

Om du använder en anpassad fråga kan du kontrollera att hello `_ts` egenskapen projiceras hello fråga.

<a name="IncrementalProgress"></a>
### <a name="incremental-progress-and-custom-queries"></a>Inkrementell status och anpassade frågor
Inkrementell status under indexeringen säkerställer att om indexeraren körningen har avbrutits av tillfälliga fel eller tidsgränsen för körning kan välja hello indexeraren där den avbröts nästa gång den körs i stället för att toore index hello hela samlingen från grunden. Detta är särskilt viktigt när indexering stora samlingar. 

tooenable inkrementell pågår när du använder en anpassad fråga för att se till att din fråga sorterar hello resultat av hello `_ts` kolumn. Detta gör att regelbundet kontrollera pekar som använder Azure Search tooprovide inkrementell förlopp i hello förekomst av fel.   

I vissa fall, även om frågan innehåller en `ORDER BY [collection alias]._ts` -sats Azure Search kan inte härleda hello frågan sorterade efter hello `_ts`. Du kan se Azure Search att resultaten ordnas med hjälp av hello `assumeOrderByHighWaterMarkColumn` konfigurationsegenskapen. toospecify tips, skapa eller uppdatera indexeraren enligt följande: 

    {
     ... other indexer definition properties
     "parameters" : {
            "configuration" : { "assumeOrderByHighWaterMarkColumn" : true } }
    } 

<a name="DataDeletionDetectionPolicy"></a>
## <a name="indexing-deleted-documents"></a>Indexering bort dokument
När rader tas bort från samling hello du normalt toodelete dessa rader från hello sökindex samt. hello syftet med en identifiering av princip för borttagning av data är tooefficiently identifiera borttagna dataobjekt. Hello stöds endast principen är för närvarande hello `Soft Delete` princip (borttagning är markerade med en flagga av något slag), som anges på följande sätt:

    {
        "@odata.type" : "#Microsoft.Azure.Search.SoftDeleteColumnDeletionDetectionPolicy",
        "softDeleteColumnName" : "hello property that specifies whether a document was deleted",
        "softDeleteMarkerValue" : "hello value that identifies a document as deleted"
    }

Om du använder en anpassad fråga, se till att egenskapen hello refereras av `softDeleteColumnName` projiceras hello fråga.

hello följande exempel skapar en datakälla med en princip för mjuk borttagning:

    POST https://[Search service name].search.windows.net/datasources?api-version=2016-09-01
    Content-Type: application/json
    api-key: [Search service admin key]

    {
        "name": "mydocdbdatasource",
        "type": "documentdb",
        "credentials": {
            "connectionString": "AccountEndpoint=https://myDocDbEndpoint.documents.azure.com;AccountKey=myDocDbAuthKey;Database=myDocDbDatabaseId"
        },
        "container": { "name": "myDocDbCollectionId" },
        "dataChangeDetectionPolicy": {
            "@odata.type": "#Microsoft.Azure.Search.HighWaterMarkChangeDetectionPolicy",
            "highWaterMarkColumnName": "_ts"
        },
        "dataDeletionDetectionPolicy": {
            "@odata.type": "#Microsoft.Azure.Search.SoftDeleteColumnDeletionDetectionPolicy",
            "softDeleteColumnName": "isDeleted",
            "softDeleteMarkerValue": "true"
        }
    }

## <a name="NextSteps"></a>Nästa steg
Grattis! Du har lärt dig hur toointegrate Azure Cosmos DB Azure Search med hello indexerare för Cosmos DB.

* toolearn hur mer om Azure Cosmos DB finns hello [Cosmos DB webbtjänstsida](https://azure.microsoft.com/services/documentdb/).
* toolearn hur mer om Azure Search finns hello [service söksidan](https://azure.microsoft.com/services/search/).
