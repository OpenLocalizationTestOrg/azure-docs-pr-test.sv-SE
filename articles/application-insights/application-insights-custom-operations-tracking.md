---
title: "aaaTrack anpassade åtgärder med Azure Application Insights .NET SDK | Microsoft Docs"
description: "Spårning av anpassade åtgärder med Azure Application Insights .NET SDK"
services: application-insights
documentationcenter: .net
author: SergeyKanzhelev
manager: carmonm
ms.service: application-insights
ms.workload: TBD
ms.tgt_pltfrm: ibiza
ms.devlang: multiple
ms.topic: article
ms.date: 06/31/2017
ms.author: sergkanz
ms.openlocfilehash: fe338d3e2b17a3dae43c96c60a19f57b3f46f0a5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="track-custom-operations-with-application-insights-net-sdk"></a>Spåra anpassade åtgärder med Application Insights SDK för .NET

Azure Application Insights SDK automatiskt spåra inkommande HTTP-begäranden och anropar toodependent tjänster, till exempel HTTP-begäranden och SQL-frågor. Spårning och korrelation av begäranden och beroenden ger inblick i hello hela programmet svarstider och tillförlitlighet över alla mikrotjänster som kombinerar det här programmet. 

Det finns en klass med programmet mönster som inte stöds Allmänt. Korrekt övervakning av dessa mönster kräver manuell kod instrumentation. Den här artikeln beskriver några mönster som kan kräva manuella instrumentation, till exempel anpassade kö för bearbetning och kör tidskrävande bakgrundsaktiviteter.

Det här dokumentet innehåller vägledning om hur tootrack anpassade åtgärder med hello Application Insights SDK. Den här dokumentationen är relevant för:

- Application Insights för .NET (även kallat Base SDK) version 2.4 +.
- Application Insights för web applications (kör ASP.NET) version 2.4 +.
- Application Insights för ASP.NET Core version 2.1 +.

## <a name="overview"></a>Översikt
En åtgärd är en logisk mängd arbete köras av ett program. Den har ett namn, starttid, varaktighet, resultat och en kontext för körning som användarnamn, egenskaper och resultatet. Om åtgärden A initierades av åtgärden B har sedan åtgärden B angetts som en överordnad för A. En åtgärd kan bara ha en överordnad, men den kan ha många underordnade åtgärder. Mer information om åtgärder och telemetri korrelation finns [Azure Application Insights telemetri korrelation](application-insights-correlation.md).

