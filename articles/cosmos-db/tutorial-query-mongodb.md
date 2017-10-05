---
title: "Azure Cosmos DB: Hur man frågan med DocumentDB-API: et? | Microsoft Docs"
description: "Lär dig att fråga med DocumentDB-API: et för Azure Cosmos DB"
services: cosmos-db
documentationcenter: 
author: mimig1
manager: jhubbard
editor: 
tags: 
ms.assetid: 
ms.service: cosmos-db
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: 
ms.date: 05/10/2017
ms.author: mimig
ms.openlocfilehash: feffc553a9aa931d96cec71c101674fce08a466b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="azure-cosmos-db-how-to-query-with-api-for-mongodb"></a><span data-ttu-id="c8674-104">Azure Cosmos DB: Hur man frågan med API för MongoDB?</span><span class="sxs-lookup"><span data-stu-id="c8674-104">Azure Cosmos DB: How to query with API for MongoDB?</span></span>

<span data-ttu-id="c8674-105">Azure Cosmos DB [API för MongoDB](mongodb-introduction.md) stöder [MongoDB shell frågor](https://docs.mongodb.com/manual/tutorial/query-documents/).</span><span class="sxs-lookup"><span data-stu-id="c8674-105">The Azure Cosmos DB [API for MongoDB](mongodb-introduction.md) supports [MongoDB shell queries](https://docs.mongodb.com/manual/tutorial/query-documents/).</span></span> 

<span data-ttu-id="c8674-106">Den här artikeln omfattar följande aktiviteter:</span><span class="sxs-lookup"><span data-stu-id="c8674-106">This article covers the following tasks:</span></span> 

> [!div class="checklist"]
> * <span data-ttu-id="c8674-107">Datafrågor med MongoDB</span><span class="sxs-lookup"><span data-stu-id="c8674-107">Querying data with MongoDB</span></span>

## <a name="sample-document"></a><span data-ttu-id="c8674-108">Exempel på ett dokument</span><span class="sxs-lookup"><span data-stu-id="c8674-108">Sample document</span></span>

<span data-ttu-id="c8674-109">Frågorna i den här artikeln använder följande exempeldokumentet.</span><span class="sxs-lookup"><span data-stu-id="c8674-109">The queries in this article use the following sample document.</span></span>

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
## <span data-ttu-id="c8674-110"><a id="examplequery1"></a>Exempelfråga 1</span><span class="sxs-lookup"><span data-stu-id="c8674-110"><a id="examplequery1"></a> Example query 1</span></span> 

<span data-ttu-id="c8674-111">Exempel family dokumentet ovan får följande fråga returnerar dokument där fältet id matchar `WakefieldFamily`.</span><span class="sxs-lookup"><span data-stu-id="c8674-111">Given the sample family document above, the following query returns the documents where the id field matches `WakefieldFamily`.</span></span>

<span data-ttu-id="c8674-112">**Fråga**</span><span class="sxs-lookup"><span data-stu-id="c8674-112">**Query**</span></span>
    
    db.families.find({ id: “WakefieldFamily”})

<span data-ttu-id="c8674-113">**Resultat**</span><span class="sxs-lookup"><span data-stu-id="c8674-113">**Results**</span></span>

    {
    "_id": "ObjectId(\"58f65e1198f3a12c7090e68c\")",
    "id": "WakefieldFamily",
    "parents": [
      {
        "familyName": "Wakefield",
        "givenName": "Robin"
      },
      {
        "familyName": "Miller",
        "givenName": "Ben"
      }
    ],
    "children": [
      {
        "familyName": "Merriam",
        "givenName": "Jesse",
        "gender": "female",
        "grade": 1,
        "pets": [
          { "givenName": "Goofy" },
          { "givenName": "Shadow" }
        ]
      },
      {
        "familyName": "Miller",
        "givenName": "Lisa",
        "gender": "female",
        "grade": 8
      }
    ],
    "address": {
      "state": "NY",
      "county": "Manhattan",
      "city": "NY"
    },
    "creationDate": 1431620462,
    "isRegistered": false
    }

## <span data-ttu-id="c8674-114"><a id="examplequery2"></a>Exempelfråga 2</span><span class="sxs-lookup"><span data-stu-id="c8674-114"><a id="examplequery2"></a>Example query 2</span></span> 

<span data-ttu-id="c8674-115">Nästa fråga returnerar alla underordnade i familjen.</span><span class="sxs-lookup"><span data-stu-id="c8674-115">The next query returns all the children in the family.</span></span> 

<span data-ttu-id="c8674-116">**Fråga**</span><span class="sxs-lookup"><span data-stu-id="c8674-116">**Query**</span></span>
    
    db.familes.find( { id: “WakefieldFamily” }, { children: true } )

<span data-ttu-id="c8674-117">**Resultat**</span><span class="sxs-lookup"><span data-stu-id="c8674-117">**Results**</span></span>

    {
    "_id": "ObjectId("58f65e1198f3a12c7090e68c")",
    "children": [
      {
        "familyName": "Merriam",
        "givenName": "Jesse",
        "gender": "female",
        "grade": 1,
        "pets": [
          { "givenName": "Goofy" },
          { "givenName": "Shadow" }
        ]
      },
      {
        "familyName": "Miller",
        "givenName": "Lisa",
        "gender": "female",
        "grade": 8
      }
    ]
    }


## <span data-ttu-id="c8674-118"><a id="examplequery3"></a>Exempelfråga 3</span><span class="sxs-lookup"><span data-stu-id="c8674-118"><a id="examplequery3"></a>Example query 3</span></span> 

<span data-ttu-id="c8674-119">Nästa fråga returnerar familjer som är registrerad.</span><span class="sxs-lookup"><span data-stu-id="c8674-119">The next query returns all the families which are registered.</span></span> 

<span data-ttu-id="c8674-120">**Fråga**</span><span class="sxs-lookup"><span data-stu-id="c8674-120">**Query**</span></span>
    
    db.families.find( { "isRegistered" : true })
<span data-ttu-id="c8674-121">**Resultaten** inget dokument kommer att returneras.</span><span class="sxs-lookup"><span data-stu-id="c8674-121">**Results** No document will be returned.</span></span> 

## <span data-ttu-id="c8674-122"><a id="examplequery4"></a>Exempelfråga 4</span><span class="sxs-lookup"><span data-stu-id="c8674-122"><a id="examplequery4"></a>Example query 4</span></span>

<span data-ttu-id="c8674-123">Nästa fråga returnerar familjer som inte är registrerad.</span><span class="sxs-lookup"><span data-stu-id="c8674-123">The next query returns all the families which are not registered.</span></span> 

<span data-ttu-id="c8674-124">**Fråga**</span><span class="sxs-lookup"><span data-stu-id="c8674-124">**Query**</span></span>
    
    db.families.find( { "isRegistered" : false })
<span data-ttu-id="c8674-125">**Resultat**</span><span class="sxs-lookup"><span data-stu-id="c8674-125">**Results**</span></span>

     {
    "_id": ObjectId("58f65e1198f3a12c7090e68c"),
    "id": "WakefieldFamily",
    "parents": [{
        "familyName": "Wakefield",
        "givenName": "Robin"
    }, {
        "familyName": "Miller",
        "givenName": "Ben"
    }],
    "children": [{
        "familyName": "Merriam",
        "givenName": "Jesse",
        "gender": "female",
        "grade": 1,
        "pets": [{
            "givenName": "Goofy"
        }, {
            "givenName": "Shadow"
        }]
    }, {
        "familyName": "Miller",
        "givenName": "Lisa",
        "gender": "female",
        "grade": 8
    }],
    "address": {
        "state": "NY",
        "county": "Manhattan",
        "city": "NY"
    },
    "creationDate": 1431620462,
    "isRegistered": false
<span data-ttu-id="c8674-126">}</span><span class="sxs-lookup"><span data-stu-id="c8674-126">}</span></span>

