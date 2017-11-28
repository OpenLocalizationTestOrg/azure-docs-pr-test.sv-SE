---
title: "aaaGet igång med Azure Mobile Engagement för iOS i Swift | Microsoft Docs"
description: "Lär dig hur toouse Azure Mobile Engagement med analyser och Push-meddelanden för iOS-appar."
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
ms.openlocfilehash: 9a3841d305745f8b80c6b0c86aabe18e0c7c0e59
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-azure-mobile-engagement-for-ios-apps-in-swift"></a><span data-ttu-id="442ac-103">Komma igång med Azure Mobile Engagement för iOS-appar i Swift</span><span class="sxs-lookup"><span data-stu-id="442ac-103">Get Started with Azure Mobile Engagement for iOS Apps in Swift</span></span>
[!INCLUDE [Hero tutorial switcher](../../includes/mobile-engagement-hero-tutorial-switcher.md)]

<span data-ttu-id="442ac-104">Det här avsnittet beskrivs hur du toouse Azure Mobile Engagement toounderstand din app användnings- och skicka push-meddelanden toosegmented användare tooan iOS-program.</span><span class="sxs-lookup"><span data-stu-id="442ac-104">This topic shows you how toouse Azure Mobile Engagement toounderstand your app usage and send push notifications toosegmented users tooan iOS application.</span></span>
<span data-ttu-id="442ac-105">I den här självstudiekursen skapar du en tom iOS-app som samlar in grundläggande data och tar emot push-meddelanden via Apple Push Notification System (APNS).</span><span class="sxs-lookup"><span data-stu-id="442ac-105">In this tutorial, you create a blank iOS app that collects basic data and receives push notifications using Apple Push Notification System (APNS).</span></span>

<span data-ttu-id="442ac-106">Den här kursen kräver hello följande:</span><span class="sxs-lookup"><span data-stu-id="442ac-106">This tutorial requires hello following:</span></span>

* <span data-ttu-id="442ac-107">XCode 8, som du kan installera från Mac App Store</span><span class="sxs-lookup"><span data-stu-id="442ac-107">XCode 8, which you can install from your MAC App Store</span></span>
* <span data-ttu-id="442ac-108">Hej [Mobile Engagement iOS SDK]</span><span class="sxs-lookup"><span data-stu-id="442ac-108">hello [Mobile Engagement iOS SDK]</span></span>
* <span data-ttu-id="442ac-109">Certifikat för push-meddelanden (.p12) som kan hämtas från Apple Dev Center</span><span class="sxs-lookup"><span data-stu-id="442ac-109">Push notification certificate (.p12) that you can obtain on your Apple Dev Center</span></span>

> [!NOTE]
> <span data-ttu-id="442ac-110">Den här kursen använder Swift version 3.0.</span><span class="sxs-lookup"><span data-stu-id="442ac-110">This tutorial uses Swift version 3.0.</span></span> 
> 
> 

<span data-ttu-id="442ac-111">Du måste slutföra den här självstudiekursen innan du påbörjar någon annan kurs om Mobile Engagement och iOS-appar.</span><span class="sxs-lookup"><span data-stu-id="442ac-111">Completing this tutorial is a prerequisite for all other Mobile Engagement tutorials for iOS apps.</span></span>

> [!NOTE]
> <span data-ttu-id="442ac-112">toocomplete den här självstudiekursen kommer du måste ha ett aktivt Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="442ac-112">toocomplete this tutorial, you must have an active Azure account.</span></span> <span data-ttu-id="442ac-113">Om du inte har något konto kan skapa du ett kostnadsfritt utvärderingskonto på bara några minuter.</span><span class="sxs-lookup"><span data-stu-id="442ac-113">If you don't have an account, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="442ac-114">Mer information om den [kostnadsfria utvärderingsversionen av Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-ios-swift-get-started).</span><span class="sxs-lookup"><span data-stu-id="442ac-114">For details, see [Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-ios-swift-get-started).</span></span>
> 
> 

## <span data-ttu-id="442ac-115"><a id="setup-azme"></a>Konfigurera Mobile Engagement för din iOS-app</span><span class="sxs-lookup"><span data-stu-id="442ac-115"><a id="setup-azme"></a>Setup Mobile Engagement for your iOS app</span></span>
[!INCLUDE [Create Mobile Engagement App in Portal](../../includes/mobile-engagement-create-app-in-portal-new.md)]

