---
title: "aaaCreate en funktion i Azure som utlösts av Kömeddelanden | Microsoft Docs"
description: "Använd Azure Functions toocreate en serverlösa funktion som anropas av en meddelanden som skickats tooan Azure Storage-kö."
services: azure-functions
documentationcenter: na
author: ggailey777
manager: erikre
editor: 
tags: 
ms.assetid: 361da2a4-15d1-4903-bdc4-cc4b27fc3ff4
ms.service: functions
ms.devlang: multiple
ms.topic: quickstart
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 05/31/2017
ms.author: glenga
ms.custom: mvc
ms.openlocfilehash: e9501ed336b502eaeee3fa62ec4ae085c76de0ac
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-function-triggered-by-azure-queue-storage"></a><span data-ttu-id="2575a-103">Skapa en funktion som utlöses av Azure Queue Storage</span><span class="sxs-lookup"><span data-stu-id="2575a-103">Create a function triggered by Azure Queue storage</span></span>

<span data-ttu-id="2575a-104">Lär dig hur toocreate en funktion som utlöses när meddelanden skickas tooan Azure Storage-kö.</span><span class="sxs-lookup"><span data-stu-id="2575a-104">Learn how toocreate a function triggered when messages are submitted tooan Azure Storage queue.</span></span>

![Visa meddelandet i hello loggar.](./media/functions-create-storage-queue-triggered-function/function-app-in-portal-editor.png)

## <a name="prerequisites"></a><span data-ttu-id="2575a-106">Krav</span><span class="sxs-lookup"><span data-stu-id="2575a-106">Prerequisites</span></span>

