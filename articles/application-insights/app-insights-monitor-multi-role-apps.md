---
title: "aaaAzure Application Insights stöd för flera komponenter, mikrotjänster och behållare | Microsoft Docs"
description: "Övervakning av appar som består av flera komponenter eller roller för prestanda och användning."
services: application-insights
documentationcenter: 
author: CFreemanwa
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 05/17/2017
ms.author: bwren
ms.openlocfilehash: 6185eedf32ec450d7541603b94de6c3dcdf64a85
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-multi-component-applications-with-application-insights-preview"></a><span data-ttu-id="ce97d-103">Övervaka flera komponenten program med Application Insights (förhandsversion)</span><span class="sxs-lookup"><span data-stu-id="ce97d-103">Monitor multi-component applications with Application Insights (preview)</span></span>

<span data-ttu-id="ce97d-104">Du kan övervaka appar som består av flera server-komponenter, roller eller -tjänster med [Azure Application Insights](app-insights-overview.md).</span><span class="sxs-lookup"><span data-stu-id="ce97d-104">You can monitor apps that consist of multiple server components, roles, or services with [Azure Application Insights](app-insights-overview.md).</span></span> <span data-ttu-id="ce97d-105">hello hälsotillståndet för hello komponenter och hello relationer mellan dem visas på en enda programavbildningen.</span><span class="sxs-lookup"><span data-stu-id="ce97d-105">hello health of hello components and hello relationships between them are displayed on a single Application Map.</span></span> <span data-ttu-id="ce97d-106">Du kan spåra enskilda åtgärder med hjälp av flera komponenter med automatisk HTTP korrelation.</span><span class="sxs-lookup"><span data-stu-id="ce97d-106">You can trace individual operations through multiple components with automatic HTTP correlation.</span></span> <span data-ttu-id="ce97d-107">Behållaren diagnostik integrering och samverkar med programtelemetri.</span><span class="sxs-lookup"><span data-stu-id="ce97d-107">Container diagnostics can be integrated and correlated with application telemetry.</span></span> <span data-ttu-id="ce97d-108">Använda en enda Application Insights-resurs för alla hello komponenter för programmet.</span><span class="sxs-lookup"><span data-stu-id="ce97d-108">Use a single Application Insights resource for all hello components of your application.</span></span> 

![Flera komponenten programavbildningen](./media/app-insights-monitor-multi-role-apps/app-map.png)

<span data-ttu-id="ce97d-110">Vi använder ”komponent' här toomean någon fungerande del av ett omfattande program.</span><span class="sxs-lookup"><span data-stu-id="ce97d-110">We use 'component' here toomean any functioning part of a large application.</span></span> <span data-ttu-id="ce97d-111">Till exempel en typisk affärsprogram kan bestå av klientkod som körs i webbläsare, prata tooone eller mer app webbtjänster, vilka i sin tur använder tillbaka avsluta tjänster.</span><span class="sxs-lookup"><span data-stu-id="ce97d-111">For example, a typical business application may consist of client code running in web browsers, talking tooone or more web app services, which in turn use back end services.</span></span> <span data-ttu-id="ce97d-112">Server-komponenter kan vara lokalt på i hello moln, eller kanske Azure webb- och arbetsroller roller eller kan köras i behållare, till exempel Docker eller Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="ce97d-112">Server components may be hosted on-premises on in hello cloud, or may be Azure web and worker roles, or may run in containers such as Docker or Service Fabric.</span></span> 

### <a name="sharing-a-single-application-insights-resource"></a><span data-ttu-id="ce97d-113">Dela en enda Application Insights-resurs</span><span class="sxs-lookup"><span data-stu-id="ce97d-113">Sharing a single Application Insights resource</span></span> 

