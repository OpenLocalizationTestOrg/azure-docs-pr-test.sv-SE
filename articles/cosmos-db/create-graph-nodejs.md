---
title: "aaaBuild ett Azure Cosmos DB Node.js-program med hjälp av Graph API | Microsoft Docs"
description: "Anger en Node.js-kodexempel som du kan använda tooconnect tooand fråga Azure Cosmos DB"
services: cosmos-db
documentationcenter: 
author: dennyglee
manager: jhubbard
editor: 
ms.assetid: daacbabf-1bb5-497f-92db-079910703046
ms.service: cosmos-db
ms.custom: quick start connect, mvc
ms.workload: 
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 07/14/2017
ms.author: denlee
ms.openlocfilehash: 1445755842bc4e4a84ca2b2f789aadde8467e190
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-cosmos-db-build-a-nodejs-application-by-using-graph-api"></a>Azure Cosmos DB: Skapa en Node.js-app med Graph API

Azure Cosmos-DB är hello globalt distribueras med flera modeller databastjänst från Microsoft. Du kan snabbt skapa och fråga dokument och nyckel/värde-diagrammet databaser, som omfattas av hello global distributionsplatsen och skala horisontellt funktionerna i hello kärnan i Azure Cosmos DB. 

Den här Snabbkurs artikeln visar hur toocreate en Azure-Cosmos-DB konto för Graph API (förhandsgranskning), databas och diagram med hjälp av hello Azure-portalen. Du sedan skapa och köra en konsolapp med hjälp av hello öppen källkod [Gremlin Node.js](https://www.npmjs.com/package/gremlin-secure) drivrutin.  

> [!NOTE]
> Hej npm-modulen `gremlin-secure` är en modifierad version av `gremlin` modul, med stöd för SSL och SASL som krävs för att ansluta med Azure Cosmos DB. Källkoden finns på [GitHub](https://github.com/CosmosDB/gremlin-javascript).
>

## <a name="prerequisites"></a>Krav

Innan du kan köra det här exemplet måste du ha hello följande krav:
* [Node.js](https://nodejs.org/en/)-version v0.10.29 eller senare
* [Git](http://git-scm.com/)

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-a-database-account"></a>Skapa ett databaskonto

[!INCLUDE [cosmos-db-create-dbaccount-graph](../../includes/cosmos-db-create-dbaccount-graph.md)]

## <a name="add-a-graph"></a>Lägga till en graf

[!INCLUDE [cosmos-db-create-graph](../../includes/cosmos-db-create-graph.md)]

## <a name="clone-hello-sample-application"></a>Klona hello exempelprogrammet

Nu ska vi klona Graph-API-app från GitHub, ange hello anslutningssträngen och kör den. Du ser hur enkelt som det är att toowork med data programmässigt. 

1. Öppna en Git-terminalfönster, till exempel Git Bash och ändra (via `cd` kommando) tooa arbetskatalogen.  

2. Hello kör följande kommando tooclone hello exempel lagringsplatsen. 

    ```bash
    git clone https://github.com/Azure-Samples/azure-cosmos-db-graph-nodejs-getting-started.git
    ```

3. Öppna hello lösningsfilen i Visual Studio. 

## <a name="review-hello-code"></a>Granska hello kod

Låt oss göra en snabb genomgång av vad som händer i hello app. Öppna hello `app.js` fil, och du hittar hello följande rader med kod. 

* Hej Gremlin klient skapas.

    ```nodejs
    const client = Gremlin.createClient(
        443, 
        config.endpoint, 
        { 
            "session": false, 
            "ssl": true, 
            "user": `/dbs/${config.database}/colls/${config.collection}`,
            "password": config.primaryKey
        });
    ```

  hello konfigurationer finns i `config.js`, som vi redigera i hello efter avsnittet.

* En serie Gremlin steg utförs med hello `client.execute` metod.

    ```nodejs
    console.log('Running Count'); 
    client.execute("g.V().count()", { }, (err, results) => {
        if (err) return console.error(err);
        console.log(JSON.stringify(results));
        console.log();
    });
    ```

## <a name="update-your-connection-string"></a>Uppdatera din anslutningssträng

1. Öppna hello config.js-fil. 

2. Fyll i hello config.endpoint nyckel med hello i config.js, **Gremlin URI** värde från hello **översikt** sidan hello Azure-portalen. 

    `config.endpoint = "GRAPHENDPOINT";`

    ![Visa och kopiera en snabbtangent i hello Azure-portalen, bladet nycklar](./media/create-graph-nodejs/gremlin-uri.png)

   Om hello **Gremlin URI** värdet är tomt, du kan generera hello värdet från hello **nycklar** sidan hello-portalen, med hello **URI** värde, ta bort https:// och ändra dokument toographs.

   Hej Gremlin slutpunkt måste vara endast hello värdnamn utan hello protocol/portnummer som `mygraphdb.graphs.azure.com` (inte `https://mygraphdb.graphs.azure.com` eller `mygraphdb.graphs.azure.com:433`).

3. I config.js, fyller du i hello config.primaryKey värdet in med hello **primärnyckel** värde från hello **nycklar** sidan hello Azure-portalen. 

    `config.primaryKey = "PRIMARYKEY";`

   ![hello Azure portalbladet nycklar](./media/create-graph-nodejs/keys.png)

4. Ange hello databasnamnet och (behållaren) diagramnamn för hello värdet av config.database och config.collection. 

Här är ett exempel på hur din färdiga config.js-fil ska se ut:

```nodejs
var config = {}

// Note that this must not have HTTPS or hello port number
config.endpoint = "testgraphacct.graphs.azure.com";
config.primaryKey = "Pams6e7LEUS7LJ2Qk0fjZf3eGo65JdMWHmyn65i52w8ozPX2oxY3iP0yu05t9v1WymAHNcMwPIqNAEv3XDFsEg==";
config.database = "graphdb"
config.collection = "Persons"

module.exports = config;
```

## <a name="run-hello-console-app"></a>Kör hello-konsolprogram

1. Öppna ett terminalfönster och ändra (via `cd` kommando) toohello installationskatalogen för hello package.json fil som ingår i hello-projekt.  

2. Kör `npm install` tooinstall hello krävs npm-modulerna, inklusive `gremlin-secure`.

3. Kör `node app.js` i en terminal toostart node-App.

## <a name="browse-with-data-explorer"></a>Bläddra med datautforskaren

Du kan nu gå tillbaka tooData Explorer i hello Azure portal tooview, fråga, ändra och arbeta med den nya informationen i diagrammet.

Data Explorer hello ny databas visas i hello **diagram** fönstret. Expandera hello-databasen, följt av hello samling, och klicka sedan på **diagram**.

hello-data som genereras av hello sample-appen visas i hello nästa ruta inom hello **diagram** fliken när du klickar på **Använd Filter**.

Försök att slutföra `g.V()` med `.has('firstName', 'Thomas')` tootest hello filter. Observera att värdet för hello är skiftlägeskänsliga.

## <a name="review-slas-in-hello-azure-portal"></a>Granska SLA: er i hello Azure-portalen

[!INCLUDE [cosmosdb-tutorial-review-slas](../../includes/cosmos-db-tutorial-review-slas.md)]

## <a name="clean-up-your-resources"></a>Rensa dina resurser

Om du inte planerar toocontinue som använder den här appen kan du ta bort alla resurser som du skapade i den här artikeln hello följande: 

1. Klicka på hello Azure-portalen på hello vänstra navigeringsmenyn **resursgrupper**, och klicka sedan på hello namnet på hello resursen som du skapade. 
2. På din resurs gruppen klickar du på **ta bort**hello typnamn för hello resurs toobe tas bort och klicka sedan på **ta bort**.

## <a name="next-steps"></a>Nästa steg

Du har lärt dig hur toocreate ett Azure DB som Cosmos-konto, skapa ett diagram med hjälp av Data Explorer och kör en app i den här artikeln. Nu kan du skapa mer komplexa frågor och implementera kraftfull logik för grafbläddring med Gremlin. 

> [!div class="nextstepaction"]
> [Fråga med hjälp av Gremlin](tutorial-query-graph.md)
