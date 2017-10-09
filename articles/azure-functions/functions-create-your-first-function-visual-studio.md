---
title: "aaaCreate din första funktion i Azure med hjälp av Visual Studio | Microsoft Docs"
description: "Skapa och publicera en enkel HTTP aktiveras funktionen tooAzure med hjälp av Azure Functions verktyg för Visual Studio."
services: functions
documentationcenter: na
author: rachelappel
manager: erikre
editor: 
tags: 
keywords: azure functions, functions, event processing, compute, serverless architecture
ms.assetid: 82db1177-2295-4e39-bd42-763f6082e796
ms.service: functions
ms.devlang: multiple
ms.topic: quickstart
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 07/05/2017
ms.author: glenga
ms.custom: mvc
ms.openlocfilehash: 851e5b98dcc2da00334620896a0ea31f566589f7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-your-first-function-using-visual-studio"></a><span data-ttu-id="f0459-104">Skapa din första funktion med Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f0459-104">Create your first function using Visual Studio</span></span>

<span data-ttu-id="f0459-105">Azure Functions kan du köra din kod i en miljö med serverlösa utan toofirst skapa en virtuell dator eller publicera ett webbprogram.</span><span class="sxs-lookup"><span data-stu-id="f0459-105">Azure Functions lets you execute your code in a serverless environment without having toofirst create a VM or publish a web application.</span></span>

<span data-ttu-id="f0459-106">I det här avsnittet dig du hur toouse hello 2017 för Visual Studio tools för Azure Functions toocreate och testa en ”hello world”-funktionen lokalt.</span><span class="sxs-lookup"><span data-stu-id="f0459-106">In this topic, you learn how toouse hello Visual Studio 2017 tools for Azure Functions toocreate and test a "hello world" function locally.</span></span> <span data-ttu-id="f0459-107">Du sedan publicerar hello funktionen koden tooAzure.</span><span class="sxs-lookup"><span data-stu-id="f0459-107">You will then publish hello function code tooAzure.</span></span> <span data-ttu-id="f0459-108">Dessa verktyg är tillgängliga som en del av hello Azure-utveckling arbetsbelastningen i Visual Studio 2017 version 15,3 eller en senare version.</span><span class="sxs-lookup"><span data-stu-id="f0459-108">These tools are available as part of hello Azure development workload in Visual Studio 2017 version 15.3, or a later version.</span></span>

![Azure Functions-kod i ett Visual Studio-projekt](./media/functions-create-your-first-function-visual-studio/functions-vstools-intro.png)

## <a name="prerequisites"></a><span data-ttu-id="f0459-110">Krav</span><span class="sxs-lookup"><span data-stu-id="f0459-110">Prerequisites</span></span>

<span data-ttu-id="f0459-111">toocomplete självstudier, installationen:</span><span class="sxs-lookup"><span data-stu-id="f0459-111">toocomplete this tutorial, install:</span></span>

* <span data-ttu-id="f0459-112">[Visual Studio 2017 version 15,3](https://www.visualstudio.com/vs/preview/), inklusive hello **Azure-utveckling** arbetsbelastning.</span><span class="sxs-lookup"><span data-stu-id="f0459-112">[Visual Studio 2017 version 15.3](https://www.visualstudio.com/vs/preview/), including hello **Azure development** workload.</span></span>

    ![Installera Visual Studio 2017 med hello Azure-utveckling arbetsbelastning](./media/functions-create-your-first-function-visual-studio/functions-vs-workloads.png)
    
    >[!NOTE]  
    <span data-ttu-id="f0459-114">När du installerar eller uppgraderar tooVisual Studio 2017 version 15,3 kanske du också toomanually uppdatering hello 2017 för Visual Studio tools för Azure Functions.</span><span class="sxs-lookup"><span data-stu-id="f0459-114">After you install or upgrade tooVisual Studio 2017 version 15.3, you might also need toomanually update hello Visual Studio 2017 tools for Azure Functions.</span></span> <span data-ttu-id="f0459-115">Du kan uppdatera hello verktyg från hello **verktyg** menyn under **tillägg och uppdateringar...**   >  **Uppdateringar** > **Visual Studio Marketplace** > **Azure Functions och Web jobb verktyg**  >  **Uppdatering**.</span><span class="sxs-lookup"><span data-stu-id="f0459-115">You can update hello tools from hello **Tools** menu under **Extensions and Updates...** > **Updates** > **Visual Studio Marketplace** > **Azure Functions and Web Jobs Tools** > **Update**.</span></span> 

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)] 

