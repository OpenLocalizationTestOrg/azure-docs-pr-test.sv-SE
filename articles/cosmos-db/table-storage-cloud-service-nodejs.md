---
title: aaaWeb app med table storage (Node.js) | Microsoft Docs
description: "En självstudiekurs som bygger på hello Web App med en snabb genomgång genom att lägga till Azure Storage-tjänster och hello Azure-modulen."
services: cosmos-db
documentationcenter: nodejs
author: mimig1
manager: jhubbard
editor: tysonn
ms.assetid: e90959a2-4cb2-4b19-9bfb-aede15b18b1c
ms.service: cosmos-db
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: article
ms.date: 12/08/2016
ms.author: mimig
ms.openlocfilehash: 7eefc09baab61cf44c98183135abe572b11812e9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="nodejs-web-application-using-storage"></a>Node.js-Webbapp som använder lagring
## <a name="overview"></a>Översikt
I den här självstudiekursen hello program som du skapade i den [Node.js-Webbapp med hjälp av] kursen utökas med hello Microsoft Azure-klientbibliotek för Node.js toowork med data management-tjänster. Du kan utöka ditt program genom att skapa ett webbaserat-uppgiftslista program som du distribuerar tooAzure. hello uppgiftslista kan användaren hämta uppgifter, lägga till nya aktiviteter och markera aktiviteter som slutförda.

hello uppgiftsobjekt lagras i Azure Storage. Azure Storage tillhandahåller lagring av Ostrukturerade data som är feltolerant och hög tillgänglighet. Azure Storage innehåller flera datastrukturer där du kan lagra och komma åt data. Du kan använda hello lagringstjänster från hello API: er som ingår i hello Azure SDK för Node.js eller via REST API: er. Mer information finns i [lagring och åtkomst till Data i Azure].

Den här kursen förutsätter att du har slutfört hello [Node.js-Webbapp] och [Node.js med snabb][Node.js-Webbapp med hjälp av] självstudier.

Den innehåller hello följande information:

* Hur toowork med hello Jade mall-motorn
* Hur toowork med Azure Data Management-tjänster

hello följande skärmbild visar hello slutförts program:

![hello slutförts webbsida i internet explorer](./media/table-storage-cloud-service-nodejs/getting-started-1.png)

## <a name="setting-storage-credentials-in-webconfig"></a>Ange autentiseringsuppgifter för lagring i Web.Config
Du måste överföra i lagring autentiseringsuppgifter tooaccess Azure Storage. Detta görs genom att använda hello web.config Programinställningar.
hello web.config inställningar skickas miljö variabler tooNode som läses sedan av hello Azure SDK.

> [!NOTE]
> Lagring autentiseringsuppgifter används bara när programmet hello är distribuerade tooAzure. När du kör i hello-emulatorn använder hello programmet hello storage-emulatorn.
>
>

Utför följande steg tooretrieve hello lagringskontouppgifter hello och Lägg till dem toohello web.config-inställningar:

1. Om den inte redan är öppen, starta hello Azure PowerShell från hello **starta** menyn genom att expandera **alla program, Azure**, högerklicka på **Azure PowerShell**, och välj sedan  **Kör som administratör**.
2. Ändra kataloger toohello mapp som innehåller programmet. Till exempel C:\\nod\\tasklist\\WebRole1.
3. Ange följande cmdlet tooretrieve hello lagring kontoinformation hello från hello Azure Powershell-fönstret:

    ```powershell
    PS C:\node\tasklist\WebRole1> Get-AzureStorageAccounts
    ```

   hello hämtar föregående cmdlet hello lista över storage-konton och nycklar som är associerade med din värdbaserade tjänst.

   > [!NOTE]
   > Eftersom hello Azure SDK om du skapar ett lagringskonto när du distribuerar en tjänst, ska ett lagringskonto redan finnas från att distribuera ditt program i hello tidigare guider.
   >
   >
4. Öppna hello **ServiceDefinition.csdef** -fil som innehåller hello miljöinställningar som används när programmet hello är distribuerade tooAzure:

    ```powershell
    PS C:\node\tasklist> notepad ServiceDefinition.csdef
    ```

