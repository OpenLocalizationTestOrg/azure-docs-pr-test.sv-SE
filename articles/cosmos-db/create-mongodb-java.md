---
title: 'Azure Cosmos DB: Skapa en konsolapp med Java och hello MongoDB API | Microsoft Docs'
description: "Anger en Java-kodexempel som du kan använda tooconnect tooand query hello Azure Cosmos DB MongoDB API"
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
ms.openlocfilehash: fbe416f6b20ed2bb83a1d41eb70ffc6e3cee2b61
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-cosmos-db-build-a-mongodb-api-console-app-with-java-and-hello-azure-portal"></a><span data-ttu-id="9dd87-103">Azure DB Cosmos: Skapa en MongoDB-API-konsolapp med Java och hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="9dd87-103">Azure Cosmos DB: Build a MongoDB API console app with Java and hello Azure portal</span></span>

<span data-ttu-id="9dd87-104">Azure Cosmos DB är Microsofts globalt distribuerade databastjänst för flera datamodeller.</span><span class="sxs-lookup"><span data-stu-id="9dd87-104">Azure Cosmos DB is Microsoft’s globally distributed multi-model database service.</span></span> <span data-ttu-id="9dd87-105">Du kan snabbt skapa och fråga dokument och nyckel/värde-diagrammet databaser, som omfattas av hello global distributionsplatsen och skala horisontellt funktionerna i hello kärnan i Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="9dd87-105">You can quickly create and query document, key/value, and graph databases, all of which benefit from hello global distribution and horizontal scale capabilities at hello core of Azure Cosmos DB.</span></span> 

