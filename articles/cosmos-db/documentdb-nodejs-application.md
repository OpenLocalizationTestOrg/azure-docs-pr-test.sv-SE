---
title: "Skapa en Node.js-webbapp för Azure Cosmos DB | Microsoft Docs"
description: "Den här självstudien om Node.js utforskar hur du använder Microsoft Azure Cosmos DB för att lagra och komma åt data från en Node.js Express-webbapp på Azure Websites."
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
ms.openlocfilehash: 1a98509a98bcd2a5de593eb006f905766fe72966
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <span data-ttu-id="eeb73-104"><a name="_Toc395783175"></a>Skapa ett Node.js-webbprogram med Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="eeb73-104"><a name="_Toc395783175"></a>Build a Node.js web application using Azure Cosmos DB</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="eeb73-105">.NET</span><span class="sxs-lookup"><span data-stu-id="eeb73-105">.NET</span></span>](documentdb-dotnet-application.md)
> * [<span data-ttu-id="eeb73-106">Node.js</span><span class="sxs-lookup"><span data-stu-id="eeb73-106">Node.js</span></span>](documentdb-nodejs-application.md)
> * [<span data-ttu-id="eeb73-107">Java</span><span class="sxs-lookup"><span data-stu-id="eeb73-107">Java</span></span>](documentdb-java-application.md)
> * [<span data-ttu-id="eeb73-108">Python</span><span class="sxs-lookup"><span data-stu-id="eeb73-108">Python</span></span>](documentdb-python-application.md)
> 
> 

<span data-ttu-id="eeb73-109">Den här självstudien om Node.js beskrivs hur du använder Azure Cosmos DB och DocumentDB-API för att lagra och komma åt data från ett node.Ja Express-program på Azure Websites.</span><span class="sxs-lookup"><span data-stu-id="eeb73-109">This Node.js tutorial shows you how to use Azure Cosmos DB and the DocumentDB API to store and access data from a Node.js Express application hosted on Azure Websites.</span></span> <span data-ttu-id="eeb73-110">Du bygger ett enkelt webbaserat aktivitetshanteringsprogram, en ToDo-app, där du kan skapa, hämta och slutföra aktiviteter.</span><span class="sxs-lookup"><span data-stu-id="eeb73-110">You build a simple web-based task-management application, a ToDo app, that allows creating, retrieving, and completing tasks.</span></span> <span data-ttu-id="eeb73-111">Uppgifterna lagras som JSON-dokument i Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="eeb73-111">The tasks are stored as JSON documents in Azure Cosmos DB.</span></span> <span data-ttu-id="eeb73-112">Den här självstudien vägleder dig genom skapandet och distributionen av appen och förklarar vad som händer i varje kodfragment.</span><span class="sxs-lookup"><span data-stu-id="eeb73-112">This tutorial walks you through the creation and deployment of the app and explains what's happening in each snippet.</span></span>

![Skärmdump av programmet My Todo List som skapas genom stegen i den här självstudien om Node.js](./media/documentdb-nodejs-application/cosmos-db-node-js-mytodo.png)

<span data-ttu-id="eeb73-114">Har du inte tid att gå igenom självstudien, men vill ha hela lösningen?</span><span class="sxs-lookup"><span data-stu-id="eeb73-114">Don't have time to complete the tutorial and just want to get the complete solution?</span></span> <span data-ttu-id="eeb73-115">Inga problem, du kan hämta den fullständiga exempellösningen från [GitHub][GitHub].</span><span class="sxs-lookup"><span data-stu-id="eeb73-115">Not a problem, you can get the complete sample solution from [GitHub][GitHub].</span></span> <span data-ttu-id="eeb73-116">Läs bara [Viktigt](https://github.com/Azure-Samples/documentdb-node-todo-app/blob/master/README.md)-filen för instruktioner om hur du kör appen.</span><span class="sxs-lookup"><span data-stu-id="eeb73-116">Just read the [Readme](https://github.com/Azure-Samples/documentdb-node-todo-app/blob/master/README.md) file for instructions on how to run the app.</span></span>

## <span data-ttu-id="eeb73-117"><a name="_Toc395783176"></a>Förhandskrav</span><span class="sxs-lookup"><span data-stu-id="eeb73-117"><a name="_Toc395783176"></a>Prerequisites</span></span>
> [!TIP]
> <span data-ttu-id="eeb73-118">Den här Node.js-självstudien förutsätter att du har tidigare erfarenhet av Node.js och Azure Websites.</span><span class="sxs-lookup"><span data-stu-id="eeb73-118">This Node.js tutorial assumes that you have some prior experience using Node.js and Azure Websites.</span></span>
> 
> 

<span data-ttu-id="eeb73-119">Innan du följer anvisningarna i den här artikeln bör du se till att du har följande:</span><span class="sxs-lookup"><span data-stu-id="eeb73-119">Before following the instructions in this article, you should ensure that you have the following:</span></span>

