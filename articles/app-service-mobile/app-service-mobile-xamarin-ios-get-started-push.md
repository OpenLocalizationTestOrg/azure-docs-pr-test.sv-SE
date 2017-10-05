---
title: "Lägga till push-meddelanden för Xamarin.iOS-app med Azure App Service"
description: "Lär dig hur du använder Azure App Service för att skicka push-meddelanden till Xamarin.iOS-app"
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
ms.openlocfilehash: bf922e49c4c92d0065817a5dd6c7d10a04737304
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="add-push-notifications-to-your-xamarinios-app"></a><span data-ttu-id="d8399-103">Lägga till push-meddelanden för Xamarin.iOS-App</span><span class="sxs-lookup"><span data-stu-id="d8399-103">Add push notifications to your Xamarin.iOS App</span></span>
[!INCLUDE [app-service-mobile-selector-get-started-push](../../includes/app-service-mobile-selector-get-started-push.md)]

## <a name="overview"></a><span data-ttu-id="d8399-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="d8399-104">Overview</span></span>
<span data-ttu-id="d8399-105">I kursen får du lägga till push-meddelanden till den [Xamarin.iOS Snabbstart](app-service-mobile-xamarin-ios-get-started.md) projekt så att ett push-meddelande skickas till enheten varje gång en post infogas.</span><span class="sxs-lookup"><span data-stu-id="d8399-105">In this tutorial, you add push notifications to the [Xamarin.iOS quick start](app-service-mobile-xamarin-ios-get-started.md) project so that a push notification is sent to the device every time a record is inserted.</span></span>

<span data-ttu-id="d8399-106">Om du inte använder hämtade Snabbstart serverprojekt behöver push notification extension-paketet.</span><span class="sxs-lookup"><span data-stu-id="d8399-106">If you do not use the downloaded quick start server project, you will need the push notification extension package.</span></span> <span data-ttu-id="d8399-107">Se [arbeta med serverdelen .NET SDK för Azure Mobile Apps](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md) för mer information.</span><span class="sxs-lookup"><span data-stu-id="d8399-107">See [Work with the .NET backend server SDK for Azure Mobile Apps](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md) for more information.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d8399-108">Krav</span><span class="sxs-lookup"><span data-stu-id="d8399-108">Prerequisites</span></span>
* <span data-ttu-id="d8399-109">Slutför den [Xamarin.iOS quickstart](app-service-mobile-xamarin-ios-get-started.md) kursen.</span><span class="sxs-lookup"><span data-stu-id="d8399-109">Complete the [Xamarin.iOS quickstart](app-service-mobile-xamarin-ios-get-started.md) tutorial.</span></span>
* <span data-ttu-id="d8399-110">En fysisk iOS-enhet.</span><span class="sxs-lookup"><span data-stu-id="d8399-110">A physical iOS device.</span></span> <span data-ttu-id="d8399-111">Push-meddelanden stöds inte av iOS-simulatorn.</span><span class="sxs-lookup"><span data-stu-id="d8399-111">Push notifications are not supported by the iOS simulator.</span></span>

## <a name="register-the-app-for-push-notifications-on-apples-developer-portal"></a><span data-ttu-id="d8399-112">Registrera appen för push-meddelanden på Apple developer-portalen</span><span class="sxs-lookup"><span data-stu-id="d8399-112">Register the app for push notifications on Apple's developer portal</span></span>
[!INCLUDE [Enable Apple Push Notifications](../../includes/enable-apple-push-notifications.md)]

## <a name="configure-your-mobile-app-to-send-push-notifications"></a><span data-ttu-id="d8399-113">Konfigurera Mobilappen för att skicka push-meddelanden</span><span class="sxs-lookup"><span data-stu-id="d8399-113">Configure your Mobile App to send push notifications</span></span>
[!INCLUDE [app-service-mobile-apns-configure-push](../../includes/app-service-mobile-apns-configure-push.md)]

## <a name="update-the-server-project-to-send-push-notifications"></a><span data-ttu-id="d8399-114">Uppdatera serverprojekt för att skicka push-meddelanden</span><span class="sxs-lookup"><span data-stu-id="d8399-114">Update the server project to send push notifications</span></span>
[!INCLUDE [app-service-mobile-update-server-project-for-push-template](../../includes/app-service-mobile-update-server-project-for-push-template.md)]

## <a name="configure-your-xamarinios-project"></a><span data-ttu-id="d8399-115">Konfigurera Xamarin.iOS-projekt</span><span class="sxs-lookup"><span data-stu-id="d8399-115">Configure your Xamarin.iOS project</span></span>
[!INCLUDE [app-service-mobile-xamarin-ios-configure-project](../../includes/app-service-mobile-xamarin-ios-configure-project.md)]

