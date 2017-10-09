---
title: aaaHow tooquery med SQL i Azure Cosmos DB? | Microsoft Docs
description: "Läs tooquery med DocumentDB-data med SQL i Azure Cosmos DB"
services: cosmos-db
documentationcenter: 
author: mimig1
manager: jhubbard
editor: 
tags: 
ms.assetid: 
ms.service: cosmos-db
ms.custom: tutorial-develop
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: 
ms.date: 05/10/2017
ms.author: mimig
ms.openlocfilehash: d3dc51acf92cb78d4f4d9dbac7ec54b1382431cc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-cosmos-db-how-tooquery-using-sql"></a><span data-ttu-id="8b093-104">Azure Cosmos DB: Hur tooquery med hjälp av SQL?</span><span class="sxs-lookup"><span data-stu-id="8b093-104">Azure Cosmos DB: How tooquery using SQL?</span></span>

<span data-ttu-id="8b093-105">hello Azure Cosmos DB [DocumentDB API](documentdb-introduction.md) stöder frågar dokument med hjälp av SQL.</span><span class="sxs-lookup"><span data-stu-id="8b093-105">hello Azure Cosmos DB [DocumentDB API](documentdb-introduction.md) supports querying documents using SQL.</span></span> <span data-ttu-id="8b093-106">Den här artikeln innehåller ett exempel på dokument och två exempel SQL-frågor och resultat.</span><span class="sxs-lookup"><span data-stu-id="8b093-106">This article provides a sample document and two sample SQL queries and results.</span></span>

<span data-ttu-id="8b093-107">Den här artikeln beskriver hello följande uppgifter:</span><span class="sxs-lookup"><span data-stu-id="8b093-107">This article covers hello following tasks:</span></span> 

> [!div class="checklist"]
> * <span data-ttu-id="8b093-108">Hämtning av data med SQL</span><span class="sxs-lookup"><span data-stu-id="8b093-108">Querying data with SQL</span></span>

## <a name="sample-document"></a><span data-ttu-id="8b093-109">Exempel på ett dokument</span><span class="sxs-lookup"><span data-stu-id="8b093-109">Sample document</span></span>

<span data-ttu-id="8b093-110">hello SQL-frågor i den här artikeln använder hello följande exempel-dokument.</span><span class="sxs-lookup"><span data-stu-id="8b093-110">hello SQL queries in this article use hello following sample document.</span></span>

```json
{
  "id": "WakefieldFamily",
  "parents": [
      { "familyName": "Wakefield", "givenName": "Robin" },
      { "familyName": "Miller", "givenName": "Ben" }
  ],
  "children": [
      {
        "familyName": "Merriam", 
        "givenName": "Jesse", 
        "gender": "female", "grade": 1,
        "pets": [
            { "givenName": "Goofy" },
            { "givenName": "Shadow" }
        ]
      },
      { 
        "familyName": "Miller", 
         "givenName": "Lisa", 
         "gender": "female", 
         "grade": 8 }
  ],
  "address": { "state": "NY", "county": "Manhattan", "city": "NY" },
  "creationDate": 1431620462,
  "isRegistered": false
}
```
## <a name="where-can-i-run-sql-queries"></a><span data-ttu-id="8b093-111">Var kan jag köra SQL-frågor?</span><span class="sxs-lookup"><span data-stu-id="8b093-111">Where can I run SQL queries?</span></span>

