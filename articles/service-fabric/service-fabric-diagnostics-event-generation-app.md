---
title: "Service Fabric nivå programövervakning aaaAzure | Microsoft Docs"
description: "Lär dig mer om programmet och service händelser och Prestandaloggar används toomonitor och diagnostisera Azure Service Fabric-kluster."
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
ms.date: 05/26/2017
ms.author: dekapur
ms.openlocfilehash: 4f4da1eaad4b88428eaa3a2100ac25c8a285a727
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="application-and-service-level-event-and-log-generation"></a>Program- och nivå händelse och logg

## <a name="instrumenting-hello-code-with-custom-events"></a>Den instrumentella hello kod med anpassade händelser

Instrumentering hello koden är hello grunden för de flesta andra aspekter av övervaka dina tjänster. Instrumentation är hello enda sättet du vet att det är något fel och toodiagnose vad måste toobe fast. Men det är tekniskt möjligt tooconnect en felsökare tooa produktion tjänst, är det inte vanligt. Därför är har detaljerad instrumentationsdata viktigt.

Vissa produkter instrumentera automatiskt din kod. Även om dessa lösningar kan också fungera, krävs nästan alltid manuell instrumentation. Du måste ha tillräckligt med information tooforensically felsöka hello program i hello slutet. Det här dokumentet beskriver olika metoder tooinstrumenting koden, och när toochoose en närmar sig över en annan.

