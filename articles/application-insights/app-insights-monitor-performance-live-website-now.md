---
title: aaaMonitor live ASP.NET web app med Azure Application Insights | Microsoft Docs
description: "Övervaka prestanda för en webbplats utan att distribuera den igen. Fungerar med ASP.NET-webbappar som finns lokalt, i virtuella datorer eller på Azure."
services: application-insights
documentationcenter: .net
author: CFreemanwa
manager: carmonm
ms.assetid: 769a5ea4-a8c6-4c18-b46c-657e864e24de
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: get-started-article
ms.date: 05/05/2017
ms.author: bwren
ms.openlocfilehash: 0d53f0a59974f40767fae681bafc4f358d1283a8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="instrument-web-apps-at-runtime-with-application-insights"></a><span data-ttu-id="3a28b-104">Instrumentera webbappar vid körning med Application Insights</span><span class="sxs-lookup"><span data-stu-id="3a28b-104">Instrument web apps at runtime with Application Insights</span></span>


<span data-ttu-id="3a28b-105">Du kan instrumentera en live-webbapp med Azure Application Insights utan toomodify eller distribuera din kod.</span><span class="sxs-lookup"><span data-stu-id="3a28b-105">You can instrument a live web app with Azure Application Insights, without having toomodify or redeploy your code.</span></span> <span data-ttu-id="3a28b-106">Om dina appar hanteras av en lokal IIS-server installerar du	Statusövervakare.</span><span class="sxs-lookup"><span data-stu-id="3a28b-106">If your apps are hosted by an on-premises IIS server, install Status Monitor.</span></span> <span data-ttu-id="3a28b-107">Om de Azure-webbappar eller köras i en Azure VM, kan du växla på Application Insights-övervakning från hello Azure på Kontrollpanelen.</span><span class="sxs-lookup"><span data-stu-id="3a28b-107">If they're Azure web apps or run in an Azure VM, you can switch on Application Insights monitoring from hello Azure control panel.</span></span> <span data-ttu-id="3a28b-108">(Det finns även olika artiklar om hur du instrumenterar [J2EE-livewebbappar](app-insights-java-live.md) och [Azure Cloud Services](app-insights-cloudservices.md).) Du behöver en [Microsoft Azure](http://azure.com)-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="3a28b-108">(There are also separate articles about instrumenting [live J2EE web apps](app-insights-java-live.md) and [Azure Cloud Services](app-insights-cloudservices.md).) You need a [Microsoft Azure](http://azure.com) subscription.</span></span>

![exempeldiagram](./media/app-insights-monitor-performance-live-website-now/10-intro.png)

<span data-ttu-id="3a28b-110">Du kan välja mellan tre vägar tooapply Application Insights tooyour .NET-webbprogram:</span><span class="sxs-lookup"><span data-stu-id="3a28b-110">You have a choice of three routes tooapply Application Insights tooyour .NET web applications:</span></span>

* <span data-ttu-id="3a28b-111">**Byggtid:** [hello Lägg till Application Insights SDK] [ greenbrown] tooyour web app-kod.</span><span class="sxs-lookup"><span data-stu-id="3a28b-111">**Build time:** [Add hello Application Insights SDK][greenbrown] tooyour web app code.</span></span>
* <span data-ttu-id="3a28b-112">**Körtid:** Instrumentera ditt webbprogram på hello-servern enligt beskrivningen nedan, utan att återskapa och omdistribuera hello kod.</span><span class="sxs-lookup"><span data-stu-id="3a28b-112">**Run time:** Instrument your web app on hello server, as described below, without rebuilding and redeploying hello code.</span></span>
* <span data-ttu-id="3a28b-113">**Både:** skapa hello SDK i koden web app och gäller även hello körning tillägg.</span><span class="sxs-lookup"><span data-stu-id="3a28b-113">**Both:** Build hello SDK into your web app code, and also apply hello run-time extensions.</span></span> <span data-ttu-id="3a28b-114">Hämta hello bästa av båda alternativen.</span><span class="sxs-lookup"><span data-stu-id="3a28b-114">Get hello best of both options.</span></span>

<span data-ttu-id="3a28b-115">Här är en sammanfattning av vad du får med respektive väg:</span><span class="sxs-lookup"><span data-stu-id="3a28b-115">Here's a summary of what you get by each route:</span></span>

