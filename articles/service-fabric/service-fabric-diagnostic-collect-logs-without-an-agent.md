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
# <a name="collect-logs-directly-from-an-azure-service-fabric-service-process"></a>Samla in loggar direkt från en tjänsten Azure Service Fabric-process
## <a name="in-process-log-collection"></a>Processen Logginsamling
Samla in program loggar med [Azure Diagnostics tillägget](service-fabric-diagnostics-how-to-setup-wad.md) är ett bra alternativ för **Azure Service Fabric** tjänster om hello uppsättning loggen källor och mål är litet, inte ändras ofta och det är ett enkelt mappning mellan hello källor och sina mål. Om inte ett alternativ är toohave services skicka loggar med deras direkt tooa central plats. Den här processen kallas **processen Logginsamling** och har flera potentiella fördelar:

* *Enkel konfiguration och distribution*

    * hello konfigurationen av diagnostikdata samlingen har bara en del av hello tjänstkonfigurationen. Det är enkelt tooalways behålla det ”synkroniserad” med hello resten av programmet hello.
    * Programspecifika eller per service configuration kan enkelt uppnås.
        * Agent-baserade Logginsamling kräver vanligtvis en separat distributionen och konfigurationen av hello diagnostikagenten som är en extra administrativa uppgifter och en tänkbar orsak till fel. Det är ofta endast en instans av hello agenten tillåts per virtuell dator (nod) och hello agentkonfiguration delas med alla program och tjänster som körs på noden. 

* *Flexibilitet*
   
    * hello-program kan skicka hello data oavsett var den måste toogo, så länge det finns ett klientbiblioteket som stöder hello riktade Datalagringssystemet. Nya mål kan läggas till på önskat sätt.
    * Komplexa capture, filtrering och Datasammanställning regler kan implementeras.
    * Agent-baserade Logginsamling begränsas ofta av hello data sänkor som hello har stöd för agenten. Vissa agenter är extensible.

* *Åtkomst toointernal programdata och kontext*
   
    * hello diagnostiska undersystemet körs i processen för hello-program/tjänst kan enkelt utöka hello spårningar med relevant information.
    * Med agent-baserade Logginsamling måste hello data skickas tooan agent via någon mekanism för kommunikation mellan processer, till exempel händelsespårning för Windows. Den här mekanismen kan införa ytterligare begränsningar.

Det är möjligt toocombine och utnyttja båda metoderna för samlingen. Faktiskt kanske hello bästa lösningen för många program. Agent-baserade samling är en fysisk lösning för att samla in loggar relaterade toohello hela klustret och enskilda klusternoder. Det är mycket mer tillförlitligt sätt än i processen Logginsamling, toodiagnose service startproblem och krascher. Med många tjänster som körs i ett Service Fabric-kluster är det också varje tjänst som gör en egen pågående Logginsamling som resulterar i ett stort antal utgående anslutningar från hello kluster. Stort antal utgående anslutningar beskatta både för hello nätverkets undersystem och hello loggmålet. En agent som [ **Azure Diagnostics** ](../cloud-services/cloud-services-dotnet-diagnostics.md) kan samla in data från flera tjänster och skicka alla data via några anslutningar, förbättra genomflöde. 

