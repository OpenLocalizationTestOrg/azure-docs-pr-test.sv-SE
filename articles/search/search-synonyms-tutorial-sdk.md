---
title: "aaaSynonyms Förhandsgranska kurs i Azure Search | Microsoft Docs"
description: "Lägg till hello synonymer preview funktionen tooan index i Azure Search."
services: search
manager: jhubbard
documentationcenter: 
author: HeidiSteen
ms.service: search
ms.devlang: NA
ms.workload: search
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.date: 03/31/2017
ms.author: heidist
ms.openlocfilehash: 055c1cbafb945823a3dc4da0c522db236b1d192c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="synonym-preview-c-tutorial-for-azure-search"></a><span data-ttu-id="95a5d-103">Förhandsgranska synonymer – en självstudie i C# för Azure Search</span><span class="sxs-lookup"><span data-stu-id="95a5d-103">Synonym (preview) C# tutorial for Azure Search</span></span>

<span data-ttu-id="95a5d-104">Synonymer expandera en fråga med matchande på villkor som anses vara samma sak toohello inkommande termen.</span><span class="sxs-lookup"><span data-stu-id="95a5d-104">Synonyms expand a query by matching on terms considered semantically equivalent toohello input term.</span></span> <span data-ttu-id="95a5d-105">Du kanske exempelvis vill ”bil” toomatch dokument som innehåller hello villkoren ”bil” eller ”fordon”.</span><span class="sxs-lookup"><span data-stu-id="95a5d-105">For example, you might want "car" toomatch documents containing hello terms "automobile" or "vehicle".</span></span>

<span data-ttu-id="95a5d-106">I Azure Search definieras synonymer i en *synonymmappning* enligt *mappningsregler* som associerar ekvivalenta termer.</span><span class="sxs-lookup"><span data-stu-id="95a5d-106">In Azure Search, synonyms are defined in a *synonym map*, through *mapping rules* that associate equivalent terms.</span></span> <span data-ttu-id="95a5d-107">Du kan skapa flera synonymen maps, publicera dem som en tjänsteomfattande resurs tillgängliga tooany index och referera till vilka en toouse på hello fältnivå.</span><span class="sxs-lookup"><span data-stu-id="95a5d-107">You can create multiple synonym maps, post them as a service-wide resource available tooany index, and then reference which one toouse at hello field level.</span></span> <span data-ttu-id="95a5d-108">När databasfrågan har dessutom toosearching ett index Azure Search en sökning i en synonym karta, om en anges på fälten som används i hello frågan.</span><span class="sxs-lookup"><span data-stu-id="95a5d-108">At query time, in addition toosearching an index, Azure Search does a lookup in a synonym map, if one is specified on fields used in hello query.</span></span>

> [!NOTE]
> <span data-ttu-id="95a5d-109">hello synonymer funktionen är för närvarande i Förhandsgranska och stöds i endast hello senaste förhandsversionen API och SDK-versioner (api-version = 2016-09-01-Preview, SDK version 4.x-preview).</span><span class="sxs-lookup"><span data-stu-id="95a5d-109">hello synonyms feature is currently in preview and only supported in hello latest preview API and SDK versions (api-version=2016-09-01-Preview, SDK version 4.x-preview).</span></span> <span data-ttu-id="95a5d-110">Funktionen stöds för närvarande inte på Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="95a5d-110">There is no Azure portal support at this time.</span></span> <span data-ttu-id="95a5d-111">För förhandsversioner av API:er gäller inget SLA och de här funktionerna kan ändras, så du bör inte använda dem i produktionsmiljöer.</span><span class="sxs-lookup"><span data-stu-id="95a5d-111">Preview APIs are not under SLA and preview features may change, so we do not recommend using them in production applications.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="95a5d-112">Krav</span><span class="sxs-lookup"><span data-stu-id="95a5d-112">Prerequisites</span></span>

<span data-ttu-id="95a5d-113">Krav på Självstudier innehåller hello följande:</span><span class="sxs-lookup"><span data-stu-id="95a5d-113">Tutorial requirements include hello following:</span></span>

