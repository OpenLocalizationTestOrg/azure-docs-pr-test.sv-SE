---
title: Webbprogram med tabellagring (Node.js) | Microsoft Docs
description: "En självstudiekurs som bygger på webbprogram med snabb kursen genom att lägga till Azure Storage-tjänster och Azure-modulen."
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
ms.openlocfilehash: 5d7ee2f529b5127ee60ec8b4f5acaa49e75ddf39
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="nodejs-web-application-using-storage"></a><span data-ttu-id="7c4af-103">Node.js-Webbapp som använder lagring</span><span class="sxs-lookup"><span data-stu-id="7c4af-103">Node.js Web Application using Storage</span></span>
## <a name="overview"></a><span data-ttu-id="7c4af-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="7c4af-104">Overview</span></span>
<span data-ttu-id="7c4af-105">I den här självstudiekursen kommer du utökar programmet skapas i den [Node.js-Webbapp med hjälp av] självstudiekurs med hjälp av Microsoft Azure-klientbibliotek för Node.js för att arbeta med data management-tjänster.</span><span class="sxs-lookup"><span data-stu-id="7c4af-105">In this tutorial, you will extend the application created in the [Node.js Web Application using Express] tutorial by using the Microsoft Azure Client Libraries for Node.js to work with data management services.</span></span> <span data-ttu-id="7c4af-106">Du ska utöka ditt program för att skapa ett webbaserat-uppgiftslista program som du kan distribuera till Azure.</span><span class="sxs-lookup"><span data-stu-id="7c4af-106">You will extend your application to create a web-based task-list application that you can deploy to Azure.</span></span> <span data-ttu-id="7c4af-107">Listan kan användaren hämta uppgifter, lägga till nya aktiviteter och markera aktiviteter som slutförda.</span><span class="sxs-lookup"><span data-stu-id="7c4af-107">The task list allows a user to retrieve tasks, add new tasks, and mark tasks as completed.</span></span>

<span data-ttu-id="7c4af-108">Uppgiftsobjekt lagras i Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="7c4af-108">The task items are stored in Azure Storage.</span></span> <span data-ttu-id="7c4af-109">Azure Storage tillhandahåller lagring av Ostrukturerade data som är feltolerant och hög tillgänglighet.</span><span class="sxs-lookup"><span data-stu-id="7c4af-109">Azure Storage provides unstructured data storage that is fault-tolerant and highly available.</span></span> <span data-ttu-id="7c4af-110">Azure Storage innehåller flera datastrukturer där du kan lagra och komma åt data, och du kan utnyttja lagringstjänster från API: er som ingår i Azure SDK för Node.js eller via REST API: er.</span><span class="sxs-lookup"><span data-stu-id="7c4af-110">Azure Storage includes several data structures where you can store and access data, and you can leverage the storage services from the APIs included in the Azure SDK for Node.js or via REST APIs.</span></span> <span data-ttu-id="7c4af-111">Mer information finns i [lagring och åtkomst till Data i Azure].</span><span class="sxs-lookup"><span data-stu-id="7c4af-111">For more information, see [Storing and Accessing Data in Azure].</span></span>

<span data-ttu-id="7c4af-112">Den här kursen förutsätter att du har slutfört den [Node.js-Webbapp] och [Node.js med snabb][Node.js-Webbapp med hjälp av] självstudier.</span><span class="sxs-lookup"><span data-stu-id="7c4af-112">This tutorial assumes that you have completed the [Node.js Web Application] and [Node.js with Express][Node.js Web Application using Express] tutorials.</span></span>

<span data-ttu-id="7c4af-113">Du kommer att lära dig:</span><span class="sxs-lookup"><span data-stu-id="7c4af-113">You will learn:</span></span>

* <span data-ttu-id="7c4af-114">Hur du arbetar med motorn Jade mall</span><span class="sxs-lookup"><span data-stu-id="7c4af-114">How to work with the Jade template engine</span></span>
* <span data-ttu-id="7c4af-115">Hur du arbetar med Azure Data Management services</span><span class="sxs-lookup"><span data-stu-id="7c4af-115">How to work with Azure Data Management services</span></span>

