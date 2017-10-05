---
title: "Avgränsa telemetri från utveckling, testa och släpp i Azure Application Insights | Microsoft Docs"
description: "Direkt telemetri till olika resurser för utveckling, test- och stämplar."
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
ms.openlocfilehash: f51fa4639aaa60686cc349683713c6e5f9732bb9
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="separating-telemetry-from-development-test-and-production"></a><span data-ttu-id="32c33-103">Avgränsa telemetri från utveckling, Test och produktion</span><span class="sxs-lookup"><span data-stu-id="32c33-103">Separating telemetry from Development, Test, and Production</span></span>

<span data-ttu-id="32c33-104">När du utvecklar nästa version av ett program måste du inte vill att blanda ihop den [Programinsikter](app-insights-overview.md) telemetri från den nya versionen och den redan utgivna versionen.</span><span class="sxs-lookup"><span data-stu-id="32c33-104">When you are developing the next version of a web application, you don't want to mix up the [Application Insights](app-insights-overview.md) telemetry from the new version and the already released version.</span></span> <span data-ttu-id="32c33-105">Skicka telemetrin från olika development steg för att avgränsa Application Insights-resurser med separat instrumentation nycklar (ikeys) för att undvika förvirring.</span><span class="sxs-lookup"><span data-stu-id="32c33-105">To avoid confusion, send the telemetry from different development stages to separate Application Insights resources, with separate instrumentation keys (ikeys).</span></span> <span data-ttu-id="32c33-106">Om du vill göra det lättare att ändra instrumentation nyckeln som en version som flyttas från ett steg till en annan, kan det vara användbart att ange ikey i koden i stället för i konfigurationsfilen.</span><span class="sxs-lookup"><span data-stu-id="32c33-106">To make it easier to change the instrumentation key as a version moves from one stage to another, it can be useful to set the ikey in code instead of in the configuration file.</span></span> 

