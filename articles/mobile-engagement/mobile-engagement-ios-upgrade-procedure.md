---
title: Azure Mobile Engagement iOS SDK uppgradera proceduren | Microsoft Docs
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
ms.openlocfilehash: 37c7f133d079186f828d58cabce0d2a259efd085
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="upgrade-procedures"></a><span data-ttu-id="334d3-103">Uppgraderingsprocesser</span><span class="sxs-lookup"><span data-stu-id="334d3-103">Upgrade procedures</span></span>
<span data-ttu-id="334d3-104">Om du redan har integrerat en äldre version av Engagement till programmet, måste du Tänk på följande när du uppgraderar SDK.</span><span class="sxs-lookup"><span data-stu-id="334d3-104">If you already have integrated an older version of Engagement into your application, you have to consider the following points when upgrading the SDK.</span></span>

<span data-ttu-id="334d3-105">För varje ny version av SDK måste du först ersätta (ta bort och importera på nytt i xcode) mapparna EngagementSDK och EngagementReach.</span><span class="sxs-lookup"><span data-stu-id="334d3-105">For each new version of the SDK you must first replace (remove and re-import in xcode) the EngagementSDK and EngagementReach folders.</span></span>

## <a name="from-300-to-400"></a><span data-ttu-id="334d3-106">Från 3.0.0 till 4.0.0</span><span class="sxs-lookup"><span data-stu-id="334d3-106">From 3.0.0 to 4.0.0</span></span>
### <a name="xcode-8"></a><span data-ttu-id="334d3-107">XCode 8</span><span class="sxs-lookup"><span data-stu-id="334d3-107">XCode 8</span></span>
<span data-ttu-id="334d3-108">XCode 8 är obligatoriskt från version 4.0.0 av SDK.</span><span class="sxs-lookup"><span data-stu-id="334d3-108">XCode 8 is mandatory starting from version 4.0.0 of the SDK.</span></span>

