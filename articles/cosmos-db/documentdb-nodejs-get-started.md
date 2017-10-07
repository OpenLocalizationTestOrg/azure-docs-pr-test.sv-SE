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
# <a name="nodejs-tutorial-use-hello-documentdb-api-in-azure-cosmos-db-toocreate-a-nodejs-console-application"></a>Självstudie om node.js: Använd hello DocumentDB API i Azure Cosmos DB toocreate ett Node.js-konsolprogram
> [!div class="op_single_selector"]
> * [.NET](documentdb-get-started.md)
> * [.NET Core](documentdb-dotnetcore-get-started.md)
> * [Node.js för MongoDB](mongodb-samples.md)
> * [Node.js](documentdb-nodejs-get-started.md)
> * [Java](documentdb-java-get-started.md)
> * [C++](documentdb-cpp-get-started.md)
>  
> 

Välkommen till självstudien om Node.js toohello för hello Azure Cosmos DB Node.js SDK! När du har genomfört den här självstudiekursen har du ett konsolprogram som skapar och skickar frågor till Azure Cosmos DB-resurser.

Vi tar upp följande:

* Skapa och ansluta tooan Azure DB som Cosmos-konto
* Konfigurera ditt program
* Skapa en Node-databas
* Skapa en samling
* Skapa JSON-dokument
* Frågar hello samling
* Ersätta ett dokument
* Ta bort ett dokument
* Om du tar bort hello node-databas

