---
title: "Komma igång med Azure Mobile Engagement för iOS i Swift | Microsoft Docs"
description: "Lär dig hur du använder Azure Mobile Engagement med analyser och push-meddelanden för iOS-appar."
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: 196c282d-6f2f-4cbc-aeee-6517c5ad866d
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-ios
ms.devlang: swift
ms.topic: hero-article
ms.date: 09/20/2016
ms.author: piyushjo
ms.openlocfilehash: 1011b9823333e79a52cd2d187df4f8d063b1f799
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="get-started-with-azure-mobile-engagement-for-ios-apps-in-swift"></a><span data-ttu-id="5ea08-103">Komma igång med Azure Mobile Engagement för iOS-appar i Swift</span><span class="sxs-lookup"><span data-stu-id="5ea08-103">Get Started with Azure Mobile Engagement for iOS Apps in Swift</span></span>
[!INCLUDE [Hero tutorial switcher](../../includes/mobile-engagement-hero-tutorial-switcher.md)]

<span data-ttu-id="5ea08-104">I det här avsnittet beskrivs hur du använder Azure Mobile Engagement för att förstå appanvändningen, och hur du skickar push-meddelanden till segmenterade användare i ett iOS-program.</span><span class="sxs-lookup"><span data-stu-id="5ea08-104">This topic shows you how to use Azure Mobile Engagement to understand your app usage and send push notifications to segmented users to an iOS application.</span></span>
<span data-ttu-id="5ea08-105">I den här självstudiekursen skapar du en tom iOS-app som samlar in grundläggande data och tar emot push-meddelanden via Apple Push Notification System (APNS).</span><span class="sxs-lookup"><span data-stu-id="5ea08-105">In this tutorial, you create a blank iOS app that collects basic data and receives push notifications using Apple Push Notification System (APNS).</span></span>

<span data-ttu-id="5ea08-106">Följande krävs för den här självstudiekursen:</span><span class="sxs-lookup"><span data-stu-id="5ea08-106">This tutorial requires the following:</span></span>

* <span data-ttu-id="5ea08-107">XCode 8, som du kan installera från Mac App Store</span><span class="sxs-lookup"><span data-stu-id="5ea08-107">XCode 8, which you can install from your MAC App Store</span></span>
* <span data-ttu-id="5ea08-108">[Mobile Engagement iOS SDK]</span><span class="sxs-lookup"><span data-stu-id="5ea08-108">the [Mobile Engagement iOS SDK]</span></span>
* <span data-ttu-id="5ea08-109">Certifikat för push-meddelanden (.p12) som kan hämtas från Apple Dev Center</span><span class="sxs-lookup"><span data-stu-id="5ea08-109">Push notification certificate (.p12) that you can obtain on your Apple Dev Center</span></span>

> [!NOTE]
> <span data-ttu-id="5ea08-110">Den här kursen använder Swift version 3.0.</span><span class="sxs-lookup"><span data-stu-id="5ea08-110">This tutorial uses Swift version 3.0.</span></span> 
> 
> 

<span data-ttu-id="5ea08-111">Du måste slutföra den här självstudiekursen innan du påbörjar någon annan kurs om Mobile Engagement och iOS-appar.</span><span class="sxs-lookup"><span data-stu-id="5ea08-111">Completing this tutorial is a prerequisite for all other Mobile Engagement tutorials for iOS apps.</span></span>

> [!NOTE]
> <span data-ttu-id="5ea08-112">Du måste ha ett aktivt Azure-konto för att slutföra den här kursen.</span><span class="sxs-lookup"><span data-stu-id="5ea08-112">To complete this tutorial, you must have an active Azure account.</span></span> <span data-ttu-id="5ea08-113">Om du inte har något konto kan skapa du ett kostnadsfritt utvärderingskonto på bara några minuter.</span><span class="sxs-lookup"><span data-stu-id="5ea08-113">If you don't have an account, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="5ea08-114">Mer information om den [kostnadsfria utvärderingsversionen av Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-ios-swift-get-started).</span><span class="sxs-lookup"><span data-stu-id="5ea08-114">For details, see [Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-ios-swift-get-started).</span></span>
> 
> 

