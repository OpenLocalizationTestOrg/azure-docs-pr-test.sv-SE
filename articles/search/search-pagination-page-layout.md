---
title: "aaaHow toopage sökresultat i Azure Search | Microsoft Docs"
description: "Sidbrytning i Azure Search värdbaserade moln search-tjänsten på Microsoft Azure."
services: search
documentationcenter: 
author: HeidiSteen
manager: jhubbard
editor: 
ms.assetid: a0a1d315-8624-4cdf-b38e-ba12569c6fcc
ms.service: search
ms.devlang: rest-api
ms.workload: search
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 08/29/2016
ms.author: heidist
ms.openlocfilehash: e3abc1ca4d5994b0a77955379081a4fcfa5a7fa7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toopage-search-results-in-azure-search"></a><span data-ttu-id="d54e7-103">Hur toopage sökresultat i Azure Search</span><span class="sxs-lookup"><span data-stu-id="d54e7-103">How toopage search results in Azure Search</span></span>
<span data-ttu-id="d54e7-104">Den här artikeln innehåller råd om hur toouse hello Azure Söktjänsts-REST API tooimplement standardelement en sökning resultatsida, till exempel totalt antal, hämta dokument, sorteringsordningar och navigering.</span><span class="sxs-lookup"><span data-stu-id="d54e7-104">This article provides guidance on how toouse hello Azure Search Service REST API tooimplement standard elements of a search results page, such as total counts, document retrieval, sort orders, and navigation.</span></span>

