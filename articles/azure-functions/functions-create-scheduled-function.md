---
title: "aaaCreate en funktion som körs enligt ett schema i Azure | Microsoft Docs"
description: "Lär dig hur toocreate en funktion i Azure som körs enligt ett schema som du definierar."
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
ms.openlocfilehash: 793b06a65a154466dfd4c121bcc88082227cd597
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-function-in-azure-that-is-triggered-by-a-timer"></a><span data-ttu-id="7f211-103">Skapa en funktion i Azure som utlöses av en timer</span><span class="sxs-lookup"><span data-stu-id="7f211-103">Create a function in Azure that is triggered by a timer</span></span>

<span data-ttu-id="7f211-104">Lär dig hur toouse Azure Functions toocreate en funktion som körs baserat ett schema som du definierar.</span><span class="sxs-lookup"><span data-stu-id="7f211-104">Learn how toouse Azure Functions toocreate a function that runs based a schedule that you define.</span></span>

![Skapa funktionsapp i hello Azure-portalen](./media/functions-create-scheduled-function/function-app-in-portal-editor.png)

## <a name="prerequisites"></a><span data-ttu-id="7f211-106">Krav</span><span class="sxs-lookup"><span data-stu-id="7f211-106">Prerequisites</span></span>

<span data-ttu-id="7f211-107">toocomplete den här kursen:</span><span class="sxs-lookup"><span data-stu-id="7f211-107">toocomplete this tutorial:</span></span>

+ <span data-ttu-id="7f211-108">Om du inte har en Azure-prenumeration kan du skapa ett [kostnadsfritt konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) innan du börjar.</span><span class="sxs-lookup"><span data-stu-id="7f211-108">If you don't have an Azure subscription, create a [free account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) before you begin.</span></span>

[!INCLUDE [functions-portal-favorite-function-apps](../../includes/functions-portal-favorite-function-apps.md)]

## <a name="create-an-azure-function-app"></a><span data-ttu-id="7f211-109">Skapa en Azure Functions-app</span><span class="sxs-lookup"><span data-stu-id="7f211-109">Create an Azure Function app</span></span>

[!INCLUDE [Create function app Azure portal](../../includes/functions-create-function-app-portal.md)]

![Funktionsappen skapades.](./media/functions-create-first-azure-function/function-app-create-success.png)

<span data-ttu-id="7f211-111">Därefter skapar du en funktion i hello ny funktionsapp.</span><span class="sxs-lookup"><span data-stu-id="7f211-111">Next, you create a function in hello new function app.</span></span>

<a name="create-function"></a>

## <a name="create-a-timer-triggered-function"></a><span data-ttu-id="7f211-112">Skapa en timerutlöst funktion</span><span class="sxs-lookup"><span data-stu-id="7f211-112">Create a timer triggered function</span></span>

1. <span data-ttu-id="7f211-113">Expandera funktionen appen och klicka på hello ** + ** knappen för nästa**funktioner**.</span><span class="sxs-lookup"><span data-stu-id="7f211-113">Expand your function app and click hello **+** button next too**Functions**.</span></span> <span data-ttu-id="7f211-114">Om det är första hello-funktion i din funktionsapp **anpassad funktionen**.</span><span class="sxs-lookup"><span data-stu-id="7f211-114">If this is hello first function in your function app, select **Custom function**.</span></span> <span data-ttu-id="7f211-115">Detta visar hello fullständig uppsättning funktionen mallar.</span><span class="sxs-lookup"><span data-stu-id="7f211-115">This displays hello complete set of function templates.</span></span>

    ![Funktioner quickstart sida i hello Azure-portalen](./media/functions-create-scheduled-function/add-first-function.png)

