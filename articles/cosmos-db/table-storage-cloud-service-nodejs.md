---
title: Webbprogram med tabellagring (Node.js) | Microsoft Docs
description: "En självstudiekurs som bygger på webbprogram med snabb kursen genom att lägga till Azure Storage-tjänster och Azure-modulen."
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
ms.openlocfilehash: b802f880c1131abb7eb9ba00dd8f2e65017bc802
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="nodejs-web-application-using-storage"></a><span data-ttu-id="52d6f-103">Node.js-Webbapp som använder lagring</span><span class="sxs-lookup"><span data-stu-id="52d6f-103">Node.js Web Application using Storage</span></span>
## <a name="overview"></a><span data-ttu-id="52d6f-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="52d6f-104">Overview</span></span>
<span data-ttu-id="52d6f-105">I den här självstudiekursen programmet du skapade i den [Node.js-Webbapp med hjälp av] kursen utökas som arbetar med data management-tjänster med Microsoft Azure-klientbibliotek för Node.js.</span><span class="sxs-lookup"><span data-stu-id="52d6f-105">In this tutorial, the application you created in the [Node.js Web Application using Express] tutorial is extended using the Microsoft Azure Client Libraries for Node.js to work with data management services.</span></span> <span data-ttu-id="52d6f-106">Du kan utöka ditt program genom att skapa en webbaserad-uppgiftslista program som du kan distribuera till Azure.</span><span class="sxs-lookup"><span data-stu-id="52d6f-106">You extend your application by creating a web-based task-list application that you can deploy to Azure.</span></span> <span data-ttu-id="52d6f-107">Listan kan användaren hämta uppgifter, lägga till nya aktiviteter och markera aktiviteter som slutförda.</span><span class="sxs-lookup"><span data-stu-id="52d6f-107">The task list allows a user to retrieve tasks, add new tasks, and mark tasks as completed.</span></span>

<span data-ttu-id="52d6f-108">Uppgiftsobjekt lagras i Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="52d6f-108">The task items are stored in Azure Storage.</span></span> <span data-ttu-id="52d6f-109">Azure Storage tillhandahåller lagring av Ostrukturerade data som är feltolerant och hög tillgänglighet.</span><span class="sxs-lookup"><span data-stu-id="52d6f-109">Azure Storage provides unstructured data storage that is fault-tolerant and highly available.</span></span> <span data-ttu-id="52d6f-110">Azure Storage innehåller flera datastrukturer där du kan lagra och komma åt data.</span><span class="sxs-lookup"><span data-stu-id="52d6f-110">Azure Storage includes several data structures where you can store and access data.</span></span> <span data-ttu-id="52d6f-111">Du kan använda lagringstjänsterna från API: er som ingår i Azure SDK för Node.js eller via REST API: er.</span><span class="sxs-lookup"><span data-stu-id="52d6f-111">You can use the storage services from the APIs included in the Azure SDK for Node.js or via REST APIs.</span></span> <span data-ttu-id="52d6f-112">Mer information finns i [lagring och åtkomst till Data i Azure].</span><span class="sxs-lookup"><span data-stu-id="52d6f-112">For more information, see [Storing and Accessing Data in Azure].</span></span>

<span data-ttu-id="52d6f-113">Den här kursen förutsätter att du har slutfört den [Node.js-Webbapp] och [Node.js med snabb][Node.js-Webbapp med hjälp av] självstudier.</span><span class="sxs-lookup"><span data-stu-id="52d6f-113">This tutorial assumes that you have completed the [Node.js Web Application] and [Node.js with Express][Node.js Web Application using Express] tutorials.</span></span>

<span data-ttu-id="52d6f-114">Det innehåller följande information:</span><span class="sxs-lookup"><span data-stu-id="52d6f-114">It contains the following information:</span></span>

