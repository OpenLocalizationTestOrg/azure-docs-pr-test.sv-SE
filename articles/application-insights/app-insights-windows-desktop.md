---
title: "aaaMonitoring användning och prestanda för Windows-program"
description: "Analysera användning och prestanda för Windows-program med HockeyApp och Application Insights."
services: application-insights
documentationcenter: windows
author: CFreemanwa
manager: carmonm
ms.assetid: 19040746-3315-47e7-8c60-4b3000d2ddc4
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/26/2016
ms.author: bwren
ms.openlocfilehash: 73806885a6f0ed3896c0e43308c90ba087007887
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="monitoring-usage-and-performance-in-windows-desktop-apps"></a>Övervaka användning och prestanda för Windows-appar


Med [Azure Application Insights](app-insights-overview.md) och [HockeyApp](https://hockeyapp.net) kan du övervaka dina distribuerade program för användning och prestanda.

> [!IMPORTANT]
> Vi rekommenderar [HockeyApp](https://hockeyapp.net) toodistribute och övervaka skrivbords- och appar. Med HockeyApp kan du hantera distribution, livetestning och användarfeedback samt övervaka användning och krasher. Du kan också [exportera och fråga din telemetri med Analytics](app-insights-hockeyapp-bridge-app.md).
> 
> Även om telemetri kan skickas tooApplication insikter från ett skrivbordsprogram är huvudsakligen användbart för felsökning och experiment.
> 
> 

## <a name="toosend-telemetry-tooapplication-insights-from-a-windows-application"></a>toosend telemetri tooApplication insikter från ett Windows-program
1. I hello [Azure-portalen](https://portal.azure.com), [skapa Application Insights-resurs](app-insights-create-new-resource.md). Välj ASP.NET-app för programtyp.
2. Ta en kopia av hello Instrumentation nyckel. Hitta hello nyckel i hello Essentials nedrullningsbara av hello ny resurs som du nyss skapade. 
3. Redigera hello NuGet-paket i ditt app-projekt i Visual Studio och Lägg till Microsoft.ApplicationInsights.WindowsServer. (Eller välj Microsoft.ApplicationInsights om du bara vill hello utan API, utan hello standard telemetri samling moduler.)
4. Ange hello instrumentation nyckel i koden:
   
    `TelemetryConfiguration.Active.InstrumentationKey = "` *din nyckel* `";` 
   
    eller i ApplicationInsights.config (om du har installerat en hello standard telemetri paket):
   
    `<InstrumentationKey>`*din nyckel*`</InstrumentationKey>` 
   
    Om du använder ApplicationInsights.config kontrollera dess egenskaper i Solution Explorer är inställda för**Skapa åtgärd = innehåll, kopiera tooOutput Directory = kopiera**.
5. [Använd hello API](app-insights-api-custom-events-metrics.md) toosend telemetri.
6. Kör appen och se hello telemetri i hello resursen som du skapade i hello Azure-portalen.

## <a name="telemetry"></a>Exempelkod
```C#

    public partial class Form1 : Form
    {
        private TelemetryClient tc = new TelemetryClient();
        ...
        private void Form1_Load(object sender, EventArgs e)
        {
            // Alternative toosetting ikey in config file:
            tc.InstrumentationKey = "key copied from portal";

            // Set session data:
            tc.Context.User.Id = Environment.UserName;
            tc.Context.Session.Id = Guid.NewGuid().ToString();
            tc.Context.Device.OperatingSystem = Environment.OSVersion.ToString();

            // Log a page view:
            tc.TrackPageView("Form1");
            ...
        }

        protected override void OnClosing(CancelEventArgs e)
        {
            stop = true;
            if (tc != null)
            {
                tc.Flush(); // only for desktop apps

                // Allow time for flushing:
                System.Threading.Thread.Sleep(1000);
            }
            base.OnClosing(e);
        }

```

## <a name="next-steps"></a>Nästa steg
* [Skapa en instrumentpanel](app-insights-dashboards.md)
* [Diagnostiksökning](app-insights-diagnostic-search.md)
* [Utforska mått](app-insights-metrics-explorer.md)
* [Skriv analysfrågor](app-insights-analytics.md)

