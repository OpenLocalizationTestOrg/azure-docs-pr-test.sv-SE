---
title: "aaaSeparating telemetri från utveckling, testa och släpp i Azure Application Insights | Microsoft Docs"
description: "Direkt telemetri toodifferent resurser för utveckling, test- och stämplar."
services: application-insights
documentationcenter: 
author: CFreemanwa
manager: carmonm
ms.assetid: 578e30f0-31ed-4f39-baa8-01b4c2f310c9
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 05/15/2017
ms.author: bwren
ms.openlocfilehash: a294c8c70f46d7c29b460461c3494c83e13a0cbe
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="separating-telemetry-from-development-test-and-production"></a><span data-ttu-id="95101-103">Avgränsa telemetri från utveckling, Test och produktion</span><span class="sxs-lookup"><span data-stu-id="95101-103">Separating telemetry from Development, Test, and Production</span></span>

<span data-ttu-id="95101-104">När du utvecklar hello nästa version av ett program måste du inte vill toomix in hello [Programinsikter](app-insights-overview.md) telemetri från hello nya och hello redan släppts-versionen.</span><span class="sxs-lookup"><span data-stu-id="95101-104">When you are developing hello next version of a web application, you don't want toomix up hello [Application Insights](app-insights-overview.md) telemetry from hello new version and hello already released version.</span></span> <span data-ttu-id="95101-105">tooavoid förvirring skicka hello telemetri från olika development skapar tooseparate Application Insights-resurser med separat instrumentation nycklar (ikeys).</span><span class="sxs-lookup"><span data-stu-id="95101-105">tooavoid confusion, send hello telemetry from different development stages tooseparate Application Insights resources, with separate instrumentation keys (ikeys).</span></span> <span data-ttu-id="95101-106">toomake den enklare toochange hello instrumentation nyckeln som en version som flyttas från en fas tooanother det kan vara användbart tooset hello ikey i koden i stället för i hello konfigurationsfilen.</span><span class="sxs-lookup"><span data-stu-id="95101-106">toomake it easier toochange hello instrumentation key as a version moves from one stage tooanother, it can be useful tooset hello ikey in code instead of in hello configuration file.</span></span> 

