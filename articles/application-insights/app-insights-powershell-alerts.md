---
title: aaaUse Powershell tooset aviseringar i Application Insights | Microsoft Docs
description: "Automatisera konfigurationen av Application Insights tooget e-postmeddelanden om ändringar av mått."
services: application-insights
documentationcenter: 
author: CFreemanwa
manager: carmonm
ms.assetid: 05d6a9e0-77a2-4a35-9052-a7768d23a196
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 10/31/2016
ms.author: bwren
ms.openlocfilehash: d68e5f9511bb4015f59175724bc1a4a04ecf43e1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-powershell-tooset-alerts-in-application-insights"></a><span data-ttu-id="69333-103">Använd PowerShell tooset aviseringar i Application Insights</span><span class="sxs-lookup"><span data-stu-id="69333-103">Use PowerShell tooset alerts in Application Insights</span></span>
<span data-ttu-id="69333-104">Du kan automatisera hello konfigurationen av [aviseringar](app-insights-alerts.md) i [Programinsikter](app-insights-overview.md).</span><span class="sxs-lookup"><span data-stu-id="69333-104">You can automate hello configuration of [alerts](app-insights-alerts.md) in [Application Insights](app-insights-overview.md).</span></span>

<span data-ttu-id="69333-105">Dessutom kan du [ange webhooks tooautomate aviseringen svar tooan](../monitoring-and-diagnostics/insights-webhooks-alerts.md).</span><span class="sxs-lookup"><span data-stu-id="69333-105">In addition, you can [set webhooks tooautomate your response tooan alert](../monitoring-and-diagnostics/insights-webhooks-alerts.md).</span></span>