> [!NOTE]
> <span data-ttu-id="334d3-109">Om du verkligen är beroende av XCode 7 så att du kan använda den [iOS Engagement SDK v3.2.4](https://aka.ms/r6oouh).</span><span class="sxs-lookup"><span data-stu-id="334d3-109">If you really depend on XCode 7 then you may use the [iOS Engagement SDK v3.2.4](https://aka.ms/r6oouh).</span></span> <span data-ttu-id="334d3-110">Det finns ett känt fel på reach-modulen för den här tidigare version när du kör på iOS 10-enheter: systemmeddelanden är inte åtgärdade.</span><span class="sxs-lookup"><span data-stu-id="334d3-110">There is a known bug on the reach module of this previous version while running on iOS 10 devices:  system notifications are not actioned.</span></span> <span data-ttu-id="334d3-111">Åtgärda detta måste du implementera föråldrad API `application:didReceiveRemoteNotification:` i din app delegera på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="334d3-111">To fix this you will have to implement the deprecated API `application:didReceiveRemoteNotification:` in your app delegate as follows:</span></span>
> 
> 

    - (void)application:(UIApplication*)application
    didReceiveRemoteNotification:(NSDictionary*)userInfo
    {
        [[EngagementAgent shared] applicationDidReceiveRemoteNotification:userInfo fetchCompletionHandler:nil];
    }

> [!IMPORTANT]
> <span data-ttu-id="334d3-112">**Vi rekommenderar inte den här lösningen** som detta beteende kan ändras i kommande (även mindre) iOS version uppgraderar eftersom den här iOS API är inaktuell.</span><span class="sxs-lookup"><span data-stu-id="334d3-112">**We do not recommend this workaround** as this behavior can change in any upcoming (even minor) iOS version upgrade because this iOS API is deprecated.</span></span> <span data-ttu-id="334d3-113">Du måste växla till XCode 8 så snart som möjligt.</span><span class="sxs-lookup"><span data-stu-id="334d3-113">You should switch to XCode 8 as soon as possible.</span></span>
> 
> 

### <a name="usernotifications-framework"></a><span data-ttu-id="334d3-114">UserNotifications framework</span><span class="sxs-lookup"><span data-stu-id="334d3-114">UserNotifications framework</span></span>
<span data-ttu-id="334d3-115">Du måste lägga till den `UserNotifications` ramverk i din Build Phases.</span><span class="sxs-lookup"><span data-stu-id="334d3-115">You need to add the `UserNotifications` framework in your Build Phases.</span></span>

<span data-ttu-id="334d3-116">Öppna ditt projekt i Projektutforskaren, och välj rätt mål.</span><span class="sxs-lookup"><span data-stu-id="334d3-116">in the project explorer, open your project pane and select the correct target.</span></span> <span data-ttu-id="334d3-117">Öppna den **”Build-faser”** fliken och i den **”länka binär med bibliotek”** menyn Lägg till framework `UserNotifications.framework` -ange länken som`Optional`</span><span class="sxs-lookup"><span data-stu-id="334d3-117">Then, open the **"Build phases"** tab and in the **"Link Binary With Libraries"** menu, add framework `UserNotifications.framework` - set the link as `Optional`</span></span>

### <a name="application-push-capability"></a><span data-ttu-id="334d3-118">Programmet push-funktion</span><span class="sxs-lookup"><span data-stu-id="334d3-118">Application push capability</span></span>
<span data-ttu-id="334d3-119">XCode 8 kan återställa din app push-funktion, kontrollera den den `capability` fliken i ditt valda målet.</span><span class="sxs-lookup"><span data-stu-id="334d3-119">XCode 8 may reset your app push capability, please double check it in the `capability` tab of your selected target.</span></span>

### <a name="add-the-new-ios-10-notification-registration-code"></a><span data-ttu-id="334d3-120">Lägg till den nya iOS 10 registrering Meddelandekoden</span><span class="sxs-lookup"><span data-stu-id="334d3-120">Add the new iOS 10 notification registration code</span></span>
<span data-ttu-id="334d3-121">Äldre kodfragmentet att registrera app för meddelanden fungerar fortfarande men använder föråldrad API: er när du kör på iOS 10.</span><span class="sxs-lookup"><span data-stu-id="334d3-121">The older code snippet to register the app to notifications still works but is using deprecated APIs while running on iOS 10.</span></span>

<span data-ttu-id="334d3-122">Importera den `User Notification` framework:</span><span class="sxs-lookup"><span data-stu-id="334d3-122">Import the `User Notification` framework:</span></span>

        #import <UserNotifications/UserNotifications.h> 

<span data-ttu-id="334d3-123">I programdelegaten `application:didFinishLaunchingWithOptions` metoden Ersätt:</span><span class="sxs-lookup"><span data-stu-id="334d3-123">In your application delegate `application:didFinishLaunchingWithOptions` method replace:</span></span>

    if ([application respondsToSelector:@selector(registerUserNotificationSettings:)]) {
        [application registerUserNotificationSettings:[UIUserNotificationSettings settingsForTypes:(UIUserNotificationTypeBadge | UIUserNotificationTypeSound | UIUserNotificationTypeAlert) categories:nil]];
        [application registerForRemoteNotifications];
    }
    else {

        [application registerForRemoteNotificationTypes:(UIRemoteNotificationTypeBadge | UIRemoteNotificationTypeSound | UIRemoteNotificationTypeAlert)];
    }

<span data-ttu-id="334d3-124">av:</span><span class="sxs-lookup"><span data-stu-id="334d3-124">by :</span></span>

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

### <a name="resolve-unusernotificationcenter-delegate-conflicts"></a><span data-ttu-id="334d3-125">UNUserNotificationCenter ombud konfliktlösning</span><span class="sxs-lookup"><span data-stu-id="334d3-125">Resolve UNUserNotificationCenter delegate conflicts</span></span>

<span data-ttu-id="334d3-126">*Om varken ditt program eller en av tredjeparts-biblioteken implementerar en `UNUserNotificationCenterDelegate` och sedan kan du hoppa över den här delen.*</span><span class="sxs-lookup"><span data-stu-id="334d3-126">*If neither your application or one of your third party libraries implements a `UNUserNotificationCenterDelegate` then you can skip this part.*</span></span>

<span data-ttu-id="334d3-127">En `UNUserNotificationCenter` ombud används av SDK för att övervaka livscykeln för Engagement meddelanden på enheter som kör IOS 10 eller senare.</span><span class="sxs-lookup"><span data-stu-id="334d3-127">A `UNUserNotificationCenter` delegate is used by the SDK to monitor the life cycle of Engagement notifications on devices running on iOS 10 or greater.</span></span> <span data-ttu-id="334d3-128">SDK har sin egen implementering av den `UNUserNotificationCenterDelegate` protokoll, men det kan bara finnas ett `UNUserNotificationCenter` delegera per program.</span><span class="sxs-lookup"><span data-stu-id="334d3-128">The SDK has its own implementation of the `UNUserNotificationCenterDelegate` protocol but there can be only one `UNUserNotificationCenter` delegate per application.</span></span> <span data-ttu-id="334d3-129">Andra ombud som lagts till i `UNUserNotificationCenter` objekt står i konflikt med en Engagement.</span><span class="sxs-lookup"><span data-stu-id="334d3-129">Any other delegate added to the `UNUserNotificationCenter` object will conflict with the Engagement one.</span></span> <span data-ttu-id="334d3-130">Om SDK upptäcker din eller någon annan tredje parts ombud kommer den inte använda sin egen implementering som ger dig möjlighet att lösa konflikter.</span><span class="sxs-lookup"><span data-stu-id="334d3-130">If the SDK detects your or any other third party's delegate then it will not use its own implementation to give you a chance to resolve the conflicts.</span></span> <span data-ttu-id="334d3-131">Du måste lägga till Engagement-kod till din egen ombud för att lösa konflikter.</span><span class="sxs-lookup"><span data-stu-id="334d3-131">You will have to add the Engagement logic to your own delegate in order to resolve the conflicts.</span></span>

<span data-ttu-id="334d3-132">Det finns två sätt att göra detta.</span><span class="sxs-lookup"><span data-stu-id="334d3-132">There are two ways to achieve this.</span></span>

<span data-ttu-id="334d3-133">Förslag 1, genom att vidarebefordra ombudet anrop till SDK:</span><span class="sxs-lookup"><span data-stu-id="334d3-133">Proposal 1, simply by forwarding your delegate calls to the SDK:</span></span>

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

<span data-ttu-id="334d3-134">Eller förslag 2, som ärver från den `AEUserNotificationHandler` klass</span><span class="sxs-lookup"><span data-stu-id="334d3-134">Or proposal 2, by inheriting from the `AEUserNotificationHandler` class</span></span>

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
> <span data-ttu-id="334d3-135">Du kan fastställa om ett meddelande kommer från Engagement eller inte genom att ange dess `userInfo` ordlista till agenten `isEngagementPushPayload:` klassen metoden.</span><span class="sxs-lookup"><span data-stu-id="334d3-135">You can determine whether a notification comes from Engagement or not by passing its `userInfo` dictionary to the Agent `isEngagementPushPayload:` class method.</span></span>

<span data-ttu-id="334d3-136">Se till att den `UNUserNotificationCenter` objektets ombud är inställd på ombudet i antingen den `application:willFinishLaunchingWithOptions:` eller `application:didFinishLaunchingWithOptions:` -metoden i programdelegaten.</span><span class="sxs-lookup"><span data-stu-id="334d3-136">Make sure that the `UNUserNotificationCenter` object's delegate is set to your delegate within either the `application:willFinishLaunchingWithOptions:` or the `application:didFinishLaunchingWithOptions:` method of your application delegate.</span></span>
<span data-ttu-id="334d3-137">Till exempel om du har implementerat ovan förslag 1:</span><span class="sxs-lookup"><span data-stu-id="334d3-137">For instance, if you implemented the above proposal 1:</span></span>

      - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
        // Any other code
  
        [UNUserNotificationCenter currentNotificationCenter].delegate = self;
        return YES;
      }

