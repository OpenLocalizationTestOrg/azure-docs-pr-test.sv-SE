---
title: "Svar på aviseringar i OMS Log Analytics | Microsoft Docs"
description: "Aviseringar i Log Analytics kan identifiera viktig information i OMS-databasen och proaktivt meddelar dig om problem eller anropa åtgärder om du vill försöka åtgärda.  Den här artikeln beskriver hur du skapar en aviseringsregel och information om olika åtgärder som de kan ta."
services: log-analytics
documentationcenter: 
author: bwren
manager: jwhit
editor: tysonn
ms.assetid: 6cfd2a46-b6a2-4f79-a67b-08ce488f9a91
ms.service: log-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/28/2017
ms.author: bwren
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: b8731e1fe48b7d809b113eb5273e3962542b8f34
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="add-actions-to-alert-rules-in-log-analytics"></a><span data-ttu-id="4dcf5-104">Lägg till åtgärder i Varningsregler i logganalys</span><span class="sxs-lookup"><span data-stu-id="4dcf5-104">Add actions to alert rules in Log Analytics</span></span>
<span data-ttu-id="4dcf5-105">När en [avisering skapas i logganalys](log-analytics-alerts.md), har möjlighet att [konfigurera varningsregeln](log-analytics-alerts.md) att utföra en eller flera åtgärder.</span><span class="sxs-lookup"><span data-stu-id="4dcf5-105">When an [alert is created in Log Analytics](log-analytics-alerts.md), you have the option of [configuring the alert rule](log-analytics-alerts.md) to perform one or more actions.</span></span>  <span data-ttu-id="4dcf5-106">Den här artikeln beskrivs olika åtgärder som är tillgängliga och information om hur du konfigurerar varje slag.</span><span class="sxs-lookup"><span data-stu-id="4dcf5-106">This article describes the different actions that are available and details on configuring each kind.</span></span>

