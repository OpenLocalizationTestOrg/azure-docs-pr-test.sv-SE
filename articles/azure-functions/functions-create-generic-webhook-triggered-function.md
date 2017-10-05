---
title: "Skapa en funktion i Azure som utlöses av en allmän webhook | Microsoft Docs"
description: "Använd Azure Functions för att skapa en serverlösa funktion som anropas av en webhook i Azure."
services: functions
documentationcenter: na
author: ggailey777
manager: cfowler
editor: 
tags: 
ms.assetid: fafc10c0-84da-4404-b4fa-eea03c7bf2b1
ms.service: functions
ms.devlang: multiple
ms.topic: quickstart
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 08/12/2017
ms.author: glenga
ms.custom: mvc
ms.openlocfilehash: f283f8d79c5ae5fb6a72c84c9e9edb7bb8de4a83
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="create-a-function-triggered-by-a-generic-webhook"></a><span data-ttu-id="e7354-103">Skapa en funktion som utlöses av en allmän webhook</span><span class="sxs-lookup"><span data-stu-id="e7354-103">Create a function triggered by a generic webhook</span></span>

<span data-ttu-id="e7354-104">Med Azure Functions kan du köra kod i en serverfri miljö utan att först behöva skapa en virtuell dator eller publicera en webbapp.</span><span class="sxs-lookup"><span data-stu-id="e7354-104">Azure Functions lets you execute your code in a serverless environment without having to first create a VM or publish a web application.</span></span> <span data-ttu-id="e7354-105">Du kan till exempel konfigurera en funktion som utlöses av en avisering som skapats av Azure-Monitor.</span><span class="sxs-lookup"><span data-stu-id="e7354-105">For example, you can configure a function to be triggered by an alert raised by Azure Monitor.</span></span> <span data-ttu-id="e7354-106">Det här avsnittet visar hur du kör C#-kod när en resursgrupp läggs till din prenumeration.</span><span class="sxs-lookup"><span data-stu-id="e7354-106">This topic shows you how to execute C# code when a resource group is added to your subscription.</span></span>   

![Allmän webhook aktiveras funktionen i Azure-portalen](./media/functions-create-generic-webhook-triggered-function/function-completed.png)

## <a name="prerequisites"></a><span data-ttu-id="e7354-108">Krav</span><span class="sxs-lookup"><span data-stu-id="e7354-108">Prerequisites</span></span> 

<span data-ttu-id="e7354-109">För att slutföra den här självstudien behöver du:</span><span class="sxs-lookup"><span data-stu-id="e7354-109">To complete this tutorial:</span></span>

+ <span data-ttu-id="e7354-110">Om du inte har en Azure-prenumeration kan du skapa ett [kostnadsfritt konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) innan du börjar.</span><span class="sxs-lookup"><span data-stu-id="e7354-110">If you don't have an Azure subscription, create a [free account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) before you begin.</span></span>

[!INCLUDE [functions-portal-favorite-function-apps](../../includes/functions-portal-favorite-function-apps.md)]

## <a name="create-an-azure-function-app"></a><span data-ttu-id="e7354-111">Skapa en Azure Functions-app</span><span class="sxs-lookup"><span data-stu-id="e7354-111">Create an Azure Function app</span></span>

[!INCLUDE [Create function app Azure portal](../../includes/functions-create-function-app-portal.md)]

<span data-ttu-id="e7354-112">Därefter skapar du en funktion i den nya funktionsappen.</span><span class="sxs-lookup"><span data-stu-id="e7354-112">Next, you create a function in the new function app.</span></span>

## <span data-ttu-id="e7354-113"><a name="create-function"></a>Skapa en allmän webhook utlöses-funktion</span><span class="sxs-lookup"><span data-stu-id="e7354-113"><a name="create-function"></a>Create a generic webhook triggered function</span></span>

