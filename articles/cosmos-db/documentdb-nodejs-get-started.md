---
title: "aaaNode.js vägledning för hello DocumentDB API: et för Azure Cosmos DB | Microsoft Docs"
description: "En självstudie om Node.js som skapar en Cosmos-DB med hello DocumentDB-API."
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
ms.openlocfilehash: fce244c6a5f321608e82ca51a2c987e84b98bffa
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="nodejs-tutorial-use-hello-documentdb-api-in-azure-cosmos-db-toocreate-a-nodejs-console-application"></a><span data-ttu-id="ef6a5-104">Självstudie om node.js: Använd hello DocumentDB API i Azure Cosmos DB toocreate ett Node.js-konsolprogram</span><span class="sxs-lookup"><span data-stu-id="ef6a5-104">Node.js tutorial: Use hello DocumentDB API in Azure Cosmos DB toocreate a Node.js console application</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="ef6a5-105">.NET</span><span class="sxs-lookup"><span data-stu-id="ef6a5-105">.NET</span></span>](documentdb-get-started.md)
> * [<span data-ttu-id="ef6a5-106">.NET Core</span><span class="sxs-lookup"><span data-stu-id="ef6a5-106">.NET Core</span></span>](documentdb-dotnetcore-get-started.md)
> * [<span data-ttu-id="ef6a5-107">Node.js för MongoDB</span><span class="sxs-lookup"><span data-stu-id="ef6a5-107">Node.js for MongoDB</span></span>](mongodb-samples.md)
> * [<span data-ttu-id="ef6a5-108">Node.js</span><span class="sxs-lookup"><span data-stu-id="ef6a5-108">Node.js</span></span>](documentdb-nodejs-get-started.md)
> * [<span data-ttu-id="ef6a5-109">Java</span><span class="sxs-lookup"><span data-stu-id="ef6a5-109">Java</span></span>](documentdb-java-get-started.md)
> * [<span data-ttu-id="ef6a5-110">C++</span><span class="sxs-lookup"><span data-stu-id="ef6a5-110">C++</span></span>](documentdb-cpp-get-started.md)
>  
> 

<span data-ttu-id="ef6a5-111">Välkommen till självstudien om Node.js toohello för hello Azure Cosmos DB Node.js SDK!</span><span class="sxs-lookup"><span data-stu-id="ef6a5-111">Welcome toohello Node.js tutorial for hello Azure Cosmos DB Node.js SDK!</span></span> <span data-ttu-id="ef6a5-112">När du har genomfört den här självstudiekursen har du ett konsolprogram som skapar och skickar frågor till Azure Cosmos DB-resurser.</span><span class="sxs-lookup"><span data-stu-id="ef6a5-112">After following this tutorial, you'll have a console application that creates and queries Azure Cosmos DB resources.</span></span>

<span data-ttu-id="ef6a5-113">Vi tar upp följande:</span><span class="sxs-lookup"><span data-stu-id="ef6a5-113">We'll cover:</span></span>

* <span data-ttu-id="ef6a5-114">Skapa och ansluta tooan Azure DB som Cosmos-konto</span><span class="sxs-lookup"><span data-stu-id="ef6a5-114">Creating and connecting tooan Azure Cosmos DB account</span></span>
* <span data-ttu-id="ef6a5-115">Konfigurera ditt program</span><span class="sxs-lookup"><span data-stu-id="ef6a5-115">Setting up your application</span></span>
* <span data-ttu-id="ef6a5-116">Skapa en Node-databas</span><span class="sxs-lookup"><span data-stu-id="ef6a5-116">Creating a node database</span></span>
* <span data-ttu-id="ef6a5-117">Skapa en samling</span><span class="sxs-lookup"><span data-stu-id="ef6a5-117">Creating a collection</span></span>
* <span data-ttu-id="ef6a5-118">Skapa JSON-dokument</span><span class="sxs-lookup"><span data-stu-id="ef6a5-118">Creating JSON documents</span></span>
* <span data-ttu-id="ef6a5-119">Frågar hello samling</span><span class="sxs-lookup"><span data-stu-id="ef6a5-119">Querying hello collection</span></span>
* <span data-ttu-id="ef6a5-120">Ersätta ett dokument</span><span class="sxs-lookup"><span data-stu-id="ef6a5-120">Replacing a document</span></span>
* <span data-ttu-id="ef6a5-121">Ta bort ett dokument</span><span class="sxs-lookup"><span data-stu-id="ef6a5-121">Deleting a document</span></span>
* <span data-ttu-id="ef6a5-122">Om du tar bort hello node-databas</span><span class="sxs-lookup"><span data-stu-id="ef6a5-122">Deleting hello node database</span></span>

