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
# <a name="use-powershell-tooset-alerts-in-application-insights"></a>Använd PowerShell tooset aviseringar i Application Insights
Du kan automatisera hello konfigurationen av [aviseringar](app-insights-alerts.md) i [Programinsikter](app-insights-overview.md).

Dessutom kan du [ange webhooks tooautomate aviseringen svar tooan](../monitoring-and-diagnostics/insights-webhooks-alerts.md).

> [!NOTE]
> Om du vill toocreate resurser och varningar på hello samma tid bör du överväga att [med en Azure Resource Manager-mall](app-insights-powershell.md).
>
>

## <a name="one-time-setup"></a>Enstaka installationen
Om du inte har använt PowerShell med din Azure-prenumeration innan du:

Installera hello Azure Powershell-modulen på hello datorn där du vill att toorun hello skript.

* Installera [Microsoft Web Platform Installer (v5 eller högre)](http://www.microsoft.com/web/downloads/platform.aspx).
* Använd den tooinstall Microsoft Azure Powershell

## <a name="connect-tooazure"></a>Ansluta tooAzure
Starta Azure PowerShell och [ansluter tooyour prenumeration](/powershell/azure/overview):

```PowerShell

    Add-AzureAccount
```


## <a name="get-alerts"></a>Få aviseringar
    Get-AzureAlertRmRule -ResourceGroup "Fabrikam" [-Name "My rule"] [-DetailedOutput]

## <a name="add-alert"></a>Lägg till avisering
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



## <a name="example-1"></a>Exempel 1
E-posta mig om hello serverns svar tooHTTP begäranden, ett genomsnitt över 5 minuter är långsammare än 1 sekund. Application Insights-resurs kallas IceCreamWebApp och den är i resursgruppen Fabrikam. Jag hello ägare hello Azure-prenumeration.

hello GUID är hello prenumerations-ID (inte hello instrumentation nyckel av programmet hello).

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

## <a name="example-2"></a>Exempel 2
Jag har ett program som jag använder [TrackMetric()](app-insights-api-custom-events-metrics.md#trackmetric) tooreport ett mått med namnet ”salesPerHour”. Skicka ett e-postmeddelande toomy kollegor om ”salesPerHour” sjunker under 100, ett genomsnitt under 24 timmar.

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

hello samma regel kan användas för hello mått som rapporteras med hjälp av hello [mätning parametern](app-insights-api-custom-events-metrics.md#properties) för spårning av ett annat anrop till exempel TrackEvent eller trackPageView.

## <a name="metric-names"></a>Tjänstmåttets namn
| Måttnamnet | Skärmnamn på | Beskrivning |
| --- | --- | --- |
| `basicExceptionBrowser.count` |Webbläsarundantag |Antal undantagsfel utan felhantering utlöstes i hello webbläsare. |
| `basicExceptionServer.count` |Undantag för |Antal undantagsfel utan felhantering utlöstes av hello app |
| `clientPerformance.clientProcess.value` |Bearbetningstid för klienten |Tiden mellan tar emot hello sista byten av ett dokument tills hello DOM har lästs in. Asynkrona förfrågningar kan fortfarande vara under bearbetning. |
| `clientPerformance.networkConnection.value` |Nätverket Anslut sidinläsningstiden |Tid hello webbläsare tar tooconnect toohello nätverk. Kan vara 0 om cachelagrade. |
| `clientPerformance.receiveRequest.value` |Ta emot svarstid |Tiden mellan webbläsaren som skickar begäran toostarting tooreceive svar. |
| `clientPerformance.sendRequest.value` |Skicka tid för begäran |Tid som toosend webbläsarbegäran. |
| `clientPerformance.total.value` |Sidhämtningstid |Tiden från användarförfrågan till dess att DOM, formatmallar, skript och bilder har lästs in. |
| `performanceCounter.available_bytes.value` |Tillgängligt minne |Fysiskt minne som är direkt tillgängligt för en process eller för systemanvändning. |
| `performanceCounter.io_data_bytes_per_sec.value` |Process-i/o-hastighet |Totalt antal byte per andra Läs och skriftliga toofiles, nätverk och enheter. |
| `performanceCounter.number_of_exceps_thrown_per_sec.value` |hastighet för undantag |Undantag per sekund. |
| `performanceCounter.percentage_processor_time.value` |Processen CPU |hello procentandel av förfluten tid som alla processens trådar som används av hello processor tooexecution instruktioner för hello program processen. |
| `performanceCounter.percentage_processor_total.value` |Processortid |hello procentandel av tiden som hello processor tillbringar i icke-inaktiva trådar. |
| `performanceCounter.process_private_bytes.value` |Processen för privata byte |Minne som tilldelats exklusivt toohello övervakade programprocesser. |
| `performanceCounter.request_execution_time.value` |Körningstid för ASP.NET-begäran |Körningstid för senaste hello-begäran. |
| `performanceCounter.requests_in_application_queue.value` |ASP.NET-begäranden i kö för körning |Längden på hello programbegärandekön. |
| `performanceCounter.requests_per_sec.value` |ASP.NET-begärandehastighet |Hastigheten för alla begäranden toohello program per sekund från ASP.NET. |
| `remoteDependencyFailed.durationMetric.count` |Beroende fel |Antal misslyckade anrop gjorda av hello server tooexternal programresurser. |
| `request.duration` |Serversvarstid |Tiden mellan ta emot en HTTP-begäran och slutför hello svar skickas. |
| `request.rate` |Begärandehastighet |Hastigheten för alla begäranden toohello program per sekund. |
| `requestFailed.count` |Misslyckade begäranden |Antal HTTP-begäranden som resulterade i en svarskoden > = 400 |
| `view.count` |Sidvisningar |Antal klientens användarförfrågningar för en webbsida. Syntetisk trafik filtreras bort. |
| {din anpassade mått name} |{Måttnamnet} |Värdet för din mått som rapporteras av [TrackMetric](app-insights-api-custom-events-metrics.md#trackmetric) eller i hello [mätningar parametern för ett anrop för spårning av](app-insights-api-custom-events-metrics.md#properties). |

hello mått skickas av olika telemetri moduler:

| Mått grupp | Insamlaren modul |
| --- | --- |
| basicExceptionBrowser,<br/>clientPerformance,<br/>vy |[Webbläsaren JavaScript](app-insights-javascript.md) |
| performanceCounter |[Prestanda](app-insights-configuration-with-applicationinsights-config.md) |
| remoteDependencyFailed |[Beroende](app-insights-configuration-with-applicationinsights-config.md) |
| begäran<br/>requestFailed |[Serverbegäran](app-insights-configuration-with-applicationinsights-config.md) |

## <a name="webhooks"></a>Webhooks
Du kan [automatisera aviseringen svar tooan](../monitoring-and-diagnostics/insights-webhooks-alerts.md). Azure anropar en webbadress som du väljer när en avisering utlöses.

## <a name="see-also"></a>Se även
* [Skriptet tooconfigure Application Insights](app-insights-powershell-script-create-resource.md)
* [Skapa Application Insights och testa webbresurser från mallar](app-insights-powershell.md)
* [Automatisera koppling Microsoft Azure-diagnostik tooApplication insikter](app-insights-powershell-azure-diagnostics.md)
* [Automatisera svar tooan aviseringen](../monitoring-and-diagnostics/insights-webhooks-alerts.md)
