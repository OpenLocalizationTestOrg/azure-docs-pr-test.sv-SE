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
# <a name="application-and-service-level-event-and-log-generation"></a><span data-ttu-id="b0a29-103">Program- och nivå händelse och logg</span><span class="sxs-lookup"><span data-stu-id="b0a29-103">Application and service level event and log generation</span></span>

## <a name="instrumenting-hello-code-with-custom-events"></a><span data-ttu-id="b0a29-104">Den instrumentella hello kod med anpassade händelser</span><span class="sxs-lookup"><span data-stu-id="b0a29-104">Instrumenting hello code with custom events</span></span>

<span data-ttu-id="b0a29-105">Instrumentering hello koden är hello grunden för de flesta andra aspekter av övervaka dina tjänster.</span><span class="sxs-lookup"><span data-stu-id="b0a29-105">Instrumenting hello code is hello basis for most other aspects of monitoring your services.</span></span> <span data-ttu-id="b0a29-106">Instrumentation är hello enda sättet du vet att det är något fel och toodiagnose vad måste toobe fast.</span><span class="sxs-lookup"><span data-stu-id="b0a29-106">Instrumentation is hello only way you can know that something is wrong, and toodiagnose what needs toobe fixed.</span></span> <span data-ttu-id="b0a29-107">Men det är tekniskt möjligt tooconnect en felsökare tooa produktion tjänst, är det inte vanligt.</span><span class="sxs-lookup"><span data-stu-id="b0a29-107">Although technically it's possible tooconnect a debugger tooa production service, it's not a common practice.</span></span> <span data-ttu-id="b0a29-108">Därför är har detaljerad instrumentationsdata viktigt.</span><span class="sxs-lookup"><span data-stu-id="b0a29-108">So, having detailed instrumentation data is important.</span></span>

<span data-ttu-id="b0a29-109">Vissa produkter instrumentera automatiskt din kod.</span><span class="sxs-lookup"><span data-stu-id="b0a29-109">Some products automatically instrument your code.</span></span> <span data-ttu-id="b0a29-110">Även om dessa lösningar kan också fungera, krävs nästan alltid manuell instrumentation.</span><span class="sxs-lookup"><span data-stu-id="b0a29-110">Although these solutions can work well, manual instrumentation is almost always required.</span></span> <span data-ttu-id="b0a29-111">Du måste ha tillräckligt med information tooforensically felsöka hello program i hello slutet.</span><span class="sxs-lookup"><span data-stu-id="b0a29-111">In hello end, you must have enough information tooforensically debug hello application.</span></span> <span data-ttu-id="b0a29-112">Det här dokumentet beskriver olika metoder tooinstrumenting koden, och när toochoose en närmar sig över en annan.</span><span class="sxs-lookup"><span data-stu-id="b0a29-112">This document describes different approaches tooinstrumenting your code, and when toochoose one approach over another.</span></span>

