---
title: "Azure Search-tjänsten REST API-Version 2015-02-28-Preview | Microsoft Docs"
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
ms.openlocfilehash: e6ad5c964bfa8421be2706cb4015980e01a271b7
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="azure-search-service-rest-api-version-2015-02-28-preview"></a><span data-ttu-id="1a5b1-103">Azure Söktjänsts-REST API: Version 2015-02-28-Preview</span><span class="sxs-lookup"><span data-stu-id="1a5b1-103">Azure Search Service REST API: Version 2015-02-28-Preview</span></span>
<span data-ttu-id="1a5b1-104">Den här artikeln är referensdokumentationen för `api-version=2015-02-28-Preview`.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-104">This article is the reference documentation for `api-version=2015-02-28-Preview`.</span></span> <span data-ttu-id="1a5b1-105">Den här förhandsgranskningen utökar den aktuella allmänt tillgängliga versionen [api-version = 2015-02-28](https://msdn.microsoft.com/library/dn798935.aspx), genom att tillhandahålla experiment följande funktioner:</span><span class="sxs-lookup"><span data-stu-id="1a5b1-105">This preview extends the current generally available version, [api-version=2015-02-28](https://msdn.microsoft.com/library/dn798935.aspx), by providing the following experimental features:</span></span>

* <span data-ttu-id="1a5b1-106">`moreLikeThis`Frågeparametern i den [Sök dokument](#SearchDocs) API.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-106">`moreLikeThis` query parameter in the [Search Documents](#SearchDocs) API.</span></span> <span data-ttu-id="1a5b1-107">Den söker efter andra dokument som är relevanta för ett visst dokument.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-107">It finds other documents that are relevant to another specific document.</span></span>

<span data-ttu-id="1a5b1-108">Några ytterligare delar av den `2015-02-28-Preview` REST API dokumenteras separat.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-108">A few additional parts of the `2015-02-28-Preview` REST API are documented separately.</span></span> <span data-ttu-id="1a5b1-109">Exempel på dessa är:</span><span class="sxs-lookup"><span data-stu-id="1a5b1-109">These include:</span></span>

* [<span data-ttu-id="1a5b1-110">Bedömningsprofil profiler</span><span class="sxs-lookup"><span data-stu-id="1a5b1-110">Scoring Profiles</span></span>](search-api-scoring-profiles-2015-02-28-preview.md)
* [<span data-ttu-id="1a5b1-111">Indexerare</span><span class="sxs-lookup"><span data-stu-id="1a5b1-111">Indexers</span></span>](search-api-indexers-2015-02-28-preview.md)

<span data-ttu-id="1a5b1-112">Azure Search-tjänsten är tillgänglig i flera versioner.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-112">Azure Search service is available in multiple versions.</span></span> <span data-ttu-id="1a5b1-113">Se [Sök Versionhantering](http://msdn.microsoft.com/library/azure/dn864560.aspx) mer information.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-113">Please refer to [Search Service Versioning](http://msdn.microsoft.com/library/azure/dn864560.aspx) for details.</span></span>

## <a name="apis-in-this-document"></a><span data-ttu-id="1a5b1-114">API: er i det här dokumentet</span><span class="sxs-lookup"><span data-stu-id="1a5b1-114">APIs in this document</span></span>
<span data-ttu-id="1a5b1-115">API för Azure Search-tjänsten stöder två URL-syntax för API: et: enkelt och OData (se [stöd för OData (Azure Search-API)](http://msdn.microsoft.com/library/azure/dn798932.aspx) information).</span><span class="sxs-lookup"><span data-stu-id="1a5b1-115">Azure Search service API supports two URL syntaxes for API operations: simple and OData (see [Support for OData (Azure Search API)](http://msdn.microsoft.com/library/azure/dn798932.aspx) for details).</span></span> <span data-ttu-id="1a5b1-116">I följande lista visas enkel syntax.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-116">The following list shows the simple syntax.</span></span>

[<span data-ttu-id="1a5b1-117">Skapa Index</span><span class="sxs-lookup"><span data-stu-id="1a5b1-117">Create Index</span></span>](#CreateIndex)

    POST /indexes?api-version=2015-02-28-Preview

[<span data-ttu-id="1a5b1-118">Uppdatera Index</span><span class="sxs-lookup"><span data-stu-id="1a5b1-118">Update Index</span></span>](#UpdateIndex)

    PUT /indexes/[index name]?api-version=2015-02-28-Preview

[<span data-ttu-id="1a5b1-119">Hämta Index</span><span class="sxs-lookup"><span data-stu-id="1a5b1-119">Get Index</span></span>](#GetIndex)

    GET /indexes/[index name]?api-version=2015-02-28-Preview

[<span data-ttu-id="1a5b1-120">Visar en lista över index</span><span class="sxs-lookup"><span data-stu-id="1a5b1-120">Listing Indexes</span></span>](#ListIndexes)

    GET /indexes?api-version=2015-02-28-Preview

[<span data-ttu-id="1a5b1-121">Få indexstatistik</span><span class="sxs-lookup"><span data-stu-id="1a5b1-121">Get Index Statistics</span></span>](#GetIndexStats)

    GET /indexes/[index name]/stats?api-version=2015-02-28-Preview

[<span data-ttu-id="1a5b1-122">Testa Analyzer</span><span class="sxs-lookup"><span data-stu-id="1a5b1-122">Test Analyzer</span></span>](#TestAnalyzer)

    POST /indexes/[index name]/analyze?api-version=2015-02-28-Preview

[<span data-ttu-id="1a5b1-123">Ta bort ett Index</span><span class="sxs-lookup"><span data-stu-id="1a5b1-123">Delete an Index</span></span>](#DeleteIndex)

    DELETE /indexes/[index name]?api-version=2015-02-28-Preview

[<span data-ttu-id="1a5b1-124">Lägga till, ta bort, och uppdatera Data i ett Index</span><span class="sxs-lookup"><span data-stu-id="1a5b1-124">Add, Delete, and Update Data within an Index</span></span>](#AddOrUpdateDocuments)

    POST /indexes/[index name]/docs/index?api-version=2015-02-28-Preview

[<span data-ttu-id="1a5b1-125">Sök igenom dokument</span><span class="sxs-lookup"><span data-stu-id="1a5b1-125">Search Documents</span></span>](#SearchDocs)

    GET /indexes/[index name]/docs?[query parameters]
    POST /indexes/[index name]/docs/search?api-version=2015-02-28-Preview

[<span data-ttu-id="1a5b1-126">Sökning dokumentet</span><span class="sxs-lookup"><span data-stu-id="1a5b1-126">Lookup Document</span></span>](#LookupAPI)

     GET /indexes/[index name]/docs/[key]?[query parameters]

[<span data-ttu-id="1a5b1-127">Antal dokument</span><span class="sxs-lookup"><span data-stu-id="1a5b1-127">Count Documents</span></span>](#CountDocs)

    GET /indexes/[index name]/docs/$count?api-version=2015-02-28-Preview

[<span data-ttu-id="1a5b1-128">Förslag</span><span class="sxs-lookup"><span data-stu-id="1a5b1-128">Suggestions</span></span>](#Suggestions)

    GET /indexes/[index name]/docs/suggest?[query parameters]
    POST /indexes/[index name]/docs/suggest?api-version=2015-02-28-Preview

- - -
<a name="IndexOps"></a>

## <a name="index-operations"></a><span data-ttu-id="1a5b1-129">Indexåtgärder</span><span class="sxs-lookup"><span data-stu-id="1a5b1-129">Index Operations</span></span>
<span data-ttu-id="1a5b1-130">Du kan skapa och hantera index i Azure Search-tjänsten via enkel HTTP-begäranden (POST, GET, PUT, DELETE) mot en resurs med angivet index.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-130">You can create and manage indexes in Azure Search service via simple HTTP requests (POST, GET, PUT, DELETE) against a given index resource.</span></span> <span data-ttu-id="1a5b1-131">Om du vill skapa ett index efter du först ett JSON-dokument som beskriver indexeringsschema.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-131">To create an index, you first POST a JSON document that describes the index schema.</span></span> <span data-ttu-id="1a5b1-132">Schemat definierar fälten för indexet sina datatyper och hur de kan användas (t.ex, i fulltextsökningar, filter, sortering eller faceting).</span><span class="sxs-lookup"><span data-stu-id="1a5b1-132">The schema defines the fields of the index, their data types, and how they can be used (for example, in full-text searches, filters, sorting, or faceting).</span></span> <span data-ttu-id="1a5b1-133">Den definierar även bedömningsprofil profiler, suggesters och andra attribut för att konfigurera beteendet för indexet.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-133">It also defines scoring profiles, suggesters and other attributes to configure the behavior of the index.</span></span>

<span data-ttu-id="1a5b1-134">I följande exempel innehåller en illustration av ett schema som används för att söka på hotellinformation med beskrivningsfältet som definierats i två språk.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-134">The following example provides an illustration of a schema used for searching on hotel information with the Description field defined in two languages.</span></span> <span data-ttu-id="1a5b1-135">Observera hur attribut styr hur fältet används.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-135">Notice how attributes control how the field is used.</span></span> <span data-ttu-id="1a5b1-136">Till exempel den `hotelId` används som Dokumentnyckel (`"key": true`) och har exkluderats från fulltextsökningar (`"searchable": false`).</span><span class="sxs-lookup"><span data-stu-id="1a5b1-136">For example the `hotelId` is used as the document key (`"key": true`) and is excluded from full-text searches (`"searchable": false`).</span></span>

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

<span data-ttu-id="1a5b1-137">När indexet har skapats måste du överföra dokument som fyller i indexet.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-137">After the index is created, you'll upload documents that populate the index.</span></span> <span data-ttu-id="1a5b1-138">Se [Lägg till eller uppdatera dokument](#AddOrUpdateDocuments) för nästa steg.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-138">See [Add or Update Documents](#AddOrUpdateDocuments) for this next step.</span></span>

<span data-ttu-id="1a5b1-139">En videopresentation indexering i Azure Search, finns det [Channel 9 molnet omfattar avsnitt på Azure Search](http://go.microsoft.com/fwlink/p/?LinkId=511509).</span><span class="sxs-lookup"><span data-stu-id="1a5b1-139">For a video introduction to indexing in Azure Search, see the [Channel 9 Cloud Cover episode on Azure Search](http://go.microsoft.com/fwlink/p/?LinkId=511509).</span></span>

<a name="CreateIndex"></a>

## <a name="create-index"></a><span data-ttu-id="1a5b1-140">Skapa index</span><span class="sxs-lookup"><span data-stu-id="1a5b1-140">Create Index</span></span>
<span data-ttu-id="1a5b1-141">Ett index är primärt ordna och söka dokument i Azure Search liknar hur en tabell ordnar poster i en databas.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-141">An index is the primary means of organizing and searching documents in Azure Search, similar to how a table organizes records in a database.</span></span> <span data-ttu-id="1a5b1-142">Varje indexet innehåller en samling dokument att alla motsvara indexeringsschema (fältnamn, datatyper och egenskaper), men index också ange ytterligare konstruktioner (suggesters, bedömningsprofil profiler och alternativ för CORS) som definierar andra Sök beteenden.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-142">Each index has a collection of documents that all conform to the index schema (field names, data types, and properties), but indexes also specify additional constructs (suggesters, scoring profiles, and CORS options) that define other search behaviors.</span></span>

<span data-ttu-id="1a5b1-143">Du kan skapa ett nytt index i en Azure Search-tjänst med en HTTP POST eller PUT-begäran.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-143">You can create a new index within an Azure Search service using an HTTP POST or PUT request.</span></span> <span data-ttu-id="1a5b1-144">Brödtexten i begäran är en JSON-schema som anger informationen index och konfiguration.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-144">The body of the request is a JSON schema that specifies the index and configuration information.</span></span>

    POST https://[service name].search.windows.net/indexes?api-version=[api-version]
    Content-Type: application/json
    api-key: [admin key]

<span data-ttu-id="1a5b1-145">Du kan också använda PUT och ange Indexnamnet på URI: N.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-145">Alternatively, you can use PUT and specify the index name on the URI.</span></span> <span data-ttu-id="1a5b1-146">Om indexet inte finns, kommer att skapas.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-146">If the index does not exist, it will be created.</span></span>

    PUT https://[search service url]/indexes/[index name]?api-version=[api-version]

<span data-ttu-id="1a5b1-147">Skapa ett index anger strukturen för de dokument som lagras och används i sökningar.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-147">Creating an index determines the structure of the documents stored and used in search operations.</span></span> <span data-ttu-id="1a5b1-148">Fylla indexet är en separat åtgärd.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-148">Populating the index is a separate operation.</span></span> <span data-ttu-id="1a5b1-149">Det här steget kan du använda en [indexeraren](https://msdn.microsoft.com/library/azure/mt183328.aspx) (tillgängligt för datakällor som stöds) eller en [Lägg till, uppdatera eller ta bort dokument](https://msdn.microsoft.com/library/azure/dn798930.aspx) igen.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-149">For this step, you can use an [indexer](https://msdn.microsoft.com/library/azure/mt183328.aspx) (available for supported data sources) or an [Add, Update, or Delete Documents](https://msdn.microsoft.com/library/azure/dn798930.aspx) operation.</span></span> <span data-ttu-id="1a5b1-150">Omvända indexet genereras när dokumenten är bokförd.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-150">The inverted index is generated when the documents are posted.</span></span>

<span data-ttu-id="1a5b1-151">**Obs**: det maximala antalet tillåtna index varierar beroende på prisnivå.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-151">**Note**: The maximum number of indexes allowed varies by pricing tier.</span></span> <span data-ttu-id="1a5b1-152">Tjänsten gratis kan upp till 3 index.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-152">The free service allows up to 3 indexes.</span></span> <span data-ttu-id="1a5b1-153">Standard-tjänsten tillåter 50 index per söktjänst.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-153">Standard service allows 50 indexes per Search service.</span></span> <span data-ttu-id="1a5b1-154">Se [gränser och begränsningar](http://msdn.microsoft.com/library/azure/dn798934.aspx) mer information.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-154">See [Limits and constraints](http://msdn.microsoft.com/library/azure/dn798934.aspx) for details.</span></span>

<span data-ttu-id="1a5b1-155">**Förfrågan**</span><span class="sxs-lookup"><span data-stu-id="1a5b1-155">**Request**</span></span>

<span data-ttu-id="1a5b1-156">HTTPS krävs för alla tjänstbegäranden.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-156">HTTPS is required for all service requests.</span></span> <span data-ttu-id="1a5b1-157">Den **Create Index** begäran kan konstrueras med en POST eller PUT metod.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-157">The **Create Index** request can be constructed using either a POST or PUT method.</span></span> <span data-ttu-id="1a5b1-158">Du måste ange ett indexnamn i begärandetexten tillsammans med schemadefinition index när du använder POST.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-158">When using POST, you must provide an index name in the request body along with the index schema definition.</span></span> <span data-ttu-id="1a5b1-159">Med PUT är Indexnamnet en del av URL: en.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-159">With PUT, the index name is part of the URL.</span></span> <span data-ttu-id="1a5b1-160">Om det inte finns index skapas.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-160">If the index doesn't exist, it is created.</span></span> <span data-ttu-id="1a5b1-161">Om det redan finns uppdateras den till den nya definitionen.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-161">If it already exists, it is updated to the new definition.</span></span>

<span data-ttu-id="1a5b1-162">Indexnamnet måste vara gemener, börja med en bokstav eller siffra, har inga snedstreck eller punkter och vara mindre än 128 tecken.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-162">The index name must be lower case, start with a letter or number, have no slashes or dots, and be less than 128 characters.</span></span> <span data-ttu-id="1a5b1-163">När du har startat Indexnamnet med en bokstav eller siffra kan resten av namnet innehålla en bokstav, antal och tankstreck, så länge streck inte är i följd.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-163">After starting the index name with a letter or number, the rest of the name can include any letter, number and dashes, as long as the dashes are not consecutive.</span></span>

<span data-ttu-id="1a5b1-164">Den `api-version` krävs.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-164">The `api-version` is required.</span></span> <span data-ttu-id="1a5b1-165">Se [Sök Versionhantering](http://msdn.microsoft.com/library/azure/dn864560.aspx) för en lista över tillgängliga versioner.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-165">See [Search Service Versioning](http://msdn.microsoft.com/library/azure/dn864560.aspx) for a list of available versions.</span></span>

<span data-ttu-id="1a5b1-166">**Huvuden för begäran**</span><span class="sxs-lookup"><span data-stu-id="1a5b1-166">**Request Headers**</span></span>

<span data-ttu-id="1a5b1-167">I följande lista beskrivs de obligatoriska och valfria begärandehuvudena.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-167">The following list describes the required and optional request headers.</span></span>

* <span data-ttu-id="1a5b1-168">`Content-Type`: Krävs.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-168">`Content-Type`: Required.</span></span> <span data-ttu-id="1a5b1-169">Ställ in på`application/json`</span><span class="sxs-lookup"><span data-stu-id="1a5b1-169">Set this to `application/json`</span></span>
* <span data-ttu-id="1a5b1-170">`api-key`: Krävs.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-170">`api-key`: Required.</span></span> <span data-ttu-id="1a5b1-171">Den `api-key` används för att</span><span class="sxs-lookup"><span data-stu-id="1a5b1-171">The `api-key` is used to</span></span>
* <span data-ttu-id="1a5b1-172">autentisera begäran om att din söktjänst.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-172">authenticate the request to your Search service.</span></span> <span data-ttu-id="1a5b1-173">Det är ett strängvärde som unik för din tjänst.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-173">It is a string value, unique to your service.</span></span> <span data-ttu-id="1a5b1-174">Den **Create Index** begäran måste innehålla en `api-key` huvudet inställt till din administrationsnyckel (i stället för en fråga nyckel).</span><span class="sxs-lookup"><span data-stu-id="1a5b1-174">The **Create Index** request must include an `api-key` header set to your admin key (as opposed to a query key).</span></span>

<span data-ttu-id="1a5b1-175">Du måste också tjänstnamnet att konstruera den begärda Webbadressen.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-175">You will also need the service name to construct the request URL.</span></span> <span data-ttu-id="1a5b1-176">Du kan hämta både tjänstens namn och `api-key` från tjänsten instrumentpanelen i Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-176">You can get both the service name and `api-key` from your service dashboard in the Azure Portal.</span></span> <span data-ttu-id="1a5b1-177">Se [skapa en Azure Search-tjänst i portalen](search-create-service-portal.md) sidan navigering hjälp.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-177">See [Create an Azure Search service in the portal](search-create-service-portal.md) for page navigation help.</span></span>

<span data-ttu-id="1a5b1-178"><a name="RequestData"></a>
**Begäran brödtext Syntax**</span><span class="sxs-lookup"><span data-stu-id="1a5b1-178"><a name="RequestData"></a>
**Request Body Syntax**</span></span>

<span data-ttu-id="1a5b1-179">Brödtexten i begäran innehåller en schemadefinition av som innehåller en lista över datafält i dokument som kan matas in i detta index, datatyper, attribut, samt en valfri lista med bedömningen profiler som används för att poängsätta matchande dokument när databasfrågan.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-179">The body of the request contains a schema definition, which includes the list of data fields within documents that will be fed into this index, data types, attributes, as well as an optional list of scoring profiles that are used to score matching documents at query time.</span></span>

<span data-ttu-id="1a5b1-180">Observera att för en POST-begäran måste du ange Indexets namn i begärandetexten.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-180">Note that for a POST request, you must specify the index name in the request body.</span></span>

<span data-ttu-id="1a5b1-181">Det kan bara finnas ett nyckelfält i indexet.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-181">There can only be one key field in the index.</span></span> <span data-ttu-id="1a5b1-182">Det måste vara ett strängfält.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-182">It has to be a string field.</span></span> <span data-ttu-id="1a5b1-183">Det här fältet motsvarar den unika identifieraren för varje dokument som lagras i indexet.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-183">This field represents the unique identifier for each document stored within the index.</span></span>

<span data-ttu-id="1a5b1-184">De huvudsakliga delarna i ett index är följande:</span><span class="sxs-lookup"><span data-stu-id="1a5b1-184">The main parts of an index include the following:</span></span>

* `name`
* <span data-ttu-id="1a5b1-185">`fields`som kan matas in i detta index, inklusive namn, datatyp och egenskaper som definierar tillåtna åtgärder på fältet.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-185">`fields` that will be fed into this index, including name, data type, and properties that define allowable actions on that field.</span></span>
* <span data-ttu-id="1a5b1-186">`suggesters`används för automatisk komplettering eller typ-ahead-frågor.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-186">`suggesters` used for auto-complete or type-ahead queries.</span></span>
* <span data-ttu-id="1a5b1-187">`scoringProfiles`används för anpassad sökning poäng rangordning.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-187">`scoringProfiles` used for custom search score ranking.</span></span> <span data-ttu-id="1a5b1-188">Se [Lägg till bedömningsprofil profiler](https://msdn.microsoft.com/library/azure/dn798928.aspx) mer information.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-188">See [Add scoring profiles](https://msdn.microsoft.com/library/azure/dn798928.aspx) for details.</span></span>
* <span data-ttu-id="1a5b1-189">`analyzers`, `charFilters`, `tokenizers`, `tokenFilters` används för att definiera hur dina dokument/frågor är uppdelade indexeras/sökbara token.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-189">`analyzers`, `charFilters`, `tokenizers`, `tokenFilters` used to define how your documents/queries are broken into indexable/searchable tokens.</span></span> <span data-ttu-id="1a5b1-190">Se [analys i Azure Search](https://aka.ms//azsanalysis) mer information.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-190">See [Analysis in Azure Search](https://aka.ms//azsanalysis) for details.</span></span>
* <span data-ttu-id="1a5b1-191">`defaultScoringProfile`används för att skriva över dina standard bedömningen beteenden.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-191">`defaultScoringProfile` used to overwrite the default scoring behaviors.</span></span>
* <span data-ttu-id="1a5b1-192">`corsOptions`att tillåta cross-origin frågor mot ditt index.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-192">`corsOptions` to allow cross-origin queries against your index.</span></span>

<span data-ttu-id="1a5b1-193">Syntaxen för att strukturera nyttolasten i begäran är som följer.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-193">The syntax for structuring the request payload is as follows.</span></span> <span data-ttu-id="1a5b1-194">Ett exempel på en begäran har angetts mer på i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-194">A sample request is provided further on in this topic.</span></span>

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
          "analyzer": "name of the analyzer used for search and indexing", (only if 'searchAnalyzer' and 'indexAnalyzer' are not set)
          "searchAnalyzer": "name of the search analyzer", (only if 'indexAnalyzer' is set and 'analyzer' is not set)
          "indexAnalyzer": "name of the indexing analyzer" (only if 'searchAnalyzer' is set and 'analyzer' is not set)
        }
      ],
      "suggesters": [
        {
          "name": "name of suggester",
          "searchMode": "analyzingInfixMatching" (other modes may be added in the future),
          "sourceFields": ["field1", "field2", ...]
        }
      ],
      "scoringProfiles": [
        {
          "name": "name of scoring profile",
          "text": (optional, only applies to searchable fields) {
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
                "boostingDuration": "..." (value representing timespan leading to now over which boosting occurs)
              },
              "distance": {
                "referencePointParameter": "...", (parameter to be passed in queries to use as reference location, see "scoringParameter" for syntax details)
                "boostingDistance": # (the distance in kilometers from the reference location where the boosting range ends)
              },
              "tag": {
                "tagsParameter": "..." (parameter to be passed in queries to specify list of tags to compare against target field, see "scoringParameter" for syntax details)
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

<span data-ttu-id="1a5b1-195">**Indexattribut**</span><span class="sxs-lookup"><span data-stu-id="1a5b1-195">**Index Attributes**</span></span>

<span data-ttu-id="1a5b1-196">Följande attribut kan anges när ett index skapas.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-196">The following attributes can be set when creating an index.</span></span> <span data-ttu-id="1a5b1-197">Mer information om batchbedömning och bedömningen profiler finns [lägga till bedömningen profiler](https://msdn.microsoft.com/library/azure/dn798928.aspx).</span><span class="sxs-lookup"><span data-stu-id="1a5b1-197">For details about scoring and scoring profiles, see [Add scoring Profiles](https://msdn.microsoft.com/library/azure/dn798928.aspx).</span></span>

<span data-ttu-id="1a5b1-198">`name`-Anger namnet på fältet.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-198">`name` - Sets the name of the field.</span></span>

<span data-ttu-id="1a5b1-199">`type`-Anger datatypen för fältet.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-199">`type` - Sets the data type for the field.</span></span>

<span data-ttu-id="1a5b1-200">`searchable`-Markerar fältet som fulltext-sökning kan.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-200">`searchable` - Marks the field as full-text search-able.</span></span> <span data-ttu-id="1a5b1-201">Det innebär att den kommer att göras analys, till exempel radbrytning under indexeringen.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-201">This means it will undergo analysis such as word-breaking during indexing.</span></span> <span data-ttu-id="1a5b1-202">Om du ställer in en `searchable` fält till ett värde som ”skiner”, internt den delas upp i enskilda token ”Soligt” och ”dag”.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-202">If you set a `searchable` field to a value like "sunny day", internally it will be split into the individual tokens "sunny" and "day".</span></span> <span data-ttu-id="1a5b1-203">Detta gör att fulltextsökningar för dessa villkor.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-203">This enables full-text searches for these terms.</span></span> <span data-ttu-id="1a5b1-204">Fält av typen `Edm.String` eller `Collection(Edm.String)` är `searchable` som standard.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-204">Fields of type `Edm.String` or `Collection(Edm.String)` are `searchable` by default.</span></span> <span data-ttu-id="1a5b1-205">Fält av andra typer kan inte vara `searchable`.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-205">Fields of other types cannot be `searchable`.</span></span>

* <span data-ttu-id="1a5b1-206">**Obs**: `searchable` fält använda extra utrymme i ditt index eftersom en ytterligare principfilerna version av fältvärdet fulltextsökningar lagras i Azure Search.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-206">**Note**: `searchable` fields consume extra space in your index since Azure Search will store an additional tokenized version of the field value for full-text searches.</span></span> <span data-ttu-id="1a5b1-207">Om du vill spara utrymme i ditt index och behöver du ett fält som ska ingå i sökningar, anger `searchable` till `false`.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-207">If you want to save space in your index and you don't need a field to be included in searches, set `searchable` to `false`.</span></span>

<span data-ttu-id="1a5b1-208">`filterable`-Kan refereras i fältet `$filter` frågor.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-208">`filterable` - Allows the field to be referenced in `$filter` queries.</span></span> <span data-ttu-id="1a5b1-209">`filterable`skiljer sig från `searchable` i hur strängar ska hanteras.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-209">`filterable` differs from `searchable` in how strings are handled.</span></span> <span data-ttu-id="1a5b1-210">Fält av typen `Edm.String` eller `Collection(Edm.String)` som är `filterable` inte genomgår radbrytning, så att jämförelser för endast exakt matchar.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-210">Fields of type `Edm.String` or `Collection(Edm.String)` that are `filterable` do not undergo word-breaking, so comparisons are for exact matches only.</span></span> <span data-ttu-id="1a5b1-211">Till exempel om du ställer in sådant fält `f` till ”skiner” `$filter=f eq 'sunny'` hittar inga matchningar men `$filter=f eq 'sunny day'` kommer.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-211">For example, if you set such a field `f` to "sunny day", `$filter=f eq 'sunny'` will find no matches, but `$filter=f eq 'sunny day'` will.</span></span> <span data-ttu-id="1a5b1-212">Alla fält är `filterable` som standard.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-212">All fields are `filterable` by default.</span></span>

<span data-ttu-id="1a5b1-213">`sortable`-Som standard sorterar systemet resultat poäng, men i många upplevelser användare vill sortera efter fält i dokument.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-213">`sortable` - By default the system sorts results by score, but in many experiences users will want to sort by fields in the documents.</span></span> <span data-ttu-id="1a5b1-214">Fält av typen `Collection(Edm.String)` får inte vara `sortable`.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-214">Fields of type `Collection(Edm.String)` cannot be `sortable`.</span></span> <span data-ttu-id="1a5b1-215">Alla andra fält är `sortable` som standard.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-215">All other fields are `sortable` by default.</span></span>

<span data-ttu-id="1a5b1-216">`facetable`-Vanligtvis används i en presentation av sökresultat som innehåller antalet träffar per kategori (till exempel, Sök efter digitalkameror och se träffar efter varumärke, megapixel, av pris, etc.).</span><span class="sxs-lookup"><span data-stu-id="1a5b1-216">`facetable`- Typically used in a presentation of search results that includes hit count by category (for example, search for digital cameras and see hits by brand, by megapixels, by price, etc.).</span></span> <span data-ttu-id="1a5b1-217">Det här alternativet kan inte användas med fält av typen `Edm.GeographyPoint`.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-217">This option cannot be used with fields of type `Edm.GeographyPoint`.</span></span> <span data-ttu-id="1a5b1-218">Alla andra fält är `facetable` som standard.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-218">All other fields are `facetable` by default.</span></span>

* <span data-ttu-id="1a5b1-219">**Obs**: fält av typen `Edm.String` som är `filterable`, `sortable`, eller `facetable` kan vara högst 32 KB längd.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-219">**Note**: Fields of type `Edm.String` that are `filterable`, `sortable`, or `facetable` can be at most 32KB in length.</span></span> <span data-ttu-id="1a5b1-220">Detta beror på att dessa fält behandlas som en enda sökterm och den maximala längden på en term i Azure Search är 32KB.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-220">This is because such fields are treated as a single search term, and the maximum length of a term in Azure Search is 32KB.</span></span> <span data-ttu-id="1a5b1-221">Om du behöver lagra mer text än detta i ett enda strängfält behöver du uttryckligen ställa in `filterable`, `sortable`, och `facetable` till `false` i Indexdefinition.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-221">If you need to store more text than this in a single string field, you will need to explicitly set `filterable`, `sortable`, and `facetable` to `false` in your index definition.</span></span>
* <span data-ttu-id="1a5b1-222">**Obs**: om ett fält har ingen av de ovanstående attribut `true` (`searchable`, `filterable`, `sortable`, eller`facetable`) fältet effektivt undantagits från inverterad indexet.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-222">**Note**: If a field has none of the above attributes set to `true` (`searchable`, `filterable`, `sortable`,  or`facetable`) the field is effectively excluded from the inverted index.</span></span> <span data-ttu-id="1a5b1-223">Det här alternativet är användbart för fält som inte används i frågor, men som behövs i sökresultaten.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-223">This option is useful for fields that are not used in queries, but are needed in search results.</span></span> <span data-ttu-id="1a5b1-224">Förutom dessa fält från indexet förbättrar prestanda.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-224">Excluding such fields from the index improves performance.</span></span>

<span data-ttu-id="1a5b1-225">`key`-Markerar fältet innehåller unika identifierare för dokument i indexet.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-225">`key` - Marks the field as containing unique identifiers for documents within the index.</span></span> <span data-ttu-id="1a5b1-226">Exakt ett fält måste väljas som den `key` fältet och det måste vara av typen `Edm.String`.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-226">Exactly one field must be chosen as the `key` field and it must be of type `Edm.String`.</span></span> <span data-ttu-id="1a5b1-227">Nyckelfält kan användas för att leta upp dokument direkt via den [sökning API](#LookupAPI).</span><span class="sxs-lookup"><span data-stu-id="1a5b1-227">Key fields can be used to look up documents directly via the [Lookup API](#LookupAPI).</span></span>

<span data-ttu-id="1a5b1-228">`retrievable`-Anger om fältet kan returneras i sökresultatet.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-228">`retrievable` - Sets whether the field can be returned in a search result.</span></span>  <span data-ttu-id="1a5b1-229">Detta är användbart när du vill använda ett fält (till exempel marginal) som ett filter, sortera eller bedömningen mekanism men inte vill att fält som ska visas för slutanvändaren.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-229">This is useful when you want to use a field (for example, margin) as a filter, sorting, or scoring mechanism but do not want the field to be visible to the end user.</span></span> <span data-ttu-id="1a5b1-230">Det här attributet måste vara `true` för `key` fält.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-230">This attribute must be `true` for `key` fields.</span></span>

<span data-ttu-id="1a5b1-231">`analyzer`-Anger namnet på analyzer om du vill använda för fält vid sökning och indexering tid.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-231">`analyzer` - Sets the name of the analyzer to use for the field at search time and indexing time.</span></span> <span data-ttu-id="1a5b1-232">Tillåtna uppsättning värden finns [Analyzers](https://msdn.microsoft.com/library/mt605304.aspx).</span><span class="sxs-lookup"><span data-stu-id="1a5b1-232">For the allowed set of values see [Analyzers](https://msdn.microsoft.com/library/mt605304.aspx).</span></span> <span data-ttu-id="1a5b1-233">Det här alternativet kan användas med `searchable` fält och den kan inte anges tillsammans med antingen `searchAnalyzer` eller `indexAnalyzer`.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-233">This option can be used only with `searchable` fields and it can't be set together with either `searchAnalyzer` or `indexAnalyzer`.</span></span>  <span data-ttu-id="1a5b1-234">När analyzer är valt, kan inte ändras för fältet.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-234">Once the analyzer is chosen, it cannot be changed for the field.</span></span>

<span data-ttu-id="1a5b1-235">`searchAnalyzer`-Anger namnet på analyzer som används för närvarande sökning för fältet.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-235">`searchAnalyzer` - Sets the name of the analyzer used at search time for the field.</span></span> <span data-ttu-id="1a5b1-236">Tillåtna uppsättning värden finns [Analyzers](https://msdn.microsoft.com/library/mt605304.aspx).</span><span class="sxs-lookup"><span data-stu-id="1a5b1-236">For the allowed set of values see [Analyzers](https://msdn.microsoft.com/library/mt605304.aspx).</span></span> <span data-ttu-id="1a5b1-237">Det här alternativet kan användas med `searchable` fält.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-237">This option can be used only with `searchable` fields.</span></span> <span data-ttu-id="1a5b1-238">Det måste anges tillsammans med `indexAnalyzer` och kan inte anges tillsammans med den `analyzer` alternativet.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-238">It must be set together with `indexAnalyzer` and it cannot be set together with the `analyzer` option.</span></span> <span data-ttu-id="1a5b1-239">Den här analyzer kan uppdateras på ett befintligt fält.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-239">This analyzer can be updated on an existing field.</span></span>

<span data-ttu-id="1a5b1-240">`indexAnalyzer`-Anger namnet på analyzer som används för närvarande indexering för fältet.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-240">`indexAnalyzer` - Sets the name of the analyzer used at indexing time for the field.</span></span> <span data-ttu-id="1a5b1-241">Tillåtna uppsättning värden finns [Analyzers](https://msdn.microsoft.com/library/mt605304.aspx).</span><span class="sxs-lookup"><span data-stu-id="1a5b1-241">For the allowed set of values see [Analyzers](https://msdn.microsoft.com/library/mt605304.aspx).</span></span> <span data-ttu-id="1a5b1-242">Det här alternativet kan användas med `searchable` fält.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-242">This option can be used only with `searchable` fields.</span></span> <span data-ttu-id="1a5b1-243">Det måste anges tillsammans med `searchAnalyzer` och kan inte anges tillsammans med den `analyzer` alternativet.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-243">It must be set together with `searchAnalyzer` and it cannot be set together with the `analyzer` option.</span></span> <span data-ttu-id="1a5b1-244">När analyzer är valt, kan inte ändras för fältet.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-244">Once the analyzer is chosen, it cannot be changed for the field.</span></span>

<span data-ttu-id="1a5b1-245">`suggesters`-Anger sökläget och fält som är källan till innehållet för förslag.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-245">`suggesters` - Sets the search mode and fields that are the source of the content for suggestions.</span></span> <span data-ttu-id="1a5b1-246">Se [Suggesters](#Suggesters) mer information.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-246">See [Suggesters](#Suggesters) for details.</span></span>

<span data-ttu-id="1a5b1-247">`scoringProfiles`-Definierar anpassade bedömningsprofil beteenden som gör att du kan påverka vilka objekt som visas överst i sökresultaten.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-247">`scoringProfiles` - Defines custom scoring behaviors that let you influence which items appear higher in search results.</span></span> <span data-ttu-id="1a5b1-248">Bedömningsprofil profiler består av fältvikter och funktioner.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-248">Scoring profiles are made up of field weights and functions.</span></span> <span data-ttu-id="1a5b1-249">Se [lägga till bedömningen profiler](https://msdn.microsoft.com/library/azure/dn798928.aspx) för mer information om de attribut som används i en bedömningsprofilen.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-249">See [Add scoring Profiles](https://msdn.microsoft.com/library/azure/dn798928.aspx) for more information about the attributes used in a scoring profile.</span></span>

<span data-ttu-id="1a5b1-250"><!-- This is a standalone topic in MSDN -->
<a name="LanguageSupport"></a>
**Språkstöd**</span><span class="sxs-lookup"><span data-stu-id="1a5b1-250"><!-- This is a standalone topic in MSDN -->
<a name="LanguageSupport"></a>
**Language support**</span></span>

<span data-ttu-id="1a5b1-251">Sökbara fält genomgå analys som omfattar ofta avstavning text normalisering och filtrera bort villkor.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-251">Searchable fields undergo analysis that most frequently involves word-breaking, text normalization, and filtering out terms.</span></span> <span data-ttu-id="1a5b1-252">Som standard sökbara fält i Azure Search analyseras med den [Apache Lucene Standard analyzer](http://lucene.apache.org/core/4_9_0/analyzers-common/index.html) som delar upp text i element som följer den[”Unicode-Text segmentering”](http://unicode.org/reports/tr29/) regler.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-252">By default, searchable fields in Azure Search are analyzed with the [Apache Lucene Standard analyzer](http://lucene.apache.org/core/4_9_0/analyzers-common/index.html) which breaks text into elements following the["Unicode Text Segmentation"](http://unicode.org/reports/tr29/) rules.</span></span> <span data-ttu-id="1a5b1-253">Dessutom konverterar standard analyzer alla tecken till gemener formuläret.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-253">Additionally, the standard analyzer converts all characters to their lower case form.</span></span> <span data-ttu-id="1a5b1-254">Både indexerade dokument och sökvillkoren kan du gå igenom analysen under indexering och frågebearbetning.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-254">Both indexed documents and search terms go through the analysis during indexing and query processing.</span></span>

<span data-ttu-id="1a5b1-255">Azure Search har stöd för flera olika språk.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-255">Azure Search supports a variety of languages.</span></span> <span data-ttu-id="1a5b1-256">Varje språk kräver en icke-standard text analyzer som-konton för egenskaperna för ett visst språk.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-256">Each language requires a non-standard text analyzer which accounts for characteristics of a given language.</span></span> <span data-ttu-id="1a5b1-257">Azure Search finns två typer av analyzers:</span><span class="sxs-lookup"><span data-stu-id="1a5b1-257">Azure Search offers two types of analyzers:</span></span>

* <span data-ttu-id="1a5b1-258">35 analyzers backas upp av Lucene.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-258">35 analyzers backed by Lucene.</span></span>
* <span data-ttu-id="1a5b1-259">50 analyzers backas upp av egna Microsoft naturligt språk bearbetning teknik som används i Office och Bing.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-259">50 analyzers backed by proprietary Microsoft natural language processing technology used in Office and Bing.</span></span>

<span data-ttu-id="1a5b1-260">Vissa utvecklare kanske föredrar Lucene mer bekanta, enkla, öppen källkod lösningen.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-260">Some developers might prefer the more familiar, simple, open-source solution of Lucene.</span></span> <span data-ttu-id="1a5b1-261">Lucene analyzers är snabbare, men Microsoft analyzers har avancerade funktioner, till exempel lemmatisering, word decompounding (i språk som tyska, danska, nederländska, svenska, norska, estniska, Slutför, ungerska, slovakiska) och entitet recognition (URL: er, e-postmeddelanden, datum, nummer).</span><span class="sxs-lookup"><span data-stu-id="1a5b1-261">Lucene analyzers are faster, but the Microsoft analyzers have advanced capabilities, such as lemmatization, word decompounding (in languages like German, Danish, Dutch, Swedish, Norwegian, Estonian, Finish, Hungarian, Slovak) and entity recognition (URLs, emails, dates, numbers).</span></span> <span data-ttu-id="1a5b1-262">Om möjligt bör du köra jämförelser av både Microsoft och Lucene analyzers att avgöra vilket som är en bättre anpassning.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-262">If possible, you should run comparisons of both the Microsoft and Lucene analyzers to decide which one is a better fit.</span></span>

<span data-ttu-id="1a5b1-263">***Jämförelse***</span><span class="sxs-lookup"><span data-stu-id="1a5b1-263">***How they compare***</span></span>

<span data-ttu-id="1a5b1-264">Lucene analyzer för engelska utökar standard analyzer.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-264">The Lucene analyzer for English extends the standard analyzer.</span></span> <span data-ttu-id="1a5b1-265">Det tar bort genitiv (avslutande) från ord, gäller härrör enligt [Porter härrör algoritmen](http://tartarus.org/~martin/PorterStemmer/), och tar bort engelska [stoppord](http://en.wikipedia.org/wiki/Stop_words).</span><span class="sxs-lookup"><span data-stu-id="1a5b1-265">It removes possessives (trailing 's) from words, applies stemming as per [Porter Stemming algorithm](http://tartarus.org/~martin/PorterStemmer/), and removes English [stop words](http://en.wikipedia.org/wiki/Stop_words).</span></span>

<span data-ttu-id="1a5b1-266">Jämförelse utför Microsoft analyzer lemmatisering i stället för härrör.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-266">In comparison, the Microsoft analyzer performs lemmatization instead of stemming.</span></span> <span data-ttu-id="1a5b1-267">Det innebär att den kan hantera böjda och oregelbundna word formulär förbättras vad resulterar i mer relevant sökresultat (titta på modul 7 av [Azure Search MVA presentation](http://www.microsoftvirtualacademy.com/training-courses/adding-microsoft-azure-search-to-your-websites-and-apps) för mer information).</span><span class="sxs-lookup"><span data-stu-id="1a5b1-267">It means it can handle inflected and irregular word forms much better what results in more relevant search results (watch module 7 of [Azure Search MVA presentation](http://www.microsoftvirtualacademy.com/training-courses/adding-microsoft-azure-search-to-your-websites-and-apps) for more details).</span></span>

<span data-ttu-id="1a5b1-268">Indexering med Microsoft analyzers är i genomsnitt två till tre gånger långsammare än motsvarigheterna Lucene beroende på språket.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-268">Indexing with Microsoft analyzers is on average two to three times slower than their Lucene equivalents, depending on the language.</span></span> <span data-ttu-id="1a5b1-269">Sökningsprestanda avsevärt påverkas inte för frågor genomsnittlig storlek.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-269">Search performance should not be significantly affected for average size queries.</span></span>

<span data-ttu-id="1a5b1-270">***Konfiguration***</span><span class="sxs-lookup"><span data-stu-id="1a5b1-270">***Configuration***</span></span>

<span data-ttu-id="1a5b1-271">Du kan ange för varje fält i indexdefinitionen den `analyzer` egenskapen till ett analyzer namn som anger vilka språk och leverantör.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-271">For each field in the index definition, you can set the `analyzer` property to an analyzer name that specifies which language and vendor.</span></span> <span data-ttu-id="1a5b1-272">Samma analyzer tillämpas när indexering och sökning för fältet.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-272">The same analyzer will be applied when indexing and searching for that field.</span></span>
<span data-ttu-id="1a5b1-273">Du kan till exempel ha separata fält för engelska, franska och spanska hotell beskrivningar som finns sida vid sida i samma index.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-273">For example, you can have separate fields for English, French, and Spanish hotel descriptions that exist side-by-side in the same index.</span></span> <span data-ttu-id="1a5b1-274">Använd den ['searchFields' Frågeparametern](#SearchQueryParameters) att ange vilket språkspecifika fält om du vill söka mot i dina frågor.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-274">Use the ['searchFields' query parameter](#SearchQueryParameters) to specify which language-specific field to search against in your queries.</span></span> <span data-ttu-id="1a5b1-275">Du kan granska frågan exempel som innehåller den `analyzer` egenskap i [Sök dokument](#SearchDocs).</span><span class="sxs-lookup"><span data-stu-id="1a5b1-275">You can review query examples that include the `analyzer` property in [Search Documents](#SearchDocs).</span></span> 

<span data-ttu-id="1a5b1-276">***Analyzer lista***</span><span class="sxs-lookup"><span data-stu-id="1a5b1-276">***Analyzer list***</span></span>

<span data-ttu-id="1a5b1-277">Nedan visas en lista över språk som stöds tillsammans med Lucene och Microsoft analyzer namn.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-277">Below is the list of supported languages together with Lucene and Microsoft analyzer names.</span></span>

<table style="font-size:12">
    <tr>
        <th><span data-ttu-id="1a5b1-278">Språk</span><span class="sxs-lookup"><span data-stu-id="1a5b1-278">Language</span></span></th>
        <th><span data-ttu-id="1a5b1-279">Microsoft analyzer namn</span><span class="sxs-lookup"><span data-stu-id="1a5b1-279">Microsoft analyzer name</span></span></th>
        <th><span data-ttu-id="1a5b1-280">Lucene analyzer namn</span><span class="sxs-lookup"><span data-stu-id="1a5b1-280">Lucene analyzer name</span></span></th>
    </tr>
    <tr>
        <td><span data-ttu-id="1a5b1-281">Arabiska</span><span class="sxs-lookup"><span data-stu-id="1a5b1-281">Arabic</span></span></td>
        <td><span data-ttu-id="1a5b1-282">ar.Microsoft</span><span class="sxs-lookup"><span data-stu-id="1a5b1-282">ar.microsoft</span></span></td>
        <td><span data-ttu-id="1a5b1-283">ar.lucene</span><span class="sxs-lookup"><span data-stu-id="1a5b1-283">ar.lucene</span></span></td>        
    </tr>
    <tr>
        <td><span data-ttu-id="1a5b1-284">Armeniska</span><span class="sxs-lookup"><span data-stu-id="1a5b1-284">Armenian</span></span></td>
        <td></td>
        <td><span data-ttu-id="1a5b1-285">hy.lucene</span><span class="sxs-lookup"><span data-stu-id="1a5b1-285">hy.lucene</span></span></td>
      </tr>
    <tr>
        <td><span data-ttu-id="1a5b1-286">Bangla</span><span class="sxs-lookup"><span data-stu-id="1a5b1-286">Bangla</span></span></td>
        <td><span data-ttu-id="1a5b1-287">bn.Microsoft</span><span class="sxs-lookup"><span data-stu-id="1a5b1-287">bn.microsoft</span></span></td>
        <td></td>
    </tr>
      <tr>
        <td><span data-ttu-id="1a5b1-288">Baskiska</span><span class="sxs-lookup"><span data-stu-id="1a5b1-288">Basque</span></span></td>
        <td></td>
        <td><span data-ttu-id="1a5b1-289">EU.lucene</span><span class="sxs-lookup"><span data-stu-id="1a5b1-289">eu.lucene</span></span></td>
    </tr>
      <tr>
         <td><span data-ttu-id="1a5b1-290">Bulgariska</span><span class="sxs-lookup"><span data-stu-id="1a5b1-290">Bulgarian</span></span></td>
        <td><span data-ttu-id="1a5b1-291">BG.Microsoft</span><span class="sxs-lookup"><span data-stu-id="1a5b1-291">bg.microsoft</span></span></td>
        <td><span data-ttu-id="1a5b1-292">BG.lucene</span><span class="sxs-lookup"><span data-stu-id="1a5b1-292">bg.lucene</span></span></td>
      </tr>
      <tr>
        <td><span data-ttu-id="1a5b1-293">Katalanska</span><span class="sxs-lookup"><span data-stu-id="1a5b1-293">Catalan</span></span></td>
        <td><span data-ttu-id="1a5b1-294">CA.Microsoft</span><span class="sxs-lookup"><span data-stu-id="1a5b1-294">ca.microsoft</span></span></td>
        <td><span data-ttu-id="1a5b1-295">CA.lucene</span><span class="sxs-lookup"><span data-stu-id="1a5b1-295">ca.lucene</span></span></td>          
      </tr>
    <tr>
        <td><span data-ttu-id="1a5b1-296">Kinesiska, förenklad</span><span class="sxs-lookup"><span data-stu-id="1a5b1-296">Chinese Simplified</span></span></td>
        <td><span data-ttu-id="1a5b1-297">zh-Hans.microsoft</span><span class="sxs-lookup"><span data-stu-id="1a5b1-297">zh-Hans.microsoft</span></span></td>
        <td><span data-ttu-id="1a5b1-298">zh-Hans.lucene</span><span class="sxs-lookup"><span data-stu-id="1a5b1-298">zh-Hans.lucene</span></span></td>        
    </tr>
    <tr>
        <td><span data-ttu-id="1a5b1-299">Kinesiska, traditionell</span><span class="sxs-lookup"><span data-stu-id="1a5b1-299">Chinese Traditional</span></span></td>
        <td><span data-ttu-id="1a5b1-300">zh-Hant.microsoft</span><span class="sxs-lookup"><span data-stu-id="1a5b1-300">zh-Hant.microsoft</span></span></td>
        <td><span data-ttu-id="1a5b1-301">zh-Hant.lucene</span><span class="sxs-lookup"><span data-stu-id="1a5b1-301">zh-Hant.lucene</span></span></td>        
    <tr>
    <tr>
        <td><span data-ttu-id="1a5b1-302">Kroatiska</span><span class="sxs-lookup"><span data-stu-id="1a5b1-302">Croatian</span></span></td>
        <td><span data-ttu-id="1a5b1-303">HR.Microsoft</span><span class="sxs-lookup"><span data-stu-id="1a5b1-303">hr.microsoft</span></span></td>
        <td/></td>
    </tr>
    <tr>
        <td><span data-ttu-id="1a5b1-304">Tjeckiska</span><span class="sxs-lookup"><span data-stu-id="1a5b1-304">Czech</span></span></td>
        <td><span data-ttu-id="1a5b1-305">CS.Microsoft</span><span class="sxs-lookup"><span data-stu-id="1a5b1-305">cs.microsoft</span></span></td>
        <td><span data-ttu-id="1a5b1-306">CS.lucene</span><span class="sxs-lookup"><span data-stu-id="1a5b1-306">cs.lucene</span></span></td>        
    </tr>    
    <tr>
        <td><span data-ttu-id="1a5b1-307">Danska</span><span class="sxs-lookup"><span data-stu-id="1a5b1-307">Danish</span></span></td>
        <td><span data-ttu-id="1a5b1-308">da.Microsoft</span><span class="sxs-lookup"><span data-stu-id="1a5b1-308">da.microsoft</span></span></td>
        <td><span data-ttu-id="1a5b1-309">da.lucene</span><span class="sxs-lookup"><span data-stu-id="1a5b1-309">da.lucene</span></span></td>        
    </tr>    
    <tr>
        <td><span data-ttu-id="1a5b1-310">Nederländska</span><span class="sxs-lookup"><span data-stu-id="1a5b1-310">Dutch</span></span></td>
        <td><span data-ttu-id="1a5b1-311">NL.Microsoft</span><span class="sxs-lookup"><span data-stu-id="1a5b1-311">nl.microsoft</span></span></td>
        <td><span data-ttu-id="1a5b1-312">NL.lucene</span><span class="sxs-lookup"><span data-stu-id="1a5b1-312">nl.lucene</span></span></td>    
    </tr>    
    <tr>
        <td><span data-ttu-id="1a5b1-313">Svenska</span><span class="sxs-lookup"><span data-stu-id="1a5b1-313">English</span></span></td>        
        <td><span data-ttu-id="1a5b1-314">en.Microsoft</span><span class="sxs-lookup"><span data-stu-id="1a5b1-314">en.microsoft</span></span></td>
        <td><span data-ttu-id="1a5b1-315">en.lucene</span><span class="sxs-lookup"><span data-stu-id="1a5b1-315">en.lucene</span></span></td>        
    </tr>
    <tr>
        <td><span data-ttu-id="1a5b1-316">Estniska</span><span class="sxs-lookup"><span data-stu-id="1a5b1-316">Estonian</span></span></td>
        <td><span data-ttu-id="1a5b1-317">et.Microsoft</span><span class="sxs-lookup"><span data-stu-id="1a5b1-317">et.microsoft</span></span></td>
        <td></td>
    </tr>
    <tr>
        <td><span data-ttu-id="1a5b1-318">Finska</span><span class="sxs-lookup"><span data-stu-id="1a5b1-318">Finnish</span></span></td>
        <td><span data-ttu-id="1a5b1-319">Fi.Microsoft</span><span class="sxs-lookup"><span data-stu-id="1a5b1-319">fi.microsoft</span></span></td>
        <td><span data-ttu-id="1a5b1-320">Fi.lucene</span><span class="sxs-lookup"><span data-stu-id="1a5b1-320">fi.lucene</span></span></td>        
    </tr>    
    <tr>
        <td><span data-ttu-id="1a5b1-321">Franska</span><span class="sxs-lookup"><span data-stu-id="1a5b1-321">French</span></span></td>
        <td><span data-ttu-id="1a5b1-322">fr.Microsoft</span><span class="sxs-lookup"><span data-stu-id="1a5b1-322">fr.microsoft</span></span></td>
        <td><span data-ttu-id="1a5b1-323">fr.lucene</span><span class="sxs-lookup"><span data-stu-id="1a5b1-323">fr.lucene</span></span></td>        
    </tr>
    <tr>
        <td><span data-ttu-id="1a5b1-324">Galiciska</span><span class="sxs-lookup"><span data-stu-id="1a5b1-324">Galician</span></span></td>
        <td></td>
        <td><span data-ttu-id="1a5b1-325">GL.lucene</span><span class="sxs-lookup"><span data-stu-id="1a5b1-325">gl.lucene</span></span></td>        
      </tr>
    <tr>
        <td><span data-ttu-id="1a5b1-326">Tyska</span><span class="sxs-lookup"><span data-stu-id="1a5b1-326">German</span></span></td>
        <td><span data-ttu-id="1a5b1-327">de.Microsoft</span><span class="sxs-lookup"><span data-stu-id="1a5b1-327">de.microsoft</span></span></td>
        <td><span data-ttu-id="1a5b1-328">de.lucene</span><span class="sxs-lookup"><span data-stu-id="1a5b1-328">de.lucene</span></span></td>        
    </tr>
    <tr>
        <td><span data-ttu-id="1a5b1-329">Grekiska</span><span class="sxs-lookup"><span data-stu-id="1a5b1-329">Greek</span></span></td>
        <td><span data-ttu-id="1a5b1-330">el.Microsoft</span><span class="sxs-lookup"><span data-stu-id="1a5b1-330">el.microsoft</span></span></td>
        <td><span data-ttu-id="1a5b1-331">el.lucene</span><span class="sxs-lookup"><span data-stu-id="1a5b1-331">el.lucene</span></span></td>        
    </tr>
    <tr>
        <td><span data-ttu-id="1a5b1-332">Gujarati</span><span class="sxs-lookup"><span data-stu-id="1a5b1-332">Gujarati</span></span></td>
        <td><span data-ttu-id="1a5b1-333">Gu.Microsoft</span><span class="sxs-lookup"><span data-stu-id="1a5b1-333">gu.microsoft</span></span></td>
        <td></td>
    </tr>
    <tr>
        <td><span data-ttu-id="1a5b1-334">Hebreiska</span><span class="sxs-lookup"><span data-stu-id="1a5b1-334">Hebrew</span></span></td>
        <td><span data-ttu-id="1a5b1-335">HE.Microsoft</span><span class="sxs-lookup"><span data-stu-id="1a5b1-335">he.microsoft</span></span></td>
        <td></td>
    </tr>
    <tr>
        <td><span data-ttu-id="1a5b1-336">Hindi</span><span class="sxs-lookup"><span data-stu-id="1a5b1-336">Hindi</span></span></td>
        <td><span data-ttu-id="1a5b1-337">Hi.Microsoft</span><span class="sxs-lookup"><span data-stu-id="1a5b1-337">hi.microsoft</span></span></td>
        <td><span data-ttu-id="1a5b1-338">Hi.lucene</span><span class="sxs-lookup"><span data-stu-id="1a5b1-338">hi.lucene</span></span></td>        
    </tr>
    <tr>
        <td><span data-ttu-id="1a5b1-339">Ungerska</span><span class="sxs-lookup"><span data-stu-id="1a5b1-339">Hungarian</span></span></td>        
        <td><span data-ttu-id="1a5b1-340">HU.Microsoft</span><span class="sxs-lookup"><span data-stu-id="1a5b1-340">hu.microsoft</span></span></td>
        <td><span data-ttu-id="1a5b1-341">HU.lucene</span><span class="sxs-lookup"><span data-stu-id="1a5b1-341">hu.lucene</span></span></td>
    </tr>
    <tr>
        <td><span data-ttu-id="1a5b1-342">Isländska</span><span class="sxs-lookup"><span data-stu-id="1a5b1-342">Icelandic</span></span></td>
        <td><span data-ttu-id="1a5b1-343">is.Microsoft</span><span class="sxs-lookup"><span data-stu-id="1a5b1-343">is.microsoft</span></span></td>
        <td></td>
    </tr>
    <tr>
        <td><span data-ttu-id="1a5b1-344">Indonesiska (Bahasa)</span><span class="sxs-lookup"><span data-stu-id="1a5b1-344">Indonesian (Bahasa)</span></span></td>
        <td><span data-ttu-id="1a5b1-345">ID.Microsoft</span><span class="sxs-lookup"><span data-stu-id="1a5b1-345">id.microsoft</span></span></td>
        <td><span data-ttu-id="1a5b1-346">ID.lucene</span><span class="sxs-lookup"><span data-stu-id="1a5b1-346">id.lucene</span></span></td>        
    </tr>
    <tr>
        <td><span data-ttu-id="1a5b1-347">Irländska</span><span class="sxs-lookup"><span data-stu-id="1a5b1-347">Irish</span></span></td>
        <td></td>
          <td><span data-ttu-id="1a5b1-348">GA.lucene</span><span class="sxs-lookup"><span data-stu-id="1a5b1-348">ga.lucene</span></span></td>
    </tr>
    <tr>
        <td><span data-ttu-id="1a5b1-349">Italienska</span><span class="sxs-lookup"><span data-stu-id="1a5b1-349">Italian</span></span></td>
        <td><span data-ttu-id="1a5b1-350">IT.Microsoft</span><span class="sxs-lookup"><span data-stu-id="1a5b1-350">it.microsoft</span></span></td>
        <td><span data-ttu-id="1a5b1-351">IT.lucene</span><span class="sxs-lookup"><span data-stu-id="1a5b1-351">it.lucene</span></span></td>        
    </tr>
    <tr>
        <td><span data-ttu-id="1a5b1-352">Japanska</span><span class="sxs-lookup"><span data-stu-id="1a5b1-352">Japanese</span></span></td>
        <td><span data-ttu-id="1a5b1-353">Ja.Microsoft</span><span class="sxs-lookup"><span data-stu-id="1a5b1-353">ja.microsoft</span></span></td>
        <td><span data-ttu-id="1a5b1-354">Ja.lucene</span><span class="sxs-lookup"><span data-stu-id="1a5b1-354">ja.lucene</span></span></td>

    </tr>
    <tr>
        <td><span data-ttu-id="1a5b1-355">Kannada</span><span class="sxs-lookup"><span data-stu-id="1a5b1-355">Kannada</span></span></td>
        <td><span data-ttu-id="1a5b1-356">KA.Microsoft</span><span class="sxs-lookup"><span data-stu-id="1a5b1-356">ka.microsoft</span></span></td>
        <td></td>
    </tr>
    <tr>
        <td><span data-ttu-id="1a5b1-357">Koreanska</span><span class="sxs-lookup"><span data-stu-id="1a5b1-357">Korean</span></span></td>
        <td><span data-ttu-id="1a5b1-358">Ko.Microsoft</span><span class="sxs-lookup"><span data-stu-id="1a5b1-358">ko.microsoft</span></span></td>
        <td><span data-ttu-id="1a5b1-359">Ko.lucene</span><span class="sxs-lookup"><span data-stu-id="1a5b1-359">ko.lucene</span></span></td>
    </tr>
    <tr>
        <td><span data-ttu-id="1a5b1-360">Lettiska</span><span class="sxs-lookup"><span data-stu-id="1a5b1-360">Latvian</span></span></td>        
        <td><span data-ttu-id="1a5b1-361">LV.Microsoft</span><span class="sxs-lookup"><span data-stu-id="1a5b1-361">lv.microsoft</span></span></td>
        <td><span data-ttu-id="1a5b1-362">LV.lucene</span><span class="sxs-lookup"><span data-stu-id="1a5b1-362">lv.lucene</span></span></td>    
    </tr>
    <tr>
        <td><span data-ttu-id="1a5b1-363">Litauiska</span><span class="sxs-lookup"><span data-stu-id="1a5b1-363">Lithuanian</span></span></td>
        <td><span data-ttu-id="1a5b1-364">lt.Microsoft</span><span class="sxs-lookup"><span data-stu-id="1a5b1-364">lt.microsoft</span></span></td>
        <td></td>
    </tr>
    <tr>
        <td><span data-ttu-id="1a5b1-365">Malayalam</span><span class="sxs-lookup"><span data-stu-id="1a5b1-365">Malayalam</span></span></td>
        <td><span data-ttu-id="1a5b1-366">ml.Microsoft</span><span class="sxs-lookup"><span data-stu-id="1a5b1-366">ml.microsoft</span></span></td>
        <td></td>
    </tr>
    <tr>
        <td><span data-ttu-id="1a5b1-367">Malajiska (latinsk)</span><span class="sxs-lookup"><span data-stu-id="1a5b1-367">Malay (Latin)</span></span></td>
        <td><span data-ttu-id="1a5b1-368">MS.Microsoft</span><span class="sxs-lookup"><span data-stu-id="1a5b1-368">ms.microsoft</span></span></td>
        <td></td>
    </tr>
    <tr>
        <td><span data-ttu-id="1a5b1-369">Marathi</span><span class="sxs-lookup"><span data-stu-id="1a5b1-369">Marathi</span></span></td>
        <td><span data-ttu-id="1a5b1-370">MR.Microsoft</span><span class="sxs-lookup"><span data-stu-id="1a5b1-370">mr.microsoft</span></span></td>
        <td></td>
    </tr>
    <tr>
        <td><span data-ttu-id="1a5b1-371">Norska</span><span class="sxs-lookup"><span data-stu-id="1a5b1-371">Norwegian</span></span></td>
        <td><span data-ttu-id="1a5b1-372">NB.Microsoft</span><span class="sxs-lookup"><span data-stu-id="1a5b1-372">nb.microsoft</span></span></td>
        <td><span data-ttu-id="1a5b1-373">No.lucene</span><span class="sxs-lookup"><span data-stu-id="1a5b1-373">no.lucene</span></span></td>        
    </tr>
      <tr>
        <td><span data-ttu-id="1a5b1-374">Persiska</span><span class="sxs-lookup"><span data-stu-id="1a5b1-374">Persian</span></span></td>
        <td></td>
        <td><span data-ttu-id="1a5b1-375">fa.lucene</span><span class="sxs-lookup"><span data-stu-id="1a5b1-375">fa.lucene</span></span></td>        
      </tr>
    <tr>
        <td><span data-ttu-id="1a5b1-376">Polska</span><span class="sxs-lookup"><span data-stu-id="1a5b1-376">Polish</span></span></td>
        <td><span data-ttu-id="1a5b1-377">PL.Microsoft</span><span class="sxs-lookup"><span data-stu-id="1a5b1-377">pl.microsoft</span></span></td>
        <td><span data-ttu-id="1a5b1-378">PL.lucene</span><span class="sxs-lookup"><span data-stu-id="1a5b1-378">pl.lucene</span></span></td>        
    </tr>
    <tr>
        <td><span data-ttu-id="1a5b1-379">Portugisiska (Brasilien)</span><span class="sxs-lookup"><span data-stu-id="1a5b1-379">Portuguese (Brazil)</span></span></td>
        <td><span data-ttu-id="1a5b1-380">pt-Br.microsoft</span><span class="sxs-lookup"><span data-stu-id="1a5b1-380">pt-Br.microsoft</span></span></td>
        <td><span data-ttu-id="1a5b1-381">pt-Br.lucene</span><span class="sxs-lookup"><span data-stu-id="1a5b1-381">pt-Br.lucene</span></span></td>        
    </tr>
    <tr>
        <td><span data-ttu-id="1a5b1-382">Portugisiska (Portugal)</span><span class="sxs-lookup"><span data-stu-id="1a5b1-382">Portuguese (Portugal)</span></span></td>
        <td><span data-ttu-id="1a5b1-383">pt-Pt.microsoft</span><span class="sxs-lookup"><span data-stu-id="1a5b1-383">pt-Pt.microsoft</span></span></td>        
        <td><span data-ttu-id="1a5b1-384">pt-Pt.lucene</span><span class="sxs-lookup"><span data-stu-id="1a5b1-384">pt-Pt.lucene</span></span></td>
    </tr>
    <tr>
        <td><span data-ttu-id="1a5b1-385">Punjabi</span><span class="sxs-lookup"><span data-stu-id="1a5b1-385">Punjabi</span></span></td>
        <td><span data-ttu-id="1a5b1-386">pa.Microsoft</span><span class="sxs-lookup"><span data-stu-id="1a5b1-386">pa.microsoft</span></span></td>
        <td></td>
    </tr>
    <tr>
        <td><span data-ttu-id="1a5b1-387">Rumänska</span><span class="sxs-lookup"><span data-stu-id="1a5b1-387">Romanian</span></span></td>
        <td><span data-ttu-id="1a5b1-388">ro.Microsoft</span><span class="sxs-lookup"><span data-stu-id="1a5b1-388">ro.microsoft</span></span></td>
        <td><span data-ttu-id="1a5b1-389">ro.lucene</span><span class="sxs-lookup"><span data-stu-id="1a5b1-389">ro.lucene</span></span></td>
    </tr>
    <tr>
        <td><span data-ttu-id="1a5b1-390">Ryska</span><span class="sxs-lookup"><span data-stu-id="1a5b1-390">Russian</span></span></td>
        <td><span data-ttu-id="1a5b1-391">RU.Microsoft</span><span class="sxs-lookup"><span data-stu-id="1a5b1-391">ru.microsoft</span></span></td>
        <td><span data-ttu-id="1a5b1-392">RU.lucene</span><span class="sxs-lookup"><span data-stu-id="1a5b1-392">ru.lucene</span></span></td>    
    </tr>
    <tr>
        <td><span data-ttu-id="1a5b1-393">Serbiska (kyrillisk)</span><span class="sxs-lookup"><span data-stu-id="1a5b1-393">Serbian (Cyrillic)</span></span></td>
        <td><span data-ttu-id="1a5b1-394">SR-cyrillic.microsoft</span><span class="sxs-lookup"><span data-stu-id="1a5b1-394">sr-cyrillic.microsoft</span></span></td>
        <td></td>
    </tr>
    <tr>
        <td><span data-ttu-id="1a5b1-395">Serbiska (latinsk)</span><span class="sxs-lookup"><span data-stu-id="1a5b1-395">Serbian (Latin)</span></span></td>
        <td><span data-ttu-id="1a5b1-396">SR-latin.microsoft</span><span class="sxs-lookup"><span data-stu-id="1a5b1-396">sr-latin.microsoft</span></span></td>
        <td></td>
    </tr>
    <tr>
        <td><span data-ttu-id="1a5b1-397">Slovakiska</span><span class="sxs-lookup"><span data-stu-id="1a5b1-397">Slovak</span></span></td>
        <td><span data-ttu-id="1a5b1-398">Sk.Microsoft</span><span class="sxs-lookup"><span data-stu-id="1a5b1-398">sk.microsoft</span></span></td>
        <td></td>
    </tr>
    <tr>
        <td><span data-ttu-id="1a5b1-399">Slovenska</span><span class="sxs-lookup"><span data-stu-id="1a5b1-399">Slovenian</span></span></td>
        <td><span data-ttu-id="1a5b1-400">Sl.Microsoft</span><span class="sxs-lookup"><span data-stu-id="1a5b1-400">sl.microsoft</span></span></td>
        <td></td>
    </tr>
    <tr>
        <td><span data-ttu-id="1a5b1-401">Spanska</span><span class="sxs-lookup"><span data-stu-id="1a5b1-401">Spanish</span></span></td>
        <td><span data-ttu-id="1a5b1-402">ES.Microsoft</span><span class="sxs-lookup"><span data-stu-id="1a5b1-402">es.microsoft</span></span></td>
        <td><span data-ttu-id="1a5b1-403">ES.lucene</span><span class="sxs-lookup"><span data-stu-id="1a5b1-403">es.lucene</span></span></td>
    </tr>
    <tr>
        <td><span data-ttu-id="1a5b1-404">Svenska</span><span class="sxs-lookup"><span data-stu-id="1a5b1-404">Swedish</span></span></td>
        <td><span data-ttu-id="1a5b1-405">SV.Microsoft</span><span class="sxs-lookup"><span data-stu-id="1a5b1-405">sv.microsoft</span></span></td>
        <td><span data-ttu-id="1a5b1-406">SV.lucene</span><span class="sxs-lookup"><span data-stu-id="1a5b1-406">sv.lucene</span></span></td>
    </tr>

    <tr>
        <td><span data-ttu-id="1a5b1-407">Tamilska</span><span class="sxs-lookup"><span data-stu-id="1a5b1-407">Tamil</span></span></td>
        <td><span data-ttu-id="1a5b1-408">ta.Microsoft</span><span class="sxs-lookup"><span data-stu-id="1a5b1-408">ta.microsoft</span></span></td>
        <td></td>
    </tr>
    <tr>
        <td><span data-ttu-id="1a5b1-409">Telugu</span><span class="sxs-lookup"><span data-stu-id="1a5b1-409">Telugu</span></span></td>
        <td><span data-ttu-id="1a5b1-410">te.Microsoft</span><span class="sxs-lookup"><span data-stu-id="1a5b1-410">te.microsoft</span></span></td>
        <td></td>
    </tr>
    <tr>
        <td><span data-ttu-id="1a5b1-411">Thai</span><span class="sxs-lookup"><span data-stu-id="1a5b1-411">Thai</span></span></td>
        <td><span data-ttu-id="1a5b1-412">TH.Microsoft</span><span class="sxs-lookup"><span data-stu-id="1a5b1-412">th.microsoft</span></span></td>
        <td><span data-ttu-id="1a5b1-413">TH.lucene</span><span class="sxs-lookup"><span data-stu-id="1a5b1-413">th.lucene</span></span></td>
    </tr>
    <tr>
        <td><span data-ttu-id="1a5b1-414">Turkiska</span><span class="sxs-lookup"><span data-stu-id="1a5b1-414">Turkish</span></span></td>
        <td><span data-ttu-id="1a5b1-415">TR.Microsoft</span><span class="sxs-lookup"><span data-stu-id="1a5b1-415">tr.microsoft</span></span></td>
        <td><span data-ttu-id="1a5b1-416">TR.lucene</span><span class="sxs-lookup"><span data-stu-id="1a5b1-416">tr.lucene</span></span></td>        
    </tr>
    <tr>
        <td><span data-ttu-id="1a5b1-417">Ukrainska</span><span class="sxs-lookup"><span data-stu-id="1a5b1-417">Ukrainian</span></span></td>
        <td><span data-ttu-id="1a5b1-418">UK.Microsoft</span><span class="sxs-lookup"><span data-stu-id="1a5b1-418">uk.microsoft</span></span></td>
        <td></td>
    </tr>
    <tr>
        <td><span data-ttu-id="1a5b1-419">Urdu</span><span class="sxs-lookup"><span data-stu-id="1a5b1-419">Urdu</span></span></td>
        <td><span data-ttu-id="1a5b1-420">Your.Microsoft</span><span class="sxs-lookup"><span data-stu-id="1a5b1-420">ur.microsoft</span></span></td>
        <td></td>
    </tr>
    <tr>
        <td><span data-ttu-id="1a5b1-421">Vietnamesiska</span><span class="sxs-lookup"><span data-stu-id="1a5b1-421">Vietnamese</span></span></td>
        <td><span data-ttu-id="1a5b1-422">Vi.Microsoft</span><span class="sxs-lookup"><span data-stu-id="1a5b1-422">vi.microsoft</span></span></td>
        <td></td>
    </tr>
    <td colspan="3"><span data-ttu-id="1a5b1-423">Dessutom ger Azure Search språkoberoende analyzer konfigurationer</span><span class="sxs-lookup"><span data-stu-id="1a5b1-423">Additionally Azure Search provides language-agnostic analyzer configurations</span></span></td>
    <tr>
        <td><span data-ttu-id="1a5b1-424">Standard-ASCII vikning</span><span class="sxs-lookup"><span data-stu-id="1a5b1-424">Standard ASCII Folding</span></span></td>
        <td><span data-ttu-id="1a5b1-425">standardasciifolding.lucene</span><span class="sxs-lookup"><span data-stu-id="1a5b1-425">standardasciifolding.lucene</span></span></td>
        <td>
        <ul>
            <li><span data-ttu-id="1a5b1-426">Unicode-text segmentering (Standard Tokenizer)</span><span class="sxs-lookup"><span data-stu-id="1a5b1-426">Unicode text segmentation (Standard Tokenizer)</span></span></li>
            <li><span data-ttu-id="1a5b1-427">Vikning filtret för ASCII - omvandlar Unicode-tecken som inte tillhör uppsättningen först 127 ASCII-tecken till deras motsvarigheter i ASCII.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-427">ASCII folding filter - converts Unicode characters that don't belong to the set of first 127 ASCII characters into their ASCII equivalents.</span></span> <span data-ttu-id="1a5b1-428">Detta är användbart för att ta bort diakritiska tecken.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-428">This is useful for removing diacritics.</span></span></li>
        </ul>
        </td>
    </tr>
</table>

<span data-ttu-id="1a5b1-429">Alla analyzers med namn som är försedd med <i>lucene</i> drivs av [Apache Lucene språkanalys](http://lucene.apache.org/core/4_9_0/analyzers-common/overview-summary.html).</span><span class="sxs-lookup"><span data-stu-id="1a5b1-429">All analyzers with names annotated with <i>lucene</i> are powered by [Apache Lucene's language analyzers](http://lucene.apache.org/core/4_9_0/analyzers-common/overview-summary.html).</span></span> <span data-ttu-id="1a5b1-430">Mer information om ASCII vikning filtret hittar [här](http://lucene.apache.org/core/4_9_0/analyzers-common/org/apache/lucene/analysis/miscellaneous/ASCIIFoldingFilter.html).</span><span class="sxs-lookup"><span data-stu-id="1a5b1-430">More information about the ASCII folding filter can be found [here](http://lucene.apache.org/core/4_9_0/analyzers-common/org/apache/lucene/analysis/miscellaneous/ASCIIFoldingFilter.html).</span></span>

<span data-ttu-id="1a5b1-431">**Förslag på alternativ**</span><span class="sxs-lookup"><span data-stu-id="1a5b1-431">**Suggesters**</span></span>

<span data-ttu-id="1a5b1-432">En `suggester` definierar vilka fält i ett index som används för att stödja automatisk komplettering i sökningar.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-432">A `suggester` defines which fields in an index are used to support auto-complete in searches.</span></span> <span data-ttu-id="1a5b1-433">Vanligtvis partiella söksträngar skickas till den [förslag API](#Suggestions) när användaren skriver en sökfråga och API: N returnerar en uppsättning föreslagna fraser.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-433">Typically partial search strings are sent to the [Suggestions API](#Suggestions) while the user is typing a search query, and the API returns a set of suggested phrases.</span></span> <span data-ttu-id="1a5b1-434">En förslagsställarens som du definierar i indexet avgör vilka fält som används för att skapa typen ahead sökvillkoren.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-434">A suggester that you define in the index determines which fields are used to build the type-ahead search terms.</span></span> <span data-ttu-id="1a5b1-435">Se [Suggesters](#Suggesters) konfigurationsinformation.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-435">See [Suggesters](#Suggesters) for configuration details.</span></span>

<span data-ttu-id="1a5b1-436">**Poängprofiler**</span><span class="sxs-lookup"><span data-stu-id="1a5b1-436">**Scoring profiles**</span></span>

<span data-ttu-id="1a5b1-437">En `scoringProfile` definierar anpassade bedömningsprofil beteenden som gör att du kan påverka vilka objekt som visas överst i sökresultaten.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-437">A `scoringProfile` defines custom scoring behaviors that let you influence which items appear higher in the search results.</span></span> <span data-ttu-id="1a5b1-438">Bedömningsprofil profiler består av fältvikter och funktioner.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-438">Scoring profiles are made up of field weights and functions.</span></span> <span data-ttu-id="1a5b1-439">Om du vill använda dem anger du en profil med namnet på frågesträngen.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-439">To use them, you specify a profile by name on the query string.</span></span>

<span data-ttu-id="1a5b1-440">Standard bedömningen profilen fungerar i bakgrunden att beräkna en sökning poäng för alla objekt i en resultatuppsättning.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-440">A default scoring profile operates behind the scenes to compute a search score for every item in a result set.</span></span> <span data-ttu-id="1a5b1-441">Du kan använda den interna namnlösa bedömningsprofilen.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-441">You can use the internal, unnamed scoring profile.</span></span> <span data-ttu-id="1a5b1-442">Du kan också ange `defaultScoringProfile` att använda en anpassad profil som standard, anropas när en anpassad profil inte anges i frågesträngen.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-442">Alternatively, set `defaultScoringProfile` to use a custom profile as the default, invoked whenever a custom profile is not specified on the query string.</span></span>

<span data-ttu-id="1a5b1-443">Se [Lägg till bedömningsprofil profiler sökindex (Azure Söktjänsts-REST API)](search-api-scoring-profiles-2015-02-28-preview.md) mer information.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-443">See [Add scoring profiles to a search index (Azure Search Service REST API)](search-api-scoring-profiles-2015-02-28-preview.md) for details.</span></span>

<span data-ttu-id="1a5b1-444">**Alternativ för CORS**</span><span class="sxs-lookup"><span data-stu-id="1a5b1-444">**CORS Options**</span></span>

<span data-ttu-id="1a5b1-445">Javascript för klientsidan kan inte anropa alla API: er som standard eftersom webbläsaren hindrar alla cross-origin-begäranden.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-445">Client-side Javascript cannot call any APIs by default since the browser will prevent all cross-origin requests.</span></span> <span data-ttu-id="1a5b1-446">Aktivera CORS (Cross-Origin Resource Sharing) genom att ange den `corsOptions` attribut för att tillåta cross-origin frågor till ditt index.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-446">Enable CORS (Cross-Origin Resource Sharing) by setting the `corsOptions` attribute to allow cross-origin queries to your index.</span></span> <span data-ttu-id="1a5b1-447">Observera att endast query API: er stöd CORS av säkerhetsskäl.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-447">Note that only query APIs support CORS for security reasons.</span></span> <span data-ttu-id="1a5b1-448">Följande alternativ kan anges för CORS:</span><span class="sxs-lookup"><span data-stu-id="1a5b1-448">The following options can be set for CORS:</span></span>

* <span data-ttu-id="1a5b1-449">`allowedOrigins`(krävs): det här är en lista över ursprung som beviljas åtkomst till ditt index.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-449">`allowedOrigins` (required): This is a list of origins that will be granted access to your index.</span></span> <span data-ttu-id="1a5b1-450">Detta innebär att alla Javascript-kod från dessa ursprung tillåts fråga ditt index (förutsatt att den ger rätt API-nyckel).</span><span class="sxs-lookup"><span data-stu-id="1a5b1-450">This means that any Javascript code served from those origins will be allowed to query your index (assuming it provides the correct API key).</span></span> <span data-ttu-id="1a5b1-451">Varje ursprung har vanligtvis formatet `protocol://fully-qualified-domain-name:port` även om porten utelämnas ofta.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-451">Each origin is typically of the form `protocol://fully-qualified-domain-name:port` although the port is often omitted.</span></span> <span data-ttu-id="1a5b1-452">Se [i den här artikeln](http://go.microsoft.com/fwlink/?LinkId=330822) för mer information.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-452">See [this article](http://go.microsoft.com/fwlink/?LinkId=330822) for more details.</span></span>
  * <span data-ttu-id="1a5b1-453">Om du vill tillåta åtkomst till alla ursprung `*` som ett enskilt objekt i den `allowedOrigins` matris.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-453">If you want to allow access to all origins, include `*` as a single item in the `allowedOrigins` array.</span></span> <span data-ttu-id="1a5b1-454">Observera att **detta rekommenderas inte för produktion search-tjänster.**</span><span class="sxs-lookup"><span data-stu-id="1a5b1-454">Note that **this is not recommended practice for production search services.**</span></span> <span data-ttu-id="1a5b1-455">Det kan dock vara användbar för utveckling eller felsökning.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-455">However, it may be useful for development or debugging purposes.</span></span>
* <span data-ttu-id="1a5b1-456">`maxAgeInSeconds`(valfritt): webbläsare använder det här värdet för att fastställa varaktigheten (i sekunder) för cache CORS preflight-svar.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-456">`maxAgeInSeconds` (optional): Browsers use this value to determine the duration (in seconds) to cache CORS preflight responses.</span></span> <span data-ttu-id="1a5b1-457">Detta måste vara ett icke-negativt heltal.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-457">This must be a non-negative integer.</span></span> <span data-ttu-id="1a5b1-458">Ju större det här värdet är, desto bättre prestanda kommer att men ju längre tid det tar för ändringar av CORS-principer ska gälla.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-458">The larger this value is, the better performance will be, but the longer it will take for CORS policy changes to take effect.</span></span> <span data-ttu-id="1a5b1-459">Om den inte är inställd kommer på 5 minuter att används standardvärdet.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-459">If it is not set, a default duration of 5 minutes will be used.</span></span>

<span data-ttu-id="1a5b1-460"><a name="CreateUpdateIndexExample"></a>
**Begäran Body-exempel**</span><span class="sxs-lookup"><span data-stu-id="1a5b1-460"><a name="CreateUpdateIndexExample"></a>
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

<span data-ttu-id="1a5b1-461">**Svar**</span><span class="sxs-lookup"><span data-stu-id="1a5b1-461">**Response**</span></span>

<span data-ttu-id="1a5b1-462">För en lyckad begäran: ”201 skapad”.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-462">For a successful request: "201 Created".</span></span>

<span data-ttu-id="1a5b1-463">Som standard innehåller svarstexten JSON för indexdefinitionen som har skapats.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-463">By default the response body will contain the JSON for the index definition that was created.</span></span> <span data-ttu-id="1a5b1-464">Om den `Prefer` huvudet i begäran är inställd på `return=minimal`svarstexten är tomt och statuskoden lyckas blir ”204 saknar innehåll” i stället för ”201 skapad”.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-464">If the `Prefer` request header is set to `return=minimal`, the response body will be empty and the success status code will be "204 No Content" instead of "201 Created".</span></span> <span data-ttu-id="1a5b1-465">Detta gäller oavsett om PUT eller POST användes för att skapa index.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-465">This is true regardless of whether PUT or POST was used to create the index.</span></span>

<span data-ttu-id="1a5b1-466">**Kommentarer**</span><span class="sxs-lookup"><span data-stu-id="1a5b1-466">**Remarks**</span></span>

<span data-ttu-id="1a5b1-467">Det finns för närvarande begränsat stöd för index schema-uppdateringar.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-467">Currently, there is limited support for index schema updates.</span></span> <span data-ttu-id="1a5b1-468">Schema-uppdateringar som kräver omindexering, till exempel ändring av fälttyp stöds inte för närvarande.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-468">Any schema updates that would require re-indexing such as changing field types are not currently supported.</span></span> <span data-ttu-id="1a5b1-469">Även om befintliga fält inte kan ändras eller tas bort, kan nya fält läggas till ett befintligt index när som helst.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-469">Although existing fields cannot be changed or deleted, new fields can be added to an existing index at any time.</span></span> <span data-ttu-id="1a5b1-470">När ett nytt fält läggs alla befintliga dokument i indexet automatiskt att ha ett null-värde för fältet.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-470">When a new field is added, all existing documents in the index will automatically have a null value for that field.</span></span> <span data-ttu-id="1a5b1-471">Inga ytterligare lagringsutrymme används förrän nya dokument har lagts till i indexet.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-471">No additional storage space will be consumed until new documents are added to the index.</span></span>

<a name="Suggesters"></a>

## <a name="suggesters"></a><span data-ttu-id="1a5b1-472">Förslag på alternativ</span><span class="sxs-lookup"><span data-stu-id="1a5b1-472">Suggesters</span></span>
<span data-ttu-id="1a5b1-473">Funktionen förslag i Azure Search är en typ i förväg eller automatisk komplettering fråga-funktion som ger en lista över möjliga söktermer som svar på partiella sträng indata som angetts i en sökruta.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-473">The suggestions feature in Azure Search is a type-ahead or auto-complete query capability, providing a list of potential search terms in response to partial string inputs entered into a search box.</span></span> <span data-ttu-id="1a5b1-474">Har du antagligen märkt frågeförslag när du använder kommersiella sökmotorer: att skriva ”.NET” i Bing genererar en lista över villkor för ”.NET 4.5” ”, .NET Framework 3,5-tums, och så vidare.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-474">You've probably noticed query suggestions when using commercial web search engines: typing ".NET" in Bing produces a list of terms for ".NET 4.5", ".NET Framework 3.5", and so forth.</span></span> <span data-ttu-id="1a5b1-475">När du använder tjänsten Search REST-API, kräver implementera förslag i ett anpassat program för Azure Search följande:</span><span class="sxs-lookup"><span data-stu-id="1a5b1-475">When using the Search service REST API, implementing suggestions in a custom Azure Search application requires the following:</span></span>

* <span data-ttu-id="1a5b1-476">Aktivera förslag genom att lägga till en **förslagsställarens** konstruktionen i ditt index med namnet sökläget och en lista med fält som typ-ahead anropas.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-476">Enable suggestions by adding a **suggester** construction in your index, giving the name, search mode, and a list of fields for which type-ahead is invoked.</span></span> <span data-ttu-id="1a5b1-477">Till exempel om du anger ”cityName” som ett fält i datakällan, att skriva en partiell sökning sträng med ”Sea” leder ”Seattle”, ”snäckor” och ”Seatac” (alla tre är faktiska stadsnamn) erbjuds som frågeförslag till användaren.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-477">For example, if you specify "cityName" as a source field, typing a partial search string of "Sea" will result in "Seattle", "Seaside", and "Seatac" (all three are actual city names) offered up as query suggestions to the user.</span></span>
* <span data-ttu-id="1a5b1-478">Anropa förslag genom att anropa den [förslag API](#Suggestions) i programkoden.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-478">Invoke suggestions by calling the [Suggestions API](#Suggestions) in your application code.</span></span> <span data-ttu-id="1a5b1-479">Vanligtvis skickas partiella söksträngar till tjänsten när användaren är att skriva en fråga och returnerar en uppsättning föreslagna fraser för detta API.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-479">Typically partial search strings are sent to the service while the user is typing a search query, and this API returns a set of suggested phrases.</span></span>

<span data-ttu-id="1a5b1-480">Den här artikeln förklarar hur du konfigurerar en **förslagsställarens**.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-480">This article explains how to configure a **suggester**.</span></span> <span data-ttu-id="1a5b1-481">Du bör också granska den [förslag API](#Suggestions) information om hur en förslagsställarens används.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-481">You should also review the [Suggestions API](#Suggestions) for details on how a suggester is used.</span></span>

<span data-ttu-id="1a5b1-482">**Användning**</span><span class="sxs-lookup"><span data-stu-id="1a5b1-482">**Usage**</span></span>

<span data-ttu-id="1a5b1-483">`Suggesters`skapas i index och fungerar bäst för att föreslå specifika dokument i stället för Lös ord eller fraser.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-483">`Suggesters` are created in the index and work best when used to suggest specific documents rather than loose terms or phrases.</span></span> <span data-ttu-id="1a5b1-484">Fälten bra kandidat är titlar, namn och andra relativt korta fraser som kan identifiera ett objekt.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-484">The best candidate fields are titles, names, and other relatively short phrases that can identify an item.</span></span> <span data-ttu-id="1a5b1-485">Mindre effektiva är återkommande fält, till exempel kategorier och taggar eller mycket långa fält, till exempel beskrivningar och kommentarer fält.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-485">Less effective are repetitive fields, such as categories and tags, or very long fields such as descriptions or comments fields.</span></span>

<span data-ttu-id="1a5b1-486">Som en del av indexdefinitionen, kan du lägga till en enda förslagsställarens till den `suggesters` samling.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-486">As part of the index definition, you can add a single suggester to the `suggesters` collection.</span></span> <span data-ttu-id="1a5b1-487">Egenskaper som definierar en förslagsställarens inkluderar följande:</span><span class="sxs-lookup"><span data-stu-id="1a5b1-487">Properties that define a suggester include the following:</span></span>

* <span data-ttu-id="1a5b1-488">`name`: Namnet på förslagsställarens.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-488">`name`: The name of the suggester.</span></span> <span data-ttu-id="1a5b1-489">Du kan använda namnet på förslagsställarens när du anropar den `suggest` API.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-489">You use the name of the suggester when calling the `suggest` API.</span></span>
* <span data-ttu-id="1a5b1-490">`searchMode`: Den strategi som används för att söka efter möjliga fraser.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-490">`searchMode`: The strategy used to search for candidate phrases.</span></span> <span data-ttu-id="1a5b1-491">Det enda läge som stöds för närvarande är `analyzingInfixMatching`, vilket genomför flexibla matchningar av fraser i början eller i mitten av meningarna.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-491">The only mode currently supported is `analyzingInfixMatching`, which performs flexible matching of phrases at the beginning or in the middle of sentences.</span></span>
* <span data-ttu-id="1a5b1-492">`sourceFields`: En lista med en eller flera fält som är källan till innehållet för förslag.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-492">`sourceFields`: A list of one or more fields that are the source of the content for suggestions.</span></span> <span data-ttu-id="1a5b1-493">Endast fält av typen `Edm.String` och `Collection(Edm.String)` kanske källor för förslag.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-493">Only fields of type `Edm.String` and `Collection(Edm.String)` may be sources for suggestions.</span></span> <span data-ttu-id="1a5b1-494">Endast de fält som inte har en anpassad språk analyzer ange kan användas.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-494">Only fields that don't have a custom language analyzer set can be used.</span></span>

<span data-ttu-id="1a5b1-495">**Förslagsställarens-exempel**</span><span class="sxs-lookup"><span data-stu-id="1a5b1-495">**Suggester Example**</span></span>

<span data-ttu-id="1a5b1-496">En förslagsställarens är en del av indexet.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-496">A suggester is part of the index.</span></span> <span data-ttu-id="1a5b1-497">Endast en förslagsställarens kan finnas i den `suggesters` samling i den aktuella versionen, tillsammans med fältsamlingen och `scoringProfiles`.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-497">Only one suggester can exist in the `suggesters` collection in the current version, alongside the fields collection and `scoringProfiles`.</span></span>

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
> <span data-ttu-id="1a5b1-498">Om du använder den offentliga förhandsversionen av Azure Search `suggesters` ersätter en äldre boolesk egenskap (`"suggestions": false`) som bara stöds prefix förslag för korta strängar (3-25 tecken).</span><span class="sxs-lookup"><span data-stu-id="1a5b1-498">If you used the public preview version of Azure Search, `suggesters` replaces an older boolean property (`"suggestions": false`) that only supported prefix suggestions for short strings (3-25 characters).</span></span> <span data-ttu-id="1a5b1-499">Dess ersättare `suggesters`, stöder infix matchar som söker efter matchande villkoren i början eller i mitten fältet innehåll med bättre tolerans för fel i söksträngar.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-499">Its replacement, `suggesters`, supports infix matching that finds matching terms at the beginning or in the middle of field content, with better tolerance for mistakes in search strings.</span></span> <span data-ttu-id="1a5b1-500">Från och med allmänt tillgänglig, är detta nu endast implementering av förslag API.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-500">Starting with the generally available release, this is now the only implementation of the suggestions API.</span></span> <span data-ttu-id="1a5b1-501">Den äldre `suggestions` egenskap som introducerades i `api-version=2014-07-31-Preview` fortsätter att fungera i den här versionen, men fungerar inte i den `2015-02-28` eller senare versioner av Azure Search.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-501">The older `suggestions` property that was introduced in `api-version=2014-07-31-Preview` continues to work in that version, but is not operational in the `2015-02-28` or later versions of Azure Search.</span></span>
> 
> 

<a name="UpdateIndex"></a>

## <a name="update-index"></a><span data-ttu-id="1a5b1-502">Uppdatera Index</span><span class="sxs-lookup"><span data-stu-id="1a5b1-502">Update Index</span></span>
<span data-ttu-id="1a5b1-503">Du kan uppdatera ett befintligt index i Azure Search med hjälp av en HTTP PUT-begäran.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-503">You can update an existing index within Azure Search using an HTTP PUT request.</span></span> <span data-ttu-id="1a5b1-504">Uppdateringar kan inkludera att lägga till nya fält i det befintliga schemat, ändra alternativ för CORS och ändra bedömningsprofil profiler.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-504">Updates can include adding new fields to the existing schema, modifying CORS options, and modifying scoring profiles.</span></span> <span data-ttu-id="1a5b1-505">Se [lägga till bedömningen profiler](https://msdn.microsoft.com/library/azure/dn798928.aspx) för mer information.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-505">See [Add scoring Profiles](https://msdn.microsoft.com/library/azure/dn798928.aspx) for more information.</span></span> <span data-ttu-id="1a5b1-506">Du kan ange namnet på indexet för att uppdatera på URI-begäran:</span><span class="sxs-lookup"><span data-stu-id="1a5b1-506">You specify the name of the index to update on the request URI:</span></span>

    PUT https://[search service url]/indexes/[index name]?api-version=[api-version]
    Content-Type: application/json
    api-key: [admin key]

<span data-ttu-id="1a5b1-507">**Viktigt:** stöd för index schemauppdateringar är begränsad till åtgärder som inte kräver återskapa sökindexet.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-507">**Important:** Support for index schema updates is limited to operations that don't require rebuilding the search index.</span></span> <span data-ttu-id="1a5b1-508">Schema-uppdateringar som kräver omindexering, till exempel ändring av fälttyp, stöds inte för närvarande.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-508">Any schema updates that would require re-indexing, such as changing field types, are not currently supported.</span></span> <span data-ttu-id="1a5b1-509">Nya fält läggas när som helst, även om befintliga fält inte kan ändras eller tas bort.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-509">New fields can be added at any time, although existing fields cannot be changed or deleted.</span></span> <span data-ttu-id="1a5b1-510">Samma gäller `suggesters`.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-510">The same applies to `suggesters`.</span></span> <span data-ttu-id="1a5b1-511">Nya fält kan läggas till i en förslagsställarens när fälten läggs till, men fält kan inte tas bort från `suggesters` och befintliga fält inte kan läggas till `suggesters`.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-511">New fields may be added to a suggester at the time the fields are added, but fields cannot be removed from `suggesters` and existing fields cannot be added to `suggesters`.</span></span>

<span data-ttu-id="1a5b1-512">När du lägger till ett nytt fält till ett index har alla befintliga dokument i indexet automatiskt ett null-värde för fältet.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-512">When adding a new field to an index, all existing documents in the index will automatically have a null value for that field.</span></span> <span data-ttu-id="1a5b1-513">Inga ytterligare lagringsutrymme används förrän nya dokument har lagts till i indexet.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-513">No additional storage space will be consumed until new documents are added to the index.</span></span>

<span data-ttu-id="1a5b1-514">**Förfrågan**</span><span class="sxs-lookup"><span data-stu-id="1a5b1-514">**Request**</span></span>

<span data-ttu-id="1a5b1-515">HTTPS krävs för alla tjänstbegäranden.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-515">HTTPS is required for all service requests.</span></span> <span data-ttu-id="1a5b1-516">Den **uppdatera Index** begäran har skapats med hjälp av HTTP PUT.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-516">The **Update Index** request is constructed using HTTP PUT.</span></span> <span data-ttu-id="1a5b1-517">Med PUT är Indexnamnet en del av URL: en.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-517">With PUT, the index name is part of the URL.</span></span> <span data-ttu-id="1a5b1-518">Om det inte finns index skapas.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-518">If the index doesn't exist, it is created.</span></span> <span data-ttu-id="1a5b1-519">Om indexet redan uppdateras den till den nya definitionen.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-519">If the index already exists, it is updated to the new definition.</span></span>

<span data-ttu-id="1a5b1-520">Indexnamnet måste vara gemener, börja med en bokstav eller siffra, har inga snedstreck eller punkter och vara mindre än 128 tecken.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-520">The index name must be lower case, start with a letter or number, have no slashes or dots, and be less than 128 characters.</span></span> <span data-ttu-id="1a5b1-521">När du har startat Indexnamnet med en bokstav eller siffra kan resten av namnet innehålla en bokstav, antal och tankstreck, så länge streck inte är i följd.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-521">After starting the index name with a letter or number, the rest of the name can include any letter, number and dashes, as long as the dashes are not consecutive.</span></span>

<span data-ttu-id="1a5b1-522">`api-version=[string]`(krävs).</span><span class="sxs-lookup"><span data-stu-id="1a5b1-522">`api-version=[string]` (required).</span></span> <span data-ttu-id="1a5b1-523">Förhandsversionen är `api-version=2015-02-28-Preview`.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-523">The preview version is `api-version=2015-02-28-Preview`.</span></span> <span data-ttu-id="1a5b1-524">Se [Sök Versionhantering](http://msdn.microsoft.com/library/azure/dn864560.aspx) information och alternativa versioner.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-524">See [Search Service Versioning](http://msdn.microsoft.com/library/azure/dn864560.aspx) for details and alternative versions.</span></span>

<span data-ttu-id="1a5b1-525">**Huvuden för begäran**</span><span class="sxs-lookup"><span data-stu-id="1a5b1-525">**Request Headers**</span></span>

<span data-ttu-id="1a5b1-526">I följande lista beskrivs de obligatoriska och valfria begärandehuvudena.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-526">The following list describes the required and optional request headers.</span></span>

* <span data-ttu-id="1a5b1-527">`Content-Type`: Krävs.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-527">`Content-Type`: Required.</span></span> <span data-ttu-id="1a5b1-528">Ställ in på`application/json`</span><span class="sxs-lookup"><span data-stu-id="1a5b1-528">Set this to `application/json`</span></span>
* <span data-ttu-id="1a5b1-529">`api-key`: Krävs.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-529">`api-key`: Required.</span></span> <span data-ttu-id="1a5b1-530">Den `api-key` används för att autentisera begäran om att din söktjänst.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-530">The `api-key` is used to authenticate the request to your Search service.</span></span> <span data-ttu-id="1a5b1-531">Det är ett strängvärde som unik för din tjänst.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-531">It is a string value, unique to your service.</span></span> <span data-ttu-id="1a5b1-532">Den **uppdatera Index** begäran måste innehålla en `api-key` huvudet inställt till din administrationsnyckel (i stället för en fråga nyckel).</span><span class="sxs-lookup"><span data-stu-id="1a5b1-532">The **Update Index** request must include an `api-key` header set to your admin key (as opposed to a query key).</span></span>

<span data-ttu-id="1a5b1-533">Du måste också tjänstnamnet att konstruera den begärda Webbadressen.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-533">You will also need the service name to construct the request URL.</span></span> <span data-ttu-id="1a5b1-534">Du kan hämta namnet på tjänsten och `api-key` från tjänsten instrumentpanelen i Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-534">You can get the service name and `api-key` from your service dashboard in the Azure Portal.</span></span> <span data-ttu-id="1a5b1-535">Se [skapa en Azure Search-tjänst i portalen](search-create-service-portal.md) sidan navigering hjälp.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-535">See [Create an Azure Search service in the portal](search-create-service-portal.md) for page navigation help.</span></span>

<span data-ttu-id="1a5b1-536">**Begäran brödtext Syntax**</span><span class="sxs-lookup"><span data-stu-id="1a5b1-536">**Request Body Syntax**</span></span>

<span data-ttu-id="1a5b1-537">När du uppdaterar ett befintligt index, inkluderar brödtexten den ursprungliga schemadefinitionen plus nya fält som du lägger till samt ändrade bedömningsprofil profiler, suggesters och alternativ för CORS, eventuella.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-537">When updating an existing index, the body must include the original schema definition, plus the new fields you are adding, as well as the modified scoring profiles, suggesters and CORS options, if any.</span></span> <span data-ttu-id="1a5b1-538">Om du inte ändrar bedömningsprofil profiler och alternativ för CORS, måste du inkludera original från när indexet har skapats.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-538">If you are not modifying the scoring profiles and CORS options, you must include the originals from when the index was created.</span></span> <span data-ttu-id="1a5b1-539">I allmänhet det bästa mönstret som ska användas för uppdateringar är att hämta indexdefinitionen med GET, ändra och sedan uppdatera med PUT.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-539">In general the best pattern to use for updates is to retrieve the index definition with a GET, modify it, then update it with PUT.</span></span>

<span data-ttu-id="1a5b1-540">Schema-syntax används för att skapa ett index återges här i informationssyfte.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-540">The schema syntax used to create an index is reproduced here for convenience.</span></span> <span data-ttu-id="1a5b1-541">Se [Create Index](#CreateIndex) för mer information.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-541">See [Create Index](#CreateIndex) for more details.</span></span>

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
          "analyzer": "name of the analyzer used for search and indexing", (only if 'searchAnalyzer' and 'indexAnalyzer' are not set)
          "searchAnalyzer": "name of the search analyzer", (only if 'indexAnalyzer' is set and 'analyzer' is not set)
          "indexAnalyzer": "name of the indexing analyzer" (only if 'searchAnalyzer' is set and 'analyzer' is not set)
        }
      ],
      "suggesters": [
        {
          "name": "name of suggester",
          "searchMode": "analyzingInfixMatching" (other modes may be added in the future),
          "sourceFields": ["field1", "field2", ...]
        }
      ],
      "scoringProfiles": [
        {
          "name": "name of scoring profile",
          "text": (optional, only applies to searchable fields) {
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
                "boostingDuration": "..." (value representing timespan leading to now over which boosting occurs)
              },
              "distance": {
                "referencePointParameter": "...", (parameter to be passed in queries to use as reference location, see "scoringParameter" for syntax details)
                "boostingDistance": # (the distance in kilometers from the reference location where the boosting range ends)
              },
              "tag": {
                "tagsParameter": "..." (parameter to be passed in queries to specify list of tags to compare against target field, see "scoringParameter" for syntax details)
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


<span data-ttu-id="1a5b1-542">**Svar**</span><span class="sxs-lookup"><span data-stu-id="1a5b1-542">**Response**</span></span>

<span data-ttu-id="1a5b1-543">För en lyckad begäran ”: 204 saknar innehåll”.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-543">For a successful request: "204 No Content".</span></span>

<span data-ttu-id="1a5b1-544">Som standard är svarstexten tom.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-544">By default the response body will be empty.</span></span> <span data-ttu-id="1a5b1-545">Men om den `Prefer` huvudet i begäran är inställd på `return=representation`, svarstexten innehåller JSON för indexdefinitionen som har uppdaterats.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-545">However, if the `Prefer` request header is set to `return=representation`, the response body will contain the JSON for the index definition that was updated.</span></span> <span data-ttu-id="1a5b1-546">I det här fallet statuskoden lyckas blir ”200 OK”.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-546">In this case, the success status code will be "200 OK".</span></span>

<span data-ttu-id="1a5b1-547">**Uppdaterar indexdefinitionen med anpassade analyzers**</span><span class="sxs-lookup"><span data-stu-id="1a5b1-547">**Updating index definition with custom analyzers**</span></span>

<span data-ttu-id="1a5b1-548">När en analyzer, en tokenizer, ett token filter eller ett char-filter har definierats kan inte ändras.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-548">Once an analyzer, a tokenizer, a token filter or a char filter is defined, it cannot be modified.</span></span> <span data-ttu-id="1a5b1-549">Nya kan läggas till ett befintligt index endast om den `allowIndexDowntime` -flaggan inställd på true i begäran om uppdatering index:</span><span class="sxs-lookup"><span data-stu-id="1a5b1-549">New ones can be added to an existing index only if the `allowIndexDowntime` flag is set to true in the index update request:</span></span> 

`PUT https://[search service name].search.windows.net/indexes/[index name]?api-version=[api-version]&allowIndexDowntime=true`

<span data-ttu-id="1a5b1-550">Observera att den här åtgärden placeras indexet offline för minst ett par sekunder, och din indexering och fråga begäranden att misslyckas.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-550">Note that this operation will put your index offline for at least a few seconds, causing your indexing and query requests to fail.</span></span> <span data-ttu-id="1a5b1-551">Prestanda- och skrivbehörighet tillgängligheten för indexet kan vara försämrad i flera minuter när indexet uppdateras eller längre för mycket stora index.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-551">Performance and write availability of the index can be impaired for several minutes after the index is updated, or longer for very large indexes.</span></span>

<a name="ListIndexes"></a>

## <a name="list-indexes"></a><span data-ttu-id="1a5b1-552">Lista över index</span><span class="sxs-lookup"><span data-stu-id="1a5b1-552">List Indexes</span></span>
<span data-ttu-id="1a5b1-553">Den **listan index** åtgärden returnerar en lista över index som för närvarande i Azure Search-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-553">The **List Indexes** operation returns a list of the indexes currently in your Azure Search service.</span></span>

    GET https://[service name].search.windows.net/indexes?api-version=[api-version]
    api-key: [admin key]

<span data-ttu-id="1a5b1-554">**Förfrågan**</span><span class="sxs-lookup"><span data-stu-id="1a5b1-554">**Request**</span></span>

<span data-ttu-id="1a5b1-555">HTTPS krävs för alla tjänstbegäranden.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-555">HTTPS is required for all service requests.</span></span> <span data-ttu-id="1a5b1-556">Den **listan index** begäran kan konstrueras med GET-metoden.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-556">The **List Indexes** request can be constructed using the GET method.</span></span>

<span data-ttu-id="1a5b1-557">`api-version=[string]`(krävs).</span><span class="sxs-lookup"><span data-stu-id="1a5b1-557">`api-version=[string]` (required).</span></span> <span data-ttu-id="1a5b1-558">Förhandsversionen är `api-version=2015-02-28-Preview`.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-558">The preview version is `api-version=2015-02-28-Preview`.</span></span> <span data-ttu-id="1a5b1-559">Se [Sök Versionhantering](http://msdn.microsoft.com/library/azure/dn864560.aspx) information och alternativa versioner.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-559">See [Search Service Versioning](http://msdn.microsoft.com/library/azure/dn864560.aspx) for details and alternative versions.</span></span>

<span data-ttu-id="1a5b1-560">**Huvuden för begäran**</span><span class="sxs-lookup"><span data-stu-id="1a5b1-560">**Request Headers**</span></span>

<span data-ttu-id="1a5b1-561">I följande lista beskrivs de obligatoriska och valfria begärandehuvudena.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-561">The following list describes the required and optional request headers.</span></span>

* <span data-ttu-id="1a5b1-562">`api-key`: Krävs.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-562">`api-key`: Required.</span></span> <span data-ttu-id="1a5b1-563">Den `api-key` används för att autentisera begäran om att din söktjänst.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-563">The `api-key` is used to authenticate the request to your Search service.</span></span> <span data-ttu-id="1a5b1-564">Det är ett strängvärde som unik för din tjänst.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-564">It is a string value, unique to your service.</span></span> <span data-ttu-id="1a5b1-565">Den **listan index** begäran måste innehålla en `api-key` inställd på en administrationsnyckel (i stället för en fråga nyckel).</span><span class="sxs-lookup"><span data-stu-id="1a5b1-565">The **List Indexes** request must include an `api-key` set to an admin key (as opposed to a query key).</span></span>

<span data-ttu-id="1a5b1-566">Du måste också tjänstnamnet att konstruera den begärda Webbadressen.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-566">You will also need the service name to construct the request URL.</span></span> <span data-ttu-id="1a5b1-567">Du kan hämta namnet på tjänsten och `api-key` från tjänsten instrumentpanelen i Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-567">You can get the service name and `api-key` from your service dashboard in the Azure Portal.</span></span> <span data-ttu-id="1a5b1-568">Se [skapa en Azure Search-tjänst i portalen](search-create-service-portal.md) sidan navigering hjälp.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-568">See [Create an Azure Search service in the portal](search-create-service-portal.md) for page navigation help.</span></span>

<span data-ttu-id="1a5b1-569">**Begärandetexten**</span><span class="sxs-lookup"><span data-stu-id="1a5b1-569">**Request Body**</span></span>

<span data-ttu-id="1a5b1-570">Ingen.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-570">None.</span></span>

<span data-ttu-id="1a5b1-571">**Svar**</span><span class="sxs-lookup"><span data-stu-id="1a5b1-571">**Response**</span></span>

<span data-ttu-id="1a5b1-572">Statuskod: 200 OK returneras för ett lyckat svar.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-572">Status Code: 200 OK is returned for a successful response.</span></span>

<span data-ttu-id="1a5b1-573">Här är ett exempel svarstexten:</span><span class="sxs-lookup"><span data-stu-id="1a5b1-573">Here is an example response body:</span></span>

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

<span data-ttu-id="1a5b1-574">Observera att du kan filtrera svaret till bara de egenskaper som du är intresserad av.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-574">Note that you can filter the response down to just the properties you're interested in.</span></span> <span data-ttu-id="1a5b1-575">Till exempel om du vill att endast en lista över namn på index använda OData `$select` frågealternativet:</span><span class="sxs-lookup"><span data-stu-id="1a5b1-575">For example, if you want only a list of index names, use the OData `$select` query option:</span></span>

    GET /indexes?api-version=2015-02-28-Preview&$select=name

<span data-ttu-id="1a5b1-576">I det här fallet visas svaret från exemplet ovan på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="1a5b1-576">In this case, the response from the above example would appear as follows:</span></span>

    {
      "value": [
        {"name": "Books"},
        {"name": "Games"},
        ...
      ]
    }

<span data-ttu-id="1a5b1-577">Detta är en användbar teknik för att spara bandbredd om du har många index i din söktjänst.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-577">This is a useful technique to save bandwidth if you have a lot of indexes in your Search service.</span></span>

<a name="GetIndex"></a>

## <a name="get-index"></a><span data-ttu-id="1a5b1-578">Hämta Index</span><span class="sxs-lookup"><span data-stu-id="1a5b1-578">Get Index</span></span>
<span data-ttu-id="1a5b1-579">Den **hämta Index** åtgärden hämtar indexdefinitionen från Azure Search.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-579">The **Get Index** operation gets the index definition from Azure Search.</span></span>

    GET https://[service name].search.windows.net/indexes/[index name]?api-version=[api-version]
    api-key: [admin key]

<span data-ttu-id="1a5b1-580">**Förfrågan**</span><span class="sxs-lookup"><span data-stu-id="1a5b1-580">**Request**</span></span>

<span data-ttu-id="1a5b1-581">HTTPS krävs för tjänstbegäranden.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-581">HTTPS is required for service requests.</span></span> <span data-ttu-id="1a5b1-582">Den **hämta Index** begäran kan konstrueras med GET-metoden.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-582">The **Get Index** request can be constructed using the GET method.</span></span>

<span data-ttu-id="1a5b1-583">[Index name] i URI-begäran anger det index som ska returneras från samlingen index.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-583">The [index name] in the request URI specifies which index to return from the indexes collection.</span></span>

<span data-ttu-id="1a5b1-584">`api-version=[string]`(krävs).</span><span class="sxs-lookup"><span data-stu-id="1a5b1-584">`api-version=[string]` (required).</span></span> <span data-ttu-id="1a5b1-585">Förhandsversionen är `api-version=2015-02-28-Preview`.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-585">The preview version is `api-version=2015-02-28-Preview`.</span></span> <span data-ttu-id="1a5b1-586">Se [Sök Versionhantering](http://msdn.microsoft.com/library/azure/dn864560.aspx) information och alternativa versioner.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-586">See [Search Service Versioning](http://msdn.microsoft.com/library/azure/dn864560.aspx) for details and alternative versions.</span></span>

<span data-ttu-id="1a5b1-587">**Huvuden för begäran**</span><span class="sxs-lookup"><span data-stu-id="1a5b1-587">**Request Headers**</span></span>

<span data-ttu-id="1a5b1-588">I följande lista beskrivs de obligatoriska och valfria begärandehuvudena.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-588">The following list describes the required and optional request headers.</span></span>

* <span data-ttu-id="1a5b1-589">`api-key`: `api-key` Används för att autentisera begäran om att din söktjänst.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-589">`api-key`: The `api-key` is used to authenticate the request to your Search service.</span></span> <span data-ttu-id="1a5b1-590">Det är ett strängvärde som unik för din tjänst.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-590">It is a string value, unique to your service.</span></span> <span data-ttu-id="1a5b1-591">Den **hämta Index** begäran måste innehålla en `api-key` inställd på en administrationsnyckel (i stället för en fråga nyckel).</span><span class="sxs-lookup"><span data-stu-id="1a5b1-591">The **Get Index** request must include an `api-key` set to an admin key (as opposed to a query key).</span></span>

<span data-ttu-id="1a5b1-592">Du måste också tjänstnamnet att konstruera den begärda Webbadressen.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-592">You will also need the service name to construct the request URL.</span></span> <span data-ttu-id="1a5b1-593">Du kan hämta namnet på tjänsten och `api-key` från tjänsten instrumentpanelen i Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-593">You can get the service name and `api-key` from your service dashboard in the Azure Portal.</span></span> <span data-ttu-id="1a5b1-594">Se [skapa en Azure Search-tjänst i portalen](search-create-service-portal.md) sidan navigering hjälp.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-594">See [Create an Azure Search service in the portal](search-create-service-portal.md) for page navigation help.</span></span>

<span data-ttu-id="1a5b1-595">**Begärandetexten**</span><span class="sxs-lookup"><span data-stu-id="1a5b1-595">**Request Body**</span></span>

<span data-ttu-id="1a5b1-596">Ingen.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-596">None.</span></span>

<span data-ttu-id="1a5b1-597">**Svar**</span><span class="sxs-lookup"><span data-stu-id="1a5b1-597">**Response**</span></span>

<span data-ttu-id="1a5b1-598">Statuskod: 200 OK returneras för ett lyckat svar.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-598">Status Code: 200 OK is returned for a successful response.</span></span>

<span data-ttu-id="1a5b1-599">Du ser i exemplet JSON i [att skapa och uppdatera ett Index](#CreateUpdateIndexExample) ett exempel på nyttolasten svar.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-599">See the example JSON in [Creating and Updating an Index](#CreateUpdateIndexExample) for an example of the response payload.</span></span>

<a name="DeleteIndex"></a>

## <a name="delete-index"></a><span data-ttu-id="1a5b1-600">Ta bort indexet</span><span class="sxs-lookup"><span data-stu-id="1a5b1-600">Delete Index</span></span>
<span data-ttu-id="1a5b1-601">Den **ta bort indexet** åtgärden tar bort ett index och associerade dokument från din Azure Search-tjänst.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-601">The **Delete Index** operation removes an index and associated documents from your Azure Search service.</span></span> <span data-ttu-id="1a5b1-602">Du kan hämta Indexnamnet från tjänsten instrumentpanelen i Azure portal och API: et.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-602">You can get the index name from the service dashboard in the Azure portal, or from the API.</span></span> <span data-ttu-id="1a5b1-603">Se [listan index](#ListIndexes) mer information.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-603">See [List Indexes](#ListIndexes) for details.</span></span>

    DELETE https://[service name].search.windows.net/indexes/[index name]?api-version=[api-version]
    api-key: [admin key]

<span data-ttu-id="1a5b1-604">**Förfrågan**</span><span class="sxs-lookup"><span data-stu-id="1a5b1-604">**Request**</span></span>

<span data-ttu-id="1a5b1-605">HTTPS krävs för tjänstbegäranden.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-605">HTTPS is required for service requests.</span></span> <span data-ttu-id="1a5b1-606">Den **ta bort indexet** begäran kan konstrueras med DELETE-metoden.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-606">The **Delete Index** request can be constructed using the DELETE method.</span></span>

<span data-ttu-id="1a5b1-607">[Index name] i URI-begäran anger det index som ska tas bort från samlingen index.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-607">The [index name] in the request URI specifies which index to delete from the indexes collection.</span></span>

<span data-ttu-id="1a5b1-608">`api-version=[string]`(krävs).</span><span class="sxs-lookup"><span data-stu-id="1a5b1-608">`api-version=[string]` (required).</span></span> <span data-ttu-id="1a5b1-609">Förhandsversionen är `api-version=2015-02-28-Preview`.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-609">The preview version is `api-version=2015-02-28-Preview`.</span></span> <span data-ttu-id="1a5b1-610">Se [Sök Versionhantering](http://msdn.microsoft.com/library/azure/dn864560.aspx) information och alternativa versioner.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-610">See [Search Service Versioning](http://msdn.microsoft.com/library/azure/dn864560.aspx) for details and alternative versions.</span></span>

<span data-ttu-id="1a5b1-611">**Huvuden för begäran**</span><span class="sxs-lookup"><span data-stu-id="1a5b1-611">**Request Headers**</span></span>

<span data-ttu-id="1a5b1-612">I följande lista beskrivs de obligatoriska och valfria begärandehuvudena.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-612">The following list describes the required and optional request headers.</span></span>

* <span data-ttu-id="1a5b1-613">`api-key`: Krävs.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-613">`api-key`: Required.</span></span> <span data-ttu-id="1a5b1-614">Den `api-key` används för att autentisera begäran om att din söktjänst.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-614">The `api-key` is used to authenticate the request to your Search service.</span></span> <span data-ttu-id="1a5b1-615">Det är ett strängvärde som unik för din tjänst-URL.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-615">It is a string value, unique to your service URL.</span></span> <span data-ttu-id="1a5b1-616">Den **ta bort indexet** begäran måste innehålla en `api-key` huvudet inställt till din administrationsnyckel (i stället för en fråga nyckel).</span><span class="sxs-lookup"><span data-stu-id="1a5b1-616">The **Delete Index** request must include an `api-key` header set to your admin key (as opposed to a query key).</span></span>

<span data-ttu-id="1a5b1-617">Du måste också tjänstnamnet att konstruera den begärda Webbadressen.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-617">You will also need the service name to construct the request URL.</span></span> <span data-ttu-id="1a5b1-618">Du kan hämta namnet på tjänsten och `api-key` från tjänsten instrumentpanelen i Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-618">You can get the service name and `api-key` from your service dashboard in the Azure Portal.</span></span> <span data-ttu-id="1a5b1-619">Se [skapa en Azure Search-tjänst i portalen](search-create-service-portal.md) sidan navigering hjälp.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-619">See [Create an Azure Search service in the portal](search-create-service-portal.md) for page navigation help.</span></span>

<span data-ttu-id="1a5b1-620">**Begärandetexten**</span><span class="sxs-lookup"><span data-stu-id="1a5b1-620">**Request Body**</span></span>

<span data-ttu-id="1a5b1-621">Ingen.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-621">None.</span></span>

<span data-ttu-id="1a5b1-622">**Svar**</span><span class="sxs-lookup"><span data-stu-id="1a5b1-622">**Response**</span></span>

<span data-ttu-id="1a5b1-623">Statuskod: 204 Nej innehåll returneras för ett lyckat svar.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-623">Status Code: 204 No Content is returned for a successful response.</span></span>

<a name="GetIndexStats"></a>

## <a name="get-index-statistics"></a><span data-ttu-id="1a5b1-624">Få indexstatistik</span><span class="sxs-lookup"><span data-stu-id="1a5b1-624">Get Index Statistics</span></span>
<span data-ttu-id="1a5b1-625">Den **hämta indexstatistik** åtgärd returnerar från Azure Search en dokumentantal för aktuellt index plus lagringskvoten.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-625">The **Get Index Statistics** operation returns from Azure Search a document count for the current index, plus storage usage.</span></span>

    GET https://[service name].search.windows.net/indexes/[index name]/stats?api-version=[api-version]
    api-key: [admin key]

> [!NOTE]
> <span data-ttu-id="1a5b1-626">Statistik om dokumentet antal och lagringsstorlek har samlats in några minuters mellanrum, inte i realtid.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-626">Statistics on document count and storage size are collected every few minutes, not in real time.</span></span> <span data-ttu-id="1a5b1-627">Statistik som returneras av denna API kan därför inte återspegla ändringar på grund av de senaste Indexeringsåtgärder.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-627">Therefore, the statistics returned by this API may not reflect changes caused by recent indexing operations.</span></span>
> 
> 

<span data-ttu-id="1a5b1-628">**Förfrågan**</span><span class="sxs-lookup"><span data-stu-id="1a5b1-628">**Request**</span></span>

<span data-ttu-id="1a5b1-629">HTTPS krävs för alla begäranden som tjänster.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-629">HTTPS is required for all services requests.</span></span> <span data-ttu-id="1a5b1-630">Den **hämta indexstatistik** begäran kan konstrueras med GET-metoden.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-630">The **Get Index Statistics** request can be constructed using the GET method.</span></span>

<span data-ttu-id="1a5b1-631">[Index name] i URI-begäran talar om tjänsten för att returnera indexstatistik för det angivna indexet.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-631">The [index name] in the request URI tells the service to return index statistics for the specified index.</span></span>

<span data-ttu-id="1a5b1-632">`api-version=[string]`(krävs).</span><span class="sxs-lookup"><span data-stu-id="1a5b1-632">`api-version=[string]` (required).</span></span> <span data-ttu-id="1a5b1-633">Förhandsversionen är `api-version=2015-02-28-Preview`.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-633">The preview version is `api-version=2015-02-28-Preview`.</span></span> <span data-ttu-id="1a5b1-634">Se [Sök Versionhantering](http://msdn.microsoft.com/library/azure/dn864560.aspx) information och alternativa versioner.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-634">See [Search Service Versioning](http://msdn.microsoft.com/library/azure/dn864560.aspx) for details and alternative versions.</span></span>

<span data-ttu-id="1a5b1-635">**Huvuden för begäran**</span><span class="sxs-lookup"><span data-stu-id="1a5b1-635">**Request Headers**</span></span>

<span data-ttu-id="1a5b1-636">I följande lista beskrivs de obligatoriska och valfria begärandehuvudena.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-636">The following list describes the required and optional request headers.</span></span>

* <span data-ttu-id="1a5b1-637">`api-key`: `api-key` Används för att autentisera begäran om att din söktjänst.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-637">`api-key`: The `api-key` is used to authenticate the request to your Search service.</span></span> <span data-ttu-id="1a5b1-638">Det är ett strängvärde som unik för din tjänst.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-638">It is a string value, unique to your service.</span></span> <span data-ttu-id="1a5b1-639">Den **hämta indexstatistik** begäran måste innehålla en `api-key` inställd på en administrationsnyckel (i stället för en fråga nyckel).</span><span class="sxs-lookup"><span data-stu-id="1a5b1-639">The **Get Index Statistics** request must include an `api-key` set to an admin key (as opposed to a query key).</span></span>

<span data-ttu-id="1a5b1-640">Du måste också tjänstnamnet att konstruera den begärda Webbadressen.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-640">You will also need the service name to construct the request URL.</span></span> <span data-ttu-id="1a5b1-641">Du kan hämta namnet på tjänsten och `api-key` från tjänsten instrumentpanelen i Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-641">You can get the service name and `api-key` from your service dashboard in the Azure Portal.</span></span> <span data-ttu-id="1a5b1-642">Se [skapa en Azure Search-tjänst i portalen](search-create-service-portal.md) sidan navigering hjälp.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-642">See [Create an Azure Search service in the portal](search-create-service-portal.md) for page navigation help.</span></span>

<span data-ttu-id="1a5b1-643">**Begärandetexten**</span><span class="sxs-lookup"><span data-stu-id="1a5b1-643">**Request Body**</span></span>

<span data-ttu-id="1a5b1-644">Ingen.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-644">None.</span></span>

<span data-ttu-id="1a5b1-645">**Svar**</span><span class="sxs-lookup"><span data-stu-id="1a5b1-645">**Response**</span></span>

<span data-ttu-id="1a5b1-646">Statuskod: 200 OK returneras för ett lyckat svar.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-646">Status Code: 200 OK is returned for a successful response.</span></span>

<span data-ttu-id="1a5b1-647">Svarstexten finns i följande format:</span><span class="sxs-lookup"><span data-stu-id="1a5b1-647">The response body is in the following format:</span></span>

    {
      "documentCount": number,
      "storageSize": number (size of the index in bytes)
    }

<a name="TestAnalyzer"></a>

## <a name="test-analyzer"></a><span data-ttu-id="1a5b1-648">Testa Analyzer</span><span class="sxs-lookup"><span data-stu-id="1a5b1-648">Test Analyzer</span></span>
<span data-ttu-id="1a5b1-649">Den **analysera API** visar hur en analyzer delar upp text i token.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-649">The **Analyze API** shows how an analyzer breaks text into tokens.</span></span>

    POST https://[service name].search.windows.net/indexes/[index name]/analyze?api-version=[api-version]
    Content-Type: application/json
    api-key: [admin key]

<span data-ttu-id="1a5b1-650">**Förfrågan**</span><span class="sxs-lookup"><span data-stu-id="1a5b1-650">**Request**</span></span>

<span data-ttu-id="1a5b1-651">HTTPS krävs för alla begäranden som tjänster.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-651">HTTPS is required for all services requests.</span></span> <span data-ttu-id="1a5b1-652">Den **analysera API** begäran kan konstrueras med POST-metoden.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-652">The **Analyze API** request can be constructed using the POST method.</span></span>

<span data-ttu-id="1a5b1-653">`api-version=[string]`(krävs).</span><span class="sxs-lookup"><span data-stu-id="1a5b1-653">`api-version=[string]` (required).</span></span> <span data-ttu-id="1a5b1-654">Förhandsversionen är `api-version=2015-02-28-Preview`.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-654">The preview version is `api-version=2015-02-28-Preview`.</span></span> <span data-ttu-id="1a5b1-655">Se [Sök Versionhantering](http://msdn.microsoft.com/library/azure/dn864560.aspx) information och alternativa versioner.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-655">See [Search Service Versioning](http://msdn.microsoft.com/library/azure/dn864560.aspx) for details and alternative versions.</span></span>

<span data-ttu-id="1a5b1-656">**Huvuden för begäran**</span><span class="sxs-lookup"><span data-stu-id="1a5b1-656">**Request Headers**</span></span>

<span data-ttu-id="1a5b1-657">I följande lista beskrivs de obligatoriska och valfria begärandehuvudena.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-657">The following list describes the required and optional request headers.</span></span>

* <span data-ttu-id="1a5b1-658">`api-key`: `api-key` Används för att autentisera begäran om att din söktjänst.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-658">`api-key`: The `api-key` is used to authenticate the request to your Search service.</span></span> <span data-ttu-id="1a5b1-659">Det är ett strängvärde som unik för din tjänst.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-659">It is a string value, unique to your service.</span></span> <span data-ttu-id="1a5b1-660">Den **analysera API** begäran måste innehålla en `api-key` inställd på en administrationsnyckel (i stället för en fråga nyckel).</span><span class="sxs-lookup"><span data-stu-id="1a5b1-660">The **Analyze API** request must include an `api-key` set to an admin key (as opposed to a query key).</span></span>

<span data-ttu-id="1a5b1-661">Du måste också Indexnamnet och tjänstnamn att konstruera den begärda Webbadressen.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-661">You will also need the index name and the service name to construct the request URL.</span></span> <span data-ttu-id="1a5b1-662">Du kan hämta namnet på tjänsten och `api-key` från tjänsten instrumentpanelen i Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-662">You can get the service name and `api-key` from your service dashboard in the Azure Portal.</span></span> <span data-ttu-id="1a5b1-663">Se [skapa en Azure Search-tjänst i portalen](search-create-service-portal.md) sidan navigering hjälp.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-663">See [Create an Azure Search service in the portal](search-create-service-portal.md) for page navigation help.</span></span>

<span data-ttu-id="1a5b1-664">**Begärandetexten**</span><span class="sxs-lookup"><span data-stu-id="1a5b1-664">**Request Body**</span></span>

    {
      "text": "Text to analyze",
      "analyzer": "analyzer_name"
    }

<span data-ttu-id="1a5b1-665">eller</span><span class="sxs-lookup"><span data-stu-id="1a5b1-665">or</span></span>

    {
      "text": "Text to analyze",
      "tokenizer": "tokenizer_name",
      "tokenFilters": (optional) [ "token_filter_name" ],
      "charFilters": (optional) [ "char_filter_name" ]
    }

<span data-ttu-id="1a5b1-666">Den `analyzer_name`, `tokenizer_name`, `token_filter_name` och `char_filter_name` måste vara giltiga namn på fördefinierade eller anpassade analyzers, tokenizers, token-filter och char filter för indexet.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-666">The `analyzer_name`, `tokenizer_name`, `token_filter_name` and `char_filter_name` need to be valid names of predefined or custom analyzers, tokenizers, token filters and char filters for the index.</span></span> <span data-ttu-id="1a5b1-667">Mer information om processen för lexikala analys finns [analys i Azure Search](https://aka.ms/azsanalysis).</span><span class="sxs-lookup"><span data-stu-id="1a5b1-667">To learn more about the process of lexical analysis see [Analysis in Azure Search](https://aka.ms/azsanalysis).</span></span>

<span data-ttu-id="1a5b1-668">**Svar**</span><span class="sxs-lookup"><span data-stu-id="1a5b1-668">**Response**</span></span>

<span data-ttu-id="1a5b1-669">Statuskod: 200 OK returneras för ett lyckat svar.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-669">Status Code: 200 OK is returned for a successful response.</span></span>

<span data-ttu-id="1a5b1-670">Svarstexten finns i följande format:</span><span class="sxs-lookup"><span data-stu-id="1a5b1-670">The response body is in the following format:</span></span>

    {
      "tokens": [
        {
          "token": string (token),
          "startOffset": number (index of the first character of the token),
          "endOffset": number (index of the last character of the token),
          "position": number (position of the token in the input text)
        },
        ...
      ]
    }

<span data-ttu-id="1a5b1-671">**Analysera API-exempel**</span><span class="sxs-lookup"><span data-stu-id="1a5b1-671">**Analyze API example**</span></span>

<span data-ttu-id="1a5b1-672">**Förfrågan**</span><span class="sxs-lookup"><span data-stu-id="1a5b1-672">**Request**</span></span>

    {
      "text": "Text to analyze",
      "analyzer": "standard"
    }

<span data-ttu-id="1a5b1-673">**Svar**</span><span class="sxs-lookup"><span data-stu-id="1a5b1-673">**Response**</span></span>

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

## <a name="document-operations"></a><span data-ttu-id="1a5b1-674">Dokumentet åtgärder</span><span class="sxs-lookup"><span data-stu-id="1a5b1-674">Document Operations</span></span>
<span data-ttu-id="1a5b1-675">I Azure Search index lagras i molnet och fylls i med hjälp av JSON-dokument som du överför till tjänsten.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-675">In Azure Search, an index is stored in the cloud and populated using JSON documents that you upload to the service.</span></span> <span data-ttu-id="1a5b1-676">Alla dokument som du överför utgör Kristi Sök data.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-676">All the documents that you upload comprise the corpus of your search data.</span></span> <span data-ttu-id="1a5b1-677">Dokument innehåller fält, av vilka vissa tokeniserad i söktermer som de överförs.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-677">Documents contain fields, some of which are tokenized into search terms as they are uploaded.</span></span> <span data-ttu-id="1a5b1-678">Den `/docs` URL-segment i Azure Search API representerar samling av dokument i ett index.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-678">The `/docs` URL segment in the Azure Search API represents the collection of documents in an index.</span></span> <span data-ttu-id="1a5b1-679">Alla åtgärder som utförs i samlingen som överför sammanslagning, ta bort eller fråga dokument vidta placera i samband med ett index, så URL: er för dessa åtgärder börjar alltid med `/indexes/[index name]/docs` efter ett angivet index.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-679">All operations performed on the collection such as uploading, merging, deleting, or querying documents take place in the context of a single index, so the URLs for these operations will always start with `/indexes/[index name]/docs` for a given index name.</span></span>

<span data-ttu-id="1a5b1-680">Programkoden måste antingen generera JSON-dokument för att överföra till Azure Search eller använda en [indexeraren](https://msdn.microsoft.com/library/dn946891.aspx) att läsa in dokument om datakällan är Azure SQL Database eller Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-680">Your application code must either generate JSON documents to upload to Azure Search or you can use an [indexer](https://msdn.microsoft.com/library/dn946891.aspx) to load documents if the data source is either Azure SQL Database or Azure Cosmos DB.</span></span> <span data-ttu-id="1a5b1-681">Normalt fylls index i från en enda datamängd som du anger.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-681">Typically, indexes are populated from a single dataset that you provide.</span></span>

<span data-ttu-id="1a5b1-682">Du bör planera ska ha ett dokument för varje objekt som du vill söka efter.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-682">You should plan on having one document for each item that you want to search.</span></span> <span data-ttu-id="1a5b1-683">En film uthyrning programmet kan ha ett dokument per film, en storefront programmet kan ha ett dokument per SKU, ett online kursprogramvara program kan ha ett dokument per kursen, research företag kan ha ett dokument för varje academic papper i sin databas och så vidare.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-683">A movie rental application might have one document per movie, a storefront application might have one document per SKU, an online courseware application might have one document per course, a research firm might have one document for each academic paper in their repository, and so on.</span></span>

<span data-ttu-id="1a5b1-684">Dokument består av ett eller flera fält.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-684">Documents consist of one or more fields.</span></span> <span data-ttu-id="1a5b1-685">Fälten kan innehålla text som är tokeniserad med Azure Search in söktermer som inte tokeniserad eller icke-textvärden som kan användas i filter eller bedömningsprofil profiler.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-685">Fields can contain text that is tokenized by Azure Search into search terms, as well as non-tokenized or non-text values that can be used in filters or scoring profiles.</span></span> <span data-ttu-id="1a5b1-686">Namn, datatyper och funktioner som stöds för varje fält bestäms av indexeringsschema.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-686">The names, data types, and search features supported for each field are determined by the index schema.</span></span> <span data-ttu-id="1a5b1-687">Något av fälten i varje indexeringsschema måste anges som ett ID och varje dokument måste ha ett värde för fältet ID som unikt identifierar dokumentet i indexet.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-687">One of the fields in each index schema must be designated as an ID, and each document must have a value for the ID field that uniquely identifies that document in the index.</span></span> <span data-ttu-id="1a5b1-688">Alla andra dokumentfält är valfria och som standard till en null-värde om lämnas Ospecificerad.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-688">All other document fields are optional and will default to a null value if left unspecified.</span></span> <span data-ttu-id="1a5b1-689">Observera att null-värden inte plats i sökindexet.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-689">Note that null values do not take up space in the search index.</span></span>

<span data-ttu-id="1a5b1-690">Innan du kan ladda upp dokument, måste du ha skapat indexet på tjänsten.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-690">Before you can upload documents, you must have already created the index on the service.</span></span> <span data-ttu-id="1a5b1-691">Se [Create Index](#CreateIndex) mer information om det här första steget.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-691">See [Create Index](#CreateIndex) for details about this first step.</span></span>

<a name="AddOrUpdateDocuments"></a>

## <a name="add-update-or-delete-documents"></a><span data-ttu-id="1a5b1-692">Lägga till, uppdatera, eller ta bort dokument</span><span class="sxs-lookup"><span data-stu-id="1a5b1-692">Add, Update, or Delete Documents</span></span>
<span data-ttu-id="1a5b1-693">Du kan ladda upp, merge, merge eller överför eller ta bort dokument från ett specificerat index med hjälp av HTTP POST.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-693">You can upload, merge, merge-or-upload or delete documents from a specified index using HTTP POST.</span></span> <span data-ttu-id="1a5b1-694">För många uppdateringar rekommenderas massbearbetning av dokument (1000 dokument per batch) eller ca 16 MB per batch.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-694">For large numbers of updates, batching of documents (up to 1000 documents per batch or about 16 MB per batch) is recommended.</span></span>

    POST https://[service name].search.windows.net/indexes/[index name]/docs/index?api-version=[api-version]
    Content-Type: application/json
    api-key: [admin key]

<span data-ttu-id="1a5b1-695">**Förfrågan**</span><span class="sxs-lookup"><span data-stu-id="1a5b1-695">**Request**</span></span>

<span data-ttu-id="1a5b1-696">HTTPS krävs för alla tjänstbegäranden.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-696">HTTPS is required for all service requests.</span></span> <span data-ttu-id="1a5b1-697">Du kan ladda upp, merge, merge eller överför eller ta bort dokument från ett specificerat index med hjälp av HTTP POST.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-697">You can upload, merge, merge-or-upload or delete documents from a specified index using HTTP POST.</span></span>

<span data-ttu-id="1a5b1-698">URI-begäran innehåller [Indexnamnet] anger vilken index om du vill skicka dokument.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-698">The request URI includes [index name], specifying which index to post documents.</span></span> <span data-ttu-id="1a5b1-699">Du kan bara skicka dokument till ett index i taget.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-699">You can only post documents to one index at a time.</span></span>

<span data-ttu-id="1a5b1-700">`api-version=[string]`(krävs).</span><span class="sxs-lookup"><span data-stu-id="1a5b1-700">`api-version=[string]` (required).</span></span> <span data-ttu-id="1a5b1-701">Förhandsversionen är `api-version=2015-02-28-Preview`.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-701">The preview version is `api-version=2015-02-28-Preview`.</span></span> <span data-ttu-id="1a5b1-702">Se [Sök Versionhantering](http://msdn.microsoft.com/library/azure/dn864560.aspx) information och alternativa versioner.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-702">See [Search Service Versioning](http://msdn.microsoft.com/library/azure/dn864560.aspx) for details and alternative versions.</span></span>

<span data-ttu-id="1a5b1-703">**Huvuden för begäran**</span><span class="sxs-lookup"><span data-stu-id="1a5b1-703">**Request Headers**</span></span>

<span data-ttu-id="1a5b1-704">I följande lista beskrivs de obligatoriska och valfria begärandehuvudena.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-704">The following list describes the required and optional request headers.</span></span>

* <span data-ttu-id="1a5b1-705">`Content-Type`: Krävs.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-705">`Content-Type`: Required.</span></span> <span data-ttu-id="1a5b1-706">Ställ in på`application/json`</span><span class="sxs-lookup"><span data-stu-id="1a5b1-706">Set this to `application/json`</span></span>
* <span data-ttu-id="1a5b1-707">`api-key`: Krävs.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-707">`api-key`: Required.</span></span> <span data-ttu-id="1a5b1-708">Den `api-key` används för att autentisera begäran om att din söktjänst.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-708">The `api-key` is used to authenticate the request to your Search service.</span></span> <span data-ttu-id="1a5b1-709">Det är ett strängvärde som unik för din tjänst.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-709">It is a string value, unique to your service.</span></span> <span data-ttu-id="1a5b1-710">Den **lägga till dokument** begäran måste innehålla en `api-key` huvudet inställt till din administrationsnyckel (i stället för en fråga nyckel).</span><span class="sxs-lookup"><span data-stu-id="1a5b1-710">The **Add Documents** request must include an `api-key` header set to your admin key (as opposed to a query key).</span></span>

<span data-ttu-id="1a5b1-711">Du måste också tjänstnamnet att konstruera den begärda Webbadressen.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-711">You will also need the service name to construct the request URL.</span></span> <span data-ttu-id="1a5b1-712">Du kan hämta namnet på tjänsten och `api-key` från tjänsten instrumentpanelen i Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-712">You can get the service name and `api-key` from your service dashboard in the Azure Portal.</span></span> <span data-ttu-id="1a5b1-713">Se [skapa en Azure Search-tjänst i portalen](search-create-service-portal.md) sidan navigering hjälp.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-713">See [Create an Azure Search service in the portal](search-create-service-portal.md) for page navigation help.</span></span>

<span data-ttu-id="1a5b1-714">**Begärandetexten**</span><span class="sxs-lookup"><span data-stu-id="1a5b1-714">**Request Body**</span></span>

<span data-ttu-id="1a5b1-715">Brödtexten i begäran innehåller ett eller flera dokument indexeras.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-715">The body of the request contains one or more documents to be indexed.</span></span> <span data-ttu-id="1a5b1-716">Dokument identifieras med en unik nyckel.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-716">Documents are identified by a unique key.</span></span> <span data-ttu-id="1a5b1-717">Varje dokument som är associerat med en åtgärd: Överför dokument, mergeOrUpload eller ta bort.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-717">Each document is associated with an action: upload, merge, mergeOrUpload or delete.</span></span> <span data-ttu-id="1a5b1-718">Överför förfrågningar måste innehålla dokumentdata som en uppsättning nyckel/värde-par.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-718">Upload requests must include the document data as a set of key/value pairs.</span></span>

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
> <span data-ttu-id="1a5b1-719">Dokumentet nycklar kan endast innehålla bokstäver, siffror, bindestreck (”-”), understreck (”_”) och likhetstecken (”=”).</span><span class="sxs-lookup"><span data-stu-id="1a5b1-719">Document keys can only contain letters, numbers, dashes ("-"), underscores ("_"), and equal signs ("=").</span></span> <span data-ttu-id="1a5b1-720">Mer information finns i [namngivningsregler](https://msdn.microsoft.com/library/azure/dn857353.aspx).</span><span class="sxs-lookup"><span data-stu-id="1a5b1-720">For more details, see [Naming rules](https://msdn.microsoft.com/library/azure/dn857353.aspx).</span></span>
> 
> 

<span data-ttu-id="1a5b1-721">**Dokumentåtgärder**</span><span class="sxs-lookup"><span data-stu-id="1a5b1-721">**Document Actions**</span></span>

* <span data-ttu-id="1a5b1-722">`upload`: En åtgärd som överför liknar ”upsert” där dokumentet kommer att infogas om det är nya och uppdaterade/ersättas om den finns.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-722">`upload`: An upload action is similar to an "upsert" where the document will be inserted if it is new and updated/replaced if it exists.</span></span> <span data-ttu-id="1a5b1-723">Observera att alla fält ersätts i fallet med uppdateringen.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-723">Note that all fields are replaced in the update case.</span></span>
* <span data-ttu-id="1a5b1-724">`merge`: Sammanslagning uppdaterar ett befintligt dokument med de angivna fälten.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-724">`merge`: Merge updates an existing document with the specified fields.</span></span> <span data-ttu-id="1a5b1-725">Om dokumentet inte finns misslyckas kopplingen.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-725">If the document doesn't exist, the merge will fail.</span></span> <span data-ttu-id="1a5b1-726">Alla fält som du anger i en sammanfogning ersätter det befintliga fältet i dokumentet.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-726">Any field you specify in a merge will replace the existing field in the document.</span></span> <span data-ttu-id="1a5b1-727">Detta gäller även fält av typen `Collection(Edm.String)`.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-727">This includes fields of type `Collection(Edm.String)`.</span></span> <span data-ttu-id="1a5b1-728">Om dokumentet innehåller fältet ”taggar” med värdet till exempel `["budget"]` och du utföra en koppling med värdet `["economy", "pool"]` ”taggar” det slutliga värdet för fältet ”taggar” blir `["economy", "pool"]`.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-728">For example, if the document contains a field "tags" with value `["budget"]` and you execute a merge with value `["economy", "pool"]` for "tags", the final value of the "tags" field will be `["economy", "pool"]`.</span></span> <span data-ttu-id="1a5b1-729">Kommer det att **inte** vara `["budget", "economy", "pool"]`.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-729">It will **not** be `["budget", "economy", "pool"]`.</span></span>
* <span data-ttu-id="1a5b1-730">`mergeOrUpload`: fungerar som `merge` om ett dokument med den angivna nyckeln finns redan i indexet.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-730">`mergeOrUpload`: behaves like `merge` if a document with the given key already exists in the index.</span></span> <span data-ttu-id="1a5b1-731">Om dokumentet inte finns fungerar som `upload` med ett nytt dokument.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-731">If the document does not exist it behaves like `upload` with a new document.</span></span>
* <span data-ttu-id="1a5b1-732">`delete`: Ta bort tar bort det angivna dokumentet från indexet.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-732">`delete`: Delete removes the specified document from the index.</span></span> <span data-ttu-id="1a5b1-733">Observera att alla fält som du anger i en `delete` åtgärden än nyckelfältet kommer att ignoreras.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-733">Note that any fields you specify in a `delete` operation other than the key field will be ignored.</span></span> <span data-ttu-id="1a5b1-734">Om du vill ta bort ett fält från ett dokument, använda `merge` enkelt och i stället ange fältet explicit till `null`.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-734">If you want to remove an individual field from a document, use `merge` instead and simply set the field explicitly to `null`.</span></span>

<span data-ttu-id="1a5b1-735">**Svar**</span><span class="sxs-lookup"><span data-stu-id="1a5b1-735">**Response**</span></span>

<span data-ttu-id="1a5b1-736">Statuskoden 200 returneras (OK) för ett lyckat svar, vilket innebär att alla objekt indexerade.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-736">Status code 200 (OK) is returned for a successful response, meaning that all items have been successfully indexed.</span></span> <span data-ttu-id="1a5b1-737">Detta anges av den `status` egenskapen anges till true för alla artiklar, såväl som den `statusCode` egenskapsuppsättningen till 201 (för nyligen uppladdade dokument) eller 200 (för sammanfogade eller borttagna dokument):</span><span class="sxs-lookup"><span data-stu-id="1a5b1-737">This is indicated by the `status` property being set to true for all items, as well as the `statusCode` property being set to either 201 (for newly uploaded documents) or 200 (for merged or deleted documents):</span></span>

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

<span data-ttu-id="1a5b1-738">Statuskoden 207 (flera Status) returnerades när minst ett objekt inte har har indexerats.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-738">Status code 207 (Multi-Status) is returned when at least one item was not successfully indexed.</span></span> <span data-ttu-id="1a5b1-739">Objekt som inte har indexerats har den `status` fältet inställt på false.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-739">Items that have not been indexed have the `status` field set to false.</span></span> <span data-ttu-id="1a5b1-740">Den `errorMessage` och `statusCode` egenskaper kommer att ange orsaken till felet indexering:</span><span class="sxs-lookup"><span data-stu-id="1a5b1-740">The `errorMessage` and `statusCode` properties will indicate the reason for the indexing error:</span></span>

    {
      "value": [
        {
          "key": "unique_key_of_document_1",
          "status": false,
          "errorMessage": "The search service is too busy to process this document. Please try again later.",
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
          "errorMessage": "Index is temporarily unavailable because it was updated with the 'allowIndexDowntime' flag set to 'true'. Please try again later.",
          "statusCode": 422
        }
      ]
    }  

<span data-ttu-id="1a5b1-741">I följande tabell beskrivs olika per dokument statuskoder som kan returneras i svaret.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-741">The following table explains the various per-document status codes that can be returned in the response.</span></span> <span data-ttu-id="1a5b1-742">Observera att vissa anger problem med begäran, medan andra indikerar tillfälliga fel.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-742">Note that some indicate problems with the request itself, while others indicate temporary error conditions.</span></span> <span data-ttu-id="1a5b1-743">Denna du kan försöka efter en stund.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-743">The latter you should retry after a delay.</span></span>

<table style="font-size:12">
    <tr>
        <th><span data-ttu-id="1a5b1-744">Statuskod</span><span class="sxs-lookup"><span data-stu-id="1a5b1-744">Status code</span></span></th>
        <th><span data-ttu-id="1a5b1-745">Betydelse</span><span class="sxs-lookup"><span data-stu-id="1a5b1-745">Meaning</span></span></th>
        <th><span data-ttu-id="1a5b1-746">Återförsökbart</span><span class="sxs-lookup"><span data-stu-id="1a5b1-746">Retryable</span></span></th>
        <th><span data-ttu-id="1a5b1-747">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="1a5b1-747">Notes</span></span></th>
    </tr>
    <tr>
        <td><span data-ttu-id="1a5b1-748">200</span><span class="sxs-lookup"><span data-stu-id="1a5b1-748">200</span></span></td>
        <td><span data-ttu-id="1a5b1-749">Dokumentet har har ändrats eller tagits bort.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-749">Document was successfully modified or deleted.</span></span></td>
        <td><span data-ttu-id="1a5b1-750">Saknas</span><span class="sxs-lookup"><span data-stu-id="1a5b1-750">n/a</span></span></td>
        <td><span data-ttu-id="1a5b1-751">Ta bort är <a href="https://en.wikipedia.org/wiki/Idempotence">idempotent</a>.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-751">Delete operations are <a href="https://en.wikipedia.org/wiki/Idempotence">idempotent</a>.</span></span> <span data-ttu-id="1a5b1-752">Det vill säga även om en Dokumentnyckel inte finns i index, leder försöker en borttagningsåtgärd med nyckeln statuskod 200.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-752">That is, even if a document key does not exist in the index, attempting a delete operation with that key will result in a 200 status code.</span></span></td>
    </tr>
    <tr>
        <td><span data-ttu-id="1a5b1-753">201</span><span class="sxs-lookup"><span data-stu-id="1a5b1-753">201</span></span></td>
        <td><span data-ttu-id="1a5b1-754">Dokumentet har skapats.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-754">Document was successfully created.</span></span></td>
        <td><span data-ttu-id="1a5b1-755">Saknas</span><span class="sxs-lookup"><span data-stu-id="1a5b1-755">n/a</span></span></td>
        <td></td>
    </tr>
    <tr>
        <td><span data-ttu-id="1a5b1-756">400</span><span class="sxs-lookup"><span data-stu-id="1a5b1-756">400</span></span></td>
        <td><span data-ttu-id="1a5b1-757">Ett fel uppstod i dokumentet som hindrade den från att indexeras.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-757">There was an error in the document that prevented it from being indexed.</span></span></td>
        <td><span data-ttu-id="1a5b1-758">Nej</span><span class="sxs-lookup"><span data-stu-id="1a5b1-758">No</span></span></td>
        <td><span data-ttu-id="1a5b1-759">Felmeddelande i svaret visar fel med dokumentet.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-759">The error message in the response will indicate what is wrong with the document.</span></span></td>
    </tr>
    <tr>
        <td><span data-ttu-id="1a5b1-760">404</span><span class="sxs-lookup"><span data-stu-id="1a5b1-760">404</span></span></td>
        <td><span data-ttu-id="1a5b1-761">Dokumentet kan inte sammanfogas eftersom den angivna nyckeln inte finns i indexet.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-761">The document could not be merged because the given key doesn't exist in the index.</span></span></td>
        <td><span data-ttu-id="1a5b1-762">Nej</span><span class="sxs-lookup"><span data-stu-id="1a5b1-762">No</span></span></td>
        <td><span data-ttu-id="1a5b1-763">Det här felet uppstår inte för överföringar, eftersom de skapar nya dokument och det uppstår inte för borttagningar eftersom de är <a href="https://en.wikipedia.org/wiki/Idempotence">idempotent</a>.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-763">This error does not occur for uploads since they create new documents, and it does not occur for deletes because they are <a href="https://en.wikipedia.org/wiki/Idempotence">idempotent</a>.</span></span></td>
    </tr>
    <tr>
        <td><span data-ttu-id="1a5b1-764">409</span><span class="sxs-lookup"><span data-stu-id="1a5b1-764">409</span></span></td>
        <td><span data-ttu-id="1a5b1-765">En versionskonflikt upptäcktes vid försök att indexera ett dokument.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-765">A version conflict was detected when attempting to index a document.</span></span></td>
        <td><span data-ttu-id="1a5b1-766">Ja</span><span class="sxs-lookup"><span data-stu-id="1a5b1-766">Yes</span></span></td>
        <td><span data-ttu-id="1a5b1-767">Detta kan inträffa när du försöker att mer än en gång samtidigt indexera samma dokument.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-767">This can happen when you're trying to index the same document more than once concurrently.</span></span></td>
    </tr>
    <tr>
        <td><span data-ttu-id="1a5b1-768">422</span><span class="sxs-lookup"><span data-stu-id="1a5b1-768">422</span></span></td>
        <td><span data-ttu-id="1a5b1-769">Indexet är inte tillgänglig för tillfället eftersom den uppdaterades med 'allowIndexDowntime'-flaggan inställd på 'true'.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-769">The index is temporarily unavailable because it was updated with the 'allowIndexDowntime' flag set to 'true'.</span></span></td>
        <td><span data-ttu-id="1a5b1-770">Ja</span><span class="sxs-lookup"><span data-stu-id="1a5b1-770">Yes</span></span></td>
        <td></td>
    </tr>
    <tr>
        <td><span data-ttu-id="1a5b1-771">503</span><span class="sxs-lookup"><span data-stu-id="1a5b1-771">503</span></span></td>
        <td><span data-ttu-id="1a5b1-772">Din söktjänst är tillfälligt otillgänglig, möjligen på grund av hög belastning.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-772">Your search service is temporarily unavailable, possibly due to heavy load.</span></span></td>
        <td><span data-ttu-id="1a5b1-773">Ja</span><span class="sxs-lookup"><span data-stu-id="1a5b1-773">Yes</span></span></td>
        <td><span data-ttu-id="1a5b1-774">Koden ska vänta innan du försöker igen i det här fallet eller riskerar du att förlänga att tjänsten.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-774">Your code should wait before retrying in this case or you risk prolonging the service unavailability.</span></span></td>
    </tr>
</table> 

<span data-ttu-id="1a5b1-775">**Obs**: om din klientkod ofta påträffar ett 207 svar, en möjlig orsak är att systemet är under belastning.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-775">**Note**: If your client code frequently encounters a 207 response, one possible reason is that the system is under load.</span></span> <span data-ttu-id="1a5b1-776">Du kan kontrollera detta genom att kontrollera den `statusCode` -egenskapen för 503.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-776">You can confirm this by checking the `statusCode` property for 503.</span></span> <span data-ttu-id="1a5b1-777">Om så är fallet, rekommenderar vi ***begränsning indexering begäranden***.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-777">If this is the case, we recommend ***throttling indexing requests***.</span></span> <span data-ttu-id="1a5b1-778">Annars, om indexering trafik inte subside systemet kunde starta avvisar alla begäranden med 503-fel.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-778">Otherwise, if indexing traffic doesn't subside, the system could start rejecting all requests with 503 errors.</span></span>

<span data-ttu-id="1a5b1-779">Statuskoden 429 anger att du har överskridit kvoten för antalet dokument per index.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-779">Status code 429 indicates that you have exceeded your quota on the number of documents per index.</span></span> <span data-ttu-id="1a5b1-780">Du måste skapa ett nytt index eller uppgradera för högre kapacitetsgränser.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-780">You must either create a new index or upgrade for higher capacity limits.</span></span>

<span data-ttu-id="1a5b1-781">**Exempel:**</span><span class="sxs-lookup"><span data-stu-id="1a5b1-781">**Example:**</span></span>

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

## <a name="search-documents"></a><span data-ttu-id="1a5b1-782">Sök igenom dokument</span><span class="sxs-lookup"><span data-stu-id="1a5b1-782">Search Documents</span></span>
<span data-ttu-id="1a5b1-783">En **Sök** åtgärden utfärdas som en GET eller POST-begäran och anger parametrarna som ger kriterierna för att välja matchande dokument.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-783">A **Search** operation is issued as a GET or POST request and specifies parameters that give the criteria for selecting matching documents.</span></span>

    GET https://[service name].search.windows.net/indexes/[index name]/docs?[query parameters]
    api-key: [admin or query key]

    POST https://[service name].search.windows.net/indexes/[index name]/docs/search?api-version=[api-version]
    Content-Type: application/json
    api-key: [admin or query key]

<span data-ttu-id="1a5b1-784">**När du ska använda en POST i stället för att hämta**</span><span class="sxs-lookup"><span data-stu-id="1a5b1-784">**When to use POST instead of GET**</span></span>

<span data-ttu-id="1a5b1-785">När du använder HTTP GET för att anropa den **Sök** API, du måste vara medveten om att längden på den begärda Webbadressen inte får överskrida 8 KB.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-785">When you use HTTP GET to call the **Search** API, you need to be aware that the length of the request URL cannot exceed 8 KB.</span></span> <span data-ttu-id="1a5b1-786">Detta är vanligtvis tillräckligt för de flesta program.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-786">This is usually enough for most applications.</span></span> <span data-ttu-id="1a5b1-787">Vissa program kan dock ge mycket stora frågor eller OData filteruttryck.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-787">However, some applications produce very large queries or OData filter expressions.</span></span> <span data-ttu-id="1a5b1-788">För dessa program är med hjälp av HTTP POST ett bättre alternativ eftersom det tillåter större filter och frågor än GET.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-788">For these applications, using HTTP POST is a better choice because it allows larger filters and queries than GET.</span></span> <span data-ttu-id="1a5b1-789">Med POST är antalet villkor eller satser i en fråga begränsande faktorn inte storleken på raw frågan eftersom begäran storleksgränsen för POST är ungefär 16 MB.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-789">With POST, the number of terms or clauses in a query is the limiting factor, not the size of the raw query since the request size limit for POST is approximately 16 MB.</span></span>

> [!NOTE]
> <span data-ttu-id="1a5b1-790">Även om storleksgränsen för POST-begäran är mycket stora får inte sökningar och filteruttryck vara godtyckligt komplexa.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-790">Even though the POST request size limit is very large, search queries and filter expressions cannot be arbitrarily complex.</span></span> <span data-ttu-id="1a5b1-791">Se [Lucene frågesyntaxen](https://msdn.microsoft.com/library/mt589323.aspx) och [OData-uttryckssyntax](https://msdn.microsoft.com/library/dn798921.aspx) för mer information om begränsningar för sökning fråge- och komplexitet.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-791">See [Lucene query syntax](https://msdn.microsoft.com/library/mt589323.aspx) and [OData expression syntax](https://msdn.microsoft.com/library/dn798921.aspx) for more information about search query and filter complexity limitations.</span></span>
> 
> 

<span data-ttu-id="1a5b1-792">**Förfrågan**</span><span class="sxs-lookup"><span data-stu-id="1a5b1-792">**Request**</span></span>

<span data-ttu-id="1a5b1-793">HTTPS krävs för tjänstbegäranden.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-793">HTTPS is required for service requests.</span></span> <span data-ttu-id="1a5b1-794">Den **Sök** begäran kan konstrueras med metoderna GET eller POST.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-794">The **Search** request can be constructed using the GET or POST methods.</span></span>

<span data-ttu-id="1a5b1-795">URI-begäran anger vilka index fråga efter alla dokument som matchar parametrarna.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-795">The request URI specifies which index to query, for all documents that match the parameters.</span></span> <span data-ttu-id="1a5b1-796">Parametrar har angetts i frågesträngen när det gäller GET-begäranden och i begäran brödtext för POST-begäranden.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-796">Parameters are specified on the query string in the case of GET requests, and in the request body in the case of POST requests.</span></span>

<span data-ttu-id="1a5b1-797">Ett bra tips när du skapar GET-begäranden, Kom ihåg att [URL-koda](https://msdn.microsoft.com/library/system.uri.escapedatastring.aspx) särskilda frågeparametrar vid anrop av REST API direkt.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-797">As a best practice when creating GET requests, remember to [URL-encode](https://msdn.microsoft.com/library/system.uri.escapedatastring.aspx) specific query parameters when calling the REST API directly.</span></span> <span data-ttu-id="1a5b1-798">För **Sök** åtgärder, detta omfattar:</span><span class="sxs-lookup"><span data-stu-id="1a5b1-798">For **Search** operations, this includes:</span></span>

* `$filter`
* `facet`
* `highlightPreTag`
* `highlightPostTag`
* `search`
* `moreLikeThis`

<span data-ttu-id="1a5b1-799">URL-kodning rekommenderas endast på ovanstående Frågeparametrar.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-799">URL encoding is only recommended on the above query parameters.</span></span> <span data-ttu-id="1a5b1-800">Om du av misstag URL-koda hela frågesträngen (allt efter den?), begäranden bryts.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-800">If you inadvertently URL-encode the entire query string (everything after the ?), requests will break.</span></span>

<span data-ttu-id="1a5b1-801">Dessutom krävs URL-kodning bara vid anrop av REST API: et direkt med GET.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-801">Also, URL encoding is only necessary when calling the REST API directly using GET.</span></span> <span data-ttu-id="1a5b1-802">Ingen URL-kodning krävs när du anropar **Sök** med POST, eller när du använder den [.NET-klientbibliotek](https://msdn.microsoft.com/library/dn951165.aspx), som hanterar URL-kodning för dig.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-802">No URL encoding is necessary when calling **Search** using POST, or when using the [.NET client library](https://msdn.microsoft.com/library/dn951165.aspx), which handles URL encoding for you.</span></span>

<span data-ttu-id="1a5b1-803"><a name="SearchQueryParameters"></a>
**Frågeparametrar**</span><span class="sxs-lookup"><span data-stu-id="1a5b1-803"><a name="SearchQueryParameters"></a>
**Query Parameters**</span></span>

<span data-ttu-id="1a5b1-804">**Sök** accepterar flera parametrar som tillhandahåller frågevillkor och även ange sökbeteendet.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-804">**Search** accepts several parameters that provide query criteria and also specify search behavior.</span></span> <span data-ttu-id="1a5b1-805">Du anger dessa parametrar i URL: en frågesträng vid anrop av **Sök** via GET och som JSON-egenskaper i frågans brödtext vid anrop av **Sök** via POST.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-805">You provide these parameters in the URL query string when calling **Search** via GET, and as JSON properties in the request body when calling **Search** via POST.</span></span> <span data-ttu-id="1a5b1-806">Syntaxen för vissa parametrar skiljer mellan GET och POST.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-806">The syntax for some parameters is slightly different between GET and POST.</span></span> <span data-ttu-id="1a5b1-807">Skillnaderna beskrivs i tillämpliga fall nedan:</span><span class="sxs-lookup"><span data-stu-id="1a5b1-807">These differences are noted as applicable below:</span></span>

<span data-ttu-id="1a5b1-808">`search=[string]`(valfritt) - texten att söka efter.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-808">`search=[string]` (optional) - The text to search for.</span></span> <span data-ttu-id="1a5b1-809">Alla `searchable` fält söks som standard om inte `searchFields` har angetts.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-809">All `searchable` fields are searched by default unless `searchFields` is specified.</span></span> <span data-ttu-id="1a5b1-810">När du söker `searchable` fält, söktexten själva tokeniserad så att flera villkor kan avgränsas med blanksteg (till exempel: `search=hello world`).</span><span class="sxs-lookup"><span data-stu-id="1a5b1-810">When searching `searchable` fields, the search text itself is tokenized, so multiple terms can be separated by white space (for example: `search=hello world`).</span></span> <span data-ttu-id="1a5b1-811">Om du vill matcha alla termen använder `*` (det kan vara användbart för booleskt filterfrågor).</span><span class="sxs-lookup"><span data-stu-id="1a5b1-811">To match any term, use `*` (this can be useful for boolean filter queries).</span></span> <span data-ttu-id="1a5b1-812">Om du utesluter den här parametern har samma effekt som du anger `*`.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-812">Omitting this parameter has the same effect as setting it to `*`.</span></span> <span data-ttu-id="1a5b1-813">Se [enkel frågesyntaxen](https://msdn.microsoft.com/library/dn798920.aspx) för närmare information om söksyntax.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-813">See [Simple Query Syntax](https://msdn.microsoft.com/library/dn798920.aspx) for specifics on the search syntax.</span></span>

* <span data-ttu-id="1a5b1-814">**Obs**: resultatet kan ibland vara konstigt när frågor skickas över `searchable` fält.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-814">**Note**: The results can sometimes be surprising when querying over `searchable` fields.</span></span> <span data-ttu-id="1a5b1-815">Tokenizer innehåller logik för att hantera fall som är gemensamma för engelsk text som apostrofer, kommatecken i siffror osv. Till exempel `search=123,456` matchar en enskild term 123,456 i stället för enskilda villkoren 123 och 456, eftersom kommatecken används som avgränsare tusen för stora tal på engelska.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-815">The tokenizer includes logic to handle cases common to English text like apostrophes, commas in numbers, etc. For example, `search=123,456` will match a single term 123,456 rather than the individual terms 123 and 456, since commas are used as thousand-separators for large numbers in English.</span></span> <span data-ttu-id="1a5b1-816">Därför bör du använda blanksteg i stället för skiljetecken avgränsa villkoren i den `search` parameter.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-816">For this reason, we recommend using white space rather than punctuation to separate terms in the `search` parameter.</span></span>

<span data-ttu-id="1a5b1-817">`searchMode=any|all`(valfritt, som standard `any`) – oavsett om någon eller samtliga av sökord måste matchas för att kunna räkna dokumentet som en matchning.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-817">`searchMode=any|all` (optional, defaults to `any`) - whether any or all of the search terms must be matched in order to count the document as a match.</span></span>

<span data-ttu-id="1a5b1-818">`searchFields=[string]`(valfritt) – lista över kommaavgränsade fältnamn att söka efter den angivna texten.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-818">`searchFields=[string]` (optional) - The list of comma-separated field names to search for the specified text.</span></span> <span data-ttu-id="1a5b1-819">Målfält måste vara markerad som `searchable`.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-819">Target fields must be marked as `searchable`.</span></span>

<span data-ttu-id="1a5b1-820">`queryType=simple|full`(valfritt, som standard `simple`) – när inställd på ”enkel” söktext tolkas med hjälp av en enkel frågespråk som möjliggör symboler som, + * och ””.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-820">`queryType=simple|full` (optional, defaults to `simple`) - when set to "simple" search text is interpreted using a simple query language that allows for symbols such as +, * and "".</span></span> <span data-ttu-id="1a5b1-821">Frågor utvärderas över alla sökbara fält (fält som anges i `searchFields`) i varje dokument som standard.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-821">Queries are evaluated across all searchable fields (or fields indicated in `searchFields`) in each document by default.</span></span> <span data-ttu-id="1a5b1-822">När frågetypen har angetts till `full` söktext tolkas med Lucene frågespråket som tillåter specifika fält och viktat sökningar.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-822">When the query type is set to `full` search text is interpreted using the Lucene query language which allows field-specific and weighted searches.</span></span> <span data-ttu-id="1a5b1-823">Se [enkel frågesyntaxen](https://msdn.microsoft.com/library/dn798920.aspx) och [Lucene frågesyntaxen](https://msdn.microsoft.com/library/mt589323.aspx) för närmare information om Sök-syntax.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-823">See [Simple Query Syntax](https://msdn.microsoft.com/library/dn798920.aspx) and [Lucene Query Syntax](https://msdn.microsoft.com/library/mt589323.aspx) for specifics on the search syntaxes.</span></span> 

> [!NOTE]
> <span data-ttu-id="1a5b1-824">Intervallet sökning i Lucene frågespråket inte stöds för $filter som ger liknande funktionalitet.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-824">Range search in the Lucene query language is not supported in favor of $filter which offers similar functionality.</span></span>
> 
> 

<span data-ttu-id="1a5b1-825">`moreLikeThis=[key]`(valfritt) **Viktigt:** den här funktionen är endast tillgänglig i `2015-02-28-Preview`.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-825">`moreLikeThis=[key]` (optional) **Important:** This feature is only available in `2015-02-28-Preview`.</span></span> <span data-ttu-id="1a5b1-826">Det här alternativet kan inte användas i en fråga som innehåller parametern text search `search=[string]`.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-826">This option cannot be used in a query that contains the text search parameter, `search=[string]`.</span></span> <span data-ttu-id="1a5b1-827">Den `moreLikeThis` parametern hittas dokument som liknar det dokument som anges av dokumentnyckeln.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-827">The `moreLikeThis` parameter finds documents that are similar to the document specified by the document key.</span></span> <span data-ttu-id="1a5b1-828">När en sökning har begärts med `moreLikeThis`, en lista över söktermer genereras baserat på frekvens och sällsynt termer i källdokument.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-828">When a search request is made with `moreLikeThis`, a list of search terms is generated based on the frequency and rarity of terms in the source document.</span></span> <span data-ttu-id="1a5b1-829">Dessa villkor används sedan för att begära.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-829">Those terms are then used to make the request.</span></span> <span data-ttu-id="1a5b1-830">Som standard innehållet i alla `searchable` fält anses om `searchFields` används för att begränsa vilka fält som genomsöks.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-830">By default, the contents of all `searchable` fields are considered unless `searchFields` is used to restrict which fields are searched.</span></span>  

<span data-ttu-id="1a5b1-831">`$skip=#`(valfritt) – Antal sökresultat att hoppa över; Får inte vara större än 100 000.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-831">`$skip=#` (optional) - the number of search results to skip; Cannot be greater than 100,000.</span></span> <span data-ttu-id="1a5b1-832">Om du vill skanna dokument i följd men kan inte använda `$skip` på grund av den här begränsningen kan du använda `$orderby` på en helt sorterade nyckel och `$filter` med en fråga i stället.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-832">If you need to scan documents in sequence but cannot use `$skip` due to this limitation, consider using `$orderby` on a totally-ordered key and `$filter` with a range query instead.</span></span>

> [!NOTE]
> <span data-ttu-id="1a5b1-833">När du anropar **Sök** med efter den här parametern heter `skip` i stället för `$skip`.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-833">When calling **Search** using POST, this parameter is named `skip` instead of `$skip`.</span></span>
> 
> 

<span data-ttu-id="1a5b1-834">`$top=#`(valfritt) – Antal sökresultat att hämta.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-834">`$top=#` (optional) - the number of search results to retrieve.</span></span> <span data-ttu-id="1a5b1-835">Detta kan användas tillsammans med `$skip` att implementera sidindelning på klientsidan i sökresultaten.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-835">This can be used in conjunction with `$skip` to implement client-side paging of search results.</span></span>

> [!NOTE]
> <span data-ttu-id="1a5b1-836">När du anropar **Sök** med efter den här parametern heter `top` i stället för `$top`.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-836">When calling **Search** using POST, this parameter is named `top` instead of `$top`.</span></span>
> 
> 

<span data-ttu-id="1a5b1-837">`$count=true|false`(valfritt, som standard `false`) – anger om du vill hämta Totalt antal resultat.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-837">`$count=true|false` (optional, defaults to `false`) - Specifies whether to fetch the total count of results.</span></span> <span data-ttu-id="1a5b1-838">Detta är antalet alla dokument som matchar den `search` och `$filter` parametrar, ignorerar `$top` och `$skip`.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-838">This is the count of all documents that match the `search` and `$filter` parameters, ignoring `$top` and `$skip`.</span></span> <span data-ttu-id="1a5b1-839">Om det här värdet anges till `true` kan påverka prestanda.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-839">Setting this value to `true` may have a performance impact.</span></span> <span data-ttu-id="1a5b1-840">Observera att antalet returnerade en uppskattning.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-840">Note that the count returned is an approximation.</span></span>

> [!NOTE]
> <span data-ttu-id="1a5b1-841">När du anropar **Sök** med efter den här parametern heter `count` i stället för `$count`.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-841">When calling **Search** using POST, this parameter is named `count` instead of `$count`.</span></span>
> 
> 

<span data-ttu-id="1a5b1-842">`$orderby=[string]`(valfritt) – en lista över CSV-uttryck för att sortera resultaten efter.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-842">`$orderby=[string]` (optional) - A list of comma-separated expressions to sort the results by.</span></span> <span data-ttu-id="1a5b1-843">Varje uttryck kan vara ett fältnamn eller ett anrop till den `geo.distance()` funktion.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-843">Each expression can be either a field name or a call to the `geo.distance()` function.</span></span> <span data-ttu-id="1a5b1-844">Varje uttryck kan följas av `asc` anges stigande ordning och `desc` att ange fallande.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-844">Each expression can be followed by `asc` to indicated ascending, and `desc` to indicate descending.</span></span> <span data-ttu-id="1a5b1-845">Standardvärdet är stigande ordning.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-845">The default is ascending order.</span></span> <span data-ttu-id="1a5b1-846">TIES delas av matchar resultat av dokument.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-846">Ties will be broken by the match scores of documents.</span></span> <span data-ttu-id="1a5b1-847">Om ingen `$orderby` anges standardsorteringsordningen fallande dokumentet matchar poäng.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-847">If no `$orderby` is specified, the default sort order is descending by document match score.</span></span> <span data-ttu-id="1a5b1-848">Det finns en gräns på 32-satser för `$orderby`.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-848">There is a limit of 32 clauses for `$orderby`.</span></span>

> [!NOTE]
> <span data-ttu-id="1a5b1-849">När du anropar **Sök** med efter den här parametern heter `orderby` i stället för `$orderby`.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-849">When calling **Search** using POST, this parameter is named `orderby` instead of `$orderby`.</span></span>
> 
> 

<span data-ttu-id="1a5b1-850">`$select=[string]`(valfritt) – en lista över kommaavgränsade fält för att hämta.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-850">`$select=[string]` (optional) - A list of comma-separated fields to retrieve.</span></span> <span data-ttu-id="1a5b1-851">Om inget anges ingår alla fält som har markerats som hämtas i schemat.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-851">If unspecified, all fields marked as retrievable in the schema are included.</span></span> <span data-ttu-id="1a5b1-852">Du kan också explicit begära alla fält genom att ange den här parametern `*`.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-852">You can also explicitly request all fields by setting this parameter to `*`.</span></span>

> [!NOTE]
> <span data-ttu-id="1a5b1-853">När du anropar **Sök** med efter den här parametern heter `select` i stället för `$select`.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-853">When calling **Search** using POST, this parameter is named `select` instead of `$select`.</span></span>
> 
> 

<span data-ttu-id="1a5b1-854">`facet=[string]`(noll eller fler) - aspekten av ett fält.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-854">`facet=[string]` (zero or more) - A field to facet by.</span></span> <span data-ttu-id="1a5b1-855">Strängen kan alternativt innehåller parametrar för att anpassa faceting uttryckt i CSV- `name:value` par.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-855">Optionally the string may contain parameters to customize the faceting expressed as comma-separated `name:value` pairs.</span></span> <span data-ttu-id="1a5b1-856">Giltiga parametrar finns:</span><span class="sxs-lookup"><span data-stu-id="1a5b1-856">Valid parameters are:</span></span>

* <span data-ttu-id="1a5b1-857">`count`(max antal aspekten villkor; standardvärdet är 10).</span><span class="sxs-lookup"><span data-stu-id="1a5b1-857">`count` (max number of facet terms; default is 10).</span></span> <span data-ttu-id="1a5b1-858">Det finns inget maximum men högre värden medföra en motsvarande prestandaförsämring, särskilt om fasetterad fältet innehåller ett stort antal unika villkor.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-858">There is no maximum, but higher values incur a corresponding performance penalty, especially if the faceted field contains a large number of unique terms.</span></span>
  * <span data-ttu-id="1a5b1-859">Till exempel: `facet=category,count:5` hämtar upp fem kategorier i aspekten resultat.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-859">For example: `facet=category,count:5` gets the top five categories in facet results.</span></span>  
  * <span data-ttu-id="1a5b1-860">**Obs**: om den `count` parameter är mindre än antalet unika villkor, resultaten kan vara felaktig.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-860">**Note**: If the `count` parameter is less than the number of unique terms, the results may not be accurate.</span></span> <span data-ttu-id="1a5b1-861">Det beror på hur faceting frågor är fördelade på shards.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-861">This is due to the way faceting queries are distributed across shards.</span></span> <span data-ttu-id="1a5b1-862">Öka `count` vanligtvis ökar exaktheten termen antal, men på en prestandakostnad.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-862">Increasing `count` generally increases the accuracy of the term counts, but at a performance cost.</span></span>
* <span data-ttu-id="1a5b1-863">`sort`(en av `count` att sortera *fallande* efter antal `-count` att sortera *stigande* efter antal `value` att sortera *stigande* efter värde, eller `-value` att sortera *fallande* av värdet)</span><span class="sxs-lookup"><span data-stu-id="1a5b1-863">`sort` (one of `count` to sort *descending* by count, `-count` to sort *ascending* by count, `value` to sort *ascending* by value, or `-value` to sort *descending* by value)</span></span>
  * <span data-ttu-id="1a5b1-864">Till exempel: `facet=category,count:3,sort:count` hämtar de tre översta kategorier i aspekten resulterar i fallande ordning efter antalet dokument med varje Ortnamn.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-864">For example: `facet=category,count:3,sort:count` gets the top three categories in facet results in descending order by the number of documents with each city name.</span></span> <span data-ttu-id="1a5b1-865">Till exempel om de översta tre kategorierna är Budget, översikt och Lyxig, och Budget har 5 träffar, översikt har 6 och Lyxig har 4, blir sedan buckets i ordningen som översikt, Budget, Lyxig.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-865">For example, if the top three categories are Budget, Motel, and Luxury, and Budget has 5 hits, Motel has 6, and Luxury has 4, then the buckets will be in the order Motel, Budget, Luxury.</span></span>
  * <span data-ttu-id="1a5b1-866">Till exempel: `facet=rating,sort:-value` producerar buckets för alla möjliga betyg i fallande ordning efter värde.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-866">For example: `facet=rating,sort:-value` produces buckets for all possible ratings, in descending order by value.</span></span> <span data-ttu-id="1a5b1-867">Till exempel om klassificeringarna är från 1 till 5, beställs buckets 5, 4, 3, 2, 1, oavsett hur många dokument matchar varje klassificering.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-867">For example, if the ratings are from 1 to 5, the buckets will be ordered 5, 4, 3, 2, 1, irrespective of how many documents match each rating.</span></span>
* <span data-ttu-id="1a5b1-868">`values`(pipe-avgränsad numeriskt eller `Edm.DateTimeOffset` värden som anger en dynamisk värdeuppsättningen aspekten post)</span><span class="sxs-lookup"><span data-stu-id="1a5b1-868">`values` (pipe-delimited numeric or `Edm.DateTimeOffset` values specifying a dynamic set of facet entry values)</span></span>
  * <span data-ttu-id="1a5b1-869">Till exempel: `facet=baseRate,values:10|20` ger tre buckets: en för grundavgift 0 till men med upp till 10, en för 10, men inte inklusive 20 och en för 20 eller högre.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-869">For example: `facet=baseRate,values:10|20` produces three buckets: One for base rate 0 up to but not including 10, one for 10 up to but not including 20, and one for 20 or higher.</span></span>
  * <span data-ttu-id="1a5b1-870">Till exempel: `facet=lastRenovationDate,values:2010-02-01T00:00:00Z` ger två buckets: en för hotell renoverade före februari 2010 och en för hotell renoverade februari 1 2010 eller senare.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-870">For example: `facet=lastRenovationDate,values:2010-02-01T00:00:00Z` produces two buckets: One for hotels renovated before February 2010, and one for hotels renovated February 1st, 2010 or later.</span></span>
* <span data-ttu-id="1a5b1-871">`interval`(heltal-intervall som är större än 0 för siffror eller `minute`, `hour`, `day`, `week`, `month`, `quarter`, `year` för värden för datum-tid)</span><span class="sxs-lookup"><span data-stu-id="1a5b1-871">`interval` (integer interval greater than 0 for numbers, or `minute`, `hour`, `day`, `week`, `month`, `quarter`, `year` for date time values)</span></span>
  * <span data-ttu-id="1a5b1-872">Till exempel: `facet=baseRate,interval:100` producerar buckets baserat på grundavgift intervall i storlek 100.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-872">For example: `facet=baseRate,interval:100` produces buckets based on base rate ranges of size 100.</span></span> <span data-ttu-id="1a5b1-873">Till exempel om grundpris alla mellan $60 och $600, blir det buckets för 0-100, 200 100, 200 300, 300-400, 400 500 och 500 600.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-873">For example, if base rates are all between $60 and $600, there will be buckets for 0-100, 100-200, 200-300, 300-400, 400-500, and 500-600.</span></span>
  * <span data-ttu-id="1a5b1-874">Till exempel: `facet=lastRenovationDate,interval:year` producerar en bucket för varje år när hotell har renoverade.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-874">For example: `facet=lastRenovationDate,interval:year` produces one bucket for each year when hotels were renovated.</span></span>
* <span data-ttu-id="1a5b1-875">`timeoffset`([+-] hh: mm, [+-] hhmm, eller [+-] hh) `timeoffset` är valfritt.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-875">`timeoffset` ([+-]hh:mm, [+-]hhmm, or [+-]hh) `timeoffset` is optional.</span></span> <span data-ttu-id="1a5b1-876">Kan endast kombineras med de `interval` alternativet endast när tillämpas på ett fält av typen `Edm.DateTimeOffset`.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-876">It can only be combined with the `interval` option, and only when applied to a field of type `Edm.DateTimeOffset`.</span></span> <span data-ttu-id="1a5b1-877">Värdet anger förskjutning för UTC-tid till kontot för att tid gränser.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-877">The value specifies the UTC time offset to account for in setting time boundaries.</span></span>
  * <span data-ttu-id="1a5b1-878">Till exempel: `facet=lastRenovationDate,interval:day,timeoffset:-01:00` använder dag gränsen som börjar på UTC 01:00:00 (midnatt i tidszon mål)</span><span class="sxs-lookup"><span data-stu-id="1a5b1-878">For example: `facet=lastRenovationDate,interval:day,timeoffset:-01:00` uses the day boundary that starts at 01:00:00 UTC (midnight in the target time zone)</span></span>
* <span data-ttu-id="1a5b1-879">**Obs**: `count` och `sort` kan kombineras i samma aspekten specifikationen, men de kan inte kombineras med `interval` eller `values`, och `interval` och `values` kan inte kombineras.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-879">**Note**: `count` and `sort` can be combined in the same facet specification, but they cannot be combined with `interval` or `values`, and `interval` and `values` cannot be combined together.</span></span>
* <span data-ttu-id="1a5b1-880">**Obs**: intervall aspekter på datum och tid beräknas baserat på UTC-tid om `timeoffset` har inte angetts.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-880">**Note**: Interval facets on date time are computed based on UTC time if `timeoffset` is not specified.</span></span> <span data-ttu-id="1a5b1-881">Exempel: för `facet=lastRenovationDate,interval:day`, gräns startas på 00:00:00 UTC.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-881">For example: for `facet=lastRenovationDate,interval:day`, the day boundary starts at 00:00:00 UTC.</span></span> 

> [!NOTE]
> <span data-ttu-id="1a5b1-882">När du anropar **Sök** med efter den här parametern heter `facets` i stället för `facet`.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-882">When calling **Search** using POST, this parameter is named `facets` instead of `facet`.</span></span> <span data-ttu-id="1a5b1-883">Du också ange den som en JSON-matris med strängar som där varje sträng är ett separat aspekten uttryck.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-883">Also, you specify it as a JSON array of strings where each string is a separate facet expression.</span></span>
> 
> 

<span data-ttu-id="1a5b1-884">`$filter=[string]`(valfritt) – ett strukturerade sökuttryck standard OData-syntax.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-884">`$filter=[string]` (optional) - A structured search expression in standard OData syntax.</span></span>

> [!NOTE]
> <span data-ttu-id="1a5b1-885">När du anropar **Sök** med efter den här parametern heter `filter` i stället för `$filter`.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-885">When calling **Search** using POST, this parameter is named `filter` instead of `$filter`.</span></span>
> 
> 

<span data-ttu-id="1a5b1-886">`highlight=[string]`(valfritt) – en uppsättning csv-fältnamn som används för träffar markeras.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-886">`highlight=[string]` (optional) - A set of comma-separated field names used for hit highlights.</span></span> <span data-ttu-id="1a5b1-887">Endast `searchable` fält kan användas för träffar markering.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-887">Only `searchable` fields can be used for hit highlighting.</span></span>

<span data-ttu-id="1a5b1-888">`highlightPreTag=[string]`(valfritt, som standard `<em>`) – en string-tagg som annat för att träffa visar.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-888">`highlightPreTag=[string]` (optional, defaults to `<em>`) - A string tag that prepends to hit highlights.</span></span> <span data-ttu-id="1a5b1-889">Måste anges med `highlightPostTag`.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-889">Must be set with `highlightPostTag`.</span></span>

> [!NOTE]
> <span data-ttu-id="1a5b1-890">När du anropar **Sök** använder GET, reserverade tecken i URL-Adressen måste vara procent-kodat (till exempel % 23 i stället för #).</span><span class="sxs-lookup"><span data-stu-id="1a5b1-890">When calling **Search** using GET, reserved characters in the URL must be percent-encoded (for example, %23 instead of #).</span></span>
> 
> 

<span data-ttu-id="1a5b1-891">`highlightPostTag=[string]`(valfritt, som standard `</em>`)-taggen sträng som lägger till för att träffa visar.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-891">`highlightPostTag=[string]` (optional, defaults to `</em>`) - a string tag that appends to hit highlights.</span></span> <span data-ttu-id="1a5b1-892">Måste anges med `highlightPreTag`.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-892">Must be set with `highlightPreTag`.</span></span>

> [!NOTE]
> <span data-ttu-id="1a5b1-893">När du anropar **Sök** använder GET, reserverade tecken i URL-Adressen måste vara procent-kodat (till exempel % 23 i stället för #).</span><span class="sxs-lookup"><span data-stu-id="1a5b1-893">When calling **Search** using GET, reserved characters in the URL must be percent-encoded (for example, %23 instead of #).</span></span>
> 
> 

<span data-ttu-id="1a5b1-894">`scoringProfile=[string]`(valfritt) – namnet på bedömningsprofilen att utvärdera matcha resultat för att matcha dokument för att sortera resultaten.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-894">`scoringProfile=[string]` (optional) - The name of a scoring profile to evaluate match scores for matching documents in order to sort the results.</span></span>

<span data-ttu-id="1a5b1-895">`scoringParameter=[string]`(noll eller fler) – anger värden för varje parameter som definierats i funktionen för bedömningsprofil (till exempel `referencePointParameter`) i formatet `name-value1,value2,...`.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-895">`scoringParameter=[string]` (zero or more) - Indicates the values for each parameter defined in a scoring function (for example, `referencePointParameter`) using the format `name-value1,value2,...`.</span></span>

* <span data-ttu-id="1a5b1-896">Till exempel om bedömningsprofilen definierar en funktion med en parameter med namnet ”mylocation” sträng frågealternativet skulle vara `&scoringParameter=mylocation--122.2,44.8`.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-896">For example, if the scoring profile defines a function with a parameter called "mylocation" the query string option would be `&scoringParameter=mylocation--122.2,44.8`.</span></span> <span data-ttu-id="1a5b1-897">Första streck skiljer namnet från listan, medan andra streck är en del av det första värdet (longitud i det här exemplet).</span><span class="sxs-lookup"><span data-stu-id="1a5b1-897">The first dash separates the name from the value list, while the second dash is part of the first value (longitude in this example).</span></span>
* <span data-ttu-id="1a5b1-898">För bedömningsprofil parametrar som taggar för förstärkning som kan innehålla kommatecken, kan du undanta sådana värden i listan med enkla citattecken.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-898">For scoring parameters such as for tag boosting that can contain commas, you can escape any such values in the list using single quotes.</span></span> <span data-ttu-id="1a5b1-899">Om själva värdena innehåller enkla citattecken, kan du undanta dem av dubblerade.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-899">If the values themselves contain single quotes, you can escape them by doubling.</span></span>
  * <span data-ttu-id="1a5b1-900">Till exempel om du har en tagg förstärkning parameter med namnet ”mytag” och du vill höja för taggen värden ”Hello, O'Brien” och ”Smith” frågan alternativ för anslutningssträngen skulle vara `&scoringParameter=mytag-'Hello, O''Brien',Smith`.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-900">For example, if you have a tag boosting parameter called "mytag" and you want to boost on the tag values "Hello, O'Brien" and "Smith", the query string option would be `&scoringParameter=mytag-'Hello, O''Brien',Smith`.</span></span> <span data-ttu-id="1a5b1-901">Observera att citattecken är endast för värden med kommatecken.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-901">Note that quotes are only required for values that contain commas.</span></span>

> [!NOTE]
> <span data-ttu-id="1a5b1-902">När du anropar **Sök** med efter den här parametern heter `scoringParameters` i stället för `scoringParameter`.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-902">When calling **Search** using POST, this parameter is named `scoringParameters` instead of `scoringParameter`.</span></span> <span data-ttu-id="1a5b1-903">Du också ange den som en JSON-matris med strängar som där varje sträng är en separat `name-values` par.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-903">Also, you specify it as a JSON array of strings where each string is a separate `name-values` pair.</span></span>
> 
> 

<span data-ttu-id="1a5b1-904">`minimumCoverage`(valfritt, standardvärdet är 100)-ett tal mellan 0 och 100 som anger procentandelen av det index som måste förses med en sökfråga för frågan som ska rapporteras som lyckas.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-904">`minimumCoverage` (optional, defaults to 100) - a number between 0 and 100 indicating the percentage of the index that must be covered by a search query in order for the query to be reported as a success.</span></span> <span data-ttu-id="1a5b1-905">Som standard hela indexet måste vara tillgängliga eller `Search` returneras HTTP-statuskoden 503.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-905">By default, the entire index must be available or `Search` will return HTTP status code 503.</span></span> <span data-ttu-id="1a5b1-906">Om du ställer in `minimumCoverage` och `Search` lyckas den ska returnera HTTP 200 och innehålla en `@search.coverage` värde i svaret som anger procentandelen av det index som ingick i frågan.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-906">If you set `minimumCoverage` and `Search` succeeds, it will return HTTP 200 and include a `@search.coverage` value in the response indicating the percentage of the index that was included in the query.</span></span>

> [!NOTE]
> <span data-ttu-id="1a5b1-907">Ange den här parametern till ett värde som är lägre än 100 kan vara användbart för att söka efter tillgänglighet även för tjänster med bara en replik.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-907">Setting this parameter to a value lower than 100 can be useful for ensuring search availability even for services with only one replica.</span></span> <span data-ttu-id="1a5b1-908">Dock är inte alla matchande dokument garanterat måste finnas i sökresultaten.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-908">However, not all matching documents are guaranteed to be present in the search results.</span></span> <span data-ttu-id="1a5b1-909">Om Sök återkalla är viktigare för ditt program än tillgänglighet, så det är bäst att lämna `minimumCoverage` standardinställningen 100.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-909">If search recall is more important to your application than availability, then it's best to leave `minimumCoverage` at its default value of 100.</span></span>
> 
> 

<span data-ttu-id="1a5b1-910">`api-version=[string]`(krävs).</span><span class="sxs-lookup"><span data-stu-id="1a5b1-910">`api-version=[string]` (required).</span></span> <span data-ttu-id="1a5b1-911">Förhandsversionen är `api-version=2015-02-28-Preview`.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-911">The preview version is `api-version=2015-02-28-Preview`.</span></span> <span data-ttu-id="1a5b1-912">Se [Sök Versionhantering](http://msdn.microsoft.com/library/azure/dn864560.aspx) information och alternativa versioner.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-912">See [Search Service Versioning](http://msdn.microsoft.com/library/azure/dn864560.aspx) for details and alternative versions.</span></span>

<span data-ttu-id="1a5b1-913">Obs: för den här åtgärden den `api-version` har angetts som en frågeparameter i URL: en oavsett om du anropar **Sök** med GET eller POST.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-913">Note: For this operation, the `api-version` is specified as a query parameter in the URL regardless of whether you call **Search** with GET or POST.</span></span>

<span data-ttu-id="1a5b1-914">**Huvuden för begäran**</span><span class="sxs-lookup"><span data-stu-id="1a5b1-914">**Request Headers**</span></span>

<span data-ttu-id="1a5b1-915">I följande lista beskrivs de obligatoriska och valfria begärandehuvudena.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-915">The following list describes the required and optional request headers.</span></span>

* <span data-ttu-id="1a5b1-916">`api-key`: `api-key` Används för att autentisera begäran om att din söktjänst.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-916">`api-key`: The `api-key` is used to authenticate the request to your Search service.</span></span> <span data-ttu-id="1a5b1-917">Det är ett strängvärde som unik för din tjänst-URL.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-917">It is a string value, unique to your service URL.</span></span> <span data-ttu-id="1a5b1-918">Den **Sök** begäran kan ange en administrationsnyckeln eller Frågenyckeln för `api-key`.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-918">The **Search** request can specify either an admin key or query key for `api-key`.</span></span>

<span data-ttu-id="1a5b1-919">Du måste också tjänstnamnet att konstruera den begärda Webbadressen.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-919">You will also need the service name to construct the request URL.</span></span> <span data-ttu-id="1a5b1-920">Du kan hämta namnet på tjänsten och `api-key` från tjänsten instrumentpanelen i Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-920">You can get the service name and `api-key` from your service dashboard in the Azure Portal.</span></span> <span data-ttu-id="1a5b1-921">Se [skapa en Azure Search-tjänst i portalen](search-create-service-portal.md) sidan navigering hjälp.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-921">See [Create an Azure Search service in the portal](search-create-service-portal.md) for page navigation help.</span></span>

<span data-ttu-id="1a5b1-922">**Begärandetexten**</span><span class="sxs-lookup"><span data-stu-id="1a5b1-922">**Request Body**</span></span>

<span data-ttu-id="1a5b1-923">För GET: Ingen.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-923">For GET: None.</span></span>

<span data-ttu-id="1a5b1-924">För POST:</span><span class="sxs-lookup"><span data-stu-id="1a5b1-924">For POST:</span></span>

    {
      "count": true | false (default),
      "facets": [ "facet_expression_1", "facet_expression_2", ... ],
      "filter": "odata_filter_expression",
      "highlight": "highlight_field_1, highlight_field_2, ...",
      "highlightPreTag": "pre_tag",
      "highlightPostTag": "post_tag",
      "minimumCoverage": # (% of index that must be covered to declare query successful; default 100),
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

<span data-ttu-id="1a5b1-925">**Fortsättning av svar som partiell sökning**</span><span class="sxs-lookup"><span data-stu-id="1a5b1-925">**Continuation of Partial Search Responses**</span></span>

<span data-ttu-id="1a5b1-926">Ibland kan inte Azure Search returnera alla begärda resultat i ett enda Search-svar.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-926">Sometimes Azure Search can't return all the requested results in a single Search response.</span></span> <span data-ttu-id="1a5b1-927">Detta kan inträffa av olika skäl, till exempel när begäranden för många dokument genom att inte ange `$top` eller ange ett värde för `$top` som är för stor.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-927">This can happen for different reasons, such as when the query requests too many documents by not specifying `$top` or specifying a value for `$top` that is too large.</span></span> <span data-ttu-id="1a5b1-928">I sådana fall Azure Search innehåller den `@odata.nextLink` kommentar i brödtext för svar, och även `@search.nextPageParameters` om den har en POST-begäran.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-928">In such cases, Azure Search will include the `@odata.nextLink` annotation in the response body, and also `@search.nextPageParameters` if it was a POST request.</span></span> <span data-ttu-id="1a5b1-929">Du kan använda värdena för dessa anteckningar för att formulera en annan sökbegäran att hämta nästa del av search-svar.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-929">You can use the values of these annotations to formulate another Search request to get the next part of the search response.</span></span> <span data-ttu-id="1a5b1-930">Detta kallas en ***fortsättning*** på den ursprungliga sökningen begäran och anteckningarna vanligtvis kallas ***fortsättning token***.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-930">This is called a ***continuation*** of the original Search request, and the annotations are generally called ***continuation tokens***.</span></span> <span data-ttu-id="1a5b1-931">Se [exemplet nedan](#SearchResponse) mer information om syntaxen för dessa anteckningar och var de förekommer i brödtext för svar.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-931">See [the example below](#SearchResponse) for details on the syntax of these annotations and where they appear in the response body.</span></span> 

<span data-ttu-id="1a5b1-932">Skälen varför Azure Search kan returnera fortsättning token är implementeringen-specifika och kan ändras.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-932">The reasons why Azure Search might return continuation tokens are implementation-specific and subject to change.</span></span> <span data-ttu-id="1a5b1-933">Robust klienter ska alltid vara redo att hantera fall där färre dokument än väntat returneras en fortsättningstoken ingår fortsätta hämtning av dokument.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-933">Robust clients should always be ready to handle cases where fewer documents than expected are returned and a continuation token is included to continue retrieving documents.</span></span> <span data-ttu-id="1a5b1-934">Observera också att du måste använda samma HTTP-metoden som den ursprungliga begäranden ska kunna fortsätta.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-934">Also note that you must use the same HTTP method as the original request in order to continue.</span></span> <span data-ttu-id="1a5b1-935">Till exempel om du har skickat en begäran om att hämta alla fortsättning begäranden som du skickar måste också använda GET (och även för POST).</span><span class="sxs-lookup"><span data-stu-id="1a5b1-935">For example, if you sent a GET request, any continuation requests you send must also use GET (and likewise for POST).</span></span>

<span data-ttu-id="1a5b1-936"><a name="SearchResponse"></a>
**Svar**</span><span class="sxs-lookup"><span data-stu-id="1a5b1-936"><a name="SearchResponse"></a>
**Response**</span></span>

<span data-ttu-id="1a5b1-937">Statuskod: 200 OK returneras för ett lyckat svar.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-937">Status Code: 200 OK is returned for a successful response.</span></span>

    {
      "@odata.count": # (if $count=true was provided in the query),
      "@search.coverage": # (if minimumCoverage was provided in the query),
      "@search.facets": { (if faceting was specified in the query)
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
      "@search.nextPageParameters": { (request body to fetch the next page of results if not all results could be returned in this response and Search was called with POST)
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
      "@odata.nextLink": (URL to fetch the next page of results if not all results could be returned in this response; Applies to both GET and POST)
    }

<span data-ttu-id="1a5b1-938">**Exempel:**</span><span class="sxs-lookup"><span data-stu-id="1a5b1-938">**Examples:**</span></span>

<span data-ttu-id="1a5b1-939">Du hittar ytterligare exempel på den [syntaxen för OData-uttryck för Azure Search](https://msdn.microsoft.com/library/azure/dn798921.aspx) sidan.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-939">You can find additional examples on the [OData Expression Syntax for Azure Search](https://msdn.microsoft.com/library/azure/dn798921.aspx) page.</span></span>

1)    <span data-ttu-id="1a5b1-940">Sök indexet sorterade i fallande efter datum.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-940">Search the Index sorted descending by date.</span></span>

    <span data-ttu-id="1a5b1-941">GET-/indexes/hotels/docs? Sök = * & $orderby = lastRenovationDate desc & api-version = 2015-02-28-Preview</span><span class="sxs-lookup"><span data-stu-id="1a5b1-941">GET /indexes/hotels/docs?search=*&$orderby=lastRenovationDate desc&api-version=2015-02-28-Preview</span></span>

    <span data-ttu-id="1a5b1-942">POST /indexes/hotels/docs/search? api-version 2015-02-28-Preview = {”Sök” ”: *”, ”orderby”: ”lastRenovationDate desc”}</span><span class="sxs-lookup"><span data-stu-id="1a5b1-942">POST /indexes/hotels/docs/search?api-version=2015-02-28-Preview { "search": "*", "orderby": "lastRenovationDate desc" }</span></span>

2)    <span data-ttu-id="1a5b1-943">Söka i en fasetterad sökning och hämta facets för kategorier, klassificering, etiketter samt artiklar med baseRate i särskilda intervall:</span><span class="sxs-lookup"><span data-stu-id="1a5b1-943">In a faceted search, search the index and retrieve facets for categories, rating, tags, as well as items with baseRate in specific ranges:</span></span>

    <span data-ttu-id="1a5b1-944">GET-/indexes/hotels/docs? Sök = test & aspekten = kategori & aspekten = klassificering & aspekten = taggar & aspekten = baseRate värden: 80 | 150 | 220 & api-version = 2015-02-28-Preview</span><span class="sxs-lookup"><span data-stu-id="1a5b1-944">GET /indexes/hotels/docs?search=test&facet=category&facet=rating&facet=tags&facet=baseRate,values:80|150|220&api-version=2015-02-28-Preview</span></span>

    <span data-ttu-id="1a5b1-945">POST /indexes/hotels/docs/search? api-version 2015-02-28-Preview = {”Sök”: ”test”, ”facets”: [”kategori”, ”klassificering”, ”-taggar” ”baseRate värden: 80 | 150 | 220”]}</span><span class="sxs-lookup"><span data-stu-id="1a5b1-945">POST /indexes/hotels/docs/search?api-version=2015-02-28-Preview { "search": "test", "facets": [ "category", "rating", "tags", "baseRate,values:80|150|220" ] }</span></span>

3)    <span data-ttu-id="1a5b1-946">Med hjälp av ett filter, begränsa föregående fasetterad frågeresultaten när användaren klickar på klassificeringen 3 och kategori ”översikt”:</span><span class="sxs-lookup"><span data-stu-id="1a5b1-946">Using a filter, narrow down the previous faceted query results after the user clicks on rating 3 and category "Motel":</span></span>

    <span data-ttu-id="1a5b1-947">GET-/indexes/hotels/docs? Sök = test & aspekten = taggar & aspekten = baseRate värden: 80 | 150 | 220 & $filter = klassificering eq 3 och kategori eq 'översikt' & api-version = 2015-02-28-Preview</span><span class="sxs-lookup"><span data-stu-id="1a5b1-947">GET /indexes/hotels/docs?search=test&facet=tags&facet=baseRate,values:80|150|220&$filter=rating eq 3 and category eq 'Motel'&api-version=2015-02-28-Preview</span></span>

    <span data-ttu-id="1a5b1-948">POST /indexes/hotels/docs/search? api-version = 2015-02-28-Preview {”Sök”: ”test”, ”facets”: [”taggar” ”, baseRate värden: 80 | 150 | 220”], ”filter”: ”klassificering eq 3 och kategori eq 'Översikt'”}</span><span class="sxs-lookup"><span data-stu-id="1a5b1-948">POST /indexes/hotels/docs/search?api-version=2015-02-28-Preview { "search": "test", "facets": [ "tags", "baseRate,values:80|150|220" ], "filter": "rating eq 3 and category eq 'Motel'" }</span></span>

4) <span data-ttu-id="1a5b1-949">Ange en övre gräns på unikt villkor som returneras i en fråga i en fasetterad sökning.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-949">In a faceted search, set an upper limit on unique terms returned in a query.</span></span> <span data-ttu-id="1a5b1-950">Standardvärdet är 10, men du kan öka eller minska det här värdet med den `count` parameter på den `facet` attribut:</span><span class="sxs-lookup"><span data-stu-id="1a5b1-950">The default is 10, but you can increase or decrease this value using the `count` parameter on the `facet` attribute:</span></span>

    <span data-ttu-id="1a5b1-951">GET-/indexes/hotels/docs? Sök = test & aspekten = ort, antal: 5 & api-version = 2015-02-28-Preview</span><span class="sxs-lookup"><span data-stu-id="1a5b1-951">GET /indexes/hotels/docs?search=test&facet=city,count:5&api-version=2015-02-28-Preview</span></span>

    <span data-ttu-id="1a5b1-952">POST /indexes/hotels/docs/search? api-version 2015-02-28-Preview = {”Sök”: ”test”, ”facets”: [”Ort, antal: 5”]}</span><span class="sxs-lookup"><span data-stu-id="1a5b1-952">POST /indexes/hotels/docs/search?api-version=2015-02-28-Preview  {    "search": "test",    "facets": [ "city,count:5" ]  }</span></span>

5)    <span data-ttu-id="1a5b1-953">Söka inom specifika områden. Till exempel ett fält för vissa språk:</span><span class="sxs-lookup"><span data-stu-id="1a5b1-953">Search the Index within specific fields; For example, a language-specific field:</span></span>

    <span data-ttu-id="1a5b1-954">GET-/indexes/hotels/docs? Sök = hôtel & searchFields = description_fr & api-version = 2015-02-28-Preview</span><span class="sxs-lookup"><span data-stu-id="1a5b1-954">GET /indexes/hotels/docs?search=hôtel&searchFields=description_fr&api-version=2015-02-28-Preview</span></span>

    <span data-ttu-id="1a5b1-955">POST /indexes/hotels/docs/search? api-version 2015-02-28-Preview = {”Sök”: ”hôtel”, ”searchFields”: ”description_fr”}</span><span class="sxs-lookup"><span data-stu-id="1a5b1-955">POST /indexes/hotels/docs/search?api-version=2015-02-28-Preview { "search": "hôtel", "searchFields": "description_fr" }</span></span>

6) <span data-ttu-id="1a5b1-956">Sök Index över flera fält.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-956">Search the Index across multiple fields.</span></span> <span data-ttu-id="1a5b1-957">Du kan till exempel lagra och fråga sökbara fält på flera olika språk, allt i samma index.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-957">For example, you can store and query searchable fields in multiple languages, all within the same index.</span></span>  <span data-ttu-id="1a5b1-958">Om engelska och franska beskrivningar finnas samtidigt i samma dokument, returnerar någon eller samtliga i resultatet av frågan:</span><span class="sxs-lookup"><span data-stu-id="1a5b1-958">If English and French descriptions co-exist in the same document, you can return any or all in the query results:</span></span>

    <span data-ttu-id="1a5b1-959">GET-/indexes/hotels/docs? Sök = hotell & searchFields = beskrivning, description_fr & api-version = 2015-02-28-Preview</span><span class="sxs-lookup"><span data-stu-id="1a5b1-959">GET /indexes/hotels/docs?search=hotel&searchFields=description,description_fr&api-version=2015-02-28-Preview</span></span>

    <span data-ttu-id="1a5b1-960">POST /indexes/hotels/docs/search? api-version 2015-02-28-Preview = {”Sök”: ”hotell”, ”searchFields”: ”beskrivning, description_fr”}</span><span class="sxs-lookup"><span data-stu-id="1a5b1-960">POST /indexes/hotels/docs/search?api-version=2015-02-28-Preview  {    "search": "hotel",    "searchFields": "description, description_fr"  }</span></span>

<span data-ttu-id="1a5b1-961">Observera att du bara kan fråga ett index i taget.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-961">Note that you can only query one index at a time.</span></span> <span data-ttu-id="1a5b1-962">Skapa inte flera index för varje språk om du planerar att fråga en i taget.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-962">Do not create multiple indexes for each language unless you plan to query one at a time.</span></span>

7)    <span data-ttu-id="1a5b1-963">Växlingsfil - Get sidan 1 objekt (sidstorlek är 10):</span><span class="sxs-lookup"><span data-stu-id="1a5b1-963">Paging - Get the 1st page of items (page size is 10):</span></span>

    <span data-ttu-id="1a5b1-964">GET-/indexes/hotels/docs? Sök = * & $skip = 0 & $top = 10 & api-version = 2015-02-28-Preview</span><span class="sxs-lookup"><span data-stu-id="1a5b1-964">GET /indexes/hotels/docs?search=*&$skip=0&$top=10&api-version=2015-02-28-Preview</span></span>

    <span data-ttu-id="1a5b1-965">POST /indexes/hotels/docs/search? api-version 2015-02-28-Preview = {”Sök” ”: *”, ”hoppa över”: 0, ”top”: 10}</span><span class="sxs-lookup"><span data-stu-id="1a5b1-965">POST /indexes/hotels/docs/search?api-version=2015-02-28-Preview { "search": "*", "skip": 0, "top": 10 }</span></span>

8)    <span data-ttu-id="1a5b1-966">Växlingsfil - Get sidan 2 i objekt (sidstorlek är 10):</span><span class="sxs-lookup"><span data-stu-id="1a5b1-966">Paging - Get the 2nd page of items (page size is 10):</span></span>

    <span data-ttu-id="1a5b1-967">GET-/indexes/hotels/docs? Sök = * & $skip = 10 & $top = 10 & api-version = 2015-02-28-Preview</span><span class="sxs-lookup"><span data-stu-id="1a5b1-967">GET /indexes/hotels/docs?search=*&$skip=10&$top=10&api-version=2015-02-28-Preview</span></span>

    <span data-ttu-id="1a5b1-968">POST /indexes/hotels/docs/search? api-version 2015-02-28-Preview = {”Sök” ”: *”, ”hoppa över”: ”top” 10: 10}</span><span class="sxs-lookup"><span data-stu-id="1a5b1-968">POST /indexes/hotels/docs/search?api-version=2015-02-28-Preview { "search": "*", "skip": 10, "top": 10 }</span></span>

9)    <span data-ttu-id="1a5b1-969">Hämta en specifik uppsättning fält:</span><span class="sxs-lookup"><span data-stu-id="1a5b1-969">Retrieve a specific set of fields:</span></span>

    <span data-ttu-id="1a5b1-970">GET-/indexes/hotels/docs? Sök = * & $select = hotelName, beskrivning och api-version = 2015-02-28-Preview</span><span class="sxs-lookup"><span data-stu-id="1a5b1-970">GET /indexes/hotels/docs?search=*&$select=hotelName,description&api-version=2015-02-28-Preview</span></span>

    <span data-ttu-id="1a5b1-971">POST /indexes/hotels/docs/search? api-version 2015-02-28-Preview = {”Sök” ”: *”, ”Välj”: ”hotelName beskrivning”}</span><span class="sxs-lookup"><span data-stu-id="1a5b1-971">POST /indexes/hotels/docs/search?api-version=2015-02-28-Preview { "search": "*", "select": "hotelName, description" }</span></span>

10)  <span data-ttu-id="1a5b1-972">Hämta dokument som matchar ett specifikt filter-uttryck:</span><span class="sxs-lookup"><span data-stu-id="1a5b1-972">Retrieve documents matching a specific filter expression:</span></span>

    <span data-ttu-id="1a5b1-973">GET-/indexes/hotels/docs? $filter = (baseRate ge 60 och baseRate lt 300) eller hotelName eq 'Avancerad förblir' & api-version = 2015-02-28-Preview</span><span class="sxs-lookup"><span data-stu-id="1a5b1-973">GET /indexes/hotels/docs?$filter=(baseRate ge 60 and baseRate lt 300) or hotelName eq 'Fancy Stay'&api-version=2015-02-28-Preview</span></span>

    <span data-ttu-id="1a5b1-974">POST /indexes/hotels/docs/search? api-version 2015-02-28-Preview = {”filter” ”: (baseRate ge 60 och baseRate lt 300) eller hotelName eq 'Avancerad stanna'”}</span><span class="sxs-lookup"><span data-stu-id="1a5b1-974">POST /indexes/hotels/docs/search?api-version=2015-02-28-Preview {  "filter": "(baseRate ge 60 and baseRate lt 300) or hotelName eq 'Fancy Stay'" }</span></span>

11) <span data-ttu-id="1a5b1-975">Sök index och returnera fragment med träffar viktiga funktioner</span><span class="sxs-lookup"><span data-stu-id="1a5b1-975">Search the index and return fragments with hit highlights</span></span>

    <span data-ttu-id="1a5b1-976">GET-/indexes/hotels/docs? Sök = något & Markera = beskrivning & api-version = 2015-02-28-Preview</span><span class="sxs-lookup"><span data-stu-id="1a5b1-976">GET /indexes/hotels/docs?search=something&highlight=description&api-version=2015-02-28-Preview</span></span>

    <span data-ttu-id="1a5b1-977">POST /indexes/hotels/docs/search? api-version 2015-02-28-Preview = {”Sök”: ”något”, ”highlight”: ”beskrivning”}</span><span class="sxs-lookup"><span data-stu-id="1a5b1-977">POST /indexes/hotels/docs/search?api-version=2015-02-28-Preview {   "search": "something",   "highlight": "description" }</span></span>

12) <span data-ttu-id="1a5b1-978">Söka och returnera dokument sorteras från längre bort från en referens närmare plats</span><span class="sxs-lookup"><span data-stu-id="1a5b1-978">Search the index and return documents sorted from closer to farther away from a reference location</span></span>

    <span data-ttu-id="1a5b1-979">GET-/indexes/hotels/docs? Sök = något & $orderby=geo.distance (plats, geography'POINT(-122.12315 47.88121)') & api-version = 2015-02-28-Preview</span><span class="sxs-lookup"><span data-stu-id="1a5b1-979">GET /indexes/hotels/docs?search=something&$orderby=geo.distance(location, geography'POINT(-122.12315 47.88121)')&api-version=2015-02-28-Preview</span></span>

    <span data-ttu-id="1a5b1-980">POST /indexes/hotels/docs/search? api-version 2015-02-28-Preview = {”Sök”: ”något”, ”orderby” ”: geo.distance (plats, geography'POINT(-122.12315 47.88121)')”}</span><span class="sxs-lookup"><span data-stu-id="1a5b1-980">POST /indexes/hotels/docs/search?api-version=2015-02-28-Preview {   "search": "something",   "orderby": "geo.distance(location, geography'POINT(-122.12315 47.88121)')" }</span></span>

13) <span data-ttu-id="1a5b1-981">Söka under förutsättning att det finns en bedömningsprofilen som kallas ”geo” med två avståndsresultatfunktioner, en anger en parameter som kallas ”currentLocation” och en definierar en parameter med namnet ”lastLocation”</span><span class="sxs-lookup"><span data-stu-id="1a5b1-981">Search the index assuming there's a scoring profile called "geo" with two distance scoring functions, one defining a parameter called "currentLocation" and one defining a parameter called "lastLocation"</span></span>

    <span data-ttu-id="1a5b1-982">GET-/indexes/hotels/docs? Sök = något & scoringProfile = geo & scoringParameter = currentLocation--122.123,44.77233 & scoringParameter = lastLocation--121.499,44.2113 & api-version = 2015-02-28-Preview</span><span class="sxs-lookup"><span data-stu-id="1a5b1-982">GET /indexes/hotels/docs?search=something&scoringProfile=geo&scoringParameter=currentLocation--122.123,44.77233&scoringParameter=lastLocation--121.499,44.2113&api-version=2015-02-28-Preview</span></span>

    <span data-ttu-id="1a5b1-983">POST /indexes/hotels/docs/search? api-version 2015-02-28-Preview = {”Sök”: ”något”, ”scoringProfile”: ”geo”, ”scoringParameters”: [”currentLocation--122.123,44.77233” ”, lastLocation--121.499,44.2113”]}</span><span class="sxs-lookup"><span data-stu-id="1a5b1-983">POST /indexes/hotels/docs/search?api-version=2015-02-28-Preview {   "search": "something",   "scoringProfile": "geo",   "scoringParameters": [ "currentLocation--122.123,44.77233", "lastLocation--121.499,44.2113" ] }</span></span>

14) <span data-ttu-id="1a5b1-984">Hitta dokument i index med [enkel frågesyntaxen](https://msdn.microsoft.com/library/dn798920.aspx).</span><span class="sxs-lookup"><span data-stu-id="1a5b1-984">Find documents in the index using [simple query syntax](https://msdn.microsoft.com/library/dn798920.aspx).</span></span> <span data-ttu-id="1a5b1-985">Den här frågan returnerar hotell där sökbara fält innehåller villkoren ”bekvämlighet” och ”plats” men inte ”översikt”:</span><span class="sxs-lookup"><span data-stu-id="1a5b1-985">This query returns hotels where searchable fields contain the terms "comfort" and "location" but not "motel":</span></span>

    <span data-ttu-id="1a5b1-986">GET-/indexes/hotels/docs? Sök = bekvämlighet + plats-översikt & searchMode = all & api-version = 2015-02-28-Preview</span><span class="sxs-lookup"><span data-stu-id="1a5b1-986">GET /indexes/hotels/docs?search=comfort +location -motel&searchMode=all&api-version=2015-02-28-Preview</span></span>

    <span data-ttu-id="1a5b1-987">POST /indexes/hotels/docs/search? api-version 2015-02-28-Preview = {”Sök” ”: bekvämlighet + plats-översikt”, ”searchMode”: ”alla”}</span><span class="sxs-lookup"><span data-stu-id="1a5b1-987">POST /indexes/hotels/docs/search?api-version=2015-02-28-Preview {   "search": "comfort +location -motel",   "searchMode": "all" }</span></span>

<span data-ttu-id="1a5b1-988">Observera användningen av `searchMode=all` ovan.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-988">Note the use of `searchMode=all` above.</span></span> <span data-ttu-id="1a5b1-989">Inklusive den här parametern åsidosätter standardvärdet `searchMode=any`, att se till att som `-motel` innebär ”och inte” i stället för ”eller inte”.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-989">Including this parameter overrides the default of `searchMode=any`, ensuring that `-motel` means "AND NOT" instead of "OR NOT".</span></span> <span data-ttu-id="1a5b1-990">Utan `searchMode=all`, får du ”eller inte” som utökar snarare än begränsar sökresultaten och det kan vara krånglig till vissa användare.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-990">Without `searchMode=all`, you get "OR NOT" which expands rather than restricts search results, and this can be counter-intuitive to some users.</span></span>

15) <span data-ttu-id="1a5b1-991">Hitta dokument i index med [lucene frågesyntaxen](https://msdn.microsoft.com/library/mt589323.aspx).</span><span class="sxs-lookup"><span data-stu-id="1a5b1-991">Find documents in the index using [lucene query syntax](https://msdn.microsoft.com/library/mt589323.aspx).</span></span> <span data-ttu-id="1a5b1-992">Den här frågan returnerar hotell där kategorifältet innehåller termen ”budget” och alla sökbara fält som innehåller frasen ”nyligen renoverade”.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-992">This query returns hotels where the category field contains the term "budget" and all searchable fields containing the phrase "recently renovated".</span></span> <span data-ttu-id="1a5b1-993">Dokument som innehåller frasen ”nyligen renoverade” rankas högre på grund av termen öka värdet (3)</span><span class="sxs-lookup"><span data-stu-id="1a5b1-993">Documents containing the phrase "recently renovated" are ranked higher as a result of the term boost value (3)</span></span>

    <span data-ttu-id="1a5b1-994">GET-/indexes/hotels/docs? Sök = kategori: budget och \"nyligen renoverade\"^ 3 & searchMode = all & api-version = 2015-02-28-Preview & querytype = full</span><span class="sxs-lookup"><span data-stu-id="1a5b1-994">GET /indexes/hotels/docs?search=category:budget AND \"recently renovated\"^3&searchMode=all&api-version=2015-02-28-Preview&querytype=full</span></span>

    <span data-ttu-id="1a5b1-995">POST /indexes/hotels/docs/search? api-version 2015-02-28-Preview = {”Sök” ”: kategori: budget och \"nyligen renoverade\"^ 3”, ”queryType”: ”full”, ”searchMode”: ”alla”}</span><span class="sxs-lookup"><span data-stu-id="1a5b1-995">POST /indexes/hotels/docs/search?api-version=2015-02-28-Preview {   "search": "category:budget AND \"recently renovated\"^3",   "queryType": "full",   "searchMode": "all" }</span></span>

<a name="LookupAPI"></a>

## <a name="lookup-document"></a><span data-ttu-id="1a5b1-996">Sökning dokumentet</span><span class="sxs-lookup"><span data-stu-id="1a5b1-996">Lookup Document</span></span>
<span data-ttu-id="1a5b1-997">Den **sökning dokumentet** åtgärden hämtar ett dokument från Azure Search.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-997">The **Lookup Document** operation retrieves a document from Azure Search.</span></span> <span data-ttu-id="1a5b1-998">Detta är användbart när en användare klickar på en specifik sökresultat, och du vill söka efter specifik information om dokumentet.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-998">This is useful when a user clicks on a specific search result, and you want to look up specific details about that document.</span></span>

    GET https://[service name].search.windows.net/indexes/[index name]/docs/[key]?[query parameters]
    api-key: [admin or query key]

<span data-ttu-id="1a5b1-999">**Förfrågan**</span><span class="sxs-lookup"><span data-stu-id="1a5b1-999">**Request**</span></span>

<span data-ttu-id="1a5b1-1000">HTTPS krävs för tjänstbegäranden.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-1000">HTTPS is required for service requests.</span></span> <span data-ttu-id="1a5b1-1001">Den **sökning dokumentet** begäran kan konstrueras enligt följande.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-1001">The **Lookup Document** request can be constructed as follows.</span></span>

    GET /indexes/[index name]/docs/key?[query parameters]

<span data-ttu-id="1a5b1-1002">Alternativt kan du använda den traditionella OData-syntaxen för nyckelsökningen:</span><span class="sxs-lookup"><span data-stu-id="1a5b1-1002">Alternatively, you can use the traditional OData syntax for key lookup:</span></span>

    GET /indexes('[index name]')/docs('[key]')?[query parameters]

<span data-ttu-id="1a5b1-1003">URI-begäran innehåller ett [indexnamn] och [], anger vilka dokumentet för att hämta från vilka index.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-1003">The request URI includes an [index name] and [key], specifying which document to retrieve from which index.</span></span> <span data-ttu-id="1a5b1-1004">Du kan bara hämta ett dokument åt gången.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-1004">You can only get one document at a time.</span></span> <span data-ttu-id="1a5b1-1005">Använd **Sök** att hämta flera dokument i en enskild begäran.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-1005">Use **Search** to get multiple documents in a single request.</span></span>

<span data-ttu-id="1a5b1-1006">**Frågeparametrar**</span><span class="sxs-lookup"><span data-stu-id="1a5b1-1006">**Query Parameters**</span></span>

<span data-ttu-id="1a5b1-1007">`$select=[string]`(valfritt) – en lista över kommaavgränsade fält för att hämta.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-1007">`$select=[string]` (optional) - a list of comma-separated fields to retrieve.</span></span> <span data-ttu-id="1a5b1-1008">Om Ospecificerad eller inställd på `*`, alla fält som har markerats som hämtas i schemat ingår i projektionen.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-1008">If unspecified or set to `*`, all fields marked as retrievable in the schema are included in the projection.</span></span>

<span data-ttu-id="1a5b1-1009">`api-version=[string]`(krävs).</span><span class="sxs-lookup"><span data-stu-id="1a5b1-1009">`api-version=[string]` (required).</span></span> <span data-ttu-id="1a5b1-1010">Förhandsversionen är `api-version=2015-02-28-Preview`.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-1010">The preview version is `api-version=2015-02-28-Preview`.</span></span> <span data-ttu-id="1a5b1-1011">Se [Sök Versionhantering](http://msdn.microsoft.com/library/azure/dn864560.aspx) information och alternativa versioner.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-1011">See [Search Service Versioning](http://msdn.microsoft.com/library/azure/dn864560.aspx) for details and alternative versions.</span></span>

<span data-ttu-id="1a5b1-1012">Obs: för den här åtgärden på `api-version` har angetts som en frågeparameter.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-1012">Note: For this operation, the `api-version` is specified as a query parameter.</span></span>

<span data-ttu-id="1a5b1-1013">**Huvuden för begäran**</span><span class="sxs-lookup"><span data-stu-id="1a5b1-1013">**Request Headers**</span></span>

<span data-ttu-id="1a5b1-1014">I följande lista beskrivs de obligatoriska och valfria begärandehuvudena.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-1014">The following list describes the required and optional request headers.</span></span>

* <span data-ttu-id="1a5b1-1015">`api-key`: `api-key` Används för att autentisera begäran om att din söktjänst.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-1015">`api-key`: The `api-key` is used to authenticate the request to your Search service.</span></span> <span data-ttu-id="1a5b1-1016">Det är ett strängvärde som unik för din tjänst-URL.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-1016">It is a string value, unique to your service URL.</span></span> <span data-ttu-id="1a5b1-1017">Den **sökning dokumentet** begäran kan ange en administrationsnyckeln eller Frågenyckeln för `api-key`.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-1017">The **Lookup Document** request can specify either an admin key or query key for `api-key`.</span></span>

<span data-ttu-id="1a5b1-1018">Du måste också tjänstnamnet att konstruera den begärda Webbadressen.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-1018">You will also need the service name to construct the request URL.</span></span> <span data-ttu-id="1a5b1-1019">Du kan hämta namnet på tjänsten och `api-key` från tjänsten instrumentpanelen i Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-1019">You can get the service name and `api-key` from your service dashboard in the Azure Portal.</span></span> <span data-ttu-id="1a5b1-1020">Se [skapa en Azure Search-tjänst i portalen](search-create-service-portal.md) sidan navigering hjälp.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-1020">See [Create an Azure Search service in the portal](search-create-service-portal.md) for page navigation help.</span></span>

<span data-ttu-id="1a5b1-1021">**Begärandetexten**</span><span class="sxs-lookup"><span data-stu-id="1a5b1-1021">**Request Body**</span></span>

<span data-ttu-id="1a5b1-1022">Ingen.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-1022">None.</span></span>

<span data-ttu-id="1a5b1-1023">**Svar**</span><span class="sxs-lookup"><span data-stu-id="1a5b1-1023">**Response**</span></span>

<span data-ttu-id="1a5b1-1024">Statuskod: 200 OK returneras för ett lyckat svar.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-1024">Status Code: 200 OK is returned for a successful response.</span></span>

    {
      field_name: field_value (fields matching the default or specified projection)
    }

<span data-ttu-id="1a5b1-1025">**Exempel**</span><span class="sxs-lookup"><span data-stu-id="1a5b1-1025">**Example**</span></span>

<span data-ttu-id="1a5b1-1026">Sökning dokument har nyckel '2'</span><span class="sxs-lookup"><span data-stu-id="1a5b1-1026">Lookup the document that has key '2'</span></span>

    GET /indexes/hotels/docs/2?api-version=2015-02-28-Preview

<span data-ttu-id="1a5b1-1027">Dokumentet som har nyckeln ”3” med hjälp av syntaxen för OData-sökning:</span><span class="sxs-lookup"><span data-stu-id="1a5b1-1027">Lookup the document that has key '3' using OData syntax:</span></span>

    GET /indexes('hotels')/docs('3')?api-version=2015-02-28-Preview

<a name="CountDocs"></a>

## <a name="count-documents"></a><span data-ttu-id="1a5b1-1028">Antal dokument</span><span class="sxs-lookup"><span data-stu-id="1a5b1-1028">Count Documents</span></span>
<span data-ttu-id="1a5b1-1029">Den **antalet dokument** åtgärden hämtas en uppräkning av antalet dokument i en sökindex.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-1029">The **Count Documents** operation retrieves a count of the number of documents in a search index.</span></span> <span data-ttu-id="1a5b1-1030">Den `$count` syntax är en del av OData-protokollet.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-1030">The `$count` syntax is part of the OData protocol.</span></span>

    GET https://[service name].search.windows.net/indexes/[index name]/docs/$count?api-version=[api-version]
    Accept: text/plain
    api-key: [admin or query key]

<span data-ttu-id="1a5b1-1031">**Förfrågan**</span><span class="sxs-lookup"><span data-stu-id="1a5b1-1031">**Request**</span></span>

<span data-ttu-id="1a5b1-1032">HTTPS krävs för tjänstbegäranden.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-1032">HTTPS is required for service requests.</span></span> <span data-ttu-id="1a5b1-1033">Den **antalet dokument** begäran kan konstrueras med GET-metoden.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-1033">The **Count Documents** request can be constructed using the GET method.</span></span>

<span data-ttu-id="1a5b1-1034">[Index name] i URI-begäran talar om tjänsten för att returnera en uppräkning av alla objekt i samlingen docs för det angivna indexet.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-1034">The [index name] in the request URI tells the service to return a count of all items in the docs collection of the specified index.</span></span>

<span data-ttu-id="1a5b1-1035">`api-version=[string]`(krävs).</span><span class="sxs-lookup"><span data-stu-id="1a5b1-1035">`api-version=[string]` (required).</span></span> <span data-ttu-id="1a5b1-1036">Förhandsversionen är `api-version=2015-02-28-Preview`.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-1036">The preview version is `api-version=2015-02-28-Preview`.</span></span> <span data-ttu-id="1a5b1-1037">Se [Sök Versionhantering](http://msdn.microsoft.com/library/azure/dn864560.aspx) information och alternativa versioner.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-1037">See [Search Service Versioning](http://msdn.microsoft.com/library/azure/dn864560.aspx) for details and alternative versions.</span></span>

<span data-ttu-id="1a5b1-1038">**Huvuden för begäran**</span><span class="sxs-lookup"><span data-stu-id="1a5b1-1038">**Request Headers**</span></span>

<span data-ttu-id="1a5b1-1039">I följande lista beskrivs de obligatoriska och valfria begärandehuvudena.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-1039">The following list describes the required and optional request headers.</span></span>

* <span data-ttu-id="1a5b1-1040">`Accept`: Det här värdet måste anges till `text/plain`.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-1040">`Accept`: This value must be set to `text/plain`.</span></span>
* <span data-ttu-id="1a5b1-1041">`api-key`: `api-key` Används för att autentisera begäran om att din söktjänst.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-1041">`api-key`: The `api-key` is used to authenticate the request to your Search service.</span></span> <span data-ttu-id="1a5b1-1042">Det är ett strängvärde som unik för din tjänst-URL.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-1042">It is a string value, unique to your service URL.</span></span> <span data-ttu-id="1a5b1-1043">Den **antalet dokument** begäran kan ange en administrationsnyckeln eller Frågenyckeln för `api-key`.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-1043">The **Count Documents** request can specify either an admin key or query key for `api-key`.</span></span>

<span data-ttu-id="1a5b1-1044">Du måste också tjänstnamnet att konstruera den begärda Webbadressen.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-1044">You will also need the service name to construct the request URL.</span></span> <span data-ttu-id="1a5b1-1045">Du kan hämta namnet på tjänsten och `api-key` från tjänsten instrumentpanelen i Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-1045">You can get the service name and `api-key` from your service dashboard in the Azure Portal.</span></span> <span data-ttu-id="1a5b1-1046">Se [skapa en Azure Search-tjänst i portalen](search-create-service-portal.md) sidan navigering hjälp.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-1046">See [Create an Azure Search service in the portal](search-create-service-portal.md) for page navigation help.</span></span>

<span data-ttu-id="1a5b1-1047">**Begärandetexten**</span><span class="sxs-lookup"><span data-stu-id="1a5b1-1047">**Request Body**</span></span>

<span data-ttu-id="1a5b1-1048">Ingen.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-1048">None.</span></span>

<span data-ttu-id="1a5b1-1049">**Svar**</span><span class="sxs-lookup"><span data-stu-id="1a5b1-1049">**Response**</span></span>

<span data-ttu-id="1a5b1-1050">Statuskod: 200 OK returneras för ett lyckat svar.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-1050">Status Code: 200 OK is returned for a successful response.</span></span>

<span data-ttu-id="1a5b1-1051">Svarstexten innehåller värdet för antal som ett heltal som är formaterad med oformaterad text.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-1051">The response body contains the count value as an integer formatted in plain text.</span></span>

<a name="Suggestions"></a>

## <a name="suggestions"></a><span data-ttu-id="1a5b1-1052">Förslag</span><span class="sxs-lookup"><span data-stu-id="1a5b1-1052">Suggestions</span></span>
<span data-ttu-id="1a5b1-1053">Den **förslag** åtgärden hämtar förslag baserat på partiell sökning indata.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-1053">The **Suggestions** operation retrieves suggestions based on partial search input.</span></span> <span data-ttu-id="1a5b1-1054">Den används vanligtvis i sökrutorna för att tillhandahålla typ-ahead förslag som användare skriver in söktermer.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-1054">It's typically used in search boxes to provide type-ahead suggestions as users are entering search terms.</span></span>

<span data-ttu-id="1a5b1-1055">Förslag begäranden sträva efter tyder på måldokument så att den föreslagna texten upprepas om flera kandidatdokument matchar samma indata.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-1055">Suggestion requests aim at suggesting target documents, so the suggested text may be repeated if multiple candidate documents match the same search input.</span></span> <span data-ttu-id="1a5b1-1056">Du kan använda `$select` att hämta andra dokumentfält (inklusive dokumentnyckeln) så att du kan se vilka dokumentet är källa för varje förslag.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-1056">You can use `$select` to retrieve other document fields (including the document key) so that you can tell which document is the source for each suggestion.</span></span>

<span data-ttu-id="1a5b1-1057">En **förslag** utfärdas igen som en GET eller POST-begäran.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-1057">A **Suggestions** operation is issued as a GET or POST request.</span></span>

    GET https://[service name].search.windows.net/indexes/[index name]/docs/suggest?[query parameters]
    api-key: [admin or query key]

    POST https://[service name].search.windows.net/indexes/[index name]/docs/suggest?api-version=[api-version]
    Content-Type: application/json
    api-key: [admin or query key]

<span data-ttu-id="1a5b1-1058">**När du ska använda en POST i stället för att hämta**</span><span class="sxs-lookup"><span data-stu-id="1a5b1-1058">**When to use POST instead of GET**</span></span>

<span data-ttu-id="1a5b1-1059">När du använder HTTP GET för att anropa den **förslag** API, du måste vara medveten om att längden på den begärda Webbadressen inte får överskrida 8 KB.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-1059">When you use HTTP GET to call the **Suggestions** API, you need to be aware that the length of the request URL cannot exceed 8 KB.</span></span> <span data-ttu-id="1a5b1-1060">Detta är vanligtvis tillräckligt för de flesta program.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-1060">This is usually enough for most applications.</span></span> <span data-ttu-id="1a5b1-1061">Men genererar vissa program mycket stora frågor, speciellt OData filteruttryck.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-1061">However, some applications produce very large queries, specifically OData filter expressions.</span></span> <span data-ttu-id="1a5b1-1062">För dessa program är med hjälp av HTTP POST ett bättre alternativ eftersom det tillåter filter med större än GET.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-1062">For these applications, using HTTP POST is a better choice because it allows larger filters than GET.</span></span> <span data-ttu-id="1a5b1-1063">Med POST är antalet satser i ett filter begränsande faktorn inte storleken på raw Filtersträngen eftersom begäran storleksgränsen för POST är ungefär 16 MB.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-1063">With POST, the number of clauses in a filter is the limiting factor, not the size of the raw filter string since the request size limit for POST is approximately 16 MB.</span></span>

> [!NOTE]
> <span data-ttu-id="1a5b1-1064">Även om storleksgränsen för POST-begäran är väldigt stor får inte filteruttryck vara godtyckligt komplexa.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-1064">Even though the POST request size limit is very large, filter expressions cannot be arbitrarily complex.</span></span> <span data-ttu-id="1a5b1-1065">Se [OData-uttryckssyntax](https://msdn.microsoft.com/library/dn798921.aspx) för mer information om begränsningar för filter komplexitet.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-1065">See [OData expression syntax](https://msdn.microsoft.com/library/dn798921.aspx) for more information about filter complexity limitations.</span></span>
> 
> 

<span data-ttu-id="1a5b1-1066">**Förfrågan**</span><span class="sxs-lookup"><span data-stu-id="1a5b1-1066">**Request**</span></span>

<span data-ttu-id="1a5b1-1067">HTTPS krävs för tjänstbegäranden.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-1067">HTTPS is required for service requests.</span></span> <span data-ttu-id="1a5b1-1068">Den **förslag** begäran kan konstrueras med metoderna GET eller POST.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-1068">The **Suggestions** request can be constructed using the GET or POST methods.</span></span>

<span data-ttu-id="1a5b1-1069">URI-begäran anger namnet på indexet för frågan.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-1069">The request URI specifies the name of the index to query.</span></span> <span data-ttu-id="1a5b1-1070">Parametrar, till exempel delvis inkommande sökvillkor har angetts i frågesträngen när det gäller GET-begäranden och i begäran brödtext för POST-begäranden.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-1070">Parameters, such as the partially input search term, are specified on the query string in the case of GET requests, and in the request body in the case of POST requests.</span></span>

<span data-ttu-id="1a5b1-1071">Ett bra tips när du skapar GET-begäranden, Kom ihåg att [URL-koda](https://msdn.microsoft.com/library/system.uri.escapedatastring.aspx) särskilda frågeparametrar vid anrop av REST API direkt.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-1071">As a best practice when creating GET requests, remember to [URL-encode](https://msdn.microsoft.com/library/system.uri.escapedatastring.aspx) specific query parameters when calling the REST API directly.</span></span> <span data-ttu-id="1a5b1-1072">För **förslag** åtgärder, detta omfattar:</span><span class="sxs-lookup"><span data-stu-id="1a5b1-1072">For **Suggestions** operations, this includes:</span></span>

* `$filter`
* `highlightPreTag`
* `highlightPostTag`
* `search`

<span data-ttu-id="1a5b1-1073">URL-kodning rekommenderas endast på ovanstående Frågeparametrar.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-1073">URL encoding is only recommended on the above query parameters.</span></span> <span data-ttu-id="1a5b1-1074">Om du av misstag URL-koda hela frågesträngen (allt efter den?), begäranden bryts.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-1074">If you inadvertently URL-encode the entire query string (everything after the ?), requests will break.</span></span>

<span data-ttu-id="1a5b1-1075">Dessutom krävs URL-kodning bara vid anrop av REST API: et direkt med GET.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-1075">Also, URL encoding is only necessary when calling the REST API directly using GET.</span></span> <span data-ttu-id="1a5b1-1076">Ingen URL-kodning krävs när du anropar **förslag** med POST, eller när du använder den [.NET-klientbibliotek](https://msdn.microsoft.com/library/dn951165.aspx), som hanterar URL-kodning för dig.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-1076">No URL encoding is necessary when calling **Suggestions** using POST, or when using the [.NET client library](https://msdn.microsoft.com/library/dn951165.aspx), which handles URL encoding for you.</span></span>

<span data-ttu-id="1a5b1-1077">**Frågeparametrar**</span><span class="sxs-lookup"><span data-stu-id="1a5b1-1077">**Query Parameters**</span></span>

<span data-ttu-id="1a5b1-1078">**Förslag** accepterar flera parametrar som tillhandahåller frågevillkor och även ange sökbeteendet.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-1078">**Suggestions** accepts several parameters that provide query criteria and also specify search behavior.</span></span> <span data-ttu-id="1a5b1-1079">Du anger dessa parametrar i URL: en frågesträng vid anrop av **förslag** via GET och som JSON-egenskaper i frågans brödtext vid anrop av **förslag** via POST.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-1079">You provide these parameters in the URL query string when calling **Suggestions** via GET, and as JSON properties in the request body when calling **Suggestions** via POST.</span></span> <span data-ttu-id="1a5b1-1080">Syntaxen för vissa parametrar skiljer mellan GET och POST.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-1080">The syntax for some parameters is slightly different between GET and POST.</span></span> <span data-ttu-id="1a5b1-1081">Skillnaderna beskrivs i tillämpliga fall nedan:</span><span class="sxs-lookup"><span data-stu-id="1a5b1-1081">These differences are noted as applicable below:</span></span>

<span data-ttu-id="1a5b1-1082">`search=[string]`-text för sökning att använda att föreslå frågor.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-1082">`search=[string]` - the search text to use to suggest queries.</span></span> <span data-ttu-id="1a5b1-1083">Måste vara minst 1 tecken och högst 100 tecken.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-1083">Must be at least 1 character, and no more than 100 characters.</span></span>

<span data-ttu-id="1a5b1-1084">`highlightPreTag=[string]`(valfritt) – en sträng tagg som läggs om du vill söka träffar.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-1084">`highlightPreTag=[string]` (optional) - a string tag that prepends to search hits.</span></span> <span data-ttu-id="1a5b1-1085">Måste anges med `highlightPostTag`.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-1085">Must be set with `highlightPostTag`.</span></span>

> [!NOTE]
> <span data-ttu-id="1a5b1-1086">När du anropar **förslag** använder GET, reserverade tecken i URL-Adressen måste vara procent-kodat (till exempel % 23 i stället för #).</span><span class="sxs-lookup"><span data-stu-id="1a5b1-1086">When calling **Suggestions** using GET, reserved characters in the URL must be percent-encoded (for example, %23 instead of #).</span></span>
> 
> 

<span data-ttu-id="1a5b1-1087">`highlightPostTag=[string]`(valfritt) – en sträng tagg som lägger till om du vill söka träffar.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-1087">`highlightPostTag=[string]` (optional) - a string tag that appends to search hits.</span></span> <span data-ttu-id="1a5b1-1088">Måste anges med `highlightPreTag`.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-1088">Must be set with `highlightPreTag`.</span></span>

> [!NOTE]
> <span data-ttu-id="1a5b1-1089">När du anropar **förslag** använder GET, reserverade tecken i URL-Adressen måste vara procent-kodat (till exempel % 23 i stället för #).</span><span class="sxs-lookup"><span data-stu-id="1a5b1-1089">When calling **Suggestions** using GET, reserved characters in the URL must be percent-encoded (for example, %23 instead of #).</span></span>
> 
> 

<span data-ttu-id="1a5b1-1090">`suggesterName=[string]`-namnet på förslagsställarens som anges i den `suggesters` samling som är en del av indexdefinitionen.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-1090">`suggesterName=[string]` - the name of the suggester as specified in the `suggesters` collection that's part of the index definition.</span></span> <span data-ttu-id="1a5b1-1091">En `suggester` bestämmer vilka fält som genomsöks för föreslagna sökord.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-1091">A `suggester` determines which fields are scanned for suggested query terms.</span></span> <span data-ttu-id="1a5b1-1092">Se [Suggesters](#Suggesters) mer information.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-1092">See [Suggesters](#Suggesters) for details.</span></span>

<span data-ttu-id="1a5b1-1093">`fuzzy=[boolean]`(valfritt, som standard = false)-om värdet är true detta API hittar förslag även om det finns en saknas eller ersatta tecken i söktexten.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-1093">`fuzzy=[boolean]` (optional, default = false) - when set to true this API will find suggestions even if there's a substituted or missing character in the search text.</span></span> <span data-ttu-id="1a5b1-1094">Detta ger en bättre upplevelse i vissa fall gäller det vid en prestandakostnad eftersom fuzzy förslag sökningar är långsammare och förbruka mer resurser.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-1094">While this provides a better experience in some scenarios it comes at a performance cost as fuzzy suggestion searches are slower and consume more resources.</span></span>

<span data-ttu-id="1a5b1-1095">`searchFields=[string]`(valfritt) – lista över kommaavgränsade fältnamn att söka efter den angivna texten.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-1095">`searchFields=[string]` (optional) - the list of comma-separated field names to search for the specified search text.</span></span> <span data-ttu-id="1a5b1-1096">Målfält måste vara aktiverat för förslag.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-1096">Target fields must be enabled for suggestions.</span></span>

<span data-ttu-id="1a5b1-1097">`$top=#`(valfritt, som standard = 5)-antal förslag för att hämta.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-1097">`$top=#` (optional, default = 5) - the number of suggestions to retrieve.</span></span> <span data-ttu-id="1a5b1-1098">Måste vara ett tal mellan 1 och 100.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-1098">Must be a number between 1 and 100.</span></span>

> [!NOTE]
> <span data-ttu-id="1a5b1-1099">När du anropar **förslag** med efter den här parametern heter `top` i stället för `$top`.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-1099">When calling **Suggestions** using POST, this parameter is named `top` instead of `$top`.</span></span>
> 
> 

<span data-ttu-id="1a5b1-1100">`$filter=[string]`(valfritt) - uttryck som filtrerar dokument anses vara förslag.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-1100">`$filter=[string]` (optional) - an expression that filters the documents considered for suggestions.</span></span>

> [!NOTE]
> <span data-ttu-id="1a5b1-1101">När du anropar **förslag** med efter den här parametern heter `filter` i stället för `$filter`.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-1101">When calling **Suggestions** using POST, this parameter is named `filter` instead of `$filter`.</span></span>
> 
> 

<span data-ttu-id="1a5b1-1102">`$orderby=[string]`(valfritt) – en lista över CSV-uttryck för att sortera resultaten efter.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-1102">`$orderby=[string]` (optional) - a list of comma-separated expressions to sort the results by.</span></span> <span data-ttu-id="1a5b1-1103">Varje uttryck kan vara ett fältnamn eller ett anrop till den `geo.distance()` funktion.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-1103">Each expression can be either a field name or a call to the `geo.distance()` function.</span></span> <span data-ttu-id="1a5b1-1104">Varje uttryck kan följas av `asc` anges stigande ordning och `desc` att ange fallande.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-1104">Each expression can be followed by `asc` to indicated ascending, and `desc` to indicate descending.</span></span> <span data-ttu-id="1a5b1-1105">Standardvärdet är stigande ordning.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-1105">The default is ascending order.</span></span> <span data-ttu-id="1a5b1-1106">Det finns en gräns på 32-satser för `$orderby`.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-1106">There is a limit of 32 clauses for `$orderby`.</span></span>

> [!NOTE]
> <span data-ttu-id="1a5b1-1107">När du anropar **förslag** med efter den här parametern heter `orderby` i stället för `$orderby`.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-1107">When calling **Suggestions** using POST, this parameter is named `orderby` instead of `$orderby`.</span></span>
> 
> 

<span data-ttu-id="1a5b1-1108">`$select=[string]`(valfritt) – en lista över kommaavgränsade fält för att hämta.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-1108">`$select=[string]` (optional) - a list of comma-separated fields to retrieve.</span></span> <span data-ttu-id="1a5b1-1109">Om inget returneras endast dokument nyckel och förslag text.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-1109">If unspecified, only the document key and suggestion text is returned.</span></span> <span data-ttu-id="1a5b1-1110">Du kan uttryckligen begära alla fält genom att ange den här parametern `*`.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-1110">You can explicitly request all fields by setting this parameter to `*`.</span></span>

> [!NOTE]
> <span data-ttu-id="1a5b1-1111">När du anropar **förslag** med efter den här parametern heter `select` i stället för `$select`.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-1111">When calling **Suggestions** using POST, this parameter is named `select` instead of `$select`.</span></span>
> 
> 

<span data-ttu-id="1a5b1-1112">`minimumCoverage`(valfritt, 80 som standard) – ett tal mellan 0 och 100 som anger procentandelen av det index som måste omfattas av en fråga förslag för att frågan ska rapporteras som lyckas.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-1112">`minimumCoverage` (optional, defaults to 80) - a number between 0 and 100 indicating the percentage of the index that must be covered by a suggestions query in order for the query to be reported as a success.</span></span> <span data-ttu-id="1a5b1-1113">Som standard minst 80% av indexet måste vara tillgängliga eller `Suggest` returneras HTTP-statuskoden 503.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-1113">By default, at least 80% of the index must be available or `Suggest` will return HTTP status code 503.</span></span> <span data-ttu-id="1a5b1-1114">Om du ställer in `minimumCoverage` och `Suggest` lyckas den ska returnera HTTP 200 och innehålla en `@search.coverage` värde i svaret som anger procentandelen av det index som ingick i frågan.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-1114">If you set `minimumCoverage` and `Suggest` succeeds, it will return HTTP 200 and include a `@search.coverage` value in the response indicating the percentage of the index that was included in the query.</span></span>

> [!NOTE]
> <span data-ttu-id="1a5b1-1115">Ange den här parametern till ett värde som är lägre än 100 kan vara användbart för att söka efter tillgänglighet även för tjänster med bara en replik.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-1115">Setting this parameter to a value lower than 100 can be useful for ensuring search availability even for services with only one replica.</span></span> <span data-ttu-id="1a5b1-1116">Inga matchande förslag är dock garanterat måste finnas i resultaten.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-1116">However, not all matching suggestions are guaranteed to be present in the results.</span></span> <span data-ttu-id="1a5b1-1117">Om återkallning är viktigare för ditt program än tillgänglighet, så rekommenderas inte att sänka `minimumCoverage` nedan standardvärdet 80.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-1117">If recall is more important to your application than availability, then it's best not to lower `minimumCoverage` below its default value of 80.</span></span>
> 
> 

<span data-ttu-id="1a5b1-1118">`api-version=[string]`(krävs).</span><span class="sxs-lookup"><span data-stu-id="1a5b1-1118">`api-version=[string]` (required).</span></span> <span data-ttu-id="1a5b1-1119">Förhandsversionen är `api-version=2015-02-28-Preview`.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-1119">The preview version is `api-version=2015-02-28-Preview`.</span></span> <span data-ttu-id="1a5b1-1120">Se [Sök Versionhantering](http://msdn.microsoft.com/library/azure/dn864560.aspx) information och alternativa versioner.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-1120">See [Search Service Versioning](http://msdn.microsoft.com/library/azure/dn864560.aspx) for details and alternative versions.</span></span>

<span data-ttu-id="1a5b1-1121">Obs: för den här åtgärden den `api-version` har angetts som en frågeparameter i URL: en oavsett om du anropar **förslag** med GET eller POST.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-1121">Note: For this operation, the `api-version` is specified as a query parameter in the URL regardless of whether you call **Suggestions** with GET or POST.</span></span>

<span data-ttu-id="1a5b1-1122">**Huvuden för begäran**</span><span class="sxs-lookup"><span data-stu-id="1a5b1-1122">**Request Headers**</span></span>

<span data-ttu-id="1a5b1-1123">I följande lista beskrivs de obligatoriska och valfria begärandehuvudena</span><span class="sxs-lookup"><span data-stu-id="1a5b1-1123">The following list describes the required and optional request headers</span></span>

* <span data-ttu-id="1a5b1-1124">`api-key`: `api-key` Används för att autentisera begäran om att din söktjänst.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-1124">`api-key`: The `api-key` is used to authenticate the request to your Search service.</span></span> <span data-ttu-id="1a5b1-1125">Det är ett strängvärde som unik för din tjänst-URL.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-1125">It is a string value, unique to your service URL.</span></span> <span data-ttu-id="1a5b1-1126">Den **förslag** begäran kan ange en administrationsnyckeln eller Frågenyckeln som den `api-key`.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-1126">The **Suggestions** request can specify either an admin key or query key as the `api-key`.</span></span>

<span data-ttu-id="1a5b1-1127">Du måste också tjänstnamnet att konstruera den begärda Webbadressen.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-1127">You will also need the service name to construct the request URL.</span></span> <span data-ttu-id="1a5b1-1128">Du kan hämta namnet på tjänsten och `api-key` från tjänsten instrumentpanelen i Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-1128">You can get the service name and `api-key` from your service dashboard in the Azure Portal.</span></span> <span data-ttu-id="1a5b1-1129">Se [skapa en Azure Search-tjänst i portalen](search-create-service-portal.md) sidan navigering hjälp.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-1129">See [Create an Azure Search service in the portal](search-create-service-portal.md) for page navigation help.</span></span>

<span data-ttu-id="1a5b1-1130">**Begärandetexten**</span><span class="sxs-lookup"><span data-stu-id="1a5b1-1130">**Request Body**</span></span>

<span data-ttu-id="1a5b1-1131">För GET: Ingen.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-1131">For GET: None.</span></span>

<span data-ttu-id="1a5b1-1132">För POST:</span><span class="sxs-lookup"><span data-stu-id="1a5b1-1132">For POST:</span></span>

    {
      "filter": "odata_filter_expression",
      "fuzzy": true | false (default),
      "highlightPreTag": "pre_tag",
      "highlightPostTag": "post_tag",
      "minimumCoverage": # (% of index that must be covered to declare query successful; default 80),
      "orderby": "orderby_expression",
      "search": "partial_search_input",
      "searchFields": "field_name_1, field_name_2, ...",
      "select": "field_name_1, field_name_2, ...",
      "suggesterName": "suggester_name",
      "top": # (default 5)
    }

<span data-ttu-id="1a5b1-1133">**Svar**</span><span class="sxs-lookup"><span data-stu-id="1a5b1-1133">**Response**</span></span>

<span data-ttu-id="1a5b1-1134">Statuskod: 200 OK returneras för ett lyckat svar.</span><span class="sxs-lookup"><span data-stu-id="1a5b1-1134">Status Code: 200 OK is returned for a successful response.</span></span>

    {
      "@search.coverage": # (if minimumCoverage was provided in the query),
      "value": [
        {
          "@search.text": "...",
          "<key field>": "..."
        },
        ...
      ]
    }

<span data-ttu-id="1a5b1-1135">Om alternativet projektion används för att hämta de ingår i varje element i matrisen fält:</span><span class="sxs-lookup"><span data-stu-id="1a5b1-1135">If the projection option is used to retrieve fields they are included in each element of the array:</span></span>

    {
      "@search.coverage": # (if minimumCoverage was provided in the query),
      "value": [
        {
          "@search.text": "...",
          "<key field>": "..."
          <other projected data fields>
        },
        ...
      ]
    }

<span data-ttu-id="1a5b1-1136">**Exempel**</span><span class="sxs-lookup"><span data-stu-id="1a5b1-1136">**Example**</span></span>

<span data-ttu-id="1a5b1-1137">Hämta 5 förslag där partiell sökning-indata är 'lux'</span><span class="sxs-lookup"><span data-stu-id="1a5b1-1137">Retrieve 5 suggestions where the partial search input is 'lux'</span></span>

    GET /indexes/hotels/docs/suggest?search=lux&$top=5&suggesterName=sg&api-version=2015-02-28-Preview

    POST /indexes/hotels/docs/suggest?api-version=2015-02-28-Preview
    {
      "search": "lux",
      "top": 5,
      "suggesterName": "sg"
    }
