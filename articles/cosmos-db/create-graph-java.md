---
title: aaaCreate en databas med Azure Cosmos DB diagram med Java | Microsoft Docs
description: "Anger en Java code exempel som du kan använda tooconnect tooand frågan diagramdata i Azure Cosmos-databasen med Gremlin."
services: cosmos-db
documentationcenter: 
author: dennyglee
manager: jhubbard
editor: 
ms.assetid: daacbabf-1bb5-497f-92db-079910703046
ms.service: cosmos-db
ms.custom: quick start connect, mvc
ms.workload: 
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 08/24/2017
ms.author: denlee
ms.openlocfilehash: 595c0fb108f3dbe8c83674f0c9c4b0cdd3ab4c95
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-cosmos-db-create-a-graph-database-using-java-and-hello-azure-portal"></a><span data-ttu-id="1ee4b-103">Azure DB Cosmos: Skapa en graph-databas som använder Java och hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="1ee4b-103">Azure Cosmos DB: Create a graph database using Java and hello Azure portal</span></span>

<span data-ttu-id="1ee4b-104">Azure Cosmos DB är Microsofts globalt distribuerade databastjänst för flera datamodeller.</span><span class="sxs-lookup"><span data-stu-id="1ee4b-104">Azure Cosmos DB is Microsoft’s globally distributed multi-model database service.</span></span> <span data-ttu-id="1ee4b-105">Du kan snabbt skapa och fråga dokument och nyckel/värde-diagrammet databaser, som omfattas av hello global distributionsplatsen och skala horisontellt funktionerna i hello kärnan i Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="1ee4b-105">You can quickly create and query document, key/value, and graph databases, all of which benefit from hello global distribution and horizontal scale capabilities at hello core of Azure Cosmos DB.</span></span> 

