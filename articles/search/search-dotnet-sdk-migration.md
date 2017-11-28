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
# <a name="upgrading-toohello-azure-search-net-sdk-version-3"></a><span data-ttu-id="f614c-103">Uppgradera toohello Azure Search .NET SDK version 3</span><span class="sxs-lookup"><span data-stu-id="f614c-103">Upgrading toohello Azure Search .NET SDK version 3</span></span>
<span data-ttu-id="f614c-104">Om du använder version 2.0 Förhandsgranska eller äldre av hello [Azure Search .NET SDK](https://aka.ms/search-sdk), den här artikeln hjälper dig att uppgradera din programmet toouse version 3.</span><span class="sxs-lookup"><span data-stu-id="f614c-104">If you're using version 2.0-preview or older of hello [Azure Search .NET SDK](https://aka.ms/search-sdk), this article will help you upgrade your application toouse version 3.</span></span>

<span data-ttu-id="f614c-105">En mer allmän genomgång av hello SDK och exempel finns i [hur toouse Azure söka från ett .NET-program](search-howto-dotnet-sdk.md).</span><span class="sxs-lookup"><span data-stu-id="f614c-105">For a more general walkthrough of hello SDK including examples, see [How toouse Azure Search from a .NET Application](search-howto-dotnet-sdk.md).</span></span>

<span data-ttu-id="f614c-106">Version 3 av hello Azure Search .NET SDK innehåller vissa ändringar från tidigare versioner.</span><span class="sxs-lookup"><span data-stu-id="f614c-106">Version 3 of hello Azure Search .NET SDK contains some changes from earlier versions.</span></span> <span data-ttu-id="f614c-107">Detta är oftast mindre, så ändrar koden kräver endast minsta möjliga ansträngning.</span><span class="sxs-lookup"><span data-stu-id="f614c-107">These are mostly minor, so changing your code should require only minimal effort.</span></span> <span data-ttu-id="f614c-108">Se [steg tooupgrade](#UpgradeSteps) anvisningar för hur toochange din kod toouse hello nya SDK-version.</span><span class="sxs-lookup"><span data-stu-id="f614c-108">See [Steps tooupgrade](#UpgradeSteps) for instructions on how toochange your code toouse hello new SDK version.</span></span>

> [!NOTE]
> <span data-ttu-id="f614c-109">Om du använder version 1.0.2-preview eller senare, du bör först uppgradera tooversion 1.1 och sedan uppgradera tooversion 3.</span><span class="sxs-lookup"><span data-stu-id="f614c-109">If you're using version 1.0.2-preview or older, you should upgrade tooversion 1.1 first, and then upgrade tooversion 3.</span></span> <span data-ttu-id="f614c-110">Se [bilaga: steg tooupgrade tooversion 1.1](#UpgradeStepsV1) anvisningar.</span><span class="sxs-lookup"><span data-stu-id="f614c-110">See [Appendix: Steps tooupgrade tooversion 1.1](#UpgradeStepsV1) for instructions.</span></span>
>
> <span data-ttu-id="f614c-111">Din Azure Search-tjänstinstansen stöder flera REST API-versioner, inklusive hello senaste.</span><span class="sxs-lookup"><span data-stu-id="f614c-111">Your Azure Search service instance supports several REST API versions, including hello latest one.</span></span> <span data-ttu-id="f614c-112">Du kan fortsätta toouse en version när den inte längre hello senaste, men vi rekommenderar att du migrerar din kod toouse hello senaste versionen.</span><span class="sxs-lookup"><span data-stu-id="f614c-112">You can continue toouse a version when it is no longer hello latest one, but we recommend that you migrate your code toouse hello newest version.</span></span> <span data-ttu-id="f614c-113">När du använder hello REST-API, måste du ange hello API-versionen i varje begäran via parametern för hello api-version.</span><span class="sxs-lookup"><span data-stu-id="f614c-113">When using hello REST API, you must specify hello API version in every request via hello api-version parameter.</span></span> <span data-ttu-id="f614c-114">När du använder hello .NET SDK avgör hello version av hello SDK som du använder hello motsvarande version av hello REST API.</span><span class="sxs-lookup"><span data-stu-id="f614c-114">When using hello .NET SDK, hello version of hello SDK you’re using determines hello corresponding version of hello REST API.</span></span> <span data-ttu-id="f614c-115">Om du använder en äldre SDK kan du fortsätta toorun koden utan ändringar även om hello tjänsten uppgraderade toosupport nyare API-version.</span><span class="sxs-lookup"><span data-stu-id="f614c-115">If you are using an older SDK, you can continue toorun that code with no changes even if hello service is upgraded toosupport a newer API version.</span></span>

<a name="WhatsNew"></a>

## <a name="whats-new-in-version-3"></a><span data-ttu-id="f614c-116">Vad är nytt i version 3</span><span class="sxs-lookup"><span data-stu-id="f614c-116">What's new in version 3</span></span>
<span data-ttu-id="f614c-117">Version 3 av hello Azure Search .NET SDK mål hello senaste allmänt tillgänglig version av hello Azure Search REST API specifikt 2016-09-01.</span><span class="sxs-lookup"><span data-stu-id="f614c-117">Version 3 of hello Azure Search .NET SDK targets hello latest generally available version of hello Azure Search REST API, specifically 2016-09-01.</span></span> <span data-ttu-id="f614c-118">Detta gör det möjligt toouse många nya funktioner i Azure Search från ett .NET-program, inklusive hello följande:</span><span class="sxs-lookup"><span data-stu-id="f614c-118">This makes it possible toouse many new features of Azure Search from a .NET application, including hello following:</span></span>

* [<span data-ttu-id="f614c-119">Anpassade analysverktyg</span><span class="sxs-lookup"><span data-stu-id="f614c-119">Custom analyzers</span></span>](https://aka.ms/customanalyzers)
* <span data-ttu-id="f614c-120">[Azure Blob Storage](search-howto-indexing-azure-blob-storage.md) och [Azure Table Storage](search-howto-indexing-azure-tables.md) stöd för indexerare</span><span class="sxs-lookup"><span data-stu-id="f614c-120">[Azure Blob Storage](search-howto-indexing-azure-blob-storage.md) and [Azure Table Storage](search-howto-indexing-azure-tables.md) indexer support</span></span>
* <span data-ttu-id="f614c-121">Indexerare anpassning via [fältet mappningar](search-indexer-field-mappings.md)</span><span class="sxs-lookup"><span data-stu-id="f614c-121">Indexer customization via [field mappings](search-indexer-field-mappings.md)</span></span>
* <span data-ttu-id="f614c-122">ETags stöder tooenable säker samtidig uppdatering av indexet definitioner, indexerare och datakällor</span><span class="sxs-lookup"><span data-stu-id="f614c-122">ETags support tooenable safe concurrent updating of index definitions, indexers, and data sources</span></span>
* <span data-ttu-id="f614c-123">Stöd för att skapa index fältdefinitioner deklarativt för pynta modellklass och använda hello nya `FieldBuilder` klass.</span><span class="sxs-lookup"><span data-stu-id="f614c-123">Support for building index field definitions declaratively by decorating your model class and using hello new `FieldBuilder` class.</span></span>
* <span data-ttu-id="f614c-124">Stöd för .NET Core och portabla .NET-profil 111</span><span class="sxs-lookup"><span data-stu-id="f614c-124">Support for .NET Core and .NET Portable Profile 111</span></span>

<a name="UpgradeSteps"></a>

## <a name="steps-tooupgrade"></a><span data-ttu-id="f614c-125">Steg tooupgrade</span><span class="sxs-lookup"><span data-stu-id="f614c-125">Steps tooupgrade</span></span>
<span data-ttu-id="f614c-126">Uppdatera först NuGet-referens för `Microsoft.Azure.Search` antingen hello NuGet Package Manager-konsolen eller genom att högerklicka på projektreferenserna och välja ”hantera NuGet-paket...” i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="f614c-126">First, update your NuGet reference for `Microsoft.Azure.Search` using either hello NuGet Package Manager Console or by right-clicking on your project references and selecting "Manage NuGet Packages..." in Visual Studio.</span></span>

<span data-ttu-id="f614c-127">När NuGet har hämtats hello nya paket och deras beroenden, återskapa projektet.</span><span class="sxs-lookup"><span data-stu-id="f614c-127">Once NuGet has downloaded hello new packages and their dependencies, rebuild your project.</span></span> <span data-ttu-id="f614c-128">Beroende på hur koden är strukturerad, kan den återskapa har.</span><span class="sxs-lookup"><span data-stu-id="f614c-128">Depending on how your code is structured, it may rebuild successfully.</span></span> <span data-ttu-id="f614c-129">I så fall, är du redo toogo!</span><span class="sxs-lookup"><span data-stu-id="f614c-129">If so, you're ready toogo!</span></span>

<span data-ttu-id="f614c-130">Om din build misslyckas, bör du se ett build-fel som hello nedan:</span><span class="sxs-lookup"><span data-stu-id="f614c-130">If your build fails, you should see a build error like hello following:</span></span>

    Program.cs(31,45,31,86): error CS0266: Cannot implicitly convert type 'Microsoft.Azure.Search.ISearchIndexClient' too'Microsoft.Azure.Search.SearchIndexClient'. An explicit conversion exists (are you missing a cast?)

<span data-ttu-id="f614c-131">hello nästa steg är toofix build-fel.</span><span class="sxs-lookup"><span data-stu-id="f614c-131">hello next step is toofix this build error.</span></span> <span data-ttu-id="f614c-132">Se [bryta ändringar i version 3](#ListOfChanges) mer information om vad som orsakar hello fel och hur toofix den.</span><span class="sxs-lookup"><span data-stu-id="f614c-132">See [Breaking changes in version 3](#ListOfChanges) for details on what causes hello error and how toofix it.</span></span>

<span data-ttu-id="f614c-133">Du kan se ytterligare build varningar relaterade tooobsolete metoder eller egenskaper.</span><span class="sxs-lookup"><span data-stu-id="f614c-133">You may see additional build warnings related tooobsolete methods or properties.</span></span> <span data-ttu-id="f614c-134">hello varningar innehåller instruktioner om vilken toouse i stället för hello föråldrad funktion.</span><span class="sxs-lookup"><span data-stu-id="f614c-134">hello warnings will include instructions on what toouse instead of hello deprecated feature.</span></span> <span data-ttu-id="f614c-135">Till exempel om programmet använder hello `IndexingParameters.Base64EncodeKeys` egenskapen som du bör få en varning om att`"This property is obsolete. Please create a field mapping using 'FieldMapping.Base64Encode' instead."`</span><span class="sxs-lookup"><span data-stu-id="f614c-135">For example, if your application uses hello `IndexingParameters.Base64EncodeKeys` property, you should get a warning that says `"This property is obsolete. Please create a field mapping using 'FieldMapping.Base64Encode' instead."`</span></span>

<span data-ttu-id="f614c-136">När du har åtgärdat eventuella build-fel, kan du ändra tooyour programmet tootake utnyttja nya funktioner om du vill.</span><span class="sxs-lookup"><span data-stu-id="f614c-136">Once you've fixed any build errors, you can make changes tooyour application tootake advantage of new functionality if you wish.</span></span> <span data-ttu-id="f614c-137">Nya funktioner i hello SDK beskrivs i [vad är nytt i version 3](#WhatsNew).</span><span class="sxs-lookup"><span data-stu-id="f614c-137">New features in hello SDK are detailed in [What's new in version 3](#WhatsNew).</span></span>

<a name="ListOfChanges"></a>

## <a name="breaking-changes-in-version-3"></a><span data-ttu-id="f614c-138">Gör ändringar i version 3</span><span class="sxs-lookup"><span data-stu-id="f614c-138">Breaking changes in version 3</span></span>
<span data-ttu-id="f614c-139">Det ändrar ett litet antal viktiga förändringar i version 3 för som kan kräva kod dessutom toorebuilding ditt program.</span><span class="sxs-lookup"><span data-stu-id="f614c-139">There a small number of breaking changes in version 3 that may require code changes in addition toorebuilding your application.</span></span>

### <a name="indexesgetclient-return-type"></a><span data-ttu-id="f614c-140">Indexes.GetClient returtyp</span><span class="sxs-lookup"><span data-stu-id="f614c-140">Indexes.GetClient return type</span></span>
<span data-ttu-id="f614c-141">Hej `Indexes.GetClient` metoden har en ny returtyp.</span><span class="sxs-lookup"><span data-stu-id="f614c-141">hello `Indexes.GetClient` method has a new return type.</span></span> <span data-ttu-id="f614c-142">Tidigare returnerade `SearchIndexClient`, men detta har ändrats för`ISearchIndexClient` i version 2.0-preview och som följer ändra med tooversion 3.</span><span class="sxs-lookup"><span data-stu-id="f614c-142">Previously, it returned `SearchIndexClient`, but this was changed too`ISearchIndexClient` in version 2.0-preview, and that change carries over tooversion 3.</span></span> <span data-ttu-id="f614c-143">Detta är toosupport kunder som vill toomock hello `GetClient` metod för kontroller genom att returnera en fingerad implementering av `ISearchIndexClient`.</span><span class="sxs-lookup"><span data-stu-id="f614c-143">This is toosupport customers that wish toomock hello `GetClient` method for unit tests by returning a mock implementation of `ISearchIndexClient`.</span></span>

#### <a name="example"></a><span data-ttu-id="f614c-144">Exempel</span><span class="sxs-lookup"><span data-stu-id="f614c-144">Example</span></span>
<span data-ttu-id="f614c-145">Om din kod ser ut så här:</span><span class="sxs-lookup"><span data-stu-id="f614c-145">If your code looks like this:</span></span>

```csharp
SearchIndexClient indexClient = serviceClient.Indexes.GetClient("hotels");
```

<span data-ttu-id="f614c-146">Du kan ändra den toothis toofix någon skapa fel:</span><span class="sxs-lookup"><span data-stu-id="f614c-146">You can change it toothis toofix any build errors:</span></span>

```csharp
ISearchIndexClient indexClient = serviceClient.Indexes.GetClient("hotels");
```

### <a name="analyzername-datatype-and-others-are-no-longer-implicitly-convertible-toostrings"></a><span data-ttu-id="f614c-147">AnalyzerName och datatyp är inte längre implicit omvandlas toostrings</span><span class="sxs-lookup"><span data-stu-id="f614c-147">AnalyzerName, DataType, and others are no longer implicitly convertible toostrings</span></span>
<span data-ttu-id="f614c-148">Det finns många typer i hello Azure Search .NET SDK som härleds från `ExtensibleEnum`.</span><span class="sxs-lookup"><span data-stu-id="f614c-148">There are many types in hello Azure Search .NET SDK that derive from `ExtensibleEnum`.</span></span> <span data-ttu-id="f614c-149">Tidigare var dessa typer av alla implicit omvandlas tootype `string`.</span><span class="sxs-lookup"><span data-stu-id="f614c-149">Previously these types were all implicitly convertible tootype `string`.</span></span> <span data-ttu-id="f614c-150">Men ett fel upptäcktes i hello `Object.Equals` implementering för dessa klasser och åtgärdar hello bugg krävs om du inaktiverar den här implicit konvertering.</span><span class="sxs-lookup"><span data-stu-id="f614c-150">However, a bug was discovered in hello `Object.Equals` implementation for these classes, and fixing hello bug required disabling this implicit conversion.</span></span> <span data-ttu-id="f614c-151">Explicit konvertering för`string` fortfarande är tillåtet.</span><span class="sxs-lookup"><span data-stu-id="f614c-151">Explicit conversion too`string` is still allowed.</span></span>

#### <a name="example"></a><span data-ttu-id="f614c-152">Exempel</span><span class="sxs-lookup"><span data-stu-id="f614c-152">Example</span></span>
<span data-ttu-id="f614c-153">Om din kod ser ut så här:</span><span class="sxs-lookup"><span data-stu-id="f614c-153">If your code looks like this:</span></span>

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

<span data-ttu-id="f614c-154">Du kan ändra den toothis toofix någon skapa fel:</span><span class="sxs-lookup"><span data-stu-id="f614c-154">You can change it toothis toofix any build errors:</span></span>

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

### <a name="removed-obsolete-members"></a><span data-ttu-id="f614c-155">Ta bort föråldrade medlemmar</span><span class="sxs-lookup"><span data-stu-id="f614c-155">Removed obsolete members</span></span>

<span data-ttu-id="f614c-156">Du kan se build fel relaterade toomethods och egenskaper som har markerats som föråldrade i version 2.0-preview och därefter tas bort i version 3.</span><span class="sxs-lookup"><span data-stu-id="f614c-156">You may see build errors related toomethods or properties that were marked as obsolete in version 2.0-preview and subsequently removed in version 3.</span></span> <span data-ttu-id="f614c-157">Om du stöter på dessa fel, hur tooresolve dem:</span><span class="sxs-lookup"><span data-stu-id="f614c-157">If you encounter such errors, here is how tooresolve them:</span></span>

- <span data-ttu-id="f614c-158">Om du använder den här konstruktorn: `ScoringParameter(string name, string value)`, Använd den här i stället:`ScoringParameter(string name, IEnumerable<string> values)`</span><span class="sxs-lookup"><span data-stu-id="f614c-158">If you were using this constructor: `ScoringParameter(string name, string value)`, use this one instead: `ScoringParameter(string name, IEnumerable<string> values)`</span></span>
- <span data-ttu-id="f614c-159">Om du använde hello `ScoringParameter.Value` egenskapen, Använd hello `ScoringParameter.Values` egenskap eller hello `ToString` metod i stället.</span><span class="sxs-lookup"><span data-stu-id="f614c-159">If you were using hello `ScoringParameter.Value` property, use hello `ScoringParameter.Values` property or hello `ToString` method instead.</span></span>
- <span data-ttu-id="f614c-160">Om du använde hello `SearchRequestOptions.RequestId` egenskapen, Använd hello `ClientRequestId` egenskapen i stället.</span><span class="sxs-lookup"><span data-stu-id="f614c-160">If you were using hello `SearchRequestOptions.RequestId` property, use hello `ClientRequestId` property instead.</span></span>

### <a name="removed-preview-features"></a><span data-ttu-id="f614c-161">Borttagna förhandsgranskningsfunktioner</span><span class="sxs-lookup"><span data-stu-id="f614c-161">Removed preview features</span></span>

<span data-ttu-id="f614c-162">Om du uppgraderar från version 2.0-preview tooversion 3, Tänk på att JSON och CSV parsning stöd för Blob indexerare har tagits bort eftersom dessa funktioner är fortfarande under förhandsgranskning.</span><span class="sxs-lookup"><span data-stu-id="f614c-162">If you are upgrading from version 2.0-preview tooversion 3, be aware that JSON and CSV parsing support for Blob Indexers has been removed since these features are still in preview.</span></span> <span data-ttu-id="f614c-163">Mer specifikt hello följande metoder för hello `IndexingParametersExtensions` klass har tagits bort:</span><span class="sxs-lookup"><span data-stu-id="f614c-163">Specifically, hello following methods of hello `IndexingParametersExtensions` class have been removed:</span></span>

- `ParseJson`
- `ParseJsonArrays`
- `ParseDelimitedTextFiles`

<span data-ttu-id="f614c-164">Om programmet har en fast beroende på de här funktionerna, kommer inte att kunna tooupgrade tooversion 3 av hello Azure Search .NET SDK.</span><span class="sxs-lookup"><span data-stu-id="f614c-164">If your application has a hard dependency on these features, you will not be able tooupgrade tooversion 3 of hello Azure Search .NET SDK.</span></span> <span data-ttu-id="f614c-165">Du kan fortsätta toouse version 2.0-preview.</span><span class="sxs-lookup"><span data-stu-id="f614c-165">You can continue toouse version 2.0-preview.</span></span> <span data-ttu-id="f614c-166">Men du tänka på att **vi rekommenderar inte att använda Förhandsgranska SDK: er i produktionsprogram**.</span><span class="sxs-lookup"><span data-stu-id="f614c-166">However, please keep in mind that **we do not recommend using preview SDKs in production applications**.</span></span> <span data-ttu-id="f614c-167">Förhandsgranskningsfunktioner gäller enbart och kan ändras.</span><span class="sxs-lookup"><span data-stu-id="f614c-167">Preview features are for evaluation only and may change.</span></span>

## <a name="conclusion"></a><span data-ttu-id="f614c-168">Slutsats</span><span class="sxs-lookup"><span data-stu-id="f614c-168">Conclusion</span></span>
<span data-ttu-id="f614c-169">Om du vill ha mer information om hur du använder hello Azure Search .NET SDK finns i vår nyligen uppdaterat [anvisningar](search-howto-dotnet-sdk.md).</span><span class="sxs-lookup"><span data-stu-id="f614c-169">If you need more details on using hello Azure Search .NET SDK, see our recently updated [How-to](search-howto-dotnet-sdk.md).</span></span>

<span data-ttu-id="f614c-170">Vi uppskattar din feedback på hello SDK.</span><span class="sxs-lookup"><span data-stu-id="f614c-170">We welcome your feedback on hello SDK.</span></span> <span data-ttu-id="f614c-171">Om du får problem känna sig fria tooask oss hjälp om hello [Azure Search MSDN-forum](https://social.msdn.microsoft.com/Forums/azure/home?forum=azuresearch).</span><span class="sxs-lookup"><span data-stu-id="f614c-171">If you encounter problems, feel free tooask us for help on hello [Azure Search MSDN forum](https://social.msdn.microsoft.com/Forums/azure/home?forum=azuresearch).</span></span> <span data-ttu-id="f614c-172">Om du hittar ett programfel kan du filen ett problem i hello [Azure .NET SDK GitHub-lagringsplatsen](https://github.com/Azure/azure-sdk-for-net/issues).</span><span class="sxs-lookup"><span data-stu-id="f614c-172">If you find a bug, you can file an issue in hello [Azure .NET SDK GitHub repository](https://github.com/Azure/azure-sdk-for-net/issues).</span></span> <span data-ttu-id="f614c-173">Se till att tooprefix problemet rubriken med ”Sök SDK”:.</span><span class="sxs-lookup"><span data-stu-id="f614c-173">Make sure tooprefix your issue title with "Search SDK: ".</span></span>

<span data-ttu-id="f614c-174">Tack för att använda Azure Search!</span><span class="sxs-lookup"><span data-stu-id="f614c-174">Thank you for using Azure Search!</span></span>

<a name="UpgradeStepsV1"></a>

## <a name="appendix-steps-tooupgrade-tooversion-11"></a><span data-ttu-id="f614c-175">Bilaga: Steg tooupgrade tooversion 1.1</span><span class="sxs-lookup"><span data-stu-id="f614c-175">Appendix: Steps tooupgrade tooversion 1.1</span></span>
> [!NOTE]
> <span data-ttu-id="f614c-176">Det här avsnittet gäller endast toousers av hello Azure Search .NET SDK version 1.0.2-preview och äldre.</span><span class="sxs-lookup"><span data-stu-id="f614c-176">This section applies only toousers of hello Azure Search .NET SDK version 1.0.2-preview and older.</span></span>
> 
> 

<span data-ttu-id="f614c-177">Uppdatera först NuGet-referens för `Microsoft.Azure.Search` antingen hello NuGet Package Manager-konsolen eller genom att högerklicka på projektreferenserna och välja ”hantera NuGet-paket...” i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="f614c-177">First, update your NuGet reference for `Microsoft.Azure.Search` using either hello NuGet Package Manager Console or by right-clicking on your project references and selecting "Manage NuGet Packages..." in Visual Studio.</span></span>

<span data-ttu-id="f614c-178">När NuGet har hämtats hello nya paket och deras beroenden, återskapa projektet.</span><span class="sxs-lookup"><span data-stu-id="f614c-178">Once NuGet has downloaded hello new packages and their dependencies, rebuild your project.</span></span>

<span data-ttu-id="f614c-179">Om du tidigare har använt version 1.0.0-preview, 1.0.1-preview eller 1.0.2-preview, hello build ska lyckas och du är redo toogo!</span><span class="sxs-lookup"><span data-stu-id="f614c-179">If you were previously using version 1.0.0-preview, 1.0.1-preview, or 1.0.2-preview, hello build should succeed and you're ready toogo!</span></span>

<span data-ttu-id="f614c-180">Om du använde tidigare version 0.13.0-preview eller äldre, bör du se Skapa fel som hello nedan:</span><span class="sxs-lookup"><span data-stu-id="f614c-180">If you were previously using version 0.13.0-preview or older, you should see build errors like hello following:</span></span>

    Program.cs(137,56,137,62): error CS0117: 'Microsoft.Azure.Search.Models.IndexBatch' does not contain a definition for 'Create'
    Program.cs(137,99,137,105): error CS0117: 'Microsoft.Azure.Search.Models.IndexAction' does not contain a definition for 'Create'
    Program.cs(146,41,146,54): error CS1061: 'Microsoft.Azure.Search.IndexBatchException' does not contain a definition for 'IndexResponse' and no extension method 'IndexResponse' accepting a first argument of type 'Microsoft.Azure.Search.IndexBatchException' could be found (are you missing a using directive or an assembly reference?)
    Program.cs(163,13,163,42): error CS0246: hello type or namespace name 'DocumentSearchResponse' could not be found (are you missing a using directive or an assembly reference?)

<span data-ttu-id="f614c-181">hello nästa steg är toofix hello build-fel i taget.</span><span class="sxs-lookup"><span data-stu-id="f614c-181">hello next step is toofix hello build errors one by one.</span></span> <span data-ttu-id="f614c-182">De flesta kräver att ändra vissa klass- och metoden namn som har bytt namn i hello SDK.</span><span class="sxs-lookup"><span data-stu-id="f614c-182">Most will require changing some class and method names that have been renamed in hello SDK.</span></span> <span data-ttu-id="f614c-183">[Lista över bryta ändringar i version 1.1](#ListOfChangesV1) innehåller en lista över ändringarna namn.</span><span class="sxs-lookup"><span data-stu-id="f614c-183">[List of breaking changes in version 1.1](#ListOfChangesV1) contains a list of these name changes.</span></span>

<span data-ttu-id="f614c-184">Om du använder egna klasser toomodel dokument och dessa klasser har egenskaperna för icke-nullbar primitiva typer (till exempel `int` eller `bool` i C#), är en felkorrigering hello 1.1-versionen av hello SDK som bör du vara medveten om.</span><span class="sxs-lookup"><span data-stu-id="f614c-184">If you're using custom classes toomodel your documents, and those classes have properties of non-nullable primitive types (for example, `int` or `bool` in C#), there is a bug fix in hello 1.1 version of hello SDK of which you should be aware.</span></span> <span data-ttu-id="f614c-185">Se [felkorrigeringar i version 1.1](#BugFixesV1) mer information.</span><span class="sxs-lookup"><span data-stu-id="f614c-185">See [Bug fixes in version 1.1](#BugFixesV1) for more details.</span></span>

<span data-ttu-id="f614c-186">Slutligen, när du har åtgärdat eventuella build-fel kan du ändra tooyour programmet tootake utnyttja nya funktioner om du vill.</span><span class="sxs-lookup"><span data-stu-id="f614c-186">Finally, once you've fixed any build errors, you can make changes tooyour application tootake advantage of new functionality if you wish.</span></span>

<a name="ListOfChangesV1"></a>

### <a name="list-of-breaking-changes-in-version-11"></a><span data-ttu-id="f614c-187">Lista över bryta ändringar i version 1.1</span><span class="sxs-lookup"><span data-stu-id="f614c-187">List of breaking changes in version 1.1</span></span>
<span data-ttu-id="f614c-188">hello är följande lista sorterade efter hello sannolikheten att hello påverkas programkoden.</span><span class="sxs-lookup"><span data-stu-id="f614c-188">hello following list is ordered by hello likelihood that hello change will affect your application code.</span></span>

#### <a name="indexbatch-and-indexaction-changes"></a><span data-ttu-id="f614c-189">IndexBatch och IndexAction ändringar</span><span class="sxs-lookup"><span data-stu-id="f614c-189">IndexBatch and IndexAction changes</span></span>
<span data-ttu-id="f614c-190">`IndexBatch.Create`har bytt namn för`IndexBatch.New` och har inte längre en `params` argumentet.</span><span class="sxs-lookup"><span data-stu-id="f614c-190">`IndexBatch.Create` has been renamed too`IndexBatch.New` and no longer has a `params` argument.</span></span> <span data-ttu-id="f614c-191">Du kan använda `IndexBatch.New` för batchar som blanda olika typer av åtgärder (sammanslagningar, borttagningar osv.).</span><span class="sxs-lookup"><span data-stu-id="f614c-191">You can use `IndexBatch.New` for batches that mix different types of actions (merges, deletes, etc.).</span></span> <span data-ttu-id="f614c-192">Dessutom finns nya statiska metoder för att skapa batchar där alla hello åtgärder är hello samma: `Delete`, `Merge`, `MergeOrUpload`, och `Upload`.</span><span class="sxs-lookup"><span data-stu-id="f614c-192">In addition, there are new static methods for creating batches where all hello actions are hello same: `Delete`, `Merge`, `MergeOrUpload`, and `Upload`.</span></span>

<span data-ttu-id="f614c-193">`IndexAction`Offentliga konstruktorer har inte längre och dess egenskaper är nu inte ändras.</span><span class="sxs-lookup"><span data-stu-id="f614c-193">`IndexAction` no longer has public constructors and its properties are now immutable.</span></span> <span data-ttu-id="f614c-194">Du bör använda hello nya statiska metoder för att skapa åtgärder för olika ändamål: `Delete`, `Merge`, `MergeOrUpload`, och `Upload`.</span><span class="sxs-lookup"><span data-stu-id="f614c-194">You should use hello new static methods for creating actions for different purposes: `Delete`, `Merge`, `MergeOrUpload`, and `Upload`.</span></span> <span data-ttu-id="f614c-195">`IndexAction.Create`har tagits bort.</span><span class="sxs-lookup"><span data-stu-id="f614c-195">`IndexAction.Create` has been removed.</span></span> <span data-ttu-id="f614c-196">Om du använde hello-överlagring som tar endast ett dokument, kontrollera att toouse `Upload` i stället.</span><span class="sxs-lookup"><span data-stu-id="f614c-196">If you used hello overload that takes only a document, make sure toouse `Upload` instead.</span></span>

##### <a name="example"></a><span data-ttu-id="f614c-197">Exempel</span><span class="sxs-lookup"><span data-stu-id="f614c-197">Example</span></span>
<span data-ttu-id="f614c-198">Om din kod ser ut så här:</span><span class="sxs-lookup"><span data-stu-id="f614c-198">If your code looks like this:</span></span>

    var batch = IndexBatch.Create(documents.Select(doc => IndexAction.Create(doc)));
    indexClient.Documents.Index(batch);

<span data-ttu-id="f614c-199">Du kan ändra den toothis toofix någon skapa fel:</span><span class="sxs-lookup"><span data-stu-id="f614c-199">You can change it toothis toofix any build errors:</span></span>

    var batch = IndexBatch.New(documents.Select(doc => IndexAction.Upload(doc)));
    indexClient.Documents.Index(batch);

<span data-ttu-id="f614c-200">Om du vill kan du ytterligare förenkla den toothis:</span><span class="sxs-lookup"><span data-stu-id="f614c-200">If you want, you can further simplify it toothis:</span></span>

    var batch = IndexBatch.Upload(documents);
    indexClient.Documents.Index(batch);

#### <a name="indexbatchexception-changes"></a><span data-ttu-id="f614c-201">IndexBatchException ändringar</span><span class="sxs-lookup"><span data-stu-id="f614c-201">IndexBatchException changes</span></span>
<span data-ttu-id="f614c-202">Hej `IndexBatchException.IndexResponse` egenskap har ändrats för`IndexingResults`, och dess typ är nu `IList<IndexingResult>`.</span><span class="sxs-lookup"><span data-stu-id="f614c-202">hello `IndexBatchException.IndexResponse` property has been renamed too`IndexingResults`, and its type is now `IList<IndexingResult>`.</span></span>

##### <a name="example"></a><span data-ttu-id="f614c-203">Exempel</span><span class="sxs-lookup"><span data-stu-id="f614c-203">Example</span></span>
<span data-ttu-id="f614c-204">Om din kod ser ut så här:</span><span class="sxs-lookup"><span data-stu-id="f614c-204">If your code looks like this:</span></span>

    catch (IndexBatchException e)
    {
        Console.WriteLine(
            "Failed tooindex some of hello documents: {0}",
            String.Join(", ", e.IndexResponse.Results.Where(r => !r.Succeeded).Select(r => r.Key)));
    }

<span data-ttu-id="f614c-205">Du kan ändra den toothis toofix någon skapa fel:</span><span class="sxs-lookup"><span data-stu-id="f614c-205">You can change it toothis toofix any build errors:</span></span>

    catch (IndexBatchException e)
    {
        Console.WriteLine(
            "Failed tooindex some of hello documents: {0}",
            String.Join(", ", e.IndexingResults.Where(r => !r.Succeeded).Select(r => r.Key)));
    }

<a name="OperationMethodChanges"></a>

#### <a name="operation-method-changes"></a><span data-ttu-id="f614c-206">Åtgärden metoden ändringar</span><span class="sxs-lookup"><span data-stu-id="f614c-206">Operation method changes</span></span>
<span data-ttu-id="f614c-207">Varje åtgärd i hello Azure Search .NET SDK visas som en uppsättning metoden överlagringar för synkrona och asynkrona anropare.</span><span class="sxs-lookup"><span data-stu-id="f614c-207">Each operation in hello Azure Search .NET SDK is exposed as a set of method overloads for synchronous and asynchronous callers.</span></span> <span data-ttu-id="f614c-208">hello har signaturer och factoring av dessa metoden överlagringar ändrats i version 1.1.</span><span class="sxs-lookup"><span data-stu-id="f614c-208">hello signatures and factoring of these method overloads has changed in version 1.1.</span></span>

<span data-ttu-id="f614c-209">Till exempel visas hello ”hämta indexstatistik”-åtgärd i äldre versioner av hello SDK dessa signaturer:</span><span class="sxs-lookup"><span data-stu-id="f614c-209">For example, hello "Get Index Statistics" operation in older versions of hello SDK exposed these signatures:</span></span>

<span data-ttu-id="f614c-210">I `IIndexOperations`:</span><span class="sxs-lookup"><span data-stu-id="f614c-210">In `IIndexOperations`:</span></span>

    // Asynchronous operation with all parameters
    Task<IndexGetStatisticsResponse> GetStatisticsAsync(
        string indexName,
        CancellationToken cancellationToken);

<span data-ttu-id="f614c-211">I `IndexOperationsExtensions`:</span><span class="sxs-lookup"><span data-stu-id="f614c-211">In `IndexOperationsExtensions`:</span></span>

    // Asynchronous operation with only required parameters
    public static Task<IndexGetStatisticsResponse> GetStatisticsAsync(
        this IIndexOperations operations,
        string indexName);

    // Synchronous operation with only required parameters
    public static IndexGetStatisticsResponse GetStatistics(
        this IIndexOperations operations,
        string indexName);

<span data-ttu-id="f614c-212">hello metoden signaturer för hello samma åtgärd i version 1.1 ser ut så här:</span><span class="sxs-lookup"><span data-stu-id="f614c-212">hello method signatures for hello same operation in version 1.1 look like this:</span></span>

<span data-ttu-id="f614c-213">I `IIndexesOperations`:</span><span class="sxs-lookup"><span data-stu-id="f614c-213">In `IIndexesOperations`:</span></span>

    // Asynchronous operation with lower-level HTTP features exposed
    Task<AzureOperationResponse<IndexGetStatisticsResult>> GetStatisticsWithHttpMessagesAsync(
        string indexName,
        SearchRequestOptions searchRequestOptions = default(SearchRequestOptions),
        Dictionary<string, List<string>> customHeaders = null,
        CancellationToken cancellationToken = default(CancellationToken));

<span data-ttu-id="f614c-214">I `IndexesOperationsExtensions`:</span><span class="sxs-lookup"><span data-stu-id="f614c-214">In `IndexesOperationsExtensions`:</span></span>

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

<span data-ttu-id="f614c-215">Från och med version 1.1, ordnar hello Azure Search .NET SDK åtgärden metoder på olika sätt:</span><span class="sxs-lookup"><span data-stu-id="f614c-215">Starting with version 1.1, hello Azure Search .NET SDK organizes operation methods differently:</span></span>

* <span data-ttu-id="f614c-216">Valfria parametrar finns nu modelleras som standard parametrar snarare än ytterligare en metod överlagringar.</span><span class="sxs-lookup"><span data-stu-id="f614c-216">Optional parameters are now modeled as default parameters rather than additional method overloads.</span></span> <span data-ttu-id="f614c-217">Detta minskar hello antalet metoden överlagringar ibland dramatiskt.</span><span class="sxs-lookup"><span data-stu-id="f614c-217">This reduces hello number of method overloads, sometimes dramatically.</span></span>
* <span data-ttu-id="f614c-218">Hej tilläggsmetoder dölja nu mycket hello överflödig information om HTTP från hello anroparen.</span><span class="sxs-lookup"><span data-stu-id="f614c-218">hello extension methods now hide a lot of hello extraneous details of HTTP from hello caller.</span></span> <span data-ttu-id="f614c-219">Till exempel äldre versioner av hello SDK returnerade ett svarsobjekt med en HTTP-statuskod som du ofta behövde toocheck eftersom åtgärden metoder utlösa `CloudException` för varje statuskod som indikerar ett fel.</span><span class="sxs-lookup"><span data-stu-id="f614c-219">For example, older versions of hello SDK returned a response object with an HTTP status code, which you often didn't need toocheck because operation methods throw `CloudException` for any status code that indicates an error.</span></span> <span data-ttu-id="f614c-220">Hej nya tillägg metoder bara returnera modellobjekt, hello slipper toounwrap sparar du dem i din kod.</span><span class="sxs-lookup"><span data-stu-id="f614c-220">hello new extension methods just return model objects, saving you hello trouble of having toounwrap them in your code.</span></span>
* <span data-ttu-id="f614c-221">Däremot exponerar hello core gränssnitt nu metoder som ger dig större kontroll på hello HTTP nivå om det behövs.</span><span class="sxs-lookup"><span data-stu-id="f614c-221">Conversely, hello core interfaces now expose methods that give you more control at hello HTTP level if you need it.</span></span> <span data-ttu-id="f614c-222">Du kan nu skicka in anpassade HTTP-huvuden toobe i begäranden och hello nya `AzureOperationResponse<T>` returnera typen ger direktåtkomst toohello `HttpRequestMessage` och `HttpResponseMessage` för hello åtgärden.</span><span class="sxs-lookup"><span data-stu-id="f614c-222">You can now pass in custom HTTP headers toobe included in requests, and hello new `AzureOperationResponse<T>` return type gives you direct access toohello `HttpRequestMessage` and `HttpResponseMessage` for hello operation.</span></span> <span data-ttu-id="f614c-223">`AzureOperationResponse`har definierats i hello `Microsoft.Rest.Azure` namnområde och ersätter `Hyak.Common.OperationResponse`.</span><span class="sxs-lookup"><span data-stu-id="f614c-223">`AzureOperationResponse` is defined in hello `Microsoft.Rest.Azure` namespace and replaces `Hyak.Common.OperationResponse`.</span></span>

#### <a name="scoringparameters-changes"></a><span data-ttu-id="f614c-224">ScoringParameters ändringar</span><span class="sxs-lookup"><span data-stu-id="f614c-224">ScoringParameters changes</span></span>
<span data-ttu-id="f614c-225">En ny klass med namnet `ScoringParameter` har lagts till i hello senaste SDK toomake den enklare tooprovide parametrar tooscoring profiler i en sökfråga.</span><span class="sxs-lookup"><span data-stu-id="f614c-225">A new class named `ScoringParameter` has been added in hello latest SDK toomake it easier tooprovide parameters tooscoring profiles in a search query.</span></span> <span data-ttu-id="f614c-226">Tidigare hello `ScoringProfiles` -egenskapen för hello `SearchParameters` klassen angavs som `IList<string>`; Nu det skrivs som `IList<ScoringParameter>`.</span><span class="sxs-lookup"><span data-stu-id="f614c-226">Previously hello `ScoringProfiles` property of hello `SearchParameters` class was typed as `IList<string>`; Now it is typed as `IList<ScoringParameter>`.</span></span>

##### <a name="example"></a><span data-ttu-id="f614c-227">Exempel</span><span class="sxs-lookup"><span data-stu-id="f614c-227">Example</span></span>
<span data-ttu-id="f614c-228">Om din kod ser ut så här:</span><span class="sxs-lookup"><span data-stu-id="f614c-228">If your code looks like this:</span></span>

    var sp = new SearchParameters();
    sp.ScoringProfile = "jobsScoringFeatured";      // Use a scoring profile
    sp.ScoringParameters = new[] { "featuredParam-featured", "mapCenterParam-" + lon + "," + lat };

<span data-ttu-id="f614c-229">Du kan ändra den toothis toofix någon skapa fel:</span><span class="sxs-lookup"><span data-stu-id="f614c-229">You can change it toothis toofix any build errors:</span></span> 

    var sp = new SearchParameters();
    sp.ScoringProfile = "jobsScoringFeatured";      // Use a scoring profile
    sp.ScoringParameters =
        new[]
        {
            new ScoringParameter("featuredParam", new[] { "featured" }),
            new ScoringParameter("mapCenterParam", GeographyPoint.Create(lat, lon))
        };

#### <a name="model-class-changes"></a><span data-ttu-id="f614c-230">Klassen Modelländringar</span><span class="sxs-lookup"><span data-stu-id="f614c-230">Model class changes</span></span>
<span data-ttu-id="f614c-231">På grund av toohello signatur ändringar som beskrivs i [igen metoden ändringar](#OperationMethodChanges), många klasser i hello `Microsoft.Azure.Search.Models` namnområde har ändrats eller tagits bort.</span><span class="sxs-lookup"><span data-stu-id="f614c-231">Due toohello signature changes described in [Operation method changes](#OperationMethodChanges), many classes in hello `Microsoft.Azure.Search.Models` namespace have been renamed or removed.</span></span> <span data-ttu-id="f614c-232">Exempel:</span><span class="sxs-lookup"><span data-stu-id="f614c-232">For example:</span></span>

* <span data-ttu-id="f614c-233">`IndexDefinitionResponse`har ersatts av`AzureOperationResponse<Index>`</span><span class="sxs-lookup"><span data-stu-id="f614c-233">`IndexDefinitionResponse` has been replaced by `AzureOperationResponse<Index>`</span></span>
* <span data-ttu-id="f614c-234">`DocumentSearchResponse`har bytt namn för`DocumentSearchResult`</span><span class="sxs-lookup"><span data-stu-id="f614c-234">`DocumentSearchResponse` has been renamed too`DocumentSearchResult`</span></span>
* <span data-ttu-id="f614c-235">`IndexResult`har bytt namn för`IndexingResult`</span><span class="sxs-lookup"><span data-stu-id="f614c-235">`IndexResult` has been renamed too`IndexingResult`</span></span>
* <span data-ttu-id="f614c-236">`Documents.Count()`Returnerar nu en `long` med hello dokumentantal i stället för en`DocumentCountResponse`</span><span class="sxs-lookup"><span data-stu-id="f614c-236">`Documents.Count()` now returns a `long` with hello document count instead of a `DocumentCountResponse`</span></span>
* <span data-ttu-id="f614c-237">`IndexGetStatisticsResponse`har bytt namn för`IndexGetStatisticsResult`</span><span class="sxs-lookup"><span data-stu-id="f614c-237">`IndexGetStatisticsResponse` has been renamed too`IndexGetStatisticsResult`</span></span>
* <span data-ttu-id="f614c-238">`IndexListResponse`har bytt namn för`IndexListResult`</span><span class="sxs-lookup"><span data-stu-id="f614c-238">`IndexListResponse` has been renamed too`IndexListResult`</span></span>

<span data-ttu-id="f614c-239">toosummarize, `OperationResponse`-härledda klasser som fanns endast toowrap model-objektet har tagits bort.</span><span class="sxs-lookup"><span data-stu-id="f614c-239">toosummarize, `OperationResponse`-derived classes that existed only toowrap a model object have been removed.</span></span> <span data-ttu-id="f614c-240">hello återstående klasser har fått sina suffix har ändrats från `Response` för`Result`.</span><span class="sxs-lookup"><span data-stu-id="f614c-240">hello remaining classes have had their suffix changed from `Response` too`Result`.</span></span>

##### <a name="example"></a><span data-ttu-id="f614c-241">Exempel</span><span class="sxs-lookup"><span data-stu-id="f614c-241">Example</span></span>
<span data-ttu-id="f614c-242">Om din kod ser ut så här:</span><span class="sxs-lookup"><span data-stu-id="f614c-242">If your code looks like this:</span></span>

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

<span data-ttu-id="f614c-243">Du kan ändra den toothis toofix någon skapa fel:</span><span class="sxs-lookup"><span data-stu-id="f614c-243">You can change it toothis toofix any build errors:</span></span>

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

##### <a name="response-classes-and-ienumerable"></a><span data-ttu-id="f614c-244">Svaret klasser och IEnumerable</span><span class="sxs-lookup"><span data-stu-id="f614c-244">Response classes and IEnumerable</span></span>
<span data-ttu-id="f614c-245">En ytterligare ändring som kan påverka din kod är att implementera svar klasser som innehåller samlingar längre `IEnumerable<T>`.</span><span class="sxs-lookup"><span data-stu-id="f614c-245">An additional change that may affect your code is that response classes that hold collections no longer implement `IEnumerable<T>`.</span></span> <span data-ttu-id="f614c-246">Du kan i stället öppna hello samlingsegenskap direkt.</span><span class="sxs-lookup"><span data-stu-id="f614c-246">Instead, you can access hello collection property directly.</span></span> <span data-ttu-id="f614c-247">Om exempelvis koden ser ut så här:</span><span class="sxs-lookup"><span data-stu-id="f614c-247">For example, if your code looks like this:</span></span>

    DocumentSearchResponse<Hotel> response = indexClient.Documents.Search<Hotel>(searchText, sp);
    foreach (SearchResult<Hotel> result in response)
    {
        Console.WriteLine(result.Document);
    }

<span data-ttu-id="f614c-248">Du kan ändra den toothis toofix någon skapa fel:</span><span class="sxs-lookup"><span data-stu-id="f614c-248">You can change it toothis toofix any build errors:</span></span>

    DocumentSearchResult<Hotel> response = indexClient.Documents.Search<Hotel>(searchText, sp);
    foreach (SearchResult<Hotel> result in response.Results)
    {
        Console.WriteLine(result.Document);
    }

##### <a name="special-case-for-web-applications"></a><span data-ttu-id="f614c-249">Specialfall för webbprogram</span><span class="sxs-lookup"><span data-stu-id="f614c-249">Special case for web applications</span></span>
<span data-ttu-id="f614c-250">Om du har ett webbprogram som Serialiserar `DocumentSearchResponse` direkt toosend sökresultat toohello webbläsare, behöver du toochange koden eller hello resultat kommer inte att serialisera korrekt.</span><span class="sxs-lookup"><span data-stu-id="f614c-250">If you have a web application that serializes `DocumentSearchResponse` directly toosend search results toohello browser, you will need toochange your code or hello results will not serialize correctly.</span></span> <span data-ttu-id="f614c-251">Om exempelvis koden ser ut så här:</span><span class="sxs-lookup"><span data-stu-id="f614c-251">For example, if your code looks like this:</span></span>

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

<span data-ttu-id="f614c-252">Du kan ändra den genom att hämta hello `.Results` -egenskapen för hello Sök svar toofix Sök resultatet återgivning:</span><span class="sxs-lookup"><span data-stu-id="f614c-252">You can change it by getting hello `.Results` property of hello search response toofix search result rendering:</span></span>

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

<span data-ttu-id="f614c-253">Du har toolook sådana fall i koden själv. **hello kompileraren varnar dig inte** eftersom `JsonResult.Data` är av typen `object`.</span><span class="sxs-lookup"><span data-stu-id="f614c-253">You will have toolook for such cases in your code yourself; **hello compiler will not warn you** because `JsonResult.Data` is of type `object`.</span></span>

#### <a name="cloudexception-changes"></a><span data-ttu-id="f614c-254">CloudException ändringar</span><span class="sxs-lookup"><span data-stu-id="f614c-254">CloudException changes</span></span>
<span data-ttu-id="f614c-255">Hej `CloudException` klass har flyttats från hello `Hyak.Common` namnområde toohello `Microsoft.Rest.Azure` namnområde.</span><span class="sxs-lookup"><span data-stu-id="f614c-255">hello `CloudException` class has moved from hello `Hyak.Common` namespace toohello `Microsoft.Rest.Azure` namespace.</span></span> <span data-ttu-id="f614c-256">Dessutom dess `Error` egenskap har ändrats för`Body`.</span><span class="sxs-lookup"><span data-stu-id="f614c-256">Also, its `Error` property has been renamed too`Body`.</span></span>

#### <a name="searchserviceclient-and-searchindexclient-changes"></a><span data-ttu-id="f614c-257">SearchServiceClient och SearchIndexClient ändringar</span><span class="sxs-lookup"><span data-stu-id="f614c-257">SearchServiceClient and SearchIndexClient changes</span></span>
<span data-ttu-id="f614c-258">Hej typ av hello `Credentials` egenskap har ändrats från `SearchCredentials` tooits basklassen `ServiceClientCredentials`.</span><span class="sxs-lookup"><span data-stu-id="f614c-258">hello type of hello `Credentials` property has changed from `SearchCredentials` tooits base class, `ServiceClientCredentials`.</span></span> <span data-ttu-id="f614c-259">Om du behöver tooaccess hello `SearchCredentials` av en `SearchIndexClient` eller `SearchServiceClient`, Använd hello nya `SearchCredentials` egenskapen.</span><span class="sxs-lookup"><span data-stu-id="f614c-259">If you need tooaccess hello `SearchCredentials` of a `SearchIndexClient` or `SearchServiceClient`, please use hello new `SearchCredentials` property.</span></span>

<span data-ttu-id="f614c-260">I tidigare versioner av hello SDK, `SearchServiceClient` och `SearchIndexClient` hade konstruktorer som tog en `HttpClient` parameter.</span><span class="sxs-lookup"><span data-stu-id="f614c-260">In older versions of hello SDK, `SearchServiceClient` and `SearchIndexClient` had constructors that took an `HttpClient` parameter.</span></span> <span data-ttu-id="f614c-261">Dessa har ersatts av konstruktorer som tar en `HttpClientHandler` och en matris med `DelegatingHandler` objekt.</span><span class="sxs-lookup"><span data-stu-id="f614c-261">These have been replaced with constructors that take an `HttpClientHandler` and an array of `DelegatingHandler` objects.</span></span> <span data-ttu-id="f614c-262">Detta gör det enklare tooinstall anpassad hanterare toopre processen HTTP-begäranden om det behövs.</span><span class="sxs-lookup"><span data-stu-id="f614c-262">This makes it easier tooinstall custom handlers toopre-process HTTP requests if necessary.</span></span>

<span data-ttu-id="f614c-263">Slutligen hello konstruktorer som tog en `Uri` och `SearchCredentials` har ändrats.</span><span class="sxs-lookup"><span data-stu-id="f614c-263">Finally, hello constructors that took a `Uri` and `SearchCredentials` have changed.</span></span> <span data-ttu-id="f614c-264">Till exempel om du har kod som ser ut så här:</span><span class="sxs-lookup"><span data-stu-id="f614c-264">For example, if you have code that looks like this:</span></span>

    var client =
        new SearchServiceClient(
            new SearchCredentials("abc123"),
            new Uri("http://myservice.search.windows.net"));

<span data-ttu-id="f614c-265">Du kan ändra den toothis toofix någon skapa fel:</span><span class="sxs-lookup"><span data-stu-id="f614c-265">You can change it toothis toofix any build errors:</span></span>

    var client =
        new SearchServiceClient(
            new Uri("http://myservice.search.windows.net"),
            new SearchCredentials("abc123"));

<span data-ttu-id="f614c-266">Observera att hello typ av hello autentiseringsuppgifter parametern har också ändrats för`ServiceClientCredentials`.</span><span class="sxs-lookup"><span data-stu-id="f614c-266">Also note that hello type of hello credentials parameter has changed too`ServiceClientCredentials`.</span></span> <span data-ttu-id="f614c-267">Detta är inte troligt tooaffect koden sedan `SearchCredentials` är härledd från `ServiceClientCredentials`.</span><span class="sxs-lookup"><span data-stu-id="f614c-267">This is unlikely tooaffect your code since `SearchCredentials` is derived from `ServiceClientCredentials`.</span></span>

#### <a name="passing-a-request-id"></a><span data-ttu-id="f614c-268">Skicka en begäran-ID</span><span class="sxs-lookup"><span data-stu-id="f614c-268">Passing a request ID</span></span>
<span data-ttu-id="f614c-269">I äldre versioner av hello SDK kan du ange en begärande-ID på hello `SearchServiceClient` eller `SearchIndexClient` och det skulle ingå i varje begäran toohello REST API.</span><span class="sxs-lookup"><span data-stu-id="f614c-269">In older versions of hello SDK, you could set a request ID on hello `SearchServiceClient` or `SearchIndexClient` and it would be included in every request toohello REST API.</span></span> <span data-ttu-id="f614c-270">Detta är användbart för felsökning av problem med din söktjänst om du behöver stöd för toocontact.</span><span class="sxs-lookup"><span data-stu-id="f614c-270">This is useful for troubleshooting issues with your search service if you need toocontact support.</span></span> <span data-ttu-id="f614c-271">Det är dock mer användbar tooset ett unikt begäran-ID för varje åtgärd i stället för att toouse hello samma ID för alla åtgärder.</span><span class="sxs-lookup"><span data-stu-id="f614c-271">However, it is more useful tooset a unique request ID for every operation rather than toouse hello same ID for all operations.</span></span> <span data-ttu-id="f614c-272">Därför hello `SetClientRequestId` metoder för `SearchServiceClient` och `SearchIndexClient` har tagits bort.</span><span class="sxs-lookup"><span data-stu-id="f614c-272">For this reason, hello `SetClientRequestId` methods of `SearchServiceClient` and `SearchIndexClient` have been removed.</span></span> <span data-ttu-id="f614c-273">I stället kan du kan skicka en metod för begäran-ID tooeach åtgärden via hello valfria `SearchRequestOptions` parameter.</span><span class="sxs-lookup"><span data-stu-id="f614c-273">Instead, you can pass a request ID tooeach operation method via hello optional `SearchRequestOptions` parameter.</span></span>

> [!NOTE]
> <span data-ttu-id="f614c-274">I en framtida utgåva av hello SDK vi lägga till en ny mekanism för att ange ID för förfrågan globalt på klientens hello objekt som är kompatibel med hello-metod som används av andra Azure-SDK.</span><span class="sxs-lookup"><span data-stu-id="f614c-274">In a future release of hello SDK, we will add a new mechanism for setting a request ID globally on hello client objects that is consistent with hello approach used by other Azure SDKs.</span></span>
> 
> 

#### <a name="example"></a><span data-ttu-id="f614c-275">Exempel</span><span class="sxs-lookup"><span data-stu-id="f614c-275">Example</span></span>
<span data-ttu-id="f614c-276">Om du har kod som ser ut så här:</span><span class="sxs-lookup"><span data-stu-id="f614c-276">If you have code that looks like this:</span></span>

    client.SetClientRequestId(Guid.NewGuid());
    ...
    long count = client.Documents.Count();

<span data-ttu-id="f614c-277">Du kan ändra den toothis toofix någon skapa fel:</span><span class="sxs-lookup"><span data-stu-id="f614c-277">You can change it toothis toofix any build errors:</span></span>

    long count = client.Documents.Count(new SearchRequestOptions(requestId: Guid.NewGuid()));

#### <a name="interface-name-changes"></a><span data-ttu-id="f614c-278">Gränssnittet namnändringar</span><span class="sxs-lookup"><span data-stu-id="f614c-278">Interface name changes</span></span>
<span data-ttu-id="f614c-279">hello åtgärden gruppnamn gränssnitt har alla ändrade toobe som överensstämmer med deras motsvarande egenskapsnamn:</span><span class="sxs-lookup"><span data-stu-id="f614c-279">hello operation group interface names have all changed toobe consistent with their corresponding property names:</span></span>

* <span data-ttu-id="f614c-280">Hej typ av `ISearchServiceClient.Indexes` har ändrats från `IIndexOperations` för`IIndexesOperations`.</span><span class="sxs-lookup"><span data-stu-id="f614c-280">hello type of `ISearchServiceClient.Indexes` has been renamed from `IIndexOperations` too`IIndexesOperations`.</span></span>
* <span data-ttu-id="f614c-281">Hej typ av `ISearchServiceClient.Indexers` har ändrats från `IIndexerOperations` för`IIndexersOperations`.</span><span class="sxs-lookup"><span data-stu-id="f614c-281">hello type of `ISearchServiceClient.Indexers` has been renamed from `IIndexerOperations` too`IIndexersOperations`.</span></span>
* <span data-ttu-id="f614c-282">Hej typ av `ISearchServiceClient.DataSources` har ändrats från `IDataSourceOperations` för`IDataSourcesOperations`.</span><span class="sxs-lookup"><span data-stu-id="f614c-282">hello type of `ISearchServiceClient.DataSources` has been renamed from `IDataSourceOperations` too`IDataSourcesOperations`.</span></span>
* <span data-ttu-id="f614c-283">Hej typ av `ISearchIndexClient.Documents` har ändrats från `IDocumentOperations` för`IDocumentsOperations`.</span><span class="sxs-lookup"><span data-stu-id="f614c-283">hello type of `ISearchIndexClient.Documents` has been renamed from `IDocumentOperations` too`IDocumentsOperations`.</span></span>

<span data-ttu-id="f614c-284">Den här ändringen är osannolikt tooaffect din kod om du har skapat mocks av dessa gränssnitt för testning.</span><span class="sxs-lookup"><span data-stu-id="f614c-284">This change is unlikely tooaffect your code unless you created mocks of these interfaces for test purposes.</span></span>

<a name="BugFixesV1"></a>

### <a name="bug-fixes-in-version-11"></a><span data-ttu-id="f614c-285">Felkorrigeringar i version 1.1</span><span class="sxs-lookup"><span data-stu-id="f614c-285">Bug fixes in version 1.1</span></span>
<span data-ttu-id="f614c-286">Det uppstod ett fel i äldre versioner av hello Azure Search .NET SDK om tooserialization av anpassade modellen klasser.</span><span class="sxs-lookup"><span data-stu-id="f614c-286">There was a bug in older versions of hello Azure Search .NET SDK relating tooserialization of custom model classes.</span></span> <span data-ttu-id="f614c-287">hello fel kan inträffa om du har skapat en anpassad modellklass med en egenskap för en icke-nullbar värdetyp.</span><span class="sxs-lookup"><span data-stu-id="f614c-287">hello bug could occur if you created a custom model class with a property of a non-nullable value type.</span></span>

#### <a name="steps-tooreproduce"></a><span data-ttu-id="f614c-288">Steg tooreproduce</span><span class="sxs-lookup"><span data-stu-id="f614c-288">Steps tooreproduce</span></span>
<span data-ttu-id="f614c-289">Skapa en anpassad modellklass med en egenskap av typen icke-kan ha värdet null.</span><span class="sxs-lookup"><span data-stu-id="f614c-289">Create a custom model class with a property of non-nullable value type.</span></span> <span data-ttu-id="f614c-290">Till exempel lägga till en offentlig `UnitCount` egenskap av typen `int` i stället för `int?`.</span><span class="sxs-lookup"><span data-stu-id="f614c-290">For example, add a public `UnitCount` property of type `int` instead of `int?`.</span></span>

<span data-ttu-id="f614c-291">Om du indexera ett dokument med hello standardvärdet för den aktuella typen (till exempel 0 för `int`), hello fältet är null i Azure Search.</span><span class="sxs-lookup"><span data-stu-id="f614c-291">If you index a document with hello default value of that type (for example, 0 for `int`), hello field will be null in Azure Search.</span></span> <span data-ttu-id="f614c-292">Om du senare söker efter dokumentet hello `Search` anropet kommer att kasta `JsonSerializationException` klagande som det går inte att konvertera `null` för`int`.</span><span class="sxs-lookup"><span data-stu-id="f614c-292">If you subsequently search for that document, hello `Search` call will throw `JsonSerializationException` complaining that it can't convert `null` too`int`.</span></span>

<span data-ttu-id="f614c-293">Filter får dessutom inte fungerar som förväntat eftersom null skrevs toohello index i stället för hello avsett värde.</span><span class="sxs-lookup"><span data-stu-id="f614c-293">Also, filters may not work as expected since null was written toohello index instead of hello intended value.</span></span>

#### <a name="fix-details"></a><span data-ttu-id="f614c-294">Åtgärda information</span><span class="sxs-lookup"><span data-stu-id="f614c-294">Fix details</span></span>
<span data-ttu-id="f614c-295">Vi har löst problemet i version 1.1 av hello SDK.</span><span class="sxs-lookup"><span data-stu-id="f614c-295">We have fixed this issue in version 1.1 of hello SDK.</span></span> <span data-ttu-id="f614c-296">Nu om du har en modellklass så här:</span><span class="sxs-lookup"><span data-stu-id="f614c-296">Now, if you have a model class like this:</span></span>

    public class Model
    {
        public string Key { get; set; }

        public int IntValue { get; set; }
    }

<span data-ttu-id="f614c-297">och du ställer in `IntValue` too0 att värdet är nu korrekt serialiserad som 0 hello uppkopplat läge och lagras som 0 i hello index.</span><span class="sxs-lookup"><span data-stu-id="f614c-297">and you set `IntValue` too0, that value is now correctly serialized as 0 on hello wire and stored as 0 in hello index.</span></span> <span data-ttu-id="f614c-298">Avrunda utlösning också fungerar som förväntat.</span><span class="sxs-lookup"><span data-stu-id="f614c-298">Round tripping also works as expected.</span></span>

<span data-ttu-id="f614c-299">En potentiell problemet toobe är medveten om med den här metoden: Om du använder en modell av typen har ett icke-nullbar egenskapen, det finns också**garantera** att inga dokument i indexet innehåller ett null-värde för hello motsvarande fält.</span><span class="sxs-lookup"><span data-stu-id="f614c-299">There is one potential issue toobe aware of with this approach: If you use a model type with a non-nullable property, you have too**guarantee** that no documents in your index contain a null value for hello corresponding field.</span></span> <span data-ttu-id="f614c-300">Varken hello SDK eller hello Azure Search REST API hjälper du tooenforce detta.</span><span class="sxs-lookup"><span data-stu-id="f614c-300">Neither hello SDK nor hello Azure Search REST API will help you tooenforce this.</span></span>

<span data-ttu-id="f614c-301">Detta är inte bara ett hypotetiskt problem: Tänk dig ett scenario där du lägger till ett nytt fält tooan befintliga index som är av typen `Edm.Int32`.</span><span class="sxs-lookup"><span data-stu-id="f614c-301">This is not just a hypothetical concern: Imagine a scenario where you add a new field tooan existing index that is of type `Edm.Int32`.</span></span> <span data-ttu-id="f614c-302">När du har uppdaterat hello indexdefinitionen har alla dokument som ett null-värde för det nya fältet (eftersom alla typer inte kan vara null i Azure Search).</span><span class="sxs-lookup"><span data-stu-id="f614c-302">After updating hello index definition, all documents will have a null value for that new field (since all types are nullable in Azure Search).</span></span> <span data-ttu-id="f614c-303">Om du använder en modellklass sedan med en icke-nullbar `int` -egenskapen för det här fältet får du en `JsonSerializationException` så här när du försöker tooretrieve dokument:</span><span class="sxs-lookup"><span data-stu-id="f614c-303">If you then use a model class with a non-nullable `int` property for that field, you will get a `JsonSerializationException` like this when trying tooretrieve documents:</span></span>

    Error converting value {null} tootype 'System.Int32'. Path 'IntValue'.

<span data-ttu-id="f614c-304">Därför kan rekommenderar vi ändå att du använder kan ha värdet null typer i din modell-klasser som bästa praxis.</span><span class="sxs-lookup"><span data-stu-id="f614c-304">For this reason, we still recommend that you use nullable types in your model classes as a best practice.</span></span>

<span data-ttu-id="f614c-305">Mer information om korrigeringsfilen programfel och hello finns [problemet på GitHub](https://github.com/Azure/azure-sdk-for-net/issues/1063).</span><span class="sxs-lookup"><span data-stu-id="f614c-305">For more details on this bug and hello fix, please see [this issue on GitHub](https://github.com/Azure/azure-sdk-for-net/issues/1063).</span></span>

