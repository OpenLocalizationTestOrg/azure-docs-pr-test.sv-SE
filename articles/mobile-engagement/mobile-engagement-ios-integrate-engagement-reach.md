---
title: "aaaAzure Mobile Engagement iOS SDK nå Integration | Microsoft Docs"
description: "Senaste uppdateringarna och procedurer för iOS SDK för Azure Mobile Engagement"
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: 1f5f5857-867c-40c5-9d76-675a343a0296
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-ios
ms.devlang: objective-c
ms.topic: article
ms.date: 12/13/2016
ms.author: piyushjo
ms.openlocfilehash: 40c9bfbdb475ab0b97bdbc9cea798a59cb8a71ac
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toointegrate-engagement-reach-on-ios"></a><span data-ttu-id="5a3e9-103">Hur tooIntegrate Engagement nå på iOS</span><span class="sxs-lookup"><span data-stu-id="5a3e9-103">How tooIntegrate Engagement Reach on iOS</span></span>
<span data-ttu-id="5a3e9-104">Du måste följa hello integration beskrivs i hello [hur tooIntegrate Engagement för iOS-dokument](mobile-engagement-ios-integrate-engagement.md) innan du följer den här guiden.</span><span class="sxs-lookup"><span data-stu-id="5a3e9-104">You must follow hello integration procedure described in hello [How tooIntegrate Engagement on iOS document](mobile-engagement-ios-integrate-engagement.md) before following this guide.</span></span>

<span data-ttu-id="5a3e9-105">Den här dokumentationen kräver XCode 8.</span><span class="sxs-lookup"><span data-stu-id="5a3e9-105">This documentation requires XCode 8.</span></span> <span data-ttu-id="5a3e9-106">Om du verkligen är beroende av XCode 7 så att du kan använda hello [iOS Engagement SDK v3.2.4](https://aka.ms/r6oouh).</span><span class="sxs-lookup"><span data-stu-id="5a3e9-106">If you really depend on XCode 7 then you may use hello [iOS Engagement SDK v3.2.4](https://aka.ms/r6oouh).</span></span> <span data-ttu-id="5a3e9-107">Det finns ett känt fel på den här tidigare version när du kör på iOS 10-enheter: systemmeddelanden är inte åtgärdade.</span><span class="sxs-lookup"><span data-stu-id="5a3e9-107">There is a known bug on this previous version while running on iOS 10 devices:  system notifications are not actioned.</span></span> <span data-ttu-id="5a3e9-108">toofix detta behöver tooimplement hello föråldrad API `application:didReceiveRemoteNotification:` i din app delegera på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="5a3e9-108">toofix this you will have tooimplement hello deprecated API `application:didReceiveRemoteNotification:` in your app delegate as follows:</span></span>

    - (void)application:(UIApplication*)application
    didReceiveRemoteNotification:(NSDictionary*)userInfo
    {
        [[EngagementAgent shared] applicationDidReceiveRemoteNotification:userInfo fetchCompletionHandler:nil];
    }

> [!IMPORTANT]
> <span data-ttu-id="5a3e9-109">**Vi rekommenderar inte den här lösningen** som detta beteende kan ändras i kommande (även mindre) iOS version uppgraderar eftersom den här iOS API är inaktuell.</span><span class="sxs-lookup"><span data-stu-id="5a3e9-109">**We do not recommend this workaround** as this behavior can change in any upcoming (even minor) iOS version upgrade because this iOS API is deprecated.</span></span> <span data-ttu-id="5a3e9-110">Du bör växla tooXCode 8 så snart som möjligt.</span><span class="sxs-lookup"><span data-stu-id="5a3e9-110">You should switch tooXCode 8 as soon as possible.</span></span>
>
>

### <a name="enable-your-app-tooreceive-silent-push-notifications"></a><span data-ttu-id="5a3e9-111">Aktivera din app tooreceive tysta Push-meddelanden</span><span class="sxs-lookup"><span data-stu-id="5a3e9-111">Enable your app tooreceive Silent Push Notifications</span></span>
[!INCLUDE [mobile-engagement-ios-silent-push](../../includes/mobile-engagement-ios-silent-push.md)]

## <a name="integration-steps"></a><span data-ttu-id="5a3e9-112">Integreringen</span><span class="sxs-lookup"><span data-stu-id="5a3e9-112">Integration steps</span></span>
### <a name="embed-hello-engagement-reach-sdk-into-your-ios-project"></a><span data-ttu-id="5a3e9-113">Bädda in hello Engagement nå SDK i din iOS-projekt</span><span class="sxs-lookup"><span data-stu-id="5a3e9-113">Embed hello Engagement Reach SDK into your iOS project</span></span>
* <span data-ttu-id="5a3e9-114">Lägg till hello Reach sdk i Xcode-projektet.</span><span class="sxs-lookup"><span data-stu-id="5a3e9-114">Add hello Reach sdk in your Xcode project.</span></span> <span data-ttu-id="5a3e9-115">I Xcode gå för**projekt \> lägga till tooproject** och välj hello `EngagementReach` mapp.</span><span class="sxs-lookup"><span data-stu-id="5a3e9-115">In Xcode, go too**Project \> Add tooproject** and choose hello `EngagementReach` folder.</span></span>

### <a name="modify-your-application-delegate"></a><span data-ttu-id="5a3e9-116">Ändra programdelegaten</span><span class="sxs-lookup"><span data-stu-id="5a3e9-116">Modify your Application Delegate</span></span>
* <span data-ttu-id="5a3e9-117">Importera hello Engagement nå modulen hello över din implementering fil:</span><span class="sxs-lookup"><span data-stu-id="5a3e9-117">At hello top of your implementation file, import hello Engagement Reach module:</span></span>

      [...]
      #import "AEReachModule.h"
* <span data-ttu-id="5a3e9-118">Inuti metoden `applicationDidFinishLaunching:` eller `application:didFinishLaunchingWithOptions:`, skapar du en räckviddsmodul och skickar den tooyour befintliga initieringsrad:</span><span class="sxs-lookup"><span data-stu-id="5a3e9-118">Inside method `applicationDidFinishLaunching:` or `application:didFinishLaunchingWithOptions:`, create a reach module and pass it tooyour existing Engagement initialization line:</span></span>

      - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
        AEReachModule* reach = [AEReachModule moduleWithNotificationIcon:[UIImage imageNamed:@"icon.png"]];
        [EngagementAgent init:@"Endpoint={YOUR_APP_COLLECTION.DOMAIN};SdkKey={YOUR_SDK_KEY};AppId={YOUR_APPID}" modules:reach, nil];
        [...]

        return YES;
      }
