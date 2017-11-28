---
title: 'Azure Cosmos DB: Skapa en webbapp med Xamarin- och Facebook-autentisering | Microsoft Docs'
description: "Anger en .NET-kodexempel som du kan använda tooconnect tooand fråga Azure Cosmos DB"
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
ms.openlocfilehash: 5f71dddd2b2f5bd117e481c96c17915fc58d2119
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-cosmos-db-build-a-web-app-with-net-xamarin-and-facebook-authentication"></a><span data-ttu-id="0f1e1-103">Azure Cosmos DB: Skapa en webbapp med Xamarin- och Facebook-autentisering</span><span class="sxs-lookup"><span data-stu-id="0f1e1-103">Azure Cosmos DB: Build a web app with .NET, Xamarin, and Facebook authentication</span></span>

<span data-ttu-id="0f1e1-104">Azure Cosmos DB är Microsofts globalt distribuerade databastjänst för flera datamodeller.</span><span class="sxs-lookup"><span data-stu-id="0f1e1-104">Azure Cosmos DB is Microsoft’s globally distributed multi-model database service.</span></span> <span data-ttu-id="0f1e1-105">Du kan snabbt skapa och fråga dokument och nyckel/värde-diagrammet databaser, som omfattas av hello global distributionsplatsen och skala horisontellt funktionerna i hello kärnan i Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="0f1e1-105">You can quickly create and query document, key/value, and graph databases, all of which benefit from hello global distribution and horizontal scale capabilities at hello core of Azure Cosmos DB.</span></span> 

