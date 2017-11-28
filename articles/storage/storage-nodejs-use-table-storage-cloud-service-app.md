---
title: aaaWeb app med table storage (Node.js) | Microsoft Docs
description: "En självstudiekurs som bygger på hello Web App med en snabb genomgång genom att lägga till Azure Storage-tjänster och hello Azure-modulen."
services: cloud-services, storage
documentationcenter: nodejs
author: mmacy
manager: timlt
editor: tysonn
ms.assetid: e90959a2-4cb2-4b19-9bfb-aede15b18b1c
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: article
ms.date: 12/08/2016
ms.author: marsma
ms.openlocfilehash: 4eba16f09f8b69cbc135d097e6ca71e08b33733c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="nodejs-web-application-using-storage"></a><span data-ttu-id="83cd3-103">Node.js-Webbapp som använder lagring</span><span class="sxs-lookup"><span data-stu-id="83cd3-103">Node.js Web Application using Storage</span></span>
## <a name="overview"></a><span data-ttu-id="83cd3-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="83cd3-104">Overview</span></span>
<span data-ttu-id="83cd3-105">I den här självstudiekursen kommer du utökar hello-program som skapats i den [Node.js-Webbapp med hjälp av] självstudiekurs med hello Microsoft Azure-klientbibliotek för Node.js toowork med data management-tjänster.</span><span class="sxs-lookup"><span data-stu-id="83cd3-105">In this tutorial, you will extend hello application created in the [Node.js Web Application using Express] tutorial by using hello Microsoft Azure Client Libraries for Node.js toowork with data management services.</span></span> <span data-ttu-id="83cd3-106">Du ska utöka ditt program toocreate ett webbaserat-uppgiftslista program som du distribuerar tooAzure.</span><span class="sxs-lookup"><span data-stu-id="83cd3-106">You will extend your application toocreate a web-based task-list application that you can deploy tooAzure.</span></span> <span data-ttu-id="83cd3-107">hello uppgiftslista kan användaren hämta uppgifter, lägga till nya aktiviteter och markera aktiviteter som slutförda.</span><span class="sxs-lookup"><span data-stu-id="83cd3-107">hello task list allows a user to retrieve tasks, add new tasks, and mark tasks as completed.</span></span>

<span data-ttu-id="83cd3-108">hello uppgiftsobjekt lagras i Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="83cd3-108">hello task items are stored in Azure Storage.</span></span> <span data-ttu-id="83cd3-109">Azure Storage tillhandahåller lagring av Ostrukturerade data som är feltolerant och hög tillgänglighet.</span><span class="sxs-lookup"><span data-stu-id="83cd3-109">Azure Storage provides unstructured data storage that is fault-tolerant and highly available.</span></span> <span data-ttu-id="83cd3-110">Azure Storage innehåller flera datastrukturer där du kan lagra och komma åt data, och du kan utnyttja hello lagringstjänster från hello API: er som ingår i hello Azure SDK för Node.js eller via REST API: er.</span><span class="sxs-lookup"><span data-stu-id="83cd3-110">Azure Storage includes several data structures where you can store and access data, and you can leverage hello storage services from hello APIs included in hello Azure SDK for Node.js or via REST APIs.</span></span> <span data-ttu-id="83cd3-111">Mer information finns i [lagring och åtkomst till Data i Azure].</span><span class="sxs-lookup"><span data-stu-id="83cd3-111">For more information, see [Storing and Accessing Data in Azure].</span></span>

<span data-ttu-id="83cd3-112">Den här kursen förutsätter att du har slutfört hello [Node.js-Webbapp] och [Node.js med snabb][Node.js-Webbapp med hjälp av] självstudier.</span><span class="sxs-lookup"><span data-stu-id="83cd3-112">This tutorial assumes that you have completed hello [Node.js Web Application] and [Node.js with Express][Node.js Web Application using Express] tutorials.</span></span>

<span data-ttu-id="83cd3-113">Du kommer att lära dig:</span><span class="sxs-lookup"><span data-stu-id="83cd3-113">You will learn:</span></span>

