---
title: 'Azure Cosmos DB: Utveckla med hello Graph API i .NET | Microsoft Docs'
description: "Lär dig hur toodevelop med Azure Cosmos DB DocumentDB-API med hjälp av .NET"
services: cosmos-db
documentationcenter: 
author: dennyglee
manager: jhubbard
editor: 
ms.assetid: cc8df0be-672b-493e-95a4-26dd52632261
ms.service: cosmos-db
ms.workload: 
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 05/10/2017
ms.author: denlee
ms.custom: mvc
ms.openlocfilehash: 12e435d8169aeee6e818dac4a3b66c7a0ec5f2d5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-cosmos-db-develop-with-hello-graph-api-in-net"></a><span data-ttu-id="6373a-103">Azure Cosmos DB: Utveckla med hello Graph API i .NET</span><span class="sxs-lookup"><span data-stu-id="6373a-103">Azure Cosmos DB: Develop with hello Graph API in .NET</span></span>
<span data-ttu-id="6373a-104">Azure Cosmos-DB är Microsofts globalt distribuerade flera modellen database-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="6373a-104">Azure Cosmos DB is Microsoft's globally distributed multi-model database service.</span></span> <span data-ttu-id="6373a-105">Du kan snabbt skapa och fråga dokument och nyckel/värde-diagrammet databaser, som omfattas av hello global distributionsplatsen och skala horisontellt funktionerna i hello kärnan i Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="6373a-105">You can quickly create and query document, key/value, and graph databases, all of which benefit from hello global distribution and horizontal scale capabilities at hello core of Azure Cosmos DB.</span></span> 

<span data-ttu-id="6373a-106">Den här kursen visar hur toocreate ett Azure DB som Cosmos-kontot med hello Azure-portalen och toocreate ett diagram databas och en behållare.</span><span class="sxs-lookup"><span data-stu-id="6373a-106">This tutorial demonstrates how toocreate an Azure Cosmos DB account using hello Azure portal and how toocreate a graph database and container.</span></span> <span data-ttu-id="6373a-107">sedan hello program skapar ett enkelt sociala nätverk med fyra personer som använder hello [Graph API](graph-sdk-dotnet.md) (förhandsversion) och sedan passerar och frågar hello diagram med hjälp av Gremlin.</span><span class="sxs-lookup"><span data-stu-id="6373a-107">hello application then creates a simple social network with four people using hello [Graph API](graph-sdk-dotnet.md) (preview), then traverses and queries hello graph using Gremlin.</span></span>

<span data-ttu-id="6373a-108">Den här kursen ingår hello följande uppgifter:</span><span class="sxs-lookup"><span data-stu-id="6373a-108">This tutorial covers hello following tasks:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="6373a-109">Skapa ett Azure Cosmos DB-konto</span><span class="sxs-lookup"><span data-stu-id="6373a-109">Create an Azure Cosmos DB account</span></span> 
> * <span data-ttu-id="6373a-110">Skapa ett diagram databas och en behållare</span><span class="sxs-lookup"><span data-stu-id="6373a-110">Create a graph database and container</span></span>
> * <span data-ttu-id="6373a-111">Serialisera hörn och kanter too.NET objekt</span><span class="sxs-lookup"><span data-stu-id="6373a-111">Serialize vertices and edges too.NET objects</span></span>
> * <span data-ttu-id="6373a-112">Lägg till hörn och kanter</span><span class="sxs-lookup"><span data-stu-id="6373a-112">Add vertices and edges</span></span>
> * <span data-ttu-id="6373a-113">Frågan hello diagram med hjälp av Gremlin</span><span class="sxs-lookup"><span data-stu-id="6373a-113">Query hello graph using Gremlin</span></span>

## <a name="graphs-in-azure-cosmos-db"></a><span data-ttu-id="6373a-114">Diagram i Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="6373a-114">Graphs in Azure Cosmos DB</span></span>
<span data-ttu-id="6373a-115">Du kan använda Azure Cosmos DB toocreate-, update- och fråga diagram med hjälp av hello [Microsoft.Azure.Graphs](graph-sdk-dotnet.md) bibliotek.</span><span class="sxs-lookup"><span data-stu-id="6373a-115">You can use Azure Cosmos DB toocreate, update, and query graphs using hello [Microsoft.Azure.Graphs](graph-sdk-dotnet.md) library.</span></span> <span data-ttu-id="6373a-116">Hej Microsoft.Azure.Graph-bibliotek innehåller en enda metod `CreateGremlinQuery<T>` ovanpå hello `DocumentClient` klassen tooexecute Gremlin frågor.</span><span class="sxs-lookup"><span data-stu-id="6373a-116">hello Microsoft.Azure.Graph library provides a single extension method `CreateGremlinQuery<T>` on top of hello `DocumentClient` class tooexecute Gremlin queries.</span></span>

