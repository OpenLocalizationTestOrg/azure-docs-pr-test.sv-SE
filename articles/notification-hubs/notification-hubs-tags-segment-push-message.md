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
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="routing-and-tag-expressions"></a><span data-ttu-id="8036d-103">Routning och tagg uttryck</span><span class="sxs-lookup"><span data-stu-id="8036d-103">Routing and tag expressions</span></span>
## <a name="overview"></a><span data-ttu-id="8036d-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="8036d-104">Overview</span></span>
<span data-ttu-id="8036d-105">Tagguttryck kan du mål speciella grupper av enheter eller mer specifikt registreringar när du skickar ett push-meddelande via Notification Hubs.</span><span class="sxs-lookup"><span data-stu-id="8036d-105">Tag expressions enable you to target specific sets of devices, or more specifically registrations, when sending a push notification through Notification Hubs.</span></span>

## <a name="targeting-specific-registrations"></a><span data-ttu-id="8036d-106">Målobjekt för specifika registreringar</span><span class="sxs-lookup"><span data-stu-id="8036d-106">Targeting specific registrations</span></span>
<span data-ttu-id="8036d-107">Det enda sättet att målet specifik avisering registreringar är att associera taggar med dem, rikta taggarna.</span><span class="sxs-lookup"><span data-stu-id="8036d-107">The only way to target specific notification registrations is to associate tags with them, then target those tags.</span></span> <span data-ttu-id="8036d-108">Enligt beskrivningen i [registrering Management](notification-hubs-push-notification-registration-management.md)för att ta emot push-meddelanden som en app måste registrera en enhet som hanteras på en meddelandehubb.</span><span class="sxs-lookup"><span data-stu-id="8036d-108">As discussed in [Registration Management](notification-hubs-push-notification-registration-management.md), in order to receive push notifications an app has to register a device handle on a notification hub.</span></span> <span data-ttu-id="8036d-109">När en registrering skapas på en meddelandehubb kan serverdelen program skicka push-meddelanden till den.</span><span class="sxs-lookup"><span data-stu-id="8036d-109">Once a registration is created on a notification hub, the application backend can send push notifications to it.</span></span>
<span data-ttu-id="8036d-110">Serverdelen program kan välja registreringar till målet med ett visst meddelande på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="8036d-110">The application backend can choose the registrations to target with a specific notification in the following ways:</span></span>

1. <span data-ttu-id="8036d-111">**Broadcast**: alla registreringar i meddelandehubben ta emot meddelandet.</span><span class="sxs-lookup"><span data-stu-id="8036d-111">**Broadcast**: all registrations in the notification hub receive the notification.</span></span>
2. <span data-ttu-id="8036d-112">**Taggen**: alla registreringar som innehåller den angivna taggen ta emot meddelandet.</span><span class="sxs-lookup"><span data-stu-id="8036d-112">**Tag**: all registrations that contain the specified tag receive the notification.</span></span>
3. <span data-ttu-id="8036d-113">**Tagga uttryck**: alla registreringar vars uppsättning taggar som matchar det angivna uttrycket ta emot meddelandet.</span><span class="sxs-lookup"><span data-stu-id="8036d-113">**Tag expression**: all registrations whose set of tags match the specified expression receive the notification.</span></span>

## <a name="tags"></a><span data-ttu-id="8036d-114">Taggar</span><span class="sxs-lookup"><span data-stu-id="8036d-114">Tags</span></span>
<span data-ttu-id="8036d-115">En tagg kan vara valfri sträng som innehåller alfanumeriska upp till 120 tecken och följande icke-alfanumeriska tecken: '_' ' @', '#', '. ',':', '-'.</span><span class="sxs-lookup"><span data-stu-id="8036d-115">A tag can be any string, up to 120 characters, containing alphanumeric and the following non-alphanumeric characters: ‘_’, ‘@’, ‘#’, ‘.’, ‘:’, ‘-’.</span></span> <span data-ttu-id="8036d-116">I följande exempel visas ett program som du kan ta emot popup-meddelanden om särskilda musik grupper.</span><span class="sxs-lookup"><span data-stu-id="8036d-116">The following example shows an application from which you can receive toast notifications about specific music groups.</span></span> <span data-ttu-id="8036d-117">I det här scenariot är ett enkelt sätt att vägen meddelanden till etiketten registreringar med taggar som representerar olika banden, enligt följande bild.</span><span class="sxs-lookup"><span data-stu-id="8036d-117">In this scenario, a simple way to route notifications is to label registrations with tags that represent the different bands, as in the following picture.</span></span>

![](./media/notification-hubs-routing-tag-expressions/notification-hubs-tags.png)

