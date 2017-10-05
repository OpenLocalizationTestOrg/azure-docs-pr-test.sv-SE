---
title: "Övervaka en ASP.NET-livewebbapp med Azure Application Insights | Microsoft Docs"
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
ms.openlocfilehash: d07a0c81f89100c378456bbea8dca1c009cc8d77
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="instrument-web-apps-at-runtime-with-application-insights"></a><span data-ttu-id="e8b9b-104">Instrumentera webbappar vid körning med Application Insights</span><span class="sxs-lookup"><span data-stu-id="e8b9b-104">Instrument web apps at runtime with Application Insights</span></span>


<span data-ttu-id="e8b9b-105">Du kan instrumentera en live-webbapp med Azure Application Insights utan att behöva ändra eller omdistribuera din kod.</span><span class="sxs-lookup"><span data-stu-id="e8b9b-105">You can instrument a live web app with Azure Application Insights, without having to modify or redeploy your code.</span></span> <span data-ttu-id="e8b9b-106">Om dina appar hanteras av en lokal IIS-server installerar du	Statusövervakare.</span><span class="sxs-lookup"><span data-stu-id="e8b9b-106">If your apps are hosted by an on-premises IIS server, install Status Monitor.</span></span> <span data-ttu-id="e8b9b-107">Om de är Azure-webbappar eller körs i en virtuell Azure-dator kan du aktivera Application Insights-övervakning från Azures kontrollpanel.</span><span class="sxs-lookup"><span data-stu-id="e8b9b-107">If they're Azure web apps or run in an Azure VM, you can switch on Application Insights monitoring from the Azure control panel.</span></span> <span data-ttu-id="e8b9b-108">(Det finns även olika artiklar om hur du instrumenterar [J2EE-livewebbappar](app-insights-java-live.md) och [Azure Cloud Services](app-insights-cloudservices.md).) Du behöver en [Microsoft Azure](http://azure.com)-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="e8b9b-108">(There are also separate articles about instrumenting [live J2EE web apps](app-insights-java-live.md) and [Azure Cloud Services](app-insights-cloudservices.md).) You need a [Microsoft Azure](http://azure.com) subscription.</span></span>

![exempeldiagram](./media/app-insights-monitor-performance-live-website-now/10-intro.png)

<span data-ttu-id="e8b9b-110">Du kan tillämpa Application Insights på dina .NET-webbprogram via tre vägar:</span><span class="sxs-lookup"><span data-stu-id="e8b9b-110">You have a choice of three routes to apply Application Insights to your .NET web applications:</span></span>

* <span data-ttu-id="e8b9b-111">**Byggtid:** [Lägg till Application Insights SDK][greenbrown] i webbappens kod.</span><span class="sxs-lookup"><span data-stu-id="e8b9b-111">**Build time:** [Add the Application Insights SDK][greenbrown] to your web app code.</span></span>
* <span data-ttu-id="e8b9b-112">**Körtid:** Instrumentera din webbapp på servern, så som beskrivs nedan, utan att behöva bygga om och omdistribuera koden.</span><span class="sxs-lookup"><span data-stu-id="e8b9b-112">**Run time:** Instrument your web app on the server, as described below, without rebuilding and redeploying the code.</span></span>
* <span data-ttu-id="e8b9b-113">**Båda:** Skapa SDK:n i din webbappskod, och tillämpa även körningstilläggen.</span><span class="sxs-lookup"><span data-stu-id="e8b9b-113">**Both:** Build the SDK into your web app code, and also apply the run-time extensions.</span></span> <span data-ttu-id="e8b9b-114">Få ut det bästa av två världar.</span><span class="sxs-lookup"><span data-stu-id="e8b9b-114">Get the best of both options.</span></span>

<span data-ttu-id="e8b9b-115">Här är en sammanfattning av vad du får med respektive väg:</span><span class="sxs-lookup"><span data-stu-id="e8b9b-115">Here's a summary of what you get by each route:</span></span>

