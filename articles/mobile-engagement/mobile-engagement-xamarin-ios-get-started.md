---
title: "aaaGet igång med Azure Mobile Engagement för Xamarin.iOS"
description: "Lär dig hur toouse Azure Mobile Engagement med analyser och Push-meddelanden för Xamarin.iOS-appar."
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
ms.openlocfilehash: 02340a744753dcc5cd1b6888a5fa87628be47b68
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-azure-mobile-engagement-for-xamarinios-apps"></a><span data-ttu-id="c8477-103">Kom igång med Azure Mobile Engagement för Xamarin.iOS-appar</span><span class="sxs-lookup"><span data-stu-id="c8477-103">Get Started with Azure Mobile Engagement for Xamarin.iOS Apps</span></span>
[!INCLUDE [Hero tutorial switcher](../../includes/mobile-engagement-hero-tutorial-switcher.md)]

<span data-ttu-id="c8477-104">Det här avsnittet beskrivs hur du toouse Azure Mobile Engagement toounderstand din app användnings- och skicka push-meddelanden toosegmented användare i ett Xamarin.iOS-program.</span><span class="sxs-lookup"><span data-stu-id="c8477-104">This topic shows you how toouse Azure Mobile Engagement toounderstand your app usage and send push notifications toosegmented users in a Xamarin.iOS application.</span></span>
<span data-ttu-id="c8477-105">I den här kursen får du skapa en tom Xamarin.iOS-app som samlar in grundläggande data och tar emot push-meddelanden via Apple Push Notification System (APNS).</span><span class="sxs-lookup"><span data-stu-id="c8477-105">In this tutorial, you create a blank Xamarin.iOS app that collects basic data and receives push notifications using Apple Push Notification System (APNS).</span></span>

