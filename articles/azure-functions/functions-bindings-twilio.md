---
title: aaaAzure funktioner Twilio bindning | Microsoft Docs
description: "Förstå hur toouse Twilio-bindningar med Azure Functions."
services: functions
documentationcenter: na
author: wesmc7777
manager: erikre
editor: 
tags: 
keywords: "Azure functions, funktioner, händelsebearbetning, dynamiska beräkning serverlösa arkitektur"
ms.assetid: a60263aa-3de9-4e1b-a2bb-0b52e70d559b
ms.service: functions
ms.devlang: multiple
ms.topic: reference
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 10/20/2016
ms.author: wesmc
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 882853947850e7d6795ca5b2f3fb6b9a83ede182
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="send-sms-messages-from-azure-functions-using-hello-twilio-output-binding"></a><span data-ttu-id="c1b52-104">Skicka SMS-meddelanden från Azure Functions med hello Twilio-utdatabindning</span><span class="sxs-lookup"><span data-stu-id="c1b52-104">Send SMS messages from Azure Functions using hello Twilio output binding</span></span>
[!INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

<span data-ttu-id="c1b52-105">Den här artikeln förklarar hur tooconfigure och använda Twilio-bindningar med Azure Functions.</span><span class="sxs-lookup"><span data-stu-id="c1b52-105">This article explains how tooconfigure and use Twilio bindings with Azure Functions.</span></span> 

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

<span data-ttu-id="c1b52-106">Azure Functions stöder Twilio utdata bindningar tooenable funktioner toosend SMS-meddelanden meddelanden med några få rader med kod och en [Twilio](https://www.twilio.com/) konto.</span><span class="sxs-lookup"><span data-stu-id="c1b52-106">Azure Functions supports Twilio output bindings tooenable your functions toosend SMS text messages with a few lines of code and a [Twilio](https://www.twilio.com/) account.</span></span> 

## <a name="functionjson-for-hello-twilio-output-binding"></a><span data-ttu-id="c1b52-107">Function.JSON för hello Twilio utdata bindning</span><span class="sxs-lookup"><span data-stu-id="c1b52-107">function.json for hello Twilio output binding</span></span>
<span data-ttu-id="c1b52-108">Hej function.json filen innehåller hello följande egenskaper:</span><span class="sxs-lookup"><span data-stu-id="c1b52-108">hello function.json file provides hello following properties:</span></span>

* <span data-ttu-id="c1b52-109">`name`: Variabelnamn som används i Funktionskoden för hello Twilio SMS textmeddelande.</span><span class="sxs-lookup"><span data-stu-id="c1b52-109">`name` : Variable name used in function code for hello Twilio SMS text message.</span></span>
* <span data-ttu-id="c1b52-110">`type`: måste anges för*”twilioSms”*.</span><span class="sxs-lookup"><span data-stu-id="c1b52-110">`type` : must be set too*"twilioSms"*.</span></span>
* <span data-ttu-id="c1b52-111">`accountSid`: Det här värdet måste anges toohello namnet på en Appinställning som innehåller dina Twilio-konto Sid.</span><span class="sxs-lookup"><span data-stu-id="c1b52-111">`accountSid` : This value must be set toohello name of an App Setting that holds your Twilio Account Sid.</span></span>
* <span data-ttu-id="c1b52-112">`authToken`: Det här värdet måste anges toohello namnet på en Appinställning som innehåller dina Twilio-token för autentisering.</span><span class="sxs-lookup"><span data-stu-id="c1b52-112">`authToken` : This value must be set toohello name of an App Setting that holds your Twilio authentication token.</span></span>
* <span data-ttu-id="c1b52-113">`to`: Det här värdet anges toohello telefonnummer som hello SMS-meddelanden skickas till.</span><span class="sxs-lookup"><span data-stu-id="c1b52-113">`to` : This value is set toohello phone number that hello SMS text is sent to.</span></span>
* <span data-ttu-id="c1b52-114">`from`: Det här värdet anges toohello telefonnummer som hello SMS-meddelanden skickas från.</span><span class="sxs-lookup"><span data-stu-id="c1b52-114">`from` : This value is set toohello phone number that hello SMS text is sent from.</span></span>
* <span data-ttu-id="c1b52-115">`direction`: måste anges för*”out”*.</span><span class="sxs-lookup"><span data-stu-id="c1b52-115">`direction` : must be set too*"out"*.</span></span>
* <span data-ttu-id="c1b52-116">`body`: Det här värdet kan vara används toohard kod hello SMS textmeddelande om du inte behöver tooset den dynamiskt i hello code för din funktion.</span><span class="sxs-lookup"><span data-stu-id="c1b52-116">`body` : This value can be used toohard code hello SMS text message if you don't need tooset it dynamically in hello code for your function.</span></span> 

<span data-ttu-id="c1b52-117">Exempel function.json:</span><span class="sxs-lookup"><span data-stu-id="c1b52-117">Example function.json:</span></span>

```json
{
  "type": "twilioSms",
  "name": "message",
  "accountSid": "TwilioAccountSid",
  "authToken": "TwilioAuthToken",
  "to": "+1704XXXXXXX",
  "from": "+1425XXXXXXX",
  "direction": "out",
  "body": "Azure Functions Testing"
}
```


## <a name="example-c-queue-trigger-with-twilio-output-binding"></a><span data-ttu-id="c1b52-118">Exempel C# kön utlösare med Twilio-utdatabindning</span><span class="sxs-lookup"><span data-stu-id="c1b52-118">Example C# queue trigger with Twilio output binding</span></span>
#### <a name="synchronous"></a><span data-ttu-id="c1b52-119">Synkron</span><span class="sxs-lookup"><span data-stu-id="c1b52-119">Synchronous</span></span>
<span data-ttu-id="c1b52-120">Den här synkron kodexempel för en utlösare för Azure Storage-kö använder en out parametern toosend text meddelandet tooa kunden placeras en order.</span><span class="sxs-lookup"><span data-stu-id="c1b52-120">This synchronous example code for an Azure Storage queue trigger uses an out parameter toosend a text message tooa customer who placed an order.</span></span>

```cs
#r "Newtonsoft.Json"
#r "Twilio.Api"

using System;
using Newtonsoft.Json;
using Twilio;

public static void Run(string myQueueItem, out SMSMessage message,  TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {myQueueItem}");

    // In this example hello queue item is a JSON string representing an order that contains hello name of a 
    // customer and a mobile number toosend text updates to.
    dynamic order = JsonConvert.DeserializeObject(myQueueItem);
    string msg = "Hello " + order.name + ", thank you for your order.";

    // Even if you want toouse a hard coded message and number in hello binding, you must at least 
    // initialize hello SMSMessage variable.
    message = new SMSMessage();

    // A dynamic message can be set instead of hello body in hello output binding. In this example, we use 
    // hello order information toopersonalize a text message toohello mobile number provided for
    // order status updates.
    message.Body = msg;
    message.too= order.mobileNumber;
}
```

#### <a name="asynchronous"></a><span data-ttu-id="c1b52-121">Asynkron</span><span class="sxs-lookup"><span data-stu-id="c1b52-121">Asynchronous</span></span>
<span data-ttu-id="c1b52-122">Den här asynkrona kodexempel för en utlösare för Azure Storage-kö skickar en text meddelandet tooa kund som placeras en order.</span><span class="sxs-lookup"><span data-stu-id="c1b52-122">This asynchronous example code for an Azure Storage queue trigger sends a text message tooa customer who placed an order.</span></span>

```cs
#r "Newtonsoft.Json"
#r "Twilio.Api"

using System;
using Newtonsoft.Json;
using Twilio;

public static async Task Run(string myQueueItem, IAsyncCollector<SMSMessage> message,  TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {myQueueItem}");

    // In this example hello queue item is a JSON string representing an order that contains hello name of a 
    // customer and a mobile number toosend text updates to.
    dynamic order = JsonConvert.DeserializeObject(myQueueItem);
    string msg = "Hello " + order.name + ", thank you for your order.";

    // Even if you want toouse a hard coded message and number in hello binding, you must at least 
    // initialize hello SMSMessage variable.
    SMSMessage smsText = new SMSMessage();

    // A dynamic message can be set instead of hello body in hello output binding. In this example, we use 
    // hello order information toopersonalize a text message toohello mobile number provided for
    // order status updates.
    smsText.Body = msg;
    smsText.too= order.mobileNumber;

    await message.AddAsync(smsText);
}
```

## <a name="example-nodejs-queue-trigger-with-twilio-output-binding"></a><span data-ttu-id="c1b52-123">Exempel Node.js kön utlösare med Twilio-utdatabindning</span><span class="sxs-lookup"><span data-stu-id="c1b52-123">Example Node.js queue trigger with Twilio output binding</span></span>
<span data-ttu-id="c1b52-124">Det här exemplet med Node.js skickar en text meddelandet tooa kund som placeras en order.</span><span class="sxs-lookup"><span data-stu-id="c1b52-124">This Node.js example sends a text message tooa customer who placed an order.</span></span>

```javascript
module.exports = function (context, myQueueItem) {
    context.log('Node.js queue trigger function processed work item', myQueueItem);

    // In this example hello queue item is a JSON string representing an order that contains hello name of a 
    // customer and a mobile number toosend text updates to.
    var msg = "Hello " + myQueueItem.name + ", thank you for your order.";

    // Even if you want toouse a hard coded message and number in hello binding, you must at least 
    // initialize hello message binding.
    context.bindings.message = {};

    // A dynamic message can be set instead of hello body in hello output binding. In this example, we use 
    // hello order information toopersonalize a text message toohello mobile number provided for
    // order status updates.
    context.bindings.message = {
        body : msg,
        too: myQueueItem.mobileNumber
    };

    context.done();
};
```

## <a name="next-steps"></a><span data-ttu-id="c1b52-125">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="c1b52-125">Next steps</span></span>
[!INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)]

