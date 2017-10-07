---
title: "aaaFiltering och förbearbetning i hello Azure Application Insights SDK | Microsoft Docs"
description: "Skriva telemetri processorer och telemetri initierare för hello SDK toofilter eller Lägg till egenskaper toohello data innan hello telemetri skickas toohello Application Insights-portalen."
services: application-insights
documentationcenter: 
author: beckylino
manager: carmonm
ms.assetid: 38a9e454-43d5-4dba-a0f0-bd7cd75fb97b
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: multiple
ms.topic: article
ms.date: 11/23/2016
ms.author: bwren
ms.openlocfilehash: 51b9db69b2375b8799718f1b0e1af77620dc2692
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="filtering-and-preprocessing-telemetry-in-hello-application-insights-sdk"></a><span data-ttu-id="4c489-103">Filtrering och förbearbetning telemetri i hello Application Insights SDK</span><span class="sxs-lookup"><span data-stu-id="4c489-103">Filtering and preprocessing telemetry in hello Application Insights SDK</span></span>


<span data-ttu-id="4c489-104">Du kan skriva och konfigurera plugin-program för hello Application Insights SDK toocustomize hur telemetri fångas och bearbetas innan den skickas toohello Application Insights-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="4c489-104">You can write and configure plug-ins for hello Application Insights SDK toocustomize how telemetry is captured and processed before it is sent toohello Application Insights service.</span></span>

