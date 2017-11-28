---
title: aaaCreate en databas med Azure Cosmos DB dokument med Java | Microsoft Docs | Microsoft Docs
description: "Anger en Java-kodexempel som du kan använda tooconnect tooand query hello Azure Cosmos DB DocumentDB API"
services: cosmos-db
documentationcenter: 
author: mimig1
manager: jhubbard
editor: 
ms.assetid: 89ea62bb-c620-46d5-baa0-eefd9888557c
ms.service: cosmos-db
ms.custom: quick start connect, mvc
ms.workload: 
ms.tgt_pltfrm: na
ms.devlang: java
ms.topic: hero-article
ms.date: 08/02/2017
ms.author: mimig
ms.openlocfilehash: 400c9e7780034d3e28d749e734786e950edad22f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-cosmos-db-create-a-document-database-using-java-and-hello-azure-portal"></a><span data-ttu-id="3dc30-103">Azure DB Cosmos: Skapa en databas för dokument som använder Java och hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="3dc30-103">Azure Cosmos DB: Create a document database using Java and hello Azure portal</span></span>

<span data-ttu-id="3dc30-104">Azure Cosmos DB är Microsofts globalt distribuerade databastjänst för flera datamodeller.</span><span class="sxs-lookup"><span data-stu-id="3dc30-104">Azure Cosmos DB is Microsoft’s globally distributed multi-model database service.</span></span> <span data-ttu-id="3dc30-105">Du kan snabbt skapa och fråga dokument och nyckel/värde-diagrammet databaser, som omfattas av hello global distributionsplatsen och skala horisontellt funktionerna i hello kärnan i Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="3dc30-105">You can quickly create and query document, key/value, and graph databases, all of which benefit from hello global distribution and horizontal scale capabilities at hello core of Azure Cosmos DB.</span></span> 

<span data-ttu-id="3dc30-106">Denna Snabbstart skapar ett dokument hello databas med hjälp av Azure portal-verktyg för Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="3dc30-106">This quickstart creates a document database using hello Azure portal tools for Azure Cosmos DB.</span></span> <span data-ttu-id="3dc30-107">Den här snabbstarten visar också hur tooquickly skapar en Java-konsolapp med hello [DocumentDB Java API](documentdb-sdk-java.md).</span><span class="sxs-lookup"><span data-stu-id="3dc30-107">This quickstart also shows you how tooquickly create a Java console app using hello [DocumentDB Java API](documentdb-sdk-java.md).</span></span> <span data-ttu-id="3dc30-108">hello anvisningarna i den här snabbstarten kan tillämpas på alla operativsystem som kan köra Java.</span><span class="sxs-lookup"><span data-stu-id="3dc30-108">hello instructions in this quickstart can be followed on any operating system that is capable of running Java.</span></span> <span data-ttu-id="3dc30-109">Genom att slutföra den här snabbstarten du är bekant med att skapa och ändra dokumentet databasresurser i hello Användargränssnittet eller genom att programmera, beroende på vilket som är dina inställningar.</span><span class="sxs-lookup"><span data-stu-id="3dc30-109">By completing this quickstart you'll be familiar with creating and modifying document database resources in either hello UI or programatically, whichever is your preference.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="3dc30-110">Krav</span><span class="sxs-lookup"><span data-stu-id="3dc30-110">Prerequisites</span></span>

* [<span data-ttu-id="3dc30-111">Java Development Kit (JDK) 1.7+</span><span class="sxs-lookup"><span data-stu-id="3dc30-111">Java Development Kit (JDK) 1.7+</span></span>](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)
    * <span data-ttu-id="3dc30-112">Ubuntu, kör `apt-get install default-jdk` tooinstall hello JDK.</span><span class="sxs-lookup"><span data-stu-id="3dc30-112">On Ubuntu, run `apt-get install default-jdk` tooinstall hello JDK.</span></span>
    * <span data-ttu-id="3dc30-113">Vara säker på att tooset hello JAVA_HOME miljö variabeln toopoint toohello mapp där hello JDK är installerad.</span><span class="sxs-lookup"><span data-stu-id="3dc30-113">Be sure tooset hello JAVA_HOME environment variable toopoint toohello folder where hello JDK is installed.</span></span>