<span data-ttu-id="7c4af-116">En skärmbild av det färdiga programmet understiger:</span><span class="sxs-lookup"><span data-stu-id="7c4af-116">A screenshot of the completed application is below:</span></span>

![Slutförda webbsida i internet explorer](./media/storage-nodejs-use-table-storage-cloud-service-app/getting-started-1.png)

## <a name="setting-storage-credentials-in-webconfig"></a><span data-ttu-id="7c4af-118">Ange autentiseringsuppgifter för lagring i Web.Config</span><span class="sxs-lookup"><span data-stu-id="7c4af-118">Setting Storage Credentials in Web.Config</span></span>
<span data-ttu-id="7c4af-119">Om du vill komma åt Azure Storage, måste du ange autentiseringsuppgifter för lagring.</span><span class="sxs-lookup"><span data-stu-id="7c4af-119">To access Azure Storage, you need to pass in storage credentials.</span></span> <span data-ttu-id="7c4af-120">Om du vill göra detta måste använda web.config Programinställningar.</span><span class="sxs-lookup"><span data-stu-id="7c4af-120">To do this, you utilize web.config application settings.</span></span>
<span data-ttu-id="7c4af-121">Dessa inställningar kommer att skickas som miljövariabler till noden som läses sedan av Azure SDK.</span><span class="sxs-lookup"><span data-stu-id="7c4af-121">Those settings will be passed as environment variables to Node, which are then read by the Azure SDK.</span></span>

> [!NOTE]
> <span data-ttu-id="7c4af-122">Lagring autentiseringsuppgifter används bara när programmet distribueras till Azure.</span><span class="sxs-lookup"><span data-stu-id="7c4af-122">Storage credentials are only used when the application is deployed to Azure.</span></span> <span data-ttu-id="7c4af-123">När du kör i emulatorn programmet kommer att använda lagringsemulatorn.</span><span class="sxs-lookup"><span data-stu-id="7c4af-123">When running in the emulator, the application will use the storage emulator.</span></span>
>
>

<span data-ttu-id="7c4af-124">Utför följande steg för att hämta autentiseringsuppgifter för lagringskonto och lägga till dem i web.config-inställningar:</span><span class="sxs-lookup"><span data-stu-id="7c4af-124">Perform the following steps to retrieve the storage account credentials and add them to the web.config settings:</span></span>

1. <span data-ttu-id="7c4af-125">Om den inte redan är öppen start av Azure PowerShell från den **starta** menyn genom att expandera **alla program, Azure**, högerklicka på **Azure PowerShell**, och välj sedan  **Kör som administratör**.</span><span class="sxs-lookup"><span data-stu-id="7c4af-125">If it is not already open, start the Azure PowerShell from the **Start** menu by expanding **All Programs, Azure**, right-click **Azure PowerShell**, and then select **Run As Administrator**.</span></span>
2. <span data-ttu-id="7c4af-126">Ändra sökvägen till mappen som innehåller programmet.</span><span class="sxs-lookup"><span data-stu-id="7c4af-126">Change directories to the folder containing your application.</span></span> <span data-ttu-id="7c4af-127">Till exempel C:\\nod\\tasklist\\WebRole1.</span><span class="sxs-lookup"><span data-stu-id="7c4af-127">For example, C:\\node\\tasklist\\WebRole1.</span></span>
3. <span data-ttu-id="7c4af-128">Ange följande cmdlet för att hämta kontoinformationen för lagring i Azure Powershell-fönstret:</span><span class="sxs-lookup"><span data-stu-id="7c4af-128">From the Azure Powershell window enter the following cmdlet to retrieve the storage account information:</span></span>

    ```powershell
    PS C:\node\tasklist\WebRole1> Get-AzureStorageAccounts
    ```

   <span data-ttu-id="7c4af-129">Detta hämtar listan över storage-konton och konto nycklar som hör till din värdbaserade tjänst.</span><span class="sxs-lookup"><span data-stu-id="7c4af-129">This retrieves the list of storage accounts and account keys associated with your hosted service.</span></span>

   > [!NOTE]
   > <span data-ttu-id="7c4af-130">Eftersom Azure SDK om du skapar ett lagringskonto när du distribuerar en tjänst, ska ett lagringskonto redan finnas från att distribuera ditt program i föregående guider.</span><span class="sxs-lookup"><span data-stu-id="7c4af-130">Since the Azure SDK creates a storage account when you deploy a service, a storage account should already exist from deploying your application in the previous guides.</span></span>
   >
   >
