---
title: 'Azure Cosmos DB: Skapa en app med Python och hello DocumentDB API | Microsoft Docs'
description: "Anger en Python-kodexempel som du kan använda tooconnect tooand query hello Azure Cosmos DB DocumentDB API"
services: cosmos-db
documentationcenter: 
author: mimig1
manager: jhubbard
editor: 
ms.assetid: 51c11be2-af6d-425f-a86a-39cbfe61da29
ms.service: cosmos-db
ms.custom: quick start connect, mvc
ms.workload: 
ms.tgt_pltfrm: na
ms.devlang: python
ms.topic: hero-article
ms.date: 05/13/2017
ms.author: mimig
ms.openlocfilehash: e66965ab493c6ef693e88a3767a401d39e1bde2a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-cosmos-db-build-a-documentdb-api-app-with-python-and-hello-azure-portal"></a><span data-ttu-id="1d822-103">Azure DB Cosmos: Skapa en DocumentDB-API-app med Python och hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="1d822-103">Azure Cosmos DB: Build a DocumentDB API app with Python and hello Azure portal</span></span>

<span data-ttu-id="1d822-104">Azure Cosmos DB är Microsofts globalt distribuerade databastjänst för flera datamodeller.</span><span class="sxs-lookup"><span data-stu-id="1d822-104">Azure Cosmos DB is Microsoft’s globally distributed multi-model database service.</span></span> <span data-ttu-id="1d822-105">Du kan snabbt skapa och fråga dokument och nyckel/värde-diagrammet databaser, som omfattas av hello global distributionsplatsen och skala horisontellt funktionerna i hello kärnan i Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="1d822-105">You can quickly create and query document, key/value, and graph databases, all of which benefit from hello global distribution and horizontal scale capabilities at hello core of Azure Cosmos DB.</span></span> 

