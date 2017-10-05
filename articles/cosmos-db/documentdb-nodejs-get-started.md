---
title: "Självstudiekurs om Node.js för DocumentDB-API:et för Azure Cosmos DB | Microsoft-dokument"
description: "En självstudiekurs om Node.js som beskriver hur du skapar en Cosmos DB-databas med DocumentDB-API:et."
keywords: "självstudier för node.js, noddatabas"
services: cosmos-db
documentationcenter: node.js
author: AndrewHoh
manager: jhubbard
editor: monicar
ms.assetid: 14d52110-1dce-4ac0-9dd9-f936afccd550
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: node
ms.topic: article
ms.date: 08/14/2017
ms.author: anhoh
ms.openlocfilehash: 6510e0270bb2efa252a2b2ad40014c5d26b74a81
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="nodejs-tutorial-use-the-documentdb-api-in-azure-cosmos-db-to-create-a-nodejs-console-application"></a><span data-ttu-id="736e9-104">Självstudie om node.js: använda DocumentDB-API i Azure Cosmos-databasen för att skapa en Node.js-konsolprogram</span><span class="sxs-lookup"><span data-stu-id="736e9-104">Node.js tutorial: Use the DocumentDB API in Azure Cosmos DB to create a Node.js console application</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="736e9-105">.NET</span><span class="sxs-lookup"><span data-stu-id="736e9-105">.NET</span></span>](documentdb-get-started.md)
> * [<span data-ttu-id="736e9-106">.NET Core</span><span class="sxs-lookup"><span data-stu-id="736e9-106">.NET Core</span></span>](documentdb-dotnetcore-get-started.md)
> * [<span data-ttu-id="736e9-107">Node.js för MongoDB</span><span class="sxs-lookup"><span data-stu-id="736e9-107">Node.js for MongoDB</span></span>](mongodb-samples.md)
> * [<span data-ttu-id="736e9-108">Node.js</span><span class="sxs-lookup"><span data-stu-id="736e9-108">Node.js</span></span>](documentdb-nodejs-get-started.md)
> * [<span data-ttu-id="736e9-109">Java</span><span class="sxs-lookup"><span data-stu-id="736e9-109">Java</span></span>](documentdb-java-get-started.md)
> * [<span data-ttu-id="736e9-110">C++</span><span class="sxs-lookup"><span data-stu-id="736e9-110">C++</span></span>](documentdb-cpp-get-started.md)
>  
> 

<span data-ttu-id="736e9-111">Välkommen till självstudiekursen om Node.js för Azure Cosmos DB Node.js SDK!</span><span class="sxs-lookup"><span data-stu-id="736e9-111">Welcome to the Node.js tutorial for the Azure Cosmos DB Node.js SDK!</span></span> <span data-ttu-id="736e9-112">När du har genomfört den här självstudiekursen har du ett konsolprogram som skapar och skickar frågor till Azure Cosmos DB-resurser.</span><span class="sxs-lookup"><span data-stu-id="736e9-112">After following this tutorial, you'll have a console application that creates and queries Azure Cosmos DB resources.</span></span>

<span data-ttu-id="736e9-113">Vi tar upp följande:</span><span class="sxs-lookup"><span data-stu-id="736e9-113">We'll cover:</span></span>

* <span data-ttu-id="736e9-114">Skapa och ansluta till ett Azure Cosmos DB-konto</span><span class="sxs-lookup"><span data-stu-id="736e9-114">Creating and connecting to an Azure Cosmos DB account</span></span>
* <span data-ttu-id="736e9-115">Konfigurera ditt program</span><span class="sxs-lookup"><span data-stu-id="736e9-115">Setting up your application</span></span>
* <span data-ttu-id="736e9-116">Skapa en Node-databas</span><span class="sxs-lookup"><span data-stu-id="736e9-116">Creating a node database</span></span>
* <span data-ttu-id="736e9-117">Skapa en samling</span><span class="sxs-lookup"><span data-stu-id="736e9-117">Creating a collection</span></span>
* <span data-ttu-id="736e9-118">Skapa JSON-dokument</span><span class="sxs-lookup"><span data-stu-id="736e9-118">Creating JSON documents</span></span>
* <span data-ttu-id="736e9-119">Skicka frågor till samlingen</span><span class="sxs-lookup"><span data-stu-id="736e9-119">Querying the collection</span></span>
* <span data-ttu-id="736e9-120">Ersätta ett dokument</span><span class="sxs-lookup"><span data-stu-id="736e9-120">Replacing a document</span></span>
* <span data-ttu-id="736e9-121">Ta bort ett dokument</span><span class="sxs-lookup"><span data-stu-id="736e9-121">Deleting a document</span></span>
* <span data-ttu-id="736e9-122">Ta bort Node-databasen</span><span class="sxs-lookup"><span data-stu-id="736e9-122">Deleting the node database</span></span>

