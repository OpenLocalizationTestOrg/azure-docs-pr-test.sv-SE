---
title: "aaaDependency spårning i Azure Application Insights | Microsoft Docs"
description: "Analysera användningen, tillgängligheten och prestanda i din lokala program eller Microsoft Azure-webbapp med Application Insights."
services: application-insights
documentationcenter: .net
author: CFreemanwa
manager: carmonm
ms.assetid: d15c4ca8-4c1a-47ab-a03d-c322b4bb2a9e
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 05/04/2017
ms.author: bwren
ms.openlocfilehash: e72f5465462ae8e64363cbbaa62911aff636c504
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="set-up-application-insights-dependency-tracking"></a><span data-ttu-id="9db63-103">Konfigurera Application Insights: beroende-spårning</span><span class="sxs-lookup"><span data-stu-id="9db63-103">Set up Application Insights: Dependency tracking</span></span>
<span data-ttu-id="9db63-104">En *beroende* är en extern komponent som anropas av din app.</span><span class="sxs-lookup"><span data-stu-id="9db63-104">A *dependency* is an external component that is called by your app.</span></span> <span data-ttu-id="9db63-105">Det är normalt en tjänst som anropas med HTTP, eller en databas eller ett filsystem.</span><span class="sxs-lookup"><span data-stu-id="9db63-105">It's typically a service called using HTTP, or a database, or a file system.</span></span> <span data-ttu-id="9db63-106">[Application Insights](app-insights-overview.md) mäter hur länge programmet väntar på beroenden och hur ofta en beroendeanropet misslyckas.</span><span class="sxs-lookup"><span data-stu-id="9db63-106">[Application Insights](app-insights-overview.md) measures how long your application waits for dependencies and how often a dependency call fails.</span></span> <span data-ttu-id="9db63-107">Du kan undersöka specifika anrop och koppla dem toorequests och undantag.</span><span class="sxs-lookup"><span data-stu-id="9db63-107">You can investigate specific calls, and relate them toorequests and exceptions.</span></span>

![exempeldiagram](./media/app-insights-asp-net-dependencies/10-intro.png)

<span data-ttu-id="9db63-109">beroendeövervakare för hello out box rapporter för närvarande anrop toothese typer av beroenden:</span><span class="sxs-lookup"><span data-stu-id="9db63-109">hello out-of-the-box dependency monitor currently reports calls toothese  types of dependencies:</span></span>

* <span data-ttu-id="9db63-110">Server</span><span class="sxs-lookup"><span data-stu-id="9db63-110">Server</span></span>
  * <span data-ttu-id="9db63-111">SQL-databaser</span><span class="sxs-lookup"><span data-stu-id="9db63-111">SQL databases</span></span>
  * <span data-ttu-id="9db63-112">ASP.NET-webbprogram och WCF-tjänster som använder HTTP-baserade bindningar</span><span class="sxs-lookup"><span data-stu-id="9db63-112">ASP.NET web and WCF services that use HTTP-based bindings</span></span>
  * <span data-ttu-id="9db63-113">Lokal eller fjärransluten HTTP-anrop</span><span class="sxs-lookup"><span data-stu-id="9db63-113">Local or remote HTTP calls</span></span>
  * <span data-ttu-id="9db63-114">Azure Cosmos DB, tabell, blob-lagring och kön</span><span class="sxs-lookup"><span data-stu-id="9db63-114">Azure Cosmos DB, table, blob storage, and queue</span></span>
* <span data-ttu-id="9db63-115">Webbsidor</span><span class="sxs-lookup"><span data-stu-id="9db63-115">Web pages</span></span>
  * <span data-ttu-id="9db63-116">AJAX-anrop</span><span class="sxs-lookup"><span data-stu-id="9db63-116">AJAX calls</span></span>

