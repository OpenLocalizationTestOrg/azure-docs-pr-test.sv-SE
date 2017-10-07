---
title: aaaHow toosend schemalagda meddelanden | Microsoft Docs
description: "Det här avsnittet beskriver hur du använder schemalagda meddelanden med Azure Notification Hubs."
services: notification-hubs
documentationcenter: .net
keywords: "push-meddelanden, push-meddelande, schemalägga push-meddelanden"
author: ysxu
manager: erikre
editor: 
ms.assetid: 6b718c75-75dd-4c99-aee3-db1288235c1a
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-android
ms.devlang: dotnet
ms.topic: article
ms.date: 06/29/2016
ms.author: yuaxu
ms.openlocfilehash: 9b3ba715dad6f5d824a615e83f2863b0db47b533
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-to-send-scheduled-notifications"></a><span data-ttu-id="9f206-104">Så här: Skicka schemalagda meddelanden</span><span class="sxs-lookup"><span data-stu-id="9f206-104">How To: Send scheduled notifications</span></span>
## <a name="overview"></a><span data-ttu-id="9f206-105">Översikt</span><span class="sxs-lookup"><span data-stu-id="9f206-105">Overview</span></span>
<span data-ttu-id="9f206-106">Om du har ett scenario där du vill toosend ett meddelande på någon punkt i hello framtida, men inte har ett enkelt sätt toowake in serverdelskoden toosend hello-meddelande.</span><span class="sxs-lookup"><span data-stu-id="9f206-106">If you have a scenario in which you want toosend a notification at some point in hello future, but do not have an easy way toowake up your back-end code toosend hello notification.</span></span> <span data-ttu-id="9f206-107">Standardnivån Notification Hubs stöder en funktion som gör att du tooschedule meddelanden in too7 dagar i hello framtida.</span><span class="sxs-lookup"><span data-stu-id="9f206-107">Standard tier Notification Hubs supports a feature that enables you tooschedule notifications up too7 days in hello future.</span></span>

<span data-ttu-id="9f206-108">När du skickar ett meddelande kan bara använda hello [ScheduledNotification](https://msdn.microsoft.com/library/microsoft.azure.notificationhubs.schedulednotification.aspx) klassen i hello Notification Hubs SDK som visas i följande exempel hello:</span><span class="sxs-lookup"><span data-stu-id="9f206-108">When sending a notification, simply use hello [ScheduledNotification](https://msdn.microsoft.com/library/microsoft.azure.notificationhubs.schedulednotification.aspx) class in hello Notification Hubs SDK as shown in hello following example:</span></span>

    Notification notification = new AppleNotification("{\"aps\":{\"alert\":\"Happy birthday!\"}}");
    var scheduled = await hub.ScheduleNotificationAsync(notification, new DateTime(2014, 7, 19, 0, 0, 0));

<span data-ttu-id="9f206-109">Du kan också avbryta en tidigare schemalagda meddelanden med dess notificationId:</span><span class="sxs-lookup"><span data-stu-id="9f206-109">Also, you can cancel a previously scheduled notification using its notificationId:</span></span>

    await hub.CancelNotificationAsync(scheduled.ScheduledNotificationId);

<span data-ttu-id="9f206-110">Det finns ingen begränsning för hello antalet schemalagda meddelanden som du kan skicka.</span><span class="sxs-lookup"><span data-stu-id="9f206-110">There are no limits on hello number of scheduled notifications you can send.</span></span>