## <span data-ttu-id="442ac-116"><a id="connecting-app"></a>Ansluta appen toohello Mobile Engagement-serverdelen</span><span class="sxs-lookup"><span data-stu-id="442ac-116"><a id="connecting-app"></a>Connect your app toohello Mobile Engagement backend</span></span>
<span data-ttu-id="442ac-117">Den här kursen behandlas en ”grundläggande integration”, vilket är hello minimal ange nödvändiga toocollect data och skicka ett push-meddelande.</span><span class="sxs-lookup"><span data-stu-id="442ac-117">This tutorial presents a "basic integration", which is hello minimal set required toocollect data and send a push notification.</span></span> <span data-ttu-id="442ac-118">hello fullständiga integrationsdokumentationen finns i hello [Mobile Engagement iOS SDK-integration](mobile-engagement-ios-sdk-overview.md)</span><span class="sxs-lookup"><span data-stu-id="442ac-118">hello complete integration documentation can be found in hello [Mobile Engagement iOS SDK integration](mobile-engagement-ios-sdk-overview.md)</span></span>

<span data-ttu-id="442ac-119">Vi skapar en grundläggande app i XCode toodemonstrate hello integrering:</span><span class="sxs-lookup"><span data-stu-id="442ac-119">We will create a basic app with XCode toodemonstrate hello integration:</span></span>

### <a name="create-a-new-ios-project"></a><span data-ttu-id="442ac-120">Skapa ett nytt iOS-projekt</span><span class="sxs-lookup"><span data-stu-id="442ac-120">Create a new iOS project</span></span>
[!INCLUDE [Create a new iOS Project](../../includes/mobile-engagement-create-new-ios-app.md)]

### <a name="connect-your-app-toomobile-engagement-backend"></a><span data-ttu-id="442ac-121">Ansluta appen tooMobile Engagement-serverdelen</span><span class="sxs-lookup"><span data-stu-id="442ac-121">Connect your app tooMobile Engagement backend</span></span>
1. <span data-ttu-id="442ac-122">Hämta hello [Mobile Engagement iOS SDK]</span><span class="sxs-lookup"><span data-stu-id="442ac-122">Download hello [Mobile Engagement iOS SDK]</span></span>
2. <span data-ttu-id="442ac-123">Extrahera hello. tar.gz tooa mapp på datorn</span><span class="sxs-lookup"><span data-stu-id="442ac-123">Extract hello .tar.gz file tooa folder in your computer</span></span>
3. <span data-ttu-id="442ac-124">Högerklicka på hello projektet och välj ”Lägg till filer för...”</span><span class="sxs-lookup"><span data-stu-id="442ac-124">Right click hello project and select "Add files too..."</span></span>
   
    ![][1]
4. <span data-ttu-id="442ac-125">Navigera toohello mapp där du extraherade hello SDK och välj hello `EngagementSDK` mappen och tryck på OK.</span><span class="sxs-lookup"><span data-stu-id="442ac-125">Navigate toohello folder where you extracted hello SDK and select hello `EngagementSDK` folder then press OK.</span></span>
   
    ![][2]
5. <span data-ttu-id="442ac-126">Öppna hello `Build Phases` fliken och i hello `Link Binary With Libraries` menyn Lägg till hello ramverk enligt nedan:</span><span class="sxs-lookup"><span data-stu-id="442ac-126">Open hello `Build Phases` tab and in hello `Link Binary With Libraries` menu add hello frameworks as shown below:</span></span>
   
    ![][3]
6. <span data-ttu-id="442ac-127">Skapa en bryggning huvud toobe kan toouse hello SDK: ns Objective C-API: er genom att välja Arkiv > Nytt > fil > iOS > källa > huvudfil.</span><span class="sxs-lookup"><span data-stu-id="442ac-127">Create a Bridging header toobe able toouse hello SDK's Objective C APIs by choosing File > New > File > iOS > Source > Header File.</span></span>
   
    ![][4]
7. <span data-ttu-id="442ac-128">Redigera hello bryggning sidhuvud filen tooexpose Mobile Engagement Objective-C-koden tooyour Swift-kod, lägga till hello följande importer:</span><span class="sxs-lookup"><span data-stu-id="442ac-128">Edit hello bridging header file tooexpose Mobile Engagement Objective-C code tooyour Swift code, add hello following imports :</span></span>
   
        /* Mobile Engagement Agent */
        #import "AEModule.h"
        #import "AEPushMessage.h"
        #import "AEStorage.h"
        #import "EngagementAgent.h"
        #import "EngagementTableViewController.h"
        #import "EngagementViewController.h"
        #import "AEUserNotificationHandler.h"
        #import "AEIdfaProvider.h"
8. <span data-ttu-id="442ac-129">Kontrollera att hello Interimshuvudfilen med Objective-C skapa inställningen under Swift-kompilator – kodgenerering har en sökväg toothis rubrik under Skapa inställningar.</span><span class="sxs-lookup"><span data-stu-id="442ac-129">Under Build Settings, make sure hello Objective-C Bridging Header build setting under Swift Compiler - Code Generation has a path toothis header.</span></span> <span data-ttu-id="442ac-130">Här följer ett exempel på sökvägen: **$(SRCROOT)/MySuperApp/MySuperApp-Bridging-Header.h (beroende på hello sökväg)**</span><span class="sxs-lookup"><span data-stu-id="442ac-130">Here is a path example: **$(SRCROOT)/MySuperApp/MySuperApp-Bridging-Header.h (depending on hello path)**</span></span>
   
   ![][6]
