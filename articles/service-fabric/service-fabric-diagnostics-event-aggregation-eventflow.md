---
title: "aaaAzure Service Fabric händelse aggregeringen med EventFlow | Microsoft Docs"
description: "Läs mer om sammanställa och samlar in händelser med hjälp av EventFlow för övervakning och diagnostik av Azure Service Fabric-kluster."
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: timlt
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/30/2017
ms.author: dekapur
ms.openlocfilehash: c0141d3ed72d835139250af3589e298fd22d8f89
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="event-aggregation-and-collection-using-eventflow"></a><span data-ttu-id="a7eae-103">Aggregering av händelse och med EventFlow</span><span class="sxs-lookup"><span data-stu-id="a7eae-103">Event aggregation and collection using EventFlow</span></span>

<span data-ttu-id="a7eae-104">[Microsoft Diagnostics EventFlow](https://github.com/Azure/diagnostics-eventflow) kan vidarebefordra händelser från en nod tooone eller mer övervakning mål.</span><span class="sxs-lookup"><span data-stu-id="a7eae-104">[Microsoft Diagnostics EventFlow](https://github.com/Azure/diagnostics-eventflow) can route events from a node tooone or more monitoring destinations.</span></span> <span data-ttu-id="a7eae-105">Eftersom den ingår som en NuGet-paket i projektet service färdas EventFlow koden och konfigurationen med hello-tjänsten, vilket eliminerar hello per nod konfigurationsproblem som tidigare nämnts om Azure-diagnostik.</span><span class="sxs-lookup"><span data-stu-id="a7eae-105">Because it is included as a NuGet package in your service project, EventFlow code and configuration travel with hello service, eliminating hello per-node configuration issue mentioned earlier about Azure Diagnostics.</span></span> <span data-ttu-id="a7eae-106">EventFlow körs inom din tjänstprocessen och ansluter direkt toohello konfigurerats utdata.</span><span class="sxs-lookup"><span data-stu-id="a7eae-106">EventFlow runs within your service process, and connects directly toohello configured outputs.</span></span> <span data-ttu-id="a7eae-107">På grund av hello direkt anslutning fungerar EventFlow för Azure-behållaren och lokala distributioner.</span><span class="sxs-lookup"><span data-stu-id="a7eae-107">Because of hello direct connection, EventFlow works for Azure, container, and on-premises service deployments.</span></span> <span data-ttu-id="a7eae-108">Var försiktig om du kör EventFlow i scenarier med hög densitet som i en behållare, eftersom varje EventFlow pipeline gör en extern anslutning.</span><span class="sxs-lookup"><span data-stu-id="a7eae-108">Be careful if you run EventFlow in high-density scenarios, such as in a container, because each EventFlow pipeline makes an external connection.</span></span> <span data-ttu-id="a7eae-109">Så om du har flera processer får du flera utgående anslutningar!</span><span class="sxs-lookup"><span data-stu-id="a7eae-109">So, if you host several processes, you get several outbound connections!</span></span> <span data-ttu-id="a7eae-110">Detta är inte lika mycket problem för Service Fabric-program, eftersom alla repliker av en `ServiceType` kör samma process i hello och det begränsar hello antal utgående anslutningar.</span><span class="sxs-lookup"><span data-stu-id="a7eae-110">This isn't as much a concern for Service Fabric applications, because all replicas of a `ServiceType` run in hello same process, and this limits hello number of outbound connections.</span></span> <span data-ttu-id="a7eae-111">EventFlow erbjuder också händelsefiltrering så att endast hello-händelser som matchar angivna hello-filtret skickas.</span><span class="sxs-lookup"><span data-stu-id="a7eae-111">EventFlow also offers event filtering, so that only hello events that match hello specified filter are sent.</span></span>

## <a name="setting-up-eventflow"></a><span data-ttu-id="a7eae-112">Konfigurera EventFlow</span><span class="sxs-lookup"><span data-stu-id="a7eae-112">Setting up EventFlow</span></span>

<span data-ttu-id="a7eae-113">EventFlow binärfiler är tillgängliga som en uppsättning NuGet-paket.</span><span class="sxs-lookup"><span data-stu-id="a7eae-113">EventFlow binaries are available as a set of NuGet packages.</span></span> <span data-ttu-id="a7eae-114">tooadd EventFlow tooa Service Fabric service-projekt, högerklicka på hello-projekt i hello Solution Explorer och välj ”Hantera NuGet-paket”.</span><span class="sxs-lookup"><span data-stu-id="a7eae-114">tooadd EventFlow tooa Service Fabric service project, right-click hello project in hello Solution Explorer and choose "Manage NuGet packages."</span></span> <span data-ttu-id="a7eae-115">Växla toohello ”Bläddra” fliken och Sök efter ”`Diagnostics.EventFlow`”:</span><span class="sxs-lookup"><span data-stu-id="a7eae-115">Switch toohello "Browse" tab and search for "`Diagnostics.EventFlow`":</span></span>

![EventFlow NuGet-paket i Visual Studio-NuGet-Pakethanteraren UI](./media/service-fabric-diagnostics-event-aggregation-eventflow/eventflow-nuget.png)

<span data-ttu-id="a7eae-117">En lista över olika paket visas, märkt med ”anger” och ”utdata” visas.</span><span class="sxs-lookup"><span data-stu-id="a7eae-117">You will see a list of various packages show up, labeled with "Inputs" and "Outputs".</span></span> <span data-ttu-id="a7eae-118">EventFlow stöder olika providrar för olika loggning och analyzers.</span><span class="sxs-lookup"><span data-stu-id="a7eae-118">EventFlow supports various different logging providers and analyzers.</span></span> <span data-ttu-id="a7eae-119">hello-tjänst som är värd EventFlow bör inkludera paket beroende på hello källa och mål för hello programloggar.</span><span class="sxs-lookup"><span data-stu-id="a7eae-119">hello service hosting EventFlow should include appropriate packages depending on hello source and destination for hello application logs.</span></span> <span data-ttu-id="a7eae-120">Dessutom toohello core ServiceFabric paketet, behöver du också minst ett indata- och konfigurerats.</span><span class="sxs-lookup"><span data-stu-id="a7eae-120">In addition toohello core ServiceFabric package, you also need at least one Input and Output configured.</span></span> <span data-ttu-id="a7eae-121">Exmaple, kan du lägga till följande paket toosent EventSource händelser tooApplication insikter hello:</span><span class="sxs-lookup"><span data-stu-id="a7eae-121">For exmaple, you can add hello following packages toosent EventSource events tooApplication Insights:</span></span>

* <span data-ttu-id="a7eae-122">`Microsoft.Diagnostics.EventFlow.Input.EventSource`toocapture data från hello-EventSource tjänstklassen och standard EventSources som *Microsoft ServiceFabric Services* och *Microsoft-ServiceFabric-aktörer*)</span><span class="sxs-lookup"><span data-stu-id="a7eae-122">`Microsoft.Diagnostics.EventFlow.Input.EventSource` toocapture data from hello service's EventSource class, and from standard EventSources such as *Microsoft-ServiceFabric-Services* and *Microsoft-ServiceFabric-Actors*)</span></span>
* <span data-ttu-id="a7eae-123">`Microsoft.Diagnostics.EventFlow.Output.ApplicationInsights`(vi toosend hello loggar tooan Azure Application Insights-resurs)</span><span class="sxs-lookup"><span data-stu-id="a7eae-123">`Microsoft.Diagnostics.EventFlow.Output.ApplicationInsights` (we are going toosend hello logs tooan Azure Application Insights resource)</span></span>
* <span data-ttu-id="a7eae-124">`Microsoft.Diagnostics.EventFlow.ServiceFabric`(aktiverar initieringen av hello EventFlow pipeline från Service Fabric tjänstkonfiguration och rapporterar eventuella problem med att skicka diagnostikdata som Service Fabric hälsorapporter)</span><span class="sxs-lookup"><span data-stu-id="a7eae-124">`Microsoft.Diagnostics.EventFlow.ServiceFabric`(enables initialization of hello EventFlow pipeline from Service Fabric service configuration and reports any problems with sending diagnostic data as Service Fabric health reports)</span></span>

