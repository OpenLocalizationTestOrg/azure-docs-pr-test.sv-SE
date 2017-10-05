---
title: "Skapa din första funktion i Azure Portal | Microsoft Docs"
description: "Lär dig hur du skapar din första Azure-funktion för serverfri körning i Azure Portal."
services: functions
documentationcenter: na
author: ggailey777
manager: erikre
editor: 
tags: 
ms.assetid: 96cf87b9-8db6-41a8-863a-abb828e3d06d
ms.service: functions
ms.devlang: multiple
ms.topic: quickstart
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 08/07/2017
ms.author: glenga
ms.custom: mvc
ms.openlocfilehash: 3ec1f278f21d89782137625aff200f07f15fd9fb
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="create-your-first-function-in-the-azure-portal"></a><span data-ttu-id="dcad3-103">Skapa din första funktion i Azure Portal</span><span class="sxs-lookup"><span data-stu-id="dcad3-103">Create your first function in the Azure portal</span></span>

<span data-ttu-id="dcad3-104">Med Azure Functions kan du köra kod i en serverfri miljö utan att först behöva skapa en virtuell dator eller publicera en webbapp.</span><span class="sxs-lookup"><span data-stu-id="dcad3-104">Azure Functions lets you execute your code in a serverless environment without having to first create a VM or publish a web application.</span></span> <span data-ttu-id="dcad3-105">I det här ämnet får du lära dig att använda Functions till att skapa en ”hello world”-funktion i Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="dcad3-105">In this topic, learn how to use Functions to create a "hello world" function in the Azure portal.</span></span>

![Skapa en funktionsapp i Azure Portal](./media/functions-create-first-azure-function/function-app-in-portal-editor.png)

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="log-in-to-azure"></a><span data-ttu-id="dcad3-107">Logga in på Azure</span><span class="sxs-lookup"><span data-stu-id="dcad3-107">Log in to Azure</span></span>

