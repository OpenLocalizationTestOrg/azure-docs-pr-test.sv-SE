---
title: "aaaHow toouse Azure Search från ett .NET-program | Microsoft Docs"
description: "Hur toouse Azure söka från ett .NET-program"
services: search
documentationcenter: 
author: brjohnstmsft
manager: jlembicz
editor: 
ms.assetid: 93653341-c05f-4cfd-be45-bb877f964fcb
ms.service: search
ms.devlang: dotnet
ms.workload: search
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 05/22/2017
ms.author: brjohnst
ms.openlocfilehash: 8e13fbe5549547d65941b856ce5a90611261388f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-azure-search-from-a-net-application"></a><span data-ttu-id="14d66-103">Hur toouse Azure söka från ett .NET-program</span><span class="sxs-lookup"><span data-stu-id="14d66-103">How toouse Azure Search from a .NET Application</span></span>
<span data-ttu-id="14d66-104">Den här artikeln är en genomgång tooget igång med hello [Azure Search .NET SDK](https://aka.ms/search-sdk).</span><span class="sxs-lookup"><span data-stu-id="14d66-104">This article is a walkthrough tooget you up and running with hello [Azure Search .NET SDK](https://aka.ms/search-sdk).</span></span> <span data-ttu-id="14d66-105">Du kan använda hello .NET SDK tooimplement en omfattande söka upplevelse i ditt program med Azure Search.</span><span class="sxs-lookup"><span data-stu-id="14d66-105">You can use hello .NET SDK tooimplement a rich search experience in your application using Azure Search.</span></span>

## <a name="whats-in-hello-azure-search-sdk"></a><span data-ttu-id="14d66-106">Vad är hello Azure Search SDK</span><span class="sxs-lookup"><span data-stu-id="14d66-106">What's in hello Azure Search SDK</span></span>
<span data-ttu-id="14d66-107">hello SDK består av ett klientbibliotek `Microsoft.Azure.Search`.</span><span class="sxs-lookup"><span data-stu-id="14d66-107">hello SDK consists of a client library, `Microsoft.Azure.Search`.</span></span> <span data-ttu-id="14d66-108">Det gör du toomanage ditt index, datakällor och indexerare, samt överföra hantera dokument och köra frågor alla utan toodeal med hello detaljer om HTTP- och JSON.</span><span class="sxs-lookup"><span data-stu-id="14d66-108">It enables you toomanage your indexes, data sources, and indexers, as well as upload and manage documents, and execute queries, all without having toodeal with hello details of HTTP and JSON.</span></span>

<span data-ttu-id="14d66-109">hello klientbiblioteket definierar klasser som `Index`, `Field`, och `Document`, samt åtgärder som `Indexes.Create` och `Documents.Search` på hello `SearchServiceClient` och `SearchIndexClient` klasser.</span><span class="sxs-lookup"><span data-stu-id="14d66-109">hello client library defines classes like `Index`, `Field`, and `Document`, as well as operations like `Indexes.Create` and `Documents.Search` on hello `SearchServiceClient` and `SearchIndexClient` classes.</span></span> <span data-ttu-id="14d66-110">De här klasserna är uppdelade i hello följande namnområden:</span><span class="sxs-lookup"><span data-stu-id="14d66-110">These classes are organized into hello following namespaces:</span></span>

* [<span data-ttu-id="14d66-111">Microsoft.Azure.Search</span><span class="sxs-lookup"><span data-stu-id="14d66-111">Microsoft.Azure.Search</span></span>](https://docs.microsoft.com/dotnet/api/microsoft.azure.search)
* [<span data-ttu-id="14d66-112">Microsoft.Azure.Search.Models</span><span class="sxs-lookup"><span data-stu-id="14d66-112">Microsoft.Azure.Search.Models</span></span>](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models)

<span data-ttu-id="14d66-113">hello nuvarande version av hello Azure Search .NET SDK är nu allmänt tillgänglig.</span><span class="sxs-lookup"><span data-stu-id="14d66-113">hello current version of hello Azure Search .NET SDK is now generally available.</span></span> <span data-ttu-id="14d66-114">Om du vill ha feedback tooprovide oss tooincorporate i nästa version av hello, finns våra [feedback sidan](https://feedback.azure.com/forums/263029-azure-search/).</span><span class="sxs-lookup"><span data-stu-id="14d66-114">If you would like tooprovide feedback for us tooincorporate in hello next version, please visit our [feedback page](https://feedback.azure.com/forums/263029-azure-search/).</span></span>

<span data-ttu-id="14d66-115">hello .NET SDK stöder version `2016-09-01` av hello [Azure Search REST API](https://docs.microsoft.com/rest/api/searchservice/).</span><span class="sxs-lookup"><span data-stu-id="14d66-115">hello .NET SDK supports version `2016-09-01` of hello [Azure Search REST API](https://docs.microsoft.com/rest/api/searchservice/).</span></span> <span data-ttu-id="14d66-116">Den här versionen innehåller nu stöd för anpassade analyzers och stöd för Azure Blob och Azure Table indexeraren.</span><span class="sxs-lookup"><span data-stu-id="14d66-116">This version now includes support for custom analyzers and Azure Blob and Azure Table indexer support.</span></span> <span data-ttu-id="14d66-117">Förhandsgranska funktioner som är *inte* en del av den här versionen, till exempel stöd för indexering av JSON- och CSV-filer finns i [preview](search-api-2015-02-28-preview.md) och är tillgängliga via hello äldre [2.0-förhandsversionen av hello .NET SDK ](https://aka.ms/search-sdk-preview).</span><span class="sxs-lookup"><span data-stu-id="14d66-117">Preview features that are *not* part of this version, such as support for indexing JSON and CSV files, are in [preview](search-api-2015-02-28-preview.md) and available via hello older [2.0-preview version of hello .NET SDK](https://aka.ms/search-sdk-preview).</span></span>

<span data-ttu-id="14d66-118">Detta SDK stöder inte [hanteringsåtgärder](https://docs.microsoft.com/rest/api/searchmanagement/) , till exempel skapa och skala Search-tjänster och hantera API-nycklar.</span><span class="sxs-lookup"><span data-stu-id="14d66-118">This SDK does not support [Management Operations](https://docs.microsoft.com/rest/api/searchmanagement/) such as creating and scaling Search services and managing API keys.</span></span> <span data-ttu-id="14d66-119">Om du behöver toomanage din Sök efter resurser från en .NET-program, kan du använda hello [Azure Search .NET Management SDK](https://aka.ms/search-mgmt-sdk).</span><span class="sxs-lookup"><span data-stu-id="14d66-119">If you need toomanage your Search resources from a .NET application, you can use hello [Azure Search .NET Management SDK](https://aka.ms/search-mgmt-sdk).</span></span>

## <a name="upgrading-toohello-latest-version-of-hello-sdk"></a><span data-ttu-id="14d66-120">Uppgradera toohello senaste versionen av hello SDK</span><span class="sxs-lookup"><span data-stu-id="14d66-120">Upgrading toohello latest version of hello SDK</span></span>
<span data-ttu-id="14d66-121">Om du redan använder en äldre version av hello Azure Search .NET SDK och du vill ha tooupgrade toohello ny allmänt tillgänglig version, [i den här artikeln](search-dotnet-sdk-migration.md) förklarar hur.</span><span class="sxs-lookup"><span data-stu-id="14d66-121">If you're already using an older version of hello Azure Search .NET SDK and you'd like tooupgrade toohello new generally available version, [this article](search-dotnet-sdk-migration.md) explains how.</span></span>

## <a name="requirements-for-hello-sdk"></a><span data-ttu-id="14d66-122">Krav för hello SDK</span><span class="sxs-lookup"><span data-stu-id="14d66-122">Requirements for hello SDK</span></span>
1. <span data-ttu-id="14d66-123">Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="14d66-123">Visual Studio 2017.</span></span>
2. <span data-ttu-id="14d66-124">Din egen Azure Search-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="14d66-124">Your own Azure Search service.</span></span> <span data-ttu-id="14d66-125">I ordning toouse hello SDK behöver du hello namnet på din tjänst och en eller flera API-nycklar.</span><span class="sxs-lookup"><span data-stu-id="14d66-125">In order toouse hello SDK, you will need hello name of your service and one or more API keys.</span></span> <span data-ttu-id="14d66-126">[Skapa en tjänst i hello portal](search-create-service-portal.md) hjälper dig genom stegen.</span><span class="sxs-lookup"><span data-stu-id="14d66-126">[Create a service in hello portal](search-create-service-portal.md) will help you through these steps.</span></span>
3. <span data-ttu-id="14d66-127">Hämta hello Azure Search .NET SDK [NuGet-paketet](http://www.nuget.org/packages/Microsoft.Azure.Search) med hjälp av ”hantera NuGet-paket” i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="14d66-127">Download hello Azure Search .NET SDK [NuGet package](http://www.nuget.org/packages/Microsoft.Azure.Search) by using "Manage NuGet Packages" in Visual Studio.</span></span> <span data-ttu-id="14d66-128">Sök bara efter hello paketnamnet `Microsoft.Azure.Search` på NuGet.org.</span><span class="sxs-lookup"><span data-stu-id="14d66-128">Just search for hello package name `Microsoft.Azure.Search` on NuGet.org.</span></span>

<span data-ttu-id="14d66-129">hello Azure Search .NET SDK stöder hello .NET Framework 4.6 och .NET Core-program.</span><span class="sxs-lookup"><span data-stu-id="14d66-129">hello Azure Search .NET SDK supports applications targeting hello .NET Framework 4.6 and .NET Core.</span></span>

## <a name="core-scenarios"></a><span data-ttu-id="14d66-130">Huvudscenarier</span><span class="sxs-lookup"><span data-stu-id="14d66-130">Core scenarios</span></span>
<span data-ttu-id="14d66-131">Det finns flera saker du behöver toodo i sökprogrammet.</span><span class="sxs-lookup"><span data-stu-id="14d66-131">There are several things you'll need toodo in your search application.</span></span> <span data-ttu-id="14d66-132">I den här självstudiekursen kommer tar vi upp dessa huvudscenarier:</span><span class="sxs-lookup"><span data-stu-id="14d66-132">In this tutorial, we'll cover these core scenarios:</span></span>

* <span data-ttu-id="14d66-133">Skapa ett index</span><span class="sxs-lookup"><span data-stu-id="14d66-133">Creating an index</span></span>
* <span data-ttu-id="14d66-134">Fylla hello index med dokument</span><span class="sxs-lookup"><span data-stu-id="14d66-134">Populating hello index with documents</span></span>
* <span data-ttu-id="14d66-135">Söker efter dokument med hjälp av fulltextsökning och filter</span><span class="sxs-lookup"><span data-stu-id="14d66-135">Searching for documents using full-text search and filters</span></span>

<span data-ttu-id="14d66-136">hello exempelkod som följer visar var och en av dessa.</span><span class="sxs-lookup"><span data-stu-id="14d66-136">hello sample code that follows illustrates each of these.</span></span> <span data-ttu-id="14d66-137">Känna sig fria toouse hello kodfragment i ditt eget program.</span><span class="sxs-lookup"><span data-stu-id="14d66-137">Feel free toouse hello code snippets in your own application.</span></span>

### <a name="overview"></a><span data-ttu-id="14d66-138">Översikt</span><span class="sxs-lookup"><span data-stu-id="14d66-138">Overview</span></span>
<span data-ttu-id="14d66-139">hello exempelprogrammet vi kommer att utforska skapar en ny index med namnet ”hotell” fylls med några dokument och sedan kör vissa sökfrågor.</span><span class="sxs-lookup"><span data-stu-id="14d66-139">hello sample application we'll be exploring creates a new index named "hotels", populates it with a few documents, then executes some search queries.</span></span> <span data-ttu-id="14d66-140">Här är hello huvudsakliga program, visar hello övergripande flöde:</span><span class="sxs-lookup"><span data-stu-id="14d66-140">Here is hello main program, showing hello overall flow:</span></span>

```csharp
// This sample shows how toodelete, create, upload documents and query an index
static void Main(string[] args)
{
    IConfigurationBuilder builder = new ConfigurationBuilder().AddJsonFile("appsettings.json");
    IConfigurationRoot configuration = builder.Build();

    SearchServiceClient serviceClient = CreateSearchServiceClient(configuration);

    Console.WriteLine("{0}", "Deleting index...\n");
    DeleteHotelsIndexIfExists(serviceClient);

    Console.WriteLine("{0}", "Creating index...\n");
    CreateHotelsIndex(serviceClient);

    ISearchIndexClient indexClient = serviceClient.Indexes.GetClient("hotels");

    Console.WriteLine("{0}", "Uploading documents...\n");
    UploadDocuments(indexClient);

    ISearchIndexClient indexClientForQueries = CreateSearchIndexClient(configuration);

    RunQueries(indexClientForQueries);

    Console.WriteLine("{0}", "Complete.  Press any key tooend application...\n");
    Console.ReadKey();
}
```

> [!NOTE]
> <span data-ttu-id="14d66-141">Du kan hitta hello fullständig källkoden för hello exempelprogrammet används i denna genomgång på [GitHub](https://github.com/Azure-Samples/search-dotnet-getting-started/tree/master/DotNetHowTo).</span><span class="sxs-lookup"><span data-stu-id="14d66-141">You can find hello full source code of hello sample application used in this walk through on [GitHub](https://github.com/Azure-Samples/search-dotnet-getting-started/tree/master/DotNetHowTo).</span></span>
> 
>

<span data-ttu-id="14d66-142">Vi går igenom detta steg för steg.</span><span class="sxs-lookup"><span data-stu-id="14d66-142">We'll walk through this step by step.</span></span> <span data-ttu-id="14d66-143">Vi måste först toocreate en ny `SearchServiceClient`.</span><span class="sxs-lookup"><span data-stu-id="14d66-143">First we need toocreate a new `SearchServiceClient`.</span></span> <span data-ttu-id="14d66-144">Det här objektet kan toomanage index.</span><span class="sxs-lookup"><span data-stu-id="14d66-144">This object allows you toomanage indexes.</span></span> <span data-ttu-id="14d66-145">I ordning tooconstruct en måste tooprovide ditt Azure Search-tjänstnamn samt ett administrations-API-nyckel.</span><span class="sxs-lookup"><span data-stu-id="14d66-145">In order tooconstruct one, you need tooprovide your Azure Search service name as well as an admin API key.</span></span> <span data-ttu-id="14d66-146">Du kan ange den här informationen i hello `appsettings.json` -filen för hello [exempelprogram](https://github.com/Azure-Samples/search-dotnet-getting-started/tree/master/DotNetHowTo).</span><span class="sxs-lookup"><span data-stu-id="14d66-146">You can enter this information in hello `appsettings.json` file of hello [sample application](https://github.com/Azure-Samples/search-dotnet-getting-started/tree/master/DotNetHowTo).</span></span>

```csharp
private static SearchServiceClient CreateSearchServiceClient(IConfigurationRoot configuration)
{
    string searchServiceName = configuration["SearchServiceName"];
    string adminApiKey = configuration["SearchServiceAdminApiKey"];

    SearchServiceClient serviceClient = new SearchServiceClient(searchServiceName, new SearchCredentials(adminApiKey));
    return serviceClient;
}
```

> [!NOTE]
> <span data-ttu-id="14d66-147">Om du anger en felaktig nyckel (till exempel en fråga nyckel som där en administratör för krävdes) hello `SearchServiceClient` genereras en `CloudException` med hello fel meddelandet ”otillåtna” hello första gången du anropa en åtgärdsmetod, som `Indexes.Create`.</span><span class="sxs-lookup"><span data-stu-id="14d66-147">If you provide an incorrect key (for example, a query key where an admin key was required), hello `SearchServiceClient` will throw a `CloudException` with hello error message "Forbidden" hello first time you call an operation method on it, such as `Indexes.Create`.</span></span> <span data-ttu-id="14d66-148">Om detta händer tooyou dubbelkolla våra API-nyckel.</span><span class="sxs-lookup"><span data-stu-id="14d66-148">If this happens tooyou, double-check our API key.</span></span>
> 
> 

<span data-ttu-id="14d66-149">hello anropa nästa raderna metoder toocreate ett index med namnet ”hotell” ta bort den först om det redan finns.</span><span class="sxs-lookup"><span data-stu-id="14d66-149">hello next few lines call methods toocreate an index named "hotels", deleting it first if it already exists.</span></span> <span data-ttu-id="14d66-150">Vi kommer att gå igenom dessa metoder lite senare.</span><span class="sxs-lookup"><span data-stu-id="14d66-150">We will walk through these methods a little later.</span></span>

```csharp
Console.WriteLine("{0}", "Deleting index...\n");
DeleteHotelsIndexIfExists(serviceClient);

Console.WriteLine("{0}", "Creating index...\n");
CreateHotelsIndex(serviceClient);
```

<span data-ttu-id="14d66-151">Därefter måste hello index toobe fylls i.</span><span class="sxs-lookup"><span data-stu-id="14d66-151">Next, hello index needs toobe populated.</span></span> <span data-ttu-id="14d66-152">toodo detta, behöver vi en `SearchIndexClient`.</span><span class="sxs-lookup"><span data-stu-id="14d66-152">toodo this, we will need a `SearchIndexClient`.</span></span> <span data-ttu-id="14d66-153">Det finns två sätt tooobtain en: genom att skapa den eller genom att anropa `Indexes.GetClient` på hello `SearchServiceClient`.</span><span class="sxs-lookup"><span data-stu-id="14d66-153">There are two ways tooobtain one: by constructing it, or by calling `Indexes.GetClient` on hello `SearchServiceClient`.</span></span> <span data-ttu-id="14d66-154">Vi använder hello senare i informationssyfte.</span><span class="sxs-lookup"><span data-stu-id="14d66-154">We use hello latter for convenience.</span></span>

```csharp
ISearchIndexClient indexClient = serviceClient.Indexes.GetClient("hotels");
```

> [!NOTE]
> <span data-ttu-id="14d66-155">I ett typiskt sökprogram hanteras indexhanteringen och ifyllningen av en separat komponent från sökfrågorna.</span><span class="sxs-lookup"><span data-stu-id="14d66-155">In a typical search application, index management and population is handled by a separate component from search queries.</span></span> <span data-ttu-id="14d66-156">`Indexes.GetClient`är praktiskt för att fylla ett index eftersom du sparar du hello problem för att ge en annan `SearchCredentials`.</span><span class="sxs-lookup"><span data-stu-id="14d66-156">`Indexes.GetClient` is convenient for populating an index because it saves you hello trouble of providing another `SearchCredentials`.</span></span> <span data-ttu-id="14d66-157">Detta åstadkoms genom att skicka hello admin-nyckel som du använt toocreate hello `SearchServiceClient` toohello nya `SearchIndexClient`.</span><span class="sxs-lookup"><span data-stu-id="14d66-157">It does this by passing hello admin key that you used toocreate hello `SearchServiceClient` toohello new `SearchIndexClient`.</span></span> <span data-ttu-id="14d66-158">Men i hello en del av programmet som frågor körs, är det bättre toocreate hello `SearchIndexClient` direkt så att du kan skicka in en nyckel för frågan i stället för en administrationsnyckel.</span><span class="sxs-lookup"><span data-stu-id="14d66-158">However, in hello part of your application that executes queries, it is better toocreate hello `SearchIndexClient` directly so that you can pass in a query key instead of an admin key.</span></span> <span data-ttu-id="14d66-159">Detta är konsekventa med hello principen om minsta behörighet och hjälper toomake programmet säkrare.</span><span class="sxs-lookup"><span data-stu-id="14d66-159">This is consistent with hello principle of least privilege and will help toomake your application more secure.</span></span> <span data-ttu-id="14d66-160">Du hittar mer information om administratör och fråga nycklar [här](https://docs.microsoft.com/rest/api/searchservice/#authentication-and-authorization).</span><span class="sxs-lookup"><span data-stu-id="14d66-160">You can find out more about admin keys and query keys [here](https://docs.microsoft.com/rest/api/searchservice/#authentication-and-authorization).</span></span>
> 
> 

<span data-ttu-id="14d66-161">Nu när vi har en `SearchIndexClient`, fyller vi hello index.</span><span class="sxs-lookup"><span data-stu-id="14d66-161">Now that we have a `SearchIndexClient`, we can populate hello index.</span></span> <span data-ttu-id="14d66-162">Detta görs med en annan metod som går igenom senare.</span><span class="sxs-lookup"><span data-stu-id="14d66-162">This is done by another method that we will walk through later.</span></span>

```csharp
Console.WriteLine("{0}", "Uploading documents...\n");
UploadDocuments(indexClient);
```

<span data-ttu-id="14d66-163">Slutligen vi köra några sökningar och visa resultat som hello.</span><span class="sxs-lookup"><span data-stu-id="14d66-163">Finally, we execute a few search queries and display hello results.</span></span> <span data-ttu-id="14d66-164">Den här gången vi använder en annan `SearchIndexClient`:</span><span class="sxs-lookup"><span data-stu-id="14d66-164">This time we use a different `SearchIndexClient`:</span></span>

```csharp
ISearchIndexClient indexClientForQueries = CreateSearchIndexClient(configuration);

RunQueries(indexClientForQueries);
```

<span data-ttu-id="14d66-165">Vi ska titta närmare på hello `RunQueries` metoden senare.</span><span class="sxs-lookup"><span data-stu-id="14d66-165">We will take a closer look at hello `RunQueries` method later.</span></span> <span data-ttu-id="14d66-166">Här är hello kod toocreate hello nya `SearchIndexClient`:</span><span class="sxs-lookup"><span data-stu-id="14d66-166">Here is hello code toocreate hello new `SearchIndexClient`:</span></span>

```csharp
private static SearchIndexClient CreateSearchIndexClient(IConfigurationRoot configuration)
{
    string searchServiceName = configuration["SearchServiceName"];
    string queryApiKey = configuration["SearchServiceQueryApiKey"];

    SearchIndexClient indexClient = new SearchIndexClient(searchServiceName, "hotels", new SearchCredentials(queryApiKey));
    return indexClient;
}
```

<span data-ttu-id="14d66-167">Nu vi använder en nyckel för frågan eftersom vi inte behöver skrivbehörighet toohello index.</span><span class="sxs-lookup"><span data-stu-id="14d66-167">This time we use a query key since we do not need write access toohello index.</span></span> <span data-ttu-id="14d66-168">Du kan ange den här informationen i hello `appsettings.json` -filen för hello [exempelprogram](https://github.com/Azure-Samples/search-dotnet-getting-started/tree/master/DotNetHowTo).</span><span class="sxs-lookup"><span data-stu-id="14d66-168">You can enter this information in hello `appsettings.json` file of hello [sample application](https://github.com/Azure-Samples/search-dotnet-getting-started/tree/master/DotNetHowTo).</span></span>

<span data-ttu-id="14d66-169">Om du kör det här programmet med ett giltigt tjänstnamn och API-nycklar hello utdata ska se ut så här:</span><span class="sxs-lookup"><span data-stu-id="14d66-169">If you run this application with a valid service name and API keys, hello output should look like this:</span></span>

    Deleting index...
    
    Creating index...
    
    Uploading documents...
    
    Waiting for documents toobe indexed...
    
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
    
    Complete.  Press any key tooend application...

<span data-ttu-id="14d66-170">hello fullständig programs källkod för hello tillhandahålls hello slutet av den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="14d66-170">hello full source code of hello application is provided at hello end of this article.</span></span>

<span data-ttu-id="14d66-171">Vi ska ta en närmare titt på hello metoderna anropas av `Main`.</span><span class="sxs-lookup"><span data-stu-id="14d66-171">Next, we will take a closer look at each of hello methods called by `Main`.</span></span>

### <a name="creating-an-index"></a><span data-ttu-id="14d66-172">Skapa ett index</span><span class="sxs-lookup"><span data-stu-id="14d66-172">Creating an index</span></span>
<span data-ttu-id="14d66-173">När du har skapat en `SearchServiceClient`, hello nästa sak `Main` har är delete hello ”hotell” index om den redan finns.</span><span class="sxs-lookup"><span data-stu-id="14d66-173">After creating a `SearchServiceClient`, hello next thing `Main` does is delete hello "hotels" index if it already exists.</span></span> <span data-ttu-id="14d66-174">Som görs genom att följa metoden hello:</span><span class="sxs-lookup"><span data-stu-id="14d66-174">That is done by hello following method:</span></span>

```csharp
private static void DeleteHotelsIndexIfExists(SearchServiceClient serviceClient)
{
    if (serviceClient.Indexes.Exists("hotels"))
    {
        serviceClient.Indexes.Delete("hotels");
    }
}
```

<span data-ttu-id="14d66-175">Den här metoden använder hello anges `SearchServiceClient` toocheck om finns hello index och ta bort den.</span><span class="sxs-lookup"><span data-stu-id="14d66-175">This method uses hello given `SearchServiceClient` toocheck if hello index exists, and if so, delete it.</span></span>

> [!NOTE]
> <span data-ttu-id="14d66-176">hello exempelkoden i den här artikeln använder hello synkrona metoder för hello Azure Search .NET SDK för enkelhetens skull.</span><span class="sxs-lookup"><span data-stu-id="14d66-176">hello example code in this article uses hello synchronous methods of hello Azure Search .NET SDK for simplicity.</span></span> <span data-ttu-id="14d66-177">Vi rekommenderar att du använder hello asynkrona metoder i ditt eget program tookeep dem skalbar och svarstid.</span><span class="sxs-lookup"><span data-stu-id="14d66-177">We recommend that you use hello asynchronous methods in your own applications tookeep them scalable and responsive.</span></span> <span data-ttu-id="14d66-178">Till exempel i hello metod du kan använda `ExistsAsync` och `DeleteAsync` i stället för `Exists` och `Delete`.</span><span class="sxs-lookup"><span data-stu-id="14d66-178">For example, in hello method above you could use `ExistsAsync` and `DeleteAsync` instead of `Exists` and `Delete`.</span></span>
> 
> 

<span data-ttu-id="14d66-179">Nästa `Main` skapar ett nytt ”hotell” index genom att anropa den här metoden:</span><span class="sxs-lookup"><span data-stu-id="14d66-179">Next, `Main` creates a new "hotels" index by calling this method:</span></span>

```csharp
private static void CreateHotelsIndex(SearchServiceClient serviceClient)
{
    var definition = new Index()
    {
        Name = "hotels",
        Fields = FieldBuilder.BuildForType<Hotel>()
    };

    serviceClient.Indexes.Create(definition);
}
```

<span data-ttu-id="14d66-180">Den här metoden skapar en ny `Index` objekt med en lista över `Field` objekt som definierar hello schemat för ny hello-index.</span><span class="sxs-lookup"><span data-stu-id="14d66-180">This method creates a new `Index` object with a list of `Field` objects that defines hello schema of hello new index.</span></span> <span data-ttu-id="14d66-181">Varje fält har ett namn, datatyp och flera attribut som definierar dess sökbeteendet.</span><span class="sxs-lookup"><span data-stu-id="14d66-181">Each field has a name, data type, and several attributes that define its search behavior.</span></span> <span data-ttu-id="14d66-182">Hej `FieldBuilder` klassen använder reflektion toocreate en lista över `Field` objekt för hello index genom att undersöka hello allmänna egenskaper och attribut för hello anges `Hotel` modellklass.</span><span class="sxs-lookup"><span data-stu-id="14d66-182">hello `FieldBuilder` class uses reflection toocreate a list of `Field` objects for hello index by examining hello public properties and attributes of hello given `Hotel` model class.</span></span> <span data-ttu-id="14d66-183">Vi ska titta närmare på hello `Hotel` klassen vid ett senare tillfälle.</span><span class="sxs-lookup"><span data-stu-id="14d66-183">We'll take a closer look at hello `Hotel` class later on.</span></span>

> [!NOTE]
> <span data-ttu-id="14d66-184">Du kan alltid skapa hello lista över `Field` objekt direkt i stället för `FieldBuilder` om det behövs.</span><span class="sxs-lookup"><span data-stu-id="14d66-184">You can always create hello list of `Field` objects directly instead of using `FieldBuilder` if needed.</span></span> <span data-ttu-id="14d66-185">Till exempel kanske du inte vill toouse en modellklass eller du kan behöva toouse en befintlig modellklass som du inte vill toomodify genom att lägga till attribut.</span><span class="sxs-lookup"><span data-stu-id="14d66-185">For example, you may not want toouse a model class or you may need toouse an existing model class that you don't want toomodify by adding attributes.</span></span>
>
> 

<span data-ttu-id="14d66-186">Dessutom toofields, du kan också lägga till bedömningsprofil profiler, suggesters eller CORS alternativ toohello Index (dessa har uteslutits från hello exemplet planeringsaspekter).</span><span class="sxs-lookup"><span data-stu-id="14d66-186">In addition toofields, you can also add scoring profiles, suggesters, or CORS options toohello Index (these are omitted from hello sample for brevity).</span></span> <span data-ttu-id="14d66-187">Du hittar mer information om hello-objektet och dess komponenter som ingår i hello [SDK referens](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.index#microsoft_azure_search_models_index)samt som i hello [Azure Search REST API-referensen](https://docs.microsoft.com/rest/api/searchservice/).</span><span class="sxs-lookup"><span data-stu-id="14d66-187">You can find more information about hello Index object and its constituent parts in hello [SDK reference](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.index#microsoft_azure_search_models_index), as well as in hello [Azure Search REST API reference](https://docs.microsoft.com/rest/api/searchservice/).</span></span>

### <a name="populating-hello-index"></a><span data-ttu-id="14d66-188">Fylla hello index</span><span class="sxs-lookup"><span data-stu-id="14d66-188">Populating hello index</span></span>
<span data-ttu-id="14d66-189">hello nästa steg i `Main` är toopopulate hello nyskapade index.</span><span class="sxs-lookup"><span data-stu-id="14d66-189">hello next step in `Main` is toopopulate hello newly-created index.</span></span> <span data-ttu-id="14d66-190">Detta görs i hello följande metod:</span><span class="sxs-lookup"><span data-stu-id="14d66-190">This is done in hello following method:</span></span>

```csharp
private static void UploadDocuments(ISearchIndexClient indexClient)
{
    var hotels = new Hotel[]
    {
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
        },
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
        },
        new Hotel() 
        { 
            HotelId = "3", 
            BaseRate = 129.99,
            Description = "Close tootown hall and hello river"
        }
    };

    var batch = IndexBatch.Upload(hotels);

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
}
```

<span data-ttu-id="14d66-191">Den här metoden har fyra delar.</span><span class="sxs-lookup"><span data-stu-id="14d66-191">This method has four parts.</span></span> <span data-ttu-id="14d66-192">hello först skapar en matris med `Hotel` objekt som ska användas som indata tooupload toohello indexet.</span><span class="sxs-lookup"><span data-stu-id="14d66-192">hello first creates an array of `Hotel` objects that will serve as our input data tooupload toohello index.</span></span> <span data-ttu-id="14d66-193">Dessa data är hårdkodat för enkelhetens skull.</span><span class="sxs-lookup"><span data-stu-id="14d66-193">This data is hard-coded for simplicity.</span></span> <span data-ttu-id="14d66-194">I ditt eget program kommer troligen dina data från en extern datakälla, till exempel en SQL-databas.</span><span class="sxs-lookup"><span data-stu-id="14d66-194">In your own application, your data will likely come from an external data source such as a SQL database.</span></span>

<span data-ttu-id="14d66-195">hello andra delen skapar en `IndexBatch` som innehåller hello dokument.</span><span class="sxs-lookup"><span data-stu-id="14d66-195">hello second part creates an `IndexBatch` containing hello documents.</span></span> <span data-ttu-id="14d66-196">Du anger hello åtgärden som du vill tooapply toohello batch hello tidpunkt då du skapar det, i det här fallet genom att anropa `IndexBatch.Upload`.</span><span class="sxs-lookup"><span data-stu-id="14d66-196">You specify hello operation you want tooapply toohello batch at hello time you create it, in this case by calling `IndexBatch.Upload`.</span></span> <span data-ttu-id="14d66-197">hello batch är överförda toohello Azure Search index av hello `Documents.Index` metod.</span><span class="sxs-lookup"><span data-stu-id="14d66-197">hello batch is then uploaded toohello Azure Search index by hello `Documents.Index` method.</span></span>

> [!NOTE]
> <span data-ttu-id="14d66-198">I det här exemplet vi bara ladda upp dokument.</span><span class="sxs-lookup"><span data-stu-id="14d66-198">In this example, we are just uploading documents.</span></span> <span data-ttu-id="14d66-199">Om du vill använda toomerge ändringar i befintliga dokument eller ta bort dokument kan du skapa batchar genom att anropa `IndexBatch.Merge`, `IndexBatch.MergeOrUpload`, eller `IndexBatch.Delete` i stället.</span><span class="sxs-lookup"><span data-stu-id="14d66-199">If you wanted toomerge changes into existing documents or delete documents, you could create batches by calling `IndexBatch.Merge`, `IndexBatch.MergeOrUpload`, or `IndexBatch.Delete` instead.</span></span> <span data-ttu-id="14d66-200">Du kan också blanda olika åtgärder i en enda grupp genom att anropa `IndexBatch.New`, som tar en samling `IndexAction` objekt, som talar om för Azure Search tooperform en viss åtgärd i ett dokument.</span><span class="sxs-lookup"><span data-stu-id="14d66-200">You can also mix different operations in a single batch by calling `IndexBatch.New`, which takes a collection of `IndexAction` objects, each of which tells Azure Search tooperform a particular operation on a document.</span></span> <span data-ttu-id="14d66-201">Du kan skapa varje `IndexAction` med åtgärden genom att anropa hello motsvarande metod som `IndexAction.Merge`, `IndexAction.Upload`och så vidare.</span><span class="sxs-lookup"><span data-stu-id="14d66-201">You can create each `IndexAction` with its own operation by calling hello corresponding method such as `IndexAction.Merge`, `IndexAction.Upload`, and so on.</span></span>
> 
> 

<span data-ttu-id="14d66-202">hello tredje delen av den här metoden är ett catch-block som hanterar ett viktigt fel ärende för indexering.</span><span class="sxs-lookup"><span data-stu-id="14d66-202">hello third part of this method is a catch block that handles an important error case for indexing.</span></span> <span data-ttu-id="14d66-203">Om din Azure Search-tjänst misslyckas tooindex vissa hello dokument i hello batch en `IndexBatchException` genereras av `Documents.Index`.</span><span class="sxs-lookup"><span data-stu-id="14d66-203">If your Azure Search service fails tooindex some of hello documents in hello batch, an `IndexBatchException` is thrown by `Documents.Index`.</span></span> <span data-ttu-id="14d66-204">Detta kan inträffa om du indexerar dokument när tjänsten är hårt belastad.</span><span class="sxs-lookup"><span data-stu-id="14d66-204">This can happen if you are indexing documents while your service is under heavy load.</span></span> <span data-ttu-id="14d66-205">**Vi rekommenderar starkt att du uttryckligen hanterar den här situationen i din kod.**</span><span class="sxs-lookup"><span data-stu-id="14d66-205">**We strongly recommend explicitly handling this case in your code.**</span></span> <span data-ttu-id="14d66-206">Du kan fördröja och försök indexering hello-dokument som misslyckades du kan logga in och fortsätta som hello exempel har eller kan du göra något annat beroende på programmets konsekvens kraven</span><span class="sxs-lookup"><span data-stu-id="14d66-206">You can delay and then retry indexing hello documents that failed, or you can log and continue like hello sample does, or you can do something else depending on your application's data consistency requirements.</span></span>

> [!NOTE]
> <span data-ttu-id="14d66-207">Du kan använda hello `FindFailedActionsToRetry` metoden tooconstruct en ny grupp som innehåller endast hello åtgärder som misslyckades i ett tidigare anrop för`Index`.</span><span class="sxs-lookup"><span data-stu-id="14d66-207">You can use hello `FindFailedActionsToRetry` method tooconstruct a new batch containing only hello actions that failed in a previous call too`Index`.</span></span> <span data-ttu-id="14d66-208">hello metoden dokumenteras [här](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.indexbatchexception#Microsoft_Azure_Search_IndexBatchException_FindFailedActionsToRetry_Microsoft_Azure_Search_Models_IndexBatch_System_String_) och det finns en beskrivning av hur tooproperly använder den [på StackOverflow](http://stackoverflow.com/questions/40012885/azure-search-net-sdk-how-to-use-findfailedactionstoretry).</span><span class="sxs-lookup"><span data-stu-id="14d66-208">hello method is documented [here](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.indexbatchexception#Microsoft_Azure_Search_IndexBatchException_FindFailedActionsToRetry_Microsoft_Azure_Search_Models_IndexBatch_System_String_) and there is a discussion of how tooproperly use it [on StackOverflow](http://stackoverflow.com/questions/40012885/azure-search-net-sdk-how-to-use-findfailedactionstoretry).</span></span>
>
>

<span data-ttu-id="14d66-209">Slutligen hello `UploadDocuments` metoden fördröjningar i två sekunder.</span><span class="sxs-lookup"><span data-stu-id="14d66-209">Finally, hello `UploadDocuments` method delays for two seconds.</span></span> <span data-ttu-id="14d66-210">Indexering sker asynkront i Azure Search-tjänsten så hello exempelprogrammet måste toowait en kort tid tooensure att hello dokument är tillgängliga för sökning.</span><span class="sxs-lookup"><span data-stu-id="14d66-210">Indexing happens asynchronously in your Azure Search service, so hello sample application needs toowait a short time tooensure that hello documents are available for searching.</span></span> <span data-ttu-id="14d66-211">Fördröjningar som den här är normalt endast nödvändiga i demonstrationer, tester och exempelprogram.</span><span class="sxs-lookup"><span data-stu-id="14d66-211">Delays like this are typically only necessary in demos, tests, and sample applications.</span></span>

#### <a name="how-hello-net-sdk-handles-documents"></a><span data-ttu-id="14d66-212">Hanteringen av dokument i hello .NET SDK</span><span class="sxs-lookup"><span data-stu-id="14d66-212">How hello .NET SDK handles documents</span></span>
<span data-ttu-id="14d66-213">Du kanske undrar hur hello Azure Search .NET SDK är kan tooupload instanser av en användardefinierad klass som `Hotel` toohello index.</span><span class="sxs-lookup"><span data-stu-id="14d66-213">You may be wondering how hello Azure Search .NET SDK is able tooupload instances of a user-defined class like `Hotel` toohello index.</span></span> <span data-ttu-id="14d66-214">toohelp besvara frågan, ska vi titta på hello `Hotel` klass:</span><span class="sxs-lookup"><span data-stu-id="14d66-214">toohelp answer that question, let's look at hello `Hotel` class:</span></span>

```csharp
using System;
using Microsoft.Azure.Search;
using Microsoft.Azure.Search.Models;
using Microsoft.Spatial;
using Newtonsoft.Json;

// hello SerializePropertyNamesAsCamelCase attribute is defined in hello Azure Search .NET SDK.
// It ensures that Pascal-case property names in hello model class are mapped toocamel-case
// field names in hello index.
[SerializePropertyNamesAsCamelCase]
public partial class Hotel
{
    [System.ComponentModel.DataAnnotations.Key]
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
}
```

<span data-ttu-id="14d66-215">hello först öppna toonotice är att varje offentlig egenskap för `Hotel` motsvarar tooa fält i hello indexdefinitionen, men med en viktig skillnad: hello varje fält börjar med en gemen bokstav (”slitbanor fallet”) vid hello namnet på varje offentlig Egenskapen för `Hotel` börjar med en versal (”Pascal fall”).</span><span class="sxs-lookup"><span data-stu-id="14d66-215">hello first thing toonotice is that each public property of `Hotel` corresponds tooa field in hello index definition, but with one crucial difference: hello name of each field starts with a lower-case letter ("camel case"), while hello name of each public property of `Hotel` starts with an upper-case letter ("Pascal case").</span></span> <span data-ttu-id="14d66-216">Det här är ett vanligt scenario i .NET-program som utför databindning där hello målschemat är utanför hello kontroll över hello programutvecklaren.</span><span class="sxs-lookup"><span data-stu-id="14d66-216">This is a common scenario in .NET applications that perform data-binding where hello target schema is outside hello control of hello application developer.</span></span> <span data-ttu-id="14d66-217">Du kan se hello SDK toomap hello egenskapen namn toocamel fall automatiskt med hello snarare än att tooviolate hello .NET naming riktlinjer genom att egenskapen namn slitbanor skiftläge `[SerializePropertyNamesAsCamelCase]` attribut.</span><span class="sxs-lookup"><span data-stu-id="14d66-217">Rather than having tooviolate hello .NET naming guidelines by making property names camel-case, you can tell hello SDK toomap hello property names toocamel-case automatically with hello `[SerializePropertyNamesAsCamelCase]` attribute.</span></span>

> [!NOTE]
> <span data-ttu-id="14d66-218">hello Azure Search .NET SDK använder hello [NewtonSoft JSON.NET](http://www.newtonsoft.com/json/help/html/Introduction.htm) biblioteket tooserialize och deserialisera din anpassade modellen objekt tooand från JSON.</span><span class="sxs-lookup"><span data-stu-id="14d66-218">hello Azure Search .NET SDK uses hello [NewtonSoft JSON.NET](http://www.newtonsoft.com/json/help/html/Introduction.htm) library tooserialize and deserialize your custom model objects tooand from JSON.</span></span> <span data-ttu-id="14d66-219">Du kan anpassa den här serialiseringen om det behövs.</span><span class="sxs-lookup"><span data-stu-id="14d66-219">You can customize this serialization if needed.</span></span> <span data-ttu-id="14d66-220">Mer information finns i [anpassad serialisering med JSON.NET](#JsonDotNet).</span><span class="sxs-lookup"><span data-stu-id="14d66-220">For more details, see [Custom Serialization with JSON.NET](#JsonDotNet).</span></span>
> 
> 

<span data-ttu-id="14d66-221">hello andra sak toonotice är hello attribut som `IsFilterable`, `IsSearchable`, `Key`, och `Analyzer` som skapa snygga varje offentlig egenskap.</span><span class="sxs-lookup"><span data-stu-id="14d66-221">hello second thing toonotice are hello attributes such as `IsFilterable`, `IsSearchable`, `Key`, and `Analyzer` that decorate each public property.</span></span> <span data-ttu-id="14d66-222">Dessa attribut mappar direkt toohello [motsvarande attribut för hello Azure Search index](https://docs.microsoft.com/rest/api/searchservice/create-index#request).</span><span class="sxs-lookup"><span data-stu-id="14d66-222">These attributes map directly toohello [corresponding attributes of hello Azure Search index](https://docs.microsoft.com/rest/api/searchservice/create-index#request).</span></span> <span data-ttu-id="14d66-223">Hej `FieldBuilder` klassen använder dessa tooconstruct fältdefinitioner för hello index.</span><span class="sxs-lookup"><span data-stu-id="14d66-223">hello `FieldBuilder` class uses these tooconstruct field definitions for hello index.</span></span>

<span data-ttu-id="14d66-224">hello tredje viktigt om hello `Hotel` klass är hello datatyperna hello offentliga egenskaper.</span><span class="sxs-lookup"><span data-stu-id="14d66-224">hello third important thing about hello `Hotel` class are hello data types of hello public properties.</span></span> <span data-ttu-id="14d66-225">hello .NET-typer av de här egenskaperna mappa tootheir motsvarande fälttyp i hello indexdefinitionen.</span><span class="sxs-lookup"><span data-stu-id="14d66-225">hello .NET types of  these properties map tootheir equivalent field types in hello index definition.</span></span> <span data-ttu-id="14d66-226">Till exempel hello `Category` strängegenskap mappar toohello `category` fält som är av typen `Edm.String`.</span><span class="sxs-lookup"><span data-stu-id="14d66-226">For example, hello `Category` string property maps toohello `category` field, which is of type `Edm.String`.</span></span> <span data-ttu-id="14d66-227">Det finns liknande typ mappningar mellan `bool?` och `Edm.Boolean`, `DateTimeOffset?` och `Edm.DateTimeOffset`, etc. hello särskilda regler för mappning hello dokumenteras med hello `Documents.Get` metod i hello [Azure Search .NET SDK Referens för](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.idocumentsoperations#Microsoft_Azure_Search_IDocumentsOperations_GetWithHttpMessagesAsync__1_System_String_System_Collections_Generic_IEnumerable_System_String__Microsoft_Azure_Search_Models_SearchRequestOptions_System_Collections_Generic_Dictionary_System_String_System_Collections_Generic_List_System_String___System_Threading_CancellationToken_).</span><span class="sxs-lookup"><span data-stu-id="14d66-227">There are similar type mappings between `bool?` and `Edm.Boolean`, `DateTimeOffset?` and `Edm.DateTimeOffset`, etc. hello specific rules for hello type mapping are documented with hello `Documents.Get` method in hello [Azure Search .NET SDK reference](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.idocumentsoperations#Microsoft_Azure_Search_IDocumentsOperations_GetWithHttpMessagesAsync__1_System_String_System_Collections_Generic_IEnumerable_System_String__Microsoft_Azure_Search_Models_SearchRequestOptions_System_Collections_Generic_Dictionary_System_String_System_Collections_Generic_List_System_String___System_Threading_CancellationToken_).</span></span> <span data-ttu-id="14d66-228">Hej `FieldBuilder` klassen tar hand om mappningen för dig, men det kan fortfarande vara användbara toounderstand om du behöver tootroubleshoot problemen serialisering.</span><span class="sxs-lookup"><span data-stu-id="14d66-228">hello `FieldBuilder` class takes care of this mapping for you, but it can still be helpful toounderstand in case you need tootroubleshoot any serialization issues.</span></span>

<span data-ttu-id="14d66-229">Den här möjligheten toouse egna klasser som dokument fungerar i båda riktningarna; Du kan också hämta sökresultat och hello SDK automatiskt deserialisera dem tooa typ du väljer, som vi kommer att se i hello nästa avsnitt.</span><span class="sxs-lookup"><span data-stu-id="14d66-229">This ability toouse your own classes as documents works in both directions; You can also retrieve search results and have hello SDK automatically deserialize them tooa type of your choice, as we will see in hello next section.</span></span>

> [!NOTE]
> <span data-ttu-id="14d66-230">hello Azure Search .NET SDK stöder också dynamiskt skrev dokument med hjälp av hello `Document` klassen, som är en nyckel/värde-mappning av toofield värdena för fältet namn.</span><span class="sxs-lookup"><span data-stu-id="14d66-230">hello Azure Search .NET SDK also supports dynamically-typed documents using hello `Document` class, which is a key/value mapping of field names toofield values.</span></span> <span data-ttu-id="14d66-231">Detta är användbart i scenarier där du inte vet hello indexeringsschema i designläge, eller om det skulle vara olämplig toobind toospecific modellen klasser.</span><span class="sxs-lookup"><span data-stu-id="14d66-231">This is useful in scenarios where you don't know hello index schema at design-time, or where it would be inconvenient toobind toospecific model classes.</span></span> <span data-ttu-id="14d66-232">Alla hello metoder i hello SDK som handlar om dokument har överlagringar som fungerar med hello `Document` klass, samt strikt typkontroll överlagringar som har en generisk typparameter.</span><span class="sxs-lookup"><span data-stu-id="14d66-232">All hello methods in hello SDK that deal with documents have overloads that work with hello `Document` class, as well as strongly-typed overloads that take a generic type parameter.</span></span> <span data-ttu-id="14d66-233">Endast hello senare används i hello exempelkoden i den här självstudiekursen.</span><span class="sxs-lookup"><span data-stu-id="14d66-233">Only hello latter are used in hello sample code in this tutorial.</span></span> <span data-ttu-id="14d66-234">Hej `Document` klassen ärver från `Dictionary<string, object>`.</span><span class="sxs-lookup"><span data-stu-id="14d66-234">hello `Document` class inherits from `Dictionary<string, object>`.</span></span> <span data-ttu-id="14d66-235">Du kan hitta annan information [här](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.document#microsoft_azure_search_models_document).</span><span class="sxs-lookup"><span data-stu-id="14d66-235">You can find other details [here](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.document#microsoft_azure_search_models_document).</span></span>
> 
> 

<span data-ttu-id="14d66-236">**Varför du bör använda datatyper som kan ha värdet null**</span><span class="sxs-lookup"><span data-stu-id="14d66-236">**Why you should use nullable data types**</span></span>

<span data-ttu-id="14d66-237">När du skapar egna modellen klasser toomap tooan Azure Search index, rekommenderar vi deklarerar egenskaper för värdetyper som `bool` och `int` toobe kan ha värdet null (till exempel `bool?` i stället för `bool`).</span><span class="sxs-lookup"><span data-stu-id="14d66-237">When designing your own model classes toomap tooan Azure Search index, we recommend declaring properties of value types such as `bool` and `int` toobe nullable (for example, `bool?` instead of `bool`).</span></span> <span data-ttu-id="14d66-238">Om du använder en icke-nullbar egenskapen, det finns också**garantera** att inga dokument i indexet innehåller ett null-värde för hello motsvarande fält.</span><span class="sxs-lookup"><span data-stu-id="14d66-238">If you use a non-nullable property, you have too**guarantee** that no documents in your index contain a null value for hello corresponding field.</span></span> <span data-ttu-id="14d66-239">Varken hello SDK eller hello Azure Search-tjänsten kan du tooenforce detta.</span><span class="sxs-lookup"><span data-stu-id="14d66-239">Neither hello SDK nor hello Azure Search service will help you tooenforce this.</span></span>

<span data-ttu-id="14d66-240">Detta är inte bara ett hypotetiskt problem: Tänk dig ett scenario där du lägger till ett nytt fält tooan befintliga index som är av typen `Edm.Int32`.</span><span class="sxs-lookup"><span data-stu-id="14d66-240">This is not just a hypothetical concern: Imagine a scenario where you add a new field tooan existing index that is of type `Edm.Int32`.</span></span> <span data-ttu-id="14d66-241">När du har uppdaterat hello indexdefinitionen har alla dokument som ett null-värde för det nya fältet (eftersom alla typer inte kan vara null i Azure Search).</span><span class="sxs-lookup"><span data-stu-id="14d66-241">After updating hello index definition, all documents will have a null value for that new field (since all types are nullable in Azure Search).</span></span> <span data-ttu-id="14d66-242">Om du använder en modellklass sedan med en icke-nullbar `int` -egenskapen för det här fältet får du en `JsonSerializationException` så här när du försöker tooretrieve dokument:</span><span class="sxs-lookup"><span data-stu-id="14d66-242">If you then use a model class with a non-nullable `int` property for that field, you will get a `JsonSerializationException` like this when trying tooretrieve documents:</span></span>

    Error converting value {null} tootype 'System.Int32'. Path 'IntValue'.

<span data-ttu-id="14d66-243">Av den anledningen rekommenderar vi att du använder nullbara typer i dina modellklasser som bästa praxis.</span><span class="sxs-lookup"><span data-stu-id="14d66-243">For this reason, we recommend that you use nullable types in your model classes as a best practice.</span></span>

<a name="JsonDotNet"></a>

#### <a name="custom-serialization-with-jsonnet"></a><span data-ttu-id="14d66-244">Anpassad serialisering med JSON.NET</span><span class="sxs-lookup"><span data-stu-id="14d66-244">Custom Serialization with JSON.NET</span></span>
<span data-ttu-id="14d66-245">hello SDK använder JSON.NET för serialisering och avserialisering av dokument.</span><span class="sxs-lookup"><span data-stu-id="14d66-245">hello SDK uses JSON.NET for serializing and deserializing documents.</span></span> <span data-ttu-id="14d66-246">Du kan anpassa serialisering och avserialisering vid behov genom att definiera egna `JsonConverter` eller `IContractResolver` (se hello [JSON.NET dokumentationen](http://www.newtonsoft.com/json/help/html/Introduction.htm) för mer information).</span><span class="sxs-lookup"><span data-stu-id="14d66-246">You can customize serialization and deserialization if needed by defining your own `JsonConverter` or `IContractResolver` (see hello [JSON.NET documentation](http://www.newtonsoft.com/json/help/html/Introduction.htm) for more details).</span></span> <span data-ttu-id="14d66-247">Detta kan vara användbart när du vill tooadapt en befintlig modellklass från ditt program för användning med Azure Search och andra mer avancerade scenarier.</span><span class="sxs-lookup"><span data-stu-id="14d66-247">This can be useful when you want tooadapt an existing model class from your application for use with Azure Search, and other more advanced scenarios.</span></span> <span data-ttu-id="14d66-248">Med anpassad serialisering kan du till exempel:</span><span class="sxs-lookup"><span data-stu-id="14d66-248">For example, with custom serialization you can:</span></span>

* <span data-ttu-id="14d66-249">Ta med eller undanta vissa egenskaper hos din modellklass lagras som dokumentfält.</span><span class="sxs-lookup"><span data-stu-id="14d66-249">Include or exclude certain properties of your model class from being stored as document fields.</span></span>
* <span data-ttu-id="14d66-250">Mappa egenskapsnamn i koden och fältnamnen i ditt index.</span><span class="sxs-lookup"><span data-stu-id="14d66-250">Map between property names in your code and field names in your index.</span></span>
* <span data-ttu-id="14d66-251">Skapa anpassade attribut som kan användas för att mappa egenskaper toodocument fält.</span><span class="sxs-lookup"><span data-stu-id="14d66-251">Create custom attributes that can be used for mapping properties toodocument fields.</span></span>

<span data-ttu-id="14d66-252">Du kan hitta exempel på att implementera anpassad serialisering i hello kontroller för hello Azure Search .NET SDK på GitHub.</span><span class="sxs-lookup"><span data-stu-id="14d66-252">You can find examples of implementing custom serialization in hello unit tests for hello Azure Search .NET SDK on GitHub.</span></span> <span data-ttu-id="14d66-253">En bra utgångspunkt är [mappen](https://github.com/Azure/azure-sdk-for-net/tree/AutoRest/src/Search/Search.Tests/Tests/Models).</span><span class="sxs-lookup"><span data-stu-id="14d66-253">A good starting point is [this folder](https://github.com/Azure/azure-sdk-for-net/tree/AutoRest/src/Search/Search.Tests/Tests/Models).</span></span> <span data-ttu-id="14d66-254">Den innehåller klasser som används av hello anpassad serialisering tester.</span><span class="sxs-lookup"><span data-stu-id="14d66-254">It contains classes that are used by hello custom serialization tests.</span></span>

### <a name="searching-for-documents-in-hello-index"></a><span data-ttu-id="14d66-255">Söker efter dokument i hello index</span><span class="sxs-lookup"><span data-stu-id="14d66-255">Searching for documents in hello index</span></span>
<span data-ttu-id="14d66-256">hello sista steget i hello exempelprogrammet är toosearch för vissa dokument i hello index.</span><span class="sxs-lookup"><span data-stu-id="14d66-256">hello last step in hello sample application is toosearch for some documents in hello index.</span></span> <span data-ttu-id="14d66-257">hello följa metoden gör detta:</span><span class="sxs-lookup"><span data-stu-id="14d66-257">hello following method does this:</span></span>

```csharp
private static void RunQueries(ISearchIndexClient indexClient)
{
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
}
```

<span data-ttu-id="14d66-258">Varje gång det kör en fråga, den här metoden först skapar en ny `SearchParameters` objekt.</span><span class="sxs-lookup"><span data-stu-id="14d66-258">Each time it executes a query, this method first creates a new `SearchParameters` object.</span></span> <span data-ttu-id="14d66-259">Detta är att använda toospecify ytterligare alternativ för hello frågan som sortering, filtrering, sidindelning och faceting.</span><span class="sxs-lookup"><span data-stu-id="14d66-259">This is used toospecify additional options for hello query such as sorting, filtering, paging, and faceting.</span></span> <span data-ttu-id="14d66-260">I den här metoden vi konfigurerar hello `Filter`, `Select`, `OrderBy`, och `Top` -egenskapen för olika frågor.</span><span class="sxs-lookup"><span data-stu-id="14d66-260">In this method, we're setting hello `Filter`, `Select`, `OrderBy`, and `Top` property for different queries.</span></span> <span data-ttu-id="14d66-261">Alla hello `SearchParameters` egenskaper dokumenteras [här](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.searchparameters).</span><span class="sxs-lookup"><span data-stu-id="14d66-261">All hello `SearchParameters` properties are documented [here](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.searchparameters).</span></span>

<span data-ttu-id="14d66-262">hello nästa steg är tooactually köra hello sökfråga.</span><span class="sxs-lookup"><span data-stu-id="14d66-262">hello next step is tooactually execute hello search query.</span></span> <span data-ttu-id="14d66-263">Detta görs med hjälp av hello `Documents.Search` metod.</span><span class="sxs-lookup"><span data-stu-id="14d66-263">This is done using hello `Documents.Search` method.</span></span> <span data-ttu-id="14d66-264">För varje fråga vi skicka hello Sök text toouse som en sträng (eller `"*"` om det finns ingen Söktext), plus hello söka parametrarna som skapats tidigare.</span><span class="sxs-lookup"><span data-stu-id="14d66-264">For each query, we pass hello search text toouse as a string (or `"*"` if there is no search text), plus hello search parameters created earlier.</span></span> <span data-ttu-id="14d66-265">Vi även ange `Hotel` som hello typparameter för `Documents.Search`, som visar hello SDK toodeserialize dokument i hello sökresultat i objekt av typen `Hotel`.</span><span class="sxs-lookup"><span data-stu-id="14d66-265">We also specify `Hotel` as hello type parameter for `Documents.Search`, which tells hello SDK toodeserialize documents in hello search results into objects of type `Hotel`.</span></span>

> [!NOTE]
> <span data-ttu-id="14d66-266">Du hittar mer information om hello Sök frågesyntaxen uttryck [här](https://docs.microsoft.com/rest/api/searchservice/Simple-query-syntax-in-Azure-Search).</span><span class="sxs-lookup"><span data-stu-id="14d66-266">You can find more information about hello search query expression syntax [here](https://docs.microsoft.com/rest/api/searchservice/Simple-query-syntax-in-Azure-Search).</span></span>
> 
> 

<span data-ttu-id="14d66-267">Slutligen efter varje fråga den här metoden går exempelkoden igenom alla hello matchningar i hello sökresultat, skriva ut varje dokument toohello konsol:</span><span class="sxs-lookup"><span data-stu-id="14d66-267">Finally, after each query this method iterates through all hello matches in hello search results, printing each document toohello console:</span></span>

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

<span data-ttu-id="14d66-268">Låt oss ta en närmare titt på var och en av hello frågor i sin tur.</span><span class="sxs-lookup"><span data-stu-id="14d66-268">Let's take a closer look at each of hello queries in turn.</span></span> <span data-ttu-id="14d66-269">Här är hello kod tooexecute hello första frågan:</span><span class="sxs-lookup"><span data-stu-id="14d66-269">Here is hello code tooexecute hello first query:</span></span>

```csharp
parameters =
    new SearchParameters()
    {
        Select = new[] { "hotelName" }
    };

results = indexClient.Documents.Search<Hotel>("budget", parameters);

WriteDocuments(results);
```

<span data-ttu-id="14d66-270">I det här fallet vi söker efter hotell som matchar hello ordet ”budget” och vi vill tooget tillbaka bara hello hotell namn, som anges av hello `Select` parameter.</span><span class="sxs-lookup"><span data-stu-id="14d66-270">In this case, we're searching for hotels that match hello word "budget", and we want tooget back only hello hotel names, as specified by hello `Select` parameter.</span></span> <span data-ttu-id="14d66-271">Här följer hello resultat:</span><span class="sxs-lookup"><span data-stu-id="14d66-271">Here are hello results:</span></span>

    Name: Roach Motel

<span data-ttu-id="14d66-272">Sedan vi vill toofind hello hotell som mindre än 150 USD automatiskt varje natt kostar och bara returnera hello hotell ID och beskrivning:</span><span class="sxs-lookup"><span data-stu-id="14d66-272">Next, we want toofind hello hotels with a nightly rate of less than $150, and return only hello hotel ID and description:</span></span>

```csharp
parameters =
    new SearchParameters()
    {
        Filter = "baseRate lt 150",
        Select = new[] { "hotelId", "description" }
    };

results = indexClient.Documents.Search<Hotel>("*", parameters);

WriteDocuments(results);
```

<span data-ttu-id="14d66-273">Den här frågan använder en OData `$filter` uttryck, `baseRate lt 150`, toofilter hello dokument i hello index.</span><span class="sxs-lookup"><span data-stu-id="14d66-273">This query uses an OData `$filter` expression, `baseRate lt 150`, toofilter hello documents in hello index.</span></span> <span data-ttu-id="14d66-274">Du hittar mer information om hello OData-syntax som har stöd för Azure Search [här](https://docs.microsoft.com/rest/api/searchservice/OData-Expression-Syntax-for-Azure-Search).</span><span class="sxs-lookup"><span data-stu-id="14d66-274">You can find out more about hello OData syntax that Azure Search supports [here](https://docs.microsoft.com/rest/api/searchservice/OData-Expression-Syntax-for-Azure-Search).</span></span>

<span data-ttu-id="14d66-275">Här följer hello resultaten av hello-frågan:</span><span class="sxs-lookup"><span data-stu-id="14d66-275">Here are hello results of hello query:</span></span>

    ID: 2   Description: Cheapest hotel in town
    ID: 3   Description: Close tootown hall and hello river

<span data-ttu-id="14d66-276">Därefter vill vi toofind hello översta två hotell som har varit senast renoverade och visar hello hotell namn och datum för senaste renovering.</span><span class="sxs-lookup"><span data-stu-id="14d66-276">Next, we want toofind hello top two hotels that have been most recently renovated, and show hello hotel name and last renovation date.</span></span> <span data-ttu-id="14d66-277">Här är hello kod:</span><span class="sxs-lookup"><span data-stu-id="14d66-277">Here is hello code:</span></span> 

```csharp
parameters =
    new SearchParameters()
    {
        OrderBy = new[] { "lastRenovationDate desc" },
        Select = new[] { "hotelName", "lastRenovationDate" },
        Top = 2
    };

results = indexClient.Documents.Search<Hotel>("*", parameters);

WriteDocuments(results);
```

<span data-ttu-id="14d66-278">I detta fall kan vi använda igen OData syntax toospecify hello `OrderBy` parameter som `lastRenovationDate desc`.</span><span class="sxs-lookup"><span data-stu-id="14d66-278">In this case, we again use OData syntax toospecify hello `OrderBy` parameter as `lastRenovationDate desc`.</span></span> <span data-ttu-id="14d66-279">Vi också ange `Top` too2 tooensure vi bara hämta hello två översta dokument.</span><span class="sxs-lookup"><span data-stu-id="14d66-279">We also set `Top` too2 tooensure we only get hello top two documents.</span></span> <span data-ttu-id="14d66-280">Som tidigare kan du ange `Select` toospecify vilka fält som ska returneras.</span><span class="sxs-lookup"><span data-stu-id="14d66-280">As before, we set `Select` toospecify which fields should be returned.</span></span>

<span data-ttu-id="14d66-281">Här följer hello resultat:</span><span class="sxs-lookup"><span data-stu-id="14d66-281">Here are hello results:</span></span>

    Name: Fancy Stay        Last renovated on: 6/27/2010 12:00:00 AM +00:00
    Name: Roach Motel       Last renovated on: 4/28/1982 12:00:00 AM +00:00

<span data-ttu-id="14d66-282">Slutligen vill vi toofind alla hotell som matchar hello ordet ”översikt”:</span><span class="sxs-lookup"><span data-stu-id="14d66-282">Finally, we want toofind all hotels that match hello word "motel":</span></span>

```csharp
parameters = new SearchParameters();
results = indexClient.Documents.Search<Hotel>("motel", parameters);

WriteDocuments(results);
```

<span data-ttu-id="14d66-283">Här är hello-resultat, som omfattar alla fält eftersom vi inte har angett hello `Select` egenskapen:</span><span class="sxs-lookup"><span data-stu-id="14d66-283">And here are hello results, which include all fields since we did not specify hello `Select` property:</span></span>

    ID: 2   Base rate: 79.99        Description: Cheapest hotel in town     Description (French): Hôtel le moins cher en ville      Name: Roach Motel       Category: Budget        Tags: [motel, budget]   Parking included: yes   Smoking allowed: yes    Last renovated on: 4/28/1982 12:00:00 AM +00:00 Rating: 1/5     Location: Latitude 49.678581, longitude -122.131577

<span data-ttu-id="14d66-284">Det här steget har slutförts hello kursen, men stoppar inte här.</span><span class="sxs-lookup"><span data-stu-id="14d66-284">This step completes hello tutorial, but don't stop here.</span></span> <span data-ttu-id="14d66-285">**Nästa steg** innehåller ytterligare resurser för att lära dig mer om Azure Search.</span><span class="sxs-lookup"><span data-stu-id="14d66-285">**Next steps** provides additional resources for learning more about Azure Search.</span></span>

## <a name="next-steps"></a><span data-ttu-id="14d66-286">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="14d66-286">Next steps</span></span>
* <span data-ttu-id="14d66-287">Bläddra hello referenser för hello [.NET SDK](https://docs.microsoft.com/dotnet/api/microsoft.azure.search) och [REST API](https://docs.microsoft.com/rest/api/searchservice/).</span><span class="sxs-lookup"><span data-stu-id="14d66-287">Browse hello references for hello [.NET SDK](https://docs.microsoft.com/dotnet/api/microsoft.azure.search) and [REST API](https://docs.microsoft.com/rest/api/searchservice/).</span></span>
* <span data-ttu-id="14d66-288">Fördjupa dina kunskaper genom [videor och andra exempel och självstudier](search-video-demo-tutorial-list.md).</span><span class="sxs-lookup"><span data-stu-id="14d66-288">Deepen your knowledge through [videos and other samples and tutorials](search-video-demo-tutorial-list.md).</span></span>
* <span data-ttu-id="14d66-289">Granska [namngivningskonventioner](https://docs.microsoft.com/rest/api/searchservice/Naming-rules) toolearn hello regler för namngivning av olika objekt.</span><span class="sxs-lookup"><span data-stu-id="14d66-289">Review [naming conventions](https://docs.microsoft.com/rest/api/searchservice/Naming-rules) toolearn hello rules for naming various objects.</span></span>
* <span data-ttu-id="14d66-290">Granska [datatyper som stöds](https://docs.microsoft.com/rest/api/searchservice/Supported-data-types) i Azure Search.</span><span class="sxs-lookup"><span data-stu-id="14d66-290">Review [supported data types](https://docs.microsoft.com/rest/api/searchservice/Supported-data-types) in Azure Search.</span></span>
