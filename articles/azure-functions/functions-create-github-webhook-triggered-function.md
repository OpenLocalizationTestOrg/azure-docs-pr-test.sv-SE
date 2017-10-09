---
title: "aaaCreate en funktion i Azure som utlöses av en GitHub-webhook | Microsoft Docs"
description: "Använd Azure Functions toocreate en serverlösa funktion som anropas av en GitHub-webhook."
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
ms.openlocfilehash: 8ffcde82c9310d749159ed53eab113658e38a030
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-function-triggered-by-a-github-webhook"></a><span data-ttu-id="0df7c-103">Skapa en funktion som utlöses av en GitHub-webhook</span><span class="sxs-lookup"><span data-stu-id="0df7c-103">Create a function triggered by a GitHub webhook</span></span>

<span data-ttu-id="0df7c-104">Lär dig hur toocreate en funktion som utlöses av en HTTP-begäran för webhook med en GitHub-specifika nyttolast.</span><span class="sxs-lookup"><span data-stu-id="0df7c-104">Learn how toocreate a function that is triggered by an HTTP webhook request with a GitHub-specific payload.</span></span>

![Github-Webhook aktiveras funktionen i hello Azure-portalen](./media/functions-create-github-webhook-triggered-function/function-app-in-portal-editor.png)

## <a name="prerequisites"></a><span data-ttu-id="0df7c-106">Krav</span><span class="sxs-lookup"><span data-stu-id="0df7c-106">Prerequisites</span></span>

+ <span data-ttu-id="0df7c-107">Ett GitHub-konto med minst ett projekt.</span><span class="sxs-lookup"><span data-stu-id="0df7c-107">A GitHub account with at least one project.</span></span>
+ <span data-ttu-id="0df7c-108">En Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="0df7c-108">An Azure subscription.</span></span> <span data-ttu-id="0df7c-109">Om du inte har ett konto kan du skapa ett [kostnadsfritt konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) innan du börjar.</span><span class="sxs-lookup"><span data-stu-id="0df7c-109">If you don't have one, create a [free account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) before you begin.</span></span>

[!INCLUDE [functions-portal-favorite-function-apps](../../includes/functions-portal-favorite-function-apps.md)]

## <a name="create-an-azure-function-app"></a><span data-ttu-id="0df7c-110">Skapa en Azure Functions-app</span><span class="sxs-lookup"><span data-stu-id="0df7c-110">Create an Azure Function app</span></span>

[!INCLUDE [Create function app Azure portal](../../includes/functions-create-function-app-portal.md)]

![Funktionsappen skapades.](./media/functions-create-first-azure-function/function-app-create-success.png)

<span data-ttu-id="0df7c-112">Därefter skapar du en funktion i hello ny funktionsapp.</span><span class="sxs-lookup"><span data-stu-id="0df7c-112">Next, you create a function in hello new function app.</span></span>

<a name="create-function"></a>

## <a name="create-a-github-webhook-triggered-function"></a><span data-ttu-id="0df7c-113">Skapa en webhook-utlöst GitHub-funktion</span><span class="sxs-lookup"><span data-stu-id="0df7c-113">Create a GitHub webhook triggered function</span></span>

1. <span data-ttu-id="0df7c-114">Expandera funktionen appen och klicka på hello  **+**  knappen för nästa**funktioner**.</span><span class="sxs-lookup"><span data-stu-id="0df7c-114">Expand your function app and click hello **+** button next too**Functions**.</span></span> <span data-ttu-id="0df7c-115">Om det är första hello-funktion i din funktionsapp **anpassad funktionen**.</span><span class="sxs-lookup"><span data-stu-id="0df7c-115">If this is hello first function in your function app, select **Custom function**.</span></span> <span data-ttu-id="0df7c-116">Detta visar hello fullständig uppsättning funktionen mallar.</span><span class="sxs-lookup"><span data-stu-id="0df7c-116">This displays hello complete set of function templates.</span></span>

    ![Funktioner quickstart sida i hello Azure-portalen](./media/functions-create-github-webhook-triggered-function/add-first-function.png)

2. <span data-ttu-id="0df7c-118">Välj hello **GitHub-WebHook** mall för språket.</span><span class="sxs-lookup"><span data-stu-id="0df7c-118">Select hello **GitHub WebHook** template for your desired language.</span></span> <span data-ttu-id="0df7c-119">**Namnge funktionen** och klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="0df7c-119">**Name your function**, then select **Create**.</span></span>

     ![Skapa en funktion för GitHub-webhook utlöses i hello Azure-portalen](./media/functions-create-github-webhook-triggered-function/functions-create-github-webhook-trigger.png) 