* <span data-ttu-id="83cd3-114">Hur toowork med hello Jade mall-motorn</span><span class="sxs-lookup"><span data-stu-id="83cd3-114">How toowork with hello Jade template engine</span></span>
* <span data-ttu-id="83cd3-115">Hur toowork med Azure Data Management-tjänster</span><span class="sxs-lookup"><span data-stu-id="83cd3-115">How toowork with Azure Data Management services</span></span>

<span data-ttu-id="83cd3-116">En skärmbild av programmet hello slutförts understiger:</span><span class="sxs-lookup"><span data-stu-id="83cd3-116">A screenshot of hello completed application is below:</span></span>

![hello slutförts webbsida i internet explorer](./media/storage-nodejs-use-table-storage-cloud-service-app/getting-started-1.png)

## <a name="setting-storage-credentials-in-webconfig"></a><span data-ttu-id="83cd3-118">Ange autentiseringsuppgifter för lagring i Web.Config</span><span class="sxs-lookup"><span data-stu-id="83cd3-118">Setting Storage Credentials in Web.Config</span></span>
<span data-ttu-id="83cd3-119">tooaccess Azure Storage, behöver du toopass i lagring autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="83cd3-119">tooaccess Azure Storage, you need toopass in storage credentials.</span></span> <span data-ttu-id="83cd3-120">toodo kan du använda inställningar för web.config-program.</span><span class="sxs-lookup"><span data-stu-id="83cd3-120">toodo this, you utilize web.config application settings.</span></span>
<span data-ttu-id="83cd3-121">Dessa inställningar ska skickas som miljö variabler tooNode som läses sedan av hello Azure SDK.</span><span class="sxs-lookup"><span data-stu-id="83cd3-121">Those settings will be passed as environment variables tooNode, which are then read by hello Azure SDK.</span></span>

> [!NOTE]
> <span data-ttu-id="83cd3-122">Lagring autentiseringsuppgifter används bara när programmet hello är distribuerade tooAzure.</span><span class="sxs-lookup"><span data-stu-id="83cd3-122">Storage credentials are only used when hello application is deployed tooAzure.</span></span> <span data-ttu-id="83cd3-123">När du kör i hello-emulatorn använder hello programmet hello storage-emulatorn.</span><span class="sxs-lookup"><span data-stu-id="83cd3-123">When running in hello emulator, hello application will use hello storage emulator.</span></span>
>
>

<span data-ttu-id="83cd3-124">Utför följande steg tooretrieve hello lagringskontouppgifter hello och Lägg till dem toohello web.config-inställningar:</span><span class="sxs-lookup"><span data-stu-id="83cd3-124">Perform hello following steps tooretrieve hello storage account credentials and add them toohello web.config settings:</span></span>

1. <span data-ttu-id="83cd3-125">Om den inte redan är öppen, starta hello Azure PowerShell från hello **starta** menyn genom att expandera **alla program, Azure**, högerklicka på **Azure PowerShell**, och välj sedan  **Kör som administratör**.</span><span class="sxs-lookup"><span data-stu-id="83cd3-125">If it is not already open, start hello Azure PowerShell from hello **Start** menu by expanding **All Programs, Azure**, right-click **Azure PowerShell**, and then select **Run As Administrator**.</span></span>
2. <span data-ttu-id="83cd3-126">Ändra kataloger toohello mapp som innehåller programmet.</span><span class="sxs-lookup"><span data-stu-id="83cd3-126">Change directories toohello folder containing your application.</span></span> <span data-ttu-id="83cd3-127">Till exempel C:\\nod\\tasklist\\WebRole1.</span><span class="sxs-lookup"><span data-stu-id="83cd3-127">For example, C:\\node\\tasklist\\WebRole1.</span></span>
3. <span data-ttu-id="83cd3-128">Ange följande cmdlet tooretrieve hello lagring kontoinformation hello från hello Azure Powershell-fönstret:</span><span class="sxs-lookup"><span data-stu-id="83cd3-128">From hello Azure Powershell window enter hello following cmdlet tooretrieve hello storage account information:</span></span>

    ```powershell
    PS C:\node\tasklist\WebRole1> Get-AzureStorageAccounts
    ```

   <span data-ttu-id="83cd3-129">Detta hämtar hello lista över storage-konton och konto nycklar som hör till din värdbaserade tjänst.</span><span class="sxs-lookup"><span data-stu-id="83cd3-129">This retrieves hello list of storage accounts and account keys associated with your hosted service.</span></span>

   > [!NOTE]
   > <span data-ttu-id="83cd3-130">Eftersom hello Azure SDK om du skapar ett lagringskonto när du distribuerar en tjänst, ska ett lagringskonto redan finnas från att distribuera ditt program i hello tidigare guider.</span><span class="sxs-lookup"><span data-stu-id="83cd3-130">Since hello Azure SDK creates a storage account when you deploy a service, a storage account should already exist from deploying your application in hello previous guides.</span></span>
   >
   >