>[!NOTE]
><span data-ttu-id="a7eae-125">`Microsoft.Diagnostics.EventFlow.Input.EventSource`paketet kräver hello service-projekt tootarget .NET Framework 4.6 eller senare.</span><span class="sxs-lookup"><span data-stu-id="a7eae-125">`Microsoft.Diagnostics.EventFlow.Input.EventSource` package requires hello service project tootarget .NET Framework 4.6 or newer.</span></span> <span data-ttu-id="a7eae-126">Se till att ange hello lämpliga Målversionen av framework i projektegenskaperna innan du installerar det här paketet.</span><span class="sxs-lookup"><span data-stu-id="a7eae-126">Make sure you set hello appropriate target framework in project properties before installing this package.</span></span>

<span data-ttu-id="a7eae-127">När alla hello paket som är installerade, hello nästa steg är tooconfigure och aktivera EventFlow i hello-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="a7eae-127">After all hello packages are installed, hello next step is tooconfigure and enable EventFlow in hello service.</span></span>

## <a name="configuring-and-enabling-log-collection"></a><span data-ttu-id="a7eae-128">Konfigurerar och aktiverar Logginsamling</span><span class="sxs-lookup"><span data-stu-id="a7eae-128">Configuring and enabling log collection</span></span>
<span data-ttu-id="a7eae-129">Hej EventFlow pipeline ansvarar för att skicka hello loggar har skapats från en specifikation som lagras i en konfigurationsfil.</span><span class="sxs-lookup"><span data-stu-id="a7eae-129">hello EventFlow pipeline responsible for sending hello logs is created from a specification stored in a configuration file.</span></span> <span data-ttu-id="a7eae-130">Hej `Microsoft.Diagnostics.EventFlow.ServiceFabric` paketet installerar en första EventFlow konfigurationsfil under `PackageRoot\Config` lösningsmapp som heter `eventFlowConfig.json`.</span><span class="sxs-lookup"><span data-stu-id="a7eae-130">hello `Microsoft.Diagnostics.EventFlow.ServiceFabric` package installs a starting EventFlow configuration file under `PackageRoot\Config` solution folder, named `eventFlowConfig.json`.</span></span> <span data-ttu-id="a7eae-131">Den här konfigurationsfilen måste toobe ändrade toocapture data från hello standardtjänsten `EventSource` klassen och alla andra indata som du vill tooconfigure och skicka data toohello rätt plats.</span><span class="sxs-lookup"><span data-stu-id="a7eae-131">This configuration file needs toobe modified toocapture data from hello default service `EventSource` class, and any other inputs you want tooconfigure, and send data toohello appropriate place.</span></span>

