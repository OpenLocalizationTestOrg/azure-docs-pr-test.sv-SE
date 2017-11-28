---
title: "aaaGet igång med Azure Mobile Engagement för iOS i Objective C | Microsoft Docs"
description: "Lär dig hur toouse Azure Mobile Engagement med analyser och push-meddelanden för iOS-appar."
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: 117b5742-522b-41de-98c5-f432da2d98f8
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-ios
ms.devlang: objective-c
ms.topic: hero-article
ms.date: 07/17/2017
ms.author: piyushjo
ms.openlocfilehash: 51a5013f23d2d04a7b9b30c83b47017898b2bb00
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-azure-mobile-engagement-for-ios-apps-in-objective-c"></a><span data-ttu-id="754ac-103">Komma igång med Azure Mobile Engagement för iOS-appar i Objective C</span><span class="sxs-lookup"><span data-stu-id="754ac-103">Get Started with Azure Mobile Engagement for iOS apps in Objective C</span></span>
[!INCLUDE [Hero tutorial switcher](../../includes/mobile-engagement-hero-tutorial-switcher.md)]

<span data-ttu-id="754ac-104">Det här avsnittet beskrivs hur du toouse Azure Mobile Engagement toounderstand din app användnings- och skicka push-meddelanden toosegmented användare tooan iOS-program.</span><span class="sxs-lookup"><span data-stu-id="754ac-104">This topic shows you how toouse Azure Mobile Engagement toounderstand your app usage and send push notifications toosegmented users tooan iOS application.</span></span>
<span data-ttu-id="754ac-105">I den här självstudiekursen skapar du en tom iOS-app som samlar in grundläggande data och tar emot push-meddelanden via Apple Push Notification System (APNS).</span><span class="sxs-lookup"><span data-stu-id="754ac-105">In this tutorial, you create a blank iOS app that collects basic data and receives push notifications using Apple Push Notification System (APNS).</span></span>

<span data-ttu-id="754ac-106">Den här kursen kräver hello följande:</span><span class="sxs-lookup"><span data-stu-id="754ac-106">This tutorial requires hello following:</span></span>

* <span data-ttu-id="754ac-107">XCode 8, som du kan installera från Mac App Store</span><span class="sxs-lookup"><span data-stu-id="754ac-107">XCode 8, which you can install from your MAC App Store</span></span>
* <span data-ttu-id="754ac-108">Hej [Mobile Engagement iOS SDK]</span><span class="sxs-lookup"><span data-stu-id="754ac-108">hello [Mobile Engagement iOS SDK]</span></span>

<span data-ttu-id="754ac-109">Du måste slutföra den här självstudiekursen innan du påbörjar någon annan kurs om Mobile Engagement och iOS-appar.</span><span class="sxs-lookup"><span data-stu-id="754ac-109">Completing this tutorial is a prerequisite for all other Mobile Engagement tutorials for iOS apps.</span></span>

> [!NOTE]
> <span data-ttu-id="754ac-110">toocomplete den här självstudiekursen kommer du måste ha ett aktivt Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="754ac-110">toocomplete this tutorial, you must have an active Azure account.</span></span> <span data-ttu-id="754ac-111">Om du inte har något konto kan skapa du ett kostnadsfritt utvärderingskonto på bara några minuter.</span><span class="sxs-lookup"><span data-stu-id="754ac-111">If you don't have an account, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="754ac-112">Mer information om den [kostnadsfria utvärderingsversionen av Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-ios-get-started).</span><span class="sxs-lookup"><span data-stu-id="754ac-112">For details, see [Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-ios-get-started).</span></span>
>
>

## <span data-ttu-id="754ac-113"><a id="setup-azme"></a>Konfigurera Mobile Engagement för din iOS-app</span><span class="sxs-lookup"><span data-stu-id="754ac-113"><a id="setup-azme"></a>Setup Mobile Engagement for your iOS app</span></span>
[!INCLUDE [Create Mobile Engagement App in Portal](../../includes/mobile-engagement-create-app-in-portal-new.md)]

## <span data-ttu-id="754ac-114"><a id="connecting-app"></a>Ansluta appen toohello Mobile Engagement-serverdelen</span><span class="sxs-lookup"><span data-stu-id="754ac-114"><a id="connecting-app"></a>Connect your app toohello Mobile Engagement backend</span></span>
<span data-ttu-id="754ac-115">Den här kursen behandlas en ”grundläggande integration”, vilket är hello minimal ange nödvändiga toocollect data och skicka ett push-meddelande.</span><span class="sxs-lookup"><span data-stu-id="754ac-115">This tutorial presents a "basic integration", which is hello minimal set required toocollect data and send a push notification.</span></span> <span data-ttu-id="754ac-116">hello fullständiga integrationsdokumentationen finns i hello [Mobile Engagement iOS SDK-integration](mobile-engagement-ios-sdk-overview.md)</span><span class="sxs-lookup"><span data-stu-id="754ac-116">hello complete integration documentation can be found in hello [Mobile Engagement iOS SDK integration](mobile-engagement-ios-sdk-overview.md)</span></span>