<span data-ttu-id="8b093-112">Du kan köra frågor med hello Data Explorer i hello Azure-portalen via hello [REST-API och SDK](documentdb-sdk-dotnet.md), och även hello [Query playground](https://www.documentdb.com/sql/demo), som körs frågor på en befintlig uppsättning exempeldata.</span><span class="sxs-lookup"><span data-stu-id="8b093-112">You can run queries using hello Data Explorer in hello Azure portal, via hello [REST API and SDKs](documentdb-sdk-dotnet.md), and even hello [Query playground](https://www.documentdb.com/sql/demo), which runs queries on an existing set of sample data.</span></span>

<span data-ttu-id="8b093-113">Mer information om SQL-frågor finns:</span><span class="sxs-lookup"><span data-stu-id="8b093-113">For more information about SQL queries, see:</span></span>
* [<span data-ttu-id="8b093-114">SQL-fråga och SQL-syntax</span><span class="sxs-lookup"><span data-stu-id="8b093-114">SQL query and SQL syntax</span></span>](documentdb-sql-query.md)

## <a name="prerequisites"></a><span data-ttu-id="8b093-115">Krav</span><span class="sxs-lookup"><span data-stu-id="8b093-115">Prerequisites</span></span>

<span data-ttu-id="8b093-116">Den här kursen förutsätter att du har ett konto för Azure Cosmos DB och samling.</span><span class="sxs-lookup"><span data-stu-id="8b093-116">This tutorial assumes you have an Azure Cosmos DB account and collection.</span></span> <span data-ttu-id="8b093-117">Har inte något av de?</span><span class="sxs-lookup"><span data-stu-id="8b093-117">Don't have any of those?</span></span> <span data-ttu-id="8b093-118">Fullständig hello [5 minuter quickstart](create-mongodb-nodejs.md) eller hello [developer kursen](tutorial-develop-mongodb.md) toocreate ett konto och samling.</span><span class="sxs-lookup"><span data-stu-id="8b093-118">Complete hello [5-minute quickstart](create-mongodb-nodejs.md) or hello [developer tutorial](tutorial-develop-mongodb.md) toocreate an account and collection.</span></span>

## <a name="example-query-1"></a><span data-ttu-id="8b093-119">Exempelfråga 1</span><span class="sxs-lookup"><span data-stu-id="8b093-119">Example query 1</span></span>

<span data-ttu-id="8b093-120">Hello exempel family dokument ovan får följande SQL-frågan returnerar hello dokument där hello id-fältet matchar `WakefieldFamily`.</span><span class="sxs-lookup"><span data-stu-id="8b093-120">Given hello sample family document above, following SQL query returns hello documents where hello id field matches `WakefieldFamily`.</span></span> <span data-ttu-id="8b093-121">Eftersom det är en `SELECT *` hello utdata från frågan hello-instruktionen är hello fullständig JSON-dokumentet:</span><span class="sxs-lookup"><span data-stu-id="8b093-121">Since it's a `SELECT *` statement, hello output of hello query is hello complete JSON document:</span></span>

<span data-ttu-id="8b093-122">**Fråga**</span><span class="sxs-lookup"><span data-stu-id="8b093-122">**Query**</span></span>

    SELECT * 
    FROM Families f 
    WHERE f.id = "WakefieldFamily"

<span data-ttu-id="8b093-123">**Resultat**</span><span class="sxs-lookup"><span data-stu-id="8b093-123">**Results**</span></span>

```json
{
  "id": "WakefieldFamily",
  "parents": [
      { "familyName": "Wakefield", "givenName": "Robin" },
      { "familyName": "Miller", "givenName": "Ben" }
  ],
  "children": [
      {
        "familyName": "Merriam", 
        "givenName": "Jesse", 
        "gender": "female", "grade": 1,
        "pets": [
            { "givenName": "Goofy" },
            { "givenName": "Shadow" }
        ]
      },
      { 
        "familyName": "Miller", 
         "givenName": "Lisa", 
         "gender": "female", 
         "grade": 8 }
  ],
  "address": { "state": "NY", "county": "Manhattan", "city": "NY" },
  "creationDate": 1431620462,
  "isRegistered": false
}
```

## <a name="example-query-2"></a><span data-ttu-id="8b093-124">Exempelfråga 2</span><span class="sxs-lookup"><span data-stu-id="8b093-124">Example query 2</span></span>

<span data-ttu-id="8b093-125">hello nästa fråga returnerar alla hello angivet namn på barn i hello familj vars id matchar `WakefieldFamily` sorterade efter deras klass.</span><span class="sxs-lookup"><span data-stu-id="8b093-125">hello next query returns all hello given names of children in hello family whose id matches `WakefieldFamily` ordered by their grade.</span></span>

<span data-ttu-id="8b093-126">**Fråga**</span><span class="sxs-lookup"><span data-stu-id="8b093-126">**Query**</span></span>

    SELECT c.givenName 
    FROM Families f 
    JOIN c IN f.children 
    WHERE f.id = 'WakefieldFamily'
    ORDER BY f.children.grade ASC

<span data-ttu-id="8b093-127">**Resultat**</span><span class="sxs-lookup"><span data-stu-id="8b093-127">**Results**</span></span>

    [
      { "givenName": "Jesse" }, 
      { "givenName": "Lisa"}
    ]


## <a name="next-steps"></a><span data-ttu-id="8b093-128">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="8b093-128">Next steps</span></span>

<span data-ttu-id="8b093-129">I den här kursen har du gjort hello följande:</span><span class="sxs-lookup"><span data-stu-id="8b093-129">In this tutorial, you've done hello following:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="8b093-130">Lärt dig hur tooquery med SQL</span><span class="sxs-lookup"><span data-stu-id="8b093-130">Learned how tooquery using SQL</span></span>  

<span data-ttu-id="8b093-131">Du kan nu fortsätta toohello nästa självstudiekurs toolearn hur toodistribute data globalt.</span><span class="sxs-lookup"><span data-stu-id="8b093-131">You can now proceed toohello next tutorial toolearn how toodistribute your data globally.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="8b093-132">Distribuera dina data globalt</span><span class="sxs-lookup"><span data-stu-id="8b093-132">Distribute your data globally</span></span>](tutorial-global-distribution-documentdb.md)

