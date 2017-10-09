---
title: aaaAzure funktioner Notification Hub bindning | Microsoft Docs
description: "Förstå hur toouse Azure Notification Hub-bindning i Azure Functions."
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
ms.openlocfilehash: d192424a8ec701d02f8bcb4aa4c1d189b20537a5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-functions-notification-hub-output-binding"></a>Azure Functions Notification Hub-utdatabindning
[!INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

Den här artikeln förklarar hur tooconfigure och kod Azure Notification Hub-bindningar i Azure Functions. 

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

Dina funktioner kan skicka push-meddelanden med hjälp av en konfigurerade Azure Notification Hub med några få rader med kod. Hello Azure Notification Hub måste dock konfigureras för hello plattform meddelanden tjänster (PNS) du vill toouse. Mer information om hur du konfigurerar en Azure Notification Hub och utveckla ett klientprogram som kan registrera tooreceive meddelanden finns [komma igång med Notification Hubs](../notification-hubs/notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md) och klicka på klienten målplattformen på hello längst upp.

hello-meddelanden som du skickar kan vara interna meddelanden eller meddelanden om mallar. Interna meddelanden riktade till en viss notification-plattform som konfigurerats i hello `platform` -egenskapen för hello-utdatabindning. Ett meddelande om mallen kan vara används tootarget flera plattformar.   

## <a name="notification-hub-output-binding-properties"></a>Utdata bindning egenskaper för meddelandehubben
Hej function.json filen innehåller hello följande egenskaper:

* `name`: Variabelnamn som används i Funktionskoden för hälsningsmeddelande notification hub.
* `type`: måste anges för*”notificationHub”*.
* `tagExpression`: Tagguttryck Tillåt att meddelanden levereras tooa uppsättning av enheter som har registrerat tooreceive meddelanden som matchar hello etikettuttrycket toospecify.  Mer information finns i [Routning och tagg uttryck](../notification-hubs/notification-hubs-tags-segment-push-message.md).
* `hubName`: Namnet på hello notification hub resurs i hello Azure-portalen.
* `connection`: Den här anslutningssträngen måste vara en **Programinställningen** ange anslutningssträng toohello *DefaultFullSharedAccessSignature* värde för meddelandehubben.
* `direction`: måste anges för*”out”*. 
* `platform`: hello plattform egenskapen anger hello notification platform notification-mål. Måste vara något av följande värden hello:
  * Som standard om hello plattform egenskapen utelämnas från hello utdata bindning, kan mall meddelanden vara används tootarget någon plattform som konfigurerats på hello Azure Notification Hub. Mer information om hur du använder mallar i allmänhet toosend mellan plattform meddelanden med Azure Notification Hub, se [mallar](../notification-hubs/notification-hubs-templates-cross-platform-push-messages.md).
  * `apns`: Apple Push Notification Service. Mer information om att konfigurera hello meddelandehubben för APNS och ta emot hello-meddelande i ett klientprogram finns [skicka push-meddelanden tooiOS med Azure Notification Hubs](../notification-hubs/notification-hubs-ios-apple-push-notification-apns-get-started.md) 
  * `adm`: [Amazon Device Messaging](https://developer.amazon.com/device-messaging). Mer information om hur du konfigurerar hello meddelandehubben för ADM och ta emot hello-meddelande i en Kindle-app finns [komma igång med Notification Hubs för Kindle-appar](../notification-hubs/notification-hubs-kindle-amazon-adm-push-notification.md) 
  * `gcm`: [Google Cloud Messaging](https://developers.google.com/cloud-messaging/). Firebase Cloud Messaging som är hello ny version av GCM stöds också. Mer information om hur du konfigurerar hello meddelandehubben för GCM-FCM och ta emot hello-meddelande i en Android-klientappen finns [skicka push-meddelanden tooAndroid med Azure Notification Hubs](../notification-hubs/notification-hubs-android-push-notification-google-fcm-get-started.md)
  * `wns`: [Windows Push Notification Services](https://msdn.microsoft.com/en-us/windows/uwp/controls-and-patterns/tiles-and-notifications-windows-push-notification-services--wns--overview) riktad Windows-plattformar. Windows Phone 8.1 och senare stöds också av WNS. Mer information om hur du konfigurerar hello meddelandehubben för WNS och ta emot hello-meddelande i en app för universella Windowsplattformen (UWP) finns [komma igång med Notification Hubs för Windows Universal-plattformen appar](../notification-hubs/notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md)
  * `mpns`: [Microsoft Push Notification Service](https://msdn.microsoft.com/en-us/library/windows/apps/ff402558.aspx). Den här plattformen stöder Windows Phone 8 och Windows Phone-plattformar som tidigare. Mer information om att konfigurera hello meddelandehubben för MPNS och ta emot hello-meddelande i en Windows Phone-app finns [skicka push-meddelanden med Azure Notification Hubs på Windows Phone](../notification-hubs/notification-hubs-windows-mobile-push-notifications-mpns.md)

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
toouse en meddelandehubb utdatabindning måste du konfigurera hello anslutningssträngen för hello hubb. Detta kan göras på hello *integrera* fliken genom att välja din meddelandehubb eller skapa en ny. 

Du kan även manuellt lägga till en anslutningssträng för en befintlig hubb genom att lägga till en anslutningssträng för hello *DefaultFullSharedAccessSignature* tooyour meddelandehubben. Den här anslutningssträngen innehåller funktionen åtkomsten behörighet toosend meddelanden. Hej *DefaultFullSharedAccessSignature* anslutning strängvärde som kan nås från hello **nycklar** knapp i hello huvudbladet i ditt notification hub-resurs i hello Azure-portalen. toomanually lägga till en anslutningssträng för din hubb, Använd hello följande steg: 

1. På hello **funktionsapp** bladet för hello Azure-portalen klickar du på **funktionen App-Inställningar > Gå tooApp inställningar**.
2. I hello **inställningar** bladet, klickar du på **programinställningar**.
3. Bläddra nedåt toohello **appinställningar** , och lägger till en namngiven post för *DefaultFullSharedAccessSignature* värde för meddelandehubben.
4. Referera till din App inställningsnamn sträng i hello utdata bindningar. Liknande för**MyHubConnectionString** används i hello-exemplet ovan.

## <a name="apns-native-notifications-with-c-queue-triggers"></a>Intern APN-meddelanden med C#-utlösare
Det här exemplet illustrerar hur toouse typer som definieras i hello [Microsoft Azure Notification Hubs Library](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/) toosend ett enhetligt APN-meddelande. 

```cs
#r "Microsoft.Azure.NotificationHubs"
#r "Newtonsoft.Json"

using System;
using Microsoft.Azure.NotificationHubs;
using Newtonsoft.Json;

public static async Task Run(string myQueueItem, IAsyncCollector<Notification> notification, TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {myQueueItem}");

    // In this example hello queue item is a new user toobe processed in hello form of a JSON string with 
    // a "name" value.
    //
    // hello JSON format for a native APNS notification is ...
    // { "aps": { "alert": "notification message" }}  

    log.Info($"Sending APNS notification of a new user");    
    dynamic user = JsonConvert.DeserializeObject(myQueueItem);    
    string apnsNotificationPayload = "{\"aps\": {\"alert\": \"A new user wants toobe added (" + 
                                        user.name + ")\" }}";
    log.Info($"{apnsNotificationPayload}");
    await notification.AddAsync(new AppleNotification(apnsNotificationPayload));        
}
```

## <a name="gcm-native-notifications-with-c-queue-triggers"></a>Intern GCM-meddelanden med C#-utlösare
Det här exemplet illustrerar hur toouse typer som definieras i hello [Microsoft Azure Notification Hubs Library](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/) toosend ett enhetligt GCM-meddelande. 

```cs
#r "Microsoft.Azure.NotificationHubs"
#r "Newtonsoft.Json"

using System;
using Microsoft.Azure.NotificationHubs;
using Newtonsoft.Json;

public static async Task Run(string myQueueItem, IAsyncCollector<Notification> notification, TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {myQueueItem}");

    // In this example hello queue item is a new user toobe processed in hello form of a JSON string with 
    // a "name" value.
    //
    // hello JSON format for a native GCM notification is ...
    // { "data": { "message": "notification message" }}  

    log.Info($"Sending GCM notification of a new user");    
    dynamic user = JsonConvert.DeserializeObject(myQueueItem);    
    string gcmNotificationPayload = "{\"data\": {\"message\": \"A new user wants toobe added (" + 
                                        user.name + ")\" }}";
    log.Info($"{gcmNotificationPayload}");
    await notification.AddAsync(new GcmNotification(gcmNotificationPayload));        
}
```

## <a name="wns-native-notifications-with-c-queue-triggers"></a>WNS interna meddelanden med C#-utlösare
Det här exemplet illustrerar hur toouse typer som definieras i hello [Microsoft Azure Notification Hubs Library](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/) toosend interna WNS popup-meddelande. 

```cs
#r "Microsoft.Azure.NotificationHubs"
#r "Newtonsoft.Json"

using System;
using Microsoft.Azure.NotificationHubs;
using Newtonsoft.Json;

public static async Task Run(string myQueueItem, IAsyncCollector<Notification> notification, TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {myQueueItem}");

    // In this example hello queue item is a new user toobe processed in hello form of a JSON string with 
    // a "name" value.
    //
    // hello XML format for a native WNS toast notification is ...
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
                                            "A new user wants toobe added (" + user.name + ")" + 
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
Det här exemplet skickar en avisering för en [mallen registrering](../notification-hubs/notification-hubs-templates-cross-platform-push-messages.md) som innehåller en `message` platshållare i hello mallen.

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
Om du använder asynkrona koden är out-parametrar inte tillåtna. I det här fallet använder `IAsyncCollector` tooreturn mall-meddelande. hello är följande kod en asynkron exempel på hello koden ovan. 

```cs
using System;
using System.Threading.Tasks;
using System.Collections.Generic;

public static async Task Run(string myQueueItem, IAsyncCollector<IDictionary<string,string>> notification, TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {myQueueItem}");

    log.Info($"Sending Template Notification tooNotification Hub");
    await notification.AddAsync(GetTemplateProperties(myQueueItem));    
}

private static IDictionary<string, string> GetTemplateProperties(string message)
{
    Dictionary<string, string> templateProperties = new Dictionary<string, string>();
    templateProperties["user"] = "A new user wants toobe added : " + message;
    return templateProperties;
}
```

## <a name="template-example-using-json"></a>Exempel med JSON
Det här exemplet skickar en avisering för en [mallen registrering](../notification-hubs/notification-hubs-templates-cross-platform-push-messages.md) som innehåller en `message` platshållare i hello mallen med en giltig JSON-sträng.

```cs
using System;

public static void Run(string myQueueItem,  out string notification, TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {myQueueItem}");
    notification = "{\"message\":\"Hello from C#. Processed a queue item!\"}";
}
```

## <a name="template-example-using-notification-hubs-library-types"></a>Exempel med Notification Hubs bibliotekstyper
Det här exemplet illustrerar hur toouse typer som definieras i hello [Microsoft Azure Notification Hubs Library](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/). 

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