<span data-ttu-id="1d822-106">Den här snabbstartsguide visar hur toocreate ett konto i Azure Cosmos DB, dokumentdatabasen och samlingen använder hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="1d822-106">This quick start demonstrates how toocreate an Azure Cosmos DB account, document database, and collection using hello Azure portal.</span></span> <span data-ttu-id="1d822-107">Du sedan skapa och köra en konsolapp som bygger på hello [DocumentDB Python API](documentdb-sdk-python.md).</span><span class="sxs-lookup"><span data-stu-id="1d822-107">You then build and run a console app built on hello [DocumentDB Python API](documentdb-sdk-python.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="1d822-108">Krav</span><span class="sxs-lookup"><span data-stu-id="1d822-108">Prerequisites</span></span>

* <span data-ttu-id="1d822-109">Innan du kan köra det här exemplet måste du ha hello följande krav:</span><span class="sxs-lookup"><span data-stu-id="1d822-109">Before you can run this sample, you must have hello following prerequisites:</span></span>
    * <span data-ttu-id="1d822-110">[Visual Studio 2015](http://www.visualstudio.com/) eller senare.</span><span class="sxs-lookup"><span data-stu-id="1d822-110">[Visual Studio 2015](http://www.visualstudio.com/) or higher.</span></span>
    * <span data-ttu-id="1d822-111">Python Tools för Visual Studio från [GitHub](http://microsoft.github.io/PTVS/).</span><span class="sxs-lookup"><span data-stu-id="1d822-111">Python Tools for Visual Studio from [GitHub](http://microsoft.github.io/PTVS/).</span></span> <span data-ttu-id="1d822-112">I den här självstudien används Python Tools för VS 2015.</span><span class="sxs-lookup"><span data-stu-id="1d822-112">This tutorial uses Python Tools for VS 2015.</span></span>
    * <span data-ttu-id="1d822-113">Python 2.7 från [python.org](https://www.python.org/downloads/release/python-2712/)</span><span class="sxs-lookup"><span data-stu-id="1d822-113">Python 2.7 from [python.org](https://www.python.org/downloads/release/python-2712/)</span></span>

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-a-database-account"></a><span data-ttu-id="1d822-114">Skapa ett databaskonto</span><span class="sxs-lookup"><span data-stu-id="1d822-114">Create a database account</span></span>

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

## <a name="add-a-collection"></a><span data-ttu-id="1d822-115">Lägga till en samling</span><span class="sxs-lookup"><span data-stu-id="1d822-115">Add a collection</span></span>

[!INCLUDE [cosmos-db-create-collection](../../includes/cosmos-db-create-collection.md)]

## <a name="clone-hello-sample-application"></a><span data-ttu-id="1d822-116">Klona hello exempelprogrammet</span><span class="sxs-lookup"><span data-stu-id="1d822-116">Clone hello sample application</span></span>

<span data-ttu-id="1d822-117">Nu ska vi klona en DocumentDB-API-app från github, ange hello anslutningssträngen och kör den.</span><span class="sxs-lookup"><span data-stu-id="1d822-117">Now let's clone a DocumentDB API app from github, set hello connection string, and run it.</span></span> <span data-ttu-id="1d822-118">Du ser hur enkelt är det toowork med data programmässigt.</span><span class="sxs-lookup"><span data-stu-id="1d822-118">You see how easy it is toowork with data programmatically.</span></span> 

1. <span data-ttu-id="1d822-119">Öppna ett git terminalfönster, till exempel git bash och `cd` tooa arbetskatalogen.</span><span class="sxs-lookup"><span data-stu-id="1d822-119">Open a git terminal window, such as git bash, and `cd` tooa working directory.</span></span>  

2. <span data-ttu-id="1d822-120">Hello kör följande kommando tooclone hello exempel lagringsplatsen.</span><span class="sxs-lookup"><span data-stu-id="1d822-120">Run hello following command tooclone hello sample repository.</span></span> 

    ```bash
    git clone https://github.com/Azure-Samples/azure-cosmos-db-documentdb-python-getting-started.git
    ```  
## <a name="review-hello-code"></a><span data-ttu-id="1d822-121">Granska hello kod</span><span class="sxs-lookup"><span data-stu-id="1d822-121">Review hello code</span></span>

<span data-ttu-id="1d822-122">Låt oss göra en snabb genomgång av vad som händer i hello app.</span><span class="sxs-lookup"><span data-stu-id="1d822-122">Let's make a quick review of what's happening in hello app.</span></span> <span data-ttu-id="1d822-123">Öppna hello DocumentDBGetStarted.py fil- och du hittar att dessa rader med kod skapar hello Azure Cosmos DB resurser.</span><span class="sxs-lookup"><span data-stu-id="1d822-123">Open hello DocumentDBGetStarted.py file and you'll find that these lines of code create hello Azure Cosmos DB resources.</span></span> 


* <span data-ttu-id="1d822-124">Hej DocumentClient har initierats.</span><span class="sxs-lookup"><span data-stu-id="1d822-124">hello DocumentClient is initialized.</span></span>

    ```python
    # Initialize hello Python DocumentDB client
    client = document_client.DocumentClient(config['ENDPOINT'], {'masterKey': config['MASTERKEY']})
    ```

* <span data-ttu-id="1d822-125">En ny databas skapas.</span><span class="sxs-lookup"><span data-stu-id="1d822-125">A new database is created.</span></span>

    ```python
    # Create a database
    db = client.CreateDatabase({ 'id': config['DOCUMENTDB_DATABASE'] })
    ```

* <span data-ttu-id="1d822-126">En ny samling skapas.</span><span class="sxs-lookup"><span data-stu-id="1d822-126">A new collection is created.</span></span>

    ```python
    # Create collection options
    options = {
        'offerEnableRUPerMinuteThroughput': True,
        'offerVersion': "V2",
        'offerThroughput': 400
    }

    # Create a collection
    collection = client.CreateCollection(db['_self'], { 'id': config['DOCUMENTDB_COLLECTION'] }, options)
    ```

* <span data-ttu-id="1d822-127">Vissa dokument skapas.</span><span class="sxs-lookup"><span data-stu-id="1d822-127">Some documents are created.</span></span>

    ```python
    # Create some documents
    document1 = client.CreateDocument(collection['_self'],
        { 
            'id': 'server1',
            'Web Site': 0,
            'Cloud Service': 0,
            'Virtual Machine': 0,
            'name': 'some' 
        })
    ```

* <span data-ttu-id="1d822-128">En fråga utförs med hjälp av SQL</span><span class="sxs-lookup"><span data-stu-id="1d822-128">A query is performed using SQL</span></span>

    ```python
    # Query them in SQL
    query = { 'query': 'SELECT * FROM server s' }    
            
    options = {} 
    options['enableCrossPartitionQuery'] = True
    options['maxItemCount'] = 2

    result_iterable = client.QueryDocuments(collection['_self'], query, options)
    results = list(result_iterable);

    print(results)
    ```

## <a name="update-your-connection-string"></a><span data-ttu-id="1d822-129">Uppdatera din anslutningssträng</span><span class="sxs-lookup"><span data-stu-id="1d822-129">Update your connection string</span></span>

<span data-ttu-id="1d822-130">Gå tillbaka toohello Azure portal tooget din Anslutningssträngsinformation nu och kopierar den till hello app.</span><span class="sxs-lookup"><span data-stu-id="1d822-130">Now go back toohello Azure portal tooget your connection string information and copy it into hello app.</span></span>

1. <span data-ttu-id="1d822-131">I hello [Azure-portalen](http://portal.azure.com/), i din Azure Cosmos DB konto, i hello vänstra navigeringsrutan klickar du på **nycklar**, och klicka sedan på **skrivskyddad nycklar**.</span><span class="sxs-lookup"><span data-stu-id="1d822-131">In hello [Azure portal](http://portal.azure.com/), in your Azure Cosmos DB account, in hello left navigation click **Keys**, and then click **Read-write Keys**.</span></span> <span data-ttu-id="1d822-132">Du måste använda hello kopiera knappar i hello höger sida av hello skärmen toocopy hello URI och primärnyckel i hello `DocumentDBGetStarted.py` filen i hello nästa steg.</span><span class="sxs-lookup"><span data-stu-id="1d822-132">You'll use hello copy buttons on hello right side of hello screen toocopy hello URI and Primary Key into hello `DocumentDBGetStarted.py` file in hello next step.</span></span>

    ![Visa och kopiera en snabbtangent i hello Azure-portalen, bladet nycklar](./media/create-documentdb-dotnet/keys.png)

2. <span data-ttu-id="1d822-134">I öppna hello `DocumentDBGetStarted.py` fil.</span><span class="sxs-lookup"><span data-stu-id="1d822-134">In Open hello `DocumentDBGetStarted.py` file.</span></span> 

3. <span data-ttu-id="1d822-135">Kopiera URI-värdet från hello-portal (med hello kopieringsknappen) och gör den hello värdet för hello endpoint nyckeln i `DocumentDBGetStarted.py`.</span><span class="sxs-lookup"><span data-stu-id="1d822-135">Copy your URI value from hello portal (using hello copy button) and make it hello value of hello endpoint key in `DocumentDBGetStarted.py`.</span></span> 

    `config.ENDPOINT : "https://FILLME.documents.azure.com"`

4. <span data-ttu-id="1d822-136">Kopiera värdet för PRIMÄRNYCKELN från hello-portalen och gör det hello värdet för hello `config.MASTERKEY` i `DocumentDBGetStarted.py`.</span><span class="sxs-lookup"><span data-stu-id="1d822-136">Then copy your PRIMARY KEY value from hello portal and make it hello value of hello `config.MASTERKEY` in `DocumentDBGetStarted.py`.</span></span> <span data-ttu-id="1d822-137">Du har nu uppdaterat din app med alla hello information som behövs för toocommunicate med Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="1d822-137">You've now updated your app with all hello info it needs toocommunicate with Azure Cosmos DB.</span></span> 

    `config.MASTERKEY : "FILLME"`
    
## <a name="run-hello-app"></a><span data-ttu-id="1d822-138">Kör hello-appen</span><span class="sxs-lookup"><span data-stu-id="1d822-138">Run hello app</span></span>
1. <span data-ttu-id="1d822-139">Högerklicka på hello-projekt i Visual Studio **Solution Explorer**, Välj hello aktuella Python-miljön, högerklicka och klicka sedan.</span><span class="sxs-lookup"><span data-stu-id="1d822-139">In Visual Studio, right-click on hello project in **Solution Explorer**, select hello current Python environment, then right click.</span></span>

2. <span data-ttu-id="1d822-140">Välj alternativet för att installera Python Package och skriv **pydocumentdb**</span><span class="sxs-lookup"><span data-stu-id="1d822-140">Select Install Python Package, then type in **pydocumentdb**</span></span>

3. <span data-ttu-id="1d822-141">Kör F5 toorun hello program.</span><span class="sxs-lookup"><span data-stu-id="1d822-141">Run F5 toorun hello application.</span></span> <span data-ttu-id="1d822-142">Appen visas i din webbläsare.</span><span class="sxs-lookup"><span data-stu-id="1d822-142">Your app displays in your browser.</span></span> 

<span data-ttu-id="1d822-143">Du kan nu gå tillbaka tooData Explorer Se fråga, ändra och arbeta med dessa nya data.</span><span class="sxs-lookup"><span data-stu-id="1d822-143">You can now go back tooData Explorer and see query, modify, and work with this new data.</span></span> 

## <a name="review-slas-in-hello-azure-portal"></a><span data-ttu-id="1d822-144">Granska SLA: er i hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="1d822-144">Review SLAs in hello Azure portal</span></span>

[!INCLUDE [cosmosdb-tutorial-review-slas](../../includes/cosmos-db-tutorial-review-slas.md)]

## <a name="clean-up-resources"></a><span data-ttu-id="1d822-145">Rensa resurser</span><span class="sxs-lookup"><span data-stu-id="1d822-145">Clean up resources</span></span>

<span data-ttu-id="1d822-146">Om du inte kommer toocontinue toouse den här appen, tar du bort alla resurser som skapats av denna Snabbstart i hello Azure-portalen med hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="1d822-146">If you're not going toocontinue toouse this app, delete all resources created by this quickstart in hello Azure portal with hello following steps:</span></span>

1. <span data-ttu-id="1d822-147">Hello vänstra menyn i hello Azure-portalen klickar du på **resursgrupper** och klicka sedan på hello namnet på hello resurs du skapat.</span><span class="sxs-lookup"><span data-stu-id="1d822-147">From hello left-hand menu in hello Azure portal, click **Resource groups** and then click hello name of hello resource you created.</span></span> 
2. <span data-ttu-id="1d822-148">På din resurs gruppen klickar du på **ta bort**typnamn hello för hello resurs toodelete i hello textrutan och klicka sedan på **ta bort**.</span><span class="sxs-lookup"><span data-stu-id="1d822-148">On your resource group page, click **Delete**, type hello name of hello resource toodelete in hello text box, and then click **Delete**.</span></span>

## <a name="next-steps"></a><span data-ttu-id="1d822-149">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="1d822-149">Next steps</span></span>

<span data-ttu-id="1d822-150">Du har lärt dig hur toocreate ett Azure DB som Cosmos-konto, skapa en samling med hello Data Explorer och kör en app i denna Snabbstart.</span><span class="sxs-lookup"><span data-stu-id="1d822-150">In this quickstart, you've learned how toocreate an Azure Cosmos DB account, create a collection using hello Data Explorer, and run an app.</span></span> <span data-ttu-id="1d822-151">Nu kan du importera ytterligare data tooyour Cosmos-DB-konto.</span><span class="sxs-lookup"><span data-stu-id="1d822-151">You can now import additional data tooyour Cosmos DB account.</span></span> 

> [!div class="nextstepaction"]
> [<span data-ttu-id="1d822-152">Importera data till Azure Cosmos DB för hello DocumentDB-API</span><span class="sxs-lookup"><span data-stu-id="1d822-152">Import data into Azure Cosmos DB for hello DocumentDB API</span></span>](import-data.md)


