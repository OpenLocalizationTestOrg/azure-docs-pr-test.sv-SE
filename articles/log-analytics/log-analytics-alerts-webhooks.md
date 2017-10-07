---
title: "aaaWebhook Aviseringsåtgärd provet i OMS Log Analytics | Microsoft Docs"
description: "En av hello åtgärder du kan köra i svaret tooa logganalys aviseringen är en * webhook *, vilket gör att du tooinvoke en extern process via en HTTP-begäran. Den här artikeln beskriver hur ett exempel på hur du skapar en webhook-åtgärd i en Log Analytics-avisering med Slack."
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
ms.openlocfilehash: e60bdc4922347073d572c2e4719461b13e8e7d1c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-alert-webhook-action-in-oms-log-analytics-toosend-message-tooslack"></a><span data-ttu-id="cb990-104">Skapa en avisering webhook-åtgärd i OMS logganalys toosend meddelandet tooSlack</span><span class="sxs-lookup"><span data-stu-id="cb990-104">Create an alert webhook action in OMS Log Analytics toosend message tooSlack</span></span>
<span data-ttu-id="cb990-105">En av hello åtgärder du kan köra i svaret tooa [logganalys avisering](log-analytics-alerts.md) är en *webhook*, vilket gör att du tooinvoke en extern process via en HTTP-begäran.</span><span class="sxs-lookup"><span data-stu-id="cb990-105">One of hello actions you can run in response tooa [Log Analytics alert](log-analytics-alerts.md) is a *webhook*, which allows you tooinvoke an external process through a single HTTP request.</span></span>  <span data-ttu-id="cb990-106">Du kan läsa om information om aviseringar och webhooks i [aviseringar i logganalys](log-analytics-alerts.md)</span><span class="sxs-lookup"><span data-stu-id="cb990-106">You can read about details of alerts and webhooks in [Alerts in Log Analytics](log-analytics-alerts.md)</span></span>

<span data-ttu-id="cb990-107">I den här artikeln går vi igenom ett exempel på hur du skapar en webhook-åtgärd i en Log Analytics-avisering med Slack som är en meddelandetjänst.</span><span class="sxs-lookup"><span data-stu-id="cb990-107">In this article, we’ll walk through an example of creating a webhook action in a Log Analytics alert using Slack which is a messaging service.</span></span>

