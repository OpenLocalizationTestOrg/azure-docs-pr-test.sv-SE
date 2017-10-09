---
title: "aaaJava självstudien om apputveckling med hjälp av Azure Cosmos DB | Microsoft Docs"
description: "Den här självstudien Java visar hur toouse hello Azure Cosmos DB och hello DocumentDB API toostore och komma åt data från en Java-program på Azure Websites."
keywords: "Programutveckling, självstudier för databas, java-program, självstudier för java-webbprogram, documentdb, azure, Microsoft azure"
services: cosmos-db
documentationcenter: java
author: dennyglee
manager: jhubbard
editor: mimig
ms.assetid: 0867a4a2-4bf5-4898-a1f4-44e3868f8725
ms.service: cosmos-db
ms.devlang: java
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.date: 08/22/2017
ms.author: denlee
ms.openlocfilehash: e073de23beb0037ee1e37b48a69e8fe7cdc3fc1d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="build-a-java-web-application-using-azure-cosmos-db-and-hello-documentdb-api"></a>Skapa en Java-webbapp med Azure Cosmos DB och hello DocumentDB-API
> [!div class="op_single_selector"]
> * [.NET](documentdb-dotnet-application.md)
> * [Node.js](documentdb-nodejs-application.md)
> * [Java](documentdb-java-application.md)
> * [Python](documentdb-python-application.md)
> 
> 

Den här självstudien Java visar hur toouse hello [Microsoft Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) service toostore och komma åt data från en Java-program på Azure App Service Web Apps. I det här avsnittet får du veta följande:

* Hur toobuild ett grundläggande JavaServer sidor (JSP)-program i Eclipse.
* Hur toowork med hello Azure Cosmos DB tjänst med hjälp av hello [Azure Cosmos DB Java SDK](https://github.com/Azure/azure-documentdb-java).

Den här självstudien om Java visar hur toocreate en webbaserad aktivitetshanteringsapp som gör att du toocreate, hämta och markera aktiviteter som slutförda, enligt följande bild hello. Var och en av hello uppgifter i hello ToDo-listan lagras som JSON-dokument i Azure Cosmos DB.

![Java-app med att göra-lista](./media/documentdb-java-application/image1.png)

> [!TIP]
> Den här självstudien om apputveckling förutsätter att du har tidigare erfarenhet av Java. Om du är ny tooJava eller hello [nödvändiga verktyg](#Prerequisites), vi rekommenderar att du hämtar hello fullständig [todo](https://github.com/Azure-Samples/documentdb-java-todo-app) projektet från GitHub och skapa den med hjälp av [hello instruktioner hello slutet av det här artikel](#GetProject). När du har skapat kan granska du hello artikel toogain inblick i hello koden i hello ramen för hello-projekt.  
> 
> 

## <a id="Prerequisites"></a>Förhandskrav för den här självstudien för Java-webbprogram
Innan du börjar den här självstudien om apputveckling måste du ha hello följande:

* Ett aktivt Azure-konto. Om du inte har något konto kan du skapa ett kostnadsfritt utvärderingskonto på bara några minuter. Mer information om den [kostnadsfria utvärderingsversionen av Azure](https://azure.microsoft.com/pricing/free-trial/)

    ELLER

    En lokal installation av hello [Azure Cosmos DB emulatorn](local-emulator.md).
* [Java Development Kit (JDK) 7+](http://www.oracle.com/technetwork/java/javase/downloads/index.html).
* [Eclipse IDE för Java EE-utvecklare.](http://www.eclipse.org/downloads/packages/eclipse-ide-java-ee-developers/lunasr1)
* [En Azure-webbplats med Java runtime environment (t.ex. Tomcat eller Jetty) aktiverat.](../app-service-web/web-sites-java-get-started.md)

Om du installerar verktygen för hello första gången, coreservlets.com ger en beskrivning av hello installationsprocessen under hello Snabbstart i sina [Självstudier: Installing TomCat7 and med Eclipse](http://www.coreservlets.com/Apache-Tomcat-Tutorial/tomcat-7-with-eclipse.html) artikel.

## <a id="CreateDB"></a>Steg 1: Skapa ett Azure DB som Cosmos-konto
Vi ska börja med att skapa ett Azure Cosmos DB-konto. Om du redan har ett konto eller om du använder hello Azure Cosmos DB-emulatorn för den här kursen kan du hoppa över för[steg 2: skapa hello JSP Java-program](#CreateJSP).

[!INCLUDE [create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

[!INCLUDE [keys](../../includes/cosmos-db-keys.md)]

## <a id="CreateJSP"></a>Steg 2: Skapa hello JSP Java-program
toocreate hello JSP-appen:

1. Vi börjar med att skapa ett Java-projekt. Starta Eclipse och klicka på **Arkiv**, **Nytt** och slutligen **Dynamiskt webbprojekt**. Om du inte ser **dynamiskt webbprojekt** visas som tillgängliga projekt hello följande: Klicka på **filen**, klickar du på **ny**, klickar du på **projekt**..., expandera **Web**, klickar du på **dynamiskt webbprojekt**, och klicka på **nästa**.
   
    ![JSP Java-apputveckling](./media/documentdb-java-application/image10.png)
2. Ange ett projektnamn i hello **projektnamn** rutan och i hello **Körningsmål** nedrullningsbara menyn, Välj eventuellt ett värde (t.ex. Apache Tomcat v7.0) och klicka sedan på **Slutför**. Om du markerar ett mål för körning kan du toorun projektet lokalt genom Eclipse.
3. Expandera projektet i Eclipse i vyn Projektutforskaren hello. Högerklicka på **Webbinnehåll**, klicka på **Ny** och sedan på **JSP-fil**.
4. I hello **ny JSP-fil** dialogrutan, namnet hello fil **index.jsp**. Behåll hello överordnade mappen som **webbinnehåll**som visas i hello följande bild och klicka sedan på **nästa**.
   
    ![Skapa en ny JSP-fil – självstudie om Java-webbapp](./media/documentdb-java-application/image11.png)
5. I hello **Välj JSP-mall** dialogrutan för hello syftet med den här självstudiekursen väljer **ny JSP-fil (html)**, och klicka sedan på **Slutför**.
6. Lägga till text toodisplay när hello index.jsp-filen öppnas i Eclipse **Hello World!** inom hello befintliga <body> element. Uppdaterade <body> innehållet bör se ut som följande kod hello:
   
        <body>
            <% out.println("Hello World!"); %>
        </body>
7. Spara hello index.jsp-filen.
8. Om du anger ett mål för körning i steg 2, kan du klicka på **projekt** och sedan **kör** toorun JSP-appen lokalt:
   
    ![Hello World – självstudie om Java-app](./media/documentdb-java-application/image12.png)

## <a id="InstallSDK"></a>Steg 3: Installera hello DocumentDB Java SDK
hello enklaste sättet toopull i hello DocumentDB Java SDK och dess beroenden är via [Apache Maven](http://maven.apache.org/).

toodo detta, behöver du tooconvert projekt tooa maven-projekt genom att slutföra hello följande steg:

1. Högerklicka på projektet i hello Projektutforskaren, klicka på **konfigurera**, klickar du på **konvertera tooMaven projekt**.
2. I hello **Skapa ny POM** , acceptera hello standardinställningar och på **Slutför**.
3. I **Projektutforskaren**öppnar hello pom.xml fil.
4. På hello **beroenden** på fliken hello **beroenden** rutan klickar du på **Lägg till**.
5. I hello **Välj beroende** fönstret hello följande:
   
   * I hello **grupp-Id** ange com.microsoft.azure.
   * I hello **artefakt-Id** ange azure documentdb.
   * I hello **Version** ange 1.5.1.
     
   ![Installera DocumentDB-SDK för Java-app](./media/documentdb-java-application/image13.png)
     
   * Eller Lägg till hello beroende XML för grupp-Id och artefakt-Id direkt toohello pom.xml via en textredigerare:
     
        <dependency><groupId>com.microsoft.azure</groupId> <artifactId>azure documentdb</artifactId> <version>1.9.1</version></dependency>
6. Klicka på **OK** så installerar Maven hello DocumentDB Java SDK.
7. Spara hello pom.xml.

## <a id="UseService"></a>Steg 4: Använda hello Azure DB som Cosmos-tjänsten i ett Java-program
1. Först ska vi definiera hello TodoItem objekt i TodoItem.java:
   
        @Data
        @Builder
        public class TodoItem {
            private String category;
            private boolean complete;
            private String id;
            private String name;
        }
   
    I det här projektet använder [Project Lombok](http://projectlombok.org/) toogenerate hello konstruktor, GET, set-metoder och ett verktyg. Du kan också skriva koden manuellt eller har hello IDE generera den.
2. tooinvoke hello Azure Cosmos DB tjänsten måste du initiera en ny **DocumentClient**. I allmänhet är det bästa tooreuse hello **DocumentClient** - snarare än att skapa en ny klient för varje efterföljande begäran. Vi kan återanvända hello klienten genom att omsluta hello klienten i en **DocumentClientFactory**. I DocumentClientFactory.java, behöver du toopaste hello URI och PRIMÄRNYCKEL värden som du sparade tooyour Urklipp i [steg 1](#CreateDB). Ersätt [YOUR\_ENDPOINT\_HERE] med din URI och ersätt [YOUR\_KEY\_HERE] med din PRIMÄRNYCKEL.
   
        private static final String HOST = "[YOUR_ENDPOINT_HERE]";
        private static final String MASTER_KEY = "[YOUR_KEY_HERE]";
   
        private static DocumentClient documentClient = new DocumentClient(HOST, MASTER_KEY,
                        ConnectionPolicy.GetDefault(), ConsistencyLevel.Session);
   
        public static DocumentClient getDocumentClient() {
            return documentClient;
        }
3. Nu skapar vi ett objekt DAO (Data Access) tooabstract beständighet Cosmos DB för tooAzure vår ToDo-objekt.
   
    Order toosave ToDo-objekt tooa samling, hello klienten måste tooknow vilken databas och samling toopersist för (refererade med självlänkar). I allmänhet är det bästa toocache hello databasen och samlingen när det är möjligt tooavoid ytterligare rundor toohello databas.
   
    hello följande kod visar hur tooretrieve vår databas och samling, om den finns, eller skapa en ny om det inte finns:
   
        public class DocDbDao implements TodoDao {
            // hello name of our database.
            private static final String DATABASE_ID = "TodoDB";
   
            // hello name of our collection.
            private static final String COLLECTION_ID = "TodoCollection";
   
            // hello Azure Cosmos DB Client
            private static DocumentClient documentClient = DocumentClientFactory
                    .getDocumentClient();
   
            // Cache for hello database object, so we don't have tooquery for it to
            // retrieve self links.
            private static Database databaseCache;
   
            // Cache for hello collection object, so we don't have tooquery for it to
            // retrieve self links.
            private static DocumentCollection collectionCache;
   
            private Database getTodoDatabase() {
                if (databaseCache == null) {
                    // Get hello database if it exists
                    List<Database> databaseList = documentClient
                            .queryDatabases(
                                    "SELECT * FROM root r WHERE r.id='" + DATABASE_ID
                                            + "'", null).getQueryIterable().toList();
   
                    if (databaseList.size() > 0) {
                        // Cache hello database object so we won't have tooquery for it
                        // later tooretrieve hello selfLink.
                        databaseCache = databaseList.get(0);
                    } else {
                        // Create hello database if it doesn't exist.
                        try {
                            Database databaseDefinition = new Database();
                            databaseDefinition.setId(DATABASE_ID);
   
                            databaseCache = documentClient.createDatabase(
                                    databaseDefinition, null).getResource();
                        } catch (DocumentClientException e) {
                            // TODO: Something has gone terribly wrong - hello app wasn't
                            // able tooquery or create hello collection.
                            // Verify your connection, endpoint, and key.
                            e.printStackTrace();
                        }
                    }
                }
   
                return databaseCache;
            }
   
            private DocumentCollection getTodoCollection() {
                if (collectionCache == null) {
                    // Get hello collection if it exists.
                    List<DocumentCollection> collectionList = documentClient
                            .queryCollections(
                                    getTodoDatabase().getSelfLink(),
                                    "SELECT * FROM root r WHERE r.id='" + COLLECTION_ID
                                            + "'", null).getQueryIterable().toList();
   
                    if (collectionList.size() > 0) {
                        // Cache hello collection object so we won't have tooquery for it
                        // later tooretrieve hello selfLink.
                        collectionCache = collectionList.get(0);
                    } else {
                        // Create hello collection if it doesn't exist.
                        try {
                            DocumentCollection collectionDefinition = new DocumentCollection();
                            collectionDefinition.setId(COLLECTION_ID);
   
                            collectionCache = documentClient.createCollection(
                                    getTodoDatabase().getSelfLink(),
                                    collectionDefinition, null).getResource();
                        } catch (DocumentClientException e) {
                            // TODO: Something has gone terribly wrong - hello app wasn't
                            // able tooquery or create hello collection.
                            // Verify your connection, endpoint, and key.
                            e.printStackTrace();
                        }
                    }
                }
   
                return collectionCache;
            }
        }
4. hello nästa steg är toowrite vissa kod toopersist hello TodoItems i samlingen toohello. I det här exemplet ska vi använda [Gson](https://code.google.com/p/google-gson/) tooserialize och deserialisera TodoItem Plain Old Java Objects (Pojo) tooJSON dokument.
   
        // We'll use Gson for POJO <=> JSON serialization for this example.
        private static Gson gson = new Gson();
   
        @Override
        public TodoItem createTodoItem(TodoItem todoItem) {
            // Serialize hello TodoItem as a JSON Document.
            Document todoItemDocument = new Document(gson.toJson(todoItem));
   
            // Annotate hello document as a TodoItem for retrieval (so that we can
            // store multiple entity types in hello collection).
            todoItemDocument.set("entityType", "todoItem");
   
            try {
                // Persist hello document using hello DocumentClient.
                todoItemDocument = documentClient.createDocument(
                        getTodoCollection().getSelfLink(), todoItemDocument, null,
                        false).getResource();
            } catch (DocumentClientException e) {
                e.printStackTrace();
                return null;
            }
   
            return gson.fromJson(todoItemDocument.toString(), TodoItem.class);
        }
5. Precis som med databaser och samlingar i Azure Cosmos DB används självlänkar även för att referera till dokument. Hej följande helper-funktionen kan vi hämta dokument av ett annat attribut (t.ex. ”id”) än självlänkar:
   
        private Document getDocumentById(String id) {
            // Retrieve hello document using hello DocumentClient.
            List<Document> documentList = documentClient
                    .queryDocuments(getTodoCollection().getSelfLink(),
                            "SELECT * FROM root r WHERE r.id='" + id + "'", null)
                    .getQueryIterable().toList();
   
            if (documentList.size() > 0) {
                return documentList.get(0);
            } else {
                return null;
            }
        }
6. Vi kan använda hello hjälpmetoden i steg 5 tooretrieve ett TodoItem JSON-dokument efter id och sedan deserialisera tooa POJO:
   
        @Override
        public TodoItem readTodoItem(String id) {
            // Retrieve hello document by id using our helper method.
            Document todoItemDocument = getDocumentById(id);
   
            if (todoItemDocument != null) {
                // De-serialize hello document in tooa TodoItem.
                return gson.fromJson(todoItemDocument.toString(), TodoItem.class);
            } else {
                return null;
            }
        }
7. Vi kan också använda hello DocumentClient tooget en samling eller en lista över TodoItems med hjälp av DocumentDB SQL:
   
        @Override
        public List<TodoItem> readTodoItems() {
            List<TodoItem> todoItems = new ArrayList<TodoItem>();
   
            // Retrieve hello TodoItem documents
            List<Document> documentList = documentClient
                    .queryDocuments(getTodoCollection().getSelfLink(),
                            "SELECT * FROM root r WHERE r.entityType = 'todoItem'",
                            null).getQueryIterable().toList();
   
            // De-serialize hello documents in tooTodoItems.
            for (Document todoItemDocument : documentList) {
                todoItems.add(gson.fromJson(todoItemDocument.toString(),
                        TodoItem.class));
            }
   
            return todoItems;
        }
8. Det finns många sätt tooupdate ett dokument med hello DocumentClient. I vår Todo-App vill vi toobe kan tootoggle om ett TodoItem har slutförts. Detta kan uppnås genom att uppdatera hello ”slutförd” attribut i hello dokument:
   
        @Override
        public TodoItem updateTodoItem(String id, boolean isComplete) {
            // Retrieve hello document from hello database
            Document todoItemDocument = getDocumentById(id);
   
            // You can update hello document as a JSON document directly.
            // For more complex operations - you could de-serialize hello document in
            // tooa POJO, update hello POJO, and then re-serialize hello POJO back in to
            // a document.
            todoItemDocument.set("complete", isComplete);
   
            try {
                // Persist/replace hello updated document.
                todoItemDocument = documentClient.replaceDocument(todoItemDocument,
                        null).getResource();
            } catch (DocumentClientException e) {
                e.printStackTrace();
                return null;
            }
   
            return gson.fromJson(todoItemDocument.toString(), TodoItem.class);
        }
9. Slutligen vill vi hello möjlighet toodelete ett TodoItem från listan. toodo kan vi kan använda hello hjälpmetoden vi skrev tidigare tooretrieve hello självlänken och sedan instruera hello klienten toodelete den:
   
        @Override
        public boolean deleteTodoItem(String id) {
            // Azure Cosmos DB refers toodocuments by self link rather than id.
   
            // Query for hello document tooretrieve hello self link.
            Document todoItemDocument = getDocumentById(id);
   
            try {
                // Delete hello document by self link.
                documentClient.deleteDocument(todoItemDocument.getSelfLink(), null);
            } catch (DocumentClientException e) {
                e.printStackTrace();
                return false;
            }
   
            return true;
        }

## <a id="Wire"></a>Steg 5: Sammankoppla hello resten av hello av Java programutvecklingsprojekt tillsammans
Nu när vi har testat färdigt hello roliga bits - återstår är toobuild ett snabbt användargränssnitt och ansluta det upp tooour DAO.

1. Först måste börja med att skapa en domänkontrollant toocall vår DAO:
   
        public class TodoItemController {
            public static TodoItemController getInstance() {
                if (todoItemController == null) {
                    todoItemController = new TodoItemController(TodoDaoFactory.getDao());
                }
                return todoItemController;
            }
   
            private static TodoItemController todoItemController;
   
            private final TodoDao todoDao;
   
            TodoItemController(TodoDao todoDao) {
                this.todoDao = todoDao;
            }
   
            public TodoItem createTodoItem(@NonNull String name,
                    @NonNull String category, boolean isComplete) {
                TodoItem todoItem = TodoItem.builder().name(name).category(category)
                        .complete(isComplete).build();
                return todoDao.createTodoItem(todoItem);
            }
   
            public boolean deleteTodoItem(@NonNull String id) {
                return todoDao.deleteTodoItem(id);
            }
   
            public TodoItem getTodoItemById(@NonNull String id) {
                return todoDao.readTodoItem(id);
            }
   
            public List<TodoItem> getTodoItems() {
                return todoDao.readTodoItems();
            }
   
            public TodoItem updateTodoItem(@NonNull String id, boolean isComplete) {
                return todoDao.updateTodoItem(id, isComplete);
            }
        }
   
    I ett mer komplext program kanske hello styrningen innehåller komplicerad affärslogik utöver hello DAO.
2. Därefter skapar vi ett servlet tooroute HTTP-begäranden toohello domänkontrollant:
   
        public class TodoServlet extends HttpServlet {
            // API Keys
            public static final String API_METHOD = "method";
   
            // API Methods
            public static final String CREATE_TODO_ITEM = "createTodoItem";
            public static final String GET_TODO_ITEMS = "getTodoItems";
            public static final String UPDATE_TODO_ITEM = "updateTodoItem";
   
            // API Parameters
            public static final String TODO_ITEM_ID = "todoItemId";
            public static final String TODO_ITEM_NAME = "todoItemName";
            public static final String TODO_ITEM_CATEGORY = "todoItemCategory";
            public static final String TODO_ITEM_COMPLETE = "todoItemComplete";
   
            public static final String MESSAGE_ERROR_INVALID_METHOD = "{'error': 'Invalid method'}";
   
            private static final long serialVersionUID = 1L;
            private static final Gson gson = new Gson();
   
            @Override
            protected void doGet(HttpServletRequest request,
                    HttpServletResponse response) throws ServletException, IOException {
   
                String apiResponse = MESSAGE_ERROR_INVALID_METHOD;
   
                TodoItemController todoItemController = TodoItemController
                        .getInstance();
   
                String id = request.getParameter(TODO_ITEM_ID);
                String name = request.getParameter(TODO_ITEM_NAME);
                String category = request.getParameter(TODO_ITEM_CATEGORY);
                boolean isComplete = StringUtils.equalsIgnoreCase("true",
                        request.getParameter(TODO_ITEM_COMPLETE)) ? true : false;
   
                switch (request.getParameter(API_METHOD)) {
                case CREATE_TODO_ITEM:
                    apiResponse = gson.toJson(todoItemController.createTodoItem(name,
                            category, isComplete));
                    break;
                case GET_TODO_ITEMS:
                    apiResponse = gson.toJson(todoItemController.getTodoItems());
                    break;
                case UPDATE_TODO_ITEM:
                    apiResponse = gson.toJson(todoItemController.updateTodoItem(id,
                            isComplete));
                    break;
                default:
                    break;
                }
   
                response.getWriter().println(apiResponse);
            }
   
            @Override
            protected void doPost(HttpServletRequest request,
                    HttpServletResponse response) throws ServletException, IOException {
                doGet(request, response);
            }
        }
3. Vi behöver en web interface toodisplay toohello användare. Vi skriver hello index.jsp vi skapade tidigare:
    ```html
        <html>
        <head>
          <meta http-equiv="Content-Type" content="text/html; charset=ISO-8859-1">
          <meta http-equiv="X-UA-Compatible" content="IE=edge;" />
          <title>Azure Cosmos DB Java Sample</title>
   
          <!-- Bootstrap -->
          <link href="//ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap.min.css" rel="stylesheet">
   
          <style>
            /* Add padding toobody for fixed nav bar */
            body {
              padding-top: 50px;
            }
          </style>
        </head>
        <body>
          <!-- Nav Bar -->
          <div class="navbar navbar-inverse navbar-fixed-top" role="navigation">
            <div class="container">
              <div class="navbar-header">
                <a class="navbar-brand" href="#">My Tasks</a>
              </div>
            </div>
          </div>
   
          <!-- Body -->
          <div class="container">
            <h1>My ToDo List</h1>
   
            <hr/>
   
            <!-- hello ToDo List -->
            <div class = "todoList">
              <table class="table table-bordered table-striped" id="todoItems">
                <thead>
                  <tr>
                    <th>Name</th>
                    <th>Category</th>
                    <th>Complete</th>
                  </tr>
                </thead>
                <tbody>
                </tbody>
              </table>
   
              <!-- Update Button -->
              <div class="todoUpdatePanel">
                <form class="form-horizontal" role="form">
                  <button type="button" class="btn btn-primary">Update Tasks</button>
                </form>
              </div>
   
            </div>
   
            <hr/>
   
            <!-- Item Input Form -->
            <div class="todoForm">
              <form class="form-horizontal" role="form">
                <div class="form-group">
                  <label for="inputItemName" class="col-sm-2">Task Name</label>
                  <div class="col-sm-10">
                    <input type="text" class="form-control" id="inputItemName" placeholder="Enter name">
                  </div>
                </div>
   
                <div class="form-group">
                  <label for="inputItemCategory" class="col-sm-2">Task Category</label>
                  <div class="col-sm-10">
                    <input type="text" class="form-control" id="inputItemCategory" placeholder="Enter category">
                  </div>
                </div>
   
                <button type="button" class="btn btn-primary">Add Task</button>
              </form>
            </div>
   
          </div>
   
          <!-- Placed at hello end of hello document so hello pages load faster -->
          <script src="//ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.1.min.js"></script>
          <script src="//ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/bootstrap.min.js"></script>
          <script src="assets/todo.js"></script>
        </body>
        </html>
    ```
4. Och slutligen skriva vissa klientens JavaScript tootie hello webbaserat användargränssnitt och hello servlet tillsammans:
   
        var todoApp = {
          /*
           * API methods toocall Java backend.
           */
          apiEndpoint: "api",
   
          createTodoItem: function(name, category, isComplete) {
            $.post(todoApp.apiEndpoint, {
                "method": "createTodoItem",
                "todoItemName": name,
                "todoItemCategory": category,
                "todoItemComplete": isComplete
              },
              function(data) {
                var todoItem = data;
                todoApp.addTodoItemToTable(todoItem.id, todoItem.name, todoItem.category, todoItem.complete);
              },
              "json");
          },
   
          getTodoItems: function() {
            $.post(todoApp.apiEndpoint, {
                "method": "getTodoItems"
              },
              function(data) {
                var todoItemArr = data;
                $.each(todoItemArr, function(index, value) {
                  todoApp.addTodoItemToTable(value.id, value.name, value.category, value.complete);
                });
              },
              "json");
          },
   
          updateTodoItem: function(id, isComplete) {
            $.post(todoApp.apiEndpoint, {
                "method": "updateTodoItem",
                "todoItemId": id,
                "todoItemComplete": isComplete
              },
              function(data) {},
              "json");
          },
   
          /*
           * UI Methods
           */
          addTodoItemToTable: function(id, name, category, isComplete) {
            var rowColor = isComplete ? "active" : "warning";
   
            todoApp.ui_table().append($("<tr>")
              .append($("<td>").text(name))
              .append($("<td>").text(category))
              .append($("<td>")
                .append($("<input>")
                  .attr("type", "checkbox")
                  .attr("id", id)
                  .attr("checked", isComplete)
                  .attr("class", "isComplete")
                ))
              .addClass(rowColor)
            );
          },
   
          /*
           * UI Bindings
           */
          bindCreateButton: function() {
            todoApp.ui_createButton().click(function() {
              todoApp.createTodoItem(todoApp.ui_createNameInput().val(), todoApp.ui_createCategoryInput().val(), false);
              todoApp.ui_createNameInput().val("");
              todoApp.ui_createCategoryInput().val("");
            });
          },
   
          bindUpdateButton: function() {
            todoApp.ui_updateButton().click(function() {
              // Disable button temporarily.
              var myButton = $(this);
              var originalText = myButton.text();
              $(this).text("Updating...");
              $(this).prop("disabled", true);
   
              // Call api tooupdate todo items.
              $.each(todoApp.ui_updateId(), function(index, value) {
                todoApp.updateTodoItem(value.name, value.value);
                $(value).remove();
              });
   
              // Re-enable button.
              setTimeout(function() {
                myButton.prop("disabled", false);
                myButton.text(originalText);
              }, 500);
            });
          },
   
          bindUpdateCheckboxes: function() {
            todoApp.ui_table().on("click", ".isComplete", function(event) {
              var checkboxElement = $(event.currentTarget);
              var rowElement = $(event.currentTarget).parents('tr');
              var id = checkboxElement.attr('id');
              var isComplete = checkboxElement.is(':checked');
   
              // Toggle table row color
              if (isComplete) {
                rowElement.addClass("active");
                rowElement.removeClass("warning");
              } else {
                rowElement.removeClass("active");
                rowElement.addClass("warning");
              }
   
              // Update hidden inputs for update panel.
              todoApp.ui_updateForm().children("input[name='" + id + "']").remove();
   
              todoApp.ui_updateForm().append($("<input>")
                .attr("type", "hidden")
                .attr("class", "updateComplete")
                .attr("name", id)
                .attr("value", isComplete));
   
            });
          },
   
          /*
           * UI Elements
           */
          ui_createNameInput: function() {
            return $(".todoForm #inputItemName");
          },
   
          ui_createCategoryInput: function() {
            return $(".todoForm #inputItemCategory");
          },
   
          ui_createButton: function() {
            return $(".todoForm button");
          },
   
          ui_table: function() {
            return $(".todoList table tbody");
          },
   
          ui_updateButton: function() {
            return $(".todoUpdatePanel button");
          },
   
          ui_updateForm: function() {
            return $(".todoUpdatePanel form");
          },
   
          ui_updateId: function() {
            return $(".todoUpdatePanel .updateComplete");
          },
   
          /*
           * Install hello TodoApp
           */
          install: function() {
            todoApp.bindCreateButton();
            todoApp.bindUpdateButton();
            todoApp.bindUpdateCheckboxes();
   
            todoApp.getTodoItems();
          }
        };
   
        $(document).ready(function() {
          todoApp.install();
        });
5. Fantastiskt! Nu är återstår tootest hello program. Kör hello programmet lokalt och lägga till några Todo-objekt genom att fylla i hello objektnamn och kategori och klicka på **Lägg till aktivitet**.
6. När hello objektet visas kan du uppdatera om aktiviteten är slutförd genom växla hello kryssrutan och klicka på **uppdatera aktiviteter**.

## <a id="Deploy"></a>Steg 6: Distribuera webbplatser för tooAzure din Java-program
Azure webbplatser kan du distribuera Java-program bara exportera appen som en WAR-fil och antingen ladda upp den via källkontrollen (t.ex. Git) eller FTP.

1. tooexport ditt program som en WAR-fil, högerklicka på projektet i **Projektutforskaren**, klickar du på **exportera**, och klicka sedan på **WAR-filen**.
2. I hello **WAR-Export** fönstret hello följande:
   
   * Ange azure-documentdb-java-sample i hello Web project.
   * Välj ett mål toosave hello WAR-filen hello mål i rutan.
   * Klicka på **Slutför**.
3. Nu när du har en WAR-fil i hand, du kan bara ladda upp den tooyour webbplatsen Azure **webbappar** directory. Anvisningar för att ladda upp hello-fil finns [lägga till en Java-program tooAzure App Service Web Apps](../app-service-web/web-sites-java-add-app.md).
   
    När hello WAR-filen är överförda toohello webbappar katalog, identifierar hello körningsmiljön att du har lagt till den och kommer automatiskt att läsa in den.
4. tooview din färdiga produkt navigera toohttp://YOUR\_plats\_NAME.azurewebsites.net/azure-java-sample/ och börja lägga till aktiviteter!

## <a id="GetProject"></a>Hämta hello projektet från GitHub
Alla hello prover i den här kursen ingår i hello [todo](https://github.com/Azure-Samples/documentdb-java-todo-app) projektet på GitHub. tooimport hello todo-projektet till Eclipse, se till att du har hello programvara och resurser som anges i hello [krav](#Prerequisites) och sedan hello följande:

1. Installera [Project Lombok](http://projectlombok.org/). Lombok är används toogenerate konstruktorer, get och set-metoder i hello-projekt. När du har hämtat hello lombok.jar filen, dubbelklicka på den tooinstall den eller installera det från hello-kommandoraden.
2. Om Eclipse är öppen, Stäng och starta om den tooload Lombok.
3. I Eclipse på hello **filen** -menyn klickar du på **importera**.
4. I hello **importera** -fönstret klickar du på **Git**, klickar du på **projekt från Git**, och klicka sedan på **nästa**.
5. På hello **Välj Lagerkälla** klickar du på **klona URI**.
6. På hello **Git-Lagerkälla** i hello skärmen **URI** rutan Ange https://github.com/Azure-Samples/java-todo-app.git och klicka sedan på **nästa**.
7. På hello **val av gren** kontrollerar du att **master** är markerad och klicka sedan på **nästa**.
8. På hello **lokal Destination** klickar du på **Bläddra** tooselect en mapp där hello databasen kan kopieras och klickar sedan på **nästa**.
9. På hello **väljer en toouse guide för importprojekt** kontrollerar du att **importera befintliga projekt** är markerad och klicka sedan på **nästa**.
10. På hello **Importera projekt** skärmen, avmarkera hello **DocumentDB** projektet och klicka sedan på **Slutför**. Hej DocumentDB-projektet innehåller hello Azure Cosmos DB Java SDK, som vi ska lägga till som beroende i stället.
11. I **Projektutforskaren**, navigera tooazure-documentdb-java-sample\src\com.microsoft.azure.documentdb.sample.dao\DocumentClientFactory.java och Ersätt hello värd och huvudnyckel värden med hello URI och PRIMÄRNYCKEL för din Azure DB som Cosmos-kontot och sedan spara hello-fil. Mer information finns [steg 1. Skapa ett databaskonto Azure Cosmos DB](#CreateDB).
12. I **Projektutforskaren**, högerklicka på hello **azure-documentdb-java-sample**, klickar du på **byggsökväg**, och klicka sedan på **konfigurera byggsökväg**.
13. På hello **Java byggsökväg** skärmen i hello högra rutan, Välj hello **bibliotek** fliken och klicka sedan på **lägga till externa JAR: er**. Navigera toohello platsen för hello lombok.jar-filen och klicka på **öppna**, och klicka sedan på **OK**.
14. Använd steg 12 tooopen hello **egenskaper** fönstret igen, och klicka sedan på hello vänster **Körningsmål**.
15. På hello **Körningsmål** klickar du på **ny**väljer **Apache Tomcat v7.0**, och klicka sedan på **OK**.
16. Använd steg 12 tooopen hello **egenskaper** fönstret igen, och klicka sedan på hello vänster **Projektfasetter**.
17. På hello **Projektfasetter** väljer **dynamisk webbmodul** och **Java**, och klicka sedan på **OK**.
18. På hello **servrar** längst hello hello-skärmen, högerklickar på **Tomcat v7.0-Server på localhost** och klicka sedan på **lägga till och ta bort**.
19. På hello **lägga till och ta bort** fönstret flytta **azure-documentdb-java-sample** toohello **konfigurerad** rutan och klicka på **Slutför**.
20. I hello **servrar** genom att högerklicka på **Tomcat v7.0-Server på localhost**, och klicka sedan på **starta om**.
21. I en webbläsare, navigera toohttp://localhost:8080 / azure-documentdb-java-sample / och börja lägga till tooyour uppgiftslista. Observera att om du har ändrat portarnas standardvärden, ändrar du 8080 toohello-värdet som du har valt.
22. toodeploy ditt projekt tooan Azure-webbplats, se [steg 6. Distribuera ditt program tooAzure webbplatser](#Deploy).

[1]: media/documentdb-java-application/keys.png