4. <span data-ttu-id="7c4af-131">Öppna den **ServiceDefinition.csdef** -fil som innehåller miljöinställningar som används när programmet distribueras till Azure:</span><span class="sxs-lookup"><span data-stu-id="7c4af-131">Open the **ServiceDefinition.csdef** file containing the environment settings that are used when the application is deployed to Azure:</span></span>

    ```powershell
    PS C:\node\tasklist> notepad ServiceDefinition.csdef
    ```

5. <span data-ttu-id="7c4af-132">Infoga följande kodblock under **miljö** element, ersätter {LAGRINGSKONTO} och {LAGRINGSÅTKOMSTNYCKEL} med namnet på kontot och den primära nyckeln för lagringskontot som du vill använda för distribution:</span><span class="sxs-lookup"><span data-stu-id="7c4af-132">Insert the following block under **Environment** element, substituting {STORAGE ACCOUNT} and {STORAGE ACCESS KEY} with the account name and the primary key for the storage account you want to use for deployment:</span></span>

  <Variable name="AZURE_STORAGE_ACCOUNT" value="{STORAGE ACCOUNT}" />
  <Variable name="AZURE_STORAGE_ACCESS_KEY" value="{STORAGE ACCESS KEY}" />

   ![Filinnehållet web.cloud.config](./media/storage-nodejs-use-table-storage-cloud-service-app/node37.png)

6. <span data-ttu-id="7c4af-134">Spara filen och stäng Anteckningar.</span><span class="sxs-lookup"><span data-stu-id="7c4af-134">Save the file and close notepad.</span></span>

### <a name="install-additional-modules"></a><span data-ttu-id="7c4af-135">Installera ytterligare moduler</span><span class="sxs-lookup"><span data-stu-id="7c4af-135">Install additional modules</span></span>
1. <span data-ttu-id="7c4af-136">Använd följande kommando för att installera [azure], [nod-uuid] [nconf] och [asynkrona] moduler lokalt samt att spara en post för att den **package.json** fil:</span><span class="sxs-lookup"><span data-stu-id="7c4af-136">Use the following command to install the [azure], [node-uuid], [nconf] and [async] modules locally as well as to save an entry for them to the **package.json** file:</span></span>

  ```powershell
  PS C:\node\tasklist\WebRole1> npm install azure-storage node-uuid async nconf --save
  ```

  <span data-ttu-id="7c4af-137">Kommandots utdata ska se ut ungefär så här:</span><span class="sxs-lookup"><span data-stu-id="7c4af-137">The output of this command should appear similar to the following:</span></span>

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

## <a name="using-the-table-service-in-a-node-application"></a><span data-ttu-id="7c4af-138">Med hjälp av tjänsten tabellen i en node-App</span><span class="sxs-lookup"><span data-stu-id="7c4af-138">Using the Table service in a node application</span></span>
<span data-ttu-id="7c4af-139">I det här avsnittet kommer du utökar den grundläggande program som skapats av **express** kommandot genom att lägga till en **task.js** -fil som innehåller modellen för dina uppgifter.</span><span class="sxs-lookup"><span data-stu-id="7c4af-139">In this section you will extend the basic application created by the **express** command by adding a **task.js** file which contains the model for your tasks.</span></span> <span data-ttu-id="7c4af-140">Du kan också ändra den befintliga **app.js** och skapa en ny **tasklist.js** fil som används i modellen.</span><span class="sxs-lookup"><span data-stu-id="7c4af-140">You will also modify the existing **app.js** and create a new **tasklist.js** file that uses the model.</span></span>