5. Infoga hello följande blockera **miljö** element, ersätter {LAGRINGSKONTO} och {LAGRINGSÅTKOMSTNYCKEL} med hello kontonamn och hello primärnyckeln för hello storage-konto som du vill använda toouse för distributionen:

  <Variable name="AZURE_STORAGE_ACCOUNT" value="{STORAGE ACCOUNT}" />
  <Variable name="AZURE_STORAGE_ACCESS_KEY" value="{STORAGE ACCESS KEY}" />

   ![Hej web.cloud.config filinnehåll](./media/table-storage-cloud-service-nodejs/node37.png)

6. Spara hello-filen och stäng Anteckningar.

### <a name="install-additional-modules"></a>Installera ytterligare moduler
1. Använd hello efter kommandot tooinstall hello [azure], [nod-uuid], [nconf] och [asynkrona] moduler lokalt samt toosave en post för dem toohello **package.json** fil:

  ```powershell
  PS C:\node\tasklist\WebRole1> npm install azure-storage node-uuid async nconf --save
  ```

  hello utdata från kommandot ska se ut ungefär toohello följande:

  ```
  node-uuid@1.4.1 node_modules\node-uuid

  nconf@0.6.9 node_modules\nconf
  ├── ini@1.1.0
  ├── async@0.2.9
  └── optimist@0.6.0 (wordwrap@0.0.2, minimist@0.0.8)

  azure-storage@0.1.0 node_modules\azure-storage
  ├── extend@1.2.1
  ├── xmlbuilder@0.4.3
  ├── mime@1.2.11
  ├── underscore@1.4.4
  ├── validator@3.1.0
  ├── node-uuid@1.4.1
  ├── xml2js@0.2.7 (sax@0.5.2)
  └── request@2.27.0 (json-stringify-safe@5.0.0, tunnel-agent@0.3.0, aws-sign@0.3.0, forever-agent@0.5.2, qs@0.6.6, oauth-sign@0.3.0, cookie-jar@0.3.0, hawk@1.0.0, form-data@0.1.3, http-signature@0.10.0)
  ```

## <a name="using-hello-table-service-in-a-node-application"></a>Använda hello tabelltjänsten i en node-App
I det här avsnittet hello grundläggande program som skapats av hello **express** kommandot utökas genom att lägga till en **task.js** -fil som innehåller hello modell för dina uppgifter. Ändra befintliga hello **app.js** filen och skapa en ny **tasklist.js** fil som använder hello modellen.

### <a name="create-hello-model"></a>Skapa hello modell
1. I hello **WebRole1** directory, skapa en ny katalog med namnet **modeller**.
2. I hello **modeller** directory, skapa en ny fil med namnet **task.js**. Den här filen innehåller hello modellen för hello aktiviteter som skapats av programmet.
3. Hello början av hello **task.js** lägger du till följande kod tooreference krävs bibliotek hello:

    ```nodejs
    var azure = require('azure-storage');
    var uuid = require('node-uuid');
    var entityGen = azure.TableUtilities.entityGenerator;
    ```

4. Sedan lägger du till kod toodefine och exportera hello Task-objekt. hello aktivitetsobjektet ansvarar för att ansluta toohello tabell.

    ```nodejs
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
    ```

5. Lägg till hello följande kod toodefine fler metoder på hello aktivitetsobjektet som tillåter samverkan med data som lagras i tabellen hello:

    ```nodejs
    Task.prototype = {
      find: function(query, callback) {
        self = this;
        self.storageClient.queryEntities(query, function entitiesQueried(error, result) {
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
    ```

6. Spara och Stäng hello **task.js** fil.

### <a name="create-hello-controller"></a>Skapa hello-styrenhet
1. I hello **WebRole1/vägar** directory, skapa en ny fil med namnet **tasklist.js** och öppna den i en textredigerare.
2. Lägg till följande kod för hello**tasklist.js**. Den här koden läser in hello azure och async-moduler som används av **tasklist.js** och definierar hello **TaskList** funktion, som mottar en instans av hello **aktivitet** objekt vi definierade tidigare:

    ```nodejs
    var azure = require('azure-storage');
    var async = require('async');

    module.exports = TaskList;

    function TaskList(task) {
      this.task = task;
    }
    ```

