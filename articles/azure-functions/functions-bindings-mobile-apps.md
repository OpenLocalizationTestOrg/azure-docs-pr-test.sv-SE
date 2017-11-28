---
title: Azure Functions Mobile Apps bindningar | Microsoft Docs
description: "Förstå hur du använder Azure Mobile Apps bindningar i Azure Functions."
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
ms.openlocfilehash: c5e1c02984f9773b263c0bee7685c7d5ff62e658
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="azure-functions-mobile-apps-bindings"></a><span data-ttu-id="950f4-104">Azure Functions Mobile Apps bindningar</span><span class="sxs-lookup"><span data-stu-id="950f4-104">Azure Functions Mobile Apps bindings</span></span>
[!INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

<span data-ttu-id="950f4-105">Den här artikeln förklarar hur du konfigurerar och code [Azure Mobile Apps](../app-service-mobile/app-service-mobile-value-prop.md) bindningar i Azure Functions.</span><span class="sxs-lookup"><span data-stu-id="950f4-105">This article explains how to configure and code [Azure Mobile Apps](../app-service-mobile/app-service-mobile-value-prop.md) bindings in Azure Functions.</span></span> <span data-ttu-id="950f4-106">Azure Functions stöder indata och utdata bindningar för Mobile Apps.</span><span class="sxs-lookup"><span data-stu-id="950f4-106">Azure Functions supports input and output bindings for Mobile Apps.</span></span>

<span data-ttu-id="950f4-107">Mobile Apps indata och utdata-bindningar kan du [läsa från och skriva till datatabeller](../app-service-mobile/app-service-mobile-node-backend-how-to-use-server-sdk.md#TableOperations) i din mobila app.</span><span class="sxs-lookup"><span data-stu-id="950f4-107">The Mobile Apps input and output bindings let you [read from and write to data tables](../app-service-mobile/app-service-mobile-node-backend-how-to-use-server-sdk.md#TableOperations) in your mobile app.</span></span>

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

<a name="input"></a>

## <a name="mobile-apps-input-binding"></a><span data-ttu-id="950f4-108">Mobile Apps indatabindning</span><span class="sxs-lookup"><span data-stu-id="950f4-108">Mobile Apps input binding</span></span>
<span data-ttu-id="950f4-109">Mobile Apps-indatabindning läser en post från en mobil tabell slutpunkt och skickar det till din funktion.</span><span class="sxs-lookup"><span data-stu-id="950f4-109">The Mobile Apps input binding loads a record from a mobile table endpoint and passes it into your function.</span></span> <span data-ttu-id="950f4-110">I C# och F # funktioner skickas ändringar som görs till posten automatiskt tillbaka till tabellen när funktionen avslutas korrekt.</span><span class="sxs-lookup"><span data-stu-id="950f4-110">In a C# and F# functions, any changes made to the record are automatically sent back to the table when the function exits successfully.</span></span>

<span data-ttu-id="950f4-111">Mobile Apps indata för en funktion använder följande JSON-objekt i den `bindings` matris med function.json:</span><span class="sxs-lookup"><span data-stu-id="950f4-111">The Mobile Apps input to a function uses the following JSON object in the `bindings` array of function.json:</span></span>

```json
{
    "name": "<Name of input parameter in function signature>",
    "type": "mobileTable",
    "tableName": "<Name of your mobile app's data table>",
    "id" : "<Id of the record to retrieve - see below>",
    "connection": "<Name of app setting that has your mobile app's URL - see below>",
    "apiKey": "<Name of app setting that has your mobile app's API key - see below>",
    "direction": "in"
}
```

<span data-ttu-id="950f4-112">Observera följande:</span><span class="sxs-lookup"><span data-stu-id="950f4-112">Note the following:</span></span>

* <span data-ttu-id="950f4-113">`id`kan vara statisk eller det kan vara baserad på utlösaren som anropar funktionen.</span><span class="sxs-lookup"><span data-stu-id="950f4-113">`id` can be static, or it can be based on the trigger that invokes the function.</span></span> <span data-ttu-id="950f4-114">Om du använder till exempel en [kö utlösaren]() för din funktion sedan `"id": "{queueTrigger}"` använder strängvärdet för kön meddelandet som post-ID för att hämta.</span><span class="sxs-lookup"><span data-stu-id="950f4-114">For example, if you use a [queue trigger]() for your function, then `"id": "{queueTrigger}"` uses the string value of the queue message as the record ID to retrieve.</span></span>
* <span data-ttu-id="950f4-115">`connection`ska innehålla namnet på en appinställning i funktionen appen, som i sin tur innehåller URL-Adressen till din mobila app.</span><span class="sxs-lookup"><span data-stu-id="950f4-115">`connection` should contain the name of an app setting in your function app, which in turn contains the URL of your mobile app.</span></span> <span data-ttu-id="950f4-116">URL: en använder funktionen för att skapa nödvändiga REST-åtgärder mot din mobila app.</span><span class="sxs-lookup"><span data-stu-id="950f4-116">The function uses this URL to construct the required REST operations against your mobile app.</span></span> <span data-ttu-id="950f4-117">Du [skapa en appinställningen i appen funktionen]() som innehåller din mobila app-URL (som ser ut så `http://<appname>.azurewebsites.net`), ange namnet på appinställningen i den `connection` egenskap i den inkommande bindningen.</span><span class="sxs-lookup"><span data-stu-id="950f4-117">You [create an app setting in your function app]() that contains your mobile app's URL (which looks like `http://<appname>.azurewebsites.net`), then specify the name of the app setting in the `connection` property in your input binding.</span></span> 
* <span data-ttu-id="950f4-118">Du måste ange `apiKey` om du [implementera en API-nyckel i mobilappsserverdelen Node.js](https://github.com/Azure/azure-mobile-apps-node/tree/master/samples/api-key), eller [implementera en API-nyckel i mobilappsserverdelen .NET](https://github.com/Azure/azure-mobile-apps-net-server/wiki/Implementing-Application-Key).</span><span class="sxs-lookup"><span data-stu-id="950f4-118">You need to specify `apiKey` if you [implement an API key in your Node.js mobile app backend](https://github.com/Azure/azure-mobile-apps-node/tree/master/samples/api-key), or [implement an API key in your .NET mobile app backend](https://github.com/Azure/azure-mobile-apps-net-server/wiki/Implementing-Application-Key).</span></span> <span data-ttu-id="950f4-119">Gör du [skapa en appinställningen i appen funktionen]() som innehåller API-nyckeln, Lägg sedan till den `apiKey` egenskap i den inkommande bindningen med namnet för appinställningen.</span><span class="sxs-lookup"><span data-stu-id="950f4-119">To do this, you [create an app setting in your function app]() that contains the API key, then add the `apiKey` property in your input binding with the name of the app setting.</span></span> 
  
  > [!IMPORTANT]
  > <span data-ttu-id="950f4-120">Den här API-nyckeln måste inte delas med din mobila app-klienter.</span><span class="sxs-lookup"><span data-stu-id="950f4-120">This API key must not be shared with your mobile app clients.</span></span> <span data-ttu-id="950f4-121">Det bör bara distribueras på ett säkert sätt till tjänsten på klientsidan klienter, till exempel Azure Functions.</span><span class="sxs-lookup"><span data-stu-id="950f4-121">It should only be distributed securely to service-side clients, like Azure Functions.</span></span> 
  > 
  > [!NOTE]
  > <span data-ttu-id="950f4-122">Azure Functions lagrar dina anslutningsinformationen och API-nycklar som app-inställningar så att de inte kontrolleras i din källkontroll.</span><span class="sxs-lookup"><span data-stu-id="950f4-122">Azure Functions stores your connection information and API keys as app settings so that they are not checked into your source control repository.</span></span> <span data-ttu-id="950f4-123">Detta skyddar känslig information.</span><span class="sxs-lookup"><span data-stu-id="950f4-123">This safeguards your sensitive information.</span></span>
  > 
  > 

<a name="inputusage"></a>

## <a name="input-usage"></a><span data-ttu-id="950f4-124">Inkommande användning</span><span class="sxs-lookup"><span data-stu-id="950f4-124">Input usage</span></span>
<span data-ttu-id="950f4-125">Det här avsnittet visar hur du använder Mobile Apps-indatabindning i funktionskoden.</span><span class="sxs-lookup"><span data-stu-id="950f4-125">This section shows you how to use your Mobile Apps input binding in your function code.</span></span> 

<span data-ttu-id="950f4-126">När posten med den angivna tabellen och post-ID hittas skickas till den namngivna [JObject](http://www.newtonsoft.com/json/help/html/t_newtonsoft_json_linq_jobject.htm) parameter (eller i Node.js, den skickas till den `context.bindings.<name>` objekt).</span><span class="sxs-lookup"><span data-stu-id="950f4-126">When the record with the specified table and record ID is found, it is passed into the named [JObject](http://www.newtonsoft.com/json/help/html/t_newtonsoft_json_linq_jobject.htm) parameter (or, in Node.js, it is passed into the `context.bindings.<name>` object).</span></span> <span data-ttu-id="950f4-127">Om posten inte hittas, parametern är `null`.</span><span class="sxs-lookup"><span data-stu-id="950f4-127">When the record is not found, the parameter is `null`.</span></span> 

<span data-ttu-id="950f4-128">I C# och F # funktioner, alla ändringar du gör inkommande post (Indataparametern) skickas automatiskt tillbaka till tabellen Mobile Apps när funktionen avslutas korrekt.</span><span class="sxs-lookup"><span data-stu-id="950f4-128">In C# and F# functions, any changes you make to the input record (input parameter) is automatically sent back to the Mobile Apps table when the function exits successfully.</span></span> <span data-ttu-id="950f4-129">I Node.js-funktion, använda `context.bindings.<name>` att komma åt Indataposten.</span><span class="sxs-lookup"><span data-stu-id="950f4-129">In Node.js functions, use `context.bindings.<name>` to access the input record.</span></span> <span data-ttu-id="950f4-130">Du kan inte ändra en post i Node.js.</span><span class="sxs-lookup"><span data-stu-id="950f4-130">You cannot modify a record in Node.js.</span></span>

<a name="inputsample"></a>

## <a name="input-sample"></a><span data-ttu-id="950f4-131">Indata-exempel</span><span class="sxs-lookup"><span data-stu-id="950f4-131">Input sample</span></span>
<span data-ttu-id="950f4-132">Anta att du har följande function.json, som hämtar Mobilapp tabell post med ID: t för utlösaren kömeddelande:</span><span class="sxs-lookup"><span data-stu-id="950f4-132">Suppose you have the following function.json, that retrieves a Mobile App table record with the id of the queue trigger message:</span></span>

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

<span data-ttu-id="950f4-133">Finns det språkspecifika exempel som använder Indataposten från bindningen.</span><span class="sxs-lookup"><span data-stu-id="950f4-133">See the language-specific sample that uses the input record from the binding.</span></span> <span data-ttu-id="950f4-134">C# och F #-exempel också ändra postens `text` egenskapen.</span><span class="sxs-lookup"><span data-stu-id="950f4-134">The C# and F# samples also modify the record's `text` property.</span></span>

* [<span data-ttu-id="950f4-135">C#</span><span class="sxs-lookup"><span data-stu-id="950f4-135">C#</span></span>](#inputcsharp)
* [<span data-ttu-id="950f4-136">Node.js</span><span class="sxs-lookup"><span data-stu-id="950f4-136">Node.js</span></span>](#inputnodejs)

<a name="inputcsharp"></a>

### <a name="input-sample-in-c"></a><span data-ttu-id="950f4-137">Inkommande exemplet i C#</span><span class="sxs-lookup"><span data-stu-id="950f4-137">Input sample in C#</span></span> #

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

### <a name="input-sample-in-nodejs"></a><span data-ttu-id="950f4-138">Inkommande exemplet i Node.js</span><span class="sxs-lookup"><span data-stu-id="950f4-138">Input sample in Node.js</span></span>

```javascript
module.exports = function (context, myQueueItem) {    
    context.log(context.bindings.record);
    context.done();
};
```

<a name="output"></a>

## <a name="mobile-apps-output-binding"></a><span data-ttu-id="950f4-139">Mobile Apps-utdatabindning</span><span class="sxs-lookup"><span data-stu-id="950f4-139">Mobile Apps output binding</span></span>
<span data-ttu-id="950f4-140">Använda Mobile Apps utdata bindning för att skriva en ny post till en slutpunkt för Mobile Apps-tabellen.</span><span class="sxs-lookup"><span data-stu-id="950f4-140">Use the Mobile Apps output binding to write a new record to a Mobile Apps table endpoint.</span></span>  

<span data-ttu-id="950f4-141">Mobilappar som utdata för en funktion använder följande JSON-objekt i den `bindings` matris med function.json:</span><span class="sxs-lookup"><span data-stu-id="950f4-141">The Mobile Apps output for a function uses the following JSON object in the `bindings` array of function.json:</span></span>

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

<span data-ttu-id="950f4-142">Observera följande:</span><span class="sxs-lookup"><span data-stu-id="950f4-142">Note the following:</span></span>

* <span data-ttu-id="950f4-143">`connection`ska innehålla namnet på en appinställning i funktionen appen, som i sin tur innehåller URL-Adressen till din mobila app.</span><span class="sxs-lookup"><span data-stu-id="950f4-143">`connection` should contain the name of an app setting in your function app, which in turn contains the URL of your mobile app.</span></span> <span data-ttu-id="950f4-144">URL: en använder funktionen för att skapa nödvändiga REST-åtgärder mot din mobila app.</span><span class="sxs-lookup"><span data-stu-id="950f4-144">The function uses this URL to construct the required REST operations against your mobile app.</span></span> <span data-ttu-id="950f4-145">Du [skapa en appinställningen i appen funktionen]() som innehåller din mobila app-URL (som ser ut så `http://<appname>.azurewebsites.net`), ange namnet på appinställningen i den `connection` egenskap i den inkommande bindningen.</span><span class="sxs-lookup"><span data-stu-id="950f4-145">You [create an app setting in your function app]() that contains your mobile app's URL (which looks like `http://<appname>.azurewebsites.net`), then specify the name of the app setting in the `connection` property in your input binding.</span></span> 
* <span data-ttu-id="950f4-146">Du måste ange `apiKey` om du [implementera en API-nyckel i mobilappsserverdelen Node.js](https://github.com/Azure/azure-mobile-apps-node/tree/master/samples/api-key), eller [implementera en API-nyckel i mobilappsserverdelen .NET](https://github.com/Azure/azure-mobile-apps-net-server/wiki/Implementing-Application-Key).</span><span class="sxs-lookup"><span data-stu-id="950f4-146">You need to specify `apiKey` if you [implement an API key in your Node.js mobile app backend](https://github.com/Azure/azure-mobile-apps-node/tree/master/samples/api-key), or [implement an API key in your .NET mobile app backend](https://github.com/Azure/azure-mobile-apps-net-server/wiki/Implementing-Application-Key).</span></span> <span data-ttu-id="950f4-147">Gör du [skapa en appinställningen i appen funktionen]() som innehåller API-nyckeln, Lägg sedan till den `apiKey` egenskap i den inkommande bindningen med namnet för appinställningen.</span><span class="sxs-lookup"><span data-stu-id="950f4-147">To do this, you [create an app setting in your function app]() that contains the API key, then add the `apiKey` property in your input binding with the name of the app setting.</span></span> 
  
  > [!IMPORTANT]
  > <span data-ttu-id="950f4-148">Den här API-nyckeln måste inte delas med din mobila app-klienter.</span><span class="sxs-lookup"><span data-stu-id="950f4-148">This API key must not be shared with your mobile app clients.</span></span> <span data-ttu-id="950f4-149">Det bör bara distribueras på ett säkert sätt till tjänsten på klientsidan klienter, till exempel Azure Functions.</span><span class="sxs-lookup"><span data-stu-id="950f4-149">It should only be distributed securely to service-side clients, like Azure Functions.</span></span> 
  > 
  > [!NOTE]
  > <span data-ttu-id="950f4-150">Azure Functions lagrar dina anslutningsinformationen och API-nycklar som app-inställningar så att de inte kontrolleras i din källkontroll.</span><span class="sxs-lookup"><span data-stu-id="950f4-150">Azure Functions stores your connection information and API keys as app settings so that they are not checked into your source control repository.</span></span> <span data-ttu-id="950f4-151">Detta skyddar känslig information.</span><span class="sxs-lookup"><span data-stu-id="950f4-151">This safeguards your sensitive information.</span></span>
  > 
  > 

<a name="outputusage"></a>

## <a name="output-usage"></a><span data-ttu-id="950f4-152">Användning av utdata</span><span class="sxs-lookup"><span data-stu-id="950f4-152">Output usage</span></span>
<span data-ttu-id="950f4-153">Det här avsnittet visar hur du använder din Mobile Apps utdata bindningen i din funktionskoden.</span><span class="sxs-lookup"><span data-stu-id="950f4-153">This section shows you how to use your Mobile Apps output binding in your function code.</span></span> 

<span data-ttu-id="950f4-154">I C#-funktioner, använder en namngiven output-parameter av typen `out object` åtkomst till utdata-posten.</span><span class="sxs-lookup"><span data-stu-id="950f4-154">In C# functions, use a named output parameter of type `out object` to access the output record.</span></span> <span data-ttu-id="950f4-155">I Node.js-funktion, använda `context.bindings.<name>` till utdata-posten.</span><span class="sxs-lookup"><span data-stu-id="950f4-155">In Node.js functions, use `context.bindings.<name>` to access the output record.</span></span>

<a name="outputsample"></a>

## <a name="output-sample"></a><span data-ttu-id="950f4-156">Exempel på utdata</span><span class="sxs-lookup"><span data-stu-id="950f4-156">Output sample</span></span>
<span data-ttu-id="950f4-157">Anta att du har följande function.json, som definierar en kö-utlösare och en Mobile Apps-utdata:</span><span class="sxs-lookup"><span data-stu-id="950f4-157">Suppose you have the following function.json, that defines a queue trigger and a Mobile Apps output:</span></span>

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

<span data-ttu-id="950f4-158">Se exemplet språkspecifika som skapar en post i tabellen Mobile Apps slutpunkten med innehållet i kön meddelandet.</span><span class="sxs-lookup"><span data-stu-id="950f4-158">See the language-specific sample that creates a record in the Mobile Apps table endpoint with the content of the queue message.</span></span>

* [<span data-ttu-id="950f4-159">C#</span><span class="sxs-lookup"><span data-stu-id="950f4-159">C#</span></span>](#outcsharp)
* [<span data-ttu-id="950f4-160">Node.js</span><span class="sxs-lookup"><span data-stu-id="950f4-160">Node.js</span></span>](#outnodejs)

<a name="outcsharp"></a>

### <a name="output-sample-in-c"></a><span data-ttu-id="950f4-161">Utdata exemplet i C#</span><span class="sxs-lookup"><span data-stu-id="950f4-161">Output sample in C#</span></span> #

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

### <a name="output-sample-in-nodejs"></a><span data-ttu-id="950f4-162">Utdata exemplet i Node.js</span><span class="sxs-lookup"><span data-stu-id="950f4-162">Output sample in Node.js</span></span>

```javascript
module.exports = function (context, myQueueItem) {

    context.bindings.record = {
        text : "I'm running in a Node function! Data: '" + myQueueItem + "'"
    }   

    context.done();
};
```

## <a name="next-steps"></a><span data-ttu-id="950f4-163">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="950f4-163">Next steps</span></span>
[!INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)]