1. <span data-ttu-id="e7354-114">Expandera funktionsappen och klicka på knappen **+** bredvid **Funktioner**.</span><span class="sxs-lookup"><span data-stu-id="e7354-114">Expand your function app and click the **+** button next to **Functions**.</span></span> <span data-ttu-id="e7354-115">Om den här funktionen är den första i funktionen appen, Välj **anpassad funktionen**.</span><span class="sxs-lookup"><span data-stu-id="e7354-115">If this function is the first one in your function app, select **Custom function**.</span></span> <span data-ttu-id="e7354-116">Detta visar en fullständig uppsättning med funktionsmallar.</span><span class="sxs-lookup"><span data-stu-id="e7354-116">This displays the complete set of function templates.</span></span>

    ![Sidan snabbstart för funktioner i Azure Portal](./media/functions-create-generic-webhook-triggered-function/add-first-function.png)

2. <span data-ttu-id="e7354-118">Välj den **allmän WebHook - C#** mall.</span><span class="sxs-lookup"><span data-stu-id="e7354-118">Select the **Generic WebHook - C#** template.</span></span> <span data-ttu-id="e7354-119">Skriv ett namn för ditt C#-funktionen och sedan välja **skapa**.</span><span class="sxs-lookup"><span data-stu-id="e7354-119">Type a name for your C# function, then select **Create**.</span></span>

     ![Skapa en allmän webhook utlöses funktion i Azure-portalen](./media/functions-create-generic-webhook-triggered-function/functions-create-generic-webhook-trigger.png) 

2. <span data-ttu-id="e7354-121">Klicka på den nya funktionen **<> / Get funktions-URL**, kopiera och spara värdet.</span><span class="sxs-lookup"><span data-stu-id="e7354-121">In your new function, click **</> Get function URL**, then copy and save the value.</span></span> <span data-ttu-id="e7354-122">Du kan använda det här värdet för att konfigurera webhooken.</span><span class="sxs-lookup"><span data-stu-id="e7354-122">You use this value to configure the webhook.</span></span> 

    ![Granska funktionskoden](./media/functions-create-generic-webhook-triggered-function/functions-copy-function-url.png)
         
<span data-ttu-id="e7354-124">Därefter skapar du en webhook-slutpunkt i en aktivitet loggen avisering i Azure-Monitor.</span><span class="sxs-lookup"><span data-stu-id="e7354-124">Next, you create a webhook endpoint in an activity log alert in Azure Monitor.</span></span> 

## <a name="create-an-activity-log-alert"></a><span data-ttu-id="e7354-125">Skapa en aktivitet loggen avisering</span><span class="sxs-lookup"><span data-stu-id="e7354-125">Create an activity log alert</span></span>

1. <span data-ttu-id="e7354-126">I Azure portal, navigerar du till den **övervakaren** tjänsten, Välj **aviseringar**, och klicka på **Lägg till aktivitet loggen avisering**.</span><span class="sxs-lookup"><span data-stu-id="e7354-126">In the Azure portal, navigate to the **Monitor** service, select **Alerts**, and click **Add activity log alert**.</span></span>   

    ![Övervaka](./media/functions-create-generic-webhook-triggered-function/functions-monitor-add-alert.png)

