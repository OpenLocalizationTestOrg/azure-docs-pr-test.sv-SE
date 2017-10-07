---
title: "aaaIP adresser som används av Application Insights | Microsoft Docs"
description: "Servern brandväggsundantag som krävs för Application Insights"
services: application-insights
documentationcenter: .net
author: CFreemanwa
manager: carmonm
ms.assetid: 44d989f8-bae9-40ff-bfd5-8343d3e59358
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 8/11/2017
ms.author: bwren
ms.openlocfilehash: 2c101b8da2ba9594fbff607f4f7551cda80c3c25
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="ip-addresses-used-by-application-insights"></a><span data-ttu-id="c0d19-103">IP-adresser som används av Application Insights</span><span class="sxs-lookup"><span data-stu-id="c0d19-103">IP addresses used by Application Insights</span></span>
<span data-ttu-id="c0d19-104">Hej [Azure Application Insights](app-insights-overview.md) tjänsten använder ett antal IP-adresser.</span><span class="sxs-lookup"><span data-stu-id="c0d19-104">hello [Azure Application Insights](app-insights-overview.md) service uses a number of IP addresses.</span></span> <span data-ttu-id="c0d19-105">Du kan behöva tooknow adresserna om hello-app som du övervakar finns bakom en brandvägg.</span><span class="sxs-lookup"><span data-stu-id="c0d19-105">You might need tooknow these addresses if hello app that you are monitoring is hosted behind a firewall.</span></span>

> [!NOTE]
> <span data-ttu-id="c0d19-106">Men dessa adresser är statiska, det är möjligt att vi behöver toochange dem från tid tootime.</span><span class="sxs-lookup"><span data-stu-id="c0d19-106">Although these addresses are static, it's possible that we will need toochange them from time tootime.</span></span>
> 
> 

## <a name="outgoing-ports"></a><span data-ttu-id="c0d19-107">Utgående portar</span><span class="sxs-lookup"><span data-stu-id="c0d19-107">Outgoing ports</span></span>
<span data-ttu-id="c0d19-108">Du behöver tooopen vissa utgående portar i serverns brandvägg tooallow hello Application Insights SDK och/eller statusövervakaren toosend data toohello portal:</span><span class="sxs-lookup"><span data-stu-id="c0d19-108">You need tooopen some outgoing ports in your server's firewall tooallow hello Application Insights SDK and/or Status Monitor toosend data toohello portal:</span></span>

