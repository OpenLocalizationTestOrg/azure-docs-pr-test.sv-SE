---
title: 'Azure Cosmos DB: skapa en app med Node.js och DocumentDB-API:n | Microsoft Docs'
description: "Presenterar ett .Node.js-kodexempel som du kan använda för att ansluta till och fråga Azure Cosmos DB DocumentDB-API:n"
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
ms.openlocfilehash: 26e3548bf6aacbc60c4c46a5cc88749ca14cec01
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="azure-cosmos-db-build-a-documentdb-api-app-with-nodejs-and-the-azure-portal"></a><span data-ttu-id="0b7cd-103">Azure Cosmos DB: skapa en DocumentDB-API-app med Node.js och Azure Portal</span><span class="sxs-lookup"><span data-stu-id="0b7cd-103">Azure Cosmos DB: Build a DocumentDB API app with Node.js and the Azure portal</span></span>

<span data-ttu-id="0b7cd-104">Azure Cosmos DB är Microsofts globalt distribuerade databastjänst för flera datamodeller.</span><span class="sxs-lookup"><span data-stu-id="0b7cd-104">Azure Cosmos DB is Microsoft’s globally distributed multi-model database service.</span></span> <span data-ttu-id="0b7cd-105">Du kan snabbt skapa och ställa frågor mot databaser med dokument, nyckel/värde-par och grafer. Du får fördelar av den globala distributionen och den horisontella skalningsförmågan som ligger i grunden hos Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="0b7cd-105">You can quickly create and query document, key/value, and graph databases, all of which benefit from the global distribution and horizontal scale capabilities at the core of Azure Cosmos DB.</span></span> 