<span data-ttu-id="a7eae-132">Här är ett exempel *eventFlowConfig.json* baserat på hello NuGet-paket som nämns ovan:</span><span class="sxs-lookup"><span data-stu-id="a7eae-132">Here is a sample *eventFlowConfig.json* based on hello NuGet packages mentioned above:</span></span>
```json
{
  "inputs": [
    {
      "type": "EventSource",
      "sources": [
        { "providerName": "Microsoft-ServiceFabric-Services" },
        { "providerName": "Microsoft-ServiceFabric-Actors" },
        // (replace hello following value with your service's ServiceEventSource name)
        { "providerName": "your-service-EventSource-name" }
      ]
    }
  ],
  "filters": [
    {
      "type": "drop",
      "include": "Level == Verbose"
    }
  ],
  "outputs": [
    {
      "type": "ApplicationInsights",
      // (replace hello following value with your AI resource's instrumentation key)
      "instrumentationKey": "00000000-0000-0000-0000-000000000000"
    }
  ],
  "schemaVersion": "2016-08-11"
}
```

<span data-ttu-id="a7eae-133">hello tjänstens ServiceEventSource heter hello värdet för hello namnegenskapen i hello `EventSourceAttribute` tillämpas toohello ServiceEventSource klass.</span><span class="sxs-lookup"><span data-stu-id="a7eae-133">hello name of service's ServiceEventSource is hello value of hello Name property of hello `EventSourceAttribute` applied toohello ServiceEventSource class.</span></span> <span data-ttu-id="a7eae-134">Det har angetts i hello `ServiceEventSource.cs` fil som är en del av hello service-kod.</span><span class="sxs-lookup"><span data-stu-id="a7eae-134">It is all specified in hello `ServiceEventSource.cs` file, which is part of hello service code.</span></span> <span data-ttu-id="a7eae-135">Till exempel i hello följande kodfragment hello hello ServiceEventSource heter *mitt företag-Application1-Stateless1*:</span><span class="sxs-lookup"><span data-stu-id="a7eae-135">For example, in hello following code snippet hello name of hello ServiceEventSource is *MyCompany-Application1-Stateless1*:</span></span>

