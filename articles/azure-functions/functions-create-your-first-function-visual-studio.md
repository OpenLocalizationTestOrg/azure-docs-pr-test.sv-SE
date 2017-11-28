---
title: "Skapa din första funktion i Azure med Visual Studio | Microsoft Docs"
description: "Skapa och publicera en enkel HTTP-utlöst funktion till Azure med Azure Functions Tools för Visual Studio."
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
ms.openlocfilehash: 04370558725d76ffe83d8aaf5d16c88fd2803ba9
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="create-your-first-function-using-visual-studio"></a><span data-ttu-id="16113-104">Skapa din första funktion med Visual Studio</span><span class="sxs-lookup"><span data-stu-id="16113-104">Create your first function using Visual Studio</span></span>

<span data-ttu-id="16113-105">Med Azure Functions kan du köra kod i en serverfri miljö utan att först behöva skapa en virtuell dator eller publicera en webbapp.</span><span class="sxs-lookup"><span data-stu-id="16113-105">Azure Functions lets you execute your code in a serverless environment without having to first create a VM or publish a web application.</span></span>

<span data-ttu-id="16113-106">Lär dig hur du använder 2017 för Visual Studio tools för Azure Functions för att skapa och testa en ”hello world”-funktionen lokalt i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="16113-106">In this topic, you learn how to use the Visual Studio 2017 tools for Azure Functions to create and test a "hello world" function locally.</span></span> <span data-ttu-id="16113-107">Du publicerar sedan funktionskoden till Azure.</span><span class="sxs-lookup"><span data-stu-id="16113-107">You will then publish the function code to Azure.</span></span> <span data-ttu-id="16113-108">De här verktygen är tillgängliga som en del av arbetsbelastningen Azure Development i Visual Studio 2017 version 15.3, eller en senare version.</span><span class="sxs-lookup"><span data-stu-id="16113-108">These tools are available as part of the Azure development workload in Visual Studio 2017 version 15.3, or a later version.</span></span>

![Azure Functions-kod i ett Visual Studio-projekt](./media/functions-create-your-first-function-visual-studio/functions-vstools-intro.png)

## <a name="prerequisites"></a><span data-ttu-id="16113-110">Krav</span><span class="sxs-lookup"><span data-stu-id="16113-110">Prerequisites</span></span>

<span data-ttu-id="16113-111">För att slutföra den här självstudien installerar du:</span><span class="sxs-lookup"><span data-stu-id="16113-111">To complete this tutorial, install:</span></span>

* <span data-ttu-id="16113-112">[Visual Studio 2017 version 15.3](https://www.visualstudio.com/vs/preview/), inklusive arbetsbelastningen **Azure Development**.</span><span class="sxs-lookup"><span data-stu-id="16113-112">[Visual Studio 2017 version 15.3](https://www.visualstudio.com/vs/preview/), including the **Azure development** workload.</span></span>

    ![Installera Visual Studio 2017 med arbetsbelastningen Azure Development](./media/functions-create-your-first-function-visual-studio/functions-vs-workloads.png)
    
    >[!NOTE]  
    <span data-ttu-id="16113-114">När du installerar eller uppgraderar till Visual Studio 2017 version 15,3 kan också behöva uppdatera 2017 för Visual Studio-verktygen för Azure Functions manuellt.</span><span class="sxs-lookup"><span data-stu-id="16113-114">After you install or upgrade to Visual Studio 2017 version 15.3, you might also need to manually update the Visual Studio 2017 tools for Azure Functions.</span></span> <span data-ttu-id="16113-115">Du kan uppdatera verktyg från den **verktyg** menyn under **tillägg och uppdateringar...**   >  **Uppdateringar** > **Visual Studio Marketplace** > **Azure Functions och Web jobb verktyg**  >  **Uppdatering**.</span><span class="sxs-lookup"><span data-stu-id="16113-115">You can update the tools from the **Tools** menu under **Extensions and Updates...** > **Updates** > **Visual Studio Marketplace** > **Azure Functions and Web Jobs Tools** > **Update**.</span></span> 

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)] 

