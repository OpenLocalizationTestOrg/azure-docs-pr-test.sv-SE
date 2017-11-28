---
title: "Samla in loggar direkt från en tjänsten Azure Service Fabric-process | Microsoft Azure"
description: "Beskriver Service Fabric-program kan skicka loggarna direkt till en central plats som Azure Application Insights eller Elasticsearch, utan agent för Azure-diagnostik."
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
ms.openlocfilehash: b7d2541928f4248750417a77d99033c8b4354dcc
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="collect-logs-directly-from-an-azure-service-fabric-service-process"></a><span data-ttu-id="48b85-103">Samla in loggar direkt från en tjänsten Azure Service Fabric-process</span><span class="sxs-lookup"><span data-stu-id="48b85-103">Collect logs directly from an Azure Service Fabric service process</span></span>
## <a name="in-process-log-collection"></a><span data-ttu-id="48b85-104">Processen Logginsamling</span><span class="sxs-lookup"><span data-stu-id="48b85-104">In-process log collection</span></span>
<span data-ttu-id="48b85-105">Samla in program loggar med [Azure Diagnostics tillägget](service-fabric-diagnostics-how-to-setup-wad.md) är ett bra alternativ för **Azure Service Fabric** tjänster om loggen källor och mål är liten, inte ändras ofta och det finns en enkel mappning mellan källorna och sina mål.</span><span class="sxs-lookup"><span data-stu-id="48b85-105">Collecting application logs using [Azure Diagnostics extension](service-fabric-diagnostics-how-to-setup-wad.md) is a good option for **Azure Service Fabric** services if the set of log sources and destinations is small, does not change often, and there is a straightforward mapping between the sources and their destinations.</span></span> <span data-ttu-id="48b85-106">Om inte ett alternativ är att tjänster för skicka loggar med deras direkt till en central plats.</span><span class="sxs-lookup"><span data-stu-id="48b85-106">If not, an alternative is to have services send their logs directly to a central location.</span></span> <span data-ttu-id="48b85-107">Den här processen kallas **processen Logginsamling** och har flera potentiella fördelar:</span><span class="sxs-lookup"><span data-stu-id="48b85-107">This process is known as **in-process log collection** and has several potential advantages:</span></span>

* <span data-ttu-id="48b85-108">*Enkel konfiguration och distribution*</span><span class="sxs-lookup"><span data-stu-id="48b85-108">*Easy configuration and deployment*</span></span>

    * <span data-ttu-id="48b85-109">Konfigurationen av diagnostikdata samlingen har bara en del av konfigurationen för tjänsten.</span><span class="sxs-lookup"><span data-stu-id="48b85-109">The configuration of diagnostic data collection is just part of the service configuration.</span></span> <span data-ttu-id="48b85-110">Det är lätt att alltid synkronisera den ”” med resten av programmet.</span><span class="sxs-lookup"><span data-stu-id="48b85-110">It is easy to always keep it "in sync" with the rest of the application.</span></span>
    * <span data-ttu-id="48b85-111">Programspecifika eller per service configuration kan enkelt uppnås.</span><span class="sxs-lookup"><span data-stu-id="48b85-111">Per-application or per-service configuration is easily achievable.</span></span>
        * <span data-ttu-id="48b85-112">Agent-baserade Logginsamling kräver vanligtvis en separat distributionen och konfigurationen av diagnostiska agenten som är en extra administrativa uppgifter och en tänkbar orsak till fel.</span><span class="sxs-lookup"><span data-stu-id="48b85-112">Agent-based log collection usually requires a separate deployment and configuration of the diagnostic agent, which is an extra administrative task and a potential source of errors.</span></span> <span data-ttu-id="48b85-113">Det är ofta endast en instans av agenten tillåts per virtuell dator (nod) och konfigurationen av delas med alla program och tjänster som körs på noden.</span><span class="sxs-lookup"><span data-stu-id="48b85-113">Often there is only one instance of the agent allowed per virtual machine (node) and the agent configuration is shared among all applications and services running on that node.</span></span> 