<span data-ttu-id="736e9-123">Har du inte tid?</span><span class="sxs-lookup"><span data-stu-id="736e9-123">Don't have time?</span></span> <span data-ttu-id="736e9-124">Oroa dig inte!</span><span class="sxs-lookup"><span data-stu-id="736e9-124">Don't worry!</span></span> <span data-ttu-id="736e9-125">Den kompletta lösningen finns på [GitHub](https://github.com/Azure-Samples/documentdb-node-getting-started).</span><span class="sxs-lookup"><span data-stu-id="736e9-125">The complete solution is available on [GitHub](https://github.com/Azure-Samples/documentdb-node-getting-started).</span></span> <span data-ttu-id="736e9-126">En snabbguide finns i [Hämta den kompletta lösningen](#GetSolution).</span><span class="sxs-lookup"><span data-stu-id="736e9-126">See [Get the complete solution](#GetSolution) for quick instructions.</span></span>

<span data-ttu-id="736e9-127">När du har genomfört självstudien om Node.js får du gärna ge oss feedback med röstningsknapparna högst upp och längst ned på den här sidan.</span><span class="sxs-lookup"><span data-stu-id="736e9-127">After you've completed the Node.js tutorial, please use the voting buttons at the top and bottom of this page to give us feedback.</span></span> <span data-ttu-id="736e9-128">Om du vill att vi ska kontakta dig direkt kan du skriva din e-postadress i kommentaren.</span><span class="sxs-lookup"><span data-stu-id="736e9-128">If you'd like us to contact you directly, feel free to include your email address in your comments.</span></span>

<span data-ttu-id="736e9-129">Nu sätter vi igång!</span><span class="sxs-lookup"><span data-stu-id="736e9-129">Now let's get started!</span></span>

## <a name="prerequisites-for-the-nodejs-tutorial"></a><span data-ttu-id="736e9-130">Förutsättningar för självstudien om Node.js</span><span class="sxs-lookup"><span data-stu-id="736e9-130">Prerequisites for the Node.js tutorial</span></span>
<span data-ttu-id="736e9-131">Se till att du har följande:</span><span class="sxs-lookup"><span data-stu-id="736e9-131">Please make sure you have the following:</span></span>

* <span data-ttu-id="736e9-132">Ett aktivt Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="736e9-132">An active Azure account.</span></span> <span data-ttu-id="736e9-133">Om du inte har ett kan du registrera dig för en [kostnadsfri utvärderingsversion av Azure](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="736e9-133">If you don't have one, you can sign up for a [Free Azure Trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>
    * <span data-ttu-id="736e9-134">Du kan också använda [Azure Cosmos DB-emulatorn](local-emulator.md) i den här självstudiekursen.</span><span class="sxs-lookup"><span data-stu-id="736e9-134">Alternatively, you can use the [Azure Cosmos DB Emulator](local-emulator.md) for this tutorial.</span></span>
* <span data-ttu-id="736e9-135">[Node.js](https://nodejs.org/) version 0.10.29 eller högre.</span><span class="sxs-lookup"><span data-stu-id="736e9-135">[Node.js](https://nodejs.org/) version v0.10.29 or higher.</span></span>

## <a name="step-1-create-an-azure-cosmos-db-account"></a><span data-ttu-id="736e9-136">Steg 1: Skapa ett Azure Cosmos DB-konto</span><span class="sxs-lookup"><span data-stu-id="736e9-136">Step 1: Create an Azure Cosmos DB account</span></span>
<span data-ttu-id="736e9-137">Nu ska vi skapa ett Azure Cosmos DB-konto.</span><span class="sxs-lookup"><span data-stu-id="736e9-137">Let's create an Azure Cosmos DB account.</span></span> <span data-ttu-id="736e9-138">Om du redan har ett konto som du vill använda kan du gå vidare till [konfigurera Node.js-programmet](#SetupNode).</span><span class="sxs-lookup"><span data-stu-id="736e9-138">If you already have an account you want to use, you can skip ahead to [Set up your Node.js application](#SetupNode).</span></span> <span data-ttu-id="736e9-139">Om du använder Azure Cosmos DB-emulatorn, följer du stegen i [Azure Cosmos DB emulatorn](local-emulator.md) konfigurera emulatorn och gå vidare till [konfigurera Node.js-programmet](#SetupNode).</span><span class="sxs-lookup"><span data-stu-id="736e9-139">If you are using the Azure Cosmos DB Emulator, please follow the steps at [Azure Cosmos DB Emulator](local-emulator.md) to setup the emulator and skip ahead to [Set up your Node.js application](#SetupNode).</span></span>

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

## <span data-ttu-id="736e9-140"><a id="SetupNode"></a>Steg 2: Konfigurera Node.js-programmet</span><span class="sxs-lookup"><span data-stu-id="736e9-140"><a id="SetupNode"></a>Step 2: Set up your Node.js application</span></span>
1. <span data-ttu-id="736e9-141">Öppna valfri terminal.</span><span class="sxs-lookup"><span data-stu-id="736e9-141">Open your favorite terminal.</span></span>
2. <span data-ttu-id="736e9-142">Leta reda på mappen eller katalogen där du vill spara Node.js-programmet.</span><span class="sxs-lookup"><span data-stu-id="736e9-142">Locate the folder or directory where you'd like to save your Node.js application.</span></span>
3. <span data-ttu-id="736e9-143">Skapa två tomma JavaScript-filer med följande kommandon:</span><span class="sxs-lookup"><span data-stu-id="736e9-143">Create two empty JavaScript files with the following commands:</span></span>
   * <span data-ttu-id="736e9-144">Windows:</span><span class="sxs-lookup"><span data-stu-id="736e9-144">Windows:</span></span>
     * ```fsutil file createnew app.js 0```
     * ```fsutil file createnew config.js 0```
   * <span data-ttu-id="736e9-145">Linux/OS X:</span><span class="sxs-lookup"><span data-stu-id="736e9-145">Linux/OS X:</span></span>
     * ```touch app.js```
     * ```touch config.js```
4. <span data-ttu-id="736e9-146">Installera modulen documentdb via npm.</span><span class="sxs-lookup"><span data-stu-id="736e9-146">Install the documentdb module via npm.</span></span> <span data-ttu-id="736e9-147">Ange följande kommando:</span><span class="sxs-lookup"><span data-stu-id="736e9-147">Use the following command:</span></span>
   * ```npm install documentdb --save```

<span data-ttu-id="736e9-148">Bra!</span><span class="sxs-lookup"><span data-stu-id="736e9-148">Great!</span></span> <span data-ttu-id="736e9-149">Nu när vi har slutfört installationen kan vi börja skriva kod.</span><span class="sxs-lookup"><span data-stu-id="736e9-149">Now that you've finished setting up, let's start writing some code.</span></span>

## <span data-ttu-id="736e9-150"><a id="Config"></a>Steg 3: Ange appkonfigurationer</span><span class="sxs-lookup"><span data-stu-id="736e9-150"><a id="Config"></a>Step 3: Set your app's configurations</span></span>
<span data-ttu-id="736e9-151">Öppna ```config.js``` i valfri textredigerare.</span><span class="sxs-lookup"><span data-stu-id="736e9-151">Open ```config.js``` in your favorite text editor.</span></span>

<span data-ttu-id="736e9-152">Sedan, kopiera och klistra in kodfragmentet nedan och ange egenskaper ```config.endpoint``` och ```config.primaryKey``` till din Azure Cosmos DB endpoint-uri och primärnyckel.</span><span class="sxs-lookup"><span data-stu-id="736e9-152">Then, copy and paste the code snippet below and set properties ```config.endpoint``` and ```config.primaryKey``` to your Azure Cosmos DB endpoint uri and primary key.</span></span> <span data-ttu-id="736e9-153">Båda dessa konfigurationer finns i den [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="736e9-153">Both these configurations can be found in the [Azure portal](https://portal.azure.com).</span></span>

![Självstudie om node.js – skärmdump av Azure portal som visar ett Azure DB som Cosmos-konto med hubben aktiv markerad, knappen nycklar markerad i bladet Azure DB som Cosmos-konto och värdena URI, PRIMÄRNYCKEL och SEKUNDÄRNYCKEL markerade i bladet nycklar – Node-databas][keys]

    // ADD THIS PART TO YOUR CODE
    var config = {}

    config.endpoint = "~your Azure Cosmos DB endpoint uri here~";
    config.primaryKey = "~your primary key here~";

<span data-ttu-id="736e9-155">Kopiera och klistra in ```database id```, ```collection id``` och ```JSON documents``` till ditt ```config```-objekt nedan där du anger ```config.endpoint```- och ```config.authKey```-egenskaper.</span><span class="sxs-lookup"><span data-stu-id="736e9-155">Copy and paste the ```database id```, ```collection id```, and ```JSON documents``` to your ```config``` object below where you set your ```config.endpoint``` and ```config.authKey``` properties.</span></span> <span data-ttu-id="736e9-156">Om du redan har data som du vill lagra i databasen kan du använda [datamigreringsverktyget](import-data.md) för Azure Cosmos DB i stället för att lägga till dokumentdefinitionerna.</span><span class="sxs-lookup"><span data-stu-id="736e9-156">If you already have data you'd like to store in your database, you can use Azure Cosmos DB's [Data Migration tool](import-data.md) rather than adding the document definitions.</span></span>

    config.endpoint = "~your Azure Cosmos DB endpoint uri here~";
    config.primaryKey = "~your primary key here~";

    // ADD THIS PART TO YOUR CODE
    config.database = {
        "id": "FamilyDB"
    };

    config.collection = {
        "id": "FamilyColl"
    };

    config.documents = {
        "Andersen": {
            "id": "Anderson.1",
            "lastName": "Andersen",
            "parents": [{
                "firstName": "Thomas"
            }, {
                    "firstName": "Mary Kay"
                }],
            "children": [{
                "firstName": "Henriette Thaulow",
                "gender": "female",
                "grade": 5,
                "pets": [{
                    "givenName": "Fluffy"
                }]
            }],
            "address": {
                "state": "WA",
                "county": "King",
                "city": "Seattle"
            }
        },
        "Wakefield": {
            "id": "Wakefield.7",
            "parents": [{
                "familyName": "Wakefield",
                "firstName": "Robin"
            }, {
                    "familyName": "Miller",
                    "firstName": "Ben"
                }],
            "children": [{
                "familyName": "Merriam",
                "firstName": "Jesse",
                "gender": "female",
                "grade": 8,
                "pets": [{
                    "givenName": "Goofy"
                }, {
                        "givenName": "Shadow"
                    }]
            }, {
                    "familyName": "Miller",
                    "firstName": "Lisa",
                    "gender": "female",
                    "grade": 1
                }],
            "address": {
                "state": "NY",
                "county": "Manhattan",
                "city": "NY"
            },
            "isRegistered": false
        }
    };


<span data-ttu-id="736e9-157">Databasen, samlingen och dokumentdefinitionerna kommer att fungera som Azure Cosmos-DB ```database id```, ```collection id```, och dokumentens data.</span><span class="sxs-lookup"><span data-stu-id="736e9-157">The database, collection, and document definitions will act as your Azure Cosmos DB ```database id```, ```collection id```, and documents' data.</span></span>

<span data-ttu-id="736e9-158">Exportera slutligen ```config```-objektet, så att du kan referera till det i filen ```app.js```.</span><span class="sxs-lookup"><span data-stu-id="736e9-158">Finally, export your ```config``` object, so that you can reference it within the ```app.js``` file.</span></span>

            },
            "isRegistered": false
        }
    };

    // ADD THIS PART TO YOUR CODE
    module.exports = config;

## <span data-ttu-id="736e9-159"><a id="Connect"></a>Steg 4: Ansluta till ett Azure Cosmos DB-konto</span><span class="sxs-lookup"><span data-stu-id="736e9-159"><a id="Connect"></a> Step 4: Connect to an Azure Cosmos DB account</span></span>
<span data-ttu-id="736e9-160">Öppna den tomma filen ```app.js``` i textredigeraren.</span><span class="sxs-lookup"><span data-stu-id="736e9-160">Open your empty ```app.js``` file in the text editor.</span></span> <span data-ttu-id="736e9-161">Kopiera och klistra in koden nedan för att importera modulen ```documentdb``` och modulen ```config``` som du just skapade.</span><span class="sxs-lookup"><span data-stu-id="736e9-161">Copy and paste the code below to import the ```documentdb``` module and your newly created ```config``` module.</span></span>

    // ADD THIS PART TO YOUR CODE
    "use strict";

    var documentClient = require("documentdb").DocumentClient;
    var config = require("./config");
    var url = require('url');

<span data-ttu-id="736e9-162">Kopiera och klistra in koden för att använda ```config.endpoint``` och ```config.primaryKey``` som du tidigare skapade och för att skapa en ny dokumentklient.</span><span class="sxs-lookup"><span data-stu-id="736e9-162">Copy and paste the code to use the previously saved ```config.endpoint``` and ```config.primaryKey``` to create a new DocumentClient.</span></span>

    var config = require("./config");
    var url = require('url');

    // ADD THIS PART TO YOUR CODE
    var client = new documentClient(config.endpoint, { "masterKey": config.primaryKey });

<span data-ttu-id="736e9-163">Nu när du har koden för att initiera Azure DB som Cosmos-klienten ska vi titta på arbeta med Azure Cosmos DB resurser.</span><span class="sxs-lookup"><span data-stu-id="736e9-163">Now that you have the code to initialize the Azure Cosmos DB client, let's take a look at working with Azure Cosmos DB resources.</span></span>

## <a name="step-5-create-a-node-database"></a><span data-ttu-id="736e9-164">Steg 5: Skapa en Node-databas</span><span class="sxs-lookup"><span data-stu-id="736e9-164">Step 5: Create a Node database</span></span>
<span data-ttu-id="736e9-165">Kopiera och klistra in koden nedan för att ange HTTP-status för Kunde inte hittas, databasens URL och samlingens URL.</span><span class="sxs-lookup"><span data-stu-id="736e9-165">Copy and paste the code below to set the HTTP status for Not Found, the database url, and the collection url.</span></span> <span data-ttu-id="736e9-166">Dessa URL: er är hur Azure DB som Cosmos-klienten hittar rätt databas och samling.</span><span class="sxs-lookup"><span data-stu-id="736e9-166">These urls are how the Azure Cosmos DB client will find the right database and collection.</span></span>

    var client = new documentClient(config.endpoint, { "masterKey": config.primaryKey });

    // ADD THIS PART TO YOUR CODE
    var HttpStatusCodes = { NOTFOUND: 404 };
    var databaseUrl = `dbs/${config.database.id}`;
    var collectionUrl = `${databaseUrl}/colls/${config.collection.id}`;

<span data-ttu-id="736e9-167">Du kan skapa en [databas](documentdb-resources.md#databases) med funktionen [createDatabase](https://azure.github.io/azure-documentdb-node/DocumentClient.html) för klassen **DocumentClient**.</span><span class="sxs-lookup"><span data-stu-id="736e9-167">A [database](documentdb-resources.md#databases) can be created by using the [createDatabase](https://azure.github.io/azure-documentdb-node/DocumentClient.html) function of the **DocumentClient** class.</span></span> <span data-ttu-id="736e9-168">En databas är en logisk behållare för dokumentlagring, partitionerad över samlingarna.</span><span class="sxs-lookup"><span data-stu-id="736e9-168">A database is the logical container of document storage partitioned across collections.</span></span>

<span data-ttu-id="736e9-169">Kopiera och klistra in funktionen **getDatabase** för att skapa den nya databasen i filen app.js med ```id``` som anges i objektet ```config```.</span><span class="sxs-lookup"><span data-stu-id="736e9-169">Copy and paste the **getDatabase** function for creating your new database in the app.js file with the ```id``` specified in the ```config``` object.</span></span> <span data-ttu-id="736e9-170">Funktionen kommer att kontrollera om en databas med samma ```FamilyRegistry```-id redan finns.</span><span class="sxs-lookup"><span data-stu-id="736e9-170">The function will check if the database with the same ```FamilyRegistry``` id does not already exist.</span></span> <span data-ttu-id="736e9-171">Om den finns returnerar vi den databasen istället för att skapa en ny.</span><span class="sxs-lookup"><span data-stu-id="736e9-171">If it does exist, we'll return that database instead of creating a new one.</span></span>

    var collectionUrl = `${databaseUrl}/colls/${config.collection.id}`;

    // ADD THIS PART TO YOUR CODE
    function getDatabase() {
        console.log(`Getting database:\n${config.database.id}\n`);

        return new Promise((resolve, reject) => {
            client.readDatabase(databaseUrl, (err, result) => {
                if (err) {
                    if (err.code == HttpStatusCodes.NOTFOUND) {
                        client.createDatabase(config.database, (err, created) => {
                            if (err) reject(err)
                            else resolve(created);
                        });
                    } else {
                        reject(err);
                    }
                } else {
                    resolve(result);
                }
            });
        });
    }

<span data-ttu-id="736e9-172">Kopiera och klistra in koden nedan där du konfigurerar funktionen **getDatabase** att lägga till hjälpfunktionen **exit** som skriver stängningsmeddelandet och anropet till funktionen **getDatabase**.</span><span class="sxs-lookup"><span data-stu-id="736e9-172">Copy and paste the code below where you set the **getDatabase** function to add the helper function **exit** that will print the exit message and the call to **getDatabase** function.</span></span>

                } else {
                    resolve(result);
                }
            });
        });
    }

    // ADD THIS PART TO YOUR CODE
    function exit(message) {
        console.log(message);
        console.log('Press any key to exit');
        process.stdin.setRawMode(true);
        process.stdin.resume();
        process.stdin.on('data', process.exit.bind(process, 0));
    }

    getDatabase()
    .then(() => { exit(`Completed successfully`); })
    .catch((error) => { exit(`Completed with error ${JSON.stringify(error)}`) });