2. <span data-ttu-id="e7354-128">Använd inställningarna på det sätt som beskrivs i tabellen:</span><span class="sxs-lookup"><span data-stu-id="e7354-128">Use the settings as specified in the table:</span></span>

    ![Skapa en aktivitet loggen avisering](./media/functions-create-generic-webhook-triggered-function/functions-monitor-add-alert-settings.png)

    | <span data-ttu-id="e7354-130">Inställning</span><span class="sxs-lookup"><span data-stu-id="e7354-130">Setting</span></span>      |  <span data-ttu-id="e7354-131">Föreslaget värde</span><span class="sxs-lookup"><span data-stu-id="e7354-131">Suggested value</span></span>   | <span data-ttu-id="e7354-132">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="e7354-132">Description</span></span>                              |
    | ------------ |  ------- | -------------------------------------------------- |
    | <span data-ttu-id="e7354-133">**Aktiviteten loggnamn för avisering**</span><span class="sxs-lookup"><span data-stu-id="e7354-133">**Activity log alert name**</span></span> | <span data-ttu-id="e7354-134">resurs-grupp-skapa-avisering</span><span class="sxs-lookup"><span data-stu-id="e7354-134">resource-group-create-alert</span></span> | <span data-ttu-id="e7354-135">Namnet på aktiviteten loggen aviseringen.</span><span class="sxs-lookup"><span data-stu-id="e7354-135">Name of the activity log alert.</span></span> |
    | <span data-ttu-id="e7354-136">**Prenumeration**</span><span class="sxs-lookup"><span data-stu-id="e7354-136">**Subscription**</span></span> | <span data-ttu-id="e7354-137">Din prenumeration</span><span class="sxs-lookup"><span data-stu-id="e7354-137">Your subscription</span></span> | <span data-ttu-id="e7354-138">Den prenumeration som du använder för den här kursen.</span><span class="sxs-lookup"><span data-stu-id="e7354-138">The subscription you are using for this tutorial.</span></span> | 
    |  <span data-ttu-id="e7354-139">**Resursgrupp**</span><span class="sxs-lookup"><span data-stu-id="e7354-139">**Resource Group**</span></span> | <span data-ttu-id="e7354-140">myResourceGroup</span><span class="sxs-lookup"><span data-stu-id="e7354-140">myResourceGroup</span></span> | <span data-ttu-id="e7354-141">Den resursgrupp som aviseringen resurser har distribuerats till.</span><span class="sxs-lookup"><span data-stu-id="e7354-141">The resource group that the alert resources are deployed to.</span></span> <span data-ttu-id="e7354-142">Med hjälp av samma resursgrupp som din funktionsapp gör det enklare att rensa när du har slutfört guiden.</span><span class="sxs-lookup"><span data-stu-id="e7354-142">Using the same resource group as your function app makes it easier to clean up after you complete the tutorial.</span></span> |
    | <span data-ttu-id="e7354-143">**Händelsekategori**</span><span class="sxs-lookup"><span data-stu-id="e7354-143">**Event category**</span></span> | <span data-ttu-id="e7354-144">Administrativa</span><span class="sxs-lookup"><span data-stu-id="e7354-144">Administrative</span></span> | <span data-ttu-id="e7354-145">Den här kategorin innehåller ändringar som gjorts i Azure-resurser.</span><span class="sxs-lookup"><span data-stu-id="e7354-145">This category includes changes made to Azure resources.</span></span>  |
    | <span data-ttu-id="e7354-146">**Resurstyp**</span><span class="sxs-lookup"><span data-stu-id="e7354-146">**Resource type**</span></span> | <span data-ttu-id="e7354-147">Resursgrupper</span><span class="sxs-lookup"><span data-stu-id="e7354-147">Resource groups</span></span> | <span data-ttu-id="e7354-148">Filtrerar aviseringar till resursen gruppaktiviteter.</span><span class="sxs-lookup"><span data-stu-id="e7354-148">Filters alerts to resource group activities.</span></span> |
    | <span data-ttu-id="e7354-149">**Resursgrupp**</span><span class="sxs-lookup"><span data-stu-id="e7354-149">**Resource Group**</span></span><br/><span data-ttu-id="e7354-150">och **resurs**</span><span class="sxs-lookup"><span data-stu-id="e7354-150">and **Resource**</span></span> | <span data-ttu-id="e7354-151">Alla</span><span class="sxs-lookup"><span data-stu-id="e7354-151">All</span></span> | <span data-ttu-id="e7354-152">Övervaka alla resurser.</span><span class="sxs-lookup"><span data-stu-id="e7354-152">Monitor all resources.</span></span> |
    | <span data-ttu-id="e7354-153">**Åtgärdsnamn**</span><span class="sxs-lookup"><span data-stu-id="e7354-153">**Operation name**</span></span> | <span data-ttu-id="e7354-154">Skapa resursgrupp</span><span class="sxs-lookup"><span data-stu-id="e7354-154">Create Resource Group</span></span> | <span data-ttu-id="e7354-155">Filtrerar aviseringar för att skapa åtgärder.</span><span class="sxs-lookup"><span data-stu-id="e7354-155">Filters alerts to create operations.</span></span> |
    | <span data-ttu-id="e7354-156">**Nivå**</span><span class="sxs-lookup"><span data-stu-id="e7354-156">**Level**</span></span> | <span data-ttu-id="e7354-157">Information</span><span class="sxs-lookup"><span data-stu-id="e7354-157">Informational</span></span> | <span data-ttu-id="e7354-158">Omfatta nivå informationsaviseringar.</span><span class="sxs-lookup"><span data-stu-id="e7354-158">Include informational level alerts.</span></span> | 
    | <span data-ttu-id="e7354-159">**Status**</span><span class="sxs-lookup"><span data-stu-id="e7354-159">**Status**</span></span> | <span data-ttu-id="e7354-160">Lyckades</span><span class="sxs-lookup"><span data-stu-id="e7354-160">Succeeded</span></span> | <span data-ttu-id="e7354-161">Filtrerar aviseringar till åtgärder som har slutförts.</span><span class="sxs-lookup"><span data-stu-id="e7354-161">Filters alerts to actions that have completed successfully.</span></span> |
    | <span data-ttu-id="e7354-162">**Grupp**</span><span class="sxs-lookup"><span data-stu-id="e7354-162">**Action group**</span></span> | <span data-ttu-id="e7354-163">Ny</span><span class="sxs-lookup"><span data-stu-id="e7354-163">New</span></span> | <span data-ttu-id="e7354-164">Skapa en ny åtgärdsgrupp, som definierar åtgärden tar när en avisering utlöses.</span><span class="sxs-lookup"><span data-stu-id="e7354-164">Create a new action group, which defines the action takes when an alert is raised.</span></span> |
    | <span data-ttu-id="e7354-165">**Åtgärden gruppnamn**</span><span class="sxs-lookup"><span data-stu-id="e7354-165">**Action group name**</span></span> | <span data-ttu-id="e7354-166">funktionen webhooken</span><span class="sxs-lookup"><span data-stu-id="e7354-166">function-webhook</span></span> | <span data-ttu-id="e7354-167">Ett namn som identifierar åtgärdsgruppen.</span><span class="sxs-lookup"><span data-stu-id="e7354-167">A name to identify the action group.</span></span>  | 
    | <span data-ttu-id="e7354-168">**Kort namn**</span><span class="sxs-lookup"><span data-stu-id="e7354-168">**Short name**</span></span> | <span data-ttu-id="e7354-169">funcwebhook</span><span class="sxs-lookup"><span data-stu-id="e7354-169">funcwebhook</span></span> | <span data-ttu-id="e7354-170">Ett kort namn för åtgärdsgruppen.</span><span class="sxs-lookup"><span data-stu-id="e7354-170">A short name for the action group.</span></span> |  