2. <span data-ttu-id="7f211-117">Välj hello **TimerTrigger** mall för språket.</span><span class="sxs-lookup"><span data-stu-id="7f211-117">Select hello **TimerTrigger** template for your desired language.</span></span> <span data-ttu-id="7f211-118">Använd sedan hello inställningar som anges i hello tabell:</span><span class="sxs-lookup"><span data-stu-id="7f211-118">Then use hello settings as specified in hello table:</span></span>

    ![Skapa en som utlöste timerfunktion i hello Azure-portalen.](./media/functions-create-scheduled-function/functions-create-timer-trigger.png)

    | <span data-ttu-id="7f211-120">Inställning</span><span class="sxs-lookup"><span data-stu-id="7f211-120">Setting</span></span> | <span data-ttu-id="7f211-121">Föreslaget värde</span><span class="sxs-lookup"><span data-stu-id="7f211-121">Suggested value</span></span> | <span data-ttu-id="7f211-122">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="7f211-122">Description</span></span> |
    |---|---|---|
    | <span data-ttu-id="7f211-123">**Namnge din funktion**</span><span class="sxs-lookup"><span data-stu-id="7f211-123">**Name your function**</span></span> | <span data-ttu-id="7f211-124">TimerTriggerCSharp1</span><span class="sxs-lookup"><span data-stu-id="7f211-124">TimerTriggerCSharp1</span></span> | <span data-ttu-id="7f211-125">Definierar hello namn tidsinställda utlösts-funktionen.</span><span class="sxs-lookup"><span data-stu-id="7f211-125">Defines hello name of your timer triggered function.</span></span> |
    | <span data-ttu-id="7f211-126">**[Schema](http://en.wikipedia.org/wiki/Cron#CRON_expression)**</span><span class="sxs-lookup"><span data-stu-id="7f211-126">**[Schedule](http://en.wikipedia.org/wiki/Cron#CRON_expression)**</span></span> | <span data-ttu-id="7f211-127">0 \*/1 \* \* \* \*</span><span class="sxs-lookup"><span data-stu-id="7f211-127">0 \*/1 \* \* \* \*</span></span> | <span data-ttu-id="7f211-128">Ett sex fält [CRON-uttryck](http://en.wikipedia.org/wiki/Cron#CRON_expression) som schemalägger din funktion toorun varje minut.</span><span class="sxs-lookup"><span data-stu-id="7f211-128">A six field [CRON expression](http://en.wikipedia.org/wiki/Cron#CRON_expression) that schedules your function toorun every minute.</span></span> |

2. <span data-ttu-id="7f211-129">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="7f211-129">Click **Create**.</span></span> <span data-ttu-id="7f211-130">En funktion skapas i valt språk som körs varje minut.</span><span class="sxs-lookup"><span data-stu-id="7f211-130">A function is created in your chosen language that runs every minute.</span></span>

3. <span data-ttu-id="7f211-131">Kontrollera körning genom att visa spårningsinformation skrivs toohello loggar.</span><span class="sxs-lookup"><span data-stu-id="7f211-131">Verify execution by viewing trace information written toohello logs.</span></span>

    ![Funktioner logga viewer i hello Azure-portalen.](./media/functions-create-scheduled-function/functions-timer-trigger-view-logs2.png)

<span data-ttu-id="7f211-133">Du kan nu ändra hello funktionen schemat så att den körs så ofta som en gång i timmen.</span><span class="sxs-lookup"><span data-stu-id="7f211-133">Now, you can change hello function's schedule so that it runs less often, such as once every hour.</span></span> 

## <a name="update-hello-timer-schedule"></a><span data-ttu-id="7f211-134">Uppdateringsschema hello timer</span><span class="sxs-lookup"><span data-stu-id="7f211-134">Update hello timer schedule</span></span>

1. <span data-ttu-id="7f211-135">Expandera funktionen och klicka på **Integrera**.</span><span class="sxs-lookup"><span data-stu-id="7f211-135">Expand your function and click **Integrate**.</span></span> <span data-ttu-id="7f211-136">Detta är där du ange indata och utdata bindningar för din funktion och även ange hello schema.</span><span class="sxs-lookup"><span data-stu-id="7f211-136">This is where you define input and output bindings for your function and also set hello schedule.</span></span> 

2. <span data-ttu-id="7f211-137">Ange det nya **Schema**-värdet `0 0 */1 * * *` och klicka sedan på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="7f211-137">Enter a new **Schedule** value of `0 0 */1 * * *`, and then click **Save**.</span></span>  

![Funktioner uppdateringsschema timer i hello Azure-portalen.](./media/functions-create-scheduled-function/functions-timer-trigger-change-schedule.png)

<span data-ttu-id="7f211-139">Du har nu en funktion som körs en gång i timmen.</span><span class="sxs-lookup"><span data-stu-id="7f211-139">You now have a function that runs once every hour.</span></span> 

## <a name="clean-up-resources"></a><span data-ttu-id="7f211-140">Rensa resurser</span><span class="sxs-lookup"><span data-stu-id="7f211-140">Clean up resources</span></span>

[!INCLUDE [Next steps note](../../includes/functions-quickstart-cleanup.md)]

## <a name="next-steps"></a><span data-ttu-id="7f211-141">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="7f211-141">Next steps</span></span>

<span data-ttu-id="7f211-142">Du har skapat en funktion som körs enligt ett schema.</span><span class="sxs-lookup"><span data-stu-id="7f211-142">You have created a function that runs based on a schedule.</span></span>

[!INCLUDE [Next steps note](../../includes/functions-quickstart-next-steps.md)]

<span data-ttu-id="7f211-143">Mer information timerutlösare finns i [Schedule code execution with Azure Functions](functions-bindings-timer.md) (Schemalägga kodkörning med Azure Functions).</span><span class="sxs-lookup"><span data-stu-id="7f211-143">For more information timer triggers, see [Schedule code execution with Azure Functions](functions-bindings-timer.md).</span></span>