* <span data-ttu-id="4c489-105">[Provtagning](app-insights-sampling.md) minskar hello telemetrivolym utan att påverka din statistik.</span><span class="sxs-lookup"><span data-stu-id="4c489-105">[Sampling](app-insights-sampling.md) reduces hello volume of telemetry without affecting your statistics.</span></span> <span data-ttu-id="4c489-106">Den bevarar tillsammans relaterade datapunkter så att du kan navigera mellan dem när de undersöker ett problem.</span><span class="sxs-lookup"><span data-stu-id="4c489-106">It keeps together related data points so that you can navigate between them when diagnosing a problem.</span></span> <span data-ttu-id="4c489-107">Hello-portalen är hello Totalt antal multiplicerade toocompensate för hello provtagning.</span><span class="sxs-lookup"><span data-stu-id="4c489-107">In hello portal, hello total counts are multiplied toocompensate for hello sampling.</span></span>
* <span data-ttu-id="4c489-108">Filtrering med telemetri processorer [för ASP.NET](#filtering) eller [Java](app-insights-java-filter-telemetry.md) kan du välja eller ändra telemetri i hello SDK innan den skickas toohello server.</span><span class="sxs-lookup"><span data-stu-id="4c489-108">Filtering with Telemetry Processors [for ASP.NET](#filtering) or [Java](app-insights-java-filter-telemetry.md) lets you select or modify telemetry in hello SDK before it is sent toohello server.</span></span> <span data-ttu-id="4c489-109">Exempelvis kan du minska hello telemetrivolym exkluderar begäranden från robotar.</span><span class="sxs-lookup"><span data-stu-id="4c489-109">For example, you could reduce hello volume of telemetry by excluding requests from robots.</span></span> <span data-ttu-id="4c489-110">Men filtrering är en grundläggande tillvägagångssätt tooreducing trafik än samplingsfrekvensen.</span><span class="sxs-lookup"><span data-stu-id="4c489-110">But filtering is a more basic approach tooreducing traffic than sampling.</span></span> <span data-ttu-id="4c489-111">Det gör att du mer kontroll över vad skickas men du har toobe medveten om att den påverkar din statistik – till exempel om du filtrerar ut alla lyckade begäranden.</span><span class="sxs-lookup"><span data-stu-id="4c489-111">It allows you more control over what is transmitted, but you have toobe aware that it affects your statistics - for example, if you filter out all successful requests.</span></span>
* <span data-ttu-id="4c489-112">[Telemetri initierare lägga till egenskaper](#add-properties) tooany telemetri som skickas från din app, inklusive telemetri från hello standard moduler.</span><span class="sxs-lookup"><span data-stu-id="4c489-112">[Telemetry Initializers add properties](#add-properties) tooany telemetry sent from your app, including telemetry from hello standard modules.</span></span> <span data-ttu-id="4c489-113">Exempelvis kan du lägga till beräknade värden. eller versionsnummer genom vilka toofilter hello data i hello-portalen.</span><span class="sxs-lookup"><span data-stu-id="4c489-113">For example, you could add calculated values; or version numbers by which toofilter hello data in hello portal.</span></span>
* <span data-ttu-id="4c489-114">[hello SDK API](app-insights-api-custom-events-metrics.md) är används toosend anpassade händelser och mått.</span><span class="sxs-lookup"><span data-stu-id="4c489-114">[hello SDK API](app-insights-api-custom-events-metrics.md) is used toosend custom events and metrics.</span></span>

<span data-ttu-id="4c489-115">Innan du börjar:</span><span class="sxs-lookup"><span data-stu-id="4c489-115">Before you start:</span></span>

* <span data-ttu-id="4c489-116">Installera hello Application Insights [SDK för ASP.NET](app-insights-asp-net.md) eller [SDK för Java](app-insights-java-get-started.md) i din app.</span><span class="sxs-lookup"><span data-stu-id="4c489-116">Install hello Application Insights [SDK for ASP.NET](app-insights-asp-net.md) or [SDK for Java](app-insights-java-get-started.md) in your app.</span></span>

<a name="filtering"></a>

## <a name="filtering-itelemetryprocessor"></a><span data-ttu-id="4c489-117">Filtrering: ITelemetryProcessor</span><span class="sxs-lookup"><span data-stu-id="4c489-117">Filtering: ITelemetryProcessor</span></span>
<span data-ttu-id="4c489-118">Den här metoden ger mer kontroll över vad inkluderas eller uteslutas från hello telemetri dataström.</span><span class="sxs-lookup"><span data-stu-id="4c489-118">This technique gives you more direct control over what is included or excluded from hello telemetry stream.</span></span> <span data-ttu-id="4c489-119">Du kan använda tillsammans med provtagning, eller separat.</span><span class="sxs-lookup"><span data-stu-id="4c489-119">You can use it in conjunction with Sampling, or separately.</span></span>

<span data-ttu-id="4c489-120">toofilter telemetri som du skriver en telemetri processor och registrera den med hello SDK.</span><span class="sxs-lookup"><span data-stu-id="4c489-120">toofilter telemetry, you write a telemetry processor and register it with hello SDK.</span></span> <span data-ttu-id="4c489-121">All telemetri passerar processorn och du kan välja toodrop från hello strömma eller lägga till egenskaper.</span><span class="sxs-lookup"><span data-stu-id="4c489-121">All telemetry goes through your processor, and you can choose toodrop it from hello stream, or add properties.</span></span> <span data-ttu-id="4c489-122">Detta inkluderar telemetri från hello standard moduler, till exempel hello HTTP-begäran insamlaren och hello beroende insamlaren samt telemetri som du har skrivit själv.</span><span class="sxs-lookup"><span data-stu-id="4c489-122">This includes telemetry from hello standard modules such as hello HTTP request collector and hello dependency collector, as well as telemetry you have written yourself.</span></span> <span data-ttu-id="4c489-123">Du kan till exempel filtrera ut telemetri om förfrågningar från robotar eller lyckade beroendeanrop.</span><span class="sxs-lookup"><span data-stu-id="4c489-123">You can, for example, filter out telemetry about requests from robots, or successful dependency calls.</span></span>

> [!WARNING]
> <span data-ttu-id="4c489-124">Filtrering hello telemetri som skickas från hello SDK-processorer kan ge skeva hello statistik du finns i hello-portalen och gör det svårt toofollow relaterade objekt.</span><span class="sxs-lookup"><span data-stu-id="4c489-124">Filtering hello telemetry sent from hello SDK using processors can skew hello statistics that you see in hello portal, and make it difficult toofollow related items.</span></span>
>
> <span data-ttu-id="4c489-125">I stället använda [provtagning](app-insights-sampling.md).</span><span class="sxs-lookup"><span data-stu-id="4c489-125">Instead, consider using [sampling](app-insights-sampling.md).</span></span>
>
>

### <a name="create-a-telemetry-processor-c"></a><span data-ttu-id="4c489-126">Skapa en telemetri-processor (C#)</span><span class="sxs-lookup"><span data-stu-id="4c489-126">Create a telemetry processor (C#)</span></span>
1. <span data-ttu-id="4c489-127">Kontrollera att hello Application Insights SDK i projektet är version 2.0.0 eller senare.</span><span class="sxs-lookup"><span data-stu-id="4c489-127">Verify that hello Application Insights SDK in your project is  version 2.0.0 or later.</span></span> <span data-ttu-id="4c489-128">Högerklicka på projektet i Visual Studio Solution Explorer och välja hantera NuGet-paket.</span><span class="sxs-lookup"><span data-stu-id="4c489-128">Right-click your project in Visual Studio Solution Explorer and choose Manage NuGet Packages.</span></span> <span data-ttu-id="4c489-129">Kontrollera Microsoft.ApplicationInsights.Web i NuGet-Pakethanteraren.</span><span class="sxs-lookup"><span data-stu-id="4c489-129">In NuGet package manager, check Microsoft.ApplicationInsights.Web.</span></span>
2. <span data-ttu-id="4c489-130">toocreate ett filter, implementera ITelemetryProcessor.</span><span class="sxs-lookup"><span data-stu-id="4c489-130">toocreate a filter, implement ITelemetryProcessor.</span></span> <span data-ttu-id="4c489-131">Det här är en annan utökningspunkt som telemetri modul, telemetri initieraren och telemetri kanal.</span><span class="sxs-lookup"><span data-stu-id="4c489-131">This is another extensibility point like telemetry module, telemetry initializer, and telemetry channel.</span></span>

    <span data-ttu-id="4c489-132">Observera att telemetri processorer konstruera en kedja av bearbetning.</span><span class="sxs-lookup"><span data-stu-id="4c489-132">Notice that Telemetry Processors construct a chain of processing.</span></span> <span data-ttu-id="4c489-133">När du initierar en telemetri processor, kan du skicka en länk toohello nästa processor i hello kedja.</span><span class="sxs-lookup"><span data-stu-id="4c489-133">When you instantiate a telemetry processor, you pass a link toohello next processor in hello chain.</span></span> <span data-ttu-id="4c489-134">När toohello Process-metoden skickas en datapunkt telemetri sker arbetet och sedan anrop hello nästa telemetri Processor i hello kedja.</span><span class="sxs-lookup"><span data-stu-id="4c489-134">When a telemetry data point is passed toohello Process method, it does its work and then calls hello next Telemetry Processor in hello chain.</span></span>

    ``` C#

    using Microsoft.ApplicationInsights.Channel;
    using Microsoft.ApplicationInsights.Extensibility;

    public class SuccessfulDependencyFilter : ITelemetryProcessor
      {

        private ITelemetryProcessor Next { get; set; }

        // You can pass values from .config
        public string MyParamFromConfigFile { get; set; }

        // Link processors tooeach other in a chain.
        public SuccessfulDependencyFilter(ITelemetryProcessor next)
        {
            this.Next = next;
        }
        public void Process(ITelemetry item)
        {
            // toofilter out an item, just return
            if (!OKtoSend(item)) { return; }
            // Modify hello item if required
            ModifyItem(item);

            this.Next.Process(item);
        }

        // Example: replace with your own criteria.
        private bool OKtoSend (ITelemetry item)
        {
            var dependency = item as DependencyTelemetry;
            if (dependency == null) return true;

            return dependency.Success != true;
        }

        // Example: replace with your own modifiers.
        private void ModifyItem (ITelemetry item)
        {
            item.Context.Properties.Add("app-version", "1." + MyParamFromConfigFile);
        }
    }

    ```
1. <span data-ttu-id="4c489-135">Infoga det i ApplicationInsights.config:</span><span class="sxs-lookup"><span data-stu-id="4c489-135">Insert this in ApplicationInsights.config:</span></span>

```XML

    <TelemetryProcessors>
      <Add Type="WebApplication9.SuccessfulDependencyFilter, WebApplication9">
         <!-- Set public property -->
         <MyParamFromConfigFile>2-beta</MyParamFromConfigFile>
      </Add>
    </TelemetryProcessors>

```

<span data-ttu-id="4c489-136">(Detta är hello samma avsnitt där du vill initiera ett filter för provtagning.)</span><span class="sxs-lookup"><span data-stu-id="4c489-136">(This is hello same section where you initialize a sampling filter.)</span></span>

<span data-ttu-id="4c489-137">Du kan överföra strängvärden från hello .config-fil genom att tillhandahålla offentliga namngivna egenskaper i klassen.</span><span class="sxs-lookup"><span data-stu-id="4c489-137">You can pass string values from hello .config file by providing public named properties in your class.</span></span>

> [!WARNING]
> <span data-ttu-id="4c489-138">Ta hand toomatch hello typnamn och eventuella egenskapsnamn i hello .config-fil toohello klass och egenskapsnamn i hello kod.</span><span class="sxs-lookup"><span data-stu-id="4c489-138">Take care toomatch hello type name and any property names in hello .config file toohello class and property names in hello code.</span></span> <span data-ttu-id="4c489-139">Om hello .config-filen refererar till en obefintlig eller egenskapen, misslyckas hello SDK tyst toosend alla telemetri.</span><span class="sxs-lookup"><span data-stu-id="4c489-139">If hello .config file references a non-existent type or property, hello SDK may silently fail toosend any telemetry.</span></span>
>
>

<span data-ttu-id="4c489-140">**Du kan också** du kan initiera hello filter i koden.</span><span class="sxs-lookup"><span data-stu-id="4c489-140">**Alternatively,** you can initialize hello filter in code.</span></span> <span data-ttu-id="4c489-141">I en lämplig initiering – till exempel AppStart i Global.asax.cs - Infoga processorn i kedjan hello:</span><span class="sxs-lookup"><span data-stu-id="4c489-141">In a suitable initialization class - for example AppStart in Global.asax.cs - insert your processor into hello chain:</span></span>

```C#

    var builder = TelemetryConfiguration.Active.TelemetryProcessorChainBuilder;
    builder.Use((next) => new SuccessfulDependencyFilter(next));

    // If you have more processors:
    builder.Use((next) => new AnotherProcessor(next));

    builder.Build();

```

<span data-ttu-id="4c489-142">TelemetryClients som skapats efter den här punkten kommer att använda din processorer.</span><span class="sxs-lookup"><span data-stu-id="4c489-142">TelemetryClients created after this point will use your processors.</span></span>

### <a name="example-filters"></a><span data-ttu-id="4c489-143">Exempel filter</span><span class="sxs-lookup"><span data-stu-id="4c489-143">Example filters</span></span>
#### <a name="synthetic-requests"></a><span data-ttu-id="4c489-144">Syntetiska begäranden</span><span class="sxs-lookup"><span data-stu-id="4c489-144">Synthetic requests</span></span>
<span data-ttu-id="4c489-145">Filtrera ut robotar och web tester.</span><span class="sxs-lookup"><span data-stu-id="4c489-145">Filter out bots and web tests.</span></span> <span data-ttu-id="4c489-146">Även om Metrics Explorer ger du Hej alternativet toofilter ut syntetiska källor, det här alternativet minskar trafiken genom att filtrera dem på hello SDK.</span><span class="sxs-lookup"><span data-stu-id="4c489-146">Although Metrics Explorer gives you hello option toofilter out synthetic sources, this option reduces traffic by filtering them at hello SDK.</span></span>

``` C#

    public void Process(ITelemetry item)
    {
      if (!string.IsNullOrEmpty(item.Context.Operation.SyntheticSource)) {return;}

      // Send everything else:
      this.Next.Process(item);
    }

```

#### <a name="failed-authentication"></a><span data-ttu-id="4c489-147">Misslyckad verifiering</span><span class="sxs-lookup"><span data-stu-id="4c489-147">Failed authentication</span></span>
<span data-ttu-id="4c489-148">Filtrera ut begäranden med svaret ”401”.</span><span class="sxs-lookup"><span data-stu-id="4c489-148">Filter out requests with a "401" response.</span></span>

```C#

public void Process(ITelemetry item)
{
    var request = item as RequestTelemetry;

    if (request != null &&
    request.ResponseCode.Equals("401", StringComparison.OrdinalIgnoreCase))
    {
        // toofilter out an item, just terminate hello chain:
        return;
    }
    // Send everything else:
    this.Next.Process(item);
}

```

#### <a name="filter-out-fast-remote-dependency-calls"></a><span data-ttu-id="4c489-149">Filtrera bort snabb remote beroendeanrop</span><span class="sxs-lookup"><span data-stu-id="4c489-149">Filter out fast remote dependency calls</span></span>
<span data-ttu-id="4c489-150">Om du bara vill toodiagnose anrop som är långsamma filtrera bort hello fast som.</span><span class="sxs-lookup"><span data-stu-id="4c489-150">If you only want toodiagnose calls that are slow, filter out hello fast ones.</span></span>

> [!NOTE]
> <span data-ttu-id="4c489-151">Detta kan ge skeva hello statistik som du ser på hello-portalen.</span><span class="sxs-lookup"><span data-stu-id="4c489-151">This will skew hello statistics you see on hello portal.</span></span> <span data-ttu-id="4c489-152">hello beroende diagram ser ut som om hello beroendeanrop är alla fel.</span><span class="sxs-lookup"><span data-stu-id="4c489-152">hello dependency chart will look as if hello dependency calls are all failures.</span></span>
>
>

``` C#

public void Process(ITelemetry item)
{
    var request = item as DependencyTelemetry;

    if (request != null && request.Duration.TotalMilliseconds < 100)
    {
        return;
    }
    this.Next.Process(item);
}

```

#### <a name="diagnose-dependency-issues"></a><span data-ttu-id="4c489-153">Diagnostisera beroendeproblem</span><span class="sxs-lookup"><span data-stu-id="4c489-153">Diagnose dependency issues</span></span>
<span data-ttu-id="4c489-154">[Den här bloggen](https://azure.microsoft.com/blog/implement-an-application-insights-telemetry-processor/) beskriver ett projekt toodiagnose beroende problem genom att automatiskt skicka regelbunden ping toodependencies.</span><span class="sxs-lookup"><span data-stu-id="4c489-154">[This blog](https://azure.microsoft.com/blog/implement-an-application-insights-telemetry-processor/) describes a project toodiagnose dependency issues by automatically sending regular pings toodependencies.</span></span>


<a name="add-properties"></a>

## <a name="add-properties-itelemetryinitializer"></a><span data-ttu-id="4c489-155">Lägga till egenskaper: ITelemetryInitializer</span><span class="sxs-lookup"><span data-stu-id="4c489-155">Add properties: ITelemetryInitializer</span></span>
<span data-ttu-id="4c489-156">Använda telemetri initierare toodefine globala egenskaper som skickas med all telemetri; och toooverride valda beteendet för hello standard telemetri moduler.</span><span class="sxs-lookup"><span data-stu-id="4c489-156">Use telemetry initializers toodefine global properties that are sent with all telemetry; and toooverride selected behavior of hello standard telemetry modules.</span></span>

<span data-ttu-id="4c489-157">Till exempel hello Application Insights för webbpaketet samlar in telemetri om HTTP-förfrågningar.</span><span class="sxs-lookup"><span data-stu-id="4c489-157">For example, hello Application Insights for Web package collects telemetry about HTTP requests.</span></span> <span data-ttu-id="4c489-158">Som standard flaggor som misslyckad varje begäran med en svarskoden > = 400.</span><span class="sxs-lookup"><span data-stu-id="4c489-158">By default, it flags as failed any request with a response code >= 400.</span></span> <span data-ttu-id="4c489-159">Men om du vill tootreat 400 som lyckas, kan du ange en telemetri initieraren som anger hello lyckade egenskapen.</span><span class="sxs-lookup"><span data-stu-id="4c489-159">But if you want tootreat 400 as a success, you can provide a telemetry initializer that sets hello Success property.</span></span>

<span data-ttu-id="4c489-160">Om du anger en telemetri initieraren kallas när någon av hello Track*() metoder anropas.</span><span class="sxs-lookup"><span data-stu-id="4c489-160">If you provide a telemetry initializer, it is called whenever any of hello Track*() methods is called.</span></span> <span data-ttu-id="4c489-161">Detta inkluderar metoder anropas av hello standard telemetri moduler.</span><span class="sxs-lookup"><span data-stu-id="4c489-161">This includes methods called by hello standard telemetry modules.</span></span> <span data-ttu-id="4c489-162">Dessa moduler ange konventionen är inte en egenskap som redan har ställts in av en initierare.</span><span class="sxs-lookup"><span data-stu-id="4c489-162">By convention, these modules do not set any property that has already been set by an initializer.</span></span>

<span data-ttu-id="4c489-163">**Definiera din initieraren**</span><span class="sxs-lookup"><span data-stu-id="4c489-163">**Define your initializer**</span></span>

<span data-ttu-id="4c489-164">*C#*</span><span class="sxs-lookup"><span data-stu-id="4c489-164">*C#*</span></span>

```C#

    using System;
    using Microsoft.ApplicationInsights.Channel;
    using Microsoft.ApplicationInsights.DataContracts;
    using Microsoft.ApplicationInsights.Extensibility;

    namespace MvcWebRole.Telemetry
    {
      /*
       * Custom TelemetryInitializer that overrides hello default SDK
       * behavior of treating response codes >= 400 as failed requests
       *
       */
      public class MyTelemetryInitializer : ITelemetryInitializer
      {
        public void Initialize(ITelemetry telemetry)
        {
            var requestTelemetry = telemetry as RequestTelemetry;
            // Is this a TrackRequest() ?
            if (requestTelemetry == null) return;
            int code;
            bool parsed = Int32.TryParse(requestTelemetry.ResponseCode, out code);
            if (!parsed) return;
            if (code >= 400 && code < 500)
            {
                // If we set hello Success property, hello SDK won't change it:
                requestTelemetry.Success = true;
                // Allow us toofilter these requests in hello portal:
                requestTelemetry.Context.Properties["Overridden400s"] = "true";
            }
            // else leave hello SDK tooset hello Success property      
        }
      }
    }
```

<span data-ttu-id="4c489-165">**Läsa in din initieraren**</span><span class="sxs-lookup"><span data-stu-id="4c489-165">**Load your initializer**</span></span>

<span data-ttu-id="4c489-166">I ApplicationInsights.config:</span><span class="sxs-lookup"><span data-stu-id="4c489-166">In ApplicationInsights.config:</span></span>

    <ApplicationInsights>
      <TelemetryInitializers>
        <!-- Fully qualified type name, assembly name: -->
        <Add Type="MvcWebRole.Telemetry.MyTelemetryInitializer, MvcWebRole"/>
        ...
      </TelemetryInitializers>
    </ApplicationInsights>

<span data-ttu-id="4c489-167">*Du kan också* du kan initiera hello initierare i koden, till exempel i Global.aspx.cs:</span><span class="sxs-lookup"><span data-stu-id="4c489-167">*Alternatively,* you can instantiate hello initializer in code, for example in Global.aspx.cs:</span></span>

```C#
    protected void Application_Start()
    {
        // ...
        TelemetryConfiguration.Active.TelemetryInitializers
        .Add(new MyTelemetryInitializer());
    }
```


[<span data-ttu-id="4c489-168">Se mer av det här exemplet.</span><span class="sxs-lookup"><span data-stu-id="4c489-168">See more of this sample.</span></span>](https://github.com/Microsoft/ApplicationInsights-Home/tree/master/Samples/AzureEmailService/MvcWebRole)

<a name="js-initializer"></a>

### <a name="javascript-telemetry-initializers"></a><span data-ttu-id="4c489-169">JavaScript telemetri initierare</span><span class="sxs-lookup"><span data-stu-id="4c489-169">JavaScript telemetry initializers</span></span>
<span data-ttu-id="4c489-170">*JavaScript*</span><span class="sxs-lookup"><span data-stu-id="4c489-170">*JavaScript*</span></span>

<span data-ttu-id="4c489-171">Infoga en telemetri initieraren omedelbart efter hello initieringskod som du fick från portalen hello:</span><span class="sxs-lookup"><span data-stu-id="4c489-171">Insert a telemetry initializer immediately after hello initialization code that you got from hello portal:</span></span>

```JS

    <script type="text/javascript">
        // ... initialization code
        ...({
            instrumentationKey: "your instrumentation key"
        });
        window.appInsights = appInsights;


        // Adding telemetry initializer.
        // This is called whenever a new telemetry item
        // is created.

        appInsights.queue.push(function () {
            appInsights.context.addTelemetryInitializer(function (envelope) {
                var telemetryItem = envelope.data.baseData;

                // toocheck hello telemetry item’s type - for example PageView:
                if (envelope.name == Microsoft.ApplicationInsights.Telemetry.PageView.envelopeType) {
                    // this statement removes url from all page view documents
                    telemetryItem.url = "URL CENSORED";
                }

                // tooset custom properties:
                telemetryItem.properties = telemetryItem.properties || {};
                telemetryItem.properties["globalProperty"] = "boo";

                // tooset custom metrics:
                telemetryItem.measurements = telemetryItem.measurements || {};
                telemetryItem.measurements["globalMetric"] = 100;
            });
        });

        // End of inserted code.

        appInsights.trackPageView();
    </script>
```

<span data-ttu-id="4c489-172">En sammanfattning av hello icke anpassade egenskaper som är tillgängliga på hello telemetryItem finns [exportera datamodellen i Application Insights](app-insights-export-data-model.md).</span><span class="sxs-lookup"><span data-stu-id="4c489-172">For a summary of hello non-custom properties available on hello telemetryItem, see [Application Insights Export Data Model](app-insights-export-data-model.md).</span></span>

<span data-ttu-id="4c489-173">Du kan lägga till så många initierare som du vill.</span><span class="sxs-lookup"><span data-stu-id="4c489-173">You can add as many initializers as you like.</span></span>

## <a name="itelemetryprocessor-and-itelemetryinitializer"></a><span data-ttu-id="4c489-174">ITelemetryProcessor och ITelemetryInitializer</span><span class="sxs-lookup"><span data-stu-id="4c489-174">ITelemetryProcessor and ITelemetryInitializer</span></span>
<span data-ttu-id="4c489-175">Vad är hello skillnaden mellan telemetri processorer och telemetri initierare?</span><span class="sxs-lookup"><span data-stu-id="4c489-175">What's hello difference between telemetry processors and telemetry initializers?</span></span>

* <span data-ttu-id="4c489-176">Det finns vissa överlappningar i vad du kan göra med dem: kan vara används tooadd egenskaper tootelemetry.</span><span class="sxs-lookup"><span data-stu-id="4c489-176">There are some overlaps in what you can do with them: both can be used tooadd properties tootelemetry.</span></span>
* <span data-ttu-id="4c489-177">TelemetryInitializers körs alltid innan TelemetryProcessors.</span><span class="sxs-lookup"><span data-stu-id="4c489-177">TelemetryInitializers always run before TelemetryProcessors.</span></span>
* <span data-ttu-id="4c489-178">TelemetryProcessors Tillåt toocompletely ersätta eller ta bort ett telemetri-objekt.</span><span class="sxs-lookup"><span data-stu-id="4c489-178">TelemetryProcessors allow you toocompletely replace or discard a telemetry item.</span></span>
* <span data-ttu-id="4c489-179">TelemetryProcessors bearbeta inte prestandaräknaren telemetri.</span><span class="sxs-lookup"><span data-stu-id="4c489-179">TelemetryProcessors don't process performance counter telemetry.</span></span>


## <a name="reference-docs"></a><span data-ttu-id="4c489-180">Referensdokument</span><span class="sxs-lookup"><span data-stu-id="4c489-180">Reference docs</span></span>
* [<span data-ttu-id="4c489-181">API-översikt</span><span class="sxs-lookup"><span data-stu-id="4c489-181">API Overview</span></span>](app-insights-api-custom-events-metrics.md)
* [<span data-ttu-id="4c489-182">ASP.NET-referens</span><span class="sxs-lookup"><span data-stu-id="4c489-182">ASP.NET reference</span></span>](https://msdn.microsoft.com/library/dn817570.aspx)

## <a name="sdk-code"></a><span data-ttu-id="4c489-183">SDK-kod</span><span class="sxs-lookup"><span data-stu-id="4c489-183">SDK Code</span></span>
* [<span data-ttu-id="4c489-184">ASP.NET Core SDK</span><span class="sxs-lookup"><span data-stu-id="4c489-184">ASP.NET Core SDK</span></span>](https://github.com/Microsoft/ApplicationInsights-dotnet)
* [<span data-ttu-id="4c489-185">ASP.NET 5</span><span class="sxs-lookup"><span data-stu-id="4c489-185">ASP.NET 5</span></span>](https://github.com/Microsoft/ApplicationInsights-aspnet5)
* [<span data-ttu-id="4c489-186">JavaScript SDK</span><span class="sxs-lookup"><span data-stu-id="4c489-186">JavaScript SDK</span></span>](https://github.com/Microsoft/ApplicationInsights-JS)

## <span data-ttu-id="4c489-187"><a name="next"></a>Nästa steg</span><span class="sxs-lookup"><span data-stu-id="4c489-187"><a name="next"></a>Next steps</span></span>
* [<span data-ttu-id="4c489-188">Sök händelser och loggar</span><span class="sxs-lookup"><span data-stu-id="4c489-188">Search events and logs</span></span>](app-insights-diagnostic-search.md)
* [<span data-ttu-id="4c489-189">Sampling</span><span class="sxs-lookup"><span data-stu-id="4c489-189">Sampling</span></span>](app-insights-sampling.md)
* [<span data-ttu-id="4c489-190">Felsökning</span><span class="sxs-lookup"><span data-stu-id="4c489-190">Troubleshooting</span></span>](app-insights-troubleshoot-faq.md)
