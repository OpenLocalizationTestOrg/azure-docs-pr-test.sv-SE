---
title: "Hur du identifierar datakällor i Azure Data Catalog | Microsoft Docs"
description: "Den här artikeln visar hur du identifierar registrerade datatillgångar med Azure Data Catalog, inklusive sökning och filtrering och använda träffar färgmarkera funktionerna i Azure Data Catalog-portalen."
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
ms.openlocfilehash: 9ff67dcb5ecb00440f73f979fd8d2b79a570c674
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="how-to-discover-data-sources-in-azure-data-catalog"></a><span data-ttu-id="78fde-103">Hur du identifierar datakällor i Azure Data Catalog</span><span class="sxs-lookup"><span data-stu-id="78fde-103">How to discover data sources in Azure Data Catalog</span></span>
## <a name="introduction"></a><span data-ttu-id="78fde-104">Introduktion</span><span class="sxs-lookup"><span data-stu-id="78fde-104">Introduction</span></span>
<span data-ttu-id="78fde-105">Azure Data Catalog är en helt hanterad molntjänst som fungerar som ett system för registrering och identifiering för företagets datakällor.</span><span class="sxs-lookup"><span data-stu-id="78fde-105">Azure Data Catalog is a fully managed cloud service that serves as a system of registration and discovery for enterprise data sources.</span></span> <span data-ttu-id="78fde-106">Med andra ord Data Catalog hjälper användare att identifiera, förstå och använda datakällor och det hjälper organisationer som ger mer värde ur befintliga data.</span><span class="sxs-lookup"><span data-stu-id="78fde-106">In other words, Data Catalog helps people discover, understand, and use data sources, and it helps organizations get more value from their existing data.</span></span> <span data-ttu-id="78fde-107">När en datakälla har registrerats med Data Catalog kan indexeras dess metadata av tjänsten, så att du enkelt kan söka för att identifiera de data du behöver.</span><span class="sxs-lookup"><span data-stu-id="78fde-107">After a data source is registered with Data Catalog, its metadata is indexed by the service, so that you can easily search to discover the data you need.</span></span>

## <a name="searching-and-filtering"></a><span data-ttu-id="78fde-108">Sökning och filtrering</span><span class="sxs-lookup"><span data-stu-id="78fde-108">Searching and filtering</span></span>
<span data-ttu-id="78fde-109">Identifieringen i Data Catalog använder två primära mekanismer: sökning och filtrering.</span><span class="sxs-lookup"><span data-stu-id="78fde-109">Discovery in Data Catalog uses two primary mechanisms: searching and filtering.</span></span>

<span data-ttu-id="78fde-110">Sökningen har utformats att vara både intuitiv och kraftfull.</span><span class="sxs-lookup"><span data-stu-id="78fde-110">Searching is designed to be both intuitive and powerful.</span></span> <span data-ttu-id="78fde-111">Som standard matchas sökvillkor mot en egenskap i katalogen, inklusive kommentarer av användaren.</span><span class="sxs-lookup"><span data-stu-id="78fde-111">By default, search terms are matched against any property in the catalog, including user-provided annotations.</span></span>

<span data-ttu-id="78fde-112">Filtreringen är avsedd att komplettera sökningen.</span><span class="sxs-lookup"><span data-stu-id="78fde-112">Filtering is designed to complement searching.</span></span> <span data-ttu-id="78fde-113">Du kan välja specifika egenskaper som till exempel experter, typ av datakälla, objekttyp och taggar.</span><span class="sxs-lookup"><span data-stu-id="78fde-113">You can select specific characteristics such as experts, data source type, object type, and tags.</span></span> <span data-ttu-id="78fde-114">Du kan visa endast matchande datatillgångar och begränsa sökresultaten till matchande tillgångar.</span><span class="sxs-lookup"><span data-stu-id="78fde-114">You can view only matching data assets, and constrain search results to matching assets.</span></span>

<span data-ttu-id="78fde-115">Genom att använda en kombination av sökning och filtrering kan navigera du snabbt datakällor som har registrerats med Data Catalog för att identifiera de datakällor som du behöver.</span><span class="sxs-lookup"><span data-stu-id="78fde-115">By using a combination of searching and filtering, you can quickly navigate the data sources that have been registered with Data Catalog to discover the data sources you need.</span></span>