## <a name="create-an-azure-functions-project-in-visual-studio"></a><span data-ttu-id="16113-116">Skapa ett Azure Functions-projekt i Visual Studio</span><span class="sxs-lookup"><span data-stu-id="16113-116">Create an Azure Functions project in Visual Studio</span></span>

[!INCLUDE [Create a project using the Azure Functions template](../../includes/functions-vstools-create.md)]

<span data-ttu-id="16113-117">Nu när du har skapat projektet kan du skapa din första funktion.</span><span class="sxs-lookup"><span data-stu-id="16113-117">Now that you have created the project, you can create your first function.</span></span>

## <a name="create-the-function"></a><span data-ttu-id="16113-118">Skapa funktionen</span><span class="sxs-lookup"><span data-stu-id="16113-118">Create the function</span></span>

1. <span data-ttu-id="16113-119">I **Solution Explorer** högerklickar du på projektnoden och väljer **Lägg till** > **Nytt objekt**.</span><span class="sxs-lookup"><span data-stu-id="16113-119">In **Solution Explorer**, right-click on your project node and select **Add** > **New Item**.</span></span> <span data-ttu-id="16113-120">Välj **Azure Function** och klicka på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="16113-120">Select **Azure Function** and click **Add**.</span></span>

2. <span data-ttu-id="16113-121">Välj **HttpTrigger**, ange ett **Funktionsnamn**, välj **Anonym** för **Åtkomsträttigheter** och klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="16113-121">Select **HttpTrigger**, type a **Function Name**, select **Anonymous** for **Access Rights**, and click **Create**.</span></span> <span data-ttu-id="16113-122">Funktionen som skapas kan nås av en HTTP-begäran från alla klienter.</span><span class="sxs-lookup"><span data-stu-id="16113-122">The function created is accessed by an HTTP request from any client.</span></span> 

    ![Skapa en ny Azure Function](./media/functions-create-your-first-function-visual-studio/functions-vstools-add-new-function-2.png)

    <span data-ttu-id="16113-124">En kodfil läggs till ditt projekt som innehåller en klass som implementerar din funktionskoden.</span><span class="sxs-lookup"><span data-stu-id="16113-124">A code file is added to your project that contains a class that implements your function code.</span></span> <span data-ttu-id="16113-125">Den här koden är baserad på en mall, som tar emot ett namnvärde och ta bort eko tillbaka.</span><span class="sxs-lookup"><span data-stu-id="16113-125">This code is based on a template, which receives a name value and echos it back.</span></span> <span data-ttu-id="16113-126">Den **FunctionName** attributet anger namnet på funktionen.</span><span class="sxs-lookup"><span data-stu-id="16113-126">The **FunctionName** attribute sets the name of your function.</span></span> <span data-ttu-id="16113-127">Den **HttpTrigger** attributet anger meddelandet som utlöser funktionen.</span><span class="sxs-lookup"><span data-stu-id="16113-127">The **HttpTrigger** attribute indicates the message that triggers the function.</span></span> 

    ![Funktionen kodfilen](./media/functions-create-your-first-function-visual-studio/functions-code-page.png)

<span data-ttu-id="16113-129">Nu när du har skapat en HTTP-utlöst funktion kan du testa den på en lokal dator.</span><span class="sxs-lookup"><span data-stu-id="16113-129">Now that you have created an HTTP-triggered function, you can test it on your local computer.</span></span>

## <a name="test-the-function-locally"></a><span data-ttu-id="16113-130">Testa funktionen lokalt</span><span class="sxs-lookup"><span data-stu-id="16113-130">Test the function locally</span></span>

<span data-ttu-id="16113-131">Med Azure Functions Core Tools kan du köra Azure Functions-projekt på din lokala utvecklingsdator.</span><span class="sxs-lookup"><span data-stu-id="16113-131">Azure Functions Core Tools lets you run Azure Functions project on your local development computer.</span></span> <span data-ttu-id="16113-132">Du uppmanas att installera de här verktygen första gången du startar en funktion från Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="16113-132">You are prompted to install these tools the first time you start a function from Visual Studio.</span></span>  

