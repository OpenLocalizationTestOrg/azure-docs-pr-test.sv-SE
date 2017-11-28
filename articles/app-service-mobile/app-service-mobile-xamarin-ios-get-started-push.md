---
title: aaaAdd push-meddelanden tooyour Xamarin.iOS-app med Azure App Service
description: "Lär dig hur toouse Azure App Service toosend push-meddelanden tooyour Xamarin.iOS-app"
services: app-service\mobile
documentationcenter: xamarin
author: ggailey777
manager: syntaxc4
editor: 
ms.assetid: 2921214a-49f8-45e1-a306-a85ce21defca
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-xamarin-ios
ms.devlang: dotnet
ms.topic: article
ms.date: 10/12/2016
ms.author: glenga
ms.openlocfilehash: 3e6439aee4f3fe0f60b9786d0bbfd74c4f5e52d1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="add-push-notifications-tooyour-xamarinios-app"></a><span data-ttu-id="3b8f1-103">Lägg till push-meddelanden tooyour Xamarin.iOS-App</span><span class="sxs-lookup"><span data-stu-id="3b8f1-103">Add push notifications tooyour Xamarin.iOS App</span></span>
[!INCLUDE [app-service-mobile-selector-get-started-push](../../includes/app-service-mobile-selector-get-started-push.md)]

## <a name="overview"></a><span data-ttu-id="3b8f1-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="3b8f1-104">Overview</span></span>
<span data-ttu-id="3b8f1-105">I kursen får du lägga till push-meddelanden toohello [Xamarin.iOS Snabbstart](app-service-mobile-xamarin-ios-get-started.md) projekt så att ett push-meddelande skickas toohello enheten varje gång en post infogas.</span><span class="sxs-lookup"><span data-stu-id="3b8f1-105">In this tutorial, you add push notifications toohello [Xamarin.iOS quick start](app-service-mobile-xamarin-ios-get-started.md) project so that a push notification is sent toohello device every time a record is inserted.</span></span>

<span data-ttu-id="3b8f1-106">Om du inte använder hello laddat ned Snabbstart serverprojekt och du behöver hello tilläggspaket för push-meddelande.</span><span class="sxs-lookup"><span data-stu-id="3b8f1-106">If you do not use hello downloaded quick start server project, you will need hello push notification extension package.</span></span> <span data-ttu-id="3b8f1-107">Se [arbeta med serverdelen för hello .NET SDK för Azure Mobile Apps](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md) för mer information.</span><span class="sxs-lookup"><span data-stu-id="3b8f1-107">See [Work with hello .NET backend server SDK for Azure Mobile Apps](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md) for more information.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="3b8f1-108">Krav</span><span class="sxs-lookup"><span data-stu-id="3b8f1-108">Prerequisites</span></span>
* <span data-ttu-id="3b8f1-109">Fullständig hello [Xamarin.iOS quickstart](app-service-mobile-xamarin-ios-get-started.md) kursen.</span><span class="sxs-lookup"><span data-stu-id="3b8f1-109">Complete hello [Xamarin.iOS quickstart](app-service-mobile-xamarin-ios-get-started.md) tutorial.</span></span>
* <span data-ttu-id="3b8f1-110">En fysisk iOS-enhet.</span><span class="sxs-lookup"><span data-stu-id="3b8f1-110">A physical iOS device.</span></span> <span data-ttu-id="3b8f1-111">Push-meddelanden stöds inte av hello iOS-simulatorn.</span><span class="sxs-lookup"><span data-stu-id="3b8f1-111">Push notifications are not supported by hello iOS simulator.</span></span>

## <a name="register-hello-app-for-push-notifications-on-apples-developer-portal"></a><span data-ttu-id="3b8f1-112">Registrera hello app för push-meddelanden på Apple developer-portalen</span><span class="sxs-lookup"><span data-stu-id="3b8f1-112">Register hello app for push notifications on Apple's developer portal</span></span>
[!INCLUDE [Enable Apple Push Notifications](../../includes/enable-apple-push-notifications.md)]