<span data-ttu-id="736e9-173">Leta upp filen ```app.js``` i terminalen och kör kommandot: ```node app.js```</span><span class="sxs-lookup"><span data-stu-id="736e9-173">In your terminal, locate your ```app.js``` file and run the command: ```node app.js```</span></span>

<span data-ttu-id="736e9-174">Grattis!</span><span class="sxs-lookup"><span data-stu-id="736e9-174">Congratulations!</span></span> <span data-ttu-id="736e9-175">Du har skapat en Azure Cosmos DB-databas.</span><span class="sxs-lookup"><span data-stu-id="736e9-175">You have successfully created an Azure Cosmos DB database.</span></span>

## <span data-ttu-id="736e9-176"><a id="CreateColl"></a>Steg 6: Skapa en samling</span><span class="sxs-lookup"><span data-stu-id="736e9-176"><a id="CreateColl"></a>Step 6: Create a collection</span></span>
> [!WARNING]
> <span data-ttu-id="736e9-177">**CreateDocumentCollectionAsync** skapar en ny samling, vilket får konsekvenser för priset.</span><span class="sxs-lookup"><span data-stu-id="736e9-177">**CreateDocumentCollectionAsync** will create a new collection, which has pricing implications.</span></span> <span data-ttu-id="736e9-178">Mer information finns på vår [sida med priser](https://azure.microsoft.com/pricing/details/cosmos-db/).</span><span class="sxs-lookup"><span data-stu-id="736e9-178">For more details, please visit our [pricing page](https://azure.microsoft.com/pricing/details/cosmos-db/).</span></span>
> 
> 

<span data-ttu-id="736e9-179">En [samling](documentdb-resources.md#collections) kan skapas med funktionen [createCollection](https://azure.github.io/azure-documentdb-node/DocumentClient.html) för klassen **DocumentClient**.</span><span class="sxs-lookup"><span data-stu-id="736e9-179">A [collection](documentdb-resources.md#collections) can be created by using the [createCollection](https://azure.github.io/azure-documentdb-node/DocumentClient.html) function of the **DocumentClient** class.</span></span> <span data-ttu-id="736e9-180">En samling är en behållare för JSON-dokument och associerad JavaScript-applogik.</span><span class="sxs-lookup"><span data-stu-id="736e9-180">A collection is a container of JSON documents and associated JavaScript application logic.</span></span>

<span data-ttu-id="736e9-181">Kopiera och klistra in funktionen **getCollection** under funktionen **getDatabase** i app.js-filen för att skapa den nya samlingen med ```id``` som anges i objektet ```config```.</span><span class="sxs-lookup"><span data-stu-id="736e9-181">Copy and paste the **getCollection** function underneath the **getDatabase** function in the app.js file to create your new collection with the ```id``` specified in the ```config``` object.</span></span> <span data-ttu-id="736e9-182">Vi kontrollerar igen att det inte redan finns en samling med samma ```FamilyCollection```-id.</span><span class="sxs-lookup"><span data-stu-id="736e9-182">Again, we'll check to make sure a collection with the same ```FamilyCollection``` id does not already exist.</span></span> <span data-ttu-id="736e9-183">Om det gör det returnerar vi den samlingen istället för att skapa en ny.</span><span class="sxs-lookup"><span data-stu-id="736e9-183">If it does exist, we'll return that collection instead of creating a new one.</span></span>

                } else {
                    resolve(result);
                }
            });
        });
    }

    // ADD THIS PART TO YOUR CODE
    function getCollection() {
        console.log(`Getting collection:\n${config.collection.id}\n`);

        return new Promise((resolve, reject) => {
            client.readCollection(collectionUrl, (err, result) => {
                if (err) {
                    if (err.code == HttpStatusCodes.NOTFOUND) {
                        client.createCollection(databaseUrl, config.collection, { offerThroughput: 400 }, (err, created) => {
                            if (err) reject(err)
                            else resolve(created);
                        });
                    } else {
                        reject(err);
                    }
                } else {
                    resolve(result);
                }
            });
        });
    }