<span data-ttu-id="d54e7-105">I samtliga fall som anges nedan, sidrelaterade som bidrar med data eller information tooyour sökresultatsidan anges via hello [Sök efter dokument](http://msdn.microsoft.com/library/azure/dn798927.aspx) förfrågningar som skickas tooyour Azure Search-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="d54e7-105">In every case mentioned below, page-related options that contribute data or information tooyour search results page are specified through hello [Search Document](http://msdn.microsoft.com/library/azure/dn798927.aspx) requests sent tooyour Azure Search Service.</span></span> <span data-ttu-id="d54e7-106">Begäran innehålla en GET-command, sökväg, och frågeparametrar som information om vad som efterfrågas hello-tjänsten och hur tooformulate hello svar.</span><span class="sxs-lookup"><span data-stu-id="d54e7-106">Requests include a GET command, path, and query parameters that inform hello service what is being requested, and how tooformulate hello response.</span></span>

> [!NOTE]
> <span data-ttu-id="d54e7-107">En giltig begäran innehåller ett antal element, till exempel en URL: en och sökväg, HTTP-verb `api-version`och så vidare.</span><span class="sxs-lookup"><span data-stu-id="d54e7-107">A valid request includes a number of elements, such as a service URL and path, HTTP verb, `api-version`, and so on.</span></span> <span data-ttu-id="d54e7-108">Planeringsaspekter bort vi hello exempel toohighlight bara hello syntax som är relevanta toopagination.</span><span class="sxs-lookup"><span data-stu-id="d54e7-108">For brevity, we trimmed hello examples toohighlight just hello syntax that is relevant toopagination.</span></span> <span data-ttu-id="d54e7-109">Se hello [Azure Söktjänsts-REST API](http://msdn.microsoft.com/library/azure/dn798935.aspx) dokumentationen för mer information om begäran-syntax.</span><span class="sxs-lookup"><span data-stu-id="d54e7-109">Please see hello [Azure Search Service REST API](http://msdn.microsoft.com/library/azure/dn798935.aspx) documentation for details about request syntax.</span></span>
> 
> 

## <a name="total-hits-and-page-counts"></a><span data-ttu-id="d54e7-110">Totalt antal träffar och sida räknar</span><span class="sxs-lookup"><span data-stu-id="d54e7-110">Total hits and Page Counts</span></span>
<span data-ttu-id="d54e7-111">Visar hello Totalt antal resultat har returnerats från en fråga och returnerar de resulterar i mindre segment är de grundläggande toovirtually alla sidor i sökningen.</span><span class="sxs-lookup"><span data-stu-id="d54e7-111">Showing hello total number of results returned from a query, and then returning those results in smaller chunks, is fundamental toovirtually all search pages.</span></span>

![][1]

<span data-ttu-id="d54e7-112">I Azure Search använder du hello `$count`, `$top`, och `$skip` parametrar tooreturn dessa värden.</span><span class="sxs-lookup"><span data-stu-id="d54e7-112">In Azure Search, you use hello `$count`, `$top`, and `$skip` parameters tooreturn these values.</span></span> <span data-ttu-id="d54e7-113">hello följande exempel visas ett exempel på begäran för totala träffar som `@OData.count`:</span><span class="sxs-lookup"><span data-stu-id="d54e7-113">hello following example shows a sample request for total hits, returned as `@OData.count`:</span></span>

        GET /indexes/onlineCatalog/docs?$count=true

<span data-ttu-id="d54e7-114">Hämta dokument i grupper med 15, och även visar hello Totalt antal träffar, början på första sidan i hello:</span><span class="sxs-lookup"><span data-stu-id="d54e7-114">Retrieve documents in groups of 15, and also show hello total hits, starting at hello first page:</span></span>

        GET /indexes/onlineCatalog/docs?search=*$top=15&$skip=0&$count=true

<span data-ttu-id="d54e7-115">Paginating resultat kräver båda `$top` och `$skip`, där `$top` anger hur många objekt tooreturn i en batch och `$skip` anger hur många objekt tooskip.</span><span class="sxs-lookup"><span data-stu-id="d54e7-115">Paginating results requires both `$top` and `$skip`, where `$top` specifies how many items tooreturn in a batch, and `$skip` specifies how many items tooskip.</span></span> <span data-ttu-id="d54e7-116">Hello följande exempel varje sida som visar hello bredvid 15 objekt visas med hello inkrementell hoppar i hello `$skip` parameter.</span><span class="sxs-lookup"><span data-stu-id="d54e7-116">In hello following example, each page shows hello next 15 items, indicated by hello incremental jumps in hello `$skip` parameter.</span></span>

        GET /indexes/onlineCatalog/docs?search=*$top=15&$skip=0&$count=true

        GET /indexes/onlineCatalog/docs?search=*$top=15&$skip=15&$count=true

        GET /indexes/onlineCatalog/docs?search=*$top=15&$skip=30&$count=true

## <a name="layout"></a><span data-ttu-id="d54e7-117">Layout</span><span class="sxs-lookup"><span data-stu-id="d54e7-117">Layout</span></span>
<span data-ttu-id="d54e7-118">På en sida med sökresultat, kanske du vill tooshow en miniatyrbild, en delmängd av fält och en länk tooa fullständiga produktsida.</span><span class="sxs-lookup"><span data-stu-id="d54e7-118">On a search results page, you might want tooshow a thumbnail image, a subset of fields, and a link tooa full product page.</span></span>

 ![][2]

<span data-ttu-id="d54e7-119">I Azure Search kan du använda `$select` och en sökning kommandot tooimplement upplevelsen.</span><span class="sxs-lookup"><span data-stu-id="d54e7-119">In Azure Search, you would use `$select` and a lookup command tooimplement this experience.</span></span>

<span data-ttu-id="d54e7-120">tooreturn en delmängd av fält för en sida vid sida layout:</span><span class="sxs-lookup"><span data-stu-id="d54e7-120">tooreturn a subset of fields for a tiled layout:</span></span>

        GET /indexes/ onlineCatalog/docs?search=*&$select=productName,imageFile,description,price,rating 

<span data-ttu-id="d54e7-121">Bilder och mediafiler är inte direkt sökbara och ska lagras i en annan plattform för lagring, till exempel Azure Blob storage tooreduce kostnader.</span><span class="sxs-lookup"><span data-stu-id="d54e7-121">Images and media files are not directly searchable and should be stored in another storage platform, such as Azure Blob storage, tooreduce costs.</span></span> <span data-ttu-id="d54e7-122">Definiera ett fält som lagrar hello URL-adressen för hello externt innehåll i hello index och dokument.</span><span class="sxs-lookup"><span data-stu-id="d54e7-122">In hello index and documents, define a field that stores hello URL address of hello external content.</span></span> <span data-ttu-id="d54e7-123">Du kan sedan använda hello fältet som en bildreferens till en.</span><span class="sxs-lookup"><span data-stu-id="d54e7-123">You can then use hello field as an image reference.</span></span> <span data-ttu-id="d54e7-124">hello URL toohello bilden måste vara i hello dokumentet.</span><span class="sxs-lookup"><span data-stu-id="d54e7-124">hello URL toohello image should be in hello document.</span></span>

<span data-ttu-id="d54e7-125">tooretrieve en produktbeskrivning sidan för en **onClick** händelse, Använd [sökning dokumentet](http://msdn.microsoft.com/library/azure/dn798929.aspx) toopass i hello dokumentet tooretrieve hello nyckeln.</span><span class="sxs-lookup"><span data-stu-id="d54e7-125">tooretrieve a product description page for an **onClick** event, use [Lookup Document](http://msdn.microsoft.com/library/azure/dn798929.aspx) toopass in hello key of hello document tooretrieve.</span></span> <span data-ttu-id="d54e7-126">hello datatyp hello nyckel är `Edm.String`.</span><span class="sxs-lookup"><span data-stu-id="d54e7-126">hello data type of hello key is `Edm.String`.</span></span> <span data-ttu-id="d54e7-127">I det här exemplet är det *246810*.</span><span class="sxs-lookup"><span data-stu-id="d54e7-127">In this example, it is *246810*.</span></span> 

        GET /indexes/onlineCatalog/docs/246810

## <a name="sort-by-relevance-rating-or-price"></a><span data-ttu-id="d54e7-128">Sortera efter relevans, klassificering eller pris</span><span class="sxs-lookup"><span data-stu-id="d54e7-128">Sort by relevance, rating, or price</span></span>
<span data-ttu-id="d54e7-129">Sorteringsordningar ofta toorelevance som standard, men det är vanliga toomake alternativa sorteringsordningar är tillgänglig så att kunder kan snabbt blandar befintliga resultat i en annan rank ordning.</span><span class="sxs-lookup"><span data-stu-id="d54e7-129">Sort orders often default toorelevance, but it's common toomake alternative sort orders readily available so that customers can quickly reshuffle existing results into a different rank order.</span></span>

 ![][3]

<span data-ttu-id="d54e7-130">I Azure Search sortering baseras på hello `$orderby` uttryck för alla fält som indexeras som`"Sortable": true.`</span><span class="sxs-lookup"><span data-stu-id="d54e7-130">In Azure Search, sorting is based on hello `$orderby` expression, for all fields that are indexed as `"Sortable": true.`</span></span>

<span data-ttu-id="d54e7-131">Relevans är starkt kopplat till bedömningen profiler.</span><span class="sxs-lookup"><span data-stu-id="d54e7-131">Relevance is strongly associated with scoring profiles.</span></span> <span data-ttu-id="d54e7-132">Du kan använda hello standard poäng, som förlitar sig på text analys och statistik toorank ordning alla resultat med högre poäng fortsätter toodocuments med flera eller starkare matchar en sökterm.</span><span class="sxs-lookup"><span data-stu-id="d54e7-132">You can use hello default scoring, which relies on text analysis and statistics toorank order all results, with higher scores going toodocuments with more or stronger matches on a search term.</span></span>

<span data-ttu-id="d54e7-133">Alternativa sorteringsordningar är vanligtvis kopplad till **onClick** händelser som motringning tooa metod som skapar hello sorteringsordning.</span><span class="sxs-lookup"><span data-stu-id="d54e7-133">Alternative sort orders are typically associated with **onClick** events that call back tooa method that builds hello sort order.</span></span> <span data-ttu-id="d54e7-134">Till exempel få den här sidelement:</span><span class="sxs-lookup"><span data-stu-id="d54e7-134">For example, given this page element:</span></span>

 ![][4]

<span data-ttu-id="d54e7-135">Du skapar en metod som accepterar hello valt sorteringsalternativet som indata och returnerar en sorterad lista för hello villkor som är associerade med det alternativet.</span><span class="sxs-lookup"><span data-stu-id="d54e7-135">You would create a method that accepts hello selected sort option as input, and returns an ordered list for hello criteria associated with that option.</span></span>

 ![][5]

> [!NOTE]
> <span data-ttu-id="d54e7-136">Hello standard bedömningen är tillräcklig för många scenarier, rekommenderar vi basera relevans på en anpassad bedömningsprofilen i stället.</span><span class="sxs-lookup"><span data-stu-id="d54e7-136">While hello default scoring is sufficient for many scenarios, we recommend basing relevance on a custom scoring profile instead.</span></span> <span data-ttu-id="d54e7-137">En anpassad bedömningsprofilen ger dig ett sätt tooboost objekt som är bättre tooyour företag.</span><span class="sxs-lookup"><span data-stu-id="d54e7-137">A custom scoring profile gives you a way tooboost items that are more beneficial tooyour business.</span></span> <span data-ttu-id="d54e7-138">Se [lägga till en bedömningsprofilen](http://msdn.microsoft.com/library/azure/dn798928.aspx) för mer information.</span><span class="sxs-lookup"><span data-stu-id="d54e7-138">See [Add a scoring profile](http://msdn.microsoft.com/library/azure/dn798928.aspx) for more information.</span></span> 
> 
> 

## <a name="faceted-navigation"></a><span data-ttu-id="d54e7-139">Aspektbaserad navigering</span><span class="sxs-lookup"><span data-stu-id="d54e7-139">Faceted navigation</span></span>
<span data-ttu-id="d54e7-140">Sök navigering är vanliga på en resultatsida ofta finns på hello-sida eller en sida.</span><span class="sxs-lookup"><span data-stu-id="d54e7-140">Search navigation is common on a results page, often located at hello side or top of a page.</span></span> <span data-ttu-id="d54e7-141">I Azure Search ger fasetterad navigering automatisk dirigerad sökningen baserat på fördefinierade filter.</span><span class="sxs-lookup"><span data-stu-id="d54e7-141">In Azure Search, faceted navigation provides self-directed search based on predefined filters.</span></span> <span data-ttu-id="d54e7-142">Se [fasetterad navigering i Azure Search](search-faceted-navigation.md) mer information.</span><span class="sxs-lookup"><span data-stu-id="d54e7-142">See [Faceted navigation in Azure Search](search-faceted-navigation.md) for details.</span></span>

## <a name="filters-at-hello-page-level"></a><span data-ttu-id="d54e7-143">Filter på hello sidan nivå</span><span class="sxs-lookup"><span data-stu-id="d54e7-143">Filters at hello page level</span></span>
<span data-ttu-id="d54e7-144">Om lösningsdesignen ingår dedikerade söksidor för specifika typer av innehåll (till exempel ett online retail program som har avdelningar som nämns i hello överst på sidan för hello), kan du infoga ett filteruttryck tillsammans med en **onClick** händelse tooopen en sida i en filtrerad tillstånd.</span><span class="sxs-lookup"><span data-stu-id="d54e7-144">If your solution design included dedicated search pages for specific types of content (for example, an online retail application that has departments listed at hello top of hello page), you can insert a filter expression alongside an **onClick** event tooopen a page in a prefiltered state.</span></span> 

<span data-ttu-id="d54e7-145">Du kan skicka ett filter med eller utan ett sökuttryck.</span><span class="sxs-lookup"><span data-stu-id="d54e7-145">You can send a filter with or without a search expression.</span></span> <span data-ttu-id="d54e7-146">Till exempel filtrerar hello följande begäran på varumärke, returnerar endast de dokument som matchar den.</span><span class="sxs-lookup"><span data-stu-id="d54e7-146">For example, hello following request will filter on brand name, returning only those documents that match it.</span></span>

        GET /indexes/onlineCatalog/docs?$filter=brandname eq ‘Microsoft’ and category eq ‘Games’

<span data-ttu-id="d54e7-147">Se [Sök dokument (Azure Search-API)](http://msdn.microsoft.com/library/azure/dn798927.aspx) för mer information om `$filter` uttryck.</span><span class="sxs-lookup"><span data-stu-id="d54e7-147">See [Search Documents (Azure Search API)](http://msdn.microsoft.com/library/azure/dn798927.aspx) for more information about `$filter` expressions.</span></span>

## <a name="see-also"></a><span data-ttu-id="d54e7-148">Se även</span><span class="sxs-lookup"><span data-stu-id="d54e7-148">See Also</span></span>
* [<span data-ttu-id="d54e7-149">Azure Söktjänsts-REST API</span><span class="sxs-lookup"><span data-stu-id="d54e7-149">Azure Search Service REST API</span></span>](http://msdn.microsoft.com/library/azure/dn798935.aspx)
* [<span data-ttu-id="d54e7-150">Indexåtgärder</span><span class="sxs-lookup"><span data-stu-id="d54e7-150">Index Operations</span></span>](http://msdn.microsoft.com/library/azure/dn798918.aspx)
* [<span data-ttu-id="d54e7-151">Dokumentet åtgärder</span><span class="sxs-lookup"><span data-stu-id="d54e7-151">Document Operations</span></span>](http://msdn.microsoft.com/library/azure/dn800962.aspx)
* [<span data-ttu-id="d54e7-152">Video och självstudier om Azure Search</span><span class="sxs-lookup"><span data-stu-id="d54e7-152">Video and tutorials about Azure Search</span></span>](search-video-demo-tutorial-list.md)
* [<span data-ttu-id="d54e7-153">Fasetterad navigering i Azure Search</span><span class="sxs-lookup"><span data-stu-id="d54e7-153">Faceted Navigation in Azure Search</span></span>](search-faceted-navigation.md)

<!--Image references-->
[1]: ./media/search-pagination-page-layout/Pages-1-Viewing1ofNResults.PNG
[2]: ./media/search-pagination-page-layout/Pages-2-Tiled.PNG
[3]: ./media/search-pagination-page-layout/Pages-3-SortBy.png
[4]: ./media/search-pagination-page-layout/Pages-4-SortbyRelevance.png
[5]: ./media/search-pagination-page-layout/Pages-5-BuildSort.png 