<span data-ttu-id="0f1e1-106">Den här snabbstartsguide visar hur toocreate ett konto i Azure Cosmos DB, dokumentdatabasen och samlingen använder hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="0f1e1-106">This quick start demonstrates how toocreate an Azure Cosmos DB account, document database, and collection using hello Azure portal.</span></span> <span data-ttu-id="0f1e1-107">Du sedan skapa och distribuera en todo lista-webbapp som bygger på hello [DocumentDB .NET API](documentdb-sdk-dotnet.md), [Xamarin](https://www.xamarin.com/), och hello Azure Cosmos-auktorisering databasmotor.</span><span class="sxs-lookup"><span data-stu-id="0f1e1-107">You'll then build and deploy a todo list web app built on hello [DocumentDB .NET API](documentdb-sdk-dotnet.md), [Xamarin](https://www.xamarin.com/), and hello Azure Cosmos DB authorization engine.</span></span> <span data-ttu-id="0f1e1-108">webbprogrammet för hello todo-listan implementerar en användarspecifik datamönster som gör att användare toologin med hjälp av Facebook Auth och hantera sina egna toodo-objekt.</span><span class="sxs-lookup"><span data-stu-id="0f1e1-108">hello todo list web app implements a per-user data pattern that enables users toologin using Facebook Auth and manage their own toodo items.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="0f1e1-109">Krav</span><span class="sxs-lookup"><span data-stu-id="0f1e1-109">Prerequisites</span></span>

<span data-ttu-id="0f1e1-110">Om du inte redan har Visual Studio 2017 installerat, du kan hämta och använda hello **ledigt** [Visual Studio 2017 Community Edition](https://www.visualstudio.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="0f1e1-110">If you don’t already have Visual Studio 2017 installed, you can download and use hello **free** [Visual Studio 2017 Community Edition](https://www.visualstudio.com/downloads/).</span></span> <span data-ttu-id="0f1e1-111">Kontrollera att du aktiverar **Azure-utveckling** under installationen av hello Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="0f1e1-111">Make sure that you enable **Azure development** during hello Visual Studio setup.</span></span>

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-a-database-account"></a><span data-ttu-id="0f1e1-112">Skapa ett databaskonto</span><span class="sxs-lookup"><span data-stu-id="0f1e1-112">Create a database account</span></span>

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

## <a name="add-a-collection"></a><span data-ttu-id="0f1e1-113">Lägga till en samling</span><span class="sxs-lookup"><span data-stu-id="0f1e1-113">Add a collection</span></span>

[!INCLUDE [cosmos-db-create-collection](../../includes/cosmos-db-create-collection.md)]

## <a name="clone-hello-sample-application"></a><span data-ttu-id="0f1e1-114">Klona hello exempelprogrammet</span><span class="sxs-lookup"><span data-stu-id="0f1e1-114">Clone hello sample application</span></span>

<span data-ttu-id="0f1e1-115">Nu ska vi klona en DocumentDB-API-app från github, ange hello anslutningssträngen och kör den.</span><span class="sxs-lookup"><span data-stu-id="0f1e1-115">Now let's clone a DocumentDB API app from github, set hello connection string, and run it.</span></span> <span data-ttu-id="0f1e1-116">Du ser hur enkelt som det är att toowork med data programmässigt.</span><span class="sxs-lookup"><span data-stu-id="0f1e1-116">You'll see how easy it is toowork with data programmatically.</span></span> 

1. <span data-ttu-id="0f1e1-117">Öppna ett git terminalfönster, till exempel git bash och `cd` tooa arbetskatalogen.</span><span class="sxs-lookup"><span data-stu-id="0f1e1-117">Open a git terminal window, such as git bash, and `cd` tooa working directory.</span></span>  

2. <span data-ttu-id="0f1e1-118">Hello kör följande kommando tooclone hello exempel lagringsplatsen.</span><span class="sxs-lookup"><span data-stu-id="0f1e1-118">Run hello following command tooclone hello sample repository.</span></span> 

    ```bash
    git clone https://github.com/Azure/azure-documentdb-dotnet.git
    ```

3. <span data-ttu-id="0f1e1-119">Öppna sedan hello DocumentDBTodo.sln filen från hello samples/xamarin/UserItems/xamarin.forms mapp i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="0f1e1-119">Then open hello DocumentDBTodo.sln file from hello samples/xamarin/UserItems/xamarin.forms folder in Visual Studio.</span></span> 

## <a name="review-hello-code"></a><span data-ttu-id="0f1e1-120">Granska hello kod</span><span class="sxs-lookup"><span data-stu-id="0f1e1-120">Review hello code</span></span>

<span data-ttu-id="0f1e1-121">hello kod i hello Xamarin mapp innehåller:</span><span class="sxs-lookup"><span data-stu-id="0f1e1-121">hello code in hello Xamarin folder contains:</span></span>

* <span data-ttu-id="0f1e1-122">Xamarin-app.</span><span class="sxs-lookup"><span data-stu-id="0f1e1-122">Xamarin app.</span></span> <span data-ttu-id="0f1e1-123">hello app lagrar hello användarens todo-objekt i en partitionerad samling med namnet UserItems.</span><span class="sxs-lookup"><span data-stu-id="0f1e1-123">hello app stores hello user's todo items in a partitioned collection named UserItems.</span></span>
* <span data-ttu-id="0f1e1-124">API för resurstokenutjämning.</span><span class="sxs-lookup"><span data-stu-id="0f1e1-124">Resource token broker API.</span></span> <span data-ttu-id="0f1e1-125">En enkel ASP.NET Web API toobroker Azure Cosmos DB resurs tokens toohello inloggade användare hello-appen.</span><span class="sxs-lookup"><span data-stu-id="0f1e1-125">A simple ASP.NET Web API toobroker Azure Cosmos DB resource tokens toohello logged in users of hello app.</span></span> <span data-ttu-id="0f1e1-126">Resurs-token är tillfällig åtkomst-token som ger hello app hello åtkomst toohello loggas i användarens data.</span><span class="sxs-lookup"><span data-stu-id="0f1e1-126">Resource tokens are short-lived access tokens that provide hello app with hello access toohello logged in user's data.</span></span>

<span data-ttu-id="0f1e1-127">hello autentisering och dataflöde illustreras i hello diagrammet nedan.</span><span class="sxs-lookup"><span data-stu-id="0f1e1-127">hello authentication and data flow is illustrated in hello diagram below.</span></span>

* <span data-ttu-id="0f1e1-128">Hej UserItems samlingen har skapats med hello partitionsnyckel ' / userid'.</span><span class="sxs-lookup"><span data-stu-id="0f1e1-128">hello UserItems collection is created with hello partition key '/userid'.</span></span> <span data-ttu-id="0f1e1-129">Anger en partitionsnyckel för en samling kan Azure Cosmos DB tooscale oändligt som hello antalet användare och objekt växer.</span><span class="sxs-lookup"><span data-stu-id="0f1e1-129">Specifying a partition key for a collection allows Azure Cosmos DB tooscale infinitely as hello number of users and items grows.</span></span>
* <span data-ttu-id="0f1e1-130">Hej Xamarin-app kan användare toologin med Facebook-autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="0f1e1-130">hello Xamarin app allows users toologin with Facebook credentials.</span></span>
* <span data-ttu-id="0f1e1-131">Hej Xamarin app använder Facebook åtkomst-token tooauthenticate med ResourceTokenBroker API</span><span class="sxs-lookup"><span data-stu-id="0f1e1-131">hello Xamarin app uses Facebook access token tooauthenticate with ResourceTokenBroker API</span></span>
* <span data-ttu-id="0f1e1-132">hello resurs token broker API autentiserar hello förfrågan med en App Service-Auth-funktionen och begär en resurs Azure DB som Cosmos-token med läsning och skrivning åtkomst tooall dokument delning hello den autentiserade användarens partitionsnyckel.</span><span class="sxs-lookup"><span data-stu-id="0f1e1-132">hello resource token broker API authenticates hello request using App Service Auth feature, and requests an Azure Cosmos DB resource token with read/write access tooall documents sharing hello authenticated user's partition key.</span></span>
* <span data-ttu-id="0f1e1-133">Resursen token broker returnerar hello resurs token toohello klientappen.</span><span class="sxs-lookup"><span data-stu-id="0f1e1-133">Resource token broker returns hello resource token toohello client app.</span></span>
* <span data-ttu-id="0f1e1-134">hello bereder sig åtkomst till hello användarens todo-objekt använder hello resurs-token.</span><span class="sxs-lookup"><span data-stu-id="0f1e1-134">hello app accesses hello user's todo items using hello resource token.</span></span>

![Att göra-app med exempeldata](./media/create-documentdb-xamarin-dotnet/tokenbroker.png)
    
## <a name="update-your-connection-string"></a><span data-ttu-id="0f1e1-136">Uppdatera din anslutningssträng</span><span class="sxs-lookup"><span data-stu-id="0f1e1-136">Update your connection string</span></span>

<span data-ttu-id="0f1e1-137">Gå tillbaka toohello Azure portal tooget din Anslutningssträngsinformation nu och kopierar den till hello app.</span><span class="sxs-lookup"><span data-stu-id="0f1e1-137">Now go back toohello Azure portal tooget your connection string information and copy it into hello app.</span></span>

1. <span data-ttu-id="0f1e1-138">I hello [Azure-portalen](http://portal.azure.com/), i din Azure Cosmos DB konto, i hello vänstra navigeringsrutan klickar du på **nycklar**, och klicka sedan på **skrivskyddad nycklar**.</span><span class="sxs-lookup"><span data-stu-id="0f1e1-138">In hello [Azure portal](http://portal.azure.com/), in your Azure Cosmos DB account, in hello left navigation click **Keys**, and then click **Read-write Keys**.</span></span> <span data-ttu-id="0f1e1-139">I hello Web.config-fil i hello nästa steg ska du använda hello kopiera knappar hello höger på hello skärmen toocopy hello URI och primärnyckel.</span><span class="sxs-lookup"><span data-stu-id="0f1e1-139">You'll use hello copy buttons on hello right side of hello screen toocopy hello URI and Primary Key into hello Web.config file in hello next step.</span></span>

    ![Visa och kopiera en snabbtangent i hello Azure-portalen, bladet nycklar](./media/create-documentdb-xamarin-dotnet/keys.png)

2. <span data-ttu-id="0f1e1-141">Öppna i Visual Studio 2017 hello Web.config-filen i hello azure-documentdb-dotnet/prover/xamarin/UserItems/ResourceTokenBroker/ResourceTokenBroker mapp.</span><span class="sxs-lookup"><span data-stu-id="0f1e1-141">In Visual Studio 2017, open hello Web.config file in hello azure-documentdb-dotnet/samples/xamarin/UserItems/ResourceTokenBroker/ResourceTokenBroker folder.</span></span> 

3. <span data-ttu-id="0f1e1-142">Kopiera URI-värdet från hello-portal (med hello kopieringsknappen) och gör den hello värdet för hello accountUrl i Web.config.</span><span class="sxs-lookup"><span data-stu-id="0f1e1-142">Copy your URI value from hello portal (using hello copy button) and make it hello value of hello accountUrl in Web.config.</span></span> 

    `<add key="accountUrl" value="{Azure Cosmos DB account URL}"/>`

4. <span data-ttu-id="0f1e1-143">Kopiera värdet för PRIMÄRNYCKELN från hello-portalen och gör det hello värdet för hello accountKey i Web.congif.</span><span class="sxs-lookup"><span data-stu-id="0f1e1-143">Then copy your PRIMARY KEY value from hello portal and make it hello value of hello accountKey in Web.congif.</span></span> 

    `<add key="accountKey" value="{Azure Cosmos DB secret}"/>`

<span data-ttu-id="0f1e1-144">Du har nu uppdaterat din app med alla hello information som behövs för toocommunicate med Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="0f1e1-144">You've now updated your app with all hello info it needs toocommunicate with Azure Cosmos DB.</span></span> 

## <a name="build-and-deploy-hello-web-app"></a><span data-ttu-id="0f1e1-145">Skapa och distribuera hello-webbprogram</span><span class="sxs-lookup"><span data-stu-id="0f1e1-145">Build and deploy hello web app</span></span>

1. <span data-ttu-id="0f1e1-146">I hello Azure-portalen, skapa en Apptjänst webbplats toohost hello resurs token broker API.</span><span class="sxs-lookup"><span data-stu-id="0f1e1-146">In hello Azure portal, create an App Service website toohost hello Resource token broker API.</span></span>
2. <span data-ttu-id="0f1e1-147">Öppna hello App inställningsbladet för hello resurs token broker API-webbplats i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="0f1e1-147">In hello Azure portal, open hello App Settings blade of hello Resource token broker API website.</span></span> <span data-ttu-id="0f1e1-148">Fyll i hello följande app-inställningar:</span><span class="sxs-lookup"><span data-stu-id="0f1e1-148">Fill in hello following app settings:</span></span>

    * <span data-ttu-id="0f1e1-149">accountUrl - hello Azure Cosmos DB kontots URL från hello nycklar fliken i ditt Azure DB som Cosmos-konto.</span><span class="sxs-lookup"><span data-stu-id="0f1e1-149">accountUrl - hello Azure Cosmos DB account URL from hello Keys tab of your Azure Cosmos DB account.</span></span>
    * <span data-ttu-id="0f1e1-150">accountKey - huvudnyckeln för hello Azure Cosmos DB konto från hello nycklar fliken i ditt Azure DB som Cosmos-konto.</span><span class="sxs-lookup"><span data-stu-id="0f1e1-150">accountKey - hello Azure Cosmos DB account master key from hello Keys tab of your Azure Cosmos DB account.</span></span>
    * <span data-ttu-id="0f1e1-151">databaseId och collectionId för din skapade databas och samling</span><span class="sxs-lookup"><span data-stu-id="0f1e1-151">databaseId and collectionId of your created database and collection</span></span>

3. <span data-ttu-id="0f1e1-152">Publicera hello ResourceTokenBroker lösning tooyour skapas webbplatsen.</span><span class="sxs-lookup"><span data-stu-id="0f1e1-152">Publish hello ResourceTokenBroker solution tooyour created website.</span></span>

4. <span data-ttu-id="0f1e1-153">Öppna hello Xamarin-projektet och navigera tooTodoItemManager.cs.</span><span class="sxs-lookup"><span data-stu-id="0f1e1-153">Open hello Xamarin project, and navigate tooTodoItemManager.cs.</span></span> <span data-ttu-id="0f1e1-154">Fyll i hello värden för accountURL, collectionId, databaseId samt resourceTokenBrokerURL som hello grundläggande https-url för hello resurs token broker webbplats.</span><span class="sxs-lookup"><span data-stu-id="0f1e1-154">Fill in hello values for accountURL, collectionId, databaseId, as well as resourceTokenBrokerURL as hello base https url for hello resource token broker website.</span></span>

5. <span data-ttu-id="0f1e1-155">Fullständig hello [hur tooconfigure Apptjänst programmet toouse Facebook inloggningen](../app-service-mobile/app-service-mobile-how-to-configure-facebook-authentication.md) självstudiekursen toosetup Facebook-autentisering och konfigurera hello ResourceTokenBroker webbplats.</span><span class="sxs-lookup"><span data-stu-id="0f1e1-155">Complete hello [How tooconfigure your App Service application toouse Facebook login](../app-service-mobile/app-service-mobile-how-to-configure-facebook-authentication.md) tutorial toosetup Facebook authentication and configure hello ResourceTokenBroker website.</span></span>

    <span data-ttu-id="0f1e1-156">Kör hello Xamarin-app.</span><span class="sxs-lookup"><span data-stu-id="0f1e1-156">Run hello Xamarin app.</span></span>

## <a name="review-slas-in-hello-azure-portal"></a><span data-ttu-id="0f1e1-157">Granska SLA: er i hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="0f1e1-157">Review SLAs in hello Azure portal</span></span>

[!INCLUDE [cosmosdb-tutorial-review-slas](../../includes/cosmos-db-tutorial-review-slas.md)]

## <a name="clean-up-resources"></a><span data-ttu-id="0f1e1-158">Rensa resurser</span><span class="sxs-lookup"><span data-stu-id="0f1e1-158">Clean up resources</span></span>

<span data-ttu-id="0f1e1-159">Om du inte kommer toocontinue toouse den här appen, tar du bort alla resurser som skapats av denna Snabbstart i hello Azure-portalen med hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="0f1e1-159">If you're not going toocontinue toouse this app, delete all resources created by this quickstart in hello Azure portal with hello following steps:</span></span> 

1. <span data-ttu-id="0f1e1-160">Hello vänstra menyn i hello Azure-portalen klickar du på **resursgrupper** och klicka sedan på hello namnet på hello resursen som du nyss skapade.</span><span class="sxs-lookup"><span data-stu-id="0f1e1-160">From hello left-hand menu in hello Azure portal, click **Resource groups** and then click hello name of hello resource you just created.</span></span> 
2. <span data-ttu-id="0f1e1-161">På din resurs gruppen klickar du på **ta bort**typnamn hello för hello resurs toodelete i hello textrutan och klicka sedan på **ta bort**.</span><span class="sxs-lookup"><span data-stu-id="0f1e1-161">On your resource group page, click **Delete**, type hello name of hello resource toodelete in hello text box, and then click **Delete**.</span></span>

## <a name="next-steps"></a><span data-ttu-id="0f1e1-162">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="0f1e1-162">Next steps</span></span>

<span data-ttu-id="0f1e1-163">Du har lärt dig hur toocreate ett Azure DB som Cosmos-konto, skapa en samling med hello Data Explorer och skapa och distribuera en Xamarin-app i denna Snabbstart.</span><span class="sxs-lookup"><span data-stu-id="0f1e1-163">In this quickstart, you've learned how toocreate an Azure Cosmos DB account, create a collection using hello Data Explorer, and build and deploy a Xamarin app.</span></span> <span data-ttu-id="0f1e1-164">Nu kan du importera ytterligare data tooyour Cosmos-DB-konto.</span><span class="sxs-lookup"><span data-stu-id="0f1e1-164">You can now import additional data tooyour Cosmos DB account.</span></span> 

> [!div class="nextstepaction"]
> [<span data-ttu-id="0f1e1-165">Importera data till Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="0f1e1-165">Import data into Azure Cosmos DB</span></span>](import-data.md)