* <span data-ttu-id="52d6f-115">Hur du arbetar med motorn Jade mall</span><span class="sxs-lookup"><span data-stu-id="52d6f-115">How to work with the Jade template engine</span></span>
* <span data-ttu-id="52d6f-116">Hur du arbetar med Azure Data Management services</span><span class="sxs-lookup"><span data-stu-id="52d6f-116">How to work with Azure Data Management services</span></span>

<span data-ttu-id="52d6f-117">Följande skärmbild visar den färdiga appen:</span><span class="sxs-lookup"><span data-stu-id="52d6f-117">The following screenshot shows the completed application:</span></span>

![Slutförda webbsida i internet explorer](./media/table-storage-cloud-service-nodejs/getting-started-1.png)

## <a name="setting-storage-credentials-in-webconfig"></a><span data-ttu-id="52d6f-119">Ange autentiseringsuppgifter för lagring i Web.Config</span><span class="sxs-lookup"><span data-stu-id="52d6f-119">Setting Storage Credentials in Web.Config</span></span>
<span data-ttu-id="52d6f-120">Du måste överföra i lagring autentiseringsuppgifter för åtkomst till Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="52d6f-120">You must pass in storage credentials to access Azure Storage.</span></span> <span data-ttu-id="52d6f-121">Detta görs genom att använda inställningarna för web.config-programmet.</span><span class="sxs-lookup"><span data-stu-id="52d6f-121">This is done by utilizing the web.config application settings.</span></span>
<span data-ttu-id="52d6f-122">Inställningarna för web.config skickas som miljövariabler till noden, som läses sedan av Azure SDK.</span><span class="sxs-lookup"><span data-stu-id="52d6f-122">The web.config settings are passed as environment variables to Node, which are then read by the Azure SDK.</span></span>

> [!NOTE]
> <span data-ttu-id="52d6f-123">Lagring autentiseringsuppgifter används bara när programmet distribueras till Azure.</span><span class="sxs-lookup"><span data-stu-id="52d6f-123">Storage credentials are only used when the application is deployed to Azure.</span></span> <span data-ttu-id="52d6f-124">När du kör i emulatorn använder programmet storage-emulatorn.</span><span class="sxs-lookup"><span data-stu-id="52d6f-124">When running in the emulator, the application uses the storage emulator.</span></span>
>
>

<span data-ttu-id="52d6f-125">Utför följande steg för att hämta autentiseringsuppgifter för lagringskonto och lägga till dem i web.config-inställningar:</span><span class="sxs-lookup"><span data-stu-id="52d6f-125">Perform the following steps to retrieve the storage account credentials and add them to the web.config settings:</span></span>

1. <span data-ttu-id="52d6f-126">Om den inte redan är öppen start av Azure PowerShell från den **starta** menyn genom att expandera **alla program, Azure**, högerklicka på **Azure PowerShell**, och välj sedan  **Kör som administratör**.</span><span class="sxs-lookup"><span data-stu-id="52d6f-126">If it is not already open, start the Azure PowerShell from the **Start** menu by expanding **All Programs, Azure**, right-click **Azure PowerShell**, and then select **Run As Administrator**.</span></span>
2. <span data-ttu-id="52d6f-127">Ändra sökvägen till mappen som innehåller programmet.</span><span class="sxs-lookup"><span data-stu-id="52d6f-127">Change directories to the folder containing your application.</span></span> <span data-ttu-id="52d6f-128">Till exempel C:\\nod\\tasklist\\WebRole1.</span><span class="sxs-lookup"><span data-stu-id="52d6f-128">For example, C:\\node\\tasklist\\WebRole1.</span></span>
3. <span data-ttu-id="52d6f-129">Ange följande cmdlet för att hämta kontoinformationen för lagring i Azure Powershell-fönstret:</span><span class="sxs-lookup"><span data-stu-id="52d6f-129">From the Azure Powershell window, enter the following cmdlet to retrieve the storage account information:</span></span>

    ```powershell
    PS C:\node\tasklist\WebRole1> Get-AzureStorageAccounts
    ```

   <span data-ttu-id="52d6f-130">Föregående cmdlet hämtar listan över storage-konton och konto nycklar som hör till din värdbaserade tjänst.</span><span class="sxs-lookup"><span data-stu-id="52d6f-130">The preceding cmdlet retrieves the list of storage accounts and account keys associated with your hosted service.</span></span>

   > [!NOTE]
   > <span data-ttu-id="52d6f-131">Eftersom Azure SDK om du skapar ett lagringskonto när du distribuerar en tjänst, ska ett lagringskonto redan finnas från att distribuera ditt program i föregående guider.</span><span class="sxs-lookup"><span data-stu-id="52d6f-131">Since the Azure SDK creates a storage account when you deploy a service, a storage account should already exist from deploying your application in the previous guides.</span></span>
   >
   >
