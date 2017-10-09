---
title: "aaaJava självstudien om apputveckling med hjälp av Azure Cosmos DB | Microsoft Docs"
description: "Den här självstudien Java visar hur toouse hello Azure Cosmos DB och hello DocumentDB API toostore och komma åt data från en Java-program på Azure Websites."
keywords: "Programutveckling, självstudier för databas, java-program, självstudier för java-webbprogram, documentdb, azure, Microsoft azure"
services: cosmos-db
documentationcenter: java
author: dennyglee
manager: jhubbard
editor: mimig
ms.assetid: 0867a4a2-4bf5-4898-a1f4-44e3868f8725
ms.service: cosmos-db
ms.devlang: java
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.date: 08/22/2017
ms.author: denlee
ms.openlocfilehash: e073de23beb0037ee1e37b48a69e8fe7cdc3fc1d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="build-a-java-web-application-using-azure-cosmos-db-and-hello-documentdb-api"></a><span data-ttu-id="68780-104">Skapa en Java-webbapp med Azure Cosmos DB och hello DocumentDB-API</span><span class="sxs-lookup"><span data-stu-id="68780-104">Build a Java web application using Azure Cosmos DB and hello DocumentDB API</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="68780-105">.NET</span><span class="sxs-lookup"><span data-stu-id="68780-105">.NET</span></span>](documentdb-dotnet-application.md)
> * [<span data-ttu-id="68780-106">Node.js</span><span class="sxs-lookup"><span data-stu-id="68780-106">Node.js</span></span>](documentdb-nodejs-application.md)
> * [<span data-ttu-id="68780-107">Java</span><span class="sxs-lookup"><span data-stu-id="68780-107">Java</span></span>](documentdb-java-application.md)
> * [<span data-ttu-id="68780-108">Python</span><span class="sxs-lookup"><span data-stu-id="68780-108">Python</span></span>](documentdb-python-application.md)
> 
> 

<span data-ttu-id="68780-109">Den här självstudien Java visar hur toouse hello [Microsoft Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) service toostore och komma åt data från en Java-program på Azure App Service Web Apps.</span><span class="sxs-lookup"><span data-stu-id="68780-109">This Java web application tutorial shows you how toouse hello [Microsoft Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) service toostore and access data from a Java application hosted on Azure App Service Web Apps.</span></span> <span data-ttu-id="68780-110">I det här avsnittet får du veta följande:</span><span class="sxs-lookup"><span data-stu-id="68780-110">In this topic, you will learn:</span></span>

