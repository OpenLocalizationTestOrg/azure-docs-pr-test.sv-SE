---
title: "aaaCollect loggarna direkt från en Azure Service Fabric-tjänsten process | Microsoft Azure"
description: "Beskriver Service Fabric-program kan skicka loggarna direkt tooa central plats som Azure Application Insights eller Elasticsearch, utan agent för Azure-diagnostik."
services: service-fabric
documentationcenter: .net
author: karolz-ms
manager: rwike77
editor: 
ms.assetid: ab92c99b-1edd-4677-8c28-4e591d909b47
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 01/18/2017
ms.author: karolz
redirect_url: /azure/service-fabric/service-fabric-diagnostics-event-aggregation-eventflow
ms.openlocfilehash: d0681a2a6aaa76028d7cb469c31c006f24bbe954
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="collect-logs-directly-from-an-azure-service-fabric-service-process"></a><span data-ttu-id="264da-103">Samla in loggar direkt från en tjänsten Azure Service Fabric-process</span><span class="sxs-lookup"><span data-stu-id="264da-103">Collect logs directly from an Azure Service Fabric service process</span></span>
## <a name="in-process-log-collection"></a><span data-ttu-id="264da-104">Processen Logginsamling</span><span class="sxs-lookup"><span data-stu-id="264da-104">In-process log collection</span></span>
<span data-ttu-id="264da-105">Samla in program loggar med [Azure Diagnostics tillägget](service-fabric-diagnostics-how-to-setup-wad.md) är ett bra alternativ för **Azure Service Fabric** tjänster om hello uppsättning loggen källor och mål är litet, inte ändras ofta och det är ett enkelt mappning mellan hello källor och sina mål.</span><span class="sxs-lookup"><span data-stu-id="264da-105">Collecting application logs using [Azure Diagnostics extension](service-fabric-diagnostics-how-to-setup-wad.md) is a good option for **Azure Service Fabric** services if hello set of log sources and destinations is small, does not change often, and there is a straightforward mapping between hello sources and their destinations.</span></span> <span data-ttu-id="264da-106">Om inte ett alternativ är toohave services skicka loggar med deras direkt tooa central plats.</span><span class="sxs-lookup"><span data-stu-id="264da-106">If not, an alternative is toohave services send their logs directly tooa central location.</span></span> <span data-ttu-id="264da-107">Den här processen kallas **processen Logginsamling** och har flera potentiella fördelar:</span><span class="sxs-lookup"><span data-stu-id="264da-107">This process is known as **in-process log collection** and has several potential advantages:</span></span>

* <span data-ttu-id="264da-108">*Enkel konfiguration och distribution*</span><span class="sxs-lookup"><span data-stu-id="264da-108">*Easy configuration and deployment*</span></span>

    * <span data-ttu-id="264da-109">hello konfigurationen av diagnostikdata samlingen har bara en del av hello tjänstkonfigurationen.</span><span class="sxs-lookup"><span data-stu-id="264da-109">hello configuration of diagnostic data collection is just part of hello service configuration.</span></span> <span data-ttu-id="264da-110">Det är enkelt tooalways behålla det ”synkroniserad” med hello resten av programmet hello.</span><span class="sxs-lookup"><span data-stu-id="264da-110">It is easy tooalways keep it "in sync" with hello rest of hello application.</span></span>
    * <span data-ttu-id="264da-111">Programspecifika eller per service configuration kan enkelt uppnås.</span><span class="sxs-lookup"><span data-stu-id="264da-111">Per-application or per-service configuration is easily achievable.</span></span>
        * <span data-ttu-id="264da-112">Agent-baserade Logginsamling kräver vanligtvis en separat distributionen och konfigurationen av hello diagnostikagenten som är en extra administrativa uppgifter och en tänkbar orsak till fel.</span><span class="sxs-lookup"><span data-stu-id="264da-112">Agent-based log collection usually requires a separate deployment and configuration of hello diagnostic agent, which is an extra administrative task and a potential source of errors.</span></span> <span data-ttu-id="264da-113">Det är ofta endast en instans av hello agenten tillåts per virtuell dator (nod) och hello agentkonfiguration delas med alla program och tjänster som körs på noden.</span><span class="sxs-lookup"><span data-stu-id="264da-113">Often there is only one instance of hello agent allowed per virtual machine (node) and hello agent configuration is shared among all applications and services running on that node.</span></span> 