## <a name="eventsource"></a>eventSource
När du skapar en Service Fabric-lösning från en mall i Visual Studio en **EventSource**-härledd klass (**ServiceEventSource** eller **ActorEventSource**) genereras. En mall skapas, där du kan lägga till händelser för programmet eller tjänsten. Hej **EventSource** namn **måste** vara unikt och bör ändras från hello standardsträngen till mallen mitt företag -&lt;lösning&gt; - &lt; projektet&gt;. Att flera **EventSource** definitioner som använder hello samma namn orsaker ett problem vid körning. Varje definierad händelse måste ha en unik identifierare. Ett körningsfel inträffar om en identifierare som inte är unikt. Vissa organisationer preassign intervall med värden för identifierare tooavoid konflikter mellan separat utvecklingsgrupper. Mer information finns i [Vance's blog](https://blogs.msdn.microsoft.com/vancem/2012/07/09/introduction-tutorial-logging-etw-events-in-c-system-diagnostics-tracing-eventsource/) eller hello [MSDN-dokumentationen](https://msdn.microsoft.com/library/dn774985(v=pandp.20).aspx).

### <a name="using-structured-eventsource-events"></a>Med hjälp av strukturerade EventSource händelser

Var och en av hello händelser i hello kodexempel i det här avsnittet har definierats för specialfall, till exempel när en service type har registrerats. När du definierar meddelanden av användningsfall, data kan paketeras med hello hello fel och du kan mer söka enkelt och filter baserat på hello namn eller värde i hello angivna egenskaper. Strukturera hello instrumentation utdata gör det enklare tooconsume men kräver mer tankar och tid toodefine en ny händelse för varje användningsfall. Vissa Händelsedefinitioner kan delas mellan hello hela programmet. Till exempel en metod starta eller stoppa skulle händelsen återanvändas i många tjänster i ett program. En domänspecifika-tjänst som ett order-system kan ha en **CreateOrder** händelsen, som har en egen unik händelse. Den här metoden kan generera många händelser och potentiellt kräver samordning av identifierare i projektgruppen. 

```csharp
    [EventSource(Name = "MyCompany-VotingState-VotingStateService")]
    internal sealed class ServiceEventSource : EventSource
    {
        public static readonly ServiceEventSource Current = new ServiceEventSource();

        // hello instance constructor is private tooenforce singleton semantics.
        private ServiceEventSource() : base() { }

        ...

        // hello ServiceTypeRegistered event contains a unique identifier, an event attribute that defined hello event, and hello code implementation of hello event.
        private const int ServiceTypeRegisteredEventId = 3;
        [Event(ServiceTypeRegisteredEventId, Level = EventLevel.Informational, Message = "Service host process {0} registered service type {1}", Keywords = Keywords.ServiceInitialization)]
        public void ServiceTypeRegistered(int hostProcessId, string serviceType)
        {
            WriteEvent(ServiceTypeRegisteredEventId, hostProcessId, serviceType);
        }

        // hello ServiceHostInitializationFailed event contains a unique identifier, an event attribute that defined hello event, and hello code implementation of hello event.
        private const int ServiceHostInitializationFailedEventId = 4;
        [Event(ServiceHostInitializationFailedEventId, Level = EventLevel.Error, Message = "Service host initialization failed", Keywords = Keywords.ServiceInitialization)]
        public void ServiceHostInitializationFailed(string exception)
        {
            WriteEvent(ServiceHostInitializationFailedEventId, exception);
        }
```

### <a name="using-eventsource-generically"></a>Med hjälp av EventSource Allmänt

Eftersom kan vara svårt att definiera specifika händelser, definiera många några händelser med en gemensam uppsättning parametrar som vanligtvis utdata av information som en sträng. Mycket av hello strukturerad aspekt går förlorad och det är svårare toosearch och filtrera hello resultat. Några händelser som vanligtvis motsvarar toohello loggningsnivåer definieras i den här metoden. hello följande fragment definierar ett felsöknings- och fel meddelande:

```csharp
    [EventSource(Name = "MyCompany-VotingState-VotingStateService")]
    internal sealed class ServiceEventSource : EventSource
    {
        public static readonly ServiceEventSource Current = new ServiceEventSource();

        // hello Instance constructor is private, tooenforce singleton semantics.
        private ServiceEventSource() : base() { }

        ...

        private const int DebugEventId = 10;
        [Event(DebugEventId, Level = EventLevel.Verbose, Message = "{0}")]
        public void Debug(string msg)
        {
            WriteEvent(DebugEventId, msg);
        }

        private const int ErrorEventId = 11;
        [Event(ErrorEventId, Level = EventLevel.Error, Message = "Error: {0} - {1}")]
        public void Error(string error, string msg)
        {
            WriteEvent(ErrorEventId, error, msg);
        }
```

Med hjälp av en kombination av strukturerade och allmänna instrumentation kan fungerar också bra. Strukturerade instrumentation används för att rapportera fel och mått. Allmänna händelser kan användas för hello detaljerad loggning som används av tekniker för felsökning.

## <a name="aspnet-core-logging"></a>ASP.NET Core loggning

Viktiga toocarefully planera hur du kommer instrumentera din kod. hello rätt instrumentation schemat kan hjälpa dig att undvika potentiellt destabilizing din kodbas och behöver tooreinstrument hello kod. tooreduce risk som du kan välja en instrumentation bibliotek som [Microsoft.Extensions.Logging](https://www.nuget.org/packages/Microsoft.Extensions.Logging/), vilket är en del av Microsoft ASP.NET Core. ASP.NET Core har en [ILogger](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.ilogger) gränssnitt som du kan använda med hello leverantör du väljer, och minimerar hello effekt på befintlig kod. Du kan använda hello koden i ASP.NET Core på Windows och Linux och fullständig .NET Framework i hello så instrumentation koden är standardiserad. Detta är ytterligare utforskade nedan:

### <a name="using-microsoftextensionslogging-in-service-fabric"></a>Med hjälp av Microsoft.Extensions.Logging i Service Fabric

1. Lägg till hello Microsoft.Extensions.Logging NuGet-paketet toohello projektet tooinstrument. Lägg även till eventuella provider-paket (ett paket från tredje part, finns i följande exempel hello). Mer information finns i [loggning i ASP.NET Core](https://docs.microsoft.com/aspnet/core/fundamentals/logging).
2. Lägg till en **med** direktiv för Microsoft.Extensions.Logging tooyour service-filen.
3. Definiera en privat variabel inom service-klassen.

  ```csharp
  private ILogger _logger = null;

  ```
4. Lägg till den här koden i hello konstruktorn för tjänstklassen:

  ```csharp
  _logger = new LoggerFactory().CreateLogger<Stateless>();

  ```
5. Starta instrumentering koden i din metoder. Här följer några exempel:

  ```csharp
  _logger.LogDebug("Debug-level event from Microsoft.Logging");
  _logger.LogInformation("Informational-level event from Microsoft.Logging");

  // In this variant, we're adding structured properties RequestName and Duration, which have values MyRequest and hello duration of hello request.
  // Later in hello article, we discuss why this step is useful.
  _logger.LogInformation("{RequestName} {Duration}", "MyRequest", requestDuration);

  ```

## <a name="using-other-logging-providers"></a>Med hjälp av andra leverantörer av loggning

Vissa leverantörer Använd hello-metod som beskrivs i föregående avsnitt, hello inklusive [Serilog](https://serilog.net/), [NLog](http://nlog-project.org/), och [Loggr](https://github.com/imobile3/Loggr.Extensions.Logging). Du kan ansluta var och en av dessa i ASP.NET Core loggning eller du kan använda dem separat. Serilog har en funktion som förbättra alla meddelanden från en logg. Den här funktionen kan vara användbar toooutput hello tjänstens namn, typ och partitionsinformation. toouse den här funktionen i hello ASP.NET Core infrastruktur, utföra de här stegen:

1. Lägg till hello Serilog, Serilog.Extensions.Logging, och Serilog.Sinks.Observable NuGet-paket toohello projekt. Exempelvis hello nästa du också lägga till Serilog.Sinks.Literate. Bättre visas nedan.
2. I Serilog, skapar du en LoggerConfiguration och hello loggaren-instans.

  ```csharp
  Log.Logger = new LoggerConfiguration().WriteTo.LiterateConsole().CreateLogger();
  ```

3. Lägg till en Serilog.ILogger argumentet toohello service konstruktor, och hello nyskapad meddelandeloggfiler.

  ```csharp
  ServiceRuntime.RegisterServiceAsync("StatelessType", context => new Stateless(context, Log.Logger)).GetAwaiter().GetResult();
  ```

4. Lägga till hello efter koden som skapar hello egenskapen enrichers för hello hello service konstruktorn **ServiceTypeName**, **ServiceName**, **PartitionId**, och  **InstanceId** egenskaperna för hello-tjänst. Det ger också en egenskap enricher toohello ASP.NET Core loggning fabriken, så att du kan använda Microsoft.Extensions.Logging.ILogger i koden.

  ```csharp
  public Stateless(StatelessServiceContext context, Serilog.ILogger serilog)
      : base(context)
  {
      PropertyEnricher[] properties = new PropertyEnricher[]
      {
          new PropertyEnricher("ServiceTypeName", context.ServiceTypeName),
          new PropertyEnricher("ServiceName", context.ServiceName),
          new PropertyEnricher("PartitionId", context.PartitionId),
          new PropertyEnricher("InstanceId", context.ReplicaOrInstanceId),
      };

      serilog.ForContext(properties);

      _logger = new LoggerFactory().AddSerilog(serilog.ForContext(properties)).CreateLogger<Stateless>();
  }
  ```

5. Betalningsinstrument hello kod hello samma som om du använde ASP.NET Core utan Serilog.

  >[!NOTE]
  >Vi rekommenderar att du inte använder hello statiskt Log.Logger med hello föregående exempel. Service Fabric kan vara värd för flera instanser av hello tjänsten samma typ i en enda process. Om du använder Hej statiska Log.Logger, hello senaste skrivaren av hello egenskapen enrichers visas värdena för alla instanser som körs. Det här är ett skäl till varför hello _logger variabeln är en privat medlemsvariabel för hello tjänstklass. Du måste också göra hello _logger tillgängliga toocommon kod, som kan användas för tjänster.

## <a name="choosing-a-logging-provider"></a>Att välja en provider för loggning

Om programmet är beroende av hög prestanda, **EventSource** vanligtvis är en bra metod. **EventSource** *vanligtvis* använder färre resurser och presterar bättre än ASP.NET Core loggning eller någon av hello tillgängliga tredjeparts-lösningar.  Detta är inte ett problem för många tjänster, men om tjänsten är inriktade på prestanda, med hjälp av **EventSource** kan vara ett bättre alternativ. Dock tooget dessa fördelarna med strukturerade loggning **EventSource** kräver en större från din teknikteamet. Om möjligt gör en snabb prototyp av några alternativ för loggning och välj sedan hello som bäst uppfyller dina behov.

## <a name="next-steps"></a>Nästa steg

När du har valt dina loggning providern tooinstrument dina program och tjänster, måste din loggar och händelser toobe samman innan de kan skickas tooany analysplattform. Läs mer om [EventFlow](service-fabric-diagnostics-event-aggregation-eventflow.md) och [BOMULLSTUSS](service-fabric-diagnostics-event-aggregation-wad.md) toobetter känner till vissa av hello rekommenderat alternativ.