* <span data-ttu-id="eeb73-120">Ett aktivt Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="eeb73-120">An active Azure account.</span></span> <span data-ttu-id="eeb73-121">Om du inte har något konto kan du skapa ett kostnadsfritt utvärderingskonto på bara några minuter.</span><span class="sxs-lookup"><span data-stu-id="eeb73-121">If you don't have an account, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="eeb73-122">Mer information finns i [kostnadsfri utvärderingsversion av Azure](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="eeb73-122">For details, see [Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

   <span data-ttu-id="eeb73-123">ELLER</span><span class="sxs-lookup"><span data-stu-id="eeb73-123">OR</span></span>

   <span data-ttu-id="eeb73-124">En lokal installation av den [Azure Cosmos DB emulatorn](local-emulator.md) (endast Windows).</span><span class="sxs-lookup"><span data-stu-id="eeb73-124">A local installation of the [Azure Cosmos DB Emulator](local-emulator.md) (Windows only).</span></span>
* <span data-ttu-id="eeb73-125">[Node.js][Node.js] version v0.10.29 eller högre.</span><span class="sxs-lookup"><span data-stu-id="eeb73-125">[Node.js][Node.js] version v0.10.29 or higher.</span></span>
* <span data-ttu-id="eeb73-126">[Express generator](http://www.expressjs.com/starter/generator.html) (kan installeras via `npm install express-generator -g`)</span><span class="sxs-lookup"><span data-stu-id="eeb73-126">[Express generator](http://www.expressjs.com/starter/generator.html) (you can install this via `npm install express-generator -g`)</span></span>
* <span data-ttu-id="eeb73-127">[Git][Git].</span><span class="sxs-lookup"><span data-stu-id="eeb73-127">[Git][Git].</span></span>

## <span data-ttu-id="eeb73-128"><a name="_Toc395637761"></a>Steg 1: Skapa ett Azure Cosmos DB-databaskonto</span><span class="sxs-lookup"><span data-stu-id="eeb73-128"><a name="_Toc395637761"></a>Step 1: Create an Azure Cosmos DB database account</span></span>
<span data-ttu-id="eeb73-129">Vi ska börja med att skapa ett Azure Cosmos DB-konto.</span><span class="sxs-lookup"><span data-stu-id="eeb73-129">Let's start by creating an Azure Cosmos DB account.</span></span> <span data-ttu-id="eeb73-130">Om du redan har ett konto eller om du använder Azure Cosmos DB-emulatorn för den här kursen kan du gå vidare till [Steg 2: Skapa ett nytt Node.js-program](#_Toc395783178).</span><span class="sxs-lookup"><span data-stu-id="eeb73-130">If you already have an account or if you are using the Azure Cosmos DB Emulator for this tutorial, you can skip to [Step 2: Create a new Node.js application](#_Toc395783178).</span></span>

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

[!INCLUDE [cosmos-db-keys](../../includes/cosmos-db-keys.md)]

## <span data-ttu-id="eeb73-131"><a name="_Toc395783178"></a>Steg 2: Skapa ett nytt Node.js-program</span><span class="sxs-lookup"><span data-stu-id="eeb73-131"><a name="_Toc395783178"></a>Step 2: Create a new Node.js application</span></span>
<span data-ttu-id="eeb73-132">Nu ska vi skapa ett grundläggande Hello World Node.js-projekt med [Express](http://expressjs.com/)-ramverket.</span><span class="sxs-lookup"><span data-stu-id="eeb73-132">Now let's learn to create a basic Hello World Node.js project using the [Express](http://expressjs.com/) framework.</span></span>

1. <span data-ttu-id="eeb73-133">Öppna din favoritterminal, till exempel Node.js-kommandotolken.</span><span class="sxs-lookup"><span data-stu-id="eeb73-133">Open your favorite terminal, such as the Node.js command prompt.</span></span>
2. <span data-ttu-id="eeb73-134">Navigera till den katalog där du vill lagra det nya programmet.</span><span class="sxs-lookup"><span data-stu-id="eeb73-134">Navigate to the directory in which you'd like to store the new application.</span></span>
3. <span data-ttu-id="eeb73-135">Skapa ett nytt program kallat **todo** med hjälp av Express Generator.</span><span class="sxs-lookup"><span data-stu-id="eeb73-135">Use the express generator to generate a new application called **todo**.</span></span>
   
        express todo
4. <span data-ttu-id="eeb73-136">Öppna din nya **todo**-katalog och installera beroenden.</span><span class="sxs-lookup"><span data-stu-id="eeb73-136">Open your new **todo** directory and install dependencies.</span></span>
   
        cd todo
        npm install
5. <span data-ttu-id="eeb73-137">Kör det nya programmet.</span><span class="sxs-lookup"><span data-stu-id="eeb73-137">Run your new application.</span></span>
   
        npm start
6. <span data-ttu-id="eeb73-138">Du kan visa det nya programmet genom att öppna [http://localhost:3000](http://localhost:3000) i webbläsaren.</span><span class="sxs-lookup"><span data-stu-id="eeb73-138">You can view your new application by navigating your browser to [http://localhost:3000](http://localhost:3000).</span></span>
   
    ![Lär dig använda Node.js – Skärmdump av programmet Hello World i ett webbläsarfönster](./media/documentdb-nodejs-application/cosmos-db-node-js-express.png)

    <span data-ttu-id="eeb73-140">Därefter, för att stoppa programmet, trycker du på CTRL+C i terminalfönstret och klickar sedan på **y** för att avbryta batch-jobbet.</span><span class="sxs-lookup"><span data-stu-id="eeb73-140">Then, to stop the application, press CTRL+C in the terminal window and then click **y** to terminate the batch job.</span></span>

## <span data-ttu-id="eeb73-141"><a name="_Toc395783179"></a>Steg 3: Installera ytterligare moduler</span><span class="sxs-lookup"><span data-stu-id="eeb73-141"><a name="_Toc395783179"></a>Step 3: Install additional modules</span></span>
<span data-ttu-id="eeb73-142">Filen **package.json** är en av filerna som skapas i projektets rot.</span><span class="sxs-lookup"><span data-stu-id="eeb73-142">The **package.json** file is one of the files created in the root of the project.</span></span> <span data-ttu-id="eeb73-143">Den här filen innehåller en lista över ytterligare moduler som krävs för Node.js-programmet.</span><span class="sxs-lookup"><span data-stu-id="eeb73-143">This file contains a list of additional modules that are required for your Node.js application.</span></span> <span data-ttu-id="eeb73-144">Senare, när du distribuerar programmet till Azure Websites används den här filen för att avgöra vilka moduler måste installeras på Azure som stöd för ditt program.</span><span class="sxs-lookup"><span data-stu-id="eeb73-144">Later, when you deploy this application to Azure Websites, this file is used to determine which modules need to be installed on Azure to support your application.</span></span> <span data-ttu-id="eeb73-145">Vi behöver installera två paket till för den här självstudien.</span><span class="sxs-lookup"><span data-stu-id="eeb73-145">We still need to install two more packages for this tutorial.</span></span>

1. <span data-ttu-id="eeb73-146">Gå tillbaka till terminalen och installera modulen **async** via npm.</span><span class="sxs-lookup"><span data-stu-id="eeb73-146">Back in the terminal, install the **async** module via npm.</span></span>
   
        npm install async --save
2. <span data-ttu-id="eeb73-147">Installera modulen **documentdb** via npm.</span><span class="sxs-lookup"><span data-stu-id="eeb73-147">Install the **documentdb** module via npm.</span></span> <span data-ttu-id="eeb73-148">Det här är modulen där alla Azure DB som Cosmos-magin händer.</span><span class="sxs-lookup"><span data-stu-id="eeb73-148">This is the module where all the Azure Cosmos DB magic happens.</span></span>
   
        npm install documentdb --save
3. <span data-ttu-id="eeb73-149">En snabb kontroll av filen **package.json** i appen bör visa ytterligare moduler.</span><span class="sxs-lookup"><span data-stu-id="eeb73-149">A quick check of the **package.json** file of the application should show the additional modules.</span></span> <span data-ttu-id="eeb73-150">Den här filen talar om för Azure vilka paket som ska laddas ned och installeras när du kör programmet.</span><span class="sxs-lookup"><span data-stu-id="eeb73-150">This file will tell Azure which packages to download and install when running your application.</span></span> <span data-ttu-id="eeb73-151">Det bör likna exemplet nedan.</span><span class="sxs-lookup"><span data-stu-id="eeb73-151">It should resemble the example below.</span></span>
   
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
   
    <span data-ttu-id="eeb73-152">Det här visar för Node (och senare Azure) att ditt program är beroende av de här ytterligare modulerna.</span><span class="sxs-lookup"><span data-stu-id="eeb73-152">This tells Node (and Azure later) that your application depends on these additional modules.</span></span>

## <span data-ttu-id="eeb73-153"><a name="_Toc395783180"></a>Steg 4: Använda Azure Cosmos DB-tjänsten i ett nodprogram</span><span class="sxs-lookup"><span data-stu-id="eeb73-153"><a name="_Toc395783180"></a>Step 4: Using the Azure Cosmos DB service in a node application</span></span>
<span data-ttu-id="eeb73-154">Nu när vi har slutfört den första installationen och konfigurationen är det dags att ta itu med vårt verkliga syfte: att skriva kod med Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="eeb73-154">That takes care of all the initial setup and configuration, now let’s get down to why we’re here, and that’s to write some code using Azure Cosmos DB.</span></span>

### <a name="create-the-model"></a><span data-ttu-id="eeb73-155">Skapa modellen</span><span class="sxs-lookup"><span data-stu-id="eeb73-155">Create the model</span></span>
1. <span data-ttu-id="eeb73-156">Skapa en ny katalog i projektkatalogen med namnet **models**, i samma katalog som package.json-filen.</span><span class="sxs-lookup"><span data-stu-id="eeb73-156">In the project directory, create a new directory named **models** in the same directory as the package.json file.</span></span>
2. <span data-ttu-id="eeb73-157">I katalogen **models** skapar du en ny fil med namnet **taskDao.js**.</span><span class="sxs-lookup"><span data-stu-id="eeb73-157">In the **models** directory, create a new file named **taskDao.js**.</span></span> <span data-ttu-id="eeb73-158">Den här filen innehåller modellen för de aktiviteter som skapats av vårt program.</span><span class="sxs-lookup"><span data-stu-id="eeb73-158">This file will contain the model for the tasks created by our application.</span></span>
3. <span data-ttu-id="eeb73-159">I samma **models**-katalog skapar du ytterligare en ny fil med namnet **docdbUtils.js**.</span><span class="sxs-lookup"><span data-stu-id="eeb73-159">In the same **models** directory, create another new file named **docdbUtils.js**.</span></span> <span data-ttu-id="eeb73-160">Den här filen innehåller användbar och återanvändbar kod som vi ska använda i programmet.</span><span class="sxs-lookup"><span data-stu-id="eeb73-160">This file will contain some useful, reusable, code that we will use throughout our application.</span></span> 
4. <span data-ttu-id="eeb73-161">Kopiera följande kod till **docdbUtils.js**</span><span class="sxs-lookup"><span data-stu-id="eeb73-161">Copy the following code in to **docdbUtils.js**</span></span>
   
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
   
5. <span data-ttu-id="eeb73-162">Spara och stäng filen **docdbUtils.js**.</span><span class="sxs-lookup"><span data-stu-id="eeb73-162">Save and close the **docdbUtils.js** file.</span></span>
6. <span data-ttu-id="eeb73-163">I början av filen **taskDao.js** lägger du till följande kod för att referera till **DocumentDBClient** och **docdbUtils.js** som vi skapade ovan:</span><span class="sxs-lookup"><span data-stu-id="eeb73-163">At the beginning of the **taskDao.js** file, add the following code to reference the **DocumentDBClient** and the **docdbUtils.js** we created above:</span></span>
   
        var DocumentDBClient = require('documentdb').DocumentClient;
        var docdbUtils = require('./docdbUtils');
7. <span data-ttu-id="eeb73-164">Sedan lägger du till kod för att definiera och exportera aktivitetsobjektet.</span><span class="sxs-lookup"><span data-stu-id="eeb73-164">Next, you will add code to define and export the Task object.</span></span> <span data-ttu-id="eeb73-165">Den ansvarar för att initiera vårt aktivitetsobjekt och konfigurera databasen och dokumentsamlingen som vi ska använda.</span><span class="sxs-lookup"><span data-stu-id="eeb73-165">This is responsible for initializing our Task object and setting up the Database and Document Collection we will use.</span></span>
   
        function TaskDao(documentDBClient, databaseId, collectionId) {
          this.client = documentDBClient;
          this.databaseId = databaseId;
          this.collectionId = collectionId;
   
          this.database = null;
          this.collection = null;
        }
   
        module.exports = TaskDao;
8. <span data-ttu-id="eeb73-166">Lägg sedan till följande kod för att definiera ytterligare metoder för aktivitetsobjektet, som gör att det går att interagera med data som lagras i Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="eeb73-166">Next, add the following code to define additional methods on the Task object, which allow interactions with data stored in Azure Cosmos DB.</span></span>
   
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
9. <span data-ttu-id="eeb73-167">Spara och stäng filen **taskDao.js**.</span><span class="sxs-lookup"><span data-stu-id="eeb73-167">Save and close the **taskDao.js** file.</span></span> 

### <a name="create-the-controller"></a><span data-ttu-id="eeb73-168">Skapa styrningen</span><span class="sxs-lookup"><span data-stu-id="eeb73-168">Create the controller</span></span>
1. <span data-ttu-id="eeb73-169">I projektets **routes**-katalog skapar du en ny fil med namnet **tasklist.js**.</span><span class="sxs-lookup"><span data-stu-id="eeb73-169">In the **routes** directory of your project, create a new file named **tasklist.js**.</span></span> 
2. <span data-ttu-id="eeb73-170">Lägg till följande kod i **tasklist.js**.</span><span class="sxs-lookup"><span data-stu-id="eeb73-170">Add the following code to **tasklist.js**.</span></span> <span data-ttu-id="eeb73-171">Den läser in DocumentDBClient och async-moduler som används av **tasklist.js**.</span><span class="sxs-lookup"><span data-stu-id="eeb73-171">This loads the DocumentDBClient and async modules, which are used by **tasklist.js**.</span></span> <span data-ttu-id="eeb73-172">Detta definieras även i funktionen **TaskList**, som mottar en instans av **Task**-objektet som vi definierade tidigare:</span><span class="sxs-lookup"><span data-stu-id="eeb73-172">This also defined the **TaskList** function, which is passed an instance of the **Task** object we defined earlier:</span></span>
   
        var DocumentDBClient = require('documentdb').DocumentClient;
        var async = require('async');
   
        function TaskList(taskDao) {
          this.taskDao = taskDao;
        }
   
        module.exports = TaskList;
3. <span data-ttu-id="eeb73-173">Fortsätt att lägga till kod i filen **tasklist.js** genom att lägga till metoderna **showTasks, addTask** och **completeTasks**:</span><span class="sxs-lookup"><span data-stu-id="eeb73-173">Continue adding to the **tasklist.js** file by adding the methods used to **showTasks, addTask**, and **completeTasks**:</span></span>
   
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
4. <span data-ttu-id="eeb73-174">Spara och stäng filen **tasklist.js**.</span><span class="sxs-lookup"><span data-stu-id="eeb73-174">Save and close the **tasklist.js** file.</span></span>

### <a name="add-configjs"></a><span data-ttu-id="eeb73-175">Lägg till config.js</span><span class="sxs-lookup"><span data-stu-id="eeb73-175">Add config.js</span></span>
1. <span data-ttu-id="eeb73-176">Skapa en ny fil med namnet **config.js** i projektkatalogen.</span><span class="sxs-lookup"><span data-stu-id="eeb73-176">In your project directory create a new file named **config.js**.</span></span>
2. <span data-ttu-id="eeb73-177">Lägg till följande i **config.js**.</span><span class="sxs-lookup"><span data-stu-id="eeb73-177">Add the following to **config.js**.</span></span> <span data-ttu-id="eeb73-178">Det definierar konfigurationsinställningar och värden som behövs i appen.</span><span class="sxs-lookup"><span data-stu-id="eeb73-178">This defines configuration settings and values needed for our application.</span></span>
   
        var config = {}
   
        config.host = process.env.HOST || "[the URI value from the Azure Cosmos DB Keys blade on http://portal.azure.com]";
        config.authKey = process.env.AUTH_KEY || "[the PRIMARY KEY value from the Azure Cosmos DB Keys blade on http://portal.azure.com]";
        config.databaseId = "ToDoList";
        config.collectionId = "Items";
   
        module.exports = config;
3. <span data-ttu-id="eeb73-179">I filen **config.js** uppdaterar du värdet för HOST och AUTH_KEY med värdena på bladet Nycklar för ditt Azure Cosmos DB-konto på [Microsoft Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="eeb73-179">In the **config.js** file, update the values of HOST and AUTH_KEY using the values found in the Keys blade of your Azure Cosmos DB account on the [Microsoft Azure portal](https://portal.azure.com).</span></span>
4. <span data-ttu-id="eeb73-180">Spara och stäng filen **config.js**.</span><span class="sxs-lookup"><span data-stu-id="eeb73-180">Save and close the **config.js** file.</span></span>

### <a name="modify-appjs"></a><span data-ttu-id="eeb73-181">Ändra app.js</span><span class="sxs-lookup"><span data-stu-id="eeb73-181">Modify app.js</span></span>
1. <span data-ttu-id="eeb73-182">Öppna filen **app.js** i projektkatalogen.</span><span class="sxs-lookup"><span data-stu-id="eeb73-182">In the project directory, open the **app.js** file.</span></span> <span data-ttu-id="eeb73-183">Den här filen skapades tidigare när Express-webbappen skapades.</span><span class="sxs-lookup"><span data-stu-id="eeb73-183">This file was created earlier when the Express web application was created.</span></span>
2. <span data-ttu-id="eeb73-184">Lägg till följande kod högst upp i **app.js**</span><span class="sxs-lookup"><span data-stu-id="eeb73-184">Add the following code to the top of **app.js**</span></span>
   
        var DocumentDBClient = require('documentdb').DocumentClient;
        var config = require('./config');
        var TaskList = require('./routes/tasklist');
        var TaskDao = require('./models/taskDao');
3. <span data-ttu-id="eeb73-185">Den här koden definierar vilken konfigurationsfil som ska användas och läser sedan värden från denna fil till några variabler som vi snart ska använda.</span><span class="sxs-lookup"><span data-stu-id="eeb73-185">This code defines the config file to be used, and proceeds to read values out of this file into some variables we will use soon.</span></span>
4. <span data-ttu-id="eeb73-186">Ersätt följande två rader i filen **app.js**:</span><span class="sxs-lookup"><span data-stu-id="eeb73-186">Replace the following two lines in **app.js** file:</span></span>
   
        app.use('/', index);
        app.use('/users', users); 
   
      <span data-ttu-id="eeb73-187">med följande utdrag:</span><span class="sxs-lookup"><span data-stu-id="eeb73-187">with the following snippet:</span></span>
   
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
5. <span data-ttu-id="eeb73-188">Dessa rader definierar en ny instans av **TaskDao**-objektet med en ny anslutning till Azure Cosmos DB (med de värden som lästs in från **config.js**), initierar aktivitetsobjektet och binder formuläråtgärder till metoder i **TaskList**-styrenheten.</span><span class="sxs-lookup"><span data-stu-id="eeb73-188">These lines define a new instance of our **TaskDao** object, with a new connection to Azure Cosmos DB (using the values read from the **config.js**), initialize the task object and then bind form actions to methods on our **TaskList** controller.</span></span> 
6. <span data-ttu-id="eeb73-189">Avsluta med att spara och stänga filen **app.js**. Vi är nästan klara.</span><span class="sxs-lookup"><span data-stu-id="eeb73-189">Finally, save and close the **app.js** file, we're just about done.</span></span>

## <span data-ttu-id="eeb73-190"><a name="_Toc395783181"></a>Steg 5: Skapa ett användargränssnitt</span><span class="sxs-lookup"><span data-stu-id="eeb73-190"><a name="_Toc395783181"></a>Step 5: Build a user interface</span></span>
<span data-ttu-id="eeb73-191">Nu är det dags att skapa användargränssnittet, så att användaren faktiskt kan samverka med vår app.</span><span class="sxs-lookup"><span data-stu-id="eeb73-191">Now let’s turn our attention to building the user interface so a user can actually interact with our application.</span></span> <span data-ttu-id="eeb73-192">Express-appen som vi skapade använder **Jade** som visningsmotor.</span><span class="sxs-lookup"><span data-stu-id="eeb73-192">The Express application we created uses **Jade** as the view engine.</span></span> <span data-ttu-id="eeb73-193">Mer information om Jade finns på [http://jade-lang.com/](http://jade-lang.com/).</span><span class="sxs-lookup"><span data-stu-id="eeb73-193">For more information on Jade please refer to [http://jade-lang.com/](http://jade-lang.com/).</span></span>

1. <span data-ttu-id="eeb73-194">Filen **layout.jade** i katalogen **views** används som en global mall för andra **.jade**-filer.</span><span class="sxs-lookup"><span data-stu-id="eeb73-194">The **layout.jade** file in the **views** directory is used as a global template for other **.jade** files.</span></span> <span data-ttu-id="eeb73-195">I det här steget ändrar du den så att den använder [Twitter Bootstrap](https://github.com/twbs/bootstrap), vilket är en verktygslåda som gör det enkelt att utforma en snygg webbplats.</span><span class="sxs-lookup"><span data-stu-id="eeb73-195">In this step you will modify it to use [Twitter Bootstrap](https://github.com/twbs/bootstrap), which is a toolkit that makes it easy to design a nice looking website.</span></span> 
2. <span data-ttu-id="eeb73-196">Öppna filen **layout.jade** i mappen **views** och ersätt innehållet med följande:</span><span class="sxs-lookup"><span data-stu-id="eeb73-196">Open the **layout.jade** file found in the **views** folder and replace the contents with the following:</span></span>

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

    <span data-ttu-id="eeb73-197">Det säger åt **Jade**-motorn att rendera en del HTML för vårt program och skapar ett **block** kallat **content** där vi kan tillhandahålla layout för våra innehållssidor.</span><span class="sxs-lookup"><span data-stu-id="eeb73-197">This effectively tells the **Jade** engine to render some HTML for our application and creates a **block** called **content** where we can supply the layout for our content pages.</span></span>

    <span data-ttu-id="eeb73-198">Spara och stäng filen **layout.jade**.</span><span class="sxs-lookup"><span data-stu-id="eeb73-198">Save and close this **layout.jade** file.</span></span>

3. <span data-ttu-id="eeb73-199">Öppna nu filen **index.jade**, den vy som ska användas av vårt program, och ersätt innehållet i filen med följande:</span><span class="sxs-lookup"><span data-stu-id="eeb73-199">Now open the **index.jade** file, the view that will be used by our application, and replace the content of the file with the following:</span></span>
   
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
   

<span data-ttu-id="eeb73-200">Det utökar layouten och ger innehåll för platshållaren **content** som vi såg i filen **layout.jade**.</span><span class="sxs-lookup"><span data-stu-id="eeb73-200">This extends layout, and provides content for the **content** placeholder we saw in the **layout.jade** file earlier.</span></span>
   
<span data-ttu-id="eeb73-201">I den här layouten skapade vi två HTML-formulär.</span><span class="sxs-lookup"><span data-stu-id="eeb73-201">In this layout we created two HTML forms.</span></span>

<span data-ttu-id="eeb73-202">Det första formuläret innehåller en tabell för våra data och en knapp som gör att vi kan uppdatera objekt genom att publicera till metoden **/completetask** i styrningen.</span><span class="sxs-lookup"><span data-stu-id="eeb73-202">The first form contains a table for our data and a button that allows us to update items by posting to **/completetask** method of our controller.</span></span>
    
<span data-ttu-id="eeb73-203">Det andra formuläret innehåller två inmatningsfält och en knapp som gör att vi kan skapa ett nytt objekt genom att publicera till metoden **/addtask** i styrningen.</span><span class="sxs-lookup"><span data-stu-id="eeb73-203">The second form contains two input fields and a button that allows us to create a new item by posting to **/addtask** method of our controller.</span></span>

<span data-ttu-id="eeb73-204">Det här ska vara allt som behövs för att appen ska fungera.</span><span class="sxs-lookup"><span data-stu-id="eeb73-204">This should be all that we need for our application to work.</span></span>

## <span data-ttu-id="eeb73-205"><a name="_Toc395783181"></a>Steg 6: Kör ditt program lokalt</span><span class="sxs-lookup"><span data-stu-id="eeb73-205"><a name="_Toc395783181"></a>Step 6: Run your application locally</span></span>
1. <span data-ttu-id="eeb73-206">Om du vill testa programmet på din lokala dator kör du `npm start` i terminalen för att starta programmet, uppdatera därefter din [http://localhost:3000](http://localhost:3000)-webbläsarsida.</span><span class="sxs-lookup"><span data-stu-id="eeb73-206">To test the application on your local machine, run `npm start` in the terminal to start your application, then refresh your [http://localhost:3000](http://localhost:3000) browser page.</span></span> <span data-ttu-id="eeb73-207">Sidan ska nu se ut som på bilden nedan:</span><span class="sxs-lookup"><span data-stu-id="eeb73-207">The page should now look like the image below:</span></span>
   
    ![Skärmdump av programmet MyTodo List i ett webbläsarfönster](./media/documentdb-nodejs-application/cosmos-db-node-js-localhost.png)

    > [!TIP]
    > <span data-ttu-id="eeb73-209">Om du får ett felmeddelande om indraget i layout.jade-filen eller index.jade-filen, säkerställ att de två första raderna i båda filerna är vänsterjusterade, utan blanksteg.</span><span class="sxs-lookup"><span data-stu-id="eeb73-209">If you receive an error about the indent in the layout.jade file or the index.jade file, ensure that the first two lines in both files is left justified, with no spaces.</span></span> <span data-ttu-id="eeb73-210">Om det finns blanksteg före de två första raderna, ta bort dem, spara filerna och uppdatera sedan webbläsarfönstret.</span><span class="sxs-lookup"><span data-stu-id="eeb73-210">If there are spaces before the first two lines, remove them, save both files, then refresh your browser window.</span></span> 

2. <span data-ttu-id="eeb73-211">Använd fälten Objekt, Objektnamn och Kategori och klicka sedan på **Lägg till objekt**.</span><span class="sxs-lookup"><span data-stu-id="eeb73-211">Use the Item, Item Name and Category fields to enter a new task and then click **Add Item**.</span></span> <span data-ttu-id="eeb73-212">När du gör det skapas ett dokument i Azure Cosmos DB med dessa egenskaper.</span><span class="sxs-lookup"><span data-stu-id="eeb73-212">This creates a document in Azure Cosmos DB with those properties.</span></span> 
3. <span data-ttu-id="eeb73-213">Sidan bör uppdateras och visa det nya objektet i ToDo-listan.</span><span class="sxs-lookup"><span data-stu-id="eeb73-213">The page should update to display the newly created item in the ToDo list.</span></span>
   
    ![Skärmdump av programmet med ett nytt objekt i ToDo-listan](./media/documentdb-nodejs-application/cosmos-db-node-js-added-task.png)
4. <span data-ttu-id="eeb73-215">Du slutför en aktivitet genom att markera kryssrutan i kolumnen Complete och sedan klicka på **Update tasks**.</span><span class="sxs-lookup"><span data-stu-id="eeb73-215">To complete a task, simply check the checkbox in the Complete column, and then click **Update tasks**.</span></span> <span data-ttu-id="eeb73-216">Detta uppdaterar det dokument som du redan har skapat.</span><span class="sxs-lookup"><span data-stu-id="eeb73-216">This updates the document you already created.</span></span>

5. <span data-ttu-id="eeb73-217">För att stoppa programmet trycker du på CTRL+C i terminalfönstret och klickar sedan på **Y** för att avbryta batch-jobbet.</span><span class="sxs-lookup"><span data-stu-id="eeb73-217">To stop the application, press CTRL+C in the terminal window and then click **Y** to terminate the batch job.</span></span>

## <span data-ttu-id="eeb73-218"><a name="_Toc395783182"></a>Steg 7: Distribuera ditt programutvecklingsprojekt till Azure Websites</span><span class="sxs-lookup"><span data-stu-id="eeb73-218"><a name="_Toc395783182"></a>Step 7: Deploy your application development project to Azure Websites</span></span>
1. <span data-ttu-id="eeb73-219">Om du inte redan gjort det aktiverar du en git-databas för Azure-webbplatsen.</span><span class="sxs-lookup"><span data-stu-id="eeb73-219">If you haven't already, enable a git repository for your Azure Website.</span></span> <span data-ttu-id="eeb73-220">Information om hur du gör det finns i artikeln [Lokal Git-distribuering på Azure App Service](../app-service-web/app-service-deploy-local-git.md).</span><span class="sxs-lookup"><span data-stu-id="eeb73-220">You can find instructions on how to do this in the [Local Git Deployment to Azure App Service](../app-service-web/app-service-deploy-local-git.md) topic.</span></span>
2. <span data-ttu-id="eeb73-221">Lägg till Azure-webbplatsen som en fjärransluten git.</span><span class="sxs-lookup"><span data-stu-id="eeb73-221">Add your Azure Website as a git remote.</span></span>
   
        git remote add azure https://username@your-azure-website.scm.azurewebsites.net:443/your-azure-website.git
3. <span data-ttu-id="eeb73-222">Distribuera genom att pusha till fjärranslutningen.</span><span class="sxs-lookup"><span data-stu-id="eeb73-222">Deploy by pushing to the remote.</span></span>
   
        git push azure master
4. <span data-ttu-id="eeb73-223">I några sekunder har git publicerat din webbapp och öppnar en webbläsare där du kan se ditt arbete som körs i Azure!</span><span class="sxs-lookup"><span data-stu-id="eeb73-223">In a few seconds, git will finish publishing your web application and launch a browser where you can see your handiwork running in Azure!</span></span>

    <span data-ttu-id="eeb73-224">Grattis!</span><span class="sxs-lookup"><span data-stu-id="eeb73-224">Congratulations!</span></span> <span data-ttu-id="eeb73-225">Du har skapat ditt första Node.js Express-webbprogram med Azure Cosmos DB och publicerat det på Azure Websites.</span><span class="sxs-lookup"><span data-stu-id="eeb73-225">You have just built your first Node.js Express Web Application using Azure Cosmos DB and published it to Azure Websites.</span></span>

    <span data-ttu-id="eeb73-226">Om du vill hämta eller referera till det färdiga referensprogrammet för den här självstudien kan det hämtas från [GitHub][GitHub].</span><span class="sxs-lookup"><span data-stu-id="eeb73-226">If you want to download or refer to the complete reference application for this tutorial, it can be downloaded from [GitHub][GitHub].</span></span>

## <span data-ttu-id="eeb73-227"><a name="_Toc395637775"></a>Nästa steg</span><span class="sxs-lookup"><span data-stu-id="eeb73-227"><a name="_Toc395637775"></a>Next steps</span></span>

* <span data-ttu-id="eeb73-228">Vill du testa skalning och prestanda med Azure Cosmos DB?</span><span class="sxs-lookup"><span data-stu-id="eeb73-228">Want to perform scale and performance testing with Azure Cosmos DB?</span></span> <span data-ttu-id="eeb73-229">Mer information finns i avsnittet om hur du [testar prestanda och skalning med Azure Cosmos DB](performance-testing.md)</span><span class="sxs-lookup"><span data-stu-id="eeb73-229">See [Performance and Scale Testing with Azure Cosmos DB](performance-testing.md)</span></span>
* <span data-ttu-id="eeb73-230">Lär dig hur du [övervakar ett Azure Cosmos DB-konto](monitor-accounts.md).</span><span class="sxs-lookup"><span data-stu-id="eeb73-230">Learn how to [monitor an Azure Cosmos DB account](monitor-accounts.md).</span></span>
* <span data-ttu-id="eeb73-231">Kör frågor mot vår exempeldatauppsättning i [Query Playground](https://www.documentdb.com/sql/demo).</span><span class="sxs-lookup"><span data-stu-id="eeb73-231">Run queries against our sample dataset in the [Query Playground](https://www.documentdb.com/sql/demo).</span></span>
* <span data-ttu-id="eeb73-232">Utforska [Azure Cosmos DB-dokumentationen](https://docs.microsoft.com/azure/documentdb/).</span><span class="sxs-lookup"><span data-stu-id="eeb73-232">Explore the [Azure Cosmos DB documentation](https://docs.microsoft.com/azure/documentdb/).</span></span>

[Node.js]: http://nodejs.org/
[Git]: http://git-scm.com/
[GitHub]: https://github.com/Azure-Samples/documentdb-node-todo-app

