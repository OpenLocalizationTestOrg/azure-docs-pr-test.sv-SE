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
# <a name="how-to-send-scheduled-notifications"></a>Så här: Skicka schemalagda meddelanden
## <a name="overview"></a>Översikt
Om du har ett scenario där du vill toosend ett meddelande på någon punkt i hello framtida, men inte har ett enkelt sätt toowake in serverdelskoden toosend hello-meddelande. Standardnivån Notification Hubs stöder en funktion som gör att du tooschedule meddelanden in too7 dagar i hello framtida.

När du skickar ett meddelande kan bara använda hello [ScheduledNotification](https://msdn.microsoft.com/library/microsoft.azure.notificationhubs.schedulednotification.aspx) klassen i hello Notification Hubs SDK som visas i följande exempel hello:

    Notification notification = new AppleNotification("{\"aps\":{\"alert\":\"Happy birthday!\"}}");
    var scheduled = await hub.ScheduleNotificationAsync(notification, new DateTime(2014, 7, 19, 0, 0, 0));

Du kan också avbryta en tidigare schemalagda meddelanden med dess notificationId:

    await hub.CancelNotificationAsync(scheduled.ScheduledNotificationId);

Det finns ingen begränsning för hello antalet schemalagda meddelanden som du kan skicka.