<span data-ttu-id="8036d-118">I den här bilden meddelandet märkta **Beatles** når tablet som registrerats med taggen **Beatles**.</span><span class="sxs-lookup"><span data-stu-id="8036d-118">In this picture, the message tagged **Beatles** reaches only the tablet that registered with the tag **Beatles**.</span></span>

<span data-ttu-id="8036d-119">Mer information om hur du skapar registreringar för taggar finns [registrering Management](notification-hubs-push-notification-registration-management.md).</span><span class="sxs-lookup"><span data-stu-id="8036d-119">For more information about creating registrations for tags, see [Registration Management](notification-hubs-push-notification-registration-management.md).</span></span>

<span data-ttu-id="8036d-120">Du kan skicka meddelanden till taggar skicka meddelanden metoder för den `Microsoft.Azure.NotificationHubs.NotificationHubClient` klassen i den [Microsoft Azure Notification Hub](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/) SDK.</span><span class="sxs-lookup"><span data-stu-id="8036d-120">You can send notifications to tags using the send notifications methods of the `Microsoft.Azure.NotificationHubs.NotificationHubClient` class in the [Microsoft Azure Notification Hubs](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/) SDK.</span></span> <span data-ttu-id="8036d-121">Du kan också använda Node.js eller Push-meddelanden REST API: erna.</span><span class="sxs-lookup"><span data-stu-id="8036d-121">You can also use Node.js, or the Push Notifications REST APIs.</span></span>  <span data-ttu-id="8036d-122">Här är ett exempel med hjälp av SDK.</span><span class="sxs-lookup"><span data-stu-id="8036d-122">Here's an example using the SDK.</span></span>

    Microsoft.Azure.NotificationHubs.NotificationOutcome outcome = null;

    // Windows 8.1 / Windows Phone 8.1
    var toast = @"<toast><visual><binding template=""ToastText01""><text id=""1"">" +
    "You requested a Beatles notification</text></binding></visual></toast>";
    outcome = await Notifications.Instance.Hub.SendWindowsNativeNotificationAsync(toast, "Beatles");

    // Windows 10
    toast = @"<toast><visual><binding template=""ToastGeneric""><text id=""1"">" +
    "You requested a Wailers notification</text></binding></visual></toast>";
    outcome = await Notifications.Instance.Hub.SendWindowsNativeNotificationAsync(toast, "Wailers");




<span data-ttu-id="8036d-123">Taggar behöver inte vara företablerad och kan referera till flera app-specifik begrepp.</span><span class="sxs-lookup"><span data-stu-id="8036d-123">Tags do not have to be pre-provisioned and can refer to multiple app-specific concepts.</span></span> <span data-ttu-id="8036d-124">Till exempel användare med det här exempelprogrammet kommentera band och vill få toasts, inte bara för kommentarer om deras favorit band, utan även för alla kommentarer från sina vänner oavsett bandet som de kommentarer.</span><span class="sxs-lookup"><span data-stu-id="8036d-124">For example, users of this example application can comment on bands and want to receive toasts, not only for the comments on their favorite bands, but also for all comments from their friends, regardless of the band on which they are commenting.</span></span> <span data-ttu-id="8036d-125">Följande bild visar ett exempel på det här scenariot:</span><span class="sxs-lookup"><span data-stu-id="8036d-125">The following picture shows an example of this scenario:</span></span>

![](./media/notification-hubs-routing-tag-expressions/notification-hubs-tags2.png)

<span data-ttu-id="8036d-126">I den här bilden Alice är intresserad av uppdateringar för Beatles och Bob är intresserad av uppdateringar för Wailers.</span><span class="sxs-lookup"><span data-stu-id="8036d-126">In this picture, Alice is interested in updates for the Beatles, and Bob is interested in updates for the Wailers.</span></span> <span data-ttu-id="8036d-127">Bob är också intresserat Charlies kommentarer och Charlie är intresserad av Wailers.</span><span class="sxs-lookup"><span data-stu-id="8036d-127">Bob is also interested in Charlie’s comments, and Charlie is in interested in the Wailers.</span></span> <span data-ttu-id="8036d-128">När ett meddelande skickas för Charlies kommentera Beatles, får både Alice och Bob den.</span><span class="sxs-lookup"><span data-stu-id="8036d-128">When a notification is sent for Charlie’s comment on the Beatles, both Alice and Bob receive it.</span></span>

<span data-ttu-id="8036d-129">Medan du kan koda flera frågor i taggar (till exempel ”band_Beatles” eller ”follows_Charlie”), är taggar enkla strängar och inte egenskaper med värden.</span><span class="sxs-lookup"><span data-stu-id="8036d-129">While you can encode multiple concerns in tags (for example, “band_Beatles” or “follows_Charlie”), tags are simple strings and not properties with values.</span></span> <span data-ttu-id="8036d-130">En registrering matchas endast på närvaron eller frånvaron av en viss tagg.</span><span class="sxs-lookup"><span data-stu-id="8036d-130">A registration is matched only on the presence or absence of a specific tag.</span></span>

