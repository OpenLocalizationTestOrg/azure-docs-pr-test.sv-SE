---
title: "Skapa en funktion som körs enligt ett schema i Azure | Microsoft Docs"
description: "Lär dig hur du skapar en funktion i Azure som körs enligt ett schema du definierar."
services: functions
documentationcenter: na
author: ggailey777
manager: erikre
editor: 
tags: 
ms.assetid: ba50ee47-58e0-4972-b67b-828f2dc48701
ms.service: functions
ms.devlang: multiple
ms.topic: quickstart
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 05/31/2017
ms.author: glenga
ms.custom: mvc
ms.openlocfilehash: 03cc5e71e8eb20002cf58e713fc0fc92a9129874
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="create-a-function-in-azure-that-is-triggered-by-a-timer"></a><span data-ttu-id="b3bc3-103">Skapa en funktion i Azure som utlöses av en timer</span><span class="sxs-lookup"><span data-stu-id="b3bc3-103">Create a function in Azure that is triggered by a timer</span></span>

<span data-ttu-id="b3bc3-104">Lär dig hur du använder Azure Functions till att skapa en funktion som körs enligt ett schema du definierar.</span><span class="sxs-lookup"><span data-stu-id="b3bc3-104">Learn how to use Azure Functions to create a function that runs based a schedule that you define.</span></span>

![Skapa en funktionsapp i Azure Portal](./media/functions-create-scheduled-function/function-app-in-portal-editor.png)

## <a name="prerequisites"></a><span data-ttu-id="b3bc3-106">Krav</span><span class="sxs-lookup"><span data-stu-id="b3bc3-106">Prerequisites</span></span>

<span data-ttu-id="b3bc3-107">För att slutföra den här självstudien behöver du:</span><span class="sxs-lookup"><span data-stu-id="b3bc3-107">To complete this tutorial:</span></span>

+ <span data-ttu-id="b3bc3-108">Om du inte har en Azure-prenumeration kan du skapa ett [kostnadsfritt konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) innan du börjar.</span><span class="sxs-lookup"><span data-stu-id="b3bc3-108">If you don't have an Azure subscription, create a [free account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) before you begin.</span></span>

[!INCLUDE [functions-portal-favorite-function-apps](../../includes/functions-portal-favorite-function-apps.md)]

## <a name="create-an-azure-function-app"></a><span data-ttu-id="b3bc3-109">Skapa en Azure Functions-app</span><span class="sxs-lookup"><span data-stu-id="b3bc3-109">Create an Azure Function app</span></span>

[!INCLUDE [Create function app Azure portal](../../includes/functions-create-function-app-portal.md)]

![Funktionsappen skapades.](./media/functions-create-first-azure-function/function-app-create-success.png)

<span data-ttu-id="b3bc3-111">Därefter skapar du en funktion i den nya funktionsappen.</span><span class="sxs-lookup"><span data-stu-id="b3bc3-111">Next, you create a function in the new function app.</span></span>

<a name="create-function"></a>

## <a name="create-a-timer-triggered-function"></a><span data-ttu-id="b3bc3-112">Skapa en timerutlöst funktion</span><span class="sxs-lookup"><span data-stu-id="b3bc3-112">Create a timer triggered function</span></span>

1. <span data-ttu-id="b3bc3-113">Expandera funktionsappen och klicka på knappen **+** bredvid **Funktioner**.</span><span class="sxs-lookup"><span data-stu-id="b3bc3-113">Expand your function app and click the **+** button next to **Functions**.</span></span> <span data-ttu-id="b3bc3-114">Om det är den första funktionen i din funktionsapp väljer du **Anpassad funktion**.</span><span class="sxs-lookup"><span data-stu-id="b3bc3-114">If this is the first function in your function app, select **Custom function**.</span></span> <span data-ttu-id="b3bc3-115">Detta visar en fullständig uppsättning med funktionsmallar.</span><span class="sxs-lookup"><span data-stu-id="b3bc3-115">This displays the complete set of function templates.</span></span>

    ![Sidan snabbstart för funktioner i Azure Portal](./media/functions-create-scheduled-function/add-first-function.png)