3. Fortsätt att lägga till toohello **tasklist.js** filen genom att lägga till hello-metoder som används för**showTasks**, **addTask**, och **completeTasks**:

    ```nodejs
    TaskList.prototype = {
      showTasks: function(req, res) {
        self = this;
        var query = azure.TableQuery()
          .where('completed eq ?', false);
        self.task.find(query, function itemsFound(error, items) {
          res.render('index',{title: 'My ToDo List ', tasks: items});
        });
      },

      addTask: function(req,res) {
        var self = this
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
    ```

4. Spara hello **tasklist.js** fil.

### <a name="modify-appjs"></a>Ändra app.js
1. I hello **WebRole1** katalog, öppna hello **app.js** i en textredigerare.
2. Lägg till hello följande tooload hello azure modulen hello början av hello-filen, och ange hello namn och partition tabellnyckel:

    ```nodejs
    var azure = require('azure-storage');
    var tableName = 'tasks';
    var partitionKey = 'hometasks';
    ```

3. Rulla ned toowhere som du ser i filen app.js hello hello följande rad:

    ```nodejs
    app.use('/', routes);
    app.use('/users', users);
    ```

    Ersätt hello föregående rader med hello följande kod. Den här koden initierar en instans av <strong>aktivitet</strong> med en anslutning tooyour storage-konto. Hej <strong>aktivitet</strong> skickas toohello <strong>TaskList</strong>, som använder den toocommunicate med hello tabelltjänsten:

    ```nodejs
    var TaskList = require('./routes/tasklist');
    var Task = require('./models/task');
    var task = new Task(azure.createTableService(), tableName, partitionKey);
    var taskList = new TaskList(task);

    app.get('/', taskList.showTasks.bind(taskList));
    app.post('/addtask', taskList.addTask.bind(taskList));
    app.post('/completetask', taskList.completeTask.bind(taskList));
    ```

4. Spara hello **app.js** fil.

### <a name="modify-hello-index-view"></a>Ändra hello indexvy
1. Ändra kataloger toohello **vyer** katalog och öppna hello **index.jade** i en textredigerare.
2. Ersätt hello innehållet i hello **index.jade** fil med hello följande kod. Den här koden definierar hello vy för visning av befintliga aktiviteter och definierar ett formulär för att lägga till nya aktiviteter och markera befintliga som slutförts.

    ```
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
          if tasks != []
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
    ```

3. Spara och Stäng **index.jade** fil.

