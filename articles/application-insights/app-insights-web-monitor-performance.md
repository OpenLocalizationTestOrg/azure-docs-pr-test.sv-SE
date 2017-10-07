---
title: "aaaMonitor appens hälso- och användningsinformation med Application Insights"
description: "Kom igång med Application Insights. Analysera användning, tillgänglighet och prestanda för din lokala eller Microsoft Azure-program."
services: application-insights
documentationcenter: 
author: CFreemanwa
manager: carmonm
ms.assetid: 40650472-e860-4c1b-a589-9956245df307
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 11/25/2015
ms.author: bwren
ms.openlocfilehash: 9230a6e65e5afb00c36122ff1d1de01ba19cd7f8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-performance-in-web-applications"></a><span data-ttu-id="bf540-104">Övervaka prestanda i webbprogram</span><span class="sxs-lookup"><span data-stu-id="bf540-104">Monitor performance in web applications</span></span>


<span data-ttu-id="bf540-105">Kontrollera att programmet fungerar bra och lär dig snabbt eventuella fel.</span><span class="sxs-lookup"><span data-stu-id="bf540-105">Make sure your application is performing well, and find out quickly about any failures.</span></span> <span data-ttu-id="bf540-106">[Application Insights] [ start] information om eventuella prestandaproblem och undantag, och hjälper dig att hitta och diagnostisera hello rot-orsaker.</span><span class="sxs-lookup"><span data-stu-id="bf540-106">[Application Insights][start] will tell you about any performance issues and exceptions, and help you find and diagnose hello root causes.</span></span>

<span data-ttu-id="bf540-107">Application Insights kan övervaka Java- och ASP.NET-webbprogram och tjänster, WCF-tjänster.</span><span class="sxs-lookup"><span data-stu-id="bf540-107">Application Insights can monitor both Java and ASP.NET web applications and services, WCF services.</span></span> <span data-ttu-id="bf540-108">De kan vara lokalt, på virtuella datorer eller som Microsoft Azure-webbplatser.</span><span class="sxs-lookup"><span data-stu-id="bf540-108">They can be hosted on-premises, on virtual machines, or as Microsoft Azure websites.</span></span> 

<span data-ttu-id="bf540-109">Hello klientsidan vidta Application Insights telemetri från webbsidor och en mängd olika enheter, inklusive iOS, Android och Windows Store-appar.</span><span class="sxs-lookup"><span data-stu-id="bf540-109">On hello client side, Application Insights can take telemetry from web pages and a wide variety of devices including iOS, Android and Windows Store apps.</span></span>

>[!Note]
> <span data-ttu-id="bf540-110">Vi har gjort en ny miljö för att hitta långsamt utför sidor i ditt webbprogram.</span><span class="sxs-lookup"><span data-stu-id="bf540-110">We have made a new experience available for finding slow performing pages in your web application.</span></span> <span data-ttu-id="bf540-111">Om du inte har åtkomst till tooit kan du aktivera det genom att konfigurera alternativ för förhandsgranskning med hello [Preview bladet](app-insights-previews.md).</span><span class="sxs-lookup"><span data-stu-id="bf540-111">If you don't have access tooit, enable it by configuring your preview options with hello [Preview blade](app-insights-previews.md).</span></span> <span data-ttu-id="bf540-112">Läs mer om den nya upplevelsen i [hitta och åtgärda flaskhalsar med hello interaktiva prestanda undersökningen](#Find-and-fix-performance-bottlenecks-with-an-interactive-Performance-investigation).</span><span class="sxs-lookup"><span data-stu-id="bf540-112">Read about this new experience in [Find and fix performance bottlenecks with hello interactive Performance investigation](#Find-and-fix-performance-bottlenecks-with-an-interactive-Performance-investigation).</span></span>

## <span data-ttu-id="bf540-113"><a name="setup"></a>Konfigurera övervakning av programprestanda</span><span class="sxs-lookup"><span data-stu-id="bf540-113"><a name="setup"></a>Set up performance monitoring</span></span>
<span data-ttu-id="bf540-114">Om du ännu inte har lagt till Application Insights tooyour projektet (om det inte har ApplicationInsights.config), väljer du något av följande sätt tooget igång:</span><span class="sxs-lookup"><span data-stu-id="bf540-114">If you haven't yet added Application Insights tooyour project (that is, if it doesn't have ApplicationInsights.config), choose one of these ways tooget started:</span></span>

