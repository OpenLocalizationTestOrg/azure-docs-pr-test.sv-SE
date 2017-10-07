---
title: "Självstudiekurs om NoSQL: DocumentDB API för Azure Cosmos DB Java SDK | Microsoft Docs"
description: "En självstudiekurs om NoSQL som skapar en onlinedatabas och Java-konsolprogram med hello DocumentDB API för Azure Cosmos DB. Azure DocumentDB är en NoSQL-databas för JSON."
keywords: nosql tutorial, online database, java console application
services: cosmos-db
documentationcenter: Java
author: arramac
manager: jhubbard
editor: monicar
ms.assetid: 75a9efa1-7edd-4fed-9882-c0177274cbb2
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: java
ms.topic: article
ms.date: 05/22/2017
ms.author: arramac
ms.openlocfilehash: 1a298a15ab911d140b9df30ad52cfe0fa07c55b8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="nosql-tutorial-build-a-documentdb-api-java-console-application"></a><span data-ttu-id="41639-105">Självstudiekurs om NoSQL: skapa ett DocumentDB Java för API-konsolprogram</span><span class="sxs-lookup"><span data-stu-id="41639-105">NoSQL tutorial: Build a DocumentDB API Java console application</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="41639-106">.NET</span><span class="sxs-lookup"><span data-stu-id="41639-106">.NET</span></span>](documentdb-get-started.md)
> * [<span data-ttu-id="41639-107">.NET Core</span><span class="sxs-lookup"><span data-stu-id="41639-107">.NET Core</span></span>](documentdb-dotnetcore-get-started.md)
> * [<span data-ttu-id="41639-108">Node.js för MongoDB</span><span class="sxs-lookup"><span data-stu-id="41639-108">Node.js for MongoDB</span></span>](mongodb-samples.md)
> * [<span data-ttu-id="41639-109">Node.js</span><span class="sxs-lookup"><span data-stu-id="41639-109">Node.js</span></span>](documentdb-nodejs-get-started.md)
> * [<span data-ttu-id="41639-110">Java</span><span class="sxs-lookup"><span data-stu-id="41639-110">Java</span></span>](documentdb-java-get-started.md)
> * [<span data-ttu-id="41639-111">C++</span><span class="sxs-lookup"><span data-stu-id="41639-111">C++</span></span>](documentdb-cpp-get-started.md)
>  
> 

<span data-ttu-id="41639-112">Välkommen till toohello NoSQL-självstudiekursen för hello DocumentDB API: et för Azure Cosmos DB Java SDK!</span><span class="sxs-lookup"><span data-stu-id="41639-112">Welcome toohello NoSQL tutorial for hello DocumentDB API for Azure Cosmos DB Java SDK!</span></span> <span data-ttu-id="41639-113">När du har genomfört den här självstudiekursen har du ett konsolprogram som skapar och skickar frågor till Azure Cosmos DB-resurser.</span><span class="sxs-lookup"><span data-stu-id="41639-113">After following this tutorial, you'll have a console application that creates and queries Azure Cosmos DB resources.</span></span>

<span data-ttu-id="41639-114">Vi går igenom:</span><span class="sxs-lookup"><span data-stu-id="41639-114">We cover:</span></span>

* <span data-ttu-id="41639-115">Skapa och ansluta tooan Azure DB som Cosmos-konto</span><span class="sxs-lookup"><span data-stu-id="41639-115">Creating and connecting tooan Azure Cosmos DB account</span></span>
* <span data-ttu-id="41639-116">Konfigurera en Visual Studio-lösning</span><span class="sxs-lookup"><span data-stu-id="41639-116">Configuring your Visual Studio Solution</span></span>
* <span data-ttu-id="41639-117">Skapa en onlinedatabas</span><span class="sxs-lookup"><span data-stu-id="41639-117">Creating an online database</span></span>
* <span data-ttu-id="41639-118">Skapa en samling</span><span class="sxs-lookup"><span data-stu-id="41639-118">Creating a collection</span></span>
* <span data-ttu-id="41639-119">Skapa JSON-dokument</span><span class="sxs-lookup"><span data-stu-id="41639-119">Creating JSON documents</span></span>
* <span data-ttu-id="41639-120">Frågar hello samling</span><span class="sxs-lookup"><span data-stu-id="41639-120">Querying hello collection</span></span>
* <span data-ttu-id="41639-121">Skapa JSON-dokument</span><span class="sxs-lookup"><span data-stu-id="41639-121">Creating JSON documents</span></span>
* <span data-ttu-id="41639-122">Frågar hello samling</span><span class="sxs-lookup"><span data-stu-id="41639-122">Querying hello collection</span></span>
* <span data-ttu-id="41639-123">Ersätta ett dokument</span><span class="sxs-lookup"><span data-stu-id="41639-123">Replacing a document</span></span>
* <span data-ttu-id="41639-124">Ta bort ett dokument</span><span class="sxs-lookup"><span data-stu-id="41639-124">Deleting a document</span></span>
* <span data-ttu-id="41639-125">Tar bort hello-databasen</span><span class="sxs-lookup"><span data-stu-id="41639-125">Deleting hello database</span></span>