* <span data-ttu-id="5a3e9-119">Ändra **'icon.png'** med hello avbildningens namn som du vill ha som ditt meddelandeikonen.</span><span class="sxs-lookup"><span data-stu-id="5a3e9-119">Modify **'icon.png'** string with hello image name you want as your notification icon.</span></span>
* <span data-ttu-id="5a3e9-120">Om du vill toouse hello alternativet *Aktivitetsikon uppdateringsvärde* i Reach-kampanjer eller om du vill toouse interna push \<SaaS Reach/API/kampanj format/intern Push\> kampanjer, måste du låta hello Reach-modulen hantera hello Aktivitetsikon ikonen sig själv (den tas bort automatiskt i hello programmet Aktivitetsikon och även återställa hello-värde som lagras av Engagement varje gång hello program är igång eller foregrounded).</span><span class="sxs-lookup"><span data-stu-id="5a3e9-120">If you want toouse hello option *Update badge value* in Reach campaigns or if you want toouse native push \</SaaS/Reach API/Campaign format/Native Push\> campaigns, you must let hello Reach module manage hello badge icon itself (it will automatically clear hello application badge and also reset hello value stored by Engagement every time hello application is started or foregrounded).</span></span> <span data-ttu-id="5a3e9-121">Detta görs genom att lägga till följande rad efter Reach-modulen initiering hello:</span><span class="sxs-lookup"><span data-stu-id="5a3e9-121">This is done by adding hello following line after Reach module initialization:</span></span>

      [reach setAutoBadgeEnabled:YES];
* <span data-ttu-id="5a3e9-122">Om du vill toohandle Reach data-push, måste du låta programdelegaten överensstämmer toohello `AEReachDataPushDelegate` protokoll.</span><span class="sxs-lookup"><span data-stu-id="5a3e9-122">If you want toohandle Reach data push, you must let your Application delegate conform toohello `AEReachDataPushDelegate` protocol.</span></span> <span data-ttu-id="5a3e9-123">Lägg till följande rad efter Reach-modulen initiering hello:</span><span class="sxs-lookup"><span data-stu-id="5a3e9-123">Add hello following line after Reach module initialization:</span></span>

      [reach setDataPushDelegate:self];
* <span data-ttu-id="5a3e9-124">Sedan kan du implementera hello metoder `onDataPushStringReceived:` och `onDataPushBase64ReceivedWithDecodedBody:andEncodedBody:` i programdelegaten:</span><span class="sxs-lookup"><span data-stu-id="5a3e9-124">Then you can implement hello methods `onDataPushStringReceived:` and `onDataPushBase64ReceivedWithDecodedBody:andEncodedBody:` in your application delegate:</span></span>

      -(BOOL)didReceiveStringDataPushWithCategory:(NSString*)category body:(NSString*)body
      {
         NSLog(@"String data push message with category <%@> received: %@", category, body);
         return YES;
      }

      -(BOOL)didReceiveBase64DataPushWithCategory:(NSString*)category decodedBody:(NSData *)decodedBody encodedBody:(NSString *)encodedBody
      {
         NSLog(@"Base64 data push message with category <%@> received: %@", category, encodedBody);
         // Do something useful with decodedBody like updating an image view
         return YES;
      }

### <a name="category"></a><span data-ttu-id="5a3e9-125">Kategori</span><span class="sxs-lookup"><span data-stu-id="5a3e9-125">Category</span></span>
<span data-ttu-id="5a3e9-126">hello kategori parametern är valfri när du skapar en Data-Push-kampanj och gör att du toofilter data push-meddelanden.</span><span class="sxs-lookup"><span data-stu-id="5a3e9-126">hello category parameter is optional when you create a Data Push campaign and allows you toofilter data pushes.</span></span> <span data-ttu-id="5a3e9-127">Detta är användbart om du vill toopush olika typer av `Base64` data och vill tooidentify typ innan parsningen dem.</span><span class="sxs-lookup"><span data-stu-id="5a3e9-127">This is useful if you want toopush different kinds of `Base64` data and want tooidentify their type before parsing them.</span></span>

<span data-ttu-id="5a3e9-128">**Tillämpningsprogrammet är nu redo tooreceive och visa nå innehållet!**</span><span class="sxs-lookup"><span data-stu-id="5a3e9-128">**Your application is now ready tooreceive and display reach contents!**</span></span>

## <a name="how-tooreceive-announcements-and-polls-at-any-time"></a><span data-ttu-id="5a3e9-129">Hur tooreceive meddelanden och avsökningar när som helst</span><span class="sxs-lookup"><span data-stu-id="5a3e9-129">How tooreceive announcements and polls at any time</span></span>
<span data-ttu-id="5a3e9-130">Engagement kan skicka Reach meddelanden tooyour slutanvändare när som helst genom att använda hello Apple Push Notification Service.</span><span class="sxs-lookup"><span data-stu-id="5a3e9-130">Engagement can send Reach notifications tooyour end users at any time by using hello Apple Push Notification Service.</span></span>

<span data-ttu-id="5a3e9-131">tooenable den här funktionen kan du ha tooprepare ditt program för Apple push-meddelanden och ändra programdelegaten.</span><span class="sxs-lookup"><span data-stu-id="5a3e9-131">tooenable this functionality, you'll have tooprepare your application for Apple push notifications and modify your application delegate.</span></span>

