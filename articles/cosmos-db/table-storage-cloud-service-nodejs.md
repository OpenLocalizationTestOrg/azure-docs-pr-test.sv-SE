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
# <a name="nodejs-web-application-using-storage"></a><span data-ttu-id="b71f4-103">Node.js-Webbapp som använder lagring</span><span class="sxs-lookup"><span data-stu-id="b71f4-103">Node.js Web Application using Storage</span></span>
## <a name="overview"></a><span data-ttu-id="b71f4-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="b71f4-104">Overview</span></span>
<span data-ttu-id="b71f4-105">I den här självstudiekursen hello program som du skapade i den [Node.js-Webbapp med hjälp av] kursen utökas med hello Microsoft Azure-klientbibliotek för Node.js toowork med data management-tjänster.</span><span class="sxs-lookup"><span data-stu-id="b71f4-105">In this tutorial, hello application you created in the [Node.js Web Application using Express] tutorial is extended using hello Microsoft Azure Client Libraries for Node.js toowork with data management services.</span></span> <span data-ttu-id="b71f4-106">Du kan utöka ditt program genom att skapa ett webbaserat-uppgiftslista program som du distribuerar tooAzure.</span><span class="sxs-lookup"><span data-stu-id="b71f4-106">You extend your application by creating a web-based task-list application that you can deploy tooAzure.</span></span> <span data-ttu-id="b71f4-107">hello uppgiftslista kan användaren hämta uppgifter, lägga till nya aktiviteter och markera aktiviteter som slutförda.</span><span class="sxs-lookup"><span data-stu-id="b71f4-107">hello task list allows a user to retrieve tasks, add new tasks, and mark tasks as completed.</span></span>

<span data-ttu-id="b71f4-108">hello uppgiftsobjekt lagras i Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="b71f4-108">hello task items are stored in Azure Storage.</span></span> <span data-ttu-id="b71f4-109">Azure Storage tillhandahåller lagring av Ostrukturerade data som är feltolerant och hög tillgänglighet.</span><span class="sxs-lookup"><span data-stu-id="b71f4-109">Azure Storage provides unstructured data storage that is fault-tolerant and highly available.</span></span> <span data-ttu-id="b71f4-110">Azure Storage innehåller flera datastrukturer där du kan lagra och komma åt data.</span><span class="sxs-lookup"><span data-stu-id="b71f4-110">Azure Storage includes several data structures where you can store and access data.</span></span> <span data-ttu-id="b71f4-111">Du kan använda hello lagringstjänster från hello API: er som ingår i hello Azure SDK för Node.js eller via REST API: er.</span><span class="sxs-lookup"><span data-stu-id="b71f4-111">You can use hello storage services from hello APIs included in hello Azure SDK for Node.js or via REST APIs.</span></span> <span data-ttu-id="b71f4-112">Mer information finns i [lagring och åtkomst till Data i Azure].</span><span class="sxs-lookup"><span data-stu-id="b71f4-112">For more information, see [Storing and Accessing Data in Azure].</span></span>

<span data-ttu-id="b71f4-113">Den här kursen förutsätter att du har slutfört hello [Node.js-Webbapp] och [Node.js med snabb][Node.js-Webbapp med hjälp av] självstudier.</span><span class="sxs-lookup"><span data-stu-id="b71f4-113">This tutorial assumes that you have completed hello [Node.js Web Application] and [Node.js with Express][Node.js Web Application using Express] tutorials.</span></span>

<span data-ttu-id="b71f4-114">Den innehåller hello följande information:</span><span class="sxs-lookup"><span data-stu-id="b71f4-114">It contains hello following information:</span></span>

* <span data-ttu-id="b71f4-115">Hur toowork med hello Jade mall-motorn</span><span class="sxs-lookup"><span data-stu-id="b71f4-115">How toowork with hello Jade template engine</span></span>
* <span data-ttu-id="b71f4-116">Hur toowork med Azure Data Management-tjänster</span><span class="sxs-lookup"><span data-stu-id="b71f4-116">How toowork with Azure Data Management services</span></span>

