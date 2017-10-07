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
# <a name="azure-cosmos-db-create-a-document-database-using-java-and-hello-azure-portal"></a>Azure DB Cosmos: Skapa en databas för dokument som använder Java och hello Azure-portalen

Azure Cosmos DB är Microsofts globalt distribuerade databastjänst för flera datamodeller. Du kan snabbt skapa och fråga dokument och nyckel/värde-diagrammet databaser, som omfattas av hello global distributionsplatsen och skala horisontellt funktionerna i hello kärnan i Azure Cosmos DB. 

Denna Snabbstart skapar ett dokument hello databas med hjälp av Azure portal-verktyg för Azure Cosmos DB. Den här snabbstarten visar också hur tooquickly skapar en Java-konsolapp med hello [DocumentDB Java API](documentdb-sdk-java.md). hello anvisningarna i den här snabbstarten kan tillämpas på alla operativsystem som kan köra Java. Genom att slutföra den här snabbstarten du är bekant med att skapa och ändra dokumentet databasresurser i hello Användargränssnittet eller genom att programmera, beroende på vilket som är dina inställningar.

## <a name="prerequisites"></a>Krav

* [Java Development Kit (JDK) 1.7+](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)
    * Ubuntu, kör `apt-get install default-jdk` tooinstall hello JDK.
    * Vara säker på att tooset hello JAVA_HOME miljö variabeln toopoint toohello mapp där hello JDK är installerad.