<span data-ttu-id="736e9-184">Kopiera och klistra in koden under anropet till **getDatabase** för att köra funktionen **getCollection**.</span><span class="sxs-lookup"><span data-stu-id="736e9-184">Copy and paste the code below the call to **getDatabase** to execute the **getCollection** function.</span></span>

    getDatabase()

    // ADD THIS PART TO YOUR CODE
    .then(() => getCollection())
    // ENDS HERE

    .then(() => { exit(`Completed successfully`); })
    .catch((error) => { exit(`Completed with error ${JSON.stringify(error)}`) });

<span data-ttu-id="736e9-185">Leta upp filen ```app.js``` i terminalen och kör kommandot: ```node app.js```</span><span class="sxs-lookup"><span data-stu-id="736e9-185">In your terminal, locate your ```app.js``` file and run the command: ```node app.js```</span></span>

<span data-ttu-id="736e9-186">Grattis!</span><span class="sxs-lookup"><span data-stu-id="736e9-186">Congratulations!</span></span> <span data-ttu-id="736e9-187">Du har skapat en Azure DB som Cosmos-samling.</span><span class="sxs-lookup"><span data-stu-id="736e9-187">You have successfully created an Azure Cosmos DB collection.</span></span>

## <span data-ttu-id="736e9-188"><a id="CreateDoc"></a>Steg 7: Skapa ett dokument</span><span class="sxs-lookup"><span data-stu-id="736e9-188"><a id="CreateDoc"></a>Step 7: Create a document</span></span>
<span data-ttu-id="736e9-189">Du kan skapa ett [dokument](documentdb-resources.md#documents) med metoden [createDocument](https://azure.github.io/azure-documentdb-node/DocumentClient.html) för klassen **DocumentClient**.</span><span class="sxs-lookup"><span data-stu-id="736e9-189">A [document](documentdb-resources.md#documents) can be created by using the [createDocument](https://azure.github.io/azure-documentdb-node/DocumentClient.html) function of the **DocumentClient** class.</span></span> <span data-ttu-id="736e9-190">Dokument är användardefinierat (godtyckligt) JSON-innehåll.</span><span class="sxs-lookup"><span data-stu-id="736e9-190">Documents are user defined (arbitrary) JSON content.</span></span> <span data-ttu-id="736e9-191">Nu kan du infoga ett dokument i Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="736e9-191">You can now insert a document into Azure Cosmos DB.</span></span>

<span data-ttu-id="736e9-192">Kopiera och klistra in funktionen **getFamilyDocument** under funktionen **getCollection** för att skapa dokumenten med de JSON-data som sparas i objektet ```config```.</span><span class="sxs-lookup"><span data-stu-id="736e9-192">Copy and paste the **getFamilyDocument** function underneath the **getCollection** function for creating the documents containing the JSON data saved in the ```config``` object.</span></span> <span data-ttu-id="736e9-193">Vi kontrollerar igen att det inte redan finns ett dokument med samma id.</span><span class="sxs-lookup"><span data-stu-id="736e9-193">Again, we'll check to make sure a document with the same id does not already exist.</span></span>

                } else {
                    resolve(result);
                }
            });
        });
    }

    // ADD THIS PART TO YOUR CODE
    function getFamilyDocument(document) {
        let documentUrl = `${collectionUrl}/docs/${document.id}`;
        console.log(`Getting document:\n${document.id}\n`);

        return new Promise((resolve, reject) => {
            client.readDocument(documentUrl, { partitionKey: document.district }, (err, result) => {
                if (err) {
                    if (err.code == HttpStatusCodes.NOTFOUND) {
                        client.createDocument(collectionUrl, document, (err, created) => {
                            if (err) reject(err)
                            else resolve(created);
                        });
                    } else {
                        reject(err);
                    }
                } else {
                    resolve(result);
                }
            });
        });
    };

