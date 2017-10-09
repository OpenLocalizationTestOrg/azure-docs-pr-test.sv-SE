---
title: aaaMonitor en Webbapp | Microsoft Docs
description: "Lär dig hur tooset in övervakning på ditt webbprogram"
services: App-Service
keywords: 
author: btardif
ms.author: byvinyal
ms.date: 04/04/2017
ms.topic: article
ms.service: app-service-web
ms.openlocfilehash: c2f5e9842c732a804f1caee5d67e53dad24e190a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-app-service"></a><span data-ttu-id="a8fca-103">Övervaka App Service</span><span class="sxs-lookup"><span data-stu-id="a8fca-103">Monitor App Service</span></span>
<span data-ttu-id="a8fca-104">Den här självstudiekursen vägleder dig genom att övervaka din app och använda hello inbyggda plattform verktyg toosolve problem när de inträffar.</span><span class="sxs-lookup"><span data-stu-id="a8fca-104">This tutorial walks you through monitoring your app and using hello built-in platform tools toosolve problems when they occur.</span></span>

<span data-ttu-id="a8fca-105">Varje avsnitt i det här dokumentet går över en specifik funktion.</span><span class="sxs-lookup"><span data-stu-id="a8fca-105">Each section of this document goes over a specific feature.</span></span> <span data-ttu-id="a8fca-106">Använda hello-funktioner tillsammans kan du:</span><span class="sxs-lookup"><span data-stu-id="a8fca-106">Using hello features together let you:</span></span>
- <span data-ttu-id="a8fca-107">Identifierar ett problem i din app.</span><span class="sxs-lookup"><span data-stu-id="a8fca-107">Identifying an issue in your app.</span></span>
- <span data-ttu-id="a8fca-108">Avgöra när hello problemet orsakas av din kod eller hello plattform.</span><span class="sxs-lookup"><span data-stu-id="a8fca-108">Determining when hello issue is caused by your code or hello platform.</span></span>
- <span data-ttu-id="a8fca-109">Begränsa hello källan hello problemet i koden.</span><span class="sxs-lookup"><span data-stu-id="a8fca-109">Narrow down hello source of hello problem in your code.</span></span>
- <span data-ttu-id="a8fca-110">Felsökning och åtgärda hello problem.</span><span class="sxs-lookup"><span data-stu-id="a8fca-110">Debugging and fixing hello issue.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="a8fca-111">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="a8fca-111">Before you begin</span></span>
- <span data-ttu-id="a8fca-112">Du behöver en Web App toomonitor och följ hello beskrivs stegen.</span><span class="sxs-lookup"><span data-stu-id="a8fca-112">You need a Web App toomonitor and follow hello outlined steps.</span></span>
    - <span data-ttu-id="a8fca-113">Du kan skapa ett program efter hello stegen som beskrivs i hello [skapa en ASP.NET-app i Azure med SQL Database](app-service-web-tutorial-dotnet-sqldatabase.md) kursen.</span><span class="sxs-lookup"><span data-stu-id="a8fca-113">You can create an application following hello steps described in hello [Create an ASP.NET app in Azure with SQL Database](app-service-web-tutorial-dotnet-sqldatabase.md) tutorial.</span></span>