* <span data-ttu-id="264da-114">*Flexibilitet*</span><span class="sxs-lookup"><span data-stu-id="264da-114">*Flexibility*</span></span>
   
    * <span data-ttu-id="264da-115">hello-program kan skicka hello data oavsett var den måste toogo, så länge det finns ett klientbiblioteket som stöder hello riktade Datalagringssystemet.</span><span class="sxs-lookup"><span data-stu-id="264da-115">hello application can send hello data wherever it needs toogo, as long as there is a client library that supports hello targeted data storage system.</span></span> <span data-ttu-id="264da-116">Nya mål kan läggas till på önskat sätt.</span><span class="sxs-lookup"><span data-stu-id="264da-116">New destinations can be added as desired.</span></span>
    * <span data-ttu-id="264da-117">Komplexa capture, filtrering och Datasammanställning regler kan implementeras.</span><span class="sxs-lookup"><span data-stu-id="264da-117">Complex capture, filtering, and data-aggregation rules can be implemented.</span></span>
    * <span data-ttu-id="264da-118">Agent-baserade Logginsamling begränsas ofta av hello data sänkor som hello har stöd för agenten.</span><span class="sxs-lookup"><span data-stu-id="264da-118">Agent-based log collection is often limited by hello data sinks that hello agent supports.</span></span> <span data-ttu-id="264da-119">Vissa agenter är extensible.</span><span class="sxs-lookup"><span data-stu-id="264da-119">Some agents are extensible.</span></span>

* <span data-ttu-id="264da-120">*Åtkomst toointernal programdata och kontext*</span><span class="sxs-lookup"><span data-stu-id="264da-120">*Access toointernal application data and context*</span></span>
   
    * <span data-ttu-id="264da-121">hello diagnostiska undersystemet körs i processen för hello-program/tjänst kan enkelt utöka hello spårningar med relevant information.</span><span class="sxs-lookup"><span data-stu-id="264da-121">hello diagnostic subsystem running inside hello application/service process can easily augment hello traces with contextual information.</span></span>
    * <span data-ttu-id="264da-122">Med agent-baserade Logginsamling måste hello data skickas tooan agent via någon mekanism för kommunikation mellan processer, till exempel händelsespårning för Windows.</span><span class="sxs-lookup"><span data-stu-id="264da-122">With agent-based log collection, hello data must be sent tooan agent via some inter-process communication mechanism, such as Event Tracing for Windows.</span></span> <span data-ttu-id="264da-123">Den här mekanismen kan införa ytterligare begränsningar.</span><span class="sxs-lookup"><span data-stu-id="264da-123">This mechanism could impose additional limitations.</span></span>

<span data-ttu-id="264da-124">Det är möjligt toocombine och utnyttja båda metoderna för samlingen.</span><span class="sxs-lookup"><span data-stu-id="264da-124">It is possible toocombine and benefit from both collection methods.</span></span> <span data-ttu-id="264da-125">Faktiskt kanske hello bästa lösningen för många program.</span><span class="sxs-lookup"><span data-stu-id="264da-125">Indeed, it might be hello best solution for many applications.</span></span> <span data-ttu-id="264da-126">Agent-baserade samling är en fysisk lösning för att samla in loggar relaterade toohello hela klustret och enskilda klusternoder.</span><span class="sxs-lookup"><span data-stu-id="264da-126">Agent-based collection is a natural solution for collecting logs related toohello whole cluster and individual cluster nodes.</span></span> <span data-ttu-id="264da-127">Det är mycket mer tillförlitligt sätt än i processen Logginsamling, toodiagnose service startproblem och krascher.</span><span class="sxs-lookup"><span data-stu-id="264da-127">It is much more reliable way, than in-process log collection, toodiagnose service startup problems and crashes.</span></span> <span data-ttu-id="264da-128">Med många tjänster som körs i ett Service Fabric-kluster är det också varje tjänst som gör en egen pågående Logginsamling som resulterar i ett stort antal utgående anslutningar från hello kluster.</span><span class="sxs-lookup"><span data-stu-id="264da-128">Also, with many services running inside a Service Fabric cluster, each service doing its own in-process log collection results in numerous outgoing connections from hello cluster.</span></span> <span data-ttu-id="264da-129">Stort antal utgående anslutningar beskatta både för hello nätverkets undersystem och hello loggmålet.</span><span class="sxs-lookup"><span data-stu-id="264da-129">Large number of outgoing connections is taxing both for hello network subsystem and for hello log destination.</span></span> <span data-ttu-id="264da-130">En agent som [ **Azure Diagnostics** ](../cloud-services/cloud-services-dotnet-diagnostics.md) kan samla in data från flera tjänster och skicka alla data via några anslutningar, förbättra genomflöde.</span><span class="sxs-lookup"><span data-stu-id="264da-130">An agent such as [**Azure Diagnostics**](../cloud-services/cloud-services-dotnet-diagnostics.md) can gather data from multiple services and send all data through a few connections, improving throughput.</span></span> 

