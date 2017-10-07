---
title: "aaa ”ladda upp data (.NET - Azure Search) | Microsoft Docs ”"
description: "Lär dig hur tooupload data tooan index i Azure Search med hello .NET SDK."
services: search
documentationcenter: 
author: brjohnstmsft
manager: jhubbard
editor: 
tags: 
ms.assetid: 0e0e7e7b-7178-4c26-95c6-2fd1e8015aca
ms.service: search
ms.devlang: dotnet
ms.workload: search
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.date: 01/13/2017
ms.author: brjohnst
ms.openlocfilehash: 78ddbefb522884d1f61cb275c25c091487aee639
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="upload-data-tooazure-search-using-hello-net-sdk"></a><span data-ttu-id="a988a-103">Överför data tooAzure Sök med hjälp av hello .NET SDK</span><span class="sxs-lookup"><span data-stu-id="a988a-103">Upload data tooAzure Search using hello .NET SDK</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="a988a-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="a988a-104">Overview</span></span>](search-what-is-data-import.md)
> * [<span data-ttu-id="a988a-105">NET</span><span class="sxs-lookup"><span data-stu-id="a988a-105">.NET</span></span>](search-import-data-dotnet.md)
> * [<span data-ttu-id="a988a-106">REST</span><span class="sxs-lookup"><span data-stu-id="a988a-106">REST</span></span>](search-import-data-rest-api.md)
> 
> 