## <a name="search-syntax"></a><span data-ttu-id="78fde-116">Söksyntax</span><span class="sxs-lookup"><span data-stu-id="78fde-116">Search syntax</span></span>
<span data-ttu-id="78fde-117">Men fritext standardsökningen är enkel och intuitiv, kan du också använda söksyntax för Data Catalog för större kontroll över sökresultaten.</span><span class="sxs-lookup"><span data-stu-id="78fde-117">Although the default free text search is simple and intuitive, you can also use Data Catalog search syntax for greater control over the search results.</span></span> <span data-ttu-id="78fde-118">Data katalogsökning stöder följande tekniker:</span><span class="sxs-lookup"><span data-stu-id="78fde-118">Data Catalog search supports the following techniques:</span></span>

| <span data-ttu-id="78fde-119">Teknik</span><span class="sxs-lookup"><span data-stu-id="78fde-119">Technique</span></span> | <span data-ttu-id="78fde-120">Användning</span><span class="sxs-lookup"><span data-stu-id="78fde-120">Use</span></span> | <span data-ttu-id="78fde-121">Exempel</span><span class="sxs-lookup"><span data-stu-id="78fde-121">Example</span></span> |
| --- | --- | --- |
| <span data-ttu-id="78fde-122">Enkel sökning</span><span class="sxs-lookup"><span data-stu-id="78fde-122">Basic search</span></span> |<span data-ttu-id="78fde-123">Enkel sökning som använder en eller flera sökvillkor.</span><span class="sxs-lookup"><span data-stu-id="78fde-123">Basic search that uses one or more search terms.</span></span> <span data-ttu-id="78fde-124">Resultatet är alla objekt som matchar en egenskap med ett eller flera av de villkor som angetts.</span><span class="sxs-lookup"><span data-stu-id="78fde-124">Results are any assets that match any property with one or more of the terms specified.</span></span> |`sales data` |
| <span data-ttu-id="78fde-125">Egenskapsomfång</span><span class="sxs-lookup"><span data-stu-id="78fde-125">Property scoping</span></span> |<span data-ttu-id="78fde-126">Returnera endast datakällor där söktermen matchas med den angivna egenskapen.</span><span class="sxs-lookup"><span data-stu-id="78fde-126">Return only data sources where the search term is matched with the specified property.</span></span> |`name:finance` |
| <span data-ttu-id="78fde-127">Booleska operatorer</span><span class="sxs-lookup"><span data-stu-id="78fde-127">Boolean operators</span></span> |<span data-ttu-id="78fde-128">Utöka eller begränsa sökningen med booleska åtgärder.</span><span class="sxs-lookup"><span data-stu-id="78fde-128">Broaden or narrow a search by using Boolean operations.</span></span> |`finance NOT corporate` |
| <span data-ttu-id="78fde-129">Gruppera med parenteser</span><span class="sxs-lookup"><span data-stu-id="78fde-129">Grouping with parenthesis</span></span> |<span data-ttu-id="78fde-130">Använd parenteser för att gruppera delar av frågan för att logisk isolering, särskilt i kombination med booleska operatorer.</span><span class="sxs-lookup"><span data-stu-id="78fde-130">Use parentheses to group parts of the query to achieve logical isolation, especially in conjunction with Boolean operators.</span></span> |`name:finance AND (tags:Q1 OR tags:Q2)` |
| <span data-ttu-id="78fde-131">Jämförelseoperatorer</span><span class="sxs-lookup"><span data-stu-id="78fde-131">Comparison operators</span></span> |<span data-ttu-id="78fde-132">Använda andra jämförelser än lika för egenskaper som innehåller datum och numeriska datatyper.</span><span class="sxs-lookup"><span data-stu-id="78fde-132">Use comparisons other than equality for properties that have numeric and date data types.</span></span> |`modifiedTime > "11/05/2014"` |