4. <span data-ttu-id="83cd3-131">Öppna hello **ServiceDefinition.csdef** -fil som innehåller hello miljöinställningar som används när programmet hello är distribuerade tooAzure:</span><span class="sxs-lookup"><span data-stu-id="83cd3-131">Open hello **ServiceDefinition.csdef** file containing hello environment settings that are used when hello application is deployed tooAzure:</span></span>

    ```powershell
    PS C:\node\tasklist> notepad ServiceDefinition.csdef
    ```

5. <span data-ttu-id="83cd3-132">Infoga hello följande blockera **miljö** element, ersätter {LAGRINGSKONTO} och {LAGRINGSÅTKOMSTNYCKEL} med hello kontonamn och hello primärnyckeln för hello storage-konto som du vill använda toouse för distributionen:</span><span class="sxs-lookup"><span data-stu-id="83cd3-132">Insert hello following block under **Environment** element, substituting {STORAGE ACCOUNT} and {STORAGE ACCESS KEY} with hello account name and hello primary key for hello storage account you want toouse for deployment:</span></span>

  <Variable name="AZURE_STORAGE_ACCOUNT" value="{STORAGE ACCOUNT}" />
  <Variable name="AZURE_STORAGE_ACCESS_KEY" value="{STORAGE ACCESS KEY}" />

   ![Hej web.cloud.config filinnehåll](./media/storage-nodejs-use-table-storage-cloud-service-app/node37.png)

6. <span data-ttu-id="83cd3-134">Spara hello-filen och stäng Anteckningar.</span><span class="sxs-lookup"><span data-stu-id="83cd3-134">Save hello file and close notepad.</span></span>

### <a name="install-additional-modules"></a><span data-ttu-id="83cd3-135">Installera ytterligare moduler</span><span class="sxs-lookup"><span data-stu-id="83cd3-135">Install additional modules</span></span>
1. <span data-ttu-id="83cd3-136">Använd hello efter kommandot tooinstall hello [azure], [nod-uuid], [nconf] och [asynkrona] moduler lokalt samt toosave en post för dem toohello **package.json** fil:</span><span class="sxs-lookup"><span data-stu-id="83cd3-136">Use hello following command tooinstall hello [azure], [node-uuid], [nconf] and [async] modules locally as well as toosave an entry for them toohello **package.json** file:</span></span>

  ```powershell
  PS C:\node\tasklist\WebRole1> npm install azure-storage node-uuid async nconf --save
  ```

  <span data-ttu-id="83cd3-137">hello utdata från kommandot ska se ut ungefär toohello följande:</span><span class="sxs-lookup"><span data-stu-id="83cd3-137">hello output of this command should appear similar toohello following:</span></span>

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

