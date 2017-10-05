---
title: 'Azure Cosmos DB: Utveckla med Graph API i .NET | Microsoft Docs'
description: "Lär dig att utveckla med Azure Cosmos DB DocumentDB-API med hjälp av .NET"
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
ms.openlocfilehash: eeaa0c4f84a408815371742334d2ba7ce600b72f
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="azure-cosmos-db-develop-with-the-graph-api-in-net"></a><span data-ttu-id="7c454-103">Azure Cosmos DB: Utveckla med Graph API i .NET</span><span class="sxs-lookup"><span data-stu-id="7c454-103">Azure Cosmos DB: Develop with the Graph API in .NET</span></span>
<span data-ttu-id="7c454-104">Azure Cosmos-DB är Microsofts globalt distribuerade flera modellen database-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="7c454-104">Azure Cosmos DB is Microsoft's globally distributed multi-model database service.</span></span> <span data-ttu-id="7c454-105">Du kan snabbt skapa och ställa frågor mot databaser med dokument, nyckel/värde-par och grafer. Du får fördelar av den globala distributionen och den horisontella skalningsförmågan som ligger i grunden hos Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="7c454-105">You can quickly create and query document, key/value, and graph databases, all of which benefit from the global distribution and horizontal scale capabilities at the core of Azure Cosmos DB.</span></span> 

<span data-ttu-id="7c454-106">Den här kursen visar hur du skapar ett Azure DB som Cosmos-konto med Azure-portalen och hur du skapar ett diagram databas och en behållare.</span><span class="sxs-lookup"><span data-stu-id="7c454-106">This tutorial demonstrates how to create an Azure Cosmos DB account using the Azure portal and how to create a graph database and container.</span></span> <span data-ttu-id="7c454-107">Sedan skapar ett enkelt sociala nätverk med fyra personer som använder den [Graph API](graph-sdk-dotnet.md) (förhandsversion) och sedan passerar och frågar diagrammet med Gremlin.</span><span class="sxs-lookup"><span data-stu-id="7c454-107">The application then creates a simple social network with four people using the [Graph API](graph-sdk-dotnet.md) (preview), then traverses and queries the graph using Gremlin.</span></span>

<span data-ttu-id="7c454-108">Den här kursen ingår följande uppgifter:</span><span class="sxs-lookup"><span data-stu-id="7c454-108">This tutorial covers the following tasks:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="7c454-109">Skapa ett Azure Cosmos DB-konto</span><span class="sxs-lookup"><span data-stu-id="7c454-109">Create an Azure Cosmos DB account</span></span> 
> * <span data-ttu-id="7c454-110">Skapa ett diagram databas och en behållare</span><span class="sxs-lookup"><span data-stu-id="7c454-110">Create a graph database and container</span></span>
> * <span data-ttu-id="7c454-111">Serialisera hörn och kanter till .NET-objekt</span><span class="sxs-lookup"><span data-stu-id="7c454-111">Serialize vertices and edges to .NET objects</span></span>
> * <span data-ttu-id="7c454-112">Lägg till hörn och kanter</span><span class="sxs-lookup"><span data-stu-id="7c454-112">Add vertices and edges</span></span>
> * <span data-ttu-id="7c454-113">Fråga diagrammet med Gremlin</span><span class="sxs-lookup"><span data-stu-id="7c454-113">Query the graph using Gremlin</span></span>

## <a name="graphs-in-azure-cosmos-db"></a><span data-ttu-id="7c454-114">Diagram i Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="7c454-114">Graphs in Azure Cosmos DB</span></span>
<span data-ttu-id="7c454-115">Du kan använda Azure Cosmos DB att skapa, uppdatera och fråga diagram med hjälp av den [Microsoft.Azure.Graphs](graph-sdk-dotnet.md) bibliotek.</span><span class="sxs-lookup"><span data-stu-id="7c454-115">You can use Azure Cosmos DB to create, update, and query graphs using the [Microsoft.Azure.Graphs](graph-sdk-dotnet.md) library.</span></span> <span data-ttu-id="7c454-116">Microsoft.Azure.Graph-bibliotek innehåller en enda metod `CreateGremlinQuery<T>` ovanpå det `DocumentClient` klassen för att köra Gremlin frågor.</span><span class="sxs-lookup"><span data-stu-id="7c454-116">The Microsoft.Azure.Graph library provides a single extension method `CreateGremlinQuery<T>` on top of the `DocumentClient` class to execute Gremlin queries.</span></span>