<span data-ttu-id="b71f4-117">hello följande skärmbild visar hello slutförts program:</span><span class="sxs-lookup"><span data-stu-id="b71f4-117">hello following screenshot shows hello completed application:</span></span>

![hello slutförts webbsida i internet explorer](./media/table-storage-cloud-service-nodejs/getting-started-1.png)

## <a name="setting-storage-credentials-in-webconfig"></a><span data-ttu-id="b71f4-119">Ange autentiseringsuppgifter för lagring i Web.Config</span><span class="sxs-lookup"><span data-stu-id="b71f4-119">Setting Storage Credentials in Web.Config</span></span>
<span data-ttu-id="b71f4-120">Du måste överföra i lagring autentiseringsuppgifter tooaccess Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="b71f4-120">You must pass in storage credentials tooaccess Azure Storage.</span></span> <span data-ttu-id="b71f4-121">Detta görs genom att använda hello web.config Programinställningar.</span><span class="sxs-lookup"><span data-stu-id="b71f4-121">This is done by utilizing hello web.config application settings.</span></span>
<span data-ttu-id="b71f4-122">hello web.config inställningar skickas miljö variabler tooNode som läses sedan av hello Azure SDK.</span><span class="sxs-lookup"><span data-stu-id="b71f4-122">hello web.config settings are passed as environment variables tooNode, which are then read by hello Azure SDK.</span></span>

> [!NOTE]
> <span data-ttu-id="b71f4-123">Lagring autentiseringsuppgifter används bara när programmet hello är distribuerade tooAzure.</span><span class="sxs-lookup"><span data-stu-id="b71f4-123">Storage credentials are only used when hello application is deployed tooAzure.</span></span> <span data-ttu-id="b71f4-124">När du kör i hello-emulatorn använder hello programmet hello storage-emulatorn.</span><span class="sxs-lookup"><span data-stu-id="b71f4-124">When running in hello emulator, hello application uses hello storage emulator.</span></span>
>
>

<span data-ttu-id="b71f4-125">Utför följande steg tooretrieve hello lagringskontouppgifter hello och Lägg till dem toohello web.config-inställningar:</span><span class="sxs-lookup"><span data-stu-id="b71f4-125">Perform hello following steps tooretrieve hello storage account credentials and add them toohello web.config settings:</span></span>

1. <span data-ttu-id="b71f4-126">Om den inte redan är öppen, starta hello Azure PowerShell från hello **starta** menyn genom att expandera **alla program, Azure**, högerklicka på **Azure PowerShell**, och välj sedan  **Kör som administratör**.</span><span class="sxs-lookup"><span data-stu-id="b71f4-126">If it is not already open, start hello Azure PowerShell from hello **Start** menu by expanding **All Programs, Azure**, right-click **Azure PowerShell**, and then select **Run As Administrator**.</span></span>
2. <span data-ttu-id="b71f4-127">Ändra kataloger toohello mapp som innehåller programmet.</span><span class="sxs-lookup"><span data-stu-id="b71f4-127">Change directories toohello folder containing your application.</span></span> <span data-ttu-id="b71f4-128">Till exempel C:\\nod\\tasklist\\WebRole1.</span><span class="sxs-lookup"><span data-stu-id="b71f4-128">For example, C:\\node\\tasklist\\WebRole1.</span></span>
3. <span data-ttu-id="b71f4-129">Ange följande cmdlet tooretrieve hello lagring kontoinformation hello från hello Azure Powershell-fönstret:</span><span class="sxs-lookup"><span data-stu-id="b71f4-129">From hello Azure Powershell window, enter hello following cmdlet tooretrieve hello storage account information:</span></span>

    ```powershell
    PS C:\node\tasklist\WebRole1> Get-AzureStorageAccounts
    ```

   <span data-ttu-id="b71f4-130">hello hämtar föregående cmdlet hello lista över storage-konton och nycklar som är associerade med din värdbaserade tjänst.</span><span class="sxs-lookup"><span data-stu-id="b71f4-130">hello preceding cmdlet retrieves hello list of storage accounts and account keys associated with your hosted service.</span></span>

   > [!NOTE]
   > <span data-ttu-id="b71f4-131">Eftersom hello Azure SDK om du skapar ett lagringskonto när du distribuerar en tjänst, ska ett lagringskonto redan finnas från att distribuera ditt program i hello tidigare guider.</span><span class="sxs-lookup"><span data-stu-id="b71f4-131">Since hello Azure SDK creates a storage account when you deploy a service, a storage account should already exist from deploying your application in hello previous guides.</span></span>
   >
   >
