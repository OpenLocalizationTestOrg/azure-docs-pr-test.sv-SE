---
title: "aaa ”fråga ett index (REST API - Azure Search) | Microsoft Docs ”"
description: "Skapa en sökfråga i Azure search och använda Sök parametrar toofilter och sortera sökresultaten."
services: search
documentationcenter: 
manager: jhubbard
author: ashmaka
ms.assetid: 8b3ca890-2f5f-44b6-a140-6cb676fc2c9c
ms.service: search
ms.devlang: na
ms.workload: search
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.date: 01/12/2017
ms.author: ashmaka
ms.openlocfilehash: 2f12238b8f4b045f536489cfc8766fb68307bbe2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="query-your-azure-search-index-using-hello-rest-api"></a><span data-ttu-id="e4006-103">Fråga ditt Azure Search-index med hello REST API</span><span class="sxs-lookup"><span data-stu-id="e4006-103">Query your Azure Search index using hello REST API</span></span>
> [!div class="op_single_selector"]
>
> * [<span data-ttu-id="e4006-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="e4006-104">Overview</span></span>](search-query-overview.md)
> * [<span data-ttu-id="e4006-105">Portalen</span><span class="sxs-lookup"><span data-stu-id="e4006-105">Portal</span></span>](search-explorer.md)
> * [<span data-ttu-id="e4006-106">.NET</span><span class="sxs-lookup"><span data-stu-id="e4006-106">.NET</span></span>](search-query-dotnet.md)
> * [<span data-ttu-id="e4006-107">REST</span><span class="sxs-lookup"><span data-stu-id="e4006-107">REST</span></span>](search-query-rest-api.md)
>
>

<span data-ttu-id="e4006-108">Den här artikeln visar hur tooquery ett index med hjälp av hello [Azure Search REST API](https://docs.microsoft.com/rest/api/searchservice/).</span><span class="sxs-lookup"><span data-stu-id="e4006-108">This article shows you how tooquery an index using hello [Azure Search REST API](https://docs.microsoft.com/rest/api/searchservice/).</span></span>

<span data-ttu-id="e4006-109">Innan du påbörjar den här genomgången bör du redan ha [skapat ett Azure Search-index](search-what-is-an-index.md) och [fyllt det med data](search-what-is-data-import.md).</span><span class="sxs-lookup"><span data-stu-id="e4006-109">Before beginning this walkthrough, you should already have [created an Azure Search index](search-what-is-an-index.md) and [populated it with data](search-what-is-data-import.md).</span></span> <span data-ttu-id="e4006-110">Mer bakgrundsinformation finns i [Hur fullständig textsökning fungerar i Azure Search](search-lucene-query-architecture.md).</span><span class="sxs-lookup"><span data-stu-id="e4006-110">For background information, see [How full text search works in Azure Search](search-lucene-query-architecture.md).</span></span>

## <a name="identify-your-azure-search-services-query-api-key"></a><span data-ttu-id="e4006-111">Identifiera API-frågenyckeln för Azure Search-tjänsten</span><span class="sxs-lookup"><span data-stu-id="e4006-111">Identify your Azure Search service's query api-key</span></span>
<span data-ttu-id="e4006-112">En viktig del av varje search-åtgärd mot hello Azure Search REST API är hello *api-nyckel* som genererades för hello-tjänsten som du har etablerats.</span><span class="sxs-lookup"><span data-stu-id="e4006-112">A key component of every search operation against hello Azure Search REST API is hello *api-key* that was generated for hello service you provisioned.</span></span> <span data-ttu-id="e4006-113">Med en giltig nyckel upprättar förtroende mellan hello programmet skickar hello begäran och hello-tjänsten som hanterar den fall per begäran.</span><span class="sxs-lookup"><span data-stu-id="e4006-113">Having a valid key establishes trust, on a per request basis, between hello application sending hello request and hello service that handles it.</span></span>

1. <span data-ttu-id="e4006-114">toofind din tjänst api-nycklar, du kan logga in toohello [Azure-portalen](https://portal.azure.com/)</span><span class="sxs-lookup"><span data-stu-id="e4006-114">toofind your service's api-keys, you can sign in toohello [Azure portal](https://portal.azure.com/)</span></span>
2. <span data-ttu-id="e4006-115">Gå tooyour Azure Search service-bladet</span><span class="sxs-lookup"><span data-stu-id="e4006-115">Go tooyour Azure Search service's blade</span></span>
3. <span data-ttu-id="e4006-116">Klicka på ikonen för hello ”nycklar”</span><span class="sxs-lookup"><span data-stu-id="e4006-116">Click hello "Keys" icon</span></span>

<span data-ttu-id="e4006-117">Tjänsten har *administratörsnycklar* och *frågenycklar*.</span><span class="sxs-lookup"><span data-stu-id="e4006-117">Your service has *admin keys* and *query keys*.</span></span>

* <span data-ttu-id="e4006-118">Din primära och sekundära *admin nycklar* bevilja fullständiga rättigheter tooall åtgärder, inklusive hello möjlighet toomanage hello service, skapa och ta bort index, indexerare och datakällor.</span><span class="sxs-lookup"><span data-stu-id="e4006-118">Your primary and secondary *admin keys* grant full rights tooall operations, including hello ability toomanage hello service, create and delete indexes, indexers, and data sources.</span></span> <span data-ttu-id="e4006-119">Det finns två nycklar så att du kan fortsätta toouse hello sekundärnyckeln om du väljer tooregenerate hello primärnyckel och vice versa.</span><span class="sxs-lookup"><span data-stu-id="e4006-119">There are two keys so that you can continue toouse hello secondary key if you decide tooregenerate hello primary key, and vice-versa.</span></span>
* <span data-ttu-id="e4006-120">Din *fråga nycklar* bevilja läsbehörighet tooindexes och dokument och är vanligtvis distribuerade tooclient program som utfärdar search-begäranden.</span><span class="sxs-lookup"><span data-stu-id="e4006-120">Your *query keys* grant read-only access tooindexes and documents, and are typically distributed tooclient applications that issue search requests.</span></span>

<span data-ttu-id="e4006-121">Du kan använda en av din fråga nycklar för hello frågor till ett index är.</span><span class="sxs-lookup"><span data-stu-id="e4006-121">For hello purposes of querying an index, you can use one of your query keys.</span></span> <span data-ttu-id="e4006-122">Admin-nycklar kan också användas för frågor, men du bör använda en fråga nyckel i din programkod eftersom det bättre följer hello [principen om minsta behörighet](https://en.wikipedia.org/wiki/Principle_of_least_privilege).</span><span class="sxs-lookup"><span data-stu-id="e4006-122">Your admin keys can also be used for queries, but you should use a query key in your application code as this better follows hello [Principle of least privilege](https://en.wikipedia.org/wiki/Principle_of_least_privilege).</span></span>

## <a name="formulate-your-query"></a><span data-ttu-id="e4006-123">Formulera frågan</span><span class="sxs-lookup"><span data-stu-id="e4006-123">Formulate your query</span></span>
<span data-ttu-id="e4006-124">Det finns två sätt för[söka ditt index med hello REST API](https://docs.microsoft.com/rest/api/searchservice/Search-Documents).</span><span class="sxs-lookup"><span data-stu-id="e4006-124">There are two ways too[search your index using hello REST API](https://docs.microsoft.com/rest/api/searchservice/Search-Documents).</span></span> <span data-ttu-id="e4006-125">Ett sätt är tooissue en HTTP POST-begäran där din frågeparametrar definieras i ett JSON-objekt i hello begärandetexten.</span><span class="sxs-lookup"><span data-stu-id="e4006-125">One way is tooissue an HTTP POST request where your query parameters are defined in a JSON object in hello request body.</span></span> <span data-ttu-id="e4006-126">hello annat sätt är tooissue en HTTP GET-begäran där din frågeparametrar definieras inom hello URL-begäran.</span><span class="sxs-lookup"><span data-stu-id="e4006-126">hello other way is tooissue an HTTP GET request where your query parameters are defined within hello request URL.</span></span> <span data-ttu-id="e4006-127">POST har flera [mjukas upp gränser](https://docs.microsoft.com/rest/api/searchservice/Search-Documents) på Frågeparametrar än GET hello storlek.</span><span class="sxs-lookup"><span data-stu-id="e4006-127">POST has more [relaxed limits](https://docs.microsoft.com/rest/api/searchservice/Search-Documents) on hello size of query parameters than GET.</span></span> <span data-ttu-id="e4006-128">Av den anledningen rekommenderar vi att du använder POST såvida det inte finns särskilda omständigheter som gör att GET är lämpligare.</span><span class="sxs-lookup"><span data-stu-id="e4006-128">For this reason, we recommend using POST unless you have special circumstances where using GET would be more convenient.</span></span>

<span data-ttu-id="e4006-129">För både POST och GET måste tooprovide din *tjänstnamnet*, *Indexnamnet*, och hello rätt *API-versionen* (hello aktuella API-versionen är `2016-09-01` hello tidpunkt publicera det här dokumentet) i hello URL-begäran.</span><span class="sxs-lookup"><span data-stu-id="e4006-129">For both POST and GET, you need tooprovide your *service name*, *index name*, and hello proper *API version* (hello current API version is `2016-09-01` at hello time of publishing this document) in hello request URL.</span></span> <span data-ttu-id="e4006-130">GET, hello *frågesträng* på hello slutet av hello URL kan du ge frågeparametrar Hej.</span><span class="sxs-lookup"><span data-stu-id="e4006-130">For GET, hello *query string* at hello end of hello URL is where you provide hello query parameters.</span></span> <span data-ttu-id="e4006-131">Nedan följer hello URL-format:</span><span class="sxs-lookup"><span data-stu-id="e4006-131">See below for hello URL format:</span></span>

    https://[service name].search.windows.net/indexes/[index name]/docs?[query string]&api-version=2016-09-01

<span data-ttu-id="e4006-132">hello-format för POST är hello samma, men med endast api-version i hello frågan string-parametrar.</span><span class="sxs-lookup"><span data-stu-id="e4006-132">hello format for POST is hello same, but with only api-version in hello query string parameters.</span></span>

#### <a name="example-queries"></a><span data-ttu-id="e4006-133">Exempelfrågor</span><span class="sxs-lookup"><span data-stu-id="e4006-133">Example Queries</span></span>
<span data-ttu-id="e4006-134">Här är några exempelfrågor för ett index med namnet ”hotels”.</span><span class="sxs-lookup"><span data-stu-id="e4006-134">Here are a few example queries on an index named "hotels".</span></span> <span data-ttu-id="e4006-135">Frågorna visas i både GET- och POST-format.</span><span class="sxs-lookup"><span data-stu-id="e4006-135">These queries are shown in both GET and POST format.</span></span>

<span data-ttu-id="e4006-136">Sökas hello termen ”budget' hello hela indexet och returnerar bara hello `hotelName` fält:</span><span class="sxs-lookup"><span data-stu-id="e4006-136">Search hello entire index for hello term 'budget' and return only hello `hotelName` field:</span></span>

```
GET https://[service name].search.windows.net/indexes/hotels/docs?search=budget&$select=hotelName&api-version=2016-09-01

POST https://[service name].search.windows.net/indexes/hotels/docs/search?api-version=2016-09-01
{
    "search": "budget",
    "select": "hotelName"
}
```

<span data-ttu-id="e4006-137">Tillämpa ett filter toohello index toofind hotell billigare än 150 USD per natt och returnera hello `hotelId` och `description`:</span><span class="sxs-lookup"><span data-stu-id="e4006-137">Apply a filter toohello index toofind hotels cheaper than $150 per night, and return hello `hotelId` and `description`:</span></span>

```
GET https://[service name].search.windows.net/indexes/hotels/docs?search=*&$filter=baseRate lt 150&$select=hotelId,description&api-version=2016-09-01

POST https://[service name].search.windows.net/indexes/hotels/docs/search?api-version=2016-09-01
{
    "search": "*",
    "filter": "baseRate lt 150",
    "select": "hotelId,description"
}
```

<span data-ttu-id="e4006-138">Sök hello hela index, efter ett visst fält (`lastRenovationDate`) i fallande ordning, ta hello två översta resultat och visa endast `hotelName` och `lastRenovationDate`:</span><span class="sxs-lookup"><span data-stu-id="e4006-138">Search hello entire index, order by a specific field (`lastRenovationDate`) in descending order, take hello top two results, and show only `hotelName` and `lastRenovationDate`:</span></span>

```
GET https://[service name].search.windows.net/indexes/hotels/docs?search=*&$top=2&$orderby=lastRenovationDate desc&$select=hotelName,lastRenovationDate&api-version=2016-09-01

POST https://[service name].search.windows.net/indexes/hotels/docs/search?api-version=2016-09-01
{
    "search": "*",
    "orderby": "lastRenovationDate desc",
    "select": "hotelName,lastRenovationDate",
    "top": 2
}
```

## <a name="submit-your-http-request"></a><span data-ttu-id="e4006-139">Skicka HTTP-begäran</span><span class="sxs-lookup"><span data-stu-id="e4006-139">Submit your HTTP request</span></span>
<span data-ttu-id="e4006-140">Nu när du har formulerat frågan som en del av HTTP-begärans URL (för GET) eller brödtext (för POST) kan du definiera begärandehuvudena och skicka din fråga.</span><span class="sxs-lookup"><span data-stu-id="e4006-140">Now that you have formulated your query as part of your HTTP request URL (for GET) or body (for POST), you can define your request headers and submit your query.</span></span>

#### <a name="request-and-request-headers"></a><span data-ttu-id="e4006-141">Begäran och begärandehuvuden</span><span class="sxs-lookup"><span data-stu-id="e4006-141">Request and Request Headers</span></span>
<span data-ttu-id="e4006-142">Du måste definiera två begärandehuvuden för GET, eller tre för POST:</span><span class="sxs-lookup"><span data-stu-id="e4006-142">You must define two request headers for GET, or three for POST:</span></span>

1. <span data-ttu-id="e4006-143">Hej `api-key` huvud måste anges toohello Frågenyckeln du hittade i steg I ovan.</span><span class="sxs-lookup"><span data-stu-id="e4006-143">hello `api-key` header must be set toohello query key you found in step I above.</span></span> <span data-ttu-id="e4006-144">Du kan också använda en administrationsnyckel som hello `api-key` huvud, men det rekommenderas att du använder en nyckel för frågan som uteslutande beviljas läsbehörighet tooindexes och dokument.</span><span class="sxs-lookup"><span data-stu-id="e4006-144">You can also use an admin key as hello `api-key` header, but it is recommended that you use a query key as it exclusively grants read-only access tooindexes and documents.</span></span>
2. <span data-ttu-id="e4006-145">Hej `Accept` huvud måste anges för`application/json`.</span><span class="sxs-lookup"><span data-stu-id="e4006-145">hello `Accept` header must be set too`application/json`.</span></span>
3. <span data-ttu-id="e4006-146">POST, hello `Content-Type` huvud också ställas in för`application/json`.</span><span class="sxs-lookup"><span data-stu-id="e4006-146">For POST only, hello `Content-Type` header should also be set too`application/json`.</span></span>

<span data-ttu-id="e4006-147">Nedan finns en HTTP GET-begäran toosearch hello ”hotell” index med hello Azure Search REST-API, med hjälp av en enkel fråga som söker efter hello termen ”översikt”:</span><span class="sxs-lookup"><span data-stu-id="e4006-147">See below for an HTTP GET request toosearch hello "hotels" index using hello Azure Search REST API, using a simple query that searches for hello term "motel":</span></span>

```
GET https://[service name].search.windows.net/indexes/hotels/docs?search=motel&api-version=2016-09-01
Accept: application/json
api-key: [query key]
```

<span data-ttu-id="e4006-148">Här är hello samma exempelfråga nu med hjälp av HTTP POST:</span><span class="sxs-lookup"><span data-stu-id="e4006-148">Here is hello same example query, this time using HTTP POST:</span></span>

```
POST https://[service name].search.windows.net/indexes/hotels/docs/search?api-version=2016-09-01
Content-Type: application/json
Accept: application/json
api-key: [query key]

{
    "search": "motel"
}
```

<span data-ttu-id="e4006-149">En lyckad fråga resulterar i en statuskod för `200 OK` och hello sökresultaten visas som JSON i hello svarstexten.</span><span class="sxs-lookup"><span data-stu-id="e4006-149">A successful query request will result in a Status Code of `200 OK` and hello search results are returned as JSON in hello response body.</span></span> <span data-ttu-id="e4006-150">Här är vad hello resultat för hello ovan frågan ser ut som, förutsatt att hello ”hotell” index fylls med hello exempeldata i [Import av Data i Azure Search med hello REST API](search-import-data-rest-api.md) (Observera att hello JSON har formaterats för tydlighetens skull).</span><span class="sxs-lookup"><span data-stu-id="e4006-150">Here is what hello results for hello above query look like, assuming hello "hotels" index is populated with hello sample data in [Data Import in Azure Search using hello REST API](search-import-data-rest-api.md) (note that hello JSON has been formatted for clarity).</span></span>

```JSON
{
    "value": [
        {
            "@search.score": 0.59600675,
            "hotelId": "2",
            "baseRate": 79.99,
            "description": "Cheapest hotel in town",
            "description_fr": "Hôtel le moins cher en ville",
            "hotelName": "Roach Motel",
            "category": "Budget",
            "tags":["motel", "budget"],
            "parkingIncluded": true,
            "smokingAllowed": true,
            "lastRenovationDate": "1982-04-28T00:00:00Z",
            "rating": 1,
            "location": {
                "type": "Point",
                "coordinates": [-122.131577, 49.678581],
                "crs": {
                    "type":"name",
                    "properties": {
                        "name": "EPSG:4326"
                    }
                }
            }
        }
    ]
}
```

<span data-ttu-id="e4006-151">toolearn fler besök hello ”” avsnittet av [Sök dokument](https://docs.microsoft.com/rest/api/searchservice/Search-Documents).</span><span class="sxs-lookup"><span data-stu-id="e4006-151">toolearn more, please visit hello "Response" section of [Search Documents](https://docs.microsoft.com/rest/api/searchservice/Search-Documents).</span></span> <span data-ttu-id="e4006-152">Mer information om andra HTTP-statuskoder som kan returneras om det uppstår fel finns i [HTTP-statuskoder (Azure Search)](https://docs.microsoft.com/rest/api/searchservice/HTTP-status-codes).</span><span class="sxs-lookup"><span data-stu-id="e4006-152">For more information on other HTTP status codes that could be returned in case of failure, see [HTTP status codes (Azure Search)](https://docs.microsoft.com/rest/api/searchservice/HTTP-status-codes).</span></span>