I hello Application Insights SDK för .NET, hello åtgärden beskrivs av hello abstrakt klass [OperationTelemetry](https://github.com/Microsoft/ApplicationInsights-dotnet/blob/develop/src/Core/Managed/Shared/Extensibility/Implementation/OperationTelemetry.cs) och dess underordnade [RequestTelemetry](https://github.com/Microsoft/ApplicationInsights-dotnet/blob/develop/src/Core/Managed/Shared/DataContracts/RequestTelemetry.cs) och [DependencyTelemetry ](https://github.com/Microsoft/ApplicationInsights-dotnet/blob/develop/src/Core/Managed/Shared/DataContracts/DependencyTelemetry.cs).

## <a name="incoming-operations-tracking"></a>Inkommande operations spårning 
Hej Application Insights web SDK samlar automatiskt in HTTP-begäranden för ASP.NET-program som körs i en IIS-pipeline och alla program som ASP.NET Core. Det finns stöd för community lösningar för andra plattformar och ramverk. Men om programmet hello inte stöds av någon av hello standard eller community stöds lösningar, kan du instrumentera det manuellt.

Ett annat exempel som kräver anpassade spårning är hello arbetare som tar emot objekt från hello kö. För vissa köer anropa hello tooadd ett meddelande spåras toothis kö som ett beroende. Dock samlas hello övergripande åtgärden som beskriver meddelandebehandling inte in automatiskt.

Nu ska vi se hur vi kan spåra dessa åtgärder.

På en hög nivå hello uppgift är toocreate `RequestTelemetry` och ange kända egenskaper. När hello-åtgärden har slutförts kan spåra du hello telemetri. hello som följande exempel visar den här uppgiften.

### <a name="http-request-in-owin-self-hosted-app"></a>HTTP-begäran i Owin egenvärdbaserat app
I det här exemplet vi följer hello [HTTP-protokollet för korrelation](https://github.com/dotnet/corefx/blob/master/src/System.Diagnostics.DiagnosticSource/src/HttpCorrelationProtocol.md). Du kan förvänta tooreceive huvuden som det beskrivs.

``` C#
public class ApplicationInsightsMiddleware : OwinMiddleware
{
    private readonly TelemetryClient telemetryClient = new TelemetryClient(TelemetryConfiguration.Active);
    
    public ApplicationInsightsMiddleware(OwinMiddleware next) : base(next) {}

    public override async Task Invoke(IOwinContext context)
    {
        // Let's create and start RequestTelemetry.
        var requestTelemetry = new RequestTelemetry
        {
            Name = $"{context.Request.Method} {context.Request.Uri.GetLeftPart(UriPartial.Path)}"
        };

        // If there is a Request-Id received from hello upstream service, set hello telemetry context accordingly.
        if (context.Request.Headers.ContainsKey("Request-Id"))
        {
            var requestId = context.Request.Headers.Get("Request-Id");
            // Get hello operation ID from hello Request-Id (if you follow hello HTTP Protocol for Correlation).
            requestTelemetry.Context.Operation.Id = GetOperationId(requestId);
            requestTelemetry.Context.Operation.ParentId = requestId;
        }

        // StartOperation is a helper method that allows correlation of 
        // current operations with nested operations/telemetry
        // and initializes start time and duration on telemetry items.
        var operation = telemetryClient.StartOperation(requestTelemetry);

        // Process hello request.
        try
        {
            await Next.Invoke(context);
        }
        catch (Exception e)
        {
            requestTelemetry.Success = false;
            telemetryClient.TrackException(e);
            throw;
        }
        finally
        {
            // Update status code and success as appropriate.
            if (context.Response != null)
            {
                requestTelemetry.ResponseCode = context.Response.StatusCode.ToString();
                requestTelemetry.Success = context.Response.StatusCode >= 200 && context.Response.StatusCode <= 299;
            }
            else
            {
                requestTelemetry.Success = false;
            }

            // Now it's time toostop hello operation (and track telemetry).
            telemetryClient.StopOperation(operation);
        }
    }
    
    public static string GetOperationId(string id)
    {
        // Returns hello root ID from hello '|' toohello first '.' if any.
        int rootEnd = id.IndexOf('.');
        if (rootEnd < 0)
            rootEnd = id.Length;

        int rootStart = id[0] == '|' ? 1 : 0;
        return id.Substring(rootStart, rootEnd - rootStart);
    }
}
```

hello HTTP-protokollet för korrelation deklarerar också hello `Correlation-Context` huvud. Dock har utelämnats här för enkelhetens skull.

## <a name="queue-instrumentation"></a>Kön instrumentation
För HTTP-kommunikation har vi skapat ett protokoll toopass korrelation information. Du kan överföra ytterligare metadata tillsammans med hello-meddelande och med andra inte med vissa köer protokoll.

### <a name="service-bus-queue"></a>Service Bus-kö
Med hello Azure [Service Bus-kö](../service-bus-messaging/index.md), du kan skicka en egenskapsuppsättning tillsammans med hello-meddelande. Vi använder toopass hello Korrelations-ID.

hello Service Bus-kö använder TCP-baserat protokoll. Application Insights automatiskt spårar inte kön åtgärder, så vi spårar dem manuellt. hello status Created åtgärden är en API för push-format och vi kan tootrack den.

#### <a name="enqueue"></a>Sätta

```C#
public async Task Enqueue(string payload)
{
    // StartOperation is a helper method that initializes hello telemetry item
    // and allows correlation of this operation with its parent and children.
    var operation = telemetryClient.StartOperation<DependencyTelemetry>("enqueue " + queueName);
    operation.Telemetry.Type = "Queue";
    operation.Telemetry.Data = "Enqueue " + queueName;

    var message = new BrokeredMessage(payload);
    // Service Bus queue allows hello property bag toopass along with hello message.
    // We will use them toopass our correlation identifiers (and other context)
    // toohello consumer.
    message.Properties.Add("ParentId", operation.Telemetry.Id);
    message.Properties.Add("RootId", operation.Telemetry.Context.Operation.Id);

    try
    {
        await queue.SendAsync(message);
        
        // Set operation.Telemetry Success and ResponseCode here.
        operation.Telemetry.Success = true;
    }
    catch (Exception e)
    {
        telemetryClient.TrackException(e);
        // Set operation.Telemetry Success and ResponseCode here.
        operation.Telemetry.Success = false;
        throw;
    }
    finally
    {
        telemetryClient.StopOperation(operation);
    }
}
```

#### <a name="process"></a>Processen
```C#
public async Task Process(BrokeredMessage message)
{
    // After hello message is taken from hello queue, create RequestTelemetry tootrack its processing.
    // It might also make sense tooget hello name from hello message.
    RequestTelemetry requestTelemetry = new RequestTelemetry { Name = "Dequeue " + queueName };

    var rootId = message.Properties["RootId"].ToString();
    var parentId = message.Properties["ParentId"].ToString();
    // Get hello operation ID from hello Request-Id (if you follow hello HTTP Protocol for Correlation).
    requestTelemetry.Context.Operation.Id = rootId;
    requestTelemetry.Context.Operation.ParentId = parentId;

    var operation = telemetryClient.StartOperation(requestTelemetry);

    try
    {
        await ProcessMessage();
    }
    catch (Exception e)
    {
        telemetryClient.TrackException(e);
        throw;
    }
    finally
    {
        // Update status code and success as appropriate.
        telemetryClient.StopOperation(operation);
    }
}
```

### <a name="azure-storage-queue"></a>Azure Storage-kö
följande exempel visar hur hello tootrack hello [Azure Storage-kö](../storage/queues/storage-dotnet-how-to-use-queues.md) åtgärder och korrelera telemetri mellan hello producenten, hello konsumenten och Azure Storage. 

hello lagring kön har en HTTP-API. Alla anrop toohello kön spåras av hello Application Insights beroende insamlare för HTTP-begäranden.
Kontrollera att du har `Microsoft.ApplicationInsights.DependencyCollector.HttpDependenciesParsingTelemetryInitializer` i `applicationInsights.config`. Om du inte har det, lägga till den programmässigt enligt beskrivningen i [filtrering och förbearbetning i hello Azure Application Insights SDK](app-insights-api-filtering-sampling.md).

Om du konfigurerar Application Insights manuellt, kontrollera att du kan skapa och initiera `Microsoft.ApplicationInsights.DependencyCollector.DependencyTrackingTelemetryModule` på samma sätt som:
 
``` C#
DependencyTrackingTelemetryModule module = new DependencyTrackingTelemetryModule();

// You can prevent correlation header injection toosome domains by adding it toohello excluded list.
// Make sure you add a Storage endpoint. Otherwise, you might experience request signature validation issues on hello Storage service side.
module.ExcludeComponentCorrelationHttpHeadersOnDomains.Add("core.windows.net");
module.Initialize(TelemetryConfiguration.Active);

// Do not forget toodispose of hello module during application shutdown.
```

Du kan också toocorrelate hello Application Insights-åtgärds-ID med hello lagring begärande-ID. Mer information om hur tooset och hämta en begäran klient och server begäran-ID finns [övervaka, diagnostisera och felsöka Azure Storage](../storage/common/storage-monitoring-diagnosing-troubleshooting.md#end-to-end-tracing).

#### <a name="enqueue"></a>Sätta
Eftersom lagringsköer stöder hello http-API, spåras alla åtgärder med hello kön automatiskt av Application Insights. I många fall kan borde den här instrumentation räcka. Dock toocorrelate spårar på hello konsumenten sida med producenten spårningar, du måste skicka en del kontext för korrelation på samma sätt toohow vi göra det i hello HTTP-protokollet för korrelation. 

I det här exemplet vi spårar hello valfria `Enqueue` igen. Du kan:

 - **Korrelera återförsök (eventuella)**: alla har en gemensam överordnad som är hello `Enqueue` igen. De är också spåras som underordnade till hello inkommande begäran. Om det finns flera begäranden logiskt toohello kön, kanske svårt toofind som samtalet resulterade i återförsök.
 - **Korrelera lagring loggar (om och vid behov)**: de är korrelerad med Application Insights telemetry.

Hej `Enqueue` åtgärden är hello underordnad till en överordnad-åtgärd (till exempel en inkommande HTTP-begäran). hello HTTP beroendeanropet är hello underordnad hello `Enqueue` åtgärden och hello två nivåer under för hello inkommande begäran:

```C#
public async Task Enqueue(CloudQueue queue, string message)
{
    var operation = telemetryClient.StartOperation<DependencyTelemetry>("enqueue " + queue.Name);
    operation.Telemetry.Type = "Queue";
    operation.Telemetry.Data = "Enqueue " + queue.Name;

    // MessagePayload represents your custom message and also serializes correlation identifiers into payload.
    // For example, if you choose toopass payload serialized tooJSON, it might look like
    // {'RootId' : 'some-id', 'ParentId' : '|some-id.1.2.3.', 'message' : 'your message tooprocess'}
    var jsonPayload = JsonConvert.SerializeObject(new MessagePayload
    {
        RootId = operation.Telemetry.Context.Operation.Id,
        ParentId = operation.Telemetry.Id,
        Payload = message
    });
    
    CloudQueueMessage queueMessage = new CloudQueueMessage(jsonPayload);

    // Add operation.Telemetry.Id toohello OperationContext toocorrelate Storage logs and Application Insights telemetry.
    OperationContext context = new OperationContext { ClientRequestID = operation.Telemetry.Id};

    try
    {
        await queue.AddMessageAsync(queueMessage, null, null, new QueueRequestOptions(), context);
    }
    catch (StorageException e)
    {
        operation.Telemetry.Properties.Add("AzureServiceRequestID", e.RequestInformation.ServiceRequestID);
        operation.Telemetry.Success = false;
        operation.Telemetry.ResultCode = e.RequestInformation.HttpStatusCode.ToString();
        telemetryClient.TrackException(e);
    }
    finally
    {
        // Update status code and success as appropriate.
        telemetryClient.StopOperation(operation);
    }
}  
```

rapporter om tooreduce hello mängden telemetri tillämpningsprogrammet eller om du inte vill tootrack hello `Enqueue` åtgärd för andra orsaker, Använd hello `Activity` API direkt:

- Skapa (och starta) en ny `Activity` i stället för att starta hello Application Insights-åtgärden. Du gör *inte* måste tooassign egenskaper på den utom hello åtgärdsnamn.
- Serialisera `yourActivity.Id` till nyttolast för hello-meddelande i stället för `operation.Telemetry.Id`. Du kan också använda `Activity.Current.Id`.


#### <a name="dequeue"></a>Status Created
På samma sätt för`Enqueue`, en faktiska HTTP toohello lagring begärandekö spåras automatiskt av Application Insights. Dock hello `Enqueue` åtgärden förmodligen sker i hello överordnade kontext, till exempel en inkommande begäran-kontext. Application Insights SDK automatiskt korrelera sådan åtgärd (och dess HTTP-del) med hello överordnade begäran och andra telemetri som rapporterats i hello samma omfång.

Hej `Dequeue` åtgärden är svår. hello Application Insights SDK spårar automatiskt HTTP-begäranden. Men vet den inte hello korrelation kontext förrän tolkas hello-meddelande. Det är inte möjligt toocorrelate hello HTTP-begäran tooget hello-meddelande med hello resten av hello telemetri.

I många fall kan vara det användbart toocorrelate hello HTTP-begäran toohello kö med andra spår samt. hello exemplet nedan visar hur toodo den:

``` C#
public async Task<MessagePayload> Dequeue(CloudQueue queue)
{
    var telemetry = new DependencyTelemetry
    {
        Type = "Queue",
        Name = "Dequeue " + queue.Name
    };

    telemetry.Start();

    try
    {
        var message = await queue.GetMessageAsync();

        if (message != null)
        {
            var payload = JsonConvert.DeserializeObject<MessagePayload>(message.AsString);

            // If there is a message, we want toocorrelate hello Dequeue operation with processing.
            // However, we will only know what correlation ID toouse after we get it from hello message,
            // so we will report telemetry after we know hello IDs.
            telemetry.Context.Operation.Id = payload.RootId;
            telemetry.Context.Operation.ParentId = payload.ParentId;

            // Delete hello message.
            return payload;
        }
    }
    catch (StorageException e)
    {
        telemetry.Properties.Add("AzureServiceRequestID", e.RequestInformation.ServiceRequestID);
        telemetry.Success = false;
        telemetry.ResultCode = e.RequestInformation.HttpStatusCode.ToString();
        telemetryClient.TrackException(e);
    }
    finally
    {
        // Update status code and success as appropriate.
        telemetry.Stop();
        telemetryClient.Track(telemetry);
    }

    return null;
}
```

#### <a name="process"></a>Processen

I följande exempel hello, vi spåra ett inkommande meddelande på ett sätt på samma sätt toohow vi spåra en inkommande HTTP för begäran:

```C#
public async Task Process(MessagePayload message)
{
    // After hello message is dequeued from hello queue, create RequestTelemetry tootrack its processing.
    RequestTelemetry requestTelemetry = new RequestTelemetry { Name = "Dequeue " + queueName };
    // It might also make sense tooget hello name from hello message.
    requestTelemetry.Context.Operation.Id = message.RootId;
    requestTelemetry.Context.Operation.ParentId = message.ParentId;

    var operation = telemetryClient.StartOperation(requestTelemetry);

    try
    {
        await ProcessMessage();
    }
    catch (Exception e)
    {
        telemetryClient.TrackException(e);
        throw;
    }
    finally
    {
        // Update status code and success as appropriate.
        telemetryClient.StopOperation(operation);
    }
}
```

På liknande sätt kan andra köåtgärder instrumenterats. En titt åtgärd bör instrumenterats på ett liknande sätt som en dequeue-åtgärd. Instrumentering kön hanteringsåtgärder är inte nödvändigt. Application Insights spårar åtgärder, till exempel HTTP och i de flesta fall är tillräckliga.

När du instrumentera borttagning av meddelanden måste du ange hello åtgärden (korrelation) identifierare. Du kan också använda hello `Activity` API. Sedan behöver du inte tooset åtgärden identifierare på hello telemetri objekt eftersom Application Insights gör det:

- Skapa en ny `Activity` när du har installerat ett objekt från hello kö.
- Använd `Activity.SetParentId(message.ParentId)` toocorrelate konsumenten och producenten loggar.
- Starta hello `Activity`.
- Spåra status Created bearbeta och delete-åtgärder med hjälp av `Start/StopOperation` hjälpprogram. Göra det från hello samma asynkrona Kontrollflöde (körningskontexten). På så sätt är de korrelerade korrekt.
- Stoppa hello `Activity`.
- Använd `Start/StopOperation`, eller anropa `Track` telemetri manuellt.

### <a name="batch-processing"></a>Batch-bearbetning
Med vissa köer du status Created flera meddelanden med en begäran. Bearbeta sådana meddelanden är förmodligen oberoende och tillhör toohello olika logiska åtgärder. I detta fall det inte är möjligt toocorrelate hello `Dequeue` åtgärden tooparticular meddelandebehandling.

Varje meddelande som ska bearbetas i sin egen asynkron Kontrollflöde. Mer information finns i hello [utgående beroenden spårning](#outgoing-dependencies-tracking) avsnitt.

## <a name="long-running-background-tasks"></a>Långvariga bakgrundsaktiviteter
Vissa program startar långvariga åtgärder som kan orsakas av användarförfrågningar. Ur hello spårning/instrumentation skiljer den sig inte från begäran eller beroende: 

``` C#
async Task BackgroundTask()
{
    var operation = telemetryClient.StartOperation<RequestTelemetry>(taskName);
    operation.Telemetry.Type = "Background";
    try
    {
        int progress = 0;
        while (progress < 100)
        {
            // Process hello task.
            telemetryClient.TrackTrace($"done {progress++}%");
        }
        // Update status code and success as appropriate.
    }
    catch (Exception e)
    {
        telemetryClient.TrackException(e);
        // Update status code and success as appropriate.
        throw;
    }
    finally
    {
        telemetryClient.StopOperation(operation);
    }
}
```

I det här exemplet använder vi `telemetryClient.StartOperation` toocreate `RequestTelemetry` och fill hello korrelation kontext. Anta att du har en överordnad åtgärd som har skapats av inkommande begäranden som schemalagda hello igen. Så länge `BackgroundTask` startar i Hej samma asynkrona Kontrollflöde som en inkommande begäran, den korreleras med den överordnade igen. `BackgroundTask`och alla kapslade telemetri objekt automatiskt kopplas ihop med hello-begäran som orsakade det, även när hello begäran avslutas.

När hello aktiviteten startar från hello bakgrundstråd som inte har någon åtgärd (`Activity`) som är kopplade till den, `BackgroundTask` inte har någon överordnad. Det kan dock ha kapslade åtgärder. Alla artiklar för telemetri som rapporterats från hello aktiviteten är korrelerade toohello `RequestTelemetry` skapas i `BackgroundTask`.

## <a name="outgoing-dependencies-tracking"></a>Utgående beroenden spårning
Du kan spåra dina egna beroende typ eller en åtgärd som inte stöds av Application Insights.

Hej `Enqueue` metod i hello Service Bus-kö eller hello lagring kö kan fungera som exempel på sådana anpassade spårning.

hello allmänna riktlinjer för anpassade beroende spårning är att:

- Anropa hello `TelemetryClient.StartOperation` (tillägg)-metod som fyller hello `DependencyTelemetry` egenskaper som krävs för korrelation och vissa andra egenskaper (starttid stämpel, varaktighet).
- Ange andra anpassade egenskaper för hello `DependencyTelemetry`, till exempel hello namn och en kontext som du behöver.
- Se ett beroende anropa och vänta tills den.
- Stoppa hello igen med `StopOperation` när den är klar.
- Hantera undantag.

`StopOperation`endast avbryts hello som startades. Om hello aktuella körs åtgärden inte matchar hello en du vill toostop, `StopOperation` händer ingenting. Den här situationen kan inträffa om du startar flera åtgärder parallellt i hello samma körningskontexten:

```C#
var firstOperation = telemetryClient.StartOperation<DependencyTelemetry>("task 1");
var firstOperation = telemetryClient.StartOperation<DependencyTelemetry>("task 1");
var firstTask = RunMyTaskAsync();

var secondOperation = telemetryClient.StartOperation<DependencyTelemetry>("task 2");
var secondTask = RunMyTaskAsync();

await firstTask;

// This will do nothing and will not report telemetry for hello first operation
// as currently secondOperation is active.
telemetryClient.StopOperation(firstOperation); 

await secondTask;
```

Se till att du alltid anropa `StartOperation` och kör uppgiften i sin egen kontext:
```C#
public async Task RunMyTaskAsync()
{
    var operation = telemetryClient.StartOperation<DependencyTelemetry>("task 1");
    try 
    {
        var myTask = await StartMyTaskAsync();
        // Update status code and success as appropriate.
    }
    catch(...) 
    {
        // Update status code and success as appropriate.
    }
    finally 
    {
        telemetryClient.StopOperation(operation);
    }
}
```

## <a name="next-steps"></a>Nästa steg

- Hello grundläggande information om hur [telemetri korrelation](application-insights-correlation.md) i Application Insights.
- Se hello [datamodellen](application-insights-data-model.md) för Application Insights typer och modell.
- Rapportera anpassad [händelser och mått](app-insights-api-custom-events-metrics.md) tooApplication insikter.
- Kolla standard [configuration](app-insights-configuration-with-applicationinsights-config.md#telemetry-initializers-aspnet) för kontexten egenskapssamling.
- Kontrollera hello [System.Diagnostics.Activity användarhandboken](https://github.com/dotnet/corefx/blob/master/src/System.Diagnostics.DiagnosticSource/src/ActivityUserGuide.md) toosee hur vi korrelera telemetri.
