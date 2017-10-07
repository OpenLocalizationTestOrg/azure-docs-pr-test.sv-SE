---
title: 'Azure Cosmos DB: Skapa en app med Node.js och hello DocumentDB API | Microsoft Docs'
description: "Anger en Node.js-kodexempel som du kan använda tooconnect tooand query hello Azure Cosmos DB DocumentDB API"
services: cosmos-db
documentationcenter: 
author: mimig1
manager: jhubbard
editor: 
ms.assetid: 9c0f033c-240e-4fee-8421-08907231087f
ms.service: cosmos-db
ms.custom: quick start connect, mvc
ms.workload: 
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: hero-article
ms.date: 05/10/2017
ms.author: mimig
ms.openlocfilehash: 287d860c7d6f788f05a397b238ef0f841c3c30ca
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-cosmos-db-build-a-documentdb-api-app-with-nodejs-and-hello-azure-portal"></a>Azure DB Cosmos: Skapa en DocumentDB-API-app med Node.js och hello Azure-portalen

Azure Cosmos DB är Microsofts globalt distribuerade databastjänst för flera datamodeller. Du kan snabbt skapa och fråga dokument och nyckel/värde-diagrammet databaser, som omfattas av hello global distributionsplatsen och skala horisontellt funktionerna i hello kärnan i Azure Cosmos DB. 

Den här snabbstartsguide visar hur toocreate ett konto i Azure Cosmos DB, dokumentdatabasen och samlingen använder hello Azure-portalen. Du sedan skapa och köra en konsolapp som bygger på hello [DocumentDB Node.js API](documentdb-sdk-node.md).

## <a name="prerequisites"></a>Krav

* Innan du kan köra det här exemplet måste du ha hello följande krav:
    * [Node.js](https://nodejs.org/en/) version v0.10.29 eller senare
    * [Git](http://git-scm.com/)

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-a-database-account"></a>Skapa ett databaskonto

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

## <a name="add-a-collection"></a>Lägga till en samling

[!INCLUDE [cosmos-db-create-collection](../../includes/cosmos-db-create-collection.md)]

## <a name="clone-hello-sample-application"></a>Klona hello exempelprogrammet

Nu ska vi klona en DocumentDB-API-app från github, ange hello anslutningssträngen och kör den. Du ser hur enkelt är det toowork med data programmässigt. 

1. Öppna ett git terminalfönster, till exempel git bash och `CD` tooa arbetskatalogen.  

2. Hello kör följande kommando tooclone hello exempel lagringsplatsen. 

    ```bash
    git clone https://github.com/Azure-Samples/azure-cosmos-db-documentdb-nodejs-getting-started.git
    ```

## <a name="review-hello-code"></a>Granska hello kod

Låt oss göra en snabb genomgång av vad som händer i hello app. Öppna hello `app.js` fil och du hittar att dessa rader med kod skapar hello Azure Cosmos DB resurser. 

* Hej `documentClient` har initierats.

    ```nodejs
    var client = new documentClient(config.endpoint, { "masterKey": config.primaryKey });
    ```

* En ny databas skapas.

    ```nodejs
    client.createDatabase(config.database, (err, created) => {
        if (err) reject(err)
        else resolve(created);
    });
    ```

* En ny samling skapas.

    ```nodejs
    client.createCollection(databaseUrl, config.collection, { offerThroughput: 400 }, (err, created) => {
        if (err) reject(err)
        else resolve(created);
    });
    ```

* Vissa dokument skapas.

    ```nodejs
    client.createDocument(collectionUrl, document, (err, created) => {
        if (err) reject(err)
        else resolve(created);
    });
    ```

* En SQL-fråga över JSON utförs.

    ```nodejs
    client.queryDocuments(
        collectionUrl,
        'SELECT VALUE r.children FROM root r WHERE r.lastName = "Andersen"'
    ).toArray((err, results) => {
        if (err) reject(err)
        else {
            for (var queryResult of results) {
                let resultString = JSON.stringify(queryResult);
                console.log(`\tQuery returned ${resultString}`);
            }
            console.log();
            resolve(results);
        }
    });
    ```    

## <a name="update-your-connection-string"></a>Uppdatera din anslutningssträng

Gå tillbaka toohello Azure portal tooget din Anslutningssträngsinformation nu och kopierar den till hello app.

1. I hello [Azure-portalen](http://portal.azure.com/), i din Azure Cosmos DB konto, i hello vänstra navigeringsrutan klickar du på **nycklar**, och klicka sedan på **skrivskyddad nycklar**. Du måste använda hello kopiera knappar i hello höger sida av hello skärmen toocopy hello URI och primärnyckel i hello `config.js` filen i hello nästa steg.

    ![Visa och kopiera en snabbtangent i hello Azure-portalen, bladet nycklar](./media/create-documentdb-dotnet/keys.png)

2. I öppna hello `config.js` fil. 

3. Kopiera URI-värdet från hello-portal (med hello kopieringsknappen) och gör den hello värdet för hello endpoint nyckeln i `config.js`. 

    `config.endpoint = "https://FILLME.documents.azure.com"`

4. Kopiera värdet för PRIMÄRNYCKELN från hello-portalen och gör det hello värdet för hello `config.primaryKey` i `config.js`. Du har nu uppdaterat din app med alla hello information som behövs för toocommunicate med Azure Cosmos DB. 

    `config.primaryKey "FILLME"`
    
## <a name="run-hello-app"></a>Kör hello-appen
1. Kör `npm install` krävs npm-modulerna i en terminal tooinstall

2. Kör `node app.js` i en terminal toostart node-App.

Du kan nu gå tillbaka tooData Explorer Se fråga, ändra och arbeta med dessa nya data. 

## <a name="review-slas-in-hello-azure-portal"></a>Granska SLA: er i hello Azure-portalen

[!INCLUDE [cosmosdb-tutorial-review-slas](../../includes/cosmos-db-tutorial-review-slas.md)]

## <a name="clean-up-resources"></a>Rensa resurser

Om du inte kommer toocontinue toouse den här appen, tar du bort alla resurser som skapats av denna Snabbstart i hello Azure-portalen med hello följande steg:

1. Hello vänstra menyn i hello Azure-portalen klickar du på **resursgrupper** och klicka sedan på hello namnet på hello resurs du skapat. 
2. På din resurs gruppen klickar du på **ta bort**typnamn hello för hello resurs toodelete i hello textrutan och klicka sedan på **ta bort**.

## <a name="next-steps"></a>Nästa steg

Du har lärt dig hur toocreate ett Azure DB som Cosmos-konto, skapa en samling med hello Data Explorer och kör en app i denna Snabbstart. Nu kan du importera ytterligare data tooyour Cosmos-DB-konto. 

> [!div class="nextstepaction"]
> [Importera data till Azure Cosmos DB](import-data.md)


