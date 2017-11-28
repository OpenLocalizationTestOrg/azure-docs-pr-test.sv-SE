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
# <a name="azure-functions-notification-hub-output-binding"></a><span data-ttu-id="887bc-104">Azure Functions Notification Hub-utdatabindning</span><span class="sxs-lookup"><span data-stu-id="887bc-104">Azure Functions Notification Hub output binding</span></span>
[!INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

<span data-ttu-id="887bc-105">Den här artikeln förklarar hur du konfigurerar och koden Azure Notification Hub bindningar i Azure Functions.</span><span class="sxs-lookup"><span data-stu-id="887bc-105">This article explains how to configure and code Azure Notification Hub bindings in Azure Functions.</span></span> 

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

<span data-ttu-id="887bc-106">Dina funktioner kan skicka push-meddelanden med hjälp av en konfigurerade Azure Notification Hub med några få rader med kod.</span><span class="sxs-lookup"><span data-stu-id="887bc-106">Your functions can send push notifications using a configured Azure Notification Hub with a few lines of code.</span></span> <span data-ttu-id="887bc-107">Azure Notification Hub måste dock vara konfigurerad för den plattform meddelanden tjänster (PNS) du vill använda.</span><span class="sxs-lookup"><span data-stu-id="887bc-107">However, the Azure Notification Hub must be configured for the Platform Notifications Services (PNS) you want to use.</span></span> <span data-ttu-id="887bc-108">Mer information om hur du konfigurerar en Azure Notification Hub och utveckla ett klientprogram som kan registrera för att ta emot meddelanden, se [komma igång med Notification Hubs](../notification-hubs/notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md) och klicka på klienten målplattformen längst upp.</span><span class="sxs-lookup"><span data-stu-id="887bc-108">For more information on configuring an Azure Notification Hub and developing a client applications that register to receive notifications, see [Getting started with Notification Hubs](../notification-hubs/notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md) and click your target client platform at the top.</span></span>

<span data-ttu-id="887bc-109">Du skickar meddelanden kan vara interna meddelanden eller meddelanden om mallar.</span><span class="sxs-lookup"><span data-stu-id="887bc-109">The notifications you send can be native notifications or template notifications.</span></span> <span data-ttu-id="887bc-110">Interna meddelanden riktade till en viss notification-plattform som konfigurerats i den `platform` egenskapen för utdata-bindning.</span><span class="sxs-lookup"><span data-stu-id="887bc-110">Native notifications target a specific notification platform as configured in the `platform` property of the output binding.</span></span> <span data-ttu-id="887bc-111">Ett meddelande om mall kan användas för att fokusera på flera plattformar.</span><span class="sxs-lookup"><span data-stu-id="887bc-111">A template notification can be used to target multiple platforms.</span></span>   

## <a name="notification-hub-output-binding-properties"></a><span data-ttu-id="887bc-112">Utdata bindning egenskaper för meddelandehubben</span><span class="sxs-lookup"><span data-stu-id="887bc-112">Notification hub output binding properties</span></span>
<span data-ttu-id="887bc-113">Filen function.json innehåller följande egenskaper:</span><span class="sxs-lookup"><span data-stu-id="887bc-113">The function.json file provides the following properties:</span></span>

* <span data-ttu-id="887bc-114">`name`: Variabelnamn som används i Funktionskoden för hubben meddelandet.</span><span class="sxs-lookup"><span data-stu-id="887bc-114">`name` : Variable name used in function code for the notification hub message.</span></span>
* <span data-ttu-id="887bc-115">`type`: måste anges till *”notificationHub”*.</span><span class="sxs-lookup"><span data-stu-id="887bc-115">`type` : must be set to *"notificationHub"*.</span></span>
* <span data-ttu-id="887bc-116">`tagExpression`: Tagguttryck kan du ange att meddelanden ska levereras till en uppsättning enheter som har registrerats för att ta emot meddelanden som matchar etikettuttrycket.</span><span class="sxs-lookup"><span data-stu-id="887bc-116">`tagExpression` : Tag expressions allow you to specify that notifications be delivered to a set of devices who have registered to receive notifications that match the tag expression.</span></span>  <span data-ttu-id="887bc-117">Mer information finns i [Routning och tagg uttryck](../notification-hubs/notification-hubs-tags-segment-push-message.md).</span><span class="sxs-lookup"><span data-stu-id="887bc-117">For more information, see [Routing and tag expressions](../notification-hubs/notification-hubs-tags-segment-push-message.md).</span></span>
* <span data-ttu-id="887bc-118">`hubName`: Namnet på notification hub resurs i Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="887bc-118">`hubName` : Name of the notification hub resource in the Azure portal.</span></span>
* <span data-ttu-id="887bc-119">`connection`: Den här anslutningssträngen måste vara en **programinställning** ange anslutningssträng den *DefaultFullSharedAccessSignature* värde för meddelandehubben.</span><span class="sxs-lookup"><span data-stu-id="887bc-119">`connection` : This connection string must be an **Application Setting** connection string set to the *DefaultFullSharedAccessSignature* value for your notification hub.</span></span>
* <span data-ttu-id="887bc-120">`direction`: måste anges till *”out”*.</span><span class="sxs-lookup"><span data-stu-id="887bc-120">`direction` : must be set to *"out"*.</span></span> 
* <span data-ttu-id="887bc-121">`platform`: Egenskapen plattform anger meddelande platform notification-mål.</span><span class="sxs-lookup"><span data-stu-id="887bc-121">`platform` : The platform property indicates the notification platform your notification targets.</span></span> <span data-ttu-id="887bc-122">Måste vara ett av följande värden:</span><span class="sxs-lookup"><span data-stu-id="887bc-122">Must be one of the following values:</span></span>
  * <span data-ttu-id="887bc-123">Som standard om egenskapen plattform utelämnas från utdata-bindningen kan mallen meddelanden användas för alla plattformar som konfigurerats på Azure Notification Hub.</span><span class="sxs-lookup"><span data-stu-id="887bc-123">By default, if the platform property is omitted from the output binding, template notifications can be used to target any platform configured on the Azure Notification Hub.</span></span> <span data-ttu-id="887bc-124">Mer information om hur du använder mallar i allmänhet att skicka mellan plattform meddelanden med en Azure Notification Hub finns [mallar](../notification-hubs/notification-hubs-templates-cross-platform-push-messages.md).</span><span class="sxs-lookup"><span data-stu-id="887bc-124">For more information on using templates in general to send cross platform notifications with an Azure Notification Hub, see [Templates](../notification-hubs/notification-hubs-templates-cross-platform-push-messages.md).</span></span>
  * <span data-ttu-id="887bc-125">`apns`: Apple Push Notification Service.</span><span class="sxs-lookup"><span data-stu-id="887bc-125">`apns` : Apple Push Notification Service.</span></span> <span data-ttu-id="887bc-126">Mer information om hur du konfigurerar meddelandehubben för APNS och ta emot meddelandet i ett klientprogram finns [skicka push-meddelanden till iOS med Azure Notification Hubs](../notification-hubs/notification-hubs-ios-apple-push-notification-apns-get-started.md)</span><span class="sxs-lookup"><span data-stu-id="887bc-126">For more information on configuring the notification hub for APNS and receiving the notification in a client app, see [Sending push notifications to iOS with Azure Notification Hubs](../notification-hubs/notification-hubs-ios-apple-push-notification-apns-get-started.md)</span></span> 
  * <span data-ttu-id="887bc-127">`adm`: [Amazon Device Messaging](https://developer.amazon.com/device-messaging).</span><span class="sxs-lookup"><span data-stu-id="887bc-127">`adm` : [Amazon Device Messaging](https://developer.amazon.com/device-messaging).</span></span> <span data-ttu-id="887bc-128">Mer information om hur du konfigurerar meddelandehubben för ADM och ta emot meddelandet i en Kindle-app finns [komma igång med Notification Hubs för Kindle-appar](../notification-hubs/notification-hubs-kindle-amazon-adm-push-notification.md)</span><span class="sxs-lookup"><span data-stu-id="887bc-128">For more information on configuring the notification hub for ADM and receiving the notification in a Kindle app, see [Getting Started with Notification Hubs for Kindle apps](../notification-hubs/notification-hubs-kindle-amazon-adm-push-notification.md)</span></span> 
  * <span data-ttu-id="887bc-129">`gcm`: [Google Cloud Messaging](https://developers.google.com/cloud-messaging/).</span><span class="sxs-lookup"><span data-stu-id="887bc-129">`gcm` : [Google Cloud Messaging](https://developers.google.com/cloud-messaging/).</span></span> <span data-ttu-id="887bc-130">Firebase Cloud Messaging, vilket är den nya versionen av GCM stöds också.</span><span class="sxs-lookup"><span data-stu-id="887bc-130">Firebase Cloud Messaging, which is the new version of GCM, is also supported.</span></span> <span data-ttu-id="887bc-131">Mer information om hur du konfigurerar meddelandehubben för GCM-FCM och ta emot meddelandet i en Android-klientappen finns [skicka push-meddelanden till Android med Azure Notification Hubs](../notification-hubs/notification-hubs-android-push-notification-google-fcm-get-started.md)</span><span class="sxs-lookup"><span data-stu-id="887bc-131">For more information on configuring the notification hub for GCM/FCM and receiving the notification in an Android client app, see [Sending push notifications to Android with Azure Notification Hubs](../notification-hubs/notification-hubs-android-push-notification-google-fcm-get-started.md)</span></span>
  * <span data-ttu-id="887bc-132">`wns`: [Windows Push Notification Services](https://msdn.microsoft.com/en-us/windows/uwp/controls-and-patterns/tiles-and-notifications-windows-push-notification-services--wns--overview) riktad Windows-plattformar.</span><span class="sxs-lookup"><span data-stu-id="887bc-132">`wns` : [Windows Push Notification Services](https://msdn.microsoft.com/en-us/windows/uwp/controls-and-patterns/tiles-and-notifications-windows-push-notification-services--wns--overview) targeting Windows platforms.</span></span> <span data-ttu-id="887bc-133">Windows Phone 8.1 och senare stöds också av WNS.</span><span class="sxs-lookup"><span data-stu-id="887bc-133">Windows Phone 8.1 and later is also supported by WNS.</span></span> <span data-ttu-id="887bc-134">Mer information om hur du konfigurerar meddelandehubben för WNS och ta emot meddelandet i en app för universella Windowsplattformen (UWP) finns [komma igång med Notification Hubs för Windows Universal-plattformen appar](../notification-hubs/notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md)</span><span class="sxs-lookup"><span data-stu-id="887bc-134">For more information on configuring the notification hub for WNS and receiving the notification in a Universal Windows Platform (UWP) app, see [Getting started with Notification Hubs for Windows Universal Platform Apps](../notification-hubs/notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md)</span></span>
  * <span data-ttu-id="887bc-135">`mpns`: [Microsoft Push Notification Service](https://msdn.microsoft.com/en-us/library/windows/apps/ff402558.aspx).</span><span class="sxs-lookup"><span data-stu-id="887bc-135">`mpns` : [Microsoft Push Notification Service](https://msdn.microsoft.com/en-us/library/windows/apps/ff402558.aspx).</span></span> <span data-ttu-id="887bc-136">Den här plattformen stöder Windows Phone 8 och Windows Phone-plattformar som tidigare.</span><span class="sxs-lookup"><span data-stu-id="887bc-136">This platform supports Windows Phone 8 and earlier Windows Phone platforms.</span></span> <span data-ttu-id="887bc-137">Mer information om hur du konfigurerar meddelandehubben för MPNS och ta emot meddelandet i en Windows Phone-app finns [skicka push-meddelanden med Azure Notification Hubs på Windows Phone](../notification-hubs/notification-hubs-windows-mobile-push-notifications-mpns.md)</span><span class="sxs-lookup"><span data-stu-id="887bc-137">For more information on configuring the notification hub for MPNS and receiving the notification in a Windows Phone app, see [Sending push notifications with Azure Notification Hubs on Windows Phone](../notification-hubs/notification-hubs-windows-mobile-push-notifications-mpns.md)</span></span>

<span data-ttu-id="887bc-138">Exempel function.json:</span><span class="sxs-lookup"><span data-stu-id="887bc-138">Example function.json:</span></span>

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

## <a name="notification-hub-connection-string-setup"></a><span data-ttu-id="887bc-139">Notification hub anslutningsinställningar sträng</span><span class="sxs-lookup"><span data-stu-id="887bc-139">Notification hub connection string setup</span></span>
<span data-ttu-id="887bc-140">Om du vill använda en Notification hub utdata-bindning måste du konfigurera anslutningssträngen för hubben.</span><span class="sxs-lookup"><span data-stu-id="887bc-140">To use a Notification hub output binding, you must configure the connection string for the hub.</span></span> <span data-ttu-id="887bc-141">Detta kan göras på den *integrera* fliken genom att välja din meddelandehubb eller skapa en ny.</span><span class="sxs-lookup"><span data-stu-id="887bc-141">This can be done on the *Integrate* tab by selecting your notification hub or creating a new one.</span></span> 

<span data-ttu-id="887bc-142">Du kan även manuellt lägga till en anslutningssträng för en befintlig hubb genom att lägga till en anslutningssträng för den *DefaultFullSharedAccessSignature* till meddelandehubben.</span><span class="sxs-lookup"><span data-stu-id="887bc-142">You can also manually add a connection string for an existing hub by adding a connection string for the *DefaultFullSharedAccessSignature* to your notification hub.</span></span> <span data-ttu-id="887bc-143">Den här anslutningssträngen innehåller funktionen access-behörighet att skicka meddelanden.</span><span class="sxs-lookup"><span data-stu-id="887bc-143">This connection string provides your function access permission to send notification messages.</span></span> <span data-ttu-id="887bc-144">Den *DefaultFullSharedAccessSignature* anslutning strängvärde som kan nås från den **nycklar** knapp i huvudbladet i din notification hub-resurs i Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="887bc-144">The *DefaultFullSharedAccessSignature* connection string value can be accessed from the **keys** button in the main blade of your notification hub resource in the Azure portal.</span></span> <span data-ttu-id="887bc-145">Använd följande steg om du vill lägga till en anslutningssträng för din hubb manuellt:</span><span class="sxs-lookup"><span data-stu-id="887bc-145">To manually add a connection string for your hub, use the following steps:</span></span> 

1. <span data-ttu-id="887bc-146">På den **funktionsapp** bladet för Azure-portalen klickar du på **funktionen App-Inställningar > Gå till inställningar för Apptjänst**.</span><span class="sxs-lookup"><span data-stu-id="887bc-146">On the **Function app** blade of the Azure portal, click **Function App Settings > Go to App Service settings**.</span></span>
2. <span data-ttu-id="887bc-147">I den **inställningar** bladet, klickar du på **programinställningar**.</span><span class="sxs-lookup"><span data-stu-id="887bc-147">In the **Settings** blade, click **Application Settings**.</span></span>
3. <span data-ttu-id="887bc-148">Rulla ned till den **appinställningar** , och lägger till en namngiven post för *DefaultFullSharedAccessSignature* värde för meddelandehubben.</span><span class="sxs-lookup"><span data-stu-id="887bc-148">Scroll down to the **App settings** section, and add a named entry for *DefaultFullSharedAccessSignature* value for your notification hub.</span></span>
4. <span data-ttu-id="887bc-149">Referera till din App sträng inställningsnamn i utdata-bindningar.</span><span class="sxs-lookup"><span data-stu-id="887bc-149">Reference your App setting string name in the output bindings.</span></span> <span data-ttu-id="887bc-150">Liknar **MyHubConnectionString** används i exemplet ovan.</span><span class="sxs-lookup"><span data-stu-id="887bc-150">Similar to **MyHubConnectionString** used in the example above.</span></span>

## <a name="apns-native-notifications-with-c-queue-triggers"></a><span data-ttu-id="887bc-151">Intern APN-meddelanden med C#-utlösare</span><span class="sxs-lookup"><span data-stu-id="887bc-151">APNS native notifications with C# queue triggers</span></span>
<span data-ttu-id="887bc-152">Det här exemplet visar hur du använder typer som definieras i den [Microsoft Azure Notification Hubs Library](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/) att skicka en inbyggd APN-avisering.</span><span class="sxs-lookup"><span data-stu-id="887bc-152">This example shows how to use types defined in the [Microsoft Azure Notification Hubs Library](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/) to send a native APNS notification.</span></span> 

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

## <a name="gcm-native-notifications-with-c-queue-triggers"></a><span data-ttu-id="887bc-153">Intern GCM-meddelanden med C#-utlösare</span><span class="sxs-lookup"><span data-stu-id="887bc-153">GCM native notifications with C# queue triggers</span></span>
<span data-ttu-id="887bc-154">Det här exemplet visar hur du använder typer som definieras i den [Microsoft Azure Notification Hubs Library](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/) att skicka en inbyggd GCM-avisering.</span><span class="sxs-lookup"><span data-stu-id="887bc-154">This example shows how to use types defined in the [Microsoft Azure Notification Hubs Library](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/) to send a native GCM notification.</span></span> 

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

## <a name="wns-native-notifications-with-c-queue-triggers"></a><span data-ttu-id="887bc-155">WNS interna meddelanden med C#-utlösare</span><span class="sxs-lookup"><span data-stu-id="887bc-155">WNS native notifications with C# queue triggers</span></span>
<span data-ttu-id="887bc-156">Det här exemplet visar hur du använder typer som definieras i den [Microsoft Azure Notification Hubs Library](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/) att skicka ett enhetligt WNS popup-meddelande.</span><span class="sxs-lookup"><span data-stu-id="887bc-156">This example shows how to use types defined in the [Microsoft Azure Notification Hubs Library](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/) to send a native WNS toast notification.</span></span> 

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

## <a name="template-example-for-nodejs-timer-triggers"></a><span data-ttu-id="887bc-157">Exempel för Node.js timer utlösare</span><span class="sxs-lookup"><span data-stu-id="887bc-157">Template example for Node.js timer triggers</span></span>
<span data-ttu-id="887bc-158">Det här exemplet skickar en avisering för en [mallen registrering](../notification-hubs/notification-hubs-templates-cross-platform-push-messages.md) som innehåller `location` och `message`.</span><span class="sxs-lookup"><span data-stu-id="887bc-158">This example sends a notification for a [template registration](../notification-hubs/notification-hubs-templates-cross-platform-push-messages.md) that contains `location` and `message`.</span></span>

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

## <a name="template-example-for-f-timer-triggers"></a><span data-ttu-id="887bc-159">Exempel för F # timer utlösare</span><span class="sxs-lookup"><span data-stu-id="887bc-159">Template example for F# timer triggers</span></span>
<span data-ttu-id="887bc-160">Det här exemplet skickar en avisering för en [mallen registrering](../notification-hubs/notification-hubs-templates-cross-platform-push-messages.md) som innehåller `location` och `message`.</span><span class="sxs-lookup"><span data-stu-id="887bc-160">This example sends a notification for a [template registration](../notification-hubs/notification-hubs-templates-cross-platform-push-messages.md) that contains `location` and `message`.</span></span>

```fsharp
let Run(myTimer: TimerInfo, notification: byref<IDictionary<string, string>>) =
    notification = dict [("location", "Redmond"); ("message", "Hello from F#!")]
```

## <a name="template-example-using-an-out-parameter"></a><span data-ttu-id="887bc-161">Exempel med en out-parameter</span><span class="sxs-lookup"><span data-stu-id="887bc-161">Template example using an out parameter</span></span>
<span data-ttu-id="887bc-162">Det här exemplet skickar en avisering för en [mallen registrering](../notification-hubs/notification-hubs-templates-cross-platform-push-messages.md) som innehåller en `message` platshållare i mallen.</span><span class="sxs-lookup"><span data-stu-id="887bc-162">This example sends a notification for a [template registration](../notification-hubs/notification-hubs-templates-cross-platform-push-messages.md) that contains a `message` place holder in the template.</span></span>

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

## <a name="template-example-with-asynchronous-function"></a><span data-ttu-id="887bc-163">Exempel på mallen med asynkron funktion</span><span class="sxs-lookup"><span data-stu-id="887bc-163">Template example with asynchronous function</span></span>
<span data-ttu-id="887bc-164">Om du använder asynkrona koden är out-parametrar inte tillåtna.</span><span class="sxs-lookup"><span data-stu-id="887bc-164">If you are using asynchronous code, out parameters are not allowed.</span></span> <span data-ttu-id="887bc-165">I det här fallet använder `IAsyncCollector` att returnera mall-meddelande.</span><span class="sxs-lookup"><span data-stu-id="887bc-165">In this case use `IAsyncCollector` to return your template notification.</span></span> <span data-ttu-id="887bc-166">Följande kod är ett asynkront exempel av koden ovan.</span><span class="sxs-lookup"><span data-stu-id="887bc-166">The following code is an asynchronous example of the code above.</span></span> 

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

## <a name="template-example-using-json"></a><span data-ttu-id="887bc-167">Exempel med JSON</span><span class="sxs-lookup"><span data-stu-id="887bc-167">Template example using JSON</span></span>
<span data-ttu-id="887bc-168">Det här exemplet skickar en avisering för en [mallen registrering](../notification-hubs/notification-hubs-templates-cross-platform-push-messages.md) som innehåller en `message` platshållare i mallen med hjälp av en giltig JSON-sträng.</span><span class="sxs-lookup"><span data-stu-id="887bc-168">This example sends a notification for a [template registration](../notification-hubs/notification-hubs-templates-cross-platform-push-messages.md) that contains a `message` place holder in the template using a valid JSON string.</span></span>

```cs
using System;

public static void Run(string myQueueItem,  out string notification, TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {myQueueItem}");
    notification = "{\"message\":\"Hello from C#. Processed a queue item!\"}";
}
```

## <a name="template-example-using-notification-hubs-library-types"></a><span data-ttu-id="887bc-169">Exempel med Notification Hubs bibliotekstyper</span><span class="sxs-lookup"><span data-stu-id="887bc-169">Template example using Notification Hubs library types</span></span>
<span data-ttu-id="887bc-170">Det här exemplet visar hur du använder typer som definieras i den [Microsoft Azure Notification Hubs Library](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/).</span><span class="sxs-lookup"><span data-stu-id="887bc-170">This example shows how to use types defined in the [Microsoft Azure Notification Hubs Library](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/).</span></span> 

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

## <a name="next-steps"></a><span data-ttu-id="887bc-171">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="887bc-171">Next steps</span></span>
[!INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)]

