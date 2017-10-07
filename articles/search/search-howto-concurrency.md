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
# <a name="how-toomanage-concurrency-in-azure-search"></a>Hur toomanage samtidighet i Azure Search

När du hanterar Azure Search-resurser, till exempel index och datakällor, är det viktigt tooupdate resurser på ett säkert sätt, särskilt om resurser används samtidigt av olika komponenter i ditt program. När två klienter samtidigt uppdaterar resurser utan samordning, är konkurrenstillstånd möjliga. tooprevent detta, Azure Search erbjuder en *optimistisk*. Det finns inga lås på en resurs. Det finns i stället en ETag för varje resurs som identifierar hello resursversionen så att du kan skapa förfrågningar som undviker oavsiktliga skrivs över.

> [!Tip]
> Konceptuell kod i en [exempel C#-lösningen](https://github.com/Azure-Samples/search-dotnet-getting-started/tree/master/DotNetETagsExplainer) förklarar hur samtidighetskontroll fungerar i Azure Search. hello kod skapar villkor som anropar samtidighetskontroll. Läsning hello [kodfragmentet nedan](#samplecode) räcker troligen för de flesta utvecklare, men om du vill toorun den, redigera appsettings.json tooadd hello tjänstens namn och en admin api-nyckel. Tjänsten URL: en `http://myservice.search.windows.net`, hello tjänstnamnet är `myservice`.

## <a name="how-it-works"></a>Hur det fungerar

Optimistisk samtidighet implementeras via åtkomst villkoret söker i API-anrop skriva tooindexes, indexerare, datakällor och synonymMap resurser. 

Alla resurser som har en [ *entitetstaggen (ETag)* ](https://en.wikipedia.org/wiki/HTTP_ETag) som innehåller versionsinformation för objektet. Du kan undvika samtidiga uppdateringar i ett normalt arbetsflöde genom att kontrollera hello ETag först (get, ändra lokalt, uppdatera) genom att säkerställa hello resursens ETag matchar den lokala kopian. 

+ hello REST API: N används en [ETag](https://docs.microsoft.com/rest/api/searchservice/common-http-request-and-response-headers-used-in-azure-search) på hello huvudet i begäran.
+ hello .NET SDK anger hello ETag via ett accessCondition objekt, ange hello [If-Match | Huvudet If-Match-ingen](https://docs.microsoft.com/rest/api/searchservice/common-http-request-and-response-headers-used-in-azure-search) på hello resursen. Alla objekt som ärver från [IResourceWithETag (.NET SDK)](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.iresourcewithetag) har ett accessCondition-objekt.

Varje gång du uppdaterar en resurs ändras dess ETag automatiskt. När du implementerar samtidighet management allt du gör är att publicera ett villkor på hello begäran om uppdatering som kräver hello fjärresursen toohave hello samma ETag som hello kopia av hello-resurs som du har ändrat på hello-klienten. Om en samtidig process har redan ändrats hello fjärresursen, hello ETag matchar inte hello villkor och hello begäran att misslyckas med HTTP 412. Om du använder hello .NET SDK, visar detta som en `CloudException` där hello `IsAccessConditionFailed()` metod returnerar true.

> [!Note]
> Det finns endast en mekanism för samtidighet. Den används alltid oavsett vilket API används för att uppdatera resurserna. 

<a name="samplecode"></a>
## <a name="use-cases-and-sample-code"></a>Användningsfall och exempelkod

hello följande kod visar accessCondition söker efter uppdateringsåtgärder:

+ Inte en uppdatering om hello resursen finns inte längre
+ En uppdatering att misslyckas om hello resursversionen ändras

### <a name="sample-code-from-dotnetetagsexplainer-programhttpsgithubcomazure-samplessearch-dotnet-getting-startedtreemasterdotnetetagsexplainer"></a>Exempel på kod från [DotNetETagsExplainer program](https://github.com/Azure-Samples/search-dotnet-getting-started/tree/master/DotNetETagsExplainer)

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

## <a name="design-pattern"></a>Designmönstret

Ett designmönster för att implementera Optimistisk samtidighet ska innehålla en loop som försöker hello villkor för åtkomstkontrollen, ett test för hello åtkomst villkoret och eventuellt hämtar ett uppdaterade resursen innan du försöker toore-hello ändringarna. 

Det här kodstycket illustrerar hello lägga till ett synonymMap tooan index som redan finns. Den här koden är från hello [synonymen (förhandsgranskning) C#-självstudiekurs för Azure Search](https://docs.microsoft.com/azure/search/search-synonyms-tutorial-sdk). 

hello fragment hämtar hello ”hotell” index, kontrollerar versionen på en uppdateringsåtgärd hello, genererar ett undantag om hello villkoret misslyckas och sedan återförsök hello åtgärden (upp toothree gånger), från och med index hämtning från hello server tooget hello senaste version.

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


## <a name="next-steps"></a>Nästa steg

Granska hello [synonymer C#-exempel](https://github.com/Azure-Samples/search-dotnet-getting-started/tree/master/DotNetHowToSynonyms) för mer kontext på hur toosafely uppdatera ett befintligt index.

Försök med att ändra någon av följande hello prover tooinclude ETags eller AccessCondition objekt.

+ [REST API-exemplet på Github](https://github.com/Azure-Samples/search-rest-api-getting-started) 
+ [.NET SDK-exempel på Github](https://github.com/Azure-Samples/search-dotnet-getting-started). Den här lösningen innehåller hello ”DotNetEtagsExplainer” projekt som innehåller hello koden som visas i den här artikeln.

## <a name="see-also"></a>Se även

  [Vanliga HTTP-begäran och svar rubriker](https://docs.microsoft.com/rest/api/searchservice/common-http-request-and-response-headers-used-in-azure-search)    
  [Statuskoder för HTTP](https://docs.microsoft.com/rest/api/searchservice/http-status-codes) [Index-åtgärder (REST-API)](https://docs.microsoft.com/\rest/api/searchservice/index-operations)