## <span data-ttu-id="5ea08-115"><a id="setup-azme"></a>Konfigurera Mobile Engagement för din iOS-app</span><span class="sxs-lookup"><span data-stu-id="5ea08-115"><a id="setup-azme"></a>Setup Mobile Engagement for your iOS app</span></span>
[!INCLUDE [Create Mobile Engagement App in Portal](../../includes/mobile-engagement-create-app-in-portal-new.md)]

## <span data-ttu-id="5ea08-116"><a id="connecting-app"></a>Anslut appen till Mobile Engagement-serverdelen</span><span class="sxs-lookup"><span data-stu-id="5ea08-116"><a id="connecting-app"></a>Connect your app to the Mobile Engagement backend</span></span>
<span data-ttu-id="5ea08-117">I den här kursen behandlas en ”grundläggande integration”, vilket är den minsta uppsättningen som krävs för att samla in data och skicka ett push-meddelande.</span><span class="sxs-lookup"><span data-stu-id="5ea08-117">This tutorial presents a "basic integration", which is the minimal set required to collect data and send a push notification.</span></span> <span data-ttu-id="5ea08-118">Den fullständiga integrationsdokumentationen finns i [Mobile Engagement iOS SDK-integration](mobile-engagement-ios-sdk-overview.md)</span><span class="sxs-lookup"><span data-stu-id="5ea08-118">The complete integration documentation can be found in the [Mobile Engagement iOS SDK integration](mobile-engagement-ios-sdk-overview.md)</span></span>

<span data-ttu-id="5ea08-119">Vi skapar en grundläggande app i XCode för att demonstrera integrationen:</span><span class="sxs-lookup"><span data-stu-id="5ea08-119">We will create a basic app with XCode to demonstrate the integration:</span></span>

### <a name="create-a-new-ios-project"></a><span data-ttu-id="5ea08-120">Skapa ett nytt iOS-projekt</span><span class="sxs-lookup"><span data-stu-id="5ea08-120">Create a new iOS project</span></span>
[!INCLUDE [Create a new iOS Project](../../includes/mobile-engagement-create-new-ios-app.md)]

### <a name="connect-your-app-to-mobile-engagement-backend"></a><span data-ttu-id="5ea08-121">Ansluta appen till Mobile Engagement-serverdelen</span><span class="sxs-lookup"><span data-stu-id="5ea08-121">Connect your app to Mobile Engagement backend</span></span>
1. <span data-ttu-id="5ea08-122">Ladda ned [Mobile Engagement iOS SDK]</span><span class="sxs-lookup"><span data-stu-id="5ea08-122">Download the [Mobile Engagement iOS SDK]</span></span>
2. <span data-ttu-id="5ea08-123">Extrahera .tar.gz-filen till en mapp på datorn</span><span class="sxs-lookup"><span data-stu-id="5ea08-123">Extract the .tar.gz file to a folder in your computer</span></span>
3. <span data-ttu-id="5ea08-124">Högerklicka på projektet och välj sedan Lägg till filer i ...</span><span class="sxs-lookup"><span data-stu-id="5ea08-124">Right click the project and select "Add files to ..."</span></span>
   
    ![][1]
4. <span data-ttu-id="5ea08-125">Navigera till mappen dit du extraherade SDK, markera mappen `EngagementSDK` och klicka sedan på OK.</span><span class="sxs-lookup"><span data-stu-id="5ea08-125">Navigate to the folder where you extracted the SDK and select the `EngagementSDK` folder then press OK.</span></span>
   
    ![][2]
5. <span data-ttu-id="5ea08-126">Öppna fliken `Build Phases` och lägg till ramverk enligt nedan med hjälp av menyn `Link Binary With Libraries`:</span><span class="sxs-lookup"><span data-stu-id="5ea08-126">Open the `Build Phases` tab and in the `Link Binary With Libraries` menu add the frameworks as shown below:</span></span>
   
    ![][3]
6. <span data-ttu-id="5ea08-127">Skapa en interimshuvudfil för att kunna använda SDK:ns Objective C-API:er genom att välja Arkiv > Ny(tt) > Fil > iOS > Källa > Huvudfil.</span><span class="sxs-lookup"><span data-stu-id="5ea08-127">Create a Bridging header to be able to use the SDK's Objective C APIs by choosing File > New > File > iOS > Source > Header File.</span></span>
   
    ![][4]
