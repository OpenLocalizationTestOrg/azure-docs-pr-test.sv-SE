---
title: Uppgradera till Azure Search .NET SDK version 1.1 | Microsoft Docs
description: Uppgradera till Azure Search .NET SDK version 1.1
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
ms.openlocfilehash: 9782454e3bfc697b63cde8aa28a14be0c393c36b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="upgrading-to-the-azure-search-net-sdk-version-3"></a><span data-ttu-id="4dd8d-103">Uppgradera till Azure Search .NET SDK version 3</span><span class="sxs-lookup"><span data-stu-id="4dd8d-103">Upgrading to the Azure Search .NET SDK version 3</span></span>
<span data-ttu-id="4dd8d-104">Om du använder version 2.0 Förhandsgranska eller äldre av den [Azure Search .NET SDK](https://aka.ms/search-sdk), den här artikeln hjälper dig att uppgradera ditt program att använda version 3.</span><span class="sxs-lookup"><span data-stu-id="4dd8d-104">If you're using version 2.0-preview or older of the [Azure Search .NET SDK](https://aka.ms/search-sdk), this article will help you upgrade your application to use version 3.</span></span>

<span data-ttu-id="4dd8d-105">En mer allmän genomgång av SDK inklusive exempel finns [hur du använder Azure Search från ett .NET-program](search-howto-dotnet-sdk.md).</span><span class="sxs-lookup"><span data-stu-id="4dd8d-105">For a more general walkthrough of the SDK including examples, see [How to use Azure Search from a .NET Application](search-howto-dotnet-sdk.md).</span></span>

<span data-ttu-id="4dd8d-106">Azure Search .NET SDK-version 3 innehåller vissa ändringar från tidigare versioner.</span><span class="sxs-lookup"><span data-stu-id="4dd8d-106">Version 3 of the Azure Search .NET SDK contains some changes from earlier versions.</span></span> <span data-ttu-id="4dd8d-107">Detta är oftast mindre, så ändrar koden kräver endast minsta möjliga ansträngning.</span><span class="sxs-lookup"><span data-stu-id="4dd8d-107">These are mostly minor, so changing your code should require only minimal effort.</span></span> <span data-ttu-id="4dd8d-108">Se [steg för att uppgradera](#UpgradeSteps) för instruktioner om hur du ändrar koden att använda den nya SDK-versionen.</span><span class="sxs-lookup"><span data-stu-id="4dd8d-108">See [Steps to upgrade](#UpgradeSteps) for instructions on how to change your code to use the new SDK version.</span></span>

> [!NOTE]
> <span data-ttu-id="4dd8d-109">Om du använder version 1.0.2-preview eller äldre, bör du uppgradera till version 1.1 först och därefter uppgradera till version 3.</span><span class="sxs-lookup"><span data-stu-id="4dd8d-109">If you're using version 1.0.2-preview or older, you should upgrade to version 1.1 first, and then upgrade to version 3.</span></span> <span data-ttu-id="4dd8d-110">Se [bilaga: steg för att uppgradera till version 1.1](#UpgradeStepsV1) anvisningar.</span><span class="sxs-lookup"><span data-stu-id="4dd8d-110">See [Appendix: Steps to upgrade to version 1.1](#UpgradeStepsV1) for instructions.</span></span>
>
> <span data-ttu-id="4dd8d-111">Din Azure Search-tjänstinstansen stöder flera REST API-versioner, inklusive det senaste.</span><span class="sxs-lookup"><span data-stu-id="4dd8d-111">Your Azure Search service instance supports several REST API versions, including the latest one.</span></span> <span data-ttu-id="4dd8d-112">Du kan fortsätta att använda en version när det är inte längre den senaste, men vi rekommenderar att du migrerar din kod för att använda den senaste versionen.</span><span class="sxs-lookup"><span data-stu-id="4dd8d-112">You can continue to use a version when it is no longer the latest one, but we recommend that you migrate your code to use the newest version.</span></span> <span data-ttu-id="4dd8d-113">När du använder REST-API, måste du ange API-versionen i varje begäran via parametern api-version.</span><span class="sxs-lookup"><span data-stu-id="4dd8d-113">When using the REST API, you must specify the API version in every request via the api-version parameter.</span></span> <span data-ttu-id="4dd8d-114">När du använder .NET SDK, anger versionen av du använder SDK motsvarande version av REST API.</span><span class="sxs-lookup"><span data-stu-id="4dd8d-114">When using the .NET SDK, the version of the SDK you’re using determines the corresponding version of the REST API.</span></span> <span data-ttu-id="4dd8d-115">Om du använder en äldre SDK kan fortsätta du att köra koden utan ändringar, även om tjänsten uppgraderas för att stödja en nyare API-version.</span><span class="sxs-lookup"><span data-stu-id="4dd8d-115">If you are using an older SDK, you can continue to run that code with no changes even if the service is upgraded to support a newer API version.</span></span>

<a name="WhatsNew"></a>

## <a name="whats-new-in-version-3"></a><span data-ttu-id="4dd8d-116">Vad är nytt i version 3</span><span class="sxs-lookup"><span data-stu-id="4dd8d-116">What's new in version 3</span></span>
<span data-ttu-id="4dd8d-117">Version 3 av .NET SDK för Azure Search riktar sig till senast allmänt tillgänglig version av Azure Search-REST-API, särskilt 2016-09-01.</span><span class="sxs-lookup"><span data-stu-id="4dd8d-117">Version 3 of the Azure Search .NET SDK targets the latest generally available version of the Azure Search REST API, specifically 2016-09-01.</span></span> <span data-ttu-id="4dd8d-118">Detta gör det möjligt att använda många nya funktioner i Azure Search från ett .NET-program, inklusive följande:</span><span class="sxs-lookup"><span data-stu-id="4dd8d-118">This makes it possible to use many new features of Azure Search from a .NET application, including the following:</span></span>

* [<span data-ttu-id="4dd8d-119">Anpassade analysverktyg</span><span class="sxs-lookup"><span data-stu-id="4dd8d-119">Custom analyzers</span></span>](https://aka.ms/customanalyzers)
* <span data-ttu-id="4dd8d-120">[Azure Blob Storage](search-howto-indexing-azure-blob-storage.md) och [Azure Table Storage](search-howto-indexing-azure-tables.md) stöd för indexerare</span><span class="sxs-lookup"><span data-stu-id="4dd8d-120">[Azure Blob Storage](search-howto-indexing-azure-blob-storage.md) and [Azure Table Storage](search-howto-indexing-azure-tables.md) indexer support</span></span>
* <span data-ttu-id="4dd8d-121">Indexerare anpassning via [fältet mappningar](search-indexer-field-mappings.md)</span><span class="sxs-lookup"><span data-stu-id="4dd8d-121">Indexer customization via [field mappings](search-indexer-field-mappings.md)</span></span>
* <span data-ttu-id="4dd8d-122">ETags som stöd för att aktivera säker samtidig uppdatering av definitioner av index och indexerare datakällor</span><span class="sxs-lookup"><span data-stu-id="4dd8d-122">ETags support to enable safe concurrent updating of index definitions, indexers, and data sources</span></span>
* <span data-ttu-id="4dd8d-123">Stöd för att skapa index fältdefinitioner deklarativt för pynta modellklass och använda den nya `FieldBuilder` klass.</span><span class="sxs-lookup"><span data-stu-id="4dd8d-123">Support for building index field definitions declaratively by decorating your model class and using the new `FieldBuilder` class.</span></span>
* <span data-ttu-id="4dd8d-124">Stöd för .NET Core och portabla .NET-profil 111</span><span class="sxs-lookup"><span data-stu-id="4dd8d-124">Support for .NET Core and .NET Portable Profile 111</span></span>

<a name="UpgradeSteps"></a>

## <a name="steps-to-upgrade"></a><span data-ttu-id="4dd8d-125">Instruktioner för uppgradering</span><span class="sxs-lookup"><span data-stu-id="4dd8d-125">Steps to upgrade</span></span>
<span data-ttu-id="4dd8d-126">Uppdatera först NuGet-referens för `Microsoft.Azure.Search` NuGet Package Manager-konsolen eller genom att högerklicka på projektreferenserna och välja ”hantera NuGet-paket...” i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="4dd8d-126">First, update your NuGet reference for `Microsoft.Azure.Search` using either the NuGet Package Manager Console or by right-clicking on your project references and selecting "Manage NuGet Packages..." in Visual Studio.</span></span>

<span data-ttu-id="4dd8d-127">När NuGet har laddat ned nya paket och deras beroenden, återskapa projektet.</span><span class="sxs-lookup"><span data-stu-id="4dd8d-127">Once NuGet has downloaded the new packages and their dependencies, rebuild your project.</span></span> <span data-ttu-id="4dd8d-128">Beroende på hur koden är strukturerad, kan den återskapa har.</span><span class="sxs-lookup"><span data-stu-id="4dd8d-128">Depending on how your code is structured, it may rebuild successfully.</span></span> <span data-ttu-id="4dd8d-129">I så fall, är du redo att sätta igång!</span><span class="sxs-lookup"><span data-stu-id="4dd8d-129">If so, you're ready to go!</span></span>

<span data-ttu-id="4dd8d-130">Om din build misslyckas, bör du se ett build-fel som liknar följande:</span><span class="sxs-lookup"><span data-stu-id="4dd8d-130">If your build fails, you should see a build error like the following:</span></span>

    Program.cs(31,45,31,86): error CS0266: Cannot implicitly convert type 'Microsoft.Azure.Search.ISearchIndexClient' to 'Microsoft.Azure.Search.SearchIndexClient'. An explicit conversion exists (are you missing a cast?)

<span data-ttu-id="4dd8d-131">Nästa steg är att korrigera felet build.</span><span class="sxs-lookup"><span data-stu-id="4dd8d-131">The next step is to fix this build error.</span></span> <span data-ttu-id="4dd8d-132">Se [bryta ändringar i version 3](#ListOfChanges) för information om vad som orsakar felet och hur du åtgärdar den.</span><span class="sxs-lookup"><span data-stu-id="4dd8d-132">See [Breaking changes in version 3](#ListOfChanges) for details on what causes the error and how to fix it.</span></span>

<span data-ttu-id="4dd8d-133">Du kan se ytterligare build-varningar som rör föråldrade metoder eller egenskaper.</span><span class="sxs-lookup"><span data-stu-id="4dd8d-133">You may see additional build warnings related to obsolete methods or properties.</span></span> <span data-ttu-id="4dd8d-134">Varningar innehåller information om vad du ska använda i stället för föråldrad funktion.</span><span class="sxs-lookup"><span data-stu-id="4dd8d-134">The warnings will include instructions on what to use instead of the deprecated feature.</span></span> <span data-ttu-id="4dd8d-135">Om programmet använder till exempel den `IndexingParameters.Base64EncodeKeys` egenskapen som du bör få en varning om att`"This property is obsolete. Please create a field mapping using 'FieldMapping.Base64Encode' instead."`</span><span class="sxs-lookup"><span data-stu-id="4dd8d-135">For example, if your application uses the `IndexingParameters.Base64EncodeKeys` property, you should get a warning that says `"This property is obsolete. Please create a field mapping using 'FieldMapping.Base64Encode' instead."`</span></span>

<span data-ttu-id="4dd8d-136">När du har åtgärdat eventuella build-fel, kan du göra ändringar i programmet för att dra nytta av nya funktioner, om du vill.</span><span class="sxs-lookup"><span data-stu-id="4dd8d-136">Once you've fixed any build errors, you can make changes to your application to take advantage of new functionality if you wish.</span></span> <span data-ttu-id="4dd8d-137">Nya funktioner i SDK beskrivs i [vad är nytt i version 3](#WhatsNew).</span><span class="sxs-lookup"><span data-stu-id="4dd8d-137">New features in the SDK are detailed in [What's new in version 3](#WhatsNew).</span></span>

<a name="ListOfChanges"></a>

## <a name="breaking-changes-in-version-3"></a><span data-ttu-id="4dd8d-138">Gör ändringar i version 3</span><span class="sxs-lookup"><span data-stu-id="4dd8d-138">Breaking changes in version 3</span></span>
<span data-ttu-id="4dd8d-139">Ett litet antal bryta ändringar i version 3 som kan kräva kod ändrar det förutom återskapar ditt program.</span><span class="sxs-lookup"><span data-stu-id="4dd8d-139">There a small number of breaking changes in version 3 that may require code changes in addition to rebuilding your application.</span></span>

### <a name="indexesgetclient-return-type"></a><span data-ttu-id="4dd8d-140">Indexes.GetClient returtyp</span><span class="sxs-lookup"><span data-stu-id="4dd8d-140">Indexes.GetClient return type</span></span>
<span data-ttu-id="4dd8d-141">Den `Indexes.GetClient` metoden har en ny returtyp.</span><span class="sxs-lookup"><span data-stu-id="4dd8d-141">The `Indexes.GetClient` method has a new return type.</span></span> <span data-ttu-id="4dd8d-142">Tidigare returnerade `SearchIndexClient`, men detta har ändrats till `ISearchIndexClient` i version 2.0-preview och att ändra följer med till version 3.</span><span class="sxs-lookup"><span data-stu-id="4dd8d-142">Previously, it returned `SearchIndexClient`, but this was changed to `ISearchIndexClient` in version 2.0-preview, and that change carries over to version 3.</span></span> <span data-ttu-id="4dd8d-143">Detta är att stödja kunder som vill mock den `GetClient` metod för kontroller genom att returnera en fingerad implementering av `ISearchIndexClient`.</span><span class="sxs-lookup"><span data-stu-id="4dd8d-143">This is to support customers that wish to mock the `GetClient` method for unit tests by returning a mock implementation of `ISearchIndexClient`.</span></span>

#### <a name="example"></a><span data-ttu-id="4dd8d-144">Exempel</span><span class="sxs-lookup"><span data-stu-id="4dd8d-144">Example</span></span>
<span data-ttu-id="4dd8d-145">Om din kod ser ut så här:</span><span class="sxs-lookup"><span data-stu-id="4dd8d-145">If your code looks like this:</span></span>

```csharp
SearchIndexClient indexClient = serviceClient.Indexes.GetClient("hotels");
```

<span data-ttu-id="4dd8d-146">Du kan ändra den till det rätta till eventuella build-fel:</span><span class="sxs-lookup"><span data-stu-id="4dd8d-146">You can change it to this to fix any build errors:</span></span>

```csharp
ISearchIndexClient indexClient = serviceClient.Indexes.GetClient("hotels");
```

### <a name="analyzername-datatype-and-others-are-no-longer-implicitly-convertible-to-strings"></a><span data-ttu-id="4dd8d-147">AnalyzerName och datatyp är inte längre implicit omvandlas till strängar</span><span class="sxs-lookup"><span data-stu-id="4dd8d-147">AnalyzerName, DataType, and others are no longer implicitly convertible to strings</span></span>
<span data-ttu-id="4dd8d-148">Det finns många typer i Azure Search .NET SDK som härleds från `ExtensibleEnum`.</span><span class="sxs-lookup"><span data-stu-id="4dd8d-148">There are many types in the Azure Search .NET SDK that derive from `ExtensibleEnum`.</span></span> <span data-ttu-id="4dd8d-149">Tidigare var dessa typer av alla implicit omvandlas till typen `string`.</span><span class="sxs-lookup"><span data-stu-id="4dd8d-149">Previously these types were all implicitly convertible to type `string`.</span></span> <span data-ttu-id="4dd8d-150">Men ett fel upptäcktes i den `Object.Equals` implementering för dessa klasser och åtgärda felet krävs om du inaktiverar den här implicit konvertering.</span><span class="sxs-lookup"><span data-stu-id="4dd8d-150">However, a bug was discovered in the `Object.Equals` implementation for these classes, and fixing the bug required disabling this implicit conversion.</span></span> <span data-ttu-id="4dd8d-151">Explicit konvertering till `string` fortfarande är tillåtet.</span><span class="sxs-lookup"><span data-stu-id="4dd8d-151">Explicit conversion to `string` is still allowed.</span></span>

#### <a name="example"></a><span data-ttu-id="4dd8d-152">Exempel</span><span class="sxs-lookup"><span data-stu-id="4dd8d-152">Example</span></span>
<span data-ttu-id="4dd8d-153">Om din kod ser ut så här:</span><span class="sxs-lookup"><span data-stu-id="4dd8d-153">If your code looks like this:</span></span>

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

<span data-ttu-id="4dd8d-154">Du kan ändra den till det rätta till eventuella build-fel:</span><span class="sxs-lookup"><span data-stu-id="4dd8d-154">You can change it to this to fix any build errors:</span></span>

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

### <a name="removed-obsolete-members"></a><span data-ttu-id="4dd8d-155">Ta bort föråldrade medlemmar</span><span class="sxs-lookup"><span data-stu-id="4dd8d-155">Removed obsolete members</span></span>

<span data-ttu-id="4dd8d-156">Du kan visa build-fel relaterade till metoder eller egenskaper som har markerats som föråldrade i version 2.0-förhandsgranskning och därefter tas bort i version 3.</span><span class="sxs-lookup"><span data-stu-id="4dd8d-156">You may see build errors related to methods or properties that were marked as obsolete in version 2.0-preview and subsequently removed in version 3.</span></span> <span data-ttu-id="4dd8d-157">Om du stöter på dessa fel, är här hur du löser dem:</span><span class="sxs-lookup"><span data-stu-id="4dd8d-157">If you encounter such errors, here is how to resolve them:</span></span>

- <span data-ttu-id="4dd8d-158">Om du använder den här konstruktorn: `ScoringParameter(string name, string value)`, Använd den här i stället:`ScoringParameter(string name, IEnumerable<string> values)`</span><span class="sxs-lookup"><span data-stu-id="4dd8d-158">If you were using this constructor: `ScoringParameter(string name, string value)`, use this one instead: `ScoringParameter(string name, IEnumerable<string> values)`</span></span>
- <span data-ttu-id="4dd8d-159">Om du använder den `ScoringParameter.Value` anger den `ScoringParameter.Values` egenskapen eller `ToString` metod i stället.</span><span class="sxs-lookup"><span data-stu-id="4dd8d-159">If you were using the `ScoringParameter.Value` property, use the `ScoringParameter.Values` property or the `ToString` method instead.</span></span>
- <span data-ttu-id="4dd8d-160">Om du använder den `SearchRequestOptions.RequestId` anger den `ClientRequestId` egenskapen i stället.</span><span class="sxs-lookup"><span data-stu-id="4dd8d-160">If you were using the `SearchRequestOptions.RequestId` property, use the `ClientRequestId` property instead.</span></span>

### <a name="removed-preview-features"></a><span data-ttu-id="4dd8d-161">Borttagna förhandsgranskningsfunktioner</span><span class="sxs-lookup"><span data-stu-id="4dd8d-161">Removed preview features</span></span>

<span data-ttu-id="4dd8d-162">Om du uppgraderar från version 2.0-preview till version 3, Tänk på att JSON och CSV parsning stöd för Blob indexerare har tagits bort eftersom dessa funktioner är fortfarande under förhandsgranskning.</span><span class="sxs-lookup"><span data-stu-id="4dd8d-162">If you are upgrading from version 2.0-preview to version 3, be aware that JSON and CSV parsing support for Blob Indexers has been removed since these features are still in preview.</span></span> <span data-ttu-id="4dd8d-163">Mer specifikt kan följande metoder för den `IndexingParametersExtensions` klass har tagits bort:</span><span class="sxs-lookup"><span data-stu-id="4dd8d-163">Specifically, the following methods of the `IndexingParametersExtensions` class have been removed:</span></span>

- `ParseJson`
- `ParseJsonArrays`
- `ParseDelimitedTextFiles`

<span data-ttu-id="4dd8d-164">Om programmet har en fast beroende på de här funktionerna, kan du inte uppgradera till version 3 av Azure Search .NET SDK.</span><span class="sxs-lookup"><span data-stu-id="4dd8d-164">If your application has a hard dependency on these features, you will not be able to upgrade to version 3 of the Azure Search .NET SDK.</span></span> <span data-ttu-id="4dd8d-165">Du kan fortsätta att använda version 2.0-preview.</span><span class="sxs-lookup"><span data-stu-id="4dd8d-165">You can continue to use version 2.0-preview.</span></span> <span data-ttu-id="4dd8d-166">Men du tänka på att **vi rekommenderar inte att använda Förhandsgranska SDK: er i produktionsprogram**.</span><span class="sxs-lookup"><span data-stu-id="4dd8d-166">However, please keep in mind that **we do not recommend using preview SDKs in production applications**.</span></span> <span data-ttu-id="4dd8d-167">Förhandsgranskningsfunktioner gäller enbart och kan ändras.</span><span class="sxs-lookup"><span data-stu-id="4dd8d-167">Preview features are for evaluation only and may change.</span></span>

## <a name="conclusion"></a><span data-ttu-id="4dd8d-168">Slutsats</span><span class="sxs-lookup"><span data-stu-id="4dd8d-168">Conclusion</span></span>
<span data-ttu-id="4dd8d-169">Om du vill ha mer information om hur du använder Azure Search .NET SDK finns i vår nyligen uppdaterat [anvisningar](search-howto-dotnet-sdk.md).</span><span class="sxs-lookup"><span data-stu-id="4dd8d-169">If you need more details on using the Azure Search .NET SDK, see our recently updated [How-to](search-howto-dotnet-sdk.md).</span></span>

<span data-ttu-id="4dd8d-170">Vi uppskattar din feedback om SDK.</span><span class="sxs-lookup"><span data-stu-id="4dd8d-170">We welcome your feedback on the SDK.</span></span> <span data-ttu-id="4dd8d-171">Om du får problem passa på att be om hjälp oss på den [Azure Search MSDN-forum](https://social.msdn.microsoft.com/Forums/azure/home?forum=azuresearch).</span><span class="sxs-lookup"><span data-stu-id="4dd8d-171">If you encounter problems, feel free to ask us for help on the [Azure Search MSDN forum](https://social.msdn.microsoft.com/Forums/azure/home?forum=azuresearch).</span></span> <span data-ttu-id="4dd8d-172">Om du hittar ett programfel kan du filen ett problem i den [Azure .NET SDK GitHub-lagringsplatsen](https://github.com/Azure/azure-sdk-for-net/issues).</span><span class="sxs-lookup"><span data-stu-id="4dd8d-172">If you find a bug, you can file an issue in the [Azure .NET SDK GitHub repository](https://github.com/Azure/azure-sdk-for-net/issues).</span></span> <span data-ttu-id="4dd8d-173">Se till att prefixet problemet med rubriken ”Sök SDK”:.</span><span class="sxs-lookup"><span data-stu-id="4dd8d-173">Make sure to prefix your issue title with "Search SDK: ".</span></span>

<span data-ttu-id="4dd8d-174">Tack för att använda Azure Search!</span><span class="sxs-lookup"><span data-stu-id="4dd8d-174">Thank you for using Azure Search!</span></span>

<a name="UpgradeStepsV1"></a>

## <a name="appendix-steps-to-upgrade-to-version-11"></a><span data-ttu-id="4dd8d-175">Bilaga: Steg för att uppgradera till version 1.1</span><span class="sxs-lookup"><span data-stu-id="4dd8d-175">Appendix: Steps to upgrade to version 1.1</span></span>
> [!NOTE]
> <span data-ttu-id="4dd8d-176">Det här avsnittet gäller enbart för användare i Azure Search .NET SDK version 1.0.2-preview och äldre.</span><span class="sxs-lookup"><span data-stu-id="4dd8d-176">This section applies only to users of the Azure Search .NET SDK version 1.0.2-preview and older.</span></span>
> 
> 

<span data-ttu-id="4dd8d-177">Uppdatera först NuGet-referens för `Microsoft.Azure.Search` NuGet Package Manager-konsolen eller genom att högerklicka på projektreferenserna och välja ”hantera NuGet-paket...” i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="4dd8d-177">First, update your NuGet reference for `Microsoft.Azure.Search` using either the NuGet Package Manager Console or by right-clicking on your project references and selecting "Manage NuGet Packages..." in Visual Studio.</span></span>

<span data-ttu-id="4dd8d-178">När NuGet har laddat ned nya paket och deras beroenden, återskapa projektet.</span><span class="sxs-lookup"><span data-stu-id="4dd8d-178">Once NuGet has downloaded the new packages and their dependencies, rebuild your project.</span></span>

<span data-ttu-id="4dd8d-179">Om du använde tidigare version 1.0.0-preview, 1.0.1-preview eller 1.0.2-preview, bygga ska lyckas och du är redo att sätta igång!</span><span class="sxs-lookup"><span data-stu-id="4dd8d-179">If you were previously using version 1.0.0-preview, 1.0.1-preview, or 1.0.2-preview, the build should succeed and you're ready to go!</span></span>

<span data-ttu-id="4dd8d-180">Om du använde tidigare version 0.13.0-preview eller äldre, bör du se Skapa fel på följande:</span><span class="sxs-lookup"><span data-stu-id="4dd8d-180">If you were previously using version 0.13.0-preview or older, you should see build errors like the following:</span></span>

    Program.cs(137,56,137,62): error CS0117: 'Microsoft.Azure.Search.Models.IndexBatch' does not contain a definition for 'Create'
    Program.cs(137,99,137,105): error CS0117: 'Microsoft.Azure.Search.Models.IndexAction' does not contain a definition for 'Create'
    Program.cs(146,41,146,54): error CS1061: 'Microsoft.Azure.Search.IndexBatchException' does not contain a definition for 'IndexResponse' and no extension method 'IndexResponse' accepting a first argument of type 'Microsoft.Azure.Search.IndexBatchException' could be found (are you missing a using directive or an assembly reference?)
    Program.cs(163,13,163,42): error CS0246: The type or namespace name 'DocumentSearchResponse' could not be found (are you missing a using directive or an assembly reference?)

<span data-ttu-id="4dd8d-181">Nästa steg är att åtgärda build-fel i taget.</span><span class="sxs-lookup"><span data-stu-id="4dd8d-181">The next step is to fix the build errors one by one.</span></span> <span data-ttu-id="4dd8d-182">De flesta kräver att ändra vissa klass- och metoden namn som har bytt i SDK.</span><span class="sxs-lookup"><span data-stu-id="4dd8d-182">Most will require changing some class and method names that have been renamed in the SDK.</span></span> <span data-ttu-id="4dd8d-183">[Lista över bryta ändringar i version 1.1](#ListOfChangesV1) innehåller en lista över ändringarna namn.</span><span class="sxs-lookup"><span data-stu-id="4dd8d-183">[List of breaking changes in version 1.1](#ListOfChangesV1) contains a list of these name changes.</span></span>

<span data-ttu-id="4dd8d-184">Om du använder egna klasser för att modellera dokumenten, och dessa klasser har egenskaper av icke-nullbar primitiva typer (till exempel `int` eller `bool` i C#), det finns en felkorrigering i 1.1-versionen av SDK som bör du vara medveten om.</span><span class="sxs-lookup"><span data-stu-id="4dd8d-184">If you're using custom classes to model your documents, and those classes have properties of non-nullable primitive types (for example, `int` or `bool` in C#), there is a bug fix in the 1.1 version of the SDK of which you should be aware.</span></span> <span data-ttu-id="4dd8d-185">Se [felkorrigeringar i version 1.1](#BugFixesV1) mer information.</span><span class="sxs-lookup"><span data-stu-id="4dd8d-185">See [Bug fixes in version 1.1](#BugFixesV1) for more details.</span></span>

<span data-ttu-id="4dd8d-186">Slutligen när du har åtgärdat eventuella build-fel, kan du göra ändringar i programmet för att dra nytta av nya funktioner, om du vill.</span><span class="sxs-lookup"><span data-stu-id="4dd8d-186">Finally, once you've fixed any build errors, you can make changes to your application to take advantage of new functionality if you wish.</span></span>

<a name="ListOfChangesV1"></a>

### <a name="list-of-breaking-changes-in-version-11"></a><span data-ttu-id="4dd8d-187">Lista över bryta ändringar i version 1.1</span><span class="sxs-lookup"><span data-stu-id="4dd8d-187">List of breaking changes in version 1.1</span></span>
<span data-ttu-id="4dd8d-188">I följande lista är sorterade efter sannolikheten att påverkas av programkoden.</span><span class="sxs-lookup"><span data-stu-id="4dd8d-188">The following list is ordered by the likelihood that the change will affect your application code.</span></span>

#### <a name="indexbatch-and-indexaction-changes"></a><span data-ttu-id="4dd8d-189">IndexBatch och IndexAction ändringar</span><span class="sxs-lookup"><span data-stu-id="4dd8d-189">IndexBatch and IndexAction changes</span></span>
<span data-ttu-id="4dd8d-190">`IndexBatch.Create`har bytt namn till `IndexBatch.New` och har inte längre en `params` argumentet.</span><span class="sxs-lookup"><span data-stu-id="4dd8d-190">`IndexBatch.Create` has been renamed to `IndexBatch.New` and no longer has a `params` argument.</span></span> <span data-ttu-id="4dd8d-191">Du kan använda `IndexBatch.New` för batchar som blanda olika typer av åtgärder (sammanslagningar, borttagningar osv.).</span><span class="sxs-lookup"><span data-stu-id="4dd8d-191">You can use `IndexBatch.New` for batches that mix different types of actions (merges, deletes, etc.).</span></span> <span data-ttu-id="4dd8d-192">Dessutom finns nya statiska metoder för att skapa batchar där alla åtgärder är samma: `Delete`, `Merge`, `MergeOrUpload`, och `Upload`.</span><span class="sxs-lookup"><span data-stu-id="4dd8d-192">In addition, there are new static methods for creating batches where all the actions are the same: `Delete`, `Merge`, `MergeOrUpload`, and `Upload`.</span></span>

<span data-ttu-id="4dd8d-193">`IndexAction`Offentliga konstruktorer har inte längre och dess egenskaper är nu inte ändras.</span><span class="sxs-lookup"><span data-stu-id="4dd8d-193">`IndexAction` no longer has public constructors and its properties are now immutable.</span></span> <span data-ttu-id="4dd8d-194">Du bör använda de nya statiska metoderna för att skapa åtgärder för olika ändamål: `Delete`, `Merge`, `MergeOrUpload`, och `Upload`.</span><span class="sxs-lookup"><span data-stu-id="4dd8d-194">You should use the new static methods for creating actions for different purposes: `Delete`, `Merge`, `MergeOrUpload`, and `Upload`.</span></span> <span data-ttu-id="4dd8d-195">`IndexAction.Create`har tagits bort.</span><span class="sxs-lookup"><span data-stu-id="4dd8d-195">`IndexAction.Create` has been removed.</span></span> <span data-ttu-id="4dd8d-196">Om du använde överlagring som tar endast ett dokument, se till att använda `Upload` i stället.</span><span class="sxs-lookup"><span data-stu-id="4dd8d-196">If you used the overload that takes only a document, make sure to use `Upload` instead.</span></span>

##### <a name="example"></a><span data-ttu-id="4dd8d-197">Exempel</span><span class="sxs-lookup"><span data-stu-id="4dd8d-197">Example</span></span>
<span data-ttu-id="4dd8d-198">Om din kod ser ut så här:</span><span class="sxs-lookup"><span data-stu-id="4dd8d-198">If your code looks like this:</span></span>

    var batch = IndexBatch.Create(documents.Select(doc => IndexAction.Create(doc)));
    indexClient.Documents.Index(batch);

<span data-ttu-id="4dd8d-199">Du kan ändra den till det rätta till eventuella build-fel:</span><span class="sxs-lookup"><span data-stu-id="4dd8d-199">You can change it to this to fix any build errors:</span></span>

    var batch = IndexBatch.New(documents.Select(doc => IndexAction.Upload(doc)));
    indexClient.Documents.Index(batch);

<span data-ttu-id="4dd8d-200">Om du vill kan förenklar du ytterligare det till den här:</span><span class="sxs-lookup"><span data-stu-id="4dd8d-200">If you want, you can further simplify it to this:</span></span>

    var batch = IndexBatch.Upload(documents);
    indexClient.Documents.Index(batch);

#### <a name="indexbatchexception-changes"></a><span data-ttu-id="4dd8d-201">IndexBatchException ändringar</span><span class="sxs-lookup"><span data-stu-id="4dd8d-201">IndexBatchException changes</span></span>
<span data-ttu-id="4dd8d-202">Den `IndexBatchException.IndexResponse` egenskapen har bytt namn till `IndexingResults`, och dess typ är nu `IList<IndexingResult>`.</span><span class="sxs-lookup"><span data-stu-id="4dd8d-202">The `IndexBatchException.IndexResponse` property has been renamed to `IndexingResults`, and its type is now `IList<IndexingResult>`.</span></span>

##### <a name="example"></a><span data-ttu-id="4dd8d-203">Exempel</span><span class="sxs-lookup"><span data-stu-id="4dd8d-203">Example</span></span>
<span data-ttu-id="4dd8d-204">Om din kod ser ut så här:</span><span class="sxs-lookup"><span data-stu-id="4dd8d-204">If your code looks like this:</span></span>

    catch (IndexBatchException e)
    {
        Console.WriteLine(
            "Failed to index some of the documents: {0}",
            String.Join(", ", e.IndexResponse.Results.Where(r => !r.Succeeded).Select(r => r.Key)));
    }

<span data-ttu-id="4dd8d-205">Du kan ändra den till det rätta till eventuella build-fel:</span><span class="sxs-lookup"><span data-stu-id="4dd8d-205">You can change it to this to fix any build errors:</span></span>

    catch (IndexBatchException e)
    {
        Console.WriteLine(
            "Failed to index some of the documents: {0}",
            String.Join(", ", e.IndexingResults.Where(r => !r.Succeeded).Select(r => r.Key)));
    }

<a name="OperationMethodChanges"></a>

#### <a name="operation-method-changes"></a><span data-ttu-id="4dd8d-206">Åtgärden metoden ändringar</span><span class="sxs-lookup"><span data-stu-id="4dd8d-206">Operation method changes</span></span>
<span data-ttu-id="4dd8d-207">Varje åtgärd i Azure Search .NET SDK visas som en uppsättning metoden överlagringar för synkrona och asynkrona anropare.</span><span class="sxs-lookup"><span data-stu-id="4dd8d-207">Each operation in the Azure Search .NET SDK is exposed as a set of method overloads for synchronous and asynchronous callers.</span></span> <span data-ttu-id="4dd8d-208">Signaturerna och factoring av dessa metoden överlagringar har ändrats i version 1.1.</span><span class="sxs-lookup"><span data-stu-id="4dd8d-208">The signatures and factoring of these method overloads has changed in version 1.1.</span></span>

<span data-ttu-id="4dd8d-209">Till exempel visas ”hämta indexstatistik”-åtgärd i äldre versioner av SDK dessa signaturer:</span><span class="sxs-lookup"><span data-stu-id="4dd8d-209">For example, the "Get Index Statistics" operation in older versions of the SDK exposed these signatures:</span></span>

<span data-ttu-id="4dd8d-210">I `IIndexOperations`:</span><span class="sxs-lookup"><span data-stu-id="4dd8d-210">In `IIndexOperations`:</span></span>

    // Asynchronous operation with all parameters
    Task<IndexGetStatisticsResponse> GetStatisticsAsync(
        string indexName,
        CancellationToken cancellationToken);

<span data-ttu-id="4dd8d-211">I `IndexOperationsExtensions`:</span><span class="sxs-lookup"><span data-stu-id="4dd8d-211">In `IndexOperationsExtensions`:</span></span>

    // Asynchronous operation with only required parameters
    public static Task<IndexGetStatisticsResponse> GetStatisticsAsync(
        this IIndexOperations operations,
        string indexName);

    // Synchronous operation with only required parameters
    public static IndexGetStatisticsResponse GetStatistics(
        this IIndexOperations operations,
        string indexName);

<span data-ttu-id="4dd8d-212">Metoden signaturer för samma åtgärd i version 1.1 se ut så här:</span><span class="sxs-lookup"><span data-stu-id="4dd8d-212">The method signatures for the same operation in version 1.1 look like this:</span></span>

<span data-ttu-id="4dd8d-213">I `IIndexesOperations`:</span><span class="sxs-lookup"><span data-stu-id="4dd8d-213">In `IIndexesOperations`:</span></span>

    // Asynchronous operation with lower-level HTTP features exposed
    Task<AzureOperationResponse<IndexGetStatisticsResult>> GetStatisticsWithHttpMessagesAsync(
        string indexName,
        SearchRequestOptions searchRequestOptions = default(SearchRequestOptions),
        Dictionary<string, List<string>> customHeaders = null,
        CancellationToken cancellationToken = default(CancellationToken));

<span data-ttu-id="4dd8d-214">I `IndexesOperationsExtensions`:</span><span class="sxs-lookup"><span data-stu-id="4dd8d-214">In `IndexesOperationsExtensions`:</span></span>

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

<span data-ttu-id="4dd8d-215">Från och med version 1.1, ordnar .NET SDK för Azure Search åtgärden metoder på olika sätt:</span><span class="sxs-lookup"><span data-stu-id="4dd8d-215">Starting with version 1.1, the Azure Search .NET SDK organizes operation methods differently:</span></span>

* <span data-ttu-id="4dd8d-216">Valfria parametrar finns nu modelleras som standard parametrar snarare än ytterligare en metod överlagringar.</span><span class="sxs-lookup"><span data-stu-id="4dd8d-216">Optional parameters are now modeled as default parameters rather than additional method overloads.</span></span> <span data-ttu-id="4dd8d-217">Detta minskar antalet metoden överlagringar ibland dramatiskt.</span><span class="sxs-lookup"><span data-stu-id="4dd8d-217">This reduces the number of method overloads, sometimes dramatically.</span></span>
* <span data-ttu-id="4dd8d-218">Tilläggsmetoder dölja nu mycket extra information om HTTP från anroparen.</span><span class="sxs-lookup"><span data-stu-id="4dd8d-218">The extension methods now hide a lot of the extraneous details of HTTP from the caller.</span></span> <span data-ttu-id="4dd8d-219">Till exempel äldre versioner av SDK returnerade ett svarsobjekt med en HTTP-statuskod som ofta inte behöver du kontrollera eftersom åtgärden metoder som resulterar i `CloudException` för varje statuskod som indikerar ett fel.</span><span class="sxs-lookup"><span data-stu-id="4dd8d-219">For example, older versions of the SDK returned a response object with an HTTP status code, which you often didn't need to check because operation methods throw `CloudException` for any status code that indicates an error.</span></span> <span data-ttu-id="4dd8d-220">De nya metoderna för tillägget returnera bara modellobjekt, vilket sparar dig att packa upp dem i din kod.</span><span class="sxs-lookup"><span data-stu-id="4dd8d-220">The new extension methods just return model objects, saving you the trouble of having to unwrap them in your code.</span></span>
* <span data-ttu-id="4dd8d-221">Grundläggande gränssnitt däremot nu exponera metoder som ger dig större kontroll på HTTP-nivå om det behövs.</span><span class="sxs-lookup"><span data-stu-id="4dd8d-221">Conversely, the core interfaces now expose methods that give you more control at the HTTP level if you need it.</span></span> <span data-ttu-id="4dd8d-222">Du kan nu skicka in egna HTTP-rubriker som ska ingå i begäranden och den nya `AzureOperationResponse<T>` returtyp ger direktåtkomst till det `HttpRequestMessage` och `HttpResponseMessage` för åtgärden.</span><span class="sxs-lookup"><span data-stu-id="4dd8d-222">You can now pass in custom HTTP headers to be included in requests, and the new `AzureOperationResponse<T>` return type gives you direct access to the `HttpRequestMessage` and `HttpResponseMessage` for the operation.</span></span> <span data-ttu-id="4dd8d-223">`AzureOperationResponse`har definierats i den `Microsoft.Rest.Azure` namnområde och ersätter `Hyak.Common.OperationResponse`.</span><span class="sxs-lookup"><span data-stu-id="4dd8d-223">`AzureOperationResponse` is defined in the `Microsoft.Rest.Azure` namespace and replaces `Hyak.Common.OperationResponse`.</span></span>

#### <a name="scoringparameters-changes"></a><span data-ttu-id="4dd8d-224">ScoringParameters ändringar</span><span class="sxs-lookup"><span data-stu-id="4dd8d-224">ScoringParameters changes</span></span>
<span data-ttu-id="4dd8d-225">En ny klass med namnet `ScoringParameter` har lagts till i den senaste SDK: N gör det lättare att ange parametrar för att poängberäkningen profiler i en sökfråga.</span><span class="sxs-lookup"><span data-stu-id="4dd8d-225">A new class named `ScoringParameter` has been added in the latest SDK to make it easier to provide parameters to scoring profiles in a search query.</span></span> <span data-ttu-id="4dd8d-226">Tidigare den `ScoringProfiles` -egenskapen för den `SearchParameters` klassen angavs som `IList<string>`; Nu det skrivs som `IList<ScoringParameter>`.</span><span class="sxs-lookup"><span data-stu-id="4dd8d-226">Previously the `ScoringProfiles` property of the `SearchParameters` class was typed as `IList<string>`; Now it is typed as `IList<ScoringParameter>`.</span></span>

##### <a name="example"></a><span data-ttu-id="4dd8d-227">Exempel</span><span class="sxs-lookup"><span data-stu-id="4dd8d-227">Example</span></span>
<span data-ttu-id="4dd8d-228">Om din kod ser ut så här:</span><span class="sxs-lookup"><span data-stu-id="4dd8d-228">If your code looks like this:</span></span>

    var sp = new SearchParameters();
    sp.ScoringProfile = "jobsScoringFeatured";      // Use a scoring profile
    sp.ScoringParameters = new[] { "featuredParam-featured", "mapCenterParam-" + lon + "," + lat };

<span data-ttu-id="4dd8d-229">Du kan ändra den till det rätta till eventuella build-fel:</span><span class="sxs-lookup"><span data-stu-id="4dd8d-229">You can change it to this to fix any build errors:</span></span> 

    var sp = new SearchParameters();
    sp.ScoringProfile = "jobsScoringFeatured";      // Use a scoring profile
    sp.ScoringParameters =
        new[]
        {
            new ScoringParameter("featuredParam", new[] { "featured" }),
            new ScoringParameter("mapCenterParam", GeographyPoint.Create(lat, lon))
        };

#### <a name="model-class-changes"></a><span data-ttu-id="4dd8d-230">Klassen Modelländringar</span><span class="sxs-lookup"><span data-stu-id="4dd8d-230">Model class changes</span></span>
<span data-ttu-id="4dd8d-231">På grund av signatur-ändringar som beskrivs i [igen metoden ändringar](#OperationMethodChanges), många klasser i den `Microsoft.Azure.Search.Models` namnområde har ändrats eller tagits bort.</span><span class="sxs-lookup"><span data-stu-id="4dd8d-231">Due to the signature changes described in [Operation method changes](#OperationMethodChanges), many classes in the `Microsoft.Azure.Search.Models` namespace have been renamed or removed.</span></span> <span data-ttu-id="4dd8d-232">Exempel:</span><span class="sxs-lookup"><span data-stu-id="4dd8d-232">For example:</span></span>

* <span data-ttu-id="4dd8d-233">`IndexDefinitionResponse`har ersatts av`AzureOperationResponse<Index>`</span><span class="sxs-lookup"><span data-stu-id="4dd8d-233">`IndexDefinitionResponse` has been replaced by `AzureOperationResponse<Index>`</span></span>
* <span data-ttu-id="4dd8d-234">`DocumentSearchResponse` har bytt namn till `DocumentSearchResult`</span><span class="sxs-lookup"><span data-stu-id="4dd8d-234">`DocumentSearchResponse` has been renamed to `DocumentSearchResult`</span></span>
* <span data-ttu-id="4dd8d-235">`IndexResult` har bytt namn till `IndexingResult`</span><span class="sxs-lookup"><span data-stu-id="4dd8d-235">`IndexResult` has been renamed to `IndexingResult`</span></span>
* <span data-ttu-id="4dd8d-236">`Documents.Count()`Returnerar nu en `long` med dokumentantal i stället för en`DocumentCountResponse`</span><span class="sxs-lookup"><span data-stu-id="4dd8d-236">`Documents.Count()` now returns a `long` with the document count instead of a `DocumentCountResponse`</span></span>
* <span data-ttu-id="4dd8d-237">`IndexGetStatisticsResponse` har bytt namn till `IndexGetStatisticsResult`</span><span class="sxs-lookup"><span data-stu-id="4dd8d-237">`IndexGetStatisticsResponse` has been renamed to `IndexGetStatisticsResult`</span></span>
* <span data-ttu-id="4dd8d-238">`IndexListResponse` har bytt namn till `IndexListResult`</span><span class="sxs-lookup"><span data-stu-id="4dd8d-238">`IndexListResponse` has been renamed to `IndexListResult`</span></span>

<span data-ttu-id="4dd8d-239">Sammanfattningsvis `OperationResponse`-härledda klasser som fanns endast för att omsluta en model-objektet har tagits bort.</span><span class="sxs-lookup"><span data-stu-id="4dd8d-239">To summarize, `OperationResponse`-derived classes that existed only to wrap a model object have been removed.</span></span> <span data-ttu-id="4dd8d-240">De återstående klasserna har fått sina suffix har ändrats från `Response` till `Result`.</span><span class="sxs-lookup"><span data-stu-id="4dd8d-240">The remaining classes have had their suffix changed from `Response` to `Result`.</span></span>

##### <a name="example"></a><span data-ttu-id="4dd8d-241">Exempel</span><span class="sxs-lookup"><span data-stu-id="4dd8d-241">Example</span></span>
<span data-ttu-id="4dd8d-242">Om din kod ser ut så här:</span><span class="sxs-lookup"><span data-stu-id="4dd8d-242">If your code looks like this:</span></span>

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

<span data-ttu-id="4dd8d-243">Du kan ändra den till det rätta till eventuella build-fel:</span><span class="sxs-lookup"><span data-stu-id="4dd8d-243">You can change it to this to fix any build errors:</span></span>

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

##### <a name="response-classes-and-ienumerable"></a><span data-ttu-id="4dd8d-244">Svaret klasser och IEnumerable</span><span class="sxs-lookup"><span data-stu-id="4dd8d-244">Response classes and IEnumerable</span></span>
<span data-ttu-id="4dd8d-245">En ytterligare ändring som kan påverka din kod är att implementera svar klasser som innehåller samlingar längre `IEnumerable<T>`.</span><span class="sxs-lookup"><span data-stu-id="4dd8d-245">An additional change that may affect your code is that response classes that hold collections no longer implement `IEnumerable<T>`.</span></span> <span data-ttu-id="4dd8d-246">Du kan i stället öppna samlingsegenskapen direkt.</span><span class="sxs-lookup"><span data-stu-id="4dd8d-246">Instead, you can access the collection property directly.</span></span> <span data-ttu-id="4dd8d-247">Om exempelvis koden ser ut så här:</span><span class="sxs-lookup"><span data-stu-id="4dd8d-247">For example, if your code looks like this:</span></span>

    DocumentSearchResponse<Hotel> response = indexClient.Documents.Search<Hotel>(searchText, sp);
    foreach (SearchResult<Hotel> result in response)
    {
        Console.WriteLine(result.Document);
    }

<span data-ttu-id="4dd8d-248">Du kan ändra den till det rätta till eventuella build-fel:</span><span class="sxs-lookup"><span data-stu-id="4dd8d-248">You can change it to this to fix any build errors:</span></span>

    DocumentSearchResult<Hotel> response = indexClient.Documents.Search<Hotel>(searchText, sp);
    foreach (SearchResult<Hotel> result in response.Results)
    {
        Console.WriteLine(result.Document);
    }

##### <a name="special-case-for-web-applications"></a><span data-ttu-id="4dd8d-249">Specialfall för webbprogram</span><span class="sxs-lookup"><span data-stu-id="4dd8d-249">Special case for web applications</span></span>
<span data-ttu-id="4dd8d-250">Om du har ett webbprogram som Serialiserar `DocumentSearchResponse` direkt om du vill skicka sökresultaten till webbläsaren, behöver du ändra koden eller resultatet kommer inte att serialisera korrekt.</span><span class="sxs-lookup"><span data-stu-id="4dd8d-250">If you have a web application that serializes `DocumentSearchResponse` directly to send search results to the browser, you will need to change your code or the results will not serialize correctly.</span></span> <span data-ttu-id="4dd8d-251">Om exempelvis koden ser ut så här:</span><span class="sxs-lookup"><span data-stu-id="4dd8d-251">For example, if your code looks like this:</span></span>

    public ActionResult Search(string q = "")
    {
        // If blank search, assume they want to search everything
        if (string.IsNullOrWhiteSpace(q))
            q = "*";

        return new JsonResult
        {
            JsonRequestBehavior = JsonRequestBehavior.AllowGet,
            Data = _featuresSearch.Search(q)
        };
    }

<span data-ttu-id="4dd8d-252">Du kan ändra den genom att hämta den `.Results` egenskapen för search-svar för att åtgärda Sök resultatet återgivning:</span><span class="sxs-lookup"><span data-stu-id="4dd8d-252">You can change it by getting the `.Results` property of the search response to fix search result rendering:</span></span>

    public ActionResult Search(string q = "")
    {
        // If blank search, assume they want to search everything
        if (string.IsNullOrWhiteSpace(q))
            q = "*";

        return new JsonResult
        {
            JsonRequestBehavior = JsonRequestBehavior.AllowGet,
            Data = _featuresSearch.Search(q).Results
        };
    }

<span data-ttu-id="4dd8d-253">Du behöver titta i sådana fall i koden själv. **Kompileraren varnar dig inte** eftersom `JsonResult.Data` är av typen `object`.</span><span class="sxs-lookup"><span data-stu-id="4dd8d-253">You will have to look for such cases in your code yourself; **The compiler will not warn you** because `JsonResult.Data` is of type `object`.</span></span>

#### <a name="cloudexception-changes"></a><span data-ttu-id="4dd8d-254">CloudException ändringar</span><span class="sxs-lookup"><span data-stu-id="4dd8d-254">CloudException changes</span></span>
<span data-ttu-id="4dd8d-255">Den `CloudException` klass har flyttats från den `Hyak.Common` namnområde och den `Microsoft.Rest.Azure` namnområde.</span><span class="sxs-lookup"><span data-stu-id="4dd8d-255">The `CloudException` class has moved from the `Hyak.Common` namespace to the `Microsoft.Rest.Azure` namespace.</span></span> <span data-ttu-id="4dd8d-256">Dessutom dess `Error` egenskapen har bytt namn till `Body`.</span><span class="sxs-lookup"><span data-stu-id="4dd8d-256">Also, its `Error` property has been renamed to `Body`.</span></span>

#### <a name="searchserviceclient-and-searchindexclient-changes"></a><span data-ttu-id="4dd8d-257">SearchServiceClient och SearchIndexClient ändringar</span><span class="sxs-lookup"><span data-stu-id="4dd8d-257">SearchServiceClient and SearchIndexClient changes</span></span>
<span data-ttu-id="4dd8d-258">Typ av den `Credentials` egenskap har ändrats från `SearchCredentials` till dess basklass `ServiceClientCredentials`.</span><span class="sxs-lookup"><span data-stu-id="4dd8d-258">The type of the `Credentials` property has changed from `SearchCredentials` to its base class, `ServiceClientCredentials`.</span></span> <span data-ttu-id="4dd8d-259">Om du behöver åtkomst till den `SearchCredentials` av en `SearchIndexClient` eller `SearchServiceClient`, Använd den nya `SearchCredentials` egenskapen.</span><span class="sxs-lookup"><span data-stu-id="4dd8d-259">If you need to access the `SearchCredentials` of a `SearchIndexClient` or `SearchServiceClient`, please use the new `SearchCredentials` property.</span></span>

<span data-ttu-id="4dd8d-260">I tidigare versioner av SDK, `SearchServiceClient` och `SearchIndexClient` hade konstruktorer som tog en `HttpClient` parameter.</span><span class="sxs-lookup"><span data-stu-id="4dd8d-260">In older versions of the SDK, `SearchServiceClient` and `SearchIndexClient` had constructors that took an `HttpClient` parameter.</span></span> <span data-ttu-id="4dd8d-261">Dessa har ersatts av konstruktorer som tar en `HttpClientHandler` och en matris med `DelegatingHandler` objekt.</span><span class="sxs-lookup"><span data-stu-id="4dd8d-261">These have been replaced with constructors that take an `HttpClientHandler` and an array of `DelegatingHandler` objects.</span></span> <span data-ttu-id="4dd8d-262">På så sätt blir det lättare att installera egna hanterare för att Förbearbeta HTTP-begäranden om det behövs.</span><span class="sxs-lookup"><span data-stu-id="4dd8d-262">This makes it easier to install custom handlers to pre-process HTTP requests if necessary.</span></span>

<span data-ttu-id="4dd8d-263">Slutligen konstruktorer som tog en `Uri` och `SearchCredentials` har ändrats.</span><span class="sxs-lookup"><span data-stu-id="4dd8d-263">Finally, the constructors that took a `Uri` and `SearchCredentials` have changed.</span></span> <span data-ttu-id="4dd8d-264">Till exempel om du har kod som ser ut så här:</span><span class="sxs-lookup"><span data-stu-id="4dd8d-264">For example, if you have code that looks like this:</span></span>

    var client =
        new SearchServiceClient(
            new SearchCredentials("abc123"),
            new Uri("http://myservice.search.windows.net"));

<span data-ttu-id="4dd8d-265">Du kan ändra den till det rätta till eventuella build-fel:</span><span class="sxs-lookup"><span data-stu-id="4dd8d-265">You can change it to this to fix any build errors:</span></span>

    var client =
        new SearchServiceClient(
            new Uri("http://myservice.search.windows.net"),
            new SearchCredentials("abc123"));

<span data-ttu-id="4dd8d-266">Även Observera att typen för parametern autentiseringsuppgifter har ändrats till `ServiceClientCredentials`.</span><span class="sxs-lookup"><span data-stu-id="4dd8d-266">Also note that the type of the credentials parameter has changed to `ServiceClientCredentials`.</span></span> <span data-ttu-id="4dd8d-267">Det är inte sannolikt att påverka din kod sedan `SearchCredentials` är härledd från `ServiceClientCredentials`.</span><span class="sxs-lookup"><span data-stu-id="4dd8d-267">This is unlikely to affect your code since `SearchCredentials` is derived from `ServiceClientCredentials`.</span></span>

#### <a name="passing-a-request-id"></a><span data-ttu-id="4dd8d-268">Skicka en begäran-ID</span><span class="sxs-lookup"><span data-stu-id="4dd8d-268">Passing a request ID</span></span>
<span data-ttu-id="4dd8d-269">I äldre versioner av SDK kan du ange en begärande-ID på den `SearchServiceClient` eller `SearchIndexClient` och det skulle ingå i varje begäran REST-API: et.</span><span class="sxs-lookup"><span data-stu-id="4dd8d-269">In older versions of the SDK, you could set a request ID on the `SearchServiceClient` or `SearchIndexClient` and it would be included in every request to the REST API.</span></span> <span data-ttu-id="4dd8d-270">Detta är användbart för felsökning av problem med din söktjänst om du behöver kontakta support.</span><span class="sxs-lookup"><span data-stu-id="4dd8d-270">This is useful for troubleshooting issues with your search service if you need to contact support.</span></span> <span data-ttu-id="4dd8d-271">Det är mer användbar för att ange en unik begäran-ID för varje åtgärd i stället för att använda samma ID för alla åtgärder.</span><span class="sxs-lookup"><span data-stu-id="4dd8d-271">However, it is more useful to set a unique request ID for every operation rather than to use the same ID for all operations.</span></span> <span data-ttu-id="4dd8d-272">Därför kan den `SetClientRequestId` metoder för `SearchServiceClient` och `SearchIndexClient` har tagits bort.</span><span class="sxs-lookup"><span data-stu-id="4dd8d-272">For this reason, the `SetClientRequestId` methods of `SearchServiceClient` and `SearchIndexClient` have been removed.</span></span> <span data-ttu-id="4dd8d-273">I stället du kan skicka en begärande-ID till varje åtgärdsmetod via det valfria `SearchRequestOptions` parameter.</span><span class="sxs-lookup"><span data-stu-id="4dd8d-273">Instead, you can pass a request ID to each operation method via the optional `SearchRequestOptions` parameter.</span></span>

> [!NOTE]
> <span data-ttu-id="4dd8d-274">I en framtida version av SDK, kommer vi lägga till en ny mekanism för att ange ID för förfrågan globalt på klienten objekt som är kompatibel med den metod som används av andra Azure-SDK.</span><span class="sxs-lookup"><span data-stu-id="4dd8d-274">In a future release of the SDK, we will add a new mechanism for setting a request ID globally on the client objects that is consistent with the approach used by other Azure SDKs.</span></span>
> 
> 

#### <a name="example"></a><span data-ttu-id="4dd8d-275">Exempel</span><span class="sxs-lookup"><span data-stu-id="4dd8d-275">Example</span></span>
<span data-ttu-id="4dd8d-276">Om du har kod som ser ut så här:</span><span class="sxs-lookup"><span data-stu-id="4dd8d-276">If you have code that looks like this:</span></span>

    client.SetClientRequestId(Guid.NewGuid());
    ...
    long count = client.Documents.Count();

<span data-ttu-id="4dd8d-277">Du kan ändra den till det rätta till eventuella build-fel:</span><span class="sxs-lookup"><span data-stu-id="4dd8d-277">You can change it to this to fix any build errors:</span></span>

    long count = client.Documents.Count(new SearchRequestOptions(requestId: Guid.NewGuid()));

#### <a name="interface-name-changes"></a><span data-ttu-id="4dd8d-278">Gränssnittet namnändringar</span><span class="sxs-lookup"><span data-stu-id="4dd8d-278">Interface name changes</span></span>
<span data-ttu-id="4dd8d-279">Namnen på åtgärden gränssnitt har ändrats för att överensstämma med motsvarande egenskapsnamn:</span><span class="sxs-lookup"><span data-stu-id="4dd8d-279">The operation group interface names have all changed to be consistent with their corresponding property names:</span></span>

* <span data-ttu-id="4dd8d-280">Typ av `ISearchServiceClient.Indexes` har ändrats från `IIndexOperations` till `IIndexesOperations`.</span><span class="sxs-lookup"><span data-stu-id="4dd8d-280">The type of `ISearchServiceClient.Indexes` has been renamed from `IIndexOperations` to `IIndexesOperations`.</span></span>
* <span data-ttu-id="4dd8d-281">Typ av `ISearchServiceClient.Indexers` har ändrats från `IIndexerOperations` till `IIndexersOperations`.</span><span class="sxs-lookup"><span data-stu-id="4dd8d-281">The type of `ISearchServiceClient.Indexers` has been renamed from `IIndexerOperations` to `IIndexersOperations`.</span></span>
* <span data-ttu-id="4dd8d-282">Typ av `ISearchServiceClient.DataSources` har ändrats från `IDataSourceOperations` till `IDataSourcesOperations`.</span><span class="sxs-lookup"><span data-stu-id="4dd8d-282">The type of `ISearchServiceClient.DataSources` has been renamed from `IDataSourceOperations` to `IDataSourcesOperations`.</span></span>
* <span data-ttu-id="4dd8d-283">Typ av `ISearchIndexClient.Documents` har ändrats från `IDocumentOperations` till `IDocumentsOperations`.</span><span class="sxs-lookup"><span data-stu-id="4dd8d-283">The type of `ISearchIndexClient.Documents` has been renamed from `IDocumentOperations` to `IDocumentsOperations`.</span></span>

<span data-ttu-id="4dd8d-284">Den här ändringen är inte troligt att påverka din kod om du har skapat mocks av dessa gränssnitt för testning.</span><span class="sxs-lookup"><span data-stu-id="4dd8d-284">This change is unlikely to affect your code unless you created mocks of these interfaces for test purposes.</span></span>

<a name="BugFixesV1"></a>

### <a name="bug-fixes-in-version-11"></a><span data-ttu-id="4dd8d-285">Felkorrigeringar i version 1.1</span><span class="sxs-lookup"><span data-stu-id="4dd8d-285">Bug fixes in version 1.1</span></span>
<span data-ttu-id="4dd8d-286">Det uppstod ett fel i äldre versioner av Azure Search .NET SDK som är relaterat till serialisering av anpassade modellen klasser.</span><span class="sxs-lookup"><span data-stu-id="4dd8d-286">There was a bug in older versions of the Azure Search .NET SDK relating to serialization of custom model classes.</span></span> <span data-ttu-id="4dd8d-287">Felet kan inträffa om du har skapat en anpassad modellklass med en egenskap för en icke-nullbar värdetyp.</span><span class="sxs-lookup"><span data-stu-id="4dd8d-287">The bug could occur if you created a custom model class with a property of a non-nullable value type.</span></span>

#### <a name="steps-to-reproduce"></a><span data-ttu-id="4dd8d-288">Steg för att återskapa</span><span class="sxs-lookup"><span data-stu-id="4dd8d-288">Steps to reproduce</span></span>
<span data-ttu-id="4dd8d-289">Skapa en anpassad modellklass med en egenskap av typen icke-kan ha värdet null.</span><span class="sxs-lookup"><span data-stu-id="4dd8d-289">Create a custom model class with a property of non-nullable value type.</span></span> <span data-ttu-id="4dd8d-290">Till exempel lägga till en offentlig `UnitCount` egenskap av typen `int` i stället för `int?`.</span><span class="sxs-lookup"><span data-stu-id="4dd8d-290">For example, add a public `UnitCount` property of type `int` instead of `int?`.</span></span>

<span data-ttu-id="4dd8d-291">Om du indexera ett dokument med standardvärdet för den aktuella typen (till exempel 0 för `int`), fältet är null i Azure Search.</span><span class="sxs-lookup"><span data-stu-id="4dd8d-291">If you index a document with the default value of that type (for example, 0 for `int`), the field will be null in Azure Search.</span></span> <span data-ttu-id="4dd8d-292">Om du senare söker efter dokument, den `Search` anropet kommer att kasta `JsonSerializationException` klagande som det går inte att konvertera `null` till `int`.</span><span class="sxs-lookup"><span data-stu-id="4dd8d-292">If you subsequently search for that document, the `Search` call will throw `JsonSerializationException` complaining that it can't convert `null` to `int`.</span></span>

<span data-ttu-id="4dd8d-293">Filter får dessutom inte fungerar som förväntat eftersom null har skrivits till index i stället för det avsedda värdet.</span><span class="sxs-lookup"><span data-stu-id="4dd8d-293">Also, filters may not work as expected since null was written to the index instead of the intended value.</span></span>

#### <a name="fix-details"></a><span data-ttu-id="4dd8d-294">Åtgärda information</span><span class="sxs-lookup"><span data-stu-id="4dd8d-294">Fix details</span></span>
<span data-ttu-id="4dd8d-295">Vi har löst problemet i version 1.1 av SDK.</span><span class="sxs-lookup"><span data-stu-id="4dd8d-295">We have fixed this issue in version 1.1 of the SDK.</span></span> <span data-ttu-id="4dd8d-296">Nu om du har en modellklass så här:</span><span class="sxs-lookup"><span data-stu-id="4dd8d-296">Now, if you have a model class like this:</span></span>

    public class Model
    {
        public string Key { get; set; }

        public int IntValue { get; set; }
    }

<span data-ttu-id="4dd8d-297">och du ställer in `IntValue` 0 värdet är nu korrekt serialiserad som 0 på kabeln och lagras som 0 i indexet.</span><span class="sxs-lookup"><span data-stu-id="4dd8d-297">and you set `IntValue` to 0, that value is now correctly serialized as 0 on the wire and stored as 0 in the index.</span></span> <span data-ttu-id="4dd8d-298">Avrunda utlösning också fungerar som förväntat.</span><span class="sxs-lookup"><span data-stu-id="4dd8d-298">Round tripping also works as expected.</span></span>

<span data-ttu-id="4dd8d-299">Det finns en potentiella problem vara medveten om med den här metoden: Om du använder en modell av typen med en icke-nullbar egenskap, måste du **garantera** att inga dokument i indexet innehåller ett null-värde för motsvarande fält.</span><span class="sxs-lookup"><span data-stu-id="4dd8d-299">There is one potential issue to be aware of with this approach: If you use a model type with a non-nullable property, you have to **guarantee** that no documents in your index contain a null value for the corresponding field.</span></span> <span data-ttu-id="4dd8d-300">Varken SDK eller REST API för Azure Search hjälper dig att göra detta.</span><span class="sxs-lookup"><span data-stu-id="4dd8d-300">Neither the SDK nor the Azure Search REST API will help you to enforce this.</span></span>

<span data-ttu-id="4dd8d-301">Detta är inte bara ett hypotetiskt problem. Tänk dig ett scenario där du lägger till ett nytt fält till ett befintligt index som är av typen `Edm.Int32`.</span><span class="sxs-lookup"><span data-stu-id="4dd8d-301">This is not just a hypothetical concern: Imagine a scenario where you add a new field to an existing index that is of type `Edm.Int32`.</span></span> <span data-ttu-id="4dd8d-302">När du har uppdaterat indexdefinitionen har alla dokument ett null-värde för det nya fältet (eftersom alla typer kan vara null i Azure Search).</span><span class="sxs-lookup"><span data-stu-id="4dd8d-302">After updating the index definition, all documents will have a null value for that new field (since all types are nullable in Azure Search).</span></span> <span data-ttu-id="4dd8d-303">Om du sedan använder en modellklass med en icke-nullbar `int`-egenskap för det fältet returneras ett `JsonSerializationException` som detta när du försöker hämta dokument:</span><span class="sxs-lookup"><span data-stu-id="4dd8d-303">If you then use a model class with a non-nullable `int` property for that field, you will get a `JsonSerializationException` like this when trying to retrieve documents:</span></span>

    Error converting value {null} to type 'System.Int32'. Path 'IntValue'.

<span data-ttu-id="4dd8d-304">Därför kan rekommenderar vi ändå att du använder kan ha värdet null typer i din modell-klasser som bästa praxis.</span><span class="sxs-lookup"><span data-stu-id="4dd8d-304">For this reason, we still recommend that you use nullable types in your model classes as a best practice.</span></span>

<span data-ttu-id="4dd8d-305">Mer information om det här felet och korrigering finns [problemet på GitHub](https://github.com/Azure/azure-sdk-for-net/issues/1063).</span><span class="sxs-lookup"><span data-stu-id="4dd8d-305">For more details on this bug and the fix, please see [this issue on GitHub](https://github.com/Azure/azure-sdk-for-net/issues/1063).</span></span>