<span data-ttu-id="7c454-117">Gremlin är en funktionell programmeringsspråk som stöder skriva åtgärder (DML) och fråga och traversal-åtgärder.</span><span class="sxs-lookup"><span data-stu-id="7c454-117">Gremlin is a functional programming language that supports write operations (DML) and query and traversal operations.</span></span> <span data-ttu-id="7c454-118">Vi igenom några exempel i den här artikeln för att få din igång med Gremlin.</span><span class="sxs-lookup"><span data-stu-id="7c454-118">We cover a few examples in this article to get your started with Gremlin.</span></span> <span data-ttu-id="7c454-119">Se [Gremlin frågor](gremlin-support.md) en detaljerad genomgång av Gremlin av funktionerna i Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="7c454-119">See [Gremlin queries](gremlin-support.md) for a detailed walkthrough of Gremlin capabilities available in Azure Cosmos DB.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="7c454-120">Krav</span><span class="sxs-lookup"><span data-stu-id="7c454-120">Prerequisites</span></span>
<span data-ttu-id="7c454-121">Se till att du har följande:</span><span class="sxs-lookup"><span data-stu-id="7c454-121">Please make sure you have the following:</span></span>

* <span data-ttu-id="7c454-122">Ett aktivt Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="7c454-122">An active Azure account.</span></span> <span data-ttu-id="7c454-123">Om du inte har ett kan du registrera dig för ett [kostnadsfritt konto](https://azure.microsoft.com/free/).</span><span class="sxs-lookup"><span data-stu-id="7c454-123">If you don't have one, you can sign up for a [free account](https://azure.microsoft.com/free/).</span></span> 
    * <span data-ttu-id="7c454-124">Du kan också använda [Azure DocumentDB-emulatorn](local-emulator.md) för den här självstudien.</span><span class="sxs-lookup"><span data-stu-id="7c454-124">Alternatively, you can use the [Azure DocumentDB Emulator](local-emulator.md) for this tutorial.</span></span>
* <span data-ttu-id="7c454-125">[Visual Studio](http://www.visualstudio.com/).</span><span class="sxs-lookup"><span data-stu-id="7c454-125">[Visual Studio](http://www.visualstudio.com/).</span></span>

## <a name="create-database-account"></a><span data-ttu-id="7c454-126">Skapa konto</span><span class="sxs-lookup"><span data-stu-id="7c454-126">Create database account</span></span>

<span data-ttu-id="7c454-127">Börja med att skapa ett Azure DB som Cosmos-konto i Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="7c454-127">Let's start by creating an Azure Cosmos DB account in the Azure portal.</span></span>  

> [!TIP]
> * <span data-ttu-id="7c454-128">Redan har ett Azure DB som Cosmos-konto?</span><span class="sxs-lookup"><span data-stu-id="7c454-128">Already have an Azure Cosmos DB account?</span></span> <span data-ttu-id="7c454-129">I så fall, gå vidare till [konfigurera Visual Studio-lösning](#SetupVS)</span><span class="sxs-lookup"><span data-stu-id="7c454-129">If so, skip ahead to [Set up your Visual Studio solution](#SetupVS)</span></span>
> * <span data-ttu-id="7c454-130">Hade du ett Azure DocumentDB-konto?</span><span class="sxs-lookup"><span data-stu-id="7c454-130">Did you have an Azure DocumentDB account?</span></span> <span data-ttu-id="7c454-131">Om ditt konto är nu ett Azure DB som Cosmos-konto och du kan gå vidare till så [konfigurera Visual Studio-lösning](#SetupVS).</span><span class="sxs-lookup"><span data-stu-id="7c454-131">If so, your account is now an Azure Cosmos DB account and you can skip ahead to [Set up your Visual Studio solution](#SetupVS).</span></span>  
> * <span data-ttu-id="7c454-132">Om du använder Azure Cosmos DB-emulatorn, följer du stegen i [Azure Cosmos DB emulatorn](local-emulator.md) konfigurera emulatorn och gå vidare till [ställa in din Visual Studio-lösning](#SetupVS).</span><span class="sxs-lookup"><span data-stu-id="7c454-132">If you are using the Azure Cosmos DB Emulator, please follow the steps at [Azure Cosmos DB Emulator](local-emulator.md) to setup the emulator and skip ahead to [Set up your Visual Studio Solution](#SetupVS).</span></span> 
>
> 

[!INCLUDE [cosmos-db-create-dbaccount-graph](../../includes/cosmos-db-create-dbaccount-graph.md)]

## <span data-ttu-id="7c454-133"><a id="SetupVS"></a>Konfigurera din Visual Studio-lösning</span><span class="sxs-lookup"><span data-stu-id="7c454-133"><a id="SetupVS"></a>Set up your Visual Studio solution</span></span>
1. <span data-ttu-id="7c454-134">Öppna **Visual Studio** på datorn.</span><span class="sxs-lookup"><span data-stu-id="7c454-134">Open **Visual Studio** on your computer.</span></span>
2. <span data-ttu-id="7c454-135">I menyn **Arkiv** väljer du **Nytt** och sedan **Projekt**.</span><span class="sxs-lookup"><span data-stu-id="7c454-135">On the **File** menu, select **New**, and then choose **Project**.</span></span>
3. <span data-ttu-id="7c454-136">I den **nytt projekt** markerar **mallar** / **Visual C#** / **Konsolapp (.NET Framework)**namnge projektet och klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="7c454-136">In the **New Project** dialog, select **Templates** / **Visual C#** / **Console App (.NET Framework)**, name your project, and then click **OK**.</span></span>
4. <span data-ttu-id="7c454-137">I **Solution Explorer** högerklickar du på den nya konsolappen, som finns under din Visual Studio-lösning, och klickar sedan på **Hantera NuGet-paket ...**</span><span class="sxs-lookup"><span data-stu-id="7c454-137">In the **Solution Explorer**, right click on your new console application, which is under your Visual Studio solution, and then click **Manage NuGet Packages...**</span></span>
5. <span data-ttu-id="7c454-138">I den **NuGet** klickar du på **Bläddra**, och skriv **Microsoft.Azure.Graphs** i sökrutan och kontrollera den **är förhandsversioner**.</span><span class="sxs-lookup"><span data-stu-id="7c454-138">In the **NuGet** tab, click **Browse**, and type **Microsoft.Azure.Graphs** in the search box, and check the **Include prerelease versions**.</span></span>
6. <span data-ttu-id="7c454-139">I resultatet hitta **Microsoft.Azure.Graphs** och på **installera**.</span><span class="sxs-lookup"><span data-stu-id="7c454-139">Within the results, find **Microsoft.Azure.Graphs** and click **Install**.</span></span>
   
   <span data-ttu-id="7c454-140">Om du får ett meddelande om att granska ändringar i lösningen klickar du på **OK**.</span><span class="sxs-lookup"><span data-stu-id="7c454-140">If you get a message about reviewing changes to the solution, click **OK**.</span></span> <span data-ttu-id="7c454-141">Om du får ett meddelande om godkännande av licens klickar du på **Jag godkänner**.</span><span class="sxs-lookup"><span data-stu-id="7c454-141">If you get a message about license acceptance, click **I accept**.</span></span>
   
    <span data-ttu-id="7c454-142">Den `Microsoft.Azure.Graphs` -bibliotek innehåller en enda metod `CreateGremlinQuery<T>` för att köra Gremlin åtgärder.</span><span class="sxs-lookup"><span data-stu-id="7c454-142">The `Microsoft.Azure.Graphs` library provides a single extension method `CreateGremlinQuery<T>` for executing Gremlin operations.</span></span> <span data-ttu-id="7c454-143">Gremlin är en funktionell programmeringsspråk som stöder skriva åtgärder (DML) och fråga och traversal-åtgärder.</span><span class="sxs-lookup"><span data-stu-id="7c454-143">Gremlin is a functional programming language that supports write operations (DML) and query and traversal operations.</span></span> <span data-ttu-id="7c454-144">Vi igenom några exempel i den här artikeln för att få din igång med Gremlin.</span><span class="sxs-lookup"><span data-stu-id="7c454-144">We cover a few examples in this article to get your started with Gremlin.</span></span> <span data-ttu-id="7c454-145">[Gremlin frågor](gremlin-support.md) har en detaljerad genomgång av Gremlin funktioner i Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="7c454-145">[Gremlin queries](gremlin-support.md) has a detailed walkthrough of Gremlin capabilities in Azure Cosmos DB.</span></span>

## <span data-ttu-id="7c454-146"><a id="add-references"></a>Anslut appen</span><span class="sxs-lookup"><span data-stu-id="7c454-146"><a id="add-references"></a>Connect your app</span></span>

<span data-ttu-id="7c454-147">Lägg till nedanstående två konstanter och dina *klienten* variabeln i ditt program.</span><span class="sxs-lookup"><span data-stu-id="7c454-147">Add these two constants and your *client* variable in your application.</span></span> 

```csharp
string endpoint = ConfigurationManager.AppSettings["Endpoint"]; 
string authKey = ConfigurationManager.AppSettings["AuthKey"]; 
``` 
<span data-ttu-id="7c454-148">Därefter head tillbaka till den [Azure-portalen](https://portal.azure.com) att hämta dina slutpunkts-URL och primärnyckel.</span><span class="sxs-lookup"><span data-stu-id="7c454-148">Next, head back to the [Azure portal](https://portal.azure.com) to retrieve your endpoint URL and primary key.</span></span> <span data-ttu-id="7c454-149">Slutpunkts-URL:en och den primära nyckeln behövs för att programmet ska veta vart ditt program ska ansluta, och för att Azure Cosmos DB ska lita på programmets anslutning.</span><span class="sxs-lookup"><span data-stu-id="7c454-149">The endpoint URL and primary key are necessary for your application to understand where to connect to, and for Azure Cosmos DB to trust your application's connection.</span></span> 

<span data-ttu-id="7c454-150">Navigera till ditt Azure DB som Cosmos-konto i Azure-portalen klickar du på **nycklar**, och klicka sedan på **skrivskyddad nycklar**.</span><span class="sxs-lookup"><span data-stu-id="7c454-150">In the Azure portal, navigate to your Azure Cosmos DB account, click **Keys**, and then click **Read-write Keys**.</span></span> 

<span data-ttu-id="7c454-151">Kopiera URI: N från portalen och via `Endpoint` i slutpunktsegenskapen.</span><span class="sxs-lookup"><span data-stu-id="7c454-151">Copy the URI from the portal and paste it over `Endpoint` in the endpoint property above.</span></span> <span data-ttu-id="7c454-152">Kopiera den PRIMÄRNYCKELN från portalen och klistrar in det i den `AuthKey` egenskapen ovan.</span><span class="sxs-lookup"><span data-stu-id="7c454-152">Then copy the PRIMARY KEY from the portal and paste it into the `AuthKey` property above.</span></span> 

<span data-ttu-id="7c454-153">! [Skärmdump av Azure portal som används av kursen för att skapa ett C#-program.</span><span class="sxs-lookup"><span data-stu-id="7c454-153">![Screen shot of the Azure portal used by the tutorial to create a C# application.</span></span> <span data-ttu-id="7c454-154">Visar en Cosmos-databas med Azure-konto knappen nycklar markerad i navigeringen till Azure Cosmos DB och värdena URI och PRIMÄRNYCKEL markerade i bladet nycklar] [nycklar]</span><span class="sxs-lookup"><span data-stu-id="7c454-154">Shows an Azure Cosmos DB account the KEYS button highlighted on the Azure Cosmos DB navigation , and the URI and PRIMARY KEY values highlighted on the Keys blade][keys]</span></span> 
 
## <span data-ttu-id="7c454-155"><a id="instantiate"></a>Skapa en instans av DocumentClient</span><span class="sxs-lookup"><span data-stu-id="7c454-155"><a id="instantiate"></a>Instantiate the DocumentClient</span></span> 
<span data-ttu-id="7c454-156">Skapa sedan en ny instans av den **DocumentClient**.</span><span class="sxs-lookup"><span data-stu-id="7c454-156">Next, create a new instance of the **DocumentClient**.</span></span>  

```csharp 
DocumentClient client = new DocumentClient(new Uri(endpoint), authKey); 
``` 

## <span data-ttu-id="7c454-157"><a id="create-database"></a>Skapa en databas</span><span class="sxs-lookup"><span data-stu-id="7c454-157"><a id="create-database"></a>Create a database</span></span> 

<span data-ttu-id="7c454-158">Nu skapa en Azure-Cosmos-DB [databasen](documentdb-resources.md#databases) med hjälp av den [CreateDatabaseAsync](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.documentclient.createdatabaseasync.aspx) metod eller [CreateDatabaseIfNotExistsAsync](https://msdn.microsoft.com/library/microsoft.azure.documents.client.documentclient.createdatabaseifnotexistsasync.aspx) metod för den **DocumentClient** klass från den [.NET DocumentDB SDK](documentdb-sdk-dotnet.md).</span><span class="sxs-lookup"><span data-stu-id="7c454-158">Now, create an Azure Cosmos DB [database](documentdb-resources.md#databases) by using the [CreateDatabaseAsync](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.documentclient.createdatabaseasync.aspx) method or [CreateDatabaseIfNotExistsAsync](https://msdn.microsoft.com/library/microsoft.azure.documents.client.documentclient.createdatabaseifnotexistsasync.aspx) method of the **DocumentClient** class from the [DocumentDB .NET SDK](documentdb-sdk-dotnet.md).</span></span>  

```csharp 
Database database = await client.CreateDatabaseIfNotExistsAsync(new Database { Id = "graphdb" }); 
``` 
 
## <a name="create-a-graph"></a><span data-ttu-id="7c454-159">Skapa ett diagram</span><span class="sxs-lookup"><span data-stu-id="7c454-159">Create a graph</span></span> 

<span data-ttu-id="7c454-160">Därefter skapa en behållare för diagram med hjälp av den med hjälp av den [CreateDocumentCollectionAsync](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.documentclient.createdocumentcollectionasync.aspx) metod eller [CreateDocumentCollectionIfNotExistsAsync](https://msdn.microsoft.com/library/microsoft.azure.documents.client.documentclient.createdocumentcollectionifnotexistsasync.aspx) metod för den **DocumentClient** klass.</span><span class="sxs-lookup"><span data-stu-id="7c454-160">Next, create a graph container by using the using the [CreateDocumentCollectionAsync](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.documentclient.createdocumentcollectionasync.aspx) method or [CreateDocumentCollectionIfNotExistsAsync](https://msdn.microsoft.com/library/microsoft.azure.documents.client.documentclient.createdocumentcollectionifnotexistsasync.aspx) method of the **DocumentClient** class.</span></span> <span data-ttu-id="7c454-161">En samling är en behållare för diagram entiteter.</span><span class="sxs-lookup"><span data-stu-id="7c454-161">A collection is a container of graph entities.</span></span> 

```csharp 
DocumentCollection graph = await client.CreateDocumentCollectionIfNotExistsAsync( 
    UriFactory.CreateDatabaseUri("graphdb"), 
    new DocumentCollection { Id = "graphcollz" }, 
    new RequestOptions { OfferThroughput = 1000 }); 
``` 

## <span data-ttu-id="7c454-162"><a id="serializing"></a>Serialisera hörn och kanter till .NET-objekt</span><span class="sxs-lookup"><span data-stu-id="7c454-162"><a id="serializing"></a>Serialize vertices and edges to .NET objects</span></span>
<span data-ttu-id="7c454-163">Använder Azure Cosmos-DB i [GraphSON kabelformat](gremlin-support.md), som definierar en JSON-schema för formhörnen, kanter och egenskaper.</span><span class="sxs-lookup"><span data-stu-id="7c454-163">Azure Cosmos DB uses the [GraphSON wire format](gremlin-support.md), which defines a JSON schema for vertices, edges, and properties.</span></span> <span data-ttu-id="7c454-164">Azure Cosmos DB .NET SDK innehåller JSON.NET som ett beroende och detta gör att vi kan serialisering/deserialisering GraphSON i .NET-objekt som vi kan arbeta med kod.</span><span class="sxs-lookup"><span data-stu-id="7c454-164">The Azure Cosmos DB .NET SDK includes JSON.NET as a dependency, and this allows us to serialize/deserialize GraphSON into .NET objects that we can work with in code.</span></span>

<span data-ttu-id="7c454-165">Exempelvis vi arbeta med ett enkelt sociala nätverk med fyra personer.</span><span class="sxs-lookup"><span data-stu-id="7c454-165">As an example, let's work with a simple social network with four people.</span></span> <span data-ttu-id="7c454-166">Vi titta på hur du skapar `Person` formhörnen, lägga till `Knows` relationer mellan dem, och sedan fråga och passerar i diagrammet för att söka efter ”vän vän” relationer.</span><span class="sxs-lookup"><span data-stu-id="7c454-166">We look at how to create `Person` vertices, add `Knows` relationships between them, then query and traverse the graph to find "friend of friend" relationships.</span></span> 

<span data-ttu-id="7c454-167">Den `Microsoft.Azure.Graphs.Elements` namnområde ger `Vertex`, `Edge`, `Property` och `VertexProperty` klasser för avserialisering av GraphSON svar väldefinierade .NET-objekt.</span><span class="sxs-lookup"><span data-stu-id="7c454-167">The `Microsoft.Azure.Graphs.Elements` namespace provides `Vertex`, `Edge`, `Property` and `VertexProperty` classes for deserializing GraphSON responses to well-defined .NET objects.</span></span>

## <a name="run-gremlin-using-creategremlinquery"></a><span data-ttu-id="7c454-168">Kör Gremlin med CreateGremlinQuery</span><span class="sxs-lookup"><span data-stu-id="7c454-168">Run Gremlin using CreateGremlinQuery</span></span>
<span data-ttu-id="7c454-169">Gremlin som SQL, stöder Läs-, Skriv- och frågor.</span><span class="sxs-lookup"><span data-stu-id="7c454-169">Gremlin, like SQL, supports read, write, and query operations.</span></span> <span data-ttu-id="7c454-170">Till exempel följande utdrag visar hur du skapar formhörnen kanter utföra vissa exempelfrågor som använder `CreateGremlinQuery<T>`, och asynkront gå igenom dessa resultat med hjälp av `ExecuteNextAsync` och ' HasMoreResults.</span><span class="sxs-lookup"><span data-stu-id="7c454-170">For example, the following snippet shows how to create vertices, edges, perform some sample queries using `CreateGremlinQuery<T>`, and asynchronously iterate through these results using `ExecuteNextAsync` and \`HasMoreResults.</span></span>

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

    // The CreateGremlinQuery method extensions allow you to execute Gremlin queries and iterate
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

## <a name="add-vertices-and-edges"></a><span data-ttu-id="7c454-171">Lägg till hörn och kanter</span><span class="sxs-lookup"><span data-stu-id="7c454-171">Add vertices and edges</span></span>

<span data-ttu-id="7c454-172">Nu ska vi titta på Gremlin-instruktioner som visas i föregående avsnitt detalj.</span><span class="sxs-lookup"><span data-stu-id="7c454-172">Let's look at the Gremlin statements shown in the preceding section more detail.</span></span> <span data-ttu-id="7c454-173">Första vi vissa formhörnen med Gremlin's `addV` metod.</span><span class="sxs-lookup"><span data-stu-id="7c454-173">First we some vertices using Gremlin's `addV` method.</span></span> <span data-ttu-id="7c454-174">Följande kodutdrag skapar en ”Thomas Andersen” nod av typen ”Person”, med egenskaper för förnamn, efternamn och ålder.</span><span class="sxs-lookup"><span data-stu-id="7c454-174">For example, the following snippet creates a "Thomas Andersen" vertex of type "Person", with properties for first name, last name, and age.</span></span>

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

<span data-ttu-id="7c454-175">Vi skapa vissa kanter mellan dessa formhörnen med Gremlin's `addE` metod.</span><span class="sxs-lookup"><span data-stu-id="7c454-175">Then we create some edges between these vertices using Gremlin's `addE` method.</span></span> 

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

<span data-ttu-id="7c454-176">Vi kan uppdatera en befintlig vertex med `properties` steg i Gremlin.</span><span class="sxs-lookup"><span data-stu-id="7c454-176">We can update an existing vertex by using `properties` step in Gremlin.</span></span> <span data-ttu-id="7c454-177">Vi hoppar över anropet för att köra frågan via `HasMoreResults` och `ExecuteNextAsync` för resten av exemplen.</span><span class="sxs-lookup"><span data-stu-id="7c454-177">We skip the call to execute the query via `HasMoreResults` and `ExecuteNextAsync` for the rest of the examples.</span></span>

```cs
// Update a vertex
client.CreateGremlinQuery<Vertex>(
    graphCollection, 
    "g.V('thomas').property('age', 45)");
```

<span data-ttu-id="7c454-178">Du kan släppa kanter och formhörnen med Gremlin's `drop` steg.</span><span class="sxs-lookup"><span data-stu-id="7c454-178">You can drop edges and vertices using Gremlin's `drop` step.</span></span> <span data-ttu-id="7c454-179">Här är ett kodfragment som visar hur du tar bort en nod och en kant.</span><span class="sxs-lookup"><span data-stu-id="7c454-179">Here's a snippet that shows how to delete a vertex and an edge.</span></span> <span data-ttu-id="7c454-180">Observera att släppa en nod utför borttagningsåtgärder associerade kanter.</span><span class="sxs-lookup"><span data-stu-id="7c454-180">Note that dropping a vertex performs a cascading delete of the associated edges.</span></span>

```cs
// Drop an edge
client.CreateGremlinQuery(graphCollection, "g.E('thomasKnowsRobin').drop()");

// Drop a vertex
client.CreateGremlinQuery(graphCollection, "g.V('robin').drop()");
```

## <a name="query-the-graph"></a><span data-ttu-id="7c454-181">Frågan i diagrammet</span><span class="sxs-lookup"><span data-stu-id="7c454-181">Query the graph</span></span>

<span data-ttu-id="7c454-182">Du kan utföra frågor och traversals som också använder Gremlin.</span><span class="sxs-lookup"><span data-stu-id="7c454-182">You can perform queries and traversals also using Gremlin.</span></span> <span data-ttu-id="7c454-183">Till exempel visar följande utdrag hur du räkna antalet formhörnen i diagrammet:</span><span class="sxs-lookup"><span data-stu-id="7c454-183">For example, the following snippet shows how to count the number of vertices in the graph:</span></span>

```cs
// Run a query to count vertices
IDocumentQuery<int> countQuery = client.CreateGremlinQuery<int>(graphCollection, "g.V().count()");
```
<span data-ttu-id="7c454-184">Du kan utföra filter med Gremlin's `has` och `hasLabel` steg och kombinera dem med hjälp av `and`, `or`, och `not` att skapa fler komplexa filter:</span><span class="sxs-lookup"><span data-stu-id="7c454-184">You can perform filters using Gremlin's `has` and `hasLabel` steps, and combine them using `and`, `or`, and `not` to build more complex filters:</span></span>

```cs
// Run a query with filter
IDocumentQuery<Vertex> personsByAge = client.CreateGremlinQuery<Vertex>(
  graphCollection, 
  "g.V().hasLabel('person').has('age', gt(40))");
```

<span data-ttu-id="7c454-185">Du kan projicera vissa egenskaper i frågeresultatet med hjälp av den `values` steg:</span><span class="sxs-lookup"><span data-stu-id="7c454-185">You can project certain properties in the query results using the `values` step:</span></span>

```cs
// Run a query with projection
IDocumentQuery<string> firstNames = client.CreateGremlinQuery<string>(
  graphCollection, 
  $"g.V().hasLabel('person').values('firstName')");
```

<span data-ttu-id="7c454-186">Hittills har sett vi endast frågeoperatorer som fungerar i en databas.</span><span class="sxs-lookup"><span data-stu-id="7c454-186">So far, we've only seen query operators that work in any database.</span></span> <span data-ttu-id="7c454-187">Diagram är snabb och effektiv för traversal åtgärder när du behöver att navigera till relaterade kanter och formhörnen.</span><span class="sxs-lookup"><span data-stu-id="7c454-187">Graphs are fast and efficient for traversal operations when you need to navigate to related edges and vertices.</span></span> <span data-ttu-id="7c454-188">Vi ska hitta alla vänner till Thomas.</span><span class="sxs-lookup"><span data-stu-id="7c454-188">Let's find all friends of Thomas.</span></span> <span data-ttu-id="7c454-189">Vi kan göra detta med hjälp av Gremlin's `outE` steg för att söka efter alla de kanter ut från Thomas och sedan bläddra till de i formhörnen från dessa kanter med Gremlin's `inV` steg:</span><span class="sxs-lookup"><span data-stu-id="7c454-189">We do this by using Gremlin's `outE` step to find all the out-edges from Thomas, then traversing to the in-vertices from those edges using Gremlin's `inV` step:</span></span>

```cs
// Run a traversal (find friends of Thomas)
IDocumentQuery<Vertex> friendsOfThomas = client.CreateGremlinQuery<Vertex>(
  graphCollection,
  "g.V('thomas').outE('knows').inV().hasLabel('person')");
```

<span data-ttu-id="7c454-190">Nästa fråga utför två hopp för att söka efter alla Thomas' ”vänner till vänner”, genom att anropa `outE` och `inV` två gånger.</span><span class="sxs-lookup"><span data-stu-id="7c454-190">The next query performs two hops to find all of Thomas' "friends of friends", by calling `outE` and `inV` two times.</span></span> 

```cs
// Run a traversal (find friends of friends of Thomas)
IDocumentQuery<Vertex> friendsOfFriendsOfThomas = client.CreateGremlinQuery<Vertex>(
  graphCollection,
  "g.V('thomas').outE('knows').inV().hasLabel('person').outE('knows').inV().hasLabel('person')");
```

<span data-ttu-id="7c454-191">Du kan skapa mer komplexa frågor och implementera kraftfulla diagrammet traversal logik med Gremlin, inklusive blanda filteruttryck, utför slingor med hjälp av den `loop` steg och implementera villkorlig navigering med hjälp av den `choose` steg.</span><span class="sxs-lookup"><span data-stu-id="7c454-191">You can build more complex queries and implement powerful graph traversal logic using Gremlin, including mixing filter expressions, performing looping using the `loop` step, and implementing conditional navigation using the `choose` step.</span></span> <span data-ttu-id="7c454-192">Mer information om vad du kan göra med [Gremlin stöd](gremlin-support.md)!</span><span class="sxs-lookup"><span data-stu-id="7c454-192">Learn more about what you can do with [Gremlin support](gremlin-support.md)!</span></span>

<span data-ttu-id="7c454-193">Det, självstudierna Azure Cosmos DB är klar!</span><span class="sxs-lookup"><span data-stu-id="7c454-193">That's it, this Azure Cosmos DB tutorial is complete!</span></span> 

## <a name="clean-up-resources"></a><span data-ttu-id="7c454-194">Rensa resurser</span><span class="sxs-lookup"><span data-stu-id="7c454-194">Clean up resources</span></span>

<span data-ttu-id="7c454-195">Om du inte planerar att använda den här appen mer följer du stegen nedan för att ta bort alla resurser som du har skapat i den här självstudiekursen på Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="7c454-195">If you're not going to continue to use this app, use the following steps to delete all resources created by this tutorial in the Azure portal.</span></span>  

1. <span data-ttu-id="7c454-196">Klicka på **Resursgrupper** på den vänstra menyn i Azure Portal och sedan på namnet på den resurs du skapade.</span><span class="sxs-lookup"><span data-stu-id="7c454-196">From the left-hand menu in the Azure portal, click **Resource groups** and then click the name of the resource you created.</span></span> 
2. <span data-ttu-id="7c454-197">På sidan med resursgrupper klickar du på **Ta bort**, skriver in namnet på resursen att ta bort i textrutan och klickar sedan på **Ta bort**.</span><span class="sxs-lookup"><span data-stu-id="7c454-197">On your resource group page, click **Delete**, type the name of the resource to delete in the text box, and then click **Delete**.</span></span>

## <a name="next-steps"></a><span data-ttu-id="7c454-198">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="7c454-198">Next Steps</span></span>

<span data-ttu-id="7c454-199">I den här självstudiekursen kommer du har gjort följande:</span><span class="sxs-lookup"><span data-stu-id="7c454-199">In this tutorial, you've done the following:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="7c454-200">Skapa ett Azure DB som Cosmos-konto</span><span class="sxs-lookup"><span data-stu-id="7c454-200">Created an Azure Cosmos DB account</span></span> 
> * <span data-ttu-id="7c454-201">Skapa ett diagram databas och en behållare</span><span class="sxs-lookup"><span data-stu-id="7c454-201">Created a graph database and container</span></span>
> * <span data-ttu-id="7c454-202">Serialiserade hörn och kanter till .NET-objekt</span><span class="sxs-lookup"><span data-stu-id="7c454-202">Serialized vertices and edges to .NET objects</span></span>
> * <span data-ttu-id="7c454-203">Tillagda hörn och kanter</span><span class="sxs-lookup"><span data-stu-id="7c454-203">Added vertices and edges</span></span>
> * <span data-ttu-id="7c454-204">Frågas diagrammet med Gremlin</span><span class="sxs-lookup"><span data-stu-id="7c454-204">Queried the graph using Gremlin</span></span>

<span data-ttu-id="7c454-205">Nu kan du skapa mer komplexa frågor och implementera kraftfull logik för grafbläddring med Gremlin.</span><span class="sxs-lookup"><span data-stu-id="7c454-205">You can now build more complex queries and implement powerful graph traversal logic using Gremlin.</span></span> 

> [!div class="nextstepaction"]
> [<span data-ttu-id="7c454-206">Fråga med hjälp av Gremlin</span><span class="sxs-lookup"><span data-stu-id="7c454-206">Query using Gremlin</span></span>](tutorial-query-graph.md)
