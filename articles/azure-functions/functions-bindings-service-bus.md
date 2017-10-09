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
# <a name="azure-functions-service-bus-bindings"></a><span data-ttu-id="d9324-104">Azure Functions Service Bus-bindningar</span><span class="sxs-lookup"><span data-stu-id="d9324-104">Azure Functions Service Bus bindings</span></span>
[!INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

<span data-ttu-id="d9324-105">Den här artikeln förklarar hur tooconfigure och arbeta med Azure Service Bus-bindningar i Azure Functions.</span><span class="sxs-lookup"><span data-stu-id="d9324-105">This article explains how tooconfigure and work with Azure Service Bus bindings in Azure Functions.</span></span> 

<span data-ttu-id="d9324-106">Azure Functions stöder utlösa och utgående bindningar för Service Bus-köer och ämnen.</span><span class="sxs-lookup"><span data-stu-id="d9324-106">Azure Functions supports trigger and output bindings for Service Bus queues and topics.</span></span>

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

<a name="trigger"></a>

## <a name="service-bus-trigger"></a><span data-ttu-id="d9324-107">Service Bus-utlösare</span><span class="sxs-lookup"><span data-stu-id="d9324-107">Service Bus trigger</span></span>
<span data-ttu-id="d9324-108">Använda Service Bus hello utlösaren toorespond toomessages från en Service Bus-kö eller ett ämne.</span><span class="sxs-lookup"><span data-stu-id="d9324-108">Use hello Service Bus trigger toorespond toomessages from a Service Bus queue or topic.</span></span> 

<span data-ttu-id="d9324-109">hello Service Bus-kö och avsnittet utlösare definieras av hello följande JSON-objekt i hello `bindings` matris med function.json:</span><span class="sxs-lookup"><span data-stu-id="d9324-109">hello Service Bus queue and topic triggers are defined by hello following JSON objects in hello `bindings` array of function.json:</span></span>

* <span data-ttu-id="d9324-110">*kön* utlösare:</span><span class="sxs-lookup"><span data-stu-id="d9324-110">*queue* trigger:</span></span>

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

* <span data-ttu-id="d9324-111">*avsnittet* utlösare:</span><span class="sxs-lookup"><span data-stu-id="d9324-111">*topic* trigger:</span></span>

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

<span data-ttu-id="d9324-112">Observera följande hello:</span><span class="sxs-lookup"><span data-stu-id="d9324-112">Note hello following:</span></span>

* <span data-ttu-id="d9324-113">För `connection`, [skapa en appinställningen i appen funktionen](functions-how-to-use-azure-function-app-settings.md) som innehåller hello anslutning sträng tooyour Service Bus-namnrymd, ange hello namnet på hello appinställningen i hello `connection` egenskap i utlösaren.</span><span class="sxs-lookup"><span data-stu-id="d9324-113">For `connection`, [create an app setting in your function app](functions-how-to-use-azure-function-app-settings.md) that contains hello connection string tooyour Service Bus namespace, then specify hello name of hello app setting in hello `connection` property in your trigger.</span></span> <span data-ttu-id="d9324-114">Du hämta hello anslutningssträng genom att följa hello stegen som visas vid [hämta autentiseringsuppgifter för hantering av hello](../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md#obtain-the-management-credentials).</span><span class="sxs-lookup"><span data-stu-id="d9324-114">You obtain hello connection string by following hello steps shown at [Obtain hello management credentials](../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md#obtain-the-management-credentials).</span></span>
  <span data-ttu-id="d9324-115">hello anslutningssträngen måste vara för en Service Bus-namnrymd, inte begränsat tooa särskild kö eller avsnittet.</span><span class="sxs-lookup"><span data-stu-id="d9324-115">hello connection string must be for a Service Bus namespace, not limited tooa specific queue or topic.</span></span>
  <span data-ttu-id="d9324-116">Om du lämnar `connection` tom hello utlösaren förutsätter att en standard Service Bus-anslutningssträng har angetts i en app inställning med namnet `AzureWebJobsServiceBus`.</span><span class="sxs-lookup"><span data-stu-id="d9324-116">If you leave `connection` empty, hello trigger assumes that a default Service Bus connection string is specified in an app setting named `AzureWebJobsServiceBus`.</span></span>
* <span data-ttu-id="d9324-117">För `accessRights`, tillgängliga värden är `manage` och `listen`.</span><span class="sxs-lookup"><span data-stu-id="d9324-117">For `accessRights`, available values are `manage` and `listen`.</span></span> <span data-ttu-id="d9324-118">hello standardvärdet är `manage`, vilket anger att hello `connection` har hello **hantera** behörighet.</span><span class="sxs-lookup"><span data-stu-id="d9324-118">hello default is `manage`, which indicates that hello `connection` has hello **Manage** permission.</span></span> <span data-ttu-id="d9324-119">Om du använder en anslutningssträng som inte har hello **hantera** , behörighetsgrupp `accessRights` för`listen`.</span><span class="sxs-lookup"><span data-stu-id="d9324-119">If you use a connection string that does not have hello **Manage** permission, set `accessRights` too`listen`.</span></span> <span data-ttu-id="d9324-120">Annars hello Functions-runtime kan hantera misslyckas försöker toodo åtgärder som kräver rättigheter.</span><span class="sxs-lookup"><span data-stu-id="d9324-120">Otherwise, hello Functions runtime might fail trying toodo operations that require manage rights.</span></span>

## <a name="trigger-behavior"></a><span data-ttu-id="d9324-121">Utlösaren beteende</span><span class="sxs-lookup"><span data-stu-id="d9324-121">Trigger behavior</span></span>
* <span data-ttu-id="d9324-122">**Single-threading** – som standard hello funktioner runtime processer flera meddelanden samtidigt.</span><span class="sxs-lookup"><span data-stu-id="d9324-122">**Single-threading** - By default, hello Functions runtime processes multiple messages concurrently.</span></span> <span data-ttu-id="d9324-123">toodirect hello runtime tooprocess bara ett enda kö eller ett ämne meddelande samtidigt, ange `serviceBus.maxConcurrentCalls` too1 i *host.json*.</span><span class="sxs-lookup"><span data-stu-id="d9324-123">toodirect hello runtime tooprocess only a single queue or topic message at a time, set `serviceBus.maxConcurrentCalls` too1 in *host.json*.</span></span> 
  <span data-ttu-id="d9324-124">Information om *host.json*, se [mappstrukturen](functions-reference.md#folder-structure) och [host.json](https://github.com/Azure/azure-webjobs-sdk-script/wiki/host.json).</span><span class="sxs-lookup"><span data-stu-id="d9324-124">For information about *host.json*, see [Folder Structure](functions-reference.md#folder-structure) and [host.json](https://github .com/Azure/azure-webjobs-sdk-script/wiki/host.json).</span></span>
* <span data-ttu-id="d9324-125">**Hantering av förgiftade meddelanden** -Service Bus har sin egen hantering av skadligt meddelande som inte styrs eller konfigurerats i Azure Functions konfiguration eller kod.</span><span class="sxs-lookup"><span data-stu-id="d9324-125">**Poison message handling** - Service Bus does its own poison message handling, which can't be controlled or configured in Azure Functions configuration or code.</span></span> 
* <span data-ttu-id="d9324-126">**PeekLock beteende** -hello Functions-runtime tar emot ett meddelande i [ `PeekLock` läge](../service-bus-messaging/service-bus-performance-improvements.md#receive-mode) och anropar `Complete` på hello-meddelande om hello funktionen slutförs eller samtal `Abandon` om hello misslyckas åtgärden.</span><span class="sxs-lookup"><span data-stu-id="d9324-126">**PeekLock behavior** - hello Functions runtime receives a message in [`PeekLock` mode](../service-bus-messaging/service-bus-performance-improvements.md#receive-mode) and calls `Complete` on hello message if hello function finishes successfully, or calls `Abandon` if hello function fails.</span></span> 
  <span data-ttu-id="d9324-127">Om Hej funktionen körs längre än hello `PeekLock` timeout hello Lås förnyas automatiskt.</span><span class="sxs-lookup"><span data-stu-id="d9324-127">If hello function runs longer than hello `PeekLock` timeout, hello lock is automatically renewed.</span></span>

<a name="triggerusage"></a>

## <a name="trigger-usage"></a><span data-ttu-id="d9324-128">Utlösaren användning</span><span class="sxs-lookup"><span data-stu-id="d9324-128">Trigger usage</span></span>
<span data-ttu-id="d9324-129">Det här avsnittet visar hur toouse Service Bus utlösa i funktionskoden.</span><span class="sxs-lookup"><span data-stu-id="d9324-129">This section shows you how toouse your Service Bus trigger in your function code.</span></span> 

<span data-ttu-id="d9324-130">I C# och F # kan hälsningsmeddelande Service Bus-utlösare vara avserialiserat tooany av hello följande indatatyper:</span><span class="sxs-lookup"><span data-stu-id="d9324-130">In C# and F#, hello Service Bus trigger message can be deserialized tooany of hello following input types:</span></span>

* <span data-ttu-id="d9324-131">`string`-användbart för sträng meddelanden</span><span class="sxs-lookup"><span data-stu-id="d9324-131">`string` - useful for string messages</span></span>
* <span data-ttu-id="d9324-132">`byte[]`-användbart för binära data</span><span class="sxs-lookup"><span data-stu-id="d9324-132">`byte[]` - useful for binary data</span></span>
* <span data-ttu-id="d9324-133">Alla [objekt](https://msdn.microsoft.com/library/system.object.aspx) – det är användbart för JSON-serialiserade data.</span><span class="sxs-lookup"><span data-stu-id="d9324-133">Any [Object](https://msdn.microsoft.com/library/system.object.aspx) - useful for JSON-serialized data.</span></span>
  <span data-ttu-id="d9324-134">Om du exempelvis deklarera en anpassad Indatatyp `CustomType`, Azure Functions försöker toodeserialize hello JSON-data till den angivna typen.</span><span class="sxs-lookup"><span data-stu-id="d9324-134">If you declare a custom input type, such as `CustomType`, Azure Functions tries toodeserialize hello JSON data into your specified type.</span></span>
* <span data-ttu-id="d9324-135">`BrokeredMessage`-ger du hello avserialiseras meddelande med hello [BrokeredMessage.GetBody<T>()](https://msdn.microsoft.com/library/hh144211.aspx) metod.</span><span class="sxs-lookup"><span data-stu-id="d9324-135">`BrokeredMessage` - gives you hello deserialized message with hello [BrokeredMessage.GetBody<T>()](https://msdn.microsoft.com/library/hh144211.aspx) method.</span></span>

<span data-ttu-id="d9324-136">I Node.js skickas hello Service Bus utlösaren meddelande till funktionen hello som en sträng eller en JSON-objekt.</span><span class="sxs-lookup"><span data-stu-id="d9324-136">In Node.js, hello Service Bus trigger message is passed into hello function as either a string or JSON object.</span></span>

<a name="triggersample"></a>

## <a name="trigger-sample"></a><span data-ttu-id="d9324-137">Utlösaren exempel</span><span class="sxs-lookup"><span data-stu-id="d9324-137">Trigger sample</span></span>
<span data-ttu-id="d9324-138">Anta att du har följande function.json hello:</span><span class="sxs-lookup"><span data-stu-id="d9324-138">Suppose you have hello following function.json:</span></span>

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

<span data-ttu-id="d9324-139">Se hello språkspecifika prov som bearbetar ett meddelande för Service Bus-kö.</span><span class="sxs-lookup"><span data-stu-id="d9324-139">See hello language-specific sample that processes a Service Bus queue message.</span></span>

* [<span data-ttu-id="d9324-140">C#</span><span class="sxs-lookup"><span data-stu-id="d9324-140">C#</span></span>](#triggercsharp)
* [<span data-ttu-id="d9324-141">F#</span><span class="sxs-lookup"><span data-stu-id="d9324-141">F#</span></span>](#triggerfsharp)
* [<span data-ttu-id="d9324-142">Node.js</span><span class="sxs-lookup"><span data-stu-id="d9324-142">Node.js</span></span>](#triggernodejs)

<a name="triggercsharp"></a>

### <a name="trigger-sample-in-c"></a><span data-ttu-id="d9324-143">Utlösaren exemplet i C#</span><span class="sxs-lookup"><span data-stu-id="d9324-143">Trigger sample in C#</span></span> #

```cs
public static void Run(string myQueueItem, TraceWriter log)
{
    log.Info($"C# ServiceBus queue trigger function processed message: {myQueueItem}");
}
```

<a name="triggerfsharp"></a>

### <a name="trigger-sample-in-f"></a><span data-ttu-id="d9324-144">Utlösaren exemplet i F #</span><span class="sxs-lookup"><span data-stu-id="d9324-144">Trigger sample in F#</span></span> #

```fsharp
let Run(myQueueItem: string, log: TraceWriter) =
    log.Info(sprintf "F# ServiceBus queue trigger function processed message: %s" myQueueItem)
```

<a name="triggernodejs"></a>

### <a name="trigger-sample-in-nodejs"></a><span data-ttu-id="d9324-145">Utlösaren exemplet i Node.js</span><span class="sxs-lookup"><span data-stu-id="d9324-145">Trigger sample in Node.js</span></span>

```javascript
module.exports = function(context, myQueueItem) {
    context.log('Node.js ServiceBus queue trigger function processed message', myQueueItem);
    context.done();
};
```

<a name="output"></a>

## <a name="service-bus-output-binding"></a><span data-ttu-id="d9324-146">Service Bus-utdatabindning</span><span class="sxs-lookup"><span data-stu-id="d9324-146">Service Bus output binding</span></span>
<span data-ttu-id="d9324-147">hello Service Bus-kö och avsnittet utdata för en funktion använder hello följande JSON-objekt i hello `bindings` matris med function.json:</span><span class="sxs-lookup"><span data-stu-id="d9324-147">hello Service Bus queue and topic output for a function use hello following JSON objects in hello `bindings` array of function.json:</span></span>

* <span data-ttu-id="d9324-148">*kön* utdata:</span><span class="sxs-lookup"><span data-stu-id="d9324-148">*queue* output:</span></span>

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
* <span data-ttu-id="d9324-149">*avsnittet* utdata:</span><span class="sxs-lookup"><span data-stu-id="d9324-149">*topic* output:</span></span>

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

<span data-ttu-id="d9324-150">Observera följande hello:</span><span class="sxs-lookup"><span data-stu-id="d9324-150">Note hello following:</span></span>

* <span data-ttu-id="d9324-151">För `connection`, [skapa en appinställningen i appen funktionen](functions-how-to-use-azure-function-app-settings.md) som innehåller hello anslutning sträng tooyour Service Bus-namnrymd, ange hello namnet på hello appinställningen i hello `connection` egenskap i din utdata bindning.</span><span class="sxs-lookup"><span data-stu-id="d9324-151">For `connection`, [create an app setting in your function app](functions-how-to-use-azure-function-app-settings.md) that contains hello connection string tooyour Service Bus namespace, then specify hello name of hello app setting in hello `connection` property in your output binding.</span></span> <span data-ttu-id="d9324-152">Du hämta hello anslutningssträng genom att följa hello stegen som visas vid [hämta autentiseringsuppgifter för hantering av hello](../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md#obtain-the-management-credentials).</span><span class="sxs-lookup"><span data-stu-id="d9324-152">You obtain hello connection string by following hello steps shown at [Obtain hello management credentials](../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md#obtain-the-management-credentials).</span></span>
  <span data-ttu-id="d9324-153">hello anslutningssträngen måste vara för en Service Bus-namnrymd, inte begränsat tooa särskild kö eller avsnittet.</span><span class="sxs-lookup"><span data-stu-id="d9324-153">hello connection string must be for a Service Bus namespace, not limited tooa specific queue or topic.</span></span>
  <span data-ttu-id="d9324-154">Om du lämnar `connection` tom hello utdata bindning förutsätter att en standard Service Bus-anslutningssträng har angetts i en app inställning med namnet `AzureWebJobsServiceBus`.</span><span class="sxs-lookup"><span data-stu-id="d9324-154">If you leave `connection` empty, hello output binding assumes that a default Service Bus connection string is specified in an app setting named `AzureWebJobsServiceBus`.</span></span>
* <span data-ttu-id="d9324-155">För `accessRights`, tillgängliga värden är `manage` och `listen`.</span><span class="sxs-lookup"><span data-stu-id="d9324-155">For `accessRights`, available values are `manage` and `listen`.</span></span> <span data-ttu-id="d9324-156">hello standardvärdet är `manage`, vilket anger att hello `connection` har hello **hantera** behörighet.</span><span class="sxs-lookup"><span data-stu-id="d9324-156">hello default is `manage`, which indicates that hello `connection` has hello **Manage** permission.</span></span> <span data-ttu-id="d9324-157">Om du använder en anslutningssträng som inte har hello **hantera** , behörighetsgrupp `accessRights` för`listen`.</span><span class="sxs-lookup"><span data-stu-id="d9324-157">If you use a connection string that does not have hello **Manage** permission, set `accessRights` too`listen`.</span></span> <span data-ttu-id="d9324-158">Annars hello Functions-runtime kan hantera misslyckas försöker toodo åtgärder som kräver rättigheter.</span><span class="sxs-lookup"><span data-stu-id="d9324-158">Otherwise, hello Functions runtime might fail trying toodo operations that require manage rights.</span></span>

<a name="outputusage"></a>

## <a name="output-usage"></a><span data-ttu-id="d9324-159">Användning av utdata</span><span class="sxs-lookup"><span data-stu-id="d9324-159">Output usage</span></span>
<span data-ttu-id="d9324-160">Azure Functions kan skapa ett Service Bus-kömeddelande från någon av följande typer hello i C# och F #:</span><span class="sxs-lookup"><span data-stu-id="d9324-160">In C# and F#, Azure Functions can create a Service Bus queue message from any of hello following types:</span></span>

* <span data-ttu-id="d9324-161">Alla [objekt](https://msdn.microsoft.com/library/system.object.aspx) -parameterdefinitionen ser ut som `out T paramName` (C#).</span><span class="sxs-lookup"><span data-stu-id="d9324-161">Any [Object](https://msdn.microsoft.com/library/system.object.aspx) - Parameter definition looks like `out T paramName` (C#).</span></span>
  <span data-ttu-id="d9324-162">Funktioner deserializes hello objekt i ett JSON-meddelande.</span><span class="sxs-lookup"><span data-stu-id="d9324-162">Functions deserializes hello object into a JSON message.</span></span> <span data-ttu-id="d9324-163">Om hello värdet är null när hello funktionen avslutas skapar funktioner hello-meddelande med ett null-objekt.</span><span class="sxs-lookup"><span data-stu-id="d9324-163">If hello output value is null when hello function exits, Functions creates hello message with a null object.</span></span>
* <span data-ttu-id="d9324-164">`string`-Parameterdefinitionen ser ut som `out string paraName` (C#).</span><span class="sxs-lookup"><span data-stu-id="d9324-164">`string` - Parameter definition looks like `out string paraName` (C#).</span></span> <span data-ttu-id="d9324-165">Om hello parametervärdet är icke-null när hello funktionen avslutas, skapar ett meddelande i funktioner.</span><span class="sxs-lookup"><span data-stu-id="d9324-165">If hello parameter value is non-null when hello function exits, Functions creates a message.</span></span>
* <span data-ttu-id="d9324-166">`byte[]`-Parameterdefinitionen ser ut som `out byte[] paraName` (C#).</span><span class="sxs-lookup"><span data-stu-id="d9324-166">`byte[]` - Parameter definition looks like `out byte[] paraName` (C#).</span></span> <span data-ttu-id="d9324-167">Om hello parametervärdet är icke-null när hello funktionen avslutas, skapar ett meddelande i funktioner.</span><span class="sxs-lookup"><span data-stu-id="d9324-167">If hello parameter value is non-null when hello function exits, Functions creates a message.</span></span>
* <span data-ttu-id="d9324-168">`BrokeredMessage`Parameterdefinitionen ser ut som `out BrokeredMessage paraName` (C#).</span><span class="sxs-lookup"><span data-stu-id="d9324-168">`BrokeredMessage` Parameter definition looks like `out BrokeredMessage paraName` (C#).</span></span> <span data-ttu-id="d9324-169">Om hello parametervärdet är icke-null när hello funktionen avslutas, skapar ett meddelande i funktioner.</span><span class="sxs-lookup"><span data-stu-id="d9324-169">If hello parameter value is non-null when hello function exits, Functions creates a message.</span></span>

<span data-ttu-id="d9324-170">Du kan använda för att skapa flera meddelanden i en C#-funktion, `ICollector<T>` eller `IAsyncCollector<T>`.</span><span class="sxs-lookup"><span data-stu-id="d9324-170">For creating multiple messages in a C# function, you can use `ICollector<T>` or `IAsyncCollector<T>`.</span></span> <span data-ttu-id="d9324-171">Skapa ett meddelande när du anropar hello `Add` metod.</span><span class="sxs-lookup"><span data-stu-id="d9324-171">A message is created when you call hello `Add` method.</span></span>

<span data-ttu-id="d9324-172">I Node.js, kan du tilldela en sträng, en bytematris eller ett Javascript-objekt (deserialisera till JSON) för`context.binding.<paramName>`.</span><span class="sxs-lookup"><span data-stu-id="d9324-172">In Node.js, you can assign a string, a byte array, or a Javascript object (deserialized into JSON) too`context.binding.<paramName>`.</span></span>

<a name="outputsample"></a>

## <a name="output-sample"></a><span data-ttu-id="d9324-173">Exempel på utdata</span><span class="sxs-lookup"><span data-stu-id="d9324-173">Output sample</span></span>
<span data-ttu-id="d9324-174">Anta att du har följande function.json som definierar en Service Bus-kö utdata hello:</span><span class="sxs-lookup"><span data-stu-id="d9324-174">Suppose you have hello following function.json, that defines a Service Bus queue output:</span></span>

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

<span data-ttu-id="d9324-175">Se hello språkspecifika prov som skickar ett meddelande toohello service bus-kö.</span><span class="sxs-lookup"><span data-stu-id="d9324-175">See hello language-specific sample that sends a message toohello service bus queue.</span></span>

* [<span data-ttu-id="d9324-176">C#</span><span class="sxs-lookup"><span data-stu-id="d9324-176">C#</span></span>](#outcsharp)
* [<span data-ttu-id="d9324-177">F#</span><span class="sxs-lookup"><span data-stu-id="d9324-177">F#</span></span>](#outfsharp)
* [<span data-ttu-id="d9324-178">Node.js</span><span class="sxs-lookup"><span data-stu-id="d9324-178">Node.js</span></span>](#outnodejs)

<a name="outcsharp"></a>

### <a name="output-sample-in-c"></a><span data-ttu-id="d9324-179">Utdata exemplet i C#</span><span class="sxs-lookup"><span data-stu-id="d9324-179">Output sample in C#</span></span> #

```cs
public static void Run(TimerInfo myTimer, TraceWriter log, out string outputSbQueue)
{
    string message = $"Service Bus queue message created at: {DateTime.Now}";
    log.Info(message); 
    outputSbQueue = message;
}
```

<span data-ttu-id="d9324-180">Eller toocreate flera meddelanden:</span><span class="sxs-lookup"><span data-stu-id="d9324-180">Or, toocreate multiple messages:</span></span>

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

### <a name="output-sample-in-f"></a><span data-ttu-id="d9324-181">Utdata exemplet i F #</span><span class="sxs-lookup"><span data-stu-id="d9324-181">Output sample in F#</span></span> #

```fsharp
let Run(myTimer: TimerInfo, log: TraceWriter, outputSbQueue: byref<string>) =
    let message = sprintf "Service Bus queue message created at: %s" (DateTime.Now.ToString())
    log.Info(message)
    outputSbQueue = message
```

<a name="outnodejs"></a>

### <a name="output-sample-in-nodejs"></a><span data-ttu-id="d9324-182">Utdata exemplet i Node.js</span><span class="sxs-lookup"><span data-stu-id="d9324-182">Output sample in Node.js</span></span>

```javascript
module.exports = function (context, myTimer) {
    var message = 'Service Bus queue message created at ' + timeStamp;
    context.log(message);   
    context.bindings.outputSbQueueMsg = message;
    context.done();
};
```

<span data-ttu-id="d9324-183">Eller toocreate flera meddelanden:</span><span class="sxs-lookup"><span data-stu-id="d9324-183">Or, toocreate multiple messages:</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="d9324-184">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="d9324-184">Next steps</span></span>
[!INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)]

