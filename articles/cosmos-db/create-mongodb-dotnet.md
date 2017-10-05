---
title: 'Azure Cosmos DB: Skapa en webbapp med .NET och MongoDB-API:t | Microsoft Docs'
description: "Presenterar ett .NET-kodexempel som du kan använda för att ansluta till och ställa frågor via Azure Cosmos DB MongoDB-API:t"
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
ms.openlocfilehash: 2d30bec75d701b1fd55355d1e139350b6d828c9a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="azure-cosmos-db-build-a-mongodb-api-web-app-with-net-and-the-azure-portal"></a><span data-ttu-id="99033-103">Azure Cosmos DB: skapa en MongoDB-API-webbapp med .NET och Azure Portal</span><span class="sxs-lookup"><span data-stu-id="99033-103">Azure Cosmos DB: Build a MongoDB API web app with .NET and the Azure portal</span></span>

<span data-ttu-id="99033-104">Azure Cosmos DB är Microsofts globalt distribuerade databastjänst för flera datamodeller.</span><span class="sxs-lookup"><span data-stu-id="99033-104">Azure Cosmos DB is Microsoft’s globally distributed multi-model database service.</span></span> <span data-ttu-id="99033-105">Du kan snabbt skapa och ställa frågor mot databaser med dokument, nyckel/värde-par och grafer. Du får fördelar av den globala distributionen och den horisontella skalningsförmågan som ligger i grunden hos Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="99033-105">You can quickly create and query document, key/value, and graph databases, all of which benefit from the global distribution and horizontal scale capabilities at the core of Azure Cosmos DB.</span></span> 

