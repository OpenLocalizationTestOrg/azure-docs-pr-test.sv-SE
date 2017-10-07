---
title: aaaHow tooquery diagramdata i Azure Cosmos DB? | Microsoft Docs
description: "Läs tooquery diagramdata i Azure Cosmos DB"
services: cosmos-db
documentationcenter: 
author: dennyglee
manager: jhubbard
editor: 
tags: 
ms.assetid: 8bde5c80-581c-4f70-acb4-9578873c92fa
ms.service: cosmos-db
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: 
ms.date: 05/10/2017
ms.author: denlee
ms.openlocfilehash: fdde881edd6c488e2fea51e5c9665e1d736009fa
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-cosmos-db-how-tooquery-with-hello-graph-api-preview"></a><span data-ttu-id="1a7c9-104">Azure Cosmos DB: Hur tooquery med hello du Graph API (förhandsgranskning)?</span><span class="sxs-lookup"><span data-stu-id="1a7c9-104">Azure Cosmos DB: How tooquery with hello Graph API (preview)?</span></span>

<span data-ttu-id="1a7c9-105">hello Azure Cosmos DB [Graph API](graph-introduction.md) (förhandsversion) stöder [Gremlin](https://docs.mongodb.com/manual/tutorial/query-documents/) frågor.</span><span class="sxs-lookup"><span data-stu-id="1a7c9-105">hello Azure Cosmos DB [Graph API](graph-introduction.md) (preview) supports [Gremlin](https://docs.mongodb.com/manual/tutorial/query-documents/) queries.</span></span> <span data-ttu-id="1a7c9-106">Den här artikeln innehåller exempeldokument och frågor tooget som du startade.</span><span class="sxs-lookup"><span data-stu-id="1a7c9-106">This article provides sample documents and queries tooget you started.</span></span> <span data-ttu-id="1a7c9-107">En detaljerad Gremlin referens har angetts i hello [Gremlin stöd](gremlin-support.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="1a7c9-107">A detailed Gremlin reference is provided in hello [Gremlin support](gremlin-support.md) article.</span></span>

<span data-ttu-id="1a7c9-108">Den här artikeln beskriver hello följande uppgifter:</span><span class="sxs-lookup"><span data-stu-id="1a7c9-108">This article covers hello following tasks:</span></span> 

> [!div class="checklist"]
> * <span data-ttu-id="1a7c9-109">Datafrågor med Gremlin</span><span class="sxs-lookup"><span data-stu-id="1a7c9-109">Querying data with Gremlin</span></span>

## <a name="prerequisites"></a><span data-ttu-id="1a7c9-110">Krav</span><span class="sxs-lookup"><span data-stu-id="1a7c9-110">Prerequisites</span></span>

<span data-ttu-id="1a7c9-111">För dessa frågor toowork måste du har ett konto i Azure Cosmos DB och har diagramdata i hello behållaren.</span><span class="sxs-lookup"><span data-stu-id="1a7c9-111">For these queries toowork, you must have an Azure Cosmos DB account and have graph data in hello container.</span></span> <span data-ttu-id="1a7c9-112">Har inte något av de?</span><span class="sxs-lookup"><span data-stu-id="1a7c9-112">Don't have any of those?</span></span> <span data-ttu-id="1a7c9-113">Fullständig hello [5 minuter quickstart](create-graph-dotnet.md) eller hello [developer kursen](tutorial-query-graph.md) toocreate ett konto och fylla i databasen.</span><span class="sxs-lookup"><span data-stu-id="1a7c9-113">Complete hello [5-minute quickstart](create-graph-dotnet.md) or hello [developer tutorial](tutorial-query-graph.md) toocreate an account and populate your database.</span></span> <span data-ttu-id="1a7c9-114">Du kan köra följande frågor med hello hello [Azure Cosmos DB .NET graph-bibliotek](graph-sdk-dotnet.md), [Gremlin konsolen](https://tinkerpop.apache.org/docs/current/reference/#gremlin-console), eller ditt favoritprogram Gremlin-drivrutinen.</span><span class="sxs-lookup"><span data-stu-id="1a7c9-114">You can run hello following queries using hello [Azure Cosmos DB .NET graph library](graph-sdk-dotnet.md), [Gremlin console](https://tinkerpop.apache.org/docs/current/reference/#gremlin-console), or your favorite Gremlin driver.</span></span>

## <a name="count-vertices-in-hello-graph"></a><span data-ttu-id="1a7c9-115">Antal formhörnen i hello graph</span><span class="sxs-lookup"><span data-stu-id="1a7c9-115">Count vertices in hello graph</span></span>

<span data-ttu-id="1a7c9-116">hello följande utdrag visar hur toocount hello antalet formhörnen i hello diagram:</span><span class="sxs-lookup"><span data-stu-id="1a7c9-116">hello following snippet shows how toocount hello number of vertices in hello graph:</span></span>

```
g.V().count()
```

## <a name="filters"></a><span data-ttu-id="1a7c9-117">Filter</span><span class="sxs-lookup"><span data-stu-id="1a7c9-117">Filters</span></span>

<span data-ttu-id="1a7c9-118">Du kan utföra filter med Gremlin's `has` och `hasLabel` steg och kombinera dem med hjälp av `and`, `or`, och `not` toobuild mer komplexa filter.</span><span class="sxs-lookup"><span data-stu-id="1a7c9-118">You can perform filters using Gremlin's `has` and `hasLabel` steps, and combine them using `and`, `or`, and `not` toobuild more complex filters.</span></span> <span data-ttu-id="1a7c9-119">Azure Cosmos-DB tillhandahåller schema-oberoende indexering av alla egenskaper i din formhörnen och grader för snabb frågor:</span><span class="sxs-lookup"><span data-stu-id="1a7c9-119">Azure Cosmos DB provides schema-agnostic indexing of all properties within your vertices and degrees for fast queries:</span></span>

```
g.V().hasLabel('person').has('age', gt(40))
```

## <a name="projection"></a><span data-ttu-id="1a7c9-120">Projektion</span><span class="sxs-lookup"><span data-stu-id="1a7c9-120">Projection</span></span>

<span data-ttu-id="1a7c9-121">Du kan projicera vissa egenskaper i hello frågeresultat med hello `values` steg:</span><span class="sxs-lookup"><span data-stu-id="1a7c9-121">You can project certain properties in hello query results using hello `values` step:</span></span>

```
g.V().hasLabel('person').values('firstName')
```

## <a name="find-related-edges-and-vertices"></a><span data-ttu-id="1a7c9-122">Hitta relaterade kanter och formhörnen</span><span class="sxs-lookup"><span data-stu-id="1a7c9-122">Find related edges and vertices</span></span>

<span data-ttu-id="1a7c9-123">Hittills har sett vi endast frågeoperatorer som fungerar i en databas.</span><span class="sxs-lookup"><span data-stu-id="1a7c9-123">So far, we've only seen query operators that work in any database.</span></span> <span data-ttu-id="1a7c9-124">Diagram är snabb och effektiv för traversal åtgärder när du behöver toonavigate toorelated kanter och formhörnen.</span><span class="sxs-lookup"><span data-stu-id="1a7c9-124">Graphs are fast and efficient for traversal operations when you need toonavigate toorelated edges and vertices.</span></span> <span data-ttu-id="1a7c9-125">Vi ska hitta alla vänner till Thomas.</span><span class="sxs-lookup"><span data-stu-id="1a7c9-125">Let's find all friends of Thomas.</span></span> <span data-ttu-id="1a7c9-126">Vi kan göra detta med hjälp av Gremlin's `outE` steg toofind alla hello kanter ut från Thomas och sedan gå igenom toohello i formhörnen från dessa kanter med Gremlin's `inV` steg:</span><span class="sxs-lookup"><span data-stu-id="1a7c9-126">We do this by using Gremlin's `outE` step toofind all hello out-edges from Thomas, then traversing toohello in-vertices from those edges using Gremlin's `inV` step:</span></span>

```cs
g.V('thomas').outE('knows').inV().hasLabel('person')
```

<span data-ttu-id="1a7c9-127">hello nästa frågan utför två hopp toofind alla Thomas' ”vänner till vänner”, genom att anropa `outE` och `inV` två gånger.</span><span class="sxs-lookup"><span data-stu-id="1a7c9-127">hello next query performs two hops toofind all of Thomas' "friends of friends", by calling `outE` and `inV` two times.</span></span> 

```cs
g.V('thomas').outE('knows').inV().hasLabel('person').outE('knows').inV().hasLabel('person')
```

<span data-ttu-id="1a7c9-128">Du kan skapa mer komplexa frågor och implementera kraftfulla diagrammet traversal logik med Gremlin, inklusive blandningen filter uttryck, utför slingor med hello `loop` steg och implementera villkorlig navigeringen med hello `choose` steg.</span><span class="sxs-lookup"><span data-stu-id="1a7c9-128">You can build more complex queries and implement powerful graph traversal logic using Gremlin, including mixing filter expressions, performing looping using hello `loop` step, and implementing conditional navigation using hello `choose` step.</span></span> <span data-ttu-id="1a7c9-129">Mer information om vad du kan göra med [Gremlin stöd](gremlin-support.md)!</span><span class="sxs-lookup"><span data-stu-id="1a7c9-129">Learn more about what you can do with [Gremlin support](gremlin-support.md)!</span></span>

## <a name="next-steps"></a><span data-ttu-id="1a7c9-130">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="1a7c9-130">Next steps</span></span>

<span data-ttu-id="1a7c9-131">I den här kursen har du gjort hello följande:</span><span class="sxs-lookup"><span data-stu-id="1a7c9-131">In this tutorial, you've done hello following:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="1a7c9-132">Lärt dig hur tooquery använder Graph</span><span class="sxs-lookup"><span data-stu-id="1a7c9-132">Learned how tooquery using Graph</span></span> 

<span data-ttu-id="1a7c9-133">Du kan nu fortsätta toohello nästa självstudiekurs toolearn hur toodistribute data globalt.</span><span class="sxs-lookup"><span data-stu-id="1a7c9-133">You can now proceed toohello next tutorial toolearn how toodistribute your data globally.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="1a7c9-134">Distribuera dina data globalt</span><span class="sxs-lookup"><span data-stu-id="1a7c9-134">Distribute your data globally</span></span>](tutorial-global-distribution-documentdb.md)