---
title: aaaAzure Mobile Engagement iOS SDK uppgradera proceduren | Microsoft Docs
description: "Senaste uppdateringarna och procedurer för iOS SDK för Azure Mobile Engagement"
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: 72a9e493-3f14-4e52-b6e2-0490fd04b184
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-ios
ms.devlang: objective-c
ms.topic: article
ms.date: 12/13/2016
ms.author: piyushjo
ms.openlocfilehash: 5a81bcaaec72aec665b3334e6400d520454d56a7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="upgrade-procedures"></a><span data-ttu-id="45eaa-103">Uppgraderingsprocesser</span><span class="sxs-lookup"><span data-stu-id="45eaa-103">Upgrade procedures</span></span>
<span data-ttu-id="45eaa-104">Om du redan har integrerat en äldre version av Engagement i ditt program, har du tooconsider hello följande punkter när du uppgraderar hello SDK.</span><span class="sxs-lookup"><span data-stu-id="45eaa-104">If you already have integrated an older version of Engagement into your application, you have tooconsider hello following points when upgrading hello SDK.</span></span>

<span data-ttu-id="45eaa-105">För varje ny version av hello SDK måste du först ersätta (ta bort och importera på nytt i xcode) hello EngagementSDK och EngagementReach mappar.</span><span class="sxs-lookup"><span data-stu-id="45eaa-105">For each new version of hello SDK you must first replace (remove and re-import in xcode) hello EngagementSDK and EngagementReach folders.</span></span>

## <a name="from-300-too400"></a><span data-ttu-id="45eaa-106">Från 3.0.0 too4.0.0</span><span class="sxs-lookup"><span data-stu-id="45eaa-106">From 3.0.0 too4.0.0</span></span>
### <a name="xcode-8"></a><span data-ttu-id="45eaa-107">XCode 8</span><span class="sxs-lookup"><span data-stu-id="45eaa-107">XCode 8</span></span>
<span data-ttu-id="45eaa-108">XCode 8 är obligatoriskt från version 4.0.0 av hello SDK.</span><span class="sxs-lookup"><span data-stu-id="45eaa-108">XCode 8 is mandatory starting from version 4.0.0 of hello SDK.</span></span>