7. <span data-ttu-id="5ea08-128">Redigera interimshuvudfilen och exponera Objective-C-koden i Mobile Engagement för Swift-koden genom att lägga till följande importer:</span><span class="sxs-lookup"><span data-stu-id="5ea08-128">Edit the bridging header file to expose Mobile Engagement Objective-C code to your Swift code, add the following imports :</span></span>
   
        /* Mobile Engagement Agent */
        #import "AEModule.h"
        #import "AEPushMessage.h"
        #import "AEStorage.h"
        #import "EngagementAgent.h"
        #import "EngagementTableViewController.h"
        #import "EngagementViewController.h"
        #import "AEUserNotificationHandler.h"
        #import "AEIdfaProvider.h"
8. <span data-ttu-id="5ea08-129">Gå till Build Settings (Versionsinställningar) och se till att inställningen för interimshuvudfilen med Objective-C har en sökväg till huvudfilen under Swift Compiler – Code Generation (Swift-kompilator – Kodgenerering).</span><span class="sxs-lookup"><span data-stu-id="5ea08-129">Under Build Settings, make sure the Objective-C Bridging Header build setting under Swift Compiler - Code Generation has a path to this header.</span></span> <span data-ttu-id="5ea08-130">Här följer ett exempel på en sökväg: **$(SRCROOT)/MySuperApp/MySuperApp-Bridging-Header.h (beroende på sökvägen)**</span><span class="sxs-lookup"><span data-stu-id="5ea08-130">Here is a path example: **$(SRCROOT)/MySuperApp/MySuperApp-Bridging-Header.h (depending on the path)**</span></span>
   
   ![][6]
9. <span data-ttu-id="5ea08-131">Gå tillbaka till Azure-portalen via appsidan *Anslutningsinformation* och kopiera anslutningssträngen</span><span class="sxs-lookup"><span data-stu-id="5ea08-131">Go back to the Azure portal in your app's *Connection Info* page and copy the Connection String</span></span>
   
   ![][5]
10. <span data-ttu-id="5ea08-132">Klistra in anslutningssträngen i delegaten `didFinishLaunchingWithOptions`</span><span class="sxs-lookup"><span data-stu-id="5ea08-132">Now paste the connection string in the `didFinishLaunchingWithOptions` delegate</span></span>
    
        func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplicationLaunchOptionsKey: Any]?) -> Bool
        {
              [...]
                EngagementAgent.init("Endpoint={YOUR_APP_COLLECTION.DOMAIN};SdkKey={YOUR_SDK_KEY};AppId={YOUR_APPID}")
              [...]
        }

## <span data-ttu-id="5ea08-133"><a id="monitor"></a>Aktivera realtidsövervakning</span><span class="sxs-lookup"><span data-stu-id="5ea08-133"><a id="monitor"></a>Enabling real-time monitoring</span></span>
<span data-ttu-id="5ea08-134">För att kunna börja skicka data och försäkra dig om att användarna är aktiva måste du skicka minst en skärm (aktivitet) till Mobile Engagement-serverdelen.</span><span class="sxs-lookup"><span data-stu-id="5ea08-134">In order to start sending data and ensuring that the users are active, you must send at least one screen (Activity) to the Mobile Engagement backend.</span></span>

1. <span data-ttu-id="5ea08-135">Öppna filen **ViewController.swift** byt ut basklassen **ViewController** mot **EngagementViewController**:</span><span class="sxs-lookup"><span data-stu-id="5ea08-135">Open the **ViewController.swift** file and replace the base class of **ViewController** to be **EngagementViewController**:</span></span>
   
    `class ViewController : EngagementViewController {`

## <span data-ttu-id="5ea08-136"><a id="monitor"></a>Anslut appen med realtidsövervakning</span><span class="sxs-lookup"><span data-stu-id="5ea08-136"><a id="monitor"></a>Connect app with real-time monitoring</span></span>
[!INCLUDE [Connect app with real-time monitoring](../../includes/mobile-engagement-connect-app-with-monitor.md)]