3. <span data-ttu-id="0df7c-121">Klicka på den nya funktionen **<> / Get funktions-URL**, kopiera och spara hello värden.</span><span class="sxs-lookup"><span data-stu-id="0df7c-121">In your new function, click **</> Get function URL**, then copy and save hello values.</span></span> <span data-ttu-id="0df7c-122">Hello samma sak för **<> / hämta GitHub hemlighet**.</span><span class="sxs-lookup"><span data-stu-id="0df7c-122">Do hello same thing for **</> Get GitHub secret**.</span></span> <span data-ttu-id="0df7c-123">Du kan använda dessa värden tooconfigure hello webhook i GitHub.</span><span class="sxs-lookup"><span data-stu-id="0df7c-123">You use these values tooconfigure hello webhook in GitHub.</span></span>

    ![Granska hello Funktionskoden](./media/functions-create-github-webhook-triggered-function/functions-copy-function-url-github-secret.png)

<span data-ttu-id="0df7c-125">Skapa sedan en webhook i GitHub-lagringsplatsen.</span><span class="sxs-lookup"><span data-stu-id="0df7c-125">Next, you create a webhook in your GitHub repository.</span></span>

## <a name="configure-hello-webhook"></a><span data-ttu-id="0df7c-126">Konfigurera hello-webhook</span><span class="sxs-lookup"><span data-stu-id="0df7c-126">Configure hello webhook</span></span>

1. <span data-ttu-id="0df7c-127">Navigera tooa databasen som du äger i GitHub.</span><span class="sxs-lookup"><span data-stu-id="0df7c-127">In GitHub, navigate tooa repository that you own.</span></span> <span data-ttu-id="0df7c-128">Du kan också använda en lagringsplats du har förgrenat.</span><span class="sxs-lookup"><span data-stu-id="0df7c-128">You can also use any repository that you have forked.</span></span> <span data-ttu-id="0df7c-129">Om du behöver toofork en databas kan använda <https://github.com/Azure-Samples/functions-quickstart>.</span><span class="sxs-lookup"><span data-stu-id="0df7c-129">If you need toofork a repository, use <https://github.com/Azure-Samples/functions-quickstart>.</span></span>

1. <span data-ttu-id="0df7c-130">Klicka på **Inställningar**, klicka på **Webhooks** och klicka sedan på **Lägg till webhook**.</span><span class="sxs-lookup"><span data-stu-id="0df7c-130">Click **Settings**, then click **Webhooks**, and  **Add webhook**.</span></span>

    ![Lägga till en GitHub-webhook](./media/functions-create-github-webhook-triggered-function/functions-create-new-github-webhook-2.png)

1. <span data-ttu-id="0df7c-132">Använda inställningar som anges i hello tabell och klicka sedan på **lägga till webhook**.</span><span class="sxs-lookup"><span data-stu-id="0df7c-132">Use settings as specified in hello table, then click **Add webhook**.</span></span>

    ![Ange hello Webhooksadressen och hemlighet](./media/functions-create-github-webhook-triggered-function/functions-create-new-github-webhook-3.png)

| <span data-ttu-id="0df7c-134">Inställning</span><span class="sxs-lookup"><span data-stu-id="0df7c-134">Setting</span></span> | <span data-ttu-id="0df7c-135">Föreslaget värde</span><span class="sxs-lookup"><span data-stu-id="0df7c-135">Suggested value</span></span> | <span data-ttu-id="0df7c-136">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="0df7c-136">Description</span></span> |
|---|---|---|
| <span data-ttu-id="0df7c-137">**Payload URL** (Webbadress för nyttolast)</span><span class="sxs-lookup"><span data-stu-id="0df7c-137">**Payload URL**</span></span> | <span data-ttu-id="0df7c-138">Kopierat värde</span><span class="sxs-lookup"><span data-stu-id="0df7c-138">Copied value</span></span> | <span data-ttu-id="0df7c-139">Använd hello-värdet som returneras av **<> / Get funktions-URL**.</span><span class="sxs-lookup"><span data-stu-id="0df7c-139">Use hello value returned by  **</> Get function URL**.</span></span> |
| <span data-ttu-id="0df7c-140">**Hemlighet**</span><span class="sxs-lookup"><span data-stu-id="0df7c-140">**Secret**</span></span>   | <span data-ttu-id="0df7c-141">Kopierat värde</span><span class="sxs-lookup"><span data-stu-id="0df7c-141">Copied value</span></span> | <span data-ttu-id="0df7c-142">Använd hello-värdet som returneras av **<> / hämta GitHub hemlighet**.</span><span class="sxs-lookup"><span data-stu-id="0df7c-142">Use hello value returned by  **</> Get GitHub secret**.</span></span> |
| <span data-ttu-id="0df7c-143">**Innehållstyp**</span><span class="sxs-lookup"><span data-stu-id="0df7c-143">**Content type**</span></span> | <span data-ttu-id="0df7c-144">application/json</span><span class="sxs-lookup"><span data-stu-id="0df7c-144">application/json</span></span> | <span data-ttu-id="0df7c-145">hello förväntar sig en JSON-nyttolast.</span><span class="sxs-lookup"><span data-stu-id="0df7c-145">hello function expects a JSON payload.</span></span> |
| <span data-ttu-id="0df7c-146">Händelseutlösare</span><span class="sxs-lookup"><span data-stu-id="0df7c-146">Event triggers</span></span> | <span data-ttu-id="0df7c-147">Låt mig välja enskilda händelser</span><span class="sxs-lookup"><span data-stu-id="0df7c-147">Let me select individual events</span></span> | <span data-ttu-id="0df7c-148">Vi vill bara tootrigger på problemet kommentar händelser.</span><span class="sxs-lookup"><span data-stu-id="0df7c-148">We only want tootrigger on issue comment events.</span></span>  |
| | <span data-ttu-id="0df7c-149">Ärendekommentar</span><span class="sxs-lookup"><span data-stu-id="0df7c-149">Issue comment</span></span> |  |