<span data-ttu-id="9db63-117">Övervaka fungerar med hjälp av [byte kod instrumentation](https://msdn.microsoft.com/library/z9z62c29.aspx) runt valda metoder.</span><span class="sxs-lookup"><span data-stu-id="9db63-117">Monitoring works by using [byte code instrumentation](https://msdn.microsoft.com/library/z9z62c29.aspx) around selected methods.</span></span> <span data-ttu-id="9db63-118">Prestanda försämras är minimal.</span><span class="sxs-lookup"><span data-stu-id="9db63-118">Performance overhead is minimal.</span></span>

<span data-ttu-id="9db63-119">Du kan också skriva egna SDK anrop toomonitor andra beroenden, både i hello klienten och servern kod med hjälp av hello [TrackDependency API](app-insights-api-custom-events-metrics.md#trackdependency).</span><span class="sxs-lookup"><span data-stu-id="9db63-119">You can also write your own SDK calls toomonitor other dependencies, both in hello client and server code, using hello [TrackDependency API](app-insights-api-custom-events-metrics.md#trackdependency).</span></span>

## <a name="set-up-dependency-monitoring"></a><span data-ttu-id="9db63-120">Konfigurera övervakning av beroende</span><span class="sxs-lookup"><span data-stu-id="9db63-120">Set up dependency monitoring</span></span>
<span data-ttu-id="9db63-121">Partiell beroendeinformation samlas in automatiskt med hello [Application Insights SDK](app-insights-asp-net.md).</span><span class="sxs-lookup"><span data-stu-id="9db63-121">Partial dependency information is collected automatically by hello [Application Insights SDK](app-insights-asp-net.md).</span></span> <span data-ttu-id="9db63-122">tooget fullständiga data, installera hello lämplig agent för hello-värdservern.</span><span class="sxs-lookup"><span data-stu-id="9db63-122">tooget complete data, install hello appropriate agent for hello host server.</span></span>

| <span data-ttu-id="9db63-123">Plattform</span><span class="sxs-lookup"><span data-stu-id="9db63-123">Platform</span></span> | <span data-ttu-id="9db63-124">Installera</span><span class="sxs-lookup"><span data-stu-id="9db63-124">Install</span></span> |
| --- | --- |
| <span data-ttu-id="9db63-125">IIS-servern</span><span class="sxs-lookup"><span data-stu-id="9db63-125">IIS Server</span></span> |<span data-ttu-id="9db63-126">Antingen [installera statusövervakaren på servern](app-insights-monitor-performance-live-website-now.md) eller [uppgradera ditt program too.NET framework 4.6 eller senare](http://go.microsoft.com/fwlink/?LinkId=528259) och installera hello [Application Insights SDK](app-insights-asp-net.md) i din app.</span><span class="sxs-lookup"><span data-stu-id="9db63-126">Either [install Status Monitor on your server](app-insights-monitor-performance-live-website-now.md) or [Upgrade your application too.NET framework 4.6 or later](http://go.microsoft.com/fwlink/?LinkId=528259) and install hello [Application Insights SDK](app-insights-asp-net.md)  in your app.</span></span> |
| <span data-ttu-id="9db63-127">Azure-webbapp</span><span class="sxs-lookup"><span data-stu-id="9db63-127">Azure Web App</span></span> |<span data-ttu-id="9db63-128">I Kontrollpanelen web app, [öppna hello Application Insights-bladet i Kontrollpanelen web app](app-insights-azure-web-apps.md) och välj Installera om du uppmanas.</span><span class="sxs-lookup"><span data-stu-id="9db63-128">In your web app control panel, [open hello Application Insights blade in your web app control panel](app-insights-azure-web-apps.md) and choose Install if prompted.</span></span> |
| <span data-ttu-id="9db63-129">Azure-molntjänst</span><span class="sxs-lookup"><span data-stu-id="9db63-129">Azure Cloud Service</span></span> |<span data-ttu-id="9db63-130">[Använd startaktivitet](app-insights-cloudservices.md) eller [installera .NET framework 4.6 +](../cloud-services/cloud-services-dotnet-install-dotnet.md)</span><span class="sxs-lookup"><span data-stu-id="9db63-130">[Use startup task](app-insights-cloudservices.md) or [Install .NET framework 4.6+](../cloud-services/cloud-services-dotnet-install-dotnet.md)</span></span> |

## <a name="where-toofind-dependency-data"></a><span data-ttu-id="9db63-131">Där toofind beroendedata</span><span class="sxs-lookup"><span data-stu-id="9db63-131">Where toofind dependency data</span></span>
* <span data-ttu-id="9db63-132">[Programavbildningen](#application-map) visualizes beroenden mellan appen och angränsande komponenter.</span><span class="sxs-lookup"><span data-stu-id="9db63-132">[Application Map](#application-map) visualizes dependencies between your app and neighbouring components.</span></span>
* <span data-ttu-id="9db63-133">[Prestanda, webbläsare och fel blad](#performance-and-blades) visa beroende serverdata.</span><span class="sxs-lookup"><span data-stu-id="9db63-133">[Performance, browser, and failure blades](#performance-and-blades) show server dependency data.</span></span>
* <span data-ttu-id="9db63-134">[Webbläsare bladet](#ajax-calls) visar AJAX-anrop från användarnas webbläsare.</span><span class="sxs-lookup"><span data-stu-id="9db63-134">[Browsers blade](#ajax-calls) shows AJAX calls from your users' browsers.</span></span>
* <span data-ttu-id="9db63-135">[Klicka dig igenom från långsamma eller misslyckade förfrågningar](#diagnose-slow-requests) toocheck sina beroendeanrop.</span><span class="sxs-lookup"><span data-stu-id="9db63-135">[Click through from slow or failed requests](#diagnose-slow-requests) toocheck their dependency calls.</span></span>
* <span data-ttu-id="9db63-136">[Analytics](#analytics) kan vara används tooquery beroendedata.</span><span class="sxs-lookup"><span data-stu-id="9db63-136">[Analytics](#analytics) can be used tooquery dependency data.</span></span>

## <a name="application-map"></a><span data-ttu-id="9db63-137">Programkarta</span><span class="sxs-lookup"><span data-stu-id="9db63-137">Application Map</span></span>
<span data-ttu-id="9db63-138">Programavbildningen fungerar som en visual stöd toodiscovering beroenden mellan hello komponenter i ditt program.</span><span class="sxs-lookup"><span data-stu-id="9db63-138">Application Map acts as a visual aid toodiscovering dependencies between hello components of your application.</span></span> <span data-ttu-id="9db63-139">Den genereras automatiskt från hello telemetri från din app.</span><span class="sxs-lookup"><span data-stu-id="9db63-139">It is automatically generated from hello telemetry from your app.</span></span> <span data-ttu-id="9db63-140">Det här exemplet visar AJAX-anrop från hello webbläsare skript och REST-anrop från hello server app tootwo externa tjänster.</span><span class="sxs-lookup"><span data-stu-id="9db63-140">This example shows AJAX calls from hello browser scripts and REST calls from hello server app tootwo external services.</span></span>

![Programkarta](./media/app-insights-asp-net-dependencies/08.png)

* <span data-ttu-id="9db63-142">**Navigera från hello rutorna** toorelevant beroende och andra diagram.</span><span class="sxs-lookup"><span data-stu-id="9db63-142">**Navigate from hello boxes** toorelevant dependency and other charts.</span></span>
* <span data-ttu-id="9db63-143">**PIN-kod hello kartan** toohello [instrumentpanelen](app-insights-dashboards.md), där den fungerar som den ska.</span><span class="sxs-lookup"><span data-stu-id="9db63-143">**Pin hello map** toohello [dashboard](app-insights-dashboards.md), where it will be fully functional.</span></span>

<span data-ttu-id="9db63-144">[Läs mer](app-insights-app-map.md).</span><span class="sxs-lookup"><span data-stu-id="9db63-144">[Learn more](app-insights-app-map.md).</span></span>

## <a name="performance-and-failure-blades"></a><span data-ttu-id="9db63-145">Prestanda och fel blad</span><span class="sxs-lookup"><span data-stu-id="9db63-145">Performance and failure blades</span></span>
<span data-ttu-id="9db63-146">hello prestanda bladet visar hello varaktighet för beroendeanrop gjorda av hello server-appen.</span><span class="sxs-lookup"><span data-stu-id="9db63-146">hello performance blade shows hello duration of dependency calls made by hello server app.</span></span> <span data-ttu-id="9db63-147">Det finns en översikt över diagram och en tabell som är segmenterat av anrop.</span><span class="sxs-lookup"><span data-stu-id="9db63-147">There's a summary chart and a table segmented by call.</span></span>

![Prestandadiagram bladet beroende](./media/app-insights-asp-net-dependencies/dependencies-in-performance-blade.png)

<span data-ttu-id="9db63-149">Klicka dig igenom hello sammanfattning diagram eller hello tabellen objekt toosearch raw förekomster av dessa anrop.</span><span class="sxs-lookup"><span data-stu-id="9db63-149">Click through hello summary charts or hello table items toosearch raw occurrences of these calls.</span></span>

![Beroende anropet instanser](./media/app-insights-asp-net-dependencies/dependency-call-instance.png)

<span data-ttu-id="9db63-151">**Fel antal** visas på hello **fel** bladet.</span><span class="sxs-lookup"><span data-stu-id="9db63-151">**Failure counts** are shown on hello **Failures** blade.</span></span> <span data-ttu-id="9db63-152">Ett fel är alla returkoder som inte är i intervallet-hello 399 200, eller okänd.</span><span class="sxs-lookup"><span data-stu-id="9db63-152">A failure is any return code that is not in hello range 200-399, or unknown.</span></span>

> [!NOTE]
> <span data-ttu-id="9db63-153">**100% fel?**</span><span class="sxs-lookup"><span data-stu-id="9db63-153">**100% failures?**</span></span> <span data-ttu-id="9db63-154">-Detta innebär förmodligen att du endast får partiella beroendedata.</span><span class="sxs-lookup"><span data-stu-id="9db63-154">- This probably indicates that you are only getting partial dependency data.</span></span> <span data-ttu-id="9db63-155">Du behöver för[ställa in beroende övervakning lämpliga tooyour plattform](#set-up-dependency-monitoring).</span><span class="sxs-lookup"><span data-stu-id="9db63-155">You need too[set up dependency monitoring appropriate tooyour platform](#set-up-dependency-monitoring).</span></span>
>
>

## <a name="ajax-calls"></a><span data-ttu-id="9db63-156">AJAX-anrop</span><span class="sxs-lookup"><span data-stu-id="9db63-156">AJAX Calls</span></span>
<span data-ttu-id="9db63-157">hello webbläsare bladet visar hello varaktighet och antalet misslyckade AJAX-anrop från [JavaScript i webbsidorna](app-insights-javascript.md).</span><span class="sxs-lookup"><span data-stu-id="9db63-157">hello Browsers blade shows hello duration and failure rate of AJAX calls from [JavaScript in your web pages](app-insights-javascript.md).</span></span> <span data-ttu-id="9db63-158">De visas som beroenden.</span><span class="sxs-lookup"><span data-stu-id="9db63-158">They are shown as Dependencies.</span></span>

## <span data-ttu-id="9db63-159"><a name="diagnosis"></a>Diagnostisera långsam begäranden</span><span class="sxs-lookup"><span data-stu-id="9db63-159"><a name="diagnosis"></a> Diagnose slow requests</span></span>
<span data-ttu-id="9db63-160">Varje begäran händelse är associerad med hello beroendeanrop, undantag och andra händelser som spåras medan appen bearbetar hello begäran.</span><span class="sxs-lookup"><span data-stu-id="9db63-160">Each request event is associated with hello dependency calls, exceptions and other events that are tracked while your app is processing hello request.</span></span> <span data-ttu-id="9db63-161">Så om vissa begäranden felaktigt, kan du läsa mer om det är på grund av tooslow svar från ett beroende.</span><span class="sxs-lookup"><span data-stu-id="9db63-161">So if some requests are performing badly, you can find out whether it's due tooslow responses from a dependency.</span></span>

<span data-ttu-id="9db63-162">Låt oss gå igenom ett exempel på som.</span><span class="sxs-lookup"><span data-stu-id="9db63-162">Let's walk through an example of that.</span></span>

### <a name="tracing-from-requests-toodependencies"></a><span data-ttu-id="9db63-163">Spårning från begäranden toodependencies</span><span class="sxs-lookup"><span data-stu-id="9db63-163">Tracing from requests toodependencies</span></span>
<span data-ttu-id="9db63-164">Öppna hello prestanda bladet och titta på hello rutnätet begäranden:</span><span class="sxs-lookup"><span data-stu-id="9db63-164">Open hello Performance blade, and look at hello grid of requests:</span></span>

![Lista med begäranden med medelvärden och antal](./media/app-insights-asp-net-dependencies/02-reqs.png)

<span data-ttu-id="9db63-166">hello upp en tar mycket lång tid.</span><span class="sxs-lookup"><span data-stu-id="9db63-166">hello top one is taking very long.</span></span> <span data-ttu-id="9db63-167">Låt oss se om vi ta reda på om hello tid det tar.</span><span class="sxs-lookup"><span data-stu-id="9db63-167">Let's see if we can find out where hello time is spent.</span></span>

<span data-ttu-id="9db63-168">Klicka på de rad toosee enskilda begäran:</span><span class="sxs-lookup"><span data-stu-id="9db63-168">Click that row toosee individual request events:</span></span>

![Lista över begäran förekomster](./media/app-insights-asp-net-dependencies/03-instances.png)

<span data-ttu-id="9db63-170">Klicka på alla långvariga instans tooinspect den ytterligare och rulla ned toohello remote beroende anrop relaterade toothis begäran:</span><span class="sxs-lookup"><span data-stu-id="9db63-170">Click any long-running instance tooinspect it further, and scroll down toohello remote dependency calls related toothis request:</span></span>

![Hitta anrop tooRemote beroenden, identifiera onormal varaktighet](./media/app-insights-asp-net-dependencies/04-dependencies.png)

<span data-ttu-id="9db63-172">Det ser ut som de flesta hello tid behandling denna begäran har använt i en anropet tooa lokal tjänst.</span><span class="sxs-lookup"><span data-stu-id="9db63-172">It looks like most of hello time servicing this request was spent in a call tooa local service.</span></span>

<span data-ttu-id="9db63-173">Välj den raden tooget mer information:</span><span class="sxs-lookup"><span data-stu-id="9db63-173">Select that row tooget more information:</span></span>

![Klicka dig igenom den fjärranslutna beroende tooidentify hello sällan](./media/app-insights-asp-net-dependencies/05-detail.png)

<span data-ttu-id="9db63-175">Ser ut som detta är där hello problem.</span><span class="sxs-lookup"><span data-stu-id="9db63-175">Looks like this is where hello problem is.</span></span> <span data-ttu-id="9db63-176">Vi har precisa hello problemet, så nu vi bara behöver toofind reda på varför anropet tar så lång tid.</span><span class="sxs-lookup"><span data-stu-id="9db63-176">We've pinpointed hello problem, so now we just need toofind out why that call is taking so long.</span></span>

### <a name="request-timeline"></a><span data-ttu-id="9db63-177">Tidslinje för begäran</span><span class="sxs-lookup"><span data-stu-id="9db63-177">Request timeline</span></span>
<span data-ttu-id="9db63-178">I annat fall finns ingen beroendeanropet som är särskilt lång.</span><span class="sxs-lookup"><span data-stu-id="9db63-178">In a different case, there is no dependency call that is particularly long.</span></span> <span data-ttu-id="9db63-179">Men genom att växla toohello tidslinjevyn kan vi se där hello fördröjning uppstod i vår intern bearbetning:</span><span class="sxs-lookup"><span data-stu-id="9db63-179">But by switching toohello timeline view, we can see where hello delay occurred in our internal processing:</span></span>

![Hitta anrop tooRemote beroenden, identifiera onormal varaktighet](./media/app-insights-asp-net-dependencies/04-1.png)

<span data-ttu-id="9db63-181">Det verkar toobe ett stort mellanrum efter hello första beroendeanropet så att vi ska titta på vår kod toosee varför som är.</span><span class="sxs-lookup"><span data-stu-id="9db63-181">There seems toobe a big gap after hello first dependency call, so we should look at our code toosee why that is.</span></span>

### <a name="profile-your-live-site"></a><span data-ttu-id="9db63-182">Webbplatsen live-profil</span><span class="sxs-lookup"><span data-stu-id="9db63-182">Profile your live site</span></span>

<span data-ttu-id="9db63-183">Ingen idé där hello tillfälle? Hej [Application Insights profiler](app-insights-profiler.md) spårningar HTTP anropar tooyour aktiva plats och visar vilka funktioner i koden tog hello längsta tid.</span><span class="sxs-lookup"><span data-stu-id="9db63-183">No idea where hello time goes? hello [Application Insights profiler](app-insights-profiler.md) traces HTTP calls tooyour live site and shows you which functions in your code took hello longest time.</span></span>

## <a name="failed-requests"></a><span data-ttu-id="9db63-184">Misslyckade begäranden</span><span class="sxs-lookup"><span data-stu-id="9db63-184">Failed requests</span></span>
<span data-ttu-id="9db63-185">Misslyckade begäranden kan även vara associerad med misslyckade anrop toodependencies.</span><span class="sxs-lookup"><span data-stu-id="9db63-185">Failed requests might also be associated with failed calls toodependencies.</span></span> <span data-ttu-id="9db63-186">Vi kan igen och klicka på via tootrack ned hello problem.</span><span class="sxs-lookup"><span data-stu-id="9db63-186">Again, we can click through tootrack down hello problem.</span></span>

![Klicka på hello diagram för misslyckade begäranden](./media/app-insights-asp-net-dependencies/06-fail.png)

<span data-ttu-id="9db63-188">Klicka dig igenom tooan förekomsten av en misslyckad begäran och titta på dess associerade händelserna.</span><span class="sxs-lookup"><span data-stu-id="9db63-188">Click through tooan occurrence of a failed request, and look at its associated events.</span></span>

![Klicka på typ av begäran, hello tooget tooa olika instansvyn för Hej samma instans, klickar du på den tooget undantagsinformation.](./media/app-insights-asp-net-dependencies/07-faildetail.png)

## <a name="analytics"></a><span data-ttu-id="9db63-190">Analys</span><span class="sxs-lookup"><span data-stu-id="9db63-190">Analytics</span></span>
<span data-ttu-id="9db63-191">Du kan spåra beroenden i hello [Log Analytics-frågespråket](https://docs.loganalytics.io/).</span><span class="sxs-lookup"><span data-stu-id="9db63-191">You can track dependencies in hello [Log Analytics query language](https://docs.loganalytics.io/).</span></span> <span data-ttu-id="9db63-192">Här följer några exempel.</span><span class="sxs-lookup"><span data-stu-id="9db63-192">Here are some examples.</span></span>

* <span data-ttu-id="9db63-193">Hitta alla misslyckade beroendeanrop:</span><span class="sxs-lookup"><span data-stu-id="9db63-193">Find any failed dependency calls:</span></span>

```

    dependencies | where success != "True" | take 10
```

* <span data-ttu-id="9db63-194">Hitta AJAX-anrop:</span><span class="sxs-lookup"><span data-stu-id="9db63-194">Find AJAX calls:</span></span>

```

    dependencies | where client_Type == "Browser" | take 10
```

* <span data-ttu-id="9db63-195">Hitta beroendeanrop som är associerade med begäranden:</span><span class="sxs-lookup"><span data-stu-id="9db63-195">Find dependency calls associated with requests:</span></span>

```

    dependencies
    | where timestamp > ago(1d) and  client_Type != "Browser"
    | join (requests | where timestamp > ago(1d))
      on operation_Id  
```


* <span data-ttu-id="9db63-196">Hitta AJAX-anrop som är associerade med sidvisningar:</span><span class="sxs-lookup"><span data-stu-id="9db63-196">Find AJAX calls associated with page views:</span></span>

```

    dependencies
    | where timestamp > ago(1d) and  client_Type == "Browser"
    | join (browserTimings | where timestamp > ago(1d))
      on operation_Id
```



## <a name="custom-dependency-tracking"></a><span data-ttu-id="9db63-197">Anpassade beroende spårning</span><span class="sxs-lookup"><span data-stu-id="9db63-197">Custom dependency tracking</span></span>
<span data-ttu-id="9db63-198">hello standard beroende spårning modulen upptäcker automatiskt externa beroenden som databaser och REST API: er.</span><span class="sxs-lookup"><span data-stu-id="9db63-198">hello standard dependency-tracking module automatically discovers external dependencies such as databases and REST APIs.</span></span> <span data-ttu-id="9db63-199">Men du kanske vissa ytterligare komponenter toobe behandlas i hello samma sätt.</span><span class="sxs-lookup"><span data-stu-id="9db63-199">But you might want some additional components toobe treated in hello same way.</span></span>

<span data-ttu-id="9db63-200">Du kan skriva kod som skickar beroendeinformation med hello samma [TrackDependency API](app-insights-api-custom-events-metrics.md#trackdependency) som används av hello standard moduler.</span><span class="sxs-lookup"><span data-stu-id="9db63-200">You can write code that sends dependency information, using hello same [TrackDependency API](app-insights-api-custom-events-metrics.md#trackdependency) that is used by hello standard modules.</span></span>

<span data-ttu-id="9db63-201">Du skapar din kod med en sammansättning som du inte skriva själv, du kan tiden för alla hello anrop tooit toofind vilka bidrag gör tooyour svar gånger.</span><span class="sxs-lookup"><span data-stu-id="9db63-201">For example, if you build your code with an assembly that you didn't write yourself, you could time all hello calls tooit, toofind out what contribution it makes tooyour response times.</span></span> <span data-ttu-id="9db63-202">toohave dessa data visas i hello beroendediagrammen i Application Insights, skicka den via `TrackDependency`.</span><span class="sxs-lookup"><span data-stu-id="9db63-202">toohave this data displayed in hello dependency charts in Application Insights, send it using `TrackDependency`.</span></span>

```C#

            var startTime = DateTime.UtcNow;
            var timer = System.Diagnostics.Stopwatch.StartNew();
            try
            {
                success = dependency.Call();
            }
            finally
            {
                timer.Stop();
                telemetry.TrackDependency("myDependency", "myCall", startTime, timer.Elapsed, success);
            }
```

<span data-ttu-id="9db63-203">Om du vill tooswitch av hello standard beroende spårning modul, tar du bort hello referens tooDependencyTrackingTelemetryModule i [ApplicationInsights.config](app-insights-configuration-with-applicationinsights-config.md).</span><span class="sxs-lookup"><span data-stu-id="9db63-203">If you want tooswitch off hello standard dependency tracking module, remove hello reference tooDependencyTrackingTelemetryModule in [ApplicationInsights.config](app-insights-configuration-with-applicationinsights-config.md).</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="9db63-204">Felsökning</span><span class="sxs-lookup"><span data-stu-id="9db63-204">Troubleshooting</span></span>
<span data-ttu-id="9db63-205">*Beroende lyckade flagga alltid visar som SANT eller FALSKT.*</span><span class="sxs-lookup"><span data-stu-id="9db63-205">*Dependency success flag always shows either true or false.*</span></span>

<span data-ttu-id="9db63-206">*SQL-fråga som inte visas i sin helhet.*</span><span class="sxs-lookup"><span data-stu-id="9db63-206">*SQL query not shown in full.*</span></span>

* <span data-ttu-id="9db63-207">Uppgradera toohello senaste versionen av hello SDK.</span><span class="sxs-lookup"><span data-stu-id="9db63-207">Upgrade toohello latest version of hello SDK.</span></span> <span data-ttu-id="9db63-208">Om din version av .NET är mindre än 4.6:</span><span class="sxs-lookup"><span data-stu-id="9db63-208">If your .NET version is less than 4.6:</span></span>
  * <span data-ttu-id="9db63-209">IIS-värd: Installera [Application Insights Agent](app-insights-monitor-performance-live-website-now.md) på hello värdservrar.</span><span class="sxs-lookup"><span data-stu-id="9db63-209">IIS host: Install [Application Insights Agent](app-insights-monitor-performance-live-website-now.md) on hello host servers.</span></span>
  * <span data-ttu-id="9db63-210">Azure-webbapp: öppna Application Insights i Kontrollpanelen för hello web app och installera Application Insights.</span><span class="sxs-lookup"><span data-stu-id="9db63-210">Azure web app: Open Application Insights tab in hello web app control panel, and install Application Insights.</span></span>

## <a name="video"></a><span data-ttu-id="9db63-211">Video</span><span class="sxs-lookup"><span data-stu-id="9db63-211">Video</span></span>

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/112/player]

## <a name="next-steps"></a><span data-ttu-id="9db63-212">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="9db63-212">Next steps</span></span>
* [<span data-ttu-id="9db63-213">Undantag</span><span class="sxs-lookup"><span data-stu-id="9db63-213">Exceptions</span></span>](app-insights-asp-net-exceptions.md)
* [<span data-ttu-id="9db63-214">Användar- och siddata</span><span class="sxs-lookup"><span data-stu-id="9db63-214">User & page data</span></span>](app-insights-javascript.md)
* [<span data-ttu-id="9db63-215">Tillgänglighet</span><span class="sxs-lookup"><span data-stu-id="9db63-215">Availability</span></span>](app-insights-monitor-web-app-availability.md)