> [!NOTE]
> <span data-ttu-id="45eaa-109">Om du verkligen är beroende av XCode 7 så att du kan använda hello [iOS Engagement SDK v3.2.4](https://aka.ms/r6oouh).</span><span class="sxs-lookup"><span data-stu-id="45eaa-109">If you really depend on XCode 7 then you may use hello [iOS Engagement SDK v3.2.4](https://aka.ms/r6oouh).</span></span> <span data-ttu-id="45eaa-110">Det finns ett känt fel på hello reach-modulen för den här tidigare version när du kör på iOS 10-enheter: systemmeddelanden är inte åtgärdade.</span><span class="sxs-lookup"><span data-stu-id="45eaa-110">There is a known bug on hello reach module of this previous version while running on iOS 10 devices:  system notifications are not actioned.</span></span> <span data-ttu-id="45eaa-111">toofix detta behöver tooimplement hello föråldrad API `application:didReceiveRemoteNotification:` i din app delegera på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="45eaa-111">toofix this you will have tooimplement hello deprecated API `application:didReceiveRemoteNotification:` in your app delegate as follows:</span></span>
> 
> 

    - (void)application:(UIApplication*)application
    didReceiveRemoteNotification:(NSDictionary*)userInfo
    {
        [[EngagementAgent shared] applicationDidReceiveRemoteNotification:userInfo fetchCompletionHandler:nil];
    }

> [!IMPORTANT]
> <span data-ttu-id="45eaa-112">**Vi rekommenderar inte den här lösningen** som detta beteende kan ändras i kommande (även mindre) iOS version uppgraderar eftersom den här iOS API är inaktuell.</span><span class="sxs-lookup"><span data-stu-id="45eaa-112">**We do not recommend this workaround** as this behavior can change in any upcoming (even minor) iOS version upgrade because this iOS API is deprecated.</span></span> <span data-ttu-id="45eaa-113">Du bör växla tooXCode 8 så snart som möjligt.</span><span class="sxs-lookup"><span data-stu-id="45eaa-113">You should switch tooXCode 8 as soon as possible.</span></span>
> 
> 

### <a name="usernotifications-framework"></a><span data-ttu-id="45eaa-114">UserNotifications framework</span><span class="sxs-lookup"><span data-stu-id="45eaa-114">UserNotifications framework</span></span>
<span data-ttu-id="45eaa-115">Du behöver tooadd hello `UserNotifications` ramverk i din Build Phases.</span><span class="sxs-lookup"><span data-stu-id="45eaa-115">You need tooadd hello `UserNotifications` framework in your Build Phases.</span></span>

<span data-ttu-id="45eaa-116">Öppna projektet-fönstret och välj hello rätt mål i hello Projektutforskaren.</span><span class="sxs-lookup"><span data-stu-id="45eaa-116">in hello project explorer, open your project pane and select hello correct target.</span></span> <span data-ttu-id="45eaa-117">Öppna hello **”Build-faser”** fliken och i hello **”länka binär med bibliotek”** menyn Lägg till framework `UserNotifications.framework` -uppsättningen hello länka som`Optional`</span><span class="sxs-lookup"><span data-stu-id="45eaa-117">Then, open hello **"Build phases"** tab and in hello **"Link Binary With Libraries"** menu, add framework `UserNotifications.framework` - set hello link as `Optional`</span></span>

### <a name="application-push-capability"></a><span data-ttu-id="45eaa-118">Programmet push-funktion</span><span class="sxs-lookup"><span data-stu-id="45eaa-118">Application push capability</span></span>
<span data-ttu-id="45eaa-119">XCode 8 kan återställa din app push-funktion, kontrollera den i hello `capability` fliken i ditt valda målet.</span><span class="sxs-lookup"><span data-stu-id="45eaa-119">XCode 8 may reset your app push capability, please double check it in hello `capability` tab of your selected target.</span></span>

### <a name="add-hello-new-ios-10-notification-registration-code"></a><span data-ttu-id="45eaa-120">Lägg till hello nya iOS 10 registrering Meddelandekod</span><span class="sxs-lookup"><span data-stu-id="45eaa-120">Add hello new iOS 10 notification registration code</span></span>
<span data-ttu-id="45eaa-121">hello äldre kodfragment tooregister hello app toonotifications fungerar fortfarande men använder föråldrad API: er när du kör på iOS 10.</span><span class="sxs-lookup"><span data-stu-id="45eaa-121">hello older code snippet tooregister hello app toonotifications still works but is using deprecated APIs while running on iOS 10.</span></span>

<span data-ttu-id="45eaa-122">Importera hello `User Notification` framework:</span><span class="sxs-lookup"><span data-stu-id="45eaa-122">Import hello `User Notification` framework:</span></span>

        #import <UserNotifications/UserNotifications.h> 

<span data-ttu-id="45eaa-123">I programdelegaten `application:didFinishLaunchingWithOptions` metoden Ersätt:</span><span class="sxs-lookup"><span data-stu-id="45eaa-123">In your application delegate `application:didFinishLaunchingWithOptions` method replace:</span></span>

    if ([application respondsToSelector:@selector(registerUserNotificationSettings:)]) {
        [application registerUserNotificationSettings:[UIUserNotificationSettings settingsForTypes:(UIUserNotificationTypeBadge | UIUserNotificationTypeSound | UIUserNotificationTypeAlert) categories:nil]];
        [application registerForRemoteNotifications];
    }
    else {

        [application registerForRemoteNotificationTypes:(UIRemoteNotificationTypeBadge | UIRemoteNotificationTypeSound | UIRemoteNotificationTypeAlert)];
    }

<span data-ttu-id="45eaa-124">av:</span><span class="sxs-lookup"><span data-stu-id="45eaa-124">by :</span></span>

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

### <a name="resolve-unusernotificationcenter-delegate-conflicts"></a><span data-ttu-id="45eaa-125">UNUserNotificationCenter ombud konfliktlösning</span><span class="sxs-lookup"><span data-stu-id="45eaa-125">Resolve UNUserNotificationCenter delegate conflicts</span></span>

<span data-ttu-id="45eaa-126">*Om varken ditt program eller en av tredjeparts-biblioteken implementerar en `UNUserNotificationCenterDelegate` och sedan kan du hoppa över den här delen.*</span><span class="sxs-lookup"><span data-stu-id="45eaa-126">*If neither your application or one of your third party libraries implements a `UNUserNotificationCenterDelegate` then you can skip this part.*</span></span>

<span data-ttu-id="45eaa-127">En `UNUserNotificationCenter` delegat som används av hello SDK toomonitor hello livscykel Engagement meddelanden på enheter som kör IOS 10 eller senare.</span><span class="sxs-lookup"><span data-stu-id="45eaa-127">A `UNUserNotificationCenter` delegate is used by hello SDK toomonitor hello life cycle of Engagement notifications on devices running on iOS 10 or greater.</span></span> <span data-ttu-id="45eaa-128">hello SDK har sin egen implementering av hello `UNUserNotificationCenterDelegate` protokoll, men det kan bara finnas ett `UNUserNotificationCenter` delegera per program.</span><span class="sxs-lookup"><span data-stu-id="45eaa-128">hello SDK has its own implementation of hello `UNUserNotificationCenterDelegate` protocol but there can be only one `UNUserNotificationCenter` delegate per application.</span></span> <span data-ttu-id="45eaa-129">Andra ombud läggs toohello `UNUserNotificationCenter` objekt står i konflikt med hello Engagement en.</span><span class="sxs-lookup"><span data-stu-id="45eaa-129">Any other delegate added toohello `UNUserNotificationCenter` object will conflict with hello Engagement one.</span></span> <span data-ttu-id="45eaa-130">Om hello SDK upptäcker din eller någon annan tredje parts ombud kommer det inte att använda sin egen implementering toogive hello du en chans tooresolve konflikter.</span><span class="sxs-lookup"><span data-stu-id="45eaa-130">If hello SDK detects your or any other third party's delegate then it will not use its own implementation toogive you a chance tooresolve hello conflicts.</span></span> <span data-ttu-id="45eaa-131">Du måste tooadd hello Engagement logik tooyour äger en delegat i ordning tooresolve hello konflikter.</span><span class="sxs-lookup"><span data-stu-id="45eaa-131">You will have tooadd hello Engagement logic tooyour own delegate in order tooresolve hello conflicts.</span></span>

<span data-ttu-id="45eaa-132">Det finns två sätt tooachieve detta.</span><span class="sxs-lookup"><span data-stu-id="45eaa-132">There are two ways tooachieve this.</span></span>

<span data-ttu-id="45eaa-133">Förslag 1, genom att helt enkelt vidarebefordra ombudet anropar toohello SDK:</span><span class="sxs-lookup"><span data-stu-id="45eaa-133">Proposal 1, simply by forwarding your delegate calls toohello SDK:</span></span>

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

<span data-ttu-id="45eaa-134">Eller förslag 2, med arv från hello `AEUserNotificationHandler` klass</span><span class="sxs-lookup"><span data-stu-id="45eaa-134">Or proposal 2, by inheriting from hello `AEUserNotificationHandler` class</span></span>

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
> <span data-ttu-id="45eaa-135">Du kan fastställa om ett meddelande kommer från Engagement eller inte genom att ange dess `userInfo` ordlista toohello Agent `isEngagementPushPayload:` klassen metoden.</span><span class="sxs-lookup"><span data-stu-id="45eaa-135">You can determine whether a notification comes from Engagement or not by passing its `userInfo` dictionary toohello Agent `isEngagementPushPayload:` class method.</span></span>

<span data-ttu-id="45eaa-136">Kontrollera att hello `UNUserNotificationCenter` objektets ombud anges tooyour ombud inom antingen hello `application:willFinishLaunchingWithOptions:` eller hello `application:didFinishLaunchingWithOptions:` metod för programdelegaten.</span><span class="sxs-lookup"><span data-stu-id="45eaa-136">Make sure that hello `UNUserNotificationCenter` object's delegate is set tooyour delegate within either hello `application:willFinishLaunchingWithOptions:` or hello `application:didFinishLaunchingWithOptions:` method of your application delegate.</span></span>
<span data-ttu-id="45eaa-137">Till exempel om du har implementerat hello ovan förslag 1:</span><span class="sxs-lookup"><span data-stu-id="45eaa-137">For instance, if you implemented hello above proposal 1:</span></span>

      - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
        // Any other code
  
        [UNUserNotificationCenter currentNotificationCenter].delegate = self;
        return YES;
      }

