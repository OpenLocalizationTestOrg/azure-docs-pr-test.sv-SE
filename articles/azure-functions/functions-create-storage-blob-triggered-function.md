---
title: "aaaCreate en funktion i Azure som utlösts av Blob storage | Microsoft Docs"
description: "Använd Azure Functions toocreate en serverlösa funktion som anropas av objekt som lagts till tooAzure Blob storage."
services: azure-functions
documentationcenter: na
author: ggailey777
manager: erikre
editor: 
tags: 
ms.assetid: d6bff41c-a624-40c1-bbc7-80590df29ded
ms.service: functions
ms.devlang: multiple
ms.topic: quickstart
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 05/31/2017
ms.author: glenga
ms.custom: mvc
ms.openlocfilehash: acb7d29abb07a22da11d0e65d2ed54591f8e3f4f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-function-triggered-by-azure-blob-storage"></a><span data-ttu-id="3d0da-103">Skapa en funktion som utlöses av Azure Blob Storage</span><span class="sxs-lookup"><span data-stu-id="3d0da-103">Create a function triggered by Azure Blob storage</span></span>

<span data-ttu-id="3d0da-104">Lär dig hur toocreate en funktion som utlöses när filerna har överförts tooor uppdateras i Azure Blob storage.</span><span class="sxs-lookup"><span data-stu-id="3d0da-104">Learn how toocreate a function triggered when files are uploaded tooor updated in Azure Blob storage.</span></span>

![Visa meddelandet i hello loggar.](./media/functions-create-storage-blob-triggered-function/function-app-in-portal-editor.png)

## <a name="prerequisites"></a><span data-ttu-id="3d0da-106">Krav</span><span class="sxs-lookup"><span data-stu-id="3d0da-106">Prerequisites</span></span>

