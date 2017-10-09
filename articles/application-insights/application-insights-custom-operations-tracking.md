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
# <a name="track-custom-operations-with-application-insights-net-sdk"></a><span data-ttu-id="ce792-103">Spåra anpassade åtgärder med Application Insights SDK för .NET</span><span class="sxs-lookup"><span data-stu-id="ce792-103">Track custom operations with Application Insights .NET SDK</span></span>

<span data-ttu-id="ce792-104">Azure Application Insights SDK automatiskt spåra inkommande HTTP-begäranden och anropar toodependent tjänster, till exempel HTTP-begäranden och SQL-frågor.</span><span class="sxs-lookup"><span data-stu-id="ce792-104">Azure Application Insights SDKs automatically track incoming HTTP requests and calls toodependent services, such as HTTP requests and SQL queries.</span></span> <span data-ttu-id="ce792-105">Spårning och korrelation av begäranden och beroenden ger inblick i hello hela programmet svarstider och tillförlitlighet över alla mikrotjänster som kombinerar det här programmet.</span><span class="sxs-lookup"><span data-stu-id="ce792-105">Tracking and correlation of requests and dependencies give you visibility into hello whole application's responsiveness and reliability across all microservices that combine this application.</span></span> 

<span data-ttu-id="ce792-106">Det finns en klass med programmet mönster som inte stöds Allmänt.</span><span class="sxs-lookup"><span data-stu-id="ce792-106">There is a class of application patterns that can't be supported generically.</span></span> <span data-ttu-id="ce792-107">Korrekt övervakning av dessa mönster kräver manuell kod instrumentation.</span><span class="sxs-lookup"><span data-stu-id="ce792-107">Proper monitoring of such patterns requires manual code instrumentation.</span></span> <span data-ttu-id="ce792-108">Den här artikeln beskriver några mönster som kan kräva manuella instrumentation, till exempel anpassade kö för bearbetning och kör tidskrävande bakgrundsaktiviteter.</span><span class="sxs-lookup"><span data-stu-id="ce792-108">This article covers a few patterns that might require manual instrumentation, such as custom queue processing and running long-running background tasks.</span></span>

<span data-ttu-id="ce792-109">Det här dokumentet innehåller vägledning om hur tootrack anpassade åtgärder med hello Application Insights SDK.</span><span class="sxs-lookup"><span data-stu-id="ce792-109">This document provides guidance on how tootrack custom operations with hello Application Insights SDK.</span></span> <span data-ttu-id="ce792-110">Den här dokumentationen är relevant för:</span><span class="sxs-lookup"><span data-stu-id="ce792-110">This documentation is relevant for:</span></span>

- <span data-ttu-id="ce792-111">Application Insights för .NET (även kallat Base SDK) version 2.4 +.</span><span class="sxs-lookup"><span data-stu-id="ce792-111">Application Insights for .NET (also known as Base SDK) version 2.4+.</span></span>
- <span data-ttu-id="ce792-112">Application Insights för web applications (kör ASP.NET) version 2.4 +.</span><span class="sxs-lookup"><span data-stu-id="ce792-112">Application Insights for web applications (running ASP.NET) version 2.4+.</span></span>
- <span data-ttu-id="ce792-113">Application Insights för ASP.NET Core version 2.1 +.</span><span class="sxs-lookup"><span data-stu-id="ce792-113">Application Insights for ASP.NET Core version 2.1+.</span></span>

## <a name="overview"></a><span data-ttu-id="ce792-114">Översikt</span><span class="sxs-lookup"><span data-stu-id="ce792-114">Overview</span></span>
<span data-ttu-id="ce792-115">En åtgärd är en logisk mängd arbete köras av ett program.</span><span class="sxs-lookup"><span data-stu-id="ce792-115">An operation is a logical piece of work run by an application.</span></span> <span data-ttu-id="ce792-116">Den har ett namn, starttid, varaktighet, resultat och en kontext för körning som användarnamn, egenskaper och resultatet.</span><span class="sxs-lookup"><span data-stu-id="ce792-116">It has a name, start time, duration, result, and a context of execution like user name, properties, and result.</span></span> <span data-ttu-id="ce792-117">Om åtgärden A initierades av åtgärden B har sedan åtgärden B angetts som en överordnad för A. En åtgärd kan bara ha en överordnad, men den kan ha många underordnade åtgärder.</span><span class="sxs-lookup"><span data-stu-id="ce792-117">If operation A was initiated by operation B, then operation B is set as a parent for A. An operation can have only one parent, but it can have many child operations.</span></span> <span data-ttu-id="ce792-118">Mer information om åtgärder och telemetri korrelation finns [Azure Application Insights telemetri korrelation](application-insights-correlation.md).</span><span class="sxs-lookup"><span data-stu-id="ce792-118">For more information on operations and telemetry correlation, see [Azure Application Insights telemetry correlation](application-insights-correlation.md).</span></span>

