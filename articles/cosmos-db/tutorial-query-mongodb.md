---
title: 'Azure Cosmos DB: Tooquery med hur hello API DocumentDB? | Microsoft Docs'
description: "Läs tooquery med hello DocumentDB API för Azure Cosmos DB"
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
ms.openlocfilehash: e3e5a49f7510942bcfb15330e5f86c5dd8b1e5d9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-cosmos-db-how-tooquery-with-api-for-mongodb"></a><span data-ttu-id="ac053-104">Azure Cosmos DB: Hur tooquery med API för MongoDB?</span><span class="sxs-lookup"><span data-stu-id="ac053-104">Azure Cosmos DB: How tooquery with API for MongoDB?</span></span>

<span data-ttu-id="ac053-105">hello Azure Cosmos DB [API för MongoDB](mongodb-introduction.md) stöder [MongoDB shell frågor](https://docs.mongodb.com/manual/tutorial/query-documents/).</span><span class="sxs-lookup"><span data-stu-id="ac053-105">hello Azure Cosmos DB [API for MongoDB](mongodb-introduction.md) supports [MongoDB shell queries](https://docs.mongodb.com/manual/tutorial/query-documents/).</span></span> 

<span data-ttu-id="ac053-106">Den här artikeln beskriver hello följande uppgifter:</span><span class="sxs-lookup"><span data-stu-id="ac053-106">This article covers hello following tasks:</span></span> 

> [!div class="checklist"]
> * <span data-ttu-id="ac053-107">Datafrågor med MongoDB</span><span class="sxs-lookup"><span data-stu-id="ac053-107">Querying data with MongoDB</span></span>

## <a name="sample-document"></a><span data-ttu-id="ac053-108">Exempel på ett dokument</span><span class="sxs-lookup"><span data-stu-id="ac053-108">Sample document</span></span>

<span data-ttu-id="ac053-109">hello frågorna i den här artikeln använder hello följande exempel-dokument.</span><span class="sxs-lookup"><span data-stu-id="ac053-109">hello queries in this article use hello following sample document.</span></span>

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
## <span data-ttu-id="ac053-110"><a id="examplequery1"></a>Exempelfråga 1</span><span class="sxs-lookup"><span data-stu-id="ac053-110"><a id="examplequery1"></a> Example query 1</span></span> 

<span data-ttu-id="ac053-111">Angivna hello exempel family dokument ovan hello följande fråga returnerar hello dokument där hello id-fältet matchar `WakefieldFamily`.</span><span class="sxs-lookup"><span data-stu-id="ac053-111">Given hello sample family document above, hello following query returns hello documents where hello id field matches `WakefieldFamily`.</span></span>

<span data-ttu-id="ac053-112">**Fråga**</span><span class="sxs-lookup"><span data-stu-id="ac053-112">**Query**</span></span>
    
    db.families.find({ id: “WakefieldFamily”})

<span data-ttu-id="ac053-113">**Resultat**</span><span class="sxs-lookup"><span data-stu-id="ac053-113">**Results**</span></span>

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

## <span data-ttu-id="ac053-114"><a id="examplequery2"></a>Exempelfråga 2</span><span class="sxs-lookup"><span data-stu-id="ac053-114"><a id="examplequery2"></a>Example query 2</span></span> 

<span data-ttu-id="ac053-115">hello nästa fråga returnerar alla hello i hello-serien.</span><span class="sxs-lookup"><span data-stu-id="ac053-115">hello next query returns all hello children in hello family.</span></span> 

<span data-ttu-id="ac053-116">**Fråga**</span><span class="sxs-lookup"><span data-stu-id="ac053-116">**Query**</span></span>
    
    db.familes.find( { id: “WakefieldFamily” }, { children: true } )

<span data-ttu-id="ac053-117">**Resultat**</span><span class="sxs-lookup"><span data-stu-id="ac053-117">**Results**</span></span>

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


## <span data-ttu-id="ac053-118"><a id="examplequery3"></a>Exempelfråga 3</span><span class="sxs-lookup"><span data-stu-id="ac053-118"><a id="examplequery3"></a>Example query 3</span></span> 

<span data-ttu-id="ac053-119">hello nästa fråga returnerar alla hello-familjer som är registrerad.</span><span class="sxs-lookup"><span data-stu-id="ac053-119">hello next query returns all hello families which are registered.</span></span> 

<span data-ttu-id="ac053-120">**Fråga**</span><span class="sxs-lookup"><span data-stu-id="ac053-120">**Query**</span></span>
    
    db.families.find( { "isRegistered" : true })
<span data-ttu-id="ac053-121">**Resultaten** inget dokument kommer att returneras.</span><span class="sxs-lookup"><span data-stu-id="ac053-121">**Results** No document will be returned.</span></span> 

## <span data-ttu-id="ac053-122"><a id="examplequery4"></a>Exempelfråga 4</span><span class="sxs-lookup"><span data-stu-id="ac053-122"><a id="examplequery4"></a>Example query 4</span></span>

<span data-ttu-id="ac053-123">hello nästa fråga returnerar alla hello-familjer som inte är registrerad.</span><span class="sxs-lookup"><span data-stu-id="ac053-123">hello next query returns all hello families which are not registered.</span></span> 

<span data-ttu-id="ac053-124">**Fråga**</span><span class="sxs-lookup"><span data-stu-id="ac053-124">**Query**</span></span>
    
    db.families.find( { "isRegistered" : false })
<span data-ttu-id="ac053-125">**Resultat**</span><span class="sxs-lookup"><span data-stu-id="ac053-125">**Results**</span></span>

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
<span data-ttu-id="ac053-126">}</span><span class="sxs-lookup"><span data-stu-id="ac053-126">}</span></span>