## <a name="create-an-azure-functions-project-in-visual-studio"></a><span data-ttu-id="f0459-116">Skapa ett Azure Functions-projekt i Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f0459-116">Create an Azure Functions project in Visual Studio</span></span>

[!INCLUDE [Create a project using hello Azure Functions template](../../includes/functions-vstools-create.md)]

<span data-ttu-id="f0459-117">Nu när du har skapat hello-projekt kan skapa du din första funktion.</span><span class="sxs-lookup"><span data-stu-id="f0459-117">Now that you have created hello project, you can create your first function.</span></span>

## <a name="create-hello-function"></a><span data-ttu-id="f0459-118">Skapa hello-funktion</span><span class="sxs-lookup"><span data-stu-id="f0459-118">Create hello function</span></span>

1. <span data-ttu-id="f0459-119">I **Solution Explorer** högerklickar du på projektnoden och väljer **Lägg till** > **Nytt objekt**.</span><span class="sxs-lookup"><span data-stu-id="f0459-119">In **Solution Explorer**, right-click on your project node and select **Add** > **New Item**.</span></span> <span data-ttu-id="f0459-120">Välj **Azure Function** och klicka på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="f0459-120">Select **Azure Function** and click **Add**.</span></span>

2. <span data-ttu-id="f0459-121">Välj **HttpTrigger**, ange ett **Funktionsnamn**, välj **Anonym** för **Åtkomsträttigheter** och klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="f0459-121">Select **HttpTrigger**, type a **Function Name**, select **Anonymous** for **Access Rights**, and click **Create**.</span></span> <span data-ttu-id="f0459-122">hello-funktion skapas används av en HTTP-begäran från klienter.</span><span class="sxs-lookup"><span data-stu-id="f0459-122">hello function created is accessed by an HTTP request from any client.</span></span> 

    ![Skapa en ny Azure Function](./media/functions-create-your-first-function-visual-studio/functions-vstools-add-new-function-2.png)

    <span data-ttu-id="f0459-124">En kodfil läggs tooyour projekt som innehåller en klass som implementerar din funktionskoden.</span><span class="sxs-lookup"><span data-stu-id="f0459-124">A code file is added tooyour project that contains a class that implements your function code.</span></span> <span data-ttu-id="f0459-125">Den här koden är baserad på en mall, som tar emot ett namnvärde och ta bort eko tillbaka.</span><span class="sxs-lookup"><span data-stu-id="f0459-125">This code is based on a template, which receives a name value and echos it back.</span></span> <span data-ttu-id="f0459-126">Hej **FunctionName** attribut anger hello namnet på funktionen.</span><span class="sxs-lookup"><span data-stu-id="f0459-126">hello **FunctionName** attribute sets hello name of your function.</span></span> <span data-ttu-id="f0459-127">Hej **HttpTrigger** attributet anger hello-meddelande som utlöser hello-funktionen.</span><span class="sxs-lookup"><span data-stu-id="f0459-127">hello **HttpTrigger** attribute indicates hello message that triggers hello function.</span></span> 

    ![Funktionen kodfilen](./media/functions-create-your-first-function-visual-studio/functions-code-page.png)

<span data-ttu-id="f0459-129">Nu när du har skapat en HTTP-utlöst funktion kan du testa den på en lokal dator.</span><span class="sxs-lookup"><span data-stu-id="f0459-129">Now that you have created an HTTP-triggered function, you can test it on your local computer.</span></span>

## <a name="test-hello-function-locally"></a><span data-ttu-id="f0459-130">Testa hello funktionen lokalt</span><span class="sxs-lookup"><span data-stu-id="f0459-130">Test hello function locally</span></span>