<span data-ttu-id="ef6a5-123">Har du inte tid?</span><span class="sxs-lookup"><span data-stu-id="ef6a5-123">Don't have time?</span></span> <span data-ttu-id="ef6a5-124">Oroa dig inte!</span><span class="sxs-lookup"><span data-stu-id="ef6a5-124">Don't worry!</span></span> <span data-ttu-id="ef6a5-125">hello kompletta lösningen finns på [GitHub](https://github.com/Azure-Samples/documentdb-node-getting-started).</span><span class="sxs-lookup"><span data-stu-id="ef6a5-125">hello complete solution is available on [GitHub](https://github.com/Azure-Samples/documentdb-node-getting-started).</span></span> <span data-ttu-id="ef6a5-126">Se [Hämta fullständiga lösningen till hello](#GetSolution) snabbguide.</span><span class="sxs-lookup"><span data-stu-id="ef6a5-126">See [Get hello complete solution](#GetSolution) for quick instructions.</span></span>

<span data-ttu-id="ef6a5-127">När du har slutfört självstudien om Node.js hello, knappar Kontrollera Använd hello röstning på hello början och slutet av den här sidan toogive oss feedback.</span><span class="sxs-lookup"><span data-stu-id="ef6a5-127">After you've completed hello Node.js tutorial, please use hello voting buttons at hello top and bottom of this page toogive us feedback.</span></span> <span data-ttu-id="ef6a5-128">Om du vill att vi toocontact du direkt, anser ledigt tooinclude den e-postadress i dina kommentarer.</span><span class="sxs-lookup"><span data-stu-id="ef6a5-128">If you'd like us toocontact you directly, feel free tooinclude your email address in your comments.</span></span>

<span data-ttu-id="ef6a5-129">Nu sätter vi igång!</span><span class="sxs-lookup"><span data-stu-id="ef6a5-129">Now let's get started!</span></span>

## <a name="prerequisites-for-hello-nodejs-tutorial"></a><span data-ttu-id="ef6a5-130">Förutsättningar för självstudien om Node.js hello</span><span class="sxs-lookup"><span data-stu-id="ef6a5-130">Prerequisites for hello Node.js tutorial</span></span>
<span data-ttu-id="ef6a5-131">Kontrollera att du har hello följande:</span><span class="sxs-lookup"><span data-stu-id="ef6a5-131">Please make sure you have hello following:</span></span>

* <span data-ttu-id="ef6a5-132">Ett aktivt Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="ef6a5-132">An active Azure account.</span></span> <span data-ttu-id="ef6a5-133">Om du inte har ett kan du registrera dig för en [kostnadsfri utvärderingsversion av Azure](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="ef6a5-133">If you don't have one, you can sign up for a [Free Azure Trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>
    * <span data-ttu-id="ef6a5-134">Du kan också använda hello [Azure Cosmos DB emulatorn](local-emulator.md) för den här självstudiekursen.</span><span class="sxs-lookup"><span data-stu-id="ef6a5-134">Alternatively, you can use hello [Azure Cosmos DB Emulator](local-emulator.md) for this tutorial.</span></span>
* <span data-ttu-id="ef6a5-135">[Node.js](https://nodejs.org/) version 0.10.29 eller högre.</span><span class="sxs-lookup"><span data-stu-id="ef6a5-135">[Node.js](https://nodejs.org/) version v0.10.29 or higher.</span></span>

## <a name="step-1-create-an-azure-cosmos-db-account"></a><span data-ttu-id="ef6a5-136">Steg 1: Skapa ett Azure Cosmos DB-konto</span><span class="sxs-lookup"><span data-stu-id="ef6a5-136">Step 1: Create an Azure Cosmos DB account</span></span>
<span data-ttu-id="ef6a5-137">Nu ska vi skapa ett Azure Cosmos DB-konto.</span><span class="sxs-lookup"><span data-stu-id="ef6a5-137">Let's create an Azure Cosmos DB account.</span></span> <span data-ttu-id="ef6a5-138">Om du redan har ett konto som du vill toouse kan du gå vidare för[konfigurera Node.js-programmet](#SetupNode).</span><span class="sxs-lookup"><span data-stu-id="ef6a5-138">If you already have an account you want toouse, you can skip ahead too[Set up your Node.js application](#SetupNode).</span></span> <span data-ttu-id="ef6a5-139">Om du använder hello Azure Cosmos DB-emulatorn, följ hello stegen i [Azure Cosmos DB emulatorn](local-emulator.md) toosetup hello emulatorn och gå vidare för[konfigurera Node.js-programmet](#SetupNode).</span><span class="sxs-lookup"><span data-stu-id="ef6a5-139">If you are using hello Azure Cosmos DB Emulator, please follow hello steps at [Azure Cosmos DB Emulator](local-emulator.md) toosetup hello emulator and skip ahead too[Set up your Node.js application](#SetupNode).</span></span>

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

## <span data-ttu-id="ef6a5-140"><a id="SetupNode"></a>Steg 2: Konfigurera Node.js-programmet</span><span class="sxs-lookup"><span data-stu-id="ef6a5-140"><a id="SetupNode"></a>Step 2: Set up your Node.js application</span></span>
1. <span data-ttu-id="ef6a5-141">Öppna valfri terminal.</span><span class="sxs-lookup"><span data-stu-id="ef6a5-141">Open your favorite terminal.</span></span>
2. <span data-ttu-id="ef6a5-142">Leta upp hello mappen eller katalogen där du vill ha toosave Node.js-programmet.</span><span class="sxs-lookup"><span data-stu-id="ef6a5-142">Locate hello folder or directory where you'd like toosave your Node.js application.</span></span>
3. <span data-ttu-id="ef6a5-143">Skapa två tomma JavaScript-filer med hello följande kommandon:</span><span class="sxs-lookup"><span data-stu-id="ef6a5-143">Create two empty JavaScript files with hello following commands:</span></span>
   * <span data-ttu-id="ef6a5-144">Windows:</span><span class="sxs-lookup"><span data-stu-id="ef6a5-144">Windows:</span></span>
     * ```fsutil file createnew app.js 0```
     * ```fsutil file createnew config.js 0```
   * <span data-ttu-id="ef6a5-145">Linux/OS X:</span><span class="sxs-lookup"><span data-stu-id="ef6a5-145">Linux/OS X:</span></span>
     * ```touch app.js```
     * ```touch config.js```
4. <span data-ttu-id="ef6a5-146">Installera modulen för hello documentdb via npm.</span><span class="sxs-lookup"><span data-stu-id="ef6a5-146">Install hello documentdb module via npm.</span></span> <span data-ttu-id="ef6a5-147">Använd hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="ef6a5-147">Use hello following command:</span></span>
   * ```npm install documentdb --save```

<span data-ttu-id="ef6a5-148">Bra!</span><span class="sxs-lookup"><span data-stu-id="ef6a5-148">Great!</span></span> <span data-ttu-id="ef6a5-149">Nu när vi har slutfört installationen kan vi börja skriva kod.</span><span class="sxs-lookup"><span data-stu-id="ef6a5-149">Now that you've finished setting up, let's start writing some code.</span></span>

## <span data-ttu-id="ef6a5-150"><a id="Config"></a>Steg 3: Ange appkonfigurationer</span><span class="sxs-lookup"><span data-stu-id="ef6a5-150"><a id="Config"></a>Step 3: Set your app's configurations</span></span>
<span data-ttu-id="ef6a5-151">Öppna ```config.js``` i valfri textredigerare.</span><span class="sxs-lookup"><span data-stu-id="ef6a5-151">Open ```config.js``` in your favorite text editor.</span></span>

<span data-ttu-id="ef6a5-152">Sedan, kopiera och klistra in hello kodfragmentet nedan och ange egenskaper ```config.endpoint``` och ```config.primaryKey``` tooyour Azure Cosmos DB endpoint-uri och primärnyckel.</span><span class="sxs-lookup"><span data-stu-id="ef6a5-152">Then, copy and paste hello code snippet below and set properties ```config.endpoint``` and ```config.primaryKey``` tooyour Azure Cosmos DB endpoint uri and primary key.</span></span> <span data-ttu-id="ef6a5-153">Båda dessa konfigurationer finns i hello [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="ef6a5-153">Both these configurations can be found in hello [Azure portal](https://portal.azure.com).</span></span>

![Självstudie om node.js – skärmdump av hello Azure portal som visar ett Azure DB som Cosmos-konto med hubben för hello aktiv markerad, hello nycklar knappen är markerad på hello Azure DB som Cosmos-kontoblad, och hello URI, PRIMÄRNYCKEL och SEKUNDÄRNYCKEL markerade i hello Bladet nycklar – Node-databas][keys]

    // ADD THIS PART tooYOUR CODE
    var config = {}

    config.endpoint = "~your Azure Cosmos DB endpoint uri here~";
    config.primaryKey = "~your primary key here~";

<span data-ttu-id="ef6a5-155">Kopiera och klistra in hello ```database id```, ```collection id```, och ```JSON documents``` tooyour ```config``` objekt nedan där du anger din ```config.endpoint``` och ```config.authKey``` egenskaper.</span><span class="sxs-lookup"><span data-stu-id="ef6a5-155">Copy and paste hello ```database id```, ```collection id```, and ```JSON documents``` tooyour ```config``` object below where you set your ```config.endpoint``` and ```config.authKey``` properties.</span></span> <span data-ttu-id="ef6a5-156">Om du redan har data som du vill att toostore i databasen kan du använda Azure Cosmos DB [datamigreringsverktyget](import-data.md) i stället för att lägga till dokumentdefinitionerna hello.</span><span class="sxs-lookup"><span data-stu-id="ef6a5-156">If you already have data you'd like toostore in your database, you can use Azure Cosmos DB's [Data Migration tool](import-data.md) rather than adding hello document definitions.</span></span>

    config.endpoint = "~your Azure Cosmos DB endpoint uri here~";
    config.primaryKey = "~your primary key here~";

    // ADD THIS PART tooYOUR CODE
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


<span data-ttu-id="ef6a5-157">hello-databasen, samlingen och dokumentdefinitionerna kommer att fungera som Azure Cosmos-DB ```database id```, ```collection id```, och dokumentens data.</span><span class="sxs-lookup"><span data-stu-id="ef6a5-157">hello database, collection, and document definitions will act as your Azure Cosmos DB ```database id```, ```collection id```, and documents' data.</span></span>

<span data-ttu-id="ef6a5-158">Exportera slutligen din ```config``` objektet, så att du kan referera till det i hello ```app.js``` fil.</span><span class="sxs-lookup"><span data-stu-id="ef6a5-158">Finally, export your ```config``` object, so that you can reference it within hello ```app.js``` file.</span></span>

            },
            "isRegistered": false
        }
    };

    // ADD THIS PART tooYOUR CODE
    module.exports = config;

## <span data-ttu-id="ef6a5-159"><a id="Connect"></a>Steg 4: Anslut tooan Azure DB som Cosmos-konto</span><span class="sxs-lookup"><span data-stu-id="ef6a5-159"><a id="Connect"></a> Step 4: Connect tooan Azure Cosmos DB account</span></span>
<span data-ttu-id="ef6a5-160">Öppna tomt ```app.js``` i hello textredigerare.</span><span class="sxs-lookup"><span data-stu-id="ef6a5-160">Open your empty ```app.js``` file in hello text editor.</span></span> <span data-ttu-id="ef6a5-161">Kopiera och klistra in koden hello under tooimport hello ```documentdb``` modulen och den nyligen skapade ```config``` modul.</span><span class="sxs-lookup"><span data-stu-id="ef6a5-161">Copy and paste hello code below tooimport hello ```documentdb``` module and your newly created ```config``` module.</span></span>

    // ADD THIS PART tooYOUR CODE
    "use strict";

    var documentClient = require("documentdb").DocumentClient;
    var config = require("./config");
    var url = require('url');

<span data-ttu-id="ef6a5-162">Kopiera och klistra in hello kod toouse hello sparat tidigare ```config.endpoint``` och ```config.primaryKey``` toocreate en ny Dokumentklient.</span><span class="sxs-lookup"><span data-stu-id="ef6a5-162">Copy and paste hello code toouse hello previously saved ```config.endpoint``` and ```config.primaryKey``` toocreate a new DocumentClient.</span></span>

    var config = require("./config");
    var url = require('url');

    // ADD THIS PART tooYOUR CODE
    var client = new documentClient(config.endpoint, { "masterKey": config.primaryKey });

<span data-ttu-id="ef6a5-163">Nu när du har hello kod tooinitialize hello Azure Cosmos DB-klient ska vi titta på arbeta med Azure Cosmos DB resurser.</span><span class="sxs-lookup"><span data-stu-id="ef6a5-163">Now that you have hello code tooinitialize hello Azure Cosmos DB client, let's take a look at working with Azure Cosmos DB resources.</span></span>

## <a name="step-5-create-a-node-database"></a><span data-ttu-id="ef6a5-164">Steg 5: Skapa en Node-databas</span><span class="sxs-lookup"><span data-stu-id="ef6a5-164">Step 5: Create a Node database</span></span>
<span data-ttu-id="ef6a5-165">Kopiera och klistra in koden hello under tooset hello HTTP-status för gick inte att hitta hello databasens url och hello samlingens url.</span><span class="sxs-lookup"><span data-stu-id="ef6a5-165">Copy and paste hello code below tooset hello HTTP status for Not Found, hello database url, and hello collection url.</span></span> <span data-ttu-id="ef6a5-166">Dessa URL: er är hur hello Azure Cosmos DB klienten hittar hello rätt databas och samling.</span><span class="sxs-lookup"><span data-stu-id="ef6a5-166">These urls are how hello Azure Cosmos DB client will find hello right database and collection.</span></span>

    var client = new documentClient(config.endpoint, { "masterKey": config.primaryKey });

    // ADD THIS PART tooYOUR CODE
    var HttpStatusCodes = { NOTFOUND: 404 };
    var databaseUrl = `dbs/${config.database.id}`;
    var collectionUrl = `${databaseUrl}/colls/${config.collection.id}`;

<span data-ttu-id="ef6a5-167">En [databasen](documentdb-resources.md#databases) kan skapas med hjälp av hello [createDatabase](https://azure.github.io/azure-documentdb-node/DocumentClient.html) funktion av hello **DocumentClient** klass.</span><span class="sxs-lookup"><span data-stu-id="ef6a5-167">A [database](documentdb-resources.md#databases) can be created by using hello [createDatabase](https://azure.github.io/azure-documentdb-node/DocumentClient.html) function of hello **DocumentClient** class.</span></span> <span data-ttu-id="ef6a5-168">En databas är hello logisk behållare för dokumentlagring, partitionerad över samlingarna.</span><span class="sxs-lookup"><span data-stu-id="ef6a5-168">A database is hello logical container of document storage partitioned across collections.</span></span>

<span data-ttu-id="ef6a5-169">Kopiera och klistra in hello **getDatabase** för att skapa den nya databasen i hello app.js fil med hello ```id``` anges i hello ```config``` objekt.</span><span class="sxs-lookup"><span data-stu-id="ef6a5-169">Copy and paste hello **getDatabase** function for creating your new database in hello app.js file with hello ```id``` specified in hello ```config``` object.</span></span> <span data-ttu-id="ef6a5-170">hello funktionen kommer att kontrollera om hello-databas med hello samma ```FamilyRegistry``` id inte redan finns.</span><span class="sxs-lookup"><span data-stu-id="ef6a5-170">hello function will check if hello database with hello same ```FamilyRegistry``` id does not already exist.</span></span> <span data-ttu-id="ef6a5-171">Om den finns returnerar vi den databasen istället för att skapa en ny.</span><span class="sxs-lookup"><span data-stu-id="ef6a5-171">If it does exist, we'll return that database instead of creating a new one.</span></span>

    var collectionUrl = `${databaseUrl}/colls/${config.collection.id}`;

    // ADD THIS PART tooYOUR CODE
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

<span data-ttu-id="ef6a5-172">Kopiera och klistra in hello koden nedan där du kan ange hello **getDatabase** fungerar tooadd hello hjälpfunktion **avsluta** som skrivs ut avsluta hälsningsmeddelande och hello anrop för**getDatabase** funktion.</span><span class="sxs-lookup"><span data-stu-id="ef6a5-172">Copy and paste hello code below where you set hello **getDatabase** function tooadd hello helper function **exit** that will print hello exit message and hello call too**getDatabase** function.</span></span>

                } else {
                    resolve(result);
                }
            });
        });
    }

    // ADD THIS PART tooYOUR CODE
    function exit(message) {
        console.log(message);
        console.log('Press any key tooexit');
        process.stdin.setRawMode(true);
        process.stdin.resume();
        process.stdin.on('data', process.exit.bind(process, 0));
    }

    getDatabase()
    .then(() => { exit(`Completed successfully`); })
    .catch((error) => { exit(`Completed with error ${JSON.stringify(error)}`) });