## <span data-ttu-id="ac053-127"><a id="examplequery5"></a>Exempelfråga 5</span><span class="sxs-lookup"><span data-stu-id="ac053-127"><a id="examplequery5"></a>Example query 5</span></span>

<span data-ttu-id="ac053-128">hello nästa fråga returnerar alla hello-familjer som inte är registrerade och tillstånd är NY.</span><span class="sxs-lookup"><span data-stu-id="ac053-128">hello next query returns all hello families which are not registered and state is NY.</span></span> 

<span data-ttu-id="ac053-129">**Fråga**</span><span class="sxs-lookup"><span data-stu-id="ac053-129">**Query**</span></span>
    
     db.families.find( { "isRegistered" : false, "address.state" : "NY" })

<span data-ttu-id="ac053-130">**Resultat**</span><span class="sxs-lookup"><span data-stu-id="ac053-130">**Results**</span></span>

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
<span data-ttu-id="ac053-131">}</span><span class="sxs-lookup"><span data-stu-id="ac053-131">}</span></span>


## <span data-ttu-id="ac053-132"><a id="examplequery6"></a>Exempelfråga 6</span><span class="sxs-lookup"><span data-stu-id="ac053-132"><a id="examplequery6"></a>Example query 6</span></span>

<span data-ttu-id="ac053-133">hello Returnerar nästa frågan alla hello familjer där underordnade klasser är 8.</span><span class="sxs-lookup"><span data-stu-id="ac053-133">hello next query returns all hello families where children grades are 8.</span></span>

<span data-ttu-id="ac053-134">**Fråga**</span><span class="sxs-lookup"><span data-stu-id="ac053-134">**Query**</span></span>
  
     db.families.find( { children : { $elemMatch: { grade : 8 }} } )

<span data-ttu-id="ac053-135">**Resultat**</span><span class="sxs-lookup"><span data-stu-id="ac053-135">**Results**</span></span>

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
<span data-ttu-id="ac053-136">}</span><span class="sxs-lookup"><span data-stu-id="ac053-136">}</span></span>

## <span data-ttu-id="ac053-137"><a id="examplequery7"></a>Exempelfråga 7</span><span class="sxs-lookup"><span data-stu-id="ac053-137"><a id="examplequery7"></a>Example query 7</span></span>

<span data-ttu-id="ac053-138">hello Returnerar nästa frågan alla hello familjer där storleken på underordnade matris är 3.</span><span class="sxs-lookup"><span data-stu-id="ac053-138">hello next query returns all hello families where size of children array is 3.</span></span>

<span data-ttu-id="ac053-139">**Fråga**</span><span class="sxs-lookup"><span data-stu-id="ac053-139">**Query**</span></span>
  
      db.Family.find( {children: { $size:3} } )

<span data-ttu-id="ac053-140">**Resultat**</span><span class="sxs-lookup"><span data-stu-id="ac053-140">**Results**</span></span>

<span data-ttu-id="ac053-141">Inga resultat returneras som vi inte har mer än 2 underordnade.</span><span class="sxs-lookup"><span data-stu-id="ac053-141">No results will be returned as we do not have more than 2 children.</span></span> <span data-ttu-id="ac053-142">Endast när parametern är 2 kommer den här frågan lyckas och returnerar hello hela dokumentet.</span><span class="sxs-lookup"><span data-stu-id="ac053-142">Only when parameter is 2 this query will succeed and return hello full document.</span></span>

## <a name="next-steps"></a><span data-ttu-id="ac053-143">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="ac053-143">Next steps</span></span>

<span data-ttu-id="ac053-144">I den här kursen har du gjort hello följande:</span><span class="sxs-lookup"><span data-stu-id="ac053-144">In this tutorial, you've done hello following:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="ac053-145">Lärt dig hur tooquery med MongoDB</span><span class="sxs-lookup"><span data-stu-id="ac053-145">Learned how tooquery using MongoDB</span></span> 

<span data-ttu-id="ac053-146">Du kan nu fortsätta toohello nästa självstudiekurs toolearn hur toodistribute data globalt.</span><span class="sxs-lookup"><span data-stu-id="ac053-146">You can now proceed toohello next tutorial toolearn how toodistribute your data globally.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="ac053-147">Distribuera dina data globalt</span><span class="sxs-lookup"><span data-stu-id="ac053-147">Distribute your data globally</span></span>](tutorial-global-distribution-documentdb.md)

