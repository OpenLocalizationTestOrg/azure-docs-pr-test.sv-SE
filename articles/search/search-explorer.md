---
title: "Köra frågor mot ett index (Portal – Azure Search) | Microsoft Docs"
description: "Skicka en sökfråga i Sökutforskaren på Azure Portal."
services: search
manager: jhubbard
documentationcenter: 
author: ashmaka
ms.assetid: 8e524188-73a7-44db-9e64-ae8bf66b05d3
ms.service: search
ms.devlang: NA
ms.workload: search
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.date: 07/10/2017
ms.author: ashmaka
ms.openlocfilehash: dd68d8ed073bf7b8666ddef35a2f1f84df690b4b
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="query-an-azure-search-index-using-search-explorer-in-the-azure-portal"></a><span data-ttu-id="9345d-103">Köra frågor mot ett Azure Search-index med Sökutforskaren i Azure Portal</span><span class="sxs-lookup"><span data-stu-id="9345d-103">Query an Azure Search index using Search Explorer in the Azure Portal</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="9345d-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="9345d-104">Overview</span></span>](search-query-overview.md)
> * [<span data-ttu-id="9345d-105">Portalen</span><span class="sxs-lookup"><span data-stu-id="9345d-105">Portal</span></span>](search-explorer.md)
> * [<span data-ttu-id="9345d-106">.NET</span><span class="sxs-lookup"><span data-stu-id="9345d-106">.NET</span></span>](search-query-dotnet.md)
> * [<span data-ttu-id="9345d-107">REST</span><span class="sxs-lookup"><span data-stu-id="9345d-107">REST</span></span>](search-query-rest-api.md)
> 
> 

<span data-ttu-id="9345d-108">Den här artikeln beskriver hur du kör frågor mot ett Azure Search-index med hjälp av **Sökutforskaren** i Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="9345d-108">This article shows you how to query an Azure Search index using **Search Explorer** in the Azure portal.</span></span> <span data-ttu-id="9345d-109">Med Sökutforskaren kan du skicka enkla frågor eller fullständiga Lucene-frågesträngar till befintliga index i din tjänst.</span><span class="sxs-lookup"><span data-stu-id="9345d-109">You can use Search Explorer to submit simple or full Lucene query strings to any existing index in your service.</span></span>