* [Ladda ned](http://maven.apache.org/download.cgi) och [installera](http://maven.apache.org/install.html) ett [Maven](http://maven.apache.org/)-binärarkiv
    * Ubuntu, du kan köra `apt-get install maven` tooinstall Maven.
* [Git](https://www.git-scm.com/)
    * Ubuntu, du kan köra `sudo apt-get install git` tooinstall Git.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-a-database-account"></a>Skapa ett databaskonto

Innan du kan skapa en dokumentdatabasen måste toocreate ett databaskonto i SQL (DocumentDB) med Azure Cosmos DB.

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

## <a name="add-a-collection"></a>Lägga till en samling

[!INCLUDE [cosmos-db-create-collection](../../includes/cosmos-db-create-collection.md)]

<a id="add-sample-data"></a>
## <a name="add-sample-data"></a>Lägg till exempeldata

Du kan nu lägga till tooyour nya insamling av data med hjälp av Data Explorer.

1. I Data Explorer visas hello ny databas i hello samlingar rutan. Expandera hello **uppgifter** databas, expandera hello **objekt** samlingen, klickar du på **dokument**, och klicka sedan på **nya dokument**. 

   ![Skapa nya dokument i Data Explorer i hello Azure-portalen](./media/create-documentdb-dotnet/azure-cosmosdb-data-explorer-new-document.png)
  
2. Lägg till en dokumentsamling toohello med följande struktur hello.

     ```json
     {
         "id": "1",
         "category": "personal",
         "name": "groceries",
         "description": "Pick up apples and strawberries.",
         "isComplete": false
     }
     ```

3. När du har lagt till hello json toohello **dokument** klickar du på **spara**.

    ![Kopiera i json-data och klicka på Spara i Data Explorer i hello Azure-portalen](./media/create-documentdb-dotnet/azure-cosmosdb-data-explorer-save-document.png)

4.  Skapa och spara en mer dokument där du infoga ett unikt värde för hello `id` egenskap och hello andra egenskaper som du vill ändra. Dina nya dokument kan ha vilken struktur du vill eftersom Azure Cosmos DB inte kräver något schema för dina data.

     Du kan nu använda frågor i Data Explorer tooretrieve dina data genom att klicka på hello **Redigera Filter** och **Använd Filter** knappar. Som standard använder Data Explorer `SELECT * FROM c` tooretrieve alla dokument i hello samlingen, men du kan ändra den olika tooa [SQL-frågan](documentdb-sql-query.md), som `SELECT * FROM c ORDER BY c._ts DESC`, tooreturn alla hello dokument i fallande ordning utifrån tidsstämpel. 
 
     Du kan också använda Data Explorer toocreate lagrade procedurer, UDF: er och utlösare tooperform serversidan affärslogik samt som skala genomflöde. Data Explorer visar alla hello inbyggda programmatisk åtkomst till data i hello API: er, men ger enkel åtkomst tooyour data i hello Azure-portalen.

## <a name="clone-hello-sample-application"></a>Klona hello exempelprogrammet

Nu ska vi växla tooworking med kod. Vi ska klona en DocumentDB-API-app från GitHub, ange hello anslutningssträngen och kör den. Du ser hur enkelt är det toowork med data programmässigt. 

1. Öppna ett git terminalfönster, till exempel git bash och `CD` tooa arbetskatalogen.  

2. Hello kör följande kommando tooclone hello exempel lagringsplatsen. 

    ```bash
    git clone https://github.com/Azure-Samples/azure-cosmos-db-documentdb-java-getting-started.git
    ```

## <a name="review-hello-code"></a>Granska hello kod

Låt oss göra en snabb genomgång av vad som händer i hello app. Öppna hello `Program.java` från hello \src\GetStarted mapp och söka efter dessa rader med kod som skapar hello Azure Cosmos DB resurser. 

* Hej `DocumentClient` har initierats.

    ```java
    this.client = new DocumentClient("https://FILLME.documents.azure.com",
            "FILLME", 
            new ConnectionPolicy(),
            ConsistencyLevel.Session);
    ```

* En ny databas skapas.

    ```java
    Database database = new Database();
    database.setId(databaseName);
    
    this.client.createDatabase(database, null);
    ```

* En ny samling skapas.

    ```java
    DocumentCollection collectionInfo = new DocumentCollection();
    collectionInfo.setId(collectionName);

    ...

    this.client.createCollection(databaseLink, collectionInfo, requestOptions);
    ```

* Vissa dokument skapas.

    ```java
    // Any Java object within your code can be serialized into JSON and written tooAzure Cosmos DB
    Family andersenFamily = new Family();
    andersenFamily.setId("Andersen.1");
    andersenFamily.setLastName("Andersen");
    // More properties

    String collectionLink = String.format("/dbs/%s/colls/%s", databaseName, collectionName);
    this.client.createDocument(collectionLink, family, new RequestOptions(), true);
    ```

* En SQL-fråga över JSON utförs.

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

## <a name="update-your-connection-string"></a>Uppdatera din anslutningssträng

Gå tillbaka toohello Azure portal tooget din Anslutningssträngsinformation nu och kopierar den till hello app. Detta gör att din app toocommunicate med värdbaserade databasen.

1. I hello [Azure-portalen](http://portal.azure.com/), i din Azure Cosmos DB konto, i hello vänstra navigeringsrutan klickar du på **nycklar**, och klicka sedan på **skrivskyddad nycklar**. Du måste använda hello kopiera knappar i hello höger sida av hello skärmen toocopy hello URI och PRIMÄRNYCKEL i hello `Program.java` filen i hello nästa steg.

    ![Visa och kopiera en snabbtangent i hello Azure-portalen, bladet nycklar](./media/create-documentdb-dotnet/keys.png)

2. I hello öppna `Program.java` filen, kopiera URI-värdet från hello-portal (med hello kopieringsknappen) och gör det hello värdet för hello endpoint toohello DocumentClient konstruktor i `Program.java`. 

    `"https://FILLME.documents.azure.com"`

4. Kopiera PRIMARY KEY-värdet från hello-portalen och klistra in den över ”FILLME”, vilket gör det hello andra parameter i hello DocumentClient konstruktor. Du har nu uppdaterat din app med alla hello information som behövs för toocommunicate med Azure Cosmos DB. 
    
## <a name="run-hello-app"></a>Kör hello-appen

1. I hello git terminalfönster, `cd` toohello azure-cosmos-db-documentdb-java-getting-started mapp.

2. Skriv i hello git terminalfönster, `mvn package` tooinstall hello krävs Java-paket.

3. Kör i hello git terminalfönster, `mvn exec:java -D exec.mainClass=GetStarted.Program` hello terminalfönster toostart Java-programmet.

    I hello terminalfönster får du meddelande om att hello FamilyDB databasen har skapats och toopress viktiga toocontinue. Tryck på en nyckel toocreate hello-databas, växla toohello Data Explorer och du ser nu innehåller en FamilyDB databas. Fortsätt toopress nycklar toocreate hello insamling och hello dokument och sedan köra en fråga. När hello projektet är klar tas hello resurser bort från ditt konto. 

    ![Visa och kopiera en snabbtangent i hello Azure-portalen, bladet nycklar](./media/create-documentdb-java/console-output.png)


## <a name="review-slas-in-hello-azure-portal"></a>Granska SLA: er i hello Azure-portalen

[!INCLUDE [cosmosdb-tutorial-review-slas](../../includes/cosmos-db-tutorial-review-slas.md)]

## <a name="clean-up-resources"></a>Rensa resurser

Om du inte kommer toocontinue toouse den här appen, tar du bort alla resurser som skapats av denna Snabbstart i hello Azure-portalen med hello följande steg:

1. Hello vänstra menyn i hello Azure-portalen klickar du på **resursgrupper** och klicka sedan på hello namnet på hello resurs du skapat. 
2. På din resurs gruppen klickar du på **ta bort**typnamn hello för hello resurs toodelete i hello textrutan och klicka sedan på **ta bort**.

## <a name="next-steps"></a>Nästa steg

I den här snabbstarten du har lärt dig hur toocreate ett konto i Azure Cosmos DB, dokument-databas och samling med hello Data Explorer och kör en app toodo hello samma sak programmässigt. Nu kan du importera ytterligare data tooyour Cosmos-DB-konto. 

> [!div class="nextstepaction"]
> [Importera data till Azure Cosmos DB](import-data.md)


