---
title: "Skapa en funktion i Azure som utlöses av en GitHub-webhook | Microsoft Docs"
description: "Använd Azure Functions för att skapa en funktion utan server som startas av en GitHub-webhook."
services: functions
documentationcenter: na
author: ggailey777
manager: erikre
editor: 
tags: 
ms.assetid: 36ef34b8-3729-4940-86d2-cb8e176fcc06
ms.service: functions
ms.devlang: multiple
ms.topic: quickstart
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 05/31/2017
ms.author: glenga
ms.custom: mvc
ms.openlocfilehash: 038bb4cf0a9278416261c05ddaa0ee97d83b63c5
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="create-a-function-triggered-by-a-github-webhook"></a><span data-ttu-id="0584b-103">Skapa en funktion som utlöses av en GitHub-webhook</span><span class="sxs-lookup"><span data-stu-id="0584b-103">Create a function triggered by a GitHub webhook</span></span>

<span data-ttu-id="0584b-104">Lär dig hur du skapar en funktion som utlöses av en HTTP-webhookbegäran med en GitHub-specifik nyttolast.</span><span class="sxs-lookup"><span data-stu-id="0584b-104">Learn how to create a function that is triggered by an HTTP webhook request with a GitHub-specific payload.</span></span>

![Github-webhookutlöst funktion i Azure Portal](./media/functions-create-github-webhook-triggered-function/function-app-in-portal-editor.png)

## <a name="prerequisites"></a><span data-ttu-id="0584b-106">Krav</span><span class="sxs-lookup"><span data-stu-id="0584b-106">Prerequisites</span></span>

+ <span data-ttu-id="0584b-107">Ett GitHub-konto med minst ett projekt.</span><span class="sxs-lookup"><span data-stu-id="0584b-107">A GitHub account with at least one project.</span></span>
+ <span data-ttu-id="0584b-108">En Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="0584b-108">An Azure subscription.</span></span> <span data-ttu-id="0584b-109">Om du inte har ett konto kan du skapa ett [kostnadsfritt konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) innan du börjar.</span><span class="sxs-lookup"><span data-stu-id="0584b-109">If you don't have one, create a [free account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) before you begin.</span></span>

[!INCLUDE [functions-portal-favorite-function-apps](../../includes/functions-portal-favorite-function-apps.md)]

## <a name="create-an-azure-function-app"></a><span data-ttu-id="0584b-110">Skapa en Azure Functions-app</span><span class="sxs-lookup"><span data-stu-id="0584b-110">Create an Azure Function app</span></span>

[!INCLUDE [Create function app Azure portal](../../includes/functions-create-function-app-portal.md)]

![Funktionsappen skapades.](./media/functions-create-first-azure-function/function-app-create-success.png)

<span data-ttu-id="0584b-112">Därefter skapar du en funktion i den nya funktionsappen.</span><span class="sxs-lookup"><span data-stu-id="0584b-112">Next, you create a function in the new function app.</span></span>

<a name="create-function"></a>

## <a name="create-a-github-webhook-triggered-function"></a><span data-ttu-id="0584b-113">Skapa en webhook-utlöst GitHub-funktion</span><span class="sxs-lookup"><span data-stu-id="0584b-113">Create a GitHub webhook triggered function</span></span>

1. <span data-ttu-id="0584b-114">Expandera funktionsappen och klicka på knappen **+** bredvid **Funktioner**.</span><span class="sxs-lookup"><span data-stu-id="0584b-114">Expand your function app and click the **+** button next to **Functions**.</span></span> <span data-ttu-id="0584b-115">Om det är den första funktionen i din funktionsapp väljer du **Anpassad funktion**.</span><span class="sxs-lookup"><span data-stu-id="0584b-115">If this is the first function in your function app, select **Custom function**.</span></span> <span data-ttu-id="0584b-116">Detta visar en fullständig uppsättning med funktionsmallar.</span><span class="sxs-lookup"><span data-stu-id="0584b-116">This displays the complete set of function templates.</span></span>

    ![Sidan snabbstart för funktioner i Azure Portal](./media/functions-create-github-webhook-triggered-function/add-first-function.png)

2. <span data-ttu-id="0584b-118">Välj den **GitHub-WebHook** mall för språket.</span><span class="sxs-lookup"><span data-stu-id="0584b-118">Select the **GitHub WebHook** template for your desired language.</span></span> <span data-ttu-id="0584b-119">**Namnge funktionen** och klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="0584b-119">**Name your function**, then select **Create**.</span></span>

     ![Skapa en funktion som utlöses med en GitHub-webhook i Azure Portal](./media/functions-create-github-webhook-triggered-function/functions-create-github-webhook-trigger.png) 