### <a name="prepare-your-application-for-apple-push-notifications"></a><span data-ttu-id="5a3e9-132">Förbered ditt program för Apple push-meddelanden</span><span class="sxs-lookup"><span data-stu-id="5a3e9-132">Prepare your application for Apple push notifications</span></span>
<span data-ttu-id="5a3e9-133">Följ guiden hello: [hur tooPrepare ditt program för Apple Push-meddelanden](https://developer.apple.com/library/ios/documentation/IDEs/Conceptual/AppDistributionGuide/AddingCapabilities/AddingCapabilities.html#//apple_ref/doc/uid/TP40012582-CH26-SW6)</span><span class="sxs-lookup"><span data-stu-id="5a3e9-133">Please follow hello guide : [How tooPrepare your Application for Apple Push Notifications](https://developer.apple.com/library/ios/documentation/IDEs/Conceptual/AppDistributionGuide/AddingCapabilities/AddingCapabilities.html#//apple_ref/doc/uid/TP40012582-CH26-SW6)</span></span>

### <a name="add-hello-necessary-client-code"></a><span data-ttu-id="5a3e9-134">Lägg till hello nödvändiga klientkod</span><span class="sxs-lookup"><span data-stu-id="5a3e9-134">Add hello necessary client code</span></span>
<span data-ttu-id="5a3e9-135">*Ditt program bör nu ha en registrerad Apple push-certifikat hello Engagement klientdel.*</span><span class="sxs-lookup"><span data-stu-id="5a3e9-135">*At this point your application should have a registered Apple push certificate in hello Engagement frontend.*</span></span>

<span data-ttu-id="5a3e9-136">Om det inte är redan gjort måste tooregister programmet tooreceive push-meddelanden.</span><span class="sxs-lookup"><span data-stu-id="5a3e9-136">If it's not done already, you need tooregister your application tooreceive push notifications.</span></span>

* <span data-ttu-id="5a3e9-137">Importera hello `User Notification` framework:</span><span class="sxs-lookup"><span data-stu-id="5a3e9-137">Import hello `User Notification` framework:</span></span>

        #import <UserNotifications/UserNotifications.h>
* <span data-ttu-id="5a3e9-138">Lägg till följande rad när programmet startas hello (vanligtvis i `application:didFinishLaunchingWithOptions:`):</span><span class="sxs-lookup"><span data-stu-id="5a3e9-138">Add hello following line when your application starts (typically in `application:didFinishLaunchingWithOptions:`):</span></span>

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

<span data-ttu-id="5a3e9-139">Du måste sedan tooprovide tooEngagement hello enhetstoken som returnerades av Apple-servrar.</span><span class="sxs-lookup"><span data-stu-id="5a3e9-139">Then, You need tooprovide tooEngagement hello device token returned by Apple servers.</span></span> <span data-ttu-id="5a3e9-140">Detta görs i hello metod med namnet `application:didRegisterForRemoteNotificationsWithDeviceToken:` i programdelegaten:</span><span class="sxs-lookup"><span data-stu-id="5a3e9-140">This is done in hello method named `application:didRegisterForRemoteNotificationsWithDeviceToken:` in your application delegate:</span></span>

    - (void)application:(UIApplication*)application didRegisterForRemoteNotificationsWithDeviceToken:(NSData*)deviceToken
    {
        [[EngagementAgent shared] registerDeviceToken:deviceToken];
    }

<span data-ttu-id="5a3e9-141">Slutligen har du tooinform hello Engagement SDK när programmet tar emot ett meddelande om fjärråtkomst.</span><span class="sxs-lookup"><span data-stu-id="5a3e9-141">Finally, you have tooinform hello Engagement SDK when your application receives a remote notification.</span></span> <span data-ttu-id="5a3e9-142">toodo som anropar hello metoden `applicationDidReceiveRemoteNotification:fetchCompletionHandler:` i programdelegaten:</span><span class="sxs-lookup"><span data-stu-id="5a3e9-142">toodo that, call hello method `applicationDidReceiveRemoteNotification:fetchCompletionHandler:` in your application delegate:</span></span>

    - (void)application:(UIApplication*)application didReceiveRemoteNotification:(NSDictionary*)userInfo fetchCompletionHandler:(void (^)(UIBackgroundFetchResult result))handler
    {
        [[EngagementAgent shared] applicationDidReceiveRemoteNotification:userInfo fetchCompletionHandler:handler];
    }

> [!IMPORTANT]
> <span data-ttu-id="5a3e9-143">Som standard kontrollerar Engagement nå hello completionHandler.</span><span class="sxs-lookup"><span data-stu-id="5a3e9-143">By default, Engagement Reach controls hello completionHandler.</span></span> <span data-ttu-id="5a3e9-144">Om du vill toomanually svara toohello `handler` blockera i koden, du kan skicka nil för hello `handler` argumentet och kontroll hello slutförande blockera själv.</span><span class="sxs-lookup"><span data-stu-id="5a3e9-144">If you want toomanually respond toohello `handler` block in your code, you can pass nil for hello `handler` argument and control hello completion block yourself.</span></span> <span data-ttu-id="5a3e9-145">Se hello `UIBackgroundFetchResult` typ för en lista över möjliga värden.</span><span class="sxs-lookup"><span data-stu-id="5a3e9-145">See hello `UIBackgroundFetchResult` type for a list of possible values.</span></span>
>
>

### <a name="full-example"></a><span data-ttu-id="5a3e9-146">Fullständigt exempel</span><span class="sxs-lookup"><span data-stu-id="5a3e9-146">Full example</span></span>
<span data-ttu-id="5a3e9-147">Här är ett fullständigt exempel på integrering:</span><span class="sxs-lookup"><span data-stu-id="5a3e9-147">Here is a full example of integration:</span></span>

    #pragma mark -
    #pragma mark Application lifecycle

    - (BOOL)application:(UIApplication*)application didFinishLaunchingWithOptions:(NSDictionary*)launchOptions
    {
      /* Reach module */
      AEReachModule* reach = [AEReachModule moduleWithNotificationIcon:[UIImage imageNamed:@"icon.png"]];
      [reach setAutoBadgeEnabled:YES];

      /* Engagement initialization */
      [EngagementAgent init:@"Endpoint={YOUR_APP_COLLECTION.DOMAIN};SdkKey={YOUR_SDK_KEY};AppId={YOUR_APPID}" modules:reach, nil];
      [[EngagementAgent shared] setPushDelegate:self];

      /* Views */
      [window addSubview:[tabBarController view]];
      [window makeKeyAndVisible];

      [application registerForRemoteNotificationTypes:UIRemoteNotificationTypeAlert|UIRemoteNotificationTypeBadge|UIRemoteNotificationTypeSound];
      return YES;
    }

    - (void)application:(UIApplication*)application didRegisterForRemoteNotificationsWithDeviceToken:(NSData*)deviceToken
    {
      [[EngagementAgent shared] registerDeviceToken:deviceToken];
    }

    - (void)application:(UIApplication *)application didReceiveRemoteNotification:(NSDictionary *)userInfo fetchCompletionHandler:(void (^)(UIBackgroundFetchResult result))handler
    {
        [[EngagementAgent shared] applicationDidReceiveRemoteNotification:userInfo fetchCompletionHandler:handler];
    }

### <a name="resolve-unusernotificationcenter-delegate-conflicts"></a><span data-ttu-id="5a3e9-148">UNUserNotificationCenter ombud konfliktlösning</span><span class="sxs-lookup"><span data-stu-id="5a3e9-148">Resolve UNUserNotificationCenter delegate conflicts</span></span>

<span data-ttu-id="5a3e9-149">*Om varken ditt program eller en av tredjeparts-biblioteken implementerar en `UNUserNotificationCenterDelegate` och sedan kan du hoppa över den här delen.*</span><span class="sxs-lookup"><span data-stu-id="5a3e9-149">*If neither your application or one of your third party libraries implements a `UNUserNotificationCenterDelegate` then you can skip this part.*</span></span>

<span data-ttu-id="5a3e9-150">En `UNUserNotificationCenter` delegat som används av hello SDK toomonitor hello livscykel Engagement meddelanden på enheter som kör IOS 10 eller senare.</span><span class="sxs-lookup"><span data-stu-id="5a3e9-150">A `UNUserNotificationCenter` delegate is used by hello SDK toomonitor hello life cycle of Engagement notifications on devices running on iOS 10 or greater.</span></span> <span data-ttu-id="5a3e9-151">hello SDK har sin egen implementering av hello `UNUserNotificationCenterDelegate` protokoll, men det kan bara finnas ett `UNUserNotificationCenter` delegera per program.</span><span class="sxs-lookup"><span data-stu-id="5a3e9-151">hello SDK has its own implementation of hello `UNUserNotificationCenterDelegate` protocol but there can be only one `UNUserNotificationCenter` delegate per application.</span></span> <span data-ttu-id="5a3e9-152">Andra ombud läggs toohello `UNUserNotificationCenter` objekt står i konflikt med hello Engagement en.</span><span class="sxs-lookup"><span data-stu-id="5a3e9-152">Any other delegate added toohello `UNUserNotificationCenter` object will conflict with hello Engagement one.</span></span> <span data-ttu-id="5a3e9-153">Om hello SDK upptäcker din eller någon annan tredje parts ombud kommer det inte att använda sin egen implementering toogive hello du en chans tooresolve konflikter.</span><span class="sxs-lookup"><span data-stu-id="5a3e9-153">If hello SDK detects your or any other third party's delegate then it will not use its own implementation toogive you a chance tooresolve hello conflicts.</span></span> <span data-ttu-id="5a3e9-154">Du måste tooadd hello Engagement logik tooyour äger en delegat i ordning tooresolve hello konflikter.</span><span class="sxs-lookup"><span data-stu-id="5a3e9-154">You will have tooadd hello Engagement logic tooyour own delegate in order tooresolve hello conflicts.</span></span>

<span data-ttu-id="5a3e9-155">Det finns två sätt tooachieve detta.</span><span class="sxs-lookup"><span data-stu-id="5a3e9-155">There are two ways tooachieve this.</span></span>

<span data-ttu-id="5a3e9-156">Förslag 1, genom att helt enkelt vidarebefordra ombudet anropar toohello SDK:</span><span class="sxs-lookup"><span data-stu-id="5a3e9-156">Proposal 1, simply by forwarding your delegate calls toohello SDK:</span></span>

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

<span data-ttu-id="5a3e9-157">Eller förslag 2, med arv från hello `AEUserNotificationHandler` klass</span><span class="sxs-lookup"><span data-stu-id="5a3e9-157">Or proposal 2, by inheriting from hello `AEUserNotificationHandler` class</span></span>

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
> <span data-ttu-id="5a3e9-158">Du kan fastställa om ett meddelande kommer från Engagement eller inte genom att ange dess `userInfo` ordlista toohello Agent `isEngagementPushPayload:` klassen metoden.</span><span class="sxs-lookup"><span data-stu-id="5a3e9-158">You can determine whether a notification comes from Engagement or not by passing its `userInfo` dictionary toohello Agent `isEngagementPushPayload:` class method.</span></span>

<span data-ttu-id="5a3e9-159">Kontrollera att hello `UNUserNotificationCenter` objektets ombud anges tooyour ombud inom antingen hello `application:willFinishLaunchingWithOptions:` eller hello `application:didFinishLaunchingWithOptions:` metod för programdelegaten.</span><span class="sxs-lookup"><span data-stu-id="5a3e9-159">Make sure that hello `UNUserNotificationCenter` object's delegate is set tooyour delegate within either hello `application:willFinishLaunchingWithOptions:` or hello `application:didFinishLaunchingWithOptions:` method of your application delegate.</span></span>
<span data-ttu-id="5a3e9-160">Till exempel om du har implementerat hello ovan förslag 1:</span><span class="sxs-lookup"><span data-stu-id="5a3e9-160">For instance, if you implemented hello above proposal 1:</span></span>

      - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
        // Any other code

        [UNUserNotificationCenter currentNotificationCenter].delegate = self;
        return YES;
      }