1. <span data-ttu-id="16113-133">Tryck på F5 för att testa funktionen.</span><span class="sxs-lookup"><span data-stu-id="16113-133">To test your function, press F5.</span></span> <span data-ttu-id="16113-134">Acceptera begäran från Visual Studio för att ladda ned och installera Azure Functions Core (CLI)-verktyg.</span><span class="sxs-lookup"><span data-stu-id="16113-134">If prompted, accept the request from Visual Studio to download and install Azure Functions Core (CLI) tools.</span></span>  <span data-ttu-id="16113-135">Du kan även behöva skapa ett brandväggsundantag så att verktygen kan hantera HTTP-förfrågningar.</span><span class="sxs-lookup"><span data-stu-id="16113-135">You may also need to enable a firewall exception so that the tools can handle HTTP requests.</span></span>

2. <span data-ttu-id="16113-136">Kopiera URL:en för funktionen från dina Azure Functions-utdata.</span><span class="sxs-lookup"><span data-stu-id="16113-136">Copy the URL of your function from the Azure Functions runtime output.</span></span>  

    ![Lokal Azure-körningsmiljö](./media/functions-create-your-first-function-visual-studio/functions-vstools-f5.png)

3. <span data-ttu-id="16113-138">Klistra in webbadressen för HTTP-begäran i webbläsarens adressfält.</span><span class="sxs-lookup"><span data-stu-id="16113-138">Paste the URL for the HTTP request into your browser's address bar.</span></span> <span data-ttu-id="16113-139">Lägg till frågesträngen `&name=<yourname>` i webbadressen och kör din begäran.</span><span class="sxs-lookup"><span data-stu-id="16113-139">Append the query string `&name=<yourname>` to this URL and execute the request.</span></span> <span data-ttu-id="16113-140">Nedan visas svaret på den lokala GET-begäran som returnerades av funktionen i webbläsaren:</span><span class="sxs-lookup"><span data-stu-id="16113-140">The following shows the response in the browser to the local GET request returned by the function:</span></span> 

    ![Svar för funktion-localhost i webbläsaren](./media/functions-create-your-first-function-visual-studio/functions-test-local-browser.png)

4. <span data-ttu-id="16113-142">För att stoppa felsökningen klickar du på knappen **Stopp** i Visual Studio-verktygsfältet.</span><span class="sxs-lookup"><span data-stu-id="16113-142">To stop debugging, click the **Stop** button on the Visual Studio toolbar.</span></span>

<span data-ttu-id="16113-143">När du har kontrollerat att funktionen körs korrekt på den lokala datorn är det dags att publicera projektet på Azure.</span><span class="sxs-lookup"><span data-stu-id="16113-143">After you have verified that the function runs correctly on your local computer, it's time to publish the project to Azure.</span></span>

## <a name="publish-the-project-to-azure"></a><span data-ttu-id="16113-144">Publicera projektet på Azure</span><span class="sxs-lookup"><span data-stu-id="16113-144">Publish the project to Azure</span></span>

<span data-ttu-id="16113-145">Du måste ha en funktionsapp i din Azure-prenumeration innan du kan publicera projektet.</span><span class="sxs-lookup"><span data-stu-id="16113-145">You must have a function app in your Azure subscription before you can publish your project.</span></span> <span data-ttu-id="16113-146">Du kan skapa en funktionsapp direkt från Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="16113-146">You can create a function app right from Visual Studio.</span></span>

[!INCLUDE [Publish the project to Azure](../../includes/functions-vstools-publish.md)]

## <a name="test-your-function-in-azure"></a><span data-ttu-id="16113-147">Testa din funktion i Azure</span><span class="sxs-lookup"><span data-stu-id="16113-147">Test your function in Azure</span></span>