## <a name="from-200-too300"></a><span data-ttu-id="45eaa-138">Från 2.0.0 too3.0.0</span><span class="sxs-lookup"><span data-stu-id="45eaa-138">From 2.0.0 too3.0.0</span></span>
<span data-ttu-id="45eaa-139">Bort stöd för iOS 4.X.</span><span class="sxs-lookup"><span data-stu-id="45eaa-139">Dropped support for iOS 4.X.</span></span> <span data-ttu-id="45eaa-140">Från och med den här versionen hello distributionsmålet för ditt program måste vara minst iOS 6.</span><span class="sxs-lookup"><span data-stu-id="45eaa-140">Starting from this version hello deployment target of your application must be at least iOS 6.</span></span>

<span data-ttu-id="45eaa-141">Om du använder Reach i ditt program, måste du lägga till `remote-notification` värdet toohello `UIBackgroundModes` matris i filen Info.plist i ordning tooreceive remote meddelanden.</span><span class="sxs-lookup"><span data-stu-id="45eaa-141">If you are using Reach in your application, you must add `remote-notification` value toohello `UIBackgroundModes` array in your Info.plist file in order tooreceive remote notifications.</span></span>

<span data-ttu-id="45eaa-142">Hej metoden `application:didReceiveRemoteNotification:` måste ersättas med toobe `application:didReceiveRemoteNotification:fetchCompletionHandler:` i programdelegaten.</span><span class="sxs-lookup"><span data-stu-id="45eaa-142">hello method `application:didReceiveRemoteNotification:` needs toobe replaced by `application:didReceiveRemoteNotification:fetchCompletionHandler:` in your application delegate.</span></span>

