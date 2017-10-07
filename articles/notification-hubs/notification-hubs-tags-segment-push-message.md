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
# <a name="routing-and-tag-expressions"></a><span data-ttu-id="a6ea2-103">Routning och tagg uttryck</span><span class="sxs-lookup"><span data-stu-id="a6ea2-103">Routing and tag expressions</span></span>
## <a name="overview"></a><span data-ttu-id="a6ea2-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="a6ea2-104">Overview</span></span>
<span data-ttu-id="a6ea2-105">Tagguttryck aktivera tootarget vissa uppsättningar av enheter eller mer specifikt registreringar när du skickar ett push-meddelande via Notification Hubs.</span><span class="sxs-lookup"><span data-stu-id="a6ea2-105">Tag expressions enable you tootarget specific sets of devices, or more specifically registrations, when sending a push notification through Notification Hubs.</span></span>

## <a name="targeting-specific-registrations"></a><span data-ttu-id="a6ea2-106">Målobjekt för specifika registreringar</span><span class="sxs-lookup"><span data-stu-id="a6ea2-106">Targeting specific registrations</span></span>
<span data-ttu-id="a6ea2-107">hello endast sätt tootarget specifik avisering registreringar är tooassociate taggar med dem, rikta taggarna.</span><span class="sxs-lookup"><span data-stu-id="a6ea2-107">hello only way tootarget specific notification registrations is tooassociate tags with them, then target those tags.</span></span> <span data-ttu-id="a6ea2-108">Enligt beskrivningen i [registrering Management](notification-hubs-push-notification-registration-management.md), i ordning tooreceive push hantera meddelanden som en app har tooregister en enhet på en meddelandehubb.</span><span class="sxs-lookup"><span data-stu-id="a6ea2-108">As discussed in [Registration Management](notification-hubs-push-notification-registration-management.md), in order tooreceive push notifications an app has tooregister a device handle on a notification hub.</span></span> <span data-ttu-id="a6ea2-109">När du har skapat en registrering på en meddelandehubb kan hello programmet serverdel skicka push-meddelanden tooit.</span><span class="sxs-lookup"><span data-stu-id="a6ea2-109">Once a registration is created on a notification hub, hello application backend can send push notifications tooit.</span></span>
<span data-ttu-id="a6ea2-110">hello programmet backend kan välja hello registreringar tootarget med ett visst meddelande i hello följande sätt:</span><span class="sxs-lookup"><span data-stu-id="a6ea2-110">hello application backend can choose hello registrations tootarget with a specific notification in hello following ways:</span></span>

1. <span data-ttu-id="a6ea2-111">**Broadcast**: alla registreringar i hello meddelandehubben får hello-meddelande.</span><span class="sxs-lookup"><span data-stu-id="a6ea2-111">**Broadcast**: all registrations in hello notification hub receive hello notification.</span></span>
2. <span data-ttu-id="a6ea2-112">**Taggen**: alla registreringar som innehåller hello angivna taggen får hello-meddelande.</span><span class="sxs-lookup"><span data-stu-id="a6ea2-112">**Tag**: all registrations that contain hello specified tag receive hello notification.</span></span>
3. <span data-ttu-id="a6ea2-113">**Tagga uttryck**: alla registreringar vars uppsättning taggar matchar hello angivna uttrycket får hello-meddelande.</span><span class="sxs-lookup"><span data-stu-id="a6ea2-113">**Tag expression**: all registrations whose set of tags match hello specified expression receive hello notification.</span></span>

## <a name="tags"></a><span data-ttu-id="a6ea2-114">Taggar</span><span class="sxs-lookup"><span data-stu-id="a6ea2-114">Tags</span></span>
<span data-ttu-id="a6ea2-115">En tagg kan vara valfri sträng in too120 tecken, som innehåller alfanumeriska och hello följande icke-alfanumeriska tecken: '_' ' @', '#', '. ',':', '-'.</span><span class="sxs-lookup"><span data-stu-id="a6ea2-115">A tag can be any string, up too120 characters, containing alphanumeric and hello following non-alphanumeric characters: ‘_’, ‘@’, ‘#’, ‘.’, ‘:’, ‘-’.</span></span> <span data-ttu-id="a6ea2-116">hello visas följande exempel ett program som du kan ta emot popup-meddelanden om särskilda musik grupper.</span><span class="sxs-lookup"><span data-stu-id="a6ea2-116">hello following example shows an application from which you can receive toast notifications about specific music groups.</span></span> <span data-ttu-id="a6ea2-117">I det här scenariot är ett enkelt sätt tooroute meddelanden toolabel registreringar med taggar som representerar hello olika band, som i följande bild hello.</span><span class="sxs-lookup"><span data-stu-id="a6ea2-117">In this scenario, a simple way tooroute notifications is toolabel registrations with tags that represent hello different bands, as in hello following picture.</span></span>

![](./media/notification-hubs-routing-tag-expressions/notification-hubs-tags.png)