<span data-ttu-id="ef6a5-173">Leta upp i terminalen din ```app.js``` filen och köra hello-kommando:```node app.js```</span><span class="sxs-lookup"><span data-stu-id="ef6a5-173">In your terminal, locate your ```app.js``` file and run hello command: ```node app.js```</span></span>

<span data-ttu-id="ef6a5-174">Grattis!</span><span class="sxs-lookup"><span data-stu-id="ef6a5-174">Congratulations!</span></span> <span data-ttu-id="ef6a5-175">Du har skapat en Azure Cosmos DB-databas.</span><span class="sxs-lookup"><span data-stu-id="ef6a5-175">You have successfully created an Azure Cosmos DB database.</span></span>

## <span data-ttu-id="ef6a5-176"><a id="CreateColl"></a>Steg 6: Skapa en samling</span><span class="sxs-lookup"><span data-stu-id="ef6a5-176"><a id="CreateColl"></a>Step 6: Create a collection</span></span>
> [!WARNING]
> <span data-ttu-id="ef6a5-177">**CreateDocumentCollectionAsync** skapar en ny samling, vilket får konsekvenser för priset.</span><span class="sxs-lookup"><span data-stu-id="ef6a5-177">**CreateDocumentCollectionAsync** will create a new collection, which has pricing implications.</span></span> <span data-ttu-id="ef6a5-178">Mer information finns på vår [sida med priser](https://azure.microsoft.com/pricing/details/cosmos-db/).</span><span class="sxs-lookup"><span data-stu-id="ef6a5-178">For more details, please visit our [pricing page](https://azure.microsoft.com/pricing/details/cosmos-db/).</span></span>
> 
> 

<span data-ttu-id="ef6a5-179">En [samling](documentdb-resources.md#collections) kan skapas med hjälp av hello [createCollection](https://azure.github.io/azure-documentdb-node/DocumentClient.html) funktion av hello **DocumentClient** klass.</span><span class="sxs-lookup"><span data-stu-id="ef6a5-179">A [collection](documentdb-resources.md#collections) can be created by using hello [createCollection](https://azure.github.io/azure-documentdb-node/DocumentClient.html) function of hello **DocumentClient** class.</span></span> <span data-ttu-id="ef6a5-180">En samling är en behållare för JSON-dokument och associerad JavaScript-applogik.</span><span class="sxs-lookup"><span data-stu-id="ef6a5-180">A collection is a container of JSON documents and associated JavaScript application logic.</span></span>

<span data-ttu-id="ef6a5-181">Kopiera och klistra in hello **getCollection** funktion under hello **getDatabase** fungera i hello app.js filen toocreate den nya samlingen med hello ```id``` anges i hello ```config```objekt.</span><span class="sxs-lookup"><span data-stu-id="ef6a5-181">Copy and paste hello **getCollection** function underneath hello **getDatabase** function in hello app.js file toocreate your new collection with hello ```id``` specified in hello ```config``` object.</span></span> <span data-ttu-id="ef6a5-182">Vi kontrollerar igen toomake till en samling med hello samma ```FamilyCollection``` id inte redan finns.</span><span class="sxs-lookup"><span data-stu-id="ef6a5-182">Again, we'll check toomake sure a collection with hello same ```FamilyCollection``` id does not already exist.</span></span> <span data-ttu-id="ef6a5-183">Om det gör det returnerar vi den samlingen istället för att skapa en ny.</span><span class="sxs-lookup"><span data-stu-id="ef6a5-183">If it does exist, we'll return that collection instead of creating a new one.</span></span>

                } else {
                    resolve(result);
                }
            });
        });
    }

    // ADD THIS PART tooYOUR CODE
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

