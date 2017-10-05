---
title: "Fråga ett index (REST-API – Azure Search)| Microsoft Docs"
description: "Skapa en sökfråga i Azure Search och använd sökparametrar för att filtrera och sortera sökresultat."
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
ms.openlocfilehash: 49062bec233ad35cd457f9665fa94c1855343582
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="query-your-azure-search-index-using-the-rest-api"></a><span data-ttu-id="0c483-103">Skicka frågor mot ditt Azure Search-index med hjälp av REST-API:et</span><span class="sxs-lookup"><span data-stu-id="0c483-103">Query your Azure Search index using the REST API</span></span>
> [!div class="op_single_selector"]
>
> * [<span data-ttu-id="0c483-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="0c483-104">Overview</span></span>](search-query-overview.md)
> * [<span data-ttu-id="0c483-105">Portalen</span><span class="sxs-lookup"><span data-stu-id="0c483-105">Portal</span></span>](search-explorer.md)
> * [<span data-ttu-id="0c483-106">.NET</span><span class="sxs-lookup"><span data-stu-id="0c483-106">.NET</span></span>](search-query-dotnet.md)
> * [<span data-ttu-id="0c483-107">REST</span><span class="sxs-lookup"><span data-stu-id="0c483-107">REST</span></span>](search-query-rest-api.md)
>
>

<span data-ttu-id="0c483-108">Den här artikeln beskriver hur du kör frågor mot ett index med hjälp av [REST-API:et för Azure Search](https://docs.microsoft.com/rest/api/searchservice/).</span><span class="sxs-lookup"><span data-stu-id="0c483-108">This article shows you how to query an index using the [Azure Search REST API](https://docs.microsoft.com/rest/api/searchservice/).</span></span>

<span data-ttu-id="0c483-109">Innan du påbörjar den här genomgången bör du redan ha [skapat ett Azure Search-index](search-what-is-an-index.md) och [fyllt det med data](search-what-is-data-import.md).</span><span class="sxs-lookup"><span data-stu-id="0c483-109">Before beginning this walkthrough, you should already have [created an Azure Search index](search-what-is-an-index.md) and [populated it with data](search-what-is-data-import.md).</span></span> <span data-ttu-id="0c483-110">Mer bakgrundsinformation finns i [Hur fullständig textsökning fungerar i Azure Search](search-lucene-query-architecture.md).</span><span class="sxs-lookup"><span data-stu-id="0c483-110">For background information, see [How full text search works in Azure Search](search-lucene-query-architecture.md).</span></span>

## <a name="identify-your-azure-search-services-query-api-key"></a><span data-ttu-id="0c483-111">Identifiera API-frågenyckeln för Azure Search-tjänsten</span><span class="sxs-lookup"><span data-stu-id="0c483-111">Identify your Azure Search service's query api-key</span></span>
<span data-ttu-id="0c483-112">En viktig del av varje sökåtgärd mot REST-API:et för Azure Search är *API-nyckeln* som genererades för tjänsten som du etablerat.</span><span class="sxs-lookup"><span data-stu-id="0c483-112">A key component of every search operation against the Azure Search REST API is the *api-key* that was generated for the service you provisioned.</span></span> <span data-ttu-id="0c483-113">En giltig nyckel upprättar förtroende, i varje begäran, mellan programmet som skickar begäran och tjänsten som hanterar den.</span><span class="sxs-lookup"><span data-stu-id="0c483-113">Having a valid key establishes trust, on a per request basis, between the application sending the request and the service that handles it.</span></span>

1. <span data-ttu-id="0c483-114">Om du vill hitta din tjänsts API-nycklar kan du logga in på [Azure Portal](https://portal.azure.com/)</span><span class="sxs-lookup"><span data-stu-id="0c483-114">To find your service's api-keys, you can sign in to the [Azure portal](https://portal.azure.com/)</span></span>
2. <span data-ttu-id="0c483-115">Gå till Azure Search-tjänstens blad</span><span class="sxs-lookup"><span data-stu-id="0c483-115">Go to your Azure Search service's blade</span></span>
3. <span data-ttu-id="0c483-116">Klicka på ikonen ”Nycklar”</span><span class="sxs-lookup"><span data-stu-id="0c483-116">Click the "Keys" icon</span></span>

<span data-ttu-id="0c483-117">Tjänsten har *administratörsnycklar* och *frågenycklar*.</span><span class="sxs-lookup"><span data-stu-id="0c483-117">Your service has *admin keys* and *query keys*.</span></span>

* <span data-ttu-id="0c483-118">Dina primära och sekundära *administratörsnycklar* ger fullständig behörighet för alla åtgärder, inklusive möjligheten att hantera tjänsten, skapa och ta bort index, indexerare och datakällor.</span><span class="sxs-lookup"><span data-stu-id="0c483-118">Your primary and secondary *admin keys* grant full rights to all operations, including the ability to manage the service, create and delete indexes, indexers, and data sources.</span></span> <span data-ttu-id="0c483-119">Det finns två nycklar så att du kan fortsätta att använda den sekundära nyckeln om du bestämmer dig för att återskapa den primära nyckeln och tvärtom.</span><span class="sxs-lookup"><span data-stu-id="0c483-119">There are two keys so that you can continue to use the secondary key if you decide to regenerate the primary key, and vice-versa.</span></span>
* <span data-ttu-id="0c483-120">Dina *frågenycklar* beviljar läsbehörighet till index och dokument och distribueras vanligen till klientprogram som skickar sökförfrågningar.</span><span class="sxs-lookup"><span data-stu-id="0c483-120">Your *query keys* grant read-only access to indexes and documents, and are typically distributed to client applications that issue search requests.</span></span>

<span data-ttu-id="0c483-121">Du kan använda en av din frågenycklar för att skicka frågor till ett index.</span><span class="sxs-lookup"><span data-stu-id="0c483-121">For the purposes of querying an index, you can use one of your query keys.</span></span> <span data-ttu-id="0c483-122">Administratörsnycklarna kan även användas för frågor, men du bör använda en frågenyckel i programkoden eftersom detta bättre överensstämmer med [principen om lägsta behörighet](https://en.wikipedia.org/wiki/Principle_of_least_privilege).</span><span class="sxs-lookup"><span data-stu-id="0c483-122">Your admin keys can also be used for queries, but you should use a query key in your application code as this better follows the [Principle of least privilege](https://en.wikipedia.org/wiki/Principle_of_least_privilege).</span></span>

## <a name="formulate-your-query"></a><span data-ttu-id="0c483-123">Formulera frågan</span><span class="sxs-lookup"><span data-stu-id="0c483-123">Formulate your query</span></span>
<span data-ttu-id="0c483-124">Du kan [söka i ditt index med hjälp av REST-API:et](https://docs.microsoft.com/rest/api/searchservice/Search-Documents) på två sätt.</span><span class="sxs-lookup"><span data-stu-id="0c483-124">There are two ways to [search your index using the REST API](https://docs.microsoft.com/rest/api/searchservice/Search-Documents).</span></span> <span data-ttu-id="0c483-125">Ett sätt är att skicka en HTTP POST-begäran där dina frågeparametrar definieras i ett JSON-objekt i begärandetexten.</span><span class="sxs-lookup"><span data-stu-id="0c483-125">One way is to issue an HTTP POST request where your query parameters are defined in a JSON object in the request body.</span></span> <span data-ttu-id="0c483-126">Det andra sättet är att skicka en HTTP GET-begäran där dina frågeparametrar definieras i URL:en för begäran.</span><span class="sxs-lookup"><span data-stu-id="0c483-126">The other way is to issue an HTTP GET request where your query parameters are defined within the request URL.</span></span> <span data-ttu-id="0c483-127">POST har mindre [restriktiva gränser](https://docs.microsoft.com/rest/api/searchservice/Search-Documents) vad gäller frågeparametrarnas storlek än GET.</span><span class="sxs-lookup"><span data-stu-id="0c483-127">POST has more [relaxed limits](https://docs.microsoft.com/rest/api/searchservice/Search-Documents) on the size of query parameters than GET.</span></span> <span data-ttu-id="0c483-128">Av den anledningen rekommenderar vi att du använder POST såvida det inte finns särskilda omständigheter som gör att GET är lämpligare.</span><span class="sxs-lookup"><span data-stu-id="0c483-128">For this reason, we recommend using POST unless you have special circumstances where using GET would be more convenient.</span></span>

<span data-ttu-id="0c483-129">För både POST och GET måste du ange *tjänstnamnet*, *indexnamnet* och *API-versionen* (den aktuella API-versionen är `2016-09-01` vid tidpunkten för publiceringen av det här dokumentet) i URL:en för begäran.</span><span class="sxs-lookup"><span data-stu-id="0c483-129">For both POST and GET, you need to provide your *service name*, *index name*, and the proper *API version* (the current API version is `2016-09-01` at the time of publishing this document) in the request URL.</span></span> <span data-ttu-id="0c483-130">För GET anger du frågeparametrarna i *frågesträngen* i slutet av URL:en.</span><span class="sxs-lookup"><span data-stu-id="0c483-130">For GET, the *query string* at the end of the URL is where you provide the query parameters.</span></span> <span data-ttu-id="0c483-131">Se URL-formatet nedan:</span><span class="sxs-lookup"><span data-stu-id="0c483-131">See below for the URL format:</span></span>

    https://[service name].search.windows.net/indexes/[index name]/docs?[query string]&api-version=2016-09-01

<span data-ttu-id="0c483-132">Formatet för POST är samma, men med endast ”api-version” i frågesträngsparametrarna.</span><span class="sxs-lookup"><span data-stu-id="0c483-132">The format for POST is the same, but with only api-version in the query string parameters.</span></span>

#### <a name="example-queries"></a><span data-ttu-id="0c483-133">Exempelfrågor</span><span class="sxs-lookup"><span data-stu-id="0c483-133">Example Queries</span></span>
<span data-ttu-id="0c483-134">Här är några exempelfrågor för ett index med namnet ”hotels”.</span><span class="sxs-lookup"><span data-stu-id="0c483-134">Here are a few example queries on an index named "hotels".</span></span> <span data-ttu-id="0c483-135">Frågorna visas i både GET- och POST-format.</span><span class="sxs-lookup"><span data-stu-id="0c483-135">These queries are shown in both GET and POST format.</span></span>

<span data-ttu-id="0c483-136">Sök igenom hela indexet efter termen ”budget” och returnera bara `hotelName`-fältet:</span><span class="sxs-lookup"><span data-stu-id="0c483-136">Search the entire index for the term 'budget' and return only the `hotelName` field:</span></span>

```
GET https://[service name].search.windows.net/indexes/hotels/docs?search=budget&$select=hotelName&api-version=2016-09-01

POST https://[service name].search.windows.net/indexes/hotels/docs/search?api-version=2016-09-01
{
    "search": "budget",
    "select": "hotelName"
}
```

<span data-ttu-id="0c483-137">Använd ett filter för indexet för att hitta hotell som är billigare än 150 USD per natt och returnera `hotelId` och `description`:</span><span class="sxs-lookup"><span data-stu-id="0c483-137">Apply a filter to the index to find hotels cheaper than $150 per night, and return the `hotelId` and `description`:</span></span>

```
GET https://[service name].search.windows.net/indexes/hotels/docs?search=*&$filter=baseRate lt 150&$select=hotelId,description&api-version=2016-09-01

POST https://[service name].search.windows.net/indexes/hotels/docs/search?api-version=2016-09-01
{
    "search": "*",
    "filter": "baseRate lt 150",
    "select": "hotelId,description"
}
```

<span data-ttu-id="0c483-138">Sök igenom hela indexet, ordna efter ett visst fält (`lastRenovationDate`) i fallande ordning och visa endast `hotelName` och `lastRenovationDate`:</span><span class="sxs-lookup"><span data-stu-id="0c483-138">Search the entire index, order by a specific field (`lastRenovationDate`) in descending order, take the top two results, and show only `hotelName` and `lastRenovationDate`:</span></span>

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

## <a name="submit-your-http-request"></a><span data-ttu-id="0c483-139">Skicka HTTP-begäran</span><span class="sxs-lookup"><span data-stu-id="0c483-139">Submit your HTTP request</span></span>
<span data-ttu-id="0c483-140">Nu när du har formulerat frågan som en del av HTTP-begärans URL (för GET) eller brödtext (för POST) kan du definiera begärandehuvudena och skicka din fråga.</span><span class="sxs-lookup"><span data-stu-id="0c483-140">Now that you have formulated your query as part of your HTTP request URL (for GET) or body (for POST), you can define your request headers and submit your query.</span></span>

#### <a name="request-and-request-headers"></a><span data-ttu-id="0c483-141">Begäran och begärandehuvuden</span><span class="sxs-lookup"><span data-stu-id="0c483-141">Request and Request Headers</span></span>
<span data-ttu-id="0c483-142">Du måste definiera två begärandehuvuden för GET, eller tre för POST:</span><span class="sxs-lookup"><span data-stu-id="0c483-142">You must define two request headers for GET, or three for POST:</span></span>

1. <span data-ttu-id="0c483-143">`api-key`-huvudet måste anges till frågenyckeln som du identifierade i steg I ovan.</span><span class="sxs-lookup"><span data-stu-id="0c483-143">The `api-key` header must be set to the query key you found in step I above.</span></span> <span data-ttu-id="0c483-144">Du kan även använda en administratörsnyckel som `api-key`-huvud, men vi rekommenderar att du använder en frågenyckel eftersom den ger exklusiv läsbehörighet till index och dokument.</span><span class="sxs-lookup"><span data-stu-id="0c483-144">You can also use an admin key as the `api-key` header, but it is recommended that you use a query key as it exclusively grants read-only access to indexes and documents.</span></span>
2. <span data-ttu-id="0c483-145">`Accept`-huvudet måste anges till `application/json`.</span><span class="sxs-lookup"><span data-stu-id="0c483-145">The `Accept` header must be set to `application/json`.</span></span>
3. <span data-ttu-id="0c483-146">För POST måste `Content-Type`-huvudet också anges till `application/json`.</span><span class="sxs-lookup"><span data-stu-id="0c483-146">For POST only, the `Content-Type` header should also be set to `application/json`.</span></span>

<span data-ttu-id="0c483-147">Nedan illustreras en HTTP GET-begäran för en sökning i ”hotels”-indexet med hjälp av REST-API:et för Azure Search, med en enkel fråga som söker efter termen ”motel”:</span><span class="sxs-lookup"><span data-stu-id="0c483-147">See below for an HTTP GET request to search the "hotels" index using the Azure Search REST API, using a simple query that searches for the term "motel":</span></span>

```
GET https://[service name].search.windows.net/indexes/hotels/docs?search=motel&api-version=2016-09-01
Accept: application/json
api-key: [query key]
```

<span data-ttu-id="0c483-148">Här är samma exempelfråga men med HTTP POST:</span><span class="sxs-lookup"><span data-stu-id="0c483-148">Here is the same example query, this time using HTTP POST:</span></span>

```
POST https://[service name].search.windows.net/indexes/hotels/docs/search?api-version=2016-09-01
Content-Type: application/json
Accept: application/json
api-key: [query key]

{
    "search": "motel"
}
```

<span data-ttu-id="0c483-149">Ett lyckat frågeresultat returnerar statuskoden `200 OK` och sökresultaten returneras som JSON i svarstexten.</span><span class="sxs-lookup"><span data-stu-id="0c483-149">A successful query request will result in a Status Code of `200 OK` and the search results are returned as JSON in the response body.</span></span> <span data-ttu-id="0c483-150">Så här ser resultatet ut för ovanstående fråga, förutsatt att indexet ”hotels” fyllts med exempeldata från [Dataimport i Azure Search med hjälp av REST-API:et](search-import-data-rest-api.md). (Observera att JSON har formaterats för tydlighetens skull.)</span><span class="sxs-lookup"><span data-stu-id="0c483-150">Here is what the results for the above query look like, assuming the "hotels" index is populated with the sample data in [Data Import in Azure Search using the REST API](search-import-data-rest-api.md) (note that the JSON has been formatted for clarity).</span></span>

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

<span data-ttu-id="0c483-151">Om du vill veta mer går du till avsnittet ”Svar” i [Söka efter dokument](https://docs.microsoft.com/rest/api/searchservice/Search-Documents).</span><span class="sxs-lookup"><span data-stu-id="0c483-151">To learn more, please visit the "Response" section of [Search Documents](https://docs.microsoft.com/rest/api/searchservice/Search-Documents).</span></span> <span data-ttu-id="0c483-152">Mer information om andra HTTP-statuskoder som kan returneras om det uppstår fel finns i [HTTP-statuskoder (Azure Search)](https://docs.microsoft.com/rest/api/searchservice/HTTP-status-codes).</span><span class="sxs-lookup"><span data-stu-id="0c483-152">For more information on other HTTP status codes that could be returned in case of failure, see [HTTP status codes (Azure Search)](https://docs.microsoft.com/rest/api/searchservice/HTTP-status-codes).</span></span>
