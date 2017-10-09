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
# <a name="azure-cosmos-db-how-tooquery-using-sql"></a>Azure Cosmos DB: Hur tooquery med hjälp av SQL?

hello Azure Cosmos DB [DocumentDB API](documentdb-introduction.md) stöder frågar dokument med hjälp av SQL. Den här artikeln innehåller ett exempel på dokument och två exempel SQL-frågor och resultat.

Den här artikeln beskriver hello följande uppgifter: 

> [!div class="checklist"]
> * Hämtning av data med SQL

## <a name="sample-document"></a>Exempel på ett dokument

hello SQL-frågor i den här artikeln använder hello följande exempel-dokument.

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
## <a name="where-can-i-run-sql-queries"></a>Var kan jag köra SQL-frågor?

Du kan köra frågor med hello Data Explorer i hello Azure-portalen via hello [REST-API och SDK](documentdb-sdk-dotnet.md), och även hello [Query playground](https://www.documentdb.com/sql/demo), som körs frågor på en befintlig uppsättning exempeldata.

Mer information om SQL-frågor finns:
* [SQL-fråga och SQL-syntax](documentdb-sql-query.md)

## <a name="prerequisites"></a>Krav

Den här kursen förutsätter att du har ett konto för Azure Cosmos DB och samling. Har inte något av de? Fullständig hello [5 minuter quickstart](create-mongodb-nodejs.md) eller hello [developer kursen](tutorial-develop-mongodb.md) toocreate ett konto och samling.

## <a name="example-query-1"></a>Exempelfråga 1

Hello exempel family dokument ovan får följande SQL-frågan returnerar hello dokument där hello id-fältet matchar `WakefieldFamily`. Eftersom det är en `SELECT *` hello utdata från frågan hello-instruktionen är hello fullständig JSON-dokumentet:

**Fråga**

    SELECT * 
    FROM Families f 
    WHERE f.id = "WakefieldFamily"

**Resultat**

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

## <a name="example-query-2"></a>Exempelfråga 2

hello nästa fråga returnerar alla hello angivet namn på barn i hello familj vars id matchar `WakefieldFamily` sorterade efter deras klass.

**Fråga**

    SELECT c.givenName 
    FROM Families f 
    JOIN c IN f.children 
    WHERE f.id = 'WakefieldFamily'
    ORDER BY f.children.grade ASC

**Resultat**

    [
      { "givenName": "Jesse" }, 
      { "givenName": "Lisa"}
    ]


## <a name="next-steps"></a>Nästa steg

I den här kursen har du gjort hello följande:

> [!div class="checklist"]
> * Lärt dig hur tooquery med SQL  

Du kan nu fortsätta toohello nästa självstudiekurs toolearn hur toodistribute data globalt.

> [!div class="nextstepaction"]
> [Distribuera dina data globalt](tutorial-global-distribution-documentdb.md)

