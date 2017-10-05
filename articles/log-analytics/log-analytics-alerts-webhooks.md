---
title: "Webhook Aviseringsåtgärd provet i OMS Log Analytics | Microsoft Docs"
description: "En av de åtgärder som du kan köra som svar på en avisering om Log Analytics är en * webhook *, där du kan anropa en extern process via en HTTP-begäran. Den här artikeln beskriver hur ett exempel på hur du skapar en webhook-åtgärd i en Log Analytics-avisering med Slack."
services: log-analytics
documentationcenter: 
author: bwren
manager: jwhit
editor: tysonn
ms.assetid: 13c39f0f-fd3c-472d-8324-ddf7538be45e
ms.service: log-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/27/2017
ms.author: bwren
ms.openlocfilehash: 55b66132f7ec5c26c0a7cac1ec0a5c403dbd1082
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="create-an-alert-webhook-action-in-oms-log-analytics-to-send-message-to-slack"></a><span data-ttu-id="6ec0a-104">Skapa en avisering webhook-åtgärd i OMS Log Analytics för att skicka meddelanden till Slack</span><span class="sxs-lookup"><span data-stu-id="6ec0a-104">Create an alert webhook action in OMS Log Analytics to send message to Slack</span></span>
<span data-ttu-id="6ec0a-105">En av åtgärderna som du kan köra som svar på en [logganalys avisering](log-analytics-alerts.md) är en *webhook*, där du kan anropa en extern process via en HTTP-begäran.</span><span class="sxs-lookup"><span data-stu-id="6ec0a-105">One of the actions you can run in response to a [Log Analytics alert](log-analytics-alerts.md) is a *webhook*, which allows you to invoke an external process through a single HTTP request.</span></span>  <span data-ttu-id="6ec0a-106">Du kan läsa om information om aviseringar och webhooks i [aviseringar i logganalys](log-analytics-alerts.md)</span><span class="sxs-lookup"><span data-stu-id="6ec0a-106">You can read about details of alerts and webhooks in [Alerts in Log Analytics](log-analytics-alerts.md)</span></span>

<span data-ttu-id="6ec0a-107">I den här artikeln går vi igenom ett exempel på hur du skapar en webhook-åtgärd i en Log Analytics-avisering med Slack som är en meddelandetjänst.</span><span class="sxs-lookup"><span data-stu-id="6ec0a-107">In this article, we’ll walk through an example of creating a webhook action in a Log Analytics alert using Slack which is a messaging service.</span></span>

