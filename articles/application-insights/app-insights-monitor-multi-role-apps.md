---
title: "Azure Application Insights stöd för flera komponenter, mikrotjänster och behållare | Microsoft Docs"
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
ms.openlocfilehash: ca1bb8ee886c4b4e69be9dd653d6a52b874e1f5a
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="monitor-multi-component-applications-with-application-insights-preview"></a><span data-ttu-id="79f6a-103">Övervaka flera komponenten program med Application Insights (förhandsversion)</span><span class="sxs-lookup"><span data-stu-id="79f6a-103">Monitor multi-component applications with Application Insights (preview)</span></span>

<span data-ttu-id="79f6a-104">Du kan övervaka appar som består av flera server-komponenter, roller eller -tjänster med [Azure Application Insights](app-insights-overview.md).</span><span class="sxs-lookup"><span data-stu-id="79f6a-104">You can monitor apps that consist of multiple server components, roles, or services with [Azure Application Insights](app-insights-overview.md).</span></span> <span data-ttu-id="79f6a-105">Hälsotillståndet för komponenterna och relationer mellan dem visas på en enda programavbildningen.</span><span class="sxs-lookup"><span data-stu-id="79f6a-105">The health of the components and the relationships between them are displayed on a single Application Map.</span></span> <span data-ttu-id="79f6a-106">Du kan spåra enskilda åtgärder med hjälp av flera komponenter med automatisk HTTP korrelation.</span><span class="sxs-lookup"><span data-stu-id="79f6a-106">You can trace individual operations through multiple components with automatic HTTP correlation.</span></span> <span data-ttu-id="79f6a-107">Behållaren diagnostik integrering och samverkar med programtelemetri.</span><span class="sxs-lookup"><span data-stu-id="79f6a-107">Container diagnostics can be integrated and correlated with application telemetry.</span></span> <span data-ttu-id="79f6a-108">Använda en enda Application Insights-resurs för alla komponenter i ditt program.</span><span class="sxs-lookup"><span data-stu-id="79f6a-108">Use a single Application Insights resource for all the components of your application.</span></span> 

![Flera komponenten programavbildningen](./media/app-insights-monitor-multi-role-apps/app-map.png)

<span data-ttu-id="79f6a-110">Vi använder här ”komponent” betyder någon fungerande del av ett omfattande program.</span><span class="sxs-lookup"><span data-stu-id="79f6a-110">We use 'component' here to mean any functioning part of a large application.</span></span> <span data-ttu-id="79f6a-111">Till exempel en typisk affärsprogram kan bestå av klientkod som körs i webbläsare som kommunicerar med en eller flera web app-tjänster, som i sin tur använder tillbaka avsluta tjänster.</span><span class="sxs-lookup"><span data-stu-id="79f6a-111">For example, a typical business application may consist of client code running in web browsers, talking to one or more web app services, which in turn use back end services.</span></span> <span data-ttu-id="79f6a-112">Server-komponenter kan vara lokalt på i molnet, eller kanske Azure webb- och arbetsroller roller eller kan köras i behållare, till exempel Docker eller Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="79f6a-112">Server components may be hosted on-premises on in the cloud, or may be Azure web and worker roles, or may run in containers such as Docker or Service Fabric.</span></span> 

### <a name="sharing-a-single-application-insights-resource"></a><span data-ttu-id="79f6a-113">Dela en enda Application Insights-resurs</span><span class="sxs-lookup"><span data-stu-id="79f6a-113">Sharing a single Application Insights resource</span></span> 