<span data-ttu-id="754ac-117">Vi skapar en grundläggande app i XCode toodemonstrate hello integrering.</span><span class="sxs-lookup"><span data-stu-id="754ac-117">We will create a basic app with XCode toodemonstrate hello integration.</span></span>

### <a name="create-a-new-ios-project"></a><span data-ttu-id="754ac-118">Skapa ett nytt iOS-projekt</span><span class="sxs-lookup"><span data-stu-id="754ac-118">Create a new iOS project</span></span>
[!INCLUDE [Create a new iOS Project](../../includes/mobile-engagement-create-new-ios-app.md)]

### <a name="connect-your-app-toohello-mobile-engagement-backend"></a><span data-ttu-id="754ac-119">Ansluta appen toohello Mobile Engagement-serverdelen</span><span class="sxs-lookup"><span data-stu-id="754ac-119">Connect your app toohello Mobile Engagement backend</span></span>
1. <span data-ttu-id="754ac-120">Hämta hello [Mobile Engagement iOS SDK].</span><span class="sxs-lookup"><span data-stu-id="754ac-120">Download hello [Mobile Engagement iOS SDK].</span></span>
2. <span data-ttu-id="754ac-121">Extrahera hello. tar.gz tooa mapp på datorn.</span><span class="sxs-lookup"><span data-stu-id="754ac-121">Extract hello .tar.gz file tooa folder in your computer.</span></span>
3. <span data-ttu-id="754ac-122">Högerklicka på hello-projektet och välj sedan **Lägg till filer i**.</span><span class="sxs-lookup"><span data-stu-id="754ac-122">Right-click hello project, and then select **Add files to**.</span></span>

    ![][1]
4. <span data-ttu-id="754ac-123">Navigera toohello mapp där du extraherade hello SDK, markera hello `EngagementSDK` mappen, klicka på **alternativ** hello nedre vänstra hörnet och kontrollera att kryssrutan hello **kopiera objekt vid behov** och hello kryssrutan för mål-kontrolleras och tryck sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="754ac-123">Navigate toohello folder where you extracted hello SDK, select hello `EngagementSDK` folder, click **Options** on hello bottom left corner and make sure that hello checkbox **Copy items if needed** and hello checkbox for your target are checked then press **OK**.</span></span>

    ![][2]
5. <span data-ttu-id="754ac-124">Öppna hello **Build Phases** fliken och i hello **länka binär med bibliotek** menyn Lägg till hello ramverk enligt nedan:</span><span class="sxs-lookup"><span data-stu-id="754ac-124">Open hello **Build Phases** tab, and in hello **Link Binary With Libraries** menu, add hello frameworks as shown below:</span></span>

    ![][3]
6. <span data-ttu-id="754ac-125">Gå tillbaka toohello Azure-portalen via appsidan **anslutningsinformation** sida och kopiera hello anslutningssträngen.</span><span class="sxs-lookup"><span data-stu-id="754ac-125">Go back toohello Azure portal in your app's **Connection Info** page and copy hello connection string.</span></span>

    ![][4]
7. <span data-ttu-id="754ac-126">Lägg till följande kodrad i hello din **AppDelegate.m** fil.</span><span class="sxs-lookup"><span data-stu-id="754ac-126">Add hello following line of code in your **AppDelegate.m** file.</span></span>

        #import "EngagementAgent.h"
8. <span data-ttu-id="754ac-127">Klistra in hello anslutningssträngen i hello `didFinishLaunchingWithOptions` delegera.</span><span class="sxs-lookup"><span data-stu-id="754ac-127">Now paste hello connection string in hello `didFinishLaunchingWithOptions` delegate.</span></span>

        - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions
        {
              [...]   
              [EngagementAgent init:@"Endpoint={YOUR_APP_COLLECTION.DOMAIN};SdkKey={YOUR_SDK_KEY};AppId={YOUR_APPID}"];
              [...]
        }
9. <span data-ttu-id="754ac-128">`setTestLogEnabled`är ett valfritt uttryck som aktiverar SDK-loggar du tooidentify problem.</span><span class="sxs-lookup"><span data-stu-id="754ac-128">`setTestLogEnabled` is an optional statement which enables SDK logs for you tooidentify issues.</span></span>