<span data-ttu-id="ef6a5-184">Kopiera och klistra in hello koden under anropet hello för**getDatabase** tooexecute hello **getCollection** funktion.</span><span class="sxs-lookup"><span data-stu-id="ef6a5-184">Copy and paste hello code below hello call too**getDatabase** tooexecute hello **getCollection** function.</span></span>

    getDatabase()

    // ADD THIS PART tooYOUR CODE
    .then(() => getCollection())
    // ENDS HERE

    .then(() => { exit(`Completed successfully`); })
    .catch((error) => { exit(`Completed with error ${JSON.stringify(error)}`) });

<span data-ttu-id="ef6a5-185">Leta upp i terminalen din ```app.js``` filen och köra hello-kommando:```node app.js```</span><span class="sxs-lookup"><span data-stu-id="ef6a5-185">In your terminal, locate your ```app.js``` file and run hello command: ```node app.js```</span></span>

<span data-ttu-id="ef6a5-186">Grattis!</span><span class="sxs-lookup"><span data-stu-id="ef6a5-186">Congratulations!</span></span> <span data-ttu-id="ef6a5-187">Du har skapat en Azure DB som Cosmos-samling.</span><span class="sxs-lookup"><span data-stu-id="ef6a5-187">You have successfully created an Azure Cosmos DB collection.</span></span>