<span data-ttu-id="6373a-117">Gremlin är en funktionell programmeringsspråk som stöder skriva åtgärder (DML) och fråga och traversal-åtgärder.</span><span class="sxs-lookup"><span data-stu-id="6373a-117">Gremlin is a functional programming language that supports write operations (DML) and query and traversal operations.</span></span> <span data-ttu-id="6373a-118">Vi igenom några exempel i den här artikeln tooget din igång med Gremlin.</span><span class="sxs-lookup"><span data-stu-id="6373a-118">We cover a few examples in this article tooget your started with Gremlin.</span></span> <span data-ttu-id="6373a-119">Se [Gremlin frågor](gremlin-support.md) en detaljerad genomgång av Gremlin av funktionerna i Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="6373a-119">See [Gremlin queries](gremlin-support.md) for a detailed walkthrough of Gremlin capabilities available in Azure Cosmos DB.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="6373a-120">Krav</span><span class="sxs-lookup"><span data-stu-id="6373a-120">Prerequisites</span></span>
<span data-ttu-id="6373a-121">Kontrollera att du har hello följande:</span><span class="sxs-lookup"><span data-stu-id="6373a-121">Please make sure you have hello following:</span></span>

* <span data-ttu-id="6373a-122">Ett aktivt Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="6373a-122">An active Azure account.</span></span> <span data-ttu-id="6373a-123">Om du inte har ett kan du registrera dig för ett [kostnadsfritt konto](https://azure.microsoft.com/free/).</span><span class="sxs-lookup"><span data-stu-id="6373a-123">If you don't have one, you can sign up for a [free account](https://azure.microsoft.com/free/).</span></span> 
    * <span data-ttu-id="6373a-124">Du kan också använda hello [Azure DocumentDB-emulatorn](local-emulator.md) för den här självstudiekursen.</span><span class="sxs-lookup"><span data-stu-id="6373a-124">Alternatively, you can use hello [Azure DocumentDB Emulator](local-emulator.md) for this tutorial.</span></span>
* <span data-ttu-id="6373a-125">[Visual Studio](http://www.visualstudio.com/).</span><span class="sxs-lookup"><span data-stu-id="6373a-125">[Visual Studio](http://www.visualstudio.com/).</span></span>

## <a name="create-database-account"></a><span data-ttu-id="6373a-126">Skapa konto</span><span class="sxs-lookup"><span data-stu-id="6373a-126">Create database account</span></span>

<span data-ttu-id="6373a-127">Börja med att skapa ett Azure DB som Cosmos-konto i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="6373a-127">Let's start by creating an Azure Cosmos DB account in hello Azure portal.</span></span>  

> [!TIP]
> * <span data-ttu-id="6373a-128">Redan har ett Azure DB som Cosmos-konto?</span><span class="sxs-lookup"><span data-stu-id="6373a-128">Already have an Azure Cosmos DB account?</span></span> <span data-ttu-id="6373a-129">I så fall, kan du gå vidare för[konfigurera Visual Studio-lösning](#SetupVS)</span><span class="sxs-lookup"><span data-stu-id="6373a-129">If so, skip ahead too[Set up your Visual Studio solution](#SetupVS)</span></span>
> * <span data-ttu-id="6373a-130">Hade du ett Azure DocumentDB-konto?</span><span class="sxs-lookup"><span data-stu-id="6373a-130">Did you have an Azure DocumentDB account?</span></span> <span data-ttu-id="6373a-131">Om ditt konto är nu en Azure DB som Cosmos-konto och du gå vidare för så[konfigurera Visual Studio-lösning](#SetupVS).</span><span class="sxs-lookup"><span data-stu-id="6373a-131">If so, your account is now an Azure Cosmos DB account and you can skip ahead too[Set up your Visual Studio solution](#SetupVS).</span></span>  
> * <span data-ttu-id="6373a-132">Om du använder hello Azure Cosmos DB-emulatorn, följ hello stegen i [Azure Cosmos DB emulatorn](local-emulator.md) toosetup hello emulatorn och gå vidare för[ställa in din Visual Studio-lösning](#SetupVS).</span><span class="sxs-lookup"><span data-stu-id="6373a-132">If you are using hello Azure Cosmos DB Emulator, please follow hello steps at [Azure Cosmos DB Emulator](local-emulator.md) toosetup hello emulator and skip ahead too[Set up your Visual Studio Solution](#SetupVS).</span></span> 
>
> 

[!INCLUDE [cosmos-db-create-dbaccount-graph](../../includes/cosmos-db-create-dbaccount-graph.md)]

## <span data-ttu-id="6373a-133"><a id="SetupVS"></a>Konfigurera din Visual Studio-lösning</span><span class="sxs-lookup"><span data-stu-id="6373a-133"><a id="SetupVS"></a>Set up your Visual Studio solution</span></span>
1. <span data-ttu-id="6373a-134">Öppna **Visual Studio** på datorn.</span><span class="sxs-lookup"><span data-stu-id="6373a-134">Open **Visual Studio** on your computer.</span></span>
2. <span data-ttu-id="6373a-135">På hello **filen** väljer du **ny**, och välj sedan **projekt**.</span><span class="sxs-lookup"><span data-stu-id="6373a-135">On hello **File** menu, select **New**, and then choose **Project**.</span></span>
3. <span data-ttu-id="6373a-136">I hello **nytt projekt** markerar **mallar** / **Visual C#** / **Konsolapp (.NET Framework)**namnge projektet och klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="6373a-136">In hello **New Project** dialog, select **Templates** / **Visual C#** / **Console App (.NET Framework)**, name your project, and then click **OK**.</span></span>
4. <span data-ttu-id="6373a-137">I hello **Solution Explorer**, högerklicka på den nya konsolappen, som är under din Visual Studio-lösning, och klicka sedan på **hantera NuGet-paket...**</span><span class="sxs-lookup"><span data-stu-id="6373a-137">In hello **Solution Explorer**, right click on your new console application, which is under your Visual Studio solution, and then click **Manage NuGet Packages...**</span></span>
5. <span data-ttu-id="6373a-138">I hello **NuGet** klickar du på **Bläddra**, och skriv **Microsoft.Azure.Graphs** i sökrutan hello och kontrollera hello **inkluderar förhandsversioner**.</span><span class="sxs-lookup"><span data-stu-id="6373a-138">In hello **NuGet** tab, click **Browse**, and type **Microsoft.Azure.Graphs** in hello search box, and check hello **Include prerelease versions**.</span></span>
6. <span data-ttu-id="6373a-139">I hello resultat hitta **Microsoft.Azure.Graphs** och på **installera**.</span><span class="sxs-lookup"><span data-stu-id="6373a-139">Within hello results, find **Microsoft.Azure.Graphs** and click **Install**.</span></span>
   
   <span data-ttu-id="6373a-140">Om du får ett meddelande om att granska ändringar toohello lösning, klickar du på **OK**.</span><span class="sxs-lookup"><span data-stu-id="6373a-140">If you get a message about reviewing changes toohello solution, click **OK**.</span></span> <span data-ttu-id="6373a-141">Om du får ett meddelande om godkännande av licens klickar du på **Jag godkänner**.</span><span class="sxs-lookup"><span data-stu-id="6373a-141">If you get a message about license acceptance, click **I accept**.</span></span>
   
    <span data-ttu-id="6373a-142">Hej `Microsoft.Azure.Graphs` -bibliotek innehåller en enda metod `CreateGremlinQuery<T>` för att köra Gremlin åtgärder.</span><span class="sxs-lookup"><span data-stu-id="6373a-142">hello `Microsoft.Azure.Graphs` library provides a single extension method `CreateGremlinQuery<T>` for executing Gremlin operations.</span></span> <span data-ttu-id="6373a-143">Gremlin är en funktionell programmeringsspråk som stöder skriva åtgärder (DML) och fråga och traversal-åtgärder.</span><span class="sxs-lookup"><span data-stu-id="6373a-143">Gremlin is a functional programming language that supports write operations (DML) and query and traversal operations.</span></span> <span data-ttu-id="6373a-144">Vi igenom några exempel i den här artikeln tooget din igång med Gremlin.</span><span class="sxs-lookup"><span data-stu-id="6373a-144">We cover a few examples in this article tooget your started with Gremlin.</span></span> <span data-ttu-id="6373a-145">[Gremlin frågor](gremlin-support.md) har en detaljerad genomgång av Gremlin funktioner i Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="6373a-145">[Gremlin queries](gremlin-support.md) has a detailed walkthrough of Gremlin capabilities in Azure Cosmos DB.</span></span>

## <span data-ttu-id="6373a-146"><a id="add-references"></a>Anslut appen</span><span class="sxs-lookup"><span data-stu-id="6373a-146"><a id="add-references"></a>Connect your app</span></span>

<span data-ttu-id="6373a-147">Lägg till nedanstående två konstanter och dina *klienten* variabeln i ditt program.</span><span class="sxs-lookup"><span data-stu-id="6373a-147">Add these two constants and your *client* variable in your application.</span></span> 

```csharp
string endpoint = ConfigurationManager.AppSettings["Endpoint"]; 
string authKey = ConfigurationManager.AppSettings["AuthKey"]; 
``` 
<span data-ttu-id="6373a-148">Därefter head tillbaka toohello [Azure-portalen](https://portal.azure.com) tooretrieve din slutpunkts-URL och primärnyckel.</span><span class="sxs-lookup"><span data-stu-id="6373a-148">Next, head back toohello [Azure portal](https://portal.azure.com) tooretrieve your endpoint URL and primary key.</span></span> <span data-ttu-id="6373a-149">hello slutpunkts-URL och primärnyckel behövs för ditt program toounderstand där tooconnect till med Azure Cosmos DB tootrust appens anslutning.</span><span class="sxs-lookup"><span data-stu-id="6373a-149">hello endpoint URL and primary key are necessary for your application toounderstand where tooconnect to, and for Azure Cosmos DB tootrust your application's connection.</span></span> 

<span data-ttu-id="6373a-150">I hello Azure portal, navigerar tooyour Azure Cosmos DB konto, klickar på **nycklar**, och klicka sedan på **skrivskyddad nycklar**.</span><span class="sxs-lookup"><span data-stu-id="6373a-150">In hello Azure portal, navigate tooyour Azure Cosmos DB account, click **Keys**, and then click **Read-write Keys**.</span></span> 

<span data-ttu-id="6373a-151">Kopiera hello URI från hello-portalen och via `Endpoint` i hello slutpunktsegenskapen ovan.</span><span class="sxs-lookup"><span data-stu-id="6373a-151">Copy hello URI from hello portal and paste it over `Endpoint` in hello endpoint property above.</span></span> <span data-ttu-id="6373a-152">Och sedan kopiera hello PRIMÄRNYCKEL från hello-portalen och klistra in den i hello `AuthKey` egenskapen ovan.</span><span class="sxs-lookup"><span data-stu-id="6373a-152">Then copy hello PRIMARY KEY from hello portal and paste it into hello `AuthKey` property above.</span></span> 

<span data-ttu-id="6373a-153">! [Skärmbild som visar hello Azure-portalen som används av hello självstudiekursen toocreate C#-program.</span><span class="sxs-lookup"><span data-stu-id="6373a-153">![Screen shot of hello Azure portal used by hello tutorial toocreate a C# application.</span></span> <span data-ttu-id="6373a-154">Visar en Azure Cosmos DB konto hello knappen nycklar markerad i hello Azure Cosmos DB navigering och hello URI och PRIMÄRNYCKEL-värden som är markerade i hello bladet nycklar] [nycklar]</span><span class="sxs-lookup"><span data-stu-id="6373a-154">Shows an Azure Cosmos DB account hello KEYS button highlighted on hello Azure Cosmos DB navigation , and hello URI and PRIMARY KEY values highlighted on hello Keys blade][keys]</span></span> 
 
## <span data-ttu-id="6373a-155"><a id="instantiate"></a>Skapa en instans av hello DocumentClient</span><span class="sxs-lookup"><span data-stu-id="6373a-155"><a id="instantiate"></a>Instantiate hello DocumentClient</span></span> 
<span data-ttu-id="6373a-156">Skapa sedan en ny instans av hello **DocumentClient**.</span><span class="sxs-lookup"><span data-stu-id="6373a-156">Next, create a new instance of hello **DocumentClient**.</span></span>  

```csharp 
DocumentClient client = new DocumentClient(new Uri(endpoint), authKey); 
``` 

## <span data-ttu-id="6373a-157"><a id="create-database"></a>Skapa en databas</span><span class="sxs-lookup"><span data-stu-id="6373a-157"><a id="create-database"></a>Create a database</span></span> 

<span data-ttu-id="6373a-158">Nu ska du skapa en Azure-Cosmos-DB [databasen](documentdb-resources.md#databases) med hjälp av hello [CreateDatabaseAsync](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.documentclient.createdatabaseasync.aspx) metod eller [CreateDatabaseIfNotExistsAsync](https://msdn.microsoft.com/library/microsoft.azure.documents.client.documentclient.createdatabaseifnotexistsasync.aspx) metod för hello  **DocumentClient** klass från hello [.NET DocumentDB SDK](documentdb-sdk-dotnet.md).</span><span class="sxs-lookup"><span data-stu-id="6373a-158">Now, create an Azure Cosmos DB [database](documentdb-resources.md#databases) by using hello [CreateDatabaseAsync](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.documentclient.createdatabaseasync.aspx) method or [CreateDatabaseIfNotExistsAsync](https://msdn.microsoft.com/library/microsoft.azure.documents.client.documentclient.createdatabaseifnotexistsasync.aspx) method of hello **DocumentClient** class from hello [DocumentDB .NET SDK](documentdb-sdk-dotnet.md).</span></span>  

```csharp 
Database database = await client.CreateDatabaseIfNotExistsAsync(new Database { Id = "graphdb" }); 
``` 
 
## <a name="create-a-graph"></a><span data-ttu-id="6373a-159">Skapa ett diagram</span><span class="sxs-lookup"><span data-stu-id="6373a-159">Create a graph</span></span> 

<span data-ttu-id="6373a-160">Skapa sedan en behållare för diagram med hjälp av hello med hello [CreateDocumentCollectionAsync](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.documentclient.createdocumentcollectionasync.aspx) metod eller [CreateDocumentCollectionIfNotExistsAsync](https://msdn.microsoft.com/library/microsoft.azure.documents.client.documentclient.createdocumentcollectionifnotexistsasync.aspx) metod för hello **DocumentClient**  klass.</span><span class="sxs-lookup"><span data-stu-id="6373a-160">Next, create a graph container by using hello using hello [CreateDocumentCollectionAsync](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.documentclient.createdocumentcollectionasync.aspx) method or [CreateDocumentCollectionIfNotExistsAsync](https://msdn.microsoft.com/library/microsoft.azure.documents.client.documentclient.createdocumentcollectionifnotexistsasync.aspx) method of hello **DocumentClient** class.</span></span> <span data-ttu-id="6373a-161">En samling är en behållare för diagram entiteter.</span><span class="sxs-lookup"><span data-stu-id="6373a-161">A collection is a container of graph entities.</span></span> 

```csharp 
DocumentCollection graph = await client.CreateDocumentCollectionIfNotExistsAsync( 
    UriFactory.CreateDatabaseUri("graphdb"), 
    new DocumentCollection { Id = "graphcollz" }, 
    new RequestOptions { OfferThroughput = 1000 }); 
``` 

## <span data-ttu-id="6373a-162"><a id="serializing"></a>Serialisera hörn och kanter too.NET objekt</span><span class="sxs-lookup"><span data-stu-id="6373a-162"><a id="serializing"></a>Serialize vertices and edges too.NET objects</span></span>
<span data-ttu-id="6373a-163">Använder Azure Cosmos-DB hello [GraphSON kabelformat](gremlin-support.md), som definierar en JSON-schema för formhörnen, kanter och egenskaper.</span><span class="sxs-lookup"><span data-stu-id="6373a-163">Azure Cosmos DB uses hello [GraphSON wire format](gremlin-support.md), which defines a JSON schema for vertices, edges, and properties.</span></span> <span data-ttu-id="6373a-164">hello Azure Cosmos DB .NET SDK innehåller JSON.NET som ett beroende och detta gör att vi tooserialize/deserialisering GraphSON i .NET-objekt som vi kan arbeta med kod.</span><span class="sxs-lookup"><span data-stu-id="6373a-164">hello Azure Cosmos DB .NET SDK includes JSON.NET as a dependency, and this allows us tooserialize/deserialize GraphSON into .NET objects that we can work with in code.</span></span>

<span data-ttu-id="6373a-165">Exempelvis vi arbeta med ett enkelt sociala nätverk med fyra personer.</span><span class="sxs-lookup"><span data-stu-id="6373a-165">As an example, let's work with a simple social network with four people.</span></span> <span data-ttu-id="6373a-166">Vi titta på hur toocreate `Person` formhörnen, lägga till `Knows` relationer mellan dem, och sedan fråga och passerar hello diagram toofind ”vän vän” relationer.</span><span class="sxs-lookup"><span data-stu-id="6373a-166">We look at how toocreate `Person` vertices, add `Knows` relationships between them, then query and traverse hello graph toofind "friend of friend" relationships.</span></span> 

<span data-ttu-id="6373a-167">Hej `Microsoft.Azure.Graphs.Elements` namnområde ger `Vertex`, `Edge`, `Property` och `VertexProperty` klasser för avserialisering av GraphSON svar toowell användardefinierade .NET objekt.</span><span class="sxs-lookup"><span data-stu-id="6373a-167">hello `Microsoft.Azure.Graphs.Elements` namespace provides `Vertex`, `Edge`, `Property` and `VertexProperty` classes for deserializing GraphSON responses toowell-defined .NET objects.</span></span>

## <a name="run-gremlin-using-creategremlinquery"></a><span data-ttu-id="6373a-168">Kör Gremlin med CreateGremlinQuery</span><span class="sxs-lookup"><span data-stu-id="6373a-168">Run Gremlin using CreateGremlinQuery</span></span>
<span data-ttu-id="6373a-169">Gremlin som SQL, stöder Läs-, Skriv- och frågor.</span><span class="sxs-lookup"><span data-stu-id="6373a-169">Gremlin, like SQL, supports read, write, and query operations.</span></span> <span data-ttu-id="6373a-170">Till exempel hello följande utdrag visar hur toocreate formhörnen, kanter, utföra vissa exempelfrågor som använder `CreateGremlinQuery<T>`, och asynkront gå igenom dessa resultat med hjälp av `ExecuteNextAsync` och ' HasMoreResults.</span><span class="sxs-lookup"><span data-stu-id="6373a-170">For example, hello following snippet shows how toocreate vertices, edges, perform some sample queries using `CreateGremlinQuery<T>`, and asynchronously iterate through these results using `ExecuteNextAsync` and \`HasMoreResults.</span></span>

```cs
Dictionary<string, string> gremlinQueries = new Dictionary<string, string>
{
    { "Cleanup",        "g.V().drop()" },
    { "AddVertex 1",    "g.addV('person').property('id', 'thomas').property('firstName', 'Thomas').property('age', 44)" },
    { "AddVertex 2",    "g.addV('person').property('id', 'mary').property('firstName', 'Mary').property('lastName', 'Andersen').property('age', 39)" },
    { "AddVertex 3",    "g.addV('person').property('id', 'ben').property('firstName', 'Ben').property('lastName', 'Miller')" },
    { "AddVertex 4",    "g.addV('person').property('id', 'robin').property('firstName', 'Robin').property('lastName', 'Wakefield')" },
    { "AddEdge 1",      "g.V('thomas').addE('knows').to(g.V('mary'))" },
    { "AddEdge 2",      "g.V('thomas').addE('knows').to(g.V('ben'))" },
    { "AddEdge 3",      "g.V('ben').addE('knows').to(g.V('robin'))" },
    { "UpdateVertex",   "g.V('thomas').property('age', 44)" },
    { "CountVertices",  "g.V().count()" },
    { "Filter Range",   "g.V().hasLabel('person').has('age', gt(40))" },
    { "Project",        "g.V().hasLabel('person').values('firstName')" },
    { "Sort",           "g.V().hasLabel('person').order().by('firstName', decr)" },
    { "Traverse",       "g.V('thomas').outE('knows').inV().hasLabel('person')" },
    { "Traverse 2x",    "g.V('thomas').outE('knows').inV().hasLabel('person').outE('knows').inV().hasLabel('person')" },
    { "Loop",           "g.V('thomas').repeat(out()).until(has('id', 'robin')).path()" },
    { "DropEdge",       "g.V('thomas').outE('knows').where(inV().has('id', 'mary')).drop()" },
    { "CountEdges",     "g.E().count()" },
    { "DropVertex",     "g.V('thomas').drop()" },
};

foreach (KeyValuePair<string, string> gremlinQuery in gremlinQueries)
{
    Console.WriteLine($"Running {gremlinQuery.Key}: {gremlinQuery.Value}");

    // hello CreateGremlinQuery method extensions allow you tooexecute Gremlin queries and iterate
    // results asychronously
    IDocumentQuery<dynamic> query = client.CreateGremlinQuery<dynamic>(graph, gremlinQuery.Value);
    while (query.HasMoreResults)
    {
        foreach (dynamic result in await query.ExecuteNextAsync())
        {
            Console.WriteLine($"\t {JsonConvert.SerializeObject(result)}");
        }
    }

    Console.WriteLine();
}
```

## <a name="add-vertices-and-edges"></a><span data-ttu-id="6373a-171">Lägg till hörn och kanter</span><span class="sxs-lookup"><span data-stu-id="6373a-171">Add vertices and edges</span></span>

<span data-ttu-id="6373a-172">Nu ska vi titta på hello Gremlin rapporterna visas i föregående avsnitt hello detalj.</span><span class="sxs-lookup"><span data-stu-id="6373a-172">Let's look at hello Gremlin statements shown in hello preceding section more detail.</span></span> <span data-ttu-id="6373a-173">Första vi vissa formhörnen med Gremlin's `addV` metod.</span><span class="sxs-lookup"><span data-stu-id="6373a-173">First we some vertices using Gremlin's `addV` method.</span></span> <span data-ttu-id="6373a-174">Till exempel skapas hello följande kodavsnitt en ”Thomas Andersen” nod av typen ”Person”, med egenskaper för förnamn, efternamn och ålder.</span><span class="sxs-lookup"><span data-stu-id="6373a-174">For example, hello following snippet creates a "Thomas Andersen" vertex of type "Person", with properties for first name, last name, and age.</span></span>

```cs
// Create a vertex
IDocumentQuery<Vertex> createVertexQuery = client.CreateGremlinQuery<Vertex>(
    graphCollection, 
    "g.addV('person').property('firstName', 'Thomas')");

while (createVertexQuery.HasMoreResults)
{
    Vertex thomas = (await create.ExecuteNextAsync<Vertex>()).First();
}
```

<span data-ttu-id="6373a-175">Vi skapa vissa kanter mellan dessa formhörnen med Gremlin's `addE` metod.</span><span class="sxs-lookup"><span data-stu-id="6373a-175">Then we create some edges between these vertices using Gremlin's `addE` method.</span></span> 

```cs
// Add a "knows" edge
IDocumentQuery<Edge> createEdgeQuery = client.CreateGremlinQuery<Edge>(
    graphCollection, 
    "g.V('thomas').addE('knows').to(g.V('mary'))");

while (create.HasMoreResults)
{
    Edge thomasKnowsMaryEdge = (await create.ExecuteNextAsync<Edge>()).First();
}
```

<span data-ttu-id="6373a-176">Vi kan uppdatera en befintlig vertex med `properties` steg i Gremlin.</span><span class="sxs-lookup"><span data-stu-id="6373a-176">We can update an existing vertex by using `properties` step in Gremlin.</span></span> <span data-ttu-id="6373a-177">Vi hoppar över hello anropet tooexecute hello frågan via `HasMoreResults` och `ExecuteNextAsync` för hello resten av hello exempel.</span><span class="sxs-lookup"><span data-stu-id="6373a-177">We skip hello call tooexecute hello query via `HasMoreResults` and `ExecuteNextAsync` for hello rest of hello examples.</span></span>

```cs
// Update a vertex
client.CreateGremlinQuery<Vertex>(
    graphCollection, 
    "g.V('thomas').property('age', 45)");
```

<span data-ttu-id="6373a-178">Du kan släppa kanter och formhörnen med Gremlin's `drop` steg.</span><span class="sxs-lookup"><span data-stu-id="6373a-178">You can drop edges and vertices using Gremlin's `drop` step.</span></span> <span data-ttu-id="6373a-179">Här är ett kodfragment som visar hur toodelete en nod och en kant.</span><span class="sxs-lookup"><span data-stu-id="6373a-179">Here's a snippet that shows how toodelete a vertex and an edge.</span></span> <span data-ttu-id="6373a-180">Observera att släppa en nod utför borttagningsåtgärder av hello associerade kanter.</span><span class="sxs-lookup"><span data-stu-id="6373a-180">Note that dropping a vertex performs a cascading delete of hello associated edges.</span></span>

```cs
// Drop an edge
client.CreateGremlinQuery(graphCollection, "g.E('thomasKnowsRobin').drop()");

// Drop a vertex
client.CreateGremlinQuery(graphCollection, "g.V('robin').drop()");
```

## <a name="query-hello-graph"></a><span data-ttu-id="6373a-181">Frågan hello diagram</span><span class="sxs-lookup"><span data-stu-id="6373a-181">Query hello graph</span></span>

<span data-ttu-id="6373a-182">Du kan utföra frågor och traversals som också använder Gremlin.</span><span class="sxs-lookup"><span data-stu-id="6373a-182">You can perform queries and traversals also using Gremlin.</span></span> <span data-ttu-id="6373a-183">Hello följande fragment visar till exempel hur toocount hello antalet formhörnen i hello diagram:</span><span class="sxs-lookup"><span data-stu-id="6373a-183">For example, hello following snippet shows how toocount hello number of vertices in hello graph:</span></span>

```cs
// Run a query toocount vertices
IDocumentQuery<int> countQuery = client.CreateGremlinQuery<int>(graphCollection, "g.V().count()");
```
<span data-ttu-id="6373a-184">Du kan utföra filter med Gremlin's `has` och `hasLabel` steg och kombinera dem med hjälp av `and`, `or`, och `not` toobuild mer komplexa filter:</span><span class="sxs-lookup"><span data-stu-id="6373a-184">You can perform filters using Gremlin's `has` and `hasLabel` steps, and combine them using `and`, `or`, and `not` toobuild more complex filters:</span></span>

```cs
// Run a query with filter
IDocumentQuery<Vertex> personsByAge = client.CreateGremlinQuery<Vertex>(
  graphCollection, 
  "g.V().hasLabel('person').has('age', gt(40))");
```

<span data-ttu-id="6373a-185">Du kan projicera vissa egenskaper i hello frågeresultat med hello `values` steg:</span><span class="sxs-lookup"><span data-stu-id="6373a-185">You can project certain properties in hello query results using hello `values` step:</span></span>

```cs
// Run a query with projection
IDocumentQuery<string> firstNames = client.CreateGremlinQuery<string>(
  graphCollection, 
  $"g.V().hasLabel('person').values('firstName')");
```

<span data-ttu-id="6373a-186">Hittills har sett vi endast frågeoperatorer som fungerar i en databas.</span><span class="sxs-lookup"><span data-stu-id="6373a-186">So far, we've only seen query operators that work in any database.</span></span> <span data-ttu-id="6373a-187">Diagram är snabb och effektiv för traversal åtgärder när du behöver toonavigate toorelated kanter och formhörnen.</span><span class="sxs-lookup"><span data-stu-id="6373a-187">Graphs are fast and efficient for traversal operations when you need toonavigate toorelated edges and vertices.</span></span> <span data-ttu-id="6373a-188">Vi ska hitta alla vänner till Thomas.</span><span class="sxs-lookup"><span data-stu-id="6373a-188">Let's find all friends of Thomas.</span></span> <span data-ttu-id="6373a-189">Vi kan göra detta med hjälp av Gremlin's `outE` steg toofind alla hello kanter ut från Thomas och sedan gå igenom toohello i formhörnen från dessa kanter med Gremlin's `inV` steg:</span><span class="sxs-lookup"><span data-stu-id="6373a-189">We do this by using Gremlin's `outE` step toofind all hello out-edges from Thomas, then traversing toohello in-vertices from those edges using Gremlin's `inV` step:</span></span>

```cs
// Run a traversal (find friends of Thomas)
IDocumentQuery<Vertex> friendsOfThomas = client.CreateGremlinQuery<Vertex>(
  graphCollection,
  "g.V('thomas').outE('knows').inV().hasLabel('person')");
```

<span data-ttu-id="6373a-190">hello nästa frågan utför två hopp toofind alla Thomas' ”vänner till vänner”, genom att anropa `outE` och `inV` två gånger.</span><span class="sxs-lookup"><span data-stu-id="6373a-190">hello next query performs two hops toofind all of Thomas' "friends of friends", by calling `outE` and `inV` two times.</span></span> 

```cs
// Run a traversal (find friends of friends of Thomas)
IDocumentQuery<Vertex> friendsOfFriendsOfThomas = client.CreateGremlinQuery<Vertex>(
  graphCollection,
  "g.V('thomas').outE('knows').inV().hasLabel('person').outE('knows').inV().hasLabel('person')");
```

<span data-ttu-id="6373a-191">Du kan skapa mer komplexa frågor och implementera kraftfulla diagrammet traversal logik med Gremlin, inklusive blandningen filter uttryck, utför slingor med hello `loop` steg och implementera villkorlig navigeringen med hello `choose` steg.</span><span class="sxs-lookup"><span data-stu-id="6373a-191">You can build more complex queries and implement powerful graph traversal logic using Gremlin, including mixing filter expressions, performing looping using hello `loop` step, and implementing conditional navigation using hello `choose` step.</span></span> <span data-ttu-id="6373a-192">Mer information om vad du kan göra med [Gremlin stöd](gremlin-support.md)!</span><span class="sxs-lookup"><span data-stu-id="6373a-192">Learn more about what you can do with [Gremlin support](gremlin-support.md)!</span></span>

<span data-ttu-id="6373a-193">Det, självstudierna Azure Cosmos DB är klar!</span><span class="sxs-lookup"><span data-stu-id="6373a-193">That's it, this Azure Cosmos DB tutorial is complete!</span></span> 

## <a name="clean-up-resources"></a><span data-ttu-id="6373a-194">Rensa resurser</span><span class="sxs-lookup"><span data-stu-id="6373a-194">Clean up resources</span></span>

<span data-ttu-id="6373a-195">Om du inte kommer toocontinue toouse den här appen, använda hello följande steg toodelete alla resurser som skapats av den här kursen i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="6373a-195">If you're not going toocontinue toouse this app, use hello following steps toodelete all resources created by this tutorial in hello Azure portal.</span></span>  

1. <span data-ttu-id="6373a-196">Hello vänstra menyn i hello Azure-portalen klickar du på **resursgrupper** och klicka sedan på hello namnet på hello resurs du skapat.</span><span class="sxs-lookup"><span data-stu-id="6373a-196">From hello left-hand menu in hello Azure portal, click **Resource groups** and then click hello name of hello resource you created.</span></span> 
2. <span data-ttu-id="6373a-197">På din resurs gruppen klickar du på **ta bort**typnamn hello för hello resurs toodelete i hello textrutan och klicka sedan på **ta bort**.</span><span class="sxs-lookup"><span data-stu-id="6373a-197">On your resource group page, click **Delete**, type hello name of hello resource toodelete in hello text box, and then click **Delete**.</span></span>

## <a name="next-steps"></a><span data-ttu-id="6373a-198">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="6373a-198">Next Steps</span></span>

<span data-ttu-id="6373a-199">I den här kursen har du gjort hello följande:</span><span class="sxs-lookup"><span data-stu-id="6373a-199">In this tutorial, you've done hello following:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="6373a-200">Skapa ett Azure DB som Cosmos-konto</span><span class="sxs-lookup"><span data-stu-id="6373a-200">Created an Azure Cosmos DB account</span></span> 
> * <span data-ttu-id="6373a-201">Skapa ett diagram databas och en behållare</span><span class="sxs-lookup"><span data-stu-id="6373a-201">Created a graph database and container</span></span>
> * <span data-ttu-id="6373a-202">Serialiserade hörn och kanter too.NET objekt</span><span class="sxs-lookup"><span data-stu-id="6373a-202">Serialized vertices and edges too.NET objects</span></span>
> * <span data-ttu-id="6373a-203">Tillagda hörn och kanter</span><span class="sxs-lookup"><span data-stu-id="6373a-203">Added vertices and edges</span></span>
> * <span data-ttu-id="6373a-204">Efterfrågade hello diagram med hjälp av Gremlin</span><span class="sxs-lookup"><span data-stu-id="6373a-204">Queried hello graph using Gremlin</span></span>

<span data-ttu-id="6373a-205">Nu kan du skapa mer komplexa frågor och implementera kraftfull logik för grafbläddring med Gremlin.</span><span class="sxs-lookup"><span data-stu-id="6373a-205">You can now build more complex queries and implement powerful graph traversal logic using Gremlin.</span></span> 

> [!div class="nextstepaction"]
> [<span data-ttu-id="6373a-206">Fråga med hjälp av Gremlin</span><span class="sxs-lookup"><span data-stu-id="6373a-206">Query using Gremlin</span></span>](tutorial-query-graph.md)