<span data-ttu-id="ce97d-114">hello viktiga tekniken här är toosend telemetri från varje komponent i ditt program toohello samma Application Insights-resurs, men Använd hello `cloud_RoleName` egenskapen toodistinguish komponenter vid behov.</span><span class="sxs-lookup"><span data-stu-id="ce97d-114">hello key technique here is toosend telemetry from every component in your application toohello same Application Insights resource, but use hello `cloud_RoleName` property toodistinguish components when necessary.</span></span> <span data-ttu-id="ce97d-115">hello Application Insights SDK lägger till hello `cloud_RoleName` egenskapen toohello telemetri komponenter genererar.</span><span class="sxs-lookup"><span data-stu-id="ce97d-115">hello Application Insights SDK adds hello `cloud_RoleName` property toohello telemetry components emit.</span></span> <span data-ttu-id="ce97d-116">Till exempel hello SDK kommer att lägga till en webbplatsens namn eller tjänsten rollen namn toohello `cloud_RoleName` egenskapen.</span><span class="sxs-lookup"><span data-stu-id="ce97d-116">For example, hello SDK will add a web site name, or service role name toohello `cloud_RoleName` property.</span></span> <span data-ttu-id="ce97d-117">Du kan åsidosätta detta värde med en telemetryinitializer.</span><span class="sxs-lookup"><span data-stu-id="ce97d-117">You can override this value with a telemetryinitializer.</span></span> <span data-ttu-id="ce97d-118">hello programavbildningen använder hello `cloud_RoleName` egenskapen tooidentify hello komponenter på hello karta.</span><span class="sxs-lookup"><span data-stu-id="ce97d-118">hello Application Map uses hello `cloud_RoleName` property tooidentify hello components on hello map.</span></span>

