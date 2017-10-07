---
title: en Azure Cosmos DB .NET-program med aaaBuild hello tabell API | Microsoft Docs
description: "Kom igång med tabell-API:t i Azure Cosmos DB med hjälp av .NET"
services: cosmos-db
documentationcenter: 
author: arramac
manager: jhubbard
editor: 
ms.assetid: 66327041-4d5e-4ce6-a394-fee107c18e59
ms.service: cosmos-db
ms.custom: quick start connect, mvc
ms.workload: 
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 06/22/2017
ms.author: arramac
ms.openlocfilehash: bdd4f8ec45407962b3d2cb26aa814a20cfc62173
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-cosmos-db-build-a-net-application-using-hello-table-api"></a><span data-ttu-id="ffedc-103">Azure Cosmos DB: Skapa ett .NET-program med hjälp av hello tabell-API</span><span class="sxs-lookup"><span data-stu-id="ffedc-103">Azure Cosmos DB: Build a .NET application using hello Table API</span></span>

<span data-ttu-id="ffedc-104">Azure Cosmos DB är Microsofts globalt distribuerade databastjänst för flera datamodeller.</span><span class="sxs-lookup"><span data-stu-id="ffedc-104">Azure Cosmos DB is Microsoft’s globally distributed multi-model database service.</span></span> <span data-ttu-id="ffedc-105">Du kan snabbt skapa och fråga dokument och nyckel/värde-diagrammet databaser, som omfattas av hello global distributionsplatsen och skala horisontellt funktionerna i hello kärnan i Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="ffedc-105">You can quickly create and query document, key/value, and graph databases, all of which benefit from hello global distribution and horizontal scale capabilities at hello core of Azure Cosmos DB.</span></span> 

