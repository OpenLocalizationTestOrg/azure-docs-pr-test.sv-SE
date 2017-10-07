---
title: "aaa ”skapa ett index (.NET API - Azure Search) | Microsoft Docs ”"
description: Skapa ett index i kod med hello Azure Search .NET SDK.
services: search
documentationcenter: 
author: brjohnstmsft
manager: jhubbard
editor: 
tags: azure-portal
ms.assetid: 3a851647-fc7b-4fb6-8506-6aaa519e77cd
ms.service: search
ms.devlang: dotnet
ms.workload: search
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.date: 05/22/2017
ms.author: brjohnst
ms.openlocfilehash: 7fa4030b8c3565bc02b1d6bb4426331657cf3a5f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-azure-search-index-using-hello-net-sdk"></a>Skapa ett Azure Search-index med hello .NET SDK
> [!div class="op_single_selector"]
> * [Översikt](search-what-is-an-index.md)
> * [Portalen](search-create-index-portal.md)
> * [.NET](search-create-index-dotnet.md)
> * [REST](search-create-index-rest-api.md)
> 
> 

Den här artikeln vägleder dig genom hello processen att skapa ett Azure Search [index](https://docs.microsoft.com/rest/api/searchservice/Create-Index) med hello [Azure Search .NET SDK](https://aka.ms/search-sdk).

Innan du följer den här guiden och skapar ett index bör du redan ha [skapat en Azure Search-tjänst](search-create-service-portal.md).

> [!NOTE]
> All exempelkod i den här artikeln är skriven i C#. Du kan hitta hello fullständig källkoden [på GitHub](http://aka.ms/search-dotnet-howto). Du kan också läsa om hello [Azure Search .NET SDK](search-howto-dotnet-sdk.md) för en mer detaljerad genomgång av hello exempelkod.


## <a name="identify-your-azure-search-services-admin-api-key"></a>Identifiera Azure Search-tjänstens API-administratörsnyckel
Nu när du har etablerat en Azure Search-tjänst är nästan klar tooissue begäranden mot din tjänstslutpunkten med hello .NET SDK. Först behöver tooobtain en av hello admin api-nycklar som har genererats för hello söktjänsten du etablerat. hello .NET SDK skickar den här api-nyckel för varje begäran tooyour-tjänst. Med en giltig nyckel upprättar förtroende mellan hello programmet skickar hello begäran och hello-tjänsten som hanterar den fall per begäran.

1. toofind din tjänst api-nycklar, logga in toohello [Azure-portalen](https://portal.azure.com/)
2. Gå tooyour Azure Search service-bladet
3. Klicka på hello ”nycklar”-ikon

Tjänsten har *administratörsnycklar* och *frågenycklar*.

* Din primära och sekundära *admin nycklar* bevilja fullständiga rättigheter tooall åtgärder, inklusive hello möjlighet toomanage hello service, skapa och ta bort index, indexerare och datakällor. Det finns två nycklar så att du kan fortsätta toouse hello sekundärnyckeln om du väljer tooregenerate hello primärnyckel och vice versa.
* Din *fråga nycklar* bevilja läsbehörighet tooindexes och dokument och är vanligtvis distribuerade tooclient program som utfärdar search-begäranden.

För hello syftet med att skapa ett index, kan du använda antingen primär eller sekundär administrationsnyckel.

<a name="CreateSearchServiceClient"></a>

## <a name="create-an-instance-of-hello-searchserviceclient-class"></a>Skapa en instans av hello SearchServiceClient-klass
toostart med hello Azure Search .NET SDK måste du toocreate en instans av hello `SearchServiceClient` klass. Den här klassen har flera konstruktorer. hello du tar din sökning tjänstnamn och ett `SearchCredentials` objekt som parametrar. `SearchCredentials` omsluter din API-nyckel.

hello koden nedan skapar en ny `SearchServiceClient` med värden för hello Sök tjänstnamn och api-nyckel som lagras i hello programmets konfigurationsfil (`appsettings.json` hello gäller hello [exempelprogram](http://aka.ms/search-dotnet-howto)):

```csharp
private static SearchServiceClient CreateSearchServiceClient(IConfigurationRoot configuration)
{
    string searchServiceName = configuration["SearchServiceName"];
    string adminApiKey = configuration["SearchServiceAdminApiKey"];

    SearchServiceClient serviceClient = new SearchServiceClient(searchServiceName, new SearchCredentials(adminApiKey));
    return serviceClient;
}
```

`SearchServiceClient` har en `Indexes`-egenskap. Den här egenskapen stöder alla hello metoderna du behöver toocreate, lista, uppdatera eller ta bort Azure Search-index.

> [!NOTE]
> Hej `SearchServiceClient` klassen hanterar anslutningar tooyour search-tjänsten. I ordning tooavoid öppna för många anslutningar, bör du tooshare en instans av `SearchServiceClient` i ditt program om möjligt. Dess metoder är trådsäker tooenable sådana delning.
> 
> 

<a name="DefineIndex"></a>

## <a name="define-your-azure-search-index"></a>Definiera ditt Azure Search-index
En enda anrop toohello `Indexes.Create` metoden skapar ditt index. Den här metoden tar som en parameter ett `Index`-objekt som definierar ditt Azure Search-index. Du behöver toocreate ett `Index` objekt och initiera den på följande sätt:

1. Ange hello `Name` -egenskapen för hello `Index` objektet toohello namnet på ditt index.
2. Ange hello `Fields` -egenskapen för hello `Index` tooan objektmatris av `Field` objekt. hello enklaste sättet toocreate hello `Field` objekt är genom att anropa hello `FieldBuilder.BuildForType` metoden och skickar en modellklass för hello typparameter. En modellklass har egenskaper som toohello mappning av ditt index. Detta ger dig toobind dokument från din sökning index tooinstances av din modellklass.

> [!NOTE]
> Om du inte planerar toouse en modellklass, kan du fortfarande definiera ditt index genom att skapa `Field` objekt direkt. Du kan ange hello namnet på hello fältet toohello konstruktor, tillsammans med hello datatyp (eller analyzer för strängfält). Du kan också ange andra egenskaper som `IsSearchable`, `IsFilterable` osv.
>
>

Det är viktigt att du behåller dina Sök upplevelse och företag behov i åtanke när du skapar ditt index som varje fält måste tilldelas hello [lämpliga egenskaper](https://docs.microsoft.com/rest/api/searchservice/Create-Index). Dessa egenskaper styra vilka söka funktioner (filtrering, faceting, sortering fulltextsökning osv) gäller toowhich fält. En egenskap som du inte uttryckligen anger hello `Field` klass som standard toodisabling hello motsvarande sökfunktionen såvida du inte uttryckligen aktiverar den.

I vårt exempel har vi gett indexet namnet ”hotels” och definierat fälten med en modellklass. Varje egenskap för hello modellklass har attribut som avgör hello sökrelaterade beteenden för hello motsvarande fältet. Hej modellklass definieras enligt följande:

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

Vi har noggrant valt hello attribut för varje egenskap baserat på hur vi tror att de ska användas i ett program. Till exempel är det troligt att personer som söker efter hotell är intresserad av nyckelordsmatchning på hello `description` fältet, så vi Aktivera textsökning för fältet genom att lägga till hello `IsSearchable` attributet toohello `Description` egenskapen.

Observera att ett fält i ditt index av typen `string` hello betecknas som hello *nyckeln* fält genom att lägga till hello `Key` attribut (finns `HotelId` i hello ovanstående exempel).

hello indexdefinitionen ovan använder ett språk analyzer för hello `description_fr` eftersom den är avsedd toostore franska text. Se [hello Language support avsnittet](https://docs.microsoft.com/rest/api/searchservice/Language-support) samt hello motsvarande [blogginlägget](https://azure.microsoft.com/blog/language-support-in-azure-search/) mer information om språkanalys.

> [!NOTE]
> Som standard används hello namn för varje egenskap i klassen modellen som hello namnet på hello motsvarande fält i hello index. Om du vill toomap alla egendom namn toocamel skiftläge fältnamn, markera hello klass med hello `SerializePropertyNamesAsCamelCase` attribut. Om du vill toomap tooa annat namn, kan du använda hello `JsonProperty` attribut som hello `DescriptionFr` egenskapen ovan. Hej `JsonProperty` attributet företräde framför hello `SerializePropertyNamesAsCamelCase` attribut.
> 
> 

Nu när vi har definierat en modellklass kan vi lätt skapa en indexdefinition:

```csharp
var definition = new Index()
{
    Name = "hotels",
    Fields = FieldBuilder.BuildForType<Hotel>()
};
```

## <a name="create-hello-index"></a>Skapa hello index
Nu när du har en initierad `Index` objekt du kan skapa hello index genom att anropa `Indexes.Create` på din `SearchServiceClient` objekt:

```csharp
serviceClient.Indexes.Create(definition);
```

För en lyckad begäran returnerar hello-metoden normalt. Om det finns ett problem med hello begäran, till exempel en ogiltig parameter, hello metoden genereras `CloudException`.

När du är klar med ett index och vill toodelete den bara anropa hello `Indexes.Delete` metod på din `SearchServiceClient`. Detta är till exempel hur vi skulle ta bort hello ”hotell” index:

```csharp
serviceClient.Indexes.Delete("hotels");
```

> [!NOTE]
> hello exempelkoden i den här artikeln använder hello synkrona metoder för hello Azure Search .NET SDK för enkelhetens skull. Vi rekommenderar att du använder hello asynkrona metoder i ditt eget program tookeep dem skalbar och svarstid. Till exempel i hello exemplen ovan du kan använda `CreateAsync` och `DeleteAsync` i stället för `Create` och `Delete`.
> 
> 

## <a name="next-steps"></a>Nästa steg
När du har skapat ett Azure Search-index, kommer du att redo för[överföra innehållet till hello index](search-what-is-data-import.md) så att du kan börja söka data.

