---
title: 'Azure Cosmos DB: Skapa en webbapp med .NET och hello MongoDB API | Microsoft Docs'
description: "Anger en .NET-kodexempel som du kan använda tooconnect tooand query hello Azure Cosmos DB MongoDB API"
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
ms.openlocfilehash: c85cc47f772a19aaa7181611b75a8acaedbc4c42
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-cosmos-db-build-a-mongodb-api-web-app-with-net-and-hello-azure-portal"></a><span data-ttu-id="a0f81-103">Azure DB Cosmos: Skapa en webbapp i MongoDB-API med .NET och hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="a0f81-103">Azure Cosmos DB: Build a MongoDB API web app with .NET and hello Azure portal</span></span>

<span data-ttu-id="a0f81-104">Azure Cosmos DB är Microsofts globalt distribuerade databastjänst för flera datamodeller.</span><span class="sxs-lookup"><span data-stu-id="a0f81-104">Azure Cosmos DB is Microsoft’s globally distributed multi-model database service.</span></span> <span data-ttu-id="a0f81-105">Du kan snabbt skapa och fråga dokument och nyckel/värde-diagrammet databaser, som omfattas av hello global distributionsplatsen och skala horisontellt funktionerna i hello kärnan i Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="a0f81-105">You can quickly create and query document, key/value, and graph databases, all of which benefit from hello global distribution and horizontal scale capabilities at hello core of Azure Cosmos DB.</span></span> 

