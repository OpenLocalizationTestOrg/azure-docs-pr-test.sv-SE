---
title: "aaaNode.js webbapp med hello Azure Table-tjänsten"
description: "Den här kursen lär du dig hur toouse hello Azure Table tjänsten toostore data från ett Node.js-program som finns i Azure App Service Web Apps."
tags: azure-portal
services: app-service\web, storage
documentationcenter: nodejs
author: TomArcher
manager: routlaw
editor: 
ms.assetid: 029e6f46-f586-4309-adbf-71c7b8d537d4
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: article
ms.date: 08/17/2016
ms.author: tarcher
ms.openlocfilehash: f6e08335b4c7f62f7b3994287edd586860cb7135
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="nodejs-web-app-using-hello-azure-table-service"></a>Node.js-webbapp med hjälp av hello Azure Table-tjänsten
## <a name="overview"></a>Översikt
Den här kursen visar hur toouse tabelltjänsten som tillhandahålls av Azure Data Management toostore och komma åt data från en [nod] program finns i [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) Web Apps. Den här kursen förutsätter att du har tidigare erfarenhet av noden och [Git].

Du kommer att lära dig:

* Hur toouse npm (noden package manager) tooinstall hello nod moduler
* Hur toowork med hello Azure tabelltjänst
* Hur toouse hello Azure CLI toocreate ett webbprogram.

Genom att följa den här självstudiekursen skapar du en enkel webbaserad ”uppgiftslistan” program som kan skapa, hämta och uppgifter som slutförs. hello uppgifter lagras i hello tabelltjänsten.

Här är hello slutförts program:

![En webbsida med en tom tasklist][node-table-finished]