<span data-ttu-id="99033-106">I den här snabbstarten visas hur du skapar ett Azure Cosmos DB-konto, en dokumentdatabas och en samling med hjälp av Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="99033-106">This quick start demonstrates how to create an Azure Cosmos DB account, document database, and collection using the Azure portal.</span></span> <span data-ttu-id="99033-107">Sedan kommer du att skapa och distribuera en uppgiftslistewebbapp som är byggd med [MondoDB .NET-drivrutinen](https://docs.mongodb.com/ecosystem/drivers/csharp/).</span><span class="sxs-lookup"><span data-stu-id="99033-107">You'll then build and deploy a tasks list web app built on the [MongoDB .NET driver](https://docs.mongodb.com/ecosystem/drivers/csharp/).</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="99033-108">Krav</span><span class="sxs-lookup"><span data-stu-id="99033-108">Prerequisites</span></span>

<span data-ttu-id="99033-109">Om du inte har Visual Studio 2017 installerad kan du ladda ned och använda [Visual Studio 2017 Community Edition](https://www.visualstudio.com/downloads/) **utan kostnad**.</span><span class="sxs-lookup"><span data-stu-id="99033-109">If you don’t already have Visual Studio 2017 installed, you can download and use the **free** [Visual Studio 2017 Community Edition](https://www.visualstudio.com/downloads/).</span></span> <span data-ttu-id="99033-110">Se till att du aktiverar **Azure-utveckling** under installationen av Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="99033-110">Make sure that you enable **Azure development** during the Visual Studio setup.</span></span>
[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

<a id="create-account"></a>
## <a name="create-a-database-account"></a><span data-ttu-id="99033-111">Skapa ett databaskonto</span><span class="sxs-lookup"><span data-stu-id="99033-111">Create a database account</span></span>

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount-mongodb.md)]

## <a name="clone-the-sample-application"></a><span data-ttu-id="99033-112">Klona exempelprogrammet</span><span class="sxs-lookup"><span data-stu-id="99033-112">Clone the sample application</span></span>

<span data-ttu-id="99033-113">Nu ska vi klona en MongoDB API-app från github, ange anslutningssträngen och köra appen.</span><span class="sxs-lookup"><span data-stu-id="99033-113">Now let's clone a MongoDB API app from github, set the connection string, and run it.</span></span> <span data-ttu-id="99033-114">Du kommer att se hur lätt det är att arbeta med data programmässigt.</span><span class="sxs-lookup"><span data-stu-id="99033-114">You'll see how easy it is to work with data programmatically.</span></span> 

1. <span data-ttu-id="99033-115">Öppna ett git-terminalfönster, till exempel git bash, och `cd` till en arbetskatalog.</span><span class="sxs-lookup"><span data-stu-id="99033-115">Open a git terminal window, such as git bash, and `cd` to a working directory.</span></span>  

2. <span data-ttu-id="99033-116">Klona exempellagringsplatsen med följande kommando.</span><span class="sxs-lookup"><span data-stu-id="99033-116">Run the following command to clone the sample repository.</span></span> 

    ```bash
    git clone https://github.com/Azure-Samples/azure-cosmos-db-mongodb-dotnet-getting-started.git
    ```

3. <span data-ttu-id="99033-117">Öppna därefter lösningsfilen i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="99033-117">Then open the solution file in Visual Studio.</span></span> 

## <a name="review-the-code"></a><span data-ttu-id="99033-118">Granska koden</span><span class="sxs-lookup"><span data-stu-id="99033-118">Review the code</span></span>

<span data-ttu-id="99033-119">Vi gör en snabb genomgång av vad som händer i appen.</span><span class="sxs-lookup"><span data-stu-id="99033-119">Let's make a quick review of what's happening in the app.</span></span> <span data-ttu-id="99033-120">Öppna filen **Dal.cs** under katalogen **DAL** så ser du att de här kodraderna skapar Azure Cosmos DB-resurserna.</span><span class="sxs-lookup"><span data-stu-id="99033-120">Open the **Dal.cs** file under the **DAL** directory and you'll find that these lines of code create the Azure Cosmos DB resources.</span></span> 

* <span data-ttu-id="99033-121">Initiera Mongo-klienten.</span><span class="sxs-lookup"><span data-stu-id="99033-121">Initialize the Mongo Client.</span></span>

    ```cs
        MongoClientSettings settings = new MongoClientSettings();
        settings.Server = new MongoServerAddress(host, 10255);
        settings.UseSsl = true;
        settings.SslSettings = new SslSettings();
        settings.SslSettings.EnabledSslProtocols = SslProtocols.Tls12;

        MongoIdentity identity = new MongoInternalIdentity(dbName, userName);
        MongoIdentityEvidence evidence = new PasswordEvidence(password);

        settings.Credentials = new List<MongoCredential>()
        {
            new MongoCredential("SCRAM-SHA-1", identity, evidence)
        };

        MongoClient client = new MongoClient(settings);
    ```

* <span data-ttu-id="99033-122">Hämta databasen och samlingen.</span><span class="sxs-lookup"><span data-stu-id="99033-122">Retrieve the database and the collection.</span></span>

    ```cs
    private string dbName = "Tasks";
    private string collectionName = "TasksList";

    var database = client.GetDatabase(dbName);
    var todoTaskCollection = database.GetCollection<MyTask>(collectionName);
    ```

* <span data-ttu-id="99033-123">Hämta alla dokument.</span><span class="sxs-lookup"><span data-stu-id="99033-123">Retrieve all documents.</span></span>

    ```cs
    collection.Find(new BsonDocument()).ToList();
    ```

## <a name="update-your-connection-string"></a><span data-ttu-id="99033-124">Uppdatera din anslutningssträng</span><span class="sxs-lookup"><span data-stu-id="99033-124">Update your connection string</span></span>

<span data-ttu-id="99033-125">Gå nu tillbaka till Azure Portal för att hämta information om din anslutningssträng och kopiera den till appen.</span><span class="sxs-lookup"><span data-stu-id="99033-125">Now go back to the Azure portal to get your connection string information and copy it into the app.</span></span>

1. <span data-ttu-id="99033-126">Öppna ditt Azure Cosmos DB-konto i [Azure Portal](http://portal.azure.com/), klicka på **Anslutningssträng** och därefter på **Läs- och skrivnycklar**.</span><span class="sxs-lookup"><span data-stu-id="99033-126">In the [Azure portal](http://portal.azure.com/), in your Azure Cosmos DB account, in the left navigation click **Connection String**, and then click **Read-write Keys**.</span></span> <span data-ttu-id="99033-127">Använd kopieringsknapparna till höger på skärmen och kopiera Användarnamn, Lösenord och Värd till filen Dal.cs i nästa steg.</span><span class="sxs-lookup"><span data-stu-id="99033-127">You'll use the copy buttons on the right side of the screen to copy the Username, Password, and Host into the Dal.cs file in the next step.</span></span>

2. <span data-ttu-id="99033-128">Öppna filen **Dal.cs** i katalogen **DAL**.</span><span class="sxs-lookup"><span data-stu-id="99033-128">Open the **Dal.cs** file in the **DAL** directory.</span></span> 

3. <span data-ttu-id="99033-129">Kopiera ditt **användarnamn** från portalen (med kopieringsknappen) och gör det till värdet för **username** i filen **Dal.cs**.</span><span class="sxs-lookup"><span data-stu-id="99033-129">Copy your **username** value from the portal (using the copy button) and make it the value of the **username** in your **Dal.cs** file.</span></span> 

4. <span data-ttu-id="99033-130">Kopiera sedan **värden** från portalen och gör den till värdet för **host** i filen **Dal.cs**.</span><span class="sxs-lookup"><span data-stu-id="99033-130">Then copy your **host** value from the portal and make it the value of the **host** in your **Dal.cs** file.</span></span> 

5. <span data-ttu-id="99033-131">Kopiera slutligen **lösenordet** från portalen och gör den till värdet för **password** i filen **Dal.cs**.</span><span class="sxs-lookup"><span data-stu-id="99033-131">Finally copy your **password** value from the portal and make it the value of the **password** in your **Dal.cs** file.</span></span> 

<span data-ttu-id="99033-132">Du har nu uppdaterat appen med all information som behövs för kommunikation med Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="99033-132">You've now updated your app with all the info it needs to communicate with Azure Cosmos DB.</span></span> 
    
## <a name="run-the-web-app"></a><span data-ttu-id="99033-133">Kör webbappen</span><span class="sxs-lookup"><span data-stu-id="99033-133">Run the web app</span></span>

1. <span data-ttu-id="99033-134">I Visual Studio högerklickar du på projektet i **Solution Explorer** och därefter på **Hantera NuGet-paket**.</span><span class="sxs-lookup"><span data-stu-id="99033-134">In Visual Studio, right-click on the project in **Solution Explorer** and then click **Manage NuGet Packages**.</span></span> 

2. <span data-ttu-id="99033-135">I NuGet-rutan **Bläddra** skriver du in *MongoDB.Driver*.</span><span class="sxs-lookup"><span data-stu-id="99033-135">In the NuGet **Browse** box, type *MongoDB.Driver*.</span></span>

3. <span data-ttu-id="99033-136">Installera biblioteket **MongoDB.Driver** från resultaten.</span><span class="sxs-lookup"><span data-stu-id="99033-136">From the results, install the **MongoDB.Driver** library.</span></span> <span data-ttu-id="99033-137">Det här installerar MongoDB.Driver-paketet samt alla beroenden.</span><span class="sxs-lookup"><span data-stu-id="99033-137">This installs the MongoDB.Driver package as well as all dependencies.</span></span>

4. <span data-ttu-id="99033-138">Tryck på Ctrl + F5 för att köra programmet.</span><span class="sxs-lookup"><span data-stu-id="99033-138">Click CTRL + F5 to run the application.</span></span> <span data-ttu-id="99033-139">Appen visas i din webbläsare.</span><span class="sxs-lookup"><span data-stu-id="99033-139">Your app displays in your browser.</span></span> 

5. <span data-ttu-id="99033-140">Klicka på **Skapa** i webbläsaren och skapa några nya uppgifter i din uppgiftslisteapp.</span><span class="sxs-lookup"><span data-stu-id="99033-140">Click **Create** in the browser and create a few new tasks in your task list app.</span></span>

## <a name="review-slas-in-the-azure-portal"></a><span data-ttu-id="99033-141">Granska serviceavtal i Azure Portal</span><span class="sxs-lookup"><span data-stu-id="99033-141">Review SLAs in the Azure portal</span></span>

[!INCLUDE [cosmosdb-tutorial-review-slas](../../includes/cosmos-db-tutorial-review-slas.md)]

## <a name="clean-up-resources"></a><span data-ttu-id="99033-142">Rensa resurser</span><span class="sxs-lookup"><span data-stu-id="99033-142">Clean up resources</span></span>

<span data-ttu-id="99033-143">Om du inte planerar att fortsätta använda den här appen tar du bort alla resurser som skapades i snabbstarten i Azure Portal med följande steg:</span><span class="sxs-lookup"><span data-stu-id="99033-143">If you're not going to continue to use this app, delete all resources created by this quickstart in the Azure portal with the following steps:</span></span>

1. <span data-ttu-id="99033-144">Klicka på **Resursgrupper** på den vänstra menyn i Azure Portal och sedan på namnet på den resurs du skapade.</span><span class="sxs-lookup"><span data-stu-id="99033-144">From the left-hand menu in the Azure portal, click **Resource groups** and then click the name of the resource you created.</span></span> 
2. <span data-ttu-id="99033-145">På sidan med resursgrupper klickar du på **Ta bort**, skriver in namnet på resursen att ta bort i textrutan och klickar sedan på **Ta bort**.</span><span class="sxs-lookup"><span data-stu-id="99033-145">On your resource group page, click **Delete**, type the name of the resource to delete in the text box, and then click **Delete**.</span></span>

## <a name="next-steps"></a><span data-ttu-id="99033-146">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="99033-146">Next steps</span></span>

<span data-ttu-id="99033-147">I den här snabbstarten har du lärt dig hur du skapar ett Azure Cosmos DB-konto och kör en webbapp via API:t för MongoDB.</span><span class="sxs-lookup"><span data-stu-id="99033-147">In this quickstart, you've learned how to create an Azure Cosmos DB account and run a web app using the API for MongoDB.</span></span> <span data-ttu-id="99033-148">Du kan nu importera ytterligare data till ditt Cosmos DB-konto.</span><span class="sxs-lookup"><span data-stu-id="99033-148">You can now import additional data to your Cosmos DB account.</span></span> 

> [!div class="nextstepaction"]
> [<span data-ttu-id="99033-149">Importera data till Azure Cosmos DB för användning med MongoDB-API:t</span><span class="sxs-lookup"><span data-stu-id="99033-149">Import data into Azure Cosmos DB for the MongoDB API</span></span>](mongodb-migrate.md)