<span data-ttu-id="a6ea2-118">I den här bilden hello-meddelande märkta **Beatles** når endast hello tablet som registrerats med hello tagg **Beatles**.</span><span class="sxs-lookup"><span data-stu-id="a6ea2-118">In this picture, hello message tagged **Beatles** reaches only hello tablet that registered with hello tag **Beatles**.</span></span>

<span data-ttu-id="a6ea2-119">Mer information om hur du skapar registreringar för taggar finns [registrering Management](notification-hubs-push-notification-registration-management.md).</span><span class="sxs-lookup"><span data-stu-id="a6ea2-119">For more information about creating registrations for tags, see [Registration Management](notification-hubs-push-notification-registration-management.md).</span></span>

<span data-ttu-id="a6ea2-120">Du kan skicka meddelanden tootags med hello skicka meddelanden metoder för hello `Microsoft.Azure.NotificationHubs.NotificationHubClient` klassen i hello [Microsoft Azure Notification Hub](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/) SDK.</span><span class="sxs-lookup"><span data-stu-id="a6ea2-120">You can send notifications tootags using hello send notifications methods of hello `Microsoft.Azure.NotificationHubs.NotificationHubClient` class in hello [Microsoft Azure Notification Hubs](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/) SDK.</span></span> <span data-ttu-id="a6ea2-121">Du kan också använda Node.js eller hello Push-meddelanden REST API: er.</span><span class="sxs-lookup"><span data-stu-id="a6ea2-121">You can also use Node.js, or hello Push Notifications REST APIs.</span></span>  <span data-ttu-id="a6ea2-122">Här är ett exempel på användning av hello SDK.</span><span class="sxs-lookup"><span data-stu-id="a6ea2-122">Here's an example using hello SDK.</span></span>

    Microsoft.Azure.NotificationHubs.NotificationOutcome outcome = null;

    // Windows 8.1 / Windows Phone 8.1
    var toast = @"<toast><visual><binding template=""ToastText01""><text id=""1"">" +
    "You requested a Beatles notification</text></binding></visual></toast>";
    outcome = await Notifications.Instance.Hub.SendWindowsNativeNotificationAsync(toast, "Beatles");

    // Windows 10
    toast = @"<toast><visual><binding template=""ToastGeneric""><text id=""1"">" +
    "You requested a Wailers notification</text></binding></visual></toast>";
    outcome = await Notifications.Instance.Hub.SendWindowsNativeNotificationAsync(toast, "Wailers");




<span data-ttu-id="a6ea2-123">Taggar har inte toobe etableras i förväg och kan referera toomultiple app-specifik begrepp.</span><span class="sxs-lookup"><span data-stu-id="a6ea2-123">Tags do not have toobe pre-provisioned and can refer toomultiple app-specific concepts.</span></span> <span data-ttu-id="a6ea2-124">Till exempel användare med det här exempelprogrammet kommentera band och vill tooreceive toasts, inte bara för hello kommentarer om deras favorit band, utan även för alla kommentarer från sina vänner oavsett hello band där de kommentarer.</span><span class="sxs-lookup"><span data-stu-id="a6ea2-124">For example, users of this example application can comment on bands and want tooreceive toasts, not only for hello comments on their favorite bands, but also for all comments from their friends, regardless of hello band on which they are commenting.</span></span> <span data-ttu-id="a6ea2-125">hello följande bild visar ett exempel på det här scenariot:</span><span class="sxs-lookup"><span data-stu-id="a6ea2-125">hello following picture shows an example of this scenario:</span></span>

![](./media/notification-hubs-routing-tag-expressions/notification-hubs-tags2.png)

<span data-ttu-id="a6ea2-126">I den här bilden Alice är intresserad av uppdateringar för hello Beatles och Bob är intresserad av uppdateringar för hello Wailers.</span><span class="sxs-lookup"><span data-stu-id="a6ea2-126">In this picture, Alice is interested in updates for hello Beatles, and Bob is interested in updates for hello Wailers.</span></span> <span data-ttu-id="a6ea2-127">Bob är också intresserat Charlies kommentarer och Charlie är intresserad av hello Wailers.</span><span class="sxs-lookup"><span data-stu-id="a6ea2-127">Bob is also interested in Charlie’s comments, and Charlie is in interested in hello Wailers.</span></span> <span data-ttu-id="a6ea2-128">När ett meddelande skickas för Charlies kommentera hello Beatles, får både Alice och Bob den.</span><span class="sxs-lookup"><span data-stu-id="a6ea2-128">When a notification is sent for Charlie’s comment on hello Beatles, both Alice and Bob receive it.</span></span>

<span data-ttu-id="a6ea2-129">Medan du kan koda flera frågor i taggar (till exempel ”band_Beatles” eller ”follows_Charlie”), är taggar enkla strängar och inte egenskaper med värden.</span><span class="sxs-lookup"><span data-stu-id="a6ea2-129">While you can encode multiple concerns in tags (for example, “band_Beatles” or “follows_Charlie”), tags are simple strings and not properties with values.</span></span> <span data-ttu-id="a6ea2-130">En registrering matchas endast på hello närvaron eller frånvaron av en viss tagg.</span><span class="sxs-lookup"><span data-stu-id="a6ea2-130">A registration is matched only on hello presence or absence of a specific tag.</span></span>

