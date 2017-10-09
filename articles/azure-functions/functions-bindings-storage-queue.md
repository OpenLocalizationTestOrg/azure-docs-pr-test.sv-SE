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
# <a name="azure-functions-queue-storage-bindings"></a><span data-ttu-id="1d3af-104">Azure Functions Queue Storage bindningar</span><span class="sxs-lookup"><span data-stu-id="1d3af-104">Azure Functions Queue Storage bindings</span></span>
[!INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

<span data-ttu-id="1d3af-105">Den här artikeln beskriver hur tooconfigure och kod Azure Queue storage bindningar i Azure Functions.</span><span class="sxs-lookup"><span data-stu-id="1d3af-105">This article describes how tooconfigure and code Azure Queue storage bindings in Azure Functions.</span></span> <span data-ttu-id="1d3af-106">Azure Functions stöder utlösa och utgående bindningar för Azure köer.</span><span class="sxs-lookup"><span data-stu-id="1d3af-106">Azure Functions supports trigger and output bindings for Azure queues.</span></span> <span data-ttu-id="1d3af-107">Funktioner som är tillgängliga i alla bindningar finns [Azure Functions-utlösare och bindningar begrepp](functions-triggers-bindings.md).</span><span class="sxs-lookup"><span data-stu-id="1d3af-107">For features that are available in all bindings, see [Azure Functions triggers and bindings concepts](functions-triggers-bindings.md).</span></span>

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

<a name="trigger"></a>

## <a name="queue-storage-trigger"></a><span data-ttu-id="1d3af-108">Queue storage utlösare</span><span class="sxs-lookup"><span data-stu-id="1d3af-108">Queue storage trigger</span></span>
<span data-ttu-id="1d3af-109">hello Azure Queue storage utlösare kan toomonitor en kö lagring för nya meddelanden och reagera toothem.</span><span class="sxs-lookup"><span data-stu-id="1d3af-109">hello Azure Queue storage trigger enables you toomonitor a queue storage for new messages and react toothem.</span></span> 

<span data-ttu-id="1d3af-110">Definiera en utlösare för kön med hello **integrera** fliken i hello Functions-portalen.</span><span class="sxs-lookup"><span data-stu-id="1d3af-110">Define a queue trigger using hello **Integrate** tab in hello Functions portal.</span></span> <span data-ttu-id="1d3af-111">hello skapar portalen hello definitionen i hello **bindningar** avsnitt i *function.json*:</span><span class="sxs-lookup"><span data-stu-id="1d3af-111">hello portal creates hello following definition in hello  **bindings** section of *function.json*:</span></span>

```json
{
    "type": "queueTrigger",
    "direction": "in",
    "name": "<hello name used tooidentify hello trigger data in your code>",
    "queueName": "<Name of queue toopoll>",
    "connection":"<Name of app setting - see below>"
}
```

* <span data-ttu-id="1d3af-112">Hej `connection` egenskap måste innehålla hello namnet på en appinställning som innehåller en anslutningssträng för lagring.</span><span class="sxs-lookup"><span data-stu-id="1d3af-112">hello `connection` property must contain hello name of an app setting that contains a storage connection string.</span></span> <span data-ttu-id="1d3af-113">I hello Azure-portalen, hello standardredigeraren i hello **integrera** appinställningen du konfigurerar när du väljer ett lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="1d3af-113">In hello Azure portal, hello standard editor in hello **Integrate** tab configures this app setting for you when you select a storage account.</span></span>

<span data-ttu-id="1d3af-114">Fler inställningar kan anges i en [host.json filen](https://github.com/Azure/azure-webjobs-sdk-script/wiki/host.json) toofurther finjustera lagring-utlösare.</span><span class="sxs-lookup"><span data-stu-id="1d3af-114">Additional settings can be provided in a [host.json file](https://github.com/Azure/azure-webjobs-sdk-script/wiki/host.json) toofurther fine-tune queue storage triggers.</span></span> <span data-ttu-id="1d3af-115">Du kan till exempel ändra hello kön avsökningsintervall i host.json.</span><span class="sxs-lookup"><span data-stu-id="1d3af-115">For example, you can change hello queue polling interval in host.json.</span></span>

<a name="triggerusage"></a>

## <a name="using-a-queue-trigger"></a><span data-ttu-id="1d3af-116">Med hjälp av en kö-utlösare</span><span class="sxs-lookup"><span data-stu-id="1d3af-116">Using a queue trigger</span></span>
<span data-ttu-id="1d3af-117">Node.js-funktion åt hello kön data med `context.bindings.<name>`.</span><span class="sxs-lookup"><span data-stu-id="1d3af-117">In Node.js functions, access hello queue data using `context.bindings.<name>`.</span></span>


<span data-ttu-id="1d3af-118">I .NET-funktioner åt hello kön nyttolast med en metodparameter `CloudQueueMessage paramName`.</span><span class="sxs-lookup"><span data-stu-id="1d3af-118">In .NET functions, access hello queue payload using a method parameter such as `CloudQueueMessage paramName`.</span></span> <span data-ttu-id="1d3af-119">Här, `paramName` är hello-värde som du angav i hello [konfiguration av utlösare](#trigger).</span><span class="sxs-lookup"><span data-stu-id="1d3af-119">Here, `paramName` is hello value you specified in hello [trigger configuration](#trigger).</span></span> <span data-ttu-id="1d3af-120">hälsningsmeddelande för kön kan vara avserialiserat tooany av hello följande typer:</span><span class="sxs-lookup"><span data-stu-id="1d3af-120">hello queue message can be deserialized tooany of hello following types:</span></span>

* <span data-ttu-id="1d3af-121">POCO-objekt.</span><span class="sxs-lookup"><span data-stu-id="1d3af-121">POCO object.</span></span> <span data-ttu-id="1d3af-122">Använd om hello kön nyttolasten är ett JSON-objekt.</span><span class="sxs-lookup"><span data-stu-id="1d3af-122">Use if hello queue payload is a JSON object.</span></span> <span data-ttu-id="1d3af-123">hello Functions-runtime deserializes hello nyttolast i hello POCO-objekt.</span><span class="sxs-lookup"><span data-stu-id="1d3af-123">hello Functions runtime deserializes hello payload into hello POCO object.</span></span> 
* `string`
* `byte[]`
* [`CloudQueueMessage`]

<a name="meta"></a>

### <a name="queue-trigger-metadata"></a><span data-ttu-id="1d3af-124">Kö utlösaren metadata</span><span class="sxs-lookup"><span data-stu-id="1d3af-124">Queue trigger metadata</span></span>
<span data-ttu-id="1d3af-125">hello kö utlösaren innehåller flera metadataegenskaper för.</span><span class="sxs-lookup"><span data-stu-id="1d3af-125">hello queue trigger provides several metadata properties.</span></span> <span data-ttu-id="1d3af-126">De här egenskaperna kan användas som en del av bindande uttryck i andra bindningar eller parametrar i din kod.</span><span class="sxs-lookup"><span data-stu-id="1d3af-126">These properties can be used as part of binding expressions in other bindings or as parameters in your code.</span></span> <span data-ttu-id="1d3af-127">hello värden har hello samma semantik som [ `CloudQueueMessage` ].</span><span class="sxs-lookup"><span data-stu-id="1d3af-127">hello values have hello same semantics as [`CloudQueueMessage`].</span></span>

* <span data-ttu-id="1d3af-128">**QueueTrigger** -kön nyttolasten (om det är en giltig sträng)</span><span class="sxs-lookup"><span data-stu-id="1d3af-128">**QueueTrigger** - queue payload (if a valid string)</span></span>
* <span data-ttu-id="1d3af-129">**DequeueCount** -typen `int`.</span><span class="sxs-lookup"><span data-stu-id="1d3af-129">**DequeueCount** - Type `int`.</span></span> <span data-ttu-id="1d3af-130">Hej antalet gånger som det här meddelandet har har tagits bort.</span><span class="sxs-lookup"><span data-stu-id="1d3af-130">hello number of times this message has been dequeued.</span></span>
* <span data-ttu-id="1d3af-131">**ExpirationTime** -typen `DateTimeOffset?`.</span><span class="sxs-lookup"><span data-stu-id="1d3af-131">**ExpirationTime** - Type `DateTimeOffset?`.</span></span> <span data-ttu-id="1d3af-132">hello tid att hello-meddelande upphör att gälla.</span><span class="sxs-lookup"><span data-stu-id="1d3af-132">hello time that hello message expires.</span></span>
* <span data-ttu-id="1d3af-133">**ID** -typen `string`.</span><span class="sxs-lookup"><span data-stu-id="1d3af-133">**Id** - Type `string`.</span></span> <span data-ttu-id="1d3af-134">Kön meddelande-ID.</span><span class="sxs-lookup"><span data-stu-id="1d3af-134">Queue message ID.</span></span>
* <span data-ttu-id="1d3af-135">**InsertionTime** -typen `DateTimeOffset?`.</span><span class="sxs-lookup"><span data-stu-id="1d3af-135">**InsertionTime** - Type `DateTimeOffset?`.</span></span> <span data-ttu-id="1d3af-136">hello tid att hello-meddelande har lagts till toohello kön.</span><span class="sxs-lookup"><span data-stu-id="1d3af-136">hello time that hello message was added toohello queue.</span></span>
* <span data-ttu-id="1d3af-137">**NextVisibleTime** -typen `DateTimeOffset?`.</span><span class="sxs-lookup"><span data-stu-id="1d3af-137">**NextVisibleTime** - Type `DateTimeOffset?`.</span></span> <span data-ttu-id="1d3af-138">hello tid att hello-meddelande visas bredvid.</span><span class="sxs-lookup"><span data-stu-id="1d3af-138">hello time that hello message will next be visible.</span></span>
* <span data-ttu-id="1d3af-139">**PopReceipt** -typen `string`.</span><span class="sxs-lookup"><span data-stu-id="1d3af-139">**PopReceipt** - Type `string`.</span></span> <span data-ttu-id="1d3af-140">hello-meddelande pop inleverans.</span><span class="sxs-lookup"><span data-stu-id="1d3af-140">hello message's pop receipt.</span></span>

<span data-ttu-id="1d3af-141">Se hur toouse hello kön metadata i [utlösaren exempel](#triggersample).</span><span class="sxs-lookup"><span data-stu-id="1d3af-141">See how toouse hello queue metadata in [Trigger sample](#triggersample).</span></span>

<a name="triggersample"></a>

## <a name="trigger-sample"></a><span data-ttu-id="1d3af-142">Utlösaren exempel</span><span class="sxs-lookup"><span data-stu-id="1d3af-142">Trigger sample</span></span>
<span data-ttu-id="1d3af-143">Anta att du har följande function.json som definierar en kö utlösare hello:</span><span class="sxs-lookup"><span data-stu-id="1d3af-143">Suppose you have hello following function.json that defines a queue trigger:</span></span>

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

<span data-ttu-id="1d3af-144">Se hello språkspecifika exempel som hämtar och loggar kön metadata.</span><span class="sxs-lookup"><span data-stu-id="1d3af-144">See hello language-specific sample that retrieves and logs queue metadata.</span></span>

* [<span data-ttu-id="1d3af-145">C#</span><span class="sxs-lookup"><span data-stu-id="1d3af-145">C#</span></span>](#triggercsharp)
* [<span data-ttu-id="1d3af-146">Node.js</span><span class="sxs-lookup"><span data-stu-id="1d3af-146">Node.js</span></span>](#triggernodejs)

<a name="triggercsharp"></a>

### <a name="trigger-sample-in-c"></a><span data-ttu-id="1d3af-147">Utlösaren exemplet i C#</span><span class="sxs-lookup"><span data-stu-id="1d3af-147">Trigger sample in C#</span></span> #
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

### <a name="trigger-sample-in-nodejs"></a><span data-ttu-id="1d3af-148">Utlösaren exemplet i Node.js</span><span class="sxs-lookup"><span data-stu-id="1d3af-148">Trigger sample in Node.js</span></span>

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

### <a name="handling-poison-queue-messages"></a><span data-ttu-id="1d3af-149">Hantering av skadligt Kömeddelanden</span><span class="sxs-lookup"><span data-stu-id="1d3af-149">Handling poison queue messages</span></span>
<span data-ttu-id="1d3af-150">När en kö Utlösarfunktion misslyckas, försöker Azure Functions funktionen in toofive tider för en viss kö-meddelande, inklusive hello först försök.</span><span class="sxs-lookup"><span data-stu-id="1d3af-150">When a queue trigger function fails, Azure Functions retries that function up toofive times for a given queue message, including hello first try.</span></span> <span data-ttu-id="1d3af-151">Om alla fem försök misslyckas hello functions-runtime lägger till meddelandet tooa queue storage med namnet  *&lt;originalqueuename >-skadligt*.</span><span class="sxs-lookup"><span data-stu-id="1d3af-151">If all five attempts fail, hello functions runtime adds a message tooa queue storage named *&lt;originalqueuename>-poison*.</span></span> <span data-ttu-id="1d3af-152">Du kan skriva en funktion tooprocess meddelanden från hello skadligt kö med loggning av dem eller skicka ett meddelande om att manuella åtgärder krävs.</span><span class="sxs-lookup"><span data-stu-id="1d3af-152">You can write a function tooprocess messages from hello poison queue by logging them or sending a  notification that manual attention is needed.</span></span> 

<span data-ttu-id="1d3af-153">toohandle förgiftade meddelanden manuellt, kontrollera hello `dequeueCount` av kön hello-meddelande (se [kö utlösaren metadata](#meta)).</span><span class="sxs-lookup"><span data-stu-id="1d3af-153">toohandle poison messages manually, check hello `dequeueCount` of hello queue message (see [Queue trigger metadata](#meta)).</span></span>

<a name="output"></a>

## <a name="queue-storage-output-binding"></a><span data-ttu-id="1d3af-154">Queue storage-utdatabindning</span><span class="sxs-lookup"><span data-stu-id="1d3af-154">Queue storage output binding</span></span>
<span data-ttu-id="1d3af-155">hello Azure queue storage utdata bindning kan du toowrite tooa kön.</span><span class="sxs-lookup"><span data-stu-id="1d3af-155">hello Azure queue storage output binding enables you toowrite messages tooa queue.</span></span> 

<span data-ttu-id="1d3af-156">Definierar en kö utdata bindning med hello **integrera** fliken i hello Functions-portalen.</span><span class="sxs-lookup"><span data-stu-id="1d3af-156">Define a queue output binding using hello **Integrate** tab in hello Functions portal.</span></span> <span data-ttu-id="1d3af-157">hello skapar portalen hello definitionen i hello **bindningar** avsnitt i *function.json*:</span><span class="sxs-lookup"><span data-stu-id="1d3af-157">hello portal creates hello following definition in hello  **bindings** section of *function.json*:</span></span>

```json
{
   "type": "queue",
   "direction": "out",
   "name": "<hello name used tooidentify hello trigger data in your code>",
   "queueName": "<Name of queue toowrite to>",
   "connection":"<Name of app setting - see below>"
}
```

* <span data-ttu-id="1d3af-158">Hej `connection` egenskap måste innehålla hello namnet på en appinställning som innehåller en anslutningssträng för lagring.</span><span class="sxs-lookup"><span data-stu-id="1d3af-158">hello `connection` property must contain hello name of an app setting that contains a storage connection string.</span></span> <span data-ttu-id="1d3af-159">I hello Azure-portalen, hello standardredigeraren i hello **integrera** appinställningen du konfigurerar när du väljer ett lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="1d3af-159">In hello Azure portal, hello standard editor in hello **Integrate** tab configures this app setting for you when you select a storage account.</span></span>

<a name="outputusage"></a>

## <a name="using-a-queue-output-binding"></a><span data-ttu-id="1d3af-160">Med hjälp av en kö utdatabindning</span><span class="sxs-lookup"><span data-stu-id="1d3af-160">Using a queue output binding</span></span>
<span data-ttu-id="1d3af-161">I Node.js-funktion, du åtkomst till hello utdata kön med `context.bindings.<name>`.</span><span class="sxs-lookup"><span data-stu-id="1d3af-161">In Node.js functions, you access hello output queue using `context.bindings.<name>`.</span></span>

<span data-ttu-id="1d3af-162">Du kan spara tooany av hello följande typer i .NET-funktioner.</span><span class="sxs-lookup"><span data-stu-id="1d3af-162">In .NET functions, you can output tooany of hello following types.</span></span> <span data-ttu-id="1d3af-163">När det är en parameter av typen `T`, `T` måste vara något av hello stöds utdata typer som `string` eller en POCO.</span><span class="sxs-lookup"><span data-stu-id="1d3af-163">When there is a type parameter `T`, `T` must be one of hello supported output types, such as `string` or a POCO.</span></span>

* <span data-ttu-id="1d3af-164">`out T`(serialiserad som JSON)</span><span class="sxs-lookup"><span data-stu-id="1d3af-164">`out T` (serialized as JSON)</span></span>
* `out string`
* `out byte[]`
* <span data-ttu-id="1d3af-165">`out` [`CloudQueueMessage`]</span><span class="sxs-lookup"><span data-stu-id="1d3af-165">`out` [`CloudQueueMessage`]</span></span> 
* `ICollector<T>`
* `IAsyncCollector<T>`
* [`CloudQueue`](/dotnet/api/microsoft.windowsazure.storage.queue.cloudqueue)

<span data-ttu-id="1d3af-166">Du kan också använda hello-metodens returtyp som hello-utdatabindning.</span><span class="sxs-lookup"><span data-stu-id="1d3af-166">You can also use hello method return type as hello output binding.</span></span>

<a name="outputsample"></a>

## <a name="queue-output-sample"></a><span data-ttu-id="1d3af-167">Kön utdata-exempel</span><span class="sxs-lookup"><span data-stu-id="1d3af-167">Queue output sample</span></span>
<span data-ttu-id="1d3af-168">hello följande *function.json* definierar en HTTP-utlösare med en kö utdatabindning:</span><span class="sxs-lookup"><span data-stu-id="1d3af-168">hello following *function.json* defines an HTTP trigger with a queue output binding:</span></span>

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

<span data-ttu-id="1d3af-169">Se hello språkspecifika exempel som matar ut ett kömeddelande med hello inkommande HTTP-nyttolast.</span><span class="sxs-lookup"><span data-stu-id="1d3af-169">See hello language-specific sample that outputs a queue message with hello incoming HTTP payload.</span></span>

* [<span data-ttu-id="1d3af-170">C#</span><span class="sxs-lookup"><span data-stu-id="1d3af-170">C#</span></span>](#outcsharp)
* [<span data-ttu-id="1d3af-171">Node.js</span><span class="sxs-lookup"><span data-stu-id="1d3af-171">Node.js</span></span>](#outnodejs)

<a name="outcsharp"></a>

### <a name="queue-output-sample-in-c"></a><span data-ttu-id="1d3af-172">Kön utdata exemplet i C#</span><span class="sxs-lookup"><span data-stu-id="1d3af-172">Queue output sample in C#</span></span> #

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

<span data-ttu-id="1d3af-173">toosend flera meddelanden använder en `ICollector`:</span><span class="sxs-lookup"><span data-stu-id="1d3af-173">toosend multiple messages, use an `ICollector`:</span></span>

```cs
public static void Run(CustomQueueMessage input, ICollector<CustomQueueMessage> myQueueItem, TraceWriter log)
{
    myQueueItem.Add(input);
    myQueueItem.Add(new CustomQueueMessage { PersonName = "You", Title = "None" });
}
```

<a name="outnodejs"></a>

### <a name="queue-output-sample-in-nodejs"></a><span data-ttu-id="1d3af-174">Kön utdata exemplet i Node.js</span><span class="sxs-lookup"><span data-stu-id="1d3af-174">Queue output sample in Node.js</span></span>

```javascript
module.exports = function (context, input) {
    context.done(null, input.body);
};
```

<span data-ttu-id="1d3af-175">Eller toosend flera meddelanden</span><span class="sxs-lookup"><span data-stu-id="1d3af-175">Or, toosend multiple messages,</span></span>

```javascript
module.exports = function(context) {
    // Define a message array for hello myQueueItem output binding. 
    context.bindings.myQueueItem = ["message 1","message 2"];
    context.done();
};
```

## <a name="next-steps"></a><span data-ttu-id="1d3af-176">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="1d3af-176">Next steps</span></span>

<span data-ttu-id="1d3af-177">Ett exempel på en funktion som använder lagring-utlösare och bindningar finns [skapa ett Azure-tjänsten för Azure-funktion som anslutning tooan](functions-create-an-azure-connected-function.md).</span><span class="sxs-lookup"><span data-stu-id="1d3af-177">For an example of a function that uses queue storage triggers and bindings, see [Create an Azure Function connected tooan Azure service](functions-create-an-azure-connected-function.md).</span></span>

[!INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)]

<!-- LINKS -->

['CloudQueueMessage']: /dotnet/api/microsoft.windowsazure.storage.queue.cloudqueuemessage