> [!NOTE]
> Om du vill tooget igång med Azure App Service innan du registrerar dig för ett Azure-konto går för[prova App Service](https://azure.microsoft.com/try/app-service/), där kan du direkt skapa en tillfällig startwebbapp i App Service. Inget kreditkort krävs, och du gör inga åtaganden.
> 
> 

## <a name="prerequisites"></a>Krav
Innan du följer hello anvisningarna i den här artikeln bör du kontrollera att du har hello följande installerat:

* [nod] version 0.10.24 eller högre
* [Git]

[!INCLUDE [create-account-and-websites-note](../../includes/create-account-and-websites-note.md)]

## <a name="create-a-storage-account"></a>skapar ett lagringskonto
Skapa ett Azure-lagringskonto. hello appen kommer att använda det här kontot toostore hello arbetsuppgifter.

1. Logga in på hello [Azure Portal](https://portal.azure.com/).
2. Klicka på hello **ny** ikon på hello längst ned till vänster i hello portal och klicka sedan på **Data + lagring** > **lagring**. Ge hello storage-konto ett unikt namn och skapa en ny [resursgruppen](../azure-resource-manager/resource-group-overview.md) för den.
   
      ![Knappen Nytt](./media/storage-nodejs-use-table-storage-web-site/configure-storage.png)
   
    När hello storage-konto har skapats, hello **meddelanden** knappen blinkar en grön **lyckade** och hello lagringskontots blad är öppet tooshow det hör toohello ny resurs gruppen du Skapa.
3. I bladet hello lagringskonto klickar du på **inställningar** > **nycklar**. Kopiera hello primära nyckel toohello Urklipp.
   
    ![Snabbtangent][portal-storage-access-keys]

## <a name="install-modules-and-generate-scaffolding"></a>Installera moduler och generera scaffold-teknik
I det här avsnittet ska du skapa ett nytt program för nod och använder npm tooadd modulen paket. För det här programmet använder du hello [Express] och [Azure] moduler. hello Express modulen innehåller ett Model View Controller ramverk för noden, när hello Azure moduler ger anslutningsbarhet toohello tabelltjänsten.

### <a name="install-express-and-generate-scaffolding"></a>Installera express och generera scaffold-teknik
1. Skapa en ny katalog med namnet från kommandoraden hello **tasklist** och växel toothat directory.  
2. Ange hello efter kommandot tooinstall hello Express modulen.
   
        npm install express-generator@4.2.0 -g
   
    Hello operativsystem måste du använda tooput 'sudo-innan hello-kommando:
   
        sudo npm install express-generator@4.2.0 -g
   
    hello utdata visas liknande toohello följande exempel:
   
        express-generator@4.2.0 /usr/local/lib/node_modules/express-generator
        ├── mkdirp@0.3.5
        └── commander@1.3.2 (keypress@0.1.0)
   
   > [!NOTE]
   > Hej ”-g-parametern installerar hello modulen globalt. På så sätt kan vi använda **express** toogenerate web app scaffold-teknik utan tootype i ytterligare information om sökvägen.
   > 
   > 
3. toocreate hello scaffold-teknik för hello program ange hello **express** kommando:
   
        express
   
    hello utdata från kommandot visas liknande toohello följande exempel:
   
           create : .
           create : ./package.json
           create : ./app.js
           create : ./public
           create : ./public/images
           create : ./routes
           create : ./routes/index.js
           create : ./routes/users.js
           create : ./public/stylesheets
           create : ./public/stylesheets/style.css
           create : ./views
           create : ./views/index.jade
           create : ./views/layout.jade
           create : ./views/error.jade
           create : ./public/javascripts
           create : ./bin
           create : ./bin/www
   
           install dependencies:
             $ cd . && npm install
   
           run hello app:
             $ DEBUG=my-application ./bin/www
   
    Nu har du flera nya kataloger och filer i hello **tasklist** directory.

### <a name="install-additional-modules"></a>Installera ytterligare moduler
En av hello filer som **express** skapar är **package.json**. Den här filen innehåller en lista över beroenden för modulen. Senare, när du distribuerar hello programmet tooApp Service Web Apps kan den här filen avgör vilka moduler måste toobe installerad på Azure.

Ange följande kommando tooinstall hello moduler som beskrivs i hello hello från kommandoraden hello, **package.json** fil. Du kan behöva toouse ”sudo”.

    npm install

hello utdata från kommandot visas liknande toohello följande exempel:

    debug@0.7.4 node_modules\debug

    cookie-parser@1.0.1 node_modules\cookie-parser
    ├── cookie-signature@1.0.3
    └── cookie@0.1.0

    [...]


Skriv följande kommando tooinstall hello hello [azure], [nod uuid], [nconf] och [asynkrona] moduler:

    npm install azure-storage node-uuid async nconf --save

Hej **--spara** flaggan lägger till poster för dessa moduler toohello **package.json** fil.

hello utdata från kommandot visas liknande toohello följande exempel:

    async@0.9.0 node_modules\async

    node-uuid@1.4.1 node_modules\node-uuid

    nconf@0.6.9 node_modules\nconf
    ├── ini@1.2.1
    ├── async@0.2.9
    └── optimist@0.6.0 (wordwrap@0.0.2, minimist@0.0.10)

    [...]


## <a name="create-hello-application"></a>Skapa hello program
Vi är nu redo toobuild hello program.

### <a name="create-a-model"></a>Skapa en modell
En *modellen* är ett objekt som representerar hello data i ditt program. För hello programmet är hello endast modellen ett task-objekt som representerar ett objekt i hello att göra-lista. Aktiviteter har hello följande fält:

* PartitionKey
* RowKey
* namnet (sträng)
* kategori (sträng)
* slutförda (Boolean)

**PartitionKey** och **RowKey** används hello Tabelltjänsten som registernycklar. Mer information finns i [förstå hello tabelltjänst-datamodellen](https://msdn.microsoft.com/library/azure/dd179338.aspx).

1. I hello **tasklist** directory, skapa en ny katalog med namnet **modeller**.
2. I hello **modeller** directory, skapa en ny fil med namnet **task.js**. Den här filen innehåller hello modellen för hello aktiviteter som skapats av programmet.
3. Hello början av hello **task.js** lägger du till följande kod tooreference krävs bibliotek hello:
   
        var azure = require('azure-storage');
          var uuid = require('node-uuid');
        var entityGen = azure.TableUtilities.entityGenerator;
4. Lägg till följande hello code toodefine och exportera hello Task-objekt. Det här objektet är ansvarig för att ansluta toohello tabell.
   
          module.exports = Task;
   
        function Task(storageClient, tableName, partitionKey) {
          this.storageClient = storageClient;
          this.tableName = tableName;
          this.partitionKey = partitionKey;
          this.storageClient.createTableIfNotExists(tableName, function tableCreated(error) {
            if(error) {
              throw error;
            }
          });
        };
5. Lägg till hello följande kod toodefine fler metoder på hello aktivitetsobjektet som tillåter samverkan med data som lagras i tabellen hello:
   
        Task.prototype = {
          find: function(query, callback) {
            self = this;
            self.storageClient.queryEntities(this.tableName, query, null, function entitiesQueried(error, result) {
              if(error) {
                callback(error);
              } else {
                callback(null, result.entries);
              }
            });
          },
   
          addItem: function(item, callback) {
            self = this;
            // use entityGenerator tooset types
            // NOTE: RowKey must be a string type, even though
            // it contains a GUID in this example.
            var itemDescriptor = {
              PartitionKey: entityGen.String(self.partitionKey),
              RowKey: entityGen.String(uuid()),
              name: entityGen.String(item.name),
              category: entityGen.String(item.category),
              completed: entityGen.Boolean(false)
            };
            self.storageClient.insertEntity(self.tableName, itemDescriptor, function entityInserted(error) {
              if(error){  
                callback(error);
              }
              callback(null);
            });
          },
   
          updateItem: function(rKey, callback) {
            self = this;
            self.storageClient.retrieveEntity(self.tableName, self.partitionKey, rKey, function entityQueried(error, entity) {
              if(error) {
                callback(error);
              }
              entity.completed._ = true;
              self.storageClient.updateEntity(self.tableName, entity, function entityUpdated(error) {
                if(error) {
                  callback(error);
                }
                callback(null);
              });
            });
          }
        }
6. Spara och Stäng hello **task.js** fil.

### <a name="create-a-controller"></a>Skapa en domänkontrollant
En *domänkontrollant* hanterar HTTP-begäranden och återger hello HTML-svaret.

1. I hello **tasklist/vägar** directory, skapa en ny fil med namnet **tasklist.js** och öppna den i en textredigerare.
2. Lägg till följande kod för hello**tasklist.js**. Den läser in hello azure och async-moduler som används av **tasklist.js**. Detta definierar också hello **TaskList** funktion, som mottar en instans av hello **aktivitet** objektet som vi definierade tidigare:
   
        var azure = require('azure-storage');
        var async = require('async');
   
        module.exports = TaskList;
3. Definiera en **TaskList** objekt.
   
        function TaskList(task) {
          this.task = task;
        }
4. Lägg till följande metoder för hello**TaskList**:
   
        TaskList.prototype = {
          showTasks: function(req, res) {
            self = this;
            var query = new azure.TableQuery()
              .where('completed eq ?', false);
            self.task.find(query, function itemsFound(error, items) {
              res.render('index',{title: 'My ToDo List ', tasks: items});
            });
          },
   
          addTask: function(req,res) {
            var self = this;
            var item = req.body.item;
            self.task.addItem(item, function itemAdded(error) {
              if(error) {
                throw error;
              }
              res.redirect('/');
            });
          },
   
          completeTask: function(req,res) {
            var self = this;
            var completedTasks = Object.keys(req.body);
            async.forEach(completedTasks, function taskIterator(completedTask, callback) {
              self.task.updateItem(completedTask, function itemsUpdated(error) {
                if(error){
                  callback(error);
                } else {
                  callback(null);
                }
              });
            }, function goHome(error){
              if(error) {
                throw error;
              } else {
               res.redirect('/');
              }
            });
          }
        }

### <a name="modify-appjs"></a>Ändra app.js
1. Från hello **tasklist** katalog, öppna hello **app.js** fil. Den här filen skapades tidigare genom att köra hello **express** kommando.
2. Hello början av hello-fil, lägger du till hello följande tooload hello azure modul, set hello tabellnamn, partitionsnyckel och ange hello lagring autentiseringsuppgifter som används av det här exemplet:
   
        var azure = require('azure-storage');
        var nconf = require('nconf');
        nconf.env()
             .file({ file: 'config.json', search: true });
        var tableName = nconf.get("TABLE_NAME");
        var partitionKey = nconf.get("PARTITION_KEY");
        var accountName = nconf.get("STORAGE_NAME");
        var accountKey = nconf.get("STORAGE_KEY");
   
   > [!NOTE]
   > nconf läses hello konfigurationsvärden från miljövariabler eller hello **config.json** fil som skapas senare.
   > 
   > 
3. Rulla ned toowhere som du ser i filen app.js hello hello följande rad:
   
        app.use('/', routes);
        app.use('/users', users);
   
    Ersätt hello ovan rader med hello koden nedan. Detta ska initiera en instans av <strong>aktivitet</strong> med en anslutning tooyour storage-konto. Detta skickas toohello <strong>TaskList</strong>, som kommer att använda den toocommunicate med hello tabelltjänsten:
   
        var TaskList = require('./routes/tasklist');
        var Task = require('./models/task');
        var task = new Task(azure.createTableService(accountName, accountKey), tableName, partitionKey);
        var taskList = new TaskList(task);
   
        app.get('/', taskList.showTasks.bind(taskList));
        app.post('/addtask', taskList.addTask.bind(taskList));
        app.post('/completetask', taskList.completeTask.bind(taskList));
4. Spara hello **app.js** fil.

### <a name="modify-hello-index-view"></a>Ändra hello indexvy
1. Öppna hello **tasklist/views/index.jade** i en textredigerare.
2. Ersätt hello hela innehållet i filen hello med hello följande kod. Detta definierar en vy som visar befintliga aktiviteter och innehåller ett formulär för att lägga till nya aktiviteter och markera befintliga som slutförts.
   
        extends layout
   
        block content
          h1= title
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
                    td #{task.name._}
                    td #{task.category._}
                    - var day   = task.Timestamp._.getDate();
                    - var month = task.Timestamp._.getMonth() + 1;
                    - var year  = task.Timestamp._.getFullYear();
                    td #{month + "/" + day + "/" + year}
                    td
                      input(type="checkbox", name="#{task.RowKey._}", value="#{!task.completed._}", checked=task.completed._)
            button.btn(type="submit") Update tasks
          hr
          form.well(action="/addtask", method="post")
            label Item Name:
            input(name="item[name]", type="textbox")
            label Item Category:
            input(name="item[category]", type="textbox")
            br
            button.btn(type="submit") Add item
3. Spara och Stäng **index.jade** fil.

### <a name="modify-hello-global-layout"></a>Ändra hello globala layout
Hej **layout.jade** filen i hello **vyer** katalogen är en global mall för andra **.jade** filer. I det här steget ändrar du den toouse [Twitter Bootstrap](https://github.com/twbs/bootstrap), vilket är en verktygslåda som gör det enkelt toodesign ett bra söker webbprogram.

Ladda ned och extrahera hello filer för [Twitter Bootstrap](http://getbootstrap.com/). Kopiera hello **bootstrap.min.css** filen från hello Bootstrap **css** till hello mappen **offentlig/matmallar** katalog för programmet.

Från hello **vyer** mappen öppnar **layout.jade** och Ersätt hello hela innehållet med hello följande:

    doctype html
    html
      head
        title= title
        link(rel='stylesheet', href='/stylesheets/bootstrap.min.css')
        link(rel='stylesheet', href='/stylesheets/style.css')
      body.app
        nav.navbar.navbar-default
          div.navbar-header
          a.navbar-brand(href='/') My Tasks
        block content

### <a name="create-a-config-file"></a>Skapa en konfigurationsfil
toorun hello appen lokalt vi ska lägga till Azure Storage-autentiseringsuppgifter i en konfigurationsfil. Skapa en fil med namnet **config.json* * med hello följande JSON:

    {
        "STORAGE_NAME": "<storage account name>",
        "STORAGE_KEY": "<storage access key>",
        "PARTITION_KEY": "mytasks",
        "TABLE_NAME": "tasks"
    }

Ersätt **lagringskontonamnet** med hello namnet hello storage-kontot du skapade tidigare och ersätter **lagringsåtkomstnyckel** med hello primärnyckeln för ditt lagringskonto. Exempel:

    {
        "STORAGE_NAME": "nodejsappstorage",
        "STORAGE_KEY": "KG0oDd..."
        "PARTITION_KEY": "mytasks",
        "TABLE_NAME": "tasks"
    }

Spara filen *en katalog högre nivå* än hello **tasklist** katalog så här:

    parent/
      |-- config.json
      |-- tasklist/

hello beror för att göra detta på tooavoid kontrollerar hello-konfigurationsfilen i källkontroll där den kan bli offentliga. När vi distribuerar hello app tooAzure ska vi använda miljövariabler i stället för en .config-fil.

## <a name="run-hello-application-locally"></a>Kör hello programmet lokalt
tootest hello program på den lokala datorn, utför hello följande steg:

1. Från kommandoraden hello, ändra kataloger toohello **tasklist** directory.
2. Använd följande kommando toolaunch hello programmet lokalt hello:
   
        npm start
3. Öppna en webbläsare och gå toohttp://127.0.0.1:3000.
   
    En webbsida liknande toohello följande exempel visas.
   
    ![En webbsida visas en tom tasklist][node-table-finished]
4. toocreate ett nytt att göra-objekt, ange ett namn och kategori och klicka på **Lägg till objekt**. 
5. toomark en aktivitet som slutförd, kontrollera **Slutför** och på **Uppdateringsuppgifter**.
   
    ![En avbildning av hello nytt objekt i listan hello uppgifter][node-table-list-items]

Även om hello programmet körs lokalt, lagras hello data i hello Azure Table-tjänsten.

## <a name="deploy-your-application-tooazure"></a>Distribuera programmet-tooAzure
hello stegen i det här avsnittet använder hello Azure kommandoradsverktyg toocreate en ny webbapp i App Service och sedan använda Git toodeploy ditt program. tooperform dessa steg som du måste ha en Azure-prenumeration.

> [!NOTE]
> De här stegen kan också utföras med hjälp av hello [Azure Portal](https://portal.azure.com/). Se [skapa och distribuera en Node.js-webbapp i Azure App Service].
> 
> Om detta är hello första webbapp du har skapat, måste du använda hello Azure Portal toodeploy det här programmet.
> 
> 

tooget igång, installera hello [Azure CLI] genom att ange följande kommando från kommandoraden hello hello:

    npm install azure-cli -g

### <a name="import-publishing-settings"></a>Importera publiceringsinställningarna
I det här steget ska du hämta en fil som innehåller information om din prenumeration.

1. Ange hello följande kommando:
   
        azure login
   
    Detta kommando startar en webbläsare och navigerar toohello hämtningssidan. Om du uppmanas logga in med hello-konto som är associerade med din Azure-prenumeration.
   
    <!-- ![hello download page][download-publishing-settings] -->
   
    hello Filhämtning startar automatiskt. Om det inte du klickar på hello länk hello början av hello toomanually download hello växlingsfilen. Spara hello fil- och Observera hello filsökväg.
2. Ange hello följande tooimport hello inställningar:
   
        azure account import <path-to-file>
   
    Ange hello sökvägen och filnamnet för hello publicera inställningsfilen som du hämtade i hello föregående steg.
3. Ta bort hello när hello inställningarna importeras inställningsfilen för publicering. Den längre behövs inte och innehåller känslig information om din Azure-prenumeration.

### <a name="create-an-app-service-web-app"></a>Skapa en Apptjänst-webbapp
1. Från kommandoraden hello, ändra kataloger toohello **tasklist** directory.
2. Använd följande kommando toocreate ett nytt webbprogram hello.
   
        azure site create --git
   
    Du uppmanas att hello webbprogrammets namn och plats. Ange ett unikt namn och välj hello samma geografiska plats som ditt Azure Storage-konto.
   
    Hej `--git` parametern skapar en Git-lagringsplats i Azure för det här webbprogrammet. Dessutom initieras en Git-lagringsplats i hello aktuella katalogen om ingen finns, och lägger till en [Git remote] med namnet ”azure”, vilket är används toopublish hello programmet tooAzure. Slutligen skapas en **web.config** filen som innehåller inställningarna som används av Azure toohost noden program. Om du utelämnar hello `--git` parametern men hello directory innehåller en Git-lagringsplats, hello kommandot kommer fortfarande skapa hello ”azure” fjärransluten.
   
    När det här kommandot har slutförts visas utdata liknande toohello följande. Observera att hello rad som börjar med **webbplatsen skapas på** innehåller hello URL för hello webbprogrammet.
   
        info:   Executing command site create
        help:   Need a site name
        Name: TableTasklist
        info:   Using location southcentraluswebspace
        info:   Executing `git init`
        info:   Creating default .gitignore file
        info:   Creating a new web site
        info:   Created web site at  tabletasklist.azurewebsites.net
        info:   Initializing repository
        info:   Repository initialized
        info:   Executing `git remote add azure https://username@tabletasklist.azurewebsites.net/TableTasklist.git`
        info:   site create command OK
   
   > [!NOTE]
   > Du kommer att instrueras toouse hello Azure Portal toocreate hello webbprogram om det här är hello första App Service webbapp för din prenumeration. Mer information finns i [skapa och distribuera en Node.js-webbapp i Azure App Service].
   > 
   > 

### <a name="set-environment-variables"></a>Uppsättning miljövariabler
I det här steget ska du lägga till miljön variabler tooyour webbappkonfigurationen på Azure.
Ange hello följande från hello kommandorad:

    azure site appsetting add
        STORAGE_NAME=<storage account name>;STORAGE_KEY=<storage access key>;PARTITION_KEY=mytasks;TABLE_NAME=tasks


Ersätt  **<storage account name>**  med hello namnet hello storage-kontot du skapade tidigare och ersätter  **<storage access key>**  med hello primärnyckeln för ditt lagringskonto. (Använd hello samma värden som hello config.json-fil som du skapade tidigare.)

Du kan också ange miljövariabler i hello [Azure Portal](https://portal.azure.com/):

1. Öppna hello webbapps blad genom att klicka på **Bläddra** > **Web Apps** > din webbprogrammets namn.
2. I din webbapps blad klickar du på **alla inställningar** > **programinställningar**.
   
     <!-- ![Top Menu](./media/storage-nodejs-use-table-storage-web-site/PollsCommonWebSiteTopMenu.png) -->
3. Bläddra nedåt toohello **appinställningar** och lägger till hello nyckel/värde-par.
   
     ![App-inställningar](./media/storage-nodejs-use-table-storage-web-site/storage-tasks-appsettings.png)
4. Klicka på **SPARA**.

### <a name="publish-hello-application"></a>Publicera programmet hello
toopublish hello app genomför hello kod filer tooGit och sedan skicka tooazure eller huvudtjänsten.

1. Ange dina autentiseringsuppgifter för distribution.
   
        azure site deployment user set <name> <password>
2. Lägg till och genomför programfiler.
   
        git add .
        git commit -m "adding files"
3. Push hello commit toohello App Service webbapp:
   
        git push azure master
   
    Använd **master** som hello mål gren. Hello slutet av hello distribution, kan du se en instruktion liknande toohello följande exempel:
   
        toohttps://username@tabletasklist.azurewebsites.net/TableTasklist.git
          * [new branch]      master -> master
4. När hello push har slutförts, bläddra toohello web app-URL som tidigare returnerats av hello `azure create site` kommandot tooview ditt program.

## <a name="next-steps"></a>Nästa steg
Hello steg i den här artikeln beskriver med hello tabell toostore tjänstinformation, du kan också använda [MongoDB](https://mlab.com/azure/). 

## <a name="additional-resources"></a>Ytterligare resurser
[Azure CLI]

## <a name="whats-changed"></a>Nyheter
* En guide toohello övergången från webbplatser tooApp tjänsten finns: [Azure App Service och dess påverkan på befintliga Azure-tjänster](http://go.microsoft.com/fwlink/?LinkId=529714)

<!-- URLs -->

[skapa och distribuera en Node.js-webbapp i Azure App Service]: app-service-web-get-started-nodejs.md
[Azure Developer Center]: /develop/nodejs/

[nod]: http://nodejs.org
[Git]: http://git-scm.com
[Express]: http://expressjs.com
[for free]: http://windowsazure.com
[Git remote]: http://git-scm.com/docs/git-remote

[Azure CLI]:../cli-install-nodejs.md

[azure]: https://github.com/Azure/azure-sdk-for-node
[nod uuid]: https://www.npmjs.com/package/node-uuid
[nconf]: https://www.npmjs.com/package/nconf
[asynkrona]: https://www.npmjs.com/package/async

[Azure Portal]: https://portal.azure.com

[Create and deploy a Node.js application tooan Azure Web Site]: app-service-web-get-started-nodejs.md

<!-- Image References -->

[node-table-finished]: ./media/storage-nodejs-use-table-storage-web-site/table_todo_empty.png
[node-table-list-items]: ./media/storage-nodejs-use-table-storage-web-site/table_todo_list.png
[download-publishing-settings]: ./media/storage-nodejs-use-table-storage-web-site/azure-account-download-cli.png
[portal-storage-access-keys]: ./media/storage-nodejs-use-table-storage-web-site/manage-access-keys.png
[app-settings]: ./media/storage-nodejs-use-table-storage-web-site/storage-tasks-appsettings.png
