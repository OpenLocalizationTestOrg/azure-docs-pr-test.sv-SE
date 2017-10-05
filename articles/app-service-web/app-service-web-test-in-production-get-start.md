---
title: "Kom igång med testa i produktion för Web Apps"
description: "Läs mer om Test i produktion (TiP)-funktionen i Azure App Service Web Apps."
services: app-service\web
documentationcenter: 
author: cephalin
manager: erikre
editor: 
ms.assetid: 4623468d-886e-4203-8012-8f86deb2790b
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/13/2016
ms.author: cephalin
ms.openlocfilehash: 9f38b635140cacf0513c75385bce3c110a930969
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="get-started-with-test-in-production-for-web-apps"></a><span data-ttu-id="44d3a-103">Kom igång med testa i produktion för Web Apps</span><span class="sxs-lookup"><span data-stu-id="44d3a-103">Get started with test in production for Web Apps</span></span>
<span data-ttu-id="44d3a-104">Testa i produktion eller live-testa ditt webbprogram med levande kunden trafik, är en test-strategi som appen utvecklare integrera allt i sina [flexibel utveckling](https://en.wikipedia.org/wiki/Agile_software_development) metoder.</span><span class="sxs-lookup"><span data-stu-id="44d3a-104">Testing in production, or live-testing your web app using live customer traffic, is a test strategy that app developers increasingly integrate into their [agile development](https://en.wikipedia.org/wiki/Agile_software_development) methodology.</span></span> <span data-ttu-id="44d3a-105">På så sätt kan du testa kvaliteten på dina appar med levande användaraktiviteten i produktionsmiljön, till skillnad från syntetiskt data i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="44d3a-105">It enables you to test the quality of your apps with live user traffic in your production environment, as opposed to synthesized data in a test environment.</span></span> <span data-ttu-id="44d3a-106">Genom att exponera din nya app till verkliga användare, kan du få information om verkliga problem som din app kan stöta på när den har distribuerats.</span><span class="sxs-lookup"><span data-stu-id="44d3a-106">By exposing your new app to real users, you can be informed on the real problems your app may face once it is deployed.</span></span> <span data-ttu-id="44d3a-107">Du kan kontrollera funktionalitet, prestanda och värdet för din app-uppdateringar mot volymen, hastighet och antal användartrafiken, som du kan aldrig ungefärliga i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="44d3a-107">You can verify the functionality, performance, and value of your app updates against the volume, velocity, and variety of real user traffic, which you can never approximate in a test environment.</span></span>

## <a name="traffic-routing-in-app-service-web-apps"></a><span data-ttu-id="44d3a-108">Trafik routning i App Service Web Apps</span><span class="sxs-lookup"><span data-stu-id="44d3a-108">Traffic Routing in App Service Web Apps</span></span>
<span data-ttu-id="44d3a-109">Med funktionen routning av nätverkstrafik i [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714), du kan dirigera om en del av live användartrafik till en eller flera [distributionsfack](web-sites-staged-publishing.md), och analysera din app med [Azure-program Insikter](/services/application-insights/) eller [Azure HDInsight](/services/hdinsight/), eller ett verktyg från tredje part som [New Relic](/marketplace/partners/newrelic/newrelic/) validera din ändring.</span><span class="sxs-lookup"><span data-stu-id="44d3a-109">With the Traffic Routing feature in [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714), you can direct a portion of live user traffic to one or more [deployment slots](web-sites-staged-publishing.md), and then analyze your app with [Azure Application Insights](/services/application-insights/) or [Azure HDInsight](/services/hdinsight/), or a third-party tool like [New Relic](/marketplace/partners/newrelic/newrelic/) to validate your change.</span></span> <span data-ttu-id="44d3a-110">Du kan exempelvis implementera följande scenarier med App Service:</span><span class="sxs-lookup"><span data-stu-id="44d3a-110">For example, you can implement the following scenarios with App Service:</span></span>

* <span data-ttu-id="44d3a-111">Identifiera funktionella buggar eller identifiera flaskhalsar i dina uppdateringar före distributionen av hela platsen</span><span class="sxs-lookup"><span data-stu-id="44d3a-111">Discover functional bugs or pinpoint performance bottlenecks in your updates prior to site-wide deployment</span></span>
* <span data-ttu-id="44d3a-112">Utföra ”kontrollerade test flygplan” dina ändringar genom att mäta användbarhet mått på beta-app</span><span class="sxs-lookup"><span data-stu-id="44d3a-112">Perform "controlled test flights" of your changes by measuring usability metrics on the beta app</span></span>
* <span data-ttu-id="44d3a-113">Gradvis snabbt skala upp till en ny uppdatering och smidigt tillbaka till den aktuella versionen om ett fel inträffar</span><span class="sxs-lookup"><span data-stu-id="44d3a-113">Gradually ramp up to a new update, and gracefully back down to the current version if an error occurs</span></span> 
* <span data-ttu-id="44d3a-114">Optimera verksamhetsresultat för din app genom att köra [A / B testar](https://en.wikipedia.org/wiki/A/B_testing) eller [multivariate testerna](https://en.wikipedia.org/wiki/Multivariate_testing_in_marketing) i flera distributionsplatser</span><span class="sxs-lookup"><span data-stu-id="44d3a-114">Optimize your app's business results by running [A/B tests](https://en.wikipedia.org/wiki/A/B_testing) or [multivariate tests](https://en.wikipedia.org/wiki/Multivariate_testing_in_marketing) in multiple deployment slots</span></span>

### <a name="requirements-for-using-traffic-routing-in-web-apps"></a><span data-ttu-id="44d3a-115">Krav för att använda Routning av nätverkstrafik i Web Apps</span><span class="sxs-lookup"><span data-stu-id="44d3a-115">Requirements for using Traffic Routing in Web Apps</span></span>
* <span data-ttu-id="44d3a-116">Ditt webbprogram måste köras i **Standard** eller **Premium** nivån, eftersom det krävs för flera distributionsplatser.</span><span class="sxs-lookup"><span data-stu-id="44d3a-116">Your web app must run in **Standard** or **Premium** tier, as it is required for multiple deployment slots.</span></span>
* <span data-ttu-id="44d3a-117">Trafikroutning kräver cookies är aktiverade i användarnas webbläsare för att fungera korrekt.</span><span class="sxs-lookup"><span data-stu-id="44d3a-117">In order to work properly, Traffic Routing requires cookies to be enabled in the users' browser.</span></span> <span data-ttu-id="44d3a-118">Trafik Routning använder cookies för att fästa en klientwebbläsare till en distributionsplats för livslängd klientsessionen.</span><span class="sxs-lookup"><span data-stu-id="44d3a-118">Traffic Routing uses cookies to pin a client browser to a deployment slot for the life the client session.</span></span>
* <span data-ttu-id="44d3a-119">Trafik routning stöd för avancerade scenarier tips via Azure PowerShell-cmdlets.</span><span class="sxs-lookup"><span data-stu-id="44d3a-119">Traffic Routing supports advanced TiP scenarios through Azure PowerShell cmdlets.</span></span>

## <a name="route-traffic-segment-to-a-deployment-slot"></a><span data-ttu-id="44d3a-120">Väg trafik segmentet till en distributionsplats</span><span class="sxs-lookup"><span data-stu-id="44d3a-120">Route traffic segment to a deployment slot</span></span>
<span data-ttu-id="44d3a-121">På nivån basic i varje scenario tips vidarebefordra fördefinierade procent av din live trafik till en icke-produktion distributionsplats.</span><span class="sxs-lookup"><span data-stu-id="44d3a-121">At the basic level in every TiP scenario, you route a predefined percentage of your live traffic to a non-production deployment slot.</span></span> <span data-ttu-id="44d3a-122">Följ stegen nedan om du vill göra detta:</span><span class="sxs-lookup"><span data-stu-id="44d3a-122">To do this, follow the steps below:</span></span>

> [!NOTE]
> <span data-ttu-id="44d3a-123">De här stegen förutsätter att du redan har en [icke-produktion distributionsplatsen](web-sites-staged-publishing.md) och som önskade app webbinnehållet redan [distribueras](web-sites-deploy.md) till den.</span><span class="sxs-lookup"><span data-stu-id="44d3a-123">The steps here assumes that you already have a [non-production deployment slot](web-sites-staged-publishing.md) and that the desired web app content is already [deployed](web-sites-deploy.md) to it.</span></span>
> 
> 

1. <span data-ttu-id="44d3a-124">Logga in på den [Azure-portalen](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="44d3a-124">Log into the [Azure Portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="44d3a-125">I din webbapps blad klickar du på **inställningar** > **Trafikroutning**.</span><span class="sxs-lookup"><span data-stu-id="44d3a-125">In your web app's blade, click **Settings** > **Traffic Routing**.</span></span>
   ![](./media/app-service-web-test-in-production/01-traffic-routing.png)
3. <span data-ttu-id="44d3a-126">Välj den plats som du vill dirigera trafik till och procentandelen av den totala trafiken som du vill ha och klickar sedan **spara**.</span><span class="sxs-lookup"><span data-stu-id="44d3a-126">Select the slot that you want to route traffic to and the percentage of the total traffic you desire, then click **Save**.</span></span>
   
    ![](./media/app-service-web-test-in-production/02-select-slot.png)
4. <span data-ttu-id="44d3a-127">Gå till bladet för den distributionsplatsen.</span><span class="sxs-lookup"><span data-stu-id="44d3a-127">Go to the deployment slot's blade.</span></span> <span data-ttu-id="44d3a-128">Du bör nu se live trafik dirigeras till den.</span><span class="sxs-lookup"><span data-stu-id="44d3a-128">You should now see live traffic being routed to it.</span></span>
   
    ![](./media/app-service-web-test-in-production/03-traffic-routed.png)

<span data-ttu-id="44d3a-129">När du har konfigurerat routning av nätverkstrafik dirigeras den angivna procentandelen av klienter slumpmässigt till din produktionsplatsen.</span><span class="sxs-lookup"><span data-stu-id="44d3a-129">Once Traffic Routing is configured, the specified percentage of clients will be randomly routed to your non-production slot.</span></span> <span data-ttu-id="44d3a-130">Det är dock viktigt att Observera att när en klient omdirigeras automatiskt till en viss plats, den ”Fäst” till platsen för den klientsessionen.</span><span class="sxs-lookup"><span data-stu-id="44d3a-130">However, it is important to note that once a client is automatically routed to a specific slot, it will be "pinned" to that slot for the life of that client session.</span></span> <span data-ttu-id="44d3a-131">Detta har gjort med hjälp av en cookie fästa användarsessionen.</span><span class="sxs-lookup"><span data-stu-id="44d3a-131">This done using a cookie to pin the user session.</span></span> <span data-ttu-id="44d3a-132">Om du granska den HTTP-begäranden, hittar du en `TipMix` cookie i varje efterföljande begäran.</span><span class="sxs-lookup"><span data-stu-id="44d3a-132">If you inspect the HTTP requests, you will find a `TipMix` cookie in every subsequent request.</span></span>

![](./media/app-service-web-test-in-production/04-tip-cookie.png)

## <a name="force-client-requests-to-a-specific-slot"></a><span data-ttu-id="44d3a-133">Tvinga klientbegäranden till en specifik plats</span><span class="sxs-lookup"><span data-stu-id="44d3a-133">Force client requests to a specific slot</span></span>
<span data-ttu-id="44d3a-134">Förutom automatisk trafikroutning kan Apptjänst väg begäranden till en specifik plats.</span><span class="sxs-lookup"><span data-stu-id="44d3a-134">In addition to automatic traffic routing, App Service is able to route requests to a specific slot.</span></span> <span data-ttu-id="44d3a-135">Detta är användbart när du vill att användarna ska kunna välja i eller CEIP beta-app.</span><span class="sxs-lookup"><span data-stu-id="44d3a-135">This is useful when you want your users to be able to opt-into or opt-out of your beta app.</span></span> <span data-ttu-id="44d3a-136">Om du vill göra detta måste du använda den `x-ms-routing-name` Frågeparametern.</span><span class="sxs-lookup"><span data-stu-id="44d3a-136">To do this, you use the `x-ms-routing-name` query parameter.</span></span>

<span data-ttu-id="44d3a-137">Att omdirigera användare till en specifik plats med hjälp av `x-ms-routing-name`, måste du se till att platsen har redan lagts till i listan för routning av nätverkstrafik.</span><span class="sxs-lookup"><span data-stu-id="44d3a-137">To reroute users to a specific slot using `x-ms-routing-name`, you must make sure that the slot is already added to the Traffic Routing list.</span></span> <span data-ttu-id="44d3a-138">Eftersom du vill vidarebefordra till en plats uttryckligen faktiska routning procentandelen som du ställer in spelar ingen roll.</span><span class="sxs-lookup"><span data-stu-id="44d3a-138">Since you want to route to a slot explicitly, the actual routing percentage you set doesn't matter.</span></span> <span data-ttu-id="44d3a-139">Om du vill kan utforma du en ”länk av beta” som användarna kan klicka på för att komma åt appen beta.</span><span class="sxs-lookup"><span data-stu-id="44d3a-139">If you want, you can craft a "beta link" that users can click to access the beta app.</span></span>

![](./media/app-service-web-test-in-production/06-enable-x-ms-routing-name.png)

### <a name="opt-users-out-of-beta-app"></a><span data-ttu-id="44d3a-140">Välja användare utanför beta-appen</span><span class="sxs-lookup"><span data-stu-id="44d3a-140">Opt users out of beta app</span></span>
<span data-ttu-id="44d3a-141">Så att användarna kan välja bort appen beta, exempelvis kan du placera den här länken på din webbsida:</span><span class="sxs-lookup"><span data-stu-id="44d3a-141">To let users opt out of your beta app, for example, you can put this link in your web page:</span></span>

    <a href="<webappname>.azurewebsites.net/?x-ms-routing-name=self">Go back to production app</a>

<span data-ttu-id="44d3a-142">Strängen `x-ms-routing-name=self` anger produktionsplatsen.</span><span class="sxs-lookup"><span data-stu-id="44d3a-142">The string `x-ms-routing-name=self` specifies the production slot.</span></span> <span data-ttu-id="44d3a-143">När klientens webbläsare åtkomst till länken, inte bara omdirigeras till produktionsplatsen, men varje efterföljande begäran innehåller den `x-ms-routing-name=self` cookie som PIN-session till produktionsplatsen.</span><span class="sxs-lookup"><span data-stu-id="44d3a-143">Once the client browser access the link, not only is it redirected to the production slot, but every subsequent request will contain the `x-ms-routing-name=self` cookie that pins the session to the production slot.</span></span>

![](./media/app-service-web-test-in-production/05-access-production-slot.png)

### <a name="opt-users-in-to-beta-app"></a><span data-ttu-id="44d3a-144">Välja användare i beta-appen</span><span class="sxs-lookup"><span data-stu-id="44d3a-144">Opt users in to beta app</span></span>
<span data-ttu-id="44d3a-145">Så att användare kan välja att beta-app genom att ange samma Frågeparametern namnet på icke-produktionsplatsen, till exempel:</span><span class="sxs-lookup"><span data-stu-id="44d3a-145">To let users opt in to your beta app, set the same query parameter to the name of the non-production slot, for example:</span></span>

        <webappname>.azurewebsites.net/?x-ms-routing-name=staging

## <a name="more-resources"></a><span data-ttu-id="44d3a-146">Fler resurser</span><span class="sxs-lookup"><span data-stu-id="44d3a-146">More resources</span></span>
* [<span data-ttu-id="44d3a-147">Skapa mellanlagringsmiljöer för web apps i Azure App Service</span><span class="sxs-lookup"><span data-stu-id="44d3a-147">Set up staging environments for web apps in Azure App Service</span></span>](web-sites-staged-publishing.md)
* [<span data-ttu-id="44d3a-148">Distribuera ett komplexa program förutsägbart i Azure</span><span class="sxs-lookup"><span data-stu-id="44d3a-148">Deploy a complex application predictably in Azure</span></span>](app-service-deploy-complex-application-predictably.md)
* [<span data-ttu-id="44d3a-149">Flexibel programvaruutveckling med Azure App Service</span><span class="sxs-lookup"><span data-stu-id="44d3a-149">Agile software development with Azure App Service</span></span>](app-service-agile-software-development.md)
* [<span data-ttu-id="44d3a-150">Använda DevOps miljöer effektivt för ditt webbprogram</span><span class="sxs-lookup"><span data-stu-id="44d3a-150">Use DevOps environments effectively for your web apps</span></span>](app-service-web-staged-publishing-realworld-scenarios.md)