## <a name="using-hello-table-service-in-a-node-application"></a><span data-ttu-id="83cd3-138">Använda hello tabelltjänsten i en node-App</span><span class="sxs-lookup"><span data-stu-id="83cd3-138">Using hello Table service in a node application</span></span>
<span data-ttu-id="83cd3-139">I det här avsnittet ska du utöka grundläggande hello program som skapats hello **express** kommandot genom att lägga till en **task.js** -fil som innehåller hello modell för dina uppgifter.</span><span class="sxs-lookup"><span data-stu-id="83cd3-139">In this section you will extend hello basic application created by hello **express** command by adding a **task.js** file which contains hello model for your tasks.</span></span> <span data-ttu-id="83cd3-140">Du kan också ändra hello befintliga **app.js** och skapa en ny **tasklist.js** fil som använder hello modellen.</span><span class="sxs-lookup"><span data-stu-id="83cd3-140">You will also modify hello existing **app.js** and create a new **tasklist.js** file that uses hello model.</span></span>

### <a name="create-hello-model"></a><span data-ttu-id="83cd3-141">Skapa hello modell</span><span class="sxs-lookup"><span data-stu-id="83cd3-141">Create hello model</span></span>
1. <span data-ttu-id="83cd3-142">I hello **WebRole1** directory, skapa en ny katalog med namnet **modeller**.</span><span class="sxs-lookup"><span data-stu-id="83cd3-142">In hello **WebRole1** directory, create a new directory named **models**.</span></span>
2. <span data-ttu-id="83cd3-143">I hello **modeller** directory, skapa en ny fil med namnet **task.js**.</span><span class="sxs-lookup"><span data-stu-id="83cd3-143">In hello **models** directory, create a new file named **task.js**.</span></span> <span data-ttu-id="83cd3-144">Den här filen innehåller hello modellen för hello aktiviteter som skapats av programmet.</span><span class="sxs-lookup"><span data-stu-id="83cd3-144">This file will contain hello model for hello tasks created by your application.</span></span>
3. <span data-ttu-id="83cd3-145">Hello början av hello **task.js** lägger du till följande kod tooreference krävs bibliotek hello:</span><span class="sxs-lookup"><span data-stu-id="83cd3-145">At hello beginning of hello **task.js** file, add hello following code tooreference required libraries:</span></span>

    ```nodejs
    var azure = require('azure-storage');
    var uuid = require('node-uuid');
    var entityGen = azure.TableUtilities.entityGenerator;
    ```

4. <span data-ttu-id="83cd3-146">Därefter måste du lägga till kod toodefine och exportera aktivitetsobjektet hello.</span><span class="sxs-lookup"><span data-stu-id="83cd3-146">Next, you will add code toodefine and export hello Task object.</span></span> <span data-ttu-id="83cd3-147">Det här objektet är ansvarig för att ansluta toohello tabell.</span><span class="sxs-lookup"><span data-stu-id="83cd3-147">This object is responsible for connecting toohello table.</span></span>

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

5. <span data-ttu-id="83cd3-148">Lägg till hello följande kod toodefine fler metoder på hello aktivitetsobjektet som tillåter samverkan med data som lagras i tabellen hello:</span><span class="sxs-lookup"><span data-stu-id="83cd3-148">Next, add hello following code toodefine additional methods on hello Task object, which allow interactions with data stored in hello table:</span></span>

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

6. <span data-ttu-id="83cd3-149">Spara och Stäng hello **task.js** fil.</span><span class="sxs-lookup"><span data-stu-id="83cd3-149">Save and close hello **task.js** file.</span></span>

### <a name="create-hello-controller"></a><span data-ttu-id="83cd3-150">Skapa hello-styrenhet</span><span class="sxs-lookup"><span data-stu-id="83cd3-150">Create hello controller</span></span>
1. <span data-ttu-id="83cd3-151">I hello **WebRole1/vägar** directory, skapa en ny fil med namnet **tasklist.js** och öppna den i en textredigerare.</span><span class="sxs-lookup"><span data-stu-id="83cd3-151">In hello **WebRole1/routes** directory, create a new file named **tasklist.js** and open it in a text editor.</span></span>
2. <span data-ttu-id="83cd3-152">Lägg till följande kod för hello**tasklist.js**.</span><span class="sxs-lookup"><span data-stu-id="83cd3-152">Add hello following code too**tasklist.js**.</span></span> <span data-ttu-id="83cd3-153">Den läser in hello azure och async-moduler som används av **tasklist.js**.</span><span class="sxs-lookup"><span data-stu-id="83cd3-153">This loads hello azure and async modules, which are used by **tasklist.js**.</span></span> <span data-ttu-id="83cd3-154">Detta definierar också hello **TaskList** funktion, som mottar en instans av hello **aktivitet** objektet som vi definierade tidigare:</span><span class="sxs-lookup"><span data-stu-id="83cd3-154">This also defines hello **TaskList** function, which is passed an instance of hello **Task** object we defined earlier:</span></span>

    ```nodejs
    var azure = require('azure-storage');
    var async = require('async');

    module.exports = TaskList;

    function TaskList(task) {
      this.task = task;
    }
    ```

