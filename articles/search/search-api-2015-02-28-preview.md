---
title: "aaaAzure Sök Service REST API-Version 2015-02-28-Preview | Microsoft Docs"
description: "Azure Search Service REST API-Version 2015-02-28-Preview innehåller experiment funktioner, till exempel naturliga språkanalys och moreLikeThis sökningar."
services: search
documentationcenter: na
author: brjohnstmsft
manager: pablocas
editor: 
ms.assetid: 3dba3bf8-9c83-42f6-82bc-04727bd11037
ms.service: search
ms.devlang: rest-api
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: search
ms.date: 05/01/2017
ms.author: brjohnst
ms.openlocfilehash: 63cb30b7f43a42552c6744ea087afea947857224
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-search-service-rest-api-version-2015-02-28-preview"></a><span data-ttu-id="9c7dd-103">Azure Söktjänsts-REST API: Version 2015-02-28-Preview</span><span class="sxs-lookup"><span data-stu-id="9c7dd-103">Azure Search Service REST API: Version 2015-02-28-Preview</span></span>
<span data-ttu-id="9c7dd-104">Den här artikeln är hello i referensdokumentationen för `api-version=2015-02-28-Preview`.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-104">This article is hello reference documentation for `api-version=2015-02-28-Preview`.</span></span> <span data-ttu-id="9c7dd-105">Den här förhandsgranskningen utökar hello aktuella allmänt tillgänglig version, [api-version = 2015-02-28](https://msdn.microsoft.com/library/dn798935.aspx), genom att tillhandahålla hello följande experiment funktioner:</span><span class="sxs-lookup"><span data-stu-id="9c7dd-105">This preview extends hello current generally available version, [api-version=2015-02-28](https://msdn.microsoft.com/library/dn798935.aspx), by providing hello following experimental features:</span></span>