* <span data-ttu-id="68780-111">Hur toobuild ett grundläggande JavaServer sidor (JSP)-program i Eclipse.</span><span class="sxs-lookup"><span data-stu-id="68780-111">How toobuild a basic JavaServer Pages (JSP) application in Eclipse.</span></span>
* <span data-ttu-id="68780-112">Hur toowork med hello Azure Cosmos DB tjänst med hjälp av hello [Azure Cosmos DB Java SDK](https://github.com/Azure/azure-documentdb-java).</span><span class="sxs-lookup"><span data-stu-id="68780-112">How toowork with hello Azure Cosmos DB service using hello [Azure Cosmos DB Java SDK](https://github.com/Azure/azure-documentdb-java).</span></span>

<span data-ttu-id="68780-113">Den här självstudien om Java visar hur toocreate en webbaserad aktivitetshanteringsapp som gör att du toocreate, hämta och markera aktiviteter som slutförda, enligt följande bild hello.</span><span class="sxs-lookup"><span data-stu-id="68780-113">This Java application tutorial shows you how toocreate a web-based task-management application that enables you toocreate, retrieve, and mark tasks as complete, as shown in hello following image.</span></span> <span data-ttu-id="68780-114">Var och en av hello uppgifter i hello ToDo-listan lagras som JSON-dokument i Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="68780-114">Each of hello tasks in hello ToDo list are stored as JSON documents in Azure Cosmos DB.</span></span>

![Java-app med att göra-lista](./media/documentdb-java-application/image1.png)

> [!TIP]
> <span data-ttu-id="68780-116">Den här självstudien om apputveckling förutsätter att du har tidigare erfarenhet av Java.</span><span class="sxs-lookup"><span data-stu-id="68780-116">This application development tutorial assumes that you have prior experience using Java.</span></span> <span data-ttu-id="68780-117">Om du är ny tooJava eller hello [nödvändiga verktyg](#Prerequisites), vi rekommenderar att du hämtar hello fullständig [todo](https://github.com/Azure-Samples/documentdb-java-todo-app) projektet från GitHub och skapa den med hjälp av [hello instruktioner hello slutet av det här artikel](#GetProject).</span><span class="sxs-lookup"><span data-stu-id="68780-117">If you are new tooJava or hello [prerequisite tools](#Prerequisites), we recommend downloading hello complete [todo](https://github.com/Azure-Samples/documentdb-java-todo-app) project from GitHub and building it using [hello instructions at hello end of this article](#GetProject).</span></span> <span data-ttu-id="68780-118">När du har skapat kan granska du hello artikel toogain inblick i hello koden i hello ramen för hello-projekt.</span><span class="sxs-lookup"><span data-stu-id="68780-118">Once you have it built, you can review hello article toogain insight on hello code in hello context of hello project.</span></span>  
> 
> 

## <span data-ttu-id="68780-119"><a id="Prerequisites"></a>Förhandskrav för den här självstudien för Java-webbprogram</span><span class="sxs-lookup"><span data-stu-id="68780-119"><a id="Prerequisites"></a>Prerequisites for this Java web application tutorial</span></span>
<span data-ttu-id="68780-120">Innan du börjar den här självstudien om apputveckling måste du ha hello följande:</span><span class="sxs-lookup"><span data-stu-id="68780-120">Before you begin this application development tutorial, you must have hello following:</span></span>

* <span data-ttu-id="68780-121">Ett aktivt Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="68780-121">An active Azure account.</span></span> <span data-ttu-id="68780-122">Om du inte har något konto kan du skapa ett kostnadsfritt utvärderingskonto på bara några minuter.</span><span class="sxs-lookup"><span data-stu-id="68780-122">If you don't have an account, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="68780-123">Mer information om den [kostnadsfria utvärderingsversionen av Azure](https://azure.microsoft.com/pricing/free-trial/)</span><span class="sxs-lookup"><span data-stu-id="68780-123">For details, see [Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/)</span></span>

    <span data-ttu-id="68780-124">ELLER</span><span class="sxs-lookup"><span data-stu-id="68780-124">OR</span></span>

    <span data-ttu-id="68780-125">En lokal installation av hello [Azure Cosmos DB emulatorn](local-emulator.md).</span><span class="sxs-lookup"><span data-stu-id="68780-125">A local installation of hello [Azure Cosmos DB Emulator](local-emulator.md).</span></span>
* <span data-ttu-id="68780-126">[Java Development Kit (JDK) 7+](http://www.oracle.com/technetwork/java/javase/downloads/index.html).</span><span class="sxs-lookup"><span data-stu-id="68780-126">[Java Development Kit (JDK) 7+](http://www.oracle.com/technetwork/java/javase/downloads/index.html).</span></span>
* [<span data-ttu-id="68780-127">Eclipse IDE för Java EE-utvecklare.</span><span class="sxs-lookup"><span data-stu-id="68780-127">Eclipse IDE for Java EE Developers.</span></span>](http://www.eclipse.org/downloads/packages/eclipse-ide-java-ee-developers/lunasr1)
* [<span data-ttu-id="68780-128">En Azure-webbplats med Java runtime environment (t.ex. Tomcat eller Jetty) aktiverat.</span><span class="sxs-lookup"><span data-stu-id="68780-128">An Azure Web Site with a Java runtime environment (e.g. Tomcat or Jetty) enabled.</span></span>](../app-service-web/web-sites-java-get-started.md)

<span data-ttu-id="68780-129">Om du installerar verktygen för hello första gången, coreservlets.com ger en beskrivning av hello installationsprocessen under hello Snabbstart i sina [Självstudier: Installing TomCat7 and med Eclipse](http://www.coreservlets.com/Apache-Tomcat-Tutorial/tomcat-7-with-eclipse.html) artikel.</span><span class="sxs-lookup"><span data-stu-id="68780-129">If you're installing these tools for hello first time, coreservlets.com provides a walk-through of hello installation process in hello Quick Start section of their [Tutorial: Installing TomCat7 and Using it with Eclipse](http://www.coreservlets.com/Apache-Tomcat-Tutorial/tomcat-7-with-eclipse.html) article.</span></span>

## <span data-ttu-id="68780-130"><a id="CreateDB"></a>Steg 1: Skapa ett Azure DB som Cosmos-konto</span><span class="sxs-lookup"><span data-stu-id="68780-130"><a id="CreateDB"></a>Step 1: Create an Azure Cosmos DB account</span></span>
<span data-ttu-id="68780-131">Vi ska börja med att skapa ett Azure Cosmos DB-konto.</span><span class="sxs-lookup"><span data-stu-id="68780-131">Let's start by creating an Azure Cosmos DB account.</span></span> <span data-ttu-id="68780-132">Om du redan har ett konto eller om du använder hello Azure Cosmos DB-emulatorn för den här kursen kan du hoppa över för[steg 2: skapa hello JSP Java-program](#CreateJSP).</span><span class="sxs-lookup"><span data-stu-id="68780-132">If you already have an account or if you are using hello Azure Cosmos DB Emulator for this tutorial, you can skip too[Step 2: Create hello Java JSP application](#CreateJSP).</span></span>

[!INCLUDE [create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

[!INCLUDE [keys](../../includes/cosmos-db-keys.md)]

## <span data-ttu-id="68780-133"><a id="CreateJSP"></a>Steg 2: Skapa hello JSP Java-program</span><span class="sxs-lookup"><span data-stu-id="68780-133"><a id="CreateJSP"></a>Step 2: Create hello Java JSP application</span></span>
<span data-ttu-id="68780-134">toocreate hello JSP-appen:</span><span class="sxs-lookup"><span data-stu-id="68780-134">toocreate hello JSP application:</span></span>

1. <span data-ttu-id="68780-135">Vi börjar med att skapa ett Java-projekt.</span><span class="sxs-lookup"><span data-stu-id="68780-135">First, we’ll start off by creating a Java project.</span></span> <span data-ttu-id="68780-136">Starta Eclipse och klicka på **Arkiv**, **Nytt** och slutligen **Dynamiskt webbprojekt**.</span><span class="sxs-lookup"><span data-stu-id="68780-136">Start Eclipse, then click **File**, click **New**, and then click **Dynamic Web Project**.</span></span> <span data-ttu-id="68780-137">Om du inte ser **dynamiskt webbprojekt** visas som tillgängliga projekt hello följande: Klicka på **filen**, klickar du på **ny**, klickar du på **projekt**..., expandera **Web**, klickar du på **dynamiskt webbprojekt**, och klicka på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="68780-137">If you don’t see **Dynamic Web Project** listed as an available project, do hello following: click **File**, click **New**, click **Project**…, expand **Web**, click **Dynamic Web Project**, and click **Next**.</span></span>
   
    ![JSP Java-apputveckling](./media/documentdb-java-application/image10.png)
2. <span data-ttu-id="68780-139">Ange ett projektnamn i hello **projektnamn** rutan och i hello **Körningsmål** nedrullningsbara menyn, Välj eventuellt ett värde (t.ex. Apache Tomcat v7.0) och klicka sedan på **Slutför**.</span><span class="sxs-lookup"><span data-stu-id="68780-139">Enter a project name in hello **Project name** box, and in hello **Target Runtime** drop-down menu, optionally select a value (e.g. Apache Tomcat v7.0), and then click **Finish**.</span></span> <span data-ttu-id="68780-140">Om du markerar ett mål för körning kan du toorun projektet lokalt genom Eclipse.</span><span class="sxs-lookup"><span data-stu-id="68780-140">Selecting a target runtime enables you toorun your project locally through Eclipse.</span></span>
3. <span data-ttu-id="68780-141">Expandera projektet i Eclipse i vyn Projektutforskaren hello.</span><span class="sxs-lookup"><span data-stu-id="68780-141">In Eclipse, in hello Project Explorer view, expand your project.</span></span> <span data-ttu-id="68780-142">Högerklicka på **Webbinnehåll**, klicka på **Ny** och sedan på **JSP-fil**.</span><span class="sxs-lookup"><span data-stu-id="68780-142">Right-click **WebContent**, click **New**, and then click **JSP File**.</span></span>
4. <span data-ttu-id="68780-143">I hello **ny JSP-fil** dialogrutan, namnet hello fil **index.jsp**.</span><span class="sxs-lookup"><span data-stu-id="68780-143">In hello **New JSP File** dialog box, name hello file **index.jsp**.</span></span> <span data-ttu-id="68780-144">Behåll hello överordnade mappen som **webbinnehåll**som visas i hello följande bild och klicka sedan på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="68780-144">Keep hello parent folder as **WebContent**, as shown in hello following illustration, and then click **Next**.</span></span>
   
    ![Skapa en ny JSP-fil – självstudie om Java-webbapp](./media/documentdb-java-application/image11.png)
5. <span data-ttu-id="68780-146">I hello **Välj JSP-mall** dialogrutan för hello syftet med den här självstudiekursen väljer **ny JSP-fil (html)**, och klicka sedan på **Slutför**.</span><span class="sxs-lookup"><span data-stu-id="68780-146">In hello **Select JSP Template** dialog box, for hello purpose of this tutorial select **New JSP File (html)**, and then click **Finish**.</span></span>
6. <span data-ttu-id="68780-147">Lägga till text toodisplay när hello index.jsp-filen öppnas i Eclipse **Hello World!**</span><span class="sxs-lookup"><span data-stu-id="68780-147">When hello index.jsp file opens in Eclipse, add text toodisplay **Hello World!**</span></span> <span data-ttu-id="68780-148">inom hello befintliga <body> element.</span><span class="sxs-lookup"><span data-stu-id="68780-148">within hello existing <body> element.</span></span> <span data-ttu-id="68780-149">Uppdaterade <body> innehållet bör se ut som följande kod hello:</span><span class="sxs-lookup"><span data-stu-id="68780-149">Your updated <body> content should look like hello following code:</span></span>
   
        <body>
            <% out.println("Hello World!"); %>
        </body>
7. <span data-ttu-id="68780-150">Spara hello index.jsp-filen.</span><span class="sxs-lookup"><span data-stu-id="68780-150">Save hello index.jsp file.</span></span>
8. <span data-ttu-id="68780-151">Om du anger ett mål för körning i steg 2, kan du klicka på **projekt** och sedan **kör** toorun JSP-appen lokalt:</span><span class="sxs-lookup"><span data-stu-id="68780-151">If you set a target runtime in step 2, you can click **Project** and then **Run** toorun your JSP application locally:</span></span>
   
    ![Hello World – självstudie om Java-app](./media/documentdb-java-application/image12.png)

## <span data-ttu-id="68780-153"><a id="InstallSDK"></a>Steg 3: Installera hello DocumentDB Java SDK</span><span class="sxs-lookup"><span data-stu-id="68780-153"><a id="InstallSDK"></a>Step 3: Install hello DocumentDB Java SDK</span></span>
<span data-ttu-id="68780-154">hello enklaste sättet toopull i hello DocumentDB Java SDK och dess beroenden är via [Apache Maven](http://maven.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="68780-154">hello easiest way toopull in hello DocumentDB Java SDK and its dependencies is through [Apache Maven](http://maven.apache.org/).</span></span>

<span data-ttu-id="68780-155">toodo detta, behöver du tooconvert projekt tooa maven-projekt genom att slutföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="68780-155">toodo this, you will need tooconvert your project tooa maven project by completing hello following steps:</span></span>

1. <span data-ttu-id="68780-156">Högerklicka på projektet i hello Projektutforskaren, klicka på **konfigurera**, klickar du på **konvertera tooMaven projekt**.</span><span class="sxs-lookup"><span data-stu-id="68780-156">Right-click your project in hello Project Explorer, click **Configure**, click **Convert tooMaven Project**.</span></span>
2. <span data-ttu-id="68780-157">I hello **Skapa ny POM** , acceptera hello standardinställningar och på **Slutför**.</span><span class="sxs-lookup"><span data-stu-id="68780-157">In hello **Create new POM** window, accept hello defaults and click **Finish**.</span></span>
3. <span data-ttu-id="68780-158">I **Projektutforskaren**öppnar hello pom.xml fil.</span><span class="sxs-lookup"><span data-stu-id="68780-158">In **Project Explorer**, open hello pom.xml file.</span></span>
4. <span data-ttu-id="68780-159">På hello **beroenden** på fliken hello **beroenden** rutan klickar du på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="68780-159">On hello **Dependencies** tab, in hello **Dependencies** pane, click **Add**.</span></span>
5. <span data-ttu-id="68780-160">I hello **Välj beroende** fönstret hello följande:</span><span class="sxs-lookup"><span data-stu-id="68780-160">In hello **Select Dependency** window, do hello following:</span></span>
   
   * <span data-ttu-id="68780-161">I hello **grupp-Id** ange com.microsoft.azure.</span><span class="sxs-lookup"><span data-stu-id="68780-161">In hello **Group Id** box, enter com.microsoft.azure.</span></span>
   * <span data-ttu-id="68780-162">I hello **artefakt-Id** ange azure documentdb.</span><span class="sxs-lookup"><span data-stu-id="68780-162">In hello **Artifact Id** box, enter azure-documentdb.</span></span>
   * <span data-ttu-id="68780-163">I hello **Version** ange 1.5.1.</span><span class="sxs-lookup"><span data-stu-id="68780-163">In hello **Version** box, enter 1.5.1.</span></span>
     
   ![Installera DocumentDB-SDK för Java-app](./media/documentdb-java-application/image13.png)
     
   * <span data-ttu-id="68780-165">Eller Lägg till hello beroende XML för grupp-Id och artefakt-Id direkt toohello pom.xml via en textredigerare:</span><span class="sxs-lookup"><span data-stu-id="68780-165">Or add hello dependency XML for Group Id and Artifact Id directly toohello pom.xml via a text editor:</span></span>
     
        <span data-ttu-id="68780-166"><dependency><groupId>com.microsoft.azure</groupId> <artifactId>azure documentdb</artifactId> <version>1.9.1</version></dependency></span><span class="sxs-lookup"><span data-stu-id="68780-166"><dependency> <groupId>com.microsoft.azure</groupId> <artifactId>azure-documentdb</artifactId> <version>1.9.1</version> </dependency></span></span>
6. <span data-ttu-id="68780-167">Klicka på **OK** så installerar Maven hello DocumentDB Java SDK.</span><span class="sxs-lookup"><span data-stu-id="68780-167">Click **OK** and Maven will install hello DocumentDB Java SDK.</span></span>
7. <span data-ttu-id="68780-168">Spara hello pom.xml.</span><span class="sxs-lookup"><span data-stu-id="68780-168">Save hello pom.xml file.</span></span>

## <span data-ttu-id="68780-169"><a id="UseService"></a>Steg 4: Använda hello Azure DB som Cosmos-tjänsten i ett Java-program</span><span class="sxs-lookup"><span data-stu-id="68780-169"><a id="UseService"></a>Step 4: Using hello Azure Cosmos DB service in a Java application</span></span>
1. <span data-ttu-id="68780-170">Först ska vi definiera hello TodoItem objekt i TodoItem.java:</span><span class="sxs-lookup"><span data-stu-id="68780-170">First, let's define hello TodoItem object in TodoItem.java:</span></span>
   
        @Data
        @Builder
        public class TodoItem {
            private String category;
            private boolean complete;
            private String id;
            private String name;
        }
   
    <span data-ttu-id="68780-171">I det här projektet använder [Project Lombok](http://projectlombok.org/) toogenerate hello konstruktor, GET, set-metoder och ett verktyg.</span><span class="sxs-lookup"><span data-stu-id="68780-171">In this project, we are using [Project Lombok](http://projectlombok.org/) toogenerate hello constructor, getters, setters, and a builder.</span></span> <span data-ttu-id="68780-172">Du kan också skriva koden manuellt eller har hello IDE generera den.</span><span class="sxs-lookup"><span data-stu-id="68780-172">Alternatively, you can write this code manually or have hello IDE generate it.</span></span>
2. <span data-ttu-id="68780-173">tooinvoke hello Azure Cosmos DB tjänsten måste du initiera en ny **DocumentClient**.</span><span class="sxs-lookup"><span data-stu-id="68780-173">tooinvoke hello Azure Cosmos DB service, you must instantiate a new **DocumentClient**.</span></span> <span data-ttu-id="68780-174">I allmänhet är det bästa tooreuse hello **DocumentClient** - snarare än att skapa en ny klient för varje efterföljande begäran.</span><span class="sxs-lookup"><span data-stu-id="68780-174">In general, it is best tooreuse hello **DocumentClient** - rather than construct a new client for each subsequent request.</span></span> <span data-ttu-id="68780-175">Vi kan återanvända hello klienten genom att omsluta hello klienten i en **DocumentClientFactory**.</span><span class="sxs-lookup"><span data-stu-id="68780-175">We can reuse hello client by wrapping hello client in a **DocumentClientFactory**.</span></span> <span data-ttu-id="68780-176">I DocumentClientFactory.java, behöver du toopaste hello URI och PRIMÄRNYCKEL värden som du sparade tooyour Urklipp i [steg 1](#CreateDB).</span><span class="sxs-lookup"><span data-stu-id="68780-176">In DocumentClientFactory.java, you need toopaste hello URI and PRIMARY KEY value you saved tooyour clipboard in [step 1](#CreateDB).</span></span> <span data-ttu-id="68780-177">Ersätt [YOUR\_ENDPOINT\_HERE] med din URI och ersätt [YOUR\_KEY\_HERE] med din PRIMÄRNYCKEL.</span><span class="sxs-lookup"><span data-stu-id="68780-177">Replace [YOUR\_ENDPOINT\_HERE] with your URI and replace [YOUR\_KEY\_HERE] with your PRIMARY KEY.</span></span>
   
        private static final String HOST = "[YOUR_ENDPOINT_HERE]";
        private static final String MASTER_KEY = "[YOUR_KEY_HERE]";
   
        private static DocumentClient documentClient = new DocumentClient(HOST, MASTER_KEY,
                        ConnectionPolicy.GetDefault(), ConsistencyLevel.Session);
   
        public static DocumentClient getDocumentClient() {
            return documentClient;
        }
3. <span data-ttu-id="68780-178">Nu skapar vi ett objekt DAO (Data Access) tooabstract beständighet Cosmos DB för tooAzure vår ToDo-objekt.</span><span class="sxs-lookup"><span data-stu-id="68780-178">Now let's create a Data Access Object (DAO) tooabstract persisting our ToDo items tooAzure Cosmos DB.</span></span>
   
    <span data-ttu-id="68780-179">Order toosave ToDo-objekt tooa samling, hello klienten måste tooknow vilken databas och samling toopersist för (refererade med självlänkar).</span><span class="sxs-lookup"><span data-stu-id="68780-179">In order toosave ToDo items tooa collection, hello client needs tooknow which database and collection toopersist too(as referenced by self-links).</span></span> <span data-ttu-id="68780-180">I allmänhet är det bästa toocache hello databasen och samlingen när det är möjligt tooavoid ytterligare rundor toohello databas.</span><span class="sxs-lookup"><span data-stu-id="68780-180">In general, it is best toocache hello database and collection when possible tooavoid additional round-trips toohello database.</span></span>
   
    <span data-ttu-id="68780-181">hello följande kod visar hur tooretrieve vår databas och samling, om den finns, eller skapa en ny om det inte finns:</span><span class="sxs-lookup"><span data-stu-id="68780-181">hello following code illustrates how tooretrieve our database and collection, if it exists, or create a new one if it doesn't exist:</span></span>
   
        public class DocDbDao implements TodoDao {
            // hello name of our database.
            private static final String DATABASE_ID = "TodoDB";
   
            // hello name of our collection.
            private static final String COLLECTION_ID = "TodoCollection";
   
            // hello Azure Cosmos DB Client
            private static DocumentClient documentClient = DocumentClientFactory
                    .getDocumentClient();
   
            // Cache for hello database object, so we don't have tooquery for it to
            // retrieve self links.
            private static Database databaseCache;
   
            // Cache for hello collection object, so we don't have tooquery for it to
            // retrieve self links.
            private static DocumentCollection collectionCache;
   
            private Database getTodoDatabase() {
                if (databaseCache == null) {
                    // Get hello database if it exists
                    List<Database> databaseList = documentClient
                            .queryDatabases(
                                    "SELECT * FROM root r WHERE r.id='" + DATABASE_ID
                                            + "'", null).getQueryIterable().toList();
   
                    if (databaseList.size() > 0) {
                        // Cache hello database object so we won't have tooquery for it
                        // later tooretrieve hello selfLink.
                        databaseCache = databaseList.get(0);
                    } else {
                        // Create hello database if it doesn't exist.
                        try {
                            Database databaseDefinition = new Database();
                            databaseDefinition.setId(DATABASE_ID);
   
                            databaseCache = documentClient.createDatabase(
                                    databaseDefinition, null).getResource();
                        } catch (DocumentClientException e) {
                            // TODO: Something has gone terribly wrong - hello app wasn't
                            // able tooquery or create hello collection.
                            // Verify your connection, endpoint, and key.
                            e.printStackTrace();
                        }
                    }
                }
   
                return databaseCache;
            }
   
            private DocumentCollection getTodoCollection() {
                if (collectionCache == null) {
                    // Get hello collection if it exists.
                    List<DocumentCollection> collectionList = documentClient
                            .queryCollections(
                                    getTodoDatabase().getSelfLink(),
                                    "SELECT * FROM root r WHERE r.id='" + COLLECTION_ID
                                            + "'", null).getQueryIterable().toList();
   
                    if (collectionList.size() > 0) {
                        // Cache hello collection object so we won't have tooquery for it
                        // later tooretrieve hello selfLink.
                        collectionCache = collectionList.get(0);
                    } else {
                        // Create hello collection if it doesn't exist.
                        try {
                            DocumentCollection collectionDefinition = new DocumentCollection();
                            collectionDefinition.setId(COLLECTION_ID);
   
                            collectionCache = documentClient.createCollection(
                                    getTodoDatabase().getSelfLink(),
                                    collectionDefinition, null).getResource();
                        } catch (DocumentClientException e) {
                            // TODO: Something has gone terribly wrong - hello app wasn't
                            // able tooquery or create hello collection.
                            // Verify your connection, endpoint, and key.
                            e.printStackTrace();
                        }
                    }
                }
   
                return collectionCache;
            }
        }
4. <span data-ttu-id="68780-182">hello nästa steg är toowrite vissa kod toopersist hello TodoItems i samlingen toohello.</span><span class="sxs-lookup"><span data-stu-id="68780-182">hello next step is toowrite some code toopersist hello TodoItems in toohello collection.</span></span> <span data-ttu-id="68780-183">I det här exemplet ska vi använda [Gson](https://code.google.com/p/google-gson/) tooserialize och deserialisera TodoItem Plain Old Java Objects (Pojo) tooJSON dokument.</span><span class="sxs-lookup"><span data-stu-id="68780-183">In this example, we will use [Gson](https://code.google.com/p/google-gson/) tooserialize and de-serialize TodoItem Plain Old Java Objects (POJOs) tooJSON documents.</span></span>
   
        // We'll use Gson for POJO <=> JSON serialization for this example.
        private static Gson gson = new Gson();
   
        @Override
        public TodoItem createTodoItem(TodoItem todoItem) {
            // Serialize hello TodoItem as a JSON Document.
            Document todoItemDocument = new Document(gson.toJson(todoItem));
   
            // Annotate hello document as a TodoItem for retrieval (so that we can
            // store multiple entity types in hello collection).
            todoItemDocument.set("entityType", "todoItem");
   
            try {
                // Persist hello document using hello DocumentClient.
                todoItemDocument = documentClient.createDocument(
                        getTodoCollection().getSelfLink(), todoItemDocument, null,
                        false).getResource();
            } catch (DocumentClientException e) {
                e.printStackTrace();
                return null;
            }
   
            return gson.fromJson(todoItemDocument.toString(), TodoItem.class);
        }
5. <span data-ttu-id="68780-184">Precis som med databaser och samlingar i Azure Cosmos DB används självlänkar även för att referera till dokument.</span><span class="sxs-lookup"><span data-stu-id="68780-184">Like Azure Cosmos DB databases and collections, documents are also referenced by self-links.</span></span> <span data-ttu-id="68780-185">Hej följande helper-funktionen kan vi hämta dokument av ett annat attribut (t.ex. ”id”) än självlänkar:</span><span class="sxs-lookup"><span data-stu-id="68780-185">hello following helper function lets us retrieve documents by another attribute (e.g. "id") rather than self-link:</span></span>
   
        private Document getDocumentById(String id) {
            // Retrieve hello document using hello DocumentClient.
            List<Document> documentList = documentClient
                    .queryDocuments(getTodoCollection().getSelfLink(),
                            "SELECT * FROM root r WHERE r.id='" + id + "'", null)
                    .getQueryIterable().toList();
   
            if (documentList.size() > 0) {
                return documentList.get(0);
            } else {
                return null;
            }
        }
6. <span data-ttu-id="68780-186">Vi kan använda hello hjälpmetoden i steg 5 tooretrieve ett TodoItem JSON-dokument efter id och sedan deserialisera tooa POJO:</span><span class="sxs-lookup"><span data-stu-id="68780-186">We can use hello helper method in step 5 tooretrieve a TodoItem JSON document by id and then deserialize it tooa POJO:</span></span>
   
        @Override
        public TodoItem readTodoItem(String id) {
            // Retrieve hello document by id using our helper method.
            Document todoItemDocument = getDocumentById(id);
   
            if (todoItemDocument != null) {
                // De-serialize hello document in tooa TodoItem.
                return gson.fromJson(todoItemDocument.toString(), TodoItem.class);
            } else {
                return null;
            }
        }
7. <span data-ttu-id="68780-187">Vi kan också använda hello DocumentClient tooget en samling eller en lista över TodoItems med hjälp av DocumentDB SQL:</span><span class="sxs-lookup"><span data-stu-id="68780-187">We can also use hello DocumentClient tooget a collection or list of TodoItems using DocumentDB SQL:</span></span>
   
        @Override
        public List<TodoItem> readTodoItems() {
            List<TodoItem> todoItems = new ArrayList<TodoItem>();
   
            // Retrieve hello TodoItem documents
            List<Document> documentList = documentClient
                    .queryDocuments(getTodoCollection().getSelfLink(),
                            "SELECT * FROM root r WHERE r.entityType = 'todoItem'",
                            null).getQueryIterable().toList();
   
            // De-serialize hello documents in tooTodoItems.
            for (Document todoItemDocument : documentList) {
                todoItems.add(gson.fromJson(todoItemDocument.toString(),
                        TodoItem.class));
            }
   
            return todoItems;
        }
8. <span data-ttu-id="68780-188">Det finns många sätt tooupdate ett dokument med hello DocumentClient.</span><span class="sxs-lookup"><span data-stu-id="68780-188">There are many ways tooupdate a document with hello DocumentClient.</span></span> <span data-ttu-id="68780-189">I vår Todo-App vill vi toobe kan tootoggle om ett TodoItem har slutförts.</span><span class="sxs-lookup"><span data-stu-id="68780-189">In our Todo list application, we want toobe able tootoggle whether a TodoItem is complete.</span></span> <span data-ttu-id="68780-190">Detta kan uppnås genom att uppdatera hello ”slutförd” attribut i hello dokument:</span><span class="sxs-lookup"><span data-stu-id="68780-190">This can be achieved by updating hello "complete" attribute within hello document:</span></span>
   
        @Override
        public TodoItem updateTodoItem(String id, boolean isComplete) {
            // Retrieve hello document from hello database
            Document todoItemDocument = getDocumentById(id);
   
            // You can update hello document as a JSON document directly.
            // For more complex operations - you could de-serialize hello document in
            // tooa POJO, update hello POJO, and then re-serialize hello POJO back in to
            // a document.
            todoItemDocument.set("complete", isComplete);
   
            try {
                // Persist/replace hello updated document.
                todoItemDocument = documentClient.replaceDocument(todoItemDocument,
                        null).getResource();
            } catch (DocumentClientException e) {
                e.printStackTrace();
                return null;
            }
   
            return gson.fromJson(todoItemDocument.toString(), TodoItem.class);
        }
9. <span data-ttu-id="68780-191">Slutligen vill vi hello möjlighet toodelete ett TodoItem från listan.</span><span class="sxs-lookup"><span data-stu-id="68780-191">Finally, we want hello ability toodelete a TodoItem from our list.</span></span> <span data-ttu-id="68780-192">toodo kan vi kan använda hello hjälpmetoden vi skrev tidigare tooretrieve hello självlänken och sedan instruera hello klienten toodelete den:</span><span class="sxs-lookup"><span data-stu-id="68780-192">toodo this, we can use hello helper method we wrote earlier tooretrieve hello self-link and then tell hello client toodelete it:</span></span>
   
        @Override
        public boolean deleteTodoItem(String id) {
            // Azure Cosmos DB refers toodocuments by self link rather than id.
   
            // Query for hello document tooretrieve hello self link.
            Document todoItemDocument = getDocumentById(id);
   
            try {
                // Delete hello document by self link.
                documentClient.deleteDocument(todoItemDocument.getSelfLink(), null);
            } catch (DocumentClientException e) {
                e.printStackTrace();
                return false;
            }
   
            return true;
        }

## <span data-ttu-id="68780-193"><a id="Wire"></a>Steg 5: Sammankoppla hello resten av hello av Java programutvecklingsprojekt tillsammans</span><span class="sxs-lookup"><span data-stu-id="68780-193"><a id="Wire"></a>Step 5: Wiring hello rest of hello of Java application development project together</span></span>
<span data-ttu-id="68780-194">Nu när vi har testat färdigt hello roliga bits - återstår är toobuild ett snabbt användargränssnitt och ansluta det upp tooour DAO.</span><span class="sxs-lookup"><span data-stu-id="68780-194">Now that we've finished hello fun bits - all that's left is toobuild a quick user interface and wire it up tooour DAO.</span></span>

1. <span data-ttu-id="68780-195">Först måste börja med att skapa en domänkontrollant toocall vår DAO:</span><span class="sxs-lookup"><span data-stu-id="68780-195">First, let's start with building a controller toocall our DAO:</span></span>
   
        public class TodoItemController {
            public static TodoItemController getInstance() {
                if (todoItemController == null) {
                    todoItemController = new TodoItemController(TodoDaoFactory.getDao());
                }
                return todoItemController;
            }
   
            private static TodoItemController todoItemController;
   
            private final TodoDao todoDao;
   
            TodoItemController(TodoDao todoDao) {
                this.todoDao = todoDao;
            }
   
            public TodoItem createTodoItem(@NonNull String name,
                    @NonNull String category, boolean isComplete) {
                TodoItem todoItem = TodoItem.builder().name(name).category(category)
                        .complete(isComplete).build();
                return todoDao.createTodoItem(todoItem);
            }
   
            public boolean deleteTodoItem(@NonNull String id) {
                return todoDao.deleteTodoItem(id);
            }
   
            public TodoItem getTodoItemById(@NonNull String id) {
                return todoDao.readTodoItem(id);
            }
   
            public List<TodoItem> getTodoItems() {
                return todoDao.readTodoItems();
            }
   
            public TodoItem updateTodoItem(@NonNull String id, boolean isComplete) {
                return todoDao.updateTodoItem(id, isComplete);
            }
        }
   
    <span data-ttu-id="68780-196">I ett mer komplext program kanske hello styrningen innehåller komplicerad affärslogik utöver hello DAO.</span><span class="sxs-lookup"><span data-stu-id="68780-196">In a more complex application, hello controller may house complicated business logic on top of hello DAO.</span></span>
2. <span data-ttu-id="68780-197">Därefter skapar vi ett servlet tooroute HTTP-begäranden toohello domänkontrollant:</span><span class="sxs-lookup"><span data-stu-id="68780-197">Next, we'll create a servlet tooroute HTTP requests toohello controller:</span></span>
   
        public class TodoServlet extends HttpServlet {
            // API Keys
            public static final String API_METHOD = "method";
   
            // API Methods
            public static final String CREATE_TODO_ITEM = "createTodoItem";
            public static final String GET_TODO_ITEMS = "getTodoItems";
            public static final String UPDATE_TODO_ITEM = "updateTodoItem";
   
            // API Parameters
            public static final String TODO_ITEM_ID = "todoItemId";
            public static final String TODO_ITEM_NAME = "todoItemName";
            public static final String TODO_ITEM_CATEGORY = "todoItemCategory";
            public static final String TODO_ITEM_COMPLETE = "todoItemComplete";
   
            public static final String MESSAGE_ERROR_INVALID_METHOD = "{'error': 'Invalid method'}";
   
            private static final long serialVersionUID = 1L;
            private static final Gson gson = new Gson();
   
            @Override
            protected void doGet(HttpServletRequest request,
                    HttpServletResponse response) throws ServletException, IOException {
   
                String apiResponse = MESSAGE_ERROR_INVALID_METHOD;
   
                TodoItemController todoItemController = TodoItemController
                        .getInstance();
   
                String id = request.getParameter(TODO_ITEM_ID);
                String name = request.getParameter(TODO_ITEM_NAME);
                String category = request.getParameter(TODO_ITEM_CATEGORY);
                boolean isComplete = StringUtils.equalsIgnoreCase("true",
                        request.getParameter(TODO_ITEM_COMPLETE)) ? true : false;
   
                switch (request.getParameter(API_METHOD)) {
                case CREATE_TODO_ITEM:
                    apiResponse = gson.toJson(todoItemController.createTodoItem(name,
                            category, isComplete));
                    break;
                case GET_TODO_ITEMS:
                    apiResponse = gson.toJson(todoItemController.getTodoItems());
                    break;
                case UPDATE_TODO_ITEM:
                    apiResponse = gson.toJson(todoItemController.updateTodoItem(id,
                            isComplete));
                    break;
                default:
                    break;
                }
   
                response.getWriter().println(apiResponse);
            }
   
            @Override
            protected void doPost(HttpServletRequest request,
                    HttpServletResponse response) throws ServletException, IOException {
                doGet(request, response);
            }
        }
3. <span data-ttu-id="68780-198">Vi behöver en web interface toodisplay toohello användare.</span><span class="sxs-lookup"><span data-stu-id="68780-198">We'll need a web user interface toodisplay toohello user.</span></span> <span data-ttu-id="68780-199">Vi skriver hello index.jsp vi skapade tidigare:</span><span class="sxs-lookup"><span data-stu-id="68780-199">Let's re-write hello index.jsp we created earlier:</span></span>
    ```html
        <html>
        <head>
          <meta http-equiv="Content-Type" content="text/html; charset=ISO-8859-1">
          <meta http-equiv="X-UA-Compatible" content="IE=edge;" />
          <title>Azure Cosmos DB Java Sample</title>
   
          <!-- Bootstrap -->
          <link href="//ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap.min.css" rel="stylesheet">
   
          <style>
            /* Add padding toobody for fixed nav bar */
            body {
              padding-top: 50px;
            }
          </style>
        </head>
        <body>
          <!-- Nav Bar -->
          <div class="navbar navbar-inverse navbar-fixed-top" role="navigation">
            <div class="container">
              <div class="navbar-header">
                <a class="navbar-brand" href="#">My Tasks</a>
              </div>
            </div>
          </div>
   
          <!-- Body -->
          <div class="container">
            <h1>My ToDo List</h1>
   
            <hr/>
   
            <!-- hello ToDo List -->
            <div class = "todoList">
              <table class="table table-bordered table-striped" id="todoItems">
                <thead>
                  <tr>
                    <th>Name</th>
                    <th>Category</th>
                    <th>Complete</th>
                  </tr>
                </thead>
                <tbody>
                </tbody>
              </table>
   
              <!-- Update Button -->
              <div class="todoUpdatePanel">
                <form class="form-horizontal" role="form">
                  <button type="button" class="btn btn-primary">Update Tasks</button>
                </form>
              </div>
   
            </div>
   
            <hr/>
   
            <!-- Item Input Form -->
            <div class="todoForm">
              <form class="form-horizontal" role="form">
                <div class="form-group">
                  <label for="inputItemName" class="col-sm-2">Task Name</label>
                  <div class="col-sm-10">
                    <input type="text" class="form-control" id="inputItemName" placeholder="Enter name">
                  </div>
                </div>
   
                <div class="form-group">
                  <label for="inputItemCategory" class="col-sm-2">Task Category</label>
                  <div class="col-sm-10">
                    <input type="text" class="form-control" id="inputItemCategory" placeholder="Enter category">
                  </div>
                </div>
   
                <button type="button" class="btn btn-primary">Add Task</button>
              </form>
            </div>
   
          </div>
   
          <!-- Placed at hello end of hello document so hello pages load faster -->
          <script src="//ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.1.min.js"></script>
          <script src="//ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/bootstrap.min.js"></script>
          <script src="assets/todo.js"></script>
        </body>
        </html>
    ```
4. <span data-ttu-id="68780-200">Och slutligen skriva vissa klientens JavaScript tootie hello webbaserat användargränssnitt och hello servlet tillsammans:</span><span class="sxs-lookup"><span data-stu-id="68780-200">And finally, write some client-side JavaScript tootie hello web user interface and hello servlet together:</span></span>
   
        var todoApp = {
          /*
           * API methods toocall Java backend.
           */
          apiEndpoint: "api",
   
          createTodoItem: function(name, category, isComplete) {
            $.post(todoApp.apiEndpoint, {
                "method": "createTodoItem",
                "todoItemName": name,
                "todoItemCategory": category,
                "todoItemComplete": isComplete
              },
              function(data) {
                var todoItem = data;
                todoApp.addTodoItemToTable(todoItem.id, todoItem.name, todoItem.category, todoItem.complete);
              },
              "json");
          },
   
          getTodoItems: function() {
            $.post(todoApp.apiEndpoint, {
                "method": "getTodoItems"
              },
              function(data) {
                var todoItemArr = data;
                $.each(todoItemArr, function(index, value) {
                  todoApp.addTodoItemToTable(value.id, value.name, value.category, value.complete);
                });
              },
              "json");
          },
   
          updateTodoItem: function(id, isComplete) {
            $.post(todoApp.apiEndpoint, {
                "method": "updateTodoItem",
                "todoItemId": id,
                "todoItemComplete": isComplete
              },
              function(data) {},
              "json");
          },
   
          /*
           * UI Methods
           */
          addTodoItemToTable: function(id, name, category, isComplete) {
            var rowColor = isComplete ? "active" : "warning";
   
            todoApp.ui_table().append($("<tr>")
              .append($("<td>").text(name))
              .append($("<td>").text(category))
              .append($("<td>")
                .append($("<input>")
                  .attr("type", "checkbox")
                  .attr("id", id)
                  .attr("checked", isComplete)
                  .attr("class", "isComplete")
                ))
              .addClass(rowColor)
            );
          },
   
          /*
           * UI Bindings
           */
          bindCreateButton: function() {
            todoApp.ui_createButton().click(function() {
              todoApp.createTodoItem(todoApp.ui_createNameInput().val(), todoApp.ui_createCategoryInput().val(), false);
              todoApp.ui_createNameInput().val("");
              todoApp.ui_createCategoryInput().val("");
            });
          },
   
          bindUpdateButton: function() {
            todoApp.ui_updateButton().click(function() {
              // Disable button temporarily.
              var myButton = $(this);
              var originalText = myButton.text();
              $(this).text("Updating...");
              $(this).prop("disabled", true);
   
              // Call api tooupdate todo items.
              $.each(todoApp.ui_updateId(), function(index, value) {
                todoApp.updateTodoItem(value.name, value.value);
                $(value).remove();
              });
   
              // Re-enable button.
              setTimeout(function() {
                myButton.prop("disabled", false);
                myButton.text(originalText);
              }, 500);
            });
          },
   
          bindUpdateCheckboxes: function() {
            todoApp.ui_table().on("click", ".isComplete", function(event) {
              var checkboxElement = $(event.currentTarget);
              var rowElement = $(event.currentTarget).parents('tr');
              var id = checkboxElement.attr('id');
              var isComplete = checkboxElement.is(':checked');
   
              // Toggle table row color
              if (isComplete) {
                rowElement.addClass("active");
                rowElement.removeClass("warning");
              } else {
                rowElement.removeClass("active");
                rowElement.addClass("warning");
              }
   
              // Update hidden inputs for update panel.
              todoApp.ui_updateForm().children("input[name='" + id + "']").remove();
   
              todoApp.ui_updateForm().append($("<input>")
                .attr("type", "hidden")
                .attr("class", "updateComplete")
                .attr("name", id)
                .attr("value", isComplete));
   
            });
          },
   
          /*
           * UI Elements
           */
          ui_createNameInput: function() {
            return $(".todoForm #inputItemName");
          },
   
          ui_createCategoryInput: function() {
            return $(".todoForm #inputItemCategory");
          },
   
          ui_createButton: function() {
            return $(".todoForm button");
          },
   
          ui_table: function() {
            return $(".todoList table tbody");
          },
   
          ui_updateButton: function() {
            return $(".todoUpdatePanel button");
          },
   
          ui_updateForm: function() {
            return $(".todoUpdatePanel form");
          },
   
          ui_updateId: function() {
            return $(".todoUpdatePanel .updateComplete");
          },
   
          /*
           * Install hello TodoApp
           */
          install: function() {
            todoApp.bindCreateButton();
            todoApp.bindUpdateButton();
            todoApp.bindUpdateCheckboxes();
   
            todoApp.getTodoItems();
          }
        };
   
        $(document).ready(function() {
          todoApp.install();
        });
5. <span data-ttu-id="68780-201">Fantastiskt!</span><span class="sxs-lookup"><span data-stu-id="68780-201">Awesome!</span></span> <span data-ttu-id="68780-202">Nu är återstår tootest hello program.</span><span class="sxs-lookup"><span data-stu-id="68780-202">Now all that's left is tootest hello application.</span></span> <span data-ttu-id="68780-203">Kör hello programmet lokalt och lägga till några Todo-objekt genom att fylla i hello objektnamn och kategori och klicka på **Lägg till aktivitet**.</span><span class="sxs-lookup"><span data-stu-id="68780-203">Run hello application locally, and add some Todo items by filling in hello item name and category and clicking **Add Task**.</span></span>
6. <span data-ttu-id="68780-204">När hello objektet visas kan du uppdatera om aktiviteten är slutförd genom växla hello kryssrutan och klicka på **uppdatera aktiviteter**.</span><span class="sxs-lookup"><span data-stu-id="68780-204">Once hello item appears, you can update whether it's complete by toggling hello checkbox and clicking **Update Tasks**.</span></span>

## <span data-ttu-id="68780-205"><a id="Deploy"></a>Steg 6: Distribuera webbplatser för tooAzure din Java-program</span><span class="sxs-lookup"><span data-stu-id="68780-205"><a id="Deploy"></a>Step 6: Deploy your Java application tooAzure Web Sites</span></span>
<span data-ttu-id="68780-206">Azure webbplatser kan du distribuera Java-program bara exportera appen som en WAR-fil och antingen ladda upp den via källkontrollen (t.ex. Git) eller FTP.</span><span class="sxs-lookup"><span data-stu-id="68780-206">Azure Web Sites makes deploying Java applications as simple as exporting your application as a WAR file and either uploading it via source control (e.g. Git) or FTP.</span></span>

1. <span data-ttu-id="68780-207">tooexport ditt program som en WAR-fil, högerklicka på projektet i **Projektutforskaren**, klickar du på **exportera**, och klicka sedan på **WAR-filen**.</span><span class="sxs-lookup"><span data-stu-id="68780-207">tooexport your application as a WAR file, right-click on your project in **Project Explorer**, click **Export**, and then click **WAR File**.</span></span>
2. <span data-ttu-id="68780-208">I hello **WAR-Export** fönstret hello följande:</span><span class="sxs-lookup"><span data-stu-id="68780-208">In hello **WAR Export** window, do hello following:</span></span>
   
   * <span data-ttu-id="68780-209">Ange azure-documentdb-java-sample i hello Web project.</span><span class="sxs-lookup"><span data-stu-id="68780-209">In hello Web project box, enter azure-documentdb-java-sample.</span></span>
   * <span data-ttu-id="68780-210">Välj ett mål toosave hello WAR-filen hello mål i rutan.</span><span class="sxs-lookup"><span data-stu-id="68780-210">In hello Destination box, choose a destination toosave hello WAR file.</span></span>
   * <span data-ttu-id="68780-211">Klicka på **Slutför**.</span><span class="sxs-lookup"><span data-stu-id="68780-211">Click **Finish**.</span></span>
3. <span data-ttu-id="68780-212">Nu när du har en WAR-fil i hand, du kan bara ladda upp den tooyour webbplatsen Azure **webbappar** directory.</span><span class="sxs-lookup"><span data-stu-id="68780-212">Now that you have a WAR file in hand, you can simply upload it tooyour Azure Web Site's **webapps** directory.</span></span> <span data-ttu-id="68780-213">Anvisningar för att ladda upp hello-fil finns [lägga till en Java-program tooAzure App Service Web Apps](../app-service-web/web-sites-java-add-app.md).</span><span class="sxs-lookup"><span data-stu-id="68780-213">For instructions on uploading hello file, see [Add a Java application tooAzure App Service Web Apps](../app-service-web/web-sites-java-add-app.md).</span></span>
   
    <span data-ttu-id="68780-214">När hello WAR-filen är överförda toohello webbappar katalog, identifierar hello körningsmiljön att du har lagt till den och kommer automatiskt att läsa in den.</span><span class="sxs-lookup"><span data-stu-id="68780-214">Once hello WAR file is uploaded toohello webapps directory, hello runtime environment will detect that you've added it and will automatically load it.</span></span>
4. <span data-ttu-id="68780-215">tooview din färdiga produkt navigera toohttp://YOUR\_plats\_NAME.azurewebsites.net/azure-java-sample/ och börja lägga till aktiviteter!</span><span class="sxs-lookup"><span data-stu-id="68780-215">tooview your finished product, navigate toohttp://YOUR\_SITE\_NAME.azurewebsites.net/azure-java-sample/ and start adding your tasks!</span></span>

## <span data-ttu-id="68780-216"><a id="GetProject"></a>Hämta hello projektet från GitHub</span><span class="sxs-lookup"><span data-stu-id="68780-216"><a id="GetProject"></a>Get hello project from GitHub</span></span>
<span data-ttu-id="68780-217">Alla hello prover i den här kursen ingår i hello [todo](https://github.com/Azure-Samples/documentdb-java-todo-app) projektet på GitHub.</span><span class="sxs-lookup"><span data-stu-id="68780-217">All hello samples in this tutorial are included in hello [todo](https://github.com/Azure-Samples/documentdb-java-todo-app) project on GitHub.</span></span> <span data-ttu-id="68780-218">tooimport hello todo-projektet till Eclipse, se till att du har hello programvara och resurser som anges i hello [krav](#Prerequisites) och sedan hello följande:</span><span class="sxs-lookup"><span data-stu-id="68780-218">tooimport hello todo project into Eclipse, ensure you have hello software and resources listed in hello [Prerequisites](#Prerequisites) section, then do hello following:</span></span>

1. <span data-ttu-id="68780-219">Installera [Project Lombok](http://projectlombok.org/).</span><span class="sxs-lookup"><span data-stu-id="68780-219">Install [Project Lombok](http://projectlombok.org/).</span></span> <span data-ttu-id="68780-220">Lombok är används toogenerate konstruktorer, get och set-metoder i hello-projekt.</span><span class="sxs-lookup"><span data-stu-id="68780-220">Lombok is used toogenerate constructors, getters, setters in hello project.</span></span> <span data-ttu-id="68780-221">När du har hämtat hello lombok.jar filen, dubbelklicka på den tooinstall den eller installera det från hello-kommandoraden.</span><span class="sxs-lookup"><span data-stu-id="68780-221">Once you have downloaded hello lombok.jar file, double-click it tooinstall it or install it from hello command line.</span></span>
2. <span data-ttu-id="68780-222">Om Eclipse är öppen, Stäng och starta om den tooload Lombok.</span><span class="sxs-lookup"><span data-stu-id="68780-222">If Eclipse is open, close it and restart it tooload Lombok.</span></span>
3. <span data-ttu-id="68780-223">I Eclipse på hello **filen** -menyn klickar du på **importera**.</span><span class="sxs-lookup"><span data-stu-id="68780-223">In Eclipse, on hello **File** menu, click **Import**.</span></span>
4. <span data-ttu-id="68780-224">I hello **importera** -fönstret klickar du på **Git**, klickar du på **projekt från Git**, och klicka sedan på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="68780-224">In hello **Import** window, click **Git**, click **Projects from Git**, and then click **Next**.</span></span>
5. <span data-ttu-id="68780-225">På hello **Välj Lagerkälla** klickar du på **klona URI**.</span><span class="sxs-lookup"><span data-stu-id="68780-225">On hello **Select Repository Source** screen, click **Clone URI**.</span></span>
6. <span data-ttu-id="68780-226">På hello **Git-Lagerkälla** i hello skärmen **URI** rutan Ange https://github.com/Azure-Samples/java-todo-app.git och klicka sedan på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="68780-226">On hello **Source Git Repository** screen, in hello **URI** box, enter https://github.com/Azure-Samples/java-todo-app.git, and then click **Next**.</span></span>
7. <span data-ttu-id="68780-227">På hello **val av gren** kontrollerar du att **master** är markerad och klicka sedan på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="68780-227">On hello **Branch Selection** screen, ensure that **master** is selected, and then click **Next**.</span></span>
8. <span data-ttu-id="68780-228">På hello **lokal Destination** klickar du på **Bläddra** tooselect en mapp där hello databasen kan kopieras och klickar sedan på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="68780-228">On hello **Local Destination** screen, click **Browse** tooselect a folder where hello repository can be copied, and then click **Next**.</span></span>
9. <span data-ttu-id="68780-229">På hello **väljer en toouse guide för importprojekt** kontrollerar du att **importera befintliga projekt** är markerad och klicka sedan på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="68780-229">On hello **Select a wizard toouse for importing projects** screen, ensure that **Import existing projects** is selected, and then click **Next**.</span></span>
10. <span data-ttu-id="68780-230">På hello **Importera projekt** skärmen, avmarkera hello **DocumentDB** projektet och klicka sedan på **Slutför**.</span><span class="sxs-lookup"><span data-stu-id="68780-230">On hello **Import Projects** screen, unselect hello **DocumentDB** project, and then click **Finish**.</span></span> <span data-ttu-id="68780-231">Hej DocumentDB-projektet innehåller hello Azure Cosmos DB Java SDK, som vi ska lägga till som beroende i stället.</span><span class="sxs-lookup"><span data-stu-id="68780-231">hello DocumentDB project contains hello Azure Cosmos DB Java SDK, which we will add as a dependency instead.</span></span>
11. <span data-ttu-id="68780-232">I **Projektutforskaren**, navigera tooazure-documentdb-java-sample\src\com.microsoft.azure.documentdb.sample.dao\DocumentClientFactory.java och Ersätt hello värd och huvudnyckel värden med hello URI och PRIMÄRNYCKEL för din Azure DB som Cosmos-kontot och sedan spara hello-fil.</span><span class="sxs-lookup"><span data-stu-id="68780-232">In **Project Explorer**, navigate tooazure-documentdb-java-sample\src\com.microsoft.azure.documentdb.sample.dao\DocumentClientFactory.java and replace hello HOST and MASTER_KEY values with hello URI and PRIMARY KEY for your Azure Cosmos DB account, and then save hello file.</span></span> <span data-ttu-id="68780-233">Mer information finns [steg 1. Skapa ett databaskonto Azure Cosmos DB](#CreateDB).</span><span class="sxs-lookup"><span data-stu-id="68780-233">For more information, see [Step 1. Create an Azure Cosmos DB database account](#CreateDB).</span></span>
12. <span data-ttu-id="68780-234">I **Projektutforskaren**, högerklicka på hello **azure-documentdb-java-sample**, klickar du på **byggsökväg**, och klicka sedan på **konfigurera byggsökväg**.</span><span class="sxs-lookup"><span data-stu-id="68780-234">In **Project Explorer**, right click hello **azure-documentdb-java-sample**, click **Build Path**, and then click **Configure Build Path**.</span></span>
13. <span data-ttu-id="68780-235">På hello **Java byggsökväg** skärmen i hello högra rutan, Välj hello **bibliotek** fliken och klicka sedan på **lägga till externa JAR: er**.</span><span class="sxs-lookup"><span data-stu-id="68780-235">On hello **Java Build Path** screen, in hello right pane, select hello **Libraries** tab, and then click **Add External JARs**.</span></span> <span data-ttu-id="68780-236">Navigera toohello platsen för hello lombok.jar-filen och klicka på **öppna**, och klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="68780-236">Navigate toohello location of hello lombok.jar file, and click **Open**, and then click **OK**.</span></span>
14. <span data-ttu-id="68780-237">Använd steg 12 tooopen hello **egenskaper** fönstret igen, och klicka sedan på hello vänster **Körningsmål**.</span><span class="sxs-lookup"><span data-stu-id="68780-237">Use step 12 tooopen hello **Properties** window again, and then in hello left pane click **Targeted Runtimes**.</span></span>
15. <span data-ttu-id="68780-238">På hello **Körningsmål** klickar du på **ny**väljer **Apache Tomcat v7.0**, och klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="68780-238">On hello **Targeted Runtimes** screen, click **New**, select **Apache Tomcat v7.0**, and then click **OK**.</span></span>
16. <span data-ttu-id="68780-239">Använd steg 12 tooopen hello **egenskaper** fönstret igen, och klicka sedan på hello vänster **Projektfasetter**.</span><span class="sxs-lookup"><span data-stu-id="68780-239">Use step 12 tooopen hello **Properties** window again, and then in hello left pane click **Project Facets**.</span></span>
17. <span data-ttu-id="68780-240">På hello **Projektfasetter** väljer **dynamisk webbmodul** och **Java**, och klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="68780-240">On hello **Project Facets** screen, select **Dynamic Web Module** and **Java**, and then click **OK**.</span></span>
18. <span data-ttu-id="68780-241">På hello **servrar** längst hello hello-skärmen, högerklickar på **Tomcat v7.0-Server på localhost** och klicka sedan på **lägga till och ta bort**.</span><span class="sxs-lookup"><span data-stu-id="68780-241">On hello **Servers** tab at hello bottom of hello screen, right-click **Tomcat v7.0 Server at localhost** and then click **Add and Remove**.</span></span>
19. <span data-ttu-id="68780-242">På hello **lägga till och ta bort** fönstret flytta **azure-documentdb-java-sample** toohello **konfigurerad** rutan och klicka på **Slutför**.</span><span class="sxs-lookup"><span data-stu-id="68780-242">On hello **Add and Remove** window, move **azure-documentdb-java-sample** toohello **Configured** box, and then click **Finish**.</span></span>
20. <span data-ttu-id="68780-243">I hello **servrar** genom att högerklicka på **Tomcat v7.0-Server på localhost**, och klicka sedan på **starta om**.</span><span class="sxs-lookup"><span data-stu-id="68780-243">In hello **Servers** tab, right-click **Tomcat v7.0 Server at localhost**, and then click **Restart**.</span></span>
21. <span data-ttu-id="68780-244">I en webbläsare, navigera toohttp://localhost:8080 / azure-documentdb-java-sample / och börja lägga till tooyour uppgiftslista.</span><span class="sxs-lookup"><span data-stu-id="68780-244">In a browser, navigate toohttp://localhost:8080/azure-documentdb-java-sample/ and start adding tooyour task list.</span></span> <span data-ttu-id="68780-245">Observera att om du har ändrat portarnas standardvärden, ändrar du 8080 toohello-värdet som du har valt.</span><span class="sxs-lookup"><span data-stu-id="68780-245">Note that if you changed your default port values, change 8080 toohello value you selected.</span></span>
22. <span data-ttu-id="68780-246">toodeploy ditt projekt tooan Azure-webbplats, se [steg 6. Distribuera ditt program tooAzure webbplatser](#Deploy).</span><span class="sxs-lookup"><span data-stu-id="68780-246">toodeploy your project tooan Azure web site, see [Step 6. Deploy your application tooAzure Web Sites](#Deploy).</span></span>

[1]: media/documentdb-java-application/keys.png