> [!NOTE]
> <span data-ttu-id="6ec0a-108">Du måste ha en Slack-konto för att slutföra det här exemplet.</span><span class="sxs-lookup"><span data-stu-id="6ec0a-108">You must have a Slack account to complete this sample.</span></span>  <span data-ttu-id="6ec0a-109">Du kan registrera dig för ett kostnadsfritt konto på [slack.com](http://slack.com).</span><span class="sxs-lookup"><span data-stu-id="6ec0a-109">You can sign up for a free account at [slack.com](http://slack.com).</span></span>
> 
> 

## <a name="step-1---enable-webhooks-in-slack"></a><span data-ttu-id="6ec0a-110">Steg 1 – Aktivera webhooks i Slack</span><span class="sxs-lookup"><span data-stu-id="6ec0a-110">Step 1 - Enable webhooks in Slack</span></span>
1. <span data-ttu-id="6ec0a-111">Logga in till Slack på [slack.com](http://slack.com).</span><span class="sxs-lookup"><span data-stu-id="6ec0a-111">Sign in to Slack at [slack.com](http://slack.com).</span></span>
2. <span data-ttu-id="6ec0a-112">Välj en kanal i den **kanaler** avsnitt i den vänstra rutan.</span><span class="sxs-lookup"><span data-stu-id="6ec0a-112">Select a channel in the **Channels** section in the left pane.</span></span>  <span data-ttu-id="6ec0a-113">Det här är den kanal som meddelandet skickas till.</span><span class="sxs-lookup"><span data-stu-id="6ec0a-113">This is the channel that the message will be sent to.</span></span>  <span data-ttu-id="6ec0a-114">Du kan välja en standard-kanaler som **allmänna** eller **slumpmässiga**.</span><span class="sxs-lookup"><span data-stu-id="6ec0a-114">You can select one of the default channels such as **general** or **random**.</span></span>  <span data-ttu-id="6ec0a-115">I ett produktionsscenario, skulle du förmodligen skapa en särskild kanal som **criticalservicealerts**.</span><span class="sxs-lookup"><span data-stu-id="6ec0a-115">In a production scenario, you would most likely create a special channel such as **criticalservicealerts**.</span></span> <br>
   
   ![Slack-kanaler](media/log-analytics-alerts-webhooks/oms-webhooks01.png)
3. <span data-ttu-id="6ec0a-117">Klicka på **lägga till en app eller en anpassad integrering** att öppna programkatalogen.</span><span class="sxs-lookup"><span data-stu-id="6ec0a-117">Click **Add an app or custom integration** to open the App Directory.</span></span>
4. <span data-ttu-id="6ec0a-118">Typen *webhooks* i sökrutan och väljer sedan **inkommande WebHooks**.</span><span class="sxs-lookup"><span data-stu-id="6ec0a-118">Type *webhooks* into the search box and then select **Incoming WebHooks**.</span></span> <br>
   
   ![Slack-kanaler](media/log-analytics-alerts-webhooks/oms-webhooks02.png)
5. <span data-ttu-id="6ec0a-120">Klicka på **installera** bredvid ditt Teamnamn.</span><span class="sxs-lookup"><span data-stu-id="6ec0a-120">Click **Install** next to your team name.</span></span>
6. <span data-ttu-id="6ec0a-121">Klicka på **lägga till konfigurationen**.</span><span class="sxs-lookup"><span data-stu-id="6ec0a-121">Click **Add Configuration**.</span></span>
7. <span data-ttu-id="6ec0a-122">Välj den kanal som du ska använda för det här exemplet och klicka sedan på **Lägg till inkommande WebHooks integrering**.</span><span class="sxs-lookup"><span data-stu-id="6ec0a-122">Select the the channel that you're going to use for this example, and then click **Add Incoming WebHooks integration**.</span></span>  
8. <span data-ttu-id="6ec0a-123">Kopiera den **Webhooksadressen**.</span><span class="sxs-lookup"><span data-stu-id="6ec0a-123">Copy the **Webhook URL**.</span></span>  <span data-ttu-id="6ec0a-124">Du kommer att klistra in det i aviseringskonfigurationen.</span><span class="sxs-lookup"><span data-stu-id="6ec0a-124">You'll be pasting this into the Alert configuration.</span></span> <br>
   
    ![Slack-kanaler](media/log-analytics-alerts-webhooks/oms-webhooks05.png)

## <a name="step-2---create-alert-rule-in-log-analytics"></a><span data-ttu-id="6ec0a-126">Steg 2 – Skapa varningsregeln i logganalys</span><span class="sxs-lookup"><span data-stu-id="6ec0a-126">Step 2 - Create alert rule in Log Analytics</span></span>
1. <span data-ttu-id="6ec0a-127">[Skapa en aviseringsregel](log-analytics-alerts.md) med följande inställningar.</span><span class="sxs-lookup"><span data-stu-id="6ec0a-127">[Create an alert rule](log-analytics-alerts.md) with the following settings.</span></span>
   * <span data-ttu-id="6ec0a-128">Fråga:```    Type=Event EventLevelName=error ```</span><span class="sxs-lookup"><span data-stu-id="6ec0a-128">Query: ```    Type=Event EventLevelName=error ```</span></span>
   * <span data-ttu-id="6ec0a-129">Sök efter den här aviseringen var: 5 minuter</span><span class="sxs-lookup"><span data-stu-id="6ec0a-129">Check for this alert every: 5 minutes</span></span>
   * <span data-ttu-id="6ec0a-130">Antalet resultat är: större än 10</span><span class="sxs-lookup"><span data-stu-id="6ec0a-130">The number of results is: greater than 10</span></span>
   * <span data-ttu-id="6ec0a-131">Under den här tidsperioden: 60 minuter</span><span class="sxs-lookup"><span data-stu-id="6ec0a-131">Over this time window: 60 minutes</span></span>
   * <span data-ttu-id="6ec0a-132">Välj **Ja** för **Webhook** och **nr** för andra åtgärder.</span><span class="sxs-lookup"><span data-stu-id="6ec0a-132">Select **Yes** for **Webhook** and **No** for the other actions.</span></span>
2. <span data-ttu-id="6ec0a-133">Klistra in Webbadressen till Slack den **Webhooksadressen** fältet.</span><span class="sxs-lookup"><span data-stu-id="6ec0a-133">Paste the Slack URL into the **Webhook URL** field.</span></span>
3. <span data-ttu-id="6ec0a-134">Välj alternativet för att **omfattar en anpassad JSON-nyttolast**.</span><span class="sxs-lookup"><span data-stu-id="6ec0a-134">Select the option to **include a custom JSON payload**.</span></span>
4. <span data-ttu-id="6ec0a-135">Slack förväntar sig en nyttolast som har formaterats i JSON med en parameter med namnet *text*.</span><span class="sxs-lookup"><span data-stu-id="6ec0a-135">Slack expects a payload formatted in JSON with a parameter named *text*.</span></span>  <span data-ttu-id="6ec0a-136">Detta är den text som visas i meddelandet som skapas.</span><span class="sxs-lookup"><span data-stu-id="6ec0a-136">This is the text that it will display in the message it creates.</span></span>  <span data-ttu-id="6ec0a-137">Du kan använda en eller flera av aviseringen parametrar med den  *#*  symbol exempelvis som i följande exempel.</span><span class="sxs-lookup"><span data-stu-id="6ec0a-137">You can use one or more of the alert parameters using the *#* symbol such as in the following example.</span></span>
   
    ```
    {
    "text":"#alertrulename fired with #searchresultcount records which exceeds the over threshold of #thresholdvalue ."
    }
    ```
   
    ![exempel JSON-nyttolast](media/log-analytics-alerts-webhooks/oms-webhooks07.png)
5. <span data-ttu-id="6ec0a-139">Klicka på **spara** att spara varningsregeln.</span><span class="sxs-lookup"><span data-stu-id="6ec0a-139">Click **Save** to save the alert rule.</span></span>
6. <span data-ttu-id="6ec0a-140">Vänta tillräckligt med tid för en avisering ska skapas och sedan kontrollera Slack för ett meddelande som liknar följande.</span><span class="sxs-lookup"><span data-stu-id="6ec0a-140">Wait sufficient time for an alert to be created and then check Slack for a message which will be similar to the following.</span></span>
   
   ![exempel webhook i Slack](media/log-analytics-alerts-webhooks/oms-webhooks08.png)

### <a name="advanced-webhook-payload-for-slack"></a><span data-ttu-id="6ec0a-142">Avancerade webhook nyttolasten för Slack</span><span class="sxs-lookup"><span data-stu-id="6ec0a-142">Advanced webhook payload for Slack</span></span>
<span data-ttu-id="6ec0a-143">Stor utsträckning kan du anpassa inkommande meddelanden med Slack.</span><span class="sxs-lookup"><span data-stu-id="6ec0a-143">You can extensively customize inbound messages with Slack.</span></span> <span data-ttu-id="6ec0a-144">Mer information finns i [inkommande Webhooks](https://api.slack.com/incoming-webhooks) på Slack-webbplatsen.</span><span class="sxs-lookup"><span data-stu-id="6ec0a-144">For more information, see [Incoming Webhooks](https://api.slack.com/incoming-webhooks) on the Slack website.</span></span> <span data-ttu-id="6ec0a-145">Följande är en mer komplex nyttolast för att skapa ett omfattande meddelande med formatering:</span><span class="sxs-lookup"><span data-stu-id="6ec0a-145">Following is a more complex payload to create a rich message with formatting:</span></span>

    {
        "attachments": [
            {
                "title":"OMS Alerts Custom Payload",
                "fields": [
                    {
                        "title": "Alert Rule Name",
                        "value": "#alertrulename"},
                    {
                        "title": "Link To SearchResults",
                        "value": "<#linktosearchresults|OMS Search Results>"},
                    {
                        "title": "Search Interval",
                        "value": "#searchinterval"},
                    {
                        "title": "Threshold Operator",
                        "value": "#thresholdoperator"},
                    {
                        "title": "Threshold Value",
                        "value": "#thresholdvalue"}
                ],
                "color": "#F35A00"
            }
        ]
    }


<span data-ttu-id="6ec0a-146">Detta skulle generera ett meddelande i Slack liknar följande.</span><span class="sxs-lookup"><span data-stu-id="6ec0a-146">This would generate a message in Slack similar to the following.</span></span>

![exempel på meddelande i Slack](media/log-analytics-alerts-webhooks/oms-webhooks09.png)

## <a name="summary"></a><span data-ttu-id="6ec0a-148">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="6ec0a-148">Summary</span></span>
<span data-ttu-id="6ec0a-149">Med den här varningsregeln på plats har du ett meddelande som skickas till Slack varje gång villkoret är uppfyllt.</span><span class="sxs-lookup"><span data-stu-id="6ec0a-149">With this alert rule in place, you would have a message sent to Slack every time the criteria is met.</span></span>  

<span data-ttu-id="6ec0a-150">Detta är endast ett exempel på en åtgärd som du kan skapa som svar på en avisering.</span><span class="sxs-lookup"><span data-stu-id="6ec0a-150">This is only one example of an action that you can create in response to an alert.</span></span>  <span data-ttu-id="6ec0a-151">Du kan skapa en webhook-åtgärd som anropar en annan extern tjänst, en runbook-åtgärden för att starta en runbook i Azure Automation eller en e-åtgärd för att skicka ett e-postmeddelande till dig själv eller andra mottagare.</span><span class="sxs-lookup"><span data-stu-id="6ec0a-151">You could create a webhook action that calls another external service, a runbook action to start a runbook in Azure Automation, or an email action to send a mail to yourself or other recipients.</span></span>   

## <a name="next-steps"></a><span data-ttu-id="6ec0a-152">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="6ec0a-152">Next Steps</span></span>
* <span data-ttu-id="6ec0a-153">Lär dig mer om andra [Varna åtgärder i logganalys](log-analytics-alerts-actions.md) inklusive andra åtgärder.</span><span class="sxs-lookup"><span data-stu-id="6ec0a-153">Learn about other [alert actions in Log Analytics](log-analytics-alerts-actions.md) including other actions.</span></span>