<span data-ttu-id="95101-107">(Om datorn är en Azure-molntjänst är [en annan metod för att separat ikeys](app-insights-cloudservices.md).)</span><span class="sxs-lookup"><span data-stu-id="95101-107">(If your system is an Azure Cloud Service, there's [another method of setting separate ikeys](app-insights-cloudservices.md).)</span></span>

## <a name="about-resources-and-instrumentation-keys"></a><span data-ttu-id="95101-108">Om resurser och instrumentation nycklar</span><span class="sxs-lookup"><span data-stu-id="95101-108">About resources and instrumentation keys</span></span>

<span data-ttu-id="95101-109">När du ställer in Application Insights-övervakning för webbappen kan du skapa en Application Insights *resurs* i Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="95101-109">When you set up Application Insights monitoring for your web app, you create an Application Insights *resource* in Microsoft Azure.</span></span> <span data-ttu-id="95101-110">Du öppnar den här resursen i hello Azure-portalen i ordning toosee och analysera hello telemetri som samlats in från din app.</span><span class="sxs-lookup"><span data-stu-id="95101-110">You open this resource in hello Azure portal in order toosee and analyze hello telemetry collected from your app.</span></span> <span data-ttu-id="95101-111">hello resurs har identifierats av en *instrumentation nyckeln* (ikey).</span><span class="sxs-lookup"><span data-stu-id="95101-111">hello resource is identified by an *instrumentation key* (ikey).</span></span> <span data-ttu-id="95101-112">När du installerar hello Application Insights paketet toomonitor din app kan du konfigurera den med hello instrumentation nyckel så att den vet där toosend hello telemetri.</span><span class="sxs-lookup"><span data-stu-id="95101-112">When you install hello Application Insights package toomonitor your app, you configure it with hello instrumentation key, so that it knows where toosend hello telemetry.</span></span>

<span data-ttu-id="95101-113">Du vanligtvis välja toouse separata resurser eller en enda delad resurs i olika scenarier:</span><span class="sxs-lookup"><span data-stu-id="95101-113">You typically choose toouse separate resources or a single shared resource in different scenarios:</span></span>

* <span data-ttu-id="95101-114">Olika, oberoende program - använda en separat resurs och ikey för varje app.</span><span class="sxs-lookup"><span data-stu-id="95101-114">Different, independent applications - Use a separate resource and ikey for each app.</span></span>
* <span data-ttu-id="95101-115">Flera komponenter eller roller från en affärsprogram - använda en [enkel delad resurs](app-insights-monitor-multi-role-apps.md) för alla hello komponenten appar.</span><span class="sxs-lookup"><span data-stu-id="95101-115">Multiple components or roles of one business application - Use a [single shared resource](app-insights-monitor-multi-role-apps.md) for all hello component apps.</span></span> <span data-ttu-id="95101-116">Telemetri kan filtreras eller segmenterade av hello cloud_RoleName-egenskapen.</span><span class="sxs-lookup"><span data-stu-id="95101-116">Telemetry can be filtered or segmented by hello cloud_RoleName property.</span></span>
* <span data-ttu-id="95101-117">Utveckling och Test Release - använda en separat resurs och ikey för versioner av hello i 'i stämpel' eller steget i produktion.</span><span class="sxs-lookup"><span data-stu-id="95101-117">Development, Test, and Release - Use a separate resource and ikey for versions of hello system in 'stamp' or stage of production.</span></span>
* <span data-ttu-id="95101-118">EN | B-testning - använda en enskild resurs.</span><span class="sxs-lookup"><span data-stu-id="95101-118">A | B testing - Use a single resource.</span></span> <span data-ttu-id="95101-119">Skapa en TelemetryInitializer tooadd en egenskapen toohello telemetri som identifierar hello varianter.</span><span class="sxs-lookup"><span data-stu-id="95101-119">Create a TelemetryInitializer tooadd a property toohello telemetry that identifies hello variants.</span></span>


## <span data-ttu-id="95101-120"><a name="dynamic-ikey"></a>Dynamisk instrumentation nyckel</span><span class="sxs-lookup"><span data-stu-id="95101-120"><a name="dynamic-ikey"></a> Dynamic instrumentation key</span></span>

<span data-ttu-id="95101-121">toomake underlättar toochange hello ikey som hello kod flyttar mellan stegen i produktion, ange den i koden i stället för i hello konfigurationsfil.</span><span class="sxs-lookup"><span data-stu-id="95101-121">toomake it easier toochange hello ikey as hello code moves between stages of production, set it in code instead of in hello configuration file.</span></span>

<span data-ttu-id="95101-122">Ange hello nyckeln i en initieringsmetod, till exempel global.aspx.cs i en ASP.NET-tjänst:</span><span class="sxs-lookup"><span data-stu-id="95101-122">Set hello key in an initialization method, such as global.aspx.cs in an ASP.NET service:</span></span>

<span data-ttu-id="95101-123">*C#*</span><span class="sxs-lookup"><span data-stu-id="95101-123">*C#*</span></span>

    protected void Application_Start()
    {
      Microsoft.ApplicationInsights.Extensibility.
        TelemetryConfiguration.Active.InstrumentationKey = 
          // - for example -
          WebConfigurationManager.AppSettings["ikey"];
      ...

<span data-ttu-id="95101-124">I det här exemplet placeras hello ikeys för hello olika resurser i olika versioner av hello Webbkonfigurationsfilen.</span><span class="sxs-lookup"><span data-stu-id="95101-124">In this example, hello ikeys for hello different resources are placed in different versions of hello web configuration file.</span></span> <span data-ttu-id="95101-125">Växlar hello Webbkonfigurationsfilen - som du kan göra i hello versionen skript - ska växla hello målresursen.</span><span class="sxs-lookup"><span data-stu-id="95101-125">Swapping hello web configuration file - which you can do as part of hello release script - will swap hello target resource.</span></span>

### <a name="web-pages"></a><span data-ttu-id="95101-126">Webbsidor</span><span class="sxs-lookup"><span data-stu-id="95101-126">Web pages</span></span>
<span data-ttu-id="95101-127">Hej iKey används också i din app webbsidor i hello [skript som du har fått från hello Snabbstart-bladet](app-insights-javascript.md).</span><span class="sxs-lookup"><span data-stu-id="95101-127">hello iKey is also used in your app's web pages, in hello [script that you got from hello quick start blade](app-insights-javascript.md).</span></span> <span data-ttu-id="95101-128">I stället för att koda den bokstavligt hello skript, generera den från hello Servertillstånd.</span><span class="sxs-lookup"><span data-stu-id="95101-128">Instead of coding it literally into hello script, generate it from hello server state.</span></span> <span data-ttu-id="95101-129">Till exempel i en ASP.NET-app:</span><span class="sxs-lookup"><span data-stu-id="95101-129">For example, in an ASP.NET app:</span></span>

<span data-ttu-id="95101-130">*JavaScript i Razor*</span><span class="sxs-lookup"><span data-stu-id="95101-130">*JavaScript in Razor*</span></span>

    <script type="text/javascript">
    // Standard Application Insights web page script:
    var appInsights = window.appInsights || function(config){ ...
    // Modify this part:
    }({instrumentationKey:  
      // Generate from server property:
      "@Microsoft.ApplicationInsights.Extensibility.
         TelemetryConfiguration.Active.InstrumentationKey"
    }) // ...


## <a name="create-additional-application-insights-resources"></a><span data-ttu-id="95101-131">Skapa ytterligare Application Insights-resurser</span><span class="sxs-lookup"><span data-stu-id="95101-131">Create additional Application Insights resources</span></span>
<span data-ttu-id="95101-132">tooseparate telemetri för olika programkomponenter eller annan stämplar (dev/test/produktion) av hello samma komponent och du har toocreate en ny Application Insights-resurs.</span><span class="sxs-lookup"><span data-stu-id="95101-132">tooseparate telemetry for different application components, or for different stamps (dev/test/production) of hello same component, then you'll have toocreate a new Application Insights resource.</span></span>

<span data-ttu-id="95101-133">I hello [portal.azure.com](https://portal.azure.com), Lägg till Application Insights-resurs:</span><span class="sxs-lookup"><span data-stu-id="95101-133">In hello [portal.azure.com](https://portal.azure.com), add an Application Insights resource:</span></span>

![Klicka på Nytt, Application Insights](./media/app-insights-separate-resources/01-new.png)

* <span data-ttu-id="95101-135">**Programtyp** påverkar vad som visas på hello översikt bladet och hello-egenskaper som är tillgängliga i [mått explorer](app-insights-metrics-explorer.md).</span><span class="sxs-lookup"><span data-stu-id="95101-135">**Application type** affects what you see on hello overview blade and hello properties available in [metric explorer](app-insights-metrics-explorer.md).</span></span> <span data-ttu-id="95101-136">Välj ett av hello web typer för webbsidor om du inte ser typen av app.</span><span class="sxs-lookup"><span data-stu-id="95101-136">If you don't see your type of app, choose one of hello web types for web pages.</span></span>
* <span data-ttu-id="95101-137">**Resursgruppen** är i syfte att underlätta för att hantera egenskaper som [åtkomstkontroll](app-insights-resources-roles-access-control.md).</span><span class="sxs-lookup"><span data-stu-id="95101-137">**Resource group** is a convenience for managing properties like [access control](app-insights-resources-roles-access-control.md).</span></span> <span data-ttu-id="95101-138">Du kan använda separata resursgrupper för utveckling, testning och produktion.</span><span class="sxs-lookup"><span data-stu-id="95101-138">You could use separate resource groups for development, test, and production.</span></span>
* <span data-ttu-id="95101-139">**Prenumerationen** är din betalningskonto i Azure.</span><span class="sxs-lookup"><span data-stu-id="95101-139">**Subscription** is your payment account in Azure.</span></span>
* <span data-ttu-id="95101-140">**Plats** är där vi behåller dina data.</span><span class="sxs-lookup"><span data-stu-id="95101-140">**Location** is where we keep your data.</span></span> <span data-ttu-id="95101-141">För närvarande kan den inte ändras.</span><span class="sxs-lookup"><span data-stu-id="95101-141">Currently it can't be changed.</span></span> 
* <span data-ttu-id="95101-142">**Lägg till toodashboard** placerar en snabb åtkomst-panelen för din resurs på Azure-startsidan.</span><span class="sxs-lookup"><span data-stu-id="95101-142">**Add toodashboard** puts a quick-access tile for your resource on your Azure Home page.</span></span> 

<span data-ttu-id="95101-143">Att skapa hello resurs tar några sekunder.</span><span class="sxs-lookup"><span data-stu-id="95101-143">Creating hello resource takes a few seconds.</span></span> <span data-ttu-id="95101-144">En varning visas när du är klar.</span><span class="sxs-lookup"><span data-stu-id="95101-144">You'll see an alert when it's done.</span></span>

<span data-ttu-id="95101-145">(Du kan skriva en [PowerShell-skript](app-insights-powershell-script-create-resource.md) toocreate en resurs automatiskt.)</span><span class="sxs-lookup"><span data-stu-id="95101-145">(You can write a [PowerShell script](app-insights-powershell-script-create-resource.md) toocreate a resource automatically.)</span></span>

### <a name="getting-hello-instrumentation-key"></a><span data-ttu-id="95101-146">Hämta hello instrumentation nyckel</span><span class="sxs-lookup"><span data-stu-id="95101-146">Getting hello instrumentation key</span></span>
<span data-ttu-id="95101-147">hello instrumentation nyckel identifierar hello-resurs som du skapade.</span><span class="sxs-lookup"><span data-stu-id="95101-147">hello instrumentation key identifies hello resource that you created.</span></span> 

![Klicka på Essentials, hello Instrumentation nyckeln CTRL + C](./media/app-insights-separate-resources/02-props.png)

<span data-ttu-id="95101-149">Du behöver hello instrumentation nycklar för alla hello resurser toowhich appen skickar data.</span><span class="sxs-lookup"><span data-stu-id="95101-149">You need hello instrumentation keys of all hello resources toowhich your app will send data.</span></span>

## <a name="filter-on-build-number"></a><span data-ttu-id="95101-150">Filtrera efter build-nummer</span><span class="sxs-lookup"><span data-stu-id="95101-150">Filter on build number</span></span>
<span data-ttu-id="95101-151">När du publicerar en ny version av din app, ska du toobe kan tooseparate hello telemetri från olika versioner.</span><span class="sxs-lookup"><span data-stu-id="95101-151">When you publish a new version of your app, you'll want toobe able tooseparate hello telemetry from different builds.</span></span>

<span data-ttu-id="95101-152">Du kan ange hello programversion egenskapen så att du kan filtrera [Sök](app-insights-diagnostic-search.md) och [mått explorer](app-insights-metrics-explorer.md) resultat.</span><span class="sxs-lookup"><span data-stu-id="95101-152">You can set hello Application Version property so that you can filter [search](app-insights-diagnostic-search.md) and [metric explorer](app-insights-metrics-explorer.md) results.</span></span>

![Filtrering på en egenskap](./media/app-insights-separate-resources/050-filter.png)

<span data-ttu-id="95101-154">Det finns flera olika metoder för att egenskapen hello-programversionen.</span><span class="sxs-lookup"><span data-stu-id="95101-154">There are several different methods of setting hello Application Version property.</span></span>

* <span data-ttu-id="95101-155">Ange direkt:</span><span class="sxs-lookup"><span data-stu-id="95101-155">Set directly:</span></span>

    `telemetryClient.Context.Component.Version = typeof(MyProject.MyClass).Assembly.GetName().Version;`
* <span data-ttu-id="95101-156">Omsluta den raden i en [telemetri initieraren](app-insights-api-custom-events-metrics.md#defaults) tooensure som alla TelemetryClient instanser är konsekvent.</span><span class="sxs-lookup"><span data-stu-id="95101-156">Wrap that line in a [telemetry initializer](app-insights-api-custom-events-metrics.md#defaults) tooensure that all TelemetryClient instances are set consistently.</span></span>
* <span data-ttu-id="95101-157">[ASP.NET] Ange hello version i `BuildInfo.config`.</span><span class="sxs-lookup"><span data-stu-id="95101-157">[ASP.NET] Set hello version in `BuildInfo.config`.</span></span> <span data-ttu-id="95101-158">hello webbmodul ska hämta hello-version från hello BuildLabel nod.</span><span class="sxs-lookup"><span data-stu-id="95101-158">hello web module will pick up hello version from hello BuildLabel node.</span></span> <span data-ttu-id="95101-159">Inkludera den här filen i projektet och Kom ihåg tooset hello alltid kopiera egenskapen i Solution Explorer.</span><span class="sxs-lookup"><span data-stu-id="95101-159">Include this file in your project and remember tooset hello Copy Always property in Solution Explorer.</span></span>

    ```XML

    <?xml version="1.0" encoding="utf-8"?>
    <DeploymentEvent xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns="http://schemas.microsoft.com/VisualStudio/DeploymentEvent/2013/06">
      <ProjectName>AppVersionExpt</ProjectName>
      <Build type="MSBuild">
        <MSBuild>
          <BuildLabel kind="label">1.0.0.2</BuildLabel>
        </MSBuild>
      </Build>
    </DeploymentEvent>

    ```
* <span data-ttu-id="95101-160">[ASP.NET] Generera BuildInfo.config automatiskt i MSBuild.</span><span class="sxs-lookup"><span data-stu-id="95101-160">[ASP.NET] Generate BuildInfo.config automatically in MSBuild.</span></span> <span data-ttu-id="95101-161">toodo, lägga till några rader tooyour `.csproj` fil:</span><span class="sxs-lookup"><span data-stu-id="95101-161">toodo this, add a few lines tooyour `.csproj` file:</span></span>

    ```XML

    <PropertyGroup>
      <GenerateBuildInfoConfigFile>true</GenerateBuildInfoConfigFile>    <IncludeServerNameInBuildInfo>true</IncludeServerNameInBuildInfo>
    </PropertyGroup>
    ```

    <span data-ttu-id="95101-162">Detta genererar en fil med namnet *yourProjectName*. BuildInfo.config. hello publiceringsprocessen byter namn på den tooBuildInfo.config.</span><span class="sxs-lookup"><span data-stu-id="95101-162">This generates a file called *yourProjectName*.BuildInfo.config. hello Publish process renames it tooBuildInfo.config.</span></span>

    <span data-ttu-id="95101-163">hello build etiketten innehåller platshållare (AutoGen_...) när du skapar med Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="95101-163">hello build label contains a placeholder (AutoGen_...) when you build with Visual Studio.</span></span> <span data-ttu-id="95101-164">Men när byggts med MSBuild det fylls i med hello rätt versionsnumret.</span><span class="sxs-lookup"><span data-stu-id="95101-164">But when built with MSBuild, it is populated with hello correct version number.</span></span>

    <span data-ttu-id="95101-165">tooallow MSBuild toogenerate versionsnummer, ange hello version som `1.0.*` i AssemblyReference.cs</span><span class="sxs-lookup"><span data-stu-id="95101-165">tooallow MSBuild toogenerate version numbers, set hello version like `1.0.*` in AssemblyReference.cs</span></span>

## <a name="version-and-release-tracking"></a><span data-ttu-id="95101-166">Spårning av versionen och utgåva</span><span class="sxs-lookup"><span data-stu-id="95101-166">Version and release tracking</span></span>
<span data-ttu-id="95101-167">tootrack hello programversion, se till att `buildinfo.config` har genererats av Microsoft skapa Engine-processen.</span><span class="sxs-lookup"><span data-stu-id="95101-167">tootrack hello application version, make sure `buildinfo.config` is generated by your Microsoft Build Engine process.</span></span> <span data-ttu-id="95101-168">I filen .csproj lägger du till:</span><span class="sxs-lookup"><span data-stu-id="95101-168">In your .csproj file, add:</span></span>  

```XML

    <PropertyGroup>
      <GenerateBuildInfoConfigFile>true</GenerateBuildInfoConfigFile>    <IncludeServerNameInBuildInfo>true</IncludeServerNameInBuildInfo>
    </PropertyGroup>
```

<span data-ttu-id="95101-169">När den har hello build info hello Application Insights webbmodulen lägger automatiskt till **programversion** som ett objekt med egenskapen tooevery av telemetri.</span><span class="sxs-lookup"><span data-stu-id="95101-169">When it has hello build info, hello Application Insights web module automatically adds **Application version** as a property tooevery item of telemetry.</span></span> <span data-ttu-id="95101-170">Gör att du toofilter av version när du utför [diagnostiska sökningar](app-insights-diagnostic-search.md), eller när du [utforska mått](app-insights-metrics-explorer.md).</span><span class="sxs-lookup"><span data-stu-id="95101-170">That allows you toofilter by version when you perform [diagnostic searches](app-insights-diagnostic-search.md), or when you [explore metrics](app-insights-metrics-explorer.md).</span></span>

<span data-ttu-id="95101-171">Observera dock att hello build-versionsnummer genereras bara av hello Microsoft skapa Engine, inte av hello utvecklare skapar i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="95101-171">However, notice that hello build version number is generated only by hello Microsoft Build Engine, not by hello developer build in Visual Studio.</span></span>

### <a name="release-annotations"></a><span data-ttu-id="95101-172">Versionsanteckningar</span><span class="sxs-lookup"><span data-stu-id="95101-172">Release annotations</span></span>
<span data-ttu-id="95101-173">Om du använder Visual Studio Team Services, kan du [få en anteckning markör](app-insights-annotations.md) till tooyour diagram när du släpper en ny version.</span><span class="sxs-lookup"><span data-stu-id="95101-173">If you use Visual Studio Team Services, you can [get an annotation marker](app-insights-annotations.md) added tooyour charts whenever you release a new version.</span></span> <span data-ttu-id="95101-174">hello följande bild visar hur den här markören visas.</span><span class="sxs-lookup"><span data-stu-id="95101-174">hello following image shows how this marker appears.</span></span>

![Skärmbild av exempel på versionsanteckning i ett diagram](./media/app-insights-asp-net/release-annotation.png)
## <a name="next-steps"></a><span data-ttu-id="95101-176">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="95101-176">Next steps</span></span>

* [<span data-ttu-id="95101-177">Delade resurser för flera roller</span><span class="sxs-lookup"><span data-stu-id="95101-177">Shared resources for multiple roles</span></span>](app-insights-monitor-multi-role-apps.md)
* [<span data-ttu-id="95101-178">Skapa en telemetri initieraren toodistinguish A | B varianter</span><span class="sxs-lookup"><span data-stu-id="95101-178">Create a Telemetry Initializer toodistinguish A|B variants</span></span>](app-insights-api-filtering-sampling.md#add-properties)