<span data-ttu-id="ce97d-119">Mer information om hur Åsidosätt hello `cloud_RoleName` egenskapen Se [lägga till egenskaper: ITelemetryInitializer](app-insights-api-filtering-sampling.md#add-properties-itelemetryinitializer).</span><span class="sxs-lookup"><span data-stu-id="ce97d-119">For more information about how do override hello `cloud_RoleName` property see [Add properties: ITelemetryInitializer](app-insights-api-filtering-sampling.md#add-properties-itelemetryinitializer).</span></span>  

<span data-ttu-id="ce97d-120">I vissa fall kan detta kanske inte är rätt och du kanske föredrar toouse separata resurser för olika grupper av komponenter.</span><span class="sxs-lookup"><span data-stu-id="ce97d-120">In some cases, this may not be appropriate, and you may prefer toouse separate resources for different groups of components.</span></span> <span data-ttu-id="ce97d-121">Till exempel behöva toouse olika resurser för att hantera fakturering.</span><span class="sxs-lookup"><span data-stu-id="ce97d-121">For example, you might need toouse different resources for management or billing purposes.</span></span> <span data-ttu-id="ce97d-122">Med hjälp av separata resurser innebär att du inte ser alla hello-komponenter som visas på en enda programavbildningen; och att du kan inte fråga på komponenter i [Analytics](app-insights-analytics.md).</span><span class="sxs-lookup"><span data-stu-id="ce97d-122">Using separate resources means that you don't see all hello components displayed on a single Application Map; and that you can't query across components in [Analytics](app-insights-analytics.md).</span></span> <span data-ttu-id="ce97d-123">Du kan också ha tooset hello separata resurser.</span><span class="sxs-lookup"><span data-stu-id="ce97d-123">You also have tooset up hello separate resources.</span></span>

<span data-ttu-id="ce97d-124">Med den begränsning antar vi i hello resten av det här dokumentet som du vill toosend data från flera komponenter tooone Application Insights-resurs.</span><span class="sxs-lookup"><span data-stu-id="ce97d-124">With that caveat, we'll assume in hello rest of this document that you want toosend data from multiple components tooone Application Insights resource.</span></span>

## <a name="configure-multi-component-applications"></a><span data-ttu-id="ce97d-125">Konfigurera flera komponenten program</span><span class="sxs-lookup"><span data-stu-id="ce97d-125">Configure multi-component applications</span></span>

<span data-ttu-id="ce97d-126">Mappa om tooget flera komponenten program måste du tooachieve dessa mål:</span><span class="sxs-lookup"><span data-stu-id="ce97d-126">tooget a multi-component application map, you need tooachieve these goals:</span></span>

* <span data-ttu-id="ce97d-127">**Installera hello senaste förhandsversionen** Application Insights-paketet i varje komponent av programmet hello.</span><span class="sxs-lookup"><span data-stu-id="ce97d-127">**Install hello latest pre-release** Application Insights package in each component of hello application.</span></span> 
* <span data-ttu-id="ce97d-128">**Dela en enda Application Insights-resurs** för alla hello serverkomponenter i ditt program.</span><span class="sxs-lookup"><span data-stu-id="ce97d-128">**Share a single Application Insights resource** for all hello components of your application.</span></span>
* <span data-ttu-id="ce97d-129">**Aktivera flera rollen programavbildningen** hello förhandsgranskningar-bladet.</span><span class="sxs-lookup"><span data-stu-id="ce97d-129">**Enable Multi-role Application Map** in hello Previews blade.</span></span>

<span data-ttu-id="ce97d-130">Konfigurera varje komponent i ditt program med hello lämplig metod för typen.</span><span class="sxs-lookup"><span data-stu-id="ce97d-130">Configure each component of your application using hello appropriate method for its type.</span></span> <span data-ttu-id="ce97d-131">([ASP.NET](app-insights-asp-net.md), [Java](app-insights-java-get-started.md), [Node.js](app-insights-nodejs.md), [JavaScript](app-insights-javascript.md).)</span><span class="sxs-lookup"><span data-stu-id="ce97d-131">([ASP.NET](app-insights-asp-net.md), [Java](app-insights-java-get-started.md), [Node.js](app-insights-nodejs.md), [JavaScript](app-insights-javascript.md).)</span></span>

### <a name="1-install-hello-latest-pre-release-package"></a><span data-ttu-id="ce97d-132">1. Installera hello senaste förhands-paket</span><span class="sxs-lookup"><span data-stu-id="ce97d-132">1. Install hello latest pre-release package</span></span>

<span data-ttu-id="ce97d-133">Uppdatera eller installera hello program insikter paket i hello projekt för varje serverkomponent.</span><span class="sxs-lookup"><span data-stu-id="ce97d-133">Update or install hello Appication Insights packages in hello project for each server component.</span></span> <span data-ttu-id="ce97d-134">Om du använder Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="ce97d-134">If you're using Visual Studio:</span></span>

1. <span data-ttu-id="ce97d-135">Högerklicka på projektet och välj **hantera NuGet-paket**.</span><span class="sxs-lookup"><span data-stu-id="ce97d-135">Right-click a project and select **Manage NuGet Packages**.</span></span> 
2. <span data-ttu-id="ce97d-136">Välj **inkludera förhandsversion**.</span><span class="sxs-lookup"><span data-stu-id="ce97d-136">Select **Include prerelease**.</span></span>
3. <span data-ttu-id="ce97d-137">Om Application Insights visas paketen i uppdateringar, markera dem.</span><span class="sxs-lookup"><span data-stu-id="ce97d-137">If Application Insights packages appear in Updates, select them.</span></span> 

    <span data-ttu-id="ce97d-138">Annars kan söka efter och installera hello lämpligt paket:</span><span class="sxs-lookup"><span data-stu-id="ce97d-138">Otherwise, browse for and install hello appropriate package:</span></span>
    
    * <span data-ttu-id="ce97d-139">Microsoft.ApplicationInsights.WindowsServer</span><span class="sxs-lookup"><span data-stu-id="ce97d-139">Microsoft.ApplicationInsights.WindowsServer</span></span>
    * <span data-ttu-id="ce97d-140">Microsoft.ApplicationInsights.ServiceFabric - komponenter som körs som gäst körbara filer och Docker-behållare som kör ett i Service Fabric-program</span><span class="sxs-lookup"><span data-stu-id="ce97d-140">Microsoft.ApplicationInsights.ServiceFabric - for components running as guest executables and Docker containers running a in Service Fabric application</span></span>
    * <span data-ttu-id="ce97d-141">Microsoft.ApplicationInsights.ServiceFabric.Native - för tillförlitlig tjänster i ServiceFabric program</span><span class="sxs-lookup"><span data-stu-id="ce97d-141">Microsoft.ApplicationInsights.ServiceFabric.Native - for reliable services in ServiceFabric applications</span></span>
    * <span data-ttu-id="ce97d-142">Microsoft.ApplicationInsights.Kubernetes för komponenter som körs i Docker på Kubernetes</span><span class="sxs-lookup"><span data-stu-id="ce97d-142">Microsoft.ApplicationInsights.Kubernetes for components running in Docker on Kubernetes</span></span>

### <a name="2-share-a-single-application-insights-resource"></a><span data-ttu-id="ce97d-143">2. Dela en enda Application Insights-resurs</span><span class="sxs-lookup"><span data-stu-id="ce97d-143">2. Share a single Application Insights resource</span></span>

* <span data-ttu-id="ce97d-144">Högerklicka på ett projekt i Visual Studio och välj **konfigurera Application Insights**, eller **Programinsikter > Konfigurera**.</span><span class="sxs-lookup"><span data-stu-id="ce97d-144">In Visual Studio, right-click a project and select **Configure Application Insights**, or **Application Insights > Configure**.</span></span> <span data-ttu-id="ce97d-145">Använd hello guiden toocreate Application Insights-resurs för första hello-projektet.</span><span class="sxs-lookup"><span data-stu-id="ce97d-145">For hello first project, use hello wizard toocreate an Application Insights resource.</span></span> <span data-ttu-id="ce97d-146">För efterföljande projekt hello väljer du samma resurs.</span><span class="sxs-lookup"><span data-stu-id="ce97d-146">For subsequent projects, select hello same resource.</span></span>
* <span data-ttu-id="ce97d-147">Om det finns inga Application Insights-menyn, konfigurera manuellt:</span><span class="sxs-lookup"><span data-stu-id="ce97d-147">If there is no Application Insights menu, configure manually:</span></span>

   1. <span data-ttu-id="ce97d-148">I [Azure-portalen](https://portal,azure.com), öppna hello Application Insights-resurs som du redan har skapats för en annan komponent.</span><span class="sxs-lookup"><span data-stu-id="ce97d-148">In [Azure portal](https://portal,azure.com), open hello Application Insights resource you already created for another component.</span></span>
   2. <span data-ttu-id="ce97d-149">I hello översikt bladet och öppna hello Essentials nedrullningsbara fliken kopia hello **Instrumentation nyckel.**</span><span class="sxs-lookup"><span data-stu-id="ce97d-149">In hello Overview blade, open hello Essentials drop-down tab, and copy hello **Instrumentation Key.**</span></span>
   3. <span data-ttu-id="ce97d-150">Öppna ApplicationInsights.config och infoga i projektet:`<InstrumentationKey>your copied key</InstrumentationKey>`</span><span class="sxs-lookup"><span data-stu-id="ce97d-150">In your project, open ApplicationInsights.config and insert: `<InstrumentationKey>your copied key</InstrumentationKey>`</span></span>

![Kopiera hello instrumentation viktiga toohello .config-fil](./media/app-insights-monitor-multi-role-apps/copy-instrumentation-key.png)


### <a name="3-enable-multi-role-application-map"></a><span data-ttu-id="ce97d-152">3. Aktivera flera rollen programavbildningen</span><span class="sxs-lookup"><span data-stu-id="ce97d-152">3. Enable multi-role Application Map</span></span>

<span data-ttu-id="ce97d-153">Öppna hello resurs för ditt program i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="ce97d-153">In hello Azure portal, open hello resource for your application.</span></span> <span data-ttu-id="ce97d-154">Aktivera i hello förhandsgranskningar bladet *flera rollen programavbildningen*.</span><span class="sxs-lookup"><span data-stu-id="ce97d-154">In hello Previews blade, enable *Multi-role Application Map*.</span></span>

### <a name="4-enable-docker-metrics-optional"></a><span data-ttu-id="ce97d-155">4. Aktivera Docker mätvärden (valfritt)</span><span class="sxs-lookup"><span data-stu-id="ce97d-155">4. Enable Docker metrics (Optional)</span></span> 

<span data-ttu-id="ce97d-156">Om en komponent som körs i en Docker som finns på en Windows Azure-dator, kan du samla in ytterligare mått från hello behållare.</span><span class="sxs-lookup"><span data-stu-id="ce97d-156">If a component runs in a Docker hosted on an Azure Windows VM, you can collect additional metrics from hello container.</span></span> <span data-ttu-id="ce97d-157">Infoga det i ditt [Azure diagnostics](../monitoring-and-diagnostics/azure-diagnostics.md) konfigurationsfil:</span><span class="sxs-lookup"><span data-stu-id="ce97d-157">Insert this in your [Azure diagnostics](../monitoring-and-diagnostics/azure-diagnostics.md) configuration file:</span></span>

```
"DiagnosticMonitorConfiguration": {
        ...
        "sinks": "applicationInsights",
        "DockerSources": {
                "Stats": {
                    "enabled": true,
                    "sampleRate": "PT1M"
                }
            },
        ...
    }
    ...   
    "SinksConfig": {
        "Sink": [{
            "name": "applicationInsights",
            "ApplicationInsights": "<your instrumentation key here>"
        }]
    }
    ...
}

```

## <a name="use-cloudrolename-tooseparate-components"></a><span data-ttu-id="ce97d-158">Använda cloud_RoleName tooseparate komponenter</span><span class="sxs-lookup"><span data-stu-id="ce97d-158">Use cloud_RoleName tooseparate components</span></span>

<span data-ttu-id="ce97d-159">Hej `cloud_RoleName` egenskapen är anslutna tooall telemetri.</span><span class="sxs-lookup"><span data-stu-id="ce97d-159">hello `cloud_RoleName` property is attached tooall telemetry.</span></span> <span data-ttu-id="ce97d-160">Den identifierar hello komponent - hello roll eller tjänst - kommer hello telemetri.</span><span class="sxs-lookup"><span data-stu-id="ce97d-160">It identifies hello component - hello role or service - that originates hello telemetry.</span></span> <span data-ttu-id="ce97d-161">(Det är inte hello samma som cloud_RoleInstance som separerar identiska roller som körs parallellt på flera serverprocesser eller datorer.)</span><span class="sxs-lookup"><span data-stu-id="ce97d-161">(It is not hello same as cloud_RoleInstance, which separates identical roles that are running in parallel on multiple server processes or machines.)</span></span>

<span data-ttu-id="ce97d-162">Du kan filtrera eller segmentera dina telemetri som använder den här egenskapen i hello-portalen.</span><span class="sxs-lookup"><span data-stu-id="ce97d-162">In hello portal, you can filter or segment your telemetry using this property.</span></span> <span data-ttu-id="ce97d-163">I det här exemplet är hello fel bladet filtrerade tooshow bara information från hello frontwebbservern tjänsten, filtrera fel från hello CRM API-serverdel:</span><span class="sxs-lookup"><span data-stu-id="ce97d-163">In this example, hello Failures blade is filtered tooshow just information from hello front-end web service, filtering out failures from hello CRM API backend:</span></span>

![Mått diagram åtskilda med rollen Molnnamn](./media/app-insights-monitor-multi-role-apps/cloud-role-name.png)

## <a name="trace-operations-between-components"></a><span data-ttu-id="ce97d-165">Spåra åtgärder mellan komponenter</span><span class="sxs-lookup"><span data-stu-id="ce97d-165">Trace operations between components</span></span>

<span data-ttu-id="ce97d-166">Du kan spåra från en komponent tooanother, hello-anrop som görs under bearbetning av en enskild åtgärd.</span><span class="sxs-lookup"><span data-stu-id="ce97d-166">You can trace from one component tooanother, hello calls made while processing an individual operation.</span></span>


![Visa telemetri för åtgärden](./media/app-insights-monitor-multi-role-apps/show-telemetry-for-operation.png)

<span data-ttu-id="ce97d-168">Klicka dig igenom tooa korrelerade lista över telemetri för den här åtgärden över hello frontwebbservern och hello backend-API:</span><span class="sxs-lookup"><span data-stu-id="ce97d-168">Click through tooa correlated list of telemetry for this operation across hello front-end web server and hello back-end API:</span></span>

![Söka i komponenter](./media/app-insights-monitor-multi-role-apps/search-across-components.png)


## <a name="next-steps"></a><span data-ttu-id="ce97d-170">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="ce97d-170">Next steps</span></span>

* [<span data-ttu-id="ce97d-171">Separata telemetri från utveckling, Test och produktion</span><span class="sxs-lookup"><span data-stu-id="ce97d-171">Separate telemetry from Development, Test, and Production</span></span>](app-insights-separate-resources.md)
