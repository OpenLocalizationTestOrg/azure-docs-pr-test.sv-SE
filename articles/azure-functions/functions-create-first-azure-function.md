---
title: "aaaCreate din första funktionen från hello Azure Portal | Microsoft Docs"
description: "Lär dig hur toocreate ditt första Azure fungerar för serverlösa körning med hello Azure-portalen."
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
ms.openlocfilehash: 84283d7d4bc6015061946af4589f9a70ae61f36b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-your-first-function-in-hello-azure-portal"></a><span data-ttu-id="d5650-103">Skapa din första funktion i hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="d5650-103">Create your first function in hello Azure portal</span></span>

<span data-ttu-id="d5650-104">Azure Functions kan du köra din kod i en miljö med serverlösa utan toofirst skapa en virtuell dator eller publicera ett webbprogram.</span><span class="sxs-lookup"><span data-stu-id="d5650-104">Azure Functions lets you execute your code in a serverless environment without having toofirst create a VM or publish a web application.</span></span> <span data-ttu-id="d5650-105">I det här avsnittet lär du dig hur fungerar toouse toocreate ”hello world”-funktionen i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="d5650-105">In this topic, learn how toouse Functions toocreate a "hello world" function in hello Azure portal.</span></span>

![Skapa funktionsapp i hello Azure-portalen](./media/functions-create-first-azure-function/function-app-in-portal-editor.png)

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="log-in-tooazure"></a><span data-ttu-id="d5650-107">Logga in tooAzure</span><span class="sxs-lookup"><span data-stu-id="d5650-107">Log in tooAzure</span></span>

