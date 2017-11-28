---
title: aaaResponses tooalerts i OMS Log Analytics | Microsoft Docs
description: "Aviseringar i Log Analytics identifiera viktig information i OMS-databasen och kan proaktivt meddelar dig om problem eller anropa åtgärder tooattempt toocorrect dem.  Den här artikeln beskriver hur toocreate en aviseringsregel och information hello olika åtgärder de kan vidta."
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
ms.openlocfilehash: d24bb726a96e7143985f111c0599dc4e7898b4f0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="add-actions-tooalert-rules-in-log-analytics"></a><span data-ttu-id="fc927-104">Lägg till åtgärder tooalert regler i logganalys</span><span class="sxs-lookup"><span data-stu-id="fc927-104">Add actions tooalert rules in Log Analytics</span></span>
<span data-ttu-id="fc927-105">När en [avisering skapas i logganalys](log-analytics-alerts.md), har hello möjlighet att [varningsregel för att konfigurera hello](log-analytics-alerts.md) tooperform en eller flera åtgärder.</span><span class="sxs-lookup"><span data-stu-id="fc927-105">When an [alert is created in Log Analytics](log-analytics-alerts.md), you have hello option of [configuring hello alert rule](log-analytics-alerts.md) tooperform one or more actions.</span></span>  <span data-ttu-id="fc927-106">Den här artikeln beskriver hello olika åtgärder som är tillgängliga och information om hur du konfigurerar varje slag.</span><span class="sxs-lookup"><span data-stu-id="fc927-106">This article describes hello different actions that are available and details on configuring each kind.</span></span>

