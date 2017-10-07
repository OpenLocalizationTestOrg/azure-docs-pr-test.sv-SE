---
title: aaaDiagnose fel och undantag i webbappar med Azure Application Insights | Microsoft Docs
description: "Fånga undantag från ASP.NET appar tillsammans med begärandetelemetri."
services: application-insights
documentationcenter: .net
author: CFreemanwa
manager: carmonm
ms.assetid: d1e98390-3ce4-4d04-9351-144314a42aa2
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 03/14/2017
ms.author: bwren
ms.openlocfilehash: 8930e6d2b29f83ea635c4ecb7afd11fc1d97d085
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="diagnose-exceptions-in-your-web-apps-with-application-insights"></a><span data-ttu-id="b71a0-103">Diagnostisera undantag i ditt webbprogram med Application Insights</span><span class="sxs-lookup"><span data-stu-id="b71a0-103">Diagnose exceptions in your web apps with Application Insights</span></span>
<span data-ttu-id="b71a0-104">Undantag i livewebbappar rapporteras av [Programinsikter](app-insights-overview.md).</span><span class="sxs-lookup"><span data-stu-id="b71a0-104">Exceptions in your live web app are reported by [Application Insights](app-insights-overview.md).</span></span> <span data-ttu-id="b71a0-105">Du kan jämföra misslyckade begäranden med undantag och andra händelser på både hello klienten och servern, så att du snabbt kan diagnostisera hello orsaker.</span><span class="sxs-lookup"><span data-stu-id="b71a0-105">You can correlate failed requests with exceptions and other events at both hello client and server, so that you can quickly diagnose hello causes.</span></span>

## <a name="set-up-exception-reporting"></a><span data-ttu-id="b71a0-106">Konfigurera undantag reporting</span><span class="sxs-lookup"><span data-stu-id="b71a0-106">Set up exception reporting</span></span>
* <span data-ttu-id="b71a0-107">toohave undantag som rapporterats från din server:</span><span class="sxs-lookup"><span data-stu-id="b71a0-107">toohave exceptions reported from your server app:</span></span>
  * <span data-ttu-id="b71a0-108">Installera [Application Insights SDK](app-insights-asp-net.md) i din Appkod eller</span><span class="sxs-lookup"><span data-stu-id="b71a0-108">Install [Application Insights SDK](app-insights-asp-net.md) in your app code, or</span></span>
  * <span data-ttu-id="b71a0-109">IIS-webbservrar: kör [Application Insights Agent](app-insights-monitor-performance-live-website-now.md); eller</span><span class="sxs-lookup"><span data-stu-id="b71a0-109">IIS web servers: Run [Application Insights Agent](app-insights-monitor-performance-live-website-now.md); or</span></span>
  * <span data-ttu-id="b71a0-110">Azure-webbappar: Lägg till hello [Application Insights Extension](app-insights-azure-web-apps.md)</span><span class="sxs-lookup"><span data-stu-id="b71a0-110">Azure web apps: Add hello [Application Insights Extension](app-insights-azure-web-apps.md)</span></span>
  * <span data-ttu-id="b71a0-111">Java-webbappar: Installera hello [Java-agent](app-insights-java-agent.md)</span><span class="sxs-lookup"><span data-stu-id="b71a0-111">Java web apps: Install hello [Java agent](app-insights-java-agent.md)</span></span>