<span data-ttu-id="79f6a-114">Med viktiga tekniken är att skicka telemetri från varje komponent i ditt program till samma Application Insights-resurs, men använda den `cloud_RoleName` egenskapen att skilja komponenter vid behov.</span><span class="sxs-lookup"><span data-stu-id="79f6a-114">The key technique here is to send telemetry from every component in your application to the same Application Insights resource, but use the `cloud_RoleName` property to distinguish components when necessary.</span></span> <span data-ttu-id="79f6a-115">Application Insights SDK lägger till den `cloud_RoleName` egenskapen telemetri komponenter genererar.</span><span class="sxs-lookup"><span data-stu-id="79f6a-115">The Application Insights SDK adds the `cloud_RoleName` property to the telemetry components emit.</span></span> <span data-ttu-id="79f6a-116">Till exempel SDK kommer att lägga till en webbplatsens namn eller tjänstnamnet som rollen ska den `cloud_RoleName` egenskapen.</span><span class="sxs-lookup"><span data-stu-id="79f6a-116">For example, the SDK will add a web site name, or service role name to the `cloud_RoleName` property.</span></span> <span data-ttu-id="79f6a-117">Du kan åsidosätta detta värde med en telemetryinitializer.</span><span class="sxs-lookup"><span data-stu-id="79f6a-117">You can override this value with a telemetryinitializer.</span></span> <span data-ttu-id="79f6a-118">Kartan program använder den `cloud_RoleName` egenskapen för att identifiera komponenterna på kartan.</span><span class="sxs-lookup"><span data-stu-id="79f6a-118">The Application Map uses the `cloud_RoleName` property to identify the components on the map.</span></span>