## <a name="how-toocustomize-campaigns"></a><span data-ttu-id="5a3e9-161">Hur toocustomize kampanjer</span><span class="sxs-lookup"><span data-stu-id="5a3e9-161">How toocustomize campaigns</span></span>
### <a name="notifications"></a><span data-ttu-id="5a3e9-162">Meddelanden</span><span class="sxs-lookup"><span data-stu-id="5a3e9-162">Notifications</span></span>
<span data-ttu-id="5a3e9-163">Det finns två typer av aviseringar: systemet och i app-meddelanden.</span><span class="sxs-lookup"><span data-stu-id="5a3e9-163">There are two types of notifications: system and in-app notifications.</span></span>

<span data-ttu-id="5a3e9-164">Systemmeddelanden hanteras av iOS och kan inte anpassas.</span><span class="sxs-lookup"><span data-stu-id="5a3e9-164">System notifications are handled by iOS, and cannot be customized.</span></span>

<span data-ttu-id="5a3e9-165">I appen meddelanden består av en vy som läggs till dynamiskt toohello aktuella-fönstret.</span><span class="sxs-lookup"><span data-stu-id="5a3e9-165">In-app notifications are made of a view that is dynamically added toohello current application window.</span></span> <span data-ttu-id="5a3e9-166">Detta kallas en meddelandet överlägget.</span><span class="sxs-lookup"><span data-stu-id="5a3e9-166">This is called a notification overlay.</span></span> <span data-ttu-id="5a3e9-167">Meddelande överlägg är bra för en snabb integrering eftersom de inte kräver du toomodify vyn i ditt program.</span><span class="sxs-lookup"><span data-stu-id="5a3e9-167">Notification overlays are great for a fast integration because they does not require you toomodify any view in your application.</span></span>

#### <a name="layout"></a><span data-ttu-id="5a3e9-168">Layout</span><span class="sxs-lookup"><span data-stu-id="5a3e9-168">Layout</span></span>
<span data-ttu-id="5a3e9-169">toomodify hello utseendet på meddelanden i appen, du kan bara ändra hello filen `AENotificationView.xib` tooyour måste, förutsatt att du behåller hello taggvärden och typer av hello befintliga undervyer.</span><span class="sxs-lookup"><span data-stu-id="5a3e9-169">toomodify hello look of your in-app notifications, you can simply modify hello file `AENotificationView.xib` tooyour needs, as long as you keep hello tag values and types of hello existing subviews.</span></span>

<span data-ttu-id="5a3e9-170">Som standard visas i appen meddelanden längst hello hello-skärmen.</span><span class="sxs-lookup"><span data-stu-id="5a3e9-170">By default, in-app notifications are presented at hello bottom of hello screen.</span></span> <span data-ttu-id="5a3e9-171">Om du föredrar toodisplay dem hello överst på skärmen, redigera hello tillhandahålls `AENotificationView.xib` och ändra hello `AutoSizing` egenskapen hello huvudsakliga vyn så att den kan hållas hello överst i dess superview.</span><span class="sxs-lookup"><span data-stu-id="5a3e9-171">If you prefer toodisplay them at hello top of screen, edit hello provided `AENotificationView.xib` and change hello `AutoSizing` property of hello main view so it can be kept at hello top of its superview.</span></span>