## <span data-ttu-id="ef6a5-188"><a id="CreateDoc"></a>Steg 7: Skapa ett dokument</span><span class="sxs-lookup"><span data-stu-id="ef6a5-188"><a id="CreateDoc"></a>Step 7: Create a document</span></span>
<span data-ttu-id="ef6a5-189">En [dokument](documentdb-resources.md#documents) kan skapas med hjälp av hello [createDocument](https://azure.github.io/azure-documentdb-node/DocumentClient.html) funktion av hello **DocumentClient** klass.</span><span class="sxs-lookup"><span data-stu-id="ef6a5-189">A [document](documentdb-resources.md#documents) can be created by using hello [createDocument](https://azure.github.io/azure-documentdb-node/DocumentClient.html) function of hello **DocumentClient** class.</span></span> <span data-ttu-id="ef6a5-190">Dokument är användardefinierat (godtyckligt) JSON-innehåll.</span><span class="sxs-lookup"><span data-stu-id="ef6a5-190">Documents are user defined (arbitrary) JSON content.</span></span> <span data-ttu-id="ef6a5-191">Nu kan du infoga ett dokument i Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="ef6a5-191">You can now insert a document into Azure Cosmos DB.</span></span>

<span data-ttu-id="ef6a5-192">Kopiera och klistra in hello **getFamilyDocument** funktion under hello **getCollection** för att skapa hello dokument som innehåller hello JSON-data som sparas i hello ```config``` objekt.</span><span class="sxs-lookup"><span data-stu-id="ef6a5-192">Copy and paste hello **getFamilyDocument** function underneath hello **getCollection** function for creating hello documents containing hello JSON data saved in hello ```config``` object.</span></span> <span data-ttu-id="ef6a5-193">Vi kontrollerar igen toomake att ett dokument med hello samma id inte redan finns.</span><span class="sxs-lookup"><span data-stu-id="ef6a5-193">Again, we'll check toomake sure a document with hello same id does not already exist.</span></span>

                } else {
                    resolve(result);
                }
            });
        });
    }

    // ADD THIS PART tooYOUR CODE
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

<span data-ttu-id="ef6a5-194">Kopiera och klistra in hello koden under anropet hello för**getCollection** tooexecute hello **getFamilyDocument** funktion.</span><span class="sxs-lookup"><span data-stu-id="ef6a5-194">Copy and paste hello code below hello call too**getCollection** tooexecute hello **getFamilyDocument** function.</span></span>

    getDatabase()
    .then(() => getCollection())

    // ADD THIS PART tooYOUR CODE
    .then(() => getFamilyDocument(config.documents.Andersen))
    .then(() => getFamilyDocument(config.documents.Wakefield))
    // ENDS HERE

    .then(() => { exit(`Completed successfully`); })
    .catch((error) => { exit(`Completed with error ${JSON.stringify(error)}`) });

<span data-ttu-id="ef6a5-195">Leta upp i terminalen din ```app.js``` filen och köra hello-kommando:```node app.js```</span><span class="sxs-lookup"><span data-stu-id="ef6a5-195">In your terminal, locate your ```app.js``` file and run hello command: ```node app.js```</span></span>

<span data-ttu-id="ef6a5-196">Grattis!</span><span class="sxs-lookup"><span data-stu-id="ef6a5-196">Congratulations!</span></span> <span data-ttu-id="ef6a5-197">Du har skapat ett Azure DB som Cosmos-dokument.</span><span class="sxs-lookup"><span data-stu-id="ef6a5-197">You have successfully created an Azure Cosmos DB document.</span></span>

![Självstudie om node.js – Diagram som illustrerar hello hierarkiska relationen mellan hello konto, hello-databasen, hello samlingen och hello dokument - nod databas](./media/documentdb-nodejs-get-started/node-js-tutorial-cosmos-db-account.png)

## <span data-ttu-id="ef6a5-199"><a id="Query"></a>Steg 8: Skicka frågor mot Azure Cosmos DB-resurser</span><span class="sxs-lookup"><span data-stu-id="ef6a5-199"><a id="Query"></a>Step 8: Query Azure Cosmos DB resources</span></span>
<span data-ttu-id="ef6a5-200">Azure Cosmos DB stöder [komplexa frågor](documentdb-sql-query.md) mot JSON-dokument som lagras i varje samling.</span><span class="sxs-lookup"><span data-stu-id="ef6a5-200">Azure Cosmos DB supports [rich queries](documentdb-sql-query.md) against JSON documents stored in each collection.</span></span> <span data-ttu-id="ef6a5-201">hello visar följande exempelkod en fråga som du kan köra mot hello dokument i samlingen.</span><span class="sxs-lookup"><span data-stu-id="ef6a5-201">hello following sample code shows a query that you can run against hello documents in your collection.</span></span>

