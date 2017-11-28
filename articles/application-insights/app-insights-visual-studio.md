---
title: aaaDebug program med Azure Application Insights i Visual Studio | Microsoft Docs
description: "Prestandaanalys och diagnostik för webbappar vid felsökning och i produktion."
services: application-insights
documentationcenter: .net
author: CFreemanwa
manager: carmonm
ms.assetid: 2059802b-1131-477e-a7b4-5f70fb53f974
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 07/7/2017
ms.author: bwren
ms.openlocfilehash: 20491fbe4505bf719039e5d1c220b1afec01db25
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="debug-your-applications-with-azure-application-insights-in-visual-studio"></a><span data-ttu-id="6fb43-103">Felsöka program med Azure Application Insights i Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6fb43-103">Debug your applications with Azure Application Insights in Visual Studio</span></span>
<span data-ttu-id="6fb43-104">I Visual Studio (2015 och senare) kan du analysera prestanda och diagnostisera problem i din ASP.NET-webbapp både när du felsöker och i produktion med hjälp av telemetri från [Azure Application Insights](app-insights-overview.md).</span><span class="sxs-lookup"><span data-stu-id="6fb43-104">In Visual Studio (2015 and later), you can analyze performance and diagnose issues in your ASP.NET web app both in debugging and in production, using telemetry from [Azure Application Insights](app-insights-overview.md).</span></span>

<span data-ttu-id="6fb43-105">Om du har skapat ditt ASP.NET-webbprogram med hjälp av Visual Studio 2017 eller senare, den redan har hello Application Insights SDK.</span><span class="sxs-lookup"><span data-stu-id="6fb43-105">If you created your ASP.NET web app using Visual Studio 2017 or later, it already has hello Application Insights SDK.</span></span> <span data-ttu-id="6fb43-106">Annars, om du inte redan gjort det, [Lägg till Application Insights tooyour app](app-insights-asp-net.md).</span><span class="sxs-lookup"><span data-stu-id="6fb43-106">Otherwise, if you haven't done so already, [add Application Insights tooyour app](app-insights-asp-net.md).</span></span>