<span data-ttu-id="1ee4b-106">Denna Snabbstart skapar ett diagram hello databas med hjälp av Azure portal-verktyg för Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="1ee4b-106">This quickstart creates a graph database using hello Azure portal tools for Azure Cosmos DB.</span></span> <span data-ttu-id="1ee4b-107">Den här snabbstarten visar också hur tooquickly skapar en Java-konsolapp med hjälp av en graph-databas med hello OSS [Gremlin Java](https://mvnrepository.com/artifact/org.apache.tinkerpop/gremlin-driver) drivrutin.</span><span class="sxs-lookup"><span data-stu-id="1ee4b-107">This quickstart also shows you how tooquickly create a Java console app using a graph database using hello OSS [Gremlin Java](https://mvnrepository.com/artifact/org.apache.tinkerpop/gremlin-driver) driver.</span></span> <span data-ttu-id="1ee4b-108">hello anvisningarna i den här snabbstarten kan tillämpas på alla operativsystem som kan köra Java.</span><span class="sxs-lookup"><span data-stu-id="1ee4b-108">hello instructions in this quickstart can be followed on any operating system that is capable of running Java.</span></span> <span data-ttu-id="1ee4b-109">Denna Snabbstart lär du dig att skapa och ändra diagram resurser i hello Användargränssnittet eller programmässigt, beroende på vilket som är dina inställningar.</span><span class="sxs-lookup"><span data-stu-id="1ee4b-109">This quickstart familiarizes you with creating and modifying graph resources in either hello UI or programmatically, whichever is your preference.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="1ee4b-110">Krav</span><span class="sxs-lookup"><span data-stu-id="1ee4b-110">Prerequisites</span></span>

* [<span data-ttu-id="1ee4b-111">Java Development Kit (JDK) 1.7+</span><span class="sxs-lookup"><span data-stu-id="1ee4b-111">Java Development Kit (JDK) 1.7+</span></span>](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)
    * <span data-ttu-id="1ee4b-112">Ubuntu, kör `apt-get install default-jdk` tooinstall hello JDK.</span><span class="sxs-lookup"><span data-stu-id="1ee4b-112">On Ubuntu, run `apt-get install default-jdk` tooinstall hello JDK.</span></span>
    * <span data-ttu-id="1ee4b-113">Vara säker på att tooset hello JAVA_HOME miljö variabeln toopoint toohello mapp där hello JDK är installerad.</span><span class="sxs-lookup"><span data-stu-id="1ee4b-113">Be sure tooset hello JAVA_HOME environment variable toopoint toohello folder where hello JDK is installed.</span></span>
* <span data-ttu-id="1ee4b-114">[Ladda ned](http://maven.apache.org/download.cgi) och [installera](http://maven.apache.org/install.html) ett [Maven](http://maven.apache.org/)-binärarkiv</span><span class="sxs-lookup"><span data-stu-id="1ee4b-114">[Download](http://maven.apache.org/download.cgi) and [install](http://maven.apache.org/install.html) a [Maven](http://maven.apache.org/) binary archive</span></span>
    * <span data-ttu-id="1ee4b-115">Ubuntu, du kan köra `apt-get install maven` tooinstall Maven.</span><span class="sxs-lookup"><span data-stu-id="1ee4b-115">On Ubuntu, you can run `apt-get install maven` tooinstall Maven.</span></span>
* [<span data-ttu-id="1ee4b-116">Git</span><span class="sxs-lookup"><span data-stu-id="1ee4b-116">Git</span></span>](https://www.git-scm.com/)
    * <span data-ttu-id="1ee4b-117">Ubuntu, du kan köra `sudo apt-get install git` tooinstall Git.</span><span class="sxs-lookup"><span data-stu-id="1ee4b-117">On Ubuntu, you can run `sudo apt-get install git` tooinstall Git.</span></span>

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-a-database-account"></a><span data-ttu-id="1ee4b-118">Skapa ett databaskonto</span><span class="sxs-lookup"><span data-stu-id="1ee4b-118">Create a database account</span></span>

<span data-ttu-id="1ee4b-119">Innan du kan skapa en graph-databas måste toocreate ett databaskonto i Gremlin (diagram) med Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="1ee4b-119">Before you can create a graph database, you need toocreate a Gremlin (Graph) database account with Azure Cosmos DB.</span></span>

[!INCLUDE [cosmos-db-create-dbaccount-graph](../../includes/cosmos-db-create-dbaccount-graph.md)]

## <a name="add-a-graph"></a><span data-ttu-id="1ee4b-120">Lägga till en graf</span><span class="sxs-lookup"><span data-stu-id="1ee4b-120">Add a graph</span></span>

<span data-ttu-id="1ee4b-121">Du kan nu använda hello Data Explorer verktyget i hello Azure portal toocreate en graph-databas.</span><span class="sxs-lookup"><span data-stu-id="1ee4b-121">You can now use hello Data Explorer tool in hello Azure portal toocreate a graph database.</span></span> 

1. <span data-ttu-id="1ee4b-122">Klicka på hello Azure-portalen i hello vänstra navigeringsmenyn **Data Explorer (förhandsgranskning)**.</span><span class="sxs-lookup"><span data-stu-id="1ee4b-122">In hello Azure portal, in hello left navigation menu, click **Data Explorer (Preview)**.</span></span> 
2. <span data-ttu-id="1ee4b-123">I hello **Data Explorer (förhandsgranskning)** bladet, klickar du på **nytt diagram**, fyll sedan i hello sida med hello följande information:</span><span class="sxs-lookup"><span data-stu-id="1ee4b-123">In hello **Data Explorer (Preview)** blade, click **New Graph**, then fill in hello page using hello following information:</span></span>

    ![Data Explorer i hello Azure-portalen](./media/create-graph-java/azure-cosmosdb-data-explorer.png)

    <span data-ttu-id="1ee4b-125">Inställning</span><span class="sxs-lookup"><span data-stu-id="1ee4b-125">Setting</span></span>|<span data-ttu-id="1ee4b-126">Föreslaget värde</span><span class="sxs-lookup"><span data-stu-id="1ee4b-126">Suggested value</span></span>|<span data-ttu-id="1ee4b-127">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="1ee4b-127">Description</span></span>
    ---|---|---
    <span data-ttu-id="1ee4b-128">Databas-id</span><span class="sxs-lookup"><span data-stu-id="1ee4b-128">Database ID</span></span>|<span data-ttu-id="1ee4b-129">sample-database</span><span class="sxs-lookup"><span data-stu-id="1ee4b-129">sample-database</span></span>|<span data-ttu-id="1ee4b-130">hello-ID för den nya databasen.</span><span class="sxs-lookup"><span data-stu-id="1ee4b-130">hello ID for your new database.</span></span> <span data-ttu-id="1ee4b-131">Databasnamn måste innehålla mellan 1 och 255 tecken och får inte innehålla `/ \ # ?` eller avslutande blanksteg.</span><span class="sxs-lookup"><span data-stu-id="1ee4b-131">Database names must be between 1 and 255 characters, and cannot contain `/ \ # ?` or a trailing space.</span></span>
    <span data-ttu-id="1ee4b-132">Graf-id</span><span class="sxs-lookup"><span data-stu-id="1ee4b-132">Graph ID</span></span>|<span data-ttu-id="1ee4b-133">sample-graph</span><span class="sxs-lookup"><span data-stu-id="1ee4b-133">sample-graph</span></span>|<span data-ttu-id="1ee4b-134">hello-ID för nya diagrammet.</span><span class="sxs-lookup"><span data-stu-id="1ee4b-134">hello ID for your new graph.</span></span> <span data-ttu-id="1ee4b-135">Diagrammet namn har hello samma tecken krav som databas-ID: n.</span><span class="sxs-lookup"><span data-stu-id="1ee4b-135">Graph names have hello same character requirements as database ids.</span></span>
    <span data-ttu-id="1ee4b-136">Lagringskapacitet</span><span class="sxs-lookup"><span data-stu-id="1ee4b-136">Storage Capacity</span></span>| <span data-ttu-id="1ee4b-137">10 GB</span><span class="sxs-lookup"><span data-stu-id="1ee4b-137">10 GB</span></span>|<span data-ttu-id="1ee4b-138">Lämna hello standardvärdet.</span><span class="sxs-lookup"><span data-stu-id="1ee4b-138">Leave hello default value.</span></span> <span data-ttu-id="1ee4b-139">Detta är hello lagringskapaciteten för hello-databasen.</span><span class="sxs-lookup"><span data-stu-id="1ee4b-139">This is hello storage capacity of hello database.</span></span>
    <span data-ttu-id="1ee4b-140">Dataflöde</span><span class="sxs-lookup"><span data-stu-id="1ee4b-140">Throughput</span></span>|<span data-ttu-id="1ee4b-141">400 RU:er</span><span class="sxs-lookup"><span data-stu-id="1ee4b-141">400 RUs</span></span>|<span data-ttu-id="1ee4b-142">Lämna hello standardvärdet.</span><span class="sxs-lookup"><span data-stu-id="1ee4b-142">Leave hello default value.</span></span> <span data-ttu-id="1ee4b-143">Du kan skala upp hello genomströmning senare om du vill tooreduce svarstid.</span><span class="sxs-lookup"><span data-stu-id="1ee4b-143">You can scale up hello throughput later if you want tooreduce latency.</span></span>
    <span data-ttu-id="1ee4b-144">Partitionsnyckeln</span><span class="sxs-lookup"><span data-stu-id="1ee4b-144">Partition key</span></span>|<span data-ttu-id="1ee4b-145">Lämna tomt</span><span class="sxs-lookup"><span data-stu-id="1ee4b-145">Leave blank</span></span>|<span data-ttu-id="1ee4b-146">För hello syftet med den här snabbstarten, inget hello partitionsnyckel.</span><span class="sxs-lookup"><span data-stu-id="1ee4b-146">For hello purpose of this quickstart, leave hello partition key blank.</span></span>

3. <span data-ttu-id="1ee4b-147">När hello formuläret, klickar du på **OK**.</span><span class="sxs-lookup"><span data-stu-id="1ee4b-147">Once hello form is filled out, click **OK**.</span></span>

## <a name="clone-hello-sample-application"></a><span data-ttu-id="1ee4b-148">Klona hello exempelprogrammet</span><span class="sxs-lookup"><span data-stu-id="1ee4b-148">Clone hello sample application</span></span>

<span data-ttu-id="1ee4b-149">Nu ska vi klona en graph-app från github, ange hello anslutningssträngen och kör den.</span><span class="sxs-lookup"><span data-stu-id="1ee4b-149">Now let's clone a graph app from github, set hello connection string, and run it.</span></span> <span data-ttu-id="1ee4b-150">Du ser hur enkelt är det toowork med data programmässigt.</span><span class="sxs-lookup"><span data-stu-id="1ee4b-150">You see how easy it is toowork with data programmatically.</span></span> 

1. <span data-ttu-id="1ee4b-151">Öppna ett git terminalfönster, till exempel git bash och `cd` tooa arbetskatalogen.</span><span class="sxs-lookup"><span data-stu-id="1ee4b-151">Open a git terminal window, such as git bash, and `cd` tooa working directory.</span></span>  

2. <span data-ttu-id="1ee4b-152">Hello kör följande kommando tooclone hello exempel lagringsplatsen.</span><span class="sxs-lookup"><span data-stu-id="1ee4b-152">Run hello following command tooclone hello sample repository.</span></span> 

    ```bash
    git clone https://github.com/Azure-Samples/azure-cosmos-db-graph-java-getting-started.git
    ```

## <a name="review-hello-code"></a><span data-ttu-id="1ee4b-153">Granska hello kod</span><span class="sxs-lookup"><span data-stu-id="1ee4b-153">Review hello code</span></span>

<span data-ttu-id="1ee4b-154">Låt oss göra en snabb genomgång av vad som händer i hello app.</span><span class="sxs-lookup"><span data-stu-id="1ee4b-154">Let's make a quick review of what's happening in hello app.</span></span> <span data-ttu-id="1ee4b-155">Öppna hello `Program.java` från hello \src\GetStarted mapp och söka efter dessa rader med kod.</span><span class="sxs-lookup"><span data-stu-id="1ee4b-155">Open hello `Program.java` file from hello \src\GetStarted folder and find these lines of code.</span></span> 

* <span data-ttu-id="1ee4b-156">Hej Gremlin `Client` har initierats från hello konfigurationen i `src/remote.yaml`.</span><span class="sxs-lookup"><span data-stu-id="1ee4b-156">hello Gremlin `Client` is initialized from hello configuration in `src/remote.yaml`.</span></span>

    ```java
    cluster = Cluster.build(new File("src/remote.yaml")).create();
    ...
    client = cluster.connect();
    ```

* <span data-ttu-id="1ee4b-157">En serie Gremlin steg utförs med hjälp av hello `client.submit` metod.</span><span class="sxs-lookup"><span data-stu-id="1ee4b-157">A series of Gremlin steps are executed using hello `client.submit` method.</span></span>

    ```java
    ResultSet results = client.submit(gremlin);

    CompletableFuture<List<Result>> completableFutureResults = results.all();
    List<Result> resultList = completableFutureResults.get();

    for (Result result : resultList) {
        System.out.println(result.toString());
    }
    ```

## <a name="update-your-connection-string"></a><span data-ttu-id="1ee4b-158">Uppdatera din anslutningssträng</span><span class="sxs-lookup"><span data-stu-id="1ee4b-158">Update your connection string</span></span>

1. <span data-ttu-id="1ee4b-159">Öppna hello src/remote.yaml fil.</span><span class="sxs-lookup"><span data-stu-id="1ee4b-159">Open hello src/remote.yaml file.</span></span> 

3. <span data-ttu-id="1ee4b-160">Fyll i din *värdar*, *användarnamn*, och *lösenord* värden i hello src/remote.yaml filen.</span><span class="sxs-lookup"><span data-stu-id="1ee4b-160">Fill in your *hosts*, *username*, and *password* values in hello src/remote.yaml file.</span></span> <span data-ttu-id="1ee4b-161">hello resten av inställningarna för hello behöver inte toobe ändras.</span><span class="sxs-lookup"><span data-stu-id="1ee4b-161">hello rest of hello settings do not need toobe changed.</span></span>

    <span data-ttu-id="1ee4b-162">Inställning</span><span class="sxs-lookup"><span data-stu-id="1ee4b-162">Setting</span></span>|<span data-ttu-id="1ee4b-163">Föreslaget värde</span><span class="sxs-lookup"><span data-stu-id="1ee4b-163">Suggested value</span></span>|<span data-ttu-id="1ee4b-164">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="1ee4b-164">Description</span></span>
    ---|---|---
    <span data-ttu-id="1ee4b-165">Värdar</span><span class="sxs-lookup"><span data-stu-id="1ee4b-165">Hosts</span></span>|<span data-ttu-id="1ee4b-166">[***.graphs.azure.com]</span><span class="sxs-lookup"><span data-stu-id="1ee4b-166">[***.graphs.azure.com]</span></span>|<span data-ttu-id="1ee4b-167">Se hello skärmbilden nedan.</span><span class="sxs-lookup"><span data-stu-id="1ee4b-167">See hello screenshot following this table.</span></span> <span data-ttu-id="1ee4b-168">Det här värdet är hello Gremlin URI-värde på översiktssidan för hello av hello Azure-portalen inom hakparenteser med hello avslutande: 443 / tas bort.</span><span class="sxs-lookup"><span data-stu-id="1ee4b-168">This value is hello Gremlin URI value on hello Overview page of hello Azure portal, in square brackets, with hello trailing :443/ removed.</span></span><br><br><span data-ttu-id="1ee4b-169">Det här värdet kan också hämtas från hello nycklar på fliken med hello URI-värde genom att ta bort https://, ändra dokument toographs och tar bort avslutande hello: 443 /.</span><span class="sxs-lookup"><span data-stu-id="1ee4b-169">This value can also be retrieved from hello Keys tab, using hello URI value by removing https://, changing documents toographs, and removing hello trailing :443/.</span></span>
    <span data-ttu-id="1ee4b-170">Användarnamn</span><span class="sxs-lookup"><span data-stu-id="1ee4b-170">Username</span></span>|<span data-ttu-id="1ee4b-171">/dbs/sample-database/colls/sample-graph</span><span class="sxs-lookup"><span data-stu-id="1ee4b-171">/dbs/sample-database/colls/sample-graph</span></span>|<span data-ttu-id="1ee4b-172">hello resurs hello formatet `/dbs/<db>/colls/<coll>` där `<db>` är befintliga databasnamnet och `<coll>` är ditt befintliga samlingsnamn.</span><span class="sxs-lookup"><span data-stu-id="1ee4b-172">hello resource of hello form `/dbs/<db>/colls/<coll>` where `<db>` is your existing database name and `<coll>` is your existing collection name.</span></span>
    <span data-ttu-id="1ee4b-173">Lösenord</span><span class="sxs-lookup"><span data-stu-id="1ee4b-173">Password</span></span>|<span data-ttu-id="1ee4b-174">*Din primära huvudnyckel*</span><span class="sxs-lookup"><span data-stu-id="1ee4b-174">*Your primary master key*</span></span>|<span data-ttu-id="1ee4b-175">Se hello andra skärmbilden nedan.</span><span class="sxs-lookup"><span data-stu-id="1ee4b-175">See hello second screenshot following this table.</span></span> <span data-ttu-id="1ee4b-176">Det här värdet är ditt primära nyckel som du kan hämta hello nycklar sidan för hello Azure-portalen hello primärnyckel i rutan.</span><span class="sxs-lookup"><span data-stu-id="1ee4b-176">This value is your primary key, which you can retrieve from hello Keys page of hello Azure portal, in hello Primary Key box.</span></span> <span data-ttu-id="1ee4b-177">Kopiera hello-värde med hjälp av hello kopieringsknappen hello höger på hello rutan.</span><span class="sxs-lookup"><span data-stu-id="1ee4b-177">Copy hello value using hello copy button on hello right side of hello box.</span></span>

    <span data-ttu-id="1ee4b-178">Kopiera hello för hello värdar värdet **Gremlin URI** värde från hello **översikt** sidan.</span><span class="sxs-lookup"><span data-stu-id="1ee4b-178">For hello Hosts value, copy hello **Gremlin URI** value from hello **Overview** page.</span></span> <span data-ttu-id="1ee4b-179">Om den är tom finns hello instruktionerna i hello värdar rad i föregående tabell om hur du skapar hello Gremlin URI från bladet nycklar för hello hello.</span><span class="sxs-lookup"><span data-stu-id="1ee4b-179">If it's empty, see hello instructions in hello Hosts row in hello preceding table about creating hello Gremlin URI from hello Keys blade.</span></span>
<span data-ttu-id="1ee4b-180">![Visa och kopiera hello Gremlin URI-värde på översiktssidan för hello i hello Azure-portalen](./media/create-graph-java/gremlin-uri.png)</span><span class="sxs-lookup"><span data-stu-id="1ee4b-180">![View and copy hello Gremlin URI value on hello Overview page in hello Azure portal](./media/create-graph-java/gremlin-uri.png)</span></span>

    <span data-ttu-id="1ee4b-181">Kopiera hello för hello lösenordsvärdet **primärnyckel** från hello **nycklar** bladet: ![visa och kopiera primärnyckeln i hello Azure-portalen nycklar sida](./media/create-graph-java/keys.png)</span><span class="sxs-lookup"><span data-stu-id="1ee4b-181">For hello Password value, copy hello **Primary key** from hello **Keys** blade: ![View and copy your primary key in hello Azure portal, Keys page](./media/create-graph-java/keys.png)</span></span>

## <a name="run-hello-console-app"></a><span data-ttu-id="1ee4b-182">Kör hello-konsolprogram</span><span class="sxs-lookup"><span data-stu-id="1ee4b-182">Run hello console app</span></span>

1. <span data-ttu-id="1ee4b-183">I hello git terminalfönster, `cd` toohello azure-cosmos-db-graph-java-getting-started mapp.</span><span class="sxs-lookup"><span data-stu-id="1ee4b-183">In hello git terminal window, `cd` toohello azure-cosmos-db-graph-java-getting-started folder.</span></span>

2. <span data-ttu-id="1ee4b-184">Skriv i hello git terminalfönster, `mvn package` tooinstall hello krävs Java-paket.</span><span class="sxs-lookup"><span data-stu-id="1ee4b-184">In hello git terminal window, type `mvn package` tooinstall hello required Java packages.</span></span>

3. <span data-ttu-id="1ee4b-185">Kör i hello git terminalfönster, `mvn exec:java -D exec.mainClass=GetStarted.Program` hello terminalfönster toostart Java-programmet.</span><span class="sxs-lookup"><span data-stu-id="1ee4b-185">In hello git terminal window, run `mvn exec:java -D exec.mainClass=GetStarted.Program` in hello terminal window toostart your Java application.</span></span>

<span data-ttu-id="1ee4b-186">hello terminalfönster visar hello formhörnen läggs toohello diagram.</span><span class="sxs-lookup"><span data-stu-id="1ee4b-186">hello terminal window displays hello vertices being added toohello graph.</span></span> <span data-ttu-id="1ee4b-187">När programmet hello är klar kan du växla tillbaka toohello Azure-portalen i webbläsaren.</span><span class="sxs-lookup"><span data-stu-id="1ee4b-187">Once hello program completes, switch back toohello Azure portal in your internet browser.</span></span> 

<a id="add-sample-data"></a>
## <a name="review-and-add-sample-data"></a><span data-ttu-id="1ee4b-188">Granska och lägg till exempeldata</span><span class="sxs-lookup"><span data-stu-id="1ee4b-188">Review and add sample data</span></span>

<span data-ttu-id="1ee4b-189">Du kan nu gå tillbaka tooData Explorer och se hello formhörnen läggs toohello diagram och lägga till ytterligare datapunkter.</span><span class="sxs-lookup"><span data-stu-id="1ee4b-189">You can now go back tooData Explorer and see hello vertices added toohello graph, and add additional data points.</span></span>

1. <span data-ttu-id="1ee4b-190">Data Explorer, expandera hello **exempeldatabasen**/**exempel diagram**, klickar du på **diagram**, och klicka sedan på **Använd Filter**.</span><span class="sxs-lookup"><span data-stu-id="1ee4b-190">In Data Explorer, expand hello **sample-database**/**sample-graph**, click **Graph**, and then click **Apply Filter**.</span></span> 

   ![Skapa nya dokument i Data Explorer i hello Azure-portalen](./media/create-graph-java/azure-cosmosdb-data-explorer-expanded.png)

2. <span data-ttu-id="1ee4b-192">I hello **resultat** listan, Lägg märke till hello nya användare läggs till toohello diagram.</span><span class="sxs-lookup"><span data-stu-id="1ee4b-192">In hello **Results** list, notice hello new users added toohello graph.</span></span> <span data-ttu-id="1ee4b-193">Välj **ben** och Observera att han har anslutit toorobin.</span><span class="sxs-lookup"><span data-stu-id="1ee4b-193">Select **ben** and notice that he's connected toorobin.</span></span> <span data-ttu-id="1ee4b-194">Du kan flytta hello formhörnen på hello diagrammet explorer, Zooma in och ut och expandera hello storleken på hello diagrammet explorer yta.</span><span class="sxs-lookup"><span data-stu-id="1ee4b-194">You can move hello vertices around on hello graph explorer, zoom in and out, and expand hello size of hello graph explorer surface.</span></span> 

   ![Nya formhörnen i hello diagram i Data Explorer i hello Azure-portalen](./media/create-graph-java/azure-cosmosdb-graph-explorer-new.png)

3. <span data-ttu-id="1ee4b-196">Lägg till några nya användare toohello-diagrammet med hello Data Explorer.</span><span class="sxs-lookup"><span data-stu-id="1ee4b-196">Let's add a few new users toohello graph using hello Data Explorer.</span></span> <span data-ttu-id="1ee4b-197">Klicka på hello **nya Vertex** knappen tooadd data tooyour diagram.</span><span class="sxs-lookup"><span data-stu-id="1ee4b-197">Click hello **New Vertex** button tooadd data tooyour graph.</span></span>

   ![Skapa nya dokument i Data Explorer i hello Azure-portalen](./media/create-graph-java/azure-cosmosdb-data-explorer-new-vertex.png)

4. <span data-ttu-id="1ee4b-199">Ange en etikett för *person* ange hello följande nycklar och värden toocreate hello första hörn i hello graph.</span><span class="sxs-lookup"><span data-stu-id="1ee4b-199">Enter a label of *person* then enter hello following keys and values toocreate hello first vertex in hello graph.</span></span> <span data-ttu-id="1ee4b-200">Tänk på att du kan skapa unika egenskaper för varje person i grafen.</span><span class="sxs-lookup"><span data-stu-id="1ee4b-200">Notice that you can create unique properties for each person in your graph.</span></span> <span data-ttu-id="1ee4b-201">Endast hello id nyckeln är obligatorisk.</span><span class="sxs-lookup"><span data-stu-id="1ee4b-201">Only hello id key is required.</span></span>

    <span data-ttu-id="1ee4b-202">key</span><span class="sxs-lookup"><span data-stu-id="1ee4b-202">key</span></span>|<span data-ttu-id="1ee4b-203">värde</span><span class="sxs-lookup"><span data-stu-id="1ee4b-203">value</span></span>|<span data-ttu-id="1ee4b-204">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="1ee4b-204">Notes</span></span>
    ----|----|----
    <span data-ttu-id="1ee4b-205">id</span><span class="sxs-lookup"><span data-stu-id="1ee4b-205">id</span></span>|<span data-ttu-id="1ee4b-206">ashley</span><span class="sxs-lookup"><span data-stu-id="1ee4b-206">ashley</span></span>|<span data-ttu-id="1ee4b-207">hello Unik identifierare för hello hörn.</span><span class="sxs-lookup"><span data-stu-id="1ee4b-207">hello unique identifier for hello vertex.</span></span> <span data-ttu-id="1ee4b-208">Om du inte anger något id skapas ett automatiskt.</span><span class="sxs-lookup"><span data-stu-id="1ee4b-208">If you don't specify an id, one is generated for you.</span></span>
    <span data-ttu-id="1ee4b-209">kön</span><span class="sxs-lookup"><span data-stu-id="1ee4b-209">gender</span></span>|<span data-ttu-id="1ee4b-210">kvinna</span><span class="sxs-lookup"><span data-stu-id="1ee4b-210">female</span></span>| 
    <span data-ttu-id="1ee4b-211">teknik</span><span class="sxs-lookup"><span data-stu-id="1ee4b-211">tech</span></span> | <span data-ttu-id="1ee4b-212">Java</span><span class="sxs-lookup"><span data-stu-id="1ee4b-212">java</span></span> | 

    > [!NOTE]
    > <span data-ttu-id="1ee4b-213">I den här snabbstartsguiden skapar vi en icke-partitionerad samling.</span><span class="sxs-lookup"><span data-stu-id="1ee4b-213">In this quickstart we create a non-partitioned collection.</span></span> <span data-ttu-id="1ee4b-214">Men om du skapar en partitionerad samling genom att ange en partitionsnyckel när hello samling skapas måste tooinclude hello partitionsnyckel som en nyckel i varje ny hörn.</span><span class="sxs-lookup"><span data-stu-id="1ee4b-214">However, if you create a partitioned collection by specifying a partition key during hello collection creation, then you need tooinclude hello partition key as a key in each new vertex.</span></span> 

5. <span data-ttu-id="1ee4b-215">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="1ee4b-215">Click **OK**.</span></span> <span data-ttu-id="1ee4b-216">Du kan behöva tooexpand skärmen-toosee **OK** på hello ned hello-skärmen.</span><span class="sxs-lookup"><span data-stu-id="1ee4b-216">You may need tooexpand your screen toosee **OK** on hello bottom of hello screen.</span></span>

6. <span data-ttu-id="1ee4b-217">Klicka på **Nytt hörn** igen och lägg till ytterligare en ny användare.</span><span class="sxs-lookup"><span data-stu-id="1ee4b-217">Click **New Vertex** again and add an additional new user.</span></span> <span data-ttu-id="1ee4b-218">Ange en etikett för *person* ange hello följande nycklar och värden:</span><span class="sxs-lookup"><span data-stu-id="1ee4b-218">Enter a label of *person* then enter hello following keys and values:</span></span>

    <span data-ttu-id="1ee4b-219">key</span><span class="sxs-lookup"><span data-stu-id="1ee4b-219">key</span></span>|<span data-ttu-id="1ee4b-220">värde</span><span class="sxs-lookup"><span data-stu-id="1ee4b-220">value</span></span>|<span data-ttu-id="1ee4b-221">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="1ee4b-221">Notes</span></span>
    ----|----|----
    <span data-ttu-id="1ee4b-222">id</span><span class="sxs-lookup"><span data-stu-id="1ee4b-222">id</span></span>|<span data-ttu-id="1ee4b-223">rakesh</span><span class="sxs-lookup"><span data-stu-id="1ee4b-223">rakesh</span></span>|<span data-ttu-id="1ee4b-224">hello Unik identifierare för hello hörn.</span><span class="sxs-lookup"><span data-stu-id="1ee4b-224">hello unique identifier for hello vertex.</span></span> <span data-ttu-id="1ee4b-225">Om du inte anger något id skapas ett automatiskt.</span><span class="sxs-lookup"><span data-stu-id="1ee4b-225">If you don't specify an id, one is generated for you.</span></span>
    <span data-ttu-id="1ee4b-226">kön</span><span class="sxs-lookup"><span data-stu-id="1ee4b-226">gender</span></span>|<span data-ttu-id="1ee4b-227">man</span><span class="sxs-lookup"><span data-stu-id="1ee4b-227">male</span></span>| 
    <span data-ttu-id="1ee4b-228">skola</span><span class="sxs-lookup"><span data-stu-id="1ee4b-228">school</span></span>|<span data-ttu-id="1ee4b-229">MIT</span><span class="sxs-lookup"><span data-stu-id="1ee4b-229">MIT</span></span>| 

7. <span data-ttu-id="1ee4b-230">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="1ee4b-230">Click **OK**.</span></span> 

8. <span data-ttu-id="1ee4b-231">Klicka på **Använd Filter** med hello standard `g.V()` filter.</span><span class="sxs-lookup"><span data-stu-id="1ee4b-231">Click **Apply Filter** with hello default `g.V()` filter.</span></span> <span data-ttu-id="1ee4b-232">Alla hello användare visas nu i hello **resultat** lista.</span><span class="sxs-lookup"><span data-stu-id="1ee4b-232">All of hello users now show in hello **Results** list.</span></span> <span data-ttu-id="1ee4b-233">När du lägger till mer data kan du använda filter toolimit resultat.</span><span class="sxs-lookup"><span data-stu-id="1ee4b-233">As you add more data, you can use filters toolimit your results.</span></span> <span data-ttu-id="1ee4b-234">Som standard använder Data Explorer `g.V()` tooretrieve alla formhörnen i ett diagram, men du kan ändra den olika tooa [diagram frågan](tutorial-query-graph.md), som `g.V().count()`, tooreturn en uppräkning av alla hello formhörnen i hello diagram i JSON-format.</span><span class="sxs-lookup"><span data-stu-id="1ee4b-234">By default, Data Explorer uses `g.V()` tooretrieve all vertices in a graph, but you can change that tooa different [graph query](tutorial-query-graph.md), such as `g.V().count()`, tooreturn a count of all hello vertices in hello graph in JSON format.</span></span>

9. <span data-ttu-id="1ee4b-235">Nu kan vi koppla ihop Rakesh och Ashley.</span><span class="sxs-lookup"><span data-stu-id="1ee4b-235">Now we can connect rakesh and ashley.</span></span> <span data-ttu-id="1ee4b-236">Se till att **Anita** valde hello i **resultat** listan och klicka sedan på hello Redigera bredvid för**mål** nedre höger.</span><span class="sxs-lookup"><span data-stu-id="1ee4b-236">Ensure **ashley** in selected in hello **Results** list, then click hello edit button next too**Targets** on lower right side.</span></span> <span data-ttu-id="1ee4b-237">Du kan behöva toowiden din fönstret toosee hello **egenskaper** område.</span><span class="sxs-lookup"><span data-stu-id="1ee4b-237">You may need toowiden your window toosee hello **Properties** area.</span></span>

   ![Ändra hello målet för ett hörn i ett diagram](./media/create-graph-java/azure-cosmosdb-data-explorer-edit-target.png)

10. <span data-ttu-id="1ee4b-239">I hello **mål** skriver *rakesh*, och i hello **kant etikett** skriver *vet*, och klicka sedan på hello kryssrutan.</span><span class="sxs-lookup"><span data-stu-id="1ee4b-239">In hello **Target** box type *rakesh*, and in hello **Edge label** box type *knows*, and then click hello check box.</span></span>

   ![Lägg till en anslutning mellan Ashley och Rakesh i datautforskaren](./media/create-graph-java/azure-cosmosdb-data-explorer-set-target.png)

11. <span data-ttu-id="1ee4b-241">Nu välja **rakesh** från hello resultatlistan och se att Anita och rakesh är anslutna.</span><span class="sxs-lookup"><span data-stu-id="1ee4b-241">Now select **rakesh** from hello results list and see that ashley and rakesh are connected.</span></span> 

   ![Två hörn anslutna i datautforskaren](./media/create-graph-java/azure-cosmosdb-graph-explorer.png)

    <span data-ttu-id="1ee4b-243">Du kan också använda Data Explorer toocreate lagrade procedurer, UDF: er och utlösare tooperform serversidan affärslogik samt som skala genomflöde.</span><span class="sxs-lookup"><span data-stu-id="1ee4b-243">You can also use Data Explorer toocreate stored procedures, UDFs, and triggers tooperform server-side business logic as well as scale throughput.</span></span> <span data-ttu-id="1ee4b-244">Data Explorer visar alla hello inbyggda programmatisk åtkomst till data i hello API: er, men ger enkel åtkomst tooyour data i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="1ee4b-244">Data Explorer exposes all of hello built-in programmatic data access available in hello APIs, but provides easy access tooyour data in hello Azure portal.</span></span>



## <a name="review-slas-in-hello-azure-portal"></a><span data-ttu-id="1ee4b-245">Granska SLA: er i hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="1ee4b-245">Review SLAs in hello Azure portal</span></span>

[!INCLUDE [cosmosdb-tutorial-review-slas](../../includes/cosmos-db-tutorial-review-slas.md)]

## <a name="clean-up-resources"></a><span data-ttu-id="1ee4b-246">Rensa resurser</span><span class="sxs-lookup"><span data-stu-id="1ee4b-246">Clean up resources</span></span>

<span data-ttu-id="1ee4b-247">Om du inte kommer toocontinue toouse den här appen, tar du bort alla resurser som skapats av denna Snabbstart i hello Azure-portalen med hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="1ee4b-247">If you're not going toocontinue toouse this app, delete all resources created by this quickstart in hello Azure portal with hello following steps:</span></span> 

1. <span data-ttu-id="1ee4b-248">Hello vänstra menyn i hello Azure-portalen klickar du på **resursgrupper** och klicka sedan på hello namnet på hello resurs du skapat.</span><span class="sxs-lookup"><span data-stu-id="1ee4b-248">From hello left-hand menu in hello Azure portal, click **Resource groups** and then click hello name of hello resource you created.</span></span> 
2. <span data-ttu-id="1ee4b-249">På din resurs gruppen klickar du på **ta bort**typnamn hello för hello resurs toodelete i hello textrutan och klicka sedan på **ta bort**.</span><span class="sxs-lookup"><span data-stu-id="1ee4b-249">On your resource group page, click **Delete**, type hello name of hello resource toodelete in hello text box, and then click **Delete**.</span></span>

## <a name="next-steps"></a><span data-ttu-id="1ee4b-250">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="1ee4b-250">Next steps</span></span>

<span data-ttu-id="1ee4b-251">Du har lärt dig hur toocreate ett Azure DB som Cosmos-konto och skapa diagram med hello Data Explorer och kör en app i denna Snabbstart.</span><span class="sxs-lookup"><span data-stu-id="1ee4b-251">In this quickstart, you've learned how toocreate an Azure Cosmos DB account, create a graph using hello Data Explorer, and run an app.</span></span> <span data-ttu-id="1ee4b-252">Nu kan du skapa mer komplexa frågor och implementera kraftfull logik för grafbläddring med Gremlin.</span><span class="sxs-lookup"><span data-stu-id="1ee4b-252">You can now build more complex queries and implement powerful graph traversal logic using Gremlin.</span></span> 

> [!div class="nextstepaction"]
> [<span data-ttu-id="1ee4b-253">Fråga med hjälp av Gremlin</span><span class="sxs-lookup"><span data-stu-id="1ee4b-253">Query using Gremlin</span></span>](tutorial-query-graph.md)