```csharp
[EventSource(Name = "MyCompany-Application1-Stateless1")]
internal sealed class ServiceEventSource : EventSource
{
    // (rest of ServiceEventSource implementation)
}
```

<span data-ttu-id="a7eae-136">Observera att `eventFlowConfig.json` filen är en del av tjänsten konfigurationspaket.</span><span class="sxs-lookup"><span data-stu-id="a7eae-136">Note that `eventFlowConfig.json` file is part of service configuration package.</span></span> <span data-ttu-id="a7eae-137">Ändringar toothis filen kan ingå i fullständig eller configuration-endast uppgraderingar av hello service, ämne tooService Fabric uppgradera hälsokontroller och automatisk återställning om uppgraderingen skulle misslyckas.</span><span class="sxs-lookup"><span data-stu-id="a7eae-137">Changes toothis file can be included in full- or configuration-only upgrades of hello service, subject tooService Fabric upgrade health checks and automatic rollback if there is upgrade failure.</span></span> <span data-ttu-id="a7eae-138">Mer information finns i [uppgradering av Service Fabric-programmet](service-fabric-application-upgrade.md).</span><span class="sxs-lookup"><span data-stu-id="a7eae-138">For more information, see [Service Fabric application upgrade](service-fabric-application-upgrade.md).</span></span>

