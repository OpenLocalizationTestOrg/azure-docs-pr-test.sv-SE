---
title: "Övervaka användning och prestanda för Windows-appar"
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
ms.openlocfilehash: 9d7e2a390adf10cbf5d88dd0084ce09136987309
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="monitoring-usage-and-performance-in-windows-desktop-apps"></a><span data-ttu-id="dacf6-103">Övervaka användning och prestanda för Windows-appar</span><span class="sxs-lookup"><span data-stu-id="dacf6-103">Monitoring usage and performance in Windows Desktop apps</span></span>


<span data-ttu-id="dacf6-104">Med [Azure Application Insights](app-insights-overview.md) och [HockeyApp](https://hockeyapp.net) kan du övervaka dina distribuerade program för användning och prestanda.</span><span class="sxs-lookup"><span data-stu-id="dacf6-104">[Azure Application Insights](app-insights-overview.md) and [HockeyApp](https://hockeyapp.net) let you monitor your deployed application for usage and performance.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="dacf6-105">Vi rekommenderar [HockeyApp](https://hockeyapp.net) för att distribuera och övervaka appar för dator eller mobila enheter.</span><span class="sxs-lookup"><span data-stu-id="dacf6-105">We recommend [HockeyApp](https://hockeyapp.net) to distribute and monitor desktop and device apps.</span></span> <span data-ttu-id="dacf6-106">Med HockeyApp kan du hantera distribution, livetestning och användarfeedback samt övervaka användning och krasher.</span><span class="sxs-lookup"><span data-stu-id="dacf6-106">With HockeyApp, you can manage distribution, live testing, and user feedback, as well as monitor usage and crash reports.</span></span> <span data-ttu-id="dacf6-107">Du kan också [exportera och fråga din telemetri med Analytics](app-insights-hockeyapp-bridge-app.md).</span><span class="sxs-lookup"><span data-stu-id="dacf6-107">You can also [export and query your telemetry with Analytics](app-insights-hockeyapp-bridge-app.md).</span></span>
> 
> <span data-ttu-id="dacf6-108">Även om telemetri kan skickas till Application Insights från ett program, är detta huvudsakligen användbatr för felsökning och utveckling.</span><span class="sxs-lookup"><span data-stu-id="dacf6-108">Although telemetry can be sent to Application Insights from a desktop application, this is chiefly useful for debugging and experimental purposes.</span></span>
> 
> 

## <a name="to-send-telemetry-to-application-insights-from-a-windows-application"></a><span data-ttu-id="dacf6-109">Skicka telemetri till Application Insights från ett Windows-program</span><span class="sxs-lookup"><span data-stu-id="dacf6-109">To send telemetry to Application Insights from a Windows application</span></span>
1. <span data-ttu-id="dacf6-110">[Skapa en Application Insights-resurs](app-insights-create-new-resource.md) på [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="dacf6-110">In the [Azure portal](https://portal.azure.com), [create an Application Insights resource](app-insights-create-new-resource.md).</span></span> <span data-ttu-id="dacf6-111">Välj ASP.NET-app för programtyp.</span><span class="sxs-lookup"><span data-stu-id="dacf6-111">For application type, choose ASP.NET app.</span></span>
2. <span data-ttu-id="dacf6-112">Kopiera instrumenteringsnyckeln.</span><span class="sxs-lookup"><span data-stu-id="dacf6-112">Take a copy of the Instrumentation Key.</span></span> <span data-ttu-id="dacf6-113">Hitta nyckeln i Essentials-listrutan för den nya resursen som du nyss skapade.</span><span class="sxs-lookup"><span data-stu-id="dacf6-113">Find the key in the Essentials drop-down of the new resource you just created.</span></span> 
3. <span data-ttu-id="dacf6-114">Redigera NuGet-paketet av för ett programprojekt och lägg till Microsoft.ApplicationInsights.WindowsServer i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="dacf6-114">In Visual Studio, edit the NuGet packages of your app project, and add Microsoft.ApplicationInsights.WindowsServer.</span></span> <span data-ttu-id="dacf6-115">(Eller välj Microsoft.ApplicationInsights om du bara vill ha API, utan standardmoduler för telemetrisk insamling)</span><span class="sxs-lookup"><span data-stu-id="dacf6-115">(Or choose Microsoft.ApplicationInsights if you just want the bare API, without the standard telemetry collection modules.)</span></span>
4. <span data-ttu-id="dacf6-116">Ange antingen instrumentationsnyckeln i koden:</span><span class="sxs-lookup"><span data-stu-id="dacf6-116">Set the instrumentation key either in your code:</span></span>
   
    <span data-ttu-id="dacf6-117">`TelemetryConfiguration.Active.InstrumentationKey = "` *din nyckel* `";`</span><span class="sxs-lookup"><span data-stu-id="dacf6-117">`TelemetryConfiguration.Active.InstrumentationKey = "` *your key* `";`</span></span> 
   
    <span data-ttu-id="dacf6-118">eller i ApplicationInsights.config (om du har installerat ett standard-telemetripaket):</span><span class="sxs-lookup"><span data-stu-id="dacf6-118">or in ApplicationInsights.config (if you installed one of the standard telemetry packages):</span></span>
   
    <span data-ttu-id="dacf6-119">`<InstrumentationKey>`*din nyckel*`</InstrumentationKey>`</span><span class="sxs-lookup"><span data-stu-id="dacf6-119">`<InstrumentationKey>`*your key*`</InstrumentationKey>`</span></span> 
   
    <span data-ttu-id="dacf6-120">Om du använder ApplicationInsights.config, kontrollera att egenskaperna i Solution Explorer är inställda på **Byggåtgärd = Innehåll, Kopiera till utdatakatalog = Kopiera**.</span><span class="sxs-lookup"><span data-stu-id="dacf6-120">If you use ApplicationInsights.config, make sure its properties in Solution Explorer are set to **Build Action = Content, Copy to Output Directory = Copy**.</span></span>
5. <span data-ttu-id="dacf6-121">[Använda API](app-insights-api-custom-events-metrics.md) att skicka telemetri.</span><span class="sxs-lookup"><span data-stu-id="dacf6-121">[Use the API](app-insights-api-custom-events-metrics.md) to send telemetry.</span></span>
6. <span data-ttu-id="dacf6-122">Kör din app och se telemetri i den resurs du skapat i Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="dacf6-122">Run your app, and see the telemetry in the resource you created in the Azure Portal.</span></span>

## <span data-ttu-id="dacf6-123"><a name="telemetry"></a>Exempelkod</span><span class="sxs-lookup"><span data-stu-id="dacf6-123"><a name="telemetry"></a>Example code</span></span>
```C#

    public partial class Form1 : Form
    {
        private TelemetryClient tc = new TelemetryClient();
        ...
        private void Form1_Load(object sender, EventArgs e)
        {
            // Alternative to setting ikey in config file:
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

## <a name="next-steps"></a><span data-ttu-id="dacf6-124">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="dacf6-124">Next steps</span></span>
* [<span data-ttu-id="dacf6-125">Skapa en instrumentpanel</span><span class="sxs-lookup"><span data-stu-id="dacf6-125">Create a dashboard</span></span>](app-insights-dashboards.md)
* [<span data-ttu-id="dacf6-126">Diagnostiksökning</span><span class="sxs-lookup"><span data-stu-id="dacf6-126">Diagnostic Search</span></span>](app-insights-diagnostic-search.md)
* [<span data-ttu-id="dacf6-127">Utforska mått</span><span class="sxs-lookup"><span data-stu-id="dacf6-127">Explore metrics</span></span>](app-insights-metrics-explorer.md)
* [<span data-ttu-id="dacf6-128">Skriv analysfrågor</span><span class="sxs-lookup"><span data-stu-id="dacf6-128">Write Analytics queries</span></span>](app-insights-analytics.md)