<span data-ttu-id="a988a-107">Den här artikeln visar hur toouse hello [Azure Search .NET SDK](https://aka.ms/search-sdk) tooimport data till ett Azure Search-index.</span><span class="sxs-lookup"><span data-stu-id="a988a-107">This article will show you how toouse hello [Azure Search .NET SDK](https://aka.ms/search-sdk) tooimport data into an Azure Search index.</span></span>

<span data-ttu-id="a988a-108">Innan du påbörjar den här genomgången bör du redan ha [skapat ett Azure Search-index](search-what-is-an-index.md).</span><span class="sxs-lookup"><span data-stu-id="a988a-108">Before beginning this walkthrough, you should already have [created an Azure Search index](search-what-is-an-index.md).</span></span> <span data-ttu-id="a988a-109">Den här artikeln förutsätter också att du redan har skapat en `SearchServiceClient` objekt som visas i [skapa ett Azure Search-index med hello .NET SDK](search-create-index-dotnet.md#CreateSearchServiceClient).</span><span class="sxs-lookup"><span data-stu-id="a988a-109">This article also assumes that you have already created a `SearchServiceClient` object, as shown in [Create an Azure Search index using hello .NET SDK](search-create-index-dotnet.md#CreateSearchServiceClient).</span></span>

> [!NOTE]
> <span data-ttu-id="a988a-110">All exempelkod i den här artikeln är skriven i C#.</span><span class="sxs-lookup"><span data-stu-id="a988a-110">All sample code in this article is written in C#.</span></span> <span data-ttu-id="a988a-111">Du kan hitta hello fullständig källkoden [på GitHub](http://aka.ms/search-dotnet-howto).</span><span class="sxs-lookup"><span data-stu-id="a988a-111">You can find hello full source code [on GitHub](http://aka.ms/search-dotnet-howto).</span></span> <span data-ttu-id="a988a-112">Du kan också läsa om hello [Azure Search .NET SDK](search-howto-dotnet-sdk.md) för en mer detaljerad genomgång av hello exempelkod.</span><span class="sxs-lookup"><span data-stu-id="a988a-112">You can also read about hello [Azure Search .NET SDK](search-howto-dotnet-sdk.md) for a more detailed walk through of hello sample code.</span></span>

<span data-ttu-id="a988a-113">I toopush dokument till ditt index med hello .NET SDK, måste du:</span><span class="sxs-lookup"><span data-stu-id="a988a-113">In order toopush documents into your index using hello .NET SDK, you will need to:</span></span>

1. <span data-ttu-id="a988a-114">Skapa en `SearchIndexClient` objektet tooconnect tooyour sökindex.</span><span class="sxs-lookup"><span data-stu-id="a988a-114">Create a `SearchIndexClient` object tooconnect tooyour search index.</span></span>
2. <span data-ttu-id="a988a-115">Skapa en `IndexBatch` som innehåller hello dokument toobe lagts till, ändrats eller tagits bort.</span><span class="sxs-lookup"><span data-stu-id="a988a-115">Create an `IndexBatch` containing hello documents toobe added, modified, or deleted.</span></span>
3. <span data-ttu-id="a988a-116">Anropa hello `Documents.Index` metod för din `SearchIndexClient` toosend hello `IndexBatch` tooyour sökindex.</span><span class="sxs-lookup"><span data-stu-id="a988a-116">Call hello `Documents.Index` method of your `SearchIndexClient` toosend hello `IndexBatch` tooyour search index.</span></span>

## <a name="create-an-instance-of-hello-searchindexclient-class"></a><span data-ttu-id="a988a-117">Skapa en instans av hello SearchIndexClient-klass</span><span class="sxs-lookup"><span data-stu-id="a988a-117">Create an instance of hello SearchIndexClient class</span></span>
<span data-ttu-id="a988a-118">tooimport data till ditt index med hjälp av hello Azure Search .NET SDK måste du toocreate en instans av hello `SearchIndexClient` klass.</span><span class="sxs-lookup"><span data-stu-id="a988a-118">tooimport data into your index using hello Azure Search .NET SDK, you will need toocreate an instance of hello `SearchIndexClient` class.</span></span> <span data-ttu-id="a988a-119">Du kan skapa den här instansen själv, men det är enklare om du redan har en `SearchServiceClient` instans toocall dess `Indexes.GetClient` metod.</span><span class="sxs-lookup"><span data-stu-id="a988a-119">You can construct this instance yourself, but it's easier if you already have a `SearchServiceClient` instance toocall its `Indexes.GetClient` method.</span></span> <span data-ttu-id="a988a-120">Här är exempelvis hur du vill hämta en `SearchIndexClient` för hello index med namnet ”hotell” från en `SearchServiceClient` med namnet `serviceClient`:</span><span class="sxs-lookup"><span data-stu-id="a988a-120">For example, here is how you would obtain a `SearchIndexClient` for hello index named "hotels" from a `SearchServiceClient` named `serviceClient`:</span></span>

```csharp
ISearchIndexClient indexClient = serviceClient.Indexes.GetClient("hotels");
```

> [!NOTE]
> <span data-ttu-id="a988a-121">I ett typiskt sökprogram hanteras indexhanteringen och ifyllningen av en separat komponent från sökfrågorna.</span><span class="sxs-lookup"><span data-stu-id="a988a-121">In a typical search application, index management and population is handled by a separate component from search queries.</span></span> <span data-ttu-id="a988a-122">`Indexes.GetClient`är praktiskt för att fylla ett index eftersom du sparar du hello problem för att ge en annan `SearchCredentials`.</span><span class="sxs-lookup"><span data-stu-id="a988a-122">`Indexes.GetClient` is convenient for populating an index because it saves you hello trouble of providing another `SearchCredentials`.</span></span> <span data-ttu-id="a988a-123">Detta åstadkoms genom att skicka hello admin-nyckel som du använt toocreate hello `SearchServiceClient` toohello nya `SearchIndexClient`.</span><span class="sxs-lookup"><span data-stu-id="a988a-123">It does this by passing hello admin key that you used toocreate hello `SearchServiceClient` toohello new `SearchIndexClient`.</span></span> <span data-ttu-id="a988a-124">Men i hello en del av programmet som frågor körs, är det bättre toocreate hello `SearchIndexClient` direkt så att du kan skicka in en nyckel för frågan i stället för en administrationsnyckel.</span><span class="sxs-lookup"><span data-stu-id="a988a-124">However, in hello part of your application that executes queries, it is better toocreate hello `SearchIndexClient` directly so that you can pass in a query key instead of an admin key.</span></span> <span data-ttu-id="a988a-125">Detta är förenligt med hello [principen om minsta behörighet](https://en.wikipedia.org/wiki/Principle_of_least_privilege) och hjälper toomake programmet säkrare.</span><span class="sxs-lookup"><span data-stu-id="a988a-125">This is consistent with hello [principle of least privilege](https://en.wikipedia.org/wiki/Principle_of_least_privilege) and will help toomake your application more secure.</span></span> <span data-ttu-id="a988a-126">Du hittar mer information om administratör och fråga nycklar i hello [Azure Search REST API-referensen](https://docs.microsoft.com/rest/api/searchservice/).</span><span class="sxs-lookup"><span data-stu-id="a988a-126">You can find out more about admin keys and query keys in hello [Azure Search REST API reference](https://docs.microsoft.com/rest/api/searchservice/).</span></span>
> 
> 

<span data-ttu-id="a988a-127">`SearchIndexClient` har en `Documents`-egenskap.</span><span class="sxs-lookup"><span data-stu-id="a988a-127">`SearchIndexClient` has a `Documents` property.</span></span> <span data-ttu-id="a988a-128">Den här egenskapen anger alla hello-metoder som du behöver tooadd, ändra, ta bort eller fråga dokument i ditt index.</span><span class="sxs-lookup"><span data-stu-id="a988a-128">This property provides all hello methods you need tooadd, modify, delete, or query documents in your index.</span></span>

## <a name="decide-which-indexing-action-toouse"></a><span data-ttu-id="a988a-129">Bestäm vilka indexering åtgärden toouse</span><span class="sxs-lookup"><span data-stu-id="a988a-129">Decide which indexing action toouse</span></span>
<span data-ttu-id="a988a-130">tooimport data med hjälp av hello .NET SDK, behöver du toopackage upp data till ett `IndexBatch` objekt.</span><span class="sxs-lookup"><span data-stu-id="a988a-130">tooimport data using hello .NET SDK, you will need toopackage up your data into an `IndexBatch` object.</span></span> <span data-ttu-id="a988a-131">En `IndexBatch` kapslar in en samling `IndexAction` objekt, som innehåller ett dokument och en egenskap som talar om Azure Search vilken åtgärd tooperform på dokumentet (överföringen, merge, ta bort och så vidare).</span><span class="sxs-lookup"><span data-stu-id="a988a-131">An `IndexBatch` encapsulates a collection of `IndexAction` objects, each of which contains a document and a property that tells Azure Search what action tooperform on that document (upload, merge, delete, etc).</span></span> <span data-ttu-id="a988a-132">Beroende på vilka hello under åtgärder som du väljer endast vissa fält måste inkluderas för varje dokument:</span><span class="sxs-lookup"><span data-stu-id="a988a-132">Depending on which of hello below actions you choose, only certain fields must be included for each document:</span></span>

| <span data-ttu-id="a988a-133">Åtgärd</span><span class="sxs-lookup"><span data-stu-id="a988a-133">Action</span></span> | <span data-ttu-id="a988a-134">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="a988a-134">Description</span></span> | <span data-ttu-id="a988a-135">Nödvändiga fält för varje dokument</span><span class="sxs-lookup"><span data-stu-id="a988a-135">Necessary fields for each document</span></span> | <span data-ttu-id="a988a-136">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="a988a-136">Notes</span></span> |
| --- | --- | --- | --- |
| `Upload` |<span data-ttu-id="a988a-137">En `Upload` åtgärden är liknande tooan ”upsert” där hello dokumentet infogas om det är nytt och uppdatera/ersättas om den finns.</span><span class="sxs-lookup"><span data-stu-id="a988a-137">An `Upload` action is similar tooan "upsert" where hello document will be inserted if it is new and updated/replaced if it exists.</span></span> |<span data-ttu-id="a988a-138">nyckel plus andra fält som du vill toodefine</span><span class="sxs-lookup"><span data-stu-id="a988a-138">key, plus any other fields you wish toodefine</span></span> |<span data-ttu-id="a988a-139">När du uppdaterar och ersätta ett befintligt dokument, alla fält som anges i begäran hello har dess fält som har angetts för`null`.</span><span class="sxs-lookup"><span data-stu-id="a988a-139">When updating/replacing an existing document, any field that is not specified in hello request will have its field set too`null`.</span></span> <span data-ttu-id="a988a-140">Detta inträffar även när hello fältet tidigare var inställt på tooa icke-null-värde.</span><span class="sxs-lookup"><span data-stu-id="a988a-140">This occurs even when hello field was previously set tooa non-null value.</span></span> |
| `Merge` |<span data-ttu-id="a988a-141">Uppdateringar som har ett befintligt dokument med hello angivna fält.</span><span class="sxs-lookup"><span data-stu-id="a988a-141">Updates an existing document with hello specified fields.</span></span> <span data-ttu-id="a988a-142">Om hello dokumentet inte finns i hello index, misslyckas hello sammanslagning.</span><span class="sxs-lookup"><span data-stu-id="a988a-142">If hello document does not exist in hello index, hello merge will fail.</span></span> |<span data-ttu-id="a988a-143">nyckel plus andra fält som du vill toodefine</span><span class="sxs-lookup"><span data-stu-id="a988a-143">key, plus any other fields you wish toodefine</span></span> |<span data-ttu-id="a988a-144">Alla fält som du anger i en merge ersätter hello befintliga fält i hello.</span><span class="sxs-lookup"><span data-stu-id="a988a-144">Any field you specify in a merge will replace hello existing field in hello document.</span></span> <span data-ttu-id="a988a-145">Detta gäller även fält av typen `DataType.Collection(DataType.String)`.</span><span class="sxs-lookup"><span data-stu-id="a988a-145">This includes fields of type `DataType.Collection(DataType.String)`.</span></span> <span data-ttu-id="a988a-146">Till exempel om hello dokumentet innehåller ett fält `tags` med värdet `["budget"]` och du utföra en koppling med värdet `["economy", "pool"]` för `tags`, hello slutliga värdet för hello `tags` fältet kommer att vara `["economy", "pool"]`.</span><span class="sxs-lookup"><span data-stu-id="a988a-146">For example, if hello document contains a field `tags` with value `["budget"]` and you execute a merge with value `["economy", "pool"]` for `tags`, hello final value of hello `tags` field will be `["economy", "pool"]`.</span></span> <span data-ttu-id="a988a-147">Det blir inte `["budget", "economy", "pool"]`.</span><span class="sxs-lookup"><span data-stu-id="a988a-147">It will not be `["budget", "economy", "pool"]`.</span></span> |
| `MergeOrUpload` |<span data-ttu-id="a988a-148">Den här åtgärden fungerar som `Merge` om ett dokument med hello anges nyckeln redan finns i hello index.</span><span class="sxs-lookup"><span data-stu-id="a988a-148">This action behaves like `Merge` if a document with hello given key already exists in hello index.</span></span> <span data-ttu-id="a988a-149">Om hello dokumentet inte finns, fungerar den som `Upload` med ett nytt dokument.</span><span class="sxs-lookup"><span data-stu-id="a988a-149">If hello document does not exist, it behaves like `Upload` with a new document.</span></span> |<span data-ttu-id="a988a-150">nyckel plus andra fält som du vill toodefine</span><span class="sxs-lookup"><span data-stu-id="a988a-150">key, plus any other fields you wish toodefine</span></span> |- |
| `Delete` |<span data-ttu-id="a988a-151">Tar bort hello angivna dokumentet från hello index.</span><span class="sxs-lookup"><span data-stu-id="a988a-151">Removes hello specified document from hello index.</span></span> |<span data-ttu-id="a988a-152">endast nyckel</span><span class="sxs-lookup"><span data-stu-id="a988a-152">key only</span></span> |<span data-ttu-id="a988a-153">Alla fält som du anger än hello nyckelfältet kommer att ignoreras.</span><span class="sxs-lookup"><span data-stu-id="a988a-153">Any fields you specify other than hello key field will be ignored.</span></span> <span data-ttu-id="a988a-154">Om du vill tooremove ett fält från ett dokument, använda `Merge` enkelt och i stället ange hello fältet explicit toonull.</span><span class="sxs-lookup"><span data-stu-id="a988a-154">If you want tooremove an individual field from a document, use `Merge` instead and simply set hello field explicitly toonull.</span></span> |

<span data-ttu-id="a988a-155">Du kan ange vilken åtgärd du vill toouse med hello olika statiska metoder för hello `IndexBatch` och `IndexAction` klasser, som visas i nästa avsnitt om hello.</span><span class="sxs-lookup"><span data-stu-id="a988a-155">You can specify what action you want toouse with hello various static methods of hello `IndexBatch` and `IndexAction` classes, as shown in hello next section.</span></span>

## <a name="construct-your-indexbatch"></a><span data-ttu-id="a988a-156">Skapa IndexBatch</span><span class="sxs-lookup"><span data-stu-id="a988a-156">Construct your IndexBatch</span></span>
<span data-ttu-id="a988a-157">Nu när du vet vilka åtgärder tooperform på dina dokument du är klar tooconstruct hello `IndexBatch`.</span><span class="sxs-lookup"><span data-stu-id="a988a-157">Now that you know which actions tooperform on your documents, you are ready tooconstruct hello `IndexBatch`.</span></span> <span data-ttu-id="a988a-158">Hej exemplet nedan visar hur toocreate en batch med några olika åtgärder.</span><span class="sxs-lookup"><span data-stu-id="a988a-158">hello example below shows how toocreate a batch with a few different actions.</span></span> <span data-ttu-id="a988a-159">Observera att vårt exempel använder en anpassad klass som heter `Hotel` som mappar tooa dokument i hello ”hotell” index.</span><span class="sxs-lookup"><span data-stu-id="a988a-159">Note that our example uses a custom class called `Hotel` that maps tooa document in hello "hotels" index.</span></span>

```csharp
var actions =
    new IndexAction<Hotel>[]
    {
        IndexAction.Upload(
            new Hotel()
            {
                HotelId = "1",
                BaseRate = 199.0,
                Description = "Best hotel in town",
                DescriptionFr = "Meilleur hôtel en ville",
                HotelName = "Fancy Stay",
                Category = "Luxury",
                Tags = new[] { "pool", "view", "wifi", "concierge" },
                ParkingIncluded = false,
                SmokingAllowed = false,
                LastRenovationDate = new DateTimeOffset(2010, 6, 27, 0, 0, 0, TimeSpan.Zero),
                Rating = 5,
                Location = GeographyPoint.Create(47.678581, -122.131577)
            }),
        IndexAction.Upload(
            new Hotel()
            {
                HotelId = "2",
                BaseRate = 79.99,
                Description = "Cheapest hotel in town",
                DescriptionFr = "Hôtel le moins cher en ville",
                HotelName = "Roach Motel",
                Category = "Budget",
                Tags = new[] { "motel", "budget" },
                ParkingIncluded = true,
                SmokingAllowed = true,
                LastRenovationDate = new DateTimeOffset(1982, 4, 28, 0, 0, 0, TimeSpan.Zero),
                Rating = 1,
                Location = GeographyPoint.Create(49.678581, -122.131577)
            }),
        IndexAction.MergeOrUpload(
            new Hotel()
            {
                HotelId = "3",
                BaseRate = 129.99,
                Description = "Close tootown hall and hello river"
            }),
        IndexAction.Delete(new Hotel() { HotelId = "6" })
    };

var batch = IndexBatch.New(actions);
```

<span data-ttu-id="a988a-160">I det här fallet använder du `Upload`, `MergeOrUpload`, och `Delete` som våra Sök-åtgärder, som anges av hello metoderna hello `IndexAction` klass.</span><span class="sxs-lookup"><span data-stu-id="a988a-160">In this case, we are using `Upload`, `MergeOrUpload`, and `Delete` as our search actions, as specified by hello methods called on hello `IndexAction` class.</span></span>

<span data-ttu-id="a988a-161">Anta att exempelindexet ”hotels” redan fyllts med ett antal dokument.</span><span class="sxs-lookup"><span data-stu-id="a988a-161">Assume that this example "hotels" index is already populated with a number of documents.</span></span> <span data-ttu-id="a988a-162">Observera hur vi hade inte toospecify alla fält för hello möjliga dokument när du använder `MergeOrUpload` och hur vi bara ange hello Dokumentnyckel (`HotelId`) när du använder `Delete`.</span><span class="sxs-lookup"><span data-stu-id="a988a-162">Note how we did not have toospecify all hello possible document fields when using `MergeOrUpload` and how we only specified hello document key (`HotelId`) when using `Delete`.</span></span>

<span data-ttu-id="a988a-163">Observera också att du får bara innehålla too1000 dokument i en enskild indexering begäran.</span><span class="sxs-lookup"><span data-stu-id="a988a-163">Also, note that you can only include up too1000 documents in a single indexing request.</span></span>

> [!NOTE]
> <span data-ttu-id="a988a-164">I det här exemplet använder vi olika åtgärder toodifferent dokument.</span><span class="sxs-lookup"><span data-stu-id="a988a-164">In this example, we are applying different actions toodifferent documents.</span></span> <span data-ttu-id="a988a-165">Om du vill använda tooperform hello samma åtgärder i alla dokument i hello batch istället för att ringa `IndexBatch.New`, kunde du använda hello andra statiska metoder för `IndexBatch`.</span><span class="sxs-lookup"><span data-stu-id="a988a-165">If you wanted tooperform hello same actions across all documents in hello batch, instead of calling `IndexBatch.New`, you could use hello other static methods of `IndexBatch`.</span></span> <span data-ttu-id="a988a-166">Du kan till exempel skapa batchar genom att anropa `IndexBatch.Merge`, `IndexBatch.MergeOrUpload` eller `IndexBatch.Delete`.</span><span class="sxs-lookup"><span data-stu-id="a988a-166">For example, you could create batches by calling `IndexBatch.Merge`, `IndexBatch.MergeOrUpload`, or `IndexBatch.Delete`.</span></span> <span data-ttu-id="a988a-167">Dessa metoder använder en samling dokument (objekt av typen `Hotel` i det här exemplet) i stället för `IndexAction`-objekt.</span><span class="sxs-lookup"><span data-stu-id="a988a-167">These methods take a collection of documents (objects of type `Hotel` in this example) instead of `IndexAction` objects.</span></span>
> 
> 

## <a name="import-data-toohello-index"></a><span data-ttu-id="a988a-168">Importera data toohello index</span><span class="sxs-lookup"><span data-stu-id="a988a-168">Import data toohello index</span></span>
<span data-ttu-id="a988a-169">Nu när du har en initierad `IndexBatch` objekt, kan du skicka det toohello index genom att anropa `Documents.Index` på din `SearchIndexClient` objekt.</span><span class="sxs-lookup"><span data-stu-id="a988a-169">Now that you have an initialized `IndexBatch` object, you can send it toohello index by calling `Documents.Index` on your `SearchIndexClient` object.</span></span> <span data-ttu-id="a988a-170">följande exempel visar hur hello toocall `Index`, plus vissa extra steg behöver du tooperform:</span><span class="sxs-lookup"><span data-stu-id="a988a-170">hello following example shows how toocall `Index`, as well as some extra steps you will need tooperform:</span></span>

```csharp
try
{
    indexClient.Documents.Index(batch);
}
catch (IndexBatchException e)
{
    // Sometimes when your Search service is under load, indexing will fail for some of hello documents in
    // hello batch. Depending on your application, you can take compensating actions like delaying and
    // retrying. For this simple demo, we just log hello failed document keys and continue.
    Console.WriteLine(
        "Failed tooindex some of hello documents: {0}",
        String.Join(", ", e.IndexingResults.Where(r => !r.Succeeded).Select(r => r.Key)));
}

Console.WriteLine("Waiting for documents toobe indexed...\n");
Thread.Sleep(2000);
```

<span data-ttu-id="a988a-171">Obs hello `try` / `catch` omgivande hello anropet toohello `Index` metod.</span><span class="sxs-lookup"><span data-stu-id="a988a-171">Note hello `try`/`catch` surrounding hello call toohello `Index` method.</span></span> <span data-ttu-id="a988a-172">hello catch-blocket hanterar ett viktigt fel ärende för indexering.</span><span class="sxs-lookup"><span data-stu-id="a988a-172">hello catch block handles an important error case for indexing.</span></span> <span data-ttu-id="a988a-173">Om din Azure Search-tjänst misslyckas tooindex vissa hello dokument i hello batch en `IndexBatchException` genereras av `Documents.Index`.</span><span class="sxs-lookup"><span data-stu-id="a988a-173">If your Azure Search service fails tooindex some of hello documents in hello batch, an `IndexBatchException` is thrown by `Documents.Index`.</span></span> <span data-ttu-id="a988a-174">Detta kan inträffa om du indexerar dokument när tjänsten är hårt belastad.</span><span class="sxs-lookup"><span data-stu-id="a988a-174">This can happen if you are indexing documents while your service is under heavy load.</span></span> <span data-ttu-id="a988a-175">**Vi rekommenderar starkt att du uttryckligen hanterar den här situationen i din kod.**</span><span class="sxs-lookup"><span data-stu-id="a988a-175">**We strongly recommend explicitly handling this case in your code.**</span></span> <span data-ttu-id="a988a-176">Du kan fördröja och försök indexering hello-dokument som misslyckades du kan logga in och fortsätta som hello exempel har eller kan du göra något annat beroende på programmets konsekvens kraven</span><span class="sxs-lookup"><span data-stu-id="a988a-176">You can delay and then retry indexing hello documents that failed, or you can log and continue like hello sample does, or you can do something else depending on your application's data consistency requirements.</span></span>

<span data-ttu-id="a988a-177">Slutligen hello koden i hello-exemplet ovan fördröjningar i två sekunder.</span><span class="sxs-lookup"><span data-stu-id="a988a-177">Finally, hello code in hello example above delays for two seconds.</span></span> <span data-ttu-id="a988a-178">Indexering sker asynkront i Azure Search-tjänsten så hello exempelprogrammet måste toowait en kort tid tooensure att hello dokument är tillgängliga för sökning.</span><span class="sxs-lookup"><span data-stu-id="a988a-178">Indexing happens asynchronously in your Azure Search service, so hello sample application needs toowait a short time tooensure that hello documents are available for searching.</span></span> <span data-ttu-id="a988a-179">Fördröjningar som den här är normalt endast nödvändiga i demonstrationer, tester och exempelprogram.</span><span class="sxs-lookup"><span data-stu-id="a988a-179">Delays like this are typically only necessary in demos, tests, and sample applications.</span></span>

<a name="HotelClass"></a>

### <a name="how-hello-net-sdk-handles-documents"></a><span data-ttu-id="a988a-180">Hanteringen av dokument i hello .NET SDK</span><span class="sxs-lookup"><span data-stu-id="a988a-180">How hello .NET SDK handles documents</span></span>
<span data-ttu-id="a988a-181">Du kanske undrar hur hello Azure Search .NET SDK är kan tooupload instanser av en användardefinierad klass som `Hotel` toohello index.</span><span class="sxs-lookup"><span data-stu-id="a988a-181">You may be wondering how hello Azure Search .NET SDK is able tooupload instances of a user-defined class like `Hotel` toohello index.</span></span> <span data-ttu-id="a988a-182">toohelp besvara frågan, ska vi titta på hello `Hotel` -klassen, som mappar toohello indexeringsschema som definierats i [skapa ett Azure Search-index med hello .NET SDK](search-create-index-dotnet.md#DefineIndex):</span><span class="sxs-lookup"><span data-stu-id="a988a-182">toohelp answer that question, let's look at hello `Hotel` class, which maps toohello index schema defined in [Create an Azure Search index using hello .NET SDK](search-create-index-dotnet.md#DefineIndex):</span></span>

```csharp
[SerializePropertyNamesAsCamelCase]
public partial class Hotel
{
    [Key]
    [IsFilterable]
    public string HotelId { get; set; }

    [IsFilterable, IsSortable, IsFacetable]
    public double? BaseRate { get; set; }

    [IsSearchable]
    public string Description { get; set; }

    [IsSearchable]
    [Analyzer(AnalyzerName.AsString.FrLucene)]
    [JsonProperty("description_fr")]
    public string DescriptionFr { get; set; }

    [IsSearchable, IsFilterable, IsSortable]
    public string HotelName { get; set; }

    [IsSearchable, IsFilterable, IsSortable, IsFacetable]
    public string Category { get; set; }

    [IsSearchable, IsFilterable, IsFacetable]
    public string[] Tags { get; set; }

    [IsFilterable, IsFacetable]
    public bool? ParkingIncluded { get; set; }

    [IsFilterable, IsFacetable]
    public bool? SmokingAllowed { get; set; }

    [IsFilterable, IsSortable, IsFacetable]
    public DateTimeOffset? LastRenovationDate { get; set; }

    [IsFilterable, IsSortable, IsFacetable]
    public int? Rating { get; set; }

    [IsFilterable, IsSortable]
    public GeographyPoint Location { get; set; }

    // ToString() method omitted for brevity...
}
```

<span data-ttu-id="a988a-183">hello först öppna toonotice är att varje offentlig egenskap för `Hotel` motsvarar tooa fält i hello indexdefinitionen, men med en viktig skillnad: hello varje fält börjar med en gemen bokstav (”slitbanor fallet”) vid hello namnet på varje offentlig Egenskapen för `Hotel` börjar med en versal (”Pascal fall”).</span><span class="sxs-lookup"><span data-stu-id="a988a-183">hello first thing toonotice is that each public property of `Hotel` corresponds tooa field in hello index definition, but with one crucial difference: hello name of each field starts with a lower-case letter ("camel case"), while hello name of each public property of `Hotel` starts with an upper-case letter ("Pascal case").</span></span> <span data-ttu-id="a988a-184">Det här är ett vanligt scenario i .NET-program som utför databindning där hello målschemat är utanför hello kontroll över hello programutvecklaren.</span><span class="sxs-lookup"><span data-stu-id="a988a-184">This is a common scenario in .NET applications that perform data-binding where hello target schema is outside hello control of hello application developer.</span></span> <span data-ttu-id="a988a-185">Du kan se hello SDK toomap hello egenskapen namn toocamel fall automatiskt med hello snarare än att tooviolate hello .NET naming riktlinjer genom att egenskapen namn slitbanor skiftläge `[SerializePropertyNamesAsCamelCase]` attribut.</span><span class="sxs-lookup"><span data-stu-id="a988a-185">Rather than having tooviolate hello .NET naming guidelines by making property names camel-case, you can tell hello SDK toomap hello property names toocamel-case automatically with hello `[SerializePropertyNamesAsCamelCase]` attribute.</span></span>

> [!NOTE]
> <span data-ttu-id="a988a-186">hello Azure Search .NET SDK använder hello [NewtonSoft JSON.NET](http://www.newtonsoft.com/json/help/html/Introduction.htm) biblioteket tooserialize och deserialisera din anpassade modellen objekt tooand från JSON.</span><span class="sxs-lookup"><span data-stu-id="a988a-186">hello Azure Search .NET SDK uses hello [NewtonSoft JSON.NET](http://www.newtonsoft.com/json/help/html/Introduction.htm) library tooserialize and deserialize your custom model objects tooand from JSON.</span></span> <span data-ttu-id="a988a-187">Du kan anpassa den här serialiseringen om det behövs.</span><span class="sxs-lookup"><span data-stu-id="a988a-187">You can customize this serialization if needed.</span></span> <span data-ttu-id="a988a-188">Mer information finns i [Anpassad serialisering med JSON.NET](search-howto-dotnet-sdk.md#JsonDotNet).</span><span class="sxs-lookup"><span data-stu-id="a988a-188">You can find more details in [Custom Serialization with JSON.NET](search-howto-dotnet-sdk.md#JsonDotNet).</span></span> <span data-ttu-id="a988a-189">Ett exempel på detta är hello använda hello `[JsonProperty]` -attributet på hello `DescriptionFr` egenskap i hello exempelkod ovan.</span><span class="sxs-lookup"><span data-stu-id="a988a-189">One example of this is hello use of hello `[JsonProperty]` attribute on hello `DescriptionFr` property in hello sample code above.</span></span>
> 
> 

<span data-ttu-id="a988a-190">hello stycke om hello `Hotel` klass är hello datatyperna hello offentliga egenskaper.</span><span class="sxs-lookup"><span data-stu-id="a988a-190">hello second important thing about hello `Hotel` class are hello data types of hello public properties.</span></span> <span data-ttu-id="a988a-191">hello .NET-typer av de här egenskaperna mappa tootheir motsvarande fälttyp i hello indexdefinitionen.</span><span class="sxs-lookup"><span data-stu-id="a988a-191">hello .NET types of these properties map tootheir equivalent field types in hello index definition.</span></span> <span data-ttu-id="a988a-192">Till exempel hello `Category` strängegenskap mappar toohello `category` fält som är av typen `DataType.String`.</span><span class="sxs-lookup"><span data-stu-id="a988a-192">For example, hello `Category` string property maps toohello `category` field, which is of type `DataType.String`.</span></span> <span data-ttu-id="a988a-193">Det finns liknande typmappningar mellan `bool?` och `DataType.Boolean`, `DateTimeOffset?` och `DataType.DateTimeOffset` osv.</span><span class="sxs-lookup"><span data-stu-id="a988a-193">There are similar type mappings between `bool?` and `DataType.Boolean`, `DateTimeOffset?` and `DataType.DateTimeOffset`, and so forth.</span></span> <span data-ttu-id="a988a-194">hello särskilda regler för mappning hello dokumenteras med hello `Documents.Get` metod i hello [Azure Search .NET SDK referens](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.idocumentsoperations#Microsoft_Azure_Search_IDocumentsOperations_GetWithHttpMessagesAsync__1_System_String_System_Collections_Generic_IEnumerable_System_String__Microsoft_Azure_Search_Models_SearchRequestOptions_System_Collections_Generic_Dictionary_System_String_System_Collections_Generic_List_System_String___System_Threading_CancellationToken_).</span><span class="sxs-lookup"><span data-stu-id="a988a-194">hello specific rules for hello type mapping are documented with hello `Documents.Get` method in hello [Azure Search .NET SDK reference](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.idocumentsoperations#Microsoft_Azure_Search_IDocumentsOperations_GetWithHttpMessagesAsync__1_System_String_System_Collections_Generic_IEnumerable_System_String__Microsoft_Azure_Search_Models_SearchRequestOptions_System_Collections_Generic_Dictionary_System_String_System_Collections_Generic_List_System_String___System_Threading_CancellationToken_).</span></span>

<span data-ttu-id="a988a-195">Den här möjligheten toouse egna klasser som dokument fungerar i båda riktningarna; Du kan också hämta sökresultat och hello SDK automatiskt deserialisera dem tooa typ du väljer, enligt hello [nästa artikel](search-query-dotnet.md).</span><span class="sxs-lookup"><span data-stu-id="a988a-195">This ability toouse your own classes as documents works in both directions; You can also retrieve search results and have hello SDK automatically deserialize them tooa type of your choice, as shown in hello [next article](search-query-dotnet.md).</span></span>

> [!NOTE]
> <span data-ttu-id="a988a-196">hello Azure Search .NET SDK stöder också dynamiskt skrev dokument med hjälp av hello `Document` klassen, som är en nyckel/värde-mappning av toofield värdena för fältet namn.</span><span class="sxs-lookup"><span data-stu-id="a988a-196">hello Azure Search .NET SDK also supports dynamically-typed documents using hello `Document` class, which is a key/value mapping of field names toofield values.</span></span> <span data-ttu-id="a988a-197">Detta är användbart i scenarier där du inte vet hello indexeringsschema i designläge, eller om det skulle vara olämplig toobind toospecific modellen klasser.</span><span class="sxs-lookup"><span data-stu-id="a988a-197">This is useful in scenarios where you don't know hello index schema at design-time, or where it would be inconvenient toobind toospecific model classes.</span></span> <span data-ttu-id="a988a-198">Alla hello metoder i hello SDK som handlar om dokument har överlagringar som fungerar med hello `Document` klass, samt strikt typkontroll överlagringar som har en generisk typparameter.</span><span class="sxs-lookup"><span data-stu-id="a988a-198">All hello methods in hello SDK that deal with documents have overloads that work with hello `Document` class, as well as strongly-typed overloads that take a generic type parameter.</span></span> <span data-ttu-id="a988a-199">Endast hello senare används i hello exempelkoden i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="a988a-199">Only hello latter are used in hello sample code in this article.</span></span>
> 
> 

<span data-ttu-id="a988a-200">**Varför du bör använda datatyper som kan ha värdet null**</span><span class="sxs-lookup"><span data-stu-id="a988a-200">**Why you should use nullable data types**</span></span>

<span data-ttu-id="a988a-201">När du skapar egna modellen klasser toomap tooan Azure Search index, rekommenderar vi deklarerar egenskaper för värdetyper som `bool` och `int` toobe kan ha värdet null (till exempel `bool?` i stället för `bool`).</span><span class="sxs-lookup"><span data-stu-id="a988a-201">When designing your own model classes toomap tooan Azure Search index, we recommend declaring properties of value types such as `bool` and `int` toobe nullable (for example, `bool?` instead of `bool`).</span></span> <span data-ttu-id="a988a-202">Om du använder en icke-nullbar egenskapen, det finns också**garantera** att inga dokument i indexet innehåller ett null-värde för hello motsvarande fält.</span><span class="sxs-lookup"><span data-stu-id="a988a-202">If you use a non-nullable property, you have too**guarantee** that no documents in your index contain a null value for hello corresponding field.</span></span> <span data-ttu-id="a988a-203">Varken hello SDK eller hello Azure Search-tjänsten kan du tooenforce detta.</span><span class="sxs-lookup"><span data-stu-id="a988a-203">Neither hello SDK nor hello Azure Search service will help you tooenforce this.</span></span>

<span data-ttu-id="a988a-204">Detta är inte bara ett hypotetiskt problem: Tänk dig ett scenario där du lägger till ett nytt fält tooan befintliga index som är av typen `DataType.Int32`.</span><span class="sxs-lookup"><span data-stu-id="a988a-204">This is not just a hypothetical concern: Imagine a scenario where you add a new field tooan existing index that is of type `DataType.Int32`.</span></span> <span data-ttu-id="a988a-205">När du har uppdaterat hello indexdefinitionen har alla dokument som ett null-värde för det nya fältet (eftersom alla typer inte kan vara null i Azure Search).</span><span class="sxs-lookup"><span data-stu-id="a988a-205">After updating hello index definition, all documents will have a null value for that new field (since all types are nullable in Azure Search).</span></span> <span data-ttu-id="a988a-206">Om du använder en modellklass sedan med en icke-nullbar `int` -egenskapen för det här fältet får du en `JsonSerializationException` så här när du försöker tooretrieve dokument:</span><span class="sxs-lookup"><span data-stu-id="a988a-206">If you then use a model class with a non-nullable `int` property for that field, you will get a `JsonSerializationException` like this when trying tooretrieve documents:</span></span>

    Error converting value {null} tootype 'System.Int32'. Path 'IntValue'.

<span data-ttu-id="a988a-207">Av den anledningen rekommenderar vi att du använder nullbara typer i dina modellklasser som bästa praxis.</span><span class="sxs-lookup"><span data-stu-id="a988a-207">For this reason, we recommend that you use nullable types in your model classes as a best practice.</span></span>

## <a name="next-steps"></a><span data-ttu-id="a988a-208">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="a988a-208">Next steps</span></span>
<span data-ttu-id="a988a-209">Du kommer att redo toostart utfärda frågor toosearch för dokument efter fylla ditt Azure Search-index.</span><span class="sxs-lookup"><span data-stu-id="a988a-209">After populating your Azure Search index, you will be ready toostart issuing queries toosearch for documents.</span></span> <span data-ttu-id="a988a-210">Mer information finns i [Fråga ditt Azure Search-index](search-query-overview.md).</span><span class="sxs-lookup"><span data-stu-id="a988a-210">See [Query Your Azure Search Index](search-query-overview.md) for details.</span></span>

