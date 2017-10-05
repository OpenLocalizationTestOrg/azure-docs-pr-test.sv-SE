---
title: 'Azure Cosmos DB: Skapa en konsolapp med Java och MongoDB-API:t | Microsoft Docs'
description: "Presenterar ett Java-kodexempel som du kan använda för att ansluta till och ställa frågor via Azure Cosmos DB MongoDB-API:t"
services: cosmos-db
documentationcenter: 
author: mimig1
manager: jhubbard
editor: 
ms.assetid: 
ms.service: cosmos-db
ms.custom: quick start connect, mvc
ms.workload: 
ms.tgt_pltfrm: na
ms.devlang: java
ms.topic: hero-article
ms.date: 05/10/2017
ms.author: mimig
ms.openlocfilehash: f84294d7d324f094d173f7a2ec89759266a74210
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="azure-cosmos-db-build-a-mongodb-api-console-app-with-java-and-the-azure-portal"></a><span data-ttu-id="c1230-103">Azure Cosmos DB: Skapa en MongoDB API-konsolapp med Java och Azure Portal</span><span class="sxs-lookup"><span data-stu-id="c1230-103">Azure Cosmos DB: Build a MongoDB API console app with Java and the Azure portal</span></span>

<span data-ttu-id="c1230-104">Azure Cosmos DB är Microsofts globalt distribuerade databastjänst för flera datamodeller.</span><span class="sxs-lookup"><span data-stu-id="c1230-104">Azure Cosmos DB is Microsoft’s globally distributed multi-model database service.</span></span> <span data-ttu-id="c1230-105">Du kan snabbt skapa och ställa frågor mot databaser med dokument, nyckel/värde-par och grafer. Du får fördelar av den globala distributionen och den horisontella skalningsförmågan som ligger i grunden hos Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="c1230-105">You can quickly create and query document, key/value, and graph databases, all of which benefit from the global distribution and horizontal scale capabilities at the core of Azure Cosmos DB.</span></span> 

