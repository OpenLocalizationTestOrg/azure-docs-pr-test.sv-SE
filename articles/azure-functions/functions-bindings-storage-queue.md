---
title: aaaAzure funktioner queue storage bindningar | Microsoft Docs
description: "Förstå hur toouse Azure Storage utlösare och bindningar i Azure Functions."
services: functions
documentationcenter: na
author: lindydonna
manager: erikre
editor: 
tags: 
keywords: "Azure functions, funktioner, händelsebearbetning, dynamiska beräkning serverlösa arkitektur"
ms.assetid: 4e6a837d-e64f-45a0-87b7-aa02688a75f3
ms.service: functions
ms.devlang: multiple
ms.topic: reference
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 05/30/2017
ms.author: glenga
ms.openlocfilehash: 438b4f63e823149072c86fdefa7e15bfd2a2c4df
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-functions-queue-storage-bindings"></a>Azure Functions Queue Storage bindningar
[!INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

Den här artikeln beskriver hur tooconfigure och kod Azure Queue storage bindningar i Azure Functions. Azure Functions stöder utlösa och utgående bindningar för Azure köer. Funktioner som är tillgängliga i alla bindningar finns [Azure Functions-utlösare och bindningar begrepp](functions-triggers-bindings.md).

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

<a name="trigger"></a>

## <a name="queue-storage-trigger"></a>Queue storage utlösare
hello Azure Queue storage utlösare kan toomonitor en kö lagring för nya meddelanden och reagera toothem. 

Definiera en utlösare för kön med hello **integrera** fliken i hello Functions-portalen. hello skapar portalen hello definitionen i hello **bindningar** avsnitt i *function.json*:

```json
{
    "type": "queueTrigger",
    "direction": "in",
    "name": "<hello name used tooidentify hello trigger data in your code>",
    "queueName": "<Name of queue toopoll>",
    "connection":"<Name of app setting - see below>"
}
```

* Hej `connection` egenskap måste innehålla hello namnet på en appinställning som innehåller en anslutningssträng för lagring. I hello Azure-portalen, hello standardredigeraren i hello **integrera** appinställningen du konfigurerar när du väljer ett lagringskonto.

Fler inställningar kan anges i en [host.json filen](https://github.com/Azure/azure-webjobs-sdk-script/wiki/host.json) toofurther finjustera lagring-utlösare. Du kan till exempel ändra hello kön avsökningsintervall i host.json.

<a name="triggerusage"></a>

## <a name="using-a-queue-trigger"></a>Med hjälp av en kö-utlösare
Node.js-funktion åt hello kön data med `context.bindings.<name>`.


I .NET-funktioner åt hello kön nyttolast med en metodparameter `CloudQueueMessage paramName`. Här, `paramName` är hello-värde som du angav i hello [konfiguration av utlösare](#trigger). hälsningsmeddelande för kön kan vara avserialiserat tooany av hello följande typer:

* POCO-objekt. Använd om hello kön nyttolasten är ett JSON-objekt. hello Functions-runtime deserializes hello nyttolast i hello POCO-objekt. 
* `string`
* `byte[]`
* [`CloudQueueMessage`]

<a name="meta"></a>

### <a name="queue-trigger-metadata"></a>Kö utlösaren metadata
hello kö utlösaren innehåller flera metadataegenskaper för. De här egenskaperna kan användas som en del av bindande uttryck i andra bindningar eller parametrar i din kod. hello värden har hello samma semantik som [ `CloudQueueMessage` ].

* **QueueTrigger** -kön nyttolasten (om det är en giltig sträng)
* **DequeueCount** -typen `int`. Hej antalet gånger som det här meddelandet har har tagits bort.
* **ExpirationTime** -typen `DateTimeOffset?`. hello tid att hello-meddelande upphör att gälla.
* **ID** -typen `string`. Kön meddelande-ID.
* **InsertionTime** -typen `DateTimeOffset?`. hello tid att hello-meddelande har lagts till toohello kön.
* **NextVisibleTime** -typen `DateTimeOffset?`. hello tid att hello-meddelande visas bredvid.
* **PopReceipt** -typen `string`. hello-meddelande pop inleverans.

Se hur toouse hello kön metadata i [utlösaren exempel](#triggersample).

<a name="triggersample"></a>

## <a name="trigger-sample"></a>Utlösaren exempel
Anta att du har följande function.json som definierar en kö utlösare hello:

```json
{
    "disabled": false,
    "bindings": [
        {
            "type": "queueTrigger",
            "direction": "in",
            "name": "myQueueItem",
            "queueName": "myqueue-items",
            "connection":"MyStorageConnectionString"
        }
    ]
}
```

Se hello språkspecifika exempel som hämtar och loggar kön metadata.

* [C#](#triggercsharp)
* [Node.js](#triggernodejs)

<a name="triggercsharp"></a>

### <a name="trigger-sample-in-c"></a>Utlösaren exemplet i C# #
```csharp
#r "Microsoft.WindowsAzure.Storage"

using Microsoft.WindowsAzure.Storage.Queue;
using System;

public static void Run(CloudQueueMessage myQueueItem, 
    DateTimeOffset expirationTime, 
    DateTimeOffset insertionTime, 
    DateTimeOffset nextVisibleTime,
    string queueTrigger,
    string id,
    string popReceipt,
    int dequeueCount,
    TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {myQueueItem.AsString}\n" +
        $"queueTrigger={queueTrigger}\n" +
        $"expirationTime={expirationTime}\n" +
        $"insertionTime={insertionTime}\n" +
        $"nextVisibleTime={nextVisibleTime}\n" +
        $"id={id}\n" +
        $"popReceipt={popReceipt}\n" + 
        $"dequeueCount={dequeueCount}");
}
```

<!--
<a name="triggerfsharp"></a>
### Trigger sample in F# ## 
```fsharp

```
-->

<a name="triggernodejs"></a>

### <a name="trigger-sample-in-nodejs"></a>Utlösaren exemplet i Node.js

```javascript
module.exports = function (context) {
    context.log('Node.js queue trigger function processed work item', context.bindings.myQueueItem);
    context.log('queueTrigger =', context.bindingData.queueTrigger);
    context.log('expirationTime =', context.bindingData.expirationTime);
    context.log('insertionTime =', context.bindingData.insertionTime);
    context.log('nextVisibleTime =', context.bindingData.nextVisibleTime);
    context.log('id=', context.bindingData.id);
    context.log('popReceipt =', context.bindingData.popReceipt);
    context.log('dequeueCount =', context.bindingData.dequeueCount);
    context.done();
};
```

### <a name="handling-poison-queue-messages"></a>Hantering av skadligt Kömeddelanden
När en kö Utlösarfunktion misslyckas, försöker Azure Functions funktionen in toofive tider för en viss kö-meddelande, inklusive hello först försök. Om alla fem försök misslyckas hello functions-runtime lägger till meddelandet tooa queue storage med namnet  *&lt;originalqueuename >-skadligt*. Du kan skriva en funktion tooprocess meddelanden från hello skadligt kö med loggning av dem eller skicka ett meddelande om att manuella åtgärder krävs. 

toohandle förgiftade meddelanden manuellt, kontrollera hello `dequeueCount` av kön hello-meddelande (se [kö utlösaren metadata](#meta)).

<a name="output"></a>

## <a name="queue-storage-output-binding"></a>Queue storage-utdatabindning
hello Azure queue storage utdata bindning kan du toowrite tooa kön. 

Definierar en kö utdata bindning med hello **integrera** fliken i hello Functions-portalen. hello skapar portalen hello definitionen i hello **bindningar** avsnitt i *function.json*:

```json
{
   "type": "queue",
   "direction": "out",
   "name": "<hello name used tooidentify hello trigger data in your code>",
   "queueName": "<Name of queue toowrite to>",
   "connection":"<Name of app setting - see below>"
}
```

* Hej `connection` egenskap måste innehålla hello namnet på en appinställning som innehåller en anslutningssträng för lagring. I hello Azure-portalen, hello standardredigeraren i hello **integrera** appinställningen du konfigurerar när du väljer ett lagringskonto.

<a name="outputusage"></a>

## <a name="using-a-queue-output-binding"></a>Med hjälp av en kö utdatabindning
I Node.js-funktion, du åtkomst till hello utdata kön med `context.bindings.<name>`.

Du kan spara tooany av hello följande typer i .NET-funktioner. När det är en parameter av typen `T`, `T` måste vara något av hello stöds utdata typer som `string` eller en POCO.

* `out T`(serialiserad som JSON)
* `out string`
* `out byte[]`
* `out` [`CloudQueueMessage`] 
* `ICollector<T>`
* `IAsyncCollector<T>`
* [`CloudQueue`](/dotnet/api/microsoft.windowsazure.storage.queue.cloudqueue)

Du kan också använda hello-metodens returtyp som hello-utdatabindning.

<a name="outputsample"></a>

## <a name="queue-output-sample"></a>Kön utdata-exempel
hello följande *function.json* definierar en HTTP-utlösare med en kö utdatabindning:

```json
{
  "bindings": [
    {
      "type": "httpTrigger",
      "direction": "in",
      "authLevel": "function",
      "name": "input"
    },
    {
      "type": "http",
      "direction": "out",
      "name": "return"
    },
    {
      "type": "queue",
      "direction": "out",
      "name": "$return",
      "queueName": "outqueue",
      "connection": "MyStorageConnectionString",
    }
  ]
}
``` 

Se hello språkspecifika exempel som matar ut ett kömeddelande med hello inkommande HTTP-nyttolast.

* [C#](#outcsharp)
* [Node.js](#outnodejs)

<a name="outcsharp"></a>

### <a name="queue-output-sample-in-c"></a>Kön utdata exemplet i C# #

```cs
// C# example of HTTP trigger binding tooa custom POCO, with a queue output binding
public class CustomQueueMessage
{
    public string PersonName { get; set; }
    public string Title { get; set; }
}

public static CustomQueueMessage Run(CustomQueueMessage input, TraceWriter log)
{
    return input;
}
```

toosend flera meddelanden använder en `ICollector`:

```cs
public static void Run(CustomQueueMessage input, ICollector<CustomQueueMessage> myQueueItem, TraceWriter log)
{
    myQueueItem.Add(input);
    myQueueItem.Add(new CustomQueueMessage { PersonName = "You", Title = "None" });
}
```

<a name="outnodejs"></a>

### <a name="queue-output-sample-in-nodejs"></a>Kön utdata exemplet i Node.js

```javascript
module.exports = function (context, input) {
    context.done(null, input.body);
};
```

Eller toosend flera meddelanden

```javascript
module.exports = function(context) {
    // Define a message array for hello myQueueItem output binding. 
    context.bindings.myQueueItem = ["message 1","message 2"];
    context.done();
};
```

## <a name="next-steps"></a>Nästa steg

Ett exempel på en funktion som använder lagring-utlösare och bindningar finns [skapa ett Azure-tjänsten för Azure-funktion som anslutning tooan](functions-create-an-azure-connected-function.md).

[!INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)]

<!-- LINKS -->

['CloudQueueMessage']: /dotnet/api/microsoft.windowsazure.storage.queue.cloudqueuemessage
