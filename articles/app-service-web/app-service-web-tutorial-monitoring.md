---
title: "Övervaka ett webbprogram | Microsoft Docs"
description: "Lär dig hur du ställer in övervakning på ditt webbprogram"
services: App-Service
keywords: 
author: btardif
ms.author: byvinyal
ms.date: 04/04/2017
ms.topic: article
ms.service: app-service-web
ms.openlocfilehash: 29df824062d00e01b786533033097948c008588f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="monitor-app-service"></a><span data-ttu-id="da276-103">Övervaka App Service</span><span class="sxs-lookup"><span data-stu-id="da276-103">Monitor App Service</span></span>
<span data-ttu-id="da276-104">Den här självstudiekursen vägleder dig genom att övervaka din app och använda de inbyggda plattformsverktyg för att lösa problem när de inträffar.</span><span class="sxs-lookup"><span data-stu-id="da276-104">This tutorial walks you through monitoring your app and using the built-in platform tools to solve problems when they occur.</span></span>

<span data-ttu-id="da276-105">Varje avsnitt i det här dokumentet går över en specifik funktion.</span><span class="sxs-lookup"><span data-stu-id="da276-105">Each section of this document goes over a specific feature.</span></span> <span data-ttu-id="da276-106">Tillsammans med funktionerna kan du:</span><span class="sxs-lookup"><span data-stu-id="da276-106">Using the features together let you:</span></span>
- <span data-ttu-id="da276-107">Identifierar ett problem i din app.</span><span class="sxs-lookup"><span data-stu-id="da276-107">Identifying an issue in your app.</span></span>
- <span data-ttu-id="da276-108">Avgör om problemet beror på din kod eller plattform.</span><span class="sxs-lookup"><span data-stu-id="da276-108">Determining when the issue is caused by your code or the platform.</span></span>
- <span data-ttu-id="da276-109">Begränsa källan till problemet i din kod.</span><span class="sxs-lookup"><span data-stu-id="da276-109">Narrow down the source of the problem in your code.</span></span>
- <span data-ttu-id="da276-110">Felsökning och åtgärda problemet.</span><span class="sxs-lookup"><span data-stu-id="da276-110">Debugging and fixing the issue.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="da276-111">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="da276-111">Before you begin</span></span>
- <span data-ttu-id="da276-112">Du behöver ett webbprogram för att övervaka och följ stegen nedan.</span><span class="sxs-lookup"><span data-stu-id="da276-112">You need a Web App to monitor and follow the outlined steps.</span></span>
    - <span data-ttu-id="da276-113">Du kan skapa ett program följer du stegen som beskrivs i den [skapa en ASP.NET-app i Azure med SQL Database](app-service-web-tutorial-dotnet-sqldatabase.md) kursen.</span><span class="sxs-lookup"><span data-stu-id="da276-113">You can create an application following the steps described in the [Create an ASP.NET app in Azure with SQL Database](app-service-web-tutorial-dotnet-sqldatabase.md) tutorial.</span></span>

