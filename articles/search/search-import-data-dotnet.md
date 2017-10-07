---
title: "aaa ”ladda upp data (.NET - Azure Search) | Microsoft Docs ”"
description: "Lär dig hur tooupload data tooan index i Azure Search med hello .NET SDK."
services: search
documentationcenter: 
author: brjohnstmsft
manager: jhubbard
editor: 
tags: 
ms.assetid: 0e0e7e7b-7178-4c26-95c6-2fd1e8015aca
ms.service: search
ms.devlang: dotnet
ms.workload: search
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.date: 01/13/2017
ms.author: brjohnst
ms.openlocfilehash: 78ddbefb522884d1f61cb275c25c091487aee639
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="upload-data-tooazure-search-using-hello-net-sdk"></a>Överför data tooAzure Sök med hjälp av hello .NET SDK
> [!div class="op_single_selector"]
> * [Översikt](search-what-is-data-import.md)
> * [NET](search-import-data-dotnet.md)
> * [REST](search-import-data-rest-api.md)
> 
> 

Den här artikeln visar hur toouse hello [Azure Search .NET SDK](https://aka.ms/search-sdk) tooimport data till ett Azure Search-index.

Innan du påbörjar den här genomgången bör du redan ha [skapat ett Azure Search-index](search-what-is-an-index.md). Den här artikeln förutsätter också att du redan har skapat en `SearchServiceClient` objekt som visas i [skapa ett Azure Search-index med hello .NET SDK](search-create-index-dotnet.md#CreateSearchServiceClient).

> [!NOTE]
> All exempelkod i den här artikeln är skriven i C#. Du kan hitta hello fullständig källkoden [på GitHub](http://aka.ms/search-dotnet-howto). Du kan också läsa om hello [Azure Search .NET SDK](search-howto-dotnet-sdk.md) för en mer detaljerad genomgång av hello exempelkod.

I toopush dokument till ditt index med hello .NET SDK, måste du:

1. Skapa en `SearchIndexClient` objektet tooconnect tooyour sökindex.
2. Skapa en `IndexBatch` som innehåller hello dokument toobe lagts till, ändrats eller tagits bort.
3. Anropa hello `Documents.Index` metod för din `SearchIndexClient` toosend hello `IndexBatch` tooyour sökindex.

## <a name="create-an-instance-of-hello-searchindexclient-class"></a>Skapa en instans av hello SearchIndexClient-klass
tooimport data till ditt index med hjälp av hello Azure Search .NET SDK måste du toocreate en instans av hello `SearchIndexClient` klass. Du kan skapa den här instansen själv, men det är enklare om du redan har en `SearchServiceClient` instans toocall dess `Indexes.GetClient` metod. Här är exempelvis hur du vill hämta en `SearchIndexClient` för hello index med namnet ”hotell” från en `SearchServiceClient` med namnet `serviceClient`:

```csharp
ISearchIndexClient indexClient = serviceClient.Indexes.GetClient("hotels");
```

> [!NOTE]
> I ett typiskt sökprogram hanteras indexhanteringen och ifyllningen av en separat komponent från sökfrågorna. `Indexes.GetClient`är praktiskt för att fylla ett index eftersom du sparar du hello problem för att ge en annan `SearchCredentials`. Detta åstadkoms genom att skicka hello admin-nyckel som du använt toocreate hello `SearchServiceClient` toohello nya `SearchIndexClient`. Men i hello en del av programmet som frågor körs, är det bättre toocreate hello `SearchIndexClient` direkt så att du kan skicka in en nyckel för frågan i stället för en administrationsnyckel. Detta är förenligt med hello [principen om minsta behörighet](https://en.wikipedia.org/wiki/Principle_of_least_privilege) och hjälper toomake programmet säkrare. Du hittar mer information om administratör och fråga nycklar i hello [Azure Search REST API-referensen](https://docs.microsoft.com/rest/api/searchservice/).
> 
> 

`SearchIndexClient` har en `Documents`-egenskap. Den här egenskapen anger alla hello-metoder som du behöver tooadd, ändra, ta bort eller fråga dokument i ditt index.

## <a name="decide-which-indexing-action-toouse"></a>Bestäm vilka indexering åtgärden toouse
tooimport data med hjälp av hello .NET SDK, behöver du toopackage upp data till ett `IndexBatch` objekt. En `IndexBatch` kapslar in en samling `IndexAction` objekt, som innehåller ett dokument och en egenskap som talar om Azure Search vilken åtgärd tooperform på dokumentet (överföringen, merge, ta bort och så vidare). Beroende på vilka hello under åtgärder som du väljer endast vissa fält måste inkluderas för varje dokument:

| Åtgärd | Beskrivning | Nödvändiga fält för varje dokument | Anteckningar |
| --- | --- | --- | --- |
| `Upload` |En `Upload` åtgärden är liknande tooan ”upsert” där hello dokumentet infogas om det är nytt och uppdatera/ersättas om den finns. |nyckel plus andra fält som du vill toodefine |När du uppdaterar och ersätta ett befintligt dokument, alla fält som anges i begäran hello har dess fält som har angetts för`null`. Detta inträffar även när hello fältet tidigare var inställt på tooa icke-null-värde. |
| `Merge` |Uppdateringar som har ett befintligt dokument med hello angivna fält. Om hello dokumentet inte finns i hello index, misslyckas hello sammanslagning. |nyckel plus andra fält som du vill toodefine |Alla fält som du anger i en merge ersätter hello befintliga fält i hello. Detta gäller även fält av typen `DataType.Collection(DataType.String)`. Till exempel om hello dokumentet innehåller ett fält `tags` med värdet `["budget"]` och du utföra en koppling med värdet `["economy", "pool"]` för `tags`, hello slutliga värdet för hello `tags` fältet kommer att vara `["economy", "pool"]`. Det blir inte `["budget", "economy", "pool"]`. |
| `MergeOrUpload` |Den här åtgärden fungerar som `Merge` om ett dokument med hello anges nyckeln redan finns i hello index. Om hello dokumentet inte finns, fungerar den som `Upload` med ett nytt dokument. |nyckel plus andra fält som du vill toodefine |- |
| `Delete` |Tar bort hello angivna dokumentet från hello index. |endast nyckel |Alla fält som du anger än hello nyckelfältet kommer att ignoreras. Om du vill tooremove ett fält från ett dokument, använda `Merge` enkelt och i stället ange hello fältet explicit toonull. |

Du kan ange vilken åtgärd du vill toouse med hello olika statiska metoder för hello `IndexBatch` och `IndexAction` klasser, som visas i nästa avsnitt om hello.

## <a name="construct-your-indexbatch"></a>Skapa IndexBatch
Nu när du vet vilka åtgärder tooperform på dina dokument du är klar tooconstruct hello `IndexBatch`. Hej exemplet nedan visar hur toocreate en batch med några olika åtgärder. Observera att vårt exempel använder en anpassad klass som heter `Hotel` som mappar tooa dokument i hello ”hotell” index.

```csharp
var actions =
    new IndexAction<Hotel>[]
    {
        IndexAction.Upload(
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
            }),
        IndexAction.Upload(
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
            }),
        IndexAction.MergeOrUpload(
            new Hotel()
            {
                HotelId = "3",
                BaseRate = 129.99,
                Description = "Close tootown hall and hello river"
            }),
        IndexAction.Delete(new Hotel() { HotelId = "6" })
    };

var batch = IndexBatch.New(actions);
```

I det här fallet använder du `Upload`, `MergeOrUpload`, och `Delete` som våra Sök-åtgärder, som anges av hello metoderna hello `IndexAction` klass.

Anta att exempelindexet ”hotels” redan fyllts med ett antal dokument. Observera hur vi hade inte toospecify alla fält för hello möjliga dokument när du använder `MergeOrUpload` och hur vi bara ange hello Dokumentnyckel (`HotelId`) när du använder `Delete`.

Observera också att du får bara innehålla too1000 dokument i en enskild indexering begäran.

> [!NOTE]
> I det här exemplet använder vi olika åtgärder toodifferent dokument. Om du vill använda tooperform hello samma åtgärder i alla dokument i hello batch istället för att ringa `IndexBatch.New`, kunde du använda hello andra statiska metoder för `IndexBatch`. Du kan till exempel skapa batchar genom att anropa `IndexBatch.Merge`, `IndexBatch.MergeOrUpload` eller `IndexBatch.Delete`. Dessa metoder använder en samling dokument (objekt av typen `Hotel` i det här exemplet) i stället för `IndexAction`-objekt.
> 
> 

## <a name="import-data-toohello-index"></a>Importera data toohello index
Nu när du har en initierad `IndexBatch` objekt, kan du skicka det toohello index genom att anropa `Documents.Index` på din `SearchIndexClient` objekt. följande exempel visar hur hello toocall `Index`, plus vissa extra steg behöver du tooperform:

```csharp
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
```

Obs hello `try` / `catch` omgivande hello anropet toohello `Index` metod. hello catch-blocket hanterar ett viktigt fel ärende för indexering. Om din Azure Search-tjänst misslyckas tooindex vissa hello dokument i hello batch en `IndexBatchException` genereras av `Documents.Index`. Detta kan inträffa om du indexerar dokument när tjänsten är hårt belastad. **Vi rekommenderar starkt att du uttryckligen hanterar den här situationen i din kod.** Du kan fördröja och försök indexering hello-dokument som misslyckades du kan logga in och fortsätta som hello exempel har eller kan du göra något annat beroende på programmets konsekvens kraven

Slutligen hello koden i hello-exemplet ovan fördröjningar i två sekunder. Indexering sker asynkront i Azure Search-tjänsten så hello exempelprogrammet måste toowait en kort tid tooensure att hello dokument är tillgängliga för sökning. Fördröjningar som den här är normalt endast nödvändiga i demonstrationer, tester och exempelprogram.

<a name="HotelClass"></a>

### <a name="how-hello-net-sdk-handles-documents"></a>Hanteringen av dokument i hello .NET SDK
Du kanske undrar hur hello Azure Search .NET SDK är kan tooupload instanser av en användardefinierad klass som `Hotel` toohello index. toohelp besvara frågan, ska vi titta på hello `Hotel` -klassen, som mappar toohello indexeringsschema som definierats i [skapa ett Azure Search-index med hello .NET SDK](search-create-index-dotnet.md#DefineIndex):

```csharp
[SerializePropertyNamesAsCamelCase]
public partial class Hotel
{
    [Key]
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

    // ToString() method omitted for brevity...
}
```

hello först öppna toonotice är att varje offentlig egenskap för `Hotel` motsvarar tooa fält i hello indexdefinitionen, men med en viktig skillnad: hello varje fält börjar med en gemen bokstav (”slitbanor fallet”) vid hello namnet på varje offentlig Egenskapen för `Hotel` börjar med en versal (”Pascal fall”). Det här är ett vanligt scenario i .NET-program som utför databindning där hello målschemat är utanför hello kontroll över hello programutvecklaren. Du kan se hello SDK toomap hello egenskapen namn toocamel fall automatiskt med hello snarare än att tooviolate hello .NET naming riktlinjer genom att egenskapen namn slitbanor skiftläge `[SerializePropertyNamesAsCamelCase]` attribut.

> [!NOTE]
> hello Azure Search .NET SDK använder hello [NewtonSoft JSON.NET](http://www.newtonsoft.com/json/help/html/Introduction.htm) biblioteket tooserialize och deserialisera din anpassade modellen objekt tooand från JSON. Du kan anpassa den här serialiseringen om det behövs. Mer information finns i [Anpassad serialisering med JSON.NET](search-howto-dotnet-sdk.md#JsonDotNet). Ett exempel på detta är hello använda hello `[JsonProperty]` -attributet på hello `DescriptionFr` egenskap i hello exempelkod ovan.
> 
> 

hello stycke om hello `Hotel` klass är hello datatyperna hello offentliga egenskaper. hello .NET-typer av de här egenskaperna mappa tootheir motsvarande fälttyp i hello indexdefinitionen. Till exempel hello `Category` strängegenskap mappar toohello `category` fält som är av typen `DataType.String`. Det finns liknande typmappningar mellan `bool?` och `DataType.Boolean`, `DateTimeOffset?` och `DataType.DateTimeOffset` osv. hello särskilda regler för mappning hello dokumenteras med hello `Documents.Get` metod i hello [Azure Search .NET SDK referens](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.idocumentsoperations#Microsoft_Azure_Search_IDocumentsOperations_GetWithHttpMessagesAsync__1_System_String_System_Collections_Generic_IEnumerable_System_String__Microsoft_Azure_Search_Models_SearchRequestOptions_System_Collections_Generic_Dictionary_System_String_System_Collections_Generic_List_System_String___System_Threading_CancellationToken_).

Den här möjligheten toouse egna klasser som dokument fungerar i båda riktningarna; Du kan också hämta sökresultat och hello SDK automatiskt deserialisera dem tooa typ du väljer, enligt hello [nästa artikel](search-query-dotnet.md).

> [!NOTE]
> hello Azure Search .NET SDK stöder också dynamiskt skrev dokument med hjälp av hello `Document` klassen, som är en nyckel/värde-mappning av toofield värdena för fältet namn. Detta är användbart i scenarier där du inte vet hello indexeringsschema i designläge, eller om det skulle vara olämplig toobind toospecific modellen klasser. Alla hello metoder i hello SDK som handlar om dokument har överlagringar som fungerar med hello `Document` klass, samt strikt typkontroll överlagringar som har en generisk typparameter. Endast hello senare används i hello exempelkoden i den här artikeln.
> 
> 

**Varför du bör använda datatyper som kan ha värdet null**

När du skapar egna modellen klasser toomap tooan Azure Search index, rekommenderar vi deklarerar egenskaper för värdetyper som `bool` och `int` toobe kan ha värdet null (till exempel `bool?` i stället för `bool`). Om du använder en icke-nullbar egenskapen, det finns också**garantera** att inga dokument i indexet innehåller ett null-värde för hello motsvarande fält. Varken hello SDK eller hello Azure Search-tjänsten kan du tooenforce detta.

Detta är inte bara ett hypotetiskt problem: Tänk dig ett scenario där du lägger till ett nytt fält tooan befintliga index som är av typen `DataType.Int32`. När du har uppdaterat hello indexdefinitionen har alla dokument som ett null-värde för det nya fältet (eftersom alla typer inte kan vara null i Azure Search). Om du använder en modellklass sedan med en icke-nullbar `int` -egenskapen för det här fältet får du en `JsonSerializationException` så här när du försöker tooretrieve dokument:

    Error converting value {null} tootype 'System.Int32'. Path 'IntValue'.

Av den anledningen rekommenderar vi att du använder nullbara typer i dina modellklasser som bästa praxis.

## <a name="next-steps"></a>Nästa steg
Du kommer att redo toostart utfärda frågor toosearch för dokument efter fylla ditt Azure Search-index. Mer information finns i [Fråga ditt Azure Search-index](search-query-overview.md).