4. <span data-ttu-id="52d6f-132">Öppna den **ServiceDefinition.csdef** -fil som innehåller miljöinställningar som används när programmet distribueras till Azure:</span><span class="sxs-lookup"><span data-stu-id="52d6f-132">Open the **ServiceDefinition.csdef** file containing the environment settings that are used when the application is deployed to Azure:</span></span>

    ```powershell
    PS C:\node\tasklist> notepad ServiceDefinition.csdef
    ```

5. <span data-ttu-id="52d6f-133">Infoga följande kodblock under **miljö** element, ersätter {LAGRINGSKONTO} och {LAGRINGSÅTKOMSTNYCKEL} med namnet på kontot och den primära nyckeln för lagringskontot som du vill använda för distribution:</span><span class="sxs-lookup"><span data-stu-id="52d6f-133">Insert the following block under **Environment** element, substituting {STORAGE ACCOUNT} and {STORAGE ACCESS KEY} with the account name and the primary key for the storage account you want to use for deployment:</span></span>

  <Variable name="AZURE_STORAGE_ACCOUNT" value="{STORAGE ACCOUNT}" />
  <Variable name="AZURE_STORAGE_ACCESS_KEY" value="{STORAGE ACCESS KEY}" />

   ![Filinnehållet web.cloud.config](./media/table-storage-cloud-service-nodejs/node37.png)

6. <span data-ttu-id="52d6f-135">Spara filen och stäng Anteckningar.</span><span class="sxs-lookup"><span data-stu-id="52d6f-135">Save the file and close notepad.</span></span>

### <a name="install-additional-modules"></a><span data-ttu-id="52d6f-136">Installera ytterligare moduler</span><span class="sxs-lookup"><span data-stu-id="52d6f-136">Install additional modules</span></span>
1. <span data-ttu-id="52d6f-137">Använd följande kommando för att installera [azure], [nod-uuid] [nconf] och [asynkrona] moduler lokalt samt att spara en post för att den **package.json** fil:</span><span class="sxs-lookup"><span data-stu-id="52d6f-137">Use the following command to install the [azure], [node-uuid], [nconf] and [async] modules locally as well as to save an entry for them to the **package.json** file:</span></span>

  ```powershell
  PS C:\node\tasklist\WebRole1> npm install azure-storage node-uuid async nconf --save
  ```

  <span data-ttu-id="52d6f-138">Kommandots utdata ska se ut ungefär så här:</span><span class="sxs-lookup"><span data-stu-id="52d6f-138">The output of this command should appear similar to the following:</span></span>

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