* <span data-ttu-id="48b85-114">*Flexibilitet*</span><span class="sxs-lookup"><span data-stu-id="48b85-114">*Flexibility*</span></span>
   
    * <span data-ttu-id="48b85-115">Programmet kan skicka data oavsett var den ska, så länge det finns ett klientbiblioteket som stöder måldata lagringssystemet.</span><span class="sxs-lookup"><span data-stu-id="48b85-115">The application can send the data wherever it needs to go, as long as there is a client library that supports the targeted data storage system.</span></span> <span data-ttu-id="48b85-116">Nya mål kan läggas till på önskat sätt.</span><span class="sxs-lookup"><span data-stu-id="48b85-116">New destinations can be added as desired.</span></span>
    * <span data-ttu-id="48b85-117">Komplexa capture, filtrering och Datasammanställning regler kan implementeras.</span><span class="sxs-lookup"><span data-stu-id="48b85-117">Complex capture, filtering, and data-aggregation rules can be implemented.</span></span>
    * <span data-ttu-id="48b85-118">Agent-baserade Logginsamling begränsas ofta av data sänkor som har stöd för agenten.</span><span class="sxs-lookup"><span data-stu-id="48b85-118">Agent-based log collection is often limited by the data sinks that the agent supports.</span></span> <span data-ttu-id="48b85-119">Vissa agenter är extensible.</span><span class="sxs-lookup"><span data-stu-id="48b85-119">Some agents are extensible.</span></span>

* <span data-ttu-id="48b85-120">*Åtkomst till interna programdata och kontext*</span><span class="sxs-lookup"><span data-stu-id="48b85-120">*Access to internal application data and context*</span></span>
   
    * <span data-ttu-id="48b85-121">Undersystemet diagnostik körs i processen för program/tjänst kan enkelt utöka spårningar med relevant information.</span><span class="sxs-lookup"><span data-stu-id="48b85-121">The diagnostic subsystem running inside the application/service process can easily augment the traces with contextual information.</span></span>
    * <span data-ttu-id="48b85-122">Med agent-baserade Logginsamling måste data skickas till en agent via någon mekanism för kommunikation mellan processer, till exempel händelsespårning för Windows.</span><span class="sxs-lookup"><span data-stu-id="48b85-122">With agent-based log collection, the data must be sent to an agent via some inter-process communication mechanism, such as Event Tracing for Windows.</span></span> <span data-ttu-id="48b85-123">Den här mekanismen kan införa ytterligare begränsningar.</span><span class="sxs-lookup"><span data-stu-id="48b85-123">This mechanism could impose additional limitations.</span></span>

<span data-ttu-id="48b85-124">Det är möjligt att kombinera och dra nytta av båda metoderna för samlingen.</span><span class="sxs-lookup"><span data-stu-id="48b85-124">It is possible to combine and benefit from both collection methods.</span></span> <span data-ttu-id="48b85-125">Faktiskt kan det vara den bästa lösningen för många program.</span><span class="sxs-lookup"><span data-stu-id="48b85-125">Indeed, it might be the best solution for many applications.</span></span> <span data-ttu-id="48b85-126">Agent-baserade samling är en fysisk lösning för att samla in loggar som är relaterade till hela klustret och enskilda klusternoder.</span><span class="sxs-lookup"><span data-stu-id="48b85-126">Agent-based collection is a natural solution for collecting logs related to the whole cluster and individual cluster nodes.</span></span> <span data-ttu-id="48b85-127">Det är mycket mer tillförlitligt sätt än i processen Logginsamling att diagnostisera problem med tjänsten startades och kraschar.</span><span class="sxs-lookup"><span data-stu-id="48b85-127">It is much more reliable way, than in-process log collection, to diagnose service startup problems and crashes.</span></span> <span data-ttu-id="48b85-128">Med många tjänster som körs i ett Service Fabric-kluster är det dessutom varje tjänst som gör en egen pågående Logginsamling som resulterar i ett stort antal utgående anslutningar från klustret.</span><span class="sxs-lookup"><span data-stu-id="48b85-128">Also, with many services running inside a Service Fabric cluster, each service doing its own in-process log collection results in numerous outgoing connections from the cluster.</span></span> <span data-ttu-id="48b85-129">Stort antal utgående anslutningar beskatta både för det nätverk och för mål-loggen.</span><span class="sxs-lookup"><span data-stu-id="48b85-129">Large number of outgoing connections is taxing both for the network subsystem and for the log destination.</span></span> <span data-ttu-id="48b85-130">En agent som [ **Azure Diagnostics** ](../cloud-services/cloud-services-dotnet-diagnostics.md) kan samla in data från flera tjänster och skicka alla data via några anslutningar, förbättra genomflöde.</span><span class="sxs-lookup"><span data-stu-id="48b85-130">An agent such as [**Azure Diagnostics**](../cloud-services/cloud-services-dotnet-diagnostics.md) can gather data from multiple services and send all data through a few connections, improving throughput.</span></span> 

