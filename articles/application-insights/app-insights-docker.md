---
title: aaaMonitor Docker-program i Azure Application Insights | Microsoft Docs
description: "Docker prestandaräknarna, händelser och undantag kan visas i Application Insights, tillsammans med hello telemetri från hello av appar."
services: application-insights
documentationcenter: 
author: CFreemanwa
manager: carmonm
ms.assetid: 27a3083d-d67f-4a07-8f3c-4edb65a0a685
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 03/14/2017
ms.author: bwren
ms.openlocfilehash: 9aaf1076bae25485a396db1bb3dcd13bccd87c19
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-docker-applications-in-application-insights"></a><span data-ttu-id="8de23-103">Övervaka Docker-program i Application Insights</span><span class="sxs-lookup"><span data-stu-id="8de23-103">Monitor Docker applications in Application Insights</span></span>
<span data-ttu-id="8de23-104">Livscykelhändelser och prestanda prestandaräknare från [Docker](https://www.docker.com/) kan ritas upp behållare på Application Insights.</span><span class="sxs-lookup"><span data-stu-id="8de23-104">Lifecycle events and performance counters from [Docker](https://www.docker.com/) containers can be charted on Application Insights.</span></span> <span data-ttu-id="8de23-105">Installera hello [Programinsikter](app-insights-overview.md) bilden i en behållare i värden och den visar prestandaräknare för hello värden som hello bra som för andra bilder.</span><span class="sxs-lookup"><span data-stu-id="8de23-105">Install hello [Application Insights](app-insights-overview.md) image in a container in your host, and it will display performance counters for hello host, as well as for hello other images.</span></span>

<span data-ttu-id="8de23-106">Med Docker, kan du distribuera dina appar i lightweight-behållare med alla beroenden.</span><span class="sxs-lookup"><span data-stu-id="8de23-106">With Docker, you distribute your apps in lightweight containers complete with all dependencies.</span></span> <span data-ttu-id="8de23-107">De kan köras på en värddator som kör en Docker-motorn.</span><span class="sxs-lookup"><span data-stu-id="8de23-107">They'll run on any host machine that runs a Docker Engine.</span></span>

<span data-ttu-id="8de23-108">När du kör hello [Application Insights bild](https://hub.docker.com/r/microsoft/applicationinsights/) på Docker-värden som du får följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="8de23-108">When you run hello [Application Insights image](https://hub.docker.com/r/microsoft/applicationinsights/) on your Docker host, you get these benefits:</span></span>

* <span data-ttu-id="8de23-109">Livscykel telemetri om alla hello-behållare som körs på värd hello - starta, stoppa och så vidare.</span><span class="sxs-lookup"><span data-stu-id="8de23-109">Lifecycle telemetry about all hello containers running on hello host - start, stop, and so on.</span></span>
* <span data-ttu-id="8de23-110">Prestandaräknare för alla hello-behållare.</span><span class="sxs-lookup"><span data-stu-id="8de23-110">Performance counters for all hello containers.</span></span> <span data-ttu-id="8de23-111">CPU, minne, nätverksanvändning och mer.</span><span class="sxs-lookup"><span data-stu-id="8de23-111">CPU, memory, network usage, and more.</span></span>
* <span data-ttu-id="8de23-112">Om du [installerat Application Insights SDK för Java](app-insights-java-live.md) i hello-appar som körs i hello behållare, alla hello telemetri för dessa appar har ytterligare egenskaper som identifierar hello-behållaren och värddatorn.</span><span class="sxs-lookup"><span data-stu-id="8de23-112">If you [installed Application Insights SDK for Java](app-insights-java-live.md) in hello apps running in hello containers, all hello telemetry of those apps will have additional properties identifying hello container and host machine.</span></span> <span data-ttu-id="8de23-113">Om du har instanser av en app som körs i mer än en värddator kan du exempelvis enkelt filtrera din app telemetri av värden.</span><span class="sxs-lookup"><span data-stu-id="8de23-113">So for example, if you have instances of an app running in more than one host, you can easily filter your app telemetry by host.</span></span>

![Exempel](./media/app-insights-docker/00.png)

## <a name="set-up-your-application-insights-resource"></a><span data-ttu-id="8de23-115">Konfigurera Application Insights-resurs</span><span class="sxs-lookup"><span data-stu-id="8de23-115">Set up your Application Insights resource</span></span>
1. <span data-ttu-id="8de23-116">Logga in på [Microsoft Azure-portalen](https://azure.com) och öppna hello Application Insights-resurs för din app, eller [skapa en ny](app-insights-create-new-resource.md).</span><span class="sxs-lookup"><span data-stu-id="8de23-116">Sign into [Microsoft Azure portal](https://azure.com) and open hello Application Insights resource for your app; or [create a new one](app-insights-create-new-resource.md).</span></span> 
   
    <span data-ttu-id="8de23-117">*Vilken resurs som ska jag använda?*</span><span class="sxs-lookup"><span data-stu-id="8de23-117">*Which resource should I use?*</span></span> <span data-ttu-id="8de23-118">Om hello-appar som körs på värden har utvecklats av någon annan, måste du för[skapar en ny Application Insights-resurs](app-insights-create-new-resource.md).</span><span class="sxs-lookup"><span data-stu-id="8de23-118">If hello apps that you are running on your host were developed by someone else, then you need too[create a new Application Insights resource](app-insights-create-new-resource.md).</span></span> <span data-ttu-id="8de23-119">Detta är där du kan visa och analysera hello telemetri.</span><span class="sxs-lookup"><span data-stu-id="8de23-119">This is where you view and analyze hello telemetry.</span></span> <span data-ttu-id="8de23-120">(Välj Allmänt för typ av hello app).</span><span class="sxs-lookup"><span data-stu-id="8de23-120">(Select 'General' for hello app type.)</span></span>
   
    <span data-ttu-id="8de23-121">Men om du utvecklar hello hello appar sedan vi hoppas att du [lagt till Application Insights SDK](app-insights-java-live.md) tooeach av dem.</span><span class="sxs-lookup"><span data-stu-id="8de23-121">But if you're hello developer of hello apps, then we hope you [added Application Insights SDK](app-insights-java-live.md) tooeach of them.</span></span> <span data-ttu-id="8de23-122">Du kan konfigurera alla toosend telemetri tooone resurs och ska du använda samma resurs toodisplay hello Docker livscykel och prestanda information om de är alla verkligen komponenter i ett enda affärsprogram.</span><span class="sxs-lookup"><span data-stu-id="8de23-122">If they're all really components of a single business application, then you might configure all of them toosend telemetry tooone resource, and you'll use that same resource toodisplay hello Docker lifecycle and performance data.</span></span> 
   
    <span data-ttu-id="8de23-123">Ett tredje scenario är att du har utvecklat de flesta av hello appar, men du använder separata resurser toodisplay sina telemetri.</span><span class="sxs-lookup"><span data-stu-id="8de23-123">A third scenario is that you developed most of hello apps, but you are using separate resources toodisplay their telemetry.</span></span> <span data-ttu-id="8de23-124">I så fall måste du förmodligen också vill toocreate en separat resurs för hello Docker-data.</span><span class="sxs-lookup"><span data-stu-id="8de23-124">In that case, you probably also want toocreate a separate resource for hello Docker data.</span></span> 
2. <span data-ttu-id="8de23-125">Lägg till hello Docker panelen: Välj **lägga till panelen**dra hello Docker sida vid sida från hello-galleriet och klicka sedan på **klar**.</span><span class="sxs-lookup"><span data-stu-id="8de23-125">Add hello Docker tile: Choose **Add Tile**, drag hello Docker tile from hello gallery, and then click **Done**.</span></span> 
   
    ![Exempel](./media/app-insights-docker/03.png)
3. <span data-ttu-id="8de23-127">Klicka på hello **Essentials** listrutan och kopiera hello Instrumentation nyckel.</span><span class="sxs-lookup"><span data-stu-id="8de23-127">Click hello **Essentials** drop-down and copy hello Instrumentation Key.</span></span> <span data-ttu-id="8de23-128">Du använder den här tootell hello SDK där toosend dess telemetri.</span><span class="sxs-lookup"><span data-stu-id="8de23-128">You use this tootell hello SDK where toosend its telemetry.</span></span>

    ![Exempel](./media/app-insights-docker/02-props.png)

<span data-ttu-id="8de23-130">Hålla webbläsarfönstret praktiska, som du återkommer tooit snart toolook din telemetri.</span><span class="sxs-lookup"><span data-stu-id="8de23-130">Keep that browser window handy, as you'll come back tooit soon toolook at your telemetry.</span></span>

## <a name="run-hello-application-insights-monitor-on-your-host"></a><span data-ttu-id="8de23-131">Kör hello Application Insights monitor på din värd</span><span class="sxs-lookup"><span data-stu-id="8de23-131">Run hello Application Insights monitor on your host</span></span>
<span data-ttu-id="8de23-132">Nu när du har någonstans toodisplay hello telemetri kan ställa du in hello av app som samlar in och skicka den.</span><span class="sxs-lookup"><span data-stu-id="8de23-132">Now that you've got somewhere toodisplay hello telemetry, you can set up hello containerized app that will collect and send it.</span></span>

1. <span data-ttu-id="8de23-133">Anslut tooyour Docker-värden.</span><span class="sxs-lookup"><span data-stu-id="8de23-133">Connect tooyour Docker host.</span></span> 
2. <span data-ttu-id="8de23-134">Redigera din instrumentation nyckeln i det här kommandot och kör det:</span><span class="sxs-lookup"><span data-stu-id="8de23-134">Edit your instrumentation key into this command, and then run it:</span></span>
   
   ```
   
   docker run -v /var/run/docker.sock:/docker.sock -d microsoft/applicationinsights ikey=000000-1111-2222-3333-444444444
   ```

<span data-ttu-id="8de23-135">Endast en Application Insights-avbildning krävs per Docker-värd.</span><span class="sxs-lookup"><span data-stu-id="8de23-135">Only one Application Insights image is required per Docker host.</span></span> <span data-ttu-id="8de23-136">Om programmet distribueras på flera Docker-värdar, upprepa sedan hello-kommando på varje värd.</span><span class="sxs-lookup"><span data-stu-id="8de23-136">If your application is deployed on multiple Docker hosts, then repeat hello command on every host.</span></span>

## <a name="update-your-app"></a><span data-ttu-id="8de23-137">Uppdatera ditt program</span><span class="sxs-lookup"><span data-stu-id="8de23-137">Update your app</span></span>
<span data-ttu-id="8de23-138">Om ditt program är försett med hello [Application Insights SDK för Java](app-insights-java-get-started.md), Lägg till följande rad i hello ApplicationInsights.xml-filen i projektet, under hello hello `<TelemetryInitializers>` element:</span><span class="sxs-lookup"><span data-stu-id="8de23-138">If your application is instrumented with hello [Application Insights SDK for Java](app-insights-java-get-started.md), add hello following line into hello ApplicationInsights.xml file in your project, under hello `<TelemetryInitializers>` element:</span></span>

```xml

    <Add type="com.microsoft.applicationinsights.extensibility.initializer.docker.DockerContextInitializer"/> 
```

<span data-ttu-id="8de23-139">Detta lägger till Docker information, till exempel behållare och värd-id tooevery telemetri objekt som skickats från din app.</span><span class="sxs-lookup"><span data-stu-id="8de23-139">This adds Docker information such as container and host id tooevery telemetry item sent from your app.</span></span>

## <a name="view-your-telemetry"></a><span data-ttu-id="8de23-140">Visa telemetrin</span><span class="sxs-lookup"><span data-stu-id="8de23-140">View your telemetry</span></span>
<span data-ttu-id="8de23-141">Gå tillbaka tooyour Application Insights-resurs i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="8de23-141">Go back tooyour Application Insights resource in hello Azure portal.</span></span>

<span data-ttu-id="8de23-142">Klicka dig igenom hello Docker-panelen.</span><span class="sxs-lookup"><span data-stu-id="8de23-142">Click through hello Docker tile.</span></span>

<span data-ttu-id="8de23-143">Kort visas data som kommer från hello Docker-app, särskilt om du har andra behållare som körs på Docker-motorn.</span><span class="sxs-lookup"><span data-stu-id="8de23-143">You'll shortly see data arriving from hello Docker app, especially if you have other containers running on your Docker engine.</span></span>

<span data-ttu-id="8de23-144">Här är några av hello vyer som du kan hämta.</span><span class="sxs-lookup"><span data-stu-id="8de23-144">Here are some of hello views you can get.</span></span>

### <a name="perf-counters-by-host-activity-by-image"></a><span data-ttu-id="8de23-145">Prestandaräknarna av värden, aktivitet efter bilden</span><span class="sxs-lookup"><span data-stu-id="8de23-145">Perf counters by host, activity by image</span></span>
![Exempel](./media/app-insights-docker/10.png)

![Exempel](./media/app-insights-docker/11.png)

<span data-ttu-id="8de23-148">Klicka på värden eller image-namn för mer information.</span><span class="sxs-lookup"><span data-stu-id="8de23-148">Click any host or image name for more detail.</span></span>

<span data-ttu-id="8de23-149">toocustomize hello vyn klickar du på alla diagram, hello rutnätet rubrik eller Använd Lägg till diagram.</span><span class="sxs-lookup"><span data-stu-id="8de23-149">toocustomize hello view, click any chart, hello grid heading, or use Add Chart.</span></span> 

<span data-ttu-id="8de23-150">[Mer information om metrics explorer](app-insights-metrics-explorer.md).</span><span class="sxs-lookup"><span data-stu-id="8de23-150">[Learn more about metrics explorer](app-insights-metrics-explorer.md).</span></span>

### <a name="docker-container-events"></a><span data-ttu-id="8de23-151">Docker behållaren händelser</span><span class="sxs-lookup"><span data-stu-id="8de23-151">Docker container events</span></span>
![Exempel](./media/app-insights-docker/13.png)

<span data-ttu-id="8de23-153">tooinvestigate enskilda händelser, klickar du på [Sök](app-insights-diagnostic-search.md).</span><span class="sxs-lookup"><span data-stu-id="8de23-153">tooinvestigate individual events, click [Search](app-insights-diagnostic-search.md).</span></span> <span data-ttu-id="8de23-154">Sök och filtrera toofind hello händelser som du vill.</span><span class="sxs-lookup"><span data-stu-id="8de23-154">Search and filter toofind hello events you want.</span></span> <span data-ttu-id="8de23-155">Klicka på en händelse tooget detalj.</span><span class="sxs-lookup"><span data-stu-id="8de23-155">Click any event tooget more detail.</span></span>

### <a name="exceptions-by-container-name"></a><span data-ttu-id="8de23-156">Undantag av behållarens namn</span><span class="sxs-lookup"><span data-stu-id="8de23-156">Exceptions by container name</span></span>
![Exempel](./media/app-insights-docker/14.png)

### <a name="docker-context-added-tooapp-telemetry"></a><span data-ttu-id="8de23-158">Docker-kontexten läggs tooapp telemetri</span><span class="sxs-lookup"><span data-stu-id="8de23-158">Docker context added tooapp telemetry</span></span>
<span data-ttu-id="8de23-159">Telemetri som skickas från hello programmet försett med AI SDK, utökat med Docker kontext för begäran:</span><span class="sxs-lookup"><span data-stu-id="8de23-159">Request telemetry sent from hello application instrumented with AI SDK, enriched with Docker context:</span></span>

![Exempel](./media/app-insights-docker/16.png)

<span data-ttu-id="8de23-161">Processortid och tillgängligt minnesprestandaräknare utökat grupperade efter Docker behållarnamn:</span><span class="sxs-lookup"><span data-stu-id="8de23-161">Processor time and available memory performance counters, enriched and grouped by Docker container name:</span></span>

![Exempel](./media/app-insights-docker/15.png)

## <a name="q--a"></a><span data-ttu-id="8de23-163">Frågor och svar</span><span class="sxs-lookup"><span data-stu-id="8de23-163">Q & A</span></span>
<span data-ttu-id="8de23-164">*Vad Application Insights mig som jag inte kan hämta från Docker?*</span><span class="sxs-lookup"><span data-stu-id="8de23-164">*What does Application Insights give me that I can't get from Docker?*</span></span>

* <span data-ttu-id="8de23-165">Specificering av prestandaräknare av behållaren och image.</span><span class="sxs-lookup"><span data-stu-id="8de23-165">Detailed breakdown of performance counters by container and image.</span></span>
* <span data-ttu-id="8de23-166">Integrera behållare och app-data i en instrumentpanel.</span><span class="sxs-lookup"><span data-stu-id="8de23-166">Integrate container and app data in one dashboard.</span></span>
* <span data-ttu-id="8de23-167">[Exportera telemetri](app-insights-export-telemetry.md) för ytterligare analys tooa databasen, Power BI eller andra instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="8de23-167">[Export telemetry](app-insights-export-telemetry.md) for further analysis tooa database, Power BI or other dashboard.</span></span>

<span data-ttu-id="8de23-168">*Hur får jag telemetri från själva hello appen?*</span><span class="sxs-lookup"><span data-stu-id="8de23-168">*How do I get telemetry from hello app itself?*</span></span>

* <span data-ttu-id="8de23-169">Installera hello Application Insights SDK i hello app.</span><span class="sxs-lookup"><span data-stu-id="8de23-169">Install hello Application Insights SDK in hello app.</span></span> <span data-ttu-id="8de23-170">Lär dig hur för: [Java-webbappar](app-insights-java-get-started.md), [Windows web apps](app-insights-asp-net.md).</span><span class="sxs-lookup"><span data-stu-id="8de23-170">Learn how for: [Java web apps](app-insights-java-get-started.md), [Windows web apps](app-insights-asp-net.md).</span></span>

## <a name="video"></a><span data-ttu-id="8de23-171">Video</span><span class="sxs-lookup"><span data-stu-id="8de23-171">Video</span></span>

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/100/player]

## <a name="next-steps"></a><span data-ttu-id="8de23-172">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="8de23-172">Next steps</span></span>

* [<span data-ttu-id="8de23-173">Application Insights för Java</span><span class="sxs-lookup"><span data-stu-id="8de23-173">Application Insights for Java</span></span>](app-insights-java-get-started.md)
* [<span data-ttu-id="8de23-174">Application Insights för Node.js</span><span class="sxs-lookup"><span data-stu-id="8de23-174">Application Insights for Node.js</span></span>](app-insights-nodejs.md)
* [<span data-ttu-id="8de23-175">Application Insights för ASP.NET</span><span class="sxs-lookup"><span data-stu-id="8de23-175">Application Insights for ASP.NET</span></span>](app-insights-asp-net.md)
