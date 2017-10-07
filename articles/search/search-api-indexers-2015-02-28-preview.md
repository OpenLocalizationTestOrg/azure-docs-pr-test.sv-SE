---
title: "Indexeringsåtgärder (Azure Söktjänsts-REST API: 2015-02-28-Preview) | Microsoft Docs"
description: "Indexeringsåtgärder (Azure Söktjänsts-REST API: 2015-02-28-Preview)"
services: search
documentationcenter: 
author: chaosrealm
manager: pablocas
editor: 
ms.assetid: 42b5f85c-8304-4bc7-8e1e-a9c76b8ca25a
ms.service: search
ms.devlang: rest-api
ms.workload: search
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 05/01/2017
ms.author: eugenesh
ms.openlocfilehash: 4dd9591072b44eeabae6eac1182b4eea10fd4a22
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="indexer-operations-azure-search-service-rest-api-2015-02-28-preview"></a>Indexeringsåtgärder (Azure Söktjänsts-REST API: 2015-02-28-Preview)
> [!NOTE]
> Den här artikeln beskriver indexerare i hello [2015-02-28-Preview REST API](search-api-2015-02-28-preview.md). Den här API-versionen lägger till förhandsversioner av indexerare i Azure Blob Storage med dokumentet extrahering och Azure Table Storage indexeraren plus andra förbättringar. hello API stöder också allmänt tillgänglig (GA) indexerare, inklusive indexerare för Azure SQL Database, SQL Server på virtuella Azure-datorer och Azure Cosmos DB.
> 
> 

## <a name="overview"></a>Översikt
Azure Search kan integreras direkt med vissa gemensamma datakällor, ta bort hello måste toowrite kod tooindex dina data. tooset upp den här upp, kan du anropa hello Azure Search API toocreate och hantera **indexerare** och **datakällor**. 

En **indexeraren** är en resurs som ansluter datakällor med mål search-index. En indexerare används i hello följande sätt: 

* Utför en enstaka kopia av hello data toopopulate ett index.
* Synkronisera ett index med ändringar i hello datakällan enligt ett schema. hello-schemat är en del av hello indexeraren definition.
* Anropa på begäran tooupdate ett index efter behov. 

