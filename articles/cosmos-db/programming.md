---
title: "aaaServer sida JavaScript-programmering för Azure Cosmos DB | Microsoft Docs"
description: "Lär dig hur toouse Azure Cosmos DB toowrite lagrade procedurer, databasutlösare och användardefinierade funktioner (UDF) i JavaScript. Hämta databasen programing tips och mycket mer."
keywords: "Utlösare, lagrad procedur, lagrad procedur, databasprogram, sproc, documentdb, azure, Microsoft azure-databas"
services: cosmos-db
documentationcenter: 
author: aliuy
manager: jhubbard
editor: mimig
ms.assetid: 0fba7ebd-a4fc-4253-a786-97f1354fbf17
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/13/2016
ms.author: andrl
ms.openlocfilehash: 5a011d1c4b0b5908d5de73607a1bc328ed1711d0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-cosmos-db-server-side-programming-stored-procedures-database-triggers-and-udfs"></a>Azure DB Cosmos serversidan programmering: lagrade procedurer, databasutlösare och UDF: er
Lär dig hur Azure Cosmos DB språk integreras, transaktionell körning av JavaScript kan utvecklare skriva **lagrade procedurer**, **utlösare** och **användardefinierade funktioner (UDF)** internt i en [ECMAScript 2015](http://www.ecma-international.org/ecma-262/6.0/) JavaScript. Detta ger dig toowrite databasen program med programlogik som levererats och köras direkt på hello partitioner för lagring av databasen. 

Vi rekommenderar komma igång genom att titta på hello följande video, där Andrew Liu är en kort introduktion tooCosmos DBS serversidan databasen programmeringsmodell. 

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Azure-Demo-A-Quick-Intro-to-Azure-DocumentDBs-Server-Side-Javascript/player]
> 
> 

Gå sedan tillbaka toothis artikel, där du lär dig hello svar toohello följande frågor:  

* Hur jag skriva en lagrad procedur, utlösare och UDF med hjälp av JavaScript?
* Hur garanterar Cosmos DB av?
* Hur fungerar transaktioner i Cosmos-databasen?
* Vad är utlöser före och efter utlöser och hur skriver jag en?
* Hur registrerar jag och köra lagrade procedurer, utlösare och UDF RESTful sätt med hjälp av HTTP?
* Vad Cosmos DB SDK: er är tillgängliga toocreate och köra lagrade procedurer, utlösare och UDF: er?

## <a name="introduction-toostored-procedure-and-udf-programming"></a>Introduktion tooStored proceduren och UDF programmering
Den här metoden för *”JavaScript som en modern dag T-SQL”* Frigör programutvecklare från hello svårigheter av felmatchningar system och Objektrelationer mappning teknik. Det finns också ett antal inbyggda fördelar som kan utnyttjade toobuild omfattande program:  

* **Procedurmässig logik:** JavaScript som en hög nivå programmeringsspråk, ger en omfattande och välbekanta gränssnittet tooexpress affärslogik. Du kan utföra komplexa sekvenser av operations närmare toohello data.
* **Atomiska transaktioner:** Cosmos DB garanterar att databasen åtgärder som utförs i en lagrad procedur eller utlösare är atomiska. På så sätt kan ett program kombinera relaterade åtgärder i en enda grupp, så att alla lyckas eller ingen av dem lyckas. 
* **Prestanda:** hello faktum att JSON är är mappade toohello Javascript språk typsystemet och även hello grundläggande körningsenheten på lagring i Cosmos DB möjliggör ett antal prestandaoptimeringar som lazy materialisering av JSON-dokument i hello buffert poolen och gör dem tillgängliga på begäran toohello kod körs. Det finns flera prestandafördelarna som är associerade med leverans business logic toohello databas:
  
  * Batchbearbetning – utvecklare kan gruppera åtgärder som infogar och skicka dem gruppvis. hello trafik Nätverksfördröjningen kostnad och hello store övergripande toocreate separata transaktioner minskas avsevärt. 
  * Före kompileringen – Cosmos DB precompiles lagrade procedurer, utlösare och användardefinierade funktioner (UDF) tooavoid JavaScript kompilering kostnaden för varje anrop. hello amorteras kostnader för att skapa hello bytekod för hello procedurmässig logik är tooa minimalt värde.
  * Sekvensering – många åtgärder måste en sidoeffekt (”utlösaren”) som potentiellt omfattar en eller flera sekundära store-operationer. Utöver odelbarhet, detta är mer performant när flyttas toohello server. 
* **Inkapsling:** lagrade procedurer kan vara används toogroup affärslogik på ett ställe. Detta har två fördelar:
  * Det lägger till ett Abstraktionslager ovanpå hello rådata som gör att data arkitekter tooevolve sina program oberoende av hello data. Detta är särskilt användbar när hello data är schema-mindre på grund av toohello sprödbrott antaganden som kan behöva toobe inbyggd i hello program om de har toodeal med data direkt.  
  * Denna framställning kan företag skydda sina data genom att effektivisera hello åtkomst från hello skript.  