<span data-ttu-id="264da-131">I den här artikeln visar vi hur tooset upp en pågående logga samlingen med [ **EventFlow öppen källkod biblioteket**](https://github.com/Azure/diagnostics-eventflow).</span><span class="sxs-lookup"><span data-stu-id="264da-131">In this article, we show how tooset up an in-process log collection using [**EventFlow open-source library**](https://github.com/Azure/diagnostics-eventflow).</span></span> <span data-ttu-id="264da-132">Andra bibliotek som kan användas för hello samma syfte, men EventFlow har hello fördelen med har utformats speciellt för pågående loggen samlingen och toosupport Service Fabric-tjänster.</span><span class="sxs-lookup"><span data-stu-id="264da-132">Other libraries might be used for hello same purpose, but EventFlow has hello benefit of having been designed specifically for in-process log collection and toosupport Service Fabric services.</span></span> <span data-ttu-id="264da-133">Vi använder [ **Azure Application Insights** ](https://azure.microsoft.com/services/application-insights/) som hello loggmålet.</span><span class="sxs-lookup"><span data-stu-id="264da-133">We use [**Azure Application Insights**](https://azure.microsoft.com/services/application-insights/) as hello log destination.</span></span> <span data-ttu-id="264da-134">Andra mål som [ **Händelsehubbar** ](https://azure.microsoft.com/services/event-hubs/) eller [ **Elasticsearch** ](https://www.elastic.co/products/elasticsearch) stöds också.</span><span class="sxs-lookup"><span data-stu-id="264da-134">Other destinations such as [**Event Hubs**](https://azure.microsoft.com/services/event-hubs/) or [**Elasticsearch**](https://www.elastic.co/products/elasticsearch) are also supported.</span></span> <span data-ttu-id="264da-135">Det är bara en fråga av installerar lämpligt NuGet-paket och konfigurerar hello mål i konfigurationsfilen för hello EventFlow.</span><span class="sxs-lookup"><span data-stu-id="264da-135">It is just a question of installing appropriate NuGet package and configuring hello destination in hello EventFlow configuration file.</span></span> <span data-ttu-id="264da-136">Läs mer på loggen mål än Application Insights [EventFlow dokumentationen](https://github.com/Azure/diagnostics-eventflow).</span><span class="sxs-lookup"><span data-stu-id="264da-136">For more information on log destinations other than Application Insights, see [EventFlow documentation](https://github.com/Azure/diagnostics-eventflow).</span></span>

## <a name="adding-eventflow-library-tooa-service-fabric-service-project"></a><span data-ttu-id="264da-137">Lägga till EventFlow biblioteket tooa Service Fabric service-projekt</span><span class="sxs-lookup"><span data-stu-id="264da-137">Adding EventFlow library tooa Service Fabric service project</span></span>
<span data-ttu-id="264da-138">EventFlow binärfiler är tillgängliga som en uppsättning NuGet-paket.</span><span class="sxs-lookup"><span data-stu-id="264da-138">EventFlow binaries are available as a set of NuGet packages.</span></span> <span data-ttu-id="264da-139">tooadd EventFlow tooa Service Fabric service-projekt, högerklicka på hello-projekt i hello Solution Explorer och välj ”Hantera NuGet-paket”.</span><span class="sxs-lookup"><span data-stu-id="264da-139">tooadd EventFlow tooa Service Fabric service project, right-click hello project in hello Solution Explorer and choose "Manage NuGet packages."</span></span> <span data-ttu-id="264da-140">Växla toohello ”Bläddra” fliken och Sök efter ”`Diagnostics.EventFlow`”:</span><span class="sxs-lookup"><span data-stu-id="264da-140">Switch toohello "Browse" tab and search for "`Diagnostics.EventFlow`":</span></span>

![EventFlow NuGet-paket i Visual Studio-NuGet-Pakethanteraren UI][1]

<span data-ttu-id="264da-142">hello-tjänst som är värd EventFlow bör inkludera paket beroende på hello källa och mål för hello programloggar.</span><span class="sxs-lookup"><span data-stu-id="264da-142">hello service hosting EventFlow should include appropriate packages depending on hello source and destination for hello application logs.</span></span> <span data-ttu-id="264da-143">Lägg till hello följande paket:</span><span class="sxs-lookup"><span data-stu-id="264da-143">Add hello following packages:</span></span> 

* `Microsoft.Diagnostics.EventFlow.Inputs.EventSource` 
    * <span data-ttu-id="264da-144">(toocapture data från hello-EventSource tjänstklassen och standard EventSources som *Microsoft ServiceFabric Services* och *Microsoft-ServiceFabric-aktörer*)</span><span class="sxs-lookup"><span data-stu-id="264da-144">(toocapture data from hello service's EventSource class, and from standard EventSources such as *Microsoft-ServiceFabric-Services* and *Microsoft-ServiceFabric-Actors*)</span></span>
* `Microsoft.Diagnostics.EventFlow.Outputs.ApplicationInsights` 
    * <span data-ttu-id="264da-145">(vi toosend hello loggar tooan Azure Application Insights-resurs)</span><span class="sxs-lookup"><span data-stu-id="264da-145">(we are going toosend hello logs tooan Azure Application Insights resource)</span></span>  
* `Microsoft.Diagnostics.EventFlow.ServiceFabric` 
    * <span data-ttu-id="264da-146">(aktiverar initieringen av hello EventFlow pipeline från Service Fabric tjänstkonfiguration och rapporterar eventuella problem med att skicka diagnostikdata som Service Fabric hälsorapporter)</span><span class="sxs-lookup"><span data-stu-id="264da-146">(enables initialization of hello EventFlow pipeline from Service Fabric service configuration and reports any problems with sending diagnostic data as Service Fabric health reports)</span></span>

> [!NOTE]
> <span data-ttu-id="264da-147">`Microsoft.Diagnostics.EventFlow.Inputs.EventSource`paketet kräver hello service-projekt tootarget .NET Framework 4.6 eller senare.</span><span class="sxs-lookup"><span data-stu-id="264da-147">`Microsoft.Diagnostics.EventFlow.Inputs.EventSource` package requires hello service project tootarget .NET Framework 4.6 or newer.</span></span> <span data-ttu-id="264da-148">Se till att ange hello lämpliga Målversionen av framework i projektegenskaperna innan du installerar det här paketet.</span><span class="sxs-lookup"><span data-stu-id="264da-148">Make sure you set hello appropriate target framework in project properties before installing this package.</span></span> 

<span data-ttu-id="264da-149">När alla hello paket som är installerade, hello nästa steg är tooconfigure och aktivera EventFlow i hello-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="264da-149">After all hello packages are installed, hello next step is tooconfigure and enable EventFlow in hello service.</span></span>

## <a name="configuring-and-enabling-log-collection"></a><span data-ttu-id="264da-150">Konfigurerar och aktiverar Logginsamling</span><span class="sxs-lookup"><span data-stu-id="264da-150">Configuring and enabling log collection</span></span>
<span data-ttu-id="264da-151">EventFlow pipeline, ansvarar för att skicka hello loggar har skapats från en specifikation som lagras i en konfigurationsfil.</span><span class="sxs-lookup"><span data-stu-id="264da-151">EventFlow pipeline, responsible for sending hello logs, is created from a specification stored in a configuration file.</span></span> <span data-ttu-id="264da-152">`Microsoft.Diagnostics.EventFlow.ServiceFabric`paketet installerar en första EventFlow konfigurationsfil under `PackageRoot\Config` lösningsmapp.</span><span class="sxs-lookup"><span data-stu-id="264da-152">`Microsoft.Diagnostics.EventFlow.ServiceFabric` package installs a starting EventFlow configuration file under `PackageRoot\Config` solution folder.</span></span> <span data-ttu-id="264da-153">hello-filnamnet är `eventFlowConfig.json`.</span><span class="sxs-lookup"><span data-stu-id="264da-153">hello file name is `eventFlowConfig.json`.</span></span> <span data-ttu-id="264da-154">Den här konfigurationsfilen måste toobe ändrade toocapture data från hello standardtjänsten `EventSource` klassen och skicka data tooApplication Insights-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="264da-154">This configuration file needs toobe modified toocapture data from hello default service `EventSource` class and send data tooApplication Insights service.</span></span>

> [!NOTE]
> <span data-ttu-id="264da-155">Vi förutsätter att du är bekant med **Azure Application Insights** tjänsten och att du har en Application Insights-resurs som du planerar toouse toomonitor Service Fabric-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="264da-155">We assume that you are familiar with **Azure Application Insights** service and that you have an Application Insights resource that you plan toouse toomonitor your Service Fabric service.</span></span> <span data-ttu-id="264da-156">Om du behöver mer information, se [skapa Application Insights-resurs](../application-insights/app-insights-create-new-resource.md).</span><span class="sxs-lookup"><span data-stu-id="264da-156">If you need more information, please see [Create an Application Insights resource](../application-insights/app-insights-create-new-resource.md).</span></span>

<span data-ttu-id="264da-157">Öppna hello `eventFlowConfig.json` filen i hello-redigeraren och ändra dess innehåll som visas nedan.</span><span class="sxs-lookup"><span data-stu-id="264da-157">Open hello `eventFlowConfig.json` file in hello editor and change its content as shown below.</span></span> <span data-ttu-id="264da-158">Se till att tooreplace hello ServiceEventSource namn och Application Insights instrumentation nyckel enligt toocomments.</span><span class="sxs-lookup"><span data-stu-id="264da-158">Make sure tooreplace hello ServiceEventSource name and Application Insights instrumentation key according toocomments.</span></span> 

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

> [!NOTE]
> <span data-ttu-id="264da-159">hello tjänstens ServiceEventSource heter hello värdet för hello namnegenskapen i hello `EventSourceAttribute` tillämpas toohello ServiceEventSource klass.</span><span class="sxs-lookup"><span data-stu-id="264da-159">hello name of service's ServiceEventSource is hello value of hello Name property of hello `EventSourceAttribute` applied toohello ServiceEventSource class.</span></span> <span data-ttu-id="264da-160">Det har angetts i hello `ServiceEventSource.cs` fil som är en del av hello service-kod.</span><span class="sxs-lookup"><span data-stu-id="264da-160">It is all specified in hello `ServiceEventSource.cs` file, which is part of hello service code.</span></span> <span data-ttu-id="264da-161">Till exempel i hello följande kodfragment hello hello ServiceEventSource heter *mitt företag-Application1-Stateless1*:</span><span class="sxs-lookup"><span data-stu-id="264da-161">For example, in hello following code snippet hello name of hello ServiceEventSource is *MyCompany-Application1-Stateless1*:</span></span>
> ```csharp
> [EventSource(Name = "MyCompany-Application1-Stateless1")]
> internal sealed class ServiceEventSource : EventSource
> {
>    // (rest of ServiceEventSource implementation)
>} 
> ```

<span data-ttu-id="264da-162">Observera att `eventFlowConfig.json` filen är en del av tjänsten konfigurationspaket.</span><span class="sxs-lookup"><span data-stu-id="264da-162">Note that `eventFlowConfig.json` file is part of service configuration package.</span></span> <span data-ttu-id="264da-163">Ändringar toothis filen kan ingå i fullständig eller configuration-endast uppgraderingar av hello service, ämne tooService Fabric uppgradera hälsokontroller och automatisk återställning om uppgraderingen skulle misslyckas.</span><span class="sxs-lookup"><span data-stu-id="264da-163">Changes toothis file can be included in full- or configuration-only upgrades of hello service, subject tooService Fabric upgrade health checks and automatic rollback if there is upgrade failure.</span></span> <span data-ttu-id="264da-164">Mer information finns i [uppgradering av Service Fabric-programmet](service-fabric-application-upgrade.md).</span><span class="sxs-lookup"><span data-stu-id="264da-164">For more information, see [Service Fabric application upgrade](service-fabric-application-upgrade.md).</span></span>

<span data-ttu-id="264da-165">hello sista steget är tooinstantiate EventFlow pipeline i din tjänst startkoden finns i `Program.cs` fil.</span><span class="sxs-lookup"><span data-stu-id="264da-165">hello final step is tooinstantiate EventFlow pipeline in your service's startup code, located in `Program.cs` file.</span></span> <span data-ttu-id="264da-166">Följande exempel EventFlow-relaterade tillägg markeras med kommentarer från och med i hello `****`:</span><span class="sxs-lookup"><span data-stu-id="264da-166">In hello following example  EventFlow-related additions are marked with comments starting with `****`:</span></span>

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

<span data-ttu-id="264da-167">hello namn angavs som parameter. hello av hello `CreatePipeline` metod för hello `ServiceFabricDiagnosticsPipelineFactory` är hello namnet på hello *hälsa entiteten* som representerar hello EventFlow loggen samling pipeline.</span><span class="sxs-lookup"><span data-stu-id="264da-167">hello name passed as hello parameter of hello `CreatePipeline` method of hello `ServiceFabricDiagnosticsPipelineFactory` is hello name of hello *health entity* representing hello EventFlow log collection pipeline.</span></span> <span data-ttu-id="264da-168">Det här namnet används om EventFlow påträffar och felmeddelandet och rapporter via hello Service Fabric hälsa undersystemet.</span><span class="sxs-lookup"><span data-stu-id="264da-168">This name is used if EventFlow encounters and error and reports it through hello Service Fabric health subsystem.</span></span>

## <a name="verification"></a><span data-ttu-id="264da-169">Verifieringen</span><span class="sxs-lookup"><span data-stu-id="264da-169">Verification</span></span>
<span data-ttu-id="264da-170">Starta tjänsten och notera hello Debug utdatafönstret i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="264da-170">Start your service and observe hello Debug output window in Visual Studio.</span></span> <span data-ttu-id="264da-171">När hello-tjänsten har startats borde du se bevis att din tjänst skickar ”Application Insights Telemetry” poster.</span><span class="sxs-lookup"><span data-stu-id="264da-171">After hello service is started, you should start seeing evidence that your service is sending "Application Insights Telemetry" records.</span></span> <span data-ttu-id="264da-172">Öppna en webbläsare och gå gå tooyour Application Insights-resurs.</span><span class="sxs-lookup"><span data-stu-id="264da-172">Open a web browser and navigate go tooyour Application Insights resource.</span></span> <span data-ttu-id="264da-173">Öppna fliken ”Sök” (överst hello i hello standardbladet ”Overview”).</span><span class="sxs-lookup"><span data-stu-id="264da-173">Open "Search" tab (at hello top of hello default "Overview" blade).</span></span> <span data-ttu-id="264da-174">Efter en kort fördröjning borde du se dina spår i hello Application Insights-portalen:</span><span class="sxs-lookup"><span data-stu-id="264da-174">After a short delay you should start seeing your traces in hello Application Insights portal:</span></span>

![Application Insights-portalen visar loggar från ett Service Fabric-program][2]

## <a name="next-steps"></a><span data-ttu-id="264da-176">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="264da-176">Next steps</span></span>
* [<span data-ttu-id="264da-177">Lär dig mer om hur du diagnostiserar och övervakningstjänsten Service Fabric</span><span class="sxs-lookup"><span data-stu-id="264da-177">Learn more about diagnosing and monitoring a Service Fabric service</span></span>](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)
* [<span data-ttu-id="264da-178">EventFlow dokumentation</span><span class="sxs-lookup"><span data-stu-id="264da-178">EventFlow documentation</span></span>](https://github.com/Azure/diagnostics-eventflow)


<!--Image references-->
[1]: ./media/service-fabric-diagnostics-collect-logs-without-an-agent/eventflow-nugets.png
[2]: ./media/service-fabric-diagnostics-collect-logs-without-an-agent/ai-traces.png