## <span data-ttu-id="754ac-129"><a id="monitor"></a>Aktivera realtidsövervakning</span><span class="sxs-lookup"><span data-stu-id="754ac-129"><a id="monitor"></a>Enable real-time monitoring</span></span>
<span data-ttu-id="754ac-130">Du måste skicka minst en skärm (aktivitet) toohello Mobile Engagement-serverdelen i ordning toostart skicka data och se till att hello användarna är aktiva.</span><span class="sxs-lookup"><span data-stu-id="754ac-130">In order toostart sending data and ensuring that hello users are active, you must send at least one screen (Activity) toohello Mobile Engagement backend.</span></span>

1. <span data-ttu-id="754ac-131">Öppna hello **ViewController.h** och importera **EngagementViewController.h**:</span><span class="sxs-lookup"><span data-stu-id="754ac-131">Open hello **ViewController.h** file and import **EngagementViewController.h**:</span></span>

    `#import "EngagementViewController.h"`
2. <span data-ttu-id="754ac-132">Ersätt nu hello superklassen för hello **ViewController** gränssnittet med `EngagementViewController`:</span><span class="sxs-lookup"><span data-stu-id="754ac-132">Now replace hello super class of hello **ViewController** interface by `EngagementViewController`:</span></span>

    `@interface ViewController : EngagementViewController`

## <span data-ttu-id="754ac-133"><a id="monitor"></a>Anslut appen med realtidsövervakning</span><span class="sxs-lookup"><span data-stu-id="754ac-133"><a id="monitor"></a>Connect app with real-time monitoring</span></span>
[!INCLUDE [Connect app with real-time monitoring](../../includes/mobile-engagement-connect-app-with-monitor.md)]

## <span data-ttu-id="754ac-134"><a id="integrate-push"></a>Aktivera push-meddelanden och meddelanden i appen</span><span class="sxs-lookup"><span data-stu-id="754ac-134"><a id="integrate-push"></a>Enable push notifications and in-app messaging</span></span>
<span data-ttu-id="754ac-135">Mobile Engagement kan du toointeract med användarna och räckvidd med push-meddelanden och appmeddelanden i hello kontext kampanjer.</span><span class="sxs-lookup"><span data-stu-id="754ac-135">Mobile Engagement allows you toointeract with your users and REACH with push notifications and in-app messaging in hello context of campaigns.</span></span> <span data-ttu-id="754ac-136">Denna modul är heter REACH i hello Mobile Engagement-portalen.</span><span class="sxs-lookup"><span data-stu-id="754ac-136">This module is called REACH in hello Mobile Engagement portal.</span></span>
<span data-ttu-id="754ac-137">hello följande avsnitt konfigurerar din app tooreceive dem.</span><span class="sxs-lookup"><span data-stu-id="754ac-137">hello following sections set up your app tooreceive them.</span></span>

### <a name="enable-your-app-tooreceive-silent-push-notifications"></a><span data-ttu-id="754ac-138">Aktivera din app tooreceive tysta Push-meddelanden</span><span class="sxs-lookup"><span data-stu-id="754ac-138">Enable your app tooreceive Silent Push Notifications</span></span>
[!INCLUDE [mobile-engagement-ios-silent-push](../../includes/mobile-engagement-ios-silent-push.md)]

### <a name="add-hello-reach-library-tooyour-project"></a><span data-ttu-id="754ac-139">Lägga till hello Reach-biblioteket tooyour projekt</span><span class="sxs-lookup"><span data-stu-id="754ac-139">Add hello Reach library tooyour project</span></span>
1. <span data-ttu-id="754ac-140">Högerklicka på ditt projekt.</span><span class="sxs-lookup"><span data-stu-id="754ac-140">Right-click your project.</span></span>
2. <span data-ttu-id="754ac-141">Välj **Lägg till fil i**.</span><span class="sxs-lookup"><span data-stu-id="754ac-141">Select **Add file to**.</span></span>
3. <span data-ttu-id="754ac-142">Navigera toohello mapp där du extraherade hello SDK.</span><span class="sxs-lookup"><span data-stu-id="754ac-142">Navigate toohello folder where you extracted hello SDK.</span></span>
4. <span data-ttu-id="754ac-143">Välj hello `EngagementReach` mapp.</span><span class="sxs-lookup"><span data-stu-id="754ac-143">Select hello `EngagementReach` folder.</span></span>
5. <span data-ttu-id="754ac-144">Klicka på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="754ac-144">Click **Add**.</span></span>

### <a name="modify-your-application-delegate"></a><span data-ttu-id="754ac-145">Ändra programdelegaten</span><span class="sxs-lookup"><span data-stu-id="754ac-145">Modify your Application Delegate</span></span>
1. <span data-ttu-id="754ac-146">Tillbaka i **AppDeletegate.m** fil, importera hello Engagement Reach-modulen.</span><span class="sxs-lookup"><span data-stu-id="754ac-146">Back in **AppDeletegate.m** file, import hello Engagement Reach module.</span></span>

        #import "AEReachModule.h"
        #import <UserNotifications/UserNotifications.h>
