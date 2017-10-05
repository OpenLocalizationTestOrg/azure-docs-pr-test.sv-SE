---
title: "Azure Application Insights för ASP.NET Core | Microsoft Docs"
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
ms.openlocfilehash: d86495eea467977f6c079de72e2b49a2a1da2b60
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="application-insights-for-aspnet-core"></a><span data-ttu-id="e3a8d-103">Application Insights för ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e3a8d-103">Application Insights for ASP.NET Core</span></span>
<span data-ttu-id="e3a8d-104">[Application Insights](app-insights-overview.md) kan du övervaka ditt webbprogram för tillgänglighet, prestanda och användning.</span><span class="sxs-lookup"><span data-stu-id="e3a8d-104">[Application Insights](app-insights-overview.md) lets you monitor your web application for availability, performance and usage.</span></span> <span data-ttu-id="e3a8d-105">Med den feedback du får om appens prestanda och effektivitet kan du fatta välgrundade beslut om designen i varje utvecklingslivscykel.</span><span class="sxs-lookup"><span data-stu-id="e3a8d-105">With the feedback you get about the performance and effectiveness of your app in the wild, you can make informed choices about the direction of the design in each development lifecycle.</span></span>

![Exempel](./media/app-insights-asp-net-core/sample.png)

<span data-ttu-id="e3a8d-107">Du behöver en prenumeration med [Microsoft Azure](http://azure.com).</span><span class="sxs-lookup"><span data-stu-id="e3a8d-107">You'll need a subscription with [Microsoft Azure](http://azure.com).</span></span> <span data-ttu-id="e3a8d-108">Logga in med ett Microsoft-konto som du kanske har för Windows, XBox Live eller andra Microsoft-molntjänster.</span><span class="sxs-lookup"><span data-stu-id="e3a8d-108">Sign in with a Microsoft account, which you might have for Windows, XBox Live, or other Microsoft cloud services.</span></span> <span data-ttu-id="e3a8d-109">Gruppen kan ha en organisationens prenumeration på Azure: Be ägare att lägga till dig i den med ditt Microsoft-konto.</span><span class="sxs-lookup"><span data-stu-id="e3a8d-109">Your team might have an organizational subscription to Azure: ask the owner to add you to it using your Microsoft account.</span></span>

## <a name="getting-started"></a><span data-ttu-id="e3a8d-110">Komma igång</span><span class="sxs-lookup"><span data-stu-id="e3a8d-110">Getting started</span></span>

* <span data-ttu-id="e3a8d-111">Högerklicka på ditt projekt i Visual Studio Solution Explorer och markera **konfigurera Application Insights**, eller **Lägg till > Application Insights**.</span><span class="sxs-lookup"><span data-stu-id="e3a8d-111">In Visual Studio Solution Explorer, right-click your project and select **Configure Application Insights**, or **Add > Application Insights**.</span></span> <span data-ttu-id="e3a8d-112">[Läs mer](app-insights-asp-net.md).</span><span class="sxs-lookup"><span data-stu-id="e3a8d-112">[Learn more](app-insights-asp-net.md).</span></span>
* <span data-ttu-id="e3a8d-113">Om du inte ser dessa kommandon på menyn, följer du de [manuell komma igång-guide](https://github.com/Microsoft/ApplicationInsights-aspnetcore/wiki/Getting-Started).</span><span class="sxs-lookup"><span data-stu-id="e3a8d-113">If you don't see those menu commands, follow the [manual getting Started guide](https://github.com/Microsoft/ApplicationInsights-aspnetcore/wiki/Getting-Started).</span></span> <span data-ttu-id="e3a8d-114">Du kan behöva göra detta om projektet har skapats med en version av Visual Studio innan 2017.</span><span class="sxs-lookup"><span data-stu-id="e3a8d-114">You may need to do this if your project was created with a version of Visual Studio before 2017.</span></span>

## <a name="using-application-insights"></a><span data-ttu-id="e3a8d-115">Använda Application Insights</span><span class="sxs-lookup"><span data-stu-id="e3a8d-115">Using Application Insights</span></span>
<span data-ttu-id="e3a8d-116">Logga in på den [Microsoft Azure-portalen](https://portal.azure.com)väljer **alla resurser** eller **Programinsikter**, och välj sedan den resurs du skapat för att övervaka din app.</span><span class="sxs-lookup"><span data-stu-id="e3a8d-116">Sign into the [Microsoft Azure portal](https://portal.azure.com), select **All Resources** or **Application Insights**, and then select the resource you created to monitor your app.</span></span>

<span data-ttu-id="e3a8d-117">Använda din app på ett tag i en separat webbläsarfönster.</span><span class="sxs-lookup"><span data-stu-id="e3a8d-117">In a separate browser window, use your app for a while.</span></span> <span data-ttu-id="e3a8d-118">Data som visas i Application Insights-diagram visas.</span><span class="sxs-lookup"><span data-stu-id="e3a8d-118">You'll see data appearing in the Application Insights charts.</span></span> <span data-ttu-id="e3a8d-119">(Du kan behöva klicka på Uppdatera.) Det är bara en liten mängd data medan du utvecklar, men dessa diagram verkligen kan underlätta alive när du publicerar din app och har många användare.</span><span class="sxs-lookup"><span data-stu-id="e3a8d-119">(You might have to click Refresh.) There will be only a small amount of data while you're developing, but these charts really come alive when you publish your app and have many users.</span></span> 

<span data-ttu-id="e3a8d-120">På översiktssidan visar viktiga prestandadiagram: serversvarstid, sidinläsningstiden och antalet misslyckade begäranden.</span><span class="sxs-lookup"><span data-stu-id="e3a8d-120">The overview page shows key performance charts: server response time,  page load time, and counts of failed requests.</span></span> <span data-ttu-id="e3a8d-121">Klicka på diagram om du vill se mer diagram och data.</span><span class="sxs-lookup"><span data-stu-id="e3a8d-121">Click any chart to see more charts and data.</span></span>

<span data-ttu-id="e3a8d-122">Vyer i portalen delas in i tre huvudkategorier:</span><span class="sxs-lookup"><span data-stu-id="e3a8d-122">Views in the portal fall into three main categories:</span></span>

* <span data-ttu-id="e3a8d-123">[Metrics Explorer](app-insights-metrics-explorer.md) visar diagram och tabeller för mått och antal, till exempel svarstider, fel eller mått som du skapar själv med de [API](app-insights-api-custom-events-metrics.md).</span><span class="sxs-lookup"><span data-stu-id="e3a8d-123">[Metrics Explorer](app-insights-metrics-explorer.md) shows graphs and tables of metrics and counts, such as response times, failure rates, or metrics you create yourself with the [API](app-insights-api-custom-events-metrics.md).</span></span> <span data-ttu-id="e3a8d-124">Filtrera och segmentera data av egenskapsvärden för att få en bättre förståelse för din app och dess användare.</span><span class="sxs-lookup"><span data-stu-id="e3a8d-124">Filter and segment the data by property values to get a better understanding of your app and its users.</span></span>
* <span data-ttu-id="e3a8d-125">[Sök Explorer](app-insights-diagnostic-search.md) visar enskilda händelser, till exempel specifika begäranden, undantag, loggspårningar eller händelser som du själv har skapat med den [API](app-insights-api-custom-events-metrics.md).</span><span class="sxs-lookup"><span data-stu-id="e3a8d-125">[Search Explorer](app-insights-diagnostic-search.md) lists individual events, such as specific requests, exceptions, log traces, or events you created yourself with the [API](app-insights-api-custom-events-metrics.md).</span></span> <span data-ttu-id="e3a8d-126">Filtrera och söka i händelserna och navigera mellan relaterade händelser att undersöka problem.</span><span class="sxs-lookup"><span data-stu-id="e3a8d-126">Filter and search in the events, and navigate among related events to investigate issues.</span></span>
* <span data-ttu-id="e3a8d-127">[Analytics](app-insights-analytics.md) kan du köra SQL-liknande frågor via din telemetri och är ett kraftfullt verktyg för analys och diagnostik.</span><span class="sxs-lookup"><span data-stu-id="e3a8d-127">[Analytics](app-insights-analytics.md) lets you run SQL-like queries over your telemetry, and is a powerful analytical and diagnostic tool.</span></span>

## <a name="alerts"></a><span data-ttu-id="e3a8d-128">Aviseringar</span><span class="sxs-lookup"><span data-stu-id="e3a8d-128">Alerts</span></span>
* <span data-ttu-id="e3a8d-129">Automatiskt få [proaktiv diagnostiska aviseringar](app-insights-proactive-diagnostics.md) som berättar om avvikande ändringar i fel och andra mått.</span><span class="sxs-lookup"><span data-stu-id="e3a8d-129">You automatically get [proactive diagnostic alerts](app-insights-proactive-diagnostics.md) that tell you about anomalous changes in failure rates and other metrics.</span></span>
* <span data-ttu-id="e3a8d-130">Ställ in [tillgänglighetstester](app-insights-monitor-web-app-availability.md) att testa din webbplats kontinuerligt från platser över hela världen och få e-postmeddelanden när någon testa misslyckas.</span><span class="sxs-lookup"><span data-stu-id="e3a8d-130">Set up [availability tests](app-insights-monitor-web-app-availability.md) to test your website continually from locations worldwide, and get emails as soon as any test fails.</span></span>
* <span data-ttu-id="e3a8d-131">Ställ in [mått aviseringar](app-insights-monitor-web-app-availability.md) du behöver veta om mått som svarstider eller undantag priser går utanför acceptabla gränser.</span><span class="sxs-lookup"><span data-stu-id="e3a8d-131">Set up [metric alerts](app-insights-monitor-web-app-availability.md) to know if metrics such as response times or exception rates go outside acceptable limits.</span></span>

## <a name="video"></a><span data-ttu-id="e3a8d-132">Video</span><span class="sxs-lookup"><span data-stu-id="e3a8d-132">Video</span></span>

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/100/player] 

## <a name="open-source"></a><span data-ttu-id="e3a8d-133">Öppen källkod</span><span class="sxs-lookup"><span data-stu-id="e3a8d-133">Open source</span></span>
[<span data-ttu-id="e3a8d-134">Läsa och bidra till koden</span><span class="sxs-lookup"><span data-stu-id="e3a8d-134">Read and contribute to the code</span></span>](https://github.com/Microsoft/ApplicationInsights-aspnetcore#recent-updates)


## <a name="next-steps"></a><span data-ttu-id="e3a8d-135">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="e3a8d-135">Next steps</span></span>
* <span data-ttu-id="e3a8d-136">[Lägg till telemetri till dina webbsidor](app-insights-javascript.md) övervakaren sidan användning och prestanda.</span><span class="sxs-lookup"><span data-stu-id="e3a8d-136">[Add telemetry to your web pages](app-insights-javascript.md) to monitor page usage and performance.</span></span>
* <span data-ttu-id="e3a8d-137">[Övervaka beroenden](app-insights-asp-net-dependencies.md) att se om REST, SQL eller andra externa resurser saktar ner dig.</span><span class="sxs-lookup"><span data-stu-id="e3a8d-137">[Monitor dependencies](app-insights-asp-net-dependencies.md) to see if REST, SQL or other external resources are slowing you down.</span></span>
* <span data-ttu-id="e3a8d-138">[Använd API](app-insights-api-custom-events-metrics.md) att skicka dina egna händelser och mått för en mer detaljerad vy av appens prestanda och användning.</span><span class="sxs-lookup"><span data-stu-id="e3a8d-138">[Use the API](app-insights-api-custom-events-metrics.md) to send your own events and metrics for a more detailed view of your app's performance and usage.</span></span>
* <span data-ttu-id="e3a8d-139">[Tillgänglighetstester](app-insights-monitor-web-app-availability.md) Kontrollera din app hela tiden från hela världen.</span><span class="sxs-lookup"><span data-stu-id="e3a8d-139">[Availability tests](app-insights-monitor-web-app-availability.md) check your app constantly from around the world.</span></span> 

