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
# <a name="azure-functions-timer-trigger"></a><span data-ttu-id="b5b3f-104">Azure Functions timer som utlösare</span><span class="sxs-lookup"><span data-stu-id="b5b3f-104">Azure Functions timer trigger</span></span>

[!INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

<span data-ttu-id="b5b3f-105">Den här artikeln förklarar hur tooconfigure och kod timer utlöser i Azure Functions.</span><span class="sxs-lookup"><span data-stu-id="b5b3f-105">This article explains how tooconfigure and code timer triggers in Azure Functions.</span></span> <span data-ttu-id="b5b3f-106">Azure Functions har en timer utlösaren bindning som låter dig köra din funktionskod baserat på ett definierat schema.</span><span class="sxs-lookup"><span data-stu-id="b5b3f-106">Azure Functions has a timer trigger binding that lets you run your function code based on a defined schedule.</span></span> 

<span data-ttu-id="b5b3f-107">hello timer som utlösare stöder flera instanser skalbar. En instans av en viss timerfunktion körs i alla instanser.</span><span class="sxs-lookup"><span data-stu-id="b5b3f-107">hello timer trigger supports multi-instance scale-out. A single instance of a particular timer function is run across all instances.</span></span>

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

<a id="trigger"></a>

## <a name="timer-trigger"></a><span data-ttu-id="b5b3f-108">Timerutlösare</span><span class="sxs-lookup"><span data-stu-id="b5b3f-108">Timer trigger</span></span>
<span data-ttu-id="b5b3f-109">hello timer utlösaren tooa använder funktionen hello följande JSON-objekt i hello `bindings` matris med function.json:</span><span class="sxs-lookup"><span data-stu-id="b5b3f-109">hello timer trigger tooa function uses hello following JSON object in hello `bindings` array of function.json:</span></span>

```json
{
    "schedule": "<CRON expression - see below>",
    "name": "<Name of trigger parameter in function signature>",
    "type": "timerTrigger",
    "direction": "in"
}
```

<span data-ttu-id="b5b3f-110">Hej värdet för `schedule` är en [CRON-uttryck](http://en.wikipedia.org/wiki/Cron#CRON_expression) som innehåller dessa sex fält:</span><span class="sxs-lookup"><span data-stu-id="b5b3f-110">hello value of `schedule` is a [CRON expression](http://en.wikipedia.org/wiki/Cron#CRON_expression) that includes these six fields:</span></span> 

    {second} {minute} {hour} {day} {month} {day-of-week}
&nbsp;
>[!NOTE]   
><span data-ttu-id="b5b3f-111">Många av hello cron-uttryck som du hittar online utelämna hello `{second}` fältet.</span><span class="sxs-lookup"><span data-stu-id="b5b3f-111">Many of hello cron expressions you find online omit hello `{second}` field.</span></span> <span data-ttu-id="b5b3f-112">Om du kopierar från en av dem, behöver du tooadjust för hello extra `{second}` fältet.</span><span class="sxs-lookup"><span data-stu-id="b5b3f-112">If you copy from one of them, you need tooadjust for hello extra `{second}` field.</span></span> <span data-ttu-id="b5b3f-113">Specifika exempel finns [schemalägga exempel](#examples) nedan.</span><span class="sxs-lookup"><span data-stu-id="b5b3f-113">For specific examples, see [Schedule examples](#examples) below.</span></span>

<span data-ttu-id="b5b3f-114">hello standardtidszon används med hello CRON-uttryck är Coordinated Universal Time (UTC).</span><span class="sxs-lookup"><span data-stu-id="b5b3f-114">hello default time zone used with hello CRON expressions is Coordinated Universal Time (UTC).</span></span> <span data-ttu-id="b5b3f-115">toohave CRON-uttryck baserat på en annan tidszon, skapa en ny appinställning för funktionen appen med namnet `WEBSITE_TIME_ZONE`.</span><span class="sxs-lookup"><span data-stu-id="b5b3f-115">toohave your CRON expression based on another time zone, create a new app setting for your function app named `WEBSITE_TIME_ZONE`.</span></span> <span data-ttu-id="b5b3f-116">Ange hello värdenamn toohello av hello önskad tidszon som visas i hello [Microsoft tidszon Index](https://msdn.microsoft.com/library/ms912391.aspx).</span><span class="sxs-lookup"><span data-stu-id="b5b3f-116">Set hello value toohello name of hello desired time zone as shown in hello [Microsoft Time Zone Index](https://msdn.microsoft.com/library/ms912391.aspx).</span></span> 

<span data-ttu-id="b5b3f-117">Till exempel *Eastern, normaltid* är UTC-05:00.</span><span class="sxs-lookup"><span data-stu-id="b5b3f-117">For example, *Eastern Standard Time* is UTC-05:00.</span></span> <span data-ttu-id="b5b3f-118">toohave din timer utlösa brand på 10:00 AM GMT varje dag, Använd hello följande CRON-uttryck som-konton för UTC-tid:</span><span class="sxs-lookup"><span data-stu-id="b5b3f-118">toohave your timer trigger fire at 10:00 AM EST every day, use hello following CRON expression that accounts for UTC time zone:</span></span>

```json
"schedule": "0 0 15 * * *",
``` 

<span data-ttu-id="b5b3f-119">Alternativt kan du lägga till en ny appinställning för funktionen appen med namnet `WEBSITE_TIME_ZONE` och ange hello värdet för**Eastern, normaltid**.</span><span class="sxs-lookup"><span data-stu-id="b5b3f-119">Alternatively, you could add a new app setting for your function app named `WEBSITE_TIME_ZONE` and set hello value too**Eastern Standard Time**.</span></span>  <span data-ttu-id="b5b3f-120">Hello följande CRON-uttryck kan sedan användas för 10:00 AM GMT:</span><span class="sxs-lookup"><span data-stu-id="b5b3f-120">Then hello following CRON expression could be used for 10:00 AM EST:</span></span> 

```json
"schedule": "0 0 10 * * *",
``` 


<a name="examples"></a>

## <a name="schedule-examples"></a><span data-ttu-id="b5b3f-121">Schema-exempel</span><span class="sxs-lookup"><span data-stu-id="b5b3f-121">Schedule examples</span></span>
<span data-ttu-id="b5b3f-122">Här följer några exempel på CRON-uttryck som du kan använda för hello `schedule` egenskapen.</span><span class="sxs-lookup"><span data-stu-id="b5b3f-122">Here are some samples of CRON expressions you can use for hello `schedule` property.</span></span> 

<span data-ttu-id="b5b3f-123">tootrigger en gång var femte minut:</span><span class="sxs-lookup"><span data-stu-id="b5b3f-123">tootrigger once every five minutes:</span></span>

```json
"schedule": "0 */5 * * * *"
```

<span data-ttu-id="b5b3f-124">tootrigger en gång vid hello upp i varje timme:</span><span class="sxs-lookup"><span data-stu-id="b5b3f-124">tootrigger once at hello top of every hour:</span></span>

```json
"schedule": "0 0 * * * *",
```

<span data-ttu-id="b5b3f-125">tootrigger en gång varannan timme:</span><span class="sxs-lookup"><span data-stu-id="b5b3f-125">tootrigger once every two hours:</span></span>

```json
"schedule": "0 0 */2 * * *",
```

<span data-ttu-id="b5b3f-126">tootrigger en gång i timmen från too5 9: 00 PM:</span><span class="sxs-lookup"><span data-stu-id="b5b3f-126">tootrigger once every hour from 9 AM too5 PM:</span></span>

```json
"schedule": "0 0 9-17 * * *",
```

<span data-ttu-id="b5b3f-127">tootrigger på 9:30:00 varje dag:</span><span class="sxs-lookup"><span data-stu-id="b5b3f-127">tootrigger At 9:30 AM every day:</span></span>

```json
"schedule": "0 30 9 * * *",
```

<span data-ttu-id="b5b3f-128">tootrigger på 9:30:00 varje vardag:</span><span class="sxs-lookup"><span data-stu-id="b5b3f-128">tootrigger At 9:30 AM every weekday:</span></span>

```json
"schedule": "0 30 9 * * 1-5",
```

<a name="usage"></a>

## <a name="trigger-usage"></a><span data-ttu-id="b5b3f-129">Utlösaren användning</span><span class="sxs-lookup"><span data-stu-id="b5b3f-129">Trigger usage</span></span>
<span data-ttu-id="b5b3f-130">När en timer Utlösarfunktion anropas hello [timer-objekt](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions/Extensions/Timers/TimerInfo.cs) skickas till hello-funktionen.</span><span class="sxs-lookup"><span data-stu-id="b5b3f-130">When a timer trigger function is invoked, hello [timer object](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions/Extensions/Timers/TimerInfo.cs) is passed into hello function.</span></span> <span data-ttu-id="b5b3f-131">hello är följande JSON ett exempel representation av hello timer-objekt.</span><span class="sxs-lookup"><span data-stu-id="b5b3f-131">hello following JSON is an example representation of hello timer object.</span></span> 

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

## <a name="trigger-sample"></a><span data-ttu-id="b5b3f-132">Utlösaren exempel</span><span class="sxs-lookup"><span data-stu-id="b5b3f-132">Trigger sample</span></span>
<span data-ttu-id="b5b3f-133">Anta att du har följande timer som utlösare i hello hello `bindings` matris med function.json:</span><span class="sxs-lookup"><span data-stu-id="b5b3f-133">Suppose you have hello following timer trigger in hello `bindings` array of function.json:</span></span>

```json
{
    "schedule": "0 */5 * * * *",
    "name": "myTimer",
    "type": "timerTrigger",
    "direction": "in"
}
```

<span data-ttu-id="b5b3f-134">Se hello språkspecifika prov som läser hello timer objektet toosee om är försenat.</span><span class="sxs-lookup"><span data-stu-id="b5b3f-134">See hello language-specific sample that reads hello timer object toosee whether it's running late.</span></span>

* [<span data-ttu-id="b5b3f-135">C#</span><span class="sxs-lookup"><span data-stu-id="b5b3f-135">C#</span></span>](#triggercsharp)
* [<span data-ttu-id="b5b3f-136">F#</span><span class="sxs-lookup"><span data-stu-id="b5b3f-136">F#</span></span>](#triggerfsharp)
* [<span data-ttu-id="b5b3f-137">Node.js</span><span class="sxs-lookup"><span data-stu-id="b5b3f-137">Node.js</span></span>](#triggernodejs)

<a name="triggercsharp"></a>

### <a name="trigger-sample-in-c"></a><span data-ttu-id="b5b3f-138">Utlösaren exemplet i C#</span><span class="sxs-lookup"><span data-stu-id="b5b3f-138">Trigger sample in C#</span></span> #
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

### <a name="trigger-sample-in-f"></a><span data-ttu-id="b5b3f-139">Utlösaren exemplet i F #</span><span class="sxs-lookup"><span data-stu-id="b5b3f-139">Trigger sample in F#</span></span> #
```fsharp
let Run(myTimer: TimerInfo, log: TraceWriter ) =
    if (myTimer.IsPastDue) then
        log.Info("F# function is running late.")
    let now = DateTime.Now.ToLongTimeString()
    log.Info(sprintf "F# function executed at %s!" now)
```

<a name="triggernodejs"></a>

### <a name="trigger-sample-in-nodejs"></a><span data-ttu-id="b5b3f-140">Utlösaren exemplet i Node.js</span><span class="sxs-lookup"><span data-stu-id="b5b3f-140">Trigger sample in Node.js</span></span>
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

## <a name="next-steps"></a><span data-ttu-id="b5b3f-141">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="b5b3f-141">Next steps</span></span>
[!INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)]

