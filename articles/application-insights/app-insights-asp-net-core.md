---
title: "aaaAzure Programinsikter för ASP.NET Core | Microsoft Docs"
description: "Övervaka webbprogram för tillgänglighet, prestanda och användning."
services: application-insights
documentationcenter: .net
author: CFreemanwa
manager: carmonm
ms.assetid: 3b722e47-38bd-4667-9ba4-65b7006c074c
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 03/14/2017
ms.author: bwren
ms.openlocfilehash: a7a27f9eef1daec5b0deae9fd88906e646980659
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="application-insights-for-aspnet-core"></a><span data-ttu-id="b081b-103">Application Insights för ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="b081b-103">Application Insights for ASP.NET Core</span></span>
<span data-ttu-id="b081b-104">[Application Insights](app-insights-overview.md) kan du övervaka ditt webbprogram för tillgänglighet, prestanda och användning.</span><span class="sxs-lookup"><span data-stu-id="b081b-104">[Application Insights](app-insights-overview.md) lets you monitor your web application for availability, performance and usage.</span></span> <span data-ttu-id="b081b-105">Du kan göra välgrundade beslut om hello riktning hello design med hello feedback du använda om hello prestanda och effektivitet för din app i hello, i varje utvecklingslivscykeln för.</span><span class="sxs-lookup"><span data-stu-id="b081b-105">With hello feedback you get about hello performance and effectiveness of your app in hello wild, you can make informed choices about hello direction of hello design in each development lifecycle.</span></span>

![Exempel](./media/app-insights-asp-net-core/sample.png)

