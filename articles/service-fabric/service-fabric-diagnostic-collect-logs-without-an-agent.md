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
# <a name="collect-logs-directly-from-an-azure-service-fabric-service-process"></a>Samla in loggar direkt från en tjänsten Azure Service Fabric-process
## <a name="in-process-log-collection"></a>Processen Logginsamling
Samla in program loggar med [Azure Diagnostics tillägget](service-fabric-diagnostics-how-to-setup-wad.md) är ett bra alternativ för **Azure Service Fabric** tjänster om loggen källor och mål är liten, inte ändras ofta och det finns en enkel mappning mellan källorna och sina mål. Om inte ett alternativ är att tjänster för skicka loggar med deras direkt till en central plats. Den här processen kallas **processen Logginsamling** och har flera potentiella fördelar:

* *Enkel konfiguration och distribution*

    * Konfigurationen av diagnostikdata samlingen har bara en del av konfigurationen för tjänsten. Det är lätt att alltid synkronisera den ”” med resten av programmet.
    * Programspecifika eller per service configuration kan enkelt uppnås.
        * Agent-baserade Logginsamling kräver vanligtvis en separat distributionen och konfigurationen av diagnostiska agenten som är en extra administrativa uppgifter och en tänkbar orsak till fel. Det är ofta endast en instans av agenten tillåts per virtuell dator (nod) och konfigurationen av delas med alla program och tjänster som körs på noden. 

* *Flexibilitet*
   
    * Programmet kan skicka data oavsett var den ska, så länge det finns ett klientbiblioteket som stöder måldata lagringssystemet. Nya mål kan läggas till på önskat sätt.
    * Komplexa capture, filtrering och Datasammanställning regler kan implementeras.
    * Agent-baserade Logginsamling begränsas ofta av data sänkor som har stöd för agenten. Vissa agenter är extensible.

* *Åtkomst till interna programdata och kontext*
   
    * Undersystemet diagnostik körs i processen för program/tjänst kan enkelt utöka spårningar med relevant information.
    * Med agent-baserade Logginsamling måste data skickas till en agent via någon mekanism för kommunikation mellan processer, till exempel händelsespårning för Windows. Den här mekanismen kan införa ytterligare begränsningar.

Det är möjligt att kombinera och dra nytta av båda metoderna för samlingen. Faktiskt kan det vara den bästa lösningen för många program. Agent-baserade samling är en fysisk lösning för att samla in loggar som är relaterade till hela klustret och enskilda klusternoder. Det är mycket mer tillförlitligt sätt än i processen Logginsamling att diagnostisera problem med tjänsten startades och kraschar. Med många tjänster som körs i ett Service Fabric-kluster är det dessutom varje tjänst som gör en egen pågående Logginsamling som resulterar i ett stort antal utgående anslutningar från klustret. Stort antal utgående anslutningar beskatta både för det nätverk och för mål-loggen. En agent som [ **Azure Diagnostics** ](../cloud-services/cloud-services-dotnet-diagnostics.md) kan samla in data från flera tjänster och skicka alla data via några anslutningar, förbättra genomflöde. 