#### <a name="categories"></a><span data-ttu-id="5a3e9-172">Kategorier</span><span class="sxs-lookup"><span data-stu-id="5a3e9-172">Categories</span></span>
<span data-ttu-id="5a3e9-173">När du ändrar hello tillhandahålls layout, kan du ändra hello utseende på alla meddelanden.</span><span class="sxs-lookup"><span data-stu-id="5a3e9-173">When you modify hello provided layout, you modify hello look of all your notifications.</span></span> <span data-ttu-id="5a3e9-174">Kategorier kan toodefine olika riktas söks meddelanden (möjligen beteenden).</span><span class="sxs-lookup"><span data-stu-id="5a3e9-174">Categories allow you toodefine various targeted looks (possibly behaviors) for notifications.</span></span> <span data-ttu-id="5a3e9-175">En kategori kan anges när du skapar en Reach-kampanj.</span><span class="sxs-lookup"><span data-stu-id="5a3e9-175">A category can be specified when you create a Reach campaign.</span></span> <span data-ttu-id="5a3e9-176">Tänk på att kategorier kan du anpassa meddelanden och avsökningar, som beskrivs senare i det här dokumentet.</span><span class="sxs-lookup"><span data-stu-id="5a3e9-176">Keep in mind that categories also let you customize announcements and polls, that is described later in this document.</span></span>

<span data-ttu-id="5a3e9-177">tooregister en kategori hanterare för dina meddelanden behöver du tooadd ett anrop när hello nå modulen har initierats.</span><span class="sxs-lookup"><span data-stu-id="5a3e9-177">tooregister a category handler for your notifications, you need tooadd a call once hello reach module is initialized.</span></span>

    AEReachModule* reach = [AEReachModule moduleWithNotificationIcon:[UIImage imageNamed:@"icon.png"]];
    [reach registerNotifier:myNotifier forCategory:@"my_category"];
    ...

<span data-ttu-id="5a3e9-178">`myNotifier`måste vara en instans av ett objekt som uppfyller toohello protokollet `AENotifier`.</span><span class="sxs-lookup"><span data-stu-id="5a3e9-178">`myNotifier` must be an instance of an object that conforms toohello protocol `AENotifier`.</span></span>

<span data-ttu-id="5a3e9-179">Du kan implementera hello protokollet metoder själv eller välja tooreimplement hello befintlig klass `AEDefaultNotifier` som redan utför de flesta av hello arbete.</span><span class="sxs-lookup"><span data-stu-id="5a3e9-179">You can implement hello protocol methods by yourself or you can choose tooreimplement hello existing class `AEDefaultNotifier` which already performs most of hello work.</span></span>

<span data-ttu-id="5a3e9-180">Om du vill tooredefine hello Aviseringsvy för en viss kategori, kan du följa det här exemplet:</span><span class="sxs-lookup"><span data-stu-id="5a3e9-180">For example, if you want tooredefine hello notification view for a specific category, you can follow this example:</span></span>

    #import "AEDefaultNotifier.h"
    #import "AENotificationView.h"
    @interface MyNotifier : AEDefaultNotifier
    @end

    @implementation MyNotifier

    -(NSString*)nibNameForCategory:(NSString*)category
    {
      return "MyNotificationView";
    }

    @end

<span data-ttu-id="5a3e9-181">Det här enkla exemplet på kategorin förutsätter att du har en fil med namnet `MyNotificationView.xib` i programmets-paket.</span><span class="sxs-lookup"><span data-stu-id="5a3e9-181">This simple example of category assume that you have a file named `MyNotificationView.xib` in your main application bundle.</span></span> <span data-ttu-id="5a3e9-182">Om hello-metoden inte är kan toofind en motsvarande `.xib`, visas inte hello-meddelande och Engagement kommer att bli ett meddelande i hello-konsolen.</span><span class="sxs-lookup"><span data-stu-id="5a3e9-182">If hello method is not able toofind a corresponding `.xib`, hello notification will not be displayed and Engagement will output a message in hello console.</span></span>

<span data-ttu-id="5a3e9-183">hello tillhandahålls nib filen bör beakta hello följande regler:</span><span class="sxs-lookup"><span data-stu-id="5a3e9-183">hello provided nib file should respect hello following rules:</span></span>

* <span data-ttu-id="5a3e9-184">Det får endast innehålla en vy.</span><span class="sxs-lookup"><span data-stu-id="5a3e9-184">It should only contain one view.</span></span>
* <span data-ttu-id="5a3e9-185">Undervyer ska vara av samma typer som hello viktiga inuti hello tillhandahålls nib fil med namnet hello`AENotificationView.xib`</span><span class="sxs-lookup"><span data-stu-id="5a3e9-185">Subviews should be of hello same types as hello ones inside hello provided nib file named `AENotificationView.xib`</span></span>
* <span data-ttu-id="5a3e9-186">Undervyer ska ha hello samma taggar som hello viktiga inuti hello tillhandahålls nib fil med namnet`AENotificationView.xib`</span><span class="sxs-lookup"><span data-stu-id="5a3e9-186">Subviews should have hello same tags as hello ones inside hello provided nib file named `AENotificationView.xib`</span></span>

> [!TIP]
> <span data-ttu-id="5a3e9-187">Bara kopiera hello angivna nib-filen som heter `AENotificationView.xib`, och börjar arbeta därifrån.</span><span class="sxs-lookup"><span data-stu-id="5a3e9-187">Just copy hello provided nib file, named `AENotificationView.xib`, and start working from there.</span></span> <span data-ttu-id="5a3e9-188">Men tänk på att, hello visa innanför nib filen hör toohello klassen `AENotificationView`.</span><span class="sxs-lookup"><span data-stu-id="5a3e9-188">But be careful, hello view inside this nib file is associated toohello class `AENotificationView`.</span></span> <span data-ttu-id="5a3e9-189">Den här klassen omdefinieras hello metoden `layoutSubViews` toomove och ändra storlek på dess undervyer enligt toocontext.</span><span class="sxs-lookup"><span data-stu-id="5a3e9-189">This class redefined hello method `layoutSubViews` toomove and resize its subviews according toocontext.</span></span> <span data-ttu-id="5a3e9-190">Du kanske vill tooreplace den med en `UIView` eller en anpassad vy klass.</span><span class="sxs-lookup"><span data-stu-id="5a3e9-190">You may want tooreplace it with an `UIView` or you custom view class.</span></span>
>
>

