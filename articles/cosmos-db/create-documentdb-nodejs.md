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
# <a name="azure-cosmos-db-build-a-documentdb-api-app-with-nodejs-and-hello-azure-portal"></a><span data-ttu-id="fb3cd-103">Azure DB Cosmos: Skapa en DocumentDB-API-app med Node.js och hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="fb3cd-103">Azure Cosmos DB: Build a DocumentDB API app with Node.js and hello Azure portal</span></span>

<span data-ttu-id="fb3cd-104">Azure Cosmos DB är Microsofts globalt distribuerade databastjänst för flera datamodeller.</span><span class="sxs-lookup"><span data-stu-id="fb3cd-104">Azure Cosmos DB is Microsoft’s globally distributed multi-model database service.</span></span> <span data-ttu-id="fb3cd-105">Du kan snabbt skapa och fråga dokument och nyckel/värde-diagrammet databaser, som omfattas av hello global distributionsplatsen och skala horisontellt funktionerna i hello kärnan i Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="fb3cd-105">You can quickly create and query document, key/value, and graph databases, all of which benefit from hello global distribution and horizontal scale capabilities at hello core of Azure Cosmos DB.</span></span> 

<span data-ttu-id="fb3cd-106">Den här snabbstartsguide visar hur toocreate ett konto i Azure Cosmos DB, dokumentdatabasen och samlingen använder hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="fb3cd-106">This quick start demonstrates how toocreate an Azure Cosmos DB account, document database, and collection using hello Azure portal.</span></span> <span data-ttu-id="fb3cd-107">Du sedan skapa och köra en konsolapp som bygger på hello [DocumentDB Node.js API](documentdb-sdk-node.md).</span><span class="sxs-lookup"><span data-stu-id="fb3cd-107">You then build and run a console app built on hello [DocumentDB Node.js API](documentdb-sdk-node.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="fb3cd-108">Krav</span><span class="sxs-lookup"><span data-stu-id="fb3cd-108">Prerequisites</span></span>

* <span data-ttu-id="fb3cd-109">Innan du kan köra det här exemplet måste du ha hello följande krav:</span><span class="sxs-lookup"><span data-stu-id="fb3cd-109">Before you can run this sample, you must have hello following prerequisites:</span></span>
    * <span data-ttu-id="fb3cd-110">[Node.js](https://nodejs.org/en/) version v0.10.29 eller senare</span><span class="sxs-lookup"><span data-stu-id="fb3cd-110">[Node.js](https://nodejs.org/en/) version v0.10.29 or higher</span></span>
    * [<span data-ttu-id="fb3cd-111">Git</span><span class="sxs-lookup"><span data-stu-id="fb3cd-111">Git</span></span>](http://git-scm.com/)

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-a-database-account"></a><span data-ttu-id="fb3cd-112">Skapa ett databaskonto</span><span class="sxs-lookup"><span data-stu-id="fb3cd-112">Create a database account</span></span>

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

## <a name="add-a-collection"></a><span data-ttu-id="fb3cd-113">Lägga till en samling</span><span class="sxs-lookup"><span data-stu-id="fb3cd-113">Add a collection</span></span>

[!INCLUDE [cosmos-db-create-collection](../../includes/cosmos-db-create-collection.md)]

## <a name="clone-hello-sample-application"></a><span data-ttu-id="fb3cd-114">Klona hello exempelprogrammet</span><span class="sxs-lookup"><span data-stu-id="fb3cd-114">Clone hello sample application</span></span>

<span data-ttu-id="fb3cd-115">Nu ska vi klona en DocumentDB-API-app från github, ange hello anslutningssträngen och kör den.</span><span class="sxs-lookup"><span data-stu-id="fb3cd-115">Now let's clone a DocumentDB API app from github, set hello connection string, and run it.</span></span> <span data-ttu-id="fb3cd-116">Du ser hur enkelt är det toowork med data programmässigt.</span><span class="sxs-lookup"><span data-stu-id="fb3cd-116">You see how easy it is toowork with data programmatically.</span></span> 

1. <span data-ttu-id="fb3cd-117">Öppna ett git terminalfönster, till exempel git bash och `CD` tooa arbetskatalogen.</span><span class="sxs-lookup"><span data-stu-id="fb3cd-117">Open a git terminal window, such as git bash, and `CD` tooa working directory.</span></span>  

2. <span data-ttu-id="fb3cd-118">Hello kör följande kommando tooclone hello exempel lagringsplatsen.</span><span class="sxs-lookup"><span data-stu-id="fb3cd-118">Run hello following command tooclone hello sample repository.</span></span> 

    ```bash
    git clone https://github.com/Azure-Samples/azure-cosmos-db-documentdb-nodejs-getting-started.git
    ```

## <a name="review-hello-code"></a><span data-ttu-id="fb3cd-119">Granska hello kod</span><span class="sxs-lookup"><span data-stu-id="fb3cd-119">Review hello code</span></span>

<span data-ttu-id="fb3cd-120">Låt oss göra en snabb genomgång av vad som händer i hello app.</span><span class="sxs-lookup"><span data-stu-id="fb3cd-120">Let's make a quick review of what's happening in hello app.</span></span> <span data-ttu-id="fb3cd-121">Öppna hello `app.js` fil och du hittar att dessa rader med kod skapar hello Azure Cosmos DB resurser.</span><span class="sxs-lookup"><span data-stu-id="fb3cd-121">Open hello `app.js` file and you find that these lines of code create hello Azure Cosmos DB resources.</span></span> 

* <span data-ttu-id="fb3cd-122">Hej `documentClient` har initierats.</span><span class="sxs-lookup"><span data-stu-id="fb3cd-122">hello `documentClient` is initialized.</span></span>

    ```nodejs
    var client = new documentClient(config.endpoint, { "masterKey": config.primaryKey });
    ```

* <span data-ttu-id="fb3cd-123">En ny databas skapas.</span><span class="sxs-lookup"><span data-stu-id="fb3cd-123">A new database is created.</span></span>

    ```nodejs
    client.createDatabase(config.database, (err, created) => {
        if (err) reject(err)
        else resolve(created);
    });
    ```

* <span data-ttu-id="fb3cd-124">En ny samling skapas.</span><span class="sxs-lookup"><span data-stu-id="fb3cd-124">A new collection is created.</span></span>

    ```nodejs
    client.createCollection(databaseUrl, config.collection, { offerThroughput: 400 }, (err, created) => {
        if (err) reject(err)
        else resolve(created);
    });
    ```

* <span data-ttu-id="fb3cd-125">Vissa dokument skapas.</span><span class="sxs-lookup"><span data-stu-id="fb3cd-125">Some documents are created.</span></span>

    ```nodejs
    client.createDocument(collectionUrl, document, (err, created) => {
        if (err) reject(err)
        else resolve(created);
    });
    ```

* <span data-ttu-id="fb3cd-126">En SQL-fråga över JSON utförs.</span><span class="sxs-lookup"><span data-stu-id="fb3cd-126">A SQL query over JSON is performed.</span></span>

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

## <a name="update-your-connection-string"></a><span data-ttu-id="fb3cd-127">Uppdatera din anslutningssträng</span><span class="sxs-lookup"><span data-stu-id="fb3cd-127">Update your connection string</span></span>

<span data-ttu-id="fb3cd-128">Gå tillbaka toohello Azure portal tooget din Anslutningssträngsinformation nu och kopierar den till hello app.</span><span class="sxs-lookup"><span data-stu-id="fb3cd-128">Now go back toohello Azure portal tooget your connection string information and copy it into hello app.</span></span>

1. <span data-ttu-id="fb3cd-129">I hello [Azure-portalen](http://portal.azure.com/), i din Azure Cosmos DB konto, i hello vänstra navigeringsrutan klickar du på **nycklar**, och klicka sedan på **skrivskyddad nycklar**.</span><span class="sxs-lookup"><span data-stu-id="fb3cd-129">In hello [Azure portal](http://portal.azure.com/), in your Azure Cosmos DB account, in hello left navigation click **Keys**, and then click **Read-write Keys**.</span></span> <span data-ttu-id="fb3cd-130">Du måste använda hello kopiera knappar i hello höger sida av hello skärmen toocopy hello URI och primärnyckel i hello `config.js` filen i hello nästa steg.</span><span class="sxs-lookup"><span data-stu-id="fb3cd-130">You'll use hello copy buttons on hello right side of hello screen toocopy hello URI and Primary Key into hello `config.js` file in hello next step.</span></span>

    ![Visa och kopiera en snabbtangent i hello Azure-portalen, bladet nycklar](./media/create-documentdb-dotnet/keys.png)

2. <span data-ttu-id="fb3cd-132">I öppna hello `config.js` fil.</span><span class="sxs-lookup"><span data-stu-id="fb3cd-132">In Open hello `config.js` file.</span></span> 

3. <span data-ttu-id="fb3cd-133">Kopiera URI-värdet från hello-portal (med hello kopieringsknappen) och gör den hello värdet för hello endpoint nyckeln i `config.js`.</span><span class="sxs-lookup"><span data-stu-id="fb3cd-133">Copy your URI value from hello portal (using hello copy button) and make it hello value of hello endpoint key in `config.js`.</span></span> 

    `config.endpoint = "https://FILLME.documents.azure.com"`

4. <span data-ttu-id="fb3cd-134">Kopiera värdet för PRIMÄRNYCKELN från hello-portalen och gör det hello värdet för hello `config.primaryKey` i `config.js`.</span><span class="sxs-lookup"><span data-stu-id="fb3cd-134">Then copy your PRIMARY KEY value from hello portal and make it hello value of hello `config.primaryKey` in `config.js`.</span></span> <span data-ttu-id="fb3cd-135">Du har nu uppdaterat din app med alla hello information som behövs för toocommunicate med Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="fb3cd-135">You've now updated your app with all hello info it needs toocommunicate with Azure Cosmos DB.</span></span> 

    `config.primaryKey "FILLME"`
    
## <a name="run-hello-app"></a><span data-ttu-id="fb3cd-136">Kör hello-appen</span><span class="sxs-lookup"><span data-stu-id="fb3cd-136">Run hello app</span></span>
1. <span data-ttu-id="fb3cd-137">Kör `npm install` krävs npm-modulerna i en terminal tooinstall</span><span class="sxs-lookup"><span data-stu-id="fb3cd-137">Run `npm install` in a terminal tooinstall required npm modules</span></span>

2. <span data-ttu-id="fb3cd-138">Kör `node app.js` i en terminal toostart node-App.</span><span class="sxs-lookup"><span data-stu-id="fb3cd-138">Run `node app.js` in a terminal toostart your node application.</span></span>

<span data-ttu-id="fb3cd-139">Du kan nu gå tillbaka tooData Explorer Se fråga, ändra och arbeta med dessa nya data.</span><span class="sxs-lookup"><span data-stu-id="fb3cd-139">You can now go back tooData Explorer and see query, modify, and work with this new data.</span></span> 

## <a name="review-slas-in-hello-azure-portal"></a><span data-ttu-id="fb3cd-140">Granska SLA: er i hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="fb3cd-140">Review SLAs in hello Azure portal</span></span>

[!INCLUDE [cosmosdb-tutorial-review-slas](../../includes/cosmos-db-tutorial-review-slas.md)]

## <a name="clean-up-resources"></a><span data-ttu-id="fb3cd-141">Rensa resurser</span><span class="sxs-lookup"><span data-stu-id="fb3cd-141">Clean up resources</span></span>

<span data-ttu-id="fb3cd-142">Om du inte kommer toocontinue toouse den här appen, tar du bort alla resurser som skapats av denna Snabbstart i hello Azure-portalen med hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="fb3cd-142">If you're not going toocontinue toouse this app, delete all resources created by this quickstart in hello Azure portal with hello following steps:</span></span>

1. <span data-ttu-id="fb3cd-143">Hello vänstra menyn i hello Azure-portalen klickar du på **resursgrupper** och klicka sedan på hello namnet på hello resurs du skapat.</span><span class="sxs-lookup"><span data-stu-id="fb3cd-143">From hello left-hand menu in hello Azure portal, click **Resource groups** and then click hello name of hello resource you created.</span></span> 
2. <span data-ttu-id="fb3cd-144">På din resurs gruppen klickar du på **ta bort**typnamn hello för hello resurs toodelete i hello textrutan och klicka sedan på **ta bort**.</span><span class="sxs-lookup"><span data-stu-id="fb3cd-144">On your resource group page, click **Delete**, type hello name of hello resource toodelete in hello text box, and then click **Delete**.</span></span>

## <a name="next-steps"></a><span data-ttu-id="fb3cd-145">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="fb3cd-145">Next steps</span></span>

<span data-ttu-id="fb3cd-146">Du har lärt dig hur toocreate ett Azure DB som Cosmos-konto, skapa en samling med hello Data Explorer och kör en app i denna Snabbstart.</span><span class="sxs-lookup"><span data-stu-id="fb3cd-146">In this quickstart, you've learned how toocreate an Azure Cosmos DB account, create a collection using hello Data Explorer, and run an app.</span></span> <span data-ttu-id="fb3cd-147">Nu kan du importera ytterligare data tooyour Cosmos-DB-konto.</span><span class="sxs-lookup"><span data-stu-id="fb3cd-147">You can now import additional data tooyour Cosmos DB account.</span></span> 

> [!div class="nextstepaction"]
> [<span data-ttu-id="fb3cd-148">Importera data till Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="fb3cd-148">Import data into Azure Cosmos DB</span></span>](import-data.md)


