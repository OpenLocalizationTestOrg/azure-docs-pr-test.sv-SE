---
title: "Azure Functions Händelsehubbar bindningar | Microsoft Docs"
description: "Förstå hur du använder Azure Event Hubs bindningar i Azure Functions."
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
ms.openlocfilehash: 19021bef8b7156b3049f43b0275c0ed0c6b22514
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="azure-functions-event-hubs-bindings"></a><span data-ttu-id="901df-104">Azure Functions Händelsehubbar bindningar</span><span class="sxs-lookup"><span data-stu-id="901df-104">Azure Functions Event Hubs bindings</span></span>
[!INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

<span data-ttu-id="901df-105">Den här artikeln beskriver hur du konfigurerar och använder [Azure Event Hubs](../event-hubs/event-hubs-what-is-event-hubs.md) bindningar för Azure Functions.</span><span class="sxs-lookup"><span data-stu-id="901df-105">This article explains how to configure and use [Azure Event Hubs](../event-hubs/event-hubs-what-is-event-hubs.md) bindings for Azure Functions.</span></span>
<span data-ttu-id="901df-106">Azure Functions stöder utlösa och utgående bindningar för Händelsehubbar.</span><span class="sxs-lookup"><span data-stu-id="901df-106">Azure Functions supports trigger and output bindings for Event Hubs.</span></span>

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

<span data-ttu-id="901df-107">Om du har använt Azure Event Hubs, se den [översikt av Händelsehubbar](../event-hubs/event-hubs-what-is-event-hubs.md).</span><span class="sxs-lookup"><span data-stu-id="901df-107">If you are new to Azure Event Hubs, see the [Event Hubs overview](../event-hubs/event-hubs-what-is-event-hubs.md).</span></span>

<a name="trigger"></a>

## <a name="event-hub-trigger"></a><span data-ttu-id="901df-108">Händelseutlösare hub</span><span class="sxs-lookup"><span data-stu-id="901df-108">Event hub trigger</span></span>
<span data-ttu-id="901df-109">Använd Händelsehubbar utlösaren ska svara på en händelse som skickas till en event hub händelseström.</span><span class="sxs-lookup"><span data-stu-id="901df-109">Use the Event Hubs trigger to respond to an event sent to an event hub event stream.</span></span> <span data-ttu-id="901df-110">Du måste ha läsbehörighet till händelsehubben för att konfigurera utlösaren.</span><span class="sxs-lookup"><span data-stu-id="901df-110">You must have read access to the event hub to set up the trigger.</span></span>

<span data-ttu-id="901df-111">Händelsehubbar funktionen utlösaren använder följande JSON-objekt i den `bindings` matris med function.json:</span><span class="sxs-lookup"><span data-stu-id="901df-111">The Event Hubs function trigger uses the following JSON object in the `bindings` array of function.json:</span></span>

```json
{
    "type": "eventHubTrigger",
    "name": "<Name of trigger parameter in function signature>",
    "direction": "in",
    "path": "<Name of the event hub>",
    "consumerGroup": "Consumer group to use - see below",
    "connection": "<Name of app setting with connection string - see below>"
}
```

<span data-ttu-id="901df-112">`consumerGroup`är en valfri egenskap som används för att ange den [konsumentgrupp](../event-hubs/event-hubs-features.md#event-consumers) används för att prenumerera på händelser i hubben.</span><span class="sxs-lookup"><span data-stu-id="901df-112">`consumerGroup` is an optional property used to set the [consumer group](../event-hubs/event-hubs-features.md#event-consumers) used to subscribe to events in the hub.</span></span> <span data-ttu-id="901df-113">Om det utelämnas används den `$Default` konsumentgrupp används.</span><span class="sxs-lookup"><span data-stu-id="901df-113">If omitted, the `$Default` consumer group is used.</span></span>  
<span data-ttu-id="901df-114">`connection`måste vara namnet på en appinställning som innehåller anslutningssträngen till den event hub-namnområdet.</span><span class="sxs-lookup"><span data-stu-id="901df-114">`connection` must be the name of an app setting that contains the connection string to the event hub's namespace.</span></span>
<span data-ttu-id="901df-115">Kopiera denna anslutningssträng genom att klicka på den **anslutningsinformationen** knappen för den *namnområde*, inte händelsehubben sig själv.</span><span class="sxs-lookup"><span data-stu-id="901df-115">Copy this connection string by clicking the **Connection Information** button for the *namespace*, not the event hub itself.</span></span> <span data-ttu-id="901df-116">Den här anslutningssträngen måste ha minst läsbehörighet utlösaren ska aktiveras.</span><span class="sxs-lookup"><span data-stu-id="901df-116">This connection string must have at least read permissions to activate the trigger.</span></span>

<span data-ttu-id="901df-117">[Ytterligare inställningar](https://github.com/Azure/azure-webjobs-sdk-script/wiki/host.json) kan anges i en host.json fil att ytterligare finjustera Händelsehubbar utlösare.</span><span class="sxs-lookup"><span data-stu-id="901df-117">[Additional settings](https://github.com/Azure/azure-webjobs-sdk-script/wiki/host.json) can be provided in a host.json file to further fine tune Event Hubs triggers.</span></span>  

<a name="triggerusage"></a>

## <a name="trigger-usage"></a><span data-ttu-id="901df-118">Utlösaren användning</span><span class="sxs-lookup"><span data-stu-id="901df-118">Trigger usage</span></span>
<span data-ttu-id="901df-119">När en funktion för Händelsehubbar utlösare utlöses skickas meddelandet som utlöser den i funktionen som en sträng.</span><span class="sxs-lookup"><span data-stu-id="901df-119">When an Event Hubs trigger function is triggered, the message that triggers it is passed into the function as a string.</span></span>

<a name="triggersample"></a>

## <a name="trigger-sample"></a><span data-ttu-id="901df-120">Utlösaren exempel</span><span class="sxs-lookup"><span data-stu-id="901df-120">Trigger sample</span></span>
<span data-ttu-id="901df-121">Anta att du har följande Händelsehubbar utlösare i den `bindings` matris med function.json:</span><span class="sxs-lookup"><span data-stu-id="901df-121">Suppose you have the following Event Hubs trigger in the `bindings` array of function.json:</span></span>

```json
{
  "type": "eventHubTrigger",
  "name": "myEventHubMessage",
  "direction": "in",
  "path": "MyEventHub",
  "connection": "myEventHubReadConnectionString"
}
```

<span data-ttu-id="901df-122">Se exemplet språkspecifika loggar av meddelandetexten i hubben händelseutlösare.</span><span class="sxs-lookup"><span data-stu-id="901df-122">See the language-specific sample that logs the message body of the event hub trigger.</span></span>

* [<span data-ttu-id="901df-123">C#</span><span class="sxs-lookup"><span data-stu-id="901df-123">C#</span></span>](#triggercsharp)
* [<span data-ttu-id="901df-124">F#</span><span class="sxs-lookup"><span data-stu-id="901df-124">F#</span></span>](#triggerfsharp)
* [<span data-ttu-id="901df-125">Node.js</span><span class="sxs-lookup"><span data-stu-id="901df-125">Node.js</span></span>](#triggernodejs)

<a name="triggercsharp"></a>

### <a name="trigger-sample-in-c"></a><span data-ttu-id="901df-126">Utlösaren exemplet i C#</span><span class="sxs-lookup"><span data-stu-id="901df-126">Trigger sample in C#</span></span> #

```cs
using System;

public static void Run(string myEventHubMessage, TraceWriter log)
{
    log.Info($"C# Event Hub trigger function processed a message: {myEventHubMessage}");
}
```

<span data-ttu-id="901df-127">Du kan också få händelsen som en [EventData](/dotnet/api/microsoft.servicebus.messaging.eventdata) -objektet, vilket ger dig tillgång till metadata för händelser.</span><span class="sxs-lookup"><span data-stu-id="901df-127">You can also receive the event as an [EventData](/dotnet/api/microsoft.servicebus.messaging.eventdata) object, which gives you access to the event metadata.</span></span>

```cs
#r "Microsoft.ServiceBus"
using System.Text;
using Microsoft.ServiceBus.Messaging;

public static void Run(EventData myEventHubMessage, TraceWriter log)
{
    log.Info($"{Encoding.UTF8.GetString(myEventHubMessage.GetBytes())}");
}
```

<span data-ttu-id="901df-128">Om du vill ta emot händelser i en batch ändra Metodsignaturen till `string[]` eller `EventData[]`.</span><span class="sxs-lookup"><span data-stu-id="901df-128">To receive events in a batch, change the method signature to `string[]` or `EventData[]`.</span></span>

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

### <a name="trigger-sample-in-f"></a><span data-ttu-id="901df-129">Utlösaren exemplet i F #</span><span class="sxs-lookup"><span data-stu-id="901df-129">Trigger sample in F#</span></span> #

```fsharp
let Run(myEventHubMessage: string, log: TraceWriter) =
    log.Info(sprintf "F# eventhub trigger function processed work item: %s" myEventHubMessage)
```

<a name="triggernodejs"></a>

### <a name="trigger-sample-in-nodejs"></a><span data-ttu-id="901df-130">Utlösaren exemplet i Node.js</span><span class="sxs-lookup"><span data-stu-id="901df-130">Trigger sample in Node.js</span></span>

```javascript
module.exports = function (context, myEventHubMessage) {
    context.log('Node.js eventhub trigger function processed work item', myEventHubMessage);    
    context.done();
};
```

<a name="output"></a>

## <a name="event-hubs-output-binding"></a><span data-ttu-id="901df-131">Händelsehubbar utdatabindning</span><span class="sxs-lookup"><span data-stu-id="901df-131">Event Hubs output binding</span></span>
<span data-ttu-id="901df-132">Använda Händelsehubbar utdata bindning skriva händelser till en event hub händelseström.</span><span class="sxs-lookup"><span data-stu-id="901df-132">Use the Event Hubs output binding to write events to an event hub event stream.</span></span> <span data-ttu-id="901df-133">Du måste ha skicka behörighet till en händelsehubb skriva händelser till den.</span><span class="sxs-lookup"><span data-stu-id="901df-133">You must have send permission to an event hub to write events to it.</span></span>

<span data-ttu-id="901df-134">Utdata bindningen använder följande JSON-objekt i den `bindings` matris med function.json:</span><span class="sxs-lookup"><span data-stu-id="901df-134">The output binding uses the following JSON object in the `bindings` array of function.json:</span></span>

```json
{
    "type": "eventHub",
    "name": "<Name of output parameter in function signature>",
    "path": "<Name of event hub>",
    "connection": "<Name of app setting with connection string - see below>"
    "direction": "out"
}
```

<span data-ttu-id="901df-135">`connection`måste vara namnet på en appinställning som innehåller anslutningssträngen till den event hub-namnområdet.</span><span class="sxs-lookup"><span data-stu-id="901df-135">`connection` must be the name of an app setting that contains the connection string to the event hub's namespace.</span></span>
<span data-ttu-id="901df-136">Kopiera denna anslutningssträng genom att klicka på den **anslutningsinformationen** knappen för den *namnområde*, inte händelsehubben sig själv.</span><span class="sxs-lookup"><span data-stu-id="901df-136">Copy this connection string by clicking the **Connection Information** button for the *namespace*, not the event hub itself.</span></span> <span data-ttu-id="901df-137">Den här anslutningssträngen måste ha skicka behörighet att skicka meddelandet till händelseströmmen.</span><span class="sxs-lookup"><span data-stu-id="901df-137">This connection string must have send permissions to send the message to the event stream.</span></span>

## <a name="output-usage"></a><span data-ttu-id="901df-138">Användning av utdata</span><span class="sxs-lookup"><span data-stu-id="901df-138">Output usage</span></span>
<span data-ttu-id="901df-139">Det här avsnittet visar hur du använder din Händelsehubbar utdata bindningen i din funktionskoden.</span><span class="sxs-lookup"><span data-stu-id="901df-139">This section shows you how to use your Event Hubs output binding in your function code.</span></span>

<span data-ttu-id="901df-140">Du kan skicka meddelanden till den konfigurerade event hub med följande parameter:</span><span class="sxs-lookup"><span data-stu-id="901df-140">You can output messages to the configured event hub with the following parameter types:</span></span>

* `out string`
* <span data-ttu-id="901df-141">`ICollector<string>`(för att skicka flera meddelanden)</span><span class="sxs-lookup"><span data-stu-id="901df-141">`ICollector<string>` (to output multiple messages)</span></span>
* <span data-ttu-id="901df-142">`IAsyncCollector<string>`(asynkrona versionen av `ICollector<T>`)</span><span class="sxs-lookup"><span data-stu-id="901df-142">`IAsyncCollector<string>` (async version of `ICollector<T>`)</span></span>

<a name="outputsample"></a>

## <a name="output-sample"></a><span data-ttu-id="901df-143">Exempel på utdata</span><span class="sxs-lookup"><span data-stu-id="901df-143">Output sample</span></span>
<span data-ttu-id="901df-144">Anta att du har följande Händelsehubbar utdatabindning i den `bindings` matris med function.json:</span><span class="sxs-lookup"><span data-stu-id="901df-144">Suppose you have the following Event Hubs output binding in the `bindings` array of function.json:</span></span>

```json
{
    "type": "eventHub",
    "name": "outputEventHubMessage",
    "path": "myeventhub",
    "connection": "MyEventHubSend",
    "direction": "out"
}
```

<span data-ttu-id="901df-145">Se exemplet språkspecifika som skriver en händelse till även dataströmmen.</span><span class="sxs-lookup"><span data-stu-id="901df-145">See the language-specific sample that writes an event to the even stream.</span></span>

* [<span data-ttu-id="901df-146">C#</span><span class="sxs-lookup"><span data-stu-id="901df-146">C#</span></span>](#outcsharp)
* [<span data-ttu-id="901df-147">F#</span><span class="sxs-lookup"><span data-stu-id="901df-147">F#</span></span>](#outfsharp)
* [<span data-ttu-id="901df-148">Node.js</span><span class="sxs-lookup"><span data-stu-id="901df-148">Node.js</span></span>](#outnodejs)

<a name="outcsharp"></a>

### <a name="output-sample-in-c"></a><span data-ttu-id="901df-149">Utdata exemplet i C#</span><span class="sxs-lookup"><span data-stu-id="901df-149">Output sample in C#</span></span> #

```cs
using System;

public static void Run(TimerInfo myTimer, out string outputEventHubMessage, TraceWriter log)
{
    String msg = $"TimerTriggerCSharp1 executed at: {DateTime.Now}";
    log.Verbose(msg);   
    outputEventHubMessage = msg;
}
```

<span data-ttu-id="901df-150">Eller skapa flera meddelanden:</span><span class="sxs-lookup"><span data-stu-id="901df-150">Or, to create multiple messages:</span></span>

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

### <a name="output-sample-in-f"></a><span data-ttu-id="901df-151">Utdata exemplet i F #</span><span class="sxs-lookup"><span data-stu-id="901df-151">Output sample in F#</span></span> #

```fsharp
let Run(myTimer: TimerInfo, outputEventHubMessage: byref<string>, log: TraceWriter) =
    let msg = sprintf "TimerTriggerFSharp1 executed at: %s" DateTime.Now.ToString()
    log.Verbose(msg);
    outputEventHubMessage <- msg;
```

<a name="outnodejs"></a>

### <a name="output-sample-for-nodejs"></a><span data-ttu-id="901df-152">Exempel utdata för Node.js</span><span class="sxs-lookup"><span data-stu-id="901df-152">Output sample for Node.js</span></span>

```javascript
module.exports = function (context, myTimer) {
    var timeStamp = new Date().toISOString();
    context.log('Event Hub message created at: ', timeStamp);   
    context.bindings.outputEventHubMessage = "Event Hub message created at: " + timeStamp;
    context.done();
};
```

<span data-ttu-id="901df-153">Eller, för att skicka flera meddelanden</span><span class="sxs-lookup"><span data-stu-id="901df-153">Or, to send multiple messages,</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="901df-154">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="901df-154">Next steps</span></span>
[!INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)]