<span data-ttu-id="5a3e9-191">Om du behöver djupare anpassning av meddelanden (om du vill att till exempel tooload vyn direkt från hello kod), bör tootake en titt på hello tillhandahålls källa kod och klassen i dokumentationen `Protocol ReferencesDefaultNotifier` och `AENotifier`.</span><span class="sxs-lookup"><span data-stu-id="5a3e9-191">If you need deeper customization of your notifications(if you want for instance tooload your view directly from hello code), it is recommended tootake a look at hello provided source code and class documentation of `Protocol ReferencesDefaultNotifier` and `AENotifier`.</span></span>

<span data-ttu-id="5a3e9-192">Observera att du kan använda hello samma meddelaren för flera kategorier.</span><span class="sxs-lookup"><span data-stu-id="5a3e9-192">Note that you can use hello same notifier for multiple categories.</span></span>

<span data-ttu-id="5a3e9-193">Du kan också omdefinierade hello standard meddelaren så här:</span><span class="sxs-lookup"><span data-stu-id="5a3e9-193">You can also redefined hello default notifier like this:</span></span>

    AEReachModule* reach = [AEReachModule moduleWithNotificationIcon:[UIImage imageNamed:@"icon.png"]];
    [reach registerNotifier:myNotifier forCategory:kAEReachDefaultCategory];

##### <a name="notification-handling"></a><span data-ttu-id="5a3e9-194">Meddelande-hantering</span><span class="sxs-lookup"><span data-stu-id="5a3e9-194">Notification handling</span></span>
<span data-ttu-id="5a3e9-195">När du använder hello standardkategori, vissa livscykel metoder anropas på hello `AEReachContent` objekt tooreport statistik och uppdatera hello kampanj tillstånd:</span><span class="sxs-lookup"><span data-stu-id="5a3e9-195">When using hello default category, some life cycle methods are called on hello `AEReachContent` object tooreport statistics and update hello campaign state:</span></span>

* <span data-ttu-id="5a3e9-196">När hello-meddelande visas i programmet hello `displayNotification` metoden anropas (som rapporterar statistik) av `AEReachModule` om `handleNotification:` returnerar `YES`.</span><span class="sxs-lookup"><span data-stu-id="5a3e9-196">When hello notification is displayed in application, hello `displayNotification` method is called (which reports statistics) by `AEReachModule` if `handleNotification:` returns `YES`.</span></span>
* <span data-ttu-id="5a3e9-197">Om hello-meddelande avvisas hello `exitNotification` metoden anropas statistik rapporteras och nästa kampanjer kan nu bearbetas.</span><span class="sxs-lookup"><span data-stu-id="5a3e9-197">If hello notification is dismissed, hello `exitNotification` method is called, statistic is reported and next campaigns can now be processed.</span></span>
* <span data-ttu-id="5a3e9-198">Om du hello-meddelande `actionNotification` är anropas statistik rapporteras och hello associerad åtgärd utförs.</span><span class="sxs-lookup"><span data-stu-id="5a3e9-198">If hello notification is clicked, `actionNotification` is called, statistic is reported and hello associated action is performed.</span></span>

<span data-ttu-id="5a3e9-199">Om din implementering av `AENotifier` kringgår hello standardbeteendet har du toocall metoderna livscykel själv.</span><span class="sxs-lookup"><span data-stu-id="5a3e9-199">If your implementation of `AENotifier` bypasses hello default behavior, you have toocall these life cycle methods by yourself.</span></span> <span data-ttu-id="5a3e9-200">hello som följande exempel visar tillfällen där hello standardbeteendet kringgås:</span><span class="sxs-lookup"><span data-stu-id="5a3e9-200">hello following examples illustrate some cases where hello default behavior is bypassed:</span></span>

* <span data-ttu-id="5a3e9-201">Du inte utökar `AEDefaultNotifier`, t.ex. du har implementerat kategori hantering från grunden.</span><span class="sxs-lookup"><span data-stu-id="5a3e9-201">You don't extend `AEDefaultNotifier`, e.g. you implemented category handling from scratch.</span></span>
* <span data-ttu-id="5a3e9-202">Du overrode `prepareNotificationView:forContent:`, vara säker på att toomap minst `onNotificationActioned` eller `onNotificationExited` tooone U.I-kontroller.</span><span class="sxs-lookup"><span data-stu-id="5a3e9-202">You overrode `prepareNotificationView:forContent:`, be sure toomap at least `onNotificationActioned` or `onNotificationExited` tooone of your U.I controls.</span></span>

> [!WARNING]
> <span data-ttu-id="5a3e9-203">Om `handleNotification:` returnerar ett undantag, hello innehåll tas bort och `drop` är anropas detta rapporteras i statistik och nästa kampanjer kan nu bearbetas.</span><span class="sxs-lookup"><span data-stu-id="5a3e9-203">If `handleNotification:` throws an exception, hello content is deleted and `drop` is called, this is reported in statistics and next campaigns can now be processed.</span></span>
>
>

#### <a name="include-notification-as-part-of-an-existing-view"></a><span data-ttu-id="5a3e9-204">Inkludera meddelande som en del av en befintlig vy</span><span class="sxs-lookup"><span data-stu-id="5a3e9-204">Include notification as part of an existing view</span></span>
<span data-ttu-id="5a3e9-205">Överlägg är bra för en snabb integrering men kan ibland inte vara praktiskt eller kan orsaka oönskade sidoeffekter.</span><span class="sxs-lookup"><span data-stu-id="5a3e9-205">Overlays are great for a fast integration but can be sometimes not convenient, or can have unwanted side effects.</span></span>

<span data-ttu-id="5a3e9-206">Om du inte är nöjd med hello överlägget systemet i några av dina vyer kan anpassa du den för dessa vyer.</span><span class="sxs-lookup"><span data-stu-id="5a3e9-206">If you're not satisfied with hello overlay system in some of your views, you can customize it for these views.</span></span>

<span data-ttu-id="5a3e9-207">Du kan bestämma tooinclude layout för våra notification i befintliga vyer.</span><span class="sxs-lookup"><span data-stu-id="5a3e9-207">You can decide tooinclude our notification layout in your existing views.</span></span> <span data-ttu-id="5a3e9-208">toodo, så det finns två implementering format:</span><span class="sxs-lookup"><span data-stu-id="5a3e9-208">toodo so, there is two implementation styles:</span></span>

