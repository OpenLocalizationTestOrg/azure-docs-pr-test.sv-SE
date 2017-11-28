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
# <a name="monitoring-usage-and-performance-in-windows-desktop-apps"></a><span data-ttu-id="df49c-103">Övervaka användning och prestanda för Windows-appar</span><span class="sxs-lookup"><span data-stu-id="df49c-103">Monitoring usage and performance in Windows Desktop apps</span></span>


<span data-ttu-id="df49c-104">Med [Azure Application Insights](app-insights-overview.md) och [HockeyApp](https://hockeyapp.net) kan du övervaka dina distribuerade program för användning och prestanda.</span><span class="sxs-lookup"><span data-stu-id="df49c-104">[Azure Application Insights](app-insights-overview.md) and [HockeyApp](https://hockeyapp.net) let you monitor your deployed application for usage and performance.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="df49c-105">Vi rekommenderar [HockeyApp](https://hockeyapp.net) toodistribute och övervaka skrivbords- och appar.</span><span class="sxs-lookup"><span data-stu-id="df49c-105">We recommend [HockeyApp](https://hockeyapp.net) toodistribute and monitor desktop and device apps.</span></span> <span data-ttu-id="df49c-106">Med HockeyApp kan du hantera distribution, livetestning och användarfeedback samt övervaka användning och krasher.</span><span class="sxs-lookup"><span data-stu-id="df49c-106">With HockeyApp, you can manage distribution, live testing, and user feedback, as well as monitor usage and crash reports.</span></span> <span data-ttu-id="df49c-107">Du kan också [exportera och fråga din telemetri med Analytics](app-insights-hockeyapp-bridge-app.md).</span><span class="sxs-lookup"><span data-stu-id="df49c-107">You can also [export and query your telemetry with Analytics](app-insights-hockeyapp-bridge-app.md).</span></span>
> 
> <span data-ttu-id="df49c-108">Även om telemetri kan skickas tooApplication insikter från ett skrivbordsprogram är huvudsakligen användbart för felsökning och experiment.</span><span class="sxs-lookup"><span data-stu-id="df49c-108">Although telemetry can be sent tooApplication Insights from a desktop application, this is chiefly useful for debugging and experimental purposes.</span></span>
> 
> 

## <a name="toosend-telemetry-tooapplication-insights-from-a-windows-application"></a><span data-ttu-id="df49c-109">toosend telemetri tooApplication insikter från ett Windows-program</span><span class="sxs-lookup"><span data-stu-id="df49c-109">toosend telemetry tooApplication Insights from a Windows application</span></span>
1. <span data-ttu-id="df49c-110">I hello [Azure-portalen](https://portal.azure.com), [skapa Application Insights-resurs](app-insights-create-new-resource.md).</span><span class="sxs-lookup"><span data-stu-id="df49c-110">In hello [Azure portal](https://portal.azure.com), [create an Application Insights resource](app-insights-create-new-resource.md).</span></span> <span data-ttu-id="df49c-111">Välj ASP.NET-app för programtyp.</span><span class="sxs-lookup"><span data-stu-id="df49c-111">For application type, choose ASP.NET app.</span></span>
2. <span data-ttu-id="df49c-112">Ta en kopia av hello Instrumentation nyckel.</span><span class="sxs-lookup"><span data-stu-id="df49c-112">Take a copy of hello Instrumentation Key.</span></span> <span data-ttu-id="df49c-113">Hitta hello nyckel i hello Essentials nedrullningsbara av hello ny resurs som du nyss skapade.</span><span class="sxs-lookup"><span data-stu-id="df49c-113">Find hello key in hello Essentials drop-down of hello new resource you just created.</span></span> 
3. <span data-ttu-id="df49c-114">Redigera hello NuGet-paket i ditt app-projekt i Visual Studio och Lägg till Microsoft.ApplicationInsights.WindowsServer.</span><span class="sxs-lookup"><span data-stu-id="df49c-114">In Visual Studio, edit hello NuGet packages of your app project, and add Microsoft.ApplicationInsights.WindowsServer.</span></span> <span data-ttu-id="df49c-115">(Eller välj Microsoft.ApplicationInsights om du bara vill hello utan API, utan hello standard telemetri samling moduler.)</span><span class="sxs-lookup"><span data-stu-id="df49c-115">(Or choose Microsoft.ApplicationInsights if you just want hello bare API, without hello standard telemetry collection modules.)</span></span>
4. <span data-ttu-id="df49c-116">Ange hello instrumentation nyckel i koden:</span><span class="sxs-lookup"><span data-stu-id="df49c-116">Set hello instrumentation key either in your code:</span></span>
   
    <span data-ttu-id="df49c-117">`TelemetryConfiguration.Active.InstrumentationKey = "` *din nyckel* `";`</span><span class="sxs-lookup"><span data-stu-id="df49c-117">`TelemetryConfiguration.Active.InstrumentationKey = "` *your key* `";`</span></span> 
   
    <span data-ttu-id="df49c-118">eller i ApplicationInsights.config (om du har installerat en hello standard telemetri paket):</span><span class="sxs-lookup"><span data-stu-id="df49c-118">or in ApplicationInsights.config (if you installed one of hello standard telemetry packages):</span></span>
   
    <span data-ttu-id="df49c-119">`<InstrumentationKey>`*din nyckel*`</InstrumentationKey>`</span><span class="sxs-lookup"><span data-stu-id="df49c-119">`<InstrumentationKey>`*your key*`</InstrumentationKey>`</span></span> 
   
    <span data-ttu-id="df49c-120">Om du använder ApplicationInsights.config kontrollera dess egenskaper i Solution Explorer är inställda för**Skapa åtgärd = innehåll, kopiera tooOutput Directory = kopiera**.</span><span class="sxs-lookup"><span data-stu-id="df49c-120">If you use ApplicationInsights.config, make sure its properties in Solution Explorer are set too**Build Action = Content, Copy tooOutput Directory = Copy**.</span></span>
5. <span data-ttu-id="df49c-121">[Använd hello API](app-insights-api-custom-events-metrics.md) toosend telemetri.</span><span class="sxs-lookup"><span data-stu-id="df49c-121">[Use hello API](app-insights-api-custom-events-metrics.md) toosend telemetry.</span></span>
6. <span data-ttu-id="df49c-122">Kör appen och se hello telemetri i hello resursen som du skapade i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="df49c-122">Run your app, and see hello telemetry in hello resource you created in hello Azure Portal.</span></span>

## <span data-ttu-id="df49c-123"><a name="telemetry"></a>Exempelkod</span><span class="sxs-lookup"><span data-stu-id="df49c-123"><a name="telemetry"></a>Example code</span></span>
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

## <a name="next-steps"></a><span data-ttu-id="df49c-124">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="df49c-124">Next steps</span></span>
* [<span data-ttu-id="df49c-125">Skapa en instrumentpanel</span><span class="sxs-lookup"><span data-stu-id="df49c-125">Create a dashboard</span></span>](app-insights-dashboards.md)
* [<span data-ttu-id="df49c-126">Diagnostiksökning</span><span class="sxs-lookup"><span data-stu-id="df49c-126">Diagnostic Search</span></span>](app-insights-diagnostic-search.md)
* [<span data-ttu-id="df49c-127">Utforska mått</span><span class="sxs-lookup"><span data-stu-id="df49c-127">Explore metrics</span></span>](app-insights-metrics-explorer.md)
* [<span data-ttu-id="df49c-128">Skriv analysfrågor</span><span class="sxs-lookup"><span data-stu-id="df49c-128">Write Analytics queries</span></span>](app-insights-analytics.md)

