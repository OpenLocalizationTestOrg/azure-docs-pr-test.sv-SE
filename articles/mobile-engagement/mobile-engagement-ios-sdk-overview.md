---
title: "aaaAzure Mobile Engagement iOS SDK översikt | Microsoft Docs"
description: "Senaste uppdateringarna och procedurer för iOS SDK för Azure Mobile Engagement"
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: 3a03bbd6-bcf8-436c-9775-5a8188629252
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-ios
ms.devlang: objective-c
ms.topic: article
ms.date: 07/17/2017
ms.author: piyushjo
ms.openlocfilehash: 38f0da2f84df9c62f8fbca233bfda8b9936fdc0f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="ios-sdk-for-azure-mobile-engagement"></a><span data-ttu-id="7da4a-103">iOS SDK för Azure Mobile Engagement</span><span class="sxs-lookup"><span data-stu-id="7da4a-103">iOS SDK for Azure Mobile Engagement</span></span>
<span data-ttu-id="7da4a-104">Börja här tooget alla hello information om hur toointegrate Azure Mobile Engagement i en iOS-App.</span><span class="sxs-lookup"><span data-stu-id="7da4a-104">Start here tooget all hello details on how toointegrate Azure Mobile Engagement in an iOS App.</span></span> <span data-ttu-id="7da4a-105">Om du vill att toogive den ett försök att först kontrollera att du gör våra [15 minuter att slutföra kursen](mobile-engagement-ios-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="7da4a-105">If you'd like toogive it a try first, make sure you do our [15 minutes tutorial](mobile-engagement-ios-get-started.md).</span></span>

<span data-ttu-id="7da4a-106">Klicka på toosee hello [SDK innehåll](mobile-engagement-ios-sdk-content.md)</span><span class="sxs-lookup"><span data-stu-id="7da4a-106">Click toosee hello [SDK Content](mobile-engagement-ios-sdk-content.md)</span></span>

## <a name="integration-procedures"></a><span data-ttu-id="7da4a-107">Integration procedurer</span><span class="sxs-lookup"><span data-stu-id="7da4a-107">Integration procedures</span></span>
1. <span data-ttu-id="7da4a-108">Börja här: [hur toointegrate Mobile Engagement i din iOS-app](mobile-engagement-ios-integrate-engagement.md)</span><span class="sxs-lookup"><span data-stu-id="7da4a-108">Start here: [How toointegrate Mobile Engagement in your iOS app](mobile-engagement-ios-integrate-engagement.md)</span></span>
2. <span data-ttu-id="7da4a-109">För aviseringar: [hur toointegrate Reach (meddelanden) i din iOS-app](mobile-engagement-ios-integrate-engagement-reach.md)</span><span class="sxs-lookup"><span data-stu-id="7da4a-109">For Notifications: [How toointegrate Reach (Notifications) in your iOS app](mobile-engagement-ios-integrate-engagement-reach.md)</span></span>
3. <span data-ttu-id="7da4a-110">Tagga planera implementeringen: [hur toouse hello avancerade Mobile Engagement API-märkning i din iOS-app](mobile-engagement-ios-use-engagement-api.md)</span><span class="sxs-lookup"><span data-stu-id="7da4a-110">Tag plan implementation: [How toouse hello advanced Mobile Engagement tagging API in your iOS app](mobile-engagement-ios-use-engagement-api.md)</span></span>

## <a name="release-notes"></a><span data-ttu-id="7da4a-111">Viktig information</span><span class="sxs-lookup"><span data-stu-id="7da4a-111">Release notes</span></span>
### <a name="410-07172017"></a><span data-ttu-id="7da4a-112">4.1.0 (07/17/2017)</span><span class="sxs-lookup"><span data-stu-id="7da4a-112">4.1.0 (07/17/2017)</span></span>
* <span data-ttu-id="7da4a-113">Fast Aktivitetsikoner avmarkerad i bakgrunden.</span><span class="sxs-lookup"><span data-stu-id="7da4a-113">Fixed badges cleared in background.</span></span>
* <span data-ttu-id="7da4a-114">Fast varningar på XCode 9 om API: er som inte anropas i huvudkön.</span><span class="sxs-lookup"><span data-stu-id="7da4a-114">Fixed warnings on XCode 9 about APIs not called in main queue.</span></span>
* <span data-ttu-id="7da4a-115">Fast en minnesläcka i Reach-avsökningar.</span><span class="sxs-lookup"><span data-stu-id="7da4a-115">Fixed a memory leak in Reach polls.</span></span>
* <span data-ttu-id="7da4a-116">Bort stöd för iOS 6.X.</span><span class="sxs-lookup"><span data-stu-id="7da4a-116">Dropped support for iOS 6.X.</span></span> <span data-ttu-id="7da4a-117">Från och med den här versionen hello distributionsmålet för ditt program måste vara minst iOS 7.</span><span class="sxs-lookup"><span data-stu-id="7da4a-117">Starting from this version hello deployment target of your application must be at least iOS 7.</span></span>

<span data-ttu-id="7da4a-118">Tidigare version finns hello [slutföra viktig information](mobile-engagement-ios-release-notes.md)</span><span class="sxs-lookup"><span data-stu-id="7da4a-118">For earlier version please see hello [complete release notes](mobile-engagement-ios-release-notes.md)</span></span>

## <a name="upgrade-procedures"></a><span data-ttu-id="7da4a-119">Uppgraderingsprocesser</span><span class="sxs-lookup"><span data-stu-id="7da4a-119">Upgrade procedures</span></span>
<span data-ttu-id="7da4a-120">Om du redan har integrerat en äldre version av Engagement i ditt program, har du tooconsider hello följande punkter när du uppgraderar hello SDK.</span><span class="sxs-lookup"><span data-stu-id="7da4a-120">If you already have integrated an older version of Engagement into your application, you have tooconsider hello following points when upgrading hello SDK.</span></span>

<span data-ttu-id="7da4a-121">Du kan ha toofollow flera procedurer om du har missat flera versioner av hello SDK finns hello fullständig [uppgradera procedurer](mobile-engagement-ios-upgrade-procedure.md).</span><span class="sxs-lookup"><span data-stu-id="7da4a-121">You may have toofollow several procedures if you missed several versions of hello SDK see hello complete [Upgrade Procedures](mobile-engagement-ios-upgrade-procedure.md).</span></span>

<span data-ttu-id="7da4a-122">För varje ny version av hello SDK måste du först ersätta (ta bort och importera på nytt i xcode) hello EngagementSDK och EngagementReach mappar.</span><span class="sxs-lookup"><span data-stu-id="7da4a-122">For each new version of hello SDK you must first replace (remove and re-import in xcode) hello EngagementSDK and EngagementReach folders.</span></span>

### <a name="from-300-too400"></a><span data-ttu-id="7da4a-123">Från 3.0.0 too4.0.0</span><span class="sxs-lookup"><span data-stu-id="7da4a-123">From 3.0.0 too4.0.0</span></span>
### <a name="xcode-8"></a><span data-ttu-id="7da4a-124">XCode 8</span><span class="sxs-lookup"><span data-stu-id="7da4a-124">XCode 8</span></span>
<span data-ttu-id="7da4a-125">XCode 8 är obligatoriskt från version 4.0.0 av hello SDK.</span><span class="sxs-lookup"><span data-stu-id="7da4a-125">XCode 8 is mandatory starting from version 4.0.0 of hello SDK.</span></span>

> [!NOTE]
> <span data-ttu-id="7da4a-126">Om du verkligen är beroende av XCode 7 så att du kan använda hello [iOS Engagement SDK v3.2.4](https://aka.ms/r6oouh).</span><span class="sxs-lookup"><span data-stu-id="7da4a-126">If you really depend on XCode 7 then you may use hello [iOS Engagement SDK v3.2.4](https://aka.ms/r6oouh).</span></span> <span data-ttu-id="7da4a-127">Det finns ett känt fel på hello reach-modulen för den här tidigare version när du kör på iOS 10-enheter: systemmeddelanden är inte åtgärdade.</span><span class="sxs-lookup"><span data-stu-id="7da4a-127">There is a known bug on hello reach module of this previous version while running on iOS 10 devices:  system notifications are not actioned.</span></span> <span data-ttu-id="7da4a-128">toofix detta behöver tooimplement hello föråldrad API `application:didReceiveRemoteNotification:` i din app delegera på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="7da4a-128">toofix this you will have tooimplement hello deprecated API `application:didReceiveRemoteNotification:` in your app delegate as follows:</span></span>
>
>

    - (void)application:(UIApplication*)application
    didReceiveRemoteNotification:(NSDictionary*)userInfo
    {
        [[EngagementAgent shared] applicationDidReceiveRemoteNotification:userInfo fetchCompletionHandler:nil];
    }

> [!IMPORTANT]
> <span data-ttu-id="7da4a-129">**Vi rekommenderar inte den här lösningen** som detta beteende kan ändras i kommande (även mindre) iOS version uppgraderar eftersom den här iOS API är inaktuell.</span><span class="sxs-lookup"><span data-stu-id="7da4a-129">**We do not recommend this workaround** as this behavior can change in any upcoming (even minor) iOS version upgrade because this iOS API is deprecated.</span></span> <span data-ttu-id="7da4a-130">Du bör växla tooXCode 8 så snart som möjligt.</span><span class="sxs-lookup"><span data-stu-id="7da4a-130">You should switch tooXCode 8 as soon as possible.</span></span>
>
>

#### <a name="usernotifications-framework"></a><span data-ttu-id="7da4a-131">UserNotifications framework</span><span class="sxs-lookup"><span data-stu-id="7da4a-131">UserNotifications framework</span></span>
<span data-ttu-id="7da4a-132">Du behöver tooadd hello `UserNotifications` ramverk i din Build Phases.</span><span class="sxs-lookup"><span data-stu-id="7da4a-132">You need tooadd hello `UserNotifications` framework in your Build Phases.</span></span>

<span data-ttu-id="7da4a-133">Öppna projektet-fönstret och välj hello rätt mål i hello Projektutforskaren.</span><span class="sxs-lookup"><span data-stu-id="7da4a-133">in hello project explorer, open your project pane and select hello correct target.</span></span> <span data-ttu-id="7da4a-134">Öppna hello **”Build-faser”** fliken och i hello **”länka binär med bibliotek”** menyn Lägg till framework `UserNotifications.framework` -uppsättningen hello länka som`Optional`</span><span class="sxs-lookup"><span data-stu-id="7da4a-134">Then, open hello **"Build phases"** tab and in hello **"Link Binary With Libraries"** menu, add framework `UserNotifications.framework` - set hello link as `Optional`</span></span>

#### <a name="application-push-capability"></a><span data-ttu-id="7da4a-135">Programmet push-funktion</span><span class="sxs-lookup"><span data-stu-id="7da4a-135">Application push capability</span></span>
<span data-ttu-id="7da4a-136">XCode 8 kan återställa din app push-funktion, kontrollera den i hello `capability` fliken i ditt valda målet.</span><span class="sxs-lookup"><span data-stu-id="7da4a-136">XCode 8 may reset your app push capability, please double check it in hello `capability` tab of your selected target.</span></span>

#### <a name="add-hello-new-ios-10-notification-registration-code"></a><span data-ttu-id="7da4a-137">Lägg till hello nya iOS 10 registrering Meddelandekod</span><span class="sxs-lookup"><span data-stu-id="7da4a-137">Add hello new iOS 10 notification registration code</span></span>
<span data-ttu-id="7da4a-138">hello äldre kodfragment tooregister hello app toonotifications fungerar fortfarande men använder föråldrad API: er när du kör på iOS 10.</span><span class="sxs-lookup"><span data-stu-id="7da4a-138">hello older code snippet tooregister hello app toonotifications still works but is using deprecated APIs while running on iOS 10.</span></span>

<span data-ttu-id="7da4a-139">Importera hello `User Notification` framework:</span><span class="sxs-lookup"><span data-stu-id="7da4a-139">Import hello `User Notification` framework:</span></span>

        #import <UserNotifications/UserNotifications.h>

<span data-ttu-id="7da4a-140">I programdelegaten `application:didFinishLaunchingWithOptions` metoden Ersätt:</span><span class="sxs-lookup"><span data-stu-id="7da4a-140">In your application delegate `application:didFinishLaunchingWithOptions` method replace:</span></span>

        if ([application respondsToSelector:@selector(registerUserNotificationSettings:)]) {
            [application registerUserNotificationSettings:[UIUserNotificationSettings settingsForTypes:(UIUserNotificationTypeBadge | UIUserNotificationTypeSound | UIUserNotificationTypeAlert) categories:nil]];
            [application registerForRemoteNotifications];
        }
        else {

            [application registerForRemoteNotificationTypes:(UIRemoteNotificationTypeBadge | UIRemoteNotificationTypeSound | UIRemoteNotificationTypeAlert)];
        }

<span data-ttu-id="7da4a-141">av:</span><span class="sxs-lookup"><span data-stu-id="7da4a-141">by :</span></span>

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

#### <a name="resolve-unusernotificationcenter-delegate-conflicts"></a><span data-ttu-id="7da4a-142">UNUserNotificationCenter ombud konfliktlösning</span><span class="sxs-lookup"><span data-stu-id="7da4a-142">Resolve UNUserNotificationCenter delegate conflicts</span></span>

<span data-ttu-id="7da4a-143">*Om varken ditt program eller en av tredjeparts-biblioteken implementerar en `UNUserNotificationCenterDelegate` och sedan kan du hoppa över den här delen.*</span><span class="sxs-lookup"><span data-stu-id="7da4a-143">*If neither your application or one of your third party libraries implements a `UNUserNotificationCenterDelegate` then you can skip this part.*</span></span>

<span data-ttu-id="7da4a-144">En `UNUserNotificationCenter` delegat som används av hello SDK toomonitor hello livscykel Engagement meddelanden på enheter som kör IOS 10 eller senare.</span><span class="sxs-lookup"><span data-stu-id="7da4a-144">A `UNUserNotificationCenter` delegate is used by hello SDK toomonitor hello life cycle of Engagement notifications on devices running on iOS 10 or greater.</span></span> <span data-ttu-id="7da4a-145">hello SDK har sin egen implementering av hello `UNUserNotificationCenterDelegate` protokoll, men det kan bara finnas ett `UNUserNotificationCenter` delegera per program.</span><span class="sxs-lookup"><span data-stu-id="7da4a-145">hello SDK has its own implementation of hello `UNUserNotificationCenterDelegate` protocol but there can be only one `UNUserNotificationCenter` delegate per application.</span></span> <span data-ttu-id="7da4a-146">Andra ombud läggs toohello `UNUserNotificationCenter` objekt står i konflikt med hello Engagement en.</span><span class="sxs-lookup"><span data-stu-id="7da4a-146">Any other delegate added toohello `UNUserNotificationCenter` object will conflict with hello Engagement one.</span></span> <span data-ttu-id="7da4a-147">Om hello SDK upptäcker din eller någon annan tredje parts ombud kommer det inte att använda sin egen implementering toogive hello du en chans tooresolve konflikter.</span><span class="sxs-lookup"><span data-stu-id="7da4a-147">If hello SDK detects your or any other third party's delegate then it will not use its own implementation toogive you a chance tooresolve hello conflicts.</span></span> <span data-ttu-id="7da4a-148">Du måste tooadd hello Engagement logik tooyour äger en delegat i ordning tooresolve hello konflikter.</span><span class="sxs-lookup"><span data-stu-id="7da4a-148">You will have tooadd hello Engagement logic tooyour own delegate in order tooresolve hello conflicts.</span></span>

<span data-ttu-id="7da4a-149">Det finns två sätt tooachieve detta.</span><span class="sxs-lookup"><span data-stu-id="7da4a-149">There are two ways tooachieve this.</span></span>

<span data-ttu-id="7da4a-150">Förslag 1, genom att helt enkelt vidarebefordra ombudet anropar toohello SDK:</span><span class="sxs-lookup"><span data-stu-id="7da4a-150">Proposal 1, simply by forwarding your delegate calls toohello SDK:</span></span>

    #import <UIKit/UIKit.h>
    #import "EngagementAgent.h"
    #import <UserNotifications/UserNotifications.h>


    @interface MyAppDelegate : NSObject <UIApplicationDelegate, UNUserNotificationCenterDelegate>
    @end

    @implementation MyAppDelegate

    - (void)userNotificationCenter:(UNUserNotificationCenter *)center willPresentNotification:(UNNotification *)notification withCompletionHandler:(void (^)(UNNotificationPresentationOptions options))completionHandler
    {
      // Your own logic.

      [[EngagementAgent shared] userNotificationCenterWillPresentNotification:notification withCompletionHandler:completionHandler]
    }

    - (void)userNotificationCenter:(UNUserNotificationCenter *)center didReceiveNotificationResponse:(UNNotificationResponse *)response withCompletionHandler:(void(^)())completionHandler
    {
      // Your own logic.

      [[EngagementAgent shared] userNotificationCenterDidReceiveNotificationResponse:response withCompletionHandler:completionHandler]
    }
    @end

<span data-ttu-id="7da4a-151">Eller förslag 2, med arv från hello `AEUserNotificationHandler` klass</span><span class="sxs-lookup"><span data-stu-id="7da4a-151">Or proposal 2, by inheriting from hello `AEUserNotificationHandler` class</span></span>

    #import "AEUserNotificationHandler.h"
    #import "EngagementAgent.h"

    @interface CustomUserNotificationHandler :AEUserNotificationHandler
    @end

    @implementation CustomUserNotificationHandler

    - (void)userNotificationCenter:(UNUserNotificationCenter *)center willPresentNotification:(UNNotification *)notification withCompletionHandler:(void (^)(UNNotificationPresentationOptions options))completionHandler
    {
      // Your own logic.

      [super userNotificationCenter:center willPresentNotification:notification withCompletionHandler:completionHandler];
    }

    - (void)userNotificationCenter:(UNUserNotificationCenter *)center didReceiveNotificationResponse: UNNotificationResponse *)response withCompletionHandler:(void(^)())completionHandler
    {
      // Your own logic.

      [super userNotificationCenter:center didReceiveNotificationResponse:response withCompletionHandler:completionHandler];
    }

    @end

> [!NOTE]
> <span data-ttu-id="7da4a-152">Du kan fastställa om ett meddelande kommer från Engagement eller inte genom att ange dess `userInfo` ordlista toohello Agent `isEngagementPushPayload:` klassen metoden.</span><span class="sxs-lookup"><span data-stu-id="7da4a-152">You can determine whether a notification comes from Engagement or not by passing its `userInfo` dictionary toohello Agent `isEngagementPushPayload:` class method.</span></span>

<span data-ttu-id="7da4a-153">Kontrollera att hello `UNUserNotificationCenter` objektets ombud anges tooyour ombud inom antingen hello `application:willFinishLaunchingWithOptions:` eller hello `application:didFinishLaunchingWithOptions:` metod för programdelegaten.</span><span class="sxs-lookup"><span data-stu-id="7da4a-153">Make sure that hello `UNUserNotificationCenter` object's delegate is set tooyour delegate within either hello `application:willFinishLaunchingWithOptions:` or hello `application:didFinishLaunchingWithOptions:` method of your application delegate.</span></span>
<span data-ttu-id="7da4a-154">Till exempel om du har implementerat hello ovan förslag 1:</span><span class="sxs-lookup"><span data-stu-id="7da4a-154">For instance, if you implemented hello above proposal 1:</span></span>

      - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
        // Any other code

        [UNUserNotificationCenter currentNotificationCenter].delegate = self;
        return YES;
      }