<span data-ttu-id="736e9-194">Kopiera och klistra in koden under anropet till **getCollection** för att köra funktionen **getFamilyDocument**.</span><span class="sxs-lookup"><span data-stu-id="736e9-194">Copy and paste the code below the call to **getCollection** to execute the **getFamilyDocument** function.</span></span>

    getDatabase()
    .then(() => getCollection())

    // ADD THIS PART TO YOUR CODE
    .then(() => getFamilyDocument(config.documents.Andersen))
    .then(() => getFamilyDocument(config.documents.Wakefield))
    // ENDS HERE

    .then(() => { exit(`Completed successfully`); })
    .catch((error) => { exit(`Completed with error ${JSON.stringify(error)}`) });

<span data-ttu-id="736e9-195">Leta upp filen ```app.js``` i terminalen och kör kommandot: ```node app.js```</span><span class="sxs-lookup"><span data-stu-id="736e9-195">In your terminal, locate your ```app.js``` file and run the command: ```node app.js```</span></span>

<span data-ttu-id="736e9-196">Grattis!</span><span class="sxs-lookup"><span data-stu-id="736e9-196">Congratulations!</span></span> <span data-ttu-id="736e9-197">Du har skapat ett Azure DB som Cosmos-dokument.</span><span class="sxs-lookup"><span data-stu-id="736e9-197">You have successfully created an Azure Cosmos DB document.</span></span>

![Självstudie om Node.js – Diagram som illustrerar den hierarkiska relationen mellan kontot, databasen, samlingen och dokumenten – Node-databas](./media/documentdb-nodejs-get-started/node-js-tutorial-cosmos-db-account.png)

## <span data-ttu-id="736e9-199"><a id="Query"></a>Steg 8: Skicka frågor mot Azure Cosmos DB-resurser</span><span class="sxs-lookup"><span data-stu-id="736e9-199"><a id="Query"></a>Step 8: Query Azure Cosmos DB resources</span></span>
<span data-ttu-id="736e9-200">Azure Cosmos DB stöder [komplexa frågor](documentdb-sql-query.md) mot JSON-dokument som lagras i varje samling.</span><span class="sxs-lookup"><span data-stu-id="736e9-200">Azure Cosmos DB supports [rich queries](documentdb-sql-query.md) against JSON documents stored in each collection.</span></span> <span data-ttu-id="736e9-201">Följande exempelkod visar en fråga som du kan köra mot dokumenten i samlingen.</span><span class="sxs-lookup"><span data-stu-id="736e9-201">The following sample code shows a query that you can run against the documents in your collection.</span></span>