Har du inte tid? Oroa dig inte! hello kompletta lösningen finns på [GitHub](https://github.com/Azure-Samples/documentdb-node-getting-started). Se [Hämta fullständiga lösningen till hello](#GetSolution) snabbguide.

När du har slutfört självstudien om Node.js hello, knappar Kontrollera Använd hello röstning på hello början och slutet av den här sidan toogive oss feedback. Om du vill att vi toocontact du direkt, anser ledigt tooinclude den e-postadress i dina kommentarer.

Nu sätter vi igång!

## <a name="prerequisites-for-hello-nodejs-tutorial"></a>Förutsättningar för självstudien om Node.js hello
Kontrollera att du har hello följande:

* Ett aktivt Azure-konto. Om du inte har ett kan du registrera dig för en [kostnadsfri utvärderingsversion av Azure](https://azure.microsoft.com/pricing/free-trial/).
    * Du kan också använda hello [Azure Cosmos DB emulatorn](local-emulator.md) för den här självstudiekursen.
* [Node.js](https://nodejs.org/) version 0.10.29 eller högre.

## <a name="step-1-create-an-azure-cosmos-db-account"></a>Steg 1: Skapa ett Azure Cosmos DB-konto
Nu ska vi skapa ett Azure Cosmos DB-konto. Om du redan har ett konto som du vill toouse kan du gå vidare för[konfigurera Node.js-programmet](#SetupNode). Om du använder hello Azure Cosmos DB-emulatorn, följ hello stegen i [Azure Cosmos DB emulatorn](local-emulator.md) toosetup hello emulatorn och gå vidare för[konfigurera Node.js-programmet](#SetupNode).

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

## <a id="SetupNode"></a>Steg 2: Konfigurera Node.js-programmet
1. Öppna valfri terminal.
2. Leta upp hello mappen eller katalogen där du vill ha toosave Node.js-programmet.
3. Skapa två tomma JavaScript-filer med hello följande kommandon:
   * Windows:
     * ```fsutil file createnew app.js 0```
     * ```fsutil file createnew config.js 0```
   * Linux/OS X:
     * ```touch app.js```
     * ```touch config.js```
4. Installera modulen för hello documentdb via npm. Använd hello följande kommando:
   * ```npm install documentdb --save```

Bra! Nu när vi har slutfört installationen kan vi börja skriva kod.

## <a id="Config"></a>Steg 3: Ange appkonfigurationer
Öppna ```config.js``` i valfri textredigerare.

Sedan, kopiera och klistra in hello kodfragmentet nedan och ange egenskaper ```config.endpoint``` och ```config.primaryKey``` tooyour Azure Cosmos DB endpoint-uri och primärnyckel. Båda dessa konfigurationer finns i hello [Azure-portalen](https://portal.azure.com).

![Självstudie om node.js – skärmdump av hello Azure portal som visar ett Azure DB som Cosmos-konto med hubben för hello aktiv markerad, hello nycklar knappen är markerad på hello Azure DB som Cosmos-kontoblad, och hello URI, PRIMÄRNYCKEL och SEKUNDÄRNYCKEL markerade i hello Bladet nycklar – Node-databas][keys]

    // ADD THIS PART tooYOUR CODE
    var config = {}

    config.endpoint = "~your Azure Cosmos DB endpoint uri here~";
    config.primaryKey = "~your primary key here~";

Kopiera och klistra in hello ```database id```, ```collection id```, och ```JSON documents``` tooyour ```config``` objekt nedan där du anger din ```config.endpoint``` och ```config.authKey``` egenskaper. Om du redan har data som du vill att toostore i databasen kan du använda Azure Cosmos DB [datamigreringsverktyget](import-data.md) i stället för att lägga till dokumentdefinitionerna hello.

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


hello-databasen, samlingen och dokumentdefinitionerna kommer att fungera som Azure Cosmos-DB ```database id```, ```collection id```, och dokumentens data.

Exportera slutligen din ```config``` objektet, så att du kan referera till det i hello ```app.js``` fil.

            },
            "isRegistered": false
        }
    };

    // ADD THIS PART tooYOUR CODE
    module.exports = config;

## <a id="Connect"></a>Steg 4: Anslut tooan Azure DB som Cosmos-konto
Öppna tomt ```app.js``` i hello textredigerare. Kopiera och klistra in koden hello under tooimport hello ```documentdb``` modulen och den nyligen skapade ```config``` modul.

    // ADD THIS PART tooYOUR CODE
    "use strict";

    var documentClient = require("documentdb").DocumentClient;
    var config = require("./config");
    var url = require('url');

Kopiera och klistra in hello kod toouse hello sparat tidigare ```config.endpoint``` och ```config.primaryKey``` toocreate en ny Dokumentklient.

    var config = require("./config");
    var url = require('url');

    // ADD THIS PART tooYOUR CODE
    var client = new documentClient(config.endpoint, { "masterKey": config.primaryKey });

Nu när du har hello kod tooinitialize hello Azure Cosmos DB-klient ska vi titta på arbeta med Azure Cosmos DB resurser.

## <a name="step-5-create-a-node-database"></a>Steg 5: Skapa en Node-databas
Kopiera och klistra in koden hello under tooset hello HTTP-status för gick inte att hitta hello databasens url och hello samlingens url. Dessa URL: er är hur hello Azure Cosmos DB klienten hittar hello rätt databas och samling.

    var client = new documentClient(config.endpoint, { "masterKey": config.primaryKey });

    // ADD THIS PART tooYOUR CODE
    var HttpStatusCodes = { NOTFOUND: 404 };
    var databaseUrl = `dbs/${config.database.id}`;
    var collectionUrl = `${databaseUrl}/colls/${config.collection.id}`;

En [databasen](documentdb-resources.md#databases) kan skapas med hjälp av hello [createDatabase](https://azure.github.io/azure-documentdb-node/DocumentClient.html) funktion av hello **DocumentClient** klass. En databas är hello logisk behållare för dokumentlagring, partitionerad över samlingarna.

Kopiera och klistra in hello **getDatabase** för att skapa den nya databasen i hello app.js fil med hello ```id``` anges i hello ```config``` objekt. hello funktionen kommer att kontrollera om hello-databas med hello samma ```FamilyRegistry``` id inte redan finns. Om den finns returnerar vi den databasen istället för att skapa en ny.

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

Kopiera och klistra in hello koden nedan där du kan ange hello **getDatabase** fungerar tooadd hello hjälpfunktion **avsluta** som skrivs ut avsluta hälsningsmeddelande och hello anrop för**getDatabase** funktion.

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

Leta upp i terminalen din ```app.js``` filen och köra hello-kommando:```node app.js```

Grattis! Du har skapat en Azure Cosmos DB-databas.

## <a id="CreateColl"></a>Steg 6: Skapa en samling
> [!WARNING]
> **CreateDocumentCollectionAsync** skapar en ny samling, vilket får konsekvenser för priset. Mer information finns på vår [sida med priser](https://azure.microsoft.com/pricing/details/cosmos-db/).
> 
> 

En [samling](documentdb-resources.md#collections) kan skapas med hjälp av hello [createCollection](https://azure.github.io/azure-documentdb-node/DocumentClient.html) funktion av hello **DocumentClient** klass. En samling är en behållare för JSON-dokument och associerad JavaScript-applogik.

Kopiera och klistra in hello **getCollection** funktion under hello **getDatabase** fungera i hello app.js filen toocreate den nya samlingen med hello ```id``` anges i hello ```config```objekt. Vi kontrollerar igen toomake till en samling med hello samma ```FamilyCollection``` id inte redan finns. Om det gör det returnerar vi den samlingen istället för att skapa en ny.

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

Kopiera och klistra in hello koden under anropet hello för**getDatabase** tooexecute hello **getCollection** funktion.

    getDatabase()

    // ADD THIS PART tooYOUR CODE
    .then(() => getCollection())
    // ENDS HERE

    .then(() => { exit(`Completed successfully`); })
    .catch((error) => { exit(`Completed with error ${JSON.stringify(error)}`) });

Leta upp i terminalen din ```app.js``` filen och köra hello-kommando:```node app.js```

Grattis! Du har skapat en Azure DB som Cosmos-samling.

## <a id="CreateDoc"></a>Steg 7: Skapa ett dokument
En [dokument](documentdb-resources.md#documents) kan skapas med hjälp av hello [createDocument](https://azure.github.io/azure-documentdb-node/DocumentClient.html) funktion av hello **DocumentClient** klass. Dokument är användardefinierat (godtyckligt) JSON-innehåll. Nu kan du infoga ett dokument i Azure Cosmos DB.

Kopiera och klistra in hello **getFamilyDocument** funktion under hello **getCollection** för att skapa hello dokument som innehåller hello JSON-data som sparas i hello ```config``` objekt. Vi kontrollerar igen toomake att ett dokument med hello samma id inte redan finns.

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

Kopiera och klistra in hello koden under anropet hello för**getCollection** tooexecute hello **getFamilyDocument** funktion.

    getDatabase()
    .then(() => getCollection())

    // ADD THIS PART tooYOUR CODE
    .then(() => getFamilyDocument(config.documents.Andersen))
    .then(() => getFamilyDocument(config.documents.Wakefield))
    // ENDS HERE

    .then(() => { exit(`Completed successfully`); })
    .catch((error) => { exit(`Completed with error ${JSON.stringify(error)}`) });

Leta upp i terminalen din ```app.js``` filen och köra hello-kommando:```node app.js```

Grattis! Du har skapat ett Azure DB som Cosmos-dokument.

![Självstudie om node.js – Diagram som illustrerar hello hierarkiska relationen mellan hello konto, hello-databasen, hello samlingen och hello dokument - nod databas](./media/documentdb-nodejs-get-started/node-js-tutorial-cosmos-db-account.png)

## <a id="Query"></a>Steg 8: Skicka frågor mot Azure Cosmos DB-resurser
Azure Cosmos DB stöder [komplexa frågor](documentdb-sql-query.md) mot JSON-dokument som lagras i varje samling. hello visar följande exempelkod en fråga som du kan köra mot hello dokument i samlingen.

Kopiera och klistra in hello **queryCollection** funktion under hello **getFamilyDocument** funktion i hello app.js-filen. Azure Cosmos-DB stöder SQL-liknande frågor som visas nedan. Mer information om hur du skapar komplexa frågor kolla hello [Query Playground](https://www.documentdb.com/sql/demo) och hello [frågedokumentationen](documentdb-sql-query.md).

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


hello följande diagram illustrerar hur hello Azure Cosmos DB SQL-frågan syntax anropas mot hello samlingen du skapade.

![Självstudie om node.js – Diagram som illustrerar hello omfånget och innebörden av frågan hello - nod databas](./media/documentdb-nodejs-get-started/node-js-tutorial-collection-documents.png)

Hej [FROM](documentdb-sql-query.md#FromClause) nyckelord är valfritt i frågan hello eftersom Azure DB som Cosmos-frågor redan är begränsade tooa enda samling. ”FROM Families f” kan därför bytas mot ”FROM root r” eller annat valfritt variabelnamn som du väljer. Azure Cosmos-DB kommer härleda att familjer, roten eller variabelnamnet hello du valde, referens hello-samling som standard.

Kopiera och klistra in hello koden under anropet hello för**getFamilyDocument** tooexecute hello **queryCollection** funktion.

    .then(() => getFamilyDocument(config.documents.Andersen))
    .then(() => getFamilyDocument(config.documents.Wakefield))

    // ADD THIS PART tooYOUR CODE
    .then(() => queryCollection())
    // ENDS HERE

    .then(() => { exit(`Completed successfully`); })
    .catch((error) => { exit(`Completed with error ${JSON.stringify(error)}`) });

Leta upp i terminalen din ```app.js``` filen och köra hello-kommando:```node app.js```

Grattis! Du har skickat frågor mot Azure DB Cosmos-dokument.

## <a id="ReplaceDocument"></a>Steg 9: Ersätta ett dokument
Azure Cosmos DB har stöd för ersättning av JSON-dokument.

Kopiera och klistra in hello **replaceFamilyDocument** funktion under hello **queryCollection** funktion i hello app.js-filen.

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

Kopiera och klistra in hello koden under anropet hello för**queryCollection** tooexecute hello **replaceDocument** funktion. Lägg även till hello kod toocall **queryCollection** igen tooverify hello dokumentet har ändrats.

    .then(() => getFamilyDocument(config.documents.Andersen))
    .then(() => getFamilyDocument(config.documents.Wakefield))
    .then(() => queryCollection())

    // ADD THIS PART tooYOUR CODE
    .then(() => replaceFamilyDocument(config.documents.Andersen))
    .then(() => queryCollection())
    // ENDS HERE

    .then(() => { exit(`Completed successfully`); })
    .catch((error) => { exit(`Completed with error ${JSON.stringify(error)}`) });

Leta upp i terminalen din ```app.js``` filen och köra hello-kommando:```node app.js```

Grattis! Du har ersatt ett Azure Cosmos DB-dokument.

## <a id="DeleteDocument"></a>Steg 10: Ta bort ett dokument
Azure Cosmos DB har stöd för borttagning av JSON-dokument.

Kopiera och klistra in hello **deleteFamilyDocument** funktion under hello **replaceFamilyDocument** funktion.

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

Kopiera och klistra in hello koden nedan hello anropet toohello andra **queryCollection** tooexecute hello **deleteDocument** funktion.

    .then(() => queryCollection())
    .then(() => replaceFamilyDocument(config.documents.Andersen))
    .then(() => queryCollection())

    // ADD THIS PART tooYOUR CODE
    .then(() => deleteFamilyDocument(config.documents.Andersen))
    // ENDS HERE

    .then(() => { exit(`Completed successfully`); })
    .catch((error) => { exit(`Completed with error ${JSON.stringify(error)}`) });

Leta upp i terminalen din ```app.js``` filen och köra hello-kommando:```node app.js```

Grattis! Du har tagit bort ett Azure Cosmos DB-dokument.

## <a id="DeleteDatabase"></a>Steg 11: Ta bort hello Node-databas
Ta bort hello skapade databas tar bort hello databasen och alla underordnade resurser (samlingar, dokument, etc.).

Kopiera och klistra in hello **Rensa** funktion under hello **deleteFamilyDocument** tooremove hello databasen och alla hello underordnade resurser.

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

Kopiera och klistra in hello koden under anropet hello för**deleteFamilyDocument** tooexecute hello **Rensa** funktion.

    .then(() => deleteFamilyDocument(config.documents.Andersen))

    // ADD THIS PART tooYOUR CODE
    .then(() => cleanup())
    // ENDS HERE

    .then(() => { exit(`Completed successfully`); })
    .catch((error) => { exit(`Completed with error ${JSON.stringify(error)}`) });

## <a id="Run"></a>Steg 12: Kör Node.js-programmet i sin helhet!
Helt hello sekvensen för att anropa dina funktioner bör se ut så här:

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

Leta upp i terminalen din ```app.js``` filen och köra hello-kommando:```node app.js```

Du bör se get started-appens hello utdata. hello utdata bör motsvara hello exempeltexten nedan.

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

Grattis! Du har slutfört självstudien om Node.js hello och har ditt första Azure DB som Cosmos-konsolprogram!

## <a id="GetSolution"></a>Hämta hello fullständiga självstudiekursen lösningen till Node.js
Om du inte har tid toocomplete hello stegen i den här självstudiekursen, eller bara toodownload hello kod kan du hämta det från [GitHub](https://github.com/Azure-Samples/documentdb-node-getting-started).

toorun hello GetStarted-lösningen som innehåller alla hello prover i den här artikeln, behöver du hello följande:

* [Azure Cosmos DB-konto][create-account].
* Hej [GetStarted](https://github.com/Azure-Samples/documentdb-node-getting-started) lösningen som finns på GitHub.

Installera hello **documentdb** modul via npm. Använd hello följande kommando:

* ```npm install documentdb --save```

Nästa i hello ```config.js``` -fil, uppdatering hello värdena config.endpoint och config.authKey enligt beskrivningen i [steg3: Ange appkonfigurationer](#Config). 

I terminalen Leta upp din ```app.js``` filen och köra hello-kommandot: ```node app.js```.

Då är det bara att bygga den, så är du på väg! 

## <a name="next-steps"></a>Nästa steg
* Vill du ha ett mer komplext Node.js-exempel? Mer information finns i [Skapa ett Node.js-webbprogram med Azure Cosmos DB](documentdb-nodejs-application.md).
* Lär dig hur för[övervaka ett konto i Azure Cosmos DB](monitor-accounts.md).
* Kör frågor mot vår exempeldatauppsättning i hello [Query Playground](https://www.documentdb.com/sql/demo).
* Mer information om hello programmeringsmodell i hello utveckla avsnitt i hello [dokumentationssidan för Azure Cosmos DB](https://azure.microsoft.com/documentation/services/documentdb/).

[create-account]: create-documentdb-dotnet.md#create-account
[keys]: media/documentdb-nodejs-get-started/node-js-tutorial-keys.png