4. <span data-ttu-id="b71f4-132">Öppna hello **ServiceDefinition.csdef** -fil som innehåller hello miljöinställningar som används när programmet hello är distribuerade tooAzure:</span><span class="sxs-lookup"><span data-stu-id="b71f4-132">Open hello **ServiceDefinition.csdef** file containing hello environment settings that are used when hello application is deployed tooAzure:</span></span>

    ```powershell
    PS C:\node\tasklist> notepad ServiceDefinition.csdef
    ```

5. <span data-ttu-id="b71f4-133">Infoga hello följande blockera **miljö** element, ersätter {LAGRINGSKONTO} och {LAGRINGSÅTKOMSTNYCKEL} med hello kontonamn och hello primärnyckeln för hello storage-konto som du vill använda toouse för distributionen:</span><span class="sxs-lookup"><span data-stu-id="b71f4-133">Insert hello following block under **Environment** element, substituting {STORAGE ACCOUNT} and {STORAGE ACCESS KEY} with hello account name and hello primary key for hello storage account you want toouse for deployment:</span></span>

  <Variable name="AZURE_STORAGE_ACCOUNT" value="{STORAGE ACCOUNT}" />
  <Variable name="AZURE_STORAGE_ACCESS_KEY" value="{STORAGE ACCESS KEY}" />

   ![Hej web.cloud.config filinnehåll](./media/table-storage-cloud-service-nodejs/node37.png)

6. <span data-ttu-id="b71f4-135">Spara hello-filen och stäng Anteckningar.</span><span class="sxs-lookup"><span data-stu-id="b71f4-135">Save hello file and close notepad.</span></span>

### <a name="install-additional-modules"></a><span data-ttu-id="b71f4-136">Installera ytterligare moduler</span><span class="sxs-lookup"><span data-stu-id="b71f4-136">Install additional modules</span></span>
1. <span data-ttu-id="b71f4-137">Använd hello efter kommandot tooinstall hello [azure], [nod-uuid], [nconf] och [asynkrona] moduler lokalt samt toosave en post för dem toohello **package.json** fil:</span><span class="sxs-lookup"><span data-stu-id="b71f4-137">Use hello following command tooinstall hello [azure], [node-uuid], [nconf] and [async] modules locally as well as toosave an entry for them toohello **package.json** file:</span></span>

  ```powershell
  PS C:\node\tasklist\WebRole1> npm install azure-storage node-uuid async nconf --save
  ```

  <span data-ttu-id="b71f4-138">hello utdata från kommandot ska se ut ungefär toohello följande:</span><span class="sxs-lookup"><span data-stu-id="b71f4-138">hello output of this command should appear similar toohello following:</span></span>

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

## <a name="using-hello-table-service-in-a-node-application"></a><span data-ttu-id="b71f4-139">Använda hello tabelltjänsten i en node-App</span><span class="sxs-lookup"><span data-stu-id="b71f4-139">Using hello Table service in a node application</span></span>
<span data-ttu-id="b71f4-140">I det här avsnittet hello grundläggande program som skapats av hello **express** kommandot utökas genom att lägga till en **task.js** -fil som innehåller hello modell för dina uppgifter.</span><span class="sxs-lookup"><span data-stu-id="b71f4-140">In this section, hello basic application created by hello **express** command is extended by adding a **task.js** file containing hello model for your tasks.</span></span> <span data-ttu-id="b71f4-141">Ändra befintliga hello **app.js** filen och skapa en ny **tasklist.js** fil som använder hello modellen.</span><span class="sxs-lookup"><span data-stu-id="b71f4-141">Modify hello existing **app.js** file and create a new **tasklist.js** file that uses hello model.</span></span>

