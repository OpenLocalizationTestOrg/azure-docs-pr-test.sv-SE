---
title: aaaAzure funktioner Mobile Apps bindningar | Microsoft Docs
description: "Förstå hur toouse Azure Mobile Apps bindningar i Azure Functions."
services: functions
documentationcenter: na
author: ggailey777
manager: erikre
editor: 
tags: 
keywords: "Azure functions, funktioner, händelsebearbetning, dynamiska beräkning serverlösa arkitektur"
ms.assetid: faad1263-0fa5-41a9-964f-aecbc0be706a
ms.service: functions
ms.devlang: multiple
ms.topic: reference
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 10/31/2016
ms.author: glenga
ms.openlocfilehash: d3679a5d5c66705b32e422ec17e3a1e6d6ac063c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-functions-mobile-apps-bindings"></a><span data-ttu-id="55c8b-104">Azure Functions Mobile Apps bindningar</span><span class="sxs-lookup"><span data-stu-id="55c8b-104">Azure Functions Mobile Apps bindings</span></span>
[!INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

<span data-ttu-id="55c8b-105">Den här artikeln förklarar hur tooconfigure och kod [Azure Mobile Apps](../app-service-mobile/app-service-mobile-value-prop.md) bindningar i Azure Functions.</span><span class="sxs-lookup"><span data-stu-id="55c8b-105">This article explains how tooconfigure and code [Azure Mobile Apps](../app-service-mobile/app-service-mobile-value-prop.md) bindings in Azure Functions.</span></span> <span data-ttu-id="55c8b-106">Azure Functions stöder indata och utdata bindningar för Mobile Apps.</span><span class="sxs-lookup"><span data-stu-id="55c8b-106">Azure Functions supports input and output bindings for Mobile Apps.</span></span>

<span data-ttu-id="55c8b-107">hello Mobile Apps indata och utdata-bindningar kan du [läsa och skriva toodata tabeller](../app-service-mobile/app-service-mobile-node-backend-how-to-use-server-sdk.md#TableOperations) i din mobila app.</span><span class="sxs-lookup"><span data-stu-id="55c8b-107">hello Mobile Apps input and output bindings let you [read from and write toodata tables](../app-service-mobile/app-service-mobile-node-backend-how-to-use-server-sdk.md#TableOperations) in your mobile app.</span></span>

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

<a name="input"></a>

## <a name="mobile-apps-input-binding"></a><span data-ttu-id="55c8b-108">Mobile Apps indatabindning</span><span class="sxs-lookup"><span data-stu-id="55c8b-108">Mobile Apps input binding</span></span>
<span data-ttu-id="55c8b-109">hello Mobile Apps indatabindning läser en post från en mobil tabell slutpunkt och skickar det till din funktion.</span><span class="sxs-lookup"><span data-stu-id="55c8b-109">hello Mobile Apps input binding loads a record from a mobile table endpoint and passes it into your function.</span></span> <span data-ttu-id="55c8b-110">I C# och F # funktioner, alla ändringar som gjorts toohello post skickas automatiskt tillbaka toohello tabellen när hello funktionen avslutas korrekt.</span><span class="sxs-lookup"><span data-stu-id="55c8b-110">In a C# and F# functions, any changes made toohello record are automatically sent back toohello table when hello function exits successfully.</span></span>

<span data-ttu-id="55c8b-111">hello Mobile Apps indata tooa funktion använder hello följande JSON-objekt i hello `bindings` matris med function.json:</span><span class="sxs-lookup"><span data-stu-id="55c8b-111">hello Mobile Apps input tooa function uses hello following JSON object in hello `bindings` array of function.json:</span></span>

```json
{
    "name": "<Name of input parameter in function signature>",
    "type": "mobileTable",
    "tableName": "<Name of your mobile app's data table>",
    "id" : "<Id of hello record tooretrieve - see below>",
    "connection": "<Name of app setting that has your mobile app's URL - see below>",
    "apiKey": "<Name of app setting that has your mobile app's API key - see below>",
    "direction": "in"
}
```

<span data-ttu-id="55c8b-112">Observera följande hello:</span><span class="sxs-lookup"><span data-stu-id="55c8b-112">Note hello following:</span></span>

* <span data-ttu-id="55c8b-113">`id`kan vara statisk eller det kan vara baserad på hello utlösare som anropar hello-funktionen.</span><span class="sxs-lookup"><span data-stu-id="55c8b-113">`id` can be static, or it can be based on hello trigger that invokes hello function.</span></span> <span data-ttu-id="55c8b-114">Om du använder till exempel en [kö utlösaren]() för din funktion sedan `"id": "{queueTrigger}"` använder hello strängvärdet för kön hälsningsmeddelande som hello poster ID tooretrieve.</span><span class="sxs-lookup"><span data-stu-id="55c8b-114">For example, if you use a [queue trigger]() for your function, then `"id": "{queueTrigger}"` uses hello string value of hello queue message as hello record ID tooretrieve.</span></span>
* <span data-ttu-id="55c8b-115">`connection`ska innehålla hello namnet på en appinställning i funktionen appen, som i sin tur innehåller hello URL för din mobila app.</span><span class="sxs-lookup"><span data-stu-id="55c8b-115">`connection` should contain hello name of an app setting in your function app, which in turn contains hello URL of your mobile app.</span></span> <span data-ttu-id="55c8b-116">hello funktionen använder den här URL: en tooconstruct hello krävs REST-åtgärder mot din mobila app.</span><span class="sxs-lookup"><span data-stu-id="55c8b-116">hello function uses this URL tooconstruct hello required REST operations against your mobile app.</span></span> <span data-ttu-id="55c8b-117">Du [skapa en appinställningen i appen funktionen]() som innehåller din mobila app-URL (som ser ut så `http://<appname>.azurewebsites.net`), ange hello namnet på hello appinställningen i hello `connection` egenskap i den inkommande bindningen.</span><span class="sxs-lookup"><span data-stu-id="55c8b-117">You [create an app setting in your function app]() that contains your mobile app's URL (which looks like `http://<appname>.azurewebsites.net`), then specify hello name of hello app setting in hello `connection` property in your input binding.</span></span> 
* <span data-ttu-id="55c8b-118">Du behöver toospecify `apiKey` om du [implementera en API-nyckel i mobilappsserverdelen Node.js](https://github.com/Azure/azure-mobile-apps-node/tree/master/samples/api-key), eller [implementera en API-nyckel i mobilappsserverdelen .NET](https://github.com/Azure/azure-mobile-apps-net-server/wiki/Implementing-Application-Key).</span><span class="sxs-lookup"><span data-stu-id="55c8b-118">You need toospecify `apiKey` if you [implement an API key in your Node.js mobile app backend](https://github.com/Azure/azure-mobile-apps-node/tree/master/samples/api-key), or [implement an API key in your .NET mobile app backend](https://github.com/Azure/azure-mobile-apps-net-server/wiki/Implementing-Application-Key).</span></span> <span data-ttu-id="55c8b-119">toodo kan du [skapa en appinställningen i appen funktionen]() som innehåller hello API-nyckel, lägga till hello `apiKey` egenskap i din indatabindning med hello namnet hello appinställningen.</span><span class="sxs-lookup"><span data-stu-id="55c8b-119">toodo this, you [create an app setting in your function app]() that contains hello API key, then add hello `apiKey` property in your input binding with hello name of hello app setting.</span></span> 
  
  > [!IMPORTANT]
  > <span data-ttu-id="55c8b-120">Den här API-nyckeln måste inte delas med din mobila app-klienter.</span><span class="sxs-lookup"><span data-stu-id="55c8b-120">This API key must not be shared with your mobile app clients.</span></span> <span data-ttu-id="55c8b-121">Det bör bara vara distribuerad på ett säkert sätt tooservice sida klienter, till exempel Azure Functions.</span><span class="sxs-lookup"><span data-stu-id="55c8b-121">It should only be distributed securely tooservice-side clients, like Azure Functions.</span></span> 
  > 
  > [!NOTE]
  > <span data-ttu-id="55c8b-122">Azure Functions lagrar dina anslutningsinformationen och API-nycklar som app-inställningar så att de inte kontrolleras i din källkontroll.</span><span class="sxs-lookup"><span data-stu-id="55c8b-122">Azure Functions stores your connection information and API keys as app settings so that they are not checked into your source control repository.</span></span> <span data-ttu-id="55c8b-123">Detta skyddar känslig information.</span><span class="sxs-lookup"><span data-stu-id="55c8b-123">This safeguards your sensitive information.</span></span>
  > 
  > 

<a name="inputusage"></a>

## <a name="input-usage"></a><span data-ttu-id="55c8b-124">Inkommande användning</span><span class="sxs-lookup"><span data-stu-id="55c8b-124">Input usage</span></span>
<span data-ttu-id="55c8b-125">Det här avsnittet visar hur toouse Mobile Apps inkommande bindningen i din funktionskoden.</span><span class="sxs-lookup"><span data-stu-id="55c8b-125">This section shows you how toouse your Mobile Apps input binding in your function code.</span></span> 

<span data-ttu-id="55c8b-126">När hello post med hello angiven tabell och post-ID hittas, skickas detta till hello med namnet [JObject](http://www.newtonsoft.com/json/help/html/t_newtonsoft_json_linq_jobject.htm) parameter (eller i Node.js, den överförs till hello `context.bindings.<name>` objekt).</span><span class="sxs-lookup"><span data-stu-id="55c8b-126">When hello record with hello specified table and record ID is found, it is passed into hello named [JObject](http://www.newtonsoft.com/json/help/html/t_newtonsoft_json_linq_jobject.htm) parameter (or, in Node.js, it is passed into hello `context.bindings.<name>` object).</span></span> <span data-ttu-id="55c8b-127">När hello-post inte hittas hello-parametern är `null`.</span><span class="sxs-lookup"><span data-stu-id="55c8b-127">When hello record is not found, hello parameter is `null`.</span></span> 

<span data-ttu-id="55c8b-128">I C# och F # funktioner, alla ändringar du gör toohello indata post (Indataparametern) skickas automatiskt tillbaka toohello Mobile Apps tabellen när hello funktionen avslutas korrekt.</span><span class="sxs-lookup"><span data-stu-id="55c8b-128">In C# and F# functions, any changes you make toohello input record (input parameter) is automatically sent back toohello Mobile Apps table when hello function exits successfully.</span></span> <span data-ttu-id="55c8b-129">I Node.js-funktion, använda `context.bindings.<name>` tooaccess hello Indataposten.</span><span class="sxs-lookup"><span data-stu-id="55c8b-129">In Node.js functions, use `context.bindings.<name>` tooaccess hello input record.</span></span> <span data-ttu-id="55c8b-130">Du kan inte ändra en post i Node.js.</span><span class="sxs-lookup"><span data-stu-id="55c8b-130">You cannot modify a record in Node.js.</span></span>

<a name="inputsample"></a>

## <a name="input-sample"></a><span data-ttu-id="55c8b-131">Indata-exempel</span><span class="sxs-lookup"><span data-stu-id="55c8b-131">Input sample</span></span>
<span data-ttu-id="55c8b-132">Anta att du har följande function.json hello hämtas som en Mobilapp tabell post med hälsningsmeddelande kö utlösaren hello-id:</span><span class="sxs-lookup"><span data-stu-id="55c8b-132">Suppose you have hello following function.json, that retrieves a Mobile App table record with hello id of hello queue trigger message:</span></span>

```json
{
"bindings": [
    {
    "name": "myQueueItem",
    "queueName": "myqueue-items",
    "connection":"",
    "type": "queueTrigger",
    "direction": "in"
    },
    {
        "name": "record",
        "type": "mobileTable",
        "tableName": "MyTable",
        "id" : "{queueTrigger}",
        "connection": "My_MobileApp_Url",
        "apiKey": "My_MobileApp_Key",
        "direction": "in"
    }
],
"disabled": false
}
```

<span data-ttu-id="55c8b-133">Se hello språkspecifika exempel som använder hello Indataposten från hello bindning.</span><span class="sxs-lookup"><span data-stu-id="55c8b-133">See hello language-specific sample that uses hello input record from hello binding.</span></span> <span data-ttu-id="55c8b-134">hello C# och F #-exempel också ändra hello post `text` egenskapen.</span><span class="sxs-lookup"><span data-stu-id="55c8b-134">hello C# and F# samples also modify hello record's `text` property.</span></span>

* [<span data-ttu-id="55c8b-135">C#</span><span class="sxs-lookup"><span data-stu-id="55c8b-135">C#</span></span>](#inputcsharp)
* [<span data-ttu-id="55c8b-136">Node.js</span><span class="sxs-lookup"><span data-stu-id="55c8b-136">Node.js</span></span>](#inputnodejs)

<a name="inputcsharp"></a>

### <a name="input-sample-in-c"></a><span data-ttu-id="55c8b-137">Inkommande exemplet i C#</span><span class="sxs-lookup"><span data-stu-id="55c8b-137">Input sample in C#</span></span> #

```cs
#r "Newtonsoft.Json"    
using Newtonsoft.Json.Linq;

public static void Run(string myQueueItem, JObject record)
{
    if (record != null)
    {
        record["Text"] = "This has changed.";
    }    
}
```

<!--
<a name="inputfsharp"></a>
### Input sample in F# ## 

```fsharp
#r "Newtonsoft.Json"    
open Newtonsoft.Json.Linq
let Run(myQueueItem: string, record: JObject) =
  inputDocument?text <- "This has changed."
```
-->

<a name="inputnodejs"></a>

### <a name="input-sample-in-nodejs"></a><span data-ttu-id="55c8b-138">Inkommande exemplet i Node.js</span><span class="sxs-lookup"><span data-stu-id="55c8b-138">Input sample in Node.js</span></span>

```javascript
module.exports = function (context, myQueueItem) {    
    context.log(context.bindings.record);
    context.done();
};
```

<a name="output"></a>

## <a name="mobile-apps-output-binding"></a><span data-ttu-id="55c8b-139">Mobile Apps-utdatabindning</span><span class="sxs-lookup"><span data-stu-id="55c8b-139">Mobile Apps output binding</span></span>
<span data-ttu-id="55c8b-140">Använd hello Mobile Apps utdata bindning toowrite en ny post tooa Mobile Apps tabell slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="55c8b-140">Use hello Mobile Apps output binding toowrite a new record tooa Mobile Apps table endpoint.</span></span>  

<span data-ttu-id="55c8b-141">hello Mobile Apps utdata för en funktion använder hello följande JSON-objekt i hello `bindings` matris med function.json:</span><span class="sxs-lookup"><span data-stu-id="55c8b-141">hello Mobile Apps output for a function uses hello following JSON object in hello `bindings` array of function.json:</span></span>

```json
{
    "name": "<Name of output parameter in function signature>",
    "type": "mobileTable",
    "tableName": "<Name of your mobile app's data table>",
    "connection": "<Name of app setting that has your mobile app's URL - see below>",
    "apiKey": "<Name of app setting that has your mobile app's API key - see below>",
    "direction": "out"
}
```

<span data-ttu-id="55c8b-142">Observera följande hello:</span><span class="sxs-lookup"><span data-stu-id="55c8b-142">Note hello following:</span></span>

* <span data-ttu-id="55c8b-143">`connection`ska innehålla hello namnet på en appinställning i funktionen appen, som i sin tur innehåller hello URL för din mobila app.</span><span class="sxs-lookup"><span data-stu-id="55c8b-143">`connection` should contain hello name of an app setting in your function app, which in turn contains hello URL of your mobile app.</span></span> <span data-ttu-id="55c8b-144">hello funktionen använder den här URL: en tooconstruct hello krävs REST-åtgärder mot din mobila app.</span><span class="sxs-lookup"><span data-stu-id="55c8b-144">hello function uses this URL tooconstruct hello required REST operations against your mobile app.</span></span> <span data-ttu-id="55c8b-145">Du [skapa en appinställningen i appen funktionen]() som innehåller din mobila app-URL (som ser ut så `http://<appname>.azurewebsites.net`), ange hello namnet på hello appinställningen i hello `connection` egenskap i den inkommande bindningen.</span><span class="sxs-lookup"><span data-stu-id="55c8b-145">You [create an app setting in your function app]() that contains your mobile app's URL (which looks like `http://<appname>.azurewebsites.net`), then specify hello name of hello app setting in hello `connection` property in your input binding.</span></span> 
* <span data-ttu-id="55c8b-146">Du behöver toospecify `apiKey` om du [implementera en API-nyckel i mobilappsserverdelen Node.js](https://github.com/Azure/azure-mobile-apps-node/tree/master/samples/api-key), eller [implementera en API-nyckel i mobilappsserverdelen .NET](https://github.com/Azure/azure-mobile-apps-net-server/wiki/Implementing-Application-Key).</span><span class="sxs-lookup"><span data-stu-id="55c8b-146">You need toospecify `apiKey` if you [implement an API key in your Node.js mobile app backend](https://github.com/Azure/azure-mobile-apps-node/tree/master/samples/api-key), or [implement an API key in your .NET mobile app backend](https://github.com/Azure/azure-mobile-apps-net-server/wiki/Implementing-Application-Key).</span></span> <span data-ttu-id="55c8b-147">toodo kan du [skapa en appinställningen i appen funktionen]() som innehåller hello API-nyckel, lägga till hello `apiKey` egenskap i din indatabindning med hello namnet hello appinställningen.</span><span class="sxs-lookup"><span data-stu-id="55c8b-147">toodo this, you [create an app setting in your function app]() that contains hello API key, then add hello `apiKey` property in your input binding with hello name of hello app setting.</span></span> 
  
  > [!IMPORTANT]
  > <span data-ttu-id="55c8b-148">Den här API-nyckeln måste inte delas med din mobila app-klienter.</span><span class="sxs-lookup"><span data-stu-id="55c8b-148">This API key must not be shared with your mobile app clients.</span></span> <span data-ttu-id="55c8b-149">Det bör bara vara distribuerad på ett säkert sätt tooservice sida klienter, till exempel Azure Functions.</span><span class="sxs-lookup"><span data-stu-id="55c8b-149">It should only be distributed securely tooservice-side clients, like Azure Functions.</span></span> 
  > 
  > [!NOTE]
  > <span data-ttu-id="55c8b-150">Azure Functions lagrar dina anslutningsinformationen och API-nycklar som app-inställningar så att de inte kontrolleras i din källkontroll.</span><span class="sxs-lookup"><span data-stu-id="55c8b-150">Azure Functions stores your connection information and API keys as app settings so that they are not checked into your source control repository.</span></span> <span data-ttu-id="55c8b-151">Detta skyddar känslig information.</span><span class="sxs-lookup"><span data-stu-id="55c8b-151">This safeguards your sensitive information.</span></span>
  > 
  > 

<a name="outputusage"></a>

## <a name="output-usage"></a><span data-ttu-id="55c8b-152">Användning av utdata</span><span class="sxs-lookup"><span data-stu-id="55c8b-152">Output usage</span></span>
<span data-ttu-id="55c8b-153">Det här avsnittet visar hur toouse Mobile Apps utdata bindningen i din funktionskoden.</span><span class="sxs-lookup"><span data-stu-id="55c8b-153">This section shows you how toouse your Mobile Apps output binding in your function code.</span></span> 

<span data-ttu-id="55c8b-154">I C#-funktioner, använder en namngiven output-parameter av typen `out object` tooaccess hello utdata post.</span><span class="sxs-lookup"><span data-stu-id="55c8b-154">In C# functions, use a named output parameter of type `out object` tooaccess hello output record.</span></span> <span data-ttu-id="55c8b-155">I Node.js-funktion, använda `context.bindings.<name>` tooaccess hello utdata post.</span><span class="sxs-lookup"><span data-stu-id="55c8b-155">In Node.js functions, use `context.bindings.<name>` tooaccess hello output record.</span></span>

<a name="outputsample"></a>

## <a name="output-sample"></a><span data-ttu-id="55c8b-156">Exempel på utdata</span><span class="sxs-lookup"><span data-stu-id="55c8b-156">Output sample</span></span>
<span data-ttu-id="55c8b-157">Anta att du har följande function.json som definierar en kö-utlösare och en Mobile Apps utdata hello:</span><span class="sxs-lookup"><span data-stu-id="55c8b-157">Suppose you have hello following function.json, that defines a queue trigger and a Mobile Apps output:</span></span>

```json
{
"bindings": [
    {
    "name": "myQueueItem",
    "queueName": "myqueue-items",
    "connection":"",
    "type": "queueTrigger",
    "direction": "in"
    },
    {
    "name": "record",
    "type": "mobileTable",
    "tableName": "MyTable",
    "connection": "My_MobileApp_Url",
    "apiKey": "My_MobileApp_Key",
    "direction": "out"
    }
],
"disabled": false
}
```

<span data-ttu-id="55c8b-158">Se hello språkspecifika exempel som skapar en post i hello Mobile Apps tabell slutpunkten med hello innehållet hello kön meddelandet.</span><span class="sxs-lookup"><span data-stu-id="55c8b-158">See hello language-specific sample that creates a record in hello Mobile Apps table endpoint with hello content of hello queue message.</span></span>

* [<span data-ttu-id="55c8b-159">C#</span><span class="sxs-lookup"><span data-stu-id="55c8b-159">C#</span></span>](#outcsharp)
* [<span data-ttu-id="55c8b-160">Node.js</span><span class="sxs-lookup"><span data-stu-id="55c8b-160">Node.js</span></span>](#outnodejs)

<a name="outcsharp"></a>

### <a name="output-sample-in-c"></a><span data-ttu-id="55c8b-161">Utdata exemplet i C#</span><span class="sxs-lookup"><span data-stu-id="55c8b-161">Output sample in C#</span></span> #

```cs
public static void Run(string myQueueItem, out object record)
{
    record = new {
        Text = $"I'm running in a C# function! {myQueueItem}"
    };
}
```

<!--
<a name="outfsharp"></a>
### Output sample in F# ## 
```fsharp

```
-->
<a name="outnodejs"></a>

### <a name="output-sample-in-nodejs"></a><span data-ttu-id="55c8b-162">Utdata exemplet i Node.js</span><span class="sxs-lookup"><span data-stu-id="55c8b-162">Output sample in Node.js</span></span>

```javascript
module.exports = function (context, myQueueItem) {

    context.bindings.record = {
        text : "I'm running in a Node function! Data: '" + myQueueItem + "'"
    }   

    context.done();
};
```

## <a name="next-steps"></a><span data-ttu-id="55c8b-163">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="55c8b-163">Next steps</span></span>
[!INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)]

