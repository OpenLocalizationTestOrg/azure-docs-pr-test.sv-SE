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
# <a name="azure-cosmos-db-server-side-programming-stored-procedures-database-triggers-and-udfs"></a><span data-ttu-id="e8c04-105">Azure DB Cosmos serversidan programmering: lagrade procedurer, databasutlösare och UDF: er</span><span class="sxs-lookup"><span data-stu-id="e8c04-105">Azure Cosmos DB server-side programming: Stored procedures, database triggers, and UDFs</span></span>
<span data-ttu-id="e8c04-106">Lär dig hur Azure Cosmos DB språk integreras, transaktionell körning av JavaScript kan utvecklare skriva **lagrade procedurer**, **utlösare** och **användardefinierade funktioner (UDF)** internt i en [ECMAScript 2015](http://www.ecma-international.org/ecma-262/6.0/) JavaScript.</span><span class="sxs-lookup"><span data-stu-id="e8c04-106">Learn how Azure Cosmos DB’s language integrated, transactional execution of JavaScript lets developers write **stored procedures**, **triggers** and **user defined functions (UDFs)** natively in an [ECMAScript 2015](http://www.ecma-international.org/ecma-262/6.0/) JavaScript.</span></span> <span data-ttu-id="e8c04-107">Detta ger dig toowrite databasen program med programlogik som levererats och köras direkt på hello partitioner för lagring av databasen.</span><span class="sxs-lookup"><span data-stu-id="e8c04-107">This allows you toowrite database program application logic that can be shipped and executed directly on hello database storage partitions.</span></span> 

<span data-ttu-id="e8c04-108">Vi rekommenderar komma igång genom att titta på hello följande video, där Andrew Liu är en kort introduktion tooCosmos DBS serversidan databasen programmeringsmodell.</span><span class="sxs-lookup"><span data-stu-id="e8c04-108">We recommend getting started by watching hello following video, where Andrew Liu provides a brief introduction tooCosmos DB's server-side database programming model.</span></span> 

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Azure-Demo-A-Quick-Intro-to-Azure-DocumentDBs-Server-Side-Javascript/player]
> 
> 

<span data-ttu-id="e8c04-109">Gå sedan tillbaka toothis artikel, där du lär dig hello svar toohello följande frågor:</span><span class="sxs-lookup"><span data-stu-id="e8c04-109">Then, return toothis article, where you'll learn hello answers toohello following questions:</span></span>  

* <span data-ttu-id="e8c04-110">Hur jag skriva en lagrad procedur, utlösare och UDF med hjälp av JavaScript?</span><span class="sxs-lookup"><span data-stu-id="e8c04-110">How do I write a a stored procedure, trigger, or UDF using JavaScript?</span></span>
* <span data-ttu-id="e8c04-111">Hur garanterar Cosmos DB av?</span><span class="sxs-lookup"><span data-stu-id="e8c04-111">How does Cosmos DB guarantee ACID?</span></span>
* <span data-ttu-id="e8c04-112">Hur fungerar transaktioner i Cosmos-databasen?</span><span class="sxs-lookup"><span data-stu-id="e8c04-112">How do transactions work in Cosmos DB?</span></span>
* <span data-ttu-id="e8c04-113">Vad är utlöser före och efter utlöser och hur skriver jag en?</span><span class="sxs-lookup"><span data-stu-id="e8c04-113">What are pre-triggers and post-triggers and how do I write one?</span></span>
* <span data-ttu-id="e8c04-114">Hur registrerar jag och köra lagrade procedurer, utlösare och UDF RESTful sätt med hjälp av HTTP?</span><span class="sxs-lookup"><span data-stu-id="e8c04-114">How do I register and execute a stored procedure, trigger, or UDF in a RESTful manner by using HTTP?</span></span>
* <span data-ttu-id="e8c04-115">Vad Cosmos DB SDK: er är tillgängliga toocreate och köra lagrade procedurer, utlösare och UDF: er?</span><span class="sxs-lookup"><span data-stu-id="e8c04-115">What Cosmos DB SDKs are available toocreate and execute stored procedures, triggers, and UDFs?</span></span>

## <a name="introduction-toostored-procedure-and-udf-programming"></a><span data-ttu-id="e8c04-116">Introduktion tooStored proceduren och UDF programmering</span><span class="sxs-lookup"><span data-stu-id="e8c04-116">Introduction tooStored Procedure and UDF Programming</span></span>
<span data-ttu-id="e8c04-117">Den här metoden för *”JavaScript som en modern dag T-SQL”* Frigör programutvecklare från hello svårigheter av felmatchningar system och Objektrelationer mappning teknik.</span><span class="sxs-lookup"><span data-stu-id="e8c04-117">This approach of *“JavaScript as a modern day T-SQL”* frees application developers from hello complexities of type system mismatches and object-relational mapping technologies.</span></span> <span data-ttu-id="e8c04-118">Det finns också ett antal inbyggda fördelar som kan utnyttjade toobuild omfattande program:</span><span class="sxs-lookup"><span data-stu-id="e8c04-118">It also has a number of intrinsic advantages that can be utilized toobuild rich applications:</span></span>  

* <span data-ttu-id="e8c04-119">**Procedurmässig logik:** JavaScript som en hög nivå programmeringsspråk, ger en omfattande och välbekanta gränssnittet tooexpress affärslogik.</span><span class="sxs-lookup"><span data-stu-id="e8c04-119">**Procedural Logic:** JavaScript as a high level programming language, provides a rich and familiar interface tooexpress business logic.</span></span> <span data-ttu-id="e8c04-120">Du kan utföra komplexa sekvenser av operations närmare toohello data.</span><span class="sxs-lookup"><span data-stu-id="e8c04-120">You can perform complex sequences of operations closer toohello data.</span></span>
* <span data-ttu-id="e8c04-121">**Atomiska transaktioner:** Cosmos DB garanterar att databasen åtgärder som utförs i en lagrad procedur eller utlösare är atomiska.</span><span class="sxs-lookup"><span data-stu-id="e8c04-121">**Atomic Transactions:** Cosmos DB guarantees that database operations performed inside a single stored procedure or trigger are atomic.</span></span> <span data-ttu-id="e8c04-122">På så sätt kan ett program kombinera relaterade åtgärder i en enda grupp, så att alla lyckas eller ingen av dem lyckas.</span><span class="sxs-lookup"><span data-stu-id="e8c04-122">This lets an application combine related operations in a single batch so that either all of them succeed or none of them succeed.</span></span> 
* <span data-ttu-id="e8c04-123">**Prestanda:** hello faktum att JSON är är mappade toohello Javascript språk typsystemet och även hello grundläggande körningsenheten på lagring i Cosmos DB möjliggör ett antal prestandaoptimeringar som lazy materialisering av JSON-dokument i hello buffert poolen och gör dem tillgängliga på begäran toohello kod körs.</span><span class="sxs-lookup"><span data-stu-id="e8c04-123">**Performance:** hello fact that JSON is intrinsically mapped toohello Javascript language type system and is also hello basic unit of storage in Cosmos DB allows for a number of optimizations like lazy materialization of JSON documents in hello buffer pool and making them available on-demand toohello executing code.</span></span> <span data-ttu-id="e8c04-124">Det finns flera prestandafördelarna som är associerade med leverans business logic toohello databas:</span><span class="sxs-lookup"><span data-stu-id="e8c04-124">There are more performance benefits associated with shipping business logic toohello database:</span></span>
  
  * <span data-ttu-id="e8c04-125">Batchbearbetning – utvecklare kan gruppera åtgärder som infogar och skicka dem gruppvis.</span><span class="sxs-lookup"><span data-stu-id="e8c04-125">Batching – Developers can group operations like inserts and submit them in bulk.</span></span> <span data-ttu-id="e8c04-126">hello trafik Nätverksfördröjningen kostnad och hello store övergripande toocreate separata transaktioner minskas avsevärt.</span><span class="sxs-lookup"><span data-stu-id="e8c04-126">hello network traffic latency cost and hello store overhead toocreate separate transactions are reduced significantly.</span></span> 
  * <span data-ttu-id="e8c04-127">Före kompileringen – Cosmos DB precompiles lagrade procedurer, utlösare och användardefinierade funktioner (UDF) tooavoid JavaScript kompilering kostnaden för varje anrop.</span><span class="sxs-lookup"><span data-stu-id="e8c04-127">Pre-compilation – Cosmos DB precompiles stored procedures, triggers and user defined functions (UDFs) tooavoid JavaScript compilation cost for each invocation.</span></span> <span data-ttu-id="e8c04-128">hello amorteras kostnader för att skapa hello bytekod för hello procedurmässig logik är tooa minimalt värde.</span><span class="sxs-lookup"><span data-stu-id="e8c04-128">hello overhead of building hello byte code for hello procedural logic is amortized tooa minimal value.</span></span>
  * <span data-ttu-id="e8c04-129">Sekvensering – många åtgärder måste en sidoeffekt (”utlösaren”) som potentiellt omfattar en eller flera sekundära store-operationer.</span><span class="sxs-lookup"><span data-stu-id="e8c04-129">Sequencing – Many operations need a side-effect (“trigger”) that potentially involves doing one or many secondary store operations.</span></span> <span data-ttu-id="e8c04-130">Utöver odelbarhet, detta är mer performant när flyttas toohello server.</span><span class="sxs-lookup"><span data-stu-id="e8c04-130">Aside from atomicity, this is more performant when moved toohello server.</span></span> 
* <span data-ttu-id="e8c04-131">**Inkapsling:** lagrade procedurer kan vara används toogroup affärslogik på ett ställe.</span><span class="sxs-lookup"><span data-stu-id="e8c04-131">**Encapsulation:** Stored procedures can be used toogroup business logic in one place.</span></span> <span data-ttu-id="e8c04-132">Detta har två fördelar:</span><span class="sxs-lookup"><span data-stu-id="e8c04-132">This has two advantages:</span></span>
  * <span data-ttu-id="e8c04-133">Det lägger till ett Abstraktionslager ovanpå hello rådata som gör att data arkitekter tooevolve sina program oberoende av hello data.</span><span class="sxs-lookup"><span data-stu-id="e8c04-133">It adds an abstraction layer on top of hello raw data, which enables data architects tooevolve their applications independently from hello data.</span></span> <span data-ttu-id="e8c04-134">Detta är särskilt användbar när hello data är schema-mindre på grund av toohello sprödbrott antaganden som kan behöva toobe inbyggd i hello program om de har toodeal med data direkt.</span><span class="sxs-lookup"><span data-stu-id="e8c04-134">This is particularly advantageous when hello data is schema-less, due toohello brittle assumptions that may need toobe baked into hello application if they have toodeal with data directly.</span></span>  
  * <span data-ttu-id="e8c04-135">Denna framställning kan företag skydda sina data genom att effektivisera hello åtkomst från hello skript.</span><span class="sxs-lookup"><span data-stu-id="e8c04-135">This abstraction lets enterprises keep their data secure by streamlining hello access from hello scripts.</span></span>  