<span data-ttu-id="736e9-202">Kopiera och klistra in funktionen **queryCollection** under funktionen **getFamilyDocument** i app.js-filen.</span><span class="sxs-lookup"><span data-stu-id="736e9-202">Copy and paste the **queryCollection** function underneath the **getFamilyDocument** function in the app.js file.</span></span> <span data-ttu-id="736e9-203">Azure Cosmos-DB stöder SQL-liknande frågor som visas nedan.</span><span class="sxs-lookup"><span data-stu-id="736e9-203">Azure Cosmos DB supports SQL-like queries as shown below.</span></span> <span data-ttu-id="736e9-204">Mer information om hur du skapar komplexa frågor finns i [Query Playground](https://www.documentdb.com/sql/demo) och [frågedokumentationen](documentdb-sql-query.md).</span><span class="sxs-lookup"><span data-stu-id="736e9-204">For more information on building complex queries, check out the [Query Playground](https://www.documentdb.com/sql/demo) and the [query documentation](documentdb-sql-query.md).</span></span>

                } else {
                    resolve(result);
                }
            });
        });
    }

    // ADD THIS PART TO YOUR CODE
    function queryCollection() {
        console.log(`Querying collection through index:\n${config.collection.id}`);

        return new Promise((resolve, reject) => {
            client.queryDocuments(
                collectionUrl,
                'SELECT VALUE r.children FROM root r WHERE r.lastName = "Andersen"'
            ).toArray((err, results) => {
                if (err) reject(err)
                else {
                    for (var queryResult of results) {
                        let resultString = JSON.stringify(queryResult);
                        console.log(`\tQuery returned ${resultString}`);
                    }
                    console.log();
                    resolve(results);
                }
            });
        });
    };


<span data-ttu-id="736e9-205">Följande diagram illustrerar hur Azure Cosmos-Databasens SQL-frågesyntaxen anropas mot samlingen du skapade.</span><span class="sxs-lookup"><span data-stu-id="736e9-205">The following diagram illustrates how the Azure Cosmos DB SQL query syntax is called against the collection you created.</span></span>

![Självstudie om Node.js – Diagram som illustrerar frågans omfång och innebörd – Node-databas](./media/documentdb-nodejs-get-started/node-js-tutorial-collection-documents.png)

<span data-ttu-id="736e9-207">Den [FROM](documentdb-sql-query.md#FromClause) nyckelord är valfritt i frågan eftersom Azure DB som Cosmos-frågor redan är begränsade till en enda samling.</span><span class="sxs-lookup"><span data-stu-id="736e9-207">The [FROM](documentdb-sql-query.md#FromClause) keyword is optional in the query because Azure Cosmos DB queries are already scoped to a single collection.</span></span> <span data-ttu-id="736e9-208">”FROM Families f” kan därför bytas mot ”FROM root r” eller annat valfritt variabelnamn som du väljer.</span><span class="sxs-lookup"><span data-stu-id="736e9-208">Therefore, "FROM Families f" can be swapped with "FROM root r", or any other variable name you choose.</span></span> <span data-ttu-id="736e9-209">Azure Cosmos-DB kommer härleda att familjer, roten eller variabelnamnet som du har valt, referera till den aktuella samlingen som standard.</span><span class="sxs-lookup"><span data-stu-id="736e9-209">Azure Cosmos DB will infer that Families, root, or the variable name you chose, reference the current collection by default.</span></span>

<span data-ttu-id="736e9-210">Kopiera och klistra in koden under anropet till **getFamilyDocument** för att köra funktionen **queryCollection**.</span><span class="sxs-lookup"><span data-stu-id="736e9-210">Copy and paste the code below the call to **getFamilyDocument** to execute the **queryCollection** function.</span></span>

    .then(() => getFamilyDocument(config.documents.Andersen))
    .then(() => getFamilyDocument(config.documents.Wakefield))

    // ADD THIS PART TO YOUR CODE
    .then(() => queryCollection())
    // ENDS HERE

    .then(() => { exit(`Completed successfully`); })
    .catch((error) => { exit(`Completed with error ${JSON.stringify(error)}`) });

<span data-ttu-id="736e9-211">Leta upp filen ```app.js``` i terminalen och kör kommandot: ```node app.js```</span><span class="sxs-lookup"><span data-stu-id="736e9-211">In your terminal, locate your ```app.js``` file and run the command: ```node app.js```</span></span>

<span data-ttu-id="736e9-212">Grattis!</span><span class="sxs-lookup"><span data-stu-id="736e9-212">Congratulations!</span></span> <span data-ttu-id="736e9-213">Du har skickat frågor mot Azure DB Cosmos-dokument.</span><span class="sxs-lookup"><span data-stu-id="736e9-213">You have successfully queried Azure Cosmos DB documents.</span></span>

## <span data-ttu-id="736e9-214"><a id="ReplaceDocument"></a>Steg 9: Ersätta ett dokument</span><span class="sxs-lookup"><span data-stu-id="736e9-214"><a id="ReplaceDocument"></a>Step 9: Replace a document</span></span>
<span data-ttu-id="736e9-215">Azure Cosmos DB har stöd för ersättning av JSON-dokument.</span><span class="sxs-lookup"><span data-stu-id="736e9-215">Azure Cosmos DB supports replacing JSON documents.</span></span>

<span data-ttu-id="736e9-216">Kopiera och klistra in funktionen **replaceFamilyDocument** under funktionen **queryCollection** i app.js-filen.</span><span class="sxs-lookup"><span data-stu-id="736e9-216">Copy and paste the **replaceFamilyDocument** function underneath the **queryCollection** function in the app.js file.</span></span>

                    }
                    console.log();
                    resolve(result);
                }
            });
        });
    }

    // ADD THIS PART TO YOUR CODE
    function replaceFamilyDocument(document) {
        let documentUrl = `${collectionUrl}/docs/${document.id}`;
        console.log(`Replacing document:\n${document.id}\n`);
        document.children[0].grade = 6;

        return new Promise((resolve, reject) => {
            client.replaceDocument(documentUrl, document, (err, result) => {
                if (err) reject(err);
                else {
                    resolve(result);
                }
            });
        });
    };

<span data-ttu-id="736e9-217">Kopiera och klistra in koden under anropet till **queryCollection** för att köra funktionen **replaceDocument**.</span><span class="sxs-lookup"><span data-stu-id="736e9-217">Copy and paste the code below the call to **queryCollection** to execute the **replaceDocument** function.</span></span> <span data-ttu-id="736e9-218">Lägg även till kod för att anropa **queryCollection** på nytt för att kontrollera att dokumentet har ändrats.</span><span class="sxs-lookup"><span data-stu-id="736e9-218">Also, add the code to call **queryCollection** again to verify that the document had successfully changed.</span></span>

    .then(() => getFamilyDocument(config.documents.Andersen))
    .then(() => getFamilyDocument(config.documents.Wakefield))
    .then(() => queryCollection())

    // ADD THIS PART TO YOUR CODE
    .then(() => replaceFamilyDocument(config.documents.Andersen))
    .then(() => queryCollection())
    // ENDS HERE

    .then(() => { exit(`Completed successfully`); })
    .catch((error) => { exit(`Completed with error ${JSON.stringify(error)}`) });