## <a name="add-push-notifications-to-your-app"></a><span data-ttu-id="d8399-116">Lägg till push-meddelanden i appen</span><span class="sxs-lookup"><span data-stu-id="d8399-116">Add push notifications to your app</span></span>
1. <span data-ttu-id="d8399-117">I **QSTodoService**, Lägg till följande egenskap så att **AppDelegate** kan hämta den mobila klienten:</span><span class="sxs-lookup"><span data-stu-id="d8399-117">In **QSTodoService**, add the following property so that **AppDelegate** can acquire the mobile client:</span></span>
   
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
2. <span data-ttu-id="d8399-118">Lägg till följande `using` uttryck överst i den **AppDelegate.cs** fil.</span><span class="sxs-lookup"><span data-stu-id="d8399-118">Add the following `using` statement to the top of the **AppDelegate.cs** file.</span></span>
   
        using Microsoft.WindowsAzure.MobileServices;
        using Newtonsoft.Json.Linq;
3. <span data-ttu-id="d8399-119">I **AppDelegate**, åsidosätta den **FinishedLaunching** händelse:</span><span class="sxs-lookup"><span data-stu-id="d8399-119">In **AppDelegate**, override the **FinishedLaunching** event:</span></span>
   
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
4. <span data-ttu-id="d8399-120">I samma fil åsidosätta den **RegisteredForRemoteNotifications** händelse.</span><span class="sxs-lookup"><span data-stu-id="d8399-120">In the same file, override the **RegisteredForRemoteNotifications** event.</span></span> <span data-ttu-id="d8399-121">I den här koden registrerar du för en enkel mall-meddelandet som skickas över alla plattformar som stöds av servern.</span><span class="sxs-lookup"><span data-stu-id="d8399-121">In this code you are registering for a simple template notification that will be sent across all supported platforms by the server.</span></span>
   
    <span data-ttu-id="d8399-122">Mer information om mallar med Notification Hubs finns [mallar](../notification-hubs/notification-hubs-templates-cross-platform-push-messages.md).</span><span class="sxs-lookup"><span data-stu-id="d8399-122">For more information on templates with Notification Hubs, see [Templates](../notification-hubs/notification-hubs-templates-cross-platform-push-messages.md).</span></span>

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


1. <span data-ttu-id="d8399-123">Sedan kan åsidosätta den **DidReceivedRemoteNotification** händelse:</span><span class="sxs-lookup"><span data-stu-id="d8399-123">Then, override the **DidReceivedRemoteNotification** event:</span></span>
   
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

<span data-ttu-id="d8399-124">Appen har uppdaterats för att stödja push-meddelanden.</span><span class="sxs-lookup"><span data-stu-id="d8399-124">Your app is now updated to support push notifications.</span></span>

## <span data-ttu-id="d8399-125"><a name="test"></a>Testa push-meddelanden i appen</span><span class="sxs-lookup"><span data-stu-id="d8399-125"><a name="test"></a>Test push notifications in your app</span></span>
1. <span data-ttu-id="d8399-126">Tryck på den **kör** knappen för att bygga projektet och starta appen i en kompatibla iOS-enhet och klicka sedan på **OK** att ta emot push-meddelanden.</span><span class="sxs-lookup"><span data-stu-id="d8399-126">Press the **Run** button to build the project and start the app in an iOS capable device, then click **OK** to accept push notifications.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="d8399-127">Du måste uttryckligen godkänna push-meddelanden från din app.</span><span class="sxs-lookup"><span data-stu-id="d8399-127">You must explicitly accept push notifications from your app.</span></span> <span data-ttu-id="d8399-128">Denna begäran inträffar bara första gången som appen körs.</span><span class="sxs-lookup"><span data-stu-id="d8399-128">This request only occurs the first time that the app runs.</span></span>
   > 
   > 
2. <span data-ttu-id="d8399-129">I appen, skriver du en uppgift och klicka sedan på plustecknet (**+**) ikon.</span><span class="sxs-lookup"><span data-stu-id="d8399-129">In the app, type a task, and then click the plus (**+**) icon.</span></span>
3. <span data-ttu-id="d8399-130">Kontrollera att ett meddelande tas emot och klickar sedan **OK** att stänga meddelandet.</span><span class="sxs-lookup"><span data-stu-id="d8399-130">Verify that a notification is received, then click **OK** to dismiss the notification.</span></span>
4. <span data-ttu-id="d8399-131">Upprepa steg 2 och omedelbart stänga appen och sedan kontrollera att ett meddelande visas.</span><span class="sxs-lookup"><span data-stu-id="d8399-131">Repeat step 2 and immediately close the app, then verify that a notification is shown.</span></span>

<span data-ttu-id="d8399-132">Du har slutfört den här kursen.</span><span class="sxs-lookup"><span data-stu-id="d8399-132">You have successfully completed this tutorial.</span></span>

<!-- Images. -->

<!-- URLs. -->