<span data-ttu-id="0df7c-150">Nu hello webhook är konfigurerade tootrigger din funktion när en ny problemet kommentar läggs.</span><span class="sxs-lookup"><span data-stu-id="0df7c-150">Now, hello webhook is configured tootrigger your function when a new issue comment is added.</span></span>

## <a name="test-hello-function"></a><span data-ttu-id="0df7c-151">Testa hello-funktionen</span><span class="sxs-lookup"><span data-stu-id="0df7c-151">Test hello function</span></span>

1. <span data-ttu-id="0df7c-152">Öppna i GitHub-lagringsplatsen hello **problem** fliken i ett nytt webbläsarfönster.</span><span class="sxs-lookup"><span data-stu-id="0df7c-152">In your GitHub repository, open hello **Issues** tab in a new browser window.</span></span>

1. <span data-ttu-id="0df7c-153">I hello nya fönstret, klickar du på **nytt ärende**, Skriv ett namn och klicka sedan på **skicka nya**.</span><span class="sxs-lookup"><span data-stu-id="0df7c-153">In hello new window, click **New Issue**, type a title, and then click **Submit new issue**.</span></span>

1. <span data-ttu-id="0df7c-154">Skriv en kommentar i hello problemet, och klicka på **kommentar**.</span><span class="sxs-lookup"><span data-stu-id="0df7c-154">In hello issue, type a comment and click **Comment**.</span></span>

    ![Lägg till en GitHub-ärendekommentar.](./media/functions-create-github-webhook-triggered-function/functions-github-webhook-add-comment.png)

1. <span data-ttu-id="0df7c-156">Gå tillbaka toohello portal och visa hello loggar.</span><span class="sxs-lookup"><span data-stu-id="0df7c-156">Go back toohello portal and view hello logs.</span></span> <span data-ttu-id="0df7c-157">Du bör se en spårningspost med hello ny Kommentartext.</span><span class="sxs-lookup"><span data-stu-id="0df7c-157">You should see a trace entry with hello new comment text.</span></span>

     ![Visa hello Kommentartext i hello loggar.](./media/functions-create-github-webhook-triggered-function/function-app-view-logs.png)

## <a name="clean-up-resources"></a><span data-ttu-id="0df7c-159">Rensa resurser</span><span class="sxs-lookup"><span data-stu-id="0df7c-159">Clean up resources</span></span>

[!INCLUDE [Next steps note](../../includes/functions-quickstart-cleanup.md)]

## <a name="next-steps"></a><span data-ttu-id="0df7c-160">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="0df7c-160">Next steps</span></span>

<span data-ttu-id="0df7c-161">Du har skapat en funktion som körs när en begäran tas emot från en GitHub-webhook.</span><span class="sxs-lookup"><span data-stu-id="0df7c-161">You have created a function that runs when a request is received from a GitHub webhook.</span></span>

[!INCLUDE [Next steps note](../../includes/functions-quickstart-next-steps.md)]

<span data-ttu-id="0df7c-162">Mer information om webhook-utlösare finns i [Azure Functions HTTP och webhook-bindningar](functions-bindings-http-webhook.md).</span><span class="sxs-lookup"><span data-stu-id="0df7c-162">For more information about webhook triggers, see [Azure Functions HTTP and webhook bindings](functions-bindings-http-webhook.md).</span></span>
