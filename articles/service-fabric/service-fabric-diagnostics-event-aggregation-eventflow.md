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
# <a name="event-aggregation-and-collection-using-eventflow"></a>Aggregering av händelse och med EventFlow

[Microsoft Diagnostics EventFlow](https://github.com/Azure/diagnostics-eventflow) kan vidarebefordra händelser från en nod tooone eller mer övervakning mål. Eftersom den ingår som en NuGet-paket i projektet service färdas EventFlow koden och konfigurationen med hello-tjänsten, vilket eliminerar hello per nod konfigurationsproblem som tidigare nämnts om Azure-diagnostik. EventFlow körs inom din tjänstprocessen och ansluter direkt toohello konfigurerats utdata. På grund av hello direkt anslutning fungerar EventFlow för Azure-behållaren och lokala distributioner. Var försiktig om du kör EventFlow i scenarier med hög densitet som i en behållare, eftersom varje EventFlow pipeline gör en extern anslutning. Så om du har flera processer får du flera utgående anslutningar! Detta är inte lika mycket problem för Service Fabric-program, eftersom alla repliker av en `ServiceType` kör samma process i hello och det begränsar hello antal utgående anslutningar. EventFlow erbjuder också händelsefiltrering så att endast hello-händelser som matchar angivna hello-filtret skickas.

## <a name="setting-up-eventflow"></a>Konfigurera EventFlow

EventFlow binärfiler är tillgängliga som en uppsättning NuGet-paket. tooadd EventFlow tooa Service Fabric service-projekt, högerklicka på hello-projekt i hello Solution Explorer och välj ”Hantera NuGet-paket”. Växla toohello ”Bläddra” fliken och Sök efter ”`Diagnostics.EventFlow`”:

![EventFlow NuGet-paket i Visual Studio-NuGet-Pakethanteraren UI](./media/service-fabric-diagnostics-event-aggregation-eventflow/eventflow-nuget.png)

En lista över olika paket visas, märkt med ”anger” och ”utdata” visas. EventFlow stöder olika providrar för olika loggning och analyzers. hello-tjänst som är värd EventFlow bör inkludera paket beroende på hello källa och mål för hello programloggar. Dessutom toohello core ServiceFabric paketet, behöver du också minst ett indata- och konfigurerats. Exmaple, kan du lägga till följande paket toosent EventSource händelser tooApplication insikter hello:

* `Microsoft.Diagnostics.EventFlow.Input.EventSource`toocapture data från hello-EventSource tjänstklassen och standard EventSources som *Microsoft ServiceFabric Services* och *Microsoft-ServiceFabric-aktörer*)
* `Microsoft.Diagnostics.EventFlow.Output.ApplicationInsights`(vi toosend hello loggar tooan Azure Application Insights-resurs)
* `Microsoft.Diagnostics.EventFlow.ServiceFabric`(aktiverar initieringen av hello EventFlow pipeline från Service Fabric tjänstkonfiguration och rapporterar eventuella problem med att skicka diagnostikdata som Service Fabric hälsorapporter)

>[!NOTE]
>`Microsoft.Diagnostics.EventFlow.Input.EventSource`paketet kräver hello service-projekt tootarget .NET Framework 4.6 eller senare. Se till att ange hello lämpliga Målversionen av framework i projektegenskaperna innan du installerar det här paketet.

När alla hello paket som är installerade, hello nästa steg är tooconfigure och aktivera EventFlow i hello-tjänsten.

## <a name="configuring-and-enabling-log-collection"></a>Konfigurerar och aktiverar Logginsamling
Hej EventFlow pipeline ansvarar för att skicka hello loggar har skapats från en specifikation som lagras i en konfigurationsfil. Hej `Microsoft.Diagnostics.EventFlow.ServiceFabric` paketet installerar en första EventFlow konfigurationsfil under `PackageRoot\Config` lösningsmapp som heter `eventFlowConfig.json`. Den här konfigurationsfilen måste toobe ändrade toocapture data från hello standardtjänsten `EventSource` klassen och alla andra indata som du vill tooconfigure och skicka data toohello rätt plats.

Här är ett exempel *eventFlowConfig.json* baserat på hello NuGet-paket som nämns ovan:
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

hello tjänstens ServiceEventSource heter hello värdet för hello namnegenskapen i hello `EventSourceAttribute` tillämpas toohello ServiceEventSource klass. Det har angetts i hello `ServiceEventSource.cs` fil som är en del av hello service-kod. Till exempel i hello följande kodfragment hello hello ServiceEventSource heter *mitt företag-Application1-Stateless1*:

```csharp
[EventSource(Name = "MyCompany-Application1-Stateless1")]
internal sealed class ServiceEventSource : EventSource
{
    // (rest of ServiceEventSource implementation)
}
```

Observera att `eventFlowConfig.json` filen är en del av tjänsten konfigurationspaket. Ändringar toothis filen kan ingå i fullständig eller configuration-endast uppgraderingar av hello service, ämne tooService Fabric uppgradera hälsokontroller och automatisk återställning om uppgraderingen skulle misslyckas. Mer information finns i [uppgradering av Service Fabric-programmet](service-fabric-application-upgrade.md).

Hej *filter* avsnittet av hello config kan toofurther anpassa hello information som är pågående toogo via hello EventFlow pipeline toohello utdata, så att du toodrop eller innehålla viss information eller ändra hello struktur för hello händelsedata. Mer information om filtrering finns [EventFlow filter](https://github.com/Azure/diagnostics-eventflow#filters).

hello sista steget är tooinstantiate EventFlow pipeline i din tjänst startkoden finns i `Program.cs` fil:

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

### <a name="using-service-fabric-settings-and-application-parameters-tooin-eventflowconfig"></a>Med hjälp av Service Fabric-inställningar och program parametrar tooin eventFlowConfig

EventFlow stöder Service Fabric och parametrarna tooconfigure EventFlow programinställningar. Du kan se tooService Fabric inställningar parametrar med den här särskilda syntaxen för värden:

```json
servicefabric:/<section-name>/<setting-name>
``` 

`<section-name>`hello Service Fabric konfigurationsavsnittet hello namn och `<setting-name>` är hello Konfigurationsinställningen att tillhandahålla hello-värde som ska använda tooconfigure en EventFlow inställning. Mer om hur tooread toodo detta, går för[stöd för Service Fabric-inställningar och programparametrar](https://github.com/Azure/diagnostics-eventflow#support-for-service-fabric-settings-and-application-parameters).

## <a name="verification"></a>Verifieringen

Starta tjänsten och notera hello Debug utdatafönstret i Visual Studio. När hello-tjänsten har startats borde du se bevis som din tjänst skickar registrerar toohello utdata som du har konfigurerat. Navigera tooyour händelse analys och visualisering plattform och bekräfta att loggarna har startat tooshow upp (kan ta några minuter).

## <a name="next-steps"></a>Nästa steg

* [Händelseanalys och visualisering med Application Insights](service-fabric-diagnostics-event-analysis-appinsights.md)
* [Händelseanalys och visualisering med OMS](service-fabric-diagnostics-event-analysis-oms.md)
* [EventFlow dokumentation](https://github.com/Azure/diagnostics-eventflow)