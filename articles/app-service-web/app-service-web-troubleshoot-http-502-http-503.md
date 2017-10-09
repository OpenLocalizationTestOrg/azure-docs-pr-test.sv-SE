---
title: "aaaFix 502 Felaktig gateway, 503-tjänsten inte är tillgänglig | Microsoft Docs"
description: "Felsöka 502 Felaktig gateway och 503 tjänsten tillgänglig fel i webbappen i Azure App Service."
services: app-service\web
documentationcenter: 
author: cephalin
manager: erikre
editor: 
tags: top-support-issue
keywords: "502 Felaktig gateway 503-tjänsten tillgänglig, fel 503, fel 502"
ms.assetid: 51cd331a-a3fa-438f-90ef-385e755e50d5
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/06/2016
ms.author: cephalin
ms.openlocfilehash: d9d8dcddaac930967a2e8d2bfd8cad09e6824c17
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-http-errors-of-502-bad-gateway-and-503-service-unavailable-in-your-azure-web-apps"></a><span data-ttu-id="de5be-104">Felsök HTTP-fel ”502 Felaktig gateway” och ”503 inte tillgänglig” i Azure-webbappar</span><span class="sxs-lookup"><span data-stu-id="de5be-104">Troubleshoot HTTP errors of "502 bad gateway" and "503 service unavailable" in your Azure web apps</span></span>
<span data-ttu-id="de5be-105">”502 Felaktig gateway” och ”503 inte tillgänglig” är vanliga fel i ditt webbprogram som finns i [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714).</span><span class="sxs-lookup"><span data-stu-id="de5be-105">"502 bad gateway" and "503 service unavailable" are common errors in your web app hosted in [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714).</span></span> <span data-ttu-id="de5be-106">Den här artikeln hjälper dig att felsöka felen.</span><span class="sxs-lookup"><span data-stu-id="de5be-106">This article helps you troubleshoot these errors.</span></span>

