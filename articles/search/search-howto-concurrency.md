---
title: aaaHow toomanage samtidiga skriver tooresources i Azure Search
description: "Använd Optimistisk samtidighet tooavoid halva luften kollisioner på uppdateringar eller borttagningar tooAzure sökindexen, indexerare, -datakällor."
services: search
documentationcenter: 
author: HeidiSteen
manager: jhubbard
editor: 
tags: azure-portal
ms.assetid: 
ms.service: search
ms.devlang: 
ms.workload: search
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 07/21/2017
ms.author: heidist
ms.openlocfilehash: c061d2b5c4d2dbd0fd5633405b01ab2912fbc754
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toomanage-concurrency-in-azure-search"></a><span data-ttu-id="c3e10-103">Hur toomanage samtidighet i Azure Search</span><span class="sxs-lookup"><span data-stu-id="c3e10-103">How toomanage concurrency in Azure Search</span></span>

<span data-ttu-id="c3e10-104">När du hanterar Azure Search-resurser, till exempel index och datakällor, är det viktigt tooupdate resurser på ett säkert sätt, särskilt om resurser används samtidigt av olika komponenter i ditt program.</span><span class="sxs-lookup"><span data-stu-id="c3e10-104">When managing Azure Search resources such as indexes and data sources, it's important tooupdate resources safely, especially if resources are accessed concurrently by different components of your application.</span></span> <span data-ttu-id="c3e10-105">När två klienter samtidigt uppdaterar resurser utan samordning, är konkurrenstillstånd möjliga.</span><span class="sxs-lookup"><span data-stu-id="c3e10-105">When two clients concurrently update a resource without coordination, race conditions are possible.</span></span> <span data-ttu-id="c3e10-106">tooprevent detta, Azure Search erbjuder en *optimistisk*.</span><span class="sxs-lookup"><span data-stu-id="c3e10-106">tooprevent this, Azure Search offers an *optimistic concurrency model*.</span></span> <span data-ttu-id="c3e10-107">Det finns inga lås på en resurs.</span><span class="sxs-lookup"><span data-stu-id="c3e10-107">There are no locks on a resource.</span></span> <span data-ttu-id="c3e10-108">Det finns i stället en ETag för varje resurs som identifierar hello resursversionen så att du kan skapa förfrågningar som undviker oavsiktliga skrivs över.</span><span class="sxs-lookup"><span data-stu-id="c3e10-108">Instead, there is an ETag for every resource that identifies hello resource version so that you can craft requests that avoid accidental overwrites.</span></span>

