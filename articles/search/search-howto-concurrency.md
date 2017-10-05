---
title: "Så här hanterar du samtidiga skriver till resurser i Azure Search"
description: "Optimistisk samtidighet Undvik att använda mitt luften kollisioner på uppdateringar eller borttagningar Azure Search index, indexerare,-datakällor."
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
ms.openlocfilehash: aee1b7376d4829e3e2f5a232525e3c3cb4df9d8e
ms.sourcegitcommit: 422efcbac5b6b68295064bd545132fcc98349d01
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/29/2017
---
# <a name="how-to-manage-concurrency-in-azure-search"></a><span data-ttu-id="70c2f-103">Så här hanterar du samtidighet i Azure Search</span><span class="sxs-lookup"><span data-stu-id="70c2f-103">How to manage concurrency in Azure Search</span></span>

<span data-ttu-id="70c2f-104">När du hanterar Azure Search-resurser, till exempel index och datakällor, är det viktigt att uppdatera resurser på ett säkert sätt, särskilt om resurser används samtidigt av olika komponenter i ditt program.</span><span class="sxs-lookup"><span data-stu-id="70c2f-104">When managing Azure Search resources such as indexes and data sources, it's important to update resources safely, especially if resources are accessed concurrently by different components of your application.</span></span> <span data-ttu-id="70c2f-105">När två klienter samtidigt uppdaterar resurser utan samordning, är konkurrenstillstånd möjliga.</span><span class="sxs-lookup"><span data-stu-id="70c2f-105">When two clients concurrently update a resource without coordination, race conditions are possible.</span></span> <span data-ttu-id="70c2f-106">Om du vill förhindra detta Azure Search erbjuder en *optimistisk*.</span><span class="sxs-lookup"><span data-stu-id="70c2f-106">To prevent this, Azure Search offers an *optimistic concurrency model*.</span></span> <span data-ttu-id="70c2f-107">Det finns inga lås på en resurs.</span><span class="sxs-lookup"><span data-stu-id="70c2f-107">There are no locks on a resource.</span></span> <span data-ttu-id="70c2f-108">Det finns i stället en ETag för varje resurs som identifierar resursversionen så att du kan skapa förfrågningar som undviker oavsiktliga skrivs över.</span><span class="sxs-lookup"><span data-stu-id="70c2f-108">Instead, there is an ETag for every resource that identifies the resource version so that you can craft requests that avoid accidental overwrites.</span></span>