2. <span data-ttu-id="754ac-147">I hello `application:didFinishLaunchingWithOptions` metoden skapar du en räckviddsmodul och skickar den tooyour befintliga initieringsrad:</span><span class="sxs-lookup"><span data-stu-id="754ac-147">Inside hello `application:didFinishLaunchingWithOptions` method, create a Reach module and pass it tooyour existing Engagement initialization line:</span></span>

        - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
            AEReachModule * reach = [AEReachModule moduleWithNotificationIcon:[UIImage imageNamed:@"icon.png"]];
            [EngagementAgent init:@"Endpoint={YOUR_APP_COLLECTION.DOMAIN};SdkKey={YOUR_SDK_KEY};AppId={YOUR_APPID}" modules:reach, nil];
            [...]
            return YES;
        }

### <a name="enable-your-app-tooreceive-apns-push-notifications"></a><span data-ttu-id="754ac-148">Aktivera din app tooreceive APN Push-meddelanden</span><span class="sxs-lookup"><span data-stu-id="754ac-148">Enable your app tooreceive APNS Push Notifications</span></span>
1. <span data-ttu-id="754ac-149">Lägg till följande rad toohello hello `application:didFinishLaunchingWithOptions` metoden:</span><span class="sxs-lookup"><span data-stu-id="754ac-149">Add hello following line toohello `application:didFinishLaunchingWithOptions` method:</span></span>

        if (NSFoundationVersionNumber >= NSFoundationVersionNumber_iOS_8_0)
        {
            if (NSFoundationVersionNumber > NSFoundationVersionNumber_iOS_9_x_Max)
            {
                [UNUserNotificationCenter.currentNotificationCenter requestAuthorizationWithOptions:(UNAuthorizationOptionBadge | UNAuthorizationOptionSound | UNAuthorizationOptionAlert) completionHandler:^(BOOL granted, NSError * _Nullable error) {}];
            }else
            {
                [application registerUserNotificationSettings:[UIUserNotificationSettings settingsForTypes:(UIUserNotificationTypeBadge | UIUserNotificationTypeSound | UIUserNotificationTypeAlert)   categories:nil]];
            }
            [application registerForRemoteNotifications];
        }
        else
        {
            [application registerForRemoteNotificationTypes:(UIRemoteNotificationTypeBadge | UIRemoteNotificationTypeSound | UIRemoteNotificationTypeAlert)];
        }
2. <span data-ttu-id="754ac-150">Lägg till hello `application:didRegisterForRemoteNotificationsWithDeviceToken` metoden på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="754ac-150">Add hello `application:didRegisterForRemoteNotificationsWithDeviceToken` method as follows:</span></span>

        - (void)application:(UIApplication *)application didRegisterForRemoteNotificationsWithDeviceToken:(NSData *)deviceToken
        {
             [[EngagementAgent shared] registerDeviceToken:deviceToken];
            NSLog(@"Registered Token: %@", deviceToken);
        }
3. <span data-ttu-id="754ac-151">Lägg till hello `didFailToRegisterForRemoteNotificationsWithError` metoden på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="754ac-151">Add hello `didFailToRegisterForRemoteNotificationsWithError` method as follows:</span></span>

        - (void)application:(UIApplication*)application didFailToRegisterForRemoteNotificationsWithError:(NSError*)error
        {
           NSLog(@"Failed tooget token, error: %@", error);
        }
4. <span data-ttu-id="754ac-152">Lägg till hello `didReceiveRemoteNotification:fetchCompletionHandler` metoden på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="754ac-152">Add hello `didReceiveRemoteNotification:fetchCompletionHandler` method as follows:</span></span>

        - (void)application:(UIApplication *)application didReceiveRemoteNotification:(NSDictionary *)userInfo fetchCompletionHandler:(void (^)(UIBackgroundFetchResult result))handler
        {
            [[EngagementAgent shared] applicationDidReceiveRemoteNotification:userInfo fetchCompletionHandler:handler];
        }

[!INCLUDE [mobile-engagement-ios-send-push-push](../../includes/mobile-engagement-ios-send-push.md)]

<!-- URLs. -->
[Mobile Engagement iOS SDK]: http://aka.ms/qk2rnj

<!-- Images. -->
[1]: ./media/mobile-engagement-ios-get-started/xcode-add-files.png
[2]: ./media/mobile-engagement-ios-get-started/xcode-select-engagement-sdk.png
[3]: ./media/mobile-engagement-ios-get-started/xcode-build-phases.png
[4]: ./media/mobile-engagement-ios-get-started/app-connection-info-page.png