### <a name="create-the-model"></a><span data-ttu-id="7c4af-141">Skapa modellen</span><span class="sxs-lookup"><span data-stu-id="7c4af-141">Create the model</span></span>
1. <span data-ttu-id="7c4af-142">I den **WebRole1** directory, skapa en ny katalog med namnet **modeller**.</span><span class="sxs-lookup"><span data-stu-id="7c4af-142">In the **WebRole1** directory, create a new directory named **models**.</span></span>
2. <span data-ttu-id="7c4af-143">I den **modeller** directory, skapa en ny fil med namnet **task.js**.</span><span class="sxs-lookup"><span data-stu-id="7c4af-143">In the **models** directory, create a new file named **task.js**.</span></span> <span data-ttu-id="7c4af-144">Den här filen innehåller modellen för de aktiviteter som skapats av programmet.</span><span class="sxs-lookup"><span data-stu-id="7c4af-144">This file will contain the model for the tasks created by your application.</span></span>
3. <span data-ttu-id="7c4af-145">I början av den **task.js** lägger du till följande kod för att referera till bibliotek som krävs:</span><span class="sxs-lookup"><span data-stu-id="7c4af-145">At the beginning of the **task.js** file, add the following code to reference required libraries:</span></span>

    ```nodejs
    var azure = require('azure-storage');
    var uuid = require('node-uuid');
    var entityGen = azure.TableUtilities.entityGenerator;
    ```

4. <span data-ttu-id="7c4af-146">Sedan lägger du till kod för att definiera och exportera aktivitetsobjektet.</span><span class="sxs-lookup"><span data-stu-id="7c4af-146">Next, you will add code to define and export the Task object.</span></span> <span data-ttu-id="7c4af-147">Det här objektet är ansvarig för att ansluta till tabellen.</span><span class="sxs-lookup"><span data-stu-id="7c4af-147">This object is responsible for connecting to the table.</span></span>

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

5. <span data-ttu-id="7c4af-148">Lägg till följande kod för att definiera ytterligare metoder för aktivitetsobjektet som tillåter samverkan med data som lagras i tabellen:</span><span class="sxs-lookup"><span data-stu-id="7c4af-148">Next, add the following code to define additional methods on the Task object, which allow interactions with data stored in the table:</span></span>

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
        // use entityGenerator to set types
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

6. <span data-ttu-id="7c4af-149">Spara och Stäng den **task.js** fil.</span><span class="sxs-lookup"><span data-stu-id="7c4af-149">Save and close the **task.js** file.</span></span>

### <a name="create-the-controller"></a><span data-ttu-id="7c4af-150">Skapa styrningen</span><span class="sxs-lookup"><span data-stu-id="7c4af-150">Create the controller</span></span>
1. <span data-ttu-id="7c4af-151">I den **WebRole1/vägar** directory, skapa en ny fil med namnet **tasklist.js** och öppna den i en textredigerare.</span><span class="sxs-lookup"><span data-stu-id="7c4af-151">In the **WebRole1/routes** directory, create a new file named **tasklist.js** and open it in a text editor.</span></span>
2. <span data-ttu-id="7c4af-152">Lägg till följande kod i **tasklist.js**.</span><span class="sxs-lookup"><span data-stu-id="7c4af-152">Add the following code to **tasklist.js**.</span></span> <span data-ttu-id="7c4af-153">Den läser in i azure och async-moduler som används av **tasklist.js**.</span><span class="sxs-lookup"><span data-stu-id="7c4af-153">This loads the azure and async modules, which are used by **tasklist.js**.</span></span> <span data-ttu-id="7c4af-154">Detta definierar också den **TaskList** funktion, som mottar en instans av den **aktivitet** objektet som vi definierade tidigare:</span><span class="sxs-lookup"><span data-stu-id="7c4af-154">This also defines the **TaskList** function, which is passed an instance of the **Task** object we defined earlier:</span></span>

    ```nodejs
    var azure = require('azure-storage');
    var async = require('async');

    module.exports = TaskList;

    function TaskList(task) {
      this.task = task;
    }
    ```

3. <span data-ttu-id="7c4af-155">Fortsätta att lägga till den **tasklist.js** filen genom att lägga till de metoder som används för att **showTasks**, **addTask**, och **completeTasks**:</span><span class="sxs-lookup"><span data-stu-id="7c4af-155">Continue adding to the **tasklist.js** file by adding the methods used to **showTasks**, **addTask**, and **completeTasks**:</span></span>

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