### <a name="create-hello-model"></a><span data-ttu-id="b71f4-142">Skapa hello modell</span><span class="sxs-lookup"><span data-stu-id="b71f4-142">Create hello model</span></span>
1. <span data-ttu-id="b71f4-143">I hello **WebRole1** directory, skapa en ny katalog med namnet **modeller**.</span><span class="sxs-lookup"><span data-stu-id="b71f4-143">In hello **WebRole1** directory, create a new directory named **models**.</span></span>
2. <span data-ttu-id="b71f4-144">I hello **modeller** directory, skapa en ny fil med namnet **task.js**.</span><span class="sxs-lookup"><span data-stu-id="b71f4-144">In hello **models** directory, create a new file named **task.js**.</span></span> <span data-ttu-id="b71f4-145">Den här filen innehåller hello modellen för hello aktiviteter som skapats av programmet.</span><span class="sxs-lookup"><span data-stu-id="b71f4-145">This file contains hello model for hello tasks created by your application.</span></span>
3. <span data-ttu-id="b71f4-146">Hello början av hello **task.js** lägger du till följande kod tooreference krävs bibliotek hello:</span><span class="sxs-lookup"><span data-stu-id="b71f4-146">At hello beginning of hello **task.js** file, add hello following code tooreference required libraries:</span></span>

    ```nodejs
    var azure = require('azure-storage');
    var uuid = require('node-uuid');
    var entityGen = azure.TableUtilities.entityGenerator;
    ```

4. <span data-ttu-id="b71f4-147">Sedan lägger du till kod toodefine och exportera hello Task-objekt.</span><span class="sxs-lookup"><span data-stu-id="b71f4-147">Next, add code toodefine and export hello Task object.</span></span> <span data-ttu-id="b71f4-148">hello aktivitetsobjektet ansvarar för att ansluta toohello tabell.</span><span class="sxs-lookup"><span data-stu-id="b71f4-148">hello Task object is responsible for connecting toohello table.</span></span>

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

5. <span data-ttu-id="b71f4-149">Lägg till hello följande kod toodefine fler metoder på hello aktivitetsobjektet som tillåter samverkan med data som lagras i tabellen hello:</span><span class="sxs-lookup"><span data-stu-id="b71f4-149">Next, add hello following code toodefine additional methods on hello Task object, which allow interactions with data stored in hello table:</span></span>

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

6. <span data-ttu-id="b71f4-150">Spara och Stäng hello **task.js** fil.</span><span class="sxs-lookup"><span data-stu-id="b71f4-150">Save and close hello **task.js** file.</span></span>

### <a name="create-hello-controller"></a><span data-ttu-id="b71f4-151">Skapa hello-styrenhet</span><span class="sxs-lookup"><span data-stu-id="b71f4-151">Create hello controller</span></span>
1. <span data-ttu-id="b71f4-152">I hello **WebRole1/vägar** directory, skapa en ny fil med namnet **tasklist.js** och öppna den i en textredigerare.</span><span class="sxs-lookup"><span data-stu-id="b71f4-152">In hello **WebRole1/routes** directory, create a new file named **tasklist.js** and open it in a text editor.</span></span>
2. <span data-ttu-id="b71f4-153">Lägg till följande kod för hello**tasklist.js**.</span><span class="sxs-lookup"><span data-stu-id="b71f4-153">Add hello following code too**tasklist.js**.</span></span> <span data-ttu-id="b71f4-154">Den här koden läser in hello azure och async-moduler som används av **tasklist.js** och definierar hello **TaskList** funktion, som mottar en instans av hello **aktivitet** objekt vi definierade tidigare:</span><span class="sxs-lookup"><span data-stu-id="b71f4-154">This code loads hello azure and async modules, which are used by **tasklist.js** and defines hello **TaskList** function, which is passed an instance of hello **Task** object we defined earlier:</span></span>

    ```nodejs
    var azure = require('azure-storage');
    var async = require('async');

    module.exports = TaskList;

    function TaskList(task) {
      this.task = task;
    }
    ```

