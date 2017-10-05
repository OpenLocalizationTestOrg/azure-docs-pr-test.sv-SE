---
title: "Hur man frågan med SQL i Azure Cosmos DB? | Microsoft Docs"
description: "Lär dig hur du fråga med DocumentDB data med SQL i Azure Cosmos DB"
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
ms.openlocfilehash: a2a562c06c6302b9548e758b4c6754ec13b6001d
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="azure-cosmos-db-how-to-query-using-sql"></a><span data-ttu-id="952a1-104">Azure Cosmos DB: Hur man frågan med hjälp av SQL?</span><span class="sxs-lookup"><span data-stu-id="952a1-104">Azure Cosmos DB: How to query using SQL?</span></span>

<span data-ttu-id="952a1-105">Azure Cosmos DB [DocumentDB API](documentdb-introduction.md) stöder frågar dokument med hjälp av SQL.</span><span class="sxs-lookup"><span data-stu-id="952a1-105">The Azure Cosmos DB [DocumentDB API](documentdb-introduction.md) supports querying documents using SQL.</span></span> <span data-ttu-id="952a1-106">Den här artikeln innehåller ett exempel på dokument och två exempel SQL-frågor och resultat.</span><span class="sxs-lookup"><span data-stu-id="952a1-106">This article provides a sample document and two sample SQL queries and results.</span></span>

<span data-ttu-id="952a1-107">Den här artikeln omfattar följande aktiviteter:</span><span class="sxs-lookup"><span data-stu-id="952a1-107">This article covers the following tasks:</span></span> 

> [!div class="checklist"]
> * <span data-ttu-id="952a1-108">Hämtning av data med SQL</span><span class="sxs-lookup"><span data-stu-id="952a1-108">Querying data with SQL</span></span>

## <a name="sample-document"></a><span data-ttu-id="952a1-109">Exempel på ett dokument</span><span class="sxs-lookup"><span data-stu-id="952a1-109">Sample document</span></span>

<span data-ttu-id="952a1-110">SQL-frågor i den här artikeln använder följande exempeldokumentet.</span><span class="sxs-lookup"><span data-stu-id="952a1-110">The SQL queries in this article use the following sample document.</span></span>

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
## <a name="where-can-i-run-sql-queries"></a><span data-ttu-id="952a1-111">Var kan jag köra SQL-frågor?</span><span class="sxs-lookup"><span data-stu-id="952a1-111">Where can I run SQL queries?</span></span>