- <span data-ttu-id="2575a-107">Hämta och installera hello [Microsoft Azure Lagringsutforskaren](http://storageexplorer.com/).</span><span class="sxs-lookup"><span data-stu-id="2575a-107">Download and install hello [Microsoft Azure Storage Explorer](http://storageexplorer.com/).</span></span>

- <span data-ttu-id="2575a-108">En Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="2575a-108">An Azure subscription.</span></span> <span data-ttu-id="2575a-109">Om du inte har ett konto kan du skapa ett [kostnadsfritt konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) innan du börjar.</span><span class="sxs-lookup"><span data-stu-id="2575a-109">If you don't have one, create a [free account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) before you begin.</span></span>

[!INCLUDE [functions-portal-favorite-function-apps](../../includes/functions-portal-favorite-function-apps.md)]

## <a name="create-an-azure-function-app"></a><span data-ttu-id="2575a-110">Skapa en Azure Functions-app</span><span class="sxs-lookup"><span data-stu-id="2575a-110">Create an Azure Function app</span></span>

[!INCLUDE [Create function app Azure portal](../../includes/functions-create-function-app-portal.md)]

![Funktionsappen skapades.](./media/functions-create-first-azure-function/function-app-create-success.png)

<span data-ttu-id="2575a-112">Därefter skapar du en funktion i hello ny funktionsapp.</span><span class="sxs-lookup"><span data-stu-id="2575a-112">Next, you create a function in hello new function app.</span></span>

<a name="create-function"></a>

## <a name="create-a-queue-triggered-function"></a><span data-ttu-id="2575a-113">Skapa en funktion som utlöses av en kö</span><span class="sxs-lookup"><span data-stu-id="2575a-113">Create a Queue triggered function</span></span>

1. <span data-ttu-id="2575a-114">Expandera funktionen appen och klicka på hello  **+**  knappen för nästa**funktioner**.</span><span class="sxs-lookup"><span data-stu-id="2575a-114">Expand your function app and click hello **+** button next too**Functions**.</span></span> <span data-ttu-id="2575a-115">Om det är första hello-funktion i din funktionsapp **anpassad funktionen**.</span><span class="sxs-lookup"><span data-stu-id="2575a-115">If this is hello first function in your function app, select **Custom function**.</span></span> <span data-ttu-id="2575a-116">Detta visar hello fullständig uppsättning funktionen mallar.</span><span class="sxs-lookup"><span data-stu-id="2575a-116">This displays hello complete set of function templates.</span></span>

    ![Funktioner quickstart sida i hello Azure-portalen](./media/functions-create-storage-queue-triggered-function/add-first-function.png)

2. <span data-ttu-id="2575a-118">Välj hello **QueueTrigger** mall för önskat språk och Använd hello inställningar som anges i hello tabell.</span><span class="sxs-lookup"><span data-stu-id="2575a-118">Select hello **QueueTrigger** template for your desired language, and  use hello settings as specified in hello table.</span></span>

    ![Skapa hello lagringsfunktionen kön utlöses.](./media/functions-create-storage-queue-triggered-function/functions-create-queue-storage-trigger-portal.png)
    
    | <span data-ttu-id="2575a-120">Inställning</span><span class="sxs-lookup"><span data-stu-id="2575a-120">Setting</span></span> | <span data-ttu-id="2575a-121">Föreslaget värde</span><span class="sxs-lookup"><span data-stu-id="2575a-121">Suggested value</span></span> | <span data-ttu-id="2575a-122">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="2575a-122">Description</span></span> |
    |---|---|---|
    | <span data-ttu-id="2575a-123">**Könamn**</span><span class="sxs-lookup"><span data-stu-id="2575a-123">**Queue name**</span></span>   | <span data-ttu-id="2575a-124">myqueue-items</span><span class="sxs-lookup"><span data-stu-id="2575a-124">myqueue-items</span></span>    | <span data-ttu-id="2575a-125">Namnet på hello kö tooconnect tooin ditt lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="2575a-125">Name of hello queue tooconnect tooin your Storage account.</span></span> |
    | <span data-ttu-id="2575a-126">**Lagringskontoanslutning**</span><span class="sxs-lookup"><span data-stu-id="2575a-126">**Storage account connection**</span></span> | <span data-ttu-id="2575a-127">AzureWebJobStorage</span><span class="sxs-lookup"><span data-stu-id="2575a-127">AzureWebJobStorage</span></span> | <span data-ttu-id="2575a-128">Du kan använda hello konto lagringsanslutning redan används av din funktionsapp eller skapa en ny.</span><span class="sxs-lookup"><span data-stu-id="2575a-128">You can use hello storage account connection already being used by your function app, or create a new one.</span></span>  |
    | <span data-ttu-id="2575a-129">**Namnge din funktion**</span><span class="sxs-lookup"><span data-stu-id="2575a-129">**Name your function**</span></span> | <span data-ttu-id="2575a-130">Ett unikt namn i funktionsappen</span><span class="sxs-lookup"><span data-stu-id="2575a-130">Unique in your function app</span></span> | <span data-ttu-id="2575a-131">Namnge funktionen som utlöses av kön.</span><span class="sxs-lookup"><span data-stu-id="2575a-131">Name of this queue triggered function.</span></span> |

3. <span data-ttu-id="2575a-132">Klicka på **skapa** toocreate din funktion.</span><span class="sxs-lookup"><span data-stu-id="2575a-132">Click **Create** toocreate your function.</span></span>

<span data-ttu-id="2575a-133">Därefter måste du ansluta tooyour Azure Storage-konto och skapa hello **MinKö objekt** storage-kö.</span><span class="sxs-lookup"><span data-stu-id="2575a-133">Next, you connect tooyour Azure Storage account and create hello **myqueue-items** storage queue.</span></span>

## <a name="create-hello-queue"></a><span data-ttu-id="2575a-134">Skapa hello kö</span><span class="sxs-lookup"><span data-stu-id="2575a-134">Create hello queue</span></span>

1. <span data-ttu-id="2575a-135">Klicka på **Integrera** i din funktion, expandera **Dokumentation** och kopiera både **kontonamnet** och **kontonyckeln**.</span><span class="sxs-lookup"><span data-stu-id="2575a-135">In your function, click **Integrate**, expand **Documentation**, and copy both **Account name** and **Account key**.</span></span> <span data-ttu-id="2575a-136">Du använder dessa autentiseringsuppgifter tooconnect toohello storage-konto.</span><span class="sxs-lookup"><span data-stu-id="2575a-136">You use these credentials tooconnect toohello storage account.</span></span> <span data-ttu-id="2575a-137">Hoppa över toostep 4 om du redan har anslutit ditt lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="2575a-137">If you have already connected your storage account, skip toostep 4.</span></span>

    ![Hämta hello Storage-konto med autentiseringsuppgifter för anslutning.](./media/functions-create-storage-queue-triggered-function/functions-storage-account-connection.png)<span data-ttu-id="2575a-139">v</span><span class="sxs-lookup"><span data-stu-id="2575a-139">v</span></span>

1. <span data-ttu-id="2575a-140">Kör hello [Microsoft Azure Lagringsutforskaren](http://storageexplorer.com/) verktyg, klicka på hello ansluta ikonen hello vänster, Välj **använder ett lagringskontonamn och nyckel**, och klicka på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="2575a-140">Run hello [Microsoft Azure Storage Explorer](http://storageexplorer.com/) tool, click hello connect icon on hello left, choose **Use a storage account name and key**, and click **Next**.</span></span>

    ![Köra hello Lagringsutforskaren för kontot.](./media/functions-create-storage-queue-triggered-function/functions-storage-manager-connect-1.png)

1. <span data-ttu-id="2575a-142">Ange hello **kontonamn** och **kontonyckel** från steg 1, klickar du på **nästa** och sedan **Anslut**.</span><span class="sxs-lookup"><span data-stu-id="2575a-142">Enter hello **Account name** and **Account key** from step 1, click **Next** and then **Connect**.</span></span>

    ![Ange autentiseringsuppgifter för lagring av hello och ansluta.](./media/functions-create-storage-queue-triggered-function/functions-storage-manager-connect-2.png)

1. <span data-ttu-id="2575a-144">Expandera hello kopplade storage-konto, högerklicka på **köer**, klickar du på **Skapa kö**, typen `myqueue-items`, och tryck sedan på RETUR.</span><span class="sxs-lookup"><span data-stu-id="2575a-144">Expand hello attached storage account, right-click **Queues**, click **Create queue**, type `myqueue-items`, and then press enter.</span></span>

    ![Skapa en lagringskö.](./media/functions-create-storage-queue-triggered-function/functions-storage-manager-create-queue.png)

<span data-ttu-id="2575a-146">Nu när du har en kö för lagring kan testa du hello funktionen genom att lägga till en meddelandekö toohello.</span><span class="sxs-lookup"><span data-stu-id="2575a-146">Now that you have a storage queue, you can test hello function by adding a message toohello queue.</span></span>

## <a name="test-hello-function"></a><span data-ttu-id="2575a-147">Testa hello-funktionen</span><span class="sxs-lookup"><span data-stu-id="2575a-147">Test hello function</span></span>

1. <span data-ttu-id="2575a-148">Tillbaka i hello Azure-portalen, bläddra tooyour funktionen expanderar hello **loggar** hello längst ned i hello sidan och se till att loggen strömning inte pausades.</span><span class="sxs-lookup"><span data-stu-id="2575a-148">Back in hello Azure portal, browse tooyour function expand hello **Logs** at hello bottom of hello page and make sure that log streaming isn't paused.</span></span>

1. <span data-ttu-id="2575a-149">I Lagringsutforskaren expanderar du ditt lagringskonto, **Köer** och **myqueue-items**. Klicka sedan på **Lägg till meddelande**.</span><span class="sxs-lookup"><span data-stu-id="2575a-149">In Storage Explorer, expand your storage account, **Queues**, and **myqueue-items**, then click **Add message**.</span></span>

    ![Lägg till en meddelandekö toohello.](./media/functions-create-storage-queue-triggered-function/functions-storage-manager-add-message.png)

1. <span data-ttu-id="2575a-151">Skriv ditt "Hello World!"-</span><span class="sxs-lookup"><span data-stu-id="2575a-151">Type your "Hello World!"</span></span> <span data-ttu-id="2575a-152">meddelande i **Meddelandetext** och klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="2575a-152">message in **Message text** and click **OK**.</span></span>

1. <span data-ttu-id="2575a-153">Vänta några sekunder och sedan gå tillbaka tooyour funktionsloggar och kontrollera att nya hello-meddelande har lästs från hello kö.</span><span class="sxs-lookup"><span data-stu-id="2575a-153">Wait for a few seconds, then go back tooyour function logs and verify that hello new message has been read from hello queue.</span></span>

    ![Visa meddelandet i hello loggar.](./media/functions-create-storage-queue-triggered-function/functions-queue-storage-trigger-view-logs.png)

1. <span data-ttu-id="2575a-155">Klicka på tillbaka i Lagringsutforskaren, **uppdatera** och kontrollera att hello-meddelande har bearbetats och är inte längre i hello kö.</span><span class="sxs-lookup"><span data-stu-id="2575a-155">Back in Storage Explorer, click **Refresh** and verify that hello message has been processed and is no longer in hello queue.</span></span>

## <a name="clean-up-resources"></a><span data-ttu-id="2575a-156">Rensa resurser</span><span class="sxs-lookup"><span data-stu-id="2575a-156">Clean up resources</span></span>

[!INCLUDE [Next steps note](../../includes/functions-quickstart-cleanup.md)]

## <a name="next-steps"></a><span data-ttu-id="2575a-157">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="2575a-157">Next steps</span></span>

<span data-ttu-id="2575a-158">Du har skapat en funktion som körs när ett meddelande läggs tooa storage-kö.</span><span class="sxs-lookup"><span data-stu-id="2575a-158">You have created a function that runs when a message is added tooa storage queue.</span></span>

[!INCLUDE [Next steps note](../../includes/functions-quickstart-next-steps.md)]

<span data-ttu-id="2575a-159">Mer information om Queue Storage-utlösare finns i [Azure Functions Storage queue bindings](functions-bindings-storage-queue.md) (Azure Functions-lagringsköbindningar).</span><span class="sxs-lookup"><span data-stu-id="2575a-159">For more information about Queue storage triggers, see [Azure Functions Storage queue bindings](functions-bindings-storage-queue.md).</span></span>