| <span data-ttu-id="c0d19-109">Syfte</span><span class="sxs-lookup"><span data-stu-id="c0d19-109">Purpose</span></span> | <span data-ttu-id="c0d19-110">URL: EN</span><span class="sxs-lookup"><span data-stu-id="c0d19-110">URL</span></span> | <span data-ttu-id="c0d19-111">IP-adress</span><span class="sxs-lookup"><span data-stu-id="c0d19-111">IP</span></span> | <span data-ttu-id="c0d19-112">Portar</span><span class="sxs-lookup"><span data-stu-id="c0d19-112">Ports</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="c0d19-113">Telemetri</span><span class="sxs-lookup"><span data-stu-id="c0d19-113">Telemetry</span></span> |<span data-ttu-id="c0d19-114">DC.Services.visualstudio.com</span><span class="sxs-lookup"><span data-stu-id="c0d19-114">dc.services.visualstudio.com</span></span><br/><span data-ttu-id="c0d19-115">DC.applicationinsights.microsoft.com</span><span class="sxs-lookup"><span data-stu-id="c0d19-115">dc.applicationinsights.microsoft.com</span></span> |<span data-ttu-id="c0d19-116">40.114.241.141</span><span class="sxs-lookup"><span data-stu-id="c0d19-116">40.114.241.141</span></span><br/><span data-ttu-id="c0d19-117">104.45.136.42</span><span class="sxs-lookup"><span data-stu-id="c0d19-117">104.45.136.42</span></span><br/><span data-ttu-id="c0d19-118">40.84.189.107</span><span class="sxs-lookup"><span data-stu-id="c0d19-118">40.84.189.107</span></span><br/><span data-ttu-id="c0d19-119">168.63.242.221</span><span class="sxs-lookup"><span data-stu-id="c0d19-119">168.63.242.221</span></span><br/><span data-ttu-id="c0d19-120">52.167.221.184</span><span class="sxs-lookup"><span data-stu-id="c0d19-120">52.167.221.184</span></span><br/><span data-ttu-id="c0d19-121">52.169.64.244</span><span class="sxs-lookup"><span data-stu-id="c0d19-121">52.169.64.244</span></span> |<span data-ttu-id="c0d19-122">443</span><span class="sxs-lookup"><span data-stu-id="c0d19-122">443</span></span> |
| <span data-ttu-id="c0d19-123">Direktsänd dataström mått</span><span class="sxs-lookup"><span data-stu-id="c0d19-123">Live Metrics Stream</span></span> |<span data-ttu-id="c0d19-124">rt.Services.visualstudio.com</span><span class="sxs-lookup"><span data-stu-id="c0d19-124">rt.services.visualstudio.com</span></span><br/><span data-ttu-id="c0d19-125">rt.applicationinsights.microsoft.com</span><span class="sxs-lookup"><span data-stu-id="c0d19-125">rt.applicationinsights.microsoft.com</span></span> |<span data-ttu-id="c0d19-126">23.96.28.38</span><span class="sxs-lookup"><span data-stu-id="c0d19-126">23.96.28.38</span></span><br/><span data-ttu-id="c0d19-127">13.92.40.198</span><span class="sxs-lookup"><span data-stu-id="c0d19-127">13.92.40.198</span></span> |<span data-ttu-id="c0d19-128">443</span><span class="sxs-lookup"><span data-stu-id="c0d19-128">443</span></span> |
| <span data-ttu-id="c0d19-129">Internt telemetri</span><span class="sxs-lookup"><span data-stu-id="c0d19-129">Internal Telemetry</span></span> |<span data-ttu-id="c0d19-130">breeze.aimon.applicationinsights.IO</span><span class="sxs-lookup"><span data-stu-id="c0d19-130">breeze.aimon.applicationinsights.io</span></span> |<span data-ttu-id="c0d19-131">52.161.11.71</span><span class="sxs-lookup"><span data-stu-id="c0d19-131">52.161.11.71</span></span> |<span data-ttu-id="c0d19-132">443</span><span class="sxs-lookup"><span data-stu-id="c0d19-132">443</span></span> |

## <a name="status-monitor"></a><span data-ttu-id="c0d19-133">Statusövervakaren</span><span class="sxs-lookup"><span data-stu-id="c0d19-133">Status Monitor</span></span>
<span data-ttu-id="c0d19-134">Status Monitor-konfiguration – behövs bara när du gör ändringar.</span><span class="sxs-lookup"><span data-stu-id="c0d19-134">Status Monitor Configuration - needed only when making changes.</span></span>

| <span data-ttu-id="c0d19-135">Syfte</span><span class="sxs-lookup"><span data-stu-id="c0d19-135">Purpose</span></span> | <span data-ttu-id="c0d19-136">URL: EN</span><span class="sxs-lookup"><span data-stu-id="c0d19-136">URL</span></span> | <span data-ttu-id="c0d19-137">IP-adress</span><span class="sxs-lookup"><span data-stu-id="c0d19-137">IP</span></span> | <span data-ttu-id="c0d19-138">Portar</span><span class="sxs-lookup"><span data-stu-id="c0d19-138">Ports</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="c0d19-139">Konfiguration</span><span class="sxs-lookup"><span data-stu-id="c0d19-139">Configuration</span></span> |`management.core.windows.net` | |`443` |
| <span data-ttu-id="c0d19-140">Konfiguration</span><span class="sxs-lookup"><span data-stu-id="c0d19-140">Configuration</span></span> |`management.azure.com` | |`443` |
| <span data-ttu-id="c0d19-141">Konfiguration</span><span class="sxs-lookup"><span data-stu-id="c0d19-141">Configuration</span></span> |`login.windows.net` | |`443` |
| <span data-ttu-id="c0d19-142">Konfiguration</span><span class="sxs-lookup"><span data-stu-id="c0d19-142">Configuration</span></span> |`login.microsoftonline.com` | |`443` |
| <span data-ttu-id="c0d19-143">Konfiguration</span><span class="sxs-lookup"><span data-stu-id="c0d19-143">Configuration</span></span> |`secure.aadcdn.microsoftonline-p.com` | |`443` |
| <span data-ttu-id="c0d19-144">Konfiguration</span><span class="sxs-lookup"><span data-stu-id="c0d19-144">Configuration</span></span> |`auth.gfx.ms` | |`443` |
| <span data-ttu-id="c0d19-145">Konfiguration</span><span class="sxs-lookup"><span data-stu-id="c0d19-145">Configuration</span></span> |`login.live.com` | |`443` |
| <span data-ttu-id="c0d19-146">Installation</span><span class="sxs-lookup"><span data-stu-id="c0d19-146">Installation</span></span> |`packages.nuget.org` | |`443` |

