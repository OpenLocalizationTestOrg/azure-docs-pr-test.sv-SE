---
title: "aaaAzure funktioner Service Bus utlösare och bindningar | Microsoft Docs"
description: "Förstå hur toouse Azure Service Bus utlösare och bindningar i Azure Functions."
services: functions
documentationcenter: na
author: christopheranderson
manager: erikre
editor: 
tags: 
keywords: "Azure functions, funktioner, händelsebearbetning, dynamiska beräkning serverlösa arkitektur"
ms.assetid: daedacf0-6546-4355-a65c-50873e74f66b
ms.service: functions
ms.devlang: multiple
ms.topic: reference
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 04/01/2017
ms.author: glenga
ms.openlocfilehash: dff9e89bd3840b8c11f91cae41e13502afc7aa60
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-functions-service-bus-bindings"></a>Azure Functions Service Bus-bindningar
[!INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

Den här artikeln förklarar hur tooconfigure och arbeta med Azure Service Bus-bindningar i Azure Functions. 

Azure Functions stöder utlösa och utgående bindningar för Service Bus-köer och ämnen.

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

<a name="trigger"></a>

## <a name="service-bus-trigger"></a>Service Bus-utlösare
Använda Service Bus hello utlösaren toorespond toomessages från en Service Bus-kö eller ett ämne. 

hello Service Bus-kö och avsnittet utlösare definieras av hello följande JSON-objekt i hello `bindings` matris med function.json:

* *kön* utlösare:

    ```json
    {
        "name" : "<Name of input parameter in function signature>",
        "queueName" : "<Name of hello queue>",
        "connection" : "<Name of app setting that has your queue's connection string - see below>",
        "accessRights" : "<Access rights for hello connection string - see below>",
        "type" : "serviceBusTrigger",
        "direction" : "in"
    }
    ```

* *avsnittet* utlösare:

    ```json
    {
        "name" : "<Name of input parameter in function signature>",
        "topicName" : "<Name of hello topic>",
        "subscriptionName" : "<Name of hello subscription>",
        "connection" : "<Name of app setting that has your topic's connection string - see below>",
        "accessRights" : "<Access rights for hello connection string - see below>",
        "type" : "serviceBusTrigger",
        "direction" : "in"
    }
    ```

Observera följande hello:

* För `connection`, [skapa en appinställningen i appen funktionen](functions-how-to-use-azure-function-app-settings.md) som innehåller hello anslutning sträng tooyour Service Bus-namnrymd, ange hello namnet på hello appinställningen i hello `connection` egenskap i utlösaren. Du hämta hello anslutningssträng genom att följa hello stegen som visas vid [hämta autentiseringsuppgifter för hantering av hello](../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md#obtain-the-management-credentials).
  hello anslutningssträngen måste vara för en Service Bus-namnrymd, inte begränsat tooa särskild kö eller avsnittet.
  Om du lämnar `connection` tom hello utlösaren förutsätter att en standard Service Bus-anslutningssträng har angetts i en app inställning med namnet `AzureWebJobsServiceBus`.
* För `accessRights`, tillgängliga värden är `manage` och `listen`. hello standardvärdet är `manage`, vilket anger att hello `connection` har hello **hantera** behörighet. Om du använder en anslutningssträng som inte har hello **hantera** , behörighetsgrupp `accessRights` för`listen`. Annars hello Functions-runtime kan hantera misslyckas försöker toodo åtgärder som kräver rättigheter.

## <a name="trigger-behavior"></a>Utlösaren beteende
* **Single-threading** – som standard hello funktioner runtime processer flera meddelanden samtidigt. toodirect hello runtime tooprocess bara ett enda kö eller ett ämne meddelande samtidigt, ange `serviceBus.maxConcurrentCalls` too1 i *host.json*. 
  Information om *host.json*, se [mappstrukturen](functions-reference.md#folder-structure) och [host.json](https://github.com/Azure/azure-webjobs-sdk-script/wiki/host.json).
* **Hantering av förgiftade meddelanden** -Service Bus har sin egen hantering av skadligt meddelande som inte styrs eller konfigurerats i Azure Functions konfiguration eller kod. 
* **PeekLock beteende** -hello Functions-runtime tar emot ett meddelande i [ `PeekLock` läge](../service-bus-messaging/service-bus-performance-improvements.md#receive-mode) och anropar `Complete` på hello-meddelande om hello funktionen slutförs eller samtal `Abandon` om hello misslyckas åtgärden. 
  Om Hej funktionen körs längre än hello `PeekLock` timeout hello Lås förnyas automatiskt.

<a name="triggerusage"></a>

## <a name="trigger-usage"></a>Utlösaren användning
Det här avsnittet visar hur toouse Service Bus utlösa i funktionskoden. 

I C# och F # kan hälsningsmeddelande Service Bus-utlösare vara avserialiserat tooany av hello följande indatatyper:

* `string`-användbart för sträng meddelanden
* `byte[]`-användbart för binära data
* Alla [objekt](https://msdn.microsoft.com/library/system.object.aspx) – det är användbart för JSON-serialiserade data.
  Om du exempelvis deklarera en anpassad Indatatyp `CustomType`, Azure Functions försöker toodeserialize hello JSON-data till den angivna typen.
* `BrokeredMessage`-ger du hello avserialiseras meddelande med hello [BrokeredMessage.GetBody<T>()](https://msdn.microsoft.com/library/hh144211.aspx) metod.

I Node.js skickas hello Service Bus utlösaren meddelande till funktionen hello som en sträng eller en JSON-objekt.

<a name="triggersample"></a>

## <a name="trigger-sample"></a>Utlösaren exempel
Anta att du har följande function.json hello:

```json
{
"bindings": [
    {
    "queueName": "testqueue",
    "connection": "MyServiceBusConnection",
    "name": "myQueueItem",
    "type": "serviceBusTrigger",
    "direction": "in"
    }
],
"disabled": false
}
```

Se hello språkspecifika prov som bearbetar ett meddelande för Service Bus-kö.

* [C#](#triggercsharp)
* [F#](#triggerfsharp)
* [Node.js](#triggernodejs)

<a name="triggercsharp"></a>

### <a name="trigger-sample-in-c"></a>Utlösaren exemplet i C# #

```cs
public static void Run(string myQueueItem, TraceWriter log)
{
    log.Info($"C# ServiceBus queue trigger function processed message: {myQueueItem}");
}
```

<a name="triggerfsharp"></a>

### <a name="trigger-sample-in-f"></a>Utlösaren exemplet i F # #

```fsharp
let Run(myQueueItem: string, log: TraceWriter) =
    log.Info(sprintf "F# ServiceBus queue trigger function processed message: %s" myQueueItem)
```

<a name="triggernodejs"></a>

### <a name="trigger-sample-in-nodejs"></a>Utlösaren exemplet i Node.js

```javascript
module.exports = function(context, myQueueItem) {
    context.log('Node.js ServiceBus queue trigger function processed message', myQueueItem);
    context.done();
};
```

<a name="output"></a>

## <a name="service-bus-output-binding"></a>Service Bus-utdatabindning
hello Service Bus-kö och avsnittet utdata för en funktion använder hello följande JSON-objekt i hello `bindings` matris med function.json:

* *kön* utdata:

    ```json
    {
        "name" : "<Name of output parameter in function signature>",
        "queueName" : "<Name of hello queue>",
        "connection" : "<Name of app setting that has your queue's connection string - see below>",
        "accessRights" : "<Access rights for hello connection string - see below>",
        "type" : "serviceBus",
        "direction" : "out"
    }
    ```
* *avsnittet* utdata:

    ```json
    {
        "name" : "<Name of output parameter in function signature>",
        "topicName" : "<Name of hello topic>",
        "subscriptionName" : "<Name of hello subscription>",
        "connection" : "<Name of app setting that has your topic's connection string - see below>",
        "accessRights" : "<Access rights for hello connection string - see below>",
        "type" : "serviceBus",
        "direction" : "out"
    }
    ```

Observera följande hello:

* För `connection`, [skapa en appinställningen i appen funktionen](functions-how-to-use-azure-function-app-settings.md) som innehåller hello anslutning sträng tooyour Service Bus-namnrymd, ange hello namnet på hello appinställningen i hello `connection` egenskap i din utdata bindning. Du hämta hello anslutningssträng genom att följa hello stegen som visas vid [hämta autentiseringsuppgifter för hantering av hello](../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md#obtain-the-management-credentials).
  hello anslutningssträngen måste vara för en Service Bus-namnrymd, inte begränsat tooa särskild kö eller avsnittet.
  Om du lämnar `connection` tom hello utdata bindning förutsätter att en standard Service Bus-anslutningssträng har angetts i en app inställning med namnet `AzureWebJobsServiceBus`.
* För `accessRights`, tillgängliga värden är `manage` och `listen`. hello standardvärdet är `manage`, vilket anger att hello `connection` har hello **hantera** behörighet. Om du använder en anslutningssträng som inte har hello **hantera** , behörighetsgrupp `accessRights` för`listen`. Annars hello Functions-runtime kan hantera misslyckas försöker toodo åtgärder som kräver rättigheter.

<a name="outputusage"></a>

## <a name="output-usage"></a>Användning av utdata
Azure Functions kan skapa ett Service Bus-kömeddelande från någon av följande typer hello i C# och F #:

* Alla [objekt](https://msdn.microsoft.com/library/system.object.aspx) -parameterdefinitionen ser ut som `out T paramName` (C#).
  Funktioner deserializes hello objekt i ett JSON-meddelande. Om hello värdet är null när hello funktionen avslutas skapar funktioner hello-meddelande med ett null-objekt.
* `string`-Parameterdefinitionen ser ut som `out string paraName` (C#). Om hello parametervärdet är icke-null när hello funktionen avslutas, skapar ett meddelande i funktioner.
* `byte[]`-Parameterdefinitionen ser ut som `out byte[] paraName` (C#). Om hello parametervärdet är icke-null när hello funktionen avslutas, skapar ett meddelande i funktioner.
* `BrokeredMessage`Parameterdefinitionen ser ut som `out BrokeredMessage paraName` (C#). Om hello parametervärdet är icke-null när hello funktionen avslutas, skapar ett meddelande i funktioner.

Du kan använda för att skapa flera meddelanden i en C#-funktion, `ICollector<T>` eller `IAsyncCollector<T>`. Skapa ett meddelande när du anropar hello `Add` metod.

I Node.js, kan du tilldela en sträng, en bytematris eller ett Javascript-objekt (deserialisera till JSON) för`context.binding.<paramName>`.

<a name="outputsample"></a>

## <a name="output-sample"></a>Exempel på utdata
Anta att du har följande function.json som definierar en Service Bus-kö utdata hello:

```json
{
    "bindings": [
        {
            "schedule": "0/15 * * * * *",
            "name": "myTimer",
            "runsOnStartup": true,
            "type": "timerTrigger",
            "direction": "in"
        },
        {
            "name": "outputSbQueue",
            "type": "serviceBus",
            "queueName": "testqueue",
            "connection": "MyServiceBusConnection",
            "direction": "out"
        }
    ],
    "disabled": false
}
```

Se hello språkspecifika prov som skickar ett meddelande toohello service bus-kö.

* [C#](#outcsharp)
* [F#](#outfsharp)
* [Node.js](#outnodejs)

<a name="outcsharp"></a>

### <a name="output-sample-in-c"></a>Utdata exemplet i C# #

```cs
public static void Run(TimerInfo myTimer, TraceWriter log, out string outputSbQueue)
{
    string message = $"Service Bus queue message created at: {DateTime.Now}";
    log.Info(message); 
    outputSbQueue = message;
}
```

Eller toocreate flera meddelanden:

```cs
public static void Run(TimerInfo myTimer, TraceWriter log, ICollector<string> outputSbQueue)
{
    string message = $"Service Bus queue message created at: {DateTime.Now}";
    log.Info(message); 
    outputSbQueue.Add("1 " + message);
    outputSbQueue.Add("2 " + message);
}
```

<a name="outfsharp"></a>

### <a name="output-sample-in-f"></a>Utdata exemplet i F # #

```fsharp
let Run(myTimer: TimerInfo, log: TraceWriter, outputSbQueue: byref<string>) =
    let message = sprintf "Service Bus queue message created at: %s" (DateTime.Now.ToString())
    log.Info(message)
    outputSbQueue = message
```

<a name="outnodejs"></a>

### <a name="output-sample-in-nodejs"></a>Utdata exemplet i Node.js

```javascript
module.exports = function (context, myTimer) {
    var message = 'Service Bus queue message created at ' + timeStamp;
    context.log(message);   
    context.bindings.outputSbQueueMsg = message;
    context.done();
};
```

Eller toocreate flera meddelanden:

```javascript
module.exports = function (context, myTimer) {
    var message = 'Service Bus queue message created at ' + timeStamp;
    context.log(message);   
    context.bindings.outputSbQueueMsg = [];
    context.bindings.outputSbQueueMsg.push("1 " + message);
    context.bindings.outputSbQueueMsg.push("2 " + message);
    context.done();
};
```

## <a name="next-steps"></a>Nästa steg
[!INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)]

