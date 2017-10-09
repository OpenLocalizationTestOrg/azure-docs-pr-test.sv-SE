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
# <a name="azure-functions-mobile-apps-bindings"></a>Azure Functions Mobile Apps bindningar
[!INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

Den här artikeln förklarar hur tooconfigure och kod [Azure Mobile Apps](../app-service-mobile/app-service-mobile-value-prop.md) bindningar i Azure Functions. Azure Functions stöder indata och utdata bindningar för Mobile Apps.

hello Mobile Apps indata och utdata-bindningar kan du [läsa och skriva toodata tabeller](../app-service-mobile/app-service-mobile-node-backend-how-to-use-server-sdk.md#TableOperations) i din mobila app.

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

<a name="input"></a>

## <a name="mobile-apps-input-binding"></a>Mobile Apps indatabindning
hello Mobile Apps indatabindning läser en post från en mobil tabell slutpunkt och skickar det till din funktion. I C# och F # funktioner, alla ändringar som gjorts toohello post skickas automatiskt tillbaka toohello tabellen när hello funktionen avslutas korrekt.

hello Mobile Apps indata tooa funktion använder hello följande JSON-objekt i hello `bindings` matris med function.json:

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

Observera följande hello:

* `id`kan vara statisk eller det kan vara baserad på hello utlösare som anropar hello-funktionen. Om du använder till exempel en [kö utlösaren]() för din funktion sedan `"id": "{queueTrigger}"` använder hello strängvärdet för kön hälsningsmeddelande som hello poster ID tooretrieve.
* `connection`ska innehålla hello namnet på en appinställning i funktionen appen, som i sin tur innehåller hello URL för din mobila app. hello funktionen använder den här URL: en tooconstruct hello krävs REST-åtgärder mot din mobila app. Du [skapa en appinställningen i appen funktionen]() som innehåller din mobila app-URL (som ser ut så `http://<appname>.azurewebsites.net`), ange hello namnet på hello appinställningen i hello `connection` egenskap i den inkommande bindningen. 
* Du behöver toospecify `apiKey` om du [implementera en API-nyckel i mobilappsserverdelen Node.js](https://github.com/Azure/azure-mobile-apps-node/tree/master/samples/api-key), eller [implementera en API-nyckel i mobilappsserverdelen .NET](https://github.com/Azure/azure-mobile-apps-net-server/wiki/Implementing-Application-Key). toodo kan du [skapa en appinställningen i appen funktionen]() som innehåller hello API-nyckel, lägga till hello `apiKey` egenskap i din indatabindning med hello namnet hello appinställningen. 
  
  > [!IMPORTANT]
  > Den här API-nyckeln måste inte delas med din mobila app-klienter. Det bör bara vara distribuerad på ett säkert sätt tooservice sida klienter, till exempel Azure Functions. 
  > 
  > [!NOTE]
  > Azure Functions lagrar dina anslutningsinformationen och API-nycklar som app-inställningar så att de inte kontrolleras i din källkontroll. Detta skyddar känslig information.
  > 
  > 

<a name="inputusage"></a>

## <a name="input-usage"></a>Inkommande användning
Det här avsnittet visar hur toouse Mobile Apps inkommande bindningen i din funktionskoden. 

När hello post med hello angiven tabell och post-ID hittas, skickas detta till hello med namnet [JObject](http://www.newtonsoft.com/json/help/html/t_newtonsoft_json_linq_jobject.htm) parameter (eller i Node.js, den överförs till hello `context.bindings.<name>` objekt). När hello-post inte hittas hello-parametern är `null`. 

I C# och F # funktioner, alla ändringar du gör toohello indata post (Indataparametern) skickas automatiskt tillbaka toohello Mobile Apps tabellen när hello funktionen avslutas korrekt. I Node.js-funktion, använda `context.bindings.<name>` tooaccess hello Indataposten. Du kan inte ändra en post i Node.js.

<a name="inputsample"></a>

## <a name="input-sample"></a>Indata-exempel
Anta att du har följande function.json hello hämtas som en Mobilapp tabell post med hälsningsmeddelande kö utlösaren hello-id:

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

Se hello språkspecifika exempel som använder hello Indataposten från hello bindning. hello C# och F #-exempel också ändra hello post `text` egenskapen.

* [C#](#inputcsharp)
* [Node.js](#inputnodejs)

<a name="inputcsharp"></a>

### <a name="input-sample-in-c"></a>Inkommande exemplet i C# #

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

### <a name="input-sample-in-nodejs"></a>Inkommande exemplet i Node.js

```javascript
module.exports = function (context, myQueueItem) {    
    context.log(context.bindings.record);
    context.done();
};
```

<a name="output"></a>

## <a name="mobile-apps-output-binding"></a>Mobile Apps-utdatabindning
Använd hello Mobile Apps utdata bindning toowrite en ny post tooa Mobile Apps tabell slutpunkt.  

hello Mobile Apps utdata för en funktion använder hello följande JSON-objekt i hello `bindings` matris med function.json:

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

Observera följande hello:

* `connection`ska innehålla hello namnet på en appinställning i funktionen appen, som i sin tur innehåller hello URL för din mobila app. hello funktionen använder den här URL: en tooconstruct hello krävs REST-åtgärder mot din mobila app. Du [skapa en appinställningen i appen funktionen]() som innehåller din mobila app-URL (som ser ut så `http://<appname>.azurewebsites.net`), ange hello namnet på hello appinställningen i hello `connection` egenskap i den inkommande bindningen. 
* Du behöver toospecify `apiKey` om du [implementera en API-nyckel i mobilappsserverdelen Node.js](https://github.com/Azure/azure-mobile-apps-node/tree/master/samples/api-key), eller [implementera en API-nyckel i mobilappsserverdelen .NET](https://github.com/Azure/azure-mobile-apps-net-server/wiki/Implementing-Application-Key). toodo kan du [skapa en appinställningen i appen funktionen]() som innehåller hello API-nyckel, lägga till hello `apiKey` egenskap i din indatabindning med hello namnet hello appinställningen. 
  
  > [!IMPORTANT]
  > Den här API-nyckeln måste inte delas med din mobila app-klienter. Det bör bara vara distribuerad på ett säkert sätt tooservice sida klienter, till exempel Azure Functions. 
  > 
  > [!NOTE]
  > Azure Functions lagrar dina anslutningsinformationen och API-nycklar som app-inställningar så att de inte kontrolleras i din källkontroll. Detta skyddar känslig information.
  > 
  > 

<a name="outputusage"></a>

## <a name="output-usage"></a>Användning av utdata
Det här avsnittet visar hur toouse Mobile Apps utdata bindningen i din funktionskoden. 

I C#-funktioner, använder en namngiven output-parameter av typen `out object` tooaccess hello utdata post. I Node.js-funktion, använda `context.bindings.<name>` tooaccess hello utdata post.

<a name="outputsample"></a>

## <a name="output-sample"></a>Exempel på utdata
Anta att du har följande function.json som definierar en kö-utlösare och en Mobile Apps utdata hello:

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

Se hello språkspecifika exempel som skapar en post i hello Mobile Apps tabell slutpunkten med hello innehållet hello kön meddelandet.

* [C#](#outcsharp)
* [Node.js](#outnodejs)

<a name="outcsharp"></a>

### <a name="output-sample-in-c"></a>Utdata exemplet i C# #

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

### <a name="output-sample-in-nodejs"></a>Utdata exemplet i Node.js

```javascript
module.exports = function (context, myQueueItem) {

    context.bindings.record = {
        text : "I'm running in a Node function! Data: '" + myQueueItem + "'"
    }   

    context.done();
};
```

## <a name="next-steps"></a>Nästa steg
[!INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)]