### <a name="modify-hello-global-layout"></a>Ändra hello globala layout
Hej **layout.jade** filen i hello **vyer** directory används som en global mall för andra **.jade** filer. I det här steget ändrar hello **layout.jade** filen toouse [Twitter Bootstrap](https://github.com/twbs/bootstrap), vilket är en verktygslåda som gör det enkelt toodesign en snygg webbplats.

1. Ladda ned och extrahera hello filer för [Twitter Bootstrap](http://getbootstrap.com/). Kopiera hello **bootstrap.min.css** filen från hello **bootstrap\\dist\\css** mappen toohello **offentliga\\matmallar** katalogen för tillämpningsprogrammet tasklist.
2. Från hello **vyer** mapp, öppna hello **layout.jade** filen i en textredigerare och Ersätt hello innehållet med hello följande:

    doctype html html head rubrik = Rubriklänk (rel = 'stylesheet', href='/stylesheets/bootstrap.min.css') länk (rel = 'stylesheet', href='/stylesheets/style.css') body.app nav.navbar.navbar standard div.navbar huvud a.navbar-brand(href='/') min  Uppgifter blockera innehåll

3. Spara hello **layout.jade** fil.

### <a name="running-hello-application-in-hello-emulator"></a>Kör hello programmet hello emulatorn
Använd hello följande kommando toostart hello program i hello-emulatorn.

```powershell
PS C:\node\tasklist\WebRole1> start-azureemulator -launch
```

hello webbläsaren öppnas och visar hello följande sida:

![En webbplats som växlingsbart systemminne heter uppgiftslistan med en tabell som innehåller uppgifter och fält tooadd en ny uppgift.](./media/table-storage-cloud-service-nodejs/node44.png)

Använd hello formuläret tooadd objekt eller ta bort befintliga objekt genom att markera dem som slutförda.

## <a name="publishing-hello-application-tooazure"></a>Publishing hello programmet tooAzure
Anropa hello följande cmdlet tooredeploy värdtjänsten-tooAzure i hello Windows PowerShell-fönster.

```powershell
PS C:\node\tasklist\WebRole1> Publish-AzureServiceProject -name myuniquename -location datacentername -launch
```

Ersätt **myuniquename** med ett unikt namn för det här programmet. Ersätt **datacentername** med hello namnet på ett Azure-datacenter som **västra USA**.

När hello distributionen är klar, bör du se ett svar liknande toohello följande:

```
  PS C:\node\tasklist> publish-azureserviceproject -servicename tasklist -location "West US"
  WARNING: Publishing tasklist tooMicrosoft Azure. This may take several minutes...
  WARNING: 2:18:42 PM - Preparing runtime deployment for service 'tasklist'
  WARNING: 2:18:42 PM - Verifying storage account 'tasklist'...
  WARNING: 2:18:43 PM - Preparing deployment for tasklist with Subscription ID: 65a1016d-0f67-45d2-b838-b8f373d6d52e...
  WARNING: 2:19:01 PM - Connecting...
  WARNING: 2:19:02 PM - Uploading Package toostorage service larrystore...
  WARNING: 2:19:40 PM - Upgrading...
  WARNING: 2:22:48 PM - Created Deployment ID: b7134ab29b1249ff84ada2bd157f296a.
  WARNING: 2:22:48 PM - Initializing...
  WARNING: 2:22:49 PM - Instance WebRole1_IN_0 of role WebRole1 is ready.
  WARNING: 2:22:50 PM - Created Website URL: http://tasklist.cloudapp.net/.
```

Genom att ange hello **-starta** alternativet i hello tidigare cmdleten hello webbläsaren öppnas och visar programmet körs i Azure när publiceringen är klar.

![Ett webbläsarfönster som visar hello uppgiftslistan sidan. hello URL: en anger hello sidan finns nu på Azure.](./media/table-storage-cloud-service-nodejs/getting-started-1.png)

## <a name="stopping-and-deleting-your-application"></a>Stoppa och ta bort programmet
Du kanske vill toodisable den så att du kan undvika kostnader eller skapa och distribuera andra program inom hello kostnadsfri utvärderingsversion tidsperiod när du distribuerar ditt program.

Azure fakturerar webbrollsinstanser per timme förbrukad servertid.
Servertid förbrukas när programmet har distribuerats, även om instanserna inte körs och är i hello stoppats tillstånd.

hello följande steg visar hur toostop och ta bort programmet.

1. Stoppa hello tjänstdistributionen som skapades i föregående avsnitt i hello med hello följande cmdlet i Windows PowerShell-fönstret hello:

    ```powershell
    PS C:\node\tasklist\WebRole1> Stop-AzureService
    ```

   Hello-tjänsten stoppas kan det ta flera minuter. När hello har stoppats, kan du få ett meddelande som anger att den har stoppats.

2. toodelete hello service, anrop hello följande cmdlet:

    ```powershell
    PS C:\node\tasklist\WebRole1> Remove-AzureService contosotasklist
    ```

   När du uppmanas, anger **Y** toodelete hello-tjänsten.

   Om du tar bort hello service kan ta några minuter. När hello-tjänsten har tagits bort, visas ett meddelande som anger att hello-tjänsten har tagits bort.

[Node.js-Webbapp med hjälp av]: http://azure.microsoft.com/develop/nodejs/tutorials/web-app-with-express/
[lagring och åtkomst till Data i Azure]: http://msdn.microsoft.com/library/azure/gg433040.aspx
[Node.js-Webbapp]: http://azure.microsoft.com/develop/nodejs/tutorials/getting-started/


