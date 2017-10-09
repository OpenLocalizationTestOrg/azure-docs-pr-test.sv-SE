---
title: "aaa ”fråga ett index (portal - Azure Search) | Microsoft Docs ”"
description: "Utfärda en sökfråga i hello Azure Portal Sök Explorer."
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
ms.openlocfilehash: 56bab3ef8a66eeb053fbbeb6d322acb6824fb34b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="query-an-azure-search-index-using-search-explorer-in-hello-azure-portal"></a><span data-ttu-id="ba723-103">Fråga en Azure Search-index med Sök Explorer i hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="ba723-103">Query an Azure Search index using Search Explorer in hello Azure Portal</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="ba723-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="ba723-104">Overview</span></span>](search-query-overview.md)
> * [<span data-ttu-id="ba723-105">Portalen</span><span class="sxs-lookup"><span data-stu-id="ba723-105">Portal</span></span>](search-explorer.md)
> * [<span data-ttu-id="ba723-106">.NET</span><span class="sxs-lookup"><span data-stu-id="ba723-106">.NET</span></span>](search-query-dotnet.md)
> * [<span data-ttu-id="ba723-107">REST</span><span class="sxs-lookup"><span data-stu-id="ba723-107">REST</span></span>](search-query-rest-api.md)
> 
> 

<span data-ttu-id="ba723-108">Den här artikeln visar hur tooquery ett Azure Search index med **Sök Explorer** i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="ba723-108">This article shows you how tooquery an Azure Search index using **Search Explorer** in hello Azure portal.</span></span> <span data-ttu-id="ba723-109">Du kan använda Sök Explorer toosubmit enkla eller fullständig Lucene frågan strängar tooany befintligt index i din tjänst.</span><span class="sxs-lookup"><span data-stu-id="ba723-109">You can use Search Explorer toosubmit simple or full Lucene query strings tooany existing index in your service.</span></span>

## <a name="open-hello-service-dashboard"></a><span data-ttu-id="ba723-110">Öppna hello service instrumentpanelen</span><span class="sxs-lookup"><span data-stu-id="ba723-110">Open hello service dashboard</span></span>
1. <span data-ttu-id="ba723-111">Klicka på **alla resurser** i hello hopp stapel på vänster sida av hello hello [Azure-portalen](https://portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Search%2FsearchServices).</span><span class="sxs-lookup"><span data-stu-id="ba723-111">Click **All resources** in hello jump bar on hello left side of hello [Azure portal](https://portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Search%2FsearchServices).</span></span>
2. <span data-ttu-id="ba723-112">Välj din Azure Search-tjänst.</span><span class="sxs-lookup"><span data-stu-id="ba723-112">Select your Azure Search service.</span></span>

## <a name="select-an-index"></a><span data-ttu-id="ba723-113">Välj ett index</span><span class="sxs-lookup"><span data-stu-id="ba723-113">Select an index</span></span>

<span data-ttu-id="ba723-114">Välj hello index som toosearch från hello **index** panelen.</span><span class="sxs-lookup"><span data-stu-id="ba723-114">Select hello index you would like toosearch from hello **Indexes** tile.</span></span>

   ![](./media/search-explorer/pick-index.png)

## <a name="open-search-explorer"></a><span data-ttu-id="ba723-115">Öppna Sökutforskaren</span><span class="sxs-lookup"><span data-stu-id="ba723-115">Open Search Explorer</span></span>

<span data-ttu-id="ba723-116">Klicka på hello Sök Explorer panelen tooslide öppna hello sökfältet och resultatfönstret.</span><span class="sxs-lookup"><span data-stu-id="ba723-116">Click on hello Search Explorer tile tooslide open hello search bar and results pane.</span></span>

   ![](./media/search-explorer/search-explorer-tile.png)

## <a name="start-searching"></a><span data-ttu-id="ba723-117">Starta sökningen</span><span class="sxs-lookup"><span data-stu-id="ba723-117">Start searching</span></span>

<span data-ttu-id="ba723-118">När du använder hello Sök Explorer kan du ange [fråga parametrar](https://docs.microsoft.com/rest/api/searchservice/Search-Documents) tooformulate hello frågan.</span><span class="sxs-lookup"><span data-stu-id="ba723-118">When using hello Search Explorer, you can specify [query parameters](https://docs.microsoft.com/rest/api/searchservice/Search-Documents) tooformulate hello query.</span></span>

1. <span data-ttu-id="ba723-119">Skriv en fråga i **Frågesträng** och tryck sedan på **Sök**.</span><span class="sxs-lookup"><span data-stu-id="ba723-119">In **Query string**, type a query and then press **Search**.</span></span> 

   <span data-ttu-id="ba723-120">hello frågesträngen är automatiskt parsas till hello rätt begäran URL toosubmit en HTTP-begäran mot hello Azure Search REST API.</span><span class="sxs-lookup"><span data-stu-id="ba723-120">hello query string is automatically parsed into hello proper request URL toosubmit a HTTP request against hello Azure Search REST API.</span></span>   
   
   <span data-ttu-id="ba723-121">Du kan använda någon giltig enkla eller fullständig Lucene syntax toocreate hello fråga.</span><span class="sxs-lookup"><span data-stu-id="ba723-121">You can use any valid simple or full Lucene query syntax toocreate hello request.</span></span> <span data-ttu-id="ba723-122">Hej `*` tecken är likvärdiga tooan tom eller ospecificerad sökning som returnerar alla dokument i bokstavsordning.</span><span class="sxs-lookup"><span data-stu-id="ba723-122">hello `*` character is equivalent tooan empty or unspecified search that returns all documents in no particular order.</span></span>

2. <span data-ttu-id="ba723-123">I **resultat**, frågans resultat visas i raw JSON identiska toohello nyttolast som returneras i en HTTP-svarstexten vid utfärdande av begäranden via programmering.</span><span class="sxs-lookup"><span data-stu-id="ba723-123">In  **Results**, query results are presented in raw JSON, identical toohello payload returned in an HTTP Response Body when issuing requests programmatically.</span></span>

   ![](./media/search-explorer/search-bar.png)

## <a name="next-steps"></a><span data-ttu-id="ba723-124">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="ba723-124">Next steps</span></span>

<span data-ttu-id="ba723-125">hello följande resurser ger information om ytterligare fråga syntax och exempel.</span><span class="sxs-lookup"><span data-stu-id="ba723-125">hello following resources provide additional query syntax information and examples.</span></span>

 + [<span data-ttu-id="ba723-126">Enkel frågesyntax</span><span class="sxs-lookup"><span data-stu-id="ba723-126">Simple query syntax</span></span>](https://docs.microsoft.com/rest/api/searchservice/simple-query-syntax-in-azure-search) 
 + [<span data-ttu-id="ba723-127">Lucene-frågesyntax</span><span class="sxs-lookup"><span data-stu-id="ba723-127">Lucene query syntax</span></span>](https://docs.microsoft.com/rest/api/searchservice/lucene-query-syntax-in-azure-search) 
 + [<span data-ttu-id="ba723-128">Exempel på Lucene-frågesyntax</span><span class="sxs-lookup"><span data-stu-id="ba723-128">Lucene query syntax examples</span></span>](https://docs.microsoft.com/azure/search/search-query-lucene-examples) 
 + [<span data-ttu-id="ba723-129">Syntax för OData-filteruttryck</span><span class="sxs-lookup"><span data-stu-id="ba723-129">OData Filter expression syntax</span></span>](https://docs.microsoft.com/rest/api/searchservice/odata-expression-syntax-for-azure-search) 