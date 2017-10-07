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
# <a name="ip-addresses-used-by-application-insights"></a>IP-adresser som används av Application Insights
Hej [Azure Application Insights](app-insights-overview.md) tjänsten använder ett antal IP-adresser. Du kan behöva tooknow adresserna om hello-app som du övervakar finns bakom en brandvägg.

> [!NOTE]
> Men dessa adresser är statiska, det är möjligt att vi behöver toochange dem från tid tootime.
> 
> 

## <a name="outgoing-ports"></a>Utgående portar
Du behöver tooopen vissa utgående portar i serverns brandvägg tooallow hello Application Insights SDK och/eller statusövervakaren toosend data toohello portal:

| Syfte | URL: EN | IP-adress | Portar |
| --- | --- | --- | --- |
| Telemetri |DC.Services.visualstudio.com<br/>DC.applicationinsights.microsoft.com |40.114.241.141<br/>104.45.136.42<br/>40.84.189.107<br/>168.63.242.221<br/>52.167.221.184<br/>52.169.64.244 |443 |
| Direktsänd dataström mått |rt.Services.visualstudio.com<br/>rt.applicationinsights.microsoft.com |23.96.28.38<br/>13.92.40.198 |443 |
| Internt telemetri |breeze.aimon.applicationinsights.IO |52.161.11.71 |443 |

## <a name="status-monitor"></a>Statusövervakaren
Status Monitor-konfiguration – behövs bara när du gör ändringar.

| Syfte | URL: EN | IP-adress | Portar |
| --- | --- | --- | --- |
| Konfiguration |`management.core.windows.net` | |`443` |
| Konfiguration |`management.azure.com` | |`443` |
| Konfiguration |`login.windows.net` | |`443` |
| Konfiguration |`login.microsoftonline.com` | |`443` |
| Konfiguration |`secure.aadcdn.microsoftonline-p.com` | |`443` |
| Konfiguration |`auth.gfx.ms` | |`443` |
| Konfiguration |`login.live.com` | |`443` |
| Installation |`packages.nuget.org` | |`443` |

## <a name="hockeyapp"></a>HockeyApp
| Syfte | URL: EN | IP-adress | Portar |
| --- | --- | --- | --- |
| Kraschdata |Gate.hockeyapp.NET |104.45.136.42 |80, 443 |

## <a name="availability-tests"></a>Tillgänglighetstester
Det här är hello lista med adresser som [tillgänglighet webbtester](app-insights-monitor-web-app-availability.md) körs. Om du vill toorun webbtester på din app men webbservern är begränsad tooserving specifika klienter, måste toopermit inkommande trafik från våra servrar för test av tillgänglighet.

Öppna portarna 80 (http) och 443 (https) för inkommande trafik från dessa adresser (IP-adresser är grupperade efter plats):

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

## <a name="data-access-api"></a>API för dataåtkomst
| Syfte | URI: N | IP-adress | Portar |
| --- | --- | --- | --- |
| API |API.applicationinsights.IO<br/>api1.applicationinsights.IO<br/>api2.applicationinsights.IO<br/>api3.applicationinsights.IO<br/>api4.applicationinsights.IO<br/>api5.applicationinsights.IO |13.82.26.252<br/>40.76.213.73 |80,443 |
| API-dokumentation |dev.applicationinsights.IO<br/>dev.applicationinsights.microsoft.com<br/>dev.aisvc.visualstudio.com<br/>www.applicationinsights.IO<br/>www.applicationinsights.microsoft.com<br/>www.aisvc.visualstudio.com |13.82.24.149<br/>40.114.82.10 |80,443 |
| Internt API |aigs.aisvc.visualstudio.com<br/>aigs1.aisvc.visualstudio.com<br/>aigs2.aisvc.visualstudio.com<br/>aigs3.aisvc.visualstudio.com<br/>aigs4.aisvc.visualstudio.com<br/>aigs5.aisvc.visualstudio.com<br/>aigs6.aisvc.visualstudio.com |dynamisk|443 |

## <a name="application-insights-analytics"></a>Application Insights Analytics

| Syfte | URI: N | IP-adress | Portar |
| --- | --- | --- | --- |
| Analytics-portalen | Analytics.applicationinsights.IO | dynamisk | 80,443 |
| CDN | applicationanalytics.azureedge.NET | dynamisk | 80,443 |
| Media CDN | applicationanalyticsmedia.azureedge.NET | dynamisk | 80,443 |

Obs! *. applicationinsights.io domän som ägs av Application Insights-teamet.

## <a name="application-insights-azure-portal-extension"></a>Application Insights Azure-Portaltillägg

| Syfte | URI: N | IP-adress | Portar |
| --- | --- | --- | --- |
| Application Insights Extension | stamp2.App.insightsportal.visualstudio.com | dynamisk | 80,443 |
| Application Insights Extension CDN | insightsportal-prod2-cdn.aisvc.visualstudio.com<br/>insightsportal-prod2-asiae-cdn.aisvc.visualstudio.com<br/>insightsportal-cdn-aimon.applicationinsights.io | dynamisk | 80,443 |

## <a name="application-insights-sdks"></a>Application Insights SDK

| Syfte | URI: N | IP-adress | Portar |
| --- | --- | --- | --- |
| Application Insights JS SDK CDN | az416426.VO.msecnd.NET | dynamisk | 80,443 |
| Application Insights Java SDK | aijavasdk.BLOB.Core.Windows.NET | dynamisk | 80,443 |

## <a name="profiler"></a>Profilerare

| Syfte | URI: N | IP-adress | Portar |
| --- | --- | --- | --- |
| Agent | Agent.azureserviceprofiler.NET<br/>*. agent.azureserviceprofiler.net | dynamisk | 443
| Portalen | gateway.azureserviceprofiler.NET | dynamisk | 443
| Lagring | *. core.windows.net | dynamisk | 443

## <a name="snapshot-debugger"></a>Felsökning av ögonblicksbild

| Syfte | URI: N | IP-adress | Portar |
| --- | --- | --- | --- |
| Agent | ppe.azureserviceprofiler.NET<br/>*. ppe.azureserviceprofiler.net | dynamisk | 443
| Portalen | ppe.gateway.azureserviceprofiler.NET | dynamisk | 443
| Lagring | *. core.windows.net | dynamisk | 443
