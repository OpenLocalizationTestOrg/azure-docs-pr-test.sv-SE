---
title: "aaaBuild en Node.js-webbapp för Azure Cosmos DB | Microsoft Docs"
description: "Den här självstudien om Node.js utforskar hur toouse Microsoft Azure Cosmos DB toostore och komma åt data från Node.js Express webbprogram finns på Azure Websites."
keywords: "Programutveckling, database-Självstudier lär dig använda node.js, självstudie om node.js"
services: cosmos-db
documentationcenter: nodejs
author: mimig1
manager: jhubbard
editor: cgronlun
ms.assetid: 9da9e63b-e76a-434e-96dd-195ce2699ef3
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: article
ms.date: 08/14/2017
ms.author: mimig
ms.openlocfilehash: 31194dccf37eef69d2219b0d8328a88d434f79b9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="_Toc395783175"></a>Skapa ett Node.js-webbprogram med Azure Cosmos DB
> [!div class="op_single_selector"]
> * [.NET](documentdb-dotnet-application.md)
> * [Node.js](documentdb-nodejs-application.md)
> * [Java](documentdb-java-application.md)
> * [Python](documentdb-python-application.md)
> 
> 

Den här självstudien om Node.js beskrivs hur toouse Azure Cosmos DB och hello DocumentDB API toostore och komma åt data från ett node.Ja Express-program finns på Azure Websites. Du bygger ett enkelt webbaserat aktivitetshanteringsprogram, en ToDo-app, där du kan skapa, hämta och slutföra aktiviteter. hello uppgifter lagras som JSON-dokument i Azure Cosmos DB. Den här självstudiekursen vägleder dig genom hello skapande och distribution av hello appen och förklarar vad som händer i varje fragment.

![Skärmbild som visar hello My Todo List-program som skapats i den här självstudien om Node.js](./media/documentdb-nodejs-application/cosmos-db-node-js-mytodo.png)