<span data-ttu-id="45eaa-143">”AEPushDelegate.h” är föråldrad gränssnitt och du behöver tooremove alla referenser.</span><span class="sxs-lookup"><span data-stu-id="45eaa-143">"AEPushDelegate.h" is deprecated interface and you need tooremove all references.</span></span> <span data-ttu-id="45eaa-144">Detta inbegriper att ta bort `[[EngagementAgent shared] setPushDelegate:self]` och hello delegera metoder från programdelegaten:</span><span class="sxs-lookup"><span data-stu-id="45eaa-144">This includes removing `[[EngagementAgent shared] setPushDelegate:self]` and hello delegate methods from your application delegate:</span></span>

    -(void)willRetrieveLaunchMessage;
    -(void)didFailToRetrieveLaunchMessage;
    -(void)didReceiveLaunchMessage:(AEPushMessage*)launchMessage;

## <a name="from-1160-too200"></a><span data-ttu-id="45eaa-145">Från 1.16.0 too2.0.0</span><span class="sxs-lookup"><span data-stu-id="45eaa-145">From 1.16.0 too2.0.0</span></span>
<span data-ttu-id="45eaa-146">hello beskrivs nedan hur toomigrate SDK-integration från hello Capptain tjänsten erbjuds av Capptain SAS i en app med Azure Mobile Engagement.</span><span class="sxs-lookup"><span data-stu-id="45eaa-146">hello following describes how toomigrate an SDK integration from hello Capptain service offered by Capptain SAS into an app powered by Azure Mobile Engagement.</span></span>
<span data-ttu-id="45eaa-147">Om du migrerar från en tidigare version finns hello Capptain webbplats toomigrate too1.16 först sedan tillämpa hello nedan.</span><span class="sxs-lookup"><span data-stu-id="45eaa-147">If you are migrating from an earlier version, please consult hello Capptain web site toomigrate too1.16 first then apply hello following procedure.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="45eaa-148">Capptain och Mobile Engagement hello inte samma tjänster och hello proceduren endast anges nedan visar hur toomigrate hello-klientappen.</span><span class="sxs-lookup"><span data-stu-id="45eaa-148">Capptain and Mobile Engagement are not hello same services and hello procedure given below only highlights how toomigrate hello client app.</span></span> <span data-ttu-id="45eaa-149">Migrera hello SDK i hello app kommer inte att migrera data från hello Capptain servrar toohello Mobile Engagement-servrar</span><span class="sxs-lookup"><span data-stu-id="45eaa-149">Migrating hello SDK in hello app will NOT migrate your data from hello Capptain servers toohello Mobile Engagement servers</span></span>
> 
> 