1. <span data-ttu-id="5a3e9-209">Lägg till hello notisvy med hjälp av gränssnittet builder</span><span class="sxs-lookup"><span data-stu-id="5a3e9-209">Add hello notification view using interface builder</span></span>

   * <span data-ttu-id="5a3e9-210">Öppna *gränssnitt Builder*</span><span class="sxs-lookup"><span data-stu-id="5a3e9-210">Open *Interface Builder*</span></span>
   * <span data-ttu-id="5a3e9-211">Placera 320 x 60 (eller 768 x 60 om du är på iPad) `UIView` där du vill att tooappear för hello-meddelande</span><span class="sxs-lookup"><span data-stu-id="5a3e9-211">Place a 320x60 (or 768x60 if you are on iPad) `UIView` where you want hello notification tooappear</span></span>
   * <span data-ttu-id="5a3e9-212">Ange hello taggvärde för den här vyn för: **36822491**</span><span class="sxs-lookup"><span data-stu-id="5a3e9-212">Set hello Tag value for this view too: **36822491**</span></span>
2. <span data-ttu-id="5a3e9-213">Lägg till hello notisvy programmässigt.</span><span class="sxs-lookup"><span data-stu-id="5a3e9-213">Add hello notification view programmatically.</span></span> <span data-ttu-id="5a3e9-214">Lägg bara till hello efter koden när vyn har initierats:</span><span class="sxs-lookup"><span data-stu-id="5a3e9-214">Just add hello following code when your view has been initialized:</span></span>

       UIView* notificationView = [[UIView alloc] initWithFrame:CGRectMake(0, 0, 320, 60)]; //Replace x and y coordinate values tooyour needs.
       notificationView.tag = NOTIFICATION_AREA_VIEW_TAG;
       [self.view addSubview:notificationView];

<span data-ttu-id="5a3e9-215">`NOTIFICATION_AREA_VIEW_TAG`makrot finns i `AEDefaultNotifier.h`.</span><span class="sxs-lookup"><span data-stu-id="5a3e9-215">`NOTIFICATION_AREA_VIEW_TAG` macro can be found in `AEDefaultNotifier.h`.</span></span>

> [!NOTE]
> <span data-ttu-id="5a3e9-216">hello standard meddelaren identifierar automatiskt hello notification layouten ingår i den här vyn och kommer inte att lägga till ett överlägg för den.</span><span class="sxs-lookup"><span data-stu-id="5a3e9-216">hello default notifier automatically detects that hello notification layout is included in this view and will not add an overlay for it.</span></span>
>
>

### <a name="announcements-and-polls"></a><span data-ttu-id="5a3e9-217">Meddelanden och avsökningar</span><span class="sxs-lookup"><span data-stu-id="5a3e9-217">Announcements and polls</span></span>
#### <a name="layouts"></a><span data-ttu-id="5a3e9-218">Layouter</span><span class="sxs-lookup"><span data-stu-id="5a3e9-218">Layouts</span></span>
<span data-ttu-id="5a3e9-219">Du kan ändra hello filer `AEDefaultAnnouncementView.xib` och `AEDefaultPollView.xib` så länge som du behåller hello taggvärden och typer av hello befintliga undervyer.</span><span class="sxs-lookup"><span data-stu-id="5a3e9-219">You can modify hello files `AEDefaultAnnouncementView.xib` and `AEDefaultPollView.xib` as long as you keep hello tag values and types of hello existing subviews.</span></span>

#### <a name="categories"></a><span data-ttu-id="5a3e9-220">Kategorier</span><span class="sxs-lookup"><span data-stu-id="5a3e9-220">Categories</span></span>
##### <a name="alternate-layouts"></a><span data-ttu-id="5a3e9-221">Alternativa layouter</span><span class="sxs-lookup"><span data-stu-id="5a3e9-221">Alternate layouts</span></span>
<span data-ttu-id="5a3e9-222">Hello kampanj kategori kan vara används toohave alternativa layouter för meddelanden och avsökningar som meddelanden.</span><span class="sxs-lookup"><span data-stu-id="5a3e9-222">Like notifications, hello campaign's category can be used toohave alternate layouts for your announcements and polls.</span></span>

<span data-ttu-id="5a3e9-223">toocreate en kategori för ett meddelande måste du utöka **AEAnnouncementViewController** och registrera den när hello reach-modulen har initierats:</span><span class="sxs-lookup"><span data-stu-id="5a3e9-223">toocreate a category for an announcement, you must extend **AEAnnouncementViewController** and register it once hello reach module has been initialized:</span></span>

    AEReachModule* reach = [AEReachModule moduleWithNotificationIcon:[UIImage imageNamed:@"icon.png"]];
    [reach registerAnnouncementController:[MyCustomAnnouncementViewController class] forCategory:@"my_category"];

> [!NOTE]
> <span data-ttu-id="5a3e9-224">Varje gång en användare kommer klickar du på ett meddelande för ett meddelande med hello kategori ”min\_kategori”, registrerade view-controller (i så fall `MyCustomAnnouncementViewController`) initieras genom att anropa metoden hello `initWithAnnouncement:` och hello vyn kommer att vara tillagda toohello aktuella-fönstret.</span><span class="sxs-lookup"><span data-stu-id="5a3e9-224">Each time a user will click on a notification for an announcement with hello category "my\_category", your registered view controller (in that case `MyCustomAnnouncementViewController`) will be initialized by calling hello method `initWithAnnouncement:` and hello view will be added toohello current application window.</span></span>
>
>

<span data-ttu-id="5a3e9-225">I implementeringen av hello `AEAnnouncementViewController` klass har tooread hello egenskapen `announcement` tooinitialize din undervyer.</span><span class="sxs-lookup"><span data-stu-id="5a3e9-225">In your implementation of hello `AEAnnouncementViewController` class you will have tooread hello property `announcement` tooinitialize your subviews.</span></span> <span data-ttu-id="5a3e9-226">Överväg att hello exemplet nedan, där två etiketter initierats med `title` och `body` egenskaper för hello `AEReachAnnouncement` klass:</span><span class="sxs-lookup"><span data-stu-id="5a3e9-226">Consider hello example below, where two labels are initialized using `title` and `body` properties of hello `AEReachAnnouncement` class:</span></span>

    -(void)loadView
    {
        [super loadView];

        UILabel* titleLabel = [[UILabel alloc] initWithFrame:CGRectMake(10, 20, 300, 60)];
        titleLabel.font = [UIFont systemFontOfSize:32.0];
        titleLabel.text = self.announcement.title;

        UILabel* bodyLabel = [[UILabel alloc] initWithFrame:CGRectMake(10, 20, 300, 60)];
        bodyLabel.font = [UIFont systemFontOfSize:24.0];
        bodyLabel.text = self.announcement.body;

        [self.view addSubview:titleLabel];
        [self.view addSubview:bodyLabel];
    }