<span data-ttu-id="ffedc-106">Denna Snabbstart visar hur toocreate en Cosmos-databas med Azure-konto och skapa en tabell i kontot med hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="ffedc-106">This quick start demonstrates how toocreate an Azure Cosmos DB account, and create a table within that account using hello Azure portal.</span></span> <span data-ttu-id="ffedc-107">Du sedan skriva kod tooinsert, uppdatera och ta bort entiteter och köra några frågor med hello nya [Windows Azure-Lagringstabellen Premium](https://aka.ms/premiumtablenuget) (förhandsgranskning) paket från NuGet.</span><span class="sxs-lookup"><span data-stu-id="ffedc-107">You'll then write code tooinsert, update, and delete entities, and run some queries using hello new [Windows Azure Storage Premium Table](https://aka.ms/premiumtablenuget) (preview) package from NuGet.</span></span> <span data-ttu-id="ffedc-108">Det här biblioteket har hello samma klasser och metoden signaturer som hello offentliga [Windows Azure Storage SDK: N](https://www.nuget.org/packages/WindowsAzure.Storage), men har även hello möjlighet tooconnect tooAzure Cosmos DB konton med hjälp av hello [tabell API](table-introduction.md) (förhandsversion).</span><span class="sxs-lookup"><span data-stu-id="ffedc-108">This library has hello same classes and method signatures as hello public [Windows Azure Storage SDK](https://www.nuget.org/packages/WindowsAzure.Storage), but also has hello ability tooconnect tooAzure Cosmos DB accounts using hello [Table API](table-introduction.md) (preview).</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="ffedc-109">Krav</span><span class="sxs-lookup"><span data-stu-id="ffedc-109">Prerequisites</span></span>

<span data-ttu-id="ffedc-110">Om du inte redan har Visual Studio 2017 installerat, du kan hämta och använda hello **ledigt** [Visual Studio 2017 Community Edition](https://www.visualstudio.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="ffedc-110">If you don’t already have Visual Studio 2017 installed, you can download and use hello **free** [Visual Studio 2017 Community Edition](https://www.visualstudio.com/downloads/).</span></span> <span data-ttu-id="ffedc-111">Kontrollera att du aktiverar **Azure-utveckling** under installationen av hello Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="ffedc-111">Make sure that you enable **Azure development** during hello Visual Studio setup.</span></span>

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-a-database-account"></a><span data-ttu-id="ffedc-112">Skapa ett databaskonto</span><span class="sxs-lookup"><span data-stu-id="ffedc-112">Create a database account</span></span>

[!INCLUDE [cosmos-db-create-dbaccount-table](../../includes/cosmos-db-create-dbaccount-table.md)]

## <a name="add-a-table"></a><span data-ttu-id="ffedc-113">Lägg till en tabell</span><span class="sxs-lookup"><span data-stu-id="ffedc-113">Add a table</span></span>

[!INCLUDE [cosmos-db-create-table](../../includes/cosmos-db-create-table.md)]

## <a name="add-sample-data"></a><span data-ttu-id="ffedc-114">Lägg till exempeldata</span><span class="sxs-lookup"><span data-stu-id="ffedc-114">Add sample data</span></span>

<span data-ttu-id="ffedc-115">Du kan nu lägga till data tooyour ny tabell med hjälp av Data Explorer (förhandsversion).</span><span class="sxs-lookup"><span data-stu-id="ffedc-115">You can now add data tooyour new table using Data Explorer (Preview).</span></span>

1. <span data-ttu-id="ffedc-116">Öppna Datautforskaren, expandera **sample-table**, klicka på **Entiteter** och klicka sedan på **Lägg till entitet**.</span><span class="sxs-lookup"><span data-stu-id="ffedc-116">In Data Explorer, expand **sample-table**, click **Entities**, and then click **Add Entity**.</span></span>

   ![Skapa nya entiteter i Data Explorer i hello Azure-portalen](./media/create-table-dotnet/azure-cosmosdb-data-explorer-new-document.png)
2. <span data-ttu-id="ffedc-118">Nu lägga data toohello PartitionKey värde och RowKey värdet och klicka på **lägga till entiteten**.</span><span class="sxs-lookup"><span data-stu-id="ffedc-118">Now add data toohello PartitionKey value box and RowKey value box, and click **Add Entity**.</span></span>

   ![Ange hello partitionsnyckel och Radnyckel för en ny entitet](./media/create-table-dotnet/azure-cosmosdb-data-explorer-new-entity.png)
  
    <span data-ttu-id="ffedc-120">Du kan nu lägga till fler enheter tooyour tabell, redigera entiteter eller fråga din data i Data Explorer.</span><span class="sxs-lookup"><span data-stu-id="ffedc-120">You can now add more entities tooyour table, edit your entities, or query your data in Data Explorer.</span></span> <span data-ttu-id="ffedc-121">Data Explorer är också där du kan skala din genomflöde och lägga till lagrade procedurer, användardefinierade funktioner och utlösare tooyour tabell.</span><span class="sxs-lookup"><span data-stu-id="ffedc-121">Data Explorer is also where you can scale your throughput and add stored procedures, user defined functions, and triggers tooyour table.</span></span>

## <a name="clone-hello-sample-application"></a><span data-ttu-id="ffedc-122">Klona hello exempelprogrammet</span><span class="sxs-lookup"><span data-stu-id="ffedc-122">Clone hello sample application</span></span>

<span data-ttu-id="ffedc-123">Nu ska vi klona en tabell app från github, ange hello anslutningssträngen och kör den.</span><span class="sxs-lookup"><span data-stu-id="ffedc-123">Now let's clone a Table app from github, set hello connection string, and run it.</span></span> <span data-ttu-id="ffedc-124">Du ser hur enkelt som det är att toowork med data programmässigt.</span><span class="sxs-lookup"><span data-stu-id="ffedc-124">You'll see how easy it is toowork with data programmatically.</span></span> 

1. <span data-ttu-id="ffedc-125">Öppna ett git terminalfönster, till exempel git bash och `cd` tooa arbetskatalogen.</span><span class="sxs-lookup"><span data-stu-id="ffedc-125">Open a git terminal window, such as git bash, and `cd` tooa working directory.</span></span>  

2. <span data-ttu-id="ffedc-126">Hello kör följande kommando tooclone hello exempel lagringsplatsen.</span><span class="sxs-lookup"><span data-stu-id="ffedc-126">Run hello following command tooclone hello sample repository.</span></span> 

    ```bash
    git clone https://github.com/Azure-Samples/azure-cosmos-db-table-dotnet-getting-started.git
    ```

3. <span data-ttu-id="ffedc-127">Öppna sedan hello lösningsfilen i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="ffedc-127">Then open hello solution file in Visual Studio.</span></span> 

## <a name="review-hello-code"></a><span data-ttu-id="ffedc-128">Granska hello kod</span><span class="sxs-lookup"><span data-stu-id="ffedc-128">Review hello code</span></span>

<span data-ttu-id="ffedc-129">Låt oss göra en snabb genomgång av vad som händer i hello app.</span><span class="sxs-lookup"><span data-stu-id="ffedc-129">Let's make a quick review of what's happening in hello app.</span></span> <span data-ttu-id="ffedc-130">Öppna hello Program.cs-filen och du hittar att dessa rader med kod skapar hello Azure Cosmos DB resurser.</span><span class="sxs-lookup"><span data-stu-id="ffedc-130">Open hello Program.cs file and you'll find that these lines of code create hello Azure Cosmos DB resources.</span></span> 

* <span data-ttu-id="ffedc-131">Hej CloudTableClient har initierats.</span><span class="sxs-lookup"><span data-stu-id="ffedc-131">hello CloudTableClient is initialized.</span></span>

    ```csharp
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(connectionString); 
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();
    ```

* <span data-ttu-id="ffedc-132">En ny tabell skapas om det inte finns någon.</span><span class="sxs-lookup"><span data-stu-id="ffedc-132">A new table is created if it does not exist.</span></span>

    ```csharp
    CloudTable table = tableClient.GetTableReference("people");
    table.CreateIfNotExists();
    ```

* <span data-ttu-id="ffedc-133">En ny tabellbehållare skapas.</span><span class="sxs-lookup"><span data-stu-id="ffedc-133">A new Table container is created.</span></span> <span data-ttu-id="ffedc-134">Du ser den här koden mycket lik tooregular Azure Table storage SDK.</span><span class="sxs-lookup"><span data-stu-id="ffedc-134">You will notice this code very similar tooregular Azure Table storage SDK.</span></span> 

    ```csharp
    CustomerEntity item = new CustomerEntity()
                {
                    PartitionKey = Guid.NewGuid().ToString(),
                    RowKey = Guid.NewGuid().ToString(),
                    Email = $"{GetRandomString(6)}@contoso.com",
                    PhoneNumber = "425-555-0102",
                    Bio = GetRandomString(1000)
                };
    ```

## <a name="update-your-connection-string"></a><span data-ttu-id="ffedc-135">Uppdatera din anslutningssträng</span><span class="sxs-lookup"><span data-stu-id="ffedc-135">Update your connection string</span></span>

<span data-ttu-id="ffedc-136">Nu ska vi uppdatera hello Anslutningssträngsinformation så att din app kan prata tooAzure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="ffedc-136">Now we'll update hello connection string information so your app can talk tooAzure Cosmos DB.</span></span> 

1. <span data-ttu-id="ffedc-137">Öppna hello app.config-filen i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="ffedc-137">In Visual Studio, open hello app.config file.</span></span> 

2. <span data-ttu-id="ffedc-138">I hello [Azure-portalen](http://portal.azure.com/), i hello Azure Cosmos DB kvar navigeringsmenyn, klicka på **anslutningssträngen**.</span><span class="sxs-lookup"><span data-stu-id="ffedc-138">In hello [Azure portal](http://portal.azure.com/), in hello Azure Cosmos DB left navigation menu, click **Connection String**.</span></span> <span data-ttu-id="ffedc-139">Klicka på hello kopieringsknappen för hello anslutningssträngen i hello nya rutan.</span><span class="sxs-lookup"><span data-stu-id="ffedc-139">Then in hello new pane click hello copy button for hello connection string.</span></span> 

    ![Visa och kopiera hello slutpunkt och Kontonyckel hello anslutningssträngen i fönstret](./media/create-table-dotnet/keys.png)

3. <span data-ttu-id="ffedc-141">Klistra in hello värdet i hello app.config-fil som hello värde för hello PremiumStorageConnectionString.</span><span class="sxs-lookup"><span data-stu-id="ffedc-141">Paste hello value into hello app.config file as hello value of hello PremiumStorageConnectionString.</span></span> 

    `<add key="PremiumStorageConnectionString" 
        value="DefaultEndpointsProtocol=https;AccountName=MYSTORAGEACCOUNT;AccountKey=AUTHKEY;TableEndpoint=https://COSMOSDB.documents.azure.com" />`    

    <span data-ttu-id="ffedc-142">Du kan lämna hello StandardStorageConnectionString är.</span><span class="sxs-lookup"><span data-stu-id="ffedc-142">You can leave hello StandardStorageConnectionString as is.</span></span>

<span data-ttu-id="ffedc-143">Du har nu uppdaterat din app med alla hello information som behövs för toocommunicate med Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="ffedc-143">You've now updated your app with all hello info it needs toocommunicate with Azure Cosmos DB.</span></span> 

## <a name="run-hello-web-app"></a><span data-ttu-id="ffedc-144">Köra hello-webbprogram</span><span class="sxs-lookup"><span data-stu-id="ffedc-144">Run hello web app</span></span>

1. <span data-ttu-id="ffedc-145">I Visual Studio högerklickar du på hello **PremiumTableGetStarted** projektet i **Solution Explorer** och klicka sedan på **hantera NuGet-paket**.</span><span class="sxs-lookup"><span data-stu-id="ffedc-145">In Visual Studio, right-click on hello **PremiumTableGetStarted** project in **Solution Explorer** and then click **Manage NuGet Packages**.</span></span> 

2. <span data-ttu-id="ffedc-146">I hello NuGet **Bläddra** skriver *WindowsAzure.Storage PremiumTable*.</span><span class="sxs-lookup"><span data-stu-id="ffedc-146">In hello NuGet **Browse** box, type *WindowsAzure.Storage-PremiumTable*.</span></span>

3. <span data-ttu-id="ffedc-147">Kontrollera hello **inkludera förhandsversion** rutan.</span><span class="sxs-lookup"><span data-stu-id="ffedc-147">Check hello **Include prerelease** box.</span></span> 

4. <span data-ttu-id="ffedc-148">Hello resultat för att installera hello **WindowsAzure.Storage PremiumTable** bibliotek.</span><span class="sxs-lookup"><span data-stu-id="ffedc-148">From hello results, install hello **WindowsAzure.Storage-PremiumTable** library.</span></span> <span data-ttu-id="ffedc-149">Detta installerar hello Förhandsgranska Azure Cosmos DB tabell API paket samt alla beroenden.</span><span class="sxs-lookup"><span data-stu-id="ffedc-149">This installs hello preview Azure Cosmos DB Table API package as well as all dependencies.</span></span> <span data-ttu-id="ffedc-150">Observera att detta är ett annat NuGet-paket än hello Windows Azure Storage-paketet som används av Azure Table storage.</span><span class="sxs-lookup"><span data-stu-id="ffedc-150">Note that this is a different NuGet package than hello Windows Azure Storage package used by Azure Table storage.</span></span> 

5. <span data-ttu-id="ffedc-151">Klicka på CTRL + F5 toorun hello program.</span><span class="sxs-lookup"><span data-stu-id="ffedc-151">Click CTRL + F5 toorun hello application.</span></span>

    <span data-ttu-id="ffedc-152">hello konsolfönstret visar hello data läggs till, hämta, efterfrågas, ersättas och tas bort från hello tabell.</span><span class="sxs-lookup"><span data-stu-id="ffedc-152">hello console window displays hello data being added, retrieved, queried, replaced and deleted from hello table.</span></span> <span data-ttu-id="ffedc-153">Tryck på några viktiga tooclose hello konsolfönstret när hello skriptet har slutförts.</span><span class="sxs-lookup"><span data-stu-id="ffedc-153">When hello script completes, press any key tooclose hello console window.</span></span> 
    
    ![Konsolen utdata av hello Snabbstart](./media/create-table-dotnet/azure-cosmosdb-table-quickstart-console-output.png)

6. <span data-ttu-id="ffedc-155">Om du vill toosee hello nya enheter i bara kommentera ut rader 188 208 i program.cs så att de inte bort Data Explorer kör hello exempel igen.</span><span class="sxs-lookup"><span data-stu-id="ffedc-155">If you want toosee hello new entities in Data Explorer, just comment out lines 188-208 in program.cs so they aren't deleted, then run hello sample again.</span></span> 

    <span data-ttu-id="ffedc-156">Du kan nu gå tillbaka tooData Explorer, klicka på **uppdatera**, expandera hello **personer** tabell och klicka på **entiteter**, och sedan arbeta med dessa nya data.</span><span class="sxs-lookup"><span data-stu-id="ffedc-156">You can now go back tooData Explorer, click **Refresh**, expand hello **people** table and click **Entities**, and then work with this new data.</span></span> 

    ![Nya entiteter i Data Explorer](./media/create-table-dotnet/azure-cosmosdb-table-quickstart-data-explorer.png)

## <a name="review-slas-in-hello-azure-portal"></a><span data-ttu-id="ffedc-158">Granska SLA: er i hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="ffedc-158">Review SLAs in hello Azure portal</span></span>

[!INCLUDE [cosmosdb-tutorial-review-slas](../../includes/cosmos-db-tutorial-review-slas.md)]

## <a name="clean-up-resources"></a><span data-ttu-id="ffedc-159">Rensa resurser</span><span class="sxs-lookup"><span data-stu-id="ffedc-159">Clean up resources</span></span>

<span data-ttu-id="ffedc-160">Om du inte kommer toocontinue toouse den här appen, tar du bort alla resurser som skapats av denna Snabbstart i hello Azure-portalen med hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="ffedc-160">If you're not going toocontinue toouse this app, delete all resources created by this quickstart in hello Azure portal with hello following steps:</span></span> 

1. <span data-ttu-id="ffedc-161">Hello vänstra menyn i hello Azure-portalen klickar du på **resursgrupper** och klicka sedan på hello namnet på hello resurs du skapat.</span><span class="sxs-lookup"><span data-stu-id="ffedc-161">From hello left-hand menu in hello Azure portal, click **Resource groups** and then click hello name of hello resource you created.</span></span> 
2. <span data-ttu-id="ffedc-162">På din resurs gruppen klickar du på **ta bort**typnamn hello för hello resurs toodelete i hello textrutan och klicka sedan på **ta bort**.</span><span class="sxs-lookup"><span data-stu-id="ffedc-162">On your resource group page, click **Delete**, type hello name of hello resource toodelete in hello text box, and then click **Delete**.</span></span>

## <a name="next-steps"></a><span data-ttu-id="ffedc-163">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="ffedc-163">Next steps</span></span>

<span data-ttu-id="ffedc-164">Du har lärt dig hur toocreate ett Azure DB som Cosmos-konto, skapa en tabell med hello Data Explorer och kör en app i denna Snabbstart.</span><span class="sxs-lookup"><span data-stu-id="ffedc-164">In this quickstart, you've learned how toocreate an Azure Cosmos DB account, create a table using hello Data Explorer, and run an app.</span></span>  <span data-ttu-id="ffedc-165">Nu kan du fråga data med hello tabell API.</span><span class="sxs-lookup"><span data-stu-id="ffedc-165">Now you can query your data using hello Table API.</span></span>  

> [!div class="nextstepaction"]
> [<span data-ttu-id="ffedc-166">Frågan med hello tabell-API</span><span class="sxs-lookup"><span data-stu-id="ffedc-166">Query using hello Table API</span></span>](tutorial-query-table.md)

