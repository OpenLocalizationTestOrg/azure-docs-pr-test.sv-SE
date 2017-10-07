---
title: "aaa ”ladda upp data (REST API - Azure Search) | Microsoft Docs ”"
description: "Lär dig hur tooupload data tooan index i Azure Search med hello REST API."
services: search
documentationcenter: 
author: ashmaka
manager: jhubbard
editor: 
tags: 
ms.assetid: 8d0749fb-6e08-4a17-8cd3-1a215138abc6
ms.service: search
ms.devlang: rest-api
ms.workload: search
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.date: 12/08/2016
ms.author: ashmaka
ms.openlocfilehash: 6ba1336012d1f0f6d6d6c933e16aa879afb9b824
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="upload-data-tooazure-search-using-hello-rest-api"></a><span data-ttu-id="b5ca4-103">Överför data tooAzure Sök med hjälp av hello REST API</span><span class="sxs-lookup"><span data-stu-id="b5ca4-103">Upload data tooAzure Search using hello REST API</span></span>
> [!div class="op_single_selector"]
>
> * [<span data-ttu-id="b5ca4-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="b5ca4-104">Overview</span></span>](search-what-is-data-import.md)
> * [<span data-ttu-id="b5ca4-105">NET</span><span class="sxs-lookup"><span data-stu-id="b5ca4-105">.NET</span></span>](search-import-data-dotnet.md)
> * [<span data-ttu-id="b5ca4-106">REST</span><span class="sxs-lookup"><span data-stu-id="b5ca4-106">REST</span></span>](search-import-data-rest-api.md)
>
>

<span data-ttu-id="b5ca4-107">Den här artikeln visar hur toouse hello [Azure Search REST API](https://docs.microsoft.com/rest/api/searchservice/) tooimport data till ett Azure Search-index.</span><span class="sxs-lookup"><span data-stu-id="b5ca4-107">This article will show you how toouse hello [Azure Search REST API](https://docs.microsoft.com/rest/api/searchservice/) tooimport data into an Azure Search index.</span></span>

<span data-ttu-id="b5ca4-108">Innan du påbörjar den här genomgången bör du redan ha [skapat ett Azure Search-index](search-what-is-an-index.md).</span><span class="sxs-lookup"><span data-stu-id="b5ca4-108">Before beginning this walkthrough, you should already have [created an Azure Search index](search-what-is-an-index.md).</span></span>

<span data-ttu-id="b5ca4-109">I toopush dokument till ditt index med hello REST-API, utfärdar en HTTP POST-begäran tooyour indexet URL-slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="b5ca4-109">In order toopush documents into your index using hello REST API, you will issue an HTTP POST request tooyour index's URL endpoint.</span></span> <span data-ttu-id="b5ca4-110">hello brödtext hello HTTP-begäran meddelandetexten är en JSON-objekt som innehåller hello dokument toobe lagts till, ändras eller tas bort.</span><span class="sxs-lookup"><span data-stu-id="b5ca4-110">hello body of hello HTTP request body is a JSON object containing hello documents toobe added, modified, or deleted.</span></span>

## <a name="identify-your-azure-search-services-admin-api-key"></a><span data-ttu-id="b5ca4-111">Identifiera Azure Search-tjänstens API-administratörsnyckel</span><span class="sxs-lookup"><span data-stu-id="b5ca4-111">Identify your Azure Search service's admin api-key</span></span>
<span data-ttu-id="b5ca4-112">Vid utfärdande av HTTP-begäranden mot din tjänst med hjälp av hello REST API *varje* API-begäran måste innehålla hello api-nyckel som har genererats för hello söktjänsten du etablerat.</span><span class="sxs-lookup"><span data-stu-id="b5ca4-112">When issuing HTTP requests against your service using hello REST API, *each* API request must include hello api-key that was generated for hello Search service you provisioned.</span></span> <span data-ttu-id="b5ca4-113">Med en giltig nyckel upprättar förtroende mellan hello programmet skickar hello begäran och hello-tjänsten som hanterar den fall per begäran.</span><span class="sxs-lookup"><span data-stu-id="b5ca4-113">Having a valid key establishes trust, on a per request basis, between hello application sending hello request and hello service that handles it.</span></span>

1. <span data-ttu-id="b5ca4-114">toofind din tjänst api-nycklar, du kan logga in toohello [Azure-portalen](https://portal.azure.com/)</span><span class="sxs-lookup"><span data-stu-id="b5ca4-114">toofind your service's api-keys, you can sign in toohello [Azure portal](https://portal.azure.com/)</span></span>
2. <span data-ttu-id="b5ca4-115">Gå tooyour Azure Search service-bladet</span><span class="sxs-lookup"><span data-stu-id="b5ca4-115">Go tooyour Azure Search service's blade</span></span>
3. <span data-ttu-id="b5ca4-116">Klicka på hello ”nycklar”-ikon</span><span class="sxs-lookup"><span data-stu-id="b5ca4-116">Click on hello "Keys" icon</span></span>

<span data-ttu-id="b5ca4-117">Tjänsten har *administratörsnycklar* och *frågenycklar*.</span><span class="sxs-lookup"><span data-stu-id="b5ca4-117">Your service will have *admin keys* and *query keys*.</span></span>

* <span data-ttu-id="b5ca4-118">Din primära och sekundära *admin nycklar* bevilja fullständiga rättigheter tooall åtgärder, inklusive hello möjlighet toomanage hello service, skapa och ta bort index, indexerare och datakällor.</span><span class="sxs-lookup"><span data-stu-id="b5ca4-118">Your primary and secondary *admin keys* grant full rights tooall operations, including hello ability toomanage hello service, create and delete indexes, indexers, and data sources.</span></span> <span data-ttu-id="b5ca4-119">Det finns två nycklar så att du kan fortsätta toouse hello sekundärnyckeln om du väljer tooregenerate hello primärnyckel och vice versa.</span><span class="sxs-lookup"><span data-stu-id="b5ca4-119">There are two keys so that you can continue toouse hello secondary key if you decide tooregenerate hello primary key, and vice-versa.</span></span>
* <span data-ttu-id="b5ca4-120">Din *fråga nycklar* bevilja läsbehörighet tooindexes och dokument och är vanligtvis distribuerade tooclient program som utfärdar search-begäranden.</span><span class="sxs-lookup"><span data-stu-id="b5ca4-120">Your *query keys* grant read-only access tooindexes and documents, and are typically distributed tooclient applications that issue search requests.</span></span>

<span data-ttu-id="b5ca4-121">För hello importerar data till ett index är kan du använda antingen primär eller sekundär administrationsnyckel.</span><span class="sxs-lookup"><span data-stu-id="b5ca4-121">For hello purposes of importing data into an index, you can use either your primary or secondary admin key.</span></span>

## <a name="decide-which-indexing-action-toouse"></a><span data-ttu-id="b5ca4-122">Bestäm vilka indexering åtgärden toouse</span><span class="sxs-lookup"><span data-stu-id="b5ca4-122">Decide which indexing action toouse</span></span>
<span data-ttu-id="b5ca4-123">När du använder hello REST API utfärdar HTTP POST-begäranden med JSON-begäran organ tooyour Azure Search indexet slutpunkts-URL.</span><span class="sxs-lookup"><span data-stu-id="b5ca4-123">When using hello REST API, you will issue HTTP POST requests with JSON request bodies tooyour Azure Search index's endpoint URL.</span></span> <span data-ttu-id="b5ca4-124">hello JSON-objekt i HTTP-begärandetexten innehåller en JSON-matris med namnet ”värde” som innehåller JSON-objekt som representerar dokument som du vill att tooadd tooyour index, uppdatera eller ta bort.</span><span class="sxs-lookup"><span data-stu-id="b5ca4-124">hello JSON object in your HTTP request body will contain a single JSON array named "value" containing JSON objects representing documents you would like tooadd tooyour index, update, or delete.</span></span>

<span data-ttu-id="b5ca4-125">Varje JSON-objekt i hello ”värde” matrisen representerar en toobe för dokument som indexeras.</span><span class="sxs-lookup"><span data-stu-id="b5ca4-125">Each JSON object in hello "value" array represents a document toobe indexed.</span></span> <span data-ttu-id="b5ca4-126">De här objekten innehåller hello Dokumentnyckel och anger hello önskad indexering åtgärd (överföringen, merge, ta bort och så vidare).</span><span class="sxs-lookup"><span data-stu-id="b5ca4-126">Each of these objects contains hello document's key and specifies hello desired indexing action (upload, merge, delete, etc).</span></span> <span data-ttu-id="b5ca4-127">Beroende på vilka hello under åtgärder som du väljer endast vissa fält måste inkluderas för varje dokument:</span><span class="sxs-lookup"><span data-stu-id="b5ca4-127">Depending on which of hello below actions you choose, only certain fields must be included for each document:</span></span>

| @search.action | <span data-ttu-id="b5ca4-128">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="b5ca4-128">Description</span></span> | <span data-ttu-id="b5ca4-129">Nödvändiga fält för varje dokument</span><span class="sxs-lookup"><span data-stu-id="b5ca4-129">Necessary fields for each document</span></span> | <span data-ttu-id="b5ca4-130">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="b5ca4-130">Notes</span></span> |
| --- | --- | --- | --- |
| `upload` |<span data-ttu-id="b5ca4-131">En `upload` åtgärden är liknande tooan ”upsert” där hello dokumentet infogas om det är nytt och uppdatera/ersättas om den finns.</span><span class="sxs-lookup"><span data-stu-id="b5ca4-131">An `upload` action is similar tooan "upsert" where hello document will be inserted if it is new and updated/replaced if it exists.</span></span> |<span data-ttu-id="b5ca4-132">nyckel plus andra fält som du vill toodefine</span><span class="sxs-lookup"><span data-stu-id="b5ca4-132">key, plus any other fields you wish toodefine</span></span> |<span data-ttu-id="b5ca4-133">När du uppdaterar och ersätta ett befintligt dokument, alla fält som anges i begäran hello har dess fält som har angetts för`null`.</span><span class="sxs-lookup"><span data-stu-id="b5ca4-133">When updating/replacing an existing document, any field that is not specified in hello request will have its field set too`null`.</span></span> <span data-ttu-id="b5ca4-134">Detta inträffar även när hello fältet tidigare var inställt på tooa icke-null-värde.</span><span class="sxs-lookup"><span data-stu-id="b5ca4-134">This occurs even when hello field was previously set tooa non-null value.</span></span> |
| `merge` |<span data-ttu-id="b5ca4-135">Uppdateringar som har ett befintligt dokument med hello angivna fält.</span><span class="sxs-lookup"><span data-stu-id="b5ca4-135">Updates an existing document with hello specified fields.</span></span> <span data-ttu-id="b5ca4-136">Om hello dokumentet inte finns i hello index, misslyckas hello sammanslagning.</span><span class="sxs-lookup"><span data-stu-id="b5ca4-136">If hello document does not exist in hello index, hello merge will fail.</span></span> |<span data-ttu-id="b5ca4-137">nyckel plus andra fält som du vill toodefine</span><span class="sxs-lookup"><span data-stu-id="b5ca4-137">key, plus any other fields you wish toodefine</span></span> |<span data-ttu-id="b5ca4-138">Alla fält som du anger i en merge ersätter hello befintliga fält i hello.</span><span class="sxs-lookup"><span data-stu-id="b5ca4-138">Any field you specify in a merge will replace hello existing field in hello document.</span></span> <span data-ttu-id="b5ca4-139">Detta gäller även fält av typen `Collection(Edm.String)`.</span><span class="sxs-lookup"><span data-stu-id="b5ca4-139">This includes fields of type `Collection(Edm.String)`.</span></span> <span data-ttu-id="b5ca4-140">Till exempel om hello dokumentet innehåller ett fält `tags` med värdet `["budget"]` och du utföra en koppling med värdet `["economy", "pool"]` för `tags`, hello slutliga värdet för hello `tags` fältet kommer att vara `["economy", "pool"]`.</span><span class="sxs-lookup"><span data-stu-id="b5ca4-140">For example, if hello document contains a field `tags` with value `["budget"]` and you execute a merge with value `["economy", "pool"]` for `tags`, hello final value of hello `tags` field will be `["economy", "pool"]`.</span></span> <span data-ttu-id="b5ca4-141">Det blir inte `["budget", "economy", "pool"]`.</span><span class="sxs-lookup"><span data-stu-id="b5ca4-141">It will not be `["budget", "economy", "pool"]`.</span></span> |
| `mergeOrUpload` |<span data-ttu-id="b5ca4-142">Den här åtgärden fungerar som `merge` om ett dokument med hello anges nyckeln redan finns i hello index.</span><span class="sxs-lookup"><span data-stu-id="b5ca4-142">This action behaves like `merge` if a document with hello given key already exists in hello index.</span></span> <span data-ttu-id="b5ca4-143">Om hello dokumentet inte finns, fungerar den som `upload` med ett nytt dokument.</span><span class="sxs-lookup"><span data-stu-id="b5ca4-143">If hello document does not exist, it behaves like `upload` with a new document.</span></span> |<span data-ttu-id="b5ca4-144">nyckel plus andra fält som du vill toodefine</span><span class="sxs-lookup"><span data-stu-id="b5ca4-144">key, plus any other fields you wish toodefine</span></span> |- |
| `delete` |<span data-ttu-id="b5ca4-145">Tar bort hello angivna dokumentet från hello index.</span><span class="sxs-lookup"><span data-stu-id="b5ca4-145">Removes hello specified document from hello index.</span></span> |<span data-ttu-id="b5ca4-146">endast nyckel</span><span class="sxs-lookup"><span data-stu-id="b5ca4-146">key only</span></span> |<span data-ttu-id="b5ca4-147">Alla fält som du anger än hello nyckelfältet kommer att ignoreras.</span><span class="sxs-lookup"><span data-stu-id="b5ca4-147">Any fields you specify other than hello key field will be ignored.</span></span> <span data-ttu-id="b5ca4-148">Om du vill tooremove ett fält från ett dokument, använda `merge` enkelt och i stället ange hello fältet explicit toonull.</span><span class="sxs-lookup"><span data-stu-id="b5ca4-148">If you want tooremove an individual field from a document, use `merge` instead and simply set hello field explicitly toonull.</span></span> |

## <a name="construct-your-http-request-and-request-body"></a><span data-ttu-id="b5ca4-149">Skapa HTTP-begäran och begärandetexten</span><span class="sxs-lookup"><span data-stu-id="b5ca4-149">Construct your HTTP request and request body</span></span>
<span data-ttu-id="b5ca4-150">Nu när du har samlat in hello nödvändiga fältvärden för Indexåtgärder, är du redo tooconstruct hello faktiska HTTP-begäran och JSON begära brödtext tooimport dina data.</span><span class="sxs-lookup"><span data-stu-id="b5ca4-150">Now that you have gathered hello necessary field values for your index actions, you are ready tooconstruct hello actual HTTP request and JSON request body tooimport your data.</span></span>

#### <a name="request-and-request-headers"></a><span data-ttu-id="b5ca4-151">Begäran och begärandehuvuden</span><span class="sxs-lookup"><span data-stu-id="b5ca4-151">Request and Request Headers</span></span>
<span data-ttu-id="b5ca4-152">I hello URL, behöver du tooprovide ditt namn, indexnamn (”hotell” i det här fallet), samt hello rätt API-versionen (hello aktuella API-versionen är `2016-09-01` när hello publicera det här dokumentet).</span><span class="sxs-lookup"><span data-stu-id="b5ca4-152">In hello URL, you will need tooprovide your service name, index name ("hotels" in this case), as well as hello proper API version (hello current API version is `2016-09-01` at hello time of publishing this document).</span></span> <span data-ttu-id="b5ca4-153">Du behöver toodefine hello `Content-Type` och `api-key` begärandehuvuden.</span><span class="sxs-lookup"><span data-stu-id="b5ca4-153">You will need toodefine hello `Content-Type` and `api-key` request headers.</span></span> <span data-ttu-id="b5ca4-154">Använd en av din tjänst admin nycklar för hello senare.</span><span class="sxs-lookup"><span data-stu-id="b5ca4-154">For hello latter, use one of your service's admin keys.</span></span>

    POST https://[search service].search.windows.net/indexes/hotels/docs/index?api-version=2016-09-01
    Content-Type: application/json
    api-key: [admin key]

#### <a name="request-body"></a><span data-ttu-id="b5ca4-155">Begärandetext</span><span class="sxs-lookup"><span data-stu-id="b5ca4-155">Request Body</span></span>
```JSON
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
            "@search.action": "mergeOrUpload",
            "hotelId": "3",
            "baseRate": 129.99,
            "description": "Close tootown hall and hello river"
        },
        {
            "@search.action": "delete",
            "hotelId": "6"
        }
    ]
}
```

<span data-ttu-id="b5ca4-156">I detta fall använder vi `upload`, `mergeOrUpload` och `delete` som våra sökåtgärder.</span><span class="sxs-lookup"><span data-stu-id="b5ca4-156">In this case, we are using `upload`, `mergeOrUpload`, and `delete` as our search actions.</span></span>

<span data-ttu-id="b5ca4-157">Anta att exempelindexet ”hotels” redan fyllts med ett antal dokument.</span><span class="sxs-lookup"><span data-stu-id="b5ca4-157">Assume that this example "hotels" index is already populated with a number of documents.</span></span> <span data-ttu-id="b5ca4-158">Observera hur vi hade inte toospecify alla fält för hello möjliga dokument när du använder `mergeOrUpload` och hur vi bara ange hello Dokumentnyckel (`hotelId`) när du använder `delete`.</span><span class="sxs-lookup"><span data-stu-id="b5ca4-158">Note how we did not have toospecify all hello possible document fields when using `mergeOrUpload` and how we only specified hello document key (`hotelId`) when using `delete`.</span></span>

<span data-ttu-id="b5ca4-159">Dessutom Observera att du får bara innehålla too1000 dokument (eller 16 MB) i en enskild indexering begäran.</span><span class="sxs-lookup"><span data-stu-id="b5ca4-159">Also, note that you can only include up too1000 documents (or 16 MB) in a single indexing request.</span></span>

## <a name="understand-your-http-response-code"></a><span data-ttu-id="b5ca4-160">Förstå HTTP-svarskoden</span><span class="sxs-lookup"><span data-stu-id="b5ca4-160">Understand your HTTP response code</span></span>
#### <a name="200"></a><span data-ttu-id="b5ca4-161">200</span><span class="sxs-lookup"><span data-stu-id="b5ca4-161">200</span></span>
<span data-ttu-id="b5ca4-162">När du har skickat en lyckad indexeringsbegäran får du ett HTTP-svar med statuskoden `200 OK`.</span><span class="sxs-lookup"><span data-stu-id="b5ca4-162">After submitting a successful indexing request you will receive an HTTP response with status code of `200 OK`.</span></span> <span data-ttu-id="b5ca4-163">hello JSON-meddelandetext av hello HTTP-svar är följande:</span><span class="sxs-lookup"><span data-stu-id="b5ca4-163">hello JSON body of hello HTTP response will be as follows:</span></span>

```JSON
{
    "value": [
        {
            "key": "unique_key_of_document",
            "status": true,
            "errorMessage": null
        },
        ...
    ]
}
```

#### <a name="207"></a><span data-ttu-id="b5ca4-164">207</span><span class="sxs-lookup"><span data-stu-id="b5ca4-164">207</span></span>
<span data-ttu-id="b5ca4-165">Statuskoden `207` returneras om minst ett objekt inte indexerades.</span><span class="sxs-lookup"><span data-stu-id="b5ca4-165">A status code of `207` will be returned when at least one item was not successfully indexed.</span></span> <span data-ttu-id="b5ca4-166">hello JSON-meddelandetext av hello HTTP-svar som innehåller information om misslyckade hello-dokument.</span><span class="sxs-lookup"><span data-stu-id="b5ca4-166">hello JSON body of hello HTTP response will contain information about hello unsuccessful document(s).</span></span>

```JSON
{
    "value": [
        {
            "key": "unique_key_of_document",
            "status": false,
            "errorMessage": "hello search service is too busy tooprocess this document. Please try again later."
        },
        ...
    ]
}
```

> [!NOTE]
> <span data-ttu-id="b5ca4-167">Detta innebär ofta att hello belastningen på din sökning tjänsten når en plats där indexering begäranden börjar tooreturn `503` svar.</span><span class="sxs-lookup"><span data-stu-id="b5ca4-167">This often means that hello load on your search service is reaching a point where indexing requests will begin tooreturn `503` responses.</span></span> <span data-ttu-id="b5ca4-168">I detta fall rekommenderar vi starkt att klientkoden stannar upp och väntar innan ett nytt försök görs.</span><span class="sxs-lookup"><span data-stu-id="b5ca4-168">In this case, we highly recommend that your client code back off and wait before retrying.</span></span> <span data-ttu-id="b5ca4-169">Detta ger hello system vissa tid toorecover, öka hello risken att framtida begäranden ska lyckas.</span><span class="sxs-lookup"><span data-stu-id="b5ca4-169">This will give hello system some time toorecover, increasing hello chances that future requests will succeed.</span></span> <span data-ttu-id="b5ca4-170">Du försöker dina begäranden snabbt kommer bara förlänga hello situation.</span><span class="sxs-lookup"><span data-stu-id="b5ca4-170">Rapidly retrying your requests will only prolong hello situation.</span></span>
>
>

#### <a name="429"></a><span data-ttu-id="b5ca4-171">429</span><span class="sxs-lookup"><span data-stu-id="b5ca4-171">429</span></span>
<span data-ttu-id="b5ca4-172">Statuskoden `429` returneras när du har överskridit kvoten för hello antalet dokument per index.</span><span class="sxs-lookup"><span data-stu-id="b5ca4-172">A status code of `429` will be returned when you have exceeded your quota on hello number of documents per index.</span></span>

#### <a name="503"></a><span data-ttu-id="b5ca4-173">503</span><span class="sxs-lookup"><span data-stu-id="b5ca4-173">503</span></span>
<span data-ttu-id="b5ca4-174">Statuskoden `503` returneras om ingen av hello objekt i hello-begäran har indexerades.</span><span class="sxs-lookup"><span data-stu-id="b5ca4-174">A status code of `503` will be returned if none of hello items in hello request were successfully indexed.</span></span> <span data-ttu-id="b5ca4-175">Felet innebär att hello systemet är hårt belastad och din begäran kan inte bearbetas just nu.</span><span class="sxs-lookup"><span data-stu-id="b5ca4-175">This error means that hello system is under heavy load and your request can't be processed at this time.</span></span>

> [!NOTE]
> <span data-ttu-id="b5ca4-176">I detta fall rekommenderar vi starkt att klientkoden stannar upp och väntar innan ett nytt försök görs.</span><span class="sxs-lookup"><span data-stu-id="b5ca4-176">In this case, we highly recommend that your client code back off and wait before retrying.</span></span> <span data-ttu-id="b5ca4-177">Detta ger hello system vissa tid toorecover, öka hello risken att framtida begäranden ska lyckas.</span><span class="sxs-lookup"><span data-stu-id="b5ca4-177">This will give hello system some time toorecover, increasing hello chances that future requests will succeed.</span></span> <span data-ttu-id="b5ca4-178">Du försöker dina begäranden snabbt kommer bara förlänga hello situation.</span><span class="sxs-lookup"><span data-stu-id="b5ca4-178">Rapidly retrying your requests will only prolong hello situation.</span></span>
>
>

<span data-ttu-id="b5ca4-179">Mer information om dokumentåtgärder och svar om lyckade/misslyckade åtgärder finns i [Lägga till, uppdatera eller ta bort dokument](https://docs.microsoft.com/rest/api/searchservice/AddUpdate-or-Delete-Documents).</span><span class="sxs-lookup"><span data-stu-id="b5ca4-179">For more information on document actions and success/error responses, please see [Add, Update, or Delete Documents](https://docs.microsoft.com/rest/api/searchservice/AddUpdate-or-Delete-Documents).</span></span> <span data-ttu-id="b5ca4-180">Mer information om andra HTTP-statuskoder som kan returneras om det uppstår fel finns i [HTTP-statuskoder (Azure Search)](https://docs.microsoft.com/rest/api/searchservice/HTTP-status-codes).</span><span class="sxs-lookup"><span data-stu-id="b5ca4-180">For more information on other HTTP status codes that could be returned in case of failure, see [HTTP status codes (Azure Search)](https://docs.microsoft.com/rest/api/searchservice/HTTP-status-codes).</span></span>

## <a name="next-steps"></a><span data-ttu-id="b5ca4-181">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="b5ca4-181">Next steps</span></span>
<span data-ttu-id="b5ca4-182">Du kommer att redo toostart utfärda frågor toosearch för dokument efter fylla ditt Azure Search-index.</span><span class="sxs-lookup"><span data-stu-id="b5ca4-182">After populating your Azure Search index, you will be ready toostart issuing queries toosearch for documents.</span></span> <span data-ttu-id="b5ca4-183">Mer information finns i [Fråga ditt Azure Search-index](search-query-overview.md).</span><span class="sxs-lookup"><span data-stu-id="b5ca4-183">See [Query Your Azure Search Index](search-query-overview.md) for details.</span></span>
