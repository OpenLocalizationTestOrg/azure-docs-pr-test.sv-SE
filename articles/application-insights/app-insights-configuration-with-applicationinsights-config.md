---
title: aaaApplicationInsights.config - referens i Azure | Microsoft Docs
description: "Aktivera eller inaktivera data collection moduler och lägga till prestandaräknare och andra parametrar."
services: application-insights
documentationcenter: 
author: OlegAnaniev-MSFT
editor: alancameronwills
manager: carmonm
ms.assetid: 6e397752-c086-46e9-8648-a1196e8078c2
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 05/3/2017
ms.author: bwren
ms.openlocfilehash: 76cb11349d87dfc508ec8b1c454259a0b079c48a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="configuring-hello-application-insights-sdk-with-applicationinsightsconfig-or-xml"></a>Konfigurera hello Application Insights SDK med ApplicationInsights.config eller .xml
hello Application Insights .NET SDK består av ett antal NuGet-paket. Den [core-paketet](http://www.nuget.org/packages/Microsoft.ApplicationInsights) innehåller hello API för att skicka telemetri till hello Application Insights. [Ytterligare paket](http://www.nuget.org/packages?q=Microsoft.ApplicationInsights) ange telemetri *moduler* och *initierare* för automatiskt spåra telemetri från ditt program och dess kontext. Genom att justera hello-konfigurationsfil kan du aktivera eller inaktivera telemetri moduler och initierare och ange parametrar för några av dem.

hello konfigurationsfilen heter `ApplicationInsights.config` eller `ApplicationInsights.xml`, beroende på hello typ av ditt program. Läggs den automatiskt tooyour projektet när du [installera de flesta versioner av hello SDK][start]. Läggs också tooa webbprogram genom [Status Monitor på en IIS-server][redfield], eller när du väljer hello Appplication Insights [tillägg för Azure-webbplats eller VM](app-insights-azure-web-apps.md).

Det finns inte en motsvarande filen toocontrol hello [SDK på en webbsida][client].

Det här dokumentet beskriver hello avsnitt som du ser i hello configuration-fil, hur de styra hello komponenter i hello SDK, och vilka NuGet-paket att läsa in dessa komponenter.

## <a name="telemetry-modules-aspnet"></a>Telemetri moduler (ASP.NET)
Varje telemetri modul samlar in en viss typ av data och använder hello core API toosend hello data. hello-moduler installeras av olika NuGet-paket, som också lägga till hello rader som behövs toohello .config-fil.

Det är en nod i hello konfigurationsfilen för varje modul. toodisable en modul, ta bort hello nod eller kommentera ut.

### <a name="dependency-tracking"></a>Beroende spårning
[Beroende spårning](app-insights-asp-net-dependencies.md) samlar in telemetri om anrop som gör att din app toodatabases och externa tjänster och databaser. tooallow den här modulen toowork i en IIS-server du behöver för[installera statusövervakaren][redfield]. toouse den i Azure-webbappar eller virtuella datorer, [markerar hello Application Insights-tillägget](app-insights-azure-web-apps.md).

Du kan också skriva egna beroende spårning kod med hello [TrackDependency API](app-insights-api-custom-events-metrics.md#trackdependency).

* `Microsoft.ApplicationInsights.DependencyCollector.DependencyTrackingTelemetryModule`
* [Microsoft.ApplicationInsights.DependencyCollector](http://www.nuget.org/packages/Microsoft.ApplicationInsights.DependencyCollector) NuGet-paketet.

### <a name="performance-collector"></a>Insamlare för prestanda
[Samlar in prestandaräknare för system](app-insights-performance-counters.md) t.ex CPU, minne och nätverk läses in från IIS-installationer. Du kan ange vilka räknare toocollect, inklusive prestandaräknare som du har skapat själv.

* `Microsoft.ApplicationInsights.Extensibility.PerfCounterCollector.PerformanceCollectorModule`
* [Microsoft.ApplicationInsights.PerfCounterCollector](http://www.nuget.org/packages/Microsoft.ApplicationInsights.PerfCounterCollector) NuGet-paketet.

### <a name="application-insights-diagnostics-telemetry"></a>Application Insights-Diagnostiktelemetri
Hej `DiagnosticsTelemetryModule` rapporterar fel i hello Application Insights instrumentation själva koden. Till exempel om hello kod inte kan komma åt prestandaräknare eller om en `ITelemetryInitializer` genererar ett undantag. Spårningstelemetri spåras av den här modulen visas i hello [diagnostiska Sök][diagnostic]. Skickar diagnostikdata toodc.services.vsallin.net.

* `Microsoft.ApplicationInsights.Extensibility.Implementation.Tracing.DiagnosticsTelemetryModule`
* [Microsoft.ApplicationInsights](http://www.nuget.org/packages/Microsoft.ApplicationInsights) NuGet-paketet. Om du endast installera det här paketet skapas inte hello ApplicationInsights.config-filen automatiskt.

### <a name="developer-mode"></a>Utvecklarläge
`DeveloperModeWithDebuggerAttachedTelemetryModule`Framtvingar hello Application Insights `TelemetryChannel` toosend data omedelbart, en telemetri objekt samtidigt, när en felsökare är kopplad toohello programprocessen. Detta minskar hello tidslängd mellan hello nu när ditt program spårar telemetri och när den visas på hello Application Insights-portalen. Det medför betydande kostnader i CPU: N och bandbredd.

* `Microsoft.ApplicationInsights.WindowsServer.DeveloperModeWithDebuggerAttachedTelemetryModule`
* [Application Insights Windows Server](http://www.nuget.org/packages/Microsoft.ApplicationInsights.WindowsServer/) NuGet-paketet

### <a name="web-request-tracking"></a>Webbegäran spårning
Rapporter hello [tid och resultatet svarskoden](app-insights-asp-net.md) av HTTP-begäranden.

* `Microsoft.ApplicationInsights.Web.RequestTrackingTelemetryModule`
* [Microsoft.ApplicationInsights.Web](http://www.nuget.org/packages/Microsoft.ApplicationInsights.Web) NuGet-paketet

### <a name="exception-tracking"></a>Undantagsspårning
`ExceptionTrackingTelemetryModule`spårar ohanterade undantag i ditt webbprogram. Se [fel och undantag][exceptions].

* `Microsoft.ApplicationInsights.Web.ExceptionTrackingTelemetryModule`
* [Microsoft.ApplicationInsights.Web](http://www.nuget.org/packages/Microsoft.ApplicationInsights.Web) NuGet-paketet
* `Microsoft.ApplicationInsights.WindowsServer.UnobservedExceptionTelemetryModule`-spårar [symptom uppgiften undantag](http://blogs.msdn.com/b/pfxteam/archive/2011/09/28/task-exception-handling-in-net-4-5.aspx).
* `Microsoft.ApplicationInsights.WindowsServer.UnhandledExceptionTelemetryModule`-spårar ohanterade undantag för arbetsroller, windows-tjänster och program.
* [Application Insights Windows Server](http://www.nuget.org/packages/Microsoft.ApplicationInsights.WindowsServer/) NuGet-paketet.

### <a name="eventsource-tracking"></a>EventSource spårning
`EventSourceTelemetryModule`låter dig tooconfigure EventSource händelser toobe skickas tooApplication insikter som spårningar. Information om hur du spårar EventSource händelser finns [med EventSource händelser](app-insights-asp-net-trace-logs.md#using-eventsource-events).

* `Microsoft.ApplicationInsights.EventSourceListener.EventSourceTelemetryModule`
* [Microsoft.ApplicationInsights.EventSourceListener](http://www.nuget.org/packages/Microsoft.ApplicationInsights.EventSourceListener) 

### <a name="etw-event-tracking"></a>Spårning av ETW-händelse
`EtwCollectorTelemetryModule`låter dig tooconfigure händelser från ETW-providers toobe skickas tooApplication insikter som spårningar. Information om hur du spårar ETW-händelser finns i [med hjälp av ETW-händelser](app-insights-asp-net-trace-logs.md#using-etw-events).

* `Microsoft.ApplicationInsights.EtwCollector.EtwCollectorTelemetryModule`
* [Microsoft.ApplicationInsights.EtwCollector](http://www.nuget.org/packages/Microsoft.ApplicationInsights.EtwCollector) 

### <a name="microsoftapplicationinsights"></a>Microsoft.ApplicationInsights
Hej Microsoft.ApplicationInsights erbjuder hello [core API](https://msdn.microsoft.com/library/mt420197.aspx) av hello SDK. hello andra telemetri moduler använda detta, och du kan också [använder den toodefine egna telemetri](app-insights-api-custom-events-metrics.md).

* Ingen post i ApplicationInsights.config.
* [Microsoft.ApplicationInsights](http://www.nuget.org/packages/Microsoft.ApplicationInsights) NuGet-paketet. Om du bara installera den här NuGet skapas ingen .config-fil.

## <a name="telemetry-channel"></a>Telemetri kanal
hello telemetri kanaler hanterar buffring och överföring av telemetri toohello Application Insights-tjänsten.

* `Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel.ServerTelemetryChannel`är hello standardkanalen för tjänster. Den buffrar data i minnet.
* `Microsoft.ApplicationInsights.PersistenceChannel`är ett alternativ för konsolprogram. Det kan spara alla unflushed toopersistent datalagring när din app stängs och skicka den när hello appen startas igen.

## <a name="telemetry-initializers-aspnet"></a>Telemetri initierare (ASP.NET)
Telemetri initierare ange kontextegenskaper för som skickas tillsammans med alla element på telemetri.

Du kan [skriva egna initierare](app-insights-api-filtering-sampling.md#add-properties) tooset kontextegenskaper.

hello standard initierare har värdet av hello webb- eller WindowsServer NuGet-paket:

* `AccountIdTelemetryInitializer`Egenskapen hello AccountId.
* `AuthenticatedUserIdTelemetryInitializer`Egenskapen hello AuthenticatedUserId som angetts av hello JavaScript SDK.
* `AzureRoleEnvironmentTelemetryInitializer`uppdateringar hello `RoleName` och `RoleInstance` egenskaper för hello `Device` kontext för alla telemetri artiklar med information som hämtas från hello Azure körningsmiljön.
* `BuildInfoConfigComponentVersionTelemetryInitializer`uppdateringar hello `Version` -egenskapen för hello `Component` kontext för alla telemetri artiklar med hello-värde som hämtats från hello `BuildInfo.config` fil produceras av MS-Build.
* `ClientIpHeaderTelemetryInitializer`uppdateringar `Ip` -egenskapen för hello `Location` kontexten för alla telemetri objekt baserat på hello `X-Forwarded-For` HTTP-huvud för hello-begäran.
* `DeviceTelemetryInitializer`uppdateringar hello följande egenskaper för hello `Device` kontext för alla telemetri objekt.
  * `Type`anges för ”dator”
  * `Id`anges toohello domännamnet för hello dator där hello webbtillämpning körs.
  * `OemName`är värdet toohello extraheras från hello `Win32_ComputerSystem.Manufacturer` med hjälp av WMI.
  * `Model`är värdet toohello extraheras från hello `Win32_ComputerSystem.Model` med hjälp av WMI.
  * `NetworkType`är värdet toohello extraheras från hello `NetworkInterface`.
  * `Language`anges toohello namnet på hello `CurrentCulture`.
* `DomainNameRoleInstanceTelemetryInitializer`uppdateringar hello `RoleInstance` -egenskapen för hello `Device` kontext för alla telemetri artiklar med hello domännamn för hello dator där hello webbprogram körs.
* `OperationNameTelemetryInitializer`uppdateringar hello `Name` -egenskapen för hello `RequestTelemetry` och hello `Name` -egenskapen för hello `Operation` kontexten för alla telemetri objekt baserat på hello HTTP-metoden, samt namnen på ASP.NET MVC-kontrollanten och åtgärden anropade tooprocess hello begäran.
* `OperationIdTelemetryInitializer`eller `OperationCorrelationTelemetryInitializer` uppdateringar hello `Operation.Id` kontextegenskap telemetri postobjekt spåras när en begäran med hello genereras automatiskt `RequestTelemetry.Id`.
* `SessionTelemetryInitializer`uppdateringar hello `Id` -egenskapen för hello `Session` kontext för alla telemetri objekt med värdet som har extraherats från hello `ai_session` cookie som genererats av hello ApplicationInsights JavaScript instrumentation kod som körs i hello användarens webbläsare.
* `SyntheticTelemetryInitializer`eller `SyntheticUserAgentTelemetryInitializer` uppdateringar hello `User`, `Session` och `Operation` kontexter egenskaper för alla objekt i telemetri spåras vid hantering av en begäran från en syntetisk källa, t.ex. en tillgänglighet testa eller Sök motorn bot. Som standard [Metrics Explorer](app-insights-metrics-explorer.md) visar inte syntetiska telemetri.

    Hej `<Filters>` ange identifierar egenskaperna för hello begäranden.
* `UserAgentTelemetryInitializer`uppdateringar hello `UserAgent` -egenskapen för hello `User` kontexten för alla telemetri objekt baserat på hello `User-Agent` HTTP-huvud för hello-begäran.
* `UserTelemetryInitializer`uppdateringar hello `Id` och `AcquisitionDate` egenskaper för `User` kontext för alla telemetri objekt med värden som extraheras från hello `ai_user` cookie som genererats av hello Application Insights JavaScript instrumentation kod som körs i hello användarens webbläsare.
* `WebTestTelemetryInitializer`Anger hello användar-id, sessions-id och syntetiska datakällans egenskaper för HTTP-begäranden som kommer från [tillgänglighetstester](app-insights-monitor-web-app-availability.md).
  Hej `<Filters>` ange identifierar egenskaperna för hello begäranden.

För .NET-program som körs i Service Fabric kan du inkludera hello `Microsoft.ApplicationInsights.ServiceFabric` NuGet-paketet. Det här paketet innehåller en `FabricTelemetryInitializer`, vilket ger Service Fabric egenskaper tootelemetry objekt. Mer information finns i hello [GitHub-sidan](https://go.microsoft.com/fwlink/?linkid=848457) om hello egenskaper lagts till av den här NuGet-paketet.

## <a name="telemetry-processors-aspnet"></a>Telemetri processorer (ASP.NET)
Telemetri processorer kan filtrera och ändra varje telemetri objekt precis innan den skickas från hello SDK toohello portalen.

Du kan [skriva telemetri processorerna](app-insights-api-filtering-sampling.md#filtering).

#### <a name="adaptive-sampling-telemetry-processor-from-200-beta3"></a>Anpassningsbar provtagning telemetri processor (från 2.0.0-beta3)
Den här funktionen är aktiverad som standard. Om appen skickar en mängd telemetri, denna processor tar bort vissa av den.

```xml

    <TelemetryProcessors>
      <Add Type="Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel.AdaptiveSamplingTelemetryProcessor, Microsoft.AI.ServerTelemetryChannel">
        <MaxTelemetryItemsPerSecond>5</MaxTelemetryItemsPerSecond>
      </Add>
    </TelemetryProcessors>

```

hello parametern innehåller hello mål som hello algoritmen försöker tooachieve. Varje instans av hello SDK fungerar oberoende av varandra, så om din server är ett kluster på flera datorer, hello faktiska telemetrivolym ska multipliceras därefter.

[Lär dig mer om sampling](app-insights-sampling.md).

#### <a name="fixed-rate-sampling-telemetry-processor-from-200-beta1"></a>Fast räntesats provtagning telemetri processor (från 2.0.0-beta1)
Det finns också en standard [provtagning telemetri processor](app-insights-api-filtering-sampling.md) (från 2.0.1):

```XML

    <TelemetryProcessors>
     <Add Type="Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel.SamplingTelemetryProcessor, Microsoft.AI.ServerTelemetryChannel">

     <!-- Set a percentage close too100/N where N is an integer. -->
     <!-- E.g. 50 (=100/2), 33.33 (=100/3), 25 (=100/4), 20, 1 (=100/100), 0.1 (=100/1000) -->
     <SamplingPercentage>10</SamplingPercentage>
     </Add>
   </TelemetryProcessors>

```



## <a name="channel-parameters-java"></a>Kanalparametrar (Java)
Dessa parametrar påverkar hur hello Java SDK ska lagra och rensa hello telemetridata den samlar in.

#### <a name="maxtelemetrybuffercapacity"></a>MaxTelemetryBufferCapacity
hello antal telemetri objekt som kan lagras i hello SDK InMemory-lagringen. När antalet har uppnåtts hello telemetri bufferten töms - som är hello telemetri objekt skickas toohello Application Insights-server.

* Min: 1
* Max: 1000
* Standard: 500

```

  <ApplicationInsights>
      ...
      <Channel>
       <MaxTelemetryBufferCapacity>100</MaxTelemetryBufferCapacity>
      </Channel>
      ...
  </ApplicationInsights>
```

#### <a name="flushintervalinseconds"></a>FlushIntervalInSeconds
Anger hur ofta hello data som lagras i minnet i hello ska vara en (skickade tooApplication insikter).

* Min: 1
* Max: 300
* Standard: 5

```

    <ApplicationInsights>
      ...
      <Channel>
        <FlushIntervalInSeconds>100</FlushIntervalInSeconds>
      </Channel>
      ...
    </ApplicationInsights>
```

#### <a name="maxtransmissionstoragecapacityinmb"></a>MaxTransmissionStorageCapacityInMB
Anger hello maximal storlek i MB som har tilldelats toohello beständig lagring på hello lokal disk. Den här används för bestående telemetri objekt som inte kunde toobe överförs toohello Application Insights slutpunkt. När hello lagringsstorlek har uppfyllts, ignoreras nya telemetri objekt.

* Min: 1
* Max: 100
* Standard: 10

```

   <ApplicationInsights>
      ...
      <Channel>
        <MaxTransmissionStorageCapacityInMB>50</MaxTransmissionStorageCapacityInMB>
      </Channel>
      ...
   </ApplicationInsights>
```



## <a name="instrumentationkey"></a>InstrumentationKey
Detta avgör hello Application Insights-resurs där dina data visas. Normalt skapar du en separat resurs med en separat nyckel för var och en av dina program.

Om du vill tooset hello nyckeln dynamiskt – till exempel om du vill att toosend resultatet från din programresurser toodifferent - du utelämnar hello nyckeln från hello konfigurationsfilen och ange i koden i stället.

tooset hello nyckel för alla instanser av TelemetryClient, inklusive standard telemetri moduler, ange hello nyckeln i TelemetryConfiguration.Active. Gör så här i en initieringsmetod, till exempel global.aspx.cs i en ASP.NET-tjänst:

```C#

    protected void Application_Start()
    {
      Microsoft.ApplicationInsights.Extensibility.
        TelemetryConfiguration.Active.InstrumentationKey =
          // - for example -
          WebConfigurationManager.Settings["ikey"];
      //...
```

Om du bara vill toosend en specifik uppsättning händelser tooa annan resurs kan du ange hello nyckel för en specifik TelemetryClient:

```C#

    var tc = new TelemetryClient();
    tc.Context.InstrumentationKey = "----- my key ----";
    tc.TrackEvent("myEvent");
    // ...

```

tooget en ny nyckel [skapar en ny resurs i hello Application Insights-portalen][new].

## <a name="next-steps"></a>Nästa steg
[Mer information om hello API][api].

<!--Link references-->

[api]: app-insights-api-custom-events-metrics.md
[client]: app-insights-javascript.md
[diagnostic]: app-insights-diagnostic-search.md
[exceptions]: app-insights-asp-net-exceptions.md
[netlogs]: app-insights-asp-net-trace-logs.md
[new]: app-insights-create-new-resource.md
[redfield]: app-insights-monitor-performance-live-website-now.md
[start]: app-insights-overview.md
