---
title: "Azure Functions Service Bus-utlösare och bindningar | Microsoft Docs"
description: "Förstå hur du använder Azure Service Bus-utlösare och bindningar i Azure Functions."
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
ms.openlocfilehash: b3ee306cd37ebf88dc9369ccc2dc6c670557fd5a
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="azure-functions-service-bus-bindings"></a><span data-ttu-id="e6ce7-104">Azure Functions Service Bus-bindningar</span><span class="sxs-lookup"><span data-stu-id="e6ce7-104">Azure Functions Service Bus bindings</span></span>
[!INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

<span data-ttu-id="e6ce7-105">Den här artikeln beskriver hur du konfigurerar och arbetar med Azure Service Bus-bindningar i Azure Functions.</span><span class="sxs-lookup"><span data-stu-id="e6ce7-105">This article explains how to configure and work with Azure Service Bus bindings in Azure Functions.</span></span> 

<span data-ttu-id="e6ce7-106">Azure Functions stöder utlösa och utgående bindningar för Service Bus-köer och ämnen.</span><span class="sxs-lookup"><span data-stu-id="e6ce7-106">Azure Functions supports trigger and output bindings for Service Bus queues and topics.</span></span>

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

<a name="trigger"></a>

## <a name="service-bus-trigger"></a><span data-ttu-id="e6ce7-107">Service Bus-utlösare</span><span class="sxs-lookup"><span data-stu-id="e6ce7-107">Service Bus trigger</span></span>
<span data-ttu-id="e6ce7-108">Använda Service Bus-utlösaren ska svara på meddelanden från en Service Bus-kö eller ett ämne.</span><span class="sxs-lookup"><span data-stu-id="e6ce7-108">Use the Service Bus trigger to respond to messages from a Service Bus queue or topic.</span></span> 

<span data-ttu-id="e6ce7-109">Service Bus-kö och avsnittet utlösare definieras av följande JSON-objekt i den `bindings` matris med function.json:</span><span class="sxs-lookup"><span data-stu-id="e6ce7-109">The Service Bus queue and topic triggers are defined by the following JSON objects in the `bindings` array of function.json:</span></span>

* <span data-ttu-id="e6ce7-110">*kön* utlösare:</span><span class="sxs-lookup"><span data-stu-id="e6ce7-110">*queue* trigger:</span></span>

    ```json
    {
        "name" : "<Name of input parameter in function signature>",
        "queueName" : "<Name of the queue>",
        "connection" : "<Name of app setting that has your queue's connection string - see below>",
        "accessRights" : "<Access rights for the connection string - see below>",
        "type" : "serviceBusTrigger",
        "direction" : "in"
    }
    ```

* <span data-ttu-id="e6ce7-111">*avsnittet* utlösare:</span><span class="sxs-lookup"><span data-stu-id="e6ce7-111">*topic* trigger:</span></span>

    ```json
    {
        "name" : "<Name of input parameter in function signature>",
        "topicName" : "<Name of the topic>",
        "subscriptionName" : "<Name of the subscription>",
        "connection" : "<Name of app setting that has your topic's connection string - see below>",
        "accessRights" : "<Access rights for the connection string - see below>",
        "type" : "serviceBusTrigger",
        "direction" : "in"
    }
    ```

<span data-ttu-id="e6ce7-112">Observera följande:</span><span class="sxs-lookup"><span data-stu-id="e6ce7-112">Note the following:</span></span>

* <span data-ttu-id="e6ce7-113">För `connection`, [skapa en appinställningen i appen funktionen](functions-how-to-use-azure-function-app-settings.md) som innehåller anslutningssträngen till Service Bus-namnrymd, ange namnet på appinställningen i den `connection` egenskap i utlösaren.</span><span class="sxs-lookup"><span data-stu-id="e6ce7-113">For `connection`, [create an app setting in your function app](functions-how-to-use-azure-function-app-settings.md) that contains the connection string to your Service Bus namespace, then specify the name of the app setting in the `connection` property in your trigger.</span></span> <span data-ttu-id="e6ce7-114">Du hämta anslutningssträngen genom att följa stegen som visas vid [hämta autentiseringsuppgifter för hantering](../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md#obtain-the-management-credentials).</span><span class="sxs-lookup"><span data-stu-id="e6ce7-114">You obtain the connection string by following the steps shown at [Obtain the management credentials](../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md#obtain-the-management-credentials).</span></span>
  <span data-ttu-id="e6ce7-115">Anslutningssträngen måste vara för Service Bus-namnrymd inte begränsat till en särskild kö eller ett ämne.</span><span class="sxs-lookup"><span data-stu-id="e6ce7-115">The connection string must be for a Service Bus namespace, not limited to a specific queue or topic.</span></span>
  <span data-ttu-id="e6ce7-116">Om du lämnar `connection` tom utlösaren förutsätter att en standard Service Bus-anslutningssträng har angetts i en app inställning med namnet `AzureWebJobsServiceBus`.</span><span class="sxs-lookup"><span data-stu-id="e6ce7-116">If you leave `connection` empty, the trigger assumes that a default Service Bus connection string is specified in an app setting named `AzureWebJobsServiceBus`.</span></span>
* <span data-ttu-id="e6ce7-117">För `accessRights`, tillgängliga värden är `manage` och `listen`.</span><span class="sxs-lookup"><span data-stu-id="e6ce7-117">For `accessRights`, available values are `manage` and `listen`.</span></span> <span data-ttu-id="e6ce7-118">Standardvärdet är `manage`, vilket indikerar att den `connection` har den **hantera** behörighet.</span><span class="sxs-lookup"><span data-stu-id="e6ce7-118">The default is `manage`, which indicates that the `connection` has the **Manage** permission.</span></span> <span data-ttu-id="e6ce7-119">Om du använder en anslutningssträng som inte har den **hantera** , behörighetsgrupp `accessRights` till `listen`.</span><span class="sxs-lookup"><span data-stu-id="e6ce7-119">If you use a connection string that does not have the **Manage** permission, set `accessRights` to `listen`.</span></span> <span data-ttu-id="e6ce7-120">Annars kan hantera funktionerna runtime misslyckas försöker att utföra åtgärder som kräver rättigheter.</span><span class="sxs-lookup"><span data-stu-id="e6ce7-120">Otherwise, the Functions runtime might fail trying to do operations that require manage rights.</span></span>

## <a name="trigger-behavior"></a><span data-ttu-id="e6ce7-121">Utlösaren beteende</span><span class="sxs-lookup"><span data-stu-id="e6ce7-121">Trigger behavior</span></span>
* <span data-ttu-id="e6ce7-122">**Single-threading** – som standard funktioner runtime processer flera meddelanden samtidigt.</span><span class="sxs-lookup"><span data-stu-id="e6ce7-122">**Single-threading** - By default, the Functions runtime processes multiple messages concurrently.</span></span> <span data-ttu-id="e6ce7-123">För att dirigera runtime att bearbeta en enskild kö eller ett ämne meddelande i taget ange `serviceBus.maxConcurrentCalls` 1 i *host.json*.</span><span class="sxs-lookup"><span data-stu-id="e6ce7-123">To direct the runtime to process only a single queue or topic message at a time, set `serviceBus.maxConcurrentCalls` to 1 in *host.json*.</span></span> 
  <span data-ttu-id="e6ce7-124">Information om *host.json*, se [mappstrukturen](functions-reference.md#folder-structure) och [host.json](https://github.com/Azure/azure-webjobs-sdk-script/wiki/host.json).</span><span class="sxs-lookup"><span data-stu-id="e6ce7-124">For information about *host.json*, see [Folder Structure](functions-reference.md#folder-structure) and [host.json](https://github .com/Azure/azure-webjobs-sdk-script/wiki/host.json).</span></span>
* <span data-ttu-id="e6ce7-125">**Hantering av förgiftade meddelanden** -Service Bus har sin egen hantering av skadligt meddelande som inte styrs eller konfigurerats i Azure Functions konfiguration eller kod.</span><span class="sxs-lookup"><span data-stu-id="e6ce7-125">**Poison message handling** - Service Bus does its own poison message handling, which can't be controlled or configured in Azure Functions configuration or code.</span></span> 
* <span data-ttu-id="e6ce7-126">**PeekLock beteende** -Functions-runtime tar emot ett meddelande i [ `PeekLock` läge](../service-bus-messaging/service-bus-performance-improvements.md#receive-mode) och anropar `Complete` på meddelandet om funktionen slutförs eller samtal `Abandon` om misslyckas åtgärden.</span><span class="sxs-lookup"><span data-stu-id="e6ce7-126">**PeekLock behavior** - The Functions runtime receives a message in [`PeekLock` mode](../service-bus-messaging/service-bus-performance-improvements.md#receive-mode) and calls `Complete` on the message if the function finishes successfully, or calls `Abandon` if the function fails.</span></span> 
  <span data-ttu-id="e6ce7-127">Om funktionen körs längre än den `PeekLock` timeout låset förnyas automatiskt.</span><span class="sxs-lookup"><span data-stu-id="e6ce7-127">If the function runs longer than the `PeekLock` timeout, the lock is automatically renewed.</span></span>

<a name="triggerusage"></a>

## <a name="trigger-usage"></a><span data-ttu-id="e6ce7-128">Utlösaren användning</span><span class="sxs-lookup"><span data-stu-id="e6ce7-128">Trigger usage</span></span>
<span data-ttu-id="e6ce7-129">Det här avsnittet visar hur du använder Service Bus-utlösare i funktionskoden.</span><span class="sxs-lookup"><span data-stu-id="e6ce7-129">This section shows you how to use your Service Bus trigger in your function code.</span></span> 

<span data-ttu-id="e6ce7-130">I C# och F #, kan Service Bus utlösaren meddelandet avserialiseras till någon av följande typer av inkommande:</span><span class="sxs-lookup"><span data-stu-id="e6ce7-130">In C# and F#, the Service Bus trigger message can be deserialized to any of the following input types:</span></span>

* <span data-ttu-id="e6ce7-131">`string`-användbart för sträng meddelanden</span><span class="sxs-lookup"><span data-stu-id="e6ce7-131">`string` - useful for string messages</span></span>
* <span data-ttu-id="e6ce7-132">`byte[]`-användbart för binära data</span><span class="sxs-lookup"><span data-stu-id="e6ce7-132">`byte[]` - useful for binary data</span></span>
* <span data-ttu-id="e6ce7-133">Alla [objekt](https://msdn.microsoft.com/library/system.object.aspx) – det är användbart för JSON-serialiserade data.</span><span class="sxs-lookup"><span data-stu-id="e6ce7-133">Any [Object](https://msdn.microsoft.com/library/system.object.aspx) - useful for JSON-serialized data.</span></span>
  <span data-ttu-id="e6ce7-134">Om du exempelvis deklarera en anpassad Indatatyp `CustomType`, Azure Functions görs ett försök att deserialisera JSON-data till den angivna typen.</span><span class="sxs-lookup"><span data-stu-id="e6ce7-134">If you declare a custom input type, such as `CustomType`, Azure Functions tries to deserialize the JSON data into your specified type.</span></span>
* <span data-ttu-id="e6ce7-135">`BrokeredMessage`– ger dig avserialiserat meddelandet med den [BrokeredMessage.GetBody<T>()](https://msdn.microsoft.com/library/hh144211.aspx) metod.</span><span class="sxs-lookup"><span data-stu-id="e6ce7-135">`BrokeredMessage` - gives you the deserialized message with the [BrokeredMessage.GetBody<T>()](https://msdn.microsoft.com/library/hh144211.aspx) method.</span></span>

<span data-ttu-id="e6ce7-136">I Node.js skickas Service Bus utlösaren meddelandet till funktionen som en sträng eller en JSON-objekt.</span><span class="sxs-lookup"><span data-stu-id="e6ce7-136">In Node.js, the Service Bus trigger message is passed into the function as either a string or JSON object.</span></span>

<a name="triggersample"></a>

## <a name="trigger-sample"></a><span data-ttu-id="e6ce7-137">Utlösaren exempel</span><span class="sxs-lookup"><span data-stu-id="e6ce7-137">Trigger sample</span></span>
<span data-ttu-id="e6ce7-138">Anta att du har följande function.json:</span><span class="sxs-lookup"><span data-stu-id="e6ce7-138">Suppose you have the following function.json:</span></span>

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

<span data-ttu-id="e6ce7-139">Se exemplet språkspecifika som bearbetar ett meddelande för Service Bus-kö.</span><span class="sxs-lookup"><span data-stu-id="e6ce7-139">See the language-specific sample that processes a Service Bus queue message.</span></span>

* [<span data-ttu-id="e6ce7-140">C#</span><span class="sxs-lookup"><span data-stu-id="e6ce7-140">C#</span></span>](#triggercsharp)
* [<span data-ttu-id="e6ce7-141">F#</span><span class="sxs-lookup"><span data-stu-id="e6ce7-141">F#</span></span>](#triggerfsharp)
* [<span data-ttu-id="e6ce7-142">Node.js</span><span class="sxs-lookup"><span data-stu-id="e6ce7-142">Node.js</span></span>](#triggernodejs)

<a name="triggercsharp"></a>

### <a name="trigger-sample-in-c"></a><span data-ttu-id="e6ce7-143">Utlösaren exemplet i C#</span><span class="sxs-lookup"><span data-stu-id="e6ce7-143">Trigger sample in C#</span></span> #

```cs
public static void Run(string myQueueItem, TraceWriter log)
{
    log.Info($"C# ServiceBus queue trigger function processed message: {myQueueItem}");
}
```

<a name="triggerfsharp"></a>

### <a name="trigger-sample-in-f"></a><span data-ttu-id="e6ce7-144">Utlösaren exemplet i F #</span><span class="sxs-lookup"><span data-stu-id="e6ce7-144">Trigger sample in F#</span></span> #

```fsharp
let Run(myQueueItem: string, log: TraceWriter) =
    log.Info(sprintf "F# ServiceBus queue trigger function processed message: %s" myQueueItem)
```

<a name="triggernodejs"></a>

### <a name="trigger-sample-in-nodejs"></a><span data-ttu-id="e6ce7-145">Utlösaren exemplet i Node.js</span><span class="sxs-lookup"><span data-stu-id="e6ce7-145">Trigger sample in Node.js</span></span>

```javascript
module.exports = function(context, myQueueItem) {
    context.log('Node.js ServiceBus queue trigger function processed message', myQueueItem);
    context.done();
};
```

<a name="output"></a>

## <a name="service-bus-output-binding"></a><span data-ttu-id="e6ce7-146">Service Bus-utdatabindning</span><span class="sxs-lookup"><span data-stu-id="e6ce7-146">Service Bus output binding</span></span>
<span data-ttu-id="e6ce7-147">Service Bus-kö och avsnittet utdata för en funktion använder följande JSON-objekt i den `bindings` matris med function.json:</span><span class="sxs-lookup"><span data-stu-id="e6ce7-147">The Service Bus queue and topic output for a function use the following JSON objects in the `bindings` array of function.json:</span></span>

* <span data-ttu-id="e6ce7-148">*kön* utdata:</span><span class="sxs-lookup"><span data-stu-id="e6ce7-148">*queue* output:</span></span>

    ```json
    {
        "name" : "<Name of output parameter in function signature>",
        "queueName" : "<Name of the queue>",
        "connection" : "<Name of app setting that has your queue's connection string - see below>",
        "accessRights" : "<Access rights for the connection string - see below>",
        "type" : "serviceBus",
        "direction" : "out"
    }
    ```
* <span data-ttu-id="e6ce7-149">*avsnittet* utdata:</span><span class="sxs-lookup"><span data-stu-id="e6ce7-149">*topic* output:</span></span>

    ```json
    {
        "name" : "<Name of output parameter in function signature>",
        "topicName" : "<Name of the topic>",
        "subscriptionName" : "<Name of the subscription>",
        "connection" : "<Name of app setting that has your topic's connection string - see below>",
        "accessRights" : "<Access rights for the connection string - see below>",
        "type" : "serviceBus",
        "direction" : "out"
    }
    ```

<span data-ttu-id="e6ce7-150">Observera följande:</span><span class="sxs-lookup"><span data-stu-id="e6ce7-150">Note the following:</span></span>

* <span data-ttu-id="e6ce7-151">För `connection`, [skapa en appinställningen i appen funktionen](functions-how-to-use-azure-function-app-settings.md) som innehåller anslutningssträngen till Service Bus-namnrymd, ange namnet på appinställningen i den `connection` egenskapen i utdata-bindning.</span><span class="sxs-lookup"><span data-stu-id="e6ce7-151">For `connection`, [create an app setting in your function app](functions-how-to-use-azure-function-app-settings.md) that contains the connection string to your Service Bus namespace, then specify the name of the app setting in the `connection` property in your output binding.</span></span> <span data-ttu-id="e6ce7-152">Du hämta anslutningssträngen genom att följa stegen som visas vid [hämta autentiseringsuppgifter för hantering](../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md#obtain-the-management-credentials).</span><span class="sxs-lookup"><span data-stu-id="e6ce7-152">You obtain the connection string by following the steps shown at [Obtain the management credentials](../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md#obtain-the-management-credentials).</span></span>
  <span data-ttu-id="e6ce7-153">Anslutningssträngen måste vara för Service Bus-namnrymd inte begränsat till en särskild kö eller ett ämne.</span><span class="sxs-lookup"><span data-stu-id="e6ce7-153">The connection string must be for a Service Bus namespace, not limited to a specific queue or topic.</span></span>
  <span data-ttu-id="e6ce7-154">Om du lämnar `connection` tom utdata bindningen förutsätter att en standard Service Bus-anslutningssträng har angetts i en app inställning med namnet `AzureWebJobsServiceBus`.</span><span class="sxs-lookup"><span data-stu-id="e6ce7-154">If you leave `connection` empty, the output binding assumes that a default Service Bus connection string is specified in an app setting named `AzureWebJobsServiceBus`.</span></span>
* <span data-ttu-id="e6ce7-155">För `accessRights`, tillgängliga värden är `manage` och `listen`.</span><span class="sxs-lookup"><span data-stu-id="e6ce7-155">For `accessRights`, available values are `manage` and `listen`.</span></span> <span data-ttu-id="e6ce7-156">Standardvärdet är `manage`, vilket indikerar att den `connection` har den **hantera** behörighet.</span><span class="sxs-lookup"><span data-stu-id="e6ce7-156">The default is `manage`, which indicates that the `connection` has the **Manage** permission.</span></span> <span data-ttu-id="e6ce7-157">Om du använder en anslutningssträng som inte har den **hantera** , behörighetsgrupp `accessRights` till `listen`.</span><span class="sxs-lookup"><span data-stu-id="e6ce7-157">If you use a connection string that does not have the **Manage** permission, set `accessRights` to `listen`.</span></span> <span data-ttu-id="e6ce7-158">Annars kan hantera funktionerna runtime misslyckas försöker att utföra åtgärder som kräver rättigheter.</span><span class="sxs-lookup"><span data-stu-id="e6ce7-158">Otherwise, the Functions runtime might fail trying to do operations that require manage rights.</span></span>

<a name="outputusage"></a>

## <a name="output-usage"></a><span data-ttu-id="e6ce7-159">Användning av utdata</span><span class="sxs-lookup"><span data-stu-id="e6ce7-159">Output usage</span></span>
<span data-ttu-id="e6ce7-160">I C# och F #, kan Azure Functions skapa ett Service Bus-kö-meddelande från någon av följande typer:</span><span class="sxs-lookup"><span data-stu-id="e6ce7-160">In C# and F#, Azure Functions can create a Service Bus queue message from any of the following types:</span></span>

* <span data-ttu-id="e6ce7-161">Alla [objekt](https://msdn.microsoft.com/library/system.object.aspx) -parameterdefinitionen ser ut som `out T paramName` (C#).</span><span class="sxs-lookup"><span data-stu-id="e6ce7-161">Any [Object](https://msdn.microsoft.com/library/system.object.aspx) - Parameter definition looks like `out T paramName` (C#).</span></span>
  <span data-ttu-id="e6ce7-162">Funktioner deserializes objektet till en JSON-meddelande.</span><span class="sxs-lookup"><span data-stu-id="e6ce7-162">Functions deserializes the object into a JSON message.</span></span> <span data-ttu-id="e6ce7-163">Om värdet är null när funktionen avslutas skapar funktioner meddelandet med ett null-objekt.</span><span class="sxs-lookup"><span data-stu-id="e6ce7-163">If the output value is null when the function exits, Functions creates the message with a null object.</span></span>
* <span data-ttu-id="e6ce7-164">`string`-Parameterdefinitionen ser ut som `out string paraName` (C#).</span><span class="sxs-lookup"><span data-stu-id="e6ce7-164">`string` - Parameter definition looks like `out string paraName` (C#).</span></span> <span data-ttu-id="e6ce7-165">Om parametervärdet är icke-null när funktionen avslutas, skapar ett meddelande i funktioner.</span><span class="sxs-lookup"><span data-stu-id="e6ce7-165">If the parameter value is non-null when the function exits, Functions creates a message.</span></span>
* <span data-ttu-id="e6ce7-166">`byte[]`-Parameterdefinitionen ser ut som `out byte[] paraName` (C#).</span><span class="sxs-lookup"><span data-stu-id="e6ce7-166">`byte[]` - Parameter definition looks like `out byte[] paraName` (C#).</span></span> <span data-ttu-id="e6ce7-167">Om parametervärdet är icke-null när funktionen avslutas, skapar ett meddelande i funktioner.</span><span class="sxs-lookup"><span data-stu-id="e6ce7-167">If the parameter value is non-null when the function exits, Functions creates a message.</span></span>
* <span data-ttu-id="e6ce7-168">`BrokeredMessage`Parameterdefinitionen ser ut som `out BrokeredMessage paraName` (C#).</span><span class="sxs-lookup"><span data-stu-id="e6ce7-168">`BrokeredMessage` Parameter definition looks like `out BrokeredMessage paraName` (C#).</span></span> <span data-ttu-id="e6ce7-169">Om parametervärdet är icke-null när funktionen avslutas, skapar ett meddelande i funktioner.</span><span class="sxs-lookup"><span data-stu-id="e6ce7-169">If the parameter value is non-null when the function exits, Functions creates a message.</span></span>

<span data-ttu-id="e6ce7-170">Du kan använda för att skapa flera meddelanden i en C#-funktion, `ICollector<T>` eller `IAsyncCollector<T>`.</span><span class="sxs-lookup"><span data-stu-id="e6ce7-170">For creating multiple messages in a C# function, you can use `ICollector<T>` or `IAsyncCollector<T>`.</span></span> <span data-ttu-id="e6ce7-171">Skapa ett meddelande när du anropar den `Add` metoden.</span><span class="sxs-lookup"><span data-stu-id="e6ce7-171">A message is created when you call the `Add` method.</span></span>

<span data-ttu-id="e6ce7-172">I Node.js, du kan tilldela en sträng, en bytematris eller ett Javascript-objekt (deserialisera till JSON) `context.binding.<paramName>`.</span><span class="sxs-lookup"><span data-stu-id="e6ce7-172">In Node.js, you can assign a string, a byte array, or a Javascript object (deserialized into JSON) to `context.binding.<paramName>`.</span></span>

<a name="outputsample"></a>

## <a name="output-sample"></a><span data-ttu-id="e6ce7-173">Exempel på utdata</span><span class="sxs-lookup"><span data-stu-id="e6ce7-173">Output sample</span></span>
<span data-ttu-id="e6ce7-174">Anta att du har följande function.json, som definierar en Service Bus-kö utdata:</span><span class="sxs-lookup"><span data-stu-id="e6ce7-174">Suppose you have the following function.json, that defines a Service Bus queue output:</span></span>

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

<span data-ttu-id="e6ce7-175">Finns det språkspecifika exempel som skickar ett meddelande till service bus-kö.</span><span class="sxs-lookup"><span data-stu-id="e6ce7-175">See the language-specific sample that sends a message to the service bus queue.</span></span>

* [<span data-ttu-id="e6ce7-176">C#</span><span class="sxs-lookup"><span data-stu-id="e6ce7-176">C#</span></span>](#outcsharp)
* [<span data-ttu-id="e6ce7-177">F#</span><span class="sxs-lookup"><span data-stu-id="e6ce7-177">F#</span></span>](#outfsharp)
* [<span data-ttu-id="e6ce7-178">Node.js</span><span class="sxs-lookup"><span data-stu-id="e6ce7-178">Node.js</span></span>](#outnodejs)

<a name="outcsharp"></a>

### <a name="output-sample-in-c"></a><span data-ttu-id="e6ce7-179">Utdata exemplet i C#</span><span class="sxs-lookup"><span data-stu-id="e6ce7-179">Output sample in C#</span></span> #

```cs
public static void Run(TimerInfo myTimer, TraceWriter log, out string outputSbQueue)
{
    string message = $"Service Bus queue message created at: {DateTime.Now}";
    log.Info(message); 
    outputSbQueue = message;
}
```

<span data-ttu-id="e6ce7-180">Eller skapa flera meddelanden:</span><span class="sxs-lookup"><span data-stu-id="e6ce7-180">Or, to create multiple messages:</span></span>

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

### <a name="output-sample-in-f"></a><span data-ttu-id="e6ce7-181">Utdata exemplet i F #</span><span class="sxs-lookup"><span data-stu-id="e6ce7-181">Output sample in F#</span></span> #

```fsharp
let Run(myTimer: TimerInfo, log: TraceWriter, outputSbQueue: byref<string>) =
    let message = sprintf "Service Bus queue message created at: %s" (DateTime.Now.ToString())
    log.Info(message)
    outputSbQueue = message
```

<a name="outnodejs"></a>

### <a name="output-sample-in-nodejs"></a><span data-ttu-id="e6ce7-182">Utdata exemplet i Node.js</span><span class="sxs-lookup"><span data-stu-id="e6ce7-182">Output sample in Node.js</span></span>

```javascript
module.exports = function (context, myTimer) {
    var message = 'Service Bus queue message created at ' + timeStamp;
    context.log(message);   
    context.bindings.outputSbQueueMsg = message;
    context.done();
};
```

<span data-ttu-id="e6ce7-183">Eller skapa flera meddelanden:</span><span class="sxs-lookup"><span data-stu-id="e6ce7-183">Or, to create multiple messages:</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="e6ce7-184">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="e6ce7-184">Next steps</span></span>
[!INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)]