<span data-ttu-id="ef6a5-202">Kopiera och klistra in hello **queryCollection** funktion under hello **getFamilyDocument** funktion i hello app.js-filen.</span><span class="sxs-lookup"><span data-stu-id="ef6a5-202">Copy and paste hello **queryCollection** function underneath hello **getFamilyDocument** function in hello app.js file.</span></span> <span data-ttu-id="ef6a5-203">Azure Cosmos-DB stöder SQL-liknande frågor som visas nedan.</span><span class="sxs-lookup"><span data-stu-id="ef6a5-203">Azure Cosmos DB supports SQL-like queries as shown below.</span></span> <span data-ttu-id="ef6a5-204">Mer information om hur du skapar komplexa frågor kolla hello [Query Playground](https://www.documentdb.com/sql/demo) och hello [frågedokumentationen](documentdb-sql-query.md).</span><span class="sxs-lookup"><span data-stu-id="ef6a5-204">For more information on building complex queries, check out hello [Query Playground](https://www.documentdb.com/sql/demo) and hello [query documentation](documentdb-sql-query.md).</span></span>

                } else {
                    resolve(result);
                }
            });
        });
    }

    // ADD THIS PART tooYOUR CODE
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


<span data-ttu-id="ef6a5-205">hello följande diagram illustrerar hur hello Azure Cosmos DB SQL-frågan syntax anropas mot hello samlingen du skapade.</span><span class="sxs-lookup"><span data-stu-id="ef6a5-205">hello following diagram illustrates how hello Azure Cosmos DB SQL query syntax is called against hello collection you created.</span></span>

![Självstudie om node.js – Diagram som illustrerar hello omfånget och innebörden av frågan hello - nod databas](./media/documentdb-nodejs-get-started/node-js-tutorial-collection-documents.png)

<span data-ttu-id="ef6a5-207">Hej [FROM](documentdb-sql-query.md#FromClause) nyckelord är valfritt i frågan hello eftersom Azure DB som Cosmos-frågor redan är begränsade tooa enda samling.</span><span class="sxs-lookup"><span data-stu-id="ef6a5-207">hello [FROM](documentdb-sql-query.md#FromClause) keyword is optional in hello query because Azure Cosmos DB queries are already scoped tooa single collection.</span></span> <span data-ttu-id="ef6a5-208">”FROM Families f” kan därför bytas mot ”FROM root r” eller annat valfritt variabelnamn som du väljer.</span><span class="sxs-lookup"><span data-stu-id="ef6a5-208">Therefore, "FROM Families f" can be swapped with "FROM root r", or any other variable name you choose.</span></span> <span data-ttu-id="ef6a5-209">Azure Cosmos-DB kommer härleda att familjer, roten eller variabelnamnet hello du valde, referens hello-samling som standard.</span><span class="sxs-lookup"><span data-stu-id="ef6a5-209">Azure Cosmos DB will infer that Families, root, or hello variable name you chose, reference hello current collection by default.</span></span>

<span data-ttu-id="ef6a5-210">Kopiera och klistra in hello koden under anropet hello för**getFamilyDocument** tooexecute hello **queryCollection** funktion.</span><span class="sxs-lookup"><span data-stu-id="ef6a5-210">Copy and paste hello code below hello call too**getFamilyDocument** tooexecute hello **queryCollection** function.</span></span>

    .then(() => getFamilyDocument(config.documents.Andersen))
    .then(() => getFamilyDocument(config.documents.Wakefield))

    // ADD THIS PART tooYOUR CODE
    .then(() => queryCollection())
    // ENDS HERE

    .then(() => { exit(`Completed successfully`); })
    .catch((error) => { exit(`Completed with error ${JSON.stringify(error)}`) });

<span data-ttu-id="ef6a5-211">Leta upp i terminalen din ```app.js``` filen och köra hello-kommando:```node app.js```</span><span class="sxs-lookup"><span data-stu-id="ef6a5-211">In your terminal, locate your ```app.js``` file and run hello command: ```node app.js```</span></span>

<span data-ttu-id="ef6a5-212">Grattis!</span><span class="sxs-lookup"><span data-stu-id="ef6a5-212">Congratulations!</span></span> <span data-ttu-id="ef6a5-213">Du har skickat frågor mot Azure DB Cosmos-dokument.</span><span class="sxs-lookup"><span data-stu-id="ef6a5-213">You have successfully queried Azure Cosmos DB documents.</span></span>

## <span data-ttu-id="ef6a5-214"><a id="ReplaceDocument"></a>Steg 9: Ersätta ett dokument</span><span class="sxs-lookup"><span data-stu-id="ef6a5-214"><a id="ReplaceDocument"></a>Step 9: Replace a document</span></span>
<span data-ttu-id="ef6a5-215">Azure Cosmos DB har stöd för ersättning av JSON-dokument.</span><span class="sxs-lookup"><span data-stu-id="ef6a5-215">Azure Cosmos DB supports replacing JSON documents.</span></span>

<span data-ttu-id="ef6a5-216">Kopiera och klistra in hello **replaceFamilyDocument** funktion under hello **queryCollection** funktion i hello app.js-filen.</span><span class="sxs-lookup"><span data-stu-id="ef6a5-216">Copy and paste hello **replaceFamilyDocument** function underneath hello **queryCollection** function in hello app.js file.</span></span>

                    }
                    console.log();
                    resolve(result);
                }
            });
        });
    }

    // ADD THIS PART tooYOUR CODE
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

<span data-ttu-id="ef6a5-217">Kopiera och klistra in hello koden under anropet hello för**queryCollection** tooexecute hello **replaceDocument** funktion.</span><span class="sxs-lookup"><span data-stu-id="ef6a5-217">Copy and paste hello code below hello call too**queryCollection** tooexecute hello **replaceDocument** function.</span></span> <span data-ttu-id="ef6a5-218">Lägg även till hello kod toocall **queryCollection** igen tooverify hello dokumentet har ändrats.</span><span class="sxs-lookup"><span data-stu-id="ef6a5-218">Also, add hello code toocall **queryCollection** again tooverify that hello document had successfully changed.</span></span>

    .then(() => getFamilyDocument(config.documents.Andersen))
    .then(() => getFamilyDocument(config.documents.Wakefield))
    .then(() => queryCollection())

    // ADD THIS PART tooYOUR CODE
    .then(() => replaceFamilyDocument(config.documents.Andersen))
    .then(() => queryCollection())
    // ENDS HERE

    .then(() => { exit(`Completed successfully`); })
    .catch((error) => { exit(`Completed with error ${JSON.stringify(error)}`) });

