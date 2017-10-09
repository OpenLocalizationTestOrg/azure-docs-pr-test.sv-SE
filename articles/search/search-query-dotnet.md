---
title: "aaa ”fråga ett index (.NET API - Azure Search) | Microsoft Docs ”"
description: "Skapa en sökfråga i Azure search och använda Sök parametrar toofilter och sortera sökresultaten."
services: search
manager: jhubbard
documentationcenter: 
author: brjohnstmsft
ms.assetid: 12c3efba-ea99-4187-9d2d-f63b5ec7040d
ms.service: search
ms.devlang: dotnet
ms.workload: search
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.date: 05/19/2017
ms.author: brjohnst
ms.openlocfilehash: 8b3ba1cd1270aad038fb48d9053fcff35d243e13
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="query-your-azure-search-index-using-hello-net-sdk"></a><span data-ttu-id="2bfd9-103">Fråga ditt Azure Search-index med hello .NET SDK</span><span class="sxs-lookup"><span data-stu-id="2bfd9-103">Query your Azure Search index using hello .NET SDK</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="2bfd9-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="2bfd9-104">Overview</span></span>](search-query-overview.md)
> * [<span data-ttu-id="2bfd9-105">Portalen</span><span class="sxs-lookup"><span data-stu-id="2bfd9-105">Portal</span></span>](search-explorer.md)
> * [<span data-ttu-id="2bfd9-106">.NET</span><span class="sxs-lookup"><span data-stu-id="2bfd9-106">.NET</span></span>](search-query-dotnet.md)
> * [<span data-ttu-id="2bfd9-107">REST</span><span class="sxs-lookup"><span data-stu-id="2bfd9-107">REST</span></span>](search-query-rest-api.md)
> 
> 