<span data-ttu-id="b081b-107">Du behöver en prenumeration med [Microsoft Azure](http://azure.com).</span><span class="sxs-lookup"><span data-stu-id="b081b-107">You'll need a subscription with [Microsoft Azure](http://azure.com).</span></span> <span data-ttu-id="b081b-108">Logga in med ett Microsoft-konto som du kanske har för Windows, XBox Live eller andra Microsoft-molntjänster.</span><span class="sxs-lookup"><span data-stu-id="b081b-108">Sign in with a Microsoft account, which you might have for Windows, XBox Live, or other Microsoft cloud services.</span></span> <span data-ttu-id="b081b-109">Gruppen kan ha en organisationens prenumeration tooAzure: Be hello ägare tooadd tooit med ditt Microsoft-konto.</span><span class="sxs-lookup"><span data-stu-id="b081b-109">Your team might have an organizational subscription tooAzure: ask hello owner tooadd you tooit using your Microsoft account.</span></span>

## <a name="getting-started"></a><span data-ttu-id="b081b-110">Komma igång</span><span class="sxs-lookup"><span data-stu-id="b081b-110">Getting started</span></span>

* <span data-ttu-id="b081b-111">Högerklicka på ditt projekt i Visual Studio Solution Explorer och markera **konfigurera Application Insights**, eller **Lägg till > Application Insights**.</span><span class="sxs-lookup"><span data-stu-id="b081b-111">In Visual Studio Solution Explorer, right-click your project and select **Configure Application Insights**, or **Add > Application Insights**.</span></span> <span data-ttu-id="b081b-112">[Läs mer](app-insights-asp-net.md).</span><span class="sxs-lookup"><span data-stu-id="b081b-112">[Learn more](app-insights-asp-net.md).</span></span>
* <span data-ttu-id="b081b-113">Om du inte ser dessa kommandon på menyn följer hello [manuell komma igång-guide](https://github.com/Microsoft/ApplicationInsights-aspnetcore/wiki/Getting-Started).</span><span class="sxs-lookup"><span data-stu-id="b081b-113">If you don't see those menu commands, follow hello [manual getting Started guide](https://github.com/Microsoft/ApplicationInsights-aspnetcore/wiki/Getting-Started).</span></span> <span data-ttu-id="b081b-114">Du kan behöva toodo detta om projektet har skapats med en version av Visual Studio innan 2017.</span><span class="sxs-lookup"><span data-stu-id="b081b-114">You may need toodo this if your project was created with a version of Visual Studio before 2017.</span></span>

## <a name="using-application-insights"></a><span data-ttu-id="b081b-115">Använda Application Insights</span><span class="sxs-lookup"><span data-stu-id="b081b-115">Using Application Insights</span></span>
<span data-ttu-id="b081b-116">Logga in på hello [Microsoft Azure-portalen](https://portal.azure.com)väljer **alla resurser** eller **Programinsikter**, och välj sedan hello resurs du skapat toomonitor din app.</span><span class="sxs-lookup"><span data-stu-id="b081b-116">Sign into hello [Microsoft Azure portal](https://portal.azure.com), select **All Resources** or **Application Insights**, and then select hello resource you created toomonitor your app.</span></span>

<span data-ttu-id="b081b-117">Använda din app på ett tag i en separat webbläsarfönster.</span><span class="sxs-lookup"><span data-stu-id="b081b-117">In a separate browser window, use your app for a while.</span></span> <span data-ttu-id="b081b-118">Data som visas i hello Application Insights diagram visas.</span><span class="sxs-lookup"><span data-stu-id="b081b-118">You'll see data appearing in hello Application Insights charts.</span></span> <span data-ttu-id="b081b-119">(Du kanske tooclick uppdatering.) Det är bara en liten mängd data medan du utvecklar, men dessa diagram verkligen kan underlätta alive när du publicerar din app och har många användare.</span><span class="sxs-lookup"><span data-stu-id="b081b-119">(You might have tooclick Refresh.) There will be only a small amount of data while you're developing, but these charts really come alive when you publish your app and have many users.</span></span> 

<span data-ttu-id="b081b-120">hello översiktssidan visar viktiga prestandadiagram: serversvarstid, sidinläsningstiden och antalet misslyckade begäranden.</span><span class="sxs-lookup"><span data-stu-id="b081b-120">hello overview page shows key performance charts: server response time,  page load time, and counts of failed requests.</span></span> <span data-ttu-id="b081b-121">Klicka på alla diagram toosee flera diagram och data.</span><span class="sxs-lookup"><span data-stu-id="b081b-121">Click any chart toosee more charts and data.</span></span>

<span data-ttu-id="b081b-122">Vyer i hello portal delas in i tre huvudkategorier:</span><span class="sxs-lookup"><span data-stu-id="b081b-122">Views in hello portal fall into three main categories:</span></span>

* <span data-ttu-id="b081b-123">[Metrics Explorer](app-insights-metrics-explorer.md) visar diagram och tabeller för mått och antal, till exempel svarstider, fel eller mått som du skapar själv med hello [API](app-insights-api-custom-events-metrics.md).</span><span class="sxs-lookup"><span data-stu-id="b081b-123">[Metrics Explorer](app-insights-metrics-explorer.md) shows graphs and tables of metrics and counts, such as response times, failure rates, or metrics you create yourself with hello [API](app-insights-api-custom-events-metrics.md).</span></span> <span data-ttu-id="b081b-124">Filtrera och segment hello data genom att egenskapen värden tooget en bättre förståelse för din app och dess användare.</span><span class="sxs-lookup"><span data-stu-id="b081b-124">Filter and segment hello data by property values tooget a better understanding of your app and its users.</span></span>
* <span data-ttu-id="b081b-125">[Sök Explorer](app-insights-diagnostic-search.md) visar enskilda händelser, till exempel specifika begäranden, undantag, loggspårningar eller händelser som du själv har skapat med hello [API](app-insights-api-custom-events-metrics.md).</span><span class="sxs-lookup"><span data-stu-id="b081b-125">[Search Explorer](app-insights-diagnostic-search.md) lists individual events, such as specific requests, exceptions, log traces, or events you created yourself with hello [API](app-insights-api-custom-events-metrics.md).</span></span> <span data-ttu-id="b081b-126">Filtrera och söka i hello händelser och navigera mellan relaterade händelser tooinvestigate problem.</span><span class="sxs-lookup"><span data-stu-id="b081b-126">Filter and search in hello events, and navigate among related events tooinvestigate issues.</span></span>
* <span data-ttu-id="b081b-127">[Analytics](app-insights-analytics.md) kan du köra SQL-liknande frågor via din telemetri och är ett kraftfullt verktyg för analys och diagnostik.</span><span class="sxs-lookup"><span data-stu-id="b081b-127">[Analytics](app-insights-analytics.md) lets you run SQL-like queries over your telemetry, and is a powerful analytical and diagnostic tool.</span></span>

## <a name="alerts"></a><span data-ttu-id="b081b-128">Aviseringar</span><span class="sxs-lookup"><span data-stu-id="b081b-128">Alerts</span></span>
* <span data-ttu-id="b081b-129">Automatiskt få [proaktiv diagnostiska aviseringar](app-insights-proactive-diagnostics.md) som berättar om avvikande ändringar i fel och andra mått.</span><span class="sxs-lookup"><span data-stu-id="b081b-129">You automatically get [proactive diagnostic alerts](app-insights-proactive-diagnostics.md) that tell you about anomalous changes in failure rates and other metrics.</span></span>
* <span data-ttu-id="b081b-130">Ställ in [tillgänglighetstester](app-insights-monitor-web-app-availability.md) tootest webbplatsen kontinuerligt från platser över hela världen och få e-postmeddelanden när någon testa misslyckas.</span><span class="sxs-lookup"><span data-stu-id="b081b-130">Set up [availability tests](app-insights-monitor-web-app-availability.md) tootest your website continually from locations worldwide, and get emails as soon as any test fails.</span></span>
* <span data-ttu-id="b081b-131">Ställ in [mått aviseringar](app-insights-monitor-web-app-availability.md) tooknow om mått som svarstider eller undantag priser går utanför acceptabla gränser.</span><span class="sxs-lookup"><span data-stu-id="b081b-131">Set up [metric alerts](app-insights-monitor-web-app-availability.md) tooknow if metrics such as response times or exception rates go outside acceptable limits.</span></span>

## <a name="video"></a><span data-ttu-id="b081b-132">Video</span><span class="sxs-lookup"><span data-stu-id="b081b-132">Video</span></span>

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/100/player] 

## <a name="open-source"></a><span data-ttu-id="b081b-133">Öppen källkod</span><span class="sxs-lookup"><span data-stu-id="b081b-133">Open source</span></span>
[<span data-ttu-id="b081b-134">Läsa och bidra toohello kod</span><span class="sxs-lookup"><span data-stu-id="b081b-134">Read and contribute toohello code</span></span>](https://github.com/Microsoft/ApplicationInsights-aspnetcore#recent-updates)


## <a name="next-steps"></a><span data-ttu-id="b081b-135">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="b081b-135">Next steps</span></span>
* <span data-ttu-id="b081b-136">[Lägga till telemetri tooyour webbsidor](app-insights-javascript.md) toomonitor sidan användnings- och prestandadata.</span><span class="sxs-lookup"><span data-stu-id="b081b-136">[Add telemetry tooyour web pages](app-insights-javascript.md) toomonitor page usage and performance.</span></span>
* <span data-ttu-id="b081b-137">[Övervaka beroenden](app-insights-asp-net-dependencies.md) toosee om REST, SQL eller andra externa resurser saktar ner dig.</span><span class="sxs-lookup"><span data-stu-id="b081b-137">[Monitor dependencies](app-insights-asp-net-dependencies.md) toosee if REST, SQL or other external resources are slowing you down.</span></span>
* <span data-ttu-id="b081b-138">[Använd hello API](app-insights-api-custom-events-metrics.md) toosend egna händelser och mått för en mer detaljerad vy av appens prestanda och användning.</span><span class="sxs-lookup"><span data-stu-id="b081b-138">[Use hello API](app-insights-api-custom-events-metrics.md) toosend your own events and metrics for a more detailed view of your app's performance and usage.</span></span>
* <span data-ttu-id="b081b-139">[Tillgänglighetstester](app-insights-monitor-web-app-availability.md) Kontrollera din app ständigt hello världen.</span><span class="sxs-lookup"><span data-stu-id="b081b-139">[Availability tests](app-insights-monitor-web-app-availability.md) check your app constantly from around hello world.</span></span> 

