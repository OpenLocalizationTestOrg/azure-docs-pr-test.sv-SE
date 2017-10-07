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
# <a name="nosql-tutorial-build-a-documentdb-api-java-console-application"></a>Självstudiekurs om NoSQL: skapa ett DocumentDB Java för API-konsolprogram
> [!div class="op_single_selector"]
> * [.NET](documentdb-get-started.md)
> * [.NET Core](documentdb-dotnetcore-get-started.md)
> * [Node.js för MongoDB](mongodb-samples.md)
> * [Node.js](documentdb-nodejs-get-started.md)
> * [Java](documentdb-java-get-started.md)
> * [C++](documentdb-cpp-get-started.md)
>  
> 

Välkommen till toohello NoSQL-självstudiekursen för hello DocumentDB API: et för Azure Cosmos DB Java SDK! När du har genomfört den här självstudiekursen har du ett konsolprogram som skapar och skickar frågor till Azure Cosmos DB-resurser.

Vi går igenom:

* Skapa och ansluta tooan Azure DB som Cosmos-konto
* Konfigurera en Visual Studio-lösning
* Skapa en onlinedatabas
* Skapa en samling
* Skapa JSON-dokument
* Frågar hello samling
* Skapa JSON-dokument
* Frågar hello samling
* Ersätta ett dokument
* Ta bort ett dokument
* Tar bort hello-databasen

Nu sätter vi igång!

## <a name="prerequisites"></a>Krav
Kontrollera att du har hello följande:

* Ett aktivt Azure-konto. Om du inte har ett kan du registrera dig för ett [kostnadsfritt konto](https://azure.microsoft.com/free/). Du kan också använda hello [Azure Cosmos DB emulatorn](local-emulator.md) för den här självstudiekursen.
* [Git](https://git-scm.com/downloads)
* [Java Development Kit (JDK) 7+](http://www.oracle.com/technetwork/java/javase/downloads/index.html).
* [Maven](http://maven.apache.org/download.cgi).

## <a name="step-1-create-an-azure-cosmos-db-account"></a>Steg 1: Skapa ett Azure Cosmos DB-konto
Nu ska vi skapa ett Azure Cosmos DB-konto. Om du redan har ett konto som du vill toouse kan du gå vidare för[klona hello GitHub projekt](#GitClone). Om du använder hello Azure Cosmos DB-emulatorn gör hello på [Azure Cosmos DB emulatorn](local-emulator.md) tooset upp hello-emulatorn och gå vidare för[klona hello GitHub projekt](#GitClone).

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

## <a id="GitClone"></a>Steg 2: Klona hello GitHub-projekt
Du kan komma igång genom att klona hello GitHub-lagringsplatsen för [Kom igång med Azure Cosmos DB och Java](https://github.com/Azure-Samples/documentdb-java-getting-started). Kör till exempel hello följande tooretrieve hello exempelprojektet lokalt från en lokal katalog.

    git clone git@github.com:Azure-Samples/azure-cosmos-db-documentdb-java-getting-started.git

    cd azure-cosmos-db-documentdb-java-getting-started

hello katalogen innehåller en `pom.xml` för hello projekt och en `src` mapp som innehåller Java källa kod inklusive `Program.java` som visar hur kör enkla åtgärder på Azure Cosmos DB som att skapa dokument och frågor till data i en samling. Hej `pom.xml` innehåller ett beroende på hello [DocumentDB Java SDK på Maven](https://mvnrepository.com/artifact/com.microsoft.azure/azure-documentdb).

    <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>azure-documentdb</artifactId>
        <version>LATEST</version>
    </dependency>

## <a id="Connect"></a>Steg 3: Anslut tooan Azure DB som Cosmos-konto
Därefter head tillbaka toohello [Azure Portal](https://portal.azure.com) tooretrieve din slutpunkt och primära huvudnyckel. hello Azure Cosmos DB slutpunkt och primärnyckel behövs för ditt program toounderstand där tooconnect till med Azure Cosmos DB tootrust appens anslutning.

I hello Azure-portalen, navigera tooyour Azure DB som Cosmos-konto och klicka sedan på **nycklar**. Kopiera hello URI från hello-portalen och klistrar in det i `https://FILLME.documents.azure.com` i hello Program.java-filen. Kopiera hello PRIMÄRNYCKEL från hello-portalen och klistrar in det i `FILLME`.

    this.client = new DocumentClient(
        "https://FILLME.documents.azure.com",
        "FILLME"
        , new ConnectionPolicy(),
        ConsistencyLevel.Session);

![Skärmbild som visar hello Azure-portalen som används av hello NoSQL-självstudiekursen toocreate ett Java-konsolprogram. Visar en Cosmos-databas med Azure-konto med hello hubben aktiv markerad, hello nycklar knappen är markerad i hello Azure DB som Cosmos-kontoblad och värden för hello URI, PRIMÄRNYCKEL och SEKUNDÄRNYCKEL markerade i hello bladet nycklar][keys]

## <a name="step-4-create-a-database"></a>Steg 4: Skapa en databas
Azure Cosmos-DB [databasen](documentdb-resources.md#databases) kan skapas med hjälp av hello [createDatabase](/java/api/com.microsoft.azure.documentdb._document_client.createdatabase) metod för hello **DocumentClient** klass. En databas är hello logisk behållare för JSON dokumentlagring, partitionerad över samlingarna.

    Database database = new Database();
    database.setId("familydb");
    this.client.createDatabase(database, null);

## <a id="CreateColl"></a>Steg 5: Skapa en samling
> [!WARNING]
> **createCollection** skapar en ny samling med reserverat dataflöde, vilket får priskonsekvenser. Mer information finns på vår [prissättningssida](https://azure.microsoft.com/pricing/details/cosmos-db/).
> 
> 

En [samling](documentdb-resources.md#collections) kan skapas med hjälp av hello [createCollection](/java/api/com.microsoft.azure.documentdb._document_client.createcollection) metod för hello **DocumentClient** klass. En samling är en behållare för JSON-dokument och associerad JavaScript-applogik.


    DocumentCollection collectionInfo = new DocumentCollection();
    collectionInfo.setId("familycoll");

    // Azure Cosmos DB collections can be reserved with throughput specified in request units/second. 
    // Here we create a collection with 400 RU/s.
    RequestOptions requestOptions = new RequestOptions();
    requestOptions.setOfferThroughput(400);

    this.client.createCollection("/dbs/familydb", collectionInfo, requestOptions);

## <a id="CreateDoc"></a>Steg 6: Skapa JSON-dokument
En [dokument](documentdb-resources.md#documents) kan skapas med hjälp av hello [createDocument](/java/api/com.microsoft.azure.documentdb._document_client.createdocument) metod för hello **DocumentClient** klass. Dokument är användardefinierat (godtyckligt) JSON-innehåll. Vi kan nu infoga ett eller flera dokument. Om du redan har data som du vill att toostore i databasen kan du använda Azure Cosmos DB [datamigreringsverktyget](import-data.md) tooimport hello data i en databas.

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

## <a id="Query"></a>Steg 7: Skicka frågor mot Azure Cosmos DB-resurser
Azure Cosmos DB stöder [komplexa frågor](documentdb-sql-query.md) mot JSON-dokument som lagras i varje samling.  hello följande exempelkod visar hur tooquery dokument i Azure Cosmos-databasen med hjälp av SQL-syntaxen med hello [Frågedokument](/java/api/com.microsoft.azure.documentdb._document_client.querydocuments) metod.

    FeedResponse<Document> queryResults = this.client.queryDocuments(
        "/dbs/familydb/colls/familycoll",
        "SELECT * FROM Family WHERE Family.lastName = 'Andersen'", 
        null);

    System.out.println("Running SQL query...");
    for (Document family : queryResults.getQueryIterable()) {
        System.out.println(String.format("\tRead %s", family));
    }

## <a id="ReplaceDocument"></a>Steg 8: Ersätta JSON-dokument
Azure Cosmos-DB stöder uppdatering JSON-dokument med hjälp av hello [replaceDocument](/java/api/com.microsoft.azure.documentdb._document_client.replacedocument) metod.

    // Update a property
    andersenFamily.Children[0].Grade = 6;

    this.client.replaceDocument(
        "/dbs/familydb/colls/familycoll/docs/Andersen.1", 
        andersenFamily,
        null);

## <a id="DeleteDocument"></a>Steg 9: Ta bort JSON-dokument
På samma sätt stöder Azure Cosmos DB tar bort JSON-dokument med hjälp av hello [deleteDocument](/java/api/com.microsoft.azure.documentdb._document_client.deletedocument) metod.  

    this.client.delete("/dbs/familydb/colls/familycoll/docs/Andersen.1", null);

## <a id="DeleteDatabase"></a>Steg 10: Ta bort hello-databasen
Ta bort hello skapade databasen tar bort hello databasen och alla underordnade resurser (samlingar, dokument, etc.).

    this.client.deleteDatabase("/dbs/familydb", null);

## <a id="Run"></a>Steg 11: Kör ditt Java-konsolprogram i sin helhet!
toorun hello programmet hello konsolen navigerar toohello projektmappen och kompilera med Maven:
    
    mvn package

Kör `mvn package` ned hello senaste Azure Cosmos DB-bibliotek från Maven och producerar `GetStarted-0.0.1-SNAPSHOT.jar`. Kör sedan hello appen genom att köra:

    mvn exec:java -D exec.mainClass=GetStarted.Program

Grattis! Du har slutfört den här självstudiekursen om NoSQL och har ett fungerande Java-konsolprogram!

## <a name="next-steps"></a>Nästa steg
* Vill du ha en självstudie om Java-webbappar? Mer information finns i [Skapa ett webbprogram i Java med Azure Cosmos DB](documentdb-java-application.md).
* Lär dig hur för[övervaka ett konto i Azure Cosmos DB](monitor-accounts.md).
* Kör frågor mot vår exempeldatauppsättning i hello [Query Playground](https://www.documentdb.com/sql/demo).
* Mer information om hello programmeringsmodell i hello utveckla avsnitt i hello [dokumentationssidan för Azure Cosmos DB](https://azure.microsoft.com/documentation/services/documentdb/).

[keys]: media/documentdb-get-started/nosql-tutorial-keys.png