4. <span data-ttu-id="7c4af-156">Spara den **tasklist.js** fil.</span><span class="sxs-lookup"><span data-stu-id="7c4af-156">Save the **tasklist.js** file.</span></span>

### <a name="modify-appjs"></a><span data-ttu-id="7c4af-157">Ändra app.js</span><span class="sxs-lookup"><span data-stu-id="7c4af-157">Modify app.js</span></span>
1. <span data-ttu-id="7c4af-158">I den **WebRole1** katalog, öppna den **app.js** i en textredigerare.</span><span class="sxs-lookup"><span data-stu-id="7c4af-158">In the **WebRole1** directory, open the **app.js** file in a text editor.</span></span>
2. <span data-ttu-id="7c4af-159">Lägg till följande för att läsa in modulen för azure och ange namn och partition tabellnyckel i början av filen:</span><span class="sxs-lookup"><span data-stu-id="7c4af-159">At the beginning of the file, add the following to load the azure module and set the table name and partition key:</span></span>

    ```nodejs
    var azure = require('azure-storage');
    var tableName = 'tasks';
    var partitionKey = 'hometasks';
    ```

3. <span data-ttu-id="7c4af-160">Rulla ned till där du ser följande rad i filen app.js:</span><span class="sxs-lookup"><span data-stu-id="7c4af-160">In the app.js file, scroll down to where you see the following line:</span></span>

    ```nodejs
    app.use('/', routes);
    app.use('/users', users);
    ```

    <span data-ttu-id="7c4af-161">Ersätt ovanstående rader med kod som visas nedan.</span><span class="sxs-lookup"><span data-stu-id="7c4af-161">Replace the above lines with the code shown below.</span></span> <span data-ttu-id="7c4af-162">Detta ska initiera en instans av <strong>aktivitet</strong> med en anslutning till ditt lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="7c4af-162">This will initialize an instance of <strong>Task</strong> with a connection to your storage account.</span></span> <span data-ttu-id="7c4af-163">Detta har överförts till den <strong>TaskList</strong>, som använder det för att kommunicera med tjänsten tabell:</span><span class="sxs-lookup"><span data-stu-id="7c4af-163">This is passed to the <strong>TaskList</strong>, which will use it to communicate with the Table service:</span></span>

    ```nodejs
    var TaskList = require('./routes/tasklist');
    var Task = require('./models/task');
    var task = new Task(azure.createTableService(), tableName, partitionKey);
    var taskList = new TaskList(task);

    app.get('/', taskList.showTasks.bind(taskList));
    app.post('/addtask', taskList.addTask.bind(taskList));
    app.post('/completetask', taskList.completeTask.bind(taskList));
    ```

4. <span data-ttu-id="7c4af-164">Spara den **app.js** fil.</span><span class="sxs-lookup"><span data-stu-id="7c4af-164">Save the **app.js** file.</span></span>

### <a name="modify-the-index-view"></a><span data-ttu-id="7c4af-165">Ändra indexvyn</span><span class="sxs-lookup"><span data-stu-id="7c4af-165">Modify the index view</span></span>
1. <span data-ttu-id="7c4af-166">Ändra sökvägen till den **vyer** katalog och öppna den **index.jade** i en textredigerare.</span><span class="sxs-lookup"><span data-stu-id="7c4af-166">Change directories to the **views** directory and open the **index.jade** file in a text editor.</span></span>
2. <span data-ttu-id="7c4af-167">Ersätt innehållet i den **index.jade** filen med koden nedan.</span><span class="sxs-lookup"><span data-stu-id="7c4af-167">Replace the contents of the **index.jade** file with the code below.</span></span> <span data-ttu-id="7c4af-168">Detta definierar vyn för att visa befintliga aktiviteter, samt ett formulär för att lägga till nya aktiviteter och markera befintliga som slutförts.</span><span class="sxs-lookup"><span data-stu-id="7c4af-168">This defines the view for displaying existing tasks, as well as a form for adding new tasks and marking existing ones as completed.</span></span>

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