Inte har tid toocomplete hello självstudier och bara vill tooget hello komplett lösning? Inga problem, du kan hämta hello fullständiga exempellösningen från [GitHub][GitHub]. Bara läsa hello [viktigt](https://github.com/Azure-Samples/documentdb-node-todo-app/blob/master/README.md) filen anvisningar för hur toorun hello app.

## <a name="_Toc395783176"></a>Förhandskrav
> [!TIP]
> Den här Node.js-självstudien förutsätter att du har tidigare erfarenhet av Node.js och Azure Websites.
> 
> 

Innan du följer hello anvisningarna i den här artikeln bör du kontrollera att du har hello följande:

* Ett aktivt Azure-konto. Om du inte har något konto kan du skapa ett kostnadsfritt utvärderingskonto på bara några minuter. Mer information finns i [kostnadsfri utvärderingsversion av Azure](https://azure.microsoft.com/pricing/free-trial/).

   ELLER

   En lokal installation av hello [Azure Cosmos DB emulatorn](local-emulator.md) (endast Windows).
* [Node.js][Node.js] version v0.10.29 eller högre.
* [Express generator](http://www.expressjs.com/starter/generator.html) (kan installeras via `npm install express-generator -g`)
* [Git][Git].

## <a name="_Toc395637761"></a>Steg 1: Skapa ett Azure Cosmos DB-databaskonto
Vi ska börja med att skapa ett Azure Cosmos DB-konto. Om du redan har ett konto eller om du använder hello Azure Cosmos DB-emulatorn för den här kursen kan du hoppa över för[steg 2: skapa ett nytt Node.js-program](#_Toc395783178).

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

[!INCLUDE [cosmos-db-keys](../../includes/cosmos-db-keys.md)]

## <a name="_Toc395783178"></a>Steg 2: Skapa ett nytt Node.js-program
Nu ska vi ett grundläggande Hello World Node.js-projekt med hello Läs toocreate [Express](http://expressjs.com/) framework.

1. Öppna valfri terminal, till exempel hello Node.js-kommandotolk.
2. Navigera toohello katalog där du vill att toostore hello nytt program.
3. Använd hello express generator toogenerate ett nytt program kallas **todo**.
   
        express todo
4. Öppna din nya **todo**-katalog och installera beroenden.
   
        cd todo
        npm install
5. Kör det nya programmet.
   
        npm start
6. Du kan visa det nya programmet genom att navigera din webbläsare för[http://localhost: 3000](http://localhost:3000).
   
    ![Lär dig använda Node.js – skärmdump av hello programmet Hello World i ett webbläsarfönster](./media/documentdb-nodejs-application/cosmos-db-node-js-express.png)

    Sedan toostop hello program, tryck på CTRL + C i hello terminalfönster och klicka sedan på **y** tooterminate hello batch-jobbet.

## <a name="_Toc395783179"></a>Steg 3: Installera ytterligare moduler
Hej **package.json** är en av hello-filer som skapas i hello rot hello projektet. Den här filen innehåller en lista över ytterligare moduler som krävs för Node.js-programmet. Senare, när du distribuerar det här programmet tooAzure webbplatser, är filen används toodetermine vilka moduler måste toobe installerad på Azure toosupport ditt program. Vi behöver du fortfarande tooinstall två paket för den här självstudiekursen.

1. Tillbaka i hello terminal, installera hello **asynkrona** modul via npm.
   
        npm install async --save
2. Installera hello **documentdb** modul via npm. Detta är hello-modulen där alla hello Azure Cosmos DB magin händer.
   
        npm install documentdb --save
3. En snabb kontroll av hello **package.json** -filen för programmet hello ska visa hello ytterligare moduler. Den här filen ska berätta för Azure vilka paket toodownload och installera när du kör programmet. Det bör likna hello exemplet nedan.
   
        {
          "name": "todo",
          "version": "0.0.0",
          "private": true,
          "scripts": {
            "start": "node ./bin/www"
          },
          "dependencies": {
            "async": "^2.1.4",
            "body-parser": "~1.15.2",
            "cookie-parser": "~1.4.3",
            "debug": "~2.2.0",
            "documentdb": "^1.10.0",
            "express": "~4.14.0",
            "jade": "~1.11.0",
            "morgan": "~1.7.0",
            "serve-favicon": "~2.3.0"
          }
        }
   
    Det här visar för Node (och senare Azure) att ditt program är beroende av de här ytterligare modulerna.

## <a name="_Toc395783180"></a>Steg 4: Använda hello Azure DB som Cosmos-tjänsten i en node-App
Som tar hand om alla hello installationen och konfigurationen är vi get ned toowhy vi här och som är toowrite vissa kod som använder Azure Cosmos DB.

### <a name="create-hello-model"></a>Skapa hello modell
1. Skapa en ny katalog med namnet i hello projektkatalogen **modeller** i hello samma katalog som hello package.json filen.
2. I hello **modeller** directory, skapa en ny fil med namnet **taskDao.js**. Den här filen innehåller hello modellen för hello aktiviteter som skapats av vårt program.
3. Hej i samma **modeller** directory, skapa ytterligare en ny fil med namnet **docdbUtils.js**. Den här filen innehåller användbar och återanvändbar kod som vi ska använda i programmet. 
4. Kopiera hello med följande kod för**docdbUtils.js**
   
        var DocumentDBClient = require('documentdb').DocumentClient;
   
        var DocDBUtils = {
            getOrCreateDatabase: function (client, databaseId, callback) {
                var querySpec = {
                    query: 'SELECT * FROM root r WHERE r.id= @id',
                    parameters: [{
                        name: '@id',
                        value: databaseId
                    }]
                };
   
                client.queryDatabases(querySpec).toArray(function (err, results) {
                    if (err) {
                        callback(err);
   
                    } else {
                        if (results.length === 0) {
                            var databaseSpec = {
                                id: databaseId
                            };
   
                            client.createDatabase(databaseSpec, function (err, created) {
                                callback(null, created);
                            });
   
                        } else {
                            callback(null, results[0]);
                        }
                    }
                });
            },
   
            getOrCreateCollection: function (client, databaseLink, collectionId, callback) {
                var querySpec = {
                    query: 'SELECT * FROM root r WHERE r.id=@id',
                    parameters: [{
                        name: '@id',
                        value: collectionId
                    }]
                };               
   
                client.queryCollections(databaseLink, querySpec).toArray(function (err, results) {
                    if (err) {
                        callback(err);
   
                    } else {        
                        if (results.length === 0) {
                            var collectionSpec = {
                                id: collectionId
                            };
   
                            client.createCollection(databaseLink, collectionSpec, function (err, created) {
                                callback(null, created);
                            });
   
                        } else {
                            callback(null, results[0]);
                        }
                    }
                });
            }
        };
   
        module.exports = DocDBUtils;
   
5. Spara och Stäng hello **docdbUtils.js** fil.
6. Hello början av hello **taskDao.js** lägger du till följande kod tooreference hello hello **DocumentDBClient** och hello **docdbUtils.js** vi skapade ovan:
   
        var DocumentDBClient = require('documentdb').DocumentClient;
        var docdbUtils = require('./docdbUtils');
7. Därefter måste du lägga till kod toodefine och exportera aktivitetsobjektet hello. Detta är ansvarig för att initiera vårt aktivitetsobjekt och konfigurera hello databasen och dokumentsamlingen som vi ska använda.
   
        function TaskDao(documentDBClient, databaseId, collectionId) {
          this.client = documentDBClient;
          this.databaseId = databaseId;
          this.collectionId = collectionId;
   
          this.database = null;
          this.collection = null;
        }
   
        module.exports = TaskDao;
8. Lägg till hello följande kod toodefine fler metoder på hello aktivitetsobjektet som tillåter samverkan med data som lagras i Azure Cosmos DB.
   
        TaskDao.prototype = {
            init: function (callback) {
                var self = this;
   
                docdbUtils.getOrCreateDatabase(self.client, self.databaseId, function (err, db) {
                    if (err) {
                        callback(err);
                    } else {
                        self.database = db;
                        docdbUtils.getOrCreateCollection(self.client, self.database._self, self.collectionId, function (err, coll) {
                            if (err) {
                                callback(err);
   
                            } else {
                                self.collection = coll;
                            }
                        });
                    }
                });
            },
   
            find: function (querySpec, callback) {
                var self = this;
   
                self.client.queryDocuments(self.collection._self, querySpec).toArray(function (err, results) {
                    if (err) {
                        callback(err);
   
                    } else {
                        callback(null, results);
                    }
                });
            },
   
            addItem: function (item, callback) {
                var self = this;
   
                item.date = Date.now();
                item.completed = false;
   
                self.client.createDocument(self.collection._self, item, function (err, doc) {
                    if (err) {
                        callback(err);
   
                    } else {
                        callback(null, doc);
                    }
                });
            },
   
            updateItem: function (itemId, callback) {
                var self = this;
   
                self.getItem(itemId, function (err, doc) {
                    if (err) {
                        callback(err);
   
                    } else {
                        doc.completed = true;
   
                        self.client.replaceDocument(doc._self, doc, function (err, replaced) {
                            if (err) {
                                callback(err);
   
                            } else {
                                callback(null, replaced);
                            }
                        });
                    }
                });
            },
   
            getItem: function (itemId, callback) {
                var self = this;
   
                var querySpec = {
                    query: 'SELECT * FROM root r WHERE r.id = @id',
                    parameters: [{
                        name: '@id',
                        value: itemId
                    }]
                };
   
                self.client.queryDocuments(self.collection._self, querySpec).toArray(function (err, results) {
                    if (err) {
                        callback(err);
   
                    } else {
                        callback(null, results[0]);
                    }
                });
            }
        };
9. Spara och Stäng hello **taskDao.js** fil. 

### <a name="create-hello-controller"></a>Skapa hello-styrenhet
1. I hello **vägar** katalogen för ditt projekt, skapa en ny fil med namnet **tasklist.js**. 
2. Lägg till följande kod för hello**tasklist.js**. Den läser in hello DocumentDBClient och async-moduler som används av **tasklist.js**. Detta definieras även hello **TaskList** funktion, som mottar en instans av hello **aktivitet** objektet som vi definierade tidigare:
   
        var DocumentDBClient = require('documentdb').DocumentClient;
        var async = require('async');
   
        function TaskList(taskDao) {
          this.taskDao = taskDao;
        }
   
        module.exports = TaskList;
3. Fortsätt att lägga till toohello **tasklist.js** filen genom att lägga till hello-metoder som används för**showTasks, addTask**, och **completeTasks**:
   
        TaskList.prototype = {
            showTasks: function (req, res) {
                var self = this;
   
                var querySpec = {
                    query: 'SELECT * FROM root r WHERE r.completed=@completed',
                    parameters: [{
                        name: '@completed',
                        value: false
                    }]
                };
   
                self.taskDao.find(querySpec, function (err, items) {
                    if (err) {
                        throw (err);
                    }
   
                    res.render('index', {
                        title: 'My ToDo List ',
                        tasks: items
                    });
                });
            },
   
            addTask: function (req, res) {
                var self = this;
                var item = req.body;
   
                self.taskDao.addItem(item, function (err) {
                    if (err) {
                        throw (err);
                    }
   
                    res.redirect('/');
                });
            },
   
            completeTask: function (req, res) {
                var self = this;
                var completedTasks = Object.keys(req.body);
   
                async.forEach(completedTasks, function taskIterator(completedTask, callback) {
                    self.taskDao.updateItem(completedTask, function (err) {
                        if (err) {
                            callback(err);
                        } else {
                            callback(null);
                        }
                    });
                }, function goHome(err) {
                    if (err) {
                        throw err;
                    } else {
                        res.redirect('/');
                    }
                });
            }
        };
4. Spara och Stäng hello **tasklist.js** fil.

### <a name="add-configjs"></a>Lägg till config.js
1. Skapa en ny fil med namnet **config.js** i projektkatalogen.
2. Lägg till följande hello för**config.js**. Det definierar konfigurationsinställningar och värden som behövs i appen.
   
        var config = {}
   
        config.host = process.env.HOST || "[hello URI value from hello Azure Cosmos DB Keys blade on http://portal.azure.com]";
        config.authKey = process.env.AUTH_KEY || "[hello PRIMARY KEY value from hello Azure Cosmos DB Keys blade on http://portal.azure.com]";
        config.databaseId = "ToDoList";
        config.collectionId = "Items";
   
        module.exports = config;
3. I hello **config.js** -fil, uppdatera hello värden för HOST och AUTH_KEY med hello värdena i hello nycklar bladet för din Azure DB som Cosmos-konto i hello [Microsoft Azure-portalen](https://portal.azure.com).
4. Spara och Stäng hello **config.js** fil.

### <a name="modify-appjs"></a>Ändra app.js
1. Öppna hello i hello projektkatalogen **app.js** fil. Den här filen skapades tidigare när hello Express-webbappen skapades.
2. Lägg till följande kod toohello överkant hello **app.js**
   
        var DocumentDBClient = require('documentdb').DocumentClient;
        var config = require('./config');
        var TaskList = require('./routes/tasklist');
        var TaskDao = require('./models/taskDao');
3. Den här koden definierar hello config-fil toobe används, och fortsätter tooread värden från denna fil till några variabler som vi snart ska använda.
4. Ersätt följande två rader i hello **app.js** fil:
   
        app.use('/', index);
        app.use('/users', users); 
   
      med följande kodavsnitt hello:
   
        var docDbClient = new DocumentDBClient(config.host, {
            masterKey: config.authKey
        });
        var taskDao = new TaskDao(docDbClient, config.databaseId, config.collectionId);
        var taskList = new TaskList(taskDao);
        taskDao.init();
   
        app.get('/', taskList.showTasks.bind(taskList));
        app.post('/addtask', taskList.addTask.bind(taskList));
        app.post('/completetask', taskList.completeTask.bind(taskList));
        app.set('view engine', 'jade');
5. Dessa rader definierar en ny instans av vårt **TaskDao** objekt med en ny anslutning tooAzure Cosmos DB (med hjälp av hello värden läsa från hello **config.js**), initiera hello aktivitetsobjektet och Binder formuläråtgärder toomethods på vår **TaskList** domänkontrollant. 
6. Slutligen, spara och Stäng hello **app.js** fil, vi är nästan klara.

## <a name="_Toc395783181"></a>Steg 5: Skapa ett användargränssnitt
Nu dags användargränssnittet våra uppmärksamhet toobuilding hello så att användaren faktiskt kan samverka med vår App. Hej Express-appen som vi skapade använder **Jade** som hello visningsmotor. Mer information om Jade finns för[http://jade-lang.com/](http://jade-lang.com/).

1. Hej **layout.jade** filen i hello **vyer** directory används som en global mall för andra **.jade** filer. I det här steget ändrar du den toouse [Twitter Bootstrap](https://github.com/twbs/bootstrap), vilket är en verktygslåda som gör det enkelt toodesign en snygg webbplats. 
2. Öppna hello **layout.jade** filen hittas i hello **vyer** mapp och Ersätt hello innehållet med hello följande:

    ```
    doctype html
    html
      head
        title= title
        link(rel='stylesheet', href='//ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap.min.css')
        link(rel='stylesheet', href='/stylesheets/style.css')
      body
        nav.navbar.navbar-inverse.navbar-fixed-top
          div.navbar-header
            a.navbar-brand(href='#') My Tasks
        block content
        script(src='//ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.2.min.js')
        script(src='//ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/bootstrap.min.js')
    ```

    Det säger hello **Jade** motorn toorender del HTML för vårt program och skapar en **block** kallas **innehåll** där vi kan tillhandahålla hello layout för våra innehåll sidor.

    Spara och stäng filen **layout.jade**.

3. Öppna hello **index.jade** fil, hello vy som ska användas av vårt program och Ersätt hello innehållet hello-filen med hello följande:
   
        extends layout
        block content
           h1 #{title}
           br
        
           form(action="/completetask", method="post")
             table.table.table-striped.table-bordered
               tr
                 td Name
                 td Category
                 td Date
                 td Complete
               if (typeof tasks === "undefined")
                 tr
                   td
               else
                 each task in tasks
                   tr
                     td #{task.name}
                     td #{task.category}
                     - var date  = new Date(task.date);
                     - var day   = date.getDate();
                     - var month = date.getMonth() + 1;
                     - var year  = date.getFullYear();
                     td #{month + "/" + day + "/" + year}
                     td
                       input(type="checkbox", name="#{task.id}", value="#{!task.completed}", checked=task.completed)
             button.btn.btn-primary(type="submit") Update tasks
           hr
           form.well(action="/addtask", method="post")
             .form-group
               label(for="name") Item Name:
               input.form-control(name="name", type="textbox")
             .form-group
               label(for="category") Item Category:
               input.form-control(name="category", type="textbox")
             br
             button.btn(type="submit") Add item
   

Det utökar layouten och ger innehåll för hello **innehåll** platshållare som vi såg i hello **layout.jade** filen tidigare.
   
I den här layouten skapade vi två HTML-formulär.

hello första formuläret innehåller en tabell för våra data och en knapp som gör att vi tooupdate objekt genom att publicera för**/completetask** metod i styrningen.
    
hello andra formuläret innehåller två inmatningsfält och en knapp som gör att vi toocreate ett nytt objekt genom att publicera för**/addtask** metod i styrningen.

Detta bör vara allt som behövs för vårt program toowork.

## <a name="_Toc395783181"></a>Steg 6: Kör ditt program lokalt
1. tootest hello programmet på din lokala dator kör `npm start` i hello terminal toostart ditt program och uppdatera sedan din [http://localhost: 3000](http://localhost:3000) Webbläsarsida. hello sidan bör nu se ut så hello bilden nedan:
   
    ![Skärmbild av hello programmet MyTodo List i ett webbläsarfönster](./media/documentdb-nodejs-application/cosmos-db-node-js-localhost.png)

    > [!TIP]
    > Om du får ett felmeddelande om hello indrag i hello layout.jade fil eller hello index.jade säkerställa att hello två första raderna i filer som är kvar berättigade, utan blanksteg. Om det finns blanksteg före hello två första raderna, ta bort dem, spara filer och sedan uppdatera webbläsarfönstret. 

2. Hello objekt, objektnamn och kategori fält tooenter en ny uppgift och klicka sedan på **Lägg till objekt**. När du gör det skapas ett dokument i Azure Cosmos DB med dessa egenskaper. 
3. hello sidan bör uppdateras toodisplay hello nyligen skapade objektet i hello ToDo-listan.
   
    ![Skärmdump av programmet hello med ett nytt objekt i hello göra-lista](./media/documentdb-nodejs-application/cosmos-db-node-js-added-task.png)
4. toocomplete en aktivitet bara kryssrutan hello i hello fullständig kolumnen och klicka sedan på **uppdatera aktiviteter**. Detta uppdaterar hello-dokument som du redan har skapat.

5. toostop hello program, tryck på CTRL + C i hello terminalfönster och klicka sedan på **Y** tooterminate hello batch-jobbet.

## <a name="_Toc395783182"></a>Steg 7: Distribuera ditt program development projektet tooAzure webbplatser
1. Om du inte redan gjort det aktiverar du en git-databas för Azure-webbplatsen. Du hittar anvisningar om hur toodo detta i hello [lokal Git-distribution tooAzure Apptjänst](../app-service-web/app-service-deploy-local-git.md) avsnittet.
2. Lägg till Azure-webbplatsen som en fjärransluten git.
   
        git remote add azure https://username@your-azure-website.scm.azurewebsites.net:443/your-azure-website.git
3. Distribuera genom att trycka på toohello fjärråtkomst.
   
        git push azure master
4. I några sekunder har git publicerat din webbapp och öppnar en webbläsare där du kan se ditt arbete som körs i Azure!

    Grattis! Du har precis skapat din första Node.js Express webbprogram med hjälp av Azure Cosmos DB och publicerat den tooAzure webbplatser.

    Om du vill toodownload eller referera toohello fullständiga referens program för den här kursen kan hämta från [GitHub][GitHub].

## <a name="_Toc395637775"></a>Nästa steg

* Vill tooperform skala och prestandatester med Azure Cosmos DB? Mer information finns i avsnittet om hur du [testar prestanda och skalning med Azure Cosmos DB](performance-testing.md)
* Lär dig hur för[övervaka ett konto i Azure Cosmos DB](monitor-accounts.md).
* Kör frågor mot vår exempeldatauppsättning i hello [Query Playground](https://www.documentdb.com/sql/demo).
* Utforska hello [Azure Cosmos DB dokumentationen](https://docs.microsoft.com/azure/documentdb/).

[Node.js]: http://nodejs.org/
[Git]: http://git-scm.com/
[GitHub]: https://github.com/Azure-Samples/documentdb-node-todo-app

