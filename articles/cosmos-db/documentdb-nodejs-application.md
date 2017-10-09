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
# <span data-ttu-id="13b03-104"><a name="_Toc395783175"></a>Skapa ett Node.js-webbprogram med Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="13b03-104"><a name="_Toc395783175"></a>Build a Node.js web application using Azure Cosmos DB</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="13b03-105">.NET</span><span class="sxs-lookup"><span data-stu-id="13b03-105">.NET</span></span>](documentdb-dotnet-application.md)
> * [<span data-ttu-id="13b03-106">Node.js</span><span class="sxs-lookup"><span data-stu-id="13b03-106">Node.js</span></span>](documentdb-nodejs-application.md)
> * [<span data-ttu-id="13b03-107">Java</span><span class="sxs-lookup"><span data-stu-id="13b03-107">Java</span></span>](documentdb-java-application.md)
> * [<span data-ttu-id="13b03-108">Python</span><span class="sxs-lookup"><span data-stu-id="13b03-108">Python</span></span>](documentdb-python-application.md)
> 
> 

<span data-ttu-id="13b03-109">Den här självstudien om Node.js beskrivs hur toouse Azure Cosmos DB och hello DocumentDB API toostore och komma åt data från ett node.Ja Express-program finns på Azure Websites.</span><span class="sxs-lookup"><span data-stu-id="13b03-109">This Node.js tutorial shows you how toouse Azure Cosmos DB and hello DocumentDB API toostore and access data from a Node.js Express application hosted on Azure Websites.</span></span> <span data-ttu-id="13b03-110">Du bygger ett enkelt webbaserat aktivitetshanteringsprogram, en ToDo-app, där du kan skapa, hämta och slutföra aktiviteter.</span><span class="sxs-lookup"><span data-stu-id="13b03-110">You build a simple web-based task-management application, a ToDo app, that allows creating, retrieving, and completing tasks.</span></span> <span data-ttu-id="13b03-111">hello uppgifter lagras som JSON-dokument i Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="13b03-111">hello tasks are stored as JSON documents in Azure Cosmos DB.</span></span> <span data-ttu-id="13b03-112">Den här självstudiekursen vägleder dig genom hello skapande och distribution av hello appen och förklarar vad som händer i varje fragment.</span><span class="sxs-lookup"><span data-stu-id="13b03-112">This tutorial walks you through hello creation and deployment of hello app and explains what's happening in each snippet.</span></span>

![Skärmbild som visar hello My Todo List-program som skapats i den här självstudien om Node.js](./media/documentdb-nodejs-application/cosmos-db-node-js-mytodo.png)

<span data-ttu-id="13b03-114">Inte har tid toocomplete hello självstudier och bara vill tooget hello komplett lösning?</span><span class="sxs-lookup"><span data-stu-id="13b03-114">Don't have time toocomplete hello tutorial and just want tooget hello complete solution?</span></span> <span data-ttu-id="13b03-115">Inga problem, du kan hämta hello fullständiga exempellösningen från [GitHub][GitHub].</span><span class="sxs-lookup"><span data-stu-id="13b03-115">Not a problem, you can get hello complete sample solution from [GitHub][GitHub].</span></span> <span data-ttu-id="13b03-116">Bara läsa hello [viktigt](https://github.com/Azure-Samples/documentdb-node-todo-app/blob/master/README.md) filen anvisningar för hur toorun hello app.</span><span class="sxs-lookup"><span data-stu-id="13b03-116">Just read hello [Readme](https://github.com/Azure-Samples/documentdb-node-todo-app/blob/master/README.md) file for instructions on how toorun hello app.</span></span>

## <span data-ttu-id="13b03-117"><a name="_Toc395783176"></a>Förhandskrav</span><span class="sxs-lookup"><span data-stu-id="13b03-117"><a name="_Toc395783176"></a>Prerequisites</span></span>
> [!TIP]
> <span data-ttu-id="13b03-118">Den här Node.js-självstudien förutsätter att du har tidigare erfarenhet av Node.js och Azure Websites.</span><span class="sxs-lookup"><span data-stu-id="13b03-118">This Node.js tutorial assumes that you have some prior experience using Node.js and Azure Websites.</span></span>
> 
> 

<span data-ttu-id="13b03-119">Innan du följer hello anvisningarna i den här artikeln bör du kontrollera att du har hello följande:</span><span class="sxs-lookup"><span data-stu-id="13b03-119">Before following hello instructions in this article, you should ensure that you have hello following:</span></span>