## <span data-ttu-id="5ea08-137"><a id="integrate-push"></a>Aktivera push-meddelanden och meddelanden i appen</span><span class="sxs-lookup"><span data-stu-id="5ea08-137"><a id="integrate-push"></a>Enabling Push Notifications and in-app messaging</span></span>
<span data-ttu-id="5ea08-138">Med Mobile Engagement kan du samverka med användarna, nå ut till dem och köra kampanjer med push-meddelanden och meddelanden i appen.</span><span class="sxs-lookup"><span data-stu-id="5ea08-138">Mobile Engagement allows you to interact and REACH with your users with Push Notifications and In-app Messaging in the context of campaigns.</span></span> <span data-ttu-id="5ea08-139">Modulen som används för det heter REACH och finns i Mobile Engagement-portalen.</span><span class="sxs-lookup"><span data-stu-id="5ea08-139">This module is called REACH in the Mobile Engagement portal.</span></span>
<span data-ttu-id="5ea08-140">I följande avsnitt konfigurerar du appen för att ta emot dem.</span><span class="sxs-lookup"><span data-stu-id="5ea08-140">The following sections will setup your app to receive them.</span></span>

### <a name="enable-your-app-to-receive-silent-push-notifications"></a><span data-ttu-id="5ea08-141">Konfigurera appen för att ta emot tysta push-meddelanden</span><span class="sxs-lookup"><span data-stu-id="5ea08-141">Enable your app to receive Silent Push Notifications</span></span>
[!INCLUDE [mobile-engagement-ios-silent-push](../../includes/mobile-engagement-ios-silent-push.md)]

### <a name="add-the-reach-library-to-your-project"></a><span data-ttu-id="5ea08-142">Lägg till Reach-biblioteket i projektet</span><span class="sxs-lookup"><span data-stu-id="5ea08-142">Add the Reach library to your project</span></span>
1. <span data-ttu-id="5ea08-143">Högerklicka på ditt projekt</span><span class="sxs-lookup"><span data-stu-id="5ea08-143">Right click your project</span></span>
2. <span data-ttu-id="5ea08-144">Välj `Add file to ...`</span><span class="sxs-lookup"><span data-stu-id="5ea08-144">Select `Add file to ...`</span></span>
3. <span data-ttu-id="5ea08-145">Navigera till mappen dit du extraherade SDK</span><span class="sxs-lookup"><span data-stu-id="5ea08-145">Navigate to the folder where you extracted the SDK</span></span>
4. <span data-ttu-id="5ea08-146">Välj mappen `EngagementReach`</span><span class="sxs-lookup"><span data-stu-id="5ea08-146">Select the `EngagementReach` folder</span></span>
5. <span data-ttu-id="5ea08-147">Klicka på Lägg till</span><span class="sxs-lookup"><span data-stu-id="5ea08-147">Click Add</span></span>
6. <span data-ttu-id="5ea08-148">Redigera interimshuvudfilen och exponera Objective-C-koden i Mobile Engagement med Reach-huvuden genom att lägga till följande importer:</span><span class="sxs-lookup"><span data-stu-id="5ea08-148">Edit the bridging header file to expose Mobile Engagement Objective-C Reach headers and add the following imports :</span></span>
   
        /* Mobile Engagement Reach */
        #import "AEAnnouncementViewController.h"
        #import "AEAutorotateView.h"
        #import "AEContentViewController.h"
        #import "AEDefaultAnnouncementViewController.h"
        #import "AEDefaultNotifier.h"
        #import "AEDefaultPollViewController.h"
        #import "AEInteractiveContent.h"
        #import "AENotificationView.h"
        #import "AENotifier.h"
        #import "AEPollViewController.h"
        #import "AEReachAbstractAnnouncement.h"
        #import "AEReachAnnouncement.h"
        #import "AEReachContent.h"
        #import "AEReachDataPush.h"
        #import "AEReachDataPushDelegate.h"
        #import "AEReachModule.h"
        #import "AEReachNotifAnnouncement.h"
        #import "AEReachPoll.h"
        #import "AEReachPollQuestion.h"
        #import "AEViewControllerUtil.h"
        #import "AEWebAnnouncementJsBridge.h"

