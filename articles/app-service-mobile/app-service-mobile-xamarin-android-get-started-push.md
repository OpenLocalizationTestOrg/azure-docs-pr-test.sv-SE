---
title: "Lägg till push-meddelanden i Xamarin.Android-appen | Microsoft Docs"
description: "Lär dig hur du använder Azure App Service och Azure Notification Hubs för att skicka push-meddelanden i Xamarin.Android-appen"
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
ms.openlocfilehash: c3757d56fb1792092710740dc5ffbd64f18730cf
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="add-push-notifications-to-your-xamarinandroid-app"></a><span data-ttu-id="40739-103">Lägg till push-meddelanden i Xamarin.Android-appen</span><span class="sxs-lookup"><span data-stu-id="40739-103">Add push notifications to your Xamarin.Android app</span></span>
[!INCLUDE [app-service-mobile-selector-get-started-push](../../includes/app-service-mobile-selector-get-started-push.md)]

## <a name="overview"></a><span data-ttu-id="40739-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="40739-104">Overview</span></span>
<span data-ttu-id="40739-105">I kursen får du lägga till push-meddelanden till den [Xamarin.Android Snabbstart](app-service-mobile-windows-store-dotnet-get-started.md) projekt så att ett push-meddelande skickas till enheten varje gång en post infogas.</span><span class="sxs-lookup"><span data-stu-id="40739-105">In this tutorial, you add push notifications to the [Xamarin.Android quick start](app-service-mobile-windows-store-dotnet-get-started.md) project so that a push notification is sent to the device every time a record is inserted.</span></span>

<span data-ttu-id="40739-106">Om du inte använder hämtade Snabbstart serverprojekt behöver push notification extension-paketet.</span><span class="sxs-lookup"><span data-stu-id="40739-106">If you do not use the downloaded quick start server project, you will need the push notification extension package.</span></span> <span data-ttu-id="40739-107">Se [arbeta med serverdelen .NET SDK för Azure Mobile Apps](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md) för mer information.</span><span class="sxs-lookup"><span data-stu-id="40739-107">See [Work with the .NET backend server SDK for Azure Mobile Apps](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md) for more information.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="40739-108">Krav</span><span class="sxs-lookup"><span data-stu-id="40739-108">Prerequisites</span></span>
<span data-ttu-id="40739-109">För den här kursen behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="40739-109">This tutorial requires the following:</span></span>