* [<span data-ttu-id="95a5d-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="95a5d-114">Visual Studio</span></span>](https://www.visualstudio.com/downloads/)
* [<span data-ttu-id="95a5d-115">Azure Search-tjänsten</span><span class="sxs-lookup"><span data-stu-id="95a5d-115">Azure Search service</span></span>](search-create-service-portal.md)
* [<span data-ttu-id="95a5d-116">Förhandsversionen av .NET-biblioteket Microsoft.Azure.Search</span><span class="sxs-lookup"><span data-stu-id="95a5d-116">Preview version of Microsoft.Azure.Search .NET library</span></span>](https://aka.ms/search-sdk-preview)
* [<span data-ttu-id="95a5d-117">Hur toouse Azure söka från ett .NET-program</span><span class="sxs-lookup"><span data-stu-id="95a5d-117">How toouse Azure Search from a .NET Application</span></span>](https://docs.microsoft.com/azure/search/search-howto-dotnet-sdk)

## <a name="overview"></a><span data-ttu-id="95a5d-118">Översikt</span><span class="sxs-lookup"><span data-stu-id="95a5d-118">Overview</span></span>

<span data-ttu-id="95a5d-119">Före och efter frågor visar hello värdet för synonymer.</span><span class="sxs-lookup"><span data-stu-id="95a5d-119">Before-and-after queries demonstrate hello value of synonyms.</span></span> <span data-ttu-id="95a5d-120">I den här självstudien använder vi ett exempelprogram som kör frågor och returnerar resultat för ett exempelindex.</span><span class="sxs-lookup"><span data-stu-id="95a5d-120">In this tutorial, we use a sample application that executes queries and returns results on a sample index.</span></span> <span data-ttu-id="95a5d-121">hello exempelprogrammet skapar ett litet index med namnet ”hotell” fylls med två dokument.</span><span class="sxs-lookup"><span data-stu-id="95a5d-121">hello sample application creates a small index named "hotels" populated with two documents.</span></span> <span data-ttu-id="95a5d-122">hello-program körs sökfrågor med termer och fraser som inte visas i hello index, aktiverar hello synonymer funktion, och sedan problem hello samma sökningar igen.</span><span class="sxs-lookup"><span data-stu-id="95a5d-122">hello application executes search queries using terms and phrases that do not appear in hello index, enables hello synonyms feature, then issues hello same searches again.</span></span> <span data-ttu-id="95a5d-123">hello-koden nedan visar hello övergripande flödet.</span><span class="sxs-lookup"><span data-stu-id="95a5d-123">hello code below demonstrates hello overall flow.</span></span>

```csharp
  static void Main(string[] args)
  {
      SearchServiceClient serviceClient = CreateSearchServiceClient();

      Console.WriteLine("{0}", "Cleaning up resources...\n");
      CleanupResources(serviceClient);

      Console.WriteLine("{0}", "Creating index...\n");
      CreateHotelsIndex(serviceClient);

      ISearchIndexClient indexClient = serviceClient.Indexes.GetClient("hotels");

      Console.WriteLine("{0}", "Uploading documents...\n");
      UploadDocuments(indexClient);

      ISearchIndexClient indexClientForQueries = CreateSearchIndexClient();

      RunQueriesWithNonExistentTermsInIndex(indexClientForQueries);

      Console.WriteLine("{0}", "Adding synonyms...\n");
      UploadSynonyms(serviceClient);
      EnableSynonymsInHotelsIndex(serviceClient);
      Thread.Sleep(10000); // Wait for hello changes toopropagate

      RunQueriesWithNonExistentTermsInIndex(indexClientForQueries);

      Console.WriteLine("{0}", "Complete.  Press any key tooend application...\n");

      Console.ReadKey();
  }
```
<span data-ttu-id="95a5d-124">Hej steg toocreate och fylla hello exempel index förklaras i [hur toouse Azure söka från ett .NET-program](https://docs.microsoft.com/azure/search/search-howto-dotnet-sdk).</span><span class="sxs-lookup"><span data-stu-id="95a5d-124">hello steps toocreate and populate hello sample index are explained in [How toouse Azure Search from a .NET Application](https://docs.microsoft.com/azure/search/search-howto-dotnet-sdk).</span></span>

## <a name="before-queries"></a><span data-ttu-id="95a5d-125">”Före”-frågor</span><span class="sxs-lookup"><span data-stu-id="95a5d-125">"Before" queries</span></span>

<span data-ttu-id="95a5d-126">I `RunQueriesWithNonExistentTermsInIndex` kör vi sökningar med termerna ”five star”, ”internet” och ”economy AND hotel”.</span><span class="sxs-lookup"><span data-stu-id="95a5d-126">In `RunQueriesWithNonExistentTermsInIndex`, we issue search queries with "five star", "internet", and "economy AND hotel".</span></span>
```csharp
Console.WriteLine("Search hello entire index for hello phrase \"five star\":\n");
results = indexClient.Documents.Search<Hotel>("\"five star\"", parameters);
WriteDocuments(results);

Console.WriteLine("Search hello entire index for hello term 'internet':\n");
results = indexClient.Documents.Search<Hotel>("internet", parameters);
WriteDocuments(results);

Console.WriteLine("Search hello entire index for hello terms 'economy' AND 'hotel':\n");
results = indexClient.Documents.Search<Hotel>("economy AND hotel", parameters);
WriteDocuments(results);
```
<span data-ttu-id="95a5d-127">Ingen av hello två indexerade dokument innehåller hello villkor, så att vi får hello följande utdata från hello först `RunQueriesWithNonExistentTermsInIndex`.</span><span class="sxs-lookup"><span data-stu-id="95a5d-127">Neither of hello two indexed documents contain hello terms, so we get hello following output from hello first `RunQueriesWithNonExistentTermsInIndex`.</span></span>
~~~
Search hello entire index for hello phrase "five star":

no document matched

Search hello entire index for hello term 'internet':

no document matched

Search hello entire index for hello terms 'economy' AND 'hotel':

no document matched
~~~

## <a name="enable-synonyms"></a><span data-ttu-id="95a5d-128">Aktivera synonymer</span><span class="sxs-lookup"><span data-stu-id="95a5d-128">Enable synonyms</span></span>

<span data-ttu-id="95a5d-129">Att aktivera synonymer är en tvåstegsprocess.</span><span class="sxs-lookup"><span data-stu-id="95a5d-129">Enabling synonyms is a two-step process.</span></span> <span data-ttu-id="95a5d-130">Vi först definiera och överför synonymen regler och sedan konfigurera fält toouse dem.</span><span class="sxs-lookup"><span data-stu-id="95a5d-130">We first define and upload synonym rules and then configure fields toouse them.</span></span> <span data-ttu-id="95a5d-131">hello process beskrivs i `UploadSynonyms` och `EnableSynonymsInHotelsIndex`.</span><span class="sxs-lookup"><span data-stu-id="95a5d-131">hello process is outlined in `UploadSynonyms` and `EnableSynonymsInHotelsIndex`.</span></span>

1. <span data-ttu-id="95a5d-132">Lägg till en synonym kartan tooyour search-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="95a5d-132">Add a synonym map tooyour search service.</span></span> <span data-ttu-id="95a5d-133">I `UploadSynonyms`, vi definiera fyra regler i vår synonymen map 'desc synonymmap' och ladda upp toohello service.</span><span class="sxs-lookup"><span data-stu-id="95a5d-133">In `UploadSynonyms`, we define four rules in our synonym map 'desc-synonymmap' and upload toohello service.</span></span>
```csharp
    var synonymMap = new SynonymMap()
    {
        Name = "desc-synonymmap",
        Format = "solr",
        Synonyms = "hotel, motel\n
                    internet,wifi\n
                    five star=>luxury\n
                    economy,inexpensive=>budget"
    };

    serviceClient.SynonymMaps.CreateOrUpdate(synonymMap);
```
<span data-ttu-id="95a5d-134">En synonym karta måste uppfylla toohello öppen källkod standard `solr` format.</span><span class="sxs-lookup"><span data-stu-id="95a5d-134">A synonym map must conform toohello open source standard `solr` format.</span></span> <span data-ttu-id="95a5d-135">hello format beskrivs i [synonymer i Azure Search](search-synonyms.md) under hello avsnittet `Apache Solr synonym format`.</span><span class="sxs-lookup"><span data-stu-id="95a5d-135">hello format is explained in [Synonyms in Azure Search](search-synonyms.md) under hello section `Apache Solr synonym format`.</span></span>

2. <span data-ttu-id="95a5d-136">Konfigurera sökbara fält toouse hello synonymen kartan i hello indexdefinitionen.</span><span class="sxs-lookup"><span data-stu-id="95a5d-136">Configure searchable fields toouse hello synonym map in hello index definition.</span></span> <span data-ttu-id="95a5d-137">I `EnableSynonymsInHotelsIndex`, vi aktivera synonymer i två fält `category` och `tags` genom att ange hello `synonymMaps` toohello egenskapsnamn för hello nyligen har överförts synonymen kartan.</span><span class="sxs-lookup"><span data-stu-id="95a5d-137">In `EnableSynonymsInHotelsIndex`, we enable synonyms on two fields `category` and `tags` by setting hello `synonymMaps` property toohello name of hello newly uploaded synonym map.</span></span>
```csharp
  Index index = serviceClient.Indexes.Get("hotels");
  index.Fields.First(f => f.Name == "category").SynonymMaps = new[] { "desc-synonymmap" };
  index.Fields.First(f => f.Name == "tags").SynonymMaps = new[] { "desc-synonymmap" };

  serviceClient.Indexes.CreateOrUpdate(index);
```
<span data-ttu-id="95a5d-138">När du lägger till en synonymmappning behöver du inte skapa indexet på nytt.</span><span class="sxs-lookup"><span data-stu-id="95a5d-138">When you add a synonym map, index rebuilds are not required.</span></span> <span data-ttu-id="95a5d-139">Du kan lägga till en synonym kartan tooyour tjänst och ändra befintliga fältdefinitioner i alla index toouse hello nya synonymen-mappning.</span><span class="sxs-lookup"><span data-stu-id="95a5d-139">You can add a synonym map tooyour service, and then amend existing field definitions in any index toouse hello new synonym map.</span></span> <span data-ttu-id="95a5d-140">hello lägga till nya attribut har ingen inverkan på tillgänglighet av indexet.</span><span class="sxs-lookup"><span data-stu-id="95a5d-140">hello addition of new attributes has no impact on index availability.</span></span> <span data-ttu-id="95a5d-141">hello samma gäller inaktiveras synonymer för ett fält.</span><span class="sxs-lookup"><span data-stu-id="95a5d-141">hello same applies in disabling synonyms for a field.</span></span> <span data-ttu-id="95a5d-142">Du kan bara ange hello `synonymMaps` egenskapen tooan tom lista.</span><span class="sxs-lookup"><span data-stu-id="95a5d-142">You can simply set hello `synonymMaps` property tooan empty list.</span></span>
```csharp
  index.Fields.First(f => f.Name == "category").SynonymMaps = new List<string>();
```

## <a name="after-queries"></a><span data-ttu-id="95a5d-143">”Efter”-frågor</span><span class="sxs-lookup"><span data-stu-id="95a5d-143">"After" queries</span></span>

<span data-ttu-id="95a5d-144">När hello synonymen kartan överförs och hello index är uppdaterade toouse hello synonymen mappning hello andra `RunQueriesWithNonExistentTermsInIndex` anrop matar ut hello följande:</span><span class="sxs-lookup"><span data-stu-id="95a5d-144">After hello synonym map is uploaded and hello index is updated toouse hello synonym map, hello second `RunQueriesWithNonExistentTermsInIndex` call outputs hello following:</span></span>

~~~
Search hello entire index for hello phrase "five star":

Name: Fancy Stay        Category: Luxury        Tags: [pool, view, wifi, concierge]

Search hello entire index for hello term 'internet':

Name: Fancy Stay        Category: Luxury        Tags: [pool, view, wifi, concierge]

Search hello entire index for hello terms 'economy' AND 'hotel':

Name: Roach Motel       Category: Budget        Tags: [motel, budget]
~~~
<span data-ttu-id="95a5d-145">hello första frågan hittar hello dokument från hello regel `five star=>luxury`.</span><span class="sxs-lookup"><span data-stu-id="95a5d-145">hello first query finds hello document from hello rule `five star=>luxury`.</span></span> <span data-ttu-id="95a5d-146">hello andra frågan expanderar hello Sök med `internet,wifi` och hello tredje använder både `hotel, motel` och `economy,inexpensive=>budget` att hitta hello dokument de matchade.</span><span class="sxs-lookup"><span data-stu-id="95a5d-146">hello second query expands hello search using `internet,wifi` and hello third using both `hotel, motel` and `economy,inexpensive=>budget` in finding hello documents they matched.</span></span>

<span data-ttu-id="95a5d-147">Lägga till synonymer helt ändrar hello sökinställningar.</span><span class="sxs-lookup"><span data-stu-id="95a5d-147">Adding synonyms completely changes hello search experience.</span></span> <span data-ttu-id="95a5d-148">I den här självstudiekursen misslyckades hello ursprungliga frågor tooreturn meningsfulla resultaten även om hello dokument i vårt index har relevanta.</span><span class="sxs-lookup"><span data-stu-id="95a5d-148">In this tutorial, hello original queries failed tooreturn meaningful results even though hello documents in our index were relevant.</span></span> <span data-ttu-id="95a5d-149">Vi kan expandera ett index tooinclude villkor gemensam användning, utan ändringar toounderlying data i hello index genom att aktivera synonymer.</span><span class="sxs-lookup"><span data-stu-id="95a5d-149">By enabling synonyms, we can expand an index tooinclude terms in common use, with no changes toounderlying data in hello index.</span></span>

## <a name="sample-application-source-code"></a><span data-ttu-id="95a5d-150">Källkod för exempelprogrammet</span><span class="sxs-lookup"><span data-stu-id="95a5d-150">Sample application source code</span></span>
<span data-ttu-id="95a5d-151">Du kan hitta hello fullständig källkoden för hello exempelprogrammet används i denna genomgång på [GitHub](https://github.com/Azure-Samples/search-dotnet-getting-started/tree/master/DotNetHowToSynonyms).</span><span class="sxs-lookup"><span data-stu-id="95a5d-151">You can find hello full source code of hello sample application used in this walk through on [GitHub](https://github.com/Azure-Samples/search-dotnet-getting-started/tree/master/DotNetHowToSynonyms).</span></span>

## <a name="next-steps"></a><span data-ttu-id="95a5d-152">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="95a5d-152">Next steps</span></span>

* <span data-ttu-id="95a5d-153">Granska [hur toouse synonymer i Azure Search](search-synonyms.md)</span><span class="sxs-lookup"><span data-stu-id="95a5d-153">Review [How toouse synonyms in Azure Search](search-synonyms.md)</span></span>
* <span data-ttu-id="95a5d-154">Läs om [synonymer i dokumentationen till REST API](https://aka.ms/rgm6rq)</span><span class="sxs-lookup"><span data-stu-id="95a5d-154">Review [Synonyms REST API documentation](https://aka.ms/rgm6rq)</span></span>
* <span data-ttu-id="95a5d-155">Bläddra hello referenser för hello [.NET SDK](https://docs.microsoft.com/dotnet/api/microsoft.azure.search) och [REST API](https://docs.microsoft.com/rest/api/searchservice/).</span><span class="sxs-lookup"><span data-stu-id="95a5d-155">Browse hello references for hello [.NET SDK](https://docs.microsoft.com/dotnet/api/microsoft.azure.search) and [REST API](https://docs.microsoft.com/rest/api/searchservice/).</span></span>