## <a name="from-200-to-300"></a><span data-ttu-id="334d3-138">Från 2.0.0 till 3.0.0</span><span class="sxs-lookup"><span data-stu-id="334d3-138">From 2.0.0 to 3.0.0</span></span>
<span data-ttu-id="334d3-139">Bort stöd för iOS 4.X.</span><span class="sxs-lookup"><span data-stu-id="334d3-139">Dropped support for iOS 4.X.</span></span> <span data-ttu-id="334d3-140">Startar från den här versionen av distributionsmålet för ditt program måste vara minst iOS 6.</span><span class="sxs-lookup"><span data-stu-id="334d3-140">Starting from this version the deployment target of your application must be at least iOS 6.</span></span>

<span data-ttu-id="334d3-141">Om du använder Reach i ditt program, måste du lägga till `remote-notification` värde till den `UIBackgroundModes` matris i filen Info.plist för att kunna ta emot meddelanden för fjärråtkomst.</span><span class="sxs-lookup"><span data-stu-id="334d3-141">If you are using Reach in your application, you must add `remote-notification` value to the `UIBackgroundModes` array in your Info.plist file in order to receive remote notifications.</span></span>

<span data-ttu-id="334d3-142">Metoden `application:didReceiveRemoteNotification:` måste ersättas med `application:didReceiveRemoteNotification:fetchCompletionHandler:` i programdelegaten.</span><span class="sxs-lookup"><span data-stu-id="334d3-142">The method `application:didReceiveRemoteNotification:` needs to be replaced by `application:didReceiveRemoteNotification:fetchCompletionHandler:` in your application delegate.</span></span>

