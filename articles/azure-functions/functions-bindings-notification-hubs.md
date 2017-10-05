---
title: Azure Functions Notification Hub bindning | Microsoft Docs
description: "Förstå hur du använder Azure Notification Hub-bindning i Azure Functions."
services: functions
documentationcenter: na
author: ggailey777
manager: erikre
editor: 
tags: 
keywords: "Azure functions, funktioner, händelsebearbetning, dynamiska beräkning serverlösa arkitektur"
ms.assetid: 0ff0c949-20bf-430b-8dd5-d72b7b6ee6f7
ms.service: functions
ms.devlang: multiple
ms.topic: reference
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 10/27/2016
ms.author: glenga
ms.openlocfilehash: fa3d37b963c1bb6b58127b9180cd657d7b1dabcc
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="azure-functions-notification-hub-output-binding"></a>Azure Functions Notification Hub-utdatabindning
[!INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

Den här artikeln förklarar hur du konfigurerar och koden Azure Notification Hub bindningar i Azure Functions. 

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

Dina funktioner kan skicka push-meddelanden med hjälp av en konfigurerade Azure Notification Hub med några få rader med kod. Azure Notification Hub måste dock vara konfigurerad för den plattform meddelanden tjänster (PNS) du vill använda. Mer information om hur du konfigurerar en Azure Notification Hub och utveckla ett klientprogram som kan registrera för att ta emot meddelanden, se [komma igång med Notification Hubs](../notification-hubs/notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md) och klicka på klienten målplattformen längst upp.

Du skickar meddelanden kan vara interna meddelanden eller meddelanden om mallar. Interna meddelanden riktade till en viss notification-plattform som konfigurerats i den `platform` egenskapen för utdata-bindning. Ett meddelande om mall kan användas för att fokusera på flera plattformar.   

## <a name="notification-hub-output-binding-properties"></a>Utdata bindning egenskaper för meddelandehubben
Filen function.json innehåller följande egenskaper:

* `name`: Variabelnamn som används i Funktionskoden för hubben meddelandet.
* `type`: måste anges till *”notificationHub”*.
* `tagExpression`: Tagguttryck kan du ange att meddelanden ska levereras till en uppsättning enheter som har registrerats för att ta emot meddelanden som matchar etikettuttrycket.  Mer information finns i [Routning och tagg uttryck](../notification-hubs/notification-hubs-tags-segment-push-message.md).
* `hubName`: Namnet på notification hub resurs i Azure-portalen.
* `connection`: Den här anslutningssträngen måste vara en **programinställning** ange anslutningssträng den *DefaultFullSharedAccessSignature* värde för meddelandehubben.
* `direction`: måste anges till *”out”*. 
* `platform`: Egenskapen plattform anger meddelande platform notification-mål. Måste vara ett av följande värden:
  * Som standard om egenskapen plattform utelämnas från utdata-bindningen kan mallen meddelanden användas för alla plattformar som konfigurerats på Azure Notification Hub. Mer information om hur du använder mallar i allmänhet att skicka mellan plattform meddelanden med en Azure Notification Hub finns [mallar](../notification-hubs/notification-hubs-templates-cross-platform-push-messages.md).
  * `apns`: Apple Push Notification Service. Mer information om hur du konfigurerar meddelandehubben för APNS och ta emot meddelandet i ett klientprogram finns [skicka push-meddelanden till iOS med Azure Notification Hubs](../notification-hubs/notification-hubs-ios-apple-push-notification-apns-get-started.md) 
  * `adm`: [Amazon Device Messaging](https://developer.amazon.com/device-messaging). Mer information om hur du konfigurerar meddelandehubben för ADM och ta emot meddelandet i en Kindle-app finns [komma igång med Notification Hubs för Kindle-appar](../notification-hubs/notification-hubs-kindle-amazon-adm-push-notification.md) 
  * `gcm`: [Google Cloud Messaging](https://developers.google.com/cloud-messaging/). Firebase Cloud Messaging, vilket är den nya versionen av GCM stöds också. Mer information om hur du konfigurerar meddelandehubben för GCM-FCM och ta emot meddelandet i en Android-klientappen finns [skicka push-meddelanden till Android med Azure Notification Hubs](../notification-hubs/notification-hubs-android-push-notification-google-fcm-get-started.md)
  * `wns`: [Windows Push Notification Services](https://msdn.microsoft.com/en-us/windows/uwp/controls-and-patterns/tiles-and-notifications-windows-push-notification-services--wns--overview) riktad Windows-plattformar. Windows Phone 8.1 och senare stöds också av WNS. Mer information om hur du konfigurerar meddelandehubben för WNS och ta emot meddelandet i en app för universella Windowsplattformen (UWP) finns [komma igång med Notification Hubs för Windows Universal-plattformen appar](../notification-hubs/notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md)
  * `mpns`: [Microsoft Push Notification Service](https://msdn.microsoft.com/en-us/library/windows/apps/ff402558.aspx). Den här plattformen stöder Windows Phone 8 och Windows Phone-plattformar som tidigare. Mer information om hur du konfigurerar meddelandehubben för MPNS och ta emot meddelandet i en Windows Phone-app finns [skicka push-meddelanden med Azure Notification Hubs på Windows Phone](../notification-hubs/notification-hubs-windows-mobile-push-notifications-mpns.md)

Exempel function.json:

```json
{
  "bindings": [
    {
      "name": "notification",
      "type": "notificationHub",
      "tagExpression": "",
      "hubName": "my-notification-hub",
      "connection": "MyHubConnectionString",
      "platform": "gcm",
      "direction": "out"
    }
  ],
  "disabled": false
}
```

## <a name="notification-hub-connection-string-setup"></a>Notification hub anslutningsinställningar sträng
Om du vill använda en Notification hub utdata-bindning måste du konfigurera anslutningssträngen för hubben. Detta kan göras på den *integrera* fliken genom att välja din meddelandehubb eller skapa en ny. 

Du kan även manuellt lägga till en anslutningssträng för en befintlig hubb genom att lägga till en anslutningssträng för den *DefaultFullSharedAccessSignature* till meddelandehubben. Den här anslutningssträngen innehåller funktionen access-behörighet att skicka meddelanden. Den *DefaultFullSharedAccessSignature* anslutning strängvärde som kan nås från den **nycklar** knapp i huvudbladet i din notification hub-resurs i Azure-portalen. Använd följande steg om du vill lägga till en anslutningssträng för din hubb manuellt: 

1. På den **funktionsapp** bladet för Azure-portalen klickar du på **funktionen App-Inställningar > Gå till inställningar för Apptjänst**.
2. I den **inställningar** bladet, klickar du på **programinställningar**.
3. Rulla ned till den **appinställningar** , och lägger till en namngiven post för *DefaultFullSharedAccessSignature* värde för meddelandehubben.
4. Referera till din App sträng inställningsnamn i utdata-bindningar. Liknar **MyHubConnectionString** används i exemplet ovan.

## <a name="apns-native-notifications-with-c-queue-triggers"></a>Intern APN-meddelanden med C#-utlösare
Det här exemplet visar hur du använder typer som definieras i den [Microsoft Azure Notification Hubs Library](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/) att skicka en inbyggd APN-avisering. 

```cs
#r "Microsoft.Azure.NotificationHubs"
#r "Newtonsoft.Json"

using System;
using Microsoft.Azure.NotificationHubs;
using Newtonsoft.Json;

public static async Task Run(string myQueueItem, IAsyncCollector<Notification> notification, TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {myQueueItem}");

    // In this example the queue item is a new user to be processed in the form of a JSON string with 
    // a "name" value.
    //
    // The JSON format for a native APNS notification is ...
    // { "aps": { "alert": "notification message" }}  

    log.Info($"Sending APNS notification of a new user");    
    dynamic user = JsonConvert.DeserializeObject(myQueueItem);    
    string apnsNotificationPayload = "{\"aps\": {\"alert\": \"A new user wants to be added (" + 
                                        user.name + ")\" }}";
    log.Info($"{apnsNotificationPayload}");
    await notification.AddAsync(new AppleNotification(apnsNotificationPayload));        
}
```

## <a name="gcm-native-notifications-with-c-queue-triggers"></a>Intern GCM-meddelanden med C#-utlösare
Det här exemplet visar hur du använder typer som definieras i den [Microsoft Azure Notification Hubs Library](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/) att skicka en inbyggd GCM-avisering. 

```cs
#r "Microsoft.Azure.NotificationHubs"
#r "Newtonsoft.Json"

using System;
using Microsoft.Azure.NotificationHubs;
using Newtonsoft.Json;

public static async Task Run(string myQueueItem, IAsyncCollector<Notification> notification, TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {myQueueItem}");

    // In this example the queue item is a new user to be processed in the form of a JSON string with 
    // a "name" value.
    //
    // The JSON format for a native GCM notification is ...
    // { "data": { "message": "notification message" }}  

    log.Info($"Sending GCM notification of a new user");    
    dynamic user = JsonConvert.DeserializeObject(myQueueItem);    
    string gcmNotificationPayload = "{\"data\": {\"message\": \"A new user wants to be added (" + 
                                        user.name + ")\" }}";
    log.Info($"{gcmNotificationPayload}");
    await notification.AddAsync(new GcmNotification(gcmNotificationPayload));        
}
```

## <a name="wns-native-notifications-with-c-queue-triggers"></a>WNS interna meddelanden med C#-utlösare
Det här exemplet visar hur du använder typer som definieras i den [Microsoft Azure Notification Hubs Library](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/) att skicka ett enhetligt WNS popup-meddelande. 

```cs
#r "Microsoft.Azure.NotificationHubs"
#r "Newtonsoft.Json"

using System;
using Microsoft.Azure.NotificationHubs;
using Newtonsoft.Json;

public static async Task Run(string myQueueItem, IAsyncCollector<Notification> notification, TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {myQueueItem}");

    // In this example the queue item is a new user to be processed in the form of a JSON string with 
    // a "name" value.
    //
    // The XML format for a native WNS toast notification is ...
    // <?xml version="1.0" encoding="utf-8"?>
    // <toast>
    //      <visual>
    //     <binding template="ToastText01">
    //       <text id="1">notification message</text>
    //     </binding>
    //   </visual>
    // </toast>

    log.Info($"Sending WNS toast notification of a new user");    
    dynamic user = JsonConvert.DeserializeObject(myQueueItem);    
    string wnsNotificationPayload = "<?xml version=\"1.0\" encoding=\"utf-8\"?>" +
                                    "<toast><visual><binding template=\"ToastText01\">" +
                                        "<text id=\"1\">" + 
                                            "A new user wants to be added (" + user.name + ")" + 
                                        "</text>" +
                                    "</binding></visual></toast>";

    log.Info($"{wnsNotificationPayload}");
    await notification.AddAsync(new WindowsNotification(wnsNotificationPayload));        
}
```

## <a name="template-example-for-nodejs-timer-triggers"></a>Exempel för Node.js timer utlösare
Det här exemplet skickar en avisering för en [mallen registrering](../notification-hubs/notification-hubs-templates-cross-platform-push-messages.md) som innehåller `location` och `message`.

```javascript
module.exports = function (context, myTimer) {
    var timeStamp = new Date().toISOString();

    if(myTimer.isPastDue)
    {
        context.log('Node.js is running late!');
    }
    context.log('Node.js timer trigger function ran!', timeStamp);  
    context.bindings.notification = {
        location: "Redmond",
        message: "Hello from Node!"
    };
    context.done();
};
```

## <a name="template-example-for-f-timer-triggers"></a>Exempel för F # timer utlösare
Det här exemplet skickar en avisering för en [mallen registrering](../notification-hubs/notification-hubs-templates-cross-platform-push-messages.md) som innehåller `location` och `message`.

```fsharp
let Run(myTimer: TimerInfo, notification: byref<IDictionary<string, string>>) =
    notification = dict [("location", "Redmond"); ("message", "Hello from F#!")]
```

## <a name="template-example-using-an-out-parameter"></a>Exempel med en out-parameter
Det här exemplet skickar en avisering för en [mallen registrering](../notification-hubs/notification-hubs-templates-cross-platform-push-messages.md) som innehåller en `message` platshållare i mallen.

```cs
using System;
using System.Threading.Tasks;
using System.Collections.Generic;

public static void Run(string myQueueItem,  out IDictionary<string, string> notification, TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {myQueueItem}");
    notification = GetTemplateProperties(myQueueItem);
}

private static IDictionary<string, string> GetTemplateProperties(string message)
{
    Dictionary<string, string> templateProperties = new Dictionary<string, string>();
    templateProperties["message"] = message;
    return templateProperties;
}
```

## <a name="template-example-with-asynchronous-function"></a>Exempel på mallen med asynkron funktion
Om du använder asynkrona koden är out-parametrar inte tillåtna. I det här fallet använder `IAsyncCollector` att returnera mall-meddelande. Följande kod är ett asynkront exempel av koden ovan. 

```cs
using System;
using System.Threading.Tasks;
using System.Collections.Generic;

public static async Task Run(string myQueueItem, IAsyncCollector<IDictionary<string,string>> notification, TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {myQueueItem}");

    log.Info($"Sending Template Notification to Notification Hub");
    await notification.AddAsync(GetTemplateProperties(myQueueItem));    
}

private static IDictionary<string, string> GetTemplateProperties(string message)
{
    Dictionary<string, string> templateProperties = new Dictionary<string, string>();
    templateProperties["user"] = "A new user wants to be added : " + message;
    return templateProperties;
}
```

## <a name="template-example-using-json"></a>Exempel med JSON
Det här exemplet skickar en avisering för en [mallen registrering](../notification-hubs/notification-hubs-templates-cross-platform-push-messages.md) som innehåller en `message` platshållare i mallen med hjälp av en giltig JSON-sträng.

```cs
using System;

public static void Run(string myQueueItem,  out string notification, TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {myQueueItem}");
    notification = "{\"message\":\"Hello from C#. Processed a queue item!\"}";
}
```

## <a name="template-example-using-notification-hubs-library-types"></a>Exempel med Notification Hubs bibliotekstyper
Det här exemplet visar hur du använder typer som definieras i den [Microsoft Azure Notification Hubs Library](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/). 

```cs
#r "Microsoft.Azure.NotificationHubs"

using System;
using System.Threading.Tasks;
using Microsoft.Azure.NotificationHubs;

public static void Run(string myQueueItem,  out Notification notification, TraceWriter log)
{
   log.Info($"C# Queue trigger function processed: {myQueueItem}");
   notification = GetTemplateNotification(myQueueItem);
}

private static TemplateNotification GetTemplateNotification(string message)
{
    Dictionary<string, string> templateProperties = new Dictionary<string, string>();
    templateProperties["message"] = message;
    return new TemplateNotification(templateProperties);
}
```

## <a name="next-steps"></a>Nästa steg
[!INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)]