### <a name="modify-your-application-delegate"></a><span data-ttu-id="5ea08-149">Ändra programdelegaten</span><span class="sxs-lookup"><span data-stu-id="5ea08-149">Modify your Application Delegate</span></span>
1. <span data-ttu-id="5ea08-150">Inifrån metoden `didFinishLaunchingWithOptions` skapar du en räckviddsmodul och skickar den till din befintliga initieringsrad för Engagement:</span><span class="sxs-lookup"><span data-stu-id="5ea08-150">Inside  the `didFinishLaunchingWithOptions` -  create a reach module and pass it to your existing Engagement initialization line:</span></span>
   
        func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplicationLaunchOptionsKey: Any]?) -> Bool 
        {
            let reach = AEReachModule.module(withNotificationIcon: UIImage(named:"icon.png")) as! AEReachModule
            EngagementAgent.init("Endpoint={YOUR_APP_COLLECTION.DOMAIN};SdkKey={YOUR_SDK_KEY};AppId={YOUR_APPID}", modulesArray:[reach])
            [...]
            return true
        }

### <a name="enable-your-app-to-receive-apns-push-notifications"></a><span data-ttu-id="5ea08-151">Konfigurera appen för att ta emot push-meddelanden med APNS</span><span class="sxs-lookup"><span data-stu-id="5ea08-151">Enable your app to receive APNS Push Notifications</span></span>
1. <span data-ttu-id="5ea08-152">Lägg till följande rad i metoden `didFinishLaunchingWithOptions`:</span><span class="sxs-lookup"><span data-stu-id="5ea08-152">Add the following line to the `didFinishLaunchingWithOptions` method:</span></span>
   
        if #available(iOS 8.0, *)
        {
            if #available(iOS 10.0, *)
            {
                UNUserNotificationCenter.current().requestAuthorization(options: [.alert, .badge, .sound]) { (granted, error) in }
            }else
            {
                let settings = UIUserNotificationSettings(types: [.alert, .badge, .sound], categories: nil)
                application.registerUserNotificationSettings(settings)
            }
            application.registerForRemoteNotifications()
        }
        else
        {
            application.registerForRemoteNotifications(matching: [.alert, .badge, .sound])
        }
2. <span data-ttu-id="5ea08-153">Lägg till `didRegisterForRemoteNotificationsWithDeviceToken`-metoden på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="5ea08-153">Add the `didRegisterForRemoteNotificationsWithDeviceToken` method as follows:</span></span>
   
        func application(_ application: UIApplication, didRegisterForRemoteNotificationsWithDeviceToken deviceToken: Data) {
            EngagementAgent.shared().registerDeviceToken(deviceToken)
        }
3. <span data-ttu-id="5ea08-154">Lägg till `didReceiveRemoteNotification:fetchCompletionHandler:`-metoden på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="5ea08-154">Add the `didReceiveRemoteNotification:fetchCompletionHandler:` method as follows:</span></span>
   
        func application(_ application: UIApplication, didReceiveRemoteNotification userInfo: [AnyHashable : Any], fetchCompletionHandler completionHandler: @escaping (UIBackgroundFetchResult) -> Void) {
            EngagementAgent.shared().applicationDidReceiveRemoteNotification(userInfo, fetchCompletionHandler:completionHandler)
        }

[!INCLUDE [mobile-engagement-ios-send-push-push](../../includes/mobile-engagement-ios-send-push.md)]

<!-- URLs. -->
<span data-ttu-id="5ea08-155">[Mobile Engagement iOS SDK]: http://aka.ms/qk2rnj</span><span class="sxs-lookup"><span data-stu-id="5ea08-155">[Mobile Engagement iOS SDK]: http://aka.ms/qk2rnj</span></span>

<!-- Images. -->
[1]: ./media/mobile-engagement-ios-get-started/xcode-add-files.png
[2]: ./media/mobile-engagement-ios-get-started/xcode-select-engagement-sdk.png
[3]: ./media/mobile-engagement-ios-get-started/xcode-build-phases.png
[4]: ./media/mobile-engagement-ios-swift-get-started/add-header-file.png
[5]: ./media/mobile-engagement-ios-get-started/app-connection-info-page.png
[6]: ./media/mobile-engagement-ios-swift-get-started/add-bridging-header.png