<span data-ttu-id="41639-126">Nu sätter vi igång!</span><span class="sxs-lookup"><span data-stu-id="41639-126">Now let's get started!</span></span>

## <a name="prerequisites"></a><span data-ttu-id="41639-127">Krav</span><span class="sxs-lookup"><span data-stu-id="41639-127">Prerequisites</span></span>
<span data-ttu-id="41639-128">Kontrollera att du har hello följande:</span><span class="sxs-lookup"><span data-stu-id="41639-128">Make sure you have hello following:</span></span>

* <span data-ttu-id="41639-129">Ett aktivt Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="41639-129">An active Azure account.</span></span> <span data-ttu-id="41639-130">Om du inte har ett kan du registrera dig för ett [kostnadsfritt konto](https://azure.microsoft.com/free/).</span><span class="sxs-lookup"><span data-stu-id="41639-130">If you don't have one, you can sign up for a [free account](https://azure.microsoft.com/free/).</span></span> <span data-ttu-id="41639-131">Du kan också använda hello [Azure Cosmos DB emulatorn](local-emulator.md) för den här självstudiekursen.</span><span class="sxs-lookup"><span data-stu-id="41639-131">Alternatively, you can use hello [Azure Cosmos DB Emulator](local-emulator.md) for this tutorial.</span></span>
* [<span data-ttu-id="41639-132">Git</span><span class="sxs-lookup"><span data-stu-id="41639-132">Git</span></span>](https://git-scm.com/downloads)
* <span data-ttu-id="41639-133">[Java Development Kit (JDK) 7+](http://www.oracle.com/technetwork/java/javase/downloads/index.html).</span><span class="sxs-lookup"><span data-stu-id="41639-133">[Java Development Kit (JDK) 7+](http://www.oracle.com/technetwork/java/javase/downloads/index.html).</span></span>
* <span data-ttu-id="41639-134">[Maven](http://maven.apache.org/download.cgi).</span><span class="sxs-lookup"><span data-stu-id="41639-134">[Maven](http://maven.apache.org/download.cgi).</span></span>

## <a name="step-1-create-an-azure-cosmos-db-account"></a><span data-ttu-id="41639-135">Steg 1: Skapa ett Azure Cosmos DB-konto</span><span class="sxs-lookup"><span data-stu-id="41639-135">Step 1: Create an Azure Cosmos DB account</span></span>
<span data-ttu-id="41639-136">Nu ska vi skapa ett Azure Cosmos DB-konto.</span><span class="sxs-lookup"><span data-stu-id="41639-136">Let's create an Azure Cosmos DB account.</span></span> <span data-ttu-id="41639-137">Om du redan har ett konto som du vill toouse kan du gå vidare för[klona hello GitHub projekt](#GitClone).</span><span class="sxs-lookup"><span data-stu-id="41639-137">If you already have an account you want toouse, you can skip ahead too[Clone hello GitHub project](#GitClone).</span></span> <span data-ttu-id="41639-138">Om du använder hello Azure Cosmos DB-emulatorn gör hello på [Azure Cosmos DB emulatorn](local-emulator.md) tooset upp hello-emulatorn och gå vidare för[klona hello GitHub projekt](#GitClone).</span><span class="sxs-lookup"><span data-stu-id="41639-138">If you are using hello Azure Cosmos DB Emulator, follow hello steps at [Azure Cosmos DB Emulator](local-emulator.md) tooset up hello emulator and skip ahead too[Clone hello GitHub project](#GitClone).</span></span>

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

## <span data-ttu-id="41639-139"><a id="GitClone"></a>Steg 2: Klona hello GitHub-projekt</span><span class="sxs-lookup"><span data-stu-id="41639-139"><a id="GitClone"></a>Step 2: Clone hello GitHub project</span></span>
<span data-ttu-id="41639-140">Du kan komma igång genom att klona hello GitHub-lagringsplatsen för [Kom igång med Azure Cosmos DB och Java](https://github.com/Azure-Samples/documentdb-java-getting-started).</span><span class="sxs-lookup"><span data-stu-id="41639-140">You can get started by cloning hello GitHub repository for [Get Started with Azure Cosmos DB and Java](https://github.com/Azure-Samples/documentdb-java-getting-started).</span></span> <span data-ttu-id="41639-141">Kör till exempel hello följande tooretrieve hello exempelprojektet lokalt från en lokal katalog.</span><span class="sxs-lookup"><span data-stu-id="41639-141">For example, from a local directory run hello following tooretrieve hello sample project locally.</span></span>

    git clone git@github.com:Azure-Samples/azure-cosmos-db-documentdb-java-getting-started.git

    cd azure-cosmos-db-documentdb-java-getting-started

<span data-ttu-id="41639-142">hello katalogen innehåller en `pom.xml` för hello projekt och en `src` mapp som innehåller Java källa kod inklusive `Program.java` som visar hur kör enkla åtgärder på Azure Cosmos DB som att skapa dokument och frågor till data i en samling.</span><span class="sxs-lookup"><span data-stu-id="41639-142">hello directory contains a `pom.xml` for hello project and a `src` folder containing Java source code including `Program.java` which shows how perform simple operations with Azure Cosmos DB like creating documents and querying data within a collection.</span></span> <span data-ttu-id="41639-143">Hej `pom.xml` innehåller ett beroende på hello [DocumentDB Java SDK på Maven](https://mvnrepository.com/artifact/com.microsoft.azure/azure-documentdb).</span><span class="sxs-lookup"><span data-stu-id="41639-143">hello `pom.xml` includes a dependency on hello [DocumentDB Java SDK on Maven](https://mvnrepository.com/artifact/com.microsoft.azure/azure-documentdb).</span></span>

    <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>azure-documentdb</artifactId>
        <version>LATEST</version>
    </dependency>

## <span data-ttu-id="41639-144"><a id="Connect"></a>Steg 3: Anslut tooan Azure DB som Cosmos-konto</span><span class="sxs-lookup"><span data-stu-id="41639-144"><a id="Connect"></a>Step 3: Connect tooan Azure Cosmos DB account</span></span>
<span data-ttu-id="41639-145">Därefter head tillbaka toohello [Azure Portal](https://portal.azure.com) tooretrieve din slutpunkt och primära huvudnyckel.</span><span class="sxs-lookup"><span data-stu-id="41639-145">Next, head back toohello [Azure Portal](https://portal.azure.com) tooretrieve your endpoint and primary master key.</span></span> <span data-ttu-id="41639-146">hello Azure Cosmos DB slutpunkt och primärnyckel behövs för ditt program toounderstand där tooconnect till med Azure Cosmos DB tootrust appens anslutning.</span><span class="sxs-lookup"><span data-stu-id="41639-146">hello Azure Cosmos DB endpoint and primary key are necessary for your application toounderstand where tooconnect to, and for Azure Cosmos DB tootrust your application's connection.</span></span>

<span data-ttu-id="41639-147">I hello Azure-portalen, navigera tooyour Azure DB som Cosmos-konto och klicka sedan på **nycklar**.</span><span class="sxs-lookup"><span data-stu-id="41639-147">In hello Azure Portal, navigate tooyour Azure Cosmos DB account, and then click **Keys**.</span></span> <span data-ttu-id="41639-148">Kopiera hello URI från hello-portalen och klistrar in det i `https://FILLME.documents.azure.com` i hello Program.java-filen.</span><span class="sxs-lookup"><span data-stu-id="41639-148">Copy hello URI from hello portal and paste it into `https://FILLME.documents.azure.com` in hello Program.java file.</span></span> <span data-ttu-id="41639-149">Kopiera hello PRIMÄRNYCKEL från hello-portalen och klistrar in det i `FILLME`.</span><span class="sxs-lookup"><span data-stu-id="41639-149">Then copy hello PRIMARY KEY from hello portal and paste it into `FILLME`.</span></span>

    this.client = new DocumentClient(
        "https://FILLME.documents.azure.com",
        "FILLME"
        , new ConnectionPolicy(),
        ConsistencyLevel.Session);

![Skärmbild som visar hello Azure-portalen som används av hello NoSQL-självstudiekursen toocreate ett Java-konsolprogram.][keys]

## <a name="step-4-create-a-database"></a><span data-ttu-id="41639-152">Steg 4: Skapa en databas</span><span class="sxs-lookup"><span data-stu-id="41639-152">Step 4: Create a database</span></span>
<span data-ttu-id="41639-153">Azure Cosmos-DB [databasen](documentdb-resources.md#databases) kan skapas med hjälp av hello [createDatabase](/java/api/com.microsoft.azure.documentdb._document_client.createdatabase) metod för hello **DocumentClient** klass.</span><span class="sxs-lookup"><span data-stu-id="41639-153">Your Azure Cosmos DB [database](documentdb-resources.md#databases) can be created by using hello [createDatabase](/java/api/com.microsoft.azure.documentdb._document_client.createdatabase) method of hello **DocumentClient** class.</span></span> <span data-ttu-id="41639-154">En databas är hello logisk behållare för JSON dokumentlagring, partitionerad över samlingarna.</span><span class="sxs-lookup"><span data-stu-id="41639-154">A database is hello logical container of JSON document storage partitioned across collections.</span></span>

    Database database = new Database();
    database.setId("familydb");
    this.client.createDatabase(database, null);

## <span data-ttu-id="41639-155"><a id="CreateColl"></a>Steg 5: Skapa en samling</span><span class="sxs-lookup"><span data-stu-id="41639-155"><a id="CreateColl"></a>Step 5: Create a collection</span></span>
> [!WARNING]
> <span data-ttu-id="41639-156">**createCollection** skapar en ny samling med reserverat dataflöde, vilket får priskonsekvenser.</span><span class="sxs-lookup"><span data-stu-id="41639-156">**createCollection** creates a new collection with reserved throughput, which has pricing implications.</span></span> <span data-ttu-id="41639-157">Mer information finns på vår [prissättningssida](https://azure.microsoft.com/pricing/details/cosmos-db/).</span><span class="sxs-lookup"><span data-stu-id="41639-157">For more details, visit our [pricing page](https://azure.microsoft.com/pricing/details/cosmos-db/).</span></span>
> 
> 

<span data-ttu-id="41639-158">En [samling](documentdb-resources.md#collections) kan skapas med hjälp av hello [createCollection](/java/api/com.microsoft.azure.documentdb._document_client.createcollection) metod för hello **DocumentClient** klass.</span><span class="sxs-lookup"><span data-stu-id="41639-158">A [collection](documentdb-resources.md#collections) can be created by using hello [createCollection](/java/api/com.microsoft.azure.documentdb._document_client.createcollection) method of hello **DocumentClient** class.</span></span> <span data-ttu-id="41639-159">En samling är en behållare för JSON-dokument och associerad JavaScript-applogik.</span><span class="sxs-lookup"><span data-stu-id="41639-159">A collection is a container of JSON documents and associated JavaScript application logic.</span></span>


    DocumentCollection collectionInfo = new DocumentCollection();
    collectionInfo.setId("familycoll");

    // Azure Cosmos DB collections can be reserved with throughput specified in request units/second. 
    // Here we create a collection with 400 RU/s.
    RequestOptions requestOptions = new RequestOptions();
    requestOptions.setOfferThroughput(400);

    this.client.createCollection("/dbs/familydb", collectionInfo, requestOptions);

## <span data-ttu-id="41639-160"><a id="CreateDoc"></a>Steg 6: Skapa JSON-dokument</span><span class="sxs-lookup"><span data-stu-id="41639-160"><a id="CreateDoc"></a>Step 6: Create JSON documents</span></span>
<span data-ttu-id="41639-161">En [dokument](documentdb-resources.md#documents) kan skapas med hjälp av hello [createDocument](/java/api/com.microsoft.azure.documentdb._document_client.createdocument) metod för hello **DocumentClient** klass.</span><span class="sxs-lookup"><span data-stu-id="41639-161">A [document](documentdb-resources.md#documents) can be created by using hello [createDocument](/java/api/com.microsoft.azure.documentdb._document_client.createdocument) method of hello **DocumentClient** class.</span></span> <span data-ttu-id="41639-162">Dokument är användardefinierat (godtyckligt) JSON-innehåll.</span><span class="sxs-lookup"><span data-stu-id="41639-162">Documents are user-defined (arbitrary) JSON content.</span></span> <span data-ttu-id="41639-163">Vi kan nu infoga ett eller flera dokument.</span><span class="sxs-lookup"><span data-stu-id="41639-163">We can now insert one or more documents.</span></span> <span data-ttu-id="41639-164">Om du redan har data som du vill att toostore i databasen kan du använda Azure Cosmos DB [datamigreringsverktyget](import-data.md) tooimport hello data i en databas.</span><span class="sxs-lookup"><span data-stu-id="41639-164">If you already have data you'd like toostore in your database, you can use Azure Cosmos DB's [Data Migration tool](import-data.md) tooimport hello data into a database.</span></span>

    // Insert your Java objects as documents 
    Family andersenFamily = new Family();
    andersenFamily.setId("Andersen.1");
    andersenFamily.setLastName("Andersen")

    // More initialization skipped for brevity. You can have nested references
    andersenFamily.setParents(new Parent[] { parent1, parent2 });
    andersenFamily.setDistrict("WA5");
    Address address = new Address();
    address.setCity("Seattle");
    address.setCounty("King");
    address.setState("WA");

    andersenFamily.setAddress(address);
    andersenFamily.setRegistered(true);

    this.client.createDocument("/dbs/familydb/colls/familycoll", family, new RequestOptions(), true);

![Diagram som illustrerar hello hierarkiska relationen mellan hello konto, hello onlinedatabasen, hello samlingen och hello dokument används av hello NoSQL-självstudiekursen toocreate ett Java-konsolprogram](./media/documentdb-get-started/nosql-tutorial-account-database.png)

## <span data-ttu-id="41639-166"><a id="Query"></a>Steg 7: Skicka frågor mot Azure Cosmos DB-resurser</span><span class="sxs-lookup"><span data-stu-id="41639-166"><a id="Query"></a>Step 7: Query Azure Cosmos DB resources</span></span>
<span data-ttu-id="41639-167">Azure Cosmos DB stöder [komplexa frågor](documentdb-sql-query.md) mot JSON-dokument som lagras i varje samling.</span><span class="sxs-lookup"><span data-stu-id="41639-167">Azure Cosmos DB supports rich [queries](documentdb-sql-query.md) against JSON documents stored in each collection.</span></span>  <span data-ttu-id="41639-168">hello följande exempelkod visar hur tooquery dokument i Azure Cosmos-databasen med hjälp av SQL-syntaxen med hello [Frågedokument](/java/api/com.microsoft.azure.documentdb._document_client.querydocuments) metod.</span><span class="sxs-lookup"><span data-stu-id="41639-168">hello following sample code shows how tooquery documents in Azure Cosmos DB using SQL syntax with hello [queryDocuments](/java/api/com.microsoft.azure.documentdb._document_client.querydocuments) method.</span></span>

    FeedResponse<Document> queryResults = this.client.queryDocuments(
        "/dbs/familydb/colls/familycoll",
        "SELECT * FROM Family WHERE Family.lastName = 'Andersen'", 
        null);

    System.out.println("Running SQL query...");
    for (Document family : queryResults.getQueryIterable()) {
        System.out.println(String.format("\tRead %s", family));
    }

## <span data-ttu-id="41639-169"><a id="ReplaceDocument"></a>Steg 8: Ersätta JSON-dokument</span><span class="sxs-lookup"><span data-stu-id="41639-169"><a id="ReplaceDocument"></a>Step 8: Replace JSON document</span></span>
<span data-ttu-id="41639-170">Azure Cosmos-DB stöder uppdatering JSON-dokument med hjälp av hello [replaceDocument](/java/api/com.microsoft.azure.documentdb._document_client.replacedocument) metod.</span><span class="sxs-lookup"><span data-stu-id="41639-170">Azure Cosmos DB supports updating JSON documents using hello [replaceDocument](/java/api/com.microsoft.azure.documentdb._document_client.replacedocument) method.</span></span>

    // Update a property
    andersenFamily.Children[0].Grade = 6;

    this.client.replaceDocument(
        "/dbs/familydb/colls/familycoll/docs/Andersen.1", 
        andersenFamily,
        null);

## <span data-ttu-id="41639-171"><a id="DeleteDocument"></a>Steg 9: Ta bort JSON-dokument</span><span class="sxs-lookup"><span data-stu-id="41639-171"><a id="DeleteDocument"></a>Step 9: Delete JSON document</span></span>
<span data-ttu-id="41639-172">På samma sätt stöder Azure Cosmos DB tar bort JSON-dokument med hjälp av hello [deleteDocument](/java/api/com.microsoft.azure.documentdb._document_client.deletedocument) metod.</span><span class="sxs-lookup"><span data-stu-id="41639-172">Similarly, Azure Cosmos DB supports deleting JSON documents using hello [deleteDocument](/java/api/com.microsoft.azure.documentdb._document_client.deletedocument) method.</span></span>  

    this.client.delete("/dbs/familydb/colls/familycoll/docs/Andersen.1", null);

## <span data-ttu-id="41639-173"><a id="DeleteDatabase"></a>Steg 10: Ta bort hello-databasen</span><span class="sxs-lookup"><span data-stu-id="41639-173"><a id="DeleteDatabase"></a>Step 10: Delete hello database</span></span>
<span data-ttu-id="41639-174">Ta bort hello skapade databasen tar bort hello databasen och alla underordnade resurser (samlingar, dokument, etc.).</span><span class="sxs-lookup"><span data-stu-id="41639-174">Deleting hello created database removes hello database and all children resources (collections, documents, etc.).</span></span>

    this.client.deleteDatabase("/dbs/familydb", null);

## <span data-ttu-id="41639-175"><a id="Run"></a>Steg 11: Kör ditt Java-konsolprogram i sin helhet!</span><span class="sxs-lookup"><span data-stu-id="41639-175"><a id="Run"></a>Step 11: Run your Java console application all together!</span></span>
<span data-ttu-id="41639-176">toorun hello programmet hello konsolen navigerar toohello projektmappen och kompilera med Maven:</span><span class="sxs-lookup"><span data-stu-id="41639-176">toorun hello application from hello console, navigate toohello project folder and compile using Maven:</span></span>
    
    mvn package

<span data-ttu-id="41639-177">Kör `mvn package` ned hello senaste Azure Cosmos DB-bibliotek från Maven och producerar `GetStarted-0.0.1-SNAPSHOT.jar`.</span><span class="sxs-lookup"><span data-stu-id="41639-177">Running `mvn package` downloads hello latest Azure Cosmos DB library from Maven and produces `GetStarted-0.0.1-SNAPSHOT.jar`.</span></span> <span data-ttu-id="41639-178">Kör sedan hello appen genom att köra:</span><span class="sxs-lookup"><span data-stu-id="41639-178">Then run hello app by running:</span></span>

    mvn exec:java -D exec.mainClass=GetStarted.Program

<span data-ttu-id="41639-179">Grattis!</span><span class="sxs-lookup"><span data-stu-id="41639-179">Congratulations!</span></span> <span data-ttu-id="41639-180">Du har slutfört den här självstudiekursen om NoSQL och har ett fungerande Java-konsolprogram!</span><span class="sxs-lookup"><span data-stu-id="41639-180">You've completed this NoSQL tutorial and have a working Java console application!</span></span>

## <a name="next-steps"></a><span data-ttu-id="41639-181">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="41639-181">Next steps</span></span>
* <span data-ttu-id="41639-182">Vill du ha en självstudie om Java-webbappar?</span><span class="sxs-lookup"><span data-stu-id="41639-182">Want a Java web app tutorial?</span></span> <span data-ttu-id="41639-183">Mer information finns i [Skapa ett webbprogram i Java med Azure Cosmos DB](documentdb-java-application.md).</span><span class="sxs-lookup"><span data-stu-id="41639-183">See [Build a web application with Java using Azure Cosmos DB](documentdb-java-application.md).</span></span>
* <span data-ttu-id="41639-184">Lär dig hur för[övervaka ett konto i Azure Cosmos DB](monitor-accounts.md).</span><span class="sxs-lookup"><span data-stu-id="41639-184">Learn how too[monitor an Azure Cosmos DB account](monitor-accounts.md).</span></span>
* <span data-ttu-id="41639-185">Kör frågor mot vår exempeldatauppsättning i hello [Query Playground](https://www.documentdb.com/sql/demo).</span><span class="sxs-lookup"><span data-stu-id="41639-185">Run queries against our sample dataset in hello [Query Playground](https://www.documentdb.com/sql/demo).</span></span>
* <span data-ttu-id="41639-186">Mer information om hello programmeringsmodell i hello utveckla avsnitt i hello [dokumentationssidan för Azure Cosmos DB](https://azure.microsoft.com/documentation/services/documentdb/).</span><span class="sxs-lookup"><span data-stu-id="41639-186">Learn more about hello programming model in hello Develop section of hello [Azure Cosmos DB documentation page](https://azure.microsoft.com/documentation/services/documentdb/).</span></span>

[keys]: media/documentdb-get-started/nosql-tutorial-keys.png