<span data-ttu-id="c1230-106">I den här snabbstarten visas hur du skapar ett Azure Cosmos DB-konto, en dokumentdatabas och en samling med hjälp av Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="c1230-106">This quick start demonstrates how to create an Azure Cosmos DB account, document database, and collection using the Azure portal.</span></span> <span data-ttu-id="c1230-107">Sedan kommer du att skapa och distribuera en konsolapp som är byggd med [MondoDB Java-drivrutinen](https://docs.mongodb.com/ecosystem/drivers/java/).</span><span class="sxs-lookup"><span data-stu-id="c1230-107">You'll then build and deploy a console app built on the [MongoDB Java driver](https://docs.mongodb.com/ecosystem/drivers/java/).</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="c1230-108">Krav</span><span class="sxs-lookup"><span data-stu-id="c1230-108">Prerequisites</span></span>

* <span data-ttu-id="c1230-109">Innan du kan köra det här exemplet måste du uppfylla följande krav:</span><span class="sxs-lookup"><span data-stu-id="c1230-109">Before you can run this sample, you must have the following prerequisites:</span></span>
   * <span data-ttu-id="c1230-110">JDK 1.7+ (Kör `apt-get install default-jdk` om du inte har JDK)</span><span class="sxs-lookup"><span data-stu-id="c1230-110">JDK 1.7+ (Run `apt-get install default-jdk` if you don't have JDK)</span></span>
   * <span data-ttu-id="c1230-111">Maven (Kör `apt-get install maven` om du inte har Maven)</span><span class="sxs-lookup"><span data-stu-id="c1230-111">Maven (Run `apt-get install maven` if you don't have Maven)</span></span>

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-a-database-account"></a><span data-ttu-id="c1230-112">Skapa ett databaskonto</span><span class="sxs-lookup"><span data-stu-id="c1230-112">Create a database account</span></span>

[!INCLUDE [mongodb-create-dbaccount](../../includes/cosmos-db-create-dbaccount-mongodb.md)]

## <a name="add-a-collection"></a><span data-ttu-id="c1230-113">Lägga till en samling</span><span class="sxs-lookup"><span data-stu-id="c1230-113">Add a collection</span></span>

<span data-ttu-id="c1230-114">Ge den nya databasen namnet **db** och den nya samlingen namnet **coll**.</span><span class="sxs-lookup"><span data-stu-id="c1230-114">Name your new database, **db**, and your new collection, **coll**.</span></span>

[!INCLUDE [cosmos-db-create-collection](../../includes/cosmos-db-create-collection.md)]

## <a name="clone-the-sample-application"></a><span data-ttu-id="c1230-115">Klona exempelprogrammet</span><span class="sxs-lookup"><span data-stu-id="c1230-115">Clone the sample application</span></span>

<span data-ttu-id="c1230-116">Nu ska vi klona en MongoDB API-app från github, ange anslutningssträngen och köra appen.</span><span class="sxs-lookup"><span data-stu-id="c1230-116">Now let's clone a MongoDB API app from github, set the connection string, and run it.</span></span> <span data-ttu-id="c1230-117">Du kommer att se hur lätt det är att arbeta med data programmässigt.</span><span class="sxs-lookup"><span data-stu-id="c1230-117">You'll see how easy it is to work with data programmatically.</span></span> 

1. <span data-ttu-id="c1230-118">Öppna ett git-terminalfönster, till exempel git bash, och `cd` till en arbetskatalog.</span><span class="sxs-lookup"><span data-stu-id="c1230-118">Open a git terminal window, such as git bash, and `cd` to a working directory.</span></span>  

2. <span data-ttu-id="c1230-119">Klona exempellagringsplatsen med följande kommando.</span><span class="sxs-lookup"><span data-stu-id="c1230-119">Run the following command to clone the sample repository.</span></span> 

    ```bash
    git clone https://github.com/Azure-Samples/azure-cosmos-db-mongodb-java-getting-started.git
    ```

3. <span data-ttu-id="c1230-120">Öppna därefter lösningsfilen i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="c1230-120">Then open the solution file in Visual Studio.</span></span> 

## <a name="review-the-code"></a><span data-ttu-id="c1230-121">Granska koden</span><span class="sxs-lookup"><span data-stu-id="c1230-121">Review the code</span></span>

<span data-ttu-id="c1230-122">Vi gör en snabb genomgång av vad som händer i appen.</span><span class="sxs-lookup"><span data-stu-id="c1230-122">Let's make a quick review of what's happening in the app.</span></span> <span data-ttu-id="c1230-123">Öppna filen `Program.cs` så ser du att de här kodraderna skapar Azure Cosmos DB-resurserna.</span><span class="sxs-lookup"><span data-stu-id="c1230-123">Open the `Program.cs` file and you'll find that these lines of code create the Azure Cosmos DB resources.</span></span> 

* <span data-ttu-id="c1230-124">DocumentClient initieras.</span><span class="sxs-lookup"><span data-stu-id="c1230-124">The DocumentClient is initialized.</span></span>

    ```java
    MongoClientURI uri = new MongoClientURI("FILLME");`

    MongoClient mongoClient = new MongoClient(uri);            
    ```

* <span data-ttu-id="c1230-125">En ny databas och samling skapas.</span><span class="sxs-lookup"><span data-stu-id="c1230-125">A new database and collection are created.</span></span>

    ```java
    MongoDatabase database = mongoClient.getDatabase("db");

    MongoCollection<Document> collection = database.getCollection("coll");
    ```

* <span data-ttu-id="c1230-126">Några dokument infogas med `MongoCollection.insertOne`</span><span class="sxs-lookup"><span data-stu-id="c1230-126">Some documents are inserted using `MongoCollection.insertOne`</span></span>

    ```java
    Document document = new Document("fruit", "apple")
    collection.insertOne(document);
    ```

* <span data-ttu-id="c1230-127">Några frågor körs med `MongoCollection.find`</span><span class="sxs-lookup"><span data-stu-id="c1230-127">Some queries are performed using `MongoCollection.find`</span></span>

    ```java
    Document queryResult = collection.find(Filters.eq("fruit", "apple")).first();
    System.out.println(queryResult.toJson());       
    ```

## <a name="update-your-connection-string"></a><span data-ttu-id="c1230-128">Uppdatera din anslutningssträng</span><span class="sxs-lookup"><span data-stu-id="c1230-128">Update your connection string</span></span>

<span data-ttu-id="c1230-129">Gå nu tillbaka till Azure Portal för att hämta information om din anslutningssträng och kopiera den till appen.</span><span class="sxs-lookup"><span data-stu-id="c1230-129">Now go back to the Azure portal to get your connection string information and copy it into the app.</span></span>

1. <span data-ttu-id="c1230-130">Från Konto väljer du **Snabbstart**, Java och kopierar sedan anslutningssträngen till Urklipp</span><span class="sxs-lookup"><span data-stu-id="c1230-130">From the Account, select **Quick Start**, select Java, then copy the connection string to your clipboard</span></span>

2. <span data-ttu-id="c1230-131">Öppna filen `Program.java` och ersätt argumentet för konstruktorn MongoClientURI med anslutningssträngen.</span><span class="sxs-lookup"><span data-stu-id="c1230-131">Open the `Program.java` file, replace the argument to the MongoClientURI constructor with the connection string.</span></span> <span data-ttu-id="c1230-132">Du har nu uppdaterat appen med all information som behövs för kommunikation med Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="c1230-132">You've now updated your app with all the info it needs to communicate with Azure Cosmos DB.</span></span> 
    
## <a name="run-the-console-app"></a><span data-ttu-id="c1230-133">Kör konsolappen</span><span class="sxs-lookup"><span data-stu-id="c1230-133">Run the console app</span></span>

1. <span data-ttu-id="c1230-134">Kör `mvn package` i en terminal för att installera de npm-moduler som krävs</span><span class="sxs-lookup"><span data-stu-id="c1230-134">Run `mvn package` in a terminal to install required npm modules</span></span>

2. <span data-ttu-id="c1230-135">Kör `mvn exec:java -D exec.mainClass=GetStarted.Program` i en terminal för att starta Java-programmet.</span><span class="sxs-lookup"><span data-stu-id="c1230-135">Run `mvn exec:java -D exec.mainClass=GetStarted.Program` in a terminal to start your Java application.</span></span>

<span data-ttu-id="c1230-136">Nu kan du använda [Robomongo](mongodb-robomongo.md) / [Studio 3T](mongodb-mongochef.md) till att ställa frågor mot, ändra och arbeta med dessa nya data.</span><span class="sxs-lookup"><span data-stu-id="c1230-136">You can now use [Robomongo](mongodb-robomongo.md) / [Studio 3T](mongodb-mongochef.md) to query, modify, and work with this new data.</span></span>

## <a name="review-slas-in-the-azure-portal"></a><span data-ttu-id="c1230-137">Granska serviceavtal i Azure Portal</span><span class="sxs-lookup"><span data-stu-id="c1230-137">Review SLAs in the Azure portal</span></span>

[!INCLUDE [cosmosdb-tutorial-review-slas](../../includes/cosmos-db-tutorial-review-slas.md)]

## <a name="clean-up-resources"></a><span data-ttu-id="c1230-138">Rensa resurser</span><span class="sxs-lookup"><span data-stu-id="c1230-138">Clean up resources</span></span>

<span data-ttu-id="c1230-139">Om du inte planerar att fortsätta använda den här appen tar du bort alla resurser som skapades i snabbstarten i Azure Portal med följande steg:</span><span class="sxs-lookup"><span data-stu-id="c1230-139">If you're not going to continue to use this app, delete all resources created by this quickstart in the Azure portal with the following steps:</span></span>

1. <span data-ttu-id="c1230-140">Klicka på **Resursgrupper** på den vänstra menyn i Azure Portal och sedan på namnet på den resurs du skapade.</span><span class="sxs-lookup"><span data-stu-id="c1230-140">From the left-hand menu in the Azure portal, click **Resource groups** and then click the name of the resource you created.</span></span> 
2. <span data-ttu-id="c1230-141">På sidan med resursgrupper klickar du på **Ta bort**, skriver in namnet på resursen att ta bort i textrutan och klickar sedan på **Ta bort**.</span><span class="sxs-lookup"><span data-stu-id="c1230-141">On your resource group page, click **Delete**, type the name of the resource to delete in the text box, and then click **Delete**.</span></span>

## <a name="next-steps"></a><span data-ttu-id="c1230-142">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="c1230-142">Next steps</span></span>

<span data-ttu-id="c1230-143">I den här snabbstarten har du lärt dig hur du skapar ett Azure Cosmos DB-konto, skapar en samling med Datautforskaren och kör en konsolapp.</span><span class="sxs-lookup"><span data-stu-id="c1230-143">In this quickstart, you've learned how to create an Azure Cosmos DB account, create a collection using the Data Explorer, and run a console app.</span></span> <span data-ttu-id="c1230-144">Du kan nu importera ytterligare data till ditt Cosmos DB-konto.</span><span class="sxs-lookup"><span data-stu-id="c1230-144">You can now import additional data to your Cosmos DB account.</span></span> 

> [!div class="nextstepaction"]
> [<span data-ttu-id="c1230-145">Importera MondoDB-data till Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="c1230-145">Import MongoDB data into Azure Cosmos DB</span></span>](mongodb-migrate.md)


