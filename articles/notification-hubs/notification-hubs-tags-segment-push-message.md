---
title: aaaRouting och Tagguttryck
description: "Det här avsnittet beskrivs uttryck för Routning och tagg i Azure notification hubs."
services: notification-hubs
documentationcenter: .net
author: ysxu
manager: erikre
editor: 
ms.assetid: 0fffb3bb-8ed8-4e0f-89e8-0de24a47f644
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-multiple
ms.devlang: dotnet
ms.topic: article
ms.date: 06/29/2016
ms.author: yuaxu
ms.openlocfilehash: c2c60500f7469f1cb1a73a5cf63c221a9ad6cbb4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="routing-and-tag-expressions"></a>Routning och tagg uttryck
## <a name="overview"></a>Översikt
Tagguttryck aktivera tootarget vissa uppsättningar av enheter eller mer specifikt registreringar när du skickar ett push-meddelande via Notification Hubs.

## <a name="targeting-specific-registrations"></a>Målobjekt för specifika registreringar
hello endast sätt tootarget specifik avisering registreringar är tooassociate taggar med dem, rikta taggarna. Enligt beskrivningen i [registrering Management](notification-hubs-push-notification-registration-management.md), i ordning tooreceive push hantera meddelanden som en app har tooregister en enhet på en meddelandehubb. När du har skapat en registrering på en meddelandehubb kan hello programmet serverdel skicka push-meddelanden tooit.
hello programmet backend kan välja hello registreringar tootarget med ett visst meddelande i hello följande sätt:

1. **Broadcast**: alla registreringar i hello meddelandehubben får hello-meddelande.
2. **Taggen**: alla registreringar som innehåller hello angivna taggen får hello-meddelande.
3. **Tagga uttryck**: alla registreringar vars uppsättning taggar matchar hello angivna uttrycket får hello-meddelande.

## <a name="tags"></a>Taggar
En tagg kan vara valfri sträng in too120 tecken, som innehåller alfanumeriska och hello följande icke-alfanumeriska tecken: '_' ' @', '#', '. ',':', '-'. hello visas följande exempel ett program som du kan ta emot popup-meddelanden om särskilda musik grupper. I det här scenariot är ett enkelt sätt tooroute meddelanden toolabel registreringar med taggar som representerar hello olika band, som i följande bild hello.

![](./media/notification-hubs-routing-tag-expressions/notification-hubs-tags.png)

I den här bilden hello-meddelande märkta **Beatles** når endast hello tablet som registrerats med hello tagg **Beatles**.

Mer information om hur du skapar registreringar för taggar finns [registrering Management](notification-hubs-push-notification-registration-management.md).

Du kan skicka meddelanden tootags med hello skicka meddelanden metoder för hello `Microsoft.Azure.NotificationHubs.NotificationHubClient` klassen i hello [Microsoft Azure Notification Hub](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/) SDK. Du kan också använda Node.js eller hello Push-meddelanden REST API: er.  Här är ett exempel på användning av hello SDK.

    Microsoft.Azure.NotificationHubs.NotificationOutcome outcome = null;

    // Windows 8.1 / Windows Phone 8.1
    var toast = @"<toast><visual><binding template=""ToastText01""><text id=""1"">" +
    "You requested a Beatles notification</text></binding></visual></toast>";
    outcome = await Notifications.Instance.Hub.SendWindowsNativeNotificationAsync(toast, "Beatles");

    // Windows 10
    toast = @"<toast><visual><binding template=""ToastGeneric""><text id=""1"">" +
    "You requested a Wailers notification</text></binding></visual></toast>";
    outcome = await Notifications.Instance.Hub.SendWindowsNativeNotificationAsync(toast, "Wailers");




Taggar har inte toobe etableras i förväg och kan referera toomultiple app-specifik begrepp. Till exempel användare med det här exempelprogrammet kommentera band och vill tooreceive toasts, inte bara för hello kommentarer om deras favorit band, utan även för alla kommentarer från sina vänner oavsett hello band där de kommentarer. hello följande bild visar ett exempel på det här scenariot:

![](./media/notification-hubs-routing-tag-expressions/notification-hubs-tags2.png)

I den här bilden Alice är intresserad av uppdateringar för hello Beatles och Bob är intresserad av uppdateringar för hello Wailers. Bob är också intresserat Charlies kommentarer och Charlie är intresserad av hello Wailers. När ett meddelande skickas för Charlies kommentera hello Beatles, får både Alice och Bob den.

Medan du kan koda flera frågor i taggar (till exempel ”band_Beatles” eller ”follows_Charlie”), är taggar enkla strängar och inte egenskaper med värden. En registrering matchas endast på hello närvaron eller frånvaron av en viss tagg.

En fullständig stegvis självstudiekurs om hur toouse taggar för att skicka toointerest grupper finns [bryter nyheter](notification-hubs-windows-notification-dotnet-push-xplat-segmented-wns.md).

## <a name="using-tags-tootarget-users"></a>Med hjälp av taggar tootarget användare
Ett annat sätt toouse taggar är tooidentify alla hello enheter av en viss användare. Registreringar taggas med en tagg som innehåller användar-id, som i följande bild hello:

![](./media/notification-hubs-routing-tag-expressions/notification-hubs-tags3.png)

I den här bilden når hello meddelandet märkta uid:Alice alla registreringar taggade uid:Alice; Därför måste alla Annas enheter.

## <a name="tag-expressions"></a>Tagguttryck
Finns det fall där en avisering har tootarget en uppsättning registreringar som identifieras inte som en enskild tagg, men av ett booleskt uttryck på taggar.

Överväg ett sport-program som skickar en påminnelse tooeveryone i Boston om spel mellan hello Red Sox och Cardinals. Om hello-klientappen registrerar taggar om intresse för team och plats, ska hello-meddelande vara riktad tooeveryone i Boston som är intresserade hello Red Sox eller hello Cardinals. Det här tillståndet kan uttryckas med hello följande booleskt uttryck:

    (follows_RedSox || follows_Cardinals) && location_Boston


![](./media/notification-hubs-routing-tag-expressions/notification-hubs-tags4.png)

Tagguttryck kan innehålla alla booleska operatorer som och (& &), eller (|), och inte (!). De kan också innehålla parenteser. Tagguttryck är begränsad too20 taggar om de innehåller endast ORs; Annars är de begränsade too6 taggar.

Här är ett exempel för att skicka meddelanden med Tagguttryck med hello SDK.

    Microsoft.Azure.NotificationHubs.NotificationOutcome outcome = null;

    String userTag = "(location_Boston && !follows_Cardinals)";    

    // Windows 8.1 / Windows Phone 8.1
    var toast = @"<toast><visual><binding template=""ToastText01""><text id=""1"">" +
    "You want info on hello Red Socks</text></binding></visual></toast>";
    outcome = await Notifications.Instance.Hub.SendWindowsNativeNotificationAsync(toast, userTag);

    // Windows 10
    toast = @"<toast><visual><binding template=""ToastGeneric""><text id=""1"">" +
    "You want info on hello Red Socks</text></binding></visual></toast>";
    outcome = await Notifications.Instance.Hub.SendWindowsNativeNotificationAsync(toast, userTag);