<span data-ttu-id="a6ea2-131">En fullständig stegvis självstudiekurs om hur toouse taggar för att skicka toointerest grupper finns [bryter nyheter](notification-hubs-windows-notification-dotnet-push-xplat-segmented-wns.md).</span><span class="sxs-lookup"><span data-stu-id="a6ea2-131">For a full step-by-step tutorial on how toouse tags for sending toointerest groups, see [Breaking News](notification-hubs-windows-notification-dotnet-push-xplat-segmented-wns.md).</span></span>

## <a name="using-tags-tootarget-users"></a><span data-ttu-id="a6ea2-132">Med hjälp av taggar tootarget användare</span><span class="sxs-lookup"><span data-stu-id="a6ea2-132">Using tags tootarget users</span></span>
<span data-ttu-id="a6ea2-133">Ett annat sätt toouse taggar är tooidentify alla hello enheter av en viss användare.</span><span class="sxs-lookup"><span data-stu-id="a6ea2-133">Another way toouse tags is tooidentify all hello devices of a particular user.</span></span> <span data-ttu-id="a6ea2-134">Registreringar taggas med en tagg som innehåller användar-id, som i följande bild hello:</span><span class="sxs-lookup"><span data-stu-id="a6ea2-134">Registrations can be tagged with a tag that contains a user id, as in hello following picture:</span></span>

![](./media/notification-hubs-routing-tag-expressions/notification-hubs-tags3.png)

<span data-ttu-id="a6ea2-135">I den här bilden når hello meddelandet märkta uid:Alice alla registreringar taggade uid:Alice; Därför måste alla Annas enheter.</span><span class="sxs-lookup"><span data-stu-id="a6ea2-135">In this picture, hello message tagged uid:Alice reaches all registrations tagged uid:Alice; hence, all of Alice’s devices.</span></span>

## <a name="tag-expressions"></a><span data-ttu-id="a6ea2-136">Tagguttryck</span><span class="sxs-lookup"><span data-stu-id="a6ea2-136">Tag expressions</span></span>
<span data-ttu-id="a6ea2-137">Finns det fall där en avisering har tootarget en uppsättning registreringar som identifieras inte som en enskild tagg, men av ett booleskt uttryck på taggar.</span><span class="sxs-lookup"><span data-stu-id="a6ea2-137">There are cases in which a notification has tootarget a set of registrations that is identified not by a single tag, but by a Boolean expression on tags.</span></span>

<span data-ttu-id="a6ea2-138">Överväg ett sport-program som skickar en påminnelse tooeveryone i Boston om spel mellan hello Red Sox och Cardinals.</span><span class="sxs-lookup"><span data-stu-id="a6ea2-138">Consider a sports application that sends a reminder tooeveryone in Boston about a game between hello Red Sox and Cardinals.</span></span> <span data-ttu-id="a6ea2-139">Om hello-klientappen registrerar taggar om intresse för team och plats, ska hello-meddelande vara riktad tooeveryone i Boston som är intresserade hello Red Sox eller hello Cardinals.</span><span class="sxs-lookup"><span data-stu-id="a6ea2-139">If hello client app registers tags about interest in teams and location, then hello notification should be targeted tooeveryone in Boston who is interested in either hello Red Sox or hello Cardinals.</span></span> <span data-ttu-id="a6ea2-140">Det här tillståndet kan uttryckas med hello följande booleskt uttryck:</span><span class="sxs-lookup"><span data-stu-id="a6ea2-140">This condition can be expressed with hello following Boolean expression:</span></span>

    (follows_RedSox || follows_Cardinals) && location_Boston


![](./media/notification-hubs-routing-tag-expressions/notification-hubs-tags4.png)

<span data-ttu-id="a6ea2-141">Tagguttryck kan innehålla alla booleska operatorer som och (& &), eller (|), och inte (!).</span><span class="sxs-lookup"><span data-stu-id="a6ea2-141">Tag expressions can contain all Boolean operators, such as AND (&&), OR (||), and NOT (!).</span></span> <span data-ttu-id="a6ea2-142">De kan också innehålla parenteser.</span><span class="sxs-lookup"><span data-stu-id="a6ea2-142">They can also contain parentheses.</span></span> <span data-ttu-id="a6ea2-143">Tagguttryck är begränsad too20 taggar om de innehåller endast ORs; Annars är de begränsade too6 taggar.</span><span class="sxs-lookup"><span data-stu-id="a6ea2-143">Tag expressions are limited too20 tags if they contain only ORs; otherwise they are limited too6 tags.</span></span>

<span data-ttu-id="a6ea2-144">Här är ett exempel för att skicka meddelanden med Tagguttryck med hello SDK.</span><span class="sxs-lookup"><span data-stu-id="a6ea2-144">Here's an example for sending notifications with tag expressions using hello SDK.</span></span>

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