* <span data-ttu-id="40739-110">Ett aktivt Google-konto.</span><span class="sxs-lookup"><span data-stu-id="40739-110">An active Google account.</span></span> <span data-ttu-id="40739-111">Du kan registrera dig för ett Google-konto på [accounts.google.com](http://go.microsoft.com/fwlink/p/?LinkId=268302).</span><span class="sxs-lookup"><span data-stu-id="40739-111">You can sign up for a Google account at [accounts.google.com](http://go.microsoft.com/fwlink/p/?LinkId=268302).</span></span>
* <span data-ttu-id="40739-112">[Google Cloud Messaging-klientkomponent](http://components.xamarin.com/view/GCMClient/).</span><span class="sxs-lookup"><span data-stu-id="40739-112">[Google Cloud Messaging Client Component](http://components.xamarin.com/view/GCMClient/).</span></span>

## <span data-ttu-id="40739-113"><a name="configure-hub"></a>Konfigurera en Meddelandehubb</span><span class="sxs-lookup"><span data-stu-id="40739-113"><a name="configure-hub"></a>Configure a Notification Hub</span></span>
[!INCLUDE [app-service-mobile-configure-notification-hub](../../includes/app-service-mobile-configure-notification-hub.md)]

## <span data-ttu-id="40739-114"><a id="register"></a>Aktivera Firebase Cloud Messaging</span><span class="sxs-lookup"><span data-stu-id="40739-114"><a id="register"></a>Enable Firebase Cloud Messaging</span></span>
[!INCLUDE [notification-hubs-enable-firebase-cloud-messaging](../../includes/notification-hubs-enable-firebase-cloud-messaging.md)]

## <a name="configure-azure-to-send-push-requests"></a><span data-ttu-id="40739-115">Konfigurera Azure att skicka push-begäranden</span><span class="sxs-lookup"><span data-stu-id="40739-115">Configure Azure to send push requests</span></span>
[!INCLUDE [app-service-mobile-android-configure-push](../../includes/app-service-mobile-android-configure-push-for-firebase.md)]

## <span data-ttu-id="40739-116"><a id="update-server"></a>Uppdatera serverprojekt för att skicka push-meddelanden</span><span class="sxs-lookup"><span data-stu-id="40739-116"><a id="update-server"></a>Update the server project to send push notifications</span></span>
[!INCLUDE [app-service-mobile-update-server-project-for-push-template](../../includes/app-service-mobile-update-server-project-for-push-template.md)]

## <span data-ttu-id="40739-117"><a id="configure-app"></a>Konfigurera klientprojekt för push-meddelanden</span><span class="sxs-lookup"><span data-stu-id="40739-117"><a id="configure-app"></a>Configure the client project for push notifications</span></span>
[!INCLUDE [mobile-services-xamarin-android-push-configure-project](../../includes/mobile-services-xamarin-android-push-configure-project.md)]

## <span data-ttu-id="40739-118"><a id="add-push"></a>Lägg till kod för push-meddelanden i appen</span><span class="sxs-lookup"><span data-stu-id="40739-118"><a id="add-push"></a>Add push notifications code to your app</span></span>
[!INCLUDE [app-service-mobile-xamarin-android-push-add-to-app](../../includes/app-service-mobile-xamarin-android-push-add-to-app.md)]

## <span data-ttu-id="40739-119"><a name="test"></a>Testa push-meddelanden i appen</span><span class="sxs-lookup"><span data-stu-id="40739-119"><a name="test"></a>Test push notifications in your app</span></span>
<span data-ttu-id="40739-120">Du kan testa appen med hjälp av en virtuell enhet i emulatorn.</span><span class="sxs-lookup"><span data-stu-id="40739-120">You can test the app by using a virtual device in the emulator.</span></span> <span data-ttu-id="40739-121">Det finns ytterligare konfigurationssteg som krävs vid körning på en emulator.</span><span class="sxs-lookup"><span data-stu-id="40739-121">There are additional configuration steps required when running on an emulator.</span></span>

1. <span data-ttu-id="40739-122">Se till att du distribuerar till eller felsökning på en virtuell enhet som har Google APIs som mål, enligt nedan i hanteraren för Android Virtual Device (AVD).</span><span class="sxs-lookup"><span data-stu-id="40739-122">Make sure that you are deploying to or debugging on a virtual device that has Google APIs set as the target, as shown below in the Android Virtual Device (AVD) manager.</span></span>
   
    ![](./media/app-service-mobile-xamarin-android-get-started-push/google-apis-avd-settings.png)
2. <span data-ttu-id="40739-123">Lägg till ett Google-konto för Android-enhet genom att klicka på **appar** > **inställningar** > **Lägg till konto**, följ sedan anvisningarna.</span><span class="sxs-lookup"><span data-stu-id="40739-123">Add a Google account to the Android device by clicking **Apps** > **Settings** > **Add account**, then follow the prompts.</span></span>
   
    ![](./media/app-service-mobile-xamarin-android-get-started-push/add-google-account.png)
3. <span data-ttu-id="40739-124">Kör appen todolist som innan och infoga ett nytt todo-objekt.</span><span class="sxs-lookup"><span data-stu-id="40739-124">Run the todolist app as before and insert a new todo item.</span></span> <span data-ttu-id="40739-125">Den här tiden kan visas en meddelandeikonen i meddelandefältet.</span><span class="sxs-lookup"><span data-stu-id="40739-125">This time, a notification icon is displayed in the notification area.</span></span> <span data-ttu-id="40739-126">Du kan öppna lådan meddelande om du vill visa den fullständiga texten i meddelandet.</span><span class="sxs-lookup"><span data-stu-id="40739-126">You can open the notification drawer to view the full text of the notification.</span></span>
   
    ![](./media/app-service-mobile-xamarin-android-get-started-push/android-notifications.png)

<!-- URLs. -->
[Xamarin.Android quick start]: app-service-mobile-xamarin-android-get-started.md
[Google Cloud Messaging Client Component]: http://components.xamarin.com/view/GCMClient/
[Azure Mobile Services Component]: http://components.xamarin.com/view/azure-mobile-services/