3. <span data-ttu-id="e7354-171">I **åtgärder**, lägga till en åtgärd med inställningar som anges i tabellen:</span><span class="sxs-lookup"><span data-stu-id="e7354-171">In **Actions**, add an action using the settings as specified in the table:</span></span> 

    ![Lägg till en grupp](./media/functions-create-generic-webhook-triggered-function/functions-monitor-add-alert-settings-2.png)

    | <span data-ttu-id="e7354-173">Inställning</span><span class="sxs-lookup"><span data-stu-id="e7354-173">Setting</span></span>      |  <span data-ttu-id="e7354-174">Föreslaget värde</span><span class="sxs-lookup"><span data-stu-id="e7354-174">Suggested value</span></span>   | <span data-ttu-id="e7354-175">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="e7354-175">Description</span></span>                              |
    | ------------ |  ------- | -------------------------------------------------- |
    | <span data-ttu-id="e7354-176">**Namn**</span><span class="sxs-lookup"><span data-stu-id="e7354-176">**Name**</span></span> | <span data-ttu-id="e7354-177">CallFunctionWebhook</span><span class="sxs-lookup"><span data-stu-id="e7354-177">CallFunctionWebhook</span></span> | <span data-ttu-id="e7354-178">Ett namn för åtgärden.</span><span class="sxs-lookup"><span data-stu-id="e7354-178">A name for the action.</span></span> |
    | <span data-ttu-id="e7354-179">**Åtgärdstyp**</span><span class="sxs-lookup"><span data-stu-id="e7354-179">**Action type**</span></span> | <span data-ttu-id="e7354-180">Webhook</span><span class="sxs-lookup"><span data-stu-id="e7354-180">Webhook</span></span> | <span data-ttu-id="e7354-181">Svaret på aviseringen är att en Webhooksadressen anropas.</span><span class="sxs-lookup"><span data-stu-id="e7354-181">The response to the alert is that a Webhook URL is called.</span></span> |
    | <span data-ttu-id="e7354-182">**Detaljer**</span><span class="sxs-lookup"><span data-stu-id="e7354-182">**Details**</span></span> | <span data-ttu-id="e7354-183">Funktions-URL</span><span class="sxs-lookup"><span data-stu-id="e7354-183">Function URL</span></span> | <span data-ttu-id="e7354-184">Klistra in i Webhooksadressen för den funktion som du kopierade tidigare.</span><span class="sxs-lookup"><span data-stu-id="e7354-184">Paste in the webhook URL of the function that you copied earlier.</span></span> |<span data-ttu-id="e7354-185">v</span><span class="sxs-lookup"><span data-stu-id="e7354-185">v</span></span>

