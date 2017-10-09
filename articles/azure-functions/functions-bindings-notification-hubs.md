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
# <a name="azure-functions-notification-hub-output-binding"></a><span data-ttu-id="5975a-104">Azure Functions Notification Hub-utdatabindning</span><span class="sxs-lookup"><span data-stu-id="5975a-104">Azure Functions Notification Hub output binding</span></span>
[!INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

<span data-ttu-id="5975a-105">Den här artikeln förklarar hur tooconfigure och kod Azure Notification Hub-bindningar i Azure Functions.</span><span class="sxs-lookup"><span data-stu-id="5975a-105">This article explains how tooconfigure and code Azure Notification Hub bindings in Azure Functions.</span></span> 

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

<span data-ttu-id="5975a-106">Dina funktioner kan skicka push-meddelanden med hjälp av en konfigurerade Azure Notification Hub med några få rader med kod.</span><span class="sxs-lookup"><span data-stu-id="5975a-106">Your functions can send push notifications using a configured Azure Notification Hub with a few lines of code.</span></span> <span data-ttu-id="5975a-107">Hello Azure Notification Hub måste dock konfigureras för hello plattform meddelanden tjänster (PNS) du vill toouse.</span><span class="sxs-lookup"><span data-stu-id="5975a-107">However, hello Azure Notification Hub must be configured for hello Platform Notifications Services (PNS) you want toouse.</span></span> <span data-ttu-id="5975a-108">Mer information om hur du konfigurerar en Azure Notification Hub och utveckla ett klientprogram som kan registrera tooreceive meddelanden finns [komma igång med Notification Hubs](../notification-hubs/notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md) och klicka på klienten målplattformen på hello längst upp.</span><span class="sxs-lookup"><span data-stu-id="5975a-108">For more information on configuring an Azure Notification Hub and developing a client applications that register tooreceive notifications, see [Getting started with Notification Hubs](../notification-hubs/notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md) and click your target client platform at hello top.</span></span>

<span data-ttu-id="5975a-109">hello-meddelanden som du skickar kan vara interna meddelanden eller meddelanden om mallar.</span><span class="sxs-lookup"><span data-stu-id="5975a-109">hello notifications you send can be native notifications or template notifications.</span></span> <span data-ttu-id="5975a-110">Interna meddelanden riktade till en viss notification-plattform som konfigurerats i hello `platform` -egenskapen för hello-utdatabindning.</span><span class="sxs-lookup"><span data-stu-id="5975a-110">Native notifications target a specific notification platform as configured in hello `platform` property of hello output binding.</span></span> <span data-ttu-id="5975a-111">Ett meddelande om mallen kan vara används tootarget flera plattformar.</span><span class="sxs-lookup"><span data-stu-id="5975a-111">A template notification can be used tootarget multiple platforms.</span></span>   

## <a name="notification-hub-output-binding-properties"></a><span data-ttu-id="5975a-112">Utdata bindning egenskaper för meddelandehubben</span><span class="sxs-lookup"><span data-stu-id="5975a-112">Notification hub output binding properties</span></span>
<span data-ttu-id="5975a-113">Hej function.json filen innehåller hello följande egenskaper:</span><span class="sxs-lookup"><span data-stu-id="5975a-113">hello function.json file provides hello following properties:</span></span>

* <span data-ttu-id="5975a-114">`name`: Variabelnamn som används i Funktionskoden för hälsningsmeddelande notification hub.</span><span class="sxs-lookup"><span data-stu-id="5975a-114">`name` : Variable name used in function code for hello notification hub message.</span></span>
* <span data-ttu-id="5975a-115">`type`: måste anges för*”notificationHub”*.</span><span class="sxs-lookup"><span data-stu-id="5975a-115">`type` : must be set too*"notificationHub"*.</span></span>
* <span data-ttu-id="5975a-116">`tagExpression`: Tagguttryck Tillåt att meddelanden levereras tooa uppsättning av enheter som har registrerat tooreceive meddelanden som matchar hello etikettuttrycket toospecify.</span><span class="sxs-lookup"><span data-stu-id="5975a-116">`tagExpression` : Tag expressions allow you toospecify that notifications be delivered tooa set of devices who have registered tooreceive notifications that match hello tag expression.</span></span>  <span data-ttu-id="5975a-117">Mer information finns i [Routning och tagg uttryck](../notification-hubs/notification-hubs-tags-segment-push-message.md).</span><span class="sxs-lookup"><span data-stu-id="5975a-117">For more information, see [Routing and tag expressions](../notification-hubs/notification-hubs-tags-segment-push-message.md).</span></span>
* <span data-ttu-id="5975a-118">`hubName`: Namnet på hello notification hub resurs i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="5975a-118">`hubName` : Name of hello notification hub resource in hello Azure portal.</span></span>
* <span data-ttu-id="5975a-119">`connection`: Den här anslutningssträngen måste vara en **Programinställningen** ange anslutningssträng toohello *DefaultFullSharedAccessSignature* värde för meddelandehubben.</span><span class="sxs-lookup"><span data-stu-id="5975a-119">`connection` : This connection string must be an **Application Setting** connection string set toohello *DefaultFullSharedAccessSignature* value for your notification hub.</span></span>
* <span data-ttu-id="5975a-120">`direction`: måste anges för*”out”*.</span><span class="sxs-lookup"><span data-stu-id="5975a-120">`direction` : must be set too*"out"*.</span></span> 
* <span data-ttu-id="5975a-121">`platform`: hello plattform egenskapen anger hello notification platform notification-mål.</span><span class="sxs-lookup"><span data-stu-id="5975a-121">`platform` : hello platform property indicates hello notification platform your notification targets.</span></span> <span data-ttu-id="5975a-122">Måste vara något av följande värden hello:</span><span class="sxs-lookup"><span data-stu-id="5975a-122">Must be one of hello following values:</span></span>
  * <span data-ttu-id="5975a-123">Som standard om hello plattform egenskapen utelämnas från hello utdata bindning, kan mall meddelanden vara används tootarget någon plattform som konfigurerats på hello Azure Notification Hub.</span><span class="sxs-lookup"><span data-stu-id="5975a-123">By default, if hello platform property is omitted from hello output binding, template notifications can be used tootarget any platform configured on hello Azure Notification Hub.</span></span> <span data-ttu-id="5975a-124">Mer information om hur du använder mallar i allmänhet toosend mellan plattform meddelanden med Azure Notification Hub, se [mallar](../notification-hubs/notification-hubs-templates-cross-platform-push-messages.md).</span><span class="sxs-lookup"><span data-stu-id="5975a-124">For more information on using templates in general toosend cross platform notifications with an Azure Notification Hub, see [Templates](../notification-hubs/notification-hubs-templates-cross-platform-push-messages.md).</span></span>
  * <span data-ttu-id="5975a-125">`apns`: Apple Push Notification Service.</span><span class="sxs-lookup"><span data-stu-id="5975a-125">`apns` : Apple Push Notification Service.</span></span> <span data-ttu-id="5975a-126">Mer information om att konfigurera hello meddelandehubben för APNS och ta emot hello-meddelande i ett klientprogram finns [skicka push-meddelanden tooiOS med Azure Notification Hubs](../notification-hubs/notification-hubs-ios-apple-push-notification-apns-get-started.md)</span><span class="sxs-lookup"><span data-stu-id="5975a-126">For more information on configuring hello notification hub for APNS and receiving hello notification in a client app, see [Sending push notifications tooiOS with Azure Notification Hubs](../notification-hubs/notification-hubs-ios-apple-push-notification-apns-get-started.md)</span></span> 
  * <span data-ttu-id="5975a-127">`adm`: [Amazon Device Messaging](https://developer.amazon.com/device-messaging).</span><span class="sxs-lookup"><span data-stu-id="5975a-127">`adm` : [Amazon Device Messaging](https://developer.amazon.com/device-messaging).</span></span> <span data-ttu-id="5975a-128">Mer information om hur du konfigurerar hello meddelandehubben för ADM och ta emot hello-meddelande i en Kindle-app finns [komma igång med Notification Hubs för Kindle-appar](../notification-hubs/notification-hubs-kindle-amazon-adm-push-notification.md)</span><span class="sxs-lookup"><span data-stu-id="5975a-128">For more information on configuring hello notification hub for ADM and receiving hello notification in a Kindle app, see [Getting Started with Notification Hubs for Kindle apps](../notification-hubs/notification-hubs-kindle-amazon-adm-push-notification.md)</span></span> 
  * <span data-ttu-id="5975a-129">`gcm`: [Google Cloud Messaging](https://developers.google.com/cloud-messaging/).</span><span class="sxs-lookup"><span data-stu-id="5975a-129">`gcm` : [Google Cloud Messaging](https://developers.google.com/cloud-messaging/).</span></span> <span data-ttu-id="5975a-130">Firebase Cloud Messaging som är hello ny version av GCM stöds också.</span><span class="sxs-lookup"><span data-stu-id="5975a-130">Firebase Cloud Messaging, which is hello new version of GCM, is also supported.</span></span> <span data-ttu-id="5975a-131">Mer information om hur du konfigurerar hello meddelandehubben för GCM-FCM och ta emot hello-meddelande i en Android-klientappen finns [skicka push-meddelanden tooAndroid med Azure Notification Hubs](../notification-hubs/notification-hubs-android-push-notification-google-fcm-get-started.md)</span><span class="sxs-lookup"><span data-stu-id="5975a-131">For more information on configuring hello notification hub for GCM/FCM and receiving hello notification in an Android client app, see [Sending push notifications tooAndroid with Azure Notification Hubs](../notification-hubs/notification-hubs-android-push-notification-google-fcm-get-started.md)</span></span>
  * <span data-ttu-id="5975a-132">`wns`: [Windows Push Notification Services](https://msdn.microsoft.com/en-us/windows/uwp/controls-and-patterns/tiles-and-notifications-windows-push-notification-services--wns--overview) riktad Windows-plattformar.</span><span class="sxs-lookup"><span data-stu-id="5975a-132">`wns` : [Windows Push Notification Services](https://msdn.microsoft.com/en-us/windows/uwp/controls-and-patterns/tiles-and-notifications-windows-push-notification-services--wns--overview) targeting Windows platforms.</span></span> <span data-ttu-id="5975a-133">Windows Phone 8.1 och senare stöds också av WNS.</span><span class="sxs-lookup"><span data-stu-id="5975a-133">Windows Phone 8.1 and later is also supported by WNS.</span></span> <span data-ttu-id="5975a-134">Mer information om hur du konfigurerar hello meddelandehubben för WNS och ta emot hello-meddelande i en app för universella Windowsplattformen (UWP) finns [komma igång med Notification Hubs för Windows Universal-plattformen appar](../notification-hubs/notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md)</span><span class="sxs-lookup"><span data-stu-id="5975a-134">For more information on configuring hello notification hub for WNS and receiving hello notification in a Universal Windows Platform (UWP) app, see [Getting started with Notification Hubs for Windows Universal Platform Apps](../notification-hubs/notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md)</span></span>
  * <span data-ttu-id="5975a-135">`mpns`: [Microsoft Push Notification Service](https://msdn.microsoft.com/en-us/library/windows/apps/ff402558.aspx).</span><span class="sxs-lookup"><span data-stu-id="5975a-135">`mpns` : [Microsoft Push Notification Service](https://msdn.microsoft.com/en-us/library/windows/apps/ff402558.aspx).</span></span> <span data-ttu-id="5975a-136">Den här plattformen stöder Windows Phone 8 och Windows Phone-plattformar som tidigare.</span><span class="sxs-lookup"><span data-stu-id="5975a-136">This platform supports Windows Phone 8 and earlier Windows Phone platforms.</span></span> <span data-ttu-id="5975a-137">Mer information om att konfigurera hello meddelandehubben för MPNS och ta emot hello-meddelande i en Windows Phone-app finns [skicka push-meddelanden med Azure Notification Hubs på Windows Phone](../notification-hubs/notification-hubs-windows-mobile-push-notifications-mpns.md)</span><span class="sxs-lookup"><span data-stu-id="5975a-137">For more information on configuring hello notification hub for MPNS and receiving hello notification in a Windows Phone app, see [Sending push notifications with Azure Notification Hubs on Windows Phone](../notification-hubs/notification-hubs-windows-mobile-push-notifications-mpns.md)</span></span>

<span data-ttu-id="5975a-138">Exempel function.json:</span><span class="sxs-lookup"><span data-stu-id="5975a-138">Example function.json:</span></span>

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

## <a name="notification-hub-connection-string-setup"></a><span data-ttu-id="5975a-139">Notification hub anslutningsinställningar sträng</span><span class="sxs-lookup"><span data-stu-id="5975a-139">Notification hub connection string setup</span></span>
<span data-ttu-id="5975a-140">toouse en meddelandehubb utdatabindning måste du konfigurera hello anslutningssträngen för hello hubb.</span><span class="sxs-lookup"><span data-stu-id="5975a-140">toouse a Notification hub output binding, you must configure hello connection string for hello hub.</span></span> <span data-ttu-id="5975a-141">Detta kan göras på hello *integrera* fliken genom att välja din meddelandehubb eller skapa en ny.</span><span class="sxs-lookup"><span data-stu-id="5975a-141">This can be done on hello *Integrate* tab by selecting your notification hub or creating a new one.</span></span> 

<span data-ttu-id="5975a-142">Du kan även manuellt lägga till en anslutningssträng för en befintlig hubb genom att lägga till en anslutningssträng för hello *DefaultFullSharedAccessSignature* tooyour meddelandehubben.</span><span class="sxs-lookup"><span data-stu-id="5975a-142">You can also manually add a connection string for an existing hub by adding a connection string for hello *DefaultFullSharedAccessSignature* tooyour notification hub.</span></span> <span data-ttu-id="5975a-143">Den här anslutningssträngen innehåller funktionen åtkomsten behörighet toosend meddelanden.</span><span class="sxs-lookup"><span data-stu-id="5975a-143">This connection string provides your function access permission toosend notification messages.</span></span> <span data-ttu-id="5975a-144">Hej *DefaultFullSharedAccessSignature* anslutning strängvärde som kan nås från hello **nycklar** knapp i hello huvudbladet i ditt notification hub-resurs i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="5975a-144">hello *DefaultFullSharedAccessSignature* connection string value can be accessed from hello **keys** button in hello main blade of your notification hub resource in hello Azure portal.</span></span> <span data-ttu-id="5975a-145">toomanually lägga till en anslutningssträng för din hubb, Använd hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="5975a-145">toomanually add a connection string for your hub, use hello following steps:</span></span> 

1. <span data-ttu-id="5975a-146">På hello **funktionsapp** bladet för hello Azure-portalen klickar du på **funktionen App-Inställningar > Gå tooApp inställningar**.</span><span class="sxs-lookup"><span data-stu-id="5975a-146">On hello **Function app** blade of hello Azure portal, click **Function App Settings > Go tooApp Service settings**.</span></span>
2. <span data-ttu-id="5975a-147">I hello **inställningar** bladet, klickar du på **programinställningar**.</span><span class="sxs-lookup"><span data-stu-id="5975a-147">In hello **Settings** blade, click **Application Settings**.</span></span>
3. <span data-ttu-id="5975a-148">Bläddra nedåt toohello **appinställningar** , och lägger till en namngiven post för *DefaultFullSharedAccessSignature* värde för meddelandehubben.</span><span class="sxs-lookup"><span data-stu-id="5975a-148">Scroll down toohello **App settings** section, and add a named entry for *DefaultFullSharedAccessSignature* value for your notification hub.</span></span>
4. <span data-ttu-id="5975a-149">Referera till din App inställningsnamn sträng i hello utdata bindningar.</span><span class="sxs-lookup"><span data-stu-id="5975a-149">Reference your App setting string name in hello output bindings.</span></span> <span data-ttu-id="5975a-150">Liknande för**MyHubConnectionString** används i hello-exemplet ovan.</span><span class="sxs-lookup"><span data-stu-id="5975a-150">Similar too**MyHubConnectionString** used in hello example above.</span></span>

## <a name="apns-native-notifications-with-c-queue-triggers"></a><span data-ttu-id="5975a-151">Intern APN-meddelanden med C#-utlösare</span><span class="sxs-lookup"><span data-stu-id="5975a-151">APNS native notifications with C# queue triggers</span></span>
<span data-ttu-id="5975a-152">Det här exemplet illustrerar hur toouse typer som definieras i hello [Microsoft Azure Notification Hubs Library](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/) toosend ett enhetligt APN-meddelande.</span><span class="sxs-lookup"><span data-stu-id="5975a-152">This example shows how toouse types defined in hello [Microsoft Azure Notification Hubs Library](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/) toosend a native APNS notification.</span></span> 

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

## <a name="gcm-native-notifications-with-c-queue-triggers"></a><span data-ttu-id="5975a-153">Intern GCM-meddelanden med C#-utlösare</span><span class="sxs-lookup"><span data-stu-id="5975a-153">GCM native notifications with C# queue triggers</span></span>
<span data-ttu-id="5975a-154">Det här exemplet illustrerar hur toouse typer som definieras i hello [Microsoft Azure Notification Hubs Library](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/) toosend ett enhetligt GCM-meddelande.</span><span class="sxs-lookup"><span data-stu-id="5975a-154">This example shows how toouse types defined in hello [Microsoft Azure Notification Hubs Library](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/) toosend a native GCM notification.</span></span> 

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

## <a name="wns-native-notifications-with-c-queue-triggers"></a><span data-ttu-id="5975a-155">WNS interna meddelanden med C#-utlösare</span><span class="sxs-lookup"><span data-stu-id="5975a-155">WNS native notifications with C# queue triggers</span></span>
<span data-ttu-id="5975a-156">Det här exemplet illustrerar hur toouse typer som definieras i hello [Microsoft Azure Notification Hubs Library](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/) toosend interna WNS popup-meddelande.</span><span class="sxs-lookup"><span data-stu-id="5975a-156">This example shows how toouse types defined in hello [Microsoft Azure Notification Hubs Library](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/) toosend a native WNS toast notification.</span></span> 

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

## <a name="template-example-for-nodejs-timer-triggers"></a><span data-ttu-id="5975a-157">Exempel för Node.js timer utlösare</span><span class="sxs-lookup"><span data-stu-id="5975a-157">Template example for Node.js timer triggers</span></span>
<span data-ttu-id="5975a-158">Det här exemplet skickar en avisering för en [mallen registrering](../notification-hubs/notification-hubs-templates-cross-platform-push-messages.md) som innehåller `location` och `message`.</span><span class="sxs-lookup"><span data-stu-id="5975a-158">This example sends a notification for a [template registration](../notification-hubs/notification-hubs-templates-cross-platform-push-messages.md) that contains `location` and `message`.</span></span>

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

## <a name="template-example-for-f-timer-triggers"></a><span data-ttu-id="5975a-159">Exempel för F # timer utlösare</span><span class="sxs-lookup"><span data-stu-id="5975a-159">Template example for F# timer triggers</span></span>
<span data-ttu-id="5975a-160">Det här exemplet skickar en avisering för en [mallen registrering](../notification-hubs/notification-hubs-templates-cross-platform-push-messages.md) som innehåller `location` och `message`.</span><span class="sxs-lookup"><span data-stu-id="5975a-160">This example sends a notification for a [template registration](../notification-hubs/notification-hubs-templates-cross-platform-push-messages.md) that contains `location` and `message`.</span></span>

```fsharp
let Run(myTimer: TimerInfo, notification: byref<IDictionary<string, string>>) =
    notification = dict [("location", "Redmond"); ("message", "Hello from F#!")]
```

## <a name="template-example-using-an-out-parameter"></a><span data-ttu-id="5975a-161">Exempel med en out-parameter</span><span class="sxs-lookup"><span data-stu-id="5975a-161">Template example using an out parameter</span></span>
<span data-ttu-id="5975a-162">Det här exemplet skickar en avisering för en [mallen registrering](../notification-hubs/notification-hubs-templates-cross-platform-push-messages.md) som innehåller en `message` platshållare i hello mallen.</span><span class="sxs-lookup"><span data-stu-id="5975a-162">This example sends a notification for a [template registration](../notification-hubs/notification-hubs-templates-cross-platform-push-messages.md) that contains a `message` place holder in hello template.</span></span>

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

## <a name="template-example-with-asynchronous-function"></a><span data-ttu-id="5975a-163">Exempel på mallen med asynkron funktion</span><span class="sxs-lookup"><span data-stu-id="5975a-163">Template example with asynchronous function</span></span>
<span data-ttu-id="5975a-164">Om du använder asynkrona koden är out-parametrar inte tillåtna.</span><span class="sxs-lookup"><span data-stu-id="5975a-164">If you are using asynchronous code, out parameters are not allowed.</span></span> <span data-ttu-id="5975a-165">I det här fallet använder `IAsyncCollector` tooreturn mall-meddelande.</span><span class="sxs-lookup"><span data-stu-id="5975a-165">In this case use `IAsyncCollector` tooreturn your template notification.</span></span> <span data-ttu-id="5975a-166">hello är följande kod en asynkron exempel på hello koden ovan.</span><span class="sxs-lookup"><span data-stu-id="5975a-166">hello following code is an asynchronous example of hello code above.</span></span> 

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

## <a name="template-example-using-json"></a><span data-ttu-id="5975a-167">Exempel med JSON</span><span class="sxs-lookup"><span data-stu-id="5975a-167">Template example using JSON</span></span>
<span data-ttu-id="5975a-168">Det här exemplet skickar en avisering för en [mallen registrering](../notification-hubs/notification-hubs-templates-cross-platform-push-messages.md) som innehåller en `message` platshållare i hello mallen med en giltig JSON-sträng.</span><span class="sxs-lookup"><span data-stu-id="5975a-168">This example sends a notification for a [template registration](../notification-hubs/notification-hubs-templates-cross-platform-push-messages.md) that contains a `message` place holder in hello template using a valid JSON string.</span></span>

```cs
using System;

public static void Run(string myQueueItem,  out string notification, TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {myQueueItem}");
    notification = "{\"message\":\"Hello from C#. Processed a queue item!\"}";
}
```

## <a name="template-example-using-notification-hubs-library-types"></a><span data-ttu-id="5975a-169">Exempel med Notification Hubs bibliotekstyper</span><span class="sxs-lookup"><span data-stu-id="5975a-169">Template example using Notification Hubs library types</span></span>
<span data-ttu-id="5975a-170">Det här exemplet illustrerar hur toouse typer som definieras i hello [Microsoft Azure Notification Hubs Library](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/).</span><span class="sxs-lookup"><span data-stu-id="5975a-170">This example shows how toouse types defined in hello [Microsoft Azure Notification Hubs Library](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/).</span></span> 

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

## <a name="next-steps"></a><span data-ttu-id="5975a-171">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="5975a-171">Next steps</span></span>
[!INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)]

