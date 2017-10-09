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
# <a name="azure-functions-event-hubs-bindings"></a>Azure Functions Händelsehubbar bindningar
[!INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

Den här artikeln förklarar hur tooconfigure och använda [Azure Event Hubs](../event-hubs/event-hubs-what-is-event-hubs.md) bindningar för Azure Functions.
Azure Functions stöder utlösa och utgående bindningar för Händelsehubbar.

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

Om du är ny tooAzure Händelsehubbar finns hello [översikt av Händelsehubbar](../event-hubs/event-hubs-what-is-event-hubs.md).

<a name="trigger"></a>

## <a name="event-hub-trigger"></a>Händelseutlösare hub
Använd hello Händelsehubbar utlöser toorespond tooan händelse skickas tooan event hub händelseströmmen. Du måste ha läsbehörighet toohello event hub tooset in hello utlösare.

Hej Händelsehubbar funktionen utlösaren använder hello följande JSON-objekt i hello `bindings` matris med function.json:

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

`consumerGroup`är en valfri egenskap används tooset hello [konsumentgrupp](../event-hubs/event-hubs-features.md#event-consumers) används toosubscribe tooevents i hello hubb. Om det utelämnas används hello `$Default` konsumentgrupp används.  
`connection`måste vara hello namnet på en appinställning som innehåller hello anslutning sträng toohello händelsehubbs namnutrymmet.
Kopiera denna anslutningssträng genom att klicka på hello **anslutningsinformationen** knapp för hello *namnområde*, inte hello händelsehubb sig själv. Den här anslutningssträngen måste ha minst Läs behörigheter tooactivate hello utlösare.

[Ytterligare inställningar](https://github.com/Azure/azure-webjobs-sdk-script/wiki/host.json) kan finnas i en host.json filen toofurther bra finjustera Händelsehubbar utlösare.  

<a name="triggerusage"></a>

## <a name="trigger-usage"></a>Utlösaren användning
När en funktion för Händelsehubbar utlösare utlöses skickas hello-meddelande som utlöser den till hello funktionen som en sträng.

<a name="triggersample"></a>

## <a name="trigger-sample"></a>Utlösaren exempel
Anta att du har följande Händelsehubbar utlösare i hello hello `bindings` matris med function.json:

```json
{
  "type": "eventHubTrigger",
  "name": "myEventHubMessage",
  "direction": "in",
  "path": "MyEventHub",
  "connection": "myEventHubReadConnectionString"
}
```

Se hello språkspecifika exempel loggar hello meddelandetexten hello event hub utlösare.

* [C#](#triggercsharp)
* [F#](#triggerfsharp)
* [Node.js](#triggernodejs)

<a name="triggercsharp"></a>

### <a name="trigger-sample-in-c"></a>Utlösaren exemplet i C# #

```cs
using System;

public static void Run(string myEventHubMessage, TraceWriter log)
{
    log.Info($"C# Event Hub trigger function processed a message: {myEventHubMessage}");
}
```

Du kan också få hello händelse som en [EventData](/dotnet/api/microsoft.servicebus.messaging.eventdata) -objektet, vilket ger dig åtkomst till metadata för toohello händelser.

```cs
#r "Microsoft.ServiceBus"
using System.Text;
using Microsoft.ServiceBus.Messaging;

public static void Run(EventData myEventHubMessage, TraceWriter log)
{
    log.Info($"{Encoding.UTF8.GetString(myEventHubMessage.GetBytes())}");
}
```

tooreceive händelser i en batch ändra hello Metodsignaturen för`string[]` eller `EventData[]`.

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

### <a name="trigger-sample-in-f"></a>Utlösaren exemplet i F # #

```fsharp
let Run(myEventHubMessage: string, log: TraceWriter) =
    log.Info(sprintf "F# eventhub trigger function processed work item: %s" myEventHubMessage)
```

<a name="triggernodejs"></a>

### <a name="trigger-sample-in-nodejs"></a>Utlösaren exemplet i Node.js

```javascript
module.exports = function (context, myEventHubMessage) {
    context.log('Node.js eventhub trigger function processed work item', myEventHubMessage);    
    context.done();
};
```

<a name="output"></a>

## <a name="event-hubs-output-binding"></a>Händelsehubbar utdatabindning
Använd hello Händelsehubbar utdata bindning toowrite händelser tooan event hub händelseströmmen. Du måste ha skicka behörighet tooan event hub toowrite händelser tooit.

hello utdata bindningen använder hello följande JSON-objekt i hello `bindings` matris med function.json:

```json
{
    "type": "eventHub",
    "name": "<Name of output parameter in function signature>",
    "path": "<Name of event hub>",
    "connection": "<Name of app setting with connection string - see below>"
    "direction": "out"
}
```

`connection`måste vara hello namnet på en appinställning som innehåller hello anslutning sträng toohello händelsehubbs namnutrymmet.
Kopiera denna anslutningssträng genom att klicka på hello **anslutningsinformationen** knapp för hello *namnområde*, inte hello händelsehubb sig själv. Den här anslutningssträngen måste ha skicka behörigheter toosend hello toohello händelse meddelandeströmmen.

## <a name="output-usage"></a>Användning av utdata
Det här avsnittet visar hur toouse Händelsehubbarna utdata bindningen i din funktionskoden.

Du kan spara händelsehubb för meddelanden toohello som konfigurerats med följande parametertyper hello:

* `out string`
* `ICollector<string>`(toooutput flera meddelanden)
* `IAsyncCollector<string>`(asynkrona versionen av `ICollector<T>`)

<a name="outputsample"></a>

## <a name="output-sample"></a>Exempel på utdata
Anta att du har följande hello Händelsehubbar utdatabindning i hello `bindings` matris med function.json:

```json
{
    "type": "eventHub",
    "name": "outputEventHubMessage",
    "path": "myeventhub",
    "connection": "MyEventHubSend",
    "direction": "out"
}
```

Se hello språkspecifika prov som skriver en händelse toohello även dataström.

* [C#](#outcsharp)
* [F#](#outfsharp)
* [Node.js](#outnodejs)

<a name="outcsharp"></a>

### <a name="output-sample-in-c"></a>Utdata exemplet i C# #

```cs
using System;

public static void Run(TimerInfo myTimer, out string outputEventHubMessage, TraceWriter log)
{
    String msg = $"TimerTriggerCSharp1 executed at: {DateTime.Now}";
    log.Verbose(msg);   
    outputEventHubMessage = msg;
}
```

Eller toocreate flera meddelanden:

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

### <a name="output-sample-in-f"></a>Utdata exemplet i F # #

```fsharp
let Run(myTimer: TimerInfo, outputEventHubMessage: byref<string>, log: TraceWriter) =
    let msg = sprintf "TimerTriggerFSharp1 executed at: %s" DateTime.Now.ToString()
    log.Verbose(msg);
    outputEventHubMessage <- msg;
```

<a name="outnodejs"></a>

### <a name="output-sample-for-nodejs"></a>Exempel utdata för Node.js

```javascript
module.exports = function (context, myTimer) {
    var timeStamp = new Date().toISOString();
    context.log('Event Hub message created at: ', timeStamp);   
    context.bindings.outputEventHubMessage = "Event Hub message created at: " + timeStamp;
    context.done();
};
```

Eller toosend flera meddelanden

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

## <a name="next-steps"></a>Nästa steg
[!INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)]
