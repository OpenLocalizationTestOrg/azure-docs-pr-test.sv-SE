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
# <a name="how-toouse-azure-search-from-a-net-application"></a>Hur toouse Azure söka från ett .NET-program
Den här artikeln är en genomgång tooget igång med hello [Azure Search .NET SDK](https://aka.ms/search-sdk). Du kan använda hello .NET SDK tooimplement en omfattande söka upplevelse i ditt program med Azure Search.

## <a name="whats-in-hello-azure-search-sdk"></a>Vad är hello Azure Search SDK
hello SDK består av ett klientbibliotek `Microsoft.Azure.Search`. Det gör du toomanage ditt index, datakällor och indexerare, samt överföra hantera dokument och köra frågor alla utan toodeal med hello detaljer om HTTP- och JSON.

hello klientbiblioteket definierar klasser som `Index`, `Field`, och `Document`, samt åtgärder som `Indexes.Create` och `Documents.Search` på hello `SearchServiceClient` och `SearchIndexClient` klasser. De här klasserna är uppdelade i hello följande namnområden:

* [Microsoft.Azure.Search](https://docs.microsoft.com/dotnet/api/microsoft.azure.search)
* [Microsoft.Azure.Search.Models](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models)

hello nuvarande version av hello Azure Search .NET SDK är nu allmänt tillgänglig. Om du vill ha feedback tooprovide oss tooincorporate i nästa version av hello, finns våra [feedback sidan](https://feedback.azure.com/forums/263029-azure-search/).

hello .NET SDK stöder version `2016-09-01` av hello [Azure Search REST API](https://docs.microsoft.com/rest/api/searchservice/). Den här versionen innehåller nu stöd för anpassade analyzers och stöd för Azure Blob och Azure Table indexeraren. Förhandsgranska funktioner som är *inte* en del av den här versionen, till exempel stöd för indexering av JSON- och CSV-filer finns i [preview](search-api-2015-02-28-preview.md) och är tillgängliga via hello äldre [2.0-förhandsversionen av hello .NET SDK ](https://aka.ms/search-sdk-preview).

Detta SDK stöder inte [hanteringsåtgärder](https://docs.microsoft.com/rest/api/searchmanagement/) , till exempel skapa och skala Search-tjänster och hantera API-nycklar. Om du behöver toomanage din Sök efter resurser från en .NET-program, kan du använda hello [Azure Search .NET Management SDK](https://aka.ms/search-mgmt-sdk).

## <a name="upgrading-toohello-latest-version-of-hello-sdk"></a>Uppgradera toohello senaste versionen av hello SDK
Om du redan använder en äldre version av hello Azure Search .NET SDK och du vill ha tooupgrade toohello ny allmänt tillgänglig version, [i den här artikeln](search-dotnet-sdk-migration.md) förklarar hur.

## <a name="requirements-for-hello-sdk"></a>Krav för hello SDK
1. Visual Studio 2017.
2. Din egen Azure Search-tjänsten. I ordning toouse hello SDK behöver du hello namnet på din tjänst och en eller flera API-nycklar. [Skapa en tjänst i hello portal](search-create-service-portal.md) hjälper dig genom stegen.
3. Hämta hello Azure Search .NET SDK [NuGet-paketet](http://www.nuget.org/packages/Microsoft.Azure.Search) med hjälp av ”hantera NuGet-paket” i Visual Studio. Sök bara efter hello paketnamnet `Microsoft.Azure.Search` på NuGet.org.

hello Azure Search .NET SDK stöder hello .NET Framework 4.6 och .NET Core-program.

## <a name="core-scenarios"></a>Huvudscenarier
Det finns flera saker du behöver toodo i sökprogrammet. I den här självstudiekursen kommer tar vi upp dessa huvudscenarier:

* Skapa ett index
* Fylla hello index med dokument
* Söker efter dokument med hjälp av fulltextsökning och filter

hello exempelkod som följer visar var och en av dessa. Känna sig fria toouse hello kodfragment i ditt eget program.

### <a name="overview"></a>Översikt
hello exempelprogrammet vi kommer att utforska skapar en ny index med namnet ”hotell” fylls med några dokument och sedan kör vissa sökfrågor. Här är hello huvudsakliga program, visar hello övergripande flöde:

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
> Du kan hitta hello fullständig källkoden för hello exempelprogrammet används i denna genomgång på [GitHub](https://github.com/Azure-Samples/search-dotnet-getting-started/tree/master/DotNetHowTo).
> 
>

Vi går igenom detta steg för steg. Vi måste först toocreate en ny `SearchServiceClient`. Det här objektet kan toomanage index. I ordning tooconstruct en måste tooprovide ditt Azure Search-tjänstnamn samt ett administrations-API-nyckel. Du kan ange den här informationen i hello `appsettings.json` -filen för hello [exempelprogram](https://github.com/Azure-Samples/search-dotnet-getting-started/tree/master/DotNetHowTo).

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
> Om du anger en felaktig nyckel (till exempel en fråga nyckel som där en administratör för krävdes) hello `SearchServiceClient` genereras en `CloudException` med hello fel meddelandet ”otillåtna” hello första gången du anropa en åtgärdsmetod, som `Indexes.Create`. Om detta händer tooyou dubbelkolla våra API-nyckel.
> 
> 

hello anropa nästa raderna metoder toocreate ett index med namnet ”hotell” ta bort den först om det redan finns. Vi kommer att gå igenom dessa metoder lite senare.

```csharp
Console.WriteLine("{0}", "Deleting index...\n");
DeleteHotelsIndexIfExists(serviceClient);

Console.WriteLine("{0}", "Creating index...\n");
CreateHotelsIndex(serviceClient);
```

Därefter måste hello index toobe fylls i. toodo detta, behöver vi en `SearchIndexClient`. Det finns två sätt tooobtain en: genom att skapa den eller genom att anropa `Indexes.GetClient` på hello `SearchServiceClient`. Vi använder hello senare i informationssyfte.

```csharp
ISearchIndexClient indexClient = serviceClient.Indexes.GetClient("hotels");
```

> [!NOTE]
> I ett typiskt sökprogram hanteras indexhanteringen och ifyllningen av en separat komponent från sökfrågorna. `Indexes.GetClient`är praktiskt för att fylla ett index eftersom du sparar du hello problem för att ge en annan `SearchCredentials`. Detta åstadkoms genom att skicka hello admin-nyckel som du använt toocreate hello `SearchServiceClient` toohello nya `SearchIndexClient`. Men i hello en del av programmet som frågor körs, är det bättre toocreate hello `SearchIndexClient` direkt så att du kan skicka in en nyckel för frågan i stället för en administrationsnyckel. Detta är konsekventa med hello principen om minsta behörighet och hjälper toomake programmet säkrare. Du hittar mer information om administratör och fråga nycklar [här](https://docs.microsoft.com/rest/api/searchservice/#authentication-and-authorization).
> 
> 

Nu när vi har en `SearchIndexClient`, fyller vi hello index. Detta görs med en annan metod som går igenom senare.

```csharp
Console.WriteLine("{0}", "Uploading documents...\n");
UploadDocuments(indexClient);
```

Slutligen vi köra några sökningar och visa resultat som hello. Den här gången vi använder en annan `SearchIndexClient`:

```csharp
ISearchIndexClient indexClientForQueries = CreateSearchIndexClient(configuration);

RunQueries(indexClientForQueries);
```

Vi ska titta närmare på hello `RunQueries` metoden senare. Här är hello kod toocreate hello nya `SearchIndexClient`:

```csharp
private static SearchIndexClient CreateSearchIndexClient(IConfigurationRoot configuration)
{
    string searchServiceName = configuration["SearchServiceName"];
    string queryApiKey = configuration["SearchServiceQueryApiKey"];

    SearchIndexClient indexClient = new SearchIndexClient(searchServiceName, "hotels", new SearchCredentials(queryApiKey));
    return indexClient;
}
```

Nu vi använder en nyckel för frågan eftersom vi inte behöver skrivbehörighet toohello index. Du kan ange den här informationen i hello `appsettings.json` -filen för hello [exempelprogram](https://github.com/Azure-Samples/search-dotnet-getting-started/tree/master/DotNetHowTo).

Om du kör det här programmet med ett giltigt tjänstnamn och API-nycklar hello utdata ska se ut så här:

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

hello fullständig programs källkod för hello tillhandahålls hello slutet av den här artikeln.

Vi ska ta en närmare titt på hello metoderna anropas av `Main`.

### <a name="creating-an-index"></a>Skapa ett index
När du har skapat en `SearchServiceClient`, hello nästa sak `Main` har är delete hello ”hotell” index om den redan finns. Som görs genom att följa metoden hello:

```csharp
private static void DeleteHotelsIndexIfExists(SearchServiceClient serviceClient)
{
    if (serviceClient.Indexes.Exists("hotels"))
    {
        serviceClient.Indexes.Delete("hotels");
    }
}
```

Den här metoden använder hello anges `SearchServiceClient` toocheck om finns hello index och ta bort den.

> [!NOTE]
> hello exempelkoden i den här artikeln använder hello synkrona metoder för hello Azure Search .NET SDK för enkelhetens skull. Vi rekommenderar att du använder hello asynkrona metoder i ditt eget program tookeep dem skalbar och svarstid. Till exempel i hello metod du kan använda `ExistsAsync` och `DeleteAsync` i stället för `Exists` och `Delete`.
> 
> 

Nästa `Main` skapar ett nytt ”hotell” index genom att anropa den här metoden:

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

Den här metoden skapar en ny `Index` objekt med en lista över `Field` objekt som definierar hello schemat för ny hello-index. Varje fält har ett namn, datatyp och flera attribut som definierar dess sökbeteendet. Hej `FieldBuilder` klassen använder reflektion toocreate en lista över `Field` objekt för hello index genom att undersöka hello allmänna egenskaper och attribut för hello anges `Hotel` modellklass. Vi ska titta närmare på hello `Hotel` klassen vid ett senare tillfälle.

> [!NOTE]
> Du kan alltid skapa hello lista över `Field` objekt direkt i stället för `FieldBuilder` om det behövs. Till exempel kanske du inte vill toouse en modellklass eller du kan behöva toouse en befintlig modellklass som du inte vill toomodify genom att lägga till attribut.
>
> 

Dessutom toofields, du kan också lägga till bedömningsprofil profiler, suggesters eller CORS alternativ toohello Index (dessa har uteslutits från hello exemplet planeringsaspekter). Du hittar mer information om hello-objektet och dess komponenter som ingår i hello [SDK referens](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.index#microsoft_azure_search_models_index)samt som i hello [Azure Search REST API-referensen](https://docs.microsoft.com/rest/api/searchservice/).

### <a name="populating-hello-index"></a>Fylla hello index
hello nästa steg i `Main` är toopopulate hello nyskapade index. Detta görs i hello följande metod:

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

Den här metoden har fyra delar. hello först skapar en matris med `Hotel` objekt som ska användas som indata tooupload toohello indexet. Dessa data är hårdkodat för enkelhetens skull. I ditt eget program kommer troligen dina data från en extern datakälla, till exempel en SQL-databas.

hello andra delen skapar en `IndexBatch` som innehåller hello dokument. Du anger hello åtgärden som du vill tooapply toohello batch hello tidpunkt då du skapar det, i det här fallet genom att anropa `IndexBatch.Upload`. hello batch är överförda toohello Azure Search index av hello `Documents.Index` metod.

> [!NOTE]
> I det här exemplet vi bara ladda upp dokument. Om du vill använda toomerge ändringar i befintliga dokument eller ta bort dokument kan du skapa batchar genom att anropa `IndexBatch.Merge`, `IndexBatch.MergeOrUpload`, eller `IndexBatch.Delete` i stället. Du kan också blanda olika åtgärder i en enda grupp genom att anropa `IndexBatch.New`, som tar en samling `IndexAction` objekt, som talar om för Azure Search tooperform en viss åtgärd i ett dokument. Du kan skapa varje `IndexAction` med åtgärden genom att anropa hello motsvarande metod som `IndexAction.Merge`, `IndexAction.Upload`och så vidare.
> 
> 

hello tredje delen av den här metoden är ett catch-block som hanterar ett viktigt fel ärende för indexering. Om din Azure Search-tjänst misslyckas tooindex vissa hello dokument i hello batch en `IndexBatchException` genereras av `Documents.Index`. Detta kan inträffa om du indexerar dokument när tjänsten är hårt belastad. **Vi rekommenderar starkt att du uttryckligen hanterar den här situationen i din kod.** Du kan fördröja och försök indexering hello-dokument som misslyckades du kan logga in och fortsätta som hello exempel har eller kan du göra något annat beroende på programmets konsekvens kraven

> [!NOTE]
> Du kan använda hello `FindFailedActionsToRetry` metoden tooconstruct en ny grupp som innehåller endast hello åtgärder som misslyckades i ett tidigare anrop för`Index`. hello metoden dokumenteras [här](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.indexbatchexception#Microsoft_Azure_Search_IndexBatchException_FindFailedActionsToRetry_Microsoft_Azure_Search_Models_IndexBatch_System_String_) och det finns en beskrivning av hur tooproperly använder den [på StackOverflow](http://stackoverflow.com/questions/40012885/azure-search-net-sdk-how-to-use-findfailedactionstoretry).
>
>

Slutligen hello `UploadDocuments` metoden fördröjningar i två sekunder. Indexering sker asynkront i Azure Search-tjänsten så hello exempelprogrammet måste toowait en kort tid tooensure att hello dokument är tillgängliga för sökning. Fördröjningar som den här är normalt endast nödvändiga i demonstrationer, tester och exempelprogram.

#### <a name="how-hello-net-sdk-handles-documents"></a>Hanteringen av dokument i hello .NET SDK
Du kanske undrar hur hello Azure Search .NET SDK är kan tooupload instanser av en användardefinierad klass som `Hotel` toohello index. toohelp besvara frågan, ska vi titta på hello `Hotel` klass:

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

hello först öppna toonotice är att varje offentlig egenskap för `Hotel` motsvarar tooa fält i hello indexdefinitionen, men med en viktig skillnad: hello varje fält börjar med en gemen bokstav (”slitbanor fallet”) vid hello namnet på varje offentlig Egenskapen för `Hotel` börjar med en versal (”Pascal fall”). Det här är ett vanligt scenario i .NET-program som utför databindning där hello målschemat är utanför hello kontroll över hello programutvecklaren. Du kan se hello SDK toomap hello egenskapen namn toocamel fall automatiskt med hello snarare än att tooviolate hello .NET naming riktlinjer genom att egenskapen namn slitbanor skiftläge `[SerializePropertyNamesAsCamelCase]` attribut.

> [!NOTE]
> hello Azure Search .NET SDK använder hello [NewtonSoft JSON.NET](http://www.newtonsoft.com/json/help/html/Introduction.htm) biblioteket tooserialize och deserialisera din anpassade modellen objekt tooand från JSON. Du kan anpassa den här serialiseringen om det behövs. Mer information finns i [anpassad serialisering med JSON.NET](#JsonDotNet).
> 
> 

hello andra sak toonotice är hello attribut som `IsFilterable`, `IsSearchable`, `Key`, och `Analyzer` som skapa snygga varje offentlig egenskap. Dessa attribut mappar direkt toohello [motsvarande attribut för hello Azure Search index](https://docs.microsoft.com/rest/api/searchservice/create-index#request). Hej `FieldBuilder` klassen använder dessa tooconstruct fältdefinitioner för hello index.

hello tredje viktigt om hello `Hotel` klass är hello datatyperna hello offentliga egenskaper. hello .NET-typer av de här egenskaperna mappa tootheir motsvarande fälttyp i hello indexdefinitionen. Till exempel hello `Category` strängegenskap mappar toohello `category` fält som är av typen `Edm.String`. Det finns liknande typ mappningar mellan `bool?` och `Edm.Boolean`, `DateTimeOffset?` och `Edm.DateTimeOffset`, etc. hello särskilda regler för mappning hello dokumenteras med hello `Documents.Get` metod i hello [Azure Search .NET SDK Referens för](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.idocumentsoperations#Microsoft_Azure_Search_IDocumentsOperations_GetWithHttpMessagesAsync__1_System_String_System_Collections_Generic_IEnumerable_System_String__Microsoft_Azure_Search_Models_SearchRequestOptions_System_Collections_Generic_Dictionary_System_String_System_Collections_Generic_List_System_String___System_Threading_CancellationToken_). Hej `FieldBuilder` klassen tar hand om mappningen för dig, men det kan fortfarande vara användbara toounderstand om du behöver tootroubleshoot problemen serialisering.

Den här möjligheten toouse egna klasser som dokument fungerar i båda riktningarna; Du kan också hämta sökresultat och hello SDK automatiskt deserialisera dem tooa typ du väljer, som vi kommer att se i hello nästa avsnitt.

> [!NOTE]
> hello Azure Search .NET SDK stöder också dynamiskt skrev dokument med hjälp av hello `Document` klassen, som är en nyckel/värde-mappning av toofield värdena för fältet namn. Detta är användbart i scenarier där du inte vet hello indexeringsschema i designläge, eller om det skulle vara olämplig toobind toospecific modellen klasser. Alla hello metoder i hello SDK som handlar om dokument har överlagringar som fungerar med hello `Document` klass, samt strikt typkontroll överlagringar som har en generisk typparameter. Endast hello senare används i hello exempelkoden i den här självstudiekursen. Hej `Document` klassen ärver från `Dictionary<string, object>`. Du kan hitta annan information [här](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.document#microsoft_azure_search_models_document).
> 
> 

**Varför du bör använda datatyper som kan ha värdet null**

När du skapar egna modellen klasser toomap tooan Azure Search index, rekommenderar vi deklarerar egenskaper för värdetyper som `bool` och `int` toobe kan ha värdet null (till exempel `bool?` i stället för `bool`). Om du använder en icke-nullbar egenskapen, det finns också**garantera** att inga dokument i indexet innehåller ett null-värde för hello motsvarande fält. Varken hello SDK eller hello Azure Search-tjänsten kan du tooenforce detta.

Detta är inte bara ett hypotetiskt problem: Tänk dig ett scenario där du lägger till ett nytt fält tooan befintliga index som är av typen `Edm.Int32`. När du har uppdaterat hello indexdefinitionen har alla dokument som ett null-värde för det nya fältet (eftersom alla typer inte kan vara null i Azure Search). Om du använder en modellklass sedan med en icke-nullbar `int` -egenskapen för det här fältet får du en `JsonSerializationException` så här när du försöker tooretrieve dokument:

    Error converting value {null} tootype 'System.Int32'. Path 'IntValue'.

Av den anledningen rekommenderar vi att du använder nullbara typer i dina modellklasser som bästa praxis.

<a name="JsonDotNet"></a>

#### <a name="custom-serialization-with-jsonnet"></a>Anpassad serialisering med JSON.NET
hello SDK använder JSON.NET för serialisering och avserialisering av dokument. Du kan anpassa serialisering och avserialisering vid behov genom att definiera egna `JsonConverter` eller `IContractResolver` (se hello [JSON.NET dokumentationen](http://www.newtonsoft.com/json/help/html/Introduction.htm) för mer information). Detta kan vara användbart när du vill tooadapt en befintlig modellklass från ditt program för användning med Azure Search och andra mer avancerade scenarier. Med anpassad serialisering kan du till exempel:

* Ta med eller undanta vissa egenskaper hos din modellklass lagras som dokumentfält.
* Mappa egenskapsnamn i koden och fältnamnen i ditt index.
* Skapa anpassade attribut som kan användas för att mappa egenskaper toodocument fält.

Du kan hitta exempel på att implementera anpassad serialisering i hello kontroller för hello Azure Search .NET SDK på GitHub. En bra utgångspunkt är [mappen](https://github.com/Azure/azure-sdk-for-net/tree/AutoRest/src/Search/Search.Tests/Tests/Models). Den innehåller klasser som används av hello anpassad serialisering tester.

### <a name="searching-for-documents-in-hello-index"></a>Söker efter dokument i hello index
hello sista steget i hello exempelprogrammet är toosearch för vissa dokument i hello index. hello följa metoden gör detta:

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

Varje gång det kör en fråga, den här metoden först skapar en ny `SearchParameters` objekt. Detta är att använda toospecify ytterligare alternativ för hello frågan som sortering, filtrering, sidindelning och faceting. I den här metoden vi konfigurerar hello `Filter`, `Select`, `OrderBy`, och `Top` -egenskapen för olika frågor. Alla hello `SearchParameters` egenskaper dokumenteras [här](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.searchparameters).

hello nästa steg är tooactually köra hello sökfråga. Detta görs med hjälp av hello `Documents.Search` metod. För varje fråga vi skicka hello Sök text toouse som en sträng (eller `"*"` om det finns ingen Söktext), plus hello söka parametrarna som skapats tidigare. Vi även ange `Hotel` som hello typparameter för `Documents.Search`, som visar hello SDK toodeserialize dokument i hello sökresultat i objekt av typen `Hotel`.

> [!NOTE]
> Du hittar mer information om hello Sök frågesyntaxen uttryck [här](https://docs.microsoft.com/rest/api/searchservice/Simple-query-syntax-in-Azure-Search).
> 
> 

Slutligen efter varje fråga den här metoden går exempelkoden igenom alla hello matchningar i hello sökresultat, skriva ut varje dokument toohello konsol:

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

Låt oss ta en närmare titt på var och en av hello frågor i sin tur. Här är hello kod tooexecute hello första frågan:

```csharp
parameters =
    new SearchParameters()
    {
        Select = new[] { "hotelName" }
    };

results = indexClient.Documents.Search<Hotel>("budget", parameters);

WriteDocuments(results);
```

I det här fallet vi söker efter hotell som matchar hello ordet ”budget” och vi vill tooget tillbaka bara hello hotell namn, som anges av hello `Select` parameter. Här följer hello resultat:

    Name: Roach Motel

Sedan vi vill toofind hello hotell som mindre än 150 USD automatiskt varje natt kostar och bara returnera hello hotell ID och beskrivning:

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

Den här frågan använder en OData `$filter` uttryck, `baseRate lt 150`, toofilter hello dokument i hello index. Du hittar mer information om hello OData-syntax som har stöd för Azure Search [här](https://docs.microsoft.com/rest/api/searchservice/OData-Expression-Syntax-for-Azure-Search).

Här följer hello resultaten av hello-frågan:

    ID: 2   Description: Cheapest hotel in town
    ID: 3   Description: Close tootown hall and hello river

Därefter vill vi toofind hello översta två hotell som har varit senast renoverade och visar hello hotell namn och datum för senaste renovering. Här är hello kod: 

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

I detta fall kan vi använda igen OData syntax toospecify hello `OrderBy` parameter som `lastRenovationDate desc`. Vi också ange `Top` too2 tooensure vi bara hämta hello två översta dokument. Som tidigare kan du ange `Select` toospecify vilka fält som ska returneras.

Här följer hello resultat:

    Name: Fancy Stay        Last renovated on: 6/27/2010 12:00:00 AM +00:00
    Name: Roach Motel       Last renovated on: 4/28/1982 12:00:00 AM +00:00

Slutligen vill vi toofind alla hotell som matchar hello ordet ”översikt”:

```csharp
parameters = new SearchParameters();
results = indexClient.Documents.Search<Hotel>("motel", parameters);

WriteDocuments(results);
```

Här är hello-resultat, som omfattar alla fält eftersom vi inte har angett hello `Select` egenskapen:

    ID: 2   Base rate: 79.99        Description: Cheapest hotel in town     Description (French): Hôtel le moins cher en ville      Name: Roach Motel       Category: Budget        Tags: [motel, budget]   Parking included: yes   Smoking allowed: yes    Last renovated on: 4/28/1982 12:00:00 AM +00:00 Rating: 1/5     Location: Latitude 49.678581, longitude -122.131577

Det här steget har slutförts hello kursen, men stoppar inte här. **Nästa steg** innehåller ytterligare resurser för att lära dig mer om Azure Search.

## <a name="next-steps"></a>Nästa steg
* Bläddra hello referenser för hello [.NET SDK](https://docs.microsoft.com/dotnet/api/microsoft.azure.search) och [REST API](https://docs.microsoft.com/rest/api/searchservice/).
* Fördjupa dina kunskaper genom [videor och andra exempel och självstudier](search-video-demo-tutorial-list.md).
* Granska [namngivningskonventioner](https://docs.microsoft.com/rest/api/searchservice/Naming-rules) toolearn hello regler för namngivning av olika objekt.
* Granska [datatyper som stöds](https://docs.microsoft.com/rest/api/searchservice/Supported-data-types) i Azure Search.