## <a name="hockeyapp"></a><span data-ttu-id="c0d19-147">HockeyApp</span><span class="sxs-lookup"><span data-stu-id="c0d19-147">HockeyApp</span></span>
| <span data-ttu-id="c0d19-148">Syfte</span><span class="sxs-lookup"><span data-stu-id="c0d19-148">Purpose</span></span> | <span data-ttu-id="c0d19-149">URL: EN</span><span class="sxs-lookup"><span data-stu-id="c0d19-149">URL</span></span> | <span data-ttu-id="c0d19-150">IP-adress</span><span class="sxs-lookup"><span data-stu-id="c0d19-150">IP</span></span> | <span data-ttu-id="c0d19-151">Portar</span><span class="sxs-lookup"><span data-stu-id="c0d19-151">Ports</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="c0d19-152">Kraschdata</span><span class="sxs-lookup"><span data-stu-id="c0d19-152">Crash data</span></span> |<span data-ttu-id="c0d19-153">Gate.hockeyapp.NET</span><span class="sxs-lookup"><span data-stu-id="c0d19-153">gate.hockeyapp.net</span></span> |<span data-ttu-id="c0d19-154">104.45.136.42</span><span class="sxs-lookup"><span data-stu-id="c0d19-154">104.45.136.42</span></span> |<span data-ttu-id="c0d19-155">80, 443</span><span class="sxs-lookup"><span data-stu-id="c0d19-155">80, 443</span></span> |

## <a name="availability-tests"></a><span data-ttu-id="c0d19-156">Tillgänglighetstester</span><span class="sxs-lookup"><span data-stu-id="c0d19-156">Availability tests</span></span>
<span data-ttu-id="c0d19-157">Det här är hello lista med adresser som [tillgänglighet webbtester](app-insights-monitor-web-app-availability.md) körs.</span><span class="sxs-lookup"><span data-stu-id="c0d19-157">This is hello list of addresses from which [availability web tests](app-insights-monitor-web-app-availability.md) are run.</span></span> <span data-ttu-id="c0d19-158">Om du vill toorun webbtester på din app men webbservern är begränsad tooserving specifika klienter, måste toopermit inkommande trafik från våra servrar för test av tillgänglighet.</span><span class="sxs-lookup"><span data-stu-id="c0d19-158">If you want toorun web tests on your app, but your web server is restricted tooserving specific clients, then you will have toopermit incoming traffic from our availability test servers.</span></span>

<span data-ttu-id="c0d19-159">Öppna portarna 80 (http) och 443 (https) för inkommande trafik från dessa adresser (IP-adresser är grupperade efter plats):</span><span class="sxs-lookup"><span data-stu-id="c0d19-159">Open ports 80 (http) and 443 (https) for incoming traffic from these addresses (IP addresses are grouped by location):</span></span>

