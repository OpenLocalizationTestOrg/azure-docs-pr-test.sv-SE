---
title: "aaaAzure Application Insights för Windows server- och arbetsroller roller | Microsoft Docs"
description: "Manuellt lägga till hello Application Insights SDK tooyour ASP.NET tooanalyze programanvändning, tillgänglighet och prestanda."
services: application-insights
documentationcenter: .net
author: CFreemanwa
manager: carmonm
ms.assetid: 106ba99b-b57a-43b8-8866-e02f626c8190
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: get-started-article
ms.date: 05/15/2017
ms.author: bwren
ms.openlocfilehash: 64643ef637195d10f87fc6020a77169bca66c1f1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="manually-configure-application-insights-for-net-applications"></a><span data-ttu-id="8ec2f-103">Konfigurera Application Insights för .NET manuellt</span><span class="sxs-lookup"><span data-stu-id="8ec2f-103">Manually configure Application Insights for .NET applications</span></span>

<span data-ttu-id="8ec2f-104">Du kan konfigurera [Programinsikter](app-insights-overview.md) toomonitor en mängd olika program eller programroller, komponenter eller mikrotjänster.</span><span class="sxs-lookup"><span data-stu-id="8ec2f-104">You can configure [Application Insights](app-insights-overview.md) toomonitor a wide variety of applications or application roles, components, or microservices.</span></span> <span data-ttu-id="8ec2f-105">Visual Studio erbjuder [enstegskonfiguration](app-insights-asp-net.md) för webbappar och tjänster.</span><span class="sxs-lookup"><span data-stu-id="8ec2f-105">For web apps and services, Visual Studio offers [one-step configuration](app-insights-asp-net.md).</span></span> <span data-ttu-id="8ec2f-106">För andra typer av .NET-program, som till exempel roller för serverdelar eller skrivbordsprogram, kan du konfigurera Application Insights manuellt.</span><span class="sxs-lookup"><span data-stu-id="8ec2f-106">For other types of .NET application, such as backend server roles or desktop applications, you can configure Application Insights manually.</span></span>

![Exempel på prestandaövervakningsdiagram](./media/app-insights-windows-services/10-perf.png)

#### <a name="before-you-start"></a><span data-ttu-id="8ec2f-108">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="8ec2f-108">Before you start</span></span>

<span data-ttu-id="8ec2f-109">Du behöver:</span><span class="sxs-lookup"><span data-stu-id="8ec2f-109">You need:</span></span>

