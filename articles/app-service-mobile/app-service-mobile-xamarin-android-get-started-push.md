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
# <a name="add-push-notifications-tooyour-xamarinandroid-app"></a><span data-ttu-id="fb21a-103">Lägg till push-meddelanden tooyour Xamarin.Android-app</span><span class="sxs-lookup"><span data-stu-id="fb21a-103">Add push notifications tooyour Xamarin.Android app</span></span>
[!INCLUDE [app-service-mobile-selector-get-started-push](../../includes/app-service-mobile-selector-get-started-push.md)]

## <a name="overview"></a><span data-ttu-id="fb21a-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="fb21a-104">Overview</span></span>
<span data-ttu-id="fb21a-105">I kursen får du lägga till push-meddelanden toohello [Xamarin.Android Snabbstart](app-service-mobile-windows-store-dotnet-get-started.md) projekt så att ett push-meddelande skickas toohello enheten varje gång en post infogas.</span><span class="sxs-lookup"><span data-stu-id="fb21a-105">In this tutorial, you add push notifications toohello [Xamarin.Android quick start](app-service-mobile-windows-store-dotnet-get-started.md) project so that a push notification is sent toohello device every time a record is inserted.</span></span>

<span data-ttu-id="fb21a-106">Om du inte använder hello laddat ned Snabbstart serverprojekt och du behöver hello tilläggspaket för push-meddelande.</span><span class="sxs-lookup"><span data-stu-id="fb21a-106">If you do not use hello downloaded quick start server project, you will need hello push notification extension package.</span></span> <span data-ttu-id="fb21a-107">Se [arbeta med serverdelen för hello .NET SDK för Azure Mobile Apps](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md) för mer information.</span><span class="sxs-lookup"><span data-stu-id="fb21a-107">See [Work with hello .NET backend server SDK for Azure Mobile Apps](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md) for more information.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="fb21a-108">Krav</span><span class="sxs-lookup"><span data-stu-id="fb21a-108">Prerequisites</span></span>
<span data-ttu-id="fb21a-109">Den här kursen kräver hello följande:</span><span class="sxs-lookup"><span data-stu-id="fb21a-109">This tutorial requires hello following:</span></span>