> [!NOTE]
> <span data-ttu-id="c8477-106">hello Azure Mobile Engagement-tjänsten kommer att dras tillbaka mars 2018 och är för närvarande bara tillgänglig tooexisting kunder.</span><span class="sxs-lookup"><span data-stu-id="c8477-106">hello Azure Mobile Engagement service will be retired March 2018 and is currently only available tooexisting customers.</span></span> <span data-ttu-id="c8477-107">Mer information finns i [Mobile Engagement](https://azure.microsoft.com/en-us/services/mobile-engagement/).</span><span class="sxs-lookup"><span data-stu-id="c8477-107">For more information, see [Mobile Engagement](https://azure.microsoft.com/en-us/services/mobile-engagement/).</span></span>

<span data-ttu-id="c8477-108">Den här kursen kräver hello följande:</span><span class="sxs-lookup"><span data-stu-id="c8477-108">This tutorial requires hello following:</span></span>

* <span data-ttu-id="c8477-109">[Xamarin Studio](http://xamarin.com/studio).</span><span class="sxs-lookup"><span data-stu-id="c8477-109">[Xamarin Studio](http://xamarin.com/studio).</span></span> <span data-ttu-id="c8477-110">Du kan även använda Visual Studio med Xamarin men i den här kursen används Xamarin Studio.</span><span class="sxs-lookup"><span data-stu-id="c8477-110">You can also use Visual Studio with Xamarin but this tutorial uses Xamarin Studio.</span></span> <span data-ttu-id="c8477-111">Det finns installationsanvisningar i avsnittet om [konfiguration och installation för Visual Studio och Xamarin](https://msdn.microsoft.com/library/mt613162.aspx).</span><span class="sxs-lookup"><span data-stu-id="c8477-111">For installation instructions, see [Setup and Install for Visual Studio and Xamarin](https://msdn.microsoft.com/library/mt613162.aspx).</span></span> 
* [<span data-ttu-id="c8477-112">Mobile Engagement Xamarin SDK</span><span class="sxs-lookup"><span data-stu-id="c8477-112">Mobile Engagement Xamarin SDK</span></span>](https://www.nuget.org/packages/Microsoft.Azure.Engagement.Xamarin/)

> [!NOTE]
> <span data-ttu-id="c8477-113">toocomplete den här självstudiekursen kommer du måste ha ett aktivt Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="c8477-113">toocomplete this tutorial, you must have an active Azure account.</span></span> <span data-ttu-id="c8477-114">Om du inte har något konto kan skapa du ett kostnadsfritt utvärderingskonto på bara några minuter.</span><span class="sxs-lookup"><span data-stu-id="c8477-114">If you don't have an account, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="c8477-115">Mer information om den [kostnadsfria utvärderingsversionen av Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-xamarin-ios-get-started).</span><span class="sxs-lookup"><span data-stu-id="c8477-115">For details, see [Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-xamarin-ios-get-started).</span></span>
> 
> 

## <span data-ttu-id="c8477-116"><a id="setup-azme"></a>Konfigurera Mobile Engagement för din iOS-app</span><span class="sxs-lookup"><span data-stu-id="c8477-116"><a id="setup-azme"></a>Setup Mobile Engagement for your iOS app</span></span>
[!INCLUDE [Create Mobile Engagement App in Portal](../../includes/mobile-engagement-create-app-in-portal-new.md)]

## <span data-ttu-id="c8477-117"><a id="connecting-app"></a>Ansluta appen toohello Mobile Engagement-serverdelen</span><span class="sxs-lookup"><span data-stu-id="c8477-117"><a id="connecting-app"></a>Connect your app toohello Mobile Engagement backend</span></span>
<span data-ttu-id="c8477-118">Den här kursen behandlas en ”grundläggande integration” som hello minimal ange nödvändiga toocollect data och skicka ett push-meddelande.</span><span class="sxs-lookup"><span data-stu-id="c8477-118">This tutorial presents a "basic integration" which is hello minimal set required toocollect data and send a push notification.</span></span>

<span data-ttu-id="c8477-119">Vi skapar en grundläggande app i Xamarin toodemonstrate hello integrering:</span><span class="sxs-lookup"><span data-stu-id="c8477-119">We will create a basic app with Xamarin toodemonstrate hello integration:</span></span>

### <a name="create-a-new-xamarinios-project"></a><span data-ttu-id="c8477-120">Skapa ett nytt Xamarin.iOS-projekt</span><span class="sxs-lookup"><span data-stu-id="c8477-120">Create a new Xamarin.iOS project</span></span>
1. <span data-ttu-id="c8477-121">Starta Xamarin Studio.</span><span class="sxs-lookup"><span data-stu-id="c8477-121">Launch Xamarin Studio.</span></span> <span data-ttu-id="c8477-122">Gå för**filen** -> **ny** -> **lösning**</span><span class="sxs-lookup"><span data-stu-id="c8477-122">Go too**File** -> **New** -> **Solution**</span></span> 
   
    ![][1]
2. <span data-ttu-id="c8477-123">Välj **enda visa App**, kontrollera att hello valda språket är **C#** och klicka sedan på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="c8477-123">Select **Single View App**, make sure hello selected language is **C#** and then click **Next**.</span></span>
   
    ![][2]
3. <span data-ttu-id="c8477-124">Fyll i hello **Appnamn** och hello **organisations-ID** och klicka sedan på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="c8477-124">Fill in hello **App Name** and hello **Organization Identifier** and then click **Next**.</span></span> 
   
    ![][3]
   
   > [!IMPORTANT]
   > <span data-ttu-id="c8477-125">Se till att hello Publicera profil som du använder toodeploy iOS-appen använder ett App-ID som matchar exakt med hello paket-ID som du har här.</span><span class="sxs-lookup"><span data-stu-id="c8477-125">Make sure that hello publishing profile you eventually use toodeploy your iOS app is using an App ID which matches exactly with hello Bundle Identifier you have here.</span></span> 
   > 
   > 
4. <span data-ttu-id="c8477-126">Uppdatera hello **projektnamn**, **lösningsnamn** och **plats** om krävs och klicka på **skapa**.</span><span class="sxs-lookup"><span data-stu-id="c8477-126">Update hello **Project Name**, **Solution Name** and **Location** if required and click **Create**.</span></span>
   
    ![][4]

<span data-ttu-id="c8477-127">Xamarin Studio skapar hello demoappen där Mobile Engagement ska integreras.</span><span class="sxs-lookup"><span data-stu-id="c8477-127">Xamarin Studio will create hello demo app in which we will integrate Mobile Engagement.</span></span> 

### <a name="connect-your-app-toomobile-engagement-backend"></a><span data-ttu-id="c8477-128">Ansluta appen tooMobile Engagement-serverdelen</span><span class="sxs-lookup"><span data-stu-id="c8477-128">Connect your app tooMobile Engagement backend</span></span>
1. <span data-ttu-id="c8477-129">Högerklicka på hello **paket** mapp i hello Lösningsfönstret och välj **lägga till paket...**</span><span class="sxs-lookup"><span data-stu-id="c8477-129">Right click hello **Packages** folder in hello Solution windows and select **Add Packages...**</span></span>
   
    ![][5]
2. <span data-ttu-id="c8477-130">Sök efter hello **Microsoft Azure Mobile Engagement Xamarin SDK** och lägga till den tooyour lösning.</span><span class="sxs-lookup"><span data-stu-id="c8477-130">Search for hello **Microsoft Azure Mobile Engagement Xamarin SDK** and add it tooyour solution.</span></span>  
   
    ![][6]
3. <span data-ttu-id="c8477-131">Öppna **AppDelegate.cs** och Lägg till hello följande med instruktionen:</span><span class="sxs-lookup"><span data-stu-id="c8477-131">Open **AppDelegate.cs** and add hello following using statement:</span></span>
   
        using Microsoft.Azure.Engagement.Xamarin;
4. <span data-ttu-id="c8477-132">I hello **FinishedLaunching** metod, lägga till hello följande tooinitialize hello anslutningen till Mobile Engagement-serverdelen.</span><span class="sxs-lookup"><span data-stu-id="c8477-132">In hello **FinishedLaunching** method, add hello following tooinitialize hello connection with Mobile Engagement backend.</span></span> <span data-ttu-id="c8477-133">Se till att tooadd din **ConnectionString**.</span><span class="sxs-lookup"><span data-stu-id="c8477-133">Make sure tooadd your **ConnectionString**.</span></span> <span data-ttu-id="c8477-134">Den här koden används även en **NotificationIcon** som läggs till av hello Mobile Engagement SDK kan tooreplace.</span><span class="sxs-lookup"><span data-stu-id="c8477-134">This code also uses a dummy **NotificationIcon** which is added by hello Mobile Engagement SDK which you may want tooreplace.</span></span> 
   
        EngagementConfiguration config = new EngagementConfiguration {
                        ConnectionString = "YourConnectionStringFromAzurePortal",
                        NotificationIcon = UIImage.FromBundle("close")
                    };
        EngagementAgent.Init (config);

## <span data-ttu-id="c8477-135"><a id="monitor"></a>Aktivera realtidsövervakning</span><span class="sxs-lookup"><span data-stu-id="c8477-135"><a id="monitor"></a>Enabling real-time monitoring</span></span>
<span data-ttu-id="c8477-136">Du måste skicka minst en skärm toohello Mobile Engagement-serverdelen i ordning toostart skicka data och säkerställa hello användarna är aktiva.</span><span class="sxs-lookup"><span data-stu-id="c8477-136">In order toostart sending data and ensuring hello users are active, you must send at least one screen toohello Mobile Engagement backend.</span></span>

1. <span data-ttu-id="c8477-137">Öppna **ViewController.cs** och Lägg till hello följande med instruktionen:</span><span class="sxs-lookup"><span data-stu-id="c8477-137">Open **ViewController.cs** and add hello following using statement:</span></span>
   
        using Microsoft.Azure.Engagement.Xamarin;
2. <span data-ttu-id="c8477-138">Ersätt hello klassen från vilken `ViewController` ärver från `UIViewController` för`EngagementViewController`.</span><span class="sxs-lookup"><span data-stu-id="c8477-138">Replace hello class from which `ViewController` inherits from `UIViewController` too`EngagementViewController`.</span></span> 

## <span data-ttu-id="c8477-139"><a id="monitor"></a>Anslut appen med realtidsövervakning</span><span class="sxs-lookup"><span data-stu-id="c8477-139"><a id="monitor"></a>Connect app with real-time monitoring</span></span>
[!INCLUDE [Connect app with real-time monitoring](../../includes/mobile-engagement-connect-app-with-monitor.md)]

## <span data-ttu-id="c8477-140"><a id="integrate-push"></a>Aktivera push-meddelanden och meddelanden i appen</span><span class="sxs-lookup"><span data-stu-id="c8477-140"><a id="integrate-push"></a>Enable push notifications and in-app messaging</span></span>
<span data-ttu-id="c8477-141">Mobile Engagement kan du toointeract med användarna och räckvidd med push-meddelanden och appmeddelanden i hello kontext kampanjer.</span><span class="sxs-lookup"><span data-stu-id="c8477-141">Mobile Engagement allows you toointeract with your users and REACH with push notifications and in-app messaging in hello context of campaigns.</span></span> <span data-ttu-id="c8477-142">Denna modul är heter REACH i hello Mobile Engagement-portalen.</span><span class="sxs-lookup"><span data-stu-id="c8477-142">This module is called REACH in hello Mobile Engagement portal.</span></span>
<span data-ttu-id="c8477-143">hello följande avsnitt konfigurerar din app tooreceive dem.</span><span class="sxs-lookup"><span data-stu-id="c8477-143">hello following sections set up your app tooreceive them.</span></span>

### <a name="modify-your-application-delegate"></a><span data-ttu-id="c8477-144">Ändra programdelegaten</span><span class="sxs-lookup"><span data-stu-id="c8477-144">Modify your Application Delegate</span></span>
1. <span data-ttu-id="c8477-145">Öppna hello **AppDelegate.cs** och Lägg till hello följande med instruktionen:</span><span class="sxs-lookup"><span data-stu-id="c8477-145">Open hello **AppDelegate.cs** and add hello following using statement:</span></span>
   
        using System; 
2. <span data-ttu-id="c8477-146">I hello `FinishedLaunching` metoden Lägg till hello följande tooregister för push-meddelanden efter`EngagementAgent.init(...)`</span><span class="sxs-lookup"><span data-stu-id="c8477-146">Now inside hello `FinishedLaunching` method, add hello following tooregister for push messages after `EngagementAgent.init(...)`</span></span>
   
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
3. <span data-ttu-id="c8477-147">Slutligen - uppdatera eller lägga till hello följande metoder:</span><span class="sxs-lookup"><span data-stu-id="c8477-147">Finally - update or add hello following methods:</span></span>
   
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
            Console.WriteLine("Failed tooregister for remote notifications: Error '{0}'", error);
        }
4. <span data-ttu-id="c8477-148">I din **Info.plist** fil i hello lösning, bekräfta att hello **Paketidentifierare** matchar hello **App-ID** du har i din etableringsprofil i hello Apple Dev Center.</span><span class="sxs-lookup"><span data-stu-id="c8477-148">In your **Info.plist** file in hello solution, confirm that hello **Bundle Identifier** matches with hello **App ID** you have in your provisioning profile in hello Apple Dev Center.</span></span> 
   
    ![][7]
5. <span data-ttu-id="c8477-149">Hej i samma **Info.plist** fil, se till att du har markerat hello **aktivera bakgrundslägen** och **Remote Notifications**.</span><span class="sxs-lookup"><span data-stu-id="c8477-149">In hello same **Info.plist** file, make sure that you have checked hello **Enable Background Modes** and **Remote Notifications**.</span></span> 
   
     ![][8]
6. <span data-ttu-id="c8477-150">Köra hello app på hello-enhet som du har associerat med den här publiceringsprofilen.</span><span class="sxs-lookup"><span data-stu-id="c8477-150">Run hello app on hello device you have associated with this publishing profile.</span></span> 

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
