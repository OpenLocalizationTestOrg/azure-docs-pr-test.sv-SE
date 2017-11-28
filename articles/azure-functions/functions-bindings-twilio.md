---
title: Azure Functions Twilio-bindning | Microsoft Docs
description: "Förstå hur du använder Twilio-bindningar med Azure Functions."
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
ms.openlocfilehash: 870e47ec7f8ce41ee4acadc7b8ed59298958acbe
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="send-sms-messages-from-azure-functions-using-the-twilio-output-binding"></a><span data-ttu-id="8329b-104">Skicka SMS meddelanden från Azure-funktioner med hjälp av Twilio-utdatabindning</span><span class="sxs-lookup"><span data-stu-id="8329b-104">Send SMS messages from Azure Functions using the Twilio output binding</span></span>
[!INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

<span data-ttu-id="8329b-105">Den här artikeln beskriver hur du konfigurerar och använder Twilio-bindningar med Azure Functions.</span><span class="sxs-lookup"><span data-stu-id="8329b-105">This article explains how to configure and use Twilio bindings with Azure Functions.</span></span> 

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

<span data-ttu-id="8329b-106">Azure Functions stöder Twilio utdata bindningar om du vill aktivera dina funktioner att skicka meddelanden för SMS-text med några få rader med kod och en [Twilio](https://www.twilio.com/) konto.</span><span class="sxs-lookup"><span data-stu-id="8329b-106">Azure Functions supports Twilio output bindings to enable your functions to send SMS text messages with a few lines of code and a [Twilio](https://www.twilio.com/) account.</span></span> 

## <a name="functionjson-for-the-twilio-output-binding"></a><span data-ttu-id="8329b-107">Function.JSON för Twilio-utdatabindning</span><span class="sxs-lookup"><span data-stu-id="8329b-107">function.json for the Twilio output binding</span></span>
<span data-ttu-id="8329b-108">Filen function.json innehåller följande egenskaper:</span><span class="sxs-lookup"><span data-stu-id="8329b-108">The function.json file provides the following properties:</span></span>

* <span data-ttu-id="8329b-109">`name`: Variabelnamn som används i Funktionskoden för textmeddelande Twilio SMS.</span><span class="sxs-lookup"><span data-stu-id="8329b-109">`name` : Variable name used in function code for the Twilio SMS text message.</span></span>
* <span data-ttu-id="8329b-110">`type`: måste anges till *”twilioSms”*.</span><span class="sxs-lookup"><span data-stu-id="8329b-110">`type` : must be set to *"twilioSms"*.</span></span>
* <span data-ttu-id="8329b-111">`accountSid`: Det här värdet måste anges till namnet på en Appinställning som innehåller dina Twilio-konto Sid.</span><span class="sxs-lookup"><span data-stu-id="8329b-111">`accountSid` : This value must be set to the name of an App Setting that holds your Twilio Account Sid.</span></span>
* <span data-ttu-id="8329b-112">`authToken`: Det här värdet måste anges till namnet på en Appinställning som innehåller dina Twilio-token för autentisering.</span><span class="sxs-lookup"><span data-stu-id="8329b-112">`authToken` : This value must be set to the name of an App Setting that holds your Twilio authentication token.</span></span>
* <span data-ttu-id="8329b-113">`to`: Det här värdet anges till det telefonnummer som SMS-meddelanden skickas till.</span><span class="sxs-lookup"><span data-stu-id="8329b-113">`to` : This value is set to the phone number that the SMS text is sent to.</span></span>
* <span data-ttu-id="8329b-114">`from`: Det här värdet anges till det telefonnummer som SMS-meddelanden skickas från.</span><span class="sxs-lookup"><span data-stu-id="8329b-114">`from` : This value is set to the phone number that the SMS text is sent from.</span></span>
* <span data-ttu-id="8329b-115">`direction`: måste anges till *”out”*.</span><span class="sxs-lookup"><span data-stu-id="8329b-115">`direction` : must be set to *"out"*.</span></span>
* <span data-ttu-id="8329b-116">`body`: Det här värdet kan användas för att hård code SMS textmeddelande om du inte behöver ange den dynamiskt i koden för din funktion.</span><span class="sxs-lookup"><span data-stu-id="8329b-116">`body` : This value can be used to hard code the SMS text message if you don't need to set it dynamically in the code for your function.</span></span> 

<span data-ttu-id="8329b-117">Exempel function.json:</span><span class="sxs-lookup"><span data-stu-id="8329b-117">Example function.json:</span></span>

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


## <a name="example-c-queue-trigger-with-twilio-output-binding"></a><span data-ttu-id="8329b-118">Exempel C# kön utlösare med Twilio-utdatabindning</span><span class="sxs-lookup"><span data-stu-id="8329b-118">Example C# queue trigger with Twilio output binding</span></span>
#### <a name="synchronous"></a><span data-ttu-id="8329b-119">Synkron</span><span class="sxs-lookup"><span data-stu-id="8329b-119">Synchronous</span></span>
<span data-ttu-id="8329b-120">Denna synkron kodexempel för en utlösare för Azure Storage-kö använder en out-parameter för att skicka ett SMS till en kund som placeras en order.</span><span class="sxs-lookup"><span data-stu-id="8329b-120">This synchronous example code for an Azure Storage queue trigger uses an out parameter to send a text message to a customer who placed an order.</span></span>

```cs
#r "Newtonsoft.Json"
#r "Twilio.Api"

using System;
using Newtonsoft.Json;
using Twilio;

public static void Run(string myQueueItem, out SMSMessage message,  TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {myQueueItem}");

    // In this example the queue item is a JSON string representing an order that contains the name of a 
    // customer and a mobile number to send text updates to.
    dynamic order = JsonConvert.DeserializeObject(myQueueItem);
    string msg = "Hello " + order.name + ", thank you for your order.";

    // Even if you want to use a hard coded message and number in the binding, you must at least 
    // initialize the SMSMessage variable.
    message = new SMSMessage();

    // A dynamic message can be set instead of the body in the output binding. In this example, we use 
    // the order information to personalize a text message to the mobile number provided for
    // order status updates.
    message.Body = msg;
    message.To = order.mobileNumber;
}
```

#### <a name="asynchronous"></a><span data-ttu-id="8329b-121">Asynkron</span><span class="sxs-lookup"><span data-stu-id="8329b-121">Asynchronous</span></span>
<span data-ttu-id="8329b-122">Den här asynkrona kodexempel för en utlösare för Azure Storage-kö skickar ett SMS till en kund som placeras en order.</span><span class="sxs-lookup"><span data-stu-id="8329b-122">This asynchronous example code for an Azure Storage queue trigger sends a text message to a customer who placed an order.</span></span>

```cs
#r "Newtonsoft.Json"
#r "Twilio.Api"

using System;
using Newtonsoft.Json;
using Twilio;

public static async Task Run(string myQueueItem, IAsyncCollector<SMSMessage> message,  TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {myQueueItem}");

    // In this example the queue item is a JSON string representing an order that contains the name of a 
    // customer and a mobile number to send text updates to.
    dynamic order = JsonConvert.DeserializeObject(myQueueItem);
    string msg = "Hello " + order.name + ", thank you for your order.";

    // Even if you want to use a hard coded message and number in the binding, you must at least 
    // initialize the SMSMessage variable.
    SMSMessage smsText = new SMSMessage();

    // A dynamic message can be set instead of the body in the output binding. In this example, we use 
    // the order information to personalize a text message to the mobile number provided for
    // order status updates.
    smsText.Body = msg;
    smsText.To = order.mobileNumber;

    await message.AddAsync(smsText);
}
```

## <a name="example-nodejs-queue-trigger-with-twilio-output-binding"></a><span data-ttu-id="8329b-123">Exempel Node.js kön utlösare med Twilio-utdatabindning</span><span class="sxs-lookup"><span data-stu-id="8329b-123">Example Node.js queue trigger with Twilio output binding</span></span>
<span data-ttu-id="8329b-124">Det här exemplet med Node.js skickar ett SMS till en kund som placeras en order.</span><span class="sxs-lookup"><span data-stu-id="8329b-124">This Node.js example sends a text message to a customer who placed an order.</span></span>

```javascript
module.exports = function (context, myQueueItem) {
    context.log('Node.js queue trigger function processed work item', myQueueItem);

    // In this example the queue item is a JSON string representing an order that contains the name of a 
    // customer and a mobile number to send text updates to.
    var msg = "Hello " + myQueueItem.name + ", thank you for your order.";

    // Even if you want to use a hard coded message and number in the binding, you must at least 
    // initialize the message binding.
    context.bindings.message = {};

    // A dynamic message can be set instead of the body in the output binding. In this example, we use 
    // the order information to personalize a text message to the mobile number provided for
    // order status updates.
    context.bindings.message = {
        body : msg,
        to : myQueueItem.mobileNumber
    };

    context.done();
};
```

## <a name="next-steps"></a><span data-ttu-id="8329b-125">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="8329b-125">Next steps</span></span>
[!INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)]

