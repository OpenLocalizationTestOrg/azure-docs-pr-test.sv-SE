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
# <a name="send-sms-messages-from-azure-functions-using-hello-twilio-output-binding"></a>Skicka SMS-meddelanden från Azure Functions med hello Twilio-utdatabindning
[!INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

Den här artikeln förklarar hur tooconfigure och använda Twilio-bindningar med Azure Functions. 

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

Azure Functions stöder Twilio utdata bindningar tooenable funktioner toosend SMS-meddelanden meddelanden med några få rader med kod och en [Twilio](https://www.twilio.com/) konto. 

## <a name="functionjson-for-hello-twilio-output-binding"></a>Function.JSON för hello Twilio utdata bindning
Hej function.json filen innehåller hello följande egenskaper:

* `name`: Variabelnamn som används i Funktionskoden för hello Twilio SMS textmeddelande.
* `type`: måste anges för*”twilioSms”*.
* `accountSid`: Det här värdet måste anges toohello namnet på en Appinställning som innehåller dina Twilio-konto Sid.
* `authToken`: Det här värdet måste anges toohello namnet på en Appinställning som innehåller dina Twilio-token för autentisering.
* `to`: Det här värdet anges toohello telefonnummer som hello SMS-meddelanden skickas till.
* `from`: Det här värdet anges toohello telefonnummer som hello SMS-meddelanden skickas från.
* `direction`: måste anges för*”out”*.
* `body`: Det här värdet kan vara används toohard kod hello SMS textmeddelande om du inte behöver tooset den dynamiskt i hello code för din funktion. 

Exempel function.json:

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


## <a name="example-c-queue-trigger-with-twilio-output-binding"></a>Exempel C# kön utlösare med Twilio-utdatabindning
#### <a name="synchronous"></a>Synkron
Den här synkron kodexempel för en utlösare för Azure Storage-kö använder en out parametern toosend text meddelandet tooa kunden placeras en order.

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

#### <a name="asynchronous"></a>Asynkron
Den här asynkrona kodexempel för en utlösare för Azure Storage-kö skickar en text meddelandet tooa kund som placeras en order.

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

## <a name="example-nodejs-queue-trigger-with-twilio-output-binding"></a>Exempel Node.js kön utlösare med Twilio-utdatabindning
Det här exemplet med Node.js skickar en text meddelandet tooa kund som placeras en order.

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

## <a name="next-steps"></a>Nästa steg
[!INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)]