* <span data-ttu-id="13b03-120">Ett aktivt Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="13b03-120">An active Azure account.</span></span> <span data-ttu-id="13b03-121">Om du inte har något konto kan du skapa ett kostnadsfritt utvärderingskonto på bara några minuter.</span><span class="sxs-lookup"><span data-stu-id="13b03-121">If you don't have an account, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="13b03-122">Mer information finns i [kostnadsfri utvärderingsversion av Azure](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="13b03-122">For details, see [Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

   <span data-ttu-id="13b03-123">ELLER</span><span class="sxs-lookup"><span data-stu-id="13b03-123">OR</span></span>

   <span data-ttu-id="13b03-124">En lokal installation av hello [Azure Cosmos DB emulatorn](local-emulator.md) (endast Windows).</span><span class="sxs-lookup"><span data-stu-id="13b03-124">A local installation of hello [Azure Cosmos DB Emulator](local-emulator.md) (Windows only).</span></span>
* <span data-ttu-id="13b03-125">[Node.js][Node.js] version v0.10.29 eller högre.</span><span class="sxs-lookup"><span data-stu-id="13b03-125">[Node.js][Node.js] version v0.10.29 or higher.</span></span>
* <span data-ttu-id="13b03-126">[Express generator](http://www.expressjs.com/starter/generator.html) (kan installeras via `npm install express-generator -g`)</span><span class="sxs-lookup"><span data-stu-id="13b03-126">[Express generator](http://www.expressjs.com/starter/generator.html) (you can install this via `npm install express-generator -g`)</span></span>
* <span data-ttu-id="13b03-127">[Git][Git].</span><span class="sxs-lookup"><span data-stu-id="13b03-127">[Git][Git].</span></span>

## <span data-ttu-id="13b03-128"><a name="_Toc395637761"></a>Steg 1: Skapa ett Azure Cosmos DB-databaskonto</span><span class="sxs-lookup"><span data-stu-id="13b03-128"><a name="_Toc395637761"></a>Step 1: Create an Azure Cosmos DB database account</span></span>
<span data-ttu-id="13b03-129">Vi ska börja med att skapa ett Azure Cosmos DB-konto.</span><span class="sxs-lookup"><span data-stu-id="13b03-129">Let's start by creating an Azure Cosmos DB account.</span></span> <span data-ttu-id="13b03-130">Om du redan har ett konto eller om du använder hello Azure Cosmos DB-emulatorn för den här kursen kan du hoppa över för[steg 2: skapa ett nytt Node.js-program](#_Toc395783178).</span><span class="sxs-lookup"><span data-stu-id="13b03-130">If you already have an account or if you are using hello Azure Cosmos DB Emulator for this tutorial, you can skip too[Step 2: Create a new Node.js application](#_Toc395783178).</span></span>

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

[!INCLUDE [cosmos-db-keys](../../includes/cosmos-db-keys.md)]

## <span data-ttu-id="13b03-131"><a name="_Toc395783178"></a>Steg 2: Skapa ett nytt Node.js-program</span><span class="sxs-lookup"><span data-stu-id="13b03-131"><a name="_Toc395783178"></a>Step 2: Create a new Node.js application</span></span>
<span data-ttu-id="13b03-132">Nu ska vi ett grundläggande Hello World Node.js-projekt med hello Läs toocreate [Express](http://expressjs.com/) framework.</span><span class="sxs-lookup"><span data-stu-id="13b03-132">Now let's learn toocreate a basic Hello World Node.js project using hello [Express](http://expressjs.com/) framework.</span></span>

1. <span data-ttu-id="13b03-133">Öppna valfri terminal, till exempel hello Node.js-kommandotolk.</span><span class="sxs-lookup"><span data-stu-id="13b03-133">Open your favorite terminal, such as hello Node.js command prompt.</span></span>
2. <span data-ttu-id="13b03-134">Navigera toohello katalog där du vill att toostore hello nytt program.</span><span class="sxs-lookup"><span data-stu-id="13b03-134">Navigate toohello directory in which you'd like toostore hello new application.</span></span>
3. <span data-ttu-id="13b03-135">Använd hello express generator toogenerate ett nytt program kallas **todo**.</span><span class="sxs-lookup"><span data-stu-id="13b03-135">Use hello express generator toogenerate a new application called **todo**.</span></span>
   
        express todo
4. <span data-ttu-id="13b03-136">Öppna din nya **todo**-katalog och installera beroenden.</span><span class="sxs-lookup"><span data-stu-id="13b03-136">Open your new **todo** directory and install dependencies.</span></span>
   
        cd todo
        npm install
5. <span data-ttu-id="13b03-137">Kör det nya programmet.</span><span class="sxs-lookup"><span data-stu-id="13b03-137">Run your new application.</span></span>
   
        npm start
6. <span data-ttu-id="13b03-138">Du kan visa det nya programmet genom att navigera din webbläsare för[http://localhost: 3000](http://localhost:3000).</span><span class="sxs-lookup"><span data-stu-id="13b03-138">You can view your new application by navigating your browser too[http://localhost:3000](http://localhost:3000).</span></span>
   
    ![Lär dig använda Node.js – skärmdump av hello programmet Hello World i ett webbläsarfönster](./media/documentdb-nodejs-application/cosmos-db-node-js-express.png)

    <span data-ttu-id="13b03-140">Sedan toostop hello program, tryck på CTRL + C i hello terminalfönster och klicka sedan på **y** tooterminate hello batch-jobbet.</span><span class="sxs-lookup"><span data-stu-id="13b03-140">Then, toostop hello application, press CTRL+C in hello terminal window and then click **y** tooterminate hello batch job.</span></span>

## <span data-ttu-id="13b03-141"><a name="_Toc395783179"></a>Steg 3: Installera ytterligare moduler</span><span class="sxs-lookup"><span data-stu-id="13b03-141"><a name="_Toc395783179"></a>Step 3: Install additional modules</span></span>
<span data-ttu-id="13b03-142">Hej **package.json** är en av hello-filer som skapas i hello rot hello projektet.</span><span class="sxs-lookup"><span data-stu-id="13b03-142">hello **package.json** file is one of hello files created in hello root of hello project.</span></span> <span data-ttu-id="13b03-143">Den här filen innehåller en lista över ytterligare moduler som krävs för Node.js-programmet.</span><span class="sxs-lookup"><span data-stu-id="13b03-143">This file contains a list of additional modules that are required for your Node.js application.</span></span> <span data-ttu-id="13b03-144">Senare, när du distribuerar det här programmet tooAzure webbplatser, är filen används toodetermine vilka moduler måste toobe installerad på Azure toosupport ditt program.</span><span class="sxs-lookup"><span data-stu-id="13b03-144">Later, when you deploy this application tooAzure Websites, this file is used toodetermine which modules need toobe installed on Azure toosupport your application.</span></span> <span data-ttu-id="13b03-145">Vi behöver du fortfarande tooinstall två paket för den här självstudiekursen.</span><span class="sxs-lookup"><span data-stu-id="13b03-145">We still need tooinstall two more packages for this tutorial.</span></span>

1. <span data-ttu-id="13b03-146">Tillbaka i hello terminal, installera hello **asynkrona** modul via npm.</span><span class="sxs-lookup"><span data-stu-id="13b03-146">Back in hello terminal, install hello **async** module via npm.</span></span>
   
        npm install async --save
2. <span data-ttu-id="13b03-147">Installera hello **documentdb** modul via npm.</span><span class="sxs-lookup"><span data-stu-id="13b03-147">Install hello **documentdb** module via npm.</span></span> <span data-ttu-id="13b03-148">Detta är hello-modulen där alla hello Azure Cosmos DB magin händer.</span><span class="sxs-lookup"><span data-stu-id="13b03-148">This is hello module where all hello Azure Cosmos DB magic happens.</span></span>
   
        npm install documentdb --save
3. <span data-ttu-id="13b03-149">En snabb kontroll av hello **package.json** -filen för programmet hello ska visa hello ytterligare moduler.</span><span class="sxs-lookup"><span data-stu-id="13b03-149">A quick check of hello **package.json** file of hello application should show hello additional modules.</span></span> <span data-ttu-id="13b03-150">Den här filen ska berätta för Azure vilka paket toodownload och installera när du kör programmet.</span><span class="sxs-lookup"><span data-stu-id="13b03-150">This file will tell Azure which packages toodownload and install when running your application.</span></span> <span data-ttu-id="13b03-151">Det bör likna hello exemplet nedan.</span><span class="sxs-lookup"><span data-stu-id="13b03-151">It should resemble hello example below.</span></span>
   
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
   
    <span data-ttu-id="13b03-152">Det här visar för Node (och senare Azure) att ditt program är beroende av de här ytterligare modulerna.</span><span class="sxs-lookup"><span data-stu-id="13b03-152">This tells Node (and Azure later) that your application depends on these additional modules.</span></span>

## <span data-ttu-id="13b03-153"><a name="_Toc395783180"></a>Steg 4: Använda hello Azure DB som Cosmos-tjänsten i en node-App</span><span class="sxs-lookup"><span data-stu-id="13b03-153"><a name="_Toc395783180"></a>Step 4: Using hello Azure Cosmos DB service in a node application</span></span>
<span data-ttu-id="13b03-154">Som tar hand om alla hello installationen och konfigurationen är vi get ned toowhy vi här och som är toowrite vissa kod som använder Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="13b03-154">That takes care of all hello initial setup and configuration, now let’s get down toowhy we’re here, and that’s toowrite some code using Azure Cosmos DB.</span></span>

### <a name="create-hello-model"></a><span data-ttu-id="13b03-155">Skapa hello modell</span><span class="sxs-lookup"><span data-stu-id="13b03-155">Create hello model</span></span>
1. <span data-ttu-id="13b03-156">Skapa en ny katalog med namnet i hello projektkatalogen **modeller** i hello samma katalog som hello package.json filen.</span><span class="sxs-lookup"><span data-stu-id="13b03-156">In hello project directory, create a new directory named **models** in hello same directory as hello package.json file.</span></span>
2. <span data-ttu-id="13b03-157">I hello **modeller** directory, skapa en ny fil med namnet **taskDao.js**.</span><span class="sxs-lookup"><span data-stu-id="13b03-157">In hello **models** directory, create a new file named **taskDao.js**.</span></span> <span data-ttu-id="13b03-158">Den här filen innehåller hello modellen för hello aktiviteter som skapats av vårt program.</span><span class="sxs-lookup"><span data-stu-id="13b03-158">This file will contain hello model for hello tasks created by our application.</span></span>
3. <span data-ttu-id="13b03-159">Hej i samma **modeller** directory, skapa ytterligare en ny fil med namnet **docdbUtils.js**.</span><span class="sxs-lookup"><span data-stu-id="13b03-159">In hello same **models** directory, create another new file named **docdbUtils.js**.</span></span> <span data-ttu-id="13b03-160">Den här filen innehåller användbar och återanvändbar kod som vi ska använda i programmet.</span><span class="sxs-lookup"><span data-stu-id="13b03-160">This file will contain some useful, reusable, code that we will use throughout our application.</span></span> 
4. <span data-ttu-id="13b03-161">Kopiera hello med följande kod för**docdbUtils.js**</span><span class="sxs-lookup"><span data-stu-id="13b03-161">Copy hello following code in too**docdbUtils.js**</span></span>
   
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
   
5. <span data-ttu-id="13b03-162">Spara och Stäng hello **docdbUtils.js** fil.</span><span class="sxs-lookup"><span data-stu-id="13b03-162">Save and close hello **docdbUtils.js** file.</span></span>
6. <span data-ttu-id="13b03-163">Hello början av hello **taskDao.js** lägger du till följande kod tooreference hello hello **DocumentDBClient** och hello **docdbUtils.js** vi skapade ovan:</span><span class="sxs-lookup"><span data-stu-id="13b03-163">At hello beginning of hello **taskDao.js** file, add hello following code tooreference hello **DocumentDBClient** and hello **docdbUtils.js** we created above:</span></span>
   
        var DocumentDBClient = require('documentdb').DocumentClient;
        var docdbUtils = require('./docdbUtils');
7. <span data-ttu-id="13b03-164">Därefter måste du lägga till kod toodefine och exportera aktivitetsobjektet hello.</span><span class="sxs-lookup"><span data-stu-id="13b03-164">Next, you will add code toodefine and export hello Task object.</span></span> <span data-ttu-id="13b03-165">Detta är ansvarig för att initiera vårt aktivitetsobjekt och konfigurera hello databasen och dokumentsamlingen som vi ska använda.</span><span class="sxs-lookup"><span data-stu-id="13b03-165">This is responsible for initializing our Task object and setting up hello Database and Document Collection we will use.</span></span>
   
        function TaskDao(documentDBClient, databaseId, collectionId) {
          this.client = documentDBClient;
          this.databaseId = databaseId;
          this.collectionId = collectionId;
   
          this.database = null;
          this.collection = null;
        }
   
        module.exports = TaskDao;
8. <span data-ttu-id="13b03-166">Lägg till hello följande kod toodefine fler metoder på hello aktivitetsobjektet som tillåter samverkan med data som lagras i Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="13b03-166">Next, add hello following code toodefine additional methods on hello Task object, which allow interactions with data stored in Azure Cosmos DB.</span></span>
   
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
9. <span data-ttu-id="13b03-167">Spara och Stäng hello **taskDao.js** fil.</span><span class="sxs-lookup"><span data-stu-id="13b03-167">Save and close hello **taskDao.js** file.</span></span> 

### <a name="create-hello-controller"></a><span data-ttu-id="13b03-168">Skapa hello-styrenhet</span><span class="sxs-lookup"><span data-stu-id="13b03-168">Create hello controller</span></span>
1. <span data-ttu-id="13b03-169">I hello **vägar** katalogen för ditt projekt, skapa en ny fil med namnet **tasklist.js**.</span><span class="sxs-lookup"><span data-stu-id="13b03-169">In hello **routes** directory of your project, create a new file named **tasklist.js**.</span></span> 
2. <span data-ttu-id="13b03-170">Lägg till följande kod för hello**tasklist.js**.</span><span class="sxs-lookup"><span data-stu-id="13b03-170">Add hello following code too**tasklist.js**.</span></span> <span data-ttu-id="13b03-171">Den läser in hello DocumentDBClient och async-moduler som används av **tasklist.js**.</span><span class="sxs-lookup"><span data-stu-id="13b03-171">This loads hello DocumentDBClient and async modules, which are used by **tasklist.js**.</span></span> <span data-ttu-id="13b03-172">Detta definieras även hello **TaskList** funktion, som mottar en instans av hello **aktivitet** objektet som vi definierade tidigare:</span><span class="sxs-lookup"><span data-stu-id="13b03-172">This also defined hello **TaskList** function, which is passed an instance of hello **Task** object we defined earlier:</span></span>
   
        var DocumentDBClient = require('documentdb').DocumentClient;
        var async = require('async');
   
        function TaskList(taskDao) {
          this.taskDao = taskDao;
        }
   
        module.exports = TaskList;
3. <span data-ttu-id="13b03-173">Fortsätt att lägga till toohello **tasklist.js** filen genom att lägga till hello-metoder som används för**showTasks, addTask**, och **completeTasks**:</span><span class="sxs-lookup"><span data-stu-id="13b03-173">Continue adding toohello **tasklist.js** file by adding hello methods used too**showTasks, addTask**, and **completeTasks**:</span></span>
   
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
4. <span data-ttu-id="13b03-174">Spara och Stäng hello **tasklist.js** fil.</span><span class="sxs-lookup"><span data-stu-id="13b03-174">Save and close hello **tasklist.js** file.</span></span>

### <a name="add-configjs"></a><span data-ttu-id="13b03-175">Lägg till config.js</span><span class="sxs-lookup"><span data-stu-id="13b03-175">Add config.js</span></span>
1. <span data-ttu-id="13b03-176">Skapa en ny fil med namnet **config.js** i projektkatalogen.</span><span class="sxs-lookup"><span data-stu-id="13b03-176">In your project directory create a new file named **config.js**.</span></span>
2. <span data-ttu-id="13b03-177">Lägg till följande hello för**config.js**.</span><span class="sxs-lookup"><span data-stu-id="13b03-177">Add hello following too**config.js**.</span></span> <span data-ttu-id="13b03-178">Det definierar konfigurationsinställningar och värden som behövs i appen.</span><span class="sxs-lookup"><span data-stu-id="13b03-178">This defines configuration settings and values needed for our application.</span></span>
   
        var config = {}
   
        config.host = process.env.HOST || "[hello URI value from hello Azure Cosmos DB Keys blade on http://portal.azure.com]";
        config.authKey = process.env.AUTH_KEY || "[hello PRIMARY KEY value from hello Azure Cosmos DB Keys blade on http://portal.azure.com]";
        config.databaseId = "ToDoList";
        config.collectionId = "Items";
   
        module.exports = config;
3. <span data-ttu-id="13b03-179">I hello **config.js** -fil, uppdatera hello värden för HOST och AUTH_KEY med hello värdena i hello nycklar bladet för din Azure DB som Cosmos-konto i hello [Microsoft Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="13b03-179">In hello **config.js** file, update hello values of HOST and AUTH_KEY using hello values found in hello Keys blade of your Azure Cosmos DB account on hello [Microsoft Azure portal](https://portal.azure.com).</span></span>
4. <span data-ttu-id="13b03-180">Spara och Stäng hello **config.js** fil.</span><span class="sxs-lookup"><span data-stu-id="13b03-180">Save and close hello **config.js** file.</span></span>

### <a name="modify-appjs"></a><span data-ttu-id="13b03-181">Ändra app.js</span><span class="sxs-lookup"><span data-stu-id="13b03-181">Modify app.js</span></span>
1. <span data-ttu-id="13b03-182">Öppna hello i hello projektkatalogen **app.js** fil.</span><span class="sxs-lookup"><span data-stu-id="13b03-182">In hello project directory, open hello **app.js** file.</span></span> <span data-ttu-id="13b03-183">Den här filen skapades tidigare när hello Express-webbappen skapades.</span><span class="sxs-lookup"><span data-stu-id="13b03-183">This file was created earlier when hello Express web application was created.</span></span>
2. <span data-ttu-id="13b03-184">Lägg till följande kod toohello överkant hello **app.js**</span><span class="sxs-lookup"><span data-stu-id="13b03-184">Add hello following code toohello top of **app.js**</span></span>
   
        var DocumentDBClient = require('documentdb').DocumentClient;
        var config = require('./config');
        var TaskList = require('./routes/tasklist');
        var TaskDao = require('./models/taskDao');
3. <span data-ttu-id="13b03-185">Den här koden definierar hello config-fil toobe används, och fortsätter tooread värden från denna fil till några variabler som vi snart ska använda.</span><span class="sxs-lookup"><span data-stu-id="13b03-185">This code defines hello config file toobe used, and proceeds tooread values out of this file into some variables we will use soon.</span></span>
4. <span data-ttu-id="13b03-186">Ersätt följande två rader i hello **app.js** fil:</span><span class="sxs-lookup"><span data-stu-id="13b03-186">Replace hello following two lines in **app.js** file:</span></span>
   
        app.use('/', index);
        app.use('/users', users); 
   
      <span data-ttu-id="13b03-187">med följande kodavsnitt hello:</span><span class="sxs-lookup"><span data-stu-id="13b03-187">with hello following snippet:</span></span>
   
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
5. <span data-ttu-id="13b03-188">Dessa rader definierar en ny instans av vårt **TaskDao** objekt med en ny anslutning tooAzure Cosmos DB (med hjälp av hello värden läsa från hello **config.js**), initiera hello aktivitetsobjektet och Binder formuläråtgärder toomethods på vår **TaskList** domänkontrollant.</span><span class="sxs-lookup"><span data-stu-id="13b03-188">These lines define a new instance of our **TaskDao** object, with a new connection tooAzure Cosmos DB (using hello values read from hello **config.js**), initialize hello task object and then bind form actions toomethods on our **TaskList** controller.</span></span> 
6. <span data-ttu-id="13b03-189">Slutligen, spara och Stäng hello **app.js** fil, vi är nästan klara.</span><span class="sxs-lookup"><span data-stu-id="13b03-189">Finally, save and close hello **app.js** file, we're just about done.</span></span>

## <span data-ttu-id="13b03-190"><a name="_Toc395783181"></a>Steg 5: Skapa ett användargränssnitt</span><span class="sxs-lookup"><span data-stu-id="13b03-190"><a name="_Toc395783181"></a>Step 5: Build a user interface</span></span>
<span data-ttu-id="13b03-191">Nu dags användargränssnittet våra uppmärksamhet toobuilding hello så att användaren faktiskt kan samverka med vår App.</span><span class="sxs-lookup"><span data-stu-id="13b03-191">Now let’s turn our attention toobuilding hello user interface so a user can actually interact with our application.</span></span> <span data-ttu-id="13b03-192">Hej Express-appen som vi skapade använder **Jade** som hello visningsmotor.</span><span class="sxs-lookup"><span data-stu-id="13b03-192">hello Express application we created uses **Jade** as hello view engine.</span></span> <span data-ttu-id="13b03-193">Mer information om Jade finns för[http://jade-lang.com/](http://jade-lang.com/).</span><span class="sxs-lookup"><span data-stu-id="13b03-193">For more information on Jade please refer too[http://jade-lang.com/](http://jade-lang.com/).</span></span>

1. <span data-ttu-id="13b03-194">Hej **layout.jade** filen i hello **vyer** directory används som en global mall för andra **.jade** filer.</span><span class="sxs-lookup"><span data-stu-id="13b03-194">hello **layout.jade** file in hello **views** directory is used as a global template for other **.jade** files.</span></span> <span data-ttu-id="13b03-195">I det här steget ändrar du den toouse [Twitter Bootstrap](https://github.com/twbs/bootstrap), vilket är en verktygslåda som gör det enkelt toodesign en snygg webbplats.</span><span class="sxs-lookup"><span data-stu-id="13b03-195">In this step you will modify it toouse [Twitter Bootstrap](https://github.com/twbs/bootstrap), which is a toolkit that makes it easy toodesign a nice looking website.</span></span> 
2. <span data-ttu-id="13b03-196">Öppna hello **layout.jade** filen hittas i hello **vyer** mapp och Ersätt hello innehållet med hello följande:</span><span class="sxs-lookup"><span data-stu-id="13b03-196">Open hello **layout.jade** file found in hello **views** folder and replace hello contents with hello following:</span></span>

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

    <span data-ttu-id="13b03-197">Det säger hello **Jade** motorn toorender del HTML för vårt program och skapar en **block** kallas **innehåll** där vi kan tillhandahålla hello layout för våra innehåll sidor.</span><span class="sxs-lookup"><span data-stu-id="13b03-197">This effectively tells hello **Jade** engine toorender some HTML for our application and creates a **block** called **content** where we can supply hello layout for our content pages.</span></span>

    <span data-ttu-id="13b03-198">Spara och stäng filen **layout.jade**.</span><span class="sxs-lookup"><span data-stu-id="13b03-198">Save and close this **layout.jade** file.</span></span>

3. <span data-ttu-id="13b03-199">Öppna hello **index.jade** fil, hello vy som ska användas av vårt program och Ersätt hello innehållet hello-filen med hello följande:</span><span class="sxs-lookup"><span data-stu-id="13b03-199">Now open hello **index.jade** file, hello view that will be used by our application, and replace hello content of hello file with hello following:</span></span>
   
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
   

<span data-ttu-id="13b03-200">Det utökar layouten och ger innehåll för hello **innehåll** platshållare som vi såg i hello **layout.jade** filen tidigare.</span><span class="sxs-lookup"><span data-stu-id="13b03-200">This extends layout, and provides content for hello **content** placeholder we saw in hello **layout.jade** file earlier.</span></span>
   
<span data-ttu-id="13b03-201">I den här layouten skapade vi två HTML-formulär.</span><span class="sxs-lookup"><span data-stu-id="13b03-201">In this layout we created two HTML forms.</span></span>

<span data-ttu-id="13b03-202">hello första formuläret innehåller en tabell för våra data och en knapp som gör att vi tooupdate objekt genom att publicera för**/completetask** metod i styrningen.</span><span class="sxs-lookup"><span data-stu-id="13b03-202">hello first form contains a table for our data and a button that allows us tooupdate items by posting too**/completetask** method of our controller.</span></span>
    
<span data-ttu-id="13b03-203">hello andra formuläret innehåller två inmatningsfält och en knapp som gör att vi toocreate ett nytt objekt genom att publicera för**/addtask** metod i styrningen.</span><span class="sxs-lookup"><span data-stu-id="13b03-203">hello second form contains two input fields and a button that allows us toocreate a new item by posting too**/addtask** method of our controller.</span></span>

<span data-ttu-id="13b03-204">Detta bör vara allt som behövs för vårt program toowork.</span><span class="sxs-lookup"><span data-stu-id="13b03-204">This should be all that we need for our application toowork.</span></span>

## <span data-ttu-id="13b03-205"><a name="_Toc395783181"></a>Steg 6: Kör ditt program lokalt</span><span class="sxs-lookup"><span data-stu-id="13b03-205"><a name="_Toc395783181"></a>Step 6: Run your application locally</span></span>
1. <span data-ttu-id="13b03-206">tootest hello programmet på din lokala dator kör `npm start` i hello terminal toostart ditt program och uppdatera sedan din [http://localhost: 3000](http://localhost:3000) Webbläsarsida.</span><span class="sxs-lookup"><span data-stu-id="13b03-206">tootest hello application on your local machine, run `npm start` in hello terminal toostart your application, then refresh your [http://localhost:3000](http://localhost:3000) browser page.</span></span> <span data-ttu-id="13b03-207">hello sidan bör nu se ut så hello bilden nedan:</span><span class="sxs-lookup"><span data-stu-id="13b03-207">hello page should now look like hello image below:</span></span>
   
    ![Skärmbild av hello programmet MyTodo List i ett webbläsarfönster](./media/documentdb-nodejs-application/cosmos-db-node-js-localhost.png)

    > [!TIP]
    > <span data-ttu-id="13b03-209">Om du får ett felmeddelande om hello indrag i hello layout.jade fil eller hello index.jade säkerställa att hello två första raderna i filer som är kvar berättigade, utan blanksteg.</span><span class="sxs-lookup"><span data-stu-id="13b03-209">If you receive an error about hello indent in hello layout.jade file or hello index.jade file, ensure that hello first two lines in both files is left justified, with no spaces.</span></span> <span data-ttu-id="13b03-210">Om det finns blanksteg före hello två första raderna, ta bort dem, spara filer och sedan uppdatera webbläsarfönstret.</span><span class="sxs-lookup"><span data-stu-id="13b03-210">If there are spaces before hello first two lines, remove them, save both files, then refresh your browser window.</span></span> 

2. <span data-ttu-id="13b03-211">Hello objekt, objektnamn och kategori fält tooenter en ny uppgift och klicka sedan på **Lägg till objekt**.</span><span class="sxs-lookup"><span data-stu-id="13b03-211">Use hello Item, Item Name and Category fields tooenter a new task and then click **Add Item**.</span></span> <span data-ttu-id="13b03-212">När du gör det skapas ett dokument i Azure Cosmos DB med dessa egenskaper.</span><span class="sxs-lookup"><span data-stu-id="13b03-212">This creates a document in Azure Cosmos DB with those properties.</span></span> 
3. <span data-ttu-id="13b03-213">hello sidan bör uppdateras toodisplay hello nyligen skapade objektet i hello ToDo-listan.</span><span class="sxs-lookup"><span data-stu-id="13b03-213">hello page should update toodisplay hello newly created item in hello ToDo list.</span></span>
   
    ![Skärmdump av programmet hello med ett nytt objekt i hello göra-lista](./media/documentdb-nodejs-application/cosmos-db-node-js-added-task.png)
4. <span data-ttu-id="13b03-215">toocomplete en aktivitet bara kryssrutan hello i hello fullständig kolumnen och klicka sedan på **uppdatera aktiviteter**.</span><span class="sxs-lookup"><span data-stu-id="13b03-215">toocomplete a task, simply check hello checkbox in hello Complete column, and then click **Update tasks**.</span></span> <span data-ttu-id="13b03-216">Detta uppdaterar hello-dokument som du redan har skapat.</span><span class="sxs-lookup"><span data-stu-id="13b03-216">This updates hello document you already created.</span></span>

5. <span data-ttu-id="13b03-217">toostop hello program, tryck på CTRL + C i hello terminalfönster och klicka sedan på **Y** tooterminate hello batch-jobbet.</span><span class="sxs-lookup"><span data-stu-id="13b03-217">toostop hello application, press CTRL+C in hello terminal window and then click **Y** tooterminate hello batch job.</span></span>

## <span data-ttu-id="13b03-218"><a name="_Toc395783182"></a>Steg 7: Distribuera ditt program development projektet tooAzure webbplatser</span><span class="sxs-lookup"><span data-stu-id="13b03-218"><a name="_Toc395783182"></a>Step 7: Deploy your application development project tooAzure Websites</span></span>
1. <span data-ttu-id="13b03-219">Om du inte redan gjort det aktiverar du en git-databas för Azure-webbplatsen.</span><span class="sxs-lookup"><span data-stu-id="13b03-219">If you haven't already, enable a git repository for your Azure Website.</span></span> <span data-ttu-id="13b03-220">Du hittar anvisningar om hur toodo detta i hello [lokal Git-distribution tooAzure Apptjänst](../app-service-web/app-service-deploy-local-git.md) avsnittet.</span><span class="sxs-lookup"><span data-stu-id="13b03-220">You can find instructions on how toodo this in hello [Local Git Deployment tooAzure App Service](../app-service-web/app-service-deploy-local-git.md) topic.</span></span>
2. <span data-ttu-id="13b03-221">Lägg till Azure-webbplatsen som en fjärransluten git.</span><span class="sxs-lookup"><span data-stu-id="13b03-221">Add your Azure Website as a git remote.</span></span>
   
        git remote add azure https://username@your-azure-website.scm.azurewebsites.net:443/your-azure-website.git
3. <span data-ttu-id="13b03-222">Distribuera genom att trycka på toohello fjärråtkomst.</span><span class="sxs-lookup"><span data-stu-id="13b03-222">Deploy by pushing toohello remote.</span></span>
   
        git push azure master
4. <span data-ttu-id="13b03-223">I några sekunder har git publicerat din webbapp och öppnar en webbläsare där du kan se ditt arbete som körs i Azure!</span><span class="sxs-lookup"><span data-stu-id="13b03-223">In a few seconds, git will finish publishing your web application and launch a browser where you can see your handiwork running in Azure!</span></span>

    <span data-ttu-id="13b03-224">Grattis!</span><span class="sxs-lookup"><span data-stu-id="13b03-224">Congratulations!</span></span> <span data-ttu-id="13b03-225">Du har precis skapat din första Node.js Express webbprogram med hjälp av Azure Cosmos DB och publicerat den tooAzure webbplatser.</span><span class="sxs-lookup"><span data-stu-id="13b03-225">You have just built your first Node.js Express Web Application using Azure Cosmos DB and published it tooAzure Websites.</span></span>

    <span data-ttu-id="13b03-226">Om du vill toodownload eller referera toohello fullständiga referens program för den här kursen kan hämta från [GitHub][GitHub].</span><span class="sxs-lookup"><span data-stu-id="13b03-226">If you want toodownload or refer toohello complete reference application for this tutorial, it can be downloaded from [GitHub][GitHub].</span></span>

## <span data-ttu-id="13b03-227"><a name="_Toc395637775"></a>Nästa steg</span><span class="sxs-lookup"><span data-stu-id="13b03-227"><a name="_Toc395637775"></a>Next steps</span></span>

* <span data-ttu-id="13b03-228">Vill tooperform skala och prestandatester med Azure Cosmos DB?</span><span class="sxs-lookup"><span data-stu-id="13b03-228">Want tooperform scale and performance testing with Azure Cosmos DB?</span></span> <span data-ttu-id="13b03-229">Mer information finns i avsnittet om hur du [testar prestanda och skalning med Azure Cosmos DB](performance-testing.md)</span><span class="sxs-lookup"><span data-stu-id="13b03-229">See [Performance and Scale Testing with Azure Cosmos DB](performance-testing.md)</span></span>
* <span data-ttu-id="13b03-230">Lär dig hur för[övervaka ett konto i Azure Cosmos DB](monitor-accounts.md).</span><span class="sxs-lookup"><span data-stu-id="13b03-230">Learn how too[monitor an Azure Cosmos DB account](monitor-accounts.md).</span></span>
* <span data-ttu-id="13b03-231">Kör frågor mot vår exempeldatauppsättning i hello [Query Playground](https://www.documentdb.com/sql/demo).</span><span class="sxs-lookup"><span data-stu-id="13b03-231">Run queries against our sample dataset in hello [Query Playground](https://www.documentdb.com/sql/demo).</span></span>
* <span data-ttu-id="13b03-232">Utforska hello [Azure Cosmos DB dokumentationen](https://docs.microsoft.com/azure/documentdb/).</span><span class="sxs-lookup"><span data-stu-id="13b03-232">Explore hello [Azure Cosmos DB documentation](https://docs.microsoft.com/azure/documentdb/).</span></span>

[Node.js]: http://nodejs.org/
[Git]: http://git-scm.com/
[GitHub]: https://github.com/Azure-Samples/documentdb-node-todo-app