<span data-ttu-id="a7eae-139">Hej *filter* avsnittet av hello config kan toofurther anpassa hello information som är pågående toogo via hello EventFlow pipeline toohello utdata, så att du toodrop eller innehålla viss information eller ändra hello struktur för hello händelsedata.</span><span class="sxs-lookup"><span data-stu-id="a7eae-139">hello *filters* section of hello config allows you toofurther customize hello information that is going toogo through hello EventFlow pipeline toohello outputs, allowing you toodrop or include certain information, or change hello structure of hello event data.</span></span> <span data-ttu-id="a7eae-140">Mer information om filtrering finns [EventFlow filter](https://github.com/Azure/diagnostics-eventflow#filters).</span><span class="sxs-lookup"><span data-stu-id="a7eae-140">For more information on filtering, see [EventFlow filters](https://github.com/Azure/diagnostics-eventflow#filters).</span></span>

<span data-ttu-id="a7eae-141">hello sista steget är tooinstantiate EventFlow pipeline i din tjänst startkoden finns i `Program.cs` fil:</span><span class="sxs-lookup"><span data-stu-id="a7eae-141">hello final step is tooinstantiate EventFlow pipeline in your service's startup code, located in `Program.cs` file:</span></span>

```csharp
using System;
using System.Diagnostics;
using System.Threading;
using Microsoft.ServiceFabric;
using Microsoft.ServiceFabric.Services.Runtime;

// **** EventFlow namespace
using Microsoft.Diagnostics.EventFlow.ServiceFabric;

namespace Stateless1
{
    internal static class Program
    {
        /// <summary>
        /// This is hello entry point of hello service host process.
        /// </summary>
        private static void Main()
        {
            try
            {
                // **** Instantiate log collection via EventFlow
                using (var diagnosticsPipeline = ServiceFabricDiagnosticPipelineFactory.CreatePipeline("MyApplication-MyService-DiagnosticsPipeline"))
                {

                    ServiceRuntime.RegisterServiceAsync("Stateless1Type",
                    context => new Stateless1(context)).GetAwaiter().GetResult();

                    ServiceEventSource.Current.ServiceTypeRegistered(Process.GetCurrentProcess().Id, typeof(Stateless1).Name);

                    Thread.Sleep(Timeout.Infinite);
                }
            }
            catch (Exception e)
            {
                ServiceEventSource.Current.ServiceHostInitializationFailed(e.ToString());
                throw;
            }
        }
    }
}
```

<span data-ttu-id="a7eae-142">hello namn angavs som parameter. hello av hello `CreatePipeline` metod för hello `ServiceFabricDiagnosticsPipelineFactory` är hello namnet på hello *hälsa entiteten* som representerar hello EventFlow loggen samling pipeline.</span><span class="sxs-lookup"><span data-stu-id="a7eae-142">hello name passed as hello parameter of hello `CreatePipeline` method of hello `ServiceFabricDiagnosticsPipelineFactory` is hello name of hello *health entity* representing hello EventFlow log collection pipeline.</span></span> <span data-ttu-id="a7eae-143">Det här namnet används om EventFlow påträffar och felmeddelandet och rapporter via hello Service Fabric hälsa undersystemet.</span><span class="sxs-lookup"><span data-stu-id="a7eae-143">This name is used if EventFlow encounters and error and reports it through hello Service Fabric health subsystem.</span></span>

### <a name="using-service-fabric-settings-and-application-parameters-tooin-eventflowconfig"></a><span data-ttu-id="a7eae-144">Med hjälp av Service Fabric-inställningar och program parametrar tooin eventFlowConfig</span><span class="sxs-lookup"><span data-stu-id="a7eae-144">Using Service Fabric settings and application parameters tooin eventFlowConfig</span></span>

<span data-ttu-id="a7eae-145">EventFlow stöder Service Fabric och parametrarna tooconfigure EventFlow programinställningar.</span><span class="sxs-lookup"><span data-stu-id="a7eae-145">EventFlow supports using Service Fabric settings and application paremeters tooconfigure EventFlow settings.</span></span> <span data-ttu-id="a7eae-146">Du kan se tooService Fabric inställningar parametrar med den här särskilda syntaxen för värden:</span><span class="sxs-lookup"><span data-stu-id="a7eae-146">You can refer tooService Fabric settings parameters using this special syntax for values:</span></span>

```json
servicefabric:/<section-name>/<setting-name>
``` 

<span data-ttu-id="a7eae-147">`<section-name>`hello Service Fabric konfigurationsavsnittet hello namn och `<setting-name>` är hello Konfigurationsinställningen att tillhandahålla hello-värde som ska använda tooconfigure en EventFlow inställning.</span><span class="sxs-lookup"><span data-stu-id="a7eae-147">`<section-name>` is hello name of hello Service Fabric configuration section, and `<setting-name>` is hello configuration setting providing hello value that will be used tooconfigure an EventFlow setting.</span></span> <span data-ttu-id="a7eae-148">Mer om hur tooread toodo detta, går för[stöd för Service Fabric-inställningar och programparametrar](https://github.com/Azure/diagnostics-eventflow#support-for-service-fabric-settings-and-application-parameters).</span><span class="sxs-lookup"><span data-stu-id="a7eae-148">tooread more about how toodo this, go too[Support for Service Fabric settings and application parameters](https://github.com/Azure/diagnostics-eventflow#support-for-service-fabric-settings-and-application-parameters).</span></span>

## <a name="verification"></a><span data-ttu-id="a7eae-149">Verifieringen</span><span class="sxs-lookup"><span data-stu-id="a7eae-149">Verification</span></span>

<span data-ttu-id="a7eae-150">Starta tjänsten och notera hello Debug utdatafönstret i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="a7eae-150">Start your service and observe hello Debug output window in Visual Studio.</span></span> <span data-ttu-id="a7eae-151">När hello-tjänsten har startats borde du se bevis som din tjänst skickar registrerar toohello utdata som du har konfigurerat.</span><span class="sxs-lookup"><span data-stu-id="a7eae-151">After hello service is started, you should start seeing evidence that your service is sending records toohello output that you have configured.</span></span> <span data-ttu-id="a7eae-152">Navigera tooyour händelse analys och visualisering plattform och bekräfta att loggarna har startat tooshow upp (kan ta några minuter).</span><span class="sxs-lookup"><span data-stu-id="a7eae-152">Navigate tooyour event analysis and visualization platform and confirm that logs have started tooshow up (could take a few minutes).</span></span>

## <a name="next-steps"></a><span data-ttu-id="a7eae-153">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="a7eae-153">Next steps</span></span>

* [<span data-ttu-id="a7eae-154">Händelseanalys och visualisering med Application Insights</span><span class="sxs-lookup"><span data-stu-id="a7eae-154">Event Analysis and Visualization with Application Insights</span></span>](service-fabric-diagnostics-event-analysis-appinsights.md)
* [<span data-ttu-id="a7eae-155">Händelseanalys och visualisering med OMS</span><span class="sxs-lookup"><span data-stu-id="a7eae-155">Event Analysis and Visualization with OMS</span></span>](service-fabric-diagnostics-event-analysis-oms.md)
* [<span data-ttu-id="a7eae-156">EventFlow dokumentation</span><span class="sxs-lookup"><span data-stu-id="a7eae-156">EventFlow documentation</span></span>](https://github.com/Azure/diagnostics-eventflow)