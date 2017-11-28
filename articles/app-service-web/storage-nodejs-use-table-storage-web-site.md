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
# <a name="nodejs-web-app-using-hello-azure-table-service"></a><span data-ttu-id="173cf-103">Node.js-webbapp med hjälp av hello Azure Table-tjänsten</span><span class="sxs-lookup"><span data-stu-id="173cf-103">Node.js web app using hello Azure Table Service</span></span>
## <a name="overview"></a><span data-ttu-id="173cf-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="173cf-104">Overview</span></span>
<span data-ttu-id="173cf-105">Den här kursen visar hur toouse tabelltjänsten som tillhandahålls av Azure Data Management toostore och komma åt data från en [nod] program finns i [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) Web Apps.</span><span class="sxs-lookup"><span data-stu-id="173cf-105">This tutorial shows you how toouse Table service provided by Azure Data Management toostore and access data from a [node] application hosted in [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) Web Apps.</span></span> <span data-ttu-id="173cf-106">Den här kursen förutsätter att du har tidigare erfarenhet av noden och [Git].</span><span class="sxs-lookup"><span data-stu-id="173cf-106">This tutorial assumes that you have some prior experience using node and [Git].</span></span>

<span data-ttu-id="173cf-107">Du kommer att lära dig:</span><span class="sxs-lookup"><span data-stu-id="173cf-107">You will learn:</span></span>

* <span data-ttu-id="173cf-108">Hur toouse npm (noden package manager) tooinstall hello nod moduler</span><span class="sxs-lookup"><span data-stu-id="173cf-108">How toouse npm (node package manager) tooinstall hello node modules</span></span>
* <span data-ttu-id="173cf-109">Hur toowork med hello Azure tabelltjänst</span><span class="sxs-lookup"><span data-stu-id="173cf-109">How toowork with hello Azure Table service</span></span>
* <span data-ttu-id="173cf-110">Hur toouse hello Azure CLI toocreate ett webbprogram.</span><span class="sxs-lookup"><span data-stu-id="173cf-110">How toouse hello Azure CLI toocreate a web app.</span></span>

<span data-ttu-id="173cf-111">Genom att följa den här självstudiekursen skapar du en enkel webbaserad ”uppgiftslistan” program som kan skapa, hämta och uppgifter som slutförs.</span><span class="sxs-lookup"><span data-stu-id="173cf-111">By following this tutorial, you will build a simple web-based "to-do list" application that allows creating, retrieving and completing tasks.</span></span> <span data-ttu-id="173cf-112">hello uppgifter lagras i hello tabelltjänsten.</span><span class="sxs-lookup"><span data-stu-id="173cf-112">hello tasks are stored in hello Table service.</span></span>

<span data-ttu-id="173cf-113">Här är hello slutförts program:</span><span class="sxs-lookup"><span data-stu-id="173cf-113">Here is hello completed application:</span></span>

![En webbsida med en tom tasklist][node-table-finished]

