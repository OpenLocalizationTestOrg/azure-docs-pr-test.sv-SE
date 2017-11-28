---
title: "aaaGet igång med test i produktion för webbprogram"
description: "Läs mer om hello Test i produktion (TiP)-funktionen i Azure App Service Web Apps."
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
ms.openlocfilehash: 2ddbd532ffe2a4f3e07fd386d9741a3fde3639ca
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-test-in-production-for-web-apps"></a><span data-ttu-id="13d2e-103">Kom igång med testa i produktion för Web Apps</span><span class="sxs-lookup"><span data-stu-id="13d2e-103">Get started with test in production for Web Apps</span></span>
<span data-ttu-id="13d2e-104">Testa i produktion eller live-testa ditt webbprogram med levande kunden trafik, är en test-strategi som appen utvecklare integrera allt i sina [flexibel utveckling](https://en.wikipedia.org/wiki/Agile_software_development) metoder.</span><span class="sxs-lookup"><span data-stu-id="13d2e-104">Testing in production, or live-testing your web app using live customer traffic, is a test strategy that app developers increasingly integrate into their [agile development](https://en.wikipedia.org/wiki/Agile_software_development) methodology.</span></span> <span data-ttu-id="13d2e-105">Den låter dig tootest hello kvaliteten på dina appar med levande användaraktiviteten i produktionsmiljön, som skillnad från toosynthesized data i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="13d2e-105">It enables you tootest hello quality of your apps with live user traffic in your production environment, as opposed toosynthesized data in a test environment.</span></span> <span data-ttu-id="13d2e-106">Genom att exponera dina nya app tooreal användare, kan du få information om hello verkliga problem med din app kan stöta på när den har distribuerats.</span><span class="sxs-lookup"><span data-stu-id="13d2e-106">By exposing your new app tooreal users, you can be informed on hello real problems your app may face once it is deployed.</span></span> <span data-ttu-id="13d2e-107">Du kan kontrollera hello funktionalitet, prestanda och värdet för din app-uppdateringar mot hello volym, hastighet och antal användartrafiken, som du kan aldrig ungefärliga i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="13d2e-107">You can verify hello functionality, performance, and value of your app updates against hello volume, velocity, and variety of real user traffic, which you can never approximate in a test environment.</span></span>

## <a name="traffic-routing-in-app-service-web-apps"></a><span data-ttu-id="13d2e-108">Trafik routning i App Service Web Apps</span><span class="sxs-lookup"><span data-stu-id="13d2e-108">Traffic Routing in App Service Web Apps</span></span>
<span data-ttu-id="13d2e-109">Med hello routning av nätverkstrafik funktion i [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714), du kan dirigera om en del av live användaren trafik tooone eller flera [distributionsfack](web-sites-staged-publishing.md), och analysera din app med [Azure-program Insikter](/services/application-insights/) eller [Azure HDInsight](/services/hdinsight/), eller ett verktyg från tredje part som [New Relic](/marketplace/partners/newrelic/newrelic/) toovalidate ändringen.</span><span class="sxs-lookup"><span data-stu-id="13d2e-109">With hello Traffic Routing feature in [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714), you can direct a portion of live user traffic tooone or more [deployment slots](web-sites-staged-publishing.md), and then analyze your app with [Azure Application Insights](/services/application-insights/) or [Azure HDInsight](/services/hdinsight/), or a third-party tool like [New Relic](/marketplace/partners/newrelic/newrelic/) toovalidate your change.</span></span> <span data-ttu-id="13d2e-110">Du kan exempelvis implementera hello följande scenarier med App Service:</span><span class="sxs-lookup"><span data-stu-id="13d2e-110">For example, you can implement hello following scenarios with App Service:</span></span>

* <span data-ttu-id="13d2e-111">Identifiera funktionella buggar eller identifiera flaskhalsar i uppdateringar tidigare toosite hela distributionen</span><span class="sxs-lookup"><span data-stu-id="13d2e-111">Discover functional bugs or pinpoint performance bottlenecks in your updates prior toosite-wide deployment</span></span>
* <span data-ttu-id="13d2e-112">Utföra ”kontrollerade test flygplan” dina ändringar genom att mäta användbarhet mått på hello beta-app</span><span class="sxs-lookup"><span data-stu-id="13d2e-112">Perform "controlled test flights" of your changes by measuring usability metrics on hello beta app</span></span>
* <span data-ttu-id="13d2e-113">Ökar gradvis tooa ny uppdatering och smidigt tillbaka ned toohello aktuell version om ett fel inträffar</span><span class="sxs-lookup"><span data-stu-id="13d2e-113">Gradually ramp up tooa new update, and gracefully back down toohello current version if an error occurs</span></span> 
* <span data-ttu-id="13d2e-114">Optimera verksamhetsresultat för din app genom att köra [A / B testar](https://en.wikipedia.org/wiki/A/B_testing) eller [multivariate testerna](https://en.wikipedia.org/wiki/Multivariate_testing_in_marketing) i flera distributionsplatser</span><span class="sxs-lookup"><span data-stu-id="13d2e-114">Optimize your app's business results by running [A/B tests](https://en.wikipedia.org/wiki/A/B_testing) or [multivariate tests](https://en.wikipedia.org/wiki/Multivariate_testing_in_marketing) in multiple deployment slots</span></span>

### <a name="requirements-for-using-traffic-routing-in-web-apps"></a><span data-ttu-id="13d2e-115">Krav för att använda Routning av nätverkstrafik i Web Apps</span><span class="sxs-lookup"><span data-stu-id="13d2e-115">Requirements for using Traffic Routing in Web Apps</span></span>
* <span data-ttu-id="13d2e-116">Ditt webbprogram måste köras i **Standard** eller **Premium** nivån, eftersom det krävs för flera distributionsplatser.</span><span class="sxs-lookup"><span data-stu-id="13d2e-116">Your web app must run in **Standard** or **Premium** tier, as it is required for multiple deployment slots.</span></span>
* <span data-ttu-id="13d2e-117">I ordning toowork korrekt trafikroutning cookies toobe aktiverat i hello användarnas webbläsare.</span><span class="sxs-lookup"><span data-stu-id="13d2e-117">In order toowork properly, Traffic Routing requires cookies toobe enabled in hello users' browser.</span></span> <span data-ttu-id="13d2e-118">Trafik Routning använder cookies toopin en distributionsplats för klientens webbläsare tooa för hello livslängd hello klientsessionen.</span><span class="sxs-lookup"><span data-stu-id="13d2e-118">Traffic Routing uses cookies toopin a client browser tooa deployment slot for hello life hello client session.</span></span>
* <span data-ttu-id="13d2e-119">Trafik routning stöd för avancerade scenarier tips via Azure PowerShell-cmdlets.</span><span class="sxs-lookup"><span data-stu-id="13d2e-119">Traffic Routing supports advanced TiP scenarios through Azure PowerShell cmdlets.</span></span>

## <a name="route-traffic-segment-tooa-deployment-slot"></a><span data-ttu-id="13d2e-120">Väg trafik segment tooa distributionsplatsen</span><span class="sxs-lookup"><span data-stu-id="13d2e-120">Route traffic segment tooa deployment slot</span></span>
<span data-ttu-id="13d2e-121">På hello grundläggande nivå i varje scenario tips vidarebefordra fördefinierade procent av din live trafik tooa icke-produktion distributionsplatsen.</span><span class="sxs-lookup"><span data-stu-id="13d2e-121">At hello basic level in every TiP scenario, you route a predefined percentage of your live traffic tooa non-production deployment slot.</span></span> <span data-ttu-id="13d2e-122">toodo detta, följ hello stegen nedan:</span><span class="sxs-lookup"><span data-stu-id="13d2e-122">toodo this, follow hello steps below:</span></span>

> [!NOTE]
> <span data-ttu-id="13d2e-123">hello här stegen förutsätter att du redan har en [icke-produktion distributionsplatsen](web-sites-staged-publishing.md) och den hello önskat app webbinnehåll är redan [distribueras](web-sites-deploy.md) tooit.</span><span class="sxs-lookup"><span data-stu-id="13d2e-123">hello steps here assumes that you already have a [non-production deployment slot](web-sites-staged-publishing.md) and that hello desired web app content is already [deployed](web-sites-deploy.md) tooit.</span></span>
> 
> 

1. <span data-ttu-id="13d2e-124">Logga in på hello [Azure Portal](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="13d2e-124">Log into hello [Azure Portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="13d2e-125">I din webbapps blad klickar du på **inställningar** > **Trafikroutning**.</span><span class="sxs-lookup"><span data-stu-id="13d2e-125">In your web app's blade, click **Settings** > **Traffic Routing**.</span></span>
   ![](./media/app-service-web-test-in-production/01-traffic-routing.png)
3. <span data-ttu-id="13d2e-126">Välj hello-plats som du vill tooroute trafik tooand hello procentandelen hello totala trafiken du vill ha och klickar sedan **spara**.</span><span class="sxs-lookup"><span data-stu-id="13d2e-126">Select hello slot that you want tooroute traffic tooand hello percentage of hello total traffic you desire, then click **Save**.</span></span>
   
    ![](./media/app-service-web-test-in-production/02-select-slot.png)
4. <span data-ttu-id="13d2e-127">Gå toohello distribution fack-bladet.</span><span class="sxs-lookup"><span data-stu-id="13d2e-127">Go toohello deployment slot's blade.</span></span> <span data-ttu-id="13d2e-128">Du bör nu se live trafik som dirigeras tooit.</span><span class="sxs-lookup"><span data-stu-id="13d2e-128">You should now see live traffic being routed tooit.</span></span>
   
    ![](./media/app-service-web-test-in-production/03-traffic-routed.png)

<span data-ttu-id="13d2e-129">När du har konfigurerat routning av nätverkstrafik angetts hello procentandelen klienter kommer att slumpmässigt routade tooyour icke-produktionsplatsen.</span><span class="sxs-lookup"><span data-stu-id="13d2e-129">Once Traffic Routing is configured, hello specified percentage of clients will be randomly routed tooyour non-production slot.</span></span> <span data-ttu-id="13d2e-130">Det är dock viktigt toonote att när en klient är automatiskt routade tooa specifika plats, blir ”fasta” toothat plats för hello livslängd session som klienten.</span><span class="sxs-lookup"><span data-stu-id="13d2e-130">However, it is important toonote that once a client is automatically routed tooa specific slot, it will be "pinned" toothat slot for hello life of that client session.</span></span> <span data-ttu-id="13d2e-131">Detta har gjort med hjälp av en cookie toopin hello användarsession.</span><span class="sxs-lookup"><span data-stu-id="13d2e-131">This done using a cookie toopin hello user session.</span></span> <span data-ttu-id="13d2e-132">Om du inspektera hello HTTP-begäranden, hittar du en `TipMix` cookie i varje efterföljande begäran.</span><span class="sxs-lookup"><span data-stu-id="13d2e-132">If you inspect hello HTTP requests, you will find a `TipMix` cookie in every subsequent request.</span></span>

![](./media/app-service-web-test-in-production/04-tip-cookie.png)

## <a name="force-client-requests-tooa-specific-slot"></a><span data-ttu-id="13d2e-133">Tvinga klienten begäranden tooa specifik plats</span><span class="sxs-lookup"><span data-stu-id="13d2e-133">Force client requests tooa specific slot</span></span>
<span data-ttu-id="13d2e-134">Apptjänst är i tillägg tooautomatic routning av nätverkstrafik, kan tooroute begäranden tooa specifik plats.</span><span class="sxs-lookup"><span data-stu-id="13d2e-134">In addition tooautomatic traffic routing, App Service is able tooroute requests tooa specific slot.</span></span> <span data-ttu-id="13d2e-135">Detta är användbart när du vill att dina användare toobe kan tooopt-till eller CEIP beta-app.</span><span class="sxs-lookup"><span data-stu-id="13d2e-135">This is useful when you want your users toobe able tooopt-into or opt-out of your beta app.</span></span> <span data-ttu-id="13d2e-136">toodo kan du använda hello `x-ms-routing-name` Frågeparametern.</span><span class="sxs-lookup"><span data-stu-id="13d2e-136">toodo this, you use hello `x-ms-routing-name` query parameter.</span></span>

<span data-ttu-id="13d2e-137">tooreroute användare tooa specifika plats använder `x-ms-routing-name`, måste du kontrollera hello platsen har redan lagts till toohello routning av nätverkstrafik lista.</span><span class="sxs-lookup"><span data-stu-id="13d2e-137">tooreroute users tooa specific slot using `x-ms-routing-name`, you must make sure that hello slot is already added toohello Traffic Routing list.</span></span> <span data-ttu-id="13d2e-138">Eftersom du uttryckligen vill tooroute tooa fack hello faktiska routning procentandel du anger spelar ingen roll.</span><span class="sxs-lookup"><span data-stu-id="13d2e-138">Since you want tooroute tooa slot explicitly, hello actual routing percentage you set doesn't matter.</span></span> <span data-ttu-id="13d2e-139">Om du vill kan du skapa en ”länk av beta” som användarna kan klicka på tooaccess hello beta-app.</span><span class="sxs-lookup"><span data-stu-id="13d2e-139">If you want, you can craft a "beta link" that users can click tooaccess hello beta app.</span></span>

![](./media/app-service-web-test-in-production/06-enable-x-ms-routing-name.png)

### <a name="opt-users-out-of-beta-app"></a><span data-ttu-id="13d2e-140">Välja användare utanför beta-appen</span><span class="sxs-lookup"><span data-stu-id="13d2e-140">Opt users out of beta app</span></span>
<span data-ttu-id="13d2e-141">toolet användarna välja utanför beta-appen, till exempel kan du placera den här länken på din webbsida:</span><span class="sxs-lookup"><span data-stu-id="13d2e-141">toolet users opt out of your beta app, for example, you can put this link in your web page:</span></span>

    <a href="<webappname>.azurewebsites.net/?x-ms-routing-name=self">Go back tooproduction app</a>

<span data-ttu-id="13d2e-142">Hej sträng `x-ms-routing-name=self` anger hello produktionsplatsen.</span><span class="sxs-lookup"><span data-stu-id="13d2e-142">hello string `x-ms-routing-name=self` specifies hello production slot.</span></span> <span data-ttu-id="13d2e-143">När hello klientens webbläsare åtkomst hello länken är inte bara den omdirigerade toohello produktionsplatsen, men varje efterföljande begäran innehåller hello `x-ms-routing-name=self` cookie som PIN-hello session toohello produktionsplatsen.</span><span class="sxs-lookup"><span data-stu-id="13d2e-143">Once hello client browser access hello link, not only is it redirected toohello production slot, but every subsequent request will contain hello `x-ms-routing-name=self` cookie that pins hello session toohello production slot.</span></span>

![](./media/app-service-web-test-in-production/05-access-production-slot.png)

### <a name="opt-users-in-toobeta-app"></a><span data-ttu-id="13d2e-144">Välja användare i toobeta app</span><span class="sxs-lookup"><span data-stu-id="13d2e-144">Opt users in toobeta app</span></span>
<span data-ttu-id="13d2e-145">toolet användarna välja i tooyour beta app, ange hello samma fråga toohello parameternamn för hello icke-produktionsplatsen, till exempel:</span><span class="sxs-lookup"><span data-stu-id="13d2e-145">toolet users opt in tooyour beta app, set hello same query parameter toohello name of hello non-production slot, for example:</span></span>

        <webappname>.azurewebsites.net/?x-ms-routing-name=staging

## <a name="more-resources"></a><span data-ttu-id="13d2e-146">Fler resurser</span><span class="sxs-lookup"><span data-stu-id="13d2e-146">More resources</span></span>
* [<span data-ttu-id="13d2e-147">Skapa mellanlagringsmiljöer för web apps i Azure App Service</span><span class="sxs-lookup"><span data-stu-id="13d2e-147">Set up staging environments for web apps in Azure App Service</span></span>](web-sites-staged-publishing.md)
* [<span data-ttu-id="13d2e-148">Distribuera ett komplexa program förutsägbart i Azure</span><span class="sxs-lookup"><span data-stu-id="13d2e-148">Deploy a complex application predictably in Azure</span></span>](app-service-deploy-complex-application-predictably.md)
* [<span data-ttu-id="13d2e-149">Flexibel programvaruutveckling med Azure App Service</span><span class="sxs-lookup"><span data-stu-id="13d2e-149">Agile software development with Azure App Service</span></span>](app-service-agile-software-development.md)
* [<span data-ttu-id="13d2e-150">Använda DevOps miljöer effektivt för ditt webbprogram</span><span class="sxs-lookup"><span data-stu-id="13d2e-150">Use DevOps environments effectively for your web apps</span></span>](app-service-web-staged-publishing-realworld-scenarios.md)

