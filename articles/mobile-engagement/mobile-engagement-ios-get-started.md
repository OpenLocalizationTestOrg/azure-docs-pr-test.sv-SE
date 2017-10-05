---
title: "Kom igång med Azure Mobile Engagement för iOS i Objective C | Microsoft Docs"
description: "Lär dig hur du använder Azure Mobile Engagement med analyser och push-meddelanden för iOS-appar."
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
ms.openlocfilehash: 1b87a2ebb35b31ee3d3139ecead6267e62eb1033
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="get-started-with-azure-mobile-engagement-for-ios-apps-in-objective-c"></a><span data-ttu-id="50881-103">Komma igång med Azure Mobile Engagement för iOS-appar i Objective C</span><span class="sxs-lookup"><span data-stu-id="50881-103">Get Started with Azure Mobile Engagement for iOS apps in Objective C</span></span>
[!INCLUDE [Hero tutorial switcher](../../includes/mobile-engagement-hero-tutorial-switcher.md)]

<span data-ttu-id="50881-104">I det här avsnittet beskrivs hur du använder Azure Mobile Engagement för att förstå appanvändningen, och hur du skickar push-meddelanden till segmenterade användare i ett iOS-program.</span><span class="sxs-lookup"><span data-stu-id="50881-104">This topic shows you how to use Azure Mobile Engagement to understand your app usage and send push notifications to segmented users to an iOS application.</span></span>
<span data-ttu-id="50881-105">I den här självstudiekursen skapar du en tom iOS-app som samlar in grundläggande data och tar emot push-meddelanden via Apple Push Notification System (APNS).</span><span class="sxs-lookup"><span data-stu-id="50881-105">In this tutorial, you create a blank iOS app that collects basic data and receives push notifications using Apple Push Notification System (APNS).</span></span>

<span data-ttu-id="50881-106">Följande krävs för den här självstudiekursen:</span><span class="sxs-lookup"><span data-stu-id="50881-106">This tutorial requires the following:</span></span>

* <span data-ttu-id="50881-107">XCode 8, som du kan installera från Mac App Store</span><span class="sxs-lookup"><span data-stu-id="50881-107">XCode 8, which you can install from your MAC App Store</span></span>
* <span data-ttu-id="50881-108">[Mobile Engagement iOS SDK]</span><span class="sxs-lookup"><span data-stu-id="50881-108">the [Mobile Engagement iOS SDK]</span></span>

<span data-ttu-id="50881-109">Du måste slutföra den här självstudiekursen innan du påbörjar någon annan kurs om Mobile Engagement och iOS-appar.</span><span class="sxs-lookup"><span data-stu-id="50881-109">Completing this tutorial is a prerequisite for all other Mobile Engagement tutorials for iOS apps.</span></span>

> [!NOTE]
> <span data-ttu-id="50881-110">Du måste ha ett aktivt Azure-konto för att slutföra den här kursen.</span><span class="sxs-lookup"><span data-stu-id="50881-110">To complete this tutorial, you must have an active Azure account.</span></span> <span data-ttu-id="50881-111">Om du inte har något konto kan skapa du ett kostnadsfritt utvärderingskonto på bara några minuter.</span><span class="sxs-lookup"><span data-stu-id="50881-111">If you don't have an account, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="50881-112">Mer information om den [kostnadsfria utvärderingsversionen av Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-ios-get-started).</span><span class="sxs-lookup"><span data-stu-id="50881-112">For details, see [Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-ios-get-started).</span></span>
>
>

## <span data-ttu-id="50881-113"><a id="setup-azme"></a>Konfigurera Mobile Engagement för din iOS-app</span><span class="sxs-lookup"><span data-stu-id="50881-113"><a id="setup-azme"></a>Setup Mobile Engagement for your iOS app</span></span>
[!INCLUDE [Create Mobile Engagement App in Portal](../../includes/mobile-engagement-create-app-in-portal-new.md)]

