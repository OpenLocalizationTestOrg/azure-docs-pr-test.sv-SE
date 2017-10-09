---
title: "aaaAzure funktioner Händelsehubbar bindningar | Microsoft Docs"
description: "Förstå hur toouse Azure Event Hubs bindningar i Azure Functions."
services: functions
documentationcenter: na
author: wesmc7777
manager: erikre
editor: 
tags: 
keywords: "Azure functions, funktioner, händelsebearbetning, dynamiska beräkning serverlösa arkitektur"
ms.assetid: daf81798-7acc-419a-bc32-b5a41c6db56b
ms.service: functions
ms.devlang: multiple
ms.topic: reference
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 06/20/2017
ms.author: wesmc
ms.openlocfilehash: e864f032ad5ac58d318c9843c3844b5642733a70
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-functions-event-hubs-bindings"></a><span data-ttu-id="4a516-104">Azure Functions Händelsehubbar bindningar</span><span class="sxs-lookup"><span data-stu-id="4a516-104">Azure Functions Event Hubs bindings</span></span>
[!INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

<span data-ttu-id="4a516-105">Den här artikeln förklarar hur tooconfigure och använda [Azure Event Hubs](../event-hubs/event-hubs-what-is-event-hubs.md) bindningar för Azure Functions.</span><span class="sxs-lookup"><span data-stu-id="4a516-105">This article explains how tooconfigure and use [Azure Event Hubs](../event-hubs/event-hubs-what-is-event-hubs.md) bindings for Azure Functions.</span></span>
<span data-ttu-id="4a516-106">Azure Functions stöder utlösa och utgående bindningar för Händelsehubbar.</span><span class="sxs-lookup"><span data-stu-id="4a516-106">Azure Functions supports trigger and output bindings for Event Hubs.</span></span>

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

<span data-ttu-id="4a516-107">Om du är ny tooAzure Händelsehubbar finns hello [översikt av Händelsehubbar](../event-hubs/event-hubs-what-is-event-hubs.md).</span><span class="sxs-lookup"><span data-stu-id="4a516-107">If you are new tooAzure Event Hubs, see hello [Event Hubs overview](../event-hubs/event-hubs-what-is-event-hubs.md).</span></span>

<a name="trigger"></a>

## <a name="event-hub-trigger"></a><span data-ttu-id="4a516-108">Händelseutlösare hub</span><span class="sxs-lookup"><span data-stu-id="4a516-108">Event hub trigger</span></span>
<span data-ttu-id="4a516-109">Använd hello Händelsehubbar utlöser toorespond tooan händelse skickas tooan event hub händelseströmmen.</span><span class="sxs-lookup"><span data-stu-id="4a516-109">Use hello Event Hubs trigger toorespond tooan event sent tooan event hub event stream.</span></span> <span data-ttu-id="4a516-110">Du måste ha läsbehörighet toohello event hub tooset in hello utlösare.</span><span class="sxs-lookup"><span data-stu-id="4a516-110">You must have read access toohello event hub tooset up hello trigger.</span></span>

<span data-ttu-id="4a516-111">Hej Händelsehubbar funktionen utlösaren använder hello följande JSON-objekt i hello `bindings` matris med function.json:</span><span class="sxs-lookup"><span data-stu-id="4a516-111">hello Event Hubs function trigger uses hello following JSON object in hello `bindings` array of function.json:</span></span>

```json
{
    "type": "eventHubTrigger",
    "name": "<Name of trigger parameter in function signature>",
    "direction": "in",
    "path": "<Name of hello event hub>",
    "consumerGroup": "Consumer group toouse - see below",
    "connection": "<Name of app setting with connection string - see below>"
}
```

<span data-ttu-id="4a516-112">`consumerGroup`är en valfri egenskap används tooset hello [konsumentgrupp](../event-hubs/event-hubs-features.md#event-consumers) används toosubscribe tooevents i hello hubb.</span><span class="sxs-lookup"><span data-stu-id="4a516-112">`consumerGroup` is an optional property used tooset hello [consumer group](../event-hubs/event-hubs-features.md#event-consumers) used toosubscribe tooevents in hello hub.</span></span> <span data-ttu-id="4a516-113">Om det utelämnas används hello `$Default` konsumentgrupp används.</span><span class="sxs-lookup"><span data-stu-id="4a516-113">If omitted, hello `$Default` consumer group is used.</span></span>  
<span data-ttu-id="4a516-114">`connection`måste vara hello namnet på en appinställning som innehåller hello anslutning sträng toohello händelsehubbs namnutrymmet.</span><span class="sxs-lookup"><span data-stu-id="4a516-114">`connection` must be hello name of an app setting that contains hello connection string toohello event hub's namespace.</span></span>
<span data-ttu-id="4a516-115">Kopiera denna anslutningssträng genom att klicka på hello **anslutningsinformationen** knapp för hello *namnområde*, inte hello händelsehubb sig själv.</span><span class="sxs-lookup"><span data-stu-id="4a516-115">Copy this connection string by clicking hello **Connection Information** button for hello *namespace*, not hello event hub itself.</span></span> <span data-ttu-id="4a516-116">Den här anslutningssträngen måste ha minst Läs behörigheter tooactivate hello utlösare.</span><span class="sxs-lookup"><span data-stu-id="4a516-116">This connection string must have at least read permissions tooactivate hello trigger.</span></span>

<span data-ttu-id="4a516-117">[Ytterligare inställningar](https://github.com/Azure/azure-webjobs-sdk-script/wiki/host.json) kan finnas i en host.json filen toofurther bra finjustera Händelsehubbar utlösare.</span><span class="sxs-lookup"><span data-stu-id="4a516-117">[Additional settings](https://github.com/Azure/azure-webjobs-sdk-script/wiki/host.json) can be provided in a host.json file toofurther fine tune Event Hubs triggers.</span></span>  

<a name="triggerusage"></a>

## <a name="trigger-usage"></a><span data-ttu-id="4a516-118">Utlösaren användning</span><span class="sxs-lookup"><span data-stu-id="4a516-118">Trigger usage</span></span>
<span data-ttu-id="4a516-119">När en funktion för Händelsehubbar utlösare utlöses skickas hello-meddelande som utlöser den till hello funktionen som en sträng.</span><span class="sxs-lookup"><span data-stu-id="4a516-119">When an Event Hubs trigger function is triggered, hello message that triggers it is passed into hello function as a string.</span></span>

<a name="triggersample"></a>

## <a name="trigger-sample"></a><span data-ttu-id="4a516-120">Utlösaren exempel</span><span class="sxs-lookup"><span data-stu-id="4a516-120">Trigger sample</span></span>
<span data-ttu-id="4a516-121">Anta att du har följande Händelsehubbar utlösare i hello hello `bindings` matris med function.json:</span><span class="sxs-lookup"><span data-stu-id="4a516-121">Suppose you have hello following Event Hubs trigger in hello `bindings` array of function.json:</span></span>

```json
{
  "type": "eventHubTrigger",
  "name": "myEventHubMessage",
  "direction": "in",
  "path": "MyEventHub",
  "connection": "myEventHubReadConnectionString"
}
```

<span data-ttu-id="4a516-122">Se hello språkspecifika exempel loggar hello meddelandetexten hello event hub utlösare.</span><span class="sxs-lookup"><span data-stu-id="4a516-122">See hello language-specific sample that logs hello message body of hello event hub trigger.</span></span>

* [<span data-ttu-id="4a516-123">C#</span><span class="sxs-lookup"><span data-stu-id="4a516-123">C#</span></span>](#triggercsharp)
* [<span data-ttu-id="4a516-124">F#</span><span class="sxs-lookup"><span data-stu-id="4a516-124">F#</span></span>](#triggerfsharp)
* [<span data-ttu-id="4a516-125">Node.js</span><span class="sxs-lookup"><span data-stu-id="4a516-125">Node.js</span></span>](#triggernodejs)

<a name="triggercsharp"></a>

### <a name="trigger-sample-in-c"></a><span data-ttu-id="4a516-126">Utlösaren exemplet i C#</span><span class="sxs-lookup"><span data-stu-id="4a516-126">Trigger sample in C#</span></span> #

```cs
using System;

public static void Run(string myEventHubMessage, TraceWriter log)
{
    log.Info($"C# Event Hub trigger function processed a message: {myEventHubMessage}");
}
```

<span data-ttu-id="4a516-127">Du kan också få hello händelse som en [EventData](/dotnet/api/microsoft.servicebus.messaging.eventdata) -objektet, vilket ger dig åtkomst till metadata för toohello händelser.</span><span class="sxs-lookup"><span data-stu-id="4a516-127">You can also receive hello event as an [EventData](/dotnet/api/microsoft.servicebus.messaging.eventdata) object, which gives you access toohello event metadata.</span></span>

```cs
#r "Microsoft.ServiceBus"
using System.Text;
using Microsoft.ServiceBus.Messaging;

public static void Run(EventData myEventHubMessage, TraceWriter log)
{
    log.Info($"{Encoding.UTF8.GetString(myEventHubMessage.GetBytes())}");
}
```

<span data-ttu-id="4a516-128">tooreceive händelser i en batch ändra hello Metodsignaturen för`string[]` eller `EventData[]`.</span><span class="sxs-lookup"><span data-stu-id="4a516-128">tooreceive events in a batch, change hello method signature too`string[]` or `EventData[]`.</span></span>

```cs
public static void Run(string[] eventHubMessages, TraceWriter log)
{
    foreach (var message in eventHubMessages)
    {
        log.Info($"C# Event Hub trigger function processed a message: {message}");
    }
}
```

<a name="triggerfsharp"></a>

### <a name="trigger-sample-in-f"></a><span data-ttu-id="4a516-129">Utlösaren exemplet i F #</span><span class="sxs-lookup"><span data-stu-id="4a516-129">Trigger sample in F#</span></span> #

```fsharp
let Run(myEventHubMessage: string, log: TraceWriter) =
    log.Info(sprintf "F# eventhub trigger function processed work item: %s" myEventHubMessage)
```

<a name="triggernodejs"></a>

### <a name="trigger-sample-in-nodejs"></a><span data-ttu-id="4a516-130">Utlösaren exemplet i Node.js</span><span class="sxs-lookup"><span data-stu-id="4a516-130">Trigger sample in Node.js</span></span>

```javascript
module.exports = function (context, myEventHubMessage) {
    context.log('Node.js eventhub trigger function processed work item', myEventHubMessage);    
    context.done();
};
```

<a name="output"></a>

## <a name="event-hubs-output-binding"></a><span data-ttu-id="4a516-131">Händelsehubbar utdatabindning</span><span class="sxs-lookup"><span data-stu-id="4a516-131">Event Hubs output binding</span></span>
<span data-ttu-id="4a516-132">Använd hello Händelsehubbar utdata bindning toowrite händelser tooan event hub händelseströmmen.</span><span class="sxs-lookup"><span data-stu-id="4a516-132">Use hello Event Hubs output binding toowrite events tooan event hub event stream.</span></span> <span data-ttu-id="4a516-133">Du måste ha skicka behörighet tooan event hub toowrite händelser tooit.</span><span class="sxs-lookup"><span data-stu-id="4a516-133">You must have send permission tooan event hub toowrite events tooit.</span></span>

<span data-ttu-id="4a516-134">hello utdata bindningen använder hello följande JSON-objekt i hello `bindings` matris med function.json:</span><span class="sxs-lookup"><span data-stu-id="4a516-134">hello output binding uses hello following JSON object in hello `bindings` array of function.json:</span></span>

```json
{
    "type": "eventHub",
    "name": "<Name of output parameter in function signature>",
    "path": "<Name of event hub>",
    "connection": "<Name of app setting with connection string - see below>"
    "direction": "out"
}
```

<span data-ttu-id="4a516-135">`connection`måste vara hello namnet på en appinställning som innehåller hello anslutning sträng toohello händelsehubbs namnutrymmet.</span><span class="sxs-lookup"><span data-stu-id="4a516-135">`connection` must be hello name of an app setting that contains hello connection string toohello event hub's namespace.</span></span>
<span data-ttu-id="4a516-136">Kopiera denna anslutningssträng genom att klicka på hello **anslutningsinformationen** knapp för hello *namnområde*, inte hello händelsehubb sig själv.</span><span class="sxs-lookup"><span data-stu-id="4a516-136">Copy this connection string by clicking hello **Connection Information** button for hello *namespace*, not hello event hub itself.</span></span> <span data-ttu-id="4a516-137">Den här anslutningssträngen måste ha skicka behörigheter toosend hello toohello händelse meddelandeströmmen.</span><span class="sxs-lookup"><span data-stu-id="4a516-137">This connection string must have send permissions toosend hello message toohello event stream.</span></span>

## <a name="output-usage"></a><span data-ttu-id="4a516-138">Användning av utdata</span><span class="sxs-lookup"><span data-stu-id="4a516-138">Output usage</span></span>
<span data-ttu-id="4a516-139">Det här avsnittet visar hur toouse Händelsehubbarna utdata bindningen i din funktionskoden.</span><span class="sxs-lookup"><span data-stu-id="4a516-139">This section shows you how toouse your Event Hubs output binding in your function code.</span></span>

<span data-ttu-id="4a516-140">Du kan spara händelsehubb för meddelanden toohello som konfigurerats med följande parametertyper hello:</span><span class="sxs-lookup"><span data-stu-id="4a516-140">You can output messages toohello configured event hub with hello following parameter types:</span></span>

* `out string`
* <span data-ttu-id="4a516-141">`ICollector<string>`(toooutput flera meddelanden)</span><span class="sxs-lookup"><span data-stu-id="4a516-141">`ICollector<string>` (toooutput multiple messages)</span></span>
* <span data-ttu-id="4a516-142">`IAsyncCollector<string>`(asynkrona versionen av `ICollector<T>`)</span><span class="sxs-lookup"><span data-stu-id="4a516-142">`IAsyncCollector<string>` (async version of `ICollector<T>`)</span></span>

<a name="outputsample"></a>

## <a name="output-sample"></a><span data-ttu-id="4a516-143">Exempel på utdata</span><span class="sxs-lookup"><span data-stu-id="4a516-143">Output sample</span></span>
<span data-ttu-id="4a516-144">Anta att du har följande hello Händelsehubbar utdatabindning i hello `bindings` matris med function.json:</span><span class="sxs-lookup"><span data-stu-id="4a516-144">Suppose you have hello following Event Hubs output binding in hello `bindings` array of function.json:</span></span>

```json
{
    "type": "eventHub",
    "name": "outputEventHubMessage",
    "path": "myeventhub",
    "connection": "MyEventHubSend",
    "direction": "out"
}
```

<span data-ttu-id="4a516-145">Se hello språkspecifika prov som skriver en händelse toohello även dataström.</span><span class="sxs-lookup"><span data-stu-id="4a516-145">See hello language-specific sample that writes an event toohello even stream.</span></span>

* [<span data-ttu-id="4a516-146">C#</span><span class="sxs-lookup"><span data-stu-id="4a516-146">C#</span></span>](#outcsharp)
* [<span data-ttu-id="4a516-147">F#</span><span class="sxs-lookup"><span data-stu-id="4a516-147">F#</span></span>](#outfsharp)
* [<span data-ttu-id="4a516-148">Node.js</span><span class="sxs-lookup"><span data-stu-id="4a516-148">Node.js</span></span>](#outnodejs)

<a name="outcsharp"></a>

### <a name="output-sample-in-c"></a><span data-ttu-id="4a516-149">Utdata exemplet i C#</span><span class="sxs-lookup"><span data-stu-id="4a516-149">Output sample in C#</span></span> #

```cs
using System;

public static void Run(TimerInfo myTimer, out string outputEventHubMessage, TraceWriter log)
{
    String msg = $"TimerTriggerCSharp1 executed at: {DateTime.Now}";
    log.Verbose(msg);   
    outputEventHubMessage = msg;
}
```

<span data-ttu-id="4a516-150">Eller toocreate flera meddelanden:</span><span class="sxs-lookup"><span data-stu-id="4a516-150">Or, toocreate multiple messages:</span></span>

```cs
public static void Run(TimerInfo myTimer, ICollector<string> outputEventHubMessage, TraceWriter log)
{
    string message = $"Event Hub message created at: {DateTime.Now}";
    log.Info(message);
    outputEventHubMessage.Add("1 " + message);
    outputEventHubMessage.Add("2 " + message);
}
```

<a name="outfsharp"></a>

### <a name="output-sample-in-f"></a><span data-ttu-id="4a516-151">Utdata exemplet i F #</span><span class="sxs-lookup"><span data-stu-id="4a516-151">Output sample in F#</span></span> #

```fsharp
let Run(myTimer: TimerInfo, outputEventHubMessage: byref<string>, log: TraceWriter) =
    let msg = sprintf "TimerTriggerFSharp1 executed at: %s" DateTime.Now.ToString()
    log.Verbose(msg);
    outputEventHubMessage <- msg;
```

<a name="outnodejs"></a>

### <a name="output-sample-for-nodejs"></a><span data-ttu-id="4a516-152">Exempel utdata för Node.js</span><span class="sxs-lookup"><span data-stu-id="4a516-152">Output sample for Node.js</span></span>

```javascript
module.exports = function (context, myTimer) {
    var timeStamp = new Date().toISOString();
    context.log('Event Hub message created at: ', timeStamp);   
    context.bindings.outputEventHubMessage = "Event Hub message created at: " + timeStamp;
    context.done();
};
```

<span data-ttu-id="4a516-153">Eller toosend flera meddelanden</span><span class="sxs-lookup"><span data-stu-id="4a516-153">Or, toosend multiple messages,</span></span>

```javascript
module.exports = function(context) {
    var timeStamp = new Date().toISOString();
    var message = 'Event Hub message created at: ' + timeStamp;

    context.bindings.outputEventHubMessage = [];

    context.bindings.outputEventHubMessage.push("1 " + message);
    context.bindings.outputEventHubMessage.push("2 " + message);
    context.done();
};
```

## <a name="next-steps"></a><span data-ttu-id="4a516-154">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="4a516-154">Next steps</span></span>
[!INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)]