```
AU : Sydney
13.70.83.252
13.75.150.96
13.75.153.9
13.75.158.185
BR : Sao Paulo
191.232.32.122
191.232.172.45
191.232.176.218
191.232.191.225
CH : Zurich
94.245.66.43
94.245.66.44
94.245.66.45
94.245.66.48
FR : Paris
94.245.72.44
94.245.72.45
94.245.72.46
94.245.72.49
94.245.72.52
94.245.72.53
HK : Hong Kong
13.75.121.122
23.99.115.153
23.99.123.38
23.102.232.186
52.175.38.49
52.175.39.103
IE : Dublin
13.74.184.101
13.74.185.160
40.69.200.198
52.164.224.46
52.169.12.203
52.169.14.11
52.169.237.149
52.178.183.105
JP : Kawaguchi
52.243.33.33
52.243.33.141
52.243.35.253
52.243.41.117
NL : Amsterdam
52.174.166.113
52.174.178.96
52.174.31.140
52.174.35.14
52.178.104.23
52.178.109.190
52.178.111.139
52.233.166.221
RU : Moscow
94.245.82.32
94.245.82.33
94.245.82.37
94.245.82.38
SE : Stockholm
94.245.78.40
94.245.78.41
94.245.78.42
94.245.78.45
SG : Singapore
52.187.29.7
52.187.179.17
52.187.76.248
52.187.43.24
52.163.57.91
52.187.30.120
US : CA-San Jose
104.45.228.236
104.45.237.251
13.64.152.110
13.64.156.54
13.64.232.251
13.64.236.105
13.91.94.59
40.118.131.182
40.83.189.192
40.83.215.122
US : FL-Miami
65.54.78.49
65.54.78.50
65.54.78.51
65.54.78.54
65.54.78.57
65.54.78.58
65.54.78.59
65.54.78.60
US : IL-Chicago
23.96.247.139
23.96.249.113
52.162.124.242
52.162.126.139
52.162.241.147
52.162.246.222
52.162.247.136
52.237.153.231
52.237.154.216
52.237.156.14
52.237.157.218
52.237.157.37
US : TX-San Antonio
104.210.145.106
13.84.176.24
13.84.49.16
13.85.11.137
13.85.26.248
13.85.69.145
52.171.136.162
52.171.141.253
52.171.57.172
52.171.58.140
US : VA-Ashburn
13.82.218.95
13.90.96.71
13.90.98.52
13.92.137.70
40.85.187.235
40.87.61.61
52.168.8.247
52.170.38.79
52.170.80.61
52.179.9.26

```  