* <span data-ttu-id="3dc30-114">[Ladda ned](http://maven.apache.org/download.cgi) och [installera](http://maven.apache.org/install.html) ett [Maven](http://maven.apache.org/)-binärarkiv</span><span class="sxs-lookup"><span data-stu-id="3dc30-114">[Download](http://maven.apache.org/download.cgi) and [install](http://maven.apache.org/install.html) a [Maven](http://maven.apache.org/) binary archive</span></span>
    * <span data-ttu-id="3dc30-115">Ubuntu, du kan köra `apt-get install maven` tooinstall Maven.</span><span class="sxs-lookup"><span data-stu-id="3dc30-115">On Ubuntu, you can run `apt-get install maven` tooinstall Maven.</span></span>
* [<span data-ttu-id="3dc30-116">Git</span><span class="sxs-lookup"><span data-stu-id="3dc30-116">Git</span></span>](https://www.git-scm.com/)
    * <span data-ttu-id="3dc30-117">Ubuntu, du kan köra `sudo apt-get install git` tooinstall Git.</span><span class="sxs-lookup"><span data-stu-id="3dc30-117">On Ubuntu, you can run `sudo apt-get install git` tooinstall Git.</span></span>

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-a-database-account"></a><span data-ttu-id="3dc30-118">Skapa ett databaskonto</span><span class="sxs-lookup"><span data-stu-id="3dc30-118">Create a database account</span></span>

<span data-ttu-id="3dc30-119">Innan du kan skapa en dokumentdatabasen måste toocreate ett databaskonto i SQL (DocumentDB) med Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="3dc30-119">Before you can create a document database, you need toocreate a SQL (DocumentDB) database account with Azure Cosmos DB.</span></span>

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

## <a name="add-a-collection"></a><span data-ttu-id="3dc30-120">Lägga till en samling</span><span class="sxs-lookup"><span data-stu-id="3dc30-120">Add a collection</span></span>

[!INCLUDE [cosmos-db-create-collection](../../includes/cosmos-db-create-collection.md)]

<a id="add-sample-data"></a>
## <a name="add-sample-data"></a><span data-ttu-id="3dc30-121">Lägg till exempeldata</span><span class="sxs-lookup"><span data-stu-id="3dc30-121">Add sample data</span></span>

<span data-ttu-id="3dc30-122">Du kan nu lägga till tooyour nya insamling av data med hjälp av Data Explorer.</span><span class="sxs-lookup"><span data-stu-id="3dc30-122">You can now add data tooyour new collection using Data Explorer.</span></span>

1. <span data-ttu-id="3dc30-123">I Data Explorer visas hello ny databas i hello samlingar rutan.</span><span class="sxs-lookup"><span data-stu-id="3dc30-123">In Data Explorer, hello new database appears in hello Collections pane.</span></span> <span data-ttu-id="3dc30-124">Expandera hello **uppgifter** databas, expandera hello **objekt** samlingen, klickar du på **dokument**, och klicka sedan på **nya dokument**.</span><span class="sxs-lookup"><span data-stu-id="3dc30-124">Expand hello **Tasks** database, expand hello **Items** collection, click **Documents**, and then click **New Documents**.</span></span> 

   ![Skapa nya dokument i Data Explorer i hello Azure-portalen](./media/create-documentdb-dotnet/azure-cosmosdb-data-explorer-new-document.png)
  
2. <span data-ttu-id="3dc30-126">Lägg till en dokumentsamling toohello med följande struktur hello.</span><span class="sxs-lookup"><span data-stu-id="3dc30-126">Now add a document toohello collection with hello following structure.</span></span>

     ```json
     {
         "id": "1",
         "category": "personal",
         "name": "groceries",
         "description": "Pick up apples and strawberries.",
         "isComplete": false
     }
     ```

3. <span data-ttu-id="3dc30-127">När du har lagt till hello json toohello **dokument** klickar du på **spara**.</span><span class="sxs-lookup"><span data-stu-id="3dc30-127">Once you've added hello json toohello **Documents** tab, click **Save**.</span></span>

    ![Kopiera i json-data och klicka på Spara i Data Explorer i hello Azure-portalen](./media/create-documentdb-dotnet/azure-cosmosdb-data-explorer-save-document.png)

4.  <span data-ttu-id="3dc30-129">Skapa och spara en mer dokument där du infoga ett unikt värde för hello `id` egenskap och hello andra egenskaper som du vill ändra.</span><span class="sxs-lookup"><span data-stu-id="3dc30-129">Create and save one more document where you insert a unique value for hello `id` property, and change hello other properties as you see fit.</span></span> <span data-ttu-id="3dc30-130">Dina nya dokument kan ha vilken struktur du vill eftersom Azure Cosmos DB inte kräver något schema för dina data.</span><span class="sxs-lookup"><span data-stu-id="3dc30-130">Your new documents can have any structure you want as Azure Cosmos DB doesn't impose any schema on your data.</span></span>

     <span data-ttu-id="3dc30-131">Du kan nu använda frågor i Data Explorer tooretrieve dina data genom att klicka på hello **Redigera Filter** och **Använd Filter** knappar.</span><span class="sxs-lookup"><span data-stu-id="3dc30-131">You can now use queries in Data Explorer tooretrieve your data by clicking hello **Edit Filter** and **Apply Filter** buttons.</span></span> <span data-ttu-id="3dc30-132">Som standard använder Data Explorer `SELECT * FROM c` tooretrieve alla dokument i hello samlingen, men du kan ändra den olika tooa [SQL-frågan](documentdb-sql-query.md), som `SELECT * FROM c ORDER BY c._ts DESC`, tooreturn alla hello dokument i fallande ordning utifrån tidsstämpel.</span><span class="sxs-lookup"><span data-stu-id="3dc30-132">By default, Data Explorer uses `SELECT * FROM c` tooretrieve all documents in hello collection, but you can change that tooa different [SQL query](documentdb-sql-query.md), such as `SELECT * FROM c ORDER BY c._ts DESC`, tooreturn all hello documents in descending order based on their timestamp.</span></span> 
 
     <span data-ttu-id="3dc30-133">Du kan också använda Data Explorer toocreate lagrade procedurer, UDF: er och utlösare tooperform serversidan affärslogik samt som skala genomflöde.</span><span class="sxs-lookup"><span data-stu-id="3dc30-133">You can also use Data Explorer toocreate stored procedures, UDFs, and triggers tooperform server-side business logic as well as scale throughput.</span></span> <span data-ttu-id="3dc30-134">Data Explorer visar alla hello inbyggda programmatisk åtkomst till data i hello API: er, men ger enkel åtkomst tooyour data i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="3dc30-134">Data Explorer exposes all of hello built-in programmatic data access available in hello APIs, but provides easy access tooyour data in hello Azure portal.</span></span>

## <a name="clone-hello-sample-application"></a><span data-ttu-id="3dc30-135">Klona hello exempelprogrammet</span><span class="sxs-lookup"><span data-stu-id="3dc30-135">Clone hello sample application</span></span>

<span data-ttu-id="3dc30-136">Nu ska vi växla tooworking med kod.</span><span class="sxs-lookup"><span data-stu-id="3dc30-136">Now let's switch tooworking with code.</span></span> <span data-ttu-id="3dc30-137">Vi ska klona en DocumentDB-API-app från GitHub, ange hello anslutningssträngen och kör den.</span><span class="sxs-lookup"><span data-stu-id="3dc30-137">Let's clone a DocumentDB API app from GitHub, set hello connection string, and run it.</span></span> <span data-ttu-id="3dc30-138">Du ser hur enkelt är det toowork med data programmässigt.</span><span class="sxs-lookup"><span data-stu-id="3dc30-138">You see how easy it is toowork with data programmatically.</span></span> 

1. <span data-ttu-id="3dc30-139">Öppna ett git terminalfönster, till exempel git bash och `CD` tooa arbetskatalogen.</span><span class="sxs-lookup"><span data-stu-id="3dc30-139">Open a git terminal window, such as git bash, and `CD` tooa working directory.</span></span>  

2. <span data-ttu-id="3dc30-140">Hello kör följande kommando tooclone hello exempel lagringsplatsen.</span><span class="sxs-lookup"><span data-stu-id="3dc30-140">Run hello following command tooclone hello sample repository.</span></span> 

    ```bash
    git clone https://github.com/Azure-Samples/azure-cosmos-db-documentdb-java-getting-started.git
    ```

## <a name="review-hello-code"></a><span data-ttu-id="3dc30-141">Granska hello kod</span><span class="sxs-lookup"><span data-stu-id="3dc30-141">Review hello code</span></span>

<span data-ttu-id="3dc30-142">Låt oss göra en snabb genomgång av vad som händer i hello app.</span><span class="sxs-lookup"><span data-stu-id="3dc30-142">Let's make a quick review of what's happening in hello app.</span></span> <span data-ttu-id="3dc30-143">Öppna hello `Program.java` från hello \src\GetStarted mapp och söka efter dessa rader med kod som skapar hello Azure Cosmos DB resurser.</span><span class="sxs-lookup"><span data-stu-id="3dc30-143">Open hello `Program.java` file from hello \src\GetStarted folder, and find these lines of code that create hello Azure Cosmos DB resources.</span></span> 

* <span data-ttu-id="3dc30-144">Hej `DocumentClient` har initierats.</span><span class="sxs-lookup"><span data-stu-id="3dc30-144">hello `DocumentClient` is initialized.</span></span>

    ```java
    this.client = new DocumentClient("https://FILLME.documents.azure.com",
            "FILLME", 
            new ConnectionPolicy(),
            ConsistencyLevel.Session);
    ```

* <span data-ttu-id="3dc30-145">En ny databas skapas.</span><span class="sxs-lookup"><span data-stu-id="3dc30-145">A new database is created.</span></span>

    ```java
    Database database = new Database();
    database.setId(databaseName);
    
    this.client.createDatabase(database, null);
    ```

* <span data-ttu-id="3dc30-146">En ny samling skapas.</span><span class="sxs-lookup"><span data-stu-id="3dc30-146">A new collection is created.</span></span>

    ```java
    DocumentCollection collectionInfo = new DocumentCollection();
    collectionInfo.setId(collectionName);

    ...

    this.client.createCollection(databaseLink, collectionInfo, requestOptions);
    ```

* <span data-ttu-id="3dc30-147">Vissa dokument skapas.</span><span class="sxs-lookup"><span data-stu-id="3dc30-147">Some documents are created.</span></span>

    ```java
    // Any Java object within your code can be serialized into JSON and written tooAzure Cosmos DB
    Family andersenFamily = new Family();
    andersenFamily.setId("Andersen.1");
    andersenFamily.setLastName("Andersen");
    // More properties

    String collectionLink = String.format("/dbs/%s/colls/%s", databaseName, collectionName);
    this.client.createDocument(collectionLink, family, new RequestOptions(), true);
    ```

* <span data-ttu-id="3dc30-148">En SQL-fråga över JSON utförs.</span><span class="sxs-lookup"><span data-stu-id="3dc30-148">A SQL query over JSON is performed.</span></span>

    ```java
    FeedOptions queryOptions = new FeedOptions();
    queryOptions.setPageSize(-1);
    queryOptions.setEnableCrossPartitionQuery(true);

    String collectionLink = String.format("/dbs/%s/colls/%s", databaseName, collectionName);
    FeedResponse<Document> queryResults = this.client.queryDocuments(
        collectionLink,
        "SELECT * FROM Family WHERE Family.lastName = 'Andersen'", queryOptions);

    System.out.println("Running SQL query...");
    for (Document family : queryResults.getQueryIterable()) {
        System.out.println(String.format("\tRead %s", family));
    }
    ```    

## <a name="update-your-connection-string"></a><span data-ttu-id="3dc30-149">Uppdatera din anslutningssträng</span><span class="sxs-lookup"><span data-stu-id="3dc30-149">Update your connection string</span></span>

<span data-ttu-id="3dc30-150">Gå tillbaka toohello Azure portal tooget din Anslutningssträngsinformation nu och kopierar den till hello app.</span><span class="sxs-lookup"><span data-stu-id="3dc30-150">Now go back toohello Azure portal tooget your connection string information and copy it into hello app.</span></span> <span data-ttu-id="3dc30-151">Detta gör att din app toocommunicate med värdbaserade databasen.</span><span class="sxs-lookup"><span data-stu-id="3dc30-151">This will enable your app toocommunicate with your hosted database.</span></span>

1. <span data-ttu-id="3dc30-152">I hello [Azure-portalen](http://portal.azure.com/), i din Azure Cosmos DB konto, i hello vänstra navigeringsrutan klickar du på **nycklar**, och klicka sedan på **skrivskyddad nycklar**.</span><span class="sxs-lookup"><span data-stu-id="3dc30-152">In hello [Azure portal](http://portal.azure.com/), in your Azure Cosmos DB account, in hello left navigation click **Keys**, and then click **Read-write Keys**.</span></span> <span data-ttu-id="3dc30-153">Du måste använda hello kopiera knappar i hello höger sida av hello skärmen toocopy hello URI och PRIMÄRNYCKEL i hello `Program.java` filen i hello nästa steg.</span><span class="sxs-lookup"><span data-stu-id="3dc30-153">You'll use hello copy buttons on hello right side of hello screen toocopy hello URI and PRIMARY KEY into hello `Program.java` file in hello next step.</span></span>

    ![Visa och kopiera en snabbtangent i hello Azure-portalen, bladet nycklar](./media/create-documentdb-dotnet/keys.png)

2. <span data-ttu-id="3dc30-155">I hello öppna `Program.java` filen, kopiera URI-värdet från hello-portal (med hello kopieringsknappen) och gör det hello värdet för hello endpoint toohello DocumentClient konstruktor i `Program.java`.</span><span class="sxs-lookup"><span data-stu-id="3dc30-155">In hello open `Program.java` file, copy your URI value from hello portal (using hello copy button) and make it hello value of hello endpoint toohello DocumentClient constructor in `Program.java`.</span></span> 

    `"https://FILLME.documents.azure.com"`

4. <span data-ttu-id="3dc30-156">Kopiera PRIMARY KEY-värdet från hello-portalen och klistra in den över ”FILLME”, vilket gör det hello andra parameter i hello DocumentClient konstruktor.</span><span class="sxs-lookup"><span data-stu-id="3dc30-156">Then copy your PRIMARY KEY value from hello portal and paste it over “FILLME”, making it hello second parameter in hello DocumentClient constructor.</span></span> <span data-ttu-id="3dc30-157">Du har nu uppdaterat din app med alla hello information som behövs för toocommunicate med Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="3dc30-157">You've now updated your app with all hello info it needs toocommunicate with Azure Cosmos DB.</span></span> 
    
## <a name="run-hello-app"></a><span data-ttu-id="3dc30-158">Kör hello-appen</span><span class="sxs-lookup"><span data-stu-id="3dc30-158">Run hello app</span></span>

1. <span data-ttu-id="3dc30-159">I hello git terminalfönster, `cd` toohello azure-cosmos-db-documentdb-java-getting-started mapp.</span><span class="sxs-lookup"><span data-stu-id="3dc30-159">In hello git terminal window, `cd` toohello azure-cosmos-db-documentdb-java-getting-started folder.</span></span>

2. <span data-ttu-id="3dc30-160">Skriv i hello git terminalfönster, `mvn package` tooinstall hello krävs Java-paket.</span><span class="sxs-lookup"><span data-stu-id="3dc30-160">In hello git terminal window, type `mvn package` tooinstall hello required Java packages.</span></span>

3. <span data-ttu-id="3dc30-161">Kör i hello git terminalfönster, `mvn exec:java -D exec.mainClass=GetStarted.Program` hello terminalfönster toostart Java-programmet.</span><span class="sxs-lookup"><span data-stu-id="3dc30-161">In hello git terminal window, run `mvn exec:java -D exec.mainClass=GetStarted.Program` in hello terminal window toostart your Java application.</span></span>

    <span data-ttu-id="3dc30-162">I hello terminalfönster får du meddelande om att hello FamilyDB databasen har skapats och toopress viktiga toocontinue.</span><span class="sxs-lookup"><span data-stu-id="3dc30-162">In hello terminal window, you'll receive notification that hello FamilyDB database was created, and toopress a key toocontinue.</span></span> <span data-ttu-id="3dc30-163">Tryck på en nyckel toocreate hello-databas, växla toohello Data Explorer och du ser nu innehåller en FamilyDB databas.</span><span class="sxs-lookup"><span data-stu-id="3dc30-163">Press a key toocreate hello database, then switch toohello Data Explorer and you'll see that it now contains a FamilyDB database.</span></span> <span data-ttu-id="3dc30-164">Fortsätt toopress nycklar toocreate hello insamling och hello dokument och sedan köra en fråga.</span><span class="sxs-lookup"><span data-stu-id="3dc30-164">Continue toopress keys toocreate hello collection and hello documents and then perform a query.</span></span> <span data-ttu-id="3dc30-165">När hello projektet är klar tas hello resurser bort från ditt konto.</span><span class="sxs-lookup"><span data-stu-id="3dc30-165">When hello project completes, hello resources are deleted from your account.</span></span> 

    ![Visa och kopiera en snabbtangent i hello Azure-portalen, bladet nycklar](./media/create-documentdb-java/console-output.png)


## <a name="review-slas-in-hello-azure-portal"></a><span data-ttu-id="3dc30-167">Granska SLA: er i hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="3dc30-167">Review SLAs in hello Azure portal</span></span>

[!INCLUDE [cosmosdb-tutorial-review-slas](../../includes/cosmos-db-tutorial-review-slas.md)]

## <a name="clean-up-resources"></a><span data-ttu-id="3dc30-168">Rensa resurser</span><span class="sxs-lookup"><span data-stu-id="3dc30-168">Clean up resources</span></span>

<span data-ttu-id="3dc30-169">Om du inte kommer toocontinue toouse den här appen, tar du bort alla resurser som skapats av denna Snabbstart i hello Azure-portalen med hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="3dc30-169">If you're not going toocontinue toouse this app, delete all resources created by this quickstart in hello Azure portal with hello following steps:</span></span>

1. <span data-ttu-id="3dc30-170">Hello vänstra menyn i hello Azure-portalen klickar du på **resursgrupper** och klicka sedan på hello namnet på hello resurs du skapat.</span><span class="sxs-lookup"><span data-stu-id="3dc30-170">From hello left-hand menu in hello Azure portal, click **Resource groups** and then click hello name of hello resource you created.</span></span> 
2. <span data-ttu-id="3dc30-171">På din resurs gruppen klickar du på **ta bort**typnamn hello för hello resurs toodelete i hello textrutan och klicka sedan på **ta bort**.</span><span class="sxs-lookup"><span data-stu-id="3dc30-171">On your resource group page, click **Delete**, type hello name of hello resource toodelete in hello text box, and then click **Delete**.</span></span>

## <a name="next-steps"></a><span data-ttu-id="3dc30-172">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="3dc30-172">Next steps</span></span>

<span data-ttu-id="3dc30-173">I den här snabbstarten du har lärt dig hur toocreate ett konto i Azure Cosmos DB, dokument-databas och samling med hello Data Explorer och kör en app toodo hello samma sak programmässigt.</span><span class="sxs-lookup"><span data-stu-id="3dc30-173">In this quickstart, you've learned how toocreate an Azure Cosmos DB account, document database, and collection using hello Data Explorer, and run an app toodo hello same thing programmatically.</span></span> <span data-ttu-id="3dc30-174">Nu kan du importera ytterligare data tooyour Cosmos-DB-konto.</span><span class="sxs-lookup"><span data-stu-id="3dc30-174">You can now import additional data tooyour Cosmos DB account.</span></span> 

> [!div class="nextstepaction"]
> [<span data-ttu-id="3dc30-175">Importera data till Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="3dc30-175">Import data into Azure Cosmos DB</span></span>](import-data.md)