hello skapas och körning av databasutlösare, lagrad procedur och anpassade frågeoperatorer stöds via hello [REST API](/rest/api/documentdb/), [Azure DocumentDB Studio](https://github.com/mingaliu/DocumentDBStudio/releases), och [client SDK](documentdb-sdk-dotnet.md) på flera olika plattformar inklusive .NET, Node.js och JavaScript.

Den här kursen använder hello [Node.js SDK med Q löftena](http://azure.github.io/azure-documentdb-node-q/) tooillustrate syntax och användning av lagrade procedurer, utlösare och UDF: er.   

## <a name="stored-procedures"></a>Lagrade procedurer
### <a name="example-write-a-simple-stored-procedure"></a>Exempel: Skriv en enkel lagrad procedur
Låt oss börja med en lagrad procedur som returnerar svaret ”Hello World”.

    var helloWorldStoredProc = {
        id: "helloWorld",
        serverScript: function () {
            var context = getContext();
            var response = context.getResponse();

            response.setBody("Hello, World");
        }
    }


Lagrade procedurer registreras per samling och kan användas på alla dokument och bifogade filer i samlingen. hello följande utdrag visar hur tooregister hello helloWorld lagrade proceduren med en samling. 

    // register hello stored procedure
    var createdStoredProcedure;
    client.createStoredProcedureAsync('dbs/testdb/colls/testColl', helloWorldStoredProc)
        .then(function (response) {
            createdStoredProcedure = response.resource;
            console.log("Successfully created stored procedure");
        }, function (error) {
            console.log("Error", error);
        });


När hello lagrade proceduren har registrerats kan vi köra den mot hello samlingen och läsa hello resultaten tillbaka på hello-klienten. 

    // execute hello stored procedure
    client.executeStoredProcedureAsync('dbs/testdb/colls/testColl/sprocs/helloWorld')
        .then(function (response) {
            console.log(response.result); // "Hello, World"
        }, function (err) {
            console.log("Error", error);
        });


hello context-objektet ger åtkomst tooall åtgärder som kan utföras på Cosmos-databaslagring, samt komma åt toohello förfrågan och svar objekt. Vi använde i det här fallet hello objektet tooset hello svarstexten hello-svar som skickades tillbaka toohello klienten. Mer information finns i toohello [server Azure Cosmos DB JavaScript SDK-dokumentationen](http://azure.github.io/azure-documentdb-js-server/).  

Låt oss expanderar på det här exemplet och lägga till flera databasen relaterade funktioner toohello lagrade proceduren. Lagrade procedurer kan skapa, uppdatera, läsa, fråga och ta bort dokument och bifogade filer i hello samling.    

### <a name="example-write-a-stored-procedure-toocreate-a-document"></a>Exempel: Skriv en lagrad procedur toocreate ett dokument
hello nästa utdrag visar hur toouse hello kontexten objektet toointeract med Cosmos-DB-resurser.

    var createDocumentStoredProc = {
        id: "createMyDocument",
        serverScript: function createMyDocument(documentToCreate) {
            var context = getContext();
            var collection = context.getCollection();

            var accepted = collection.createDocument(collection.getSelfLink(),
                  documentToCreate,
                  function (err, documentCreated) {
                      if (err) throw new Error('Error' + err.message);
                      context.getResponse().setBody(documentCreated.id)
                  });
            if (!accepted) return;
        }
    }


Den här lagrade proceduren tar inkommande documentToCreate, hello brödtexten i ett dokument toobe som skapats i hello samlingen. Dessa åtgärder är asynkron och beroende av JavaScript-funktionen återanrop. hello Återanropsfunktionen har två parametrar, en för hello felobjektet om hello misslyckas och en för hello skapade objektet. Inuti hello återanrop kan användare hantera hello undantag eller ett fel genereras. Om ett återanrop har angetts och det finns ett fel, genererar hello Azure Cosmos DB runtime ett fel.   

I hello-exemplet ovan genererar hello återanrop ett fel om hello-åtgärden misslyckades. Annars anges hello-id för hello skapat dokument som hello brödtext hello svar toohello klienten. Här är hur den här lagrade proceduren körs med indataparametrar.

    // register hello stored procedure
    client.createStoredProcedureAsync('dbs/testdb/colls/testColl', createDocumentStoredProc)
        .then(function (response) {
            var createdStoredProcedure = response.resource;

            // run stored procedure toocreate a document
            var docToCreate = {
                id: "DocFromSproc",
                book: "hello Hitchhiker’s Guide toohello Galaxy",
                author: "Douglas Adams"
            };

            return client.executeStoredProcedureAsync('dbs/testdb/colls/testColl/sprocs/createMyDocument',
                  docToCreate);
        }, function (error) {
            console.log("Error", error);
        })
    .then(function (response) {
        console.log(response); // "DocFromSproc"
    }, function (error) {
        console.log("Error", error);
    });


Observera att detta lagrade proceduren kan ändrade tootake en matris med dokumentet organ som indata och skapar dem i samma lagras hello procedurkörningen istället för flera nätverk begär toocreate av dem individuellt. Detta kan vara används tooimplement en effektiv bulk-import för Cosmos-DB (beskrivs senare i den här självstudiekursen).   

hello exempel beskrivs visas hur toouse lagrade procedurer. Senare i självstudiekursen hello tar vi upp utlösare och användardefinierade funktioner (UDF).

## <a name="database-program-transactions"></a>Programmet databastransaktioner
Transaktion i en typisk databas kan definieras som en sekvens med åtgärder som utförs som en logisk enhet på arbetet. Varje transaktion innehåller **ACID garantier**. AV är en välkänd förkortning som står för fyra egenskaper - odelbarhet, konsekvens, isolering och hållbarhet.  

En kort, odelbarhet garanterar att alla hello arbete som utförs i en transaktion behandlas som en enhet där antingen alla strävar eller none. Konsekvenskontroll ser till att hello data alltid är i ett internt tillstånd över transaktioner. Isolering garanterar att inga två transaktioner störa varandra – Allmänt, de flesta kommersiella system ger flera isoleringsnivåer som kan användas baserat på hello programbehov. Hållbarhet säkerställer att alla ändringar genomförs i hello databasen alltid är tillgänglig.   

I Cosmos DB JavaScript finns i hello samma minnesutrymme som hello-databas. Därför begäranden som görs i lagrade procedurer och utlösare köra i hello samma en session i databasen. Detta gör att Cosmos DB tooguarantee av för alla åtgärder som ingår i en enda lagrade proceduren/utlösare. Tänk hello följande lagrade Procedurdefinition:

    // JavaScript source code
    var exchangeItemsSproc = {
        id: "exchangeItems",
        serverScript: function (playerId1, playerId2) {
            var context = getContext();
            var collection = context.getCollection();
            var response = context.getResponse();

            var player1Document, player2Document;

            // query for players
            var filterQuery = 'SELECT * FROM Players p where p.id  = "' + playerId1 + '"';
            var accept = collection.queryDocuments(collection.getSelfLink(), filterQuery, {},
                function (err, documents, responseOptions) {
                    if (err) throw new Error("Error" + err.message);

                    if (documents.length != 1) throw "Unable toofind both names";
                    player1Document = documents[0];

                    var filterQuery2 = 'SELECT * FROM Players p where p.id = "' + playerId2 + '"';
                    var accept2 = collection.queryDocuments(collection.getSelfLink(), filterQuery2, {},
                        function (err2, documents2, responseOptions2) {
                            if (err2) throw new Error("Error" + err2.message);
                            if (documents2.length != 1) throw "Unable toofind both names";
                            player2Document = documents2[0];
                            swapItems(player1Document, player2Document);
                            return;
                        });
                    if (!accept2) throw "Unable tooread player details, abort ";
                });

            if (!accept) throw "Unable tooread player details, abort ";

            // swap hello two players’ items
            function swapItems(player1, player2) {
                var player1ItemSave = player1.item;
                player1.item = player2.item;
                player2.item = player1ItemSave;

                var accept = collection.replaceDocument(player1._self, player1,
                    function (err, docReplaced) {
                        if (err) throw "Unable tooupdate player 1, abort ";

                        var accept2 = collection.replaceDocument(player2._self, player2,
                            function (err2, docReplaced2) {
                                if (err) throw "Unable tooupdate player 2, abort"
                            });

                        if (!accept2) throw "Unable tooupdate player 2, abort";
                    });

                if (!accept) throw "Unable tooupdate player 1, abort";
            }
        }
    }

    // register hello stored procedure in Node.js client
    client.createStoredProcedureAsync(collection._self, exchangeItemsSproc)
        .then(function (response) {
            var createdStoredProcedure = response.resource;
        }
    );

Den här lagrade proceduren använder transaktioner inom spel app tootrade objekt mellan två spelare i en enda åtgärd. hello lagrade proceduren försök tooread två dokument varje motsvarande toohello spelare ID skickades som ett argument. Om båda player-dokumenten finns uppdaterar hello lagrade proceduren hello dokument genom att byta sina objekt. Om fel uppstår hello vägen utlöser ett JavaScript-undantag som implicit avbryter hello transaktion.

Om hello samling hello lagrade proceduren har registrerats mot är en enskild partition samling och sedan hello transaktion är begränsade tooall hello dokument inom hello samlingen. Om hello samlingen är partitionerad utförs lagrade procedurer i hello transaktionsomfånget för en enskild partitionsnyckel. Varje lagrade proceduren körning innehålla ett värde för partitionen som motsvarande toohello omfång hello transaction måste köras under. Mer information finns i [Azure Cosmos DB partitionering](partition-data.md).

### <a name="commit-and-rollback"></a>Commit och rollback
Transaktioner är djupt och internt integrerade i Cosmos DB JavaScript-programmeringsmodell. I JavaScript-funktionen radbryts automatiskt alla åtgärder under en enda transaktion. Om hello JavaScript har slutförts utan några undantag hello driftdatabasen toohello genomförs. I praktiken är hello ”BEGIN TRANSACTION” och ”COMMIT TRANSACTION” satser i relationsdatabaser implicit i Cosmos-databasen.  

Om alla undantag sprids från hello skript återställs Cosmos DB JavaScript-körning hello hela transaktionen. Enligt hello tidigare är exempelvis ett undantagsfel utlöses effektivt motsvarande tooa ”ROLLBACK TRANSACTION” i Cosmos-databasen.

### <a name="data-consistency"></a>Datakonsekvens
Lagrade procedurer och utlösare körs alltid på hello primära replik hello Azure Cosmos DB behållare. Detta säkerställer att läsningar av inuti lagrade procedurer erbjudande stark konsekvens. Frågor med användardefinierade funktioner kan köras på hello primära eller en sekundär replik, men vi Kontrollera toomeet hello begärt konsekvensnivå genom att välja hello lämplig replica.

## <a name="bounded-execution"></a>Begränsad körning
Alla Cosmos-DB-åtgärder måste slutföra inom angivna hello servern begär timeout-varaktighet. Den här begränsningen gäller även tooJavaScript funktioner (lagrade procedurer, utlösare och användardefinierade funktioner). Om en åtgärd inte slutförs med tidsgränsen återställs hello transaktionen. JavaScript-funktioner måste slutföras inom tidsgränsen för hello eller implementera en fortsättning baserat modellen toobatch/återuppta körning.  

I ordning toosimplify utvecklingen av lagrade procedurer och utlösare toohandle tidsfrister, alla funktioner under hello samlingsobjektet (för att skapa, läsa, Ersätt och borttagning av dokument och bifogade filer) returnerar ett booleskt värde som representerar om som åtgärden kommer att slutföras. Om det här värdet är FALSKT är uppgift att hello tidsgränsen är om tooexpire och hello installationen skriva in körningen.  Åtgärder i kö tidigare toohello första typen Lagringsåtgärden är garanterat toocomplete om hello lagrade proceduren har slutförts i tid och inte kö inga fler begäranden.  

JavaScript-funktioner är också avgränsas i resursförbrukning. Cosmos DB reserverar genomströmning per samling baserat på hello etablerats storleken på ett databaskonto. Genomströmning uttrycks som ett normaliserat enhet av CPU, minne och i/o-förbrukning kallas frågeenheter eller RUs. JavaScript-funktioner kan användaren använda ett stort antal RUs inom en kort tid och få hastighet begränsad om hello samling gränsen har nåtts. Resurs-intensiva lagrade procedurer kan också vara i karantän tooensure tillgängligheten för primitiva databasåtgärder.  

### <a name="example-bulk-importing-data-into-a-database-program"></a>Exempel: Massredigera importera data till ett databasprogram
Nedan visas ett exempel på en lagrad procedur som har skrivits toobulk importera dokument till en samling. Observera hur hello lagras procedurkörningen handtag begränsas genom att kontrollera hello boolesk returvärde från createDocument och sedan använder hello antal dokument som infogas i varje anrop till hello lagrade proceduren tootrack och återuppta utvecklingen i batchar.

    function bulkImport(docs) {
        var collection = getContext().getCollection();
        var collectionLink = collection.getSelfLink();

        // hello count of imported docs, also used as current doc index.
        var count = 0;

        // Validate input.
        if (!docs) throw new Error("hello array is undefined or null.");

        var docsLength = docs.length;
        if (docsLength == 0) {
            getContext().getResponse().setBody(0);
        }

        // Call hello create API toocreate a document.
        tryCreate(docs[count], callback);

        // Note that there are 2 exit conditions:
        // 1) hello createDocument request was not accepted. 
        //    In this case hello callback will not be called, we just call setBody and we are done.
        // 2) hello callback was called docs.length times.
        //    In this case all documents were created and we don’t need toocall tryCreate anymore. Just call setBody and we are done.
        function tryCreate(doc, callback) {
            var isAccepted = collection.createDocument(collectionLink, doc, callback);

            // If hello request was accepted, callback will be called.
            // Otherwise report current count back toohello client, 
            // which will call hello script again with remaining set of docs.
            if (!isAccepted) getContext().getResponse().setBody(count);
        }

        // This is called when collection.createDocument is done in order tooprocess hello result.
        function callback(err, doc, options) {
            if (err) throw err;

            // One more document has been inserted, increment hello count.
            count++;

            if (count >= docsLength) {
                // If we created all documents, we are done. Just set hello response.
                getContext().getResponse().setBody(count);
            } else {
                // Create next document.
                tryCreate(docs[count], callback);
            }
        }
    }

## <a id="trigger"></a>Databasutlösare
### <a name="database-pre-triggers"></a>Före databasutlösare
Cosmos DB innehåller utlösare som körs eller som utlöses av en åtgärd på ett dokument. Du kan till exempel ange en före utlösare när du skapar ett dokument – före utlösaren ska köras innan hello dokumentet skapas. hello följande är ett exempel på hur före utlösare kan vara används toovalidate hello egenskaperna för ett dokument som har skapats:

    var validateDocumentContentsTrigger = {
        id: "validateDocumentContents",
        serverScript: function validate() {
            var context = getContext();
            var request = context.getRequest();

            // document toobe created in hello current operation
            var documentToCreate = request.getBody();

            // validate properties
            if (!("timestamp" in documentToCreate)) {
                var ts = new Date();
                documentToCreate["my timestamp"] = ts.getTime();
            }

            // update hello document that will be created
            request.setBody(documentToCreate);
        },
        triggerType: TriggerType.Pre,
        triggerOperation: TriggerOperation.Create
    }


Och hello motsvarande registrering på klientsidan Node.js-kod för hello utlösare:

    // register pre-trigger
    client.createTriggerAsync(collection.self, validateDocumentContentsTrigger)
        .then(function (response) {
            console.log("Created", response.resource);
            var docToCreate = {
                id: "DocWithTrigger",
                event: "Error",
                source: "Network outage"
            };

            // run trigger while creating above document 
            var options = { preTriggerInclude: "validateDocumentContents" };

            return client.createDocumentAsync(collection.self,
                  docToCreate, options);
        }, function (error) {
            console.log("Error", error);
        })
    .then(function (response) {
        console.log(response.resource); // document with timestamp property added
    }, function (error) {
        console.log("Error", error);
    });


Före utlösare kan inte ha indataparametrar. hello request-objektet kan vara används toomanipulate hello begärandemeddelandet som är associerat med hello åtgärden. Här hello före utlösaren körs med hello skapandet av ett dokument och hello begäran meddelandetexten innehåller hello dokumentet toobe som skapats i JSON-format.   

När utlösare är registrerade ange användare hello-åtgärder som inte har. Den här utlösaren har skapats med TriggerOperation.Create, vilket innebär att hello följande inte tillåts.

    var options = { preTriggerInclude: "validateDocumentContents" };

    client.replaceDocumentAsync(docToReplace.self,
                  newDocBody, options)
    .then(function (response) {
        console.log(response.resource);
    }, function (error) {
        console.log("Error", error);
    });

    // Fails, can’t use a create trigger in a replace operation

### <a name="database-post-triggers"></a>Efter databasutlösare
Efter utlösare som före utlösare associeras med en åtgärd på ett dokument och får inte indataparametrar. De körs **när** hello-åtgärden har slutförts och ha åtkomst toohello svarsmeddelande som skickas toohello klienten.   

hello som följande exempel visar efter utlösare i praktiken:

    var updateMetadataTrigger = {
        id: "updateMetadata",
        serverScript: function updateMetadata() {
            var context = getContext();
            var collection = context.getCollection();
            var response = context.getResponse();

            // document that was created
            var createdDocument = response.getBody();

            // query for metadata document
            var filterQuery = 'SELECT * FROM root r WHERE r.id = "_metadata"';
            var accept = collection.queryDocuments(collection.getSelfLink(), filterQuery,
                updateMetadataCallback);
            if(!accept) throw "Unable tooupdate metadata, abort";

            function updateMetadataCallback(err, documents, responseOptions) {
                if(err) throw new Error("Error" + err.message);
                         if(documents.length != 1) throw 'Unable toofind metadata document';

                         var metadataDocument = documents[0];

                         // update metadata
                         metadataDocument.createdDocuments += 1;
                         metadataDocument.createdNames += " " + createdDocument.id;
                         var accept = collection.replaceDocument(metadataDocument._self,
                               metadataDocument, function(err, docReplaced) {
                                      if(err) throw "Unable tooupdate metadata, abort";
                               });
                         if(!accept) throw "Unable tooupdate metadata, abort";
                         return;                    
            }                                                                                            
        },
        triggerType: TriggerType.Post,
        triggerOperation: TriggerOperation.All
    }


hello utlösare kan registreras som visas i följande exempel hello.

    // register post-trigger
    client.createTriggerAsync('dbs/testdb/colls/testColl', updateMetadataTrigger)
        .then(function(createdTrigger) { 
            var docToCreate = { 
                name: "artist_profile_1023",
                artist: "hello Band",
                albums: ["Hellujah", "Rotators", "Spinning Top"]
            };

            // run trigger while creating above document 
            var options = { postTriggerInclude: "updateMetadata" };

            return client.createDocumentAsync(collection.self,
                  docToCreate, options);
        }, function(error) {
            console.log("Error" , error);
        })
    .then(function(response) {
        console.log(response.resource); 
    }, function(error) {
        console.log("Error" , error);
    });


Den här utlösaren frågar för hello metadata dokument och uppdaterar med information om hello nyskapade dokumentet.  

En sak som är viktigt toonote hello **transaktionella** körning av utlösare i Cosmos-databasen. Den här efter utlösaren körs som en del av hello samma transaktion som hello skapandet av hello originaldokumentet. Därför, om vi utlöser ett undantag från hello efter utlösare (t.ex. Om vi tooupdate hello Metadatadokumentet) hello hela transaktionen misslyckas och återställas. Inget dokument kommer att skapas och ett undantag ska returneras.  

## <a id="udf"></a>Användardefinierade funktioner
Användardefinierade funktioner (UDF) är används tooextend hello DocumentDB API SQL query language grammatik och implementera anpassad affärslogik. De kan endast anropas från inuti frågor. De har inte åtkomst toohello context-objektet och är avsedda toobe som används som endast beräkning JavaScript. Därför kan du köra UDF: er på sekundära repliker för hello Cosmos-DB-tjänsten.  

hello följande exempel skapar en UDF toocalculate skatter baserat på priser för olika intäkter hakparenteser och använder sedan den i en fråga toofind alla personer som betald mer än 20 000 i skatter.

    var taxUdf = {
        id: "tax",
        serverScript: function tax(income) {

            if(income == undefined) 
                throw 'no input';

            if (income < 1000) 
                return income * 0.1;
            else if (income < 10000) 
                return income * 0.2;
            else
                return income * 0.4;
        }
    }


hello UDF kan därefter användas i frågor som i följande exempel hello:

    // register UDF
    client.createUserDefinedFunctionAsync('dbs/testdb/colls/testColl', taxUdf)
        .then(function(response) { 
            console.log("Created", response.resource);

            var query = 'SELECT * FROM TaxPayers t WHERE udf.tax(t.income) > 20000'; 
            return client.queryDocuments('dbs/testdb/colls/testColl',
                   query).toArrayAsync();
        }, function(error) {
            console.log("Error" , error);
        })
    .then(function(response) {
        var documents = response.feed;
        console.log(response.resource); 
    }, function(error) {
        console.log("Error" , error);
    });

## <a name="javascript-language-integrated-query-api"></a>JavaScript språkintegrerade frågan API
Dessutom tooissuing frågor med DocumentDB SQL-grammatik, hello serversidan SDK kan du tooperform optimerade frågor med ett flytande JavaScript-gränssnitt utan kännedom om SQL. hello JavaScript frågan API kan du tooprogrammatically bygga frågor genom att skicka predikat funktioner till chainable funktionen anropas med en syntax bekant tooECMAScript5's matris built-ins och populära JavaScript-bibliotek som lodash. Frågor tolkas av hello JavaScript-körning toobe köras effektivt med hjälp av Azure Cosmos DB.

> [!NOTE]
> `__`(double understreck) är ett alias för`getContext().getCollection()`.
> <br/>
> Med andra ord kan du använda `__` eller `getContext().getCollection()` tooaccess hello JavaScript API för frågan.
> 
> 

Funktioner som stöds är:

<ul>
<li>
<b>... chain(). värdet ([återanrop] [, alternativ])</b>
<ul>
<li>
Startar en länkad anrop som måste avslutas med value().
</li>
</ul>
</li>
<li>
<b>filter (predicateFunction [, alternativ] [, motringning])</b>
<ul>
<li>
Filtrerar hello indatavärdet med en predikatfunktionens som returnerar SANT/FALSKT i ordning toofilter in/ut inkommande dokument i hello resultatmängden. Detta fungerar liknande tooa WHERE-satsen i SQL.
</li>
</ul>
</li>
<li>
<b>karta (transformationFunction [, alternativ] [, motringning])</b>
<ul>
<li>
Gäller en projektion som anges en transformation-funktion som mappar varje inkommande objektet tooa JavaScript-objekt eller -värde. Detta fungerar liknande tooa villkoret SELECT i SQL.
</li>
</ul>
</li>
<li>
<b>pluck ([egenskapsnamn] [, alternativ] [, motringning])</b>
<ul>
<li>
Det här är en genväg till en karta som extraherar hello-värdet för en enda egenskap från varje inkommande objekt.
</li>
</ul>
</li>
<li>
<b>förenkla ([isShallow] [, alternativ] [, motringning])</b>
<ul>
<li>
Kombinerar och förenklas matriser från varje inkommande objekt i tooa enda matris. Detta fungerar liknande tooSelectMany i LINQ.
</li>
</ul>
</li>
<li>
<b>sortBy ([predicate] [, alternativ] [, motringning])</b>
<ul>
<li>
Skapa en ny uppsättning dokument genom att sortera hello dokument i hello Indatadokumentet dataström i stigande ordning med hjälp av hello angivna predikat. Detta fungerar liknande tooa ORDER BY-satsen i SQL.
</li>
</ul>
</li>
<li>
<b>sortByDescending ([predicate] [, alternativ] [, motringning])</b>
<ul>
<li>
Skapa en ny uppsättning dokument genom att sortera hello dokument i hello Indatadokumentet dataström i fallande ordning med hjälp av hello angivna predikat. Detta fungerar liknande tooa x DESC ORDER BY-satsen i SQL.
</li>
</ul>
</li>
</ul>


När ingår i predikatet och/eller selector funktioner få hello följande JavaScript-konstruktioner automatiskt optimerade toorun direkt på Azure Cosmos DB index:

* Enkel operatörer: = + - * / % | ^ &amp; == != === !=== &lt; &gt; &lt;= &gt;= || &amp;&amp; &lt;&lt; &gt;&gt; &gt;&gt;&gt;! ~
* Literaler, inklusive hello objektet literal: {}
* varians returnerade

inte hämta optimerad hello följande JavaScript-konstruktioner för Azure Cosmos DB index:

* Åtkomstkontrollflödet (t.ex. om, medan)
* Funktionsanrop

Mer information, se vår [serversidan JSDocs](http://azure.github.io/azure-documentdb-js-server/).

### <a name="example-write-a-stored-procedure-using-hello-javascript-query-api"></a>Exempel: Skriv en lagrad procedur med hello JavaScript fråge-API
hello följande kodexempel är ett exempel på hur hello JavaScript frågan API kan användas i hello kontexten för en lagrad procedur. hello lagrade proceduren infogar ett dokument som anges av en indataparameter och uppdaterar ett metadata-dokument med hello `__.filter()` metod med minstorlek maxSize och totalSize baserat på hello Indatadokumentet egenskapen.

    /**
     * Insert actual doc and update metadata doc: minSize, maxSize, totalSize based on doc.size.
     */
    function insertDocumentAndUpdateMetadata(doc) {
      // HTTP error codes sent tooour callback funciton by DocDB server.
      var ErrorCode = {
        RETRY_WITH: 449,
      }

      var isAccepted = __.createDocument(__.getSelfLink(), doc, {}, function(err, doc, options) {
        if (err) throw err;

        // Check hello doc (ignore docs with invalid/zero size and metaDoc itself) and call updateMetadata.
        if (!doc.isMetadata && doc.size > 0) {
          // Get hello meta document. We keep it in hello same collection. it's hello only doc that has .isMetadata = true.
          var result = __.filter(function(x) {
            return x.isMetadata === true
          }, function(err, feed, options) {
            if (err) throw err;

            // We assume that metadata doc was pre-created and must exist when this script is called.
            if (!feed || !feed.length) throw new Error("Failed toofind hello metadata document.");

            // hello metadata document.
            var metaDoc = feed[0];

            // Update metaDoc.minSize:
            // for 1st document use doc.Size, for all hello rest see if it's less than last min.
            if (metaDoc.minSize == 0) metaDoc.minSize = doc.size;
            else metaDoc.minSize = Math.min(metaDoc.minSize, doc.size);

            // Update metaDoc.maxSize.
            metaDoc.maxSize = Math.max(metaDoc.maxSize, doc.size);

            // Update metaDoc.totalSize.
            metaDoc.totalSize += doc.size;

            // Update/replace hello metadata document in hello store.
            var isAccepted = __.replaceDocument(metaDoc._self, metaDoc, function(err) {
              if (err) throw err;
              // Note: in case concurrent updates causes conflict with ErrorCode.RETRY_WITH, we can't read hello meta again 
              //       and update again because due tooSnapshot isolation we will read same exact version (we are in same transaction).
              //       We have tootake care of that on hello client side.
            });
            if (!isAccepted) throw new Error("replaceDocument(metaDoc) returned false.");
          });
          if (!result.isAccepted) throw new Error("filter for metaDoc returned false.");
        }
      });
      if (!isAccepted) throw new Error("createDocument(actual doc) returned false.");
    }

## <a name="sql-toojavascript-cheat-sheet"></a>SQL tooJavascript fusklapp
hello följande tabell innehåller olika SQL-frågor och hello motsvarande JavaScript-frågor.

Eftersom dokumentet egenskapen nycklar med SQL-frågor (t.ex. `doc.id`) är skiftlägeskänsliga.

|SQL| JavaScript-fråga API|Beskrivningen nedan|
|---|---|---|
|VÄLJ *<br>FRÅN dokument| __.Map(Function(doc) { <br>&nbsp;&nbsp;&nbsp;&nbsp;returnera doc;<br>});|1|
|Välj docs.id, docs.message som ignorerad, docs.actions <br>FRÅN dokument|__.Map(Function(doc) {<br>&nbsp;&nbsp;&nbsp;&nbsp;returnera {<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;ID: doc.id,<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;meddelande: doc.message,<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Actions:doc.Actions<br>&nbsp;&nbsp;&nbsp;&nbsp;};<br>});|2|
|VÄLJ *<br>FRÅN dokument<br>VAR docs.id="X998_Y998”|__.filter(Function(doc) {<br>&nbsp;&nbsp;&nbsp;&nbsp;returnera doc.id === ”X998_Y998”;<br>});|3|
|VÄLJ *<br>FRÅN dokument<br>VAR ARRAY_CONTAINS (dokument. Taggar 123)|__.filter(Function(x) {<br>&nbsp;&nbsp;&nbsp;&nbsp;returnera x.Tags & & x.Tags.indexOf(123) > -1;<br>});|4|
|Välj docs.id, docs.message som ignorerad<br>FRÅN dokument<br>VAR docs.id="X998_Y998”|__.chain()<br>&nbsp;&nbsp;&nbsp;&nbsp;.filter(Function(doc) {<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;returnera doc.id === ”X998_Y998”;<br>&nbsp;&nbsp;&nbsp;&nbsp;})<br>&nbsp;&nbsp;&nbsp;&nbsp;.Map(Function(doc) {<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;returnera {<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;ID: doc.id,<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;meddelande: doc.message<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;};<br>&nbsp;&nbsp;&nbsp;&nbsp;})<br>.Value();|5|
|SELECT VALUE-tagg<br>FRÅN dokument<br>Anslut tagg i dokumenten. Taggar<br>ORDER BY docs._ts|__.chain()<br>&nbsp;&nbsp;&nbsp;&nbsp;.filter(Function(doc) {<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;returnera dokument. Taggar & & Array.isArray (doc. Taggar).<br>&nbsp;&nbsp;&nbsp;&nbsp;})<br>&nbsp;&nbsp;&nbsp;&nbsp;.sortBy(function(doc) {<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;returnera doc._ts;<br>&nbsp;&nbsp;&nbsp;&nbsp;})<br>&nbsp;&nbsp;&nbsp;&nbsp;.pluck("Tags")<br>&nbsp;&nbsp;&nbsp;&nbsp;.flatten()<br>&nbsp;&nbsp;&nbsp;&nbsp;.Value()|6|

hello förklarar följande beskrivningar varje fråga i hello tabellen ovan.
1. Leder till att alla dokument (sidbrytning med fortsättningstoken) som är.
2. Projekt hello-id, meddelande (ett alias toomsg) och åtgärd från alla dokument.
3. Frågor för dokument med hello predikat: id = ”X998_Y998”.
4. Frågor för dokument som har en property-taggar och taggarna är en matris som innehåller hello värde 123.
5. Frågor för dokument med ett predikat, id = ”X998_Y998” och sedan projekt hello-id och meddelandet (ett alias toomsg).
6. Filter för dokument som har en matrisegenskap, taggar och sorterar hello resulterande dokument efter hello _ts system tidsstämpelsegenskapen, och sedan projekt + förenklas hello taggar matris.


## <a name="runtime-support"></a>Stöd för körning
[DocumentDB JavaScript server side API](http://azure.github.io/azure-documentdb-js-server/) ger stöd för hello de flesta av hello vanlig funktioner för JavaScript-språk som standardiserade av [ECMA-262](http://www.ecma-international.org/publications/standards/Ecma-262.htm).

### <a name="security"></a>Säkerhet
JavaScript-lagrade procedurer och utlösare är i begränsat läge så att hello effekterna av ett skript inte läcker toohello andra utan att gå via hello transaktion ögonblicksbildisolering på hello databasnivå. hello runtime miljöer pool men rengöras hello kontexten efter varje körning. Därför att de är garanterat toobe säker av alla oavsiktliga sidoeffekter från varandra.

### <a name="pre-compilation"></a>Före kompileringen
Lagrade procedurer, utlösare och UDF: er är implicit förkompilerade toohello byte kodformat i ordning tooavoid kompilering kostnad vid hello tiden för varje skript-anrop. Detta säkerställer att anrop för lagrade procedurer är snabb och har en låg storleken.

## <a name="client-sdk-support"></a>Stöd för klient-SDK
I tillägg toohello DocumentDB API för [Node.js](documentdb-sdk-node.md) Azure DB som Cosmos-klient har [.NET](documentdb-sdk-dotnet.md), [.NET Core](documentdb-sdk-dotnet-core.md), [Java](documentdb-sdk-java.md), [ JavaScript](http://azure.github.io/azure-documentdb-js/), och [Python SDK](documentdb-sdk-python.md) för hello DocumentDB-API. Lagrade procedurer, utlösare och UDF: er kan skapas och köras med hjälp av dessa SDK: er samt. följande exempel visar hur hello toocreate och köra en lagrad procedur med hello .NET-klienten. Observera hur hello .NET-typer har skickats till hello lagrade procedur som JSON och läsa tillbaka.

    var markAntiquesSproc = new StoredProcedure
    {
        Id = "ValidateDocumentAge",
        Body = @"
                function(docToCreate, antiqueYear) {
                    var collection = getContext().getCollection();    
                    var response = getContext().getResponse();    

                    if(docToCreate.Year != undefined && docToCreate.Year < antiqueYear){
                        docToCreate.antique = true;
                    }

                    collection.createDocument(collection.getSelfLink(), docToCreate, {}, 
                        function(err, docCreated, options) { 
                            if(err) throw new Error('Error while creating document: ' + err.message);                              
                            if(options.maxCollectionSizeInMb == 0) throw 'max collection size not found'; 
                            response.setBody(docCreated);
                    });
             }"
    };

    // register stored procedure
    StoredProcedure createdStoredProcedure = await client.CreateStoredProcedureAsync(UriFactory.CreateDocumentCollectionUri("db", "coll"), markAntiquesSproc);
    dynamic document = new Document() { Id = "Borges_112" };
    document.Title = "Aleph";
    document.Year = 1949;

    // execute stored procedure
    Document createdDocument = await client.ExecuteStoredProcedureAsync<Document>(UriFactory.CreateStoredProcedureUri("db", "coll", "sproc"), document, 1920);


Det här exemplet visas hur toouse hello [DocumentDB .NET API](/dotnet/api/overview/azure/cosmosdb?view=azure-dotnet) toocreate före utlösare och skapa ett dokument med hello utlösaren aktiveras. 

    Trigger preTrigger = new Trigger()
    {
        Id = "CapitalizeName",
        Body = @"function() {
            var item = getContext().getRequest().getBody();
            item.id = item.id.toUpperCase();
            getContext().getRequest().setBody(item);
        }",
        TriggerOperation = TriggerOperation.Create,
        TriggerType = TriggerType.Pre
    };

    Document createdItem = await client.CreateDocumentAsync(UriFactory.CreateDocumentCollectionUri("db", "coll"), new Document { Id = "documentdb" },
        new RequestOptions
        {
            PreTriggerInclude = new List<string> { "CapitalizeName" },
        });


Och hello som följande exempel visar hur toocreate användaren definierat funktion (UDF) och använda den i en [DocumentDB API SQL-frågan](documentdb-sql-query.md).

    UserDefinedFunction function = new UserDefinedFunction()
    {
        Id = "LOWER",
        Body = @"function(input) 
        {
            return input.toLowerCase();
        }"
    };

    foreach (Book book in client.CreateDocumentQuery(UriFactory.CreateDocumentCollectionUri("db", "coll"),
        "SELECT * FROM Books b WHERE udf.LOWER(b.Title) = 'war and peace'"))
    {
        Console.WriteLine("Read {0} from query", book);
    }

## <a name="rest-api"></a>REST API
Alla Azure DB som Cosmos-åtgärder kan utföras på ett RESTful sätt. Lagrade procedurer, utlösare och användardefinierade funktioner kan registreras i en samling med hjälp av HTTP POST. hello följande är ett exempel på hur tooregister en lagrad procedur:

    POST https://<url>/sprocs/ HTTP/1.1
    authorization: <<auth>>
    x-ms-date: Thu, 07 Aug 2014 03:43:10 GMT


    var x = {
      "name": "createAndAddProperty",
      "body": function (docToCreate, addedPropertyName, addedPropertyValue) {
                var collectionManager = getContext().getCollection();
                collectionManager.createDocument(
                    collectionManager.getSelfLink(),
                    docToCreate,
                    function(err, docCreated) {
                      if(err) throw new Error('Error:  ' + err.message);
                      docCreated[addedPropertyName] = addedPropertyValue;
                      getContext().getResponse().setBody(docCreated);
                    });
            }
    }


hello lagrade proceduren registreras genom att köra en POST-begäran mot hello URI dbs/testdb/colls/testColl/sprocs med hello brödtext som innehåller hello lagrade proceduren toocreate. Utlösare och UDF: er kan registreras på samma sätt genom att utfärda ett INLÄGG mot /triggers och /udfs respektive.
Det här lagrade proceduren kan sedan köras genom att utfärda en POST-begäran mot dess resurslänken:

    POST https://<url>/sprocs/<sproc> HTTP/1.1
    authorization: <<auth>>
    x-ms-date: Thu, 07 Aug 2014 03:43:20 GMT


    [ { "name": "TestDocument", "book": "Autumn of hello Patriarch"}, "Price", 200 ]


Här kan skickas hello inkommande toohello lagrade proceduren hello frågans brödtext. Observera att hello indata skickas som ett JSON-matris av indataparametrar. hello lagrade proceduren tar hello första indata som ett dokument som är en brödtext för svar. hello-svar som vi får är följande:

    HTTP/1.1 200 OK

    { 
      name: 'TestDocument',
      book: ‘Autumn of hello Patriarch’,
      id: ‘V7tQANV3rAkDAAAAAAAAAA==‘,
      ts: 1407830727,
      self: ‘dbs/V7tQAA==/colls/V7tQANV3rAk=/docs/V7tQANV3rAkDAAAAAAAAAA==/’,
      etag: ‘6c006596-0000-0000-0000-53e9cac70000’,
      attachments: ‘attachments/’,
      Price: 200
    }


Utlösare, till skillnad från lagrade procedurer kan inte köras direkt. I stället körs de som en del av en åtgärd på ett dokument. Vi kan ange hello utlösare toorun med en förfrågan med en HTTP-huvuden. hello följer begäran toocreate ett dokument.

    POST https://<url>/docs/ HTTP/1.1
    authorization: <<auth>>
    x-ms-date: Thu, 07 Aug 2014 03:43:10 GMT
    x-ms-documentdb-pre-trigger-include: validateDocumentContents 
    x-ms-documentdb-post-trigger-include: bookCreationPostTrigger


    {
       "name": "newDocument",
       “title”: “hello Wizard of Oz”,
       “author”: “Frank Baum”,
       “pages”: 92
    }


Här anges hello före utlösaren toobe kör med hello begäran i hello x-ms-documentdb-pre-trigger-include huvudet. På motsvarande sätt anges efter utlösare i hello x-ms-documentdb-post-trigger-include huvud. Observera att både före och efter utlösare kan anges för en viss begäran.

## <a name="sample-code"></a>Exempelkod
Du kan hitta mer kodexempel för serversidan (inklusive [massborttagning](https://github.com/Azure/azure-documentdb-js-server/tree/master/samples/stored-procedures/bulkDelete.js), och [uppdatera](https://github.com/Azure/azure-documentdb-js-server/tree/master/samples/stored-procedures/update.js)) på vår [GitHub-lagringsplatsen](https://github.com/Azure/azure-documentdb-js-server/tree/master/samples).

Om du vill tooshare din helt otroligt lagrade proceduren? Skicka oss en pull-begäran! 

## <a name="next-steps"></a>Nästa steg
När du har en eller flera lagrade procedurer, utlösare och användardefinierade funktioner som har skapats kan du läsa in dem och visa dem i hello Azure-portalen med Data Explorer.

Du kan också hitta hello följande referenser och resurser som är användbara i din sökväg toolearn mer om Azure Cosmos dB serversidan programmering:

* [Azure Cosmos DB SDK](documentdb-sdk-dotnet.md)
* [DocumentDB-Studio](https://github.com/mingaliu/DocumentDBStudio/releases)
* [JSON](http://www.json.org/) 
* [JavaScript ECMA-262](http://www.ecma-international.org/publications/standards/Ecma-262.htm)
* [Säker och bärbar databasen utökningsbarhet](http://dl.acm.org/citation.cfm?id=276339) 
* [Tjänsten inriktade Database-arkitektur](http://dl.acm.org/citation.cfm?id=1066267&coll=Portal&dl=GUIDE) 
* [Värd för hello .NET Runtime i Microsoft SQL server](http://dl.acm.org/citation.cfm?id=1007669)

