---
title: "aaaAzure funktioner timer som utlösare | Microsoft Docs"
description: "Förstå hur toouse timer utlöser i Azure Functions."
services: functions
documentationcenter: na
author: christopheranderson
manager: erikre
editor: 
tags: 
keywords: "Azure functions, funktioner, händelsebearbetning, dynamiska beräkning serverlösa arkitektur"
ms.assetid: d2f013d1-f458-42ae-baf8-1810138118ac
ms.service: functions
ms.devlang: multiple
ms.topic: reference
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 02/27/2017
ms.author: glenga
ms.custom: 
ms.openlocfilehash: 17fca22372dbc55d4684c8c099cc97923a7d3cf3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-functions-timer-trigger"></a>Azure Functions timer som utlösare

[!INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

Den här artikeln förklarar hur tooconfigure och kod timer utlöser i Azure Functions. Azure Functions har en timer utlösaren bindning som låter dig köra din funktionskod baserat på ett definierat schema. 

hello timer som utlösare stöder flera instanser skalbar. En instans av en viss timerfunktion körs i alla instanser.

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

<a id="trigger"></a>

## <a name="timer-trigger"></a>Timerutlösare
hello timer utlösaren tooa använder funktionen hello följande JSON-objekt i hello `bindings` matris med function.json:

```json
{
    "schedule": "<CRON expression - see below>",
    "name": "<Name of trigger parameter in function signature>",
    "type": "timerTrigger",
    "direction": "in"
}
```

Hej värdet för `schedule` är en [CRON-uttryck](http://en.wikipedia.org/wiki/Cron#CRON_expression) som innehåller dessa sex fält: 

    {second} {minute} {hour} {day} {month} {day-of-week}
&nbsp;
>[!NOTE]   
>Många av hello cron-uttryck som du hittar online utelämna hello `{second}` fältet. Om du kopierar från en av dem, behöver du tooadjust för hello extra `{second}` fältet. Specifika exempel finns [schemalägga exempel](#examples) nedan.

hello standardtidszon används med hello CRON-uttryck är Coordinated Universal Time (UTC). toohave CRON-uttryck baserat på en annan tidszon, skapa en ny appinställning för funktionen appen med namnet `WEBSITE_TIME_ZONE`. Ange hello värdenamn toohello av hello önskad tidszon som visas i hello [Microsoft tidszon Index](https://msdn.microsoft.com/library/ms912391.aspx). 

Till exempel *Eastern, normaltid* är UTC-05:00. toohave din timer utlösa brand på 10:00 AM GMT varje dag, Använd hello följande CRON-uttryck som-konton för UTC-tid:

```json
"schedule": "0 0 15 * * *",
``` 

Alternativt kan du lägga till en ny appinställning för funktionen appen med namnet `WEBSITE_TIME_ZONE` och ange hello värdet för**Eastern, normaltid**.  Hello följande CRON-uttryck kan sedan användas för 10:00 AM GMT: 

```json
"schedule": "0 0 10 * * *",
``` 


<a name="examples"></a>

## <a name="schedule-examples"></a>Schema-exempel
Här följer några exempel på CRON-uttryck som du kan använda för hello `schedule` egenskapen. 

tootrigger en gång var femte minut:

```json
"schedule": "0 */5 * * * *"
```

tootrigger en gång vid hello upp i varje timme:

```json
"schedule": "0 0 * * * *",
```

tootrigger en gång varannan timme:

```json
"schedule": "0 0 */2 * * *",
```

tootrigger en gång i timmen från too5 9: 00 PM:

```json
"schedule": "0 0 9-17 * * *",
```

tootrigger på 9:30:00 varje dag:

```json
"schedule": "0 30 9 * * *",
```

tootrigger på 9:30:00 varje vardag:

```json
"schedule": "0 30 9 * * 1-5",
```

<a name="usage"></a>

## <a name="trigger-usage"></a>Utlösaren användning
När en timer Utlösarfunktion anropas hello [timer-objekt](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions/Extensions/Timers/TimerInfo.cs) skickas till hello-funktionen. hello är följande JSON ett exempel representation av hello timer-objekt. 

```json
{
    "Schedule":{
    },
    "ScheduleStatus": {
        "Last":"2016-10-04T10:15:00.012699+00:00",
        "Next":"2016-10-04T10:20:00+00:00"
    },
    "IsPastDue":false
}
```

<a name="sample"></a>

## <a name="trigger-sample"></a>Utlösaren exempel
Anta att du har följande timer som utlösare i hello hello `bindings` matris med function.json:

```json
{
    "schedule": "0 */5 * * * *",
    "name": "myTimer",
    "type": "timerTrigger",
    "direction": "in"
}
```

Se hello språkspecifika prov som läser hello timer objektet toosee om är försenat.

* [C#](#triggercsharp)
* [F#](#triggerfsharp)
* [Node.js](#triggernodejs)

<a name="triggercsharp"></a>

### <a name="trigger-sample-in-c"></a>Utlösaren exemplet i C# #
```csharp
public static void Run(TimerInfo myTimer, TraceWriter log)
{
    if(myTimer.IsPastDue)
    {
        log.Info("Timer is running late!");
    }
    log.Info($"C# Timer trigger function executed at: {DateTime.Now}" );  
}
```

<a name="triggerfsharp"></a>

### <a name="trigger-sample-in-f"></a>Utlösaren exemplet i F # #
```fsharp
let Run(myTimer: TimerInfo, log: TraceWriter ) =
    if (myTimer.IsPastDue) then
        log.Info("F# function is running late.")
    let now = DateTime.Now.ToLongTimeString()
    log.Info(sprintf "F# function executed at %s!" now)
```

<a name="triggernodejs"></a>

### <a name="trigger-sample-in-nodejs"></a>Utlösaren exemplet i Node.js
```JavaScript
module.exports = function (context, myTimer) {
    var timeStamp = new Date().toISOString();

    if(myTimer.isPastDue)
    {
        context.log('Node.js is running late!');
    }
    context.log('Node.js timer trigger function ran!', timeStamp);   

    context.done();
};
```

## <a name="next-steps"></a>Nästa steg
[!INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)]