3. <span data-ttu-id="83cd3-155">Fortsätt att lägga till toohello **tasklist.js** filen genom att lägga till hello-metoder som används för**showTasks**, **addTask**, och **completeTasks**:</span><span class="sxs-lookup"><span data-stu-id="83cd3-155">Continue adding toohello **tasklist.js** file by adding hello methods used too**showTasks**, **addTask**, and **completeTasks**:</span></span>

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

4. <span data-ttu-id="83cd3-156">Spara hello **tasklist.js** fil.</span><span class="sxs-lookup"><span data-stu-id="83cd3-156">Save hello **tasklist.js** file.</span></span>

### <a name="modify-appjs"></a><span data-ttu-id="83cd3-157">Ändra app.js</span><span class="sxs-lookup"><span data-stu-id="83cd3-157">Modify app.js</span></span>
1. <span data-ttu-id="83cd3-158">I hello **WebRole1** katalog, öppna hello **app.js** i en textredigerare.</span><span class="sxs-lookup"><span data-stu-id="83cd3-158">In hello **WebRole1** directory, open hello **app.js** file in a text editor.</span></span>
2. <span data-ttu-id="83cd3-159">Lägg till hello följande tooload hello azure modulen hello början av hello-filen, och ange hello namn och partition tabellnyckel:</span><span class="sxs-lookup"><span data-stu-id="83cd3-159">At hello beginning of hello file, add hello following tooload hello azure module and set hello table name and partition key:</span></span>

    ```nodejs
    var azure = require('azure-storage');
    var tableName = 'tasks';
    var partitionKey = 'hometasks';
    ```

3. <span data-ttu-id="83cd3-160">Rulla ned toowhere som du ser i filen app.js hello hello följande rad:</span><span class="sxs-lookup"><span data-stu-id="83cd3-160">In hello app.js file, scroll down toowhere you see hello following line:</span></span>

    ```nodejs
    app.use('/', routes);
    app.use('/users', users);
    ```

    <span data-ttu-id="83cd3-161">Ersätt hello ovan rader med hello koden nedan.</span><span class="sxs-lookup"><span data-stu-id="83cd3-161">Replace hello above lines with hello code shown below.</span></span> <span data-ttu-id="83cd3-162">Detta ska initiera en instans av <strong>aktivitet</strong> med en anslutning tooyour storage-konto.</span><span class="sxs-lookup"><span data-stu-id="83cd3-162">This will initialize an instance of <strong>Task</strong> with a connection tooyour storage account.</span></span> <span data-ttu-id="83cd3-163">Detta skickas toohello <strong>TaskList</strong>, som kommer att använda den toocommunicate med hello tabelltjänsten:</span><span class="sxs-lookup"><span data-stu-id="83cd3-163">This is passed toohello <strong>TaskList</strong>, which will use it toocommunicate with hello Table service:</span></span>

    ```nodejs
    var TaskList = require('./routes/tasklist');
    var Task = require('./models/task');
    var task = new Task(azure.createTableService(), tableName, partitionKey);
    var taskList = new TaskList(task);

    app.get('/', taskList.showTasks.bind(taskList));
    app.post('/addtask', taskList.addTask.bind(taskList));
    app.post('/completetask', taskList.completeTask.bind(taskList));
    ```

