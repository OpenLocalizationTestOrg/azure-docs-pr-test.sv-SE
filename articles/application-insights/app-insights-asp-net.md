---
title: "aaaSet in web app analytics för ASP.NET med Azure Application Insights | Microsoft Docs"
description: "Konfigurera prestanda-, tillgänglighets- och användningsanalyser för din lokala eller Azure-baserade ASP.NET-webbplats."
services: application-insights
documentationcenter: .net
author: CFreemanwa
manager: carmonm
ms.assetid: d0eee3c0-b328-448f-8123-f478052751db
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: get-started-article
ms.date: 05/15/2017
ms.author: bwren
ms.openlocfilehash: 61a3cdce68da48bfb9450b1d296acc1535f50a38
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="set-up-application-insights-for-your-aspnet-website"></a><span data-ttu-id="4fc26-103">Konfigurera Application Insights för din ASP.NET-webbplats</span><span class="sxs-lookup"><span data-stu-id="4fc26-103">Set up Application Insights for your ASP.NET website</span></span>

<span data-ttu-id="4fc26-104">Den här proceduren konfigurerar din ASP.NET web app toosend telemetri toohello [Azure Application Insights](app-insights-overview.md) service.</span><span class="sxs-lookup"><span data-stu-id="4fc26-104">This procedure configures your ASP.NET web app toosend telemetry toohello [Azure Application Insights](app-insights-overview.md) service.</span></span> <span data-ttu-id="4fc26-105">Den fungerar för ASP.NET-program som finns i din egen IIS-server eller i hello molnet.</span><span class="sxs-lookup"><span data-stu-id="4fc26-105">It works for ASP.NET apps that are hosted either in your own IIS server or in hello Cloud.</span></span> <span data-ttu-id="4fc26-106">Du får diagram och en kraftfull frågespråk som hjälper dig förstå hello prestanda för din app och hur personer använder plus automatiska meddelanden på fel eller prestandaproblem.</span><span class="sxs-lookup"><span data-stu-id="4fc26-106">You get charts and a powerful query language that help you understand hello performance of your app and how people are using it, plus automatic alerts on failures or performance issues.</span></span> <span data-ttu-id="4fc26-107">Många utvecklare hitta funktionerna stor som de är, men du kan också utöka och anpassa hello telemetri om du behöver.</span><span class="sxs-lookup"><span data-stu-id="4fc26-107">Many developers find these features great as they are, but you can also extend and customize hello telemetry if you need to.</span></span>

<span data-ttu-id="4fc26-108">Installationen kräver bara några klick i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="4fc26-108">Setup takes just a few clicks in Visual Studio.</span></span> <span data-ttu-id="4fc26-109">Du har hello alternativet tooavoid kostnader genom att begränsa hello telemetrivolym.</span><span class="sxs-lookup"><span data-stu-id="4fc26-109">You have hello option tooavoid charges by limiting hello volume of telemetry.</span></span> <span data-ttu-id="4fc26-110">Detta ger dig tooexperiment och felsökning eller toomonitor en plats med inte många användare.</span><span class="sxs-lookup"><span data-stu-id="4fc26-110">This allows you tooexperiment and debug, or toomonitor a site with not many users.</span></span> <span data-ttu-id="4fc26-111">När du bestämmer toogo vidare och om du vill övervaka din produktionsplatsen är enkelt tooraise hello gränsen senare.</span><span class="sxs-lookup"><span data-stu-id="4fc26-111">When you decide you want toogo ahead and monitor your production site, it's easy tooraise hello limit later.</span></span>

## <a name="before-you-start"></a><span data-ttu-id="4fc26-112">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="4fc26-112">Before you start</span></span>
<span data-ttu-id="4fc26-113">Du behöver:</span><span class="sxs-lookup"><span data-stu-id="4fc26-113">You need:</span></span>

