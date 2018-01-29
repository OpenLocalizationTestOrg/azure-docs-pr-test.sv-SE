---
title: Routning och Tagguttryck
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
ms.openlocfilehash: 18faa88641623e1248d6a33bc2d87099e1c9f624
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/11/2017
---
# <a name="routing-and-tag-expressions"></a>Routning och tagg uttryck
## <a name="overview"></a>Översikt
Tagguttryck kan du mål speciella grupper av enheter eller mer specifikt registreringar när du skickar ett push-meddelande via Notification Hubs.

## <a name="targeting-specific-registrations"></a>Målobjekt för specifika registreringar
Det enda sättet att målet specifik avisering registreringar är att associera taggar med dem, rikta taggarna. Enligt beskrivningen i [registrering Management](notification-hubs-push-notification-registration-management.md)för att ta emot push-meddelanden som en app måste registrera en enhet som hanteras på en meddelandehubb. När en registrering skapas på en meddelandehubb kan serverdelen program skicka push-meddelanden till den.
Serverdelen program kan välja registreringar till målet med ett visst meddelande på följande sätt:

1. **Broadcast**: alla registreringar i meddelandehubben ta emot meddelandet.
2. **Taggen**: alla registreringar som innehåller den angivna taggen ta emot meddelandet.
3. **Tagga uttryck**: alla registreringar vars uppsättning taggar som matchar det angivna uttrycket ta emot meddelandet.

## <a name="tags"></a>Taggar
En tagg kan vara valfri sträng som innehåller alfanumeriska upp till 120 tecken och följande icke-alfanumeriska tecken: '_' ' @', '#', '. ',':', '-'. I följande exempel visas ett program som du kan ta emot popup-meddelanden om särskilda musik grupper. I det här scenariot är ett enkelt sätt att vägen meddelanden till etiketten registreringar med taggar som representerar olika banden, enligt följande bild.

![](./media/notification-hubs-routing-tag-expressions/notification-hubs-tags.png)

I den här bilden meddelandet märkta **Beatles** når tablet som registrerats med taggen **Beatles**.

Mer information om hur du skapar registreringar för taggar finns [registrering Management](notification-hubs-push-notification-registration-management.md).

Du kan skicka meddelanden till taggar skicka meddelanden metoder för den `Microsoft.Azure.NotificationHubs.NotificationHubClient` klassen i den [Microsoft Azure Notification Hub](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/) SDK. Du kan också använda Node.js eller Push-meddelanden REST API: erna.  Här är ett exempel med hjälp av SDK.

    Microsoft.Azure.NotificationHubs.NotificationOutcome outcome = null;

    // Windows 8.1 / Windows Phone 8.1
    var toast = @"<toast><visual><binding template=""ToastText01""><text id=""1"">" +
    "You requested a Beatles notification</text></binding></visual></toast>";
    outcome = await Notifications.Instance.Hub.SendWindowsNativeNotificationAsync(toast, "Beatles");

    // Windows 10
    toast = @"<toast><visual><binding template=""ToastGeneric""><text id=""1"">" +
    "You requested a Wailers notification</text></binding></visual></toast>";
    outcome = await Notifications.Instance.Hub.SendWindowsNativeNotificationAsync(toast, "Wailers");




Taggar behöver inte vara företablerad och kan referera till flera app-specifik begrepp. Till exempel användare med det här exempelprogrammet kommentera band och vill få toasts, inte bara för kommentarer om deras favorit band, utan även för alla kommentarer från sina vänner oavsett bandet som de kommentarer. Följande bild visar ett exempel på det här scenariot:

![](./media/notification-hubs-routing-tag-expressions/notification-hubs-tags2.png)

I den här bilden Alice är intresserad av uppdateringar för Beatles och Bob är intresserad av uppdateringar för Wailers. Bob är också intresserat Charlies kommentarer och Charlie är intresserad av Wailers. När ett meddelande skickas för Charlies kommentera Beatles, får både Alice och Bob den.

Medan du kan koda flera frågor i taggar (till exempel ”band_Beatles” eller ”follows_Charlie”), är taggar enkla strängar och inte egenskaper med värden. En registrering matchas endast på närvaron eller frånvaron av en viss tagg.

En fullständig stegvis självstudiekurs om hur du använder taggar för att skicka till intressegrupper finns [bryter nyheter](notification-hubs-windows-notification-dotnet-push-xplat-segmented-wns.md).

## <a name="using-tags-to-target-users"></a>Med hjälp av taggar till målgruppsanvändare
Ett annat sätt att använda taggar är att identifiera alla enheter av en viss användare. Registreringar taggas med en tagg som innehåller användar-id, som i följande bild:

![](./media/notification-hubs-routing-tag-expressions/notification-hubs-tags3.png)

I den här bilden når meddelandet märkta uid:Alice alla registreringar taggade uid:Alice; Därför måste alla Annas enheter.

## <a name="tag-expressions"></a>Tagguttryck
Finns det fall där en avisering har som mål en uppsättning registreringar som identifieras inte som en enskild tagg, men av ett booleskt uttryck på taggar.

Överväg ett sport-program som skickar en påminnelse för alla i Boston om spel mellan Red Sox och Cardinals. Om klientappen registrerar taggar om intresse för team och plats, vara meddelandet mål för alla i Boston som är intresserade Red Sox eller Cardinals. Det här tillståndet kan uttryckas med följande booleskt uttryck:

    (follows_RedSox || follows_Cardinals) && location_Boston


![](./media/notification-hubs-routing-tag-expressions/notification-hubs-tags4.png)

Tagguttryck kan innehålla alla booleska operatorer som och (& &), eller (|), och inte (!). De kan också innehålla parenteser. Tagguttryck begränsas till 20 taggar om de innehåller endast ORs; Annars är de begränsade till 6 taggar.

Här är ett exempel för att skicka meddelanden med Tagguttryck med SDK.

    Microsoft.Azure.NotificationHubs.NotificationOutcome outcome = null;

    String userTag = "(location_Boston && !follows_Cardinals)";    

    // Windows 8.1 / Windows Phone 8.1
    var toast = @"<toast><visual><binding template=""ToastText01""><text id=""1"">" +
    "You want info on the Red Socks</text></binding></visual></toast>";
    outcome = await Notifications.Instance.Hub.SendWindowsNativeNotificationAsync(toast, userTag);

    // Windows 10
    toast = @"<toast><visual><binding template=""ToastGeneric""><text id=""1"">" +
    "You want info on the Red Socks</text></binding></visual></toast>";
    outcome = await Notifications.Instance.Hub.SendWindowsNativeNotificationAsync(toast, userTag);