| <span data-ttu-id="fc927-107">Åtgärd</span><span class="sxs-lookup"><span data-stu-id="fc927-107">Action</span></span> | <span data-ttu-id="fc927-108">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="fc927-108">Description</span></span> |
|:--|:--|
| [<span data-ttu-id="fc927-109">E-post</span><span class="sxs-lookup"><span data-stu-id="fc927-109">Email</span></span>](#email-actions) | <span data-ttu-id="fc927-110">Skicka ett e-postmeddelande med hello information om hello avisering tooone eller flera mottagare.</span><span class="sxs-lookup"><span data-stu-id="fc927-110">Send an e-mail with hello details of hello alert tooone or more recipients.</span></span> |
| [<span data-ttu-id="fc927-111">Webhook</span><span class="sxs-lookup"><span data-stu-id="fc927-111">Webhook</span></span>](#webhook-actions) | <span data-ttu-id="fc927-112">Anropa en extern process via en enkel HTTP POST-begäran.</span><span class="sxs-lookup"><span data-stu-id="fc927-112">Invoke an external process through a single HTTP POST request.</span></span> |
| [<span data-ttu-id="fc927-113">Runbook</span><span class="sxs-lookup"><span data-stu-id="fc927-113">Runbook</span></span>](#runbook-actions) | <span data-ttu-id="fc927-114">Starta en runbook i Azure Automation.</span><span class="sxs-lookup"><span data-stu-id="fc927-114">Start a runbook in Azure Automation.</span></span> |


## <a name="email-actions"></a><span data-ttu-id="fc927-115">E-post-åtgärder</span><span class="sxs-lookup"><span data-stu-id="fc927-115">Email actions</span></span>
<span data-ttu-id="fc927-116">E-post-åtgärder skicka ett e-postmeddelande med hello information om hello avisering tooone eller flera mottagare.</span><span class="sxs-lookup"><span data-stu-id="fc927-116">Email actions send an e-mail with hello details of hello alert tooone or more recipients.</span></span>  <span data-ttu-id="fc927-117">Du kan ange hello ämne hello e-post, men dess innehåll är ett standardformat genom logganalys.</span><span class="sxs-lookup"><span data-stu-id="fc927-117">You can specify hello subject of hello mail, but it's content is a standard format constructed by Log Analytics.</span></span>  <span data-ttu-id="fc927-118">Tillägg toodetails av upp tooten poster som returneras av hello loggen sökningen innehåller översiktsinformation, till exempel hello namn för hello avisering.</span><span class="sxs-lookup"><span data-stu-id="fc927-118">It includes summary information such as hello name of hello alert in addition toodetails of up tooten records returned by hello log search.</span></span>  <span data-ttu-id="fc927-119">Den innehåller också en länk tooa loggen sökning i logganalys som returnerar hello hela uppsättningen av poster från frågan.</span><span class="sxs-lookup"><span data-stu-id="fc927-119">It also includes a link tooa log search in Log Analytics that will return hello entire set of records from that query.</span></span>   <span data-ttu-id="fc927-120">hello avsändaren hello e-post är *Microsoft Operations Management Suite-teamet &lt; noreply@oms.microsoft.com &gt;* .</span><span class="sxs-lookup"><span data-stu-id="fc927-120">hello sender of hello mail is *Microsoft Operations Management Suite Team &lt;noreply@oms.microsoft.com&gt;*.</span></span> 

<span data-ttu-id="fc927-121">E-post-åtgärder kräver hello egenskaper i hello i den följande tabellen.</span><span class="sxs-lookup"><span data-stu-id="fc927-121">Email actions require hello properties in hello following table.</span></span>

| <span data-ttu-id="fc927-122">Egenskap</span><span class="sxs-lookup"><span data-stu-id="fc927-122">Property</span></span> | <span data-ttu-id="fc927-123">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="fc927-123">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="fc927-124">Ämne</span><span class="sxs-lookup"><span data-stu-id="fc927-124">Subject</span></span> |<span data-ttu-id="fc927-125">Ämnesnamn i hello e-post.</span><span class="sxs-lookup"><span data-stu-id="fc927-125">Subject in hello email.</span></span>  <span data-ttu-id="fc927-126">Du kan inte ändra hello brödtext hello e-post.</span><span class="sxs-lookup"><span data-stu-id="fc927-126">You cannot modify hello body of hello mail.</span></span> |
| <span data-ttu-id="fc927-127">mottagare</span><span class="sxs-lookup"><span data-stu-id="fc927-127">Recipients</span></span> |<span data-ttu-id="fc927-128">Tar med alla e-postmottagare.</span><span class="sxs-lookup"><span data-stu-id="fc927-128">Addresses of all e-mail recipients.</span></span>  <span data-ttu-id="fc927-129">Om du anger fler än en adress och sedan separat hello-adresser med ett semikolon (;).</span><span class="sxs-lookup"><span data-stu-id="fc927-129">If you specify more than one address, then separate hello addresses with a semicolon (;).</span></span> |


## <a name="webhook-actions"></a><span data-ttu-id="fc927-130">Webhook-åtgärder</span><span class="sxs-lookup"><span data-stu-id="fc927-130">Webhook actions</span></span>

<span data-ttu-id="fc927-131">Webhook-åtgärder kan du tooinvoke en extern process via en enkel HTTP POST-begäran.</span><span class="sxs-lookup"><span data-stu-id="fc927-131">Webhook actions allow you tooinvoke an external process through a single HTTP POST request.</span></span>  <span data-ttu-id="fc927-132">hello-tjänsten som anropas bör stöder webhooks och kontrollera hur den använder alla nyttolast tas emot.</span><span class="sxs-lookup"><span data-stu-id="fc927-132">hello service being called should support webhooks and determine how it will use any payload it receives.</span></span>  <span data-ttu-id="fc927-133">Du kan också kontakta en REST-API som inte uttryckligen stöder webhooks så länge hello-begäran är i ett format som hello API förstår.</span><span class="sxs-lookup"><span data-stu-id="fc927-133">You could also call a REST API that doesn't specifically support webhooks as long as hello request is in a format that hello API understands.</span></span>  <span data-ttu-id="fc927-134">Exempel på användning av en webhook i svaret tooan avisering skickar ett meddelande i [Slack](http://slack.com) eller skapa en incident i [PagerDuty](http://pagerduty.com/).</span><span class="sxs-lookup"><span data-stu-id="fc927-134">Examples of using a webhook in response tooan alert are sending a message in [Slack](http://slack.com) or creating an incident in [PagerDuty](http://pagerduty.com/).</span></span>  <span data-ttu-id="fc927-135">En fullständig genomgång för att skapa en aviseringsregel med en webhook toocall Slack finns på [Webhooks i logganalys aviseringar](log-analytics-alerts-webhooks.md).</span><span class="sxs-lookup"><span data-stu-id="fc927-135">A complete walkthrough of creating an alert rule with a webhook toocall Slack is available at [Webhooks in Log Analytics alerts](log-analytics-alerts-webhooks.md).</span></span>

<span data-ttu-id="fc927-136">Webhook-åtgärder kräver hello egenskaper i hello i den följande tabellen.</span><span class="sxs-lookup"><span data-stu-id="fc927-136">Webhook actions require hello properties in hello following table.</span></span>

| <span data-ttu-id="fc927-137">Egenskap</span><span class="sxs-lookup"><span data-stu-id="fc927-137">Property</span></span> | <span data-ttu-id="fc927-138">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="fc927-138">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="fc927-139">Webhooksadressen</span><span class="sxs-lookup"><span data-stu-id="fc927-139">Webhook URL</span></span> |<span data-ttu-id="fc927-140">hello-URL för hello webhooken.</span><span class="sxs-lookup"><span data-stu-id="fc927-140">hello URL of hello webhook.</span></span> |
| <span data-ttu-id="fc927-141">Anpassad JSON-nyttolast</span><span class="sxs-lookup"><span data-stu-id="fc927-141">Custom JSON payload</span></span> |<span data-ttu-id="fc927-142">Anpassad nyttolast toosend med hello webhooken.</span><span class="sxs-lookup"><span data-stu-id="fc927-142">Custom payload toosend with hello webhook.</span></span>  <span data-ttu-id="fc927-143">Mer information finns i nedan.</span><span class="sxs-lookup"><span data-stu-id="fc927-143">See below for details.</span></span> |


<span data-ttu-id="fc927-144">Webhooks lägga till en URL och en nyttolast som har formaterats i JSON som är hello data skickas toohello extern tjänst.</span><span class="sxs-lookup"><span data-stu-id="fc927-144">Webhooks include a URL and a payload formatted in JSON that is hello data sent toohello external service.</span></span>  <span data-ttu-id="fc927-145">Som standard innehåller hello nyttolasten hello värden i den följande tabellen hello.</span><span class="sxs-lookup"><span data-stu-id="fc927-145">By default, hello payload will include hello values in hello following table.</span></span>  <span data-ttu-id="fc927-146">Du kan välja den här nyttolasten med en anpassad egen tooreplace.</span><span class="sxs-lookup"><span data-stu-id="fc927-146">You can choose tooreplace this payload with a custom one of your own.</span></span>  <span data-ttu-id="fc927-147">I så fall kan du använda hello variabler i hello tabellen för varje hello parametrar tooinclude deras värde i din anpassade nyttolast.</span><span class="sxs-lookup"><span data-stu-id="fc927-147">In that case you can use hello variables in hello table for each of hello parameters tooinclude their value in your custom payload.</span></span>

| <span data-ttu-id="fc927-148">Parameter</span><span class="sxs-lookup"><span data-stu-id="fc927-148">Parameter</span></span> | <span data-ttu-id="fc927-149">Variabel</span><span class="sxs-lookup"><span data-stu-id="fc927-149">Variable</span></span> | <span data-ttu-id="fc927-150">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="fc927-150">Description</span></span> |
|:--- |:--- |:--- |
| <span data-ttu-id="fc927-151">AlertRuleName</span><span class="sxs-lookup"><span data-stu-id="fc927-151">AlertRuleName</span></span> |<span data-ttu-id="fc927-152">#alertrulename</span><span class="sxs-lookup"><span data-stu-id="fc927-152">#alertrulename</span></span> |<span data-ttu-id="fc927-153">Namnet på hello varningsregel.</span><span class="sxs-lookup"><span data-stu-id="fc927-153">Name of hello alert rule.</span></span> |
| <span data-ttu-id="fc927-154">AlertThresholdOperator</span><span class="sxs-lookup"><span data-stu-id="fc927-154">AlertThresholdOperator</span></span> |<span data-ttu-id="fc927-155">#thresholdoperator</span><span class="sxs-lookup"><span data-stu-id="fc927-155">#thresholdoperator</span></span> |<span data-ttu-id="fc927-156">Tröskelvärdet operator för hello varningsregel.</span><span class="sxs-lookup"><span data-stu-id="fc927-156">Threshold operator for hello alert rule.</span></span>  <span data-ttu-id="fc927-157">*Större än* eller *mindre än*.</span><span class="sxs-lookup"><span data-stu-id="fc927-157">*Greater than* or *Less than*.</span></span> |
| <span data-ttu-id="fc927-158">AlertThresholdValue</span><span class="sxs-lookup"><span data-stu-id="fc927-158">AlertThresholdValue</span></span> |<span data-ttu-id="fc927-159">#thresholdvalue</span><span class="sxs-lookup"><span data-stu-id="fc927-159">#thresholdvalue</span></span> |<span data-ttu-id="fc927-160">Tröskelvärde för hello varningsregel.</span><span class="sxs-lookup"><span data-stu-id="fc927-160">Threshold value for hello alert rule.</span></span> |
| <span data-ttu-id="fc927-161">LinkToSearchResults</span><span class="sxs-lookup"><span data-stu-id="fc927-161">LinkToSearchResults</span></span> |<span data-ttu-id="fc927-162">#linktosearchresults</span><span class="sxs-lookup"><span data-stu-id="fc927-162">#linktosearchresults</span></span> |<span data-ttu-id="fc927-163">Länka tooLog Analytics loggen sökning som returnerar hello poster från hello-fråga som skapade hello avisering.</span><span class="sxs-lookup"><span data-stu-id="fc927-163">Link tooLog Analytics log search that returns hello records from hello query that created hello alert.</span></span> |
| <span data-ttu-id="fc927-164">ResultCount</span><span class="sxs-lookup"><span data-stu-id="fc927-164">ResultCount</span></span> |<span data-ttu-id="fc927-165">#searchresultcount</span><span class="sxs-lookup"><span data-stu-id="fc927-165">#searchresultcount</span></span> |<span data-ttu-id="fc927-166">Antalet poster i hello sökresultat.</span><span class="sxs-lookup"><span data-stu-id="fc927-166">Number of records in hello search results.</span></span> |
| <span data-ttu-id="fc927-167">SearchIntervalEndtimeUtc</span><span class="sxs-lookup"><span data-stu-id="fc927-167">SearchIntervalEndtimeUtc</span></span> |<span data-ttu-id="fc927-168">#searchintervalendtimeutc</span><span class="sxs-lookup"><span data-stu-id="fc927-168">#searchintervalendtimeutc</span></span> |<span data-ttu-id="fc927-169">Sluttid för hello frågan i UTC-format.</span><span class="sxs-lookup"><span data-stu-id="fc927-169">End time for hello query in UTC format.</span></span> |
| <span data-ttu-id="fc927-170">SearchIntervalInSeconds</span><span class="sxs-lookup"><span data-stu-id="fc927-170">SearchIntervalInSeconds</span></span> |<span data-ttu-id="fc927-171">#searchinterval</span><span class="sxs-lookup"><span data-stu-id="fc927-171">#searchinterval</span></span> |<span data-ttu-id="fc927-172">Tidsfönstret för hello varningsregel.</span><span class="sxs-lookup"><span data-stu-id="fc927-172">Time window for hello alert rule.</span></span> |
| <span data-ttu-id="fc927-173">SearchIntervalStartTimeUtc</span><span class="sxs-lookup"><span data-stu-id="fc927-173">SearchIntervalStartTimeUtc</span></span> |<span data-ttu-id="fc927-174">#searchintervalstarttimeutc</span><span class="sxs-lookup"><span data-stu-id="fc927-174">#searchintervalstarttimeutc</span></span> |<span data-ttu-id="fc927-175">Starttid för hello frågan i UTC-format.</span><span class="sxs-lookup"><span data-stu-id="fc927-175">Start time for hello query in UTC format.</span></span> |
| <span data-ttu-id="fc927-176">searchQuery</span><span class="sxs-lookup"><span data-stu-id="fc927-176">SearchQuery</span></span> |<span data-ttu-id="fc927-177">#searchquery</span><span class="sxs-lookup"><span data-stu-id="fc927-177">#searchquery</span></span> |<span data-ttu-id="fc927-178">Loggen sökfråga används av hello varningsregel.</span><span class="sxs-lookup"><span data-stu-id="fc927-178">Log search query used by hello alert rule.</span></span> |
| <span data-ttu-id="fc927-179">SearchResults</span><span class="sxs-lookup"><span data-stu-id="fc927-179">SearchResults</span></span> |<span data-ttu-id="fc927-180">Nedan finns</span><span class="sxs-lookup"><span data-stu-id="fc927-180">See below</span></span> |<span data-ttu-id="fc927-181">Poster som returneras av hello frågan i JSON-format.</span><span class="sxs-lookup"><span data-stu-id="fc927-181">Records returned by hello query in JSON format.</span></span>  <span data-ttu-id="fc927-182">Begränsad toohello första 5 000 poster.</span><span class="sxs-lookup"><span data-stu-id="fc927-182">Limited toohello first 5,000 records.</span></span> |
| <span data-ttu-id="fc927-183">WorkspaceID</span><span class="sxs-lookup"><span data-stu-id="fc927-183">WorkspaceID</span></span> |<span data-ttu-id="fc927-184">#workspaceid</span><span class="sxs-lookup"><span data-stu-id="fc927-184">#workspaceid</span></span> |<span data-ttu-id="fc927-185">ID för din OMS-arbetsyta.</span><span class="sxs-lookup"><span data-stu-id="fc927-185">ID of your OMS workspace.</span></span> |

<span data-ttu-id="fc927-186">Du kan till exempel ange hello följande anpassad nyttolast som innehåller en enda parameter med namnet *text*.</span><span class="sxs-lookup"><span data-stu-id="fc927-186">For example, you might specify hello following custom payload that includes a single parameter called *text*.</span></span>  <span data-ttu-id="fc927-187">hello-tjänst som denna webhook anropar skulle förväntas den här parametern.</span><span class="sxs-lookup"><span data-stu-id="fc927-187">hello service that this webhook calls would be expecting this parameter.</span></span>

    {
        "text":"#alertrulename fired with #searchresultcount over threshold of #thresholdvalue."
    }

<span data-ttu-id="fc927-188">Det här exemplet nyttolast löser toosomething som hello efter när skickas toohello webhooken.</span><span class="sxs-lookup"><span data-stu-id="fc927-188">This example payload would resolve toosomething like hello following when sent toohello webhook.</span></span>

    {
        "text":"My Alert Rule fired with 18 records over threshold of 10 ."
    }

<span data-ttu-id="fc927-189">tooinclude sökresultat i en anpassad nyttolast Lägg till följande rad som en egenskap för översta nivån i hello json-nyttolast hello.</span><span class="sxs-lookup"><span data-stu-id="fc927-189">tooinclude search results in a custom payload, add hello following line as a top level property in hello json payload.</span></span>  

    "IncludeSearchResults":true

<span data-ttu-id="fc927-190">Till exempel toocreate en anpassad nyttolast som innehåller bara hello aviseringsnamn och hello sökresultat, du kan använda följande hello.</span><span class="sxs-lookup"><span data-stu-id="fc927-190">For example, toocreate a custom payload that includes just hello alert name and hello search results, you could use hello following.</span></span> 

    {
       "alertname":"#alertrulename",
       "IncludeSearchResults":true
    }


<span data-ttu-id="fc927-191">Du kan gå igenom en komplett exempel på hur du skapar en aviseringsregel med en webhook toostart en extern tjänst på [skapar en avisering webhook-åtgärd i OMS logganalys toosend meddelandet tooSlack](log-analytics-alerts-webhooks.md).</span><span class="sxs-lookup"><span data-stu-id="fc927-191">You can walk through a complete example of creating an alert rule with a webhook toostart an external service at [Create an alert webhook action in OMS Log Analytics toosend message tooSlack](log-analytics-alerts-webhooks.md).</span></span>

## <a name="runbook-actions"></a><span data-ttu-id="fc927-192">Runbook-åtgärder</span><span class="sxs-lookup"><span data-stu-id="fc927-192">Runbook actions</span></span>
<span data-ttu-id="fc927-193">Runbook-åtgärder startar en runbook i Azure Automation.</span><span class="sxs-lookup"><span data-stu-id="fc927-193">Runbook actions start a runbook in Azure Automation.</span></span>  <span data-ttu-id="fc927-194">Ordna toouse denna typ av åtgärd måste du ha hello [automatiseringslösning](log-analytics-add-solutions.md) installeras och konfigureras i OMS-arbetsyta.</span><span class="sxs-lookup"><span data-stu-id="fc927-194">In order toouse this type of action, you must have hello [Automation solution](log-analytics-add-solutions.md) installed and configured in your OMS workspace.</span></span>  <span data-ttu-id="fc927-195">Du kan välja mellan hello runbooks i hello automation-konto som du konfigurerade i hello Automation-lösningen.</span><span class="sxs-lookup"><span data-stu-id="fc927-195">You can select from hello runbooks in hello automation account that you configured in hello Automation solution.</span></span>

<span data-ttu-id="fc927-196">Runbook-åtgärder kräver hello egenskaper i hello i den följande tabellen.</span><span class="sxs-lookup"><span data-stu-id="fc927-196">Runbook actions require hello properties in hello following table.</span></span>

| <span data-ttu-id="fc927-197">Egenskap</span><span class="sxs-lookup"><span data-stu-id="fc927-197">Property</span></span> | <span data-ttu-id="fc927-198">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="fc927-198">Description</span></span> |
|:--- |:---|
| <span data-ttu-id="fc927-199">Runbook</span><span class="sxs-lookup"><span data-stu-id="fc927-199">Runbook</span></span> | <span data-ttu-id="fc927-200">Runbook som du vill toostart när en avisering skapas.</span><span class="sxs-lookup"><span data-stu-id="fc927-200">Runbook that you want toostart when an alert is created.</span></span> |
| <span data-ttu-id="fc927-201">Kör på</span><span class="sxs-lookup"><span data-stu-id="fc927-201">Run on</span></span> | <span data-ttu-id="fc927-202">Ange **Azure** toorun hello runbook i hello molnet.</span><span class="sxs-lookup"><span data-stu-id="fc927-202">Specify **Azure** toorun hello runbook in hello cloud.</span></span>  <span data-ttu-id="fc927-203">Ange **Hybrid worker** toorun hello runbook på en agent med [Hybrid Runbook Worker](../automation/automation-hybrid-runbook-worker.md ) installerad.</span><span class="sxs-lookup"><span data-stu-id="fc927-203">Specify **Hybrid worker** toorun hello runbook on an agent with [Hybrid Runbook Worker](../automation/automation-hybrid-runbook-worker.md ) installed.</span></span>  |

<span data-ttu-id="fc927-204">Runbook-åtgärder starta hello runbook med hjälp av en [webhook](../automation/automation-webhooks.md).</span><span class="sxs-lookup"><span data-stu-id="fc927-204">Runbook actions start hello runbook using a [webhook](../automation/automation-webhooks.md).</span></span>  <span data-ttu-id="fc927-205">När du skapar hello varningsregeln automatiskt skapas en ny webhook för hello runbook med namnet hello **OMS avisering reparation** följt av ett GUID.</span><span class="sxs-lookup"><span data-stu-id="fc927-205">When you create hello alert rule, it will automatically create a new webhook for hello runbook with hello name **OMS Alert Remediation** followed by a GUID.</span></span>  

<span data-ttu-id="fc927-206">Du kan inte direkt fylla eventuella parametrar för hello runbook, men hello [$WebhookData parametern](../automation/automation-webhooks.md) innehåller hello information om hello aviseringen, inklusive hello resultaten av hello logg som den skapades.</span><span class="sxs-lookup"><span data-stu-id="fc927-206">You cannot directly populate any parameters of hello runbook, but hello [$WebhookData parameter](../automation/automation-webhooks.md) will include hello details of hello alert, including hello results of hello log search that created it.</span></span>  <span data-ttu-id="fc927-207">Hej runbook behöver toodefine **$WebhookData** som en parameter för den tooaccess hello egenskaper för hello avisering.</span><span class="sxs-lookup"><span data-stu-id="fc927-207">hello runbook will need toodefine **$WebhookData** as a parameter for it tooaccess hello properties of hello alert.</span></span>  <span data-ttu-id="fc927-208">hello aviseringsdata är tillgänglig i json-format i en enda egenskap som kallas **SearchResults** i hello **RequestBody** -egenskapen för **$WebhookData**.</span><span class="sxs-lookup"><span data-stu-id="fc927-208">hello alert data is available in json format in a single property called **SearchResults** in hello **RequestBody** property of **$WebhookData**.</span></span>  <span data-ttu-id="fc927-209">Detta har med hello egenskaper i hello i den följande tabellen.</span><span class="sxs-lookup"><span data-stu-id="fc927-209">This will have with hello properties in hello following table.</span></span>

| <span data-ttu-id="fc927-210">Node</span><span class="sxs-lookup"><span data-stu-id="fc927-210">Node</span></span> | <span data-ttu-id="fc927-211">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="fc927-211">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="fc927-212">id</span><span class="sxs-lookup"><span data-stu-id="fc927-212">id</span></span> |<span data-ttu-id="fc927-213">Sökvägen och GUID hello sökning.</span><span class="sxs-lookup"><span data-stu-id="fc927-213">Path and GUID of hello search.</span></span> |
| <span data-ttu-id="fc927-214">__metadata</span><span class="sxs-lookup"><span data-stu-id="fc927-214">__metadata</span></span> |<span data-ttu-id="fc927-215">Information om hello avisering inklusive hello antalet poster och status för hello sökresultat.</span><span class="sxs-lookup"><span data-stu-id="fc927-215">Information about hello alert including hello number of records and status of hello search results.</span></span> |
| <span data-ttu-id="fc927-216">värde</span><span class="sxs-lookup"><span data-stu-id="fc927-216">value</span></span> |<span data-ttu-id="fc927-217">Separat post för varje post i hello sökresultat.</span><span class="sxs-lookup"><span data-stu-id="fc927-217">Separate entry for each record in hello search results.</span></span>  <span data-ttu-id="fc927-218">hello information om hello transaktionen matchar hello egenskaper och värden för hello-post.</span><span class="sxs-lookup"><span data-stu-id="fc927-218">hello details of hello entry will match hello properties and values of hello record.</span></span> |

<span data-ttu-id="fc927-219">Till exempel skulle hello följande runbook extrahera hello-poster som returneras av hello loggen Sök och tilldela olika egenskaper utifrån hello typ av varje post.</span><span class="sxs-lookup"><span data-stu-id="fc927-219">For example, hello following runbook would extract hello records returned by hello log search  and assign different properties based on hello type of each record.</span></span>  <span data-ttu-id="fc927-220">Observera att hello runbook startas genom att konvertera **RequestBody** från json så att den kan bearbetas med som ett objekt i PowerShell.</span><span class="sxs-lookup"><span data-stu-id="fc927-220">Note that hello runbook starts by converting **RequestBody** from json so that it can be worked with as an object in PowerShell.</span></span>

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


## <a name="next-steps"></a><span data-ttu-id="fc927-221">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="fc927-221">Next steps</span></span>
- <span data-ttu-id="fc927-222">Slutföra en genomgång för [konfigurerar en webook](log-analytics-alerts-webhooks.md) med en aviseringsregel.</span><span class="sxs-lookup"><span data-stu-id="fc927-222">Complete a walkthrough for [configuring a webook](log-analytics-alerts-webhooks.md) with an alert rule.</span></span>  
- <span data-ttu-id="fc927-223">Lär dig hur toowrite [runbooks i Azure Automation](https://azure.microsoft.com/documentation/services/automation) tooremediate problem som identifieras av aviseringar.</span><span class="sxs-lookup"><span data-stu-id="fc927-223">Learn how toowrite [runbooks in Azure Automation](https://azure.microsoft.com/documentation/services/automation) tooremediate problems identified by alerts.</span></span>