<span data-ttu-id="5a3e9-227">Om du inte vill tooload dina vyer själv, men du bara vill tooreuse hello standard meddelande Visa layout, kan du bara se styrenheten för anpassad vy utökar hello tillhandahålls klassen `AEDefaultAnnouncementViewController`.</span><span class="sxs-lookup"><span data-stu-id="5a3e9-227">If you don't want tooload your views by yourself but you just want tooreuse hello default announcement view layout, you can simply make your custom view controller extends hello provided class `AEDefaultAnnouncementViewController`.</span></span> <span data-ttu-id="5a3e9-228">I så fall duplicera hello nib filen `AEDefaultAnnouncementView.xib` och Byt namn på den så den kan läsas av anpassade view-controller (för en domänkontrollant med namnet `CustomAnnouncementViewController`, ska du anropa nib filen `CustomAnnouncementView.xib`).</span><span class="sxs-lookup"><span data-stu-id="5a3e9-228">In that case, duplicate hello nib file `AEDefaultAnnouncementView.xib` and rename it so it can be loaded by your custom view controller (for a controller named `CustomAnnouncementViewController`, you should call your nib file `CustomAnnouncementView.xib`).</span></span>

<span data-ttu-id="5a3e9-229">tooreplace hello Standardkategori för meddelanden, helt enkelt registrera styrenheten anpassad vy för hello kategori som definierats i `kAEReachDefaultCategory`:</span><span class="sxs-lookup"><span data-stu-id="5a3e9-229">tooreplace hello default category of announcements, simply register your custom view controller for hello category defined in `kAEReachDefaultCategory`:</span></span>

    [reach registerAnnouncementController:[MyCustomAnnouncementViewController class] forCategory:kAEReachDefaultCategory];

<span data-ttu-id="5a3e9-230">Omröstningar kan vara anpassade hello samma sätt:</span><span class="sxs-lookup"><span data-stu-id="5a3e9-230">Polls can be customized hello same way :</span></span>

    AEReachModule* reach = [AEReachModule moduleWithNotificationIcon:[UIImage imageNamed:@"icon.png"]];
    [reach registerPollController:[MyCustomPollViewController class] forCategory:@"my_category"];

<span data-ttu-id="5a3e9-231">Den här gången kan hello tillhandahålls `MyCustomPollViewController` måste utöka `AEPollViewController`.</span><span class="sxs-lookup"><span data-stu-id="5a3e9-231">This time, hello provided `MyCustomPollViewController` must extend `AEPollViewController`.</span></span> <span data-ttu-id="5a3e9-232">Du kan också välja tooextend från hello standard domänkontrollant: `AEDefaultPollViewController`.</span><span class="sxs-lookup"><span data-stu-id="5a3e9-232">Or you can choose tooextend from hello default controller: `AEDefaultPollViewController`.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="5a3e9-233">Glöm inte toocall antingen `action` (`submitAnswers:` för anpassade avsökningsdatum visa domänkontrollanter) eller `exit` metoden innan hello view controller avvisas.</span><span class="sxs-lookup"><span data-stu-id="5a3e9-233">Don't forget toocall either `action` (`submitAnswers:` for custom poll view controllers) or `exit` method before hello view controller is dismissed.</span></span> <span data-ttu-id="5a3e9-234">Annars statistik skickas inte (dvs. inga analyser på hello kampanj) och mer allt nästa kampanjer meddelas inte förrän hello programprocessen har startats om.</span><span class="sxs-lookup"><span data-stu-id="5a3e9-234">Otherwise, statistics won't be sent (i.e. no analytics on hello campaign) and more importantly next campaigns will not be notified until hello application process is restarted.</span></span>
>
>

##### <a name="implementation-example"></a><span data-ttu-id="5a3e9-235">Exempel på implementering</span><span class="sxs-lookup"><span data-stu-id="5a3e9-235">Implementation example</span></span>
<span data-ttu-id="5a3e9-236">I den här implementeringen hello anpassade meddelande vyn har lästs in från en extern xib-fil.</span><span class="sxs-lookup"><span data-stu-id="5a3e9-236">In this implementation hello custom announcement view is loaded from an external xib file.</span></span>

<span data-ttu-id="5a3e9-237">För avancerade notification anpassning bör som toolook på hello källkoden för hello standardimplementeringen.</span><span class="sxs-lookup"><span data-stu-id="5a3e9-237">Like for advanced notification customization, it is recommended toolook at hello source code of hello standard implementation.</span></span>

`CustomAnnouncementViewController.h`

    //Interface
    @interface CustomAnnouncementViewController : AEAnnouncementViewController {
      UILabel* titleLabel;
      UITextView* descTextView;
      UIWebView* htmlWebView;
      UIButton* okButton;
      UIButton* cancelButton;
    }

    @property (nonatomic, retain) IBOutlet UILabel* titleLabel;
    @property (nonatomic, retain) IBOutlet UITextView* descTextView;
    @property (nonatomic, retain) IBOutlet UIWebView* htmlWebView;
    @property (nonatomic, retain) IBOutlet UIButton* okButton;
    @property (nonatomic, retain) IBOutlet UIButton* cancelButton;

    -(IBAction)okButtonClicked:(id)sender;
    -(IBAction)cancelButtonClicked:(id)sender;

`CustomAnnouncementViewController.m`

    //Implementation
    @implementation CustomAnnouncementViewController
    @synthesize titleLabel;
    @synthesize descTextView;
    @synthesize htmlWebView;
    @synthesize okButton;
    @synthesize cancelButton;

    -(id)initWithAnnouncement:(AEReachAnnouncement*)anAnnouncement
    {
      self = [super initWithNibName:@"CustomAnnouncementViewController" bundle:nil];
      if (self != nil) {
        self.announcement = anAnnouncement;
      }
      return self;
    }

    - (void) dealloc
    {
      [titleLabel release];
      [descTextView release];
      [htmlWebView release];
      [okButton release];
      [cancelButton release];
      [super dealloc];
    }

    - (void)viewDidLoad {
      [super viewDidLoad];

      /* Init announcement title */
      titleLabel.text = self.announcement.title;

      /* Init announcement body */
      if(self.announcement.type == AEAnnouncementTypeHtml)
      {
        titleLabel.hidden = YES;
        htmlWebView.hidden = NO;
        [htmlWebView loadHTMLString:self.announcement.body baseURL:[NSURL URLWithString:@"http://localhost/"]];
      }
      else
      {
        titleLabel.hidden = NO;
        htmlWebView.hidden = YES;
        descTextView.text = self.announcement.body;
      }

      /* Set action button label */
      if([self.announcement.actionLabel length] > 0)
        [okButton setTitle:self.announcement.actionLabel forState:UIControlStateNormal];

      /* Set exit button label */
      if([self.announcement.exitLabel length] > 0)
        [cancelButton setTitle:self.announcement.exitLabel forState:UIControlStateNormal];
    }

    #pragma mark Actions

    -(IBAction)okButtonClicked:(id)sender
    {
        [self action];
    }

    -(IBAction)cancelButtonClicked:(id)sender
    {
        [self exit];
    }

    @end