## <span data-ttu-id="c8674-127"><a id="examplequery5"></a>Exempelfråga 5</span><span class="sxs-lookup"><span data-stu-id="c8674-127"><a id="examplequery5"></a>Example query 5</span></span>

<span data-ttu-id="c8674-128">Nästa fråga returnerar alla serier som inte är registrerad och tillstånd är NY.</span><span class="sxs-lookup"><span data-stu-id="c8674-128">The next query returns all the families which are not registered and state is NY.</span></span> 

<span data-ttu-id="c8674-129">**Fråga**</span><span class="sxs-lookup"><span data-stu-id="c8674-129">**Query**</span></span>
    
     db.families.find( { "isRegistered" : false, "address.state" : "NY" })

<span data-ttu-id="c8674-130">**Resultat**</span><span class="sxs-lookup"><span data-stu-id="c8674-130">**Results**</span></span>

     {
    "_id": ObjectId("58f65e1198f3a12c7090e68c"),
    "id": "WakefieldFamily",
    "parents": [{
        "familyName": "Wakefield",
        "givenName": "Robin"
    }, {
        "familyName": "Miller",
        "givenName": "Ben"
    }],
    "children": [{
        "familyName": "Merriam",
        "givenName": "Jesse",
        "gender": "female",
        "grade": 1,
        "pets": [{
            "givenName": "Goofy"
        }, {
            "givenName": "Shadow"
        }]
    }, {
        "familyName": "Miller",
        "givenName": "Lisa",
        "gender": "female",
        "grade": 8
    }],
    "address": {
        "state": "NY",
        "county": "Manhattan",
        "city": "NY"
    },
    "creationDate": 1431620462,
    "isRegistered": false
<span data-ttu-id="c8674-131">}</span><span class="sxs-lookup"><span data-stu-id="c8674-131">}</span></span>