## <span data-ttu-id="50881-114"><a id="connecting-app"></a>Anslut appen till Mobile Engagement-serverdelen</span><span class="sxs-lookup"><span data-stu-id="50881-114"><a id="connecting-app"></a>Connect your app to the Mobile Engagement backend</span></span>
<span data-ttu-id="50881-115">I den här kursen behandlas en ”grundläggande integration”, vilket är den minsta uppsättningen som krävs för att samla in data och skicka ett push-meddelande.</span><span class="sxs-lookup"><span data-stu-id="50881-115">This tutorial presents a "basic integration", which is the minimal set required to collect data and send a push notification.</span></span> <span data-ttu-id="50881-116">Den fullständiga integrationsdokumentationen finns i [Mobile Engagement iOS SDK-integration](mobile-engagement-ios-sdk-overview.md)</span><span class="sxs-lookup"><span data-stu-id="50881-116">The complete integration documentation can be found in the [Mobile Engagement iOS SDK integration](mobile-engagement-ios-sdk-overview.md)</span></span>

<span data-ttu-id="50881-117">Vi skapar en grundläggande app i XCode för att demonstrera integrationen.</span><span class="sxs-lookup"><span data-stu-id="50881-117">We will create a basic app with XCode to demonstrate the integration.</span></span>

### <a name="create-a-new-ios-project"></a><span data-ttu-id="50881-118">Skapa ett nytt iOS-projekt</span><span class="sxs-lookup"><span data-stu-id="50881-118">Create a new iOS project</span></span>
[!INCLUDE [Create a new iOS Project](../../includes/mobile-engagement-create-new-ios-app.md)]

### <a name="connect-your-app-to-the-mobile-engagement-backend"></a><span data-ttu-id="50881-119">Ansluta appen till Mobile Engagement-serverdelen</span><span class="sxs-lookup"><span data-stu-id="50881-119">Connect your app to the Mobile Engagement backend</span></span>
1. <span data-ttu-id="50881-120">Ladda ned [Mobile Engagement iOS SDK].</span><span class="sxs-lookup"><span data-stu-id="50881-120">Download the [Mobile Engagement iOS SDK].</span></span>
2. <span data-ttu-id="50881-121">Extrahera .tar.gz-filen till en mapp på datorn.</span><span class="sxs-lookup"><span data-stu-id="50881-121">Extract the .tar.gz file to a folder in your computer.</span></span>
3. <span data-ttu-id="50881-122">Högerklicka på projektet och välj sedan **Lägg till filer i**.</span><span class="sxs-lookup"><span data-stu-id="50881-122">Right-click the project, and then select **Add files to**.</span></span>

    ![][1]
4. <span data-ttu-id="50881-123">Navigera till mappen där du extraherade SDK, markera mappen `EngagementSDK`, klicka på **Alternativ** i det nedre vänstra hörnet och se till att kryssrutan **kopiera objekt vid behov** och kryssrutan för ditt mål är markerade. Tryck sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="50881-123">Navigate to the folder where you extracted the SDK, select the `EngagementSDK` folder, click **Options** on the bottom left corner and make sure that the checkbox **Copy items if needed** and the checkbox for your target are checked then press **OK**.</span></span>

    ![][2]
5. <span data-ttu-id="50881-124">Öppna fliken **Build Phases** (Byggfaser) och gå till menyn **Link Binary With Libraries** (Länka binära värden till bibliotek) och lägg till ramverk enligt nedan:</span><span class="sxs-lookup"><span data-stu-id="50881-124">Open the **Build Phases** tab, and in the **Link Binary With Libraries** menu, add the frameworks as shown below:</span></span>

    ![][3]
6. <span data-ttu-id="50881-125">Gå tillbaka till Azure-portalen via appsidan **Anslutningsinformation** och kopiera anslutningssträngen.</span><span class="sxs-lookup"><span data-stu-id="50881-125">Go back to the Azure portal in your app's **Connection Info** page and copy the connection string.</span></span>

    ![][4]
7. <span data-ttu-id="50881-126">Lägg till följande rad med kod i filen **AppDelegate.m**.</span><span class="sxs-lookup"><span data-stu-id="50881-126">Add the following line of code in your **AppDelegate.m** file.</span></span>

        #import "EngagementAgent.h"
8. <span data-ttu-id="50881-127">Klistra in anslutningssträngen i delegaten `didFinishLaunchingWithOptions`.</span><span class="sxs-lookup"><span data-stu-id="50881-127">Now paste the connection string in the `didFinishLaunchingWithOptions` delegate.</span></span>

        - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions
        {
              [...]   
              [EngagementAgent init:@"Endpoint={YOUR_APP_COLLECTION.DOMAIN};SdkKey={YOUR_SDK_KEY};AppId={YOUR_APPID}"];
              [...]
        }