<span data-ttu-id="9dd87-106">Den här snabbstartsguide visar hur toocreate ett konto i Azure Cosmos DB, dokumentdatabasen och samlingen använder hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="9dd87-106">This quick start demonstrates how toocreate an Azure Cosmos DB account, document database, and collection using hello Azure portal.</span></span> <span data-ttu-id="9dd87-107">Du kan sedan skapa och distribuera en konsolapp som bygger på hello [MongoDB Java drivrutinen](https://docs.mongodb.com/ecosystem/drivers/java/).</span><span class="sxs-lookup"><span data-stu-id="9dd87-107">You'll then build and deploy a console app built on hello [MongoDB Java driver](https://docs.mongodb.com/ecosystem/drivers/java/).</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="9dd87-108">Krav</span><span class="sxs-lookup"><span data-stu-id="9dd87-108">Prerequisites</span></span>

* <span data-ttu-id="9dd87-109">Innan du kan köra det här exemplet måste du ha hello följande krav:</span><span class="sxs-lookup"><span data-stu-id="9dd87-109">Before you can run this sample, you must have hello following prerequisites:</span></span>
   * <span data-ttu-id="9dd87-110">JDK 1.7+ (Kör `apt-get install default-jdk` om du inte har JDK)</span><span class="sxs-lookup"><span data-stu-id="9dd87-110">JDK 1.7+ (Run `apt-get install default-jdk` if you don't have JDK)</span></span>
   * <span data-ttu-id="9dd87-111">Maven (Kör `apt-get install maven` om du inte har Maven)</span><span class="sxs-lookup"><span data-stu-id="9dd87-111">Maven (Run `apt-get install maven` if you don't have Maven)</span></span>

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-a-database-account"></a><span data-ttu-id="9dd87-112">Skapa ett databaskonto</span><span class="sxs-lookup"><span data-stu-id="9dd87-112">Create a database account</span></span>

[!INCLUDE [mongodb-create-dbaccount](../../includes/cosmos-db-create-dbaccount-mongodb.md)]

## <a name="add-a-collection"></a><span data-ttu-id="9dd87-113">Lägga till en samling</span><span class="sxs-lookup"><span data-stu-id="9dd87-113">Add a collection</span></span>

<span data-ttu-id="9dd87-114">Ge den nya databasen namnet **db** och den nya samlingen namnet **coll**.</span><span class="sxs-lookup"><span data-stu-id="9dd87-114">Name your new database, **db**, and your new collection, **coll**.</span></span>

[!INCLUDE [cosmos-db-create-collection](../../includes/cosmos-db-create-collection.md)]

## <a name="clone-hello-sample-application"></a><span data-ttu-id="9dd87-115">Klona hello exempelprogrammet</span><span class="sxs-lookup"><span data-stu-id="9dd87-115">Clone hello sample application</span></span>

<span data-ttu-id="9dd87-116">Nu ska vi klona en MongoDB-API-app från github, ange hello anslutningssträngen och kör den.</span><span class="sxs-lookup"><span data-stu-id="9dd87-116">Now let's clone a MongoDB API app from github, set hello connection string, and run it.</span></span> <span data-ttu-id="9dd87-117">Du ser hur enkelt som det är att toowork med data programmässigt.</span><span class="sxs-lookup"><span data-stu-id="9dd87-117">You'll see how easy it is toowork with data programmatically.</span></span> 

1. <span data-ttu-id="9dd87-118">Öppna ett git terminalfönster, till exempel git bash och `cd` tooa arbetskatalogen.</span><span class="sxs-lookup"><span data-stu-id="9dd87-118">Open a git terminal window, such as git bash, and `cd` tooa working directory.</span></span>  

2. <span data-ttu-id="9dd87-119">Hello kör följande kommando tooclone hello exempel lagringsplatsen.</span><span class="sxs-lookup"><span data-stu-id="9dd87-119">Run hello following command tooclone hello sample repository.</span></span> 

    ```bash
    git clone https://github.com/Azure-Samples/azure-cosmos-db-mongodb-java-getting-started.git
    ```

3. <span data-ttu-id="9dd87-120">Öppna sedan hello lösningsfilen i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="9dd87-120">Then open hello solution file in Visual Studio.</span></span> 

## <a name="review-hello-code"></a><span data-ttu-id="9dd87-121">Granska hello kod</span><span class="sxs-lookup"><span data-stu-id="9dd87-121">Review hello code</span></span>

<span data-ttu-id="9dd87-122">Låt oss göra en snabb genomgång av vad som händer i hello app.</span><span class="sxs-lookup"><span data-stu-id="9dd87-122">Let's make a quick review of what's happening in hello app.</span></span> <span data-ttu-id="9dd87-123">Öppna hello `Program.cs` fil- och du hittar att dessa rader med kod skapar hello Azure Cosmos DB resurser.</span><span class="sxs-lookup"><span data-stu-id="9dd87-123">Open hello `Program.cs` file and you'll find that these lines of code create hello Azure Cosmos DB resources.</span></span> 

* <span data-ttu-id="9dd87-124">Hej DocumentClient har initierats.</span><span class="sxs-lookup"><span data-stu-id="9dd87-124">hello DocumentClient is initialized.</span></span>

    ```java
    MongoClientURI uri = new MongoClientURI("FILLME");`

    MongoClient mongoClient = new MongoClient(uri);            
    ```

* <span data-ttu-id="9dd87-125">En ny databas och samling skapas.</span><span class="sxs-lookup"><span data-stu-id="9dd87-125">A new database and collection are created.</span></span>

    ```java
    MongoDatabase database = mongoClient.getDatabase("db");

    MongoCollection<Document> collection = database.getCollection("coll");
    ```

* <span data-ttu-id="9dd87-126">Några dokument infogas med `MongoCollection.insertOne`</span><span class="sxs-lookup"><span data-stu-id="9dd87-126">Some documents are inserted using `MongoCollection.insertOne`</span></span>

    ```java
    Document document = new Document("fruit", "apple")
    collection.insertOne(document);
    ```

* <span data-ttu-id="9dd87-127">Några frågor körs med `MongoCollection.find`</span><span class="sxs-lookup"><span data-stu-id="9dd87-127">Some queries are performed using `MongoCollection.find`</span></span>

    ```java
    Document queryResult = collection.find(Filters.eq("fruit", "apple")).first();
    System.out.println(queryResult.toJson());       
    ```

## <a name="update-your-connection-string"></a><span data-ttu-id="9dd87-128">Uppdatera din anslutningssträng</span><span class="sxs-lookup"><span data-stu-id="9dd87-128">Update your connection string</span></span>

<span data-ttu-id="9dd87-129">Gå tillbaka toohello Azure portal tooget din Anslutningssträngsinformation nu och kopierar den till hello app.</span><span class="sxs-lookup"><span data-stu-id="9dd87-129">Now go back toohello Azure portal tooget your connection string information and copy it into hello app.</span></span>

1. <span data-ttu-id="9dd87-130">Hello konto, Välj **Snabbstart**, Välj Java och sedan kopiera hello anslutning sträng tooyour Urklipp</span><span class="sxs-lookup"><span data-stu-id="9dd87-130">From hello Account, select **Quick Start**, select Java, then copy hello connection string tooyour clipboard</span></span>

2. <span data-ttu-id="9dd87-131">Öppna hello `Program.java` fil, ersätta hello argumentet toohello MongoClientURI konstruktor med hello anslutningssträng.</span><span class="sxs-lookup"><span data-stu-id="9dd87-131">Open hello `Program.java` file, replace hello argument toohello MongoClientURI constructor with hello connection string.</span></span> <span data-ttu-id="9dd87-132">Du har nu uppdaterat din app med alla hello information som behövs för toocommunicate med Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="9dd87-132">You've now updated your app with all hello info it needs toocommunicate with Azure Cosmos DB.</span></span> 
    
## <a name="run-hello-console-app"></a><span data-ttu-id="9dd87-133">Kör hello-konsolprogram</span><span class="sxs-lookup"><span data-stu-id="9dd87-133">Run hello console app</span></span>

1. <span data-ttu-id="9dd87-134">Kör `mvn package` krävs npm-modulerna i en terminal tooinstall</span><span class="sxs-lookup"><span data-stu-id="9dd87-134">Run `mvn package` in a terminal tooinstall required npm modules</span></span>

2. <span data-ttu-id="9dd87-135">Kör `mvn exec:java -D exec.mainClass=GetStarted.Program` i en terminal toostart Java-programmet.</span><span class="sxs-lookup"><span data-stu-id="9dd87-135">Run `mvn exec:java -D exec.mainClass=GetStarted.Program` in a terminal toostart your Java application.</span></span>

<span data-ttu-id="9dd87-136">Du kan nu använda [Robomongo](mongodb-robomongo.md) / [Studio 3T](mongodb-mongochef.md) tooquery, ändra och arbeta med dessa nya data.</span><span class="sxs-lookup"><span data-stu-id="9dd87-136">You can now use [Robomongo](mongodb-robomongo.md) / [Studio 3T](mongodb-mongochef.md) tooquery, modify, and work with this new data.</span></span>

## <a name="review-slas-in-hello-azure-portal"></a><span data-ttu-id="9dd87-137">Granska SLA: er i hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="9dd87-137">Review SLAs in hello Azure portal</span></span>

[!INCLUDE [cosmosdb-tutorial-review-slas](../../includes/cosmos-db-tutorial-review-slas.md)]

## <a name="clean-up-resources"></a><span data-ttu-id="9dd87-138">Rensa resurser</span><span class="sxs-lookup"><span data-stu-id="9dd87-138">Clean up resources</span></span>

<span data-ttu-id="9dd87-139">Om du inte kommer toocontinue toouse den här appen, tar du bort alla resurser som skapats av denna Snabbstart i hello Azure-portalen med hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="9dd87-139">If you're not going toocontinue toouse this app, delete all resources created by this quickstart in hello Azure portal with hello following steps:</span></span>

1. <span data-ttu-id="9dd87-140">Hello vänstra menyn i hello Azure-portalen klickar du på **resursgrupper** och klicka sedan på hello namnet på hello resurs du skapat.</span><span class="sxs-lookup"><span data-stu-id="9dd87-140">From hello left-hand menu in hello Azure portal, click **Resource groups** and then click hello name of hello resource you created.</span></span> 
2. <span data-ttu-id="9dd87-141">På din resurs gruppen klickar du på **ta bort**typnamn hello för hello resurs toodelete i hello textrutan och klicka sedan på **ta bort**.</span><span class="sxs-lookup"><span data-stu-id="9dd87-141">On your resource group page, click **Delete**, type hello name of hello resource toodelete in hello text box, and then click **Delete**.</span></span>

## <a name="next-steps"></a><span data-ttu-id="9dd87-142">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="9dd87-142">Next steps</span></span>

<span data-ttu-id="9dd87-143">Du har lärt dig hur toocreate ett Azure DB som Cosmos-konto, skapa en samling med hello Data Explorer och kör en konsolapp i denna Snabbstart.</span><span class="sxs-lookup"><span data-stu-id="9dd87-143">In this quickstart, you've learned how toocreate an Azure Cosmos DB account, create a collection using hello Data Explorer, and run a console app.</span></span> <span data-ttu-id="9dd87-144">Nu kan du importera ytterligare data tooyour Cosmos-DB-konto.</span><span class="sxs-lookup"><span data-stu-id="9dd87-144">You can now import additional data tooyour Cosmos DB account.</span></span> 

> [!div class="nextstepaction"]
> [<span data-ttu-id="9dd87-145">Importera MongoDB-data till Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="9dd87-145">Import MongoDB data into Azure Cosmos DB</span></span>](mongodb-migrate.md)


