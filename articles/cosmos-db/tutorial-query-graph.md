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
# <a name="azure-cosmos-db-how-tooquery-with-hello-graph-api-preview"></a>Azure Cosmos DB: Hur tooquery med hello du Graph API (förhandsgranskning)?

hello Azure Cosmos DB [Graph API](graph-introduction.md) (förhandsversion) stöder [Gremlin](https://docs.mongodb.com/manual/tutorial/query-documents/) frågor. Den här artikeln innehåller exempeldokument och frågor tooget som du startade. En detaljerad Gremlin referens har angetts i hello [Gremlin stöd](gremlin-support.md) artikel.

Den här artikeln beskriver hello följande uppgifter: 

> [!div class="checklist"]
> * Datafrågor med Gremlin

## <a name="prerequisites"></a>Krav

För dessa frågor toowork måste du har ett konto i Azure Cosmos DB och har diagramdata i hello behållaren. Har inte något av de? Fullständig hello [5 minuter quickstart](create-graph-dotnet.md) eller hello [developer kursen](tutorial-query-graph.md) toocreate ett konto och fylla i databasen. Du kan köra följande frågor med hello hello [Azure Cosmos DB .NET graph-bibliotek](graph-sdk-dotnet.md), [Gremlin konsolen](https://tinkerpop.apache.org/docs/current/reference/#gremlin-console), eller ditt favoritprogram Gremlin-drivrutinen.

## <a name="count-vertices-in-hello-graph"></a>Antal formhörnen i hello graph

hello följande utdrag visar hur toocount hello antalet formhörnen i hello diagram:

```
g.V().count()
```

## <a name="filters"></a>Filter

Du kan utföra filter med Gremlin's `has` och `hasLabel` steg och kombinera dem med hjälp av `and`, `or`, och `not` toobuild mer komplexa filter. Azure Cosmos-DB tillhandahåller schema-oberoende indexering av alla egenskaper i din formhörnen och grader för snabb frågor:

```
g.V().hasLabel('person').has('age', gt(40))
```

## <a name="projection"></a>Projektion

Du kan projicera vissa egenskaper i hello frågeresultat med hello `values` steg:

```
g.V().hasLabel('person').values('firstName')
```

## <a name="find-related-edges-and-vertices"></a>Hitta relaterade kanter och formhörnen

Hittills har sett vi endast frågeoperatorer som fungerar i en databas. Diagram är snabb och effektiv för traversal åtgärder när du behöver toonavigate toorelated kanter och formhörnen. Vi ska hitta alla vänner till Thomas. Vi kan göra detta med hjälp av Gremlin's `outE` steg toofind alla hello kanter ut från Thomas och sedan gå igenom toohello i formhörnen från dessa kanter med Gremlin's `inV` steg:

```cs
g.V('thomas').outE('knows').inV().hasLabel('person')
```

hello nästa frågan utför två hopp toofind alla Thomas' ”vänner till vänner”, genom att anropa `outE` och `inV` två gånger. 

```cs
g.V('thomas').outE('knows').inV().hasLabel('person').outE('knows').inV().hasLabel('person')
```

Du kan skapa mer komplexa frågor och implementera kraftfulla diagrammet traversal logik med Gremlin, inklusive blandningen filter uttryck, utför slingor med hello `loop` steg och implementera villkorlig navigeringen med hello `choose` steg. Mer information om vad du kan göra med [Gremlin stöd](gremlin-support.md)!

## <a name="next-steps"></a>Nästa steg

I den här kursen har du gjort hello följande:

> [!div class="checklist"]
> * Lärt dig hur tooquery använder Graph 

Du kan nu fortsätta toohello nästa självstudiekurs toolearn hur toodistribute data globalt.

> [!div class="nextstepaction"]
> [Distribuera dina data globalt](tutorial-global-distribution-documentdb.md)