### <a name="agent"></a><span data-ttu-id="45eaa-150">Agent</span><span class="sxs-lookup"><span data-stu-id="45eaa-150">Agent</span></span>
<span data-ttu-id="45eaa-151">Hej metoden `registerApp:` har ersatts av hello nya metoden `init:`.</span><span class="sxs-lookup"><span data-stu-id="45eaa-151">hello method `registerApp:` has been replaced by hello new method `init:`.</span></span> <span data-ttu-id="45eaa-152">Programdelegaten måste uppdateras och Använd anslutningssträng:</span><span class="sxs-lookup"><span data-stu-id="45eaa-152">Your application delegate must be updated accordingly and use connection string:</span></span>

            - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions
            {
              [...]
              [EngagementAgent init:@"YOUR_CONNECTION_STRING"];
              [...]
            }

<span data-ttu-id="45eaa-153">SmartAd spårning har tagits bort från SDK som du precis har tooremove alla instanser av `AETrackModule` klass</span><span class="sxs-lookup"><span data-stu-id="45eaa-153">SmartAd tracking has been removed from SDK you just have tooremove all instances of `AETrackModule` class</span></span>

### <a name="class-name-changes"></a><span data-ttu-id="45eaa-154">Klassen namnändringar</span><span class="sxs-lookup"><span data-stu-id="45eaa-154">Class Name Changes</span></span>
<span data-ttu-id="45eaa-155">Som en del av hello rebranding, finns det några klassen/filnamn som behöver toobe ändras.</span><span class="sxs-lookup"><span data-stu-id="45eaa-155">As part of hello rebranding, there are couple of class/file names that need toobe changed.</span></span>

<span data-ttu-id="45eaa-156">Alla klasser prefixet ”CP” ändras med ”AE” prefix.</span><span class="sxs-lookup"><span data-stu-id="45eaa-156">All classes prefixed with "CP" are renamed with "AE" prefix.</span></span>

<span data-ttu-id="45eaa-157">Exempel:</span><span class="sxs-lookup"><span data-stu-id="45eaa-157">Example:</span></span>

* <span data-ttu-id="45eaa-158">`CPModule.h`har bytt namn för`AEModule.h`.</span><span class="sxs-lookup"><span data-stu-id="45eaa-158">`CPModule.h` is renamed too`AEModule.h`.</span></span>

<span data-ttu-id="45eaa-159">Alla klasser prefixet ”Capptain” ändras med prefixet ”Engagement”.</span><span class="sxs-lookup"><span data-stu-id="45eaa-159">All classes prefixed with "Capptain" are renamed with "Engagement" prefix.</span></span>

<span data-ttu-id="45eaa-160">Exempel:</span><span class="sxs-lookup"><span data-stu-id="45eaa-160">Examples:</span></span>

* <span data-ttu-id="45eaa-161">Hej klassen `CapptainAgent` har bytt namn för`EngagementAgent`.</span><span class="sxs-lookup"><span data-stu-id="45eaa-161">hello class `CapptainAgent` is renamed too`EngagementAgent`.</span></span>
* <span data-ttu-id="45eaa-162">Hej klassen `CapptainTableViewController` har bytt namn för`EngagementTableViewController`.</span><span class="sxs-lookup"><span data-stu-id="45eaa-162">hello class `CapptainTableViewController` is renamed too`EngagementTableViewController`.</span></span>
* <span data-ttu-id="45eaa-163">Hej klassen `CapptainUtils` har bytt namn för`EngagementUtils`.</span><span class="sxs-lookup"><span data-stu-id="45eaa-163">hello class `CapptainUtils` is renamed too`EngagementUtils`.</span></span>
* <span data-ttu-id="45eaa-164">Hej klassen `CapptainViewController` har bytt namn för`EngagementViewController`.</span><span class="sxs-lookup"><span data-stu-id="45eaa-164">hello class `CapptainViewController` is renamed too`EngagementViewController`.</span></span>