* <span data-ttu-id="4fc26-114">Visual Studio 2013 Update 3 eller senare.</span><span class="sxs-lookup"><span data-stu-id="4fc26-114">Visual Studio 2013 update 3 or later.</span></span> <span data-ttu-id="4fc26-115">Senare versioner är att föredra.</span><span class="sxs-lookup"><span data-stu-id="4fc26-115">Later is better.</span></span>
* <span data-ttu-id="4fc26-116">En prenumeration för[Microsoft Azure](http://azure.com).</span><span class="sxs-lookup"><span data-stu-id="4fc26-116">A subscription too[Microsoft Azure](http://azure.com).</span></span> <span data-ttu-id="4fc26-117">Om din grupp eller organisation har en Azure-prenumeration, hello ägare kan lägga till dig tooit, med hjälp av din [Microsoft-konto](http://live.com).</span><span class="sxs-lookup"><span data-stu-id="4fc26-117">If your team or organization has an Azure subscription, hello owner can add you tooit, by using your [Microsoft account](http://live.com).</span></span>

<span data-ttu-id="4fc26-118">Det finns alternativ avsnitt toolook på om du är intresserad av:</span><span class="sxs-lookup"><span data-stu-id="4fc26-118">There are alternative topics toolook at if you are interested in:</span></span>

* [<span data-ttu-id="4fc26-119">Instrumentering av en webbapp under körning</span><span class="sxs-lookup"><span data-stu-id="4fc26-119">Instrumenting a web app at runtime</span></span>](app-insights-monitor-performance-live-website-now.md)
* [<span data-ttu-id="4fc26-120">Azure Cloud Services</span><span class="sxs-lookup"><span data-stu-id="4fc26-120">Azure Cloud Services</span></span>](app-insights-cloudservices.md)

## <span data-ttu-id="4fc26-121"><a name="ide"></a>Steg 1: Lägg till hello Application Insights SDK</span><span class="sxs-lookup"><span data-stu-id="4fc26-121"><a name="ide"></a> Step 1: Add hello Application Insights SDK</span></span>

<span data-ttu-id="4fc26-122">Högerklicka på ditt webbappsprojekt i Solution Explorer och välj **Lägg till** > **Application Insights Telemetry...** eller **Konfigurera Application Insights**.</span><span class="sxs-lookup"><span data-stu-id="4fc26-122">Right-click your web app project in Solution Explorer, and choose **Add** > **Application Insights Telemetry...** or **Configure Application Insights**.</span></span>

![Skärmbild av Solution Explorer med Lägg till Application Insights Telemetry markerat](./media/app-insights-asp-net/appinsights-03-addExisting.png)

<span data-ttu-id="4fc26-124">(I Visual Studio 2015 finns också alternativet-tooadd Application Insights i dialogrutan för hello nytt projekt.)</span><span class="sxs-lookup"><span data-stu-id="4fc26-124">(In Visual Studio 2015, there's also an option tooadd Application Insights in hello New Project dialog.)</span></span>

<span data-ttu-id="4fc26-125">Fortsätt toohello konfigurationssidan för Application Insights:</span><span class="sxs-lookup"><span data-stu-id="4fc26-125">Continue toohello Application Insights configuration page:</span></span>

![Skärmbild av sidan Register your app with Application Insights (Registrera din app med Application Insights)](./media/app-insights-asp-net/visual-studio-register-dialog.png)

<span data-ttu-id="4fc26-127">**a.**</span><span class="sxs-lookup"><span data-stu-id="4fc26-127">**a.**</span></span> <span data-ttu-id="4fc26-128">Välj hello-konto och att du använder tooaccess Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="4fc26-128">Select hello account and subscription that you use tooaccess Azure.</span></span>

<span data-ttu-id="4fc26-129">**b.**</span><span class="sxs-lookup"><span data-stu-id="4fc26-129">**b.**</span></span> <span data-ttu-id="4fc26-130">Välj hello resurs i Azure där du vill toosee hello data från din app.</span><span class="sxs-lookup"><span data-stu-id="4fc26-130">Select hello resource in Azure where you want toosee hello data from your app.</span></span> <span data-ttu-id="4fc26-131">Vanligtvis:</span><span class="sxs-lookup"><span data-stu-id="4fc26-131">Usually:</span></span>

* <span data-ttu-id="4fc26-132">Använd en [enkel resurs för olika komponenter](app-insights-monitor-multi-role-apps.md) i ett enda program.</span><span class="sxs-lookup"><span data-stu-id="4fc26-132">Use a [single resource for different components](app-insights-monitor-multi-role-apps.md) of a single application.</span></span> 
* <span data-ttu-id="4fc26-133">Skapa separata resurser för orelaterade program.</span><span class="sxs-lookup"><span data-stu-id="4fc26-133">Create separate resources for unrelated applications.</span></span>
 
<span data-ttu-id="4fc26-134">Om du vill tooset hello grupp eller hello resursplats där dina data lagras **konfigurera inställningar för**.</span><span class="sxs-lookup"><span data-stu-id="4fc26-134">If you want tooset hello resource group or hello location where your data is stored, click **Configure settings**.</span></span> <span data-ttu-id="4fc26-135">Resursgrupper är används toocontrol åtkomst toodata.</span><span class="sxs-lookup"><span data-stu-id="4fc26-135">Resource groups are used toocontrol access toodata.</span></span> <span data-ttu-id="4fc26-136">Till exempel om du har flera program som ingår i hello samma system, kan du placera sina Application Insights-data i hello samma resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="4fc26-136">For example, if you have several apps that form part of hello same system, you might put their Application Insights data in hello same resource group.</span></span>

<span data-ttu-id="4fc26-137">**c.**</span><span class="sxs-lookup"><span data-stu-id="4fc26-137">**c.**</span></span> <span data-ttu-id="4fc26-138">Ange ett tak på hello lediga volymen begränsningen, tooavoid avgifter.</span><span class="sxs-lookup"><span data-stu-id="4fc26-138">Set a cap at hello free data volume limit, tooavoid charges.</span></span> <span data-ttu-id="4fc26-139">Application Insights är Frigör tooa vissa telemetrivolym.</span><span class="sxs-lookup"><span data-stu-id="4fc26-139">Application Insights is free up tooa certain volume of telemetry.</span></span> <span data-ttu-id="4fc26-140">När hello resursen har skapats kan du ändra ditt val i hello portal genom att öppna **funktioner + pris** > **volym datahantering** > **dagliga volymen cap**.</span><span class="sxs-lookup"><span data-stu-id="4fc26-140">After hello resource is created, you can change your selection in hello portal by opening  **Features + pricing** > **Data volume management** > **Daily volume cap**.</span></span>

<span data-ttu-id="4fc26-141">**d.**</span><span class="sxs-lookup"><span data-stu-id="4fc26-141">**d.**</span></span> <span data-ttu-id="4fc26-142">Klicka på **registrera** toogo vidare och konfigurera Application Insights för ditt webbprogram.</span><span class="sxs-lookup"><span data-stu-id="4fc26-142">Click **Register** toogo ahead and configure Application Insights for your web app.</span></span> <span data-ttu-id="4fc26-143">Telemetri skickas toohello [Azure-portalen](https://portal.azure.com), både under felsökning och när du har publicerat din app.</span><span class="sxs-lookup"><span data-stu-id="4fc26-143">Telemetry will be sent toohello [Azure portal](https://portal.azure.com), both during debugging and after you have published your app.</span></span>

<span data-ttu-id="4fc26-144">**e.**</span><span class="sxs-lookup"><span data-stu-id="4fc26-144">**e.**</span></span> <span data-ttu-id="4fc26-145">Om du inte vill toosend telemetri toohello portal när du felsöker kan bara lägga till hello Application Insights SDK tooyour app men inte konfigurera en resurs i hello-portalen.</span><span class="sxs-lookup"><span data-stu-id="4fc26-145">If you don't want toosend telemetry toohello portal while you're debugging, just add hello Application Insights SDK tooyour app but don't configure a resource in hello portal.</span></span> <span data-ttu-id="4fc26-146">När du felsöker kommer du att kunna toosee telemetri i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="4fc26-146">You will be able toosee telemetry in Visual Studio while you are debugging.</span></span> <span data-ttu-id="4fc26-147">Senare, du kan gå tillbaka till sidan för konfiguration av toothis eller du kan vänta tills du har distribuerat din app och [växla telemetri vid körning](app-insights-monitor-performance-live-website-now.md).</span><span class="sxs-lookup"><span data-stu-id="4fc26-147">Later, you can return toothis configuration page, or you could wait until after you have deployed your app and [switch on telemetry at run time](app-insights-monitor-performance-live-website-now.md).</span></span>


## <span data-ttu-id="4fc26-148"><a name="run"></a> Steg 2: Kör appen</span><span class="sxs-lookup"><span data-stu-id="4fc26-148"><a name="run"></a> Step 2: Run your app</span></span>
<span data-ttu-id="4fc26-149">Kör din app med F5.</span><span class="sxs-lookup"><span data-stu-id="4fc26-149">Run your app with F5.</span></span> <span data-ttu-id="4fc26-150">Öppna olika sidor toogenerate vissa telemetri.</span><span class="sxs-lookup"><span data-stu-id="4fc26-150">Open different pages toogenerate some telemetry.</span></span>

<span data-ttu-id="4fc26-151">I Visual Studio kan du se antalet hello-händelser som har loggats.</span><span class="sxs-lookup"><span data-stu-id="4fc26-151">In Visual Studio, you see a count of hello events that have been logged.</span></span>

![Skärmbild av Visual Studio.](./media/app-insights-asp-net/54.png)

## <a name="step-3-see-your-telemetry"></a><span data-ttu-id="4fc26-154">Steg 3: Visa din telemetri</span><span class="sxs-lookup"><span data-stu-id="4fc26-154">Step 3: See your telemetry</span></span>
<span data-ttu-id="4fc26-155">Du kan se din telemetri i Visual Studio eller i hello Application Insights web-portalen.</span><span class="sxs-lookup"><span data-stu-id="4fc26-155">You can see your telemetry either in Visual Studio or in hello Application Insights web portal.</span></span> <span data-ttu-id="4fc26-156">Sök telemetri i Visual Studio toohelp du felsöka din app.</span><span class="sxs-lookup"><span data-stu-id="4fc26-156">Search telemetry in Visual Studio toohelp you debug your app.</span></span> <span data-ttu-id="4fc26-157">Övervaka prestanda och användning i hello webbportalen när datorn är aktiv.</span><span class="sxs-lookup"><span data-stu-id="4fc26-157">Monitor performance and usage in hello web portal when your system is live.</span></span> 

### <a name="see-your-telemetry-in-visual-studio"></a><span data-ttu-id="4fc26-158">Visa din telemetri i Visual Studio</span><span class="sxs-lookup"><span data-stu-id="4fc26-158">See your telemetry in Visual Studio</span></span>

<span data-ttu-id="4fc26-159">I Visual Studio öppnas hello Application Insights.</span><span class="sxs-lookup"><span data-stu-id="4fc26-159">In Visual Studio, open hello Application Insights window.</span></span> <span data-ttu-id="4fc26-160">Antingen på hello **Programinsikter** knappen eller högerklicka på projektet i Solution Explorer, Välj **Programinsikter**, och klicka sedan på **Sök Live telemetri**.</span><span class="sxs-lookup"><span data-stu-id="4fc26-160">Either click hello **Application Insights** button, or right-click your project in Solution Explorer, select **Application Insights**, and then click **Search Live Telemetry**.</span></span>

<span data-ttu-id="4fc26-161">Hello Visual Studio Application Insights Sök i fönstret visas hello **Data från felsökningssessionen** vy för telemetri som har genererats i hello på serversidan för din app.</span><span class="sxs-lookup"><span data-stu-id="4fc26-161">In hello Visual Studio Application Insights Search window, see hello **Data from Debug session** view for telemetry generated in hello server side of your app.</span></span> <span data-ttu-id="4fc26-162">Experimentera med hello filter och på varje händelse toosee detalj.</span><span class="sxs-lookup"><span data-stu-id="4fc26-162">Experiment with hello filters, and click any event toosee more detail.</span></span>

![Skärmbild av hello visa Data från felsökningssessionen i hello Application Insights-fönstret.](./media/app-insights-asp-net/55.png)

> [!NOTE]
> <span data-ttu-id="4fc26-164">Om du inte ser några data, kontrollera hello tidsintervallet är korrekt och klicka på sökikonen hello.</span><span class="sxs-lookup"><span data-stu-id="4fc26-164">If you don't see any data, make sure hello time range is correct, and click hello Search icon.</span></span>

<span data-ttu-id="4fc26-165">[Lär dig mer om Application Insights-verktygen i Visual Studio](app-insights-visual-studio.md).</span><span class="sxs-lookup"><span data-stu-id="4fc26-165">[Learn more about Application Insights tools in Visual Studio](app-insights-visual-studio.md).</span></span>

<a name="monitor"></a>
### <a name="see-telemetry-in-web-portal"></a><span data-ttu-id="4fc26-166">Visa telemetri i webbportalen</span><span class="sxs-lookup"><span data-stu-id="4fc26-166">See telemetry in web portal</span></span>

<span data-ttu-id="4fc26-167">Du kan också se telemetri i hello Application Insights web-portalen (om du har angett tooinstall endast hello SDK).</span><span class="sxs-lookup"><span data-stu-id="4fc26-167">You can also see telemetry in hello Application Insights web portal (unless you chose tooinstall only hello SDK).</span></span> <span data-ttu-id="4fc26-168">hello-portalen har flera diagram analytiska verktyg vyer, och mellan olika komponenter än Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="4fc26-168">hello portal has more charts, analytic tools, and cross-component views than Visual Studio.</span></span> <span data-ttu-id="4fc26-169">hello portal visar även varningar.</span><span class="sxs-lookup"><span data-stu-id="4fc26-169">hello portal also provides alerts.</span></span>

<span data-ttu-id="4fc26-170">Öppna Application Insights-resursen.</span><span class="sxs-lookup"><span data-stu-id="4fc26-170">Open your Application Insights resource.</span></span> <span data-ttu-id="4fc26-171">Antingen logga in toohello [Azure-portalen](https://portal.azure.com/) och hitta den där eller högerklicka på hello-projekt i Visual Studio och låta den får du det.</span><span class="sxs-lookup"><span data-stu-id="4fc26-171">Either sign in toohello [Azure portal](https://portal.azure.com/) and find it there, or right-click hello project in Visual Studio, and let it take you there.</span></span>

![Skärmbild av Visual Studio, visar hur tooopen hello Application Insights-portalen](./media/app-insights-asp-net/appinsights-04-openPortal.png)

> [!NOTE]
> <span data-ttu-id="4fc26-173">Om du får ett fel: har du fler än en uppsättning autentiseringsuppgifter för Microsoft och du är inloggad med hello fel set?</span><span class="sxs-lookup"><span data-stu-id="4fc26-173">If you get an access error: Do you have more than one set of Microsoft credentials, and are you signed in with hello wrong set?</span></span> <span data-ttu-id="4fc26-174">Logga ut och logga in igen i hello-portalen.</span><span class="sxs-lookup"><span data-stu-id="4fc26-174">In hello portal, sign out and sign in again.</span></span>

<span data-ttu-id="4fc26-175">hello portalen öppnas på en vy över hello telemetri från din app.</span><span class="sxs-lookup"><span data-stu-id="4fc26-175">hello portal opens on a view of hello telemetry from your app.</span></span>

![Skärmbild av Application Insights-översiktssidan](./media/app-insights-asp-net/66.png)

<span data-ttu-id="4fc26-177">Klicka på någon sida vid sida eller diagram toosee detalj i hello-portalen.</span><span class="sxs-lookup"><span data-stu-id="4fc26-177">In hello portal, click any tile or chart toosee more detail.</span></span>

<span data-ttu-id="4fc26-178">[Lär dig mer om hur du använder Application Insights i hello Azure-portalen](app-insights-dashboards.md).</span><span class="sxs-lookup"><span data-stu-id="4fc26-178">[Learn more about using Application Insights in hello Azure portal](app-insights-dashboards.md).</span></span>

## <a name="step-4-publish-your-app"></a><span data-ttu-id="4fc26-179">Steg 4: Publicera appen</span><span class="sxs-lookup"><span data-stu-id="4fc26-179">Step 4: Publish your app</span></span>
<span data-ttu-id="4fc26-180">Publicera din app tooyour IIS-servern eller tooAzure.</span><span class="sxs-lookup"><span data-stu-id="4fc26-180">Publish your app tooyour IIS server or tooAzure.</span></span> <span data-ttu-id="4fc26-181">Titta på [direktsänd dataström med mått](app-insights-metrics-explorer.md#live-metrics-stream) toomake att allt fungerar.</span><span class="sxs-lookup"><span data-stu-id="4fc26-181">Watch [Live Metrics Stream](app-insights-metrics-explorer.md#live-metrics-stream) toomake sure everything is running smoothly.</span></span>

<span data-ttu-id="4fc26-182">Din telemetri bygger upp i hello Application Insights-portalen där du kan övervaka, söka din telemetri och ställa in [instrumentpaneler](app-insights-dashboards.md).</span><span class="sxs-lookup"><span data-stu-id="4fc26-182">Your telemetry builds up in hello Application Insights portal, where you can monitor metrics, search your telemetry, and set up [dashboards](app-insights-dashboards.md).</span></span> <span data-ttu-id="4fc26-183">Du kan också använda hello kraftfulla [Log Analytics-frågespråket](https://docs.loganalytics.io/) tooanalyze användnings- och prestanda- eller toofind specifika händelser.</span><span class="sxs-lookup"><span data-stu-id="4fc26-183">You can also use hello powerful [Log Analytics query language](https://docs.loganalytics.io/) tooanalyze usage and performance, or toofind specific events.</span></span>

<span data-ttu-id="4fc26-184">Du kan också fortsätta tooanalyze din telemetri i [Visual Studio](app-insights-visual-studio.md), med verktyg som diagnostiska Sök och [trender](app-insights-visual-studio-trends.md).</span><span class="sxs-lookup"><span data-stu-id="4fc26-184">You can also continue tooanalyze your telemetry in [Visual Studio](app-insights-visual-studio.md), with tools such as diagnostic search and [trends](app-insights-visual-studio-trends.md).</span></span>

> [!NOTE]
> <span data-ttu-id="4fc26-185">Om din app skickar tillräckligt med telemetri tooapproach hello [throttling-begränsning](app-insights-pricing.md#limits-summary), automatisk [provtagning](app-insights-sampling.md) växlar.</span><span class="sxs-lookup"><span data-stu-id="4fc26-185">If your app sends enough telemetry tooapproach hello [throttling limits](app-insights-pricing.md#limits-summary), automatic [sampling](app-insights-sampling.md) switches on.</span></span> <span data-ttu-id="4fc26-186">Samplingsfrekvensen minskar hello antalet telemetri som skickats från din app samtidigt som korrelerade för att ställa diagnoser.</span><span class="sxs-lookup"><span data-stu-id="4fc26-186">Sampling reduces hello quantity of telemetry sent from your app, while preserving correlated data for diagnostic purposes.</span></span>
>
>

## <span data-ttu-id="4fc26-187"><a name="land"></a> Då var allt klart</span><span class="sxs-lookup"><span data-stu-id="4fc26-187"><a name="land"></a> You're all set</span></span>

<span data-ttu-id="4fc26-188">Grattis!</span><span class="sxs-lookup"><span data-stu-id="4fc26-188">Congratulations!</span></span> <span data-ttu-id="4fc26-189">Du installerade hello Application Insights paketet i din app och konfigurerats toosend telemetri toohello Application Insights-tjänsten på Azure.</span><span class="sxs-lookup"><span data-stu-id="4fc26-189">You installed hello Application Insights package in your app, and configured it toosend telemetry toohello Application Insights service on Azure.</span></span>

![Diagram över flödet av telemetri](./media/app-insights-asp-net/01-scheme.png)

<span data-ttu-id="4fc26-191">hello Azure-resurs som tar emot telemetri för din app har identifierats av en *instrumentation nyckeln*.</span><span class="sxs-lookup"><span data-stu-id="4fc26-191">hello Azure resource that receives your app's telemetry is identified by an *instrumentation key*.</span></span> <span data-ttu-id="4fc26-192">Den här nyckeln hittar du i hello ApplicationInsights.config-filen.</span><span class="sxs-lookup"><span data-stu-id="4fc26-192">You'll find this key in hello ApplicationInsights.config file.</span></span>


## <a name="upgrade-toofuture-sdk-versions"></a><span data-ttu-id="4fc26-193">Uppgradera toofuture SDK-versioner</span><span class="sxs-lookup"><span data-stu-id="4fc26-193">Upgrade toofuture SDK versions</span></span>
<span data-ttu-id="4fc26-194">tooupgrade tooa [nya versionen av hello SDK](https://github.com/Microsoft/ApplicationInsights-dotnet-server/releases)öppnar hello **NuGet-Pakethanteraren** igen, och filtret på installerade paket.</span><span class="sxs-lookup"><span data-stu-id="4fc26-194">tooupgrade tooa [new release of hello SDK](https://github.com/Microsoft/ApplicationInsights-dotnet-server/releases), open hello **NuGet package manager** again, and filter on installed packages.</span></span> <span data-ttu-id="4fc26-195">Markera **Microsoft.ApplicationInsights.Web** och välj **Uppgradera**.</span><span class="sxs-lookup"><span data-stu-id="4fc26-195">Select **Microsoft.ApplicationInsights.Web**, and choose **Upgrade**.</span></span>

<span data-ttu-id="4fc26-196">Om du har gjort alla anpassningar tooApplicationInsights.config spara en kopia av den innan du uppgraderar.</span><span class="sxs-lookup"><span data-stu-id="4fc26-196">If you made any customizations tooApplicationInsights.config, save a copy of it before you upgrade.</span></span> <span data-ttu-id="4fc26-197">Sedan sammanfoga ändringarna i hello ny version.</span><span class="sxs-lookup"><span data-stu-id="4fc26-197">Then, merge your changes into hello new version.</span></span>

## <a name="video"></a><span data-ttu-id="4fc26-198">Video</span><span class="sxs-lookup"><span data-stu-id="4fc26-198">Video</span></span>

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/100/player]

## <a name="next-steps"></a><span data-ttu-id="4fc26-199">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="4fc26-199">Next steps</span></span>

### <a name="more-telemetry"></a><span data-ttu-id="4fc26-200">Mer telemetri</span><span class="sxs-lookup"><span data-stu-id="4fc26-200">More telemetry</span></span>

* <span data-ttu-id="4fc26-201">**[Webbläsare och webbsidesinläsning](app-insights-javascript.md)**  – Infoga ett kodfragment på dina webbsidor.</span><span class="sxs-lookup"><span data-stu-id="4fc26-201">**[Browser and page load data](app-insights-javascript.md)** - Insert a code snippet in your web pages.</span></span>
* <span data-ttu-id="4fc26-202">**[Få mer detaljerad beroende- och undantagsövervakning](app-insights-monitor-performance-live-website-now.md)**  – Installera Status Monitor på servern.</span><span class="sxs-lookup"><span data-stu-id="4fc26-202">**[Get more detailed dependency and exception monitoring](app-insights-monitor-performance-live-website-now.md)** - Install Status Monitor on your server.</span></span>
* <span data-ttu-id="4fc26-203">**[Anpassade händelser Code](app-insights-api-custom-events-metrics.md)**  toocount, tid, eller ett mått användaråtgärder.</span><span class="sxs-lookup"><span data-stu-id="4fc26-203">**[Code custom events](app-insights-api-custom-events-metrics.md)** toocount, time, or measure user actions.</span></span>
* <span data-ttu-id="4fc26-204">**[Hämta loggdata](app-insights-asp-net-trace-logs.md)**  – Korrelera loggdata med din telemetri.</span><span class="sxs-lookup"><span data-stu-id="4fc26-204">**[Get log data](app-insights-asp-net-trace-logs.md)** - Correlate log data with your telemetry.</span></span>

### <a name="analysis"></a><span data-ttu-id="4fc26-205">Analys</span><span class="sxs-lookup"><span data-stu-id="4fc26-205">Analysis</span></span>

* <span data-ttu-id="4fc26-206">**[Arbeta med Application Insights i Visual Studio](app-insights-visual-studio.md)**</span><span class="sxs-lookup"><span data-stu-id="4fc26-206">**[Working with Application Insights in Visual Studio](app-insights-visual-studio.md)**</span></span><br/><span data-ttu-id="4fc26-207">Innehåller information om felsökning med telemetri diagnostiska sökningen och detaljerad toocode.</span><span class="sxs-lookup"><span data-stu-id="4fc26-207">Includes information about debugging with telemetry, diagnostic search, and drill through toocode.</span></span>
* <span data-ttu-id="4fc26-208">**[Arbeta med hello Application Insights-portalen](app-insights-dashboards.md)**</span><span class="sxs-lookup"><span data-stu-id="4fc26-208">**[Working with hello Application Insights portal](app-insights-dashboards.md)**</span></span><br/> <span data-ttu-id="4fc26-209">Innehåller information om instrumentpaneler, kraftfulla verktyg för diagnostik och analys, aviseringar, realtidsmappning av beroenden för din app och telemetriexport.</span><span class="sxs-lookup"><span data-stu-id="4fc26-209">Includes information about dashboards, powerful diagnostic and analytic tools, alerts, a live dependency map of your application, and telemetry export.</span></span>
* <span data-ttu-id="4fc26-210">**[Analytics](app-insights-analytics-tour.md)**  -hello kraftfulla frågespråk.</span><span class="sxs-lookup"><span data-stu-id="4fc26-210">**[Analytics](app-insights-analytics-tour.md)** - hello powerful query language.</span></span>

### <a name="alerts"></a><span data-ttu-id="4fc26-211">Aviseringar</span><span class="sxs-lookup"><span data-stu-id="4fc26-211">Alerts</span></span>

* <span data-ttu-id="4fc26-212">[Tillgänglighetstester](app-insights-monitor-web-app-availability.md): skapa tester toomake till webbplatsen visas på hello-webbplatsen.</span><span class="sxs-lookup"><span data-stu-id="4fc26-212">[Availability tests](app-insights-monitor-web-app-availability.md): Create tests toomake sure your site is visible on hello web.</span></span>
* <span data-ttu-id="4fc26-213">[Diagnostik för smartkort](app-insights-proactive-diagnostics.md): dessa tester körs automatiskt, så du inte behöver toodo något annat tooset dem upp.</span><span class="sxs-lookup"><span data-stu-id="4fc26-213">[Smart diagnostics](app-insights-proactive-diagnostics.md): These tests run automatically, so you don't have toodo anything tooset them up.</span></span> <span data-ttu-id="4fc26-214">De berättar om din app har ett ovanligt antal misslyckade begäranden.</span><span class="sxs-lookup"><span data-stu-id="4fc26-214">They tell you if your app has an unusual rate of failed requests.</span></span>
* <span data-ttu-id="4fc26-215">[Mått aviseringar](app-insights-alerts.md): ange dessa toowarn om måttet överskrider ett tröskelvärde.</span><span class="sxs-lookup"><span data-stu-id="4fc26-215">[Metric alerts](app-insights-alerts.md): Set these toowarn you if a metric crosses a threshold.</span></span> <span data-ttu-id="4fc26-216">Du kan ställa in dem för anpassade mätningar som du kodar i din app.</span><span class="sxs-lookup"><span data-stu-id="4fc26-216">You can set them on custom metrics that you code into your app.</span></span>

### <a name="automation"></a><span data-ttu-id="4fc26-217">Automation</span><span class="sxs-lookup"><span data-stu-id="4fc26-217">Automation</span></span>

* [<span data-ttu-id="4fc26-218">Automatisera skapandet av en Application Insights-resurs</span><span class="sxs-lookup"><span data-stu-id="4fc26-218">Automate creating an Application Insights resource</span></span>](app-insights-powershell.md)
