---
title: "Använda Powershell för att ställa in aviseringar i Application Insights | Microsoft Docs"
description: "Automatisera konfigurationen av Application Insights som ska få e-postmeddelanden om ändringar av mått."
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
ms.openlocfilehash: 64675c51abf80daa3a55220f910aa8fdee1042ca
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="use-powershell-to-set-alerts-in-application-insights"></a><span data-ttu-id="79adc-103">Använd PowerShell för att ställa in aviseringar i Application Insights</span><span class="sxs-lookup"><span data-stu-id="79adc-103">Use PowerShell to set alerts in Application Insights</span></span>
<span data-ttu-id="79adc-104">Du kan automatisera konfigurationen av [aviseringar](app-insights-alerts.md) i [Programinsikter](app-insights-overview.md).</span><span class="sxs-lookup"><span data-stu-id="79adc-104">You can automate the configuration of [alerts](app-insights-alerts.md) in [Application Insights](app-insights-overview.md).</span></span>

<span data-ttu-id="79adc-105">Dessutom kan du [ange webhooks att automatisera dina svar på en avisering](../monitoring-and-diagnostics/insights-webhooks-alerts.md).</span><span class="sxs-lookup"><span data-stu-id="79adc-105">In addition, you can [set webhooks to automate your response to an alert](../monitoring-and-diagnostics/insights-webhooks-alerts.md).</span></span>