<span data-ttu-id="79f6a-119">Mer information om hur Åsidosätt den `cloud_RoleName` egenskapen Se [lägga till egenskaper: ITelemetryInitializer](app-insights-api-filtering-sampling.md#add-properties-itelemetryinitializer).</span><span class="sxs-lookup"><span data-stu-id="79f6a-119">For more information about how do override the `cloud_RoleName` property see [Add properties: ITelemetryInitializer](app-insights-api-filtering-sampling.md#add-properties-itelemetryinitializer).</span></span>  

<span data-ttu-id="79f6a-120">I vissa fall kan detta kanske inte är rätt och du kanske föredrar att använda separata resurser för olika grupper av komponenter.</span><span class="sxs-lookup"><span data-stu-id="79f6a-120">In some cases, this may not be appropriate, and you may prefer to use separate resources for different groups of components.</span></span> <span data-ttu-id="79f6a-121">Du kan behöva använda olika resurser för hantering eller fakturering.</span><span class="sxs-lookup"><span data-stu-id="79f6a-121">For example, you might need to use different resources for management or billing purposes.</span></span> <span data-ttu-id="79f6a-122">Med hjälp av separata resurser innebär att du inte ser alla komponenter som visas på en enda programavbildningen; och att du kan inte fråga på komponenter i [Analytics](app-insights-analytics.md).</span><span class="sxs-lookup"><span data-stu-id="79f6a-122">Using separate resources means that you don't see all the components displayed on a single Application Map; and that you can't query across components in [Analytics](app-insights-analytics.md).</span></span> <span data-ttu-id="79f6a-123">Du måste ange olika resurser.</span><span class="sxs-lookup"><span data-stu-id="79f6a-123">You also have to set up the separate resources.</span></span>

<span data-ttu-id="79f6a-124">Med den begränsning antar vi i resten av det här dokumentet som du vill skicka data från flera komponenter till en Application Insights-resurs.</span><span class="sxs-lookup"><span data-stu-id="79f6a-124">With that caveat, we'll assume in the rest of this document that you want to send data from multiple components to one Application Insights resource.</span></span>

## <a name="configure-multi-component-applications"></a><span data-ttu-id="79f6a-125">Konfigurera flera komponenten program</span><span class="sxs-lookup"><span data-stu-id="79f6a-125">Configure multi-component applications</span></span>

<span data-ttu-id="79f6a-126">För att få flera komponenten programavbildningen måste att uppnå dessa mål:</span><span class="sxs-lookup"><span data-stu-id="79f6a-126">To get a multi-component application map, you need to achieve these goals:</span></span>

* <span data-ttu-id="79f6a-127">**Installera den senaste förhandsversionen** Application Insights-paketet i varje komponent i programmet.</span><span class="sxs-lookup"><span data-stu-id="79f6a-127">**Install the latest pre-release** Application Insights package in each component of the application.</span></span> 
* <span data-ttu-id="79f6a-128">**Dela en enda Application Insights-resurs** för alla komponenter i ditt program.</span><span class="sxs-lookup"><span data-stu-id="79f6a-128">**Share a single Application Insights resource** for all the components of your application.</span></span>
* <span data-ttu-id="79f6a-129">**Aktivera flera rollen programavbildningen** i bladet förhandsgranskningar.</span><span class="sxs-lookup"><span data-stu-id="79f6a-129">**Enable Multi-role Application Map** in the Previews blade.</span></span>

<span data-ttu-id="79f6a-130">Konfigurera varje komponent i ditt program med en lämplig metod för typen.</span><span class="sxs-lookup"><span data-stu-id="79f6a-130">Configure each component of your application using the appropriate method for its type.</span></span> <span data-ttu-id="79f6a-131">([ASP.NET](app-insights-asp-net.md), [Java](app-insights-java-get-started.md), [Node.js](app-insights-nodejs.md), [JavaScript](app-insights-javascript.md).)</span><span class="sxs-lookup"><span data-stu-id="79f6a-131">([ASP.NET](app-insights-asp-net.md), [Java](app-insights-java-get-started.md), [Node.js](app-insights-nodejs.md), [JavaScript](app-insights-javascript.md).)</span></span>

### <a name="1-install-the-latest-pre-release-package"></a><span data-ttu-id="79f6a-132">1. Installera det senaste förhandsversionen paketet</span><span class="sxs-lookup"><span data-stu-id="79f6a-132">1. Install the latest pre-release package</span></span>

<span data-ttu-id="79f6a-133">Uppdatera eller installera program insikter paket i projektet för varje serverkomponent.</span><span class="sxs-lookup"><span data-stu-id="79f6a-133">Update or install the Appication Insights packages in the project for each server component.</span></span> <span data-ttu-id="79f6a-134">Om du använder Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="79f6a-134">If you're using Visual Studio:</span></span>

1. <span data-ttu-id="79f6a-135">Högerklicka på projektet och välj **hantera NuGet-paket**.</span><span class="sxs-lookup"><span data-stu-id="79f6a-135">Right-click a project and select **Manage NuGet Packages**.</span></span> 
2. <span data-ttu-id="79f6a-136">Välj **inkludera förhandsversion**.</span><span class="sxs-lookup"><span data-stu-id="79f6a-136">Select **Include prerelease**.</span></span>
3. <span data-ttu-id="79f6a-137">Om Application Insights visas paketen i uppdateringar, markera dem.</span><span class="sxs-lookup"><span data-stu-id="79f6a-137">If Application Insights packages appear in Updates, select them.</span></span> 

    <span data-ttu-id="79f6a-138">Annars kan söka efter och installera lämplig paketet:</span><span class="sxs-lookup"><span data-stu-id="79f6a-138">Otherwise, browse for and install the appropriate package:</span></span>
    
    * <span data-ttu-id="79f6a-139">Microsoft.ApplicationInsights.WindowsServer</span><span class="sxs-lookup"><span data-stu-id="79f6a-139">Microsoft.ApplicationInsights.WindowsServer</span></span>
    * <span data-ttu-id="79f6a-140">Microsoft.ApplicationInsights.ServiceFabric - komponenter som körs som gäst körbara filer och Docker-behållare som kör ett i Service Fabric-program</span><span class="sxs-lookup"><span data-stu-id="79f6a-140">Microsoft.ApplicationInsights.ServiceFabric - for components running as guest executables and Docker containers running a in Service Fabric application</span></span>
    * <span data-ttu-id="79f6a-141">Microsoft.ApplicationInsights.ServiceFabric.Native - för tillförlitlig tjänster i ServiceFabric program</span><span class="sxs-lookup"><span data-stu-id="79f6a-141">Microsoft.ApplicationInsights.ServiceFabric.Native - for reliable services in ServiceFabric applications</span></span>
    * <span data-ttu-id="79f6a-142">Microsoft.ApplicationInsights.Kubernetes för komponenter som körs i Docker på Kubernetes</span><span class="sxs-lookup"><span data-stu-id="79f6a-142">Microsoft.ApplicationInsights.Kubernetes for components running in Docker on Kubernetes</span></span>

### <a name="2-share-a-single-application-insights-resource"></a><span data-ttu-id="79f6a-143">2. Dela en enda Application Insights-resurs</span><span class="sxs-lookup"><span data-stu-id="79f6a-143">2. Share a single Application Insights resource</span></span>

* <span data-ttu-id="79f6a-144">Högerklicka på ett projekt i Visual Studio och välj **konfigurera Application Insights**, eller **Programinsikter > Konfigurera**.</span><span class="sxs-lookup"><span data-stu-id="79f6a-144">In Visual Studio, right-click a project and select **Configure Application Insights**, or **Application Insights > Configure**.</span></span> <span data-ttu-id="79f6a-145">Använd guiden Skapa Application Insights-resurs för det första projektet.</span><span class="sxs-lookup"><span data-stu-id="79f6a-145">For the first project, use the wizard to create an Application Insights resource.</span></span> <span data-ttu-id="79f6a-146">Välj samma resurs för efterföljande projekt.</span><span class="sxs-lookup"><span data-stu-id="79f6a-146">For subsequent projects, select the same resource.</span></span>
* <span data-ttu-id="79f6a-147">Om det finns inga Application Insights-menyn, konfigurera manuellt:</span><span class="sxs-lookup"><span data-stu-id="79f6a-147">If there is no Application Insights menu, configure manually:</span></span>

   1. <span data-ttu-id="79f6a-148">I [Azure-portalen](https://portal,azure.com), öppna Application Insights-resurs som du redan har skapats för en annan komponent.</span><span class="sxs-lookup"><span data-stu-id="79f6a-148">In [Azure portal](https://portal,azure.com), open the Application Insights resource you already created for another component.</span></span>
   2. <span data-ttu-id="79f6a-149">I bladet översikt, öppna Essentials nedrullningsbara fliken och kopiera den **Instrumentation nyckel.**</span><span class="sxs-lookup"><span data-stu-id="79f6a-149">In the Overview blade, open the Essentials drop-down tab, and copy the **Instrumentation Key.**</span></span>
   3. <span data-ttu-id="79f6a-150">Öppna ApplicationInsights.config och infoga i projektet:`<InstrumentationKey>your copied key</InstrumentationKey>`</span><span class="sxs-lookup"><span data-stu-id="79f6a-150">In your project, open ApplicationInsights.config and insert: `<InstrumentationKey>your copied key</InstrumentationKey>`</span></span>

![Kopiera nyckeln instrumentation till .config-filen](./media/app-insights-monitor-multi-role-apps/copy-instrumentation-key.png)


### <a name="3-enable-multi-role-application-map"></a><span data-ttu-id="79f6a-152">3. Aktivera flera rollen programavbildningen</span><span class="sxs-lookup"><span data-stu-id="79f6a-152">3. Enable multi-role Application Map</span></span>

<span data-ttu-id="79f6a-153">Öppna resursen för ditt program i Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="79f6a-153">In the Azure portal, open the resource for your application.</span></span> <span data-ttu-id="79f6a-154">I bladet förhandsgranskningar aktivera *flera rollen programavbildningen*.</span><span class="sxs-lookup"><span data-stu-id="79f6a-154">In the Previews blade, enable *Multi-role Application Map*.</span></span>

### <a name="4-enable-docker-metrics-optional"></a><span data-ttu-id="79f6a-155">4. Aktivera Docker mätvärden (valfritt)</span><span class="sxs-lookup"><span data-stu-id="79f6a-155">4. Enable Docker metrics (Optional)</span></span> 

<span data-ttu-id="79f6a-156">Om en komponent som körs i en Docker som finns på en Windows Azure-dator, kan du samla in ytterligare mått från behållaren.</span><span class="sxs-lookup"><span data-stu-id="79f6a-156">If a component runs in a Docker hosted on an Azure Windows VM, you can collect additional metrics from the container.</span></span> <span data-ttu-id="79f6a-157">Infoga det i ditt [Azure diagnostics](../monitoring-and-diagnostics/azure-diagnostics.md) konfigurationsfil:</span><span class="sxs-lookup"><span data-stu-id="79f6a-157">Insert this in your [Azure diagnostics](../monitoring-and-diagnostics/azure-diagnostics.md) configuration file:</span></span>

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

## <a name="use-cloudrolename-to-separate-components"></a><span data-ttu-id="79f6a-158">Använd cloud_RoleName för att avgränsa komponenter</span><span class="sxs-lookup"><span data-stu-id="79f6a-158">Use cloud_RoleName to separate components</span></span>

<span data-ttu-id="79f6a-159">Den `cloud_RoleName` bifogas till all telemetri.</span><span class="sxs-lookup"><span data-stu-id="79f6a-159">The `cloud_RoleName` property is attached to all telemetry.</span></span> <span data-ttu-id="79f6a-160">Komponenten - roll eller tjänst - kommer telemetrin identifieras.</span><span class="sxs-lookup"><span data-stu-id="79f6a-160">It identifies the component - the role or service - that originates the telemetry.</span></span> <span data-ttu-id="79f6a-161">(Det är inte samma som cloud_RoleInstance som separerar identiska roller som körs parallellt på flera serverprocesser eller datorer.)</span><span class="sxs-lookup"><span data-stu-id="79f6a-161">(It is not the same as cloud_RoleInstance, which separates identical roles that are running in parallel on multiple server processes or machines.)</span></span>

<span data-ttu-id="79f6a-162">Du kan filtrera eller segmentera dina telemetri som använder den här egenskapen i portalen.</span><span class="sxs-lookup"><span data-stu-id="79f6a-162">In the portal, you can filter or segment your telemetry using this property.</span></span> <span data-ttu-id="79f6a-163">Fel-bladet är filtrerad och visar bara information från tjänsten frontwebbservern filtrera fel från CRM API-serverdel i det här exemplet:</span><span class="sxs-lookup"><span data-stu-id="79f6a-163">In this example, the Failures blade is filtered to show just information from the front-end web service, filtering out failures from the CRM API backend:</span></span>

![Mått diagram åtskilda med rollen Molnnamn](./media/app-insights-monitor-multi-role-apps/cloud-role-name.png)

## <a name="trace-operations-between-components"></a><span data-ttu-id="79f6a-165">Spåra åtgärder mellan komponenter</span><span class="sxs-lookup"><span data-stu-id="79f6a-165">Trace operations between components</span></span>

<span data-ttu-id="79f6a-166">Du kan spåra från en komponent till en annan, anrop under bearbetning av en enskild åtgärd.</span><span class="sxs-lookup"><span data-stu-id="79f6a-166">You can trace from one component to another, the calls made while processing an individual operation.</span></span>


![Visa telemetri för åtgärden](./media/app-insights-monitor-multi-role-apps/show-telemetry-for-operation.png)

<span data-ttu-id="79f6a-168">Klicka här för att en korrelerade lista över telemetri för den här åtgärden på frontwebbservern och backend-API:</span><span class="sxs-lookup"><span data-stu-id="79f6a-168">Click through to a correlated list of telemetry for this operation across the front-end web server and the back-end API:</span></span>

![Söka i komponenter](./media/app-insights-monitor-multi-role-apps/search-across-components.png)


## <a name="next-steps"></a><span data-ttu-id="79f6a-170">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="79f6a-170">Next steps</span></span>

* [<span data-ttu-id="79f6a-171">Separata telemetri från utveckling, Test och produktion</span><span class="sxs-lookup"><span data-stu-id="79f6a-171">Separate telemetry from Development, Test, and Production</span></span>](app-insights-separate-resources.md)
