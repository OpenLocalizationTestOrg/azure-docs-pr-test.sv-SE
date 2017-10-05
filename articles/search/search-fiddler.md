---
title: "Hur du använder och testar REST API:erna för Azure Search med hjälp av Fiddler | Microsoft Docs"
description: "Använd Fiddler för att verifiera Azure Search-tillgänglighet och testa REST-API:erna utan kod."
services: search
documentationcenter: 
author: HeidiSteen
manager: mblythe
editor: 
ms.assetid: 790e5779-c6a3-4a07-9d1e-d6739e6b87d2
ms.service: search
ms.devlang: rest-api
ms.workload: search
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.date: 10/27/2016
ms.author: heidist
ms.openlocfilehash: c38b73fa69bee34ce2434c6274cb017c99ef3c35
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="use-fiddler-to-evaluate-and-test-azure-search-rest-apis"></a><span data-ttu-id="23b6c-103">Utvärdera och testa REST-API:erna för Azure Search med hjälp av Fiddler</span><span class="sxs-lookup"><span data-stu-id="23b6c-103">Use Fiddler to evaluate and test Azure Search REST APIs</span></span>
> [!div class="op_single_selector"]
>
> * [<span data-ttu-id="23b6c-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="23b6c-104">Overview</span></span>](search-query-overview.md)
> * [<span data-ttu-id="23b6c-105">Sökutforskaren</span><span class="sxs-lookup"><span data-stu-id="23b6c-105">Search Explorer</span></span>](search-explorer.md)
> * [<span data-ttu-id="23b6c-106">Fiddler</span><span class="sxs-lookup"><span data-stu-id="23b6c-106">Fiddler</span></span>](search-fiddler.md)
> * [<span data-ttu-id="23b6c-107">NET</span><span class="sxs-lookup"><span data-stu-id="23b6c-107">.NET</span></span>](search-query-dotnet.md)
> * [<span data-ttu-id="23b6c-108">REST</span><span class="sxs-lookup"><span data-stu-id="23b6c-108">REST</span></span>](search-query-rest-api.md)
>
>

<span data-ttu-id="23b6c-109">Den här artikeln beskriver hur du använder Fiddler, som är tillgängligt som en [kostnadsfri nedladdning från Telerik](http://www.telerik.com/fiddler), för att skicka HTTP-förfrågningar till och visa svar med hjälp av REST-API:et för Azure Search, utan att skriva någon kod.</span><span class="sxs-lookup"><span data-stu-id="23b6c-109">This article explains how to use Fiddler, available as a [free download from Telerik](http://www.telerik.com/fiddler), to issue HTTP requests to and view responses using the Azure Search REST API, without having to write any code.</span></span> <span data-ttu-id="23b6c-110">Azure Search är en fullständigt hanterad, värd- och molnbaserad söktjänst i Microsoft Azure, som enkelt kan programmeras via .NET och REST-API:er.</span><span class="sxs-lookup"><span data-stu-id="23b6c-110">Azure Search is fully-managed, hosted cloud search service on Microsoft Azure, easily programmable through .NET and REST APIs.</span></span> <span data-ttu-id="23b6c-111">Azure Search-tjänstens REST API:er finns dokumenterade på [MSDN](https://msdn.microsoft.com/library/azure/dn798935.aspx).</span><span class="sxs-lookup"><span data-stu-id="23b6c-111">The Azure Search service REST APIs are documented on [MSDN](https://msdn.microsoft.com/library/azure/dn798935.aspx).</span></span>

<span data-ttu-id="23b6c-112">I följande steg ska du skapa ett index, ladda upp dokument, fråga indexet och fråga systemet efter tjänstinformation.</span><span class="sxs-lookup"><span data-stu-id="23b6c-112">In the following steps, you'll create an index, upload documents, query the index, and then query the system for service information.</span></span>

<span data-ttu-id="23b6c-113">För att slutföra dessa steg behöver du en Azure Search-tjänst och `api-key`.</span><span class="sxs-lookup"><span data-stu-id="23b6c-113">To complete these steps, you will need an Azure Search service and `api-key`.</span></span> <span data-ttu-id="23b6c-114">Anvisningar för hur du kommer igång finns i [Skapa en Azure Search-tjänst på portalen](search-create-service-portal.md).</span><span class="sxs-lookup"><span data-stu-id="23b6c-114">See [Create an Azure Search service in the portal](search-create-service-portal.md) for instructions on how to get started.</span></span>

## <a name="create-an-index"></a><span data-ttu-id="23b6c-115">Skapa ett index</span><span class="sxs-lookup"><span data-stu-id="23b6c-115">Create an index</span></span>
1. <span data-ttu-id="23b6c-116">Starta Fiddler.</span><span class="sxs-lookup"><span data-stu-id="23b6c-116">Start Fiddler.</span></span> <span data-ttu-id="23b6c-117">Inaktivera **Capture Traffic**på **File**-menyn för att dölja HTTP-aktivitet som inte rör den aktuella aktiviteten.</span><span class="sxs-lookup"><span data-stu-id="23b6c-117">On the **File** menu, turn off **Capture Traffic** to hide extraneous HTTP activity that is unrelated to the current task.</span></span>
2. <span data-ttu-id="23b6c-118">På fliken **Composer** formulerar du en begäran som ser ut som i följande skärmbild.</span><span class="sxs-lookup"><span data-stu-id="23b6c-118">On the **Composer** tab, you'll formulate a request that looks like the following screen shot.</span></span>

      ![][1]
3. <span data-ttu-id="23b6c-119">Välj **PUT**.</span><span class="sxs-lookup"><span data-stu-id="23b6c-119">Select **PUT**.</span></span>
4. <span data-ttu-id="23b6c-120">Ange en URL som anger tjänstens URL, attribut för begäran och API-versionen.</span><span class="sxs-lookup"><span data-stu-id="23b6c-120">Enter a URL that specifies the service URL, request attributes, and the api-version.</span></span> <span data-ttu-id="23b6c-121">Några saker att tänka på:</span><span class="sxs-lookup"><span data-stu-id="23b6c-121">A few pointers to keep in mind:</span></span>

   * <span data-ttu-id="23b6c-122">Använd HTTPS som prefix.</span><span class="sxs-lookup"><span data-stu-id="23b6c-122">Use HTTPS as the prefix.</span></span>
   * <span data-ttu-id="23b6c-123">Attributet för begäran är ”/indexes/hotels”.</span><span class="sxs-lookup"><span data-stu-id="23b6c-123">Request attribute is "/indexes/hotels".</span></span> <span data-ttu-id="23b6c-124">Detta beordrar Search att skapa ett index med namnet ”hotels”.</span><span class="sxs-lookup"><span data-stu-id="23b6c-124">This tells Search to create an index named 'hotels'.</span></span>
   * <span data-ttu-id="23b6c-125">API-versionen anges i gemener, i följande format: ”?api-version=2016-09-01”.</span><span class="sxs-lookup"><span data-stu-id="23b6c-125">Api-version is lowercase, specified as "?api-version=2016-09-01".</span></span> <span data-ttu-id="23b6c-126">API-versioner är viktiga eftersom Azure Search distribuerar uppdateringar regelbundet.</span><span class="sxs-lookup"><span data-stu-id="23b6c-126">API versions are important because Azure Search deploys updates regularly.</span></span> <span data-ttu-id="23b6c-127">I sällsynta fall kan en tjänstuppdatering implementera en viktig ändring i API:et.</span><span class="sxs-lookup"><span data-stu-id="23b6c-127">On rare occasions, a service update may introduce a breaking change to the API.</span></span> <span data-ttu-id="23b6c-128">Av den anledningen kräver Azure Search en API-version i varje begäran så att du har full kontroll över vilken version som används.</span><span class="sxs-lookup"><span data-stu-id="23b6c-128">For this reason, Azure Search requires an api-version on each request so that you are in full control over which one is used.</span></span>

     <span data-ttu-id="23b6c-129">En fullständig URL ser ut som i följande exempel.</span><span class="sxs-lookup"><span data-stu-id="23b6c-129">The full URL should look similar to the following example.</span></span>

             https://my-app.search.windows.net/indexes/hotels?api-version=2016-09-01
5. <span data-ttu-id="23b6c-130">Ange huvudet för begäran och ersätt värden och API-nyckeln med värdena för din tjänst.</span><span class="sxs-lookup"><span data-stu-id="23b6c-130">Specify the request header, replacing the host and api-key with values that are valid for your service.</span></span>

         User-Agent: Fiddler
         host: my-app.search.windows.net
         content-type: application/json
         api-key: 1111222233334444
6. <span data-ttu-id="23b6c-131">I Begärandetext klistrar du in fälten som bildar indexdefinitionen.</span><span class="sxs-lookup"><span data-stu-id="23b6c-131">In Request Body, paste in the fields that make up the index definition.</span></span>

          {
         "name": "hotels",  
         "fields": [
           {"name": "hotelId", "type": "Edm.String", "key":true, "searchable": false},
           {"name": "baseRate", "type": "Edm.Double"},
           {"name": "description", "type": "Edm.String", "filterable": false, "sortable": false, "facetable": false},
           {"name": "hotelName", "type": "Edm.String"},
           {"name": "category", "type": "Edm.String"},
           {"name": "tags", "type": "Collection(Edm.String)"},
           {"name": "parkingIncluded", "type": "Edm.Boolean"},
           {"name": "smokingAllowed", "type": "Edm.Boolean"},
           {"name": "lastRenovationDate", "type": "Edm.DateTimeOffset"},
           {"name": "rating", "type": "Edm.Int32"},
           {"name": "location", "type": "Edm.GeographyPoint"}
          ]
         }
7. <span data-ttu-id="23b6c-132">Klicka på **Execute**.</span><span class="sxs-lookup"><span data-stu-id="23b6c-132">Click **Execute**.</span></span>

<span data-ttu-id="23b6c-133">Efter några sekunder bör du se ett 201 HTTP-svar i sessionslistan som anger att indexet har skapats.</span><span class="sxs-lookup"><span data-stu-id="23b6c-133">In a few seconds, you should see an HTTP 201 response in the session list, indicating the index was created successfully.</span></span>

<span data-ttu-id="23b6c-134">Om HTTP 504 returneras kontrollerar du att HTTPS används i URL:en.</span><span class="sxs-lookup"><span data-stu-id="23b6c-134">If you get HTTP 504, verify the URL specifies HTTPS.</span></span> <span data-ttu-id="23b6c-135">Om HTTP 400 eller 404 returneras kontrollerar du att alla fält har kopierats och klistrats in korrekt i begärandetexten.</span><span class="sxs-lookup"><span data-stu-id="23b6c-135">If you see HTTP 400 or 404, check the request body to verify there were no copy-paste errors.</span></span> <span data-ttu-id="23b6c-136">HTTP 403 tyder vanligen på problem med API-nyckeln (antingen en ogiltig nyckel eller ett syntaxproblem när API-nyckeln angavs).</span><span class="sxs-lookup"><span data-stu-id="23b6c-136">An HTTP 403 typically indicates a problem with the api-key (either an invalid key or a syntax problem with how the api-key is specified).</span></span>

## <a name="load-documents"></a><span data-ttu-id="23b6c-137">Läsa in dokument</span><span class="sxs-lookup"><span data-stu-id="23b6c-137">Load documents</span></span>
<span data-ttu-id="23b6c-138">På fliken **Composer** ser din begäran om att publicera dokument ut så här:</span><span class="sxs-lookup"><span data-stu-id="23b6c-138">On the **Composer** tab, your request to post documents will look like the following.</span></span> <span data-ttu-id="23b6c-139">Begäran innehåller sökdata för fyra hotell.</span><span class="sxs-lookup"><span data-stu-id="23b6c-139">The body of the request contains the search data for 4 hotels.</span></span>

   ![][2]

1. <span data-ttu-id="23b6c-140">Välj **POST**.</span><span class="sxs-lookup"><span data-stu-id="23b6c-140">Select **POST**.</span></span>
2. <span data-ttu-id="23b6c-141">Ange en URL som börjar med HTTPS, följt av tjänstens URL, följt av ”/indexes/<'indexname'>/docs/index?api-version=2016-09-01”.</span><span class="sxs-lookup"><span data-stu-id="23b6c-141">Enter a URL that starts with HTTPS, followed by your service URL, followed by "/indexes/<'indexname'>/docs/index?api-version=2016-09-01".</span></span> <span data-ttu-id="23b6c-142">En fullständig URL ser ut som i följande exempel.</span><span class="sxs-lookup"><span data-stu-id="23b6c-142">The full URL should look similar to the following example.</span></span>

         https://my-app.search.windows.net/indexes/hotels/docs/index?api-version=2016-09-01
3. <span data-ttu-id="23b6c-143">Huvudet i begäran ska vara samma som tidigare.</span><span class="sxs-lookup"><span data-stu-id="23b6c-143">Request Header should be the same as before.</span></span> <span data-ttu-id="23b6c-144">Kom ihåg att du ersatte värden och API-nyckeln med värden för din tjänst.</span><span class="sxs-lookup"><span data-stu-id="23b6c-144">Remember that you replaced the host and api-key with values that are valid for your service.</span></span>

         User-Agent: Fiddler
         host: my-app.search.windows.net
         content-type: application/json
         api-key: 1111222233334444
4. <span data-ttu-id="23b6c-145">Begärandetexten innehåller fyra dokument som ska läggas till i hotellindexet.</span><span class="sxs-lookup"><span data-stu-id="23b6c-145">The Request Body contains four documents to be added to the hotels index.</span></span>

         {
         "value": [
         {
             "@search.action": "upload",
             "hotelId": "1",
             "baseRate": 199.0,
             "description": "Best hotel in town",
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
             "@search.action": "upload",
             "hotelId": "3",
             "baseRate": 279.99,
             "description": "Surprisingly expensive",
             "hotelName": "Dew Drop Inn",
             "category": "Bed and Breakfast",
             "tags": ["charming", "quaint"],
             "parkingIncluded": true,
             "smokingAllowed": false,
             "lastRenovationDate": null,
             "rating": 4,
             "location": { "type": "Point", "coordinates": [-122.33207, 47.60621] }
           },
           {
             "@search.action": "upload",
             "hotelId": "4",
             "baseRate": 220.00,
             "description": "This could be the one",
             "hotelName": "A Hotel for Everyone",
             "category": "Basic hotel",
             "tags": ["pool", "wifi"],
             "parkingIncluded": true,
             "smokingAllowed": false,
             "lastRenovationDate": null,
             "rating": 4,
             "location": { "type": "Point", "coordinates": [-122.12151, 47.67399] }
           }
          ]
         }
5. <span data-ttu-id="23b6c-146">Klicka på **Execute**.</span><span class="sxs-lookup"><span data-stu-id="23b6c-146">Click **Execute**.</span></span>

<span data-ttu-id="23b6c-147">Efter några sekunder bör du se ett 200 HTTP-svar i sessionslistan.</span><span class="sxs-lookup"><span data-stu-id="23b6c-147">In a few seconds, you should see an HTTP 200 response in the session list.</span></span> <span data-ttu-id="23b6c-148">Detta anger att dokumenten har skapats.</span><span class="sxs-lookup"><span data-stu-id="23b6c-148">This indicates the documents were created successfully.</span></span> <span data-ttu-id="23b6c-149">Om ett 207-svar returneras misslyckades uppladdningen av minst ett dokument.</span><span class="sxs-lookup"><span data-stu-id="23b6c-149">If you get a 207, at least one document failed to upload.</span></span> <span data-ttu-id="23b6c-150">Om ett 404-svar returneras beror det på ett syntaxfel i sidhuvudet för begäran eller i begärandetexten.</span><span class="sxs-lookup"><span data-stu-id="23b6c-150">If you get a 404, you have a syntax error in either the header or body of the request.</span></span>

## <a name="query-the-index"></a><span data-ttu-id="23b6c-151">Skicka frågor mot indexet</span><span class="sxs-lookup"><span data-stu-id="23b6c-151">Query the index</span></span>
<span data-ttu-id="23b6c-152">Nu när ett index och dokument har lästs in kan du skicka frågor mot dem.</span><span class="sxs-lookup"><span data-stu-id="23b6c-152">Now that an index and documents are loaded, you can issue queries against them.</span></span>  <span data-ttu-id="23b6c-153">**GET**-kommandot på fliken **Composer**skickar frågor mot din tjänst och ser ut som i följande skärmbild.</span><span class="sxs-lookup"><span data-stu-id="23b6c-153">On the **Composer** tab, a **GET** command that queries your service will look similar to the following screen shot.</span></span>

   ![][3]

1. <span data-ttu-id="23b6c-154">Välj **GET**.</span><span class="sxs-lookup"><span data-stu-id="23b6c-154">Select **GET**.</span></span>
2. <span data-ttu-id="23b6c-155">Ange en URL som börjar med HTTPS, följt av tjänstens URL, följt av ”/indexes/<'indexname'>/docs?”, följt av frågeparametrar.</span><span class="sxs-lookup"><span data-stu-id="23b6c-155">Enter a URL that starts with HTTPS, followed by your service URL, followed by "/indexes/<'indexname'>/docs?", followed by query parameters.</span></span> <span data-ttu-id="23b6c-156">Som ett exempel kan du använda följande URL och ersätta exempelvärdnamnet med ett som är giltigt för din tjänst.</span><span class="sxs-lookup"><span data-stu-id="23b6c-156">By way of example, use the following URL, replacing the sample host name with one that is valid for your service.</span></span>

         https://my-app.search.windows.net/indexes/hotels/docs?search=motel&facet=category&facet=rating,values:1|2|3|4|5&api-version=2016-09-01

   <span data-ttu-id="23b6c-157">Den här frågan söker efter termen ”motel” och hämtar aspektkategorier för betygsättning.</span><span class="sxs-lookup"><span data-stu-id="23b6c-157">This query searches on the term “motel” and retrieves facet categories for ratings.</span></span>
3. <span data-ttu-id="23b6c-158">Huvudet i begäran ska vara samma som tidigare.</span><span class="sxs-lookup"><span data-stu-id="23b6c-158">Request Header should be the same as before.</span></span> <span data-ttu-id="23b6c-159">Kom ihåg att du ersatte värden och API-nyckeln med värden för din tjänst.</span><span class="sxs-lookup"><span data-stu-id="23b6c-159">Remember that you replaced the host and api-key with values that are valid for your service.</span></span>

         User-Agent: Fiddler
         host: my-app.search.windows.net
         content-type: application/json
         api-key: 1111222233334444

<span data-ttu-id="23b6c-160">Svarskoden bör vara 200 och svarsutdata bör se ut som i följande skärmbild.</span><span class="sxs-lookup"><span data-stu-id="23b6c-160">The response code should be 200, and the response output should look similar to the following screen shot.</span></span>

   ![][4]

<span data-ttu-id="23b6c-161">Följande exempelfråga kommer från avsnittet om [sökindexåtgärder (Azure Search-API)](http://msdn.microsoft.com/library/dn798927.aspx) på MSDN.</span><span class="sxs-lookup"><span data-stu-id="23b6c-161">The following example query is from the [Search Index operation (Azure Search API)](http://msdn.microsoft.com/library/dn798927.aspx) on MSDN.</span></span> <span data-ttu-id="23b6c-162">Många av exempelfrågorna i det här avsnittet innehåller blanksteg, som inte är tillåtna i Fiddler.</span><span class="sxs-lookup"><span data-stu-id="23b6c-162">Many of the example queries in this topic include spaces, which are not allowed in Fiddler.</span></span> <span data-ttu-id="23b6c-163">Ersätt varje blanksteg med ett plustecken (+) innan du klistrar in frågesträngen innan du försöker köra frågan i Fiddler.</span><span class="sxs-lookup"><span data-stu-id="23b6c-163">Replace each space with a + character before pasting in the query string before attempting the query in Fiddler.</span></span>

<span data-ttu-id="23b6c-164">**Innan blankstegen har ersatts:**</span><span class="sxs-lookup"><span data-stu-id="23b6c-164">**Before spaces are replaced:**</span></span>

        GET /indexes/hotels/docs?search=*&$orderby=lastRenovationDate desc&api-version=2016-09-01

<span data-ttu-id="23b6c-165">**När blankstegen har ersatts med +:**</span><span class="sxs-lookup"><span data-stu-id="23b6c-165">**After spaces are replaced with +:**</span></span>

        GET /indexes/hotels/docs?search=*&$orderby=lastRenovationDate+desc&api-version=2016-09-01

## <a name="query-the-system"></a><span data-ttu-id="23b6c-166">Fråga systemet</span><span class="sxs-lookup"><span data-stu-id="23b6c-166">Query the system</span></span>
<span data-ttu-id="23b6c-167">Du kan också skicka frågor i systemet för att visa antalet dokument och lagringsanvändningen.</span><span class="sxs-lookup"><span data-stu-id="23b6c-167">You can also query the system to get document counts and storage consumption.</span></span> <span data-ttu-id="23b6c-168">På fliken **Composer** ser din begäran ut ungefär som nedan, och svaret returnerar antalet dokument och mängden förbrukat utrymme.</span><span class="sxs-lookup"><span data-stu-id="23b6c-168">On the **Composer** tab, your request will look similar to the following, and the response will return a count for the number of documents and space used.</span></span>

 ![][5]

1. <span data-ttu-id="23b6c-169">Välj **GET**.</span><span class="sxs-lookup"><span data-stu-id="23b6c-169">Select **GET**.</span></span>
2. <span data-ttu-id="23b6c-170">Ange en URL som innehåller tjänstens URL, följt av ”/indexes/hotels/stats?api-version=2016-09-01”:</span><span class="sxs-lookup"><span data-stu-id="23b6c-170">Enter a URL that includes your service URL, followed by "/indexes/hotels/stats?api-version=2016-09-01":</span></span>

         https://my-app.search.windows.net/indexes/hotels/stats?api-version=2016-09-01
3. <span data-ttu-id="23b6c-171">Ange huvudet för begäran och ersätt värden och API-nyckeln med värdena för din tjänst.</span><span class="sxs-lookup"><span data-stu-id="23b6c-171">Specify the request header, replacing the host and api-key with values that are valid for your service.</span></span>

         User-Agent: Fiddler
         host: my-app.search.windows.net
         content-type: application/json
         api-key: 1111222233334444
4. <span data-ttu-id="23b6c-172">Lämna begärandetexten tom.</span><span class="sxs-lookup"><span data-stu-id="23b6c-172">Leave the request body empty.</span></span>
5. <span data-ttu-id="23b6c-173">Klicka på **Execute**.</span><span class="sxs-lookup"><span data-stu-id="23b6c-173">Click **Execute**.</span></span> <span data-ttu-id="23b6c-174">Du bör se en 200 HTTP-statuskod i sessionslistan.</span><span class="sxs-lookup"><span data-stu-id="23b6c-174">You should see an HTTP 200 status code in the session list.</span></span> <span data-ttu-id="23b6c-175">Välj posten som publicerats för ditt kommando.</span><span class="sxs-lookup"><span data-stu-id="23b6c-175">Select the entry posted for your command.</span></span>
6. <span data-ttu-id="23b6c-176">Klicka på fliken **Inspectors**, klicka på fliken **Headers** och välj sedan JSON-formatet.</span><span class="sxs-lookup"><span data-stu-id="23b6c-176">Click the **Inspectors** tab, click the **Headers** tab, and then select the JSON format.</span></span> <span data-ttu-id="23b6c-177">Du bör se antalet dokument och lagringsstorleken (i kB).</span><span class="sxs-lookup"><span data-stu-id="23b6c-177">You should see the document count and storage size (in KB).</span></span>

## <a name="next-steps"></a><span data-ttu-id="23b6c-178">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="23b6c-178">Next steps</span></span>
<span data-ttu-id="23b6c-179">Avsnittet [Hantera din Search-tjänst i Azure](search-manage.md) innehåller information om hur du hanterar och använder Azure Search utan att skriva någon kod.</span><span class="sxs-lookup"><span data-stu-id="23b6c-179">See [Manage your Search service on Azure](search-manage.md) for a no-code approach to managing and using Azure Search.</span></span>

<!--Image References-->
[1]: ./media/search-fiddler/AzureSearch_Fiddler1_PutIndex.png
[2]: ./media/search-fiddler/AzureSearch_Fiddler2_PostDocs.png
[3]: ./media/search-fiddler/AzureSearch_Fiddler3_Query.png
[4]: ./media/search-fiddler/AzureSearch_Fiddler4_QueryResults.png
[5]: ./media/search-fiddler/AzureSearch_Fiddler5_QueryStats.png