<span data-ttu-id="2bfd9-108">Den här artikeln visar hur tooquery ett index med hjälp av hello [Azure Search .NET SDK](https://aka.ms/search-sdk).</span><span class="sxs-lookup"><span data-stu-id="2bfd9-108">This article will show you how tooquery an index using hello [Azure Search .NET SDK](https://aka.ms/search-sdk).</span></span>

<span data-ttu-id="2bfd9-109">Innan du påbörjar den här genomgången bör du redan ha [skapat ett Azure Search-index](search-what-is-an-index.md) och [fyllt det med data](search-what-is-data-import.md).</span><span class="sxs-lookup"><span data-stu-id="2bfd9-109">Before beginning this walkthrough, you should already have [created an Azure Search index](search-what-is-an-index.md) and [populated it with data](search-what-is-data-import.md).</span></span>

> [!NOTE]
> <span data-ttu-id="2bfd9-110">All exempelkod i den här artikeln är skriven i C#.</span><span class="sxs-lookup"><span data-stu-id="2bfd9-110">All sample code in this article is written in C#.</span></span> <span data-ttu-id="2bfd9-111">Du kan hitta hello fullständig källkoden [på GitHub](http://aka.ms/search-dotnet-howto).</span><span class="sxs-lookup"><span data-stu-id="2bfd9-111">You can find hello full source code [on GitHub](http://aka.ms/search-dotnet-howto).</span></span> <span data-ttu-id="2bfd9-112">Du kan också läsa om hello [Azure Search .NET SDK](search-howto-dotnet-sdk.md) för en mer detaljerad genomgång av hello exempelkod.</span><span class="sxs-lookup"><span data-stu-id="2bfd9-112">You can also read about hello [Azure Search .NET SDK](search-howto-dotnet-sdk.md) for a more detailed walk through of hello sample code.</span></span>

## <a name="identify-your-azure-search-services-query-api-key"></a><span data-ttu-id="2bfd9-113">Identifiera API-frågenyckeln för Azure Search-tjänsten</span><span class="sxs-lookup"><span data-stu-id="2bfd9-113">Identify your Azure Search service's query api-key</span></span>
<span data-ttu-id="2bfd9-114">Nu när du har skapat ett Azure Search-index är nästan klara tooissue frågor med hello .NET SDK.</span><span class="sxs-lookup"><span data-stu-id="2bfd9-114">Now that you have created an Azure Search index, you are almost ready tooissue queries using hello .NET SDK.</span></span> <span data-ttu-id="2bfd9-115">Först behöver tooobtain en av hello frågan api-nycklar som har genererats för hello söktjänsten du etablerat.</span><span class="sxs-lookup"><span data-stu-id="2bfd9-115">First, you will need tooobtain one of hello query api-keys that was generated for hello search service you provisioned.</span></span> <span data-ttu-id="2bfd9-116">hello .NET SDK skickar den här api-nyckel för varje begäran tooyour-tjänst.</span><span class="sxs-lookup"><span data-stu-id="2bfd9-116">hello .NET SDK will send this api-key on every request tooyour service.</span></span> <span data-ttu-id="2bfd9-117">Med en giltig nyckel upprättar förtroende mellan hello programmet skickar hello begäran och hello-tjänsten som hanterar den fall per begäran.</span><span class="sxs-lookup"><span data-stu-id="2bfd9-117">Having a valid key establishes trust, on a per request basis, between hello application sending hello request and hello service that handles it.</span></span>

1. <span data-ttu-id="2bfd9-118">toofind din tjänst api-nycklar kan du logga in toohello [Azure-portalen](https://portal.azure.com/)</span><span class="sxs-lookup"><span data-stu-id="2bfd9-118">toofind your service's api-keys you can sign in toohello [Azure portal](https://portal.azure.com/)</span></span>
2. <span data-ttu-id="2bfd9-119">Gå tooyour Azure Search service-bladet</span><span class="sxs-lookup"><span data-stu-id="2bfd9-119">Go tooyour Azure Search service's blade</span></span>
3. <span data-ttu-id="2bfd9-120">Klicka på hello ”nycklar”-ikon</span><span class="sxs-lookup"><span data-stu-id="2bfd9-120">Click on hello "Keys" icon</span></span>

<span data-ttu-id="2bfd9-121">Tjänsten har *administratörsnycklar* och *frågenycklar*.</span><span class="sxs-lookup"><span data-stu-id="2bfd9-121">Your service will have *admin keys* and *query keys*.</span></span>

* <span data-ttu-id="2bfd9-122">Din primära och sekundära *admin nycklar* bevilja fullständiga rättigheter tooall åtgärder, inklusive hello möjlighet toomanage hello service, skapa och ta bort index, indexerare och datakällor.</span><span class="sxs-lookup"><span data-stu-id="2bfd9-122">Your primary and secondary *admin keys* grant full rights tooall operations, including hello ability toomanage hello service, create and delete indexes, indexers, and data sources.</span></span> <span data-ttu-id="2bfd9-123">Det finns två nycklar så att du kan fortsätta toouse hello sekundärnyckeln om du väljer tooregenerate hello primärnyckel och vice versa.</span><span class="sxs-lookup"><span data-stu-id="2bfd9-123">There are two keys so that you can continue toouse hello secondary key if you decide tooregenerate hello primary key, and vice-versa.</span></span>
* <span data-ttu-id="2bfd9-124">Din *fråga nycklar* bevilja läsbehörighet tooindexes och dokument och är vanligtvis distribuerade tooclient program som utfärdar search-begäranden.</span><span class="sxs-lookup"><span data-stu-id="2bfd9-124">Your *query keys* grant read-only access tooindexes and documents, and are typically distributed tooclient applications that issue search requests.</span></span>

<span data-ttu-id="2bfd9-125">Du kan använda en av din fråga nycklar för hello frågor till ett index är.</span><span class="sxs-lookup"><span data-stu-id="2bfd9-125">For hello purposes of querying an index, you can use one of your query keys.</span></span> <span data-ttu-id="2bfd9-126">Admin-nycklar kan också användas för frågor, men du bör använda en fråga nyckel i din programkod eftersom det bättre följer hello [principen om minsta behörighet](https://en.wikipedia.org/wiki/Principle_of_least_privilege).</span><span class="sxs-lookup"><span data-stu-id="2bfd9-126">Your admin keys can also be used for queries, but you should use a query key in your application code as this better follows hello [Principle of least privilege](https://en.wikipedia.org/wiki/Principle_of_least_privilege).</span></span>

## <a name="create-an-instance-of-hello-searchindexclient-class"></a><span data-ttu-id="2bfd9-127">Skapa en instans av hello SearchIndexClient-klass</span><span class="sxs-lookup"><span data-stu-id="2bfd9-127">Create an instance of hello SearchIndexClient class</span></span>
<span data-ttu-id="2bfd9-128">tooissue frågor med hello Azure Search .NET SDK, behöver du toocreate en instans av hello `SearchIndexClient` klass.</span><span class="sxs-lookup"><span data-stu-id="2bfd9-128">tooissue queries with hello Azure Search .NET SDK, you will need toocreate an instance of hello `SearchIndexClient` class.</span></span> <span data-ttu-id="2bfd9-129">Den här klassen har flera konstruktorer.</span><span class="sxs-lookup"><span data-stu-id="2bfd9-129">This class has several constructors.</span></span> <span data-ttu-id="2bfd9-130">hello du tar din sökning tjänstnamn, indexnamn, och en `SearchCredentials` objekt som parametrar.</span><span class="sxs-lookup"><span data-stu-id="2bfd9-130">hello one you want takes your search service name, index name, and a `SearchCredentials` object as parameters.</span></span> <span data-ttu-id="2bfd9-131">`SearchCredentials` omsluter din API-nyckel.</span><span class="sxs-lookup"><span data-stu-id="2bfd9-131">`SearchCredentials` wraps your api-key.</span></span>

<span data-ttu-id="2bfd9-132">hello koden nedan skapar en ny `SearchIndexClient` för hello ”hotell” index (skapat i [skapa ett Azure Search-index med hello .NET SDK](search-create-index-dotnet.md)) med värden för hello Sök tjänstnamn och api-nyckel som lagras i hello programmets konfiguration filen (`appsettings.json` hello gäller hello [exempelprogram](http://aka.ms/search-dotnet-howto)):</span><span class="sxs-lookup"><span data-stu-id="2bfd9-132">hello code below creates a new `SearchIndexClient` for hello "hotels" index (created in [Create an Azure Search index using hello .NET SDK](search-create-index-dotnet.md)) using values for hello search service name and api-key that are stored in hello application's config file (`appsettings.json` in hello case of hello [sample application](http://aka.ms/search-dotnet-howto)):</span></span>

```csharp
private static SearchIndexClient CreateSearchIndexClient(IConfigurationRoot configuration)
{
    string searchServiceName = configuration["SearchServiceName"];
    string queryApiKey = configuration["SearchServiceQueryApiKey"];

    SearchIndexClient indexClient = new SearchIndexClient(searchServiceName, "hotels", new SearchCredentials(queryApiKey));
    return indexClient;
}
```

<span data-ttu-id="2bfd9-133">`SearchIndexClient` har en `Documents`-egenskap.</span><span class="sxs-lookup"><span data-stu-id="2bfd9-133">`SearchIndexClient` has a `Documents` property.</span></span> <span data-ttu-id="2bfd9-134">Den här egenskapen anger alla hello metoderna du behöver tooquery Azure Search index.</span><span class="sxs-lookup"><span data-stu-id="2bfd9-134">This property provides all hello methods you need tooquery Azure Search indexes.</span></span>

## <a name="query-your-index"></a><span data-ttu-id="2bfd9-135">Skicka frågor mot ditt index</span><span class="sxs-lookup"><span data-stu-id="2bfd9-135">Query your index</span></span>
<span data-ttu-id="2bfd9-136">Sökning med hello .NET SDK är så enkelt som att anropa hello `Documents.Search` metod på din `SearchIndexClient`.</span><span class="sxs-lookup"><span data-stu-id="2bfd9-136">Searching with hello .NET SDK is as simple as calling hello `Documents.Search` method on your `SearchIndexClient`.</span></span> <span data-ttu-id="2bfd9-137">Den här metoden tar några parametrar, inklusive hello söktext tillsammans med en `SearchParameters` objekt som kan vara används toofurther förfina hello fråga.</span><span class="sxs-lookup"><span data-stu-id="2bfd9-137">This method takes a few parameters, including hello search text, along with a `SearchParameters` object that can be used toofurther refine hello query.</span></span>

#### <a name="types-of-queries"></a><span data-ttu-id="2bfd9-138">Typer av frågor</span><span class="sxs-lookup"><span data-stu-id="2bfd9-138">Types of Queries</span></span>
<span data-ttu-id="2bfd9-139">hello två huvudsakliga [fråga typer](search-query-overview.md#types-of-queries) du ska använda är `search` och `filter`.</span><span class="sxs-lookup"><span data-stu-id="2bfd9-139">hello two main [query types](search-query-overview.md#types-of-queries) you will use are `search` and `filter`.</span></span> <span data-ttu-id="2bfd9-140">En `search`-fråga söker efter en eller flera termer i alla *sökbara* fält i ditt index.</span><span class="sxs-lookup"><span data-stu-id="2bfd9-140">A `search` query searches for one or more terms in all *searchable* fields in your index.</span></span> <span data-ttu-id="2bfd9-141">En `filter`-fråga utvärderar ett booleskt uttryck i alla *filtrerbara* fält i ett index.</span><span class="sxs-lookup"><span data-stu-id="2bfd9-141">A `filter` query evaluates a boolean expression over all *filterable* fields in an index.</span></span>

<span data-ttu-id="2bfd9-142">Både sökningar och filter utförs med hello `Documents.Search` metod.</span><span class="sxs-lookup"><span data-stu-id="2bfd9-142">Both searches and filters are performed using hello `Documents.Search` method.</span></span> <span data-ttu-id="2bfd9-143">En sökfråga kan överföras i hello `searchText` parameter, medan en filteruttrycket kan överföras i hello `Filter` -egenskapen för hello `SearchParameters` klass.</span><span class="sxs-lookup"><span data-stu-id="2bfd9-143">A search query can be passed in hello `searchText` parameter, while a filter expression can be passed in hello `Filter` property of hello `SearchParameters` class.</span></span> <span data-ttu-id="2bfd9-144">toofilter utan att söka, skicka `"*"` för hello `searchText` parameter.</span><span class="sxs-lookup"><span data-stu-id="2bfd9-144">toofilter without searching, just pass `"*"` for hello `searchText` parameter.</span></span> <span data-ttu-id="2bfd9-145">toosearch utan att filtrera, lämnar du bara hello `Filter` egenskapen unset, eller inte skicka in en `SearchParameters` instansen alls.</span><span class="sxs-lookup"><span data-stu-id="2bfd9-145">toosearch without filtering, just leave hello `Filter` property unset, or do not pass in a `SearchParameters` instance at all.</span></span>

#### <a name="example-queries"></a><span data-ttu-id="2bfd9-146">Exempelfrågor</span><span class="sxs-lookup"><span data-stu-id="2bfd9-146">Example Queries</span></span>
<span data-ttu-id="2bfd9-147">hello följande exempelkod visar några olika sätt tooquery hello ”hotell” index som definierats i [skapa ett Azure Search-index med hello .NET SDK](search-create-index-dotnet.md#DefineIndex).</span><span class="sxs-lookup"><span data-stu-id="2bfd9-147">hello following sample code shows a few different ways tooquery hello "hotels" index defined in [Create an Azure Search index using hello .NET SDK](search-create-index-dotnet.md#DefineIndex).</span></span> <span data-ttu-id="2bfd9-148">Observera att hello dokument som returneras med hello sökresultaten är instanser av hello `Hotel` -klassen, som har definierats i [Import av Data i Azure Search med hello .NET SDK](search-import-data-dotnet.md#HotelClass).</span><span class="sxs-lookup"><span data-stu-id="2bfd9-148">Note that hello documents returned with hello search results are instances of hello `Hotel` class, which was defined in [Data Import in Azure Search using hello .NET SDK](search-import-data-dotnet.md#HotelClass).</span></span> <span data-ttu-id="2bfd9-149">Hej exempelkod utnyttjar en `WriteDocuments` metoden toooutput hello sökresultat toohello konsolen.</span><span class="sxs-lookup"><span data-stu-id="2bfd9-149">hello sample code makes use of a `WriteDocuments` method toooutput hello search results toohello console.</span></span> <span data-ttu-id="2bfd9-150">Den här metoden beskrivs i nästa avsnitt om hello.</span><span class="sxs-lookup"><span data-stu-id="2bfd9-150">This method is described in hello next section.</span></span>

```csharp
SearchParameters parameters;
DocumentSearchResult<Hotel> results;

Console.WriteLine("Search hello entire index for hello term 'budget' and return only hello hotelName field:\n");

parameters =
    new SearchParameters()
    {
        Select = new[] { "hotelName" }
    };

results = indexClient.Documents.Search<Hotel>("budget", parameters);

WriteDocuments(results);

Console.Write("Apply a filter toohello index toofind hotels cheaper than $150 per night, ");
Console.WriteLine("and return hello hotelId and description:\n");

parameters =
    new SearchParameters()
    {
        Filter = "baseRate lt 150",
        Select = new[] { "hotelId", "description" }
    };

results = indexClient.Documents.Search<Hotel>("*", parameters);

WriteDocuments(results);

Console.Write("Search hello entire index, order by a specific field (lastRenovationDate) ");
Console.Write("in descending order, take hello top two results, and show only hotelName and ");
Console.WriteLine("lastRenovationDate:\n");

parameters =
    new SearchParameters()
    {
        OrderBy = new[] { "lastRenovationDate desc" },
        Select = new[] { "hotelName", "lastRenovationDate" },
        Top = 2
    };

results = indexClient.Documents.Search<Hotel>("*", parameters);

WriteDocuments(results);

Console.WriteLine("Search hello entire index for hello term 'motel':\n");

parameters = new SearchParameters();
results = indexClient.Documents.Search<Hotel>("motel", parameters);

WriteDocuments(results);
```

## <a name="handle-search-results"></a><span data-ttu-id="2bfd9-151">Hantera sökresultat</span><span class="sxs-lookup"><span data-stu-id="2bfd9-151">Handle search results</span></span>
<span data-ttu-id="2bfd9-152">Hej `Documents.Search` metoden returnerar en `DocumentSearchResult` objekt som innehåller hello resultaten av hello-frågan.</span><span class="sxs-lookup"><span data-stu-id="2bfd9-152">hello `Documents.Search` method returns a `DocumentSearchResult` object that contains hello results of hello query.</span></span> <span data-ttu-id="2bfd9-153">hello exemplet i föregående avsnitt i hello används en metod som kallas `WriteDocuments` toooutput hello sökresultat toohello konsolen:</span><span class="sxs-lookup"><span data-stu-id="2bfd9-153">hello example in hello previous section used a method called `WriteDocuments` toooutput hello search results toohello console:</span></span>

```csharp
private static void WriteDocuments(DocumentSearchResult<Hotel> searchResults)
{
    foreach (SearchResult<Hotel> result in searchResults.Results)
    {
        Console.WriteLine(result.Document);
    }

    Console.WriteLine();
}
```

<span data-ttu-id="2bfd9-154">Här är vad hello resultatet ser ut som om för hello frågor i hello föregående avsnitt, antagande om hello ”hotell” index fylls med hello exempeldata i [Import av Data i Azure Search med hello .NET SDK](search-import-data-dotnet.md):</span><span class="sxs-lookup"><span data-stu-id="2bfd9-154">Here is what hello results look like for hello queries in hello previous section, assuming hello "hotels" index is populated with hello sample data in [Data Import in Azure Search using hello .NET SDK](search-import-data-dotnet.md):</span></span>

```
Search hello entire index for hello term 'budget' and return only hello hotelName field:

Name: Roach Motel

Apply a filter toohello index toofind hotels cheaper than $150 per night, and return hello hotelId and description:

ID: 2   Description: Cheapest hotel in town
ID: 3   Description: Close tootown hall and hello river

Search hello entire index, order by a specific field (lastRenovationDate) in descending order, take hello top two results, and show only hotelName and lastRenovationDate:

Name: Fancy Stay        Last renovated on: 6/27/2010 12:00:00 AM +00:00
Name: Roach Motel       Last renovated on: 4/28/1982 12:00:00 AM +00:00

Search hello entire index for hello term 'motel':

ID: 2   Base rate: 79.99        Description: Cheapest hotel in town     Description (French): Hôtel le moins cher en ville      Name: Roach Motel       Category: Budget        Tags: [motel, budget]   Parking included: yes   Smoking allowed: yes    Last renovated on: 4/28/1982 12:00:00 AM +00:00 Rating: 1/5     Location: Latitude 49.678581, longitude -122.131577
```

<span data-ttu-id="2bfd9-155">hello exempelkod ovan använder hello konsolen toooutput sökresultat.</span><span class="sxs-lookup"><span data-stu-id="2bfd9-155">hello sample code above uses hello console toooutput search results.</span></span> <span data-ttu-id="2bfd9-156">Du behöver också toodisplay sökresultat i ditt eget program.</span><span class="sxs-lookup"><span data-stu-id="2bfd9-156">You will likewise need toodisplay search results in your own application.</span></span> <span data-ttu-id="2bfd9-157">Se [det här exemplet på GitHub](https://github.com/Azure-Samples/search-dotnet-getting-started/tree/master/DotNetSample) ett exempel på hur toorender sökresultat i ett webbprogram med ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="2bfd9-157">See [this sample on GitHub](https://github.com/Azure-Samples/search-dotnet-getting-started/tree/master/DotNetSample) for an example of how toorender search results in an ASP.NET MVC-based web application.</span></span>