## <a name="configure-your-mobile-app-toosend-push-notifications"></a><span data-ttu-id="3b8f1-113">Konfigurera din Mobilapp toosend push-meddelanden</span><span class="sxs-lookup"><span data-stu-id="3b8f1-113">Configure your Mobile App toosend push notifications</span></span>
[!INCLUDE [app-service-mobile-apns-configure-push](../../includes/app-service-mobile-apns-configure-push.md)]

## <a name="update-hello-server-project-toosend-push-notifications"></a><span data-ttu-id="3b8f1-114">Uppdatera hello server projektet toosend push-meddelanden</span><span class="sxs-lookup"><span data-stu-id="3b8f1-114">Update hello server project toosend push notifications</span></span>
[!INCLUDE [app-service-mobile-update-server-project-for-push-template](../../includes/app-service-mobile-update-server-project-for-push-template.md)]

## <a name="configure-your-xamarinios-project"></a><span data-ttu-id="3b8f1-115">Konfigurera Xamarin.iOS-projekt</span><span class="sxs-lookup"><span data-stu-id="3b8f1-115">Configure your Xamarin.iOS project</span></span>
[!INCLUDE [app-service-mobile-xamarin-ios-configure-project](../../includes/app-service-mobile-xamarin-ios-configure-project.md)]

## <a name="add-push-notifications-tooyour-app"></a><span data-ttu-id="3b8f1-116">Lägg till push-meddelanden tooyour app</span><span class="sxs-lookup"><span data-stu-id="3b8f1-116">Add push notifications tooyour app</span></span>
1. <span data-ttu-id="3b8f1-117">I **QSTodoService**, Lägg till följande egenskapen hello så att **AppDelegate** kan hämta hello mobila klienten:</span><span class="sxs-lookup"><span data-stu-id="3b8f1-117">In **QSTodoService**, add hello following property so that **AppDelegate** can acquire hello mobile client:</span></span>
   
            public MobileServiceClient GetClient {
            get
            {
                return client;
            }
            private set
            {
                client = value;
            }
        }
2. <span data-ttu-id="3b8f1-118">Lägg till följande hello `using` instruktionen toohello överkant hello **AppDelegate.cs** fil.</span><span class="sxs-lookup"><span data-stu-id="3b8f1-118">Add hello following `using` statement toohello top of hello **AppDelegate.cs** file.</span></span>
   
        using Microsoft.WindowsAzure.MobileServices;
        using Newtonsoft.Json.Linq;
3. <span data-ttu-id="3b8f1-119">I **AppDelegate**, åsidosätta hello **FinishedLaunching** händelse:</span><span class="sxs-lookup"><span data-stu-id="3b8f1-119">In **AppDelegate**, override hello **FinishedLaunching** event:</span></span>
   
        public override bool FinishedLaunching(UIApplication application, NSDictionary launchOptions)
        {
            // registers for push for iOS8
            var settings = UIUserNotificationSettings.GetSettingsForTypes(
                UIUserNotificationType.Alert
                | UIUserNotificationType.Badge
                | UIUserNotificationType.Sound,
                new NSSet());
   
            UIApplication.SharedApplication.RegisterUserNotificationSettings(settings);
            UIApplication.SharedApplication.RegisterForRemoteNotifications();
   
            return true;
        }
4. <span data-ttu-id="3b8f1-120">I Hej samma fil, åsidosätta hello **RegisteredForRemoteNotifications** händelse.</span><span class="sxs-lookup"><span data-stu-id="3b8f1-120">In hello same file, override hello **RegisteredForRemoteNotifications** event.</span></span> <span data-ttu-id="3b8f1-121">I den här koden registrerar du för en enkel mall-meddelandet som skickas över alla plattformar som stöds av hello-servern.</span><span class="sxs-lookup"><span data-stu-id="3b8f1-121">In this code you are registering for a simple template notification that will be sent across all supported platforms by hello server.</span></span>
   
    <span data-ttu-id="3b8f1-122">Mer information om mallar med Notification Hubs finns [mallar](../notification-hubs/notification-hubs-templates-cross-platform-push-messages.md).</span><span class="sxs-lookup"><span data-stu-id="3b8f1-122">For more information on templates with Notification Hubs, see [Templates](../notification-hubs/notification-hubs-templates-cross-platform-push-messages.md).</span></span>

        public override void RegisteredForRemoteNotifications(UIApplication application, NSData deviceToken)
        {
            MobileServiceClient client = QSTodoService.DefaultService.GetClient;

            const string templateBodyAPNS = "{\"aps\":{\"alert\":\"$(messageParam)\"}}";

            JObject templates = new JObject();
            templates["genericMessage"] = new JObject
            {
                {"body", templateBodyAPNS}
            };

            // Register for push with your mobile app
            var push = client.GetPush();
            push.RegisterAsync(deviceToken, templates);
        }