## <span data-ttu-id="c8674-132"><a id="examplequery6"></a>Exempelfråga 6</span><span class="sxs-lookup"><span data-stu-id="c8674-132"><a id="examplequery6"></a>Example query 6</span></span>

<span data-ttu-id="c8674-133">Nästa returnerar frågan alla familjer där underordnade klasser är 8.</span><span class="sxs-lookup"><span data-stu-id="c8674-133">The next query returns all the families where children grades are 8.</span></span>

<span data-ttu-id="c8674-134">**Fråga**</span><span class="sxs-lookup"><span data-stu-id="c8674-134">**Query**</span></span>
  
     db.families.find( { children : { $elemMatch: { grade : 8 }} } )

<span data-ttu-id="c8674-135">**Resultat**</span><span class="sxs-lookup"><span data-stu-id="c8674-135">**Results**</span></span>

     {
    "_id": ObjectId("58f65e1198f3a12c7090e68c"),
    "id": "WakefieldFamily",
    "parents": [{
        "familyName": "Wakefield",
        "givenName": "Robin"
    }, {
        "familyName": "Miller",
        "givenName": "Ben"
    }],
    "children": [{
        "familyName": "Merriam",
        "givenName": "Jesse",
        "gender": "female",
        "grade": 1,
        "pets": [{
            "givenName": "Goofy"
        }, {
            "givenName": "Shadow"
        }]
    }, {
        "familyName": "Miller",
        "givenName": "Lisa",
        "gender": "female",
        "grade": 8
    }],
    "address": {
        "state": "NY",
        "county": "Manhattan",
        "city": "NY"
    },
    "creationDate": 1431620462,
    "isRegistered": false
<span data-ttu-id="c8674-136">}</span><span class="sxs-lookup"><span data-stu-id="c8674-136">}</span></span>

## <span data-ttu-id="c8674-137"><a id="examplequery7"></a>Exempelfråga 7</span><span class="sxs-lookup"><span data-stu-id="c8674-137"><a id="examplequery7"></a>Example query 7</span></span>

<span data-ttu-id="c8674-138">Nästa returnerar frågan alla familjer där storleken på underordnade matris är 3.</span><span class="sxs-lookup"><span data-stu-id="c8674-138">The next query returns all the families where size of children array is 3.</span></span>

<span data-ttu-id="c8674-139">**Fråga**</span><span class="sxs-lookup"><span data-stu-id="c8674-139">**Query**</span></span>
  
      db.Family.find( {children: { $size:3} } )

<span data-ttu-id="c8674-140">**Resultat**</span><span class="sxs-lookup"><span data-stu-id="c8674-140">**Results**</span></span>

<span data-ttu-id="c8674-141">Inga resultat returneras som vi inte har mer än 2 underordnade.</span><span class="sxs-lookup"><span data-stu-id="c8674-141">No results will be returned as we do not have more than 2 children.</span></span> <span data-ttu-id="c8674-142">Endast när parametern är 2 kommer den här frågan lyckas och returnerar det fullständiga dokumentet.</span><span class="sxs-lookup"><span data-stu-id="c8674-142">Only when parameter is 2 this query will succeed and return the full document.</span></span>

## <a name="next-steps"></a><span data-ttu-id="c8674-143">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="c8674-143">Next steps</span></span>

<span data-ttu-id="c8674-144">I den här självstudiekursen kommer du har gjort följande:</span><span class="sxs-lookup"><span data-stu-id="c8674-144">In this tutorial, you've done the following:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="c8674-145">Lärt dig hur man frågan med MongoDB</span><span class="sxs-lookup"><span data-stu-id="c8674-145">Learned how to query using MongoDB</span></span> 

<span data-ttu-id="c8674-146">Du kan nu fortsätta till nästa kurs att lära dig hur du distribuerar dina data globalt.</span><span class="sxs-lookup"><span data-stu-id="c8674-146">You can now proceed to the next tutorial to learn how to distribute your data globally.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="c8674-147">Distribuera dina data globalt</span><span class="sxs-lookup"><span data-stu-id="c8674-147">Distribute your data globally</span></span>](tutorial-global-distribution-documentdb.md)