<span data-ttu-id="de5be-107">Om du behöver mer hjälp när som helst i den här artikeln kan du kontakta hello Azure experter på [hello MSDN Azure och hello Stack Overflow-forum](https://azure.microsoft.com/support/forums/).</span><span class="sxs-lookup"><span data-stu-id="de5be-107">If you need more help at any point in this article, you can contact hello Azure experts on [hello MSDN Azure and hello Stack Overflow forums](https://azure.microsoft.com/support/forums/).</span></span> <span data-ttu-id="de5be-108">Alternativt kan du även filen en incident i Azure-supporten.</span><span class="sxs-lookup"><span data-stu-id="de5be-108">Alternatively, you can also file an Azure support incident.</span></span> <span data-ttu-id="de5be-109">Gå toohello [Azure supportwebbplats](https://azure.microsoft.com/support/options/) och klicka på **Get Support**.</span><span class="sxs-lookup"><span data-stu-id="de5be-109">Go toohello [Azure Support site](https://azure.microsoft.com/support/options/) and click on **Get Support**.</span></span>

## <a name="symptom"></a><span data-ttu-id="de5be-110">Symtom</span><span class="sxs-lookup"><span data-stu-id="de5be-110">Symptom</span></span>
<span data-ttu-id="de5be-111">När du bläddrar toohello webbappen returnerar ett HTTP ”502 felaktig Gateway” fel eller en HTTP-fel ”503 inte tillgänglig”.</span><span class="sxs-lookup"><span data-stu-id="de5be-111">When you browse toohello web app, it returns a HTTP "502 Bad Gateway" error or a HTTP "503 Service Unavailable" error.</span></span>

## <a name="cause"></a><span data-ttu-id="de5be-112">Orsak</span><span class="sxs-lookup"><span data-stu-id="de5be-112">Cause</span></span>
<span data-ttu-id="de5be-113">Det här problemet beror ofta på nivån programproblem som:</span><span class="sxs-lookup"><span data-stu-id="de5be-113">This problem is often caused by application level issues, such as:</span></span>

* <span data-ttu-id="de5be-114">begäranden som tar lång tid</span><span class="sxs-lookup"><span data-stu-id="de5be-114">requests taking a long time</span></span>
* <span data-ttu-id="de5be-115">program med hög minne/processor</span><span class="sxs-lookup"><span data-stu-id="de5be-115">application using high memory/CPU</span></span>
* <span data-ttu-id="de5be-116">programmet kraschar på grund av tooan undantag.</span><span class="sxs-lookup"><span data-stu-id="de5be-116">application crashing due tooan exception.</span></span>

## <a name="troubleshooting-steps-toosolve-502-bad-gateway-and-503-service-unavailable-errors"></a><span data-ttu-id="de5be-117">Felsökning av steg toosolve ”502 Felaktig gateway” och ”503 inte tillgänglig” fel</span><span class="sxs-lookup"><span data-stu-id="de5be-117">Troubleshooting steps toosolve "502 bad gateway" and "503 service unavailable" errors</span></span>
<span data-ttu-id="de5be-118">Felsökning kan delas in i tre olika uppgifter i sekventiell ordning:</span><span class="sxs-lookup"><span data-stu-id="de5be-118">Troubleshooting can be divided into three distinct tasks, in sequential order:</span></span>

1. [<span data-ttu-id="de5be-119">Observera och övervaka programmets beteende</span><span class="sxs-lookup"><span data-stu-id="de5be-119">Observe and monitor application behavior</span></span>](#observe)
2. [<span data-ttu-id="de5be-120">Samla in data</span><span class="sxs-lookup"><span data-stu-id="de5be-120">Collect data</span></span>](#collect)
3. [<span data-ttu-id="de5be-121">Minimera hello problemet</span><span class="sxs-lookup"><span data-stu-id="de5be-121">Mitigate hello issue</span></span>](#mitigate)

<span data-ttu-id="de5be-122">[App Service Web Apps](/services/app-service/web/) ger olika alternativ i varje steg.</span><span class="sxs-lookup"><span data-stu-id="de5be-122">[App Service Web Apps](/services/app-service/web/) gives you various options at each step.</span></span>

<a name="observe" />

### <a name="1-observe-and-monitor-application-behavior"></a><span data-ttu-id="de5be-123">1. Observera och övervaka programmets beteende</span><span class="sxs-lookup"><span data-stu-id="de5be-123">1. Observe and monitor application behavior</span></span>
#### <a name="track-service-health"></a><span data-ttu-id="de5be-124">Spåra tjänstens hälsa</span><span class="sxs-lookup"><span data-stu-id="de5be-124">Track Service health</span></span>
<span data-ttu-id="de5be-125">Microsoft Azure publicizes varje gång en tjänst avbrott eller prestanda försämras.</span><span class="sxs-lookup"><span data-stu-id="de5be-125">Microsoft Azure publicizes each time there is a service interruption or performance degradation.</span></span> <span data-ttu-id="de5be-126">Du kan spåra hello hälsotillståndet för hello-tjänsten på hello [Azure Portal](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="de5be-126">You can track hello health of hello service on hello [Azure Portal](https://portal.azure.com/).</span></span> <span data-ttu-id="de5be-127">Mer information finns i [spåra tjänstens hälsa](../monitoring-and-diagnostics/insights-service-health.md).</span><span class="sxs-lookup"><span data-stu-id="de5be-127">For more information, see [Track service health](../monitoring-and-diagnostics/insights-service-health.md).</span></span>

#### <a name="monitor-your-web-app"></a><span data-ttu-id="de5be-128">Övervaka ditt webbprogram</span><span class="sxs-lookup"><span data-stu-id="de5be-128">Monitor your web app</span></span>
<span data-ttu-id="de5be-129">Det här alternativet kan du toofind ut om programmet har med eventuella problem.</span><span class="sxs-lookup"><span data-stu-id="de5be-129">This option enables you toofind out if your application is having any issues.</span></span> <span data-ttu-id="de5be-130">I din webbapps blad klickar du på hello **begäranden och fel** panelen.</span><span class="sxs-lookup"><span data-stu-id="de5be-130">In your web app’s blade, click hello **Requests and errors** tile.</span></span> <span data-ttu-id="de5be-131">Hej **mått** bladet visar alla hello mått som du kan lägga till.</span><span class="sxs-lookup"><span data-stu-id="de5be-131">hello **Metric** blade will show you all hello metrics you can add.</span></span>

<span data-ttu-id="de5be-132">Några av hello mått som du kanske vill toomonitor för webbappen</span><span class="sxs-lookup"><span data-stu-id="de5be-132">Some of hello metrics that you might want toomonitor for your web app are</span></span>

* <span data-ttu-id="de5be-133">Genomsnittlig minne arbetsminne</span><span class="sxs-lookup"><span data-stu-id="de5be-133">Average memory working set</span></span>
* <span data-ttu-id="de5be-134">Genomsnittlig svarstid</span><span class="sxs-lookup"><span data-stu-id="de5be-134">Average response time</span></span>
* <span data-ttu-id="de5be-135">CPU-tid</span><span class="sxs-lookup"><span data-stu-id="de5be-135">CPU time</span></span>
* <span data-ttu-id="de5be-136">Arbetsminnet för minne</span><span class="sxs-lookup"><span data-stu-id="de5be-136">Memory working set</span></span>
* <span data-ttu-id="de5be-137">Begäranden</span><span class="sxs-lookup"><span data-stu-id="de5be-137">Requests</span></span>

![övervaka webbprogram ska lösa HTTP-fel 502 Felaktig gateway och 503 inte tillgänglig](./media/app-service-web-troubleshoot-HTTP-502-503/1-monitor-metrics.png)

<span data-ttu-id="de5be-139">Mer information finns i:</span><span class="sxs-lookup"><span data-stu-id="de5be-139">For more information, see:</span></span>

* [<span data-ttu-id="de5be-140">Övervakaren Web Apps i Azure App Service</span><span class="sxs-lookup"><span data-stu-id="de5be-140">Monitor Web Apps in Azure App Service</span></span>](web-sites-monitor.md)
* [<span data-ttu-id="de5be-141">Få varningsmeddelanden</span><span class="sxs-lookup"><span data-stu-id="de5be-141">Receive alert notifications</span></span>](../monitoring-and-diagnostics/insights-receive-alert-notifications.md)

<a name="collect" />

### <a name="2-collect-data"></a><span data-ttu-id="de5be-142">2. Samla in data</span><span class="sxs-lookup"><span data-stu-id="de5be-142">2. Collect data</span></span>
#### <a name="use-hello-azure-app-service-support-portal"></a><span data-ttu-id="de5be-143">Använd hello Azure App Service stöd Portal</span><span class="sxs-lookup"><span data-stu-id="de5be-143">Use hello Azure App Service Support Portal</span></span>
<span data-ttu-id="de5be-144">Web Apps ger hello möjlighet tootroubleshoot problem relaterade tooyour webbprogram genom att titta på http-loggar, händelseloggar, process Dumpar och mycket mer.</span><span class="sxs-lookup"><span data-stu-id="de5be-144">Web Apps provides you with hello ability tootroubleshoot issues related tooyour web app by looking at HTTP logs, event logs, process dumps, and more.</span></span> <span data-ttu-id="de5be-145">Du kan komma åt den här informationen med hjälp av vår portal Support vid **http://&lt;appens namn >.scm.azurewebsites.net/Support**</span><span class="sxs-lookup"><span data-stu-id="de5be-145">You can access all this information using our Support portal at **http://&lt;your app name>.scm.azurewebsites.net/Support**</span></span>

<span data-ttu-id="de5be-146">hello stöd för Azure App Service portal innehåller tre separata flikar toosupport hello tre steg i ett vanligt scenario för felsökning:</span><span class="sxs-lookup"><span data-stu-id="de5be-146">hello Azure App Service Support portal provides you with three separate tabs toosupport hello three steps of a common troubleshooting scenario:</span></span>

1. <span data-ttu-id="de5be-147">Se aktuella beteende</span><span class="sxs-lookup"><span data-stu-id="de5be-147">Observe current behavior</span></span>
2. <span data-ttu-id="de5be-148">Analysera genom att samla in diagnostikinformation och köra hello inbyggda analyzers</span><span class="sxs-lookup"><span data-stu-id="de5be-148">Analyze by collecting diagnostics information and running hello built-in analyzers</span></span>
3. <span data-ttu-id="de5be-149">Minimera</span><span class="sxs-lookup"><span data-stu-id="de5be-149">Mitigate</span></span>

<span data-ttu-id="de5be-150">Om hello problemet sker just nu, klickar du på **analysera** > **diagnostik** > **diagnostisera nu** toocreate en diagnostiska session, som samlar in HTTP loggar, loggar i Loggboken, minne Dumpar, PHP felloggarna och PHP bearbeta rapporten.</span><span class="sxs-lookup"><span data-stu-id="de5be-150">If hello issue is happening right now, click **Analyze** > **Diagnostics** > **Diagnose Now** toocreate a diagnostic session for you, which will collect HTTP logs, event viewer logs, memory dumps, PHP error logs and PHP process report.</span></span>

<span data-ttu-id="de5be-151">När hello data samlas in, kommer den även kör en analys på hello data och ger dig en HTML-rapport.</span><span class="sxs-lookup"><span data-stu-id="de5be-151">Once hello data is collected, it will also run an analysis on hello data and provide you with an HTML report.</span></span>

<span data-ttu-id="de5be-152">Om du vill toodownload hello data, som standard lagras den i hello D:\home\data\DaaS mapp.</span><span class="sxs-lookup"><span data-stu-id="de5be-152">In case you want toodownload hello data, by default, it would be stored in hello D:\home\data\DaaS folder.</span></span>

<span data-ttu-id="de5be-153">Mer information om hello stöd för Azure App Service portal finns [nya uppdateringar tooSupport plats tillägget för Azure Websites](https://azure.microsoft.com/blog/new-updates-to-support-site-extension-for-azure-websites).</span><span class="sxs-lookup"><span data-stu-id="de5be-153">For more information on hello Azure App Service Support portal, see [New Updates tooSupport Site Extension for Azure Websites](https://azure.microsoft.com/blog/new-updates-to-support-site-extension-for-azure-websites).</span></span>

#### <a name="use-hello-kudu-debug-console"></a><span data-ttu-id="de5be-154">Använd hello Kudu-Felsökningskonsolen</span><span class="sxs-lookup"><span data-stu-id="de5be-154">Use hello Kudu Debug Console</span></span>
<span data-ttu-id="de5be-155">Web Apps levereras med en Felsökningskonsolen som du kan använda för felsökning, utforska, ladda upp filer, samt JSON-slutpunkter för att hämta information om din miljö.</span><span class="sxs-lookup"><span data-stu-id="de5be-155">Web Apps comes with a debug console that you can use for debugging, exploring, uploading files, as well as JSON endpoints for getting information about your environment.</span></span> <span data-ttu-id="de5be-156">Detta kallas hello *Kudu konsolen* eller hello *SCM instrumentpanelen* för ditt webbprogram.</span><span class="sxs-lookup"><span data-stu-id="de5be-156">This is called hello *Kudu Console* or hello *SCM Dashboard* for your web app.</span></span>

<span data-ttu-id="de5be-157">Du kan komma åt den här instrumentpanelen genom att gå toohello länk **https://&lt;appens namn >.scm.azurewebsites.net/**.</span><span class="sxs-lookup"><span data-stu-id="de5be-157">You can access this dashboard by going toohello link **https://&lt;Your app name>.scm.azurewebsites.net/**.</span></span>

<span data-ttu-id="de5be-158">Några av hello saker som Kudu tillhandahåller är:</span><span class="sxs-lookup"><span data-stu-id="de5be-158">Some of hello things that Kudu provides are:</span></span>

* <span data-ttu-id="de5be-159">inställningar för ditt program</span><span class="sxs-lookup"><span data-stu-id="de5be-159">environment settings for your application</span></span>
* <span data-ttu-id="de5be-160">loggström</span><span class="sxs-lookup"><span data-stu-id="de5be-160">log stream</span></span>
* <span data-ttu-id="de5be-161">diagnostiska dump</span><span class="sxs-lookup"><span data-stu-id="de5be-161">diagnostic dump</span></span>
* <span data-ttu-id="de5be-162">Felsöka konsolen där du kan köra Powershell-cmdlets och grundläggande DOS-kommandon.</span><span class="sxs-lookup"><span data-stu-id="de5be-162">debug console in which you can run Powershell cmdlets and basic DOS commands.</span></span>

<span data-ttu-id="de5be-163">En annan användbar funktion i Kudu är att, om ditt program är att första chans undantag, kan du använda Kudu och hello SysInternals verktyget Procdump toocreate minnesdumpar.</span><span class="sxs-lookup"><span data-stu-id="de5be-163">Another useful feature of Kudu is that, in case your application is throwing first-chance exceptions, you can use Kudu and hello SysInternals tool Procdump toocreate memory dumps.</span></span> <span data-ttu-id="de5be-164">Dessa minnesdumpar är ögonblicksbilder av hello processen och ofta kan hjälpa dig att felsöka mer komplicerade problem med ditt webbprogram.</span><span class="sxs-lookup"><span data-stu-id="de5be-164">These memory dumps are snapshots of hello process and can often help you troubleshoot more complicated issues with your web app.</span></span>

<span data-ttu-id="de5be-165">Mer information om funktionerna i Kudu finns [Azure Websites online-verktyg som du bör känna till om](https://azure.microsoft.com/blog/windows-azure-websites-online-tools-you-should-know-about/).</span><span class="sxs-lookup"><span data-stu-id="de5be-165">For more information on features available in Kudu, see [Azure Websites online tools you should know about](https://azure.microsoft.com/blog/windows-azure-websites-online-tools-you-should-know-about/).</span></span>

<a name="mitigate" />

### <a name="3-mitigate-hello-issue"></a><span data-ttu-id="de5be-166">3. Minimera hello problemet</span><span class="sxs-lookup"><span data-stu-id="de5be-166">3. Mitigate hello issue</span></span>
#### <a name="scale-hello-web-app"></a><span data-ttu-id="de5be-167">Skala hello-webbprogram</span><span class="sxs-lookup"><span data-stu-id="de5be-167">Scale hello web app</span></span>
<span data-ttu-id="de5be-168">I Azure App Service kan för bättre prestanda och genomflöde, du justera hello skala där du kör programmet.</span><span class="sxs-lookup"><span data-stu-id="de5be-168">In Azure App Service, for increased performance and throughput,  you can adjust hello scale at which you are running your application.</span></span> <span data-ttu-id="de5be-169">Skala upp ett webbprogram omfattar två relaterade åtgärder: ändra din App Service-plan tooa högre prisnivån och konfigurera vissa inställningar när du har växlat toohello högre prisnivå.</span><span class="sxs-lookup"><span data-stu-id="de5be-169">Scaling up a web app involves two related actions: changing your App Service plan tooa higher pricing tier, and configuring certain settings after you have switched toohello higher pricing tier.</span></span>

<span data-ttu-id="de5be-170">Läs mer om att skala [skala en webbapp i Azure App Service](web-sites-scale.md).</span><span class="sxs-lookup"><span data-stu-id="de5be-170">For more information on scaling, see [Scale a web app in Azure App Service](web-sites-scale.md).</span></span>

<span data-ttu-id="de5be-171">Du kan dessutom välja toorun programmet på mer än en instans.</span><span class="sxs-lookup"><span data-stu-id="de5be-171">Additionally, you can choose toorun your application on more than one instance .</span></span> <span data-ttu-id="de5be-172">Detta inte bara ger mer bearbetningskapacitet, men ger dig även vissa delar av feltolerans.</span><span class="sxs-lookup"><span data-stu-id="de5be-172">This not only provides you with more processing capability, but also gives you some amount of fault tolerance.</span></span> <span data-ttu-id="de5be-173">Om hello processen kraschar på en instans fortsätter hello instanser fortfarande fler begäranden.</span><span class="sxs-lookup"><span data-stu-id="de5be-173">If hello process goes down on one instance, hello other instance will still continue serving requests.</span></span>

<span data-ttu-id="de5be-174">Du kan ange hello skalning toobe manuell eller automatisk.</span><span class="sxs-lookup"><span data-stu-id="de5be-174">You can set hello scaling toobe Manual or Automatic.</span></span>

#### <a name="use-autoheal"></a><span data-ttu-id="de5be-175">Använd säkert att AutoHeal</span><span class="sxs-lookup"><span data-stu-id="de5be-175">Use AutoHeal</span></span>
<span data-ttu-id="de5be-176">Säkert att AutoHeal återanvänds hello arbetsprocessen för din app baserat på dina inställningar (t.ex. konfigurationsändringar begäranden, minnesbaserade gränser eller hello tid behövs tooexecute en begäran).</span><span class="sxs-lookup"><span data-stu-id="de5be-176">AutoHeal recycles hello worker process for your app based on settings you choose (like configuration changes, requests, memory-based limits, or hello time needed tooexecute a request).</span></span> <span data-ttu-id="de5be-177">För hello mesta är återvinning hello processen hello snabbaste sättet toorecover från ett problem.</span><span class="sxs-lookup"><span data-stu-id="de5be-177">Most of hello time, recycle hello process is hello fastest way toorecover from a problem.</span></span> <span data-ttu-id="de5be-178">Även om du kan alltid starta om hello webbprogrammet från direkt i hello Azure-portalen, gör säkert att AutoHeal det automatiskt åt dig.</span><span class="sxs-lookup"><span data-stu-id="de5be-178">Though you can always restart hello web app from directly within hello Azure Portal, AutoHeal will do it automatically for you.</span></span> <span data-ttu-id="de5be-179">Allt du behöver toodo är att lägga till vissa utlösare i hello rotens web.config för ditt webbprogram.</span><span class="sxs-lookup"><span data-stu-id="de5be-179">All you need toodo is add some triggers in hello root web.config for your web app.</span></span> <span data-ttu-id="de5be-180">Observera att de här inställningarna fungerar i hello samma sätt även om programmet inte är en .net en.</span><span class="sxs-lookup"><span data-stu-id="de5be-180">Note that these settings would work in hello same way even if your application is not a .Net one.</span></span>

<span data-ttu-id="de5be-181">Mer information finns i [automatisk återställning Azure webbplatser](https://azure.microsoft.com/blog/auto-healing-windows-azure-web-sites/).</span><span class="sxs-lookup"><span data-stu-id="de5be-181">For more information, see [Auto-Healing Azure Web Sites](https://azure.microsoft.com/blog/auto-healing-windows-azure-web-sites/).</span></span>

#### <a name="restart-hello-web-app"></a><span data-ttu-id="de5be-182">Starta om hello-webbprogram</span><span class="sxs-lookup"><span data-stu-id="de5be-182">Restart hello web app</span></span>
<span data-ttu-id="de5be-183">Det här är ofta hello enklaste sättet toorecover engångsproblem.</span><span class="sxs-lookup"><span data-stu-id="de5be-183">This is often hello simplest way toorecover from one-time issues.</span></span> <span data-ttu-id="de5be-184">På hello [Azure Portal](https://portal.azure.com/), på bladet för ditt webbprogram har hello alternativ toostop eller starta om din app.</span><span class="sxs-lookup"><span data-stu-id="de5be-184">On hello [Azure Portal](https://portal.azure.com/), on your web app’s blade, you have hello options toostop or restart your app.</span></span>

 ![Starta om appen toosolve HTTP-fel 502 Felaktig gateway och 503 inte tillgänglig](./media/app-service-web-troubleshoot-HTTP-502-503/2-restart.png)

<span data-ttu-id="de5be-186">Du kan också hantera ditt webbprogram med hjälp av Azure Powershell.</span><span class="sxs-lookup"><span data-stu-id="de5be-186">You can also manage your web app using Azure Powershell.</span></span> <span data-ttu-id="de5be-187">Mer information finns i [Använda Azure PowerShell med Azure Resource Manager](../powershell-azure-resource-manager.md).</span><span class="sxs-lookup"><span data-stu-id="de5be-187">For more information, see [Using Azure PowerShell with Azure Resource Manager](../powershell-azure-resource-manager.md).</span></span>