3. <span data-ttu-id="0584b-121">Klicka på **</> Hämta funktionswebbadress** och kopiera och spara värdena.</span><span class="sxs-lookup"><span data-stu-id="0584b-121">In your new function, click **</> Get function URL**, then copy and save the values.</span></span> <span data-ttu-id="0584b-122">Gör samma sak för **</> Hämta GitHub-hemlighet**.</span><span class="sxs-lookup"><span data-stu-id="0584b-122">Do the same thing for **</> Get GitHub secret**.</span></span> <span data-ttu-id="0584b-123">Du behöver dessa värden när du konfigurerar webhooken i GitHub.</span><span class="sxs-lookup"><span data-stu-id="0584b-123">You use these values to configure the webhook in GitHub.</span></span>

    ![Granska funktionskoden](./media/functions-create-github-webhook-triggered-function/functions-copy-function-url-github-secret.png)

<span data-ttu-id="0584b-125">Skapa sedan en webhook i GitHub-lagringsplatsen.</span><span class="sxs-lookup"><span data-stu-id="0584b-125">Next, you create a webhook in your GitHub repository.</span></span>

## <a name="configure-the-webhook"></a><span data-ttu-id="0584b-126">Konfigurera webhooken</span><span class="sxs-lookup"><span data-stu-id="0584b-126">Configure the webhook</span></span>

1. <span data-ttu-id="0584b-127">Navigera till en lagringsplats du äger i GitHub.</span><span class="sxs-lookup"><span data-stu-id="0584b-127">In GitHub, navigate to a repository that you own.</span></span> <span data-ttu-id="0584b-128">Du kan också använda en lagringsplats du har förgrenat.</span><span class="sxs-lookup"><span data-stu-id="0584b-128">You can also use any repository that you have forked.</span></span> <span data-ttu-id="0584b-129">Om du behöver förgrena en lagringsplats använder du <https://github.com/Azure-Samples/functions-quickstart>.</span><span class="sxs-lookup"><span data-stu-id="0584b-129">If you need to fork a repository, use <https://github.com/Azure-Samples/functions-quickstart>.</span></span>

1. <span data-ttu-id="0584b-130">Klicka på **Inställningar**, klicka på **Webhooks** och klicka sedan på **Lägg till webhook**.</span><span class="sxs-lookup"><span data-stu-id="0584b-130">Click **Settings**, then click **Webhooks**, and  **Add webhook**.</span></span>

    ![Lägga till en GitHub-webhook](./media/functions-create-github-webhook-triggered-function/functions-create-new-github-webhook-2.png)

1. <span data-ttu-id="0584b-132">Använd inställningarna i tabellen och klicka på **Lägg till webhook**.</span><span class="sxs-lookup"><span data-stu-id="0584b-132">Use settings as specified in the table, then click **Add webhook**.</span></span>

    ![Ställa in webhooksadressen och hemligheten](./media/functions-create-github-webhook-triggered-function/functions-create-new-github-webhook-3.png)

| <span data-ttu-id="0584b-134">Inställning</span><span class="sxs-lookup"><span data-stu-id="0584b-134">Setting</span></span> | <span data-ttu-id="0584b-135">Föreslaget värde</span><span class="sxs-lookup"><span data-stu-id="0584b-135">Suggested value</span></span> | <span data-ttu-id="0584b-136">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="0584b-136">Description</span></span> |
|---|---|---|
| <span data-ttu-id="0584b-137">**Payload URL** (Webbadress för nyttolast)</span><span class="sxs-lookup"><span data-stu-id="0584b-137">**Payload URL**</span></span> | <span data-ttu-id="0584b-138">Kopierat värde</span><span class="sxs-lookup"><span data-stu-id="0584b-138">Copied value</span></span> | <span data-ttu-id="0584b-139">Använd värdet som returnerades med **</> Hämta funktionswebbadress**.</span><span class="sxs-lookup"><span data-stu-id="0584b-139">Use the value returned by  **</> Get function URL**.</span></span> |
| <span data-ttu-id="0584b-140">**Hemlighet**</span><span class="sxs-lookup"><span data-stu-id="0584b-140">**Secret**</span></span>   | <span data-ttu-id="0584b-141">Kopierat värde</span><span class="sxs-lookup"><span data-stu-id="0584b-141">Copied value</span></span> | <span data-ttu-id="0584b-142">Använd värdet som returnerades med **</> Hämta GitHub-hemlighet**.</span><span class="sxs-lookup"><span data-stu-id="0584b-142">Use the value returned by  **</> Get GitHub secret**.</span></span> |
| <span data-ttu-id="0584b-143">**Innehållstyp**</span><span class="sxs-lookup"><span data-stu-id="0584b-143">**Content type**</span></span> | <span data-ttu-id="0584b-144">application/json</span><span class="sxs-lookup"><span data-stu-id="0584b-144">application/json</span></span> | <span data-ttu-id="0584b-145">Funktionen förväntar sig en JSON-nyttolast.</span><span class="sxs-lookup"><span data-stu-id="0584b-145">The function expects a JSON payload.</span></span> |
| <span data-ttu-id="0584b-146">Händelseutlösare</span><span class="sxs-lookup"><span data-stu-id="0584b-146">Event triggers</span></span> | <span data-ttu-id="0584b-147">Låt mig välja enskilda händelser</span><span class="sxs-lookup"><span data-stu-id="0584b-147">Let me select individual events</span></span> | <span data-ttu-id="0584b-148">Vi vill bara utlösa för händelser med ärendekommentarer.</span><span class="sxs-lookup"><span data-stu-id="0584b-148">We only want to trigger on issue comment events.</span></span>  |
| | <span data-ttu-id="0584b-149">Ärendekommentar</span><span class="sxs-lookup"><span data-stu-id="0584b-149">Issue comment</span></span> |  |