9. <span data-ttu-id="50881-128">`setTestLogEnabled` är ett valfritt uttryck som aktiverar SDK-loggar som sedan kan användas till att identifiera problem.</span><span class="sxs-lookup"><span data-stu-id="50881-128">`setTestLogEnabled` is an optional statement which enables SDK logs for you to identify issues.</span></span>

## <span data-ttu-id="50881-129"><a id="monitor"></a>Aktivera realtidsövervakning</span><span class="sxs-lookup"><span data-stu-id="50881-129"><a id="monitor"></a>Enable real-time monitoring</span></span>
<span data-ttu-id="50881-130">För att kunna börja skicka data och försäkra dig om att användarna är aktiva måste du skicka minst en skärm (aktivitet) till Mobile Engagement-serverdelen.</span><span class="sxs-lookup"><span data-stu-id="50881-130">In order to start sending data and ensuring that the users are active, you must send at least one screen (Activity) to the Mobile Engagement backend.</span></span>

1. <span data-ttu-id="50881-131">Öppna filen **ViewController.h** och importera **EngagementViewController.h**:</span><span class="sxs-lookup"><span data-stu-id="50881-131">Open the **ViewController.h** file and import **EngagementViewController.h**:</span></span>

    `#import "EngagementViewController.h"`
2. <span data-ttu-id="50881-132">Ersätt superklassen för **ViewController**-gränssnittet med `EngagementViewController`:</span><span class="sxs-lookup"><span data-stu-id="50881-132">Now replace the super class of the **ViewController** interface by `EngagementViewController`:</span></span>

    `@interface ViewController : EngagementViewController`

## <span data-ttu-id="50881-133"><a id="monitor"></a>Anslut appen med realtidsövervakning</span><span class="sxs-lookup"><span data-stu-id="50881-133"><a id="monitor"></a>Connect app with real-time monitoring</span></span>
[!INCLUDE [Connect app with real-time monitoring](../../includes/mobile-engagement-connect-app-with-monitor.md)]

## <span data-ttu-id="50881-134"><a id="integrate-push"></a>Aktivera push-meddelanden och meddelanden i appen</span><span class="sxs-lookup"><span data-stu-id="50881-134"><a id="integrate-push"></a>Enable push notifications and in-app messaging</span></span>
<span data-ttu-id="50881-135">Med Mobile Engagement kan du samverka med användarna, och köra kampanjer med push-meddelanden och meddelanden i appen.</span><span class="sxs-lookup"><span data-stu-id="50881-135">Mobile Engagement allows you to interact with your users and REACH with push notifications and in-app messaging in the context of campaigns.</span></span> <span data-ttu-id="50881-136">Modulen som används för det heter REACH och finns i Mobile Engagement-portalen.</span><span class="sxs-lookup"><span data-stu-id="50881-136">This module is called REACH in the Mobile Engagement portal.</span></span>
<span data-ttu-id="50881-137">I följande avsnitt konfigurerar du appen för att ta emot dem.</span><span class="sxs-lookup"><span data-stu-id="50881-137">The following sections set up your app to receive them.</span></span>

### <a name="enable-your-app-to-receive-silent-push-notifications"></a><span data-ttu-id="50881-138">Konfigurera appen för att ta emot tysta push-meddelanden</span><span class="sxs-lookup"><span data-stu-id="50881-138">Enable your app to receive Silent Push Notifications</span></span>
[!INCLUDE [mobile-engagement-ios-silent-push](../../includes/mobile-engagement-ios-silent-push.md)]

### <a name="add-the-reach-library-to-your-project"></a><span data-ttu-id="50881-139">Lägg till Reach-biblioteket i projektet</span><span class="sxs-lookup"><span data-stu-id="50881-139">Add the Reach library to your project</span></span>
1. <span data-ttu-id="50881-140">Högerklicka på ditt projekt.</span><span class="sxs-lookup"><span data-stu-id="50881-140">Right-click your project.</span></span>
2. <span data-ttu-id="50881-141">Välj **Lägg till fil i**.</span><span class="sxs-lookup"><span data-stu-id="50881-141">Select **Add file to**.</span></span>
3. <span data-ttu-id="50881-142">Navigera till mappen dit du extraherade SDK.</span><span class="sxs-lookup"><span data-stu-id="50881-142">Navigate to the folder where you extracted the SDK.</span></span>
4. <span data-ttu-id="50881-143">Välj mappen `EngagementReach`.</span><span class="sxs-lookup"><span data-stu-id="50881-143">Select the `EngagementReach` folder.</span></span>
5. <span data-ttu-id="50881-144">Klicka på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="50881-144">Click **Add**.</span></span>