|  | <span data-ttu-id="3a28b-116">Byggtid</span><span class="sxs-lookup"><span data-stu-id="3a28b-116">Build time</span></span> | <span data-ttu-id="3a28b-117">Körtid</span><span class="sxs-lookup"><span data-stu-id="3a28b-117">Run time</span></span> |
| --- | --- | --- |
| <span data-ttu-id="3a28b-118">Förfrågningar och undantag</span><span class="sxs-lookup"><span data-stu-id="3a28b-118">Requests & exceptions</span></span> |<span data-ttu-id="3a28b-119">Ja</span><span class="sxs-lookup"><span data-stu-id="3a28b-119">Yes</span></span> |<span data-ttu-id="3a28b-120">Ja</span><span class="sxs-lookup"><span data-stu-id="3a28b-120">Yes</span></span> |
| [<span data-ttu-id="3a28b-121">Mer detaljerade undantag</span><span class="sxs-lookup"><span data-stu-id="3a28b-121">More detailed exceptions</span></span>](app-insights-asp-net-exceptions.md) | |<span data-ttu-id="3a28b-122">Ja</span><span class="sxs-lookup"><span data-stu-id="3a28b-122">Yes</span></span> |
| [<span data-ttu-id="3a28b-123">Beroendediagnostik</span><span class="sxs-lookup"><span data-stu-id="3a28b-123">Dependency diagnostics</span></span>](app-insights-asp-net-dependencies.md) |<span data-ttu-id="3a28b-124">I .NET 4.6+, men färre detaljer</span><span class="sxs-lookup"><span data-stu-id="3a28b-124">On .NET 4.6+, but less detail</span></span> |<span data-ttu-id="3a28b-125">Ja, fullständiga detaljer: resultatkoder, SQL-kommandotext, HTTP verb</span><span class="sxs-lookup"><span data-stu-id="3a28b-125">Yes, full detail: result codes, SQL command text, HTTP verb</span></span>|
| [<span data-ttu-id="3a28b-126">Systemprestandaräknare</span><span class="sxs-lookup"><span data-stu-id="3a28b-126">System performance counters</span></span>](app-insights-performance-counters.md) |<span data-ttu-id="3a28b-127">Ja</span><span class="sxs-lookup"><span data-stu-id="3a28b-127">Yes</span></span> |<span data-ttu-id="3a28b-128">Ja</span><span class="sxs-lookup"><span data-stu-id="3a28b-128">Yes</span></span> |
| <span data-ttu-id="3a28b-129">[API för anpassad telemetri][api]</span><span class="sxs-lookup"><span data-stu-id="3a28b-129">[API for custom telemetry][api]</span></span> |<span data-ttu-id="3a28b-130">Ja</span><span class="sxs-lookup"><span data-stu-id="3a28b-130">Yes</span></span> |<span data-ttu-id="3a28b-131">Nej</span><span class="sxs-lookup"><span data-stu-id="3a28b-131">No</span></span> |
| [<span data-ttu-id="3a28b-132">Spårningsloggsintegrering</span><span class="sxs-lookup"><span data-stu-id="3a28b-132">Trace log integration</span></span>](app-insights-asp-net-trace-logs.md) |<span data-ttu-id="3a28b-133">Ja</span><span class="sxs-lookup"><span data-stu-id="3a28b-133">Yes</span></span> |<span data-ttu-id="3a28b-134">Nej</span><span class="sxs-lookup"><span data-stu-id="3a28b-134">No</span></span> |
| [<span data-ttu-id="3a28b-135">Sidvy och användardata</span><span class="sxs-lookup"><span data-stu-id="3a28b-135">Page view & user data</span></span>](app-insights-javascript.md) |<span data-ttu-id="3a28b-136">Ja</span><span class="sxs-lookup"><span data-stu-id="3a28b-136">Yes</span></span> |<span data-ttu-id="3a28b-137">Nej</span><span class="sxs-lookup"><span data-stu-id="3a28b-137">No</span></span> |
| <span data-ttu-id="3a28b-138">Behöver toorebuild kod</span><span class="sxs-lookup"><span data-stu-id="3a28b-138">Need toorebuild code</span></span> |<span data-ttu-id="3a28b-139">Ja</span><span class="sxs-lookup"><span data-stu-id="3a28b-139">Yes</span></span> | <span data-ttu-id="3a28b-140">Nej</span><span class="sxs-lookup"><span data-stu-id="3a28b-140">No</span></span> |


## <a name="monitor-a-live-azure-web-app"></a><span data-ttu-id="3a28b-141">Övervaka en Azure-livewebbapp</span><span class="sxs-lookup"><span data-stu-id="3a28b-141">Monitor a live Azure web app</span></span>

<span data-ttu-id="3a28b-142">Om programmet körs som en Azure web tjänstens, här hur tooswitch om övervakning av:</span><span class="sxs-lookup"><span data-stu-id="3a28b-142">If your application is running as an Azure web service, here's how tooswitch on monitoring:</span></span>

* <span data-ttu-id="3a28b-143">Välj Application Insights på Kontrollpanelen för hello app i Azure.</span><span class="sxs-lookup"><span data-stu-id="3a28b-143">Select Application Insights on hello app's control panel in Azure.</span></span>

    ![Konfigurera Application Insights för en Azure-webbapp](./media/app-insights-monitor-performance-live-website-now/azure-web-setup.png)
* <span data-ttu-id="3a28b-145">När hello Application Insights sammanfattningssidan öppnas på hello längst hello nedre tooopen hello fullständig Application Insights-resurs.</span><span class="sxs-lookup"><span data-stu-id="3a28b-145">When hello Application Insights summary page opens, click hello link at hello bottom tooopen hello full Application Insights resource.</span></span>

    ![Klicka dig igenom tooApplication insikter](./media/app-insights-monitor-performance-live-website-now/azure-web-view-more.png)

<span data-ttu-id="3a28b-147">[Övervaka appar för moln och virtuella datorer](app-insights-azure.md).</span><span class="sxs-lookup"><span data-stu-id="3a28b-147">[Monitoring Cloud and VM apps](app-insights-azure.md).</span></span>

### <a name="enable-client-side-monitoring-in-azure"></a><span data-ttu-id="3a28b-148">Aktivera övervakning på klientsidan i Azure</span><span class="sxs-lookup"><span data-stu-id="3a28b-148">Enable client-side monitoring in Azure</span></span>

<span data-ttu-id="3a28b-149">Om du har aktiverat Application Insights i Azure kan du lägga till sidvy och användartelemetri.</span><span class="sxs-lookup"><span data-stu-id="3a28b-149">If you have enabled Application Insights in Azure, you can add page view and user telemetry.</span></span>

1. <span data-ttu-id="3a28b-150">Välj Inställningar > Programinställningar</span><span class="sxs-lookup"><span data-stu-id="3a28b-150">Select Settings > Application Settings</span></span>
2.  <span data-ttu-id="3a28b-151">Under Appinställningar lägger du till ett nytt nyckel/värde-par:</span><span class="sxs-lookup"><span data-stu-id="3a28b-151">Under App Settings, add a new key value pair:</span></span> 
   
    <span data-ttu-id="3a28b-152">Nyckel: `APPINSIGHTS_JAVASCRIPT_ENABLED`</span><span class="sxs-lookup"><span data-stu-id="3a28b-152">Key: `APPINSIGHTS_JAVASCRIPT_ENABLED`</span></span> 
    
    <span data-ttu-id="3a28b-153">Värde:`true`
</span><span class="sxs-lookup"><span data-stu-id="3a28b-153">Value: `true`</span></span>
3. <span data-ttu-id="3a28b-154">**Spara** hello inställningar och **starta om** din app.</span><span class="sxs-lookup"><span data-stu-id="3a28b-154">**Save** hello settings and **Restart** your app.</span></span>

