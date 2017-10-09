---
title: aaaUse MongoDB APIs toobuild appen Azure Cosmos DB | Microsoft Docs
description: "En självstudiekurs som skapar en onlinedatabas med hello Azure Cosmos DB API: er för MongoDB."
keywords: mongodb-exempel
services: cosmos-db
author: AndrewHoh
manager: jhubbard
editor: 
documentationcenter: 
ms.assetid: fb38bc53-3561-487d-9e03-20f232319a87
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/22/2017
ms.author: anhoh
ms.openlocfilehash: 09be4362fe3aac02e0163325f958210be9598383
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="build-an-azure-cosmos-db-api-for-mongodb-app-using-nodejs"></a>Skapa en Azure-Cosmos-DB: API: et för MongoDB-app med Node.js
> [!div class="op_single_selector"]
> * [.NET](documentdb-get-started.md)
> * [.NET Core](documentdb-dotnetcore-get-started.md)
> * [Java](documentdb-java-get-started.md)
> * [Node.js för MongoDB](mongodb-samples.md)
> * [Node.js](documentdb-nodejs-get-started.md)
> * [C++](documentdb-cpp-get-started.md)
>  
>

Det här exemplet visas hur toobuild en Azure-Cosmos-DB: API: et för MongoDB-konsolapp med Node.js.

toouse det här exemplet måste du:

* [Skapa](create-mongodb-dotnet.md#create-account) en Azure-Cosmos-DB: API för MongoDB-kontot.
* Hämta din MongoDB [anslutningssträngen](connect-mongodb-account.md) information.

## <a name="create-hello-app"></a>Skapa hello-app

1. Skapa en *app.js* filen och kopiera och klistra in hello koden nedan.

    ```nodejs
    var MongoClient = require('mongodb').MongoClient;
    var assert = require('assert');
    var ObjectId = require('mongodb').ObjectID;
    var url = 'mongodb://<endpoint>:<password>@<endpoint>.documents.azure.com:10255/?ssl=true';

    var insertDocument = function(db, callback) {
    db.collection('families').insertOne( {
            "id": "AndersenFamily",
            "lastName": "Andersen",
            "parents": [
                { "firstName": "Thomas" },
                { "firstName": "Mary Kay" }
            ],
            "children": [
                { "firstName": "John", "gender": "male", "grade": 7 }
            ],
            "pets": [
                { "givenName": "Fluffy" }
            ],
            "address": { "country": "USA", "state": "WA", "city": "Seattle" }
        }, function(err, result) {
        assert.equal(err, null);
        console.log("Inserted a document into hello families collection.");
        callback();
    });
    };
    
    var findFamilies = function(db, callback) {
    var cursor =db.collection('families').find( );
    cursor.each(function(err, doc) {
        assert.equal(err, null);
        if (doc != null) {
            console.dir(doc);
        } else {
            callback();
        }
    });
    };
    
    var updateFamilies = function(db, callback) {
    db.collection('families').updateOne(
        { "lastName" : "Andersen" },
        {
            $set: { "pets": [
                { "givenName": "Fluffy" },
                { "givenName": "Rocky"}
            ] },
            $currentDate: { "lastModified": true }
        }, function(err, results) {
        console.log(results);
        callback();
    });
    };
    
    var removeFamilies = function(db, callback) {
    db.collection('families').deleteMany(
        { "lastName": "Andersen" },
        function(err, results) {
            console.log(results);
            callback();
        }
    );
    };
    
    MongoClient.connect(url, function(err, db) {
    assert.equal(null, err);
    insertDocument(db, function() {
        findFamilies(db, function() {
        updateFamilies(db, function() {
            removeFamilies(db, function() {
                db.close();
            });
        });
        });
    });
    });
    ```

2. Ändra följande variabler i hello hello *app.js* fil per inställningarna för ditt konto (Lär dig hur toofind din [anslutningssträngen](connect-mongodb-account.md)):
   
    ```nodejs
    var url = 'mongodb://<endpoint>:<password>@<endpoint>.documents.azure.com:10255/?ssl=true';
    ```
     
3. Öppna valfri terminal, kör **npm installera mongodb--spara**, kör din app med **noden app.js**

## <a name="next-steps"></a>Nästa steg
* Lär dig hur för[använder MongoChef](mongodb-mongochef.md) med Azure Cosmos-DB: API för MongoDB-kontot.
