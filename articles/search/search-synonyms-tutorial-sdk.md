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
# <a name="synonym-preview-c-tutorial-for-azure-search"></a>Förhandsgranska synonymer – en självstudie i C# för Azure Search

Synonymer expandera en fråga med matchande på villkor som anses vara samma sak toohello inkommande termen. Du kanske exempelvis vill ”bil” toomatch dokument som innehåller hello villkoren ”bil” eller ”fordon”.

I Azure Search definieras synonymer i en *synonymmappning* enligt *mappningsregler* som associerar ekvivalenta termer. Du kan skapa flera synonymen maps, publicera dem som en tjänsteomfattande resurs tillgängliga tooany index och referera till vilka en toouse på hello fältnivå. När databasfrågan har dessutom toosearching ett index Azure Search en sökning i en synonym karta, om en anges på fälten som används i hello frågan.

> [!NOTE]
> hello synonymer funktionen är för närvarande i Förhandsgranska och stöds i endast hello senaste förhandsversionen API och SDK-versioner (api-version = 2016-09-01-Preview, SDK version 4.x-preview). Funktionen stöds för närvarande inte på Azure Portal. För förhandsversioner av API:er gäller inget SLA och de här funktionerna kan ändras, så du bör inte använda dem i produktionsmiljöer.

## <a name="prerequisites"></a>Krav

Krav på Självstudier innehåller hello följande:

* [Visual Studio](https://www.visualstudio.com/downloads/)
* [Azure Search-tjänsten](search-create-service-portal.md)
* [Förhandsversionen av .NET-biblioteket Microsoft.Azure.Search](https://aka.ms/search-sdk-preview)
* [Hur toouse Azure söka från ett .NET-program](https://docs.microsoft.com/azure/search/search-howto-dotnet-sdk)

## <a name="overview"></a>Översikt

Före och efter frågor visar hello värdet för synonymer. I den här självstudien använder vi ett exempelprogram som kör frågor och returnerar resultat för ett exempelindex. hello exempelprogrammet skapar ett litet index med namnet ”hotell” fylls med två dokument. hello-program körs sökfrågor med termer och fraser som inte visas i hello index, aktiverar hello synonymer funktion, och sedan problem hello samma sökningar igen. hello-koden nedan visar hello övergripande flödet.

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
Hej steg toocreate och fylla hello exempel index förklaras i [hur toouse Azure söka från ett .NET-program](https://docs.microsoft.com/azure/search/search-howto-dotnet-sdk).

## <a name="before-queries"></a>”Före”-frågor

I `RunQueriesWithNonExistentTermsInIndex` kör vi sökningar med termerna ”five star”, ”internet” och ”economy AND hotel”.
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
Ingen av hello två indexerade dokument innehåller hello villkor, så att vi får hello följande utdata från hello först `RunQueriesWithNonExistentTermsInIndex`.
~~~
Search hello entire index for hello phrase "five star":

no document matched

Search hello entire index for hello term 'internet':

no document matched

Search hello entire index for hello terms 'economy' AND 'hotel':

no document matched
~~~

## <a name="enable-synonyms"></a>Aktivera synonymer

Att aktivera synonymer är en tvåstegsprocess. Vi först definiera och överför synonymen regler och sedan konfigurera fält toouse dem. hello process beskrivs i `UploadSynonyms` och `EnableSynonymsInHotelsIndex`.

1. Lägg till en synonym kartan tooyour search-tjänsten. I `UploadSynonyms`, vi definiera fyra regler i vår synonymen map 'desc synonymmap' och ladda upp toohello service.
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
En synonym karta måste uppfylla toohello öppen källkod standard `solr` format. hello format beskrivs i [synonymer i Azure Search](search-synonyms.md) under hello avsnittet `Apache Solr synonym format`.

2. Konfigurera sökbara fält toouse hello synonymen kartan i hello indexdefinitionen. I `EnableSynonymsInHotelsIndex`, vi aktivera synonymer i två fält `category` och `tags` genom att ange hello `synonymMaps` toohello egenskapsnamn för hello nyligen har överförts synonymen kartan.
```csharp
  Index index = serviceClient.Indexes.Get("hotels");
  index.Fields.First(f => f.Name == "category").SynonymMaps = new[] { "desc-synonymmap" };
  index.Fields.First(f => f.Name == "tags").SynonymMaps = new[] { "desc-synonymmap" };

  serviceClient.Indexes.CreateOrUpdate(index);
```
När du lägger till en synonymmappning behöver du inte skapa indexet på nytt. Du kan lägga till en synonym kartan tooyour tjänst och ändra befintliga fältdefinitioner i alla index toouse hello nya synonymen-mappning. hello lägga till nya attribut har ingen inverkan på tillgänglighet av indexet. hello samma gäller inaktiveras synonymer för ett fält. Du kan bara ange hello `synonymMaps` egenskapen tooan tom lista.
```csharp
  index.Fields.First(f => f.Name == "category").SynonymMaps = new List<string>();
```

## <a name="after-queries"></a>”Efter”-frågor

När hello synonymen kartan överförs och hello index är uppdaterade toouse hello synonymen mappning hello andra `RunQueriesWithNonExistentTermsInIndex` anrop matar ut hello följande:

~~~
Search hello entire index for hello phrase "five star":

Name: Fancy Stay        Category: Luxury        Tags: [pool, view, wifi, concierge]

Search hello entire index for hello term 'internet':

Name: Fancy Stay        Category: Luxury        Tags: [pool, view, wifi, concierge]

Search hello entire index for hello terms 'economy' AND 'hotel':

Name: Roach Motel       Category: Budget        Tags: [motel, budget]
~~~
hello första frågan hittar hello dokument från hello regel `five star=>luxury`. hello andra frågan expanderar hello Sök med `internet,wifi` och hello tredje använder både `hotel, motel` och `economy,inexpensive=>budget` att hitta hello dokument de matchade.

Lägga till synonymer helt ändrar hello sökinställningar. I den här självstudiekursen misslyckades hello ursprungliga frågor tooreturn meningsfulla resultaten även om hello dokument i vårt index har relevanta. Vi kan expandera ett index tooinclude villkor gemensam användning, utan ändringar toounderlying data i hello index genom att aktivera synonymer.

## <a name="sample-application-source-code"></a>Källkod för exempelprogrammet
Du kan hitta hello fullständig källkoden för hello exempelprogrammet används i denna genomgång på [GitHub](https://github.com/Azure-Samples/search-dotnet-getting-started/tree/master/DotNetHowToSynonyms).

## <a name="next-steps"></a>Nästa steg

* Granska [hur toouse synonymer i Azure Search](search-synonyms.md)
* Läs om [synonymer i dokumentationen till REST API](https://aka.ms/rgm6rq)
* Bläddra hello referenser för hello [.NET SDK](https://docs.microsoft.com/dotnet/api/microsoft.azure.search) och [REST API](https://docs.microsoft.com/rest/api/searchservice/).