<span data-ttu-id="0584b-150">Nu har webhooken konfigurerats för att utlösa din funktion när en ny ärendekommentar läggs till.</span><span class="sxs-lookup"><span data-stu-id="0584b-150">Now, the webhook is configured to trigger your function when a new issue comment is added.</span></span>

## <a name="test-the-function"></a><span data-ttu-id="0584b-151">Testa funktionen</span><span class="sxs-lookup"><span data-stu-id="0584b-151">Test the function</span></span>

1. <span data-ttu-id="0584b-152">I din GitHub-lagringsplats öppnar du fliken **Ärenden** i ett nytt webbläsarfönster.</span><span class="sxs-lookup"><span data-stu-id="0584b-152">In your GitHub repository, open the **Issues** tab in a new browser window.</span></span>

1. <span data-ttu-id="0584b-153">I det nya fönstret klickar du på **Nytt ärende**, skriver en titel och klickar på **Submit new issue** (Skicka nytt ärende).</span><span class="sxs-lookup"><span data-stu-id="0584b-153">In the new window, click **New Issue**, type a title, and then click **Submit new issue**.</span></span>

1. <span data-ttu-id="0584b-154">Skriv en kommentar i ärendet och klicka på **Kommentar**.</span><span class="sxs-lookup"><span data-stu-id="0584b-154">In the issue, type a comment and click **Comment**.</span></span>

    ![Lägg till en GitHub-ärendekommentar.](./media/functions-create-github-webhook-triggered-function/functions-github-webhook-add-comment.png)

1. <span data-ttu-id="0584b-156">Gå tillbaka till portalen och öppna loggarna.</span><span class="sxs-lookup"><span data-stu-id="0584b-156">Go back to the portal and view the logs.</span></span> <span data-ttu-id="0584b-157">Du bör se en spårningspost med den nya kommentartexten.</span><span class="sxs-lookup"><span data-stu-id="0584b-157">You should see a trace entry with the new comment text.</span></span>

     ![Se kommentartexten i loggarna.](./media/functions-create-github-webhook-triggered-function/function-app-view-logs.png)

## <a name="clean-up-resources"></a><span data-ttu-id="0584b-159">Rensa resurser</span><span class="sxs-lookup"><span data-stu-id="0584b-159">Clean up resources</span></span>

[!INCLUDE [Next steps note](../../includes/functions-quickstart-cleanup.md)]

## <a name="next-steps"></a><span data-ttu-id="0584b-160">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="0584b-160">Next steps</span></span>

<span data-ttu-id="0584b-161">Du har skapat en funktion som körs när en begäran tas emot från en GitHub-webhook.</span><span class="sxs-lookup"><span data-stu-id="0584b-161">You have created a function that runs when a request is received from a GitHub webhook.</span></span>

[!INCLUDE [Next steps note](../../includes/functions-quickstart-next-steps.md)]

<span data-ttu-id="0584b-162">Mer information om webhook-utlösare finns i [Azure Functions HTTP och webhook-bindningar](functions-bindings-http-webhook.md).</span><span class="sxs-lookup"><span data-stu-id="0584b-162">For more information about webhook triggers, see [Azure Functions HTTP and webhook bindings](functions-bindings-http-webhook.md).</span></span>