<span data-ttu-id="78fde-133">Mer information om Data Catalog search finns i [Azure Data Catalog](https://msdn.microsoft.com/library/azure/mt267594.aspx) artikel.</span><span class="sxs-lookup"><span data-stu-id="78fde-133">For more information about Data Catalog search, see the [Azure Data Catalog](https://msdn.microsoft.com/library/azure/mt267594.aspx) article.</span></span>

## <a name="hit-highlighting"></a><span data-ttu-id="78fde-134">Träffmarkering</span><span class="sxs-lookup"><span data-stu-id="78fde-134">Hit highlighting</span></span>
<span data-ttu-id="78fde-135">När du visar sökresultat, visas alla egenskaper som matchar de angivna sökvillkoren (till exempel data Tillgångsnamn, beskrivning och taggar) är markerade för att göra det returnerades lättare att identifiera varför en viss datatillgång av en viss sökning.</span><span class="sxs-lookup"><span data-stu-id="78fde-135">When you view search results, any displayed properties that match the specified search terms (such as the data asset name, description, and tags) are highlighted to make it easier to identify why a given data asset was returned by a given search.</span></span>

> [!NOTE]
> <span data-ttu-id="78fde-136">Om du vill inaktivera träffar markeringen genom att använda den **Markera** växla i Data Catalog-portalen.</span><span class="sxs-lookup"><span data-stu-id="78fde-136">To turn off hit highlighting, use the **Highlight** switch in the Data Catalog portal.</span></span>
>
>

<span data-ttu-id="78fde-137">När du visar sökresultat, kanske den inte alltid är uppenbara varför en datatillgång ingår, även om träffar markering aktiverad.</span><span class="sxs-lookup"><span data-stu-id="78fde-137">When you view search results, it might not always be obvious why a data asset is included, even with hit highlighting enabled.</span></span> <span data-ttu-id="78fde-138">Eftersom alla egenskaper söks som standard kan en datatillgång returneras på grund av en matchning på en egenskap på kolumnnivå.</span><span class="sxs-lookup"><span data-stu-id="78fde-138">Because all properties are searched by default, a data asset might be returned because of a match on a column-level property.</span></span> <span data-ttu-id="78fde-139">Och eftersom flera användare kan kommentera registrerade datatillgångar med egna etiketter och beskrivningar, inte alla metadata kan inte visas i listan över sökresultat.</span><span class="sxs-lookup"><span data-stu-id="78fde-139">And because multiple users can annotate registered data assets with their own tags and descriptions, not all metadata might be displayed in the list of search results.</span></span>

<span data-ttu-id="78fde-140">I standard panelvy, varje bildruta som visas i sökresultatet innehåller en **visa sökterm matchar** ikonen, så att du snabbt kan visa antalet matchningar och deras plats och att gå till dem om du vill.</span><span class="sxs-lookup"><span data-stu-id="78fde-140">In the default tile view, each tile displayed in the search results includes a **View search term matches** icon, so that you can quickly view the number of matches and their location, and to jump to them if you want.</span></span>

 ![Uppnått syntaxmarkering och sökning matchar i Azure Data Catalog-portalen](./media/data-catalog-how-to-discover/search-matches.png)

## <a name="summary"></a><span data-ttu-id="78fde-142">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="78fde-142">Summary</span></span>
<span data-ttu-id="78fde-143">Eftersom registrera en datakälla med Data Catalog kopierar strukturella och beskrivande metadata från datakällan till katalogtjänsten, blir det lättare att identifiera och förstå datakällan.</span><span class="sxs-lookup"><span data-stu-id="78fde-143">Because registering a data source with Data Catalog copies structural and descriptive metadata from the data source to the catalog service, the data source becomes easier to discover and understand.</span></span> <span data-ttu-id="78fde-144">När du har registrerat en datakälla kan du identifiera med hjälp av filtrering och söka från Data Catalog-portalen.</span><span class="sxs-lookup"><span data-stu-id="78fde-144">After you've registered a data source, you can discover it by using filtering and search from within the Data Catalog portal.</span></span>

## <a name="next-steps"></a><span data-ttu-id="78fde-145">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="78fde-145">Next steps</span></span>
* <span data-ttu-id="78fde-146">Steg för steg-information om hur du identifierar datakällor finns [Kom igång med Azure Data Catalog](data-catalog-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="78fde-146">For step-by-step details about how to discover data sources, see [Get Started with Azure Data Catalog](data-catalog-get-started.md).</span></span>