* <span data-ttu-id="fb21a-110">Ett aktivt Google-konto.</span><span class="sxs-lookup"><span data-stu-id="fb21a-110">An active Google account.</span></span> <span data-ttu-id="fb21a-111">Du kan registrera dig för ett Google-konto på [accounts.google.com](http://go.microsoft.com/fwlink/p/?LinkId=268302).</span><span class="sxs-lookup"><span data-stu-id="fb21a-111">You can sign up for a Google account at [accounts.google.com](http://go.microsoft.com/fwlink/p/?LinkId=268302).</span></span>
* <span data-ttu-id="fb21a-112">[Google Cloud Messaging-klientkomponent](http://components.xamarin.com/view/GCMClient/).</span><span class="sxs-lookup"><span data-stu-id="fb21a-112">[Google Cloud Messaging Client Component](http://components.xamarin.com/view/GCMClient/).</span></span>

## <span data-ttu-id="fb21a-113"><a name="configure-hub"></a>Konfigurera en Meddelandehubb</span><span class="sxs-lookup"><span data-stu-id="fb21a-113"><a name="configure-hub"></a>Configure a Notification Hub</span></span>
[!INCLUDE [app-service-mobile-configure-notification-hub](../../includes/app-service-mobile-configure-notification-hub.md)]

## <span data-ttu-id="fb21a-114"><a id="register"></a>Aktivera Firebase Cloud Messaging</span><span class="sxs-lookup"><span data-stu-id="fb21a-114"><a id="register"></a>Enable Firebase Cloud Messaging</span></span>
[!INCLUDE [notification-hubs-enable-firebase-cloud-messaging](../../includes/notification-hubs-enable-firebase-cloud-messaging.md)]

## <a name="configure-azure-toosend-push-requests"></a><span data-ttu-id="fb21a-115">Konfigurera Azure toosend push-begäranden</span><span class="sxs-lookup"><span data-stu-id="fb21a-115">Configure Azure toosend push requests</span></span>
[!INCLUDE [app-service-mobile-android-configure-push](../../includes/app-service-mobile-android-configure-push-for-firebase.md)]

## <span data-ttu-id="fb21a-116"><a id="update-server"></a>Uppdatera hello server projektet toosend push-meddelanden</span><span class="sxs-lookup"><span data-stu-id="fb21a-116"><a id="update-server"></a>Update hello server project toosend push notifications</span></span>
[!INCLUDE [app-service-mobile-update-server-project-for-push-template](../../includes/app-service-mobile-update-server-project-for-push-template.md)]

## <span data-ttu-id="fb21a-117"><a id="configure-app"></a>Konfigurera hello klientprojekt för push-meddelanden</span><span class="sxs-lookup"><span data-stu-id="fb21a-117"><a id="configure-app"></a>Configure hello client project for push notifications</span></span>
[!INCLUDE [mobile-services-xamarin-android-push-configure-project](../../includes/mobile-services-xamarin-android-push-configure-project.md)]

## <span data-ttu-id="fb21a-118"><a id="add-push"></a>Lägg till push-meddelanden kod tooyour app</span><span class="sxs-lookup"><span data-stu-id="fb21a-118"><a id="add-push"></a>Add push notifications code tooyour app</span></span>
[!INCLUDE [app-service-mobile-xamarin-android-push-add-to-app](../../includes/app-service-mobile-xamarin-android-push-add-to-app.md)]

## <span data-ttu-id="fb21a-119"><a name="test"></a>Testa push-meddelanden i appen</span><span class="sxs-lookup"><span data-stu-id="fb21a-119"><a name="test"></a>Test push notifications in your app</span></span>
<span data-ttu-id="fb21a-120">Du kan testa hello appen med hjälp av en virtuell enhet i hello-emulatorn.</span><span class="sxs-lookup"><span data-stu-id="fb21a-120">You can test hello app by using a virtual device in hello emulator.</span></span> <span data-ttu-id="fb21a-121">Det finns ytterligare konfigurationssteg som krävs vid körning på en emulator.</span><span class="sxs-lookup"><span data-stu-id="fb21a-121">There are additional configuration steps required when running on an emulator.</span></span>

1. <span data-ttu-id="fb21a-122">Se till att du distribuerar tooor felsökning på en virtuell enhet som har Google APIs som hello mål, enligt nedan i hello Android Virtual Device (AVD)-hanteraren.</span><span class="sxs-lookup"><span data-stu-id="fb21a-122">Make sure that you are deploying tooor debugging on a virtual device that has Google APIs set as hello target, as shown below in hello Android Virtual Device (AVD) manager.</span></span>
   
    ![](./media/app-service-mobile-xamarin-android-get-started-push/google-apis-avd-settings.png)
2. <span data-ttu-id="fb21a-123">Lägg till en Google-konto toohello Android-enhet genom att klicka på **appar** > **inställningar** > **Lägg till konto**, följ sedan anvisningarna för hello.</span><span class="sxs-lookup"><span data-stu-id="fb21a-123">Add a Google account toohello Android device by clicking **Apps** > **Settings** > **Add account**, then follow hello prompts.</span></span>
   
    ![](./media/app-service-mobile-xamarin-android-get-started-push/add-google-account.png)
3. <span data-ttu-id="fb21a-124">Kör hello todolist appen som innan och infoga ett nytt todo-objekt.</span><span class="sxs-lookup"><span data-stu-id="fb21a-124">Run hello todolist app as before and insert a new todo item.</span></span> <span data-ttu-id="fb21a-125">Den här tiden kan visas en meddelandeikonen i meddelandefältet hello.</span><span class="sxs-lookup"><span data-stu-id="fb21a-125">This time, a notification icon is displayed in hello notification area.</span></span> <span data-ttu-id="fb21a-126">Du kan öppna hello lådan tooview hello fullständig meddelandetext av hello-meddelande.</span><span class="sxs-lookup"><span data-stu-id="fb21a-126">You can open hello notification drawer tooview hello full text of hello notification.</span></span>
   
    ![](./media/app-service-mobile-xamarin-android-get-started-push/android-notifications.png)

<!-- URLs. -->
[Xamarin.Android quick start]: app-service-mobile-xamarin-android-get-started.md
[Google Cloud Messaging Client Component]: http://components.xamarin.com/view/GCMClient/
[Azure Mobile Services Component]: http://components.xamarin.com/view/azure-mobile-services/