* <span data-ttu-id="b71a0-112">Installera hello [JavaScript-kodfragment](app-insights-javascript.md) i din webbsidor toocatch webbläsarundantag.</span><span class="sxs-lookup"><span data-stu-id="b71a0-112">Install hello [JavaScript snippet](app-insights-javascript.md) in your web pages toocatch browser exceptions.</span></span>
* <span data-ttu-id="b71a0-113">I vissa ramverk för programmet eller med vissa inställningar, behöver du tootake några extra steg toocatch flera undantag:</span><span class="sxs-lookup"><span data-stu-id="b71a0-113">In some application frameworks or with some settings, you need tootake some extra steps toocatch more exceptions:</span></span>
  * [<span data-ttu-id="b71a0-114">Webbformulär</span><span class="sxs-lookup"><span data-stu-id="b71a0-114">Web forms</span></span>](#web-forms)
  * [<span data-ttu-id="b71a0-115">MVC</span><span class="sxs-lookup"><span data-stu-id="b71a0-115">MVC</span></span>](#mvc)
  * [<span data-ttu-id="b71a0-116">Webb-API-1.*</span><span class="sxs-lookup"><span data-stu-id="b71a0-116">Web API 1.*</span></span>](#web-api-1)
  * [<span data-ttu-id="b71a0-117">Webb-API 2.*</span><span class="sxs-lookup"><span data-stu-id="b71a0-117">Web API 2.*</span></span>](#web-api-2)
  * [<span data-ttu-id="b71a0-118">WCF</span><span class="sxs-lookup"><span data-stu-id="b71a0-118">WCF</span></span>](#wcf)

## <a name="diagnosing-exceptions-using-visual-studio"></a><span data-ttu-id="b71a0-119">Diagnostisera undantag med Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b71a0-119">Diagnosing exceptions using Visual Studio</span></span>
<span data-ttu-id="b71a0-120">Öppna hello app lösningen i Visual Studio toohelp med felsökning.</span><span class="sxs-lookup"><span data-stu-id="b71a0-120">Open hello app solution in Visual Studio toohelp with debugging.</span></span>

<span data-ttu-id="b71a0-121">Kör hello appen på din server eller på utvecklingsdatorn med F5.</span><span class="sxs-lookup"><span data-stu-id="b71a0-121">Run hello app, either on your server or on your development machine by using F5.</span></span>

<span data-ttu-id="b71a0-122">Öppna fönstret för hello Application Insights Sök i Visual Studio och ange toodisplay händelser från din app.</span><span class="sxs-lookup"><span data-stu-id="b71a0-122">Open hello Application Insights Search window in Visual Studio, and set it toodisplay events from your app.</span></span> <span data-ttu-id="b71a0-123">Du kan göra detta genom att klicka hello Application Insights-knappen när du felsöker.</span><span class="sxs-lookup"><span data-stu-id="b71a0-123">While you're debugging, you can do this just by clicking hello Application Insights button.</span></span>

![Högerklicka på hello-projektet och välj Application Insights, öppna.](./media/app-insights-asp-net-exceptions/34.png)

<span data-ttu-id="b71a0-125">Observera att du kan filtrera hello rapporten tooshow bara undantag.</span><span class="sxs-lookup"><span data-stu-id="b71a0-125">Notice that you can filter hello report tooshow just exceptions.</span></span>

<span data-ttu-id="b71a0-126">*Inga undantag visar? Se [fånga undantag](#exceptions).*</span><span class="sxs-lookup"><span data-stu-id="b71a0-126">*No exceptions showing? See [Capture exceptions](#exceptions).*</span></span>

<span data-ttu-id="b71a0-127">Klicka på ett undantag rapporten tooshow dess stack-spårning.</span><span class="sxs-lookup"><span data-stu-id="b71a0-127">Click an exception report tooshow its stack trace.</span></span>
<span data-ttu-id="b71a0-128">Klicka på en radreferens i hello stack-spårning, tooopen hello relevanta kodfilen.</span><span class="sxs-lookup"><span data-stu-id="b71a0-128">Click a line reference in hello stack trace, tooopen hello relevant code file.</span></span>  

<span data-ttu-id="b71a0-129">Observera att CodeLens visar data om hello undantag i hello kod:</span><span class="sxs-lookup"><span data-stu-id="b71a0-129">In hello code, notice that CodeLens shows data about hello exceptions:</span></span>

![CodeLens meddelande om undantag.](./media/app-insights-asp-net-exceptions/35.png)

## <a name="diagnosing-failures-using-hello-azure-portal"></a><span data-ttu-id="b71a0-131">Felsökning av med hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="b71a0-131">Diagnosing failures using hello Azure portal</span></span>
<span data-ttu-id="b71a0-132">Från hello Application Insights översikten över appen, hello fel innehåller diagram av undantag och misslyckade HTTP-begäranden, tillsammans med en lista över hello begära URL: er som orsakar hello de vanligaste fel.</span><span class="sxs-lookup"><span data-stu-id="b71a0-132">From hello Application Insights overview of your app, hello Failures tile shows you charts of exceptions and failed HTTP requests, together with a list of hello request URLs that cause hello most frequent failures.</span></span>

![Välj inställningar för fel](./media/app-insights-asp-net-exceptions/012-start.png)

<span data-ttu-id="b71a0-134">Klicka på ett av hello misslyckades undantag typer i hello listan tooget tooindividual förekomster av hello undantag, där du kan se hello information och Stackspårning:</span><span class="sxs-lookup"><span data-stu-id="b71a0-134">Click through one of hello failed exception types in hello list tooget tooindividual occurrences of hello exception, where you can see hello details and stack trace:</span></span>

![Välj en instans av en misslyckad begäran och under undantagsinformation, hämta tooinstances hello undantag.](./media/app-insights-asp-net-exceptions/030-req-drill.png)

<span data-ttu-id="b71a0-136">**Du kan också** du kan starta från hello lista med begäranden och hitta undantag relaterade tooit.</span><span class="sxs-lookup"><span data-stu-id="b71a0-136">**Alternatively,** you can start from hello list of requests and find exceptions related tooit.</span></span>

<span data-ttu-id="b71a0-137">*Inga undantag visar? Se [fånga undantag](#exceptions).*</span><span class="sxs-lookup"><span data-stu-id="b71a0-137">*No exceptions showing? See [Capture exceptions](#exceptions).*</span></span>


## <a name="custom-tracing-and-log-data"></a><span data-ttu-id="b71a0-138">Anpassade spårning och loggdata</span><span class="sxs-lookup"><span data-stu-id="b71a0-138">Custom tracing and log data</span></span>
<span data-ttu-id="b71a0-139">tooget diagnostikdata specifika tooyour app som du kan infoga kod toosend telemetridata.</span><span class="sxs-lookup"><span data-stu-id="b71a0-139">tooget diagnostic data specific tooyour app, you can insert code toosend your own telemetry data.</span></span> <span data-ttu-id="b71a0-140">Detta visas i diagnostiska tillsammans med hello begäran, vyn sida och andra automatiskt insamlade data.</span><span class="sxs-lookup"><span data-stu-id="b71a0-140">This displayed in diagnostic search alongside hello request, page view and other automatically-collected data.</span></span>

<span data-ttu-id="b71a0-141">Har du flera alternativ:</span><span class="sxs-lookup"><span data-stu-id="b71a0-141">You have several options:</span></span>

* <span data-ttu-id="b71a0-142">[Trackevent ()](app-insights-api-custom-events-metrics.md#trackevent) används vanligtvis för att övervaka användningsmönster, men hello data skickas också visas under Anpassad händelser i diagnostiska sökningen.</span><span class="sxs-lookup"><span data-stu-id="b71a0-142">[TrackEvent()](app-insights-api-custom-events-metrics.md#trackevent) is typically used for monitoring usage patterns, but hello data it sends also appears under Custom Events in diagnostic search.</span></span> <span data-ttu-id="b71a0-143">Händelser är namngivna och kan utföra strängegenskaper och numeriska mått som du kan [filtrera sökningen diagnostiska](app-insights-diagnostic-search.md).</span><span class="sxs-lookup"><span data-stu-id="b71a0-143">Events are named, and can carry string properties and numeric metrics on which you can [filter your diagnostic searches](app-insights-diagnostic-search.md).</span></span>
* <span data-ttu-id="b71a0-144">[TrackTrace()](app-insights-api-custom-events-metrics.md#tracktrace) kan du skicka längre data, till exempel efter information.</span><span class="sxs-lookup"><span data-stu-id="b71a0-144">[TrackTrace()](app-insights-api-custom-events-metrics.md#tracktrace) lets you send longer data such as POST information.</span></span>
* <span data-ttu-id="b71a0-145">[TrackException()](#exceptions) skickar Stacka spårningar.</span><span class="sxs-lookup"><span data-stu-id="b71a0-145">[TrackException()](#exceptions) sends stack traces.</span></span> <span data-ttu-id="b71a0-146">[Mer information om undantag](#exceptions).</span><span class="sxs-lookup"><span data-stu-id="b71a0-146">[More about exceptions](#exceptions).</span></span>
* <span data-ttu-id="b71a0-147">Om du redan använder ett loggningsramverk som Log4Net eller NLog, kan du [samla in dessa loggar](app-insights-asp-net-trace-logs.md) och se dem i diagnostiska sökningen tillsammans med data för begäran och undantag.</span><span class="sxs-lookup"><span data-stu-id="b71a0-147">If you already use a logging framework like Log4Net or NLog, you can [capture those logs](app-insights-asp-net-trace-logs.md) and see them in diagnostic search alongside request and exception data.</span></span>

<span data-ttu-id="b71a0-148">toosee dessa händelser, öppna [Sök](app-insights-diagnostic-search.md), öppna Filter och välj sedan Custom Event, spårning eller undantag.</span><span class="sxs-lookup"><span data-stu-id="b71a0-148">toosee these events, open [Search](app-insights-diagnostic-search.md), open Filter, and then choose Custom Event, Trace, or Exception.</span></span>

![Detaljvisning](./media/app-insights-asp-net-exceptions/viewCustomEvents.png)

> [!NOTE]
> <span data-ttu-id="b71a0-150">Om din app genererar mycket telemetri, minska hello anpassningsbar provtagning modulen automatiskt hello volymen som skickas toohello portal genom att skicka en representativ del av händelser.</span><span class="sxs-lookup"><span data-stu-id="b71a0-150">If your app generates a lot of telemetry, hello adaptive sampling module will automatically reduce hello volume that is sent toohello portal by sending only a representative fraction of events.</span></span> <span data-ttu-id="b71a0-151">Händelser som är en del av hello samma åtgärd ska markeras eller avmarkeras som en grupp, så att du kan navigera mellan relaterade händelser.</span><span class="sxs-lookup"><span data-stu-id="b71a0-151">Events that are part of hello same operation will be selected or deselected as a group, so that you can navigate between related events.</span></span> [<span data-ttu-id="b71a0-152">Läs mer om sampling.</span><span class="sxs-lookup"><span data-stu-id="b71a0-152">Learn about sampling.</span></span>](app-insights-sampling.md)
>
>

### <a name="how-toosee-request-post-data"></a><span data-ttu-id="b71a0-153">Hur toosee begära postdata</span><span class="sxs-lookup"><span data-stu-id="b71a0-153">How toosee request POST data</span></span>
<span data-ttu-id="b71a0-154">Information om begäran med inte hello data som skickas tooyour app i en POST-anrop.</span><span class="sxs-lookup"><span data-stu-id="b71a0-154">Request details don't include hello data sent tooyour app in a POST call.</span></span> <span data-ttu-id="b71a0-155">toohave rapporteras för dessa data:</span><span class="sxs-lookup"><span data-stu-id="b71a0-155">toohave this data reported:</span></span>

* <span data-ttu-id="b71a0-156">[Installera hello SDK](app-insights-asp-net.md) i projektet program.</span><span class="sxs-lookup"><span data-stu-id="b71a0-156">[Install hello SDK](app-insights-asp-net.md) in your application project.</span></span>
* <span data-ttu-id="b71a0-157">Infoga kod i dina program toocall [Microsoft.ApplicationInsights.TrackTrace()](app-insights-api-custom-events-metrics.md#tracktrace).</span><span class="sxs-lookup"><span data-stu-id="b71a0-157">Insert code in your application toocall [Microsoft.ApplicationInsights.TrackTrace()](app-insights-api-custom-events-metrics.md#tracktrace).</span></span> <span data-ttu-id="b71a0-158">Skicka hello postdata i hello message-parameter.</span><span class="sxs-lookup"><span data-stu-id="b71a0-158">Send hello POST data in hello message parameter.</span></span> <span data-ttu-id="b71a0-159">Det finns en gräns toohello tillåtna storlek, bör du toosend bara hello viktiga data.</span><span class="sxs-lookup"><span data-stu-id="b71a0-159">There is a limit toohello permitted size, so you should try toosend just hello essential data.</span></span>
* <span data-ttu-id="b71a0-160">När du undersöker en misslyckad begäran hitta hello associerade spårningar.</span><span class="sxs-lookup"><span data-stu-id="b71a0-160">When you investigate a failed request, find hello associated traces.</span></span>  

![Detaljvisning](./media/app-insights-asp-net-exceptions/060-req-related.png)

## <span data-ttu-id="b71a0-162"><a name="exceptions"></a>Fånga undantag och relaterade diagnostikdata</span><span class="sxs-lookup"><span data-stu-id="b71a0-162"><a name="exceptions"></a> Capturing exceptions and related diagnostic data</span></span>
<span data-ttu-id="b71a0-163">Först visas inte i hello portal alla hello-undantag som orsakar fel i din app.</span><span class="sxs-lookup"><span data-stu-id="b71a0-163">At first, you won't see in hello portal all hello exceptions that cause failures in your app.</span></span> <span data-ttu-id="b71a0-164">Du ser alla webbläsarundantag (om du använder hello [JavaScript SDK](app-insights-javascript.md) på webbsidorna).</span><span class="sxs-lookup"><span data-stu-id="b71a0-164">You'll see any browser exceptions (if you're using hello [JavaScript SDK](app-insights-javascript.md) in your web pages).</span></span> <span data-ttu-id="b71a0-165">Men de flesta undantag fångas av IIS och du har toowrite lite kod toosee dem.</span><span class="sxs-lookup"><span data-stu-id="b71a0-165">But most server exceptions are caught by IIS and you have toowrite a bit of code toosee them.</span></span>

<span data-ttu-id="b71a0-166">Du kan:</span><span class="sxs-lookup"><span data-stu-id="b71a0-166">You can:</span></span>

* <span data-ttu-id="b71a0-167">**Logga undantag uttryckligen** genom att infoga kod i undantagshanterare tooreport hello undantag.</span><span class="sxs-lookup"><span data-stu-id="b71a0-167">**Log exceptions explicitly** by inserting code in exception handlers tooreport hello exceptions.</span></span>
* <span data-ttu-id="b71a0-168">**Fånga undantag automatiskt** genom att konfigurera ASP.NET framework.</span><span class="sxs-lookup"><span data-stu-id="b71a0-168">**Capture exceptions automatically** by configuring your ASP.NET framework.</span></span> <span data-ttu-id="b71a0-169">hello nödvändiga tillägg är olika för olika typer av framework.</span><span class="sxs-lookup"><span data-stu-id="b71a0-169">hello necessary additions are different for different types of framework.</span></span>

## <a name="reporting-exceptions-explicitly"></a><span data-ttu-id="b71a0-170">Rapportering undantag uttryckligen</span><span class="sxs-lookup"><span data-stu-id="b71a0-170">Reporting exceptions explicitly</span></span>
<span data-ttu-id="b71a0-171">hello är enklaste sättet tooinsert en anropet tooTrackException() i en undantagshanterare.</span><span class="sxs-lookup"><span data-stu-id="b71a0-171">hello simplest way is tooinsert a call tooTrackException() in an exception handler.</span></span>

<span data-ttu-id="b71a0-172">JavaScript</span><span class="sxs-lookup"><span data-stu-id="b71a0-172">JavaScript</span></span>

    try
    { ...
    }
    catch (ex)
    {
      appInsights.trackException(ex, "handler loc",
        {Game: currentGame.Name,
         State: currentGame.State.ToString()});
    }

<span data-ttu-id="b71a0-173">C#</span><span class="sxs-lookup"><span data-stu-id="b71a0-173">C#</span></span>

    var telemetry = new TelemetryClient();
    ...
    try
    { ...
    }
    catch (Exception ex)
    {
       // Set up some properties:
       var properties = new Dictionary <string, string>
         {{"Game", currentGame.Name}};

       var measurements = new Dictionary <string, double>
         {{"Users", currentGame.Users.Count}};

       // Send hello exception telemetry:
       telemetry.TrackException(ex, properties, measurements);
    }

<span data-ttu-id="b71a0-174">VB</span><span class="sxs-lookup"><span data-stu-id="b71a0-174">VB</span></span>

    Dim telemetry = New TelemetryClient
    ...
    Try
      ...
    Catch ex as Exception
      ' Set up some properties:
      Dim properties = New Dictionary (Of String, String)
      properties.Add("Game", currentGame.Name)

      Dim measurements = New Dictionary (Of String, Double)
      measurements.Add("Users", currentGame.Users.Count)

      ' Send hello exception telemetry:
      telemetry.TrackException(ex, properties, measurements)
    End Try

<span data-ttu-id="b71a0-175">hello egenskaper och mätningar parametrar är valfria, men är användbara för [filtrering och lägga till](app-insights-diagnostic-search.md) extra information.</span><span class="sxs-lookup"><span data-stu-id="b71a0-175">hello properties and measurements parameters are optional, but are useful for [filtering and adding](app-insights-diagnostic-search.md) extra information.</span></span> <span data-ttu-id="b71a0-176">Till exempel om du har en app som kan köra flera spel hittade alla hello undantag rapporter relaterade tooa visst spel.</span><span class="sxs-lookup"><span data-stu-id="b71a0-176">For example, if you have an app that can run several games, you could find all hello exception reports related tooa particular game.</span></span> <span data-ttu-id="b71a0-177">Du kan lägga till så många objekt som du precis som tooeach ordlistan.</span><span class="sxs-lookup"><span data-stu-id="b71a0-177">You can add as many items as you like tooeach dictionary.</span></span>

## <a name="browser-exceptions"></a><span data-ttu-id="b71a0-178">Webbläsarundantag</span><span class="sxs-lookup"><span data-stu-id="b71a0-178">Browser exceptions</span></span>
<span data-ttu-id="b71a0-179">De flesta webbläsarundantag rapporteras.</span><span class="sxs-lookup"><span data-stu-id="b71a0-179">Most browser exceptions are reported.</span></span>

<span data-ttu-id="b71a0-180">Om din webbsida innehåller skriptfiler från nätverk för innehållsleverans eller andra domäner, kontrollera din skripttypen har hello attributet ```crossorigin="anonymous"```, och den hello-servern skickar [CORS huvuden](http://enable-cors.org/).</span><span class="sxs-lookup"><span data-stu-id="b71a0-180">If your web page includes script files from content delivery networks or other domains, ensure your script tag has hello attribute ```crossorigin="anonymous"```,  and that hello server sends [CORS headers](http://enable-cors.org/).</span></span> <span data-ttu-id="b71a0-181">Detta gör att du tooget stack-spårning och information för ohanterade JavaScript-undantag från dessa resurser.</span><span class="sxs-lookup"><span data-stu-id="b71a0-181">This will allow you tooget a stack trace and detail for unhandled JavaScript exceptions from these resources.</span></span>

## <a name="web-forms"></a><span data-ttu-id="b71a0-182">Webbformulär</span><span class="sxs-lookup"><span data-stu-id="b71a0-182">Web forms</span></span>
<span data-ttu-id="b71a0-183">Webbformulär ska hello HTTP-modul kunna toocollect hello undantag när det finns inga omdirigeringar som konfigurerats med CustomErrors.</span><span class="sxs-lookup"><span data-stu-id="b71a0-183">For web forms, hello HTTP Module will be able toocollect hello exceptions when there are no redirects configured with CustomErrors.</span></span>

<span data-ttu-id="b71a0-184">Men om du har active omdirigeringar, lägger du till följande rader toohello Application_Error funktion i Global.asax.cs hello.</span><span class="sxs-lookup"><span data-stu-id="b71a0-184">But if you have active redirects, add hello following lines toohello Application_Error function in Global.asax.cs.</span></span> <span data-ttu-id="b71a0-185">(Lägg till en Global.asax-fil om du inte redan har ett.)</span><span class="sxs-lookup"><span data-stu-id="b71a0-185">(Add a Global.asax file if you don't already have one.)</span></span>

<span data-ttu-id="b71a0-186">*C#*</span><span class="sxs-lookup"><span data-stu-id="b71a0-186">*C#*</span></span>

    void Application_Error(object sender, EventArgs e)
    {
      if (HttpContext.Current.IsCustomErrorEnabled && Server.GetLastError  () != null)
      {
         var ai = new TelemetryClient(); // or re-use an existing instance

         ai.TrackException(Server.GetLastError());
      }
    }


## <a name="mvc"></a><span data-ttu-id="b71a0-187">MVC</span><span class="sxs-lookup"><span data-stu-id="b71a0-187">MVC</span></span>
<span data-ttu-id="b71a0-188">Om hello [CustomErrors](https://msdn.microsoft.com/library/h0hfz6fc.aspx) konfigurationen är `Off`, undantag ska vara tillgängliga för hello [HTTP-modul](https://msdn.microsoft.com/library/ms178468.aspx) toocollect.</span><span class="sxs-lookup"><span data-stu-id="b71a0-188">If hello [CustomErrors](https://msdn.microsoft.com/library/h0hfz6fc.aspx) configuration is `Off`, then exceptions will be available for hello [HTTP Module](https://msdn.microsoft.com/library/ms178468.aspx) toocollect.</span></span> <span data-ttu-id="b71a0-189">Men om det är `RemoteOnly` (standard), eller `On`, hello undantag kommer att tas bort och inte tillgängligt för Application Insights tooautomatically samla in.</span><span class="sxs-lookup"><span data-stu-id="b71a0-189">However, if it is `RemoteOnly` (default), or `On`, then hello exception will be cleared and not available for Application Insights tooautomatically collect.</span></span> <span data-ttu-id="b71a0-190">Du kan åtgärda detta genom att åsidosätta hello [System.Web.Mvc.HandleErrorAttribute klassen](http://msdn.microsoft.com/library/system.web.mvc.handleerrorattribute.aspx), och tillämpa hello åsidosätts klass som visas för hello MVC-versionerna nedan ([github-källan](https://github.com/AppInsightsSamples/Mvc2UnhandledExceptions/blob/master/MVC2App/Controllers/AiHandleErrorAttribute.cs)):</span><span class="sxs-lookup"><span data-stu-id="b71a0-190">You can fix that by overriding hello [System.Web.Mvc.HandleErrorAttribute class](http://msdn.microsoft.com/library/system.web.mvc.handleerrorattribute.aspx), and applying hello overridden class as shown for hello different MVC versions below ([github source](https://github.com/AppInsightsSamples/Mvc2UnhandledExceptions/blob/master/MVC2App/Controllers/AiHandleErrorAttribute.cs)):</span></span>

    using System;
    using System.Web.Mvc;
    using Microsoft.ApplicationInsights;

    namespace MVC2App.Controllers
    {
      [AttributeUsage(AttributeTargets.Class | AttributeTargets.Method, Inherited = true, AllowMultiple = true)]
      public class AiHandleErrorAttribute : HandleErrorAttribute
      {
        public override void OnException(ExceptionContext filterContext)
        {
            if (filterContext != null && filterContext.HttpContext != null && filterContext.Exception != null)
            {
                //If customError is Off, then AI HTTPModule will report hello exception
                if (filterContext.HttpContext.IsCustomErrorEnabled)
                {   //or reuse instance (recommended!). see note above  
                    var ai = new TelemetryClient();
                    ai.TrackException(filterContext.Exception);
                }
            }
            base.OnException(filterContext);
        }
      }
    }

#### <a name="mvc-2"></a><span data-ttu-id="b71a0-191">MVC 2</span><span class="sxs-lookup"><span data-stu-id="b71a0-191">MVC 2</span></span>
<span data-ttu-id="b71a0-192">Ersätt hello HandleError attribut med din nya attribut i dina domänkontrollanter.</span><span class="sxs-lookup"><span data-stu-id="b71a0-192">Replace hello HandleError attribute with your new attribute in your controllers.</span></span>

    namespace MVC2App.Controllers
    {
       [AiHandleError]
       public class HomeController : Controller
       {
    ...

[<span data-ttu-id="b71a0-193">Exempel</span><span class="sxs-lookup"><span data-stu-id="b71a0-193">Sample</span></span>](https://github.com/AppInsightsSamples/Mvc2UnhandledExceptions)

#### <a name="mvc-3"></a><span data-ttu-id="b71a0-194">MVC 3</span><span class="sxs-lookup"><span data-stu-id="b71a0-194">MVC 3</span></span>
<span data-ttu-id="b71a0-195">Registrera `AiHandleErrorAttribute` som ett globalt filter i Global.asax.cs:</span><span class="sxs-lookup"><span data-stu-id="b71a0-195">Register `AiHandleErrorAttribute` as a global filter in Global.asax.cs:</span></span>

    public class MyMvcApplication : System.Web.HttpApplication
    {
      public static void RegisterGlobalFilters(GlobalFilterCollection filters)
      {
         filters.Add(new AiHandleErrorAttribute());
      }
     ...

[<span data-ttu-id="b71a0-196">Exempel</span><span class="sxs-lookup"><span data-stu-id="b71a0-196">Sample</span></span>](https://github.com/AppInsightsSamples/Mvc3UnhandledExceptionTelemetry)

#### <a name="mvc-4-mvc5"></a><span data-ttu-id="b71a0-197">MVC 4, MVC5</span><span class="sxs-lookup"><span data-stu-id="b71a0-197">MVC 4, MVC5</span></span>
<span data-ttu-id="b71a0-198">Registrera AiHandleErrorAttribute som ett globalt filter i FilterConfig.cs:</span><span class="sxs-lookup"><span data-stu-id="b71a0-198">Register AiHandleErrorAttribute as a global filter in FilterConfig.cs:</span></span>

    public class FilterConfig
    {
      public static void RegisterGlobalFilters(GlobalFilterCollection filters)
      {
        // Default replaced with hello override tootrack unhandled exceptions
        filters.Add(new AiHandleErrorAttribute());
      }
    }

[<span data-ttu-id="b71a0-199">Exempel</span><span class="sxs-lookup"><span data-stu-id="b71a0-199">Sample</span></span>](https://github.com/AppInsightsSamples/Mvc5UnhandledExceptionTelemetry)

## <a name="web-api-1x"></a><span data-ttu-id="b71a0-200">Webb-API 1.x</span><span class="sxs-lookup"><span data-stu-id="b71a0-200">Web API 1.x</span></span>
<span data-ttu-id="b71a0-201">Åsidosätt System.Web.Http.Filters.ExceptionFilterAttribute:</span><span class="sxs-lookup"><span data-stu-id="b71a0-201">Override System.Web.Http.Filters.ExceptionFilterAttribute:</span></span>

    using System.Web.Http.Filters;
    using Microsoft.ApplicationInsights;

    namespace WebAPI.App_Start
    {
      public class AiExceptionFilterAttribute : ExceptionFilterAttribute
      {
        public override void OnException(HttpActionExecutedContext actionExecutedContext)
        {
            if (actionExecutedContext != null && actionExecutedContext.Exception != null)
            {  //or reuse instance (recommended!). see note above
                var ai = new TelemetryClient();
                ai.TrackException(actionExecutedContext.Exception);    
            }
            base.OnException(actionExecutedContext);
        }
      }
    }

<span data-ttu-id="b71a0-202">Du kan lägga till den här åsidosatt attributet toospecific domänkontrollanter eller lägga till den toohello globala filterkonfiguration i hello WebApiConfig klass:</span><span class="sxs-lookup"><span data-stu-id="b71a0-202">You could add this overridden attribute toospecific controllers, or add it toohello global filter configuration in hello WebApiConfig class:</span></span>

    using System.Web.Http;
    using WebApi1.x.App_Start;

    namespace WebApi1.x
    {
      public static class WebApiConfig
      {
        public static void Register(HttpConfiguration config)
        {
            config.Routes.MapHttpRoute(name: "DefaultApi", routeTemplate: "api/{controller}/{id}",
                defaults: new { id = RouteParameter.Optional });
            ...
            config.EnableSystemDiagnosticsTracing();

            // Capture exceptions for Application Insights:
            config.Filters.Add(new AiExceptionFilterAttribute());
        }
      }
    }

[<span data-ttu-id="b71a0-203">Exempel</span><span class="sxs-lookup"><span data-stu-id="b71a0-203">Sample</span></span>](https://github.com/AppInsightsSamples/WebApi_1.x_UnhandledExceptions)

<span data-ttu-id="b71a0-204">Det finns ett antal fall som hello-undantagsfilter som inte kan hantera.</span><span class="sxs-lookup"><span data-stu-id="b71a0-204">There are a number of cases that hello exception filters cannot handle.</span></span> <span data-ttu-id="b71a0-205">Exempel:</span><span class="sxs-lookup"><span data-stu-id="b71a0-205">For example:</span></span>

* <span data-ttu-id="b71a0-206">Undantag från domänkontrollanten konstruktorer.</span><span class="sxs-lookup"><span data-stu-id="b71a0-206">Exceptions thrown from controller constructors.</span></span>
* <span data-ttu-id="b71a0-207">Undantag från meddelandehanterare.</span><span class="sxs-lookup"><span data-stu-id="b71a0-207">Exceptions thrown from message handlers.</span></span>
* <span data-ttu-id="b71a0-208">Undantag under routning.</span><span class="sxs-lookup"><span data-stu-id="b71a0-208">Exceptions thrown during routing.</span></span>
* <span data-ttu-id="b71a0-209">Undantag under innehåll serialiseringssvar.</span><span class="sxs-lookup"><span data-stu-id="b71a0-209">Exceptions thrown during response content serialization.</span></span>

## <a name="web-api-2x"></a><span data-ttu-id="b71a0-210">Webb-API 2.x</span><span class="sxs-lookup"><span data-stu-id="b71a0-210">Web API 2.x</span></span>
<span data-ttu-id="b71a0-211">Lägg till en implementering av IExceptionLogger:</span><span class="sxs-lookup"><span data-stu-id="b71a0-211">Add an implementation of IExceptionLogger:</span></span>

    using System.Web.Http.ExceptionHandling;
    using Microsoft.ApplicationInsights;

    namespace ProductsAppPureWebAPI.App_Start
    {
      public class AiExceptionLogger : ExceptionLogger
      {
        public override void Log(ExceptionLoggerContext context)
        {
            if (context !=null && context.Exception != null)
            {//or reuse instance (recommended!). see note above
                var ai = new TelemetryClient();
                ai.TrackException(context.Exception);
            }
            base.Log(context);
        }
      }
    }

<span data-ttu-id="b71a0-212">Lägga till den här toohello tjänsterna i WebApiConfig:</span><span class="sxs-lookup"><span data-stu-id="b71a0-212">Add this toohello services in WebApiConfig:</span></span>

    using System.Web.Http;
    using System.Web.Http.ExceptionHandling;
    using ProductsAppPureWebAPI.App_Start;

    namespace WebApi2WithMVC
    {
      public static class WebApiConfig
      {
        public static void Register(HttpConfiguration config)
        {
            // Web API configuration and services

            // Web API routes
            config.MapHttpAttributeRoutes();

            config.Routes.MapHttpRoute(
                name: "DefaultApi",
                routeTemplate: "api/{controller}/{id}",
                defaults: new { id = RouteParameter.Optional }
            );
            config.Services.Add(typeof(IExceptionLogger), new AiExceptionLogger());
        }
      }
  <span data-ttu-id="b71a0-213">}</span><span class="sxs-lookup"><span data-stu-id="b71a0-213">}</span></span>

[<span data-ttu-id="b71a0-214">Exempel</span><span class="sxs-lookup"><span data-stu-id="b71a0-214">Sample</span></span>](https://github.com/AppInsightsSamples/WebApi_2.x_UnhandledExceptions)

<span data-ttu-id="b71a0-215">Alternativt kan du:</span><span class="sxs-lookup"><span data-stu-id="b71a0-215">As alternatives, you could:</span></span>

1. <span data-ttu-id="b71a0-216">Ersätt hello endast ExceptionHandler med en anpassad implementering av IExceptionHandler.</span><span class="sxs-lookup"><span data-stu-id="b71a0-216">Replace hello only ExceptionHandler with a custom implementation of IExceptionHandler.</span></span> <span data-ttu-id="b71a0-217">Detta är endast anropas när hello framework fortfarande toochoose vilka svar meddelande toosend (inte när hello-anslutningen avbröts exempelvis)</span><span class="sxs-lookup"><span data-stu-id="b71a0-217">This is only called when hello framework is still able toochoose which response message toosend (not when hello connection is aborted for instance)</span></span>
2. <span data-ttu-id="b71a0-218">Undantagsfilter (enligt beskrivningen i avsnittet hello på webb-API 1.x domänkontrollanter ovan) - inte anropas i samtliga fall.</span><span class="sxs-lookup"><span data-stu-id="b71a0-218">Exception Filters (as described in hello section on Web API 1.x controllers above) - not called in all cases.</span></span>

## <a name="wcf"></a><span data-ttu-id="b71a0-219">WCF</span><span class="sxs-lookup"><span data-stu-id="b71a0-219">WCF</span></span>
<span data-ttu-id="b71a0-220">Lägg till en klass som utökar Attribute och implementerar IErrorHandler och IServiceBehavior.</span><span class="sxs-lookup"><span data-stu-id="b71a0-220">Add a class that extends Attribute and implements IErrorHandler and IServiceBehavior.</span></span>

    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.ServiceModel.Description;
    using System.ServiceModel.Dispatcher;
    using System.Web;
    using Microsoft.ApplicationInsights;

    namespace WcfService4.ErrorHandling
    {
      public class AiLogExceptionAttribute : Attribute, IErrorHandler, IServiceBehavior
      {
        public void AddBindingParameters(ServiceDescription serviceDescription,
            System.ServiceModel.ServiceHostBase serviceHostBase,
            System.Collections.ObjectModel.Collection<ServiceEndpoint> endpoints,
            System.ServiceModel.Channels.BindingParameterCollection bindingParameters)
        {
        }

        public void ApplyDispatchBehavior(ServiceDescription serviceDescription,
            System.ServiceModel.ServiceHostBase serviceHostBase)
        {
            foreach (ChannelDispatcher disp in serviceHostBase.ChannelDispatchers)
            {
                disp.ErrorHandlers.Add(this);
            }
        }

        public void Validate(ServiceDescription serviceDescription,
            System.ServiceModel.ServiceHostBase serviceHostBase)
        {
        }

        bool IErrorHandler.HandleError(Exception error)
        {//or reuse instance (recommended!). see note above
            var ai = new TelemetryClient();

            ai.TrackException(error);
            return false;
        }

        void IErrorHandler.ProvideFault(Exception error,
            System.ServiceModel.Channels.MessageVersion version,
            ref System.ServiceModel.Channels.Message fault)
        {
        }
      }
    }

<span data-ttu-id="b71a0-221">Lägg till implementeringar av hello attributet toohello tjänsten:</span><span class="sxs-lookup"><span data-stu-id="b71a0-221">Add hello attribute toohello service implementations:</span></span>

    namespace WcfService4
    {
        [AiLogException]
        public class Service1 : IService1
        {
         ...

[<span data-ttu-id="b71a0-222">Exempel</span><span class="sxs-lookup"><span data-stu-id="b71a0-222">Sample</span></span>](https://github.com/AppInsightsSamples/WCFUnhandledExceptions)

## <a name="exception-performance-counters"></a><span data-ttu-id="b71a0-223">Prestandaräknare för undantag</span><span class="sxs-lookup"><span data-stu-id="b71a0-223">Exception performance counters</span></span>
<span data-ttu-id="b71a0-224">Om du har [installerat hello Application Insights Agent](app-insights-monitor-performance-live-website-now.md) på servern, du kan få ett diagram över hello undantag hastighet, mätt av .NET.</span><span class="sxs-lookup"><span data-stu-id="b71a0-224">If you have [installed hello Application Insights Agent](app-insights-monitor-performance-live-website-now.md) on your server, you can get a chart of hello exceptions rate, measured by .NET.</span></span> <span data-ttu-id="b71a0-225">Detta inkluderar både hanterade och ohanterade undantag för .NET.</span><span class="sxs-lookup"><span data-stu-id="b71a0-225">This includes both handled and unhandled .NET exceptions.</span></span>

<span data-ttu-id="b71a0-226">Öppna ett mått Explorer-bladet, lägga till ett nytt diagram och välj **undantag hastighet**, listade under prestandaräknare.</span><span class="sxs-lookup"><span data-stu-id="b71a0-226">Open a Metric Explorer blade, add a new chart, and select **Exception rate**, listed under Performance Counters.</span></span>

<span data-ttu-id="b71a0-227">hello .NET framework beräknar hello hastighet genom inventering hello antalet undantag i ett intervall och dividera med hello längden på hello intervall.</span><span class="sxs-lookup"><span data-stu-id="b71a0-227">hello .NET framework calculates hello rate by counting hello number of exceptions in an interval and dividing by hello length of hello interval.</span></span>

<span data-ttu-id="b71a0-228">Observera att det är detsamma som hello undantag antal beräknas genom hello Application Insights-portalen genom att räkna TrackException rapporter.</span><span class="sxs-lookup"><span data-stu-id="b71a0-228">Note that it will be different from hello 'Exceptions' count calculated by hello Application Insights portal by counting TrackException reports.</span></span> <span data-ttu-id="b71a0-229">Hej insamlingsintervallen är olika och hello SDK skickar inte TrackException rapporter för alla hanterade och ohanterade undantag.</span><span class="sxs-lookup"><span data-stu-id="b71a0-229">hello sampling intervals are different, and hello SDK doesn't send TrackException reports for all handled and unhandled exceptions.</span></span>

## <a name="video"></a><span data-ttu-id="b71a0-230">Video</span><span class="sxs-lookup"><span data-stu-id="b71a0-230">Video</span></span>

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/112/player] 

## <a name="next-steps"></a><span data-ttu-id="b71a0-231">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="b71a0-231">Next steps</span></span>
* [<span data-ttu-id="b71a0-232">Övervaka REST, SQL och andra anrop toodependencies</span><span class="sxs-lookup"><span data-stu-id="b71a0-232">Monitor REST, SQL and other calls toodependencies</span></span>](app-insights-asp-net-dependencies.md)
* [<span data-ttu-id="b71a0-233">Övervaka sidinläsningstider, webbläsarundantag och AJAX-anrop</span><span class="sxs-lookup"><span data-stu-id="b71a0-233">Monitor page load times, browser exceptions, and AJAX calls</span></span>](app-insights-javascript.md)
* [<span data-ttu-id="b71a0-234">Övervaka prestandaräknare</span><span class="sxs-lookup"><span data-stu-id="b71a0-234">Monitor performance counters</span></span>](app-insights-performance-counters.md)
