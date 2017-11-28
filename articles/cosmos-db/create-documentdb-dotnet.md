---
title: 'Azure Cosmos DB: Skapa en webbapp med .NET och hello DocumentDB API | Microsoft Docs'
description: "Anger en .NET-kodexempel som du kan använda tooconnect tooand query hello Azure Cosmos DB DocumentDB API"
services: cosmos-db
documentationcenter: 
author: mimig1
manager: jhubbard
editor: 
ms.assetid: 
ms.service: cosmos-db
ms.custom: quick start connect, mvc
ms.workload: 
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 05/10/2017
ms.author: mimig
ms.openlocfilehash: 35517e35d80c48662a51a99814652ffa1121fc5d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-cosmos-db-build-a-documentdb-api-web-app-with-net-and-hello-azure-portal"></a><span data-ttu-id="bf980-103">Azure DB Cosmos: Skapa en DocumentDB-API-webbapp med .NET och hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="bf980-103">Azure Cosmos DB: Build a DocumentDB API web app with .NET and hello Azure portal</span></span>

<span data-ttu-id="bf980-104">Azure Cosmos DB är Microsofts globalt distribuerade databastjänst för flera datamodeller.</span><span class="sxs-lookup"><span data-stu-id="bf980-104">Azure Cosmos DB is Microsoft’s globally distributed multi-model database service.</span></span> <span data-ttu-id="bf980-105">Du kan snabbt skapa och fråga dokument och nyckel/värde-diagrammet databaser, som omfattas av hello global distributionsplatsen och skala horisontellt funktionerna i hello kärnan i Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="bf980-105">You can quickly create and query document, key/value, and graph databases, all of which benefit from hello global distribution and horizontal scale capabilities at hello core of Azure Cosmos DB.</span></span> 

<span data-ttu-id="bf980-106">Den här snabbstartsguide visar hur toocreate ett konto i Azure Cosmos DB, dokumentdatabasen och samlingen använder hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="bf980-106">This quick start demonstrates how toocreate an Azure Cosmos DB account, document database, and collection using hello Azure portal.</span></span> <span data-ttu-id="bf980-107">Du sedan skapa och distribuera en todo lista-webbapp som bygger på hello [DocumentDB .NET API](documentdb-sdk-dotnet.md)som visas i följande skärmbild hello.</span><span class="sxs-lookup"><span data-stu-id="bf980-107">You'll then build and deploy a todo list web app built on hello [DocumentDB .NET API](documentdb-sdk-dotnet.md), as shown in hello following screenshot.</span></span> 

![Att göra-app med exempeldata](./media/create-documentdb-dotnet/azure-comosdb-todo-app-list.png)

## <a name="prerequisites"></a><span data-ttu-id="bf980-109">Krav</span><span class="sxs-lookup"><span data-stu-id="bf980-109">Prerequisites</span></span>