> [!NOTE]
> <span data-ttu-id="cb990-108">Du måste ha en Slack konto toocomplete det här exemplet.</span><span class="sxs-lookup"><span data-stu-id="cb990-108">You must have a Slack account toocomplete this sample.</span></span>  <span data-ttu-id="cb990-109">Du kan registrera dig för ett kostnadsfritt konto på [slack.com](http://slack.com).</span><span class="sxs-lookup"><span data-stu-id="cb990-109">You can sign up for a free account at [slack.com](http://slack.com).</span></span>
> 
> 

## <a name="step-1---enable-webhooks-in-slack"></a><span data-ttu-id="cb990-110">Steg 1 – Aktivera webhooks i Slack</span><span class="sxs-lookup"><span data-stu-id="cb990-110">Step 1 - Enable webhooks in Slack</span></span>
1. <span data-ttu-id="cb990-111">Logga in tooSlack på [slack.com](http://slack.com).</span><span class="sxs-lookup"><span data-stu-id="cb990-111">Sign in tooSlack at [slack.com](http://slack.com).</span></span>
2. <span data-ttu-id="cb990-112">Välj en kanal i hello **kanaler** avsnitt i hello till vänster.</span><span class="sxs-lookup"><span data-stu-id="cb990-112">Select a channel in hello **Channels** section in hello left pane.</span></span>  <span data-ttu-id="cb990-113">Detta är hello kanal som hello-meddelande skickas till.</span><span class="sxs-lookup"><span data-stu-id="cb990-113">This is hello channel that hello message will be sent to.</span></span>  <span data-ttu-id="cb990-114">Du kan välja något av hello standard kanaler som **allmänna** eller **slumpmässiga**.</span><span class="sxs-lookup"><span data-stu-id="cb990-114">You can select one of hello default channels such as **general** or **random**.</span></span>  <span data-ttu-id="cb990-115">I ett produktionsscenario, skulle du förmodligen skapa en särskild kanal som **criticalservicealerts**.</span><span class="sxs-lookup"><span data-stu-id="cb990-115">In a production scenario, you would most likely create a special channel such as **criticalservicealerts**.</span></span> <br>
   
   ![Slack-kanaler](media/log-analytics-alerts-webhooks/oms-webhooks01.png)
3. <span data-ttu-id="cb990-117">Klicka på **lägga till en app eller en anpassad integrering** tooopen hello programkatalogen.</span><span class="sxs-lookup"><span data-stu-id="cb990-117">Click **Add an app or custom integration** tooopen hello App Directory.</span></span>
4. <span data-ttu-id="cb990-118">Typen *webhooks* till hello sökrutan och väljer sedan **inkommande WebHooks**.</span><span class="sxs-lookup"><span data-stu-id="cb990-118">Type *webhooks* into hello search box and then select **Incoming WebHooks**.</span></span> <br>
   
   ![Slack-kanaler](media/log-analytics-alerts-webhooks/oms-webhooks02.png)
5. <span data-ttu-id="cb990-120">Klicka på **installera** nästa tooyour Teamnamn.</span><span class="sxs-lookup"><span data-stu-id="cb990-120">Click **Install** next tooyour team name.</span></span>
6. <span data-ttu-id="cb990-121">Klicka på **lägga till konfigurationen**.</span><span class="sxs-lookup"><span data-stu-id="cb990-121">Click **Add Configuration**.</span></span>
7. <span data-ttu-id="cb990-122">Välj hello hello kanal du kommer toouse för det här exemplet och klicka sedan på **Lägg till inkommande WebHooks integrering**.</span><span class="sxs-lookup"><span data-stu-id="cb990-122">Select hello hello channel that you're going toouse for this example, and then click **Add Incoming WebHooks integration**.</span></span>  
8. <span data-ttu-id="cb990-123">Kopiera hello **Webhooksadressen**.</span><span class="sxs-lookup"><span data-stu-id="cb990-123">Copy hello **Webhook URL**.</span></span>  <span data-ttu-id="cb990-124">Du kommer att klistra in det i hello varningskonfigurationen.</span><span class="sxs-lookup"><span data-stu-id="cb990-124">You'll be pasting this into hello Alert configuration.</span></span> <br>
   
    ![Slack-kanaler](media/log-analytics-alerts-webhooks/oms-webhooks05.png)

## <a name="step-2---create-alert-rule-in-log-analytics"></a><span data-ttu-id="cb990-126">Steg 2 – Skapa varningsregeln i logganalys</span><span class="sxs-lookup"><span data-stu-id="cb990-126">Step 2 - Create alert rule in Log Analytics</span></span>
1. <span data-ttu-id="cb990-127">[Skapa en aviseringsregel](log-analytics-alerts.md) med hello följande inställningar.</span><span class="sxs-lookup"><span data-stu-id="cb990-127">[Create an alert rule](log-analytics-alerts.md) with hello following settings.</span></span>
   * <span data-ttu-id="cb990-128">Fråga:```    Type=Event EventLevelName=error ```</span><span class="sxs-lookup"><span data-stu-id="cb990-128">Query: ```    Type=Event EventLevelName=error ```</span></span>
   * <span data-ttu-id="cb990-129">Sök efter den här aviseringen var: 5 minuter</span><span class="sxs-lookup"><span data-stu-id="cb990-129">Check for this alert every: 5 minutes</span></span>
   * <span data-ttu-id="cb990-130">hello antalet resultat är: större än 10</span><span class="sxs-lookup"><span data-stu-id="cb990-130">hello number of results is: greater than 10</span></span>
   * <span data-ttu-id="cb990-131">Under den här tidsperioden: 60 minuter</span><span class="sxs-lookup"><span data-stu-id="cb990-131">Over this time window: 60 minutes</span></span>
   * <span data-ttu-id="cb990-132">Välj **Ja** för **Webhook** och **nr** för hello andra åtgärder.</span><span class="sxs-lookup"><span data-stu-id="cb990-132">Select **Yes** for **Webhook** and **No** for hello other actions.</span></span>
2. <span data-ttu-id="cb990-133">Klistra in hello Slack URL till hello **Webhooksadressen** fältet.</span><span class="sxs-lookup"><span data-stu-id="cb990-133">Paste hello Slack URL into hello **Webhook URL** field.</span></span>
3. <span data-ttu-id="cb990-134">Välj alternativet för hello för**omfattar en anpassad JSON-nyttolast**.</span><span class="sxs-lookup"><span data-stu-id="cb990-134">Select hello option too**include a custom JSON payload**.</span></span>
4. <span data-ttu-id="cb990-135">Slack förväntar sig en nyttolast som har formaterats i JSON med en parameter med namnet *text*.</span><span class="sxs-lookup"><span data-stu-id="cb990-135">Slack expects a payload formatted in JSON with a parameter named *text*.</span></span>  <span data-ttu-id="cb990-136">Detta är hello text som visas i hello-meddelande skapas.</span><span class="sxs-lookup"><span data-stu-id="cb990-136">This is hello text that it will display in hello message it creates.</span></span>  <span data-ttu-id="cb990-137">Du kan använda en eller flera av hello avisering parametrar med hello  *#*  symbol exempelvis som i följande exempel hello.</span><span class="sxs-lookup"><span data-stu-id="cb990-137">You can use one or more of hello alert parameters using hello *#* symbol such as in hello following example.</span></span>
   
    ```
    {
    "text":"#alertrulename fired with #searchresultcount records which exceeds hello over threshold of #thresholdvalue ."
    }
    ```
   
    ![exempel JSON-nyttolast](media/log-analytics-alerts-webhooks/oms-webhooks07.png)
5. <span data-ttu-id="cb990-139">Klicka på **spara** toosave hello varningsregel.</span><span class="sxs-lookup"><span data-stu-id="cb990-139">Click **Save** toosave hello alert rule.</span></span>
6. <span data-ttu-id="cb990-140">Vänta tillräckligt med tid för en avisering toobe skapas och kontrollera Slack för ett meddelande som kommer att vara liknande toohello följande.</span><span class="sxs-lookup"><span data-stu-id="cb990-140">Wait sufficient time for an alert toobe created and then check Slack for a message which will be similar toohello following.</span></span>
   
   ![exempel webhook i Slack](media/log-analytics-alerts-webhooks/oms-webhooks08.png)

### <a name="advanced-webhook-payload-for-slack"></a><span data-ttu-id="cb990-142">Avancerade webhook nyttolasten för Slack</span><span class="sxs-lookup"><span data-stu-id="cb990-142">Advanced webhook payload for Slack</span></span>
<span data-ttu-id="cb990-143">Stor utsträckning kan du anpassa inkommande meddelanden med Slack.</span><span class="sxs-lookup"><span data-stu-id="cb990-143">You can extensively customize inbound messages with Slack.</span></span> <span data-ttu-id="cb990-144">Mer information finns i [inkommande Webhooks](https://api.slack.com/incoming-webhooks) på hello Slack-webbplatsen.</span><span class="sxs-lookup"><span data-stu-id="cb990-144">For more information, see [Incoming Webhooks](https://api.slack.com/incoming-webhooks) on hello Slack website.</span></span> <span data-ttu-id="cb990-145">Följande är en mer komplex nyttolast toocreate ett omfattande meddelande med formatering:</span><span class="sxs-lookup"><span data-stu-id="cb990-145">Following is a more complex payload toocreate a rich message with formatting:</span></span>

    {
        "attachments": [
            {
                "title":"OMS Alerts Custom Payload",
                "fields": [
                    {
                        "title": "Alert Rule Name",
                        "value": "#alertrulename"},
                    {
                        "title": "Link tooSearchResults",
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


<span data-ttu-id="cb990-146">Detta skulle generera ett meddelande i Slack liknande toohello följande.</span><span class="sxs-lookup"><span data-stu-id="cb990-146">This would generate a message in Slack similar toohello following.</span></span>

![exempel på meddelande i Slack](media/log-analytics-alerts-webhooks/oms-webhooks09.png)

## <a name="summary"></a><span data-ttu-id="cb990-148">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="cb990-148">Summary</span></span>
<span data-ttu-id="cb990-149">Med den här varningsregeln på plats har du ett meddelande som skickas tooSlack varje gång hello villkoret är uppfyllt.</span><span class="sxs-lookup"><span data-stu-id="cb990-149">With this alert rule in place, you would have a message sent tooSlack every time hello criteria is met.</span></span>  

<span data-ttu-id="cb990-150">Detta är bara ett exempel på en åtgärd som du kan skapa i svaret tooan avisering.</span><span class="sxs-lookup"><span data-stu-id="cb990-150">This is only one example of an action that you can create in response tooan alert.</span></span>  <span data-ttu-id="cb990-151">Du kan skapa en webhook-åtgärd som anropar en annan extern tjänst, en runbook åtgärd toostart en runbook i Azure Automation eller en e-åtgärd toosend tooyourself en e-post eller andra mottagare.</span><span class="sxs-lookup"><span data-stu-id="cb990-151">You could create a webhook action that calls another external service, a runbook action toostart a runbook in Azure Automation, or an email action toosend a mail tooyourself or other recipients.</span></span>   

## <a name="next-steps"></a><span data-ttu-id="cb990-152">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="cb990-152">Next Steps</span></span>
* <span data-ttu-id="cb990-153">Lär dig mer om andra [Varna åtgärder i logganalys](log-analytics-alerts-actions.md) inklusive andra åtgärder.</span><span class="sxs-lookup"><span data-stu-id="cb990-153">Learn about other [alert actions in Log Analytics](log-analytics-alerts-actions.md) including other actions.</span></span>


