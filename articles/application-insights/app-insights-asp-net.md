---
title: "Konfigurera webbappsanalyser för ASP.NET med Application Insights | Microsoft Docs"
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
ms.openlocfilehash: cb247ee68da88265f7c51258644064463d44f8b5
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="set-up-application-insights-for-your-aspnet-website"></a><span data-ttu-id="00752-103">Konfigurera Application Insights för din ASP.NET-webbplats</span><span class="sxs-lookup"><span data-stu-id="00752-103">Set up Application Insights for your ASP.NET website</span></span>

<span data-ttu-id="00752-104">Den här proceduren konfigurerar din ASP.NET-webbapp för att skicka telemetri till tjänsten [Azure Application Insights](app-insights-overview.md).</span><span class="sxs-lookup"><span data-stu-id="00752-104">This procedure configures your ASP.NET web app to send telemetry to the [Azure Application Insights](app-insights-overview.md) service.</span></span> <span data-ttu-id="00752-105">Den fungerar för ASP.NET-appar som finns i din IIS-server eller i molnet.</span><span class="sxs-lookup"><span data-stu-id="00752-105">It works for ASP.NET apps that are hosted either in your own IIS server or in the Cloud.</span></span> <span data-ttu-id="00752-106">Du får tillgång till diagram och ett kraftfullt frågespråket som hjälper dig att förstå hur din app presterar och hur människor använder den, plus automatiska aviseringar om fel eller prestandaproblem.</span><span class="sxs-lookup"><span data-stu-id="00752-106">You get charts and a powerful query language that help you understand the performance of your app and how people are using it, plus automatic alerts on failures or performance issues.</span></span> <span data-ttu-id="00752-107">Många utvecklare tycker att de här funktionerna är mycket bra som de är, men du kan också utöka och anpassa telemetrin om du behöver.</span><span class="sxs-lookup"><span data-stu-id="00752-107">Many developers find these features great as they are, but you can also extend and customize the telemetry if you need to.</span></span>

<span data-ttu-id="00752-108">Installationen kräver bara några klick i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="00752-108">Setup takes just a few clicks in Visual Studio.</span></span> <span data-ttu-id="00752-109">Du har möjlighet att undvika kostnader genom att begränsa mängden telemetri.</span><span class="sxs-lookup"><span data-stu-id="00752-109">You have the option to avoid charges by limiting the volume of telemetry.</span></span> <span data-ttu-id="00752-110">På så sätt kan du experimentera och felsöka eller övervaka en plats som har få användare.</span><span class="sxs-lookup"><span data-stu-id="00752-110">This allows you to experiment and debug, or to monitor a site with not many users.</span></span> <span data-ttu-id="00752-111">Om du sedan vill gå vidare och övervaka din produktionsplats, är det lätt att höja gränsen vid ett senare tillfälle.</span><span class="sxs-lookup"><span data-stu-id="00752-111">When you decide you want to go ahead and monitor your production site, it's easy to raise the limit later.</span></span>

## <a name="before-you-start"></a><span data-ttu-id="00752-112">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="00752-112">Before you start</span></span>
<span data-ttu-id="00752-113">Du behöver:</span><span class="sxs-lookup"><span data-stu-id="00752-113">You need:</span></span>