<span data-ttu-id="f0459-131">Med Azure Functions Core Tools kan du köra Azure Functions-projekt på din lokala utvecklingsdator.</span><span class="sxs-lookup"><span data-stu-id="f0459-131">Azure Functions Core Tools lets you run Azure Functions project on your local development computer.</span></span> <span data-ttu-id="f0459-132">Du kan ange tooinstall verktygen hello första gången du startar en funktion från Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="f0459-132">You are prompted tooinstall these tools hello first time you start a function from Visual Studio.</span></span>  

1. <span data-ttu-id="f0459-133">tootest din funktion, tryck på F5.</span><span class="sxs-lookup"><span data-stu-id="f0459-133">tootest your function, press F5.</span></span> <span data-ttu-id="f0459-134">Om du uppmanas acceptera hello begäran från Visual Studio toodownload och installera verktyg för Azure Functions kärnor (CLI).</span><span class="sxs-lookup"><span data-stu-id="f0459-134">If prompted, accept hello request from Visual Studio toodownload and install Azure Functions Core (CLI) tools.</span></span>  <span data-ttu-id="f0459-135">Du kanske också måste tooenable ett undantag i brandväggen så att hello verktyg kan hantera HTTP-begäranden.</span><span class="sxs-lookup"><span data-stu-id="f0459-135">You may also need tooenable a firewall exception so that hello tools can handle HTTP requests.</span></span>

2. <span data-ttu-id="f0459-136">Kopiera hello URL för din funktion från hello Azure Functions-runtime utdata.</span><span class="sxs-lookup"><span data-stu-id="f0459-136">Copy hello URL of your function from hello Azure Functions runtime output.</span></span>  

    ![Lokal Azure-körningsmiljö](./media/functions-create-your-first-function-visual-studio/functions-vstools-f5.png)

3. <span data-ttu-id="f0459-138">Klistra in hello URL för hello HTTP-begäran i webbläsarens adressfält.</span><span class="sxs-lookup"><span data-stu-id="f0459-138">Paste hello URL for hello HTTP request into your browser's address bar.</span></span> <span data-ttu-id="f0459-139">Lägg till hello frågesträngen `&name=<yourname>` toothis URL och utföra hello-begäran.</span><span class="sxs-lookup"><span data-stu-id="f0459-139">Append hello query string `&name=<yourname>` toothis URL and execute hello request.</span></span> <span data-ttu-id="f0459-140">hello följande visar hello svar i hello webbläsare toohello lokala GET-begäran returneras av hello:</span><span class="sxs-lookup"><span data-stu-id="f0459-140">hello following shows hello response in hello browser toohello local GET request returned by hello function:</span></span> 

    ![Funktionen localhost svar i hello webbläsare](./media/functions-create-your-first-function-visual-studio/functions-test-local-browser.png)

4. <span data-ttu-id="f0459-142">toostop felsökning, klicka på hello **stoppa** i verktygsfältet hello Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="f0459-142">toostop debugging, click hello **Stop** button on hello Visual Studio toolbar.</span></span>

<span data-ttu-id="f0459-143">När du har kontrollerat att hello-funktionen fungerar korrekt på den lokala datorn, är den tid toopublish hello projekt tooAzure.</span><span class="sxs-lookup"><span data-stu-id="f0459-143">After you have verified that hello function runs correctly on your local computer, it's time toopublish hello project tooAzure.</span></span>

## <a name="publish-hello-project-tooazure"></a><span data-ttu-id="f0459-144">Publicera hello projekt tooAzure</span><span class="sxs-lookup"><span data-stu-id="f0459-144">Publish hello project tooAzure</span></span>

<span data-ttu-id="f0459-145">Du måste ha en funktionsapp i din Azure-prenumeration innan du kan publicera projektet.</span><span class="sxs-lookup"><span data-stu-id="f0459-145">You must have a function app in your Azure subscription before you can publish your project.</span></span> <span data-ttu-id="f0459-146">Du kan skapa en funktionsapp direkt från Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="f0459-146">You can create a function app right from Visual Studio.</span></span>

[!INCLUDE [Publish hello project tooAzure](../../includes/functions-vstools-publish.md)]