* <span data-ttu-id="9c7dd-106">`moreLikeThis`Frågeparametern i hello [Sök dokument](#SearchDocs) API.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-106">`moreLikeThis` query parameter in hello [Search Documents](#SearchDocs) API.</span></span> <span data-ttu-id="9c7dd-107">Andra dokument som är relevanta tooanother specifikt dokument hittas.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-107">It finds other documents that are relevant tooanother specific document.</span></span>

<span data-ttu-id="9c7dd-108">Några ytterligare delar av hello `2015-02-28-Preview` REST API dokumenteras separat.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-108">A few additional parts of hello `2015-02-28-Preview` REST API are documented separately.</span></span> <span data-ttu-id="9c7dd-109">Exempel på dessa är:</span><span class="sxs-lookup"><span data-stu-id="9c7dd-109">These include:</span></span>

* [<span data-ttu-id="9c7dd-110">Bedömningsprofil profiler</span><span class="sxs-lookup"><span data-stu-id="9c7dd-110">Scoring Profiles</span></span>](search-api-scoring-profiles-2015-02-28-preview.md)
* [<span data-ttu-id="9c7dd-111">Indexerare</span><span class="sxs-lookup"><span data-stu-id="9c7dd-111">Indexers</span></span>](search-api-indexers-2015-02-28-preview.md)

<span data-ttu-id="9c7dd-112">Azure Search-tjänsten är tillgänglig i flera versioner.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-112">Azure Search service is available in multiple versions.</span></span> <span data-ttu-id="9c7dd-113">Se för[Sök Versionhantering](http://msdn.microsoft.com/library/azure/dn864560.aspx) mer information.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-113">Please refer too[Search Service Versioning](http://msdn.microsoft.com/library/azure/dn864560.aspx) for details.</span></span>

## <a name="apis-in-this-document"></a><span data-ttu-id="9c7dd-114">API: er i det här dokumentet</span><span class="sxs-lookup"><span data-stu-id="9c7dd-114">APIs in this document</span></span>
<span data-ttu-id="9c7dd-115">API för Azure Search-tjänsten stöder två URL-syntax för API: et: enkelt och OData (se [stöd för OData (Azure Search-API)](http://msdn.microsoft.com/library/azure/dn798932.aspx) information).</span><span class="sxs-lookup"><span data-stu-id="9c7dd-115">Azure Search service API supports two URL syntaxes for API operations: simple and OData (see [Support for OData (Azure Search API)](http://msdn.microsoft.com/library/azure/dn798932.aspx) for details).</span></span> <span data-ttu-id="9c7dd-116">hello visar följande lista hello enkel syntax.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-116">hello following list shows hello simple syntax.</span></span>

[<span data-ttu-id="9c7dd-117">Skapa Index</span><span class="sxs-lookup"><span data-stu-id="9c7dd-117">Create Index</span></span>](#CreateIndex)

    POST /indexes?api-version=2015-02-28-Preview

[<span data-ttu-id="9c7dd-118">Uppdatera Index</span><span class="sxs-lookup"><span data-stu-id="9c7dd-118">Update Index</span></span>](#UpdateIndex)

    PUT /indexes/[index name]?api-version=2015-02-28-Preview

[<span data-ttu-id="9c7dd-119">Hämta Index</span><span class="sxs-lookup"><span data-stu-id="9c7dd-119">Get Index</span></span>](#GetIndex)

    GET /indexes/[index name]?api-version=2015-02-28-Preview

[<span data-ttu-id="9c7dd-120">Visar en lista över index</span><span class="sxs-lookup"><span data-stu-id="9c7dd-120">Listing Indexes</span></span>](#ListIndexes)

    GET /indexes?api-version=2015-02-28-Preview

[<span data-ttu-id="9c7dd-121">Få indexstatistik</span><span class="sxs-lookup"><span data-stu-id="9c7dd-121">Get Index Statistics</span></span>](#GetIndexStats)

    GET /indexes/[index name]/stats?api-version=2015-02-28-Preview

[<span data-ttu-id="9c7dd-122">Testa Analyzer</span><span class="sxs-lookup"><span data-stu-id="9c7dd-122">Test Analyzer</span></span>](#TestAnalyzer)

    POST /indexes/[index name]/analyze?api-version=2015-02-28-Preview

[<span data-ttu-id="9c7dd-123">Ta bort ett Index</span><span class="sxs-lookup"><span data-stu-id="9c7dd-123">Delete an Index</span></span>](#DeleteIndex)

    DELETE /indexes/[index name]?api-version=2015-02-28-Preview

[<span data-ttu-id="9c7dd-124">Lägga till, ta bort, och uppdatera Data i ett Index</span><span class="sxs-lookup"><span data-stu-id="9c7dd-124">Add, Delete, and Update Data within an Index</span></span>](#AddOrUpdateDocuments)

    POST /indexes/[index name]/docs/index?api-version=2015-02-28-Preview

[<span data-ttu-id="9c7dd-125">Sök igenom dokument</span><span class="sxs-lookup"><span data-stu-id="9c7dd-125">Search Documents</span></span>](#SearchDocs)

    GET /indexes/[index name]/docs?[query parameters]
    POST /indexes/[index name]/docs/search?api-version=2015-02-28-Preview

[<span data-ttu-id="9c7dd-126">Sökning dokumentet</span><span class="sxs-lookup"><span data-stu-id="9c7dd-126">Lookup Document</span></span>](#LookupAPI)

     GET /indexes/[index name]/docs/[key]?[query parameters]

[<span data-ttu-id="9c7dd-127">Antal dokument</span><span class="sxs-lookup"><span data-stu-id="9c7dd-127">Count Documents</span></span>](#CountDocs)

    GET /indexes/[index name]/docs/$count?api-version=2015-02-28-Preview

[<span data-ttu-id="9c7dd-128">Förslag</span><span class="sxs-lookup"><span data-stu-id="9c7dd-128">Suggestions</span></span>](#Suggestions)

    GET /indexes/[index name]/docs/suggest?[query parameters]
    POST /indexes/[index name]/docs/suggest?api-version=2015-02-28-Preview

- - -
<a name="IndexOps"></a>

## <a name="index-operations"></a><span data-ttu-id="9c7dd-129">Indexåtgärder</span><span class="sxs-lookup"><span data-stu-id="9c7dd-129">Index Operations</span></span>
<span data-ttu-id="9c7dd-130">Du kan skapa och hantera index i Azure Search-tjänsten via enkel HTTP-begäranden (POST, GET, PUT, DELETE) mot en resurs med angivet index.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-130">You can create and manage indexes in Azure Search service via simple HTTP requests (POST, GET, PUT, DELETE) against a given index resource.</span></span> <span data-ttu-id="9c7dd-131">toocreate ett index du bokför först JSON-dokument som beskriver hello indexeringsschema.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-131">toocreate an index, you first POST a JSON document that describes hello index schema.</span></span> <span data-ttu-id="9c7dd-132">hello schema definierar hello fält hello index, deras datatyper och hur de kan användas (t.ex, i fulltextsökningar, filter, sortering eller faceting).</span><span class="sxs-lookup"><span data-stu-id="9c7dd-132">hello schema defines hello fields of hello index, their data types, and how they can be used (for example, in full-text searches, filters, sorting, or faceting).</span></span> <span data-ttu-id="9c7dd-133">Den definierar även bedömningsprofil profiler, suggesters och andra attribut tooconfigure hello beteendet för hello index.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-133">It also defines scoring profiles, suggesters and other attributes tooconfigure hello behavior of hello index.</span></span>

<span data-ttu-id="9c7dd-134">hello innehåller följande exempel en illustration av ett schema som används för att söka på hotellinformation med hello beskrivningsfält som definierats i två språk.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-134">hello following example provides an illustration of a schema used for searching on hotel information with hello Description field defined in two languages.</span></span> <span data-ttu-id="9c7dd-135">Observera hur attribut styr hur hello fältet används.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-135">Notice how attributes control how hello field is used.</span></span> <span data-ttu-id="9c7dd-136">Till exempel hello `hotelId` används som hello Dokumentnyckel (`"key": true`) och har exkluderats från fulltextsökningar (`"searchable": false`).</span><span class="sxs-lookup"><span data-stu-id="9c7dd-136">For example hello `hotelId` is used as hello document key (`"key": true`) and is excluded from full-text searches (`"searchable": false`).</span></span>

    {
    "name": "hotels",  
    "fields": [
      {"name": "hotelId", "type": "Edm.String", "key": true, "searchable": false},
      {"name": "baseRate", "type": "Edm.Double"},
      {"name": "description", "type": "Edm.String", "filterable": false, "sortable": false, "facetable": false},
      {"name": "description_fr", "type": "Edm.String", "filterable": false, "sortable": false, "facetable": false, "analyzer": "fr.lucene"},
      {"name": "hotelName", "type": "Edm.String"},
      {"name": "category", "type": "Edm.String"},
      {"name": "tags", "type": "Collection(Edm.String)"},
      {"name": "parkingIncluded", "type": "Edm.Boolean"},
      {"name": "smokingAllowed", "type": "Edm.Boolean"},
      {"name": "lastRenovationDate", "type": "Edm.DateTimeOffset"},
      {"name": "rating", "type": "Edm.Int32"},
      {"name": "location", "type": "Edm.GeographyPoint"}
     ],
     "suggesters": [
      {
       "name": "sg",
       "searchMode": "analyzingInfixMatching",
       "sourceFields": ["hotelName"]
      }
     ]
    }

<span data-ttu-id="9c7dd-137">När hello indexet har skapats måste du överföra dokument som fyller på hello index.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-137">After hello index is created, you'll upload documents that populate hello index.</span></span> <span data-ttu-id="9c7dd-138">Se [Lägg till eller uppdatera dokument](#AddOrUpdateDocuments) för nästa steg.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-138">See [Add or Update Documents](#AddOrUpdateDocuments) for this next step.</span></span>

<span data-ttu-id="9c7dd-139">En videopresentation tooindexing i Azure Search finns hello [Channel 9 molnet omfattar avsnitt på Azure Search](http://go.microsoft.com/fwlink/p/?LinkId=511509).</span><span class="sxs-lookup"><span data-stu-id="9c7dd-139">For a video introduction tooindexing in Azure Search, see hello [Channel 9 Cloud Cover episode on Azure Search](http://go.microsoft.com/fwlink/p/?LinkId=511509).</span></span>

<a name="CreateIndex"></a>

## <a name="create-index"></a><span data-ttu-id="9c7dd-140">Skapa index</span><span class="sxs-lookup"><span data-stu-id="9c7dd-140">Create Index</span></span>
<span data-ttu-id="9c7dd-141">Ett index är hello primära sätt att ordna och söka dokument i Azure Search, liknande toohow en tabell ordnar poster i en databas.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-141">An index is hello primary means of organizing and searching documents in Azure Search, similar toohow a table organizes records in a database.</span></span> <span data-ttu-id="9c7dd-142">Varje indexet innehåller en samling dokument att alla kraven toohello indexeringsschema (fältnamn, datatyper och egenskaper), men index också ange ytterligare konstruktioner (suggesters, bedömningsprofil profiler och alternativ för CORS) som definierar andra Sök beteenden.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-142">Each index has a collection of documents that all conform toohello index schema (field names, data types, and properties), but indexes also specify additional constructs (suggesters, scoring profiles, and CORS options) that define other search behaviors.</span></span>

<span data-ttu-id="9c7dd-143">Du kan skapa ett nytt index i en Azure Search-tjänst med en HTTP POST eller PUT-begäran.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-143">You can create a new index within an Azure Search service using an HTTP POST or PUT request.</span></span> <span data-ttu-id="9c7dd-144">hello brödtext hello-begäran är en JSON-schema som innehåller information om hello index och konfiguration.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-144">hello body of hello request is a JSON schema that specifies hello index and configuration information.</span></span>

    POST https://[service name].search.windows.net/indexes?api-version=[api-version]
    Content-Type: application/json
    api-key: [admin key]

<span data-ttu-id="9c7dd-145">Du kan också använda PUT och ange hello Indexnamnet på hello URI.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-145">Alternatively, you can use PUT and specify hello index name on hello URI.</span></span> <span data-ttu-id="9c7dd-146">Om hello indexet inte finns, kommer att skapas.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-146">If hello index does not exist, it will be created.</span></span>

    PUT https://[search service url]/indexes/[index name]?api-version=[api-version]

<span data-ttu-id="9c7dd-147">Skapa ett index anger hello strukturen för hello dokument lagras och används i sökningar.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-147">Creating an index determines hello structure of hello documents stored and used in search operations.</span></span> <span data-ttu-id="9c7dd-148">Fylla hello index är en separat åtgärd.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-148">Populating hello index is a separate operation.</span></span> <span data-ttu-id="9c7dd-149">Det här steget kan du använda en [indexeraren](https://msdn.microsoft.com/library/azure/mt183328.aspx) (tillgängligt för datakällor som stöds) eller en [Lägg till, uppdatera eller ta bort dokument](https://msdn.microsoft.com/library/azure/dn798930.aspx) igen.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-149">For this step, you can use an [indexer](https://msdn.microsoft.com/library/azure/mt183328.aspx) (available for supported data sources) or an [Add, Update, or Delete Documents](https://msdn.microsoft.com/library/azure/dn798930.aspx) operation.</span></span> <span data-ttu-id="9c7dd-150">hello inverterad index skapas vid hello dokument.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-150">hello inverted index is generated when hello documents are posted.</span></span>

<span data-ttu-id="9c7dd-151">**Obs**: hello maximalt antal tillåtna index varierar beroende på prisnivå.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-151">**Note**: hello maximum number of indexes allowed varies by pricing tier.</span></span> <span data-ttu-id="9c7dd-152">hello ledigt tjänsten kan in too3 index.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-152">hello free service allows up too3 indexes.</span></span> <span data-ttu-id="9c7dd-153">Standard-tjänsten tillåter 50 index per söktjänst.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-153">Standard service allows 50 indexes per Search service.</span></span> <span data-ttu-id="9c7dd-154">Se [gränser och begränsningar](http://msdn.microsoft.com/library/azure/dn798934.aspx) mer information.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-154">See [Limits and constraints](http://msdn.microsoft.com/library/azure/dn798934.aspx) for details.</span></span>

<span data-ttu-id="9c7dd-155">**Förfrågan**</span><span class="sxs-lookup"><span data-stu-id="9c7dd-155">**Request**</span></span>

<span data-ttu-id="9c7dd-156">HTTPS krävs för alla tjänstbegäranden.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-156">HTTPS is required for all service requests.</span></span> <span data-ttu-id="9c7dd-157">Hej **Create Index** begäran kan konstrueras med en POST eller PUT metod.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-157">hello **Create Index** request can be constructed using either a POST or PUT method.</span></span> <span data-ttu-id="9c7dd-158">När du använder POST, måste du ange ett indexnamn i begärandetexten för hello tillsammans med hello index schemadefinition.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-158">When using POST, you must provide an index name in hello request body along with hello index schema definition.</span></span> <span data-ttu-id="9c7dd-159">Med PUT är hello Indexnamnet en del av hello-URL.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-159">With PUT, hello index name is part of hello URL.</span></span> <span data-ttu-id="9c7dd-160">Om det inte finns hello index skapas.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-160">If hello index doesn't exist, it is created.</span></span> <span data-ttu-id="9c7dd-161">Om det redan finns är uppdaterade toohello ny definition.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-161">If it already exists, it is updated toohello new definition.</span></span>

<span data-ttu-id="9c7dd-162">hello Indexnamn måste vara gemener, börja med en bokstav eller siffra, har inga snedstreck eller punkter och vara mindre än 128 tecken.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-162">hello index name must be lower case, start with a letter or number, have no slashes or dots, and be less than 128 characters.</span></span> <span data-ttu-id="9c7dd-163">När du har startat hello Indexnamnet med en bokstav eller siffra kan hello resten av hello namn innehålla en bokstav, antal och tankstreck, så länge hello streck inte är i följd.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-163">After starting hello index name with a letter or number, hello rest of hello name can include any letter, number and dashes, as long as hello dashes are not consecutive.</span></span>

<span data-ttu-id="9c7dd-164">Hej `api-version` krävs.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-164">hello `api-version` is required.</span></span> <span data-ttu-id="9c7dd-165">Se [Sök Versionhantering](http://msdn.microsoft.com/library/azure/dn864560.aspx) för en lista över tillgängliga versioner.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-165">See [Search Service Versioning](http://msdn.microsoft.com/library/azure/dn864560.aspx) for a list of available versions.</span></span>

<span data-ttu-id="9c7dd-166">**Huvuden för begäran**</span><span class="sxs-lookup"><span data-stu-id="9c7dd-166">**Request Headers**</span></span>

<span data-ttu-id="9c7dd-167">hello följande lista beskriver hello nödvändiga och valfria begärandehuvuden.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-167">hello following list describes hello required and optional request headers.</span></span>

* <span data-ttu-id="9c7dd-168">`Content-Type`: Krävs.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-168">`Content-Type`: Required.</span></span> <span data-ttu-id="9c7dd-169">Ställ in för`application/json`</span><span class="sxs-lookup"><span data-stu-id="9c7dd-169">Set this too`application/json`</span></span>
* <span data-ttu-id="9c7dd-170">`api-key`: Krävs.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-170">`api-key`: Required.</span></span> <span data-ttu-id="9c7dd-171">Hej `api-key` används för att</span><span class="sxs-lookup"><span data-stu-id="9c7dd-171">hello `api-key` is used to</span></span>
* <span data-ttu-id="9c7dd-172">autentisera hello begäran tooyour Search-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-172">authenticate hello request tooyour Search service.</span></span> <span data-ttu-id="9c7dd-173">Det är ett strängvärde, unika tooyour service.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-173">It is a string value, unique tooyour service.</span></span> <span data-ttu-id="9c7dd-174">Hej **Create Index** begäran måste innehålla en `api-key` huvud ange tooyour admin-nyckel (som skillnad från tooa Frågenyckeln).</span><span class="sxs-lookup"><span data-stu-id="9c7dd-174">hello **Create Index** request must include an `api-key` header set tooyour admin key (as opposed tooa query key).</span></span>

<span data-ttu-id="9c7dd-175">Du måste också hello service name tooconstruct hello begärd URL.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-175">You will also need hello service name tooconstruct hello request URL.</span></span> <span data-ttu-id="9c7dd-176">Du får båda hello tjänstnamn och `api-key` från instrumentpanelen service i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-176">You can get both hello service name and `api-key` from your service dashboard in hello Azure Portal.</span></span> <span data-ttu-id="9c7dd-177">Se [skapa en Azure Search-tjänst i hello portal](search-create-service-portal.md) sidan navigering hjälp.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-177">See [Create an Azure Search service in hello portal](search-create-service-portal.md) for page navigation help.</span></span>

<span data-ttu-id="9c7dd-178"><a name="RequestData"></a>
**Begäran brödtext Syntax**</span><span class="sxs-lookup"><span data-stu-id="9c7dd-178"><a name="RequestData"></a>
**Request Body Syntax**</span></span>

<span data-ttu-id="9c7dd-179">hello hello-begäran innehåller en schemadefinition som innehåller hello lista över datafält i dokument som kan matas in i detta index, datatyper, attribut, samt en valfri lista med bedömningen profiler som är används tooscore matchar dokument på Frågetid.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-179">hello body of hello request contains a schema definition, which includes hello list of data fields within documents that will be fed into this index, data types, attributes, as well as an optional list of scoring profiles that are used tooscore matching documents at query time.</span></span>

<span data-ttu-id="9c7dd-180">Observera att för en POST-begäran måste du ange hello Indexnamnet i hello begärandetexten.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-180">Note that for a POST request, you must specify hello index name in hello request body.</span></span>

<span data-ttu-id="9c7dd-181">Det kan bara finnas ett nyckelfält i hello index.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-181">There can only be one key field in hello index.</span></span> <span data-ttu-id="9c7dd-182">Toobe har ett strängfält.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-182">It has toobe a string field.</span></span> <span data-ttu-id="9c7dd-183">Det här fältet motsvarar hello unika identifieraren för varje dokument som lagras i hello index.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-183">This field represents hello unique identifier for each document stored within hello index.</span></span>

<span data-ttu-id="9c7dd-184">hello delar av ett index inkludera hello följande:</span><span class="sxs-lookup"><span data-stu-id="9c7dd-184">hello main parts of an index include hello following:</span></span>

* `name`
* <span data-ttu-id="9c7dd-185">`fields`som kan matas in i detta index, inklusive namn, datatyp och egenskaper som definierar tillåtna åtgärder på fältet.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-185">`fields` that will be fed into this index, including name, data type, and properties that define allowable actions on that field.</span></span>
* <span data-ttu-id="9c7dd-186">`suggesters`används för automatisk komplettering eller typ-ahead-frågor.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-186">`suggesters` used for auto-complete or type-ahead queries.</span></span>
* <span data-ttu-id="9c7dd-187">`scoringProfiles`används för anpassad sökning poäng rangordning.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-187">`scoringProfiles` used for custom search score ranking.</span></span> <span data-ttu-id="9c7dd-188">Se [Lägg till bedömningsprofil profiler](https://msdn.microsoft.com/library/azure/dn798928.aspx) mer information.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-188">See [Add scoring profiles](https://msdn.microsoft.com/library/azure/dn798928.aspx) for details.</span></span>
* <span data-ttu-id="9c7dd-189">`analyzers`, `charFilters`, `tokenizers`, `tokenFilters` används toodefine hur dina dokument/frågor är uppdelade indexeras/sökbara token.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-189">`analyzers`, `charFilters`, `tokenizers`, `tokenFilters` used toodefine how your documents/queries are broken into indexable/searchable tokens.</span></span> <span data-ttu-id="9c7dd-190">Se [analys i Azure Search](https://aka.ms//azsanalysis) mer information.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-190">See [Analysis in Azure Search](https://aka.ms//azsanalysis) for details.</span></span>
* <span data-ttu-id="9c7dd-191">`defaultScoringProfile`använda toooverwrite hello standard bedömningen beteenden.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-191">`defaultScoringProfile` used toooverwrite hello default scoring behaviors.</span></span>
* <span data-ttu-id="9c7dd-192">`corsOptions`tooallow cross-origin frågor mot ditt index.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-192">`corsOptions` tooallow cross-origin queries against your index.</span></span>

<span data-ttu-id="9c7dd-193">hello-syntax för att strukturera hello nyttolasten i begäran är som följer.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-193">hello syntax for structuring hello request payload is as follows.</span></span> <span data-ttu-id="9c7dd-194">Ett exempel på en begäran har angetts mer på i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-194">A sample request is provided further on in this topic.</span></span>

    {
      "name": (optional on PUT; required on POST) "name_of_index",
      "fields": [
        {
          "name": "name_of_field",
          "type": "Edm.String | Collection(Edm.String) | Edm.Int32 | Edm.Int64 | Edm.Double | Edm.Boolean | Edm.DateTimeOffset | Edm.GeographyPoint",
          "searchable": true (default where applicable) | false (only Edm.String and Collection(Edm.String) fields can be searchable),
          "filterable": true (default) | false,
          "sortable": true (default where applicable) | false (Collection(Edm.String) fields cannot be sortable),
          "facetable": true (default where applicable) | false (Edm.GeographyPoint fields cannot be facetable),
          "key": true | false (default, only Edm.String fields can be keys),
          "retrievable": true (default) | false,              
          "analyzer": "name of hello analyzer used for search and indexing", (only if 'searchAnalyzer' and 'indexAnalyzer' are not set)
          "searchAnalyzer": "name of hello search analyzer", (only if 'indexAnalyzer' is set and 'analyzer' is not set)
          "indexAnalyzer": "name of hello indexing analyzer" (only if 'searchAnalyzer' is set and 'analyzer' is not set)
        }
      ],
      "suggesters": [
        {
          "name": "name of suggester",
          "searchMode": "analyzingInfixMatching" (other modes may be added in hello future),
          "sourceFields": ["field1", "field2", ...]
        }
      ],
      "scoringProfiles": [
        {
          "name": "name of scoring profile",
          "text": (optional, only applies toosearchable fields) {
            "weights": {
              "searchable_field_name": relative_weight_value (positive numbers),
              ...
            }
          },
          "functions": (optional) [
            {
              "type": "magnitude | freshness | distance | tag",
              "boost": # (positive number used as multiplier for raw score != 1),
              "fieldName": "...",
              "interpolation": "constant | linear (default) | quadratic | logarithmic",
              "magnitude": {
                "boostingRangeStart": #,
                "boostingRangeEnd": #,
                "constantBoostBeyondRange": true | false (default)
              },
              "freshness": {
                "boostingDuration": "..." (value representing timespan leading toonow over which boosting occurs)
              },
              "distance": {
                "referencePointParameter": "...", (parameter toobe passed in queries toouse as reference location, see "scoringParameter" for syntax details)
                "boostingDistance": # (hello distance in kilometers from hello reference location where hello boosting range ends)
              },
              "tag": {
                "tagsParameter": "..." (parameter toobe passed in queries toospecify list of tags toocompare against target field, see "scoringParameter" for syntax details)
              }
            }
          ],
          "functionAggregation": (optional, applies only when functions are specified)
            "sum (default) | average | minimum | maximum | firstMatching"
        }
      ],
      "analyzers":(optional)[ ... ],
      "charFilters":(optional)[ ... ],
      "tokenizers":(optional)[ ... ],
      "tokenFilters":(optional)[ ... ],
      "defaultScoringProfile": (optional) "...",
      "corsOptions": (optional) {
        "allowedOrigins": ["*"] | ["origin_1", "origin_2", ...],
        "maxAgeInSeconds": (optional) max_age_in_seconds (non-negative integer)
      }
    }

<span data-ttu-id="9c7dd-195">**Indexattribut**</span><span class="sxs-lookup"><span data-stu-id="9c7dd-195">**Index Attributes**</span></span>

<span data-ttu-id="9c7dd-196">hello kan följande attribut anges när ett index skapas.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-196">hello following attributes can be set when creating an index.</span></span> <span data-ttu-id="9c7dd-197">Mer information om batchbedömning och bedömningen profiler finns [lägga till bedömningen profiler](https://msdn.microsoft.com/library/azure/dn798928.aspx).</span><span class="sxs-lookup"><span data-stu-id="9c7dd-197">For details about scoring and scoring profiles, see [Add scoring Profiles](https://msdn.microsoft.com/library/azure/dn798928.aspx).</span></span>

<span data-ttu-id="9c7dd-198">`name`-Aktiverar hello namnet på hello fält.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-198">`name` - Sets hello name of hello field.</span></span>

<span data-ttu-id="9c7dd-199">`type`-Aktiverar hello datatyp för hello fält.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-199">`type` - Sets hello data type for hello field.</span></span>

<span data-ttu-id="9c7dd-200">`searchable`-Markerar hello fält som fulltext-sökning kan.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-200">`searchable` - Marks hello field as full-text search-able.</span></span> <span data-ttu-id="9c7dd-201">Det innebär att den kommer att göras analys, till exempel radbrytning under indexeringen.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-201">This means it will undergo analysis such as word-breaking during indexing.</span></span> <span data-ttu-id="9c7dd-202">Om du ställer in en `searchable` tooa fältvärdet som ”skiner”, internt blir dela till hello enskilda token ”Soligt” och ”dag”.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-202">If you set a `searchable` field tooa value like "sunny day", internally it will be split into hello individual tokens "sunny" and "day".</span></span> <span data-ttu-id="9c7dd-203">Detta gör att fulltextsökningar för dessa villkor.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-203">This enables full-text searches for these terms.</span></span> <span data-ttu-id="9c7dd-204">Fält av typen `Edm.String` eller `Collection(Edm.String)` är `searchable` som standard.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-204">Fields of type `Edm.String` or `Collection(Edm.String)` are `searchable` by default.</span></span> <span data-ttu-id="9c7dd-205">Fält av andra typer kan inte vara `searchable`.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-205">Fields of other types cannot be `searchable`.</span></span>

* <span data-ttu-id="9c7dd-206">**Obs**: `searchable` fält använda extra utrymme i ditt index eftersom en ytterligare principfilerna version av hello fältvärdet fulltextsökningar lagras i Azure Search.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-206">**Note**: `searchable` fields consume extra space in your index since Azure Search will store an additional tokenized version of hello field value for full-text searches.</span></span> <span data-ttu-id="9c7dd-207">Om du vill toosave utrymme i ditt index och du behöver inte en fältet toobe som ingår i sökningar genom att ange `searchable` för`false`.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-207">If you want toosave space in your index and you don't need a field toobe included in searches, set `searchable` too`false`.</span></span>

<span data-ttu-id="9c7dd-208">`filterable`-Tillåter hello fältet toobe som refereras i `$filter` frågor.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-208">`filterable` - Allows hello field toobe referenced in `$filter` queries.</span></span> <span data-ttu-id="9c7dd-209">`filterable`skiljer sig från `searchable` i hur strängar ska hanteras.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-209">`filterable` differs from `searchable` in how strings are handled.</span></span> <span data-ttu-id="9c7dd-210">Fält av typen `Edm.String` eller `Collection(Edm.String)` som är `filterable` inte genomgår radbrytning, så att jämförelser för endast exakt matchar.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-210">Fields of type `Edm.String` or `Collection(Edm.String)` that are `filterable` do not undergo word-breaking, so comparisons are for exact matches only.</span></span> <span data-ttu-id="9c7dd-211">Till exempel om du ställer in sådant fält `f` för ”skiner” `$filter=f eq 'sunny'` hittar inga matchningar men `$filter=f eq 'sunny day'` kommer.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-211">For example, if you set such a field `f` too"sunny day", `$filter=f eq 'sunny'` will find no matches, but `$filter=f eq 'sunny day'` will.</span></span> <span data-ttu-id="9c7dd-212">Alla fält är `filterable` som standard.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-212">All fields are `filterable` by default.</span></span>

<span data-ttu-id="9c7dd-213">`sortable`-Som standard sorterar hello system resultat poäng, men i många upplevelser användare vill toosort av fälten i hello dokument.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-213">`sortable` - By default hello system sorts results by score, but in many experiences users will want toosort by fields in hello documents.</span></span> <span data-ttu-id="9c7dd-214">Fält av typen `Collection(Edm.String)` får inte vara `sortable`.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-214">Fields of type `Collection(Edm.String)` cannot be `sortable`.</span></span> <span data-ttu-id="9c7dd-215">Alla andra fält är `sortable` som standard.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-215">All other fields are `sortable` by default.</span></span>

<span data-ttu-id="9c7dd-216">`facetable`-Vanligtvis används i en presentation av sökresultat som innehåller antalet träffar per kategori (till exempel, Sök efter digitalkameror och se träffar efter varumärke, megapixel, av pris, etc.).</span><span class="sxs-lookup"><span data-stu-id="9c7dd-216">`facetable`- Typically used in a presentation of search results that includes hit count by category (for example, search for digital cameras and see hits by brand, by megapixels, by price, etc.).</span></span> <span data-ttu-id="9c7dd-217">Det här alternativet kan inte användas med fält av typen `Edm.GeographyPoint`.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-217">This option cannot be used with fields of type `Edm.GeographyPoint`.</span></span> <span data-ttu-id="9c7dd-218">Alla andra fält är `facetable` som standard.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-218">All other fields are `facetable` by default.</span></span>

* <span data-ttu-id="9c7dd-219">**Obs**: fält av typen `Edm.String` som är `filterable`, `sortable`, eller `facetable` kan vara högst 32 KB längd.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-219">**Note**: Fields of type `Edm.String` that are `filterable`, `sortable`, or `facetable` can be at most 32KB in length.</span></span> <span data-ttu-id="9c7dd-220">Detta beror på att dessa fält behandlas som en enda sökterm och hello maxlängd för en term i Azure Search är 32KB.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-220">This is because such fields are treated as a single search term, and hello maximum length of a term in Azure Search is 32KB.</span></span> <span data-ttu-id="9c7dd-221">Om du behöver toostore mer text än detta i ett enda strängfält måste tooexplicitly ange `filterable`, `sortable`, och `facetable` för`false` i Indexdefinition.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-221">If you need toostore more text than this in a single string field, you will need tooexplicitly set `filterable`, `sortable`, and `facetable` too`false` in your index definition.</span></span>
* <span data-ttu-id="9c7dd-222">**Obs**: om ett fält har inget av ovanstående hello attributen för angivna`true` (`searchable`, `filterable`, `sortable`, eller`facetable`) hello fältet effektivt är exkluderad från hello inverterad index.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-222">**Note**: If a field has none of hello above attributes set too`true` (`searchable`, `filterable`, `sortable`,  or`facetable`) hello field is effectively excluded from hello inverted index.</span></span> <span data-ttu-id="9c7dd-223">Det här alternativet är användbart för fält som inte används i frågor, men som behövs i sökresultaten.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-223">This option is useful for fields that are not used in queries, but are needed in search results.</span></span> <span data-ttu-id="9c7dd-224">Förutom dessa fält från hello indexet förbättrar prestanda.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-224">Excluding such fields from hello index improves performance.</span></span>

<span data-ttu-id="9c7dd-225">`key`-Märken hello fältet innehåller unika identifierare för dokument inom hello index.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-225">`key` - Marks hello field as containing unique identifiers for documents within hello index.</span></span> <span data-ttu-id="9c7dd-226">Exakt ett fält måste väljas som hello `key` fältet och det måste vara av typen `Edm.String`.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-226">Exactly one field must be chosen as hello `key` field and it must be of type `Edm.String`.</span></span> <span data-ttu-id="9c7dd-227">Nyckelfält kan vara används toolook dokument direkt via hello [sökning API](#LookupAPI).</span><span class="sxs-lookup"><span data-stu-id="9c7dd-227">Key fields can be used toolook up documents directly via hello [Lookup API](#LookupAPI).</span></span>

<span data-ttu-id="9c7dd-228">`retrievable`-Anger om fältet hello kan returneras i sökresultatet.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-228">`retrievable` - Sets whether hello field can be returned in a search result.</span></span>  <span data-ttu-id="9c7dd-229">Detta är användbart när du vill toouse ett fält (till exempel marginal) som ett filter som sortering eller bedömningen mekanism men inte vill att hello fältet toobe synliga toohello slutanvändaren.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-229">This is useful when you want toouse a field (for example, margin) as a filter, sorting, or scoring mechanism but do not want hello field toobe visible toohello end user.</span></span> <span data-ttu-id="9c7dd-230">Det här attributet måste vara `true` för `key` fält.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-230">This attribute must be `true` for `key` fields.</span></span>

<span data-ttu-id="9c7dd-231">`analyzer`-Aktiverar hello namnet på hello analyzer toouse för hello fält vid sökning och indexering tid.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-231">`analyzer` - Sets hello name of hello analyzer toouse for hello field at search time and indexing time.</span></span> <span data-ttu-id="9c7dd-232">Hello tillåtna värden finns [Analyzers](https://msdn.microsoft.com/library/mt605304.aspx).</span><span class="sxs-lookup"><span data-stu-id="9c7dd-232">For hello allowed set of values see [Analyzers](https://msdn.microsoft.com/library/mt605304.aspx).</span></span> <span data-ttu-id="9c7dd-233">Det här alternativet kan användas med `searchable` fält och den kan inte anges tillsammans med antingen `searchAnalyzer` eller `indexAnalyzer`.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-233">This option can be used only with `searchable` fields and it can't be set together with either `searchAnalyzer` or `indexAnalyzer`.</span></span>  <span data-ttu-id="9c7dd-234">När hello analyzer är valt, kan inte ändras för hello-fältet.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-234">Once hello analyzer is chosen, it cannot be changed for hello field.</span></span>

<span data-ttu-id="9c7dd-235">`searchAnalyzer`-Aktiverar hello hello analyzer används för närvarande sökning för hello fältet namn.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-235">`searchAnalyzer` - Sets hello name of hello analyzer used at search time for hello field.</span></span> <span data-ttu-id="9c7dd-236">Hello tillåtna värden finns [Analyzers](https://msdn.microsoft.com/library/mt605304.aspx).</span><span class="sxs-lookup"><span data-stu-id="9c7dd-236">For hello allowed set of values see [Analyzers](https://msdn.microsoft.com/library/mt605304.aspx).</span></span> <span data-ttu-id="9c7dd-237">Det här alternativet kan användas med `searchable` fält.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-237">This option can be used only with `searchable` fields.</span></span> <span data-ttu-id="9c7dd-238">Det måste anges tillsammans med `indexAnalyzer` och kan inte anges tillsammans med hello `analyzer` alternativet.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-238">It must be set together with `indexAnalyzer` and it cannot be set together with hello `analyzer` option.</span></span> <span data-ttu-id="9c7dd-239">Den här analyzer kan uppdateras på ett befintligt fält.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-239">This analyzer can be updated on an existing field.</span></span>

<span data-ttu-id="9c7dd-240">`indexAnalyzer`-Aktiverar hello hello analyzer används för närvarande indexering för hello fältet namn.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-240">`indexAnalyzer` - Sets hello name of hello analyzer used at indexing time for hello field.</span></span> <span data-ttu-id="9c7dd-241">Hello tillåtna värden finns [Analyzers](https://msdn.microsoft.com/library/mt605304.aspx).</span><span class="sxs-lookup"><span data-stu-id="9c7dd-241">For hello allowed set of values see [Analyzers](https://msdn.microsoft.com/library/mt605304.aspx).</span></span> <span data-ttu-id="9c7dd-242">Det här alternativet kan användas med `searchable` fält.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-242">This option can be used only with `searchable` fields.</span></span> <span data-ttu-id="9c7dd-243">Det måste anges tillsammans med `searchAnalyzer` och kan inte anges tillsammans med hello `analyzer` alternativet.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-243">It must be set together with `searchAnalyzer` and it cannot be set together with hello `analyzer` option.</span></span> <span data-ttu-id="9c7dd-244">När hello analyzer är valt, kan inte ändras för hello-fältet.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-244">Once hello analyzer is chosen, it cannot be changed for hello field.</span></span>

<span data-ttu-id="9c7dd-245">`suggesters`-Aktiverar hello sökläget och fält som är hello informationskälla hello förslag.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-245">`suggesters` - Sets hello search mode and fields that are hello source of hello content for suggestions.</span></span> <span data-ttu-id="9c7dd-246">Se [Suggesters](#Suggesters) mer information.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-246">See [Suggesters](#Suggesters) for details.</span></span>

<span data-ttu-id="9c7dd-247">`scoringProfiles`-Definierar anpassade bedömningsprofil beteenden som gör att du kan påverka vilka objekt som visas överst i sökresultaten.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-247">`scoringProfiles` - Defines custom scoring behaviors that let you influence which items appear higher in search results.</span></span> <span data-ttu-id="9c7dd-248">Bedömningsprofil profiler består av fältvikter och funktioner.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-248">Scoring profiles are made up of field weights and functions.</span></span> <span data-ttu-id="9c7dd-249">Se [lägga till bedömningen profiler](https://msdn.microsoft.com/library/azure/dn798928.aspx) för mer information om hello-attribut som används i en bedömningsprofilen.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-249">See [Add scoring Profiles](https://msdn.microsoft.com/library/azure/dn798928.aspx) for more information about hello attributes used in a scoring profile.</span></span>

<span data-ttu-id="9c7dd-250"><!-- This is a standalone topic in MSDN -->
<a name="LanguageSupport"></a>
**Språkstöd**</span><span class="sxs-lookup"><span data-stu-id="9c7dd-250"><!-- This is a standalone topic in MSDN -->
<a name="LanguageSupport"></a>
**Language support**</span></span>

<span data-ttu-id="9c7dd-251">Sökbara fält genomgå analys som omfattar ofta avstavning text normalisering och filtrera bort villkor.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-251">Searchable fields undergo analysis that most frequently involves word-breaking, text normalization, and filtering out terms.</span></span> <span data-ttu-id="9c7dd-252">Som standard sökbara fält i Azure Search analyseras med hello [Apache Lucene Standard analyzer](http://lucene.apache.org/core/4_9_0/analyzers-common/index.html) som delar upp text i element som följer den[”Unicode-Text segmentering”](http://unicode.org/reports/tr29/) regler.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-252">By default, searchable fields in Azure Search are analyzed with hello [Apache Lucene Standard analyzer](http://lucene.apache.org/core/4_9_0/analyzers-common/index.html) which breaks text into elements following the["Unicode Text Segmentation"](http://unicode.org/reports/tr29/) rules.</span></span> <span data-ttu-id="9c7dd-253">Dessutom konverterar hello standard analyzer alla tecken tootheir gemen formuläret.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-253">Additionally, hello standard analyzer converts all characters tootheir lower case form.</span></span> <span data-ttu-id="9c7dd-254">Både indexerade dokument och sökvillkoren kan du gå igenom hello analys under indexering och frågebearbetning.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-254">Both indexed documents and search terms go through hello analysis during indexing and query processing.</span></span>

<span data-ttu-id="9c7dd-255">Azure Search har stöd för flera olika språk.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-255">Azure Search supports a variety of languages.</span></span> <span data-ttu-id="9c7dd-256">Varje språk kräver en icke-standard text analyzer som-konton för egenskaperna för ett visst språk.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-256">Each language requires a non-standard text analyzer which accounts for characteristics of a given language.</span></span> <span data-ttu-id="9c7dd-257">Azure Search finns två typer av analyzers:</span><span class="sxs-lookup"><span data-stu-id="9c7dd-257">Azure Search offers two types of analyzers:</span></span>

* <span data-ttu-id="9c7dd-258">35 analyzers backas upp av Lucene.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-258">35 analyzers backed by Lucene.</span></span>
* <span data-ttu-id="9c7dd-259">50 analyzers backas upp av egna Microsoft naturligt språk bearbetning teknik som används i Office och Bing.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-259">50 analyzers backed by proprietary Microsoft natural language processing technology used in Office and Bing.</span></span>

<span data-ttu-id="9c7dd-260">Vissa utvecklare kanske föredrar hello mer bekanta, enkla, öppen källkod lösning av Lucene.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-260">Some developers might prefer hello more familiar, simple, open-source solution of Lucene.</span></span> <span data-ttu-id="9c7dd-261">Lucene analyzers är snabbare, men hello Microsoft analyzers har avancerade funktioner, t.ex lemmatisering, word decompounding (i språk som tyska, danska, nederländska, svenska, norska, estniska, Slutför, ungerska, slovakiska) och entitet recognition (URL: er e-postmeddelanden, datum, nummer).</span><span class="sxs-lookup"><span data-stu-id="9c7dd-261">Lucene analyzers are faster, but hello Microsoft analyzers have advanced capabilities, such as lemmatization, word decompounding (in languages like German, Danish, Dutch, Swedish, Norwegian, Estonian, Finish, Hungarian, Slovak) and entity recognition (URLs, emails, dates, numbers).</span></span> <span data-ttu-id="9c7dd-262">Om möjligt bör du köra jämförelser av båda hello Microsoft och Lucene analyzers toodecide vilket som är en bättre anpassning.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-262">If possible, you should run comparisons of both hello Microsoft and Lucene analyzers toodecide which one is a better fit.</span></span>

<span data-ttu-id="9c7dd-263">***Jämförelse***</span><span class="sxs-lookup"><span data-stu-id="9c7dd-263">***How they compare***</span></span>

<span data-ttu-id="9c7dd-264">Hej Lucene analyzer för engelska utökar hello standard analyzer.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-264">hello Lucene analyzer for English extends hello standard analyzer.</span></span> <span data-ttu-id="9c7dd-265">Det tar bort genitiv (avslutande) från ord, gäller härrör enligt [Porter härrör algoritmen](http://tartarus.org/~martin/PorterStemmer/), och tar bort engelska [stoppord](http://en.wikipedia.org/wiki/Stop_words).</span><span class="sxs-lookup"><span data-stu-id="9c7dd-265">It removes possessives (trailing 's) from words, applies stemming as per [Porter Stemming algorithm](http://tartarus.org/~martin/PorterStemmer/), and removes English [stop words](http://en.wikipedia.org/wiki/Stop_words).</span></span>

<span data-ttu-id="9c7dd-266">Jämförelse utför hello Microsoft analyzer lemmatisering i stället för härrör.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-266">In comparison, hello Microsoft analyzer performs lemmatization instead of stemming.</span></span> <span data-ttu-id="9c7dd-267">Det innebär att den kan hantera böjda och oregelbundna word formulär förbättras vad resulterar i mer relevant sökresultat (titta på modul 7 av [Azure Search MVA presentation](http://www.microsoftvirtualacademy.com/training-courses/adding-microsoft-azure-search-to-your-websites-and-apps) för mer information).</span><span class="sxs-lookup"><span data-stu-id="9c7dd-267">It means it can handle inflected and irregular word forms much better what results in more relevant search results (watch module 7 of [Azure Search MVA presentation](http://www.microsoftvirtualacademy.com/training-courses/adding-microsoft-azure-search-to-your-websites-and-apps) for more details).</span></span>

<span data-ttu-id="9c7dd-268">Indexering med Microsoft analyzers är ungefär två toothree gånger långsammare än motsvarigheterna Lucene beroende på hello språk.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-268">Indexing with Microsoft analyzers is on average two toothree times slower than their Lucene equivalents, depending on hello language.</span></span> <span data-ttu-id="9c7dd-269">Sökningsprestanda avsevärt påverkas inte för frågor genomsnittlig storlek.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-269">Search performance should not be significantly affected for average size queries.</span></span>

<span data-ttu-id="9c7dd-270">***Konfiguration***</span><span class="sxs-lookup"><span data-stu-id="9c7dd-270">***Configuration***</span></span>

<span data-ttu-id="9c7dd-271">Du kan ange hello för varje fält i hello indexdefinitionen `analyzer` tooan analyzer egenskapsnamn som anger vilka språk och leverantör.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-271">For each field in hello index definition, you can set hello `analyzer` property tooan analyzer name that specifies which language and vendor.</span></span> <span data-ttu-id="9c7dd-272">hello samma analyzer tillämpas när indexering och sökning för fältet.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-272">hello same analyzer will be applied when indexing and searching for that field.</span></span>
<span data-ttu-id="9c7dd-273">Du kan till exempel ha separata fält för engelska, franska och spanska hotell beskrivningar som finns sida vid sida i hello samma index.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-273">For example, you can have separate fields for English, French, and Spanish hotel descriptions that exist side-by-side in hello same index.</span></span> <span data-ttu-id="9c7dd-274">Använd hello ['searchFields' Frågeparametern](#SearchQueryParameters) toospecify vilka språkspecifika fältet toosearch mot i dina frågor.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-274">Use hello ['searchFields' query parameter](#SearchQueryParameters) toospecify which language-specific field toosearch against in your queries.</span></span> <span data-ttu-id="9c7dd-275">Du kan granska frågan exempel som omfattar hello `analyzer` egenskap i [Sök dokument](#SearchDocs).</span><span class="sxs-lookup"><span data-stu-id="9c7dd-275">You can review query examples that include hello `analyzer` property in [Search Documents](#SearchDocs).</span></span> 

<span data-ttu-id="9c7dd-276">***Analyzer lista***</span><span class="sxs-lookup"><span data-stu-id="9c7dd-276">***Analyzer list***</span></span>

<span data-ttu-id="9c7dd-277">Nedan visas hello lista över språk som stöds tillsammans med Lucene och Microsoft analyzer namn.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-277">Below is hello list of supported languages together with Lucene and Microsoft analyzer names.</span></span>

<table style="font-size:12">
    <tr>
        <th><span data-ttu-id="9c7dd-278">Språk</span><span class="sxs-lookup"><span data-stu-id="9c7dd-278">Language</span></span></th>
        <th><span data-ttu-id="9c7dd-279">Microsoft analyzer namn</span><span class="sxs-lookup"><span data-stu-id="9c7dd-279">Microsoft analyzer name</span></span></th>
        <th><span data-ttu-id="9c7dd-280">Lucene analyzer namn</span><span class="sxs-lookup"><span data-stu-id="9c7dd-280">Lucene analyzer name</span></span></th>
    </tr>
    <tr>
        <td><span data-ttu-id="9c7dd-281">Arabiska</span><span class="sxs-lookup"><span data-stu-id="9c7dd-281">Arabic</span></span></td>
        <td><span data-ttu-id="9c7dd-282">ar.Microsoft</span><span class="sxs-lookup"><span data-stu-id="9c7dd-282">ar.microsoft</span></span></td>
        <td><span data-ttu-id="9c7dd-283">ar.lucene</span><span class="sxs-lookup"><span data-stu-id="9c7dd-283">ar.lucene</span></span></td>        
    </tr>
    <tr>
        <td><span data-ttu-id="9c7dd-284">Armeniska</span><span class="sxs-lookup"><span data-stu-id="9c7dd-284">Armenian</span></span></td>
        <td></td>
        <td><span data-ttu-id="9c7dd-285">hy.lucene</span><span class="sxs-lookup"><span data-stu-id="9c7dd-285">hy.lucene</span></span></td>
      </tr>
    <tr>
        <td><span data-ttu-id="9c7dd-286">Bangla</span><span class="sxs-lookup"><span data-stu-id="9c7dd-286">Bangla</span></span></td>
        <td><span data-ttu-id="9c7dd-287">bn.Microsoft</span><span class="sxs-lookup"><span data-stu-id="9c7dd-287">bn.microsoft</span></span></td>
        <td></td>
    </tr>
      <tr>
        <td><span data-ttu-id="9c7dd-288">Baskiska</span><span class="sxs-lookup"><span data-stu-id="9c7dd-288">Basque</span></span></td>
        <td></td>
        <td><span data-ttu-id="9c7dd-289">EU.lucene</span><span class="sxs-lookup"><span data-stu-id="9c7dd-289">eu.lucene</span></span></td>
    </tr>
      <tr>
         <td><span data-ttu-id="9c7dd-290">Bulgariska</span><span class="sxs-lookup"><span data-stu-id="9c7dd-290">Bulgarian</span></span></td>
        <td><span data-ttu-id="9c7dd-291">BG.Microsoft</span><span class="sxs-lookup"><span data-stu-id="9c7dd-291">bg.microsoft</span></span></td>
        <td><span data-ttu-id="9c7dd-292">BG.lucene</span><span class="sxs-lookup"><span data-stu-id="9c7dd-292">bg.lucene</span></span></td>
      </tr>
      <tr>
        <td><span data-ttu-id="9c7dd-293">Katalanska</span><span class="sxs-lookup"><span data-stu-id="9c7dd-293">Catalan</span></span></td>
        <td><span data-ttu-id="9c7dd-294">CA.Microsoft</span><span class="sxs-lookup"><span data-stu-id="9c7dd-294">ca.microsoft</span></span></td>
        <td><span data-ttu-id="9c7dd-295">CA.lucene</span><span class="sxs-lookup"><span data-stu-id="9c7dd-295">ca.lucene</span></span></td>          
      </tr>
    <tr>
        <td><span data-ttu-id="9c7dd-296">Kinesiska, förenklad</span><span class="sxs-lookup"><span data-stu-id="9c7dd-296">Chinese Simplified</span></span></td>
        <td><span data-ttu-id="9c7dd-297">zh-Hans.microsoft</span><span class="sxs-lookup"><span data-stu-id="9c7dd-297">zh-Hans.microsoft</span></span></td>
        <td><span data-ttu-id="9c7dd-298">zh-Hans.lucene</span><span class="sxs-lookup"><span data-stu-id="9c7dd-298">zh-Hans.lucene</span></span></td>        
    </tr>
    <tr>
        <td><span data-ttu-id="9c7dd-299">Kinesiska, traditionell</span><span class="sxs-lookup"><span data-stu-id="9c7dd-299">Chinese Traditional</span></span></td>
        <td><span data-ttu-id="9c7dd-300">zh-Hant.microsoft</span><span class="sxs-lookup"><span data-stu-id="9c7dd-300">zh-Hant.microsoft</span></span></td>
        <td><span data-ttu-id="9c7dd-301">zh-Hant.lucene</span><span class="sxs-lookup"><span data-stu-id="9c7dd-301">zh-Hant.lucene</span></span></td>        
    <tr>
    <tr>
        <td><span data-ttu-id="9c7dd-302">Kroatiska</span><span class="sxs-lookup"><span data-stu-id="9c7dd-302">Croatian</span></span></td>
        <td><span data-ttu-id="9c7dd-303">HR.Microsoft</span><span class="sxs-lookup"><span data-stu-id="9c7dd-303">hr.microsoft</span></span></td>
        <td/></td>
    </tr>
    <tr>
        <td><span data-ttu-id="9c7dd-304">Tjeckiska</span><span class="sxs-lookup"><span data-stu-id="9c7dd-304">Czech</span></span></td>
        <td><span data-ttu-id="9c7dd-305">CS.Microsoft</span><span class="sxs-lookup"><span data-stu-id="9c7dd-305">cs.microsoft</span></span></td>
        <td><span data-ttu-id="9c7dd-306">CS.lucene</span><span class="sxs-lookup"><span data-stu-id="9c7dd-306">cs.lucene</span></span></td>        
    </tr>    
    <tr>
        <td><span data-ttu-id="9c7dd-307">Danska</span><span class="sxs-lookup"><span data-stu-id="9c7dd-307">Danish</span></span></td>
        <td><span data-ttu-id="9c7dd-308">da.Microsoft</span><span class="sxs-lookup"><span data-stu-id="9c7dd-308">da.microsoft</span></span></td>
        <td><span data-ttu-id="9c7dd-309">da.lucene</span><span class="sxs-lookup"><span data-stu-id="9c7dd-309">da.lucene</span></span></td>        
    </tr>    
    <tr>
        <td><span data-ttu-id="9c7dd-310">Nederländska</span><span class="sxs-lookup"><span data-stu-id="9c7dd-310">Dutch</span></span></td>
        <td><span data-ttu-id="9c7dd-311">NL.Microsoft</span><span class="sxs-lookup"><span data-stu-id="9c7dd-311">nl.microsoft</span></span></td>
        <td><span data-ttu-id="9c7dd-312">NL.lucene</span><span class="sxs-lookup"><span data-stu-id="9c7dd-312">nl.lucene</span></span></td>    
    </tr>    
    <tr>
        <td><span data-ttu-id="9c7dd-313">Svenska</span><span class="sxs-lookup"><span data-stu-id="9c7dd-313">English</span></span></td>        
        <td><span data-ttu-id="9c7dd-314">en.Microsoft</span><span class="sxs-lookup"><span data-stu-id="9c7dd-314">en.microsoft</span></span></td>
        <td><span data-ttu-id="9c7dd-315">en.lucene</span><span class="sxs-lookup"><span data-stu-id="9c7dd-315">en.lucene</span></span></td>        
    </tr>
    <tr>
        <td><span data-ttu-id="9c7dd-316">Estniska</span><span class="sxs-lookup"><span data-stu-id="9c7dd-316">Estonian</span></span></td>
        <td><span data-ttu-id="9c7dd-317">et.Microsoft</span><span class="sxs-lookup"><span data-stu-id="9c7dd-317">et.microsoft</span></span></td>
        <td></td>
    </tr>
    <tr>
        <td><span data-ttu-id="9c7dd-318">Finska</span><span class="sxs-lookup"><span data-stu-id="9c7dd-318">Finnish</span></span></td>
        <td><span data-ttu-id="9c7dd-319">Fi.Microsoft</span><span class="sxs-lookup"><span data-stu-id="9c7dd-319">fi.microsoft</span></span></td>
        <td><span data-ttu-id="9c7dd-320">Fi.lucene</span><span class="sxs-lookup"><span data-stu-id="9c7dd-320">fi.lucene</span></span></td>        
    </tr>    
    <tr>
        <td><span data-ttu-id="9c7dd-321">Franska</span><span class="sxs-lookup"><span data-stu-id="9c7dd-321">French</span></span></td>
        <td><span data-ttu-id="9c7dd-322">fr.Microsoft</span><span class="sxs-lookup"><span data-stu-id="9c7dd-322">fr.microsoft</span></span></td>
        <td><span data-ttu-id="9c7dd-323">fr.lucene</span><span class="sxs-lookup"><span data-stu-id="9c7dd-323">fr.lucene</span></span></td>        
    </tr>
    <tr>
        <td><span data-ttu-id="9c7dd-324">Galiciska</span><span class="sxs-lookup"><span data-stu-id="9c7dd-324">Galician</span></span></td>
        <td></td>
        <td><span data-ttu-id="9c7dd-325">GL.lucene</span><span class="sxs-lookup"><span data-stu-id="9c7dd-325">gl.lucene</span></span></td>        
      </tr>
    <tr>
        <td><span data-ttu-id="9c7dd-326">Tyska</span><span class="sxs-lookup"><span data-stu-id="9c7dd-326">German</span></span></td>
        <td><span data-ttu-id="9c7dd-327">de.Microsoft</span><span class="sxs-lookup"><span data-stu-id="9c7dd-327">de.microsoft</span></span></td>
        <td><span data-ttu-id="9c7dd-328">de.lucene</span><span class="sxs-lookup"><span data-stu-id="9c7dd-328">de.lucene</span></span></td>        
    </tr>
    <tr>
        <td><span data-ttu-id="9c7dd-329">Grekiska</span><span class="sxs-lookup"><span data-stu-id="9c7dd-329">Greek</span></span></td>
        <td><span data-ttu-id="9c7dd-330">el.Microsoft</span><span class="sxs-lookup"><span data-stu-id="9c7dd-330">el.microsoft</span></span></td>
        <td><span data-ttu-id="9c7dd-331">el.lucene</span><span class="sxs-lookup"><span data-stu-id="9c7dd-331">el.lucene</span></span></td>        
    </tr>
    <tr>
        <td><span data-ttu-id="9c7dd-332">Gujarati</span><span class="sxs-lookup"><span data-stu-id="9c7dd-332">Gujarati</span></span></td>
        <td><span data-ttu-id="9c7dd-333">Gu.Microsoft</span><span class="sxs-lookup"><span data-stu-id="9c7dd-333">gu.microsoft</span></span></td>
        <td></td>
    </tr>
    <tr>
        <td><span data-ttu-id="9c7dd-334">Hebreiska</span><span class="sxs-lookup"><span data-stu-id="9c7dd-334">Hebrew</span></span></td>
        <td><span data-ttu-id="9c7dd-335">HE.Microsoft</span><span class="sxs-lookup"><span data-stu-id="9c7dd-335">he.microsoft</span></span></td>
        <td></td>
    </tr>
    <tr>
        <td><span data-ttu-id="9c7dd-336">Hindi</span><span class="sxs-lookup"><span data-stu-id="9c7dd-336">Hindi</span></span></td>
        <td><span data-ttu-id="9c7dd-337">Hi.Microsoft</span><span class="sxs-lookup"><span data-stu-id="9c7dd-337">hi.microsoft</span></span></td>
        <td><span data-ttu-id="9c7dd-338">Hi.lucene</span><span class="sxs-lookup"><span data-stu-id="9c7dd-338">hi.lucene</span></span></td>        
    </tr>
    <tr>
        <td><span data-ttu-id="9c7dd-339">Ungerska</span><span class="sxs-lookup"><span data-stu-id="9c7dd-339">Hungarian</span></span></td>        
        <td><span data-ttu-id="9c7dd-340">HU.Microsoft</span><span class="sxs-lookup"><span data-stu-id="9c7dd-340">hu.microsoft</span></span></td>
        <td><span data-ttu-id="9c7dd-341">HU.lucene</span><span class="sxs-lookup"><span data-stu-id="9c7dd-341">hu.lucene</span></span></td>
    </tr>
    <tr>
        <td><span data-ttu-id="9c7dd-342">Isländska</span><span class="sxs-lookup"><span data-stu-id="9c7dd-342">Icelandic</span></span></td>
        <td><span data-ttu-id="9c7dd-343">is.Microsoft</span><span class="sxs-lookup"><span data-stu-id="9c7dd-343">is.microsoft</span></span></td>
        <td></td>
    </tr>
    <tr>
        <td><span data-ttu-id="9c7dd-344">Indonesiska (Bahasa)</span><span class="sxs-lookup"><span data-stu-id="9c7dd-344">Indonesian (Bahasa)</span></span></td>
        <td><span data-ttu-id="9c7dd-345">ID.Microsoft</span><span class="sxs-lookup"><span data-stu-id="9c7dd-345">id.microsoft</span></span></td>
        <td><span data-ttu-id="9c7dd-346">ID.lucene</span><span class="sxs-lookup"><span data-stu-id="9c7dd-346">id.lucene</span></span></td>        
    </tr>
    <tr>
        <td><span data-ttu-id="9c7dd-347">Irländska</span><span class="sxs-lookup"><span data-stu-id="9c7dd-347">Irish</span></span></td>
        <td></td>
          <td><span data-ttu-id="9c7dd-348">GA.lucene</span><span class="sxs-lookup"><span data-stu-id="9c7dd-348">ga.lucene</span></span></td>
    </tr>
    <tr>
        <td><span data-ttu-id="9c7dd-349">Italienska</span><span class="sxs-lookup"><span data-stu-id="9c7dd-349">Italian</span></span></td>
        <td><span data-ttu-id="9c7dd-350">IT.Microsoft</span><span class="sxs-lookup"><span data-stu-id="9c7dd-350">it.microsoft</span></span></td>
        <td><span data-ttu-id="9c7dd-351">IT.lucene</span><span class="sxs-lookup"><span data-stu-id="9c7dd-351">it.lucene</span></span></td>        
    </tr>
    <tr>
        <td><span data-ttu-id="9c7dd-352">Japanska</span><span class="sxs-lookup"><span data-stu-id="9c7dd-352">Japanese</span></span></td>
        <td><span data-ttu-id="9c7dd-353">Ja.Microsoft</span><span class="sxs-lookup"><span data-stu-id="9c7dd-353">ja.microsoft</span></span></td>
        <td><span data-ttu-id="9c7dd-354">Ja.lucene</span><span class="sxs-lookup"><span data-stu-id="9c7dd-354">ja.lucene</span></span></td>

    </tr>
    <tr>
        <td><span data-ttu-id="9c7dd-355">Kannada</span><span class="sxs-lookup"><span data-stu-id="9c7dd-355">Kannada</span></span></td>
        <td><span data-ttu-id="9c7dd-356">KA.Microsoft</span><span class="sxs-lookup"><span data-stu-id="9c7dd-356">ka.microsoft</span></span></td>
        <td></td>
    </tr>
    <tr>
        <td><span data-ttu-id="9c7dd-357">Koreanska</span><span class="sxs-lookup"><span data-stu-id="9c7dd-357">Korean</span></span></td>
        <td><span data-ttu-id="9c7dd-358">Ko.Microsoft</span><span class="sxs-lookup"><span data-stu-id="9c7dd-358">ko.microsoft</span></span></td>
        <td><span data-ttu-id="9c7dd-359">Ko.lucene</span><span class="sxs-lookup"><span data-stu-id="9c7dd-359">ko.lucene</span></span></td>
    </tr>
    <tr>
        <td><span data-ttu-id="9c7dd-360">Lettiska</span><span class="sxs-lookup"><span data-stu-id="9c7dd-360">Latvian</span></span></td>        
        <td><span data-ttu-id="9c7dd-361">LV.Microsoft</span><span class="sxs-lookup"><span data-stu-id="9c7dd-361">lv.microsoft</span></span></td>
        <td><span data-ttu-id="9c7dd-362">LV.lucene</span><span class="sxs-lookup"><span data-stu-id="9c7dd-362">lv.lucene</span></span></td>    
    </tr>
    <tr>
        <td><span data-ttu-id="9c7dd-363">Litauiska</span><span class="sxs-lookup"><span data-stu-id="9c7dd-363">Lithuanian</span></span></td>
        <td><span data-ttu-id="9c7dd-364">lt.Microsoft</span><span class="sxs-lookup"><span data-stu-id="9c7dd-364">lt.microsoft</span></span></td>
        <td></td>
    </tr>
    <tr>
        <td><span data-ttu-id="9c7dd-365">Malayalam</span><span class="sxs-lookup"><span data-stu-id="9c7dd-365">Malayalam</span></span></td>
        <td><span data-ttu-id="9c7dd-366">ml.Microsoft</span><span class="sxs-lookup"><span data-stu-id="9c7dd-366">ml.microsoft</span></span></td>
        <td></td>
    </tr>
    <tr>
        <td><span data-ttu-id="9c7dd-367">Malajiska (latinsk)</span><span class="sxs-lookup"><span data-stu-id="9c7dd-367">Malay (Latin)</span></span></td>
        <td><span data-ttu-id="9c7dd-368">MS.Microsoft</span><span class="sxs-lookup"><span data-stu-id="9c7dd-368">ms.microsoft</span></span></td>
        <td></td>
    </tr>
    <tr>
        <td><span data-ttu-id="9c7dd-369">Marathi</span><span class="sxs-lookup"><span data-stu-id="9c7dd-369">Marathi</span></span></td>
        <td><span data-ttu-id="9c7dd-370">MR.Microsoft</span><span class="sxs-lookup"><span data-stu-id="9c7dd-370">mr.microsoft</span></span></td>
        <td></td>
    </tr>
    <tr>
        <td><span data-ttu-id="9c7dd-371">Norska</span><span class="sxs-lookup"><span data-stu-id="9c7dd-371">Norwegian</span></span></td>
        <td><span data-ttu-id="9c7dd-372">NB.Microsoft</span><span class="sxs-lookup"><span data-stu-id="9c7dd-372">nb.microsoft</span></span></td>
        <td><span data-ttu-id="9c7dd-373">No.lucene</span><span class="sxs-lookup"><span data-stu-id="9c7dd-373">no.lucene</span></span></td>        
    </tr>
      <tr>
        <td><span data-ttu-id="9c7dd-374">Persiska</span><span class="sxs-lookup"><span data-stu-id="9c7dd-374">Persian</span></span></td>
        <td></td>
        <td><span data-ttu-id="9c7dd-375">fa.lucene</span><span class="sxs-lookup"><span data-stu-id="9c7dd-375">fa.lucene</span></span></td>        
      </tr>
    <tr>
        <td><span data-ttu-id="9c7dd-376">Polska</span><span class="sxs-lookup"><span data-stu-id="9c7dd-376">Polish</span></span></td>
        <td><span data-ttu-id="9c7dd-377">PL.Microsoft</span><span class="sxs-lookup"><span data-stu-id="9c7dd-377">pl.microsoft</span></span></td>
        <td><span data-ttu-id="9c7dd-378">PL.lucene</span><span class="sxs-lookup"><span data-stu-id="9c7dd-378">pl.lucene</span></span></td>        
    </tr>
    <tr>
        <td><span data-ttu-id="9c7dd-379">Portugisiska (Brasilien)</span><span class="sxs-lookup"><span data-stu-id="9c7dd-379">Portuguese (Brazil)</span></span></td>
        <td><span data-ttu-id="9c7dd-380">pt-Br.microsoft</span><span class="sxs-lookup"><span data-stu-id="9c7dd-380">pt-Br.microsoft</span></span></td>
        <td><span data-ttu-id="9c7dd-381">pt-Br.lucene</span><span class="sxs-lookup"><span data-stu-id="9c7dd-381">pt-Br.lucene</span></span></td>        
    </tr>
    <tr>
        <td><span data-ttu-id="9c7dd-382">Portugisiska (Portugal)</span><span class="sxs-lookup"><span data-stu-id="9c7dd-382">Portuguese (Portugal)</span></span></td>
        <td><span data-ttu-id="9c7dd-383">pt-Pt.microsoft</span><span class="sxs-lookup"><span data-stu-id="9c7dd-383">pt-Pt.microsoft</span></span></td>        
        <td><span data-ttu-id="9c7dd-384">pt-Pt.lucene</span><span class="sxs-lookup"><span data-stu-id="9c7dd-384">pt-Pt.lucene</span></span></td>
    </tr>
    <tr>
        <td><span data-ttu-id="9c7dd-385">Punjabi</span><span class="sxs-lookup"><span data-stu-id="9c7dd-385">Punjabi</span></span></td>
        <td><span data-ttu-id="9c7dd-386">pa.Microsoft</span><span class="sxs-lookup"><span data-stu-id="9c7dd-386">pa.microsoft</span></span></td>
        <td></td>
    </tr>
    <tr>
        <td><span data-ttu-id="9c7dd-387">Rumänska</span><span class="sxs-lookup"><span data-stu-id="9c7dd-387">Romanian</span></span></td>
        <td><span data-ttu-id="9c7dd-388">ro.Microsoft</span><span class="sxs-lookup"><span data-stu-id="9c7dd-388">ro.microsoft</span></span></td>
        <td><span data-ttu-id="9c7dd-389">ro.lucene</span><span class="sxs-lookup"><span data-stu-id="9c7dd-389">ro.lucene</span></span></td>
    </tr>
    <tr>
        <td><span data-ttu-id="9c7dd-390">Ryska</span><span class="sxs-lookup"><span data-stu-id="9c7dd-390">Russian</span></span></td>
        <td><span data-ttu-id="9c7dd-391">RU.Microsoft</span><span class="sxs-lookup"><span data-stu-id="9c7dd-391">ru.microsoft</span></span></td>
        <td><span data-ttu-id="9c7dd-392">RU.lucene</span><span class="sxs-lookup"><span data-stu-id="9c7dd-392">ru.lucene</span></span></td>    
    </tr>
    <tr>
        <td><span data-ttu-id="9c7dd-393">Serbiska (kyrillisk)</span><span class="sxs-lookup"><span data-stu-id="9c7dd-393">Serbian (Cyrillic)</span></span></td>
        <td><span data-ttu-id="9c7dd-394">SR-cyrillic.microsoft</span><span class="sxs-lookup"><span data-stu-id="9c7dd-394">sr-cyrillic.microsoft</span></span></td>
        <td></td>
    </tr>
    <tr>
        <td><span data-ttu-id="9c7dd-395">Serbiska (latinsk)</span><span class="sxs-lookup"><span data-stu-id="9c7dd-395">Serbian (Latin)</span></span></td>
        <td><span data-ttu-id="9c7dd-396">SR-latin.microsoft</span><span class="sxs-lookup"><span data-stu-id="9c7dd-396">sr-latin.microsoft</span></span></td>
        <td></td>
    </tr>
    <tr>
        <td><span data-ttu-id="9c7dd-397">Slovakiska</span><span class="sxs-lookup"><span data-stu-id="9c7dd-397">Slovak</span></span></td>
        <td><span data-ttu-id="9c7dd-398">Sk.Microsoft</span><span class="sxs-lookup"><span data-stu-id="9c7dd-398">sk.microsoft</span></span></td>
        <td></td>
    </tr>
    <tr>
        <td><span data-ttu-id="9c7dd-399">Slovenska</span><span class="sxs-lookup"><span data-stu-id="9c7dd-399">Slovenian</span></span></td>
        <td><span data-ttu-id="9c7dd-400">Sl.Microsoft</span><span class="sxs-lookup"><span data-stu-id="9c7dd-400">sl.microsoft</span></span></td>
        <td></td>
    </tr>
    <tr>
        <td><span data-ttu-id="9c7dd-401">Spanska</span><span class="sxs-lookup"><span data-stu-id="9c7dd-401">Spanish</span></span></td>
        <td><span data-ttu-id="9c7dd-402">ES.Microsoft</span><span class="sxs-lookup"><span data-stu-id="9c7dd-402">es.microsoft</span></span></td>
        <td><span data-ttu-id="9c7dd-403">ES.lucene</span><span class="sxs-lookup"><span data-stu-id="9c7dd-403">es.lucene</span></span></td>
    </tr>
    <tr>
        <td><span data-ttu-id="9c7dd-404">Svenska</span><span class="sxs-lookup"><span data-stu-id="9c7dd-404">Swedish</span></span></td>
        <td><span data-ttu-id="9c7dd-405">SV.Microsoft</span><span class="sxs-lookup"><span data-stu-id="9c7dd-405">sv.microsoft</span></span></td>
        <td><span data-ttu-id="9c7dd-406">SV.lucene</span><span class="sxs-lookup"><span data-stu-id="9c7dd-406">sv.lucene</span></span></td>
    </tr>

    <tr>
        <td><span data-ttu-id="9c7dd-407">Tamilska</span><span class="sxs-lookup"><span data-stu-id="9c7dd-407">Tamil</span></span></td>
        <td><span data-ttu-id="9c7dd-408">ta.Microsoft</span><span class="sxs-lookup"><span data-stu-id="9c7dd-408">ta.microsoft</span></span></td>
        <td></td>
    </tr>
    <tr>
        <td><span data-ttu-id="9c7dd-409">Telugu</span><span class="sxs-lookup"><span data-stu-id="9c7dd-409">Telugu</span></span></td>
        <td><span data-ttu-id="9c7dd-410">te.Microsoft</span><span class="sxs-lookup"><span data-stu-id="9c7dd-410">te.microsoft</span></span></td>
        <td></td>
    </tr>
    <tr>
        <td><span data-ttu-id="9c7dd-411">Thai</span><span class="sxs-lookup"><span data-stu-id="9c7dd-411">Thai</span></span></td>
        <td><span data-ttu-id="9c7dd-412">TH.Microsoft</span><span class="sxs-lookup"><span data-stu-id="9c7dd-412">th.microsoft</span></span></td>
        <td><span data-ttu-id="9c7dd-413">TH.lucene</span><span class="sxs-lookup"><span data-stu-id="9c7dd-413">th.lucene</span></span></td>
    </tr>
    <tr>
        <td><span data-ttu-id="9c7dd-414">Turkiska</span><span class="sxs-lookup"><span data-stu-id="9c7dd-414">Turkish</span></span></td>
        <td><span data-ttu-id="9c7dd-415">TR.Microsoft</span><span class="sxs-lookup"><span data-stu-id="9c7dd-415">tr.microsoft</span></span></td>
        <td><span data-ttu-id="9c7dd-416">TR.lucene</span><span class="sxs-lookup"><span data-stu-id="9c7dd-416">tr.lucene</span></span></td>        
    </tr>
    <tr>
        <td><span data-ttu-id="9c7dd-417">Ukrainska</span><span class="sxs-lookup"><span data-stu-id="9c7dd-417">Ukrainian</span></span></td>
        <td><span data-ttu-id="9c7dd-418">UK.Microsoft</span><span class="sxs-lookup"><span data-stu-id="9c7dd-418">uk.microsoft</span></span></td>
        <td></td>
    </tr>
    <tr>
        <td><span data-ttu-id="9c7dd-419">Urdu</span><span class="sxs-lookup"><span data-stu-id="9c7dd-419">Urdu</span></span></td>
        <td><span data-ttu-id="9c7dd-420">Your.Microsoft</span><span class="sxs-lookup"><span data-stu-id="9c7dd-420">ur.microsoft</span></span></td>
        <td></td>
    </tr>
    <tr>
        <td><span data-ttu-id="9c7dd-421">Vietnamesiska</span><span class="sxs-lookup"><span data-stu-id="9c7dd-421">Vietnamese</span></span></td>
        <td><span data-ttu-id="9c7dd-422">Vi.Microsoft</span><span class="sxs-lookup"><span data-stu-id="9c7dd-422">vi.microsoft</span></span></td>
        <td></td>
    </tr>
    <td colspan="3"><span data-ttu-id="9c7dd-423">Dessutom ger Azure Search språkoberoende analyzer konfigurationer</span><span class="sxs-lookup"><span data-stu-id="9c7dd-423">Additionally Azure Search provides language-agnostic analyzer configurations</span></span></td>
    <tr>
        <td><span data-ttu-id="9c7dd-424">Standard-ASCII vikning</span><span class="sxs-lookup"><span data-stu-id="9c7dd-424">Standard ASCII Folding</span></span></td>
        <td><span data-ttu-id="9c7dd-425">standardasciifolding.lucene</span><span class="sxs-lookup"><span data-stu-id="9c7dd-425">standardasciifolding.lucene</span></span></td>
        <td>
        <ul>
            <li><span data-ttu-id="9c7dd-426">Unicode-text segmentering (Standard Tokenizer)</span><span class="sxs-lookup"><span data-stu-id="9c7dd-426">Unicode text segmentation (Standard Tokenizer)</span></span></li>
            <li><span data-ttu-id="9c7dd-427">Vikning filtret för ASCII - omvandlar Unicode-tecken som inte tillhör toohello uppsättning först 127 ASCII-tecken till deras motsvarigheter i ASCII.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-427">ASCII folding filter - converts Unicode characters that don't belong toohello set of first 127 ASCII characters into their ASCII equivalents.</span></span> <span data-ttu-id="9c7dd-428">Detta är användbart för att ta bort diakritiska tecken.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-428">This is useful for removing diacritics.</span></span></li>
        </ul>
        </td>
    </tr>
</table>

<span data-ttu-id="9c7dd-429">Alla analyzers med namn som är försedd med <i>lucene</i> drivs av [Apache Lucene språkanalys](http://lucene.apache.org/core/4_9_0/analyzers-common/overview-summary.html).</span><span class="sxs-lookup"><span data-stu-id="9c7dd-429">All analyzers with names annotated with <i>lucene</i> are powered by [Apache Lucene's language analyzers](http://lucene.apache.org/core/4_9_0/analyzers-common/overview-summary.html).</span></span> <span data-ttu-id="9c7dd-430">Mer information om hello ASCII vikning filtret hittar [här](http://lucene.apache.org/core/4_9_0/analyzers-common/org/apache/lucene/analysis/miscellaneous/ASCIIFoldingFilter.html).</span><span class="sxs-lookup"><span data-stu-id="9c7dd-430">More information about hello ASCII folding filter can be found [here](http://lucene.apache.org/core/4_9_0/analyzers-common/org/apache/lucene/analysis/miscellaneous/ASCIIFoldingFilter.html).</span></span>

<span data-ttu-id="9c7dd-431">**Förslag på alternativ**</span><span class="sxs-lookup"><span data-stu-id="9c7dd-431">**Suggesters**</span></span>

<span data-ttu-id="9c7dd-432">En `suggester` definierar vilka fält i ett index som används toosupport automatisk komplettering i sökningar.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-432">A `suggester` defines which fields in an index are used toosupport auto-complete in searches.</span></span> <span data-ttu-id="9c7dd-433">Vanligtvis partiella söksträngar skickas toohello [förslag API](#Suggestions) medan hello användaren skriver en sökfråga och hello API returnerar en uppsättning föreslagna fraser.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-433">Typically partial search strings are sent toohello [Suggestions API](#Suggestions) while hello user is typing a search query, and hello API returns a set of suggested phrases.</span></span> <span data-ttu-id="9c7dd-434">En förslagsställarens som du definierar i hello index anger vilka fält som används toobuild hello typ-ahead sökvillkoren.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-434">A suggester that you define in hello index determines which fields are used toobuild hello type-ahead search terms.</span></span> <span data-ttu-id="9c7dd-435">Se [Suggesters](#Suggesters) konfigurationsinformation.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-435">See [Suggesters](#Suggesters) for configuration details.</span></span>

<span data-ttu-id="9c7dd-436">**Poängprofiler**</span><span class="sxs-lookup"><span data-stu-id="9c7dd-436">**Scoring profiles**</span></span>

<span data-ttu-id="9c7dd-437">En `scoringProfile` definierar anpassade bedömningsprofil beteenden som gör att du kan påverka vilka objekt som visas högre upp i hello sökresultat.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-437">A `scoringProfile` defines custom scoring behaviors that let you influence which items appear higher in hello search results.</span></span> <span data-ttu-id="9c7dd-438">Bedömningsprofil profiler består av fältvikter och funktioner.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-438">Scoring profiles are made up of field weights and functions.</span></span> <span data-ttu-id="9c7dd-439">toouse dem du anger en profil med namnet på hello frågesträngen.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-439">toouse them, you specify a profile by name on hello query string.</span></span>

<span data-ttu-id="9c7dd-440">Standard bedömningen profil verkar bakom hello i bakgrunden toocompute Sök poängen för varje objekt i en resultatuppsättning.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-440">A default scoring profile operates behind hello scenes toocompute a search score for every item in a result set.</span></span> <span data-ttu-id="9c7dd-441">Du kan använda hello interna, namnlösa bedömningsprofilen.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-441">You can use hello internal, unnamed scoring profile.</span></span> <span data-ttu-id="9c7dd-442">Du kan också ange `defaultScoringProfile` toouse en anpassad profil som hello standard anropas när en anpassad profil inte har angetts på hello frågesträngen.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-442">Alternatively, set `defaultScoringProfile` toouse a custom profile as hello default, invoked whenever a custom profile is not specified on hello query string.</span></span>

<span data-ttu-id="9c7dd-443">Se [Lägg till bedömningen profiler tooa sökindex (Azure Söktjänsts-REST API)](search-api-scoring-profiles-2015-02-28-preview.md) mer information.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-443">See [Add scoring profiles tooa search index (Azure Search Service REST API)](search-api-scoring-profiles-2015-02-28-preview.md) for details.</span></span>

<span data-ttu-id="9c7dd-444">**Alternativ för CORS**</span><span class="sxs-lookup"><span data-stu-id="9c7dd-444">**CORS Options**</span></span>

<span data-ttu-id="9c7dd-445">Javascript för klientsidan kan inte anropa alla API: er som standard eftersom hello webbläsaren hindrar alla cross-origin-begäranden.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-445">Client-side Javascript cannot call any APIs by default since hello browser will prevent all cross-origin requests.</span></span> <span data-ttu-id="9c7dd-446">Aktivera CORS (Cross-Origin Resource Sharing) genom att ange hello `corsOptions` attributet tooallow cross-origin frågor tooyour index.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-446">Enable CORS (Cross-Origin Resource Sharing) by setting hello `corsOptions` attribute tooallow cross-origin queries tooyour index.</span></span> <span data-ttu-id="9c7dd-447">Observera att endast query API: er stöd CORS av säkerhetsskäl.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-447">Note that only query APIs support CORS for security reasons.</span></span> <span data-ttu-id="9c7dd-448">hello följande alternativ kan anges för CORS:</span><span class="sxs-lookup"><span data-stu-id="9c7dd-448">hello following options can be set for CORS:</span></span>

* <span data-ttu-id="9c7dd-449">`allowedOrigins`(krävs): det här är en lista över ursprung som beviljas åtkomst tooyour index.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-449">`allowedOrigins` (required): This is a list of origins that will be granted access tooyour index.</span></span> <span data-ttu-id="9c7dd-450">Detta innebär att all Javascript-kod från dessa ursprung blir tillåtna tooquery ditt index (förutsatt att den ger hello rätt API-nyckel).</span><span class="sxs-lookup"><span data-stu-id="9c7dd-450">This means that any Javascript code served from those origins will be allowed tooquery your index (assuming it provides hello correct API key).</span></span> <span data-ttu-id="9c7dd-451">Varje ursprung har vanligtvis hello formuläret `protocol://fully-qualified-domain-name:port` även om hello port utelämnas ofta.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-451">Each origin is typically of hello form `protocol://fully-qualified-domain-name:port` although hello port is often omitted.</span></span> <span data-ttu-id="9c7dd-452">Se [i den här artikeln](http://go.microsoft.com/fwlink/?LinkId=330822) för mer information.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-452">See [this article](http://go.microsoft.com/fwlink/?LinkId=330822) for more details.</span></span>
  * <span data-ttu-id="9c7dd-453">Även om du vill tooallow åtkomst tooall ursprung `*` som ett enskilt objekt i hello `allowedOrigins` matris.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-453">If you want tooallow access tooall origins, include `*` as a single item in hello `allowedOrigins` array.</span></span> <span data-ttu-id="9c7dd-454">Observera att **detta rekommenderas inte för produktion search-tjänster.**</span><span class="sxs-lookup"><span data-stu-id="9c7dd-454">Note that **this is not recommended practice for production search services.**</span></span> <span data-ttu-id="9c7dd-455">Det kan dock vara användbar för utveckling eller felsökning.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-455">However, it may be useful for development or debugging purposes.</span></span>
* <span data-ttu-id="9c7dd-456">`maxAgeInSeconds`(valfritt): webbläsare använder det här värdet toodetermine hello (i sekunder) toocache CORS preflight-svar.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-456">`maxAgeInSeconds` (optional): Browsers use this value toodetermine hello duration (in seconds) toocache CORS preflight responses.</span></span> <span data-ttu-id="9c7dd-457">Detta måste vara ett icke-negativt heltal.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-457">This must be a non-negative integer.</span></span> <span data-ttu-id="9c7dd-458">hello större det här värdet är hello bättre prestanda, men hello längre tid det tar för CORS princip ändringar tootake effekt.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-458">hello larger this value is, hello better performance will be, but hello longer it will take for CORS policy changes tootake effect.</span></span> <span data-ttu-id="9c7dd-459">Om den inte är inställd kommer på 5 minuter att används standardvärdet.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-459">If it is not set, a default duration of 5 minutes will be used.</span></span>

<span data-ttu-id="9c7dd-460"><a name="CreateUpdateIndexExample"></a>
**Begäran Body-exempel**</span><span class="sxs-lookup"><span data-stu-id="9c7dd-460"><a name="CreateUpdateIndexExample"></a>
**Request Body Example**</span></span>

    {
      "name": "hotels",  
      "fields": [
        {"name": "hotelId", "type": "Edm.String", "key": true, "searchable": false},
        {"name": "baseRate", "type": "Edm.Double"},
        {"name": "description", "type": "Edm.String", "filterable": false, "sortable": false, "facetable": false},
        {"name": "description_fr", "type": "Edm.String", "filterable": false, "sortable": false, "facetable": false, "analyzer": "fr.lucene"},
        {"name": "hotelName", "type": "Edm.String"},
        {"name": "category", "type": "Edm.String"},
        {"name": "tags", "type": "Collection(Edm.String)"},
        {"name": "parkingIncluded", "type": "Edm.Boolean"},
        {"name": "smokingAllowed", "type": "Edm.Boolean"},
        {"name": "lastRenovationDate", "type": "Edm.DateTimeOffset"},
        {"name": "rating", "type": "Edm.Int32"},
        {"name": "location", "type": "Edm.GeographyPoint"}
      ],
      "suggesters": [
        {
          "name": "sg",
          "searchMode": "analyzingInfixMatching",
          "sourceFields": ["hotelName"]
        }
      ]
    }

<span data-ttu-id="9c7dd-461">**Svar**</span><span class="sxs-lookup"><span data-stu-id="9c7dd-461">**Response**</span></span>

<span data-ttu-id="9c7dd-462">För en lyckad begäran: ”201 skapad”.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-462">For a successful request: "201 Created".</span></span>

<span data-ttu-id="9c7dd-463">Som standard innehåller hello svarstexten hello JSON för hello indexdefinitionen som har skapats.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-463">By default hello response body will contain hello JSON for hello index definition that was created.</span></span> <span data-ttu-id="9c7dd-464">Om hello `Prefer` begärandehuvudet har angetts för`return=minimal`hello svarstexten är tomt och hello lyckade statuskoden ska vara ”204 saknar innehåll” i stället för ”201 skapad”.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-464">If hello `Prefer` request header is set too`return=minimal`, hello response body will be empty and hello success status code will be "204 No Content" instead of "201 Created".</span></span> <span data-ttu-id="9c7dd-465">Detta gäller oavsett om PUT eller POST har använt toocreate hello index.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-465">This is true regardless of whether PUT or POST was used toocreate hello index.</span></span>

<span data-ttu-id="9c7dd-466">**Kommentarer**</span><span class="sxs-lookup"><span data-stu-id="9c7dd-466">**Remarks**</span></span>

<span data-ttu-id="9c7dd-467">Det finns för närvarande begränsat stöd för index schema-uppdateringar.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-467">Currently, there is limited support for index schema updates.</span></span> <span data-ttu-id="9c7dd-468">Schema-uppdateringar som kräver omindexering, till exempel ändring av fälttyp stöds inte för närvarande.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-468">Any schema updates that would require re-indexing such as changing field types are not currently supported.</span></span> <span data-ttu-id="9c7dd-469">Även om befintliga fält inte kan ändras eller tas bort kan läggas nya fält tooan befintligt index när som helst.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-469">Although existing fields cannot be changed or deleted, new fields can be added tooan existing index at any time.</span></span> <span data-ttu-id="9c7dd-470">När ett nytt fält läggs alla befintliga dokument i hello index automatiskt att ha ett null-värde för fältet.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-470">When a new field is added, all existing documents in hello index will automatically have a null value for that field.</span></span> <span data-ttu-id="9c7dd-471">Inga ytterligare lagringsutrymme används förrän nya dokument har lagts till toohello index.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-471">No additional storage space will be consumed until new documents are added toohello index.</span></span>

<a name="Suggesters"></a>

## <a name="suggesters"></a><span data-ttu-id="9c7dd-472">Förslag på alternativ</span><span class="sxs-lookup"><span data-stu-id="9c7dd-472">Suggesters</span></span>
<span data-ttu-id="9c7dd-473">hello förslag funktion i Azure Search är en typ i förväg eller automatisk komplettering frågan-funktion som ger en lista över möjliga sökorden i svar toopartial sträng indata som angetts i Sök-rutan.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-473">hello suggestions feature in Azure Search is a type-ahead or auto-complete query capability, providing a list of potential search terms in response toopartial string inputs entered into a search box.</span></span> <span data-ttu-id="9c7dd-474">Har du antagligen märkt frågeförslag när du använder kommersiella sökmotorer: att skriva ”.NET” i Bing genererar en lista över villkor för ”.NET 4.5” ”, .NET Framework 3,5-tums, och så vidare.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-474">You've probably noticed query suggestions when using commercial web search engines: typing ".NET" in Bing produces a list of terms for ".NET 4.5", ".NET Framework 3.5", and so forth.</span></span> <span data-ttu-id="9c7dd-475">När du använder hello söktjänsten REST-API, kräver implementera förslag i ett anpassat program för Azure Search hello följande:</span><span class="sxs-lookup"><span data-stu-id="9c7dd-475">When using hello Search service REST API, implementing suggestions in a custom Azure Search application requires hello following:</span></span>

* <span data-ttu-id="9c7dd-476">Aktivera förslag genom att lägga till en **förslagsställarens** konstruktionen i ditt index ger hello namn, sökläget och en lista med fält som typen ahead anropas.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-476">Enable suggestions by adding a **suggester** construction in your index, giving hello name, search mode, and a list of fields for which type-ahead is invoked.</span></span> <span data-ttu-id="9c7dd-477">Till exempel om du anger ”cityName” som ett fält i datakällan att skriva en partiell sökning sträng med ”Sea” leder till att ”Seattle”, ”snäckor” och ”Seatac” (alla tre är faktiska stadsnamn) erbjuds som frågan förslag toohello användare.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-477">For example, if you specify "cityName" as a source field, typing a partial search string of "Sea" will result in "Seattle", "Seaside", and "Seatac" (all three are actual city names) offered up as query suggestions toohello user.</span></span>
* <span data-ttu-id="9c7dd-478">Anropa förslag genom att anropa hello [förslag API](#Suggestions) i programkoden.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-478">Invoke suggestions by calling hello [Suggestions API](#Suggestions) in your application code.</span></span> <span data-ttu-id="9c7dd-479">Vanligtvis en partiell söksträngar skickas toohello tjänsten medan hello användaren skriver en sökfråga och returnerar en uppsättning föreslagna fraser för detta API.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-479">Typically partial search strings are sent toohello service while hello user is typing a search query, and this API returns a set of suggested phrases.</span></span>

<span data-ttu-id="9c7dd-480">Den här artikeln förklarar hur tooconfigure en **förslagsställarens**.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-480">This article explains how tooconfigure a **suggester**.</span></span> <span data-ttu-id="9c7dd-481">Du bör också granska hello [förslag API](#Suggestions) information om hur en förslagsställarens används.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-481">You should also review hello [Suggestions API](#Suggestions) for details on how a suggester is used.</span></span>

<span data-ttu-id="9c7dd-482">**Användning**</span><span class="sxs-lookup"><span data-stu-id="9c7dd-482">**Usage**</span></span>

<span data-ttu-id="9c7dd-483">`Suggesters`skapas i hello index och fungerar bäst när det används toosuggest specifika dokument i stället för att lösa ord eller fraser.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-483">`Suggesters` are created in hello index and work best when used toosuggest specific documents rather than loose terms or phrases.</span></span> <span data-ttu-id="9c7dd-484">hello bra kandidat fält är titlar, namn och andra relativt korta fraser som kan identifiera ett objekt.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-484">hello best candidate fields are titles, names, and other relatively short phrases that can identify an item.</span></span> <span data-ttu-id="9c7dd-485">Mindre effektiva är återkommande fält, till exempel kategorier och taggar eller mycket långa fält, till exempel beskrivningar och kommentarer fält.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-485">Less effective are repetitive fields, such as categories and tags, or very long fields such as descriptions or comments fields.</span></span>

<span data-ttu-id="9c7dd-486">Som en del av hello indexdefinitionen, kan du lägga till en enda förslagsställarens toohello `suggesters` samling.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-486">As part of hello index definition, you can add a single suggester toohello `suggesters` collection.</span></span> <span data-ttu-id="9c7dd-487">Egenskaper som definierar en förslagsställarens inkludera hello följande:</span><span class="sxs-lookup"><span data-stu-id="9c7dd-487">Properties that define a suggester include hello following:</span></span>

* <span data-ttu-id="9c7dd-488">`name`: hello hello förslagsställarens namn.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-488">`name`: hello name of hello suggester.</span></span> <span data-ttu-id="9c7dd-489">Du kan använda hello hello förslagsställarens namn när du anropar hello `suggest` API.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-489">You use hello name of hello suggester when calling hello `suggest` API.</span></span>
* <span data-ttu-id="9c7dd-490">`searchMode`: hello strategi som används för toosearch efter möjliga fraser.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-490">`searchMode`: hello strategy used toosearch for candidate phrases.</span></span> <span data-ttu-id="9c7dd-491">hello läge stöds för närvarande är `analyzingInfixMatching`, vilket genomför flexibla matchningar av fraser i början av hello eller hello mitten av meningarna.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-491">hello only mode currently supported is `analyzingInfixMatching`, which performs flexible matching of phrases at hello beginning or in hello middle of sentences.</span></span>
* <span data-ttu-id="9c7dd-492">`sourceFields`: En lista över ett eller flera fält som är hello informationskälla hello förslag.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-492">`sourceFields`: A list of one or more fields that are hello source of hello content for suggestions.</span></span> <span data-ttu-id="9c7dd-493">Endast fält av typen `Edm.String` och `Collection(Edm.String)` kanske källor för förslag.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-493">Only fields of type `Edm.String` and `Collection(Edm.String)` may be sources for suggestions.</span></span> <span data-ttu-id="9c7dd-494">Endast de fält som inte har en anpassad språk analyzer ange kan användas.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-494">Only fields that don't have a custom language analyzer set can be used.</span></span>

<span data-ttu-id="9c7dd-495">**Förslagsställarens-exempel**</span><span class="sxs-lookup"><span data-stu-id="9c7dd-495">**Suggester Example**</span></span>

<span data-ttu-id="9c7dd-496">En förslagsställarens är en del av hello index.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-496">A suggester is part of hello index.</span></span> <span data-ttu-id="9c7dd-497">Endast en förslagsställarens kan finnas i hello `suggesters` samling i hello aktuell version tillsammans med hello fält samlingen och `scoringProfiles`.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-497">Only one suggester can exist in hello `suggesters` collection in hello current version, alongside hello fields collection and `scoringProfiles`.</span></span>

        {
          "name": "hotels",
          "fields": [
             . . .
           ],
          "suggesters": [
            {
            "name": "sg",
            "searchMode": "analyzingInfixMatching",
            "sourceFields: ["hotelName", "category"]
            }
          ],
          "scoringProfiles": [
             . . .
          ]
        }

> [!NOTE]
> <span data-ttu-id="9c7dd-498">Om du använde hello offentliga förhandsversionen av Azure Search `suggesters` ersätter en äldre boolesk egenskap (`"suggestions": false`) som bara stöds prefix förslag för korta strängar (3-25 tecken).</span><span class="sxs-lookup"><span data-stu-id="9c7dd-498">If you used hello public preview version of Azure Search, `suggesters` replaces an older boolean property (`"suggestions": false`) that only supported prefix suggestions for short strings (3-25 characters).</span></span> <span data-ttu-id="9c7dd-499">Dess ersättare `suggesters`, stöder infix matchar som söker efter matchande villkoren hello början eller mitten hello av fältet innehåll med bättre tolerans för fel i söksträngar.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-499">Its replacement, `suggesters`, supports infix matching that finds matching terms at hello beginning or in hello middle of field content, with better tolerance for mistakes in search strings.</span></span> <span data-ttu-id="9c7dd-500">Från och med hello allmänt tillgänglig, är detta nu hello endast implementering av hello förslag API.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-500">Starting with hello generally available release, this is now hello only implementation of hello suggestions API.</span></span> <span data-ttu-id="9c7dd-501">hello äldre `suggestions` egenskap som introducerades i `api-version=2014-07-31-Preview` fortsätter toowork i den här versionen, men är inte funktionsduglig i hello `2015-02-28` eller senare versioner av Azure Search.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-501">hello older `suggestions` property that was introduced in `api-version=2014-07-31-Preview` continues toowork in that version, but is not operational in hello `2015-02-28` or later versions of Azure Search.</span></span>
> 
> 

<a name="UpdateIndex"></a>

## <a name="update-index"></a><span data-ttu-id="9c7dd-502">Uppdatera Index</span><span class="sxs-lookup"><span data-stu-id="9c7dd-502">Update Index</span></span>
<span data-ttu-id="9c7dd-503">Du kan uppdatera ett befintligt index i Azure Search med hjälp av en HTTP PUT-begäran.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-503">You can update an existing index within Azure Search using an HTTP PUT request.</span></span> <span data-ttu-id="9c7dd-504">Uppdateringar kan inkludera att lägga till nya fält toohello befintligt schema, ändra alternativ för CORS och ändrar bedömningsprofil profiler.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-504">Updates can include adding new fields toohello existing schema, modifying CORS options, and modifying scoring profiles.</span></span> <span data-ttu-id="9c7dd-505">Se [lägga till bedömningen profiler](https://msdn.microsoft.com/library/azure/dn798928.aspx) för mer information.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-505">See [Add scoring Profiles](https://msdn.microsoft.com/library/azure/dn798928.aspx) for more information.</span></span> <span data-ttu-id="9c7dd-506">Du kan ange hello namnet på hello index tooupdate på hello URI-begäran:</span><span class="sxs-lookup"><span data-stu-id="9c7dd-506">You specify hello name of hello index tooupdate on hello request URI:</span></span>

    PUT https://[search service url]/indexes/[index name]?api-version=[api-version]
    Content-Type: application/json
    api-key: [admin key]

<span data-ttu-id="9c7dd-507">**Viktigt:** stöd för index schemauppdateringar är begränsad toooperations som inte kräver återskapa hello sökindex.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-507">**Important:** Support for index schema updates is limited toooperations that don't require rebuilding hello search index.</span></span> <span data-ttu-id="9c7dd-508">Schema-uppdateringar som kräver omindexering, till exempel ändring av fälttyp, stöds inte för närvarande.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-508">Any schema updates that would require re-indexing, such as changing field types, are not currently supported.</span></span> <span data-ttu-id="9c7dd-509">Nya fält läggas när som helst, även om befintliga fält inte kan ändras eller tas bort.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-509">New fields can be added at any time, although existing fields cannot be changed or deleted.</span></span> <span data-ttu-id="9c7dd-510">hello samma sak gäller för`suggesters`.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-510">hello same applies too`suggesters`.</span></span> <span data-ttu-id="9c7dd-511">Går att lägga till nya fält tooa förslagsställarens på hello hello tidsfälten läggs men fält kan inte tas bort från `suggesters` och befintliga fält kan inte läggas till för`suggesters`.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-511">New fields may be added tooa suggester at hello time hello fields are added, but fields cannot be removed from `suggesters` and existing fields cannot be added too`suggesters`.</span></span>

<span data-ttu-id="9c7dd-512">När du lägger till ett nytt fält tooan index har alla befintliga dokument i hello index automatiskt ett null-värde för fältet.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-512">When adding a new field tooan index, all existing documents in hello index will automatically have a null value for that field.</span></span> <span data-ttu-id="9c7dd-513">Inga ytterligare lagringsutrymme används förrän nya dokument har lagts till toohello index.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-513">No additional storage space will be consumed until new documents are added toohello index.</span></span>

<span data-ttu-id="9c7dd-514">**Förfrågan**</span><span class="sxs-lookup"><span data-stu-id="9c7dd-514">**Request**</span></span>

<span data-ttu-id="9c7dd-515">HTTPS krävs för alla tjänstbegäranden.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-515">HTTPS is required for all service requests.</span></span> <span data-ttu-id="9c7dd-516">Hej **uppdatera Index** begäran har skapats med hjälp av HTTP PUT.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-516">hello **Update Index** request is constructed using HTTP PUT.</span></span> <span data-ttu-id="9c7dd-517">Med PUT är hello Indexnamnet en del av hello-URL.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-517">With PUT, hello index name is part of hello URL.</span></span> <span data-ttu-id="9c7dd-518">Om det inte finns hello index skapas.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-518">If hello index doesn't exist, it is created.</span></span> <span data-ttu-id="9c7dd-519">Om det redan finns hello index är uppdaterade toohello ny definition.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-519">If hello index already exists, it is updated toohello new definition.</span></span>

<span data-ttu-id="9c7dd-520">hello Indexnamn måste vara gemener, börja med en bokstav eller siffra, har inga snedstreck eller punkter och vara mindre än 128 tecken.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-520">hello index name must be lower case, start with a letter or number, have no slashes or dots, and be less than 128 characters.</span></span> <span data-ttu-id="9c7dd-521">När du har startat hello Indexnamnet med en bokstav eller siffra kan hello resten av hello namn innehålla en bokstav, antal och tankstreck, så länge hello streck inte är i följd.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-521">After starting hello index name with a letter or number, hello rest of hello name can include any letter, number and dashes, as long as hello dashes are not consecutive.</span></span>

<span data-ttu-id="9c7dd-522">`api-version=[string]`(krävs).</span><span class="sxs-lookup"><span data-stu-id="9c7dd-522">`api-version=[string]` (required).</span></span> <span data-ttu-id="9c7dd-523">hello förhandsversionen är `api-version=2015-02-28-Preview`.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-523">hello preview version is `api-version=2015-02-28-Preview`.</span></span> <span data-ttu-id="9c7dd-524">Se [Sök Versionhantering](http://msdn.microsoft.com/library/azure/dn864560.aspx) information och alternativa versioner.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-524">See [Search Service Versioning](http://msdn.microsoft.com/library/azure/dn864560.aspx) for details and alternative versions.</span></span>

<span data-ttu-id="9c7dd-525">**Huvuden för begäran**</span><span class="sxs-lookup"><span data-stu-id="9c7dd-525">**Request Headers**</span></span>

<span data-ttu-id="9c7dd-526">hello följande lista beskriver hello nödvändiga och valfria begärandehuvuden.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-526">hello following list describes hello required and optional request headers.</span></span>

* <span data-ttu-id="9c7dd-527">`Content-Type`: Krävs.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-527">`Content-Type`: Required.</span></span> <span data-ttu-id="9c7dd-528">Ställ in för`application/json`</span><span class="sxs-lookup"><span data-stu-id="9c7dd-528">Set this too`application/json`</span></span>
* <span data-ttu-id="9c7dd-529">`api-key`: Krävs.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-529">`api-key`: Required.</span></span> <span data-ttu-id="9c7dd-530">Hej `api-key` är används tooauthenticate hello begäran tooyour Search-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-530">hello `api-key` is used tooauthenticate hello request tooyour Search service.</span></span> <span data-ttu-id="9c7dd-531">Det är ett strängvärde, unika tooyour service.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-531">It is a string value, unique tooyour service.</span></span> <span data-ttu-id="9c7dd-532">Hej **uppdatera Index** begäran måste innehålla en `api-key` huvud ange tooyour admin-nyckel (som skillnad från tooa Frågenyckeln).</span><span class="sxs-lookup"><span data-stu-id="9c7dd-532">hello **Update Index** request must include an `api-key` header set tooyour admin key (as opposed tooa query key).</span></span>

<span data-ttu-id="9c7dd-533">Du måste också hello service name tooconstruct hello begärd URL.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-533">You will also need hello service name tooconstruct hello request URL.</span></span> <span data-ttu-id="9c7dd-534">Du kan hämta hello tjänstnamn och `api-key` från instrumentpanelen service i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-534">You can get hello service name and `api-key` from your service dashboard in hello Azure Portal.</span></span> <span data-ttu-id="9c7dd-535">Se [skapa en Azure Search-tjänst i hello portal](search-create-service-portal.md) sidan navigering hjälp.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-535">See [Create an Azure Search service in hello portal](search-create-service-portal.md) for page navigation help.</span></span>

<span data-ttu-id="9c7dd-536">**Begäran brödtext Syntax**</span><span class="sxs-lookup"><span data-stu-id="9c7dd-536">**Request Body Syntax**</span></span>

<span data-ttu-id="9c7dd-537">När du uppdaterar ett befintligt index, hello brödtext måste innehålla hello ursprungliga schemadefinition plus hello nya fält som du lägger till, samt hello ändras bedömningsprofil profiler, suggesters och alternativ för CORS, om sådana finns.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-537">When updating an existing index, hello body must include hello original schema definition, plus hello new fields you are adding, as well as hello modified scoring profiles, suggesters and CORS options, if any.</span></span> <span data-ttu-id="9c7dd-538">Om du inte ändrar hello bedömningsprofil profiler och alternativ för CORS, måste du inkludera hello original från när hello indexet har skapats.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-538">If you are not modifying hello scoring profiles and CORS options, you must include hello originals from when hello index was created.</span></span> <span data-ttu-id="9c7dd-539">I allmänhet hello bästa mönstret toouse för uppdateringar är tooretrieve hello indexdefinitionen med GET, ändra den och sedan uppdatera med PUT.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-539">In general hello best pattern toouse for updates is tooretrieve hello index definition with a GET, modify it, then update it with PUT.</span></span>

<span data-ttu-id="9c7dd-540">hello-schemasyntaxen används ett index återges här toocreate i informationssyfte.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-540">hello schema syntax used toocreate an index is reproduced here for convenience.</span></span> <span data-ttu-id="9c7dd-541">Se [Create Index](#CreateIndex) för mer information.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-541">See [Create Index](#CreateIndex) for more details.</span></span>

    {
      "name": (optional) "name_of_index",
      "fields": [
        {
          "name": "name_of_field",
          "type": "Edm.String | Collection(Edm.String) | Edm.Int32 | Edm.Int64 | Edm.Double | Edm.Boolean | Edm.DateTimeOffset | Edm.GeographyPoint",
          "searchable": true (default where applicable) | false (only Edm.String and Collection(Edm.String) fields can be searchable),
          "filterable": true (default) | false,
          "sortable": true (default where applicable) | false (Collection(Edm.String) fields cannot be sortable),
          "facetable": true (default where applicable) | false (Edm.GeographyPoint fields cannot be facetable),
          "key": true | false (default, only Edm.String fields can be keys),
          "retrievable": true (default) | false, 
          "analyzer": "name of hello analyzer used for search and indexing", (only if 'searchAnalyzer' and 'indexAnalyzer' are not set)
          "searchAnalyzer": "name of hello search analyzer", (only if 'indexAnalyzer' is set and 'analyzer' is not set)
          "indexAnalyzer": "name of hello indexing analyzer" (only if 'searchAnalyzer' is set and 'analyzer' is not set)
        }
      ],
      "suggesters": [
        {
          "name": "name of suggester",
          "searchMode": "analyzingInfixMatching" (other modes may be added in hello future),
          "sourceFields": ["field1", "field2", ...]
        }
      ],
      "scoringProfiles": [
        {
          "name": "name of scoring profile",
          "text": (optional, only applies toosearchable fields) {
            "weights": {
              "searchable_field_name": relative_weight_value (positive numbers),
              ...
            }
          },
          "functions": (optional) [
            {
              "type": "magnitude | freshness | distance | tag",
              "boost": # (positive number used as multiplier for raw score != 1),
              "fieldName": "...",
              "interpolation": "constant | linear (default) | quadratic | logarithmic",
              "magnitude": {
                "boostingRangeStart": #,
                "boostingRangeEnd": #,
                "constantBoostBeyondRange": true | false (default)
              },
              "freshness": {
                "boostingDuration": "..." (value representing timespan leading toonow over which boosting occurs)
              },
              "distance": {
                "referencePointParameter": "...", (parameter toobe passed in queries toouse as reference location, see "scoringParameter" for syntax details)
                "boostingDistance": # (hello distance in kilometers from hello reference location where hello boosting range ends)
              },
              "tag": {
                "tagsParameter": "..." (parameter toobe passed in queries toospecify list of tags toocompare against target field, see "scoringParameter" for syntax details)
              }
            }
          ],
          "functionAggregation": (optional, applies only when functions are specified)
            "sum (default) | average | minimum | maximum | firstMatching"
        }
      ],
      "analyzers":(optional)[ ... ],
      "charFilters":(optional)[ ... ],
      "tokenizers":(optional)[ ... ],
      "tokenFilters":(optional)[ ... ],
      "defaultScoringProfile": (optional) "...",
      "corsOptions": (optional) {
        "allowedOrigins": ["*"] | ["origin_1", "origin_2", ...],
        "maxAgeInSeconds": (optional) max_age_in_seconds (non-negative integer)
      }
    }


<span data-ttu-id="9c7dd-542">**Svar**</span><span class="sxs-lookup"><span data-stu-id="9c7dd-542">**Response**</span></span>

<span data-ttu-id="9c7dd-543">För en lyckad begäran ”: 204 saknar innehåll”.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-543">For a successful request: "204 No Content".</span></span>

<span data-ttu-id="9c7dd-544">Som standard är hello svarstexten tom.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-544">By default hello response body will be empty.</span></span> <span data-ttu-id="9c7dd-545">Emellertid, om hello `Prefer` begärandehuvudet har angetts för`return=representation`, hello svarstexten innehåller hello JSON för hello indexdefinitionen som har uppdaterats.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-545">However, if hello `Prefer` request header is set too`return=representation`, hello response body will contain hello JSON for hello index definition that was updated.</span></span> <span data-ttu-id="9c7dd-546">I det här fallet hello lyckade statuskoden ska vara ”200 OK”.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-546">In this case, hello success status code will be "200 OK".</span></span>

<span data-ttu-id="9c7dd-547">**Uppdaterar indexdefinitionen med anpassade analyzers**</span><span class="sxs-lookup"><span data-stu-id="9c7dd-547">**Updating index definition with custom analyzers**</span></span>

<span data-ttu-id="9c7dd-548">När en analyzer, en tokenizer, ett token filter eller ett char-filter har definierats kan inte ändras.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-548">Once an analyzer, a tokenizer, a token filter or a char filter is defined, it cannot be modified.</span></span> <span data-ttu-id="9c7dd-549">Nya kan läggas till tooan befintligt index om hello `allowIndexDowntime` flaggan anges tootrue i hello index uppdateringsbegäran:</span><span class="sxs-lookup"><span data-stu-id="9c7dd-549">New ones can be added tooan existing index only if hello `allowIndexDowntime` flag is set tootrue in hello index update request:</span></span> 

`PUT https://[search service name].search.windows.net/indexes/[index name]?api-version=[api-version]&allowIndexDowntime=true`

<span data-ttu-id="9c7dd-550">Observera att den här åtgärden placerar ditt index offline för minst ett par sekunder, och din indexering och fråga begär toofail.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-550">Note that this operation will put your index offline for at least a few seconds, causing your indexing and query requests toofail.</span></span> <span data-ttu-id="9c7dd-551">Prestanda- och skrivbehörighet tillgängligheten för hello indexet kan vara försämrad i flera minuter när hello indexet uppdateras eller längre för mycket stora index.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-551">Performance and write availability of hello index can be impaired for several minutes after hello index is updated, or longer for very large indexes.</span></span>

<a name="ListIndexes"></a>

## <a name="list-indexes"></a><span data-ttu-id="9c7dd-552">Lista över index</span><span class="sxs-lookup"><span data-stu-id="9c7dd-552">List Indexes</span></span>
<span data-ttu-id="9c7dd-553">Hej **listan index** åtgärden returnerar en lista över hello index för närvarande i Azure Search-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-553">hello **List Indexes** operation returns a list of hello indexes currently in your Azure Search service.</span></span>

    GET https://[service name].search.windows.net/indexes?api-version=[api-version]
    api-key: [admin key]

<span data-ttu-id="9c7dd-554">**Förfrågan**</span><span class="sxs-lookup"><span data-stu-id="9c7dd-554">**Request**</span></span>

<span data-ttu-id="9c7dd-555">HTTPS krävs för alla tjänstbegäranden.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-555">HTTPS is required for all service requests.</span></span> <span data-ttu-id="9c7dd-556">Hej **listan index** begäran kan konstrueras med hello GET-metoden.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-556">hello **List Indexes** request can be constructed using hello GET method.</span></span>

<span data-ttu-id="9c7dd-557">`api-version=[string]`(krävs).</span><span class="sxs-lookup"><span data-stu-id="9c7dd-557">`api-version=[string]` (required).</span></span> <span data-ttu-id="9c7dd-558">hello förhandsversionen är `api-version=2015-02-28-Preview`.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-558">hello preview version is `api-version=2015-02-28-Preview`.</span></span> <span data-ttu-id="9c7dd-559">Se [Sök Versionhantering](http://msdn.microsoft.com/library/azure/dn864560.aspx) information och alternativa versioner.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-559">See [Search Service Versioning](http://msdn.microsoft.com/library/azure/dn864560.aspx) for details and alternative versions.</span></span>

<span data-ttu-id="9c7dd-560">**Huvuden för begäran**</span><span class="sxs-lookup"><span data-stu-id="9c7dd-560">**Request Headers**</span></span>

<span data-ttu-id="9c7dd-561">hello följande lista beskriver hello nödvändiga och valfria begärandehuvuden.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-561">hello following list describes hello required and optional request headers.</span></span>

* <span data-ttu-id="9c7dd-562">`api-key`: Krävs.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-562">`api-key`: Required.</span></span> <span data-ttu-id="9c7dd-563">Hej `api-key` är används tooauthenticate hello begäran tooyour Search-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-563">hello `api-key` is used tooauthenticate hello request tooyour Search service.</span></span> <span data-ttu-id="9c7dd-564">Det är ett strängvärde, unika tooyour service.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-564">It is a string value, unique tooyour service.</span></span> <span data-ttu-id="9c7dd-565">Hej **listan index** begäran måste innehålla en `api-key` set tooan admin-nyckel (som skillnad från tooa Frågenyckeln).</span><span class="sxs-lookup"><span data-stu-id="9c7dd-565">hello **List Indexes** request must include an `api-key` set tooan admin key (as opposed tooa query key).</span></span>

<span data-ttu-id="9c7dd-566">Du måste också hello service name tooconstruct hello begärd URL.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-566">You will also need hello service name tooconstruct hello request URL.</span></span> <span data-ttu-id="9c7dd-567">Du kan hämta hello tjänstnamn och `api-key` från instrumentpanelen service i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-567">You can get hello service name and `api-key` from your service dashboard in hello Azure Portal.</span></span> <span data-ttu-id="9c7dd-568">Se [skapa en Azure Search-tjänst i hello portal](search-create-service-portal.md) sidan navigering hjälp.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-568">See [Create an Azure Search service in hello portal](search-create-service-portal.md) for page navigation help.</span></span>

<span data-ttu-id="9c7dd-569">**Begärandetexten**</span><span class="sxs-lookup"><span data-stu-id="9c7dd-569">**Request Body**</span></span>

<span data-ttu-id="9c7dd-570">Ingen.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-570">None.</span></span>

<span data-ttu-id="9c7dd-571">**Svar**</span><span class="sxs-lookup"><span data-stu-id="9c7dd-571">**Response**</span></span>

<span data-ttu-id="9c7dd-572">Statuskod: 200 OK returneras för ett lyckat svar.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-572">Status Code: 200 OK is returned for a successful response.</span></span>

<span data-ttu-id="9c7dd-573">Här är ett exempel svarstexten:</span><span class="sxs-lookup"><span data-stu-id="9c7dd-573">Here is an example response body:</span></span>

    {
      "value": [
        {
          "name": "Books",
          "fields": [
            {"name": "ISBN", ...},
            ...
          ]
        },
        {
          "name": "Games",
          ...
        },
        ...
      ]
    }

<span data-ttu-id="9c7dd-574">Observera att du kan filtrera hello svar ned toojust hello egenskaper som du är intresserad av.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-574">Note that you can filter hello response down toojust hello properties you're interested in.</span></span> <span data-ttu-id="9c7dd-575">Till exempel om du vill att endast en lista över namn på index använda hello OData `$select` frågealternativet:</span><span class="sxs-lookup"><span data-stu-id="9c7dd-575">For example, if you want only a list of index names, use hello OData `$select` query option:</span></span>

    GET /indexes?api-version=2015-02-28-Preview&$select=name

<span data-ttu-id="9c7dd-576">I det här fallet visas hello svar från hello över exempel på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="9c7dd-576">In this case, hello response from hello above example would appear as follows:</span></span>

    {
      "value": [
        {"name": "Books"},
        {"name": "Games"},
        ...
      ]
    }

<span data-ttu-id="9c7dd-577">Det här är en bra metod toosave bandbredd om du har många index i din söktjänst.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-577">This is a useful technique toosave bandwidth if you have a lot of indexes in your Search service.</span></span>

<a name="GetIndex"></a>

## <a name="get-index"></a><span data-ttu-id="9c7dd-578">Hämta Index</span><span class="sxs-lookup"><span data-stu-id="9c7dd-578">Get Index</span></span>
<span data-ttu-id="9c7dd-579">Hej **hämta Index** åtgärden hämtar hello indexdefinitionen från Azure Search.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-579">hello **Get Index** operation gets hello index definition from Azure Search.</span></span>

    GET https://[service name].search.windows.net/indexes/[index name]?api-version=[api-version]
    api-key: [admin key]

<span data-ttu-id="9c7dd-580">**Förfrågan**</span><span class="sxs-lookup"><span data-stu-id="9c7dd-580">**Request**</span></span>

<span data-ttu-id="9c7dd-581">HTTPS krävs för tjänstbegäranden.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-581">HTTPS is required for service requests.</span></span> <span data-ttu-id="9c7dd-582">Hej **hämta Index** begäran kan konstrueras med hello GET-metoden.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-582">hello **Get Index** request can be constructed using hello GET method.</span></span>

<span data-ttu-id="9c7dd-583">Hej [index name] i hello Begärd URI anger vilka index tooreturn från hello GetSchema.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-583">hello [index name] in hello request URI specifies which index tooreturn from hello indexes collection.</span></span>

<span data-ttu-id="9c7dd-584">`api-version=[string]`(krävs).</span><span class="sxs-lookup"><span data-stu-id="9c7dd-584">`api-version=[string]` (required).</span></span> <span data-ttu-id="9c7dd-585">hello förhandsversionen är `api-version=2015-02-28-Preview`.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-585">hello preview version is `api-version=2015-02-28-Preview`.</span></span> <span data-ttu-id="9c7dd-586">Se [Sök Versionhantering](http://msdn.microsoft.com/library/azure/dn864560.aspx) information och alternativa versioner.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-586">See [Search Service Versioning](http://msdn.microsoft.com/library/azure/dn864560.aspx) for details and alternative versions.</span></span>

<span data-ttu-id="9c7dd-587">**Huvuden för begäran**</span><span class="sxs-lookup"><span data-stu-id="9c7dd-587">**Request Headers**</span></span>

<span data-ttu-id="9c7dd-588">hello följande lista beskriver hello nödvändiga och valfria begärandehuvuden.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-588">hello following list describes hello required and optional request headers.</span></span>

* <span data-ttu-id="9c7dd-589">`api-key`: hello `api-key` är används tooauthenticate hello begäran tooyour Search-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-589">`api-key`: hello `api-key` is used tooauthenticate hello request tooyour Search service.</span></span> <span data-ttu-id="9c7dd-590">Det är ett strängvärde, unika tooyour service.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-590">It is a string value, unique tooyour service.</span></span> <span data-ttu-id="9c7dd-591">Hej **hämta Index** begäran måste innehålla en `api-key` set tooan admin-nyckel (som skillnad från tooa Frågenyckeln).</span><span class="sxs-lookup"><span data-stu-id="9c7dd-591">hello **Get Index** request must include an `api-key` set tooan admin key (as opposed tooa query key).</span></span>

<span data-ttu-id="9c7dd-592">Du måste också hello service name tooconstruct hello begärd URL.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-592">You will also need hello service name tooconstruct hello request URL.</span></span> <span data-ttu-id="9c7dd-593">Du kan hämta hello tjänstnamn och `api-key` från instrumentpanelen service i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-593">You can get hello service name and `api-key` from your service dashboard in hello Azure Portal.</span></span> <span data-ttu-id="9c7dd-594">Se [skapa en Azure Search-tjänst i hello portal](search-create-service-portal.md) sidan navigering hjälp.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-594">See [Create an Azure Search service in hello portal](search-create-service-portal.md) for page navigation help.</span></span>

<span data-ttu-id="9c7dd-595">**Begärandetexten**</span><span class="sxs-lookup"><span data-stu-id="9c7dd-595">**Request Body**</span></span>

<span data-ttu-id="9c7dd-596">Ingen.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-596">None.</span></span>

<span data-ttu-id="9c7dd-597">**Svar**</span><span class="sxs-lookup"><span data-stu-id="9c7dd-597">**Response**</span></span>

<span data-ttu-id="9c7dd-598">Statuskod: 200 OK returneras för ett lyckat svar.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-598">Status Code: 200 OK is returned for a successful response.</span></span>

<span data-ttu-id="9c7dd-599">Se hello exempel JSON i [att skapa och uppdatera ett Index](#CreateUpdateIndexExample) ett exempel på hello svar nyttolast.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-599">See hello example JSON in [Creating and Updating an Index](#CreateUpdateIndexExample) for an example of hello response payload.</span></span>

<a name="DeleteIndex"></a>

## <a name="delete-index"></a><span data-ttu-id="9c7dd-600">Ta bort indexet</span><span class="sxs-lookup"><span data-stu-id="9c7dd-600">Delete Index</span></span>
<span data-ttu-id="9c7dd-601">Hej **ta bort indexet** åtgärden tar bort ett index och associerade dokument från din Azure Search-tjänst.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-601">hello **Delete Index** operation removes an index and associated documents from your Azure Search service.</span></span> <span data-ttu-id="9c7dd-602">Du kan hämta hello Indexnamnet från hello service instrumentpanelen i hello Azure-portalen, eller från hello API.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-602">You can get hello index name from hello service dashboard in hello Azure portal, or from hello API.</span></span> <span data-ttu-id="9c7dd-603">Se [listan index](#ListIndexes) mer information.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-603">See [List Indexes](#ListIndexes) for details.</span></span>

    DELETE https://[service name].search.windows.net/indexes/[index name]?api-version=[api-version]
    api-key: [admin key]

<span data-ttu-id="9c7dd-604">**Förfrågan**</span><span class="sxs-lookup"><span data-stu-id="9c7dd-604">**Request**</span></span>

<span data-ttu-id="9c7dd-605">HTTPS krävs för tjänstbegäranden.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-605">HTTPS is required for service requests.</span></span> <span data-ttu-id="9c7dd-606">Hej **ta bort indexet** begäran kan konstrueras med hello DELETE-metoden.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-606">hello **Delete Index** request can be constructed using hello DELETE method.</span></span>

<span data-ttu-id="9c7dd-607">Hej [index name] i hello Begärd URI anger vilka index toodelete från hello GetSchema.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-607">hello [index name] in hello request URI specifies which index toodelete from hello indexes collection.</span></span>

<span data-ttu-id="9c7dd-608">`api-version=[string]`(krävs).</span><span class="sxs-lookup"><span data-stu-id="9c7dd-608">`api-version=[string]` (required).</span></span> <span data-ttu-id="9c7dd-609">hello förhandsversionen är `api-version=2015-02-28-Preview`.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-609">hello preview version is `api-version=2015-02-28-Preview`.</span></span> <span data-ttu-id="9c7dd-610">Se [Sök Versionhantering](http://msdn.microsoft.com/library/azure/dn864560.aspx) information och alternativa versioner.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-610">See [Search Service Versioning](http://msdn.microsoft.com/library/azure/dn864560.aspx) for details and alternative versions.</span></span>

<span data-ttu-id="9c7dd-611">**Huvuden för begäran**</span><span class="sxs-lookup"><span data-stu-id="9c7dd-611">**Request Headers**</span></span>

<span data-ttu-id="9c7dd-612">hello följande lista beskriver hello nödvändiga och valfria begärandehuvuden.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-612">hello following list describes hello required and optional request headers.</span></span>

* <span data-ttu-id="9c7dd-613">`api-key`: Krävs.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-613">`api-key`: Required.</span></span> <span data-ttu-id="9c7dd-614">Hej `api-key` är används tooauthenticate hello begäran tooyour Search-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-614">hello `api-key` is used tooauthenticate hello request tooyour Search service.</span></span> <span data-ttu-id="9c7dd-615">Det är ett strängvärde, unika tooyour tjänst-URL.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-615">It is a string value, unique tooyour service URL.</span></span> <span data-ttu-id="9c7dd-616">Hej **ta bort indexet** begäran måste innehålla en `api-key` huvud ange tooyour admin-nyckel (som skillnad från tooa Frågenyckeln).</span><span class="sxs-lookup"><span data-stu-id="9c7dd-616">hello **Delete Index** request must include an `api-key` header set tooyour admin key (as opposed tooa query key).</span></span>

<span data-ttu-id="9c7dd-617">Du måste också hello service name tooconstruct hello begärd URL.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-617">You will also need hello service name tooconstruct hello request URL.</span></span> <span data-ttu-id="9c7dd-618">Du kan hämta hello tjänstnamn och `api-key` från instrumentpanelen service i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-618">You can get hello service name and `api-key` from your service dashboard in hello Azure Portal.</span></span> <span data-ttu-id="9c7dd-619">Se [skapa en Azure Search-tjänst i hello portal](search-create-service-portal.md) sidan navigering hjälp.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-619">See [Create an Azure Search service in hello portal](search-create-service-portal.md) for page navigation help.</span></span>

<span data-ttu-id="9c7dd-620">**Begärandetexten**</span><span class="sxs-lookup"><span data-stu-id="9c7dd-620">**Request Body**</span></span>

<span data-ttu-id="9c7dd-621">Ingen.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-621">None.</span></span>

<span data-ttu-id="9c7dd-622">**Svar**</span><span class="sxs-lookup"><span data-stu-id="9c7dd-622">**Response**</span></span>

<span data-ttu-id="9c7dd-623">Statuskod: 204 Nej innehåll returneras för ett lyckat svar.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-623">Status Code: 204 No Content is returned for a successful response.</span></span>

<a name="GetIndexStats"></a>

## <a name="get-index-statistics"></a><span data-ttu-id="9c7dd-624">Få indexstatistik</span><span class="sxs-lookup"><span data-stu-id="9c7dd-624">Get Index Statistics</span></span>
<span data-ttu-id="9c7dd-625">Hej **hämta indexstatistik** åtgärd returnerar från Azure Search dokumentet antalet hello aktuellt index plus lagringskvoten.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-625">hello **Get Index Statistics** operation returns from Azure Search a document count for hello current index, plus storage usage.</span></span>

    GET https://[service name].search.windows.net/indexes/[index name]/stats?api-version=[api-version]
    api-key: [admin key]

> [!NOTE]
> <span data-ttu-id="9c7dd-626">Statistik om dokumentet antal och lagringsstorlek har samlats in några minuters mellanrum, inte i realtid.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-626">Statistics on document count and storage size are collected every few minutes, not in real time.</span></span> <span data-ttu-id="9c7dd-627">Hello statistik som returneras av denna API kan därför inte återspegla ändringar på grund av de senaste Indexeringsåtgärder.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-627">Therefore, hello statistics returned by this API may not reflect changes caused by recent indexing operations.</span></span>
> 
> 

<span data-ttu-id="9c7dd-628">**Förfrågan**</span><span class="sxs-lookup"><span data-stu-id="9c7dd-628">**Request**</span></span>

<span data-ttu-id="9c7dd-629">HTTPS krävs för alla begäranden som tjänster.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-629">HTTPS is required for all services requests.</span></span> <span data-ttu-id="9c7dd-630">Hej **hämta indexstatistik** begäran kan konstrueras med hello GET-metoden.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-630">hello **Get Index Statistics** request can be constructed using hello GET method.</span></span>

<span data-ttu-id="9c7dd-631">Hej [index name] i hello Begärd URI visar hello tooreturn indexstatistik för hello angivet index.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-631">hello [index name] in hello request URI tells hello service tooreturn index statistics for hello specified index.</span></span>

<span data-ttu-id="9c7dd-632">`api-version=[string]`(krävs).</span><span class="sxs-lookup"><span data-stu-id="9c7dd-632">`api-version=[string]` (required).</span></span> <span data-ttu-id="9c7dd-633">hello förhandsversionen är `api-version=2015-02-28-Preview`.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-633">hello preview version is `api-version=2015-02-28-Preview`.</span></span> <span data-ttu-id="9c7dd-634">Se [Sök Versionhantering](http://msdn.microsoft.com/library/azure/dn864560.aspx) information och alternativa versioner.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-634">See [Search Service Versioning](http://msdn.microsoft.com/library/azure/dn864560.aspx) for details and alternative versions.</span></span>

<span data-ttu-id="9c7dd-635">**Huvuden för begäran**</span><span class="sxs-lookup"><span data-stu-id="9c7dd-635">**Request Headers**</span></span>

<span data-ttu-id="9c7dd-636">hello följande lista beskriver hello nödvändiga och valfria begärandehuvuden.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-636">hello following list describes hello required and optional request headers.</span></span>

* <span data-ttu-id="9c7dd-637">`api-key`: hello `api-key` är används tooauthenticate hello begäran tooyour Search-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-637">`api-key`: hello `api-key` is used tooauthenticate hello request tooyour Search service.</span></span> <span data-ttu-id="9c7dd-638">Det är ett strängvärde, unika tooyour service.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-638">It is a string value, unique tooyour service.</span></span> <span data-ttu-id="9c7dd-639">Hej **hämta indexstatistik** begäran måste innehålla en `api-key` set tooan admin-nyckel (som skillnad från tooa Frågenyckeln).</span><span class="sxs-lookup"><span data-stu-id="9c7dd-639">hello **Get Index Statistics** request must include an `api-key` set tooan admin key (as opposed tooa query key).</span></span>

<span data-ttu-id="9c7dd-640">Du måste också hello service name tooconstruct hello begärd URL.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-640">You will also need hello service name tooconstruct hello request URL.</span></span> <span data-ttu-id="9c7dd-641">Du kan hämta hello tjänstnamn och `api-key` från instrumentpanelen service i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-641">You can get hello service name and `api-key` from your service dashboard in hello Azure Portal.</span></span> <span data-ttu-id="9c7dd-642">Se [skapa en Azure Search-tjänst i hello portal](search-create-service-portal.md) sidan navigering hjälp.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-642">See [Create an Azure Search service in hello portal](search-create-service-portal.md) for page navigation help.</span></span>

<span data-ttu-id="9c7dd-643">**Begärandetexten**</span><span class="sxs-lookup"><span data-stu-id="9c7dd-643">**Request Body**</span></span>

<span data-ttu-id="9c7dd-644">Ingen.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-644">None.</span></span>

<span data-ttu-id="9c7dd-645">**Svar**</span><span class="sxs-lookup"><span data-stu-id="9c7dd-645">**Response**</span></span>

<span data-ttu-id="9c7dd-646">Statuskod: 200 OK returneras för ett lyckat svar.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-646">Status Code: 200 OK is returned for a successful response.</span></span>

<span data-ttu-id="9c7dd-647">hello svarstexten finns i hello följande format:</span><span class="sxs-lookup"><span data-stu-id="9c7dd-647">hello response body is in hello following format:</span></span>

    {
      "documentCount": number,
      "storageSize": number (size of hello index in bytes)
    }

<a name="TestAnalyzer"></a>

## <a name="test-analyzer"></a><span data-ttu-id="9c7dd-648">Testa Analyzer</span><span class="sxs-lookup"><span data-stu-id="9c7dd-648">Test Analyzer</span></span>
<span data-ttu-id="9c7dd-649">Hej **analysera API** visar hur en analyzer delar upp text i token.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-649">hello **Analyze API** shows how an analyzer breaks text into tokens.</span></span>

    POST https://[service name].search.windows.net/indexes/[index name]/analyze?api-version=[api-version]
    Content-Type: application/json
    api-key: [admin key]

<span data-ttu-id="9c7dd-650">**Förfrågan**</span><span class="sxs-lookup"><span data-stu-id="9c7dd-650">**Request**</span></span>

<span data-ttu-id="9c7dd-651">HTTPS krävs för alla begäranden som tjänster.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-651">HTTPS is required for all services requests.</span></span> <span data-ttu-id="9c7dd-652">Hej **analysera API** begäran kan konstrueras med hello POST-metoden.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-652">hello **Analyze API** request can be constructed using hello POST method.</span></span>

<span data-ttu-id="9c7dd-653">`api-version=[string]`(krävs).</span><span class="sxs-lookup"><span data-stu-id="9c7dd-653">`api-version=[string]` (required).</span></span> <span data-ttu-id="9c7dd-654">hello förhandsversionen är `api-version=2015-02-28-Preview`.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-654">hello preview version is `api-version=2015-02-28-Preview`.</span></span> <span data-ttu-id="9c7dd-655">Se [Sök Versionhantering](http://msdn.microsoft.com/library/azure/dn864560.aspx) information och alternativa versioner.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-655">See [Search Service Versioning](http://msdn.microsoft.com/library/azure/dn864560.aspx) for details and alternative versions.</span></span>

<span data-ttu-id="9c7dd-656">**Huvuden för begäran**</span><span class="sxs-lookup"><span data-stu-id="9c7dd-656">**Request Headers**</span></span>

<span data-ttu-id="9c7dd-657">hello följande lista beskriver hello nödvändiga och valfria begärandehuvuden.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-657">hello following list describes hello required and optional request headers.</span></span>

* <span data-ttu-id="9c7dd-658">`api-key`: hello `api-key` är används tooauthenticate hello begäran tooyour Search-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-658">`api-key`: hello `api-key` is used tooauthenticate hello request tooyour Search service.</span></span> <span data-ttu-id="9c7dd-659">Det är ett strängvärde, unika tooyour service.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-659">It is a string value, unique tooyour service.</span></span> <span data-ttu-id="9c7dd-660">Hej **analysera API** begäran måste innehålla en `api-key` set tooan admin-nyckel (som skillnad från tooa Frågenyckeln).</span><span class="sxs-lookup"><span data-stu-id="9c7dd-660">hello **Analyze API** request must include an `api-key` set tooan admin key (as opposed tooa query key).</span></span>

<span data-ttu-id="9c7dd-661">Du måste också hello Indexnamnet och hello service name tooconstruct hello begärd URL.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-661">You will also need hello index name and hello service name tooconstruct hello request URL.</span></span> <span data-ttu-id="9c7dd-662">Du kan hämta hello tjänstnamn och `api-key` från instrumentpanelen service i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-662">You can get hello service name and `api-key` from your service dashboard in hello Azure Portal.</span></span> <span data-ttu-id="9c7dd-663">Se [skapa en Azure Search-tjänst i hello portal](search-create-service-portal.md) sidan navigering hjälp.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-663">See [Create an Azure Search service in hello portal](search-create-service-portal.md) for page navigation help.</span></span>

<span data-ttu-id="9c7dd-664">**Begärandetexten**</span><span class="sxs-lookup"><span data-stu-id="9c7dd-664">**Request Body**</span></span>

    {
      "text": "Text tooanalyze",
      "analyzer": "analyzer_name"
    }

<span data-ttu-id="9c7dd-665">eller</span><span class="sxs-lookup"><span data-stu-id="9c7dd-665">or</span></span>

    {
      "text": "Text tooanalyze",
      "tokenizer": "tokenizer_name",
      "tokenFilters": (optional) [ "token_filter_name" ],
      "charFilters": (optional) [ "char_filter_name" ]
    }

<span data-ttu-id="9c7dd-666">Hej `analyzer_name`, `tokenizer_name`, `token_filter_name` och `char_filter_name` behöver toobe giltiga namn på fördefinierade eller anpassade analyzers, tokenizers, token-filter och char filter för hello index.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-666">hello `analyzer_name`, `tokenizer_name`, `token_filter_name` and `char_filter_name` need toobe valid names of predefined or custom analyzers, tokenizers, token filters and char filters for hello index.</span></span> <span data-ttu-id="9c7dd-667">toolearn om hello processen lexikala analys finns [analys i Azure Search](https://aka.ms/azsanalysis).</span><span class="sxs-lookup"><span data-stu-id="9c7dd-667">toolearn more about hello process of lexical analysis see [Analysis in Azure Search](https://aka.ms/azsanalysis).</span></span>

<span data-ttu-id="9c7dd-668">**Svar**</span><span class="sxs-lookup"><span data-stu-id="9c7dd-668">**Response**</span></span>

<span data-ttu-id="9c7dd-669">Statuskod: 200 OK returneras för ett lyckat svar.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-669">Status Code: 200 OK is returned for a successful response.</span></span>

<span data-ttu-id="9c7dd-670">hello svarstexten finns i hello följande format:</span><span class="sxs-lookup"><span data-stu-id="9c7dd-670">hello response body is in hello following format:</span></span>

    {
      "tokens": [
        {
          "token": string (token),
          "startOffset": number (index of hello first character of hello token),
          "endOffset": number (index of hello last character of hello token),
          "position": number (position of hello token in hello input text)
        },
        ...
      ]
    }

<span data-ttu-id="9c7dd-671">**Analysera API-exempel**</span><span class="sxs-lookup"><span data-stu-id="9c7dd-671">**Analyze API example**</span></span>

<span data-ttu-id="9c7dd-672">**Förfrågan**</span><span class="sxs-lookup"><span data-stu-id="9c7dd-672">**Request**</span></span>

    {
      "text": "Text tooanalyze",
      "analyzer": "standard"
    }

<span data-ttu-id="9c7dd-673">**Svar**</span><span class="sxs-lookup"><span data-stu-id="9c7dd-673">**Response**</span></span>

    {
      "tokens": [
        {
          "token": "text",
          "startOffset": 0,
          "endOffset": 4,
          "position": 0
        },
        {
          "token": "to",
          "startOffset": 5,
          "endOffset": 7,
          "position": 1
        },
        {
          "token": "analyze",
          "startOffset": 8,
          "endOffset": 15,
          "position": 2
        }
      ]
    }

- - -
<a name="DocOps"></a>

## <a name="document-operations"></a><span data-ttu-id="9c7dd-674">Dokumentet åtgärder</span><span class="sxs-lookup"><span data-stu-id="9c7dd-674">Document Operations</span></span>
<span data-ttu-id="9c7dd-675">I Azure Search index lagras i molnet hello och fylls i med hjälp av JSON-dokument som du överför toohello service.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-675">In Azure Search, an index is stored in hello cloud and populated using JSON documents that you upload toohello service.</span></span> <span data-ttu-id="9c7dd-676">Alla hello-dokument som du överför utgör hello Kristi Sök data.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-676">All hello documents that you upload comprise hello corpus of your search data.</span></span> <span data-ttu-id="9c7dd-677">Dokument innehåller fält, av vilka vissa tokeniserad i söktermer som de överförs.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-677">Documents contain fields, some of which are tokenized into search terms as they are uploaded.</span></span> <span data-ttu-id="9c7dd-678">Hej `/docs` URL-segment i hello Azure Search API representerar hello samling av dokument i ett index.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-678">hello `/docs` URL segment in hello Azure Search API represents hello collection of documents in an index.</span></span> <span data-ttu-id="9c7dd-679">Alla åtgärder som utförts på hello samling, till exempel överföra sammanslagning, ta bort eller fråga dokument ske i hello kontexten för ett index, så hello URL: er för dessa åtgärder börjar alltid med `/indexes/[index name]/docs` efter ett angivet index.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-679">All operations performed on hello collection such as uploading, merging, deleting, or querying documents take place in hello context of a single index, so hello URLs for these operations will always start with `/indexes/[index name]/docs` for a given index name.</span></span>

<span data-ttu-id="9c7dd-680">Programkoden måste antingen generera JSON-dokument tooupload tooAzure Sök eller använda en [indexeraren](https://msdn.microsoft.com/library/dn946891.aspx) tooload dokument om datakällan hello Azure SQL Database eller Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-680">Your application code must either generate JSON documents tooupload tooAzure Search or you can use an [indexer](https://msdn.microsoft.com/library/dn946891.aspx) tooload documents if hello data source is either Azure SQL Database or Azure Cosmos DB.</span></span> <span data-ttu-id="9c7dd-681">Normalt fylls index i från en enda datamängd som du anger.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-681">Typically, indexes are populated from a single dataset that you provide.</span></span>

<span data-ttu-id="9c7dd-682">Du bör planera ett dokument för varje objekt som du vill toosearch ska ha.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-682">You should plan on having one document for each item that you want toosearch.</span></span> <span data-ttu-id="9c7dd-683">En film uthyrning programmet kan ha ett dokument per film, en storefront programmet kan ha ett dokument per SKU, ett online kursprogramvara program kan ha ett dokument per kursen, research företag kan ha ett dokument för varje academic papper i sin databas och så vidare.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-683">A movie rental application might have one document per movie, a storefront application might have one document per SKU, an online courseware application might have one document per course, a research firm might have one document for each academic paper in their repository, and so on.</span></span>

<span data-ttu-id="9c7dd-684">Dokument består av ett eller flera fält.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-684">Documents consist of one or more fields.</span></span> <span data-ttu-id="9c7dd-685">Fälten kan innehålla text som är tokeniserad med Azure Search in söktermer som inte tokeniserad eller icke-textvärden som kan användas i filter eller bedömningsprofil profiler.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-685">Fields can contain text that is tokenized by Azure Search into search terms, as well as non-tokenized or non-text values that can be used in filters or scoring profiles.</span></span> <span data-ttu-id="9c7dd-686">hello bestäms namn, datatyper och funktioner som stöds för varje fält av hello indexeringsschema.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-686">hello names, data types, and search features supported for each field are determined by hello index schema.</span></span> <span data-ttu-id="9c7dd-687">Ett av hello fält i varje indexeringsschema måste anges som ett ID och varje dokument måste ha ett värde för hello-fält som unikt identifierar dokumentet i hello index.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-687">One of hello fields in each index schema must be designated as an ID, and each document must have a value for hello ID field that uniquely identifies that document in hello index.</span></span> <span data-ttu-id="9c7dd-688">Alla andra dokumentfält är valfria och som standard tooa null-värde om lämnas Ospecificerad.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-688">All other document fields are optional and will default tooa null value if left unspecified.</span></span> <span data-ttu-id="9c7dd-689">Observera att null-värden inte plats i hello sökindex.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-689">Note that null values do not take up space in hello search index.</span></span>

<span data-ttu-id="9c7dd-690">Innan du kan ladda upp dokument, måste du ha skapat hello index på hello-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-690">Before you can upload documents, you must have already created hello index on hello service.</span></span> <span data-ttu-id="9c7dd-691">Se [Create Index](#CreateIndex) mer information om det här första steget.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-691">See [Create Index](#CreateIndex) for details about this first step.</span></span>

<a name="AddOrUpdateDocuments"></a>

## <a name="add-update-or-delete-documents"></a><span data-ttu-id="9c7dd-692">Lägga till, uppdatera, eller ta bort dokument</span><span class="sxs-lookup"><span data-stu-id="9c7dd-692">Add, Update, or Delete Documents</span></span>
<span data-ttu-id="9c7dd-693">Du kan ladda upp, merge, merge eller överför eller ta bort dokument från ett specificerat index med hjälp av HTTP POST.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-693">You can upload, merge, merge-or-upload or delete documents from a specified index using HTTP POST.</span></span> <span data-ttu-id="9c7dd-694">För många uppdateringar rekommenderas massbearbetning av dokument (too1000 dokument per batch) eller ca 16 MB per batch.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-694">For large numbers of updates, batching of documents (up too1000 documents per batch or about 16 MB per batch) is recommended.</span></span>

    POST https://[service name].search.windows.net/indexes/[index name]/docs/index?api-version=[api-version]
    Content-Type: application/json
    api-key: [admin key]

<span data-ttu-id="9c7dd-695">**Förfrågan**</span><span class="sxs-lookup"><span data-stu-id="9c7dd-695">**Request**</span></span>

<span data-ttu-id="9c7dd-696">HTTPS krävs för alla tjänstbegäranden.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-696">HTTPS is required for all service requests.</span></span> <span data-ttu-id="9c7dd-697">Du kan ladda upp, merge, merge eller överför eller ta bort dokument från ett specificerat index med hjälp av HTTP POST.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-697">You can upload, merge, merge-or-upload or delete documents from a specified index using HTTP POST.</span></span>

<span data-ttu-id="9c7dd-698">hello begäran URI innehåller [Indexnamnet] anger vilka index toopost dokument.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-698">hello request URI includes [index name], specifying which index toopost documents.</span></span> <span data-ttu-id="9c7dd-699">Du kan bara publicera dokument tooone index i taget.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-699">You can only post documents tooone index at a time.</span></span>

<span data-ttu-id="9c7dd-700">`api-version=[string]`(krävs).</span><span class="sxs-lookup"><span data-stu-id="9c7dd-700">`api-version=[string]` (required).</span></span> <span data-ttu-id="9c7dd-701">hello förhandsversionen är `api-version=2015-02-28-Preview`.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-701">hello preview version is `api-version=2015-02-28-Preview`.</span></span> <span data-ttu-id="9c7dd-702">Se [Sök Versionhantering](http://msdn.microsoft.com/library/azure/dn864560.aspx) information och alternativa versioner.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-702">See [Search Service Versioning](http://msdn.microsoft.com/library/azure/dn864560.aspx) for details and alternative versions.</span></span>

<span data-ttu-id="9c7dd-703">**Huvuden för begäran**</span><span class="sxs-lookup"><span data-stu-id="9c7dd-703">**Request Headers**</span></span>

<span data-ttu-id="9c7dd-704">hello följande lista beskriver hello nödvändiga och valfria begärandehuvuden.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-704">hello following list describes hello required and optional request headers.</span></span>

* <span data-ttu-id="9c7dd-705">`Content-Type`: Krävs.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-705">`Content-Type`: Required.</span></span> <span data-ttu-id="9c7dd-706">Ställ in för`application/json`</span><span class="sxs-lookup"><span data-stu-id="9c7dd-706">Set this too`application/json`</span></span>
* <span data-ttu-id="9c7dd-707">`api-key`: Krävs.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-707">`api-key`: Required.</span></span> <span data-ttu-id="9c7dd-708">Hej `api-key` är används tooauthenticate hello begäran tooyour Search-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-708">hello `api-key` is used tooauthenticate hello request tooyour Search service.</span></span> <span data-ttu-id="9c7dd-709">Det är ett strängvärde, unika tooyour service.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-709">It is a string value, unique tooyour service.</span></span> <span data-ttu-id="9c7dd-710">Hej **lägga till dokument** begäran måste innehålla en `api-key` huvud ange tooyour admin-nyckel (som skillnad från tooa Frågenyckeln).</span><span class="sxs-lookup"><span data-stu-id="9c7dd-710">hello **Add Documents** request must include an `api-key` header set tooyour admin key (as opposed tooa query key).</span></span>

<span data-ttu-id="9c7dd-711">Du måste också hello service name tooconstruct hello begärd URL.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-711">You will also need hello service name tooconstruct hello request URL.</span></span> <span data-ttu-id="9c7dd-712">Du kan hämta hello tjänstnamn och `api-key` från instrumentpanelen service i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-712">You can get hello service name and `api-key` from your service dashboard in hello Azure Portal.</span></span> <span data-ttu-id="9c7dd-713">Se [skapa en Azure Search-tjänst i hello portal](search-create-service-portal.md) sidan navigering hjälp.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-713">See [Create an Azure Search service in hello portal](search-create-service-portal.md) for page navigation help.</span></span>

<span data-ttu-id="9c7dd-714">**Begärandetexten**</span><span class="sxs-lookup"><span data-stu-id="9c7dd-714">**Request Body**</span></span>

<span data-ttu-id="9c7dd-715">hello hello-begäran innehåller en eller flera dokument toobe indexeras.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-715">hello body of hello request contains one or more documents toobe indexed.</span></span> <span data-ttu-id="9c7dd-716">Dokument identifieras med en unik nyckel.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-716">Documents are identified by a unique key.</span></span> <span data-ttu-id="9c7dd-717">Varje dokument som är associerat med en åtgärd: Överför dokument, mergeOrUpload eller ta bort.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-717">Each document is associated with an action: upload, merge, mergeOrUpload or delete.</span></span> <span data-ttu-id="9c7dd-718">Överför förfrågningar måste innehålla hello dokumentdata som en uppsättning nyckel/värde-par.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-718">Upload requests must include hello document data as a set of key/value pairs.</span></span>

    {
      "value": [
        {
          "@search.action": "upload (default) | merge | mergeOrUpload | delete",
          "key_field_name": "unique_key_of_document", (key/value pair for key field from index schema)
          "field_name": field_value (key/value pairs matching index schema)
            ...
        },
        ...
      ]
    }

> [!NOTE]
> <span data-ttu-id="9c7dd-719">Dokumentet nycklar kan endast innehålla bokstäver, siffror, bindestreck (”-”), understreck (”_”) och likhetstecken (”=”).</span><span class="sxs-lookup"><span data-stu-id="9c7dd-719">Document keys can only contain letters, numbers, dashes ("-"), underscores ("_"), and equal signs ("=").</span></span> <span data-ttu-id="9c7dd-720">Mer information finns i [namngivningsregler](https://msdn.microsoft.com/library/azure/dn857353.aspx).</span><span class="sxs-lookup"><span data-stu-id="9c7dd-720">For more details, see [Naming rules](https://msdn.microsoft.com/library/azure/dn857353.aspx).</span></span>
> 
> 

<span data-ttu-id="9c7dd-721">**Dokumentåtgärder**</span><span class="sxs-lookup"><span data-stu-id="9c7dd-721">**Document Actions**</span></span>

* <span data-ttu-id="9c7dd-722">`upload`: En åtgärd för överföringen är liknande tooan ”upsert” där hello dokumentet infogas om det är nytt och uppdatera/ersättas om den finns.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-722">`upload`: An upload action is similar tooan "upsert" where hello document will be inserted if it is new and updated/replaced if it exists.</span></span> <span data-ttu-id="9c7dd-723">Observera att alla fält ersätts i hello update fall.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-723">Note that all fields are replaced in hello update case.</span></span>
* <span data-ttu-id="9c7dd-724">`merge`: Sammanslagning uppdateringar ett befintligt dokument med hello angivna fält.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-724">`merge`: Merge updates an existing document with hello specified fields.</span></span> <span data-ttu-id="9c7dd-725">Hello merge misslyckas om hello dokumentet inte finns.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-725">If hello document doesn't exist, hello merge will fail.</span></span> <span data-ttu-id="9c7dd-726">Alla fält som du anger i en merge ersätter hello befintliga fält i hello.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-726">Any field you specify in a merge will replace hello existing field in hello document.</span></span> <span data-ttu-id="9c7dd-727">Detta gäller även fält av typen `Collection(Edm.String)`.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-727">This includes fields of type `Collection(Edm.String)`.</span></span> <span data-ttu-id="9c7dd-728">Till exempel om hello dokumentet innehåller fältet ”taggar” med värdet `["budget"]` och du utföra en koppling med värdet `["economy", "pool"]` ”taggar” hello slutliga värdet för fältet hello ”taggar” blir `["economy", "pool"]`.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-728">For example, if hello document contains a field "tags" with value `["budget"]` and you execute a merge with value `["economy", "pool"]` for "tags", hello final value of hello "tags" field will be `["economy", "pool"]`.</span></span> <span data-ttu-id="9c7dd-729">Kommer det att **inte** vara `["budget", "economy", "pool"]`.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-729">It will **not** be `["budget", "economy", "pool"]`.</span></span>
* <span data-ttu-id="9c7dd-730">`mergeOrUpload`: fungerar som `merge` om ett dokument med hello anges nyckeln redan finns i hello index.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-730">`mergeOrUpload`: behaves like `merge` if a document with hello given key already exists in hello index.</span></span> <span data-ttu-id="9c7dd-731">Om hello dokument inte finns det fungerar som `upload` med ett nytt dokument.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-731">If hello document does not exist it behaves like `upload` with a new document.</span></span>
* <span data-ttu-id="9c7dd-732">`delete`: Ta bort tar bort hello angivna dokumentet från hello index.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-732">`delete`: Delete removes hello specified document from hello index.</span></span> <span data-ttu-id="9c7dd-733">Observera att alla fält som du anger i en `delete` åtgärden än hello nyckelfältet kommer att ignoreras.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-733">Note that any fields you specify in a `delete` operation other than hello key field will be ignored.</span></span> <span data-ttu-id="9c7dd-734">Om du vill tooremove ett fält från ett dokument, använda `merge` enkelt och i stället ange hello fält explicit för`null`.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-734">If you want tooremove an individual field from a document, use `merge` instead and simply set hello field explicitly too`null`.</span></span>

<span data-ttu-id="9c7dd-735">**Svar**</span><span class="sxs-lookup"><span data-stu-id="9c7dd-735">**Response**</span></span>

<span data-ttu-id="9c7dd-736">Statuskoden 200 returneras (OK) för ett lyckat svar, vilket innebär att alla objekt indexerade.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-736">Status code 200 (OK) is returned for a successful response, meaning that all items have been successfully indexed.</span></span> <span data-ttu-id="9c7dd-737">Detta anges av hello `status` egenskapen som anges tootrue för alla objekt, samt hello `statusCode` egenskapen som anges tooeither 201 (för nyligen uppladdade dokument) eller 200 (för sammanfogade eller borttagna dokument):</span><span class="sxs-lookup"><span data-stu-id="9c7dd-737">This is indicated by hello `status` property being set tootrue for all items, as well as hello `statusCode` property being set tooeither 201 (for newly uploaded documents) or 200 (for merged or deleted documents):</span></span>

    {
      "value": [
        {
          "key": "unique_key_of_new_document",
          "status": true,
          "errorMessage": null,
          "statusCode": 201
        },
        {
          "key": "unique_key_of_merged_document",
          "status": true,
          "errorMessage": null,
          "statusCode": 200
        },
        {
          "key": "unique_key_of_deleted_document",
          "status": true,
          "errorMessage": null,
          "statusCode": 200
        }
      ]
    }  

<span data-ttu-id="9c7dd-738">Statuskoden 207 (flera Status) returnerades när minst ett objekt inte har har indexerats.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-738">Status code 207 (Multi-Status) is returned when at least one item was not successfully indexed.</span></span> <span data-ttu-id="9c7dd-739">Objekt som inte har indexerats har hello `status` fältet Ange toofalse.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-739">Items that have not been indexed have hello `status` field set toofalse.</span></span> <span data-ttu-id="9c7dd-740">Hej `errorMessage` och `statusCode` egenskaper visar hello indexering fel hello orsak:</span><span class="sxs-lookup"><span data-stu-id="9c7dd-740">hello `errorMessage` and `statusCode` properties will indicate hello reason for hello indexing error:</span></span>

    {
      "value": [
        {
          "key": "unique_key_of_document_1",
          "status": false,
          "errorMessage": "hello search service is too busy tooprocess this document. Please try again later.",
          "statusCode": 503
        },
        {
          "key": "unique_key_of_document_2",
          "status": false,
          "errorMessage": "Document not found.",
          "statusCode": 404
        },
        {
          "key": "unique_key_of_document_3",
          "status": false,
          "errorMessage": "Index is temporarily unavailable because it was updated with hello 'allowIndexDowntime' flag set too'true'. Please try again later.",
          "statusCode": 422
        }
      ]
    }  

<span data-ttu-id="9c7dd-741">hello i den följande tabellen beskrivs hello olika per dokument statuskoder som kan returneras hello svar.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-741">hello following table explains hello various per-document status codes that can be returned in hello response.</span></span> <span data-ttu-id="9c7dd-742">Observera att vissa anger problem med hello begäran, medan andra indikerar tillfälliga fel.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-742">Note that some indicate problems with hello request itself, while others indicate temporary error conditions.</span></span> <span data-ttu-id="9c7dd-743">hello senare du kan försöka efter en stund.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-743">hello latter you should retry after a delay.</span></span>

<table style="font-size:12">
    <tr>
        <th><span data-ttu-id="9c7dd-744">Statuskod</span><span class="sxs-lookup"><span data-stu-id="9c7dd-744">Status code</span></span></th>
        <th><span data-ttu-id="9c7dd-745">Betydelse</span><span class="sxs-lookup"><span data-stu-id="9c7dd-745">Meaning</span></span></th>
        <th><span data-ttu-id="9c7dd-746">Återförsökbart</span><span class="sxs-lookup"><span data-stu-id="9c7dd-746">Retryable</span></span></th>
        <th><span data-ttu-id="9c7dd-747">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="9c7dd-747">Notes</span></span></th>
    </tr>
    <tr>
        <td><span data-ttu-id="9c7dd-748">200</span><span class="sxs-lookup"><span data-stu-id="9c7dd-748">200</span></span></td>
        <td><span data-ttu-id="9c7dd-749">Dokumentet har har ändrats eller tagits bort.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-749">Document was successfully modified or deleted.</span></span></td>
        <td><span data-ttu-id="9c7dd-750">Saknas</span><span class="sxs-lookup"><span data-stu-id="9c7dd-750">n/a</span></span></td>
        <td><span data-ttu-id="9c7dd-751">Ta bort är <a href="https://en.wikipedia.org/wiki/Idempotence">idempotent</a>.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-751">Delete operations are <a href="https://en.wikipedia.org/wiki/Idempotence">idempotent</a>.</span></span> <span data-ttu-id="9c7dd-752">Det vill säga även om en Dokumentnyckel inte finns i hello index, leder försöker en borttagningsåtgärd med nyckeln statuskod 200.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-752">That is, even if a document key does not exist in hello index, attempting a delete operation with that key will result in a 200 status code.</span></span></td>
    </tr>
    <tr>
        <td><span data-ttu-id="9c7dd-753">201</span><span class="sxs-lookup"><span data-stu-id="9c7dd-753">201</span></span></td>
        <td><span data-ttu-id="9c7dd-754">Dokumentet har skapats.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-754">Document was successfully created.</span></span></td>
        <td><span data-ttu-id="9c7dd-755">Saknas</span><span class="sxs-lookup"><span data-stu-id="9c7dd-755">n/a</span></span></td>
        <td></td>
    </tr>
    <tr>
        <td><span data-ttu-id="9c7dd-756">400</span><span class="sxs-lookup"><span data-stu-id="9c7dd-756">400</span></span></td>
        <td><span data-ttu-id="9c7dd-757">Ett fel uppstod i hello-dokumentet som hindrade den från att indexeras.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-757">There was an error in hello document that prevented it from being indexed.</span></span></td>
        <td><span data-ttu-id="9c7dd-758">Nej</span><span class="sxs-lookup"><span data-stu-id="9c7dd-758">No</span></span></td>
        <td><span data-ttu-id="9c7dd-759">hello felmeddelande hello svar visar fel med hello dokument.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-759">hello error message in hello response will indicate what is wrong with hello document.</span></span></td>
    </tr>
    <tr>
        <td><span data-ttu-id="9c7dd-760">404</span><span class="sxs-lookup"><span data-stu-id="9c7dd-760">404</span></span></td>
        <td><span data-ttu-id="9c7dd-761">hello dokument kan inte kopplas eftersom hello angivna nyckeln inte finns i hello index.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-761">hello document could not be merged because hello given key doesn't exist in hello index.</span></span></td>
        <td><span data-ttu-id="9c7dd-762">Nej</span><span class="sxs-lookup"><span data-stu-id="9c7dd-762">No</span></span></td>
        <td><span data-ttu-id="9c7dd-763">Det här felet uppstår inte för överföringar, eftersom de skapar nya dokument och det uppstår inte för borttagningar eftersom de är <a href="https://en.wikipedia.org/wiki/Idempotence">idempotent</a>.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-763">This error does not occur for uploads since they create new documents, and it does not occur for deletes because they are <a href="https://en.wikipedia.org/wiki/Idempotence">idempotent</a>.</span></span></td>
    </tr>
    <tr>
        <td><span data-ttu-id="9c7dd-764">409</span><span class="sxs-lookup"><span data-stu-id="9c7dd-764">409</span></span></td>
        <td><span data-ttu-id="9c7dd-765">En versionskonflikt upptäcktes vid försök tooindex ett dokument.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-765">A version conflict was detected when attempting tooindex a document.</span></span></td>
        <td><span data-ttu-id="9c7dd-766">Ja</span><span class="sxs-lookup"><span data-stu-id="9c7dd-766">Yes</span></span></td>
        <td><span data-ttu-id="9c7dd-767">Detta kan inträffa när du försöker tooindex hello samma dokumentet mer än en gång samtidigt.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-767">This can happen when you're trying tooindex hello same document more than once concurrently.</span></span></td>
    </tr>
    <tr>
        <td><span data-ttu-id="9c7dd-768">422</span><span class="sxs-lookup"><span data-stu-id="9c7dd-768">422</span></span></td>
        <td><span data-ttu-id="9c7dd-769">hello index är inte tillgänglig för tillfället eftersom den uppdaterades med hello allowIndexDowntime flaggan set too'true'.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-769">hello index is temporarily unavailable because it was updated with hello 'allowIndexDowntime' flag set too'true'.</span></span></td>
        <td><span data-ttu-id="9c7dd-770">Ja</span><span class="sxs-lookup"><span data-stu-id="9c7dd-770">Yes</span></span></td>
        <td></td>
    </tr>
    <tr>
        <td><span data-ttu-id="9c7dd-771">503</span><span class="sxs-lookup"><span data-stu-id="9c7dd-771">503</span></span></td>
        <td><span data-ttu-id="9c7dd-772">Din söktjänst är inte tillgänglig för tillfället, möjligen på grund av tooheavy belastningen.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-772">Your search service is temporarily unavailable, possibly due tooheavy load.</span></span></td>
        <td><span data-ttu-id="9c7dd-773">Ja</span><span class="sxs-lookup"><span data-stu-id="9c7dd-773">Yes</span></span></td>
        <td><span data-ttu-id="9c7dd-774">Koden ska vänta innan du försöker igen i det här fallet eller riskerar du att förlänga hello-tjänsten inte finns.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-774">Your code should wait before retrying in this case or you risk prolonging hello service unavailability.</span></span></td>
    </tr>
</table> 

<span data-ttu-id="9c7dd-775">**Obs**: om din klientkod ofta påträffar ett 207 svar, en möjlig orsak är att hello systemet är under belastning.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-775">**Note**: If your client code frequently encounters a 207 response, one possible reason is that hello system is under load.</span></span> <span data-ttu-id="9c7dd-776">Du kan kontrollera detta genom att kontrollera hello `statusCode` -egenskapen för 503.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-776">You can confirm this by checking hello `statusCode` property for 503.</span></span> <span data-ttu-id="9c7dd-777">Om så är fallet hello rekommenderar vi ***begränsning indexering begäranden***.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-777">If this is hello case, we recommend ***throttling indexing requests***.</span></span> <span data-ttu-id="9c7dd-778">Annars, om indexering trafik inte subside hello system kunde starta avvisar alla begäranden med 503-fel.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-778">Otherwise, if indexing traffic doesn't subside, hello system could start rejecting all requests with 503 errors.</span></span>

<span data-ttu-id="9c7dd-779">Statuskoden 429 anger att du har överskridit kvoten för hello antalet dokument per index.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-779">Status code 429 indicates that you have exceeded your quota on hello number of documents per index.</span></span> <span data-ttu-id="9c7dd-780">Du måste skapa ett nytt index eller uppgradera för högre kapacitetsgränser.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-780">You must either create a new index or upgrade for higher capacity limits.</span></span>

<span data-ttu-id="9c7dd-781">**Exempel:**</span><span class="sxs-lookup"><span data-stu-id="9c7dd-781">**Example:**</span></span>

    {
      "value": [
        {
          "@search.action": "upload",
          "hotelId": "1",
          "baseRate": 199.0,
          "description": "Best hotel in town",
          "description_fr": "Meilleur hôtel en ville",
          "hotelName": "Fancy Stay",
          "category": "Luxury",
          "tags": ["pool", "view", "wifi", "concierge"],
          "parkingIncluded": false,
          "smokingAllowed": false,
          "lastRenovationDate": "2010-06-27T00:00:00Z",
          "rating": 5,
          "location": { "type": "Point", "coordinates": [-122.131577, 47.678581] }
        },
        {
          "@search.action": "upload",
          "hotelId": "2",
          "baseRate": 79.99,
          "description": "Cheapest hotel in town",
          "description_fr": "Hôtel le moins cher en ville",
          "hotelName": "Roach Motel",
          "category": "Budget",
          "tags": ["motel", "budget"],
          "parkingIncluded": true,
          "smokingAllowed": true,
          "lastRenovationDate": "1982-04-28T00:00:00Z",
          "rating": 1,
          "location": { "type": "Point", "coordinates": [-122.131577, 49.678581] }
        },
        {
          "@search.action": "merge",
          "hotelId": "3",
          "baseRate": 279.99,
          "description": "Surprisingly expensive",
          "lastRenovationDate": null
        },
        {
          "@search.action": "delete",
          "hotelId": "4"
        }
      ]
    }
- - -
<a name="SearchDocs"></a>

## <a name="search-documents"></a><span data-ttu-id="9c7dd-782">Sök igenom dokument</span><span class="sxs-lookup"><span data-stu-id="9c7dd-782">Search Documents</span></span>
<span data-ttu-id="9c7dd-783">En **Sök** åtgärden utfärdas som en GET eller POST-begäran och anger parametrarna som ger hello kriterier för att välja matchande dokument.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-783">A **Search** operation is issued as a GET or POST request and specifies parameters that give hello criteria for selecting matching documents.</span></span>

    GET https://[service name].search.windows.net/indexes/[index name]/docs?[query parameters]
    api-key: [admin or query key]

    POST https://[service name].search.windows.net/indexes/[index name]/docs/search?api-version=[api-version]
    Content-Type: application/json
    api-key: [admin or query key]

<span data-ttu-id="9c7dd-784">**När toouse bokför i stället för att hämta**</span><span class="sxs-lookup"><span data-stu-id="9c7dd-784">**When toouse POST instead of GET**</span></span>

<span data-ttu-id="9c7dd-785">När du använder HTTP GET toocall hello **Sök** API, behöver du toobe medveten om att hello hello URL: en inte kan vara längre än 8 KB.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-785">When you use HTTP GET toocall hello **Search** API, you need toobe aware that hello length of hello request URL cannot exceed 8 KB.</span></span> <span data-ttu-id="9c7dd-786">Detta är vanligtvis tillräckligt för de flesta program.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-786">This is usually enough for most applications.</span></span> <span data-ttu-id="9c7dd-787">Vissa program kan dock ge mycket stora frågor eller OData filteruttryck.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-787">However, some applications produce very large queries or OData filter expressions.</span></span> <span data-ttu-id="9c7dd-788">För dessa program är med hjälp av HTTP POST ett bättre alternativ eftersom det tillåter större filter och frågor än GET.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-788">For these applications, using HTTP POST is a better choice because it allows larger filters and queries than GET.</span></span> <span data-ttu-id="9c7dd-789">Med POST, hello antal termer eller satser i en fråga är hello begränsa faktor inte hello storleken för hello rå fråga eftersom hello begäran storleksgräns för POST är ungefär 16 MB.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-789">With POST, hello number of terms or clauses in a query is hello limiting factor, not hello size of hello raw query since hello request size limit for POST is approximately 16 MB.</span></span>

> [!NOTE]
> <span data-ttu-id="9c7dd-790">Även om storleksgränsen för hello POST-begäran är mycket stora får inte sökningar och filteruttryck vara godtyckligt komplexa.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-790">Even though hello POST request size limit is very large, search queries and filter expressions cannot be arbitrarily complex.</span></span> <span data-ttu-id="9c7dd-791">Se [Lucene frågesyntaxen](https://msdn.microsoft.com/library/mt589323.aspx) och [OData-uttryckssyntax](https://msdn.microsoft.com/library/dn798921.aspx) för mer information om begränsningar för sökning fråge- och komplexitet.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-791">See [Lucene query syntax](https://msdn.microsoft.com/library/mt589323.aspx) and [OData expression syntax](https://msdn.microsoft.com/library/dn798921.aspx) for more information about search query and filter complexity limitations.</span></span>
> 
> 

<span data-ttu-id="9c7dd-792">**Förfrågan**</span><span class="sxs-lookup"><span data-stu-id="9c7dd-792">**Request**</span></span>

<span data-ttu-id="9c7dd-793">HTTPS krävs för tjänstbegäranden.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-793">HTTPS is required for service requests.</span></span> <span data-ttu-id="9c7dd-794">Hej **Sök** begäran kan konstrueras med hello GET eller POST-metoderna.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-794">hello **Search** request can be constructed using hello GET or POST methods.</span></span>

<span data-ttu-id="9c7dd-795">URI-begäran hello anger vilka index tooquery, för alla dokument som matchar hello parametrar.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-795">hello request URI specifies which index tooquery, for all documents that match hello parameters.</span></span> <span data-ttu-id="9c7dd-796">Parametrar har angetts på hello frågesträngen hello gäller GET-begäranden och i hello begäran text i hello skiftläget för POST-begäranden.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-796">Parameters are specified on hello query string in hello case of GET requests, and in hello request body in hello case of POST requests.</span></span>

<span data-ttu-id="9c7dd-797">Ett bra tips när du skapar GET-begäranden, spara för[URL-koda](https://msdn.microsoft.com/library/system.uri.escapedatastring.aspx) specifik fråga parametrar när du anropar hello REST API direkt.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-797">As a best practice when creating GET requests, remember too[URL-encode](https://msdn.microsoft.com/library/system.uri.escapedatastring.aspx) specific query parameters when calling hello REST API directly.</span></span> <span data-ttu-id="9c7dd-798">För **Sök** åtgärder, detta omfattar:</span><span class="sxs-lookup"><span data-stu-id="9c7dd-798">For **Search** operations, this includes:</span></span>

* `$filter`
* `facet`
* `highlightPreTag`
* `highlightPostTag`
* `search`
* `moreLikeThis`

<span data-ttu-id="9c7dd-799">URL-kodning rekommenderas endast på hello ovan Frågeparametrar.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-799">URL encoding is only recommended on hello above query parameters.</span></span> <span data-ttu-id="9c7dd-800">Om du av misstag URL-koda hello hela frågesträng (allt efter hello?) bryts begäranden.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-800">If you inadvertently URL-encode hello entire query string (everything after hello ?), requests will break.</span></span>

<span data-ttu-id="9c7dd-801">Dessutom krävs URL-kodning bara när du anropar hello REST API: et direkt med komma.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-801">Also, URL encoding is only necessary when calling hello REST API directly using GET.</span></span> <span data-ttu-id="9c7dd-802">Ingen URL-kodning krävs när du anropar **Sök** med POST, eller när du använder hello [.NET-klientbibliotek](https://msdn.microsoft.com/library/dn951165.aspx), som hanterar URL-kodning för dig.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-802">No URL encoding is necessary when calling **Search** using POST, or when using hello [.NET client library](https://msdn.microsoft.com/library/dn951165.aspx), which handles URL encoding for you.</span></span>

<span data-ttu-id="9c7dd-803"><a name="SearchQueryParameters"></a>
**Frågeparametrar**</span><span class="sxs-lookup"><span data-stu-id="9c7dd-803"><a name="SearchQueryParameters"></a>
**Query Parameters**</span></span>

<span data-ttu-id="9c7dd-804">**Sök** accepterar flera parametrar som tillhandahåller frågevillkor och även ange sökbeteendet.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-804">**Search** accepts several parameters that provide query criteria and also specify search behavior.</span></span> <span data-ttu-id="9c7dd-805">Du anger dessa parametrar i hello URL: en frågesträng vid anrop av **Sök** via GET och som JSON-egenskaper i hello begärandetexten vid anrop av **Sök** via POST.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-805">You provide these parameters in hello URL query string when calling **Search** via GET, and as JSON properties in hello request body when calling **Search** via POST.</span></span> <span data-ttu-id="9c7dd-806">hello syntax för vissa parametrar är något annorlunda mellan GET och POST.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-806">hello syntax for some parameters is slightly different between GET and POST.</span></span> <span data-ttu-id="9c7dd-807">Skillnaderna beskrivs i tillämpliga fall nedan:</span><span class="sxs-lookup"><span data-stu-id="9c7dd-807">These differences are noted as applicable below:</span></span>

<span data-ttu-id="9c7dd-808">`search=[string]`(valfritt) – hello text toosearch för.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-808">`search=[string]` (optional) - hello text toosearch for.</span></span> <span data-ttu-id="9c7dd-809">Alla `searchable` fält söks som standard om inte `searchFields` har angetts.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-809">All `searchable` fields are searched by default unless `searchFields` is specified.</span></span> <span data-ttu-id="9c7dd-810">När du söker `searchable` fält, hello söktext själva tokeniserad så att flera villkor kan avgränsas med blanksteg (till exempel: `search=hello world`).</span><span class="sxs-lookup"><span data-stu-id="9c7dd-810">When searching `searchable` fields, hello search text itself is tokenized, so multiple terms can be separated by white space (for example: `search=hello world`).</span></span> <span data-ttu-id="9c7dd-811">toomatch något villkor använda `*` (det kan vara användbart för booleskt filterfrågor).</span><span class="sxs-lookup"><span data-stu-id="9c7dd-811">toomatch any term, use `*` (this can be useful for boolean filter queries).</span></span> <span data-ttu-id="9c7dd-812">Om du utesluter den här parametern har samma effekt som du anger för hello`*`.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-812">Omitting this parameter has hello same effect as setting it too`*`.</span></span> <span data-ttu-id="9c7dd-813">Se [enkel frågesyntaxen](https://msdn.microsoft.com/library/dn798920.aspx) för närmare information om hello söksyntax.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-813">See [Simple Query Syntax](https://msdn.microsoft.com/library/dn798920.aspx) for specifics on hello search syntax.</span></span>

* <span data-ttu-id="9c7dd-814">**Obs**: hello resultat kan ibland vara konstigt när frågor skickas över `searchable` fält.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-814">**Note**: hello results can sometimes be surprising when querying over `searchable` fields.</span></span> <span data-ttu-id="9c7dd-815">Hej tokenizer innehåller logik toohandle fall vanliga tooEnglish text som apostrofer, kommatecken siffror osv. Till exempel `search=123,456` matchar en enskild term 123,456 i stället för enskilda villkor hello 123 och 456, eftersom kommatecken används som avgränsare tusen för stora tal på engelska.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-815">hello tokenizer includes logic toohandle cases common tooEnglish text like apostrophes, commas in numbers, etc. For example, `search=123,456` will match a single term 123,456 rather than hello individual terms 123 and 456, since commas are used as thousand-separators for large numbers in English.</span></span> <span data-ttu-id="9c7dd-816">Därför bör du använda blanksteg i stället för skiljetecken tooseparate villkoren i hello `search` parameter.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-816">For this reason, we recommend using white space rather than punctuation tooseparate terms in hello `search` parameter.</span></span>

<span data-ttu-id="9c7dd-817">`searchMode=any|all`(valfritt, standardvärden för`any`) – oavsett om någon eller samtliga hello söktermer måste matchas i ordning toocount hello dokumentet som en matchning.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-817">`searchMode=any|all` (optional, defaults too`any`) - whether any or all of hello search terms must be matched in order toocount hello document as a match.</span></span>

<span data-ttu-id="9c7dd-818">`searchFields=[string]`(valfritt) – hello lista över kommaavgränsade fältet namn toosearch för hello angiven text.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-818">`searchFields=[string]` (optional) - hello list of comma-separated field names toosearch for hello specified text.</span></span> <span data-ttu-id="9c7dd-819">Målfält måste vara markerad som `searchable`.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-819">Target fields must be marked as `searchable`.</span></span>

<span data-ttu-id="9c7dd-820">`queryType=simple|full`(valfritt, standardvärden för`simple`) – när set för ”enkla” söktext tolkas med hjälp av en enkel frågespråk som möjliggör symboler som, + * och ””.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-820">`queryType=simple|full` (optional, defaults too`simple`) - when set too"simple" search text is interpreted using a simple query language that allows for symbols such as +, * and "".</span></span> <span data-ttu-id="9c7dd-821">Frågor utvärderas över alla sökbara fält (fält som anges i `searchFields`) i varje dokument som standard.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-821">Queries are evaluated across all searchable fields (or fields indicated in `searchFields`) in each document by default.</span></span> <span data-ttu-id="9c7dd-822">När hello frågetypen har angetts för`full` söktext tolkas med hello Lucene frågespråk som tillåter specifika fält och viktat sökningar.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-822">When hello query type is set too`full` search text is interpreted using hello Lucene query language which allows field-specific and weighted searches.</span></span> <span data-ttu-id="9c7dd-823">Se [enkel frågesyntaxen](https://msdn.microsoft.com/library/dn798920.aspx) och [Lucene frågesyntaxen](https://msdn.microsoft.com/library/mt589323.aspx) för specifika på hello Sök syntax.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-823">See [Simple Query Syntax](https://msdn.microsoft.com/library/dn798920.aspx) and [Lucene Query Syntax](https://msdn.microsoft.com/library/mt589323.aspx) for specifics on hello search syntaxes.</span></span> 

> [!NOTE]
> <span data-ttu-id="9c7dd-824">Intervallet sökning i hello Lucene frågespråket inte stöds för $filter som ger liknande funktionalitet.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-824">Range search in hello Lucene query language is not supported in favor of $filter which offers similar functionality.</span></span>
> 
> 

<span data-ttu-id="9c7dd-825">`moreLikeThis=[key]`(valfritt) **Viktigt:** den här funktionen är endast tillgänglig i `2015-02-28-Preview`.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-825">`moreLikeThis=[key]` (optional) **Important:** This feature is only available in `2015-02-28-Preview`.</span></span> <span data-ttu-id="9c7dd-826">Det här alternativet kan inte användas i en fråga som innehåller hello text search parametern `search=[string]`.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-826">This option cannot be used in a query that contains hello text search parameter, `search=[string]`.</span></span> <span data-ttu-id="9c7dd-827">Hej `moreLikeThis` parametern hittar dokument som är liknande toohello dokument som anges av hello Dokumentnyckel.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-827">hello `moreLikeThis` parameter finds documents that are similar toohello document specified by hello document key.</span></span> <span data-ttu-id="9c7dd-828">När en sökning har begärts med `moreLikeThis`, en lista över söktermer genereras baserat på hello frekvens och sällsynt villkoren i hello källdokument.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-828">When a search request is made with `moreLikeThis`, a list of search terms is generated based on hello frequency and rarity of terms in hello source document.</span></span> <span data-ttu-id="9c7dd-829">Dessa villkor är och sedan använda toomake hello begäran.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-829">Those terms are then used toomake hello request.</span></span> <span data-ttu-id="9c7dd-830">Som standard hello innehållet i alla `searchable` fält anses om `searchFields` är används toorestrict vilka fält som genomsöks.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-830">By default, hello contents of all `searchable` fields are considered unless `searchFields` is used toorestrict which fields are searched.</span></span>  

<span data-ttu-id="9c7dd-831">`$skip=#`(valfritt) – hello antalet Sök resulterar tooskip; Får inte vara större än 100 000.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-831">`$skip=#` (optional) - hello number of search results tooskip; Cannot be greater than 100,000.</span></span> <span data-ttu-id="9c7dd-832">Om du behöver tooscan dokument i följd men kan inte använda `$skip` på grund av toothis begränsning, Överväg att använda `$orderby` på en helt sorterade nyckel och `$filter` med en fråga i stället.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-832">If you need tooscan documents in sequence but cannot use `$skip` due toothis limitation, consider using `$orderby` on a totally-ordered key and `$filter` with a range query instead.</span></span>

> [!NOTE]
> <span data-ttu-id="9c7dd-833">När du anropar **Sök** med efter den här parametern heter `skip` i stället för `$skip`.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-833">When calling **Search** using POST, this parameter is named `skip` instead of `$skip`.</span></span>
> 
> 

<span data-ttu-id="9c7dd-834">`$top=#`(valfritt) – hello antalet Sök resulterar tooretrieve.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-834">`$top=#` (optional) - hello number of search results tooretrieve.</span></span> <span data-ttu-id="9c7dd-835">Detta kan användas tillsammans med `$skip` tooimplement klientsidan sidindelning i sökresultaten.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-835">This can be used in conjunction with `$skip` tooimplement client-side paging of search results.</span></span>

> [!NOTE]
> <span data-ttu-id="9c7dd-836">När du anropar **Sök** med efter den här parametern heter `top` i stället för `$top`.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-836">When calling **Search** using POST, this parameter is named `top` instead of `$top`.</span></span>
> 
> 

<span data-ttu-id="9c7dd-837">`$count=true|false`(valfritt, standardvärden för`false`) – anger om toofetch hello Totalt antal resultat.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-837">`$count=true|false` (optional, defaults too`false`) - Specifies whether toofetch hello total count of results.</span></span> <span data-ttu-id="9c7dd-838">Detta är hello antal alla dokument som matchar hello `search` och `$filter` parametrar, ignorerar `$top` och `$skip`.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-838">This is hello count of all documents that match hello `search` and `$filter` parameters, ignoring `$top` and `$skip`.</span></span> <span data-ttu-id="9c7dd-839">Om det här värdet för`true` kan påverka prestanda.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-839">Setting this value too`true` may have a performance impact.</span></span> <span data-ttu-id="9c7dd-840">Observera att returnerade hello antalet är en uppskattning.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-840">Note that hello count returned is an approximation.</span></span>

> [!NOTE]
> <span data-ttu-id="9c7dd-841">När du anropar **Sök** med efter den här parametern heter `count` i stället för `$count`.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-841">When calling **Search** using POST, this parameter is named `count` instead of `$count`.</span></span>
> 
> 

<span data-ttu-id="9c7dd-842">`$orderby=[string]`(valfritt) – en lista över uttryck avgränsade med kommatecken toosort hello resultat av.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-842">`$orderby=[string]` (optional) - A list of comma-separated expressions toosort hello results by.</span></span> <span data-ttu-id="9c7dd-843">Varje uttryck kan vara ett fältnamn eller ett anrop toohello `geo.distance()` funktion.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-843">Each expression can be either a field name or a call toohello `geo.distance()` function.</span></span> <span data-ttu-id="9c7dd-844">Varje uttryck kan följas av `asc` tooindicated stigande ordning och `desc` tooindicate fallande.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-844">Each expression can be followed by `asc` tooindicated ascending, and `desc` tooindicate descending.</span></span> <span data-ttu-id="9c7dd-845">hello standard är stigande ordning.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-845">hello default is ascending order.</span></span> <span data-ttu-id="9c7dd-846">TIES delas av hello matchar resultat av dokument.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-846">Ties will be broken by hello match scores of documents.</span></span> <span data-ttu-id="9c7dd-847">Om ingen `$orderby` anges hello standardsorteringsordningen är fallande dokumentet matchar poäng.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-847">If no `$orderby` is specified, hello default sort order is descending by document match score.</span></span> <span data-ttu-id="9c7dd-848">Det finns en gräns på 32-satser för `$orderby`.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-848">There is a limit of 32 clauses for `$orderby`.</span></span>

> [!NOTE]
> <span data-ttu-id="9c7dd-849">När du anropar **Sök** med efter den här parametern heter `orderby` i stället för `$orderby`.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-849">When calling **Search** using POST, this parameter is named `orderby` instead of `$orderby`.</span></span>
> 
> 

<span data-ttu-id="9c7dd-850">`$select=[string]`(valfritt) – en lista över kommaavgränsade fält tooretrieve.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-850">`$select=[string]` (optional) - A list of comma-separated fields tooretrieve.</span></span> <span data-ttu-id="9c7dd-851">Om inget anges ingår alla fält som har markerats som hämtas i hello schemat.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-851">If unspecified, all fields marked as retrievable in hello schema are included.</span></span> <span data-ttu-id="9c7dd-852">Du kan också explicit begära alla fält genom att ange den här parametern för`*`.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-852">You can also explicitly request all fields by setting this parameter too`*`.</span></span>

> [!NOTE]
> <span data-ttu-id="9c7dd-853">När du anropar **Sök** med efter den här parametern heter `select` i stället för `$select`.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-853">When calling **Search** using POST, this parameter is named `select` instead of `$select`.</span></span>
> 
> 

<span data-ttu-id="9c7dd-854">`facet=[string]`(noll eller fler) - en fältet toofacet av.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-854">`facet=[string]` (zero or more) - A field toofacet by.</span></span> <span data-ttu-id="9c7dd-855">Alternativt hello strängen kan innehålla parametrar toocustomize hello faceting uttryckt i CSV- `name:value` par.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-855">Optionally hello string may contain parameters toocustomize hello faceting expressed as comma-separated `name:value` pairs.</span></span> <span data-ttu-id="9c7dd-856">Giltiga parametrar finns:</span><span class="sxs-lookup"><span data-stu-id="9c7dd-856">Valid parameters are:</span></span>

* <span data-ttu-id="9c7dd-857">`count`(max antal aspekten villkor; standardvärdet är 10).</span><span class="sxs-lookup"><span data-stu-id="9c7dd-857">`count` (max number of facet terms; default is 10).</span></span> <span data-ttu-id="9c7dd-858">Det finns inget maximum men högre värden medföra en motsvarande prestandaförsämring, särskilt om hello fasetterad fältet innehåller ett stort antal unika villkor.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-858">There is no maximum, but higher values incur a corresponding performance penalty, especially if hello faceted field contains a large number of unique terms.</span></span>
  * <span data-ttu-id="9c7dd-859">Till exempel: `facet=category,count:5` hämtar hello översta fem kategorier i aspekten resultat.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-859">For example: `facet=category,count:5` gets hello top five categories in facet results.</span></span>  
  * <span data-ttu-id="9c7dd-860">**Obs**: om hello `count` parameter är mindre än hello antalet unika villkor, hello resultaten kan vara felaktig.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-860">**Note**: If hello `count` parameter is less than hello number of unique terms, hello results may not be accurate.</span></span> <span data-ttu-id="9c7dd-861">Detta är på grund av toohello sätt faceting frågor är fördelade på shards.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-861">This is due toohello way faceting queries are distributed across shards.</span></span> <span data-ttu-id="9c7dd-862">Öka `count` vanligtvis ökar hello noggrannhet hello termen antal, men på en prestandakostnad.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-862">Increasing `count` generally increases hello accuracy of hello term counts, but at a performance cost.</span></span>
* <span data-ttu-id="9c7dd-863">`sort`(en av `count` toosort *fallande* efter antal `-count` toosort *stigande* efter antal `value` toosort *stigande* efter värde, eller `-value` toosort *fallande* av värdet)</span><span class="sxs-lookup"><span data-stu-id="9c7dd-863">`sort` (one of `count` toosort *descending* by count, `-count` toosort *ascending* by count, `value` toosort *ascending* by value, or `-value` toosort *descending* by value)</span></span>
  * <span data-ttu-id="9c7dd-864">Till exempel: `facet=category,count:3,sort:count` hämtar hello översta tre kategorier i aspekten resultaten i fallande ordning efter hello antalet dokument med varje Ortnamn.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-864">For example: `facet=category,count:3,sort:count` gets hello top three categories in facet results in descending order by hello number of documents with each city name.</span></span> <span data-ttu-id="9c7dd-865">Till exempel om hello översta tre kategorier är Budget, översikt och Lyxig, och Budget har 5 träffar översikt har 6 och Lyxig har 4, blir sedan hello buckets i hello ordning översikt, Budget, Lyxig.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-865">For example, if hello top three categories are Budget, Motel, and Luxury, and Budget has 5 hits, Motel has 6, and Luxury has 4, then hello buckets will be in hello order Motel, Budget, Luxury.</span></span>
  * <span data-ttu-id="9c7dd-866">Till exempel: `facet=rating,sort:-value` producerar buckets för alla möjliga betyg i fallande ordning efter värde.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-866">For example: `facet=rating,sort:-value` produces buckets for all possible ratings, in descending order by value.</span></span> <span data-ttu-id="9c7dd-867">Till exempel om hello klassificeringar är från 1 too5, beställs hello buckets 5, 4, 3, 2, 1, oavsett hur många dokument matchar varje klassificering.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-867">For example, if hello ratings are from 1 too5, hello buckets will be ordered 5, 4, 3, 2, 1, irrespective of how many documents match each rating.</span></span>
* <span data-ttu-id="9c7dd-868">`values`(pipe-avgränsad numeriskt eller `Edm.DateTimeOffset` värden som anger en dynamisk värdeuppsättningen aspekten post)</span><span class="sxs-lookup"><span data-stu-id="9c7dd-868">`values` (pipe-delimited numeric or `Edm.DateTimeOffset` values specifying a dynamic set of facet entry values)</span></span>
  * <span data-ttu-id="9c7dd-869">Till exempel: `facet=baseRate,values:10|20` ger tre buckets: en för grundavgift 0 in toobut exklusive 10, en för 10 in toobut inte inklusive 20 och en för 20 eller högre.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-869">For example: `facet=baseRate,values:10|20` produces three buckets: One for base rate 0 up toobut not including 10, one for 10 up toobut not including 20, and one for 20 or higher.</span></span>
  * <span data-ttu-id="9c7dd-870">Till exempel: `facet=lastRenovationDate,values:2010-02-01T00:00:00Z` ger två buckets: en för hotell renoverade före februari 2010 och en för hotell renoverade februari 1 2010 eller senare.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-870">For example: `facet=lastRenovationDate,values:2010-02-01T00:00:00Z` produces two buckets: One for hotels renovated before February 2010, and one for hotels renovated February 1st, 2010 or later.</span></span>
* <span data-ttu-id="9c7dd-871">`interval`(heltal-intervall som är större än 0 för siffror eller `minute`, `hour`, `day`, `week`, `month`, `quarter`, `year` för värden för datum-tid)</span><span class="sxs-lookup"><span data-stu-id="9c7dd-871">`interval` (integer interval greater than 0 for numbers, or `minute`, `hour`, `day`, `week`, `month`, `quarter`, `year` for date time values)</span></span>
  * <span data-ttu-id="9c7dd-872">Till exempel: `facet=baseRate,interval:100` producerar buckets baserat på grundavgift intervall i storlek 100.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-872">For example: `facet=baseRate,interval:100` produces buckets based on base rate ranges of size 100.</span></span> <span data-ttu-id="9c7dd-873">Till exempel om grundpris alla mellan $60 och $600, blir det buckets för 0-100, 200 100, 200 300, 300-400, 400 500 och 500 600.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-873">For example, if base rates are all between $60 and $600, there will be buckets for 0-100, 100-200, 200-300, 300-400, 400-500, and 500-600.</span></span>
  * <span data-ttu-id="9c7dd-874">Till exempel: `facet=lastRenovationDate,interval:year` producerar en bucket för varje år när hotell har renoverade.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-874">For example: `facet=lastRenovationDate,interval:year` produces one bucket for each year when hotels were renovated.</span></span>
* <span data-ttu-id="9c7dd-875">`timeoffset`([+-] hh: mm, [+-] hhmm, eller [+-] hh) `timeoffset` är valfritt.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-875">`timeoffset` ([+-]hh:mm, [+-]hhmm, or [+-]hh) `timeoffset` is optional.</span></span> <span data-ttu-id="9c7dd-876">Kan endast kombineras med hello `interval` alternativet och endast när tillämpade tooa fält av typen `Edm.DateTimeOffset`.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-876">It can only be combined with hello `interval` option, and only when applied tooa field of type `Edm.DateTimeOffset`.</span></span> <span data-ttu-id="9c7dd-877">hello värdet anger hello UTC-tid offset tooaccount för inställningar av tid gränser.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-877">hello value specifies hello UTC time offset tooaccount for in setting time boundaries.</span></span>
  * <span data-ttu-id="9c7dd-878">Till exempel: `facet=lastRenovationDate,interval:day,timeoffset:-01:00` använder hello dag gräns som börjar vid UTC 01:00:00 (midnatt i hello mål tidszon)</span><span class="sxs-lookup"><span data-stu-id="9c7dd-878">For example: `facet=lastRenovationDate,interval:day,timeoffset:-01:00` uses hello day boundary that starts at 01:00:00 UTC (midnight in hello target time zone)</span></span>
* <span data-ttu-id="9c7dd-879">**Obs**: `count` och `sort` kan kombineras i hello samma aspekten specifikation, men de kan inte kombineras med `interval` eller `values`, och `interval` och `values` kan inte kombineras.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-879">**Note**: `count` and `sort` can be combined in hello same facet specification, but they cannot be combined with `interval` or `values`, and `interval` and `values` cannot be combined together.</span></span>
* <span data-ttu-id="9c7dd-880">**Obs**: intervall aspekter på datum och tid beräknas baserat på UTC-tid om `timeoffset` har inte angetts.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-880">**Note**: Interval facets on date time are computed based on UTC time if `timeoffset` is not specified.</span></span> <span data-ttu-id="9c7dd-881">Exempel: för `facet=lastRenovationDate,interval:day`, hello dag gräns som börjar vid 00:00:00 UTC.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-881">For example: for `facet=lastRenovationDate,interval:day`, hello day boundary starts at 00:00:00 UTC.</span></span> 

> [!NOTE]
> <span data-ttu-id="9c7dd-882">När du anropar **Sök** med efter den här parametern heter `facets` i stället för `facet`.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-882">When calling **Search** using POST, this parameter is named `facets` instead of `facet`.</span></span> <span data-ttu-id="9c7dd-883">Du också ange den som en JSON-matris med strängar som där varje sträng är ett separat aspekten uttryck.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-883">Also, you specify it as a JSON array of strings where each string is a separate facet expression.</span></span>
> 
> 

<span data-ttu-id="9c7dd-884">`$filter=[string]`(valfritt) – ett strukturerade sökuttryck standard OData-syntax.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-884">`$filter=[string]` (optional) - A structured search expression in standard OData syntax.</span></span>

> [!NOTE]
> <span data-ttu-id="9c7dd-885">När du anropar **Sök** med efter den här parametern heter `filter` i stället för `$filter`.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-885">When calling **Search** using POST, this parameter is named `filter` instead of `$filter`.</span></span>
> 
> 

<span data-ttu-id="9c7dd-886">`highlight=[string]`(valfritt) – en uppsättning csv-fältnamn som används för träffar markeras.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-886">`highlight=[string]` (optional) - A set of comma-separated field names used for hit highlights.</span></span> <span data-ttu-id="9c7dd-887">Endast `searchable` fält kan användas för träffar markering.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-887">Only `searchable` fields can be used for hit highlighting.</span></span>

<span data-ttu-id="9c7dd-888">`highlightPreTag=[string]`(valfritt, standardvärden för`<em>`) – en string-tagg som läggs toohit visar.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-888">`highlightPreTag=[string]` (optional, defaults too`<em>`) - A string tag that prepends toohit highlights.</span></span> <span data-ttu-id="9c7dd-889">Måste anges med `highlightPostTag`.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-889">Must be set with `highlightPostTag`.</span></span>

> [!NOTE]
> <span data-ttu-id="9c7dd-890">När du anropar **Sök** använder GET, reserverade tecken i hello URL måste vara procent-kodat (till exempel % 23 i stället för #).</span><span class="sxs-lookup"><span data-stu-id="9c7dd-890">When calling **Search** using GET, reserved characters in hello URL must be percent-encoded (for example, %23 instead of #).</span></span>
> 
> 

<span data-ttu-id="9c7dd-891">`highlightPostTag=[string]`(valfritt, standardvärden för`</em>`)-taggen sträng som lägger till toohit visar.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-891">`highlightPostTag=[string]` (optional, defaults too`</em>`) - a string tag that appends toohit highlights.</span></span> <span data-ttu-id="9c7dd-892">Måste anges med `highlightPreTag`.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-892">Must be set with `highlightPreTag`.</span></span>

> [!NOTE]
> <span data-ttu-id="9c7dd-893">När du anropar **Sök** använder GET, reserverade tecken i hello URL måste vara procent-kodat (till exempel % 23 i stället för #).</span><span class="sxs-lookup"><span data-stu-id="9c7dd-893">When calling **Search** using GET, reserved characters in hello URL must be percent-encoded (for example, %23 instead of #).</span></span>
> 
> 

<span data-ttu-id="9c7dd-894">`scoringProfile=[string]`(valfritt) – hello namnet på en bedömningsprofil profil tooevaluate matcha poängen för matchande dokument i ordning toosort hello resultat.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-894">`scoringProfile=[string]` (optional) - hello name of a scoring profile tooevaluate match scores for matching documents in order toosort hello results.</span></span>

<span data-ttu-id="9c7dd-895">`scoringParameter=[string]`(noll eller fler) – anger hello värden för varje parameter som definierats i funktionen för bedömningsprofil (till exempel `referencePointParameter`) hello formatet `name-value1,value2,...`.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-895">`scoringParameter=[string]` (zero or more) - Indicates hello values for each parameter defined in a scoring function (for example, `referencePointParameter`) using hello format `name-value1,value2,...`.</span></span>

* <span data-ttu-id="9c7dd-896">Till exempel om hello bedömningen profil definierar en funktion med en parameter med namnet ”mylocation” Hej frågesträngsalternativet skulle vara `&scoringParameter=mylocation--122.2,44.8`.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-896">For example, if hello scoring profile defines a function with a parameter called "mylocation" hello query string option would be `&scoringParameter=mylocation--122.2,44.8`.</span></span> <span data-ttu-id="9c7dd-897">hello första dash skiljer hello namn från hello värden i listan när hello andra streck är en del av hello första värde (longitud i det här exemplet).</span><span class="sxs-lookup"><span data-stu-id="9c7dd-897">hello first dash separates hello name from hello value list, while hello second dash is part of hello first value (longitude in this example).</span></span>
* <span data-ttu-id="9c7dd-898">För bedömningsprofil parametrar som taggar för förstärkning som kan innehålla kommatecken, kan du escape sådana värden i hello listan med hjälp av enkla citattecken.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-898">For scoring parameters such as for tag boosting that can contain commas, you can escape any such values in hello list using single quotes.</span></span> <span data-ttu-id="9c7dd-899">Om själva hello värdena innehåller enkla citattecken, kan du undanta dem av dubblerade.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-899">If hello values themselves contain single quotes, you can escape them by doubling.</span></span>
  * <span data-ttu-id="9c7dd-900">Till exempel om du har en tagg förstärkning parameter med namnet ”mytag” och du vill tooboost hello-taggen värden ”Hello, O'Brien” och ”Smith” hello frågan alternativ för anslutningssträngen skulle vara `&scoringParameter=mytag-'Hello, O''Brien',Smith`.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-900">For example, if you have a tag boosting parameter called "mytag" and you want tooboost on hello tag values "Hello, O'Brien" and "Smith", hello query string option would be `&scoringParameter=mytag-'Hello, O''Brien',Smith`.</span></span> <span data-ttu-id="9c7dd-901">Observera att citattecken är endast för värden med kommatecken.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-901">Note that quotes are only required for values that contain commas.</span></span>

> [!NOTE]
> <span data-ttu-id="9c7dd-902">När du anropar **Sök** med efter den här parametern heter `scoringParameters` i stället för `scoringParameter`.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-902">When calling **Search** using POST, this parameter is named `scoringParameters` instead of `scoringParameter`.</span></span> <span data-ttu-id="9c7dd-903">Du också ange den som en JSON-matris med strängar som där varje sträng är en separat `name-values` par.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-903">Also, you specify it as a JSON array of strings where each string is a separate `name-values` pair.</span></span>
> 
> 

<span data-ttu-id="9c7dd-904">`minimumCoverage`(valfritt, som standard too100) - ett tal mellan 0 och 100 som visar hello procentandelen hello index som måste förses med en sökfråga för hello frågan toobe rapporteras som lyckas.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-904">`minimumCoverage` (optional, defaults too100) - a number between 0 and 100 indicating hello percentage of hello index that must be covered by a search query in order for hello query toobe reported as a success.</span></span> <span data-ttu-id="9c7dd-905">Som standard hello hela index måste vara tillgängliga eller `Search` returneras HTTP-statuskoden 503.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-905">By default, hello entire index must be available or `Search` will return HTTP status code 503.</span></span> <span data-ttu-id="9c7dd-906">Om du ställer in `minimumCoverage` och `Search` lyckas den ska returnera HTTP 200 och innehålla en `@search.coverage` värdet i hello-svar som visar hello procentandelen hello index som ingick i hello-frågan.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-906">If you set `minimumCoverage` and `Search` succeeds, it will return HTTP 200 and include a `@search.coverage` value in hello response indicating hello percentage of hello index that was included in hello query.</span></span>

> [!NOTE]
> <span data-ttu-id="9c7dd-907">Om parametern tooa värdet ska vara mindre än 100 kan vara användbart för att söka efter tillgänglighet även för tjänster med bara en replik.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-907">Setting this parameter tooa value lower than 100 can be useful for ensuring search availability even for services with only one replica.</span></span> <span data-ttu-id="9c7dd-908">Dock är inte alla matchande dokument garanterat toobe finns i hello sökresultat.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-908">However, not all matching documents are guaranteed toobe present in hello search results.</span></span> <span data-ttu-id="9c7dd-909">Om sökningen återkallning är viktigare tooyour program än tillgänglighet, så är det bästa tooleave `minimumCoverage` standardinställningen 100.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-909">If search recall is more important tooyour application than availability, then it's best tooleave `minimumCoverage` at its default value of 100.</span></span>
> 
> 

<span data-ttu-id="9c7dd-910">`api-version=[string]`(krävs).</span><span class="sxs-lookup"><span data-stu-id="9c7dd-910">`api-version=[string]` (required).</span></span> <span data-ttu-id="9c7dd-911">hello förhandsversionen är `api-version=2015-02-28-Preview`.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-911">hello preview version is `api-version=2015-02-28-Preview`.</span></span> <span data-ttu-id="9c7dd-912">Se [Sök Versionhantering](http://msdn.microsoft.com/library/azure/dn864560.aspx) information och alternativa versioner.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-912">See [Search Service Versioning](http://msdn.microsoft.com/library/azure/dn864560.aspx) for details and alternative versions.</span></span>

<span data-ttu-id="9c7dd-913">Obs: För den här åtgärden hello `api-version` har angetts som en frågeparameter i hello URL oavsett om du anropar **Sök** med GET eller POST.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-913">Note: For this operation, hello `api-version` is specified as a query parameter in hello URL regardless of whether you call **Search** with GET or POST.</span></span>

<span data-ttu-id="9c7dd-914">**Huvuden för begäran**</span><span class="sxs-lookup"><span data-stu-id="9c7dd-914">**Request Headers**</span></span>

<span data-ttu-id="9c7dd-915">hello följande lista beskriver hello nödvändiga och valfria begärandehuvuden.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-915">hello following list describes hello required and optional request headers.</span></span>

* <span data-ttu-id="9c7dd-916">`api-key`: hello `api-key` är används tooauthenticate hello begäran tooyour Search-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-916">`api-key`: hello `api-key` is used tooauthenticate hello request tooyour Search service.</span></span> <span data-ttu-id="9c7dd-917">Det är ett strängvärde, unika tooyour tjänst-URL.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-917">It is a string value, unique tooyour service URL.</span></span> <span data-ttu-id="9c7dd-918">Hej **Sök** begäran kan ange en administrationsnyckeln eller Frågenyckeln för `api-key`.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-918">hello **Search** request can specify either an admin key or query key for `api-key`.</span></span>

<span data-ttu-id="9c7dd-919">Du måste också hello service name tooconstruct hello begärd URL.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-919">You will also need hello service name tooconstruct hello request URL.</span></span> <span data-ttu-id="9c7dd-920">Du kan hämta hello tjänstnamn och `api-key` från instrumentpanelen service i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-920">You can get hello service name and `api-key` from your service dashboard in hello Azure Portal.</span></span> <span data-ttu-id="9c7dd-921">Se [skapa en Azure Search-tjänst i hello portal](search-create-service-portal.md) sidan navigering hjälp.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-921">See [Create an Azure Search service in hello portal](search-create-service-portal.md) for page navigation help.</span></span>

<span data-ttu-id="9c7dd-922">**Begärandetexten**</span><span class="sxs-lookup"><span data-stu-id="9c7dd-922">**Request Body**</span></span>

<span data-ttu-id="9c7dd-923">För GET: Ingen.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-923">For GET: None.</span></span>

<span data-ttu-id="9c7dd-924">För POST:</span><span class="sxs-lookup"><span data-stu-id="9c7dd-924">For POST:</span></span>

    {
      "count": true | false (default),
      "facets": [ "facet_expression_1", "facet_expression_2", ... ],
      "filter": "odata_filter_expression",
      "highlight": "highlight_field_1, highlight_field_2, ...",
      "highlightPreTag": "pre_tag",
      "highlightPostTag": "post_tag",
      "minimumCoverage": # (% of index that must be covered toodeclare query successful; default 100),
      "moreLikeThis": "document_key" (mutually exclusive with "search" parameter),
      "orderby": "orderby_expression",
      "scoringParameters": [ "scoring_parameter_1", "scoring_parameter_2", ... ],
      "scoringProfile": "scoring_profile_name",
      "search": "simple_query_expression",
      "searchFields": "field_name_1, field_name_2, ...",
      "searchMode": "any" (default) | "all",
      "select": "field_name_1, field_name_2, ...",
      "skip": # (default 0),
      "top": #
    }

<span data-ttu-id="9c7dd-925">**Fortsättning av svar som partiell sökning**</span><span class="sxs-lookup"><span data-stu-id="9c7dd-925">**Continuation of Partial Search Responses**</span></span>

<span data-ttu-id="9c7dd-926">Ibland kan inte Azure Search returnera alla hello begärda resulterar i ett enda Search-svar.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-926">Sometimes Azure Search can't return all hello requested results in a single Search response.</span></span> <span data-ttu-id="9c7dd-927">Detta kan inträffa av olika skäl, till exempel när hello begäranden för många dokument genom att inte ange `$top` eller ange ett värde för `$top` som är för stor.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-927">This can happen for different reasons, such as when hello query requests too many documents by not specifying `$top` or specifying a value for `$top` that is too large.</span></span> <span data-ttu-id="9c7dd-928">I sådana fall Azure Search innehåller hello `@odata.nextLink` kommentar i hello svarstexten och även `@search.nextPageParameters` om den har en POST-begäran.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-928">In such cases, Azure Search will include hello `@odata.nextLink` annotation in hello response body, and also `@search.nextPageParameters` if it was a POST request.</span></span> <span data-ttu-id="9c7dd-929">Du kan använda hello värdena för dessa anteckningar tooformulate en annan Sök begäran tooget hello nästa del av hello search-svar.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-929">You can use hello values of these annotations tooformulate another Search request tooget hello next part of hello search response.</span></span> <span data-ttu-id="9c7dd-930">Detta kallas en ***fortsättning*** hello ursprungliga sökbegäran och hello kallas vanligtvis anteckningar ***fortsättning token***.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-930">This is called a ***continuation*** of hello original Search request, and hello annotations are generally called ***continuation tokens***.</span></span> <span data-ttu-id="9c7dd-931">Se [hello exemplet nedan](#SearchResponse) mer information om dessa kommentarer och var de förekommer i hello svarstexten hello-syntax.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-931">See [hello example below](#SearchResponse) for details on hello syntax of these annotations and where they appear in hello response body.</span></span> 

<span data-ttu-id="9c7dd-932">hello skäl varför Azure Search kan returnera fortsättning token är implementeringen-specifika och ämne toochange.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-932">hello reasons why Azure Search might return continuation tokens are implementation-specific and subject toochange.</span></span> <span data-ttu-id="9c7dd-933">Robust klienter ska alltid vara klar toohandle fall där färre dokument än väntat returneras en fortsättningstoken är inkluderade toocontinue hämtning av dokument.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-933">Robust clients should always be ready toohandle cases where fewer documents than expected are returned and a continuation token is included toocontinue retrieving documents.</span></span> <span data-ttu-id="9c7dd-934">Även hello Observera att du måste använda samma HTTP-metod som hello ursprungliga begäran i ordning toocontinue.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-934">Also note that you must use hello same HTTP method as hello original request in order toocontinue.</span></span> <span data-ttu-id="9c7dd-935">Till exempel om du har skickat en begäran om att hämta alla fortsättning begäranden som du skickar måste också använda GET (och även för POST).</span><span class="sxs-lookup"><span data-stu-id="9c7dd-935">For example, if you sent a GET request, any continuation requests you send must also use GET (and likewise for POST).</span></span>

<span data-ttu-id="9c7dd-936"><a name="SearchResponse"></a>
**Svar**</span><span class="sxs-lookup"><span data-stu-id="9c7dd-936"><a name="SearchResponse"></a>
**Response**</span></span>

<span data-ttu-id="9c7dd-937">Statuskod: 200 OK returneras för ett lyckat svar.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-937">Status Code: 200 OK is returned for a successful response.</span></span>

    {
      "@odata.count": # (if $count=true was provided in hello query),
      "@search.coverage": # (if minimumCoverage was provided in hello query),
      "@search.facets": { (if faceting was specified in hello query)
        "facet_field": [
          {
            "value": facet_entry_value (for non-range facets),
            "from": facet_entry_value (for range facets),
            "to": facet_entry_value (for range facets),
            "count": number_of_documents
          }
        ],
        ...
      },
      "@search.nextPageParameters": { (request body toofetch hello next page of results if not all results could be returned in this response and Search was called with POST)
        "count": ... (value from request body if present),
        "facets": ... (value from request body if present),
        "filter": ... (value from request body if present),
        "highlight": ... (value from request body if present),
        "highlightPreTag": ... (value from request body if present),
        "highlightPostTag": ... (value from request body if present),
        "minimumCoverage": ... (value from request body if present),
        "moreLikeThis": ... (value from request body if present),
        "orderby": ... (value from request body if present),
        "scoringParameters": ... (value from request body if present),
        "scoringProfile": ... (value from request body if present),
        "search": ... (value from request body if present),
        "searchFields": ... (value from request body if present),
        "searchMode": ... (value from request body if present),
        "select": ... (value from request body if present),
        "skip": ... (page size plus value from request body if present),
        "top": ... (value from request body if present minus page size),
      },
      "value": [
        {
          "@search.score": document_score (if a text query was provided),
          "@search.highlights": {
            field_name: [ subset of text, ... ],
            ...
          },
          key_field_name: document_key,
          field_name: field_value (retrievable fields or specified projection),
          ...
        },
        ...
      ],
      "@odata.nextLink": (URL toofetch hello next page of results if not all results could be returned in this response; Applies tooboth GET and POST)
    }

<span data-ttu-id="9c7dd-938">**Exempel:**</span><span class="sxs-lookup"><span data-stu-id="9c7dd-938">**Examples:**</span></span>

<span data-ttu-id="9c7dd-939">Du hittar ytterligare exempel på hello [syntaxen för OData-uttryck för Azure Search](https://msdn.microsoft.com/library/azure/dn798921.aspx) sidan.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-939">You can find additional examples on hello [OData Expression Syntax for Azure Search](https://msdn.microsoft.com/library/azure/dn798921.aspx) page.</span></span>

1)    <span data-ttu-id="9c7dd-940">Sök hello fallande Index sorteras efter datum.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-940">Search hello Index sorted descending by date.</span></span>

    <span data-ttu-id="9c7dd-941">GET-/indexes/hotels/docs? Sök = * & $orderby = lastRenovationDate desc & api-version = 2015-02-28-Preview</span><span class="sxs-lookup"><span data-stu-id="9c7dd-941">GET /indexes/hotels/docs?search=*&$orderby=lastRenovationDate desc&api-version=2015-02-28-Preview</span></span>

    <span data-ttu-id="9c7dd-942">POST /indexes/hotels/docs/search? api-version 2015-02-28-Preview = {”Sök” ”: *”, ”orderby”: ”lastRenovationDate desc”}</span><span class="sxs-lookup"><span data-stu-id="9c7dd-942">POST /indexes/hotels/docs/search?api-version=2015-02-28-Preview { "search": "*", "orderby": "lastRenovationDate desc" }</span></span>

2)    <span data-ttu-id="9c7dd-943">Söka hello index i en fasetterad sökning och hämta facets för kategorier, klassificering, etiketter samt artiklar med baseRate i särskilda intervall:</span><span class="sxs-lookup"><span data-stu-id="9c7dd-943">In a faceted search, search hello index and retrieve facets for categories, rating, tags, as well as items with baseRate in specific ranges:</span></span>

    <span data-ttu-id="9c7dd-944">GET-/indexes/hotels/docs? Sök = test & aspekten = kategori & aspekten = klassificering & aspekten = taggar & aspekten = baseRate värden: 80 | 150 | 220 & api-version = 2015-02-28-Preview</span><span class="sxs-lookup"><span data-stu-id="9c7dd-944">GET /indexes/hotels/docs?search=test&facet=category&facet=rating&facet=tags&facet=baseRate,values:80|150|220&api-version=2015-02-28-Preview</span></span>

    <span data-ttu-id="9c7dd-945">POST /indexes/hotels/docs/search? api-version 2015-02-28-Preview = {”Sök”: ”test”, ”facets”: [”kategori”, ”klassificering”, ”-taggar” ”baseRate värden: 80 | 150 | 220”]}</span><span class="sxs-lookup"><span data-stu-id="9c7dd-945">POST /indexes/hotels/docs/search?api-version=2015-02-28-Preview { "search": "test", "facets": [ "category", "rating", "tags", "baseRate,values:80|150|220" ] }</span></span>

3)    <span data-ttu-id="9c7dd-946">Med hjälp av ett filter, begränsa hello tidigare fasetterad frågeresultaten när hello användaren klickar på klassificeringen 3 och kategori ”översikt”:</span><span class="sxs-lookup"><span data-stu-id="9c7dd-946">Using a filter, narrow down hello previous faceted query results after hello user clicks on rating 3 and category "Motel":</span></span>

    <span data-ttu-id="9c7dd-947">GET-/indexes/hotels/docs? Sök = test & aspekten = taggar & aspekten = baseRate värden: 80 | 150 | 220 & $filter = klassificering eq 3 och kategori eq 'översikt' & api-version = 2015-02-28-Preview</span><span class="sxs-lookup"><span data-stu-id="9c7dd-947">GET /indexes/hotels/docs?search=test&facet=tags&facet=baseRate,values:80|150|220&$filter=rating eq 3 and category eq 'Motel'&api-version=2015-02-28-Preview</span></span>

    <span data-ttu-id="9c7dd-948">POST /indexes/hotels/docs/search? api-version = 2015-02-28-Preview {”Sök”: ”test”, ”facets”: [”taggar” ”, baseRate värden: 80 | 150 | 220”], ”filter”: ”klassificering eq 3 och kategori eq 'Översikt'”}</span><span class="sxs-lookup"><span data-stu-id="9c7dd-948">POST /indexes/hotels/docs/search?api-version=2015-02-28-Preview { "search": "test", "facets": [ "tags", "baseRate,values:80|150|220" ], "filter": "rating eq 3 and category eq 'Motel'" }</span></span>

4) <span data-ttu-id="9c7dd-949">Ange en övre gräns på unikt villkor som returneras i en fråga i en fasetterad sökning.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-949">In a faceted search, set an upper limit on unique terms returned in a query.</span></span> <span data-ttu-id="9c7dd-950">hello standardvärdet är 10, men du kan öka eller minska det här värdet med hello `count` parameter på hello `facet` attribut:</span><span class="sxs-lookup"><span data-stu-id="9c7dd-950">hello default is 10, but you can increase or decrease this value using hello `count` parameter on hello `facet` attribute:</span></span>

    <span data-ttu-id="9c7dd-951">GET-/indexes/hotels/docs? Sök = test & aspekten = ort, antal: 5 & api-version = 2015-02-28-Preview</span><span class="sxs-lookup"><span data-stu-id="9c7dd-951">GET /indexes/hotels/docs?search=test&facet=city,count:5&api-version=2015-02-28-Preview</span></span>

    <span data-ttu-id="9c7dd-952">POST /indexes/hotels/docs/search? api-version 2015-02-28-Preview = {”Sök”: ”test”, ”facets”: [”Ort, antal: 5”]}</span><span class="sxs-lookup"><span data-stu-id="9c7dd-952">POST /indexes/hotels/docs/search?api-version=2015-02-28-Preview  {    "search": "test",    "facets": [ "city,count:5" ]  }</span></span>

5)    <span data-ttu-id="9c7dd-953">Sök hello Index inom specifika områden. Till exempel ett fält för vissa språk:</span><span class="sxs-lookup"><span data-stu-id="9c7dd-953">Search hello Index within specific fields; For example, a language-specific field:</span></span>

    <span data-ttu-id="9c7dd-954">GET-/indexes/hotels/docs? Sök = hôtel & searchFields = description_fr & api-version = 2015-02-28-Preview</span><span class="sxs-lookup"><span data-stu-id="9c7dd-954">GET /indexes/hotels/docs?search=hôtel&searchFields=description_fr&api-version=2015-02-28-Preview</span></span>

    <span data-ttu-id="9c7dd-955">POST /indexes/hotels/docs/search? api-version 2015-02-28-Preview = {”Sök”: ”hôtel”, ”searchFields”: ”description_fr”}</span><span class="sxs-lookup"><span data-stu-id="9c7dd-955">POST /indexes/hotels/docs/search?api-version=2015-02-28-Preview { "search": "hôtel", "searchFields": "description_fr" }</span></span>

6) <span data-ttu-id="9c7dd-956">Sök hello Index över flera fält.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-956">Search hello Index across multiple fields.</span></span> <span data-ttu-id="9c7dd-957">Exempelvis kan du lagra och fråga sökbara fält på flera språk och alla inom hello samma index.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-957">For example, you can store and query searchable fields in multiple languages, all within hello same index.</span></span>  <span data-ttu-id="9c7dd-958">Om engelska och franska beskrivningar samexisterar i hello samma dokument, du kan returnera några eller alla i hello frågeresultat:</span><span class="sxs-lookup"><span data-stu-id="9c7dd-958">If English and French descriptions co-exist in hello same document, you can return any or all in hello query results:</span></span>

    <span data-ttu-id="9c7dd-959">GET-/indexes/hotels/docs? Sök = hotell & searchFields = beskrivning, description_fr & api-version = 2015-02-28-Preview</span><span class="sxs-lookup"><span data-stu-id="9c7dd-959">GET /indexes/hotels/docs?search=hotel&searchFields=description,description_fr&api-version=2015-02-28-Preview</span></span>

    <span data-ttu-id="9c7dd-960">POST /indexes/hotels/docs/search? api-version 2015-02-28-Preview = {”Sök”: ”hotell”, ”searchFields”: ”beskrivning, description_fr”}</span><span class="sxs-lookup"><span data-stu-id="9c7dd-960">POST /indexes/hotels/docs/search?api-version=2015-02-28-Preview  {    "search": "hotel",    "searchFields": "description, description_fr"  }</span></span>

<span data-ttu-id="9c7dd-961">Observera att du bara kan fråga ett index i taget.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-961">Note that you can only query one index at a time.</span></span> <span data-ttu-id="9c7dd-962">Skapa inte flera index för varje språk om du planerar tooquery en i taget.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-962">Do not create multiple indexes for each language unless you plan tooquery one at a time.</span></span>

7)    <span data-ttu-id="9c7dd-963">Växlingsfil - Get hello 1 sida med objekt (sidstorlek är 10):</span><span class="sxs-lookup"><span data-stu-id="9c7dd-963">Paging - Get hello 1st page of items (page size is 10):</span></span>

    <span data-ttu-id="9c7dd-964">GET-/indexes/hotels/docs? Sök = * & $skip = 0 & $top = 10 & api-version = 2015-02-28-Preview</span><span class="sxs-lookup"><span data-stu-id="9c7dd-964">GET /indexes/hotels/docs?search=*&$skip=0&$top=10&api-version=2015-02-28-Preview</span></span>

    <span data-ttu-id="9c7dd-965">POST /indexes/hotels/docs/search? api-version 2015-02-28-Preview = {”Sök” ”: *”, ”hoppa över”: 0, ”top”: 10}</span><span class="sxs-lookup"><span data-stu-id="9c7dd-965">POST /indexes/hotels/docs/search?api-version=2015-02-28-Preview { "search": "*", "skip": 0, "top": 10 }</span></span>

8)    <span data-ttu-id="9c7dd-966">Växlingsfil - Get hello 2 sida med objekt (sidstorlek är 10):</span><span class="sxs-lookup"><span data-stu-id="9c7dd-966">Paging - Get hello 2nd page of items (page size is 10):</span></span>

    <span data-ttu-id="9c7dd-967">GET-/indexes/hotels/docs? Sök = * & $skip = 10 & $top = 10 & api-version = 2015-02-28-Preview</span><span class="sxs-lookup"><span data-stu-id="9c7dd-967">GET /indexes/hotels/docs?search=*&$skip=10&$top=10&api-version=2015-02-28-Preview</span></span>

    <span data-ttu-id="9c7dd-968">POST /indexes/hotels/docs/search? api-version 2015-02-28-Preview = {”Sök” ”: *”, ”hoppa över”: ”top” 10: 10}</span><span class="sxs-lookup"><span data-stu-id="9c7dd-968">POST /indexes/hotels/docs/search?api-version=2015-02-28-Preview { "search": "*", "skip": 10, "top": 10 }</span></span>

9)    <span data-ttu-id="9c7dd-969">Hämta en specifik uppsättning fält:</span><span class="sxs-lookup"><span data-stu-id="9c7dd-969">Retrieve a specific set of fields:</span></span>

    <span data-ttu-id="9c7dd-970">GET-/indexes/hotels/docs? Sök = * & $select = hotelName, beskrivning och api-version = 2015-02-28-Preview</span><span class="sxs-lookup"><span data-stu-id="9c7dd-970">GET /indexes/hotels/docs?search=*&$select=hotelName,description&api-version=2015-02-28-Preview</span></span>

    <span data-ttu-id="9c7dd-971">POST /indexes/hotels/docs/search? api-version 2015-02-28-Preview = {”Sök” ”: *”, ”Välj”: ”hotelName beskrivning”}</span><span class="sxs-lookup"><span data-stu-id="9c7dd-971">POST /indexes/hotels/docs/search?api-version=2015-02-28-Preview { "search": "*", "select": "hotelName, description" }</span></span>

10)  <span data-ttu-id="9c7dd-972">Hämta dokument som matchar ett specifikt filter-uttryck:</span><span class="sxs-lookup"><span data-stu-id="9c7dd-972">Retrieve documents matching a specific filter expression:</span></span>

    <span data-ttu-id="9c7dd-973">GET-/indexes/hotels/docs? $filter = (baseRate ge 60 och baseRate lt 300) eller hotelName eq 'Avancerad förblir' & api-version = 2015-02-28-Preview</span><span class="sxs-lookup"><span data-stu-id="9c7dd-973">GET /indexes/hotels/docs?$filter=(baseRate ge 60 and baseRate lt 300) or hotelName eq 'Fancy Stay'&api-version=2015-02-28-Preview</span></span>

    <span data-ttu-id="9c7dd-974">POST /indexes/hotels/docs/search? api-version 2015-02-28-Preview = {”filter” ”: (baseRate ge 60 och baseRate lt 300) eller hotelName eq 'Avancerad stanna'”}</span><span class="sxs-lookup"><span data-stu-id="9c7dd-974">POST /indexes/hotels/docs/search?api-version=2015-02-28-Preview {  "filter": "(baseRate ge 60 and baseRate lt 300) or hotelName eq 'Fancy Stay'" }</span></span>

11) <span data-ttu-id="9c7dd-975">Hello sökindex och returnera fragment med träffar viktiga funktioner</span><span class="sxs-lookup"><span data-stu-id="9c7dd-975">Search hello index and return fragments with hit highlights</span></span>

    <span data-ttu-id="9c7dd-976">GET-/indexes/hotels/docs? Sök = något & Markera = beskrivning & api-version = 2015-02-28-Preview</span><span class="sxs-lookup"><span data-stu-id="9c7dd-976">GET /indexes/hotels/docs?search=something&highlight=description&api-version=2015-02-28-Preview</span></span>

    <span data-ttu-id="9c7dd-977">POST /indexes/hotels/docs/search? api-version 2015-02-28-Preview = {”Sök”: ”något”, ”highlight”: ”beskrivning”}</span><span class="sxs-lookup"><span data-stu-id="9c7dd-977">POST /indexes/hotels/docs/search?api-version=2015-02-28-Preview {   "search": "something",   "highlight": "description" }</span></span>

12) <span data-ttu-id="9c7dd-978">Hello sökindex och returnera dokument sorterade från närmare toofarther från en referensplats</span><span class="sxs-lookup"><span data-stu-id="9c7dd-978">Search hello index and return documents sorted from closer toofarther away from a reference location</span></span>

    <span data-ttu-id="9c7dd-979">GET-/indexes/hotels/docs? Sök = något & $orderby=geo.distance (plats, geography'POINT(-122.12315 47.88121)') & api-version = 2015-02-28-Preview</span><span class="sxs-lookup"><span data-stu-id="9c7dd-979">GET /indexes/hotels/docs?search=something&$orderby=geo.distance(location, geography'POINT(-122.12315 47.88121)')&api-version=2015-02-28-Preview</span></span>

    <span data-ttu-id="9c7dd-980">POST /indexes/hotels/docs/search? api-version 2015-02-28-Preview = {”Sök”: ”något”, ”orderby” ”: geo.distance (plats, geography'POINT(-122.12315 47.88121)')”}</span><span class="sxs-lookup"><span data-stu-id="9c7dd-980">POST /indexes/hotels/docs/search?api-version=2015-02-28-Preview {   "search": "something",   "orderby": "geo.distance(location, geography'POINT(-122.12315 47.88121)')" }</span></span>

13) <span data-ttu-id="9c7dd-981">Sök hello index under förutsättning att det är en bedömningsprofilen som kallas ”geo” med två avståndsresultatfunktioner, en anger en parameter som kallas ”currentLocation” och en definierar en parameter med namnet ”lastLocation”</span><span class="sxs-lookup"><span data-stu-id="9c7dd-981">Search hello index assuming there's a scoring profile called "geo" with two distance scoring functions, one defining a parameter called "currentLocation" and one defining a parameter called "lastLocation"</span></span>

    <span data-ttu-id="9c7dd-982">GET-/indexes/hotels/docs? Sök = något & scoringProfile = geo & scoringParameter = currentLocation--122.123,44.77233 & scoringParameter = lastLocation--121.499,44.2113 & api-version = 2015-02-28-Preview</span><span class="sxs-lookup"><span data-stu-id="9c7dd-982">GET /indexes/hotels/docs?search=something&scoringProfile=geo&scoringParameter=currentLocation--122.123,44.77233&scoringParameter=lastLocation--121.499,44.2113&api-version=2015-02-28-Preview</span></span>

    <span data-ttu-id="9c7dd-983">POST /indexes/hotels/docs/search? api-version 2015-02-28-Preview = {”Sök”: ”något”, ”scoringProfile”: ”geo”, ”scoringParameters”: [”currentLocation--122.123,44.77233” ”, lastLocation--121.499,44.2113”]}</span><span class="sxs-lookup"><span data-stu-id="9c7dd-983">POST /indexes/hotels/docs/search?api-version=2015-02-28-Preview {   "search": "something",   "scoringProfile": "geo",   "scoringParameters": [ "currentLocation--122.123,44.77233", "lastLocation--121.499,44.2113" ] }</span></span>

14) <span data-ttu-id="9c7dd-984">Hitta dokument i hello index med [enkel frågesyntaxen](https://msdn.microsoft.com/library/dn798920.aspx).</span><span class="sxs-lookup"><span data-stu-id="9c7dd-984">Find documents in hello index using [simple query syntax](https://msdn.microsoft.com/library/dn798920.aspx).</span></span> <span data-ttu-id="9c7dd-985">Den här frågan returnerar hotell där sökbara fält innehåller hello villkoren ”bekvämlighet” och ”plats” men inte ”översikt”:</span><span class="sxs-lookup"><span data-stu-id="9c7dd-985">This query returns hotels where searchable fields contain hello terms "comfort" and "location" but not "motel":</span></span>

    <span data-ttu-id="9c7dd-986">GET-/indexes/hotels/docs? Sök = bekvämlighet + plats-översikt & searchMode = all & api-version = 2015-02-28-Preview</span><span class="sxs-lookup"><span data-stu-id="9c7dd-986">GET /indexes/hotels/docs?search=comfort +location -motel&searchMode=all&api-version=2015-02-28-Preview</span></span>

    <span data-ttu-id="9c7dd-987">POST /indexes/hotels/docs/search? api-version 2015-02-28-Preview = {”Sök” ”: bekvämlighet + plats-översikt”, ”searchMode”: ”alla”}</span><span class="sxs-lookup"><span data-stu-id="9c7dd-987">POST /indexes/hotels/docs/search?api-version=2015-02-28-Preview {   "search": "comfort +location -motel",   "searchMode": "all" }</span></span>

<span data-ttu-id="9c7dd-988">Observera hello användning av `searchMode=all` ovan.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-988">Note hello use of `searchMode=all` above.</span></span> <span data-ttu-id="9c7dd-989">Inklusive den här parametern åsidosätter hello standardvärdet `searchMode=any`, att se till att som `-motel` innebär ”och inte” i stället för ”eller inte”.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-989">Including this parameter overrides hello default of `searchMode=any`, ensuring that `-motel` means "AND NOT" instead of "OR NOT".</span></span> <span data-ttu-id="9c7dd-990">Utan `searchMode=all`, får du ”eller inte” som utökar snarare än begränsar sökresultaten och det kan vara krånglig toosome användare.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-990">Without `searchMode=all`, you get "OR NOT" which expands rather than restricts search results, and this can be counter-intuitive toosome users.</span></span>

15) <span data-ttu-id="9c7dd-991">Hitta dokument i hello index med [lucene frågesyntaxen](https://msdn.microsoft.com/library/mt589323.aspx).</span><span class="sxs-lookup"><span data-stu-id="9c7dd-991">Find documents in hello index using [lucene query syntax](https://msdn.microsoft.com/library/mt589323.aspx).</span></span> <span data-ttu-id="9c7dd-992">Den här frågan returnerar hotell där hello kategorifältet innehåller hello termen ”budget” och alla sökbara fält som innehåller hello frasen ”nyligen renoverade”.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-992">This query returns hotels where hello category field contains hello term "budget" and all searchable fields containing hello phrase "recently renovated".</span></span> <span data-ttu-id="9c7dd-993">Dokument som innehåller hello frasen ”nyligen renoverade” rankas högre på grund av hello termen förstärkningen värde (3)</span><span class="sxs-lookup"><span data-stu-id="9c7dd-993">Documents containing hello phrase "recently renovated" are ranked higher as a result of hello term boost value (3)</span></span>

    <span data-ttu-id="9c7dd-994">GET-/indexes/hotels/docs? Sök = kategori: budget och \"nyligen renoverade\"^ 3 & searchMode = all & api-version = 2015-02-28-Preview & querytype = full</span><span class="sxs-lookup"><span data-stu-id="9c7dd-994">GET /indexes/hotels/docs?search=category:budget AND \"recently renovated\"^3&searchMode=all&api-version=2015-02-28-Preview&querytype=full</span></span>

    <span data-ttu-id="9c7dd-995">POST /indexes/hotels/docs/search? api-version 2015-02-28-Preview = {”Sök” ”: kategori: budget och \"nyligen renoverade\"^ 3”, ”queryType”: ”full”, ”searchMode”: ”alla”}</span><span class="sxs-lookup"><span data-stu-id="9c7dd-995">POST /indexes/hotels/docs/search?api-version=2015-02-28-Preview {   "search": "category:budget AND \"recently renovated\"^3",   "queryType": "full",   "searchMode": "all" }</span></span>

<a name="LookupAPI"></a>

## <a name="lookup-document"></a><span data-ttu-id="9c7dd-996">Sökning dokumentet</span><span class="sxs-lookup"><span data-stu-id="9c7dd-996">Lookup Document</span></span>
<span data-ttu-id="9c7dd-997">Hej **sökning dokumentet** åtgärden hämtar ett dokument från Azure Search.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-997">hello **Lookup Document** operation retrieves a document from Azure Search.</span></span> <span data-ttu-id="9c7dd-998">Detta är användbart när en användare klickar på en specifik sökresultat, och du vill toolook in specifik information om dokumentet.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-998">This is useful when a user clicks on a specific search result, and you want toolook up specific details about that document.</span></span>

    GET https://[service name].search.windows.net/indexes/[index name]/docs/[key]?[query parameters]
    api-key: [admin or query key]

<span data-ttu-id="9c7dd-999">**Förfrågan**</span><span class="sxs-lookup"><span data-stu-id="9c7dd-999">**Request**</span></span>

<span data-ttu-id="9c7dd-1000">HTTPS krävs för tjänstbegäranden.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-1000">HTTPS is required for service requests.</span></span> <span data-ttu-id="9c7dd-1001">Hej **sökning dokumentet** begäran kan konstrueras enligt följande.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-1001">hello **Lookup Document** request can be constructed as follows.</span></span>

    GET /indexes/[index name]/docs/key?[query parameters]

<span data-ttu-id="9c7dd-1002">Du kan också använda hello traditionella OData-syntaxen för nyckelsökningen:</span><span class="sxs-lookup"><span data-stu-id="9c7dd-1002">Alternatively, you can use hello traditional OData syntax for key lookup:</span></span>

    GET /indexes('[index name]')/docs('[key]')?[query parameters]

<span data-ttu-id="9c7dd-1003">hello begäran URI innehåller ett [indexnamn] och [], anger vilka dokumentet tooretrieve från vilka index.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-1003">hello request URI includes an [index name] and [key], specifying which document tooretrieve from which index.</span></span> <span data-ttu-id="9c7dd-1004">Du kan bara hämta ett dokument åt gången.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-1004">You can only get one document at a time.</span></span> <span data-ttu-id="9c7dd-1005">Använd **Sök** tooget flera dokument i en enskild begäran.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-1005">Use **Search** tooget multiple documents in a single request.</span></span>

<span data-ttu-id="9c7dd-1006">**Frågeparametrar**</span><span class="sxs-lookup"><span data-stu-id="9c7dd-1006">**Query Parameters**</span></span>

<span data-ttu-id="9c7dd-1007">`$select=[string]`(valfritt) – en lista över kommaavgränsade fält tooretrieve.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-1007">`$select=[string]` (optional) - a list of comma-separated fields tooretrieve.</span></span> <span data-ttu-id="9c7dd-1008">Om angetts eller har angetts för`*`, alla fält som har markerats som hämtas i hello schemat ingår i hello projektion.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-1008">If unspecified or set too`*`, all fields marked as retrievable in hello schema are included in hello projection.</span></span>

<span data-ttu-id="9c7dd-1009">`api-version=[string]`(krävs).</span><span class="sxs-lookup"><span data-stu-id="9c7dd-1009">`api-version=[string]` (required).</span></span> <span data-ttu-id="9c7dd-1010">hello förhandsversionen är `api-version=2015-02-28-Preview`.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-1010">hello preview version is `api-version=2015-02-28-Preview`.</span></span> <span data-ttu-id="9c7dd-1011">Se [Sök Versionhantering](http://msdn.microsoft.com/library/azure/dn864560.aspx) information och alternativa versioner.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-1011">See [Search Service Versioning](http://msdn.microsoft.com/library/azure/dn864560.aspx) for details and alternative versions.</span></span>

<span data-ttu-id="9c7dd-1012">Obs: För den här åtgärden hello `api-version` har angetts som en frågeparameter.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-1012">Note: For this operation, hello `api-version` is specified as a query parameter.</span></span>

<span data-ttu-id="9c7dd-1013">**Huvuden för begäran**</span><span class="sxs-lookup"><span data-stu-id="9c7dd-1013">**Request Headers**</span></span>

<span data-ttu-id="9c7dd-1014">hello följande lista beskriver hello nödvändiga och valfria begärandehuvuden.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-1014">hello following list describes hello required and optional request headers.</span></span>

* <span data-ttu-id="9c7dd-1015">`api-key`: hello `api-key` är används tooauthenticate hello begäran tooyour Search-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-1015">`api-key`: hello `api-key` is used tooauthenticate hello request tooyour Search service.</span></span> <span data-ttu-id="9c7dd-1016">Det är ett strängvärde, unika tooyour tjänst-URL.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-1016">It is a string value, unique tooyour service URL.</span></span> <span data-ttu-id="9c7dd-1017">Hej **sökning dokumentet** begäran kan ange en administrationsnyckeln eller Frågenyckeln för `api-key`.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-1017">hello **Lookup Document** request can specify either an admin key or query key for `api-key`.</span></span>

<span data-ttu-id="9c7dd-1018">Du måste också hello service name tooconstruct hello begärd URL.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-1018">You will also need hello service name tooconstruct hello request URL.</span></span> <span data-ttu-id="9c7dd-1019">Du kan hämta hello tjänstnamn och `api-key` från instrumentpanelen service i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-1019">You can get hello service name and `api-key` from your service dashboard in hello Azure Portal.</span></span> <span data-ttu-id="9c7dd-1020">Se [skapa en Azure Search-tjänst i hello portal](search-create-service-portal.md) sidan navigering hjälp.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-1020">See [Create an Azure Search service in hello portal](search-create-service-portal.md) for page navigation help.</span></span>

<span data-ttu-id="9c7dd-1021">**Begärandetexten**</span><span class="sxs-lookup"><span data-stu-id="9c7dd-1021">**Request Body**</span></span>

<span data-ttu-id="9c7dd-1022">Ingen.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-1022">None.</span></span>

<span data-ttu-id="9c7dd-1023">**Svar**</span><span class="sxs-lookup"><span data-stu-id="9c7dd-1023">**Response**</span></span>

<span data-ttu-id="9c7dd-1024">Statuskod: 200 OK returneras för ett lyckat svar.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-1024">Status Code: 200 OK is returned for a successful response.</span></span>

    {
      field_name: field_value (fields matching hello default or specified projection)
    }

<span data-ttu-id="9c7dd-1025">**Exempel**</span><span class="sxs-lookup"><span data-stu-id="9c7dd-1025">**Example**</span></span>

<span data-ttu-id="9c7dd-1026">Sökning hello dokument har nyckel '2'</span><span class="sxs-lookup"><span data-stu-id="9c7dd-1026">Lookup hello document that has key '2'</span></span>

    GET /indexes/hotels/docs/2?api-version=2015-02-28-Preview

<span data-ttu-id="9c7dd-1027">Sökning hello dokument har nyckel ”3” med OData-syntax:</span><span class="sxs-lookup"><span data-stu-id="9c7dd-1027">Lookup hello document that has key '3' using OData syntax:</span></span>

    GET /indexes('hotels')/docs('3')?api-version=2015-02-28-Preview

<a name="CountDocs"></a>

## <a name="count-documents"></a><span data-ttu-id="9c7dd-1028">Antal dokument</span><span class="sxs-lookup"><span data-stu-id="9c7dd-1028">Count Documents</span></span>
<span data-ttu-id="9c7dd-1029">Hej **antalet dokument** åtgärden hämtar antalet hello antalet dokument i en sökindex.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-1029">hello **Count Documents** operation retrieves a count of hello number of documents in a search index.</span></span> <span data-ttu-id="9c7dd-1030">Hej `$count` syntax är en del av hello OData-protokollet.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-1030">hello `$count` syntax is part of hello OData protocol.</span></span>

    GET https://[service name].search.windows.net/indexes/[index name]/docs/$count?api-version=[api-version]
    Accept: text/plain
    api-key: [admin or query key]

<span data-ttu-id="9c7dd-1031">**Förfrågan**</span><span class="sxs-lookup"><span data-stu-id="9c7dd-1031">**Request**</span></span>

<span data-ttu-id="9c7dd-1032">HTTPS krävs för tjänstbegäranden.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-1032">HTTPS is required for service requests.</span></span> <span data-ttu-id="9c7dd-1033">Hej **antalet dokument** begäran kan konstrueras med hello GET-metoden.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-1033">hello **Count Documents** request can be constructed using hello GET method.</span></span>

<span data-ttu-id="9c7dd-1034">Hej [index name] i hello Begärd URI visar hello service tooreturn en uppräkning av alla objekt i hello docs samling hello angivet index.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-1034">hello [index name] in hello request URI tells hello service tooreturn a count of all items in hello docs collection of hello specified index.</span></span>

<span data-ttu-id="9c7dd-1035">`api-version=[string]`(krävs).</span><span class="sxs-lookup"><span data-stu-id="9c7dd-1035">`api-version=[string]` (required).</span></span> <span data-ttu-id="9c7dd-1036">hello förhandsversionen är `api-version=2015-02-28-Preview`.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-1036">hello preview version is `api-version=2015-02-28-Preview`.</span></span> <span data-ttu-id="9c7dd-1037">Se [Sök Versionhantering](http://msdn.microsoft.com/library/azure/dn864560.aspx) information och alternativa versioner.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-1037">See [Search Service Versioning](http://msdn.microsoft.com/library/azure/dn864560.aspx) for details and alternative versions.</span></span>

<span data-ttu-id="9c7dd-1038">**Huvuden för begäran**</span><span class="sxs-lookup"><span data-stu-id="9c7dd-1038">**Request Headers**</span></span>

<span data-ttu-id="9c7dd-1039">hello följande lista beskriver hello nödvändiga och valfria begärandehuvuden.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-1039">hello following list describes hello required and optional request headers.</span></span>

* <span data-ttu-id="9c7dd-1040">`Accept`: Det här värdet måste anges för`text/plain`.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-1040">`Accept`: This value must be set too`text/plain`.</span></span>
* <span data-ttu-id="9c7dd-1041">`api-key`: hello `api-key` är används tooauthenticate hello begäran tooyour Search-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-1041">`api-key`: hello `api-key` is used tooauthenticate hello request tooyour Search service.</span></span> <span data-ttu-id="9c7dd-1042">Det är ett strängvärde, unika tooyour tjänst-URL.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-1042">It is a string value, unique tooyour service URL.</span></span> <span data-ttu-id="9c7dd-1043">Hej **antalet dokument** begäran kan ange en administrationsnyckeln eller Frågenyckeln för `api-key`.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-1043">hello **Count Documents** request can specify either an admin key or query key for `api-key`.</span></span>

<span data-ttu-id="9c7dd-1044">Du måste också hello service name tooconstruct hello begärd URL.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-1044">You will also need hello service name tooconstruct hello request URL.</span></span> <span data-ttu-id="9c7dd-1045">Du kan hämta hello tjänstnamn och `api-key` från instrumentpanelen service i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-1045">You can get hello service name and `api-key` from your service dashboard in hello Azure Portal.</span></span> <span data-ttu-id="9c7dd-1046">Se [skapa en Azure Search-tjänst i hello portal](search-create-service-portal.md) sidan navigering hjälp.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-1046">See [Create an Azure Search service in hello portal](search-create-service-portal.md) for page navigation help.</span></span>

<span data-ttu-id="9c7dd-1047">**Begärandetexten**</span><span class="sxs-lookup"><span data-stu-id="9c7dd-1047">**Request Body**</span></span>

<span data-ttu-id="9c7dd-1048">Ingen.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-1048">None.</span></span>

<span data-ttu-id="9c7dd-1049">**Svar**</span><span class="sxs-lookup"><span data-stu-id="9c7dd-1049">**Response**</span></span>

<span data-ttu-id="9c7dd-1050">Statuskod: 200 OK returneras för ett lyckat svar.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-1050">Status Code: 200 OK is returned for a successful response.</span></span>

<span data-ttu-id="9c7dd-1051">hello svarstexten innehåller hello count-värdet som ett heltal som är formaterad med oformaterad text.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-1051">hello response body contains hello count value as an integer formatted in plain text.</span></span>

<a name="Suggestions"></a>

## <a name="suggestions"></a><span data-ttu-id="9c7dd-1052">Förslag</span><span class="sxs-lookup"><span data-stu-id="9c7dd-1052">Suggestions</span></span>
<span data-ttu-id="9c7dd-1053">Hej **förslag** åtgärden hämtar förslag baserat på partiell sökning indata.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-1053">hello **Suggestions** operation retrieves suggestions based on partial search input.</span></span> <span data-ttu-id="9c7dd-1054">Den används vanligtvis i rutorna tooprovide typ-ahead sökförslag som användare skriver in söktermer.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-1054">It's typically used in search boxes tooprovide type-ahead suggestions as users are entering search terms.</span></span>

<span data-ttu-id="9c7dd-1055">Förslag begäranden syftet förslag på måldokument som, så hello förslag text upprepas om flera kandidatdokument matchar hello samma söka i indata.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-1055">Suggestion requests aim at suggesting target documents, so hello suggested text may be repeated if multiple candidate documents match hello same search input.</span></span> <span data-ttu-id="9c7dd-1056">Du kan använda `$select` tooretrieve andra dokumentera fält (inklusive hello Dokumentnyckel) så att du kan se vilka dokumentet är hello källa för varje förslag.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-1056">You can use `$select` tooretrieve other document fields (including hello document key) so that you can tell which document is hello source for each suggestion.</span></span>

<span data-ttu-id="9c7dd-1057">En **förslag** utfärdas igen som en GET eller POST-begäran.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-1057">A **Suggestions** operation is issued as a GET or POST request.</span></span>

    GET https://[service name].search.windows.net/indexes/[index name]/docs/suggest?[query parameters]
    api-key: [admin or query key]

    POST https://[service name].search.windows.net/indexes/[index name]/docs/suggest?api-version=[api-version]
    Content-Type: application/json
    api-key: [admin or query key]

<span data-ttu-id="9c7dd-1058">**När toouse bokför i stället för att hämta**</span><span class="sxs-lookup"><span data-stu-id="9c7dd-1058">**When toouse POST instead of GET**</span></span>

<span data-ttu-id="9c7dd-1059">När du använder HTTP GET toocall hello **förslag** API, behöver du toobe medveten om att hello hello URL: en inte kan vara längre än 8 KB.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-1059">When you use HTTP GET toocall hello **Suggestions** API, you need toobe aware that hello length of hello request URL cannot exceed 8 KB.</span></span> <span data-ttu-id="9c7dd-1060">Detta är vanligtvis tillräckligt för de flesta program.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-1060">This is usually enough for most applications.</span></span> <span data-ttu-id="9c7dd-1061">Men genererar vissa program mycket stora frågor, speciellt OData filteruttryck.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-1061">However, some applications produce very large queries, specifically OData filter expressions.</span></span> <span data-ttu-id="9c7dd-1062">För dessa program är med hjälp av HTTP POST ett bättre alternativ eftersom det tillåter filter med större än GET.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-1062">For these applications, using HTTP POST is a better choice because it allows larger filters than GET.</span></span> <span data-ttu-id="9c7dd-1063">Med INLÄGGET hello-satser i ett filter är hello begränsande faktor, hello inte storleken på hello rådata Filtersträngen eftersom hello begäran storleksgräns för POST är ungefär 16 MB.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-1063">With POST, hello number of clauses in a filter is hello limiting factor, not hello size of hello raw filter string since hello request size limit for POST is approximately 16 MB.</span></span>

> [!NOTE]
> <span data-ttu-id="9c7dd-1064">Även om storleksgränsen för hello POST-begäran är väldigt stor får inte filteruttryck vara godtyckligt komplexa.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-1064">Even though hello POST request size limit is very large, filter expressions cannot be arbitrarily complex.</span></span> <span data-ttu-id="9c7dd-1065">Se [OData-uttryckssyntax](https://msdn.microsoft.com/library/dn798921.aspx) för mer information om begränsningar för filter komplexitet.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-1065">See [OData expression syntax](https://msdn.microsoft.com/library/dn798921.aspx) for more information about filter complexity limitations.</span></span>
> 
> 

<span data-ttu-id="9c7dd-1066">**Förfrågan**</span><span class="sxs-lookup"><span data-stu-id="9c7dd-1066">**Request**</span></span>

<span data-ttu-id="9c7dd-1067">HTTPS krävs för tjänstbegäranden.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-1067">HTTPS is required for service requests.</span></span> <span data-ttu-id="9c7dd-1068">Hej **förslag** begäran kan konstrueras med hello GET eller POST-metoderna.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-1068">hello **Suggestions** request can be constructed using hello GET or POST methods.</span></span>

<span data-ttu-id="9c7dd-1069">URI-begäran hello anger hello index tooquery hello namn.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-1069">hello request URI specifies hello name of hello index tooquery.</span></span> <span data-ttu-id="9c7dd-1070">Parametrar, till exempel hello delvis inkommande sökterm anges på hello frågesträngen hello gäller GET-begäranden och i hello begäran text i hello skiftläget för POST-begäranden.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-1070">Parameters, such as hello partially input search term, are specified on hello query string in hello case of GET requests, and in hello request body in hello case of POST requests.</span></span>

<span data-ttu-id="9c7dd-1071">Ett bra tips när du skapar GET-begäranden, spara för[URL-koda](https://msdn.microsoft.com/library/system.uri.escapedatastring.aspx) specifik fråga parametrar när du anropar hello REST API direkt.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-1071">As a best practice when creating GET requests, remember too[URL-encode](https://msdn.microsoft.com/library/system.uri.escapedatastring.aspx) specific query parameters when calling hello REST API directly.</span></span> <span data-ttu-id="9c7dd-1072">För **förslag** åtgärder, detta omfattar:</span><span class="sxs-lookup"><span data-stu-id="9c7dd-1072">For **Suggestions** operations, this includes:</span></span>

* `$filter`
* `highlightPreTag`
* `highlightPostTag`
* `search`

<span data-ttu-id="9c7dd-1073">URL-kodning rekommenderas endast på hello ovan Frågeparametrar.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-1073">URL encoding is only recommended on hello above query parameters.</span></span> <span data-ttu-id="9c7dd-1074">Om du av misstag URL-koda hello hela frågesträng (allt efter hello?) bryts begäranden.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-1074">If you inadvertently URL-encode hello entire query string (everything after hello ?), requests will break.</span></span>

<span data-ttu-id="9c7dd-1075">Dessutom krävs URL-kodning bara när du anropar hello REST API: et direkt med komma.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-1075">Also, URL encoding is only necessary when calling hello REST API directly using GET.</span></span> <span data-ttu-id="9c7dd-1076">Ingen URL-kodning krävs när du anropar **förslag** med POST, eller när du använder hello [.NET-klientbibliotek](https://msdn.microsoft.com/library/dn951165.aspx), som hanterar URL-kodning för dig.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-1076">No URL encoding is necessary when calling **Suggestions** using POST, or when using hello [.NET client library](https://msdn.microsoft.com/library/dn951165.aspx), which handles URL encoding for you.</span></span>

<span data-ttu-id="9c7dd-1077">**Frågeparametrar**</span><span class="sxs-lookup"><span data-stu-id="9c7dd-1077">**Query Parameters**</span></span>

<span data-ttu-id="9c7dd-1078">**Förslag** accepterar flera parametrar som tillhandahåller frågevillkor och även ange sökbeteendet.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-1078">**Suggestions** accepts several parameters that provide query criteria and also specify search behavior.</span></span> <span data-ttu-id="9c7dd-1079">Du anger dessa parametrar i hello URL: en frågesträng vid anrop av **förslag** via GET och som JSON-egenskaper i hello begärandetexten vid anrop av **förslag** via POST.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-1079">You provide these parameters in hello URL query string when calling **Suggestions** via GET, and as JSON properties in hello request body when calling **Suggestions** via POST.</span></span> <span data-ttu-id="9c7dd-1080">hello syntax för vissa parametrar är något annorlunda mellan GET och POST.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-1080">hello syntax for some parameters is slightly different between GET and POST.</span></span> <span data-ttu-id="9c7dd-1081">Skillnaderna beskrivs i tillämpliga fall nedan:</span><span class="sxs-lookup"><span data-stu-id="9c7dd-1081">These differences are noted as applicable below:</span></span>

<span data-ttu-id="9c7dd-1082">`search=[string]`-hello text toouse toosuggest sökningar.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-1082">`search=[string]` - hello search text toouse toosuggest queries.</span></span> <span data-ttu-id="9c7dd-1083">Måste vara minst 1 tecken och högst 100 tecken.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-1083">Must be at least 1 character, and no more than 100 characters.</span></span>

<span data-ttu-id="9c7dd-1084">`highlightPreTag=[string]`(valfritt) – ett sträng-tagg som läggs toosearch träffar.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-1084">`highlightPreTag=[string]` (optional) - a string tag that prepends toosearch hits.</span></span> <span data-ttu-id="9c7dd-1085">Måste anges med `highlightPostTag`.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-1085">Must be set with `highlightPostTag`.</span></span>

> [!NOTE]
> <span data-ttu-id="9c7dd-1086">När du anropar **förslag** använder GET, reserverade tecken i hello URL måste vara procent-kodat (till exempel % 23 i stället för #).</span><span class="sxs-lookup"><span data-stu-id="9c7dd-1086">When calling **Suggestions** using GET, reserved characters in hello URL must be percent-encoded (for example, %23 instead of #).</span></span>
> 
> 

<span data-ttu-id="9c7dd-1087">`highlightPostTag=[string]`(valfritt) – ett sträng-tagg som lägger till toosearch träffar.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-1087">`highlightPostTag=[string]` (optional) - a string tag that appends toosearch hits.</span></span> <span data-ttu-id="9c7dd-1088">Måste anges med `highlightPreTag`.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-1088">Must be set with `highlightPreTag`.</span></span>

> [!NOTE]
> <span data-ttu-id="9c7dd-1089">När du anropar **förslag** använder GET, reserverade tecken i hello URL måste vara procent-kodat (till exempel % 23 i stället för #).</span><span class="sxs-lookup"><span data-stu-id="9c7dd-1089">When calling **Suggestions** using GET, reserved characters in hello URL must be percent-encoded (for example, %23 instead of #).</span></span>
> 
> 

<span data-ttu-id="9c7dd-1090">`suggesterName=[string]`-hello namnet på hello förslagsställarens som anges i hello `suggesters` samling som är en del av hello indexdefinitionen.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-1090">`suggesterName=[string]` - hello name of hello suggester as specified in hello `suggesters` collection that's part of hello index definition.</span></span> <span data-ttu-id="9c7dd-1091">En `suggester` bestämmer vilka fält som genomsöks för föreslagna sökord.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-1091">A `suggester` determines which fields are scanned for suggested query terms.</span></span> <span data-ttu-id="9c7dd-1092">Se [Suggesters](#Suggesters) mer information.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-1092">See [Suggesters](#Suggesters) for details.</span></span>

<span data-ttu-id="9c7dd-1093">`fuzzy=[boolean]`(valfritt, som standard = false)-tootrue när ange detta API hittar förslag även om det finns en saknas eller ersatta tecken i hello söktexten.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-1093">`fuzzy=[boolean]` (optional, default = false) - when set tootrue this API will find suggestions even if there's a substituted or missing character in hello search text.</span></span> <span data-ttu-id="9c7dd-1094">Detta ger en bättre upplevelse i vissa fall gäller det vid en prestandakostnad eftersom fuzzy förslag sökningar är långsammare och förbruka mer resurser.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-1094">While this provides a better experience in some scenarios it comes at a performance cost as fuzzy suggestion searches are slower and consume more resources.</span></span>

<span data-ttu-id="9c7dd-1095">`searchFields=[string]`(valfritt) – Ange hello lista med CSV-fältet namn toosearch för hello söktexten.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-1095">`searchFields=[string]` (optional) - hello list of comma-separated field names toosearch for hello specified search text.</span></span> <span data-ttu-id="9c7dd-1096">Målfält måste vara aktiverat för förslag.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-1096">Target fields must be enabled for suggestions.</span></span>

<span data-ttu-id="9c7dd-1097">`$top=#`(valfritt, som standard = 5)-hello antal förslag tooretrieve.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-1097">`$top=#` (optional, default = 5) - hello number of suggestions tooretrieve.</span></span> <span data-ttu-id="9c7dd-1098">Måste vara ett tal mellan 1 och 100.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-1098">Must be a number between 1 and 100.</span></span>

> [!NOTE]
> <span data-ttu-id="9c7dd-1099">När du anropar **förslag** med efter den här parametern heter `top` i stället för `$top`.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-1099">When calling **Suggestions** using POST, this parameter is named `top` instead of `$top`.</span></span>
> 
> 

<span data-ttu-id="9c7dd-1100">`$filter=[string]`(valfritt) - uttryck som filtrerar hello dokument anses vara förslag.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-1100">`$filter=[string]` (optional) - an expression that filters hello documents considered for suggestions.</span></span>

> [!NOTE]
> <span data-ttu-id="9c7dd-1101">När du anropar **förslag** med efter den här parametern heter `filter` i stället för `$filter`.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-1101">When calling **Suggestions** using POST, this parameter is named `filter` instead of `$filter`.</span></span>
> 
> 

<span data-ttu-id="9c7dd-1102">`$orderby=[string]`(valfritt) – en lista över uttryck avgränsade med kommatecken toosort hello resultat av.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-1102">`$orderby=[string]` (optional) - a list of comma-separated expressions toosort hello results by.</span></span> <span data-ttu-id="9c7dd-1103">Varje uttryck kan vara ett fältnamn eller ett anrop toohello `geo.distance()` funktion.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-1103">Each expression can be either a field name or a call toohello `geo.distance()` function.</span></span> <span data-ttu-id="9c7dd-1104">Varje uttryck kan följas av `asc` tooindicated stigande ordning och `desc` tooindicate fallande.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-1104">Each expression can be followed by `asc` tooindicated ascending, and `desc` tooindicate descending.</span></span> <span data-ttu-id="9c7dd-1105">hello standard är stigande ordning.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-1105">hello default is ascending order.</span></span> <span data-ttu-id="9c7dd-1106">Det finns en gräns på 32-satser för `$orderby`.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-1106">There is a limit of 32 clauses for `$orderby`.</span></span>

> [!NOTE]
> <span data-ttu-id="9c7dd-1107">När du anropar **förslag** med efter den här parametern heter `orderby` i stället för `$orderby`.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-1107">When calling **Suggestions** using POST, this parameter is named `orderby` instead of `$orderby`.</span></span>
> 
> 

<span data-ttu-id="9c7dd-1108">`$select=[string]`(valfritt) – en lista över kommaavgränsade fält tooretrieve.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-1108">`$select=[string]` (optional) - a list of comma-separated fields tooretrieve.</span></span> <span data-ttu-id="9c7dd-1109">Om inget anges endast hello Dokumentnyckel och förslag returneras.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-1109">If unspecified, only hello document key and suggestion text is returned.</span></span> <span data-ttu-id="9c7dd-1110">Du kan uttryckligen begära alla fält genom att ange den här parametern för`*`.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-1110">You can explicitly request all fields by setting this parameter too`*`.</span></span>

> [!NOTE]
> <span data-ttu-id="9c7dd-1111">När du anropar **förslag** med efter den här parametern heter `select` i stället för `$select`.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-1111">When calling **Suggestions** using POST, this parameter is named `select` instead of `$select`.</span></span>
> 
> 

<span data-ttu-id="9c7dd-1112">`minimumCoverage`(valfritt, som standard too80) - ett tal mellan 0 och 100 som visar hello procentandelen hello index som måste omfattas av en fråga förslag för hello frågan toobe rapporteras som lyckas.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-1112">`minimumCoverage` (optional, defaults too80) - a number between 0 and 100 indicating hello percentage of hello index that must be covered by a suggestions query in order for hello query toobe reported as a success.</span></span> <span data-ttu-id="9c7dd-1113">Som standard minst 80% av hello index måste vara tillgängliga eller `Suggest` returneras HTTP-statuskoden 503.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-1113">By default, at least 80% of hello index must be available or `Suggest` will return HTTP status code 503.</span></span> <span data-ttu-id="9c7dd-1114">Om du ställer in `minimumCoverage` och `Suggest` lyckas den ska returnera HTTP 200 och innehålla en `@search.coverage` värdet i hello-svar som visar hello procentandelen hello index som ingick i hello-frågan.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-1114">If you set `minimumCoverage` and `Suggest` succeeds, it will return HTTP 200 and include a `@search.coverage` value in hello response indicating hello percentage of hello index that was included in hello query.</span></span>

> [!NOTE]
> <span data-ttu-id="9c7dd-1115">Om parametern tooa värdet ska vara mindre än 100 kan vara användbart för att söka efter tillgänglighet även för tjänster med bara en replik.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-1115">Setting this parameter tooa value lower than 100 can be useful for ensuring search availability even for services with only one replica.</span></span> <span data-ttu-id="9c7dd-1116">Inga matchande förslag är dock garanterat toobe finns i hello resultat.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-1116">However, not all matching suggestions are guaranteed toobe present in hello results.</span></span> <span data-ttu-id="9c7dd-1117">Om återkallning är viktigare tooyour program än tillgänglighet och det är bästa inte toolower `minimumCoverage` nedan standardvärdet 80.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-1117">If recall is more important tooyour application than availability, then it's best not toolower `minimumCoverage` below its default value of 80.</span></span>
> 
> 

<span data-ttu-id="9c7dd-1118">`api-version=[string]`(krävs).</span><span class="sxs-lookup"><span data-stu-id="9c7dd-1118">`api-version=[string]` (required).</span></span> <span data-ttu-id="9c7dd-1119">hello förhandsversionen är `api-version=2015-02-28-Preview`.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-1119">hello preview version is `api-version=2015-02-28-Preview`.</span></span> <span data-ttu-id="9c7dd-1120">Se [Sök Versionhantering](http://msdn.microsoft.com/library/azure/dn864560.aspx) information och alternativa versioner.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-1120">See [Search Service Versioning](http://msdn.microsoft.com/library/azure/dn864560.aspx) for details and alternative versions.</span></span>

<span data-ttu-id="9c7dd-1121">Obs: För den här åtgärden hello `api-version` har angetts som en frågeparameter i hello URL oavsett om du anropar **förslag** med GET eller POST.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-1121">Note: For this operation, hello `api-version` is specified as a query parameter in hello URL regardless of whether you call **Suggestions** with GET or POST.</span></span>

<span data-ttu-id="9c7dd-1122">**Huvuden för begäran**</span><span class="sxs-lookup"><span data-stu-id="9c7dd-1122">**Request Headers**</span></span>

<span data-ttu-id="9c7dd-1123">hello följande lista beskriver hello nödvändiga och valfria begärandehuvuden</span><span class="sxs-lookup"><span data-stu-id="9c7dd-1123">hello following list describes hello required and optional request headers</span></span>

* <span data-ttu-id="9c7dd-1124">`api-key`: hello `api-key` är används tooauthenticate hello begäran tooyour Search-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-1124">`api-key`: hello `api-key` is used tooauthenticate hello request tooyour Search service.</span></span> <span data-ttu-id="9c7dd-1125">Det är ett strängvärde, unika tooyour tjänst-URL.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-1125">It is a string value, unique tooyour service URL.</span></span> <span data-ttu-id="9c7dd-1126">Hej **förslag** begäran kan ange en administrationsnyckeln eller Frågenyckeln som hello `api-key`.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-1126">hello **Suggestions** request can specify either an admin key or query key as hello `api-key`.</span></span>

<span data-ttu-id="9c7dd-1127">Du måste också hello service name tooconstruct hello begärd URL.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-1127">You will also need hello service name tooconstruct hello request URL.</span></span> <span data-ttu-id="9c7dd-1128">Du kan hämta hello tjänstnamn och `api-key` från instrumentpanelen service i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-1128">You can get hello service name and `api-key` from your service dashboard in hello Azure Portal.</span></span> <span data-ttu-id="9c7dd-1129">Se [skapa en Azure Search-tjänst i hello portal](search-create-service-portal.md) sidan navigering hjälp.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-1129">See [Create an Azure Search service in hello portal](search-create-service-portal.md) for page navigation help.</span></span>

<span data-ttu-id="9c7dd-1130">**Begärandetexten**</span><span class="sxs-lookup"><span data-stu-id="9c7dd-1130">**Request Body**</span></span>

<span data-ttu-id="9c7dd-1131">För GET: Ingen.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-1131">For GET: None.</span></span>

<span data-ttu-id="9c7dd-1132">För POST:</span><span class="sxs-lookup"><span data-stu-id="9c7dd-1132">For POST:</span></span>

    {
      "filter": "odata_filter_expression",
      "fuzzy": true | false (default),
      "highlightPreTag": "pre_tag",
      "highlightPostTag": "post_tag",
      "minimumCoverage": # (% of index that must be covered toodeclare query successful; default 80),
      "orderby": "orderby_expression",
      "search": "partial_search_input",
      "searchFields": "field_name_1, field_name_2, ...",
      "select": "field_name_1, field_name_2, ...",
      "suggesterName": "suggester_name",
      "top": # (default 5)
    }

<span data-ttu-id="9c7dd-1133">**Svar**</span><span class="sxs-lookup"><span data-stu-id="9c7dd-1133">**Response**</span></span>

<span data-ttu-id="9c7dd-1134">Statuskod: 200 OK returneras för ett lyckat svar.</span><span class="sxs-lookup"><span data-stu-id="9c7dd-1134">Status Code: 200 OK is returned for a successful response.</span></span>

    {
      "@search.coverage": # (if minimumCoverage was provided in hello query),
      "value": [
        {
          "@search.text": "...",
          "<key field>": "..."
        },
        ...
      ]
    }

<span data-ttu-id="9c7dd-1135">Om hello projektion alternativet används tooretrieve fält de ingår i varje element i matrisen hello:</span><span class="sxs-lookup"><span data-stu-id="9c7dd-1135">If hello projection option is used tooretrieve fields they are included in each element of hello array:</span></span>

    {
      "@search.coverage": # (if minimumCoverage was provided in hello query),
      "value": [
        {
          "@search.text": "...",
          "<key field>": "..."
          <other projected data fields>
        },
        ...
      ]
    }

<span data-ttu-id="9c7dd-1136">**Exempel**</span><span class="sxs-lookup"><span data-stu-id="9c7dd-1136">**Example**</span></span>

<span data-ttu-id="9c7dd-1137">Hämta 5 förslag där hello partiell sökning indata är 'lux'</span><span class="sxs-lookup"><span data-stu-id="9c7dd-1137">Retrieve 5 suggestions where hello partial search input is 'lux'</span></span>

    GET /indexes/hotels/docs/suggest?search=lux&$top=5&suggesterName=sg&api-version=2015-02-28-Preview

    POST /indexes/hotels/docs/suggest?api-version=2015-02-28-Preview
    {
      "search": "lux",
      "top": 5,
      "suggesterName": "sg"
    }