<span data-ttu-id="dcad3-108">Logga in på [Azure-portalen](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="dcad3-108">Log in to the [Azure portal](https://portal.azure.com/).</span></span>

## <a name="create-a-function-app"></a><span data-ttu-id="dcad3-109">Skapa en funktionsapp</span><span class="sxs-lookup"><span data-stu-id="dcad3-109">Create a function app</span></span>

<span data-ttu-id="dcad3-110">Du måste ha en funktionsapp som värd för körning av dina funktioner.</span><span class="sxs-lookup"><span data-stu-id="dcad3-110">You must have a function app to host the execution of your functions.</span></span> <span data-ttu-id="dcad3-111">I en funktionsapp kan du gruppera funktioner som en logisk enhet så att det blir enklare att hantera, distribuera och dela resurser.</span><span class="sxs-lookup"><span data-stu-id="dcad3-111">A function app lets you group functions as a logic unit for easier management, deployment, and sharing of resources.</span></span> 

[!INCLUDE [Create function app Azure portal](../../includes/functions-create-function-app-portal.md)]

[!INCLUDE [functions-portal-favorite-function-apps](../../includes/functions-portal-favorite-function-apps.md)]

<span data-ttu-id="dcad3-112">Därefter skapar du en funktion i den nya funktionsappen.</span><span class="sxs-lookup"><span data-stu-id="dcad3-112">Next, you create a function in the new function app.</span></span>

## <span data-ttu-id="dcad3-113"><a name="create-function"></a>Skapa en HTTP-utlöst funktion</span><span class="sxs-lookup"><span data-stu-id="dcad3-113"><a name="create-function"></a>Create an HTTP triggered function</span></span>

1. <span data-ttu-id="dcad3-114">Expandera den nya funktionsappen och klicka på knappen **+** bredvid **Funktioner**.</span><span class="sxs-lookup"><span data-stu-id="dcad3-114">Expand your new function app, then click the **+** button next to **Functions**.</span></span>

2.  <span data-ttu-id="dcad3-115">På sidan **Kom igång snabbt** väljer du **WebHook + API**. **Välj ett språk** för funktionen och klicka på **Skapa den här funktionen**.</span><span class="sxs-lookup"><span data-stu-id="dcad3-115">In the **Get started quickly** page, select **WebHook + API**, **Choose a language** for your function, and click **Create this function**.</span></span> 
   
    ![Snabbstart för funktioner i Azure Portal.](./media/functions-create-first-azure-function/function-app-quickstart-node-webhook.png)

<span data-ttu-id="dcad3-117">En funktion skapas i ditt valda språk med hjälp av mallen för en HTTP-utlöst funktion.</span><span class="sxs-lookup"><span data-stu-id="dcad3-117">A function is created in your chosen language using the template for an HTTP triggered function.</span></span> <span data-ttu-id="dcad3-118">Du kan köra den nya funktionen genom att skicka en HTTP-begäran.</span><span class="sxs-lookup"><span data-stu-id="dcad3-118">You can run the new function by sending an HTTP request.</span></span>

## <a name="test-the-function"></a><span data-ttu-id="dcad3-119">Testa funktionen</span><span class="sxs-lookup"><span data-stu-id="dcad3-119">Test the function</span></span>

1. <span data-ttu-id="dcad3-120">Klicka på den nya funktionen **</> Hämta funktions-URL**, välj **Standard (funktionsnyckel)** och klicka sedan på **Kopiera**.</span><span class="sxs-lookup"><span data-stu-id="dcad3-120">In your new function, click **</> Get function URL**, select **default (Function key)**, and then click **Copy**.</span></span> 

    ![Kopiera funktionswebbadressen från Azure Portal](./media/functions-create-first-azure-function/function-app-develop-tab-testing.png)

2. <span data-ttu-id="dcad3-122">Klistra in funktionens URL i adressfältet för din webbläsare.</span><span class="sxs-lookup"><span data-stu-id="dcad3-122">Paste the function URL into your browser's address bar.</span></span> <span data-ttu-id="dcad3-123">Lägg till frågesträngen `&name=<yourname>` till den här URL:en och tryck på knappen `Enter` på tangentbordet för att utföra begäran.</span><span class="sxs-lookup"><span data-stu-id="dcad3-123">Append the query string `&name=<yourname>` to this URL and press the `Enter` key on your keyboard to execute the request.</span></span> <span data-ttu-id="dcad3-124">Följande är ett exempel på svaret som returnerades av funktionen i Edge-webbläsaren:</span><span class="sxs-lookup"><span data-stu-id="dcad3-124">The following is an example of the response returned by the function in the Edge browser:</span></span>

    ![Funktionssvar i webbläsaren.](./media/functions-create-first-azure-function/function-app-browser-testing.png)

    <span data-ttu-id="dcad3-126">Begäransadressen innehåller en nyckel som krävs för åtkomst till din funktion över HTTP.</span><span class="sxs-lookup"><span data-stu-id="dcad3-126">The request URL includes a key that is required, by default, to access your function over HTTP.</span></span>   

3. <span data-ttu-id="dcad3-127">När din funktion körs skrivs spårningsinformation till loggarna.</span><span class="sxs-lookup"><span data-stu-id="dcad3-127">When your function runs, trace information is written to the logs.</span></span> <span data-ttu-id="dcad3-128">Om du vill visa spårningsinformationen från föregående körning återgår du till funktionen i portalen och klickar på uppåtpilen längst ned på skärmen så att **Loggar** expanderas.</span><span class="sxs-lookup"><span data-stu-id="dcad3-128">To see the trace output from the previous execution, return to your function in the portal and click the up arrow at the bottom of the screen to expand **Logs**.</span></span> 

   ![Funktionsloggvisning i Azure Portal.](./media/functions-create-first-azure-function/function-view-logs.png)

## <a name="clean-up-resources"></a><span data-ttu-id="dcad3-130">Rensa resurser</span><span class="sxs-lookup"><span data-stu-id="dcad3-130">Clean up resources</span></span>

[!INCLUDE [Clean up resources](../../includes/functions-quickstart-cleanup.md)]

## <a name="next-steps"></a><span data-ttu-id="dcad3-131">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="dcad3-131">Next steps</span></span>

<span data-ttu-id="dcad3-132">Du har nu skapat en funktionsapp med en enkel HTTP-utlöst funktion.</span><span class="sxs-lookup"><span data-stu-id="dcad3-132">You have created a function app with a simple HTTP triggered function.</span></span>  

[!INCLUDE [Next steps note](../../includes/functions-quickstart-next-steps.md)]

<span data-ttu-id="dcad3-133">Mer information finns i [Azure Functions HTTP and webhook bindings](functions-bindings-http-webhook.md) (HTTP- och webhookbindningar i Azure Functions).</span><span class="sxs-lookup"><span data-stu-id="dcad3-133">For more information, see [Azure Functions HTTP and webhook bindings](functions-bindings-http-webhook.md).</span></span>



