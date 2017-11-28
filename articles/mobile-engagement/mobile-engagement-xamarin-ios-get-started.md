---
title: "Kom igång med Azure Mobile Engagement för Xamarin.iOS"
description: "Lär dig hur du använder Azure Mobile Engagement med analyser och push-meddelanden för Xamarin.iOS-appar."
services: mobile-engagement
documentationcenter: xamarin
author: piyushjo
manager: erikre
editor: 
ms.assetid: 0448209e-fff6-47bd-985c-2cf074bac12f
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-xamarin-ios
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: 9938c3e994acf31244825b1afb347f8c9f90ebe3
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="get-started-with-azure-mobile-engagement-for-xamarinios-apps"></a><span data-ttu-id="0b420-103">Kom igång med Azure Mobile Engagement för Xamarin.iOS-appar</span><span class="sxs-lookup"><span data-stu-id="0b420-103">Get Started with Azure Mobile Engagement for Xamarin.iOS Apps</span></span>
[!INCLUDE [Hero tutorial switcher](../../includes/mobile-engagement-hero-tutorial-switcher.md)]

<span data-ttu-id="0b420-104">I den här artikeln beskrivs hur du använder Azure Mobile Engagement för att förstå appanvändningen, och hur du skickar push-meddelanden till segmenterade användare i ett Xamarin.iOS-program.</span><span class="sxs-lookup"><span data-stu-id="0b420-104">This topic shows you how to use Azure Mobile Engagement to understand your app usage and send push notifications to segmented users in a Xamarin.iOS application.</span></span>
<span data-ttu-id="0b420-105">I den här kursen får du skapa en tom Xamarin.iOS-app som samlar in grundläggande data och tar emot push-meddelanden via Apple Push Notification System (APNS).</span><span class="sxs-lookup"><span data-stu-id="0b420-105">In this tutorial, you create a blank Xamarin.iOS app that collects basic data and receives push notifications using Apple Push Notification System (APNS).</span></span>

