---
title: "aaaHow toodiscover datakällor i Azure Data Catalog | Microsoft Docs"
description: "Den här artikeln visar hur toodiscover registrerade datatillgångar med Azure Data Catalog, inklusive sökning och filtrering och använder hello träffar färgmarkera funktionerna i hello Azure Data Catalog-portalen."
services: data-catalog
documentationcenter: 
author: steelanddata
manager: NA
editor: 
tags: 
ms.assetid: f72ae3a3-6573-4710-89a7-f13555e1968c
ms.service: data-catalog
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-catalog
ms.date: 08/15/2017
ms.author: maroche
ms.openlocfilehash: 624834b8895dd50c8931c9d3e6f8dc217927c617
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toodiscover-data-sources-in-azure-data-catalog"></a><span data-ttu-id="c8c0b-103">Hur toodiscover datakällor i Azure Data Catalog</span><span class="sxs-lookup"><span data-stu-id="c8c0b-103">How toodiscover data sources in Azure Data Catalog</span></span>
## <a name="introduction"></a><span data-ttu-id="c8c0b-104">Introduktion</span><span class="sxs-lookup"><span data-stu-id="c8c0b-104">Introduction</span></span>
<span data-ttu-id="c8c0b-105">Azure Data Catalog är en helt hanterad molntjänst som fungerar som ett system för registrering och identifiering för företagets datakällor.</span><span class="sxs-lookup"><span data-stu-id="c8c0b-105">Azure Data Catalog is a fully managed cloud service that serves as a system of registration and discovery for enterprise data sources.</span></span> <span data-ttu-id="c8c0b-106">Med andra ord Data Catalog hjälper användare att identifiera, förstå och använda datakällor och det hjälper organisationer som ger mer värde ur befintliga data.</span><span class="sxs-lookup"><span data-stu-id="c8c0b-106">In other words, Data Catalog helps people discover, understand, and use data sources, and it helps organizations get more value from their existing data.</span></span> <span data-ttu-id="c8c0b-107">När en datakälla har registrerats med Data Catalog kan indexeras dess metadata av hello-tjänsten så att du lätt kan söka toodiscover hello data som du behöver.</span><span class="sxs-lookup"><span data-stu-id="c8c0b-107">After a data source is registered with Data Catalog, its metadata is indexed by hello service, so that you can easily search toodiscover hello data you need.</span></span>

## <a name="searching-and-filtering"></a><span data-ttu-id="c8c0b-108">Sökning och filtrering</span><span class="sxs-lookup"><span data-stu-id="c8c0b-108">Searching and filtering</span></span>
<span data-ttu-id="c8c0b-109">Identifieringen i Data Catalog använder två primära mekanismer: sökning och filtrering.</span><span class="sxs-lookup"><span data-stu-id="c8c0b-109">Discovery in Data Catalog uses two primary mechanisms: searching and filtering.</span></span>

<span data-ttu-id="c8c0b-110">Sökning är utformad toobe både intuitiva och kraftfull.</span><span class="sxs-lookup"><span data-stu-id="c8c0b-110">Searching is designed toobe both intuitive and powerful.</span></span> <span data-ttu-id="c8c0b-111">Som standard matchas söktermer mot någon egenskap i hello-katalogen, inklusive som användaren tillhandahållit anteckningar.</span><span class="sxs-lookup"><span data-stu-id="c8c0b-111">By default, search terms are matched against any property in hello catalog, including user-provided annotations.</span></span>

<span data-ttu-id="c8c0b-112">Filtrering är utformad toocomplement sökning.</span><span class="sxs-lookup"><span data-stu-id="c8c0b-112">Filtering is designed toocomplement searching.</span></span> <span data-ttu-id="c8c0b-113">Du kan välja specifika egenskaper som till exempel experter, typ av datakälla, objekttyp och taggar.</span><span class="sxs-lookup"><span data-stu-id="c8c0b-113">You can select specific characteristics such as experts, data source type, object type, and tags.</span></span> <span data-ttu-id="c8c0b-114">Du kan visa endast matchande datatillgångar och begränsa sökningen resultat toomatching tillgångar.</span><span class="sxs-lookup"><span data-stu-id="c8c0b-114">You can view only matching data assets, and constrain search results toomatching assets.</span></span>

<span data-ttu-id="c8c0b-115">Genom att använda en kombination av sökning och filtrering kan navigera du snabbt hello datakällor som har registrerats med Data Catalog toodiscover hello datakällor måste.</span><span class="sxs-lookup"><span data-stu-id="c8c0b-115">By using a combination of searching and filtering, you can quickly navigate hello data sources that have been registered with Data Catalog toodiscover hello data sources you need.</span></span>