## <a name="using-the-table-service-in-a-node-application"></a><span data-ttu-id="52d6f-139">Med hjälp av tjänsten tabellen i en node-App</span><span class="sxs-lookup"><span data-stu-id="52d6f-139">Using the Table service in a node application</span></span>
<span data-ttu-id="52d6f-140">I det här avsnittet grundläggande program som skapats av den **express** kommandot utökas genom att lägga till en **task.js** -fil som innehåller modellen för dina uppgifter.</span><span class="sxs-lookup"><span data-stu-id="52d6f-140">In this section, the basic application created by the **express** command is extended by adding a **task.js** file containing the model for your tasks.</span></span> <span data-ttu-id="52d6f-141">Ändra den befintliga **app.js** filen och skapa en ny **tasklist.js** fil som används i modellen.</span><span class="sxs-lookup"><span data-stu-id="52d6f-141">Modify the existing **app.js** file and create a new **tasklist.js** file that uses the model.</span></span>

### <a name="create-the-model"></a><span data-ttu-id="52d6f-142">Skapa modellen</span><span class="sxs-lookup"><span data-stu-id="52d6f-142">Create the model</span></span>
1. <span data-ttu-id="52d6f-143">I den **WebRole1** directory, skapa en ny katalog med namnet **modeller**.</span><span class="sxs-lookup"><span data-stu-id="52d6f-143">In the **WebRole1** directory, create a new directory named **models**.</span></span>
2. <span data-ttu-id="52d6f-144">I den **modeller** directory, skapa en ny fil med namnet **task.js**.</span><span class="sxs-lookup"><span data-stu-id="52d6f-144">In the **models** directory, create a new file named **task.js**.</span></span> <span data-ttu-id="52d6f-145">Den här filen innehåller modellen för de aktiviteter som skapats av programmet.</span><span class="sxs-lookup"><span data-stu-id="52d6f-145">This file contains the model for the tasks created by your application.</span></span>
3. <span data-ttu-id="52d6f-146">I början av den **task.js** lägger du till följande kod för att referera till bibliotek som krävs:</span><span class="sxs-lookup"><span data-stu-id="52d6f-146">At the beginning of the **task.js** file, add the following code to reference required libraries:</span></span>

    ```nodejs
    var azure = require('azure-storage');
    var uuid = require('node-uuid');
    var entityGen = azure.TableUtilities.entityGenerator;
    ```

4. <span data-ttu-id="52d6f-147">Lägg till kod för att definiera och exportera aktivitetsobjektet.</span><span class="sxs-lookup"><span data-stu-id="52d6f-147">Next, add code to define and export the Task object.</span></span> <span data-ttu-id="52d6f-148">Aktivitetsobjektet ansvarar för att ansluta till tabellen.</span><span class="sxs-lookup"><span data-stu-id="52d6f-148">The Task object is responsible for connecting to the table.</span></span>

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

5. <span data-ttu-id="52d6f-149">Lägg till följande kod för att definiera ytterligare metoder för aktivitetsobjektet som tillåter samverkan med data som lagras i tabellen:</span><span class="sxs-lookup"><span data-stu-id="52d6f-149">Next, add the following code to define additional methods on the Task object, which allow interactions with data stored in the table:</span></span>

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

6. <span data-ttu-id="52d6f-150">Spara och Stäng den **task.js** fil.</span><span class="sxs-lookup"><span data-stu-id="52d6f-150">Save and close the **task.js** file.</span></span>

### <a name="create-the-controller"></a><span data-ttu-id="52d6f-151">Skapa styrningen</span><span class="sxs-lookup"><span data-stu-id="52d6f-151">Create the controller</span></span>
1. <span data-ttu-id="52d6f-152">I den **WebRole1/vägar** directory, skapa en ny fil med namnet **tasklist.js** och öppna den i en textredigerare.</span><span class="sxs-lookup"><span data-stu-id="52d6f-152">In the **WebRole1/routes** directory, create a new file named **tasklist.js** and open it in a text editor.</span></span>
2. <span data-ttu-id="52d6f-153">Lägg till följande kod i **tasklist.js**.</span><span class="sxs-lookup"><span data-stu-id="52d6f-153">Add the following code to **tasklist.js**.</span></span> <span data-ttu-id="52d6f-154">Den här koden läser in i azure och async-moduler som används av **tasklist.js** och definierar de **TaskList** funktion, som mottar en instans av den **aktivitet** objekt vi definierade tidigare:</span><span class="sxs-lookup"><span data-stu-id="52d6f-154">This code loads the azure and async modules, which are used by **tasklist.js** and defines the **TaskList** function, which is passed an instance of the **Task** object we defined earlier:</span></span>

    ```nodejs
    var azure = require('azure-storage');
    var async = require('async');

    module.exports = TaskList;

    function TaskList(task) {
      this.task = task;
    }
    ```

