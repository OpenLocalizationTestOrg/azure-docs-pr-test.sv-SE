---
title: "Åtgärda 502 Felaktig gateway, 503-tjänsten inte är tillgänglig | Microsoft Docs"
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
ms.openlocfilehash: 397a6aaf7dc27adfa0fc0e722b8a2be5cc1d75f0
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="troubleshoot-http-errors-of-502-bad-gateway-and-503-service-unavailable-in-your-azure-web-apps"></a><span data-ttu-id="8e8d2-104">Felsök HTTP-fel ”502 Felaktig gateway” och ”503 inte tillgänglig” i Azure-webbappar</span><span class="sxs-lookup"><span data-stu-id="8e8d2-104">Troubleshoot HTTP errors of "502 bad gateway" and "503 service unavailable" in your Azure web apps</span></span>
<span data-ttu-id="8e8d2-105">”502 Felaktig gateway” och ”503 inte tillgänglig” är vanliga fel i ditt webbprogram som finns i [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714).</span><span class="sxs-lookup"><span data-stu-id="8e8d2-105">"502 bad gateway" and "503 service unavailable" are common errors in your web app hosted in [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714).</span></span> <span data-ttu-id="8e8d2-106">Den här artikeln hjälper dig att felsöka felen.</span><span class="sxs-lookup"><span data-stu-id="8e8d2-106">This article helps you troubleshoot these errors.</span></span>