<span data-ttu-id="ef6a5-219">Leta upp i terminalen din ```app.js``` filen och köra hello-kommando:```node app.js```</span><span class="sxs-lookup"><span data-stu-id="ef6a5-219">In your terminal, locate your ```app.js``` file and run hello command: ```node app.js```</span></span>

<span data-ttu-id="ef6a5-220">Grattis!</span><span class="sxs-lookup"><span data-stu-id="ef6a5-220">Congratulations!</span></span> <span data-ttu-id="ef6a5-221">Du har ersatt ett Azure Cosmos DB-dokument.</span><span class="sxs-lookup"><span data-stu-id="ef6a5-221">You have successfully replaced an Azure Cosmos DB document.</span></span>

## <span data-ttu-id="ef6a5-222"><a id="DeleteDocument"></a>Steg 10: Ta bort ett dokument</span><span class="sxs-lookup"><span data-stu-id="ef6a5-222"><a id="DeleteDocument"></a>Step 10: Delete a document</span></span>
<span data-ttu-id="ef6a5-223">Azure Cosmos DB har stöd för borttagning av JSON-dokument.</span><span class="sxs-lookup"><span data-stu-id="ef6a5-223">Azure Cosmos DB supports deleting JSON documents.</span></span>

<span data-ttu-id="ef6a5-224">Kopiera och klistra in hello **deleteFamilyDocument** funktion under hello **replaceFamilyDocument** funktion.</span><span class="sxs-lookup"><span data-stu-id="ef6a5-224">Copy and paste hello **deleteFamilyDocument** function underneath hello **replaceFamilyDocument** function.</span></span>

                else {
                    resolve(result);
                }
            });
        });
    };

    // ADD THIS PART tooYOUR CODE
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

<span data-ttu-id="ef6a5-225">Kopiera och klistra in hello koden nedan hello anropet toohello andra **queryCollection** tooexecute hello **deleteDocument** funktion.</span><span class="sxs-lookup"><span data-stu-id="ef6a5-225">Copy and paste hello code below hello call toohello second **queryCollection** tooexecute hello **deleteDocument** function.</span></span>

    .then(() => queryCollection())
    .then(() => replaceFamilyDocument(config.documents.Andersen))
    .then(() => queryCollection())

    // ADD THIS PART tooYOUR CODE
    .then(() => deleteFamilyDocument(config.documents.Andersen))
    // ENDS HERE

    .then(() => { exit(`Completed successfully`); })
    .catch((error) => { exit(`Completed with error ${JSON.stringify(error)}`) });

<span data-ttu-id="ef6a5-226">Leta upp i terminalen din ```app.js``` filen och köra hello-kommando:```node app.js```</span><span class="sxs-lookup"><span data-stu-id="ef6a5-226">In your terminal, locate your ```app.js``` file and run hello command: ```node app.js```</span></span>

<span data-ttu-id="ef6a5-227">Grattis!</span><span class="sxs-lookup"><span data-stu-id="ef6a5-227">Congratulations!</span></span> <span data-ttu-id="ef6a5-228">Du har tagit bort ett Azure Cosmos DB-dokument.</span><span class="sxs-lookup"><span data-stu-id="ef6a5-228">You have successfully deleted an Azure Cosmos DB document.</span></span>

## <span data-ttu-id="ef6a5-229"><a id="DeleteDatabase"></a>Steg 11: Ta bort hello Node-databas</span><span class="sxs-lookup"><span data-stu-id="ef6a5-229"><a id="DeleteDatabase"></a>Step 11: Delete hello Node database</span></span>
<span data-ttu-id="ef6a5-230">Ta bort hello skapade databas tar bort hello databasen och alla underordnade resurser (samlingar, dokument, etc.).</span><span class="sxs-lookup"><span data-stu-id="ef6a5-230">Deleting hello created database will remove hello database and all children resources (collections, documents, etc.).</span></span>

<span data-ttu-id="ef6a5-231">Kopiera och klistra in hello **Rensa** funktion under hello **deleteFamilyDocument** tooremove hello databasen och alla hello underordnade resurser.</span><span class="sxs-lookup"><span data-stu-id="ef6a5-231">Copy and paste hello **cleanup** function underneath hello **deleteFamilyDocument** function tooremove hello database and all hello children resources.</span></span>

                else {
                    resolve(result);
                }
            });
        });
    };

    // ADD THIS PART tooYOUR CODE
    function cleanup() {
        console.log(`Cleaning up by deleting database ${config.database.id}`);

        return new Promise((resolve, reject) => {
            client.deleteDatabase(databaseUrl, (err) => {
                if (err) reject(err)
                else resolve(null);
            });
        });
    }

<span data-ttu-id="ef6a5-232">Kopiera och klistra in hello koden under anropet hello för**deleteFamilyDocument** tooexecute hello **Rensa** funktion.</span><span class="sxs-lookup"><span data-stu-id="ef6a5-232">Copy and paste hello code below hello call too**deleteFamilyDocument** tooexecute hello **cleanup** function.</span></span>

    .then(() => deleteFamilyDocument(config.documents.Andersen))

    // ADD THIS PART tooYOUR CODE
    .then(() => cleanup())
    // ENDS HERE

    .then(() => { exit(`Completed successfully`); })
    .catch((error) => { exit(`Completed with error ${JSON.stringify(error)}`) });

## <span data-ttu-id="ef6a5-233"><a id="Run"></a>Steg 12: Kör Node.js-programmet i sin helhet!</span><span class="sxs-lookup"><span data-stu-id="ef6a5-233"><a id="Run"></a>Step 12: Run your Node.js application all together!</span></span>
<span data-ttu-id="ef6a5-234">Helt hello sekvensen för att anropa dina funktioner bör se ut så här:</span><span class="sxs-lookup"><span data-stu-id="ef6a5-234">Altogether, hello sequence for calling your functions should look like this:</span></span>

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