4. <span data-ttu-id="83cd3-164">Spara hello **app.js** fil.</span><span class="sxs-lookup"><span data-stu-id="83cd3-164">Save hello **app.js** file.</span></span>

### <a name="modify-hello-index-view"></a><span data-ttu-id="83cd3-165">Ändra hello indexvy</span><span class="sxs-lookup"><span data-stu-id="83cd3-165">Modify hello index view</span></span>
1. <span data-ttu-id="83cd3-166">Ändra kataloger toohello **vyer** katalog och öppna hello **index.jade** i en textredigerare.</span><span class="sxs-lookup"><span data-stu-id="83cd3-166">Change directories toohello **views** directory and open hello **index.jade** file in a text editor.</span></span>
2. <span data-ttu-id="83cd3-167">Ersätt hello innehållet i hello **index.jade** fil med hello koden nedan.</span><span class="sxs-lookup"><span data-stu-id="83cd3-167">Replace hello contents of hello **index.jade** file with hello code below.</span></span> <span data-ttu-id="83cd3-168">Detta definierar hello vy för visning av befintliga aktiviteter, samt ett formulär för att lägga till nya aktiviteter och markera befintliga som slutförts.</span><span class="sxs-lookup"><span data-stu-id="83cd3-168">This defines hello view for displaying existing tasks, as well as a form for adding new tasks and marking existing ones as completed.</span></span>

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

3. <span data-ttu-id="83cd3-169">Spara och Stäng **index.jade** fil.</span><span class="sxs-lookup"><span data-stu-id="83cd3-169">Save and close **index.jade** file.</span></span>