<span data-ttu-id="736e9-219">Leta upp filen ```app.js``` i terminalen och kör kommandot: ```node app.js```</span><span class="sxs-lookup"><span data-stu-id="736e9-219">In your terminal, locate your ```app.js``` file and run the command: ```node app.js```</span></span>

<span data-ttu-id="736e9-220">Grattis!</span><span class="sxs-lookup"><span data-stu-id="736e9-220">Congratulations!</span></span> <span data-ttu-id="736e9-221">Du har ersatt ett Azure Cosmos DB-dokument.</span><span class="sxs-lookup"><span data-stu-id="736e9-221">You have successfully replaced an Azure Cosmos DB document.</span></span>

## <span data-ttu-id="736e9-222"><a id="DeleteDocument"></a>Steg 10: Ta bort ett dokument</span><span class="sxs-lookup"><span data-stu-id="736e9-222"><a id="DeleteDocument"></a>Step 10: Delete a document</span></span>
<span data-ttu-id="736e9-223">Azure Cosmos DB har stöd för borttagning av JSON-dokument.</span><span class="sxs-lookup"><span data-stu-id="736e9-223">Azure Cosmos DB supports deleting JSON documents.</span></span>

<span data-ttu-id="736e9-224">Kopiera och klistra in funktionen **deleteFamilyDocument** under funktionen **replaceFamilyDocument**.</span><span class="sxs-lookup"><span data-stu-id="736e9-224">Copy and paste the **deleteFamilyDocument** function underneath the **replaceFamilyDocument** function.</span></span>

                else {
                    resolve(result);
                }
            });
        });
    };

    // ADD THIS PART TO YOUR CODE
    function deleteFamilyDocument(document) {
        let documentUrl = `${collectionUrl}/docs/${document.id}`;
        console.log(`Deleting document:\n${document.id}\n`);

        return new Promise((resolve, reject) => {
            client.deleteDocument(documentUrl, (err, result) => {
                if (err) reject(err);
                else {
                    resolve(result);
                }
            });
        });
    };

<span data-ttu-id="736e9-225">Kopiera och klistra in koden under anropet till den andra **queryCollection** för att köra funktionen **deleteDocument**.</span><span class="sxs-lookup"><span data-stu-id="736e9-225">Copy and paste the code below the call to the second **queryCollection** to execute the **deleteDocument** function.</span></span>

    .then(() => queryCollection())
    .then(() => replaceFamilyDocument(config.documents.Andersen))
    .then(() => queryCollection())

    // ADD THIS PART TO YOUR CODE
    .then(() => deleteFamilyDocument(config.documents.Andersen))
    // ENDS HERE

    .then(() => { exit(`Completed successfully`); })
    .catch((error) => { exit(`Completed with error ${JSON.stringify(error)}`) });

<span data-ttu-id="736e9-226">Leta upp filen ```app.js``` i terminalen och kör kommandot: ```node app.js```</span><span class="sxs-lookup"><span data-stu-id="736e9-226">In your terminal, locate your ```app.js``` file and run the command: ```node app.js```</span></span>

<span data-ttu-id="736e9-227">Grattis!</span><span class="sxs-lookup"><span data-stu-id="736e9-227">Congratulations!</span></span> <span data-ttu-id="736e9-228">Du har tagit bort ett Azure Cosmos DB-dokument.</span><span class="sxs-lookup"><span data-stu-id="736e9-228">You have successfully deleted an Azure Cosmos DB document.</span></span>

## <span data-ttu-id="736e9-229"><a id="DeleteDatabase"></a>Steg 11: Ta bort Node-databasen</span><span class="sxs-lookup"><span data-stu-id="736e9-229"><a id="DeleteDatabase"></a>Step 11: Delete the Node database</span></span>
<span data-ttu-id="736e9-230">Om du tar bort databasen du skapade försvinner databasen och alla underordnade resurser (t.ex. samlingar och dokument).</span><span class="sxs-lookup"><span data-stu-id="736e9-230">Deleting the created database will remove the database and all children resources (collections, documents, etc.).</span></span>

<span data-ttu-id="736e9-231">Kopiera och klistra in funktionen **cleanup** under funktionen **deleteFamilyDocument** för att ta bort databasen och alla underordnade resurser.</span><span class="sxs-lookup"><span data-stu-id="736e9-231">Copy and paste the **cleanup** function underneath the **deleteFamilyDocument** function to remove the database and all the children resources.</span></span>

                else {
                    resolve(result);
                }
            });
        });
    };

    // ADD THIS PART TO YOUR CODE
    function cleanup() {
        console.log(`Cleaning up by deleting database ${config.database.id}`);

        return new Promise((resolve, reject) => {
            client.deleteDatabase(databaseUrl, (err) => {
                if (err) reject(err)
                else resolve(null);
            });
        });
    }

<span data-ttu-id="736e9-232">Kopiera och klistra in koden under anropet till **deleteFamilyDocument** för att köra funktionen **cleanup**.</span><span class="sxs-lookup"><span data-stu-id="736e9-232">Copy and paste the code below the call to **deleteFamilyDocument** to execute the **cleanup** function.</span></span>

    .then(() => deleteFamilyDocument(config.documents.Andersen))

    // ADD THIS PART TO YOUR CODE
    .then(() => cleanup())
    // ENDS HERE

    .then(() => { exit(`Completed successfully`); })
    .catch((error) => { exit(`Completed with error ${JSON.stringify(error)}`) });

## <span data-ttu-id="736e9-233"><a id="Run"></a>Steg 12: Kör Node.js-programmet i sin helhet!</span><span class="sxs-lookup"><span data-stu-id="736e9-233"><a id="Run"></a>Step 12: Run your Node.js application all together!</span></span>
<span data-ttu-id="736e9-234">Hela sekvensen för att anropa dina funktioner bör se ut så här:</span><span class="sxs-lookup"><span data-stu-id="736e9-234">Altogether, the sequence for calling your functions should look like this:</span></span>

    getDatabase()
    .then(() => getCollection())
    .then(() => getFamilyDocument(config.documents.Andersen))
    .then(() => getFamilyDocument(config.documents.Wakefield))
    .then(() => queryCollection())
    .then(() => replaceFamilyDocument(config.documents.Andersen))
    .then(() => queryCollection())
    .then(() => deleteFamilyDocument(config.documents.Andersen))
    .then(() => cleanup())
    .then(() => { exit(`Completed successfully`); })
    .catch((error) => { exit(`Completed with error ${JSON.stringify(error)}`) });