3. <span data-ttu-id="52d6f-155">Fortsätta att lägga till den **tasklist.js** filen genom att lägga till de metoder som används för att **showTasks**, **addTask**, och **completeTasks**:</span><span class="sxs-lookup"><span data-stu-id="52d6f-155">Continue adding to the **tasklist.js** file by adding the methods used to **showTasks**, **addTask**, and **completeTasks**:</span></span>

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

4. <span data-ttu-id="52d6f-156">Spara den **tasklist.js** fil.</span><span class="sxs-lookup"><span data-stu-id="52d6f-156">Save the **tasklist.js** file.</span></span>

### <a name="modify-appjs"></a><span data-ttu-id="52d6f-157">Ändra app.js</span><span class="sxs-lookup"><span data-stu-id="52d6f-157">Modify app.js</span></span>
1. <span data-ttu-id="52d6f-158">I den **WebRole1** katalog, öppna den **app.js** i en textredigerare.</span><span class="sxs-lookup"><span data-stu-id="52d6f-158">In the **WebRole1** directory, open the **app.js** file in a text editor.</span></span>
2. <span data-ttu-id="52d6f-159">Lägg till följande för att läsa in modulen för azure och ange namn och partition tabellnyckel i början av filen:</span><span class="sxs-lookup"><span data-stu-id="52d6f-159">At the beginning of the file, add the following to load the azure module and set the table name and partition key:</span></span>

    ```nodejs
    var azure = require('azure-storage');
    var tableName = 'tasks';
    var partitionKey = 'hometasks';
    ```

3. <span data-ttu-id="52d6f-160">Rulla ned till där du ser följande rad i filen app.js:</span><span class="sxs-lookup"><span data-stu-id="52d6f-160">In the app.js file, scroll down to where you see the following line:</span></span>

    ```nodejs
    app.use('/', routes);
    app.use('/users', users);
    ```

    <span data-ttu-id="52d6f-161">Ersätt de föregående raderna med följande kod.</span><span class="sxs-lookup"><span data-stu-id="52d6f-161">Replace the preceding lines with the following code.</span></span> <span data-ttu-id="52d6f-162">Den här koden initierar en instans av <strong>aktivitet</strong> med en anslutning till ditt lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="52d6f-162">This code initializes an instance of <strong>Task</strong> with a connection to your storage account.</span></span> <span data-ttu-id="52d6f-163">Den <strong>aktivitet</strong> har överförts till den <strong>TaskList</strong>, som används för att kommunicera med tjänsten tabell:</span><span class="sxs-lookup"><span data-stu-id="52d6f-163">The <strong>Task</strong> is passed to the <strong>TaskList</strong>, which uses it to communicate with the Table service:</span></span>

    ```nodejs
    var TaskList = require('./routes/tasklist');
    var Task = require('./models/task');
    var task = new Task(azure.createTableService(), tableName, partitionKey);
    var taskList = new TaskList(task);

    app.get('/', taskList.showTasks.bind(taskList));
    app.post('/addtask', taskList.addTask.bind(taskList));
    app.post('/completetask', taskList.completeTask.bind(taskList));
    ```

4. <span data-ttu-id="52d6f-164">Spara den **app.js** fil.</span><span class="sxs-lookup"><span data-stu-id="52d6f-164">Save the **app.js** file.</span></span>