> [!NOTE]
> <span data-ttu-id="0b420-106">Tjänsten Azure Mobile Engagement kommer att dras tillbaka i mars 2018 och är för närvarande endast tillgänglig för befintliga kunder.</span><span class="sxs-lookup"><span data-stu-id="0b420-106">The Azure Mobile Engagement service will be retired March 2018 and is currently only available to existing customers.</span></span> <span data-ttu-id="0b420-107">Mer information finns i [Mobile Engagement](https://azure.microsoft.com/en-us/services/mobile-engagement/).</span><span class="sxs-lookup"><span data-stu-id="0b420-107">For more information, see [Mobile Engagement](https://azure.microsoft.com/en-us/services/mobile-engagement/).</span></span>

<span data-ttu-id="0b420-108">För den här kursen behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="0b420-108">This tutorial requires the following:</span></span>

* <span data-ttu-id="0b420-109">[Xamarin Studio](http://xamarin.com/studio).</span><span class="sxs-lookup"><span data-stu-id="0b420-109">[Xamarin Studio](http://xamarin.com/studio).</span></span> <span data-ttu-id="0b420-110">Du kan även använda Visual Studio med Xamarin men i den här kursen används Xamarin Studio.</span><span class="sxs-lookup"><span data-stu-id="0b420-110">You can also use Visual Studio with Xamarin but this tutorial uses Xamarin Studio.</span></span> <span data-ttu-id="0b420-111">Det finns installationsanvisningar i avsnittet om [konfiguration och installation för Visual Studio och Xamarin](https://msdn.microsoft.com/library/mt613162.aspx).</span><span class="sxs-lookup"><span data-stu-id="0b420-111">For installation instructions, see [Setup and Install for Visual Studio and Xamarin](https://msdn.microsoft.com/library/mt613162.aspx).</span></span> 
* [<span data-ttu-id="0b420-112">Mobile Engagement Xamarin SDK</span><span class="sxs-lookup"><span data-stu-id="0b420-112">Mobile Engagement Xamarin SDK</span></span>](https://www.nuget.org/packages/Microsoft.Azure.Engagement.Xamarin/)

> [!NOTE]
> <span data-ttu-id="0b420-113">Du måste ha ett aktivt Azure-konto för att slutföra den här kursen.</span><span class="sxs-lookup"><span data-stu-id="0b420-113">To complete this tutorial, you must have an active Azure account.</span></span> <span data-ttu-id="0b420-114">Om du inte har något konto kan skapa du ett kostnadsfritt utvärderingskonto på bara några minuter.</span><span class="sxs-lookup"><span data-stu-id="0b420-114">If you don't have an account, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="0b420-115">Mer information om den [kostnadsfria utvärderingsversionen av Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-xamarin-ios-get-started).</span><span class="sxs-lookup"><span data-stu-id="0b420-115">For details, see [Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-xamarin-ios-get-started).</span></span>
> 
> 

## <span data-ttu-id="0b420-116"><a id="setup-azme"></a>Konfigurera Mobile Engagement för din iOS-app</span><span class="sxs-lookup"><span data-stu-id="0b420-116"><a id="setup-azme"></a>Setup Mobile Engagement for your iOS app</span></span>
[!INCLUDE [Create Mobile Engagement App in Portal](../../includes/mobile-engagement-create-app-in-portal-new.md)]

## <span data-ttu-id="0b420-117"><a id="connecting-app"></a>Anslut appen till Mobile Engagement-serverdelen</span><span class="sxs-lookup"><span data-stu-id="0b420-117"><a id="connecting-app"></a>Connect your app to the Mobile Engagement backend</span></span>
<span data-ttu-id="0b420-118">I den här kursen behandlas en ”grundläggande integration”, vilket är den minsta uppsättningen som krävs för att samla in data och skicka ett push-meddelande.</span><span class="sxs-lookup"><span data-stu-id="0b420-118">This tutorial presents a "basic integration" which is the minimal set required to collect data and send a push notification.</span></span>

<span data-ttu-id="0b420-119">Vi kommer att skapa en grundläggande app i Xamarin för att demonstrera integrationen:</span><span class="sxs-lookup"><span data-stu-id="0b420-119">We will create a basic app with Xamarin to demonstrate the integration:</span></span>

### <a name="create-a-new-xamarinios-project"></a><span data-ttu-id="0b420-120">Skapa ett nytt Xamarin.iOS-projekt</span><span class="sxs-lookup"><span data-stu-id="0b420-120">Create a new Xamarin.iOS project</span></span>
1. <span data-ttu-id="0b420-121">Starta Xamarin Studio.</span><span class="sxs-lookup"><span data-stu-id="0b420-121">Launch Xamarin Studio.</span></span> <span data-ttu-id="0b420-122">Gå till **File** -> **New** -> **Solution** (Arkiv -> Ny -> Lösning).</span><span class="sxs-lookup"><span data-stu-id="0b420-122">Go to **File** -> **New** -> **Solution**</span></span> 
   
    ![][1]
2. <span data-ttu-id="0b420-123">Välj **Single View App** (App med enkel vy) och kontrollera att det valda språket är **C#**. Klicka sedan på **Next** (Nästa).</span><span class="sxs-lookup"><span data-stu-id="0b420-123">Select **Single View App**, make sure the selected language is **C#** and then click **Next**.</span></span>
   
    ![][2]
3. <span data-ttu-id="0b420-124">Fyll i **appens namn** och **organisations-ID:t**, och klicka sedan på **Next** (Nästa).</span><span class="sxs-lookup"><span data-stu-id="0b420-124">Fill in the **App Name** and the **Organization Identifier** and then click **Next**.</span></span> 
   
    ![][3]
   
   > [!IMPORTANT]
   > <span data-ttu-id="0b420-125">Kontrollera att publiceringsprofilen som du så småningom ska använda för att distribuera iOS-appen använder ett app-ID som exakt matchar det paket-ID som du har här.</span><span class="sxs-lookup"><span data-stu-id="0b420-125">Make sure that the publishing profile you eventually use to deploy your iOS app is using an App ID which matches exactly with the Bundle Identifier you have here.</span></span> 
   > 
   > 
4. <span data-ttu-id="0b420-126">Uppdatera **projektets namn**, **lösningens namn** och **platsen** om det behövs, och klicka på **Create** (Skapa).</span><span class="sxs-lookup"><span data-stu-id="0b420-126">Update the **Project Name**, **Solution Name** and **Location** if required and click **Create**.</span></span>
   
    ![][4]

<span data-ttu-id="0b420-127">Xamarin Studio skapar demoappen där Mobile Engagement ska integreras.</span><span class="sxs-lookup"><span data-stu-id="0b420-127">Xamarin Studio will create the demo app in which we will integrate Mobile Engagement.</span></span> 

### <a name="connect-your-app-to-mobile-engagement-backend"></a><span data-ttu-id="0b420-128">Ansluta appen till Mobile Engagement-serverdelen</span><span class="sxs-lookup"><span data-stu-id="0b420-128">Connect your app to Mobile Engagement backend</span></span>
1. <span data-ttu-id="0b420-129">Högerklicka på mappen **Packages** (Paket) i lösningsfönstret och välj **Add Packages** (Lägg till paket).</span><span class="sxs-lookup"><span data-stu-id="0b420-129">Right click the **Packages** folder in the Solution windows and select **Add Packages...**</span></span>
   
    ![][5]
2. <span data-ttu-id="0b420-130">Sök efter **Microsoft Azure Mobile Engagement Xamarin SDK** och lägg till den i lösningen.</span><span class="sxs-lookup"><span data-stu-id="0b420-130">Search for the **Microsoft Azure Mobile Engagement Xamarin SDK** and add it to your solution.</span></span>  
   
    ![][6]
3. <span data-ttu-id="0b420-131">Öppna **AppDelegate.cs** och lägg till följande med instruktionen:</span><span class="sxs-lookup"><span data-stu-id="0b420-131">Open **AppDelegate.cs** and add the following using statement:</span></span>
   
        using Microsoft.Azure.Engagement.Xamarin;
4. <span data-ttu-id="0b420-132">Lägg till följande i metoden **FinishedLaunching** för att initiera anslutningen till Mobile Engagement-serverdelen.</span><span class="sxs-lookup"><span data-stu-id="0b420-132">In the **FinishedLaunching** method, add the following to initialize the connection with Mobile Engagement backend.</span></span> <span data-ttu-id="0b420-133">Var noga med att lägga till din **ConnectionString**.</span><span class="sxs-lookup"><span data-stu-id="0b420-133">Make sure to add your **ConnectionString**.</span></span> <span data-ttu-id="0b420-134">I den här koden används även en **NotificationIcon**-platshållare som läggs till av Mobile Engagement SDK. Det kan hända att du vill ersätta den.</span><span class="sxs-lookup"><span data-stu-id="0b420-134">This code also uses a dummy **NotificationIcon** which is added by the Mobile Engagement SDK which you may want to replace.</span></span> 
   
        EngagementConfiguration config = new EngagementConfiguration {
                        ConnectionString = "YourConnectionStringFromAzurePortal",
                        NotificationIcon = UIImage.FromBundle("close")
                    };
        EngagementAgent.Init (config);

## <span data-ttu-id="0b420-135"><a id="monitor"></a>Aktivera realtidsövervakning</span><span class="sxs-lookup"><span data-stu-id="0b420-135"><a id="monitor"></a>Enabling real-time monitoring</span></span>
<span data-ttu-id="0b420-136">För att kunna börja skicka data och försäkra dig om att användarna är aktiva måste du skicka minst en skärm till Mobile Engagement-serverdelen.</span><span class="sxs-lookup"><span data-stu-id="0b420-136">In order to start sending data and ensuring the users are active, you must send at least one screen to the Mobile Engagement backend.</span></span>

1. <span data-ttu-id="0b420-137">Öppna **ViewController.cs** och lägg till följande med instruktionen:</span><span class="sxs-lookup"><span data-stu-id="0b420-137">Open **ViewController.cs** and add the following using statement:</span></span>
   
        using Microsoft.Azure.Engagement.Xamarin;
2. <span data-ttu-id="0b420-138">Ersätt klassen från vilken `ViewController` ärver från `UIViewController` till `EngagementViewController`.</span><span class="sxs-lookup"><span data-stu-id="0b420-138">Replace the class from which `ViewController` inherits from `UIViewController` to `EngagementViewController`.</span></span> 

## <span data-ttu-id="0b420-139"><a id="monitor"></a>Anslut appen med realtidsövervakning</span><span class="sxs-lookup"><span data-stu-id="0b420-139"><a id="monitor"></a>Connect app with real-time monitoring</span></span>
[!INCLUDE [Connect app with real-time monitoring](../../includes/mobile-engagement-connect-app-with-monitor.md)]

## <span data-ttu-id="0b420-140"><a id="integrate-push"></a>Aktivera push-meddelanden och meddelanden i appen</span><span class="sxs-lookup"><span data-stu-id="0b420-140"><a id="integrate-push"></a>Enable push notifications and in-app messaging</span></span>
<span data-ttu-id="0b420-141">Med Mobile Engagement kan du samverka med användarna, och köra kampanjer med push-meddelanden och meddelanden i appen.</span><span class="sxs-lookup"><span data-stu-id="0b420-141">Mobile Engagement allows you to interact with your users and REACH with push notifications and in-app messaging in the context of campaigns.</span></span> <span data-ttu-id="0b420-142">Modulen som används för det heter REACH och finns i Mobile Engagement-portalen.</span><span class="sxs-lookup"><span data-stu-id="0b420-142">This module is called REACH in the Mobile Engagement portal.</span></span>
<span data-ttu-id="0b420-143">I följande avsnitt konfigurerar du appen för att ta emot dem.</span><span class="sxs-lookup"><span data-stu-id="0b420-143">The following sections set up your app to receive them.</span></span>

### <a name="modify-your-application-delegate"></a><span data-ttu-id="0b420-144">Ändra programdelegaten</span><span class="sxs-lookup"><span data-stu-id="0b420-144">Modify your Application Delegate</span></span>
1. <span data-ttu-id="0b420-145">Öppna **AppDelegate.cs** och lägg till följande med instruktionen:</span><span class="sxs-lookup"><span data-stu-id="0b420-145">Open the **AppDelegate.cs** and add the following using statement:</span></span>
   
        using System; 
2. <span data-ttu-id="0b420-146">Lägg sedan till följande i metoden `FinishedLaunching` för att registrera för push-meddelanden efter `EngagementAgent.init(...)`</span><span class="sxs-lookup"><span data-stu-id="0b420-146">Now inside the `FinishedLaunching` method, add the following to register for push messages after `EngagementAgent.init(...)`</span></span>
   
        if (UIDevice.CurrentDevice.CheckSystemVersion(8,0))
        {
            var pushSettings = UIUserNotificationSettings.GetSettingsForTypes (
                (UIUserNotificationType.Badge |
                    UIUserNotificationType.Sound |
                    UIUserNotificationType.Alert),
                null);
            UIApplication.SharedApplication.RegisterUserNotificationSettings (pushSettings);
            UIApplication.SharedApplication.RegisterForRemoteNotifications ();
        }
        else
        {
            UIApplication.SharedApplication.RegisterForRemoteNotificationTypes (
                UIRemoteNotificationType.Badge |
                UIRemoteNotificationType.Sound |
                UIRemoteNotificationType.Alert);
        }
3. <span data-ttu-id="0b420-147">Till sist ska du uppdatera eller lägga till följande metoder:</span><span class="sxs-lookup"><span data-stu-id="0b420-147">Finally - update or add the following methods:</span></span>
   
        public override void DidReceiveRemoteNotification (UIApplication application, NSDictionary userInfo, 
            Action<UIBackgroundFetchResult> completionHandler)
        {
            EngagementAgent.ApplicationDidReceiveRemoteNotification(userInfo, completionHandler);
        }
   
        public override void RegisteredForRemoteNotifications (UIApplication application, NSData deviceToken)
        {
            // Register device token on Engagement
            EngagementAgent.RegisterDeviceToken(deviceToken);
        }
   
        public override void FailedToRegisterForRemoteNotifications(UIApplication application, NSError error)
        {
            Console.WriteLine("Failed to register for remote notifications: Error '{0}'", error);
        }
4. <span data-ttu-id="0b420-148">Bekräfta i lösningens **Info.plist**-fil att **Bundle Identifier** (Paket-ID) matchar det **App ID** som du har i din etableringsprofil i Apple Dev Center.</span><span class="sxs-lookup"><span data-stu-id="0b420-148">In your **Info.plist** file in the solution, confirm that the **Bundle Identifier** matches with the **App ID** you have in your provisioning profile in the Apple Dev Center.</span></span> 
   
    ![][7]
5. <span data-ttu-id="0b420-149">Kontrollera i samma **Info.plist**-fil att du har markerat **Enable Background Modes** (Aktivera bakgrundslägen) och **Remote Notifications** (Fjärrmeddelanden).</span><span class="sxs-lookup"><span data-stu-id="0b420-149">In the same **Info.plist** file, make sure that you have checked the **Enable Background Modes** and **Remote Notifications**.</span></span> 
   
     ![][8]
6. <span data-ttu-id="0b420-150">Kör appen på den enhet som du har associerat med den här publiceringsprofilen.</span><span class="sxs-lookup"><span data-stu-id="0b420-150">Run the app on the device you have associated with this publishing profile.</span></span> 

[!INCLUDE [mobile-engagement-ios-send-push-push](../../includes/mobile-engagement-ios-send-push.md)]

<!-- Images. -->
[1]: ./media/mobile-engagement-xamarin-ios-get-started/new-solution.png
[2]: ./media/mobile-engagement-xamarin-ios-get-started/app-type.png
[3]: ./media/mobile-engagement-xamarin-ios-get-started/configure-project-name.png
[4]: ./media/mobile-engagement-xamarin-ios-get-started/configure-project-confirm.png
[5]: ./media/mobile-engagement-xamarin-ios-get-started/add-nuget.png
[6]: ./media/mobile-engagement-xamarin-ios-get-started/add-nuget-azme.png
[7]: ./media/mobile-engagement-xamarin-ios-get-started/info-plist-confirm-bundle.png
[8]: ./media/mobile-engagement-xamarin-ios-get-started/info-plist-configure-push.png