> [!Tip]
> <span data-ttu-id="c3e10-109">Konceptuell kod i en [exempel C#-lösningen](https://github.com/Azure-Samples/search-dotnet-getting-started/tree/master/DotNetETagsExplainer) förklarar hur samtidighetskontroll fungerar i Azure Search.</span><span class="sxs-lookup"><span data-stu-id="c3e10-109">Conceptual code in a [sample C# solution](https://github.com/Azure-Samples/search-dotnet-getting-started/tree/master/DotNetETagsExplainer) explains how concurrency control works in Azure Search.</span></span> <span data-ttu-id="c3e10-110">hello kod skapar villkor som anropar samtidighetskontroll.</span><span class="sxs-lookup"><span data-stu-id="c3e10-110">hello code creates conditions that invoke concurrency control.</span></span> <span data-ttu-id="c3e10-111">Läsning hello [kodfragmentet nedan](#samplecode) räcker troligen för de flesta utvecklare, men om du vill toorun den, redigera appsettings.json tooadd hello tjänstens namn och en admin api-nyckel.</span><span class="sxs-lookup"><span data-stu-id="c3e10-111">Reading hello [code fragment below](#samplecode) is probably sufficient for most developers, but if you want toorun it, edit appsettings.json tooadd hello service name and an admin api-key.</span></span> <span data-ttu-id="c3e10-112">Tjänsten URL: en `http://myservice.search.windows.net`, hello tjänstnamnet är `myservice`.</span><span class="sxs-lookup"><span data-stu-id="c3e10-112">Given a service URL of `http://myservice.search.windows.net`, hello service name is `myservice`.</span></span>

## <a name="how-it-works"></a><span data-ttu-id="c3e10-113">Hur det fungerar</span><span class="sxs-lookup"><span data-stu-id="c3e10-113">How it works</span></span>

<span data-ttu-id="c3e10-114">Optimistisk samtidighet implementeras via åtkomst villkoret söker i API-anrop skriva tooindexes, indexerare, datakällor och synonymMap resurser.</span><span class="sxs-lookup"><span data-stu-id="c3e10-114">Optimistic concurrency is implemented through access condition checks in API calls writing tooindexes, indexers, datasources, and synonymMap resources.</span></span> 

<span data-ttu-id="c3e10-115">Alla resurser som har en [ *entitetstaggen (ETag)* ](https://en.wikipedia.org/wiki/HTTP_ETag) som innehåller versionsinformation för objektet.</span><span class="sxs-lookup"><span data-stu-id="c3e10-115">All resources have an [*entity tag (ETag)*](https://en.wikipedia.org/wiki/HTTP_ETag) that provides object version information.</span></span> <span data-ttu-id="c3e10-116">Du kan undvika samtidiga uppdateringar i ett normalt arbetsflöde genom att kontrollera hello ETag först (get, ändra lokalt, uppdatera) genom att säkerställa hello resursens ETag matchar den lokala kopian.</span><span class="sxs-lookup"><span data-stu-id="c3e10-116">By checking hello ETag first, you can avoid concurrent updates in a typical workflow (get, modify locally, update) by ensuring hello resource's ETag matches your local copy.</span></span> 

+ <span data-ttu-id="c3e10-117">hello REST API: N används en [ETag](https://docs.microsoft.com/rest/api/searchservice/common-http-request-and-response-headers-used-in-azure-search) på hello huvudet i begäran.</span><span class="sxs-lookup"><span data-stu-id="c3e10-117">hello REST API uses an [ETag](https://docs.microsoft.com/rest/api/searchservice/common-http-request-and-response-headers-used-in-azure-search) on hello request header.</span></span>
+ <span data-ttu-id="c3e10-118">hello .NET SDK anger hello ETag via ett accessCondition objekt, ange hello [If-Match | Huvudet If-Match-ingen](https://docs.microsoft.com/rest/api/searchservice/common-http-request-and-response-headers-used-in-azure-search) på hello resursen.</span><span class="sxs-lookup"><span data-stu-id="c3e10-118">hello .NET SDK sets hello ETag through an accessCondition object, setting hello [If-Match | If-Match-None header](https://docs.microsoft.com/rest/api/searchservice/common-http-request-and-response-headers-used-in-azure-search) on hello resource.</span></span> <span data-ttu-id="c3e10-119">Alla objekt som ärver från [IResourceWithETag (.NET SDK)](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.iresourcewithetag) har ett accessCondition-objekt.</span><span class="sxs-lookup"><span data-stu-id="c3e10-119">Any object inheriting from [IResourceWithETag (.NET SDK)](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.iresourcewithetag) has an accessCondition object.</span></span>

<span data-ttu-id="c3e10-120">Varje gång du uppdaterar en resurs ändras dess ETag automatiskt.</span><span class="sxs-lookup"><span data-stu-id="c3e10-120">Every time you update a resource, its ETag changes automatically.</span></span> <span data-ttu-id="c3e10-121">När du implementerar samtidighet management allt du gör är att publicera ett villkor på hello begäran om uppdatering som kräver hello fjärresursen toohave hello samma ETag som hello kopia av hello-resurs som du har ändrat på hello-klienten.</span><span class="sxs-lookup"><span data-stu-id="c3e10-121">When you implement concurrency management, all you're doing is putting a precondition on hello update request that requires hello remote resource toohave hello same ETag as hello copy of hello resource that you modified on hello client.</span></span> <span data-ttu-id="c3e10-122">Om en samtidig process har redan ändrats hello fjärresursen, hello ETag matchar inte hello villkor och hello begäran att misslyckas med HTTP 412.</span><span class="sxs-lookup"><span data-stu-id="c3e10-122">If a concurrent process has changed hello remote resource already, hello ETag will not match hello precondition and hello request will fail with HTTP 412.</span></span> <span data-ttu-id="c3e10-123">Om du använder hello .NET SDK, visar detta som en `CloudException` där hello `IsAccessConditionFailed()` metod returnerar true.</span><span class="sxs-lookup"><span data-stu-id="c3e10-123">If you're using hello .NET SDK, this manifests as a `CloudException` where hello `IsAccessConditionFailed()` extension method returns true.</span></span>

> [!Note]
> <span data-ttu-id="c3e10-124">Det finns endast en mekanism för samtidighet.</span><span class="sxs-lookup"><span data-stu-id="c3e10-124">There is only one mechanism for concurrency.</span></span> <span data-ttu-id="c3e10-125">Den används alltid oavsett vilket API används för att uppdatera resurserna.</span><span class="sxs-lookup"><span data-stu-id="c3e10-125">It's always used regardless of which API is used for resource updates.</span></span> 

<a name="samplecode"></a>
## <a name="use-cases-and-sample-code"></a><span data-ttu-id="c3e10-126">Användningsfall och exempelkod</span><span class="sxs-lookup"><span data-stu-id="c3e10-126">Use cases and sample code</span></span>

<span data-ttu-id="c3e10-127">hello följande kod visar accessCondition söker efter uppdateringsåtgärder:</span><span class="sxs-lookup"><span data-stu-id="c3e10-127">hello following code demonstrates accessCondition checks for key update operations:</span></span>

+ <span data-ttu-id="c3e10-128">Inte en uppdatering om hello resursen finns inte längre</span><span class="sxs-lookup"><span data-stu-id="c3e10-128">Fail an update if hello resource no longer exists</span></span>
+ <span data-ttu-id="c3e10-129">En uppdatering att misslyckas om hello resursversionen ändras</span><span class="sxs-lookup"><span data-stu-id="c3e10-129">Fail an update if hello resource version changes</span></span>

### <a name="sample-code-from-dotnetetagsexplainer-programhttpsgithubcomazure-samplessearch-dotnet-getting-startedtreemasterdotnetetagsexplainer"></a><span data-ttu-id="c3e10-130">Exempel på kod från [DotNetETagsExplainer program](https://github.com/Azure-Samples/search-dotnet-getting-started/tree/master/DotNetETagsExplainer)</span><span class="sxs-lookup"><span data-stu-id="c3e10-130">Sample code from [DotNetETagsExplainer program](https://github.com/Azure-Samples/search-dotnet-getting-started/tree/master/DotNetETagsExplainer)</span></span>

```
    class Program
    {
        // This sample shows how ETags work by performing conditional updates and deletes
        // on an Azure Search index.
        static void Main(string[] args)
        {
            IConfigurationBuilder builder = new ConfigurationBuilder().AddJsonFile("appsettings.json");
            IConfigurationRoot configuration = builder.Build();

            SearchServiceClient serviceClient = CreateSearchServiceClient(configuration);

            Console.WriteLine("Deleting index...\n");
            DeleteTestIndexIfExists(serviceClient);

            // Every top-level resource in Azure Search has an associated ETag that keeps track of which version
            // of hello resource you're working on. When you first create a resource such as an index, its ETag is
            // empty.
            Index index = DefineTestIndex();
            Console.WriteLine(
                $"Test index hasn't been created yet, so its ETag should be blank. ETag: '{index.ETag}'");

            // Once hello resource exists in Azure Search, its ETag will be populated. Make sure toouse hello object
            // returned by hello SearchServiceClient! Otherwise, you will still have hello old object with the
            // blank ETag.
            Console.WriteLine("Creating index...\n");
            index = serviceClient.Indexes.Create(index);
            
            Console.WriteLine($"Test index created; Its ETag should be populated. ETag: '{index.ETag}'");

            // ETags let you do some useful things you couldn't do otherwise. For example, by using an If-Match
            // condition, we can update an index using CreateOrUpdate and be guaranteed that hello update will only
            // succeed if hello index already exists.
            index.Fields.Add(new Field("name", AnalyzerName.EnMicrosoft));
            index =
                serviceClient.Indexes.CreateOrUpdate(
                    index,
                    accessCondition: AccessCondition.GenerateIfExistsCondition());

            Console.WriteLine(
                $"Test index updated; Its ETag should have changed since it was created. ETag: '{index.ETag}'");

            // More importantly, ETags protect you from concurrent updates toohello same resource. If another
            // client tries tooupdate hello resource, it will fail as long as all clients are using hello right
            // access conditions.
            Index indexForClient1 = index;
            Index indexForClient2 = serviceClient.Indexes.Get("test");

            Console.WriteLine("Simulating concurrent update. toostart, both clients see hello same ETag.");
            Console.WriteLine($"Client 1 ETag: '{indexForClient1.ETag}' Client 2 ETag: '{indexForClient2.ETag}'");

            // Client 1 successfully updates hello index.
            indexForClient1.Fields.Add(new Field("a", DataType.Int32));
            indexForClient1 =
                serviceClient.Indexes.CreateOrUpdate(
                    indexForClient1,
                    accessCondition: AccessCondition.IfNotChanged(indexForClient1));

            Console.WriteLine($"Test index updated by client 1; ETag: '{indexForClient1.ETag}'");

            // Client 2 tries tooupdate hello index, but fails, thanks toohello ETag check.
            try
            {
                indexForClient2.Fields.Add(new Field("b", DataType.Boolean));
                serviceClient.Indexes.CreateOrUpdate(
                    indexForClient2, 
                    accessCondition: AccessCondition.IfNotChanged(indexForClient2));

                Console.WriteLine("Whoops; This shouldn't happen");
                Environment.Exit(1);
            }
            catch (CloudException e) when (e.IsAccessConditionFailed())
            {
                Console.WriteLine("Client 2 failed tooupdate hello index, as expected.");
            }

            // You can also use access conditions with Delete operations. For example, you can implement an
            // atomic version of hello DeleteTestIndexIfExists method from this sample like this:
            Console.WriteLine("Deleting index...\n");
            serviceClient.Indexes.Delete("test", accessCondition: AccessCondition.GenerateIfExistsCondition());

            // This is slightly better than using hello Exists method since it makes only one round trip to
            // Azure Search instead of potentially two. It also avoids an extra Delete request in cases where
            // hello resource is deleted concurrently, but this doesn't matter much since resource deletion in
            // Azure Search is idempotent.

            // And we're done! Bye!
            Console.WriteLine("Complete.  Press any key tooend application...\n");
            Console.ReadKey();
        }

        private static SearchServiceClient CreateSearchServiceClient(IConfigurationRoot configuration)
        {
            string searchServiceName = configuration["SearchServiceName"];
            string adminApiKey = configuration["SearchServiceAdminApiKey"];

            SearchServiceClient serviceClient =
                new SearchServiceClient(searchServiceName, new SearchCredentials(adminApiKey));
            return serviceClient;
        }

        private static void DeleteTestIndexIfExists(SearchServiceClient serviceClient)
        {
            if (serviceClient.Indexes.Exists("test"))
            {
                serviceClient.Indexes.Delete("test");
            }
        }

        private static Index DefineTestIndex() =>
            new Index()
            {
                Name = "test",
                Fields = new[] { new Field("id", DataType.String) { IsKey = true } }
            };
    }
}
```

## <a name="design-pattern"></a><span data-ttu-id="c3e10-131">Designmönstret</span><span class="sxs-lookup"><span data-stu-id="c3e10-131">Design pattern</span></span>

<span data-ttu-id="c3e10-132">Ett designmönster för att implementera Optimistisk samtidighet ska innehålla en loop som försöker hello villkor för åtkomstkontrollen, ett test för hello åtkomst villkoret och eventuellt hämtar ett uppdaterade resursen innan du försöker toore-hello ändringarna.</span><span class="sxs-lookup"><span data-stu-id="c3e10-132">A design pattern for implementing optimistic concurrency should include a loop that retries hello access condition check, a test for hello access condition, and optionally retrieves an updated resource before attempting toore-apply hello changes.</span></span> 

<span data-ttu-id="c3e10-133">Det här kodstycket illustrerar hello lägga till ett synonymMap tooan index som redan finns.</span><span class="sxs-lookup"><span data-stu-id="c3e10-133">This code snippet illustrates hello addition of a synonymMap tooan index that already exists.</span></span> <span data-ttu-id="c3e10-134">Den här koden är från hello [synonymen (förhandsgranskning) C#-självstudiekurs för Azure Search](https://docs.microsoft.com/azure/search/search-synonyms-tutorial-sdk).</span><span class="sxs-lookup"><span data-stu-id="c3e10-134">This code is from hello [Synonym (preview) C# tutorial for Azure Search](https://docs.microsoft.com/azure/search/search-synonyms-tutorial-sdk).</span></span> 

<span data-ttu-id="c3e10-135">hello fragment hämtar hello ”hotell” index, kontrollerar versionen på en uppdateringsåtgärd hello, genererar ett undantag om hello villkoret misslyckas och sedan återförsök hello åtgärden (upp toothree gånger), från och med index hämtning från hello server tooget hello senaste version.</span><span class="sxs-lookup"><span data-stu-id="c3e10-135">hello snippet gets hello "hotels" index, checks hello object version on an update operation, throws an exception if hello condition fails, and then retries hello operation (up toothree times), starting with index retrieval from hello server tooget hello latest version.</span></span>

        private static void EnableSynonymsInHotelsIndexSafely(SearchServiceClient serviceClient)
        {
            int MaxNumTries = 3;

            for (int i = 0; i < MaxNumTries; ++i)
            {
                try
                {
                    Index index = serviceClient.Indexes.Get("hotels");
                    index = AddSynonymMapsToFields(index);

                    // hello IfNotChanged condition ensures that hello index is updated only if hello ETags match.
                    serviceClient.Indexes.CreateOrUpdate(index, accessCondition: AccessCondition.IfNotChanged(index));

                    Console.WriteLine("Updated hello index successfully.\n");
                    break;
                }
                catch (CloudException e) when (e.IsAccessConditionFailed())
                {
                    Console.WriteLine($"Index update failed : {e.Message}. Attempt({i}/{MaxNumTries}).\n");
                }
            }
        }
        
        private static Index AddSynonymMapsToFields(Index index)
        {
            index.Fields.First(f => f.Name == "category").SynonymMaps = new[] { "desc-synonymmap" };
            index.Fields.First(f => f.Name == "tags").SynonymMaps = new[] { "desc-synonymmap" };
            return index;
        }


## <a name="next-steps"></a><span data-ttu-id="c3e10-136">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="c3e10-136">Next steps</span></span>

<span data-ttu-id="c3e10-137">Granska hello [synonymer C#-exempel](https://github.com/Azure-Samples/search-dotnet-getting-started/tree/master/DotNetHowToSynonyms) för mer kontext på hur toosafely uppdatera ett befintligt index.</span><span class="sxs-lookup"><span data-stu-id="c3e10-137">Review hello [synonyms C# sample](https://github.com/Azure-Samples/search-dotnet-getting-started/tree/master/DotNetHowToSynonyms) for more context on how toosafely update an existing index.</span></span>

<span data-ttu-id="c3e10-138">Försök med att ändra någon av följande hello prover tooinclude ETags eller AccessCondition objekt.</span><span class="sxs-lookup"><span data-stu-id="c3e10-138">Try modifying either of hello following samples tooinclude ETags or AccessCondition objects.</span></span>

+ [<span data-ttu-id="c3e10-139">REST API-exemplet på Github</span><span class="sxs-lookup"><span data-stu-id="c3e10-139">REST API sample on Github</span></span>](https://github.com/Azure-Samples/search-rest-api-getting-started) 
+ <span data-ttu-id="c3e10-140">[.NET SDK-exempel på Github](https://github.com/Azure-Samples/search-dotnet-getting-started).</span><span class="sxs-lookup"><span data-stu-id="c3e10-140">[.NET SDK sample on Github](https://github.com/Azure-Samples/search-dotnet-getting-started).</span></span> <span data-ttu-id="c3e10-141">Den här lösningen innehåller hello ”DotNetEtagsExplainer” projekt som innehåller hello koden som visas i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="c3e10-141">This solution includes hello "DotNetEtagsExplainer" project containing hello code presented in this article.</span></span>

## <a name="see-also"></a><span data-ttu-id="c3e10-142">Se även</span><span class="sxs-lookup"><span data-stu-id="c3e10-142">See also</span></span>

  <span data-ttu-id="c3e10-143">[Vanliga HTTP-begäran och svar rubriker](https://docs.microsoft.com/rest/api/searchservice/common-http-request-and-response-headers-used-in-azure-search)  </span><span class="sxs-lookup"><span data-stu-id="c3e10-143">[Common HTTP request and response headers](https://docs.microsoft.com/rest/api/searchservice/common-http-request-and-response-headers-used-in-azure-search)  </span></span>  
  <span data-ttu-id="c3e10-144">[Statuskoder för HTTP](https://docs.microsoft.com/rest/api/searchservice/http-status-codes) [Index-åtgärder (REST-API)](https://docs.microsoft.com/\rest/api/searchservice/index-operations)</span><span class="sxs-lookup"><span data-stu-id="c3e10-144">[HTTP status codes](https://docs.microsoft.com/rest/api/searchservice/http-status-codes) [Index operations (REST API)](https://docs.microsoft.com/\rest/api/searchservice/index-operations)</span></span>