> [!NOTE]
> <span data-ttu-id="173cf-115">Om du vill tooget igång med Azure App Service innan du registrerar dig för ett Azure-konto går för[prova App Service](https://azure.microsoft.com/try/app-service/), där kan du direkt skapa en tillfällig startwebbapp i App Service.</span><span class="sxs-lookup"><span data-stu-id="173cf-115">If you want tooget started with Azure App Service before signing up for an Azure account, go too[Try App Service](https://azure.microsoft.com/try/app-service/), where you can immediately create a short-lived starter web app in App Service.</span></span> <span data-ttu-id="173cf-116">Inget kreditkort krävs, och du gör inga åtaganden.</span><span class="sxs-lookup"><span data-stu-id="173cf-116">No credit cards required; no commitments.</span></span>
> 
> 

## <a name="prerequisites"></a><span data-ttu-id="173cf-117">Krav</span><span class="sxs-lookup"><span data-stu-id="173cf-117">Prerequisites</span></span>
<span data-ttu-id="173cf-118">Innan du följer hello anvisningarna i den här artikeln bör du kontrollera att du har hello följande installerat:</span><span class="sxs-lookup"><span data-stu-id="173cf-118">Before following hello instructions in this article, ensure that you have hello following installed:</span></span>

* <span data-ttu-id="173cf-119">[nod] version 0.10.24 eller högre</span><span class="sxs-lookup"><span data-stu-id="173cf-119">[node] version 0.10.24 or higher</span></span>
* <span data-ttu-id="173cf-120">[Git]</span><span class="sxs-lookup"><span data-stu-id="173cf-120">[Git]</span></span>

[!INCLUDE [create-account-and-websites-note](../../includes/create-account-and-websites-note.md)]

## <a name="create-a-storage-account"></a><span data-ttu-id="173cf-121">skapar ett lagringskonto</span><span class="sxs-lookup"><span data-stu-id="173cf-121">Create a storage account</span></span>
<span data-ttu-id="173cf-122">Skapa ett Azure-lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="173cf-122">Create an Azure storage account.</span></span> <span data-ttu-id="173cf-123">hello appen kommer att använda det här kontot toostore hello arbetsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="173cf-123">hello app will use this account toostore hello to-do items.</span></span>

1. <span data-ttu-id="173cf-124">Logga in på hello [Azure Portal](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="173cf-124">Log into hello [Azure Portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="173cf-125">Klicka på hello **ny** ikon på hello längst ned till vänster i hello portal och klicka sedan på **Data + lagring** > **lagring**.</span><span class="sxs-lookup"><span data-stu-id="173cf-125">Click hello **New** icon on hello bottom left of hello portal, then click **Data + Storage** > **Storage**.</span></span> <span data-ttu-id="173cf-126">Ge hello storage-konto ett unikt namn och skapa en ny [resursgruppen](../azure-resource-manager/resource-group-overview.md) för den.</span><span class="sxs-lookup"><span data-stu-id="173cf-126">Give hello storage account a unique name and create a new [resource group](../azure-resource-manager/resource-group-overview.md) for it.</span></span>
   
      ![Knappen Nytt](./media/storage-nodejs-use-table-storage-web-site/configure-storage.png)
   
    <span data-ttu-id="173cf-128">När hello storage-konto har skapats, hello **meddelanden** knappen blinkar en grön **lyckade** och hello lagringskontots blad är öppet tooshow det hör toohello ny resurs gruppen du Skapa.</span><span class="sxs-lookup"><span data-stu-id="173cf-128">When hello storage account has been created, hello **Notifications** button will flash a green **SUCCESS** and hello storage account's blade is open tooshow that it belongs toohello new resource group you created.</span></span>
3. <span data-ttu-id="173cf-129">I bladet hello lagringskonto klickar du på **inställningar** > **nycklar**.</span><span class="sxs-lookup"><span data-stu-id="173cf-129">In hello storage account's blade, click **Settings** > **Keys**.</span></span> <span data-ttu-id="173cf-130">Kopiera hello primära nyckel toohello Urklipp.</span><span class="sxs-lookup"><span data-stu-id="173cf-130">Copy hello primary access key toohello clipboard.</span></span>
   
    ![Snabbtangent][portal-storage-access-keys]

## <a name="install-modules-and-generate-scaffolding"></a><span data-ttu-id="173cf-132">Installera moduler och generera scaffold-teknik</span><span class="sxs-lookup"><span data-stu-id="173cf-132">Install modules and generate scaffolding</span></span>
<span data-ttu-id="173cf-133">I det här avsnittet ska du skapa ett nytt program för nod och använder npm tooadd modulen paket.</span><span class="sxs-lookup"><span data-stu-id="173cf-133">In this section you will create a new Node application and use npm tooadd module packages.</span></span> <span data-ttu-id="173cf-134">För det här programmet använder du hello [Express] och [Azure] moduler.</span><span class="sxs-lookup"><span data-stu-id="173cf-134">For this application you will use hello [Express] and [Azure] modules.</span></span> <span data-ttu-id="173cf-135">hello Express modulen innehåller ett Model View Controller ramverk för noden, när hello Azure moduler ger anslutningsbarhet toohello tabelltjänsten.</span><span class="sxs-lookup"><span data-stu-id="173cf-135">hello Express module provides a Model View Controller framework for node, while hello Azure modules provides connectivity toohello Table service.</span></span>

### <a name="install-express-and-generate-scaffolding"></a><span data-ttu-id="173cf-136">Installera express och generera scaffold-teknik</span><span class="sxs-lookup"><span data-stu-id="173cf-136">Install express and generate scaffolding</span></span>
1. <span data-ttu-id="173cf-137">Skapa en ny katalog med namnet från kommandoraden hello **tasklist** och växel toothat directory.</span><span class="sxs-lookup"><span data-stu-id="173cf-137">From hello command line, create a new directory named **tasklist** and switch toothat directory.</span></span>  
2. <span data-ttu-id="173cf-138">Ange hello efter kommandot tooinstall hello Express modulen.</span><span class="sxs-lookup"><span data-stu-id="173cf-138">Enter hello following command tooinstall hello Express module.</span></span>
   
        npm install express-generator@4.2.0 -g
   
    <span data-ttu-id="173cf-139">Hello operativsystem måste du använda tooput 'sudo-innan hello-kommando:</span><span class="sxs-lookup"><span data-stu-id="173cf-139">Depending on hello operating system, you may need tooput 'sudo' before hello command:</span></span>
   
        sudo npm install express-generator@4.2.0 -g
   
    <span data-ttu-id="173cf-140">hello utdata visas liknande toohello följande exempel:</span><span class="sxs-lookup"><span data-stu-id="173cf-140">hello output appears similar toohello following example:</span></span>
   
        express-generator@4.2.0 /usr/local/lib/node_modules/express-generator
        ├── mkdirp@0.3.5
        └── commander@1.3.2 (keypress@0.1.0)
   
   > [!NOTE]
   > <span data-ttu-id="173cf-141">Hej ”-g-parametern installerar hello modulen globalt.</span><span class="sxs-lookup"><span data-stu-id="173cf-141">hello '-g' parameter installs hello module globally.</span></span> <span data-ttu-id="173cf-142">På så sätt kan vi använda **express** toogenerate web app scaffold-teknik utan tootype i ytterligare information om sökvägen.</span><span class="sxs-lookup"><span data-stu-id="173cf-142">That way, we can use **express** toogenerate web app scaffolding without having tootype in additional path information.</span></span>
   > 
   > 
3. <span data-ttu-id="173cf-143">toocreate hello scaffold-teknik för hello program ange hello **express** kommando:</span><span class="sxs-lookup"><span data-stu-id="173cf-143">toocreate hello scaffolding for hello application, enter hello **express** command:</span></span>
   
        express
   
    <span data-ttu-id="173cf-144">hello utdata från kommandot visas liknande toohello följande exempel:</span><span class="sxs-lookup"><span data-stu-id="173cf-144">hello output of this command appears similar toohello following example:</span></span>
   
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
   
    <span data-ttu-id="173cf-145">Nu har du flera nya kataloger och filer i hello **tasklist** directory.</span><span class="sxs-lookup"><span data-stu-id="173cf-145">You now have several new directories and files in hello **tasklist** directory.</span></span>

### <a name="install-additional-modules"></a><span data-ttu-id="173cf-146">Installera ytterligare moduler</span><span class="sxs-lookup"><span data-stu-id="173cf-146">Install additional modules</span></span>
<span data-ttu-id="173cf-147">En av hello filer som **express** skapar är **package.json**.</span><span class="sxs-lookup"><span data-stu-id="173cf-147">One of hello files that **express** creates is **package.json**.</span></span> <span data-ttu-id="173cf-148">Den här filen innehåller en lista över beroenden för modulen.</span><span class="sxs-lookup"><span data-stu-id="173cf-148">This file contains a list of module dependencies.</span></span> <span data-ttu-id="173cf-149">Senare, när du distribuerar hello programmet tooApp Service Web Apps kan den här filen avgör vilka moduler måste toobe installerad på Azure.</span><span class="sxs-lookup"><span data-stu-id="173cf-149">Later, when you deploy hello application tooApp Service Web Apps, this file determines which modules need toobe installed on Azure.</span></span>

<span data-ttu-id="173cf-150">Ange följande kommando tooinstall hello moduler som beskrivs i hello hello från kommandoraden hello, **package.json** fil.</span><span class="sxs-lookup"><span data-stu-id="173cf-150">From hello command-line, enter hello following command tooinstall hello modules described in hello **package.json** file.</span></span> <span data-ttu-id="173cf-151">Du kan behöva toouse ”sudo”.</span><span class="sxs-lookup"><span data-stu-id="173cf-151">You may need toouse 'sudo'.</span></span>

    npm install

<span data-ttu-id="173cf-152">hello utdata från kommandot visas liknande toohello följande exempel:</span><span class="sxs-lookup"><span data-stu-id="173cf-152">hello output of this command appears similar toohello following example:</span></span>

    debug@0.7.4 node_modules\debug

    cookie-parser@1.0.1 node_modules\cookie-parser
    ├── cookie-signature@1.0.3
    └── cookie@0.1.0

    [...]


<span data-ttu-id="173cf-153">Skriv följande kommando tooinstall hello hello [azure], [nod uuid], [nconf] och [asynkrona] moduler:</span><span class="sxs-lookup"><span data-stu-id="173cf-153">Next, enter hello following command tooinstall hello [azure], [node-uuid], [nconf] and [async] modules:</span></span>

    npm install azure-storage node-uuid async nconf --save

<span data-ttu-id="173cf-154">Hej **--spara** flaggan lägger till poster för dessa moduler toohello **package.json** fil.</span><span class="sxs-lookup"><span data-stu-id="173cf-154">hello **--save** flag adds entries for these modules toohello **package.json** file.</span></span>

<span data-ttu-id="173cf-155">hello utdata från kommandot visas liknande toohello följande exempel:</span><span class="sxs-lookup"><span data-stu-id="173cf-155">hello output of this command appears similar toohello following example:</span></span>

    async@0.9.0 node_modules\async

    node-uuid@1.4.1 node_modules\node-uuid

    nconf@0.6.9 node_modules\nconf
    ├── ini@1.2.1
    ├── async@0.2.9
    └── optimist@0.6.0 (wordwrap@0.0.2, minimist@0.0.10)

    [...]


## <a name="create-hello-application"></a><span data-ttu-id="173cf-156">Skapa hello program</span><span class="sxs-lookup"><span data-stu-id="173cf-156">Create hello application</span></span>
<span data-ttu-id="173cf-157">Vi är nu redo toobuild hello program.</span><span class="sxs-lookup"><span data-stu-id="173cf-157">Now we're ready toobuild hello application.</span></span>

### <a name="create-a-model"></a><span data-ttu-id="173cf-158">Skapa en modell</span><span class="sxs-lookup"><span data-stu-id="173cf-158">Create a model</span></span>
<span data-ttu-id="173cf-159">En *modellen* är ett objekt som representerar hello data i ditt program.</span><span class="sxs-lookup"><span data-stu-id="173cf-159">A *model* is an object that represents hello data in your application.</span></span> <span data-ttu-id="173cf-160">För hello programmet är hello endast modellen ett task-objekt som representerar ett objekt i hello att göra-lista.</span><span class="sxs-lookup"><span data-stu-id="173cf-160">For hello application, hello only model is a task object, which represents an item in hello to-do list.</span></span> <span data-ttu-id="173cf-161">Aktiviteter har hello följande fält:</span><span class="sxs-lookup"><span data-stu-id="173cf-161">Tasks will have hello following fields:</span></span>

* <span data-ttu-id="173cf-162">PartitionKey</span><span class="sxs-lookup"><span data-stu-id="173cf-162">PartitionKey</span></span>
* <span data-ttu-id="173cf-163">RowKey</span><span class="sxs-lookup"><span data-stu-id="173cf-163">RowKey</span></span>
* <span data-ttu-id="173cf-164">namnet (sträng)</span><span class="sxs-lookup"><span data-stu-id="173cf-164">name (string)</span></span>
* <span data-ttu-id="173cf-165">kategori (sträng)</span><span class="sxs-lookup"><span data-stu-id="173cf-165">category (string)</span></span>
* <span data-ttu-id="173cf-166">slutförda (Boolean)</span><span class="sxs-lookup"><span data-stu-id="173cf-166">completed (Boolean)</span></span>

<span data-ttu-id="173cf-167">**PartitionKey** och **RowKey** används hello Tabelltjänsten som registernycklar.</span><span class="sxs-lookup"><span data-stu-id="173cf-167">**PartitionKey** and **RowKey** are used by hello Table Service as table keys.</span></span> <span data-ttu-id="173cf-168">Mer information finns i [förstå hello tabelltjänst-datamodellen](https://msdn.microsoft.com/library/azure/dd179338.aspx).</span><span class="sxs-lookup"><span data-stu-id="173cf-168">For more information, see [Understanding hello Table Service data model](https://msdn.microsoft.com/library/azure/dd179338.aspx).</span></span>

1. <span data-ttu-id="173cf-169">I hello **tasklist** directory, skapa en ny katalog med namnet **modeller**.</span><span class="sxs-lookup"><span data-stu-id="173cf-169">In hello **tasklist** directory, create a new directory named **models**.</span></span>
2. <span data-ttu-id="173cf-170">I hello **modeller** directory, skapa en ny fil med namnet **task.js**.</span><span class="sxs-lookup"><span data-stu-id="173cf-170">In hello **models** directory, create a new file named **task.js**.</span></span> <span data-ttu-id="173cf-171">Den här filen innehåller hello modellen för hello aktiviteter som skapats av programmet.</span><span class="sxs-lookup"><span data-stu-id="173cf-171">This file will contain hello model for hello tasks created by your application.</span></span>
3. <span data-ttu-id="173cf-172">Hello början av hello **task.js** lägger du till följande kod tooreference krävs bibliotek hello:</span><span class="sxs-lookup"><span data-stu-id="173cf-172">At hello beginning of hello **task.js** file, add hello following code tooreference required libraries:</span></span>
   
        var azure = require('azure-storage');
          var uuid = require('node-uuid');
        var entityGen = azure.TableUtilities.entityGenerator;
4. <span data-ttu-id="173cf-173">Lägg till följande hello code toodefine och exportera hello Task-objekt.</span><span class="sxs-lookup"><span data-stu-id="173cf-173">Add hello following code toodefine and export hello Task object.</span></span> <span data-ttu-id="173cf-174">Det här objektet är ansvarig för att ansluta toohello tabell.</span><span class="sxs-lookup"><span data-stu-id="173cf-174">This object is responsible for connecting toohello table.</span></span>
   
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
5. <span data-ttu-id="173cf-175">Lägg till hello följande kod toodefine fler metoder på hello aktivitetsobjektet som tillåter samverkan med data som lagras i tabellen hello:</span><span class="sxs-lookup"><span data-stu-id="173cf-175">Add hello following code toodefine additional methods on hello Task object, which allow interactions with data stored in hello table:</span></span>
   
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
6. <span data-ttu-id="173cf-176">Spara och Stäng hello **task.js** fil.</span><span class="sxs-lookup"><span data-stu-id="173cf-176">Save and close hello **task.js** file.</span></span>

### <a name="create-a-controller"></a><span data-ttu-id="173cf-177">Skapa en domänkontrollant</span><span class="sxs-lookup"><span data-stu-id="173cf-177">Create a controller</span></span>
<span data-ttu-id="173cf-178">En *domänkontrollant* hanterar HTTP-begäranden och återger hello HTML-svaret.</span><span class="sxs-lookup"><span data-stu-id="173cf-178">A *controller* handles HTTP requests and renders hello HTML response.</span></span>

1. <span data-ttu-id="173cf-179">I hello **tasklist/vägar** directory, skapa en ny fil med namnet **tasklist.js** och öppna den i en textredigerare.</span><span class="sxs-lookup"><span data-stu-id="173cf-179">In hello **tasklist/routes** directory, create a new file named **tasklist.js** and open it in a text editor.</span></span>
2. <span data-ttu-id="173cf-180">Lägg till följande kod för hello**tasklist.js**.</span><span class="sxs-lookup"><span data-stu-id="173cf-180">Add hello following code too**tasklist.js**.</span></span> <span data-ttu-id="173cf-181">Den läser in hello azure och async-moduler som används av **tasklist.js**.</span><span class="sxs-lookup"><span data-stu-id="173cf-181">This loads hello azure and async modules, which are used by **tasklist.js**.</span></span> <span data-ttu-id="173cf-182">Detta definierar också hello **TaskList** funktion, som mottar en instans av hello **aktivitet** objektet som vi definierade tidigare:</span><span class="sxs-lookup"><span data-stu-id="173cf-182">This also defines hello **TaskList** function, which is passed an instance of hello **Task** object we defined earlier:</span></span>
   
        var azure = require('azure-storage');
        var async = require('async');
   
        module.exports = TaskList;
3. <span data-ttu-id="173cf-183">Definiera en **TaskList** objekt.</span><span class="sxs-lookup"><span data-stu-id="173cf-183">Define a **TaskList** object.</span></span>
   
        function TaskList(task) {
          this.task = task;
        }
4. <span data-ttu-id="173cf-184">Lägg till följande metoder för hello**TaskList**:</span><span class="sxs-lookup"><span data-stu-id="173cf-184">Add hello following methods too**TaskList**:</span></span>
   
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

### <a name="modify-appjs"></a><span data-ttu-id="173cf-185">Ändra app.js</span><span class="sxs-lookup"><span data-stu-id="173cf-185">Modify app.js</span></span>
1. <span data-ttu-id="173cf-186">Från hello **tasklist** katalog, öppna hello **app.js** fil.</span><span class="sxs-lookup"><span data-stu-id="173cf-186">From hello **tasklist** directory, open hello **app.js** file.</span></span> <span data-ttu-id="173cf-187">Den här filen skapades tidigare genom att köra hello **express** kommando.</span><span class="sxs-lookup"><span data-stu-id="173cf-187">This file was created earlier by running hello **express** command.</span></span>
2. <span data-ttu-id="173cf-188">Hello början av hello-fil, lägger du till hello följande tooload hello azure modul, set hello tabellnamn, partitionsnyckel och ange hello lagring autentiseringsuppgifter som används av det här exemplet:</span><span class="sxs-lookup"><span data-stu-id="173cf-188">At hello beginning of hello file, add hello following tooload hello azure module, set hello table name, partition key, and set hello storage credentials used by this example:</span></span>
   
        var azure = require('azure-storage');
        var nconf = require('nconf');
        nconf.env()
             .file({ file: 'config.json', search: true });
        var tableName = nconf.get("TABLE_NAME");
        var partitionKey = nconf.get("PARTITION_KEY");
        var accountName = nconf.get("STORAGE_NAME");
        var accountKey = nconf.get("STORAGE_KEY");
   
   > [!NOTE]
   > <span data-ttu-id="173cf-189">nconf läses hello konfigurationsvärden från miljövariabler eller hello **config.json** fil som skapas senare.</span><span class="sxs-lookup"><span data-stu-id="173cf-189">nconf will load hello configuration values from either environment variables or hello **config.json** file, which we will create later.</span></span>
   > 
   > 
3. <span data-ttu-id="173cf-190">Rulla ned toowhere som du ser i filen app.js hello hello följande rad:</span><span class="sxs-lookup"><span data-stu-id="173cf-190">In hello app.js file, scroll down toowhere you see hello following line:</span></span>
   
        app.use('/', routes);
        app.use('/users', users);
   
    <span data-ttu-id="173cf-191">Ersätt hello ovan rader med hello koden nedan.</span><span class="sxs-lookup"><span data-stu-id="173cf-191">Replace hello above lines with hello code shown below.</span></span> <span data-ttu-id="173cf-192">Detta ska initiera en instans av <strong>aktivitet</strong> med en anslutning tooyour storage-konto.</span><span class="sxs-lookup"><span data-stu-id="173cf-192">This will initialize an instance of <strong>Task</strong> with a connection tooyour storage account.</span></span> <span data-ttu-id="173cf-193">Detta skickas toohello <strong>TaskList</strong>, som kommer att använda den toocommunicate med hello tabelltjänsten:</span><span class="sxs-lookup"><span data-stu-id="173cf-193">This is passed toohello <strong>TaskList</strong>, which will use it toocommunicate with hello Table service:</span></span>
   
        var TaskList = require('./routes/tasklist');
        var Task = require('./models/task');
        var task = new Task(azure.createTableService(accountName, accountKey), tableName, partitionKey);
        var taskList = new TaskList(task);
   
        app.get('/', taskList.showTasks.bind(taskList));
        app.post('/addtask', taskList.addTask.bind(taskList));
        app.post('/completetask', taskList.completeTask.bind(taskList));
4. <span data-ttu-id="173cf-194">Spara hello **app.js** fil.</span><span class="sxs-lookup"><span data-stu-id="173cf-194">Save hello **app.js** file.</span></span>

### <a name="modify-hello-index-view"></a><span data-ttu-id="173cf-195">Ändra hello indexvy</span><span class="sxs-lookup"><span data-stu-id="173cf-195">Modify hello index view</span></span>
1. <span data-ttu-id="173cf-196">Öppna hello **tasklist/views/index.jade** i en textredigerare.</span><span class="sxs-lookup"><span data-stu-id="173cf-196">Open hello **tasklist/views/index.jade** file in a text editor.</span></span>
2. <span data-ttu-id="173cf-197">Ersätt hello hela innehållet i filen hello med hello följande kod.</span><span class="sxs-lookup"><span data-stu-id="173cf-197">Replace hello entire contents of hello file with hello following code.</span></span> <span data-ttu-id="173cf-198">Detta definierar en vy som visar befintliga aktiviteter och innehåller ett formulär för att lägga till nya aktiviteter och markera befintliga som slutförts.</span><span class="sxs-lookup"><span data-stu-id="173cf-198">This defines a view that displays existing tasks and includes a form for adding new tasks and marking existing ones as completed.</span></span>
   
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
3. <span data-ttu-id="173cf-199">Spara och Stäng **index.jade** fil.</span><span class="sxs-lookup"><span data-stu-id="173cf-199">Save and close **index.jade** file.</span></span>

### <a name="modify-hello-global-layout"></a><span data-ttu-id="173cf-200">Ändra hello globala layout</span><span class="sxs-lookup"><span data-stu-id="173cf-200">Modify hello global layout</span></span>
<span data-ttu-id="173cf-201">Hej **layout.jade** filen i hello **vyer** katalogen är en global mall för andra **.jade** filer.</span><span class="sxs-lookup"><span data-stu-id="173cf-201">hello **layout.jade** file in hello **views** directory is a global template for other **.jade** files.</span></span> <span data-ttu-id="173cf-202">I det här steget ändrar du den toouse [Twitter Bootstrap](https://github.com/twbs/bootstrap), vilket är en verktygslåda som gör det enkelt toodesign ett bra söker webbprogram.</span><span class="sxs-lookup"><span data-stu-id="173cf-202">In this step you will modify it toouse [Twitter Bootstrap](https://github.com/twbs/bootstrap), which is a toolkit that makes it easy toodesign a nice looking web app.</span></span>

<span data-ttu-id="173cf-203">Ladda ned och extrahera hello filer för [Twitter Bootstrap](http://getbootstrap.com/).</span><span class="sxs-lookup"><span data-stu-id="173cf-203">Download and extract hello files for [Twitter Bootstrap](http://getbootstrap.com/).</span></span> <span data-ttu-id="173cf-204">Kopiera hello **bootstrap.min.css** filen från hello Bootstrap **css** till hello mappen **offentlig/matmallar** katalog för programmet.</span><span class="sxs-lookup"><span data-stu-id="173cf-204">Copy hello **bootstrap.min.css** file from hello Bootstrap **css** folder into hello **public/stylesheets** directory of your application.</span></span>

<span data-ttu-id="173cf-205">Från hello **vyer** mappen öppnar **layout.jade** och Ersätt hello hela innehållet med hello följande:</span><span class="sxs-lookup"><span data-stu-id="173cf-205">From hello **views** folder, open **layout.jade** and replace hello entire contents with hello following:</span></span>

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

### <a name="create-a-config-file"></a><span data-ttu-id="173cf-206">Skapa en konfigurationsfil</span><span class="sxs-lookup"><span data-stu-id="173cf-206">Create a config file</span></span>
<span data-ttu-id="173cf-207">toorun hello appen lokalt vi ska lägga till Azure Storage-autentiseringsuppgifter i en konfigurationsfil.</span><span class="sxs-lookup"><span data-stu-id="173cf-207">toorun hello app locally, we'll put Azure Storage credentials into a config file.</span></span> <span data-ttu-id="173cf-208">Skapa en fil med namnet **config.json* * med hello följande JSON:</span><span class="sxs-lookup"><span data-stu-id="173cf-208">Create a file named **config.json* *with hello following JSON:</span></span>

    {
        "STORAGE_NAME": "<storage account name>",
        "STORAGE_KEY": "<storage access key>",
        "PARTITION_KEY": "mytasks",
        "TABLE_NAME": "tasks"
    }

<span data-ttu-id="173cf-209">Ersätt **lagringskontonamnet** med hello namnet hello storage-kontot du skapade tidigare och ersätter **lagringsåtkomstnyckel** med hello primärnyckeln för ditt lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="173cf-209">Replace **storage account name** with hello name of hello storage account you created earlier, and replace **storage access key** with hello primary access key for your storage account.</span></span> <span data-ttu-id="173cf-210">Exempel:</span><span class="sxs-lookup"><span data-stu-id="173cf-210">For example:</span></span>

    {
        "STORAGE_NAME": "nodejsappstorage",
        "STORAGE_KEY": "KG0oDd..."
        "PARTITION_KEY": "mytasks",
        "TABLE_NAME": "tasks"
    }

<span data-ttu-id="173cf-211">Spara filen *en katalog högre nivå* än hello **tasklist** katalog så här:</span><span class="sxs-lookup"><span data-stu-id="173cf-211">Save this file *one directory level higher* than hello **tasklist** directory, like this:</span></span>

    parent/
      |-- config.json
      |-- tasklist/

<span data-ttu-id="173cf-212">hello beror för att göra detta på tooavoid kontrollerar hello-konfigurationsfilen i källkontroll där den kan bli offentliga.</span><span class="sxs-lookup"><span data-stu-id="173cf-212">hello reason for doing this is tooavoid checking hello config file into source control, where it might become public.</span></span> <span data-ttu-id="173cf-213">När vi distribuerar hello app tooAzure ska vi använda miljövariabler i stället för en .config-fil.</span><span class="sxs-lookup"><span data-stu-id="173cf-213">When we deploy hello app tooAzure, we will use environment variables instead of a config file.</span></span>

## <a name="run-hello-application-locally"></a><span data-ttu-id="173cf-214">Kör hello programmet lokalt</span><span class="sxs-lookup"><span data-stu-id="173cf-214">Run hello application locally</span></span>
<span data-ttu-id="173cf-215">tootest hello program på den lokala datorn, utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="173cf-215">tootest hello application on your local machine, perform hello following steps:</span></span>

1. <span data-ttu-id="173cf-216">Från kommandoraden hello, ändra kataloger toohello **tasklist** directory.</span><span class="sxs-lookup"><span data-stu-id="173cf-216">From hello command-line, change directories toohello **tasklist** directory.</span></span>
2. <span data-ttu-id="173cf-217">Använd följande kommando toolaunch hello programmet lokalt hello:</span><span class="sxs-lookup"><span data-stu-id="173cf-217">Use hello following command toolaunch hello application locally:</span></span>
   
        npm start
3. <span data-ttu-id="173cf-218">Öppna en webbläsare och gå toohttp://127.0.0.1:3000.</span><span class="sxs-lookup"><span data-stu-id="173cf-218">Open a web browser and navigate toohttp://127.0.0.1:3000.</span></span>
   
    <span data-ttu-id="173cf-219">En webbsida liknande toohello följande exempel visas.</span><span class="sxs-lookup"><span data-stu-id="173cf-219">A web page similar toohello following example appears.</span></span>
   
    ![En webbsida visas en tom tasklist][node-table-finished]
4. <span data-ttu-id="173cf-221">toocreate ett nytt att göra-objekt, ange ett namn och kategori och klicka på **Lägg till objekt**.</span><span class="sxs-lookup"><span data-stu-id="173cf-221">toocreate a new to-do item, enter a name and category and click **Add Item**.</span></span> 
5. <span data-ttu-id="173cf-222">toomark en aktivitet som slutförd, kontrollera **Slutför** och på **Uppdateringsuppgifter**.</span><span class="sxs-lookup"><span data-stu-id="173cf-222">toomark a task as complete, check **Complete** and click **Update Tasks**.</span></span>
   
    ![En avbildning av hello nytt objekt i listan hello uppgifter][node-table-list-items]

<span data-ttu-id="173cf-224">Även om hello programmet körs lokalt, lagras hello data i hello Azure Table-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="173cf-224">Even though hello application is running locally, it is storing hello data in hello Azure Table service.</span></span>

## <a name="deploy-your-application-tooazure"></a><span data-ttu-id="173cf-225">Distribuera programmet-tooAzure</span><span class="sxs-lookup"><span data-stu-id="173cf-225">Deploy your application tooAzure</span></span>
<span data-ttu-id="173cf-226">hello stegen i det här avsnittet använder hello Azure kommandoradsverktyg toocreate en ny webbapp i App Service och sedan använda Git toodeploy ditt program.</span><span class="sxs-lookup"><span data-stu-id="173cf-226">hello steps in this section use hello Azure command-line tools toocreate a new web app in App Service, and then use Git toodeploy your application.</span></span> <span data-ttu-id="173cf-227">tooperform dessa steg som du måste ha en Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="173cf-227">tooperform these steps you must have an Azure subscription.</span></span>

> [!NOTE]
> <span data-ttu-id="173cf-228">De här stegen kan också utföras med hjälp av hello [Azure Portal](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="173cf-228">These steps can also be performed by using hello [Azure Portal](https://portal.azure.com/).</span></span> <span data-ttu-id="173cf-229">Se [skapa och distribuera en Node.js-webbapp i Azure App Service].</span><span class="sxs-lookup"><span data-stu-id="173cf-229">See [Build and deploy a Node.js web app in Azure App Service].</span></span>
> 
> <span data-ttu-id="173cf-230">Om detta är hello första webbapp du har skapat, måste du använda hello Azure Portal toodeploy det här programmet.</span><span class="sxs-lookup"><span data-stu-id="173cf-230">If this is hello first web app you have created, you must use hello Azure Portal toodeploy this application.</span></span>
> 
> 

<span data-ttu-id="173cf-231">tooget igång, installera hello [Azure CLI] genom att ange följande kommando från kommandoraden hello hello:</span><span class="sxs-lookup"><span data-stu-id="173cf-231">tooget started, install hello [Azure CLI] by entering hello following command from hello command line:</span></span>

    npm install azure-cli -g

### <a name="import-publishing-settings"></a><span data-ttu-id="173cf-232">Importera publiceringsinställningarna</span><span class="sxs-lookup"><span data-stu-id="173cf-232">Import publishing settings</span></span>
<span data-ttu-id="173cf-233">I det här steget ska du hämta en fil som innehåller information om din prenumeration.</span><span class="sxs-lookup"><span data-stu-id="173cf-233">In this step, you will download a file containing information about your subscription.</span></span>

1. <span data-ttu-id="173cf-234">Ange hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="173cf-234">Enter hello following command:</span></span>
   
        azure login
   
    <span data-ttu-id="173cf-235">Detta kommando startar en webbläsare och navigerar toohello hämtningssidan.</span><span class="sxs-lookup"><span data-stu-id="173cf-235">This command launches a browser and navigates toohello download page.</span></span> <span data-ttu-id="173cf-236">Om du uppmanas logga in med hello-konto som är associerade med din Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="173cf-236">If prompted, log in with hello account associated with your Azure subscription.</span></span>
   
    <!-- ![hello download page][download-publishing-settings] -->
   
    <span data-ttu-id="173cf-237">hello Filhämtning startar automatiskt. Om det inte du klickar på hello länk hello början av hello toomanually download hello växlingsfilen.</span><span class="sxs-lookup"><span data-stu-id="173cf-237">hello file download begins automatically; if it does not, you can click hello link at hello beginning of hello page toomanually download hello file.</span></span> <span data-ttu-id="173cf-238">Spara hello fil- och Observera hello filsökväg.</span><span class="sxs-lookup"><span data-stu-id="173cf-238">Save hello file and note hello file path.</span></span>
2. <span data-ttu-id="173cf-239">Ange hello följande tooimport hello inställningar:</span><span class="sxs-lookup"><span data-stu-id="173cf-239">Enter hello following command tooimport hello settings:</span></span>
   
        azure account import <path-to-file>
   
    <span data-ttu-id="173cf-240">Ange hello sökvägen och filnamnet för hello publicera inställningsfilen som du hämtade i hello föregående steg.</span><span class="sxs-lookup"><span data-stu-id="173cf-240">Specify hello path and file name of hello publishing settings file you downloaded in hello previous step.</span></span>
3. <span data-ttu-id="173cf-241">Ta bort hello när hello inställningarna importeras inställningsfilen för publicering.</span><span class="sxs-lookup"><span data-stu-id="173cf-241">After hello settings are imported, delete hello publish settings file.</span></span> <span data-ttu-id="173cf-242">Den längre behövs inte och innehåller känslig information om din Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="173cf-242">It is no longer needed, and contains sensitive information regarding your Azure subscription.</span></span>

### <a name="create-an-app-service-web-app"></a><span data-ttu-id="173cf-243">Skapa en Apptjänst-webbapp</span><span class="sxs-lookup"><span data-stu-id="173cf-243">Create an App Service web app</span></span>
1. <span data-ttu-id="173cf-244">Från kommandoraden hello, ändra kataloger toohello **tasklist** directory.</span><span class="sxs-lookup"><span data-stu-id="173cf-244">From hello command-line, change directories toohello **tasklist** directory.</span></span>
2. <span data-ttu-id="173cf-245">Använd följande kommando toocreate ett nytt webbprogram hello.</span><span class="sxs-lookup"><span data-stu-id="173cf-245">Use hello following command toocreate a new web app.</span></span>
   
        azure site create --git
   
    <span data-ttu-id="173cf-246">Du uppmanas att hello webbprogrammets namn och plats.</span><span class="sxs-lookup"><span data-stu-id="173cf-246">You will be prompted for hello web app name and location.</span></span> <span data-ttu-id="173cf-247">Ange ett unikt namn och välj hello samma geografiska plats som ditt Azure Storage-konto.</span><span class="sxs-lookup"><span data-stu-id="173cf-247">Provide a unique name and select hello same geographical location as your Azure Storage account.</span></span>
   
    <span data-ttu-id="173cf-248">Hej `--git` parametern skapar en Git-lagringsplats i Azure för det här webbprogrammet.</span><span class="sxs-lookup"><span data-stu-id="173cf-248">hello `--git` parameter creates a Git repository on Azure for this web app.</span></span> <span data-ttu-id="173cf-249">Dessutom initieras en Git-lagringsplats i hello aktuella katalogen om ingen finns, och lägger till en [Git remote] med namnet ”azure”, vilket är används toopublish hello programmet tooAzure.</span><span class="sxs-lookup"><span data-stu-id="173cf-249">It also initializes a Git repository in hello current directory if none exists, and adds a [Git remote] named 'azure', which is used toopublish hello application tooAzure.</span></span> <span data-ttu-id="173cf-250">Slutligen skapas en **web.config** filen som innehåller inställningarna som används av Azure toohost noden program.</span><span class="sxs-lookup"><span data-stu-id="173cf-250">Finally, it creates a **web.config** file, which contains settings used by Azure toohost node applications.</span></span> <span data-ttu-id="173cf-251">Om du utelämnar hello `--git` parametern men hello directory innehåller en Git-lagringsplats, hello kommandot kommer fortfarande skapa hello ”azure” fjärransluten.</span><span class="sxs-lookup"><span data-stu-id="173cf-251">If you omit hello `--git` parameter but hello directory contains a Git repository, hello command will still create hello 'azure' remote.</span></span>
   
    <span data-ttu-id="173cf-252">När det här kommandot har slutförts visas utdata liknande toohello följande.</span><span class="sxs-lookup"><span data-stu-id="173cf-252">Once this command has completed, you will see output similar toohello following.</span></span> <span data-ttu-id="173cf-253">Observera att hello rad som börjar med **webbplatsen skapas på** innehåller hello URL för hello webbprogrammet.</span><span class="sxs-lookup"><span data-stu-id="173cf-253">Note that hello line beginning with **Website created at** contains hello URL for hello web app.</span></span>
   
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
   > <span data-ttu-id="173cf-254">Du kommer att instrueras toouse hello Azure Portal toocreate hello webbprogram om det här är hello första App Service webbapp för din prenumeration.</span><span class="sxs-lookup"><span data-stu-id="173cf-254">If this is hello first App Service web app for your subscription, you will be instructed toouse hello Azure Portal toocreate hello web app.</span></span> <span data-ttu-id="173cf-255">Mer information finns i [skapa och distribuera en Node.js-webbapp i Azure App Service].</span><span class="sxs-lookup"><span data-stu-id="173cf-255">For more information, see [Build and deploy a Node.js web app in Azure App Service].</span></span>
   > 
   > 

### <a name="set-environment-variables"></a><span data-ttu-id="173cf-256">Uppsättning miljövariabler</span><span class="sxs-lookup"><span data-stu-id="173cf-256">Set environment variables</span></span>
<span data-ttu-id="173cf-257">I det här steget ska du lägga till miljön variabler tooyour webbappkonfigurationen på Azure.</span><span class="sxs-lookup"><span data-stu-id="173cf-257">In this step, you will add environment variables tooyour web app configuration on Azure.</span></span>
<span data-ttu-id="173cf-258">Ange hello följande från hello kommandorad:</span><span class="sxs-lookup"><span data-stu-id="173cf-258">From hello command line, enter hello following:</span></span>

    azure site appsetting add
        STORAGE_NAME=<storage account name>;STORAGE_KEY=<storage access key>;PARTITION_KEY=mytasks;TABLE_NAME=tasks


<span data-ttu-id="173cf-259">Ersätt  **<storage account name>**  med hello namnet hello storage-kontot du skapade tidigare och ersätter  **<storage access key>**  med hello primärnyckeln för ditt lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="173cf-259">Replace **<storage account name>** with hello name of hello storage account you created earlier, and replace **<storage access key>** with hello primary access key for your storage account.</span></span> <span data-ttu-id="173cf-260">(Använd hello samma värden som hello config.json-fil som du skapade tidigare.)</span><span class="sxs-lookup"><span data-stu-id="173cf-260">(Use hello same values as hello config.json file that you created earlier.)</span></span>

<span data-ttu-id="173cf-261">Du kan också ange miljövariabler i hello [Azure Portal](https://portal.azure.com/):</span><span class="sxs-lookup"><span data-stu-id="173cf-261">Alternatively, you can set environment variables in hello [Azure Portal](https://portal.azure.com/):</span></span>

1. <span data-ttu-id="173cf-262">Öppna hello webbapps blad genom att klicka på **Bläddra** > **Web Apps** > din webbprogrammets namn.</span><span class="sxs-lookup"><span data-stu-id="173cf-262">Open hello web app's blade by clicking **Browse** > **Web Apps** > your web app name.</span></span>
2. <span data-ttu-id="173cf-263">I din webbapps blad klickar du på **alla inställningar** > **programinställningar**.</span><span class="sxs-lookup"><span data-stu-id="173cf-263">In your web app's blade, click **All Settings** > **Application Settings**.</span></span>
   
     <!-- ![Top Menu](./media/storage-nodejs-use-table-storage-web-site/PollsCommonWebSiteTopMenu.png) -->
3. <span data-ttu-id="173cf-264">Bläddra nedåt toohello **appinställningar** och lägger till hello nyckel/värde-par.</span><span class="sxs-lookup"><span data-stu-id="173cf-264">Scroll down toohello **App settings** section and add hello key/value pairs.</span></span>
   
     ![App-inställningar](./media/storage-nodejs-use-table-storage-web-site/storage-tasks-appsettings.png)
4. <span data-ttu-id="173cf-266">Klicka på **SPARA**.</span><span class="sxs-lookup"><span data-stu-id="173cf-266">Click **SAVE**.</span></span>

### <a name="publish-hello-application"></a><span data-ttu-id="173cf-267">Publicera programmet hello</span><span class="sxs-lookup"><span data-stu-id="173cf-267">Publish hello application</span></span>
<span data-ttu-id="173cf-268">toopublish hello app genomför hello kod filer tooGit och sedan skicka tooazure eller huvudtjänsten.</span><span class="sxs-lookup"><span data-stu-id="173cf-268">toopublish hello app, commit hello code files tooGit and then push tooazure/master.</span></span>

1. <span data-ttu-id="173cf-269">Ange dina autentiseringsuppgifter för distribution.</span><span class="sxs-lookup"><span data-stu-id="173cf-269">Set your deployment credentials.</span></span>
   
        azure site deployment user set <name> <password>
2. <span data-ttu-id="173cf-270">Lägg till och genomför programfiler.</span><span class="sxs-lookup"><span data-stu-id="173cf-270">Add and commit your application files.</span></span>
   
        git add .
        git commit -m "adding files"
3. <span data-ttu-id="173cf-271">Push hello commit toohello App Service webbapp:</span><span class="sxs-lookup"><span data-stu-id="173cf-271">Push hello commit toohello App Service web app:</span></span>
   
        git push azure master
   
    <span data-ttu-id="173cf-272">Använd **master** som hello mål gren.</span><span class="sxs-lookup"><span data-stu-id="173cf-272">Use **master** as hello target branch.</span></span> <span data-ttu-id="173cf-273">Hello slutet av hello distribution, kan du se en instruktion liknande toohello följande exempel:</span><span class="sxs-lookup"><span data-stu-id="173cf-273">At hello end of hello deployment, you see a statement similar toohello following example:</span></span>
   
        toohttps://username@tabletasklist.azurewebsites.net/TableTasklist.git
          * [new branch]      master -> master
4. <span data-ttu-id="173cf-274">När hello push har slutförts, bläddra toohello web app-URL som tidigare returnerats av hello `azure create site` kommandot tooview ditt program.</span><span class="sxs-lookup"><span data-stu-id="173cf-274">Once hello push operation has completed, browse toohello web app URL returned previously by hello `azure create site` command tooview your application.</span></span>

## <a name="next-steps"></a><span data-ttu-id="173cf-275">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="173cf-275">Next steps</span></span>
<span data-ttu-id="173cf-276">Hello steg i den här artikeln beskriver med hello tabell toostore tjänstinformation, du kan också använda [MongoDB](https://mlab.com/azure/).</span><span class="sxs-lookup"><span data-stu-id="173cf-276">While hello steps in this article describe using hello Table Service toostore information, you can also use [MongoDB](https://mlab.com/azure/).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="173cf-277">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="173cf-277">Additional resources</span></span>
<span data-ttu-id="173cf-278">[Azure CLI]</span><span class="sxs-lookup"><span data-stu-id="173cf-278">[Azure CLI]</span></span>

## <a name="whats-changed"></a><span data-ttu-id="173cf-279">Nyheter</span><span class="sxs-lookup"><span data-stu-id="173cf-279">What's changed</span></span>
* <span data-ttu-id="173cf-280">En guide toohello övergången från webbplatser tooApp tjänsten finns: [Azure App Service och dess påverkan på befintliga Azure-tjänster](http://go.microsoft.com/fwlink/?LinkId=529714)</span><span class="sxs-lookup"><span data-stu-id="173cf-280">For a guide toohello change from Websites tooApp Service see: [Azure App Service and Its Impact on Existing Azure Services](http://go.microsoft.com/fwlink/?LinkId=529714)</span></span>

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
