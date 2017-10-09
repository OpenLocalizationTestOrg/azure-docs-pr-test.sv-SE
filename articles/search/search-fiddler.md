---
title: 'aaaHow toouse Fiddler tooevaluate och testa Azure Search REST API: er | Microsoft Docs'
description: "Använd Fiddler för en kod utan metoden tooverifying Azure Search tillgänglighet och testar hello REST API: er."
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
ms.openlocfilehash: 2912e1180717d7b40a1e4f7f7f00daf2cc254f0b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-fiddler-tooevaluate-and-test-azure-search-rest-apis"></a><span data-ttu-id="66939-103">Använd Fiddler tooevaluate och testa Azure Search REST API: er</span><span class="sxs-lookup"><span data-stu-id="66939-103">Use Fiddler tooevaluate and test Azure Search REST APIs</span></span>
> [!div class="op_single_selector"]
>
> * [<span data-ttu-id="66939-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="66939-104">Overview</span></span>](search-query-overview.md)
> * [<span data-ttu-id="66939-105">Sökutforskaren</span><span class="sxs-lookup"><span data-stu-id="66939-105">Search Explorer</span></span>](search-explorer.md)
> * [<span data-ttu-id="66939-106">Fiddler</span><span class="sxs-lookup"><span data-stu-id="66939-106">Fiddler</span></span>](search-fiddler.md)
> * [<span data-ttu-id="66939-107">NET</span><span class="sxs-lookup"><span data-stu-id="66939-107">.NET</span></span>](search-query-dotnet.md)
> * [<span data-ttu-id="66939-108">REST</span><span class="sxs-lookup"><span data-stu-id="66939-108">REST</span></span>](search-query-rest-api.md)
>
>

<span data-ttu-id="66939-109">Den här artikeln förklarar hur toouse Fiddler, tillgänglig som en [av Telerik](http://www.telerik.com/fiddler), tooissue HTTP-begäranden tooand Visa svar med hello Azure Search REST-API, utan att behöva toowrite någon kod.</span><span class="sxs-lookup"><span data-stu-id="66939-109">This article explains how toouse Fiddler, available as a [free download from Telerik](http://www.telerik.com/fiddler), tooissue HTTP requests tooand view responses using hello Azure Search REST API, without having toowrite any code.</span></span> <span data-ttu-id="66939-110">Azure Search är en fullständigt hanterad, värd- och molnbaserad söktjänst i Microsoft Azure, som enkelt kan programmeras via .NET och REST-API:er.</span><span class="sxs-lookup"><span data-stu-id="66939-110">Azure Search is fully-managed, hosted cloud search service on Microsoft Azure, easily programmable through .NET and REST APIs.</span></span> <span data-ttu-id="66939-111">hello Azure Search-tjänsten REST API: er som finns dokumenterade i [MSDN](https://msdn.microsoft.com/library/azure/dn798935.aspx).</span><span class="sxs-lookup"><span data-stu-id="66939-111">hello Azure Search service REST APIs are documented on [MSDN](https://msdn.microsoft.com/library/azure/dn798935.aspx).</span></span>

<span data-ttu-id="66939-112">I följande hello, ska du skapa ett index, ladda upp dokument, fråga hello index och sedan frågan hello system information om.</span><span class="sxs-lookup"><span data-stu-id="66939-112">In hello following steps, you'll create an index, upload documents, query hello index, and then query hello system for service information.</span></span>

<span data-ttu-id="66939-113">toocomplete dessa steg, behöver du en Azure Search-tjänst och `api-key`.</span><span class="sxs-lookup"><span data-stu-id="66939-113">toocomplete these steps, you will need an Azure Search service and `api-key`.</span></span> <span data-ttu-id="66939-114">Se [skapa en Azure Search-tjänst i hello portal](search-create-service-portal.md) anvisningar för hur tooget startas.</span><span class="sxs-lookup"><span data-stu-id="66939-114">See [Create an Azure Search service in hello portal](search-create-service-portal.md) for instructions on how tooget started.</span></span>

## <a name="create-an-index"></a><span data-ttu-id="66939-115">Skapa ett index</span><span class="sxs-lookup"><span data-stu-id="66939-115">Create an index</span></span>
1. <span data-ttu-id="66939-116">Starta Fiddler.</span><span class="sxs-lookup"><span data-stu-id="66939-116">Start Fiddler.</span></span> <span data-ttu-id="66939-117">På hello **filen** menyn inaktivera **fånga in trafik** toohide överflödig http-aktivitet som är inte relaterat toohello aktuella aktiviteten.</span><span class="sxs-lookup"><span data-stu-id="66939-117">On hello **File** menu, turn off **Capture Traffic** toohide extraneous HTTP activity that is unrelated toohello current task.</span></span>
2. <span data-ttu-id="66939-118">På hello **Composer** fliken ska du formulerar en begäran som ser ut som följande skärmbild som visar hello.</span><span class="sxs-lookup"><span data-stu-id="66939-118">On hello **Composer** tab, you'll formulate a request that looks like hello following screen shot.</span></span>

      ![][1]
3. <span data-ttu-id="66939-119">Välj **PUT**.</span><span class="sxs-lookup"><span data-stu-id="66939-119">Select **PUT**.</span></span>
4. <span data-ttu-id="66939-120">Ange en URL som anger hello-tjänstens URL, attribut och hello api-versionen.</span><span class="sxs-lookup"><span data-stu-id="66939-120">Enter a URL that specifies hello service URL, request attributes, and hello api-version.</span></span> <span data-ttu-id="66939-121">Några pekare tookeep i åtanke:</span><span class="sxs-lookup"><span data-stu-id="66939-121">A few pointers tookeep in mind:</span></span>

   * <span data-ttu-id="66939-122">Använd HTTPS som hello prefix.</span><span class="sxs-lookup"><span data-stu-id="66939-122">Use HTTPS as hello prefix.</span></span>
   * <span data-ttu-id="66939-123">Attributet för begäran är ”/indexes/hotels”.</span><span class="sxs-lookup"><span data-stu-id="66939-123">Request attribute is "/indexes/hotels".</span></span> <span data-ttu-id="66939-124">Detta talar om sökningen toocreate ett index med namnet 'hotell'.</span><span class="sxs-lookup"><span data-stu-id="66939-124">This tells Search toocreate an index named 'hotels'.</span></span>
   * <span data-ttu-id="66939-125">API-versionen anges i gemener, i följande format: ”?api-version=2016-09-01”.</span><span class="sxs-lookup"><span data-stu-id="66939-125">Api-version is lowercase, specified as "?api-version=2016-09-01".</span></span> <span data-ttu-id="66939-126">API-versioner är viktiga eftersom Azure Search distribuerar uppdateringar regelbundet.</span><span class="sxs-lookup"><span data-stu-id="66939-126">API versions are important because Azure Search deploys updates regularly.</span></span> <span data-ttu-id="66939-127">I sällsynta fall kan en tjänstuppdatering införa en ny ändring toohello API.</span><span class="sxs-lookup"><span data-stu-id="66939-127">On rare occasions, a service update may introduce a breaking change toohello API.</span></span> <span data-ttu-id="66939-128">Av den anledningen kräver Azure Search en API-version i varje begäran så att du har full kontroll över vilken version som används.</span><span class="sxs-lookup"><span data-stu-id="66939-128">For this reason, Azure Search requires an api-version on each request so that you are in full control over which one is used.</span></span>

     <span data-ttu-id="66939-129">hello fullständig URL bör se ut ungefär toohello följande exempel.</span><span class="sxs-lookup"><span data-stu-id="66939-129">hello full URL should look similar toohello following example.</span></span>

             https://my-app.search.windows.net/indexes/hotels?api-version=2016-09-01
5. <span data-ttu-id="66939-130">Ange hello begärandehuvudet ersätter hello värden och api-nyckel med värden som är giltiga för din tjänst.</span><span class="sxs-lookup"><span data-stu-id="66939-130">Specify hello request header, replacing hello host and api-key with values that are valid for your service.</span></span>

         User-Agent: Fiddler
         host: my-app.search.windows.net
         content-type: application/json
         api-key: 1111222233334444
6. <span data-ttu-id="66939-131">Brödtext i begäran, klistra in i hello fält som utgör hello indexdefinitionen.</span><span class="sxs-lookup"><span data-stu-id="66939-131">In Request Body, paste in hello fields that make up hello index definition.</span></span>

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
7. <span data-ttu-id="66939-132">Klicka på **Kör**.</span><span class="sxs-lookup"><span data-stu-id="66939-132">Click **Execute**.</span></span>

<span data-ttu-id="66939-133">Om några sekunder bör du se ett HTTP-201 svar i hello session listan, som anger hello indexet har skapats.</span><span class="sxs-lookup"><span data-stu-id="66939-133">In a few seconds, you should see an HTTP 201 response in hello session list, indicating hello index was created successfully.</span></span>

<span data-ttu-id="66939-134">Om du får HTTP 504 Kontrollera hello URL anger HTTPS.</span><span class="sxs-lookup"><span data-stu-id="66939-134">If you get HTTP 504, verify hello URL specifies HTTPS.</span></span> <span data-ttu-id="66939-135">Om du ser HTTP 400 eller 404 Kontrollera hello begäran brödtext tooverify det har inga kopiera / klistra in-fel.</span><span class="sxs-lookup"><span data-stu-id="66939-135">If you see HTTP 400 or 404, check hello request body tooverify there were no copy-paste errors.</span></span> <span data-ttu-id="66939-136">Ett HTTP 403 tyder vanligtvis på problem med hello api-nyckel (en ogiltig nyckel eller ett syntax problem med hur hello api-nyckel har angetts).</span><span class="sxs-lookup"><span data-stu-id="66939-136">An HTTP 403 typically indicates a problem with hello api-key (either an invalid key or a syntax problem with how hello api-key is specified).</span></span>

## <a name="load-documents"></a><span data-ttu-id="66939-137">Läsa in dokument</span><span class="sxs-lookup"><span data-stu-id="66939-137">Load documents</span></span>
<span data-ttu-id="66939-138">På hello **Composer** fliken begäran toopost dokument kommer att se ut hello följande.</span><span class="sxs-lookup"><span data-stu-id="66939-138">On hello **Composer** tab, your request toopost documents will look like hello following.</span></span> <span data-ttu-id="66939-139">hello innehåller hello begäran hello sökdata för 4 hotell.</span><span class="sxs-lookup"><span data-stu-id="66939-139">hello body of hello request contains hello search data for 4 hotels.</span></span>

   ![][2]

1. <span data-ttu-id="66939-140">Välj **POST**.</span><span class="sxs-lookup"><span data-stu-id="66939-140">Select **POST**.</span></span>
2. <span data-ttu-id="66939-141">Ange en URL som börjar med HTTPS, följt av tjänstens URL, följt av ”/indexes/<'indexname'>/docs/index?api-version=2016-09-01”.</span><span class="sxs-lookup"><span data-stu-id="66939-141">Enter a URL that starts with HTTPS, followed by your service URL, followed by "/indexes/<'indexname'>/docs/index?api-version=2016-09-01".</span></span> <span data-ttu-id="66939-142">hello fullständig URL bör se ut ungefär toohello följande exempel.</span><span class="sxs-lookup"><span data-stu-id="66939-142">hello full URL should look similar toohello following example.</span></span>

         https://my-app.search.windows.net/indexes/hotels/docs/index?api-version=2016-09-01
3. <span data-ttu-id="66939-143">Huvudet i begäran bör vara hello samma som tidigare.</span><span class="sxs-lookup"><span data-stu-id="66939-143">Request Header should be hello same as before.</span></span> <span data-ttu-id="66939-144">Kom ihåg att du har ersatt hello värden och api-nyckel med värden som är giltiga för din tjänst.</span><span class="sxs-lookup"><span data-stu-id="66939-144">Remember that you replaced hello host and api-key with values that are valid for your service.</span></span>

         User-Agent: Fiddler
         host: my-app.search.windows.net
         content-type: application/json
         api-key: 1111222233334444
4. <span data-ttu-id="66939-145">hello brödtext i begäran innehåller fyra dokument toobe tillagda toohello hotell index.</span><span class="sxs-lookup"><span data-stu-id="66939-145">hello Request Body contains four documents toobe added toohello hotels index.</span></span>

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
             "description": "This could be hello one",
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
5. <span data-ttu-id="66939-146">Klicka på **Kör**.</span><span class="sxs-lookup"><span data-stu-id="66939-146">Click **Execute**.</span></span>

<span data-ttu-id="66939-147">Du bör se ett 200 HTTP-svar i hello session listan efter några sekunder.</span><span class="sxs-lookup"><span data-stu-id="66939-147">In a few seconds, you should see an HTTP 200 response in hello session list.</span></span> <span data-ttu-id="66939-148">Detta anger hello dokument har skapats.</span><span class="sxs-lookup"><span data-stu-id="66939-148">This indicates hello documents were created successfully.</span></span> <span data-ttu-id="66939-149">Om du får en 207 misslyckades tooupload minst ett dokument.</span><span class="sxs-lookup"><span data-stu-id="66939-149">If you get a 207, at least one document failed tooupload.</span></span> <span data-ttu-id="66939-150">Om du får ett 404 har ett syntaxfel i hello sidhuvud eller hello begäran om.</span><span class="sxs-lookup"><span data-stu-id="66939-150">If you get a 404, you have a syntax error in either hello header or body of hello request.</span></span>

## <a name="query-hello-index"></a><span data-ttu-id="66939-151">Frågan hello index</span><span class="sxs-lookup"><span data-stu-id="66939-151">Query hello index</span></span>
<span data-ttu-id="66939-152">Nu när ett index och dokument har lästs in kan du skicka frågor mot dem.</span><span class="sxs-lookup"><span data-stu-id="66939-152">Now that an index and documents are loaded, you can issue queries against them.</span></span>  <span data-ttu-id="66939-153">På hello **Composer** fliken en **hämta** liknande toohello följande skärmdump ser kommandot som frågar din tjänst.</span><span class="sxs-lookup"><span data-stu-id="66939-153">On hello **Composer** tab, a **GET** command that queries your service will look similar toohello following screen shot.</span></span>

   ![][3]

1. <span data-ttu-id="66939-154">Välj **GET**.</span><span class="sxs-lookup"><span data-stu-id="66939-154">Select **GET**.</span></span>
2. <span data-ttu-id="66939-155">Ange en URL som börjar med HTTPS, följt av tjänstens URL, följt av ”/indexes/<'indexname'>/docs?”, följt av frågeparametrar.</span><span class="sxs-lookup"><span data-stu-id="66939-155">Enter a URL that starts with HTTPS, followed by your service URL, followed by "/indexes/<'indexname'>/docs?", followed by query parameters.</span></span> <span data-ttu-id="66939-156">Använd hello följande URL, ersätter hello exempel värdnamn med något som gäller för din tjänst som exempel.</span><span class="sxs-lookup"><span data-stu-id="66939-156">By way of example, use hello following URL, replacing hello sample host name with one that is valid for your service.</span></span>

         https://my-app.search.windows.net/indexes/hotels/docs?search=motel&facet=category&facet=rating,values:1|2|3|4|5&api-version=2016-09-01

   <span data-ttu-id="66939-157">Den här frågan söker efter hello termen ”översikt” och hämtar aspekten kategorier för klassificering.</span><span class="sxs-lookup"><span data-stu-id="66939-157">This query searches on hello term “motel” and retrieves facet categories for ratings.</span></span>
3. <span data-ttu-id="66939-158">Huvudet i begäran bör vara hello samma som tidigare.</span><span class="sxs-lookup"><span data-stu-id="66939-158">Request Header should be hello same as before.</span></span> <span data-ttu-id="66939-159">Kom ihåg att du har ersatt hello värden och api-nyckel med värden som är giltiga för din tjänst.</span><span class="sxs-lookup"><span data-stu-id="66939-159">Remember that you replaced hello host and api-key with values that are valid for your service.</span></span>

         User-Agent: Fiddler
         host: my-app.search.windows.net
         content-type: application/json
         api-key: 1111222233334444

<span data-ttu-id="66939-160">hello svarskoden bör vara 200 och hello svarsutdata bör se ut ungefär toohello följande skärmdump.</span><span class="sxs-lookup"><span data-stu-id="66939-160">hello response code should be 200, and hello response output should look similar toohello following screen shot.</span></span>

   ![][4]

<span data-ttu-id="66939-161">hello följande exempelfråga är från hello [sökindex åtgärden (Azure Search-API)](http://msdn.microsoft.com/library/dn798927.aspx) på MSDN.</span><span class="sxs-lookup"><span data-stu-id="66939-161">hello following example query is from hello [Search Index operation (Azure Search API)](http://msdn.microsoft.com/library/dn798927.aspx) on MSDN.</span></span> <span data-ttu-id="66939-162">Många av hello exempel frågorna i det här avsnittet innehåller blanksteg, vilket inte är tillåtna i Fiddler.</span><span class="sxs-lookup"><span data-stu-id="66939-162">Many of hello example queries in this topic include spaces, which are not allowed in Fiddler.</span></span> <span data-ttu-id="66939-163">Ersätt varje utrymme med en + tecknet innan klistra in hello frågesträng innan du försöker hello fråga i Fiddler.</span><span class="sxs-lookup"><span data-stu-id="66939-163">Replace each space with a + character before pasting in hello query string before attempting hello query in Fiddler.</span></span>

<span data-ttu-id="66939-164">**Innan blankstegen har ersatts:**</span><span class="sxs-lookup"><span data-stu-id="66939-164">**Before spaces are replaced:**</span></span>

        GET /indexes/hotels/docs?search=*&$orderby=lastRenovationDate desc&api-version=2016-09-01

<span data-ttu-id="66939-165">**När blankstegen har ersatts med +:**</span><span class="sxs-lookup"><span data-stu-id="66939-165">**After spaces are replaced with +:**</span></span>

        GET /indexes/hotels/docs?search=*&$orderby=lastRenovationDate+desc&api-version=2016-09-01

## <a name="query-hello-system"></a><span data-ttu-id="66939-166">frågan hello system</span><span class="sxs-lookup"><span data-stu-id="66939-166">Query hello system</span></span>
<span data-ttu-id="66939-167">Du kan också fråga hello system tooget antal och lagring dokumentanvändning.</span><span class="sxs-lookup"><span data-stu-id="66939-167">You can also query hello system tooget document counts and storage consumption.</span></span> <span data-ttu-id="66939-168">På hello **Composer** fliken din begäran kommer att se liknande toohello följande och hello svaret returnerar antalet hello antalet dokument och diskutrymme som används.</span><span class="sxs-lookup"><span data-stu-id="66939-168">On hello **Composer** tab, your request will look similar toohello following, and hello response will return a count for hello number of documents and space used.</span></span>

 ![][5]

1. <span data-ttu-id="66939-169">Välj **GET**.</span><span class="sxs-lookup"><span data-stu-id="66939-169">Select **GET**.</span></span>
2. <span data-ttu-id="66939-170">Ange en URL som innehåller tjänstens URL, följt av ”/indexes/hotels/stats?api-version=2016-09-01”:</span><span class="sxs-lookup"><span data-stu-id="66939-170">Enter a URL that includes your service URL, followed by "/indexes/hotels/stats?api-version=2016-09-01":</span></span>

         https://my-app.search.windows.net/indexes/hotels/stats?api-version=2016-09-01
3. <span data-ttu-id="66939-171">Ange hello begärandehuvudet ersätter hello värden och api-nyckel med värden som är giltiga för din tjänst.</span><span class="sxs-lookup"><span data-stu-id="66939-171">Specify hello request header, replacing hello host and api-key with values that are valid for your service.</span></span>

         User-Agent: Fiddler
         host: my-app.search.windows.net
         content-type: application/json
         api-key: 1111222233334444
4. <span data-ttu-id="66939-172">Lämna hello begärandetexten tomt.</span><span class="sxs-lookup"><span data-stu-id="66939-172">Leave hello request body empty.</span></span>
5. <span data-ttu-id="66939-173">Klicka på **Kör**.</span><span class="sxs-lookup"><span data-stu-id="66939-173">Click **Execute**.</span></span> <span data-ttu-id="66939-174">Du bör se en 200 HTTP-statuskod i hello sessionslista.</span><span class="sxs-lookup"><span data-stu-id="66939-174">You should see an HTTP 200 status code in hello session list.</span></span> <span data-ttu-id="66939-175">Välj hello post för ditt kommando.</span><span class="sxs-lookup"><span data-stu-id="66939-175">Select hello entry posted for your command.</span></span>
6. <span data-ttu-id="66939-176">Klicka på hello **kontrollanter** klickar du på hello **huvuden** fliken och markera sedan hello JSON-format.</span><span class="sxs-lookup"><span data-stu-id="66939-176">Click hello **Inspectors** tab, click hello **Headers** tab, and then select hello JSON format.</span></span> <span data-ttu-id="66939-177">Du bör se hello antal och lagring storlek (i KB).</span><span class="sxs-lookup"><span data-stu-id="66939-177">You should see hello document count and storage size (in KB).</span></span>

## <a name="next-steps"></a><span data-ttu-id="66939-178">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="66939-178">Next steps</span></span>
<span data-ttu-id="66939-179">Se [hantera din söktjänst på Azure](search-manage.md) för en ingen kod metoden toomanaging och använda Azure Search.</span><span class="sxs-lookup"><span data-stu-id="66939-179">See [Manage your Search service on Azure](search-manage.md) for a no-code approach toomanaging and using Azure Search.</span></span>

<!--Image References-->
[1]: ./media/search-fiddler/AzureSearch_Fiddler1_PutIndex.png
[2]: ./media/search-fiddler/AzureSearch_Fiddler2_PostDocs.png
[3]: ./media/search-fiddler/AzureSearch_Fiddler3_Query.png
[4]: ./media/search-fiddler/AzureSearch_Fiddler4_QueryResults.png
[5]: ./media/search-fiddler/AzureSearch_Fiddler5_QueryStats.png