> [!Tip]
> <span data-ttu-id="70c2f-109">Konceptuell kod i en [exempel C#-lösningen](https://github.com/Azure-Samples/search-dotnet-getting-started/tree/master/DotNetETagsExplainer) förklarar hur samtidighetskontroll fungerar i Azure Search.</span><span class="sxs-lookup"><span data-stu-id="70c2f-109">Conceptual code in a [sample C# solution](https://github.com/Azure-Samples/search-dotnet-getting-started/tree/master/DotNetETagsExplainer) explains how concurrency control works in Azure Search.</span></span> <span data-ttu-id="70c2f-110">Koden skapar villkor som anropar samtidighetskontroll.</span><span class="sxs-lookup"><span data-stu-id="70c2f-110">The code creates conditions that invoke concurrency control.</span></span> <span data-ttu-id="70c2f-111">Läsa den [kodfragmentet nedan](#samplecode) är förmodligen tillräcklig för de flesta utvecklare, men om du vill köra, redigera appsettings.json för att lägga till tjänstens namn och en admin api-nyckel.</span><span class="sxs-lookup"><span data-stu-id="70c2f-111">Reading the [code fragment below](#samplecode) is probably sufficient for most developers, but if you want to run it, edit appsettings.json to add the service name and an admin api-key.</span></span> <span data-ttu-id="70c2f-112">Tjänsten URL: en `http://myservice.search.windows.net`, tjänstnamnet är `myservice`.</span><span class="sxs-lookup"><span data-stu-id="70c2f-112">Given a service URL of `http://myservice.search.windows.net`, the service name is `myservice`.</span></span>

## <a name="how-it-works"></a><span data-ttu-id="70c2f-113">Hur det fungerar</span><span class="sxs-lookup"><span data-stu-id="70c2f-113">How it works</span></span>

<span data-ttu-id="70c2f-114">Optimistisk samtidighet implementeras via åtkomst villkoret söker i API-anrop skrivning till index, indexerare, datakällor och synonymMap resurser.</span><span class="sxs-lookup"><span data-stu-id="70c2f-114">Optimistic concurrency is implemented through access condition checks in API calls writing to indexes, indexers, datasources, and synonymMap resources.</span></span> 

<span data-ttu-id="70c2f-115">Alla resurser som har en [ *entitetstaggen (ETag)* ](https://en.wikipedia.org/wiki/HTTP_ETag) som innehåller versionsinformation för objektet.</span><span class="sxs-lookup"><span data-stu-id="70c2f-115">All resources have an [*entity tag (ETag)*](https://en.wikipedia.org/wiki/HTTP_ETag) that provides object version information.</span></span> <span data-ttu-id="70c2f-116">Du kan undvika samtidiga uppdateringar i ett normalt arbetsflöde genom att kontrollera ETag först (get, ändra lokalt, uppdatera) genom att kontrollera resursens ETag matchar den lokala kopian.</span><span class="sxs-lookup"><span data-stu-id="70c2f-116">By checking the ETag first, you can avoid concurrent updates in a typical workflow (get, modify locally, update) by ensuring the resource's ETag matches your local copy.</span></span> 

+ <span data-ttu-id="70c2f-117">REST-API: N används en [ETag](https://docs.microsoft.com/rest/api/searchservice/common-http-request-and-response-headers-used-in-azure-search) på begärandehuvudet.</span><span class="sxs-lookup"><span data-stu-id="70c2f-117">The REST API uses an [ETag](https://docs.microsoft.com/rest/api/searchservice/common-http-request-and-response-headers-used-in-azure-search) on the request header.</span></span>
+ <span data-ttu-id="70c2f-118">.NET SDK anger ETag via ett accessCondition objekt, ange den [If-Match | Huvudet If-Match-ingen](https://docs.microsoft.com/rest/api/searchservice/common-http-request-and-response-headers-used-in-azure-search) på resursen.</span><span class="sxs-lookup"><span data-stu-id="70c2f-118">The .NET SDK sets the ETag through an accessCondition object, setting the [If-Match | If-Match-None header](https://docs.microsoft.com/rest/api/searchservice/common-http-request-and-response-headers-used-in-azure-search) on the resource.</span></span> <span data-ttu-id="70c2f-119">Alla objekt som ärver från [IResourceWithETag (.NET SDK)](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.iresourcewithetag) har ett accessCondition-objekt.</span><span class="sxs-lookup"><span data-stu-id="70c2f-119">Any object inheriting from [IResourceWithETag (.NET SDK)](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.iresourcewithetag) has an accessCondition object.</span></span>

<span data-ttu-id="70c2f-120">Varje gång du uppdaterar en resurs ändras dess ETag automatiskt.</span><span class="sxs-lookup"><span data-stu-id="70c2f-120">Every time you update a resource, its ETag changes automatically.</span></span> <span data-ttu-id="70c2f-121">När du implementerar samtidighet management allt du gör är att publicera ett villkor på begäran om uppdatering som kräver fjärresursen ska ha samma ETag som en kopia av den resurs som du har ändrat på klienten.</span><span class="sxs-lookup"><span data-stu-id="70c2f-121">When you implement concurrency management, all you're doing is putting a precondition on the update request that requires the remote resource to have the same ETag as the copy of the resource that you modified on the client.</span></span> <span data-ttu-id="70c2f-122">Om en samtidig process har redan ändrats fjärresursen, ETag matchar inte villkor och begäran att misslyckas med HTTP 412.</span><span class="sxs-lookup"><span data-stu-id="70c2f-122">If a concurrent process has changed the remote resource already, the ETag will not match the precondition and the request will fail with HTTP 412.</span></span> <span data-ttu-id="70c2f-123">Om du använder .NET SDK, visar detta som en `CloudException` där den `IsAccessConditionFailed()` metod returnerar true.</span><span class="sxs-lookup"><span data-stu-id="70c2f-123">If you're using the .NET SDK, this manifests as a `CloudException` where the `IsAccessConditionFailed()` extension method returns true.</span></span>

> [!Note]
> <span data-ttu-id="70c2f-124">Det finns endast en mekanism för samtidighet.</span><span class="sxs-lookup"><span data-stu-id="70c2f-124">There is only one mechanism for concurrency.</span></span> <span data-ttu-id="70c2f-125">Den används alltid oavsett vilket API används för att uppdatera resurserna.</span><span class="sxs-lookup"><span data-stu-id="70c2f-125">It's always used regardless of which API is used for resource updates.</span></span> 

<a name="samplecode"></a>
## <a name="use-cases-and-sample-code"></a><span data-ttu-id="70c2f-126">Användningsfall och exempelkod</span><span class="sxs-lookup"><span data-stu-id="70c2f-126">Use cases and sample code</span></span>

<span data-ttu-id="70c2f-127">Följande kod visar accessCondition söker efter uppdateringsåtgärder:</span><span class="sxs-lookup"><span data-stu-id="70c2f-127">The following code demonstrates accessCondition checks for key update operations:</span></span>

+ <span data-ttu-id="70c2f-128">Inte en uppdatering om resursen finns inte längre</span><span class="sxs-lookup"><span data-stu-id="70c2f-128">Fail an update if the resource no longer exists</span></span>
+ <span data-ttu-id="70c2f-129">En uppdatering att misslyckas om resursversionen ändras</span><span class="sxs-lookup"><span data-stu-id="70c2f-129">Fail an update if the resource version changes</span></span>

### <a name="sample-code-from-dotnetetagsexplainer-programhttpsgithubcomazure-samplessearch-dotnet-getting-startedtreemasterdotnetetagsexplainer"></a><span data-ttu-id="70c2f-130">Exempel på kod från [DotNetETagsExplainer program](https://github.com/Azure-Samples/search-dotnet-getting-started/tree/master/DotNetETagsExplainer)</span><span class="sxs-lookup"><span data-stu-id="70c2f-130">Sample code from [DotNetETagsExplainer program](https://github.com/Azure-Samples/search-dotnet-getting-started/tree/master/DotNetETagsExplainer)</span></span>

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
            // of the resource you're working on. When you first create a resource such as an index, its ETag is
            // empty.
            Index index = DefineTestIndex();
            Console.WriteLine(
                $"Test index hasn't been created yet, so its ETag should be blank. ETag: '{index.ETag}'");

            // Once the resource exists in Azure Search, its ETag will be populated. Make sure to use the object
            // returned by the SearchServiceClient! Otherwise, you will still have the old object with the
            // blank ETag.
            Console.WriteLine("Creating index...\n");
            index = serviceClient.Indexes.Create(index);
            
            Console.WriteLine($"Test index created; Its ETag should be populated. ETag: '{index.ETag}'");

            // ETags let you do some useful things you couldn't do otherwise. For example, by using an If-Match
            // condition, we can update an index using CreateOrUpdate and be guaranteed that the update will only
            // succeed if the index already exists.
            index.Fields.Add(new Field("name", AnalyzerName.EnMicrosoft));
            index =
                serviceClient.Indexes.CreateOrUpdate(
                    index,
                    accessCondition: AccessCondition.GenerateIfExistsCondition());

            Console.WriteLine(
                $"Test index updated; Its ETag should have changed since it was created. ETag: '{index.ETag}'");

            // More importantly, ETags protect you from concurrent updates to the same resource. If another
            // client tries to update the resource, it will fail as long as all clients are using the right
            // access conditions.
            Index indexForClient1 = index;
            Index indexForClient2 = serviceClient.Indexes.Get("test");

            Console.WriteLine("Simulating concurrent update. To start, both clients see the same ETag.");
            Console.WriteLine($"Client 1 ETag: '{indexForClient1.ETag}' Client 2 ETag: '{indexForClient2.ETag}'");

            // Client 1 successfully updates the index.
            indexForClient1.Fields.Add(new Field("a", DataType.Int32));
            indexForClient1 =
                serviceClient.Indexes.CreateOrUpdate(
                    indexForClient1,
                    accessCondition: AccessCondition.IfNotChanged(indexForClient1));

            Console.WriteLine($"Test index updated by client 1; ETag: '{indexForClient1.ETag}'");

            // Client 2 tries to update the index, but fails, thanks to the ETag check.
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
                Console.WriteLine("Client 2 failed to update the index, as expected.");
            }

            // You can also use access conditions with Delete operations. For example, you can implement an
            // atomic version of the DeleteTestIndexIfExists method from this sample like this:
            Console.WriteLine("Deleting index...\n");
            serviceClient.Indexes.Delete("test", accessCondition: AccessCondition.GenerateIfExistsCondition());

            // This is slightly better than using the Exists method since it makes only one round trip to
            // Azure Search instead of potentially two. It also avoids an extra Delete request in cases where
            // the resource is deleted concurrently, but this doesn't matter much since resource deletion in
            // Azure Search is idempotent.

            // And we're done! Bye!
            Console.WriteLine("Complete.  Press any key to end application...\n");
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

## <a name="design-pattern"></a><span data-ttu-id="70c2f-131">Designmönstret</span><span class="sxs-lookup"><span data-stu-id="70c2f-131">Design pattern</span></span>

<span data-ttu-id="70c2f-132">Ett designmönster för att implementera på Optimistisk samtidighet ska innehålla en loop som försöker villkoret åtkomst kontrollera, ett test för villkoret åtkomst och hämtar eventuellt ett uppdaterade resursen innan du försöker att åter tillämpa ändringarna.</span><span class="sxs-lookup"><span data-stu-id="70c2f-132">A design pattern for implementing optimistic concurrency should include a loop that retries the access condition check, a test for the access condition, and optionally retrieves an updated resource before attempting to re-apply the changes.</span></span> 

<span data-ttu-id="70c2f-133">Det här kodstycket illustrerar för att lägga till en synonymMap till ett index som redan finns.</span><span class="sxs-lookup"><span data-stu-id="70c2f-133">This code snippet illustrates the addition of a synonymMap to an index that already exists.</span></span> <span data-ttu-id="70c2f-134">Den här koden är från den [synonymen (förhandsgranskning) C#-självstudiekurs för Azure Search](https://docs.microsoft.com/azure/search/search-synonyms-tutorial-sdk).</span><span class="sxs-lookup"><span data-stu-id="70c2f-134">This code is from the [Synonym (preview) C# tutorial for Azure Search](https://docs.microsoft.com/azure/search/search-synonyms-tutorial-sdk).</span></span> 

<span data-ttu-id="70c2f-135">Sammandraget hämtar ”hotell” index, kontrollerar objektversionen på en uppdateringsåtgärd, genererar ett undantag om villkoret misslyckas, och sedan försöker igen (upp till tre gånger), från och med index hämtning från servern för att hämta den senaste versionen.</span><span class="sxs-lookup"><span data-stu-id="70c2f-135">The snippet gets the "hotels" index, checks the object version on an update operation, throws an exception if the condition fails, and then retries the operation (up to three times), starting with index retrieval from the server to get the latest version.</span></span>

        private static void EnableSynonymsInHotelsIndexSafely(SearchServiceClient serviceClient)
        {
            int MaxNumTries = 3;

            for (int i = 0; i < MaxNumTries; ++i)
            {
                try
                {
                    Index index = serviceClient.Indexes.Get("hotels");
                    index = AddSynonymMapsToFields(index);

                    // The IfNotChanged condition ensures that the index is updated only if the ETags match.
                    serviceClient.Indexes.CreateOrUpdate(index, accessCondition: AccessCondition.IfNotChanged(index));

                    Console.WriteLine("Updated the index successfully.\n");
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


## <a name="next-steps"></a><span data-ttu-id="70c2f-136">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="70c2f-136">Next steps</span></span>

<span data-ttu-id="70c2f-137">Granska de [synonymer C#-exempel](https://github.com/Azure-Samples/search-dotnet-getting-started/tree/master/DotNetHowToSynonyms) för mer kontext om hur du uppdaterar ett befintligt index på ett säkert sätt.</span><span class="sxs-lookup"><span data-stu-id="70c2f-137">Review the [synonyms C# sample](https://github.com/Azure-Samples/search-dotnet-getting-started/tree/master/DotNetHowToSynonyms) for more context on how to safely update an existing index.</span></span>

<span data-ttu-id="70c2f-138">Försök att ändra någon av följande prov ska innehålla ETags eller AccessCondition objekt.</span><span class="sxs-lookup"><span data-stu-id="70c2f-138">Try modifying either of the following samples to include ETags or AccessCondition objects.</span></span>

+ [<span data-ttu-id="70c2f-139">REST API-exemplet på Github</span><span class="sxs-lookup"><span data-stu-id="70c2f-139">REST API sample on Github</span></span>](https://github.com/Azure-Samples/search-rest-api-getting-started) 
+ <span data-ttu-id="70c2f-140">[.NET SDK-exempel på Github](https://github.com/Azure-Samples/search-dotnet-getting-started).</span><span class="sxs-lookup"><span data-stu-id="70c2f-140">[.NET SDK sample on Github](https://github.com/Azure-Samples/search-dotnet-getting-started).</span></span> <span data-ttu-id="70c2f-141">Den här lösningen innehåller ”DotNetEtagsExplainer”-projektet som innehåller koden som visas i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="70c2f-141">This solution includes the "DotNetEtagsExplainer" project containing the code presented in this article.</span></span>

## <a name="see-also"></a><span data-ttu-id="70c2f-142">Se även</span><span class="sxs-lookup"><span data-stu-id="70c2f-142">See also</span></span>

  <span data-ttu-id="70c2f-143">[Vanliga HTTP-begäran och svar rubriker](https://docs.microsoft.com/rest/api/searchservice/common-http-request-and-response-headers-used-in-azure-search)  </span><span class="sxs-lookup"><span data-stu-id="70c2f-143">[Common HTTP request and response headers](https://docs.microsoft.com/rest/api/searchservice/common-http-request-and-response-headers-used-in-azure-search)  </span></span>  
  <span data-ttu-id="70c2f-144">[Statuskoder för HTTP](https://docs.microsoft.com/rest/api/searchservice/http-status-codes) [Index-åtgärder (REST-API)](https://docs.microsoft.com/\rest/api/searchservice/index-operations)</span><span class="sxs-lookup"><span data-stu-id="70c2f-144">[HTTP status codes](https://docs.microsoft.com/rest/api/searchservice/http-status-codes) [Index operations (REST API)](https://docs.microsoft.com/\rest/api/searchservice/index-operations)</span></span>