9. <span data-ttu-id="442ac-131">Gå tillbaka toohello Azure-portalen via appsidan *anslutningsinformation* sida och kopiera hello anslutningssträngen</span><span class="sxs-lookup"><span data-stu-id="442ac-131">Go back toohello Azure portal in your app's *Connection Info* page and copy hello Connection String</span></span>
   
   ![][5]
10. <span data-ttu-id="442ac-132">Klistra in hello anslutningssträngen i hello `didFinishLaunchingWithOptions` delegera</span><span class="sxs-lookup"><span data-stu-id="442ac-132">Now paste hello connection string in hello `didFinishLaunchingWithOptions` delegate</span></span>
    
        func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplicationLaunchOptionsKey: Any]?) -> Bool
        {
              [...]
                EngagementAgent.init("Endpoint={YOUR_APP_COLLECTION.DOMAIN};SdkKey={YOUR_SDK_KEY};AppId={YOUR_APPID}")
              [...]
        }

## <span data-ttu-id="442ac-133"><a id="monitor"></a>Aktivera realtidsövervakning</span><span class="sxs-lookup"><span data-stu-id="442ac-133"><a id="monitor"></a>Enabling real-time monitoring</span></span>
<span data-ttu-id="442ac-134">Du måste skicka minst en skärm (aktivitet) toohello Mobile Engagement-serverdelen i ordning toostart skicka data och se till att hello användarna är aktiva.</span><span class="sxs-lookup"><span data-stu-id="442ac-134">In order toostart sending data and ensuring that hello users are active, you must send at least one screen (Activity) toohello Mobile Engagement backend.</span></span>

1. <span data-ttu-id="442ac-135">Öppna hello **ViewController.swift** filen och ersätter hello basklassen **ViewController** toobe **EngagementViewController**:</span><span class="sxs-lookup"><span data-stu-id="442ac-135">Open hello **ViewController.swift** file and replace hello base class of **ViewController** toobe **EngagementViewController**:</span></span>
   
    `class ViewController : EngagementViewController {`

## <span data-ttu-id="442ac-136"><a id="monitor"></a>Anslut appen med realtidsövervakning</span><span class="sxs-lookup"><span data-stu-id="442ac-136"><a id="monitor"></a>Connect app with real-time monitoring</span></span>
[!INCLUDE [Connect app with real-time monitoring](../../includes/mobile-engagement-connect-app-with-monitor.md)]

## <span data-ttu-id="442ac-137"><a id="integrate-push"></a>Aktivera push-meddelanden och meddelanden i appen</span><span class="sxs-lookup"><span data-stu-id="442ac-137"><a id="integrate-push"></a>Enabling Push Notifications and in-app messaging</span></span>
<span data-ttu-id="442ac-138">Mobile Engagement kan du toointeract och räckvidd med dina användare med Push-meddelanden och meddelanden i appen hello gäller kampanjer.</span><span class="sxs-lookup"><span data-stu-id="442ac-138">Mobile Engagement allows you toointeract and REACH with your users with Push Notifications and In-app Messaging in hello context of campaigns.</span></span> <span data-ttu-id="442ac-139">Denna modul är heter REACH i hello Mobile Engagement-portalen.</span><span class="sxs-lookup"><span data-stu-id="442ac-139">This module is called REACH in hello Mobile Engagement portal.</span></span>
<span data-ttu-id="442ac-140">hello följande avsnitt konfigurerar din app tooreceive dem.</span><span class="sxs-lookup"><span data-stu-id="442ac-140">hello following sections will setup your app tooreceive them.</span></span>

### <a name="enable-your-app-tooreceive-silent-push-notifications"></a><span data-ttu-id="442ac-141">Aktivera din app tooreceive tysta Push-meddelanden</span><span class="sxs-lookup"><span data-stu-id="442ac-141">Enable your app tooreceive Silent Push Notifications</span></span>
[!INCLUDE [mobile-engagement-ios-silent-push](../../includes/mobile-engagement-ios-silent-push.md)]