> [!NOTE]
> <span data-ttu-id="79adc-106">Om du vill skapa resurser och -varningar samtidigt bör [med en Azure Resource Manager-mall](app-insights-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="79adc-106">If you want to create resources and alerts at the same time, consider [using an Azure Resource Manager template](app-insights-powershell.md).</span></span>
>
>

## <a name="one-time-setup"></a><span data-ttu-id="79adc-107">Enstaka installationen</span><span class="sxs-lookup"><span data-stu-id="79adc-107">One-time setup</span></span>
<span data-ttu-id="79adc-108">Om du inte har använt PowerShell med din Azure-prenumeration innan du:</span><span class="sxs-lookup"><span data-stu-id="79adc-108">If you haven't used PowerShell with your Azure subscription before:</span></span>

<span data-ttu-id="79adc-109">Installera Azure Powershell-modulen på datorn där du vill köra skripten.</span><span class="sxs-lookup"><span data-stu-id="79adc-109">Install the Azure Powershell module on the machine where you want to run the scripts.</span></span>

* <span data-ttu-id="79adc-110">Installera [Microsoft Web Platform Installer (v5 eller högre)](http://www.microsoft.com/web/downloads/platform.aspx).</span><span class="sxs-lookup"><span data-stu-id="79adc-110">Install [Microsoft Web Platform Installer (v5 or higher)](http://www.microsoft.com/web/downloads/platform.aspx).</span></span>
* <span data-ttu-id="79adc-111">Du installerar Microsoft Azure Powershell</span><span class="sxs-lookup"><span data-stu-id="79adc-111">Use it to install Microsoft Azure Powershell</span></span>

## <a name="connect-to-azure"></a><span data-ttu-id="79adc-112">Anslut till Azure</span><span class="sxs-lookup"><span data-stu-id="79adc-112">Connect to Azure</span></span>
<span data-ttu-id="79adc-113">Starta Azure PowerShell och [ansluta till din prenumeration](/powershell/azure/overview):</span><span class="sxs-lookup"><span data-stu-id="79adc-113">Start Azure PowerShell and [connect to your subscription](/powershell/azure/overview):</span></span>

```PowerShell

    Add-AzureAccount
```


## <a name="get-alerts"></a><span data-ttu-id="79adc-114">Få aviseringar</span><span class="sxs-lookup"><span data-stu-id="79adc-114">Get alerts</span></span>
    Get-AzureAlertRmRule -ResourceGroup "Fabrikam" [-Name "My rule"] [-DetailedOutput]

## <a name="add-alert"></a><span data-ttu-id="79adc-115">Lägg till avisering</span><span class="sxs-lookup"><span data-stu-id="79adc-115">Add alert</span></span>
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



## <a name="example-1"></a><span data-ttu-id="79adc-116">Exempel 1</span><span class="sxs-lookup"><span data-stu-id="79adc-116">Example 1</span></span>
<span data-ttu-id="79adc-117">E-posta mig om serverns svar på HTTP-begäran var i genomsnitt över 5 minuter är långsammare än 1 sekund.</span><span class="sxs-lookup"><span data-stu-id="79adc-117">Email me if the server's response to HTTP requests, averaged over 5 minutes, is slower than 1 second.</span></span> <span data-ttu-id="79adc-118">Application Insights-resurs kallas IceCreamWebApp och den är i resursgruppen Fabrikam.</span><span class="sxs-lookup"><span data-stu-id="79adc-118">My Application Insights resource is called IceCreamWebApp, and it is in resource group Fabrikam.</span></span> <span data-ttu-id="79adc-119">Jag är ägare till Azure-prenumerationen.</span><span class="sxs-lookup"><span data-stu-id="79adc-119">I am the owner of the Azure subscription.</span></span>

<span data-ttu-id="79adc-120">GUID är prenumerations-ID (inte instrumentation nyckeln för programmet).</span><span class="sxs-lookup"><span data-stu-id="79adc-120">The GUID is the subscription ID (not the instrumentation key of the application).</span></span>

    Add-AlertRule -Name "slow responses" `
     -Description "email me if the server responds slowly" `
     -ResourceGroup "Fabrikam" `
     -ResourceId "/subscriptions/00000000-0000-0000-0000-000000000000/resourcegroups/Fabrikam/providers/microsoft.insights/components/IceCreamWebApp" `
     -MetricName "request.duration" `
     -Operator GreaterThan `
     -Threshold 1 `
     -WindowSize 00:05:00 `
     -SendEmailToServiceOwners `
     -Location "East US" -RuleType Metric

## <a name="example-2"></a><span data-ttu-id="79adc-121">Exempel 2</span><span class="sxs-lookup"><span data-stu-id="79adc-121">Example 2</span></span>
<span data-ttu-id="79adc-122">Jag har ett program som jag använder [TrackMetric()](app-insights-api-custom-events-metrics.md#trackmetric) att rapportera ett mått med namnet ”salesPerHour”.</span><span class="sxs-lookup"><span data-stu-id="79adc-122">I have an application in which I use [TrackMetric()](app-insights-api-custom-events-metrics.md#trackmetric) to report a metric named "salesPerHour."</span></span> <span data-ttu-id="79adc-123">Skicka ett e-postmeddelande till Mina kollegor om ”salesPerHour” sjunker under 100, ett genomsnitt räknas ut under 24 timmar.</span><span class="sxs-lookup"><span data-stu-id="79adc-123">Send an email to my colleagues if "salesPerHour" drops below 100, averaged over 24 hours.</span></span>

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

<span data-ttu-id="79adc-124">Samma regel kan användas för det mått som rapporteras med hjälp av den [mätning parametern](app-insights-api-custom-events-metrics.md#properties) för spårning av ett annat anrop till exempel TrackEvent eller trackPageView.</span><span class="sxs-lookup"><span data-stu-id="79adc-124">The same rule can be used for the metric reported by using the [measurement parameter](app-insights-api-custom-events-metrics.md#properties) of another tracking call such as TrackEvent or trackPageView.</span></span>

## <a name="metric-names"></a><span data-ttu-id="79adc-125">Tjänstmåttets namn</span><span class="sxs-lookup"><span data-stu-id="79adc-125">Metric names</span></span>
| <span data-ttu-id="79adc-126">Måttnamnet</span><span class="sxs-lookup"><span data-stu-id="79adc-126">Metric name</span></span> | <span data-ttu-id="79adc-127">Skärmnamn på</span><span class="sxs-lookup"><span data-stu-id="79adc-127">Screen name</span></span> | <span data-ttu-id="79adc-128">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="79adc-128">Description</span></span> |
| --- | --- | --- |
| `basicExceptionBrowser.count` |<span data-ttu-id="79adc-129">Webbläsarundantag</span><span class="sxs-lookup"><span data-stu-id="79adc-129">Browser exceptions</span></span> |<span data-ttu-id="79adc-130">Antal undantagsfel utan felhantering utlöstes i webbläsaren.</span><span class="sxs-lookup"><span data-stu-id="79adc-130">Count of uncaught exceptions thrown in the browser.</span></span> |
| `basicExceptionServer.count` |<span data-ttu-id="79adc-131">Undantag för</span><span class="sxs-lookup"><span data-stu-id="79adc-131">Server exceptions</span></span> |<span data-ttu-id="79adc-132">Antal undantagsfel utan felhantering utlöstes av appen</span><span class="sxs-lookup"><span data-stu-id="79adc-132">Count of unhandled exceptions thrown by the app</span></span> |
| `clientPerformance.clientProcess.value` |<span data-ttu-id="79adc-133">Bearbetningstid för klienten</span><span class="sxs-lookup"><span data-stu-id="79adc-133">Client processing time</span></span> |<span data-ttu-id="79adc-134">Tiden mellan tar emot de sista byten av ett dokument till dess att DOM har lästs.</span><span class="sxs-lookup"><span data-stu-id="79adc-134">Time between receiving the last byte of a document until the DOM is loaded.</span></span> <span data-ttu-id="79adc-135">Asynkrona förfrågningar kan fortfarande vara under bearbetning.</span><span class="sxs-lookup"><span data-stu-id="79adc-135">Async requests may still be processing.</span></span> |
| `clientPerformance.networkConnection.value` |<span data-ttu-id="79adc-136">Nätverket Anslut sidinläsningstiden</span><span class="sxs-lookup"><span data-stu-id="79adc-136">Page load network connect time</span></span> |<span data-ttu-id="79adc-137">Tid som webbläsaren tar för att ansluta till nätverket.</span><span class="sxs-lookup"><span data-stu-id="79adc-137">Time the browser takes to connect to the network.</span></span> <span data-ttu-id="79adc-138">Kan vara 0 om cachelagrade.</span><span class="sxs-lookup"><span data-stu-id="79adc-138">Can be 0 if cached.</span></span> |
| `clientPerformance.receiveRequest.value` |<span data-ttu-id="79adc-139">Ta emot svarstid</span><span class="sxs-lookup"><span data-stu-id="79adc-139">Receiving response time</span></span> |<span data-ttu-id="79adc-140">Tiden mellan webbläsare som begäran skickades till togs emot svar.</span><span class="sxs-lookup"><span data-stu-id="79adc-140">Time between browser sending request to starting to receive response.</span></span> |
| `clientPerformance.sendRequest.value` |<span data-ttu-id="79adc-141">Skicka tid för begäran</span><span class="sxs-lookup"><span data-stu-id="79adc-141">Send request time</span></span> |<span data-ttu-id="79adc-142">Tid som webbläsaren att skicka begäran.</span><span class="sxs-lookup"><span data-stu-id="79adc-142">Time taken by browser to send request.</span></span> |
| `clientPerformance.total.value` |<span data-ttu-id="79adc-143">Sidhämtningstid</span><span class="sxs-lookup"><span data-stu-id="79adc-143">Browser page load time</span></span> |<span data-ttu-id="79adc-144">Tiden från användarförfrågan till dess att DOM, formatmallar, skript och bilder har lästs in.</span><span class="sxs-lookup"><span data-stu-id="79adc-144">Time from user request until DOM, stylesheets, scripts and images are loaded.</span></span> |
| `performanceCounter.available_bytes.value` |<span data-ttu-id="79adc-145">Tillgängligt minne</span><span class="sxs-lookup"><span data-stu-id="79adc-145">Available memory</span></span> |<span data-ttu-id="79adc-146">Fysiskt minne som är direkt tillgängligt för en process eller för systemanvändning.</span><span class="sxs-lookup"><span data-stu-id="79adc-146">Physical memory immediately available for a process or for system use.</span></span> |
| `performanceCounter.io_data_bytes_per_sec.value` |<span data-ttu-id="79adc-147">Process-i/o-hastighet</span><span class="sxs-lookup"><span data-stu-id="79adc-147">Process IO Rate</span></span> |<span data-ttu-id="79adc-148">Totalt antal byte per sekund som läses och skrivs till filer, nätverk och enheter.</span><span class="sxs-lookup"><span data-stu-id="79adc-148">Total bytes per second read and written to files, network and devices.</span></span> |
| `performanceCounter.number_of_exceps_thrown_per_sec.value` |<span data-ttu-id="79adc-149">hastighet för undantag</span><span class="sxs-lookup"><span data-stu-id="79adc-149">exception rate</span></span> |<span data-ttu-id="79adc-150">Undantag per sekund.</span><span class="sxs-lookup"><span data-stu-id="79adc-150">Exceptions thrown per second.</span></span> |
| `performanceCounter.percentage_processor_time.value` |<span data-ttu-id="79adc-151">Processen CPU</span><span class="sxs-lookup"><span data-stu-id="79adc-151">Process CPU</span></span> |<span data-ttu-id="79adc-152">Procentandelen av förfluten tid som alla processens trådar använda processorn för att köra instruktioner för hur program.</span><span class="sxs-lookup"><span data-stu-id="79adc-152">The percentage of elapsed time of all process threads used by the processor to execution instructions for the applications process.</span></span> |
| `performanceCounter.percentage_processor_total.value` |<span data-ttu-id="79adc-153">Processortid</span><span class="sxs-lookup"><span data-stu-id="79adc-153">Processor time</span></span> |<span data-ttu-id="79adc-154">Procentandel av tiden som processorn ägnat åt icke-inaktiva trådar.</span><span class="sxs-lookup"><span data-stu-id="79adc-154">The percentage of time that the processor spends in non-Idle threads.</span></span> |
| `performanceCounter.process_private_bytes.value` |<span data-ttu-id="79adc-155">Processen för privata byte</span><span class="sxs-lookup"><span data-stu-id="79adc-155">Process private bytes</span></span> |<span data-ttu-id="79adc-156">Minne som tilldelats exklusivt för att övervaka programprocesser.</span><span class="sxs-lookup"><span data-stu-id="79adc-156">Memory exclusively assigned to the monitored application's processes.</span></span> |
| `performanceCounter.request_execution_time.value` |<span data-ttu-id="79adc-157">Körningstid för ASP.NET-begäran</span><span class="sxs-lookup"><span data-stu-id="79adc-157">ASP.NET request execution time</span></span> |<span data-ttu-id="79adc-158">Körningstid för den senaste begäranden.</span><span class="sxs-lookup"><span data-stu-id="79adc-158">Execution time of the most recent request.</span></span> |
| `performanceCounter.requests_in_application_queue.value` |<span data-ttu-id="79adc-159">ASP.NET-begäranden i kö för körning</span><span class="sxs-lookup"><span data-stu-id="79adc-159">ASP.NET requests in execution queue</span></span> |<span data-ttu-id="79adc-160">Längden på programbegärandekön.</span><span class="sxs-lookup"><span data-stu-id="79adc-160">Length of the application request queue.</span></span> |
| `performanceCounter.requests_per_sec.value` |<span data-ttu-id="79adc-161">ASP.NET-begärandehastighet</span><span class="sxs-lookup"><span data-stu-id="79adc-161">ASP.NET request rate</span></span> |<span data-ttu-id="79adc-162">Hastighet för alla förfrågningar till programmet per sekund från ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="79adc-162">Rate of all requests to the application per second from ASP.NET.</span></span> |
| `remoteDependencyFailed.durationMetric.count` |<span data-ttu-id="79adc-163">Beroende fel</span><span class="sxs-lookup"><span data-stu-id="79adc-163">Dependency failures</span></span> |<span data-ttu-id="79adc-164">Antal misslyckade anrop gjorda av serverprogrammet till externa resurser.</span><span class="sxs-lookup"><span data-stu-id="79adc-164">Count of failed calls made by the server application to external resources.</span></span> |
| `request.duration` |<span data-ttu-id="79adc-165">Serversvarstid</span><span class="sxs-lookup"><span data-stu-id="79adc-165">Server response time</span></span> |<span data-ttu-id="79adc-166">Tiden mellan ta emot en HTTP-begäran och slutför skickar svar.</span><span class="sxs-lookup"><span data-stu-id="79adc-166">Time between receiving an HTTP request and finishing sending the response.</span></span> |
| `request.rate` |<span data-ttu-id="79adc-167">Begärandehastighet</span><span class="sxs-lookup"><span data-stu-id="79adc-167">Request rate</span></span> |<span data-ttu-id="79adc-168">Hastighet för alla förfrågningar till programmet per sekund.</span><span class="sxs-lookup"><span data-stu-id="79adc-168">Rate of all requests to the application per second.</span></span> |
| `requestFailed.count` |<span data-ttu-id="79adc-169">Misslyckade begäranden</span><span class="sxs-lookup"><span data-stu-id="79adc-169">Failed requests</span></span> |<span data-ttu-id="79adc-170">Antal HTTP-begäranden som resulterade i en svarskoden > = 400</span><span class="sxs-lookup"><span data-stu-id="79adc-170">Count of HTTP requests that resulted in a response code >= 400</span></span> |
| `view.count` |<span data-ttu-id="79adc-171">Sidvisningar</span><span class="sxs-lookup"><span data-stu-id="79adc-171">Page views</span></span> |<span data-ttu-id="79adc-172">Antal klientens användarförfrågningar för en webbsida.</span><span class="sxs-lookup"><span data-stu-id="79adc-172">Count of client user requests for a web page.</span></span> <span data-ttu-id="79adc-173">Syntetisk trafik filtreras bort.</span><span class="sxs-lookup"><span data-stu-id="79adc-173">Synthetic traffic is filtered out.</span></span> |
| <span data-ttu-id="79adc-174">{din anpassade mått name}</span><span class="sxs-lookup"><span data-stu-id="79adc-174">{your custom metric name}</span></span> |<span data-ttu-id="79adc-175">{Måttnamnet}</span><span class="sxs-lookup"><span data-stu-id="79adc-175">{Your metric name}</span></span> |<span data-ttu-id="79adc-176">Värdet för din mått som rapporteras av [TrackMetric](app-insights-api-custom-events-metrics.md#trackmetric) eller i den [mätningar parametern för ett anrop för spårning av](app-insights-api-custom-events-metrics.md#properties).</span><span class="sxs-lookup"><span data-stu-id="79adc-176">Your metric value reported by [TrackMetric](app-insights-api-custom-events-metrics.md#trackmetric) or in the [measurements parameter of a tracking call](app-insights-api-custom-events-metrics.md#properties).</span></span> |

<span data-ttu-id="79adc-177">Mätvärdena som skickas av olika telemetri moduler:</span><span class="sxs-lookup"><span data-stu-id="79adc-177">The metrics are sent by different telemetry modules:</span></span>

| <span data-ttu-id="79adc-178">Mått grupp</span><span class="sxs-lookup"><span data-stu-id="79adc-178">Metric group</span></span> | <span data-ttu-id="79adc-179">Insamlaren modul</span><span class="sxs-lookup"><span data-stu-id="79adc-179">Collector module</span></span> |
| --- | --- |
| <span data-ttu-id="79adc-180">basicExceptionBrowser,</span><span class="sxs-lookup"><span data-stu-id="79adc-180">basicExceptionBrowser,</span></span><br/><span data-ttu-id="79adc-181">clientPerformance,</span><span class="sxs-lookup"><span data-stu-id="79adc-181">clientPerformance,</span></span><br/><span data-ttu-id="79adc-182">vy</span><span class="sxs-lookup"><span data-stu-id="79adc-182">view</span></span> |[<span data-ttu-id="79adc-183">Webbläsaren JavaScript</span><span class="sxs-lookup"><span data-stu-id="79adc-183">Browser JavaScript</span></span>](app-insights-javascript.md) |
| <span data-ttu-id="79adc-184">performanceCounter</span><span class="sxs-lookup"><span data-stu-id="79adc-184">performanceCounter</span></span> |[<span data-ttu-id="79adc-185">Prestanda</span><span class="sxs-lookup"><span data-stu-id="79adc-185">Performance</span></span>](app-insights-configuration-with-applicationinsights-config.md) |
| <span data-ttu-id="79adc-186">remoteDependencyFailed</span><span class="sxs-lookup"><span data-stu-id="79adc-186">remoteDependencyFailed</span></span> |[<span data-ttu-id="79adc-187">Beroende</span><span class="sxs-lookup"><span data-stu-id="79adc-187">Dependency</span></span>](app-insights-configuration-with-applicationinsights-config.md) |
| <span data-ttu-id="79adc-188">begäran</span><span class="sxs-lookup"><span data-stu-id="79adc-188">request,</span></span><br/><span data-ttu-id="79adc-189">requestFailed</span><span class="sxs-lookup"><span data-stu-id="79adc-189">requestFailed</span></span> |[<span data-ttu-id="79adc-190">Serverbegäran</span><span class="sxs-lookup"><span data-stu-id="79adc-190">Server request</span></span>](app-insights-configuration-with-applicationinsights-config.md) |

## <a name="webhooks"></a><span data-ttu-id="79adc-191">Webhooks</span><span class="sxs-lookup"><span data-stu-id="79adc-191">Webhooks</span></span>
<span data-ttu-id="79adc-192">Du kan [automatisera dina svar på en avisering](../monitoring-and-diagnostics/insights-webhooks-alerts.md).</span><span class="sxs-lookup"><span data-stu-id="79adc-192">You can [automate your response to an alert](../monitoring-and-diagnostics/insights-webhooks-alerts.md).</span></span> <span data-ttu-id="79adc-193">Azure anropar en webbadress som du väljer när en avisering utlöses.</span><span class="sxs-lookup"><span data-stu-id="79adc-193">Azure will call a web address of your choice when an alert is raised.</span></span>

## <a name="see-also"></a><span data-ttu-id="79adc-194">Se även</span><span class="sxs-lookup"><span data-stu-id="79adc-194">See also</span></span>
* [<span data-ttu-id="79adc-195">Skript för att konfigurera Application Insights</span><span class="sxs-lookup"><span data-stu-id="79adc-195">Script to configure Application Insights</span></span>](app-insights-powershell-script-create-resource.md)
* [<span data-ttu-id="79adc-196">Skapa Application Insights och testa webbresurser från mallar</span><span class="sxs-lookup"><span data-stu-id="79adc-196">Create Application Insights and web test resources from templates</span></span>](app-insights-powershell.md)
* [<span data-ttu-id="79adc-197">Automatisera koppling Microsoft Azure-diagnostik till Application Insights</span><span class="sxs-lookup"><span data-stu-id="79adc-197">Automate coupling Microsoft Azure Diagnostics to Application Insights</span></span>](app-insights-powershell-azure-diagnostics.md)
* [<span data-ttu-id="79adc-198">Automatisera dina svar på en avisering</span><span class="sxs-lookup"><span data-stu-id="79adc-198">Automate your response to an alert</span></span>](../monitoring-and-diagnostics/insights-webhooks-alerts.md)