- <span data-ttu-id="a8fca-114">Om du vill tootry **fjärrfelsökning** för din app behöver du Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="a8fca-114">If you want tootry out **Remote Debugging** of your application, you need Visual Studio.</span></span>
    - <span data-ttu-id="a8fca-115">Om du inte redan har Visual Studio 2017 installerat, du kan hämta och använda hello ledigt [Visual Studio 2017 Community Edition](https://www.visualstudio.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="a8fca-115">If you don’t already have Visual Studio 2017 installed, you can download and use hello free [Visual Studio 2017 Community Edition](https://www.visualstudio.com/downloads/).</span></span>
    - <span data-ttu-id="a8fca-116">Kontrollera att du aktiverar **Azure-utveckling** under installationen av hello Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="a8fca-116">Make sure that you enable **Azure development** during hello Visual Studio setup.</span></span>

## <span data-ttu-id="a8fca-117"><a name="metrics"></a>Steg 1 – Visa mått</span><span class="sxs-lookup"><span data-stu-id="a8fca-117"><a name="metrics"></a> Step 1 - View metrics</span></span>
<span data-ttu-id="a8fca-118">**Mått** är användbara toounderstand:</span><span class="sxs-lookup"><span data-stu-id="a8fca-118">**Metrics** are useful toounderstand:</span></span>
- <span data-ttu-id="a8fca-119">Programmets hälsotillstånd</span><span class="sxs-lookup"><span data-stu-id="a8fca-119">Application health</span></span>
- <span data-ttu-id="a8fca-120">App-prestanda</span><span class="sxs-lookup"><span data-stu-id="a8fca-120">App performance</span></span>
- <span data-ttu-id="a8fca-121">Resursförbrukning</span><span class="sxs-lookup"><span data-stu-id="a8fca-121">Resource consumption</span></span>

<span data-ttu-id="a8fca-122">När du undersöker ett problem med programmet, är granska mått ett bra toostart.</span><span class="sxs-lookup"><span data-stu-id="a8fca-122">When investigating an application issue, reviewing metrics is a good place toostart.</span></span> <span data-ttu-id="a8fca-123">Azure-portalen har ett snabbt sätt toovisually inspektera hello mätvärden för din app med hjälp av **Azure-Monitor**.</span><span class="sxs-lookup"><span data-stu-id="a8fca-123">Azure portal has a quick way toovisually inspect hello metrics of your app using **Azure Monitor**.</span></span>

<span data-ttu-id="a8fca-124">Mått ger en historiköversikt över flera viktiga aggregeringar för din app.</span><span class="sxs-lookup"><span data-stu-id="a8fca-124">Metrics provide a historical view across several key aggregations for your app.</span></span> <span data-ttu-id="a8fca-125">För alla appar som finns i app service, bör du övervaka både hello Web App och hello App Service-plan.</span><span class="sxs-lookup"><span data-stu-id="a8fca-125">For any app hosted in app service, you should monitor both hello Web App and hello App Service plan.</span></span>

> [!NOTE]
> <span data-ttu-id="a8fca-126">En apptjänstplan representerar hello mängd fysiska resurser som används toohost dina appar.</span><span class="sxs-lookup"><span data-stu-id="a8fca-126">An App Service plan represents hello collection of physical resources used toohost your apps.</span></span> <span data-ttu-id="a8fca-127">Alla program som tilldelats tooan App Service-plan hello resurser definieras av den så att du toosave kostnad när värd för flera appar.</span><span class="sxs-lookup"><span data-stu-id="a8fca-127">All applications assigned tooan App Service plan share hello resources defined by it allowing you toosave cost when hosting multiple apps.</span></span>
>
> <span data-ttu-id="a8fca-128">App Service-planer definierar följande:</span><span class="sxs-lookup"><span data-stu-id="a8fca-128">App Service plans define:</span></span>
> * <span data-ttu-id="a8fca-129">Region: Nordeuropa, östra USA, Sydostasien, osv.</span><span class="sxs-lookup"><span data-stu-id="a8fca-129">Region: North Europe, East US, Southeast Asia, etc.</span></span>
> * <span data-ttu-id="a8fca-130">Instansstorleken: Liten, Medium, Large och så vidare.</span><span class="sxs-lookup"><span data-stu-id="a8fca-130">Instance Size: Small, Medium, Large, etc.</span></span>
> * <span data-ttu-id="a8fca-131">Skalningsantalet: en, två eller tre instanser, etc.</span><span class="sxs-lookup"><span data-stu-id="a8fca-131">Scale Count: one, two, or three instances, etc.</span></span>
> * <span data-ttu-id="a8fca-132">SKU: Ledigt delade, Basic, Standard, Premium, osv.</span><span class="sxs-lookup"><span data-stu-id="a8fca-132">SKU: Free, Shared, Basic, Standard, Premium, etc.</span></span>

<span data-ttu-id="a8fca-133">tooreview mätvärden för ditt webbprogram, gå toohello **översikt** bladet hello app som du vill toomonitor.</span><span class="sxs-lookup"><span data-stu-id="a8fca-133">tooreview metrics for your Web App, go toohello **Overview** blade of hello app you want toomonitor.</span></span> <span data-ttu-id="a8fca-134">Härifrån kan du visa ett diagram för din app mått som en **övervakning panelen**.</span><span class="sxs-lookup"><span data-stu-id="a8fca-134">From here, you can view a chart for your app's metrics as a **Monitoring tile**.</span></span> <span data-ttu-id="a8fca-135">Klicka på hello panelen tooedit och konfigurera vilka mått tooview och hello tid intervallet toodisplay.</span><span class="sxs-lookup"><span data-stu-id="a8fca-135">Click hello tile tooedit and configure what metrics tooview and hello time range toodisplay.</span></span>

<span data-ttu-id="a8fca-136">Ger en vy för hello programförfrågningar och fel för hello senaste timmen som standard hello-resursbladet.</span><span class="sxs-lookup"><span data-stu-id="a8fca-136">By default hello resource blade provides a view for hello Application Requests and errors for hello last hour.</span></span>
<span data-ttu-id="a8fca-137">![Övervaka App](media/app-service-web-tutorial-monitoring/app-service-monitor.png)</span><span class="sxs-lookup"><span data-stu-id="a8fca-137">![Monitor App](media/app-service-web-tutorial-monitoring/app-service-monitor.png)</span></span>

<span data-ttu-id="a8fca-138">Som du ser i exemplet hello vi har ett program som genererar många **HTTP-fel**.</span><span class="sxs-lookup"><span data-stu-id="a8fca-138">As you can see in hello example, we have an application that is generating many **HTTP Server Errors**.</span></span> <span data-ttu-id="a8fca-139">hello hög volym av fel är hello första indikation tooinvestigate måste det här programmet.</span><span class="sxs-lookup"><span data-stu-id="a8fca-139">hello high volume of errors is hello first indication we need tooinvestigate this application.</span></span>

> [!TIP]
> <span data-ttu-id="a8fca-140">Lär dig mer om Azure-Monitor med hello följande länkar:</span><span class="sxs-lookup"><span data-stu-id="a8fca-140">Learn more about Azure Monitor with hello following links:</span></span>
> - [<span data-ttu-id="a8fca-141">Kom igång med Azure-Monitor</span><span class="sxs-lookup"><span data-stu-id="a8fca-141">Get started with Azure Monitor</span></span>](..\monitoring-and-diagnostics\monitoring-overview.md)
> - [<span data-ttu-id="a8fca-142">Azure mått</span><span class="sxs-lookup"><span data-stu-id="a8fca-142">Azure Metrics</span></span>](..\monitoring-and-diagnostics\monitoring-overview-metrics.md)
> - [<span data-ttu-id="a8fca-143">Stöds mått med Azure-Monitor</span><span class="sxs-lookup"><span data-stu-id="a8fca-143">Supported metrics with Azure Monitor</span></span>](..\monitoring-and-diagnostics\monitoring-supported-metrics.md)
> - [<span data-ttu-id="a8fca-144">Azure instrumentpaneler</span><span class="sxs-lookup"><span data-stu-id="a8fca-144">Azure Dashboards</span></span>](..\azure-portal\azure-portal-dashboards.md)

## <span data-ttu-id="a8fca-145"><a name="alerts"></a>Steg 2 – konfigurera aviseringar</span><span class="sxs-lookup"><span data-stu-id="a8fca-145"><a name="alerts"></a> Step 2 - Configure Alerts</span></span>
<span data-ttu-id="a8fca-146">**Aviseringar** kan vara konfigurerade tootrigger på särskilda villkor för din app.</span><span class="sxs-lookup"><span data-stu-id="a8fca-146">**Alerts** can be configured tootrigger on specific conditions for your app.</span></span>

<span data-ttu-id="a8fca-147">I [steg 1 – Visa mått](#metrics), vi såg att programmet hello hade ett stort antal fel.</span><span class="sxs-lookup"><span data-stu-id="a8fca-147">In [Step 1 - View metrics](#metrics), we saw that hello application had a high number of errors.</span></span>

<span data-ttu-id="a8fca-148">Kan du konfigurera en varning tooautomatically få meddelanden när ett fel uppstår.</span><span class="sxs-lookup"><span data-stu-id="a8fca-148">Lets configure an alert tooautomatically get notified when errors occur.</span></span> <span data-ttu-id="a8fca-149">I det här fallet vi vill hello avisering toosend och e-post varje gång hello antal HTTP-50 X fel färdas över ett visst tröskelvärde.</span><span class="sxs-lookup"><span data-stu-id="a8fca-149">In this case, we want hello alert toosend and e-mail every time hello number of HTTP 50X errors goes over a certain threshold.</span></span>

<span data-ttu-id="a8fca-150">toocreate en avisering, navigera för**övervakning** > **aviseringar** och på **[+] Lägg till avisering**.</span><span class="sxs-lookup"><span data-stu-id="a8fca-150">toocreate an alert, navigate too**Monitoring** > **Alerts** and click **[+] Add Alert**.</span></span>

![Aviseringar](media/app-service-web-tutorial-monitoring/app-service-monitor-alerts.png)

<span data-ttu-id="a8fca-152">Ange värden för hello varningskonfigurationen:</span><span class="sxs-lookup"><span data-stu-id="a8fca-152">Provide values for hello Alert configuration:</span></span>
- <span data-ttu-id="a8fca-153">**Resurs:** hello plats toomonitor med hello avisering.</span><span class="sxs-lookup"><span data-stu-id="a8fca-153">**Resource:** hello site toomonitor with hello alert.</span></span>
- <span data-ttu-id="a8fca-154">**Namn:** ett namn för aviseringen i det här fallet: *hög HTTP 50 X*.</span><span class="sxs-lookup"><span data-stu-id="a8fca-154">**Name:** A name for your alert, in this case: *High HTTP 50X*.</span></span>
- <span data-ttu-id="a8fca-155">**Beskrivning:** oformaterad text förklaring av aviseringen är ute på.</span><span class="sxs-lookup"><span data-stu-id="a8fca-155">**Description:** Plain text explanation of what this alert is looking at.</span></span>
- <span data-ttu-id="a8fca-156">**Varning vid:** aviseringar kan titta på mått eller händelser, för det här exemplet vi tittar på mått.</span><span class="sxs-lookup"><span data-stu-id="a8fca-156">**Alert on:** Alerts can look at Metrics or Events, for this example we are looking at metrics.</span></span>
- <span data-ttu-id="a8fca-157">**Mått:** vilka mått toomonitor i det här fallet: *HTTP-fel*.</span><span class="sxs-lookup"><span data-stu-id="a8fca-157">**Metric:** What metric toomonitor, in this case: *HTTP Server Errors*.</span></span>
- <span data-ttu-id="a8fca-158">**Villkor:** när tooalert, i det här fallet väljer hello *större än* alternativet.</span><span class="sxs-lookup"><span data-stu-id="a8fca-158">**Condition:** When tooalert, in this case select hello *greater than* option.</span></span>
- <span data-ttu-id="a8fca-159">**Tröskelvärde:** vad är värdet toolook för, i det här fallet: *400*.</span><span class="sxs-lookup"><span data-stu-id="a8fca-159">**Threshold:** What is value toolook for, in this case: *400*.</span></span>
- <span data-ttu-id="a8fca-160">**Period:** aviseringar fungerar över hello medelvärdet för ett mått.</span><span class="sxs-lookup"><span data-stu-id="a8fca-160">**Period:** Alerts operate over hello average value of a metric.</span></span> <span data-ttu-id="a8fca-161">Mindre tidsperioder ge känsligare aviseringar.</span><span class="sxs-lookup"><span data-stu-id="a8fca-161">Smaller periods of time yield more sensitive alerts.</span></span> <span data-ttu-id="a8fca-162">i det här fallet vi tittar på *5 minuter*.</span><span class="sxs-lookup"><span data-stu-id="a8fca-162">in this case we are looking at *5 Minutes*.</span></span>
- <span data-ttu-id="a8fca-163">**E-ägare och deltagare:** i det här fallet: *aktiverad*.</span><span class="sxs-lookup"><span data-stu-id="a8fca-163">**Email Owners and contributors:** in this case: *Enabled*.</span></span>

<span data-ttu-id="a8fca-164">Nu när hello avisering skapas ett e-postmeddelande skickas varje gång hello appen går över hello konfigurerade tröskelvärdet.</span><span class="sxs-lookup"><span data-stu-id="a8fca-164">Now that hello alert is created an email is sent every time hello app goes over hello configured threshold.</span></span> <span data-ttu-id="a8fca-165">Aktiva aviseringar kan också ses i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="a8fca-165">Active alerts can also be reviewed in hello Azure portal.</span></span>

![Utlösta varningar](media/app-service-web-tutorial-monitoring/app-service-monitor-alerts-triggered.png)


> [!TIP]
> <span data-ttu-id="a8fca-167">Läs mer om Azure aviseringar med hello följande länkar:</span><span class="sxs-lookup"><span data-stu-id="a8fca-167">Learn more about Azure Alerts with hello following links:</span></span>
> - [<span data-ttu-id="a8fca-168">Vad är aviseringar i Microsoft Azure</span><span class="sxs-lookup"><span data-stu-id="a8fca-168">What are alerts in Microsoft Azure</span></span>](..\monitoring-and-diagnostics\monitoring-overview-alerts.md)
> - [<span data-ttu-id="a8fca-169">Utför en åtgärd på mått</span><span class="sxs-lookup"><span data-stu-id="a8fca-169">Take Action On Metrics</span></span>](..\monitoring-and-diagnostics\monitoring-overview.md)
> - [<span data-ttu-id="a8fca-170">Skapa mått aviseringar</span><span class="sxs-lookup"><span data-stu-id="a8fca-170">Create metric alerts</span></span>](..\monitoring-and-diagnostics\insights-alerts-portal.md)

## <span data-ttu-id="a8fca-171"><a name="companion"></a>Steg 3 – Apptjänst tillhörande</span><span class="sxs-lookup"><span data-stu-id="a8fca-171"><a name="companion"></a> Step 3 - App Service Companion</span></span>
<span data-ttu-id="a8fca-172">**Apptjänst tillhörande** erbjuder ett enkelt sätt toomonitor din app med en inbyggd upplevelse i din mobila enhet (iOS eller Android).</span><span class="sxs-lookup"><span data-stu-id="a8fca-172">**App Service companion** offers a convenient way toomonitor your app with a native experience in your mobile device (iOS or Android).</span></span>

<span data-ttu-id="a8fca-173">Använd medföljande App Service till:</span><span class="sxs-lookup"><span data-stu-id="a8fca-173">Use App Service Companion to:</span></span>
- <span data-ttu-id="a8fca-174">Granska program-mått</span><span class="sxs-lookup"><span data-stu-id="a8fca-174">Review application metrics</span></span>
- <span data-ttu-id="a8fca-175">Granska och reagera på programaviseringar och rekommendationer</span><span class="sxs-lookup"><span data-stu-id="a8fca-175">Review and act on application alerts and recommendations</span></span>
- <span data-ttu-id="a8fca-176">Grundläggande felsökning (Bläddra, starta, stoppa, omstart hello app)</span><span class="sxs-lookup"><span data-stu-id="a8fca-176">Perform basic troubleshooting (browse, start, stop, restart hello app)</span></span>
- <span data-ttu-id="a8fca-177">Hämta push-meddelanden för kritiska händelser.</span><span class="sxs-lookup"><span data-stu-id="a8fca-177">Get push notifications for critical events.</span></span>

![Medföljande App Service](media/app-service-web-tutorial-monitoring/app-service-companion.png)

<span data-ttu-id="a8fca-179">[![App Service medföljande App Store](media/app-service-web-tutorial-monitoring/app-service-companion-appStore.png)](https://itunes.apple.com/app/azure-app-service-companion/id1146659260)
[![Apptjänst medföljande Google Play](media/app-service-web-tutorial-monitoring/app-service-companion-googlePlay.png)](https://play.google.com/store/apps/details?id=azureApps.AzureApps)</span><span class="sxs-lookup"><span data-stu-id="a8fca-179">[![App Service Companion App Store](media/app-service-web-tutorial-monitoring/app-service-companion-appStore.png)](https://itunes.apple.com/app/azure-app-service-companion/id1146659260)
[![App Service Companion Google Play](media/app-service-web-tutorial-monitoring/app-service-companion-googlePlay.png)](https://play.google.com/store/apps/details?id=azureApps.AzureApps)</span></span>

<span data-ttu-id="a8fca-180">Du kan installera Apptjänst tillhörande från hello [Appbutiken](https://itunes.apple.com/app/azure-app-service-companion/id1146659260) eller [Google Play](https://play.google.com/store/apps/details?id=azureApps.AzureApps)</span><span class="sxs-lookup"><span data-stu-id="a8fca-180">You can install App Service companion from hello [App Store](https://itunes.apple.com/app/azure-app-service-companion/id1146659260) or [Google Play](https://play.google.com/store/apps/details?id=azureApps.AzureApps)</span></span>

## <span data-ttu-id="a8fca-181"><a name="diagnose"></a>Steg 4 – diagnostisera och lösa problem</span><span class="sxs-lookup"><span data-stu-id="a8fca-181"><a name="diagnose"></a> Step 4 - Diagnose and solve problems</span></span>
<span data-ttu-id="a8fca-182">**Diagnostisera och lösa problem** hjälper du separera programproblem formuläret plattform problem.</span><span class="sxs-lookup"><span data-stu-id="a8fca-182">**Diagnose and solve problems** helps you separate application issues form platform issues.</span></span> <span data-ttu-id="a8fca-183">Det kan också föreslå möjliga åtgärder tooget din tillbaka toohealthy Web App.</span><span class="sxs-lookup"><span data-stu-id="a8fca-183">It can also suggest possible mitigations tooget your Web App back toohealthy.</span></span>

![Diagnostisera och lösa problem](media/app-service-web-tutorial-monitoring/app-service-monitor-diagnosis.png)

<span data-ttu-id="a8fca-185">Fortsätter med hello exempel formuläret föregående steg, se vi att programmet hello har med availably problem.</span><span class="sxs-lookup"><span data-stu-id="a8fca-185">Continuing with hello example form previous steps, we can see that hello application has been having availably issues.</span></span> <span data-ttu-id="a8fca-186">Hello plattform tillgänglighet har däremot inte flyttats från 100%.</span><span class="sxs-lookup"><span data-stu-id="a8fca-186">In contrast, hello platform availability has not moved from 100%.</span></span>

<span data-ttu-id="a8fca-187">När hello app har problemet och hello plattform förblir upp, är det en tydlig indikation på att vi arbetar med ett problem med programmet.</span><span class="sxs-lookup"><span data-stu-id="a8fca-187">When hello app is having issue and hello platform stays up, it's a clear indication that we are dealing with an application issue.</span></span>

## <span data-ttu-id="a8fca-188"><a name="logging"></a>Steg 5 - loggning</span><span class="sxs-lookup"><span data-stu-id="a8fca-188"><a name="logging"></a> Step 5 - Logging</span></span>
<span data-ttu-id="a8fca-189">Nu när vi har minskar antalet problem med programmet hello fel tooan, kan vi titta på hello program- och loggar tooget mer information.</span><span class="sxs-lookup"><span data-stu-id="a8fca-189">Now that we have narrowed down hello failures tooan application issue, we can look at hello application and server logs tooget more information.</span></span>

<span data-ttu-id="a8fca-190">Loggning kan du toocollect båda **Programdiagnostik** och **Web Serverdiagnostik** loggar för Webbappen.</span><span class="sxs-lookup"><span data-stu-id="a8fca-190">Logging allows you toocollect both **Application Diagnostics** and **Web Server Diagnostics** logs for your Web App.</span></span>

### <a name="application-diagnostics"></a><span data-ttu-id="a8fca-191">Application Diagnostics</span><span class="sxs-lookup"><span data-stu-id="a8fca-191">Application Diagnostics</span></span>
<span data-ttu-id="a8fca-192">Programdiagnostik kan du toocapture spårningar produceras av programmet hello vid körning.</span><span class="sxs-lookup"><span data-stu-id="a8fca-192">Application diagnostics allows you toocapture traces produced by hello application at runtime.</span></span>

<span data-ttu-id="a8fca-193">Lägga till spårning tooyour förbättrar programmet avsevärt din möjlighet toodebug och pekpunkt problem.</span><span class="sxs-lookup"><span data-stu-id="a8fca-193">Adding tracing tooyour application greatly improves your ability toodebug and pin-point issues.</span></span>

<span data-ttu-id="a8fca-194">I ASP.NET, du kan logga in programspårningar med [System.Diagnostics.Trace klassen](https://msdn.microsoft.com/library/system.diagnostics.trace.aspx) toogenerate händelser som samlas in av hello loggen infrastruktur.</span><span class="sxs-lookup"><span data-stu-id="a8fca-194">In ASP.NET, you can log application traces using [System.Diagnostics.Trace class](https://msdn.microsoft.com/library/system.diagnostics.trace.aspx) toogenerate events that are captured by hello log infrastructure.</span></span> <span data-ttu-id="a8fca-195">Du kan också ange hello allvarlighetsgraden hello spårning för enklare filtrering.</span><span class="sxs-lookup"><span data-stu-id="a8fca-195">You can also specify hello severity of hello trace for easier filtering.</span></span>

```csharp
public ActionResult Delete(Guid? id)
{
    System.Diagnostics.Trace.TraceInformation("GET /Todos/Delete/" + id);
    if (id == null)
    {
        System.Diagnostics.Trace.TraceError("/Todos/Delete/ failed, ID is null");
        return new HttpStatusCodeResult(HttpStatusCode.BadRequest);
    }
    Todo todo = db.Todoes.Find(id);
    if (todo == null)
    {
        System.Diagnostics.Trace.TraceWarning("/Todos/Delete/ failed, ID: " + id + " could not be found");
        return HttpNotFound();
    }
    System.Diagnostics.Trace.TraceInformation("GET /Todos/Delete/" + id + "completed successfully");
    return View(todo);
}
```
<span data-ttu-id="a8fca-196">tooenable programloggning gå för**övervakning** > **diagnostikloggar** och aktivera programloggning med hello växlar.</span><span class="sxs-lookup"><span data-stu-id="a8fca-196">tooenable Application logging Go too**Monitoring** > **Diagnostic Logs** and Enable Application Logging using hello toggles.</span></span>

![Övervaka App](media/app-service-web-tutorial-monitoring/app-service-monitor-applogs.png)

<span data-ttu-id="a8fca-198">Programloggarna kan vara lagrade tooyour Web App filsystem eller push-installeras tooblob lagring.</span><span class="sxs-lookup"><span data-stu-id="a8fca-198">Application logs can be stored tooyour Web App's file system or pushed out tooblob storage.</span></span> <span data-ttu-id="a8fca-199">Scenarier för produktion är det rekommenderade toouse blob-lagring.</span><span class="sxs-lookup"><span data-stu-id="a8fca-199">For production scenarios, it's recommended toouse blob storage.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="a8fca-200">Om du aktiverar loggning påverkar dina program prestanda- och användning.</span><span class="sxs-lookup"><span data-stu-id="a8fca-200">Enabling logging has an impact on your application performance and resource utilization.</span></span> <span data-ttu-id="a8fca-201">För produktion scenarier rekommenderas felloggar.</span><span class="sxs-lookup"><span data-stu-id="a8fca-201">For production scenarios, error logs are recommended.</span></span> <span data-ttu-id="a8fca-202">Aktivera endast mer utförlig loggning när du undersöker problem.</span><span class="sxs-lookup"><span data-stu-id="a8fca-202">Only enable more verbose logging when investigating issues.</span></span>

 ### <a name="web-server-diagnostics"></a><span data-ttu-id="a8fca-203">Web Serverdiagnostik</span><span class="sxs-lookup"><span data-stu-id="a8fca-203">Web Server Diagnostics</span></span>
<span data-ttu-id="a8fca-204">Webbserverloggarna genereras även om din app inte instrumenterats.</span><span class="sxs-lookup"><span data-stu-id="a8fca-204">Web Server logs are generated even if your app is not instrumented.</span></span> <span data-ttu-id="a8fca-205">Apptjänst kan samla in tre olika typer av server-loggar:</span><span class="sxs-lookup"><span data-stu-id="a8fca-205">App Service can collect three different types of server logs:</span></span>

- <span data-ttu-id="a8fca-206">**Web Server-loggning**</span><span class="sxs-lookup"><span data-stu-id="a8fca-206">**Web Server Logging**</span></span>
    - <span data-ttu-id="a8fca-207">Information om HTTP-transaktioner med hello [W3C utökat loggfilsformat](https://msdn.microsoft.com/library/windows/desktop/aa814385.aspx).</span><span class="sxs-lookup"><span data-stu-id="a8fca-207">Information about HTTP transactions using hello [W3C extended log file format](https://msdn.microsoft.com/library/windows/desktop/aa814385.aspx).</span></span>
    - <span data-ttu-id="a8fca-208">Användbart när du fastställer övergripande plats mätvärden, till exempel hello antal begäranden som hanteras eller hur många förfrågningar som kommer från en specifik IP-adress.</span><span class="sxs-lookup"><span data-stu-id="a8fca-208">Useful when determining overall site metrics such as hello number of requests handled or how many requests are from a specific IP address.</span></span>
- <span data-ttu-id="a8fca-209">**Detaljerad felloggning**</span><span class="sxs-lookup"><span data-stu-id="a8fca-209">**Detailed Error Logging**</span></span>
    - <span data-ttu-id="a8fca-210">Detaljerad information om fel för HTTP-statuskoder som indikerar att en (statuskod 400 eller högre).</span><span class="sxs-lookup"><span data-stu-id="a8fca-210">Detailed error information for HTTP status codes that indicate a failure (status code 400 or greater).</span></span>
    - [<span data-ttu-id="a8fca-211">Mer information om detaljerad felloggning</span><span class="sxs-lookup"><span data-stu-id="a8fca-211">Learn more about detailed error logging</span></span>](https://www.iis.net/learn/troubleshoot/diagnosing-http-errors/how-to-use-http-detailed-errors-in-iis)
- <span data-ttu-id="a8fca-212">**Spårning av misslyckade begäranden**</span><span class="sxs-lookup"><span data-stu-id="a8fca-212">**Failed Request Tracing**</span></span>
    - <span data-ttu-id="a8fca-213">Detaljerad information om misslyckade förfrågningar, inklusive en spårning av hello IIS-komponenter används tooprocess hello begäran och hello tid i varje komponent.</span><span class="sxs-lookup"><span data-stu-id="a8fca-213">Detailed information on failed requests, including a trace of hello IIS components used tooprocess hello request and hello time taken in each component.</span></span>
    - <span data-ttu-id="a8fca-214">Loggar för misslyckade begäranden är användbara vid tooisolate vad som orsakar ett specifikt HTTP-fel.</span><span class="sxs-lookup"><span data-stu-id="a8fca-214">Failed request logs are useful when trying tooisolate what is causing a specific HTTP error.</span></span>
    - [<span data-ttu-id="a8fca-215">Mer information om spårning av misslyckade begäranden</span><span class="sxs-lookup"><span data-stu-id="a8fca-215">Learn more about failed request tracing</span></span>](https://www.iis.net/learn/troubleshoot/using-failed-request-tracing/troubleshooting-failed-requests-using-tracing-in-iis)

<span data-ttu-id="a8fca-216">tooenable Server-loggning:</span><span class="sxs-lookup"><span data-stu-id="a8fca-216">tooenable Server logging:</span></span>
- <span data-ttu-id="a8fca-217">Gå för**övervakning** > **diagnostikloggar**.</span><span class="sxs-lookup"><span data-stu-id="a8fca-217">go too**Monitoring** > **Diagnostic Logs**.</span></span>
- <span data-ttu-id="a8fca-218">Aktivera hello olika typer av Web Serverdiagnostik med hello växlar.</span><span class="sxs-lookup"><span data-stu-id="a8fca-218">Enable hello different types of Web Server Diagnostics using hello toggles.</span></span>

![Övervaka App](media/app-service-web-tutorial-monitoring/app-service-monitor-serverlogs.png)

> [!IMPORTANT]
> <span data-ttu-id="a8fca-220">Om du aktiverar loggning påverkar dina program prestanda- och användning.</span><span class="sxs-lookup"><span data-stu-id="a8fca-220">Enabling logging has an impact on your application performance and resource utilization.</span></span> <span data-ttu-id="a8fca-221">För produktion scenarier rekommenderas felloggar, aktivera endast mer utförlig loggning när du undersöker problem.</span><span class="sxs-lookup"><span data-stu-id="a8fca-221">For Production Scenarios, Error logs are recommended, Only Enable more verbose logging when investigating issues.</span></span>

### <a name="accessing-logs"></a><span data-ttu-id="a8fca-222">Åtkomst till loggar</span><span class="sxs-lookup"><span data-stu-id="a8fca-222">Accessing Logs</span></span>
<span data-ttu-id="a8fca-223">Loggfilerna lagras i blob storage kan nås med hjälp av Azure Lagringsutforskaren.</span><span class="sxs-lookup"><span data-stu-id="a8fca-223">Logs stored in blob storage are accessed using Azure Storage Explorer.</span></span> <span data-ttu-id="a8fca-224">Loggar som finns i hello Web App filsystem kan nås via FTP under hello följande sökvägar:</span><span class="sxs-lookup"><span data-stu-id="a8fca-224">Logs stored in hello Web App's filesystem are accessed through FTP under hello following paths:</span></span>

- <span data-ttu-id="a8fca-225">**Programloggarna** - `%HOME%/LogFiles/Application/`.</span><span class="sxs-lookup"><span data-stu-id="a8fca-225">**Application logs** - `%HOME%/LogFiles/Application/`.</span></span>
    - <span data-ttu-id="a8fca-226">Den här mappen innehåller en eller flera textfiler som innehåller information som genereras av programloggning.</span><span class="sxs-lookup"><span data-stu-id="a8fca-226">This folder contains one or more text files containing information produced by application logging.</span></span>
- <span data-ttu-id="a8fca-227">**Det gick inte begäranden** - `%HOME%/LogFiles/W3SVC#########/`.</span><span class="sxs-lookup"><span data-stu-id="a8fca-227">**Failed Request Traces** - `%HOME%/LogFiles/W3SVC#########/`.</span></span>
    - <span data-ttu-id="a8fca-228">Den här mappen innehåller en XSL-fil och en eller flera XML-filer.</span><span class="sxs-lookup"><span data-stu-id="a8fca-228">This folder contains an XSL file and one or more XML files.</span></span>
- <span data-ttu-id="a8fca-229">**Detaljerade felloggar** - `%HOME%/LogFiles/DetailedErrors/`.</span><span class="sxs-lookup"><span data-stu-id="a8fca-229">**Detailed Error Logs** - `%HOME%/LogFiles/DetailedErrors/`.</span></span>
    - <span data-ttu-id="a8fca-230">Den här mappen innehåller en eller flera .htm-filer med omfattande information om HTTP-fel som genereras av din app.</span><span class="sxs-lookup"><span data-stu-id="a8fca-230">This folder contains one or more .htm files with extensive information on HTTP errors generated by your app.</span></span>
- <span data-ttu-id="a8fca-231">**Web Server-loggar** - `%HOME%/LogFiles/http/RawLogs`.</span><span class="sxs-lookup"><span data-stu-id="a8fca-231">**Web Server Logs** - `%HOME%/LogFiles/http/RawLogs`.</span></span>
    - <span data-ttu-id="a8fca-232">Den här mappen innehåller en eller flera textfiler som formaterats med hello W3C utökat loggfilsformat.</span><span class="sxs-lookup"><span data-stu-id="a8fca-232">This folder contains one or more text files formatted using hello W3C extended log file format.</span></span>

## <span data-ttu-id="a8fca-233"><a name="streaming"></a>Steg 6 - loggen strömning</span><span class="sxs-lookup"><span data-stu-id="a8fca-233"><a name="streaming"></a> Step 6 - Log Streaming</span></span>
<span data-ttu-id="a8fca-234">Direktuppspelningsloggar är praktiskt när du felsöker ett program eftersom det sparar tid jämfört med för[åtkomst till hello loggar](#Accessing-Logs) via FTP.</span><span class="sxs-lookup"><span data-stu-id="a8fca-234">Streaming logs are convenient when debugging an application since it saves time compared too[accessing hello logs](#Accessing-Logs) through FTP.</span></span>

<span data-ttu-id="a8fca-235">Apptjänst kan strömma **programloggarna** och **Webbserverloggar** när de skapas.</span><span class="sxs-lookup"><span data-stu-id="a8fca-235">App Service can stream **Application Logs** and **Web Server Logs** as they are generated.</span></span>

> [!TIP]
> <span data-ttu-id="a8fca-236">Innan du försöker toostream loggar, kontrollera att du har aktiverat att samla in loggar enligt beskrivningen i hello [loggning](#logging) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="a8fca-236">Before trying toostream logs, make sure you have enabled collecting logs as described in hello [Logging](#logging) section.</span></span>

<span data-ttu-id="a8fca-237">toostream loggar gå för**övervakning**> **loggström**.</span><span class="sxs-lookup"><span data-stu-id="a8fca-237">toostream logs, go too**Monitoring**> **Log Stream**.</span></span> <span data-ttu-id="a8fca-238">Välj **programloggarna** eller **Web server-loggar** beroende på vilken information du söker efter.</span><span class="sxs-lookup"><span data-stu-id="a8fca-238">Select **Application Logs** or **Web server logs** depending what information you are looking for.</span></span> <span data-ttu-id="a8fca-239">Här kan du även pausa, starta om och rensa hello buffert.</span><span class="sxs-lookup"><span data-stu-id="a8fca-239">From here, you can also pause, restart, and clear hello buffer.</span></span>

![Direktuppspelningsloggar](media/app-service-web-tutorial-monitoring/app-service-monitor-logstream.png)

> [!TIP]
> <span data-ttu-id="a8fca-241">Loggar genereras bara när det trafik på hello app, öka hello detaljnivå av loggar tooget fler händelser eller information.</span><span class="sxs-lookup"><span data-stu-id="a8fca-241">Logs are only generated when there is traffic on hello app, you can also increase hello verbosity of logs tooget more events or information.</span></span>

## <span data-ttu-id="a8fca-242"><a name="remote"></a>Steg 7 – fjärrfelsökning</span><span class="sxs-lookup"><span data-stu-id="a8fca-242"><a name="remote"></a> Step 7 - Remote Debugging</span></span>
<span data-ttu-id="a8fca-243">När du har PIN-kod pekar hello källan för problem med hello program använder **fjärrfelsökning** toowalk via hello kod.</span><span class="sxs-lookup"><span data-stu-id="a8fca-243">Once you have pin-pointed hello source of hello applications problems, use **Remote Debugging** toowalk through hello code.</span></span>

<span data-ttu-id="a8fca-244">Felsökning kan du bifoga en felsökare tooyour Web App körs i hello moln.</span><span class="sxs-lookup"><span data-stu-id="a8fca-244">Remote debugging lets you attach a debugger tooyour Web App running in hello cloud.</span></span> <span data-ttu-id="a8fca-245">Du kan ange brytpunkter, ändra minne direkt, stega igenom koden och även ändra hello kodsökvägen precis som för en app som körs lokalt.</span><span class="sxs-lookup"><span data-stu-id="a8fca-245">You can set breakpoints, manipulate memory directly, step through code, and even change hello code path just like you do for an app running locally.</span></span>

<span data-ttu-id="a8fca-246">tooattach hello felsökare tooyour appen körs i hello moln:</span><span class="sxs-lookup"><span data-stu-id="a8fca-246">tooattach hello debugger tooyour app running in hello cloud:</span></span>

- <span data-ttu-id="a8fca-247">Med hjälp av Visual Studio 2017, öppna hello lösning för hello app vill du toodebug</span><span class="sxs-lookup"><span data-stu-id="a8fca-247">Using Visual Studio 2017, open hello solution for hello app you want toodebug</span></span>
- <span data-ttu-id="a8fca-248">Ange vissa bromsar punkter precis som du skulle för lokal utveckling.</span><span class="sxs-lookup"><span data-stu-id="a8fca-248">Set some brake points just like you would for local development.</span></span>
- <span data-ttu-id="a8fca-249">Öppna **cloud explorer** (ctr + / -ctrl + x).</span><span class="sxs-lookup"><span data-stu-id="a8fca-249">Open **cloud explorer** (ctr + /, ctrl + x).</span></span>
- <span data-ttu-id="a8fca-250">Logga in med dina autentiseringsuppgifter för azure vid behov.</span><span class="sxs-lookup"><span data-stu-id="a8fca-250">Log in with your azure credentials as needed.</span></span>
- <span data-ttu-id="a8fca-251">Hitta hello app som du vill toodebug</span><span class="sxs-lookup"><span data-stu-id="a8fca-251">Find hello app you want toodebug</span></span>
- <span data-ttu-id="a8fca-252">Välj **koppla felsökare** formuläret hello **åtgärder** fönstret.</span><span class="sxs-lookup"><span data-stu-id="a8fca-252">Select **Attach Debugger** form hello **Actions** pane.</span></span>

![Fjärrfelsökning](media/app-service-web-tutorial-monitoring/app-service-monitor-vsdebug.png)

<span data-ttu-id="a8fca-254">Visual Studio konfigurerar ditt program för fjärrfelsökning och öppnar ett webbläsarfönster som navigerar tooyour app.</span><span class="sxs-lookup"><span data-stu-id="a8fca-254">Visual Studio configures your application for remote debugging and launches a browser window that navigates tooyour app.</span></span> <span data-ttu-id="a8fca-255">Bläddra igenom din app tootrigger break punkter och gå igenom hello kod.</span><span class="sxs-lookup"><span data-stu-id="a8fca-255">Browse through your app tootrigger break points and step through hello code.</span></span>

> [!WARNING]
> <span data-ttu-id="a8fca-256">Du bör inte köra i felsökningsläge i produktion.</span><span class="sxs-lookup"><span data-stu-id="a8fca-256">Running in debug mode in production is not recommended.</span></span> <span data-ttu-id="a8fca-257">Om din produktionsprogrammet inte skalas ut toomultiple serverinstanser felsökning hindra hello-webbservern från svarar tooother begäranden.</span><span class="sxs-lookup"><span data-stu-id="a8fca-257">If your production app is not scaled out toomultiple server instances, debugging prevent hello web server from responding tooother requests.</span></span> <span data-ttu-id="a8fca-258">För att felsöka problem i produktionen bästa resursen är för[konfigurera loggning](#logging) och [Programinsikter](#insights).</span><span class="sxs-lookup"><span data-stu-id="a8fca-258">For troubleshooting production problems, your best resource is too[configure logging](#logging) and [Application Insights](#insights).</span></span>



## <span data-ttu-id="a8fca-259"><a name="explorer"></a>Steg 8 – Processutforskaren</span><span class="sxs-lookup"><span data-stu-id="a8fca-259"><a name="explorer"></a> Step 8 - Process Explorer</span></span>
<span data-ttu-id="a8fca-260">När programmet skalas ut toomore än en instans **processutforskaren** kan hjälpa dig att identifiera instans specifika problem.</span><span class="sxs-lookup"><span data-stu-id="a8fca-260">When your application is scaled out toomore than one instance, **process explorer** can help you identify instance specific problems.</span></span>

<span data-ttu-id="a8fca-261">Använd **bearbeta Explorer** till:</span><span class="sxs-lookup"><span data-stu-id="a8fca-261">Use **Process Explorer** to:</span></span>

- <span data-ttu-id="a8fca-262">Räkna upp alla hello processer mellan olika instanser av din programtjänstplan.</span><span class="sxs-lookup"><span data-stu-id="a8fca-262">Enumerate all hello processes across different instances of your App Service plan.</span></span>
- <span data-ttu-id="a8fca-263">Detaljnivån och visa hello referenser och moduler som är associerade med varje process.</span><span class="sxs-lookup"><span data-stu-id="a8fca-263">Drill down and view hello handles and modules associated with each process.</span></span>
- <span data-ttu-id="a8fca-264">Visa CPU, aktiv sidmängd och tråd antal vid hello bearbeta nivå toohelp du identifiera skenande processer</span><span class="sxs-lookup"><span data-stu-id="a8fca-264">View CPU, Working Set, and Thread count at hello process level toohelp you identify runaway processes</span></span>
- <span data-ttu-id="a8fca-265">Hitta öppna filreferenser och även avsluta en specifik process-instans.</span><span class="sxs-lookup"><span data-stu-id="a8fca-265">Find open file handles, and even kill a specific process instance.</span></span>

<span data-ttu-id="a8fca-266">Processutforskaren finns under **övervakning** > **Processutforskaren**.</span><span class="sxs-lookup"><span data-stu-id="a8fca-266">Process Explorer can be found under **Monitoring** > **Process Explorer**.</span></span>

![Processutforskaren](media/app-service-web-tutorial-monitoring/app-service-monitor-processexplorer.png)


## <span data-ttu-id="a8fca-268"><a name="insights"></a>Steg 9 – Application Insights</span><span class="sxs-lookup"><span data-stu-id="a8fca-268"><a name="insights"></a> Step 9 - Application Insights</span></span>
<span data-ttu-id="a8fca-269">**Application Insights** ger profileringsspårning för program och funktioner för avancerad övervakning för din app.</span><span class="sxs-lookup"><span data-stu-id="a8fca-269">**Application Insights** provides application profiling and advanced monitoring capabilities for your app.</span></span>

<span data-ttu-id="a8fca-270">Använd Application Insights toodetect och diagnostisera undantag och prestandaproblem i ditt webbprogram.</span><span class="sxs-lookup"><span data-stu-id="a8fca-270">Use Application Insights toodetect and diagnose exceptions and performance issues in your Web App.</span></span>

<span data-ttu-id="a8fca-271">Du kan aktivera Application Insights för ditt webbprogram under **övervakning** > **Application Insights**</span><span class="sxs-lookup"><span data-stu-id="a8fca-271">You can enable Application Insights for your Web App under **Monitoring** > **Application Insights**</span></span>

> [!NOTE]
> <span data-ttu-id="a8fca-272">Application Insights kan medföra att du tooinstall hello Application Insights plats tillägget toostart insamling av data.</span><span class="sxs-lookup"><span data-stu-id="a8fca-272">Application Insights might prompt you tooinstall hello Application Insights site extension toostart collecting data.</span></span> <span data-ttu-id="a8fca-273">Installation hello webbplatstillägg gör en omstart.</span><span class="sxs-lookup"><span data-stu-id="a8fca-273">Installing hello site extension causes an application restart.</span></span>

![Application Insights](media/app-service-web-tutorial-monitoring/app-service-monitor-appinsights.png)

<span data-ttu-id="a8fca-275">Application Insights har kraftfulla funktioner kan toolearn fler följa hello länkar som ingår i hello [nästa steg](#next) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="a8fca-275">Application Insights has a rich feature set, toolearn more, follow hello links included in hello [Next Steps](#next) section.</span></span>

## <span data-ttu-id="a8fca-276"><a name="next"></a>Nästa steg</span><span class="sxs-lookup"><span data-stu-id="a8fca-276"><a name="next"></a> Next steps</span></span>

 - [<span data-ttu-id="a8fca-277">Vad är Application Insights</span><span class="sxs-lookup"><span data-stu-id="a8fca-277">What is Application Insights</span></span>](..\application-insights\app-insights-overview.md)
 - [<span data-ttu-id="a8fca-278">Övervaka Azure web app prestanda med Application Insights</span><span class="sxs-lookup"><span data-stu-id="a8fca-278">Monitor Azure web app performance with Application Insights</span></span>](..\application-insights\app-insights-azure-web-apps.md)
 - [<span data-ttu-id="a8fca-279">Övervaka tillgänglighet och svarstider för alla webbplatser med Application Insights</span><span class="sxs-lookup"><span data-stu-id="a8fca-279">Monitor availability and responsiveness of any web site with Application Insights</span></span>](..\application-insights\app-insights-monitor-web-app-availability.md)