## <a name="data-access-api"></a><span data-ttu-id="c0d19-160">API för dataåtkomst</span><span class="sxs-lookup"><span data-stu-id="c0d19-160">Data access API</span></span>
| <span data-ttu-id="c0d19-161">Syfte</span><span class="sxs-lookup"><span data-stu-id="c0d19-161">Purpose</span></span> | <span data-ttu-id="c0d19-162">URI: N</span><span class="sxs-lookup"><span data-stu-id="c0d19-162">URI</span></span> | <span data-ttu-id="c0d19-163">IP-adress</span><span class="sxs-lookup"><span data-stu-id="c0d19-163">IP</span></span> | <span data-ttu-id="c0d19-164">Portar</span><span class="sxs-lookup"><span data-stu-id="c0d19-164">Ports</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="c0d19-165">API</span><span class="sxs-lookup"><span data-stu-id="c0d19-165">API</span></span> |<span data-ttu-id="c0d19-166">API.applicationinsights.IO</span><span class="sxs-lookup"><span data-stu-id="c0d19-166">api.applicationinsights.io</span></span><br/><span data-ttu-id="c0d19-167">api1.applicationinsights.IO</span><span class="sxs-lookup"><span data-stu-id="c0d19-167">api1.applicationinsights.io</span></span><br/><span data-ttu-id="c0d19-168">api2.applicationinsights.IO</span><span class="sxs-lookup"><span data-stu-id="c0d19-168">api2.applicationinsights.io</span></span><br/><span data-ttu-id="c0d19-169">api3.applicationinsights.IO</span><span class="sxs-lookup"><span data-stu-id="c0d19-169">api3.applicationinsights.io</span></span><br/><span data-ttu-id="c0d19-170">api4.applicationinsights.IO</span><span class="sxs-lookup"><span data-stu-id="c0d19-170">api4.applicationinsights.io</span></span><br/><span data-ttu-id="c0d19-171">api5.applicationinsights.IO</span><span class="sxs-lookup"><span data-stu-id="c0d19-171">api5.applicationinsights.io</span></span> |<span data-ttu-id="c0d19-172">13.82.26.252</span><span class="sxs-lookup"><span data-stu-id="c0d19-172">13.82.26.252</span></span><br/><span data-ttu-id="c0d19-173">40.76.213.73</span><span class="sxs-lookup"><span data-stu-id="c0d19-173">40.76.213.73</span></span> |<span data-ttu-id="c0d19-174">80,443</span><span class="sxs-lookup"><span data-stu-id="c0d19-174">80,443</span></span> |
| <span data-ttu-id="c0d19-175">API-dokumentation</span><span class="sxs-lookup"><span data-stu-id="c0d19-175">API docs</span></span> |<span data-ttu-id="c0d19-176">dev.applicationinsights.IO</span><span class="sxs-lookup"><span data-stu-id="c0d19-176">dev.applicationinsights.io</span></span><br/><span data-ttu-id="c0d19-177">dev.applicationinsights.microsoft.com</span><span class="sxs-lookup"><span data-stu-id="c0d19-177">dev.applicationinsights.microsoft.com</span></span><br/><span data-ttu-id="c0d19-178">dev.aisvc.visualstudio.com</span><span class="sxs-lookup"><span data-stu-id="c0d19-178">dev.aisvc.visualstudio.com</span></span><br/><span data-ttu-id="c0d19-179">www.applicationinsights.IO</span><span class="sxs-lookup"><span data-stu-id="c0d19-179">www.applicationinsights.io</span></span><br/><span data-ttu-id="c0d19-180">www.applicationinsights.microsoft.com</span><span class="sxs-lookup"><span data-stu-id="c0d19-180">www.applicationinsights.microsoft.com</span></span><br/><span data-ttu-id="c0d19-181">www.aisvc.visualstudio.com</span><span class="sxs-lookup"><span data-stu-id="c0d19-181">www.aisvc.visualstudio.com</span></span> |<span data-ttu-id="c0d19-182">13.82.24.149</span><span class="sxs-lookup"><span data-stu-id="c0d19-182">13.82.24.149</span></span><br/><span data-ttu-id="c0d19-183">40.114.82.10</span><span class="sxs-lookup"><span data-stu-id="c0d19-183">40.114.82.10</span></span> |<span data-ttu-id="c0d19-184">80,443</span><span class="sxs-lookup"><span data-stu-id="c0d19-184">80,443</span></span> |
| <span data-ttu-id="c0d19-185">Internt API</span><span class="sxs-lookup"><span data-stu-id="c0d19-185">Internal API</span></span> |<span data-ttu-id="c0d19-186">aigs.aisvc.visualstudio.com</span><span class="sxs-lookup"><span data-stu-id="c0d19-186">aigs.aisvc.visualstudio.com</span></span><br/><span data-ttu-id="c0d19-187">aigs1.aisvc.visualstudio.com</span><span class="sxs-lookup"><span data-stu-id="c0d19-187">aigs1.aisvc.visualstudio.com</span></span><br/><span data-ttu-id="c0d19-188">aigs2.aisvc.visualstudio.com</span><span class="sxs-lookup"><span data-stu-id="c0d19-188">aigs2.aisvc.visualstudio.com</span></span><br/><span data-ttu-id="c0d19-189">aigs3.aisvc.visualstudio.com</span><span class="sxs-lookup"><span data-stu-id="c0d19-189">aigs3.aisvc.visualstudio.com</span></span><br/><span data-ttu-id="c0d19-190">aigs4.aisvc.visualstudio.com</span><span class="sxs-lookup"><span data-stu-id="c0d19-190">aigs4.aisvc.visualstudio.com</span></span><br/><span data-ttu-id="c0d19-191">aigs5.aisvc.visualstudio.com</span><span class="sxs-lookup"><span data-stu-id="c0d19-191">aigs5.aisvc.visualstudio.com</span></span><br/><span data-ttu-id="c0d19-192">aigs6.aisvc.visualstudio.com</span><span class="sxs-lookup"><span data-stu-id="c0d19-192">aigs6.aisvc.visualstudio.com</span></span> |<span data-ttu-id="c0d19-193">dynamisk</span><span class="sxs-lookup"><span data-stu-id="c0d19-193">dynamic</span></span>|<span data-ttu-id="c0d19-194">443</span><span class="sxs-lookup"><span data-stu-id="c0d19-194">443</span></span> |

## <a name="application-insights-analytics"></a><span data-ttu-id="c0d19-195">Application Insights Analytics</span><span class="sxs-lookup"><span data-stu-id="c0d19-195">Application Insights Analytics</span></span>