### <a name="modify-the-index-view"></a><span data-ttu-id="52d6f-165">Ändra indexvyn</span><span class="sxs-lookup"><span data-stu-id="52d6f-165">Modify the index view</span></span>
1. <span data-ttu-id="52d6f-166">Ändra sökvägen till den **vyer** katalog och öppna den **index.jade** i en textredigerare.</span><span class="sxs-lookup"><span data-stu-id="52d6f-166">Change directories to the **views** directory and open the **index.jade** file in a text editor.</span></span>
2. <span data-ttu-id="52d6f-167">Ersätt innehållet i den **index.jade** med följande kod.</span><span class="sxs-lookup"><span data-stu-id="52d6f-167">Replace the contents of the **index.jade** file with the following code.</span></span> <span data-ttu-id="52d6f-168">Den här koden definierar vyn för att visa befintliga aktiviteter och definierar ett formulär för att lägga till nya aktiviteter och markera befintliga som slutförts.</span><span class="sxs-lookup"><span data-stu-id="52d6f-168">This code defines the view for displaying existing tasks, and defines a form for adding new tasks and marking existing ones as completed.</span></span>

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

3. <span data-ttu-id="52d6f-169">Spara och Stäng **index.jade** fil.</span><span class="sxs-lookup"><span data-stu-id="52d6f-169">Save and close **index.jade** file.</span></span>