## <a name="open-the-service-dashboard"></a><span data-ttu-id="9345d-110">Öppna instrumentpanelen</span><span class="sxs-lookup"><span data-stu-id="9345d-110">Open the service dashboard</span></span>
1. <span data-ttu-id="9345d-111">Klicka på **Alla resurser** i snabbåtkomstfältet på vänster sida av [Azure Portal](https://portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Search%2FsearchServices).</span><span class="sxs-lookup"><span data-stu-id="9345d-111">Click **All resources** in the jump bar on the left side of the [Azure portal](https://portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Search%2FsearchServices).</span></span>
2. <span data-ttu-id="9345d-112">Välj din Azure Search-tjänst.</span><span class="sxs-lookup"><span data-stu-id="9345d-112">Select your Azure Search service.</span></span>

## <a name="select-an-index"></a><span data-ttu-id="9345d-113">Välj ett index</span><span class="sxs-lookup"><span data-stu-id="9345d-113">Select an index</span></span>

<span data-ttu-id="9345d-114">Välj det index som du vill söka i från panelen **Index**.</span><span class="sxs-lookup"><span data-stu-id="9345d-114">Select the index you would like to search from the **Indexes** tile.</span></span>

   ![](./media/search-explorer/pick-index.png)

## <a name="open-search-explorer"></a><span data-ttu-id="9345d-115">Öppna Sökutforskaren</span><span class="sxs-lookup"><span data-stu-id="9345d-115">Open Search Explorer</span></span>

<span data-ttu-id="9345d-116">Klicka på panelen Sökutforskaren för att öppna sökfältet och resultatfönstret.</span><span class="sxs-lookup"><span data-stu-id="9345d-116">Click on the Search Explorer tile to slide open the search bar and results pane.</span></span>

   ![](./media/search-explorer/search-explorer-tile.png)

## <a name="start-searching"></a><span data-ttu-id="9345d-117">Starta sökningen</span><span class="sxs-lookup"><span data-stu-id="9345d-117">Start searching</span></span>

<span data-ttu-id="9345d-118">När du använder Sökutforskaren kan du formulera frågan genom att ange valfria [frågeparametrar](https://docs.microsoft.com/rest/api/searchservice/Search-Documents).</span><span class="sxs-lookup"><span data-stu-id="9345d-118">When using the Search Explorer, you can specify [query parameters](https://docs.microsoft.com/rest/api/searchservice/Search-Documents) to formulate the query.</span></span>

1. <span data-ttu-id="9345d-119">Skriv en fråga i **Frågesträng** och tryck sedan på **Sök**.</span><span class="sxs-lookup"><span data-stu-id="9345d-119">In **Query string**, type a query and then press **Search**.</span></span> 

   <span data-ttu-id="9345d-120">Frågesträngen parsas automatiskt till rätt fråge-URL för att sedan skicka en HTTP-begäran mot REST-API:et för Azure Search.</span><span class="sxs-lookup"><span data-stu-id="9345d-120">The query string is automatically parsed into the proper request URL to submit a HTTP request against the Azure Search REST API.</span></span>   
   
   <span data-ttu-id="9345d-121">Du kan skapa begäran med valfri enkel frågesyntax eller en fullständig Lucene-frågesyntax.</span><span class="sxs-lookup"><span data-stu-id="9345d-121">You can use any valid simple or full Lucene query syntax to create the request.</span></span> <span data-ttu-id="9345d-122">Tecknet `*` motsvarar en tom eller ospecificerad sökning som returnerar alla dokument (inte i någon särskild ordning).</span><span class="sxs-lookup"><span data-stu-id="9345d-122">The `*` character is equivalent to an empty or unspecified search that returns all documents in no particular order.</span></span>

2. <span data-ttu-id="9345d-123">I **Resultat** visas frågeresultat i oformaterad JSON. Dessa data är identiska med nyttolasten som returneras i en HTTP-svarstext när begäranden görs via programmering.</span><span class="sxs-lookup"><span data-stu-id="9345d-123">In  **Results**, query results are presented in raw JSON, identical to the payload returned in an HTTP Response Body when issuing requests programmatically.</span></span>

   ![](./media/search-explorer/search-bar.png)

## <a name="next-steps"></a><span data-ttu-id="9345d-124">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="9345d-124">Next steps</span></span>

<span data-ttu-id="9345d-125">Följande resurser innehåller ytterligare information om och exempel på frågesyntax.</span><span class="sxs-lookup"><span data-stu-id="9345d-125">The following resources provide additional query syntax information and examples.</span></span>

 + [<span data-ttu-id="9345d-126">Enkel frågesyntax</span><span class="sxs-lookup"><span data-stu-id="9345d-126">Simple query syntax</span></span>](https://docs.microsoft.com/rest/api/searchservice/simple-query-syntax-in-azure-search) 
 + [<span data-ttu-id="9345d-127">Lucene-frågesyntax</span><span class="sxs-lookup"><span data-stu-id="9345d-127">Lucene query syntax</span></span>](https://docs.microsoft.com/rest/api/searchservice/lucene-query-syntax-in-azure-search) 
 + [<span data-ttu-id="9345d-128">Exempel på Lucene-frågesyntax</span><span class="sxs-lookup"><span data-stu-id="9345d-128">Lucene query syntax examples</span></span>](https://docs.microsoft.com/azure/search/search-query-lucene-examples) 
 + [<span data-ttu-id="9345d-129">Syntax för OData-filteruttryck</span><span class="sxs-lookup"><span data-stu-id="9345d-129">OData Filter expression syntax</span></span>](https://docs.microsoft.com/rest/api/searchservice/odata-expression-syntax-for-azure-search) 