| <span data-ttu-id="c0d19-196">Syfte</span><span class="sxs-lookup"><span data-stu-id="c0d19-196">Purpose</span></span> | <span data-ttu-id="c0d19-197">URI: N</span><span class="sxs-lookup"><span data-stu-id="c0d19-197">URI</span></span> | <span data-ttu-id="c0d19-198">IP-adress</span><span class="sxs-lookup"><span data-stu-id="c0d19-198">IP</span></span> | <span data-ttu-id="c0d19-199">Portar</span><span class="sxs-lookup"><span data-stu-id="c0d19-199">Ports</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="c0d19-200">Analytics-portalen</span><span class="sxs-lookup"><span data-stu-id="c0d19-200">Analytics Portal</span></span> | <span data-ttu-id="c0d19-201">Analytics.applicationinsights.IO</span><span class="sxs-lookup"><span data-stu-id="c0d19-201">analytics.applicationinsights.io</span></span> | <span data-ttu-id="c0d19-202">dynamisk</span><span class="sxs-lookup"><span data-stu-id="c0d19-202">dynamic</span></span> | <span data-ttu-id="c0d19-203">80,443</span><span class="sxs-lookup"><span data-stu-id="c0d19-203">80,443</span></span> |
| <span data-ttu-id="c0d19-204">CDN</span><span class="sxs-lookup"><span data-stu-id="c0d19-204">CDN</span></span> | <span data-ttu-id="c0d19-205">applicationanalytics.azureedge.NET</span><span class="sxs-lookup"><span data-stu-id="c0d19-205">applicationanalytics.azureedge.net</span></span> | <span data-ttu-id="c0d19-206">dynamisk</span><span class="sxs-lookup"><span data-stu-id="c0d19-206">dynamic</span></span> | <span data-ttu-id="c0d19-207">80,443</span><span class="sxs-lookup"><span data-stu-id="c0d19-207">80,443</span></span> |
| <span data-ttu-id="c0d19-208">Media CDN</span><span class="sxs-lookup"><span data-stu-id="c0d19-208">Media CDN</span></span> | <span data-ttu-id="c0d19-209">applicationanalyticsmedia.azureedge.NET</span><span class="sxs-lookup"><span data-stu-id="c0d19-209">applicationanalyticsmedia.azureedge.net</span></span> | <span data-ttu-id="c0d19-210">dynamisk</span><span class="sxs-lookup"><span data-stu-id="c0d19-210">dynamic</span></span> | <span data-ttu-id="c0d19-211">80,443</span><span class="sxs-lookup"><span data-stu-id="c0d19-211">80,443</span></span> |

<span data-ttu-id="c0d19-212">Obs! *. applicationinsights.io domän som ägs av Application Insights-teamet.</span><span class="sxs-lookup"><span data-stu-id="c0d19-212">Note: *.applicationinsights.io domain is owned by Application Insights team.</span></span>

## <a name="application-insights-azure-portal-extension"></a><span data-ttu-id="c0d19-213">Application Insights Azure-Portaltillägg</span><span class="sxs-lookup"><span data-stu-id="c0d19-213">Application Insights Azure Portal Extension</span></span>

| <span data-ttu-id="c0d19-214">Syfte</span><span class="sxs-lookup"><span data-stu-id="c0d19-214">Purpose</span></span> | <span data-ttu-id="c0d19-215">URI: N</span><span class="sxs-lookup"><span data-stu-id="c0d19-215">URI</span></span> | <span data-ttu-id="c0d19-216">IP-adress</span><span class="sxs-lookup"><span data-stu-id="c0d19-216">IP</span></span> | <span data-ttu-id="c0d19-217">Portar</span><span class="sxs-lookup"><span data-stu-id="c0d19-217">Ports</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="c0d19-218">Application Insights Extension</span><span class="sxs-lookup"><span data-stu-id="c0d19-218">Application Insights Extension</span></span> | <span data-ttu-id="c0d19-219">stamp2.App.insightsportal.visualstudio.com</span><span class="sxs-lookup"><span data-stu-id="c0d19-219">stamp2.app.insightsportal.visualstudio.com</span></span> | <span data-ttu-id="c0d19-220">dynamisk</span><span class="sxs-lookup"><span data-stu-id="c0d19-220">dynamic</span></span> | <span data-ttu-id="c0d19-221">80,443</span><span class="sxs-lookup"><span data-stu-id="c0d19-221">80,443</span></span> |
| <span data-ttu-id="c0d19-222">Application Insights Extension CDN</span><span class="sxs-lookup"><span data-stu-id="c0d19-222">Application Insights Extension CDN</span></span> | <span data-ttu-id="c0d19-223">insightsportal-prod2-cdn.aisvc.visualstudio.com</span><span class="sxs-lookup"><span data-stu-id="c0d19-223">insightsportal-prod2-cdn.aisvc.visualstudio.com</span></span><br/><span data-ttu-id="c0d19-224">insightsportal-prod2-asiae-cdn.aisvc.visualstudio.com</span><span class="sxs-lookup"><span data-stu-id="c0d19-224">insightsportal-prod2-asiae-cdn.aisvc.visualstudio.com</span></span><br/><span data-ttu-id="c0d19-225">insightsportal-cdn-aimon.applicationinsights.io</span><span class="sxs-lookup"><span data-stu-id="c0d19-225">insightsportal-cdn-aimon.applicationinsights.io</span></span> | <span data-ttu-id="c0d19-226">dynamisk</span><span class="sxs-lookup"><span data-stu-id="c0d19-226">dynamic</span></span> | <span data-ttu-id="c0d19-227">80,443</span><span class="sxs-lookup"><span data-stu-id="c0d19-227">80,443</span></span> |