4. <span data-ttu-id="e7354-186">Klicka på **OK** att skapa gruppen aviseringen och åtgärden.</span><span class="sxs-lookup"><span data-stu-id="e7354-186">Click **OK** to create the alert and action group.</span></span>  

<span data-ttu-id="e7354-187">Webhooken kallas nu när gruppen skapas i din prenumeration.</span><span class="sxs-lookup"><span data-stu-id="e7354-187">The webhook is now called when a resource group is created in your subscription.</span></span> <span data-ttu-id="e7354-188">Därefter uppdaterar du koden i din funktion för att hantera loggdata JSON i brödtexten i begäran.</span><span class="sxs-lookup"><span data-stu-id="e7354-188">Next, you update the code in your function to handle the JSON log data in the body of the request.</span></span>   

## <a name="update-the-function-code"></a><span data-ttu-id="e7354-189">Uppdatera funktionskoden</span><span class="sxs-lookup"><span data-stu-id="e7354-189">Update the function code</span></span>

1. <span data-ttu-id="e7354-190">Gå tillbaka till din funktionsapp i portalen och expandera din funktion.</span><span class="sxs-lookup"><span data-stu-id="e7354-190">Navigate back to your function app in the portal, and expand your function.</span></span> 

2. <span data-ttu-id="e7354-191">Ersätt skriptkod C# i funktionen i portalen med följande kod:</span><span class="sxs-lookup"><span data-stu-id="e7354-191">Replace the C# script code in the function in the portal with the following code:</span></span>

    ```csharp
    #r "Newtonsoft.Json"
    
    using System;
    using System.Net;
    using Newtonsoft.Json;
    using Newtonsoft.Json.Linq;
    
    public static async Task<object> Run(HttpRequestMessage req, TraceWriter log)
    {
        log.Info($"Webhook was triggered!");
    
        // Get the activityLog object from the JSON in the message body.
        string jsonContent = await req.Content.ReadAsStringAsync();
        JToken activityLog = JObject.Parse(jsonContent.ToString())
            .SelectToken("data.context.activityLog");
    
        // Return an error if the resource in the activity log isn't a resource group. 
        if (activityLog == null || !string.Equals((string)activityLog["resourceType"], 
            "Microsoft.Resources/subscriptions/resourcegroups"))
        {
            log.Error("An error occured");
            return req.CreateResponse(HttpStatusCode.BadRequest, new
            {
                error = "Unexpected message payload or wrong alert received."
            });
        }
    
        // Write information about the created resource group to the streaming log.
        log.Info(string.Format("Resource group '{0}' was {1} on {2}.",
            (string)activityLog["resourceGroupName"],
            ((string)activityLog["subStatus"]).ToLower(), 
            (DateTime)activityLog["submissionTimestamp"]));
    
        return req.CreateResponse(HttpStatusCode.OK);    
    }
    ```