|  | <span data-ttu-id="e8b9b-116">Byggtid</span><span class="sxs-lookup"><span data-stu-id="e8b9b-116">Build time</span></span> | <span data-ttu-id="e8b9b-117">Körtid</span><span class="sxs-lookup"><span data-stu-id="e8b9b-117">Run time</span></span> |
| --- | --- | --- |
| <span data-ttu-id="e8b9b-118">Förfrågningar och undantag</span><span class="sxs-lookup"><span data-stu-id="e8b9b-118">Requests & exceptions</span></span> |<span data-ttu-id="e8b9b-119">Ja</span><span class="sxs-lookup"><span data-stu-id="e8b9b-119">Yes</span></span> |<span data-ttu-id="e8b9b-120">Ja</span><span class="sxs-lookup"><span data-stu-id="e8b9b-120">Yes</span></span> |
| [<span data-ttu-id="e8b9b-121">Mer detaljerade undantag</span><span class="sxs-lookup"><span data-stu-id="e8b9b-121">More detailed exceptions</span></span>](app-insights-asp-net-exceptions.md) | |<span data-ttu-id="e8b9b-122">Ja</span><span class="sxs-lookup"><span data-stu-id="e8b9b-122">Yes</span></span> |
| [<span data-ttu-id="e8b9b-123">Beroendediagnostik</span><span class="sxs-lookup"><span data-stu-id="e8b9b-123">Dependency diagnostics</span></span>](app-insights-asp-net-dependencies.md) |<span data-ttu-id="e8b9b-124">I .NET 4.6+, men färre detaljer</span><span class="sxs-lookup"><span data-stu-id="e8b9b-124">On .NET 4.6+, but less detail</span></span> |<span data-ttu-id="e8b9b-125">Ja, fullständiga detaljer: resultatkoder, SQL-kommandotext, HTTP verb</span><span class="sxs-lookup"><span data-stu-id="e8b9b-125">Yes, full detail: result codes, SQL command text, HTTP verb</span></span>|
| [<span data-ttu-id="e8b9b-126">Systemprestandaräknare</span><span class="sxs-lookup"><span data-stu-id="e8b9b-126">System performance counters</span></span>](app-insights-performance-counters.md) |<span data-ttu-id="e8b9b-127">Ja</span><span class="sxs-lookup"><span data-stu-id="e8b9b-127">Yes</span></span> |<span data-ttu-id="e8b9b-128">Ja</span><span class="sxs-lookup"><span data-stu-id="e8b9b-128">Yes</span></span> |
| <span data-ttu-id="e8b9b-129">[API för anpassad telemetri][api]</span><span class="sxs-lookup"><span data-stu-id="e8b9b-129">[API for custom telemetry][api]</span></span> |<span data-ttu-id="e8b9b-130">Ja</span><span class="sxs-lookup"><span data-stu-id="e8b9b-130">Yes</span></span> |<span data-ttu-id="e8b9b-131">Nej</span><span class="sxs-lookup"><span data-stu-id="e8b9b-131">No</span></span> |
| [<span data-ttu-id="e8b9b-132">Spårningsloggsintegrering</span><span class="sxs-lookup"><span data-stu-id="e8b9b-132">Trace log integration</span></span>](app-insights-asp-net-trace-logs.md) |<span data-ttu-id="e8b9b-133">Ja</span><span class="sxs-lookup"><span data-stu-id="e8b9b-133">Yes</span></span> |<span data-ttu-id="e8b9b-134">Nej</span><span class="sxs-lookup"><span data-stu-id="e8b9b-134">No</span></span> |
| [<span data-ttu-id="e8b9b-135">Sidvy och användardata</span><span class="sxs-lookup"><span data-stu-id="e8b9b-135">Page view & user data</span></span>](app-insights-javascript.md) |<span data-ttu-id="e8b9b-136">Ja</span><span class="sxs-lookup"><span data-stu-id="e8b9b-136">Yes</span></span> |<span data-ttu-id="e8b9b-137">Nej</span><span class="sxs-lookup"><span data-stu-id="e8b9b-137">No</span></span> |
| <span data-ttu-id="e8b9b-138">Du måste återskapa koden</span><span class="sxs-lookup"><span data-stu-id="e8b9b-138">Need to rebuild code</span></span> |<span data-ttu-id="e8b9b-139">Ja</span><span class="sxs-lookup"><span data-stu-id="e8b9b-139">Yes</span></span> | <span data-ttu-id="e8b9b-140">Nej</span><span class="sxs-lookup"><span data-stu-id="e8b9b-140">No</span></span> |


## <a name="monitor-a-live-azure-web-app"></a><span data-ttu-id="e8b9b-141">Övervaka en Azure-livewebbapp</span><span class="sxs-lookup"><span data-stu-id="e8b9b-141">Monitor a live Azure web app</span></span>

<span data-ttu-id="e8b9b-142">Om programmet körs som en Azure-webbtjänst kan du aktivera övervakning på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="e8b9b-142">If your application is running as an Azure web service, here's how to switch on monitoring:</span></span>

* <span data-ttu-id="e8b9b-143">Välj Application Insights på appens kontrollpanel i Azure.</span><span class="sxs-lookup"><span data-stu-id="e8b9b-143">Select Application Insights on the app's control panel in Azure.</span></span>

    ![Konfigurera Application Insights för en Azure-webbapp](./media/app-insights-monitor-performance-live-website-now/azure-web-setup.png)
* <span data-ttu-id="e8b9b-145">Klicka på länken längst ned för att öppna den fullständiga Applications Insights-resursen när sammanfattningssidan öppnas.</span><span class="sxs-lookup"><span data-stu-id="e8b9b-145">When the Application Insights summary page opens, click the link at the bottom to open the full Application Insights resource.</span></span>

    ![Klicka här så kommer du till Application Insights](./media/app-insights-monitor-performance-live-website-now/azure-web-view-more.png)

<span data-ttu-id="e8b9b-147">[Övervaka appar för moln och virtuella datorer](app-insights-azure.md).</span><span class="sxs-lookup"><span data-stu-id="e8b9b-147">[Monitoring Cloud and VM apps](app-insights-azure.md).</span></span>

### <a name="enable-client-side-monitoring-in-azure"></a><span data-ttu-id="e8b9b-148">Aktivera övervakning på klientsidan i Azure</span><span class="sxs-lookup"><span data-stu-id="e8b9b-148">Enable client-side monitoring in Azure</span></span>

<span data-ttu-id="e8b9b-149">Om du har aktiverat Application Insights i Azure kan du lägga till sidvy och användartelemetri.</span><span class="sxs-lookup"><span data-stu-id="e8b9b-149">If you have enabled Application Insights in Azure, you can add page view and user telemetry.</span></span>

1. <span data-ttu-id="e8b9b-150">Välj Inställningar > Programinställningar</span><span class="sxs-lookup"><span data-stu-id="e8b9b-150">Select Settings > Application Settings</span></span>
2.  <span data-ttu-id="e8b9b-151">Under Appinställningar lägger du till ett nytt nyckel/värde-par:</span><span class="sxs-lookup"><span data-stu-id="e8b9b-151">Under App Settings, add a new key value pair:</span></span> 
   
    <span data-ttu-id="e8b9b-152">Nyckel: `APPINSIGHTS_JAVASCRIPT_ENABLED`</span><span class="sxs-lookup"><span data-stu-id="e8b9b-152">Key: `APPINSIGHTS_JAVASCRIPT_ENABLED`</span></span> 
    
    <span data-ttu-id="e8b9b-153">Värde:`true`
</span><span class="sxs-lookup"><span data-stu-id="e8b9b-153">Value: `true`</span></span>
3. <span data-ttu-id="e8b9b-154">**Spara** inställningarna och **starta om** din app.</span><span class="sxs-lookup"><span data-stu-id="e8b9b-154">**Save** the settings and **Restart** your app.</span></span>

<span data-ttu-id="e8b9b-155">Application Insights JavaScript SDK infogas nu i alla webbsidor.</span><span class="sxs-lookup"><span data-stu-id="e8b9b-155">The Application Insights JavaScript SDK is now injected into each web page.</span></span>