1. <span data-ttu-id="3b8f1-123">Sedan kan åsidosätta hello **DidReceivedRemoteNotification** händelse:</span><span class="sxs-lookup"><span data-stu-id="3b8f1-123">Then, override hello **DidReceivedRemoteNotification** event:</span></span>
   
        public override void DidReceiveRemoteNotification (UIApplication application, NSDictionary userInfo, Action<UIBackgroundFetchResult> completionHandler)
        {
            NSDictionary aps = userInfo.ObjectForKey(new NSString("aps")) as NSDictionary;
   
            string alert = string.Empty;
            if (aps.ContainsKey(new NSString("alert")))
                alert = (aps [new NSString("alert")] as NSString).ToString();
   
            //show alert
            if (!string.IsNullOrEmpty(alert))
            {
                UIAlertView avAlert = new UIAlertView("Notification", alert, null, "OK", null);
                avAlert.Show();
            }
        }

<span data-ttu-id="3b8f1-124">Appen är nu uppdaterade toosupport push-meddelanden.</span><span class="sxs-lookup"><span data-stu-id="3b8f1-124">Your app is now updated toosupport push notifications.</span></span>

## <span data-ttu-id="3b8f1-125"><a name="test"></a>Testa push-meddelanden i appen</span><span class="sxs-lookup"><span data-stu-id="3b8f1-125"><a name="test"></a>Test push notifications in your app</span></span>
1. <span data-ttu-id="3b8f1-126">Tryck på hello **kör** knappen toobuild hello projektet och starta hello app i en kompatibla iOS-enhet, och klicka sedan **OK** tooaccept push-meddelanden.</span><span class="sxs-lookup"><span data-stu-id="3b8f1-126">Press hello **Run** button toobuild hello project and start hello app in an iOS capable device, then click **OK** tooaccept push notifications.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="3b8f1-127">Du måste uttryckligen godkänna push-meddelanden från din app.</span><span class="sxs-lookup"><span data-stu-id="3b8f1-127">You must explicitly accept push notifications from your app.</span></span> <span data-ttu-id="3b8f1-128">Denna begäran inträffar bara hello första gången som hello appen körs.</span><span class="sxs-lookup"><span data-stu-id="3b8f1-128">This request only occurs hello first time that hello app runs.</span></span>
   > 
   > 
2. <span data-ttu-id="3b8f1-129">Skriv en aktivitet i hello app och klicka på hello plus (**+**) ikon.</span><span class="sxs-lookup"><span data-stu-id="3b8f1-129">In hello app, type a task, and then click hello plus (**+**) icon.</span></span>
3. <span data-ttu-id="3b8f1-130">Kontrollera att ett meddelande tas emot och klickar sedan **OK** toodismiss hello-meddelande.</span><span class="sxs-lookup"><span data-stu-id="3b8f1-130">Verify that a notification is received, then click **OK** toodismiss hello notification.</span></span>
4. <span data-ttu-id="3b8f1-131">Upprepa steg 2 och omedelbart Stäng hello appen och sedan kontrollera att ett meddelande visas.</span><span class="sxs-lookup"><span data-stu-id="3b8f1-131">Repeat step 2 and immediately close hello app, then verify that a notification is shown.</span></span>

<span data-ttu-id="3b8f1-132">Du har slutfört den här kursen.</span><span class="sxs-lookup"><span data-stu-id="3b8f1-132">You have successfully completed this tutorial.</span></span>

<!-- Images. -->

<!-- URLs. -->