<span data-ttu-id="3a28b-155">hello Application Insights JavaScript SDK är nu injekteras i varje webbsida.</span><span class="sxs-lookup"><span data-stu-id="3a28b-155">hello Application Insights JavaScript SDK is now injected into each web page.</span></span>

## <a name="monitor-a-live-iis-web-app"></a><span data-ttu-id="3a28b-156">Övervaka en IIS-livewebbapp</span><span class="sxs-lookup"><span data-stu-id="3a28b-156">Monitor a live IIS web app</span></span>

<span data-ttu-id="3a28b-157">Om din app finns på en IIS-server aktiverar du Application Insights med hjälp av Statusövervakaren.</span><span class="sxs-lookup"><span data-stu-id="3a28b-157">If your app is hosted on an IIS server, enable Application Insights by using Status Monitor.</span></span>

1. <span data-ttu-id="3a28b-158">Logga in med administratörsbehörighet på IIS-webbservern.</span><span class="sxs-lookup"><span data-stu-id="3a28b-158">On your IIS web server, sign in with administrator credentials.</span></span>
2. <span data-ttu-id="3a28b-159">Om Application Insights Status Monitor inte redan är installerad, hämta och köra hello [statusövervakaren installer](http://go.microsoft.com/fwlink/?LinkId=506648) (eller kör [installationsprogram för webbplattform](https://www.microsoft.com/web/downloads/platform.aspx) och Sök i den för Application Insights Status Övervaka).</span><span class="sxs-lookup"><span data-stu-id="3a28b-159">If Application Insights Status Monitor is not already installed, download and run hello [Status Monitor installer](http://go.microsoft.com/fwlink/?LinkId=506648) (or run [Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx) and search in it for Application Insights Status Monitor).</span></span>
3. <span data-ttu-id="3a28b-160">Välj hello installerade webbprogram eller en webbplats som du vill toomonitor i Status Monitor.</span><span class="sxs-lookup"><span data-stu-id="3a28b-160">In Status Monitor, select hello installed web application or website that you want toomonitor.</span></span> <span data-ttu-id="3a28b-161">Logga in med dina Azure autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="3a28b-161">Sign in with your Azure credentials.</span></span>

    <span data-ttu-id="3a28b-162">Konfigurera hello resurs där du vill att toosee hello resulterar i hello Application Insights-portalen.</span><span class="sxs-lookup"><span data-stu-id="3a28b-162">Configure hello resource where you want toosee hello results in hello Application Insights portal.</span></span> <span data-ttu-id="3a28b-163">(Normalt, är det bästa toocreate en ny resurs.</span><span class="sxs-lookup"><span data-stu-id="3a28b-163">(Normally, it's best toocreate a new resource.</span></span> <span data-ttu-id="3a28b-164">Välj en befintlig resurs om du redan har [webbtester][availability] eller [klientövervakning][client] för den här appen.)</span><span class="sxs-lookup"><span data-stu-id="3a28b-164">Select an existing resource if you already have [web tests][availability] or [client monitoring][client] for this app.)</span></span> 

    ![Välj en app och en resurs.](./media/app-insights-monitor-performance-live-website-now/appinsights-036-configAIC.png)

4. <span data-ttu-id="3a28b-166">Starta om IIS.</span><span class="sxs-lookup"><span data-stu-id="3a28b-166">Restart IIS.</span></span>

    ![Välj omstart hello överst i dialogrutan hello.](./media/app-insights-monitor-performance-live-website-now/appinsights-036-restart.png)

    <span data-ttu-id="3a28b-168">Webbtjänsten avbryts en liten stund.</span><span class="sxs-lookup"><span data-stu-id="3a28b-168">Your web service is interrupted for a short while.</span></span>

## <a name="customize-monitoring-options"></a><span data-ttu-id="3a28b-169">Anpassa övervakningsalternativ</span><span class="sxs-lookup"><span data-stu-id="3a28b-169">Customize monitoring options</span></span>

<span data-ttu-id="3a28b-170">Aktivera Application Insights lägger till DLL-filer och ApplicationInsights.config tooyour webbprogram.</span><span class="sxs-lookup"><span data-stu-id="3a28b-170">Enabling Application Insights adds DLLs and ApplicationInsights.config tooyour web app.</span></span> <span data-ttu-id="3a28b-171">Du kan [redigera hello .config-filen](app-insights-configuration-with-applicationinsights-config.md) toochange vissa av hello alternativ.</span><span class="sxs-lookup"><span data-stu-id="3a28b-171">You can [edit hello .config file](app-insights-configuration-with-applicationinsights-config.md) toochange some of hello options.</span></span>

## <a name="when-you-re-publish-your-app-re-enable-application-insights"></a><span data-ttu-id="3a28b-172">När du publicerar appen igen ska du återaktivera Application Insights</span><span class="sxs-lookup"><span data-stu-id="3a28b-172">When you re-publish your app, re-enable Application Insights</span></span>

<span data-ttu-id="3a28b-173">Innan du publicerar appen igen bör du överväga att [lägga till Application Insights toohello kod i Visual Studio][greenbrown].</span><span class="sxs-lookup"><span data-stu-id="3a28b-173">Before you re-publish your app, consider [adding Application Insights toohello code in Visual Studio][greenbrown].</span></span> <span data-ttu-id="3a28b-174">Du får mer detaljerad telemetri och anpassad telemetri för hello möjlighet toowrite.</span><span class="sxs-lookup"><span data-stu-id="3a28b-174">You'll get more detailed telemetry and hello ability toowrite custom telemetry.</span></span>

<span data-ttu-id="3a28b-175">Om du vill toore-publicera utan att lägga till Application Insights toohello kod, Tänk på att hello distributionsprocessen kan ta bort hello DLL: er och ApplicationInsights.config från hello publicerat webbplats.</span><span class="sxs-lookup"><span data-stu-id="3a28b-175">If you want toore-publish without adding Application Insights toohello code, be aware that hello deployment process may delete hello DLLs and ApplicationInsights.config from hello published web site.</span></span> <span data-ttu-id="3a28b-176">Därför:</span><span class="sxs-lookup"><span data-stu-id="3a28b-176">Therefore:</span></span>

1. <span data-ttu-id="3a28b-177">Om du har redigerat ApplicationInsights.config tar du en kopia av den innan du publicerar appen igen.</span><span class="sxs-lookup"><span data-stu-id="3a28b-177">If you edited ApplicationInsights.config, take a copy of it before you re-publish your app.</span></span>
2. <span data-ttu-id="3a28b-178">Publicera om appen.</span><span class="sxs-lookup"><span data-stu-id="3a28b-178">Republish your app.</span></span>
3. <span data-ttu-id="3a28b-179">Återaktivera övervakning med Application Insights.</span><span class="sxs-lookup"><span data-stu-id="3a28b-179">Re-enable Application Insights monitoring.</span></span> <span data-ttu-id="3a28b-180">(Använd lämplig metod för hello: hello Azure web app på Kontrollpanelen eller hello Status Monitor på en IIS-värd.)</span><span class="sxs-lookup"><span data-stu-id="3a28b-180">(Use hello appropriate method: either hello Azure web app control panel, or hello Status Monitor on an IIS host.)</span></span>
4. <span data-ttu-id="3a28b-181">Återställa alla ändringar utförs på hello .config-fil.</span><span class="sxs-lookup"><span data-stu-id="3a28b-181">Reinstate any edits you performed on hello .config file.</span></span>


## <a name="troubleshooting-runtime-configuration-of-application-insights"></a><span data-ttu-id="3a28b-182">Felsöka körningskonfigurationen av Application Insights</span><span class="sxs-lookup"><span data-stu-id="3a28b-182">Troubleshooting runtime configuration of Application Insights</span></span>

### <a name="cant-connect-no-telemetry"></a><span data-ttu-id="3a28b-183">Går det inte att ansluta?</span><span class="sxs-lookup"><span data-stu-id="3a28b-183">Can't connect?</span></span> <span data-ttu-id="3a28b-184">Ser du ingen telemetri?</span><span class="sxs-lookup"><span data-stu-id="3a28b-184">No telemetry?</span></span>

* <span data-ttu-id="3a28b-185">Öppna [hello nödvändiga utgående portar](app-insights-ip-addresses.md#outgoing-ports) i serverns brandvägg tooallow statusövervakaren toowork.</span><span class="sxs-lookup"><span data-stu-id="3a28b-185">Open [hello necessary outgoing ports](app-insights-ip-addresses.md#outgoing-ports) in your server's firewall tooallow Status Monitor toowork.</span></span>

* <span data-ttu-id="3a28b-186">Öppna Status Monitor och välj ditt program i den vänstra rutan.</span><span class="sxs-lookup"><span data-stu-id="3a28b-186">Open Status Monitor and select your application on left pane.</span></span> <span data-ttu-id="3a28b-187">Kontrollera om det finns några diagnostiska meddelanden för det här programmet i hello ”konfigurationen meddelanden” avsnittet:</span><span class="sxs-lookup"><span data-stu-id="3a28b-187">Check if there are any diagnostics messages for this application in hello "Configuration notifications" section:</span></span>

  ![Öppna bladet toosee begäran om hello prestanda, svarstid, beroende och andra data](./media/app-insights-monitor-performance-live-website-now/appinsights-status-monitor-diagnostics-message.png)
* <span data-ttu-id="3a28b-189">Om du ser ett meddelande om ”otillräckliga behörigheter” på servern hello, försök med hello följande:</span><span class="sxs-lookup"><span data-stu-id="3a28b-189">On hello server, if you see a message about "insufficient permissions", try hello following:</span></span>
  * <span data-ttu-id="3a28b-190">I IIS-hanteraren, Välj programpoolen, öppna **avancerade inställningar**, och under **processmodellen** Observera hello identitet.</span><span class="sxs-lookup"><span data-stu-id="3a28b-190">In IIS Manager, select your application pool, open **Advanced Settings**, and under **Process Model** note hello identity.</span></span>
  * <span data-ttu-id="3a28b-191">Lägg till den här identiteten toohello prestandaövervakningsanvändare datorn management på Kontrollpanelen.</span><span class="sxs-lookup"><span data-stu-id="3a28b-191">In Computer management control panel, add this identity toohello Performance Monitor Users group.</span></span>
* <span data-ttu-id="3a28b-192">Om du har MMA/SCOM (Systems Center Operations Manager) installerat på servern kan vissa versioner vara i konflikt med varandra.</span><span class="sxs-lookup"><span data-stu-id="3a28b-192">If you have MMA/SCOM (Systems Center Operations Manager) installed on your server, some versions can conflict.</span></span> <span data-ttu-id="3a28b-193">Avinstallerar du både SCOM och statusövervakaren och installera om hello senaste versionerna.</span><span class="sxs-lookup"><span data-stu-id="3a28b-193">Uninstall both SCOM and Status Monitor, and re-install hello latest versions.</span></span>
* <span data-ttu-id="3a28b-194">Mer information finns i [Felsökning][qna].</span><span class="sxs-lookup"><span data-stu-id="3a28b-194">See [Troubleshooting][qna].</span></span>

## <a name="system-requirements"></a><span data-ttu-id="3a28b-195">Systemkrav</span><span class="sxs-lookup"><span data-stu-id="3a28b-195">System Requirements</span></span>
<span data-ttu-id="3a28b-196">Operativsystemstöd för Application Insights Status Monitor på servern:</span><span class="sxs-lookup"><span data-stu-id="3a28b-196">OS support for Application Insights Status Monitor on Server:</span></span>

* <span data-ttu-id="3a28b-197">Windows Server 2008</span><span class="sxs-lookup"><span data-stu-id="3a28b-197">Windows Server 2008</span></span>
* <span data-ttu-id="3a28b-198">Windows Server 2008 R2</span><span class="sxs-lookup"><span data-stu-id="3a28b-198">Windows Server 2008 R2</span></span>
* <span data-ttu-id="3a28b-199">Windows Server 2012</span><span class="sxs-lookup"><span data-stu-id="3a28b-199">Windows Server 2012</span></span>
* <span data-ttu-id="3a28b-200">Windows server 2012 R2</span><span class="sxs-lookup"><span data-stu-id="3a28b-200">Windows server 2012 R2</span></span>
* <span data-ttu-id="3a28b-201">Windows Server 2016</span><span class="sxs-lookup"><span data-stu-id="3a28b-201">Windows Server 2016</span></span>

<span data-ttu-id="3a28b-202">med senaste SP och .NET Framework 4.5</span><span class="sxs-lookup"><span data-stu-id="3a28b-202">with latest SP and .NET Framework 4.5</span></span>

<span data-ttu-id="3a28b-203">På klientsidan för hello: Windows 7, 8, 8.1 och 10 igen med .NET Framework 4.5</span><span class="sxs-lookup"><span data-stu-id="3a28b-203">On hello client side: Windows 7, 8, 8.1 and 10, again with .NET Framework 4.5</span></span>

<span data-ttu-id="3a28b-204">IIS-stöd: IIS 7, 7.5, 8, 8.5 (IIS krävs)</span><span class="sxs-lookup"><span data-stu-id="3a28b-204">IIS support is: IIS 7, 7.5, 8, 8.5 (IIS is required)</span></span>

## <a name="automation-with-powershell"></a><span data-ttu-id="3a28b-205">Automatisering med PowerShell</span><span class="sxs-lookup"><span data-stu-id="3a28b-205">Automation with PowerShell</span></span>
<span data-ttu-id="3a28b-206">Du kan starta och stoppa övervakningen med hjälp av PowerShell i din IIS-server.</span><span class="sxs-lookup"><span data-stu-id="3a28b-206">You can start and stop monitoring by using PowerShell on your IIS server.</span></span>

<span data-ttu-id="3a28b-207">Först importera hello Application Insights-modulen:</span><span class="sxs-lookup"><span data-stu-id="3a28b-207">First import hello Application Insights module:</span></span>

`Import-Module 'C:\Program Files\Microsoft Application Insights\Status Monitor\PowerShell\Microsoft.Diagnostics.Agent.StatusMonitor.PowerShell.dll'`

<span data-ttu-id="3a28b-208">Ta reda på vilka appar som övervakas:</span><span class="sxs-lookup"><span data-stu-id="3a28b-208">Find out which apps are being monitored:</span></span>

`Get-ApplicationInsightsMonitoringStatus [-Name appName]`

* <span data-ttu-id="3a28b-209">`-Name`(Valfritt) hello namnet på ett webbprogram.</span><span class="sxs-lookup"><span data-stu-id="3a28b-209">`-Name` (Optional) hello name of a web app.</span></span>
* <span data-ttu-id="3a28b-210">Visar hello Application Insights övervakningsstatus för varje webbprogram (eller hello med namnet app) i den här IIS-servern.</span><span class="sxs-lookup"><span data-stu-id="3a28b-210">Displays hello Application Insights monitoring status for each web app (or hello named app) in this IIS server.</span></span>
* <span data-ttu-id="3a28b-211">Returnerar `ApplicationInsightsApplication` för varje app:</span><span class="sxs-lookup"><span data-stu-id="3a28b-211">Returns `ApplicationInsightsApplication` for each app:</span></span>

  * <span data-ttu-id="3a28b-212">`SdkState==EnabledAfterDeployment`: App som övervakas och var instrumenterats vid körning av hello statusövervakaren verktyg eller genom `Start-ApplicationInsightsMonitoring`.</span><span class="sxs-lookup"><span data-stu-id="3a28b-212">`SdkState==EnabledAfterDeployment`: App is being monitored, and was instrumented at run time, either by hello Status Monitor tool, or by `Start-ApplicationInsightsMonitoring`.</span></span>
  * <span data-ttu-id="3a28b-213">`SdkState==Disabled`: hello app inte instrumenterats för Application Insights.</span><span class="sxs-lookup"><span data-stu-id="3a28b-213">`SdkState==Disabled`: hello app is not instrumented for Application Insights.</span></span> <span data-ttu-id="3a28b-214">Det har aldrig instrumenterats eller körning övervakning har inaktiverats hello statusövervakaren verktyget eller med `Stop-ApplicationInsightsMonitoring`.</span><span class="sxs-lookup"><span data-stu-id="3a28b-214">Either it was never instrumented, or run-time monitoring was disabled with hello Status Monitor tool or with `Stop-ApplicationInsightsMonitoring`.</span></span>
  * <span data-ttu-id="3a28b-215">`SdkState==EnabledByCodeInstrumentation`: hello app har instrumenterats genom att lägga till hello SDK toohello källkoden.</span><span class="sxs-lookup"><span data-stu-id="3a28b-215">`SdkState==EnabledByCodeInstrumentation`: hello app was instrumented by adding hello SDK toohello source code.</span></span> <span data-ttu-id="3a28b-216">Appens SDK kan inte uppdateras eller stoppas.</span><span class="sxs-lookup"><span data-stu-id="3a28b-216">Its SDK cannot be updated or stopped.</span></span>
  * <span data-ttu-id="3a28b-217">`SdkVersion`Visar hello-version som används för att övervaka den här appen.</span><span class="sxs-lookup"><span data-stu-id="3a28b-217">`SdkVersion` shows hello version in use for monitoring this app.</span></span>
  * <span data-ttu-id="3a28b-218">`LatestAvailableSdkVersion`Visar hello-version som är tillgängliga på hello NuGet-galleriet.</span><span class="sxs-lookup"><span data-stu-id="3a28b-218">`LatestAvailableSdkVersion`shows hello version currently available on hello NuGet gallery.</span></span> <span data-ttu-id="3a28b-219">tooupgrade hello toothis programversion, Använd `Update-ApplicationInsightsMonitoring`.</span><span class="sxs-lookup"><span data-stu-id="3a28b-219">tooupgrade hello app toothis version, use `Update-ApplicationInsightsMonitoring`.</span></span>

`Start-ApplicationInsightsMonitoring -Name appName -InstrumentationKey 00000000-000-000-000-0000000`

* <span data-ttu-id="3a28b-220">`-Name`hello namnet på hello app i IIS</span><span class="sxs-lookup"><span data-stu-id="3a28b-220">`-Name` hello name of hello app in IIS</span></span>
* <span data-ttu-id="3a28b-221">`-InstrumentationKey`Hej ikey av hello Application Insights-resurs där du vill att hello resultat toobe visas.</span><span class="sxs-lookup"><span data-stu-id="3a28b-221">`-InstrumentationKey` hello ikey of hello Application Insights resource where you want hello results toobe displayed.</span></span>
* <span data-ttu-id="3a28b-222">Den här cmdleten påverkar endast appar som inte redan har instrumenterats, dvs. SdkState==NotInstrumented.</span><span class="sxs-lookup"><span data-stu-id="3a28b-222">This cmdlet only affects apps that are not already instrumented - that is, SdkState==NotInstrumented.</span></span>

    <span data-ttu-id="3a28b-223">hello cmdlet påverkar inte en app som redan instrumenterats.</span><span class="sxs-lookup"><span data-stu-id="3a28b-223">hello cmdlet does not affect an app that is already instrumented.</span></span> <span data-ttu-id="3a28b-224">Den inte roll om hello app har instrumenterats vid byggning genom att lägga till hello SDK toohello kod eller vid körningstiden på en tidigare användning av denna cmdlet.</span><span class="sxs-lookup"><span data-stu-id="3a28b-224">It does not matter whether hello app was instrumented at build time by adding hello SDK toohello code, or at run time by a previous use of this cmdlet.</span></span>

    <span data-ttu-id="3a28b-225">hello SDK version som används för tooinstrument hello appen är hello-versionen som senast ned toothis server.</span><span class="sxs-lookup"><span data-stu-id="3a28b-225">hello SDK version used tooinstrument hello app is hello version that was most recently downloaded toothis server.</span></span>

    <span data-ttu-id="3a28b-226">toodownload hello senaste versionen, använda uppdateringen ApplicationInsightsVersion.</span><span class="sxs-lookup"><span data-stu-id="3a28b-226">toodownload hello latest version, use Update-ApplicationInsightsVersion.</span></span>
* <span data-ttu-id="3a28b-227">Returnerar `ApplicationInsightsApplication` om åtgärden lyckades.</span><span class="sxs-lookup"><span data-stu-id="3a28b-227">Returns `ApplicationInsightsApplication` on success.</span></span> <span data-ttu-id="3a28b-228">Om det misslyckas, loggar en trace toostderr.</span><span class="sxs-lookup"><span data-stu-id="3a28b-228">If it fails, it logs a trace toostderr.</span></span>

          Name                      : Default Web Site/WebApp1
          InstrumentationKey        : 00000000-0000-0000-0000-000000000000
          ProfilerState             : ApplicationInsights
          SdkState                  : EnabledAfterDeployment
          SdkVersion                : 1.2.1
          LatestAvailableSdkVersion : 1.2.3

`Stop-ApplicationInsightsMonitoring [-Name appName | -All]`

* <span data-ttu-id="3a28b-229">`-Name`hello namnet på en app i IIS</span><span class="sxs-lookup"><span data-stu-id="3a28b-229">`-Name` hello name of an app in IIS</span></span>
* <span data-ttu-id="3a28b-230">`-All` Stoppar övervakningen av alla appar på IIS-servern där `SdkState==EnabledAfterDeployment`</span><span class="sxs-lookup"><span data-stu-id="3a28b-230">`-All` Stops monitoring all apps in this IIS server for which `SdkState==EnabledAfterDeployment`</span></span>
* <span data-ttu-id="3a28b-231">Stoppar övervakningen av hello angivna appar och tar bort instrumentation.</span><span class="sxs-lookup"><span data-stu-id="3a28b-231">Stops monitoring hello specified apps and removes instrumentation.</span></span> <span data-ttu-id="3a28b-232">Den fungerar bara för appar som har varit instrumenterats på körning med hello övervakning av innehållsstatus verktyg eller starta ApplicationInsightsApplication.</span><span class="sxs-lookup"><span data-stu-id="3a28b-232">It only works for apps that have been instrumented at run-time using hello Status Monitoring tool or Start-ApplicationInsightsApplication.</span></span> <span data-ttu-id="3a28b-233">(`SdkState==EnabledAfterDeployment`)</span><span class="sxs-lookup"><span data-stu-id="3a28b-233">(`SdkState==EnabledAfterDeployment`)</span></span>
* <span data-ttu-id="3a28b-234">Returnerar ApplicationInsightsApplication.</span><span class="sxs-lookup"><span data-stu-id="3a28b-234">Returns ApplicationInsightsApplication.</span></span>

<span data-ttu-id="3a28b-235">`Update-ApplicationInsightsMonitoring -Name appName [-InstrumentationKey "0000000-0000-000-000-0000"`]</span><span class="sxs-lookup"><span data-stu-id="3a28b-235">`Update-ApplicationInsightsMonitoring -Name appName [-InstrumentationKey "0000000-0000-000-000-0000"`]</span></span>

* <span data-ttu-id="3a28b-236">`-Name`: hello namnet på en webbapp i IIS.</span><span class="sxs-lookup"><span data-stu-id="3a28b-236">`-Name`: hello name of a web app in IIS.</span></span>
* <span data-ttu-id="3a28b-237">`-InstrumentationKey` (Valfritt.) Använd den här toochange hello resurs toowhich hello appens telemetri skickas.</span><span class="sxs-lookup"><span data-stu-id="3a28b-237">`-InstrumentationKey` (Optional.) Use this toochange hello resource toowhich hello app's telemetry is sent.</span></span>
* <span data-ttu-id="3a28b-238">Den här cmdleten:</span><span class="sxs-lookup"><span data-stu-id="3a28b-238">This cmdlet:</span></span>
  * <span data-ttu-id="3a28b-239">Uppgraderingar hello med namnet app toohello version av hello SDK senast hämtats toothis datorn.</span><span class="sxs-lookup"><span data-stu-id="3a28b-239">Upgrades hello named app toohello version of hello SDK most recently downloaded toothis machine.</span></span> <span data-ttu-id="3a28b-240">(Fungerar bara om `SdkState==EnabledAfterDeployment`)</span><span class="sxs-lookup"><span data-stu-id="3a28b-240">(Only works if `SdkState==EnabledAfterDeployment`)</span></span>
  * <span data-ttu-id="3a28b-241">Om du anger en instrumentation nyckel är hello med namnet app omkonfigurerats toosend telemetri toohello resursen med nyckeln.</span><span class="sxs-lookup"><span data-stu-id="3a28b-241">If you provide an instrumentation key, hello named app is reconfigured toosend telemetry toohello resource with that key.</span></span> <span data-ttu-id="3a28b-242">(Fungerar om `SdkState != Disabled`)</span><span class="sxs-lookup"><span data-stu-id="3a28b-242">(Works if `SdkState != Disabled`)</span></span>

`Update-ApplicationInsightsVersion`

* <span data-ttu-id="3a28b-243">Hämtar hello senaste Application Insights SDK toohello server.</span><span class="sxs-lookup"><span data-stu-id="3a28b-243">Downloads hello latest Application Insights SDK toohello server.</span></span>

## <span data-ttu-id="3a28b-244"><a name="questions"></a>Frågor om Statusövervakaren</span><span class="sxs-lookup"><span data-stu-id="3a28b-244"><a name="questions"></a>Questions about Status Monitor</span></span>

### <a name="what-is-status-monitor"></a><span data-ttu-id="3a28b-245">Vad är Statusövervakaren?</span><span class="sxs-lookup"><span data-stu-id="3a28b-245">What is Status Monitor?</span></span>

<span data-ttu-id="3a28b-246">Ett program som du installerar på din IIS-webbserver.</span><span class="sxs-lookup"><span data-stu-id="3a28b-246">A desktop application that you install in your IIS web server.</span></span> <span data-ttu-id="3a28b-247">Det hjälper dig instrumentera och konfigurera webbappar.</span><span class="sxs-lookup"><span data-stu-id="3a28b-247">It helps you instrument and configure web apps.</span></span> 

### <a name="when-do-i-use-status-monitor"></a><span data-ttu-id="3a28b-248">När ska jag använda Statusövervakaren?</span><span class="sxs-lookup"><span data-stu-id="3a28b-248">When do I use Status Monitor?</span></span>

* <span data-ttu-id="3a28b-249">tooinstrument någon webbapp som körs på IIS-servern - även om det redan körs.</span><span class="sxs-lookup"><span data-stu-id="3a28b-249">tooinstrument any web app that is running on your IIS server - even if it is already running.</span></span>
* <span data-ttu-id="3a28b-250">tooenable ytterligare telemetri för webbprogram som har [byggts med hello Application Insights SDK](app-insights-asp-net.md) vid kompileringen.</span><span class="sxs-lookup"><span data-stu-id="3a28b-250">tooenable additional telemetry for web apps that have been [built with hello Application Insights SDK](app-insights-asp-net.md) at compile time.</span></span> 

### <a name="can-i-close-it-after-it-runs"></a><span data-ttu-id="3a28b-251">Kan jag stänga den när den körs?</span><span class="sxs-lookup"><span data-stu-id="3a28b-251">Can I close it after it runs?</span></span>

<span data-ttu-id="3a28b-252">Ja.</span><span class="sxs-lookup"><span data-stu-id="3a28b-252">Yes.</span></span> <span data-ttu-id="3a28b-253">Du kan stänga när den har instrumenterats hello webbplatser som du väljer.</span><span class="sxs-lookup"><span data-stu-id="3a28b-253">After it has instrumented hello websites you select, you can close it.</span></span>

<span data-ttu-id="3a28b-254">Den samlar inte in telemetri på egen hand.</span><span class="sxs-lookup"><span data-stu-id="3a28b-254">It doesn't collect telemetry by itself.</span></span> <span data-ttu-id="3a28b-255">Bara konfigurerar hello webbprogram och anger vissa behörigheter.</span><span class="sxs-lookup"><span data-stu-id="3a28b-255">It just configures hello web apps and sets some permissions.</span></span>

### <a name="what-does-status-monitor-do"></a><span data-ttu-id="3a28b-256">Vad gör Statusövervakaren?</span><span class="sxs-lookup"><span data-stu-id="3a28b-256">What does Status Monitor do?</span></span>

<span data-ttu-id="3a28b-257">När du väljer en webbapp för statusövervakaren tooinstrument:</span><span class="sxs-lookup"><span data-stu-id="3a28b-257">When you select a web app for Status Monitor tooinstrument:</span></span>

* <span data-ttu-id="3a28b-258">Hämtar och placerar hello Application Insights sammansättningar och .config-fil i hello webbapp binärfiler mapp.</span><span class="sxs-lookup"><span data-stu-id="3a28b-258">Downloads and places hello Application Insights assemblies and .config file in hello web app's binaries folder.</span></span>
* <span data-ttu-id="3a28b-259">Ändrar `web.config` tooadd hello Application Insights http-spårning modul.</span><span class="sxs-lookup"><span data-stu-id="3a28b-259">Modifies `web.config` tooadd hello Application Insights HTTP tracking module.</span></span>
* <span data-ttu-id="3a28b-260">Aktiverar CLR profilering toocollect beroendeanrop.</span><span class="sxs-lookup"><span data-stu-id="3a28b-260">Enables CLR profiling toocollect dependency calls.</span></span>

### <a name="do-i-need-toorun-status-monitor-whenever-i-update-hello-app"></a><span data-ttu-id="3a28b-261">Behöver jag toorun statusövervakaren när jag uppdaterar hello app?</span><span class="sxs-lookup"><span data-stu-id="3a28b-261">Do I need toorun Status Monitor whenever I update hello app?</span></span>

<span data-ttu-id="3a28b-262">Inte om du distribuerar om inkrementellt.</span><span class="sxs-lookup"><span data-stu-id="3a28b-262">Not if you redeploy incrementally.</span></span> 

<span data-ttu-id="3a28b-263">Om du väljer hello ”ta bort befintliga filer' i hello publiceringsprocess, behöver du toore kör statusövervakaren tooconfigure Application Insights.</span><span class="sxs-lookup"><span data-stu-id="3a28b-263">If you select hello 'delete existing files' option in hello publish process, you would need toore-run Status Monitor tooconfigure Application Insights.</span></span>

### <a name="what-telemetry-is-collected"></a><span data-ttu-id="3a28b-264">Vilken telemetri samlas in?</span><span class="sxs-lookup"><span data-stu-id="3a28b-264">What telemetry is collected?</span></span>

<span data-ttu-id="3a28b-265">För program som du endast instrumenterar vid körning med Statusövervakaren:</span><span class="sxs-lookup"><span data-stu-id="3a28b-265">For applications that you instrument only at run-time by using Status Monitor:</span></span>

* <span data-ttu-id="3a28b-266">HTTP-begäranden</span><span class="sxs-lookup"><span data-stu-id="3a28b-266">HTTP requests</span></span>
* <span data-ttu-id="3a28b-267">Anropar toodependencies</span><span class="sxs-lookup"><span data-stu-id="3a28b-267">Calls toodependencies</span></span>
* <span data-ttu-id="3a28b-268">Undantag</span><span class="sxs-lookup"><span data-stu-id="3a28b-268">Exceptions</span></span>
* <span data-ttu-id="3a28b-269">Prestandaräknare</span><span class="sxs-lookup"><span data-stu-id="3a28b-269">Performance counters</span></span>

<span data-ttu-id="3a28b-270">För program som redan har instrumenterats vid kompilering:</span><span class="sxs-lookup"><span data-stu-id="3a28b-270">For applications already instrumented at compile time:</span></span>

 * <span data-ttu-id="3a28b-271">Processräknare.</span><span class="sxs-lookup"><span data-stu-id="3a28b-271">Process counters.</span></span>
 * <span data-ttu-id="3a28b-272">Beroendeanrop (.NET 4.5;), returvärden i beroendeanrop (.NET 4.6).</span><span class="sxs-lookup"><span data-stu-id="3a28b-272">Dependency calls (.NET 4.5); return values in dependency calls (.NET 4.6).</span></span>
 * <span data-ttu-id="3a28b-273">Undantag i stackspårningsvärden.</span><span class="sxs-lookup"><span data-stu-id="3a28b-273">Exception stack trace values.</span></span>

[<span data-ttu-id="3a28b-274">Läs mer</span><span class="sxs-lookup"><span data-stu-id="3a28b-274">Learn more</span></span>](http://apmtips.com/blog/2016/11/18/how-application-insights-status-monitor-not-monitors-dependencies/)

## <a name="video"></a><span data-ttu-id="3a28b-275">Video</span><span class="sxs-lookup"><span data-stu-id="3a28b-275">Video</span></span>

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/100/player]

## <span data-ttu-id="3a28b-276"><a name="next"></a>Nästa steg</span><span class="sxs-lookup"><span data-stu-id="3a28b-276"><a name="next"></a>Next steps</span></span>

<span data-ttu-id="3a28b-277">Visa telemetrin:</span><span class="sxs-lookup"><span data-stu-id="3a28b-277">View your telemetry:</span></span>

* <span data-ttu-id="3a28b-278">[Utforska mått](app-insights-metrics-explorer.md) toomonitor prestanda och användning</span><span class="sxs-lookup"><span data-stu-id="3a28b-278">[Explore metrics](app-insights-metrics-explorer.md) toomonitor performance and usage</span></span>
* <span data-ttu-id="3a28b-279">[Söka efter händelser och loggar] [ diagnostic] toodiagnose problem</span><span class="sxs-lookup"><span data-stu-id="3a28b-279">[Search events and logs][diagnostic] toodiagnose problems</span></span>
* <span data-ttu-id="3a28b-280">[Analys](app-insights-analytics.md) för mer avancerade frågor</span><span class="sxs-lookup"><span data-stu-id="3a28b-280">[Analytics](app-insights-analytics.md) for more advanced queries</span></span>
* [<span data-ttu-id="3a28b-281">Skapa instrumentpaneler</span><span class="sxs-lookup"><span data-stu-id="3a28b-281">Create dashboards</span></span>](app-insights-dashboards.md)

<span data-ttu-id="3a28b-282">Lägg till mer telemetri:</span><span class="sxs-lookup"><span data-stu-id="3a28b-282">Add more telemetry:</span></span>

* <span data-ttu-id="3a28b-283">[Skapa webbtester] [ availability] toomake säker webbplatsen är aktiv.</span><span class="sxs-lookup"><span data-stu-id="3a28b-283">[Create web tests][availability] toomake sure your site stays live.</span></span>
* <span data-ttu-id="3a28b-284">[Lägg till webbplats klienten telemetri] [ usage] toosee undantag från webbsidan kod och toolet du infogar spåra anrop.</span><span class="sxs-lookup"><span data-stu-id="3a28b-284">[Add web client telemetry][usage] toosee exceptions from web page code and toolet you insert trace calls.</span></span>
* <span data-ttu-id="3a28b-285">[Lägg till Application Insights SDK tooyour kod] [ greenbrown] så att du kan infoga spårning och logga anrop</span><span class="sxs-lookup"><span data-stu-id="3a28b-285">[Add Application Insights SDK tooyour code][greenbrown] so that you can insert trace and log calls</span></span>

<!--Link references-->

[api]: app-insights-api-custom-events-metrics.md
[availability]: app-insights-monitor-web-app-availability.md
[client]: app-insights-javascript.md
[diagnostic]: app-insights-diagnostic-search.md
[greenbrown]: app-insights-asp-net.md
[qna]: app-insights-troubleshoot-faq.md
[roles]: app-insights-resources-roles-access-control.md
[usage]: app-insights-javascript.md