### <a name="modify-hello-global-layout"></a><span data-ttu-id="83cd3-170">Ändra hello globala layout</span><span class="sxs-lookup"><span data-stu-id="83cd3-170">Modify hello global layout</span></span>
<span data-ttu-id="83cd3-171">Hej **layout.jade** filen i hello **vyer** directory används som en global mall för andra **.jade** filer.</span><span class="sxs-lookup"><span data-stu-id="83cd3-171">hello **layout.jade** file in hello **views** directory is used as a global template for other **.jade** files.</span></span> <span data-ttu-id="83cd3-172">I det här steget ändrar du den toouse [Twitter Bootstrap](https://github.com/twbs/bootstrap), vilket är en verktygslåda som gör det enkelt toodesign en snygg webbplats.</span><span class="sxs-lookup"><span data-stu-id="83cd3-172">In this step you will modify it toouse [Twitter Bootstrap](https://github.com/twbs/bootstrap), which is a toolkit that makes it easy toodesign a nice looking website.</span></span>

1. <span data-ttu-id="83cd3-173">Ladda ned och extrahera hello filer för [Twitter Bootstrap](http://getbootstrap.com/).</span><span class="sxs-lookup"><span data-stu-id="83cd3-173">Download and extract hello files for [Twitter Bootstrap](http://getbootstrap.com/).</span></span> <span data-ttu-id="83cd3-174">Kopiera hello **bootstrap.min.css** filen från hello **bootstrap\\dist\\css** mappen toohello **offentliga\\matmallar** katalogen för tillämpningsprogrammet tasklist.</span><span class="sxs-lookup"><span data-stu-id="83cd3-174">Copy hello **bootstrap.min.css** file from hello **bootstrap\\dist\\css** folder toohello **public\\stylesheets** directory of your tasklist application.</span></span>
2. <span data-ttu-id="83cd3-175">Från hello **vyer** mapp, öppna hello **layout.jade** i din redigeraren och Ersätt hello textinnehållet med hello följande:</span><span class="sxs-lookup"><span data-stu-id="83cd3-175">From hello **views** folder, open hello **layout.jade** in your text editor and replace hello contents with hello following:</span></span>

    <span data-ttu-id="83cd3-176">doctype html html head rubrik = Rubriklänk (rel = 'stylesheet', href='/stylesheets/bootstrap.min.css') länk (rel = 'stylesheet', href='/stylesheets/style.css') body.app nav.navbar.navbar standard div.navbar huvud a.navbar-brand(href='/') min  Uppgifter blockera innehåll</span><span class="sxs-lookup"><span data-stu-id="83cd3-176">doctype html  html    head      title= title      link(rel='stylesheet', href='/stylesheets/bootstrap.min.css')      link(rel='stylesheet', href='/stylesheets/style.css')    body.app      nav.navbar.navbar-default        div.navbar-header          a.navbar-brand(href='/') My Tasks      block content</span></span>

3. <span data-ttu-id="83cd3-177">Spara hello **layout.jade** fil.</span><span class="sxs-lookup"><span data-stu-id="83cd3-177">Save hello **layout.jade** file.</span></span>

### <a name="running-hello-application-in-hello-emulator"></a><span data-ttu-id="83cd3-178">Kör hello programmet hello emulatorn</span><span class="sxs-lookup"><span data-stu-id="83cd3-178">Running hello Application in hello Emulator</span></span>
<span data-ttu-id="83cd3-179">Använd hello följande kommando toostart hello program i hello-emulatorn.</span><span class="sxs-lookup"><span data-stu-id="83cd3-179">Use hello following command toostart hello application in hello emulator.</span></span>

```powershell
PS C:\node\tasklist\WebRole1> start-azureemulator -launch
```

<span data-ttu-id="83cd3-180">hello webbläsaren öppnas och visar hello följande sida:</span><span class="sxs-lookup"><span data-stu-id="83cd3-180">hello browser will open and displays hello following page:</span></span>

![En webbplats som växlingsbart systemminne heter uppgiftslistan med en tabell som innehåller uppgifter och fält tooadd en ny uppgift.](./media/storage-nodejs-use-table-storage-cloud-service-app/node44.png)

<span data-ttu-id="83cd3-182">Använd hello formuläret tooadd objekt eller ta bort befintliga objekt genom att markera dem som slutförda.</span><span class="sxs-lookup"><span data-stu-id="83cd3-182">Use hello form tooadd items, or remove existing items by marking them as completed.</span></span>

## <a name="publishing-hello-application-tooazure"></a><span data-ttu-id="83cd3-183">Publishing hello programmet tooAzure</span><span class="sxs-lookup"><span data-stu-id="83cd3-183">Publishing hello Application tooAzure</span></span>
<span data-ttu-id="83cd3-184">Anropa hello följande cmdlet tooredeploy värdtjänsten-tooAzure i hello Windows PowerShell-fönster.</span><span class="sxs-lookup"><span data-stu-id="83cd3-184">In hello Windows PowerShell window, call hello following cmdlet tooredeploy your hosted service tooAzure.</span></span>

```powershell
PS C:\node\tasklist\WebRole1> Publish-AzureServiceProject -name myuniquename -location datacentername -launch
```

<span data-ttu-id="83cd3-185">Ersätt **myuniquename** med ett unikt namn för det här programmet.</span><span class="sxs-lookup"><span data-stu-id="83cd3-185">Replace **myuniquename** with a unique name for this application.</span></span> <span data-ttu-id="83cd3-186">Ersätt **datacentername** med hello namnet på ett Azure-datacenter som **västra USA**.</span><span class="sxs-lookup"><span data-stu-id="83cd3-186">Replace **datacentername** with hello name of an Azure data center, such as **West US**.</span></span>

<span data-ttu-id="83cd3-187">När hello distributionen är klar, bör du se ett svar liknande toohello följande:</span><span class="sxs-lookup"><span data-stu-id="83cd3-187">After hello deployment is complete, you should see a response similar toohello following:</span></span>

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

<span data-ttu-id="83cd3-188">Precis som tidigare, eftersom du har angett hello **-starta** alternativet hello webbläsaren öppnas och visar programmet körs i Azure när publiceringen är klar.</span><span class="sxs-lookup"><span data-stu-id="83cd3-188">As before, because you specified hello **-launch** option, hello browser opens and displays your application running in Azure when publishing is completed.</span></span>

![Ett webbläsarfönster som visar hello uppgiftslistan sidan.](./media/storage-nodejs-use-table-storage-cloud-service-app/getting-started-1.png)

## <a name="stopping-and-deleting-your-application"></a><span data-ttu-id="83cd3-191">Stoppa och ta bort programmet</span><span class="sxs-lookup"><span data-stu-id="83cd3-191">Stopping and Deleting Your Application</span></span>
<span data-ttu-id="83cd3-192">Du kanske vill toodisable den så att du kan undvika kostnader eller skapa och distribuera andra program inom hello kostnadsfri utvärderingsversion tidsperiod när du distribuerar ditt program.</span><span class="sxs-lookup"><span data-stu-id="83cd3-192">After deploying your application, you may want toodisable it so you can avoid costs or build and deploy other applications within hello free trial time period.</span></span>

<span data-ttu-id="83cd3-193">Azure fakturerar webbrollsinstanser per timme förbrukad servertid.</span><span class="sxs-lookup"><span data-stu-id="83cd3-193">Azure bills web role instances per hour of server time consumed.</span></span>
<span data-ttu-id="83cd3-194">Servertid förbrukas när programmet har distribuerats, även om instanserna inte körs och är i hello stoppats tillstånd.</span><span class="sxs-lookup"><span data-stu-id="83cd3-194">Server time is consumed once your application is deployed, even if the instances are not running and are in hello stopped state.</span></span>

<span data-ttu-id="83cd3-195">hello följande steg visar hur toostop och ta bort programmet.</span><span class="sxs-lookup"><span data-stu-id="83cd3-195">hello following steps show you how toostop and delete your application.</span></span>

1. <span data-ttu-id="83cd3-196">Stoppa hello tjänstdistributionen som skapades i föregående avsnitt i hello med hello följande cmdlet i Windows PowerShell-fönstret hello:</span><span class="sxs-lookup"><span data-stu-id="83cd3-196">In hello Windows PowerShell window, stop hello service deployment created in hello previous section with hello following cmdlet:</span></span>

    ```powershell
    PS C:\node\tasklist\WebRole1> Stop-AzureService
    ```

   <span data-ttu-id="83cd3-197">Hello-tjänsten stoppas kan det ta flera minuter.</span><span class="sxs-lookup"><span data-stu-id="83cd3-197">Stopping hello service may take several minutes.</span></span> <span data-ttu-id="83cd3-198">När hello har stoppats, kan du få ett meddelande som anger att den har stoppats.</span><span class="sxs-lookup"><span data-stu-id="83cd3-198">When hello service is stopped, you receive a message indicating that it has stopped.</span></span>

2. <span data-ttu-id="83cd3-199">toodelete hello service, anrop hello följande cmdlet:</span><span class="sxs-lookup"><span data-stu-id="83cd3-199">toodelete hello service, call hello following cmdlet:</span></span>

    ```powershell
    PS C:\node\tasklist\WebRole1> Remove-AzureService contosotasklist
    ```

   <span data-ttu-id="83cd3-200">När du uppmanas, anger **Y** toodelete hello-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="83cd3-200">When prompted, enter **Y** toodelete hello service.</span></span>

   <span data-ttu-id="83cd3-201">Om du tar bort hello service kan ta några minuter.</span><span class="sxs-lookup"><span data-stu-id="83cd3-201">Deleting hello service may take several minutes.</span></span> <span data-ttu-id="83cd3-202">När hello-tjänsten har tagits bort visas ett meddelande som anger att hello-tjänsten har tagits bort.</span><span class="sxs-lookup"><span data-stu-id="83cd3-202">After hello service has been deleted you receive a message indicating that hello service was deleted.</span></span>

[Node.js-Webbapp med hjälp av]: http://azure.microsoft.com/develop/nodejs/tutorials/web-app-with-express/
[lagring och åtkomst till Data i Azure]: http://msdn.microsoft.com/library/azure/gg433040.aspx
[Node.js-Webbapp]: http://azure.microsoft.com/develop/nodejs/tutorials/getting-started/


