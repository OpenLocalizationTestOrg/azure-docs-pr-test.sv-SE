---
title: aaaUpgrading toohello Azure Search .NET SDK version 1.1 | Microsoft Docs
description: Uppgradera toohello Azure Search .NET SDK version 1.1
services: search
documentationcenter: 
author: brjohnstmsft
manager: pablocas
editor: 
ms.assetid: 66f89958-a320-4a24-87f9-69315848909f
ms.service: search
ms.devlang: dotnet
ms.workload: search
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 01/11/2017
ms.author: brjohnst
ms.openlocfilehash: 291ae5731546e47b3c22c721d3552a79bdea80c1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="upgrading-toohello-azure-search-net-sdk-version-3"></a>Uppgradera toohello Azure Search .NET SDK version 3
Om du använder version 2.0 Förhandsgranska eller äldre av hello [Azure Search .NET SDK](https://aka.ms/search-sdk), den här artikeln hjälper dig att uppgradera din programmet toouse version 3.

En mer allmän genomgång av hello SDK och exempel finns i [hur toouse Azure söka från ett .NET-program](search-howto-dotnet-sdk.md).

Version 3 av hello Azure Search .NET SDK innehåller vissa ändringar från tidigare versioner. Detta är oftast mindre, så ändrar koden kräver endast minsta möjliga ansträngning. Se [steg tooupgrade](#UpgradeSteps) anvisningar för hur toochange din kod toouse hello nya SDK-version.

> [!NOTE]
> Om du använder version 1.0.2-preview eller senare, du bör först uppgradera tooversion 1.1 och sedan uppgradera tooversion 3. Se [bilaga: steg tooupgrade tooversion 1.1](#UpgradeStepsV1) anvisningar.
>
> Din Azure Search-tjänstinstansen stöder flera REST API-versioner, inklusive hello senaste. Du kan fortsätta toouse en version när den inte längre hello senaste, men vi rekommenderar att du migrerar din kod toouse hello senaste versionen. När du använder hello REST-API, måste du ange hello API-versionen i varje begäran via parametern för hello api-version. När du använder hello .NET SDK avgör hello version av hello SDK som du använder hello motsvarande version av hello REST API. Om du använder en äldre SDK kan du fortsätta toorun koden utan ändringar även om hello tjänsten uppgraderade toosupport nyare API-version.

<a name="WhatsNew"></a>

## <a name="whats-new-in-version-3"></a>Vad är nytt i version 3
Version 3 av hello Azure Search .NET SDK mål hello senaste allmänt tillgänglig version av hello Azure Search REST API specifikt 2016-09-01. Detta gör det möjligt toouse många nya funktioner i Azure Search från ett .NET-program, inklusive hello följande:

* [Anpassade analysverktyg](https://aka.ms/customanalyzers)
* [Azure Blob Storage](search-howto-indexing-azure-blob-storage.md) och [Azure Table Storage](search-howto-indexing-azure-tables.md) stöd för indexerare
* Indexerare anpassning via [fältet mappningar](search-indexer-field-mappings.md)
* ETags stöder tooenable säker samtidig uppdatering av indexet definitioner, indexerare och datakällor
* Stöd för att skapa index fältdefinitioner deklarativt för pynta modellklass och använda hello nya `FieldBuilder` klass.
* Stöd för .NET Core och portabla .NET-profil 111

<a name="UpgradeSteps"></a>

## <a name="steps-tooupgrade"></a>Steg tooupgrade
Uppdatera först NuGet-referens för `Microsoft.Azure.Search` antingen hello NuGet Package Manager-konsolen eller genom att högerklicka på projektreferenserna och välja ”hantera NuGet-paket...” i Visual Studio.

När NuGet har hämtats hello nya paket och deras beroenden, återskapa projektet. Beroende på hur koden är strukturerad, kan den återskapa har. I så fall, är du redo toogo!

Om din build misslyckas, bör du se ett build-fel som hello nedan:

    Program.cs(31,45,31,86): error CS0266: Cannot implicitly convert type 'Microsoft.Azure.Search.ISearchIndexClient' too'Microsoft.Azure.Search.SearchIndexClient'. An explicit conversion exists (are you missing a cast?)

hello nästa steg är toofix build-fel. Se [bryta ändringar i version 3](#ListOfChanges) mer information om vad som orsakar hello fel och hur toofix den.

Du kan se ytterligare build varningar relaterade tooobsolete metoder eller egenskaper. hello varningar innehåller instruktioner om vilken toouse i stället för hello föråldrad funktion. Till exempel om programmet använder hello `IndexingParameters.Base64EncodeKeys` egenskapen som du bör få en varning om att`"This property is obsolete. Please create a field mapping using 'FieldMapping.Base64Encode' instead."`

När du har åtgärdat eventuella build-fel, kan du ändra tooyour programmet tootake utnyttja nya funktioner om du vill. Nya funktioner i hello SDK beskrivs i [vad är nytt i version 3](#WhatsNew).

<a name="ListOfChanges"></a>

## <a name="breaking-changes-in-version-3"></a>Gör ändringar i version 3
Det ändrar ett litet antal viktiga förändringar i version 3 för som kan kräva kod dessutom toorebuilding ditt program.

### <a name="indexesgetclient-return-type"></a>Indexes.GetClient returtyp
Hej `Indexes.GetClient` metoden har en ny returtyp. Tidigare returnerade `SearchIndexClient`, men detta har ändrats för`ISearchIndexClient` i version 2.0-preview och som följer ändra med tooversion 3. Detta är toosupport kunder som vill toomock hello `GetClient` metod för kontroller genom att returnera en fingerad implementering av `ISearchIndexClient`.

#### <a name="example"></a>Exempel
Om din kod ser ut så här:

```csharp
SearchIndexClient indexClient = serviceClient.Indexes.GetClient("hotels");
```

Du kan ändra den toothis toofix någon skapa fel:

```csharp
ISearchIndexClient indexClient = serviceClient.Indexes.GetClient("hotels");
```

### <a name="analyzername-datatype-and-others-are-no-longer-implicitly-convertible-toostrings"></a>AnalyzerName och datatyp är inte längre implicit omvandlas toostrings
Det finns många typer i hello Azure Search .NET SDK som härleds från `ExtensibleEnum`. Tidigare var dessa typer av alla implicit omvandlas tootype `string`. Men ett fel upptäcktes i hello `Object.Equals` implementering för dessa klasser och åtgärdar hello bugg krävs om du inaktiverar den här implicit konvertering. Explicit konvertering för`string` fortfarande är tillåtet.

#### <a name="example"></a>Exempel
Om din kod ser ut så här:

```csharp
var customTokenizerName = TokenizerName.Create("my_tokenizer"); 
var customTokenFilterName = TokenFilterName.Create("my_tokenfilter"); 
var customCharFilterName = CharFilterName.Create("my_charfilter"); 
 
var index = new Index();
index.Analyzers = new Analyzer[] 
{ 
    new CustomAnalyzer( 
        "my_analyzer",  
        customTokenizerName,  
        new[] { customTokenFilterName },  
        new[] { customCharFilterName }), 
}; 
```

Du kan ändra den toothis toofix någon skapa fel:

```csharp
const string CustomTokenizerName = "my_tokenizer"; 
const string CustomTokenFilterName = "my_tokenfilter"; 
const string CustomCharFilterName = "my_charfilter"; 
 
var index = new Index();
index.Analyzers = new Analyzer[] 
{ 
    new CustomAnalyzer( 
        "my_analyzer",  
        CustomTokenizerName,  
        new TokenFilterName[] { CustomTokenFilterName },  
        new CharFilterName[] { CustomCharFilterName })
}; 
```

### <a name="removed-obsolete-members"></a>Ta bort föråldrade medlemmar

Du kan se build fel relaterade toomethods och egenskaper som har markerats som föråldrade i version 2.0-preview och därefter tas bort i version 3. Om du stöter på dessa fel, hur tooresolve dem:

- Om du använder den här konstruktorn: `ScoringParameter(string name, string value)`, Använd den här i stället:`ScoringParameter(string name, IEnumerable<string> values)`
- Om du använde hello `ScoringParameter.Value` egenskapen, Använd hello `ScoringParameter.Values` egenskap eller hello `ToString` metod i stället.
- Om du använde hello `SearchRequestOptions.RequestId` egenskapen, Använd hello `ClientRequestId` egenskapen i stället.

### <a name="removed-preview-features"></a>Borttagna förhandsgranskningsfunktioner

Om du uppgraderar från version 2.0-preview tooversion 3, Tänk på att JSON och CSV parsning stöd för Blob indexerare har tagits bort eftersom dessa funktioner är fortfarande under förhandsgranskning. Mer specifikt hello följande metoder för hello `IndexingParametersExtensions` klass har tagits bort:

- `ParseJson`
- `ParseJsonArrays`
- `ParseDelimitedTextFiles`

Om programmet har en fast beroende på de här funktionerna, kommer inte att kunna tooupgrade tooversion 3 av hello Azure Search .NET SDK. Du kan fortsätta toouse version 2.0-preview. Men du tänka på att **vi rekommenderar inte att använda Förhandsgranska SDK: er i produktionsprogram**. Förhandsgranskningsfunktioner gäller enbart och kan ändras.

## <a name="conclusion"></a>Slutsats
Om du vill ha mer information om hur du använder hello Azure Search .NET SDK finns i vår nyligen uppdaterat [anvisningar](search-howto-dotnet-sdk.md).

Vi uppskattar din feedback på hello SDK. Om du får problem känna sig fria tooask oss hjälp om hello [Azure Search MSDN-forum](https://social.msdn.microsoft.com/Forums/azure/home?forum=azuresearch). Om du hittar ett programfel kan du filen ett problem i hello [Azure .NET SDK GitHub-lagringsplatsen](https://github.com/Azure/azure-sdk-for-net/issues). Se till att tooprefix problemet rubriken med ”Sök SDK”:.

Tack för att använda Azure Search!

<a name="UpgradeStepsV1"></a>

## <a name="appendix-steps-tooupgrade-tooversion-11"></a>Bilaga: Steg tooupgrade tooversion 1.1
> [!NOTE]
> Det här avsnittet gäller endast toousers av hello Azure Search .NET SDK version 1.0.2-preview och äldre.
> 
> 

Uppdatera först NuGet-referens för `Microsoft.Azure.Search` antingen hello NuGet Package Manager-konsolen eller genom att högerklicka på projektreferenserna och välja ”hantera NuGet-paket...” i Visual Studio.

När NuGet har hämtats hello nya paket och deras beroenden, återskapa projektet.

Om du tidigare har använt version 1.0.0-preview, 1.0.1-preview eller 1.0.2-preview, hello build ska lyckas och du är redo toogo!

Om du använde tidigare version 0.13.0-preview eller äldre, bör du se Skapa fel som hello nedan:

    Program.cs(137,56,137,62): error CS0117: 'Microsoft.Azure.Search.Models.IndexBatch' does not contain a definition for 'Create'
    Program.cs(137,99,137,105): error CS0117: 'Microsoft.Azure.Search.Models.IndexAction' does not contain a definition for 'Create'
    Program.cs(146,41,146,54): error CS1061: 'Microsoft.Azure.Search.IndexBatchException' does not contain a definition for 'IndexResponse' and no extension method 'IndexResponse' accepting a first argument of type 'Microsoft.Azure.Search.IndexBatchException' could be found (are you missing a using directive or an assembly reference?)
    Program.cs(163,13,163,42): error CS0246: hello type or namespace name 'DocumentSearchResponse' could not be found (are you missing a using directive or an assembly reference?)

hello nästa steg är toofix hello build-fel i taget. De flesta kräver att ändra vissa klass- och metoden namn som har bytt namn i hello SDK. [Lista över bryta ändringar i version 1.1](#ListOfChangesV1) innehåller en lista över ändringarna namn.

Om du använder egna klasser toomodel dokument och dessa klasser har egenskaperna för icke-nullbar primitiva typer (till exempel `int` eller `bool` i C#), är en felkorrigering hello 1.1-versionen av hello SDK som bör du vara medveten om. Se [felkorrigeringar i version 1.1](#BugFixesV1) mer information.

Slutligen, när du har åtgärdat eventuella build-fel kan du ändra tooyour programmet tootake utnyttja nya funktioner om du vill.

<a name="ListOfChangesV1"></a>

### <a name="list-of-breaking-changes-in-version-11"></a>Lista över bryta ändringar i version 1.1
hello är följande lista sorterade efter hello sannolikheten att hello påverkas programkoden.

#### <a name="indexbatch-and-indexaction-changes"></a>IndexBatch och IndexAction ändringar
`IndexBatch.Create`har bytt namn för`IndexBatch.New` och har inte längre en `params` argumentet. Du kan använda `IndexBatch.New` för batchar som blanda olika typer av åtgärder (sammanslagningar, borttagningar osv.). Dessutom finns nya statiska metoder för att skapa batchar där alla hello åtgärder är hello samma: `Delete`, `Merge`, `MergeOrUpload`, och `Upload`.

`IndexAction`Offentliga konstruktorer har inte längre och dess egenskaper är nu inte ändras. Du bör använda hello nya statiska metoder för att skapa åtgärder för olika ändamål: `Delete`, `Merge`, `MergeOrUpload`, och `Upload`. `IndexAction.Create`har tagits bort. Om du använde hello-överlagring som tar endast ett dokument, kontrollera att toouse `Upload` i stället.

##### <a name="example"></a>Exempel
Om din kod ser ut så här:

    var batch = IndexBatch.Create(documents.Select(doc => IndexAction.Create(doc)));
    indexClient.Documents.Index(batch);

Du kan ändra den toothis toofix någon skapa fel:

    var batch = IndexBatch.New(documents.Select(doc => IndexAction.Upload(doc)));
    indexClient.Documents.Index(batch);

Om du vill kan du ytterligare förenkla den toothis:

    var batch = IndexBatch.Upload(documents);
    indexClient.Documents.Index(batch);

#### <a name="indexbatchexception-changes"></a>IndexBatchException ändringar
Hej `IndexBatchException.IndexResponse` egenskap har ändrats för`IndexingResults`, och dess typ är nu `IList<IndexingResult>`.

##### <a name="example"></a>Exempel
Om din kod ser ut så här:

    catch (IndexBatchException e)
    {
        Console.WriteLine(
            "Failed tooindex some of hello documents: {0}",
            String.Join(", ", e.IndexResponse.Results.Where(r => !r.Succeeded).Select(r => r.Key)));
    }

Du kan ändra den toothis toofix någon skapa fel:

    catch (IndexBatchException e)
    {
        Console.WriteLine(
            "Failed tooindex some of hello documents: {0}",
            String.Join(", ", e.IndexingResults.Where(r => !r.Succeeded).Select(r => r.Key)));
    }

<a name="OperationMethodChanges"></a>

#### <a name="operation-method-changes"></a>Åtgärden metoden ändringar
Varje åtgärd i hello Azure Search .NET SDK visas som en uppsättning metoden överlagringar för synkrona och asynkrona anropare. hello har signaturer och factoring av dessa metoden överlagringar ändrats i version 1.1.

Till exempel visas hello ”hämta indexstatistik”-åtgärd i äldre versioner av hello SDK dessa signaturer:

I `IIndexOperations`:

    // Asynchronous operation with all parameters
    Task<IndexGetStatisticsResponse> GetStatisticsAsync(
        string indexName,
        CancellationToken cancellationToken);

I `IndexOperationsExtensions`:

    // Asynchronous operation with only required parameters
    public static Task<IndexGetStatisticsResponse> GetStatisticsAsync(
        this IIndexOperations operations,
        string indexName);

    // Synchronous operation with only required parameters
    public static IndexGetStatisticsResponse GetStatistics(
        this IIndexOperations operations,
        string indexName);

hello metoden signaturer för hello samma åtgärd i version 1.1 ser ut så här:

I `IIndexesOperations`:

    // Asynchronous operation with lower-level HTTP features exposed
    Task<AzureOperationResponse<IndexGetStatisticsResult>> GetStatisticsWithHttpMessagesAsync(
        string indexName,
        SearchRequestOptions searchRequestOptions = default(SearchRequestOptions),
        Dictionary<string, List<string>> customHeaders = null,
        CancellationToken cancellationToken = default(CancellationToken));

I `IndexesOperationsExtensions`:

    // Simplified asynchronous operation
    public static Task<IndexGetStatisticsResult> GetStatisticsAsync(
        this IIndexesOperations operations,
        string indexName,
        SearchRequestOptions searchRequestOptions = default(SearchRequestOptions),
        CancellationToken cancellationToken = default(CancellationToken));

    // Simplified synchronous operation
    public static IndexGetStatisticsResult GetStatistics(
        this IIndexesOperations operations,
        string indexName,
        SearchRequestOptions searchRequestOptions = default(SearchRequestOptions));

Från och med version 1.1, ordnar hello Azure Search .NET SDK åtgärden metoder på olika sätt:

* Valfria parametrar finns nu modelleras som standard parametrar snarare än ytterligare en metod överlagringar. Detta minskar hello antalet metoden överlagringar ibland dramatiskt.
* Hej tilläggsmetoder dölja nu mycket hello överflödig information om HTTP från hello anroparen. Till exempel äldre versioner av hello SDK returnerade ett svarsobjekt med en HTTP-statuskod som du ofta behövde toocheck eftersom åtgärden metoder utlösa `CloudException` för varje statuskod som indikerar ett fel. Hej nya tillägg metoder bara returnera modellobjekt, hello slipper toounwrap sparar du dem i din kod.
* Däremot exponerar hello core gränssnitt nu metoder som ger dig större kontroll på hello HTTP nivå om det behövs. Du kan nu skicka in anpassade HTTP-huvuden toobe i begäranden och hello nya `AzureOperationResponse<T>` returnera typen ger direktåtkomst toohello `HttpRequestMessage` och `HttpResponseMessage` för hello åtgärden. `AzureOperationResponse`har definierats i hello `Microsoft.Rest.Azure` namnområde och ersätter `Hyak.Common.OperationResponse`.

#### <a name="scoringparameters-changes"></a>ScoringParameters ändringar
En ny klass med namnet `ScoringParameter` har lagts till i hello senaste SDK toomake den enklare tooprovide parametrar tooscoring profiler i en sökfråga. Tidigare hello `ScoringProfiles` -egenskapen för hello `SearchParameters` klassen angavs som `IList<string>`; Nu det skrivs som `IList<ScoringParameter>`.

##### <a name="example"></a>Exempel
Om din kod ser ut så här:

    var sp = new SearchParameters();
    sp.ScoringProfile = "jobsScoringFeatured";      // Use a scoring profile
    sp.ScoringParameters = new[] { "featuredParam-featured", "mapCenterParam-" + lon + "," + lat };

Du kan ändra den toothis toofix någon skapa fel: 

    var sp = new SearchParameters();
    sp.ScoringProfile = "jobsScoringFeatured";      // Use a scoring profile
    sp.ScoringParameters =
        new[]
        {
            new ScoringParameter("featuredParam", new[] { "featured" }),
            new ScoringParameter("mapCenterParam", GeographyPoint.Create(lat, lon))
        };

#### <a name="model-class-changes"></a>Klassen Modelländringar
På grund av toohello signatur ändringar som beskrivs i [igen metoden ändringar](#OperationMethodChanges), många klasser i hello `Microsoft.Azure.Search.Models` namnområde har ändrats eller tagits bort. Exempel:

* `IndexDefinitionResponse`har ersatts av`AzureOperationResponse<Index>`
* `DocumentSearchResponse`har bytt namn för`DocumentSearchResult`
* `IndexResult`har bytt namn för`IndexingResult`
* `Documents.Count()`Returnerar nu en `long` med hello dokumentantal i stället för en`DocumentCountResponse`
* `IndexGetStatisticsResponse`har bytt namn för`IndexGetStatisticsResult`
* `IndexListResponse`har bytt namn för`IndexListResult`

toosummarize, `OperationResponse`-härledda klasser som fanns endast toowrap model-objektet har tagits bort. hello återstående klasser har fått sina suffix har ändrats från `Response` för`Result`.

##### <a name="example"></a>Exempel
Om din kod ser ut så här:

    IndexerGetStatusResponse statusResponse = null;

    try
    {
        statusResponse = _searchClient.Indexers.GetStatus(indexer.Name);
    }
    catch (Exception ex)
    {
        Console.WriteLine("Error polling for indexer status: {0}", ex.Message);
        return;
    }

    IndexerExecutionResult lastResult = statusResponse.ExecutionInfo.LastResult;

Du kan ändra den toothis toofix någon skapa fel:

    IndexerExecutionInfo status = null;

    try
    {
        status = _searchClient.Indexers.GetStatus(indexer.Name);
    }
    catch (Exception ex)
    {
        Console.WriteLine("Error polling for indexer status: {0}", ex.Message);
        return;
    }

    IndexerExecutionResult lastResult = status.LastResult;

##### <a name="response-classes-and-ienumerable"></a>Svaret klasser och IEnumerable
En ytterligare ändring som kan påverka din kod är att implementera svar klasser som innehåller samlingar längre `IEnumerable<T>`. Du kan i stället öppna hello samlingsegenskap direkt. Om exempelvis koden ser ut så här:

    DocumentSearchResponse<Hotel> response = indexClient.Documents.Search<Hotel>(searchText, sp);
    foreach (SearchResult<Hotel> result in response)
    {
        Console.WriteLine(result.Document);
    }

Du kan ändra den toothis toofix någon skapa fel:

    DocumentSearchResult<Hotel> response = indexClient.Documents.Search<Hotel>(searchText, sp);
    foreach (SearchResult<Hotel> result in response.Results)
    {
        Console.WriteLine(result.Document);
    }

##### <a name="special-case-for-web-applications"></a>Specialfall för webbprogram
Om du har ett webbprogram som Serialiserar `DocumentSearchResponse` direkt toosend sökresultat toohello webbläsare, behöver du toochange koden eller hello resultat kommer inte att serialisera korrekt. Om exempelvis koden ser ut så här:

    public ActionResult Search(string q = "")
    {
        // If blank search, assume they want toosearch everything
        if (string.IsNullOrWhiteSpace(q))
            q = "*";

        return new JsonResult
        {
            JsonRequestBehavior = JsonRequestBehavior.AllowGet,
            Data = _featuresSearch.Search(q)
        };
    }

Du kan ändra den genom att hämta hello `.Results` -egenskapen för hello Sök svar toofix Sök resultatet återgivning:

    public ActionResult Search(string q = "")
    {
        // If blank search, assume they want toosearch everything
        if (string.IsNullOrWhiteSpace(q))
            q = "*";

        return new JsonResult
        {
            JsonRequestBehavior = JsonRequestBehavior.AllowGet,
            Data = _featuresSearch.Search(q).Results
        };
    }

Du har toolook sådana fall i koden själv. **hello kompileraren varnar dig inte** eftersom `JsonResult.Data` är av typen `object`.

#### <a name="cloudexception-changes"></a>CloudException ändringar
Hej `CloudException` klass har flyttats från hello `Hyak.Common` namnområde toohello `Microsoft.Rest.Azure` namnområde. Dessutom dess `Error` egenskap har ändrats för`Body`.

#### <a name="searchserviceclient-and-searchindexclient-changes"></a>SearchServiceClient och SearchIndexClient ändringar
Hej typ av hello `Credentials` egenskap har ändrats från `SearchCredentials` tooits basklassen `ServiceClientCredentials`. Om du behöver tooaccess hello `SearchCredentials` av en `SearchIndexClient` eller `SearchServiceClient`, Använd hello nya `SearchCredentials` egenskapen.

I tidigare versioner av hello SDK, `SearchServiceClient` och `SearchIndexClient` hade konstruktorer som tog en `HttpClient` parameter. Dessa har ersatts av konstruktorer som tar en `HttpClientHandler` och en matris med `DelegatingHandler` objekt. Detta gör det enklare tooinstall anpassad hanterare toopre processen HTTP-begäranden om det behövs.

Slutligen hello konstruktorer som tog en `Uri` och `SearchCredentials` har ändrats. Till exempel om du har kod som ser ut så här:

    var client =
        new SearchServiceClient(
            new SearchCredentials("abc123"),
            new Uri("http://myservice.search.windows.net"));

Du kan ändra den toothis toofix någon skapa fel:

    var client =
        new SearchServiceClient(
            new Uri("http://myservice.search.windows.net"),
            new SearchCredentials("abc123"));

Observera att hello typ av hello autentiseringsuppgifter parametern har också ändrats för`ServiceClientCredentials`. Detta är inte troligt tooaffect koden sedan `SearchCredentials` är härledd från `ServiceClientCredentials`.

#### <a name="passing-a-request-id"></a>Skicka en begäran-ID
I äldre versioner av hello SDK kan du ange en begärande-ID på hello `SearchServiceClient` eller `SearchIndexClient` och det skulle ingå i varje begäran toohello REST API. Detta är användbart för felsökning av problem med din söktjänst om du behöver stöd för toocontact. Det är dock mer användbar tooset ett unikt begäran-ID för varje åtgärd i stället för att toouse hello samma ID för alla åtgärder. Därför hello `SetClientRequestId` metoder för `SearchServiceClient` och `SearchIndexClient` har tagits bort. I stället kan du kan skicka en metod för begäran-ID tooeach åtgärden via hello valfria `SearchRequestOptions` parameter.

> [!NOTE]
> I en framtida utgåva av hello SDK vi lägga till en ny mekanism för att ange ID för förfrågan globalt på klientens hello objekt som är kompatibel med hello-metod som används av andra Azure-SDK.
> 
> 

#### <a name="example"></a>Exempel
Om du har kod som ser ut så här:

    client.SetClientRequestId(Guid.NewGuid());
    ...
    long count = client.Documents.Count();

Du kan ändra den toothis toofix någon skapa fel:

    long count = client.Documents.Count(new SearchRequestOptions(requestId: Guid.NewGuid()));

#### <a name="interface-name-changes"></a>Gränssnittet namnändringar
hello åtgärden gruppnamn gränssnitt har alla ändrade toobe som överensstämmer med deras motsvarande egenskapsnamn:

* Hej typ av `ISearchServiceClient.Indexes` har ändrats från `IIndexOperations` för`IIndexesOperations`.
* Hej typ av `ISearchServiceClient.Indexers` har ändrats från `IIndexerOperations` för`IIndexersOperations`.
* Hej typ av `ISearchServiceClient.DataSources` har ändrats från `IDataSourceOperations` för`IDataSourcesOperations`.
* Hej typ av `ISearchIndexClient.Documents` har ändrats från `IDocumentOperations` för`IDocumentsOperations`.

Den här ändringen är osannolikt tooaffect din kod om du har skapat mocks av dessa gränssnitt för testning.

<a name="BugFixesV1"></a>

### <a name="bug-fixes-in-version-11"></a>Felkorrigeringar i version 1.1
Det uppstod ett fel i äldre versioner av hello Azure Search .NET SDK om tooserialization av anpassade modellen klasser. hello fel kan inträffa om du har skapat en anpassad modellklass med en egenskap för en icke-nullbar värdetyp.

#### <a name="steps-tooreproduce"></a>Steg tooreproduce
Skapa en anpassad modellklass med en egenskap av typen icke-kan ha värdet null. Till exempel lägga till en offentlig `UnitCount` egenskap av typen `int` i stället för `int?`.

Om du indexera ett dokument med hello standardvärdet för den aktuella typen (till exempel 0 för `int`), hello fältet är null i Azure Search. Om du senare söker efter dokumentet hello `Search` anropet kommer att kasta `JsonSerializationException` klagande som det går inte att konvertera `null` för`int`.

Filter får dessutom inte fungerar som förväntat eftersom null skrevs toohello index i stället för hello avsett värde.

#### <a name="fix-details"></a>Åtgärda information
Vi har löst problemet i version 1.1 av hello SDK. Nu om du har en modellklass så här:

    public class Model
    {
        public string Key { get; set; }

        public int IntValue { get; set; }
    }

och du ställer in `IntValue` too0 att värdet är nu korrekt serialiserad som 0 hello uppkopplat läge och lagras som 0 i hello index. Avrunda utlösning också fungerar som förväntat.

En potentiell problemet toobe är medveten om med den här metoden: Om du använder en modell av typen har ett icke-nullbar egenskapen, det finns också**garantera** att inga dokument i indexet innehåller ett null-värde för hello motsvarande fält. Varken hello SDK eller hello Azure Search REST API hjälper du tooenforce detta.

Detta är inte bara ett hypotetiskt problem: Tänk dig ett scenario där du lägger till ett nytt fält tooan befintliga index som är av typen `Edm.Int32`. När du har uppdaterat hello indexdefinitionen har alla dokument som ett null-värde för det nya fältet (eftersom alla typer inte kan vara null i Azure Search). Om du använder en modellklass sedan med en icke-nullbar `int` -egenskapen för det här fältet får du en `JsonSerializationException` så här när du försöker tooretrieve dokument:

    Error converting value {null} tootype 'System.Int32'. Path 'IntValue'.

Därför kan rekommenderar vi ändå att du använder kan ha värdet null typer i din modell-klasser som bästa praxis.

Mer information om korrigeringsfilen programfel och hello finns [problemet på GitHub](https://github.com/Azure/azure-sdk-for-net/issues/1063).