<span data-ttu-id="e7354-192">Nu kan du testa funktionen genom att skapa en ny resursgrupp i din prenumeration.</span><span class="sxs-lookup"><span data-stu-id="e7354-192">Now you can test the function by creating a new resource group in your subscription.</span></span>

## <a name="test-the-function"></a><span data-ttu-id="e7354-193">Testa funktionen</span><span class="sxs-lookup"><span data-stu-id="e7354-193">Test the function</span></span>

1. <span data-ttu-id="e7354-194">Klicka på ikonen resurs grupp till vänster i Azure portal, Välj **+ Lägg till**, ange ett **resursgruppens namn**, och välj **skapa** att skapa en tom resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="e7354-194">Click the resource group icon in the left of the Azure portal, select **+ Add**, type a **Resource group name**, and select **Create** to create an empty resource group.</span></span>
    
    ![Skapa en resursgrupp.](./media/functions-create-generic-webhook-triggered-function/functions-create-resource-group.png)

2. <span data-ttu-id="e7354-196">Gå tillbaka till din funktion och expandera den **loggar** fönster.</span><span class="sxs-lookup"><span data-stu-id="e7354-196">Go back to your function and expand the **Logs** window.</span></span> <span data-ttu-id="e7354-197">När du har skapat resursgruppen aktivitet loggen aviseringen utlöser webhooken och funktionen körs.</span><span class="sxs-lookup"><span data-stu-id="e7354-197">After the resource group is created, the activity log alert triggers the webhook and the function executes.</span></span> <span data-ttu-id="e7354-198">Du kan se namnet på den nya resursgruppen som skrivs till loggarna.</span><span class="sxs-lookup"><span data-stu-id="e7354-198">You see the name of the new resource group written to the logs.</span></span>  

    ![Lägga till en programinställning för testet.](./media/functions-create-generic-webhook-triggered-function/function-view-logs.png)

3. <span data-ttu-id="e7354-200">(Valfritt) Gå tillbaka och ta bort resursgruppen som du skapade.</span><span class="sxs-lookup"><span data-stu-id="e7354-200">(Optional) Go back and delete the resource group that you created.</span></span> <span data-ttu-id="e7354-201">Observera att den här aktiviteten inte utlöses funktionen.</span><span class="sxs-lookup"><span data-stu-id="e7354-201">Note that this activity doesn't trigger the function.</span></span> <span data-ttu-id="e7354-202">Detta beror på att ta bort operations filtreras bort av aviseringen.</span><span class="sxs-lookup"><span data-stu-id="e7354-202">This is because delete operations are filtered out by the alert.</span></span> 

## <a name="clean-up-resources"></a><span data-ttu-id="e7354-203">Rensa resurser</span><span class="sxs-lookup"><span data-stu-id="e7354-203">Clean up resources</span></span>

[!INCLUDE [Next steps note](../../includes/functions-quickstart-cleanup.md)]

## <a name="next-steps"></a><span data-ttu-id="e7354-204">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="e7354-204">Next steps</span></span>

<span data-ttu-id="e7354-205">Du har skapat en funktion som körs när en begäran tas emot från en allmän webhook.</span><span class="sxs-lookup"><span data-stu-id="e7354-205">You have created a function that runs when a request is received from a generic webhook.</span></span> 

[!INCLUDE [Next steps note](../../includes/functions-quickstart-next-steps.md)]

<span data-ttu-id="e7354-206">Mer information om webhook-utlösare finns i [Azure Functions HTTP och webhook-bindningar](functions-bindings-http-webhook.md).</span><span class="sxs-lookup"><span data-stu-id="e7354-206">For more information about webhook triggers, see [Azure Functions HTTP and webhook bindings](functions-bindings-http-webhook.md).</span></span> <span data-ttu-id="e7354-207">Mer information om hur du utvecklar funktioner i C# finns [Azure Functions C# skript för utvecklare](functions-reference-csharp.md).</span><span class="sxs-lookup"><span data-stu-id="e7354-207">To learn more about developing functions in C#, see [Azure Functions C# script developer reference](functions-reference-csharp.md).</span></span>