| <span data-ttu-id="4dcf5-107">Åtgärd</span><span class="sxs-lookup"><span data-stu-id="4dcf5-107">Action</span></span> | <span data-ttu-id="4dcf5-108">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="4dcf5-108">Description</span></span> |
|:--|:--|
| [<span data-ttu-id="4dcf5-109">E-post</span><span class="sxs-lookup"><span data-stu-id="4dcf5-109">Email</span></span>](#email-actions) | <span data-ttu-id="4dcf5-110">Skicka ett e-postmeddelande med information om aviseringen till en eller flera mottagare.</span><span class="sxs-lookup"><span data-stu-id="4dcf5-110">Send an e-mail with the details of the alert to one or more recipients.</span></span> |
| [<span data-ttu-id="4dcf5-111">Webhook</span><span class="sxs-lookup"><span data-stu-id="4dcf5-111">Webhook</span></span>](#webhook-actions) | <span data-ttu-id="4dcf5-112">Anropa en extern process via en enkel HTTP POST-begäran.</span><span class="sxs-lookup"><span data-stu-id="4dcf5-112">Invoke an external process through a single HTTP POST request.</span></span> |
| [<span data-ttu-id="4dcf5-113">Runbook</span><span class="sxs-lookup"><span data-stu-id="4dcf5-113">Runbook</span></span>](#runbook-actions) | <span data-ttu-id="4dcf5-114">Starta en runbook i Azure Automation.</span><span class="sxs-lookup"><span data-stu-id="4dcf5-114">Start a runbook in Azure Automation.</span></span> |


## <a name="email-actions"></a><span data-ttu-id="4dcf5-115">E-post-åtgärder</span><span class="sxs-lookup"><span data-stu-id="4dcf5-115">Email actions</span></span>
<span data-ttu-id="4dcf5-116">E-post-åtgärder skicka ett e-postmeddelande med information om aviseringen till en eller flera mottagare.</span><span class="sxs-lookup"><span data-stu-id="4dcf5-116">Email actions send an e-mail with the details of the alert to one or more recipients.</span></span>  <span data-ttu-id="4dcf5-117">Du kan ange ämnet för e-post, men dess innehåll är ett standardformat genom logganalys.</span><span class="sxs-lookup"><span data-stu-id="4dcf5-117">You can specify the subject of the mail, but it's content is a standard format constructed by Log Analytics.</span></span>  <span data-ttu-id="4dcf5-118">Den innehåller översiktsinformation till exempel namnet för aviseringen förutom information om upp till tio poster som returneras av loggen sökningen.</span><span class="sxs-lookup"><span data-stu-id="4dcf5-118">It includes summary information such as the name of the alert in addition to details of up to ten records returned by the log search.</span></span>  <span data-ttu-id="4dcf5-119">Den innehåller också en länk till en logg sökning i logganalys som att returnera hela uppsättningen av poster från frågan.</span><span class="sxs-lookup"><span data-stu-id="4dcf5-119">It also includes a link to a log search in Log Analytics that will return the entire set of records from that query.</span></span>   <span data-ttu-id="4dcf5-120">Avsändaren av e-postmeddelandet är *Microsoft Operations Management Suite-teamet &lt; noreply@oms.microsoft.com &gt;* .</span><span class="sxs-lookup"><span data-stu-id="4dcf5-120">The sender of the mail is *Microsoft Operations Management Suite Team &lt;noreply@oms.microsoft.com&gt;*.</span></span> 

<span data-ttu-id="4dcf5-121">E-post-åtgärder kräver egenskaperna i följande tabell.</span><span class="sxs-lookup"><span data-stu-id="4dcf5-121">Email actions require the properties in the following table.</span></span>

| <span data-ttu-id="4dcf5-122">Egenskap</span><span class="sxs-lookup"><span data-stu-id="4dcf5-122">Property</span></span> | <span data-ttu-id="4dcf5-123">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="4dcf5-123">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="4dcf5-124">Ämne</span><span class="sxs-lookup"><span data-stu-id="4dcf5-124">Subject</span></span> |<span data-ttu-id="4dcf5-125">Ämne för e-postmeddelandet.</span><span class="sxs-lookup"><span data-stu-id="4dcf5-125">Subject in the email.</span></span>  <span data-ttu-id="4dcf5-126">Du kan inte ändra innehållet i e-postmeddelandet.</span><span class="sxs-lookup"><span data-stu-id="4dcf5-126">You cannot modify the body of the mail.</span></span> |
| <span data-ttu-id="4dcf5-127">mottagare</span><span class="sxs-lookup"><span data-stu-id="4dcf5-127">Recipients</span></span> |<span data-ttu-id="4dcf5-128">Tar med alla e-postmottagare.</span><span class="sxs-lookup"><span data-stu-id="4dcf5-128">Addresses of all e-mail recipients.</span></span>  <span data-ttu-id="4dcf5-129">Om du anger fler än en adress sedan Avgränsa adresserna med semikolon (;).</span><span class="sxs-lookup"><span data-stu-id="4dcf5-129">If you specify more than one address, then separate the addresses with a semicolon (;).</span></span> |


## <a name="webhook-actions"></a><span data-ttu-id="4dcf5-130">Webhook-åtgärder</span><span class="sxs-lookup"><span data-stu-id="4dcf5-130">Webhook actions</span></span>

<span data-ttu-id="4dcf5-131">Webhook-åtgärder kan du anropa en extern process via en enkel HTTP POST-begäran.</span><span class="sxs-lookup"><span data-stu-id="4dcf5-131">Webhook actions allow you to invoke an external process through a single HTTP POST request.</span></span>  <span data-ttu-id="4dcf5-132">Tjänsten som anropas bör stöder webhooks och kontrollera hur den använder alla nyttolast tas emot.</span><span class="sxs-lookup"><span data-stu-id="4dcf5-132">The service being called should support webhooks and determine how it will use any payload it receives.</span></span>  <span data-ttu-id="4dcf5-133">Du kan också kontakta en REST-API som inte uttryckligen stöder webhooks som begäran finns i ett format som kan användas med API: et.</span><span class="sxs-lookup"><span data-stu-id="4dcf5-133">You could also call a REST API that doesn't specifically support webhooks as long as the request is in a format that the API understands.</span></span>  <span data-ttu-id="4dcf5-134">Exempel på användning av en webhook som svar på en avisering skickas ett meddelande [Slack](http://slack.com) eller skapa en incident i [PagerDuty](http://pagerduty.com/).</span><span class="sxs-lookup"><span data-stu-id="4dcf5-134">Examples of using a webhook in response to an alert are sending a message in [Slack](http://slack.com) or creating an incident in [PagerDuty](http://pagerduty.com/).</span></span>  <span data-ttu-id="4dcf5-135">En fullständig genomgång för att skapa en aviseringsregel med en webhook att anropa Slack finns på [Webhooks i logganalys aviseringar](log-analytics-alerts-webhooks.md).</span><span class="sxs-lookup"><span data-stu-id="4dcf5-135">A complete walkthrough of creating an alert rule with a webhook to call Slack is available at [Webhooks in Log Analytics alerts](log-analytics-alerts-webhooks.md).</span></span>

<span data-ttu-id="4dcf5-136">Webhook-åtgärder kräver egenskaperna i följande tabell.</span><span class="sxs-lookup"><span data-stu-id="4dcf5-136">Webhook actions require the properties in the following table.</span></span>

| <span data-ttu-id="4dcf5-137">Egenskap</span><span class="sxs-lookup"><span data-stu-id="4dcf5-137">Property</span></span> | <span data-ttu-id="4dcf5-138">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="4dcf5-138">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="4dcf5-139">Webhooksadressen</span><span class="sxs-lookup"><span data-stu-id="4dcf5-139">Webhook URL</span></span> |<span data-ttu-id="4dcf5-140">URL till webhooken.</span><span class="sxs-lookup"><span data-stu-id="4dcf5-140">The URL of the webhook.</span></span> |
| <span data-ttu-id="4dcf5-141">Anpassad JSON-nyttolast</span><span class="sxs-lookup"><span data-stu-id="4dcf5-141">Custom JSON payload</span></span> |<span data-ttu-id="4dcf5-142">Anpassad nyttolast för att skicka med webhooken.</span><span class="sxs-lookup"><span data-stu-id="4dcf5-142">Custom payload to send with the webhook.</span></span>  <span data-ttu-id="4dcf5-143">Mer information finns i nedan.</span><span class="sxs-lookup"><span data-stu-id="4dcf5-143">See below for details.</span></span> |


<span data-ttu-id="4dcf5-144">Webhooks är en URL och en nyttolast som har formaterats i JSON som är data som skickas till externa-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="4dcf5-144">Webhooks include a URL and a payload formatted in JSON that is the data sent to the external service.</span></span>  <span data-ttu-id="4dcf5-145">Som standard innehåller nyttolasten värdena i tabellen nedan.</span><span class="sxs-lookup"><span data-stu-id="4dcf5-145">By default, the payload will include the values in the following table.</span></span>  <span data-ttu-id="4dcf5-146">Du kan välja att ersätta den här nyttolasten med ett anpassat egen.</span><span class="sxs-lookup"><span data-stu-id="4dcf5-146">You can choose to replace this payload with a custom one of your own.</span></span>  <span data-ttu-id="4dcf5-147">Du kan i så fall använda variabler i tabellen för var och en av parametrarna ta sina värdet i din anpassade nyttolast.</span><span class="sxs-lookup"><span data-stu-id="4dcf5-147">In that case you can use the variables in the table for each of the parameters to include their value in your custom payload.</span></span>

| <span data-ttu-id="4dcf5-148">Parameter</span><span class="sxs-lookup"><span data-stu-id="4dcf5-148">Parameter</span></span> | <span data-ttu-id="4dcf5-149">Variabel</span><span class="sxs-lookup"><span data-stu-id="4dcf5-149">Variable</span></span> | <span data-ttu-id="4dcf5-150">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="4dcf5-150">Description</span></span> |
|:--- |:--- |:--- |
| <span data-ttu-id="4dcf5-151">AlertRuleName</span><span class="sxs-lookup"><span data-stu-id="4dcf5-151">AlertRuleName</span></span> |<span data-ttu-id="4dcf5-152">#alertrulename</span><span class="sxs-lookup"><span data-stu-id="4dcf5-152">#alertrulename</span></span> |<span data-ttu-id="4dcf5-153">Namnet på regeln.</span><span class="sxs-lookup"><span data-stu-id="4dcf5-153">Name of the alert rule.</span></span> |
| <span data-ttu-id="4dcf5-154">AlertThresholdOperator</span><span class="sxs-lookup"><span data-stu-id="4dcf5-154">AlertThresholdOperator</span></span> |<span data-ttu-id="4dcf5-155">#thresholdoperator</span><span class="sxs-lookup"><span data-stu-id="4dcf5-155">#thresholdoperator</span></span> |<span data-ttu-id="4dcf5-156">Tröskelvärdet operator för regeln.</span><span class="sxs-lookup"><span data-stu-id="4dcf5-156">Threshold operator for the alert rule.</span></span>  <span data-ttu-id="4dcf5-157">*Större än* eller *mindre än*.</span><span class="sxs-lookup"><span data-stu-id="4dcf5-157">*Greater than* or *Less than*.</span></span> |
| <span data-ttu-id="4dcf5-158">AlertThresholdValue</span><span class="sxs-lookup"><span data-stu-id="4dcf5-158">AlertThresholdValue</span></span> |<span data-ttu-id="4dcf5-159">#thresholdvalue</span><span class="sxs-lookup"><span data-stu-id="4dcf5-159">#thresholdvalue</span></span> |<span data-ttu-id="4dcf5-160">Tröskelvärde för regeln.</span><span class="sxs-lookup"><span data-stu-id="4dcf5-160">Threshold value for the alert rule.</span></span> |
| <span data-ttu-id="4dcf5-161">LinkToSearchResults</span><span class="sxs-lookup"><span data-stu-id="4dcf5-161">LinkToSearchResults</span></span> |<span data-ttu-id="4dcf5-162">#linktosearchresults</span><span class="sxs-lookup"><span data-stu-id="4dcf5-162">#linktosearchresults</span></span> |<span data-ttu-id="4dcf5-163">Länka till logganalys loggen sökning som returnerar poster från frågan som skapade aviseringen.</span><span class="sxs-lookup"><span data-stu-id="4dcf5-163">Link to Log Analytics log search that returns the records from the query that created the alert.</span></span> |
| <span data-ttu-id="4dcf5-164">ResultCount</span><span class="sxs-lookup"><span data-stu-id="4dcf5-164">ResultCount</span></span> |<span data-ttu-id="4dcf5-165">#searchresultcount</span><span class="sxs-lookup"><span data-stu-id="4dcf5-165">#searchresultcount</span></span> |<span data-ttu-id="4dcf5-166">Antalet poster i sökresultaten.</span><span class="sxs-lookup"><span data-stu-id="4dcf5-166">Number of records in the search results.</span></span> |
| <span data-ttu-id="4dcf5-167">SearchIntervalEndtimeUtc</span><span class="sxs-lookup"><span data-stu-id="4dcf5-167">SearchIntervalEndtimeUtc</span></span> |<span data-ttu-id="4dcf5-168">#searchintervalendtimeutc</span><span class="sxs-lookup"><span data-stu-id="4dcf5-168">#searchintervalendtimeutc</span></span> |<span data-ttu-id="4dcf5-169">Sluttid för frågan i UTC-format.</span><span class="sxs-lookup"><span data-stu-id="4dcf5-169">End time for the query in UTC format.</span></span> |
| <span data-ttu-id="4dcf5-170">SearchIntervalInSeconds</span><span class="sxs-lookup"><span data-stu-id="4dcf5-170">SearchIntervalInSeconds</span></span> |<span data-ttu-id="4dcf5-171">#searchinterval</span><span class="sxs-lookup"><span data-stu-id="4dcf5-171">#searchinterval</span></span> |<span data-ttu-id="4dcf5-172">Tidsfönstret för regeln.</span><span class="sxs-lookup"><span data-stu-id="4dcf5-172">Time window for the alert rule.</span></span> |
| <span data-ttu-id="4dcf5-173">SearchIntervalStartTimeUtc</span><span class="sxs-lookup"><span data-stu-id="4dcf5-173">SearchIntervalStartTimeUtc</span></span> |<span data-ttu-id="4dcf5-174">#searchintervalstarttimeutc</span><span class="sxs-lookup"><span data-stu-id="4dcf5-174">#searchintervalstarttimeutc</span></span> |<span data-ttu-id="4dcf5-175">Starttid för frågan i UTC-format.</span><span class="sxs-lookup"><span data-stu-id="4dcf5-175">Start time for the query in UTC format.</span></span> |
| <span data-ttu-id="4dcf5-176">searchQuery</span><span class="sxs-lookup"><span data-stu-id="4dcf5-176">SearchQuery</span></span> |<span data-ttu-id="4dcf5-177">#searchquery</span><span class="sxs-lookup"><span data-stu-id="4dcf5-177">#searchquery</span></span> |<span data-ttu-id="4dcf5-178">Loggen sökfråga används av regeln.</span><span class="sxs-lookup"><span data-stu-id="4dcf5-178">Log search query used by the alert rule.</span></span> |
| <span data-ttu-id="4dcf5-179">SearchResults</span><span class="sxs-lookup"><span data-stu-id="4dcf5-179">SearchResults</span></span> |<span data-ttu-id="4dcf5-180">Nedan finns</span><span class="sxs-lookup"><span data-stu-id="4dcf5-180">See below</span></span> |<span data-ttu-id="4dcf5-181">Poster som returneras av frågan i JSON-format.</span><span class="sxs-lookup"><span data-stu-id="4dcf5-181">Records returned by the query in JSON format.</span></span>  <span data-ttu-id="4dcf5-182">Begränsad till de första 5 000 posterna.</span><span class="sxs-lookup"><span data-stu-id="4dcf5-182">Limited to the first 5,000 records.</span></span> |
| <span data-ttu-id="4dcf5-183">WorkspaceID</span><span class="sxs-lookup"><span data-stu-id="4dcf5-183">WorkspaceID</span></span> |<span data-ttu-id="4dcf5-184">#workspaceid</span><span class="sxs-lookup"><span data-stu-id="4dcf5-184">#workspaceid</span></span> |<span data-ttu-id="4dcf5-185">ID för din OMS-arbetsyta.</span><span class="sxs-lookup"><span data-stu-id="4dcf5-185">ID of your OMS workspace.</span></span> |

<span data-ttu-id="4dcf5-186">Du kan till exempel ange följande anpassad nyttolast som innehåller en enda parameter med namnet *text*.</span><span class="sxs-lookup"><span data-stu-id="4dcf5-186">For example, you might specify the following custom payload that includes a single parameter called *text*.</span></span>  <span data-ttu-id="4dcf5-187">Den tjänst som denna webhook anropar skulle förväntas den här parametern.</span><span class="sxs-lookup"><span data-stu-id="4dcf5-187">The service that this webhook calls would be expecting this parameter.</span></span>

    {
        "text":"#alertrulename fired with #searchresultcount over threshold of #thresholdvalue."
    }

<span data-ttu-id="4dcf5-188">Nyttolasten i det här exemplet skulle matchas till något som liknar följande när du skickar webhooken.</span><span class="sxs-lookup"><span data-stu-id="4dcf5-188">This example payload would resolve to something like the following when sent to the webhook.</span></span>

    {
        "text":"My Alert Rule fired with 18 records over threshold of 10 ."
    }

<span data-ttu-id="4dcf5-189">Inkludera sökresultat i en anpassad nyttolast genom att lägga till följande rad som en egenskap för översta nivån i json-nyttolast.</span><span class="sxs-lookup"><span data-stu-id="4dcf5-189">To include search results in a custom payload, add the following line as a top level property in the json payload.</span></span>  

    "IncludeSearchResults":true

<span data-ttu-id="4dcf5-190">Om du vill skapa en anpassad nyttolast som innehåller bara aviseringsnamn och sökresultaten kan du exempelvis använda följande.</span><span class="sxs-lookup"><span data-stu-id="4dcf5-190">For example, to create a custom payload that includes just the alert name and the search results, you could use the following.</span></span> 

    {
       "alertname":"#alertrulename",
       "IncludeSearchResults":true
    }


<span data-ttu-id="4dcf5-191">Du kan gå igenom en komplett exempel på hur du skapar en aviseringsregel med en webhook att starta en extern tjänst på [skapar en avisering webhook-åtgärd i OMS Log Analytics för att skicka meddelanden till Slack](log-analytics-alerts-webhooks.md).</span><span class="sxs-lookup"><span data-stu-id="4dcf5-191">You can walk through a complete example of creating an alert rule with a webhook to start an external service at [Create an alert webhook action in OMS Log Analytics to send message to Slack](log-analytics-alerts-webhooks.md).</span></span>

## <a name="runbook-actions"></a><span data-ttu-id="4dcf5-192">Runbook-åtgärder</span><span class="sxs-lookup"><span data-stu-id="4dcf5-192">Runbook actions</span></span>
<span data-ttu-id="4dcf5-193">Runbook-åtgärder startar en runbook i Azure Automation.</span><span class="sxs-lookup"><span data-stu-id="4dcf5-193">Runbook actions start a runbook in Azure Automation.</span></span>  <span data-ttu-id="4dcf5-194">För att kunna använda den här typen av åtgärden, måste du ha den [automatiseringslösning](log-analytics-add-solutions.md) installeras och konfigureras i OMS-arbetsyta.</span><span class="sxs-lookup"><span data-stu-id="4dcf5-194">In order to use this type of action, you must have the [Automation solution](log-analytics-add-solutions.md) installed and configured in your OMS workspace.</span></span>  <span data-ttu-id="4dcf5-195">Du kan välja från runbooks i automation-kontot som du konfigurerade i Automation-lösningen.</span><span class="sxs-lookup"><span data-stu-id="4dcf5-195">You can select from the runbooks in the automation account that you configured in the Automation solution.</span></span>

<span data-ttu-id="4dcf5-196">Runbook-åtgärder kräver egenskaperna i följande tabell.</span><span class="sxs-lookup"><span data-stu-id="4dcf5-196">Runbook actions require the properties in the following table.</span></span>

| <span data-ttu-id="4dcf5-197">Egenskap</span><span class="sxs-lookup"><span data-stu-id="4dcf5-197">Property</span></span> | <span data-ttu-id="4dcf5-198">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="4dcf5-198">Description</span></span> |
|:--- |:---|
| <span data-ttu-id="4dcf5-199">Runbook</span><span class="sxs-lookup"><span data-stu-id="4dcf5-199">Runbook</span></span> | <span data-ttu-id="4dcf5-200">Runbook som du vill starta när en avisering skapas.</span><span class="sxs-lookup"><span data-stu-id="4dcf5-200">Runbook that you want to start when an alert is created.</span></span> |
| <span data-ttu-id="4dcf5-201">Kör på</span><span class="sxs-lookup"><span data-stu-id="4dcf5-201">Run on</span></span> | <span data-ttu-id="4dcf5-202">Ange **Azure** att köra runbook i molnet.</span><span class="sxs-lookup"><span data-stu-id="4dcf5-202">Specify **Azure** to run the runbook in the cloud.</span></span>  <span data-ttu-id="4dcf5-203">Ange **Hybrid worker** att köra runbook på en agent med [Hybrid Runbook Worker](../automation/automation-hybrid-runbook-worker.md ) installerad.</span><span class="sxs-lookup"><span data-stu-id="4dcf5-203">Specify **Hybrid worker** to run the runbook on an agent with [Hybrid Runbook Worker](../automation/automation-hybrid-runbook-worker.md ) installed.</span></span>  |

<span data-ttu-id="4dcf5-204">Runbook-åtgärder starta en runbook med hjälp av en [webhook](../automation/automation-webhooks.md).</span><span class="sxs-lookup"><span data-stu-id="4dcf5-204">Runbook actions start the runbook using a [webhook](../automation/automation-webhooks.md).</span></span>  <span data-ttu-id="4dcf5-205">När du skapar varningsregeln automatiskt skapas en ny webhook för runbook med namnet **OMS avisering reparation** följt av ett GUID.</span><span class="sxs-lookup"><span data-stu-id="4dcf5-205">When you create the alert rule, it will automatically create a new webhook for the runbook with the name **OMS Alert Remediation** followed by a GUID.</span></span>  

<span data-ttu-id="4dcf5-206">Direkt kan du fylla i parametrar av runbook, men [$WebhookData parametern](../automation/automation-webhooks.md) innehåller information om aviseringen, inklusive resultaten av den logg som den skapades.</span><span class="sxs-lookup"><span data-stu-id="4dcf5-206">You cannot directly populate any parameters of the runbook, but the [$WebhookData parameter](../automation/automation-webhooks.md) will include the details of the alert, including the results of the log search that created it.</span></span>  <span data-ttu-id="4dcf5-207">Runbook måste du definiera **$WebhookData** som en parameter att komma åt egenskaper för aviseringen.</span><span class="sxs-lookup"><span data-stu-id="4dcf5-207">The runbook will need to define **$WebhookData** as a parameter for it to access the properties of the alert.</span></span>  <span data-ttu-id="4dcf5-208">Aviseringen data är tillgängliga i json-format i en enda egenskap som kallas **SearchResults** i den **RequestBody** -egenskapen för **$WebhookData**.</span><span class="sxs-lookup"><span data-stu-id="4dcf5-208">The alert data is available in json format in a single property called **SearchResults** in the **RequestBody** property of **$WebhookData**.</span></span>  <span data-ttu-id="4dcf5-209">Detta har med egenskaper i följande tabell.</span><span class="sxs-lookup"><span data-stu-id="4dcf5-209">This will have with the properties in the following table.</span></span>

| <span data-ttu-id="4dcf5-210">Node</span><span class="sxs-lookup"><span data-stu-id="4dcf5-210">Node</span></span> | <span data-ttu-id="4dcf5-211">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="4dcf5-211">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="4dcf5-212">id</span><span class="sxs-lookup"><span data-stu-id="4dcf5-212">id</span></span> |<span data-ttu-id="4dcf5-213">Sökvägen och GUID för sökningen.</span><span class="sxs-lookup"><span data-stu-id="4dcf5-213">Path and GUID of the search.</span></span> |
| <span data-ttu-id="4dcf5-214">__metadata</span><span class="sxs-lookup"><span data-stu-id="4dcf5-214">__metadata</span></span> |<span data-ttu-id="4dcf5-215">Information om aviseringen, inklusive antalet poster och status i sökresultaten.</span><span class="sxs-lookup"><span data-stu-id="4dcf5-215">Information about the alert including the number of records and status of the search results.</span></span> |
| <span data-ttu-id="4dcf5-216">värde</span><span class="sxs-lookup"><span data-stu-id="4dcf5-216">value</span></span> |<span data-ttu-id="4dcf5-217">Separat post för varje post i sökresultatet.</span><span class="sxs-lookup"><span data-stu-id="4dcf5-217">Separate entry for each record in the search results.</span></span>  <span data-ttu-id="4dcf5-218">Information om transaktionen matchar egenskaperna och värdena för posten.</span><span class="sxs-lookup"><span data-stu-id="4dcf5-218">The details of the entry will match the properties and values of the record.</span></span> |

<span data-ttu-id="4dcf5-219">Följande runbook skulle exempelvis extrahera de poster som returneras av loggen sökningen och tilldela olika egenskaper baserat på vilken typ av varje post.</span><span class="sxs-lookup"><span data-stu-id="4dcf5-219">For example, the following runbook would extract the records returned by the log search  and assign different properties based on the type of each record.</span></span>  <span data-ttu-id="4dcf5-220">Observera att startar runbook genom att konvertera **RequestBody** från json så att den kan bearbetas med som ett objekt i PowerShell.</span><span class="sxs-lookup"><span data-stu-id="4dcf5-220">Note that the runbook starts by converting **RequestBody** from json so that it can be worked with as an object in PowerShell.</span></span>

    param ( 
        [object]$WebhookData
    )

    $RequestBody = ConvertFrom-JSON -InputObject $WebhookData.RequestBody
    $Records     = $RequestBody.SearchResults.value

    foreach ($Record in $Records)
    {
        $Computer = $Record.Computer

        if ($Record.Type -eq 'Event')
        {
            $EventNo    = $Record.EventID
            $EventLevel = $Record.EventLevelName
            $EventData  = $Record.EventData
        }

        if ($Record.Type -eq 'Perf')
        {
            $Object    = $Record.ObjectName
            $Counter   = $Record.CounterName
            $Instance  = $Record.InstanceName
            $Value     = $Record.CounterValue
        }
    }


## <a name="next-steps"></a><span data-ttu-id="4dcf5-221">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="4dcf5-221">Next steps</span></span>
- <span data-ttu-id="4dcf5-222">Slutföra en genomgång för [konfigurerar en webook](log-analytics-alerts-webhooks.md) med en aviseringsregel.</span><span class="sxs-lookup"><span data-stu-id="4dcf5-222">Complete a walkthrough for [configuring a webook](log-analytics-alerts-webhooks.md) with an alert rule.</span></span>  
- <span data-ttu-id="4dcf5-223">Lär dig hur du skriver [runbooks i Azure Automation](https://azure.microsoft.com/documentation/services/automation) att åtgärda problem som identifieras av aviseringar.</span><span class="sxs-lookup"><span data-stu-id="4dcf5-223">Learn how to write [runbooks in Azure Automation](https://azure.microsoft.com/documentation/services/automation) to remediate problems identified by alerts.</span></span>