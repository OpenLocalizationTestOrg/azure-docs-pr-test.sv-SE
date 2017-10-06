---
title: aaaAdd push-meddelanden tooyour Xamarin.Android-app | Microsoft Docs
description: "Lär dig hur toouse Azure App Service och Azure Notification Hubs toosend push-meddelanden tooyour Xamarin.Android-app"
services: app-service\mobile
documentationcenter: xamarin
author: ysxu
manager: syntaxc4
editor: 
ms.assetid: 6f7e8517-e532-4559-9b07-874115f4c65b
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-xamarin-android
ms.devlang: dotnet
ms.topic: article
ms.date: 10/12/2016
ms.author: yuaxu
ms.openlocfilehash: c93d1d0cae06ab15e3e3e5c4b342808b2cd49113
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="add-push-notifications-tooyour-xamarinandroid-app"></a>Lägg till push-meddelanden tooyour Xamarin.Android-app
[!INCLUDE [app-service-mobile-selector-get-started-push](../../includes/app-service-mobile-selector-get-started-push.md)]

## <a name="overview"></a>Översikt
I kursen får du lägga till push-meddelanden toohello [Xamarin.Android Snabbstart](app-service-mobile-windows-store-dotnet-get-started.md) projekt så att ett push-meddelande skickas toohello enheten varje gång en post infogas.

Om du inte använder hello laddat ned Snabbstart serverprojekt och du behöver hello tilläggspaket för push-meddelande. Se [arbeta med serverdelen för hello .NET SDK för Azure Mobile Apps](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md) för mer information.

## <a name="prerequisites"></a>Krav
Den här kursen kräver hello följande:

* Ett aktivt Google-konto. Du kan registrera dig för ett Google-konto på [accounts.google.com](http://go.microsoft.com/fwlink/p/?LinkId=268302).
* [Google Cloud Messaging-klientkomponent](http://components.xamarin.com/view/GCMClient/).

## <a name="configure-hub"></a>Konfigurera en Meddelandehubb
[!INCLUDE [app-service-mobile-configure-notification-hub](../../includes/app-service-mobile-configure-notification-hub.md)]

## <a id="register"></a>Aktivera Firebase Cloud Messaging
[!INCLUDE [notification-hubs-enable-firebase-cloud-messaging](../../includes/notification-hubs-enable-firebase-cloud-messaging.md)]

## <a name="configure-azure-toosend-push-requests"></a>Konfigurera Azure toosend push-begäranden
[!INCLUDE [app-service-mobile-android-configure-push](../../includes/app-service-mobile-android-configure-push-for-firebase.md)]

## <a id="update-server"></a>Uppdatera hello server projektet toosend push-meddelanden
[!INCLUDE [app-service-mobile-update-server-project-for-push-template](../../includes/app-service-mobile-update-server-project-for-push-template.md)]

## <a id="configure-app"></a>Konfigurera hello klientprojekt för push-meddelanden
[!INCLUDE [mobile-services-xamarin-android-push-configure-project](../../includes/mobile-services-xamarin-android-push-configure-project.md)]

## <a id="add-push"></a>Lägg till push-meddelanden kod tooyour app
[!INCLUDE [app-service-mobile-xamarin-android-push-add-to-app](../../includes/app-service-mobile-xamarin-android-push-add-to-app.md)]

## <a name="test"></a>Testa push-meddelanden i appen
Du kan testa hello appen med hjälp av en virtuell enhet i hello-emulatorn. Det finns ytterligare konfigurationssteg som krävs vid körning på en emulator.

1. Se till att du distribuerar tooor felsökning på en virtuell enhet som har Google APIs som hello mål, enligt nedan i hello Android Virtual Device (AVD)-hanteraren.
   
    ![](./media/app-service-mobile-xamarin-android-get-started-push/google-apis-avd-settings.png)
2. Lägg till en Google-konto toohello Android-enhet genom att klicka på **appar** > **inställningar** > **Lägg till konto**, följ sedan anvisningarna för hello.
   
    ![](./media/app-service-mobile-xamarin-android-get-started-push/add-google-account.png)
3. Kör hello todolist appen som innan och infoga ett nytt todo-objekt. Den här tiden kan visas en meddelandeikonen i meddelandefältet hello. Du kan öppna hello lådan tooview hello fullständig meddelandetext av hello-meddelande.
   
    ![](./media/app-service-mobile-xamarin-android-get-started-push/android-notifications.png)

<!-- URLs. -->
[Xamarin.Android quick start]: app-service-mobile-xamarin-android-get-started.md
[Google Cloud Messaging Client Component]: http://components.xamarin.com/view/GCMClient/
[Azure Mobile Services Component]: http://components.xamarin.com/view/azure-mobile-services/
