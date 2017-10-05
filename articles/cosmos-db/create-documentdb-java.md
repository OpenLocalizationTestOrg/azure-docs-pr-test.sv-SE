---
title: Skapa en Azure Cosmos DB-dokumentdatabas med Java | Microsoft Docs | Microsoft Docs
description: "Presenterar ett Java-kodexempel som du kan använda för att ansluta till och fråga Azure Cosmos DB DocumentDB-API:n"
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
ms.openlocfilehash: df1a25d703a7b8082bdabb4f7d593cb005d416fe
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="azure-cosmos-db-create-a-document-database-using-java-and-the-azure-portal"></a><span data-ttu-id="84b1c-103">Azure Cosmos DB: Skapa en dokumentdatabas med Java och Azure Portal</span><span class="sxs-lookup"><span data-stu-id="84b1c-103">Azure Cosmos DB: Create a document database using Java and the Azure portal</span></span>

<span data-ttu-id="84b1c-104">Azure Cosmos DB är Microsofts globalt distribuerade databastjänst för flera datamodeller.</span><span class="sxs-lookup"><span data-stu-id="84b1c-104">Azure Cosmos DB is Microsoft’s globally distributed multi-model database service.</span></span> <span data-ttu-id="84b1c-105">Du kan snabbt skapa och ställa frågor mot databaser med dokument, nyckel/värde-par och grafer. Du får fördelar av den globala distributionen och den horisontella skalningsförmågan som ligger i grunden hos Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="84b1c-105">You can quickly create and query document, key/value, and graph databases, all of which benefit from the global distribution and horizontal scale capabilities at the core of Azure Cosmos DB.</span></span> 

<span data-ttu-id="84b1c-106">I den här snabbstarten skapar vi en dokumentdatabas med hjälp av Azure Portal-verktyg för Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="84b1c-106">This quickstart creates a document database using the Azure portal tools for Azure Cosmos DB.</span></span> <span data-ttu-id="84b1c-107">Här visas också hur du snabbt skapar en Java-konsolapp med hjälp av [DocumentDB Java API](documentdb-sdk-java.md).</span><span class="sxs-lookup"><span data-stu-id="84b1c-107">This quickstart also shows you how to quickly create a Java console app using the [DocumentDB Java API](documentdb-sdk-java.md).</span></span> <span data-ttu-id="84b1c-108">Anvisningarna i den här snabbstartsguiden gäller alla operativsystem som kan köra Java.</span><span class="sxs-lookup"><span data-stu-id="84b1c-108">The instructions in this quickstart can be followed on any operating system that is capable of running Java.</span></span> <span data-ttu-id="84b1c-109">När du har slutfört den här snabbstarten kommer du kunna skapa och ändra dokumentdatabasresurser i antingen användargränssnittet eller programmässigt, beroende på vad du föredrar.</span><span class="sxs-lookup"><span data-stu-id="84b1c-109">By completing this quickstart you'll be familiar with creating and modifying document database resources in either the UI or programatically, whichever is your preference.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="84b1c-110">Krav</span><span class="sxs-lookup"><span data-stu-id="84b1c-110">Prerequisites</span></span>

* [<span data-ttu-id="84b1c-111">Java Development Kit (JDK) 1.7+</span><span class="sxs-lookup"><span data-stu-id="84b1c-111">Java Development Kit (JDK) 1.7+</span></span>](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)
    * <span data-ttu-id="84b1c-112">I Ubuntu kör du `apt-get install default-jdk` för att installera JDK-paketet.</span><span class="sxs-lookup"><span data-stu-id="84b1c-112">On Ubuntu, run `apt-get install default-jdk` to install the JDK.</span></span>
    * <span data-ttu-id="84b1c-113">Tänk på att ställa in miljövariabeln JAVA_HOME så att den pekar på den mapp där JDK-paketet är installerat.</span><span class="sxs-lookup"><span data-stu-id="84b1c-113">Be sure to set the JAVA_HOME environment variable to point to the folder where the JDK is installed.</span></span>
