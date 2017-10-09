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
# <a name="query-your-azure-search-index-using-hello-net-sdk"></a>Fråga ditt Azure Search-index med hello .NET SDK
> [!div class="op_single_selector"]
> * [Översikt](search-query-overview.md)
> * [Portalen](search-explorer.md)
> * [.NET](search-query-dotnet.md)
> * [REST](search-query-rest-api.md)
> 
> 

Den här artikeln visar hur tooquery ett index med hjälp av hello [Azure Search .NET SDK](https://aka.ms/search-sdk).

Innan du påbörjar den här genomgången bör du redan ha [skapat ett Azure Search-index](search-what-is-an-index.md) och [fyllt det med data](search-what-is-data-import.md).

> [!NOTE]
> All exempelkod i den här artikeln är skriven i C#. Du kan hitta hello fullständig källkoden [på GitHub](http://aka.ms/search-dotnet-howto). Du kan också läsa om hello [Azure Search .NET SDK](search-howto-dotnet-sdk.md) för en mer detaljerad genomgång av hello exempelkod.

## <a name="identify-your-azure-search-services-query-api-key"></a>Identifiera API-frågenyckeln för Azure Search-tjänsten
Nu när du har skapat ett Azure Search-index är nästan klara tooissue frågor med hello .NET SDK. Först behöver tooobtain en av hello frågan api-nycklar som har genererats för hello söktjänsten du etablerat. hello .NET SDK skickar den här api-nyckel för varje begäran tooyour-tjänst. Med en giltig nyckel upprättar förtroende mellan hello programmet skickar hello begäran och hello-tjänsten som hanterar den fall per begäran.

1. toofind din tjänst api-nycklar kan du logga in toohello [Azure-portalen](https://portal.azure.com/)
2. Gå tooyour Azure Search service-bladet
3. Klicka på hello ”nycklar”-ikon

Tjänsten har *administratörsnycklar* och *frågenycklar*.

* Din primära och sekundära *admin nycklar* bevilja fullständiga rättigheter tooall åtgärder, inklusive hello möjlighet toomanage hello service, skapa och ta bort index, indexerare och datakällor. Det finns två nycklar så att du kan fortsätta toouse hello sekundärnyckeln om du väljer tooregenerate hello primärnyckel och vice versa.
* Din *fråga nycklar* bevilja läsbehörighet tooindexes och dokument och är vanligtvis distribuerade tooclient program som utfärdar search-begäranden.

Du kan använda en av din fråga nycklar för hello frågor till ett index är. Admin-nycklar kan också användas för frågor, men du bör använda en fråga nyckel i din programkod eftersom det bättre följer hello [principen om minsta behörighet](https://en.wikipedia.org/wiki/Principle_of_least_privilege).

## <a name="create-an-instance-of-hello-searchindexclient-class"></a>Skapa en instans av hello SearchIndexClient-klass
tooissue frågor med hello Azure Search .NET SDK, behöver du toocreate en instans av hello `SearchIndexClient` klass. Den här klassen har flera konstruktorer. hello du tar din sökning tjänstnamn, indexnamn, och en `SearchCredentials` objekt som parametrar. `SearchCredentials` omsluter din API-nyckel.

hello koden nedan skapar en ny `SearchIndexClient` för hello ”hotell” index (skapat i [skapa ett Azure Search-index med hello .NET SDK](search-create-index-dotnet.md)) med värden för hello Sök tjänstnamn och api-nyckel som lagras i hello programmets konfiguration filen (`appsettings.json` hello gäller hello [exempelprogram](http://aka.ms/search-dotnet-howto)):

```csharp
private static SearchIndexClient CreateSearchIndexClient(IConfigurationRoot configuration)
{
    string searchServiceName = configuration["SearchServiceName"];
    string queryApiKey = configuration["SearchServiceQueryApiKey"];

    SearchIndexClient indexClient = new SearchIndexClient(searchServiceName, "hotels", new SearchCredentials(queryApiKey));
    return indexClient;
}
```

`SearchIndexClient` har en `Documents`-egenskap. Den här egenskapen anger alla hello metoderna du behöver tooquery Azure Search index.

## <a name="query-your-index"></a>Skicka frågor mot ditt index
Sökning med hello .NET SDK är så enkelt som att anropa hello `Documents.Search` metod på din `SearchIndexClient`. Den här metoden tar några parametrar, inklusive hello söktext tillsammans med en `SearchParameters` objekt som kan vara används toofurther förfina hello fråga.

#### <a name="types-of-queries"></a>Typer av frågor
hello två huvudsakliga [fråga typer](search-query-overview.md#types-of-queries) du ska använda är `search` och `filter`. En `search`-fråga söker efter en eller flera termer i alla *sökbara* fält i ditt index. En `filter`-fråga utvärderar ett booleskt uttryck i alla *filtrerbara* fält i ett index.

Både sökningar och filter utförs med hello `Documents.Search` metod. En sökfråga kan överföras i hello `searchText` parameter, medan en filteruttrycket kan överföras i hello `Filter` -egenskapen för hello `SearchParameters` klass. toofilter utan att söka, skicka `"*"` för hello `searchText` parameter. toosearch utan att filtrera, lämnar du bara hello `Filter` egenskapen unset, eller inte skicka in en `SearchParameters` instansen alls.

#### <a name="example-queries"></a>Exempelfrågor
hello följande exempelkod visar några olika sätt tooquery hello ”hotell” index som definierats i [skapa ett Azure Search-index med hello .NET SDK](search-create-index-dotnet.md#DefineIndex). Observera att hello dokument som returneras med hello sökresultaten är instanser av hello `Hotel` -klassen, som har definierats i [Import av Data i Azure Search med hello .NET SDK](search-import-data-dotnet.md#HotelClass). Hej exempelkod utnyttjar en `WriteDocuments` metoden toooutput hello sökresultat toohello konsolen. Den här metoden beskrivs i nästa avsnitt om hello.

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

## <a name="handle-search-results"></a>Hantera sökresultat
Hej `Documents.Search` metoden returnerar en `DocumentSearchResult` objekt som innehåller hello resultaten av hello-frågan. hello exemplet i föregående avsnitt i hello används en metod som kallas `WriteDocuments` toooutput hello sökresultat toohello konsolen:

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

Här är vad hello resultatet ser ut som om för hello frågor i hello föregående avsnitt, antagande om hello ”hotell” index fylls med hello exempeldata i [Import av Data i Azure Search med hello .NET SDK](search-import-data-dotnet.md):

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

hello exempelkod ovan använder hello konsolen toooutput sökresultat. Du behöver också toodisplay sökresultat i ditt eget program. Se [det här exemplet på GitHub](https://github.com/Azure-Samples/search-dotnet-getting-started/tree/master/DotNetSample) ett exempel på hur toorender sökresultat i ett webbprogram med ASP.NET MVC.