<span data-ttu-id="736e9-235">Leta upp filen ```app.js``` i terminalen och kör kommandot: ```node app.js```</span><span class="sxs-lookup"><span data-stu-id="736e9-235">In your terminal, locate your ```app.js``` file and run the command: ```node app.js```</span></span>

<span data-ttu-id="736e9-236">Du bör nu se utdata från din kom-igång-app.</span><span class="sxs-lookup"><span data-stu-id="736e9-236">You should see the output of your get started app.</span></span> <span data-ttu-id="736e9-237">Dina utdata bör motsvara exempeltexten nedan.</span><span class="sxs-lookup"><span data-stu-id="736e9-237">The output should match the example text below.</span></span>

    Getting database:
    FamilyDB

    Getting collection:
    FamilyColl

    Getting document:
    Anderson.1

    Getting document:
    Wakefield.7

    Querying collection through index:
    FamilyColl
        Query returned [{"firstName":"Henriette Thaulow","gender":"female","grade":5,"pets":[{"givenName":"Fluffy"}]}]

    Replacing document:
    Anderson.1

    Querying collection through index:
    FamilyColl
        Query returned [{"firstName":"Henriette Thaulow","gender":"female","grade":6,"pets":[{"givenName":"Fluffy"}]}]

    Deleting document:
    Anderson.1

    Cleaning up by deleting database FamilyDB
    Completed successfully
    Press any key to exit

<span data-ttu-id="736e9-238">Grattis!</span><span class="sxs-lookup"><span data-stu-id="736e9-238">Congratulations!</span></span> <span data-ttu-id="736e9-239">Du har slutfört självstudiekursen om Node.js och skapat ditt första Azure Cosmos DB-konsolprogram!</span><span class="sxs-lookup"><span data-stu-id="736e9-239">You've created you've completed the Node.js tutorial and have your first Azure Cosmos DB console application!</span></span>

## <span data-ttu-id="736e9-240"><a id="GetSolution"></a>Hämta den fullständiga lösningen till Node.js-självstudien</span><span class="sxs-lookup"><span data-stu-id="736e9-240"><a id="GetSolution"></a>Get the complete Node.js tutorial solution</span></span>
<span data-ttu-id="736e9-241">Om du inte har tid att slutföra stegen i den här självstudien eller bara vill ladda ned koden kan du hämta den från [GitHub](https://github.com/Azure-Samples/documentdb-node-getting-started).</span><span class="sxs-lookup"><span data-stu-id="736e9-241">If you didn't have time to complete the steps in this tutorial, or just want to download the code, you can get it from [GitHub](https://github.com/Azure-Samples/documentdb-node-getting-started).</span></span>

<span data-ttu-id="736e9-242">För att köra GetStarted-lösningen med alla exempel i den här artikeln behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="736e9-242">To run the GetStarted solution that contains all the samples in this article, you will need the following:</span></span>

* <span data-ttu-id="736e9-243">[Azure Cosmos DB-konto][create-account].</span><span class="sxs-lookup"><span data-stu-id="736e9-243">[Azure Cosmos DB account][create-account].</span></span>
* <span data-ttu-id="736e9-244">[GetStarted](https://github.com/Azure-Samples/documentdb-node-getting-started)-lösningen som finns på GitHub.</span><span class="sxs-lookup"><span data-stu-id="736e9-244">The [GetStarted](https://github.com/Azure-Samples/documentdb-node-getting-started) solution available on GitHub.</span></span>

<span data-ttu-id="736e9-245">Installera modulen **documentdb** via npm.</span><span class="sxs-lookup"><span data-stu-id="736e9-245">Install the **documentdb** module via npm.</span></span> <span data-ttu-id="736e9-246">Ange följande kommando:</span><span class="sxs-lookup"><span data-stu-id="736e9-246">Use the following command:</span></span>

* ```npm install documentdb --save```

<span data-ttu-id="736e9-247">Sedan uppdaterar du värdena config.endpoint och config.authKey i filen ```config.js``` enligt beskrivningen i [Steg 3: Ange appkonfigurationer](#Config).</span><span class="sxs-lookup"><span data-stu-id="736e9-247">Next, in the ```config.js``` file, update the config.endpoint and config.authKey values as described in [Step 3: Set your app's configurations](#Config).</span></span> 

<span data-ttu-id="736e9-248">Leta sedan upp filen ```app.js``` i terminalen och kör kommandot: ```node app.js```.</span><span class="sxs-lookup"><span data-stu-id="736e9-248">Then in your terminal, locate your ```app.js``` file and run the command: ```node app.js```.</span></span>

<span data-ttu-id="736e9-249">Då är det bara att bygga den, så är du på väg!</span><span class="sxs-lookup"><span data-stu-id="736e9-249">That's it, build it and you're on your way!</span></span> 

## <a name="next-steps"></a><span data-ttu-id="736e9-250">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="736e9-250">Next steps</span></span>
* <span data-ttu-id="736e9-251">Vill du ha ett mer komplext Node.js-exempel?</span><span class="sxs-lookup"><span data-stu-id="736e9-251">Want a more complex Node.js sample?</span></span> <span data-ttu-id="736e9-252">Mer information finns i [Skapa ett Node.js-webbprogram med Azure Cosmos DB](documentdb-nodejs-application.md).</span><span class="sxs-lookup"><span data-stu-id="736e9-252">See [Build a Node.js web application using Azure Cosmos DB](documentdb-nodejs-application.md).</span></span>
* <span data-ttu-id="736e9-253">Lär dig hur du [övervakar ett Azure Cosmos DB-konto](monitor-accounts.md).</span><span class="sxs-lookup"><span data-stu-id="736e9-253">Learn how to [monitor an Azure Cosmos DB account](monitor-accounts.md).</span></span>
* <span data-ttu-id="736e9-254">Kör frågor mot vår exempeldatauppsättning i [Query Playground](https://www.documentdb.com/sql/demo).</span><span class="sxs-lookup"><span data-stu-id="736e9-254">Run queries against our sample dataset in the [Query Playground](https://www.documentdb.com/sql/demo).</span></span>
* <span data-ttu-id="736e9-255">Mer information om programmeringsmodellen finns i avsnittet Utveckla på [dokumentationssidan för Azure Cosmos DB](https://azure.microsoft.com/documentation/services/documentdb/).</span><span class="sxs-lookup"><span data-stu-id="736e9-255">Learn more about the programming model in the Develop section of the [Azure Cosmos DB documentation page](https://azure.microsoft.com/documentation/services/documentdb/).</span></span>

[create-account]: create-documentdb-dotnet.md#create-account
[keys]: media/documentdb-nodejs-get-started/node-js-tutorial-keys.png