* <span data-ttu-id="00752-114">Visual Studio 2013 Update 3 eller senare.</span><span class="sxs-lookup"><span data-stu-id="00752-114">Visual Studio 2013 update 3 or later.</span></span> <span data-ttu-id="00752-115">Senare versioner är att föredra.</span><span class="sxs-lookup"><span data-stu-id="00752-115">Later is better.</span></span>
* <span data-ttu-id="00752-116">En prenumeration på [Microsoft Azure](http://azure.com).</span><span class="sxs-lookup"><span data-stu-id="00752-116">A subscription to [Microsoft Azure](http://azure.com).</span></span> <span data-ttu-id="00752-117">Om ditt team eller din organisation har en Azure-prenumeration kan ägaren lägga till dig med hjälp av ditt [Microsoft-konto](http://live.com).</span><span class="sxs-lookup"><span data-stu-id="00752-117">If your team or organization has an Azure subscription, the owner can add you to it, by using your [Microsoft account](http://live.com).</span></span>

<span data-ttu-id="00752-118">Det finns andra artiklar som du kan läsa om du är intresserad av följande:</span><span class="sxs-lookup"><span data-stu-id="00752-118">There are alternative topics to look at if you are interested in:</span></span>

* [<span data-ttu-id="00752-119">Instrumentering av en webbapp under körning</span><span class="sxs-lookup"><span data-stu-id="00752-119">Instrumenting a web app at runtime</span></span>](app-insights-monitor-performance-live-website-now.md)
* [<span data-ttu-id="00752-120">Azure Cloud Services</span><span class="sxs-lookup"><span data-stu-id="00752-120">Azure Cloud Services</span></span>](app-insights-cloudservices.md)

## <span data-ttu-id="00752-121"><a name="ide"></a> Steg 1: Lägg till Application Insights SDK</span><span class="sxs-lookup"><span data-stu-id="00752-121"><a name="ide"></a> Step 1: Add the Application Insights SDK</span></span>

<span data-ttu-id="00752-122">Högerklicka på ditt webbappsprojekt i Solution Explorer och välj **Lägg till** > **Application Insights Telemetry...** eller **Konfigurera Application Insights**.</span><span class="sxs-lookup"><span data-stu-id="00752-122">Right-click your web app project in Solution Explorer, and choose **Add** > **Application Insights Telemetry...** or **Configure Application Insights**.</span></span>

![Skärmbild av Solution Explorer med Lägg till Application Insights Telemetry markerat](./media/app-insights-asp-net/appinsights-03-addExisting.png)

<span data-ttu-id="00752-124">(I Visual Studio 2015 finns det också ett alternativ för att lägga till Application Insights i dialogrutan Nytt projekt.)</span><span class="sxs-lookup"><span data-stu-id="00752-124">(In Visual Studio 2015, there's also an option to add Application Insights in the New Project dialog.)</span></span>

<span data-ttu-id="00752-125">Fortsätt till konfigurationssidan för Application Insights:</span><span class="sxs-lookup"><span data-stu-id="00752-125">Continue to the Application Insights configuration page:</span></span>

![Skärmbild av sidan Register your app with Application Insights (Registrera din app med Application Insights)](./media/app-insights-asp-net/visual-studio-register-dialog.png)

<span data-ttu-id="00752-127">**a.**</span><span class="sxs-lookup"><span data-stu-id="00752-127">**a.**</span></span> <span data-ttu-id="00752-128">Välj det konto och din prenumeration som du använder för att få åtkomst till Azure.</span><span class="sxs-lookup"><span data-stu-id="00752-128">Select the account and subscription that you use to access Azure.</span></span>

<span data-ttu-id="00752-129">**b.**</span><span class="sxs-lookup"><span data-stu-id="00752-129">**b.**</span></span> <span data-ttu-id="00752-130">Markera resursen i Azure där du vill visa data från din app.</span><span class="sxs-lookup"><span data-stu-id="00752-130">Select the resource in Azure where you want to see the data from your app.</span></span> <span data-ttu-id="00752-131">Vanligtvis:</span><span class="sxs-lookup"><span data-stu-id="00752-131">Usually:</span></span>

* <span data-ttu-id="00752-132">Använd en [enkel resurs för olika komponenter](app-insights-monitor-multi-role-apps.md) i ett enda program.</span><span class="sxs-lookup"><span data-stu-id="00752-132">Use a [single resource for different components](app-insights-monitor-multi-role-apps.md) of a single application.</span></span> 
* <span data-ttu-id="00752-133">Skapa separata resurser för orelaterade program.</span><span class="sxs-lookup"><span data-stu-id="00752-133">Create separate resources for unrelated applications.</span></span>
 
<span data-ttu-id="00752-134">Om du vill ange resursgruppen eller den plats där dina data är lagrade klickar du på **Konfigurera inställningar**.</span><span class="sxs-lookup"><span data-stu-id="00752-134">If you want to set the resource group or the location where your data is stored, click **Configure settings**.</span></span> <span data-ttu-id="00752-135">Resursgrupper används för att styra dataåtkomst.</span><span class="sxs-lookup"><span data-stu-id="00752-135">Resource groups are used to control access to data.</span></span> <span data-ttu-id="00752-136">Om du exempelvis har flera appar som ingår i samma system kan du placera deras Application Insights-data i samma resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="00752-136">For example, if you have several apps that form part of the same system, you might put their Application Insights data in the same resource group.</span></span>

<span data-ttu-id="00752-137">**c.**</span><span class="sxs-lookup"><span data-stu-id="00752-137">**c.**</span></span> <span data-ttu-id="00752-138">Ange en begränsning för den fria datavolymen för att undvika avgifter.</span><span class="sxs-lookup"><span data-stu-id="00752-138">Set a cap at the free data volume limit, to avoid charges.</span></span> <span data-ttu-id="00752-139">Application Insights är kostnadsfritt upp till en viss telemetrinivå.</span><span class="sxs-lookup"><span data-stu-id="00752-139">Application Insights is free up to a certain volume of telemetry.</span></span> <span data-ttu-id="00752-140">När resursen har skapats kan du ändra ditt val i portalen genom att öppna **Funktioner och prissättning**,  > **Datavolymhantering** > **Dagligt volymtak**.</span><span class="sxs-lookup"><span data-stu-id="00752-140">After the resource is created, you can change your selection in the portal by opening  **Features + pricing** > **Data volume management** > **Daily volume cap**.</span></span>

<span data-ttu-id="00752-141">**d.**</span><span class="sxs-lookup"><span data-stu-id="00752-141">**d.**</span></span> <span data-ttu-id="00752-142">Klicka på **Registrera** för att fortsätta att konfigurera Application Insights för din webbapp.</span><span class="sxs-lookup"><span data-stu-id="00752-142">Click **Register** to go ahead and configure Application Insights for your web app.</span></span> <span data-ttu-id="00752-143">Telemetri kommer att skickas till [Azure Portal](https://portal.azure.com), både under felsökningen och när du har publicerat din app.</span><span class="sxs-lookup"><span data-stu-id="00752-143">Telemetry will be sent to the [Azure portal](https://portal.azure.com), both during debugging and after you have published your app.</span></span>

<span data-ttu-id="00752-144">**e.**</span><span class="sxs-lookup"><span data-stu-id="00752-144">**e.**</span></span> <span data-ttu-id="00752-145">Om du inte vill skicka telemetri till portalen medan du felsöker, lägger du bara till Application Insights SDK i din app men konfigurerar inte en resurs i portalen.</span><span class="sxs-lookup"><span data-stu-id="00752-145">If you don't want to send telemetry to the portal while you're debugging, just add the Application Insights SDK to your app but don't configure a resource in the portal.</span></span> <span data-ttu-id="00752-146">Du kommer att kunna visa telemetri i Visual Studio när du felsöker.</span><span class="sxs-lookup"><span data-stu-id="00752-146">You will be able to see telemetry in Visual Studio while you are debugging.</span></span> <span data-ttu-id="00752-147">Du kan senare återvända till den här konfigurationssidan eller vänta tills du har distribuerat appen och [aktivera telemetri vid körtid](app-insights-monitor-performance-live-website-now.md).</span><span class="sxs-lookup"><span data-stu-id="00752-147">Later, you can return to this configuration page, or you could wait until after you have deployed your app and [switch on telemetry at run time](app-insights-monitor-performance-live-website-now.md).</span></span>


## <span data-ttu-id="00752-148"><a name="run"></a> Steg 2: Kör appen</span><span class="sxs-lookup"><span data-stu-id="00752-148"><a name="run"></a> Step 2: Run your app</span></span>
<span data-ttu-id="00752-149">Kör din app med F5.</span><span class="sxs-lookup"><span data-stu-id="00752-149">Run your app with F5.</span></span> <span data-ttu-id="00752-150">Öppna olika sidor för att skapa viss telemetri.</span><span class="sxs-lookup"><span data-stu-id="00752-150">Open different pages to generate some telemetry.</span></span>

<span data-ttu-id="00752-151">Du ser hur många händelser som har loggats i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="00752-151">In Visual Studio, you see a count of the events that have been logged.</span></span>

![Skärmbild av Visual Studio.](./media/app-insights-asp-net/54.png)

## <a name="step-3-see-your-telemetry"></a><span data-ttu-id="00752-154">Steg 3: Visa din telemetri</span><span class="sxs-lookup"><span data-stu-id="00752-154">Step 3: See your telemetry</span></span>
<span data-ttu-id="00752-155">Du kan se din telemetri antingen i Visual Studio eller i Application Insights-webbportalen.</span><span class="sxs-lookup"><span data-stu-id="00752-155">You can see your telemetry either in Visual Studio or in the Application Insights web portal.</span></span> <span data-ttu-id="00752-156">Sök telemetri i Visual Studio för att felsöka din app.</span><span class="sxs-lookup"><span data-stu-id="00752-156">Search telemetry in Visual Studio to help you debug your app.</span></span> <span data-ttu-id="00752-157">Övervaka prestanda och användning i webbportalen när systemet är aktivt.</span><span class="sxs-lookup"><span data-stu-id="00752-157">Monitor performance and usage in the web portal when your system is live.</span></span> 

### <a name="see-your-telemetry-in-visual-studio"></a><span data-ttu-id="00752-158">Visa din telemetri i Visual Studio</span><span class="sxs-lookup"><span data-stu-id="00752-158">See your telemetry in Visual Studio</span></span>

<span data-ttu-id="00752-159">Öppna fönstret Application Insights i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="00752-159">In Visual Studio, open the Application Insights window.</span></span> <span data-ttu-id="00752-160">Klicka antingen på **Application Insights** eller högerklicka på projektet i Solution Explorer, välj **Application Insights** och klicka sedan på alternativet för att **söka i realtidstelemetri**.</span><span class="sxs-lookup"><span data-stu-id="00752-160">Either click the **Application Insights** button, or right-click your project in Solution Explorer, select **Application Insights**, and then click **Search Live Telemetry**.</span></span>

<span data-ttu-id="00752-161">I Visual Studio Application Insights-sökfönstret visas vyn **Data från felsökningssessionen** för telemetri som genereras i din app på serversidan.</span><span class="sxs-lookup"><span data-stu-id="00752-161">In the Visual Studio Application Insights Search window, see the **Data from Debug session** view for telemetry generated in the server side of your app.</span></span> <span data-ttu-id="00752-162">Experimentera med filtren och klicka på en händelse om du vill visa mer information.</span><span class="sxs-lookup"><span data-stu-id="00752-162">Experiment with the filters, and click any event to see more detail.</span></span>

![Skärmbild av data från felsökningssessionsvyn i fönstret Application Insights.](./media/app-insights-asp-net/55.png)

> [!NOTE]
> <span data-ttu-id="00752-164">Om du inte ser några data kontrollerar du att tidsintervallet är korrekt och klickar på sökikonen.</span><span class="sxs-lookup"><span data-stu-id="00752-164">If you don't see any data, make sure the time range is correct, and click the Search icon.</span></span>

<span data-ttu-id="00752-165">[Lär dig mer om Application Insights-verktygen i Visual Studio](app-insights-visual-studio.md).</span><span class="sxs-lookup"><span data-stu-id="00752-165">[Learn more about Application Insights tools in Visual Studio](app-insights-visual-studio.md).</span></span>

<a name="monitor"></a>
### <a name="see-telemetry-in-web-portal"></a><span data-ttu-id="00752-166">Visa telemetri i webbportalen</span><span class="sxs-lookup"><span data-stu-id="00752-166">See telemetry in web portal</span></span>

<span data-ttu-id="00752-167">Du kan även visa telemetrin i Application Insights-webbportalen (såvida du inte väljer att installera enbart SDK:n).</span><span class="sxs-lookup"><span data-stu-id="00752-167">You can also see telemetry in the Application Insights web portal (unless you chose to install only the SDK).</span></span> <span data-ttu-id="00752-168">Portalen innehåller fler diagram, analysverktyg och komponentöverskridande vyer än Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="00752-168">The portal has more charts, analytic tools, and cross-component views than Visual Studio.</span></span> <span data-ttu-id="00752-169">Portalen visar även aviseringar.</span><span class="sxs-lookup"><span data-stu-id="00752-169">The portal also provides alerts.</span></span>

<span data-ttu-id="00752-170">Öppna Application Insights-resursen.</span><span class="sxs-lookup"><span data-stu-id="00752-170">Open your Application Insights resource.</span></span> <span data-ttu-id="00752-171">Logga antingen in på [Azure Portal](https://portal.azure.com/) och hitta den där, eller högerklicka på projektet i Visual Studio så kommer du dit.</span><span class="sxs-lookup"><span data-stu-id="00752-171">Either sign in to the [Azure portal](https://portal.azure.com/) and find it there, or right-click the project in Visual Studio, and let it take you there.</span></span>

![Skärmbild av Visual Studio som visar hur du öppnar Application Insights-portalen](./media/app-insights-asp-net/appinsights-04-openPortal.png)

> [!NOTE]
> <span data-ttu-id="00752-173">Om ett åtkomstfel visas kan det bero på att du har fler än en uppsättning Microsoft-autentiseringsuppgifter och att du kanske är inloggad med fel uppsättning.</span><span class="sxs-lookup"><span data-stu-id="00752-173">If you get an access error: Do you have more than one set of Microsoft credentials, and are you signed in with the wrong set?</span></span> <span data-ttu-id="00752-174">Logga ut och logga in igen i portalen.</span><span class="sxs-lookup"><span data-stu-id="00752-174">In the portal, sign out and sign in again.</span></span>

<span data-ttu-id="00752-175">Portalen öppnas i en vy över telemetrin från appen.</span><span class="sxs-lookup"><span data-stu-id="00752-175">The portal opens on a view of the telemetry from your app.</span></span>

![Skärmbild av Application Insights-översiktssidan](./media/app-insights-asp-net/66.png)

<span data-ttu-id="00752-177">Klicka på valfri ikon eller valfritt diagram i portalen för att visa mer information.</span><span class="sxs-lookup"><span data-stu-id="00752-177">In the portal, click any tile or chart to see more detail.</span></span>

<span data-ttu-id="00752-178">[Läs mer om hur du använder Application Insights på Azure Portal](app-insights-dashboards.md).</span><span class="sxs-lookup"><span data-stu-id="00752-178">[Learn more about using Application Insights in the Azure portal](app-insights-dashboards.md).</span></span>

## <a name="step-4-publish-your-app"></a><span data-ttu-id="00752-179">Steg 4: Publicera appen</span><span class="sxs-lookup"><span data-stu-id="00752-179">Step 4: Publish your app</span></span>
<span data-ttu-id="00752-180">Publicera din app på din IIS-server eller i Azure.</span><span class="sxs-lookup"><span data-stu-id="00752-180">Publish your app to your IIS server or to Azure.</span></span> <span data-ttu-id="00752-181">Bevaka [Live Metrics Stream](app-insights-metrics-explorer.md#live-metrics-stream) och se om allt fungerar som det ska.</span><span class="sxs-lookup"><span data-stu-id="00752-181">Watch [Live Metrics Stream](app-insights-metrics-explorer.md#live-metrics-stream) to make sure everything is running smoothly.</span></span>

<span data-ttu-id="00752-182">Du ser din telemetri byggas upp i Application Insights-portalen, där du kan övervaka mått, söka i telemetrin och konfigurera [instrumentpaneler](app-insights-dashboards.md).</span><span class="sxs-lookup"><span data-stu-id="00752-182">Your telemetry builds up in the Application Insights portal, where you can monitor metrics, search your telemetry, and set up [dashboards](app-insights-dashboards.md).</span></span> <span data-ttu-id="00752-183">Du kan också använda det kraftfulla [Log Analytics-frågespråket](https://docs.loganalytics.io/) för att analysera användning och prestanda eller söka efter specifika händelser.</span><span class="sxs-lookup"><span data-stu-id="00752-183">You can also use the powerful [Log Analytics query language](https://docs.loganalytics.io/) to analyze usage and performance, or to find specific events.</span></span>

<span data-ttu-id="00752-184">Du kan också fortsätta att analysera din telemetri i [Visual Studio](app-insights-visual-studio.md) med verktyg som diagnossökning och [trender](app-insights-visual-studio-trends.md).</span><span class="sxs-lookup"><span data-stu-id="00752-184">You can also continue to analyze your telemetry in [Visual Studio](app-insights-visual-studio.md), with tools such as diagnostic search and [trends](app-insights-visual-studio-trends.md).</span></span>

> [!NOTE]
> <span data-ttu-id="00752-185">Om din app skickar så mycket telemetri att det närmar sig [begränsningsgränserna](app-insights-pricing.md#limits-summary) aktiveras [sampling](app-insights-sampling.md) automatiskt.</span><span class="sxs-lookup"><span data-stu-id="00752-185">If your app sends enough telemetry to approach the [throttling limits](app-insights-pricing.md#limits-summary), automatic [sampling](app-insights-sampling.md) switches on.</span></span> <span data-ttu-id="00752-186">Sampling minskar mängden telemetri som skickas från din app, samtidigt som korrelerade informationen bevaras i diagnossyfte.</span><span class="sxs-lookup"><span data-stu-id="00752-186">Sampling reduces the quantity of telemetry sent from your app, while preserving correlated data for diagnostic purposes.</span></span>
>
>

## <span data-ttu-id="00752-187"><a name="land"></a> Då var allt klart</span><span class="sxs-lookup"><span data-stu-id="00752-187"><a name="land"></a> You're all set</span></span>

<span data-ttu-id="00752-188">Grattis!</span><span class="sxs-lookup"><span data-stu-id="00752-188">Congratulations!</span></span> <span data-ttu-id="00752-189">Du har installerat Application Insights-paketet i din app och konfigurerat det för att skicka telemetri till Application Insights-tjänsten på Azure.</span><span class="sxs-lookup"><span data-stu-id="00752-189">You installed the Application Insights package in your app, and configured it to send telemetry to the Application Insights service on Azure.</span></span>

![Diagram över flödet av telemetri](./media/app-insights-asp-net/01-scheme.png)

<span data-ttu-id="00752-191">Azure-resursen som tar emot din apps telemetri identifieras med en *instrumenteringsnyckel*.</span><span class="sxs-lookup"><span data-stu-id="00752-191">The Azure resource that receives your app's telemetry is identified by an *instrumentation key*.</span></span> <span data-ttu-id="00752-192">Du hittar den här nyckeln i filen ApplicationInsights.config.config.</span><span class="sxs-lookup"><span data-stu-id="00752-192">You'll find this key in the ApplicationInsights.config file.</span></span>


## <a name="upgrade-to-future-sdk-versions"></a><span data-ttu-id="00752-193">Uppgradera till framtida SDK-versioner</span><span class="sxs-lookup"><span data-stu-id="00752-193">Upgrade to future SDK versions</span></span>
<span data-ttu-id="00752-194">Om du vill uppgradera till en [ny SDK-version](https://github.com/Microsoft/ApplicationInsights-dotnet-server/releases) öppnar du **NuGet-pakethanteraren** igen och filtrerar på installerade paket.</span><span class="sxs-lookup"><span data-stu-id="00752-194">To upgrade to a [new release of the SDK](https://github.com/Microsoft/ApplicationInsights-dotnet-server/releases), open the **NuGet package manager** again, and filter on installed packages.</span></span> <span data-ttu-id="00752-195">Markera **Microsoft.ApplicationInsights.Web** och välj **Uppgradera**.</span><span class="sxs-lookup"><span data-stu-id="00752-195">Select **Microsoft.ApplicationInsights.Web**, and choose **Upgrade**.</span></span>

<span data-ttu-id="00752-196">Om du har gjort anpassningar i ApplicationInsights.config sparar du en kopia av den innan du uppgraderar.</span><span class="sxs-lookup"><span data-stu-id="00752-196">If you made any customizations to ApplicationInsights.config, save a copy of it before you upgrade.</span></span> <span data-ttu-id="00752-197">Sammanfogar sedan dina ändringar i den nya versionen.</span><span class="sxs-lookup"><span data-stu-id="00752-197">Then, merge your changes into the new version.</span></span>

## <a name="video"></a><span data-ttu-id="00752-198">Video</span><span class="sxs-lookup"><span data-stu-id="00752-198">Video</span></span>

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/100/player]

## <a name="next-steps"></a><span data-ttu-id="00752-199">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="00752-199">Next steps</span></span>

### <a name="more-telemetry"></a><span data-ttu-id="00752-200">Mer telemetri</span><span class="sxs-lookup"><span data-stu-id="00752-200">More telemetry</span></span>

* <span data-ttu-id="00752-201">**[Webbläsare och webbsidesinläsning](app-insights-javascript.md)**  – Infoga ett kodfragment på dina webbsidor.</span><span class="sxs-lookup"><span data-stu-id="00752-201">**[Browser and page load data](app-insights-javascript.md)** - Insert a code snippet in your web pages.</span></span>
* <span data-ttu-id="00752-202">**[Få mer detaljerad beroende- och undantagsövervakning](app-insights-monitor-performance-live-website-now.md)**  – Installera Status Monitor på servern.</span><span class="sxs-lookup"><span data-stu-id="00752-202">**[Get more detailed dependency and exception monitoring](app-insights-monitor-performance-live-website-now.md)** - Install Status Monitor on your server.</span></span>
* <span data-ttu-id="00752-203">**[Koda anpassade händelser](app-insights-api-custom-events-metrics.md)** för att räkna, ta tid på eller mäta användaråtgärder.</span><span class="sxs-lookup"><span data-stu-id="00752-203">**[Code custom events](app-insights-api-custom-events-metrics.md)** to count, time, or measure user actions.</span></span>
* <span data-ttu-id="00752-204">**[Hämta loggdata](app-insights-asp-net-trace-logs.md)**  – Korrelera loggdata med din telemetri.</span><span class="sxs-lookup"><span data-stu-id="00752-204">**[Get log data](app-insights-asp-net-trace-logs.md)** - Correlate log data with your telemetry.</span></span>

### <a name="analysis"></a><span data-ttu-id="00752-205">Analys</span><span class="sxs-lookup"><span data-stu-id="00752-205">Analysis</span></span>

* <span data-ttu-id="00752-206">**[Arbeta med Application Insights i Visual Studio](app-insights-visual-studio.md)**</span><span class="sxs-lookup"><span data-stu-id="00752-206">**[Working with Application Insights in Visual Studio](app-insights-visual-studio.md)**</span></span><br/><span data-ttu-id="00752-207">Innehåller information om att felsöka med telemetri, köra diagnostiksökningar och gå igenom koden.</span><span class="sxs-lookup"><span data-stu-id="00752-207">Includes information about debugging with telemetry, diagnostic search, and drill through to code.</span></span>
* <span data-ttu-id="00752-208">**[Arbeta med Application Insights-portalen](app-insights-dashboards.md)**</span><span class="sxs-lookup"><span data-stu-id="00752-208">**[Working with the Application Insights portal](app-insights-dashboards.md)**</span></span><br/> <span data-ttu-id="00752-209">Innehåller information om instrumentpaneler, kraftfulla verktyg för diagnostik och analys, aviseringar, realtidsmappning av beroenden för din app och telemetriexport.</span><span class="sxs-lookup"><span data-stu-id="00752-209">Includes information about dashboards, powerful diagnostic and analytic tools, alerts, a live dependency map of your application, and telemetry export.</span></span>
* <span data-ttu-id="00752-210">**[Analytics](app-insights-analytics-tour.md)** – Kraftfullt frågespråk.</span><span class="sxs-lookup"><span data-stu-id="00752-210">**[Analytics](app-insights-analytics-tour.md)** - The powerful query language.</span></span>

### <a name="alerts"></a><span data-ttu-id="00752-211">Aviseringar</span><span class="sxs-lookup"><span data-stu-id="00752-211">Alerts</span></span>

* <span data-ttu-id="00752-212">[Tillgänglighetstester](app-insights-monitor-web-app-availability.md): Skapa tester som kan användas för att kontrollera att webbplatsen visas på webben.</span><span class="sxs-lookup"><span data-stu-id="00752-212">[Availability tests](app-insights-monitor-web-app-availability.md): Create tests to make sure your site is visible on the web.</span></span>
* <span data-ttu-id="00752-213">[Smart diagnostik](app-insights-proactive-diagnostics.md): De här testerna körs automatiskt, så du behöver inte göra något för att konfigurera dem.</span><span class="sxs-lookup"><span data-stu-id="00752-213">[Smart diagnostics](app-insights-proactive-diagnostics.md): These tests run automatically, so you don't have to do anything to set them up.</span></span> <span data-ttu-id="00752-214">De berättar om din app har ett ovanligt antal misslyckade begäranden.</span><span class="sxs-lookup"><span data-stu-id="00752-214">They tell you if your app has an unusual rate of failed requests.</span></span>
* <span data-ttu-id="00752-215">[Måttaviseringar](app-insights-alerts.md): Används för att varna dig om ett mått överskrider ett tröskelvärde.</span><span class="sxs-lookup"><span data-stu-id="00752-215">[Metric alerts](app-insights-alerts.md): Set these to warn you if a metric crosses a threshold.</span></span> <span data-ttu-id="00752-216">Du kan ställa in dem för anpassade mätningar som du kodar i din app.</span><span class="sxs-lookup"><span data-stu-id="00752-216">You can set them on custom metrics that you code into your app.</span></span>

### <a name="automation"></a><span data-ttu-id="00752-217">Automation</span><span class="sxs-lookup"><span data-stu-id="00752-217">Automation</span></span>

* [<span data-ttu-id="00752-218">Automatisera skapandet av en Application Insights-resurs</span><span class="sxs-lookup"><span data-stu-id="00752-218">Automate creating an Application Insights resource</span></span>](app-insights-powershell.md)