<span data-ttu-id="e8c04-136">hello skapas och körning av databasutlösare, lagrad procedur och anpassade frågeoperatorer stöds via hello [REST API](/rest/api/documentdb/), [Azure DocumentDB Studio](https://github.com/mingaliu/DocumentDBStudio/releases), och [client SDK](documentdb-sdk-dotnet.md) på flera olika plattformar inklusive .NET, Node.js och JavaScript.</span><span class="sxs-lookup"><span data-stu-id="e8c04-136">hello creation and execution of database triggers, stored procedure and custom query operators is supported through hello [REST API](/rest/api/documentdb/), [Azure DocumentDB Studio](https://github.com/mingaliu/DocumentDBStudio/releases), and [client SDKs](documentdb-sdk-dotnet.md) in many platforms including .NET, Node.js and JavaScript.</span></span>

<span data-ttu-id="e8c04-137">Den här kursen använder hello [Node.js SDK med Q löftena](http://azure.github.io/azure-documentdb-node-q/) tooillustrate syntax och användning av lagrade procedurer, utlösare och UDF: er.</span><span class="sxs-lookup"><span data-stu-id="e8c04-137">This tutorial uses hello [Node.js SDK with Q Promises](http://azure.github.io/azure-documentdb-node-q/) tooillustrate syntax and usage of stored procedures, triggers, and UDFs.</span></span>   

## <a name="stored-procedures"></a><span data-ttu-id="e8c04-138">Lagrade procedurer</span><span class="sxs-lookup"><span data-stu-id="e8c04-138">Stored procedures</span></span>
### <a name="example-write-a-simple-stored-procedure"></a><span data-ttu-id="e8c04-139">Exempel: Skriv en enkel lagrad procedur</span><span class="sxs-lookup"><span data-stu-id="e8c04-139">Example: Write a simple stored procedure</span></span>
<span data-ttu-id="e8c04-140">Låt oss börja med en lagrad procedur som returnerar svaret ”Hello World”.</span><span class="sxs-lookup"><span data-stu-id="e8c04-140">Let’s start with a simple stored procedure that returns a “Hello World” response.</span></span>

    var helloWorldStoredProc = {
        id: "helloWorld",
        serverScript: function () {
            var context = getContext();
            var response = context.getResponse();

            response.setBody("Hello, World");
        }
    }


<span data-ttu-id="e8c04-141">Lagrade procedurer registreras per samling och kan användas på alla dokument och bifogade filer i samlingen.</span><span class="sxs-lookup"><span data-stu-id="e8c04-141">Stored procedures are registered per collection, and can operate on any document and attachment present in that collection.</span></span> <span data-ttu-id="e8c04-142">hello följande utdrag visar hur tooregister hello helloWorld lagrade proceduren med en samling.</span><span class="sxs-lookup"><span data-stu-id="e8c04-142">hello following snippet shows how tooregister hello helloWorld stored procedure with a collection.</span></span> 

    // register hello stored procedure
    var createdStoredProcedure;
    client.createStoredProcedureAsync('dbs/testdb/colls/testColl', helloWorldStoredProc)
        .then(function (response) {
            createdStoredProcedure = response.resource;
            console.log("Successfully created stored procedure");
        }, function (error) {
            console.log("Error", error);
        });


<span data-ttu-id="e8c04-143">När hello lagrade proceduren har registrerats kan vi köra den mot hello samlingen och läsa hello resultaten tillbaka på hello-klienten.</span><span class="sxs-lookup"><span data-stu-id="e8c04-143">Once hello stored procedure is registered, we can execute it against hello collection, and read hello results back at hello client.</span></span> 

    // execute hello stored procedure
    client.executeStoredProcedureAsync('dbs/testdb/colls/testColl/sprocs/helloWorld')
        .then(function (response) {
            console.log(response.result); // "Hello, World"
        }, function (err) {
            console.log("Error", error);
        });


<span data-ttu-id="e8c04-144">hello context-objektet ger åtkomst tooall åtgärder som kan utföras på Cosmos-databaslagring, samt komma åt toohello förfrågan och svar objekt.</span><span class="sxs-lookup"><span data-stu-id="e8c04-144">hello context object provides access tooall operations that can be performed on Cosmos DB storage, as well as access toohello request and response objects.</span></span> <span data-ttu-id="e8c04-145">Vi använde i det här fallet hello objektet tooset hello svarstexten hello-svar som skickades tillbaka toohello klienten.</span><span class="sxs-lookup"><span data-stu-id="e8c04-145">In this case, we used hello response object tooset hello body of hello response that was sent back toohello client.</span></span> <span data-ttu-id="e8c04-146">Mer information finns i toohello [server Azure Cosmos DB JavaScript SDK-dokumentationen](http://azure.github.io/azure-documentdb-js-server/).</span><span class="sxs-lookup"><span data-stu-id="e8c04-146">For more details, refer toohello [Azure Cosmos DB JavaScript server SDK documentation](http://azure.github.io/azure-documentdb-js-server/).</span></span>  

<span data-ttu-id="e8c04-147">Låt oss expanderar på det här exemplet och lägga till flera databasen relaterade funktioner toohello lagrade proceduren.</span><span class="sxs-lookup"><span data-stu-id="e8c04-147">Let us expand on this example and add more database related functionality toohello stored procedure.</span></span> <span data-ttu-id="e8c04-148">Lagrade procedurer kan skapa, uppdatera, läsa, fråga och ta bort dokument och bifogade filer i hello samling.</span><span class="sxs-lookup"><span data-stu-id="e8c04-148">Stored procedures can create, update, read, query and delete documents and attachments inside hello collection.</span></span>    

### <a name="example-write-a-stored-procedure-toocreate-a-document"></a><span data-ttu-id="e8c04-149">Exempel: Skriv en lagrad procedur toocreate ett dokument</span><span class="sxs-lookup"><span data-stu-id="e8c04-149">Example: Write a stored procedure toocreate a document</span></span>
<span data-ttu-id="e8c04-150">hello nästa utdrag visar hur toouse hello kontexten objektet toointeract med Cosmos-DB-resurser.</span><span class="sxs-lookup"><span data-stu-id="e8c04-150">hello next snippet shows how toouse hello context object toointeract with Cosmos DB resources.</span></span>

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


<span data-ttu-id="e8c04-151">Den här lagrade proceduren tar inkommande documentToCreate, hello brödtexten i ett dokument toobe som skapats i hello samlingen.</span><span class="sxs-lookup"><span data-stu-id="e8c04-151">This stored procedure takes as input documentToCreate, hello body of a document toobe created in hello current collection.</span></span> <span data-ttu-id="e8c04-152">Dessa åtgärder är asynkron och beroende av JavaScript-funktionen återanrop.</span><span class="sxs-lookup"><span data-stu-id="e8c04-152">All such operations are asynchronous and depend on JavaScript function callbacks.</span></span> <span data-ttu-id="e8c04-153">hello Återanropsfunktionen har två parametrar, en för hello felobjektet om hello misslyckas och en för hello skapade objektet.</span><span class="sxs-lookup"><span data-stu-id="e8c04-153">hello callback function has two parameters, one for hello error object in case hello operation fails, and one for hello created object.</span></span> <span data-ttu-id="e8c04-154">Inuti hello återanrop kan användare hantera hello undantag eller ett fel genereras.</span><span class="sxs-lookup"><span data-stu-id="e8c04-154">Inside hello callback, users can either handle hello exception or throw an error.</span></span> <span data-ttu-id="e8c04-155">Om ett återanrop har angetts och det finns ett fel, genererar hello Azure Cosmos DB runtime ett fel.</span><span class="sxs-lookup"><span data-stu-id="e8c04-155">In case a callback is not provided and there is an error, hello Azure Cosmos DB runtime throws an error.</span></span>   

<span data-ttu-id="e8c04-156">I hello-exemplet ovan genererar hello återanrop ett fel om hello-åtgärden misslyckades.</span><span class="sxs-lookup"><span data-stu-id="e8c04-156">In hello example above, hello callback throws an error if hello operation failed.</span></span> <span data-ttu-id="e8c04-157">Annars anges hello-id för hello skapat dokument som hello brödtext hello svar toohello klienten.</span><span class="sxs-lookup"><span data-stu-id="e8c04-157">Otherwise, it sets hello id of hello created document as hello body of hello response toohello client.</span></span> <span data-ttu-id="e8c04-158">Här är hur den här lagrade proceduren körs med indataparametrar.</span><span class="sxs-lookup"><span data-stu-id="e8c04-158">Here is how this stored procedure is executed with input parameters.</span></span>

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


<span data-ttu-id="e8c04-159">Observera att detta lagrade proceduren kan ändrade tootake en matris med dokumentet organ som indata och skapar dem i samma lagras hello procedurkörningen istället för flera nätverk begär toocreate av dem individuellt.</span><span class="sxs-lookup"><span data-stu-id="e8c04-159">Note that this stored procedure can be modified tootake an array of document bodies as input and create them all in hello same stored procedure execution instead of multiple network requests toocreate each of them individually.</span></span> <span data-ttu-id="e8c04-160">Detta kan vara används tooimplement en effektiv bulk-import för Cosmos-DB (beskrivs senare i den här självstudiekursen).</span><span class="sxs-lookup"><span data-stu-id="e8c04-160">This can be used tooimplement an efficient bulk importer for Cosmos DB (discussed later in this tutorial).</span></span>   

<span data-ttu-id="e8c04-161">hello exempel beskrivs visas hur toouse lagrade procedurer.</span><span class="sxs-lookup"><span data-stu-id="e8c04-161">hello example described demonstrated how toouse stored procedures.</span></span> <span data-ttu-id="e8c04-162">Senare i självstudiekursen hello tar vi upp utlösare och användardefinierade funktioner (UDF).</span><span class="sxs-lookup"><span data-stu-id="e8c04-162">We will cover triggers and user defined functions (UDFs) later in hello tutorial.</span></span>

## <a name="database-program-transactions"></a><span data-ttu-id="e8c04-163">Programmet databastransaktioner</span><span class="sxs-lookup"><span data-stu-id="e8c04-163">Database program transactions</span></span>
<span data-ttu-id="e8c04-164">Transaktion i en typisk databas kan definieras som en sekvens med åtgärder som utförs som en logisk enhet på arbetet.</span><span class="sxs-lookup"><span data-stu-id="e8c04-164">Transaction in a typical database can be defined as a sequence of operations performed as a single logical unit of work.</span></span> <span data-ttu-id="e8c04-165">Varje transaktion innehåller **ACID garantier**.</span><span class="sxs-lookup"><span data-stu-id="e8c04-165">Each transaction provides **ACID guarantees**.</span></span> <span data-ttu-id="e8c04-166">AV är en välkänd förkortning som står för fyra egenskaper - odelbarhet, konsekvens, isolering och hållbarhet.</span><span class="sxs-lookup"><span data-stu-id="e8c04-166">ACID is a well-known acronym that stands for four properties -  Atomicity, Consistency, Isolation and Durability.</span></span>  

<span data-ttu-id="e8c04-167">En kort, odelbarhet garanterar att alla hello arbete som utförs i en transaktion behandlas som en enhet där antingen alla strävar eller none.</span><span class="sxs-lookup"><span data-stu-id="e8c04-167">Briefly, atomicity guarantees that all hello work done inside a transaction is treated as a single unit where either all of it is committed or none.</span></span> <span data-ttu-id="e8c04-168">Konsekvenskontroll ser till att hello data alltid är i ett internt tillstånd över transaktioner.</span><span class="sxs-lookup"><span data-stu-id="e8c04-168">Consistency makes sure that hello data is always in a good internal state across transactions.</span></span> <span data-ttu-id="e8c04-169">Isolering garanterar att inga två transaktioner störa varandra – Allmänt, de flesta kommersiella system ger flera isoleringsnivåer som kan användas baserat på hello programbehov.</span><span class="sxs-lookup"><span data-stu-id="e8c04-169">Isolation guarantees that no two transactions interfere with each other – generally, most commercial systems provide multiple isolation levels that can be used based on hello application needs.</span></span> <span data-ttu-id="e8c04-170">Hållbarhet säkerställer att alla ändringar genomförs i hello databasen alltid är tillgänglig.</span><span class="sxs-lookup"><span data-stu-id="e8c04-170">Durability ensures that any change that’s committed in hello database will always be present.</span></span>   

<span data-ttu-id="e8c04-171">I Cosmos DB JavaScript finns i hello samma minnesutrymme som hello-databas.</span><span class="sxs-lookup"><span data-stu-id="e8c04-171">In Cosmos DB, JavaScript is hosted in hello same memory space as hello database.</span></span> <span data-ttu-id="e8c04-172">Därför begäranden som görs i lagrade procedurer och utlösare köra i hello samma en session i databasen.</span><span class="sxs-lookup"><span data-stu-id="e8c04-172">Hence, requests made within stored procedures and triggers execute in hello same scope of a database session.</span></span> <span data-ttu-id="e8c04-173">Detta gör att Cosmos DB tooguarantee av för alla åtgärder som ingår i en enda lagrade proceduren/utlösare.</span><span class="sxs-lookup"><span data-stu-id="e8c04-173">This enables Cosmos DB tooguarantee ACID for all operations that are part of a single stored procedure/trigger.</span></span> <span data-ttu-id="e8c04-174">Tänk hello följande lagrade Procedurdefinition:</span><span class="sxs-lookup"><span data-stu-id="e8c04-174">Consider hello following stored procedure definition:</span></span>

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

<span data-ttu-id="e8c04-175">Den här lagrade proceduren använder transaktioner inom spel app tootrade objekt mellan två spelare i en enda åtgärd.</span><span class="sxs-lookup"><span data-stu-id="e8c04-175">This stored procedure uses transactions within a gaming app tootrade items between two players in a single operation.</span></span> <span data-ttu-id="e8c04-176">hello lagrade proceduren försök tooread två dokument varje motsvarande toohello spelare ID skickades som ett argument.</span><span class="sxs-lookup"><span data-stu-id="e8c04-176">hello stored procedure attempts tooread two documents each corresponding toohello player IDs passed in as an argument.</span></span> <span data-ttu-id="e8c04-177">Om båda player-dokumenten finns uppdaterar hello lagrade proceduren hello dokument genom att byta sina objekt.</span><span class="sxs-lookup"><span data-stu-id="e8c04-177">If both player documents are found, then hello stored procedure updates hello documents by swapping their items.</span></span> <span data-ttu-id="e8c04-178">Om fel uppstår hello vägen utlöser ett JavaScript-undantag som implicit avbryter hello transaktion.</span><span class="sxs-lookup"><span data-stu-id="e8c04-178">If any errors are encountered along hello way, it throws a JavaScript exception that implicitly aborts hello transaction.</span></span>

<span data-ttu-id="e8c04-179">Om hello samling hello lagrade proceduren har registrerats mot är en enskild partition samling och sedan hello transaktion är begränsade tooall hello dokument inom hello samlingen.</span><span class="sxs-lookup"><span data-stu-id="e8c04-179">If hello collection hello stored procedure is registered against is a single-partition collection, then hello transaction is scoped tooall hello documents within hello collection.</span></span> <span data-ttu-id="e8c04-180">Om hello samlingen är partitionerad utförs lagrade procedurer i hello transaktionsomfånget för en enskild partitionsnyckel.</span><span class="sxs-lookup"><span data-stu-id="e8c04-180">If hello collection is partitioned, then stored procedures are executed in hello transaction scope of a single partition key.</span></span> <span data-ttu-id="e8c04-181">Varje lagrade proceduren körning innehålla ett värde för partitionen som motsvarande toohello omfång hello transaction måste köras under.</span><span class="sxs-lookup"><span data-stu-id="e8c04-181">Each stored procedure execution must then include a partition key value corresponding toohello scope hello transaction must run under.</span></span> <span data-ttu-id="e8c04-182">Mer information finns i [Azure Cosmos DB partitionering](partition-data.md).</span><span class="sxs-lookup"><span data-stu-id="e8c04-182">For more details, see [Azure Cosmos DB Partitioning](partition-data.md).</span></span>

### <a name="commit-and-rollback"></a><span data-ttu-id="e8c04-183">Commit och rollback</span><span class="sxs-lookup"><span data-stu-id="e8c04-183">Commit and rollback</span></span>
<span data-ttu-id="e8c04-184">Transaktioner är djupt och internt integrerade i Cosmos DB JavaScript-programmeringsmodell.</span><span class="sxs-lookup"><span data-stu-id="e8c04-184">Transactions are deeply and natively integrated into Cosmos DB’s JavaScript programming model.</span></span> <span data-ttu-id="e8c04-185">I JavaScript-funktionen radbryts automatiskt alla åtgärder under en enda transaktion.</span><span class="sxs-lookup"><span data-stu-id="e8c04-185">Inside a JavaScript function, all operations are automatically wrapped under a single transaction.</span></span> <span data-ttu-id="e8c04-186">Om hello JavaScript har slutförts utan några undantag hello driftdatabasen toohello genomförs.</span><span class="sxs-lookup"><span data-stu-id="e8c04-186">If hello JavaScript completes without any exception, hello operations toohello database are committed.</span></span> <span data-ttu-id="e8c04-187">I praktiken är hello ”BEGIN TRANSACTION” och ”COMMIT TRANSACTION” satser i relationsdatabaser implicit i Cosmos-databasen.</span><span class="sxs-lookup"><span data-stu-id="e8c04-187">In effect, hello “BEGIN TRANSACTION” and “COMMIT TRANSACTION” statements in relational databases are implicit in Cosmos DB.</span></span>  

<span data-ttu-id="e8c04-188">Om alla undantag sprids från hello skript återställs Cosmos DB JavaScript-körning hello hela transaktionen.</span><span class="sxs-lookup"><span data-stu-id="e8c04-188">If there is any exception that’s propagated from hello script, Cosmos DB’s JavaScript runtime will roll back hello whole transaction.</span></span> <span data-ttu-id="e8c04-189">Enligt hello tidigare är exempelvis ett undantagsfel utlöses effektivt motsvarande tooa ”ROLLBACK TRANSACTION” i Cosmos-databasen.</span><span class="sxs-lookup"><span data-stu-id="e8c04-189">As shown in hello earlier example, throwing an exception is effectively equivalent tooa “ROLLBACK TRANSACTION” in Cosmos DB.</span></span>

### <a name="data-consistency"></a><span data-ttu-id="e8c04-190">Datakonsekvens</span><span class="sxs-lookup"><span data-stu-id="e8c04-190">Data consistency</span></span>
<span data-ttu-id="e8c04-191">Lagrade procedurer och utlösare körs alltid på hello primära replik hello Azure Cosmos DB behållare.</span><span class="sxs-lookup"><span data-stu-id="e8c04-191">Stored procedures and triggers are always executed on hello primary replica of hello Azure Cosmos DB container.</span></span> <span data-ttu-id="e8c04-192">Detta säkerställer att läsningar av inuti lagrade procedurer erbjudande stark konsekvens.</span><span class="sxs-lookup"><span data-stu-id="e8c04-192">This ensures that reads from inside stored procedures offer strong consistency.</span></span> <span data-ttu-id="e8c04-193">Frågor med användardefinierade funktioner kan köras på hello primära eller en sekundär replik, men vi Kontrollera toomeet hello begärt konsekvensnivå genom att välja hello lämplig replica.</span><span class="sxs-lookup"><span data-stu-id="e8c04-193">Queries using user defined functions can be executed on hello primary or any secondary replica, but we ensure toomeet hello requested consistency level by choosing hello appropriate replica.</span></span>

## <a name="bounded-execution"></a><span data-ttu-id="e8c04-194">Begränsad körning</span><span class="sxs-lookup"><span data-stu-id="e8c04-194">Bounded execution</span></span>
<span data-ttu-id="e8c04-195">Alla Cosmos-DB-åtgärder måste slutföra inom angivna hello servern begär timeout-varaktighet.</span><span class="sxs-lookup"><span data-stu-id="e8c04-195">All Cosmos DB operations must complete within hello server specified request timeout duration.</span></span> <span data-ttu-id="e8c04-196">Den här begränsningen gäller även tooJavaScript funktioner (lagrade procedurer, utlösare och användardefinierade funktioner).</span><span class="sxs-lookup"><span data-stu-id="e8c04-196">This constraint also applies tooJavaScript functions (stored procedures, triggers and user-defined functions).</span></span> <span data-ttu-id="e8c04-197">Om en åtgärd inte slutförs med tidsgränsen återställs hello transaktionen.</span><span class="sxs-lookup"><span data-stu-id="e8c04-197">If an operation does not complete with that time limit, hello transaction is rolled back.</span></span> <span data-ttu-id="e8c04-198">JavaScript-funktioner måste slutföras inom tidsgränsen för hello eller implementera en fortsättning baserat modellen toobatch/återuppta körning.</span><span class="sxs-lookup"><span data-stu-id="e8c04-198">JavaScript functions must finish within hello time limit or implement a continuation based model toobatch/resume execution.</span></span>  

<span data-ttu-id="e8c04-199">I ordning toosimplify utvecklingen av lagrade procedurer och utlösare toohandle tidsfrister, alla funktioner under hello samlingsobjektet (för att skapa, läsa, Ersätt och borttagning av dokument och bifogade filer) returnerar ett booleskt värde som representerar om som åtgärden kommer att slutföras.</span><span class="sxs-lookup"><span data-stu-id="e8c04-199">In order toosimplify development of stored procedures and triggers toohandle time limits, all functions under hello collection object (for create, read, replace, and delete of documents and attachments) return a Boolean value that represents whether that operation will complete.</span></span> <span data-ttu-id="e8c04-200">Om det här värdet är FALSKT är uppgift att hello tidsgränsen är om tooexpire och hello installationen skriva in körningen.</span><span class="sxs-lookup"><span data-stu-id="e8c04-200">If this value is false, it is an indication that hello time limit is about tooexpire and that hello procedure must wrap up execution.</span></span>  <span data-ttu-id="e8c04-201">Åtgärder i kö tidigare toohello första typen Lagringsåtgärden är garanterat toocomplete om hello lagrade proceduren har slutförts i tid och inte kö inga fler begäranden.</span><span class="sxs-lookup"><span data-stu-id="e8c04-201">Operations queued prior toohello first unaccepted store operation are guaranteed toocomplete if hello stored procedure completes in time and does not queue any more requests.</span></span>  

<span data-ttu-id="e8c04-202">JavaScript-funktioner är också avgränsas i resursförbrukning.</span><span class="sxs-lookup"><span data-stu-id="e8c04-202">JavaScript functions are also bounded on resource consumption.</span></span> <span data-ttu-id="e8c04-203">Cosmos DB reserverar genomströmning per samling baserat på hello etablerats storleken på ett databaskonto.</span><span class="sxs-lookup"><span data-stu-id="e8c04-203">Cosmos DB reserves throughput per collection based on hello provisioned size of a database account.</span></span> <span data-ttu-id="e8c04-204">Genomströmning uttrycks som ett normaliserat enhet av CPU, minne och i/o-förbrukning kallas frågeenheter eller RUs.</span><span class="sxs-lookup"><span data-stu-id="e8c04-204">Throughput is expressed in terms of a normalized unit of CPU, memory and IO consumption called request units or RUs.</span></span> <span data-ttu-id="e8c04-205">JavaScript-funktioner kan användaren använda ett stort antal RUs inom en kort tid och få hastighet begränsad om hello samling gränsen har nåtts.</span><span class="sxs-lookup"><span data-stu-id="e8c04-205">JavaScript functions can potentially use up a large number of RUs within a short time, and might get rate-limited if hello collection’s limit is reached.</span></span> <span data-ttu-id="e8c04-206">Resurs-intensiva lagrade procedurer kan också vara i karantän tooensure tillgängligheten för primitiva databasåtgärder.</span><span class="sxs-lookup"><span data-stu-id="e8c04-206">Resource intensive stored procedures might also be quarantined tooensure availability of primitive database operations.</span></span>  

### <a name="example-bulk-importing-data-into-a-database-program"></a><span data-ttu-id="e8c04-207">Exempel: Massredigera importera data till ett databasprogram</span><span class="sxs-lookup"><span data-stu-id="e8c04-207">Example: Bulk importing data into a database program</span></span>
<span data-ttu-id="e8c04-208">Nedan visas ett exempel på en lagrad procedur som har skrivits toobulk importera dokument till en samling.</span><span class="sxs-lookup"><span data-stu-id="e8c04-208">Below is an example of a stored procedure that is written toobulk-import documents into a collection.</span></span> <span data-ttu-id="e8c04-209">Observera hur hello lagras procedurkörningen handtag begränsas genom att kontrollera hello boolesk returvärde från createDocument och sedan använder hello antal dokument som infogas i varje anrop till hello lagrade proceduren tootrack och återuppta utvecklingen i batchar.</span><span class="sxs-lookup"><span data-stu-id="e8c04-209">Note how hello stored procedure handles bounded execution by checking hello Boolean return value from createDocument, and then uses hello count of documents inserted in each invocation of hello stored procedure tootrack and resume progress across batches.</span></span>

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

## <span data-ttu-id="e8c04-210"><a id="trigger"></a>Databasutlösare</span><span class="sxs-lookup"><span data-stu-id="e8c04-210"><a id="trigger"></a> Database triggers</span></span>
### <a name="database-pre-triggers"></a><span data-ttu-id="e8c04-211">Före databasutlösare</span><span class="sxs-lookup"><span data-stu-id="e8c04-211">Database pre-triggers</span></span>
<span data-ttu-id="e8c04-212">Cosmos DB innehåller utlösare som körs eller som utlöses av en åtgärd på ett dokument.</span><span class="sxs-lookup"><span data-stu-id="e8c04-212">Cosmos DB provides triggers that are executed or triggered by an operation on a document.</span></span> <span data-ttu-id="e8c04-213">Du kan till exempel ange en före utlösare när du skapar ett dokument – före utlösaren ska köras innan hello dokumentet skapas.</span><span class="sxs-lookup"><span data-stu-id="e8c04-213">For example, you can specify a pre-trigger when you are creating a document – this pre-trigger will run before hello document is created.</span></span> <span data-ttu-id="e8c04-214">hello följande är ett exempel på hur före utlösare kan vara används toovalidate hello egenskaperna för ett dokument som har skapats:</span><span class="sxs-lookup"><span data-stu-id="e8c04-214">hello following is an example of how pre-triggers can be used toovalidate hello properties of a document that is being created:</span></span>

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


<span data-ttu-id="e8c04-215">Och hello motsvarande registrering på klientsidan Node.js-kod för hello utlösare:</span><span class="sxs-lookup"><span data-stu-id="e8c04-215">And hello corresponding Node.js client-side registration code for hello trigger:</span></span>

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


<span data-ttu-id="e8c04-216">Före utlösare kan inte ha indataparametrar.</span><span class="sxs-lookup"><span data-stu-id="e8c04-216">Pre-triggers cannot have any input parameters.</span></span> <span data-ttu-id="e8c04-217">hello request-objektet kan vara används toomanipulate hello begärandemeddelandet som är associerat med hello åtgärden.</span><span class="sxs-lookup"><span data-stu-id="e8c04-217">hello request object can be used toomanipulate hello request message associated with hello operation.</span></span> <span data-ttu-id="e8c04-218">Här hello före utlösaren körs med hello skapandet av ett dokument och hello begäran meddelandetexten innehåller hello dokumentet toobe som skapats i JSON-format.</span><span class="sxs-lookup"><span data-stu-id="e8c04-218">Here, hello pre-trigger is being run with hello creation of a document, and hello request message body contains hello document toobe created in JSON format.</span></span>   

<span data-ttu-id="e8c04-219">När utlösare är registrerade ange användare hello-åtgärder som inte har.</span><span class="sxs-lookup"><span data-stu-id="e8c04-219">When triggers are registered, users can specify hello operations that it can run with.</span></span> <span data-ttu-id="e8c04-220">Den här utlösaren har skapats med TriggerOperation.Create, vilket innebär att hello följande inte tillåts.</span><span class="sxs-lookup"><span data-stu-id="e8c04-220">This trigger was created with TriggerOperation.Create, which means hello following is not permitted.</span></span>

    var options = { preTriggerInclude: "validateDocumentContents" };

    client.replaceDocumentAsync(docToReplace.self,
                  newDocBody, options)
    .then(function (response) {
        console.log(response.resource);
    }, function (error) {
        console.log("Error", error);
    });

    // Fails, can’t use a create trigger in a replace operation

### <a name="database-post-triggers"></a><span data-ttu-id="e8c04-221">Efter databasutlösare</span><span class="sxs-lookup"><span data-stu-id="e8c04-221">Database post-triggers</span></span>
<span data-ttu-id="e8c04-222">Efter utlösare som före utlösare associeras med en åtgärd på ett dokument och får inte indataparametrar.</span><span class="sxs-lookup"><span data-stu-id="e8c04-222">Post-triggers, like pre-triggers, are associated with an operation on a document and don’t take any input parameters.</span></span> <span data-ttu-id="e8c04-223">De körs **när** hello-åtgärden har slutförts och ha åtkomst toohello svarsmeddelande som skickas toohello klienten.</span><span class="sxs-lookup"><span data-stu-id="e8c04-223">They run **after** hello operation has completed, and have access toohello response message that is sent toohello client.</span></span>   

<span data-ttu-id="e8c04-224">hello som följande exempel visar efter utlösare i praktiken:</span><span class="sxs-lookup"><span data-stu-id="e8c04-224">hello following example shows post-triggers in action:</span></span>

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


<span data-ttu-id="e8c04-225">hello utlösare kan registreras som visas i följande exempel hello.</span><span class="sxs-lookup"><span data-stu-id="e8c04-225">hello trigger can be registered as shown in hello following sample.</span></span>

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


<span data-ttu-id="e8c04-226">Den här utlösaren frågar för hello metadata dokument och uppdaterar med information om hello nyskapade dokumentet.</span><span class="sxs-lookup"><span data-stu-id="e8c04-226">This trigger queries for hello metadata document and updates it with details about hello newly created document.</span></span>  

<span data-ttu-id="e8c04-227">En sak som är viktigt toonote hello **transaktionella** körning av utlösare i Cosmos-databasen.</span><span class="sxs-lookup"><span data-stu-id="e8c04-227">One thing that is important toonote is hello **transactional** execution of triggers in Cosmos DB.</span></span> <span data-ttu-id="e8c04-228">Den här efter utlösaren körs som en del av hello samma transaktion som hello skapandet av hello originaldokumentet.</span><span class="sxs-lookup"><span data-stu-id="e8c04-228">This post-trigger runs as part of hello same transaction as hello creation of hello original document.</span></span> <span data-ttu-id="e8c04-229">Därför, om vi utlöser ett undantag från hello efter utlösare (t.ex. Om vi tooupdate hello Metadatadokumentet) hello hela transaktionen misslyckas och återställas.</span><span class="sxs-lookup"><span data-stu-id="e8c04-229">Therefore, if we throw an exception from hello post-trigger (say if we are unable tooupdate hello metadata document), hello whole transaction will fail and be rolled back.</span></span> <span data-ttu-id="e8c04-230">Inget dokument kommer att skapas och ett undantag ska returneras.</span><span class="sxs-lookup"><span data-stu-id="e8c04-230">No document will be created, and an exception will be returned.</span></span>  

## <span data-ttu-id="e8c04-231"><a id="udf"></a>Användardefinierade funktioner</span><span class="sxs-lookup"><span data-stu-id="e8c04-231"><a id="udf"></a>User-defined functions</span></span>
<span data-ttu-id="e8c04-232">Användardefinierade funktioner (UDF) är används tooextend hello DocumentDB API SQL query language grammatik och implementera anpassad affärslogik.</span><span class="sxs-lookup"><span data-stu-id="e8c04-232">User-defined functions (UDFs) are used tooextend hello DocumentDB API SQL query language grammar and implement custom business logic.</span></span> <span data-ttu-id="e8c04-233">De kan endast anropas från inuti frågor.</span><span class="sxs-lookup"><span data-stu-id="e8c04-233">They can only be called from inside queries.</span></span> <span data-ttu-id="e8c04-234">De har inte åtkomst toohello context-objektet och är avsedda toobe som används som endast beräkning JavaScript.</span><span class="sxs-lookup"><span data-stu-id="e8c04-234">They do not have access toohello context object and are meant toobe used as compute-only JavaScript.</span></span> <span data-ttu-id="e8c04-235">Därför kan du köra UDF: er på sekundära repliker för hello Cosmos-DB-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="e8c04-235">Therefore, UDFs can be run on secondary replicas of hello Cosmos DB service.</span></span>  

<span data-ttu-id="e8c04-236">hello följande exempel skapar en UDF toocalculate skatter baserat på priser för olika intäkter hakparenteser och använder sedan den i en fråga toofind alla personer som betald mer än 20 000 i skatter.</span><span class="sxs-lookup"><span data-stu-id="e8c04-236">hello following sample creates a UDF toocalculate income tax based on rates for various income brackets, and then uses it inside a query toofind all people who paid more than $20,000 in taxes.</span></span>

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


<span data-ttu-id="e8c04-237">hello UDF kan därefter användas i frågor som i följande exempel hello:</span><span class="sxs-lookup"><span data-stu-id="e8c04-237">hello UDF can subsequently be used in queries like in hello following sample:</span></span>

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

## <a name="javascript-language-integrated-query-api"></a><span data-ttu-id="e8c04-238">JavaScript språkintegrerade frågan API</span><span class="sxs-lookup"><span data-stu-id="e8c04-238">JavaScript language-integrated query API</span></span>
<span data-ttu-id="e8c04-239">Dessutom tooissuing frågor med DocumentDB SQL-grammatik, hello serversidan SDK kan du tooperform optimerade frågor med ett flytande JavaScript-gränssnitt utan kännedom om SQL.</span><span class="sxs-lookup"><span data-stu-id="e8c04-239">In addition tooissuing queries using DocumentDB’s SQL grammar, hello server-side SDK allows you tooperform optimized queries using a fluent JavaScript interface without any knowledge of SQL.</span></span> <span data-ttu-id="e8c04-240">hello JavaScript frågan API kan du tooprogrammatically bygga frågor genom att skicka predikat funktioner till chainable funktionen anropas med en syntax bekant tooECMAScript5's matris built-ins och populära JavaScript-bibliotek som lodash.</span><span class="sxs-lookup"><span data-stu-id="e8c04-240">hello JavaScript query API allows you tooprogrammatically build queries by passing predicate functions into chainable function calls, with a syntax familiar tooECMAScript5's Array built-ins and popular JavaScript libraries like lodash.</span></span> <span data-ttu-id="e8c04-241">Frågor tolkas av hello JavaScript-körning toobe köras effektivt med hjälp av Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="e8c04-241">Queries are parsed by hello JavaScript runtime toobe executed efficiently using Azure Cosmos DB’s indices.</span></span>

> [!NOTE]
> <span data-ttu-id="e8c04-242">`__`(double understreck) är ett alias för`getContext().getCollection()`.</span><span class="sxs-lookup"><span data-stu-id="e8c04-242">`__` (double-underscore) is an alias too`getContext().getCollection()`.</span></span>
> <br/>
> <span data-ttu-id="e8c04-243">Med andra ord kan du använda `__` eller `getContext().getCollection()` tooaccess hello JavaScript API för frågan.</span><span class="sxs-lookup"><span data-stu-id="e8c04-243">In other words, you can use `__` or `getContext().getCollection()` tooaccess hello JavaScript query API.</span></span>
> 
> 

<span data-ttu-id="e8c04-244">Funktioner som stöds är:</span><span class="sxs-lookup"><span data-stu-id="e8c04-244">Supported functions include:</span></span>

<ul>
<li><span data-ttu-id="e8c04-245">
<b>... chain(). värdet ([återanrop] [, alternativ])</b>
</span><span class="sxs-lookup"><span data-stu-id="e8c04-245">
<b>chain() ... .value([callback] [, options])</b>
</span></span><ul>
<li>
<span data-ttu-id="e8c04-246">Startar en länkad anrop som måste avslutas med value().</span><span class="sxs-lookup"><span data-stu-id="e8c04-246">Starts a chained call which must be terminated with value().</span></span>
</li>
</ul>
</li>
<li><span data-ttu-id="e8c04-247">
<b>filter (predicateFunction [, alternativ] [, motringning])</b>
</span><span class="sxs-lookup"><span data-stu-id="e8c04-247">
<b>filter(predicateFunction [, options] [, callback])</b>
</span></span><ul>
<li>
<span data-ttu-id="e8c04-248">Filtrerar hello indatavärdet med en predikatfunktionens som returnerar SANT/FALSKT i ordning toofilter in/ut inkommande dokument i hello resultatmängden.</span><span class="sxs-lookup"><span data-stu-id="e8c04-248">Filters hello input using a predicate function which returns true/false in order toofilter in/out input documents into hello resulting set.</span></span> <span data-ttu-id="e8c04-249">Detta fungerar liknande tooa WHERE-satsen i SQL.</span><span class="sxs-lookup"><span data-stu-id="e8c04-249">This behaves similar tooa WHERE clause in SQL.</span></span>
</li>
</ul>
</li>
<li><span data-ttu-id="e8c04-250">
<b>karta (transformationFunction [, alternativ] [, motringning])</b>
</span><span class="sxs-lookup"><span data-stu-id="e8c04-250">
<b>map(transformationFunction [, options] [, callback])</b>
</span></span><ul>
<li>
<span data-ttu-id="e8c04-251">Gäller en projektion som anges en transformation-funktion som mappar varje inkommande objektet tooa JavaScript-objekt eller -värde.</span><span class="sxs-lookup"><span data-stu-id="e8c04-251">Applies a projection given a transformation function which maps each input item tooa JavaScript object or value.</span></span> <span data-ttu-id="e8c04-252">Detta fungerar liknande tooa villkoret SELECT i SQL.</span><span class="sxs-lookup"><span data-stu-id="e8c04-252">This behaves similar tooa SELECT clause in SQL.</span></span>
</li>
</ul>
</li>
<li><span data-ttu-id="e8c04-253">
<b>pluck ([egenskapsnamn] [, alternativ] [, motringning])</b>
</span><span class="sxs-lookup"><span data-stu-id="e8c04-253">
<b>pluck([propertyName] [, options] [, callback])</b>
</span></span><ul>
<li>
<span data-ttu-id="e8c04-254">Det här är en genväg till en karta som extraherar hello-värdet för en enda egenskap från varje inkommande objekt.</span><span class="sxs-lookup"><span data-stu-id="e8c04-254">This is a shortcut for a map which extracts hello value of a single property from each input item.</span></span>
</li>
</ul>
</li>
<li><span data-ttu-id="e8c04-255">
<b>förenkla ([isShallow] [, alternativ] [, motringning])</b>
</span><span class="sxs-lookup"><span data-stu-id="e8c04-255">
<b>flatten([isShallow] [, options] [, callback])</b>
</span></span><ul>
<li>
<span data-ttu-id="e8c04-256">Kombinerar och förenklas matriser från varje inkommande objekt i tooa enda matris.</span><span class="sxs-lookup"><span data-stu-id="e8c04-256">Combines and flattens arrays from each input item in tooa single array.</span></span> <span data-ttu-id="e8c04-257">Detta fungerar liknande tooSelectMany i LINQ.</span><span class="sxs-lookup"><span data-stu-id="e8c04-257">This behaves similar tooSelectMany in LINQ.</span></span>
</li>
</ul>
</li>
<li><span data-ttu-id="e8c04-258">
<b>sortBy ([predicate] [, alternativ] [, motringning])</b>
</span><span class="sxs-lookup"><span data-stu-id="e8c04-258">
<b>sortBy([predicate] [, options] [, callback])</b>
</span></span><ul>
<li>
<span data-ttu-id="e8c04-259">Skapa en ny uppsättning dokument genom att sortera hello dokument i hello Indatadokumentet dataström i stigande ordning med hjälp av hello angivna predikat.</span><span class="sxs-lookup"><span data-stu-id="e8c04-259">Produce a new set of documents by sorting hello documents in hello input document stream in ascending order using hello given predicate.</span></span> <span data-ttu-id="e8c04-260">Detta fungerar liknande tooa ORDER BY-satsen i SQL.</span><span class="sxs-lookup"><span data-stu-id="e8c04-260">This behaves similar tooa ORDER BY clause in SQL.</span></span>
</li>
</ul>
</li>
<li><span data-ttu-id="e8c04-261">
<b>sortByDescending ([predicate] [, alternativ] [, motringning])</b>
</span><span class="sxs-lookup"><span data-stu-id="e8c04-261">
<b>sortByDescending([predicate] [, options] [, callback])</b>
</span></span><ul>
<li>
<span data-ttu-id="e8c04-262">Skapa en ny uppsättning dokument genom att sortera hello dokument i hello Indatadokumentet dataström i fallande ordning med hjälp av hello angivna predikat.</span><span class="sxs-lookup"><span data-stu-id="e8c04-262">Produce a new set of documents by sorting hello documents in hello input document stream in descending order using hello given predicate.</span></span> <span data-ttu-id="e8c04-263">Detta fungerar liknande tooa x DESC ORDER BY-satsen i SQL.</span><span class="sxs-lookup"><span data-stu-id="e8c04-263">This behaves similar tooa ORDER BY x DESC clause in SQL.</span></span>
</li>
</ul>
</li>
</ul>


<span data-ttu-id="e8c04-264">När ingår i predikatet och/eller selector funktioner få hello följande JavaScript-konstruktioner automatiskt optimerade toorun direkt på Azure Cosmos DB index:</span><span class="sxs-lookup"><span data-stu-id="e8c04-264">When included inside predicate and/or selector functions, hello following JavaScript constructs get automatically optimized toorun directly on Azure Cosmos DB indices:</span></span>

* <span data-ttu-id="e8c04-265">Enkel operatörer: = + - * / % | ^ &amp; == != === !=== &lt; &gt; &lt;= &gt;= || &amp;&amp; &lt;&lt; &gt;&gt; &gt;&gt;&gt;!</span><span class="sxs-lookup"><span data-stu-id="e8c04-265">Simple operators: = + - * / % | ^ &amp; == != === !=== &lt; &gt; &lt;= &gt;= || &amp;&amp; &lt;&lt; &gt;&gt; &gt;&gt;&gt;!</span></span> ~
* <span data-ttu-id="e8c04-266">Literaler, inklusive hello objektet literal: {}</span><span class="sxs-lookup"><span data-stu-id="e8c04-266">Literals, including hello object literal: {}</span></span>
* <span data-ttu-id="e8c04-267">varians returnerade</span><span class="sxs-lookup"><span data-stu-id="e8c04-267">var, return</span></span>

<span data-ttu-id="e8c04-268">inte hämta optimerad hello följande JavaScript-konstruktioner för Azure Cosmos DB index:</span><span class="sxs-lookup"><span data-stu-id="e8c04-268">hello following JavaScript constructs do not get optimized for Azure Cosmos DB indices:</span></span>

* <span data-ttu-id="e8c04-269">Åtkomstkontrollflödet (t.ex. om, medan)</span><span class="sxs-lookup"><span data-stu-id="e8c04-269">Control flow (e.g. if, for, while)</span></span>
* <span data-ttu-id="e8c04-270">Funktionsanrop</span><span class="sxs-lookup"><span data-stu-id="e8c04-270">Function calls</span></span>

<span data-ttu-id="e8c04-271">Mer information, se vår [serversidan JSDocs](http://azure.github.io/azure-documentdb-js-server/).</span><span class="sxs-lookup"><span data-stu-id="e8c04-271">For more information, please see our [Server-Side JSDocs](http://azure.github.io/azure-documentdb-js-server/).</span></span>

### <a name="example-write-a-stored-procedure-using-hello-javascript-query-api"></a><span data-ttu-id="e8c04-272">Exempel: Skriv en lagrad procedur med hello JavaScript fråge-API</span><span class="sxs-lookup"><span data-stu-id="e8c04-272">Example: Write a stored procedure using hello JavaScript query API</span></span>
<span data-ttu-id="e8c04-273">hello följande kodexempel är ett exempel på hur hello JavaScript frågan API kan användas i hello kontexten för en lagrad procedur.</span><span class="sxs-lookup"><span data-stu-id="e8c04-273">hello following code sample is an example of how hello JavaScript Query API can be used in hello context of a stored procedure.</span></span> <span data-ttu-id="e8c04-274">hello lagrade proceduren infogar ett dokument som anges av en indataparameter och uppdaterar ett metadata-dokument med hello `__.filter()` metod med minstorlek maxSize och totalSize baserat på hello Indatadokumentet egenskapen.</span><span class="sxs-lookup"><span data-stu-id="e8c04-274">hello stored procedure inserts a document, given by an input parameter, and updates a metadata document, using hello `__.filter()` method, with minSize, maxSize, and totalSize based upon hello input document's size property.</span></span>

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

## <a name="sql-toojavascript-cheat-sheet"></a><span data-ttu-id="e8c04-275">SQL tooJavascript fusklapp</span><span class="sxs-lookup"><span data-stu-id="e8c04-275">SQL tooJavascript cheat sheet</span></span>
<span data-ttu-id="e8c04-276">hello följande tabell innehåller olika SQL-frågor och hello motsvarande JavaScript-frågor.</span><span class="sxs-lookup"><span data-stu-id="e8c04-276">hello following table presents various SQL queries and hello corresponding JavaScript queries.</span></span>

<span data-ttu-id="e8c04-277">Eftersom dokumentet egenskapen nycklar med SQL-frågor (t.ex. `doc.id`) är skiftlägeskänsliga.</span><span class="sxs-lookup"><span data-stu-id="e8c04-277">As with SQL queries, document property keys (e.g. `doc.id`) are case-sensitive.</span></span>

|<span data-ttu-id="e8c04-278">SQL</span><span class="sxs-lookup"><span data-stu-id="e8c04-278">SQL</span></span>| <span data-ttu-id="e8c04-279">JavaScript-fråga API</span><span class="sxs-lookup"><span data-stu-id="e8c04-279">JavaScript Query API</span></span>|<span data-ttu-id="e8c04-280">Beskrivningen nedan</span><span class="sxs-lookup"><span data-stu-id="e8c04-280">Description below</span></span>|
|---|---|---|
|<span data-ttu-id="e8c04-281">VÄLJ *</span><span class="sxs-lookup"><span data-stu-id="e8c04-281">SELECT *</span></span><br><span data-ttu-id="e8c04-282">FRÅN dokument</span><span class="sxs-lookup"><span data-stu-id="e8c04-282">FROM docs</span></span>| <span data-ttu-id="e8c04-283">__.Map(Function(doc) {</span><span class="sxs-lookup"><span data-stu-id="e8c04-283">__.map(function(doc) {</span></span> <br><span data-ttu-id="e8c04-284">&nbsp;&nbsp;&nbsp;&nbsp;returnera doc;</span><span class="sxs-lookup"><span data-stu-id="e8c04-284">&nbsp;&nbsp;&nbsp;&nbsp;return doc;</span></span><br><span data-ttu-id="e8c04-285">});</span><span class="sxs-lookup"><span data-stu-id="e8c04-285">});</span></span>|<span data-ttu-id="e8c04-286">1</span><span class="sxs-lookup"><span data-stu-id="e8c04-286">1</span></span>|
|<span data-ttu-id="e8c04-287">Välj docs.id, docs.message som ignorerad, docs.actions</span><span class="sxs-lookup"><span data-stu-id="e8c04-287">SELECT docs.id, docs.message AS msg, docs.actions</span></span> <br><span data-ttu-id="e8c04-288">FRÅN dokument</span><span class="sxs-lookup"><span data-stu-id="e8c04-288">FROM docs</span></span>|<span data-ttu-id="e8c04-289">__.Map(Function(doc) {</span><span class="sxs-lookup"><span data-stu-id="e8c04-289">__.map(function(doc) {</span></span><br><span data-ttu-id="e8c04-290">&nbsp;&nbsp;&nbsp;&nbsp;returnera {</span><span class="sxs-lookup"><span data-stu-id="e8c04-290">&nbsp;&nbsp;&nbsp;&nbsp;return {</span></span><br><span data-ttu-id="e8c04-291">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;ID: doc.id,</span><span class="sxs-lookup"><span data-stu-id="e8c04-291">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;id: doc.id,</span></span><br><span data-ttu-id="e8c04-292">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;meddelande: doc.message,</span><span class="sxs-lookup"><span data-stu-id="e8c04-292">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;msg: doc.message,</span></span><br><span data-ttu-id="e8c04-293">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Actions:doc.Actions</span><span class="sxs-lookup"><span data-stu-id="e8c04-293">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;actions:doc.actions</span></span><br><span data-ttu-id="e8c04-294">&nbsp;&nbsp;&nbsp;&nbsp;};</span><span class="sxs-lookup"><span data-stu-id="e8c04-294">&nbsp;&nbsp;&nbsp;&nbsp;};</span></span><br><span data-ttu-id="e8c04-295">});</span><span class="sxs-lookup"><span data-stu-id="e8c04-295">});</span></span>|<span data-ttu-id="e8c04-296">2</span><span class="sxs-lookup"><span data-stu-id="e8c04-296">2</span></span>|
|<span data-ttu-id="e8c04-297">VÄLJ *</span><span class="sxs-lookup"><span data-stu-id="e8c04-297">SELECT *</span></span><br><span data-ttu-id="e8c04-298">FRÅN dokument</span><span class="sxs-lookup"><span data-stu-id="e8c04-298">FROM docs</span></span><br><span data-ttu-id="e8c04-299">VAR docs.id="X998_Y998”</span><span class="sxs-lookup"><span data-stu-id="e8c04-299">WHERE docs.id="X998_Y998"</span></span>|<span data-ttu-id="e8c04-300">__.filter(Function(doc) {</span><span class="sxs-lookup"><span data-stu-id="e8c04-300">__.filter(function(doc) {</span></span><br><span data-ttu-id="e8c04-301">&nbsp;&nbsp;&nbsp;&nbsp;returnera doc.id === ”X998_Y998”;</span><span class="sxs-lookup"><span data-stu-id="e8c04-301">&nbsp;&nbsp;&nbsp;&nbsp;return doc.id ==="X998_Y998";</span></span><br><span data-ttu-id="e8c04-302">});</span><span class="sxs-lookup"><span data-stu-id="e8c04-302">});</span></span>|<span data-ttu-id="e8c04-303">3</span><span class="sxs-lookup"><span data-stu-id="e8c04-303">3</span></span>|
|<span data-ttu-id="e8c04-304">VÄLJ *</span><span class="sxs-lookup"><span data-stu-id="e8c04-304">SELECT *</span></span><br><span data-ttu-id="e8c04-305">FRÅN dokument</span><span class="sxs-lookup"><span data-stu-id="e8c04-305">FROM docs</span></span><br><span data-ttu-id="e8c04-306">VAR ARRAY_CONTAINS (dokument. Taggar 123)</span><span class="sxs-lookup"><span data-stu-id="e8c04-306">WHERE ARRAY_CONTAINS(docs.Tags, 123)</span></span>|<span data-ttu-id="e8c04-307">__.filter(Function(x) {</span><span class="sxs-lookup"><span data-stu-id="e8c04-307">__.filter(function(x) {</span></span><br><span data-ttu-id="e8c04-308">&nbsp;&nbsp;&nbsp;&nbsp;returnera x.Tags & & x.Tags.indexOf(123) > -1;</span><span class="sxs-lookup"><span data-stu-id="e8c04-308">&nbsp;&nbsp;&nbsp;&nbsp;return x.Tags && x.Tags.indexOf(123) > -1;</span></span><br><span data-ttu-id="e8c04-309">});</span><span class="sxs-lookup"><span data-stu-id="e8c04-309">});</span></span>|<span data-ttu-id="e8c04-310">4</span><span class="sxs-lookup"><span data-stu-id="e8c04-310">4</span></span>|
|<span data-ttu-id="e8c04-311">Välj docs.id, docs.message som ignorerad</span><span class="sxs-lookup"><span data-stu-id="e8c04-311">SELECT docs.id, docs.message AS msg</span></span><br><span data-ttu-id="e8c04-312">FRÅN dokument</span><span class="sxs-lookup"><span data-stu-id="e8c04-312">FROM docs</span></span><br><span data-ttu-id="e8c04-313">VAR docs.id="X998_Y998”</span><span class="sxs-lookup"><span data-stu-id="e8c04-313">WHERE docs.id="X998_Y998"</span></span>|<span data-ttu-id="e8c04-314">__.chain()</span><span class="sxs-lookup"><span data-stu-id="e8c04-314">__.chain()</span></span><br><span data-ttu-id="e8c04-315">&nbsp;&nbsp;&nbsp;&nbsp;.filter(Function(doc) {</span><span class="sxs-lookup"><span data-stu-id="e8c04-315">&nbsp;&nbsp;&nbsp;&nbsp;.filter(function(doc) {</span></span><br><span data-ttu-id="e8c04-316">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;returnera doc.id === ”X998_Y998”;</span><span class="sxs-lookup"><span data-stu-id="e8c04-316">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;return doc.id ==="X998_Y998";</span></span><br><span data-ttu-id="e8c04-317">&nbsp;&nbsp;&nbsp;&nbsp;})</span><span class="sxs-lookup"><span data-stu-id="e8c04-317">&nbsp;&nbsp;&nbsp;&nbsp;})</span></span><br><span data-ttu-id="e8c04-318">&nbsp;&nbsp;&nbsp;&nbsp;.Map(Function(doc) {</span><span class="sxs-lookup"><span data-stu-id="e8c04-318">&nbsp;&nbsp;&nbsp;&nbsp;.map(function(doc) {</span></span><br><span data-ttu-id="e8c04-319">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;returnera {</span><span class="sxs-lookup"><span data-stu-id="e8c04-319">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;return {</span></span><br><span data-ttu-id="e8c04-320">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;ID: doc.id,</span><span class="sxs-lookup"><span data-stu-id="e8c04-320">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;id: doc.id,</span></span><br><span data-ttu-id="e8c04-321">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;meddelande: doc.message</span><span class="sxs-lookup"><span data-stu-id="e8c04-321">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;msg: doc.message</span></span><br><span data-ttu-id="e8c04-322">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;};</span><span class="sxs-lookup"><span data-stu-id="e8c04-322">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;};</span></span><br><span data-ttu-id="e8c04-323">&nbsp;&nbsp;&nbsp;&nbsp;})</span><span class="sxs-lookup"><span data-stu-id="e8c04-323">&nbsp;&nbsp;&nbsp;&nbsp;})</span></span><br><span data-ttu-id="e8c04-324">.Value();</span><span class="sxs-lookup"><span data-stu-id="e8c04-324">.value();</span></span>|<span data-ttu-id="e8c04-325">5</span><span class="sxs-lookup"><span data-stu-id="e8c04-325">5</span></span>|
|<span data-ttu-id="e8c04-326">SELECT VALUE-tagg</span><span class="sxs-lookup"><span data-stu-id="e8c04-326">SELECT VALUE tag</span></span><br><span data-ttu-id="e8c04-327">FRÅN dokument</span><span class="sxs-lookup"><span data-stu-id="e8c04-327">FROM docs</span></span><br><span data-ttu-id="e8c04-328">Anslut tagg i dokumenten. Taggar</span><span class="sxs-lookup"><span data-stu-id="e8c04-328">JOIN tag IN docs.Tags</span></span><br><span data-ttu-id="e8c04-329">ORDER BY docs._ts</span><span class="sxs-lookup"><span data-stu-id="e8c04-329">ORDER BY docs._ts</span></span>|<span data-ttu-id="e8c04-330">__.chain()</span><span class="sxs-lookup"><span data-stu-id="e8c04-330">__.chain()</span></span><br><span data-ttu-id="e8c04-331">&nbsp;&nbsp;&nbsp;&nbsp;.filter(Function(doc) {</span><span class="sxs-lookup"><span data-stu-id="e8c04-331">&nbsp;&nbsp;&nbsp;&nbsp;.filter(function(doc) {</span></span><br><span data-ttu-id="e8c04-332">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;returnera dokument. Taggar & & Array.isArray (doc. Taggar).</span><span class="sxs-lookup"><span data-stu-id="e8c04-332">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;return doc.Tags && Array.isArray(doc.Tags);</span></span><br><span data-ttu-id="e8c04-333">&nbsp;&nbsp;&nbsp;&nbsp;})</span><span class="sxs-lookup"><span data-stu-id="e8c04-333">&nbsp;&nbsp;&nbsp;&nbsp;})</span></span><br><span data-ttu-id="e8c04-334">&nbsp;&nbsp;&nbsp;&nbsp;.sortBy(function(doc) {</span><span class="sxs-lookup"><span data-stu-id="e8c04-334">&nbsp;&nbsp;&nbsp;&nbsp;.sortBy(function(doc) {</span></span><br><span data-ttu-id="e8c04-335">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;returnera doc._ts;</span><span class="sxs-lookup"><span data-stu-id="e8c04-335">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;return doc._ts;</span></span><br><span data-ttu-id="e8c04-336">&nbsp;&nbsp;&nbsp;&nbsp;})</span><span class="sxs-lookup"><span data-stu-id="e8c04-336">&nbsp;&nbsp;&nbsp;&nbsp;})</span></span><br><span data-ttu-id="e8c04-337">&nbsp;&nbsp;&nbsp;&nbsp;.pluck("Tags")</span><span class="sxs-lookup"><span data-stu-id="e8c04-337">&nbsp;&nbsp;&nbsp;&nbsp;.pluck("Tags")</span></span><br><span data-ttu-id="e8c04-338">&nbsp;&nbsp;&nbsp;&nbsp;.flatten()</span><span class="sxs-lookup"><span data-stu-id="e8c04-338">&nbsp;&nbsp;&nbsp;&nbsp;.flatten()</span></span><br><span data-ttu-id="e8c04-339">&nbsp;&nbsp;&nbsp;&nbsp;.Value()</span><span class="sxs-lookup"><span data-stu-id="e8c04-339">&nbsp;&nbsp;&nbsp;&nbsp;.value()</span></span>|<span data-ttu-id="e8c04-340">6</span><span class="sxs-lookup"><span data-stu-id="e8c04-340">6</span></span>|

<span data-ttu-id="e8c04-341">hello förklarar följande beskrivningar varje fråga i hello tabellen ovan.</span><span class="sxs-lookup"><span data-stu-id="e8c04-341">hello following descriptions explain each query in hello table above.</span></span>
1. <span data-ttu-id="e8c04-342">Leder till att alla dokument (sidbrytning med fortsättningstoken) som är.</span><span class="sxs-lookup"><span data-stu-id="e8c04-342">Results in all documents (paginated with continuation token) as is.</span></span>
2. <span data-ttu-id="e8c04-343">Projekt hello-id, meddelande (ett alias toomsg) och åtgärd från alla dokument.</span><span class="sxs-lookup"><span data-stu-id="e8c04-343">Projects hello id, message (aliased toomsg), and action from all documents.</span></span>
3. <span data-ttu-id="e8c04-344">Frågor för dokument med hello predikat: id = ”X998_Y998”.</span><span class="sxs-lookup"><span data-stu-id="e8c04-344">Queries for documents with hello predicate: id = "X998_Y998".</span></span>
4. <span data-ttu-id="e8c04-345">Frågor för dokument som har en property-taggar och taggarna är en matris som innehåller hello värde 123.</span><span class="sxs-lookup"><span data-stu-id="e8c04-345">Queries for documents that have a Tags property and Tags is an array containing hello value 123.</span></span>
5. <span data-ttu-id="e8c04-346">Frågor för dokument med ett predikat, id = ”X998_Y998” och sedan projekt hello-id och meddelandet (ett alias toomsg).</span><span class="sxs-lookup"><span data-stu-id="e8c04-346">Queries for documents with a predicate, id = "X998_Y998", and then projects hello id and message (aliased toomsg).</span></span>
6. <span data-ttu-id="e8c04-347">Filter för dokument som har en matrisegenskap, taggar och sorterar hello resulterande dokument efter hello _ts system tidsstämpelsegenskapen, och sedan projekt + förenklas hello taggar matris.</span><span class="sxs-lookup"><span data-stu-id="e8c04-347">Filters for documents which have an array property, Tags, and sorts hello resulting documents by hello _ts timestamp system property, and then projects + flattens hello Tags array.</span></span>


## <a name="runtime-support"></a><span data-ttu-id="e8c04-348">Stöd för körning</span><span class="sxs-lookup"><span data-stu-id="e8c04-348">Runtime support</span></span>
<span data-ttu-id="e8c04-349">[DocumentDB JavaScript server side API](http://azure.github.io/azure-documentdb-js-server/) ger stöd för hello de flesta av hello vanlig funktioner för JavaScript-språk som standardiserade av [ECMA-262](http://www.ecma-international.org/publications/standards/Ecma-262.htm).</span><span class="sxs-lookup"><span data-stu-id="e8c04-349">[DocumentDB JavaScript server side API](http://azure.github.io/azure-documentdb-js-server/) provides support for hello most of hello mainstream JavaScript language features as standardized by [ECMA-262](http://www.ecma-international.org/publications/standards/Ecma-262.htm).</span></span>

### <a name="security"></a><span data-ttu-id="e8c04-350">Säkerhet</span><span class="sxs-lookup"><span data-stu-id="e8c04-350">Security</span></span>
<span data-ttu-id="e8c04-351">JavaScript-lagrade procedurer och utlösare är i begränsat läge så att hello effekterna av ett skript inte läcker toohello andra utan att gå via hello transaktion ögonblicksbildisolering på hello databasnivå.</span><span class="sxs-lookup"><span data-stu-id="e8c04-351">JavaScript stored procedures and triggers are sandboxed so that hello effects of one script do not leak toohello other without going through hello snapshot transaction isolation at hello database level.</span></span> <span data-ttu-id="e8c04-352">hello runtime miljöer pool men rengöras hello kontexten efter varje körning.</span><span class="sxs-lookup"><span data-stu-id="e8c04-352">hello runtime environments are pooled but cleaned of hello context after each run.</span></span> <span data-ttu-id="e8c04-353">Därför att de är garanterat toobe säker av alla oavsiktliga sidoeffekter från varandra.</span><span class="sxs-lookup"><span data-stu-id="e8c04-353">Hence they are guaranteed toobe safe of any unintended side effects from each other.</span></span>

### <a name="pre-compilation"></a><span data-ttu-id="e8c04-354">Före kompileringen</span><span class="sxs-lookup"><span data-stu-id="e8c04-354">Pre-compilation</span></span>
<span data-ttu-id="e8c04-355">Lagrade procedurer, utlösare och UDF: er är implicit förkompilerade toohello byte kodformat i ordning tooavoid kompilering kostnad vid hello tiden för varje skript-anrop.</span><span class="sxs-lookup"><span data-stu-id="e8c04-355">Stored procedures, triggers and UDFs are implicitly precompiled toohello byte code format in order tooavoid compilation cost at hello time of each script invocation.</span></span> <span data-ttu-id="e8c04-356">Detta säkerställer att anrop för lagrade procedurer är snabb och har en låg storleken.</span><span class="sxs-lookup"><span data-stu-id="e8c04-356">This ensures invocations of stored procedures are fast and have a low footprint.</span></span>

## <a name="client-sdk-support"></a><span data-ttu-id="e8c04-357">Stöd för klient-SDK</span><span class="sxs-lookup"><span data-stu-id="e8c04-357">Client SDK support</span></span>
<span data-ttu-id="e8c04-358">I tillägg toohello DocumentDB API för [Node.js](documentdb-sdk-node.md) Azure DB som Cosmos-klient har [.NET](documentdb-sdk-dotnet.md), [.NET Core](documentdb-sdk-dotnet-core.md), [Java](documentdb-sdk-java.md), [ JavaScript](http://azure.github.io/azure-documentdb-js/), och [Python SDK](documentdb-sdk-python.md) för hello DocumentDB-API.</span><span class="sxs-lookup"><span data-stu-id="e8c04-358">In addition toohello DocumentDB API for [Node.js](documentdb-sdk-node.md) client, Azure Cosmos DB has [.NET](documentdb-sdk-dotnet.md), [.NET Core](documentdb-sdk-dotnet-core.md), [Java](documentdb-sdk-java.md), [JavaScript](http://azure.github.io/azure-documentdb-js/), and [Python SDKs](documentdb-sdk-python.md) for hello DocumentDB API.</span></span> <span data-ttu-id="e8c04-359">Lagrade procedurer, utlösare och UDF: er kan skapas och köras med hjälp av dessa SDK: er samt.</span><span class="sxs-lookup"><span data-stu-id="e8c04-359">Stored procedures, triggers and UDFs can be created and executed using any of these SDKs as well.</span></span> <span data-ttu-id="e8c04-360">följande exempel visar hur hello toocreate och köra en lagrad procedur med hello .NET-klienten.</span><span class="sxs-lookup"><span data-stu-id="e8c04-360">hello following example shows how toocreate and execute a stored procedure using hello .NET client.</span></span> <span data-ttu-id="e8c04-361">Observera hur hello .NET-typer har skickats till hello lagrade procedur som JSON och läsa tillbaka.</span><span class="sxs-lookup"><span data-stu-id="e8c04-361">Note how hello .NET types are passed into hello stored procedure as JSON and read back.</span></span>

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


<span data-ttu-id="e8c04-362">Det här exemplet visas hur toouse hello [DocumentDB .NET API](/dotnet/api/overview/azure/cosmosdb?view=azure-dotnet) toocreate före utlösare och skapa ett dokument med hello utlösaren aktiveras.</span><span class="sxs-lookup"><span data-stu-id="e8c04-362">This sample shows how toouse hello [DocumentDB .NET API](/dotnet/api/overview/azure/cosmosdb?view=azure-dotnet) toocreate a pre-trigger and create a document with hello trigger enabled.</span></span> 

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


<span data-ttu-id="e8c04-363">Och hello som följande exempel visar hur toocreate användaren definierat funktion (UDF) och använda den i en [DocumentDB API SQL-frågan](documentdb-sql-query.md).</span><span class="sxs-lookup"><span data-stu-id="e8c04-363">And hello following example shows how toocreate a user defined function (UDF) and use it in a [DocumentDB API SQL query](documentdb-sql-query.md).</span></span>

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

## <a name="rest-api"></a><span data-ttu-id="e8c04-364">REST API</span><span class="sxs-lookup"><span data-stu-id="e8c04-364">REST API</span></span>
<span data-ttu-id="e8c04-365">Alla Azure DB som Cosmos-åtgärder kan utföras på ett RESTful sätt.</span><span class="sxs-lookup"><span data-stu-id="e8c04-365">All Azure Cosmos DB operations can be performed in a RESTful manner.</span></span> <span data-ttu-id="e8c04-366">Lagrade procedurer, utlösare och användardefinierade funktioner kan registreras i en samling med hjälp av HTTP POST.</span><span class="sxs-lookup"><span data-stu-id="e8c04-366">Stored procedures, triggers and user-defined functions can be registered under a collection by using HTTP POST.</span></span> <span data-ttu-id="e8c04-367">hello följande är ett exempel på hur tooregister en lagrad procedur:</span><span class="sxs-lookup"><span data-stu-id="e8c04-367">hello following is an example of how tooregister a stored procedure:</span></span>

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


<span data-ttu-id="e8c04-368">hello lagrade proceduren registreras genom att köra en POST-begäran mot hello URI dbs/testdb/colls/testColl/sprocs med hello brödtext som innehåller hello lagrade proceduren toocreate.</span><span class="sxs-lookup"><span data-stu-id="e8c04-368">hello stored procedure is registered by executing a POST request against hello URI dbs/testdb/colls/testColl/sprocs with hello body containing hello stored procedure toocreate.</span></span> <span data-ttu-id="e8c04-369">Utlösare och UDF: er kan registreras på samma sätt genom att utfärda ett INLÄGG mot /triggers och /udfs respektive.</span><span class="sxs-lookup"><span data-stu-id="e8c04-369">Triggers and UDFs can be registered similarly by issuing a POST against /triggers and /udfs respectively.</span></span>
<span data-ttu-id="e8c04-370">Det här lagrade proceduren kan sedan köras genom att utfärda en POST-begäran mot dess resurslänken:</span><span class="sxs-lookup"><span data-stu-id="e8c04-370">This stored procedure can then be executed by issuing a POST request against its resource link:</span></span>

    POST https://<url>/sprocs/<sproc> HTTP/1.1
    authorization: <<auth>>
    x-ms-date: Thu, 07 Aug 2014 03:43:20 GMT


    [ { "name": "TestDocument", "book": "Autumn of hello Patriarch"}, "Price", 200 ]


<span data-ttu-id="e8c04-371">Här kan skickas hello inkommande toohello lagrade proceduren hello frågans brödtext.</span><span class="sxs-lookup"><span data-stu-id="e8c04-371">Here, hello input toohello stored procedure is passed in hello request body.</span></span> <span data-ttu-id="e8c04-372">Observera att hello indata skickas som ett JSON-matris av indataparametrar.</span><span class="sxs-lookup"><span data-stu-id="e8c04-372">Note that hello input is passed as a JSON array of input parameters.</span></span> <span data-ttu-id="e8c04-373">hello lagrade proceduren tar hello första indata som ett dokument som är en brödtext för svar.</span><span class="sxs-lookup"><span data-stu-id="e8c04-373">hello stored procedure takes hello first input as a document that is a response body.</span></span> <span data-ttu-id="e8c04-374">hello-svar som vi får är följande:</span><span class="sxs-lookup"><span data-stu-id="e8c04-374">hello response we receive is as follows:</span></span>

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


<span data-ttu-id="e8c04-375">Utlösare, till skillnad från lagrade procedurer kan inte köras direkt.</span><span class="sxs-lookup"><span data-stu-id="e8c04-375">Triggers, unlike stored procedures, cannot be executed directly.</span></span> <span data-ttu-id="e8c04-376">I stället körs de som en del av en åtgärd på ett dokument.</span><span class="sxs-lookup"><span data-stu-id="e8c04-376">Instead they are executed as part of an operation on a document.</span></span> <span data-ttu-id="e8c04-377">Vi kan ange hello utlösare toorun med en förfrågan med en HTTP-huvuden.</span><span class="sxs-lookup"><span data-stu-id="e8c04-377">We can specify hello triggers toorun with a request using HTTP headers.</span></span> <span data-ttu-id="e8c04-378">hello följer begäran toocreate ett dokument.</span><span class="sxs-lookup"><span data-stu-id="e8c04-378">hello following is request toocreate a document.</span></span>

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


<span data-ttu-id="e8c04-379">Här anges hello före utlösaren toobe kör med hello begäran i hello x-ms-documentdb-pre-trigger-include huvudet.</span><span class="sxs-lookup"><span data-stu-id="e8c04-379">Here hello pre-trigger toobe run with hello request is specified in hello x-ms-documentdb-pre-trigger-include header.</span></span> <span data-ttu-id="e8c04-380">På motsvarande sätt anges efter utlösare i hello x-ms-documentdb-post-trigger-include huvud.</span><span class="sxs-lookup"><span data-stu-id="e8c04-380">Correspondingly, any post-triggers are given in hello x-ms-documentdb-post-trigger-include header.</span></span> <span data-ttu-id="e8c04-381">Observera att både före och efter utlösare kan anges för en viss begäran.</span><span class="sxs-lookup"><span data-stu-id="e8c04-381">Note that both pre- and post-triggers can be specified for a given request.</span></span>

## <a name="sample-code"></a><span data-ttu-id="e8c04-382">Exempelkod</span><span class="sxs-lookup"><span data-stu-id="e8c04-382">Sample code</span></span>
<span data-ttu-id="e8c04-383">Du kan hitta mer kodexempel för serversidan (inklusive [massborttagning](https://github.com/Azure/azure-documentdb-js-server/tree/master/samples/stored-procedures/bulkDelete.js), och [uppdatera](https://github.com/Azure/azure-documentdb-js-server/tree/master/samples/stored-procedures/update.js)) på vår [GitHub-lagringsplatsen](https://github.com/Azure/azure-documentdb-js-server/tree/master/samples).</span><span class="sxs-lookup"><span data-stu-id="e8c04-383">You can find more server-side code examples (including [bulk-delete](https://github.com/Azure/azure-documentdb-js-server/tree/master/samples/stored-procedures/bulkDelete.js), and [update](https://github.com/Azure/azure-documentdb-js-server/tree/master/samples/stored-procedures/update.js)) on our [GitHub repository](https://github.com/Azure/azure-documentdb-js-server/tree/master/samples).</span></span>

<span data-ttu-id="e8c04-384">Om du vill tooshare din helt otroligt lagrade proceduren?</span><span class="sxs-lookup"><span data-stu-id="e8c04-384">Want tooshare your awesome stored procedure?</span></span> <span data-ttu-id="e8c04-385">Skicka oss en pull-begäran!</span><span class="sxs-lookup"><span data-stu-id="e8c04-385">Please, send us a pull-request!</span></span> 

## <a name="next-steps"></a><span data-ttu-id="e8c04-386">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="e8c04-386">Next steps</span></span>
<span data-ttu-id="e8c04-387">När du har en eller flera lagrade procedurer, utlösare och användardefinierade funktioner som har skapats kan du läsa in dem och visa dem i hello Azure-portalen med Data Explorer.</span><span class="sxs-lookup"><span data-stu-id="e8c04-387">Once you have one or more stored procedures, triggers, and user-defined functions created, you can load them and view them in hello Azure portal using Data Explorer.</span></span>

<span data-ttu-id="e8c04-388">Du kan också hitta hello följande referenser och resurser som är användbara i din sökväg toolearn mer om Azure Cosmos dB serversidan programmering:</span><span class="sxs-lookup"><span data-stu-id="e8c04-388">You may also find hello following references and resources useful in your path toolearn more about Azure Cosmos dB server-side programming:</span></span>

* [<span data-ttu-id="e8c04-389">Azure Cosmos DB SDK</span><span class="sxs-lookup"><span data-stu-id="e8c04-389">Azure Cosmos DB SDKs</span></span>](documentdb-sdk-dotnet.md)
* [<span data-ttu-id="e8c04-390">DocumentDB-Studio</span><span class="sxs-lookup"><span data-stu-id="e8c04-390">DocumentDB Studio</span></span>](https://github.com/mingaliu/DocumentDBStudio/releases)
* [<span data-ttu-id="e8c04-391">JSON</span><span class="sxs-lookup"><span data-stu-id="e8c04-391">JSON</span></span>](http://www.json.org/) 
* [<span data-ttu-id="e8c04-392">JavaScript ECMA-262</span><span class="sxs-lookup"><span data-stu-id="e8c04-392">JavaScript ECMA-262</span></span>](http://www.ecma-international.org/publications/standards/Ecma-262.htm)
* [<span data-ttu-id="e8c04-393">Säker och bärbar databasen utökningsbarhet</span><span class="sxs-lookup"><span data-stu-id="e8c04-393">Secure and Portable Database Extensibility</span></span>](http://dl.acm.org/citation.cfm?id=276339) 
* [<span data-ttu-id="e8c04-394">Tjänsten inriktade Database-arkitektur</span><span class="sxs-lookup"><span data-stu-id="e8c04-394">Service Oriented Database Architecture</span></span>](http://dl.acm.org/citation.cfm?id=1066267&coll=Portal&dl=GUIDE) 
* [<span data-ttu-id="e8c04-395">Värd för hello .NET Runtime i Microsoft SQL server</span><span class="sxs-lookup"><span data-stu-id="e8c04-395">Hosting hello .NET Runtime in Microsoft SQL server</span></span>](http://dl.acm.org/citation.cfm?id=1007669)