* <span data-ttu-id="84b1c-114">[Ladda ned](http://maven.apache.org/download.cgi) och [installera](http://maven.apache.org/install.html) ett [Maven](http://maven.apache.org/)-binärarkiv</span><span class="sxs-lookup"><span data-stu-id="84b1c-114">[Download](http://maven.apache.org/download.cgi) and [install](http://maven.apache.org/install.html) a [Maven](http://maven.apache.org/) binary archive</span></span>
    * <span data-ttu-id="84b1c-115">I Ubuntu kan du köra `apt-get install maven` för att installera Maven.</span><span class="sxs-lookup"><span data-stu-id="84b1c-115">On Ubuntu, you can run `apt-get install maven` to install Maven.</span></span>
* [<span data-ttu-id="84b1c-116">Git</span><span class="sxs-lookup"><span data-stu-id="84b1c-116">Git</span></span>](https://www.git-scm.com/)
    * <span data-ttu-id="84b1c-117">I Ubuntu kan du köra `sudo apt-get install git` för att installera Git.</span><span class="sxs-lookup"><span data-stu-id="84b1c-117">On Ubuntu, you can run `sudo apt-get install git` to install Git.</span></span>

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-a-database-account"></a><span data-ttu-id="84b1c-118">Skapa ett databaskonto</span><span class="sxs-lookup"><span data-stu-id="84b1c-118">Create a database account</span></span>

<span data-ttu-id="84b1c-119">Innan du kan börja skapa en dokumentdatabas måste du skapa ett SQL-databaskonto (DocumentDB) med Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="84b1c-119">Before you can create a document database, you need to create a SQL (DocumentDB) database account with Azure Cosmos DB.</span></span>

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

## <a name="add-a-collection"></a><span data-ttu-id="84b1c-120">Lägga till en samling</span><span class="sxs-lookup"><span data-stu-id="84b1c-120">Add a collection</span></span>

[!INCLUDE [cosmos-db-create-collection](../../includes/cosmos-db-create-collection.md)]

<a id="add-sample-data"></a>
## <a name="add-sample-data"></a><span data-ttu-id="84b1c-121">Lägg till exempeldata</span><span class="sxs-lookup"><span data-stu-id="84b1c-121">Add sample data</span></span>

<span data-ttu-id="84b1c-122">Du kan nu lägga till data till din nya samling med datautforskaren.</span><span class="sxs-lookup"><span data-stu-id="84b1c-122">You can now add data to your new collection using Data Explorer.</span></span>

1. <span data-ttu-id="84b1c-123">Den nya databasen visas på panelen Samlingar i datautforskaren.</span><span class="sxs-lookup"><span data-stu-id="84b1c-123">In Data Explorer, the new database appears in the Collections pane.</span></span> <span data-ttu-id="84b1c-124">Expandera databasen **Tasks** (Aktiviteter), expandera samlingen **Items** (Objekt), klicka på **Dokument** och klicka sedan på **Nya dokument**.</span><span class="sxs-lookup"><span data-stu-id="84b1c-124">Expand the **Tasks** database, expand the **Items** collection, click **Documents**, and then click **New Documents**.</span></span> 

   ![Skapa nya dokument i datautforskaren i Azure Portal](./media/create-documentdb-dotnet/azure-cosmosdb-data-explorer-new-document.png)
  
2. <span data-ttu-id="84b1c-126">Lägg nu till ett dokument i samlingen med följande struktur.</span><span class="sxs-lookup"><span data-stu-id="84b1c-126">Now add a document to the collection with the following structure.</span></span>

     ```json
     {
         "id": "1",
         "category": "personal",
         "name": "groceries",
         "description": "Pick up apples and strawberries.",
         "isComplete": false
     }
     ```

3. <span data-ttu-id="84b1c-127">När du har lagt till json på fliken **Dokument** klickar du på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="84b1c-127">Once you've added the json to the **Documents** tab, click **Save**.</span></span>

    ![Kopiera in json-data och klicka på Spara i Datautforskaren i Azure Portal](./media/create-documentdb-dotnet/azure-cosmosdb-data-explorer-save-document.png)

4.  <span data-ttu-id="84b1c-129">Skapa och spara ännu ett dokument där du lägger till ett unikt värde för egenskapen `id` och ändrar de andra egenskaperna som passar.</span><span class="sxs-lookup"><span data-stu-id="84b1c-129">Create and save one more document where you insert a unique value for the `id` property, and change the other properties as you see fit.</span></span> <span data-ttu-id="84b1c-130">Dina nya dokument kan ha vilken struktur du vill eftersom Azure Cosmos DB inte kräver något schema för dina data.</span><span class="sxs-lookup"><span data-stu-id="84b1c-130">Your new documents can have any structure you want as Azure Cosmos DB doesn't impose any schema on your data.</span></span>

     <span data-ttu-id="84b1c-131">Nu kan du använda frågor i datautforskaren för att hämta dina data genom att klicka på knapparna **Redigera filter** och **Använd filter**.</span><span class="sxs-lookup"><span data-stu-id="84b1c-131">You can now use queries in Data Explorer to retrieve your data by clicking the **Edit Filter** and **Apply Filter** buttons.</span></span> <span data-ttu-id="84b1c-132">Som standard används `SELECT * FROM c` i datautforskaren för att hämta alla dokument i samlingen, men du kan ändra det till en annan [SQL-fråga](documentdb-sql-query.md), till exempel `SELECT * FROM c ORDER BY c._ts DESC`, för att returnera alla dokument i fallande tidsstämpelordning.</span><span class="sxs-lookup"><span data-stu-id="84b1c-132">By default, Data Explorer uses `SELECT * FROM c` to retrieve all documents in the collection, but you can change that to a different [SQL query](documentdb-sql-query.md), such as `SELECT * FROM c ORDER BY c._ts DESC`, to return all the documents in descending order based on their timestamp.</span></span> 
 
     <span data-ttu-id="84b1c-133">Du kan även använda datautforskaren för att skapa lagrade procedurer, UDF:er och utlösare för att utföra affärslogik på serversidan såväl som att skala genomflödet.</span><span class="sxs-lookup"><span data-stu-id="84b1c-133">You can also use Data Explorer to create stored procedures, UDFs, and triggers to perform server-side business logic as well as scale throughput.</span></span> <span data-ttu-id="84b1c-134">Datautforskaren visar all den inbyggda programmässiga dataåtkomsten som finns tillgänglig i API:erna, men ger enkel åtkomst till dina data i Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="84b1c-134">Data Explorer exposes all of the built-in programmatic data access available in the APIs, but provides easy access to your data in the Azure portal.</span></span>

## <a name="clone-the-sample-application"></a><span data-ttu-id="84b1c-135">Klona exempelprogrammet</span><span class="sxs-lookup"><span data-stu-id="84b1c-135">Clone the sample application</span></span>

<span data-ttu-id="84b1c-136">Nu ska vi övergå till att arbeta med kod.</span><span class="sxs-lookup"><span data-stu-id="84b1c-136">Now let's switch to working with code.</span></span> <span data-ttu-id="84b1c-137">Nu ska vi klona en DocumentDB-API-app från GitHub, ange anslutningssträngen och köra den.</span><span class="sxs-lookup"><span data-stu-id="84b1c-137">Let's clone a DocumentDB API app from GitHub, set the connection string, and run it.</span></span> <span data-ttu-id="84b1c-138">Du kommer att se hur lätt det är att arbeta med data programmässigt.</span><span class="sxs-lookup"><span data-stu-id="84b1c-138">You see how easy it is to work with data programmatically.</span></span> 

1. <span data-ttu-id="84b1c-139">Öppna ett git-terminalfönster, till exempel git bash, och `CD` till en arbetskatalog.</span><span class="sxs-lookup"><span data-stu-id="84b1c-139">Open a git terminal window, such as git bash, and `CD` to a working directory.</span></span>  

2. <span data-ttu-id="84b1c-140">Klona exempellagringsplatsen med följande kommando.</span><span class="sxs-lookup"><span data-stu-id="84b1c-140">Run the following command to clone the sample repository.</span></span> 

    ```bash
    git clone https://github.com/Azure-Samples/azure-cosmos-db-documentdb-java-getting-started.git
    ```

## <a name="review-the-code"></a><span data-ttu-id="84b1c-141">Granska koden</span><span class="sxs-lookup"><span data-stu-id="84b1c-141">Review the code</span></span>

<span data-ttu-id="84b1c-142">Vi gör en snabb genomgång av vad som händer i appen.</span><span class="sxs-lookup"><span data-stu-id="84b1c-142">Let's make a quick review of what's happening in the app.</span></span> <span data-ttu-id="84b1c-143">Öppna filen `Program.java` från mappen \src\GetStarted och leta upp de här kodraderna som utgör Azure Cosmos DB-resurserna.</span><span class="sxs-lookup"><span data-stu-id="84b1c-143">Open the `Program.java` file from the \src\GetStarted folder, and find these lines of code that create the Azure Cosmos DB resources.</span></span> 

* <span data-ttu-id="84b1c-144">`DocumentClient` har initierats.</span><span class="sxs-lookup"><span data-stu-id="84b1c-144">The `DocumentClient` is initialized.</span></span>

    ```java
    this.client = new DocumentClient("https://FILLME.documents.azure.com",
            "FILLME", 
            new ConnectionPolicy(),
            ConsistencyLevel.Session);
    ```

* <span data-ttu-id="84b1c-145">En ny databas skapas.</span><span class="sxs-lookup"><span data-stu-id="84b1c-145">A new database is created.</span></span>

    ```java
    Database database = new Database();
    database.setId(databaseName);
    
    this.client.createDatabase(database, null);
    ```

* <span data-ttu-id="84b1c-146">En ny samling skapas.</span><span class="sxs-lookup"><span data-stu-id="84b1c-146">A new collection is created.</span></span>

    ```java
    DocumentCollection collectionInfo = new DocumentCollection();
    collectionInfo.setId(collectionName);

    ...

    this.client.createCollection(databaseLink, collectionInfo, requestOptions);
    ```

* <span data-ttu-id="84b1c-147">Vissa dokument skapas.</span><span class="sxs-lookup"><span data-stu-id="84b1c-147">Some documents are created.</span></span>

    ```java
    // Any Java object within your code can be serialized into JSON and written to Azure Cosmos DB
    Family andersenFamily = new Family();
    andersenFamily.setId("Andersen.1");
    andersenFamily.setLastName("Andersen");
    // More properties

    String collectionLink = String.format("/dbs/%s/colls/%s", databaseName, collectionName);
    this.client.createDocument(collectionLink, family, new RequestOptions(), true);
    ```

* <span data-ttu-id="84b1c-148">En SQL-fråga över JSON utförs.</span><span class="sxs-lookup"><span data-stu-id="84b1c-148">A SQL query over JSON is performed.</span></span>

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

## <a name="update-your-connection-string"></a><span data-ttu-id="84b1c-149">Uppdatera din anslutningssträng</span><span class="sxs-lookup"><span data-stu-id="84b1c-149">Update your connection string</span></span>

<span data-ttu-id="84b1c-150">Gå nu tillbaka till Azure Portal för att hämta information om din anslutningssträng och kopiera den till appen.</span><span class="sxs-lookup"><span data-stu-id="84b1c-150">Now go back to the Azure portal to get your connection string information and copy it into the app.</span></span> <span data-ttu-id="84b1c-151">På så vis kan appen kommunicera med den värdbaserade databasen.</span><span class="sxs-lookup"><span data-stu-id="84b1c-151">This will enable your app to communicate with your hosted database.</span></span>

1. <span data-ttu-id="84b1c-152">I [Azure Portal](http://portal.azure.com/) går du till ditt Azure Cosmos DB-konto. Klicka på **Nycklar** i det västra navigeringsfönstret och sedan på **läs- och skrivnycklar**.</span><span class="sxs-lookup"><span data-stu-id="84b1c-152">In the [Azure portal](http://portal.azure.com/), in your Azure Cosmos DB account, in the left navigation click **Keys**, and then click **Read-write Keys**.</span></span> <span data-ttu-id="84b1c-153">Använd kopieringsknapparna till höger för att kopiera URI och PRIMÄRNYCKEL till filen `Program.java` i nästa steg.</span><span class="sxs-lookup"><span data-stu-id="84b1c-153">You'll use the copy buttons on the right side of the screen to copy the URI and PRIMARY KEY into the `Program.java` file in the next step.</span></span>

    ![Visa och kopiera åtkomstnyckeln i Azure Portal, bladet Nycklar](./media/create-documentdb-dotnet/keys.png)

2. <span data-ttu-id="84b1c-155">I den öppna filen `Program.java` kopierar du URI-värdet från portalen (med kopieringsknappen) och gör det till värdet för slutpunkten för DocumentClient-konstruktorn i `Program.java`.</span><span class="sxs-lookup"><span data-stu-id="84b1c-155">In the open `Program.java` file, copy your URI value from the portal (using the copy button) and make it the value of the endpoint to the DocumentClient constructor in `Program.java`.</span></span> 

    `"https://FILLME.documents.azure.com"`

4. <span data-ttu-id="84b1c-156">Kopiera sedan PRIMÄRNYCKEL-värdet från portalen och klistra in det över FILLME så att det utgör den andra parametern i DocumentClient-konstruktorn.</span><span class="sxs-lookup"><span data-stu-id="84b1c-156">Then copy your PRIMARY KEY value from the portal and paste it over “FILLME”, making it the second parameter in the DocumentClient constructor.</span></span> <span data-ttu-id="84b1c-157">Du har nu uppdaterat appen med all information som behövs för kommunikation med Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="84b1c-157">You've now updated your app with all the info it needs to communicate with Azure Cosmos DB.</span></span> 
    
## <a name="run-the-app"></a><span data-ttu-id="84b1c-158">Kör appen</span><span class="sxs-lookup"><span data-stu-id="84b1c-158">Run the app</span></span>

1. <span data-ttu-id="84b1c-159">I git-terminalfönstret `cd` till mappen azure-cosmos-db-documentdb-java-getting-started.</span><span class="sxs-lookup"><span data-stu-id="84b1c-159">In the git terminal window, `cd` to the azure-cosmos-db-documentdb-java-getting-started folder.</span></span>

2. <span data-ttu-id="84b1c-160">I git-terminalfönstret skriver du `mvn package` för att installera de Java-paket som krävs.</span><span class="sxs-lookup"><span data-stu-id="84b1c-160">In the git terminal window, type `mvn package` to install the required Java packages.</span></span>

3. <span data-ttu-id="84b1c-161">I git-terminalfönstret kör du `mvn exec:java -D exec.mainClass=GetStarted.Program` i terminalfönstret för att starta ditt Java-program.</span><span class="sxs-lookup"><span data-stu-id="84b1c-161">In the git terminal window, run `mvn exec:java -D exec.mainClass=GetStarted.Program` in the terminal window to start your Java application.</span></span>

    <span data-ttu-id="84b1c-162">I terminalfönstret får du ett meddelande om att FamilyDB-databasen har skapats och att du ska trycka på en tangent för att fortsätta.</span><span class="sxs-lookup"><span data-stu-id="84b1c-162">In the terminal window, you'll receive notification that the FamilyDB database was created, and to press a key to continue.</span></span> <span data-ttu-id="84b1c-163">Tryck på en tangent för att skapa databasen, och växla sedan till datautforskaren så ser du att den nu innehåller en FamilyDB-databas.</span><span class="sxs-lookup"><span data-stu-id="84b1c-163">Press a key to create the database, then switch to the Data Explorer and you'll see that it now contains a FamilyDB database.</span></span> <span data-ttu-id="84b1c-164">Fortsätt att trycka på tangenter för att skapa samlingen och dokumenten och skicka sedan en fråga.</span><span class="sxs-lookup"><span data-stu-id="84b1c-164">Continue to press keys to create the collection and the documents and then perform a query.</span></span> <span data-ttu-id="84b1c-165">När projektet är klart tas resurserna bort från ditt konto.</span><span class="sxs-lookup"><span data-stu-id="84b1c-165">When the project completes, the resources are deleted from your account.</span></span> 

    ![Visa och kopiera åtkomstnyckeln i Azure Portal, bladet Nycklar](./media/create-documentdb-java/console-output.png)


## <a name="review-slas-in-the-azure-portal"></a><span data-ttu-id="84b1c-167">Granska serviceavtal i Azure Portal</span><span class="sxs-lookup"><span data-stu-id="84b1c-167">Review SLAs in the Azure portal</span></span>

[!INCLUDE [cosmosdb-tutorial-review-slas](../../includes/cosmos-db-tutorial-review-slas.md)]

## <a name="clean-up-resources"></a><span data-ttu-id="84b1c-168">Rensa resurser</span><span class="sxs-lookup"><span data-stu-id="84b1c-168">Clean up resources</span></span>

<span data-ttu-id="84b1c-169">Om du inte planerar att fortsätta använda den här appen tar du bort alla resurser som skapades i snabbstarten i Azure Portal med följande steg:</span><span class="sxs-lookup"><span data-stu-id="84b1c-169">If you're not going to continue to use this app, delete all resources created by this quickstart in the Azure portal with the following steps:</span></span>

1. <span data-ttu-id="84b1c-170">Klicka på **Resursgrupper** på den vänstra menyn i Azure Portal och sedan på namnet på den resurs du skapade.</span><span class="sxs-lookup"><span data-stu-id="84b1c-170">From the left-hand menu in the Azure portal, click **Resource groups** and then click the name of the resource you created.</span></span> 
2. <span data-ttu-id="84b1c-171">På sidan med resursgrupper klickar du på **Ta bort**, skriver in namnet på resursen att ta bort i textrutan och klickar sedan på **Ta bort**.</span><span class="sxs-lookup"><span data-stu-id="84b1c-171">On your resource group page, click **Delete**, type the name of the resource to delete in the text box, and then click **Delete**.</span></span>

## <a name="next-steps"></a><span data-ttu-id="84b1c-172">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="84b1c-172">Next steps</span></span>

<span data-ttu-id="84b1c-173">I den här snabbstarten har du lärt dig att skapa ett Azure Cosmos DB-konto, en dokumentdatabas och en samling med datautforskaren och att köra en app för att göra samma sak programmässigt.</span><span class="sxs-lookup"><span data-stu-id="84b1c-173">In this quickstart, you've learned how to create an Azure Cosmos DB account, document database, and collection using the Data Explorer, and run an app to do the same thing programmatically.</span></span> <span data-ttu-id="84b1c-174">Du kan nu importera ytterligare data till ditt Cosmos DB-konto.</span><span class="sxs-lookup"><span data-stu-id="84b1c-174">You can now import additional data to your Cosmos DB account.</span></span> 

> [!div class="nextstepaction"]
> [<span data-ttu-id="84b1c-175">Importera data till Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="84b1c-175">Import data into Azure Cosmos DB</span></span>](import-data.md)