## <a name="test-your-function-in-azure"></a><span data-ttu-id="f0459-147">Testa din funktion i Azure</span><span class="sxs-lookup"><span data-stu-id="f0459-147">Test your function in Azure</span></span>

1. <span data-ttu-id="f0459-148">Kopiera hello bas-URL för hello funktionsapp från hello publicera profilsida.</span><span class="sxs-lookup"><span data-stu-id="f0459-148">Copy hello base URL of hello function app from hello Publish profile page.</span></span> <span data-ttu-id="f0459-149">Ersätt hello `localhost:port` del av hello-URL som du använde när du testar hello funktionen lokalt med hello ny bas-URL.</span><span class="sxs-lookup"><span data-stu-id="f0459-149">Replace hello `localhost:port` portion of hello URL you used when testing hello function locally with hello new base URL.</span></span> <span data-ttu-id="f0459-150">Som tidigare gör att tooappend hello frågesträngen `&name=<yourname>` toothis URL och utföra hello-begäran.</span><span class="sxs-lookup"><span data-stu-id="f0459-150">As before, make sure tooappend hello query string `&name=<yourname>` toothis URL and execute hello request.</span></span>

    <span data-ttu-id="f0459-151">hello-URL som anropar HTTP aktiveras funktionen ser ut så här:</span><span class="sxs-lookup"><span data-stu-id="f0459-151">hello URL that calls your HTTP triggered function looks like this:</span></span>

        http://<functionappname>.azurewebsites.net/api/<functionname>?name=<yourname> 

2. <span data-ttu-id="f0459-152">Klistra in den här nya URL: en för hello HTTP-begäran i webbläsarens adressfält.</span><span class="sxs-lookup"><span data-stu-id="f0459-152">Paste this new URL for hello HTTP request into your browser's address bar.</span></span> <span data-ttu-id="f0459-153">hello följande visar hello svar i hello webbläsare toohello GET fjärrbegäran returneras av hello:</span><span class="sxs-lookup"><span data-stu-id="f0459-153">hello following shows hello response in hello browser toohello remote GET request returned by hello function:</span></span> 

    ![Funktionen svar i hello webbläsare](./media/functions-create-your-first-function-visual-studio/functions-test-remote-browser.png)
 
## <a name="next-steps"></a><span data-ttu-id="f0459-155">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="f0459-155">Next steps</span></span>

<span data-ttu-id="f0459-156">Du har använt funktionsapp för Visual Studio toocreate C# med en enkel HTTP utlöses-funktion.</span><span class="sxs-lookup"><span data-stu-id="f0459-156">You have used Visual Studio toocreate a C# function app with a simple HTTP triggered function.</span></span> 

+ <span data-ttu-id="f0459-157">toolearn hur tooconfigure projekt-toosupport andra typer av utlösare och bindningar finns hello [konfigurera hello projekt för lokal utveckling](functions-develop-vs.md#configure-the-project-for-local-development) i avsnittet [Azure Functions Tools för Visual Studio](functions-develop-vs.md).</span><span class="sxs-lookup"><span data-stu-id="f0459-157">toolearn how tooconfigure your project toosupport other types of triggers and bindings, see hello [Configure hello project for local development](functions-develop-vs.md#configure-the-project-for-local-development) section in [Azure Functions Tools for Visual Studio](functions-develop-vs.md).</span></span>
+ <span data-ttu-id="f0459-158">toolearn mer om lokala testning och felsökning med hjälp av hello Azure Functions Core verktyg finns [kod och testa Azure Functions lokalt](functions-run-local.md).</span><span class="sxs-lookup"><span data-stu-id="f0459-158">toolearn more about local testing and debugging using hello Azure Functions Core Tools, see [Code and test Azure Functions locally](functions-run-local.md).</span></span> 
+ <span data-ttu-id="f0459-159">toolearn mer information om hur du utvecklar fungerar som .NET-klassbibliotek finns [med hjälp av .NET-klassbibliotek med Azure Functions](functions-dotnet-class-library.md).</span><span class="sxs-lookup"><span data-stu-id="f0459-159">toolearn more about developing functions as .NET class libraries, see [Using .NET class libraries with Azure Functions](functions-dotnet-class-library.md).</span></span> 