## <a name="search-syntax"></a><span data-ttu-id="c8c0b-116">Söksyntax</span><span class="sxs-lookup"><span data-stu-id="c8c0b-116">Search syntax</span></span>
<span data-ttu-id="c8c0b-117">Men hello fritext standardsökningen är enkel och intuitiv, kan du också använda söksyntax för Data Catalog för större kontroll över hello sökresultat.</span><span class="sxs-lookup"><span data-stu-id="c8c0b-117">Although hello default free text search is simple and intuitive, you can also use Data Catalog search syntax for greater control over hello search results.</span></span> <span data-ttu-id="c8c0b-118">Data Catalog Sök stöder hello följande metoder:</span><span class="sxs-lookup"><span data-stu-id="c8c0b-118">Data Catalog search supports hello following techniques:</span></span>

| <span data-ttu-id="c8c0b-119">Teknik</span><span class="sxs-lookup"><span data-stu-id="c8c0b-119">Technique</span></span> | <span data-ttu-id="c8c0b-120">Användning</span><span class="sxs-lookup"><span data-stu-id="c8c0b-120">Use</span></span> | <span data-ttu-id="c8c0b-121">Exempel</span><span class="sxs-lookup"><span data-stu-id="c8c0b-121">Example</span></span> |
| --- | --- | --- |
| <span data-ttu-id="c8c0b-122">Enkel sökning</span><span class="sxs-lookup"><span data-stu-id="c8c0b-122">Basic search</span></span> |<span data-ttu-id="c8c0b-123">Enkel sökning som använder en eller flera sökvillkor.</span><span class="sxs-lookup"><span data-stu-id="c8c0b-123">Basic search that uses one or more search terms.</span></span> <span data-ttu-id="c8c0b-124">Resultatet är alla objekt som matchar en egenskap med ett eller flera av hello villkoren som angetts.</span><span class="sxs-lookup"><span data-stu-id="c8c0b-124">Results are any assets that match any property with one or more of hello terms specified.</span></span> |`sales data` |
| <span data-ttu-id="c8c0b-125">Egenskapsomfång</span><span class="sxs-lookup"><span data-stu-id="c8c0b-125">Property scoping</span></span> |<span data-ttu-id="c8c0b-126">Returnerar endast datakällor där söktermen hello matchas med hello angivna egenskapen.</span><span class="sxs-lookup"><span data-stu-id="c8c0b-126">Return only data sources where hello search term is matched with hello specified property.</span></span> |`name:finance` |
| <span data-ttu-id="c8c0b-127">Booleska operatorer</span><span class="sxs-lookup"><span data-stu-id="c8c0b-127">Boolean operators</span></span> |<span data-ttu-id="c8c0b-128">Utöka eller begränsa sökningen med booleska åtgärder.</span><span class="sxs-lookup"><span data-stu-id="c8c0b-128">Broaden or narrow a search by using Boolean operations.</span></span> |`finance NOT corporate` |
| <span data-ttu-id="c8c0b-129">Gruppera med parenteser</span><span class="sxs-lookup"><span data-stu-id="c8c0b-129">Grouping with parenthesis</span></span> |<span data-ttu-id="c8c0b-130">Använd parenteser toogroup delar av hello frågan tooachieve logisk isolering, särskilt i kombination med booleska operatorer.</span><span class="sxs-lookup"><span data-stu-id="c8c0b-130">Use parentheses toogroup parts of hello query tooachieve logical isolation, especially in conjunction with Boolean operators.</span></span> |`name:finance AND (tags:Q1 OR tags:Q2)` |
| <span data-ttu-id="c8c0b-131">Jämförelseoperatorer</span><span class="sxs-lookup"><span data-stu-id="c8c0b-131">Comparison operators</span></span> |<span data-ttu-id="c8c0b-132">Använda andra jämförelser än lika för egenskaper som innehåller datum och numeriska datatyper.</span><span class="sxs-lookup"><span data-stu-id="c8c0b-132">Use comparisons other than equality for properties that have numeric and date data types.</span></span> |`modifiedTime > "11/05/2014"` |