<span data-ttu-id="d5650-108">Logga in toohello [Azure-portalen](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="d5650-108">Log in toohello [Azure portal](https://portal.azure.com/).</span></span>

## <a name="create-a-function-app"></a><span data-ttu-id="d5650-109">Skapa en funktionsapp</span><span class="sxs-lookup"><span data-stu-id="d5650-109">Create a function app</span></span>

<span data-ttu-id="d5650-110">Du måste ha en funktion app toohost hello körningen av dina funktioner.</span><span class="sxs-lookup"><span data-stu-id="d5650-110">You must have a function app toohost hello execution of your functions.</span></span> <span data-ttu-id="d5650-111">I en funktionsapp kan du gruppera funktioner som en logisk enhet så att det blir enklare att hantera, distribuera och dela resurser.</span><span class="sxs-lookup"><span data-stu-id="d5650-111">A function app lets you group functions as a logic unit for easier management, deployment, and sharing of resources.</span></span> 

[!INCLUDE [Create function app Azure portal](../../includes/functions-create-function-app-portal.md)]

[!INCLUDE [functions-portal-favorite-function-apps](../../includes/functions-portal-favorite-function-apps.md)]

<span data-ttu-id="d5650-112">Därefter skapar du en funktion i hello ny funktionsapp.</span><span class="sxs-lookup"><span data-stu-id="d5650-112">Next, you create a function in hello new function app.</span></span>

## <span data-ttu-id="d5650-113"><a name="create-function"></a>Skapa en HTTP-utlöst funktion</span><span class="sxs-lookup"><span data-stu-id="d5650-113"><a name="create-function"></a>Create an HTTP triggered function</span></span>

1. <span data-ttu-id="d5650-114">Expandera den nya funktion appen och klicka sedan på hello  **+**  knappen för nästa**funktioner**.</span><span class="sxs-lookup"><span data-stu-id="d5650-114">Expand your new function app, then click hello **+** button next too**Functions**.</span></span>

2.  <span data-ttu-id="d5650-115">I hello **komma igång snabbt** väljer **WebHook + API**, **väljer du ett språk** för din funktion och klickar på **skapa den här funktionen** .</span><span class="sxs-lookup"><span data-stu-id="d5650-115">In hello **Get started quickly** page, select **WebHook + API**, **Choose a language** for your function, and click **Create this function**.</span></span> 
   
    ![Functions-Snabbstart i hello Azure-portalen.](./media/functions-create-first-azure-function/function-app-quickstart-node-webhook.png)

<span data-ttu-id="d5650-117">En funktion som har skapats i ditt valda språk med hjälp av hello mall för en HTTP-utlöses funktion.</span><span class="sxs-lookup"><span data-stu-id="d5650-117">A function is created in your chosen language using hello template for an HTTP triggered function.</span></span> <span data-ttu-id="d5650-118">Du kan köra hello nya funktionen genom att skicka en HTTP-begäran.</span><span class="sxs-lookup"><span data-stu-id="d5650-118">You can run hello new function by sending an HTTP request.</span></span>

## <a name="test-hello-function"></a><span data-ttu-id="d5650-119">Testa hello-funktionen</span><span class="sxs-lookup"><span data-stu-id="d5650-119">Test hello function</span></span>

1. <span data-ttu-id="d5650-120">Klicka på den nya funktionen **</> Hämta funktions-URL**, välj **Standard (funktionsnyckel)** och klicka sedan på **Kopiera**.</span><span class="sxs-lookup"><span data-stu-id="d5650-120">In your new function, click **</> Get function URL**, select **default (Function key)**, and then click **Copy**.</span></span> 

    ![Kopiera hello funktions-URL från hello Azure-portalen](./media/functions-create-first-azure-function/function-app-develop-tab-testing.png)

2. <span data-ttu-id="d5650-122">Klistra in hello funktions-URL i adressfältet i webbläsaren.</span><span class="sxs-lookup"><span data-stu-id="d5650-122">Paste hello function URL into your browser's address bar.</span></span> <span data-ttu-id="d5650-123">Lägg till hello frågesträngen `&name=<yourname>` toothis URL och tryck på hello `Enter` nyckel på tangentbordet tooexecute hello begäran.</span><span class="sxs-lookup"><span data-stu-id="d5650-123">Append hello query string `&name=<yourname>` toothis URL and press hello `Enter` key on your keyboard tooexecute hello request.</span></span> <span data-ttu-id="d5650-124">hello följande är ett exempel på hello svaret som returnerades av hello-funktionen i hello Edge-webbläsaren:</span><span class="sxs-lookup"><span data-stu-id="d5650-124">hello following is an example of hello response returned by hello function in hello Edge browser:</span></span>

    ![Funktionen svar i hello webbläsare.](./media/functions-create-first-azure-function/function-app-browser-testing.png)

    <span data-ttu-id="d5650-126">hello begäran URL innehåller en nyckel som krävs, som standard tooaccess din funktion via HTTP.</span><span class="sxs-lookup"><span data-stu-id="d5650-126">hello request URL includes a key that is required, by default, tooaccess your function over HTTP.</span></span>   

3. <span data-ttu-id="d5650-127">När din funktion körs, skrivs spårningsinformation toohello loggar.</span><span class="sxs-lookup"><span data-stu-id="d5650-127">When your function runs, trace information is written toohello logs.</span></span> <span data-ttu-id="d5650-128">toosee hello spårningsutdata från hello tidigare körning och returnera tooyour funktion i hello portal och klicka på hello UPPIL längst hello hello skärmen tooexpand **loggar**.</span><span class="sxs-lookup"><span data-stu-id="d5650-128">toosee hello trace output from hello previous execution, return tooyour function in hello portal and click hello up arrow at hello bottom of hello screen tooexpand **Logs**.</span></span> 

   ![Funktioner logga viewer i hello Azure-portalen.](./media/functions-create-first-azure-function/function-view-logs.png)

## <a name="clean-up-resources"></a><span data-ttu-id="d5650-130">Rensa resurser</span><span class="sxs-lookup"><span data-stu-id="d5650-130">Clean up resources</span></span>

[!INCLUDE [Clean up resources](../../includes/functions-quickstart-cleanup.md)]

## <a name="next-steps"></a><span data-ttu-id="d5650-131">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="d5650-131">Next steps</span></span>

<span data-ttu-id="d5650-132">Du har nu skapat en funktionsapp med en enkel HTTP-utlöst funktion.</span><span class="sxs-lookup"><span data-stu-id="d5650-132">You have created a function app with a simple HTTP triggered function.</span></span>  

[!INCLUDE [Next steps note](../../includes/functions-quickstart-next-steps.md)]

<span data-ttu-id="d5650-133">Mer information finns i [Azure Functions HTTP and webhook bindings](functions-bindings-http-webhook.md) (HTTP- och webhookbindningar i Azure Functions).</span><span class="sxs-lookup"><span data-stu-id="d5650-133">For more information, see [Azure Functions HTTP and webhook bindings](functions-bindings-http-webhook.md).</span></span>