<span data-ttu-id="334d3-143">”AEPushDelegate.h” är föråldrad gränssnitt och du måste ta bort alla referenser.</span><span class="sxs-lookup"><span data-stu-id="334d3-143">"AEPushDelegate.h" is deprecated interface and you need to remove all references.</span></span> <span data-ttu-id="334d3-144">Detta inbegriper att ta bort `[[EngagementAgent shared] setPushDelegate:self]` och delegatmetoder från programdelegaten:</span><span class="sxs-lookup"><span data-stu-id="334d3-144">This includes removing `[[EngagementAgent shared] setPushDelegate:self]` and the delegate methods from your application delegate:</span></span>

    -(void)willRetrieveLaunchMessage;
    -(void)didFailToRetrieveLaunchMessage;
    -(void)didReceiveLaunchMessage:(AEPushMessage*)launchMessage;

## <a name="from-1160-to-200"></a><span data-ttu-id="334d3-145">Från 1.16.0 till 2.0.0</span><span class="sxs-lookup"><span data-stu-id="334d3-145">From 1.16.0 to 2.0.0</span></span>
<span data-ttu-id="334d3-146">Nedan beskrivs hur du migrerar en SDK-integration från tjänsten Capptain som erbjuds av Capptain SAS i en app med Azure Mobile Engagement.</span><span class="sxs-lookup"><span data-stu-id="334d3-146">The following describes how to migrate an SDK integration from the Capptain service offered by Capptain SAS into an app powered by Azure Mobile Engagement.</span></span>
<span data-ttu-id="334d3-147">Kontakta Capptain webbplats om du vill migrera till 1.16 först och sedan använda följande procedur om du migrerar från en tidigare version.</span><span class="sxs-lookup"><span data-stu-id="334d3-147">If you are migrating from an earlier version, please consult the Capptain web site to migrate to 1.16 first then apply the following procedure.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="334d3-148">Capptain och Mobile Engagement är inte samma tjänster och proceduren nedan visar hur du migrerar klientappen endast.</span><span class="sxs-lookup"><span data-stu-id="334d3-148">Capptain and Mobile Engagement are not the same services and the procedure given below only highlights how to migrate the client app.</span></span> <span data-ttu-id="334d3-149">Migrera SDK i appen kommer inte migrera dina data från servrar som Capptain till Mobile Engagement-servrar</span><span class="sxs-lookup"><span data-stu-id="334d3-149">Migrating the SDK in the app will NOT migrate your data from the Capptain servers to the Mobile Engagement servers</span></span>
> 
> 