<span data-ttu-id="ef6a5-235">Leta upp i terminalen din ```app.js``` filen och köra hello-kommando:```node app.js```</span><span class="sxs-lookup"><span data-stu-id="ef6a5-235">In your terminal, locate your ```app.js``` file and run hello command: ```node app.js```</span></span>

<span data-ttu-id="ef6a5-236">Du bör se get started-appens hello utdata.</span><span class="sxs-lookup"><span data-stu-id="ef6a5-236">You should see hello output of your get started app.</span></span> <span data-ttu-id="ef6a5-237">hello utdata bör motsvara hello exempeltexten nedan.</span><span class="sxs-lookup"><span data-stu-id="ef6a5-237">hello output should match hello example text below.</span></span>

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
    Press any key tooexit

<span data-ttu-id="ef6a5-238">Grattis!</span><span class="sxs-lookup"><span data-stu-id="ef6a5-238">Congratulations!</span></span> <span data-ttu-id="ef6a5-239">Du har slutfört självstudien om Node.js hello och har ditt första Azure DB som Cosmos-konsolprogram!</span><span class="sxs-lookup"><span data-stu-id="ef6a5-239">You've created you've completed hello Node.js tutorial and have your first Azure Cosmos DB console application!</span></span>

## <span data-ttu-id="ef6a5-240"><a id="GetSolution"></a>Hämta hello fullständiga självstudiekursen lösningen till Node.js</span><span class="sxs-lookup"><span data-stu-id="ef6a5-240"><a id="GetSolution"></a>Get hello complete Node.js tutorial solution</span></span>
<span data-ttu-id="ef6a5-241">Om du inte har tid toocomplete hello stegen i den här självstudiekursen, eller bara toodownload hello kod kan du hämta det från [GitHub](https://github.com/Azure-Samples/documentdb-node-getting-started).</span><span class="sxs-lookup"><span data-stu-id="ef6a5-241">If you didn't have time toocomplete hello steps in this tutorial, or just want toodownload hello code, you can get it from [GitHub](https://github.com/Azure-Samples/documentdb-node-getting-started).</span></span>

<span data-ttu-id="ef6a5-242">toorun hello GetStarted-lösningen som innehåller alla hello prover i den här artikeln, behöver du hello följande:</span><span class="sxs-lookup"><span data-stu-id="ef6a5-242">toorun hello GetStarted solution that contains all hello samples in this article, you will need hello following:</span></span>

* <span data-ttu-id="ef6a5-243">[Azure Cosmos DB-konto][create-account].</span><span class="sxs-lookup"><span data-stu-id="ef6a5-243">[Azure Cosmos DB account][create-account].</span></span>
* <span data-ttu-id="ef6a5-244">Hej [GetStarted](https://github.com/Azure-Samples/documentdb-node-getting-started) lösningen som finns på GitHub.</span><span class="sxs-lookup"><span data-stu-id="ef6a5-244">hello [GetStarted](https://github.com/Azure-Samples/documentdb-node-getting-started) solution available on GitHub.</span></span>

<span data-ttu-id="ef6a5-245">Installera hello **documentdb** modul via npm.</span><span class="sxs-lookup"><span data-stu-id="ef6a5-245">Install hello **documentdb** module via npm.</span></span> <span data-ttu-id="ef6a5-246">Använd hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="ef6a5-246">Use hello following command:</span></span>

* ```npm install documentdb --save```

<span data-ttu-id="ef6a5-247">Nästa i hello ```config.js``` -fil, uppdatering hello värdena config.endpoint och config.authKey enligt beskrivningen i [steg3: Ange appkonfigurationer](#Config).</span><span class="sxs-lookup"><span data-stu-id="ef6a5-247">Next, in hello ```config.js``` file, update hello config.endpoint and config.authKey values as described in [Step 3: Set your app's configurations](#Config).</span></span> 

<span data-ttu-id="ef6a5-248">I terminalen Leta upp din ```app.js``` filen och köra hello-kommandot: ```node app.js```.</span><span class="sxs-lookup"><span data-stu-id="ef6a5-248">Then in your terminal, locate your ```app.js``` file and run hello command: ```node app.js```.</span></span>

<span data-ttu-id="ef6a5-249">Då är det bara att bygga den, så är du på väg!</span><span class="sxs-lookup"><span data-stu-id="ef6a5-249">That's it, build it and you're on your way!</span></span> 

## <a name="next-steps"></a><span data-ttu-id="ef6a5-250">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="ef6a5-250">Next steps</span></span>
* <span data-ttu-id="ef6a5-251">Vill du ha ett mer komplext Node.js-exempel?</span><span class="sxs-lookup"><span data-stu-id="ef6a5-251">Want a more complex Node.js sample?</span></span> <span data-ttu-id="ef6a5-252">Mer information finns i [Skapa ett Node.js-webbprogram med Azure Cosmos DB](documentdb-nodejs-application.md).</span><span class="sxs-lookup"><span data-stu-id="ef6a5-252">See [Build a Node.js web application using Azure Cosmos DB](documentdb-nodejs-application.md).</span></span>
* <span data-ttu-id="ef6a5-253">Lär dig hur för[övervaka ett konto i Azure Cosmos DB](monitor-accounts.md).</span><span class="sxs-lookup"><span data-stu-id="ef6a5-253">Learn how too[monitor an Azure Cosmos DB account](monitor-accounts.md).</span></span>
* <span data-ttu-id="ef6a5-254">Kör frågor mot vår exempeldatauppsättning i hello [Query Playground](https://www.documentdb.com/sql/demo).</span><span class="sxs-lookup"><span data-stu-id="ef6a5-254">Run queries against our sample dataset in hello [Query Playground](https://www.documentdb.com/sql/demo).</span></span>
* <span data-ttu-id="ef6a5-255">Mer information om hello programmeringsmodell i hello utveckla avsnitt i hello [dokumentationssidan för Azure Cosmos DB](https://azure.microsoft.com/documentation/services/documentdb/).</span><span class="sxs-lookup"><span data-stu-id="ef6a5-255">Learn more about hello programming model in hello Develop section of hello [Azure Cosmos DB documentation page](https://azure.microsoft.com/documentation/services/documentdb/).</span></span>

[create-account]: create-documentdb-dotnet.md#create-account
[keys]: media/documentdb-nodejs-get-started/node-js-tutorial-keys.png