<span data-ttu-id="0b7cd-106">I den här snabbstarten visas hur du skapar ett Azure Cosmos DB-konto, en dokumentdatabas och en samling med hjälp av Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="0b7cd-106">This quick start demonstrates how to create an Azure Cosmos DB account, document database, and collection using the Azure portal.</span></span> <span data-ttu-id="0b7cd-107">Sedan skapar du och kör en konsolapp som är byggd med [DocumentDB Node.js API](documentdb-sdk-node.md).</span><span class="sxs-lookup"><span data-stu-id="0b7cd-107">You then build and run a console app built on the [DocumentDB Node.js API](documentdb-sdk-node.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="0b7cd-108">Krav</span><span class="sxs-lookup"><span data-stu-id="0b7cd-108">Prerequisites</span></span>

* <span data-ttu-id="0b7cd-109">Innan du kan köra det här exemplet måste du uppfylla följande krav:</span><span class="sxs-lookup"><span data-stu-id="0b7cd-109">Before you can run this sample, you must have the following prerequisites:</span></span>
    * <span data-ttu-id="0b7cd-110">[Node.js](https://nodejs.org/en/) version v0.10.29 eller senare</span><span class="sxs-lookup"><span data-stu-id="0b7cd-110">[Node.js](https://nodejs.org/en/) version v0.10.29 or higher</span></span>
    * [<span data-ttu-id="0b7cd-111">Git</span><span class="sxs-lookup"><span data-stu-id="0b7cd-111">Git</span></span>](http://git-scm.com/)

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-a-database-account"></a><span data-ttu-id="0b7cd-112">Skapa ett databaskonto</span><span class="sxs-lookup"><span data-stu-id="0b7cd-112">Create a database account</span></span>

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

## <a name="add-a-collection"></a><span data-ttu-id="0b7cd-113">Lägga till en samling</span><span class="sxs-lookup"><span data-stu-id="0b7cd-113">Add a collection</span></span>

[!INCLUDE [cosmos-db-create-collection](../../includes/cosmos-db-create-collection.md)]

## <a name="clone-the-sample-application"></a><span data-ttu-id="0b7cd-114">Klona exempelprogrammet</span><span class="sxs-lookup"><span data-stu-id="0b7cd-114">Clone the sample application</span></span>

<span data-ttu-id="0b7cd-115">Nu ska vi klona en DocumentDB-API-app från github, ange anslutningssträngen och köra den.</span><span class="sxs-lookup"><span data-stu-id="0b7cd-115">Now let's clone a DocumentDB API app from github, set the connection string, and run it.</span></span> <span data-ttu-id="0b7cd-116">Du kommer att se hur lätt det är att arbeta med data programmässigt.</span><span class="sxs-lookup"><span data-stu-id="0b7cd-116">You see how easy it is to work with data programmatically.</span></span> 

1. <span data-ttu-id="0b7cd-117">Öppna ett git-terminalfönster, till exempel git bash, och `CD` till en arbetskatalog.</span><span class="sxs-lookup"><span data-stu-id="0b7cd-117">Open a git terminal window, such as git bash, and `CD` to a working directory.</span></span>  

2. <span data-ttu-id="0b7cd-118">Klona exempellagringsplatsen med följande kommando.</span><span class="sxs-lookup"><span data-stu-id="0b7cd-118">Run the following command to clone the sample repository.</span></span> 

    ```bash
    git clone https://github.com/Azure-Samples/azure-cosmos-db-documentdb-nodejs-getting-started.git
    ```

## <a name="review-the-code"></a><span data-ttu-id="0b7cd-119">Granska koden</span><span class="sxs-lookup"><span data-stu-id="0b7cd-119">Review the code</span></span>

<span data-ttu-id="0b7cd-120">Vi gör en snabb genomgång av vad som händer i appen.</span><span class="sxs-lookup"><span data-stu-id="0b7cd-120">Let's make a quick review of what's happening in the app.</span></span> <span data-ttu-id="0b7cd-121">Öppna filen `app.js` så ser du att de här kodraderna skapar Azure Cosmos DB-resurserna.</span><span class="sxs-lookup"><span data-stu-id="0b7cd-121">Open the `app.js` file and you find that these lines of code create the Azure Cosmos DB resources.</span></span> 

* <span data-ttu-id="0b7cd-122">`documentClient` har initierats.</span><span class="sxs-lookup"><span data-stu-id="0b7cd-122">The `documentClient` is initialized.</span></span>

    ```nodejs
    var client = new documentClient(config.endpoint, { "masterKey": config.primaryKey });
    ```

* <span data-ttu-id="0b7cd-123">En ny databas skapas.</span><span class="sxs-lookup"><span data-stu-id="0b7cd-123">A new database is created.</span></span>

    ```nodejs
    client.createDatabase(config.database, (err, created) => {
        if (err) reject(err)
        else resolve(created);
    });
    ```

* <span data-ttu-id="0b7cd-124">En ny samling skapas.</span><span class="sxs-lookup"><span data-stu-id="0b7cd-124">A new collection is created.</span></span>

    ```nodejs
    client.createCollection(databaseUrl, config.collection, { offerThroughput: 400 }, (err, created) => {
        if (err) reject(err)
        else resolve(created);
    });
    ```

* <span data-ttu-id="0b7cd-125">Vissa dokument skapas.</span><span class="sxs-lookup"><span data-stu-id="0b7cd-125">Some documents are created.</span></span>

    ```nodejs
    client.createDocument(collectionUrl, document, (err, created) => {
        if (err) reject(err)
        else resolve(created);
    });
    ```

* <span data-ttu-id="0b7cd-126">En SQL-fråga över JSON utförs.</span><span class="sxs-lookup"><span data-stu-id="0b7cd-126">A SQL query over JSON is performed.</span></span>

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

## <a name="update-your-connection-string"></a><span data-ttu-id="0b7cd-127">Uppdatera din anslutningssträng</span><span class="sxs-lookup"><span data-stu-id="0b7cd-127">Update your connection string</span></span>

<span data-ttu-id="0b7cd-128">Gå nu tillbaka till Azure Portal för att hämta information om din anslutningssträng och kopiera den till appen.</span><span class="sxs-lookup"><span data-stu-id="0b7cd-128">Now go back to the Azure portal to get your connection string information and copy it into the app.</span></span>

1. <span data-ttu-id="0b7cd-129">I [Azure Portal](http://portal.azure.com/) går du till ditt Azure Cosmos DB-konto. Klicka på **Nycklar** i det västra navigeringsfönstret och sedan på **läs- och skrivnycklar**.</span><span class="sxs-lookup"><span data-stu-id="0b7cd-129">In the [Azure portal](http://portal.azure.com/), in your Azure Cosmos DB account, in the left navigation click **Keys**, and then click **Read-write Keys**.</span></span> <span data-ttu-id="0b7cd-130">Du använder kopiera-knapparna till höger på skärmen för att kopiera URI:n och primärnyckeln i `config.js`-filen i nästa steg.</span><span class="sxs-lookup"><span data-stu-id="0b7cd-130">You'll use the copy buttons on the right side of the screen to copy the URI and Primary Key into the `config.js` file in the next step.</span></span>

    ![Visa och kopiera åtkomstnyckeln i Azure Portal, bladet Nycklar](./media/create-documentdb-dotnet/keys.png)

2. <span data-ttu-id="0b7cd-132">Öppna filen `config.js`.</span><span class="sxs-lookup"><span data-stu-id="0b7cd-132">In Open the `config.js` file.</span></span> 

3. <span data-ttu-id="0b7cd-133">Kopiera ditt URI-värde från portalen (med kopieringsknappen) och gör det till värdet för slutpunktsnyckeln i `config.js`.</span><span class="sxs-lookup"><span data-stu-id="0b7cd-133">Copy your URI value from the portal (using the copy button) and make it the value of the endpoint key in `config.js`.</span></span> 

    `config.endpoint = "https://FILLME.documents.azure.com"`

4. <span data-ttu-id="0b7cd-134">Kopiera sedan värdet för primärnyckeln från portalen och gör det till värdet för `config.primaryKey` i `config.js`.</span><span class="sxs-lookup"><span data-stu-id="0b7cd-134">Then copy your PRIMARY KEY value from the portal and make it the value of the `config.primaryKey` in `config.js`.</span></span> <span data-ttu-id="0b7cd-135">Du har nu uppdaterat appen med all information som behövs för kommunikation med Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="0b7cd-135">You've now updated your app with all the info it needs to communicate with Azure Cosmos DB.</span></span> 

    `config.primaryKey "FILLME"`
    
## <a name="run-the-app"></a><span data-ttu-id="0b7cd-136">Kör appen</span><span class="sxs-lookup"><span data-stu-id="0b7cd-136">Run the app</span></span>
1. <span data-ttu-id="0b7cd-137">Kör `npm install` i en terminal för att installera de npm-moduler som krävs</span><span class="sxs-lookup"><span data-stu-id="0b7cd-137">Run `npm install` in a terminal to install required npm modules</span></span>

2. <span data-ttu-id="0b7cd-138">Kör `node app.js` i en terminal för att starta nodprogrammet.</span><span class="sxs-lookup"><span data-stu-id="0b7cd-138">Run `node app.js` in a terminal to start your node application.</span></span>

<span data-ttu-id="0b7cd-139">Du kan nu gå tillbaka till datautforskaren och se frågan, ändra och arbeta med dessa nya data.</span><span class="sxs-lookup"><span data-stu-id="0b7cd-139">You can now go back to Data Explorer and see query, modify, and work with this new data.</span></span> 

## <a name="review-slas-in-the-azure-portal"></a><span data-ttu-id="0b7cd-140">Granska serviceavtal i Azure Portal</span><span class="sxs-lookup"><span data-stu-id="0b7cd-140">Review SLAs in the Azure portal</span></span>

[!INCLUDE [cosmosdb-tutorial-review-slas](../../includes/cosmos-db-tutorial-review-slas.md)]

## <a name="clean-up-resources"></a><span data-ttu-id="0b7cd-141">Rensa resurser</span><span class="sxs-lookup"><span data-stu-id="0b7cd-141">Clean up resources</span></span>

<span data-ttu-id="0b7cd-142">Om du inte planerar att fortsätta använda den här appen tar du bort alla resurser som skapades i snabbstarten i Azure Portal med följande steg:</span><span class="sxs-lookup"><span data-stu-id="0b7cd-142">If you're not going to continue to use this app, delete all resources created by this quickstart in the Azure portal with the following steps:</span></span>

1. <span data-ttu-id="0b7cd-143">Klicka på **Resursgrupper** på den vänstra menyn i Azure Portal och sedan på namnet på den resurs du skapade.</span><span class="sxs-lookup"><span data-stu-id="0b7cd-143">From the left-hand menu in the Azure portal, click **Resource groups** and then click the name of the resource you created.</span></span> 
2. <span data-ttu-id="0b7cd-144">På sidan med resursgrupper klickar du på **Ta bort**, skriver in namnet på resursen att ta bort i textrutan och klickar sedan på **Ta bort**.</span><span class="sxs-lookup"><span data-stu-id="0b7cd-144">On your resource group page, click **Delete**, type the name of the resource to delete in the text box, and then click **Delete**.</span></span>

## <a name="next-steps"></a><span data-ttu-id="0b7cd-145">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="0b7cd-145">Next steps</span></span>

<span data-ttu-id="0b7cd-146">I den här snabbstarten har du lärt dig hur man skapar ett Azure Cosmos DB-konto, skapar en samling med datautforskaren och kör en app.</span><span class="sxs-lookup"><span data-stu-id="0b7cd-146">In this quickstart, you've learned how to create an Azure Cosmos DB account, create a collection using the Data Explorer, and run an app.</span></span> <span data-ttu-id="0b7cd-147">Du kan nu importera ytterligare data till ditt Cosmos DB-konto.</span><span class="sxs-lookup"><span data-stu-id="0b7cd-147">You can now import additional data to your Cosmos DB account.</span></span> 

> [!div class="nextstepaction"]
> [<span data-ttu-id="0b7cd-148">Importera data till Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="0b7cd-148">Import data into Azure Cosmos DB</span></span>](import-data.md)