* [<span data-ttu-id="bf540-115">ASP.NET-webbprogram</span><span class="sxs-lookup"><span data-stu-id="bf540-115">ASP.NET web apps</span></span>](app-insights-asp-net.md)
  * [<span data-ttu-id="bf540-116">Lägg till undantagsövervakning</span><span class="sxs-lookup"><span data-stu-id="bf540-116">Add exception monitoring</span></span>](app-insights-asp-net-exceptions.md)
  * [<span data-ttu-id="bf540-117">Lägg till beroende övervakning</span><span class="sxs-lookup"><span data-stu-id="bf540-117">Add dependency monitoring</span></span>](app-insights-monitor-performance-live-website-now.md)
* [<span data-ttu-id="bf540-118">J2EE-webbprogram</span><span class="sxs-lookup"><span data-stu-id="bf540-118">J2EE web apps</span></span>](app-insights-java-get-started.md)
  * [<span data-ttu-id="bf540-119">Lägg till beroende övervakning</span><span class="sxs-lookup"><span data-stu-id="bf540-119">Add dependency monitoring</span></span>](app-insights-java-agent.md)

## <span data-ttu-id="bf540-120"><a name="view"></a>Utforska prestandamått</span><span class="sxs-lookup"><span data-stu-id="bf540-120"><a name="view"></a>Exploring performance metrics</span></span>
<span data-ttu-id="bf540-121">I [hello Azure-portalen](https://portal.azure.com), bläddra toohello Application Insights-resurs som du har skapat för ditt program.</span><span class="sxs-lookup"><span data-stu-id="bf540-121">In [hello Azure portal](https://portal.azure.com), browse toohello Application Insights resource that you set up for your application.</span></span> <span data-ttu-id="bf540-122">hello översikt bladet visar grundläggande prestandadata:</span><span class="sxs-lookup"><span data-stu-id="bf540-122">hello overview blade shows basic performance data:</span></span>

<span data-ttu-id="bf540-123">Klicka på alla diagram toosee detalj och toosee resultat för en längre period.</span><span class="sxs-lookup"><span data-stu-id="bf540-123">Click any chart toosee more detail, and toosee results for a longer period.</span></span> <span data-ttu-id="bf540-124">Exempel på hello begäranden panelen och välj sedan ett tidsintervall:</span><span class="sxs-lookup"><span data-stu-id="bf540-124">For example, click hello Requests tile and then select a time range:</span></span>

![Klicka dig igenom toomore data och välj ett tidsintervall](./media/app-insights-web-monitor-performance/appinsights-48metrics.png)

<span data-ttu-id="bf540-126">Klicka på ett diagram toochoose vilka mått som visas, eller lägga till ett nytt diagram och välja dess mått:</span><span class="sxs-lookup"><span data-stu-id="bf540-126">Click a chart toochoose which metrics it displays, or add a new chart and select its metrics:</span></span>

![Klicka på ett diagram toochoose mått](./media/app-insights-web-monitor-performance/appinsights-61perfchoices.png)

> [!NOTE]
> <span data-ttu-id="bf540-128">**Avmarkera alla hello mått** toosee hello fullständig val som är tillgänglig.</span><span class="sxs-lookup"><span data-stu-id="bf540-128">**Uncheck all hello metrics** toosee hello full selection that is available.</span></span> <span data-ttu-id="bf540-129">hello mått är indelade i grupper. När alla medlemmar i en grupp väljs visas hello andra medlemmar i gruppen endast.</span><span class="sxs-lookup"><span data-stu-id="bf540-129">hello metrics fall into groups; when any member of a group is selected, only hello other members of that group appear.</span></span>

## <span data-ttu-id="bf540-130"><a name="metrics"></a>Vad betyder det alla?</span><span class="sxs-lookup"><span data-stu-id="bf540-130"><a name="metrics"></a>What does it all mean?</span></span> <span data-ttu-id="bf540-131">Prestanda paneler och rapporter</span><span class="sxs-lookup"><span data-stu-id="bf540-131">Performance tiles and reports</span></span>
<span data-ttu-id="bf540-132">Det finns olika prestandamått som du kan hämta.</span><span class="sxs-lookup"><span data-stu-id="bf540-132">There are various performance metrics you can get.</span></span> <span data-ttu-id="bf540-133">Vi börjar med de som visas som standard på hello programmet bladet.</span><span class="sxs-lookup"><span data-stu-id="bf540-133">Let's start with those that appear by default on hello application blade.</span></span>

### <a name="requests"></a><span data-ttu-id="bf540-134">Begäranden</span><span class="sxs-lookup"><span data-stu-id="bf540-134">Requests</span></span>
<span data-ttu-id="bf540-135">hello antal HTTP-begäranden tas emot i en angiven period.</span><span class="sxs-lookup"><span data-stu-id="bf540-135">hello number of HTTP requests received in a specified period.</span></span> <span data-ttu-id="bf540-136">Jämför detta med hello resultat på andra rapporter toosee hur din app fungerar som hello belastningen varierar.</span><span class="sxs-lookup"><span data-stu-id="bf540-136">Compare this with hello results on other reports toosee how your app behaves as hello load varies.</span></span>

<span data-ttu-id="bf540-137">HTTP-begäranden som innehåller alla begäranden om sidor, data och avbildningar som GET eller POST.</span><span class="sxs-lookup"><span data-stu-id="bf540-137">HTTP requests include all GET or POST requests for pages, data, and images.</span></span>

<span data-ttu-id="bf540-138">Klicka på hello panelen tooget antal för specifika URL: er.</span><span class="sxs-lookup"><span data-stu-id="bf540-138">Click on hello tile tooget counts for specific URLs.</span></span>

### <a name="average-response-time"></a><span data-ttu-id="bf540-139">Genomsnittlig svarstid</span><span class="sxs-lookup"><span data-stu-id="bf540-139">Average response time</span></span>
<span data-ttu-id="bf540-140">Mått hello tiden mellan en webbegäran att ange ditt program och hello svar returneras.</span><span class="sxs-lookup"><span data-stu-id="bf540-140">Measures hello time between a web request entering your application and hello response being returned.</span></span>

<span data-ttu-id="bf540-141">hello punkter visar det genomsnittliga glidande dem.</span><span class="sxs-lookup"><span data-stu-id="bf540-141">hello points show a moving average.</span></span> <span data-ttu-id="bf540-142">Om det finns en mängd begäranden, kan det finnas några som avviker från hello medelvärdet utan en uppenbara belastning eller sjunker i hello graph.</span><span class="sxs-lookup"><span data-stu-id="bf540-142">If there are a lot of requests, there might be some that deviate from hello average without an obvious peak or dip in hello graph.</span></span>

<span data-ttu-id="bf540-143">Leta efter ovanliga toppar.</span><span class="sxs-lookup"><span data-stu-id="bf540-143">Look for unusual peaks.</span></span> <span data-ttu-id="bf540-144">I allmänhet förväntat svar tid toorise med en ökning av begäranden.</span><span class="sxs-lookup"><span data-stu-id="bf540-144">In general, expect response time toorise with a rise in requests.</span></span> <span data-ttu-id="bf540-145">Om hello upphov oproportionerligt att din app träffa en resursgräns t.ex CPU eller hello kapacitet för en tjänst som används.</span><span class="sxs-lookup"><span data-stu-id="bf540-145">If hello rise is disproportionate, your app might be hitting a resource limit such as CPU or hello capacity of a service it uses.</span></span>

<span data-ttu-id="bf540-146">Klicka på hello panelen tooget gånger för specifika URL: er.</span><span class="sxs-lookup"><span data-stu-id="bf540-146">Click hello tile tooget times for specific URLs.</span></span>

![](./media/app-insights-web-monitor-performance/appinsights-42reqs.png)

### <a name="slowest-requests"></a><span data-ttu-id="bf540-147">Långsammaste begäranden</span><span class="sxs-lookup"><span data-stu-id="bf540-147">Slowest requests</span></span>
![](./media/app-insights-web-monitor-performance/appinsights-44slowest.png)

<span data-ttu-id="bf540-148">Visar vilka begäranden kan behöva prestandajustering.</span><span class="sxs-lookup"><span data-stu-id="bf540-148">Shows which requests might need performance tuning.</span></span>

### <a name="failed-requests"></a><span data-ttu-id="bf540-149">Misslyckade begäranden</span><span class="sxs-lookup"><span data-stu-id="bf540-149">Failed requests</span></span>
![](./media/app-insights-web-monitor-performance/appinsights-46failed.png)

<span data-ttu-id="bf540-150">Antalet begäranden som utlöste undantagsfel utan felhantering.</span><span class="sxs-lookup"><span data-stu-id="bf540-150">A count of requests that threw uncaught exceptions.</span></span>

<span data-ttu-id="bf540-151">Klicka på hello panelen toosee hello information om specifika problem och välja en enskild begäran toosee dess information.</span><span class="sxs-lookup"><span data-stu-id="bf540-151">Click hello tile toosee hello details of specific failures, and select an individual request toosee its detail.</span></span> 

<span data-ttu-id="bf540-152">Ett representativt urval fel sparas för enskilda kontroll.</span><span class="sxs-lookup"><span data-stu-id="bf540-152">Only a representative sample of failures is retained for individual inspection.</span></span>

### <a name="other-metrics"></a><span data-ttu-id="bf540-153">Andra mått</span><span class="sxs-lookup"><span data-stu-id="bf540-153">Other metrics</span></span>
<span data-ttu-id="bf540-154">toosee vilka andra mått som du kan visa, klicka på ett diagram och avmarkera alla hello mått toosee hello hela tillgängliga uppsättningen.</span><span class="sxs-lookup"><span data-stu-id="bf540-154">toosee what other metrics you can display, click a graph, and then deselect all hello metrics toosee hello full available set.</span></span> <span data-ttu-id="bf540-155">Klicka på (i) toosee varje mått definition.</span><span class="sxs-lookup"><span data-stu-id="bf540-155">Click (i) toosee each metric's definition.</span></span>

![Avmarkera alla mått toosee hello hela uppsättningen](./media/app-insights-web-monitor-performance/appinsights-62allchoices.png)

<span data-ttu-id="bf540-157">Att välja alla mått inaktiverar hello hello andra som kan visas på samma schema.</span><span class="sxs-lookup"><span data-stu-id="bf540-157">Selecting any metric disables hello others that can't appear on hello same chart.</span></span>

## <a name="set-alerts"></a><span data-ttu-id="bf540-158">Ange aviseringar</span><span class="sxs-lookup"><span data-stu-id="bf540-158">Set alerts</span></span>
<span data-ttu-id="bf540-159">toobe meddelas via e-post med ovanliga värden för alla mått, lägga till en avisering.</span><span class="sxs-lookup"><span data-stu-id="bf540-159">toobe notified by email of unusual values of any metric, add an alert.</span></span> <span data-ttu-id="bf540-160">Du kan välja toosend hello e-toohello kontoadministratörer eller toospecific e-postadresser.</span><span class="sxs-lookup"><span data-stu-id="bf540-160">You can choose either toosend hello email toohello account administrators, or toospecific email addresses.</span></span>

![](./media/app-insights-web-monitor-performance/appinsights-413setMetricAlert.png)

<span data-ttu-id="bf540-161">Ange hello resursen innan hello andra egenskaper.</span><span class="sxs-lookup"><span data-stu-id="bf540-161">Set hello resource before hello other properties.</span></span> <span data-ttu-id="bf540-162">Välj inte hello webtest resurser om du vill tooset aviseringar på prestanda eller användningsstatistik.</span><span class="sxs-lookup"><span data-stu-id="bf540-162">Don't choose hello webtest resources if you want tooset alerts on performance or usage metrics.</span></span>

<span data-ttu-id="bf540-163">Vara försiktig toonote hello enheter där du uppmanas tooenter hello tröskelvärdet.</span><span class="sxs-lookup"><span data-stu-id="bf540-163">Be careful toonote hello units in which you're asked tooenter hello threshold value.</span></span>

<span data-ttu-id="bf540-164">*Hello Lägg till avisering om visas inte.*</span><span class="sxs-lookup"><span data-stu-id="bf540-164">*I don't see hello Add Alert button.*</span></span> <span data-ttu-id="bf540-165">-Är detta en grupp konto toowhich som du har skrivskyddad åtkomst?</span><span class="sxs-lookup"><span data-stu-id="bf540-165">- Is this a group account toowhich you have read-only access?</span></span> <span data-ttu-id="bf540-166">Kontrollera med hello kontoadministratör.</span><span class="sxs-lookup"><span data-stu-id="bf540-166">Check with hello account administrator.</span></span>

## <span data-ttu-id="bf540-167"><a name="diagnosis"></a>Diagnostisera problem</span><span class="sxs-lookup"><span data-stu-id="bf540-167"><a name="diagnosis"></a>Diagnosing issues</span></span>
<span data-ttu-id="bf540-168">Här följer några tips för att hitta och diagnostisera prestandaproblem:</span><span class="sxs-lookup"><span data-stu-id="bf540-168">Here are a few tips for finding and diagnosing performance issues:</span></span>

* <span data-ttu-id="bf540-169">Ställ in [webbtester] [ availability] toobe aviserad om webbplatsen kraschar eller svarar långsamt eller felaktigt.</span><span class="sxs-lookup"><span data-stu-id="bf540-169">Set up [web tests][availability] toobe alerted if your web site goes down or responds incorrectly or slowly.</span></span> 
* <span data-ttu-id="bf540-170">Jämförelseantalet hello begäran med andra mått toosee om misslyckade eller långsamma svar är relaterade tooload.</span><span class="sxs-lookup"><span data-stu-id="bf540-170">Compare hello Request count with other metrics toosee if failures or slow response are related tooload.</span></span>
* <span data-ttu-id="bf540-171">[Infoga och Sök trace-satser] [ diagnostic] i din kod toohelp identifiera problem.</span><span class="sxs-lookup"><span data-stu-id="bf540-171">[Insert and search trace statements][diagnostic] in your code toohelp pinpoint problems.</span></span>
* <span data-ttu-id="bf540-172">Övervaka ditt webbprogram i åtgärden med [direktsänd dataström med mått][livestream].</span><span class="sxs-lookup"><span data-stu-id="bf540-172">Monitor your Web app in operation with [Live Metrics Stream][livestream].</span></span>
* <span data-ttu-id="bf540-173">Avbilda hello tillstånd på ditt .net-program med [ögonblicksbild felsökare][snapshot].</span><span class="sxs-lookup"><span data-stu-id="bf540-173">Capture hello state of your .Net application with [Snapshot Debugger][snapshot].</span></span>

## <a name="find-and-fix-performance-bottlenecks-with-an-interactive-performance-investigation"></a><span data-ttu-id="bf540-174">Hitta och åtgärda flaskhalsar med en interaktiv prestanda undersökning</span><span class="sxs-lookup"><span data-stu-id="bf540-174">Find and fix performance bottlenecks with an interactive performance investigation</span></span>

<span data-ttu-id="bf540-175">Du kan använda hello ny Application Insights interaktiva prestanda undersökningen toolocate områden av ditt webbprogram som saktar ned prestandan.</span><span class="sxs-lookup"><span data-stu-id="bf540-175">You can use hello new Application Insights interactive performance investigation toolocate areas of your Web app that are slowing down overall performance.</span></span> <span data-ttu-id="bf540-176">Du kan snabbt hitta vissa sidor som saktar ned och använder hello [Profiling verktyget](app-insights-profiler.md) toosee om det finns en korrelation mellan dessa sidor.</span><span class="sxs-lookup"><span data-stu-id="bf540-176">You can quickly find specific pages that are slowing down, and use hello [Profiling tool](app-insights-profiler.md) toosee if there is a correlation between these pages.</span></span>

### <a name="create-a-list-of-slow-performing-pages"></a><span data-ttu-id="bf540-177">Skapa en lista över långsamma prestanda sidor</span><span class="sxs-lookup"><span data-stu-id="bf540-177">Create a list of slow performing pages</span></span> 

<span data-ttu-id="bf540-178">hello första steget för att söka efter prestandaproblem är tooget en lista över hello långsam svarar sidor.</span><span class="sxs-lookup"><span data-stu-id="bf540-178">hello first step for finding performance issues is tooget a list of hello slow responding pages.</span></span> <span data-ttu-id="bf540-179">hello skärmbilden nedan visas hur du använder hello prestanda bladet tooget en lista över möjliga sidor tooinvestigate ytterligare.</span><span class="sxs-lookup"><span data-stu-id="bf540-179">hello screen shot below demonstrates using hello Performance blade tooget a list of potential pages tooinvestigate further.</span></span> <span data-ttu-id="bf540-180">Du kan snabbt se från den här sidan att det var en nedgång i hello svarstid hello-appen på cirka 6:00 PM och igen klockan cirka 10.</span><span class="sxs-lookup"><span data-stu-id="bf540-180">You can quickly see from this page that there was a slow-down in hello response time of hello app at approximately 6:00 PM and again at approximately 10 PM.</span></span> <span data-ttu-id="bf540-181">Du kan också se att hello GET/kundinformation åtgärden hade vissa långvariga åtgärder med en median svarstid 507.05 millisekunder.</span><span class="sxs-lookup"><span data-stu-id="bf540-181">You can also see that hello GET customer/details operation had some long running operations with a median response time of 507.05 milliseconds.</span></span> 

![Application Insights interaktiva prestanda](./media/app-insights-web-monitor-performance/performance1.png)

### <a name="drill-down-on-specific-pages"></a><span data-ttu-id="bf540-183">Detaljnivån på specifika sidor</span><span class="sxs-lookup"><span data-stu-id="bf540-183">Drill down on specific pages</span></span>

<span data-ttu-id="bf540-184">När du har en ögonblicksbild av appens prestanda kan få du mer information på specifika åtgärder för långsamma prestanda.</span><span class="sxs-lookup"><span data-stu-id="bf540-184">Once you have a snapshot of your app's performance, you can get more details on specific slow-performing operations.</span></span> <span data-ttu-id="bf540-185">Klicka på någon åtgärd i hello toosee hello information som visas nedan.</span><span class="sxs-lookup"><span data-stu-id="bf540-185">Click on any operation in hello list toosee hello details as shown below.</span></span> <span data-ttu-id="bf540-186">Du kan se om hello prestanda var baserat på ett beroende från hello diagram.</span><span class="sxs-lookup"><span data-stu-id="bf540-186">From hello chart you can see if hello performance was based on a dependency.</span></span> <span data-ttu-id="bf540-187">Du kan också se hur många användare erfarna hello olika svarstider.</span><span class="sxs-lookup"><span data-stu-id="bf540-187">You can also see how many users experienced hello various response times.</span></span> 

![Application Insights operations bladet](./media/app-insights-web-monitor-performance/performance5.png)

### <a name="drill-down-on-a-specific-time-period"></a><span data-ttu-id="bf540-189">Detaljnivån för en viss tidsperiod</span><span class="sxs-lookup"><span data-stu-id="bf540-189">Drill down on a specific time period</span></span>

<span data-ttu-id="bf540-190">När du har identifierat en punkt i tiden tooinvestigate kan öka detaljnivån ytterligare toolook på hello specifika åtgärder som kan ha orsakat hello prestanda sidåtergivningen.</span><span class="sxs-lookup"><span data-stu-id="bf540-190">After you have identified a point in time tooinvestigate, drill down even further toolook at hello specific operations that might have caused hello performance slow-down.</span></span> <span data-ttu-id="bf540-191">När du klickar på en viss punkt i tiden hämta hello information om hello sida som visas nedan.</span><span class="sxs-lookup"><span data-stu-id="bf540-191">When you click on a specific point in time you get hello details of hello page as shown below.</span></span> <span data-ttu-id="bf540-192">I hello ser exemplet nedan du hello åtgärderna för en viss tidsperiod tillsammans med hello server svarskoder och hello åtgärden varaktighet.</span><span class="sxs-lookup"><span data-stu-id="bf540-192">In hello example below you can see hello operations listed for a given time period along with hello server response codes and hello operation duration.</span></span> <span data-ttu-id="bf540-193">Du kan också ha hello URL: en för att öppna en TFS-arbetsobjekt om du behöver toosend denna information tooyour Utvecklingsteamet.</span><span class="sxs-lookup"><span data-stu-id="bf540-193">You also have hello url for opening a TFS work item if you need toosend this information tooyour development team.</span></span>

![Tidsintervallet för Application Insights](./media/app-insights-web-monitor-performance/performance2.png)

### <a name="drill-down-on-a-specific-operation"></a><span data-ttu-id="bf540-195">Detaljnivån med en specifik åtgärd</span><span class="sxs-lookup"><span data-stu-id="bf540-195">Drill down on a specific operation</span></span>

<span data-ttu-id="bf540-196">När du har identifierat en punkt i tiden tooinvestigate kan öka detaljnivån ytterligare toolook på hello specifika åtgärder som kan ha orsakat hello prestanda sidåtergivningen.</span><span class="sxs-lookup"><span data-stu-id="bf540-196">After you have identified a point in time tooinvestigate, drill down even further toolook at hello specific operations that might have caused hello performance slow-down.</span></span> <span data-ttu-id="bf540-197">Klicka på en åtgärd från hello listan toosee hello närmare hello enligt nedan.</span><span class="sxs-lookup"><span data-stu-id="bf540-197">Click on an operation from hello list toosee hello details of hello operation as shown below.</span></span> <span data-ttu-id="bf540-198">I det här exemplet som du kan se att hello misslyckades och Application Insights har angett hello information om hello utlöste undantaget hello program.</span><span class="sxs-lookup"><span data-stu-id="bf540-198">In this example you can see that hello operation failed, and Application Insights has provided hello details of hello exception hello application threw.</span></span> <span data-ttu-id="bf540-199">Igen, kan du enkelt skapa en TFS-arbetsobjekt från det här bladet.</span><span class="sxs-lookup"><span data-stu-id="bf540-199">Again, you can easily create a TFS work item from this blade.</span></span>

![Application Insights åtgärden bladet](./media/app-insights-web-monitor-performance/performance3.png)


## <span data-ttu-id="bf540-201"><a name="next"></a>Nästa steg</span><span class="sxs-lookup"><span data-stu-id="bf540-201"><a name="next"></a>Next steps</span></span>
<span data-ttu-id="bf540-202">[Webbtester] [ availability] -webbegäranden skickat tooyour program med jämna mellanrum från hello världen.</span><span class="sxs-lookup"><span data-stu-id="bf540-202">[Web tests][availability] - Have web requests sent tooyour application at regular intervals from around hello world.</span></span>

<span data-ttu-id="bf540-203">[Fånga och söka diagnostikspår] [ diagnostic] – Infoga spårning av anrop och gå igenom hello resultat toopinpoint problem.</span><span class="sxs-lookup"><span data-stu-id="bf540-203">[Capture and search diagnostic traces][diagnostic] - Insert trace calls and sift through hello results toopinpoint issues.</span></span>

<span data-ttu-id="bf540-204">[Spårning av användning] [ usage] -ta reda på hur personer använder ditt program.</span><span class="sxs-lookup"><span data-stu-id="bf540-204">[Usage tracking][usage] - Find out how people use your application.</span></span>

<span data-ttu-id="bf540-205">[Felsökning av] [ qna] - och frågor och svar</span><span class="sxs-lookup"><span data-stu-id="bf540-205">[Troubleshooting][qna] - and Q & A</span></span>



<!--Link references-->

[availability]: app-insights-monitor-web-app-availability.md
[diagnostic]: app-insights-diagnostic-search.md
[greenbrown]: app-insights-asp-net.md
[qna]: app-insights-troubleshoot-faq.md
[redfield]: app-insights-monitor-performance-live-website-now.md
[start]: app-insights-overview.md
[usage]: app-insights-web-track-usage.md
[livestream]: app-insights-live-stream.md
[snapshot]: app-insights-snapshot-debugger.md