### <a name="modify-the-global-layout"></a><span data-ttu-id="52d6f-170">Ändra globala layouten</span><span class="sxs-lookup"><span data-stu-id="52d6f-170">Modify the global layout</span></span>
<span data-ttu-id="52d6f-171">Filen **layout.jade** i katalogen **views** används som en global mall för andra **.jade**-filer.</span><span class="sxs-lookup"><span data-stu-id="52d6f-171">The **layout.jade** file in the **views** directory is used as a global template for other **.jade** files.</span></span> <span data-ttu-id="52d6f-172">I det här steget kan du ändra den **layout.jade** fil som ska användas [Twitter Bootstrap](https://github.com/twbs/bootstrap), vilket är en verktygslåda som gör det enkelt att utforma en snygg webbplats.</span><span class="sxs-lookup"><span data-stu-id="52d6f-172">In this step, modify the **layout.jade** file to use [Twitter Bootstrap](https://github.com/twbs/bootstrap), which is a toolkit that makes it easy to design a nice looking website.</span></span>

1. <span data-ttu-id="52d6f-173">Ladda ned och extrahera filerna för [Twitter Bootstrap](http://getbootstrap.com/).</span><span class="sxs-lookup"><span data-stu-id="52d6f-173">Download and extract the files for [Twitter Bootstrap](http://getbootstrap.com/).</span></span> <span data-ttu-id="52d6f-174">Kopiera den **bootstrap.min.css** filen från den **bootstrap\\dist\\css** mappen till den **offentliga\\matmallar** katalogen för tillämpningsprogrammet tasklist.</span><span class="sxs-lookup"><span data-stu-id="52d6f-174">Copy the **bootstrap.min.css** file from the **bootstrap\\dist\\css** folder to the **public\\stylesheets** directory of your tasklist application.</span></span>
2. <span data-ttu-id="52d6f-175">Från den **vyer** mapp, öppna den **layout.jade** filen i en textredigerare och Ersätt innehållet med följande:</span><span class="sxs-lookup"><span data-stu-id="52d6f-175">From the **views** folder, open the **layout.jade** file in your text editor and replace the contents with the following:</span></span>

    <span data-ttu-id="52d6f-176">doctype html html head rubrik = Rubriklänk (rel = 'stylesheet', href='/stylesheets/bootstrap.min.css') länk (rel = 'stylesheet', href='/stylesheets/style.css') body.app nav.navbar.navbar standard div.navbar huvud a.navbar-brand(href='/') min  Uppgifter blockera innehåll</span><span class="sxs-lookup"><span data-stu-id="52d6f-176">doctype html  html    head      title= title      link(rel='stylesheet', href='/stylesheets/bootstrap.min.css')      link(rel='stylesheet', href='/stylesheets/style.css')    body.app      nav.navbar.navbar-default        div.navbar-header          a.navbar-brand(href='/') My Tasks      block content</span></span>

3. <span data-ttu-id="52d6f-177">Spara den **layout.jade** fil.</span><span class="sxs-lookup"><span data-stu-id="52d6f-177">Save the **layout.jade** file.</span></span>

### <a name="running-the-application-in-the-emulator"></a><span data-ttu-id="52d6f-178">Kör programmet i emulatorn</span><span class="sxs-lookup"><span data-stu-id="52d6f-178">Running the Application in the Emulator</span></span>
<span data-ttu-id="52d6f-179">Använd följande kommando för att starta programmet i emulatorn.</span><span class="sxs-lookup"><span data-stu-id="52d6f-179">Use the following command to start the application in the emulator.</span></span>

```powershell
PS C:\node\tasklist\WebRole1> start-azureemulator -launch
```

<span data-ttu-id="52d6f-180">Webbläsaren öppnas och visar följande sida:</span><span class="sxs-lookup"><span data-stu-id="52d6f-180">The browser opens and displays the following page:</span></span>

![En webbplats som växlingsbart systemminne med titeln uppgiftslistan med en tabell som innehåller uppgifter och för att lägga till en ny uppgift.](./media/table-storage-cloud-service-nodejs/node44.png)

<span data-ttu-id="52d6f-182">Använd formuläret för att lägga till objekt eller ta bort befintliga objekt genom att markera dem som slutförda.</span><span class="sxs-lookup"><span data-stu-id="52d6f-182">Use the form to add items, or remove existing items by marking them as completed.</span></span>

## <a name="publishing-the-application-to-azure"></a><span data-ttu-id="52d6f-183">Publicera program till Azure</span><span class="sxs-lookup"><span data-stu-id="52d6f-183">Publishing the Application to Azure</span></span>
<span data-ttu-id="52d6f-184">Anropa följande cmdlet om du vill distribuera dina värdbaserade tjänsten till Azure i Windows PowerShell-fönstret.</span><span class="sxs-lookup"><span data-stu-id="52d6f-184">In the Windows PowerShell window, call the following cmdlet to redeploy your hosted service to Azure.</span></span>

```powershell
PS C:\node\tasklist\WebRole1> Publish-AzureServiceProject -name myuniquename -location datacentername -launch
```

<span data-ttu-id="52d6f-185">Ersätt **myuniquename** med ett unikt namn för det här programmet.</span><span class="sxs-lookup"><span data-stu-id="52d6f-185">Replace **myuniquename** with a unique name for this application.</span></span> <span data-ttu-id="52d6f-186">Ersätt **datacentername** med namnet på ett Azure-datacenter som **västra USA**.</span><span class="sxs-lookup"><span data-stu-id="52d6f-186">Replace **datacentername** with the name of an Azure data center, such as **West US**.</span></span>

<span data-ttu-id="52d6f-187">När distributionen är klar bör du se ett svar som liknar följande:</span><span class="sxs-lookup"><span data-stu-id="52d6f-187">After the deployment is complete, you should see a response similar to the following:</span></span>

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

<span data-ttu-id="52d6f-188">Genom att ange den **-starta** alternativ i föregående cmdlet webbläsaren öppnas och visar programmet körs i Azure när publiceringen är klar.</span><span class="sxs-lookup"><span data-stu-id="52d6f-188">By specifying the **-launch** option in the previous cmdlet, the browser opens and displays your application running in Azure when publishing is completed.</span></span>

![Ett webbläsarfönster som visar sidan uppgiftslistan.](./media/table-storage-cloud-service-nodejs/getting-started-1.png)

## <a name="stopping-and-deleting-your-application"></a><span data-ttu-id="52d6f-191">Stoppa och ta bort programmet</span><span class="sxs-lookup"><span data-stu-id="52d6f-191">Stopping and Deleting Your Application</span></span>
<span data-ttu-id="52d6f-192">När du distribuerar ditt program, kan du vill inaktivera den så att du kan undvika kostnader eller skapa och distribuera andra program inom den kostnadsfria utvärderingsversionen tidsperioden.</span><span class="sxs-lookup"><span data-stu-id="52d6f-192">After deploying your application, you may want to disable it so you can avoid costs or build and deploy other applications within the free trial time period.</span></span>

<span data-ttu-id="52d6f-193">Azure fakturerar webbrollsinstanser per timme förbrukad servertid.</span><span class="sxs-lookup"><span data-stu-id="52d6f-193">Azure bills web role instances per hour of server time consumed.</span></span>
<span data-ttu-id="52d6f-194">Servertid förbrukas när programmet har distribuerats, även om instanserna inte körs och är i stoppat tillstånd.</span><span class="sxs-lookup"><span data-stu-id="52d6f-194">Server time is consumed once your application is deployed, even if the instances are not running and are in the stopped state.</span></span>

<span data-ttu-id="52d6f-195">Följande steg visar hur du stoppar och ta bort programmet.</span><span class="sxs-lookup"><span data-stu-id="52d6f-195">The following steps show you how to stop and delete your application.</span></span>

1. <span data-ttu-id="52d6f-196">Stoppa tjänstdistributionen som skapades i föregående avsnitt med följande cmdlet i Windows PowerShell-fönstret:</span><span class="sxs-lookup"><span data-stu-id="52d6f-196">In the Windows PowerShell window, stop the service deployment created in the previous section with the following cmdlet:</span></span>

    ```powershell
    PS C:\node\tasklist\WebRole1> Stop-AzureService
    ```

   <span data-ttu-id="52d6f-197">Det kan ta flera minuter att stoppa tjänsten.</span><span class="sxs-lookup"><span data-stu-id="52d6f-197">Stopping the service may take several minutes.</span></span> <span data-ttu-id="52d6f-198">När tjänsten har stoppats får du ett meddelande som anger att den har stoppats.</span><span class="sxs-lookup"><span data-stu-id="52d6f-198">When the service is stopped, you receive a message indicating that it has stopped.</span></span>

2. <span data-ttu-id="52d6f-199">Ta bort tjänsten genom att anropa följande cmdlet:</span><span class="sxs-lookup"><span data-stu-id="52d6f-199">To delete the service, call the following cmdlet:</span></span>

    ```powershell
    PS C:\node\tasklist\WebRole1> Remove-AzureService contosotasklist
    ```

   <span data-ttu-id="52d6f-200">När du uppmanas, anger du **Y** för att ta bort tjänsten.</span><span class="sxs-lookup"><span data-stu-id="52d6f-200">When prompted, enter **Y** to delete the service.</span></span>

   <span data-ttu-id="52d6f-201">Det kan ta flera minuter att ta bort tjänsten.</span><span class="sxs-lookup"><span data-stu-id="52d6f-201">Deleting the service may take several minutes.</span></span> <span data-ttu-id="52d6f-202">När tjänsten har tagits bort, visas ett meddelande som anger att tjänsten har tagits bort.</span><span class="sxs-lookup"><span data-stu-id="52d6f-202">After the service is deleted, you will receive a message indicating that the service was deleted.</span></span>

[Node.js-Webbapp med hjälp av]: http://azure.microsoft.com/develop/nodejs/tutorials/web-app-with-express/
[lagring och åtkomst till Data i Azure]: http://msdn.microsoft.com/library/azure/gg433040.aspx
[Node.js-Webbapp]: http://azure.microsoft.com/develop/nodejs/tutorials/getting-started/