+ <span data-ttu-id="3d0da-107">Hämta och installera hello [Microsoft Azure Lagringsutforskaren](http://storageexplorer.com/).</span><span class="sxs-lookup"><span data-stu-id="3d0da-107">Download and install hello [Microsoft Azure Storage Explorer](http://storageexplorer.com/).</span></span>
+ <span data-ttu-id="3d0da-108">En Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="3d0da-108">An Azure subscription.</span></span> <span data-ttu-id="3d0da-109">Om du inte har ett konto kan du skapa ett [kostnadsfritt konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) innan du börjar.</span><span class="sxs-lookup"><span data-stu-id="3d0da-109">If you don't have one, create a [free account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) before you begin.</span></span>

[!INCLUDE [functions-portal-favorite-function-apps](../../includes/functions-portal-favorite-function-apps.md)]

## <a name="create-an-azure-function-app"></a><span data-ttu-id="3d0da-110">Skapa en Azure Functions-app</span><span class="sxs-lookup"><span data-stu-id="3d0da-110">Create an Azure Function app</span></span>

[!INCLUDE [Create function app Azure portal](../../includes/functions-create-function-app-portal.md)]

![Funktionsappen skapades.](./media/functions-create-first-azure-function/function-app-create-success.png)

<span data-ttu-id="3d0da-112">Därefter skapar du en funktion i hello ny funktionsapp.</span><span class="sxs-lookup"><span data-stu-id="3d0da-112">Next, you create a function in hello new function app.</span></span>

<a name="create-function"></a>

## <a name="create-a-blob-storage-triggered-function"></a><span data-ttu-id="3d0da-113">Skapa en funktion som utlöses av Blob Storage</span><span class="sxs-lookup"><span data-stu-id="3d0da-113">Create a Blob storage triggered function</span></span>

1. <span data-ttu-id="3d0da-114">Expandera funktionen appen och klicka på hello  **+**  knappen för nästa**funktioner**.</span><span class="sxs-lookup"><span data-stu-id="3d0da-114">Expand your function app and click hello **+** button next too**Functions**.</span></span> <span data-ttu-id="3d0da-115">Om det är första hello-funktion i din funktionsapp **anpassad funktionen**.</span><span class="sxs-lookup"><span data-stu-id="3d0da-115">If this is hello first function in your function app, select **Custom function**.</span></span> <span data-ttu-id="3d0da-116">Detta visar hello fullständig uppsättning funktionen mallar.</span><span class="sxs-lookup"><span data-stu-id="3d0da-116">This displays hello complete set of function templates.</span></span>

    ![Funktioner quickstart sida i hello Azure-portalen](./media/functions-create-storage-blob-triggered-function/add-first-function.png)

2. <span data-ttu-id="3d0da-118">Välj hello **BlobTrigger** mall för önskat språk och Använd hello inställningar som anges i hello tabell.</span><span class="sxs-lookup"><span data-stu-id="3d0da-118">Select hello **BlobTrigger** template for your desired language, and use hello settings as specified in hello table.</span></span>

    ![Skapa hello Blob utlöses lagringsfunktionen.](./media/functions-create-storage-blob-triggered-function/functions-create-blob-storage-trigger-portal.png)

    | <span data-ttu-id="3d0da-120">Inställning</span><span class="sxs-lookup"><span data-stu-id="3d0da-120">Setting</span></span> | <span data-ttu-id="3d0da-121">Föreslaget värde</span><span class="sxs-lookup"><span data-stu-id="3d0da-121">Suggested value</span></span> | <span data-ttu-id="3d0da-122">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="3d0da-122">Description</span></span> |
    |---|---|---|
    | <span data-ttu-id="3d0da-123">**Sökväg**</span><span class="sxs-lookup"><span data-stu-id="3d0da-123">**Path**</span></span>   | <span data-ttu-id="3d0da-124">mycontainer /{namn}</span><span class="sxs-lookup"><span data-stu-id="3d0da-124">mycontainer/{name}</span></span>    | <span data-ttu-id="3d0da-125">Platsen i Blob Storage som övervakas.</span><span class="sxs-lookup"><span data-stu-id="3d0da-125">Location in Blob storage being monitored.</span></span> <span data-ttu-id="3d0da-126">hello filnamn hello blob skickas i hello bindning som hello _namn_ parameter.</span><span class="sxs-lookup"><span data-stu-id="3d0da-126">hello file name of hello blob is passed in hello binding as hello _name_ parameter.</span></span>  |
    | <span data-ttu-id="3d0da-127">**Lagringskontoanslutning**</span><span class="sxs-lookup"><span data-stu-id="3d0da-127">**Storage account connection**</span></span> | <span data-ttu-id="3d0da-128">AzureWebJobStorage</span><span class="sxs-lookup"><span data-stu-id="3d0da-128">AzureWebJobStorage</span></span> | <span data-ttu-id="3d0da-129">Du kan använda hello konto lagringsanslutning redan används av din funktionsapp eller skapa en ny.</span><span class="sxs-lookup"><span data-stu-id="3d0da-129">You can use hello storage account connection already being used by your function app, or create a new one.</span></span>  |
    | <span data-ttu-id="3d0da-130">**Namnge din funktion**</span><span class="sxs-lookup"><span data-stu-id="3d0da-130">**Name your function**</span></span> | <span data-ttu-id="3d0da-131">Ett unikt namn i funktionsappen</span><span class="sxs-lookup"><span data-stu-id="3d0da-131">Unique in your function app</span></span> | <span data-ttu-id="3d0da-132">Namnge funktionen som utlöses av blobben.</span><span class="sxs-lookup"><span data-stu-id="3d0da-132">Name of this blob triggered function.</span></span> |

3. <span data-ttu-id="3d0da-133">Klicka på **skapa** toocreate din funktion.</span><span class="sxs-lookup"><span data-stu-id="3d0da-133">Click **Create** toocreate your function.</span></span>

<span data-ttu-id="3d0da-134">Därefter måste du ansluta tooyour Azure Storage-konto och skapa hello **minbehållare** behållare.</span><span class="sxs-lookup"><span data-stu-id="3d0da-134">Next, you connect tooyour Azure Storage account and create hello **mycontainer** container.</span></span>

## <a name="create-hello-container"></a><span data-ttu-id="3d0da-135">Skapa hello behållare</span><span class="sxs-lookup"><span data-stu-id="3d0da-135">Create hello container</span></span>

1. <span data-ttu-id="3d0da-136">Klicka på **Integrera** i din funktion, expandera **Dokumentation** och kopiera både **kontonamnet** och **kontonyckeln**.</span><span class="sxs-lookup"><span data-stu-id="3d0da-136">In your function, click **Integrate**, expand **Documentation**, and copy both **Account name** and **Account key**.</span></span> <span data-ttu-id="3d0da-137">Du använder dessa autentiseringsuppgifter tooconnect toohello storage-konto.</span><span class="sxs-lookup"><span data-stu-id="3d0da-137">You use these credentials tooconnect toohello storage account.</span></span> <span data-ttu-id="3d0da-138">Hoppa över toostep 4 om du redan har anslutit ditt lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="3d0da-138">If you have already connected your storage account, skip toostep 4.</span></span>

    ![Hämta hello Storage-konto med autentiseringsuppgifter för anslutning.](./media/functions-create-storage-blob-triggered-function/functions-storage-account-connection.png)

1. <span data-ttu-id="3d0da-140">Kör hello [Microsoft Azure Lagringsutforskaren](http://storageexplorer.com/) verktyg, klicka på hello ansluta ikonen hello vänster, Välj **använder ett lagringskontonamn och nyckel**, och klicka på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="3d0da-140">Run hello [Microsoft Azure Storage Explorer](http://storageexplorer.com/) tool, click hello connect icon on hello left, choose **Use a storage account name and key**, and click **Next**.</span></span>

    ![Köra hello Lagringsutforskaren för kontot.](./media/functions-create-storage-blob-triggered-function/functions-storage-manager-connect-1.png)

1. <span data-ttu-id="3d0da-142">Ange hello **kontonamn** och **kontonyckel** från steg 1, klickar du på **nästa** och sedan **Anslut**.</span><span class="sxs-lookup"><span data-stu-id="3d0da-142">Enter hello **Account name** and **Account key** from step 1, click **Next** and then **Connect**.</span></span> 

    ![Ange autentiseringsuppgifter för lagring av hello och ansluta.](./media/functions-create-storage-blob-triggered-function/functions-storage-manager-connect-2.png)

1. <span data-ttu-id="3d0da-144">Expandera hello kopplade storage-konto, högerklicka på **Blob-behållare**, klickar du på **skapa blob-behållaren**, typen `mycontainer`, och tryck sedan på RETUR.</span><span class="sxs-lookup"><span data-stu-id="3d0da-144">Expand hello attached storage account, right-click **Blob containers**, click **Create blob container**, type `mycontainer`, and then press enter.</span></span>

    ![Skapa en lagringskö.](./media/functions-create-storage-blob-triggered-function/functions-storage-manager-create-blob-container.png)

<span data-ttu-id="3d0da-146">Nu när du har en blob-behållare kan testa du hello funktionen genom att överföra en fil toohello behållare.</span><span class="sxs-lookup"><span data-stu-id="3d0da-146">Now that you have a blob container, you can test hello function by uploading a file toohello container.</span></span>

## <a name="test-hello-function"></a><span data-ttu-id="3d0da-147">Testa hello-funktionen</span><span class="sxs-lookup"><span data-stu-id="3d0da-147">Test hello function</span></span>

1. <span data-ttu-id="3d0da-148">Tillbaka i hello Azure-portalen, bläddra tooyour funktionen expanderar hello **loggar** hello längst ned i hello sidan och se till att loggen strömning inte pausades.</span><span class="sxs-lookup"><span data-stu-id="3d0da-148">Back in hello Azure portal, browse tooyour function expand hello **Logs** at hello bottom of hello page and make sure that log streaming isn't paused.</span></span>

1. <span data-ttu-id="3d0da-149">Expandera ditt lagringskonto, **Blob containers** (Blobbehållare) och **mycontainer** i Storage Explorer.</span><span class="sxs-lookup"><span data-stu-id="3d0da-149">In Storage Explorer, expand your storage account, **Blob containers**, and **mycontainer**.</span></span> <span data-ttu-id="3d0da-150">Klicka på **Ladda upp** och sedan på **Ladda upp filer**.</span><span class="sxs-lookup"><span data-stu-id="3d0da-150">Click **Upload** and then **Upload files...**.</span></span>

    ![Överför en fil toohello blob-behållare.](./media/functions-create-storage-blob-triggered-function/functions-storage-manager-upload-file-blob.png)

1. <span data-ttu-id="3d0da-152">I hello **ladda upp filer** dialogrutan klickar du på hello **filer** fältet.</span><span class="sxs-lookup"><span data-stu-id="3d0da-152">In hello **Upload files** dialog box, click hello **Files** field.</span></span> <span data-ttu-id="3d0da-153">Bläddra efter tooa fil på den lokala datorn, till exempel en bildfil, markerar du den och klickar på **öppna** och sedan **överför**.</span><span class="sxs-lookup"><span data-stu-id="3d0da-153">Browse tooa file on your local computer, such as an image file, select it and click **Open** and then **Upload**.</span></span>

1. <span data-ttu-id="3d0da-154">Gå tillbaka tooyour funktionsloggar och kontrollera att hello-blob har lästs.</span><span class="sxs-lookup"><span data-stu-id="3d0da-154">Go back tooyour function logs and verify that hello blob has been read.</span></span>

   ![Visa meddelandet i hello loggar.](./media/functions-create-storage-blob-triggered-function/functions-blob-storage-trigger-view-logs.png)

    >[!NOTE]
    > <span data-ttu-id="3d0da-156">När funktionen appen körs i hello standardplanen förbrukning, kan det uppstå en fördröjning på upp tooseveral minuter mellan hello blob som lagts till eller uppdaterats och hello fungera som utlöses.</span><span class="sxs-lookup"><span data-stu-id="3d0da-156">When your function app runs in hello default Consumption plan, there may be a delay of up tooseveral minutes between hello blob being added or updated and hello function being triggered.</span></span> <span data-ttu-id="3d0da-157">Om du behöver låg latens i dina blobutlösta funktioner bör du köra funktionsapparna med en App Service-plan.</span><span class="sxs-lookup"><span data-stu-id="3d0da-157">If you need low latency in your blob triggered functions, consider running your function app in an App Service plan.</span></span>

## <a name="clean-up-resources"></a><span data-ttu-id="3d0da-158">Rensa resurser</span><span class="sxs-lookup"><span data-stu-id="3d0da-158">Clean up resources</span></span>

[!INCLUDE [Next steps note](../../includes/functions-quickstart-cleanup.md)]

## <a name="next-steps"></a><span data-ttu-id="3d0da-159">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="3d0da-159">Next steps</span></span>

<span data-ttu-id="3d0da-160">Du har skapat en funktion som körs när en blob läggs tooor uppdateras i Blob storage.</span><span class="sxs-lookup"><span data-stu-id="3d0da-160">You have created a function that runs when a blob is added tooor updated in Blob storage.</span></span> 

[!INCLUDE [Next steps note](../../includes/functions-quickstart-next-steps.md)]

<span data-ttu-id="3d0da-161">Mer information om Blob Storage-utlösare finns i [Azure Functions Blob storage bindings](functions-bindings-storage-blob.md) (Blob Storage-bindningar i Azure Functions).</span><span class="sxs-lookup"><span data-stu-id="3d0da-161">For more information about Blob storage triggers, see [Azure Functions Blob storage bindings](functions-bindings-storage-blob.md).</span></span>