3. <span data-ttu-id="b71f4-155">Fortsätt att lägga till toohello **tasklist.js** filen genom att lägga till hello-metoder som används för**showTasks**, **addTask**, och **completeTasks**:</span><span class="sxs-lookup"><span data-stu-id="b71f4-155">Continue adding toohello **tasklist.js** file by adding hello methods used too**showTasks**, **addTask**, and **completeTasks**:</span></span>

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

4. <span data-ttu-id="b71f4-156">Spara hello **tasklist.js** fil.</span><span class="sxs-lookup"><span data-stu-id="b71f4-156">Save hello **tasklist.js** file.</span></span>

### <a name="modify-appjs"></a><span data-ttu-id="b71f4-157">Ändra app.js</span><span class="sxs-lookup"><span data-stu-id="b71f4-157">Modify app.js</span></span>
1. <span data-ttu-id="b71f4-158">I hello **WebRole1** katalog, öppna hello **app.js** i en textredigerare.</span><span class="sxs-lookup"><span data-stu-id="b71f4-158">In hello **WebRole1** directory, open hello **app.js** file in a text editor.</span></span>
2. <span data-ttu-id="b71f4-159">Lägg till hello följande tooload hello azure modulen hello början av hello-filen, och ange hello namn och partition tabellnyckel:</span><span class="sxs-lookup"><span data-stu-id="b71f4-159">At hello beginning of hello file, add hello following tooload hello azure module and set hello table name and partition key:</span></span>

    ```nodejs
    var azure = require('azure-storage');
    var tableName = 'tasks';
    var partitionKey = 'hometasks';
    ```

3. <span data-ttu-id="b71f4-160">Rulla ned toowhere som du ser i filen app.js hello hello följande rad:</span><span class="sxs-lookup"><span data-stu-id="b71f4-160">In hello app.js file, scroll down toowhere you see hello following line:</span></span>

    ```nodejs
    app.use('/', routes);
    app.use('/users', users);
    ```

    <span data-ttu-id="b71f4-161">Ersätt hello föregående rader med hello följande kod.</span><span class="sxs-lookup"><span data-stu-id="b71f4-161">Replace hello preceding lines with hello following code.</span></span> <span data-ttu-id="b71f4-162">Den här koden initierar en instans av <strong>aktivitet</strong> med en anslutning tooyour storage-konto.</span><span class="sxs-lookup"><span data-stu-id="b71f4-162">This code initializes an instance of <strong>Task</strong> with a connection tooyour storage account.</span></span> <span data-ttu-id="b71f4-163">Hej <strong>aktivitet</strong> skickas toohello <strong>TaskList</strong>, som använder den toocommunicate med hello tabelltjänsten:</span><span class="sxs-lookup"><span data-stu-id="b71f4-163">hello <strong>Task</strong> is passed toohello <strong>TaskList</strong>, which uses it toocommunicate with hello Table service:</span></span>

    ```nodejs
    var TaskList = require('./routes/tasklist');
    var Task = require('./models/task');
    var task = new Task(azure.createTableService(), tableName, partitionKey);
    var taskList = new TaskList(task);

    app.get('/', taskList.showTasks.bind(taskList));
    app.post('/addtask', taskList.addTask.bind(taskList));
    app.post('/completetask', taskList.completeTask.bind(taskList));
    ```

4. <span data-ttu-id="b71f4-164">Spara hello **app.js** fil.</span><span class="sxs-lookup"><span data-stu-id="b71f4-164">Save hello **app.js** file.</span></span>