## <a name="monitor-a-live-iis-web-app"></a><span data-ttu-id="e8b9b-156">Övervaka en IIS-livewebbapp</span><span class="sxs-lookup"><span data-stu-id="e8b9b-156">Monitor a live IIS web app</span></span>

<span data-ttu-id="e8b9b-157">Om din app finns på en IIS-server aktiverar du Application Insights med hjälp av Statusövervakaren.</span><span class="sxs-lookup"><span data-stu-id="e8b9b-157">If your app is hosted on an IIS server, enable Application Insights by using Status Monitor.</span></span>

1. <span data-ttu-id="e8b9b-158">Logga in med administratörsbehörighet på IIS-webbservern.</span><span class="sxs-lookup"><span data-stu-id="e8b9b-158">On your IIS web server, sign in with administrator credentials.</span></span>
2. <span data-ttu-id="e8b9b-159">Om Application Insights Status Monitor inte redan är installerat laddar du ned och kör [installationsprogrammet för Status Monitor](http://go.microsoft.com/fwlink/?LinkId=506648) (eller kör [installationsprogrammet för webbplattform](https://www.microsoft.com/web/downloads/platform.aspx) och söker efter det i for Application Insights Status Monitor).</span><span class="sxs-lookup"><span data-stu-id="e8b9b-159">If Application Insights Status Monitor is not already installed, download and run the [Status Monitor installer](http://go.microsoft.com/fwlink/?LinkId=506648) (or run [Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx) and search in it for Application Insights Status Monitor).</span></span>
3. <span data-ttu-id="e8b9b-160">I statusövervakaren väljer du den installerade webbappen eller en webbplats som du vill övervaka.</span><span class="sxs-lookup"><span data-stu-id="e8b9b-160">In Status Monitor, select the installed web application or website that you want to monitor.</span></span> <span data-ttu-id="e8b9b-161">Logga in med dina Azure autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="e8b9b-161">Sign in with your Azure credentials.</span></span>

    <span data-ttu-id="e8b9b-162">Konfigurera den resurs där du vill visa resultatet i Application Insights-portalen.</span><span class="sxs-lookup"><span data-stu-id="e8b9b-162">Configure the resource where you want to see the results in the Application Insights portal.</span></span> <span data-ttu-id="e8b9b-163">(Normalt är det bäst att skapa en ny resurs.</span><span class="sxs-lookup"><span data-stu-id="e8b9b-163">(Normally, it's best to create a new resource.</span></span> <span data-ttu-id="e8b9b-164">Välj en befintlig resurs om du redan har [webbtester][availability] eller [klientövervakning][client] för den här appen.)</span><span class="sxs-lookup"><span data-stu-id="e8b9b-164">Select an existing resource if you already have [web tests][availability] or [client monitoring][client] for this app.)</span></span> 

    ![Välj en app och en resurs.](./media/app-insights-monitor-performance-live-website-now/appinsights-036-configAIC.png)

4. <span data-ttu-id="e8b9b-166">Starta om IIS.</span><span class="sxs-lookup"><span data-stu-id="e8b9b-166">Restart IIS.</span></span>

    ![Välj Starta om överst i dialogrutan.](./media/app-insights-monitor-performance-live-website-now/appinsights-036-restart.png)

    <span data-ttu-id="e8b9b-168">Webbtjänsten avbryts en liten stund.</span><span class="sxs-lookup"><span data-stu-id="e8b9b-168">Your web service is interrupted for a short while.</span></span>

## <a name="customize-monitoring-options"></a><span data-ttu-id="e8b9b-169">Anpassa övervakningsalternativ</span><span class="sxs-lookup"><span data-stu-id="e8b9b-169">Customize monitoring options</span></span>

<span data-ttu-id="e8b9b-170">När Application Insights aktiveras läggs DLL-filer och ApplicationInsights.config till i webbappen.</span><span class="sxs-lookup"><span data-stu-id="e8b9b-170">Enabling Application Insights adds DLLs and ApplicationInsights.config to your web app.</span></span> <span data-ttu-id="e8b9b-171">Du kan [redigera .config-filen](app-insights-configuration-with-applicationinsights-config.md) och ändra en del av alternativen.</span><span class="sxs-lookup"><span data-stu-id="e8b9b-171">You can [edit the .config file](app-insights-configuration-with-applicationinsights-config.md) to change some of the options.</span></span>

## <a name="when-you-re-publish-your-app-re-enable-application-insights"></a><span data-ttu-id="e8b9b-172">När du publicerar appen igen ska du återaktivera Application Insights</span><span class="sxs-lookup"><span data-stu-id="e8b9b-172">When you re-publish your app, re-enable Application Insights</span></span>

<span data-ttu-id="e8b9b-173">Innan du publicerar din app igen bör du överväga att [lägga till Applications Insights i koden i Visual Studio][greenbrown].</span><span class="sxs-lookup"><span data-stu-id="e8b9b-173">Before you re-publish your app, consider [adding Application Insights to the code in Visual Studio][greenbrown].</span></span> <span data-ttu-id="e8b9b-174">Du får mer detaljerad telemetri och möjligheten att skriva anpassad telemetri.</span><span class="sxs-lookup"><span data-stu-id="e8b9b-174">You'll get more detailed telemetry and the ability to write custom telemetry.</span></span>

<span data-ttu-id="e8b9b-175">Om du vill publicera på nytt utan att lägga till Application Insights i koden, tänk på att distributionsprocessen kan ta bort DLL-filer och ApplicationInsights.config från den publicerade webbplatsen.</span><span class="sxs-lookup"><span data-stu-id="e8b9b-175">If you want to re-publish without adding Application Insights to the code, be aware that the deployment process may delete the DLLs and ApplicationInsights.config from the published web site.</span></span> <span data-ttu-id="e8b9b-176">Därför:</span><span class="sxs-lookup"><span data-stu-id="e8b9b-176">Therefore:</span></span>

1. <span data-ttu-id="e8b9b-177">Om du har redigerat ApplicationInsights.config tar du en kopia av den innan du publicerar appen igen.</span><span class="sxs-lookup"><span data-stu-id="e8b9b-177">If you edited ApplicationInsights.config, take a copy of it before you re-publish your app.</span></span>
2. <span data-ttu-id="e8b9b-178">Publicera om appen.</span><span class="sxs-lookup"><span data-stu-id="e8b9b-178">Republish your app.</span></span>
3. <span data-ttu-id="e8b9b-179">Återaktivera övervakning med Application Insights.</span><span class="sxs-lookup"><span data-stu-id="e8b9b-179">Re-enable Application Insights monitoring.</span></span> <span data-ttu-id="e8b9b-180">(Använd lämplig metod: antingen Azure-kontrollpanelen för webbappar eller statusövervakaren på en IIS-värd.)</span><span class="sxs-lookup"><span data-stu-id="e8b9b-180">(Use the appropriate method: either the Azure web app control panel, or the Status Monitor on an IIS host.)</span></span>
4. <span data-ttu-id="e8b9b-181">Återställa redigeringar som har utförts på .config-filen.</span><span class="sxs-lookup"><span data-stu-id="e8b9b-181">Reinstate any edits you performed on the .config file.</span></span>


## <a name="troubleshooting-runtime-configuration-of-application-insights"></a><span data-ttu-id="e8b9b-182">Felsöka körningskonfigurationen av Application Insights</span><span class="sxs-lookup"><span data-stu-id="e8b9b-182">Troubleshooting runtime configuration of Application Insights</span></span>

### <a name="cant-connect-no-telemetry"></a><span data-ttu-id="e8b9b-183">Går det inte att ansluta?</span><span class="sxs-lookup"><span data-stu-id="e8b9b-183">Can't connect?</span></span> <span data-ttu-id="e8b9b-184">Ser du ingen telemetri?</span><span class="sxs-lookup"><span data-stu-id="e8b9b-184">No telemetry?</span></span>

* <span data-ttu-id="e8b9b-185">Öppna [de nödvändiga utgående portarna](app-insights-ip-addresses.md#outgoing-ports) i serverns brandvägg för att Statusövervakare ska fungera.</span><span class="sxs-lookup"><span data-stu-id="e8b9b-185">Open [the necessary outgoing ports](app-insights-ip-addresses.md#outgoing-ports) in your server's firewall to allow Status Monitor to work.</span></span>

* <span data-ttu-id="e8b9b-186">Öppna Status Monitor och välj ditt program i den vänstra rutan.</span><span class="sxs-lookup"><span data-stu-id="e8b9b-186">Open Status Monitor and select your application on left pane.</span></span> <span data-ttu-id="e8b9b-187">Kontrollera om det finns några diagnostikmeddelanden för det här programmet i avsnittet ”Konfigurationsmeddelanden”:</span><span class="sxs-lookup"><span data-stu-id="e8b9b-187">Check if there are any diagnostics messages for this application in the "Configuration notifications" section:</span></span>

  ![Öppna bladet Prestanda om du vill visa information om förfrågningar, svarstider, beroenden och andra data](./media/app-insights-monitor-performance-live-website-now/appinsights-status-monitor-diagnostics-message.png)
* <span data-ttu-id="e8b9b-189">Om du ser ett meddelande om ”otillräcklig behörighet” på servern provar du följande:</span><span class="sxs-lookup"><span data-stu-id="e8b9b-189">On the server, if you see a message about "insufficient permissions", try the following:</span></span>
  * <span data-ttu-id="e8b9b-190">I IIS-hanteraren väljer du programpoolen, öppnar **Avancerade inställningar** och noterar identiteten under **Processmodell**.</span><span class="sxs-lookup"><span data-stu-id="e8b9b-190">In IIS Manager, select your application pool, open **Advanced Settings**, and under **Process Model** note the identity.</span></span>
  * <span data-ttu-id="e8b9b-191">På kontrollpanelen för Datorhantering lägger du till den här identiteten i gruppen Användare av prestandaövervakning.</span><span class="sxs-lookup"><span data-stu-id="e8b9b-191">In Computer management control panel, add this identity to the Performance Monitor Users group.</span></span>
* <span data-ttu-id="e8b9b-192">Om du har MMA/SCOM (Systems Center Operations Manager) installerat på servern kan vissa versioner vara i konflikt med varandra.</span><span class="sxs-lookup"><span data-stu-id="e8b9b-192">If you have MMA/SCOM (Systems Center Operations Manager) installed on your server, some versions can conflict.</span></span> <span data-ttu-id="e8b9b-193">Avinstallera både SCOM och Status Monitor och installera om de senaste versionerna.</span><span class="sxs-lookup"><span data-stu-id="e8b9b-193">Uninstall both SCOM and Status Monitor, and re-install the latest versions.</span></span>
* <span data-ttu-id="e8b9b-194">Mer information finns i [Felsökning][qna].</span><span class="sxs-lookup"><span data-stu-id="e8b9b-194">See [Troubleshooting][qna].</span></span>

## <a name="system-requirements"></a><span data-ttu-id="e8b9b-195">Systemkrav</span><span class="sxs-lookup"><span data-stu-id="e8b9b-195">System Requirements</span></span>
<span data-ttu-id="e8b9b-196">Operativsystemstöd för Application Insights Status Monitor på servern:</span><span class="sxs-lookup"><span data-stu-id="e8b9b-196">OS support for Application Insights Status Monitor on Server:</span></span>

* <span data-ttu-id="e8b9b-197">Windows Server 2008</span><span class="sxs-lookup"><span data-stu-id="e8b9b-197">Windows Server 2008</span></span>
* <span data-ttu-id="e8b9b-198">Windows Server 2008 R2</span><span class="sxs-lookup"><span data-stu-id="e8b9b-198">Windows Server 2008 R2</span></span>
* <span data-ttu-id="e8b9b-199">Windows Server 2012</span><span class="sxs-lookup"><span data-stu-id="e8b9b-199">Windows Server 2012</span></span>
* <span data-ttu-id="e8b9b-200">Windows server 2012 R2</span><span class="sxs-lookup"><span data-stu-id="e8b9b-200">Windows server 2012 R2</span></span>
* <span data-ttu-id="e8b9b-201">Windows Server 2016</span><span class="sxs-lookup"><span data-stu-id="e8b9b-201">Windows Server 2016</span></span>

<span data-ttu-id="e8b9b-202">med senaste SP och .NET Framework 4.5</span><span class="sxs-lookup"><span data-stu-id="e8b9b-202">with latest SP and .NET Framework 4.5</span></span>

<span data-ttu-id="e8b9b-203">Windows 7, 8, 8.1 och 10 med .NET Framework 4.5 på klientsidan</span><span class="sxs-lookup"><span data-stu-id="e8b9b-203">On the client side: Windows 7, 8, 8.1 and 10, again with .NET Framework 4.5</span></span>

<span data-ttu-id="e8b9b-204">IIS-stöd: IIS 7, 7.5, 8, 8.5 (IIS krävs)</span><span class="sxs-lookup"><span data-stu-id="e8b9b-204">IIS support is: IIS 7, 7.5, 8, 8.5 (IIS is required)</span></span>

## <a name="automation-with-powershell"></a><span data-ttu-id="e8b9b-205">Automatisering med PowerShell</span><span class="sxs-lookup"><span data-stu-id="e8b9b-205">Automation with PowerShell</span></span>
<span data-ttu-id="e8b9b-206">Du kan starta och stoppa övervakningen med hjälp av PowerShell i din IIS-server.</span><span class="sxs-lookup"><span data-stu-id="e8b9b-206">You can start and stop monitoring by using PowerShell on your IIS server.</span></span>

<span data-ttu-id="e8b9b-207">Importera först Application Insights-modulen:</span><span class="sxs-lookup"><span data-stu-id="e8b9b-207">First import the Application Insights module:</span></span>

`Import-Module 'C:\Program Files\Microsoft Application Insights\Status Monitor\PowerShell\Microsoft.Diagnostics.Agent.StatusMonitor.PowerShell.dll'`

<span data-ttu-id="e8b9b-208">Ta reda på vilka appar som övervakas:</span><span class="sxs-lookup"><span data-stu-id="e8b9b-208">Find out which apps are being monitored:</span></span>

`Get-ApplicationInsightsMonitoringStatus [-Name appName]`

* <span data-ttu-id="e8b9b-209">`-Name` (Valfritt) Namnet på en webbapp.</span><span class="sxs-lookup"><span data-stu-id="e8b9b-209">`-Name` (Optional) The name of a web app.</span></span>
* <span data-ttu-id="e8b9b-210">Visar Application Insights-övervakningsstatusen för varje webbapp (eller den namngivna appen) på den här IIS-servern.</span><span class="sxs-lookup"><span data-stu-id="e8b9b-210">Displays the Application Insights monitoring status for each web app (or the named app) in this IIS server.</span></span>
* <span data-ttu-id="e8b9b-211">Returnerar `ApplicationInsightsApplication` för varje app:</span><span class="sxs-lookup"><span data-stu-id="e8b9b-211">Returns `ApplicationInsightsApplication` for each app:</span></span>

  * <span data-ttu-id="e8b9b-212">`SdkState==EnabledAfterDeployment`: Appen övervakas, och instrumenterades vid körningstillfället, antingen av verktyget Status Monitor eller med `Start-ApplicationInsightsMonitoring`.</span><span class="sxs-lookup"><span data-stu-id="e8b9b-212">`SdkState==EnabledAfterDeployment`: App is being monitored, and was instrumented at run time, either by the Status Monitor tool, or by `Start-ApplicationInsightsMonitoring`.</span></span>
  * <span data-ttu-id="e8b9b-213">`SdkState==Disabled`: Appen har inte instrumenterats för Application Insights.</span><span class="sxs-lookup"><span data-stu-id="e8b9b-213">`SdkState==Disabled`: The app is not instrumented for Application Insights.</span></span> <span data-ttu-id="e8b9b-214">Antingen instrumenterades den aldrig eller så inaktiverades övervakning under körning av verktyget Status Monitor eller med `Stop-ApplicationInsightsMonitoring`.</span><span class="sxs-lookup"><span data-stu-id="e8b9b-214">Either it was never instrumented, or run-time monitoring was disabled with the Status Monitor tool or with `Stop-ApplicationInsightsMonitoring`.</span></span>
  * <span data-ttu-id="e8b9b-215">`SdkState==EnabledByCodeInstrumentation`: Appen instrumenterades genom att SDK lades till i källkoden.</span><span class="sxs-lookup"><span data-stu-id="e8b9b-215">`SdkState==EnabledByCodeInstrumentation`: The app was instrumented by adding the SDK to the source code.</span></span> <span data-ttu-id="e8b9b-216">Appens SDK kan inte uppdateras eller stoppas.</span><span class="sxs-lookup"><span data-stu-id="e8b9b-216">Its SDK cannot be updated or stopped.</span></span>
  * <span data-ttu-id="e8b9b-217">`SdkVersion` visar versionen som används för att övervaka den här appen.</span><span class="sxs-lookup"><span data-stu-id="e8b9b-217">`SdkVersion` shows the version in use for monitoring this app.</span></span>
  * <span data-ttu-id="e8b9b-218">`LatestAvailableSdkVersion` visar versionen som för närvarande är tillgänglig i NuGet-galleriet.</span><span class="sxs-lookup"><span data-stu-id="e8b9b-218">`LatestAvailableSdkVersion`shows the version currently available on the NuGet gallery.</span></span> <span data-ttu-id="e8b9b-219">Om du vill uppgradera appen till den här versionen använder du `Update-ApplicationInsightsMonitoring`.</span><span class="sxs-lookup"><span data-stu-id="e8b9b-219">To upgrade the app to this version, use `Update-ApplicationInsightsMonitoring`.</span></span>

`Start-ApplicationInsightsMonitoring -Name appName -InstrumentationKey 00000000-000-000-000-0000000`

* <span data-ttu-id="e8b9b-220">`-Name` Namnet på appen i IIS</span><span class="sxs-lookup"><span data-stu-id="e8b9b-220">`-Name` The name of the app in IIS</span></span>
* <span data-ttu-id="e8b9b-221">`-InstrumentationKey` Application Insights-resursens ikey (instrumenteringsnyckel) där du vill att resultatet ska visas.</span><span class="sxs-lookup"><span data-stu-id="e8b9b-221">`-InstrumentationKey` The ikey of the Application Insights resource where you want the results to be displayed.</span></span>
* <span data-ttu-id="e8b9b-222">Den här cmdleten påverkar endast appar som inte redan har instrumenterats, dvs. SdkState==NotInstrumented.</span><span class="sxs-lookup"><span data-stu-id="e8b9b-222">This cmdlet only affects apps that are not already instrumented - that is, SdkState==NotInstrumented.</span></span>

    <span data-ttu-id="e8b9b-223">Cmdleten påverkar inte en app som redan är instrumenterad.</span><span class="sxs-lookup"><span data-stu-id="e8b9b-223">The cmdlet does not affect an app that is already instrumented.</span></span> <span data-ttu-id="e8b9b-224">Det spelar ingen roll om appen hade instrumenterats vid byggtiden genom att SDK lades till i koden, eller under körningen av en tidigare användning av den här cmdleten.</span><span class="sxs-lookup"><span data-stu-id="e8b9b-224">It does not matter whether the app was instrumented at build time by adding the SDK to the code, or at run time by a previous use of this cmdlet.</span></span>

    <span data-ttu-id="e8b9b-225">Den SDK-version som används för att instrumentera appen är den version som senast laddades ned till servern.</span><span class="sxs-lookup"><span data-stu-id="e8b9b-225">The SDK version used to instrument the app is the version that was most recently downloaded to this server.</span></span>

    <span data-ttu-id="e8b9b-226">Använd Update-ApplicationInsightsVersion för att hämta den senaste versionen.</span><span class="sxs-lookup"><span data-stu-id="e8b9b-226">To download the latest version, use Update-ApplicationInsightsVersion.</span></span>
* <span data-ttu-id="e8b9b-227">Returnerar `ApplicationInsightsApplication` om åtgärden lyckades.</span><span class="sxs-lookup"><span data-stu-id="e8b9b-227">Returns `ApplicationInsightsApplication` on success.</span></span> <span data-ttu-id="e8b9b-228">Om åtgärden misslyckas loggas en spårning till stderr.</span><span class="sxs-lookup"><span data-stu-id="e8b9b-228">If it fails, it logs a trace to stderr.</span></span>

          Name                      : Default Web Site/WebApp1
          InstrumentationKey        : 00000000-0000-0000-0000-000000000000
          ProfilerState             : ApplicationInsights
          SdkState                  : EnabledAfterDeployment
          SdkVersion                : 1.2.1
          LatestAvailableSdkVersion : 1.2.3

`Stop-ApplicationInsightsMonitoring [-Name appName | -All]`

* <span data-ttu-id="e8b9b-229">`-Name` Namnet på en app i IIS</span><span class="sxs-lookup"><span data-stu-id="e8b9b-229">`-Name` The name of an app in IIS</span></span>
* <span data-ttu-id="e8b9b-230">`-All` Stoppar övervakningen av alla appar på IIS-servern där `SdkState==EnabledAfterDeployment`</span><span class="sxs-lookup"><span data-stu-id="e8b9b-230">`-All` Stops monitoring all apps in this IIS server for which `SdkState==EnabledAfterDeployment`</span></span>
* <span data-ttu-id="e8b9b-231">Stoppar övervakningen av de angivna apparna och tar bort instrumentering.</span><span class="sxs-lookup"><span data-stu-id="e8b9b-231">Stops monitoring the specified apps and removes instrumentation.</span></span> <span data-ttu-id="e8b9b-232">Det fungerar bara för appar som har instrumenterats vid körning med statusövervakningsverktyget eller Start-ApplicationInsightsApplication.</span><span class="sxs-lookup"><span data-stu-id="e8b9b-232">It only works for apps that have been instrumented at run-time using the Status Monitoring tool or Start-ApplicationInsightsApplication.</span></span> <span data-ttu-id="e8b9b-233">(`SdkState==EnabledAfterDeployment`)</span><span class="sxs-lookup"><span data-stu-id="e8b9b-233">(`SdkState==EnabledAfterDeployment`)</span></span>
* <span data-ttu-id="e8b9b-234">Returnerar ApplicationInsightsApplication.</span><span class="sxs-lookup"><span data-stu-id="e8b9b-234">Returns ApplicationInsightsApplication.</span></span>

<span data-ttu-id="e8b9b-235">`Update-ApplicationInsightsMonitoring -Name appName [-InstrumentationKey "0000000-0000-000-000-0000"`]</span><span class="sxs-lookup"><span data-stu-id="e8b9b-235">`Update-ApplicationInsightsMonitoring -Name appName [-InstrumentationKey "0000000-0000-000-000-0000"`]</span></span>

* <span data-ttu-id="e8b9b-236">`-Name`: Namnet på en webbapp i IIS.</span><span class="sxs-lookup"><span data-stu-id="e8b9b-236">`-Name`: The name of a web app in IIS.</span></span>
* <span data-ttu-id="e8b9b-237">`-InstrumentationKey` (Valfritt.) Använd det här om du vill ändra resursen som appens telemetri skickas till.</span><span class="sxs-lookup"><span data-stu-id="e8b9b-237">`-InstrumentationKey` (Optional.) Use this to change the resource to which the app's telemetry is sent.</span></span>
* <span data-ttu-id="e8b9b-238">Den här cmdleten:</span><span class="sxs-lookup"><span data-stu-id="e8b9b-238">This cmdlet:</span></span>
  * <span data-ttu-id="e8b9b-239">Uppgraderar den namngivna appen till den version av SDK som senast laddades ned till datorn.</span><span class="sxs-lookup"><span data-stu-id="e8b9b-239">Upgrades the named app to the version of the SDK most recently downloaded to this machine.</span></span> <span data-ttu-id="e8b9b-240">(Fungerar bara om `SdkState==EnabledAfterDeployment`)</span><span class="sxs-lookup"><span data-stu-id="e8b9b-240">(Only works if `SdkState==EnabledAfterDeployment`)</span></span>
  * <span data-ttu-id="e8b9b-241">Om du anger en instrumenteringsnyckel konfigureras den namngivna appen så att den skickar telemetri till resursen med den nyckeln.</span><span class="sxs-lookup"><span data-stu-id="e8b9b-241">If you provide an instrumentation key, the named app is reconfigured to send telemetry to the resource with that key.</span></span> <span data-ttu-id="e8b9b-242">(Fungerar om `SdkState != Disabled`)</span><span class="sxs-lookup"><span data-stu-id="e8b9b-242">(Works if `SdkState != Disabled`)</span></span>

`Update-ApplicationInsightsVersion`

* <span data-ttu-id="e8b9b-243">Laddar ned senaste Application Insights SDK till servern.</span><span class="sxs-lookup"><span data-stu-id="e8b9b-243">Downloads the latest Application Insights SDK to the server.</span></span>

## <span data-ttu-id="e8b9b-244"><a name="questions"></a>Frågor om Statusövervakaren</span><span class="sxs-lookup"><span data-stu-id="e8b9b-244"><a name="questions"></a>Questions about Status Monitor</span></span>

### <a name="what-is-status-monitor"></a><span data-ttu-id="e8b9b-245">Vad är Statusövervakaren?</span><span class="sxs-lookup"><span data-stu-id="e8b9b-245">What is Status Monitor?</span></span>

<span data-ttu-id="e8b9b-246">Ett program som du installerar på din IIS-webbserver.</span><span class="sxs-lookup"><span data-stu-id="e8b9b-246">A desktop application that you install in your IIS web server.</span></span> <span data-ttu-id="e8b9b-247">Det hjälper dig instrumentera och konfigurera webbappar.</span><span class="sxs-lookup"><span data-stu-id="e8b9b-247">It helps you instrument and configure web apps.</span></span> 

### <a name="when-do-i-use-status-monitor"></a><span data-ttu-id="e8b9b-248">När ska jag använda Statusövervakaren?</span><span class="sxs-lookup"><span data-stu-id="e8b9b-248">When do I use Status Monitor?</span></span>

* <span data-ttu-id="e8b9b-249">När du instrumenterar en webbapp som körs på IIS-servern, även om den redan har startats.</span><span class="sxs-lookup"><span data-stu-id="e8b9b-249">To instrument any web app that is running on your IIS server - even if it is already running.</span></span>
* <span data-ttu-id="e8b9b-250">För att få ytterligare telemetri för webbappar som har [skapats med Application Insights SDK](app-insights-asp-net.md) vid kompilering.</span><span class="sxs-lookup"><span data-stu-id="e8b9b-250">To enable additional telemetry for web apps that have been [built with the Application Insights SDK](app-insights-asp-net.md) at compile time.</span></span> 

### <a name="can-i-close-it-after-it-runs"></a><span data-ttu-id="e8b9b-251">Kan jag stänga den när den körs?</span><span class="sxs-lookup"><span data-stu-id="e8b9b-251">Can I close it after it runs?</span></span>

<span data-ttu-id="e8b9b-252">Ja.</span><span class="sxs-lookup"><span data-stu-id="e8b9b-252">Yes.</span></span> <span data-ttu-id="e8b9b-253">Du kan stänga den efter att den har instrumenterat de webbplatser som du har valt.</span><span class="sxs-lookup"><span data-stu-id="e8b9b-253">After it has instrumented the websites you select, you can close it.</span></span>

<span data-ttu-id="e8b9b-254">Den samlar inte in telemetri på egen hand.</span><span class="sxs-lookup"><span data-stu-id="e8b9b-254">It doesn't collect telemetry by itself.</span></span> <span data-ttu-id="e8b9b-255">Den bara konfigurerar webbappar och anger vissa behörigheter.</span><span class="sxs-lookup"><span data-stu-id="e8b9b-255">It just configures the web apps and sets some permissions.</span></span>

### <a name="what-does-status-monitor-do"></a><span data-ttu-id="e8b9b-256">Vad gör Statusövervakaren?</span><span class="sxs-lookup"><span data-stu-id="e8b9b-256">What does Status Monitor do?</span></span>

<span data-ttu-id="e8b9b-257">När du har valt en webbapp som Statusövervakaren ska instrumentera gör den följande:</span><span class="sxs-lookup"><span data-stu-id="e8b9b-257">When you select a web app for Status Monitor to instrument:</span></span>

* <span data-ttu-id="e8b9b-258">Laddar ned och placerar Application Insights-sammansättningar och .config-filen i webbappens binärfilsmapp.</span><span class="sxs-lookup"><span data-stu-id="e8b9b-258">Downloads and places the Application Insights assemblies and .config file in the web app's binaries folder.</span></span>
* <span data-ttu-id="e8b9b-259">Ändrar `web.config` för att lägga till Application Insights HTTP-spårningsmodulen.</span><span class="sxs-lookup"><span data-stu-id="e8b9b-259">Modifies `web.config` to add the Application Insights HTTP tracking module.</span></span>
* <span data-ttu-id="e8b9b-260">Aktiverar CLR-profilering för att samla in beroendeanrop.</span><span class="sxs-lookup"><span data-stu-id="e8b9b-260">Enables CLR profiling to collect dependency calls.</span></span>

### <a name="do-i-need-to-run-status-monitor-whenever-i-update-the-app"></a><span data-ttu-id="e8b9b-261">Måste jag köra Statusövervakaren när jag uppdaterar appen?</span><span class="sxs-lookup"><span data-stu-id="e8b9b-261">Do I need to run Status Monitor whenever I update the app?</span></span>

<span data-ttu-id="e8b9b-262">Inte om du distribuerar om inkrementellt.</span><span class="sxs-lookup"><span data-stu-id="e8b9b-262">Not if you redeploy incrementally.</span></span> 

<span data-ttu-id="e8b9b-263">Om du väljer alternativet Ta bort befintliga filer i publiceringen måste du köra om Statusövervakaren för att konfigurera Application Insights.</span><span class="sxs-lookup"><span data-stu-id="e8b9b-263">If you select the 'delete existing files' option in the publish process, you would need to re-run Status Monitor to configure Application Insights.</span></span>

### <a name="what-telemetry-is-collected"></a><span data-ttu-id="e8b9b-264">Vilken telemetri samlas in?</span><span class="sxs-lookup"><span data-stu-id="e8b9b-264">What telemetry is collected?</span></span>

<span data-ttu-id="e8b9b-265">För program som du endast instrumenterar vid körning med Statusövervakaren:</span><span class="sxs-lookup"><span data-stu-id="e8b9b-265">For applications that you instrument only at run-time by using Status Monitor:</span></span>

* <span data-ttu-id="e8b9b-266">HTTP-begäranden</span><span class="sxs-lookup"><span data-stu-id="e8b9b-266">HTTP requests</span></span>
* <span data-ttu-id="e8b9b-267">Anrop till beroenden</span><span class="sxs-lookup"><span data-stu-id="e8b9b-267">Calls to dependencies</span></span>
* <span data-ttu-id="e8b9b-268">Undantag</span><span class="sxs-lookup"><span data-stu-id="e8b9b-268">Exceptions</span></span>
* <span data-ttu-id="e8b9b-269">Prestandaräknare</span><span class="sxs-lookup"><span data-stu-id="e8b9b-269">Performance counters</span></span>

<span data-ttu-id="e8b9b-270">För program som redan har instrumenterats vid kompilering:</span><span class="sxs-lookup"><span data-stu-id="e8b9b-270">For applications already instrumented at compile time:</span></span>

 * <span data-ttu-id="e8b9b-271">Processräknare.</span><span class="sxs-lookup"><span data-stu-id="e8b9b-271">Process counters.</span></span>
 * <span data-ttu-id="e8b9b-272">Beroendeanrop (.NET 4.5;), returvärden i beroendeanrop (.NET 4.6).</span><span class="sxs-lookup"><span data-stu-id="e8b9b-272">Dependency calls (.NET 4.5); return values in dependency calls (.NET 4.6).</span></span>
 * <span data-ttu-id="e8b9b-273">Undantag i stackspårningsvärden.</span><span class="sxs-lookup"><span data-stu-id="e8b9b-273">Exception stack trace values.</span></span>

[<span data-ttu-id="e8b9b-274">Läs mer</span><span class="sxs-lookup"><span data-stu-id="e8b9b-274">Learn more</span></span>](http://apmtips.com/blog/2016/11/18/how-application-insights-status-monitor-not-monitors-dependencies/)

## <a name="video"></a><span data-ttu-id="e8b9b-275">Video</span><span class="sxs-lookup"><span data-stu-id="e8b9b-275">Video</span></span>

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/100/player]

## <span data-ttu-id="e8b9b-276"><a name="next"></a>Nästa steg</span><span class="sxs-lookup"><span data-stu-id="e8b9b-276"><a name="next"></a>Next steps</span></span>

<span data-ttu-id="e8b9b-277">Visa telemetrin:</span><span class="sxs-lookup"><span data-stu-id="e8b9b-277">View your telemetry:</span></span>

* <span data-ttu-id="e8b9b-278">[Utforska mått](app-insights-metrics-explorer.md) för att övervaka prestanda och användning</span><span class="sxs-lookup"><span data-stu-id="e8b9b-278">[Explore metrics](app-insights-metrics-explorer.md) to monitor performance and usage</span></span>
* <span data-ttu-id="e8b9b-279">[Sök efter händelser och loggar][diagnostic] för att diagnostisera problem</span><span class="sxs-lookup"><span data-stu-id="e8b9b-279">[Search events and logs][diagnostic] to diagnose problems</span></span>
* <span data-ttu-id="e8b9b-280">[Analys](app-insights-analytics.md) för mer avancerade frågor</span><span class="sxs-lookup"><span data-stu-id="e8b9b-280">[Analytics](app-insights-analytics.md) for more advanced queries</span></span>
* [<span data-ttu-id="e8b9b-281">Skapa instrumentpaneler</span><span class="sxs-lookup"><span data-stu-id="e8b9b-281">Create dashboards</span></span>](app-insights-dashboards.md)

<span data-ttu-id="e8b9b-282">Lägg till mer telemetri:</span><span class="sxs-lookup"><span data-stu-id="e8b9b-282">Add more telemetry:</span></span>

* <span data-ttu-id="e8b9b-283">[Skapa webbtester][availability] så att du är säker på att webbplatsen är aktiv.</span><span class="sxs-lookup"><span data-stu-id="e8b9b-283">[Create web tests][availability] to make sure your site stays live.</span></span>
* <span data-ttu-id="e8b9b-284">[Lägg till telemetri för webbklienten][usage] om du vill visa undantag från webbsidans kod och lägga till spårningsanrop.</span><span class="sxs-lookup"><span data-stu-id="e8b9b-284">[Add web client telemetry][usage] to see exceptions from web page code and to let you insert trace calls.</span></span>
* <span data-ttu-id="e8b9b-285">[Lägg till Application Insights SDK i koden][greenbrown] om du vill lägga till spårnings- och logganrop</span><span class="sxs-lookup"><span data-stu-id="e8b9b-285">[Add Application Insights SDK to your code][greenbrown] so that you can insert trace and log calls</span></span>

<!--Link references-->

[api]: app-insights-api-custom-events-metrics.md
[availability]: app-insights-monitor-web-app-availability.md
[client]: app-insights-javascript.md
[diagnostic]: app-insights-diagnostic-search.md
[greenbrown]: app-insights-asp-net.md
[qna]: app-insights-troubleshoot-faq.md
[roles]: app-insights-resources-roles-access-control.md
[usage]: app-insights-javascript.md