<span data-ttu-id="ce792-119">I hello Application Insights SDK för .NET, hello åtgärden beskrivs av hello abstrakt klass [OperationTelemetry](https://github.com/Microsoft/ApplicationInsights-dotnet/blob/develop/src/Core/Managed/Shared/Extensibility/Implementation/OperationTelemetry.cs) och dess underordnade [RequestTelemetry](https://github.com/Microsoft/ApplicationInsights-dotnet/blob/develop/src/Core/Managed/Shared/DataContracts/RequestTelemetry.cs) och [DependencyTelemetry ](https://github.com/Microsoft/ApplicationInsights-dotnet/blob/develop/src/Core/Managed/Shared/DataContracts/DependencyTelemetry.cs).</span><span class="sxs-lookup"><span data-stu-id="ce792-119">In hello Application Insights .NET SDK, hello operation is described by hello abstract class [OperationTelemetry](https://github.com/Microsoft/ApplicationInsights-dotnet/blob/develop/src/Core/Managed/Shared/Extensibility/Implementation/OperationTelemetry.cs) and its descendants [RequestTelemetry](https://github.com/Microsoft/ApplicationInsights-dotnet/blob/develop/src/Core/Managed/Shared/DataContracts/RequestTelemetry.cs) and [DependencyTelemetry](https://github.com/Microsoft/ApplicationInsights-dotnet/blob/develop/src/Core/Managed/Shared/DataContracts/DependencyTelemetry.cs).</span></span>

## <a name="incoming-operations-tracking"></a><span data-ttu-id="ce792-120">Inkommande operations spårning</span><span class="sxs-lookup"><span data-stu-id="ce792-120">Incoming operations tracking</span></span> 
<span data-ttu-id="ce792-121">Hej Application Insights web SDK samlar automatiskt in HTTP-begäranden för ASP.NET-program som körs i en IIS-pipeline och alla program som ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="ce792-121">hello Application Insights web SDK automatically collects HTTP requests for ASP.NET applications that run in an IIS pipeline and all ASP.NET Core applications.</span></span> <span data-ttu-id="ce792-122">Det finns stöd för community lösningar för andra plattformar och ramverk.</span><span class="sxs-lookup"><span data-stu-id="ce792-122">There are community-supported solutions for other platforms and frameworks.</span></span> <span data-ttu-id="ce792-123">Men om programmet hello inte stöds av någon av hello standard eller community stöds lösningar, kan du instrumentera det manuellt.</span><span class="sxs-lookup"><span data-stu-id="ce792-123">However, if hello application isn't supported by any of hello standard or community-supported solutions, you can instrument it manually.</span></span>

<span data-ttu-id="ce792-124">Ett annat exempel som kräver anpassade spårning är hello arbetare som tar emot objekt från hello kö.</span><span class="sxs-lookup"><span data-stu-id="ce792-124">Another example that requires custom tracking is hello worker that receives items from hello queue.</span></span> <span data-ttu-id="ce792-125">För vissa köer anropa hello tooadd ett meddelande spåras toothis kö som ett beroende.</span><span class="sxs-lookup"><span data-stu-id="ce792-125">For some queues, hello call tooadd a message toothis queue is tracked as a dependency.</span></span> <span data-ttu-id="ce792-126">Dock samlas hello övergripande åtgärden som beskriver meddelandebehandling inte in automatiskt.</span><span class="sxs-lookup"><span data-stu-id="ce792-126">However, hello high-level operation that describes message processing is not automatically collected.</span></span>

<span data-ttu-id="ce792-127">Nu ska vi se hur vi kan spåra dessa åtgärder.</span><span class="sxs-lookup"><span data-stu-id="ce792-127">Let's see how we can track such operations.</span></span>

<span data-ttu-id="ce792-128">På en hög nivå hello uppgift är toocreate `RequestTelemetry` och ange kända egenskaper.</span><span class="sxs-lookup"><span data-stu-id="ce792-128">On a high level, hello task is toocreate `RequestTelemetry` and set known properties.</span></span> <span data-ttu-id="ce792-129">När hello-åtgärden har slutförts kan spåra du hello telemetri.</span><span class="sxs-lookup"><span data-stu-id="ce792-129">After hello operation is finished, you track hello telemetry.</span></span> <span data-ttu-id="ce792-130">hello som följande exempel visar den här uppgiften.</span><span class="sxs-lookup"><span data-stu-id="ce792-130">hello following example demonstrates this task.</span></span>

### <a name="http-request-in-owin-self-hosted-app"></a><span data-ttu-id="ce792-131">HTTP-begäran i Owin egenvärdbaserat app</span><span class="sxs-lookup"><span data-stu-id="ce792-131">HTTP request in Owin self-hosted app</span></span>
<span data-ttu-id="ce792-132">I det här exemplet vi följer hello [HTTP-protokollet för korrelation](https://github.com/dotnet/corefx/blob/master/src/System.Diagnostics.DiagnosticSource/src/HttpCorrelationProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="ce792-132">In this example, we follow hello [HTTP Protocol for Correlation](https://github.com/dotnet/corefx/blob/master/src/System.Diagnostics.DiagnosticSource/src/HttpCorrelationProtocol.md).</span></span> <span data-ttu-id="ce792-133">Du kan förvänta tooreceive huvuden som det beskrivs.</span><span class="sxs-lookup"><span data-stu-id="ce792-133">You should expect tooreceive headers that are described there.</span></span>

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

<span data-ttu-id="ce792-134">hello HTTP-protokollet för korrelation deklarerar också hello `Correlation-Context` huvud.</span><span class="sxs-lookup"><span data-stu-id="ce792-134">hello HTTP Protocol for Correlation also declares hello `Correlation-Context` header.</span></span> <span data-ttu-id="ce792-135">Dock har utelämnats här för enkelhetens skull.</span><span class="sxs-lookup"><span data-stu-id="ce792-135">However, it's omitted here for simplicity.</span></span>

## <a name="queue-instrumentation"></a><span data-ttu-id="ce792-136">Kön instrumentation</span><span class="sxs-lookup"><span data-stu-id="ce792-136">Queue instrumentation</span></span>
<span data-ttu-id="ce792-137">För HTTP-kommunikation har vi skapat ett protokoll toopass korrelation information.</span><span class="sxs-lookup"><span data-stu-id="ce792-137">For HTTP communication, we've created a protocol toopass correlation details.</span></span> <span data-ttu-id="ce792-138">Du kan överföra ytterligare metadata tillsammans med hello-meddelande och med andra inte med vissa köer protokoll.</span><span class="sxs-lookup"><span data-stu-id="ce792-138">With some queues' protocols, you can pass additional metadata along with hello message, and with others you can't.</span></span>

### <a name="service-bus-queue"></a><span data-ttu-id="ce792-139">Service Bus-kö</span><span class="sxs-lookup"><span data-stu-id="ce792-139">Service Bus queue</span></span>
<span data-ttu-id="ce792-140">Med hello Azure [Service Bus-kö](../service-bus-messaging/index.md), du kan skicka en egenskapsuppsättning tillsammans med hello-meddelande.</span><span class="sxs-lookup"><span data-stu-id="ce792-140">With hello Azure [Service Bus queue](../service-bus-messaging/index.md), you can pass a property bag along with hello message.</span></span> <span data-ttu-id="ce792-141">Vi använder toopass hello Korrelations-ID.</span><span class="sxs-lookup"><span data-stu-id="ce792-141">We use it toopass hello correlation ID.</span></span>

<span data-ttu-id="ce792-142">hello Service Bus-kö använder TCP-baserat protokoll.</span><span class="sxs-lookup"><span data-stu-id="ce792-142">hello Service Bus queue uses TCP-based protocols.</span></span> <span data-ttu-id="ce792-143">Application Insights automatiskt spårar inte kön åtgärder, så vi spårar dem manuellt.</span><span class="sxs-lookup"><span data-stu-id="ce792-143">Application Insights doesn't automatically track queue operations, so we track them manually.</span></span> <span data-ttu-id="ce792-144">hello status Created åtgärden är en API för push-format och vi kan tootrack den.</span><span class="sxs-lookup"><span data-stu-id="ce792-144">hello dequeue operation is a push-style API, and we're unable tootrack it.</span></span>

#### <a name="enqueue"></a><span data-ttu-id="ce792-145">Sätta</span><span class="sxs-lookup"><span data-stu-id="ce792-145">Enqueue</span></span>

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

#### <a name="process"></a><span data-ttu-id="ce792-146">Processen</span><span class="sxs-lookup"><span data-stu-id="ce792-146">Process</span></span>
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

### <a name="azure-storage-queue"></a><span data-ttu-id="ce792-147">Azure Storage-kö</span><span class="sxs-lookup"><span data-stu-id="ce792-147">Azure Storage queue</span></span>
<span data-ttu-id="ce792-148">följande exempel visar hur hello tootrack hello [Azure Storage-kö](../storage/queues/storage-dotnet-how-to-use-queues.md) åtgärder och korrelera telemetri mellan hello producenten, hello konsumenten och Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="ce792-148">hello following example shows how tootrack hello [Azure Storage queue](../storage/queues/storage-dotnet-how-to-use-queues.md) operations and correlate telemetry between hello producer, hello consumer, and Azure Storage.</span></span> 

<span data-ttu-id="ce792-149">hello lagring kön har en HTTP-API.</span><span class="sxs-lookup"><span data-stu-id="ce792-149">hello Storage queue has an HTTP API.</span></span> <span data-ttu-id="ce792-150">Alla anrop toohello kön spåras av hello Application Insights beroende insamlare för HTTP-begäranden.</span><span class="sxs-lookup"><span data-stu-id="ce792-150">All calls toohello queue are tracked by hello Application Insights Dependency Collector for HTTP requests.</span></span>
<span data-ttu-id="ce792-151">Kontrollera att du har `Microsoft.ApplicationInsights.DependencyCollector.HttpDependenciesParsingTelemetryInitializer` i `applicationInsights.config`.</span><span class="sxs-lookup"><span data-stu-id="ce792-151">Make sure you have `Microsoft.ApplicationInsights.DependencyCollector.HttpDependenciesParsingTelemetryInitializer` in `applicationInsights.config`.</span></span> <span data-ttu-id="ce792-152">Om du inte har det, lägga till den programmässigt enligt beskrivningen i [filtrering och förbearbetning i hello Azure Application Insights SDK](app-insights-api-filtering-sampling.md).</span><span class="sxs-lookup"><span data-stu-id="ce792-152">If you don't have it, add it programmatically as described in [Filtering and Preprocessing in hello Azure Application Insights SDK](app-insights-api-filtering-sampling.md).</span></span>

<span data-ttu-id="ce792-153">Om du konfigurerar Application Insights manuellt, kontrollera att du kan skapa och initiera `Microsoft.ApplicationInsights.DependencyCollector.DependencyTrackingTelemetryModule` på samma sätt som:</span><span class="sxs-lookup"><span data-stu-id="ce792-153">If you configure Application Insights manually, make sure you create and initialize `Microsoft.ApplicationInsights.DependencyCollector.DependencyTrackingTelemetryModule` similarly to:</span></span>
 
``` C#
DependencyTrackingTelemetryModule module = new DependencyTrackingTelemetryModule();

// You can prevent correlation header injection toosome domains by adding it toohello excluded list.
// Make sure you add a Storage endpoint. Otherwise, you might experience request signature validation issues on hello Storage service side.
module.ExcludeComponentCorrelationHttpHeadersOnDomains.Add("core.windows.net");
module.Initialize(TelemetryConfiguration.Active);

// Do not forget toodispose of hello module during application shutdown.
```

<span data-ttu-id="ce792-154">Du kan också toocorrelate hello Application Insights-åtgärds-ID med hello lagring begärande-ID.</span><span class="sxs-lookup"><span data-stu-id="ce792-154">You also might want toocorrelate hello Application Insights operation ID with hello Storage request ID.</span></span> <span data-ttu-id="ce792-155">Mer information om hur tooset och hämta en begäran klient och server begäran-ID finns [övervaka, diagnostisera och felsöka Azure Storage](../storage/common/storage-monitoring-diagnosing-troubleshooting.md#end-to-end-tracing).</span><span class="sxs-lookup"><span data-stu-id="ce792-155">For information on how tooset and get a Storage request client and a server request ID, see [Monitor, diagnose, and troubleshoot Azure Storage](../storage/common/storage-monitoring-diagnosing-troubleshooting.md#end-to-end-tracing).</span></span>

#### <a name="enqueue"></a><span data-ttu-id="ce792-156">Sätta</span><span class="sxs-lookup"><span data-stu-id="ce792-156">Enqueue</span></span>
<span data-ttu-id="ce792-157">Eftersom lagringsköer stöder hello http-API, spåras alla åtgärder med hello kön automatiskt av Application Insights.</span><span class="sxs-lookup"><span data-stu-id="ce792-157">Because Storage queues support hello HTTP API, all operations with hello queue are automatically tracked by Application Insights.</span></span> <span data-ttu-id="ce792-158">I många fall kan borde den här instrumentation räcka.</span><span class="sxs-lookup"><span data-stu-id="ce792-158">In many cases, this instrumentation should be enough.</span></span> <span data-ttu-id="ce792-159">Dock toocorrelate spårar på hello konsumenten sida med producenten spårningar, du måste skicka en del kontext för korrelation på samma sätt toohow vi göra det i hello HTTP-protokollet för korrelation.</span><span class="sxs-lookup"><span data-stu-id="ce792-159">However, toocorrelate traces on hello consumer side with producer traces, you must pass some correlation context similarly toohow we do it in hello HTTP Protocol for Correlation.</span></span> 

<span data-ttu-id="ce792-160">I det här exemplet vi spårar hello valfria `Enqueue` igen.</span><span class="sxs-lookup"><span data-stu-id="ce792-160">In this example, we track hello optional `Enqueue` operation.</span></span> <span data-ttu-id="ce792-161">Du kan:</span><span class="sxs-lookup"><span data-stu-id="ce792-161">You can:</span></span>

 - <span data-ttu-id="ce792-162">**Korrelera återförsök (eventuella)**: alla har en gemensam överordnad som är hello `Enqueue` igen.</span><span class="sxs-lookup"><span data-stu-id="ce792-162">**Correlate retries (if any)**: They all have one common parent that's hello `Enqueue` operation.</span></span> <span data-ttu-id="ce792-163">De är också spåras som underordnade till hello inkommande begäran.</span><span class="sxs-lookup"><span data-stu-id="ce792-163">Otherwise, they're tracked as children of hello incoming request.</span></span> <span data-ttu-id="ce792-164">Om det finns flera begäranden logiskt toohello kön, kanske svårt toofind som samtalet resulterade i återförsök.</span><span class="sxs-lookup"><span data-stu-id="ce792-164">If there are multiple logical requests toohello queue, it might be difficult toofind which call resulted in retries.</span></span>
 - <span data-ttu-id="ce792-165">**Korrelera lagring loggar (om och vid behov)**: de är korrelerad med Application Insights telemetry.</span><span class="sxs-lookup"><span data-stu-id="ce792-165">**Correlate Storage logs (if and when needed)**: They're correlated with Application Insights telemetry.</span></span>

<span data-ttu-id="ce792-166">Hej `Enqueue` åtgärden är hello underordnad till en överordnad-åtgärd (till exempel en inkommande HTTP-begäran).</span><span class="sxs-lookup"><span data-stu-id="ce792-166">hello `Enqueue` operation is hello child of a parent operation (for example, an incoming HTTP request).</span></span> <span data-ttu-id="ce792-167">hello HTTP beroendeanropet är hello underordnad hello `Enqueue` åtgärden och hello två nivåer under för hello inkommande begäran:</span><span class="sxs-lookup"><span data-stu-id="ce792-167">hello HTTP dependency call is hello child of hello `Enqueue` operation and hello grandchild of hello incoming request:</span></span>

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

<span data-ttu-id="ce792-168">rapporter om tooreduce hello mängden telemetri tillämpningsprogrammet eller om du inte vill tootrack hello `Enqueue` åtgärd för andra orsaker, Använd hello `Activity` API direkt:</span><span class="sxs-lookup"><span data-stu-id="ce792-168">tooreduce hello amount of telemetry your application reports or if you don't want tootrack hello `Enqueue` operation for other reasons, use hello `Activity` API directly:</span></span>

- <span data-ttu-id="ce792-169">Skapa (och starta) en ny `Activity` i stället för att starta hello Application Insights-åtgärden.</span><span class="sxs-lookup"><span data-stu-id="ce792-169">Create (and start) a new `Activity` instead of starting hello Application Insights operation.</span></span> <span data-ttu-id="ce792-170">Du gör *inte* måste tooassign egenskaper på den utom hello åtgärdsnamn.</span><span class="sxs-lookup"><span data-stu-id="ce792-170">You do *not* need tooassign any properties on it except hello operation name.</span></span>
- <span data-ttu-id="ce792-171">Serialisera `yourActivity.Id` till nyttolast för hello-meddelande i stället för `operation.Telemetry.Id`.</span><span class="sxs-lookup"><span data-stu-id="ce792-171">Serialize `yourActivity.Id` into hello message payload instead of `operation.Telemetry.Id`.</span></span> <span data-ttu-id="ce792-172">Du kan också använda `Activity.Current.Id`.</span><span class="sxs-lookup"><span data-stu-id="ce792-172">You can also use `Activity.Current.Id`.</span></span>


#### <a name="dequeue"></a><span data-ttu-id="ce792-173">Status Created</span><span class="sxs-lookup"><span data-stu-id="ce792-173">Dequeue</span></span>
<span data-ttu-id="ce792-174">På samma sätt för`Enqueue`, en faktiska HTTP toohello lagring begärandekö spåras automatiskt av Application Insights.</span><span class="sxs-lookup"><span data-stu-id="ce792-174">Similarly too`Enqueue`, an actual HTTP request toohello Storage queue is automatically tracked by Application Insights.</span></span> <span data-ttu-id="ce792-175">Dock hello `Enqueue` åtgärden förmodligen sker i hello överordnade kontext, till exempel en inkommande begäran-kontext.</span><span class="sxs-lookup"><span data-stu-id="ce792-175">However, hello `Enqueue` operation presumably happens in hello parent context, such as an incoming request context.</span></span> <span data-ttu-id="ce792-176">Application Insights SDK automatiskt korrelera sådan åtgärd (och dess HTTP-del) med hello överordnade begäran och andra telemetri som rapporterats i hello samma omfång.</span><span class="sxs-lookup"><span data-stu-id="ce792-176">Application Insights SDKs automatically correlate such an operation (and its HTTP part) with hello parent request and other telemetry reported in hello same scope.</span></span>

<span data-ttu-id="ce792-177">Hej `Dequeue` åtgärden är svår.</span><span class="sxs-lookup"><span data-stu-id="ce792-177">hello `Dequeue` operation is tricky.</span></span> <span data-ttu-id="ce792-178">hello Application Insights SDK spårar automatiskt HTTP-begäranden.</span><span class="sxs-lookup"><span data-stu-id="ce792-178">hello Application Insights SDK automatically tracks HTTP requests.</span></span> <span data-ttu-id="ce792-179">Men vet den inte hello korrelation kontext förrän tolkas hello-meddelande.</span><span class="sxs-lookup"><span data-stu-id="ce792-179">However, it doesn't know hello correlation context until hello message is parsed.</span></span> <span data-ttu-id="ce792-180">Det är inte möjligt toocorrelate hello HTTP-begäran tooget hello-meddelande med hello resten av hello telemetri.</span><span class="sxs-lookup"><span data-stu-id="ce792-180">It's not possible toocorrelate hello HTTP request tooget hello message with hello rest of hello telemetry.</span></span>

<span data-ttu-id="ce792-181">I många fall kan vara det användbart toocorrelate hello HTTP-begäran toohello kö med andra spår samt.</span><span class="sxs-lookup"><span data-stu-id="ce792-181">In many cases, it might be useful toocorrelate hello HTTP request toohello queue with other traces as well.</span></span> <span data-ttu-id="ce792-182">hello exemplet nedan visar hur toodo den:</span><span class="sxs-lookup"><span data-stu-id="ce792-182">hello following example demonstrates how toodo it:</span></span>

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

#### <a name="process"></a><span data-ttu-id="ce792-183">Processen</span><span class="sxs-lookup"><span data-stu-id="ce792-183">Process</span></span>

<span data-ttu-id="ce792-184">I följande exempel hello, vi spåra ett inkommande meddelande på ett sätt på samma sätt toohow vi spåra en inkommande HTTP för begäran:</span><span class="sxs-lookup"><span data-stu-id="ce792-184">In hello following example, we trace an incoming message in a manner similarly toohow we trace an incoming HTTP request:</span></span>

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

<span data-ttu-id="ce792-185">På liknande sätt kan andra köåtgärder instrumenterats.</span><span class="sxs-lookup"><span data-stu-id="ce792-185">Similarly, other queue operations can be instrumented.</span></span> <span data-ttu-id="ce792-186">En titt åtgärd bör instrumenterats på ett liknande sätt som en dequeue-åtgärd.</span><span class="sxs-lookup"><span data-stu-id="ce792-186">A peek operation should be instrumented in a similar way as a dequeue operation.</span></span> <span data-ttu-id="ce792-187">Instrumentering kön hanteringsåtgärder är inte nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="ce792-187">Instrumenting queue management operations isn't necessary.</span></span> <span data-ttu-id="ce792-188">Application Insights spårar åtgärder, till exempel HTTP och i de flesta fall är tillräckliga.</span><span class="sxs-lookup"><span data-stu-id="ce792-188">Application Insights tracks operations such as HTTP, and in most cases, it's enough.</span></span>

<span data-ttu-id="ce792-189">När du instrumentera borttagning av meddelanden måste du ange hello åtgärden (korrelation) identifierare.</span><span class="sxs-lookup"><span data-stu-id="ce792-189">When you instrument message deletion, make sure you set hello operation (correlation) identifiers.</span></span> <span data-ttu-id="ce792-190">Du kan också använda hello `Activity` API.</span><span class="sxs-lookup"><span data-stu-id="ce792-190">Alternatively, you can use hello `Activity` API.</span></span> <span data-ttu-id="ce792-191">Sedan behöver du inte tooset åtgärden identifierare på hello telemetri objekt eftersom Application Insights gör det:</span><span class="sxs-lookup"><span data-stu-id="ce792-191">Then you don't need tooset operation identifiers on hello telemetry items because Application Insights does it for you:</span></span>

- <span data-ttu-id="ce792-192">Skapa en ny `Activity` när du har installerat ett objekt från hello kö.</span><span class="sxs-lookup"><span data-stu-id="ce792-192">Create a new `Activity` after you've got an item from hello queue.</span></span>
- <span data-ttu-id="ce792-193">Använd `Activity.SetParentId(message.ParentId)` toocorrelate konsumenten och producenten loggar.</span><span class="sxs-lookup"><span data-stu-id="ce792-193">Use `Activity.SetParentId(message.ParentId)` toocorrelate consumer and producer logs.</span></span>
- <span data-ttu-id="ce792-194">Starta hello `Activity`.</span><span class="sxs-lookup"><span data-stu-id="ce792-194">Start hello `Activity`.</span></span>
- <span data-ttu-id="ce792-195">Spåra status Created bearbeta och delete-åtgärder med hjälp av `Start/StopOperation` hjälpprogram.</span><span class="sxs-lookup"><span data-stu-id="ce792-195">Track dequeue, process, and delete operations by using `Start/StopOperation` helpers.</span></span> <span data-ttu-id="ce792-196">Göra det från hello samma asynkrona Kontrollflöde (körningskontexten).</span><span class="sxs-lookup"><span data-stu-id="ce792-196">Do it from hello same asynchronous control flow (execution context).</span></span> <span data-ttu-id="ce792-197">På så sätt är de korrelerade korrekt.</span><span class="sxs-lookup"><span data-stu-id="ce792-197">In this way, they're correlated properly.</span></span>
- <span data-ttu-id="ce792-198">Stoppa hello `Activity`.</span><span class="sxs-lookup"><span data-stu-id="ce792-198">Stop hello `Activity`.</span></span>
- <span data-ttu-id="ce792-199">Använd `Start/StopOperation`, eller anropa `Track` telemetri manuellt.</span><span class="sxs-lookup"><span data-stu-id="ce792-199">Use `Start/StopOperation`, or call `Track` telemetry manually.</span></span>

### <a name="batch-processing"></a><span data-ttu-id="ce792-200">Batch-bearbetning</span><span class="sxs-lookup"><span data-stu-id="ce792-200">Batch processing</span></span>
<span data-ttu-id="ce792-201">Med vissa köer du status Created flera meddelanden med en begäran.</span><span class="sxs-lookup"><span data-stu-id="ce792-201">With some queues, you can dequeue multiple messages with one request.</span></span> <span data-ttu-id="ce792-202">Bearbeta sådana meddelanden är förmodligen oberoende och tillhör toohello olika logiska åtgärder.</span><span class="sxs-lookup"><span data-stu-id="ce792-202">Processing such messages is presumably independent and belongs toohello different logical operations.</span></span> <span data-ttu-id="ce792-203">I detta fall det inte är möjligt toocorrelate hello `Dequeue` åtgärden tooparticular meddelandebehandling.</span><span class="sxs-lookup"><span data-stu-id="ce792-203">In this case, it's not possible toocorrelate hello `Dequeue` operation tooparticular message processing.</span></span>

<span data-ttu-id="ce792-204">Varje meddelande som ska bearbetas i sin egen asynkron Kontrollflöde.</span><span class="sxs-lookup"><span data-stu-id="ce792-204">Each message should be processed in its own asynchronous control flow.</span></span> <span data-ttu-id="ce792-205">Mer information finns i hello [utgående beroenden spårning](#outgoing-dependencies-tracking) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="ce792-205">For more information, see hello [Outgoing dependencies tracking](#outgoing-dependencies-tracking) section.</span></span>

## <a name="long-running-background-tasks"></a><span data-ttu-id="ce792-206">Långvariga bakgrundsaktiviteter</span><span class="sxs-lookup"><span data-stu-id="ce792-206">Long-running background tasks</span></span>
<span data-ttu-id="ce792-207">Vissa program startar långvariga åtgärder som kan orsakas av användarförfrågningar.</span><span class="sxs-lookup"><span data-stu-id="ce792-207">Some applications start long-running operations that might be caused by user requests.</span></span> <span data-ttu-id="ce792-208">Ur hello spårning/instrumentation skiljer den sig inte från begäran eller beroende:</span><span class="sxs-lookup"><span data-stu-id="ce792-208">From hello tracing/instrumentation perspective, it's not different from request or dependency instrumentation:</span></span> 

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

<span data-ttu-id="ce792-209">I det här exemplet använder vi `telemetryClient.StartOperation` toocreate `RequestTelemetry` och fill hello korrelation kontext.</span><span class="sxs-lookup"><span data-stu-id="ce792-209">In this example, we use `telemetryClient.StartOperation` toocreate `RequestTelemetry` and fill hello correlation context.</span></span> <span data-ttu-id="ce792-210">Anta att du har en överordnad åtgärd som har skapats av inkommande begäranden som schemalagda hello igen.</span><span class="sxs-lookup"><span data-stu-id="ce792-210">Let's say you have a parent operation that was created by incoming requests that scheduled hello operation.</span></span> <span data-ttu-id="ce792-211">Så länge `BackgroundTask` startar i Hej samma asynkrona Kontrollflöde som en inkommande begäran, den korreleras med den överordnade igen.</span><span class="sxs-lookup"><span data-stu-id="ce792-211">As long as `BackgroundTask` starts in hello same asynchronous control flow as an incoming request, it's correlated with that parent operation.</span></span> <span data-ttu-id="ce792-212">`BackgroundTask`och alla kapslade telemetri objekt automatiskt kopplas ihop med hello-begäran som orsakade det, även när hello begäran avslutas.</span><span class="sxs-lookup"><span data-stu-id="ce792-212">`BackgroundTask` and all nested telemetry items are automatically correlated with hello request that caused it, even after hello request ends.</span></span>

<span data-ttu-id="ce792-213">När hello aktiviteten startar från hello bakgrundstråd som inte har någon åtgärd (`Activity`) som är kopplade till den, `BackgroundTask` inte har någon överordnad.</span><span class="sxs-lookup"><span data-stu-id="ce792-213">When hello task starts from hello background thread that doesn't have any operation (`Activity`) associated with it, `BackgroundTask` doesn't have any parent.</span></span> <span data-ttu-id="ce792-214">Det kan dock ha kapslade åtgärder.</span><span class="sxs-lookup"><span data-stu-id="ce792-214">However, it can have nested operations.</span></span> <span data-ttu-id="ce792-215">Alla artiklar för telemetri som rapporterats från hello aktiviteten är korrelerade toohello `RequestTelemetry` skapas i `BackgroundTask`.</span><span class="sxs-lookup"><span data-stu-id="ce792-215">All telemetry items reported from hello task are correlated toohello `RequestTelemetry` created in `BackgroundTask`.</span></span>

## <a name="outgoing-dependencies-tracking"></a><span data-ttu-id="ce792-216">Utgående beroenden spårning</span><span class="sxs-lookup"><span data-stu-id="ce792-216">Outgoing dependencies tracking</span></span>
<span data-ttu-id="ce792-217">Du kan spåra dina egna beroende typ eller en åtgärd som inte stöds av Application Insights.</span><span class="sxs-lookup"><span data-stu-id="ce792-217">You can track your own dependency kind or an operation that's not supported by Application Insights.</span></span>

<span data-ttu-id="ce792-218">Hej `Enqueue` metod i hello Service Bus-kö eller hello lagring kö kan fungera som exempel på sådana anpassade spårning.</span><span class="sxs-lookup"><span data-stu-id="ce792-218">hello `Enqueue` method in hello Service Bus queue or hello Storage queue can serve as examples for such custom tracking.</span></span>

<span data-ttu-id="ce792-219">hello allmänna riktlinjer för anpassade beroende spårning är att:</span><span class="sxs-lookup"><span data-stu-id="ce792-219">hello general approach for custom dependency tracking is to:</span></span>

- <span data-ttu-id="ce792-220">Anropa hello `TelemetryClient.StartOperation` (tillägg)-metod som fyller hello `DependencyTelemetry` egenskaper som krävs för korrelation och vissa andra egenskaper (starttid stämpel, varaktighet).</span><span class="sxs-lookup"><span data-stu-id="ce792-220">Call hello `TelemetryClient.StartOperation` (extension) method that fills hello `DependencyTelemetry` properties that are needed for correlation and some other properties (start  time stamp, duration).</span></span>
- <span data-ttu-id="ce792-221">Ange andra anpassade egenskaper för hello `DependencyTelemetry`, till exempel hello namn och en kontext som du behöver.</span><span class="sxs-lookup"><span data-stu-id="ce792-221">Set other custom properties on hello `DependencyTelemetry`, such as hello name and any other context you need.</span></span>
- <span data-ttu-id="ce792-222">Se ett beroende anropa och vänta tills den.</span><span class="sxs-lookup"><span data-stu-id="ce792-222">Make a dependency call and wait for it.</span></span>
- <span data-ttu-id="ce792-223">Stoppa hello igen med `StopOperation` när den är klar.</span><span class="sxs-lookup"><span data-stu-id="ce792-223">Stop hello operation with `StopOperation` when it's finished.</span></span>
- <span data-ttu-id="ce792-224">Hantera undantag.</span><span class="sxs-lookup"><span data-stu-id="ce792-224">Handle exceptions.</span></span>

<span data-ttu-id="ce792-225">`StopOperation`endast avbryts hello som startades.</span><span class="sxs-lookup"><span data-stu-id="ce792-225">`StopOperation` only stops hello operation that was started.</span></span> <span data-ttu-id="ce792-226">Om hello aktuella körs åtgärden inte matchar hello en du vill toostop, `StopOperation` händer ingenting.</span><span class="sxs-lookup"><span data-stu-id="ce792-226">If hello current running operation doesn't match hello one you want toostop, `StopOperation` does nothing.</span></span> <span data-ttu-id="ce792-227">Den här situationen kan inträffa om du startar flera åtgärder parallellt i hello samma körningskontexten:</span><span class="sxs-lookup"><span data-stu-id="ce792-227">This situation might happen if you start multiple operations in parallel in hello same execution context:</span></span>

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

<span data-ttu-id="ce792-228">Se till att du alltid anropa `StartOperation` och kör uppgiften i sin egen kontext:</span><span class="sxs-lookup"><span data-stu-id="ce792-228">Make sure you always call `StartOperation` and run your task in its own context:</span></span>
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

## <a name="next-steps"></a><span data-ttu-id="ce792-229">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="ce792-229">Next steps</span></span>

- <span data-ttu-id="ce792-230">Hello grundläggande information om hur [telemetri korrelation](application-insights-correlation.md) i Application Insights.</span><span class="sxs-lookup"><span data-stu-id="ce792-230">Learn hello basics of [telemetry correlation](application-insights-correlation.md) in Application Insights.</span></span>
- <span data-ttu-id="ce792-231">Se hello [datamodellen](application-insights-data-model.md) för Application Insights typer och modell.</span><span class="sxs-lookup"><span data-stu-id="ce792-231">See hello [data model](application-insights-data-model.md) for Application Insights types and data model.</span></span>
- <span data-ttu-id="ce792-232">Rapportera anpassad [händelser och mått](app-insights-api-custom-events-metrics.md) tooApplication insikter.</span><span class="sxs-lookup"><span data-stu-id="ce792-232">Report custom [events and metrics](app-insights-api-custom-events-metrics.md) tooApplication Insights.</span></span>
- <span data-ttu-id="ce792-233">Kolla standard [configuration](app-insights-configuration-with-applicationinsights-config.md#telemetry-initializers-aspnet) för kontexten egenskapssamling.</span><span class="sxs-lookup"><span data-stu-id="ce792-233">Check out standard [configuration](app-insights-configuration-with-applicationinsights-config.md#telemetry-initializers-aspnet) for context properties collection.</span></span>
- <span data-ttu-id="ce792-234">Kontrollera hello [System.Diagnostics.Activity användarhandboken](https://github.com/dotnet/corefx/blob/master/src/System.Diagnostics.DiagnosticSource/src/ActivityUserGuide.md) toosee hur vi korrelera telemetri.</span><span class="sxs-lookup"><span data-stu-id="ce792-234">Check hello [System.Diagnostics.Activity User Guide](https://github.com/dotnet/corefx/blob/master/src/System.Diagnostics.DiagnosticSource/src/ActivityUserGuide.md) toosee how we correlate telemetry.</span></span>