### <a name="modify-hello-index-view"></a><span data-ttu-id="b71f4-165">Ändra hello indexvy</span><span class="sxs-lookup"><span data-stu-id="b71f4-165">Modify hello index view</span></span>
1. <span data-ttu-id="b71f4-166">Ändra kataloger toohello **vyer** katalog och öppna hello **index.jade** i en textredigerare.</span><span class="sxs-lookup"><span data-stu-id="b71f4-166">Change directories toohello **views** directory and open hello **index.jade** file in a text editor.</span></span>
2. <span data-ttu-id="b71f4-167">Ersätt hello innehållet i hello **index.jade** fil med hello följande kod.</span><span class="sxs-lookup"><span data-stu-id="b71f4-167">Replace hello contents of hello **index.jade** file with hello following code.</span></span> <span data-ttu-id="b71f4-168">Den här koden definierar hello vy för visning av befintliga aktiviteter och definierar ett formulär för att lägga till nya aktiviteter och markera befintliga som slutförts.</span><span class="sxs-lookup"><span data-stu-id="b71f4-168">This code defines hello view for displaying existing tasks, and defines a form for adding new tasks and marking existing ones as completed.</span></span>

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

3. <span data-ttu-id="b71f4-169">Spara och Stäng **index.jade** fil.</span><span class="sxs-lookup"><span data-stu-id="b71f4-169">Save and close **index.jade** file.</span></span>