1. <span data-ttu-id="16113-148">Kopiera den grundläggande URL:en för funktionsappen från sidan Publicera profil.</span><span class="sxs-lookup"><span data-stu-id="16113-148">Copy the base URL of the function app from the Publish profile page.</span></span> <span data-ttu-id="16113-149">Ersätt `localhost:port`-delen av URL:en som du använde när du testade funktionen lokalt med den nya bas-URL:en.</span><span class="sxs-lookup"><span data-stu-id="16113-149">Replace the `localhost:port` portion of the URL you used when testing the function locally with the new base URL.</span></span> <span data-ttu-id="16113-150">Lägg till frågesträngen `&name=<yourname>` i URL:en som tidigare och kör din begäran.</span><span class="sxs-lookup"><span data-stu-id="16113-150">As before, make sure to append the query string `&name=<yourname>` to this URL and execute the request.</span></span>

    <span data-ttu-id="16113-151">Den URL som anropar den HTTP-utlösta funktionen ser ut så här:</span><span class="sxs-lookup"><span data-stu-id="16113-151">The URL that calls your HTTP triggered function looks like this:</span></span>

        http://<functionappname>.azurewebsites.net/api/<functionname>?name=<yourname> 

2. <span data-ttu-id="16113-152">Klistra in den nya URL:en för HTTP-begäran i webbläsarens adressfält.</span><span class="sxs-lookup"><span data-stu-id="16113-152">Paste this new URL for the HTTP request into your browser's address bar.</span></span> <span data-ttu-id="16113-153">Nedan visas svaret på fjärr-GET-begäran som returnerades av funktionen i webbläsaren:</span><span class="sxs-lookup"><span data-stu-id="16113-153">The following shows the response in the browser to the remote GET request returned by the function:</span></span> 

    ![Funktionssvar i webbläsaren](./media/functions-create-your-first-function-visual-studio/functions-test-remote-browser.png)
 
## <a name="next-steps"></a><span data-ttu-id="16113-155">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="16113-155">Next steps</span></span>

<span data-ttu-id="16113-156">Du har nu använt Visual Studio för att skapa en C#-funktionsapp med en enkel HTTP-utlöst funktion.</span><span class="sxs-lookup"><span data-stu-id="16113-156">You have used Visual Studio to create a C# function app with a simple HTTP triggered function.</span></span> 

+ <span data-ttu-id="16113-157">Information om hur du konfigurerar ditt projekt för att ge stöd för andra typer av utlösare och bindningar finns i [Configure the project for local development](functions-develop-vs.md#configure-the-project-for-local-development) (Konfigurera projektet för lokal utveckling) i avsnittet [Azure Functions Tools for Visual Studio](functions-develop-vs.md) (Azure Functions Tools för Visual Studio).</span><span class="sxs-lookup"><span data-stu-id="16113-157">To learn how to configure your project to support other types of triggers and bindings, see the [Configure the project for local development](functions-develop-vs.md#configure-the-project-for-local-development) section in [Azure Functions Tools for Visual Studio](functions-develop-vs.md).</span></span>
+ <span data-ttu-id="16113-158">Läs mer om lokal testning och felsökning med hjälp av Azure Functions Core Tools i [Code and test Azure Functions locally](functions-run-local.md) (Koda och testa Azure Functions lokalt).</span><span class="sxs-lookup"><span data-stu-id="16113-158">To learn more about local testing and debugging using the Azure Functions Core Tools, see [Code and test Azure Functions locally](functions-run-local.md).</span></span> 
+ <span data-ttu-id="16113-159">Mer information om hur du utvecklar fungerar som .NET-klassbibliotek finns i [Using .NET class libraries with Azure Functions](functions-dotnet-class-library.md) (Använda .NET-klassbibliotek med Azure Functions).</span><span class="sxs-lookup"><span data-stu-id="16113-159">To learn more about developing functions as .NET class libraries, see [Using .NET class libraries with Azure Functions](functions-dotnet-class-library.md).</span></span> 