<span data-ttu-id="c8c0b-133">Mer information om Data Catalog search finns hello [Azure Data Catalog](https://msdn.microsoft.com/library/azure/mt267594.aspx) artikel.</span><span class="sxs-lookup"><span data-stu-id="c8c0b-133">For more information about Data Catalog search, see hello [Azure Data Catalog](https://msdn.microsoft.com/library/azure/mt267594.aspx) article.</span></span>

## <a name="hit-highlighting"></a><span data-ttu-id="c8c0b-134">Träffmarkering</span><span class="sxs-lookup"><span data-stu-id="c8c0b-134">Hit highlighting</span></span>
<span data-ttu-id="c8c0b-135">När du visar sökresultat något visas egenskaper som matchar angivna hello sökvillkoren (till exempel hello data Tillgångsnamn, beskrivning och taggar) är markerade toomake den enklare tooidentify varför en viss datatillgång returnerades av en viss sökning.</span><span class="sxs-lookup"><span data-stu-id="c8c0b-135">When you view search results, any displayed properties that match hello specified search terms (such as hello data asset name, description, and tags) are highlighted toomake it easier tooidentify why a given data asset was returned by a given search.</span></span>

> [!NOTE]
> <span data-ttu-id="c8c0b-136">tooturn av träffar markering, Använd hello **Markera** växla i hello Data Catalog-portalen.</span><span class="sxs-lookup"><span data-stu-id="c8c0b-136">tooturn off hit highlighting, use hello **Highlight** switch in hello Data Catalog portal.</span></span>
>
>

<span data-ttu-id="c8c0b-137">När du visar sökresultat, kanske den inte alltid är uppenbara varför en datatillgång ingår, även om träffar markering aktiverad.</span><span class="sxs-lookup"><span data-stu-id="c8c0b-137">When you view search results, it might not always be obvious why a data asset is included, even with hit highlighting enabled.</span></span> <span data-ttu-id="c8c0b-138">Eftersom alla egenskaper söks som standard kan en datatillgång returneras på grund av en matchning på en egenskap på kolumnnivå.</span><span class="sxs-lookup"><span data-stu-id="c8c0b-138">Because all properties are searched by default, a data asset might be returned because of a match on a column-level property.</span></span> <span data-ttu-id="c8c0b-139">Och eftersom flera användare kan kommentera registrerade datatillgångar med egna etiketter och beskrivningar, inte alla metadata kan inte visas i hello lista över sökresultat.</span><span class="sxs-lookup"><span data-stu-id="c8c0b-139">And because multiple users can annotate registered data assets with their own tags and descriptions, not all metadata might be displayed in hello list of search results.</span></span>

<span data-ttu-id="c8c0b-140">I hello standard panelen visas varje bildruta som visas i sökresultaten hello innehåller en **visa sökterm matchar** ikonen, så att du snabbt visa hello antalet matchningar och deras plats och toojump toothem om du vill.</span><span class="sxs-lookup"><span data-stu-id="c8c0b-140">In hello default tile view, each tile displayed in hello search results includes a **View search term matches** icon, so that you can quickly view hello number of matches and their location, and toojump toothem if you want.</span></span>

 ![Träffar syntaxmarkering och sökning matchar hello Azure Data Catalog-portalen](./media/data-catalog-how-to-discover/search-matches.png)

## <a name="summary"></a><span data-ttu-id="c8c0b-142">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="c8c0b-142">Summary</span></span>
<span data-ttu-id="c8c0b-143">Eftersom registrera en datakälla med Data Catalog kopior strukturella och beskrivande metadata från hello data source toohello katalogtjänsten, hello datakällan blir enklare toodiscover och förstå.</span><span class="sxs-lookup"><span data-stu-id="c8c0b-143">Because registering a data source with Data Catalog copies structural and descriptive metadata from hello data source toohello catalog service, hello data source becomes easier toodiscover and understand.</span></span> <span data-ttu-id="c8c0b-144">När du har registrerat en datakälla kan du identifiera med hjälp av filtrering och söka från hello Data Catalog-portalen.</span><span class="sxs-lookup"><span data-stu-id="c8c0b-144">After you've registered a data source, you can discover it by using filtering and search from within hello Data Catalog portal.</span></span>

## <a name="next-steps"></a><span data-ttu-id="c8c0b-145">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="c8c0b-145">Next steps</span></span>
* <span data-ttu-id="c8c0b-146">Steg för steg-information om hur toodiscover datakällor, se [Kom igång med Azure Data Catalog](data-catalog-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="c8c0b-146">For step-by-step details about how toodiscover data sources, see [Get Started with Azure Data Catalog](data-catalog-get-started.md).</span></span>