3. <span data-ttu-id="7c4af-169">Spara och Stäng **index.jade** fil.</span><span class="sxs-lookup"><span data-stu-id="7c4af-169">Save and close **index.jade** file.</span></span>

### <a name="modify-the-global-layout"></a><span data-ttu-id="7c4af-170">Ändra globala layouten</span><span class="sxs-lookup"><span data-stu-id="7c4af-170">Modify the global layout</span></span>
<span data-ttu-id="7c4af-171">Filen **layout.jade** i katalogen **views** används som en global mall för andra **.jade**-filer.</span><span class="sxs-lookup"><span data-stu-id="7c4af-171">The **layout.jade** file in the **views** directory is used as a global template for other **.jade** files.</span></span> <span data-ttu-id="7c4af-172">I det här steget ändrar du den så att den använder [Twitter Bootstrap](https://github.com/twbs/bootstrap), vilket är en verktygslåda som gör det enkelt att utforma en snygg webbplats.</span><span class="sxs-lookup"><span data-stu-id="7c4af-172">In this step you will modify it to use [Twitter Bootstrap](https://github.com/twbs/bootstrap), which is a toolkit that makes it easy to design a nice looking website.</span></span>

1. <span data-ttu-id="7c4af-173">Ladda ned och extrahera filerna för [Twitter Bootstrap](http://getbootstrap.com/).</span><span class="sxs-lookup"><span data-stu-id="7c4af-173">Download and extract the files for [Twitter Bootstrap](http://getbootstrap.com/).</span></span> <span data-ttu-id="7c4af-174">Kopiera den **bootstrap.min.css** filen från den **bootstrap\\dist\\css** mappen till den **offentliga\\matmallar** katalogen för tillämpningsprogrammet tasklist.</span><span class="sxs-lookup"><span data-stu-id="7c4af-174">Copy the **bootstrap.min.css** file from the **bootstrap\\dist\\css** folder to the **public\\stylesheets** directory of your tasklist application.</span></span>
2. <span data-ttu-id="7c4af-175">Från den **vyer** mapp, öppna den **layout.jade** i en textredigerare och Ersätt innehållet med följande:</span><span class="sxs-lookup"><span data-stu-id="7c4af-175">From the **views** folder, open the **layout.jade** in your text editor and replace the contents with the following:</span></span>

    <span data-ttu-id="7c4af-176">doctype html html head rubrik = Rubriklänk (rel = 'stylesheet', href='/stylesheets/bootstrap.min.css') länk (rel = 'stylesheet', href='/stylesheets/style.css') body.app nav.navbar.navbar standard div.navbar huvud a.navbar-brand(href='/') min  Uppgifter blockera innehåll</span><span class="sxs-lookup"><span data-stu-id="7c4af-176">doctype html  html    head      title= title      link(rel='stylesheet', href='/stylesheets/bootstrap.min.css')      link(rel='stylesheet', href='/stylesheets/style.css')    body.app      nav.navbar.navbar-default        div.navbar-header          a.navbar-brand(href='/') My Tasks      block content</span></span>

3. <span data-ttu-id="7c4af-177">Spara den **layout.jade** fil.</span><span class="sxs-lookup"><span data-stu-id="7c4af-177">Save the **layout.jade** file.</span></span>

### <a name="running-the-application-in-the-emulator"></a><span data-ttu-id="7c4af-178">Kör programmet i emulatorn</span><span class="sxs-lookup"><span data-stu-id="7c4af-178">Running the Application in the Emulator</span></span>
<span data-ttu-id="7c4af-179">Använd följande kommando för att starta programmet i emulatorn.</span><span class="sxs-lookup"><span data-stu-id="7c4af-179">Use the following command to start the application in the emulator.</span></span>

```powershell
PS C:\node\tasklist\WebRole1> start-azureemulator -launch
```

<span data-ttu-id="7c4af-180">Webbläsaren öppnas och visar följande sida:</span><span class="sxs-lookup"><span data-stu-id="7c4af-180">The browser will open and displays the following page:</span></span>

![En webbplats som växlingsbart systemminne med titeln uppgiftslistan med en tabell som innehåller uppgifter och för att lägga till en ny uppgift.](./media/storage-nodejs-use-table-storage-cloud-service-app/node44.png)

<span data-ttu-id="7c4af-182">Använd formuläret för att lägga till objekt eller ta bort befintliga objekt genom att markera dem som slutförda.</span><span class="sxs-lookup"><span data-stu-id="7c4af-182">Use the form to add items, or remove existing items by marking them as completed.</span></span>

## <a name="publishing-the-application-to-azure"></a><span data-ttu-id="7c4af-183">Publicera program till Azure</span><span class="sxs-lookup"><span data-stu-id="7c4af-183">Publishing the Application to Azure</span></span>
<span data-ttu-id="7c4af-184">Anropa följande cmdlet om du vill distribuera dina värdbaserade tjänsten till Azure i Windows PowerShell-fönstret.</span><span class="sxs-lookup"><span data-stu-id="7c4af-184">In the Windows PowerShell window, call the following cmdlet to redeploy your hosted service to Azure.</span></span>

```powershell
PS C:\node\tasklist\WebRole1> Publish-AzureServiceProject -name myuniquename -location datacentername -launch
```

<span data-ttu-id="7c4af-185">Ersätt **myuniquename** med ett unikt namn för det här programmet.</span><span class="sxs-lookup"><span data-stu-id="7c4af-185">Replace **myuniquename** with a unique name for this application.</span></span> <span data-ttu-id="7c4af-186">Ersätt **datacentername** med namnet på ett Azure-datacenter som **västra USA**.</span><span class="sxs-lookup"><span data-stu-id="7c4af-186">Replace **datacentername** with the name of an Azure data center, such as **West US**.</span></span>

<span data-ttu-id="7c4af-187">När distributionen är klar bör du se ett svar som liknar följande:</span><span class="sxs-lookup"><span data-stu-id="7c4af-187">After the deployment is complete, you should see a response similar to the following:</span></span>

```
  PS C:\node\tasklist> publish-azureserviceproject -servicename tasklist -location "West US"
  WARNING: Publishing tasklist to Microsoft Azure. This may take several minutes...
  WARNING: 2:18:42 PM - Preparing runtime deployment for service 'tasklist'
  WARNING: 2:18:42 PM - Verifying storage account 'tasklist'...
  WARNING: 2:18:43 PM - Preparing deployment for tasklist with Subscription ID: 65a1016d-0f67-45d2-b838-b8f373d6d52e...
  WARNING: 2:19:01 PM - Connecting...
  WARNING: 2:19:02 PM - Uploading Package to storage service larrystore...
  WARNING: 2:19:40 PM - Upgrading...
  WARNING: 2:22:48 PM - Created Deployment ID: b7134ab29b1249ff84ada2bd157f296a.
  WARNING: 2:22:48 PM - Initializing...
  WARNING: 2:22:49 PM - Instance WebRole1_IN_0 of role WebRole1 is ready.
  WARNING: 2:22:50 PM - Created Website URL: http://tasklist.cloudapp.net/.
```

<span data-ttu-id="7c4af-188">Precis som tidigare, eftersom du har angett den **-starta** alternativet webbläsaren öppnas och visar programmet körs i Azure när publiceringen är klar.</span><span class="sxs-lookup"><span data-stu-id="7c4af-188">As before, because you specified the **-launch** option, the browser opens and displays your application running in Azure when publishing is completed.</span></span>

![Ett webbläsarfönster som visar sidan uppgiftslistan.](./media/storage-nodejs-use-table-storage-cloud-service-app/getting-started-1.png)

## <a name="stopping-and-deleting-your-application"></a><span data-ttu-id="7c4af-191">Stoppa och ta bort programmet</span><span class="sxs-lookup"><span data-stu-id="7c4af-191">Stopping and Deleting Your Application</span></span>
<span data-ttu-id="7c4af-192">När du distribuerar ditt program, kan du vill inaktivera den så att du kan undvika kostnader eller skapa och distribuera andra program inom den kostnadsfria utvärderingsversionen tidsperioden.</span><span class="sxs-lookup"><span data-stu-id="7c4af-192">After deploying your application, you may want to disable it so you can avoid costs or build and deploy other applications within the free trial time period.</span></span>

<span data-ttu-id="7c4af-193">Azure fakturerar webbrollsinstanser per timme förbrukad servertid.</span><span class="sxs-lookup"><span data-stu-id="7c4af-193">Azure bills web role instances per hour of server time consumed.</span></span>
<span data-ttu-id="7c4af-194">Servertid förbrukas när programmet har distribuerats, även om instanserna inte körs och är i stoppat tillstånd.</span><span class="sxs-lookup"><span data-stu-id="7c4af-194">Server time is consumed once your application is deployed, even if the instances are not running and are in the stopped state.</span></span>

<span data-ttu-id="7c4af-195">Följande steg visar hur du stoppar och ta bort programmet.</span><span class="sxs-lookup"><span data-stu-id="7c4af-195">The following steps show you how to stop and delete your application.</span></span>

1. <span data-ttu-id="7c4af-196">Stoppa tjänstdistributionen som skapades i föregående avsnitt med följande cmdlet i Windows PowerShell-fönstret:</span><span class="sxs-lookup"><span data-stu-id="7c4af-196">In the Windows PowerShell window, stop the service deployment created in the previous section with the following cmdlet:</span></span>

    ```powershell
    PS C:\node\tasklist\WebRole1> Stop-AzureService
    ```

   <span data-ttu-id="7c4af-197">Det kan ta flera minuter att stoppa tjänsten.</span><span class="sxs-lookup"><span data-stu-id="7c4af-197">Stopping the service may take several minutes.</span></span> <span data-ttu-id="7c4af-198">När tjänsten har stoppats får du ett meddelande som anger att den har stoppats.</span><span class="sxs-lookup"><span data-stu-id="7c4af-198">When the service is stopped, you receive a message indicating that it has stopped.</span></span>

2. <span data-ttu-id="7c4af-199">Ta bort tjänsten genom att anropa följande cmdlet:</span><span class="sxs-lookup"><span data-stu-id="7c4af-199">To delete the service, call the following cmdlet:</span></span>

    ```powershell
    PS C:\node\tasklist\WebRole1> Remove-AzureService contosotasklist
    ```

   <span data-ttu-id="7c4af-200">När du uppmanas, anger du **Y** för att ta bort tjänsten.</span><span class="sxs-lookup"><span data-stu-id="7c4af-200">When prompted, enter **Y** to delete the service.</span></span>

   <span data-ttu-id="7c4af-201">Det kan ta flera minuter att ta bort tjänsten.</span><span class="sxs-lookup"><span data-stu-id="7c4af-201">Deleting the service may take several minutes.</span></span> <span data-ttu-id="7c4af-202">När tjänsten har tagits bort får du ett meddelande som anger att tjänsten har tagits bort.</span><span class="sxs-lookup"><span data-stu-id="7c4af-202">After the service has been deleted you receive a message indicating that the service was deleted.</span></span>

<span data-ttu-id="7c4af-203">[Node.js-Webbapp med hjälp av]: http://azure.microsoft.com/develop/nodejs/tutorials/web-app-with-express/</span><span class="sxs-lookup"><span data-stu-id="7c4af-203">[Node.js Web Application using Express]: http://azure.microsoft.com/develop/nodejs/tutorials/web-app-with-express/</span></span>
<span data-ttu-id="7c4af-204">[lagring och åtkomst till Data i Azure]: http://msdn.microsoft.com/library/azure/gg433040.aspx</span><span class="sxs-lookup"><span data-stu-id="7c4af-204">[Storing and Accessing Data in Azure]: http://msdn.microsoft.com/library/azure/gg433040.aspx</span></span>
<span data-ttu-id="7c4af-205">[Node.js-Webbapp]: http://azure.microsoft.com/develop/nodejs/tutorials/getting-started/</span><span class="sxs-lookup"><span data-stu-id="7c4af-205">[Node.js Web Application]: http://azure.microsoft.com/develop/nodejs/tutorials/getting-started/</span></span>