### <a name="modify-your-application-delegate"></a><span data-ttu-id="50881-145">Ändra programdelegaten</span><span class="sxs-lookup"><span data-stu-id="50881-145">Modify your Application Delegate</span></span>
1. <span data-ttu-id="50881-146">I filen **AppDeletegate.m** importerar du räckviddsmodulen för Engagement.</span><span class="sxs-lookup"><span data-stu-id="50881-146">Back in **AppDeletegate.m** file, import the Engagement Reach module.</span></span>

        #import "AEReachModule.h"
        #import <UserNotifications/UserNotifications.h>
2. <span data-ttu-id="50881-147">Inifrån metoden `application:didFinishLaunchingWithOptions` skapar du en räckviddsmodul och skickar den till din befintliga initieringsrad för Engagement:</span><span class="sxs-lookup"><span data-stu-id="50881-147">Inside the `application:didFinishLaunchingWithOptions` method, create a Reach module and pass it to your existing Engagement initialization line:</span></span>

        - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
            AEReachModule * reach = [AEReachModule moduleWithNotificationIcon:[UIImage imageNamed:@"icon.png"]];
            [EngagementAgent init:@"Endpoint={YOUR_APP_COLLECTION.DOMAIN};SdkKey={YOUR_SDK_KEY};AppId={YOUR_APPID}" modules:reach, nil];
            [...]
            return YES;
        }

### <a name="enable-your-app-to-receive-apns-push-notifications"></a><span data-ttu-id="50881-148">Konfigurera appen för att ta emot push-meddelanden med APNS</span><span class="sxs-lookup"><span data-stu-id="50881-148">Enable your app to receive APNS Push Notifications</span></span>
1. <span data-ttu-id="50881-149">Lägg till följande rad i metoden `application:didFinishLaunchingWithOptions`:</span><span class="sxs-lookup"><span data-stu-id="50881-149">Add the following line to the `application:didFinishLaunchingWithOptions` method:</span></span>

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
2. <span data-ttu-id="50881-150">Lägg till `application:didRegisterForRemoteNotificationsWithDeviceToken`-metoden på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="50881-150">Add the `application:didRegisterForRemoteNotificationsWithDeviceToken` method as follows:</span></span>

        - (void)application:(UIApplication *)application didRegisterForRemoteNotificationsWithDeviceToken:(NSData *)deviceToken
        {
             [[EngagementAgent shared] registerDeviceToken:deviceToken];
            NSLog(@"Registered Token: %@", deviceToken);
        }
3. <span data-ttu-id="50881-151">Lägg till `didFailToRegisterForRemoteNotificationsWithError`-metoden på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="50881-151">Add the `didFailToRegisterForRemoteNotificationsWithError` method as follows:</span></span>

        - (void)application:(UIApplication*)application didFailToRegisterForRemoteNotificationsWithError:(NSError*)error
        {
           NSLog(@"Failed to get token, error: %@", error);
        }
4. <span data-ttu-id="50881-152">Lägg till `didReceiveRemoteNotification:fetchCompletionHandler`-metoden på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="50881-152">Add the `didReceiveRemoteNotification:fetchCompletionHandler` method as follows:</span></span>

        - (void)application:(UIApplication *)application didReceiveRemoteNotification:(NSDictionary *)userInfo fetchCompletionHandler:(void (^)(UIBackgroundFetchResult result))handler
        {
            [[EngagementAgent shared] applicationDidReceiveRemoteNotification:userInfo fetchCompletionHandler:handler];
        }

[!INCLUDE [mobile-engagement-ios-send-push-push](../../includes/mobile-engagement-ios-send-push.md)]

<!-- URLs. -->
<span data-ttu-id="50881-153">[Mobile Engagement iOS SDK]: http://aka.ms/qk2rnj</span><span class="sxs-lookup"><span data-stu-id="50881-153">[Mobile Engagement iOS SDK]: http://aka.ms/qk2rnj</span></span>

<!-- Images. -->
[1]: ./media/mobile-engagement-ios-get-started/xcode-add-files.png
[2]: ./media/mobile-engagement-ios-get-started/xcode-select-engagement-sdk.png
[3]: ./media/mobile-engagement-ios-get-started/xcode-build-phases.png
[4]: ./media/mobile-engagement-ios-get-started/app-connection-info-page.png