## <a name="eventsource"></a><span data-ttu-id="b0a29-113">eventSource</span><span class="sxs-lookup"><span data-stu-id="b0a29-113">EventSource</span></span>
<span data-ttu-id="b0a29-114">När du skapar en Service Fabric-lösning från en mall i Visual Studio en **EventSource**-härledd klass (**ServiceEventSource** eller **ActorEventSource**) genereras.</span><span class="sxs-lookup"><span data-stu-id="b0a29-114">When you create a Service Fabric solution from a template in Visual Studio, an **EventSource**-derived class (**ServiceEventSource** or **ActorEventSource**) is generated.</span></span> <span data-ttu-id="b0a29-115">En mall skapas, där du kan lägga till händelser för programmet eller tjänsten.</span><span class="sxs-lookup"><span data-stu-id="b0a29-115">A template is created, in which you can add events for your application or service.</span></span> <span data-ttu-id="b0a29-116">Hej **EventSource** namn **måste** vara unikt och bör ändras från hello standardsträngen till mallen mitt företag -&lt;lösning&gt; - &lt; projektet&gt;.</span><span class="sxs-lookup"><span data-stu-id="b0a29-116">hello **EventSource** name **must** be unique, and should be renamed from hello default template string MyCompany-&lt;solution&gt;-&lt;project&gt;.</span></span> <span data-ttu-id="b0a29-117">Att flera **EventSource** definitioner som använder hello samma namn orsaker ett problem vid körning.</span><span class="sxs-lookup"><span data-stu-id="b0a29-117">Having multiple **EventSource** definitions that use hello same name causes an issue at run time.</span></span> <span data-ttu-id="b0a29-118">Varje definierad händelse måste ha en unik identifierare.</span><span class="sxs-lookup"><span data-stu-id="b0a29-118">Each defined event must have a unique identifier.</span></span> <span data-ttu-id="b0a29-119">Ett körningsfel inträffar om en identifierare som inte är unikt.</span><span class="sxs-lookup"><span data-stu-id="b0a29-119">If an identifier is not unique, a runtime failure occurs.</span></span> <span data-ttu-id="b0a29-120">Vissa organisationer preassign intervall med värden för identifierare tooavoid konflikter mellan separat utvecklingsgrupper.</span><span class="sxs-lookup"><span data-stu-id="b0a29-120">Some organizations preassign ranges of values for identifiers tooavoid conflicts between separate development teams.</span></span> <span data-ttu-id="b0a29-121">Mer information finns i [Vance's blog](https://blogs.msdn.microsoft.com/vancem/2012/07/09/introduction-tutorial-logging-etw-events-in-c-system-diagnostics-tracing-eventsource/) eller hello [MSDN-dokumentationen](https://msdn.microsoft.com/library/dn774985(v=pandp.20).aspx).</span><span class="sxs-lookup"><span data-stu-id="b0a29-121">For more information, see [Vance's blog](https://blogs.msdn.microsoft.com/vancem/2012/07/09/introduction-tutorial-logging-etw-events-in-c-system-diagnostics-tracing-eventsource/) or hello [MSDN documentation](https://msdn.microsoft.com/library/dn774985(v=pandp.20).aspx).</span></span>

### <a name="using-structured-eventsource-events"></a><span data-ttu-id="b0a29-122">Med hjälp av strukturerade EventSource händelser</span><span class="sxs-lookup"><span data-stu-id="b0a29-122">Using structured EventSource events</span></span>

<span data-ttu-id="b0a29-123">Var och en av hello händelser i hello kodexempel i det här avsnittet har definierats för specialfall, till exempel när en service type har registrerats.</span><span class="sxs-lookup"><span data-stu-id="b0a29-123">Each of hello events in hello code examples in this section are defined for a specific case, for example, when a service type is registered.</span></span> <span data-ttu-id="b0a29-124">När du definierar meddelanden av användningsfall, data kan paketeras med hello hello fel och du kan mer söka enkelt och filter baserat på hello namn eller värde i hello angivna egenskaper.</span><span class="sxs-lookup"><span data-stu-id="b0a29-124">When you define messages by use case, data can be packaged with hello text of hello error, and you can more easily search and filter based on hello names or values of hello specified properties.</span></span> <span data-ttu-id="b0a29-125">Strukturera hello instrumentation utdata gör det enklare tooconsume men kräver mer tankar och tid toodefine en ny händelse för varje användningsfall.</span><span class="sxs-lookup"><span data-stu-id="b0a29-125">Structuring hello instrumentation output makes it easier tooconsume, but requires more thought and time toodefine a new event for each use case.</span></span> <span data-ttu-id="b0a29-126">Vissa Händelsedefinitioner kan delas mellan hello hela programmet.</span><span class="sxs-lookup"><span data-stu-id="b0a29-126">Some event definitions can be shared across hello entire application.</span></span> <span data-ttu-id="b0a29-127">Till exempel en metod starta eller stoppa skulle händelsen återanvändas i många tjänster i ett program.</span><span class="sxs-lookup"><span data-stu-id="b0a29-127">For example, a method start or stop event would be reused across many services within an application.</span></span> <span data-ttu-id="b0a29-128">En domänspecifika-tjänst som ett order-system kan ha en **CreateOrder** händelsen, som har en egen unik händelse.</span><span class="sxs-lookup"><span data-stu-id="b0a29-128">A domain-specific service, like an order system, might have a **CreateOrder** event, which has its own unique event.</span></span> <span data-ttu-id="b0a29-129">Den här metoden kan generera många händelser och potentiellt kräver samordning av identifierare i projektgruppen.</span><span class="sxs-lookup"><span data-stu-id="b0a29-129">This approach might generate many events, and potentially require coordination of identifiers across project teams.</span></span> 

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

### <a name="using-eventsource-generically"></a><span data-ttu-id="b0a29-130">Med hjälp av EventSource Allmänt</span><span class="sxs-lookup"><span data-stu-id="b0a29-130">Using EventSource generically</span></span>

<span data-ttu-id="b0a29-131">Eftersom kan vara svårt att definiera specifika händelser, definiera många några händelser med en gemensam uppsättning parametrar som vanligtvis utdata av information som en sträng.</span><span class="sxs-lookup"><span data-stu-id="b0a29-131">Because defining specific events can be difficult, many people define a few events with a common set of parameters that generally output their information as a string.</span></span> <span data-ttu-id="b0a29-132">Mycket av hello strukturerad aspekt går förlorad och det är svårare toosearch och filtrera hello resultat.</span><span class="sxs-lookup"><span data-stu-id="b0a29-132">Much of hello structured aspect is lost, and it's more difficult toosearch and filter hello results.</span></span> <span data-ttu-id="b0a29-133">Några händelser som vanligtvis motsvarar toohello loggningsnivåer definieras i den här metoden.</span><span class="sxs-lookup"><span data-stu-id="b0a29-133">In this approach, a few events that usually correspond toohello logging levels are defined.</span></span> <span data-ttu-id="b0a29-134">hello följande fragment definierar ett felsöknings- och fel meddelande:</span><span class="sxs-lookup"><span data-stu-id="b0a29-134">hello following snippet defines a debug and error message:</span></span>

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

<span data-ttu-id="b0a29-135">Med hjälp av en kombination av strukturerade och allmänna instrumentation kan fungerar också bra.</span><span class="sxs-lookup"><span data-stu-id="b0a29-135">Using a hybrid of structured and generic instrumentation also can work well.</span></span> <span data-ttu-id="b0a29-136">Strukturerade instrumentation används för att rapportera fel och mått.</span><span class="sxs-lookup"><span data-stu-id="b0a29-136">Structured instrumentation is used for reporting errors and metrics.</span></span> <span data-ttu-id="b0a29-137">Allmänna händelser kan användas för hello detaljerad loggning som används av tekniker för felsökning.</span><span class="sxs-lookup"><span data-stu-id="b0a29-137">Generic events can be used for hello detailed logging that is consumed by engineers for troubleshooting.</span></span>

## <a name="aspnet-core-logging"></a><span data-ttu-id="b0a29-138">ASP.NET Core loggning</span><span class="sxs-lookup"><span data-stu-id="b0a29-138">ASP.NET Core logging</span></span>

<span data-ttu-id="b0a29-139">Viktiga toocarefully planera hur du kommer instrumentera din kod.</span><span class="sxs-lookup"><span data-stu-id="b0a29-139">It's important toocarefully plan how you will instrument your code.</span></span> <span data-ttu-id="b0a29-140">hello rätt instrumentation schemat kan hjälpa dig att undvika potentiellt destabilizing din kodbas och behöver tooreinstrument hello kod.</span><span class="sxs-lookup"><span data-stu-id="b0a29-140">hello right instrumentation plan can help you avoid potentially destabilizing your code base, and then needing tooreinstrument hello code.</span></span> <span data-ttu-id="b0a29-141">tooreduce risk som du kan välja en instrumentation bibliotek som [Microsoft.Extensions.Logging](https://www.nuget.org/packages/Microsoft.Extensions.Logging/), vilket är en del av Microsoft ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="b0a29-141">tooreduce risk, you can choose an instrumentation library like [Microsoft.Extensions.Logging](https://www.nuget.org/packages/Microsoft.Extensions.Logging/), which is part of Microsoft ASP.NET Core.</span></span> <span data-ttu-id="b0a29-142">ASP.NET Core har en [ILogger](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.ilogger) gränssnitt som du kan använda med hello leverantör du väljer, och minimerar hello effekt på befintlig kod.</span><span class="sxs-lookup"><span data-stu-id="b0a29-142">ASP.NET Core has an [ILogger](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.ilogger) interface that you can use with hello provider of your choice, while minimizing hello effect on existing code.</span></span> <span data-ttu-id="b0a29-143">Du kan använda hello koden i ASP.NET Core på Windows och Linux och fullständig .NET Framework i hello så instrumentation koden är standardiserad.</span><span class="sxs-lookup"><span data-stu-id="b0a29-143">You can use hello code in ASP.NET Core on Windows and Linux, and in hello full .NET Framework, so your instrumentation code is standardized.</span></span> <span data-ttu-id="b0a29-144">Detta är ytterligare utforskade nedan:</span><span class="sxs-lookup"><span data-stu-id="b0a29-144">This is further explored below:</span></span>

### <a name="using-microsoftextensionslogging-in-service-fabric"></a><span data-ttu-id="b0a29-145">Med hjälp av Microsoft.Extensions.Logging i Service Fabric</span><span class="sxs-lookup"><span data-stu-id="b0a29-145">Using Microsoft.Extensions.Logging in Service Fabric</span></span>

1. <span data-ttu-id="b0a29-146">Lägg till hello Microsoft.Extensions.Logging NuGet-paketet toohello projektet tooinstrument.</span><span class="sxs-lookup"><span data-stu-id="b0a29-146">Add hello Microsoft.Extensions.Logging NuGet package toohello project you want tooinstrument.</span></span> <span data-ttu-id="b0a29-147">Lägg även till eventuella provider-paket (ett paket från tredje part, finns i följande exempel hello).</span><span class="sxs-lookup"><span data-stu-id="b0a29-147">Also, add any provider packages (for a third-party package, see hello following example).</span></span> <span data-ttu-id="b0a29-148">Mer information finns i [loggning i ASP.NET Core](https://docs.microsoft.com/aspnet/core/fundamentals/logging).</span><span class="sxs-lookup"><span data-stu-id="b0a29-148">For more information, see [Logging in ASP.NET Core](https://docs.microsoft.com/aspnet/core/fundamentals/logging).</span></span>
2. <span data-ttu-id="b0a29-149">Lägg till en **med** direktiv för Microsoft.Extensions.Logging tooyour service-filen.</span><span class="sxs-lookup"><span data-stu-id="b0a29-149">Add a **using** directive for Microsoft.Extensions.Logging tooyour service file.</span></span>
3. <span data-ttu-id="b0a29-150">Definiera en privat variabel inom service-klassen.</span><span class="sxs-lookup"><span data-stu-id="b0a29-150">Define a private variable within your service class.</span></span>

  ```csharp
  private ILogger _logger = null;

  ```
4. <span data-ttu-id="b0a29-151">Lägg till den här koden i hello konstruktorn för tjänstklassen:</span><span class="sxs-lookup"><span data-stu-id="b0a29-151">In hello constructor of your service class, add this code:</span></span>

  ```csharp
  _logger = new LoggerFactory().CreateLogger<Stateless>();

  ```
5. <span data-ttu-id="b0a29-152">Starta instrumentering koden i din metoder.</span><span class="sxs-lookup"><span data-stu-id="b0a29-152">Start instrumenting your code in your methods.</span></span> <span data-ttu-id="b0a29-153">Här följer några exempel:</span><span class="sxs-lookup"><span data-stu-id="b0a29-153">Here are a few samples:</span></span>

  ```csharp
  _logger.LogDebug("Debug-level event from Microsoft.Logging");
  _logger.LogInformation("Informational-level event from Microsoft.Logging");

  // In this variant, we're adding structured properties RequestName and Duration, which have values MyRequest and hello duration of hello request.
  // Later in hello article, we discuss why this step is useful.
  _logger.LogInformation("{RequestName} {Duration}", "MyRequest", requestDuration);

  ```

## <a name="using-other-logging-providers"></a><span data-ttu-id="b0a29-154">Med hjälp av andra leverantörer av loggning</span><span class="sxs-lookup"><span data-stu-id="b0a29-154">Using other logging providers</span></span>

<span data-ttu-id="b0a29-155">Vissa leverantörer Använd hello-metod som beskrivs i föregående avsnitt, hello inklusive [Serilog](https://serilog.net/), [NLog](http://nlog-project.org/), och [Loggr](https://github.com/imobile3/Loggr.Extensions.Logging).</span><span class="sxs-lookup"><span data-stu-id="b0a29-155">Some third-party providers use hello approach described in hello preceding section, including [Serilog](https://serilog.net/), [NLog](http://nlog-project.org/), and [Loggr](https://github.com/imobile3/Loggr.Extensions.Logging).</span></span> <span data-ttu-id="b0a29-156">Du kan ansluta var och en av dessa i ASP.NET Core loggning eller du kan använda dem separat.</span><span class="sxs-lookup"><span data-stu-id="b0a29-156">You can plug each of these into ASP.NET Core logging, or you can use them separately.</span></span> <span data-ttu-id="b0a29-157">Serilog har en funktion som förbättra alla meddelanden från en logg.</span><span class="sxs-lookup"><span data-stu-id="b0a29-157">Serilog has a feature that enriches all messages sent from a logger.</span></span> <span data-ttu-id="b0a29-158">Den här funktionen kan vara användbar toooutput hello tjänstens namn, typ och partitionsinformation.</span><span class="sxs-lookup"><span data-stu-id="b0a29-158">This feature can be useful toooutput hello service name, type, and partition information.</span></span> <span data-ttu-id="b0a29-159">toouse den här funktionen i hello ASP.NET Core infrastruktur, utföra de här stegen:</span><span class="sxs-lookup"><span data-stu-id="b0a29-159">toouse this capability in hello ASP.NET Core infrastructure, do these steps:</span></span>

1. <span data-ttu-id="b0a29-160">Lägg till hello Serilog, Serilog.Extensions.Logging, och Serilog.Sinks.Observable NuGet-paket toohello projekt.</span><span class="sxs-lookup"><span data-stu-id="b0a29-160">Add hello Serilog, Serilog.Extensions.Logging, and Serilog.Sinks.Observable NuGet packages toohello project.</span></span> <span data-ttu-id="b0a29-161">Exempelvis hello nästa du också lägga till Serilog.Sinks.Literate.</span><span class="sxs-lookup"><span data-stu-id="b0a29-161">For hello next example, also add Serilog.Sinks.Literate.</span></span> <span data-ttu-id="b0a29-162">Bättre visas nedan.</span><span class="sxs-lookup"><span data-stu-id="b0a29-162">A better approach is shown later in this article.</span></span>
2. <span data-ttu-id="b0a29-163">I Serilog, skapar du en LoggerConfiguration och hello loggaren-instans.</span><span class="sxs-lookup"><span data-stu-id="b0a29-163">In Serilog, create a LoggerConfiguration and hello logger instance.</span></span>

  ```csharp
  Log.Logger = new LoggerConfiguration().WriteTo.LiterateConsole().CreateLogger();
  ```

3. <span data-ttu-id="b0a29-164">Lägg till en Serilog.ILogger argumentet toohello service konstruktor, och hello nyskapad meddelandeloggfiler.</span><span class="sxs-lookup"><span data-stu-id="b0a29-164">Add a Serilog.ILogger argument toohello service constructor, and pass hello newly created logger.</span></span>

  ```csharp
  ServiceRuntime.RegisterServiceAsync("StatelessType", context => new Stateless(context, Log.Logger)).GetAwaiter().GetResult();
  ```

4. <span data-ttu-id="b0a29-165">Lägga till hello efter koden som skapar hello egenskapen enrichers för hello hello service konstruktorn **ServiceTypeName**, **ServiceName**, **PartitionId**, och  **InstanceId** egenskaperna för hello-tjänst.</span><span class="sxs-lookup"><span data-stu-id="b0a29-165">In hello service constructor, add hello following code, which creates hello property enrichers for hello **ServiceTypeName**, **ServiceName**, **PartitionId**, and **InstanceId** properties of hello service.</span></span> <span data-ttu-id="b0a29-166">Det ger också en egenskap enricher toohello ASP.NET Core loggning fabriken, så att du kan använda Microsoft.Extensions.Logging.ILogger i koden.</span><span class="sxs-lookup"><span data-stu-id="b0a29-166">It also adds a property enricher toohello ASP.NET Core logging factory, so you can use Microsoft.Extensions.Logging.ILogger in your code.</span></span>

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

5. <span data-ttu-id="b0a29-167">Betalningsinstrument hello kod hello samma som om du använde ASP.NET Core utan Serilog.</span><span class="sxs-lookup"><span data-stu-id="b0a29-167">Instrument hello code hello same as if you were using ASP.NET Core without Serilog.</span></span>

  >[!NOTE]
  ><span data-ttu-id="b0a29-168">Vi rekommenderar att du inte använder hello statiskt Log.Logger med hello föregående exempel.</span><span class="sxs-lookup"><span data-stu-id="b0a29-168">We recommend that you don't use hello static Log.Logger with hello preceding example.</span></span> <span data-ttu-id="b0a29-169">Service Fabric kan vara värd för flera instanser av hello tjänsten samma typ i en enda process.</span><span class="sxs-lookup"><span data-stu-id="b0a29-169">Service Fabric can host multiple instances of hello same service type within a single process.</span></span> <span data-ttu-id="b0a29-170">Om du använder Hej statiska Log.Logger, hello senaste skrivaren av hello egenskapen enrichers visas värdena för alla instanser som körs.</span><span class="sxs-lookup"><span data-stu-id="b0a29-170">If you use hello static Log.Logger, hello last writer of hello property enrichers will show values for all instances that are running.</span></span> <span data-ttu-id="b0a29-171">Det här är ett skäl till varför hello _logger variabeln är en privat medlemsvariabel för hello tjänstklass.</span><span class="sxs-lookup"><span data-stu-id="b0a29-171">This is one reason why hello _logger variable is a private member variable of hello service class.</span></span> <span data-ttu-id="b0a29-172">Du måste också göra hello _logger tillgängliga toocommon kod, som kan användas för tjänster.</span><span class="sxs-lookup"><span data-stu-id="b0a29-172">Also, you must make hello _logger available toocommon code, which might be used across services.</span></span>

## <a name="choosing-a-logging-provider"></a><span data-ttu-id="b0a29-173">Att välja en provider för loggning</span><span class="sxs-lookup"><span data-stu-id="b0a29-173">Choosing a logging provider</span></span>

<span data-ttu-id="b0a29-174">Om programmet är beroende av hög prestanda, **EventSource** vanligtvis är en bra metod.</span><span class="sxs-lookup"><span data-stu-id="b0a29-174">If your application relies on high performance, **EventSource** is usually a good approach.</span></span> <span data-ttu-id="b0a29-175">**EventSource** *vanligtvis* använder färre resurser och presterar bättre än ASP.NET Core loggning eller någon av hello tillgängliga tredjeparts-lösningar.</span><span class="sxs-lookup"><span data-stu-id="b0a29-175">**EventSource** *generally* uses fewer resources and performs better than ASP.NET Core logging or any of hello available third-party solutions.</span></span>  <span data-ttu-id="b0a29-176">Detta är inte ett problem för många tjänster, men om tjänsten är inriktade på prestanda, med hjälp av **EventSource** kan vara ett bättre alternativ.</span><span class="sxs-lookup"><span data-stu-id="b0a29-176">This isn't an issue for many services, but if your service is performance-oriented, using **EventSource** might be a better choice.</span></span> <span data-ttu-id="b0a29-177">Dock tooget dessa fördelarna med strukturerade loggning **EventSource** kräver en större från din teknikteamet.</span><span class="sxs-lookup"><span data-stu-id="b0a29-177">However, tooget these benefits of structured logging, **EventSource** requires a larger investment from your engineering team.</span></span> <span data-ttu-id="b0a29-178">Om möjligt gör en snabb prototyp av några alternativ för loggning och välj sedan hello som bäst uppfyller dina behov.</span><span class="sxs-lookup"><span data-stu-id="b0a29-178">If possible, do a quick prototype of a few logging options, and then choose hello one that best meets your needs.</span></span>

## <a name="next-steps"></a><span data-ttu-id="b0a29-179">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="b0a29-179">Next steps</span></span>

<span data-ttu-id="b0a29-180">När du har valt dina loggning providern tooinstrument dina program och tjänster, måste din loggar och händelser toobe samman innan de kan skickas tooany analysplattform.</span><span class="sxs-lookup"><span data-stu-id="b0a29-180">Once you have chosen your logging provider tooinstrument your applications and services, your logs and events need toobe aggregated before they can be sent tooany analysis platform.</span></span> <span data-ttu-id="b0a29-181">Läs mer om [EventFlow](service-fabric-diagnostics-event-aggregation-eventflow.md) och [BOMULLSTUSS](service-fabric-diagnostics-event-aggregation-wad.md) toobetter känner till vissa av hello rekommenderat alternativ.</span><span class="sxs-lookup"><span data-stu-id="b0a29-181">Read about [EventFlow](service-fabric-diagnostics-event-aggregation-eventflow.md) and [WAD](service-fabric-diagnostics-event-aggregation-wad.md) toobetter understand some of hello recommended options.</span></span>
