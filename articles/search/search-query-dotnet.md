---
title: "Fråga ett index (.NET API – Azure Search) | Microsoft Docs"
description: "Skapa en sökfråga i Azure Search och använd sökparametrar för att filtrera och sortera sökresultat."
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
ms.openlocfilehash: c3c22b83346269cf3c0327fe3fb98510a6266733
ms.sourcegitcommit: 9890483687a2b28860ec179f5fd0a292cdf11d22
ms.translationtype: HT
ms.contentlocale: sv-SE
ms.lasthandoff: 01/24/2018
---
# <a name="query-your-azure-search-index-using-the-net-sdk"></a>Skicka frågor till ditt Azure Search-index med hjälp av .NET-SDK
> [!div class="op_single_selector"]
> * [Översikt](search-query-overview.md)
> * [Portalen](search-explorer.md)
> * [NET](search-query-dotnet.md)
> * [REST](search-query-rest-api.md)
> 
> 

Den här artikeln beskriver hur du skickar frågor mot ett index med hjälp av [Azure Search .NET SDK](https://aka.ms/search-sdk).

Innan du påbörjar den här genomgången bör du redan ha [skapat ett Azure Search-index](search-what-is-an-index.md) och [fyllt det med data](search-what-is-data-import.md).

> [!NOTE]
> All exempelkod i den här artikeln är skriven i C#. Du hittar den fullständiga källkoden [på GitHub](http://aka.ms/search-dotnet-howto). Du kan läsa om [Azure Search .NET SDK](search-howto-dotnet-sdk.md) och få en mer detaljerad genomgång av exempelkoden.

## <a name="identify-your-azure-search-services-query-api-key"></a>Identifiera API-frågenyckeln för Azure Search-tjänsten
Nu när du har skapat ett Azure Search-index är du nästan redo att skicka frågor med hjälp av .NET SDK. Först måste du skaffa en av API-frågenycklarna som genererades för söktjänsten som du etablerade. .NET SDK skickar den här API-nyckeln vid varje begäran till tjänsten. En giltig nyckel upprättar förtroende, i varje begäran, mellan programmet som skickar begäran och tjänsten som hanterar den.

1. Om du vill hitta din tjänsts API-nycklar kan du logga in på [Azure Portal](https://portal.azure.com/)
2. Gå till Azure Search-tjänstens blad
3. Klicka på nyckelikonen

Tjänsten har *administratörsnycklar* och *frågenycklar*.

* Dina primära och sekundära *administratörsnycklar* ger fullständig behörighet för alla åtgärder, inklusive möjligheten att hantera tjänsten, skapa och ta bort index, indexerare och datakällor. Det finns två nycklar så att du kan fortsätta att använda den sekundära nyckeln om du bestämmer dig för att återskapa den primära nyckeln och tvärtom.
* Dina *frågenycklar* beviljar läsbehörighet till index och dokument och distribueras vanligen till klientprogram som skickar sökförfrågningar.

Du kan använda en av din frågenycklar för att skicka frågor till ett index. Administratörsnycklarna kan även användas för frågor, men du bör använda en frågenyckel i programkoden eftersom detta bättre överensstämmer med [principen om lägsta behörighet](https://en.wikipedia.org/wiki/Principle_of_least_privilege).

## <a name="create-an-instance-of-the-searchindexclient-class"></a>Skapa en instans av klassen SearchIndexClient
Om du vill skicka frågor med Azure Search .NET SDK måste du skapa en instans av klassen `SearchIndexClient`. Den här klassen har flera konstruktorer. Den som du vill använda har namnet på din söktjänst, ett indexnamn och ett `SearchCredentials`-objekt som parametrar. `SearchCredentials` omsluter din API-nyckel.

Koden nedan skapar en ny `SearchIndexClient` för indexet ”hotels” (skapas i [Skapa ett Azure Search-index med .NET SDK](search-create-index-dotnet.md)) med värdena för söktjänstens namn och API-nyckel som lagras i programmets konfigurationsfil (`appsettings.json` i fall med [provapplikation](http://aka.ms/search-dotnet-howto)):

```csharp
private static SearchIndexClient CreateSearchIndexClient(IConfigurationRoot configuration)
{
    string searchServiceName = configuration["SearchServiceName"];
    string queryApiKey = configuration["SearchServiceQueryApiKey"];

    SearchIndexClient indexClient = new SearchIndexClient(searchServiceName, "hotels", new SearchCredentials(queryApiKey));
    return indexClient;
}
```

`SearchIndexClient` har en `Documents`-egenskap. Den här egenskapen tillhandahåller alla metoder som du behöver för att skicka frågor mot Azure Search-index.

## <a name="query-your-index"></a>Skicka frågor mot ditt index
Det är enkelt att söka med .NET SDK. Du anropar bara `Documents.Search`-metoden för din `SearchIndexClient`. Den här metoden stöder några parametrar, inklusive söktexten, tillsammans med ett `SearchParameters`-objekt som kan användas för att ytterligare förfina frågan.

#### <a name="types-of-queries"></a>Typer av frågor
De två viktigaste [frågetyperna](search-query-overview.md#types-of-queries) som du använder är `search` och `filter`. En `search`-fråga söker efter en eller flera termer i alla *sökbara* fält i ditt index. En `filter`-fråga utvärderar ett booleskt uttryck i alla *filtrerbara* fält i ett index. Du kan använda sökningar och filter tillsammans eller separat.

Både sökningar och filtreringar utförs med hjälp av metoden `Documents.Search`. En sökfråga kan skickas i parametern `searchText`, medan ett filteruttryck kan skickas i `Filter`-egenskapen för klassen `SearchParameters`. Om du vill filtrera utan sökning skickar du bara `"*"` för `searchText`-parametern. Om du vill söka utan filtrering lämnar du bara `Filter`-egenskapen odefinierad eller väljer att inte skicka den i en `SearchParameters`-instans över huvud taget.

#### <a name="example-queries"></a>Exempelfrågor
Följande exempelkod visar hur du kan skicka frågor mot ”hotels”-indexet som beskrivs i [Skapa ett Azure Search-index med .NET SDK](search-create-index-dotnet.md#DefineIndex) på några olika sätt. Observera att dokumenten som returneras med sökresultaten är instanser av `Hotel`-klassen som definierades i [Dataimport i Azure Search med .NET SDK](search-import-data-dotnet.md#HotelClass). Exempelkoden utnyttjar en `WriteDocuments`-metod för att mata ut sökresultaten till konsolen. Den här metoden beskrivs i nästa avsnitt.

```csharp
SearchParameters parameters;
DocumentSearchResult<Hotel> results;

Console.WriteLine("Search the entire index for the term 'budget' and return only the hotelName field:\n");

parameters =
    new SearchParameters()
    {
        Select = new[] { "hotelName" }
    };

results = indexClient.Documents.Search<Hotel>("budget", parameters);

WriteDocuments(results);

Console.Write("Apply a filter to the index to find hotels cheaper than $150 per night, ");
Console.WriteLine("and return the hotelId and description:\n");

parameters =
    new SearchParameters()
    {
        Filter = "baseRate lt 150",
        Select = new[] { "hotelId", "description" }
    };

results = indexClient.Documents.Search<Hotel>("*", parameters);

WriteDocuments(results);

Console.Write("Search the entire index, order by a specific field (lastRenovationDate) ");
Console.Write("in descending order, take the top two results, and show only hotelName and ");
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

Console.WriteLine("Search the entire index for the term 'motel':\n");

parameters = new SearchParameters();
results = indexClient.Documents.Search<Hotel>("motel", parameters);

WriteDocuments(results);
```

## <a name="handle-search-results"></a>Hantera sökresultat
Metoden `Documents.Search` returnerar ett `DocumentSearchResult`-objekt som innehåller resultatet av frågan. I exemplet i föregående avsnitt användes metoden `WriteDocuments` för att mata ut sökresultatet till konsolen:

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

Så här ser resultatet ut för frågorna i det förra avsnittet, under antagandet att ”hotels”-indexet har fyllts med exempeldata från [Dataimport i Azure Search med .NET SDK](search-import-data-dotnet.md):

```
Search the entire index for the term 'budget' and return only the hotelName field:

Name: Roach Motel

Apply a filter to the index to find hotels cheaper than $150 per night, and return the hotelId and description:

ID: 2   Description: Cheapest hotel in town
ID: 3   Description: Close to town hall and the river

Search the entire index, order by a specific field (lastRenovationDate) in descending order, take the top two results, and show only hotelName and lastRenovationDate:

Name: Fancy Stay        Last renovated on: 6/27/2010 12:00:00 AM +00:00
Name: Roach Motel       Last renovated on: 4/28/1982 12:00:00 AM +00:00

Search the entire index for the term 'motel':

ID: 2   Base rate: 79.99        Description: Cheapest hotel in town     Description (French): Hôtel le moins cher en ville      Name: Roach Motel       Category: Budget        Tags: [motel, budget]   Parking included: yes   Smoking allowed: yes    Last renovated on: 4/28/1982 12:00:00 AM +00:00 Rating: 1/5     Location: Latitude 49.678581, longitude -122.131577
```

Exempelkoden ovan använder konsolen för att mata ut sökresultat. Du behöver också visa sökresultat i ditt eget program. Ett exempel på hur du kan visa sökresultat i en ASP.NET MVC-baserad webbapp finns i [det här exemplet på GitHub](https://github.com/Azure-Samples/search-dotnet-getting-started/tree/master/DotNetSample).