<span data-ttu-id="a0f81-106">Den här snabbstartsguide visar hur toocreate ett konto i Azure Cosmos DB, dokumentdatabasen och samlingen använder hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="a0f81-106">This quick start demonstrates how toocreate an Azure Cosmos DB account, document database, and collection using hello Azure portal.</span></span> <span data-ttu-id="a0f81-107">Du sedan skapa och distribuera en uppgifter lista-webbapp som bygger på hello [MongoDB .NET drivrutinen](https://docs.mongodb.com/ecosystem/drivers/csharp/).</span><span class="sxs-lookup"><span data-stu-id="a0f81-107">You'll then build and deploy a tasks list web app built on hello [MongoDB .NET driver](https://docs.mongodb.com/ecosystem/drivers/csharp/).</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="a0f81-108">Krav</span><span class="sxs-lookup"><span data-stu-id="a0f81-108">Prerequisites</span></span>

<span data-ttu-id="a0f81-109">Om du inte redan har Visual Studio 2017 installerat, du kan hämta och använda hello **ledigt** [Visual Studio 2017 Community Edition](https://www.visualstudio.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="a0f81-109">If you don’t already have Visual Studio 2017 installed, you can download and use hello **free** [Visual Studio 2017 Community Edition](https://www.visualstudio.com/downloads/).</span></span> <span data-ttu-id="a0f81-110">Kontrollera att du aktiverar **Azure-utveckling** under installationen av hello Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="a0f81-110">Make sure that you enable **Azure development** during hello Visual Studio setup.</span></span>
[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

<a id="create-account"></a>
## <a name="create-a-database-account"></a><span data-ttu-id="a0f81-111">Skapa ett databaskonto</span><span class="sxs-lookup"><span data-stu-id="a0f81-111">Create a database account</span></span>

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount-mongodb.md)]

## <a name="clone-hello-sample-application"></a><span data-ttu-id="a0f81-112">Klona hello exempelprogrammet</span><span class="sxs-lookup"><span data-stu-id="a0f81-112">Clone hello sample application</span></span>

<span data-ttu-id="a0f81-113">Nu ska vi klona en MongoDB-API-app från github, ange hello anslutningssträngen och kör den.</span><span class="sxs-lookup"><span data-stu-id="a0f81-113">Now let's clone a MongoDB API app from github, set hello connection string, and run it.</span></span> <span data-ttu-id="a0f81-114">Du ser hur enkelt som det är att toowork med data programmässigt.</span><span class="sxs-lookup"><span data-stu-id="a0f81-114">You'll see how easy it is toowork with data programmatically.</span></span> 

1. <span data-ttu-id="a0f81-115">Öppna ett git terminalfönster, till exempel git bash och `cd` tooa arbetskatalogen.</span><span class="sxs-lookup"><span data-stu-id="a0f81-115">Open a git terminal window, such as git bash, and `cd` tooa working directory.</span></span>  

2. <span data-ttu-id="a0f81-116">Hello kör följande kommando tooclone hello exempel lagringsplatsen.</span><span class="sxs-lookup"><span data-stu-id="a0f81-116">Run hello following command tooclone hello sample repository.</span></span> 

    ```bash
    git clone https://github.com/Azure-Samples/azure-cosmos-db-mongodb-dotnet-getting-started.git
    ```

3. <span data-ttu-id="a0f81-117">Öppna sedan hello lösningsfilen i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="a0f81-117">Then open hello solution file in Visual Studio.</span></span> 

## <a name="review-hello-code"></a><span data-ttu-id="a0f81-118">Granska hello kod</span><span class="sxs-lookup"><span data-stu-id="a0f81-118">Review hello code</span></span>

<span data-ttu-id="a0f81-119">Låt oss göra en snabb genomgång av vad som händer i hello app.</span><span class="sxs-lookup"><span data-stu-id="a0f81-119">Let's make a quick review of what's happening in hello app.</span></span> <span data-ttu-id="a0f81-120">Öppna hello **Dal.cs** fil under hello **DAL** directory och du hittar att dessa rader med kod skapar hello Azure Cosmos DB resurser.</span><span class="sxs-lookup"><span data-stu-id="a0f81-120">Open hello **Dal.cs** file under hello **DAL** directory and you'll find that these lines of code create hello Azure Cosmos DB resources.</span></span> 

* <span data-ttu-id="a0f81-121">Initiera hello Mongo-klienten.</span><span class="sxs-lookup"><span data-stu-id="a0f81-121">Initialize hello Mongo Client.</span></span>

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

* <span data-ttu-id="a0f81-122">Hämta hello databas och hello samling.</span><span class="sxs-lookup"><span data-stu-id="a0f81-122">Retrieve hello database and hello collection.</span></span>

    ```cs
    private string dbName = "Tasks";
    private string collectionName = "TasksList";

    var database = client.GetDatabase(dbName);
    var todoTaskCollection = database.GetCollection<MyTask>(collectionName);
    ```

* <span data-ttu-id="a0f81-123">Hämta alla dokument.</span><span class="sxs-lookup"><span data-stu-id="a0f81-123">Retrieve all documents.</span></span>

    ```cs
    collection.Find(new BsonDocument()).ToList();
    ```

## <a name="update-your-connection-string"></a><span data-ttu-id="a0f81-124">Uppdatera din anslutningssträng</span><span class="sxs-lookup"><span data-stu-id="a0f81-124">Update your connection string</span></span>

<span data-ttu-id="a0f81-125">Gå tillbaka toohello Azure portal tooget din Anslutningssträngsinformation nu och kopierar den till hello app.</span><span class="sxs-lookup"><span data-stu-id="a0f81-125">Now go back toohello Azure portal tooget your connection string information and copy it into hello app.</span></span>

1. <span data-ttu-id="a0f81-126">I hello [Azure-portalen](http://portal.azure.com/), i din Azure Cosmos DB konto, i hello vänstra navigeringsrutan klickar du på **anslutningssträngen**, och klicka sedan på **skrivskyddad nycklar**.</span><span class="sxs-lookup"><span data-stu-id="a0f81-126">In hello [Azure portal](http://portal.azure.com/), in your Azure Cosmos DB account, in hello left navigation click **Connection String**, and then click **Read-write Keys**.</span></span> <span data-ttu-id="a0f81-127">I hello Dal.cs fil i hello nästa steg ska du använda hello kopiera knappar hello höger på hello skärmen toocopy hello användarnamn, lösenord och värden.</span><span class="sxs-lookup"><span data-stu-id="a0f81-127">You'll use hello copy buttons on hello right side of hello screen toocopy hello Username, Password, and Host into hello Dal.cs file in hello next step.</span></span>

2. <span data-ttu-id="a0f81-128">Öppna hello **Dal.cs** filen i hello **DAL** directory.</span><span class="sxs-lookup"><span data-stu-id="a0f81-128">Open hello **Dal.cs** file in hello **DAL** directory.</span></span> 

3. <span data-ttu-id="a0f81-129">Kopiera ditt **användarnamn** värde från hello-portal (med hello kopieringsknappen) och gör den hello värdet för hello **användarnamn** i din **Dal.cs** fil.</span><span class="sxs-lookup"><span data-stu-id="a0f81-129">Copy your **username** value from hello portal (using hello copy button) and make it hello value of hello **username** in your **Dal.cs** file.</span></span> 

4. <span data-ttu-id="a0f81-130">Kopiera ditt **värden** värdet från hello-portalen och gör den hello värdet för hello **värden** i din **Dal.cs** fil.</span><span class="sxs-lookup"><span data-stu-id="a0f81-130">Then copy your **host** value from hello portal and make it hello value of hello **host** in your **Dal.cs** file.</span></span> 

5. <span data-ttu-id="a0f81-131">Slutligen Kopiera ditt **lösenord** värdet från hello-portalen och gör den hello värdet för hello **lösenord** i din **Dal.cs** fil.</span><span class="sxs-lookup"><span data-stu-id="a0f81-131">Finally copy your **password** value from hello portal and make it hello value of hello **password** in your **Dal.cs** file.</span></span> 

<span data-ttu-id="a0f81-132">Du har nu uppdaterat din app med alla hello information som behövs för toocommunicate med Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="a0f81-132">You've now updated your app with all hello info it needs toocommunicate with Azure Cosmos DB.</span></span> 
    
## <a name="run-hello-web-app"></a><span data-ttu-id="a0f81-133">Köra hello-webbprogram</span><span class="sxs-lookup"><span data-stu-id="a0f81-133">Run hello web app</span></span>

1. <span data-ttu-id="a0f81-134">Högerklicka på hello-projekt i Visual Studio **Solution Explorer** och klicka sedan på **hantera NuGet-paket**.</span><span class="sxs-lookup"><span data-stu-id="a0f81-134">In Visual Studio, right-click on hello project in **Solution Explorer** and then click **Manage NuGet Packages**.</span></span> 

2. <span data-ttu-id="a0f81-135">I hello NuGet **Bläddra** skriver *MongoDB.Driver*.</span><span class="sxs-lookup"><span data-stu-id="a0f81-135">In hello NuGet **Browse** box, type *MongoDB.Driver*.</span></span>

3. <span data-ttu-id="a0f81-136">Hello resultat för att installera hello **MongoDB.Driver** bibliotek.</span><span class="sxs-lookup"><span data-stu-id="a0f81-136">From hello results, install hello **MongoDB.Driver** library.</span></span> <span data-ttu-id="a0f81-137">Detta installerar hello MongoDB.Driver paket samt alla beroenden.</span><span class="sxs-lookup"><span data-stu-id="a0f81-137">This installs hello MongoDB.Driver package as well as all dependencies.</span></span>

4. <span data-ttu-id="a0f81-138">Klicka på CTRL + F5 toorun hello program.</span><span class="sxs-lookup"><span data-stu-id="a0f81-138">Click CTRL + F5 toorun hello application.</span></span> <span data-ttu-id="a0f81-139">Appen visas i din webbläsare.</span><span class="sxs-lookup"><span data-stu-id="a0f81-139">Your app displays in your browser.</span></span> 

5. <span data-ttu-id="a0f81-140">Klicka på **skapa** i hello webbläsare och skapa några nya aktiviteter i appen uppgiften lista.</span><span class="sxs-lookup"><span data-stu-id="a0f81-140">Click **Create** in hello browser and create a few new tasks in your task list app.</span></span>

## <a name="review-slas-in-hello-azure-portal"></a><span data-ttu-id="a0f81-141">Granska SLA: er i hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="a0f81-141">Review SLAs in hello Azure portal</span></span>

[!INCLUDE [cosmosdb-tutorial-review-slas](../../includes/cosmos-db-tutorial-review-slas.md)]

## <a name="clean-up-resources"></a><span data-ttu-id="a0f81-142">Rensa resurser</span><span class="sxs-lookup"><span data-stu-id="a0f81-142">Clean up resources</span></span>

<span data-ttu-id="a0f81-143">Om du inte kommer toocontinue toouse den här appen, tar du bort alla resurser som skapats av denna Snabbstart i hello Azure-portalen med hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="a0f81-143">If you're not going toocontinue toouse this app, delete all resources created by this quickstart in hello Azure portal with hello following steps:</span></span>

1. <span data-ttu-id="a0f81-144">Hello vänstra menyn i hello Azure-portalen klickar du på **resursgrupper** och klicka sedan på hello namnet på hello resurs du skapat.</span><span class="sxs-lookup"><span data-stu-id="a0f81-144">From hello left-hand menu in hello Azure portal, click **Resource groups** and then click hello name of hello resource you created.</span></span> 
2. <span data-ttu-id="a0f81-145">På din resurs gruppen klickar du på **ta bort**typnamn hello för hello resurs toodelete i hello textrutan och klicka sedan på **ta bort**.</span><span class="sxs-lookup"><span data-stu-id="a0f81-145">On your resource group page, click **Delete**, type hello name of hello resource toodelete in hello text box, and then click **Delete**.</span></span>

## <a name="next-steps"></a><span data-ttu-id="a0f81-146">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="a0f81-146">Next steps</span></span>

<span data-ttu-id="a0f81-147">I den här snabbstarten du har lärt dig hur toocreate ett Azure DB som Cosmos-konto och kör en web app med hjälp av hello API för MongoDB.</span><span class="sxs-lookup"><span data-stu-id="a0f81-147">In this quickstart, you've learned how toocreate an Azure Cosmos DB account and run a web app using hello API for MongoDB.</span></span> <span data-ttu-id="a0f81-148">Nu kan du importera ytterligare data tooyour Cosmos-DB-konto.</span><span class="sxs-lookup"><span data-stu-id="a0f81-148">You can now import additional data tooyour Cosmos DB account.</span></span> 

> [!div class="nextstepaction"]
> [<span data-ttu-id="a0f81-149">Importera data till Azure Cosmos DB för hello MongoDB-API</span><span class="sxs-lookup"><span data-stu-id="a0f81-149">Import data into Azure Cosmos DB for hello MongoDB API</span></span>](mongodb-migrate.md)