## <a name="application-insights-sdks"></a><span data-ttu-id="c0d19-228">Application Insights SDK</span><span class="sxs-lookup"><span data-stu-id="c0d19-228">Application Insights SDKs</span></span>

| <span data-ttu-id="c0d19-229">Syfte</span><span class="sxs-lookup"><span data-stu-id="c0d19-229">Purpose</span></span> | <span data-ttu-id="c0d19-230">URI: N</span><span class="sxs-lookup"><span data-stu-id="c0d19-230">URI</span></span> | <span data-ttu-id="c0d19-231">IP-adress</span><span class="sxs-lookup"><span data-stu-id="c0d19-231">IP</span></span> | <span data-ttu-id="c0d19-232">Portar</span><span class="sxs-lookup"><span data-stu-id="c0d19-232">Ports</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="c0d19-233">Application Insights JS SDK CDN</span><span class="sxs-lookup"><span data-stu-id="c0d19-233">Application Insights JS SDK CDN</span></span> | <span data-ttu-id="c0d19-234">az416426.VO.msecnd.NET</span><span class="sxs-lookup"><span data-stu-id="c0d19-234">az416426.vo.msecnd.net</span></span> | <span data-ttu-id="c0d19-235">dynamisk</span><span class="sxs-lookup"><span data-stu-id="c0d19-235">dynamic</span></span> | <span data-ttu-id="c0d19-236">80,443</span><span class="sxs-lookup"><span data-stu-id="c0d19-236">80,443</span></span> |
| <span data-ttu-id="c0d19-237">Application Insights Java SDK</span><span class="sxs-lookup"><span data-stu-id="c0d19-237">Application Insights Java SDK</span></span> | <span data-ttu-id="c0d19-238">aijavasdk.BLOB.Core.Windows.NET</span><span class="sxs-lookup"><span data-stu-id="c0d19-238">aijavasdk.blob.core.windows.net</span></span> | <span data-ttu-id="c0d19-239">dynamisk</span><span class="sxs-lookup"><span data-stu-id="c0d19-239">dynamic</span></span> | <span data-ttu-id="c0d19-240">80,443</span><span class="sxs-lookup"><span data-stu-id="c0d19-240">80,443</span></span> |

## <a name="profiler"></a><span data-ttu-id="c0d19-241">Profilerare</span><span class="sxs-lookup"><span data-stu-id="c0d19-241">Profiler</span></span>