<span data-ttu-id="bf980-110">Om du inte redan har Visual Studio 2017 installerat, du kan hämta och använda hello **ledigt** [Visual Studio 2017 Community Edition](https://www.visualstudio.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="bf980-110">If you don’t already have Visual Studio 2017 installed, you can download and use hello **free** [Visual Studio 2017 Community Edition](https://www.visualstudio.com/downloads/).</span></span> <span data-ttu-id="bf980-111">Kontrollera att du aktiverar **Azure-utveckling** under installationen av hello Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="bf980-111">Make sure that you enable **Azure development** during hello Visual Studio setup.</span></span>

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

<a id="create-account"></a>
## <a name="create-a-database-account"></a><span data-ttu-id="bf980-112">Skapa ett databaskonto</span><span class="sxs-lookup"><span data-stu-id="bf980-112">Create a database account</span></span>

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

<a id="create-collection"></a>
## <a name="add-a-collection"></a><span data-ttu-id="bf980-113">Lägga till en samling</span><span class="sxs-lookup"><span data-stu-id="bf980-113">Add a collection</span></span>

[!INCLUDE [cosmos-db-create-collection](../../includes/cosmos-db-create-collection.md)]

<a id="add-sample-data"></a>
## <a name="add-sample-data"></a><span data-ttu-id="bf980-114">Lägg till exempeldata</span><span class="sxs-lookup"><span data-stu-id="bf980-114">Add sample data</span></span>

<span data-ttu-id="bf980-115">Du kan nu lägga till tooyour nya insamling av data med hjälp av Data Explorer.</span><span class="sxs-lookup"><span data-stu-id="bf980-115">You can now add data tooyour new collection using Data Explorer.</span></span>

1. <span data-ttu-id="bf980-116">I Data Explorer visas hello ny databas i hello samlingar rutan.</span><span class="sxs-lookup"><span data-stu-id="bf980-116">In Data Explorer, hello new database appears in hello Collections pane.</span></span> <span data-ttu-id="bf980-117">Expandera hello **uppgifter** databas, expandera hello **objekt** samlingen, klickar du på **dokument**, och klicka sedan på **nya dokument**.</span><span class="sxs-lookup"><span data-stu-id="bf980-117">Expand hello **Tasks** database, expand hello **Items** collection, click **Documents**, and then click **New Documents**.</span></span> 

   ![Skapa nya dokument i Data Explorer i hello Azure-portalen](./media/create-documentdb-dotnet/azure-cosmosdb-data-explorer-new-document.png)
  
2. <span data-ttu-id="bf980-119">Lägg till en dokumentsamling toohello med följande struktur hello.</span><span class="sxs-lookup"><span data-stu-id="bf980-119">Now add a document toohello collection with hello following structure.</span></span>

     ```json
     {
         "id": "1",
         "category": "personal",
         "name": "groceries",
         "description": "Pick up apples and strawberries.",
         "isComplete": false
     }
     ```

3. <span data-ttu-id="bf980-120">När du har lagt till hello json toohello **dokument** klickar du på **spara**.</span><span class="sxs-lookup"><span data-stu-id="bf980-120">Once you've added hello json toohello **Documents** tab, click **Save**.</span></span>

    ![Kopiera i json-data och klicka på Spara i Data Explorer i hello Azure-portalen](./media/create-documentdb-dotnet/azure-cosmosdb-data-explorer-save-document.png)

4.  <span data-ttu-id="bf980-122">Skapa och spara en mer dokument där du infoga ett unikt värde för hello `id` egenskap och hello andra egenskaper som du vill ändra.</span><span class="sxs-lookup"><span data-stu-id="bf980-122">Create and save one more document where you insert a unique value for hello `id` property, and change hello other properties as you see fit.</span></span> <span data-ttu-id="bf980-123">Dina nya dokument kan ha vilken struktur du vill eftersom Azure Cosmos DB inte kräver något schema för dina data.</span><span class="sxs-lookup"><span data-stu-id="bf980-123">Your new documents can have any structure you want as Azure Cosmos DB doesn't impose any schema on your data.</span></span>

     <span data-ttu-id="bf980-124">Du kan nu använda frågor i Data Explorer tooretrieve dina data.</span><span class="sxs-lookup"><span data-stu-id="bf980-124">You can now use queries in Data Explorer tooretrieve your data.</span></span> <span data-ttu-id="bf980-125">Som standard använder Data Explorer `SELECT * FROM c` tooretrieve alla dokument i hello samlingen, men du kan ändra den olika tooa [SQL-frågan](documentdb-sql-query.md), som `SELECT * FROM c ORDER BY c._ts DESC`, tooreturn alla hello dokument i fallande ordning utifrån tidsstämpel.</span><span class="sxs-lookup"><span data-stu-id="bf980-125">By default, Data Explorer uses `SELECT * FROM c` tooretrieve all documents in hello collection, but you can change that tooa different [SQL query](documentdb-sql-query.md), such as `SELECT * FROM c ORDER BY c._ts DESC`, tooreturn all hello documents in descending order based on their timestamp.</span></span>
 
     <span data-ttu-id="bf980-126">Du kan också använda Data Explorer toocreate lagrade procedurer, UDF: er och utlösare tooperform serversidan affärslogik samt som skala genomflöde.</span><span class="sxs-lookup"><span data-stu-id="bf980-126">You can also use Data Explorer toocreate stored procedures, UDFs, and triggers tooperform server-side business logic as well as scale throughput.</span></span> <span data-ttu-id="bf980-127">Data Explorer visar alla hello inbyggda programmatisk åtkomst till data i hello API: er, men ger enkel åtkomst tooyour data i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="bf980-127">Data Explorer exposes all of hello built-in programmatic data access available in hello APIs, but provides easy access tooyour data in hello Azure portal.</span></span>

## <a name="clone-hello-sample-application"></a><span data-ttu-id="bf980-128">Klona hello exempelprogrammet</span><span class="sxs-lookup"><span data-stu-id="bf980-128">Clone hello sample application</span></span>

<span data-ttu-id="bf980-129">Nu ska vi växla tooworking med kod.</span><span class="sxs-lookup"><span data-stu-id="bf980-129">Now let's switch tooworking with code.</span></span> <span data-ttu-id="bf980-130">Vi ska klona en DocumentDB-API-app från GitHub, ange hello anslutningssträngen och kör den.</span><span class="sxs-lookup"><span data-stu-id="bf980-130">Let's clone a DocumentDB API app from GitHub, set hello connection string, and run it.</span></span> <span data-ttu-id="bf980-131">Du ser hur enkelt som det är att toowork med data programmässigt.</span><span class="sxs-lookup"><span data-stu-id="bf980-131">You'll see how easy it is toowork with data programmatically.</span></span> 

1. <span data-ttu-id="bf980-132">Öppna ett git terminalfönster, till exempel git bash och `CD` tooa arbetskatalogen.</span><span class="sxs-lookup"><span data-stu-id="bf980-132">Open a git terminal window, such as git bash, and `CD` tooa working directory.</span></span>  

2. <span data-ttu-id="bf980-133">Hello kör följande kommando tooclone hello exempel lagringsplatsen.</span><span class="sxs-lookup"><span data-stu-id="bf980-133">Run hello following command tooclone hello sample repository.</span></span> 

    ```bash
    git clone https://github.com/Azure-Samples/documentdb-dotnet-todo-app.git
    ```

3. <span data-ttu-id="bf980-134">Öppna sedan hello todo lösningsfilen i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="bf980-134">Then open hello todo solution file in Visual Studio.</span></span> 

## <a name="review-hello-code"></a><span data-ttu-id="bf980-135">Granska hello kod</span><span class="sxs-lookup"><span data-stu-id="bf980-135">Review hello code</span></span>

<span data-ttu-id="bf980-136">Låt oss göra en snabb genomgång av vad som händer i hello app.</span><span class="sxs-lookup"><span data-stu-id="bf980-136">Let's make a quick review of what's happening in hello app.</span></span> <span data-ttu-id="bf980-137">Öppna hello DocumentDBRepository.cs fil- och du hittar att dessa rader med kod skapar hello Azure Cosmos DB resurser.</span><span class="sxs-lookup"><span data-stu-id="bf980-137">Open hello DocumentDBRepository.cs file and you'll find that these lines of code create hello Azure Cosmos DB resources.</span></span> 

* <span data-ttu-id="bf980-138">Hej DocumentClient har initierats på rad 73.</span><span class="sxs-lookup"><span data-stu-id="bf980-138">hello DocumentClient is initialized on line 73.</span></span>

    ```csharp
    client = new DocumentClient(new Uri(ConfigurationManager.AppSettings["endpoint"]), ConfigurationManager.AppSettings["authKey"]);`
    ```

* <span data-ttu-id="bf980-139">En ny databas skapas på rad 88.</span><span class="sxs-lookup"><span data-stu-id="bf980-139">A new database is created on line 88.</span></span>

    ```csharp
    await client.CreateDatabaseAsync(new Database { Id = DatabaseId });
    ```

* <span data-ttu-id="bf980-140">En ny samling skapas på rad 107.</span><span class="sxs-lookup"><span data-stu-id="bf980-140">A new collection is created on line 107.</span></span>

    ```csharp
    await client.CreateDocumentCollectionAsync(
        UriFactory.CreateDatabaseUri(DatabaseId),
        new DocumentCollection { Id = CollectionId },
        new RequestOptions { OfferThroughput = 1000 });
    ```

## <a name="update-your-connection-string"></a><span data-ttu-id="bf980-141">Uppdatera din anslutningssträng</span><span class="sxs-lookup"><span data-stu-id="bf980-141">Update your connection string</span></span>

<span data-ttu-id="bf980-142">Gå tillbaka toohello Azure portal tooget din Anslutningssträngsinformation nu och kopierar den till hello app.</span><span class="sxs-lookup"><span data-stu-id="bf980-142">Now go back toohello Azure portal tooget your connection string information and copy it into hello app.</span></span>

1. <span data-ttu-id="bf980-143">I hello [Azure-portalen](http://portal.azure.com/), i din Azure Cosmos DB konto, i hello vänstra navigeringsrutan klickar du på **nycklar**, och klicka sedan på **skrivskyddad nycklar**.</span><span class="sxs-lookup"><span data-stu-id="bf980-143">In hello [Azure portal](http://portal.azure.com/), in your Azure Cosmos DB account, in hello left navigation click **Keys**, and then click **Read-write Keys**.</span></span> <span data-ttu-id="bf980-144">I hello web.config-fil i hello nästa steg ska du använda hello kopiera knappar hello höger på hello skärmen toocopy hello URI och primärnyckel.</span><span class="sxs-lookup"><span data-stu-id="bf980-144">You'll use hello copy buttons on hello right side of hello screen toocopy hello URI and Primary Key into hello web.config file in hello next step.</span></span>

    ![Visa och kopiera en snabbtangent i hello Azure-portalen, bladet nycklar](./media/create-documentdb-dotnet/keys.png)

2. <span data-ttu-id="bf980-146">Öppna hello web.config-filen i Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="bf980-146">In Visual Studio 2017, open hello web.config file.</span></span> 

3. <span data-ttu-id="bf980-147">Kopiera URI-värdet från hello-portal (med hello kopieringsknappen) och gör den hello värdet för hello endpoint nyckeln i web.config.</span><span class="sxs-lookup"><span data-stu-id="bf980-147">Copy your URI value from hello portal (using hello copy button) and make it hello value of hello endpoint key in web.config.</span></span> 

    `<add key="endpoint" value="FILLME" />`

4. <span data-ttu-id="bf980-148">Kopiera värdet för PRIMÄRNYCKELN från hello-portalen och gör det hello värdet för hello auktoriseringsnyckel i web.config. Du har nu uppdaterat din app med alla hello information som behövs för toocommunicate med Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="bf980-148">Then copy your PRIMARY KEY value from hello portal and make it hello value of hello authKey in web.config. You've now updated your app with all hello info it needs toocommunicate with Azure Cosmos DB.</span></span> 

    `<add key="authKey" value="FILLME" />`
    
## <a name="run-hello-web-app"></a><span data-ttu-id="bf980-149">Köra hello-webbprogram</span><span class="sxs-lookup"><span data-stu-id="bf980-149">Run hello web app</span></span>
1. <span data-ttu-id="bf980-150">Högerklicka på hello-projekt i Visual Studio **Solution Explorer** och klicka sedan på **hantera NuGet-paket**.</span><span class="sxs-lookup"><span data-stu-id="bf980-150">In Visual Studio, right-click on hello project in **Solution Explorer** and then click **Manage NuGet Packages**.</span></span> 

2. <span data-ttu-id="bf980-151">I hello NuGet **Bläddra** skriver *DocumentDB*.</span><span class="sxs-lookup"><span data-stu-id="bf980-151">In hello NuGet **Browse** box, type *DocumentDB*.</span></span>

3. <span data-ttu-id="bf980-152">Hello resultat för att installera hello **Microsoft.Azure.DocumentDB** bibliotek.</span><span class="sxs-lookup"><span data-stu-id="bf980-152">From hello results, install hello **Microsoft.Azure.DocumentDB** library.</span></span> <span data-ttu-id="bf980-153">Detta installerar hello Microsoft.Azure.DocumentDB paket samt alla beroenden.</span><span class="sxs-lookup"><span data-stu-id="bf980-153">This installs hello Microsoft.Azure.DocumentDB package as well as all dependencies.</span></span>

4. <span data-ttu-id="bf980-154">Klicka på CTRL + F5 toorun hello program.</span><span class="sxs-lookup"><span data-stu-id="bf980-154">Click CTRL + F5 toorun hello application.</span></span> <span data-ttu-id="bf980-155">Appen visas i din webbläsare.</span><span class="sxs-lookup"><span data-stu-id="bf980-155">Your app displays in your browser.</span></span> 

5. <span data-ttu-id="bf980-156">Klicka på **Skapa nytt** i hello webbläsare och skapa några nya aktiviteter i din att göra-app.</span><span class="sxs-lookup"><span data-stu-id="bf980-156">Click **Create New** in hello browser and create a few new tasks in your to-do app.</span></span>

   ![Att göra-app med exempeldata](./media/create-documentdb-dotnet/azure-comosdb-todo-app-list.png)

<span data-ttu-id="bf980-158">Du kan nu gå tillbaka tooData Explorer Se fråga, ändra och arbeta med dessa nya data.</span><span class="sxs-lookup"><span data-stu-id="bf980-158">You can now go back tooData Explorer and see query, modify, and work with this new data.</span></span> 

## <a name="review-slas-in-hello-azure-portal"></a><span data-ttu-id="bf980-159">Granska SLA: er i hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="bf980-159">Review SLAs in hello Azure portal</span></span>

[!INCLUDE [cosmosdb-tutorial-review-slas](../../includes/cosmos-db-tutorial-review-slas.md)]

## <a name="clean-up-resources"></a><span data-ttu-id="bf980-160">Rensa resurser</span><span class="sxs-lookup"><span data-stu-id="bf980-160">Clean up resources</span></span>

<span data-ttu-id="bf980-161">Om du inte kommer toocontinue toouse den här appen, tar du bort alla resurser som skapats av denna Snabbstart i hello Azure-portalen med hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="bf980-161">If you're not going toocontinue toouse this app, delete all resources created by this quickstart in hello Azure portal with hello following steps:</span></span>

1. <span data-ttu-id="bf980-162">Hello vänstra menyn i hello Azure-portalen klickar du på **resursgrupper** och klicka sedan på hello namnet på hello resurs du skapat.</span><span class="sxs-lookup"><span data-stu-id="bf980-162">From hello left-hand menu in hello Azure portal, click **Resource groups** and then click hello name of hello resource you created.</span></span> 
2. <span data-ttu-id="bf980-163">På din resurs gruppen klickar du på **ta bort**typnamn hello för hello resurs toodelete i hello textrutan och klicka sedan på **ta bort**.</span><span class="sxs-lookup"><span data-stu-id="bf980-163">On your resource group page, click **Delete**, type hello name of hello resource toodelete in hello text box, and then click **Delete**.</span></span>

## <a name="next-steps"></a><span data-ttu-id="bf980-164">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="bf980-164">Next steps</span></span>

<span data-ttu-id="bf980-165">Du har lärt dig hur toocreate ett Azure DB som Cosmos-konto, skapa en samling med hello Data Explorer och kör ett webbprogram i denna Snabbstart.</span><span class="sxs-lookup"><span data-stu-id="bf980-165">In this quickstart, you've learned how toocreate an Azure Cosmos DB account, create a collection using hello Data Explorer, and run a web app.</span></span> <span data-ttu-id="bf980-166">Nu kan du importera ytterligare data tooyour Cosmos-DB-konto.</span><span class="sxs-lookup"><span data-stu-id="bf980-166">You can now import additional data tooyour Cosmos DB account.</span></span> 

> [!div class="nextstepaction"]
> [<span data-ttu-id="bf980-167">Importera data till Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="bf980-167">Import data into Azure Cosmos DB</span></span>](import-data.md)