2. <span data-ttu-id="b3bc3-117">Välj **TimerTrigger**-mallen för språket.</span><span class="sxs-lookup"><span data-stu-id="b3bc3-117">Select the **TimerTrigger** template for your desired language.</span></span> <span data-ttu-id="b3bc3-118">Använd inställningarna som anges i tabellen:</span><span class="sxs-lookup"><span data-stu-id="b3bc3-118">Then use the settings as specified in the table:</span></span>

    ![Skapa en timerutlöst funktion i Azure Portal.](./media/functions-create-scheduled-function/functions-create-timer-trigger.png)

    | <span data-ttu-id="b3bc3-120">Inställning</span><span class="sxs-lookup"><span data-stu-id="b3bc3-120">Setting</span></span> | <span data-ttu-id="b3bc3-121">Föreslaget värde</span><span class="sxs-lookup"><span data-stu-id="b3bc3-121">Suggested value</span></span> | <span data-ttu-id="b3bc3-122">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="b3bc3-122">Description</span></span> |
    |---|---|---|
    | <span data-ttu-id="b3bc3-123">**Namnge din funktion**</span><span class="sxs-lookup"><span data-stu-id="b3bc3-123">**Name your function**</span></span> | <span data-ttu-id="b3bc3-124">TimerTriggerCSharp1</span><span class="sxs-lookup"><span data-stu-id="b3bc3-124">TimerTriggerCSharp1</span></span> | <span data-ttu-id="b3bc3-125">Det här är namnet på den timerutlösta funktionen.</span><span class="sxs-lookup"><span data-stu-id="b3bc3-125">Defines the name of your timer triggered function.</span></span> |
    | <span data-ttu-id="b3bc3-126">**[Schema](http://en.wikipedia.org/wiki/Cron#CRON_expression)**</span><span class="sxs-lookup"><span data-stu-id="b3bc3-126">**[Schedule](http://en.wikipedia.org/wiki/Cron#CRON_expression)**</span></span> | <span data-ttu-id="b3bc3-127">0 \*/1 \* \* \* \*</span><span class="sxs-lookup"><span data-stu-id="b3bc3-127">0 \*/1 \* \* \* \*</span></span> | <span data-ttu-id="b3bc3-128">Ett [CRON-uttryck](http://en.wikipedia.org/wiki/Cron#CRON_expression) med sex fält som schemalägger att funktionen ska köras varje minut.</span><span class="sxs-lookup"><span data-stu-id="b3bc3-128">A six field [CRON expression](http://en.wikipedia.org/wiki/Cron#CRON_expression) that schedules your function to run every minute.</span></span> |

2. <span data-ttu-id="b3bc3-129">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="b3bc3-129">Click **Create**.</span></span> <span data-ttu-id="b3bc3-130">En funktion skapas i valt språk som körs varje minut.</span><span class="sxs-lookup"><span data-stu-id="b3bc3-130">A function is created in your chosen language that runs every minute.</span></span>

3. <span data-ttu-id="b3bc3-131">Kontrollera körningen genom att granska spårningsinformationen som skrivs till loggarna.</span><span class="sxs-lookup"><span data-stu-id="b3bc3-131">Verify execution by viewing trace information written to the logs.</span></span>

    ![Funktionsloggvisning i Azure Portal.](./media/functions-create-scheduled-function/functions-timer-trigger-view-logs2.png)

<span data-ttu-id="b3bc3-133">Du kan nu ändra funktionens schema så att den körs mindre ofta, till exempel en gång i timmen.</span><span class="sxs-lookup"><span data-stu-id="b3bc3-133">Now, you can change the function's schedule so that it runs less often, such as once every hour.</span></span> 

## <a name="update-the-timer-schedule"></a><span data-ttu-id="b3bc3-134">Uppdatera timerschemat</span><span class="sxs-lookup"><span data-stu-id="b3bc3-134">Update the timer schedule</span></span>

1. <span data-ttu-id="b3bc3-135">Expandera funktionen och klicka på **Integrera**.</span><span class="sxs-lookup"><span data-stu-id="b3bc3-135">Expand your function and click **Integrate**.</span></span> <span data-ttu-id="b3bc3-136">Det är här du definierar in- och utdatabindningar för funktionen och även anger schemat.</span><span class="sxs-lookup"><span data-stu-id="b3bc3-136">This is where you define input and output bindings for your function and also set the schedule.</span></span> 

2. <span data-ttu-id="b3bc3-137">Ange det nya **Schema**-värdet `0 0 */1 * * *` och klicka sedan på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="b3bc3-137">Enter a new **Schedule** value of `0 0 */1 * * *`, and then click **Save**.</span></span>  

![Funktioner, uppdatera timerschema i Azure Portal.](./media/functions-create-scheduled-function/functions-timer-trigger-change-schedule.png)

<span data-ttu-id="b3bc3-139">Du har nu en funktion som körs en gång i timmen.</span><span class="sxs-lookup"><span data-stu-id="b3bc3-139">You now have a function that runs once every hour.</span></span> 

## <a name="clean-up-resources"></a><span data-ttu-id="b3bc3-140">Rensa resurser</span><span class="sxs-lookup"><span data-stu-id="b3bc3-140">Clean up resources</span></span>

[!INCLUDE [Next steps note](../../includes/functions-quickstart-cleanup.md)]

## <a name="next-steps"></a><span data-ttu-id="b3bc3-141">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="b3bc3-141">Next steps</span></span>

<span data-ttu-id="b3bc3-142">Du har skapat en funktion som körs enligt ett schema.</span><span class="sxs-lookup"><span data-stu-id="b3bc3-142">You have created a function that runs based on a schedule.</span></span>

[!INCLUDE [Next steps note](../../includes/functions-quickstart-next-steps.md)]

<span data-ttu-id="b3bc3-143">Mer information timerutlösare finns i [Schedule code execution with Azure Functions](functions-bindings-timer.md) (Schemalägga kodkörning med Azure Functions).</span><span class="sxs-lookup"><span data-stu-id="b3bc3-143">For more information timer triggers, see [Schedule code execution with Azure Functions](functions-bindings-timer.md).</span></span>