<span data-ttu-id="8e8d2-107">Om du behöver mer hjälp när som helst i den här artikeln kan du kontakta Azure-experter på [MSDN Azure och Stack Overflow-forum](https://azure.microsoft.com/support/forums/).</span><span class="sxs-lookup"><span data-stu-id="8e8d2-107">If you need more help at any point in this article, you can contact the Azure experts on [the MSDN Azure and the Stack Overflow forums](https://azure.microsoft.com/support/forums/).</span></span> <span data-ttu-id="8e8d2-108">Alternativt kan du även filen en incident i Azure-supporten.</span><span class="sxs-lookup"><span data-stu-id="8e8d2-108">Alternatively, you can also file an Azure support incident.</span></span> <span data-ttu-id="8e8d2-109">Gå till den [Azure supportwebbplats](https://azure.microsoft.com/support/options/) och klicka på **Get Support**.</span><span class="sxs-lookup"><span data-stu-id="8e8d2-109">Go to the [Azure Support site](https://azure.microsoft.com/support/options/) and click on **Get Support**.</span></span>

## <a name="symptom"></a><span data-ttu-id="8e8d2-110">Symtom</span><span class="sxs-lookup"><span data-stu-id="8e8d2-110">Symptom</span></span>
<span data-ttu-id="8e8d2-111">När du bläddrar till webbappen returnerar ett HTTP ”502 felaktig Gateway” fel eller en HTTP-fel ”503 inte tillgänglig”.</span><span class="sxs-lookup"><span data-stu-id="8e8d2-111">When you browse to the web app, it returns a HTTP "502 Bad Gateway" error or a HTTP "503 Service Unavailable" error.</span></span>

## <a name="cause"></a><span data-ttu-id="8e8d2-112">Orsak</span><span class="sxs-lookup"><span data-stu-id="8e8d2-112">Cause</span></span>
<span data-ttu-id="8e8d2-113">Det här problemet beror ofta på nivån programproblem som:</span><span class="sxs-lookup"><span data-stu-id="8e8d2-113">This problem is often caused by application level issues, such as:</span></span>

* <span data-ttu-id="8e8d2-114">begäranden som tar lång tid</span><span class="sxs-lookup"><span data-stu-id="8e8d2-114">requests taking a long time</span></span>
* <span data-ttu-id="8e8d2-115">program med hög minne/processor</span><span class="sxs-lookup"><span data-stu-id="8e8d2-115">application using high memory/CPU</span></span>
* <span data-ttu-id="8e8d2-116">programmet kraschar på grund av ett undantag.</span><span class="sxs-lookup"><span data-stu-id="8e8d2-116">application crashing due to an exception.</span></span>

## <a name="troubleshooting-steps-to-solve-502-bad-gateway-and-503-service-unavailable-errors"></a><span data-ttu-id="8e8d2-117">Felsökningssteg för att lösa ”502 Felaktig gateway” och ”503 inte tillgänglig” fel</span><span class="sxs-lookup"><span data-stu-id="8e8d2-117">Troubleshooting steps to solve "502 bad gateway" and "503 service unavailable" errors</span></span>
<span data-ttu-id="8e8d2-118">Felsökning kan delas in i tre olika uppgifter i sekventiell ordning:</span><span class="sxs-lookup"><span data-stu-id="8e8d2-118">Troubleshooting can be divided into three distinct tasks, in sequential order:</span></span>

1. [<span data-ttu-id="8e8d2-119">Observera och övervaka programmets beteende</span><span class="sxs-lookup"><span data-stu-id="8e8d2-119">Observe and monitor application behavior</span></span>](#observe)
2. [<span data-ttu-id="8e8d2-120">Samla in data</span><span class="sxs-lookup"><span data-stu-id="8e8d2-120">Collect data</span></span>](#collect)
3. [<span data-ttu-id="8e8d2-121">Åtgärda problemet</span><span class="sxs-lookup"><span data-stu-id="8e8d2-121">Mitigate the issue</span></span>](#mitigate)

<span data-ttu-id="8e8d2-122">[App Service Web Apps](/services/app-service/web/) ger olika alternativ i varje steg.</span><span class="sxs-lookup"><span data-stu-id="8e8d2-122">[App Service Web Apps](/services/app-service/web/) gives you various options at each step.</span></span>

<a name="observe" />

### <a name="1-observe-and-monitor-application-behavior"></a><span data-ttu-id="8e8d2-123">1. Observera och övervaka programmets beteende</span><span class="sxs-lookup"><span data-stu-id="8e8d2-123">1. Observe and monitor application behavior</span></span>
#### <a name="track-service-health"></a><span data-ttu-id="8e8d2-124">Spåra tjänstens hälsa</span><span class="sxs-lookup"><span data-stu-id="8e8d2-124">Track Service health</span></span>
<span data-ttu-id="8e8d2-125">Microsoft Azure publicizes varje gång en tjänst avbrott eller prestanda försämras.</span><span class="sxs-lookup"><span data-stu-id="8e8d2-125">Microsoft Azure publicizes each time there is a service interruption or performance degradation.</span></span> <span data-ttu-id="8e8d2-126">Du kan spåra hälsotillståndet för tjänsten på den [Azure Portal](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="8e8d2-126">You can track the health of the service on the [Azure Portal](https://portal.azure.com/).</span></span> <span data-ttu-id="8e8d2-127">Mer information finns i [spåra tjänstens hälsa](../monitoring-and-diagnostics/insights-service-health.md).</span><span class="sxs-lookup"><span data-stu-id="8e8d2-127">For more information, see [Track service health](../monitoring-and-diagnostics/insights-service-health.md).</span></span>

#### <a name="monitor-your-web-app"></a><span data-ttu-id="8e8d2-128">Övervaka ditt webbprogram</span><span class="sxs-lookup"><span data-stu-id="8e8d2-128">Monitor your web app</span></span>
<span data-ttu-id="8e8d2-129">Det här alternativet kan du ta reda på om ditt program är har problem.</span><span class="sxs-lookup"><span data-stu-id="8e8d2-129">This option enables you to find out if your application is having any issues.</span></span> <span data-ttu-id="8e8d2-130">I din webbapps blad klickar du på den **begäranden och fel** panelen.</span><span class="sxs-lookup"><span data-stu-id="8e8d2-130">In your web app’s blade, click the **Requests and errors** tile.</span></span> <span data-ttu-id="8e8d2-131">Den **mått** bladet visar alla mått som du kan lägga till.</span><span class="sxs-lookup"><span data-stu-id="8e8d2-131">The **Metric** blade will show you all the metrics you can add.</span></span>

<span data-ttu-id="8e8d2-132">Några av de mått som du kanske vill övervaka för webbappen</span><span class="sxs-lookup"><span data-stu-id="8e8d2-132">Some of the metrics that you might want to monitor for your web app are</span></span>

* <span data-ttu-id="8e8d2-133">Genomsnittlig minne arbetsminne</span><span class="sxs-lookup"><span data-stu-id="8e8d2-133">Average memory working set</span></span>
* <span data-ttu-id="8e8d2-134">Genomsnittlig svarstid</span><span class="sxs-lookup"><span data-stu-id="8e8d2-134">Average response time</span></span>
* <span data-ttu-id="8e8d2-135">CPU-tid</span><span class="sxs-lookup"><span data-stu-id="8e8d2-135">CPU time</span></span>
* <span data-ttu-id="8e8d2-136">Arbetsminnet för minne</span><span class="sxs-lookup"><span data-stu-id="8e8d2-136">Memory working set</span></span>
* <span data-ttu-id="8e8d2-137">Begäranden</span><span class="sxs-lookup"><span data-stu-id="8e8d2-137">Requests</span></span>

![övervaka webbprogram ska lösa HTTP-fel 502 Felaktig gateway och 503 inte tillgänglig](./media/app-service-web-troubleshoot-HTTP-502-503/1-monitor-metrics.png)

<span data-ttu-id="8e8d2-139">Mer information finns i:</span><span class="sxs-lookup"><span data-stu-id="8e8d2-139">For more information, see:</span></span>

* [<span data-ttu-id="8e8d2-140">Övervakaren Web Apps i Azure App Service</span><span class="sxs-lookup"><span data-stu-id="8e8d2-140">Monitor Web Apps in Azure App Service</span></span>](web-sites-monitor.md)
* [<span data-ttu-id="8e8d2-141">Få varningsmeddelanden</span><span class="sxs-lookup"><span data-stu-id="8e8d2-141">Receive alert notifications</span></span>](../monitoring-and-diagnostics/insights-receive-alert-notifications.md)

<a name="collect" />

### <a name="2-collect-data"></a><span data-ttu-id="8e8d2-142">2. Samla in data</span><span class="sxs-lookup"><span data-stu-id="8e8d2-142">2. Collect data</span></span>
#### <a name="use-the-azure-app-service-support-portal"></a><span data-ttu-id="8e8d2-143">Använda portalen för Azure App Service-stöd</span><span class="sxs-lookup"><span data-stu-id="8e8d2-143">Use the Azure App Service Support Portal</span></span>
<span data-ttu-id="8e8d2-144">Web Apps ger dig möjlighet att felsöka problem med ditt webbprogram genom att titta på http-loggar, händelseloggar, process Dumpar och mycket mer.</span><span class="sxs-lookup"><span data-stu-id="8e8d2-144">Web Apps provides you with the ability to troubleshoot issues related to your web app by looking at HTTP logs, event logs, process dumps, and more.</span></span> <span data-ttu-id="8e8d2-145">Du kan komma åt den här informationen med hjälp av vår portal Support vid **http://&lt;appens namn >.scm.azurewebsites.net/Support**</span><span class="sxs-lookup"><span data-stu-id="8e8d2-145">You can access all this information using our Support portal at **http://&lt;your app name>.scm.azurewebsites.net/Support**</span></span>

<span data-ttu-id="8e8d2-146">Stöd för Azure App Service-portalen innehåller tre separata flikar som stöd för ett vanligt scenario för felsökning tre steg:</span><span class="sxs-lookup"><span data-stu-id="8e8d2-146">The Azure App Service Support portal provides you with three separate tabs to support the three steps of a common troubleshooting scenario:</span></span>

1. <span data-ttu-id="8e8d2-147">Se aktuella beteende</span><span class="sxs-lookup"><span data-stu-id="8e8d2-147">Observe current behavior</span></span>
2. <span data-ttu-id="8e8d2-148">Analysera genom att samla in diagnostikinformation och köra de inbyggda analyzers</span><span class="sxs-lookup"><span data-stu-id="8e8d2-148">Analyze by collecting diagnostics information and running the built-in analyzers</span></span>
3. <span data-ttu-id="8e8d2-149">Minimera</span><span class="sxs-lookup"><span data-stu-id="8e8d2-149">Mitigate</span></span>

<span data-ttu-id="8e8d2-150">Om problemet sker just nu, klickar du på **analysera** > **diagnostik** > **diagnostisera nu** att skapa en diagnostisk session, vilket samlar in http-loggar, loggar i Loggboken, minne Dumpar, PHP felloggarna och PHP bearbeta rapporten.</span><span class="sxs-lookup"><span data-stu-id="8e8d2-150">If the issue is happening right now, click **Analyze** > **Diagnostics** > **Diagnose Now** to create a diagnostic session for you, which will collect HTTP logs, event viewer logs, memory dumps, PHP error logs and PHP process report.</span></span>

<span data-ttu-id="8e8d2-151">När data samlas in, kommer den även kör en analys på data och ger dig en HTML-rapport.</span><span class="sxs-lookup"><span data-stu-id="8e8d2-151">Once the data is collected, it will also run an analysis on the data and provide you with an HTML report.</span></span>

<span data-ttu-id="8e8d2-152">Om du vill hämta data, som standard lagras den i mappen D:\home\data\DaaS.</span><span class="sxs-lookup"><span data-stu-id="8e8d2-152">In case you want to download the data, by default, it would be stored in the D:\home\data\DaaS folder.</span></span>

<span data-ttu-id="8e8d2-153">Mer information om stöd för Azure App Service-portalen finns [nya uppdateringar till stöd för plats-tillägg för Azure Websites](https://azure.microsoft.com/blog/new-updates-to-support-site-extension-for-azure-websites).</span><span class="sxs-lookup"><span data-stu-id="8e8d2-153">For more information on the Azure App Service Support portal, see [New Updates to Support Site Extension for Azure Websites](https://azure.microsoft.com/blog/new-updates-to-support-site-extension-for-azure-websites).</span></span>

#### <a name="use-the-kudu-debug-console"></a><span data-ttu-id="8e8d2-154">Använd Kudu-Felsökningskonsolen</span><span class="sxs-lookup"><span data-stu-id="8e8d2-154">Use the Kudu Debug Console</span></span>
<span data-ttu-id="8e8d2-155">Web Apps levereras med en Felsökningskonsolen som du kan använda för felsökning, utforska, ladda upp filer, samt JSON-slutpunkter för att hämta information om din miljö.</span><span class="sxs-lookup"><span data-stu-id="8e8d2-155">Web Apps comes with a debug console that you can use for debugging, exploring, uploading files, as well as JSON endpoints for getting information about your environment.</span></span> <span data-ttu-id="8e8d2-156">Detta kallas den *Kudu konsolen* eller *SCM instrumentpanelen* för ditt webbprogram.</span><span class="sxs-lookup"><span data-stu-id="8e8d2-156">This is called the *Kudu Console* or the *SCM Dashboard* for your web app.</span></span>

<span data-ttu-id="8e8d2-157">Du kan komma åt den här instrumentpanelen genom att gå till länken **https://&lt;appens namn >.scm.azurewebsites.net/**.</span><span class="sxs-lookup"><span data-stu-id="8e8d2-157">You can access this dashboard by going to the link **https://&lt;Your app name>.scm.azurewebsites.net/**.</span></span>

<span data-ttu-id="8e8d2-158">Några av de saker som Kudu tillhandahåller är:</span><span class="sxs-lookup"><span data-stu-id="8e8d2-158">Some of the things that Kudu provides are:</span></span>

* <span data-ttu-id="8e8d2-159">inställningar för ditt program</span><span class="sxs-lookup"><span data-stu-id="8e8d2-159">environment settings for your application</span></span>
* <span data-ttu-id="8e8d2-160">loggström</span><span class="sxs-lookup"><span data-stu-id="8e8d2-160">log stream</span></span>
* <span data-ttu-id="8e8d2-161">diagnostiska dump</span><span class="sxs-lookup"><span data-stu-id="8e8d2-161">diagnostic dump</span></span>
* <span data-ttu-id="8e8d2-162">Felsöka konsolen där du kan köra Powershell-cmdlets och grundläggande DOS-kommandon.</span><span class="sxs-lookup"><span data-stu-id="8e8d2-162">debug console in which you can run Powershell cmdlets and basic DOS commands.</span></span>

<span data-ttu-id="8e8d2-163">En annan användbar funktion i Kudu är att, om ditt program är att första chans undantag, kan du använda Kudu och verktyget SysInternals Procdump att skapa minne Dumpar.</span><span class="sxs-lookup"><span data-stu-id="8e8d2-163">Another useful feature of Kudu is that, in case your application is throwing first-chance exceptions, you can use Kudu and the SysInternals tool Procdump to create memory dumps.</span></span> <span data-ttu-id="8e8d2-164">Dessa minnesdumpar är ögonblicksbilder av processen och ofta kan hjälpa dig att felsöka mer komplicerade problem med ditt webbprogram.</span><span class="sxs-lookup"><span data-stu-id="8e8d2-164">These memory dumps are snapshots of the process and can often help you troubleshoot more complicated issues with your web app.</span></span>

<span data-ttu-id="8e8d2-165">Mer information om funktionerna i Kudu finns [Azure Websites online-verktyg som du bör känna till om](https://azure.microsoft.com/blog/windows-azure-websites-online-tools-you-should-know-about/).</span><span class="sxs-lookup"><span data-stu-id="8e8d2-165">For more information on features available in Kudu, see [Azure Websites online tools you should know about](https://azure.microsoft.com/blog/windows-azure-websites-online-tools-you-should-know-about/).</span></span>

<a name="mitigate" />

### <a name="3-mitigate-the-issue"></a><span data-ttu-id="8e8d2-166">3. Åtgärda problemet</span><span class="sxs-lookup"><span data-stu-id="8e8d2-166">3. Mitigate the issue</span></span>
#### <a name="scale-the-web-app"></a><span data-ttu-id="8e8d2-167">Skala webbprogrammet</span><span class="sxs-lookup"><span data-stu-id="8e8d2-167">Scale the web app</span></span>
<span data-ttu-id="8e8d2-168">I Azure App Service kan för bättre prestanda och genomflöde, du justera skalan där du kör programmet.</span><span class="sxs-lookup"><span data-stu-id="8e8d2-168">In Azure App Service, for increased performance and throughput,  you can adjust the scale at which you are running your application.</span></span> <span data-ttu-id="8e8d2-169">Skala upp ett webbprogram omfattar två relaterade åtgärder: ändra din programtjänstplan till en högre prisnivå och konfigurera vissa inställningar när du har växlat till högre prisnivå.</span><span class="sxs-lookup"><span data-stu-id="8e8d2-169">Scaling up a web app involves two related actions: changing your App Service plan to a higher pricing tier, and configuring certain settings after you have switched to the higher pricing tier.</span></span>

<span data-ttu-id="8e8d2-170">Läs mer om att skala [skala en webbapp i Azure App Service](web-sites-scale.md).</span><span class="sxs-lookup"><span data-stu-id="8e8d2-170">For more information on scaling, see [Scale a web app in Azure App Service](web-sites-scale.md).</span></span>

<span data-ttu-id="8e8d2-171">Dessutom kan du välja att köra programmet på mer än en instans.</span><span class="sxs-lookup"><span data-stu-id="8e8d2-171">Additionally, you can choose to run your application on more than one instance .</span></span> <span data-ttu-id="8e8d2-172">Detta inte bara ger mer bearbetningskapacitet, men ger dig även vissa delar av feltolerans.</span><span class="sxs-lookup"><span data-stu-id="8e8d2-172">This not only provides you with more processing capability, but also gives you some amount of fault tolerance.</span></span> <span data-ttu-id="8e8d2-173">Om processen stängs av på en instans, kommer den andra instansen fortfarande begäranden.</span><span class="sxs-lookup"><span data-stu-id="8e8d2-173">If the process goes down on one instance, the other instance will still continue serving requests.</span></span>

<span data-ttu-id="8e8d2-174">Du kan ange skalning till manuell eller automatisk.</span><span class="sxs-lookup"><span data-stu-id="8e8d2-174">You can set the scaling to be Manual or Automatic.</span></span>

#### <a name="use-autoheal"></a><span data-ttu-id="8e8d2-175">Använd säkert att AutoHeal</span><span class="sxs-lookup"><span data-stu-id="8e8d2-175">Use AutoHeal</span></span>
<span data-ttu-id="8e8d2-176">Säkert att AutoHeal återanvänder arbetsprocessen för din app baserat på dina inställningar (till exempel konfigurationsändringar begäranden, minnesbaserade gränser eller den tid som krävs för att utföra en begäran).</span><span class="sxs-lookup"><span data-stu-id="8e8d2-176">AutoHeal recycles the worker process for your app based on settings you choose (like configuration changes, requests, memory-based limits, or the time needed to execute a request).</span></span> <span data-ttu-id="8e8d2-177">I de flesta fall, är omarbetning av processen det snabbaste sättet att återställa från ett problem.</span><span class="sxs-lookup"><span data-stu-id="8e8d2-177">Most of the time, recycle the process is the fastest way to recover from a problem.</span></span> <span data-ttu-id="8e8d2-178">Även om du kan alltid starta om webbprogrammet från direkt i Azure Portal, gör säkert att AutoHeal det automatiskt åt dig.</span><span class="sxs-lookup"><span data-stu-id="8e8d2-178">Though you can always restart the web app from directly within the Azure Portal, AutoHeal will do it automatically for you.</span></span> <span data-ttu-id="8e8d2-179">Allt du behöver göra är att lägga till vissa utlösare i rot-web.config för ditt webbprogram.</span><span class="sxs-lookup"><span data-stu-id="8e8d2-179">All you need to do is add some triggers in the root web.config for your web app.</span></span> <span data-ttu-id="8e8d2-180">Observera att de här inställningarna fungerar på samma sätt även om programmet inte är en .net en.</span><span class="sxs-lookup"><span data-stu-id="8e8d2-180">Note that these settings would work in the same way even if your application is not a .Net one.</span></span>

<span data-ttu-id="8e8d2-181">Mer information finns i [automatisk återställning Azure webbplatser](https://azure.microsoft.com/blog/auto-healing-windows-azure-web-sites/).</span><span class="sxs-lookup"><span data-stu-id="8e8d2-181">For more information, see [Auto-Healing Azure Web Sites](https://azure.microsoft.com/blog/auto-healing-windows-azure-web-sites/).</span></span>

#### <a name="restart-the-web-app"></a><span data-ttu-id="8e8d2-182">Starta om webbprogrammet</span><span class="sxs-lookup"><span data-stu-id="8e8d2-182">Restart the web app</span></span>
<span data-ttu-id="8e8d2-183">Det här är ofta det enklaste sättet att återställa engångsproblem.</span><span class="sxs-lookup"><span data-stu-id="8e8d2-183">This is often the simplest way to recover from one-time issues.</span></span> <span data-ttu-id="8e8d2-184">På den [Azure Portal](https://portal.azure.com/), på bladet för ditt webbprogram har du alternativ för att stoppa eller starta om din app.</span><span class="sxs-lookup"><span data-stu-id="8e8d2-184">On the [Azure Portal](https://portal.azure.com/), on your web app’s blade, you have the options to stop or restart your app.</span></span>

 ![Starta om appen för att lösa HTTP-fel 502 Felaktig gateway och 503 inte tillgänglig](./media/app-service-web-troubleshoot-HTTP-502-503/2-restart.png)

<span data-ttu-id="8e8d2-186">Du kan också hantera ditt webbprogram med hjälp av Azure Powershell.</span><span class="sxs-lookup"><span data-stu-id="8e8d2-186">You can also manage your web app using Azure Powershell.</span></span> <span data-ttu-id="8e8d2-187">Mer information finns i [Använda Azure PowerShell med Azure Resource Manager](../powershell-azure-resource-manager.md).</span><span class="sxs-lookup"><span data-stu-id="8e8d2-187">For more information, see [Using Azure PowerShell with Azure Resource Manager](../powershell-azure-resource-manager.md).</span></span>