### <a name="add-hello-reach-library-tooyour-project"></a><span data-ttu-id="442ac-142">Lägga till hello Reach-biblioteket tooyour projekt</span><span class="sxs-lookup"><span data-stu-id="442ac-142">Add hello Reach library tooyour project</span></span>
1. <span data-ttu-id="442ac-143">Högerklicka på ditt projekt</span><span class="sxs-lookup"><span data-stu-id="442ac-143">Right click your project</span></span>
2. <span data-ttu-id="442ac-144">Välj `Add file too...`</span><span class="sxs-lookup"><span data-stu-id="442ac-144">Select `Add file too...`</span></span>
3. <span data-ttu-id="442ac-145">Navigera toohello mapp där du extraherade hello SDK</span><span class="sxs-lookup"><span data-stu-id="442ac-145">Navigate toohello folder where you extracted hello SDK</span></span>
4. <span data-ttu-id="442ac-146">Välj hello `EngagementReach` mapp</span><span class="sxs-lookup"><span data-stu-id="442ac-146">Select hello `EngagementReach` folder</span></span>
5. <span data-ttu-id="442ac-147">Klicka på Lägg till</span><span class="sxs-lookup"><span data-stu-id="442ac-147">Click Add</span></span>
6. <span data-ttu-id="442ac-148">Redigera hello bryggning sidhuvud filen tooexpose Mobile Engagement Objective-C nå sidhuvuden och Lägg till hello följande importer:</span><span class="sxs-lookup"><span data-stu-id="442ac-148">Edit hello bridging header file tooexpose Mobile Engagement Objective-C Reach headers and add hello following imports :</span></span>
   
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

### <a name="modify-your-application-delegate"></a><span data-ttu-id="442ac-149">Ändra programdelegaten</span><span class="sxs-lookup"><span data-stu-id="442ac-149">Modify your Application Delegate</span></span>
1. <span data-ttu-id="442ac-150">I hello `didFinishLaunchingWithOptions` – skapar en räckviddsmodul och skickar den tooyour befintliga initieringsrad:</span><span class="sxs-lookup"><span data-stu-id="442ac-150">Inside  hello `didFinishLaunchingWithOptions` -  create a reach module and pass it tooyour existing Engagement initialization line:</span></span>
   
        func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplicationLaunchOptionsKey: Any]?) -> Bool 
        {
            let reach = AEReachModule.module(withNotificationIcon: UIImage(named:"icon.png")) as! AEReachModule
            EngagementAgent.init("Endpoint={YOUR_APP_COLLECTION.DOMAIN};SdkKey={YOUR_SDK_KEY};AppId={YOUR_APPID}", modulesArray:[reach])
            [...]
            return true
        }

### <a name="enable-your-app-tooreceive-apns-push-notifications"></a><span data-ttu-id="442ac-151">Aktivera din app tooreceive APN Push-meddelanden</span><span class="sxs-lookup"><span data-stu-id="442ac-151">Enable your app tooreceive APNS Push Notifications</span></span>
1. <span data-ttu-id="442ac-152">Lägg till följande rad toohello hello `didFinishLaunchingWithOptions` metoden:</span><span class="sxs-lookup"><span data-stu-id="442ac-152">Add hello following line toohello `didFinishLaunchingWithOptions` method:</span></span>
   
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
2. <span data-ttu-id="442ac-153">Lägg till hello `didRegisterForRemoteNotificationsWithDeviceToken` metoden på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="442ac-153">Add hello `didRegisterForRemoteNotificationsWithDeviceToken` method as follows:</span></span>
   
        func application(_ application: UIApplication, didRegisterForRemoteNotificationsWithDeviceToken deviceToken: Data) {
            EngagementAgent.shared().registerDeviceToken(deviceToken)
        }
3. <span data-ttu-id="442ac-154">Lägg till hello `didReceiveRemoteNotification:fetchCompletionHandler:` metoden på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="442ac-154">Add hello `didReceiveRemoteNotification:fetchCompletionHandler:` method as follows:</span></span>
   
        func application(_ application: UIApplication, didReceiveRemoteNotification userInfo: [AnyHashable : Any], fetchCompletionHandler completionHandler: @escaping (UIBackgroundFetchResult) -> Void) {
            EngagementAgent.shared().applicationDidReceiveRemoteNotification(userInfo, fetchCompletionHandler:completionHandler)
        }

[!INCLUDE [mobile-engagement-ios-send-push-push](../../includes/mobile-engagement-ios-send-push.md)]

<!-- URLs. -->
[Mobile Engagement iOS SDK]: http://aka.ms/qk2rnj

<!-- Images. -->
[1]: ./media/mobile-engagement-ios-get-started/xcode-add-files.png
[2]: ./media/mobile-engagement-ios-get-started/xcode-select-engagement-sdk.png
[3]: ./media/mobile-engagement-ios-get-started/xcode-build-phases.png
[4]: ./media/mobile-engagement-ios-swift-get-started/add-header-file.png
[5]: ./media/mobile-engagement-ios-get-started/app-connection-info-page.png
[6]: ./media/mobile-engagement-ios-swift-get-started/add-bridging-header.png