I den här artikeln visar vi hur tooset upp en pågående logga samlingen med [ **EventFlow öppen källkod biblioteket**](https://github.com/Azure/diagnostics-eventflow). Andra bibliotek som kan användas för hello samma syfte, men EventFlow har hello fördelen med har utformats speciellt för pågående loggen samlingen och toosupport Service Fabric-tjänster. Vi använder [ **Azure Application Insights** ](https://azure.microsoft.com/services/application-insights/) som hello loggmålet. Andra mål som [ **Händelsehubbar** ](https://azure.microsoft.com/services/event-hubs/) eller [ **Elasticsearch** ](https://www.elastic.co/products/elasticsearch) stöds också. Det är bara en fråga av installerar lämpligt NuGet-paket och konfigurerar hello mål i konfigurationsfilen för hello EventFlow. Läs mer på loggen mål än Application Insights [EventFlow dokumentationen](https://github.com/Azure/diagnostics-eventflow).

## <a name="adding-eventflow-library-tooa-service-fabric-service-project"></a>Lägga till EventFlow biblioteket tooa Service Fabric service-projekt
EventFlow binärfiler är tillgängliga som en uppsättning NuGet-paket. tooadd EventFlow tooa Service Fabric service-projekt, högerklicka på hello-projekt i hello Solution Explorer och välj ”Hantera NuGet-paket”. Växla toohello ”Bläddra” fliken och Sök efter ”`Diagnostics.EventFlow`”:

![EventFlow NuGet-paket i Visual Studio-NuGet-Pakethanteraren UI][1]

hello-tjänst som är värd EventFlow bör inkludera paket beroende på hello källa och mål för hello programloggar. Lägg till hello följande paket: 

* `Microsoft.Diagnostics.EventFlow.Inputs.EventSource` 
    * (toocapture data från hello-EventSource tjänstklassen och standard EventSources som *Microsoft ServiceFabric Services* och *Microsoft-ServiceFabric-aktörer*)
* `Microsoft.Diagnostics.EventFlow.Outputs.ApplicationInsights` 
    * (vi toosend hello loggar tooan Azure Application Insights-resurs)  
* `Microsoft.Diagnostics.EventFlow.ServiceFabric` 
    * (aktiverar initieringen av hello EventFlow pipeline från Service Fabric tjänstkonfiguration och rapporterar eventuella problem med att skicka diagnostikdata som Service Fabric hälsorapporter)

> [!NOTE]
> `Microsoft.Diagnostics.EventFlow.Inputs.EventSource`paketet kräver hello service-projekt tootarget .NET Framework 4.6 eller senare. Se till att ange hello lämpliga Målversionen av framework i projektegenskaperna innan du installerar det här paketet. 

När alla hello paket som är installerade, hello nästa steg är tooconfigure och aktivera EventFlow i hello-tjänsten.

## <a name="configuring-and-enabling-log-collection"></a>Konfigurerar och aktiverar Logginsamling
EventFlow pipeline, ansvarar för att skicka hello loggar har skapats från en specifikation som lagras i en konfigurationsfil. `Microsoft.Diagnostics.EventFlow.ServiceFabric`paketet installerar en första EventFlow konfigurationsfil under `PackageRoot\Config` lösningsmapp. hello-filnamnet är `eventFlowConfig.json`. Den här konfigurationsfilen måste toobe ändrade toocapture data från hello standardtjänsten `EventSource` klassen och skicka data tooApplication Insights-tjänsten.

> [!NOTE]
> Vi förutsätter att du är bekant med **Azure Application Insights** tjänsten och att du har en Application Insights-resurs som du planerar toouse toomonitor Service Fabric-tjänsten. Om du behöver mer information, se [skapa Application Insights-resurs](../application-insights/app-insights-create-new-resource.md).

Öppna hello `eventFlowConfig.json` filen i hello-redigeraren och ändra dess innehåll som visas nedan. Se till att tooreplace hello ServiceEventSource namn och Application Insights instrumentation nyckel enligt toocomments. 

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
> hello tjänstens ServiceEventSource heter hello värdet för hello namnegenskapen i hello `EventSourceAttribute` tillämpas toohello ServiceEventSource klass. Det har angetts i hello `ServiceEventSource.cs` fil som är en del av hello service-kod. Till exempel i hello följande kodfragment hello hello ServiceEventSource heter *mitt företag-Application1-Stateless1*:
> ```csharp
> [EventSource(Name = "MyCompany-Application1-Stateless1")]
> internal sealed class ServiceEventSource : EventSource
> {
>    // (rest of ServiceEventSource implementation)
>} 
> ```

Observera att `eventFlowConfig.json` filen är en del av tjänsten konfigurationspaket. Ändringar toothis filen kan ingå i fullständig eller configuration-endast uppgraderingar av hello service, ämne tooService Fabric uppgradera hälsokontroller och automatisk återställning om uppgraderingen skulle misslyckas. Mer information finns i [uppgradering av Service Fabric-programmet](service-fabric-application-upgrade.md).

hello sista steget är tooinstantiate EventFlow pipeline i din tjänst startkoden finns i `Program.cs` fil. Följande exempel EventFlow-relaterade tillägg markeras med kommentarer från och med i hello `****`:

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

hello namn angavs som parameter. hello av hello `CreatePipeline` metod för hello `ServiceFabricDiagnosticsPipelineFactory` är hello namnet på hello *hälsa entiteten* som representerar hello EventFlow loggen samling pipeline. Det här namnet används om EventFlow påträffar och felmeddelandet och rapporter via hello Service Fabric hälsa undersystemet.

## <a name="verification"></a>Verifieringen
Starta tjänsten och notera hello Debug utdatafönstret i Visual Studio. När hello-tjänsten har startats borde du se bevis att din tjänst skickar ”Application Insights Telemetry” poster. Öppna en webbläsare och gå gå tooyour Application Insights-resurs. Öppna fliken ”Sök” (överst hello i hello standardbladet ”Overview”). Efter en kort fördröjning borde du se dina spår i hello Application Insights-portalen:

![Application Insights-portalen visar loggar från ett Service Fabric-program][2]

## <a name="next-steps"></a>Nästa steg
* [Lär dig mer om hur du diagnostiserar och övervakningstjänsten Service Fabric](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)
* [EventFlow dokumentation](https://github.com/Azure/diagnostics-eventflow)


<!--Image references-->
[1]: ./media/service-fabric-diagnostics-collect-logs-without-an-agent/eventflow-nugets.png
[2]: ./media/service-fabric-diagnostics-collect-logs-without-an-agent/ai-traces.png