<span data-ttu-id="952a1-112">Du kan köra frågor med Data Explorer i Azure-portalen den [REST-API och SDK](documentdb-sdk-dotnet.md), och även [Query playground](https://www.documentdb.com/sql/demo), som körs frågor på en befintlig uppsättning exempeldata.</span><span class="sxs-lookup"><span data-stu-id="952a1-112">You can run queries using the Data Explorer in the Azure portal, via the [REST API and SDKs](documentdb-sdk-dotnet.md), and even the [Query playground](https://www.documentdb.com/sql/demo), which runs queries on an existing set of sample data.</span></span>

<span data-ttu-id="952a1-113">Mer information om SQL-frågor finns:</span><span class="sxs-lookup"><span data-stu-id="952a1-113">For more information about SQL queries, see:</span></span>
* [<span data-ttu-id="952a1-114">SQL-fråga och SQL-syntax</span><span class="sxs-lookup"><span data-stu-id="952a1-114">SQL query and SQL syntax</span></span>](documentdb-sql-query.md)

## <a name="prerequisites"></a><span data-ttu-id="952a1-115">Krav</span><span class="sxs-lookup"><span data-stu-id="952a1-115">Prerequisites</span></span>

<span data-ttu-id="952a1-116">Den här kursen förutsätter att du har ett konto för Azure Cosmos DB och samling.</span><span class="sxs-lookup"><span data-stu-id="952a1-116">This tutorial assumes you have an Azure Cosmos DB account and collection.</span></span> <span data-ttu-id="952a1-117">Har inte något av de?</span><span class="sxs-lookup"><span data-stu-id="952a1-117">Don't have any of those?</span></span> <span data-ttu-id="952a1-118">Slutför den [5 minuter quickstart](create-mongodb-nodejs.md) eller [developer kursen](tutorial-develop-mongodb.md) att skapa ett konto och samling.</span><span class="sxs-lookup"><span data-stu-id="952a1-118">Complete the [5-minute quickstart](create-mongodb-nodejs.md) or the [developer tutorial](tutorial-develop-mongodb.md) to create an account and collection.</span></span>

## <a name="example-query-1"></a><span data-ttu-id="952a1-119">Exempelfråga 1</span><span class="sxs-lookup"><span data-stu-id="952a1-119">Example query 1</span></span>

<span data-ttu-id="952a1-120">Exempel family dokumentet ovan får följande SQL-frågan returnerar dokument där fältet id matchar `WakefieldFamily`.</span><span class="sxs-lookup"><span data-stu-id="952a1-120">Given the sample family document above, following SQL query returns the documents where the id field matches `WakefieldFamily`.</span></span> <span data-ttu-id="952a1-121">Eftersom det är en `SELECT *` -instruktionen utdata från frågan är klar JSON-dokumentet:</span><span class="sxs-lookup"><span data-stu-id="952a1-121">Since it's a `SELECT *` statement, the output of the query is the complete JSON document:</span></span>

<span data-ttu-id="952a1-122">**Fråga**</span><span class="sxs-lookup"><span data-stu-id="952a1-122">**Query**</span></span>

    SELECT * 
    FROM Families f 
    WHERE f.id = "WakefieldFamily"

<span data-ttu-id="952a1-123">**Resultat**</span><span class="sxs-lookup"><span data-stu-id="952a1-123">**Results**</span></span>

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

## <a name="example-query-2"></a><span data-ttu-id="952a1-124">Exempelfråga 2</span><span class="sxs-lookup"><span data-stu-id="952a1-124">Example query 2</span></span>

<span data-ttu-id="952a1-125">Nästa fråga returnerar alla angivna namnen på underordnade i familjen vars id matchar `WakefieldFamily` sorterade efter deras klass.</span><span class="sxs-lookup"><span data-stu-id="952a1-125">The next query returns all the given names of children in the family whose id matches `WakefieldFamily` ordered by their grade.</span></span>

<span data-ttu-id="952a1-126">**Fråga**</span><span class="sxs-lookup"><span data-stu-id="952a1-126">**Query**</span></span>

    SELECT c.givenName 
    FROM Families f 
    JOIN c IN f.children 
    WHERE f.id = 'WakefieldFamily'
    ORDER BY f.children.grade ASC

<span data-ttu-id="952a1-127">**Resultat**</span><span class="sxs-lookup"><span data-stu-id="952a1-127">**Results**</span></span>

    [
      { "givenName": "Jesse" }, 
      { "givenName": "Lisa"}
    ]


## <a name="next-steps"></a><span data-ttu-id="952a1-128">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="952a1-128">Next steps</span></span>

<span data-ttu-id="952a1-129">I den här självstudiekursen kommer du har gjort följande:</span><span class="sxs-lookup"><span data-stu-id="952a1-129">In this tutorial, you've done the following:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="952a1-130">Lärt dig hur man frågan med SQL</span><span class="sxs-lookup"><span data-stu-id="952a1-130">Learned how to query using SQL</span></span>  

<span data-ttu-id="952a1-131">Du kan nu fortsätta till nästa kurs att lära dig hur du distribuerar dina data globalt.</span><span class="sxs-lookup"><span data-stu-id="952a1-131">You can now proceed to the next tutorial to learn how to distribute your data globally.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="952a1-132">Distribuera dina data globalt</span><span class="sxs-lookup"><span data-stu-id="952a1-132">Distribute your data globally</span></span>](tutorial-global-distribution-documentdb.md)