* <span data-ttu-id="8ec2f-110">En prenumeration för[Microsoft Azure](http://azure.com).</span><span class="sxs-lookup"><span data-stu-id="8ec2f-110">A subscription too[Microsoft Azure](http://azure.com).</span></span> <span data-ttu-id="8ec2f-111">Om din grupp eller organisation har en Azure-prenumeration, hello ägare kan lägga till dig tooit, med hjälp av din [Microsoft-konto](http://live.com).</span><span class="sxs-lookup"><span data-stu-id="8ec2f-111">If your team or organization has an Azure subscription, hello owner can add you tooit, using your [Microsoft account](http://live.com).</span></span>
* <span data-ttu-id="8ec2f-112">Visual Studio 2013 eller senare.</span><span class="sxs-lookup"><span data-stu-id="8ec2f-112">Visual Studio 2013 or later.</span></span>

## <span data-ttu-id="8ec2f-113"><a name="add"></a>1. Välj en Application Insights-resurs</span><span class="sxs-lookup"><span data-stu-id="8ec2f-113"><a name="add"></a>1. Choose an Application Insights resource</span></span>

<span data-ttu-id="8ec2f-114">hello resursen är där dina data samlas in och visas i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="8ec2f-114">hello 'resource' is where your data is collected and displayed in hello Azure portal.</span></span> <span data-ttu-id="8ec2f-115">Du behöver toodecide om toocreate en ny eller en befintlig resurs.</span><span class="sxs-lookup"><span data-stu-id="8ec2f-115">You need toodecide whether toocreate a new one, or share an existing one.</span></span>

### <a name="part-of-a-larger-app-use-existing-resource"></a><span data-ttu-id="8ec2f-116">En del av en större app: Använd en befintlig resurs</span><span class="sxs-lookup"><span data-stu-id="8ec2f-116">Part of a larger app: Use existing resource</span></span>

<span data-ttu-id="8ec2f-117">Om ditt webbprogram har flera komponenter – till exempel en frontend-webbprogram och en eller flera backend-tjänster – så att du ska skicka telemetri från alla hello komponenter toohello samma resurs.</span><span class="sxs-lookup"><span data-stu-id="8ec2f-117">If your web application has several components - for example, a front-end web app and one or more back-end services - then you should send telemetry from all hello components toohello same resource.</span></span> <span data-ttu-id="8ec2f-118">Detta kommer att de visas på en enda programavbildningen toobe och gör det möjligt tootrace en begäran från en komponent tooanother.</span><span class="sxs-lookup"><span data-stu-id="8ec2f-118">This will enable them toobe displayed on a single Application Map, and make it possible tootrace a request from one component tooanother.</span></span>

<span data-ttu-id="8ec2f-119">Så om du redan övervakar andra komponenter i den här appen, sedan bara Använd hello samma resurs.</span><span class="sxs-lookup"><span data-stu-id="8ec2f-119">So, if you're already monitoring other components of this app, then just use hello same resource.</span></span>

<span data-ttu-id="8ec2f-120">Öppna hello resurs i hello [Azure-portalen](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="8ec2f-120">Open hello resource in hello [Azure portal](https://portal.azure.com/).</span></span> 

### <a name="self-contained-app-create-a-new-resource"></a><span data-ttu-id="8ec2f-121">Fristående app: Skapa en ny resurs</span><span class="sxs-lookup"><span data-stu-id="8ec2f-121">Self-contained app: Create a new resource</span></span>

<span data-ttu-id="8ec2f-122">Om hello nya appen är inte relaterat tooother program, bör den ha en egen resurs.</span><span class="sxs-lookup"><span data-stu-id="8ec2f-122">If hello new app is unrelated tooother applications, then it should have its own resource.</span></span>

<span data-ttu-id="8ec2f-123">Logga in toohello [Azure-portalen](https://portal.azure.com/), och skapa en ny Application Insights-resurs.</span><span class="sxs-lookup"><span data-stu-id="8ec2f-123">Sign in toohello [Azure portal](https://portal.azure.com/), and create a new Application Insights resource.</span></span> <span data-ttu-id="8ec2f-124">Välj ASP.NET som hello programtyp.</span><span class="sxs-lookup"><span data-stu-id="8ec2f-124">Choose ASP.NET as hello application type.</span></span>

![Klicka på Nytt, Application Insights](./media/app-insights-windows-services/01-new-asp.png)

<span data-ttu-id="8ec2f-126">hello val av programtyp anger hello standardinnehåll hello resurs blad.</span><span class="sxs-lookup"><span data-stu-id="8ec2f-126">hello choice of application type sets hello default content of hello resource blades.</span></span>

## <a name="2-copy-hello-instrumentation-key"></a><span data-ttu-id="8ec2f-127">2. Kopiera hello Instrumentation nyckel</span><span class="sxs-lookup"><span data-stu-id="8ec2f-127">2. Copy hello Instrumentation Key</span></span>
<span data-ttu-id="8ec2f-128">hello nyckel identifierar hello resurs.</span><span class="sxs-lookup"><span data-stu-id="8ec2f-128">hello key identifies hello resource.</span></span> <span data-ttu-id="8ec2f-129">Du måste installera den snart i hello SDK, i ordning toodirect data toohello resursen.</span><span class="sxs-lookup"><span data-stu-id="8ec2f-129">You'll install it soon in hello SDK, in order toodirect data toohello resource.</span></span>

![Klicka på egenskaper, markera hello nyckeln och tryck på ctrl + C](./media/app-insights-windows-services/02-props-asp.png)

## <span data-ttu-id="8ec2f-131"><a name="sdk"></a>3. Installationspaket som hello Application Insights i ditt program</span><span class="sxs-lookup"><span data-stu-id="8ec2f-131"><a name="sdk"></a>3. Install hello Application Insights package in your application</span></span>
<span data-ttu-id="8ec2f-132">Installera och konfigurera hello Application Insights varierar paketet beroende på hello-plattform som du arbetar med.</span><span class="sxs-lookup"><span data-stu-id="8ec2f-132">Installing and configuring hello Application Insights package varies depending on hello platform you're working on.</span></span> 

1. <span data-ttu-id="8ec2f-133">I Visual Studio högerklickar du på projektet och väljer **Hantera Nuget-paket**.</span><span class="sxs-lookup"><span data-stu-id="8ec2f-133">In Visual Studio, right-click your project and choose **Manage Nuget Packages**.</span></span>
   
    ![Högerklicka på hello-projektet och välj Hantera Nuget-paket](./media/app-insights-windows-services/03-nuget.png)
2. <span data-ttu-id="8ec2f-135">Installera hello Application Insights-paketet för Windows server-appar, ”Microsoft.ApplicationInsights.WindowsServer”.</span><span class="sxs-lookup"><span data-stu-id="8ec2f-135">Install hello Application Insights package for Windows server apps, "Microsoft.ApplicationInsights.WindowsServer."</span></span>
   
    ![Sök efter ”Application Insights”](./media/app-insights-windows-services/04-ai-nuget.png)
   
    <span data-ttu-id="8ec2f-137">*Vilken version?*</span><span class="sxs-lookup"><span data-stu-id="8ec2f-137">*Which version?*</span></span>

    <span data-ttu-id="8ec2f-138">Kontrollera **inkludera förhandsversion** om du vill tootry våra senaste funktioner.</span><span class="sxs-lookup"><span data-stu-id="8ec2f-138">Check **Include prerelease** if you want tootry our latest features.</span></span> <span data-ttu-id="8ec2f-139">hello relevanta dokument eller bloggar du Observera att om du behöver en förhandsversion.</span><span class="sxs-lookup"><span data-stu-id="8ec2f-139">hello relevant documents or blogs note whether you need a prerelease version.</span></span>
    
    <span data-ttu-id="8ec2f-140">*Kan jag använda andra paket?*</span><span class="sxs-lookup"><span data-stu-id="8ec2f-140">*Can I use other packages?*</span></span>
   
    <span data-ttu-id="8ec2f-141">Ja.</span><span class="sxs-lookup"><span data-stu-id="8ec2f-141">Yes.</span></span> <span data-ttu-id="8ec2f-142">Välj ”Microsoft.ApplicationInsights” om du bara vill toouse hello API toosend egna telemetri.</span><span class="sxs-lookup"><span data-stu-id="8ec2f-142">Choose "Microsoft.ApplicationInsights" if you only want toouse hello API toosend your own telemetry.</span></span> <span data-ttu-id="8ec2f-143">hello Windows Server-paketet innehåller hello API plus ett antal andra paket, till exempel beroende övervakning och insamling av räknare för prestanda.</span><span class="sxs-lookup"><span data-stu-id="8ec2f-143">hello Windows Server package includes hello API plus a number of other packages such as performance counter collection and dependency monitoring.</span></span> 

### <a name="tooupgrade-toofuture-package-versions"></a><span data-ttu-id="8ec2f-144">tooupgrade toofuture paketversionerna</span><span class="sxs-lookup"><span data-stu-id="8ec2f-144">tooupgrade toofuture package versions</span></span>
<span data-ttu-id="8ec2f-145">Vi använda en ny version av hello SDK från tid tootime.</span><span class="sxs-lookup"><span data-stu-id="8ec2f-145">We release a new version of hello SDK from time tootime.</span></span>

<span data-ttu-id="8ec2f-146">tooupgrade tooa [ny version av paketet hello](https://github.com/Microsoft/ApplicationInsights-dotnet-server/releases/), öppna NuGet-Pakethanteraren igen och filtrera efter installerade paket.</span><span class="sxs-lookup"><span data-stu-id="8ec2f-146">tooupgrade tooa [new release of hello package](https://github.com/Microsoft/ApplicationInsights-dotnet-server/releases/), open NuGet package manager again and filter on installed packages.</span></span> <span data-ttu-id="8ec2f-147">Markera **Microsoft.ApplicationInsights.WindowsServer** och välj **Uppgradera**.</span><span class="sxs-lookup"><span data-stu-id="8ec2f-147">Select **Microsoft.ApplicationInsights.WindowsServer** and choose **Upgrade**.</span></span>

<span data-ttu-id="8ec2f-148">Om du har gjort alla anpassningar tooApplicationInsights.config spara en kopia av den innan du uppgraderar och därefter sammanfoga ändringarna till hello ny version.</span><span class="sxs-lookup"><span data-stu-id="8ec2f-148">If you made any customizations tooApplicationInsights.config, save a copy of it before you upgrade, and afterwards merge your changes into hello new version.</span></span>

## <a name="4-send-telemetry"></a><span data-ttu-id="8ec2f-149">4. Skicka telemetri</span><span class="sxs-lookup"><span data-stu-id="8ec2f-149">4. Send telemetry</span></span>
<span data-ttu-id="8ec2f-150">**Om du har installerat hello API-paketet:**</span><span class="sxs-lookup"><span data-stu-id="8ec2f-150">**If you installed only hello API package:**</span></span>

* <span data-ttu-id="8ec2f-151">Ange hello instrumentation nyckeln i koden, till exempel i `main()`:</span><span class="sxs-lookup"><span data-stu-id="8ec2f-151">Set hello instrumentation key in code, for example in `main()`:</span></span> 
  
    <span data-ttu-id="8ec2f-152">`TelemetryConfiguration.Active.InstrumentationKey = "` *din nyckel* `";`</span><span class="sxs-lookup"><span data-stu-id="8ec2f-152">`TelemetryConfiguration.Active.InstrumentationKey = "` *your key* `";`</span></span> 
* <span data-ttu-id="8ec2f-153">[Skriva egna telemetri med hello API](app-insights-api-custom-events-metrics.md#ikey).</span><span class="sxs-lookup"><span data-stu-id="8ec2f-153">[Write your own telemetry using hello API](app-insights-api-custom-events-metrics.md#ikey).</span></span>

<span data-ttu-id="8ec2f-154">**Om du har installerat andra Application Insights-paket** du kan om du föredrar att använda hello .config-fil tooset hello instrumentation nyckel:</span><span class="sxs-lookup"><span data-stu-id="8ec2f-154">**If you installed other Application Insights packages,** you can, if you prefer, use hello .config file tooset hello instrumentation key:</span></span>

* <span data-ttu-id="8ec2f-155">Redigera ApplicationInsights.config (som har lagts till av hello installera NuGet).</span><span class="sxs-lookup"><span data-stu-id="8ec2f-155">Edit ApplicationInsights.config (which was added by hello NuGet install).</span></span> <span data-ttu-id="8ec2f-156">Infoga det precis innan hello avslutande tagg:</span><span class="sxs-lookup"><span data-stu-id="8ec2f-156">Insert this just before hello closing tag:</span></span>
  
    <span data-ttu-id="8ec2f-157">`<InstrumentationKey>`*hello instrumentation nyckel som du kopierade*`</InstrumentationKey>`</span><span class="sxs-lookup"><span data-stu-id="8ec2f-157">`<InstrumentationKey>` *hello instrumentation key you copied* `</InstrumentationKey>`</span></span>
* <span data-ttu-id="8ec2f-158">Se till att hello egenskaper för ApplicationInsights.config i Solution Explorer är inställda för**Skapa åtgärd = innehåll, kopiera tooOutput Directory = kopiera**.</span><span class="sxs-lookup"><span data-stu-id="8ec2f-158">Make sure that hello properties of ApplicationInsights.config in Solution Explorer are set too**Build Action = Content, Copy tooOutput Directory = Copy**.</span></span>

<span data-ttu-id="8ec2f-159">Det är användbart tooset hello instrumentation nyckel i kod om du vill använda för[växel hello nyckel för olika build konfigurationer](app-insights-separate-resources.md).</span><span class="sxs-lookup"><span data-stu-id="8ec2f-159">It's useful tooset hello instrumentation key in code if you want too[switch hello key for different build configurations](app-insights-separate-resources.md).</span></span> <span data-ttu-id="8ec2f-160">Om du anger hello nyckel i koden kan du inte har tooset den i hello `.config` fil.</span><span class="sxs-lookup"><span data-stu-id="8ec2f-160">If you set hello key in code, you don't have tooset it in hello `.config` file.</span></span>

## <span data-ttu-id="8ec2f-161"><a name="run"></a> Köra projektet</span><span class="sxs-lookup"><span data-stu-id="8ec2f-161"><a name="run"></a> Run your project</span></span>
<span data-ttu-id="8ec2f-162">Använd hello **F5** toorun programmet och prova: öppna olika sidor toogenerate vissa telemetri.</span><span class="sxs-lookup"><span data-stu-id="8ec2f-162">Use hello **F5** toorun your application and try it out: open different pages toogenerate some telemetry.</span></span>

<span data-ttu-id="8ec2f-163">I Visual Studio visas antalet hello-händelser som har skickats.</span><span class="sxs-lookup"><span data-stu-id="8ec2f-163">In Visual Studio, you'll see a count of hello events that have been sent.</span></span>

![Antal händelser i Visual Studio](./media/app-insights-windows-services/appinsights-09eventcount.png)

## <span data-ttu-id="8ec2f-165"><a name="monitor"></a> Visa telemetrin</span><span class="sxs-lookup"><span data-stu-id="8ec2f-165"><a name="monitor"></a> View your telemetry</span></span>
<span data-ttu-id="8ec2f-166">Returnera toohello [Azure-portalen](https://portal.azure.com/) och bläddra tooyour Application Insights-resurs.</span><span class="sxs-lookup"><span data-stu-id="8ec2f-166">Return toohello [Azure portal](https://portal.azure.com/) and browse tooyour Application Insights resource.</span></span>

<span data-ttu-id="8ec2f-167">Leta efter data i hello översikt över diagram.</span><span class="sxs-lookup"><span data-stu-id="8ec2f-167">Look for data in hello Overview charts.</span></span> <span data-ttu-id="8ec2f-168">Först ser du bara en eller två punkter.</span><span class="sxs-lookup"><span data-stu-id="8ec2f-168">At first, you'll just see one or two points.</span></span> <span data-ttu-id="8ec2f-169">Exempel:</span><span class="sxs-lookup"><span data-stu-id="8ec2f-169">For example:</span></span>

![Klicka dig igenom toomore data](./media/app-insights-windows-services/12-first-perf.png)

<span data-ttu-id="8ec2f-171">Klicka på via alla diagram toosee mer detaljerad mått.</span><span class="sxs-lookup"><span data-stu-id="8ec2f-171">Click through any chart toosee more detailed metrics.</span></span> [<span data-ttu-id="8ec2f-172">Lär dig mer om mätvärden.</span><span class="sxs-lookup"><span data-stu-id="8ec2f-172">Learn more about metrics.</span></span>](app-insights-web-monitor-performance.md)

### <a name="no-data"></a><span data-ttu-id="8ec2f-173">Ser du inga data?</span><span class="sxs-lookup"><span data-stu-id="8ec2f-173">No data?</span></span>
* <span data-ttu-id="8ec2f-174">Använda programmet hello, öppna olika sidor så att den genererar vissa telemetri.</span><span class="sxs-lookup"><span data-stu-id="8ec2f-174">Use hello application, opening different pages so that it generates some telemetry.</span></span>
* <span data-ttu-id="8ec2f-175">Öppna hello [Sök](app-insights-diagnostic-search.md) panelen toosee enskilda händelser.</span><span class="sxs-lookup"><span data-stu-id="8ec2f-175">Open hello [Search](app-insights-diagnostic-search.md) tile, toosee individual events.</span></span> <span data-ttu-id="8ec2f-176">Ibland tar händelser lite medan längre tooget via hello mått pipeline.</span><span class="sxs-lookup"><span data-stu-id="8ec2f-176">Sometimes it takes events a little while longer tooget through hello metrics pipeline.</span></span>
* <span data-ttu-id="8ec2f-177">Vänta några sekunder och klicka på **Uppdatera**.</span><span class="sxs-lookup"><span data-stu-id="8ec2f-177">Wait a few seconds and click **Refresh**.</span></span> <span data-ttu-id="8ec2f-178">Diagram uppdatera själva regelbundet, men du kan uppdatera manuellt om du väntar vissa data tooshow.</span><span class="sxs-lookup"><span data-stu-id="8ec2f-178">Charts refresh themselves periodically, but you can refresh manually if you're waiting for some data tooshow up.</span></span>
* <span data-ttu-id="8ec2f-179">Mer information finns i [Felsökning](app-insights-troubleshoot-faq.md).</span><span class="sxs-lookup"><span data-stu-id="8ec2f-179">See [Troubleshooting](app-insights-troubleshoot-faq.md).</span></span>

## <a name="publish-your-app"></a><span data-ttu-id="8ec2f-180">Publicera appen</span><span class="sxs-lookup"><span data-stu-id="8ec2f-180">Publish your app</span></span>
<span data-ttu-id="8ec2f-181">Nu distribuera programservern tooyour eller tooAzure och titta på hello data ackumuleras.</span><span class="sxs-lookup"><span data-stu-id="8ec2f-181">Now deploy your application tooyour server or tooAzure and watch hello data accumulate.</span></span>

![Använd Visual Studio toopublish din app](./media/app-insights-windows-services/15-publish.png)

<span data-ttu-id="8ec2f-183">När du kör i felsökningsläge expedierade telemetri via hello pipelinen, så att du bör se data som visas inom några sekunder.</span><span class="sxs-lookup"><span data-stu-id="8ec2f-183">When you run in debug mode, telemetry is expedited through hello pipeline, so that you should see data appearing within seconds.</span></span> <span data-ttu-id="8ec2f-184">När du distribuerar appen i en publiceringskonfiguration ackumuleras data långsammare.</span><span class="sxs-lookup"><span data-stu-id="8ec2f-184">When you deploy your app in Release configuration, data accumulates more slowly.</span></span>

### <a name="no-data-after-you-publish-tooyour-server"></a><span data-ttu-id="8ec2f-185">Inga data när du har publicerat tooyour server?</span><span class="sxs-lookup"><span data-stu-id="8ec2f-185">No data after you publish tooyour server?</span></span>
<span data-ttu-id="8ec2f-186">Öppna portar för utgående trafik i serverns brandvägg.</span><span class="sxs-lookup"><span data-stu-id="8ec2f-186">Open ports for outgoing traffic in your server's firewall.</span></span> <span data-ttu-id="8ec2f-187">Se [den här sidan](https://docs.microsoft.com/azure/application-insights/app-insights-ip-addresses) hello lista över begärda adresser</span><span class="sxs-lookup"><span data-stu-id="8ec2f-187">See [this page](https://docs.microsoft.com/azure/application-insights/app-insights-ip-addresses) for hello list of required addresses</span></span> 

### <a name="trouble-on-your-build-server"></a><span data-ttu-id="8ec2f-188">Har du problem på build-servern?</span><span class="sxs-lookup"><span data-stu-id="8ec2f-188">Trouble on your build server?</span></span>
<span data-ttu-id="8ec2f-189">Mer information finns i [det här felsökningsavsnittet](app-insights-asp-net-troubleshoot-no-data.md#NuGetBuild).</span><span class="sxs-lookup"><span data-stu-id="8ec2f-189">Please see [this Troubleshooting item](app-insights-asp-net-troubleshoot-no-data.md#NuGetBuild).</span></span>

> [!NOTE]
> <span data-ttu-id="8ec2f-190">Om din app genererar mycket telemetri, minska hello anpassningsbar provtagning modulen automatiskt hello volymen som skickas toohello portal genom att skicka en representativ del av händelser.</span><span class="sxs-lookup"><span data-stu-id="8ec2f-190">If your app generates a lot of telemetry, hello adaptive sampling module will automatically reduce hello volume that is sent toohello portal by sending only a representative fraction of events.</span></span> <span data-ttu-id="8ec2f-191">Dock händelser som är relaterade toohello samma begäran ska markeras eller avmarkeras som en grupp, så att du kan navigera mellan relaterade händelser.</span><span class="sxs-lookup"><span data-stu-id="8ec2f-191">However, events that are related toohello same request will be selected or deselected as a group, so that you can navigate between related events.</span></span> 
> <span data-ttu-id="8ec2f-192">[Lär dig mer om insamling](app-insights-sampling.md).</span><span class="sxs-lookup"><span data-stu-id="8ec2f-192">[Learn about sampling](app-insights-sampling.md).</span></span>
> 
> 

## <a name="video"></a><span data-ttu-id="8ec2f-193">Video</span><span class="sxs-lookup"><span data-stu-id="8ec2f-193">Video</span></span>

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/100/player]

## <a name="next-steps"></a><span data-ttu-id="8ec2f-194">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="8ec2f-194">Next steps</span></span>
* <span data-ttu-id="8ec2f-195">[Lägg till mer telemetri](app-insights-asp-net-more.md) tooget hello 360 graders vy av ditt program.</span><span class="sxs-lookup"><span data-stu-id="8ec2f-195">[Add more telemetry](app-insights-asp-net-more.md) tooget hello full 360-degree view of your application.</span></span>