### <a name="modify-hello-global-layout"></a><span data-ttu-id="b71f4-170">Ändra hello globala layout</span><span class="sxs-lookup"><span data-stu-id="b71f4-170">Modify hello global layout</span></span>
<span data-ttu-id="b71f4-171">Hej **layout.jade** filen i hello **vyer** directory används som en global mall för andra **.jade** filer.</span><span class="sxs-lookup"><span data-stu-id="b71f4-171">hello **layout.jade** file in hello **views** directory is used as a global template for other **.jade** files.</span></span> <span data-ttu-id="b71f4-172">I det här steget ändrar hello **layout.jade** filen toouse [Twitter Bootstrap](https://github.com/twbs/bootstrap), vilket är en verktygslåda som gör det enkelt toodesign en snygg webbplats.</span><span class="sxs-lookup"><span data-stu-id="b71f4-172">In this step, modify hello **layout.jade** file toouse [Twitter Bootstrap](https://github.com/twbs/bootstrap), which is a toolkit that makes it easy toodesign a nice looking website.</span></span>

1. <span data-ttu-id="b71f4-173">Ladda ned och extrahera hello filer för [Twitter Bootstrap](http://getbootstrap.com/).</span><span class="sxs-lookup"><span data-stu-id="b71f4-173">Download and extract hello files for [Twitter Bootstrap](http://getbootstrap.com/).</span></span> <span data-ttu-id="b71f4-174">Kopiera hello **bootstrap.min.css** filen från hello **bootstrap\\dist\\css** mappen toohello **offentliga\\matmallar** katalogen för tillämpningsprogrammet tasklist.</span><span class="sxs-lookup"><span data-stu-id="b71f4-174">Copy hello **bootstrap.min.css** file from hello **bootstrap\\dist\\css** folder toohello **public\\stylesheets** directory of your tasklist application.</span></span>
2. <span data-ttu-id="b71f4-175">Från hello **vyer** mapp, öppna hello **layout.jade** filen i en textredigerare och Ersätt hello innehållet med hello följande:</span><span class="sxs-lookup"><span data-stu-id="b71f4-175">From hello **views** folder, open hello **layout.jade** file in your text editor and replace hello contents with hello following:</span></span>

    <span data-ttu-id="b71f4-176">doctype html html head rubrik = Rubriklänk (rel = 'stylesheet', href='/stylesheets/bootstrap.min.css') länk (rel = 'stylesheet', href='/stylesheets/style.css') body.app nav.navbar.navbar standard div.navbar huvud a.navbar-brand(href='/') min  Uppgifter blockera innehåll</span><span class="sxs-lookup"><span data-stu-id="b71f4-176">doctype html  html    head      title= title      link(rel='stylesheet', href='/stylesheets/bootstrap.min.css')      link(rel='stylesheet', href='/stylesheets/style.css')    body.app      nav.navbar.navbar-default        div.navbar-header          a.navbar-brand(href='/') My Tasks      block content</span></span>

3. <span data-ttu-id="b71f4-177">Spara hello **layout.jade** fil.</span><span class="sxs-lookup"><span data-stu-id="b71f4-177">Save hello **layout.jade** file.</span></span>

### <a name="running-hello-application-in-hello-emulator"></a><span data-ttu-id="b71f4-178">Kör hello programmet hello emulatorn</span><span class="sxs-lookup"><span data-stu-id="b71f4-178">Running hello Application in hello Emulator</span></span>
<span data-ttu-id="b71f4-179">Använd hello följande kommando toostart hello program i hello-emulatorn.</span><span class="sxs-lookup"><span data-stu-id="b71f4-179">Use hello following command toostart hello application in hello emulator.</span></span>

```powershell
PS C:\node\tasklist\WebRole1> start-azureemulator -launch
```

<span data-ttu-id="b71f4-180">hello webbläsaren öppnas och visar hello följande sida:</span><span class="sxs-lookup"><span data-stu-id="b71f4-180">hello browser opens and displays hello following page:</span></span>

![En webbplats som växlingsbart systemminne heter uppgiftslistan med en tabell som innehåller uppgifter och fält tooadd en ny uppgift.](./media/table-storage-cloud-service-nodejs/node44.png)

<span data-ttu-id="b71f4-182">Använd hello formuläret tooadd objekt eller ta bort befintliga objekt genom att markera dem som slutförda.</span><span class="sxs-lookup"><span data-stu-id="b71f4-182">Use hello form tooadd items, or remove existing items by marking them as completed.</span></span>

## <a name="publishing-hello-application-tooazure"></a><span data-ttu-id="b71f4-183">Publishing hello programmet tooAzure</span><span class="sxs-lookup"><span data-stu-id="b71f4-183">Publishing hello Application tooAzure</span></span>
<span data-ttu-id="b71f4-184">Anropa hello följande cmdlet tooredeploy värdtjänsten-tooAzure i hello Windows PowerShell-fönster.</span><span class="sxs-lookup"><span data-stu-id="b71f4-184">In hello Windows PowerShell window, call hello following cmdlet tooredeploy your hosted service tooAzure.</span></span>

```powershell
PS C:\node\tasklist\WebRole1> Publish-AzureServiceProject -name myuniquename -location datacentername -launch
```

<span data-ttu-id="b71f4-185">Ersätt **myuniquename** med ett unikt namn för det här programmet.</span><span class="sxs-lookup"><span data-stu-id="b71f4-185">Replace **myuniquename** with a unique name for this application.</span></span> <span data-ttu-id="b71f4-186">Ersätt **datacentername** med hello namnet på ett Azure-datacenter som **västra USA**.</span><span class="sxs-lookup"><span data-stu-id="b71f4-186">Replace **datacentername** with hello name of an Azure data center, such as **West US**.</span></span>

<span data-ttu-id="b71f4-187">När hello distributionen är klar, bör du se ett svar liknande toohello följande:</span><span class="sxs-lookup"><span data-stu-id="b71f4-187">After hello deployment is complete, you should see a response similar toohello following:</span></span>

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

<span data-ttu-id="b71f4-188">Genom att ange hello **-starta** alternativet i hello tidigare cmdleten hello webbläsaren öppnas och visar programmet körs i Azure när publiceringen är klar.</span><span class="sxs-lookup"><span data-stu-id="b71f4-188">By specifying hello **-launch** option in hello previous cmdlet, hello browser opens and displays your application running in Azure when publishing is completed.</span></span>

![Ett webbläsarfönster som visar hello uppgiftslistan sidan.](./media/table-storage-cloud-service-nodejs/getting-started-1.png)

## <a name="stopping-and-deleting-your-application"></a><span data-ttu-id="b71f4-191">Stoppa och ta bort programmet</span><span class="sxs-lookup"><span data-stu-id="b71f4-191">Stopping and Deleting Your Application</span></span>
<span data-ttu-id="b71f4-192">Du kanske vill toodisable den så att du kan undvika kostnader eller skapa och distribuera andra program inom hello kostnadsfri utvärderingsversion tidsperiod när du distribuerar ditt program.</span><span class="sxs-lookup"><span data-stu-id="b71f4-192">After deploying your application, you may want toodisable it so you can avoid costs or build and deploy other applications within hello free trial time period.</span></span>

<span data-ttu-id="b71f4-193">Azure fakturerar webbrollsinstanser per timme förbrukad servertid.</span><span class="sxs-lookup"><span data-stu-id="b71f4-193">Azure bills web role instances per hour of server time consumed.</span></span>
<span data-ttu-id="b71f4-194">Servertid förbrukas när programmet har distribuerats, även om instanserna inte körs och är i hello stoppats tillstånd.</span><span class="sxs-lookup"><span data-stu-id="b71f4-194">Server time is consumed once your application is deployed, even if the instances are not running and are in hello stopped state.</span></span>

<span data-ttu-id="b71f4-195">hello följande steg visar hur toostop och ta bort programmet.</span><span class="sxs-lookup"><span data-stu-id="b71f4-195">hello following steps show you how toostop and delete your application.</span></span>

1. <span data-ttu-id="b71f4-196">Stoppa hello tjänstdistributionen som skapades i föregående avsnitt i hello med hello följande cmdlet i Windows PowerShell-fönstret hello:</span><span class="sxs-lookup"><span data-stu-id="b71f4-196">In hello Windows PowerShell window, stop hello service deployment created in hello previous section with hello following cmdlet:</span></span>

    ```powershell
    PS C:\node\tasklist\WebRole1> Stop-AzureService
    ```

   <span data-ttu-id="b71f4-197">Hello-tjänsten stoppas kan det ta flera minuter.</span><span class="sxs-lookup"><span data-stu-id="b71f4-197">Stopping hello service may take several minutes.</span></span> <span data-ttu-id="b71f4-198">När hello har stoppats, kan du få ett meddelande som anger att den har stoppats.</span><span class="sxs-lookup"><span data-stu-id="b71f4-198">When hello service is stopped, you receive a message indicating that it has stopped.</span></span>

2. <span data-ttu-id="b71f4-199">toodelete hello service, anrop hello följande cmdlet:</span><span class="sxs-lookup"><span data-stu-id="b71f4-199">toodelete hello service, call hello following cmdlet:</span></span>

    ```powershell
    PS C:\node\tasklist\WebRole1> Remove-AzureService contosotasklist
    ```

   <span data-ttu-id="b71f4-200">När du uppmanas, anger **Y** toodelete hello-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="b71f4-200">When prompted, enter **Y** toodelete hello service.</span></span>

   <span data-ttu-id="b71f4-201">Om du tar bort hello service kan ta några minuter.</span><span class="sxs-lookup"><span data-stu-id="b71f4-201">Deleting hello service may take several minutes.</span></span> <span data-ttu-id="b71f4-202">När hello-tjänsten har tagits bort, visas ett meddelande som anger att hello-tjänsten har tagits bort.</span><span class="sxs-lookup"><span data-stu-id="b71f4-202">After hello service is deleted, you will receive a message indicating that hello service was deleted.</span></span>

[Node.js-Webbapp med hjälp av]: http://azure.microsoft.com/develop/nodejs/tutorials/web-app-with-express/
[lagring och åtkomst till Data i Azure]: http://msdn.microsoft.com/library/azure/gg433040.aspx
[Node.js-Webbapp]: http://azure.microsoft.com/develop/nodejs/tutorials/getting-started/