- <span data-ttu-id="da276-114">Om du vill testa **fjärrfelsökning** för din app behöver du Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="da276-114">If you want to try out **Remote Debugging** of your application, you need Visual Studio.</span></span>
    - <span data-ttu-id="da276-115">Om du inte redan har Visual Studio 2017 installerat, du kan hämta och använda den kostnadsfria [Visual Studio 2017 Community Edition](https://www.visualstudio.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="da276-115">If you don’t already have Visual Studio 2017 installed, you can download and use the free [Visual Studio 2017 Community Edition](https://www.visualstudio.com/downloads/).</span></span>
    - <span data-ttu-id="da276-116">Se till att du aktiverar **Azure-utveckling** under installationen av Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="da276-116">Make sure that you enable **Azure development** during the Visual Studio setup.</span></span>

## <span data-ttu-id="da276-117"><a name="metrics"></a>Steg 1 – Visa mått</span><span class="sxs-lookup"><span data-stu-id="da276-117"><a name="metrics"></a> Step 1 - View metrics</span></span>
<span data-ttu-id="da276-118">**Mått** är användbar för att förstå:</span><span class="sxs-lookup"><span data-stu-id="da276-118">**Metrics** are useful to understand:</span></span>
- <span data-ttu-id="da276-119">Programmets hälsotillstånd</span><span class="sxs-lookup"><span data-stu-id="da276-119">Application health</span></span>
- <span data-ttu-id="da276-120">App-prestanda</span><span class="sxs-lookup"><span data-stu-id="da276-120">App performance</span></span>
- <span data-ttu-id="da276-121">Resursförbrukning</span><span class="sxs-lookup"><span data-stu-id="da276-121">Resource consumption</span></span>

<span data-ttu-id="da276-122">När du undersöker ett problem med programmet, är granska mått ett bra ställe att börja.</span><span class="sxs-lookup"><span data-stu-id="da276-122">When investigating an application issue, reviewing metrics is a good place to start.</span></span> <span data-ttu-id="da276-123">Azure-portalen har ett snabbt sätt att visuellt inspektera mätvärden för din app med hjälp av **Azure-Monitor**.</span><span class="sxs-lookup"><span data-stu-id="da276-123">Azure portal has a quick way to visually inspect the metrics of your app using **Azure Monitor**.</span></span>

<span data-ttu-id="da276-124">Mått ger en historiköversikt över flera viktiga aggregeringar för din app.</span><span class="sxs-lookup"><span data-stu-id="da276-124">Metrics provide a historical view across several key aggregations for your app.</span></span> <span data-ttu-id="da276-125">För alla appar som finns i app service, bör du övervaka både Web App och programtjänstplanen.</span><span class="sxs-lookup"><span data-stu-id="da276-125">For any app hosted in app service, you should monitor both the Web App and the App Service plan.</span></span>

> [!NOTE]
> <span data-ttu-id="da276-126">En App Service-plan representerar den samling fysiska resurser som dina appar finns i.</span><span class="sxs-lookup"><span data-stu-id="da276-126">An App Service plan represents the collection of physical resources used to host your apps.</span></span> <span data-ttu-id="da276-127">Alla program som har tilldelats en App Service-plan delar de resurser som definierats av planen. Det innebär att du kan minska kostnaderna när du har flera appar i en plan.</span><span class="sxs-lookup"><span data-stu-id="da276-127">All applications assigned to an App Service plan share the resources defined by it allowing you to save cost when hosting multiple apps.</span></span>
>
> <span data-ttu-id="da276-128">App Service-planer definierar följande:</span><span class="sxs-lookup"><span data-stu-id="da276-128">App Service plans define:</span></span>
> * <span data-ttu-id="da276-129">Region: Nordeuropa, östra USA, Sydostasien, osv.</span><span class="sxs-lookup"><span data-stu-id="da276-129">Region: North Europe, East US, Southeast Asia, etc.</span></span>
> * <span data-ttu-id="da276-130">Instansstorleken: Liten, Medium, Large och så vidare.</span><span class="sxs-lookup"><span data-stu-id="da276-130">Instance Size: Small, Medium, Large, etc.</span></span>
> * <span data-ttu-id="da276-131">Skalningsantalet: en, två eller tre instanser, etc.</span><span class="sxs-lookup"><span data-stu-id="da276-131">Scale Count: one, two, or three instances, etc.</span></span>
> * <span data-ttu-id="da276-132">SKU: Ledigt delade, Basic, Standard, Premium, osv.</span><span class="sxs-lookup"><span data-stu-id="da276-132">SKU: Free, Shared, Basic, Standard, Premium, etc.</span></span>

<span data-ttu-id="da276-133">Om du vill granska mått för ditt webbprogram, gå till den **översikt** bladet för den app du vill övervaka.</span><span class="sxs-lookup"><span data-stu-id="da276-133">To review metrics for your Web App, go to the **Overview** blade of the app you want to monitor.</span></span> <span data-ttu-id="da276-134">Härifrån kan du visa ett diagram för din app mått som en **övervakning panelen**.</span><span class="sxs-lookup"><span data-stu-id="da276-134">From here, you can view a chart for your app's metrics as a **Monitoring tile**.</span></span> <span data-ttu-id="da276-135">Klicka på panelen om du vill redigera och konfigurera vilka mått att visa och tidsintervallet för att visa.</span><span class="sxs-lookup"><span data-stu-id="da276-135">Click the tile to edit and configure what metrics to view and the time range to display.</span></span>

<span data-ttu-id="da276-136">Som standard innehåller resursbladet en för programmet begäranden och fel för den senaste timmen.</span><span class="sxs-lookup"><span data-stu-id="da276-136">By default the resource blade provides a view for the Application Requests and errors for the last hour.</span></span>
<span data-ttu-id="da276-137">![Övervaka App](media/app-service-web-tutorial-monitoring/app-service-monitor.png)</span><span class="sxs-lookup"><span data-stu-id="da276-137">![Monitor App](media/app-service-web-tutorial-monitoring/app-service-monitor.png)</span></span>

<span data-ttu-id="da276-138">Som du ser i exemplet har vi ett program som genererar många **HTTP-fel**.</span><span class="sxs-lookup"><span data-stu-id="da276-138">As you can see in the example, we have an application that is generating many **HTTP Server Errors**.</span></span> <span data-ttu-id="da276-139">Hög volym av fel är den första uppgift som vi behöver undersöka det här programmet.</span><span class="sxs-lookup"><span data-stu-id="da276-139">The high volume of errors is the first indication we need to investigate this application.</span></span>

> [!TIP]
> <span data-ttu-id="da276-140">Lär dig mer om Azure-Monitor med följande länkar:</span><span class="sxs-lookup"><span data-stu-id="da276-140">Learn more about Azure Monitor with the following links:</span></span>
> - [<span data-ttu-id="da276-141">Kom igång med Azure-Monitor</span><span class="sxs-lookup"><span data-stu-id="da276-141">Get started with Azure Monitor</span></span>](..\monitoring-and-diagnostics\monitoring-overview.md)
> - [<span data-ttu-id="da276-142">Azure mått</span><span class="sxs-lookup"><span data-stu-id="da276-142">Azure Metrics</span></span>](..\monitoring-and-diagnostics\monitoring-overview-metrics.md)
> - [<span data-ttu-id="da276-143">Stöds mått med Azure-Monitor</span><span class="sxs-lookup"><span data-stu-id="da276-143">Supported metrics with Azure Monitor</span></span>](..\monitoring-and-diagnostics\monitoring-supported-metrics.md)
> - [<span data-ttu-id="da276-144">Azure instrumentpaneler</span><span class="sxs-lookup"><span data-stu-id="da276-144">Azure Dashboards</span></span>](..\azure-portal\azure-portal-dashboards.md)

## <span data-ttu-id="da276-145"><a name="alerts"></a>Steg 2 – konfigurera aviseringar</span><span class="sxs-lookup"><span data-stu-id="da276-145"><a name="alerts"></a> Step 2 - Configure Alerts</span></span>
<span data-ttu-id="da276-146">**Aviseringar** kan konfigureras att utlösas vid särskilda villkor för din app.</span><span class="sxs-lookup"><span data-stu-id="da276-146">**Alerts** can be configured to trigger on specific conditions for your app.</span></span>

<span data-ttu-id="da276-147">I [steg 1 – Visa mått](#metrics), vi såg att programmet har ett stort antal fel.</span><span class="sxs-lookup"><span data-stu-id="da276-147">In [Step 1 - View metrics](#metrics), we saw that the application had a high number of errors.</span></span>

<span data-ttu-id="da276-148">Kan konfigurera en avisering för att automatiskt få meddelanden när ett fel uppstår.</span><span class="sxs-lookup"><span data-stu-id="da276-148">Lets configure an alert to automatically get notified when errors occur.</span></span> <span data-ttu-id="da276-149">I det här fallet vill vi att aviseringen ska skicka och e-post varje gång antalet HTTP-50 X fel färdas över ett visst tröskelvärde.</span><span class="sxs-lookup"><span data-stu-id="da276-149">In this case, we want the alert to send and e-mail every time the number of HTTP 50X errors goes over a certain threshold.</span></span>

<span data-ttu-id="da276-150">Om du vill skapa en avisering, gå till **övervakning** > **aviseringar** och på **[+] Lägg till avisering**.</span><span class="sxs-lookup"><span data-stu-id="da276-150">To create an alert, navigate to **Monitoring** > **Alerts** and click **[+] Add Alert**.</span></span>

![Aviseringar](media/app-service-web-tutorial-monitoring/app-service-monitor-alerts.png)

<span data-ttu-id="da276-152">Ange värden för aviseringskonfigurationen:</span><span class="sxs-lookup"><span data-stu-id="da276-152">Provide values for the Alert configuration:</span></span>
- <span data-ttu-id="da276-153">**Resurs:** platsen att övervaka med aviseringen.</span><span class="sxs-lookup"><span data-stu-id="da276-153">**Resource:** The site to monitor with the alert.</span></span>
- <span data-ttu-id="da276-154">**Namn:** ett namn för aviseringen i det här fallet: *hög HTTP 50 X*.</span><span class="sxs-lookup"><span data-stu-id="da276-154">**Name:** A name for your alert, in this case: *High HTTP 50X*.</span></span>
- <span data-ttu-id="da276-155">**Beskrivning:** oformaterad text förklaring av aviseringen är ute på.</span><span class="sxs-lookup"><span data-stu-id="da276-155">**Description:** Plain text explanation of what this alert is looking at.</span></span>
- <span data-ttu-id="da276-156">**Varning vid:** aviseringar kan titta på mått eller händelser, för det här exemplet vi tittar på mått.</span><span class="sxs-lookup"><span data-stu-id="da276-156">**Alert on:** Alerts can look at Metrics or Events, for this example we are looking at metrics.</span></span>
- <span data-ttu-id="da276-157">**Mått:** vilka mått som ska övervakas, i det här fallet: *HTTP-fel*.</span><span class="sxs-lookup"><span data-stu-id="da276-157">**Metric:** What metric to monitor, in this case: *HTTP Server Errors*.</span></span>
- <span data-ttu-id="da276-158">**Villkor:** när aviseringen i det här fallet väljer den *större än* alternativet.</span><span class="sxs-lookup"><span data-stu-id="da276-158">**Condition:** When to alert, in this case select the *greater than* option.</span></span>
- <span data-ttu-id="da276-159">**Tröskelvärde:** vad är värdet att söka efter, i det här fallet: *400*.</span><span class="sxs-lookup"><span data-stu-id="da276-159">**Threshold:** What is value to look for, in this case: *400*.</span></span>
- <span data-ttu-id="da276-160">**Period:** aviseringar fungerar över medelvärdet för ett mått.</span><span class="sxs-lookup"><span data-stu-id="da276-160">**Period:** Alerts operate over the average value of a metric.</span></span> <span data-ttu-id="da276-161">Mindre tidsperioder ge känsligare aviseringar.</span><span class="sxs-lookup"><span data-stu-id="da276-161">Smaller periods of time yield more sensitive alerts.</span></span> <span data-ttu-id="da276-162">i det här fallet vi tittar på *5 minuter*.</span><span class="sxs-lookup"><span data-stu-id="da276-162">in this case we are looking at *5 Minutes*.</span></span>
- <span data-ttu-id="da276-163">**E-ägare och deltagare:** i det här fallet: *aktiverad*.</span><span class="sxs-lookup"><span data-stu-id="da276-163">**Email Owners and contributors:** in this case: *Enabled*.</span></span>

<span data-ttu-id="da276-164">Nu när aviseringen har skapats skickas ett e-postmeddelande varje gång appen går över den konfigurerade tröskeln.</span><span class="sxs-lookup"><span data-stu-id="da276-164">Now that the alert is created an email is sent every time the app goes over the configured threshold.</span></span> <span data-ttu-id="da276-165">Aktiva aviseringar kan också ses i Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="da276-165">Active alerts can also be reviewed in the Azure portal.</span></span>

![Utlösta varningar](media/app-service-web-tutorial-monitoring/app-service-monitor-alerts-triggered.png)


> [!TIP]
> <span data-ttu-id="da276-167">Läs mer om Azure aviseringar med följande länkar:</span><span class="sxs-lookup"><span data-stu-id="da276-167">Learn more about Azure Alerts with the following links:</span></span>
> - [<span data-ttu-id="da276-168">Vad är aviseringar i Microsoft Azure</span><span class="sxs-lookup"><span data-stu-id="da276-168">What are alerts in Microsoft Azure</span></span>](..\monitoring-and-diagnostics\monitoring-overview-alerts.md)
> - [<span data-ttu-id="da276-169">Utför en åtgärd på mått</span><span class="sxs-lookup"><span data-stu-id="da276-169">Take Action On Metrics</span></span>](..\monitoring-and-diagnostics\monitoring-overview.md)
> - [<span data-ttu-id="da276-170">Skapa mått aviseringar</span><span class="sxs-lookup"><span data-stu-id="da276-170">Create metric alerts</span></span>](..\monitoring-and-diagnostics\insights-alerts-portal.md)

## <span data-ttu-id="da276-171"><a name="companion"></a>Steg 3 – Apptjänst tillhörande</span><span class="sxs-lookup"><span data-stu-id="da276-171"><a name="companion"></a> Step 3 - App Service Companion</span></span>
<span data-ttu-id="da276-172">**Apptjänst tillhörande** erbjuder ett enkelt sätt att övervaka din app med en inbyggd upplevelse i din mobila enhet (iOS eller Android).</span><span class="sxs-lookup"><span data-stu-id="da276-172">**App Service companion** offers a convenient way to monitor your app with a native experience in your mobile device (iOS or Android).</span></span>

<span data-ttu-id="da276-173">Använd medföljande App Service till:</span><span class="sxs-lookup"><span data-stu-id="da276-173">Use App Service Companion to:</span></span>
- <span data-ttu-id="da276-174">Granska program-mått</span><span class="sxs-lookup"><span data-stu-id="da276-174">Review application metrics</span></span>
- <span data-ttu-id="da276-175">Granska och reagera på programaviseringar och rekommendationer</span><span class="sxs-lookup"><span data-stu-id="da276-175">Review and act on application alerts and recommendations</span></span>
- <span data-ttu-id="da276-176">Grundläggande felsökning (Bläddra, starta, stoppa, starta om appen)</span><span class="sxs-lookup"><span data-stu-id="da276-176">Perform basic troubleshooting (browse, start, stop, restart the app)</span></span>
- <span data-ttu-id="da276-177">Hämta push-meddelanden för kritiska händelser.</span><span class="sxs-lookup"><span data-stu-id="da276-177">Get push notifications for critical events.</span></span>

![Medföljande App Service](media/app-service-web-tutorial-monitoring/app-service-companion.png)

<span data-ttu-id="da276-179">[![App Service medföljande App Store](media/app-service-web-tutorial-monitoring/app-service-companion-appStore.png)](https://itunes.apple.com/app/azure-app-service-companion/id1146659260)
[![Apptjänst medföljande Google Play](media/app-service-web-tutorial-monitoring/app-service-companion-googlePlay.png)](https://play.google.com/store/apps/details?id=azureApps.AzureApps)</span><span class="sxs-lookup"><span data-stu-id="da276-179">[![App Service Companion App Store](media/app-service-web-tutorial-monitoring/app-service-companion-appStore.png)](https://itunes.apple.com/app/azure-app-service-companion/id1146659260)
[![App Service Companion Google Play](media/app-service-web-tutorial-monitoring/app-service-companion-googlePlay.png)](https://play.google.com/store/apps/details?id=azureApps.AzureApps)</span></span>

<span data-ttu-id="da276-180">Du kan installera Apptjänst tillhörande från den [Appbutiken](https://itunes.apple.com/app/azure-app-service-companion/id1146659260) eller [Google Play](https://play.google.com/store/apps/details?id=azureApps.AzureApps)</span><span class="sxs-lookup"><span data-stu-id="da276-180">You can install App Service companion from the [App Store](https://itunes.apple.com/app/azure-app-service-companion/id1146659260) or [Google Play](https://play.google.com/store/apps/details?id=azureApps.AzureApps)</span></span>

## <span data-ttu-id="da276-181"><a name="diagnose"></a>Steg 4 – diagnostisera och lösa problem</span><span class="sxs-lookup"><span data-stu-id="da276-181"><a name="diagnose"></a> Step 4 - Diagnose and solve problems</span></span>
<span data-ttu-id="da276-182">**Diagnostisera och lösa problem** hjälper du separera programproblem formuläret plattform problem.</span><span class="sxs-lookup"><span data-stu-id="da276-182">**Diagnose and solve problems** helps you separate application issues form platform issues.</span></span> <span data-ttu-id="da276-183">Det kan också föreslå möjliga åtgärder för att få ditt webbprogram till felfritt.</span><span class="sxs-lookup"><span data-stu-id="da276-183">It can also suggest possible mitigations to get your Web App back to healthy.</span></span>

![Diagnostisera och lösa problem](media/app-service-web-tutorial-monitoring/app-service-monitor-diagnosis.png)

<span data-ttu-id="da276-185">Fortsätter med de föregående stegen exempel formuläret se vi att programmet har tagits med availably problem.</span><span class="sxs-lookup"><span data-stu-id="da276-185">Continuing with the example form previous steps, we can see that the application has been having availably issues.</span></span> <span data-ttu-id="da276-186">Tillgängliga plattformar har däremot inte flyttats från 100%.</span><span class="sxs-lookup"><span data-stu-id="da276-186">In contrast, the platform availability has not moved from 100%.</span></span>

<span data-ttu-id="da276-187">När appen är med problemet och plattform förblir upp, är det en tydlig indikation på att vi arbetar med ett problem med programmet.</span><span class="sxs-lookup"><span data-stu-id="da276-187">When the app is having issue and the platform stays up, it's a clear indication that we are dealing with an application issue.</span></span>

## <span data-ttu-id="da276-188"><a name="logging"></a>Steg 5 - loggning</span><span class="sxs-lookup"><span data-stu-id="da276-188"><a name="logging"></a> Step 5 - Logging</span></span>
<span data-ttu-id="da276-189">Nu när vi har minskar antalet fel på ett problem med programmet, titta vi på program- och loggfiler för mer information.</span><span class="sxs-lookup"><span data-stu-id="da276-189">Now that we have narrowed down the failures to an application issue, we can look at the application and server logs to get more information.</span></span>

<span data-ttu-id="da276-190">Loggning kan du samla in både **Programdiagnostik** och **Web Serverdiagnostik** loggar för Webbappen.</span><span class="sxs-lookup"><span data-stu-id="da276-190">Logging allows you to collect both **Application Diagnostics** and **Web Server Diagnostics** logs for your Web App.</span></span>

### <a name="application-diagnostics"></a><span data-ttu-id="da276-191">Application Diagnostics</span><span class="sxs-lookup"><span data-stu-id="da276-191">Application Diagnostics</span></span>
<span data-ttu-id="da276-192">Programdiagnostik kan du fånga in spårningar som genereras av programmet vid körning.</span><span class="sxs-lookup"><span data-stu-id="da276-192">Application diagnostics allows you to capture traces produced by the application at runtime.</span></span>

<span data-ttu-id="da276-193">Lägga till spårning i tillämpningsprogrammet avsevärt förbättrar möjligheten att felsöka och pekpunkt problem.</span><span class="sxs-lookup"><span data-stu-id="da276-193">Adding tracing to your application greatly improves your ability to debug and pin-point issues.</span></span>

<span data-ttu-id="da276-194">I ASP.NET, du kan logga in programspårningar med [System.Diagnostics.Trace klassen](https://msdn.microsoft.com/library/system.diagnostics.trace.aspx) att generera händelser som samlas in av logg-infrastruktur.</span><span class="sxs-lookup"><span data-stu-id="da276-194">In ASP.NET, you can log application traces using [System.Diagnostics.Trace class](https://msdn.microsoft.com/library/system.diagnostics.trace.aspx) to generate events that are captured by the log infrastructure.</span></span> <span data-ttu-id="da276-195">Du kan också ange allvarlighetsgraden spårningen för enklare filtrering.</span><span class="sxs-lookup"><span data-stu-id="da276-195">You can also specify the severity of the trace for easier filtering.</span></span>

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
<span data-ttu-id="da276-196">Om du vill aktivera programloggning går du till **övervakning** > **diagnostikloggar** och aktivera programloggning med alternativen.</span><span class="sxs-lookup"><span data-stu-id="da276-196">To enable Application logging Go to **Monitoring** > **Diagnostic Logs** and Enable Application Logging using the toggles.</span></span>

![Övervaka App](media/app-service-web-tutorial-monitoring/app-service-monitor-applogs.png)

<span data-ttu-id="da276-198">Programloggarna kan lagras i filsystemet för ditt webbprogram eller skickas till blob storage.</span><span class="sxs-lookup"><span data-stu-id="da276-198">Application logs can be stored to your Web App's file system or pushed out to blob storage.</span></span> <span data-ttu-id="da276-199">Scenarier för produktion, har vi rekommenderar för att använda blob storage.</span><span class="sxs-lookup"><span data-stu-id="da276-199">For production scenarios, it's recommended to use blob storage.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="da276-200">Om du aktiverar loggning påverkar dina program prestanda- och användning.</span><span class="sxs-lookup"><span data-stu-id="da276-200">Enabling logging has an impact on your application performance and resource utilization.</span></span> <span data-ttu-id="da276-201">För produktion scenarier rekommenderas felloggar.</span><span class="sxs-lookup"><span data-stu-id="da276-201">For production scenarios, error logs are recommended.</span></span> <span data-ttu-id="da276-202">Aktivera endast mer utförlig loggning när du undersöker problem.</span><span class="sxs-lookup"><span data-stu-id="da276-202">Only enable more verbose logging when investigating issues.</span></span>

 ### <a name="web-server-diagnostics"></a><span data-ttu-id="da276-203">Web Serverdiagnostik</span><span class="sxs-lookup"><span data-stu-id="da276-203">Web Server Diagnostics</span></span>
<span data-ttu-id="da276-204">Webbserverloggarna genereras även om din app inte instrumenterats.</span><span class="sxs-lookup"><span data-stu-id="da276-204">Web Server logs are generated even if your app is not instrumented.</span></span> <span data-ttu-id="da276-205">Apptjänst kan samla in tre olika typer av server-loggar:</span><span class="sxs-lookup"><span data-stu-id="da276-205">App Service can collect three different types of server logs:</span></span>

- <span data-ttu-id="da276-206">**Web Server-loggning**</span><span class="sxs-lookup"><span data-stu-id="da276-206">**Web Server Logging**</span></span>
    - <span data-ttu-id="da276-207">Information om HTTP-transaktioner med hjälp av den [W3C utökat loggfilsformat](https://msdn.microsoft.com/library/windows/desktop/aa814385.aspx).</span><span class="sxs-lookup"><span data-stu-id="da276-207">Information about HTTP transactions using the [W3C extended log file format](https://msdn.microsoft.com/library/windows/desktop/aa814385.aspx).</span></span>
    - <span data-ttu-id="da276-208">Användbart när du fastställer övergripande plats mätvärden, till exempel antalet förfrågningar som hanteras eller hur många förfrågningar som kommer från en specifik IP-adress.</span><span class="sxs-lookup"><span data-stu-id="da276-208">Useful when determining overall site metrics such as the number of requests handled or how many requests are from a specific IP address.</span></span>
- <span data-ttu-id="da276-209">**Detaljerad felloggning**</span><span class="sxs-lookup"><span data-stu-id="da276-209">**Detailed Error Logging**</span></span>
    - <span data-ttu-id="da276-210">Detaljerad information om fel för HTTP-statuskoder som indikerar att en (statuskod 400 eller högre).</span><span class="sxs-lookup"><span data-stu-id="da276-210">Detailed error information for HTTP status codes that indicate a failure (status code 400 or greater).</span></span>
    - [<span data-ttu-id="da276-211">Mer information om detaljerad felloggning</span><span class="sxs-lookup"><span data-stu-id="da276-211">Learn more about detailed error logging</span></span>](https://www.iis.net/learn/troubleshoot/diagnosing-http-errors/how-to-use-http-detailed-errors-in-iis)
- <span data-ttu-id="da276-212">**Spårning av misslyckade begäranden**</span><span class="sxs-lookup"><span data-stu-id="da276-212">**Failed Request Tracing**</span></span>
    - <span data-ttu-id="da276-213">Detaljerad information om misslyckade förfrågningar, inklusive en spårning av IIS-komponenter som används för att bearbeta begäran och tidsåtgång i varje komponent.</span><span class="sxs-lookup"><span data-stu-id="da276-213">Detailed information on failed requests, including a trace of the IIS components used to process the request and the time taken in each component.</span></span>
    - <span data-ttu-id="da276-214">Misslyckade begäranden loggar är användbart när du försöker isolera vad som orsakar ett specifikt HTTP-fel.</span><span class="sxs-lookup"><span data-stu-id="da276-214">Failed request logs are useful when trying to isolate what is causing a specific HTTP error.</span></span>
    - [<span data-ttu-id="da276-215">Mer information om spårning av misslyckade begäranden</span><span class="sxs-lookup"><span data-stu-id="da276-215">Learn more about failed request tracing</span></span>](https://www.iis.net/learn/troubleshoot/using-failed-request-tracing/troubleshooting-failed-requests-using-tracing-in-iis)

<span data-ttu-id="da276-216">Så här aktiverar du loggning Server:</span><span class="sxs-lookup"><span data-stu-id="da276-216">To enable Server logging:</span></span>
- <span data-ttu-id="da276-217">Gå till **övervakning** > **diagnostikloggar**.</span><span class="sxs-lookup"><span data-stu-id="da276-217">go to **Monitoring** > **Diagnostic Logs**.</span></span>
- <span data-ttu-id="da276-218">Aktivera de olika typerna av Web Serverdiagnostik med alternativen.</span><span class="sxs-lookup"><span data-stu-id="da276-218">Enable the different types of Web Server Diagnostics using the toggles.</span></span>

![Övervaka App](media/app-service-web-tutorial-monitoring/app-service-monitor-serverlogs.png)

> [!IMPORTANT]
> <span data-ttu-id="da276-220">Om du aktiverar loggning påverkar dina program prestanda- och användning.</span><span class="sxs-lookup"><span data-stu-id="da276-220">Enabling logging has an impact on your application performance and resource utilization.</span></span> <span data-ttu-id="da276-221">För produktion scenarier rekommenderas felloggar, aktivera endast mer utförlig loggning när du undersöker problem.</span><span class="sxs-lookup"><span data-stu-id="da276-221">For Production Scenarios, Error logs are recommended, Only Enable more verbose logging when investigating issues.</span></span>

### <a name="accessing-logs"></a><span data-ttu-id="da276-222">Åtkomst till loggar</span><span class="sxs-lookup"><span data-stu-id="da276-222">Accessing Logs</span></span>
<span data-ttu-id="da276-223">Loggfilerna lagras i blob storage kan nås med hjälp av Azure Lagringsutforskaren.</span><span class="sxs-lookup"><span data-stu-id="da276-223">Logs stored in blob storage are accessed using Azure Storage Explorer.</span></span> <span data-ttu-id="da276-224">Loggar som finns i Webbappens filsystem kan nås via FTP under följande sökvägar:</span><span class="sxs-lookup"><span data-stu-id="da276-224">Logs stored in the Web App's filesystem are accessed through FTP under the following paths:</span></span>

- <span data-ttu-id="da276-225">**Programloggarna** - `%HOME%/LogFiles/Application/`.</span><span class="sxs-lookup"><span data-stu-id="da276-225">**Application logs** - `%HOME%/LogFiles/Application/`.</span></span>
    - <span data-ttu-id="da276-226">Den här mappen innehåller en eller flera textfiler som innehåller information som genereras av programloggning.</span><span class="sxs-lookup"><span data-stu-id="da276-226">This folder contains one or more text files containing information produced by application logging.</span></span>
- <span data-ttu-id="da276-227">**Det gick inte begäranden** - `%HOME%/LogFiles/W3SVC#########/`.</span><span class="sxs-lookup"><span data-stu-id="da276-227">**Failed Request Traces** - `%HOME%/LogFiles/W3SVC#########/`.</span></span>
    - <span data-ttu-id="da276-228">Den här mappen innehåller en XSL-fil och en eller flera XML-filer.</span><span class="sxs-lookup"><span data-stu-id="da276-228">This folder contains an XSL file and one or more XML files.</span></span>
- <span data-ttu-id="da276-229">**Detaljerade felloggar** - `%HOME%/LogFiles/DetailedErrors/`.</span><span class="sxs-lookup"><span data-stu-id="da276-229">**Detailed Error Logs** - `%HOME%/LogFiles/DetailedErrors/`.</span></span>
    - <span data-ttu-id="da276-230">Den här mappen innehåller en eller flera .htm-filer med omfattande information om HTTP-fel som genereras av din app.</span><span class="sxs-lookup"><span data-stu-id="da276-230">This folder contains one or more .htm files with extensive information on HTTP errors generated by your app.</span></span>
- <span data-ttu-id="da276-231">**Web Server-loggar** - `%HOME%/LogFiles/http/RawLogs`.</span><span class="sxs-lookup"><span data-stu-id="da276-231">**Web Server Logs** - `%HOME%/LogFiles/http/RawLogs`.</span></span>
    - <span data-ttu-id="da276-232">Den här mappen innehåller en eller flera textfiler som formaterats med W3C utökat loggfilsformat.</span><span class="sxs-lookup"><span data-stu-id="da276-232">This folder contains one or more text files formatted using the W3C extended log file format.</span></span>

## <span data-ttu-id="da276-233"><a name="streaming"></a>Steg 6 - loggen strömning</span><span class="sxs-lookup"><span data-stu-id="da276-233"><a name="streaming"></a> Step 6 - Log Streaming</span></span>
<span data-ttu-id="da276-234">Direktuppspelningsloggar är praktiskt när du felsöker ett program eftersom det sparar tid jämfört med [åtkomst till loggarna](#Accessing-Logs) via FTP.</span><span class="sxs-lookup"><span data-stu-id="da276-234">Streaming logs are convenient when debugging an application since it saves time compared to [accessing the logs](#Accessing-Logs) through FTP.</span></span>

<span data-ttu-id="da276-235">Apptjänst kan strömma **programloggarna** och **Webbserverloggar** när de skapas.</span><span class="sxs-lookup"><span data-stu-id="da276-235">App Service can stream **Application Logs** and **Web Server Logs** as they are generated.</span></span>

> [!TIP]
> <span data-ttu-id="da276-236">Innan du försöker att strömma loggar, se till att du har aktiverat att samla in loggar som beskrivs i den [loggning](#logging) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="da276-236">Before trying to stream logs, make sure you have enabled collecting logs as described in the [Logging](#logging) section.</span></span>

<span data-ttu-id="da276-237">Gå till för att strömma loggar **övervakning**> **loggström**.</span><span class="sxs-lookup"><span data-stu-id="da276-237">To stream logs, go to **Monitoring**> **Log Stream**.</span></span> <span data-ttu-id="da276-238">Välj **programloggarna** eller **Web server-loggar** beroende på vilken information du söker efter.</span><span class="sxs-lookup"><span data-stu-id="da276-238">Select **Application Logs** or **Web server logs** depending what information you are looking for.</span></span> <span data-ttu-id="da276-239">Härifrån kan du även pausa, starta om och rensa bufferten.</span><span class="sxs-lookup"><span data-stu-id="da276-239">From here, you can also pause, restart, and clear the buffer.</span></span>

![Direktuppspelningsloggar](media/app-service-web-tutorial-monitoring/app-service-monitor-logstream.png)

> [!TIP]
> <span data-ttu-id="da276-241">Loggar genereras bara när det finns någon trafik i appen, du kan också öka detaljnivå loggar få fler händelser eller information.</span><span class="sxs-lookup"><span data-stu-id="da276-241">Logs are only generated when there is traffic on the app, you can also increase the verbosity of logs to get more events or information.</span></span>

## <span data-ttu-id="da276-242"><a name="remote"></a>Steg 7 – fjärrfelsökning</span><span class="sxs-lookup"><span data-stu-id="da276-242"><a name="remote"></a> Step 7 - Remote Debugging</span></span>
<span data-ttu-id="da276-243">När du har pin-pekar källan för program-problem, använda **fjärrfelsökning** kan du gå igenom koden.</span><span class="sxs-lookup"><span data-stu-id="da276-243">Once you have pin-pointed the source of the applications problems, use **Remote Debugging** to walk through the code.</span></span>

<span data-ttu-id="da276-244">Felsökning kan du bifoga en felsökare till ditt webbprogram som körs i molnet.</span><span class="sxs-lookup"><span data-stu-id="da276-244">Remote debugging lets you attach a debugger to your Web App running in the cloud.</span></span> <span data-ttu-id="da276-245">Du kan ange brytpunkter, ändra minne direkt, stega igenom koden och även ändra koden sökvägen precis som för en app som körs lokalt.</span><span class="sxs-lookup"><span data-stu-id="da276-245">You can set breakpoints, manipulate memory directly, step through code, and even change the code path just like you do for an app running locally.</span></span>

<span data-ttu-id="da276-246">Att koppla felsökaren till din app som körs i molnet:</span><span class="sxs-lookup"><span data-stu-id="da276-246">To attach the debugger to your app running in the cloud:</span></span>

- <span data-ttu-id="da276-247">Med Visual Studio 2017 kan du öppna lösning för den app du vill felsöka</span><span class="sxs-lookup"><span data-stu-id="da276-247">Using Visual Studio 2017, open the solution for the app you want to debug</span></span>
- <span data-ttu-id="da276-248">Ange vissa bromsar punkter precis som du skulle för lokal utveckling.</span><span class="sxs-lookup"><span data-stu-id="da276-248">Set some brake points just like you would for local development.</span></span>
- <span data-ttu-id="da276-249">Öppna **cloud explorer** (ctr + / -ctrl + x).</span><span class="sxs-lookup"><span data-stu-id="da276-249">Open **cloud explorer** (ctr + /, ctrl + x).</span></span>
- <span data-ttu-id="da276-250">Logga in med dina autentiseringsuppgifter för azure vid behov.</span><span class="sxs-lookup"><span data-stu-id="da276-250">Log in with your azure credentials as needed.</span></span>
- <span data-ttu-id="da276-251">Hitta den app som du vill felsöka</span><span class="sxs-lookup"><span data-stu-id="da276-251">Find the app you want to debug</span></span>
- <span data-ttu-id="da276-252">Välj **koppla felsökare** formuläret i **åtgärder** fönstret.</span><span class="sxs-lookup"><span data-stu-id="da276-252">Select **Attach Debugger** form the **Actions** pane.</span></span>

![Fjärrfelsökning](media/app-service-web-tutorial-monitoring/app-service-monitor-vsdebug.png)

<span data-ttu-id="da276-254">Visual Studio konfigurerar ditt program för fjärrfelsökning och öppnar ett webbläsarfönster som navigerar till din app.</span><span class="sxs-lookup"><span data-stu-id="da276-254">Visual Studio configures your application for remote debugging and launches a browser window that navigates to your app.</span></span> <span data-ttu-id="da276-255">Bläddra igenom din app att utlösa break punkter och stega igenom koden.</span><span class="sxs-lookup"><span data-stu-id="da276-255">Browse through your app to trigger break points and step through the code.</span></span>

> [!WARNING]
> <span data-ttu-id="da276-256">Du bör inte köra i felsökningsläge i produktion.</span><span class="sxs-lookup"><span data-stu-id="da276-256">Running in debug mode in production is not recommended.</span></span> <span data-ttu-id="da276-257">Om din produktionsprogrammet inte skalas ut att flera serverinstanser felsökning förhindra webbservern svarar på andra begäranden.</span><span class="sxs-lookup"><span data-stu-id="da276-257">If your production app is not scaled out to multiple server instances, debugging prevent the web server from responding to other requests.</span></span> <span data-ttu-id="da276-258">För felsökning för produktion, bästa resursen är en [konfigurera loggning](#logging) och [Programinsikter](#insights).</span><span class="sxs-lookup"><span data-stu-id="da276-258">For troubleshooting production problems, your best resource is to [configure logging](#logging) and [Application Insights](#insights).</span></span>



## <span data-ttu-id="da276-259"><a name="explorer"></a>Steg 8 – Processutforskaren</span><span class="sxs-lookup"><span data-stu-id="da276-259"><a name="explorer"></a> Step 8 - Process Explorer</span></span>
<span data-ttu-id="da276-260">När programmet skalas ut till mer än en instans, **processutforskaren** kan hjälpa dig att identifiera instans specifika problem.</span><span class="sxs-lookup"><span data-stu-id="da276-260">When your application is scaled out to more than one instance, **process explorer** can help you identify instance specific problems.</span></span>

<span data-ttu-id="da276-261">Använd **bearbeta Explorer** till:</span><span class="sxs-lookup"><span data-stu-id="da276-261">Use **Process Explorer** to:</span></span>

- <span data-ttu-id="da276-262">Räkna upp alla processer mellan olika instanser av din programtjänstplan.</span><span class="sxs-lookup"><span data-stu-id="da276-262">Enumerate all the processes across different instances of your App Service plan.</span></span>
- <span data-ttu-id="da276-263">Detaljnivån och visa referenser och moduler som är associerade med varje process.</span><span class="sxs-lookup"><span data-stu-id="da276-263">Drill down and view the handles and modules associated with each process.</span></span>
- <span data-ttu-id="da276-264">Visa CPU, aktiv sidmängd och antal på processnivå som hjälper dig att identifiera skenande processer trådar</span><span class="sxs-lookup"><span data-stu-id="da276-264">View CPU, Working Set, and Thread count at the process level to help you identify runaway processes</span></span>
- <span data-ttu-id="da276-265">Hitta öppna filreferenser och även avsluta en specifik process-instans.</span><span class="sxs-lookup"><span data-stu-id="da276-265">Find open file handles, and even kill a specific process instance.</span></span>

<span data-ttu-id="da276-266">Processutforskaren finns under **övervakning** > **Processutforskaren**.</span><span class="sxs-lookup"><span data-stu-id="da276-266">Process Explorer can be found under **Monitoring** > **Process Explorer**.</span></span>

![Processutforskaren](media/app-service-web-tutorial-monitoring/app-service-monitor-processexplorer.png)


## <span data-ttu-id="da276-268"><a name="insights"></a>Steg 9 – Application Insights</span><span class="sxs-lookup"><span data-stu-id="da276-268"><a name="insights"></a> Step 9 - Application Insights</span></span>
<span data-ttu-id="da276-269">**Application Insights** ger profileringsspårning för program och funktioner för avancerad övervakning för din app.</span><span class="sxs-lookup"><span data-stu-id="da276-269">**Application Insights** provides application profiling and advanced monitoring capabilities for your app.</span></span>

<span data-ttu-id="da276-270">Använda Application Insights för att identifiera och diagnostisera undantag och prestandaproblem i ditt webbprogram.</span><span class="sxs-lookup"><span data-stu-id="da276-270">Use Application Insights to detect and diagnose exceptions and performance issues in your Web App.</span></span>

<span data-ttu-id="da276-271">Du kan aktivera Application Insights för ditt webbprogram under **övervakning** > **Application Insights**</span><span class="sxs-lookup"><span data-stu-id="da276-271">You can enable Application Insights for your Web App under **Monitoring** > **Application Insights**</span></span>

> [!NOTE]
> <span data-ttu-id="da276-272">Application Insights kan en uppmaning om att installera tillägg för Application Insights-plats för att börja samla in data.</span><span class="sxs-lookup"><span data-stu-id="da276-272">Application Insights might prompt you to install the Application Insights site extension to start collecting data.</span></span> <span data-ttu-id="da276-273">Installera tillägget plats gör att en omstart.</span><span class="sxs-lookup"><span data-stu-id="da276-273">Installing the site extension causes an application restart.</span></span>

![Application Insights](media/app-service-web-tutorial-monitoring/app-service-monitor-appinsights.png)

<span data-ttu-id="da276-275">Application Insights har en kraftfulla funktioner, om du vill veta mer Följ länkarna som ingår i den [nästa steg](#next) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="da276-275">Application Insights has a rich feature set, to learn more, follow the links included in the [Next Steps](#next) section.</span></span>

## <span data-ttu-id="da276-276"><a name="next"></a>Nästa steg</span><span class="sxs-lookup"><span data-stu-id="da276-276"><a name="next"></a> Next steps</span></span>

 - [<span data-ttu-id="da276-277">Vad är Application Insights</span><span class="sxs-lookup"><span data-stu-id="da276-277">What is Application Insights</span></span>](..\application-insights\app-insights-overview.md)
 - [<span data-ttu-id="da276-278">Övervaka Azure web app prestanda med Application Insights</span><span class="sxs-lookup"><span data-stu-id="da276-278">Monitor Azure web app performance with Application Insights</span></span>](..\application-insights\app-insights-azure-web-apps.md)
 - [<span data-ttu-id="da276-279">Övervaka tillgänglighet och svarstider för alla webbplatser med Application Insights</span><span class="sxs-lookup"><span data-stu-id="da276-279">Monitor availability and responsiveness of any web site with Application Insights</span></span>](..\application-insights\app-insights-monitor-web-app-availability.md)