<span data-ttu-id="6fb43-107">toomonitor din app när den är i live produktion du normalt visar hello Application Insights telemetri i hello [Azure-portalen](https://portal.azure.com), där du kan ställa in aviseringar och använda kraftfulla övervakningsverktyg.</span><span class="sxs-lookup"><span data-stu-id="6fb43-107">toomonitor your app when it's in live production, you normally view hello Application Insights telemetry in hello [Azure portal](https://portal.azure.com), where you can set alerts and apply powerful monitoring tools.</span></span> <span data-ttu-id="6fb43-108">Men för felsökning kan du också söka efter och analysera hello telemetri i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="6fb43-108">But for debugging, you can also search and analyze hello telemetry in Visual Studio.</span></span> <span data-ttu-id="6fb43-109">Du kan använda Visual Studio tooanalyze telemetri både från webbplatsen produktion och felsökning körs på utvecklingsdatorn.</span><span class="sxs-lookup"><span data-stu-id="6fb43-109">You can use Visual Studio tooanalyze telemetry both from your production site and from debugging runs on your development machine.</span></span> <span data-ttu-id="6fb43-110">Du kan analysera felsökning körs även om du ännu inte har konfigurerat hello SDK toosend telemetri toohello Azure-portalen i hello senare fallet.</span><span class="sxs-lookup"><span data-stu-id="6fb43-110">In hello latter case, you can analyze debugging runs even if you haven't yet configured hello SDK toosend telemetry toohello Azure portal.</span></span> 

## <span data-ttu-id="6fb43-111"><a name="run"></a> Felsöka ditt projekt</span><span class="sxs-lookup"><span data-stu-id="6fb43-111"><a name="run"></a> Debug your project</span></span>
<span data-ttu-id="6fb43-112">Kör din webbapp i lokalt felsökningsläge genom att välja F5.</span><span class="sxs-lookup"><span data-stu-id="6fb43-112">Run your web app in local debug mode by using F5.</span></span> <span data-ttu-id="6fb43-113">Öppna olika sidor toogenerate vissa telemetri.</span><span class="sxs-lookup"><span data-stu-id="6fb43-113">Open different pages toogenerate some telemetry.</span></span>

<span data-ttu-id="6fb43-114">I Visual Studio kan du se antalet hello-händelser som har loggats av hello Application Insights-modulen i projektet.</span><span class="sxs-lookup"><span data-stu-id="6fb43-114">In Visual Studio, you see a count of hello events that have been logged by hello Application Insights module in your project.</span></span>

![I Visual Studio visar hello Application Insights knappen vid felsökning.](./media/app-insights-visual-studio/appinsights-09eventcount.png)

<span data-ttu-id="6fb43-116">Klicka på den här knappen toosearch din telemetri.</span><span class="sxs-lookup"><span data-stu-id="6fb43-116">Click this button toosearch your telemetry.</span></span> 

## <a name="application-insights-search"></a><span data-ttu-id="6fb43-117">Söka med Application Insights</span><span class="sxs-lookup"><span data-stu-id="6fb43-117">Application Insights search</span></span>
<span data-ttu-id="6fb43-118">hello Application Insights sökfönstret visar händelser som har loggats.</span><span class="sxs-lookup"><span data-stu-id="6fb43-118">hello Application Insights Search window shows events that have been logged.</span></span> <span data-ttu-id="6fb43-119">(Om du har loggat in tooAzure när du ställer in Application Insights, kan du söka hello samma händelser i hello Azure-portalen.)</span><span class="sxs-lookup"><span data-stu-id="6fb43-119">(If you signed in tooAzure when you set up Application Insights, you can search hello same events in hello Azure portal.)</span></span>

![Högerklicka på hello-projektet och välj Application Insights sökning](./media/app-insights-visual-studio/34.png)

> [!NOTE] 
> <span data-ttu-id="6fb43-121">När du markera eller avmarkera filter klickar du på hello sökknappen hello slutet av hello textfält för sökning.</span><span class="sxs-lookup"><span data-stu-id="6fb43-121">After you select or deselect filters, click hello Search button at hello end of hello text search field.</span></span>
>

<span data-ttu-id="6fb43-122">hello ledigt textsökning fungerar på alla fält i hello händelser.</span><span class="sxs-lookup"><span data-stu-id="6fb43-122">hello free text search works on any fields in hello events.</span></span> <span data-ttu-id="6fb43-123">Till exempel söka efter en del av hello-URL för en sida. eller hello-värdet för en egenskap som klienten ort; eller specifika ord i en spårningslogg.</span><span class="sxs-lookup"><span data-stu-id="6fb43-123">For example, search for part of hello URL of a page; or hello value of a property such as client city; or specific words in a trace log.</span></span>

<span data-ttu-id="6fb43-124">Klicka på någon händelse toosee detaljerade egenskaper.</span><span class="sxs-lookup"><span data-stu-id="6fb43-124">Click any event toosee its detailed properties.</span></span>

<span data-ttu-id="6fb43-125">Du kan klicka på via toohello kod för begäranden tooyour webbapp.</span><span class="sxs-lookup"><span data-stu-id="6fb43-125">For requests tooyour web app, you can click through toohello code.</span></span>

![Klicka på via toohello kod under begär information](./media/app-insights-visual-studio/31.png)

<span data-ttu-id="6fb43-127">Du kan också öppna relaterade objekt toohelp diagnostisera misslyckade begäranden och undantag.</span><span class="sxs-lookup"><span data-stu-id="6fb43-127">You can also open related items toohelp diagnose failed requests or exceptions.</span></span>

![Rulla nedåt toorelated objekt under begär information](./media/app-insights-visual-studio/41.png)

## <a name="view-exceptions-and-failed-requests"></a><span data-ttu-id="6fb43-129">Visa undantag och misslyckade begäranden</span><span class="sxs-lookup"><span data-stu-id="6fb43-129">View exceptions and failed requests</span></span>
<span data-ttu-id="6fb43-130">Undantag rapporter visas i fönstret för hello Sök.</span><span class="sxs-lookup"><span data-stu-id="6fb43-130">Exception reports show in hello Search window.</span></span> <span data-ttu-id="6fb43-131">(I vissa äldre typer av ASP.NET-program kan ha för[konfigurera undantagsövervakning](app-insights-asp-net-exceptions.md) toosee undantag som hanteras av hello framework.)</span><span class="sxs-lookup"><span data-stu-id="6fb43-131">(In some older types of ASP.NET application, you have too[set up exception monitoring](app-insights-asp-net-exceptions.md) toosee exceptions that are handled by hello framework.)</span></span>

<span data-ttu-id="6fb43-132">Klicka på ett undantag tooget stack-spårning.</span><span class="sxs-lookup"><span data-stu-id="6fb43-132">Click an exception tooget a stack trace.</span></span> <span data-ttu-id="6fb43-133">Du kan klicka på via från hello stack trace toohello relevanta kodrad hello om hello koden för hello app är öppna i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="6fb43-133">If hello code of hello app is open in Visual Studio, you can click through from hello stack trace toohello relevant line of hello code.</span></span>

![Undantag för stackspårning](./media/app-insights-visual-studio/17.png)

## <a name="view-request-and-exception-summaries-in-hello-code"></a><span data-ttu-id="6fb43-135">Visa sammanfattningar begäran och undantag i hello kod</span><span class="sxs-lookup"><span data-stu-id="6fb43-135">View request and exception summaries in hello code</span></span>
<span data-ttu-id="6fb43-136">I hello kod lins rad ovanför varje hanterarmetoden visas antalet hello begäranden och undantag som loggats av Application Insights i hello föregående 24 timmar.</span><span class="sxs-lookup"><span data-stu-id="6fb43-136">In hello Code Lens line above each handler method, you see a count of hello requests and exceptions logged by Application Insights in hello past 24 h.</span></span>

![Undantag för stackspårning](./media/app-insights-visual-studio/21.png)

> [!NOTE] 
> <span data-ttu-id="6fb43-138">Koden lins visar Application Insights-data om du har [konfigurerat Application Insights-portalen app toosend telemetri toohello](app-insights-asp-net.md).</span><span class="sxs-lookup"><span data-stu-id="6fb43-138">Code Lens shows Application Insights data only if you have [configured your app toosend telemetry toohello Application Insights portal](app-insights-asp-net.md).</span></span>
>

[<span data-ttu-id="6fb43-139">Läs mer om Application Insights i CodeLens</span><span class="sxs-lookup"><span data-stu-id="6fb43-139">More about Application Insights in Code Lens</span></span>](app-insights-visual-studio-codelens.md)

## <a name="trends"></a><span data-ttu-id="6fb43-140">Trends</span><span class="sxs-lookup"><span data-stu-id="6fb43-140">Trends</span></span>
<span data-ttu-id="6fb43-141">Trends är ett verktyg för visualisering av hur din app ska bete sig över tiden.</span><span class="sxs-lookup"><span data-stu-id="6fb43-141">Trends is a tool for visualizing how your app behaves over time.</span></span> 

<span data-ttu-id="6fb43-142">Välj **utforska telemetri trender** från hello Application Insights verktygsfältsknappen eller Application Insights sökfönstret.</span><span class="sxs-lookup"><span data-stu-id="6fb43-142">Choose **Explore Telemetry Trends** from hello Application Insights toolbar button or Application Insights Search window.</span></span> <span data-ttu-id="6fb43-143">Välj en av fem vanliga frågor tooget igång.</span><span class="sxs-lookup"><span data-stu-id="6fb43-143">Choose one of five common queries tooget started.</span></span> <span data-ttu-id="6fb43-144">Du kan analysera olika datauppsättningar baserat på telemetrityper, tidsintervall och andra egenskaper.</span><span class="sxs-lookup"><span data-stu-id="6fb43-144">You can analyze different datasets based on telemetry types, time ranges, and other properties.</span></span> 

<span data-ttu-id="6fb43-145">toofind avvikelser i dina data, välja en av hello avvikelseidentifiering alternativ under hello ”vytypen” listrutan.</span><span class="sxs-lookup"><span data-stu-id="6fb43-145">toofind anomalies in your data, choose one of hello anomaly options under hello "View Type" dropdown.</span></span> <span data-ttu-id="6fb43-146">hello filtreringsalternativ längst hello hello fönstret gör det enkelt toohone i på specifika delmängder av dina telemetri.</span><span class="sxs-lookup"><span data-stu-id="6fb43-146">hello filtering options at hello bottom of hello window make it easy toohone in on specific subsets of your telemetry.</span></span>

![Trends](./media/app-insights-visual-studio/51.png)

<span data-ttu-id="6fb43-148">[Mer om Trends](app-insights-visual-studio-trends.md).</span><span class="sxs-lookup"><span data-stu-id="6fb43-148">[More about Trends](app-insights-visual-studio-trends.md).</span></span>

## <a name="local-monitoring"></a><span data-ttu-id="6fb43-149">Lokal övervakning</span><span class="sxs-lookup"><span data-stu-id="6fb43-149">Local monitoring</span></span>
<span data-ttu-id="6fb43-150">(Från Visual Studio 2015 Update 2) Om du inte har konfigurerat hello SDK toosend telemetri toohello Application Insights-portalen (så att det finns ingen instrumentation nyckel i ApplicationInsights.config) visar hello diagnostics-fönstret telemetri från din senaste felsökningssessionen.</span><span class="sxs-lookup"><span data-stu-id="6fb43-150">(From Visual Studio 2015 Update 2) If you haven't configured hello SDK toosend telemetry toohello Application Insights portal (so that there is no instrumentation key in ApplicationInsights.config) then hello diagnostics window displays telemetry from your latest debugging session.</span></span> 

<span data-ttu-id="6fb43-151">Detta är lämpligt om du redan har publicerat en tidigare version av din app.</span><span class="sxs-lookup"><span data-stu-id="6fb43-151">This is desirable if you have already published a previous version of your app.</span></span> <span data-ttu-id="6fb43-152">Vill du inte hello telemetri från dina felsökning sessioner toobe blandas med hello telemetri för hello Application Insights-portalen från hello publicerade app.</span><span class="sxs-lookup"><span data-stu-id="6fb43-152">You don't want hello telemetry from your debugging sessions toobe mixed up with hello telemetry on hello Application Insights portal from hello published app.</span></span>

<span data-ttu-id="6fb43-153">Det är också användbart om du har några [telemetri om anpassade](app-insights-api-custom-events-metrics.md) som du vill toodebug innan du skickar telemetri toohello portal.</span><span class="sxs-lookup"><span data-stu-id="6fb43-153">It's also useful if you have some [custom telemetry](app-insights-api-custom-events-metrics.md) that you want toodebug before sending telemetry toohello portal.</span></span>

* <span data-ttu-id="6fb43-154">*Först ska konfigurerade jag fullständigt Application Insights toosend telemetri toohello-portalen. Men nu vill jag toosee hello telemetri endast i Visual Studio.*</span><span class="sxs-lookup"><span data-stu-id="6fb43-154">*At first, I fully configured Application Insights toosend telemetry toohello portal. But now I'd like toosee hello telemetry only in Visual Studio.*</span></span>
  
  * <span data-ttu-id="6fb43-155">I fönstret hello-Sök inställningar finns en alternativet toosearch lokala diagnostik även om din app skicka telemetri toohello portal.</span><span class="sxs-lookup"><span data-stu-id="6fb43-155">In hello Search window's Settings, there's an option toosearch local diagnostics even if your app sends telemetry toohello portal.</span></span>
  * <span data-ttu-id="6fb43-156">toostop telemetri skickas toohello portal, kommentera hello rad `<instrumentationkey>...` från ApplicationInsights.config. När du är klar toosend telemetri toohello portal kan Avkommentera den.</span><span class="sxs-lookup"><span data-stu-id="6fb43-156">toostop telemetry being sent toohello portal, comment out hello line `<instrumentationkey>...` from ApplicationInsights.config. When you're ready toosend telemetry toohello portal again, uncomment it.</span></span>


## <a name="next-steps"></a><span data-ttu-id="6fb43-157">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="6fb43-157">Next steps</span></span>
|  |  |
| --- | --- |
| <span data-ttu-id="6fb43-158">**[Lägga till mer information](app-insights-asp-net-more.md)**</span><span class="sxs-lookup"><span data-stu-id="6fb43-158">**[Add more data](app-insights-asp-net-more.md)**</span></span><br/><span data-ttu-id="6fb43-159">Övervaka användning, tillgänglighet, beroenden och undantag.</span><span class="sxs-lookup"><span data-stu-id="6fb43-159">Monitor usage, availability, dependencies, exceptions.</span></span> <span data-ttu-id="6fb43-160">Integrera spårningar från loggningsramverk.</span><span class="sxs-lookup"><span data-stu-id="6fb43-160">Integrate traces from logging frameworks.</span></span> <span data-ttu-id="6fb43-161">Skriv anpassad telemetri.</span><span class="sxs-lookup"><span data-stu-id="6fb43-161">Write custom telemetry.</span></span> |![Visual Studio](./media/app-insights-visual-studio/64.png) |
| <span data-ttu-id="6fb43-163">**[Arbeta med hello Application Insights-portalen](app-insights-dashboards.md)**</span><span class="sxs-lookup"><span data-stu-id="6fb43-163">**[Working with hello Application Insights portal](app-insights-dashboards.md)**</span></span><br/><span data-ttu-id="6fb43-164">Visa instrumentpaneler, kraftfulla verktyg för diagnostik- och analytiska, aviseringar, en levande beroendekarta för anslutningar av programmet och exporterade telemetridata.</span><span class="sxs-lookup"><span data-stu-id="6fb43-164">View dashboards, powerful diagnostic and analytic tools, alerts, a live dependency map of your application, and exported telemetry data.</span></span> |![Visual Studio](./media/app-insights-visual-studio/62.png) |