### <a name="agent"></a><span data-ttu-id="334d3-150">Agent</span><span class="sxs-lookup"><span data-stu-id="334d3-150">Agent</span></span>
<span data-ttu-id="334d3-151">Metoden `registerApp:` har ersatts av den nya metoden `init:`.</span><span class="sxs-lookup"><span data-stu-id="334d3-151">The method `registerApp:` has been replaced by the new method `init:`.</span></span> <span data-ttu-id="334d3-152">Programdelegaten måste uppdateras och Använd anslutningssträng:</span><span class="sxs-lookup"><span data-stu-id="334d3-152">Your application delegate must be updated accordingly and use connection string:</span></span>

            - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions
            {
              [...]
              [EngagementAgent init:@"YOUR_CONNECTION_STRING"];
              [...]
            }

<span data-ttu-id="334d3-153">SmartAd spårning har tagits bort från SDK som du behöver bara ta bort alla förekomster av `AETrackModule` klass</span><span class="sxs-lookup"><span data-stu-id="334d3-153">SmartAd tracking has been removed from SDK you just have to remove all instances of `AETrackModule` class</span></span>

### <a name="class-name-changes"></a><span data-ttu-id="334d3-154">Klassen namnändringar</span><span class="sxs-lookup"><span data-stu-id="334d3-154">Class Name Changes</span></span>
<span data-ttu-id="334d3-155">Som en del av den rebranding finns det några klassen/filnamn som behöver ändras.</span><span class="sxs-lookup"><span data-stu-id="334d3-155">As part of the rebranding, there are couple of class/file names that need to be changed.</span></span>

<span data-ttu-id="334d3-156">Alla klasser prefixet ”CP” ändras med ”AE” prefix.</span><span class="sxs-lookup"><span data-stu-id="334d3-156">All classes prefixed with "CP" are renamed with "AE" prefix.</span></span>

<span data-ttu-id="334d3-157">Exempel:</span><span class="sxs-lookup"><span data-stu-id="334d3-157">Example:</span></span>

* <span data-ttu-id="334d3-158">`CPModule.h`har bytt namn till `AEModule.h`.</span><span class="sxs-lookup"><span data-stu-id="334d3-158">`CPModule.h` is renamed to `AEModule.h`.</span></span>

<span data-ttu-id="334d3-159">Alla klasser prefixet ”Capptain” ändras med prefixet ”Engagement”.</span><span class="sxs-lookup"><span data-stu-id="334d3-159">All classes prefixed with "Capptain" are renamed with "Engagement" prefix.</span></span>

<span data-ttu-id="334d3-160">Exempel:</span><span class="sxs-lookup"><span data-stu-id="334d3-160">Examples:</span></span>

* <span data-ttu-id="334d3-161">Klassen `CapptainAgent` ändras till `EngagementAgent`.</span><span class="sxs-lookup"><span data-stu-id="334d3-161">The class `CapptainAgent` is renamed to `EngagementAgent`.</span></span>
* <span data-ttu-id="334d3-162">Klassen `CapptainTableViewController` ändras till `EngagementTableViewController`.</span><span class="sxs-lookup"><span data-stu-id="334d3-162">The class `CapptainTableViewController` is renamed to `EngagementTableViewController`.</span></span>
* <span data-ttu-id="334d3-163">Klassen `CapptainUtils` ändras till `EngagementUtils`.</span><span class="sxs-lookup"><span data-stu-id="334d3-163">The class `CapptainUtils` is renamed to `EngagementUtils`.</span></span>
* <span data-ttu-id="334d3-164">Klassen `CapptainViewController` ändras till `EngagementViewController`.</span><span class="sxs-lookup"><span data-stu-id="334d3-164">The class `CapptainViewController` is renamed to `EngagementViewController`.</span></span>