En **indexeraren** är användbart när du vill regelbundna uppdateringar tooan index. Du kan antingen skapa en infogad schema som en del av en indexerare definition eller kör det på begäran med hjälp av [köra indexeraren](#RunIndexer). 

En **datakällan** anger vilka data måste toobe indexerat autentiseringsuppgifter tooaccess hello data och principer tooenable Azure Search tooefficiently identifiera ändringar i hello data (exempelvis ändrade eller borttagna rader i en databastabell). Den har definierats som en oberoende resurs så att den kan användas av flera indexerare.

hello följande datakällor stöds:

* **Azure SQL Database** och **SQLServer på virtuella Azure-datorer**. Riktad genomgång, se [i den här artikeln](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers.md). 
* **Azure Cosmos DB**. Riktad genomgång, se [i den här artikeln](search-howto-index-documentdb.md). 
* **Azure Blob Storage**, inklusive hello följande dokumentera format: PDF, Microsoft Office (DOCX-dokument, XSLX/XLS, PPTX/PPT, Ignorerad), HTML-, XML, ZIP och oformaterad text filer (inklusive JSON). Riktad genomgång, se [i den här artikeln](search-howto-indexing-azure-blob-storage.md).
* **Azure Table Storage**. Riktad genomgång, se [i den här artikeln](search-howto-indexing-azure-tables.md).

Vi funderar på att lägga till stöd för ytterligare datakällor i hello framtida. toohelp oss att prioritera dessa beslut lämna din feedback på hello [Azure Search Feedbackforum](http://feedback.azure.com/forums/263029-azure-search).

Se [Tjänstbegränsningarna](search-limits-quotas-capacity.md) för gränsvärdet relaterade resurser för tooindexer och data som källa.

## <a name="typical-usage-flow"></a>Flödet för normal användning
Du kan skapa och hantera indexerare och datakällor via enkel HTTP-begäranden (POST, GET, PUT, DELETE) mot en given `data source` eller `indexer` resurs.

Konfigurera automatisk indexering är vanligtvis en process i fyra steg:

1. Identifiera hello-datakälla som innehåller hello data som behöver toobe indexeras. Tänk på att Azure Search inte stöder alla hello datatyper finns i datakällan. Se [datatyper som stöds](https://msdn.microsoft.com/library/azure/dn798938.aspx) hello lista.
2. Skapa ett Azure Search index vars schema är kompatibel med din datakälla.
3. Skapa en datakälla för Azure Search enligt beskrivningen i [Skapa datakälla](#CreateDataSource).
4. Skapa en indexerare för Azure Search enligt [skapa indexeraren](#CreateIndexer).

Du bör planera för hur du skapar en indexerare för varje mål index och datakälla kombination. Du kan ha flera indexerare skrivning till hello samma index, och du kan återanvända hello samma datakälla för flera indexerare. Men en indexerare kan bara använda en datakälla i taget och kan bara skriva tooa enda index. 

När du har skapat en indexerare, du kan hämta statusen körning med hello [Erhåll Status för indexeraren](#GetIndexerStatus) igen. Du kan också köra en indexerare när som helst (i stället för eller i tillägg toorunning den regelbundet enligt ett schema) med hjälp av hello [köra indexeraren](#RunIndexer) igen.

<!-- MSDN has 2 art files plus a API topic link list -->


## <a name="create-data-source"></a>Skapa datakälla
I Azure Search används en datakälla med indexerare som tillhandahåller hello anslutningsinformationen för ad hoc- eller schemalagd datauppdatering för ett mål-index. Du kan skapa en ny datakälla i en Azure Search-tjänst med en HTTP POST-begäran.

    POST https://[service name].search.windows.net/datasources?api-version=[api-version]
    Content-Type: application/json
    api-key: [admin key]

Du kan också använda PUT och ange hello datakällans namn på hello URI. Om hello datakällan inte finns, skapas.

    PUT https://[service name].search.windows.net/datasources/[datasource name]?api-version=[api-version]

> [!NOTE]
> hello maximala antalet datakällor som tillåts varierar beroende på prisnivå. hello ledigt tjänsten kan too3 datakällor. Standard-tjänsten tillåter 50 datakällor. Se [Tjänstbegränsningarna](search-limits-quotas-capacity.md) mer information.
> 
> 

**Förfrågan**

HTTPS krävs för alla tjänstbegäranden. Hej **Skapa datakälla** begäran kan konstrueras med en POST eller PUT metod. När du använder POST, måste du ange ett namn på datakällan i hello begärandetexten tillsammans med hello definitionen av datakällan. Med PUT är hello namn en del av hello-URL. Om hello datakällan inte finns, skapas. Om det redan finns är uppdaterade toohello ny definition. 

hello datakällans namn måste vara gemener, börja med en bokstav eller siffra, har inga snedstreck eller punkter och vara mindre än 128 tecken. När du har startat hello namn på datakälla med en bokstav eller siffra kan hello resten av hello namn innehålla en bokstav, antal och tankstreck, så länge hello streck inte är i följd. Se [namngivningsregler](https://msdn.microsoft.com/library/azure/dn857353.aspx) mer information.

Hej `api-version` krävs. hello aktuella versionen är `2015-02-28`.

**Huvuden för begäran**

hello följande lista beskriver hello nödvändiga och valfria begärandehuvuden. 

* `Content-Type`: Krävs. Ställ in för`application/json`
* `api-key`: Krävs. Hej `api-key` är används tooauthenticate hello begäran tooyour Search-tjänsten. Det är ett strängvärde, unika tooyour service. Hej **Skapa datakälla** begäran måste innehålla en `api-key` huvud ange tooyour admin-nyckel (som skillnad från tooa Frågenyckeln). 

Du måste också hello service name tooconstruct hello begärd URL. Du får båda hello tjänstnamn och `api-key` från instrumentpanelen service i hello [Azure Portal](https://portal.azure.com/). Se [skapar en söktjänst i hello portalen](search-create-service-portal.md) sidan navigering hjälp.

<a name="CreateDataSourceRequestSyntax"></a>
**Begäran brödtext Syntax**

hello hello-begäran innehåller en definition av datakällan, som innehåller typen av datakälla för hello, autentiseringsuppgifter tooread hello data samt ett valfritt data identifiering av ändring av och data tas bort identifiering principer som används tooefficiently identifiera ändras eller borttagna data i hello-datakällan när det används med en regelbundet schemalagda indexeraren. 

hello-syntax för att strukturera hello nyttolasten i begäran är som följer. Ett exempel på en begäran har angetts mer på i det här avsnittet.

    { 
        "name" : "Required for POST, optional for PUT. hello name of hello data source",
        "description" : "Optional. Anything you want, or nothing at all",
        "type" : "Required. Must be one of 'azuresql', 'documentdb', 'azureblob', or 'azuretable'",
        "credentials" : { "connectionString" : "Required. Connection string for your data source" },
        "container" : { "name" : "Required. hello name of hello table, collection, or blob container you wish tooindex" },
        "dataChangeDetectionPolicy" : { Optional. See below for details }, 
        "dataDeletionDetectionPolicy" : { Optional. See below for details }
    }

Begäran innehåller hello följande egenskaper: 

* `name`: Krävs. hello namnet på hello-datakälla. En datakälla måste bara innehålla gemena bokstäver, siffror eller bindestreck, kan inte börja eller sluta med bindestreck och begränsad too128 tecken.
* `description`: En valfri beskrivning. 
* `type`: Krävs. Måste vara något av hello stöds typer av datakällor:
  * `azuresql`-Azure SQL Database eller SQLServer på virtuella Azure-datorer
  * `documentdb`-Cosmos azure DB
  * `azureblob`-Azure Blob Storage
  * `azuretable`-Azure Table Storage
* `credentials`:
  * hello krävs `connectionString` egenskapen anger hello anslutningssträngen för datakällan hello. hello hello Anslutningssträngens format är beroende av hello typ av datakälla: 
    * Detta är hello vanliga SQL Server-anslutningssträngen för Azure SQL. Om du använder hello Azure portal tooretrieve hello anslutningssträngen använda hello `ADO.NET connection string` alternativet.
    * Azure Cosmos DB hello anslutningssträngen måste ha följande format hello: `"AccountEndpoint=https://[your account name].documents.azure.com;AccountKey=[your account key];Database=[your database id]"`. Alla hello värden krävs. Du hittar dem i hello [Azure-portalen](https://portal.azure.com/).  
    * För Azure Blob och Table Storage är hello lagringsanslutningssträng för kontot. hello format beskrivs [här](https://azure.microsoft.com/documentation/articles/storage-configure-connection-string/). HTTPS-slutpunkt-protokollet krävs.  
* `container`, krävs: Anger hello data tooindex med hello `name` och `query` egenskaper: 
  * `name`, krävs:
    * Azure SQL: Anger hello tabell eller vy. Du kan använda schemat kvalificerade namn som `[dbo].[mytable]`.
    * DocumentDB: Anger hello samling. 
    * Azure Blob Storage: Anger hello lagringsbehållaren.
    * Azure Table Storage: Anger hello namnet på hello tabell. 
  * `query`, valfritt:
    * DocumentDB: tillåter toospecify en fråga som förenklar en godtyckliga JSON-dokumentlayout till en platt schemat som Azure Search kan indexera.  
    * Azure Blob Storage: tillåter toospecify en virtuell mapp i hello blob-behållare. Till exempel för blobbsökvägen `mycontainer/documents/blob.pdf`, `documents` kan användas som hello virtuell mapp.
    * Azure Table Storage: tillåter toospecify en fråga som filter hello uppsättning rader toobe importeras.
    * Azure SQL: frågan stöds inte. Om du behöver den här funktionen, rösta på [förslaget](https://feedback.azure.com/forums/263029-azure-search/suggestions/9893490-support-user-provided-query-in-sql-indexer)
* hello valfria `dataChangeDetectionPolicy` och `dataDeletionDetectionPolicy` egenskaperna beskrivs nedan.

<a name="DataChangeDetectionPolicies"></a>
**Principer för data ändras identifiering**

hello syftet med en data ändra princip är tooefficiently identifiera ändrade dataobjekt. Principer som stöds beror på hello typ av datakälla. I nedanstående avsnitt beskrivs varje princip. 

***Ändra identifiering för Högvattenmärkesprincip*** 

Använd den här principen när datakällan innehåller en kolumn eller en egenskap som uppfyller följande kriterier hello:

* Alla infogningar ange ett värde för hello-kolumn. 
* Alla uppdateringar tooan objektet kan du också ändra hello värdet för hello kolumnen. 
* hello-värdet för den här kolumnen ökar med varje ändring.
* Frågor som använder ett filter-satsen liknande toohello följande `WHERE [High Water Mark Column] > [Current High Water Mark Value]` kan köras effektivt.

Till exempel när du använder Azure SQL-datakällor som en indexerad `rowversion` kolumnen är hello perfekt kandidat för användning med med hello vattenmärke för principen. 

Den här principen kan anges på följande sätt:

    { 
        "@odata.type" : "#Microsoft.Azure.Search.HighWaterMarkChangeDetectionPolicy",
        "highWaterMarkColumnName" : "[a row version or last_updated column name]" 
    } 

När du använder Azure Cosmos DB-datakällor, måste du använda hello `_ts` egenskap som tillhandahålls av Azure Cosmos DB. 

När du använder Azure Blob-datakällor, Azure Search automatiskt använder en övre gräns ändra princip baserat på en blob last-modified tidsstämpel; Du behöver inte toospecify en sådan policy själv.   

***SQL-integrerad ändra princip***

Om SQL-databasen stöder [ändringsspårning](https://msdn.microsoft.com/library/bb933875.aspx), bör du använda SQL integrerad ändra Spårningsprincip. Den här principen gör hello effektivaste ändringsspårning och att Azure Search tooidentify bort rader utan att behöva toohave en explicit ”mjuk borttagning” kolumn i schemat.

Integrerad ändringsspårning stöds från och med hello följande versioner av SQL Server-databasen: 

* SQL Server 2008 R2, om du använder SQL Server på Azure Virtual Machines.
* Azure SQL Database V12, om du använder Azure SQL Database.  

Ta bort rader med hjälp av SQL integrerad ändringsspårning princip inte anger en separat data princip för identifiering av borttagning - principen har inbyggt stöd för att identifiera när. 

Den här principen kan bara användas med tabeller. den kan inte användas med vyer. Du måste tooenable ändringsspårning för hello tabellen som du använder innan du kan använda den här principen. Se [aktivera och inaktivera ändringsspårning](https://msdn.microsoft.com/library/bb964713.aspx) anvisningar.    

När du strukturerar hello **Skapa datakälla** begära SQL integrerad ändringsspårning princip kan anges på följande sätt:

    { 
        "@odata.type" : "#Microsoft.Azure.Search.SqlIntegratedChangeTrackingPolicy" 
    }

<a name="DataDeletionDetectionPolicies"></a>
**Data med principer för borttagning**

hello syftet med en identifiering av princip för borttagning av data är tooefficiently identifiera borttagna dataobjekt. Hello stöds endast principen är för närvarande hello `Soft Delete` policy som låter identifierar borttagna objekt baserat på hello-värdet för en `soft delete` kolumn eller en egenskap i hello-datakälla. Den här principen kan anges på följande sätt:

    { 
        "@odata.type" : "#Microsoft.Azure.Search.SoftDeleteColumnDeletionDetectionPolicy",
        "softDeleteColumnName" : "hello column that specifies whether a row was deleted", 
        "softDeleteMarkerValue" : "hello value that identifies a row as deleted" 
    }

> [!NOTE]
> Endast kolumner med sträng, heltal eller booleska värden stöds. hello-värde som används som `softDeleteMarkerValue` måste vara en sträng, även om hello motsvarande kolumn innehåller heltal eller booleska värden. Till exempel om hello-värdet som visas i datakällan är 1 använder `"1"` som hello `softDeleteMarkerValue`.    
> 
> 

<a name="CreateDataSourceRequestExamples"></a>
**Begäran Body-exempel**

Om du planerar toouse hello-datakälla med en indexerare som körs enligt ett schema, det här exemplet illustrerar hur toospecify ändras eller tas bort med principer: 

    { 
        "name" : "asqldatasource",
        "description" : "a description",
        "type" : "azuresql",
        "credentials" : { "connectionString" : "Server=tcp:....database.windows.net,1433;Database=...;User ID=...;Password=...;Trusted_Connection=False;Encrypt=True;Connection Timeout=30;" },
        "container" : { "name" : "sometable" },
        "dataChangeDetectionPolicy" : { "@odata.type" : "#Microsoft.Azure.Search.HighWaterMarkChangeDetectionPolicy", "highWaterMarkColumnName" : "RowVersion" }, 
        "dataDeletionDetectionPolicy" : { "@odata.type" : "#Microsoft.Azure.Search.SoftDeleteColumnDeletionDetectionPolicy", "softDeleteColumnName" : "IsDeleted", "softDeleteMarkerValue" : "true" }
    }

Om du bara avser toouse hello datakälla för en kopia av hello data kan hello principer utelämnas:

    { 
        "name" : "asqldatasource",
        "description" : "anything you want, or nothing at all",
        "type" : "azuresql",
        "credentials" : { "connectionString" : "Server=tcp:....database.windows.net,1433;Database=...;User ID=...;Password=...;Trusted_Connection=False;Encrypt=True;Connection Timeout=30;" },
        "container" : { "name" : "sometable" }
    } 

**Svar**

För en lyckad begäran: 201 skapad. 

<a name="UpdateDataSource"></a>

## <a name="update-data-source"></a>Uppdatera datakällan
Du kan uppdatera en befintlig datakälla med hjälp av en HTTP PUT-begäran. Du kan ange hello namnet på hello datakälla tooupdate på hello URI-begäran:

    PUT https://[service name].search.windows.net/datasources/[datasource name]?api-version=[api-version]
    Content-Type: application/json
    api-key: [admin key]

Hej `api-version` krävs. hello aktuella versionen är `2015-02-28`. [Azure Search-API-versioner](https://msdn.microsoft.com/library/azure/dn864560.aspx) har mer information om alternativa versioner.

Hej `api-key` måste vara en administrationsnyckel (som skillnad från tooa Frågenyckeln). Se toohello authentication-avsnittet i [Söktjänsts-REST API](https://msdn.microsoft.com/library/azure/dn798935.aspx) toolearn mer om nycklar. [Skapa en söktjänst i hello portal](search-create-service-portal.md) förklarar hur tooget hello URL: en och nyckelegenskaper används i hello-begäran.

**Förfrågan**

hello begäran brödtext syntax är hello desamma som för [Skapa datakälla begär](#CreateDataSourceRequestSyntax).

Vissa egenskaper kan inte uppdateras på en befintlig datakälla. Du kan till exempel ändra hello typ av en befintlig datakälla.  

Om du inte vill toochange hello anslutningssträngen för en befintlig datakälla, kan du ange hello literal `<unchanged>` för hello anslutningssträngen. Detta är användbart i situationer där du behöver tooupdate en datakälla men har inte åtkomst toohello anslutningssträngen eftersom det är känsliga data.

**Svar**

För en lyckad begäran: 201 Skapad om en ny datakälla har skapats och 204 Nej innehåll om en befintlig datakälla har uppdaterats.

<a name="ListDataSource"></a>

## <a name="list-data-sources"></a>Lista över datakällor
Hej **lista datakällor** åtgärden returnerar en lista över hello datakällor i Azure Search-tjänsten. 

    GET https://[service name].search.windows.net/datasources?api-version=[api-version]
    api-key: [admin key]

Hej `api-version` krävs. hello aktuella versionen är `2015-02-28`. 

Hej `api-key` måste vara en administrationsnyckel (som skillnad från tooa Frågenyckeln). Se toohello authentication-avsnittet i [Söktjänsts-REST API](https://msdn.microsoft.com/library/azure/dn798935.aspx) toolearn mer om nycklar. [Skapa en söktjänst i hello portal](search-create-service-portal.md) förklarar hur tooget hello URL: en och nyckelegenskaper används i hello-begäran.

**Svar**

För en lyckad begäran: 200 OK.

Här är ett exempel svarstexten:

    {
      "value" : [
        {
          "name": "datasource1",
          "type": "azuresql",
          ... other data source properties
        }]
    }

Observera att du kan filtrera hello svar ned toojust hello egenskaper som du är intresserad av. Till exempel om du vill att endast en lista över namn på datakällor måste använda hello OData `$select` frågealternativet:

    GET /datasources?api-version=205-02-28&$select=name

I det här fallet visas hello svar från hello över exempel på följande sätt: 

    {
      "value" : [ { "name": "datasource1" }, ... ]
    }

Det här är en bra metod toosave bandbredd om du har en stor mängd datakällor i din söktjänst.

<a name="GetDataSource"></a>

## <a name="get-data-source"></a>Hämta datakälla
Hej **hämta datakällan** åtgärden hämtar hello definitionen av datakällan från Azure Search.

    GET https://[service name].search.windows.net/datasources/[datasource name]?api-version=[api-version]
    api-key: [admin key]

Hej `api-version` krävs. hello aktuella versionen är `2015-02-28`. 

Hej `api-key` måste vara en administrationsnyckel (som skillnad från tooa Frågenyckeln). Se toohello authentication-avsnittet i [Söktjänsts-REST API](https://msdn.microsoft.com/library/azure/dn798935.aspx) toolearn mer om nycklar. [Skapa en söktjänst i hello portal](search-create-service-portal.md) förklarar hur tooget hello URL: en och nyckelegenskaper används i hello-begäran.

**Svar**

Statuskod: 200 OK returneras för ett lyckat svar.

hello svaret är liknande tooexamples i [Skapa datakälla exempel begäranden](#CreateDataSourceRequestExamples): 

    { 
        "name" : "asqldatasource",
        "description" : "a description",
        "type" : "azuresql",
        "credentials" : { "connectionString" : "Server=tcp:....database.windows.net,1433;Database=...;User ID=...;Password=...;Trusted_Connection=False;Encrypt=True;Connection Timeout=30;" },
        "container" : { "name" : "sometable" },
        "dataChangeDetectionPolicy" : { 
            "@odata.type" : "#Microsoft.Azure.Search.HighWaterMarkChangeDetectionPolicy",
            "highWaterMarkColumnName" : "RowVersion" }, 
        "dataDeletionDetectionPolicy" : { 
            "@odata.type" : "#Microsoft.Azure.Search.SoftDeleteColumnDeletionDetectionPolicy",
            "softDeleteColumnName" : "IsDeleted", 
            "softDeleteMarkerValue" : "true" }
    }

> [!NOTE]
> Ange inte hello `Accept` förfrågningshuvudet för`application/json;odata.metadata=none` när anropa denna API som gör det kommer `@odata.type` attributet toobe utelämnas från hello svar och du kommer inte att kunna toodifferentiate mellan data ändras och identifiering för borttagning av data principer för olika typer. 
> 
> 

<a name="DeleteDataSource"></a>

## <a name="delete-data-source"></a>Ta bort datakällan
Hej **ta bort datakällan** åtgärden tar bort en datakälla från Azure Search-tjänsten.

    DELETE https://[service name].search.windows.net/datasources/[datasource name]?api-version=[api-version]
    api-key: [admin key]

> [!NOTE]
> Om en indexerare refererar hello-datakälla som du tar bort, fortsätter fortfarande hello borttagningsåtgärd. Men övergår dessa indexerare i feltillstånd när deras nästa körning.  
> 
> 

Hej `api-version` krävs. hello aktuella versionen är `2015-02-28`. 

Hej `api-key` måste vara en administrationsnyckel (som skillnad från tooa Frågenyckeln). Se toohello authentication-avsnittet i [Söktjänsts-REST API](https://msdn.microsoft.com/library/azure/dn798935.aspx) toolearn mer om nycklar. [Skapa en söktjänst i hello portal](search-create-service-portal.md) förklarar hur tooget hello URL: en och nyckelegenskaper används i hello-begäran.

**Svar**

Statuskod: 204 Nej innehåll returneras för ett lyckat svar.

<a name="CreateIndexer"></a>

## <a name="create-indexer"></a>Skapa indexerare
Du kan skapa en ny indexerare i en Azure Search-tjänst med en HTTP POST-begäran.

    POST https://[service name].search.windows.net/indexers?api-version=[api-version]
    Content-Type: application/json
    api-key: [admin key]

Du kan också använda PUT och ange hello datakällans namn på hello URI. Om hello datakällan inte finns, skapas.

    PUT https://[service name].search.windows.net/indexers/[indexer name]?api-version=[api-version]

> [!NOTE]
> hello maximalt antal tillåtna indexerare varierar beroende på prisnivå. hello ledigt tjänsten kan in too3 indexerare. Standard-tjänsten tillåter 50 indexerare. Se [Tjänstbegränsningarna](search-limits-quotas-capacity.md) mer information.
> 
> 

Hej `api-version` krävs. hello aktuella versionen är `2015-02-28`. 

Hej `api-key` måste vara en administrationsnyckel (som skillnad från tooa Frågenyckeln). Se toohello authentication-avsnittet i [Söktjänsts-REST API](https://msdn.microsoft.com/library/azure/dn798935.aspx) toolearn mer om nycklar. [Skapa en söktjänst i hello portal](search-create-service-portal.md) förklarar hur tooget hello URL: en och nyckelegenskaper används i hello-begäran.

<a name="CreateIndexerRequestSyntax"></a>
**Begäran brödtext Syntax**

hello innehåller hello-begäran en definition av indexerare som anger hello datakälla och hello målindexet för indexering, samt valfritt indexeringsschema och parametrar. 

hello-syntax för att strukturera hello nyttolasten i begäran är som följer. Ett exempel på en begäran har angetts mer på i det här avsnittet.

    { 
        "name" : "Required for POST, optional for PUT. hello name of hello indexer",
        "description" : "Optional. Anything you want, or null",
        "dataSourceName" : "Required. hello name of an existing data source",
        "targetIndexName" : "Required. hello name of an existing index",
        "schedule" : { Optional. See Indexing Schedule below. },
        "parameters" : { Optional. See Indexing Parameters below. },
        "fieldMappings" : { Optional. See Field Mappings below. },
        "disabled" : Optional boolean value indicating whether hello indexer is disabled. False by default.  
    }

**Indexerschemat**

En indexerare kan du ange ett schema. Om det finns ett schema körs hello indexeraren med jämna mellanrum enligt schemat. Schemat har hello följande attribut:

* `interval`: Krävs. Duration-värde som anger ett intervall eller tid för indexeraren körs. hello minsta tillåtna intervall är 5 minuter. hello längsta är en dag. Den måste formateras som ett XSD ”daytimeduration” XSD-värde (en begränsad delmängd av ett [ISO 8601 varaktighet](http://www.w3.org/TR/xmlschema11-2/#dayTimeDuration) värdet). hello mönstret för detta är: `"P[nD][T[nH][nM]]"`. Exempel: `PT15M` för varje kvart `PT2H` för varannan timme. 
* `startTime`: Krävs. En UTC datetime när hello indexeraren ska börja köra. 

**Indexerare parametrar**

En indexerare kan du ange flera parametrar som påverkar dess beteende. Alla hello parametrar är valfria.  

* `maxFailedItems`: hello antal objekt som kan misslyckas toobe indexera innan en indexerare som kör anses vara fel. Standardvärdet är 0. Information om misslyckade artiklarna returneras av hello [Erhåll Status för indexeraren](#GetIndexerStatus) igen. 
* `maxFailedItemsPerBatch`: hello antal objekt som kan misslyckas toobe indexera i varje batch innan en indexerare som kör anses vara fel. Standardvärdet är 0.
* `base64EncodeKeys`: Anger huruvida dokumentet nycklarna är Base64-kodad. Azure Search begränsar dock det antal tecken som kan ingå i en Dokumentnyckel. Hello-värden i källdata kan dock innehålla tecken som är ogiltiga. Om det är nödvändigt tooindex sådana värden som dokumentet nycklar kan den här flaggan anges tootrue. Standardvärdet är `false`.
* `batchSize`: Anger hello antal objekt som läses från hello datakälla och indexeras som en enskild batch i ordning tooimprove prestanda. hello standard beroende hello typ av datakälla: det är 1000 för Azure SQL och Azure Cosmos DB och 10 för Azure Blob Storage.

**Fältmappningar**

Du kan använda fältet mappningar toomap namnet på ett fält i hello tooa olika fältnamn på datakälla i hello mål index. Anta till exempel att en källtabell med ett fält `_id`. Azure Search kan inte namnet på ett fält som börjar med ett understreck så hello fält måste ändras. Detta kan göras med hjälp av hello `fieldMappings` egenskapen hello indexeraren på följande sätt: 

    "fieldMappings" : [ { "sourceFieldName" : "_id", "targetFieldName" : "id" } ] 

Du kan ange flera fältmappningar: 

    "fieldMappings" : [ 
        { "sourceFieldName" : "_id", "targetFieldName" : "id" },
        { "sourceFieldName" : "_timestamp", "targetFieldName" : "timestamp" },
     ]

Både käll- och fältnamn är inte skiftlägeskänsliga.

<a name="FieldMappingFunctions"></a>
***Fältet mappning funktioner***

Fältmappningar kan också vara används tootransform källa fältvärden med *mappning funktioner*.

Endast en sådan funktion stöds: `jsonArrayToStringCollection`. Den Parsar ett fält som innehåller en sträng formaterad som en JSON-matris till ett Collection(Edm.String) fält i hello mål index. Den är avsedd för användning med Azure SQL indexeraren särskilt eftersom SQL inte har en inbyggd samlingsdatatyp. Den kan användas på följande sätt: 

    "fieldMappings" : [ { "sourceFieldName" : "tags", "mappingFunction" : { "name" : "jsonArrayToStringCollection" } } ] 

Till exempel om hello fält i datakällan innehåller hello sträng `["red", "white", "blue"]`, och sedan hello målfältet av typen `Collection(Edm.String)` fylls med hello tre värden `"red"`, `"white"` och `"blue"`.

Observera att hello `targetFieldName` egenskapen är valfri; om du lämnar hello `sourceFieldName` värdet används. 

<a name="CreateIndexerRequestExamples"></a>
**Begäran Body-exempel**

hello följande exempel skapas en indexerare som kopierar data från hello tabellen som refereras av hello `ordersds` datakällan toohello `orders` index enligt ett schema som börjar på 1 januari 2015 UTC och körs varje timme. Varje anrop av indexerare kommer att lyckas om mer än 5 artiklar inte toobe indexera i varje batch och högst 10 objekt inte toobe indexerade totalt. 

    {
        "name" : "myindexer",
        "description" : "a cool indexer",
        "dataSourceName" : "ordersds",
        "targetIndexName" : "orders",
        "schedule" : { "interval" : "PT1H", "startTime" : "2015-01-01T00:00:00Z" },
        "parameters" : { "maxFailedItems" : 10, "maxFailedItemsPerBatch" : 5, "base64EncodeKeys": false }
    }

**Svar**

201 skapad för en lyckad begäran.

<a name="UpdateIndexer"></a>

## <a name="update-indexer"></a>Uppdatera indexeraren
Du kan uppdatera en befintlig indexerare med hjälp av en HTTP PUT-begäran. Du kan ange hello namnet på hello indexeraren tooupdate på hello URI-begäran:

    PUT https://[service name].search.windows.net/indexers/[indexer name]?api-version=[api-version]
    Content-Type: application/json
    api-key: [admin key]

Hej `api-version` krävs. hello aktuella versionen är `2015-02-28`. 

Hej `api-key` måste vara en administrationsnyckel (som skillnad från tooa Frågenyckeln). Se toohello authentication-avsnittet i [Söktjänsts-REST API](https://msdn.microsoft.com/library/azure/dn798935.aspx) toolearn mer om nycklar. [Skapa en söktjänst i hello portal](search-create-service-portal.md) förklarar hur tooget hello URL: en och nyckelegenskaper används i hello-begäran.

**Förfrågan**

hello begäran brödtext syntax är hello desamma som för [skapa indexeraren begär](#CreateIndexerRequestSyntax).

**Svar**

För en lyckad begäran: 201 Skapad om en ny indexeraren har skapats och 204 Nej innehåll om en befintlig indexerare har uppdaterats.

<a name="ListIndexers"></a>

## <a name="list-indexers"></a>Lista indexerare
Hej **lista indexerare** åtgärd returnerar hello lista över indexerare i Azure Search-tjänsten. 

    GET https://[service name].search.windows.net/indexers?api-version=[api-version]
    api-key: [admin key]


Hej `api-version` krävs. hello förhandsversionen är `2015-02-28-Preview`. [Azure Search versionshantering](https://msdn.microsoft.com/library/azure/dn864560.aspx) har mer information om alternativa versioner.

Hej `api-key` måste vara en administrationsnyckel (som skillnad från tooa Frågenyckeln). Se toohello authentication-avsnittet i [Söktjänsts-REST API](https://msdn.microsoft.com/library/azure/dn798935.aspx) toolearn mer om nycklar. [Skapa en söktjänst i hello portal](search-create-service-portal.md) förklarar hur tooget hello URL: en och nyckelegenskaper används i hello-begäran.

**Svar**

För en lyckad begäran: 200 OK.

Här är ett exempel svarstexten:

    {
      "value" : [
      {
        "name" : "myindexer",
        "description" : "a cool indexer",
        "dataSourceName" : "ordersds",
        "targetIndexName" : "orders",
        ... other indexer properties
      }]
    }

Observera att du kan filtrera hello svar ned toojust hello egenskaper som du är intresserad av. Till exempel om du vill att endast en lista över indexeraren namn använda hello OData `$select` frågealternativet:

    GET /indexers?api-version=2014-10-20-Preview&$select=name

I det här fallet visas hello svar från hello över exempel på följande sätt: 

    {
      "value" : [ { "name": "myindexer" } ]
    }

Detta är en bra metod toosave bandbredd om du har många av indexerare i din söktjänst.

<a name="GetIndexer"></a>

## <a name="get-indexer"></a>Hämta indexeraren
Hej **hämta indexeraren** åtgärden hämtar hello indexeraren definition från Azure Search.

    GET https://[service name].search.windows.net/indexers/[indexer name]?api-version=[api-version]
    api-key: [admin key]

Hej `api-version` krävs. hello förhandsversionen är `2015-02-28-Preview`. 

Hej `api-key` måste vara en administrationsnyckel (som skillnad från tooa Frågenyckeln). Se toohello authentication-avsnittet i [Söktjänsts-REST API](https://msdn.microsoft.com/library/azure/dn798935.aspx) toolearn mer om nycklar. [Skapa en söktjänst i hello portal](search-create-service-portal.md) förklarar hur tooget hello URL: en och nyckelegenskaper används i hello-begäran.

**Svar**

Statuskod: 200 OK returneras för ett lyckat svar.

hello svaret är liknande tooexamples i [skapa indexeraren exempel begäranden](#CreateIndexerRequestExamples): 

    {
        "name" : "myindexer",
        "description" : "a cool indexer",
        "dataSourceName" : "ordersds",
        "targetIndexName" : "orders",
        "schedule" : { "interval" : "PT1H", "startTime" : "2015-01-01T00:00:00Z" },
        "parameters" : { "maxFailedItems" : 10, "maxFailedItemsPerBatch" : 5, "base64EncodeKeys": false }
    }


<a name="DeleteIndexer"></a>

## <a name="delete-indexer"></a>Ta bort indexeraren
Hej **ta bort indexeraren** åtgärden tar bort en indexerare från din Azure Search-tjänst.

    DELETE https://[service name].search.windows.net/indexers/[indexer name]?api-version=[api-version]
    api-key: [admin key]

När du tar bort en indexerare hello indexeraren körningar pågår som för närvarande körs toocompletion, men inga ytterligare körningar kommer att schemaläggas. Försök toouse en obefintlig indexeraren leder till att HTTP-statuskod 404 hittades inte. 

Hej `api-version` krävs. hello förhandsversionen är `2015-02-28-Preview`. 

Hej `api-key` måste vara en administrationsnyckel (som skillnad från tooa Frågenyckeln). Se toohello authentication-avsnittet i [Söktjänsts-REST API](https://msdn.microsoft.com/library/azure/dn798935.aspx) toolearn mer om nycklar. [Skapa en söktjänst i hello portal](search-create-service-portal.md) förklarar hur tooget hello URL: en och nyckelegenskaper används i hello-begäran.

**Svar**

Statuskod: 204 Nej innehåll returneras för ett lyckat svar.

<a name="RunIndexer"></a>

## <a name="run-indexer"></a>Köra indexeraren
I tillägg toorunning regelbundet enligt ett schema, kan en indexerare också startas på begäran via hello **köra indexeraren** igen: 

    POST https://[service name].search.windows.net/indexers/[indexer name]/run?api-version=[api-version]
    api-key: [admin key]

Hej `api-version` krävs. hello förhandsversionen är `2015-02-28-Preview`. 

Hej `api-key` måste vara en administrationsnyckel (som skillnad från tooa Frågenyckeln). Se toohello authentication-avsnittet i [Söktjänsts-REST API](https://msdn.microsoft.com/library/azure/dn798935.aspx) toolearn mer om nycklar. [Skapa en söktjänst i hello portal](search-create-service-portal.md) förklarar hur tooget hello URL: en och nyckelegenskaper används i hello-begäran.

**Svar**

Statuskod: 202 godkända returneras för ett lyckat svar.

<a name="GetIndexerStatus"></a>

## <a name="get-indexer-status"></a>Hämta Status för indexerare
Hej **Erhåll Status för indexeraren** åtgärden hämtar hello aktuella status och körningen historiken för en indexerare: 

    GET https://[service name].search.windows.net/indexers/[indexer name]/status?api-version=[api-version]
    api-key: [admin key]


Hej `api-version` krävs. hello förhandsversionen är `2015-02-28-Preview`. 

Hej `api-key` måste vara en administrationsnyckel (som skillnad från tooa Frågenyckeln). Se toohello authentication-avsnittet i [Söktjänsts-REST API](https://msdn.microsoft.com/library/azure/dn798935.aspx) toolearn mer om nycklar. [Skapa en söktjänst i hello portal](search-create-service-portal.md) förklarar hur tooget hello URL: en och nyckelegenskaper används i hello-begäran.

**Svar**

Statuskod: 200 OK för ett lyckat svar.

hello svarstexten innehåller information om hälsostatus övergripande indexerare, hello senaste indexeraren aktivering samt hello historik över senaste indexeraren anrop (om sådan finns). 

Ett exempel svarstexten ser ut så här: 

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

**Status för indexerare**

Indexerare status kan vara något av följande värden hello:

* `running`Anger att hello indexeraren körs normalt. Observera att vissa av hello indexeraren körningar kan fortfarande misslyckas, så det är en bra idé toocheck hello `lastResult` egenskapen samt. 
* `error`Anger att hello indexeraren uppstod ett fel som inte kan korrigeras utan mänsklig inblandning. Till exempel hello autentiseringsuppgifterna för datakällan kan ha gått ut eller hello schemat för hello datakällan eller hello målindexet har ändrats i en ny sätt. 

**Indexerare resultat av utförande**

Ett resultat av utförande indexeraren innehåller information om en enda indexeraren körning. hello senaste resultatet är anslutna som hello `lastResult` egenskapen hello indexeraren status. Andra senaste, om den finns, returneras resultatet som hello `executionHistory` egenskapen hello indexeraren status. 

Resultat av utförande indexeraren innehåller hello följande egenskaper: 

* `status`: hello status för en körning. Se [Körstatus för indexeraren](#IndexerExecutionStatus) nedan för information. 
* `errorMessage`: felmeddelande för en misslyckad körning. 
* `startTime`: hello tid i UTC när denna startades.
* `endTime`: hello tid i UTC när den här körning avslutad. Det här värdet anges inte om hello-körning pågår fortfarande.
* `errors`: en matris med objektnivå fel, om sådana finns. Varje post innehåller en Dokumentnyckel (`key` egenskapen) och felmeddelandet (`errorMessage` egenskap). 
* `itemsProcessed`: hello datakällobjekt (till exempel tabellraderna) som hello indexeraren försökte tooindex under körning av det här antalet. 
* `itemsFailed`: hello antal objekt som misslyckades under den här körningen.  
* `initialTrackingState`: alltid `null` hello första indexeraren fjärrkörning av, eller om hello data ändrar spårning princip är inte aktiverat på hello-datakälla som används. Om en sådan policy är aktiverat anger det här värdet i efterföljande körningar hello första (lägsta) ändringsspårning värdet som bearbetas av denna. 
* `finalTrackingState`: alltid `null` om hello data ändrar spårning princip är inte aktiverat på hello-datakälla som används. Annars visar hello senaste (högsta) ändringsspårning värdet som har bearbetats av denna. 

<a name="IndexerExecutionStatus"></a>
**Status för indexerare**

Indexerare Körstatus avbildar hello status för en enskild indexeraren körning. Det kan ha hello följande värden:

* `success`Anger att hello indexeraren körningen har slutförts.
* `inProgress`Anger att hello indexeraren körning pågår. 
* `transientFailure`Anger att en indexerare körning misslyckades. Se `errorMessage` -egenskapen för information. hello fel kan eller inte kräva mänsklig inblandning toofix - exempelvis åtgärda ett schemainkompatibilitet mellan hello datakälla och hello målindexet kräver användaråtgärd, men inte av ett tillfälligt datakälla driftstopp. Anrop av indexerare fortsätter per schema, om en sådan finns. 
* `persistentFailure`Anger att hello indexeraren misslyckades på ett sätt som kräver mänsklig inblandning. Schemalagda indexeraren körningar stoppa. Efter det att hello problemet använder du återställa indexeraren API toorestart hello schemalagda körningar. 
* `reset`Anger att hello indexeraren har återställts av ett anrop tooReset indexeraren API (se nedan). 

<a name="ResetIndexer"></a>

## <a name="reset-indexer"></a>Återställa indexeraren
Hej **återställa indexeraren** åtgärden återställer hello-tillståndet associerat med hello indexerare för ändringsspårning. Detta ger dig omindexering (till exempel om din datakällans schema har ändrats) från grunden för tootrigger eller toochange hello data ändra princip för en datakälla som är associerade med hello indexeraren.   

    POST https://[service name].search.windows.net/indexers/[indexer name]/reset?api-version=[api-version]
    api-key: [admin key]

Hej `api-version` krävs. hello förhandsversionen är `2015-02-28-Preview`. 

Hej `api-key` måste vara en administrationsnyckel (som skillnad från tooa Frågenyckeln). Se toohello authentication-avsnittet i [Söktjänsts-REST API](https://msdn.microsoft.com/library/azure/dn798935.aspx) toolearn mer om nycklar. [Skapa en söktjänst i hello portal](search-create-service-portal.md) förklarar hur tooget hello URL: en och nyckelegenskaper används i hello-begäran.

**Svar**

Statuskod: 204 Nej innehåll för ett lyckat svar.

## <a name="mapping-between-sql-data-types-and-azure-search-data-types"></a>Mappning mellan SQL-datatyper och Azure Search-datatyper
<table style="font-size:12">
<tr>
<td>SQL-datatypen</td>    
<td>Tillåtna målindexet fälttyp</td>
<td>Anteckningar</td>
</tr>
<tr>
<td>bitar</td>
<td>Edm.Boolean Edm.String</td>
<td></td>
</tr>
<tr>
<td>int, smallint, tinyint</td>
<td>Edm.Int32 Edm.Int64, Edm.String</td>
<td></td>
</tr>
<tr>
<td>bigint</td>
<td>Edm.Int64 Edm.String</td>
<td></td>
</tr>
<tr>
<td>verkliga, float</td>
<td>Edm.Double Edm.String</td>
<td></td>
</tr>
<tr>
<td>smallmoney pengar<br>Decimal<br>numeriskt
</td>
<td>Edm.String</td>
<td>Azure Search har inte stöd för konvertering av decimaltyper till Edm.Double eftersom detta skulle förlora precision
</td>
</tr>
<tr>
<td>char, nchar, varchar, nvarchar</td>
<td>Edm.String<br/>Collection(Edm.String)</td>
<td>Se [fältet mappning funktioner](#FieldMappingFunctions) i det här dokumentet för mer information om hur tootransform en strängkolumn i en Collection(Edm.String)</td>
</tr>
<tr>
<td>smalldatetime, datetime, datetime2, date, datetimeoffset</td>
<td>Edm.DateTimeOffset Edm.String</td>
<td></td>
</tr>
<tr>
<td>uniqueidentifer</td>
<td>Edm.String</td>
<td></td>
</tr>
<tr>
<td>geografisk plats</td>
<td>Edm.GeographyPoint</td>
<td>Endast geografi instanser av typen plats med SRID 4326 (som standard hello) stöds</td>
</tr>
<tr>
<td>ROWVERSION</td>
<td>Saknas</td>
<td>Radversioner kolumner kan inte lagras i hello sökindex, men de kan användas för ändringsspårning</td>
</tr>
<tr>
<td>tid, timespan<br>Binary, varbinary, bild,<br>XML-, geometri, CLR-typer</td>
<td>Saknas</td>
<td>Stöds inte</td>
</tr>
</table>

## <a name="mapping-between-json-data-types-and-azure-search-data-types"></a>Mappning mellan JSON-datatyper och Azure Search-datatyper
<table style="font-size:12">
<tr>
<td>JSON-datatyp</td>    
<td>Tillåtna målindexet fälttyp</td>
<td>Anteckningar</td>
</tr>
<tr>
<td>bool</td>
<td>Edm.Boolean Edm.String</td>
<td></td>
</tr>
<tr>
<td>Integrerad siffror</td>
<td>Edm.Int32 Edm.Int64, Edm.String</td>
<td></td>
</tr>
<tr>
<td>Flyttal</td>
<td>Edm.Double Edm.String</td>
<td></td>
</tr>
<tr>
<td>Sträng</td>
<td>Edm.String</td>
<td></td>
</tr>
<tr>
<td>matriser av primitiva typer, t.ex. [”a”, ”b”, ”c”]</td>
<td>Collection(Edm.String)</td>
<td></td>
</tr>
<tr>
<td>Strängar som ser ut som datum</td>
<td>Edm.DateTimeOffset Edm.String</td>
<td></td>
</tr>
<tr>
<td>GeoJSON point-objekt</td>
<td>Edm.GeographyPoint</td>
<td>GeoJSON punkter är JSON-objekt i hello följande format: {”typ”: ”Point”, ”coordinates”: [long, lat]} </td>
</tr>
<tr>
<td>Andra JSON-objekt</td>
<td>Saknas</td>
<td>Stöds inte; Azure Search stöder för närvarande endast primitiva typer och strängen samlingar</td>
</tr>
</table>