<span data-ttu-id="48b85-131">I den här artikeln visar vi hur du ställer in en logg i processen samlingen med [ **EventFlow öppen källkod biblioteket**](https://github.com/Azure/diagnostics-eventflow).</span><span class="sxs-lookup"><span data-stu-id="48b85-131">In this article, we show how to set up an in-process log collection using [**EventFlow open-source library**](https://github.com/Azure/diagnostics-eventflow).</span></span> <span data-ttu-id="48b85-132">Andra bibliotek som kan användas för samma ändamål, men EventFlow har fördelen att har utformats speciellt för pågående Logginsamling och för att stödja Service Fabric-tjänster.</span><span class="sxs-lookup"><span data-stu-id="48b85-132">Other libraries might be used for the same purpose, but EventFlow has the benefit of having been designed specifically for in-process log collection and to support Service Fabric services.</span></span> <span data-ttu-id="48b85-133">Vi använder [ **Azure Application Insights** ](https://azure.microsoft.com/services/application-insights/) som mål för loggen.</span><span class="sxs-lookup"><span data-stu-id="48b85-133">We use [**Azure Application Insights**](https://azure.microsoft.com/services/application-insights/) as the log destination.</span></span> <span data-ttu-id="48b85-134">Andra mål som [ **Händelsehubbar** ](https://azure.microsoft.com/services/event-hubs/) eller [ **Elasticsearch** ](https://www.elastic.co/products/elasticsearch) stöds också.</span><span class="sxs-lookup"><span data-stu-id="48b85-134">Other destinations such as [**Event Hubs**](https://azure.microsoft.com/services/event-hubs/) or [**Elasticsearch**](https://www.elastic.co/products/elasticsearch) are also supported.</span></span> <span data-ttu-id="48b85-135">Det är bara en fråga av installerar lämpligt NuGet-paket och konfigurerar målet i konfigurationsfilen EventFlow.</span><span class="sxs-lookup"><span data-stu-id="48b85-135">It is just a question of installing appropriate NuGet package and configuring the destination in the EventFlow configuration file.</span></span> <span data-ttu-id="48b85-136">Läs mer på loggen mål än Application Insights [EventFlow dokumentationen](https://github.com/Azure/diagnostics-eventflow).</span><span class="sxs-lookup"><span data-stu-id="48b85-136">For more information on log destinations other than Application Insights, see [EventFlow documentation](https://github.com/Azure/diagnostics-eventflow).</span></span>

## <a name="adding-eventflow-library-to-a-service-fabric-service-project"></a><span data-ttu-id="48b85-137">Lägger till EventFlow bibliotek i ett projekt för Service Fabric-tjänsten</span><span class="sxs-lookup"><span data-stu-id="48b85-137">Adding EventFlow library to a Service Fabric service project</span></span>
<span data-ttu-id="48b85-138">EventFlow binärfiler är tillgängliga som en uppsättning NuGet-paket.</span><span class="sxs-lookup"><span data-stu-id="48b85-138">EventFlow binaries are available as a set of NuGet packages.</span></span> <span data-ttu-id="48b85-139">Om du vill lägga till EventFlow till ett Service Fabric service-projekt, högerklicka på projektet i Solution Explorer och välj ”Hantera NuGet-paket”.</span><span class="sxs-lookup"><span data-stu-id="48b85-139">To add EventFlow to a Service Fabric service project, right-click the project in the Solution Explorer and choose "Manage NuGet packages."</span></span> <span data-ttu-id="48b85-140">Gå till fliken ”Bläddra” och Sök efter ”`Diagnostics.EventFlow`”:</span><span class="sxs-lookup"><span data-stu-id="48b85-140">Switch to the "Browse" tab and search for "`Diagnostics.EventFlow`":</span></span>

![EventFlow NuGet-paket i Visual Studio-NuGet-Pakethanteraren UI][1]

<span data-ttu-id="48b85-142">Tjänsten värd för EventFlow bör inkludera paket beroende på käll- och mål för programloggarna.</span><span class="sxs-lookup"><span data-stu-id="48b85-142">The service hosting EventFlow should include appropriate packages depending on the source and destination for the application logs.</span></span> <span data-ttu-id="48b85-143">Lägg till följande paket:</span><span class="sxs-lookup"><span data-stu-id="48b85-143">Add the following packages:</span></span> 

* `Microsoft.Diagnostics.EventFlow.Inputs.EventSource` 
    * <span data-ttu-id="48b85-144">(att samla in data från EventSource tjänstklass och standard EventSources som *Microsoft ServiceFabric Services* och *Microsoft-ServiceFabric-aktörer*)</span><span class="sxs-lookup"><span data-stu-id="48b85-144">(to capture data from the service's EventSource class, and from standard EventSources such as *Microsoft-ServiceFabric-Services* and *Microsoft-ServiceFabric-Actors*)</span></span>
* `Microsoft.Diagnostics.EventFlow.Outputs.ApplicationInsights` 
    * <span data-ttu-id="48b85-145">(vi ska skicka loggar till en Azure Application Insights-resurs)</span><span class="sxs-lookup"><span data-stu-id="48b85-145">(we are going to send the logs to an Azure Application Insights resource)</span></span>  
* `Microsoft.Diagnostics.EventFlow.ServiceFabric` 
    * <span data-ttu-id="48b85-146">(gör det möjligt för initiering av EventFlow pipeline från Service Fabric tjänstkonfiguration och rapporterar eventuella problem med att skicka diagnostikdata som Service Fabric hälsorapporter)</span><span class="sxs-lookup"><span data-stu-id="48b85-146">(enables initialization of the EventFlow pipeline from Service Fabric service configuration and reports any problems with sending diagnostic data as Service Fabric health reports)</span></span>

> [!NOTE]
> <span data-ttu-id="48b85-147">`Microsoft.Diagnostics.EventFlow.Inputs.EventSource`paketet kräver service-projekt att rikta .NET Framework 4.6 eller senare.</span><span class="sxs-lookup"><span data-stu-id="48b85-147">`Microsoft.Diagnostics.EventFlow.Inputs.EventSource` package requires the service project to target .NET Framework 4.6 or newer.</span></span> <span data-ttu-id="48b85-148">Kontrollera att du anger rätt Målversionen av framework i projektegenskaperna innan du installerar det här paketet.</span><span class="sxs-lookup"><span data-stu-id="48b85-148">Make sure you set the appropriate target framework in project properties before installing this package.</span></span> 

<span data-ttu-id="48b85-149">När alla paket som är installerade, är nästa steg att konfigurera och aktivera EventFlow i tjänsten.</span><span class="sxs-lookup"><span data-stu-id="48b85-149">After all the packages are installed, the next step is to configure and enable EventFlow in the service.</span></span>

## <a name="configuring-and-enabling-log-collection"></a><span data-ttu-id="48b85-150">Konfigurerar och aktiverar Logginsamling</span><span class="sxs-lookup"><span data-stu-id="48b85-150">Configuring and enabling log collection</span></span>
<span data-ttu-id="48b85-151">EventFlow pipeline, ansvarar för att skicka loggar, skapas från en specifikation som lagras i en konfigurationsfil.</span><span class="sxs-lookup"><span data-stu-id="48b85-151">EventFlow pipeline, responsible for sending the logs, is created from a specification stored in a configuration file.</span></span> <span data-ttu-id="48b85-152">`Microsoft.Diagnostics.EventFlow.ServiceFabric`paketet installerar en första EventFlow konfigurationsfil under `PackageRoot\Config` lösningsmapp.</span><span class="sxs-lookup"><span data-stu-id="48b85-152">`Microsoft.Diagnostics.EventFlow.ServiceFabric` package installs a starting EventFlow configuration file under `PackageRoot\Config` solution folder.</span></span> <span data-ttu-id="48b85-153">Filnamnet är `eventFlowConfig.json`.</span><span class="sxs-lookup"><span data-stu-id="48b85-153">The file name is `eventFlowConfig.json`.</span></span> <span data-ttu-id="48b85-154">Den här konfigurationsfilen måste ändras för att samla in data från standardtjänsten `EventSource` klassen och skicka data till Application Insights-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="48b85-154">This configuration file needs to be modified to capture data from the default service `EventSource` class and send data to Application Insights service.</span></span>

> [!NOTE]
> <span data-ttu-id="48b85-155">Vi förutsätter att du är bekant med **Azure Application Insights** tjänsten och att du har en Application Insights-resurs som du planerar att använda för att övervaka Service Fabric-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="48b85-155">We assume that you are familiar with **Azure Application Insights** service and that you have an Application Insights resource that you plan to use to monitor your Service Fabric service.</span></span> <span data-ttu-id="48b85-156">Om du behöver mer information, se [skapa Application Insights-resurs](../application-insights/app-insights-create-new-resource.md).</span><span class="sxs-lookup"><span data-stu-id="48b85-156">If you need more information, please see [Create an Application Insights resource](../application-insights/app-insights-create-new-resource.md).</span></span>

<span data-ttu-id="48b85-157">Öppna den `eventFlowConfig.json` filen i redigeraren och ändra dess innehåll som visas nedan.</span><span class="sxs-lookup"><span data-stu-id="48b85-157">Open the `eventFlowConfig.json` file in the editor and change its content as shown below.</span></span> <span data-ttu-id="48b85-158">Se till att ersätta ServiceEventSource namnet och Application Insights instrumentation nyckeln enligt kommentarer.</span><span class="sxs-lookup"><span data-stu-id="48b85-158">Make sure to replace the ServiceEventSource name and Application Insights instrumentation key according to comments.</span></span> 

```json
{
  "inputs": [
    {
      "type": "EventSource",
      "sources": [
        { "providerName": "Microsoft-ServiceFabric-Services" },
        { "providerName": "Microsoft-ServiceFabric-Actors" },
        // (replace the following value with your service's ServiceEventSource name)
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
      // (replace the following value with your AI resource's instrumentation key)
      "instrumentationKey": "00000000-0000-0000-0000-000000000000"
    }
  ],
  "schemaVersion": "2016-08-11"
}
```

> [!NOTE]
> <span data-ttu-id="48b85-159">Namnet på tjänstens ServiceEventSource är värdet för egenskapen Name för den `EventSourceAttribute` tillämpas på ServiceEventSource-klass.</span><span class="sxs-lookup"><span data-stu-id="48b85-159">The name of service's ServiceEventSource is the value of the Name property of the `EventSourceAttribute` applied to the ServiceEventSource class.</span></span> <span data-ttu-id="48b85-160">Det har angetts i den `ServiceEventSource.cs` fil som är en del av kod.</span><span class="sxs-lookup"><span data-stu-id="48b85-160">It is all specified in the `ServiceEventSource.cs` file, which is part of the service code.</span></span> <span data-ttu-id="48b85-161">Till exempel i följande kodavsnitt i ServiceEventSource heter *mitt företag-Application1-Stateless1*:</span><span class="sxs-lookup"><span data-stu-id="48b85-161">For example, in the following code snippet the name of the ServiceEventSource is *MyCompany-Application1-Stateless1*:</span></span>
> ```csharp
> [EventSource(Name = "MyCompany-Application1-Stateless1")]
> internal sealed class ServiceEventSource : EventSource
> {
>    // (rest of ServiceEventSource implementation)
>} 
> ```

<span data-ttu-id="48b85-162">Observera att `eventFlowConfig.json` filen är en del av tjänsten konfigurationspaket.</span><span class="sxs-lookup"><span data-stu-id="48b85-162">Note that `eventFlowConfig.json` file is part of service configuration package.</span></span> <span data-ttu-id="48b85-163">Ändringar i den här filen kan ingå i fullständig eller configuration-endast uppgraderingar av tjänsten, omfattas Service Fabric-uppgradera hälsokontroller och automatisk återställning om uppgraderingen skulle misslyckas.</span><span class="sxs-lookup"><span data-stu-id="48b85-163">Changes to this file can be included in full- or configuration-only upgrades of the service, subject to Service Fabric upgrade health checks and automatic rollback if there is upgrade failure.</span></span> <span data-ttu-id="48b85-164">Mer information finns i [uppgradering av Service Fabric-programmet](service-fabric-application-upgrade.md).</span><span class="sxs-lookup"><span data-stu-id="48b85-164">For more information, see [Service Fabric application upgrade](service-fabric-application-upgrade.md).</span></span>

<span data-ttu-id="48b85-165">Det sista steget är att skapa en instans av EventFlow pipeline i din tjänst startkoden finns i `Program.cs` fil.</span><span class="sxs-lookup"><span data-stu-id="48b85-165">The final step is to instantiate EventFlow pipeline in your service's startup code, located in `Program.cs` file.</span></span> <span data-ttu-id="48b85-166">I följande exempel EventFlow-relaterade tillägg markeras med kommentarer som börjar med `****`:</span><span class="sxs-lookup"><span data-stu-id="48b85-166">In the following example  EventFlow-related additions are marked with comments starting with `****`:</span></span>

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
        /// This is the entry point of the service host process.
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

<span data-ttu-id="48b85-167">Namnet som angavs som parameter för den `CreatePipeline` metod för den `ServiceFabricDiagnosticsPipelineFactory` är namnet på den *hälsa entiteten* som representerar EventFlow loggen samling pipeline.</span><span class="sxs-lookup"><span data-stu-id="48b85-167">The name passed as the parameter of the `CreatePipeline` method of the `ServiceFabricDiagnosticsPipelineFactory` is the name of the *health entity* representing the EventFlow log collection pipeline.</span></span> <span data-ttu-id="48b85-168">Det här namnet används om EventFlow påträffar och felmeddelandet och rapporter via undersystemet health Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="48b85-168">This name is used if EventFlow encounters and error and reports it through the Service Fabric health subsystem.</span></span>

## <a name="verification"></a><span data-ttu-id="48b85-169">Verifieringen</span><span class="sxs-lookup"><span data-stu-id="48b85-169">Verification</span></span>
<span data-ttu-id="48b85-170">Starta tjänsten och notera att felsöka utdatafönstret i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="48b85-170">Start your service and observe the Debug output window in Visual Studio.</span></span> <span data-ttu-id="48b85-171">När tjänsten har startats borde du se bevis att din tjänst skickar ”Application Insights Telemetry” poster.</span><span class="sxs-lookup"><span data-stu-id="48b85-171">After the service is started, you should start seeing evidence that your service is sending "Application Insights Telemetry" records.</span></span> <span data-ttu-id="48b85-172">Öppna en webbläsare och gå gå till Application Insights-resurs.</span><span class="sxs-lookup"><span data-stu-id="48b85-172">Open a web browser and navigate go to your Application Insights resource.</span></span> <span data-ttu-id="48b85-173">Öppna fliken ”Sök” (överst på standardbladet ”Overview”).</span><span class="sxs-lookup"><span data-stu-id="48b85-173">Open "Search" tab (at the top of the default "Overview" blade).</span></span> <span data-ttu-id="48b85-174">Efter en kort fördröjning borde du se dina spår i Application Insights-portalen:</span><span class="sxs-lookup"><span data-stu-id="48b85-174">After a short delay you should start seeing your traces in the Application Insights portal:</span></span>

![Application Insights-portalen visar loggar från ett Service Fabric-program][2]

## <a name="next-steps"></a><span data-ttu-id="48b85-176">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="48b85-176">Next steps</span></span>
* [<span data-ttu-id="48b85-177">Lär dig mer om hur du diagnostiserar och övervakningstjänsten Service Fabric</span><span class="sxs-lookup"><span data-stu-id="48b85-177">Learn more about diagnosing and monitoring a Service Fabric service</span></span>](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)
* [<span data-ttu-id="48b85-178">EventFlow dokumentation</span><span class="sxs-lookup"><span data-stu-id="48b85-178">EventFlow documentation</span></span>](https://github.com/Azure/diagnostics-eventflow)


<!--Image references-->
[1]: ./media/service-fabric-diagnostics-collect-logs-without-an-agent/eventflow-nugets.png
[2]: ./media/service-fabric-diagnostics-collect-logs-without-an-agent/ai-traces.png