<span data-ttu-id="8036d-131">En fullständig stegvis självstudiekurs om hur du använder taggar för att skicka till intressegrupper finns [bryter nyheter](notification-hubs-windows-notification-dotnet-push-xplat-segmented-wns.md).</span><span class="sxs-lookup"><span data-stu-id="8036d-131">For a full step-by-step tutorial on how to use tags for sending to interest groups, see [Breaking News](notification-hubs-windows-notification-dotnet-push-xplat-segmented-wns.md).</span></span>

## <a name="using-tags-to-target-users"></a><span data-ttu-id="8036d-132">Med hjälp av taggar till målgruppsanvändare</span><span class="sxs-lookup"><span data-stu-id="8036d-132">Using tags to target users</span></span>
<span data-ttu-id="8036d-133">Ett annat sätt att använda taggar är att identifiera alla enheter av en viss användare.</span><span class="sxs-lookup"><span data-stu-id="8036d-133">Another way to use tags is to identify all the devices of a particular user.</span></span> <span data-ttu-id="8036d-134">Registreringar taggas med en tagg som innehåller användar-id, som i följande bild:</span><span class="sxs-lookup"><span data-stu-id="8036d-134">Registrations can be tagged with a tag that contains a user id, as in the following picture:</span></span>

![](./media/notification-hubs-routing-tag-expressions/notification-hubs-tags3.png)

<span data-ttu-id="8036d-135">I den här bilden når meddelandet märkta uid:Alice alla registreringar taggade uid:Alice; Därför måste alla Annas enheter.</span><span class="sxs-lookup"><span data-stu-id="8036d-135">In this picture, the message tagged uid:Alice reaches all registrations tagged uid:Alice; hence, all of Alice’s devices.</span></span>

## <a name="tag-expressions"></a><span data-ttu-id="8036d-136">Tagguttryck</span><span class="sxs-lookup"><span data-stu-id="8036d-136">Tag expressions</span></span>
<span data-ttu-id="8036d-137">Finns det fall där en avisering har som mål en uppsättning registreringar som identifieras inte som en enskild tagg, men av ett booleskt uttryck på taggar.</span><span class="sxs-lookup"><span data-stu-id="8036d-137">There are cases in which a notification has to target a set of registrations that is identified not by a single tag, but by a Boolean expression on tags.</span></span>

<span data-ttu-id="8036d-138">Överväg ett sport-program som skickar en påminnelse för alla i Boston om spel mellan Red Sox och Cardinals.</span><span class="sxs-lookup"><span data-stu-id="8036d-138">Consider a sports application that sends a reminder to everyone in Boston about a game between the Red Sox and Cardinals.</span></span> <span data-ttu-id="8036d-139">Om klientappen registrerar taggar om intresse för team och plats, vara meddelandet mål för alla i Boston som är intresserade Red Sox eller Cardinals.</span><span class="sxs-lookup"><span data-stu-id="8036d-139">If the client app registers tags about interest in teams and location, then the notification should be targeted to everyone in Boston who is interested in either the Red Sox or the Cardinals.</span></span> <span data-ttu-id="8036d-140">Det här tillståndet kan uttryckas med följande booleskt uttryck:</span><span class="sxs-lookup"><span data-stu-id="8036d-140">This condition can be expressed with the following Boolean expression:</span></span>

    (follows_RedSox || follows_Cardinals) && location_Boston


![](./media/notification-hubs-routing-tag-expressions/notification-hubs-tags4.png)

<span data-ttu-id="8036d-141">Tagguttryck kan innehålla alla booleska operatorer som och (& &), eller (|), och inte (!).</span><span class="sxs-lookup"><span data-stu-id="8036d-141">Tag expressions can contain all Boolean operators, such as AND (&&), OR (||), and NOT (!).</span></span> <span data-ttu-id="8036d-142">De kan också innehålla parenteser.</span><span class="sxs-lookup"><span data-stu-id="8036d-142">They can also contain parentheses.</span></span> <span data-ttu-id="8036d-143">Tagguttryck begränsas till 20 taggar om de innehåller endast ORs; Annars är de begränsade till 6 taggar.</span><span class="sxs-lookup"><span data-stu-id="8036d-143">Tag expressions are limited to 20 tags if they contain only ORs; otherwise they are limited to 6 tags.</span></span>

<span data-ttu-id="8036d-144">Här är ett exempel för att skicka meddelanden med Tagguttryck med SDK.</span><span class="sxs-lookup"><span data-stu-id="8036d-144">Here's an example for sending notifications with tag expressions using the SDK.</span></span>

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