> [!NOTE]
> <span data-ttu-id="69333-106">Om du vill toocreate resurser och varningar på hello samma tid bör du överväga att [med en Azure Resource Manager-mall](app-insights-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="69333-106">If you want toocreate resources and alerts at hello same time, consider [using an Azure Resource Manager template](app-insights-powershell.md).</span></span>
>
>

## <a name="one-time-setup"></a><span data-ttu-id="69333-107">Enstaka installationen</span><span class="sxs-lookup"><span data-stu-id="69333-107">One-time setup</span></span>
<span data-ttu-id="69333-108">Om du inte har använt PowerShell med din Azure-prenumeration innan du:</span><span class="sxs-lookup"><span data-stu-id="69333-108">If you haven't used PowerShell with your Azure subscription before:</span></span>

<span data-ttu-id="69333-109">Installera hello Azure Powershell-modulen på hello datorn där du vill att toorun hello skript.</span><span class="sxs-lookup"><span data-stu-id="69333-109">Install hello Azure Powershell module on hello machine where you want toorun hello scripts.</span></span>

* <span data-ttu-id="69333-110">Installera [Microsoft Web Platform Installer (v5 eller högre)](http://www.microsoft.com/web/downloads/platform.aspx).</span><span class="sxs-lookup"><span data-stu-id="69333-110">Install [Microsoft Web Platform Installer (v5 or higher)](http://www.microsoft.com/web/downloads/platform.aspx).</span></span>
* <span data-ttu-id="69333-111">Använd den tooinstall Microsoft Azure Powershell</span><span class="sxs-lookup"><span data-stu-id="69333-111">Use it tooinstall Microsoft Azure Powershell</span></span>

## <a name="connect-tooazure"></a><span data-ttu-id="69333-112">Ansluta tooAzure</span><span class="sxs-lookup"><span data-stu-id="69333-112">Connect tooAzure</span></span>
<span data-ttu-id="69333-113">Starta Azure PowerShell och [ansluter tooyour prenumeration](/powershell/azure/overview):</span><span class="sxs-lookup"><span data-stu-id="69333-113">Start Azure PowerShell and [connect tooyour subscription](/powershell/azure/overview):</span></span>

```PowerShell

    Add-AzureAccount
```


## <a name="get-alerts"></a><span data-ttu-id="69333-114">Få aviseringar</span><span class="sxs-lookup"><span data-stu-id="69333-114">Get alerts</span></span>
    Get-AzureAlertRmRule -ResourceGroup "Fabrikam" [-Name "My rule"] [-DetailedOutput]

## <a name="add-alert"></a><span data-ttu-id="69333-115">Lägg till avisering</span><span class="sxs-lookup"><span data-stu-id="69333-115">Add alert</span></span>
    Add-AlertRule  -Name "{ALERT NAME}" -Description "{TEXT}" `
     -ResourceGroup "{GROUP NAME}" `
     -ResourceId "/subscriptions/{SUBSCRIPTION ID}/resourcegroups/{GROUP NAME}/providers/microsoft.insights/components/{APP RESOURCE NAME}" `
     -MetricName "{METRIC NAME}" `
     -Operator GreaterThan  `
     -Threshold {NUMBER}   `
     -WindowSize {HH:MM:SS}  `
     [-SendEmailToServiceOwners] `
     [-CustomEmails "EMAIL1@X.COM","EMAIL2@Y.COM" ] `
     -Location "East US" // must be East US at present
     -RuleType Metric



## <a name="example-1"></a><span data-ttu-id="69333-116">Exempel 1</span><span class="sxs-lookup"><span data-stu-id="69333-116">Example 1</span></span>
<span data-ttu-id="69333-117">E-posta mig om hello serverns svar tooHTTP begäranden, ett genomsnitt över 5 minuter är långsammare än 1 sekund.</span><span class="sxs-lookup"><span data-stu-id="69333-117">Email me if hello server's response tooHTTP requests, averaged over 5 minutes, is slower than 1 second.</span></span> <span data-ttu-id="69333-118">Application Insights-resurs kallas IceCreamWebApp och den är i resursgruppen Fabrikam.</span><span class="sxs-lookup"><span data-stu-id="69333-118">My Application Insights resource is called IceCreamWebApp, and it is in resource group Fabrikam.</span></span> <span data-ttu-id="69333-119">Jag hello ägare hello Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="69333-119">I am hello owner of hello Azure subscription.</span></span>

<span data-ttu-id="69333-120">hello GUID är hello prenumerations-ID (inte hello instrumentation nyckel av programmet hello).</span><span class="sxs-lookup"><span data-stu-id="69333-120">hello GUID is hello subscription ID (not hello instrumentation key of hello application).</span></span>

    Add-AlertRule -Name "slow responses" `
     -Description "email me if hello server responds slowly" `
     -ResourceGroup "Fabrikam" `
     -ResourceId "/subscriptions/00000000-0000-0000-0000-000000000000/resourcegroups/Fabrikam/providers/microsoft.insights/components/IceCreamWebApp" `
     -MetricName "request.duration" `
     -Operator GreaterThan `
     -Threshold 1 `
     -WindowSize 00:05:00 `
     -SendEmailToServiceOwners `
     -Location "East US" -RuleType Metric

## <a name="example-2"></a><span data-ttu-id="69333-121">Exempel 2</span><span class="sxs-lookup"><span data-stu-id="69333-121">Example 2</span></span>
<span data-ttu-id="69333-122">Jag har ett program som jag använder [TrackMetric()](app-insights-api-custom-events-metrics.md#trackmetric) tooreport ett mått med namnet ”salesPerHour”.</span><span class="sxs-lookup"><span data-stu-id="69333-122">I have an application in which I use [TrackMetric()](app-insights-api-custom-events-metrics.md#trackmetric) tooreport a metric named "salesPerHour."</span></span> <span data-ttu-id="69333-123">Skicka ett e-postmeddelande toomy kollegor om ”salesPerHour” sjunker under 100, ett genomsnitt under 24 timmar.</span><span class="sxs-lookup"><span data-stu-id="69333-123">Send an email toomy colleagues if "salesPerHour" drops below 100, averaged over 24 hours.</span></span>

    Add-AlertRule -Name "poor sales" `
     -Description "slow sales alert" `
     -ResourceGroup "Fabrikam" `
     -ResourceId "/subscriptions/00000000-0000-0000-0000-000000000000/resourcegroups/Fabrikam/providers/microsoft.insights/components/IceCreamWebApp" `
     -MetricName "salesPerHour" `
     -Operator LessThan `
     -Threshold 100 `
     -WindowSize 24:00:00 `
     -CustomEmails "satish@fabrikam.com","lei@fabrikam.com" `
     -Location "East US" -RuleType Metric

<span data-ttu-id="69333-124">hello samma regel kan användas för hello mått som rapporteras med hjälp av hello [mätning parametern](app-insights-api-custom-events-metrics.md#properties) för spårning av ett annat anrop till exempel TrackEvent eller trackPageView.</span><span class="sxs-lookup"><span data-stu-id="69333-124">hello same rule can be used for hello metric reported by using hello [measurement parameter](app-insights-api-custom-events-metrics.md#properties) of another tracking call such as TrackEvent or trackPageView.</span></span>

## <a name="metric-names"></a><span data-ttu-id="69333-125">Tjänstmåttets namn</span><span class="sxs-lookup"><span data-stu-id="69333-125">Metric names</span></span>
| <span data-ttu-id="69333-126">Måttnamnet</span><span class="sxs-lookup"><span data-stu-id="69333-126">Metric name</span></span> | <span data-ttu-id="69333-127">Skärmnamn på</span><span class="sxs-lookup"><span data-stu-id="69333-127">Screen name</span></span> | <span data-ttu-id="69333-128">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="69333-128">Description</span></span> |
| --- | --- | --- |
| `basicExceptionBrowser.count` |<span data-ttu-id="69333-129">Webbläsarundantag</span><span class="sxs-lookup"><span data-stu-id="69333-129">Browser exceptions</span></span> |<span data-ttu-id="69333-130">Antal undantagsfel utan felhantering utlöstes i hello webbläsare.</span><span class="sxs-lookup"><span data-stu-id="69333-130">Count of uncaught exceptions thrown in hello browser.</span></span> |
| `basicExceptionServer.count` |<span data-ttu-id="69333-131">Undantag för</span><span class="sxs-lookup"><span data-stu-id="69333-131">Server exceptions</span></span> |<span data-ttu-id="69333-132">Antal undantagsfel utan felhantering utlöstes av hello app</span><span class="sxs-lookup"><span data-stu-id="69333-132">Count of unhandled exceptions thrown by hello app</span></span> |
| `clientPerformance.clientProcess.value` |<span data-ttu-id="69333-133">Bearbetningstid för klienten</span><span class="sxs-lookup"><span data-stu-id="69333-133">Client processing time</span></span> |<span data-ttu-id="69333-134">Tiden mellan tar emot hello sista byten av ett dokument tills hello DOM har lästs in.</span><span class="sxs-lookup"><span data-stu-id="69333-134">Time between receiving hello last byte of a document until hello DOM is loaded.</span></span> <span data-ttu-id="69333-135">Asynkrona förfrågningar kan fortfarande vara under bearbetning.</span><span class="sxs-lookup"><span data-stu-id="69333-135">Async requests may still be processing.</span></span> |
| `clientPerformance.networkConnection.value` |<span data-ttu-id="69333-136">Nätverket Anslut sidinläsningstiden</span><span class="sxs-lookup"><span data-stu-id="69333-136">Page load network connect time</span></span> |<span data-ttu-id="69333-137">Tid hello webbläsare tar tooconnect toohello nätverk.</span><span class="sxs-lookup"><span data-stu-id="69333-137">Time hello browser takes tooconnect toohello network.</span></span> <span data-ttu-id="69333-138">Kan vara 0 om cachelagrade.</span><span class="sxs-lookup"><span data-stu-id="69333-138">Can be 0 if cached.</span></span> |
| `clientPerformance.receiveRequest.value` |<span data-ttu-id="69333-139">Ta emot svarstid</span><span class="sxs-lookup"><span data-stu-id="69333-139">Receiving response time</span></span> |<span data-ttu-id="69333-140">Tiden mellan webbläsaren som skickar begäran toostarting tooreceive svar.</span><span class="sxs-lookup"><span data-stu-id="69333-140">Time between browser sending request toostarting tooreceive response.</span></span> |
| `clientPerformance.sendRequest.value` |<span data-ttu-id="69333-141">Skicka tid för begäran</span><span class="sxs-lookup"><span data-stu-id="69333-141">Send request time</span></span> |<span data-ttu-id="69333-142">Tid som toosend webbläsarbegäran.</span><span class="sxs-lookup"><span data-stu-id="69333-142">Time taken by browser toosend request.</span></span> |
| `clientPerformance.total.value` |<span data-ttu-id="69333-143">Sidhämtningstid</span><span class="sxs-lookup"><span data-stu-id="69333-143">Browser page load time</span></span> |<span data-ttu-id="69333-144">Tiden från användarförfrågan till dess att DOM, formatmallar, skript och bilder har lästs in.</span><span class="sxs-lookup"><span data-stu-id="69333-144">Time from user request until DOM, stylesheets, scripts and images are loaded.</span></span> |
| `performanceCounter.available_bytes.value` |<span data-ttu-id="69333-145">Tillgängligt minne</span><span class="sxs-lookup"><span data-stu-id="69333-145">Available memory</span></span> |<span data-ttu-id="69333-146">Fysiskt minne som är direkt tillgängligt för en process eller för systemanvändning.</span><span class="sxs-lookup"><span data-stu-id="69333-146">Physical memory immediately available for a process or for system use.</span></span> |
| `performanceCounter.io_data_bytes_per_sec.value` |<span data-ttu-id="69333-147">Process-i/o-hastighet</span><span class="sxs-lookup"><span data-stu-id="69333-147">Process IO Rate</span></span> |<span data-ttu-id="69333-148">Totalt antal byte per andra Läs och skriftliga toofiles, nätverk och enheter.</span><span class="sxs-lookup"><span data-stu-id="69333-148">Total bytes per second read and written toofiles, network and devices.</span></span> |
| `performanceCounter.number_of_exceps_thrown_per_sec.value` |<span data-ttu-id="69333-149">hastighet för undantag</span><span class="sxs-lookup"><span data-stu-id="69333-149">exception rate</span></span> |<span data-ttu-id="69333-150">Undantag per sekund.</span><span class="sxs-lookup"><span data-stu-id="69333-150">Exceptions thrown per second.</span></span> |
| `performanceCounter.percentage_processor_time.value` |<span data-ttu-id="69333-151">Processen CPU</span><span class="sxs-lookup"><span data-stu-id="69333-151">Process CPU</span></span> |<span data-ttu-id="69333-152">hello procentandel av förfluten tid som alla processens trådar som används av hello processor tooexecution instruktioner för hello program processen.</span><span class="sxs-lookup"><span data-stu-id="69333-152">hello percentage of elapsed time of all process threads used by hello processor tooexecution instructions for hello applications process.</span></span> |
| `performanceCounter.percentage_processor_total.value` |<span data-ttu-id="69333-153">Processortid</span><span class="sxs-lookup"><span data-stu-id="69333-153">Processor time</span></span> |<span data-ttu-id="69333-154">hello procentandel av tiden som hello processor tillbringar i icke-inaktiva trådar.</span><span class="sxs-lookup"><span data-stu-id="69333-154">hello percentage of time that hello processor spends in non-Idle threads.</span></span> |
| `performanceCounter.process_private_bytes.value` |<span data-ttu-id="69333-155">Processen för privata byte</span><span class="sxs-lookup"><span data-stu-id="69333-155">Process private bytes</span></span> |<span data-ttu-id="69333-156">Minne som tilldelats exklusivt toohello övervakade programprocesser.</span><span class="sxs-lookup"><span data-stu-id="69333-156">Memory exclusively assigned toohello monitored application's processes.</span></span> |
| `performanceCounter.request_execution_time.value` |<span data-ttu-id="69333-157">Körningstid för ASP.NET-begäran</span><span class="sxs-lookup"><span data-stu-id="69333-157">ASP.NET request execution time</span></span> |<span data-ttu-id="69333-158">Körningstid för senaste hello-begäran.</span><span class="sxs-lookup"><span data-stu-id="69333-158">Execution time of hello most recent request.</span></span> |
| `performanceCounter.requests_in_application_queue.value` |<span data-ttu-id="69333-159">ASP.NET-begäranden i kö för körning</span><span class="sxs-lookup"><span data-stu-id="69333-159">ASP.NET requests in execution queue</span></span> |<span data-ttu-id="69333-160">Längden på hello programbegärandekön.</span><span class="sxs-lookup"><span data-stu-id="69333-160">Length of hello application request queue.</span></span> |
| `performanceCounter.requests_per_sec.value` |<span data-ttu-id="69333-161">ASP.NET-begärandehastighet</span><span class="sxs-lookup"><span data-stu-id="69333-161">ASP.NET request rate</span></span> |<span data-ttu-id="69333-162">Hastigheten för alla begäranden toohello program per sekund från ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="69333-162">Rate of all requests toohello application per second from ASP.NET.</span></span> |
| `remoteDependencyFailed.durationMetric.count` |<span data-ttu-id="69333-163">Beroende fel</span><span class="sxs-lookup"><span data-stu-id="69333-163">Dependency failures</span></span> |<span data-ttu-id="69333-164">Antal misslyckade anrop gjorda av hello server tooexternal programresurser.</span><span class="sxs-lookup"><span data-stu-id="69333-164">Count of failed calls made by hello server application tooexternal resources.</span></span> |
| `request.duration` |<span data-ttu-id="69333-165">Serversvarstid</span><span class="sxs-lookup"><span data-stu-id="69333-165">Server response time</span></span> |<span data-ttu-id="69333-166">Tiden mellan ta emot en HTTP-begäran och slutför hello svar skickas.</span><span class="sxs-lookup"><span data-stu-id="69333-166">Time between receiving an HTTP request and finishing sending hello response.</span></span> |
| `request.rate` |<span data-ttu-id="69333-167">Begärandehastighet</span><span class="sxs-lookup"><span data-stu-id="69333-167">Request rate</span></span> |<span data-ttu-id="69333-168">Hastigheten för alla begäranden toohello program per sekund.</span><span class="sxs-lookup"><span data-stu-id="69333-168">Rate of all requests toohello application per second.</span></span> |
| `requestFailed.count` |<span data-ttu-id="69333-169">Misslyckade begäranden</span><span class="sxs-lookup"><span data-stu-id="69333-169">Failed requests</span></span> |<span data-ttu-id="69333-170">Antal HTTP-begäranden som resulterade i en svarskoden > = 400</span><span class="sxs-lookup"><span data-stu-id="69333-170">Count of HTTP requests that resulted in a response code >= 400</span></span> |
| `view.count` |<span data-ttu-id="69333-171">Sidvisningar</span><span class="sxs-lookup"><span data-stu-id="69333-171">Page views</span></span> |<span data-ttu-id="69333-172">Antal klientens användarförfrågningar för en webbsida.</span><span class="sxs-lookup"><span data-stu-id="69333-172">Count of client user requests for a web page.</span></span> <span data-ttu-id="69333-173">Syntetisk trafik filtreras bort.</span><span class="sxs-lookup"><span data-stu-id="69333-173">Synthetic traffic is filtered out.</span></span> |
| <span data-ttu-id="69333-174">{din anpassade mått name}</span><span class="sxs-lookup"><span data-stu-id="69333-174">{your custom metric name}</span></span> |<span data-ttu-id="69333-175">{Måttnamnet}</span><span class="sxs-lookup"><span data-stu-id="69333-175">{Your metric name}</span></span> |<span data-ttu-id="69333-176">Värdet för din mått som rapporteras av [TrackMetric](app-insights-api-custom-events-metrics.md#trackmetric) eller i hello [mätningar parametern för ett anrop för spårning av](app-insights-api-custom-events-metrics.md#properties).</span><span class="sxs-lookup"><span data-stu-id="69333-176">Your metric value reported by [TrackMetric](app-insights-api-custom-events-metrics.md#trackmetric) or in hello [measurements parameter of a tracking call](app-insights-api-custom-events-metrics.md#properties).</span></span> |

<span data-ttu-id="69333-177">hello mått skickas av olika telemetri moduler:</span><span class="sxs-lookup"><span data-stu-id="69333-177">hello metrics are sent by different telemetry modules:</span></span>

| <span data-ttu-id="69333-178">Mått grupp</span><span class="sxs-lookup"><span data-stu-id="69333-178">Metric group</span></span> | <span data-ttu-id="69333-179">Insamlaren modul</span><span class="sxs-lookup"><span data-stu-id="69333-179">Collector module</span></span> |
| --- | --- |
| <span data-ttu-id="69333-180">basicExceptionBrowser,</span><span class="sxs-lookup"><span data-stu-id="69333-180">basicExceptionBrowser,</span></span><br/><span data-ttu-id="69333-181">clientPerformance,</span><span class="sxs-lookup"><span data-stu-id="69333-181">clientPerformance,</span></span><br/><span data-ttu-id="69333-182">vy</span><span class="sxs-lookup"><span data-stu-id="69333-182">view</span></span> |[<span data-ttu-id="69333-183">Webbläsaren JavaScript</span><span class="sxs-lookup"><span data-stu-id="69333-183">Browser JavaScript</span></span>](app-insights-javascript.md) |
| <span data-ttu-id="69333-184">performanceCounter</span><span class="sxs-lookup"><span data-stu-id="69333-184">performanceCounter</span></span> |[<span data-ttu-id="69333-185">Prestanda</span><span class="sxs-lookup"><span data-stu-id="69333-185">Performance</span></span>](app-insights-configuration-with-applicationinsights-config.md) |
| <span data-ttu-id="69333-186">remoteDependencyFailed</span><span class="sxs-lookup"><span data-stu-id="69333-186">remoteDependencyFailed</span></span> |[<span data-ttu-id="69333-187">Beroende</span><span class="sxs-lookup"><span data-stu-id="69333-187">Dependency</span></span>](app-insights-configuration-with-applicationinsights-config.md) |
| <span data-ttu-id="69333-188">begäran</span><span class="sxs-lookup"><span data-stu-id="69333-188">request,</span></span><br/><span data-ttu-id="69333-189">requestFailed</span><span class="sxs-lookup"><span data-stu-id="69333-189">requestFailed</span></span> |[<span data-ttu-id="69333-190">Serverbegäran</span><span class="sxs-lookup"><span data-stu-id="69333-190">Server request</span></span>](app-insights-configuration-with-applicationinsights-config.md) |

## <a name="webhooks"></a><span data-ttu-id="69333-191">Webhooks</span><span class="sxs-lookup"><span data-stu-id="69333-191">Webhooks</span></span>
<span data-ttu-id="69333-192">Du kan [automatisera aviseringen svar tooan](../monitoring-and-diagnostics/insights-webhooks-alerts.md).</span><span class="sxs-lookup"><span data-stu-id="69333-192">You can [automate your response tooan alert](../monitoring-and-diagnostics/insights-webhooks-alerts.md).</span></span> <span data-ttu-id="69333-193">Azure anropar en webbadress som du väljer när en avisering utlöses.</span><span class="sxs-lookup"><span data-stu-id="69333-193">Azure will call a web address of your choice when an alert is raised.</span></span>

## <a name="see-also"></a><span data-ttu-id="69333-194">Se även</span><span class="sxs-lookup"><span data-stu-id="69333-194">See also</span></span>
* [<span data-ttu-id="69333-195">Skriptet tooconfigure Application Insights</span><span class="sxs-lookup"><span data-stu-id="69333-195">Script tooconfigure Application Insights</span></span>](app-insights-powershell-script-create-resource.md)
* [<span data-ttu-id="69333-196">Skapa Application Insights och testa webbresurser från mallar</span><span class="sxs-lookup"><span data-stu-id="69333-196">Create Application Insights and web test resources from templates</span></span>](app-insights-powershell.md)
* [<span data-ttu-id="69333-197">Automatisera koppling Microsoft Azure-diagnostik tooApplication insikter</span><span class="sxs-lookup"><span data-stu-id="69333-197">Automate coupling Microsoft Azure Diagnostics tooApplication Insights</span></span>](app-insights-powershell-azure-diagnostics.md)
* [<span data-ttu-id="69333-198">Automatisera svar tooan aviseringen</span><span class="sxs-lookup"><span data-stu-id="69333-198">Automate your response tooan alert</span></span>](../monitoring-and-diagnostics/insights-webhooks-alerts.md)