<span data-ttu-id="32c33-107">(Om datorn är en Azure-molntjänst är [en annan metod för att separat ikeys](app-insights-cloudservices.md).)</span><span class="sxs-lookup"><span data-stu-id="32c33-107">(If your system is an Azure Cloud Service, there's [another method of setting separate ikeys](app-insights-cloudservices.md).)</span></span>

## <a name="about-resources-and-instrumentation-keys"></a><span data-ttu-id="32c33-108">Om resurser och instrumentation nycklar</span><span class="sxs-lookup"><span data-stu-id="32c33-108">About resources and instrumentation keys</span></span>

<span data-ttu-id="32c33-109">När du ställer in Application Insights-övervakning för webbappen kan du skapa en Application Insights *resurs* i Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="32c33-109">When you set up Application Insights monitoring for your web app, you create an Application Insights *resource* in Microsoft Azure.</span></span> <span data-ttu-id="32c33-110">Du öppnar den här resursen i Azure-portalen för att kunna se och analysera telemetri som samlats in från din app.</span><span class="sxs-lookup"><span data-stu-id="32c33-110">You open this resource in the Azure portal in order to see and analyze the telemetry collected from your app.</span></span> <span data-ttu-id="32c33-111">Resursen har identifierats av en *instrumentation nyckeln* (ikey).</span><span class="sxs-lookup"><span data-stu-id="32c33-111">The resource is identified by an *instrumentation key* (ikey).</span></span> <span data-ttu-id="32c33-112">När du installerar Application Insights-paketet för att övervaka din app kan konfigurera du den med instrumentation-nyckel så att den vet var att skicka telemetrin om.</span><span class="sxs-lookup"><span data-stu-id="32c33-112">When you install the Application Insights package to monitor your app, you configure it with the instrumentation key, so that it knows where to send the telemetry.</span></span>

<span data-ttu-id="32c33-113">Vanligtvis väljer att använda separata resurser eller en enda delad resurs i olika scenarier:</span><span class="sxs-lookup"><span data-stu-id="32c33-113">You typically choose to use separate resources or a single shared resource in different scenarios:</span></span>

* <span data-ttu-id="32c33-114">Olika, oberoende program - använda en separat resurs och ikey för varje app.</span><span class="sxs-lookup"><span data-stu-id="32c33-114">Different, independent applications - Use a separate resource and ikey for each app.</span></span>
* <span data-ttu-id="32c33-115">Flera komponenter eller roller från en affärsprogram - använda en [enkel delad resurs](app-insights-monitor-multi-role-apps.md) för komponent-appar.</span><span class="sxs-lookup"><span data-stu-id="32c33-115">Multiple components or roles of one business application - Use a [single shared resource](app-insights-monitor-multi-role-apps.md) for all the component apps.</span></span> <span data-ttu-id="32c33-116">Telemetri kan filtreras eller segmenterade av egenskapen cloud_RoleName.</span><span class="sxs-lookup"><span data-stu-id="32c33-116">Telemetry can be filtered or segmented by the cloud_RoleName property.</span></span>
* <span data-ttu-id="32c33-117">Utveckling och Test Release - använda en separat resurs och ikey för versioner av systemet i 'i stämpel' eller steget i produktion.</span><span class="sxs-lookup"><span data-stu-id="32c33-117">Development, Test, and Release - Use a separate resource and ikey for versions of the system in 'stamp' or stage of production.</span></span>
* <span data-ttu-id="32c33-118">EN | B-testning - använda en enskild resurs.</span><span class="sxs-lookup"><span data-stu-id="32c33-118">A | B testing - Use a single resource.</span></span> <span data-ttu-id="32c33-119">Skapa en TelemetryInitializer om du vill lägga till en egenskap telemetri som identifierar varianter.</span><span class="sxs-lookup"><span data-stu-id="32c33-119">Create a TelemetryInitializer to add a property to the telemetry that identifies the variants.</span></span>


## <span data-ttu-id="32c33-120"><a name="dynamic-ikey"></a>Dynamisk instrumentation nyckel</span><span class="sxs-lookup"><span data-stu-id="32c33-120"><a name="dynamic-ikey"></a> Dynamic instrumentation key</span></span>

<span data-ttu-id="32c33-121">Om du vill göra det enklare att ändra ikey efter koden flyttar mellan stegen i produktionen genom att ange den i koden i stället för i konfigurationsfilen.</span><span class="sxs-lookup"><span data-stu-id="32c33-121">To make it easier to change the ikey as the code moves between stages of production, set it in code instead of in the configuration file.</span></span>

<span data-ttu-id="32c33-122">Ange nyckeln i en initieringsmetod, till exempel global.aspx.cs i en ASP.NET-tjänst:</span><span class="sxs-lookup"><span data-stu-id="32c33-122">Set the key in an initialization method, such as global.aspx.cs in an ASP.NET service:</span></span>

<span data-ttu-id="32c33-123">*C#*</span><span class="sxs-lookup"><span data-stu-id="32c33-123">*C#*</span></span>

    protected void Application_Start()
    {
      Microsoft.ApplicationInsights.Extensibility.
        TelemetryConfiguration.Active.InstrumentationKey = 
          // - for example -
          WebConfigurationManager.AppSettings["ikey"];
      ...

<span data-ttu-id="32c33-124">I det här exemplet placeras ikeys för olika resurser i olika versioner av Webbkonfigurationsfilen.</span><span class="sxs-lookup"><span data-stu-id="32c33-124">In this example, the ikeys for the different resources are placed in different versions of the web configuration file.</span></span> <span data-ttu-id="32c33-125">Byta Webbkonfigurationsfilen - som du kan göra som en del av frigörelseskriptet - ska växla målresursen.</span><span class="sxs-lookup"><span data-stu-id="32c33-125">Swapping the web configuration file - which you can do as part of the release script - will swap the target resource.</span></span>

### <a name="web-pages"></a><span data-ttu-id="32c33-126">Webbsidor</span><span class="sxs-lookup"><span data-stu-id="32c33-126">Web pages</span></span>
<span data-ttu-id="32c33-127">IKey används också i din app webbsidor i den [skript som du har fått från Snabbstart-bladet](app-insights-javascript.md).</span><span class="sxs-lookup"><span data-stu-id="32c33-127">The iKey is also used in your app's web pages, in the [script that you got from the quick start blade](app-insights-javascript.md).</span></span> <span data-ttu-id="32c33-128">I stället för att koda den bokstavligt till skriptet, generera den från servern.</span><span class="sxs-lookup"><span data-stu-id="32c33-128">Instead of coding it literally into the script, generate it from the server state.</span></span> <span data-ttu-id="32c33-129">Till exempel i en ASP.NET-app:</span><span class="sxs-lookup"><span data-stu-id="32c33-129">For example, in an ASP.NET app:</span></span>

<span data-ttu-id="32c33-130">*JavaScript i Razor*</span><span class="sxs-lookup"><span data-stu-id="32c33-130">*JavaScript in Razor*</span></span>

    <script type="text/javascript">
    // Standard Application Insights web page script:
    var appInsights = window.appInsights || function(config){ ...
    // Modify this part:
    }({instrumentationKey:  
      // Generate from server property:
      "@Microsoft.ApplicationInsights.Extensibility.
         TelemetryConfiguration.Active.InstrumentationKey"
    }) // ...


## <a name="create-additional-application-insights-resources"></a><span data-ttu-id="32c33-131">Skapa ytterligare Application Insights-resurser</span><span class="sxs-lookup"><span data-stu-id="32c33-131">Create additional Application Insights resources</span></span>
<span data-ttu-id="32c33-132">Om du vill dela telemetri för olika programkomponenter eller annan stämplar (dev/test/produktion) av samma komponent sedan har du vill skapa en ny Application Insights-resurs.</span><span class="sxs-lookup"><span data-stu-id="32c33-132">To separate telemetry for different application components, or for different stamps (dev/test/production) of the same component, then you'll have to create a new Application Insights resource.</span></span>

<span data-ttu-id="32c33-133">I den [portal.azure.com](https://portal.azure.com), Lägg till Application Insights-resurs:</span><span class="sxs-lookup"><span data-stu-id="32c33-133">In the [portal.azure.com](https://portal.azure.com), add an Application Insights resource:</span></span>

![Klicka på Nytt, Application Insights](./media/app-insights-separate-resources/01-new.png)

* <span data-ttu-id="32c33-135">**Programtyp** påverkar vad som visas på bladet översikt och egenskaper som är tillgängliga i [mått explorer](app-insights-metrics-explorer.md).</span><span class="sxs-lookup"><span data-stu-id="32c33-135">**Application type** affects what you see on the overview blade and the properties available in [metric explorer](app-insights-metrics-explorer.md).</span></span> <span data-ttu-id="32c33-136">Om du inte ser typen av app väljer du något av följande web för webbsidor.</span><span class="sxs-lookup"><span data-stu-id="32c33-136">If you don't see your type of app, choose one of the web types for web pages.</span></span>
* <span data-ttu-id="32c33-137">**Resursgruppen** är i syfte att underlätta för att hantera egenskaper som [åtkomstkontroll](app-insights-resources-roles-access-control.md).</span><span class="sxs-lookup"><span data-stu-id="32c33-137">**Resource group** is a convenience for managing properties like [access control](app-insights-resources-roles-access-control.md).</span></span> <span data-ttu-id="32c33-138">Du kan använda separata resursgrupper för utveckling, testning och produktion.</span><span class="sxs-lookup"><span data-stu-id="32c33-138">You could use separate resource groups for development, test, and production.</span></span>
* <span data-ttu-id="32c33-139">**Prenumerationen** är din betalningskonto i Azure.</span><span class="sxs-lookup"><span data-stu-id="32c33-139">**Subscription** is your payment account in Azure.</span></span>
* <span data-ttu-id="32c33-140">**Plats** är där vi behåller dina data.</span><span class="sxs-lookup"><span data-stu-id="32c33-140">**Location** is where we keep your data.</span></span> <span data-ttu-id="32c33-141">För närvarande kan den inte ändras.</span><span class="sxs-lookup"><span data-stu-id="32c33-141">Currently it can't be changed.</span></span> 
* <span data-ttu-id="32c33-142">**Lägg till instrumentpanelen** placerar en snabb åtkomst-panelen för din resurs på Azure-startsidan.</span><span class="sxs-lookup"><span data-stu-id="32c33-142">**Add to dashboard** puts a quick-access tile for your resource on your Azure Home page.</span></span> 

<span data-ttu-id="32c33-143">Det tar några sekunder för att skapa resursen.</span><span class="sxs-lookup"><span data-stu-id="32c33-143">Creating the resource takes a few seconds.</span></span> <span data-ttu-id="32c33-144">En varning visas när du är klar.</span><span class="sxs-lookup"><span data-stu-id="32c33-144">You'll see an alert when it's done.</span></span>

<span data-ttu-id="32c33-145">(Du kan skriva en [PowerShell-skript](app-insights-powershell-script-create-resource.md) att skapa en resurs automatiskt.)</span><span class="sxs-lookup"><span data-stu-id="32c33-145">(You can write a [PowerShell script](app-insights-powershell-script-create-resource.md) to create a resource automatically.)</span></span>

### <a name="getting-the-instrumentation-key"></a><span data-ttu-id="32c33-146">Hämta nyckeln instrumentation</span><span class="sxs-lookup"><span data-stu-id="32c33-146">Getting the instrumentation key</span></span>
<span data-ttu-id="32c33-147">Nyckeln instrumentation identifierar resursen som du skapade.</span><span class="sxs-lookup"><span data-stu-id="32c33-147">The instrumentation key identifies the resource that you created.</span></span> 

![Klicka på Essentials, nyckeln Instrumentation CTRL + C](./media/app-insights-separate-resources/02-props.png)

<span data-ttu-id="32c33-149">Du måste instrumentation nycklarna för alla resurser som din app ska skicka data.</span><span class="sxs-lookup"><span data-stu-id="32c33-149">You need the instrumentation keys of all the resources to which your app will send data.</span></span>

## <a name="filter-on-build-number"></a><span data-ttu-id="32c33-150">Filtrera efter build-nummer</span><span class="sxs-lookup"><span data-stu-id="32c33-150">Filter on build number</span></span>
<span data-ttu-id="32c33-151">När du publicerar en ny version av din app vill du kunna skilja telemetrin från olika versioner.</span><span class="sxs-lookup"><span data-stu-id="32c33-151">When you publish a new version of your app, you'll want to be able to separate the telemetry from different builds.</span></span>

<span data-ttu-id="32c33-152">Du kan ange egenskapen programversion så att du kan filtrera [Sök](app-insights-diagnostic-search.md) och [mått explorer](app-insights-metrics-explorer.md) resultat.</span><span class="sxs-lookup"><span data-stu-id="32c33-152">You can set the Application Version property so that you can filter [search](app-insights-diagnostic-search.md) and [metric explorer](app-insights-metrics-explorer.md) results.</span></span>

![Filtrering på en egenskap](./media/app-insights-separate-resources/050-filter.png)

<span data-ttu-id="32c33-154">Det finns flera olika metoder för att ange egenskapen programversion.</span><span class="sxs-lookup"><span data-stu-id="32c33-154">There are several different methods of setting the Application Version property.</span></span>

* <span data-ttu-id="32c33-155">Ange direkt:</span><span class="sxs-lookup"><span data-stu-id="32c33-155">Set directly:</span></span>

    `telemetryClient.Context.Component.Version = typeof(MyProject.MyClass).Assembly.GetName().Version;`
* <span data-ttu-id="32c33-156">Omsluta den raden i en [telemetri initieraren](app-insights-api-custom-events-metrics.md#defaults) att kontrollera att alla TelemetryClient instanser är konsekvent.</span><span class="sxs-lookup"><span data-stu-id="32c33-156">Wrap that line in a [telemetry initializer](app-insights-api-custom-events-metrics.md#defaults) to ensure that all TelemetryClient instances are set consistently.</span></span>
* <span data-ttu-id="32c33-157">[ASP.NET] Ange version `BuildInfo.config`.</span><span class="sxs-lookup"><span data-stu-id="32c33-157">[ASP.NET] Set the version in `BuildInfo.config`.</span></span> <span data-ttu-id="32c33-158">Webbmodulen ska hämta version från noden BuildLabel.</span><span class="sxs-lookup"><span data-stu-id="32c33-158">The web module will pick up the version from the BuildLabel node.</span></span> <span data-ttu-id="32c33-159">Inkludera den här filen i projektet och Kom ihåg att ange egenskapen Kopiera alltid i Solution Explorer.</span><span class="sxs-lookup"><span data-stu-id="32c33-159">Include this file in your project and remember to set the Copy Always property in Solution Explorer.</span></span>

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
* <span data-ttu-id="32c33-160">[ASP.NET] Generera BuildInfo.config automatiskt i MSBuild.</span><span class="sxs-lookup"><span data-stu-id="32c33-160">[ASP.NET] Generate BuildInfo.config automatically in MSBuild.</span></span> <span data-ttu-id="32c33-161">Gör du genom att lägga till några rader till din `.csproj` fil:</span><span class="sxs-lookup"><span data-stu-id="32c33-161">To do this, add a few lines to your `.csproj` file:</span></span>

    ```XML

    <PropertyGroup>
      <GenerateBuildInfoConfigFile>true</GenerateBuildInfoConfigFile>    <IncludeServerNameInBuildInfo>true</IncludeServerNameInBuildInfo>
    </PropertyGroup>
    ```

    <span data-ttu-id="32c33-162">Detta genererar en fil med namnet *yourProjectName*. BuildInfo.config. Publiceringsprocessen byter namn på den till BuildInfo.config.</span><span class="sxs-lookup"><span data-stu-id="32c33-162">This generates a file called *yourProjectName*.BuildInfo.config. The Publish process renames it to BuildInfo.config.</span></span>

    <span data-ttu-id="32c33-163">Build-etiketten innehåller platshållare (AutoGen_...) när du skapar med Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="32c33-163">The build label contains a placeholder (AutoGen_...) when you build with Visual Studio.</span></span> <span data-ttu-id="32c33-164">Men om skapats med MSBuild fylls den med rätt versionsnumret.</span><span class="sxs-lookup"><span data-stu-id="32c33-164">But when built with MSBuild, it is populated with the correct version number.</span></span>

    <span data-ttu-id="32c33-165">Ange om du vill tillåta MSBuild att generera versionsnummer version som `1.0.*` i AssemblyReference.cs</span><span class="sxs-lookup"><span data-stu-id="32c33-165">To allow MSBuild to generate version numbers, set the version like `1.0.*` in AssemblyReference.cs</span></span>

## <a name="version-and-release-tracking"></a><span data-ttu-id="32c33-166">Spårning av versionen och utgåva</span><span class="sxs-lookup"><span data-stu-id="32c33-166">Version and release tracking</span></span>
<span data-ttu-id="32c33-167">Om du vill kunna spåra programversionen, se till att `buildinfo.config` genereras av Microsoft Build Engine-processen.</span><span class="sxs-lookup"><span data-stu-id="32c33-167">To track the application version, make sure `buildinfo.config` is generated by your Microsoft Build Engine process.</span></span> <span data-ttu-id="32c33-168">I filen .csproj lägger du till:</span><span class="sxs-lookup"><span data-stu-id="32c33-168">In your .csproj file, add:</span></span>  

```XML

    <PropertyGroup>
      <GenerateBuildInfoConfigFile>true</GenerateBuildInfoConfigFile>    <IncludeServerNameInBuildInfo>true</IncludeServerNameInBuildInfo>
    </PropertyGroup>
```

<span data-ttu-id="32c33-169">När Application Insights-webbmodulen har fått versionsinformationen läggs **programversionen** automatiskt till som en egenskap för alla telemetriobjekt.</span><span class="sxs-lookup"><span data-stu-id="32c33-169">When it has the build info, the Application Insights web module automatically adds **Application version** as a property to every item of telemetry.</span></span> <span data-ttu-id="32c33-170">Det gör att du kan filtrera baserat på version när du utför [diagnostiksökningar](app-insights-diagnostic-search.md) eller när du [undersöker mätvärden](app-insights-metrics-explorer.md).</span><span class="sxs-lookup"><span data-stu-id="32c33-170">That allows you to filter by version when you perform [diagnostic searches](app-insights-diagnostic-search.md), or when you [explore metrics](app-insights-metrics-explorer.md).</span></span>

<span data-ttu-id="32c33-171">Observera dock att build-versionsnumret endast genereras av Microsoft Build Engine, och inte av utvecklarversionen i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="32c33-171">However, notice that the build version number is generated only by the Microsoft Build Engine, not by the developer build in Visual Studio.</span></span>

### <a name="release-annotations"></a><span data-ttu-id="32c33-172">Versionsanteckningar</span><span class="sxs-lookup"><span data-stu-id="32c33-172">Release annotations</span></span>
<span data-ttu-id="32c33-173">Om du använder Visual Studio Team Services, kan du [få en anteckningsmarkör](app-insights-annotations.md) tillagd i diagrammen när du släpper en ny version.</span><span class="sxs-lookup"><span data-stu-id="32c33-173">If you use Visual Studio Team Services, you can [get an annotation marker](app-insights-annotations.md) added to your charts whenever you release a new version.</span></span> <span data-ttu-id="32c33-174">Följande bild visar hur markeringen visas.</span><span class="sxs-lookup"><span data-stu-id="32c33-174">The following image shows how this marker appears.</span></span>

![Skärmbild av exempel på versionsanteckning i ett diagram](./media/app-insights-asp-net/release-annotation.png)
## <a name="next-steps"></a><span data-ttu-id="32c33-176">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="32c33-176">Next steps</span></span>

* [<span data-ttu-id="32c33-177">Delade resurser för flera roller</span><span class="sxs-lookup"><span data-stu-id="32c33-177">Shared resources for multiple roles</span></span>](app-insights-monitor-multi-role-apps.md)
* [<span data-ttu-id="32c33-178">Skapa en telemetri initieraren Skilj A | B varianter</span><span class="sxs-lookup"><span data-stu-id="32c33-178">Create a Telemetry Initializer to distinguish A|B variants</span></span>](app-insights-api-filtering-sampling.md#add-properties)
