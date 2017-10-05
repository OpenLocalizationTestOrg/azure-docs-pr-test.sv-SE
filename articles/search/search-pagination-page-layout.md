---
title: "Hur du sidan Sökresultat i Azure Search | Microsoft Docs"
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
ms.openlocfilehash: 1054e15a2751c53aad5dbc8054c4cec41102dee9
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-page-search-results-in-azure-search"></a><span data-ttu-id="4760b-103">Arbeta med sökresultatsidor i Azure Search</span><span class="sxs-lookup"><span data-stu-id="4760b-103">How to page search results in Azure Search</span></span>
<span data-ttu-id="4760b-104">Den här artikeln innehåller råd om hur du använder den Azure Söktjänsts-REST API för att implementera standardelement för en sida med sökresultat, till exempel totalt antal, hämta dokument, sorteringsordningar och navigering.</span><span class="sxs-lookup"><span data-stu-id="4760b-104">This article provides guidance on how to use the Azure Search Service REST API to implement standard elements of a search results page, such as total counts, document retrieval, sort orders, and navigation.</span></span>

<span data-ttu-id="4760b-105">I samtliga fall som anges nedan, sidrelaterade som bidrar med data eller information på sidan med sökresultatet anges via den [Sök efter dokument](http://msdn.microsoft.com/library/azure/dn798927.aspx) förfrågningar som skickas till din Azure Search-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="4760b-105">In every case mentioned below, page-related options that contribute data or information to your search results page are specified through the [Search Document](http://msdn.microsoft.com/library/azure/dn798927.aspx) requests sent to your Azure Search Service.</span></span> <span data-ttu-id="4760b-106">Begäranden innefattar en GET-command, sökväg, och frågeparametrar som information om vad som efterfrågas tjänsten samt hur du formulera svaret.</span><span class="sxs-lookup"><span data-stu-id="4760b-106">Requests include a GET command, path, and query parameters that inform the service what is being requested, and how to formulate the response.</span></span>

> [!NOTE]
> <span data-ttu-id="4760b-107">En giltig begäran innehåller ett antal element, till exempel en URL: en och sökväg, HTTP-verb `api-version`och så vidare.</span><span class="sxs-lookup"><span data-stu-id="4760b-107">A valid request includes a number of elements, such as a service URL and path, HTTP verb, `api-version`, and so on.</span></span> <span data-ttu-id="4760b-108">Planeringsaspekter bort vi exempel för att fokusera på just den syntax som är relevant för sidbrytning.</span><span class="sxs-lookup"><span data-stu-id="4760b-108">For brevity, we trimmed the examples to highlight just the syntax that is relevant to pagination.</span></span> <span data-ttu-id="4760b-109">Finns det [Azure Söktjänsts-REST API](http://msdn.microsoft.com/library/azure/dn798935.aspx) dokumentationen för mer information om begäran-syntax.</span><span class="sxs-lookup"><span data-stu-id="4760b-109">Please see the [Azure Search Service REST API](http://msdn.microsoft.com/library/azure/dn798935.aspx) documentation for details about request syntax.</span></span>
> 
> 

## <a name="total-hits-and-page-counts"></a><span data-ttu-id="4760b-110">Totalt antal träffar och sida räknar</span><span class="sxs-lookup"><span data-stu-id="4760b-110">Total hits and Page Counts</span></span>
<span data-ttu-id="4760b-111">Visar det totala antalet resultat som returnerats från en fråga och returnerar sedan resultaten i mindre segment är grundläggande för nästan alla sidor i sökningen.</span><span class="sxs-lookup"><span data-stu-id="4760b-111">Showing the total number of results returned from a query, and then returning those results in smaller chunks, is fundamental to virtually all search pages.</span></span>

![][1]

<span data-ttu-id="4760b-112">I Azure Search använder du den `$count`, `$top`, och `$skip` parametrar för att returnera dessa värden.</span><span class="sxs-lookup"><span data-stu-id="4760b-112">In Azure Search, you use the `$count`, `$top`, and `$skip` parameters to return these values.</span></span> <span data-ttu-id="4760b-113">I följande exempel visas ett exempel på begäran för totala träffar som `@OData.count`:</span><span class="sxs-lookup"><span data-stu-id="4760b-113">The following example shows a sample request for total hits, returned as `@OData.count`:</span></span>

        GET /indexes/onlineCatalog/docs?$count=true

<span data-ttu-id="4760b-114">Hämta dokument i grupper med 15, och även visar de totala träffar början vid den första sidan:</span><span class="sxs-lookup"><span data-stu-id="4760b-114">Retrieve documents in groups of 15, and also show the total hits, starting at the first page:</span></span>

        GET /indexes/onlineCatalog/docs?search=*$top=15&$skip=0&$count=true

<span data-ttu-id="4760b-115">Paginating resultat kräver båda `$top` och `$skip`, där `$top` anger hur många objekt för att returnera i en batch och `$skip` anger hur många objekt om du vill hoppa över.</span><span class="sxs-lookup"><span data-stu-id="4760b-115">Paginating results requires both `$top` and `$skip`, where `$top` specifies how many items to return in a batch, and `$skip` specifies how many items to skip.</span></span> <span data-ttu-id="4760b-116">I följande exempel visar objekt som nästa 15 varje anges av inkrementell hoppar i den `$skip` parameter.</span><span class="sxs-lookup"><span data-stu-id="4760b-116">In the following example, each page shows the next 15 items, indicated by the incremental jumps in the `$skip` parameter.</span></span>

        GET /indexes/onlineCatalog/docs?search=*$top=15&$skip=0&$count=true

        GET /indexes/onlineCatalog/docs?search=*$top=15&$skip=15&$count=true

        GET /indexes/onlineCatalog/docs?search=*$top=15&$skip=30&$count=true

## <a name="layout"></a><span data-ttu-id="4760b-117">Layout</span><span class="sxs-lookup"><span data-stu-id="4760b-117">Layout</span></span>
<span data-ttu-id="4760b-118">På en sida med sökresultat, kanske du vill visa en miniatyrbild, en delmängd av fält och en länk till en fullständig produktsida.</span><span class="sxs-lookup"><span data-stu-id="4760b-118">On a search results page, you might want to show a thumbnail image, a subset of fields, and a link to a full product page.</span></span>

 ![][2]

<span data-ttu-id="4760b-119">Du vill använda i Azure Search `$select` och sökning för att implementera den här upplevelsen.</span><span class="sxs-lookup"><span data-stu-id="4760b-119">In Azure Search, you would use `$select` and a lookup command to implement this experience.</span></span>

<span data-ttu-id="4760b-120">Returnera en delmängd av fält för en sida vid sida layout:</span><span class="sxs-lookup"><span data-stu-id="4760b-120">To return a subset of fields for a tiled layout:</span></span>

        GET /indexes/ onlineCatalog/docs?search=*&$select=productName,imageFile,description,price,rating 

<span data-ttu-id="4760b-121">Bilder och mediefiler är inte direkt sökbara och ska lagras i en annan plattform för lagring, till exempel Azure Blob storage att minska kostnaderna.</span><span class="sxs-lookup"><span data-stu-id="4760b-121">Images and media files are not directly searchable and should be stored in another storage platform, such as Azure Blob storage, to reduce costs.</span></span> <span data-ttu-id="4760b-122">Definiera ett fält som lagrar URL-adressen för det externa innehållet i index och dokument.</span><span class="sxs-lookup"><span data-stu-id="4760b-122">In the index and documents, define a field that stores the URL address of the external content.</span></span> <span data-ttu-id="4760b-123">Du kan sedan använda fältet som en bildreferens till en.</span><span class="sxs-lookup"><span data-stu-id="4760b-123">You can then use the field as an image reference.</span></span> <span data-ttu-id="4760b-124">URL till bilden som ska vara i dokumentet.</span><span class="sxs-lookup"><span data-stu-id="4760b-124">The URL to the image should be in the document.</span></span>

<span data-ttu-id="4760b-125">Att hämta en beskrivning produktsidan för en **onClick** händelse, Använd [sökning dokumentet](http://msdn.microsoft.com/library/azure/dn798929.aspx) att skicka i dokumentet för att hämta nyckeln.</span><span class="sxs-lookup"><span data-stu-id="4760b-125">To retrieve a product description page for an **onClick** event, use [Lookup Document](http://msdn.microsoft.com/library/azure/dn798929.aspx) to pass in the key of the document to retrieve.</span></span> <span data-ttu-id="4760b-126">Datatypen för nyckeln är `Edm.String`.</span><span class="sxs-lookup"><span data-stu-id="4760b-126">The data type of the key is `Edm.String`.</span></span> <span data-ttu-id="4760b-127">I det här exemplet är det *246810*.</span><span class="sxs-lookup"><span data-stu-id="4760b-127">In this example, it is *246810*.</span></span> 

        GET /indexes/onlineCatalog/docs/246810

## <a name="sort-by-relevance-rating-or-price"></a><span data-ttu-id="4760b-128">Sortera efter relevans, klassificering eller pris</span><span class="sxs-lookup"><span data-stu-id="4760b-128">Sort by relevance, rating, or price</span></span>
<span data-ttu-id="4760b-129">Sorteringsordningar ofta standard relevans, men det är vanligt att skapa alternativa sortera order är tillgänglig så att kunder kan snabbt blandar befintliga resultat i en annan rank ordning.</span><span class="sxs-lookup"><span data-stu-id="4760b-129">Sort orders often default to relevance, but it's common to make alternative sort orders readily available so that customers can quickly reshuffle existing results into a different rank order.</span></span>

 ![][3]

<span data-ttu-id="4760b-130">I Azure Search sortering baseras på den `$orderby` uttryck för alla fält som indexeras som`"Sortable": true.`</span><span class="sxs-lookup"><span data-stu-id="4760b-130">In Azure Search, sorting is based on the `$orderby` expression, for all fields that are indexed as `"Sortable": true.`</span></span>

<span data-ttu-id="4760b-131">Relevans är starkt kopplat till bedömningen profiler.</span><span class="sxs-lookup"><span data-stu-id="4760b-131">Relevance is strongly associated with scoring profiles.</span></span> <span data-ttu-id="4760b-132">Du kan använda standard bedömningen som bygger på text analys och statistik för att rangordnas ordning alla resultat med högre resultat ska dokument med flera eller starkare matchningar på en sökterm.</span><span class="sxs-lookup"><span data-stu-id="4760b-132">You can use the default scoring, which relies on text analysis and statistics to rank order all results, with higher scores going to documents with more or stronger matches on a search term.</span></span>

<span data-ttu-id="4760b-133">Alternativa sorteringsordningar är vanligtvis kopplad till **onClick** händelser som anropa en metod som skapar sorteringsordning.</span><span class="sxs-lookup"><span data-stu-id="4760b-133">Alternative sort orders are typically associated with **onClick** events that call back to a method that builds the sort order.</span></span> <span data-ttu-id="4760b-134">Till exempel få den här sidelement:</span><span class="sxs-lookup"><span data-stu-id="4760b-134">For example, given this page element:</span></span>

 ![][4]

<span data-ttu-id="4760b-135">Du skapar en metod som accepterar valda sorteringsalternativet som indata och returnerar en sorterad lista för de kriterier som är associerade med det alternativet.</span><span class="sxs-lookup"><span data-stu-id="4760b-135">You would create a method that accepts the selected sort option as input, and returns an ordered list for the criteria associated with that option.</span></span>

 ![][5]

> [!NOTE]
> <span data-ttu-id="4760b-136">Standard bedömningen är tillräcklig för många scenarier, rekommenderar vi basera relevans på en anpassad bedömningsprofilen i stället.</span><span class="sxs-lookup"><span data-stu-id="4760b-136">While the default scoring is sufficient for many scenarios, we recommend basing relevance on a custom scoring profile instead.</span></span> <span data-ttu-id="4760b-137">En anpassad bedömningsprofilen ger dig ett sätt att förstärka-objekt som är bättre för ditt företag.</span><span class="sxs-lookup"><span data-stu-id="4760b-137">A custom scoring profile gives you a way to boost items that are more beneficial to your business.</span></span> <span data-ttu-id="4760b-138">Se [lägga till en bedömningsprofilen](http://msdn.microsoft.com/library/azure/dn798928.aspx) för mer information.</span><span class="sxs-lookup"><span data-stu-id="4760b-138">See [Add a scoring profile](http://msdn.microsoft.com/library/azure/dn798928.aspx) for more information.</span></span> 
> 
> 

## <a name="faceted-navigation"></a><span data-ttu-id="4760b-139">Aspektbaserad navigering</span><span class="sxs-lookup"><span data-stu-id="4760b-139">Faceted navigation</span></span>
<span data-ttu-id="4760b-140">Sök navigering är vanliga på en resultatsida ofta finns på sidan eller längst upp på sidan.</span><span class="sxs-lookup"><span data-stu-id="4760b-140">Search navigation is common on a results page, often located at the side or top of a page.</span></span> <span data-ttu-id="4760b-141">I Azure Search ger fasetterad navigering automatisk dirigerad sökningen baserat på fördefinierade filter.</span><span class="sxs-lookup"><span data-stu-id="4760b-141">In Azure Search, faceted navigation provides self-directed search based on predefined filters.</span></span> <span data-ttu-id="4760b-142">Se [fasetterad navigering i Azure Search](search-faceted-navigation.md) mer information.</span><span class="sxs-lookup"><span data-stu-id="4760b-142">See [Faceted navigation in Azure Search](search-faceted-navigation.md) for details.</span></span>

## <a name="filters-at-the-page-level"></a><span data-ttu-id="4760b-143">Filter på enskilda sidor</span><span class="sxs-lookup"><span data-stu-id="4760b-143">Filters at the page level</span></span>
<span data-ttu-id="4760b-144">Om en lösningsdesign ingår dedikerade söksidor för specifika typer av innehåll (till exempel ett online retail program som har avdelningar som visas längst upp på sidan), kan du infoga ett filteruttryck tillsammans med en **onClick** händelsen för att öppna en sida i en filtrerad tillstånd.</span><span class="sxs-lookup"><span data-stu-id="4760b-144">If your solution design included dedicated search pages for specific types of content (for example, an online retail application that has departments listed at the top of the page), you can insert a filter expression alongside an **onClick** event to open a page in a prefiltered state.</span></span> 

<span data-ttu-id="4760b-145">Du kan skicka ett filter med eller utan ett sökuttryck.</span><span class="sxs-lookup"><span data-stu-id="4760b-145">You can send a filter with or without a search expression.</span></span> <span data-ttu-id="4760b-146">Till exempel filtrerar följande begäran på varumärke, returnerar endast de dokument som matchar den.</span><span class="sxs-lookup"><span data-stu-id="4760b-146">For example, the following request will filter on brand name, returning only those documents that match it.</span></span>

        GET /indexes/onlineCatalog/docs?$filter=brandname eq ‘Microsoft’ and category eq ‘Games’

<span data-ttu-id="4760b-147">Se [Sök dokument (Azure Search-API)](http://msdn.microsoft.com/library/azure/dn798927.aspx) för mer information om `$filter` uttryck.</span><span class="sxs-lookup"><span data-stu-id="4760b-147">See [Search Documents (Azure Search API)](http://msdn.microsoft.com/library/azure/dn798927.aspx) for more information about `$filter` expressions.</span></span>

## <a name="see-also"></a><span data-ttu-id="4760b-148">Se även</span><span class="sxs-lookup"><span data-stu-id="4760b-148">See Also</span></span>
* [<span data-ttu-id="4760b-149">Azure Söktjänsts-REST API</span><span class="sxs-lookup"><span data-stu-id="4760b-149">Azure Search Service REST API</span></span>](http://msdn.microsoft.com/library/azure/dn798935.aspx)
* [<span data-ttu-id="4760b-150">Indexåtgärder</span><span class="sxs-lookup"><span data-stu-id="4760b-150">Index Operations</span></span>](http://msdn.microsoft.com/library/azure/dn798918.aspx)
* [<span data-ttu-id="4760b-151">Dokumentet åtgärder</span><span class="sxs-lookup"><span data-stu-id="4760b-151">Document Operations</span></span>](http://msdn.microsoft.com/library/azure/dn800962.aspx)
* [<span data-ttu-id="4760b-152">Video och självstudier om Azure Search</span><span class="sxs-lookup"><span data-stu-id="4760b-152">Video and tutorials about Azure Search</span></span>](search-video-demo-tutorial-list.md)
* [<span data-ttu-id="4760b-153">Fasetterad navigering i Azure Search</span><span class="sxs-lookup"><span data-stu-id="4760b-153">Faceted Navigation in Azure Search</span></span>](search-faceted-navigation.md)

<!--Image references-->
[1]: ./media/search-pagination-page-layout/Pages-1-Viewing1ofNResults.PNG
[2]: ./media/search-pagination-page-layout/Pages-2-Tiled.PNG
[3]: ./media/search-pagination-page-layout/Pages-3-SortBy.png
[4]: ./media/search-pagination-page-layout/Pages-4-SortbyRelevance.png
[5]: ./media/search-pagination-page-layout/Pages-5-BuildSort.png 