I den här artikeln visar vi hur du ställer in en logg i processen samlingen med [ **EventFlow öppen källkod biblioteket**](https://github.com/Azure/diagnostics-eventflow). Andra bibliotek som kan användas för samma ändamål, men EventFlow har fördelen att har utformats speciellt för pågående Logginsamling och för att stödja Service Fabric-tjänster. Vi använder [ **Azure Application Insights** ](https://azure.microsoft.com/services/application-insights/) som mål för loggen. Andra mål som [ **Händelsehubbar** ](https://azure.microsoft.com/services/event-hubs/) eller [ **Elasticsearch** ](https://www.elastic.co/products/elasticsearch) stöds också. Det är bara en fråga av installerar lämpligt NuGet-paket och konfigurerar målet i konfigurationsfilen EventFlow. Läs mer på loggen mål än Application Insights [EventFlow dokumentationen](https://github.com/Azure/diagnostics-eventflow).

## <a name="adding-eventflow-library-to-a-service-fabric-service-project"></a>Lägger till EventFlow bibliotek i ett projekt för Service Fabric-tjänsten
EventFlow binärfiler är tillgängliga som en uppsättning NuGet-paket. Om du vill lägga till EventFlow till ett Service Fabric service-projekt, högerklicka på projektet i Solution Explorer och välj ”Hantera NuGet-paket”. Gå till fliken ”Bläddra” och Sök efter ”`Diagnostics.EventFlow`”:

![EventFlow NuGet-paket i Visual Studio-NuGet-Pakethanteraren UI][1]

Tjänsten värd för EventFlow bör inkludera paket beroende på käll- och mål för programloggarna. Lägg till följande paket: 

* `Microsoft.Diagnostics.EventFlow.Inputs.EventSource` 
    * (att samla in data från EventSource tjänstklass och standard EventSources som *Microsoft ServiceFabric Services* och *Microsoft-ServiceFabric-aktörer*)
* `Microsoft.Diagnostics.EventFlow.Outputs.ApplicationInsights` 
    * (vi ska skicka loggar till en Azure Application Insights-resurs)  
* `Microsoft.Diagnostics.EventFlow.ServiceFabric` 
    * (gör det möjligt för initiering av EventFlow pipeline från Service Fabric tjänstkonfiguration och rapporterar eventuella problem med att skicka diagnostikdata som Service Fabric hälsorapporter)

> [!NOTE]
> `Microsoft.Diagnostics.EventFlow.Inputs.EventSource`paketet kräver service-projekt att rikta .NET Framework 4.6 eller senare. Kontrollera att du anger rätt Målversionen av framework i projektegenskaperna innan du installerar det här paketet. 

När alla paket som är installerade, är nästa steg att konfigurera och aktivera EventFlow i tjänsten.

## <a name="configuring-and-enabling-log-collection"></a>Konfigurerar och aktiverar Logginsamling
EventFlow pipeline, ansvarar för att skicka loggar, skapas från en specifikation som lagras i en konfigurationsfil. `Microsoft.Diagnostics.EventFlow.ServiceFabric`paketet installerar en första EventFlow konfigurationsfil under `PackageRoot\Config` lösningsmapp. Filnamnet är `eventFlowConfig.json`. Den här konfigurationsfilen måste ändras för att samla in data från standardtjänsten `EventSource` klassen och skicka data till Application Insights-tjänsten.

> [!NOTE]
> Vi förutsätter att du är bekant med **Azure Application Insights** tjänsten och att du har en Application Insights-resurs som du planerar att använda för att övervaka Service Fabric-tjänsten. Om du behöver mer information, se [skapa Application Insights-resurs](../application-insights/app-insights-create-new-resource.md).

Öppna den `eventFlowConfig.json` filen i redigeraren och ändra dess innehåll som visas nedan. Se till att ersätta ServiceEventSource namnet och Application Insights instrumentation nyckeln enligt kommentarer. 

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
> Namnet på tjänstens ServiceEventSource är värdet för egenskapen Name för den `EventSourceAttribute` tillämpas på ServiceEventSource-klass. Det har angetts i den `ServiceEventSource.cs` fil som är en del av kod. Till exempel i följande kodavsnitt i ServiceEventSource heter *mitt företag-Application1-Stateless1*:
> ```csharp
> [EventSource(Name = "MyCompany-Application1-Stateless1")]
> internal sealed class ServiceEventSource : EventSource
> {
>    // (rest of ServiceEventSource implementation)
>} 
> ```

Observera att `eventFlowConfig.json` filen är en del av tjänsten konfigurationspaket. Ändringar i den här filen kan ingå i fullständig eller configuration-endast uppgraderingar av tjänsten, omfattas Service Fabric-uppgradera hälsokontroller och automatisk återställning om uppgraderingen skulle misslyckas. Mer information finns i [uppgradering av Service Fabric-programmet](service-fabric-application-upgrade.md).

Det sista steget är att skapa en instans av EventFlow pipeline i din tjänst startkoden finns i `Program.cs` fil. I följande exempel EventFlow-relaterade tillägg markeras med kommentarer som börjar med `****`:

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

Namnet som angavs som parameter för den `CreatePipeline` metod för den `ServiceFabricDiagnosticsPipelineFactory` är namnet på den *hälsa entiteten* som representerar EventFlow loggen samling pipeline. Det här namnet används om EventFlow påträffar och felmeddelandet och rapporter via undersystemet health Service Fabric.

## <a name="verification"></a>Verifieringen
Starta tjänsten och notera att felsöka utdatafönstret i Visual Studio. När tjänsten har startats borde du se bevis att din tjänst skickar ”Application Insights Telemetry” poster. Öppna en webbläsare och gå gå till Application Insights-resurs. Öppna fliken ”Sök” (överst på standardbladet ”Overview”). Efter en kort fördröjning borde du se dina spår i Application Insights-portalen:

![Application Insights-portalen visar loggar från ett Service Fabric-program][2]

## <a name="next-steps"></a>Nästa steg
* [Lär dig mer om hur du diagnostiserar och övervakningstjänsten Service Fabric](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)
* [EventFlow dokumentation](https://github.com/Azure/diagnostics-eventflow)


<!--Image references-->
[1]: ./media/service-fabric-diagnostics-collect-logs-without-an-agent/eventflow-nugets.png
[2]: ./media/service-fabric-diagnostics-collect-logs-without-an-agent/ai-traces.png