| <span data-ttu-id="c0d19-242">Syfte</span><span class="sxs-lookup"><span data-stu-id="c0d19-242">Purpose</span></span> | <span data-ttu-id="c0d19-243">URI: N</span><span class="sxs-lookup"><span data-stu-id="c0d19-243">URI</span></span> | <span data-ttu-id="c0d19-244">IP-adress</span><span class="sxs-lookup"><span data-stu-id="c0d19-244">IP</span></span> | <span data-ttu-id="c0d19-245">Portar</span><span class="sxs-lookup"><span data-stu-id="c0d19-245">Ports</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="c0d19-246">Agent</span><span class="sxs-lookup"><span data-stu-id="c0d19-246">Agent</span></span> | <span data-ttu-id="c0d19-247">Agent.azureserviceprofiler.NET</span><span class="sxs-lookup"><span data-stu-id="c0d19-247">agent.azureserviceprofiler.net</span></span><br/><span data-ttu-id="c0d19-248">*. agent.azureserviceprofiler.net</span><span class="sxs-lookup"><span data-stu-id="c0d19-248">*.agent.azureserviceprofiler.net</span></span> | <span data-ttu-id="c0d19-249">dynamisk</span><span class="sxs-lookup"><span data-stu-id="c0d19-249">dynamic</span></span> | <span data-ttu-id="c0d19-250">443</span><span class="sxs-lookup"><span data-stu-id="c0d19-250">443</span></span>
| <span data-ttu-id="c0d19-251">Portalen</span><span class="sxs-lookup"><span data-stu-id="c0d19-251">Portal</span></span> | <span data-ttu-id="c0d19-252">gateway.azureserviceprofiler.NET</span><span class="sxs-lookup"><span data-stu-id="c0d19-252">gateway.azureserviceprofiler.net</span></span> | <span data-ttu-id="c0d19-253">dynamisk</span><span class="sxs-lookup"><span data-stu-id="c0d19-253">dynamic</span></span> | <span data-ttu-id="c0d19-254">443</span><span class="sxs-lookup"><span data-stu-id="c0d19-254">443</span></span>
| <span data-ttu-id="c0d19-255">Lagring</span><span class="sxs-lookup"><span data-stu-id="c0d19-255">Storage</span></span> | <span data-ttu-id="c0d19-256">*. core.windows.net</span><span class="sxs-lookup"><span data-stu-id="c0d19-256">*.core.windows.net</span></span> | <span data-ttu-id="c0d19-257">dynamisk</span><span class="sxs-lookup"><span data-stu-id="c0d19-257">dynamic</span></span> | <span data-ttu-id="c0d19-258">443</span><span class="sxs-lookup"><span data-stu-id="c0d19-258">443</span></span>

## <a name="snapshot-debugger"></a><span data-ttu-id="c0d19-259">Felsökning av ögonblicksbild</span><span class="sxs-lookup"><span data-stu-id="c0d19-259">Snapshot Debugger</span></span>

| <span data-ttu-id="c0d19-260">Syfte</span><span class="sxs-lookup"><span data-stu-id="c0d19-260">Purpose</span></span> | <span data-ttu-id="c0d19-261">URI: N</span><span class="sxs-lookup"><span data-stu-id="c0d19-261">URI</span></span> | <span data-ttu-id="c0d19-262">IP-adress</span><span class="sxs-lookup"><span data-stu-id="c0d19-262">IP</span></span> | <span data-ttu-id="c0d19-263">Portar</span><span class="sxs-lookup"><span data-stu-id="c0d19-263">Ports</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="c0d19-264">Agent</span><span class="sxs-lookup"><span data-stu-id="c0d19-264">Agent</span></span> | <span data-ttu-id="c0d19-265">ppe.azureserviceprofiler.NET</span><span class="sxs-lookup"><span data-stu-id="c0d19-265">ppe.azureserviceprofiler.net</span></span><br/><span data-ttu-id="c0d19-266">*. ppe.azureserviceprofiler.net</span><span class="sxs-lookup"><span data-stu-id="c0d19-266">*.ppe.azureserviceprofiler.net</span></span> | <span data-ttu-id="c0d19-267">dynamisk</span><span class="sxs-lookup"><span data-stu-id="c0d19-267">dynamic</span></span> | <span data-ttu-id="c0d19-268">443</span><span class="sxs-lookup"><span data-stu-id="c0d19-268">443</span></span>
| <span data-ttu-id="c0d19-269">Portalen</span><span class="sxs-lookup"><span data-stu-id="c0d19-269">Portal</span></span> | <span data-ttu-id="c0d19-270">ppe.gateway.azureserviceprofiler.NET</span><span class="sxs-lookup"><span data-stu-id="c0d19-270">ppe.gateway.azureserviceprofiler.net</span></span> | <span data-ttu-id="c0d19-271">dynamisk</span><span class="sxs-lookup"><span data-stu-id="c0d19-271">dynamic</span></span> | <span data-ttu-id="c0d19-272">443</span><span class="sxs-lookup"><span data-stu-id="c0d19-272">443</span></span>
| <span data-ttu-id="c0d19-273">Lagring</span><span class="sxs-lookup"><span data-stu-id="c0d19-273">Storage</span></span> | <span data-ttu-id="c0d19-274">*. core.windows.net</span><span class="sxs-lookup"><span data-stu-id="c0d19-274">*.core.windows.net</span></span> | <span data-ttu-id="c0d19-275">dynamisk</span><span class="sxs-lookup"><span data-stu-id="c0d19-275">dynamic</span></span> | <span data-ttu-id="c0d19-276">443</span><span class="sxs-lookup"><span data-stu-id="c0d19-276">443</span></span>
